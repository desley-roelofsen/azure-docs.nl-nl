---
title: Problemen met de gedegradeerde status op Azure Traffic Manager oplossen
description: Problemen met Traffic Manager profielen oplossen wanneer deze worden weer gegeven als gedegradeerde status.
services: traffic-manager
documentationcenter: ''
author: asudbring
manager: kumudD
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 05/03/2017
ms.author: allensu
ms.openlocfilehash: 8b8fbdf55e408874f6a6e83d6333583238865b5c
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74227722"
---
# <a name="troubleshooting-degraded-state-on-azure-traffic-manager"></a>Oplossen van problemen met een gedegradeerde status op Azure Traffic Manager

In dit artikel wordt beschreven hoe u een Azure Traffic Manager-profiel oplost dat een gedegradeerde status weergeeft. Als eerste stap in het oplossen van problemen met de status van een Azure-Traffic Manager is de functie voor diagnostische logboek registratie ingeschakeld.  Raadpleeg [Diagnostische logboeken inschakelen](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-diagnostic-logs) voor meer informatie. Voor dit scenario moet u een Traffic Manager profiel hebben geconfigureerd dat verwijst naar een aantal van uw gehoste cloudapp.net-services. Als de status van uw Traffic Manager een **gedegradeerde** status weergeeft, kan de status van een of meer eind punten worden **verslechterd**:

![gedegradeerde eindpunt status](./media/traffic-manager-troubleshooting-degraded/traffic-manager-degradedifonedegraded.png)

Als in de status van uw Traffic Manager een **inactieve** status wordt weer gegeven, kunnen beide eind punten worden **uitgeschakeld**:

![Inactieve Traffic Manager status](./media/traffic-manager-troubleshooting-degraded/traffic-manager-inactive.png)

## <a name="understanding-traffic-manager-probes"></a>Wat is Traffic Manager tests?

* Traffic Manager beschouwt een eind punt alleen ONLINE als de test een HTTP 200-antwoord ontvangt van het pad naar de test. Als u een andere HTTP-antwoord code retourneert, moet u die antwoord code toevoegen aan de [verwachte status codes](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring#configure-endpoint-monitoring) van uw Traffic Manager profiel.
* Een 30x beter worden-omleidings antwoord wordt behandeld als een fout, tenzij u dit als een geldige antwoord code hebt opgegeven in de [verwachte status codes](https://docs.microsoft.com/azure/traffic-manager/traffic-manager-monitoring#configure-endpoint-monitoring) van uw Traffic Manager-profiel. Het omleidings doel wordt niet door Traffic Manager getest.
* Voor HTTPs-tests worden certificaat fouten genegeerd.
* De werkelijke inhoud van het pad naar de test is niet van belang, zolang een 200 wordt geretourneerd. Het zoeken naar een URL naar bepaalde statische inhoud, zoals '/favicon.ico ', is een gang bare techniek. Dynamische inhoud, zoals de ASP-pagina's, retourneert niet altijd 200, zelfs als de toepassing in orde is.
* Een best practice bestaat uit het instellen van het probe-pad naar iets dat voldoende logica heeft om te bepalen of de site al dan niet actief is. In het vorige voor beeld, door het pad in te stellen op '/favicon.ico ', moet u alleen testen of W3wp. exe reageert. Deze test geeft mogelijk niet aan dat uw webtoepassing in orde is. Een betere optie is het instellen van een pad naar een iets zoals '/probe.aspx ' die logica heeft om de status van de site te bepalen. U kunt bijvoorbeeld prestatie meter items gebruiken om het CPU-gebruik te bepalen of het aantal mislukte aanvragen te meten. U kunt ook proberen om toegang te krijgen tot database bronnen of sessie status om ervoor te zorgen dat de webtoepassing werkt.
* Als alle eind punten in een profiel worden gedegradeerd, worden alle eind punten door Traffic Manager als gezond beschouwd en wordt verkeer naar alle eind punten gerouteerd. Dit gedrag zorgt ervoor dat problemen met het mechanisme voor het zoeken niet leiden tot een volledige onderbreking van uw service.

## <a name="troubleshooting"></a>Problemen oplossen

Als u een test fout wilt oplossen, hebt u een hulp programma nodig dat de HTTP-status code retourneert van de test-URL. Er zijn veel hulpprogram ma's beschikbaar die het onbewerkte HTTP-antwoord tonen.

* [Fiddler](https://www.telerik.com/fiddler)
* [Ezelsoor](https://curl.haxx.se/)
* [wget](http://gnuwin32.sourceforge.net/packages/wget.htm)

U kunt ook het tabblad netwerk van de F12-Hulpprogram Ma's voor fout opsporing in Internet Explorer gebruiken om de HTTP-antwoorden te bekijken.

Voor dit voor beeld willen we het antwoord zien van onze test-URL: http:\//watestsdp2008r2.cloudapp.net:80/Probe. Het volgende Power shell-voor beeld illustreert het probleem.

```powershell
Invoke-WebRequest 'http://watestsdp2008r2.cloudapp.net/Probe' -MaximumRedirection 0 -ErrorAction SilentlyContinue | Select-Object StatusCode,StatusDescription
```

Voorbeelduitvoer:

    StatusCode StatusDescription
    ---------- -----------------
           301 Moved Permanently

U ziet dat er een omleidings antwoord is ontvangen. Zoals eerder vermeld, wordt een andere status code dan 200 beschouwd als een fout. Traffic Manager wijzigt de eindpunt status in offline. Om het probleem op te lossen, controleert u de website configuratie om ervoor te zorgen dat de juiste status code kan worden geretourneerd vanaf het pad van de test. Configureer de Traffic Manager-test opnieuw zodat deze verwijst naar een pad dat een 200 retourneert.

Als uw test het HTTPS-protocol gebruikt, moet u mogelijk de certificaat controle uitschakelen om SSL/TLS-fouten tijdens uw test te voor komen. Met de volgende Power shell-instructies wordt certificaat validatie voor de huidige Power shell-sessie uitgeschakeld:

```powershell
add-type @"
using System.Net;
using System.Security.Cryptography.X509Certificates;
public class TrustAllCertsPolicy : ICertificatePolicy {
    public bool CheckValidationResult(
    ServicePoint srvPoint, X509Certificate certificate,
    WebRequest request, int certificateProblem) {
    return true;
    }
}
"@
[System.Net.ServicePointManager]::CertificatePolicy = New-Object TrustAllCertsPolicy
```

## <a name="next-steps"></a>Volgende stappen

[Over Traffic Manager routerings methoden voor verkeer](traffic-manager-routing-methods.md)

[Wat is Traffic Manager](traffic-manager-overview.md)

[Cloud Services](https://go.microsoft.com/fwlink/?LinkId=314074)

[Azure App Service](https://azure.microsoft.com/documentation/services/app-service/web/)

[Bewerkingen op Traffic Manager (REST API-referentiemateriaal)](https://go.microsoft.com/fwlink/?LinkId=313584)

[Azure Traffic Manager-cmdlets][1]

[1]: https://docs.microsoft.com/powershell/module/az.trafficmanager
