---
title: Waarschuwingen configureren-grootschalige (Citus)-Azure Database for PostgreSQL
description: In dit artikel wordt beschreven hoe u metrische waarschuwingen kunt configureren en benaderen voor Azure Database for PostgreSQL-grootschalige (Citus)
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: 68a830f344023967f07ab809d67833f99e4e2958
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74977604"
---
# <a name="use-the-azure-portal-to-set-up-alerts-on-metrics-for-azure-database-for-postgresql---hyperscale-citus"></a>Gebruik de Azure Portal om waarschuwingen in te stellen voor de Azure Database for PostgreSQL-grootschalige (Citus)

In dit artikel wordt beschreven hoe u Azure Database for PostgreSQL waarschuwingen instelt met behulp van de Azure Portal. U kunt een waarschuwing ontvangen op basis van metrische bewakings gegevens voor uw Azure-Services.

We stellen een waarschuwing in om te activeren wanneer de waarde van een opgegeven metriek een drempel overschrijdt. De waarschuwing wordt geactiveerd wanneer aan de voor waarde wordt voldaan en blijft daarna verder.

U kunt een waarschuwing configureren om de volgende acties uit te voeren wanneer deze wordt geactiveerd:
* E-mail meldingen verzenden naar de service beheerder en mede beheerders.
* E-mail verzenden naar extra e-mail berichten die u opgeeft.
* Een webhook aanroepen.

U kunt informatie over waarschuwings regels configureren en ophalen met behulp van:
* [Azure-portal](../azure-monitor/platform/alerts-metric.md#create-with-azure-portal)
* [Azure CLI](../azure-monitor/platform/alerts-metric.md#with-azure-cli)
* [Azure Monitor REST API](https://docs.microsoft.com/rest/api/monitor/metricalerts)

## <a name="create-an-alert-rule-on-a-metric-from-the-azure-portal"></a>Een waarschuwings regel maken op basis van een metriek van de Azure Portal
1. Selecteer in de [Azure Portal](https://portal.azure.com/)de Azure database for postgresql server die u wilt bewaken.

2. Selecteer in de sectie **bewaking** van de zijbalk **waarschuwingen** zoals wordt weer gegeven:

   ![Waarschuwings regels selecteren](./media/howto-hyperscale-alert-on-metric/2-alert-rules.png)

3. Selecteer **nieuwe waarschuwings regel** (+ pictogram).

4. De pagina **regel maken** wordt geopend, zoals hieronder wordt weer gegeven. Vul de vereiste gegevens in:

   ![Formulier voor metrische waarschuwing toevoegen](./media/howto-hyperscale-alert-on-metric/4-add-rule-form.png)

5. Selecteer in de sectie **voor waarde** de optie **toevoegen**.

6. Selecteer een waarde in de lijst met signalen waarop u wilt worden gewaarschuwd. In dit voor beeld selecteert u ' opslag percentage '.
   
   ![Metrische waarde selecteren](./media/howto-hyperscale-alert-on-metric/6-configure-signal-logic.png)

7. De waarschuwings logica configureren:

    * **Operator** (bijvoorbeeld ' Groter dan ')
    * **Drempel waarde** (bijvoorbeeld 85 procent)
    * De **mate waarin** de metrische regel moet worden voldaan voordat de waarschuwing wordt geactiveerd (bijvoorbeeld "In de afgelopen 30 minuten")
    * en **frequentie van de evaluatie** (bijvoorbeeld "1 minuut")
   
   Selecteer **gereed** wanneer u klaar bent.

   ![Metrische waarde selecteren](./media/howto-hyperscale-alert-on-metric/7-set-threshold-time.png)

8. Selecteer in de sectie **actie groepen** de optie **nieuwe maken** om een nieuwe groep te maken voor het ontvangen van meldingen over de waarschuwing.

9. Vul het formulier actie groep toevoegen in met een naam, korte naam, abonnement en resource groep.

    ![Actie groep](./media/howto-hyperscale-alert-on-metric/9-add-action-group.png)

10. Een actie type voor **e-mail/SMS/push/Voice** configureren.
    
    Kies e-mail Azure Resource Manager rol om meldingen te verzenden naar eigen aars, mede werkers en lezers van het abonnement.
   
    Selecteer **OK** wanneer u klaar bent.

    ![Actie groep](./media/howto-hyperscale-alert-on-metric/10-action-group-type.png)

11. Geef een naam, beschrijving en ernst voor de waarschuwings regel op.

    ![Actie groep](./media/howto-hyperscale-alert-on-metric/11-name-description-severity.png) 

12. Selecteer **waarschuwings regel maken** om de waarschuwing te maken.

    Binnen een paar minuten is de waarschuwing actief en worden triggers zoals eerder beschreven.

## <a name="manage-your-alerts"></a>De waarschuwingen beheren

Zodra u een waarschuwing hebt gemaakt, kunt u deze selecteren en de volgende acties uitvoeren:

* Een grafiek weer geven met de metrische drempel waarde en de werkelijke waarden van de vorige dag die relevant is voor deze waarschuwing.
* **Bewerk** of **Verwijder** de waarschuwings regel.
* **Schakel** de waarschuwing **in** of uit als u het ontvangen van meldingen tijdelijk wilt stoppen of hervatten.

## <a name="next-steps"></a>Volgende stappen
* Meer informatie over het [configureren van webhooks in waarschuwingen](../azure-monitor/platform/alerts-webhooks.md).
* Bekijk een [overzicht van de verzameling metrische gegevens](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md) om te controleren of uw service beschikbaar is en reageert.
