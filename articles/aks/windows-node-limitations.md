---
title: Beperkingen voor Windows Server-knooppunt groepen in azure Kubernetes service (AKS)
description: Meer informatie over de bekende beperkingen bij het uitvoeren van Windows Server-knooppunt groepen en toepassings werkbelastingen in azure Kubernetes service (AKS)
services: container-service
author: mlearned
ms.service: container-service
ms.topic: article
ms.date: 05/31/2019
ms.author: mlearned
ms.openlocfilehash: 3a57fbb010f8a04352d09d4b6d57cf465e3e6988
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74279162"
---
# <a name="current-limitations-for-windows-server-node-pools-and-application-workloads-in-azure-kubernetes-service-aks"></a>Huidige beperkingen voor Windows Server-knooppunt groepen en toepassings werkbelastingen in azure Kubernetes service (AKS)

In azure Kubernetes service (AKS) kunt u een knooppunt groep maken waarop Windows Server als het gast besturingssysteem op de knoop punten wordt uitgevoerd. Deze knoop punten kunnen systeem eigen Windows-container toepassingen uitvoeren, zoals die zijn gebaseerd op de .NET Framework. Omdat er belang rijke verschillen zijn in de manier waarop het Linux-en Windows-besturings systeem ondersteuning bieden voor containers, zijn bepaalde algemene Kubernetes-en pod-functies momenteel niet beschikbaar voor Windows-knooppunt groepen.

In dit artikel vindt u een overzicht van enkele beperkingen en besturingssysteem concepten voor Windows Server-knoop punten in AKS. Knooppunt groepen voor Windows Server zijn momenteel beschikbaar als preview-versie.

> [!IMPORTANT]
> De preview-functies van AKS zijn self-service opt-in. Previews worden ' as-is ' en ' as available ' gegeven en zijn uitgesloten van de service level agreements en beperkte garantie. AKS-previews worden gedeeltelijk gedekt door klant ondersteuning, op basis van de beste inspanningen. Daarom zijn deze functies niet bedoeld voor productie gebruik. Raadpleeg de volgende ondersteunings artikelen voor meer informatie:
>
> * [AKS-ondersteunings beleid][aks-support-policies]
> * [Veelgestelde vragen over ondersteuning voor Azure][aks-faq]

## <a name="which-windows-operating-systems-are-supported"></a>Welke Windows-besturings systemen worden ondersteund?

AKS maakt gebruik van Windows Server 2019 als de versie van het besturings systeem van de host en ondersteunt alleen isolatie van processen. Container installatie kopieën die zijn gemaakt met andere versies van Windows Server, worden niet ondersteund. [Compatibiliteit met Windows-container versie][windows-container-compat]

## <a name="is-kubernetes-different-on-windows-and-linux"></a>Is Kubernetes anders in Windows en Linux?

Ondersteuning voor de venster Server knooppunt groep bevat enkele beperkingen die deel uitmaken van de upstream-Windows-Server in Kubernetes-project. Deze beperkingen zijn niet specifiek voor AKS. Zie de sectie [ondersteunde functionaliteit en beperkingen][upstream-limitations] van het project [Inleiding tot Windows-ondersteuning in Kubernetes][intro-windows] -document in het Kubernetes-bestand voor meer informatie over deze ondersteuning voor upstreams voor Windows Server in Kubernetes.

Kubernetes is historisch gericht op Linux. Veel voor beelden die worden gebruikt in de upstream [Kubernetes.io][kubernetes] -website zijn bedoeld voor gebruik op Linux-knoop punten. Wanneer u implementaties maakt die gebruikmaken van Windows Server-containers, gelden de volgende overwegingen op het niveau van het besturings systeem:

- **Identiteit** -Linux identificeert een gebruiker op basis van een gebruikers-id (UID) van een geheel getal. Een gebruiker heeft ook een alfanumerieke gebruikers naam voor aanmelding, die Linux vertaalt naar de UID van de gebruiker. Net als Linux wordt een gebruikers groep aangeduid met een groeps-id (GID) en wordt een groeps naam omgezet in de bijbehorende GID.
    - Windows Server gebruikt een grotere binaire beveiligings-id (SID) die is opgeslagen in de SAM-data base (Windows Security Access Manager). Deze data base wordt niet gedeeld tussen de host en containers of tussen containers.
- **Bestands machtigingen** : Windows Server gebruikt een toegangs beheer lijst op basis van sid's, in plaats van een bitmasker van machtigingen en UID + GID
- **Bestands paden** : de Conventie voor Windows Server is door gebruik te gebruiken in plaats van/.
    - In pod-specificaties die volumes koppelen, geeft u het pad correct voor Windows Server-containers op. Geef bijvoorbeeld in plaats van een koppel punt van */mnt/volume* in een Linux-container een stationsletter en locatie op, zoals */K/volume* , die moeten worden gekoppeld als het station *K:* .

## <a name="what-kind-of-disks-are-supported-for-windows"></a>Wat voor soort schijven worden ondersteund voor Windows?

Azure-schijven en-Azure Files zijn de ondersteunde volume typen, toegankelijk als NTFS-volumes in de Windows Server-container.

## <a name="can-i-run-windows-only-clusters-in-aks"></a>Kan ik alleen Windows-clusters in AKS uitvoeren?

De hoofd knooppunten (het besturings vlak) in een AKS-cluster worden gehost door AKS de service. u wordt niet blootgesteld aan het besturings systeem van de knoop punten die de hoofd onderdelen hosten. Alle AKS-clusters worden gemaakt met een standaard eerste knooppunt groep, die is gebaseerd op Linux. Deze knooppunt groep bevat systeem services, die nodig zijn voor het functioneren van het cluster. Het is raadzaam ten minste twee knoop punten in de eerste knooppunt groep uit te voeren om de betrouw baarheid van uw cluster en de mogelijkheid om cluster bewerkingen te kunnen garanderen. De eerste knooppunt groep op basis van Linux kan alleen worden verwijderd als het AKS-cluster zelf is verwijderd.

## <a name="what-network-plug-ins-are-supported"></a>Welke netwerk-invoeg toepassingen worden ondersteund?

AKS-clusters met Windows-knooppunt Pools moeten het netwerk model van Azure CNI (Geavanceerd) gebruiken. Kubenet (Basic)-netwerken worden niet ondersteund. Zie [Network concepten for Applications in AKS][azure-network-models](Engelstalig) voor meer informatie over de verschillen in netwerk modellen. -Het Azure CNI-netwerk model vereist aanvullende planning en overwegingen voor het beheer van IP-adressen. Zie [Azure cni-netwerken configureren in AKS][configure-azure-cni]voor meer informatie over het plannen en implementeren van Azure cni.

## <a name="can-i-change-the-min--of-pods-per-node"></a>Kan ik het minimum aantal van elk per knoop punt wijzigen?

Het is momenteel een vereiste om te worden ingesteld op mini maal 30, om de betrouw baarheid van uw clusters te garanderen.

## <a name="how-do-patch-my-windows-nodes"></a>Hoe Patch ik mijn Windows-knoop punten?

Windows Server-knoop punten in AKS moeten worden *bijgewerkt* om de meest recente patches en updates te downloaden. Windows-updates zijn niet ingeschakeld op knoop punten in AKS. AKS geeft nieuwe installatie kopieën van de groep knoop punten weer zodra er patches beschikbaar zijn, is het de verantwoordelijkheid van de klanten om de knooppunt groepen bij te werken om op de hoogte te blijven van patches en hotfixes. Dit geldt ook voor het gebruik van de Kubernetes-versie. AKS release opmerkingen geven aan wanneer er nieuwe versies beschikbaar zijn. Zie [een knooppunt groep bijwerken in AKS][nodepool-upgrade]voor meer informatie over het upgraden van een Windows Server-knooppunt groep.

> [!NOTE]
> De bijgewerkte installatie kopie van Windows Server wordt alleen gebruikt als er een upgrade van een cluster is uitgevoerd voordat de knooppunt groep werd geüpgraded
>

## <a name="how-many-node-pools-can-i-create"></a>Hoeveel knooppunt groepen kan ik maken?

Het AKS-cluster kan Maxi maal acht (8) knooppunt Pools hebben. U kunt Maxi maal 400 knoop punten in deze knooppunt groepen hebben. [Beperkingen van de knooppunt groep][nodepool-limitations].

## <a name="what-can-i-name-my-windows-node-pools"></a>Wat kan ik mijn Windows-knooppunt groepen noemen?

U moet de naam Maxi maal 6 (zes) tekens gebruiken. Dit is een huidige beperking van AKS.

## <a name="are-all-features-supported-with-windows-nodes"></a>Worden alle functies ondersteund met Windows-knoop punten?

Netwerk beleid en kubenet worden momenteel niet ondersteund met Windows-knoop punten. 

## <a name="can-i-run-ingress-controllers-on-windows-nodes"></a>Kan ik ingangs controllers uitvoeren op Windows-knoop punten?

Ja, een ingangs-controller die ondersteuning biedt voor Windows Server-containers kan worden uitgevoerd op Windows-knoop punten in AKS.

## <a name="can-i-use-azure-dev-spaces-with-windows-nodes"></a>Kan ik Azure dev Spaces gebruiken met Windows-knoop punten?

Azure dev Spaces is momenteel alleen beschikbaar voor knooppunt groepen op basis van Linux.

## <a name="can-my-windows-server-containers-use-gmsa"></a>Kan mijn Windows Server-containers gMSA gebruiken?

GMSA-ondersteuning (Group managed service accounts) is momenteel niet beschikbaar in AKS.

## <a name="can-i-use-azure-monitor-for-containers-with-windows-nodes-and-containers"></a>Kan ik Azure Monitor gebruiken voor containers met Windows-knoop punten en containers?

Ja, u kunt Azure Monitor echter geen logboeken (stdout) uit Windows-containers verzamelen. U kunt nog steeds koppelen aan de live stream van stdout-logboeken vanuit een Windows-container.

## <a name="what-if-i-need-a-feature-which-is-not-supported"></a>Wat moet ik doen als ik een functie heb nodig die niet wordt ondersteund?

We werken hard om alle functies te gebruiken die u nodig hebt voor Windows in AKS, maar als er hiaten optreden, biedt het open source-upstream [-AKS-engine-][aks-engine] project een eenvoudige en volledig aanpas bare manier om Kubernetes in azure uit te voeren, waaronder Windows-ondersteuning. Zorg ervoor dat u het overzicht van de functies van het [AKS-plan][aks-roadmap]bekijkt.

## <a name="next-steps"></a>Volgende stappen

Om aan de slag te gaan met Windows Server-containers in AKS, [maakt u een knooppunt groep die Windows Server in AKS uitvoert][windows-node-cli].

<!-- LINKS - external -->
[kubernetes]: https://kubernetes.io
[aks-engine]: https://github.com/azure/aks-engine
[upstream-limitations]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/#supported-functionality-and-limitations
[intro-windows]: https://kubernetes.io/docs/setup/production-environment/windows/intro-windows-in-kubernetes/
[aks-roadmap]: https://github.com/Azure/AKS/projects/1

<!-- LINKS - internal -->
[azure-network-models]: concepts-network.md#azure-virtual-networks
[configure-azure-cni]: configure-azure-cni.md
[nodepool-upgrade]: use-multiple-node-pools.md#upgrade-a-node-pool
[windows-node-cli]: windows-container-cli.md
[aks-support-policies]: support-policies.md
[aks-faq]: faq.md
[azure-outbound-traffic]: ../load-balancer/load-balancer-outbound-connections.md#defaultsnat
[nodepool-limitations]: use-multiple-node-pools.md#limitations
[preview-support]: support-policies.md#preview-features-or-feature-flags
[windows-container-compat]: /virtualization/windowscontainers/deploy-containers/version-compatibility?tabs=windows-server-2019%2Cwindows-10-1909
