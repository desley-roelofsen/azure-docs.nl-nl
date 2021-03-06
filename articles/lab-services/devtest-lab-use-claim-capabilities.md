---
title: Claim mogelijkheden gebruiken in Azure DevTest Labs | Microsoft Docs
description: Meer informatie over de verschillende scenario's voor het gebruik van claim/claim mogelijkheden van Azure DevTest Labs
services: devtest-lab
documentationcenter: na
author: spelluru
manager: ''
editor: ''
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/12/2019
ms.author: spelluru
ms.openlocfilehash: a15148260bccadc59966c86031100f0e0332b0f9
ms.sourcegitcommit: 470041c681719df2d4ee9b81c9be6104befffcea
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/12/2019
ms.locfileid: "67861428"
---
# <a name="use-claim-capabilities-in-azure-devtest-labs"></a>Claim mogelijkheden gebruiken in Azure DevTest Labs
De Azure DevTest Labs-service verbetert de effectiviteit en efficiëntie van ontwikkel aars en testers. In dit artikel wordt gekeken naar de mogelijkheid om virtuele machines in Azure DevTest Labs te claimen of uit te winnen. Er worden ook verschillende manieren voor het verbeteren van de gebruikers ervaring vermeld. Voordat u verschillende scenario's bekijkt waarin deze functie kan worden gebruikt, bekijken we wat claimt  en hoe het werkt.

## <a name="claimable-machines"></a>Claim bare computers
Een claim bare computer is een virtuele machine (VM) die is gemaakt in een Lab zonder eigenaar. Zodra de computer wordt geclaimd, heeft de gebruiker een groot aantal opties voor die VM. Wanneer een gebruiker een computer claimt, worden er enkele wijzigingen aangebracht. De VM wordt verplaatst van de lijst **claim bare virtuele machines** naar de lijst **mijn virtuele machines** in de Azure Portal. 

De gebruiker kan verbinding maken met de virtuele machine, artefacten aanpassen, opnieuw starten, stoppen of claimen van de computer. Er zijn een aantal manieren om een VM te claimen:

- Maak een machine en claim deze zo dat deze wordt verplaatst naar de claim bare groep. 
- Maak een virtuele machine en plaats deze in de gedeelde groep met behulp van [Geavanceerde instellingen](https://azure.microsoft.com/updates/azure-devtest-labs-claim-lab-vms-from-a-shared-pool/).

Er zijn twee gevallen waarin de claim-claim mogelijkheden effectief kunnen worden gebruikt. De eerste draagtas vereist meer forethought en planning, die moeten worden ontworpen en correct worden uitgevoerd. En de tweede is meer situatie. Hier volgen enkele voor beelden van de verschillende gevallen.

## <a name="designed-use-of-claimable-machines"></a>Ontworpen gebruik van claim bare computers

- **Software ontwikkeling/testen:** Ontwikkel aars of testers toestaan productiever te zijn door het configureren van computers die zijn voor bereid en in een ongeclaimde status. Met een set virtuele machines met verschillende configuraties, benodigde hulpprogram ma's en met de nieuwste code kunnen gebruikers een virtuele machine claimen en aan de slag gaan zonder dat ze de tijd hoeven te best Eden aan het instellen van een machine. Voordat de Vm's worden geclaimd, worden de machines ingericht, maar worden de kosten voor het afsluiten van machines die minder vaak worden gebruikt, geminimaliseerd. Wanneer de Vm's nodig zijn, moet een gebruiker de virtuele machine claimen, waardoor de computer wordt gestart. De optie unclaim is in dit geval niet zo handig, omdat het maken van een nieuwe virtuele machine vaak eenvoudiger en goed koper is.
- **Klassikale/Labs:** Hebben Vm's vooraf geconfigureerd voor een klasse of een lab, zodat studenten direct verbinding kunnen maken met een computer met behulp van de Azure Portal.  Zodra een student een virtuele machine claimt, zorgt het lab ervoor dat niemand op dezelfde computer kan claimen. Door dit proces te automatiseren, zorgt u ervoor dat het vereiste aantal machines met de opgegeven omgeving beschikbaar is. Als studenten niet worden weer gegeven of er te laat zijn, kunnen de onclaimde machines beschikbaar blijven totdat de sessie met minimale kosten wordt uitgevoerd. De optie unclaim is niet zo effectief in dit scenario omdat de virtuele machine een onbekende status heeft wanneer de vorige gebruiker klaar is.
- **Demonstraties** Gebruik machines voor demonstraties, waarbij de machines in het lab worden ingesteld met specifieke omgevingen. Deze mogelijkheid is nuttig wanneer meerdere mensen op hetzelfde moment of op wille keurige momenten een demonstratie kunnen geven, zoals bij een conferentie. De optie unclaim kan nuttig zijn in deze situatie, omdat de demo de status van de machine niet mag wijzigen, zodat gebruikers een VM terug kunnen retour neren naar de claim bare groep voor de volgende demonstratie. Omdat de onclaimde machine ongedaan wordt gemaakt en minimale kosten kost, kunnen Vm's binnen het lab blijven voor langere Peri Oden.
- **Tijdelijke/werk nemers:** Gebruikers toestaan een machine te gebruiken. Wanneer ze verlaten, retour neren ze de virtuele machine naar de claim bare pool zonder gegevens verlies. Als de virtuele machine niet is claimen, kan een andere gebruiker de virtuele machine claimen en door gaan of de computer controleren op aanvullende informatie.
- **In het algemeen:** De mogelijkheid om een exclusieve bron te hebben om automatisch Vm's te configureren en te implementeren op een specifieke uitgebracht is nuttig in veel verschillende situaties. Er zijn verschillende situaties waarin de functie claim/unclaim gebruikers efficiënter kan maken door een geautomatiseerd proces te bieden voor het bouwen van de virtuele machines in een ongeclaimde status met een ingestelde configuratie. De configuratie (s) bevatten mogelijk verschillende besturings systemen, talen, schijven of [andere software (artefacten)](devtest-lab-artifact-author.md) , afhankelijk van uw behoeften. De mogelijkheid om een virtuele machine te claimen vanuit het lab, stelt de test gebruiker in staat om een correct geconfigureerd systeem te verkrijgen zonder de tijd of moeite van het configureren van de machine uit te geven. De test manager kan de geclaimde status van de virtuele machines gebruiken om het aantal gegenereerde machines te verbeteren, machines op te schonen en de prioriteit van configuraties te bepalen. De [Image Factory](image-factory-create.md) is een goed voor beeld van een geautomatiseerd proces voor het bouwen van vm's en installatie kopieën voor meerdere lessen. De scripts kunnen worden gewijzigd om een van de volgende situaties uit te voeren met de juiste wijzigingen of worden gebruikt als referentie voor het maken van een aangepast systeem.

## <a name="situational-use-of-claimable-machines"></a>Situatie gebruik van claim bare computers

- Gebruik de claim-claim mogelijkheid waarmee gebruikers het beheer van de computers van de ene naar de andere kunnen door geven en niet uitdrukkelijk hoeven te weten wie de machine gaat ophalen.
- Het ontwikkelen, testen en opsporen van fouten in een scenario waarbij een bepaalde machine configuratie een fout kan reproduceren, kan de computer niet claimen, waardoor een andere ontwikkelaar de machine kan claimen en het werk kan voortzetten. Deze functie is vooral nuttig omdat meer mensen op afstand werken op verschillende gebieden van de wereld. 
- Team leden kunnen met één omgeving werken. U kunt bijvoorbeeld hand matig een complexe omgeving instellen die niet kan worden geautomatiseerd of resources maken die alleen wijzigingen kunnen verwerken voor één invoer zoals installatie kopieën. In het verleden werd dit probleem opgelost door een toegewezen computer actief te hebben. De claim bare functie is een verbetering ten opzichte van het hand matige proces door gebruik te maken van ingebouwde gebruikers toegangs beheer en visuele identificatie, indien beschikbaar. Wanneer de virtuele machine niet is claimd, wordt de inrichting ongedaan gemaakt om de kosten te verlagen.
- Beschikken over een gegevens schijf die is gekoppeld aan een virtuele machine. Elke schijf tot Maxi maal ~ 1 TB aan gegevens kan een grote hoeveelheid gegevens worden door gegeven zonder dat de gegevens moeten worden gekopieerd of gedupliceerd. De virtuele machine wordt in eerste instantie gemaakt met een gekoppelde schijf met de grote hoeveelheid gegevens.  Elke gebruiker kan vervolgens de computer claimen en toegang krijgen tot de gegevens. Als u klaar bent, moet u de virtuele machine claimen om andere gebruikers toe te staan voor de computer.

Er zijn enkele voor behoud van het gebruik van claim bare computers, meestal om toegang te krijgen tot de computer. Als de computer lid is van een domein, moet aan de gebruiker die de computer claimt, al toegang worden verleend. dit gebeurt meestal door toegang te verlenen aan een groep die alle gebruikers in het lab omvat wanneer de virtuele machine wordt gemaakt. Als de computer geen lid is van een domein, moet het **wacht woord voor het opnieuw instellen** van de VM in de open bare opslag plaats worden uitgevoerd om de gebruiker als beheerder toe te voegen.  Artefacten kunnen worden toegepast, zelfs nadat de machine is gestart of geclaimd.

## <a name="next-steps"></a>Volgende stappen
Raadpleeg het volgende artikel: [Bewering bare Vm's maken en beheren in Azure DevTest Labs](devtest-lab-add-claimable-vm.md)
