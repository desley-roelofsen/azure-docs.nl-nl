---
title: Status van het databasemigratiescenario
titleSuffix: Azure Database Migration Service
description: Meer informatie over de status van de migratie scenario's die door Azure Database Migration Service worden ondersteund.
services: database-migration
author: HJToland3
ms.author: jtoland
manager: craigg
ms.reviewer: craigg
ms.service: dms
ms.workload: data-services
ms.custom: mvc
ms.topic: article
ms.date: 09/05/2019
ms.openlocfilehash: 03fb6a1aff62df878417da55a7da7b42186b3462
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75437677"
---
# <a name="status-of-migration-scenarios-supported-by-azure-database-migration-service"></a>Status van migratie scenario's die door Azure Database Migration Service worden ondersteund

Azure Database Migration Service is ontworpen ter ondersteuning van verschillende migratie scenario's (bron/doel paren) voor zowel offline (eenmalige) als online (continue synchronisatie)-migraties. De dekking van het scenario dat door Azure Database Migration Service wordt gegeven, wordt verlengd over een bepaalde periode. Nieuwe scenario's worden regel matig toegevoegd. In dit artikel worden migratie scenario's beschreven die momenteel door Azure Database Migration Service worden ondersteund en de status (persoonlijke preview, open bare preview of algemene Beschik baarheid) voor elk scenario.

## <a name="offline-versus-online-migrations"></a>Offline versus online migraties

Met Azure Database Migration Service kunt u een offline-of online migratie uitvoeren. Met *offline* migraties begint de uitval tijd van de toepassing op het moment dat de migratie wordt gestart. Gebruik een *online* migratie om de downtime te beperken tot de tijd die nodig is om naar de nieuwe omgeving te knippen wanneer de migratie is voltooid. Het is raadzaam om een offline migratie te testen om te bepalen of de downtime acceptabel is. Als dat niet het geval is, kunt u een online migratie uitvoeren.

## <a name="migration-scenario-status"></a>Status van migratie scenario

De status van de migratie scenario's die door Azure Database Migration Service worden ondersteund, is afhankelijk van de tijd. Over het algemeen worden scenario's eerst uitgebracht in **een persoonlijke preview**. Klanten die deel uitmaken van de preview-versie van een persoonlijke voor beeld, moeten een benoeming indienen via de [DMS preview-site](https://aka.ms/dms-preview). Na een persoonlijke Preview wordt de status van het scenario gewijzigd in **open bare preview**. Azure Database Migration Service gebruikers kunnen migratie scenario's in open bare preview-versie rechtstreeks vanuit de gebruikers interface uitproberen. Het is niet nodig om u aan te melden.  Migratie scenario's in de open bare preview-versie zijn echter mogelijk niet beschikbaar in alle regio's en kunnen aanvullende wijzigingen ondergaan voordat de definitieve versie wordt uitgebracht. Na de open bare Preview wordt de status van het scenario gewijzigd in **Algemeen Beschik baarheid**. Algemene Beschik baarheid (GA) is de laatste release status en de functionaliteit is voltooid en toegankelijk voor alle gebruikers.

## <a name="migration-scenario-support"></a>Ondersteuning voor migratie scenario's

In de volgende tabellen ziet u welke migratie scenario's worden ondersteund bij het gebruik van Azure Database Migration Service.

> [!NOTE]
> Als een scenario dat hieronder wordt weer gegeven, niet wordt weer gegeven in de gebruikers interface, neemt u contact op met de alias voor [Azure data base-migraties](mailto:AskAzureDatabaseMigrations@service.microsoft.com) voor aanvullende informatie.

> [!IMPORTANT]
> Als u alle scenario's wilt weer geven die momenteel door Azure Database Migration Service worden ondersteund in de persoonlijke preview, raadpleegt u de [DMS preview-site](https://aka.ms/dms-preview).

### <a name="offline-one-time-migration-support"></a>Offline migratie-ondersteuning (eenmalig)

De volgende tabel toont Azure Database Migration Service ondersteuning voor offline migraties.

| Kiezen  | Bron | Ondersteuning | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL |  |  |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL |  |  |
|   | Oracle |  |   |
| **Azure SQL-VM** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | Oracle |   |   |
| **Azure Cosmos DB** | MongoDB | ✔ | Algemene beschikbaarheid |
| **Azure DB voor MySQL** | MySQL |   |   |
|   | RDS MySQL |   |   |
| **Azure DB voor PostgreSQL** | PostgreSQL |  |
|  | RDS PostgreSQL |   |   |

### <a name="online-continuous-sync-migration-support"></a>Ondersteuning voor migratie van online (continue synchronisatie)

De volgende tabel toont Azure Database Migration Service ondersteuning voor online migraties.

| Kiezen  | Bron | Ondersteuning | Status |
| ------------- | ------------- |:-------------:|:-------------:|
| **Azure SQL DB** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL | ✔ | Algemene beschikbaarheid |
|   | Oracle |  |  |
| **Azure SQL DB MI** | SQL Server | ✔ | Algemene beschikbaarheid |
|   | RDS SQL | ✔ | Algemene beschikbaarheid |
|   | Oracle | ✔ | Persoonlijke preview |
| **Azure SQL-VM** | SQL Server |   |   |
|   | Oracle  |  |  |
| **Azure Cosmos DB** | MongoDB | ✔ | Algemene beschikbaarheid |
| **Azure DB voor MySQL** | MySQL | ✔ | Algemene beschikbaarheid |
|   | RDS MySQL | ✔ | Algemene beschikbaarheid |
| **Azure DB voor PostgreSQL** | PostgreSQL | ✔ | Algemene beschikbaarheid |
|   | RDS PostgreSQL | ✔ | Algemene beschikbaarheid |
|   | Oracle | ✔ | Openbare preview |

## <a name="next-steps"></a>Volgende stappen

Zie het artikel [Wat is de Azure database Migration service](dms-overview.md)voor een overzicht van Azure database Migration service en regionale Beschik baarheid.
