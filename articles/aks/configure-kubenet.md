---
title: Kubenet-netwerken configureren in azure Kubernetes service (AKS)
description: Meer informatie over het configureren van kubenet (Basic) in azure Kubernetes service (AKS) voor het implementeren van een AKS-cluster in een bestaand virtueel netwerk en subnet.
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 06/26/2019
ms.author: mlearned
ms.reviewer: nieberts, jomore
ms.openlocfilehash: b233c5dd639bb6652f201727748a081f6a8a4c64
ms.sourcegitcommit: 4f7dce56b6e3e3c901ce91115e0c8b7aab26fb72
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/04/2019
ms.locfileid: "71950338"
---
# <a name="use-kubenet-networking-with-your-own-ip-address-ranges-in-azure-kubernetes-service-aks"></a>Gebruik kubenet-netwerken met uw eigen IP-adresbereiken in azure Kubernetes service (AKS)

AKS-clusters gebruiken standaard [kubenet][kubenet]en er worden voor u een virtueel Azure-netwerk en-subnet gemaakt. Met *kubenet*krijgen knoop punten een IP-adres uit het subnet van het virtuele Azure-netwerk. Elk ontvangen IP-adres van een logische andere adres ruimte naar het subnet van het virtuele netwerk van Azure van de knoop punten. NAT (Network Address Translation) wordt vervolgens geconfigureerd zodat de peul bronnen kan bereiken in het virtuele Azure-netwerk. Het bron-IP-adres van het verkeer is NAT naar het primaire IP-adres van het knoop punt. Met deze methode wordt het aantal IP-adressen dat u in uw netwerk ruimte moet reserveren, aanzienlijk verminderd voor gebruik.

Met [Azure container Networking interface (cni)][cni-networking]haalt elke pod een IP-adres uit het subnet en kan het rechtstreeks worden geopend. Deze IP-adressen moeten uniek zijn binnen uw netwerk ruimte en moeten vooraf worden gepland. Elk knoop punt heeft een configuratie parameter voor het maximum aantal peulen dat wordt ondersteund. Het equivalente aantal IP-adressen per knoop punt wordt vervolgens vóór dat knoop punt gereserveerd. Deze aanpak vereist meer planning en leidt vaak tot een aflopende IP-adres afvoer of de nood zaak om clusters opnieuw te bouwen in een groter subnet naarmate uw toepassings vereisten groeien.

Dit artikel laat u zien hoe u *kubenet* -netwerken kunt gebruiken om een subnet van een virtueel netwerk voor een AKS-cluster te maken en te gebruiken. Zie [netwerk concepten voor Kubernetes en AKS][aks-network-concepts]voor meer informatie over netwerk opties en overwegingen.

> [!WARNING]
> Als u Windows Server-knooppunt groepen wilt gebruiken (momenteel in de preview-versie van AKS), moet u Azure CNI gebruiken. Het gebruik van kubenet als het netwerk model is niet beschikbaar voor Windows Server-containers.

## <a name="before-you-begin"></a>Voordat u begint

U moet de Azure CLI-versie 2.0.65 of hoger hebben geïnstalleerd en geconfigureerd. Voer  `az --version` uit om de versie te bekijken. Als u wilt installeren of upgraden, raadpleegt u [Azure cli installeren][install-azure-cli].

## <a name="overview-of-kubenet-networking-with-your-own-subnet"></a>Overzicht van kubenet-netwerken met uw eigen subnet

In veel omgevingen hebt u virtuele netwerken en subnetten met toegewezen IP-adresbereiken gedefinieerd. Deze virtuele netwerk bronnen worden gebruikt ter ondersteuning van meerdere services en toepassingen. Om netwerk connectiviteit te bieden, kunnen AKS-clusters gebruikmaken van *kubenet* (Basic-netwerken) of Azure cni (*Geavanceerd netwerk*).

Met *kubenet*ontvangen alleen de knoop punten een IP-adres in het subnet van het virtuele netwerk. Het is niet mogelijk om rechtstreeks met elkaar te communiceren. In plaats daarvan wordt door de gebruiker gedefinieerde route ring (UDR) en door sturen via IP gebruikt voor connectiviteit tussen verschillende knoop punten. U kunt ook peul implementeren achter een service die een toegewezen IP-adres ontvangt en verkeer voor de toepassing verdeelt. In het volgende diagram ziet u hoe de AKS-knoop punten een IP-adres in het subnet van het virtuele netwerk ontvangen, maar niet de peul:

![Kubenet-netwerk model met een AKS-cluster](media/use-kubenet/kubenet-overview.png)

Azure ondersteunt Maxi maal 400 routes in een UDR, zodat u geen AKS-cluster hebt dat groter is dan 400 knoop punten. Virtuele AKS- [knoop punten][virtual-nodes] en Azure-netwerk beleid worden niet ondersteund met *kubenet*.  U kunt [Calico-netwerk beleid][calico-network-policies]gebruiken, aangezien deze worden ondersteund met kubenet.

Met *Azure cni*ontvangt elke pod een IP-adres in het IP-subnet en kan het rechtstreeks communiceren met andere peulen en services. Uw clusters kunnen zo groot zijn als het IP-adres bereik dat u opgeeft. Het IP-adres bereik moet echter vooraf worden gepland en alle IP-adressen worden gebruikt door de AKS-knoop punten op basis van het maximum aantal peul dat ze kunnen ondersteunen. Geavanceerde netwerk functies en-scenario's, zoals [virtuele knoop punten][virtual-nodes] of netwerk beleid (Azure of Calico), worden ondersteund met *Azure cni*.

### <a name="ip-address-availability-and-exhaustion"></a>Beschik baarheid en uitputting van IP-adressen

Met *Azure cni*is een veelvoorkomend probleem het toegewezen IP-adres bereik te klein om extra knoop punten toe te voegen wanneer u een cluster schaalt of bijwerkt. Het netwerk team kan ook geen groot genoeg IP-adres bereik geven om de verwachte toepassings vereisten te ondersteunen.

Als inbreuk kunt u een AKS-cluster maken dat gebruikmaakt van *kubenet* en verbinding maken met een bestaand subnet van een virtueel netwerk. Op deze manier kunnen de knoop punten gedefinieerde IP-adressen ontvangen, zonder dat er een groot aantal IP-adressen hoeft te worden gereserveerd voor alle mogelijke peulen die in het cluster kunnen worden uitgevoerd.

Met *kubenet*kunt u een veel kleiner IP-adres bereik gebruiken en kunt u grote clusters en toepassings vereisten ondersteunen. Bijvoorbeeld, zelfs met een */27* IP-adres bereik, kunt u een 20-25-knooppunt cluster uitvoeren met voldoende ruimte om te schalen of bij te werken. Deze cluster grootte zou Maxi maal *2200-2750* peul ondersteunen (met een standaard maximum van 110 peul per knoop punt). Het maximum aantal peulen per knoop punt dat u kunt configureren met *kubenet* in AKS is 110.

In de volgende basis berekeningen wordt het verschil in netwerk modellen vergeleken:

- **kubenet** : een eenvoudig */24* IP-adres bereik kan Maxi maal *251* knoop punten in het cluster ondersteunen (elk subnet van het virtuele netwerk van Azure reserveert de eerste drie IP-adressen voor beheer bewerkingen)
  - Dit aantal knoop punten kan Maxi maal *27.610* peul ondersteunen (met een standaard maximum van 110 peul per knoop punt met *kubenet*)
- **Azure cni** : hetzelfde basis *-/24* -subnet-bereik kan slechts Maxi maal *8* knoop punten in het cluster ondersteunen
  - Dit aantal knoop punten kan Maxi maal *240* peul ondersteunen (met een standaard maximum van 30 peulen per knoop punt met *Azure cni*)

> [!NOTE]
> Bij deze maximum waarden wordt geen rekening gehouden met het bijwerken of schalen. In de praktijk kunt u het maximum aantal knoop punten dat het IP-adres bereik van het subnet ondersteunt niet uitvoeren. U moet de IP-adressen die beschikbaar zijn voor gebruik tijdens de schaal van de upgrade-bewerkingen, laten staan.

### <a name="virtual-network-peering-and-expressroute-connections"></a>Peering en ExpressRoute-verbindingen voor virtuele netwerken

Voor on-premises connectiviteit kunnen netwerk benaderingen van zowel *kubenet* *-als Azure-cni* gebruikmaken van [Azure Virtual Network-peering][vnet-peering] of [ExpressRoute-verbindingen][express-route]. Plan uw IP-adresbereiken zorgvuldig om overlap pingen en onjuiste verkeers routering te voor komen. Veel on-premises netwerken gebruiken bijvoorbeeld een adres bereik van *10.0.0.0/8* dat wordt geadverteerd via de ExpressRoute-verbinding. Het is raadzaam om uw AKS-clusters te maken in subnetten van het virtuele Azure-netwerk buiten dit adres bereik, zoals *172.16.0.0/16*.

### <a name="choose-a-network-model-to-use"></a>Kies een netwerk model dat u wilt gebruiken

De keuze van de netwerk-plugin die voor uw AKS-cluster moet worden gebruikt, is doorgaans een evenwicht tussen flexibiliteit en geavanceerde configuratie behoeften. Hieronder vindt u een overzicht van de overwegingen wanneer elk netwerk model het meest geschikt is.

Gebruik *kubenet* wanneer:

- U hebt beperkte IP-adres ruimte.
- De meeste pod-communicatie bevindt zich in het cluster.
- U hebt geen geavanceerde AKS-functies nodig, zoals virtuele knoop punten of Azure-netwerk beleid.  Gebruik [Calico-netwerk beleidsregels][calico-network-policies].

Gebruik *Azure cni* wanneer:

- U hebt beschik bare IP-adres ruimte.
- De meeste pod-communicatie is tot resources buiten het cluster.
- U wilt de Udr's niet beheren.
- U hebt geavanceerde functies van AKS nodig, zoals virtuele knoop punten of Azure-netwerk beleid.  Gebruik [Calico-netwerk beleidsregels][calico-network-policies].

Zie [netwerk modellen vergelijken en hun ondersteunings bereik][network-comparisons]voor meer informatie over het bepalen van het netwerk model dat moet worden gebruikt.

## <a name="create-a-virtual-network-and-subnet"></a>Een virtueel netwerk en een subnet maken

Om aan de slag te gaan met het gebruik van *kubenet* en uw eigen virtuele netwerk subnet, maakt u eerst een resource groep met de opdracht [AZ Group Create][az-group-create] . In het volgende voorbeeld wordt een resourcegroep met de naam *myResourceGroup* gemaakt op de locatie *eastus*:

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Als u geen bestaand virtueel netwerk en een subnet voor gebruik hebt, maakt u deze netwerk bronnen met behulp van de opdracht [AZ Network vnet Create][az-network-vnet-create] . In het volgende voor beeld heeft het virtuele netwerk de naam *myVnet* met het adres voorvoegsel van *192.168.0.0/16*. Er wordt een subnet gemaakt met de naam *myAKSSubnet* met het adres voorvoegsel *192.168.1.0/24*.

```azurecli-interactive
az network vnet create \
    --resource-group myResourceGroup \
    --name myAKSVnet \
    --address-prefixes 192.168.0.0/16 \
    --subnet-name myAKSSubnet \
    --subnet-prefix 192.168.1.0/24
```

## <a name="create-a-service-principal-and-assign-permissions"></a>Een service-principal maken en machtigingen toewijzen

Een AKS-cluster heeft een service-principal van Azure Active Directory nodig om met andere Azure-resources te kunnen communiceren. De Service-Principal moet machtigingen hebben om het virtuele netwerk en het subnet te beheren dat door de AKS-knoop punten wordt gebruikt. Als u een Service-Principal wilt maken, gebruikt u de opdracht [AZ AD SP create-for-RBAC][az-ad-sp-create-for-rbac] :

```azurecli-interactive
az ad sp create-for-rbac --skip-assignment
```

In de volgende voorbeeld uitvoer ziet u de toepassings-ID en het wacht woord voor uw service-principal. Deze waarden worden gebruikt in extra stappen om een rol toe te wijzen aan de Service-Principal en vervolgens het AKS-cluster te maken:

```console
$ az ad sp create-for-rbac --skip-assignment

{
  "appId": "476b3636-5eda-4c0e-9751-849e70b5cfad",
  "displayName": "azure-cli-2019-01-09-22-29-24",
  "name": "http://azure-cli-2019-01-09-22-29-24",
  "password": "a1024cd7-af7b-469f-8fd7-b293ecbb174e",
  "tenant": "72f998bf-85f1-41cf-92ab-2e7cd014db46"
}
```

Als u de juiste delegaties in de resterende stappen wilt toewijzen, gebruikt u de opdracht [AZ Network vnet show][az-network-vnet-show] en [AZ Network vnet subnet show][az-network-vnet-subnet-show] commands om de vereiste resource-id's op te halen. Deze resource-Id's worden als variabelen opgeslagen en verwezen in de overige stappen:

```azurecli-interactive
VNET_ID=$(az network vnet show --resource-group myResourceGroup --name myAKSVnet --query id -o tsv)
SUBNET_ID=$(az network vnet subnet show --resource-group myResourceGroup --vnet-name myAKSVnet --name myAKSSubnet --query id -o tsv)
```

Wijs nu de service-principal voor uw AKS-cluster *Inzender* machtigingen toe aan het virtuele netwerk met behulp van de opdracht [AZ Role Assignment Create][az-role-assignment-create] . Geef uw eigen *\<appId >* op zoals wordt weer gegeven in de uitvoer van de vorige opdracht om de service-principal te maken:

```azurecli-interactive
az role assignment create --assignee <appId> --scope $VNET_ID --role Contributor
```

## <a name="create-an-aks-cluster-in-the-virtual-network"></a>Een AKS-cluster maken in het virtuele netwerk

U hebt nu een virtueel netwerk en subnet gemaakt en machtigingen voor een service-principal gemaakt en toegewezen voor het gebruik van deze netwerk resources. Maak nu een AKS-cluster in uw virtuele netwerk en subnet met behulp van de opdracht [AZ AKS Create][az-aks-create] . Definieer uw eigen service *-principal \<appId >* en *\<password >* , zoals wordt weer gegeven in de uitvoer van de vorige opdracht om de service-principal te maken.

De volgende IP-adresbereiken worden ook gedefinieerd als onderdeel van het proces voor het maken van een cluster:

* De *--service-CIDR* wordt gebruikt voor het toewijzen van interne services in het AKS-cluster een IP-adres. Dit IP-adres bereik moet een adres ruimte zijn die elders niet in uw netwerk omgeving wordt gebruikt. Dit bereik bevat alle on-premises netwerkbereiken als u verbinding maakt met uw virtuele Azure-netwerken of een verbinding wilt maken met behulp van een snelle route of een site-naar-site-VPN-verbinding.

* Het *--DNS-service-IP-* adres moet het *.10* -adres van het IP-adres bereik van uw service zijn.

* De *--pod-CIDR* moet een grote adres ruimte zijn die elders niet in uw netwerk omgeving wordt gebruikt. Dit bereik bevat alle on-premises netwerkbereiken als u verbinding maakt met uw virtuele Azure-netwerken of een verbinding wilt maken met behulp van een snelle route of een site-naar-site-VPN-verbinding.
    * Dit adres bereik moet groot genoeg zijn voor het aantal knoop punten dat u naar verwachting omhoog wilt schalen. U kunt dit adres bereik niet meer wijzigen wanneer het cluster is geïmplementeerd als u meer adressen nodig hebt voor extra knoop punten.
    * Het IP-adres bereik van Pod wordt gebruikt om een */24* -adres ruimte toe te wijzen aan elk knoop punt in het cluster. In het volgende voor beeld wijst de *--pod-CIDR* van *10.244.0.0/16* het eerste knoop punt *10.244.0.0/24*, het tweede knoop punt *10.244.1.0/24*en het derde knoop punt *10.244.2.0/24*toe.
    * Naarmate het cluster wordt geschaald of bijgewerkt, blijft het Azure-platform een IP-adres bereik van pod toewijzen aan elk nieuw knoop punt.
    
* Met de *--docker-Bridge-Address* kunnen de AKS-knoop punten communiceren met het onderliggende beheer platform. Dit IP-adres mag zich niet binnen het IP-adres bereik van het virtuele netwerk van uw cluster bevallen en mag niet overlappen met andere adresbereiken die in het netwerk worden gebruikt.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --network-plugin kubenet \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --pod-cidr 10.244.0.0/16 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal <appId> \
    --client-secret <password>
```

> [!Note]
> Als u een AKS-cluster wilt inschakelen voor het opnemen van een [Calico-netwerk beleid][calico-network-policies] , kunt u de volgende opdracht gebruiken.

```azurecli-interactive
az aks create \
    --resource-group myResourceGroup \
    --name myAKSCluster \
    --node-count 3 \
    --network-plugin kubenet --network-policy calico \
    --service-cidr 10.0.0.0/16 \
    --dns-service-ip 10.0.0.10 \
    --pod-cidr 10.244.0.0/16 \
    --docker-bridge-address 172.17.0.1/16 \
    --vnet-subnet-id $SUBNET_ID \
    --service-principal <appId> \
    --client-secret <password>
```

Wanneer u een AKS-cluster maakt, worden er een netwerk beveiligings groep en route tabel gemaakt. Deze netwerk bronnen worden beheerd door het AKS-besturings vlak. De netwerk beveiligings groep wordt automatisch gekoppeld aan de virtuele Nic's op uw knoop punten. De route tabel wordt automatisch gekoppeld aan het subnet van het virtuele netwerk. Regels voor netwerk beveiligings groepen en route tabellen en worden automatisch bijgewerkt wanneer u Services maakt en weergeeft.

## <a name="next-steps"></a>Volgende stappen

Als er een AKS-cluster in uw bestaande subnet van het virtuele netwerk is geïmplementeerd, kunt u het cluster nu als normaal gebruiken. Ga aan de slag met het [bouwen van apps met behulp van Azure dev Spaces][dev-spaces] of [gebruik concept][use-draft], of [Implementeer apps met behulp van helm][use-helm].

<!-- LINKS - External -->
[dev-spaces]: https://docs.microsoft.com/azure/dev-spaces/
[cni-networking]: https://github.com/Azure/azure-container-networking/blob/master/docs/cni.md
[kubenet]: https://kubernetes.io/docs/concepts/cluster-administration/network-plugins/#kubenet
[Calico-network-policies]: https://docs.projectcalico.org/v3.9/security/calico-network-policy

<!-- LINKS - Internal -->
[install-azure-cli]: /cli/azure/install-azure-cli
[aks-network-concepts]: concepts-network.md
[az-group-create]: /cli/azure/group#az-group-create
[az-network-vnet-create]: /cli/azure/network/vnet#az-network-vnet-create
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-network-vnet-show]: /cli/azure/network/vnet#az-network-vnet-show
[az-network-vnet-subnet-show]: /cli/azure/network/vnet/subnet#az-network-vnet-subnet-show
[az-role-assignment-create]: /cli/azure/role/assignment#az-role-assignment-create
[az-aks-create]: /cli/azure/aks#az-aks-create
[use-helm]: kubernetes-helm.md
[use-draft]: kubernetes-draft.md
[virtual-nodes]: virtual-nodes-cli.md
[vnet-peering]: ../virtual-network/virtual-network-peering-overview.md
[express-route]: ../expressroute/expressroute-introduction.md
[network-comparisons]: concepts-network.md#compare-network-models
