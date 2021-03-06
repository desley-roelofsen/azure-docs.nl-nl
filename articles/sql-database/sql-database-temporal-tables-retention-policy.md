---
title: Historische gegevens in tijdelijke tabellen beheren
description: Meer informatie over het gebruik van een tijdelijk Bewaar beleid om historische gegevens onder uw beheer te houden.
services: sql-database
ms.service: sql-database
ms.subservice: development
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: bonova
ms.author: bonova
ms.reviewer: carlrab
ms.date: 09/25/2018
ms.openlocfilehash: 3c2460c6f5e0905f45106148ecc3e8a949cf221f
ms.sourcegitcommit: ac56ef07d86328c40fed5b5792a6a02698926c2d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/08/2019
ms.locfileid: "73820688"
---
# <a name="manage-historical-data-in-temporal-tables-with-retention-policy"></a>Historische gegevens in tijdelijke tabellen beheren met het Bewaar beleid

Tijdelijke tabellen kunnen de omvang van de data base groterder zijn dan de reguliere tabellen, met name als u historische gegevens gedurende een langere periode behoudt. Daarom is het Bewaar beleid voor historische gegevens een belang rijk aspect van het plannen en beheren van de levens cyclus van elke tijdelijke tabel. Tijdelijke tabellen in Azure SQL Database worden geleverd met eenvoudig te gebruiken Bewaar mechanisme waarmee u deze taak kunt uitvoeren.

De retentie van de tijdelijke geschiedenis kan worden geconfigureerd op het niveau van de afzonderlijke tabel, waardoor gebruikers flexibele, verouderde policies kunnen maken. Het Toep assen van tijdelijke Bewaar periode is eenvoudig: er is slechts één para meter vereist voor het maken van tabellen of het wijzigen van het schema.

Nadat u het Bewaar beleid hebt gedefinieerd, wordt Azure SQL Database regel matig gecontroleerd als er historische rijen zijn die in aanmerking komen voor het automatisch opschonen van gegevens. Het identificeren van overeenkomende rijen en het verwijderen ervan in de tabel geschiedenis vindt op transparante wijze plaats in de achtergrond taak die door het systeem is gepland en uitgevoerd. De ouderdoms voorwaarde voor de geschiedenis tabel rijen wordt gecontroleerd op basis van de kolom die het einde van de SYSTEM_TIME periode vertegenwoordigt. Als de Bewaar periode bijvoorbeeld is ingesteld op zes maanden, kunnen tabel rijen die in aanmerking komen voor opschonen, voldoen aan de volgende voor waarde:

```
ValidTo < DATEADD (MONTH, -6, SYSUTCDATETIME())
```

In het vorige voor beeld wordt aangenomen dat de kolom **ValidTo** overeenkomt met het einde van SYSTEM_TIME periode.

## <a name="how-to-configure-retention-policy"></a>Bewaar beleid configureren

Voordat u het Bewaar beleid voor een tijdelijke tabel configureert, controleert u eerst of tijdelijk historische Bewaar periode is ingeschakeld *op het database niveau*.

```
SELECT is_temporal_history_retention_enabled, name
FROM sys.databases
```

Database vlag **is_temporal_history_retention_enabled** is standaard ingesteld op ingeschakeld, maar gebruikers kunnen dit wijzigen met de instructie ALTER data base. Het wordt ook automatisch ingesteld op uitgeschakeld na een herstel bewerking naar een bepaald [tijdstip](sql-database-recovery-using-backups.md) . Als u het opruimen van tijdelijke geschiedenis wilt inschakelen voor uw data base, voert u de volgende instructie uit:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

> [!IMPORTANT]
> U kunt bewaren voor tijdelijke tabellen, zelfs als **is_temporal_history_retention_enabled** is uitgeschakeld, maar automatisch opschonen van verouderde rijen niet in dat geval wordt geactiveerd.

Het Bewaar beleid is geconfigureerd tijdens het maken van de tabel door een waarde op te geven voor de para meter HISTORY_RETENTION_PERIOD:

```sql
CREATE TABLE dbo.WebsiteUserInfo
(  
    [UserID] int NOT NULL PRIMARY KEY CLUSTERED
  , [UserName] nvarchar(100) NOT NULL
  , [PagesVisited] int NOT NULL
  , [ValidFrom] datetime2 (0) GENERATED ALWAYS AS ROW START
  , [ValidTo] datetime2 (0) GENERATED ALWAYS AS ROW END
  , PERIOD FOR SYSTEM_TIME (ValidFrom, ValidTo)
 )  
 WITH
 (
     SYSTEM_VERSIONING = ON
     (
        HISTORY_TABLE = dbo.WebsiteUserInfoHistory,
        HISTORY_RETENTION_PERIOD = 6 MONTHS
     )
 );
```

Met Azure SQL Database kunt u de Bewaar periode opgeven door gebruik te maken van verschillende tijds eenheden: dagen, weken, maanden en jaren. Als HISTORY_RETENTION_PERIOD wordt wegge laten, wordt uitgegaan van een oneindige retentie. U kunt ook oneindig tref woord expliciet gebruiken.

In sommige gevallen wilt u mogelijk Bewaar periode configureren nadat de tabel is gemaakt, of u kunt de eerder geconfigureerde waarde wijzigen. Gebruik in dat geval ALTER TABLE-instructie:

```sql
ALTER TABLE dbo.WebsiteUserInfo
SET (SYSTEM_VERSIONING = ON (HISTORY_RETENTION_PERIOD = 9 MONTHS));
```

> [!IMPORTANT]
> Als u SYSTEM_VERSIONING instelt op OFF, blijft de waarde voor de retentie periode *niet behouden* . Het instellen van SYSTEM_VERSIONING op ON zonder HISTORY_RETENTION_PERIOD expliciet resulteert in de oneindige retentie periode.

Als u de huidige status van het Bewaar beleid wilt controleren, gebruikt u de volgende query die de vlag voor tijdelijke Bewaar intentie op database niveau koppelt met Bewaar perioden voor afzonderlijke tabellen:

```sql
SELECT DB.is_temporal_history_retention_enabled,
SCHEMA_NAME(T1.schema_id) AS TemporalTableSchema,
T1.name as TemporalTableName,  SCHEMA_NAME(T2.schema_id) AS HistoryTableSchema,
T2.name as HistoryTableName,T1.history_retention_period,
T1.history_retention_period_unit_desc
FROM sys.tables T1  
OUTER APPLY (select is_temporal_history_retention_enabled from sys.databases
where name = DB_NAME()) AS DB
LEFT JOIN sys.tables T2   
ON T1.history_table_id = T2.object_id WHERE T1.temporal_type = 2
```


## <a name="how-sql-database-deletes-aged-rows"></a>Hoe SQL Database verouderde rijen verwijdert?

Het opschonings proces is afhankelijk van de index indeling van de geschiedenis tabel. Het is belang rijk te onthouden dat *alleen voor geschiedenis tabellen met een geclusterde index (B-Tree of column Store) een eindig Bewaar beleid is geconfigureerd*. Er wordt een achtergrond taak gemaakt voor het opschonen van verouderde gegevens voor alle tijdelijke tabellen met een beperkte Bewaar periode.
De logica voor het opruimen van de geclusterde index van de rowstore (B-Tree) verwijdert verouderde rij in kleinere segmenten (Maxi maal 10.000) tot een minimum aan druk op database logboek en IO-subsysteem. Hoewel bij het opruimen logica de vereiste B-structuur index wordt gebruikt, kan de volg orde van verwijderde items voor de rijen die ouder zijn dan de Bewaar periode niet stevig worden gegarandeerd. *Zorg er daarom voor dat u niet afhankelijk bent van de opschoon volgorde in uw toepassingen*.

Met de opschoon taak voor de geclusterde column Store worden hele [Rijg roepen](https://msdn.microsoft.com/library/gg492088.aspx) tegelijk verwijderd (meestal bevat 1.000.000 van de rijen). Dit is zeer efficiënt, vooral wanneer historische gegevens in een hoog tempo worden gegenereerd.

![Geclusterde column Store-retentie](./media/sql-database-temporal-tables-retention-policy/cciretention.png)

Met uitstekende gegevens compressie en een efficiënte Bewaar periode is geclusterde column store-index een perfecte keuze voor scenario's wanneer uw werk belasting snel een hoge hoeveelheid historische gegevens genereert. Dit patroon is gebruikelijk voor intensieve werk [belastingen voor transactionele verwerking waarbij tijdelijke tabellen worden gebruikt voor het](https://msdn.microsoft.com/library/mt631669.aspx) bijhouden van wijzigingen en controles, trend analyses of IOT-gegevens opname.

## <a name="index-considerations"></a>Index overwegingen

Voor de opschoon taak voor tabellen met rowstore geclusterde index moet index worden gestart met de kolom die overeenkomt met het einde van SYSTEM_TIME periode. Als een dergelijke index niet bestaat, kunt u een eindige Bewaar periode niet configureren:

*Msg 13765, niveau 16, status 1 <br></br> instellen van een eindige Bewaar periode voor de systeem versie van de tijdelijke temporalstagetestdb. dbo. WebsiteUserInfo omdat de geschiedenis tabel temporalstagetestdb. dbo. WebsiteUserInfoHistory niet vereiste geclusterde index bevatten. U kunt een geclusterde column Store-of B-Tree-index maken die begint met de kolom die overeenkomt met het einde van SYSTEM_TIME periode, in de tabel geschiedenis.*

Het is belang rijk te zien dat de standaard geschiedenis tabel die door Azure SQL Database is gemaakt, al een geclusterde index heeft, die compatibel is met het Bewaar beleid. Als u probeert die index te verwijderen voor een tabel met een beperkte Bewaar periode, mislukt de bewerking met de volgende fout:

*Msg 13766, niveau 16, status 1 <br></br> kan de geclusterde index ' WebsiteUserInfoHistory. IX_WebsiteUserInfoHistory niet verwijderen omdat deze wordt gebruikt voor het automatisch opschonen van verouderde gegevens. U kunt HISTORY_RETENTION_PERIOD instellen op oneindig in de bijbehorende tijdelijke systeem versie tabel als u deze index wilt verwijderen.*

Opschonen van de geclusterde column store-index werkt optimaal als historische rijen worden ingevoegd in oplopende volg orde (gesorteerd op de kolom einde van de periode). Dit is altijd het geval wanneer de geschiedenis tabel uitsluitend wordt gevuld door het SYSTEM_VERSIONIOING-mechanisme. Als de rijen in de geschiedenis tabel niet worden gerangschikt op de kolom einde van de periode (dit kan het geval zijn als u bestaande historische gegevens hebt gemigreerd), moet u een geclusterde column store-index maken boven op de rowstore-index van de B-structuur die op de juiste manier is besteld, om optimaal te kunnen profiteren van nemen.

Vermijd het opnieuw samen stellen van een geclusterde column store-index in de geschiedenis tabel met de eindige Bewaar periode, omdat deze de volg orde kan wijzigen in de Rijg roepen die op natuurlijke wijze door de systeem versie bewerking worden opgelegd. Als u een geclusterde column store-index opnieuw wilt maken in de geschiedenis tabel, doet u dat door deze opnieuw te maken op basis van de compatibele B-Tree-index, waarbij u de volg orde van de Rijg roepen nodig hebt voor het opschonen van gegevens. U moet dezelfde benadering hebben als u een tijdelijke tabel maakt met een bestaande geschiedenis tabel die een geclusterde kolom index zonder gegarandeerde gegevens order heeft:

```sql
/*Create B-tree ordered by the end of period column*/
CREATE CLUSTERED INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory (ValidTo)
WITH (DROP_EXISTING = ON);
GO
/*Re-create clustered columnstore index*/
CREATE CLUSTERED COLUMNSTORE INDEX IX_WebsiteUserInfoHistory ON WebsiteUserInfoHistory
WITH (DROP_EXISTING = ON);
```

Wanneer een beperkte Bewaar periode voor de geschiedenis tabel is geconfigureerd met de geclusterde column store-index, kunt u geen aanvullende niet-geclusterde B-Tree-indexen maken voor die tabel:

```sql
CREATE NONCLUSTERED INDEX IX_WebHistNCI ON WebsiteUserInfoHistory ([UserName])
```

Een poging om een instructie uit te voeren is mislukt met de volgende fout:

*Msg 13772, niveau 16, status 1 <br></br> kan geen niet-geclusterde index maken voor een tijdelijke geschiedenis tabel ' WebsiteUserInfoHistory ', omdat er een eindige retentie periode en een geclusterde column store-index zijn gedefinieerd.*

## <a name="querying-tables-with-retention-policy"></a>Query's uitvoeren op tabellen met Bewaar beleid

Met alle query's in de tijdelijke tabel worden de historische rijen die overeenkomen met een eindig Bewaar beleid automatisch gefilterd, om onvoorspelbare en inconsistente resultaten te voor komen, omdat verouderde rijen kunnen worden verwijderd door de opschoon taak, *op elk gewenst moment en in een wille keurige volg orde*.

In de volgende afbeelding ziet u het query plan voor een eenvoudige query:

```sql
SELECT * FROM dbo.WebsiteUserInfo FOR SYSTEM_TIME ALL;
```

Het query plan bevat een extra filter dat is toegepast op de kolom end of period (ValidTo) in de geclusterde index scan operator in de geschiedenis tabel (gemarkeerd). In dit voor beeld wordt ervan uitgegaan dat er één maand Bewaar periode is ingesteld voor de tabel WebsiteUserInfo.

![Bewaar query filter](./media/sql-database-temporal-tables-retention-policy/queryexecplanwithretention.png)

Als u echter direct een query voor de geschiedenis tabel uitvoert, ziet u mogelijk rijen die ouder zijn dan de opgegeven Bewaar periode, maar zonder enige garantie voor Herhaal bare query resultaten. In de volgende afbeelding ziet u het uitvoerings plan voor query's voor de query in de geschiedenis tabel zonder dat er extra filters worden toegepast:

![Query uitvoeren op geschiedenis zonder Bewaar filter](./media/sql-database-temporal-tables-retention-policy/queryexecplanhistorytable.png)

Vertrouw uw bedrijfs logica niet in de Lees geschiedenis tabel, omdat u mogelijk inconsistente of onverwachte resultaten krijgt. U wordt aangeraden tijdelijke query's met FOR SYSTEM_TIME-component te gebruiken voor het analyseren van gegevens in tijdelijke tabellen.

## <a name="point-in-time-restore-considerations"></a>Aandachtspunten voor herstel naar een bepaald tijdstip

Wanneer u een nieuwe Data Base maakt door [een bestaande Data Base te herstellen naar een specifiek punt in de tijd](sql-database-recovery-using-backups.md), is de tijdelijke Bewaar periode uitgeschakeld op het niveau van de data base. (**is_temporal_history_retention_enabled** vlag ingesteld op uit). Met deze functie kunt u alle historische rijen bekijken bij het herstellen, zonder dat u zich zorgen hoeft te maken dat verouderde rijen worden verwijderd voordat u deze opvraagt. U kunt deze gebruiken om *historische gegevens na de geconfigureerde Bewaar periode te controleren*.

Stel dat er voor een tijdelijke tabel één maand een Bewaar periode is opgegeven. Als uw data base in een Premium-servicelaag is gemaakt, kunt u in het verleden een database kopie maken met de database status tot 35 dagen. Op die manier kunt u historische rijen die Maxi maal 65 dagen oud zijn, analyseren door de geschiedenis tabel rechtstreeks te doorzoeken.

Als u het opschonen van de tijdelijke Bewaar periode wilt activeren, moet u de volgende Transact-SQL-instructie uitvoeren na herstel naar een bepaald tijdstip:

```sql
ALTER DATABASE <myDB>
SET TEMPORAL_HISTORY_RETENTION  ON
```

## <a name="next-steps"></a>Volgende stappen

Raadpleeg aan de slag [met tijdelijke tabellen in Azure SQL database](sql-database-temporal-tables.md)voor meer informatie over het gebruik van tijdelijke tabellen in uw toepassingen.

Ga naar Channel 9 om een [echte tijdelijke tekst](https://channel9.msdn.com/Blogs/jsturtevant/Azure-SQL-Temporal-Tables-with-RockStep-Solutions) van de klant te horen en een [Live tijdelijke demonstratie](https://channel9.msdn.com/Shows/Data-Exposed/Temporal-in-SQL-Server-2016)te bekijken.

Raadpleeg de [MSDN-documentatie](https://msdn.microsoft.com/library/dn935015.aspx)voor gedetailleerde informatie over tijdelijke tabellen.
