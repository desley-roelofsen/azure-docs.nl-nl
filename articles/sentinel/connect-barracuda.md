---
title: Barracuda-gegevens verbinden met Azure Sentinel | Microsoft Docs
description: Meer informatie over het verbinden van Barracuda-gegevens met Azure Sentinel.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 3b33b4aa-7286-4d79-b461-8e1812edc2e1
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/13/2019
ms.author: rkarlin
ms.openlocfilehash: 5142204db86158937c16d19bf52fb677099598a5
ms.sourcegitcommit: e0a1a9e4a5c92d57deb168580e8aa1306bd94723
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/11/2019
ms.locfileid: "72284657"
---
# <a name="connect-your-barracuda-appliance"></a>Verbinding maken met uw Barracuda-apparaat 



Met de Barracuda Web Application firewall (WAF) kunt u eenvoudig uw Barracuda-logboeken verbinden met uw Azure-Sentinel, om Dash boards weer te geven, aangepaste waarschuwingen te maken en het onderzoek te verbeteren. Dit geeft u meer inzicht in het netwerk van uw organisatie en verbetert de mogelijkheden van beveiligings bewerkingen. Azure Sentinel maakt gebruik van de systeem eigen integratie tussen **Barracuda** en log Analytics agent om naadloze integratie mogelijk te maken. 


> [!NOTE]
> Gegevens worden opgeslagen in de geografische locatie van de werk ruimte waarop u Azure Sentinel uitvoert.

## <a name="configure-and-connect-barracuda-waf"></a>Barracuda WAF configureren en verbinden
Barracuda Web Application firewall kan Logboeken rechtstreeks integreren en exporteren naar Azure Sentinel via Log Analytics-agent.
1. Ga naar de [configuratie stroom BARRACUDA WAF](https://campus.barracuda.com/product/webapplicationfirewall/doc/73696965/configure-the-barracuda-web-application-firewall-to-integrate-with-the-oms-server-and-export-logs/)en volg de instructies voor het instellen van de verbinding, met behulp van de volgende para meters:
    - **Werk ruimte-id**: Kopieer de waarde van uw werk ruimte-id van de Azure Sentinel Barracuda-connector pagina.
    - **Primaire sleutel**: Kopieer de waarde van uw primaire sleutel op de pagina Azure Sentinel Barracuda-connector.
1. Als u het relevante schema in Log Analytics voor de Barracuda-gebeurtenissen wilt gebruiken, zoekt u naar **CommonSecurityLog** en **barracuda_CL**.


## <a name="validate-connectivity"></a>Connectiviteit valideren

Het kan 20 minuten duren voordat uw logboeken in Log Analytics worden weer gegeven. 



## <a name="next-steps"></a>Volgende stappen
In dit document hebt u geleerd hoe u Barracuda-apparaten verbindt met Azure Sentinel. Raadpleeg de volgende artikelen voor meer informatie over Azure Sentinel:
- Meer informatie over hoe u [inzicht krijgt in uw gegevens en mogelijke bedreigingen](quickstart-get-visibility.md).
- Ga aan de slag [met het detecteren van bedreigingen met Azure Sentinel](tutorial-detect-threats-built-in.md).

