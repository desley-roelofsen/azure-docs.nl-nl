---
title: Serverless
description: In dit artikel wordt de nieuwe serverloze Compute-laag beschreven en vergelijkt deze met de bestaande ingerichte Compute-laag
services: sql-database
ms.service: sql-database
ms.subservice: service
ms.custom: test
ms.devlang: ''
ms.topic: conceptual
author: oslake
ms.author: moslake
ms.reviewer: sstein, carlrab
ms.date: 12/03/2019
ms.openlocfilehash: e90bff7548be5f469ebbcdc21dd9b93dc887a30e
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74931958"
---
# <a name="azure-sql-database-serverless"></a>Azure SQL Database - Serverloos

Azure SQL Database serverloos is een compute-laag voor afzonderlijke data bases waarmee de berekening automatisch wordt geschaald op basis van de vraag van de werk belasting en de facturen voor de hoeveelheid reken kracht die per seconde wordt gebruikt. De compute-laag zonder server onderbreekt ook automatisch data bases tijdens inactieve Peri Oden wanneer alleen opslag wordt gefactureerd en automatisch data bases hervat wanneer de activiteit wordt geretourneerd.

## <a name="serverless-compute-tier"></a>Serverloze compute-laag

De compute-laag zonder server voor één data base is vastgelegd door een bereik voor automatisch schalen van berekeningen en een vertraging van automatische onderbrekingen.  De configuratie van deze para meters vormen de prestaties van de data base en reken kosten.

![facturering zonder server](./media/sql-database-serverless/serverless-billing.png)

### <a name="performance-configuration"></a>Configuratie van prestaties

- De **minimale vCores** -en **maximum vCores** zijn Configureer bare para meters die het bereik van berekenings capaciteit bepalen dat voor de data base beschikbaar is. Geheugen-en i/o-limieten zijn evenredig met het opgegeven vCore-bereik.  
- De **vertraging voor automatische onderbrekingen** is een Configureer bare para meter waarmee de periode wordt gedefinieerd die de data base inactief moet zijn voordat deze automatisch wordt onderbroken. De data base wordt automatisch hervat wanneer de volgende aanmelding of andere activiteit plaatsvindt.  Het is ook mogelijk om autopauzes uit te scha kelen.

### <a name="cost"></a>Kosten

- De kosten voor een serverloze Data Base zijn de som van de reken kosten en opslag kosten.
- Wanneer het reken gebruik tussen de minimale en maximale limieten is geconfigureerd, zijn de reken kosten gebaseerd op vCore en het gebruikte geheugen.
- Wanneer het reken gebruik lager is dan de limieten die zijn geconfigureerd, zijn de reken kosten gebaseerd op de minimale vCores en het minimale geheugen dat is geconfigureerd.
- Wanneer de data base wordt onderbroken, zijn de reken kosten nul en worden er alleen opslag kosten in rekening gebracht.
- De opslag kosten worden bepaald op dezelfde manier als in de ingerichte Compute-laag.

Zie [facturering](sql-database-serverless.md#billing)voor meer informatie over de kosten.

## <a name="scenarios"></a>Scenario's

Serverloze is prijs-prestatie geoptimaliseerd voor afzonderlijke data bases met periodieke, onvoorspelbare gebruiks patronen die enige vertraging kunnen ondervinden bij het opwarmen van het aantal keer dat ze niet actief zijn. De ingerichte Compute-laag daarentegen heeft de prijs prestaties geoptimaliseerd voor afzonderlijke data bases of meerdere data bases in elastische Pools met een hoger gemiddeld gebruik dat geen vertraging kan ondervinden bij het opwarmen van berekeningen.

### <a name="scenarios-well-suited-for-serverless-compute"></a>Scenario's goed geschikt voor serverloze compute

- Afzonderlijke data bases met periodieke, onvoorspelbare gebruiks patronen tussen Peri Oden van inactiviteit en een lager gemiddelde reken gebruik gedurende een periode.
- Afzonderlijke data bases in de ingerichte Compute-laag die vaak worden geschaald en klanten die de voor keur geven aan het opnieuw schalen van berekeningen naar de service.
- Nieuwe afzonderlijke data bases zonder gebruiks geschiedenis waarbij de reken grootte moeilijk is of niet kan worden geschat vóór de implementatie in SQL Database.

### <a name="scenarios-well-suited-for-provisioned-compute"></a>Scenario's zijn goed geschikt voor de ingerichte reken kracht

- Eén data base met meer reguliere, voorspel bare gebruiks patronen en een hoger gemiddelde reken gebruik gedurende een bepaalde periode.
- Data bases die geen prestatie verhoudingen kunnen verdragen die voortkomen uit meer frequente geheugen inkortingen of vertraging bij autohervatten vanuit een onderbroken status.
- Meerdere data bases met periodieke, onvoorspelbare gebruiks patronen die kunnen worden geconsolideerd in elastische Pools voor betere optimalisatie van de prijs prestaties.

## <a name="comparison-with-provisioned-compute-tier"></a>Vergelijking met ingerichte Compute-laag

De volgende tabel bevat een overzicht van de verschillen tussen de serverloze Compute-laag en de ingerichte Compute-laag:

| | **Serverloze compute** | **Ingerichte compute** |
|:---|:---|:---|
|**Database gebruiks patroon**| Onregelmatige, onvoorspelbaar gebruik met minder gemiddelde reken capaciteit gedurende een bepaalde periode. |  Meer reguliere gebruiks patronen met een hoger gemiddelde reken gebruik gedurende een bepaalde periode of meerdere data bases met elastische Pools.|
| **Prestatie beheer-inspanning** |Lager|Hoger|
|**Berekening schalen**|Automatisch|Handmatig|
|**Reactie tijd van berekeningen**|Lager dan inactieve Peri Oden|Direct|
|**Granulatie facturering**|Per seconde|Per uur|

## <a name="purchasing-model-and-service-tier"></a>Aankoop model en servicelaag

SQL Database serverloze wordt momenteel alleen ondersteund in de laag Algemeen op generatie 5 in het vCore-aankoop model.

## <a name="autoscaling"></a>Automatisch schalen

### <a name="scaling-responsiveness"></a>Reactie tijd van schalen

Over het algemeen worden serverloze data bases uitgevoerd op een computer met voldoende capaciteit om te voldoen aan de vraag naar resources zonder onderbreking voor een hoeveelheid berekenings aanvragen binnen de limieten die zijn ingesteld met de waarde Max vCores. In sommige gevallen treedt taak verdeling automatisch op als de computer binnen een paar minuten niet in staat is om te voldoen aan de vraag van de resource. Als de vraag van de resource bijvoorbeeld 4 vCores is, maar slechts twee vCores beschikbaar is, kan het tot een paar minuten duren voordat 4 vCores worden weer gegeven. De data base blijft online tijdens taak verdeling, met uitzonde ring van een korte periode aan het einde van de bewerking wanneer verbindingen worden verwijderd.

### <a name="memory-management"></a>Geheugen beheer

Geheugen voor serverloze data bases is vaker vrijgemaakt dan voor ingerichte Compute-data bases. Dit is belang rijk voor het beheren van de kosten in serverloze en kan invloed hebben op de prestaties.

#### <a name="cache-reclamation"></a>Cache terugwinning

In tegens telling tot provisioned Compute-data bases, wordt het geheugen van de SQL-cache vrijgemaakt van een serverloze data base wanneer het CPU-of cache gebruik laag is.

- Cache gebruik wordt beschouwd als laag als de totale grootte van de meest recent gebruikte cache vermeldingen voor een bepaalde tijd onder een drempel waarde valt.
- Wanneer het inwinnen van de cache wordt geactiveerd, wordt de grootte van de doel cache oplopend verhoogd naar een fractie van de vorige grootte, en wordt er alleen een vrijmaken als het gebruik laag blijft.
- Wanneer de cache wordt verwijderd, is het beleid voor het selecteren van cache vermeldingen voor het verwijderen hetzelfde selectie beleid als voor provisioned Compute data bases wanneer de geheugen belasting hoog is.
- De cache grootte wordt nooit kleiner dan de minimale geheugen limiet, zoals gedefinieerd door de minimale vCores die kan worden geconfigureerd.

In zowel serverloze als ingerichte Compute-data bases kunnen cache vermeldingen worden verwijderd als alle beschik bare geheugen wordt gebruikt.

#### <a name="cache-hydration"></a>Cache Hydration

De SQL-cache groeit naarmate gegevens op dezelfde manier worden opgehaald van de schijf en met dezelfde snelheid als voor ingerichte data bases. Wanneer de data base bezet is, mag het cache geheugen groter worden dan de Maxi maal toegestane geheugen limiet.

## <a name="autopausing-and-autoresuming"></a>Autopauzeering en autohervatten

### <a name="autopausing"></a>Autopauzeren

Automatische onderbreking wordt geactiveerd als aan de duur van de automatische onderbreking de volgende voor waarden van toepassing zijn:

- Aantal sessies = 0
- CPU = 0 voor de werk belasting van de gebruiker die wordt uitgevoerd in de gebruikers groep

Er is een optie voor het uitschakelen van de functie voor het indien gewenst van AutoActiveren.

De volgende functies bieden geen ondersteuning voor het autopauzeren.  Dat wil zeggen, als een van de volgende functies wordt gebruikt, de data base online blijft, ongeacht de duur van de data base-inactiviteit:

- Geo-replicatie (actieve groepen met geo-replicatie en automatische failover).
- Lange termijn retentie van back-ups (LTR).
- De gesynchroniseerde data base die wordt gebruikt in SQL Data Sync.  In tegens telling tot synchronisatie databases ondersteunen de data bases van hubs en leden automatische onderbrekingen.
- De taak database die wordt gebruikt in elastische taken.

Autopauzeren wordt tijdelijk voor komen tijdens de implementatie van sommige service-updates waarvoor de data base online is.  In dergelijke gevallen wordt automatisch onderbreken opnieuw toegestaan zodra de service-update is voltooid.

### <a name="autoresuming"></a>Autohervatten

Autohervatten wordt geactiveerd als een van de volgende voor waarden op elk moment waar is:

|Functie|Trigger voor automatisch hervatten|
|---|---|
|Verificatie en autorisatie|Aanmelden|
|Detectie van bedreigingen|Instellingen voor detectie van bedreigingen in-of uitschakelen op Data Base-of server niveau.<br>Instellingen voor detectie van bedreigingen wijzigen op Data Base-of server niveau.|
|Gegevensdetectie en -classificatie|Toevoegen, wijzigen, verwijderen of weer geven van gevoeligheids labels|
|Controle|Controle records weer geven.<br>Controle beleid bijwerken of weer geven.|
|Gegevensmaskering|Regels voor gegevens maskering toevoegen, wijzigen, verwijderen of weer geven|
|Transparante dataversleuteling|Status of status van transparante gegevens versleuteling weer geven|
|Gegevens Archief voor query (prestaties)|Query Store-instellingen wijzigen of weer geven|
|Autotuning|Toepassing en verificatie van aanbevelingen voor automatische afstemming, zoals automatisch indexeren|
|Data base kopiëren|Maak een Data Base als kopie.<br>Exporteren naar een BACPAC-bestand.|
|SQL-gegevens synchronisatie|Synchronisatie tussen de data bases van de hub en het lid die worden uitgevoerd op een configureerbaar schema of die hand matig worden uitgevoerd|
|Bepaalde data base-meta gegevens wijzigen|Nieuwe data base-Tags toevoegen.<br>De maximale vCores, de minimale vCores of de vertraging voor autopause wijzigen.|
|SQL Server Management Studio (SSMS)|Als u SSMS-versies gebruikt die ouder zijn dan 18,1 en een nieuw query venster opent voor een Data Base op de server, wordt automatisch onderbroken Data Base op dezelfde server hervat. Dit gedrag treedt niet op als u SSMS-versie 18,1 of hoger gebruikt.|

Autohervatten wordt ook geactiveerd tijdens de implementatie van sommige service-updates waarvoor de data base online moet zijn.

### <a name="connectivity"></a>Connectiviteit

Als een serverloze data base wordt onderbroken, wordt de data base door de eerste aanmelding hervat en wordt een fout geretourneerd die aangeeft dat de data base niet beschikbaar is met fout code 40613. Zodra de data base is hervat, moet de aanmelding opnieuw worden geprobeerd om verbinding te maken. Data base-clients met verbindings poging logica mogen niet worden gewijzigd.

### <a name="latency"></a>Latentie

De latentie voor automatisch hervatten en automatisch onderbreken van een serverloze data base is doorgaans 1 minuut tot automatisch hervatten en 1-10 minuten om automatisch te onderbreken.

## <a name="onboarding-into-serverless-compute-tier"></a>Onboarding in Compute-laag zonder server

Het maken van een nieuwe data base of het verplaatsen van een bestaande Data Base naar een serverloze Compute-laag volgt hetzelfde patroon als het maken van een nieuwe data base in een ingerichte Compute-laag en omvat de volgende twee stappen.

1. Geef de naam van de service doelstelling op. De service doelstelling schrijft de service tier, het genereren van hardware en het maximale aantal vCores. In de volgende tabel worden de opties voor service doelstelling weer gegeven:

   |Naam van service doelstelling|Serviceniveau|Hardware genereren|Maximum aantal vCores|
   |---|---|---|---|
   |GP_S_Gen5_1|Algemeen doel|Gen5|1|
   |GP_S_Gen5_2|Algemeen doel|Gen5|2|
   |GP_S_Gen5_4|Algemeen doel|Gen5|4|
   |GP_S_Gen5_6|Algemeen doel|Gen5|6|
   |GP_S_Gen5_8|Algemeen doel|Gen5|8|
   |GP_S_Gen5_10|Algemeen doel|Gen5|10|
   |GP_S_Gen5_12|Algemeen doel|Gen5|12|
   |GP_S_Gen5_14|Algemeen doel|Gen5|14|
   |GP_S_Gen5_16|Algemeen doel|Gen5|16|

2. Geef desgewenst de minimale vCores en de vertraging voor autopause op om de standaard waarden te wijzigen. De volgende tabel bevat de beschik bare waarden voor deze para meters.

   |Parameter|Opties voor waarden|Standaardwaarde|
   |---|---|---|---|
   |Min vCores|Is afhankelijk van het maximum aantal geconfigureerde vCores-Zie [resource limieten](sql-database-vcore-resource-limits-single-databases.md#general-purpose---serverless-compute---gen5).|0,5 vCores|
   |Vertraging van autopause|Minimum: 60 minuten (1 uur)<br>Maximum: 10080 minuten (7 dagen)<br>Aantal stappen: 60 minuten<br>Autopause uitschakelen:-1|60 minuten|


### <a name="create-new-database-in-serverless-compute-tier"></a>Nieuwe data base maken in serverloze Compute-laag 

#### <a name="use-azure-portal"></a>Azure Portal gebruiken

Zie [Quick Start: een enkele data base maken in Azure SQL database met behulp van de Azure Portal](sql-database-single-database-get-started.md).

#### <a name="use-powershell"></a>PowerShell gebruiken

In het volgende voor beeld wordt een nieuwe data base gemaakt in de compute-laag zonder server.  In dit voor beeld wordt expliciet het minimale vCores, het maximum aantal vCores en de vertraging voor autopause opgegeven.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
New-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
  -ComputeModel Serverless -Edition GeneralPurpose -ComputeGeneration Gen5 `
  -MinVcore 0.5 -MaxVcore 2 -AutoPauseDelayInMinutes 720
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```powershell
az sql db create -g $resourceGroupName -s $serverName -n $databaseName `
  -e GeneralPurpose -f Gen5 -min-capacity 0.5 -c 2 --compute-model Serverless --auto-pause-delay 720
```

* * *

#### <a name="use-transact-sql-t-sql"></a>Transact-SQL (T-SQL) gebruiken

In het volgende voor beeld wordt een nieuwe data base gemaakt in de compute-laag zonder server.

```sql
CREATE DATABASE testdb
( EDITION = 'GeneralPurpose', SERVICE_OBJECTIVE = 'GP_S_Gen5_1' ) ;
```

Zie [Create Data Base](/sql/t-sql/statements/create-database-transact-sql?view=azuresqldb-current)(Engelstalig) voor meer informatie.  

### <a name="move-database-from-provisioned-compute-tier-into-serverless-compute-tier"></a>Data base verplaatsen van ingerichte Compute-laag naar serverloze Compute-laag

#### <a name="use-powershell"></a>PowerShell gebruiken

In het volgende voor beeld wordt een Data Base van de ingerichte Compute-laag verplaatst naar de serverloze Compute-laag. In dit voor beeld wordt expliciet het minimale vCores, het maximum aantal vCores en de vertraging voor autopause opgegeven.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
Set-AzSqlDatabase -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName `
  -Edition GeneralPurpose -ComputeModel Serverless -ComputeGeneration Gen5 `
  -MinVcore 1 -MaxVcore 4 -AutoPauseDelayInMinutes 1440
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```powershell
az sql db update -g $resourceGroupName -s $serverName -n $databaseName `
  --edition GeneralPurpose --min-capacity 1 --capacity 4 --family Gen5 --compute-model Serverless --auto-pause-delay 1440
```

* * *

#### <a name="use-transact-sql-t-sql"></a>Transact-SQL (T-SQL) gebruiken

In het volgende voor beeld wordt een Data Base van de ingerichte Compute-laag verplaatst naar de serverloze Compute-laag.

```sql
ALTER DATABASE testdb 
MODIFY ( SERVICE_OBJECTIVE = 'GP_S_Gen5_1') ;
```

Zie [ALTER data base](/sql/t-sql/statements/alter-database-transact-sql?view=azuresqldb-current)(Engelstalig) voor meer informatie.

### <a name="move-database-from-serverless-compute-tier-into-provisioned-compute-tier"></a>Data base verplaatsen van serverloze Compute-laag naar ingerichte Compute-laag

Een serverloze data base kan op dezelfde manier worden verplaatst naar een ingerichte Compute-laag als het verplaatsen van een ingerichte Compute-Data Base naar een serverloze Compute-laag.

## <a name="modifying-serverless-configuration"></a>Configuratie zonder server wijzigen

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

Het wijzigen van de maximale of minimale vCores en de vertraging voor autopause wordt uitgevoerd met behulp van de [set-AzSqlDatabase](/powershell/module/az.sql/set-azsqldatabase) opdracht in Power shell met behulp van de argumenten `MaxVcore`, `MinVcore`en `AutoPauseDelayInMinutes`.

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

Het wijzigen van de maximale of minimale vCores en de vertraging voor autopause wordt uitgevoerd met behulp van de opdracht [AZ SQL DB Update](/cli/azure/sql/db#az-sql-db-update) in azure CLI met behulp van de argumenten `capacity`, `min-capacity`en `auto-pause-delay`.

* * *

## <a name="monitoring"></a>Controleren

### <a name="resources-used-and-billed"></a>Gebruikte resources en gefactureerd

De resources van een serverloze Data Base worden ingekapseld door het app-pakket, het SQL-exemplaar en de entiteiten van de gebruikers resource groep.

#### <a name="app-package"></a>App-pakket

Het app-pakket is de buitenste grens voor resource beheer voor een Data Base, ongeacht of de data base zich in een serverloze of ingerichte Compute-laag bevindt. Het app-pakket bevat het SQL-exemplaar en de externe services die samen het bereik hebben van alle gebruikers-en systeem resources die worden gebruikt door een data base in SQL Database. Voor beelden van externe services zijn R en zoeken in volledige tekst. Het SQL-exemplaar is doorgaans het totale resource gebruik in het app-pakket.

#### <a name="user-resource-pool"></a>Resource groep gebruiker

De resource groep van de gebruiker is de binnenste grenzen voor bron beheer voor een Data Base, ongeacht of de data base zich in een serverloze of ingerichte Compute-laag bevindt. De resource groep van de gebruiker bereiken CPU en IO voor de werk belasting van de gebruiker die wordt gegenereerd door DDL-query's, zoals het maken en wijzigen van en DML-query's zoals selecteren, invoegen, bijwerken en verwijderen. Deze query's vertegenwoordigen in het algemeen het meest significante deel van het gebruik binnen het app-pakket.

### <a name="metrics"></a>Metrische gegevens

De metrische gegevens voor het bewaken van het resource gebruik van het app-pakket en de gebruikers groep van een serverloze Data Base worden weer gegeven in de volgende tabel:

|Entiteit|Gegevens|Beschrijving|Eenheden|
|---|---|---|---|
|App-pakket|app_cpu_percent|Het percentage vCores dat door de app wordt gebruikt ten opzichte van het maximale aantal vCores dat voor de app is toegestaan.|Percentage|
|App-pakket|app_cpu_billed|De hoeveelheid reken kracht die tijdens de rapportage periode in rekening wordt gebracht voor de app. Het betaalde bedrag tijdens deze periode is het product van deze metrische waarde en de vCore eenheids prijs. <br><br>Waarden van deze metrische gegevens worden bepaald door het samen voegen van het maximum van CPU-gebruik en het geheugen dat elke seconde wordt gebruikt. Als de gebruikte hoeveelheid kleiner is dan de minimum hoeveelheid die is ingericht zoals ingesteld door de min-vCores en het minimale geheugen, wordt de ingerichte minimum hoeveelheid gefactureerd. Als u de CPU wilt vergelijken met geheugen voor facturerings doeleinden, wordt geheugen genormaliseerd in eenheden van vCores door de hoeveelheid geheugen in GB met 3 GB per vCore opnieuw te schalen.|vCore seconden|
|App-pakket|app_memory_percent|Percentage van het geheugen dat wordt gebruikt door de app ten opzichte van Maxi maal geheugen dat is toegestaan voor de app.|Percentage|
|Gebruikers groep|cpu_percent|Het percentage vCores dat wordt gebruikt door de werk belasting van de gebruiker ten opzichte van Max vCores toegestaan voor de werk belasting|Percentage|
|Gebruikers groep|data_IO_percent|Het percentage gegevens-IOPS dat wordt gebruikt door de werk belasting van de gebruiker ten opzichte van het maximale aantal gegevens IOPS dat is toegestaan voor|Percentage|
|Gebruikers groep|log_IO_percent|Percentage logboek MB/s dat wordt gebruikt door de werk belasting van de gebruiker ten opzichte van het maximale aantal logboeken per seconde dat is toegestaan voor de werk belasting|Percentage|
|Gebruikers groep|workers_percent|Percentage van werk nemers die worden gebruikt door de werk belasting van de gebruiker ten opzichte van Maxi maal toegestane werk nemers|Percentage|
|Gebruikers groep|sessions_percent|Het percentage sessies dat wordt gebruikt door de werk belasting van de gebruiker ten opzichte van het maximum aantal toegestane sessies voor de werk belasting|Percentage|

### <a name="pause-and-resume-status"></a>Status onderbreken en hervatten

In de Azure Portal wordt de status van de Data Base weer gegeven in het deel venster Overzicht van de server met de data bases die deze bevat. De database status wordt ook weer gegeven in het deel venster Overzicht voor de data base.

Gebruik de volgende Power shell-opdracht om de status van een Data Base te onderbreken en te hervatten:

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

```powershell
Get-AzSqlDatabase -ResourceGroupName $resourcegroupname -ServerName $servername -DatabaseName $databasename `
  | Select -ExpandProperty "Status"
```

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

```powershell
az sql db show --name $databasename --resource-group $resourcegroupname --server $servername --query 'status' -o json
```

* * *

## <a name="resource-limits"></a>Bronbeperkingen

Zie [serverloze Compute-laag](sql-database-vCore-resource-limits-single-databases.md#general-purpose---serverless-compute---gen5)voor resource limieten.

## <a name="billing"></a>Billing

De hoeveelheid berekenings kosten is het maximum van CPU-gebruik en het geheugen dat elke seconde wordt gebruikt. Als de gebruikte hoeveelheid CPU en het gebruikte geheugen kleiner is dan het minimum aantal dat voor elk is ingericht, wordt de ingerichte hoeveelheid gefactureerd. Als u de CPU wilt vergelijken met geheugen voor facturerings doeleinden, wordt geheugen genormaliseerd in eenheden van vCores door de hoeveelheid geheugen in GB met 3 GB per vCore opnieuw te schalen.

- **Gefactureerde resource**: CPU en geheugen
- **Gefactureerd bedrag**: vCore eenheids prijs * Max (min vCores, vCores gebruikt, min geheugen gb * 1/3, geheugen GB gebruikt * 1/3) 
- **Facturerings frequentie**: per seconde

De vCore eenheids prijs is de kosten per vCore per seconde. Raadpleeg de [pagina met prijzen](https://azure.microsoft.com/pricing/details/sql-database/single/) voor de Azure SQL database voor specifieke eenheids prijzen in een bepaalde regio.

De gefactureerde hoeveelheid berekenings kosten wordt weer gegeven met de volgende metriek:

- **Metrisch**: App_cpu_billed (vCore seconden)
- **Definitie**: Max (min vCores, vCores gebruikt, min geheugen gb * 1/3, geheugen GB gebruikt * 1/3)
- **Rapportage frequentie**: per minuut

Deze hoeveelheid wordt per seconde berekend en op 1 minuut geaggregeerd.

Overweeg een serverloze data base die is geconfigureerd met 1 min vCore en Maxi maal 4 vCores.  Dit komt overeen met ongeveer 3 GB min geheugen en een maximum van 12 GB geheugen.  Stel dat de vertraging voor automatisch onderbreken is ingesteld op 6 uur en dat de werk belasting van de data base actief is gedurende de eerste twee uur van een periode van 24 uur en anders niet actief is.    

In dit geval wordt de data base in de eerste acht uur gefactureerd voor Compute en opslag.  Hoewel de data base na het tweede uur inactief is, wordt deze in de komende 6 uur nog steeds gefactureerd op basis van de minimale Compute die is ingericht terwijl de data base online is.  Alleen opslag wordt in rekening gebracht gedurende de rest van de periode van 24 uur wanneer de data base wordt onderbroken.

Nauw keuriger wordt de reken factuur in dit voor beeld als volgt berekend:

|Tijdsinterval|vCores elke seconde gebruikt|GB gebruikt elke seconde|Gefactureerde reken dimensie|vCore seconden gefactureerd over het tijds interval|
|---|---|---|---|---|
|0:00-1:00|4|9|vCores gebruikt|4 vCores seconden * 3600 seconde = 14400 vCore seconden|
|1:00-2:00|1|12|Gebruikt geheugen|12 GB * 1/3 * 3600 seconden = 14400 vCore seconden|
|2:00-8:00|0|0|Mini maal geheugen ingericht|3 GB * 1/3 * 21600 seconden = 21600 vCore seconden|
|8:00-24:00|0|0|Er wordt geen reken tijd in rekening gebracht|0 vCore seconden|
|Totaal aantal vCore seconden gefactureerd over 24 uur||||50400 vCore seconden|

Stel dat de reken eenheids prijs $0.000073/vCore/seconde is.  De reken tijd die voor deze periode van 24 uur wordt gefactureerd, is het product van de reken eenheids prijs en vCore seconden in rekening gebracht: $0.000073/vCore/Second * 50400 vCore seconden = $3,68

### <a name="azure-hybrid-benefit-and-reserved-capacity"></a>Azure Hybrid Benefit en gereserveerde capaciteit

Azure Hybrid Benefit (AHB) en gereserveerde capaciteits kortingen zijn niet van toepassing op de serverloze Compute-laag.

## <a name="available-regions"></a>Beschikbare regio's

De compute-laag zonder server is wereld wijd beschikbaar, met uitzonde ring van de volgende regio's: China-oost, China-noord, Duitsland-centraal, Duitsland-noordoost, UK-noord, UK-zuid 2, West-Centraal VS en US Gov Central (Iowa).

## <a name="next-steps"></a>Volgende stappen

- Zie [Quick Start: een enkele data base maken in Azure SQL database met behulp van de Azure Portal](sql-database-single-database-get-started.md)om aan de slag te gaan.
- Zie [resource limieten voor serverloze Compute-lagen](sql-database-vCore-resource-limits-single-databases.md#general-purpose---serverless-compute---gen5)voor resource limieten.
