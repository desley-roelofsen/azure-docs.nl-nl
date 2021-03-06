---
title: Meer items of records met paginering ophalen
description: Paginering instellen om de standaard limiet voor de pagina grootte te overschrijden voor connector acties in Azure Logic Apps
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 04/11/2019
ms.openlocfilehash: 75d9660eb35b5d7ddc644d177c11ae489e2853dc
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74792121"
---
# <a name="get-more-data-items-or-records-by-using-pagination-in-azure-logic-apps"></a>Meer gegevens, items of records ophalen met behulp van paginering in Azure Logic Apps

Wanneer u gegevens, items of records ophaalt met behulp van een connector actie in [Azure Logic apps](../logic-apps/logic-apps-overview.md), krijgt u mogelijk resultaten sets zo groot dat de actie niet alle resultaten tegelijk retourneert. Met sommige acties kan het aantal resultaten de standaard pagina grootte van de connector overschrijden. In dit geval retourneert de actie alleen de eerste pagina met resultaten. De standaard pagina grootte voor de actie **rijen ophalen** van SQL Server-connector is bijvoorbeeld 2048, maar kan variëren op basis van andere instellingen.

Met sommige acties kunt u een *paginerings* instelling inschakelen zodat uw logische app meer resultaten kan ophalen tot aan de paginerings limiet, maar deze resultaten als één bericht retourneert wanneer de actie is voltooid. Wanneer u paginering gebruikt, moet u een *drempel* waarde opgeven. Dit is het doel aantal resultaten dat door de actie moet worden geretourneerd. Met de actie worden resultaten opgehaald totdat de opgegeven drempel waarde wordt bereikt. Wanneer het totale aantal items kleiner is dan de opgegeven drempel waarde, haalt de actie alle resultaten op.

Als u de paginerings instelling inschakelt, worden de pagina's met resultaten opgehaald op basis van de pagina grootte van een connector. Dit kan betekenen dat er soms meer resultaten worden weer gegeven dan de opgegeven drempel waarde. Wanneer u bijvoorbeeld de actie **rijen ophalen** van SQL Server gebruikt, die de paginerings instelling ondersteunt:

* De standaard pagina grootte van de actie is 2048 records per pagina.
* Stel dat u 10.000 records hebt en 5000 records opgeeft als mini maal.
* Paginering haalt pagina's van records op, zodat er ten minste het opgegeven minimum wordt opgehaald. de actie retourneert 6144 records (3 pagina's x 2048 records), niet 5000 records.

Hier volgt een lijst met slechts enkele Connect oren waar u de standaard pagina grootte voor specifieke acties kunt overschrijden:

* [Azure Blob Storage](https://docs.microsoft.com/connectors/azureblob/)
* [Dynamics 365](https://docs.microsoft.com/connectors/dynamicscrmonline/)
* [Excel](https://docs.microsoft.com/connectors/excel/)
* [HTTP](https://docs.microsoft.com/azure/connectors/connectors-native-http)
* [IBM DB2](https://docs.microsoft.com/connectors/db2/)
* [Microsoft Teams](https://docs.microsoft.com/connectors/teams/)
* [Oracle Database](https://docs.microsoft.com/connectors/oracle/)
* [Salesforce](https://docs.microsoft.com/connectors/salesforce/)
* [SharePoint](https://docs.microsoft.com/connectors/sharepointonline/)
* [SQL Server](https://docs.microsoft.com/connectors/sql/)

## <a name="prerequisites"></a>Vereisten

* Een Azure-abonnement. Als u nog geen Azure-abonnement hebt, [meldt u zich aan voor een gratis Azure-account](https://azure.microsoft.com/free/).

* De logische app en de actie waar u paginering wilt inschakelen. Als u geen logische app hebt, raadpleegt u [Quick Start: uw eerste logische app maken](../logic-apps/quickstart-create-first-logic-app-workflow.md).

## <a name="turn-on-pagination"></a>Paginering inschakelen

Als u wilt bepalen of een actie paginering ondersteunt in de ontwerp functie voor logische apps, controleert u de instellingen van de actie voor de **paginerings** instelling. In dit voor beeld ziet u hoe u paginering inschakelt in de actie **rijen ophalen** van de SQL Server.

1. Klik in de rechter bovenhoek van de actie op de knop met weglatings tekens ( **...** ) en selecteer **instellingen**.

   ![De instellingen van de actie openen](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings.png)

   Als de actie paginering ondersteunt, wordt in de actie de **paginerings** instelling weer gegeven.

1. Wijzig de **paginerings** instelling van **uit** in **op aan**. Geef in de eigenschap **drempel** waarde een geheel getal op voor het doel aantal resultaten dat moet worden geretourneerd door de actie.

   ![Geef het minimum aantal resultaten op dat moet worden geretourneerd](./media/logic-apps-exceed-default-page-size-with-pagination/sql-action-settings-pagination.png)

1. Wanneer u klaar bent, kiest u **gereed**.

## <a name="workflow-definition---pagination"></a>Werk stroom definitie-paginering

Wanneer u paginering inschakelt voor een actie die ondersteuning biedt voor deze mogelijkheid, bevat de werk stroom definitie van de logische app de eigenschap `"paginationPolicy"` in combi natie met de eigenschap `"minimumItemCount"` in de eigenschap `"runtimeConfiguration"` van die actie, bijvoorbeeld:

```json
"actions": {
   "HTTP": {
      "inputs": {
         "method": "GET",
         "uri": "https://www.testuri.com"
      },
      "runAfter": {},
      "runtimeConfiguration": {
         "paginationPolicy": {
            "minimumItemCount": 1000
         }
      },
      "type": "Http"
   }
},
```

## <a name="get-support"></a>Krijg ondersteuning

Ga naar het [Azure Logic Apps forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps) (Forum voor Azure Logic Apps) als u vragen hebt.
