---
title: Limieten-Azure Database for PostgreSQL-één server
description: In dit artikel worden de beperkingen in Azure Database for PostgreSQL-één server beschreven, zoals het aantal opties voor verbinding en opslag engine.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/25/2019
ms.custom: fasttrack-edit
ms.openlocfilehash: d74206ebdf35a8f5b353553cb89e954cb2313611
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74768534"
---
# <a name="limits-in-azure-database-for-postgresql---single-server"></a>Limieten in Azure Database for PostgreSQL-één server
In de volgende secties worden de capaciteits-en functionele limieten in de database service beschreven. Zie het artikel over de [prijs categorieën](concepts-pricing-tiers.md) als u meer wilt weten over resource-lagen (compute, geheugen, opslag).


## <a name="maximum-connections"></a>Maximum aantal verbindingen
Het maximum aantal verbindingen per prijs categorie en vCores is als volgt: 

|**Prijscategorie**| **vCore (s)**| **Maximum aantal verbindingen** | **Maximum aantal gebruikers verbindingen** |
|---|---|---|---|
|Basic| 1| 55 | 50|
|Basic| 2| 105 | 100|
|Algemeen doel| 2| 150| 145|
|Algemeen doel| 4| 250| 245|
|Algemeen doel| 8| 480| 475|
|Algemeen doel| 16| 950| 945|
|Algemeen doel| 32| 1500| 1495|
|Algemeen doel| 64| 1900| 1895|
|Geoptimaliseerd geheugen| 2| 300| 295|
|Geoptimaliseerd geheugen| 4| 500| 495|
|Geoptimaliseerd geheugen| 8| 960| 955|
|Geoptimaliseerd geheugen| 16| 1900| 1895|
|Geoptimaliseerd geheugen| 32| 1987| 1982|

Wanneer verbindingen de limiet overschrijden, wordt mogelijk de volgende fout weer gegeven:
> Onherstelbare fout: er zijn al te veel clients

Het Azure-systeem vereist vijf verbindingen om de Azure Database for PostgreSQL server te bewaken. 

## <a name="functional-limitations"></a>Functionele beperkingen
### <a name="scale-operations"></a>Schaal bewerkingen
- Dynamisch schalen naar en van de basis prijs categorieën wordt momenteel niet ondersteund.
- Het verminderen van de grootte van de server opslag wordt momenteel niet ondersteund.

### <a name="server-version-upgrades"></a>Server versie-upgrades
- Automatische migratie tussen de primaire data base-engine versies wordt momenteel niet ondersteund. Als u wilt upgraden naar de volgende primaire versie, neemt u een [dump op en herstelt](./howto-migrate-using-dump-and-restore.md) u deze op een server die is gemaakt met de nieuwe engine versie.

> Houd er rekening mee dat het [postgresql-versie beleid](https://www.postgresql.org/support/versioning/) als een _primaire versie_ -upgrade voor postgresql versie 10 als een verhoging van het eerste _of_ tweede nummer moet worden uitgevoerd (bijvoorbeeld 9,5 tot 9,6 werd beschouwd als een _primaire_ versie-upgrade).
> Vanaf versie 10 wordt alleen een wijziging in het eerste nummer beschouwd als een primaire versie-upgrade (bijvoorbeeld 10,0 tot 10,1 is een _secundaire_ versie-upgrade en 10 tot 11 is een _primaire_ versie-upgrade).

### <a name="vnet-service-endpoints"></a>VNet-service-eind punten
- Ondersteuning voor VNet-service-eind punten is alleen voor servers met Algemeen en geoptimaliseerd voor geheugen.

### <a name="restoring-a-server"></a>Een server herstellen
- Wanneer u de functie PITR gebruikt, wordt de nieuwe server gemaakt met dezelfde prijs categorie configuraties als de server waarop deze is gebaseerd.
- De nieuwe server die tijdens het herstellen is gemaakt, bevat niet de firewall regels die op de oorspronkelijke server aanwezig waren. Firewall regels moeten afzonderlijk worden ingesteld voor deze nieuwe server.
- Het herstellen van een verwijderde server wordt niet ondersteund.

### <a name="utf-8-characters-on-windows"></a>UTF-8-tekens in Windows
- In sommige scenario's worden UTF-8-tekens niet volledig ondersteund in open-source PostgreSQL in Windows, wat van invloed is op Azure Database for PostgreSQL. Raadpleeg de thread on [Bug #15476 in de postgresql-Archive](https://www.postgresql-archive.org/BUG-15476-Problem-on-show-trgm-with-4-byte-UTF-8-characters-td6056677.html) voor meer informatie.

## <a name="next-steps"></a>Volgende stappen
- [Meer informatie over wat er beschikbaar is in elke prijs categorie](concepts-pricing-tiers.md)
- Meer informatie over [ondersteunde versies van de postgresql-data base](concepts-supported-versions.md)
- Lees [hoe u een back-up maakt en een server herstelt in azure database for PostgreSQL met behulp van de Azure Portal](howto-restore-server-portal.md)
