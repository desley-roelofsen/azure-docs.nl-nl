---
title: Aangepaste regels voor Azure Web Application firewall (WAF) v2 op Application Gateway
description: Dit artikel bevat een overzicht van aangepaste WAF-regels (Web Application firewall) op Azure-toepassing gateway.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 10/04/2019
ms.author: victorh
ms.openlocfilehash: 0ac37378797c59d79af5d026200b68154836c5ac
ms.sourcegitcommit: f4d8f4e48c49bd3bc15ee7e5a77bee3164a5ae1b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73585399"
---
# <a name="custom-rules-for-web-application-firewall-v2-on-azure-application-gateway"></a>Aangepaste regels voor Web Application firewall v2 op Azure-toepassing gateway

De Azure-toepassing gateway Web Application firewall (WAF) v2 wordt geleverd met een vooraf geconfigureerde, door een platform beheerde ruleset die bescherming biedt tegen een groot aantal verschillende soorten aanvallen. Deze aanvallen omvatten cross-site scripting, SQL-injectie en andere. Als u een WAF-beheerder bent, kunt u uw eigen regels schrijven om de regels voor basisregel sets (CRS) te verbeteren. Uw regels kunnen het aangevraagde verkeer blok keren of toestaan op basis van de overeenkomende criteria.

Met aangepaste regels kunt u uw eigen regels maken die worden geëvalueerd voor elke aanvraag die via de WAF wordt door gegeven. Deze regels bevatten een hogere prioriteit dan de rest van de regels in de beheerde regel sets. De aangepaste regels bevatten de naam van de regel, de prioriteit van de regel en een matrix met overeenkomende voor waarden. Als aan deze voor waarden wordt voldaan, wordt er een actie ondernomen (om toe te staan of te blok keren).

U kunt bijvoorbeeld alle aanvragen van een IP-adres in het bereik 192.168.5.4/24 blok keren. In deze regel is de operator *IPMatch*, het matchValues is het IP-adres bereik (192.168.5.4/24) en de actie om het verkeer te blok keren. U kunt ook de naam en prioriteit van de regel instellen.

Aangepaste regels bieden ondersteuning voor het gebruik van samengestelde logica om geavanceerde regels te maken die voldoen aan uw beveiligings behoeften. Bijvoorbeeld (Condition 1 **en** condition 2) **of** condition 3).  Dit voor beeld betekent dat als wordt voldaan aan voor waarde 1 **en** voor waarde 2, **of** als aan voor waarde 3 wordt voldaan, de WAF de actie moet uitvoeren die is opgegeven in de aangepaste regel.

Verschillende overeenkomende voor waarden binnen dezelfde regel worden altijd samengesteld met **en**. Bijvoorbeeld verkeer blok keren van een specifiek IP-adres en alleen als ze een bepaalde browser gebruiken.

Als u **of** twee verschillende voor waarden wilt, moeten de twee voor waarden in verschillende regels zijn. Bijvoorbeeld verkeer blok keren van een specifiek IP-adres of verkeer blok keren als ze een specifieke browser gebruiken.

> [!NOTE]
> Het maximum aantal aangepaste WAF-regels is 100. Zie [Azure-abonnement en service limieten, quota's en beperkingen](../../azure-subscription-service-limits.md#application-gateway-limits)voor meer informatie over Application Gateway limieten.

Reguliere expressies worden ook ondersteund in aangepaste regels, net als in de CRS-rules. Voor voor beelden van deze, zie voor beelden 3 en 5 in [aangepaste Web Application firewall regels maken en gebruiken](create-custom-waf-rules.md).

## <a name="allowing-vs-blocking"></a>Toestaan versus blok keren

Het toestaan en blok keren van verkeer is eenvoudig met aangepaste regels. U kunt bijvoorbeeld al het verkeer dat afkomstig is van een bereik van IP-adressen blok keren. U kunt een andere regel maken om verkeer toe te staan als de aanvraag afkomstig is van een specifieke browser.

Als u iets wilt toestaan, moet u ervoor zorgen dat de para meter `-Action` is ingesteld op **toestaan**. Als u iets wilt blok keren, moet u ervoor zorgen dat de para meter `-Action` is ingesteld op **blok keren**.

```azurepowershell
$AllowRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example1 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Allow

$BlockRule = New-AzApplicationGatewayFirewallCustomRule `
   -Name example2 `
   -Priority 2 `
   -RuleType MatchRule `
   -MatchCondition $condition `
   -Action Block
```

De vorige `$BlockRule` wordt toegewezen aan de volgende aangepaste regel in Azure Resource Manager:

```json
"customRules": [
      {
        "name": "blockEvilBot",
        "priority": 2,
        "ruleType": "MatchRule",
        "action": "Block",
        "matchConditions": [
          {
            "matchVariables": [
              {
                "variableName": "RequestHeaders",
                "selector": "User-Agent"
              }
            ],
            "operator": "Contains",
            "negationConditon": false,
            "matchValues": [
              "evilbot"
            ],
            "transforms": [
              "Lowercase"
            ]
          }
        ]
      }
    ], 
```

Deze aangepaste regel bevat een naam, prioriteit, een actie en de matrix van overeenkomende voor waarden waaraan moet worden voldaan voordat de actie wordt uitgevoerd. Zie de volgende veld beschrijvingen voor meer uitleg van deze velden. Zie aangepaste regels voor [Web Application firewall maken en gebruiken](create-custom-waf-rules.md)voor meer informatie.

## <a name="fields-for-custom-rules"></a>Velden voor aangepaste regels

### <a name="name-optional"></a>Naam [Optioneel]

Dit is de naam van de regel. Deze naam wordt weer gegeven in de logboeken.

### <a name="priority-required"></a>Prioriteit [vereist]

- Bepaalt de volg orde van de regel. Hoe lager de waarde, hoe eerder de evaluatie van de regel. Het toegestane bereik is 1-100. 
- Moet uniek zijn voor alle aangepaste regels. Een regel met prioriteit 40 wordt geëvalueerd vóór een regel met de prioriteit 80.

### <a name="rule-type-required"></a>Regel type [vereist]

Momenteel moet **MatchRule**zijn.

### <a name="match-variable-required"></a>Overeenkomende variabele [required]

Moet een van de variabelen zijn:

- RemoteAddr: IP-adres/hostnaam van de verbinding met de externe computer
- RequestMethod: HTTP-aanvraag methode (GET, POST, PUT, DELETE, enzovoort)
- Query string: variabele in de URI
- PostArgs: argumenten verzonden in de hoofd tekst van het bericht. Aangepaste regels die gebruikmaken van deze match-variabele worden alleen toegepast als de header content-type is ingesteld op Application/x-www-form-urlencoded en meerdelige/form-data.
- RequestUri: de URI van de aanvraag
- RequestHeaders: headers van de aanvraag
- RequestBody: dit bevat de volledige hoofd tekst van de aanvraag. Aangepaste regels die gebruikmaken van deze match-variabele worden alleen toegepast als de header content-type is ingesteld op Application/x-www-form-urlencoded. 
- RequestCookies: cookies van de aanvraag

### <a name="selector-optional"></a>Selector [Optioneel]

Beschrijft het veld van de matchVariable-verzameling. Als de matchVariable bijvoorbeeld RequestHeaders is, kan de selector zich op de header *User-agent* bevindt.

### <a name="operator-required"></a>Operator [vereist]

Dit moet een van de volgende Opera tors zijn:

- IPMatch: wordt alleen gebruikt als de overeenkomende variabele *RemoteAddr* is
- Is gelijk aan-invoer is hetzelfde als de MatchValue
- Contains
- LessThan
- GreaterThan
- LessThanOrEqual
- GreaterThanOrEqual
- BeginsWith
- EndsWith
- reguliere
- Geoovereenkomst (preview-versie)

### <a name="negate-condition-optional"></a>Voor waarde voor negatie [Optioneel]

De huidige voor waarde wordt genegeerd.

### <a name="transform-optional"></a>Transformeren [Optioneel]

Een lijst met teken reeksen met namen van trans formaties voordat de overeenkomst wordt geprobeerd. Dit kunnen de volgende trans formaties zijn:

- Kleine letters
- Interne
- UrlDecode
- UrlEncode 
- RemoveNulls
- HtmlEntityDecode

### <a name="match-values-required"></a>Overeenkomende waarden [required]

Lijst met waarden die moeten worden vergeleken, wat kan worden beschouwd als *or*' Ed '. Het kan bijvoorbeeld IP-adressen of andere teken reeksen zijn. De notatie van de waarde is afhankelijk van de vorige operator.

### <a name="action-required"></a>Actie [vereist]

- Toestaan: Hiermee wordt de trans actie geautoriseerd, waarbij alle volgende regels worden overgeslagen. Dit betekent dat de opgegeven aanvraag wordt toegevoegd aan de lijst met toegestane aanvragen en eenmaal overeenkomt met de aanvraag en wordt verzonden naar de back-end-groep. Regels die zich op de acceptatie lijst bevinden, worden niet geëvalueerd voor verdere aangepaste regels of beheerde regels.
- Blok: blokkeert de trans actie op basis van *SecDefaultAction* (detectie/preventie modus). Net als bij de actie voor toestaan, zodra de aanvraag is geëvalueerd en toegevoegd aan de lijst met geblokkeerde websites, wordt de evaluatie gestopt en wordt de aanvraag geblokkeerd. Aanvragen die voldoen aan dezelfde voor waarden worden niet geëvalueerd en worden alleen geblokkeerd. 
- Log: Hiermee kan de regel naar het logboek schrijven, maar kan de rest van de regels worden uitgevoerd voor evaluatie. Volgende aangepaste regels worden geëvalueerd in volg orde van prioriteit, gevolgd door de beheerde regels.

## <a name="geomatch-custom-rules-preview"></a>Aangepaste regels voor geomatching (preview-versie)

Aangepaste regels maken het mogelijk om op maat gemaakte regels aan te passen aan de exacte behoeften van uw toepassingen en uw beveiligings beleid. U kunt de toegang tot uw webtoepassingen nu beperken op land/regio, die beschikbaar is in open bare preview. Net als bij alle aangepaste regels kan deze logica worden samengesteld met andere regels om te voldoen aan de behoeften van uw toepassing. 

   > [!NOTE]
   > Geomatch aangepaste regels zijn beschikbaar in Zuid-Centraal VS en Europa-noord. Als u toegang wilt krijgen tot de portals, gebruikt u [deze koppeling](https://aka.ms/AppGWWAFGeoMatch) totdat deze Live voor iedereen actief is. 

Als u de operator geomatch gebruikt, kunnen de selecters een van de volgende landen codes van twee cijfers zijn. 

|Landnummer | Land naam |
| ----- | ----- |
| AD | Andorra |
| AE | Verenigde Arabische Emiraten|
| AF | Afghanistan|
| AG | Antigua en Barbuda|
| AL | Albanië|
| AM | Armenië|
| AO | Angola|
| AR | Argentinië|
| AS | Amerikaans-Samoa|
| AT | Oostenrijk|
| AU | Australië|
| AZ | Azerbeidzjan|
| BA | Bosnië en Herzegovina|
| BB | Barbados|
| BD | Bangladesh|
| BE | België|
| BF | Burkina Faso|
| BG | Bulgarije|
| BH | Bahrein|
| BI | Burundi|
| BJ | Benin|
| BL | Saint--Barthélemy|
| BN | Brunei Darussalam|
| BO | Bolivia|
| BR | Brazilië|
| BS | Bahama's|
| BT | Bhutan|
| BW | Botswana|
| BY | Belarus|
| BZ | Belize|
| CA | Canada|
| CD | Congo (DRC)|
| CF | Centraal-Afrikaanse Republiek|
| CH | Zwitserland|
| CI | Cote d'Ivoire|
| CL | Chili|
| CM | Kameroen|
| CN | China|
| CO | Colombia|
| CR | Costa Rica|
| CU | Cuba|
| CV | Cabo Verde|
| CY | Cyprus|
| CZ | Tsjechië|
| DE | Duitsland|
| DK | Denemarken|
| DO | Dominicaanse Republiek|
| DZ | Algerije|
| EC | Ecuador|
| EE | Estland|
| EG | Egypte|
| ES | Spanje|
| ET | Ethiopië|
| FI | Finland|
| FJ | Fiji|
| FM | Micronesië, Federale Staten van|
| FR | Frankrijk|
| GB | Verenigd Koninkrijk|
| GE | Georgië|
| GF | Frans-Guyana|
| GH | Ghana|
| GN | Guinee|
| GP | Guadeloupe|
| GR | Griekenland|
| GT | Guatemala|
| GY | Guyana|
| HK | Hong Kong SAR|
| HN | Honduras|
| HR | Kroatië|
| HT | Haiti|
| HU | Hongarije|
| Id | Indonesië|
| IE | Ierland|
| IL | Israël|
| IN | India|
| IQ | Irak|
| IR | Iran, Islamitische Republiek|
| IS | IJsland|
| IT | Italië|
| JM | Jamaica|
| JO | Jordanië|
| JP | Japan|
| KE | Kenia|
| KG | Kirgizië|
| KH | Cambodja|
| KI | Kiribati|
| KN | Saint Kitts en Nevis|
| KP | Noord-Korea|
| KR | Zuid-Korea|
| KW | Koeweit|
| KY | Kaaimaneilanden|
| KZ | Kazachstan|
| LA | Laos|
| LB | Libanon|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberia|
| LS | Lesotho|
| LT | Litouwen|
| LU | Luxemburg|
| LV | Letland|
| LY | Libië |
| MA | Marokko|
| MD | Moldavië, Republiek|
| MG | Madagascar|
| MK | Noord-Macedonië|
| ML | Mali|
| MM | Myanmar|
| MN | Mongolië|
| MO | Macau SAR|
| MQ | Martinique|
| MR | Mauretanië|
| MT | Malta|
| MV | Maldiven|
| MW | Malawi|
| MX | Mexico|
| MY | Maleisië|
| MZ | Mozambique|
| N.v.t. | Namibië|
| NE | Niger|
| NG | Nigeria|
| NI | Nicaragua|
| NL | Nederland|
| NO | Noorwegen|
| NP | Nepal|
| NR | Nauru|
| NZ | Nieuw-Zeeland|
| OM | Oman|
| PA | Panama|
| PE | Peru|
| PH | Filipijnen|
| PK | Pakistan|
| PL | Polen|
| PR | Puerto Rico|
| PT | Portugal|
| PW | Palau|
| PY | Paraguay|
| QA | Qatar|
| RE | Réunion|
| RO | Roemenië|
| RS | Servië|
| RU | Russische Federatie|
| RW | Rwanda|
| SA | Saoedi-Arabië|
| SD | Soedan|
| SE | Zweden|
| SG | Singapore|
| SI | Slovenië|
| SK | Slowakije|
| SN | Senegal|
| SO | Somalië|
| SR | Suriname|
| SS | Zuid-Soedan|
| SV | El Salvador|
| SY | Arabische Republiek Syrië|
| SZ | Swaziland|
| TC | Turks- en Caicoseilanden|
| TG | Togo|
| TH | Thailand|
| TN | Tunesië|
| TR | Turkije|
| TT | Trinidad en Tobago|
| TW | Taiwan|
| TZ | Tanzania, Verenigde Republiek|
| UA | Oekraïne|
| UG | Oeganda|
| VS | Verenigde Staten|
| UY | Uruguay|
| UZ | Oezbekistan|
| VC | Saint Vincent en de Grenadines|
| VE | Venezuela|
| VG | Britse Maagdeneilanden|
| VI | Amerikaanse Maagdeneilanden|
| VN | Vietnam|
| ZA | Zuid-Afrika|
| ZM | Zambia|
| ZW | Zimbabwe|

## <a name="next-steps"></a>Volgende stappen

Nadat u meer informatie over aangepaste regels hebt [gemaakt, maakt u uw eigen aangepaste regels](create-custom-waf-rules.md).
