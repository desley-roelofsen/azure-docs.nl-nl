---
title: Meer informatie over de ondersteunings opties voor Azure Service Fabric | Microsoft Docs
description: Ondersteunde cluster versies van Azure Service Fabric en koppelingen naar bestands ondersteunings tickets
services: service-fabric
documentationcenter: .net
author: pkcsf
manager: jpconnock
editor: ''
ms.assetid: ''
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: troubleshooting
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 8/24/2018
ms.author: pkc
ms.openlocfilehash: ac36b1a538d5899613e19f157695d0bc333d9ece
ms.sourcegitcommit: 13d5eb9657adf1c69cc8df12486470e66361224e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/31/2019
ms.locfileid: "68679062"
---
# <a name="azure-service-fabric-support-options"></a>Ondersteunings opties voor Azure Service Fabric

Voor het leveren van de juiste ondersteuning voor uw Service Fabric clusters waarmee u uw toepassings werk uitvoert, worden er verschillende opties voor u ingesteld. Afhankelijk van het benodigde ondersteunings niveau en de ernst van het probleem, kunt u de juiste opties kiezen. 

## <a name="report-production-issues-or-request-paid-support-for-azure"></a>Productie problemen melden of betaalde ondersteuning aanvragen voor Azure

Voor het melden van problemen op uw Service Fabric cluster dat in Azure is geïmplementeerd, opent u een ticket voor ondersteuning [op Azure Portal](https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview) of [micro soft](https://support.microsoft.com/oas/default.aspx?prid=16146)-ondersteunings portal.

Meer informatie over:
 
- [Ondersteuning van micro soft voor Azure](https://azure.microsoft.com/support/plans/?b=16.44).
- [Micro soft Premier Support](https://support.microsoft.com/en-us/premier).

> [!Note]
> Met clusters die worden uitgevoerd op een Bronze-betrouwbaarheids-of een cluster met één knoop punt, kunt u alleen test werkbelastingen uitvoeren. Als u problemen ondervindt met een cluster dat wordt uitgevoerd op Bronze betrouw baarheid of cluster met één knoop punt, helpt het micro soft-ondersteunings team u bij het beperken van het probleem, maar wordt er geen analyse van de hoofd oorzaak uitgevoerd. Raadpleeg [de betrouw bare kenmerken van het cluster](https://docs.microsoft.com/azure/service-fabric/service-fabric-cluster-capacity#the-reliability-characteristics-of-the-cluster) voor meer informatie.
>
> Raadpleeg de [controle lijst voor productie voorbereiding](https://docs.microsoft.com/azure/service-fabric/service-fabric-production-readiness-checklist)voor meer informatie over wat er is vereist voor een cluster dat gereed is voor productie.

<a id="getlivesitesupportonprem"></a>

## <a name="report-production-issues-or-request-paid-support-for-standalone-service-fabric-clusters"></a>Productie problemen melden of betaalde ondersteuning aanvragen voor zelfstandige Service Fabric clusters

Voor het melden van problemen op uw Service Fabric-cluster die on-premises of in andere Clouds zijn geïmplementeerd, opent u een ticket voor professionele ondersteuning op de [micro soft](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/overview)-ondersteunings portal.

Meer informatie over:

- [Professionele ondersteuning van micro soft voor on-premises](https://support.microsoft.com/en-us/gp/offerprophone?wa=wsignin1.0).
- [Micro soft Premier Support](https://support.microsoft.com/en-us/premier).

## <a name="report-azure-service-fabric-issues"></a>Problemen met Azure-Service Fabric melden

We hebben een GitHub-opslag plaats ingesteld voor het melden van Service Fabric problemen.  De volgende forums worden ook actief bewaakt.

### <a name="github-repo"></a>GitHub-opslagplaats 

Meld problemen met Azure Service Fabric op [service-Fabric-problemen Git opslag plaats](https://github.com/Azure/service-fabric-issues). Deze opslag plaats is bedoeld voor rapportage-en tracerings problemen met Azure Service Fabric en voor het maken van kleine functie aanvragen. **Gebruik deze niet om problemen met Live-sites te melden**.

### <a name="stackoverflow-and-msdn-forums"></a>Stack overflow en MSDN-Forums

De [service Fabric-tag op stack overflow][stackoverflow] en het [service Fabric-forum op MSDN][msdn-forum] worden het beste gebruikt voor het stellen van vragen over hoe het platform werkt en hoe u deze taken kunt uitvoeren.

### <a name="azure-feedback-forum"></a>Azure-feedback forum

Het [Azure-feedback forum voor service Fabric][uservoice-forum] is de beste plaats voor het verzenden van belang rijke ideeën die u voor het product hebt, zoals we controleren of de populairste aanvragen deel uitmaken van onze middel grote tot lange termijn planning. We raden u aan rally ondersteuning te bieden voor uw suggesties in de community.

## <a name="service-fabric-preview-versions---unsupported-for-production-use"></a>Preview-versies van Service Fabric-niet ondersteund voor productie gebruik

Van tijd tot tijd brengen we versies uit met belang rijke functies waarop we feedback willen hebben. deze worden uitgebracht als previews. Deze Preview-versies mogen alleen worden gebruikt voor test doeleinden. In uw productie cluster moet altijd een ondersteunde, stabiele, Service Fabric versie worden uitgevoerd. Een preview-versie begint altijd met een primair en secundair versie nummer van 255. Als u bijvoorbeeld een Service Fabric versie 255.255.5703.949 ziet, wordt die release versie alleen gebruikt in test clusters en is deze beschikbaar als preview. Deze Preview-versies worden ook aangekondigd op het [service Fabric team blog](https://blogs.msdn.microsoft.com/azureservicefabric) en bevatten details over de functies die zijn opgenomen.
Er is geen betaalde ondersteunings optie voor deze Preview-versies. Gebruik een van de opties die worden vermeld onder [rapport over Azure-service Fabric-problemen](https://docs.microsoft.com/azure/service-fabric/service-fabric-support#report-azure-service-fabric-issues) om vragen te stellen of feedback te geven.

## <a name="next-steps"></a>Volgende stappen

[Ondersteunde Service Fabric versies](service-fabric-versions.md)

<!--references-->
[msdn-forum]: https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureServiceFabric
[stackoverflow]: https://stackoverflow.com/questions/tagged/azure-service-fabric
[uservoice-forum]: https://feedback.azure.com/forums/293901-service-fabric
[acom-docs]: https://aka.ms/servicefabricdocs
[sample-repos]: https://aka.ms/servicefabricsamples
