---
title: Artikel over bekende problemen/fouten die zijn opgetreden bij het gebruik van Azure Database Migration Service | Microsoft Docs
description: Meer informatie over het oplossen van veelvoorkomende bekende problemen/fouten die zijn gekoppeld aan het gebruik van Azure Database Migration Service.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 06/18/2019
ms.openlocfilehash: a72a0e26c2af34942faaa6a7e62feef86082e196
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69034823"
---
# <a name="troubleshoot-common-azure-database-migration-service-issues-and-errors"></a>Veelvoorkomende problemen met Azure Database Migration Service oplossen

In dit artikel worden enkele veelvoorkomende problemen en fouten beschreven die Azure Database Migration Service gebruikers kunnen aankomen. Het artikel bevat ook informatie over het oplossen van deze problemen en fouten.

## <a name="migration-activity-in-queued-state"></a>Migratie activiteit in de wachtrij status

Wanneer u nieuwe activiteiten in een Azure Database Migration Service project maakt, blijven de activiteiten in de wachtrij staat.

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Dit probleem treedt op wanneer de Azure Database Migration Service-instantie de maximum capaciteit heeft bereikt voor lopende taken die gelijktijdig worden uitgevoerd. Er wordt een nieuwe activiteit in de wachtrij geplaatst totdat de capaciteit weer beschikbaar wordt. | Controleer of het Data Migration service-exemplaar activiteiten uitvoert in projecten. U kunt door gaan met het maken van nieuwe activiteiten die automatisch worden toegevoegd aan de wachtrij voor uitvoering. Zodra een van de bestaande uitgevoerde activiteiten is voltooid, wordt de volgende activiteit in de wachtrij gestart en wordt de status automatisch gewijzigd in het uitvoeren van de status. U hoeft geen verdere actie te ondernemen om de migratie van activiteiten in de wachtrij te starten.<br><br> |

## <a name="max-number-of-databases-selected-for-migration"></a>Maximum aantal data bases dat is geselecteerd voor migratie

De volgende fout treedt op bij het maken van een activiteit voor een database migratie project voor het verplaatsen naar Azure SQL Database of een Azure SQL Database beheerd exemplaar:

* **Fout**: Validatie fout voor migratie-instellingen "," errorDetail ":" er is meer dan het maximum aantal objecten van data bases "4" geselecteerd voor migratie. "

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout wordt weer gegeven wanneer u meer dan vier data bases hebt geselecteerd voor één migratie activiteit. Op dit moment is elke migratie activiteit beperkt tot vier data bases. | Selecteer vier of minder data bases per migratie activiteit. Als u meer dan vier data bases parallel wilt migreren, moet u een andere instantie van Azure Database Migration Service inrichten. Op dit moment ondersteunt elk abonnement Maxi maal twee Azure Database Migration Service exemplaren.<br><br> |

## <a name="errors-for-mysql-migration-to-azure-mysql-with-recovery-failures"></a>Fouten voor MySQL-migratie naar Azure MySQL met herstel fouten

Wanneer u migreert van MySQL naar Azure Database for MySQL met behulp van Azure Database Migration Service, mislukt de migratie activiteit met de volgende fout:

* **Fout**: Database migratie fout-taak ' TaskID ' is onderbroken vanwege het mislukken van een opeenvolgende herstel bewerking door [n].

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout kan optreden als de gebruiker die de migratie uitvoert, ReplicationAdmin Role en/of privileges van de replicatie-CLIENT, replicatie REPLICA en SUPER (eerdere versies dan MySQL 5.6.6) ontbreekt.<br><br><br><br><br><br><br><br><br><br><br><br><br> | Zorg ervoor dat de [vereiste bevoegdheden](https://docs.microsoft.com/azure/dms/tutorial-mysql-azure-mysql-online#prerequisites) voor het gebruikers account nauw keurig zijn geconfigureerd op het Azure database for MySQL-exemplaar. U kunt bijvoorbeeld de volgende stappen volgen om een gebruiker met de naam ' migrateuser ' te maken met de vereiste bevoegdheden:<br>1. De gebruiker migrateuser @ '% ' maken, geïdentificeerd door ' geheim '; <br>2. Ken alle bevoegdheden op db_name. * toe aan ' migrateuser ' @ '% ', geïdentificeerd door ' geheim '; Herhaal deze stap om toegang te verlenen aan meer data bases <br>3. Replicatie-slave toewijzen aan *.* op ' migrateuser ' @ '% ' geïdentificeerd door ' geheim ';<br>4. Replicatie-client toekennen op *.* op ' migrateuser ' @ '% ' geïdentificeerd door ' geheim ';<br>5. Machtigingen voor leegmaken; |

## <a name="error-when-attempting-to-stop-azure-database-migration-service"></a>Fout bij het stoppen van de Azure Database Migration Service

U ontvangt de volgende fout bij het stoppen van het Azure Database Migration Service-exemplaar:

* **Fout**: De service is niet gestopt. Fout: {'error':{'code':'InvalidRequest','message':'Een of meer activiteiten worden momenteel uitgevoerd. Als u de service wilt stoppen, wacht u totdat de activiteiten zijn voltooid of stopt u deze activiteiten hand matig en probeert u het opnieuw.}}

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout wordt weer gegeven wanneer het service-exemplaar dat u probeert te stoppen, activiteiten bevat die nog in de migratie projecten worden uitgevoerd of aanwezig zijn. <br><br><br><br><br><br> | Zorg ervoor dat er geen activiteiten worden uitgevoerd in het exemplaar van Azure Database Migration Service dat u probeert te stoppen. U kunt ook de activiteiten of projecten verwijderen voordat u probeert de service te stoppen. De volgende stappen laten zien hoe u projecten kunt verwijderen om het migratie service-exemplaar op te schonen door alle actieve taken te verwijderen:<br>1. Install-Module -Name AzureRM.DataMigration <br>2. Login-AzureRmAccount <br>3. Select-AzureRmSubscription-subscriptionname "\<subname >" <br> 4. Remove-AzureRmDataMigrationProject-name \<projectName >-ResourceGroupName \< \<rgName >-Service naam ServiceName >-DeleteRunningTask |

## <a name="error-when-attempting-to-start-azure-database-migration-service"></a>Fout bij het starten van Azure Database Migration Service

U ontvangt de volgende fout bij het starten van het Azure Database Migration Service-exemplaar:

* **Fout**: Service kan niet worden gestart. Fout: {' errorDetail ': ' de service kan niet worden gestart. Neem contact op met micro soft ondersteuning '}

| Oorzaak         | Oplossing |
| ------------- | ------------- |
| Deze fout wordt weer gegeven wanneer het vorige exemplaar niet intern is mislukt. Deze fout treedt zelden op en het technische team is hiervan op de hoogte. <br> | Verwijder het exemplaar van de service dat u niet kunt starten en richt het nieuwe in om het te vervangen. |

## <a name="error-restoring-database-while-migrating-sql-to-azure-sql-db-managed-instance"></a>Fout bij het herstellen van de data base tijdens het migreren van SQL naar Azure SQL data base Managed instance

Wanneer u een online migratie uitvoert van SQL Server naar een Azure SQL Database beheerd exemplaar, mislukt de cutover met de volgende fout:

* **Fout**: De herstel bewerking is mislukt voor de bewerkings-id ' operationId '. Code ' AuthorizationFailed ', Message ' de client ' clientId ' met object-id ' objectId ' heeft geen machtiging voor het uitvoeren van de actie ' micro soft. SQL/locations/managedDatabaseRestoreAzureAsyncOperation/read ' over scope '/Subscriptions/ subscriptionId.

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Deze fout geeft aan dat de toepassings-principal die wordt gebruikt voor online migratie van SQL Server naar een Azure SQL Database beheerde instantie geen bijdrage rechten heeft voor het abonnement. Voor bepaalde API-aanroepen met het beheerde exemplaar is deze machtiging vereist voor de herstel bewerking. <br><br><br><br><br><br><br><br><br><br><br><br><br><br> | Gebruik de `Get-AzureADServicePrincipal` Power shell- `-ObjectId` cmdlet die beschikbaar is in het fout bericht om een lijst weer te geven met de weergave naam van de toepassings-id die wordt gebruikt.<br><br> Valideer de machtigingen voor deze toepassing en controleer of deze de [rol Inzender](https://docs.microsoft.com/azure/role-based-access-control/built-in-roles#contributor) heeft op het abonnements niveau. <br><br> Het Azure Database Migration Service engineering team is bezig om de vereiste toegang te beperken voor de huidige Contribute-rol bij het abonnement. Als u een zakelijke vereiste hebt die het gebruik van een Contribute-functie niet toestaat, neemt u contact op met de ondersteuning van Azure voor meer informatie. |

## <a name="error-when-deleting-nic-associated-with-azure-database-migration-service"></a>Fout bij het verwijderen van de NIC die is gekoppeld aan Azure Database Migration Service

Wanneer u probeert een netwerk interface kaart te verwijderen die is gekoppeld aan Azure Database Migration Service, mislukt de verwijderings poging met deze fout:

* **Fout**: Kan de NIC die is gekoppeld aan Azure Database Migration Service niet verwijderen vanwege het gebruik van de NIC-service

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Dit probleem treedt op wanneer het Azure Database Migration Service-exemplaar mogelijk nog steeds aanwezig is en gebruikmaakt van de NIC. <br><br><br><br><br><br><br><br> | Als u deze NIC wilt verwijderen, verwijdert u het DMS-service-exemplaar dat automatisch de NIC verwijdert die door de service wordt gebruikt.<br><br> **Belang rijk**: Zorg ervoor dat het Azure Database Migration Service exemplaar dat wordt verwijderd, geen actieve activiteiten heeft.<br><br> Nadat alle projecten en activiteiten die aan het Azure Database Migration Service exemplaar zijn gekoppeld, zijn verwijderd, kunt u het service-exemplaar verwijderen. De NIC die wordt gebruikt door het service-exemplaar wordt automatisch gereinigd als onderdeel van de service verwijdering. |

## <a name="connection-error-when-using-expressroute"></a>Verbindings fout bij het gebruik van ExpressRoute

Als u verbinding wilt maken met de bron in de wizard Azure Database Migration-serviceproject, mislukt de verbinding na een langere time-out, als de bron gebruikmaakt van ExpressRoute voor connectiviteit.

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Wanneer u [ExpressRoute](https://azure.microsoft.com/services/expressroute/)gebruikt, [moet](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) Azure database Migration service drie service-eind punten inrichten op het Virtual Network subnet dat is gekoppeld aan de service:<br> --Service Bus-eind punt<br> --Eind punt van opslag<br> --Doel database-eind punt (bijvoorbeeld SQL-eind punt, Cosmos DB-eind punt)<br><br><br><br><br> | [Schakel](https://docs.microsoft.com/azure/dms/tutorial-sql-server-azure-sql-online) de vereiste service-eind punten in voor de ExpressRoute-connectiviteit tussen de bron en de Azure database Migration service. <br><br><br><br><br><br><br><br> |

## <a name="lock-wait-timeout-error-when-migrating-a-mysql-database-to-azure-db-for-mysql"></a>Fout tijdens vergren deling wacht tijd bij het migreren van een MySQL-data base naar Azure DB voor MySQL

Wanneer u via Azure Database Migration Service een MySQL-data base migreert naar een Azure Database for MySQL-exemplaar, mislukt de migratie met de volgende fout in de vergrendelings wachttijd:

* **Fout**: Database migratie fout-kan het bestand niet laden-kan het laad proces voor bestand ' n ' niet starten RetCode: SQL_ERROR SqlState: HY000 NativeError: 1205 bericht: [MySQL] [ODBC-stuur programma] [mysqld] wacht tijd voor vergren deling is overschreden; trans actie opnieuw starten

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Deze fout treedt op wanneer de migratie is mislukt vanwege een time-out tijdens de migratie. | Overweeg de waarde van de server parameter **' innodb_lock_wait_timeout '** te verhogen. De hoogst toegestane waarde is 1073741824. |

## <a name="error-connecting-to-source-sql-server-when-using-dynamic-port-or-named-instance"></a>Fout bij het verbinden met de bron SQL Server bij het gebruik van een dynamische poort of een benoemd exemplaar

Wanneer u probeert om Azure Database Migration Service verbinding te maken met SQL Server bron die wordt uitgevoerd op een benoemd exemplaar of een dynamische poort, mislukt de verbinding met deze fout:

* **Fout**:-1-SQL-verbinding is mislukt. Een netwerkgerelateerde of exemplaarspecifieke fout is opgetreden bij het maken van een verbinding met SQL Server. De server wordt niet gevonden of toegang tot de server is niet mogelijk. Controleer of de naam van het exemplaar juist is en of SQL Server is geconfigureerd om externe verbindingen toe te staan. providers SQL-netwerk interfaces, fout: 26-fout bij het vinden van Server/exemplaar opgegeven.

| Oorzaak         | Oplossing    |
| ------------- | ------------- |
| Dit probleem treedt op wanneer het bron SQL Server exemplaar waarmee Azure Database Migration Service verbinding probeert te maken, een dynamische poort heeft of een benoemd exemplaar gebruikt. De SQL Server Browser-service luistert naar UDP-poort 1434 voor binnenkomende verbindingen met een benoemd exemplaar of bij het gebruik van een dynamische poort. De dynamische poort kan elke keer worden gewijzigd SQL Server service opnieuw wordt gestart. U kunt de dynamische poort controleren die is toegewezen aan een exemplaar via netwerk configuratie in SQL Server Configuration Manager.<br><br><br> |Controleer of Azure Database Migration Service verbinding kan maken met de bron SQL Server Browser service op UDP-poort 1434 en het SQL Server exemplaar via de dynamisch toegewezen TCP-poort die van toepassing is. |

## <a name="additional-known-issues"></a>Meer bekende problemen

* [Bekende problemen/migratie beperkingen met online migraties naar Azure SQL Database](https://docs.microsoft.com/azure/dms/known-issues-azure-sql-online)
* [Bekende problemen/migratie beperkingen met online migraties naar Azure Database for MySQL](https://docs.microsoft.com/azure/dms/known-issues-azure-mysql-online)
* [Bekende problemen/migratie beperkingen met online migraties naar Azure Database for PostgreSQL](https://docs.microsoft.com/azure/dms/known-issues-azure-postgresql-online)

## <a name="next-steps"></a>Volgende stappen

* Bekijk het artikel [Azure database Migration Service Power shell](https://docs.microsoft.com/powershell/module/azurerm.datamigration/?view=azurermps-6.13.0#data_migration).
* Bekijk het artikel [How to Configure Server para meters in azure database for MySQL met behulp van de Azure Portal](https://docs.microsoft.com/azure/mysql/howto-server-parameters).
* Bekijk het artikel [overzicht van de vereisten voor het gebruik van Azure database Migration service](https://docs.microsoft.com/azure/dms/pre-reqs).
* Raadpleeg de [Veelgestelde vragen over het gebruik van Azure database Migration service](https://docs.microsoft.com/azure/dms/faq).
