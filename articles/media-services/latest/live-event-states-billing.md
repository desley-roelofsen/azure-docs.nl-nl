---
title: LiveEvent Staten en facturering in Azure Media Services | Microsoft Docs
description: In dit onderwerp vindt u een overzicht van Azure Media Services statussen en facturering van LiveEvent.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: ne
ms.topic: article
ms.date: 10/24/2019
ms.author: juliako
ms.openlocfilehash: af3d4b51dadfaa99a166ca0ce475c5a110d8f6e8
ms.sourcegitcommit: 4c3d6c2657ae714f4a042f2c078cf1b0ad20b3a4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/25/2019
ms.locfileid: "72933686"
---
# <a name="live-event-states-and-billing"></a>Live gebeurtenis statussen en facturering

In Azure Media Services begint de facturering van een live gebeurtenis zodra de status overgangen **wordt uitgevoerd**. Als u de live-gebeurtenis van facturering wilt stoppen, moet u de live-gebeurtenis beëindigen.

Wanneer **LiveEventEncodingType** voor uw [live-gebeurtenis](https://docs.microsoft.com/rest/api/media/liveevents) is ingesteld op Standard of Premium1080p, wordt met Media Services automatisch elke live gebeurtenis afgesloten die in de **actieve** status 12 uur na de invoer is verdwenen en er geen **Live uitvoer** is s wordt uitgevoerd. Er worden echter nog steeds kosten in rekening gebracht voor de tijd dat de live-gebeurtenis de status **actief** heeft.

> [!NOTE]
> Passthrough-Live-gebeurtenissen worden niet automatisch afgesloten en moeten expliciet worden gestopt door de API om buitensporige facturering te voor komen. 

## <a name="states"></a>Staten

De live-gebeurtenis kan een van de volgende statussen hebben.

|Staat|Beschrijving|
|---|---|
|**Gestopt**| Dit is de begin status van de live gebeurtenis na het maken (tenzij auto start is ingesteld op True.) Er vindt geen facturering plaats in deze status. Bij deze status kunnen de eigenschappen van de livegebeurtenis worden bijgewerkt, maar is streaming niet toegestaan.|
|**Ingang**| De live gebeurtenis wordt gestart en er worden resources toegewezen. Er vindt geen facturering plaats in deze status. Updates of streaming zijn niet toegestaan tijdens deze status. Als er een fout optreedt, keert de live-gebeurtenis terug naar de status gestopt.|
|**Wordt uitgevoerd**| De live-gebeurtenis resources zijn toegewezen, opname-en preview-Url's zijn gegenereerd en kunnen live streams ontvangen. Op dit moment is facturering actief. U moet expliciet Stop aanroepen in de resource van de livegebeurtenis om verdere facturering stop te zetten.|
|**Stoppen**| De live gebeurtenis wordt gestopt en de inrichting van de resources wordt ongedaan gemaakt. Er vindt geen facturering plaats in deze tijdelijke status. Updates of streaming zijn niet toegestaan tijdens deze status.|
|**Verwijder**| De livegebeurtenis wordt verwijderd. Er vindt geen facturering plaats in deze tijdelijke status. Updates of streaming zijn niet toegestaan tijdens deze status.|

## <a name="next-steps"></a>Volgende stappen

- [Overzicht van live streamen](live-streaming-overview.md)
- [Zelf studie over live streamen](stream-live-tutorial-with-api.md)
