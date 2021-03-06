---
title: Schaling van Azure Service Fabric-cluster | Microsoft Docs
description: Meer informatie over het schalen van Azure Service Fabric-clusters in of uit en omhoog of omlaag.
services: service-fabric
documentationcenter: .net
author: athinanthny
manager: chackdan
ms.assetid: 5441e7e0-d842-4398-b060-8c9d34b07c48
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: conceptual
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 11/13/2018
ms.author: atsenthi
ms.openlocfilehash: c4d7027438f19cd16fd87d629364cdf725e91607
ms.sourcegitcommit: fe6b91c5f287078e4b4c7356e0fa597e78361abe
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/29/2019
ms.locfileid: "68599855"
---
# <a name="scaling-azure-service-fabric-clusters"></a>Azure Service Fabric-clusters schalen
Een Service Fabric cluster is een met het netwerk verbonden reeks virtuele of fysieke machines waarop uw micro services worden geïmplementeerd en beheerd. Een computer of virtuele machine die deel uitmaakt van een cluster, wordt een knoop punt genoemd. Clusters kunnen mogelijk duizenden knoop punten bevatten. Nadat u een Service Fabric cluster hebt gemaakt, kunt u het cluster horizon taal schalen (Wijzig het aantal knoop punten) of verticaal (Wijzig de resources van de knoop punten).  U kunt het cluster op elk gewenst moment schalen, zelfs wanneer werk belastingen op het cluster worden uitgevoerd.  Naarmate het cluster wordt geschaald, worden uw toepassingen ook automatisch geschaald.

Waarom het cluster schalen? Toepassings vereisten veranderen in de loop van de tijd.  Mogelijk moet u cluster bronnen verg Roten om te voldoen aan de toegenomen werk belasting van de toepassing of het netwerk verkeer of cluster bronnen verlagen wanneer de vraag wordt neergezet.

## <a name="scaling-in-and-out-or-horizontal-scaling"></a>In-en uitschalen of horizon taal schalen
Hiermee wijzigt u het aantal knoop punten in het cluster.  Zodra de nieuwe knoop punten aan het cluster zijn toegevoegd, worden de services door de [cluster resource manager](service-fabric-cluster-resource-manager-introduction.md) verplaatst, waardoor de belasting van de bestaande knoop punten vermindert.  U kunt ook het aantal knoop punten verlagen als de resources van het cluster niet efficiënt worden gebruikt.  Als knoop punten het cluster verlaten, worden deze knoop punten door de services verplaatst en nemen de taken toe op de resterende knoop punten.  Het verminderen van het aantal knoop punten in een cluster dat in azure wordt uitgevoerd, bespaart u geld, omdat u betaalt voor het aantal Vm's dat u gebruikt en niet de werk belasting van deze Vm's.  

- Delen Oneindige schaal, in theorie.  Als uw toepassing is ontworpen voor schaal baarheid, kunt u onbeperkte groei inschakelen door meer knoop punten toe te voegen.  Met het hulp programma in Cloud omgevingen kunt u eenvoudig knoop punten toevoegen of verwijderen, zodat u de capaciteit eenvoudig kunt aanpassen en u alleen betaalt voor de resources die u gebruikt.  
- Nadelen Toepassingen moeten zijn [ontworpen voor schaal baarheid](service-fabric-concepts-scalability.md).  Voor toepassings databases en-persistentie is het mogelijk dat extra architectuur werk ook kan worden geschaald.  [Betrouw bare verzamelingen](service-fabric-reliable-services-reliable-collections.md) in service Fabric stateful Services, maar maak het veel eenvoudiger om uw toepassings gegevens te schalen.

Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunt type dat in een Azure-cluster is gedefinieerd, wordt [ingesteld als een afzonderlijke schaalset](service-fabric-cluster-nodetypes.md). Elk knooppunttype kan vervolgens worden geschaald of uit onafhankelijk van elkaar, hebben verschillende sets open poorten en verschillende capaciteitsstatistieken hebben. 

Houd bij het schalen van een Azure-cluster de volgende richt lijnen in acht:
- primaire knooppunt typen die productie werkbelastingen uitvoeren, moeten altijd vijf of meer knoop punten bevatten.
- niet-primaire knooppunt typen waarvoor stateful productie workloads worden uitgevoerd, moeten altijd vijf of meer knoop punten hebben.
- niet-primaire knooppunt typen waarvoor stateless productie werkbelastingen worden uitgevoerd, moeten altijd twee of meer knoop punten hebben.
- Elk knooppunt type van het [duurzaamheids niveau](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster) goud of zilver moet altijd vijf of meer knoop punten bevatten.
- Verwijder geen wille keurige VM-exemplaren/knoop punten uit een knooppunt type en gebruik altijd de functie schaalset schaalset voor virtuele machines. Het verwijderen van wille keurige VM-instanties kan een nadelige invloed hebben op de systeem capaciteit om de taak verdeling te vervolledigen.
- Als u regels voor automatisch schalen gebruikt, stelt u de regels zodanig in dat schalen (VM-exemplaren verwijderen) één knoop punt tegelijk wordt uitgevoerd. Het omlaag schalen van meer dan één exemplaar op een keer is niet veilig.

Aangezien de Service Fabric knooppunt typen in uw cluster bestaan uit virtuele-machine schaal sets op de back-end, kunt u [regels voor automatisch schalen instellen of](service-fabric-cluster-scale-up-down.md) elk knooppunt type/virtuele-machine schaalset hand matig schalen.

### <a name="programmatic-scaling"></a>Programmatisch schalen
In veel gevallen kunt u [een cluster hand matig schalen of met regels voor automatisch schalen](service-fabric-cluster-scale-up-down.md) zijn er goede oplossingen. Voor meer geavanceerde scenario's is het mogelijk dat ze niet aan de rechter kant voldoen. Mogelijke nadelen van deze benaderingen zijn:

- Voor hand matig schalen moet u zich aanmelden en expliciet schaal bewerkingen aanvragen. Als schaal bewerkingen vaak of op onvoorspelbare tijden vereist zijn, is het mogelijk dat deze methode geen goede oplossing is.
- Als regels voor automatisch schalen een exemplaar uit een schaalset voor virtuele machines verwijderen, wordt de kennis van dat knoop punt niet automatisch uit het gekoppelde Service Fabric cluster verwijderd, tenzij het knooppunt type een duurzaamheids niveau van zilver of goud heeft. Omdat regels voor automatisch schalen op het niveau van de schaalset werken (in plaats van op het Service Fabric niveau), kunnen regels voor automatisch schalen Service Fabric knoop punten verwijderen zonder dat ze correct worden afgesloten. Tijdens het verwijderen van het beleefd-knoop punt wordt de status van ' Ghost ' Service Fabric knoop punt achter na de scale-out-bewerkingen. Een persoon (of een service) zou de verwijderde knooppunt status periodiek moeten opschonen in het Service Fabric cluster.
- Een knooppunt type met een duurzaamheids niveau van goud of zilver verwijdert automatisch verwijderde knoop punten, zodat er geen extra opschoning nodig is.
- Hoewel er [veel metrische gegevens](../azure-monitor/platform/autoscale-common-metrics.md) worden ondersteund door regels voor automatisch schalen, is het nog steeds een beperkte set. Als uw scenario voor schalen aanroept op basis van bepaalde metrische gegevens die niet in deze set zijn opgenomen, zijn de regels voor automatisch schalen mogelijk niet een goede optie.

Hoe u aanpakt Service Fabric schalen, is afhankelijk van uw scenario. Als schalen ongebruikelijk is, is de mogelijkheid om knoop punten hand matig toe te voegen of te verwijderen waarschijnlijk voldoende. Voor complexere scenario's kunt u regels voor automatisch schalen en Sdk's die de mogelijkheid bieden om programmatisch schaalbaar te maken, voorzien van krachtige alternatieven.

Er bestaan Azure-Api's waarmee toepassingen programmatisch kunnen werken met schaal sets voor virtuele machines en Service Fabric clusters. Als er geen bestaande opties voor automatisch schalen voor uw scenario werken, maken deze Api's het mogelijk om aangepaste schaal logica te implementeren. 

Een van de methoden voor het implementeren van de functie voor automatische schaal aanpassing van ' Home ' is het toevoegen van een nieuwe stateless service aan de Service Fabric toepassing voor het beheren van schaal bewerkingen. Het maken van uw eigen schaal service biedt de hoogste mate van controle en aanpassings mogelijkheden over het schaal gedrag van uw toepassing. Dit kan handig zijn voor scenario's waarin nauw keurige controle is vereist of hoe een toepassing in of uit wordt geschaald. Dit besturings element wordt echter wel geleverd met een toenemende code complexiteit. Het gebruik van deze aanpak betekent dat u een eigen schaal code nodig hebt. Dit is niet-trivial. Binnen de `RunAsync` methode van de service kan een reeks Triggers bepalen of schalen is vereist (inclusief het controleren van para meters, zoals de maximale cluster grootte en het schalen van cooldowns).   

De API die wordt gebruikt voor interacties van virtuele-machine schaal sets (beide om het huidige aantal exemplaren van virtuele machines te controleren en deze te wijzigen) is de [Fluent Azure Management Compute-bibliotheek](https://www.nuget.org/packages/Microsoft.Azure.Management.Compute.Fluent/). De Fluent Compute-bibliotheek biedt een gebruiks vriendelijke API voor interactie met virtuele-machine schaal sets.  Als u wilt communiceren met het Service Fabric cluster zelf, gebruikt u [System. Fabric. FabricClient](/dotnet/api/system.fabric.fabricclient).

De schaal code hoeft niet te worden uitgevoerd als een service in het cluster, maar moet worden geschaald. Beide `IAzure` en`FabricClient` kunnen op afstand verbinding maken met de gekoppelde Azure-resources, waardoor de schaal service eenvoudig een console toepassing of Windows-service van buiten de service Fabric toepassing kan zijn.

Op basis van deze beperkingen wilt u mogelijk [meer aangepaste modellen voor automatisch schalen implementeren](service-fabric-cluster-programmatic-scaling.md).

## <a name="scaling-up-and-down-or-vertical-scaling"></a>Omhoog en omlaag schalen of verticaal schalen 
Hiermee wijzigt u de resources (CPU, geheugen of opslag) van knoop punten in het cluster.
- Delen De software-en toepassings architectuur blijft hetzelfde.
- Nadelen Eindige schaal, omdat er een limiet is voor de hoeveelheid resources op afzonderlijke knoop punten. Uitval tijd, omdat u fysieke of virtuele machines offline moet halen om resources toe te voegen of te verwijderen.

Virtuele-machineschaalsets vormen een Azure compute-resource die u gebruiken kunt om te implementeren en beheren van een verzameling van virtuele machines als een set. Elk knooppunt type dat in een Azure-cluster is gedefinieerd, wordt [ingesteld als een afzonderlijke schaalset](service-fabric-cluster-nodetypes.md). Elk knooppunt type kan vervolgens afzonderlijk worden beheerd.  Als u een knooppunt type omhoog of omlaag wilt schalen, moet u de SKU van de virtuele-machine-exemplaren in de schaalset wijzigen. 

> [!WARNING]
> We raden u aan de VM-SKU van een schaalset/knooppunt type alleen te wijzigen als deze wordt uitgevoerd in een [zilver duurzaamheid of hoger](service-fabric-cluster-capacity.md#the-durability-characteristics-of-the-cluster). Het wijzigen van de SKU-grootte van de VM is een in-place infrastructuur bewerking voor het uitvoeren van gegevens. Zonder enige mogelijkheid om deze wijziging te vertragen of te bewaken, is het mogelijk dat de bewerking gegevens verlies kan veroorzaken voor stateful Services of andere onvoorziene operationele problemen veroorzaakt, zelfs voor stateless workloads. 
>

Houd bij het schalen van een Azure-cluster de volgende richt lijnen in acht:
- Als u het niveau van een primair knoop punt omlaag wilt schalen, moet u het type nooit meer aanpassen dan wat de [betrouwbaarheids categorie](service-fabric-cluster-capacity.md#the-reliability-characteristics-of-the-cluster) toestaat.

Het proces voor het omhoog of omlaag schalen van een knooppunt type verschilt, afhankelijk van of het een niet-primair of primair knooppunt type is.

### <a name="scaling-non-primary-node-types"></a>Het schalen van niet-primaire knooppunt typen
Maak een nieuw knooppunt type met de resources die u nodig hebt.  Werk de plaatsings beperkingen van actieve services bij om het nieuwe knooppunt type op te laten bevatten.  Verlaag geleidelijk (één op een keer) het aantal exemplaren van het oude knooppunt type van het aantal instanties in nul, zodat de betrouw baarheid van het cluster niet wordt beïnvloed.  Services worden geleidelijk naar het nieuwe knooppunt type gemigreerd, omdat het oude knooppunt type buiten gebruik wordt gesteld.

### <a name="scaling-the-primary-node-type"></a>Het primaire knooppunt type schalen
We raden u aan de VM-SKU van het primaire knooppunt type niet te wijzigen. Als u meer cluster capaciteit nodig hebt, raden we u aan meer exemplaren toe te voegen. 

Als dat niet mogelijk is, kunt u een nieuw cluster maken en de [toepassings status](service-fabric-reliable-services-backup-restore.md) (indien van toepassing) herstellen van het oude cluster. U hoeft geen systeem service status te herstellen, ze worden opnieuw gemaakt wanneer u uw toepassingen naar het nieuwe cluster implementeert. Als u alleen stateless toepassingen op uw cluster hebt uitgevoerd, kunt u uw toepassingen ook implementeren naar het nieuwe cluster. u hoeft niets te herstellen. Als u besluit de niet-ondersteunde route te gaan en de VM-SKU wilt wijzigen, moet u wijzigingen aanbrengen in de model definitie van de virtuele-machine Scale set zodat deze overeenkomen met de nieuwe SKU. Als uw cluster slechts één knooppunt type heeft, moet u ervoor zorgen dat alle stateful toepassingen reageren op alle gebeurtenissen van de [levens cyclus van de service replica](service-fabric-reliable-services-lifecycle.md) (zoals replica in Build is vastgelopen) en dat de duur van de service replica opnieuw moet worden opgebouwd en kleiner is dan vijf minuten (voor het niveau van de Silver-duurzaamheid). 

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over [schaal baarheid van toepassingen](service-fabric-concepts-scalability.md).
* [Een Azure-cluster in-of uitschalen](service-fabric-tutorial-scale-cluster.md).
* [Schaal een Azure-cluster programmatisch](service-fabric-cluster-programmatic-scaling.md) met behulp van de Fluent Azure Compute SDK.
* [Een zelfstandige cluster in-of uitschalen](service-fabric-cluster-windows-server-add-remove-nodes.md).

