---
title: Problemen met het verbinden en gebruiken van Azure SQL Database
description: Bevat stappen voor het oplossen van problemen met Azure SQL Database verbindingen en het oplossen van andere SQL Database specifieke problemen
services: sql-database
ms.service: sql-database
ms.topic: troubleshooting
ms.custom: seo-lt-2019, OKR 11/2019
author: v-miegge
ms.author: ramakoni
ms.reviewer: carlrab
ms.date: 11/14/2019
ms.openlocfilehash: 0bd018d90f4ca2c64df56d27eebdc6c9160309ac
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74082409"
---
# <a name="troubleshooting-connectivity-issues-and-other-errors-with-microsoft-azure-sql-database"></a>Verbindings problemen en andere fouten oplossen met Microsoft Azure SQL Database

U ontvangt foutberichten wanneer het verbinden met Azure SQL Database mislukt. Deze verbindings problemen kunnen worden veroorzaakt door Azure SQL Database herconfiguratie, Firewall instellingen, time-out voor verbindingen of onjuiste aanmeldings gegevens. Daarnaast kunt u geen verbinding maken met Azure SQL Database als de maximum limiet van sommige Azure SQL Database resources is bereikt.

## <a name="transient-fault-error-messages"></a>Tijdelijke fout berichten

De Azure-infrastructuur biedt de mogelijkheid om servers dynamisch opnieuw te configureren wanneer zware workloads optreden in de SQL Database-service.  Dit dynamische gedrag kan ervoor zorgen dat uw clientprogramma de verbinding met SQL Database verliest. Dit type fout wordt een *tijdelijke fout*genoemd. Het wordt ten zeerste aangeraden dat uw client programma logica voor nieuwe pogingen heeft, zodat er een verbinding tot stand kan worden gebracht nadat de tijdelijke fout tijd is gecorrigeerd.  U wordt aangeraden vijf seconden vóór uw eerste nieuwe poging uit te stellen. Opnieuw proberen na een vertraging die korter is dan 5 seconden Risico's voor de Cloud service. Voor elke volgende poging moet de vertraging exponentieel toenemen, tot een maximum van 60 seconden.

Zie voor code voorbeelden van de logica voor opnieuw proberen:

* [Verbindings bibliotheken voor SQL Database en SQL Server](sql-database-libraries.md)
* [Acties voor het oplossen van verbindings fouten en tijdelijke fouten in SQL Database](sql-database-connectivity-issues.md)

> [!TIP]
> Voor het oplossen van de problemen die in de volgende secties worden besproken, voert u de stappen (in de aangegeven volg orde) uit in de sectie [stappen voor het oplossen van veelvoorkomende problemen met de verbinding](#steps-to-fix-common-connection-issues) .

### <a name="error-40613-database--x--on-server--y--is-not-currently-available"></a>Fout 40613: data base < x > op server < y > is momenteel niet beschikbaar

``40613: Database <DBname> on server < server name > is not currently available. Please retry the connection later. If the problem persists, contact customer support, and provide them the session tracing ID of '< Tracing ID >'.``

Los dit probleem als volgt op:

1. Raadpleeg het [dash board](https://status.azure.com/status) van de Microsoft Azure-service voor eventuele bekende storingen.
2. Als er geen bekende storingen zijn, gaat u naar de [ondersteunings website van Microsoft Azure](https://azure.microsoft.com/support/options) om een ondersteunings aanvraag te openen.

Zie [problemen oplossen met de fout ' Data Base op server is momenteel niet beschikbaar '](sql-database-troubleshoot-common-connection-issues.md#troubleshoot-transient-errors)voor meer informatie.

### <a name="a-network-related-or-instance-specific-error-occurred-while-establishing-a-connection-to-sql-database-server"></a>Er is een netwerkgerelateerde of exemplaar fout opgetreden tijdens het maken van een verbinding met SQL Database Server

Het probleem treedt op als de toepassing geen verbinding kan maken met de server.

U kunt dit probleem oplossen door de stappen (in de weer gegeven volg orde) in de [stappen voor het oplossen van veelvoorkomende problemen met de verbinding te](#steps-to-fix-common-connection-issues) verhelpen.

### <a name="the-serverinstance-was-not-found-or-was-not-accessible-errors-26-40-10053"></a>De server/het exemplaar is niet gevonden of niet toegankelijk (fouten 26, 40, 10053)

#### <a name="error-26-error-locating-server-specified"></a>Fout 26: fout bij het zoeken van de opgegeven server

``System.Data.SqlClient.SqlException: A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections.(provider: SQL Network Interfaces, error: 26 – Error Locating Server/Instance Specified)``

#### <a name="error-40-could-not-open-a-connection-to-the-server"></a>Fout 40: kan geen verbinding met de server openen

``A network-related or instance-specific error occurred while establishing a connection to SQL Server. The server was not found or was not accessible. Verify that the instance name is correct and that SQL Server is configured to allow remote connections. (provider: Named Pipes Provider, error: 40 - Could not open a connection to SQL Server)``

#### <a name="error-10053-a-transport-level-error-has-occurred-when-receiving-results-from-the-server"></a>Fout 10053: er is een fout op transport niveau opgetreden bij het ontvangen van resultaten van de server

``10053: A transport-level error has occurred when receiving results from the server. (Provider: TCP Provider, error: 0 - An established connection was aborted by the software in your host machine)``

#### <a name="cannot-connect-to-a-secondary-database"></a>Kan geen verbinding maken met een secundaire data base

Een verbindings poging met een secundaire data base is mislukt omdat de data base opnieuw wordt geconfigureerd en er bezig is met het Toep assen van nieuwe pagina's in het midden van een actieve trans actie op de primaire data base.

#### <a name="adonet-and-blocking-period"></a>ADO.NET en blokkerings periode

Een bespreking van de *blokkerings periode* voor clients die gebruikmaken van ADO.net is beschikbaar in [SQL Server Connection pooling (ADO.net)](https://msdn.microsoft.com/library/8xx3tyca.aspx).

### <a name="list-of-transient-fault-error-codes"></a>Lijst met tijdelijke fout codes

De volgende fouten zijn tijdelijk en moeten opnieuw worden geprobeerd in toepassings logica:

| Foutcode | Severity | Beschrijving |
| ---:| ---:|:--- |
| 4060 |16 |Kan database niet openen "%.&#x2a;ls" aangevraagd door de aanmelding. De aanmelding is mislukt. Zie [fouten 4000 tot 4999](https://docs.microsoft.com/sql/relational-databases/errors-events/database-engine-events-and-errors#errors-4000-to-4999) voor meer informatie.|
| 40197 |17 |De service heeft een fout aangetroffen bij het verwerken van uw aanvraag. Probeer het opnieuw. Fout code% d.<br/><br/>U ontvangt deze fout melding wanneer de service niet beschikbaar is vanwege software-of hardware-upgrades, hardwarefouten of andere failover-problemen. De fout code (% d) Inge sloten in het bericht van fout 40197] bevat aanvullende informatie over het soort fout of failover dat heeft plaatsgevonden. Enkele voor beelden van de fout codes zijn Inge sloten in het bericht met de fout 40197 zijn 40020, 40143, 40166 en 40540.<br/><br/>Als u opnieuw verbinding maakt met uw SQL Database Server, wordt u automatisch verbonden met een gezonde kopie van uw data base. Uw toepassing moet fout 40197 opvangen, de Inge sloten fout code (% d) in het bericht voor het oplossen van problemen registreren en opnieuw proberen verbinding te maken met SQL Database totdat de resources beschikbaar zijn en uw verbinding opnieuw tot stand is gebracht. Zie [tijdelijke fouten](sql-database-connectivity-issues.md#transient-errors-transient-faults)voor meer informatie.|
| 40501 |20 |De service is momenteel bezet. Voer de aanvraag na 10 seconden opnieuw uit. Incident-ID:% ls. Code:% d. Ga voor meer informatie naar: <br/>limieten voor &bull; &nbsp;[Data Base-server resources](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor afzonderlijke data bases](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[vCore limieten voor afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[vCore limieten voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)<br/>de [resource limieten van de beheerde instantie](sql-database-managed-instance-resource-limits.md)&bull; &nbsp;.|
| 40613 |17 |Database '%.&#x2a;ls' op server '%.&#x2a;ls' is momenteel niet beschikbaar. Probeer de verbinding later opnieuw. Als het probleem zich blijft voordoen, neem contact op met klantenondersteuning en geeft u de sessietracerings-ID "%.&#x2a;ls".<br/><br/> Deze fout kan optreden als er al een DAC (dedicated Administrator Connection) is ingesteld op de data base. Zie [tijdelijke fouten](sql-database-connectivity-issues.md#transient-errors-transient-faults)voor meer informatie.|
| 49918 |16 |Kan aanvraag niet verwerken. Er zijn onvoldoende bronnen om de aanvraag te verwerken.<br/><br/>De service is momenteel bezet. Voer de aanvraag later opnieuw uit. Ga voor meer informatie naar: <br/>limieten voor &bull; &nbsp;[Data Base-server resources](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor afzonderlijke data bases](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[vCore limieten voor afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[vCore limieten voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)<br/>de [resource limieten van de beheerde instantie](sql-database-managed-instance-resource-limits.md)&bull; &nbsp;. |
| 49919 |16 |Kan de aanvraag voor maken of bijwerken niet verwerken. Er zijn te veel bewerkingen voor het maken of bijwerken van het abonnement% ld.<br/><br/>De service is bezig met het verwerken van meerdere aanvragen voor maken of bijwerken voor uw abonnement of server. Aanvragen zijn momenteel geblokkeerd voor het optimaliseren van resources. Query [sys. dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor bewerkingen die in behandeling zijn. Wacht tot er aanvragen voor maken of bijwerken zijn voltooid of verwijder een van de in behandeling zijnde aanvragen en voer de aanvraag later opnieuw uit. Ga voor meer informatie naar: <br/>limieten voor &bull; &nbsp;[Data Base-server resources](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor afzonderlijke data bases](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[vCore limieten voor afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[vCore limieten voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)<br/>de [resource limieten van de beheerde instantie](sql-database-managed-instance-resource-limits.md)&bull; &nbsp;. |
| 49920 |16 |Kan aanvraag niet verwerken. Er worden te veel bewerkingen uitgevoerd voor het abonnement% ld.<br/><br/>De service is bezig met het verwerken van meerdere aanvragen voor dit abonnement. Aanvragen zijn momenteel geblokkeerd voor het optimaliseren van resources. Query [sys. dm_operation_status](https://msdn.microsoft.com/library/dn270022.aspx) voor de bewerkings status. Wacht totdat aanvragen in behandeling zijn voltooid of verwijder een van de in behandeling zijnde aanvragen en voer de aanvraag later opnieuw uit. Ga voor meer informatie naar: <br/>limieten voor &bull; &nbsp;[Data Base-server resources](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor afzonderlijke data bases](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[vCore limieten voor afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[vCore limieten voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)<br/>de [resource limieten van de beheerde instantie](sql-database-managed-instance-resource-limits.md)&bull; &nbsp;. |
| 4221 |16 |Aanmelden bij lezen-secundair is mislukt omdat er een lange wacht tijd op HADR_DATABASE_WAIT_FOR_TRANSITION_TO_VERSIONING is. De replica is niet beschikbaar voor aanmelding omdat er geen rijkoppen ontbreken voor trans acties die in de vlucht waren toen de replica werd gerecycled. Het probleem kan worden opgelost door de actieve trans acties op de primaire replica te herstellen of door te voeren. Exemplaren van deze voor waarde kunnen worden geminimaliseerd door lange schrijf transacties op de primaire te vermijden. |

## <a name="cannot-connect-to-server-due-to-firewall-issues"></a>Kan geen verbinding maken met de server vanwege problemen met de firewall

### <a name="error-40615-cannot-connect-to--servername-"></a>Fout 40615: er kan geen verbinding worden gemaakt met < ServerName >

U kunt dit probleem oplossen door [firewall instellingen op SQL database te configureren via de Azure Portal.](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)

### <a name="error-5-cannot-connect-to--servername-"></a>Fout 5: er kan geen verbinding worden gemaakt met < ServerName >

U kunt dit probleem oplossen door ervoor te zorgen dat poort 1433 is geopend voor uitgaande verbindingen op alle firewalls tussen de client en Internet.

Zie [Configure the Windows firewall to allow SQL Server Access](https://docs.microsoft.com/azure/sql-database/sql-database-firewall-configure)(Engelstalig) voor meer informatie.

## <a name="unable-to-log-in-to-the-server-errors-18456-40531"></a>Kan niet aanmelden bij de server (fouten 18456, 40531)

### <a name="login-failed-for-user--user-name-"></a>De aanmelding is mislukt voor de gebruiker ' < gebruikers naam > '

``Login failed for user '<User name>'.This session has been assigned a tracing ID of '<Tracing ID>'. Provide this tracing ID to customer support when you need assistance. (Microsoft SQL Server, Error: 18456)``

Als u dit probleem wilt oplossen, neemt u contact op met de service beheerder om u een geldige SQL Server gebruikers naam en wacht woord op te geven.

De service beheerder kan doorgaans de volgende stappen gebruiken om de aanmeldings referenties toe te voegen:

1. Meld u aan bij de server met behulp van SQL Server Management Studio (SSMS).
2. Voer de volgende SQL-query uit om te controleren of de aanmeldings naam is uitgeschakeld:

   ```sql
   SELECT name, is_disabled FROM sys.sql_logins
   ```

3. Als de bijbehorende naam is uitgeschakeld, schakelt u deze in met behulp van de volgende instructie:

   ```sql
   Alter login <User name> enable
   ```

4. Als de gebruikers naam voor SQL-aanmelding niet bestaat, maakt u deze door de volgende stappen uit te voeren:

   1. In SSMS dubbelklikt u op **beveiliging** om het uit te vouwen.
   2. Klik met de rechtermuisknop op **Aanmeldingen**, en selecteer vervolgens **Nieuwe aanmelding**.
   3. Bewerk en voer de volgende SQL-query uit in het gegenereerde script met tijdelijke aanduidingen:

   ```sql
   CREATE LOGIN <SQL_login_name, sysname, login_name>
   WITH PASSWORD = ‘<password, sysname, Change_Password>’
   GO
   ```

5. Dubbelklik op **database**.
6. Selecteer de data base waaraan u de gebruiker toestemming wilt verlenen.
7. Dubbelklik op **Beveiliging**.
8. Klik met de rechtermuisknop op **Gebruikers**, en selecteer vervolgens **Nieuwe gebruiker**.
9. Bewerk en voer de volgende SQL-query uit in het gegenereerde script met tijdelijke aanduidingen:

   ```sql
   CREATE USER <user_name, sysname, user_name>
   FOR LOGIN <login_name, sysname, login_name>
   WITH DEFAULT_SCHEMA = <default_schema, sysname, dbo>
   GO
   -- Add user to the database owner role

   EXEC sp_addrolemember N’db_owner’, N’<user_name, sysname, user_name>’
   GO
   ```

   > [!NOTE]
   > U kunt `sp_addrolemember` ook gebruiken om specifieke gebruikers toe te wijzen aan specifieke database rollen.

Zie [data bases en aanmeldingen beheren in Azure SQL database](https://docs.microsoft.com/azure/sql-database/sql-database-manage-logins)voor meer informatie.

## <a name="connection-timeout-expired-errors"></a>Time-out tijdens verbindingstime-out

### <a name="systemdatasqlclientsqlexception-0x80131904-connection-timeout-expired"></a>System. data. SqlClient. SqlException (0x80131904): time-out voor verbinding verlopen

``System.Data.SqlClient.SqlException (0x80131904): Connection Timeout Expired. The timeout period elapsed while attempting to consume the pre-login handshake acknowledgement. This could be because the pre-login handshake failed or the server was unable to respond back in time. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=3; handshake=29995;``

### <a name="systemdatasqlclientsqlexception-0x80131904-timeout-expired"></a>System. data. SqlClient. SqlException (0x80131904): time-out verlopen

``System.Data.SqlClient.SqlException (0x80131904): Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding.``

### <a name="systemdataentitycoreentityexception-the-underlying-provider-failed-on-open"></a>System. data. entity. core. EntityException: de onderliggende provider is niet geopend

``System.Data.Entity.Core.EntityException: The underlying provider failed on Open. -> System.Data.SqlClient.SqlException: Timeout expired. The timeout period elapsed prior to completion of the operation or the server is not responding. -> System.ComponentModel.Win32Exception: The wait operation timed out``

### <a name="cannot-connect-to--server-name-"></a>Kan geen verbinding maken met < server naam >

``Cannot connect to <server name>.ADDITIONAL INFORMATION:Connection Timeout Expired. The timeout period elapsed during the post-login phase. The connection could have timed out while waiting for server to complete the login process and respond; Or it could have timed out while attempting to create multiple active connections. The duration spent while attempting to connect to this server was - [Pre-Login] initialization=231; handshake=983; [Login] initialization=0; authentication=0; [Post-Login] complete=13000; (Microsoft SQL Server, Error: -2) For help, click: http://go.microsoft.com/fwlink?ProdName=Microsoft%20SQL%20Server&EvtSrc=MSSQLServer&EvtID=-2&LinkId=20476 The wait operation timed out``

Deze uitzonde ringen kunnen worden veroorzaakt door verbindings-of query problemen. Zie [bevestigen of een fout wordt veroorzaakt door een verbindings probleem](#confirm-whether-an-error-is-caused-by-a-connectivity-issue)om te controleren of deze fout wordt veroorzaakt door verbindings problemen.

Verbindingstime-outs doen zich voor omdat de toepassing geen verbinding kan maken met de server. U kunt dit probleem oplossen door de stappen (in de weer gegeven volg orde) in de [stappen voor het oplossen van veelvoorkomende problemen met de verbinding te](#steps-to-fix-common-connection-issues) verhelpen.

## <a name="transient-errors-errors-40197-40545"></a>Tijdelijke fouten (fouten 40197, 40545)

### <a name="error-40197-the-service-has-encountered-an-error-processing-your-request-please-try-again-error-code--code-"></a>Fout 40197: de service heeft een fout aangetroffen tijdens het verwerken van uw aanvraag. Probeer het opnieuw. Fout code < code >

Dit probleem treedt op vanwege een tijdelijke fout die is opgetreden tijdens een herconfiguratie of failover op de back-end.

U kunt dit probleem oplossen door een korte periode te wachten en opnieuw te proberen. Er is geen ondersteunings aanvraag vereist tenzij het probleem zich blijft voordoen.

Als best practice, moet u ervoor zorgen dat logica voor nieuwe pogingen is ingesteld. Zie [problemen met tijdelijke fouten en verbindings SQL database fouten oplossen](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)voor meer informatie over de logica voor opnieuw proberen.

## <a name="resource-governance-errors"></a>Resource governance-fouten

### <a name="error-10928-resource-id-d"></a>Fout 10928: Resource-ID:% d

``10928: Resource ID: %d. The %s limit for the database is %d and has been reached. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. The Resource ID value in error message indicates the resource for which limit has been reached. For sessions, Resource ID = 2.``

Ga op een van de volgende manieren te werk om dit probleem op te lossen:

* Controleer of er langlopende query's zijn.

  > [!NOTE]
  > Dit is een minimale aanpak waarmee het probleem mogelijk niet kan worden opgelost.

1. Voer de volgende SQL-query uit om de weer gave [sys. dm_exec_requests](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-exec-requests-transact-sql) te controleren om blokkerings aanvragen weer te geven:

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. Bepaal de **invoer buffer** voor de hoofd blok kering.
3. Stem de hoofd blok-query af.

   Voor een uitgebreide probleemoplossings procedure, Zie [is mijn query wordt in de Cloud verfijnd?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

Als de data base consequent de limiet heeft bereikt ondanks het blok keren van blokkerende en langlopende query's, kunt u overwegen om een upgrade uit te voeren naar een editie met meer bronnen- [edities](https://azure.microsoft.com/pricing/details/sql-database/).

Zie [dynamische systeem beheer weergaven](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/system-dynamic-management-views)voor meer informatie over dynamische beheer weergaven.

Zie [SQL database resource limieten voor Azure SQL database-server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server)voor meer informatie over database limieten.

### <a name="error-10929-resource-id-1"></a>Fout 10929: Resource-ID: 1

``10929: Resource ID: 1. The %s minimum guarantee is %d, maximum limit is %d and the current usage for the database is %d. However, the server is currently too busy to support requests greater than %d for this database. See http://go.microsoft.com/fwlink/?LinkId=267637 for assistance. Otherwise, please try again later.``

### <a name="error-40501-the-service-is-currently-busy"></a>Fout 40501: de service is momenteel bezet

``40501: The service is currently busy. Retry the request after 10 seconds. Incident ID: %ls. Code: %d.``

Dit is een beperkings fout van de engine, een indicatie dat de resource limieten worden overschreden.

Zie [resource limieten voor Data Base Server](https://docs.microsoft.com/azure/sql-database/sql-database-resource-limits-database-server)voor meer informatie over resource limieten.

### <a name="error-40544-the-database-has-reached-its-size-quota"></a>Fout 40544: de grootte van de data base is bereikt

``40544: The database has reached its size quota. Partition or delete data, drop indexes, or consult the documentation for possible resolutions. Incident ID: <ID>. Code: <code>.``

Deze fout treedt op wanneer de data base het quotum voor de grootte heeft bereikt.

U kunt de volgende stappen gebruiken om het probleem op te lossen of om extra opties te bieden:

1. Controleer de huidige grootte van de data base met behulp van het dash board in de Azure Portal.

   > [!NOTE]
   > Voer de volgende SQL-query uit om te bepalen welke tabellen de meeste ruimte gebruiken en daarom potentiële kandidaten voor opschoning hebben:

   ```sql
   SELECT o.name,
    a.SUM(p.row_count) AS 'Row Count',
    b.SUM(p.reserved_page_count) * 8.0 / 1024 AS 'Table Size (MB)'
   FROM sys.objects o
   JOIN sys.dm_db_partition_stats p on p.object_id = o.object_id
   GROUP BY o.name
   ORDER BY [Table Size (MB)] DESC
   ```

2. Als de huidige grootte niet groter is dan de maximale grootte die voor uw editie wordt ondersteund, kunt u ALTER Data Base gebruiken om de waarde voor MAXSIZE te verhogen.
3. Als de data base al de Maxi maal ondersteunde grootte voor uw versie overschrijdt, kunt u een of meer van de volgende stappen uitvoeren:

   * Normale activiteiten voor het opschonen van data bases uitvoeren. U kunt bijvoorbeeld de ongewenste gegevens opschonen met behulp van afkap ping/verwijderen of gegevens verplaatsen met behulp van SQL Server Integration Services (SSIS) of het hulp programma voor het bulksgewijs kopiëren van Program ma's (BCP).
   * Partitioneer of verwijder gegevens, verwijder indexen of Raadpleeg de documentatie voor mogelijke oplossingen.
   * Zie voor het schalen van de Data Base [afzonderlijke database resources schalen](https://docs.microsoft.com/azure/sql-database/sql-database-single-database-scale) en [bronnen voor elastische Pools schalen](https://docs.microsoft.com/azure/sql-database/sql-database-elastic-pool-scale).

### <a name="error-40549-session-is-terminated-because-you-have-a-long-running-transaction"></a>Fout 40549: de sessie is beëindigd omdat u een langlopende trans actie hebt

``40549: Session is terminated because you have a long-running transaction. Try shortening your transaction.``

Als u deze fout herhaaldelijk ondervindt, kunt u proberen om het probleem op te lossen door de volgende stappen uit te voeren:

1. Controleer de weer gave sys. dm_exec_requests om geopende sessies met een hoge waarde voor de total_elapsed_time kolom te bekijken. Voer deze controle uit door het volgende SQL-script uit te voeren:

   ```sql
   SELECT * FROM dm_exec_requests
   ```

2. Bepaal de invoer buffer voor de langlopende query.
3. De query afstemmen.

U kunt ook uw query's batcheren. Zie voor meer informatie over batch verwerking [batch verwerking gebruiken om de prestaties van SQL database-toepassingen te verbeteren](https://docs.microsoft.com/azure/sql-database/sql-database-use-batching-to-improve-performance).

Voor een uitgebreide probleemoplossings procedure, Zie [is mijn query wordt in de Cloud verfijnd?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

### <a name="error-40551-the-session-has-been-terminated-because-of-excessive-tempdb-usage"></a>Fout 40551: de sessie is beëindigd vanwege het overmatige gebruik van TEMPDB

``40551: The session has been terminated because of excessive TEMPDB usage. Try modifying your query to reduce the temporary table space usage.``

Voer de volgende stappen uit om dit probleem te omzeilen:

1. Wijzig de query's om het gebruik van tijdelijke tabel ruimte te verminderen.
2. Tijdelijke objecten verwijderen nadat ze niet meer nodig zijn.
3. Tabellen afkappen of ongebruikte tabellen verwijderen.

### <a name="error-40552-the-session-has-been-terminated-because-of-excessive-transaction-log-space-usage"></a>Fout 40552: de sessie is beëindigd vanwege het overmatige gebruik van het transactie logboek

``40552: The session has been terminated because of excessive transaction log space usage. Try modifying fewer rows in a single transaction.``

Probeer de volgende methoden om dit op te lossen:

* Het probleem kan worden veroorzaakt door invoeg-, update-of verwijderings bewerkingen.
Probeer het aantal rijen dat onmiddellijk wordt geëxploiteerd, te verminderen door batches te implementeren of te splitsen in meerdere kleinere trans acties.
* Het probleem kan zich voordoen als gevolg van het opnieuw opbouwen van de index. U kunt dit probleem omzeilen door te controleren of het aantal rijen dat in de tabel wordt beïnvloed * (gemiddelde grootte van het veld dat wordt bijgewerkt in bytes + 80) < 2 gigabytes (GB).

  > [!NOTE]
  > Voor een opnieuw opgebouwde index moet de gemiddelde grootte van het veld dat wordt bijgewerkt, worden vervangen door de gemiddelde index grootte.

### <a name="error-40553-the-session-has-been-terminated-because-of-excessive-memory-usage"></a>Fout 40553: de sessie is beëindigd vanwege het overmatige geheugen gebruik

``40553 : The session has been terminated because of excessive memory usage. Try modifying your query to process fewer rows.``

U kunt dit probleem omzeilen door de query te optimaliseren.

Voor een uitgebreide probleemoplossings procedure, Zie [is mijn query wordt in de Cloud verfijnd?](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

### <a name="table-of-additional-resource-governance-error-messages"></a>Tabel met aanvullende resource governance-fout berichten

| Foutcode | Severity | Beschrijving |
| ---:| ---:|:--- |
| 10928 |20 |Resource-ID:% d. De% s-limiet voor de data base is% d en is bereikt. Zie [SQL database resource limieten voor één en gepoolde data bases](sql-database-resource-limits-database-server.md)voor meer informatie.<br/><br/>De resource-ID geeft de resource aan die de limiet heeft bereikt. Voor worker-threads, de resource-ID = 1. Voor sessies, de resource-ID = 2.<br/><br/>Zie voor meer informatie over deze fout en hoe u deze kunt oplossen: <br/>limieten voor &bull; &nbsp;[Data Base-server resources](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor afzonderlijke data bases](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[vCore limieten voor afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[vCore limieten voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)<br/>de [resource limieten van de beheerde instantie](sql-database-managed-instance-resource-limits.md)&bull; &nbsp;. |
| 10929 |20 |Resource-ID:% d. De minimale garantie van% s is% d, de maximum limiet is% d en het huidige gebruik van de data base is% d. De server is momenteel echter bezet om aanvragen te ondersteunen die groter zijn dan% d voor deze data base. De resource-ID geeft de resource aan die de limiet heeft bereikt. Voor worker-threads, de resource-ID = 1. Voor sessies, de resource-ID = 2. Ga voor meer informatie naar: <br/>limieten voor &bull; &nbsp;[Data Base-server resources](sql-database-resource-limits-database-server.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor afzonderlijke data bases](sql-database-service-tiers-dtu.md)<br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; &nbsp;[vCore limieten voor afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[vCore limieten voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)<br/>de [resource limieten van de beheerde instantie](sql-database-managed-instance-resource-limits.md)&bull; &nbsp;. <br/>Probeer het later opnieuw. |
| 40544 |20 |De data base heeft het quotum voor de grootte bereikt. Partitioneer of verwijder gegevens, verwijder indexen of Raadpleeg de documentatie voor mogelijke oplossingen. Zie voor het schalen van de Data Base [afzonderlijke database resources schalen](sql-database-single-database-scale.md) en [bronnen voor elastische Pools schalen](sql-database-elastic-pool-scale.md).|
| 40549 |16 |De sessie is beëindigd omdat u een langlopende trans actie hebt. Probeer uw trans actie te verkorten. Zie voor meer informatie over batch verwerking [batch verwerking gebruiken om de prestaties van SQL database-toepassingen te verbeteren](sql-database-use-batching-to-improve-performance.md).|
| 40550 |16 |De sessie is beëindigd omdat er te veel vergren delingen zijn verkregen. Probeer minder rijen in één trans actie te lezen of te wijzigen. Zie voor meer informatie over batch verwerking [batch verwerking gebruiken om de prestaties van SQL database-toepassingen te verbeteren](sql-database-use-batching-to-improve-performance.md).|
| 40551 |16 |De sessie is beëindigd vanwege het overmatige `TEMPDB` gebruik. Wijzig uw query om het gebruik van de tijdelijke tabel ruimte te verminderen.<br/><br/>Als u tijdelijke objecten gebruikt, bespaart u ruimte in de `TEMPDB`-data base door tijdelijke objecten te verwijderen nadat deze niet langer nodig zijn voor de sessie. Zie [tempdb-data base in SQL database](https://docs.microsoft.com/sql/relational-databases/databases/tempdb-database#tempdb-database-in-sql-database)voor meer informatie over het gebruik van tempdb in SQL database.|
| 40552 |16 |De sessie is beëindigd vanwege het overmatige gebruik van het transactie logboek. Probeer minder rijen in één trans actie te wijzigen. Zie voor meer informatie over batch verwerking [batch verwerking gebruiken om de prestaties van SQL database-toepassingen te verbeteren](sql-database-use-batching-to-improve-performance.md).<br/><br/>Als u bulksgewijze toevoegingen uitvoert met behulp van het `bcp.exe`-hulp programma of de `System.Data.SqlClient.SqlBulkCopy`-klasse, gebruikt u de opties `-b batchsize` of `BatchSize` om het aantal rijen te beperken dat naar de server in elke trans actie is gekopieerd. Als u een index opnieuw wilt samen stellen met de `ALTER INDEX`-instructie, kunt u de `REBUILD WITH ONLINE = ON` optie gebruiken. Voor informatie over de grootte van het transactie logboek voor het vCore-aankoop model raadpleegt u: <br/>&bull; &nbsp;[vCore limieten voor afzonderlijke data bases](sql-database-vcore-resource-limits-single-databases.md)<br/>&bull; &nbsp;[vCore limieten voor elastische Pools](sql-database-vcore-resource-limits-elastic-pools.md)<br/>de [resource limieten van de beheerde instantie](sql-database-managed-instance-resource-limits.md)&bull; &nbsp;.|
| 40553 |16 |De sessie is beëindigd vanwege het overmatige geheugen gebruik. Wijzig uw query om minder rijen te verwerken.<br/><br/>Het verminderen van het aantal `ORDER BY`-en `GROUP BY`-bewerkingen in uw Transact-SQL-code vermindert de geheugen vereisten van uw query. Zie voor het schalen van de Data Base [afzonderlijke database resources schalen](sql-database-single-database-scale.md) en [bronnen voor elastische Pools schalen](sql-database-elastic-pool-scale.md).|

## <a name="elastic-pool-errors"></a>Fouten in elastische Pools

De volgende fouten zijn gerelateerd aan het maken en gebruiken van elastische Pools:

| Foutcode | Severity | Beschrijving | Corrigerende actie |
|:--- |:--- |:--- |:--- |
| 1132 | 17 |De elastische pool heeft de opslag limiet bereikt. Het opslag gebruik voor de elastische pool mag niet groter zijn dan (% d) MB. Poging tot het schrijven van gegevens naar een Data Base als de opslag limiet van de elastische pool is bereikt. Zie voor informatie over resource limieten: <br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; [limieten voor elastische Pools op vCore](sql-database-vcore-resource-limits-elastic-pools.md)&nbsp;. <br/> |U kunt overwegen de Dtu's van en/of opslag ruimte toe te voegen aan de elastische pool, indien mogelijk om de opslag limiet te verhogen, de opslag die wordt gebruikt door afzonderlijke data bases in de elastische pool te verminderen of data bases uit de elastische pool te verwijderen. Zie [elastische pool resources schalen](sql-database-elastic-pool-scale.md)voor elastische Pools schalen.|
| 10929 | 16 |De minimale garantie van% s is% d, de maximum limiet is% d en het huidige gebruik van de data base is% d. De server is momenteel echter bezet om aanvragen te ondersteunen die groter zijn dan% d voor deze data base. Zie voor informatie over resource limieten: <br/>&bull; &nbsp;[op DTU gebaseerde limieten voor elastische Pools](sql-database-dtu-resource-limits-elastic-pools.md)<br/>&bull; [limieten voor elastische Pools op vCore](sql-database-vcore-resource-limits-elastic-pools.md)&nbsp;. <br/> Probeer het later opnieuw. DTU/vCore min per data base; DTU/vCore Max per data base. Het totale aantal gelijktijdige werk rollen (aanvragen) voor alle data bases in de elastische pool heeft geprobeerd de limiet van de groep te overschrijden. |U kunt eventueel de Dtu's of vCores van de elastische pool verg Roten, zodat de werknemers limiet kan worden verhoogd of data bases uit de elastische pool kunnen worden verwijderd. |
| 40844 | 16 |De data base% ls op server% LS is een% ls Edition-data base in een elastische pool en kan geen doorlopende Kopieer relatie hebben.  |N.v.t. |
| 40857 | 16 |Kan de elastische groep niet vinden voor de server:% ls, naam van elastische groep:% ls. Opgegeven elastische groep bestaat niet in de opgegeven server. | Geef een geldige naam op voor een elastische pool. |
| 40858 | 16 |Elastische pool% ls bestaat al op de server:% ls. Opgegeven elastische groep bestaat al in de opgegeven SQL Database Server. | Geef een nieuwe naam voor de elastische groep op. |
| 40859 | 16 |De elastische pool biedt geen ondersteuning voor de servicelaag% ls. De opgegeven servicelaag wordt niet ondersteund voor het inrichten van elastische Pools. |Geef de juiste editie op of laat de servicelaag leeg om de standaard servicelaag te gebruiken. |
| 40860 | 16 |De combi natie van elastische pool% LS en service doelstelling% LS is ongeldig. Elastische pool en servicelaag kunnen alleen samen worden opgegeven als het resource type is opgegeven als ' ElasticPool '. |Geef een juiste combi natie van elastische pool en servicelaag op. |
| 40861 | 16 |De data base-editie%. *ls kan niet verschillen van de servicelaag van de elastische pool. deze is%.* ls. De data base-editie wijkt af van de servicelaag van de elastische pool. |Geef geen data base-editie op die verschilt van de servicelaag van de elastische pool.  Houd er rekening mee dat de data base-editie niet hoeft te worden opgegeven. |
| 40862 | 16 |De naam van de elastische groep moet worden opgegeven als de service doelstelling van de elastische groep is opgegeven. De service doelstelling van de elastische groep biedt geen unieke identificatie voor een elastische pool. |Geef de naam van de elastische groep op als u gebruikmaakt van de service doelstelling van de elastische groep. |
| 40864 | 16 |De Dtu's voor de elastische pool moet ten minste (% d) Dtu's zijn voor de servicelaag %1!. Er wordt geprobeerd de Dtu's voor de elastische pool in te stellen beneden de minimale limiet. |Stel de Dtu's voor de elastische pool opnieuw in op ten minste de minimale limiet. |
| 40865 | 16 |De Dtu's voor de elastische pool mag niet groter zijn dan (% d) Dtu's voor servicelaag %1!. Er wordt geprobeerd de Dtu's voor de elastische pool in te stellen boven de maximum limiet. |Stel de Dtu's voor de elastische pool in op een waarde die niet groter is dan de maximum limiet. |
| 40867 | 16 |Het maximum aantal DTU per data base moet ten minste (% d) zijn voor de servicelaag %1!. Er wordt geprobeerd het maximale DTU-maximum per data base in te stellen onder de ondersteunde limiet. | Overweeg het gebruik van de servicelaag van de elastische pool die de gewenste instelling ondersteunt. |
| 40868 | 16 |Het maximum aantal DTU per data base mag niet groter zijn dan (% d) voor servicelaag %1!. Er wordt geprobeerd het maximale DTU-maximum per data base in te stellen buiten de ondersteunde limiet. | Overweeg het gebruik van de servicelaag van de elastische pool die de gewenste instelling ondersteunt. |
| 40870 | 16 |Het DTU-minimum per data base mag niet groter zijn dan (% d) voor servicelaag %1!. Poging tot het instellen van de maximale DTU-minimum per data base dan de ondersteunde limiet. | Overweeg het gebruik van de servicelaag van de elastische pool die de gewenste instelling ondersteunt. |
| 40873 | 16 |Het aantal data bases (% d) en de DTU min per data base (% d) mag de Dtu's van de elastische pool (% d) niet overschrijden. Poging tot opgeven van DTU min voor data bases in de elastische pool die groter is dan de Dtu's van de elastische pool. | Overweeg de Dtu's van de elastische pool te verhogen, of verklein de DTU min per data base, of verklein het aantal data bases in de elastische pool. |
| 40877 | 16 |Een elastische pool kan alleen worden verwijderd als deze geen data bases bevat. De elastische pool bevat een of meer data bases en kan daarom niet worden verwijderd. |Verwijder data bases uit de elastische pool om deze te verwijderen. |
| 40881 | 16 |De elastische pool %1! ls heeft de limiet voor het aantal data bases bereikt.  De limiet voor het aantal data bases voor de elastische pool mag niet groter zijn dan (% d) voor een elastische pool met (% d) Dtu's. Poging om een Data Base te maken of toe te voegen aan een elastische pool wanneer de limiet voor het aantal data bases van de elastische pool is bereikt. | Overweeg zo mogelijk de Dtu's van de elastische pool te verhogen om de database limiet te verhogen of om data bases uit de elastische pool te verwijderen. |
| 40889 | 16 |De Dtu's of opslag limiet voor de elastische pool %1!!, kan niet worden verlaagd omdat er onvoldoende opslag ruimte beschikbaar is voor de data bases. Er wordt geprobeerd de opslag limiet van de elastische pool onder het opslag gebruik te verlagen. | Overweeg het opslag gebruik van afzonderlijke data bases in de elastische pool te verminderen of data bases uit de groep te verwijderen om de Dtu's of opslag limiet te verminderen. |
| 40891 | 16 |Het maximum aantal DTU per data base (% d) kan niet groter zijn dan de maximale DTU per data base (% d). Poging om de DTU min per data base in te stellen hoger dan het maximum aantal DTU per data base. |Zorg ervoor dat de minimale DTU per data base het maximale DTU-maximum per data base niet overschrijdt. |
| NOG TE BEPALEN | 16 |De opslag grootte voor een afzonderlijke data base in een elastische pool kan niet groter zijn dan de maximale grootte die is toegestaan door %1! de elastische pool van de service tier. De maximale grootte voor de data base overschrijdt de Maxi maal toegestane grootte van de servicelaag van de elastische groep. |Stel de maximale grootte van de data base in binnen de limieten van de Maxi maal toegestane grootte door de servicelaag van de elastische pool. |

## <a name="cannot-open-database-master-requested-by-the-login-the-login-failed"></a>Kan de data base ' Master ' die door de aanmelding is aangevraagd, niet openen. De aanmelding is mislukt

Dit probleem treedt op omdat het account geen machtiging heeft voor toegang tot de hoofd database. SQL Server Management Studio (SSMS) probeert standaard verbinding te maken met de hoofd database.

Volg deze stappen om dit probleem op te lossen:

1. Selecteer **Opties**in het aanmeldings scherm van SSMS en selecteer vervolgens **verbindings eigenschappen**.
2. Voer in het veld **verbinding maken met data base** de naam van de standaard database van de gebruiker in als de standaard aanmeldings database en selecteer vervolgens **verbinding maken**.

   ![Verbindingseigenschappen](media/troubleshoot-connectivity-issues-microsoft-azure-sql-database/cannot-open-database-master.png)

## <a name="confirm-whether-an-error-is-caused-by-a-connectivity-issue"></a>Controleren of een fout wordt veroorzaakt door een verbindings probleem

Als u wilt controleren of een fout wordt veroorzaakt door een probleem met de verbinding, bekijkt u de stack tracering voor frames die aanroepen weer geven om een verbinding te openen, zoals de volgende: (Let op de verwijzing naar de klasse **SqlConnection** ).

```
System.Data.SqlClient.SqlConnection.TryOpen(TaskCompletionSource`1 retry)
 at System.Data.SqlClient.SqlConnection.Open()
 at AzureConnectionTest.Program.Main(String[] args)
ClientConnectionId:<Client connection ID>
```

Wanneer de uitzonde ring wordt geactiveerd door query problemen, ziet u een aanroep stack die er ongeveer als volgt uitzien (Let op de verwijzing naar de klasse **SqlCommand** ). In dit geval kunt [u uw query's afstemmen](https://blogs.msdn.com/b/sqlblog/archive/2013/11/01/is-my-query-running-fine-in-the-cloud.aspx).

```
  at System.Data.SqlClient.SqlCommand.ExecuteReader()
  at AzureConnectionTest.Program.Main(String[] args)
  ClientConnectionId:<Client ID>
```

Raadpleeg de volgende bronnen voor meer informatie over de prestaties van fijnafstelling:

* [Azure SQL-indexen en-statistieken onderhouden](https://techcommunity.microsoft.com/t5/Azure-Database-Support-Blog/How-to-maintain-Azure-SQL-Indexes-and-Statistics/ba-p/368787)
* [De prestaties van de query in Azure SQL Database hand matig afstemmen](https://docs.microsoft.com/azure/sql-database/sql-database-performance-guidance)
* [Prestaties bewaken Azure SQL Database door dynamische beheer weergaven te gebruiken](https://docs.microsoft.com/azure/sql-database/sql-database-monitoring-with-dmvs)
* [Het query archief in Azure SQL Database gebruiken](https://docs.microsoft.com/azure/sql-database/sql-database-operate-query-store)

## <a name="steps-to-fix-common-connection-issues"></a>Stappen voor het oplossen van veelvoorkomende verbindings problemen

1. Zorg ervoor dat TCP/IP is ingeschakeld als een client protocol op de toepassings server. Zie [client protocollen configureren](https://docs.microsoft.com/sql/database-engine/configure-windows/configure-client-protocols)voor meer informatie. Op toepassings servers waarop u geen SQL Server-hulpprogram ma's hebt geïnstalleerd, controleert u of TCP/IP is ingeschakeld door **cliconfg. exe** (SQL Server Client Network Utility) uit te voeren.
2. Controleer de connection string van de toepassing om te controleren of deze correct is geconfigureerd. Zorg er bijvoorbeeld voor dat de connection string de juiste poort (1433) en de volledig gekwalificeerde server naam opgeeft.
Zie [SQL Server verbindings gegevens ophalen](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms#get-sql-server-connection-information).
3. Probeer de time-outwaarde voor de verbinding te verhogen. Het is raadzaam om een time-out voor de verbinding van ten minste 30 seconden te gebruiken.
4. Test de connectiviteit tussen de toepassings server en de Azure-SQL database met behulp van [SQL Server Management Studio (SSMS)](https://docs.microsoft.com/azure/sql-database/sql-database-connect-query-ssms), een udl-bestand, ping of Telnet. Zie [problemen met SQL Server connectiviteit](https://support.microsoft.com/help/4009936/solving-connectivity-errors-to-sql-server) en [Diagnostische gegevens over verbindings problemen](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues#diagnostics)oplossen voor meer informatie.

   > [!NOTE]
   > Als stap voor het oplossen van problemen kunt u ook de connectiviteit op een andere client computer testen.

5. Zorg ervoor dat de logica voor opnieuw proberen is ingesteld als best practice. Zie [problemen met tijdelijke fouten en verbindings SQL database fouten oplossen](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-issues)voor meer informatie over logica voor opnieuw proberen.

Als u met deze stappen het probleem niet kunt oplossen, kunt u proberen om meer gegevens te verzamelen en vervolgens contact op te nemen met de ondersteuning. Als uw toepassing een Cloud service is, schakelt u logboek registratie in. Deze stap retourneert een UTC-tijds tempel van de fout. Daarnaast retourneert SQL Azure de tracerings-ID. [Micro soft Customer Support Services](https://azure.microsoft.com/support/options/) kan deze informatie gebruiken.

Zie [logboek registratie van diagnostische gegevens inschakelen voor apps in azure app service](https://azure.microsoft.com/documentation/articles/web-sites-enable-diagnostic-log/)voor meer informatie over het inschakelen van logboek registratie.

## <a name="next-steps"></a>Volgende stappen

* [Architectuur van Azure SQL-connectiviteit](https://docs.microsoft.com/azure/sql-database/sql-database-connectivity-architecture)
* [Netwerk toegangs beheer van Azure SQL Database en Data Warehouse](https://docs.microsoft.com/azure/sql-database/sql-database-networkaccess-overview)
