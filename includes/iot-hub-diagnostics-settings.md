---
title: bestand opnemen
description: bestand opnemen
services: iot-hub
author: dominicbetts
ms.service: iot-hub
ms.topic: include
ms.date: 02/20/2019
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: 2498711a5b7e5bce29cd0054ba40257f8f996d43
ms.sourcegitcommit: 55f7fc8fe5f6d874d5e886cb014e2070f49f3b94
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/25/2019
ms.locfileid: "71266820"
---
### <a name="enable-logging-with-diagnostics-settings"></a>Logboek registratie met Diagnostische instellingen inschakelen

[!INCLUDE [updated-for-az](./updated-for-az.md)]

1. Aanmelden bij de [Azure-portal](https://portal.azure.com) en navigeer naar uw IoT-hub.

2. Selecteer **instellingen voor diagnostische gegevens**.

3. Selecteer **diagnostische gegevens inschakelen**.

   ![Diagnostische gegevens inschakelen](./media/iot-hub-diagnostics-settings/turnondiagnostics.png)

4. Geef de diagnostische instellingen een naam.

5. Kies waar u de logboeken wilt verzenden. U kunt een combi natie van de drie opties selecteren:

   * Archiveren naar een opslagaccount
   * Streamen naar een Event Hub
   * Verzenden naar Log Analytics

6. Kies welke bewerkingen u wilt bewaken en schakel Logboeken in voor deze bewerkingen. De bewerkingen waarvan de diagnostische instellingen kunnen rapporteren, zijn:

   * Verbindingen
   * Telemetrie van apparaat
   * Cloud-naar-apparaat-berichten
   * Bewerkingen voor apparaat-id's
   * Uploaden van bestanden
   * Berichtroutering
   * Dubbele bewerkingen van het Cloud naar het apparaat
   * Dubbele bewerkingen van het apparaat naar de Cloud
   * Dubbele bewerkingen
   * Taak bewerkingen
   * Directe methoden  
   * Gedistribueerde tracering (preview-versie)
   * Configuraties
   * Streams van apparaat
   * Metrische gegevens van apparaat

6. Sla de nieuwe instellingen op. 

Als u Diagnostische instellingen wilt inschakelen met Power shell, gebruikt u de volgende code:

```azurepowershell
Connect-AzAccount
Select-AzSubscription -SubscriptionName <subscription that includes your IoT Hub>
Set-AzDiagnosticSetting -ResourceId <your resource Id> -ServiceBusRuleId <your service bus rule Id> -Enabled $true
```

Nieuwe instellingen van kracht in ongeveer 10 minuten. Daarna worden logboeken weer gegeven in het geconfigureerde archief doel op de Blade **Diagnostische instellingen** . Zie [logboek gegevens verzamelen en gebruiken van uw Azure-resources](../articles/azure-monitor/platform/resource-logs-overview.md)voor meer informatie over het configureren van diagnostiek.
