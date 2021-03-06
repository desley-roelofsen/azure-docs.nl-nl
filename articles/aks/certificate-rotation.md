---
title: Certificaten in azure Kubernetes service (AKS) draaien
description: Meer informatie over het draaien van uw certificaten in een Azure Kubernetes service (AKS)-cluster.
services: container-service
author: zr-msft
ms.service: container-service
ms.topic: article
ms.date: 11/15/2019
ms.author: zarhoads
ms.openlocfilehash: 00d8546cb20d12c5f1a94bdcababa04a77c73133
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2019
ms.locfileid: "74134409"
---
# <a name="rotate-certificates-in-azure-kubernetes-service-aks"></a>Certificaten in azure Kubernetes service (AKS) draaien

Azure Kubernetes service (AKS) gebruikt certificaten voor verificatie met veel onderdelen. Het kan zijn dat u deze certificaten regel matig moet draaien om beveiligings-of beleids redenen. U kunt bijvoorbeeld een beleid hebben om elke 90 dagen alle certificaten te draaien.

In dit artikel wordt beschreven hoe u de certificaten in uw AKS-cluster roteert.

## <a name="before-you-begin"></a>Voordat u begint

Voor dit artikel moet u de Azure CLI-versie 2.0.76 of hoger uitvoeren. Voer `az --version` uit om de versie te bekijken. Zie [Azure CLI installeren][azure-cli-install] als u de CLI wilt installeren of een upgrade wilt uitvoeren.


### <a name="install-aks-preview-cli-extension"></a>AKS-preview CLI-extensie installeren

Als u deze functie wilt gebruiken, hebt u de *AKS-preview cli-* extensie versie 0.4.21 of hoger nodig. Installeer de Azure CLI *-extensie AKS-preview* met behulp van de opdracht [AZ extension add][az-extension-add] en controleer vervolgens of er beschik bare updates zijn met behulp van de opdracht [AZ extension update][az-extension-update] :

```azurecli-interactive
# Install the aks-preview extension
az extension add --name aks-preview

# Update the extension to make sure you have the latest version installed
az extension update --name aks-preview
```

## <a name="aks-certificates-certificate-authorities-and-service-accounts"></a>AKS certificaten, certificerings instanties en service accounts

AKS genereert en gebruikt de volgende certificaten, certificerings instanties en service accounts:

* De AKS API-server maakt een certificerings instantie (CA) die de cluster-CA wordt genoemd.
* De API-server heeft een cluster-CA, die certificaten ondertekent voor eenrichtings communicatie van de API-server naar kubelets.
* Elke kubelet maakt ook een aanvraag voor certificaat ondertekening (CSR), die door de cluster-CA is ondertekend, voor communicatie van de kubelet naar de API-server.
* De etcd-sleutel waarde Store heeft een certificaat dat is ondertekend door de cluster-CA voor communicatie van etcd naar de API-server.
* De etcd-sleutel waarde Store maakt een certificerings instantie die certificaten ondertekent voor het verifiëren en autoriseren van gegevens replicatie tussen etcd-replica's in het AKS-cluster.
* De API aggregator gebruikt de cluster-CA om certificaten uit te geven voor communicatie met andere Api's, zoals Open Service Broker voor Azure. De API aggregator kan ook een eigen certificerings instantie hebben voor het uitgeven van die certificaten, maar maakt momenteel gebruik van de cluster-CA.
* Elk knoop punt maakt gebruik van een token voor service accounts (SA), dat is ondertekend door de cluster-CA.
* De `kubectl`-client heeft een certificaat voor communicatie met het AKS-cluster.

> [!NOTE]
> AKS-clusters die vóór maart 2019 zijn gemaakt, hebben certificaten die na twee jaar verlopen. Elk cluster dat is gemaakt na 2019 maart of een cluster met de geroteerde certificaten heeft certificaten die na 30 jaar verlopen.

## <a name="rotate-your-cluster-certificates"></a>Uw cluster certificaten draaien

> [!WARNING]
> Het draaien van uw certificaten met behulp van `az aks rotate-certs` kan tot wel 30 minuten uitval tijd voor uw AKS-cluster veroorzaken.

Gebruik [AZ AKS Get-referenties][az-aks-get-credentials] om u aan te melden bij uw AKS-cluster. Met deze opdracht wordt ook het `kubectl`-client certificaat op uw lokale computer gedownload en geconfigureerd.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

Gebruik `az aks rotate-certs` om alle certificaten, Ca's en Sa's in uw cluster te draaien.

```console
az aks rotate-certs -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME
```

> [!IMPORTANT]
> Het kan tot 30 minuten duren voordat `az aks rotate-certs` is voltooid. Als de opdracht mislukt voordat deze wordt voltooid, gebruikt u `az aks show` om te controleren of de status van het cluster is *gedraaid*. Als het cluster zich in een mislukte status bevindt, voert u `az aks rotate-certs` opnieuw uit om uw certificaten opnieuw te draaien.

Controleer of de oude certificaten niet meer geldig zijn door een `kubectl` opdracht uit te voeren. Omdat u de certificaten die worden gebruikt door `kubectl`niet hebt bijgewerkt, wordt er een fout weer geven.  Bijvoorbeeld:

```console
$ kubectl get no
Unable to connect to the server: x509: certificate signed by unknown authority (possibly because of "crypto/rsa: verification error" while trying to verify candidate authority certificate "ca")
```

Werk het certificaat dat door `kubectl` wordt gebruikt bij door `az aks get-credentials`uit te voeren.

```console
az aks get-credentials -g $RESOURCE_GROUP_NAME -n $CLUSTER_NAME --overwrite-existing
```

Controleer of de certificaten zijn bijgewerkt door een `kubectl` opdracht uit te voeren, die nu slaagt. Bijvoorbeeld:

```console
kubectl get no
```

## <a name="next-steps"></a>Volgende stappen

In dit artikel wordt uitgelegd hoe u de certificaten, Ca's en Sa's van uw cluster automatisch kunt draaien. U kunt [Best practices voor cluster beveiliging en upgrades in azure Kubernetes service (AKS)][aks-best-practices-security-upgrades] bekijken voor meer informatie over de aanbevolen procedures voor beveiliging van AKS.


[azure-cli-install]: /cli/azure/install-azure-cli
[az-aks-get-credentials]: /cli/azure/aks?view=azure-cli-latest#az-aks-get-credentials
[az-extension-add]: /cli/azure/extension#az-extension-add
[az-extension-update]: /cli/azure/extension#az-extension-update
[aks-best-practices-security-upgrades]: operator-best-practices-cluster-security.md
