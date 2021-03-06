---
title: Gebruik een statisch IP-adres met de Azure Kubernetes-service (AKS) load balancer
description: Meer informatie over het maken en gebruiken van een statisch IP-adres met de Azure Kubernetes service (AKS) load balancer.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 11/06/2019
ms.author: mlearned
ms.openlocfilehash: 8457f1c0c5b6107c4b44f6f00236a33f7c67452a
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74325447"
---
# <a name="use-a-static-public-ip-address-with-the-azure-kubernetes-service-aks-load-balancer"></a>Gebruik een statisch openbaar IP-adres met de Azure Kubernetes-service (AKS) load balancer

Het open bare IP-adres dat is toegewezen aan een load balancer bron die is gemaakt met een AKS-cluster, is standaard alleen geldig voor de levens duur van die resource. Als u de Kubernetes-service verwijdert, worden ook de gekoppelde load balancer en het IP-adres verwijderd. Als u een specifiek IP-adres wilt toewijzen of een IP-adres voor opnieuw geïmplementeerde Kubernetes-Services wilt behouden, kunt u een statisch openbaar IP-adres maken en gebruiken.

In dit artikel wordt beschreven hoe u een statisch openbaar IP-adres maakt en dit toewijst aan uw Kubernetes-service.

## <a name="before-you-begin"></a>Voordat u begint

In dit artikel wordt ervan uitgegaan dat u beschikt over een bestaand AKS-cluster. Als u een AKS-cluster nodig hebt, raadpleegt u de AKS Quick Start [met behulp van de Azure cli][aks-quickstart-cli] of [met behulp van de Azure Portal][aks-quickstart-portal].

Ook moet de Azure CLI-versie 2.0.59 of hoger zijn geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, raadpleegt u [Azure cli installeren][install-azure-cli].

In dit artikel wordt gebruikgemaakt van een *standaard* SKU-IP-adres met een *standaard* SKU-Load Balancer. Zie [IP-adres typen en toewijzings methoden in azure][ip-sku]voor meer informatie.

## <a name="create-a-static-ip-address"></a>Een statisch IP-adres maken

Maak een statisch openbaar IP-adres met de opdracht [AZ Network open bare IP Create][az-network-public-ip-create] . Hieronder maakt u een statische IP-bron met de naam *myAKSPublicIP* in de resource groep *myResourceGroup* :

```azurecli-interactive
az network public-ip create \
    --resource-group myResourceGroup \
    --name myAKSPublicIP \
    --sku Standard \
    --allocation-method static
```

> [!NOTE]
> Als u een *basis* -SKU gebruikt Load Balancer in uw AKS-cluster, gebruikt u *Basic* voor de *SKU* -para meter bij het definiëren van een openbaar IP-adres. Alleen *Basic* SKU-IP-adressen werken met de *basis* -SKU Load Balancer en alleen *standaard* -SKU-ip's werken met *Standard* SKU load balancers. 

Het IP-adres wordt weer gegeven, zoals wordt weer gegeven in de volgende verkorte voorbeeld uitvoer:

```json
{
  "publicIp": {
    ...
    "ipAddress": "40.121.183.52",
    ...
  }
}
```

U kunt later het open bare IP-adres ophalen met de opdracht [AZ Network Public-IP List][az-network-public-ip-list] . Geef de naam op van de knooppunt resource groep en het open bare IP-adres dat u hebt gemaakt, en voer een query uit voor het *IP* -adres, zoals weer gegeven in het volgende voor beeld:

```azurecli-interactive
$ az network public-ip show --resource-group myResourceGroup --name myAKSPublicIP --query ipAddress --output tsv

40.121.183.52
```

## <a name="create-a-service-using-the-static-ip-address"></a>Een service maken met behulp van het statische IP-adres

Voordat u een service maakt, moet u ervoor zorgen dat de service-principal die wordt gebruikt door het AKS-cluster gedelegeerde machtigingen heeft voor de andere resource groep. Bijvoorbeeld:

```azurecli-interactive
az role assignment create \
    --assignee <SP Client ID> \
    --role "Contributor" \
    --scope /subscriptions/<subscription id>/resourceGroups/<resource group name>
```

Als u een *Load Balancer* -service met het statische open bare IP-adres wilt maken, voegt u de eigenschap `loadBalancerIP` en de waarde van het statische open bare IP-adres toe aan het yaml-manifest. Maak een bestand met de naam `load-balancer-service.yaml` en kopieer de volgende YAML. Geef uw eigen open bare IP-adres op dat u in de vorige stap hebt gemaakt. In het volgende voor beeld wordt de aantekening ook ingesteld op de resource groep met de naam *myResourceGroup*. Geef de naam van uw eigen resource groep op.

```yaml
apiVersion: v1
kind: Service
metadata:
  annotations:
    service.beta.kubernetes.io/azure-load-balancer-resource-group: myResourceGroup
  name: azure-load-balancer
spec:
  loadBalancerIP: 40.121.183.52
  type: LoadBalancer
  ports:
  - port: 80
  selector:
    app: azure-load-balancer
```

Maak de service en de implementatie met behulp van de `kubectl apply` opdracht.

```console
kubectl apply -f load-balancer-service.yaml
```

## <a name="troubleshoot"></a>Problemen oplossen

Als het statische IP-adres dat is gedefinieerd in de eigenschap *loadBalancerIP* van het Kubernetes-service manifest niet bestaat of niet is gemaakt in de resource groep node en er geen aanvullende delegaties zijn geconfigureerd, mislukt het maken van de Load Balancer-service. Als u problemen wilt oplossen, controleert u de gebeurtenissen voor het maken van een service met de opdracht [kubectl beschrijven][kubectl-describe] . Geef de naam van de service op zoals opgegeven in het YAML-manifest, zoals wordt weer gegeven in het volgende voor beeld:

```console
kubectl describe service azure-load-balancer
```

Informatie over de Kubernetes-service resource wordt weer gegeven. De *gebeurtenissen* aan het einde van de volgende voorbeeld uitvoer geven aan dat het door de *gebruiker opgegeven IP-adres niet is gevonden*. Controleer in deze scenario's of u het statische open bare IP-adres in de knooppunt resource groep hebt gemaakt en of het IP-adres dat is opgegeven in het Kubernetes-service manifest juist is.

```
Name:                     azure-load-balancer
Namespace:                default
Labels:                   <none>
Annotations:              <none>
Selector:                 app=azure-load-balancer
Type:                     LoadBalancer
IP:                       10.0.18.125
IP:                       40.121.183.52
Port:                     <unset>  80/TCP
TargetPort:               80/TCP
NodePort:                 <unset>  32582/TCP
Endpoints:                <none>
Session Affinity:         None
External Traffic Policy:  Cluster
Events:
  Type     Reason                      Age               From                Message
  ----     ------                      ----              ----                -------
  Normal   CreatingLoadBalancer        7s (x2 over 22s)  service-controller  Creating load balancer
  Warning  CreatingLoadBalancerFailed  6s (x2 over 12s)  service-controller  Error creating load balancer (will retry): Failed to create load balancer for service default/azure-load-balancer: user supplied IP Address 40.121.183.52 was not found
```

## <a name="next-steps"></a>Volgende stappen

Als u meer controle over het netwerk verkeer voor uw toepassingen wilt, kunt u in plaats daarvan [een ingangs controller maken][aks-ingress-basic]. U kunt ook [een ingangs controller met een statisch openbaar IP-adres maken][aks-static-ingress].

<!-- LINKS - External -->
[kubectl-describe]: https://kubernetes.io/docs/reference/generated/kubectl/kubectl-commands#describe

<!-- LINKS - Internal -->
[aks-faq-resource-group]: faq.md#why-are-two-resource-groups-created-with-aks
[az-network-public-ip-create]: /cli/azure/network/public-ip#az-network-public-ip-create
[az-network-public-ip-list]: /cli/azure/network/public-ip#az-network-public-ip-list
[az-aks-show]: /cli/azure/aks#az-aks-show
[aks-ingress-basic]: ingress-basic.md
[aks-static-ingress]: ingress-static-ip.md
[aks-quickstart-cli]: kubernetes-walkthrough.md
[aks-quickstart-portal]: kubernetes-walkthrough-portal.md
[install-azure-cli]: /cli/azure/install-azure-cli
[ip-sku]: ../virtual-network/virtual-network-ip-addresses-overview-arm.md#sku
