---
title: Prestatie problemen met Intelligent Insights oplossen
description: Intelligent Insights helpt u bij het oplossen van problemen met de Azure SQL Database prestatie.
services: sql-database
ms.service: sql-database
ms.subservice: performance
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: danimir
ms.author: danil
ms.reviewer: jrasnik, carlrab
ms.date: 01/25/2019
ms.openlocfilehash: b6ec1952d730b6515032572def65806a1ccbc0b2
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73810372"
---
# <a name="troubleshoot-azure-sql-database-performance-issues-with-intelligent-insights"></a>Problemen met de Azure SQL Database prestaties oplossen met Intelligent Insights

Op deze pagina vindt u informatie over de prestatie problemen Azure SQL Database en beheerde exemplaren die zijn gedetecteerd via het logboek voor diagnostische gegevens over de [intelligent Insights](sql-database-intelligent-insights.md) database prestaties. De telemetrie van het diagnostische logboek kan worden gestreamd naar [Azure monitor-logboeken](../azure-monitor/insights/azure-sql.md), [Azure Event hubs](../azure-monitor/platform/resource-logs-stream-event-hubs.md), [Azure Storage](sql-database-metrics-diag-logging.md#stream-into-storage)of een oplossing van derden voor aangepaste DevOps-waarschuwingen en rapportage mogelijkheden.

> [!NOTE]
> Zie de [Aanbevolen stroom diagram voor probleem oplossing](sql-database-intelligent-insights-troubleshoot-performance.md#recommended-troubleshooting-flow) in dit document voor een snelle SQL database voor het oplossen van problemen met intelligent Insights.
>

## <a name="detectable-database-performance-patterns"></a>Prestatie patronen voor Detecteer bare data bases

Intelligent Insights detecteert automatisch prestatie problemen met de SQL Database en de data bases van het beheerde exemplaar op basis van de wacht tijden van de query uitvoering, fouten of time-outs. Er worden prestatie patronen gedetecteerd die worden uitgevoerd in het diagnostische logboek. In de onderstaande tabel vindt u een overzicht van gedetecteerde prestatie patronen.

| Gedetecteerde prestatie patronen | Beschrijving van Azure SQL Database en elastische Pools | Beschrijving voor data bases in een beheerd exemplaar |
| :------------------- | ------------------- | ------------------- |
| [Bron limieten bereiken](sql-database-intelligent-insights-troubleshoot-performance.md#reaching-resource-limits) | Het verbruik van beschik bare resources (Dtu's), data base worker-threads of data base-aanmeldings sessies die beschikbaar zijn op het bewaakte abonnement, heeft limieten bereikt. Dit is van invloed op de SQL Database prestaties. | Het verbruik van CPU-Resources bereikt de limieten voor beheerde exemplaren. Dit is van invloed op de prestaties van de data base. |
| [Toename van de workload](sql-database-intelligent-insights-troubleshoot-performance.md#workload-increase) | Er is een verhoging of continue accumulatie van de werk belasting voor de data base gedetecteerd. Dit is van invloed op de SQL Database prestaties. | Er is een toename van de werk belasting gedetecteerd. Dit is van invloed op de prestaties van de data base. |
| [Geheugen druk](sql-database-intelligent-insights-troubleshoot-performance.md#memory-pressure) | Werk nemers die aangevraagde geheugen subsidies, moeten wachten op geheugen toewijzingen voor statistisch significante hoeveel heden tijd. Of een grotere accumulatie van werk nemers die geheugen subsidies hebben aangevraagd. Dit is van invloed op de SQL Database prestaties. | Werk nemers die geheugen subsidies hebben aangevraagd, wachten op een statistisch significante hoeveelheid tijd op geheugen toewijzingen. Dit is van invloed op de prestaties van de data base. |
| [Vergren delen](sql-database-intelligent-insights-troubleshoot-performance.md#locking) | Er is een buitensporige database vergrendeling gedetecteerd die van invloed is op de SQL Database prestaties. | Er is een buitensporige database vergrendeling gedetecteerd die invloed heeft op de prestaties van de data base. |
| [Verhoogde MAXDOP](sql-database-intelligent-insights-troubleshoot-performance.md#increased-maxdop) | De maximale mate van parallellisme (MAXDOP) is gewijzigd, wat van invloed is op de efficiëntie van de query-uitvoering. Dit is van invloed op de SQL Database prestaties. | De maximale mate van parallellisme (MAXDOP) is gewijzigd, wat van invloed is op de efficiëntie van de query-uitvoering. Dit is van invloed op de prestaties van de data base. |
| [Pagelatch-conflicten](sql-database-intelligent-insights-troubleshoot-performance.md#pagelatch-contention) | Meerdere threads proberen gelijktijdig toegang te krijgen tot dezelfde gegevens buffer pagina's in het geheugen, wat resulteert in verhoogde wacht tijden en pagelatch-conflicten veroorzaakt. Dit is van invloed op de SQL database de prestaties. | Meerdere threads proberen gelijktijdig toegang te krijgen tot dezelfde gegevens buffer pagina's in het geheugen, wat resulteert in verhoogde wacht tijden en pagelatch-conflicten veroorzaakt. Dit is van invloed op de Data Base op de prestaties. |
| [Ontbrekende index](sql-database-intelligent-insights-troubleshoot-performance.md#missing-index) | Ontbrekende index gevonden die van invloed is op de prestaties van de SQL database. | Ontbrekende index gevonden die van invloed is op de database prestaties. |
| [Nieuwe query](sql-database-intelligent-insights-troubleshoot-performance.md#new-query) | Er is een nieuwe query gedetecteerd die van invloed is op de algehele SQL Database prestaties. | Er is een nieuwe query gedetecteerd die invloed heeft op de prestaties van de data base. |
| [Verhoogde wacht statistieken](sql-database-intelligent-insights-troubleshoot-performance.md#increased-wait-statistic) | Er zijn meer wacht tijden voor de data base gedetecteerd die van invloed zijn op de SQL database prestaties. | Er zijn meer wacht tijden voor de data base gedetecteerd die van invloed zijn op de database prestaties. |
| [TempDB-conflicten](sql-database-intelligent-insights-troubleshoot-performance.md#tempdb-contention) | Meerdere threads proberen toegang te krijgen tot dezelfde TempDB-resource die een knel punt veroorzaakt. Dit is van invloed op de SQL Database prestaties. | Meerdere threads proberen toegang te krijgen tot dezelfde TempDB-resource die een knel punt veroorzaakt. Dit is van invloed op de prestaties van de data base. |
| [DTU-tekort elastische groep](sql-database-intelligent-insights-troubleshoot-performance.md#elastic-pool-dtu-shortage) | Tekort aan beschik bare Edtu's in de elastische pool heeft invloed op de prestaties van SQL Database. | Niet beschikbaar voor het beheerde exemplaar omdat het gebruikmaakt van het vCore-model. |
| [Regressie plannen](sql-database-intelligent-insights-troubleshoot-performance.md#plan-regression) | Er is een nieuw plan of een wijziging in de werk belasting van een bestaand plan gedetecteerd. Dit is van invloed op de SQL Database prestaties. | Er is een nieuw plan of een wijziging in de werk belasting van een bestaand plan gedetecteerd. Dit is van invloed op de prestaties van de data base. |
| [Wijziging in database bereik configuratie waarde](sql-database-intelligent-insights-troubleshoot-performance.md#database-scoped-configuration-value-change) | De configuratie wijziging op de SQL Database is gedetecteerd voor de prestaties van de data base. | Er is een configuratie wijziging voor de data base gedetecteerd die invloed heeft op de prestaties van de data base. |
| [Trage client](sql-database-intelligent-insights-troubleshoot-performance.md#slow-client) | Trage toepassings client kan geen uitvoer van de Data Base gebruiken die snel genoeg is. Dit is van invloed op de SQL Database prestaties. | Trage toepassings client kan geen uitvoer van de Data Base gebruiken die snel genoeg is. Dit is van invloed op de prestaties van de data base. |
| [Prijs categorie downgrade](sql-database-intelligent-insights-troubleshoot-performance.md#pricing-tier-downgrade) | Downgrade-actie voor prijs categorie heeft de beschik bare resources afgenomen. Dit is van invloed op de SQL Database prestaties. | Downgrade-actie voor prijs categorie heeft de beschik bare resources afgenomen. Dit is van invloed op de prestaties van de data base. |

> [!TIP]
> Schakel [Azure SQL database automatisch afstemmen](sql-database-automatic-tuning.md)in voor voortdurende optimalisatie van de prestaties van SQL database. Met deze unieke functie van SQL Database ingebouwde intelligentie wordt uw SQL database voortdurend bewaakt, worden indexen automatisch afgestemd en worden de correcties voor het uitvoerings plan voor query's toegepast.
>

In de volgende sectie worden Detecteer bare prestatie patronen uitvoeriger beschreven.

## <a name="reaching-resource-limits"></a>Bron limieten bereiken

### <a name="what-is-happening"></a>Wat gebeurt er

Dit gedetecteerde prestatie patroon combineert prestatie problemen die betrekking hebben op het bereiken van beschik bare resource limieten, werknemers limieten en sessie limieten. Nadat dit prestatie probleem is gedetecteerd, geeft een beschrijvings veld van het diagnostische logboek aan of het prestatie probleem betrekking heeft op resource-, werk nemer-of sessie limieten.

Resources op SQL Database worden meestal [DTU](sql-database-what-is-a-dtu.md) -of [vCore](sql-database-service-tiers-vcore.md) -bronnen genoemd. Het patroon van het bereiken van resource limieten wordt herkend wanneer de prestatie vermindering van gedetecteerde query's wordt veroorzaakt door het bereiken van een van de gemeten resource limieten.

De resource limieten voor sessies geeft het aantal beschik bare gelijktijdige aanmeldingen aan de SQL database. Dit prestatie patroon wordt herkend wanneer toepassingen die zijn verbonden met de SQL-data base, het aantal beschik bare gelijktijdige aanmeldingen bij de data base hebben bereikt. Als toepassingen proberen meer sessies te gebruiken dan beschikbaar zijn in een Data Base, is dit van invloed op de prestaties van de query.

Het bereiken van werknemers limieten is een specifiek geval van het bereiken van resource limieten, omdat beschik bare werk nemers niet worden meegeteld in het DTU-of vCore-gebruik Het bereiken van de limieten van werk nemers voor een Data Base kan de toename van de resource-specifieke wacht tijden veroorzaken, wat resulteert in de prestaties van query's.

### <a name="troubleshooting"></a>Problemen oplossen

In het diagnostische logboek worden query-hashes uitgevoerd van query's die invloed hebben op de percentages voor prestatie-en Resource verbruik. U kunt deze informatie gebruiken als uitgangs punt voor het optimaliseren van de werk belasting van uw data base. Met name kunt u de query's optimaliseren die van invloed zijn op de prestatie vermindering door indexen toe te voegen. U kunt ook toepassingen met een meer verdeling van de werk belasting optimaliseren. Als u geen workloads kunt verminderen of optimalisaties kunt maken, kunt u overwegen de prijs categorie van uw SQL database-abonnement te verhogen om de hoeveelheid beschik bare resources te verg Roten.

Als u de beschik bare sessie limieten hebt bereikt, kunt u uw toepassingen optimaliseren door het aantal aanmeldingen in de data base te verminderen. Als u het aantal aanmeldingen van uw toepassingen naar de data base niet kunt reduceren, kunt u overwegen de prijs categorie van de data base te verhogen. Of u kunt uw data base in meerdere data bases splitsen en verplaatsen voor een meer evenwichtige taak verdeling.

Zie [de limieten van SQL database maximum aantal aanmeldingen](https://blogs.technet.microsoft.com/latam/20../../how-to-deal-with-the-limits-of-azure-sql-database-maximum-logins/)voor meer suggesties voor het oplossen van sessie limieten. Zie [overzicht van resource limieten op een SQL database server](sql-database-resource-limits-database-server.md) voor informatie over limieten op het niveau van de server en het abonnement.

## <a name="workload-increase"></a>Toename van de workload

### <a name="what-is-happening"></a>Wat gebeurt er

Dit prestatie patroon identificeert problemen als gevolg van een werk belasting die wordt veroorzaakt door het verg Roten of, in een meer ernstige vorm, van een werk Last.

Deze detectie wordt uitgevoerd door een combi natie van verschillende metrische gegevens. De gemeten basis metriek detecteert een toename van de werk belasting vergeleken met de basis lijn voor de werk belasting van het verleden. De andere vorm van detectie is gebaseerd op het meten van een grote toename in actieve worker-threads die groot genoeg zijn om de query prestaties te beïnvloeden.

In de zwaarste vorm kan de werk belasting voortdurend worden gepoold als gevolg van het onvermogen van de SQL database om de werk belasting te verwerken. Het resultaat is een voortdurend groeiende werkbelasting grootte, die de voor waarde workload pooling is. Als gevolg van deze situatie wordt de tijd die de werk belasting wacht op de uitvoering toeneemt. Deze voor waarde vertegenwoordigt een van de meest ernstige prestatie problemen met de data base. Dit probleem wordt gedetecteerd door de toename van het aantal afgebroken werk threads te controleren. 

### <a name="troubleshooting"></a>Problemen oplossen

Het diagnostische logboek voert het aantal query's uit waarvan de uitvoering is verhoogd en de query-hash van de query met de grootste bijdrage aan de werk belasting. U kunt deze informatie gebruiken als uitgangs punt voor het optimaliseren van de werk belasting. De query die is geïdentificeerd als de grootste bijdrager aan de hoeveelheid werk belasting is vooral nuttig als uitgangs punt.

U kunt overwegen de werk belastingen gelijkmatig te verdelen over de data base. Overweeg het optimaliseren van de query die de prestaties beïnvloedt door indexen toe te voegen. U kunt uw werk belasting ook verdelen over meerdere data bases. Als deze oplossingen niet mogelijk zijn, kunt u overwegen de prijs categorie van uw SQL database-abonnement te verhogen om de hoeveelheid beschik bare resources te verg Roten.

## <a name="memory-pressure"></a>Geheugen druk

### <a name="what-is-happening"></a>Wat gebeurt er

Dit prestatie patroon duidt op degradatie van de prestaties van de huidige Data Base, veroorzaakt door geheugen druk, of in meer ernstige vorm een geheugen samenstellende voor waarde, vergeleken met de afgelopen zeven-daagse basis lijn voor de prestaties.

Geheugen druk geeft een prestatie probleem aan waarbij een groot aantal worker-threads geheugen toekenningen aanvraagt in de SQL database. Het hoge volume zorgt voor een hoog geheugen gebruik waarbij de SQL database niet in staat is om het geheugen efficiënt toe te wijzen aan alle werk nemers die deze aanvragen. Een van de meest voorkomende redenen voor dit probleem is gerelateerd aan de hoeveelheid geheugen die beschikbaar is voor de SQL database op één hand. Anderzijds veroorzaakt een toename in de werk belasting de toename in werkthreads en de geheugen belasting.

De meer ernstige vorm van geheugen druk is de voor waarde geheugen opstapelen. Deze voor waarde geeft aan dat een hoger aantal worker-threads geheugen toekenningen aanvraagt dan er query's worden vrijgegeven voor het geheugen. Dit aantal werkthreads dat geheugen subsidies aanvraagt, kan ook voortdurend toenemen (Piling), omdat de SQL database-engine het geheugen niet efficiënt genoeg kan toewijzen om aan de vraag te voldoen. De voor waarde geheugen in stapel staat voor een van de meest ernstige prestatie problemen met de data base.

### <a name="troubleshooting"></a>Problemen oplossen

In het diagnostische logboek wordt de details van het geheugen object opgeslagen met de mede werker (dat wil zeggen, worker-thread), gemarkeerd als de hoogste reden voor hoog geheugen gebruik en relevante tijds tempels. U kunt deze informatie gebruiken als basis voor het oplossen van problemen. 

U kunt query's met betrekking tot de Clerks optimaliseren of verwijderen met het hoogste geheugen gebruik. U kunt er ook voor zorgen dat u geen query's uitvoert op gegevens die u niet wilt gebruiken. Een goede gewoonte is om altijd een WHERE-component in uw query's te gebruiken. Daarnaast raden we u aan niet-geclusterde indexen te maken om de gegevens te zoeken in plaats van deze te scannen.

U kunt de werk belasting ook verminderen door deze te optimaliseren of te verdelen over meerdere data bases. Of u kunt uw werk belasting verdelen over meerdere data bases. Als deze oplossingen niet mogelijk zijn, kunt u overwegen de prijs categorie van uw SQL database-abonnement te verg Roten om de hoeveelheid geheugen bronnen die beschikbaar zijn voor de data base te verg Roten.

Zie [geheugen subsidies Meditation: de verwarrende SQL Server Memory Consumer met veel namen](https://blogs.msdn.microsoft.com/sqlmeditation/20../../memory-meditation-the-mysterious-sql-server-memory-consumer-with-many-names/)voor aanvullende suggesties voor probleem oplossing.

## <a name="locking"></a>Vergren delen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit prestatie patroon duidt op degradatie van de prestaties van de huidige Data Base waarin buitensporige database vergrendeling wordt gedetecteerd vergeleken met de afgelopen zeven-daagse prestatie basislijn. 

In moderne RDBMS is het vergren delen van essentieel belang voor het implementeren van multi thread-systemen waarbij de prestaties worden gemaximaliseerd door meerdere gelijktijdige werk nemers en parallelle database transacties waar mogelijk uit te voeren. Vergren deling in deze context verwijst naar het ingebouwde toegangs mechanisme waarbij slechts één trans actie exclusieve toegang kan krijgen tot de rijen, pagina's, tabellen en bestanden die vereist zijn en niet concurreren met een andere trans actie voor resources. Wanneer de trans actie die de resources voor gebruik heeft vergrendeld, wordt uitgevoerd, wordt de vergren deling van deze resources vrijgegeven, waardoor andere trans acties toegang hebben tot de vereiste resources. Zie [de data base-engine vergren delen](https://msdn.microsoft.com/library/ms190615.aspx)voor meer informatie over vergren delen.

Als trans acties die worden uitgevoerd door de SQL-engine tijdig worden gewacht om toegang te krijgen tot resources die zijn vergrendeld voor gebruik, zorgt deze wacht tijd voor de vertraging van de prestaties van de werk belasting. 

### <a name="troubleshooting"></a>Problemen oplossen

De diagnostische logboeken worden gebruikt voor het vergren delen van details die u als basis kunt gebruiken voor het oplossen van problemen. U kunt de gerapporteerde blokkerende query's analyseren, dat wil zeggen, de query's die de vergrendelings prestaties van het prestatie niveau introduceren en ze verwijderen. In sommige gevallen is het mogelijk dat u de blokkerende query's optimaliseert.

De eenvoudigste en veiligste manier om het probleem te verhelpen is om trans acties kort te houden en de vergren deling van de duurste query's te verminderen. U kunt een grote batch van bewerkingen in kleinere bewerkingen opsplitsen. Een goede gewoonte is het verminderen van de query vergrendeling door de query zo efficiënt mogelijk te maken. Verklein grote scans omdat ze de kans op deadlocks verg Roten en de algehele prestaties van de data base nadelig beïnvloeden. Voor geïdentificeerde query's die vergrendeling veroorzaken, kunt u nieuwe indexen maken of kolommen toevoegen aan de bestaande index om de tabel scans te voor komen. 

Zie voor meer suggesties [blokkerende problemen oplossen die worden veroorzaakt door het vergren delen van escalatie in SQL Server](https://support.microsoft.com/help/323630/how-to-resolve-blocking-problems-that-are-caused-by-lock-escalation-in).

## <a name="increased-maxdop"></a>Verhoogde MAXDOP

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een voor waarde waarin een gekozen query-uitvoerings plan meer is geparallelleerd dan het had. Met de SQL Database query optimalisatie kunnen de prestaties van de werk belasting worden verbeterd door query's parallel uit te voeren om zo mogelijk dingen te versnellen. In sommige gevallen best Eden parallelle werk rollen een query meer tijd op elkaar om de resultaten te synchroniseren en samen te voegen, vergeleken met het uitvoeren van dezelfde query met minder parallelle werk nemers, of zelfs in sommige gevallen ten opzichte van één werk thread.

Het expert systeem analyseert de prestaties van de huidige data base vergeleken met de basislijn periode. Hiermee wordt bepaald of een eerder uitgevoerde query langzamer is dan voorheen, omdat het uitvoerings plan voor de query niet meer wordt gebruikt.

De MAXDOP-server configuratie optie op SQL Database wordt gebruikt om te bepalen hoeveel CPU-kernen kunnen worden gebruikt om dezelfde query parallel uit te voeren. 

### <a name="troubleshooting"></a>Problemen oplossen

Het diagnostische logboek voert query-hashes uit die zijn gerelateerd aan query's waarvoor de duur van de uitvoering is verhoogd omdat ze meer zijn geparallelleerd dan ze zouden moeten hebben. In het logboek worden ook CXP-wacht tijden uitgevoerd. Deze tijd staat voor de tijd dat een enkele thread van de Organizer (thread 0) wordt gewacht voordat alle andere threads worden voltooid voordat de resultaten worden samengevoegd en vooruit worden verplaatst. Daarnaast voert het diagnostische logboek de wacht tijden uit waarop de slechtere query's in de gehele uitvoering wachten. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

Optimaliseer of Vereenvoudig complexe query's eerst. Een goede gewoonte is het opsplitsen van lange batch taken in kleinere versies. Bovendien moet u ervoor zorgen dat u indexen hebt gemaakt voor de ondersteuning van uw query's. U kunt ook hand matig de maximale mate van parallellisme (MAXDOP) afdwingen voor een query die als slechte uitvoering is gemarkeerd. Als u deze bewerking wilt configureren met T-SQL, raadpleegt u [de configuratie optie voor de MAXDOP-server configureren](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-the-max-degree-of-parallelism-server-configuration-option).

Als u de configuratie optie voor de MAXDOP-server instelt op nul (0) als standaard waarde, geeft u aan dat SQL Database alle beschik bare logische CPU-kernen kunt gebruiken voor parallelliseren-threads voor het uitvoeren van één query. Als u MAXDOP instelt op één (1), ziet u dat er slechts één kern kan worden gebruikt voor het uitvoeren van één query. In de praktijk betekent dit dat parallellisme is uitgeschakeld. Afhankelijk van de basis per geval, beschik bare kernen voor de data base en diagnostische logboek gegevens, kunt u de MAXDOP-optie afstemmen op het aantal kernen dat wordt gebruikt voor het uitvoeren van parallelle query's, waardoor het probleem in uw situatie kan worden opgelost.

## <a name="pagelatch-contention"></a>Pagelatch-conflicten

### <a name="what-is-happening"></a>Wat gebeurt er

Dit prestatie patroon geeft de huidige prestaties van de werk belasting van de data base aan vanwege pagelatch-conflicten ten opzichte van de afgelopen zeven dagen basis lijn voor de werk belasting.

Vergren delingen zijn licht gewicht synchronisatie mechanismen die worden gebruikt door SQL Database om multithreading in te scha kelen. Ze garanderen consistentie van in-Memory structuren met inbegrip van indices, Data Pages en andere interne structuren.

Er zijn veel typen hendels beschikbaar op de SQL database. Voor de eenvoud worden buffer latches gebruikt om in-Memory pagina's in de buffer groep te beveiligen. I/o-latches worden gebruikt voor het beveiligen van pagina's die nog niet in de buffer groep zijn geladen. Wanneer gegevens worden geschreven naar of gelezen van een pagina in de buffer groep, moet een werk thread eerst een buffer vergrendeling verkrijgen voor de pagina. Wanneer een werk thread probeert toegang te krijgen tot een pagina die nog niet beschikbaar is in de buffer groep in het geheugen, wordt er een i/o-aanvraag gedaan om de vereiste gegevens van de opslag te laden. Deze reeks gebeurtenissen duidt op een meer ernstige vorm van verminderde prestaties.

Conflicten met de vergren deling op de pagina wordt weer gegeven wanneer meerdere threads gelijktijdig proberen om latches te verkrijgen op dezelfde geheugen structuur, wat een verhoogde wacht tijd oplevert voor het uitvoeren van query's. In het geval van pagelatch i/o-conflicten, wanneer gegevens vanuit de opslag moeten worden geopend, is deze wacht tijd nog groter. Dit kan de prestaties van de werk belasting aanzienlijk beïnvloeden. Pagelatch-conflicten is het meest voorkomende scenario van threads die op elkaar wachten en concurreren voor bronnen op meerdere CPU-systemen.

### <a name="troubleshooting"></a>Problemen oplossen

In het diagnostische logboek worden pagelatch-inhouds gegevens beschreven. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

Omdat een pagelatch een interne controle mechanisme van SQL Database is, wordt automatisch vastgesteld wanneer ze worden gebruikt. Toepassings beslissingen, met inbegrip van schema-ontwerp, kunnen invloed hebben op het gedrag van pagelatch als gevolg van het deterministische gedrag van latches.

Eén methode voor het afhandelen van vergren deling is het vervangen van een sequentiële index sleutel met een niet-opeenvolgende sleutel om toevoegingen gelijkmatig te verdelen over een index bereik. Normaal gesp roken wordt de werk belasting proportioneel gedistribueerd met een voorloop kolom in de index. Een andere methode om te overwegen is het partitioneren van tabellen. Het maken van een schema voor hash-partitionering met een berekende kolom in een gepartitioneerde tabel is een gemeen schappelijke benadering voor het beperken van buitensporige vergrendelings conflicten. In het geval van pagelatch i/o-conflicten helpt de inleiding tot het oplossen van dit prestatie probleem. 

Zie voor meer informatie [problemen met Latches diagnosticeren en oplossen op SQL Server](https://download.microsoft.com/download/B/9/E/B9EDF2CD-1DBF-4954-B81E-82522880A2DC/SQLServerLatchContention.pdf) (PDF-down load).

## <a name="missing-index"></a>Ontbrekende index

### <a name="what-is-happening"></a>Wat gebeurt er

Dit prestatie patroon geeft de huidige prestaties van de werk belasting van de data base aan ten opzichte van de afgelopen periode van zeven dagen als gevolg van een ontbrekende index.

Een index wordt gebruikt om de prestaties van query's te versnellen. Het biedt snelle toegang tot tabel gegevens door het aantal gegevensset-pagina's te verminderen dat moet worden bezocht of gescand.

Specifieke query's die de prestaties verslechteren, worden geïdentificeerd via deze detectie waarvoor het maken van indexen gunstig zou zijn voor de prestaties.

### <a name="troubleshooting"></a>Problemen oplossen

Het diagnostische logboek voert query-hashes uit voor de query's die zijn geïdentificeerd om de prestaties van de werk belasting te beïnvloeden. U kunt indexen maken voor deze query's. U kunt deze query's ook optimaliseren of verwijderen als ze niet vereist zijn. U kunt het beste geen query uitvoeren op gegevens die u niet gebruikt.

> [!TIP]
> Wist u dat SQL Database ingebouwde intelligentie automatisch de best presterende indexen voor uw data bases kan beheren?
>
> Voor een voortdurende optimalisatie van SQL Database, raden we u aan [SQL database automatisch afstemmen](sql-database-automatic-tuning.md)in te scha kelen. Met deze unieke functie van SQL Database ingebouwde intelligentie wordt uw SQL database voortdurend bewaakt en worden automatisch indexen voor uw data bases gemaakt.
>

## <a name="new-query"></a>Nieuwe query

### <a name="what-is-happening"></a>Wat gebeurt er

Dit prestatie patroon geeft aan dat er een nieuwe query wordt gedetecteerd die slecht presteert en invloed heeft op de prestaties van de werk belasting vergeleken met de basis lijn voor zeven dagen.

Het schrijven van een goed uitgevoerde query kan soms een uitdagende taak zijn. Zie [SQL-Query's schrijven](https://msdn.microsoft.com/library/bb264565.aspx)voor meer informatie over het schrijven van query's. Zie [query afstemmen](https://msdn.microsoft.com/library/ms176005.aspx)om de bestaande query prestaties te optimaliseren.

### <a name="troubleshooting"></a>Problemen oplossen

Het diagnostische logboek voert informatie uit tot twee nieuwe meest CPU-verbruikte query's, inclusief de query-hashes. Omdat de gedetecteerde query invloed heeft op de prestaties van de werk belasting, kunt u uw query optimaliseren. Het is raadzaam om alleen de gegevens op te halen die u wilt gebruiken. U wordt ook aangeraden query's te gebruiken met een WHERE-component. We raden u ook aan complexe query's te vereenvoudigen en ze in kleinere query's te verstoren. Een andere goede gewoonte is het opsplitsen van grote batch query's in kleinere batch-query's. Het introduceren van indexen voor nieuwe query's is doorgaans een goed idee om dit prestatie probleem te verhelpen.

Overweeg het gebruik van [Azure SQL Database query Performance Insight](sql-database-query-performance.md).

## <a name="increased-wait-statistic"></a>Verhoogde wacht statistieken

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een prestatie vermindering van de werk belasting waarbij slechtere query's worden geïdentificeerd ten opzichte van de afgelopen 7-daagse basis lijn voor de werk belasting.

In dit geval kan het systeem de slechtere query's niet classificeren onder andere standaard gedetecteerde prestatie categorieën, maar is de wacht tijd die verantwoordelijk is voor de regressie, gedetecteerd. Daarom worden ze beschouwd als query's met *verhoogde wacht statistieken*, waarbij de wacht tijd die verantwoordelijk is voor de regressie ook wordt weer gegeven. 

### <a name="troubleshooting"></a>Problemen oplossen

Het diagnostische logboek voert informatie uit over verhoogde wacht tijd en query-hashes van de betrokken query's.

Omdat het systeem de hoofd oorzaak voor de query's die niet goed worden uitgevoerd niet kan identificeren, is de diagnostische informatie een goed uitgangs punt voor hand matige probleem oplossing. U kunt de prestaties van deze query's optimaliseren. Het is een goed idee om alleen de gegevens op te halen die u nodig hebt en om complexe query's te vereenvoudigen en te onderverdelen in kleinere bestanden. 

Zie [query tuning](https://msdn.microsoft.com/library/ms176005.aspx)(Engelstalig) voor meer informatie over het optimaliseren van query prestaties.

## <a name="tempdb-contention"></a>TempDB-conflicten

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een prestatie voorwaarde voor de Data Base waarin een bottleneck van threads die toegang proberen te krijgen tot tempDB-bronnen bestaat. (Aan deze voor waarde is geen IO gerelateerd.) Het typische scenario voor dit prestatie probleem is honderden gelijktijdige query's die allemaal maken, gebruiken en verwijderen van kleine tempDB-tabellen. Het systeem heeft gedetecteerd dat het aantal gelijktijdige query's met dezelfde tempDB-tabellen is verhoogd met voldoende statistische significantie om de database prestaties te beïnvloeden ten opzichte van de afgelopen zeven-daagse prestatie basislijn.

### <a name="troubleshooting"></a>Problemen oplossen

In het diagnostische logboek worden de details van tempDB-conflicten in de geheugen uitvoer uitgevoerd. U kunt de informatie als uitgangs punt gebruiken voor het oplossen van problemen. Er zijn twee dingen die u kunt uitvoeren om dit soort conflicten op te lossen en de door Voer van de totale werk belasting te verg Roten: u kunt stoppen met het gebruik van de tijdelijke tabellen. U kunt ook tabellen gebruiken die zijn geoptimaliseerd voor geheugen. 

Zie [Inleiding tot tabellen die zijn geoptimaliseerd voor geheugen](https://docs.microsoft.com/sql/relational-databases/in-memory-oltp/introduction-to-memory-optimized-tables)voor meer informatie. 

## <a name="elastic-pool-dtu-shortage"></a>DTU-tekort elastische groep

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een degradatie van de prestaties van de huidige data base-werk belasting vergeleken met de afgelopen zeven-daagse basis lijn. Dit wordt veroorzaakt door het tekort aan beschik bare Dtu's in de elastische pool van uw abonnement. 

Resources op SQL Database worden meestal [DTU-resources](sql-database-purchase-models.md#dtu-based-purchasing-model)genoemd, die bestaan uit een overvloei meting van de CPU-en IO-bronnen (data-en transactie logboek-io). [Elastische Azure-pool bronnen](sql-database-elastic-pool.md) worden gebruikt als een groep beschik bare eDTU-resources die worden gedeeld tussen meerdere data bases voor schaal doeleinden. Wanneer beschik bare eDTU-resources in uw elastische pool niet voldoende groot zijn voor de ondersteuning van alle data bases in de pool, wordt het prestatie probleem van een elastisch groep DTU-tekort gedetecteerd door het systeem.

### <a name="troubleshooting"></a>Problemen oplossen

In het diagnostische logboek worden gegevens over de elastische pool weer gegeven, de meestgebruikte data bases voor DTU en een percentage van de DTU van de pool die wordt gebruikt door de beste data base.

Omdat deze prestatie voorwaarde betrekking heeft op meerdere data bases die gebruikmaken van dezelfde pool Edtu's in de elastische pool, zijn de stappen voor probleem oplossing gericht op de topdata bases die het beste DTU gebruiken. U kunt de werk belasting verlagen op de meest gebruikte data bases, inclusief optimalisatie van de best presterende query's op deze data bases. U kunt er ook voor zorgen dat u geen query uitvoert op gegevens die u niet gebruikt. Een andere benadering is het optimaliseren van toepassingen met behulp van de meest gebruikte data bases voor DTU en de werk belasting opnieuw verdelen over meerdere data bases.

Als het verminderen en optimaliseren van de huidige werk belasting op uw top DTU-data bases niet mogelijk is, kunt u de prijs categorie voor de elastische groep verg Roten. Deze toename resulteert in de toename van de beschik bare Dtu's in de elastische pool.

## <a name="plan-regression"></a>Regressie plannen

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een voor waarde waarin SQL Database gebruikmaakt van een suboptimaal query-uitvoerings plan. Het suboptimaalste plan zorgt meestal voor meer query uitvoering, wat leidt tot langere wacht tijden voor de huidige en andere query's.

Het SQL database bepaalt het uitvoerings plan voor query's met de minste kosten voor het uitvoeren van query's. Naarmate het type query's en werk belastingen veranderen, zijn de bestaande plannen soms niet langer efficiënt of zijn er misschien SQL Database geen goede beoordeling gemaakt. Als gevolg van een correctie kunnen query-uitvoerings plannen hand matig worden geforceerd.

Dit gedetecteerde prestatie patroon combineert drie verschillende gevallen van plannings regressie: nieuwe regressie, oud plan regressie en bestaande plannen die de werk belasting wijzigen. Het specifieke type plannings regressie dat is opgetreden, is te zien in de eigenschap *Details* van het diagnose logboek.

De nieuwe regressie voorwaarde van het plan verwijst naar een status waarin SQL Database begint met het uitvoeren van een nieuw uitvoerings plan voor query's dat niet zo efficiënt is als het oude plan. De oude voor waarde voor het plannen van een plan verwijst naar de status wanneer SQL Database overschakelt van het gebruik van een nieuwe, efficiëntere planning naar het oude plan, wat niet zo efficiënt is als het nieuwe plan. De bestaande plannings regressie van de werk belasting heeft betrekking op de staat waarin de oude en de nieuwe plannen voortdurend wisselen, met het saldo voor het slecht uitgevoerde plan.

Zie [Wat is regressie plannen in SQL Server?](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../what-is-plan-regression-in-sql-server/)voor meer informatie over het plannen van regressies. 

### <a name="troubleshooting"></a>Problemen oplossen

In het diagnostische logboek worden de query-hashes, een goede plan-ID, een onjuiste plan-ID en query-Id's uitgevoerd. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

U kunt analyseren welk plan het beste presteert voor uw specifieke query's die u kunt identificeren met de opgegeven query-hashes. Nadat u hebt vastgesteld welk abonnement beter werkt voor uw query's, kunt u dit hand matig afdwingen. 

Zie [meer informatie over het voor komen van plannings regressies in SQL Server](https://blogs.msdn.microsoft.com/sqlserverstorageengine/20../../you-shall-not-regress-how-sql-server-2017-prevents-plan-regressions/).

> [!TIP]
> Wist u dat SQL Database ingebouwde intelligentie automatisch de best presterende uitvoerings plannen voor query's voor uw data bases kan beheren?
>
> Voor een voortdurende optimalisatie van SQL Database, raden we u aan [SQL database automatisch afstemmen](sql-database-automatic-tuning.md)in te scha kelen. Met deze unieke functie van SQL Database ingebouwde intelligentie wordt uw SQL database voortdurend bewaakt en worden de best presterende uitvoerings plannen voor query's voor uw data bases automatisch afgestemd.
>

## <a name="database-scoped-configuration-value-change"></a>Wijziging in database bereik configuratie waarde

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een voor waarde waarin een wijziging in de database bereik configuratie ertoe leidt dat prestatie regressie wordt gedetecteerd vergeleken met het afgelopen data base-werk van zeven dagen. Dit patroon geeft aan dat een recente wijziging die is aangebracht in de database bereik configuratie niet gunstig lijkt te zijn voor uw database prestaties.

Wijzigingen in het database bereik configuratie kunnen worden ingesteld voor elke afzonderlijke data base. Deze configuratie wordt per geval gebruikt voor het optimaliseren van de afzonderlijke prestaties van uw data base. De volgende opties kunnen worden geconfigureerd voor elke afzonderlijke Data Base: MAXDOP, LEGACY_CARDINALITY_ESTIMATION, PARAMETER_SNIFFING, QUERY_OPTIMIZER_HOTFIXES en CLEAR PROCEDURE_CACHE.

### <a name="troubleshooting"></a>Problemen oplossen

In het logboek voor diagnostische gegevens zijn wijzigingen in de configuratie van de data base vastgelegd die onlangs zijn aangebracht en die de prestaties verslechteren ten opzichte van het vorige werkings gedrag van zeven dagen. U kunt de wijzigingen in de configuratie herstellen naar de vorige waarden. U kunt ook de waarde op waarde afstemmen tot het gewenste prestatie niveau is bereikt. U kunt configuratie waarden voor database bereik kopiëren uit een vergelijk bare data base met voldoende prestaties. Als u de prestaties niet kunt oplossen, keert u terug naar de standaard SQL Database standaard waarden en probeert u de basis lijn te verfijnen.

Zie [ALTER data base-scoped Configuration (Transact-SQL)](https://msdn.microsoft.com/library/mt629158.aspx)voor meer informatie over het optimaliseren van de database bereik configuratie en de T-SQL-syntaxis bij het wijzigen van de configuratie.

## <a name="slow-client"></a>Trage client

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een voor waarde waarin de client die de SQL database gebruikt, de uitvoer van de data base niet zo snel kan verbruiken als de data base de resultaten verzendt. Omdat SQL Database geen resultaten van de uitgevoerde query's in een buffer opslaat, wordt deze langzamer en wordt gewacht tot de client de verzonden query-uitvoer heeft verbruikt voordat u doorgaat. Dit probleem kan ook worden veroorzaakt door een netwerk dat niet voldoende snel genoeg is voor het verzenden van uitvoer van de SQL database naar de Consumer-client.

Deze voor waarde wordt alleen gegenereerd als er een prestatie regressie wordt gedetecteerd vergeleken met het vorige data base-werkproces van zeven dagen. Dit prestatie probleem wordt alleen gedetecteerd als er sprake is van een statistisch significante prestatie vermindering in vergelijking met het vorige prestatie gedrag.

### <a name="troubleshooting"></a>Problemen oplossen

Dit Detecteer bare prestatie patroon duidt op een client-side voor waarde. Het oplossen van problemen is vereist voor de toepassing aan de client zijde of het netwerk aan de client zijde. In het diagnostische logboek worden de query-hashes en wacht tijden die in de afgelopen twee uur voor de client moeten worden gebruikt, gewacht. U kunt deze informatie gebruiken als basis voor het oplossen van problemen.

U kunt de prestaties van uw toepassing optimaliseren voor gebruik van deze query's. U kunt ook mogelijke problemen met de netwerk latentie overwegen. Omdat het probleem met de prestaties van de prestatie vermindering is gebaseerd op de wijziging in de afgelopen zeven-daagse prestatie basislijn, kunt u onderzoeken of recente wijzigingen in de toepassing of de netwerk voorwaarde het gevolg zijn van deze prestatie regressie gebeurtenis. 

## <a name="pricing-tier-downgrade"></a>Prijs categorie downgrade

### <a name="what-is-happening"></a>Wat gebeurt er

Dit Detecteer bare prestatie patroon duidt op een voor waarde waarin de prijs categorie van uw SQL Database abonnement is gedowngraded. Als gevolg van het verminderen van resources (Dtu's) die beschikbaar zijn voor de data base, heeft het systeem een daling van de huidige database prestaties gedetecteerd vergeleken met de afgelopen zeven dagen basis lijn.

Daarnaast kan er sprake zijn van een voor waarde waarin de prijs categorie van uw SQL Database abonnement is gedowngraded en de upgrade naar een hogere laag binnen een korte periode is uitgevoerd. Detectie van deze tijdelijke prestatie vermindering wordt in het gedeelte Details van het diagnostische logboek gegenereerd als een prijs categorie downgrade en upgrade.

### <a name="troubleshooting"></a>Problemen oplossen

Als u de prijs categorie hebt gereduceerd en daarom de Dtu's beschikbaar is voor SQL Database en u tevreden bent met de prestaties, hoeft u niets te doen. Als u uw prijs categorie hebt gereduceerd en u niet tevreden bent met uw SQL database prestaties, vermindert u de werk belasting van uw data base of neemt u de prijs categorie op een hoger niveau.

## <a name="recommended-troubleshooting-flow"></a>Aanbevolen stroom voor probleem oplossing

 Volg het stroom diagram voor een aanbevolen benadering om prestatie problemen op te lossen met behulp van Intelligent Insights.

Ga naar Azure SQL-analyse om Intelligent Insights via de Azure Portal. Probeer de waarschuwing voor binnenkomende prestaties te vinden en selecteer deze. Bepaal wat er gebeurt op de pagina detecties. Bekijk de verstrekte analyse van de hoofd oorzaak van het probleem, query tekst, trends in query tijd en incident evolutie. Probeer het probleem op te lossen met behulp van de Intelligent Insights aanbeveling om het prestatie probleem te verhelpen. 

[stroom diagram voor het oplossen van problemen ![](./media/sql-database-intelligent-insights/intelligent-insights-troubleshooting-flowchart.png)](https://github.com/Microsoft/sql-server-samples/blob/master/samples/features/intelligent-insight/Troubleshoot%20Azure%20SQL%20Database%20performance%20issues%20using%20Intelligent%20Insight.pdf)

> [!TIP]
> Selecteer het stroom diagram om een PDF-versie te downloaden.

Intelligent Insights is doorgaans één uur tijd nodig voor het uitvoeren van de analyse van de hoofd oorzaak van het prestatie probleem. Als u het probleem niet kunt vinden in Intelligent Insights en het essentieel voor u is, gebruikt u het query archief om de hoofd oorzaak van het prestatie probleem hand matig te identificeren. (Doorgaans zijn deze problemen minder dan een uur oud.) Zie [prestaties bewaken met behulp van de query Store](https://docs.microsoft.com/sql/relational-databases/performance/monitoring-performance-by-using-the-query-store)voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
- Leer [intelligent Insights](sql-database-intelligent-insights.md) -concepten.
- Gebruik het [Intelligent Insights Azure SQL database prestatie logboek voor diagnostische gegevens](sql-database-intelligent-insights-use-diagnostics-log.md).
- Bewaak [Azure SQL database met behulp van Azure SQL-analyse](https://docs.microsoft.com/azure/log-analytics/log-analytics-azure-sql).
- Meer informatie over [het verzamelen en gebruiken van logboek gegevens van uw Azure-resources](../azure-monitor/platform/resource-logs-overview.md).
