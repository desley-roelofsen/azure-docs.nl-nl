---
title: Load Balancer TCP opnieuw instellen bij inactief in azure
titleSuffix: Azure Load Balancer
description: In dit artikel vindt u informatie over Azure Load Balancer met bidirectionele TCP eerste pakketten bij time-out voor inactiviteit.
services: load-balancer
documentationcenter: na
author: asudbring
ms.custom: seodec18
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2019
ms.author: allensu
ms.openlocfilehash: b37253f37043d902d33504b99401781eb1c761c5
ms.sourcegitcommit: a107430549622028fcd7730db84f61b0064bf52f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74075936"
---
# <a name="load-balancer-with-tcp-reset-on-idle-public-preview"></a>Load Balancer met opnieuw instellen van TCP bij inactiviteit (openbare preview)

U kunt [Standard Load Balancer](load-balancer-standard-overview.md) gebruiken om een meer voorspel bare toepassings gedrag te maken voor uw SCENARIO'S door TCP reset in te scha kelen bij niet-actief voor een bepaalde regel. Het standaard gedrag van Load Balancer is het op de achtergrond neerzetten van stromen wanneer de time-out van een stroom niet actief is.  Als u deze functie inschakelt, worden Load Balancer bidirectionele TCP resets (TCP eerste pakket) verzonden naar time-out voor inactiviteit.  Hiermee wordt de eind punten van uw toepassing geïnformeerd dat er een time-out voor de verbinding is opgetreden. deze kan niet meer worden gebruikt.  Eind punten kunnen onmiddellijk een nieuwe verbinding tot stand brengen, indien nodig.

![Load Balancer TCP opnieuw instellen](media/load-balancer-tcp-reset/load-balancer-tcp-reset.png)

>[!NOTE] 
>De functie voor het opnieuw instellen van een TCP-time-out voor inactiviteit op dit moment is beschikbaar als open bare preview. Load Balancer Deze preview wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. De reden hiervoor is dat bepaalde functies mogelijk niet worden ondersteund of beperkte mogelijkheden hebben. Raadpleeg voor meer informatie de [aanvullende gebruiksrechtovereenkomst voor Microsoft Azure-previews](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
 
U wijzigt dit standaard gedrag en schakelt het verzenden van TCP-opnieuw instellen in op inactieve time-outwaarde voor binnenkomende NAT-regels, taakverdelings regels en [Uitgaande regels](https://aka.ms/lboutboundrules).  Wanneer dit is ingeschakeld per regel, verzendt Load Balancer bidirectionele TCP-opnieuw instellen (eerste TCP-pakketten) naar zowel client-als server eindpunten op het moment van time-out voor inactiviteit voor alle overeenkomende stromen.

Eind punten die TCP eerste pakketten ontvangen, sluiten de overeenkomstige socket direct. Dit biedt een onmiddellijke melding aan de eind punten die de release van de verbinding heeft en toekomstige communicatie op dezelfde TCP-verbinding zal mislukken.  Toepassingen kunnen verbindingen opschonen wanneer de socket wordt gesloten en de verbindingen zo nodig opnieuw tot stand worden gebracht, zonder te wachten tot de TCP-verbinding uiteindelijk een time-out heeft.

Voor veel scenario's kan dit ertoe leiden dat de TCP-(of Application Layer) keepalives niet hoeven te worden verzonden om de time-out voor inactiviteit van een stroom te vernieuwen. 

Als uw niet-actieve duur groter is dan de limieten die zijn toegestaan door de configuratie of uw toepassing een ongewenste gedrag laat zien waarbij TCP opnieuw instellen is ingeschakeld, moet u mogelijk nog steeds TCP-keepalives gebruiken (of keepalives van de toepassingslaag) om de Live van de TCP-verbindingen te bewaken.  Verder kan keepalives ook handig blijven als de verbinding op een andere locatie in het pad wordt verzonden, met name voor keepalives van de toepassingslaag.  

Bekijk zorgvuldig het volledige end-to-end-scenario om te bepalen of u voor delen hebt voor het inschakelen van TCP-opnieuw instellen, het aanpassen van de time-out voor inactiviteit, en als er extra stappen nodig zijn om ervoor te zorgen dat het gewenste toepassings gedrag

## <a name="enabling-tcp-reset-on-idle-timeout"></a>TCP Reset inschakelen bij time-out inactiviteit

Met API-versie 2018-07-01 kunt u verzen ding van bidirectionele TCP-bewerkingen voor time-out voor inactiviteit per regel inschakelen:

```json
      "loadBalancingRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "inboundNatRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

```json
      "outboundRules": [
        {
          "enableTcpReset": true | false,
        }
      ]
```

## <a name="regions"></a>Beschik baarheid van regio

Beschikbaar in alle regio's.

## <a name="limitations"></a>Beperkingen

- De portal kan niet worden gebruikt voor het configureren of weer geven van TCP Reset.  Gebruik in plaats daarvan sjablonen, REST-API, Az CLI 2.0 of PowerShell.
- EERSTE TCP wordt alleen verzonden tijdens de TCP-verbinding in de INGESTELDe status.

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over [standaardversie van Load Balancer](load-balancer-standard-overview.md).
- Meer informatie over [Uitgaande regels](load-balancer-outbound-rules-overview.md).
