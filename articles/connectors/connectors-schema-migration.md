---
title: Apps migreren naar het laatste schema
description: Logische apps migreren naar de meest recente schema versie
services: logic-apps
ms.suite: integration
ms.reviewer: klam, logicappspm
ms.topic: article
ms.date: 08/25/2018
ms.openlocfilehash: dc967c90f28b29a63a1f26d0aae86b6d769c1ccf
ms.sourcegitcommit: 76b48a22257a2244024f05eb9fe8aa6182daf7e2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74786925"
---
# <a name="migrate-logic-apps-to-latest-schema-version"></a>Logische apps migreren naar de meest recente schema versie

Voer de volgende stappen uit om uw bestaande Logic apps naar het nieuwste schema te verplaatsen: 

1. Open in de [Azure Portal](https://portal.azure.com)uw logische app in de ontwerp functie voor logische apps.

2. Kies **overzicht**in het menu van de logische app. Kies op de werk balk de optie **schema bijwerken**.

   > [!NOTE]
   > Wanneer u **schema bijwerken**kiest, worden de migratie stappen automatisch door Azure Logic apps uitgevoerd en wordt de code-uitvoer voor u verstrekt. U kunt deze uitvoer gebruiken voor het bijwerken van de definitie van de logische app. Zorg er echter voor dat u de aanbevolen procedures volgt, zoals beschreven in de sectie **Aanbevolen procedures** .

   ![Schema bijwerken](./media/connectors-schema-migration/update-schema.png)

   De pagina schema bijwerken wordt weer gegeven en toont een koppeling naar een document waarin de verbeteringen in het nieuwe schema worden beschreven.

## <a name="best-practices"></a>Aanbevolen procedures

Hier volgen enkele aanbevolen procedures voor het migreren van uw Logic apps naar de meest recente schema versie:

* Kopieer het gemigreerde script naar een nieuwe logische app. Overschrijf de oude versie pas nadat u de tests hebt voltooid en controleer of uw gemigreerde app werkt zoals verwacht.

* Test uw logische app **voordat** u deze in productie neemt.

* Nadat u de migratie hebt voltooid, start u de Logic apps zo dat mogelijk wordt uitgevoerd met behulp van de [beheerde api's](../connectors/apis-list.md) . Begin bijvoorbeeld met behulp van Dropbox v2, overal waar u DropBox v1 gebruikt.

## <a name="next-steps"></a>Volgende stappen

* Meer informatie over het [hand matig migreren van uw Logic apps](../logic-apps/logic-apps-schema-2015-08-01.md)
