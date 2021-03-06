---
title: bestand opnemen
description: bestand opnemen
services: cost-management
author: bandersmsft
ms.service: cost-management
ms.topic: include
ms.date: 09/17/2018
ms.author: banders
manager: dougeby
ms.custom: include file
ms.openlocfilehash: 1ffa56caebf16b588dffaba249a844915f9f44c7
ms.sourcegitcommit: 3e98da33c41a7bbd724f644ce7dedee169eb5028
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 06/18/2019
ms.locfileid: "67175777"
---
## <a name="view-cost-data"></a>Kostengegevens weergeven

Kostenbeheer van Azure door Cloudyn biedt u toegang tot al uw cloudresourcegegevens. Via de dashboardrapporten kunt u de standaard- en aangepaste rapporten vinden in een tabbladweergave. Hier volgen enkele voorbeelden van een populair dashboard en een rapport waarin u onmiddellijk de kostengegevens kunt zien.

![Management Dashboard](./media/cost-management-create-account-view-data/mgt-dash.png)

In dit voorbeeld toont het Management Dashboard de geconsolideerde kosten voor het bedrijf Contoso van al hun cloudresources. Contoso maakt gebruik van Azure, AWS en Google. Dashboards bieden overzichtelijke informatie en een snelle manier om te navigeren door rapporten.  

Als u niet zeker weet wat het doel is van een rapport in een dashboard, beweegt u de muisaanwijzer over het symbool **i** om een uitleg te zien. Klik op een rapport in een dashboard om het volledige rapport weer te geven.

U kunt ook rapporten weergeven via het rapportenmenu boven aan de portal. Laten we eens de uitgaven van Concorde aan Azure-resources in de afgelopen 30 dagen bekijken. Klik op **Costs** > **Cost Analysis** > **Actual Cost Analysis**. Wis alle waarden als er waarden zijn ingesteld voor labels, groepen of filters in het rapport.

![Actual Cost Analysis](./media/cost-management-create-account-view-data/actual-cost-01.png)

In dit voorbeeld zijn de totale kosten $ 122.273 en is het budget $ 290.000.

Nu gaan we de rapportindeling wijzigen en groepen en filters instellen om de resultaten voor Azure-kosten te verfijnen. Stel **Date Range** in op de afgelopen 30 dagen. Klik op het kolomsymbool in de rechterbovenhoek om het rapport op te maken als een staafdiagram en selecteer **Provider** onder Groups. Stel het filter voor **Provider** vervolgens in op **Azure**.

![Gefilterde Actual Cost Analysis](./media/cost-management-create-account-view-data/actual-cost-02.png)

In dit voorbeeld bedragen de totale kosten van Azure-resources in de afgelopen 30 dagen $ 3309.

Klik met de rechtermuisknop op de balk Provider (Azure) en zoom in op **Resource types**.

![inzoomen](./media/cost-management-create-account-view-data/actual-cost-03.png)

In de volgende afbeelding ziet u de kosten die Contoso heeft gemaakt voor Azure-resources. Het totaalbedrag is $ 3309. In dit voorbeeld is ongeveer de helft van de kosten voor Standard_A1-VM's en de andere helft voor verschillende Azure-services en VM-exemplaren.

![resourcetypen](./media/cost-management-create-account-view-data/actual-cost-04.png)

Klik met de rechtermuisknop op een resourcetype en selecteer **Cost Entities** om de kostenentiteiten en de services weer te geven die de resource hebt gebruikt. In de volgende voorbeeldafbeelding is lokaal redundante opslag ingesteld als het resourcetype. Contoso|Azure/Storage heeft $ 15,65 verbruikt. Engineering|Azure Storage heeft $ 164,25 verbruikt. Shared Infrastructure|Azure/Storage heeft $ 116,58 verbruikt. De totale kosten voor de services bedroeg $ 296.

![kostenentiteiten en services](./media/cost-management-create-account-view-data/actual-cost-05.png)

Zie [Analyzing your cloud billing data with Azure Cost Management by Cloudyn](https://youtu.be/G0pvI3iLH-Y) (Uw factureringsgegevens voor de cloud analyseren met Azure Cost Management door Cloudyn) als u een video voor zelfstudie wilt bekijken over uw factureringsgegevens voor de cloud.
