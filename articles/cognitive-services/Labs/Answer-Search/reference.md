---
title: Project Answer Search-verwijzing
titlesuffix: Azure Cognitive Services
description: Verwijzing voor het zoek eindpunt van het project antwoord.
services: cognitive-services
author: mikedodaro
manager: nitinme
ms.service: cognitive-services
ms.subservice: answer-search
ms.topic: reference
ms.date: 04/13/2018
ms.author: rosh
ROBOTS: NOINDEX
ms.openlocfilehash: 28449435479aef0d6a1d8aee3e53de1a78f401b3
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68705333"
---
# <a name="project-answer-search-v7-reference"></a>Naslag informatie voor het zoeken naar project antwoorden V7

Bing antwoord SearchAPI accepteert een query parameter en retourneert een `searchResponse` met `answerType`: `facts` of `entities`. 

Toepassingen die gebruikmaken van de antwoord zoeken-API verzenden aanvragen naar het eind punt met een URL voor de preview-versie van een query parameter.  De aanvraag moet de `q=searchTerm` para meter-en *OCP-APIM-abonnements sleutel* -header bevatten.   

De JSON-respons kan worden geparseerd voor feiten en entiteiten die details bevatten over het object van de zoek opdracht.

## <a name="endpoint"></a>Eindpunt
Stuur een aanvraag naar het volgende eind punt om de zoek resultaten van antwoorden te vragen. Gebruik de para meters headers en URL om verdere specificaties te definiëren.

Eind punt ophalen: 
```
https://api.labs.cognitive.microsoft.com/answerSearch/v7.0/search?q=<searchTerm>&subscription-key=0123456789ABCDEF&mkt=en-us

```

De aanvraag moet het HTTPS-protocol gebruiken en de volgende query parameter bevatten:
-  `q=<URL>`-De query die het object van de zoek opdracht identificeert

Zie [ C# Quick](c-sharp-quickstart.md) start of [Java Quick](java-quickstart.md)start (Engelstalig) voor voor beelden die laten zien hoe u aanvragen doet. 

De volgende secties bevatten technische gegevens over de antwoord objecten, query parameters en kopteksten die van invloed zijn op de zoek resultaten. 
  
Zie kopteksten voor informatie over kopteksten die aanvragen [](#headers)moeten bevatten.  
  
Zie [query parameters](#query-parameters)voor meer informatie over query parameters die aanvragen moeten bevatten.  
  
Zie [Response Objects](#response-objects)(Engelstalig) voor meer informatie over de JSON-objecten die het antwoord bevat.

De maximale lengte van de query-URL is 2.048 tekens. Om ervoor te zorgen dat de lengte van de URL de limiet niet overschrijdt, moet de maximum lengte van de query parameters kleiner zijn dan 1.500 tekens. Als de URL langer is dan 2.048 tekens, retourneert de server 404 niet gevonden.  

Zie [vereisten voor gebruik en weer gave](use-display-requirements.md)voor meer informatie over het toegestane gebruik en de weer gave van resultaten. 

> [!NOTE]
> Sommige aanvraag headers die relevant zijn voor andere zoek-Api's, hebben geen invloed op de URL-preview
> - Pragma: de aanroeper heeft geen controle over of het URL-voor beeld een cache gebruikt
> - Cache-Control: de aanroeper heeft geen controle over of het URL-voor beeld een cache gebruikt
> - User-Agent
> 
> Sommige para meters zijn momenteel niet relevant voor URL-preview-API, maar kunnen in de toekomst worden gebruikt voor verbeterde globalisering. 
 
## <a name="headers"></a>Headers  
Hieronder ziet u de kopteksten die een aanvraag en antwoord kan bevatten.  
  
|Header|Description|  
|------------|-----------------|  
|Accepteren|Optionele aanvraagheader.<br /><br /> Het standaard media type is Application/JSON. Als u wilt opgeven dat het antwoord [JSON-LD](https://json-ld.org/)gebruikt, stelt u de Accept-header in op Application/LD + JSON.|  
|<a name="acceptlanguage" />Accept-Language|Optionele aanvraagheader.<br /><br /> Een door komma's gescheiden lijst met talen die moet worden gebruikt voor gebruikersinterfacetekenreeksen. De lijst is in aflopende volgorde van voorkeur. Zie [RFC2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) voor meer informatie, waaronder de verwachte indeling.<br /><br /> Deze header en de queryparameter [setLang](#setlang) sluiten elkaar uit&mdash;geef ze niet beide op.<br /><br /> Als u deze header instelt, moet u ook de para meter CC query opgeven. Om de markt te bepalen waarvoor resultaten moeten worden geretourneerd, gebruikt Bing de eerste ondersteunde taal die wordt gevonden in de lijst en combineert deze met de parameterwaarde `cc`. Als de lijst geen ondersteunde taal bevat, vindt Bing de dichtstbijzijnde taal en markt die de aanvraag ondersteunen, of gebruikt een geaggregeerde of standaardmarkt voor de resultaten. Zie de header BingAPIs-Market om de markt te bepalen die Bing heeft gebruikt.<br /><br /> Gebruik deze header en de queryparameter `cc` alleen als u meerdere talen opgeeft. Gebruik anders de queryparameters [mkt](#mkt) en [setLang](#setlang).<br /><br /> Een gebruikersinterfacetekenreeks is een tekenreeks die wordt gebruikt als label in een gebruikersinterface. Er zijn maar weinig gebruikersinterfacetekenreeksen in de JSON-antwoordobjecten. De opgegeven taal wordt toegepast op koppelingen naar Bing.com-eigenschappen in de antwoordobjecten.|  
|<a name="market" />BingAPIs-Market|Antwoordheader.<br /><br /> De markt die wordt gebruikt door de aanvraag. De notatie is \<languageCode\>-\<countryCode\>. Bijvoorbeeld: nl-NL.|  
|<a name="traceid" />BingAPIs-TraceId|Antwoordheader.<br /><br /> De id van de logboekvermelding die de details van de aanvraag bevat. Registreer deze id wanneer er een fout optreedt. Als u het probleem niet kunt vaststellen en oplossen, neemt u deze id op bij de andere informatie die u aan het ondersteuningsteam verstrekt.|  
|<a name="subscriptionkey" />Ocp-Apim-Subscription-Key|Vereiste aanvraagheader.<br /><br /> De abonnementssleutel die u hebt ontvangen toen u zich hebt geregistreerd voor deze service in [Cognitive Services](https://www.microsoft.com/cognitive-services/).|  
|<a name="pragma" />Pragma|Optionele aanvraagheader<br /><br /> Bing retourneert standaard cache-inhoud, indien beschikbaar. Om te voorkomen dat Bing inhoud uit de cache retourneert, stelt u de Pragma-header in op no-cache (bijvoorbeeld: Pragma: no-cache).
|<a name="useragent" />User-Agent|Optionele aanvraagheader.<br /><br /> De gebruikersagent waarvan de aanvraag afkomstig is. Bing gebruikt de user-agent om mobiele gebruikers een geoptimaliseerde ervaring te bieden. Hoewel dit optioneel is, wordt u aangeraden deze header altijd op te geven.<br /><br /> De user-agent moet dezelfde tekenreeks zijn die elke veelgebruikte browser verzendt. Zie [RFC 2616](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html) voor meer informatie over gebruikersagenten.<br /><br /> Hier volgen enkele voorbeelden van user-agent-tekenreeksen.<br /><ul><li>Windows Phone&mdash;Mozilla/5.0 (compatibel; MSIE 10.0; Windows Phone 8.0; Trident/6.0; IEMobile/10.0; ARM; Touch; NOKIA; Lumia 822)<br /><br /></li><li>Android&mdash;Mozilla/5.0 (Linux; U; Android 2.3.5; nl-nl; SCH-I500 Build/GINGERBREAD) AppleWebKit/533.1 (KHTML; like Gecko) Version/4.0 Mobile Safari/533.1<br /><br /></li><li>iPhone&mdash;Mozilla/5.0 (iPhone; CPU iPhone OS 6_1 like Mac OS X) AppleWebKit/536.26 (KHTML; zoals Gecko) Mobile/10B142 iPhone4;1 BingWeb/3.03.1428.20120423<br /><br /></li><li>PC&mdash;Mozilla/5.0 (Windows NT 6.3; WOW64; Trident/7.0; Touch; rv:11.0) zoals Gecko<br /><br /></li><li>iPad&mdash;Mozilla/5.0 (iPad; CPU OS 7_0 zoals Mac OS X) AppleWebKit/537.51.1 (KHTML, zoals Gecko) Version/7.0 Mobile/11A465 Safari/9537.53</li></ul>|
|<a name="clientid" />X-MSEdge-ClientID|Optionele aanvraag- en antwoord-header.<br /><br /> Bing gebruikt deze header om gebruikers consistent gedrag te bieden bij Bing API-aanroepen. Bing introduceert nieuwe functies en verbeteringen vaak in flights, en gebruikt de client-ID als sleutel voor het toewijzen van verkeer aan verschillende flights. Als u niet dezelfde client-id gebruikt voor een gebruiker voor meerdere aanvragen, kan Bing de gebruiker toewijzen aan meerdere conflicterende flights. Toewijzing aan meerdere conflicterende flights kan leiden tot een inconsistente gebruikerservaring. Als de tweede aanvraag bijvoorbeeld een andere flighttoewijzing heeft dan de eerste, kan de ervaring onverwacht zijn. Bing kan de client-id ook gebruiken om webresultaten aan te passen aan de zoekgeschiedenis van die client-id, waardoor de gebruiker een rijkere ervaring krijgt.<br /><br /> Bing gebruikt deze header ook om de rangschikking van resultaten te verbeteren door de activiteit te analyseren die wordt gegenereerd door een client-id. De relevantie-verbeteringen helpen de kwaliteit van de resultaten die door Bing-API's worden geleverd te verbeteren, en maken op hun beurt hogere doorklikpercentages mogelijk voor de API-consument.<br /><br /> **BELANG RIJK** Hoewel dit optioneel is, moet u rekening houden met deze header. Door de client-id te behouden voor meerdere aanvragen voor dezelfde combinatie van eindgebruiker en apparaat, zorgt u voor 1) een consistente gebruikerservaring van de API-consument, en 2) hogere doorklikpercentages via een betere kwaliteit van de resultaten van de Bing-API's.<br /><br /> Hieronder volgen de basisgebruiksregels die van toepassing zijn op deze header.<br /><ul><li>Elke gebruiker die uw toepassing op het apparaat gebruikt, moet een unieke door Bing gegenereerde client-id hebben.<br /><br/>Als u deze header niet in de aanvraag opneemt, genereert Bing een id en retourneert deze in de X-MSEdge-ClientID-antwoordheader. De enige keer dat u deze header NIET in een aanvraag moet opnemen, is de eerste keer dat de gebruiker uw app op dat apparaat gebruikt.<br /><br/></li><li>Gebruik de client-id voor elke Bing-API-aanvraag die uw app voor deze gebruiker op het apparaat doet.<br /><br/></li><li>**SCHENK** U moet ervoor zorgen dat deze client-ID niet kan worden gekoppeld aan de gegevens van een authenticatable-gebruikers account.</li><br/><li>Maak de client-id persistent. Gebruik in een browser-app een persistent HTTP-cookie om ervoor te zorgen dat de id in alle sessies wordt gebruikt. Gebruik geen sessiecookie. Voor andere apps, zoals mobiele apps, gebruikt u de persistente opslag van het apparaat om de id persistent te maken.<br /><br/>De volgende keer dat de gebruiker uw app op dat apparaat gebruikt, haalt u de client-id op die u persistent hebt gemaakt.</li></ul><br /> **OPMERKING:** Bing-antwoorden mogen deze koptekst bevatten. Als het antwoord deze header bevat, registreert u de client-id en gebruikt u deze voor alle volgende Bing-aanvragen voor de gebruiker op dat apparaat.<br /><br /> **OPMERKING:** Als u de X-MSEdge-ClientID opneemt, moet u geen cookies in de aanvraag opneemt.|  
|<a name="clientip" />X-MSEdge-ClientIP|Optionele aanvraagheader.<br /><br /> Het IPv4- of IPv6-adres van het clientapparaat. Het IP-adres wordt gebruikt voor het detecteren van de locatie van de gebruiker. Bing gebruikt de locatie-informatie om het gedrag van Veilig Zoeken te bepalen.<br /><br /> **OPMERKING:** Hoewel dit optioneel is, wordt u aangeraden deze header en de X-Search-Location-header altijd op te geven.<br /><br /> Verdoezel het adres niet (bijvoorbeeld door het laatste octet te wijzigen in 0). Wanneer u het adres verdoezelt, is de locatie totaal niet in de buurt van de werkelijke locatie van het apparaat, wat ertoe kan leiden dat Bing onjuiste resultaten geeft.|  
|<a name="location" />X-Search-Location|Optionele aanvraagheader.<br /><br /> Een met puntkomma's gescheiden lijst met sleutel-waardeparen die de geografische locatie van de client beschrijven. Bing gebruikt de locatie-informatie om het gedrag van Veilig Zoeken te bepalen en relevante lokale inhoud te retourneren. Geef het sleutel-waardepaar op als \<sleutel\>:\<waarde\>. Hier volgen de sleutels die u gebruikt om de locatie van de gebruiker op te geven.<br /><br /><ul><li>lat&mdash;de breedte graad van de client locatie, in graden. De breedtegraad moet groter dan of gelijk zijn aan -90.0 en kleiner dan of gelijk aan +90.0. Negatieve waarden geven zuidelijke breedtegraden aan, en positieve waarden noordelijke.<br /><br /></li><li>lang&mdash;de lengte graad van de locatie van de client, in graden. De lengtegraad moet groter dan of gelijk zijn aan -180.0 en kleiner dan of gelijk aan +180.0. Negatieve waarden geven westelijke lengtegraden aan, en positieve waarden oostelijke.<br /><br /></li><li>de&mdash; RADIUS opnieuw, in meters, waarmee de horizontale nauw keurigheid van de coördinaten wordt opgegeven. Geef de waarde door die wordt geretourneerd door de locatieservice van het apparaat. Typische waarden zijn bijvoorbeeld 22 m voor GPS/Wi-Fi, 380 m voor triangulatie op basis van gsm-masten en 18.000 m voor reverse IP-lookup.<br /><br /></li><li>TS&mdash; de UTC-time stamp van een UNIX van wanneer de client zich op de locatie bevond. (Het UNIX-tijdstempel is het aantal seconden sinds 1 januari 1970.)<br /><br /></li><li>head&mdash;Optioneel. De relatieve koers of reisrichting van de client. Geef de reisrichting op als graden van 0 t/m 360, gerekend met de klok mee ten opzichte van het ware noorden. Geef deze sleutel alleen op als de `sp`-sleutel niet nul is.<br /><br /></li><li>SP&mdash; de horizontale snelheid (snelheid), in meters per seconde, die het client apparaat reist.<br /><br /></li><li>ALT&mdash; de hoogte van het client apparaat, in meters.<br /><br /></li><li>are&mdash;Optioneel. De straal, in meters, die de verticale nauwkeurigheid van de coördinaten aangeeft. RADIUS wordt standaard ingesteld op 50 kilo meters. Geef deze sleutel alleen op als u de `alt`-sleutel opgeeft.<br /><br /></li></ul> **OPMERKING:** Hoewel deze sleutels optioneel zijn, hoe meer informatie die u verstrekt, hoe nauw keuriger de resultaten van de locatie zijn.<br /><br /> **OPMERKING:** U wordt geadviseerd om altijd de geografische locatie van de gebruiker op te geven. Het opgeven van de locatie is vooral belangrijk als het IP-adres van de client de fysieke locatie van de gebruiker niet nauwkeurig weergeeft (bijvoorbeeld als de client VPN gebruikt). Voor optimale resultaten moet u zowel deze header als de X-MSEdge-ClientIP-header opnemen, maar neem minimaal deze header op.|

> [!NOTE] 
> Vergeet niet dat de gebruiksvoorwaarden naleving van alle toepasselijke wetgeving vereisen, inclusief die over het gebruik van deze headers. In bepaalde rechtsgebieden, zoals Europa, is het bijvoorbeeld vereist om toestemming van de gebruiker te verkrijgen voordat bepaalde traceringsapparaten op gebruikersapparaten wordt geplaatst.
  

## <a name="query-parameters"></a>Queryparameters  
De aanvraag kan de volgende query parameters bevatten. Zie de vereiste kolom voor de vereiste para meters. U moet de URL van de query parameters coderen.  
  
  
|Name|Value|type|Vereist|  
|----------|-----------|----------|--------------|  
|<a name="mkt" />mkt|De markt waaruit de resultaten afkomstig zijn. <br /><br />Zie markt codes voor een lijst met mogelijke markt waarden.<br /><br /> **OPMERKING:** De URL-preview-API ondersteunt momenteel alleen de markt en taal van de VS.<br /><br />|Tekenreeks|Ja|  
|<a name="query" />q|De URL voor de preview-versie|Tekenreeks|Ja|  
|<a name="responseformat" />responseFormat|Het media type dat voor het antwoord moet worden gebruikt. Hier volgen de mogelijke niet-hoofdletter gevoelige waarden.<br /><ul><li>JSON</li><li>JSONLD</li></ul><br /> De standaard waarde is JSON. Zie [Response Objects](#response-objects)(Engelstalig) voor informatie over de JSON-objecten die het antwoord bevat.<br /><br />  Als u JsonLd opgeeft, bevat de antwoord tekst JSON-LD objecten die de zoek resultaten bevatten. Zie [JSON-LD](https://json-ld.org/)voor meer informatie over de JSON-ld.|Reeks|Nee|  
|<a name="safesearch" />safeSearch|Een filter dat wordt gebruikt voor het filteren van inhoud voor volwassenen. Hier volgen de mogelijke niet-hoofdlettergevoelige filterwaarden.<br /><ul><li>Off&mdash;webpagina's retour neren met tekst, afbeeldingen of Video's voor volwassenen.<br /><br/></li><li>Gemiddeld&mdash;webpaginas retour neren met volwassene tekst, maar geen installatie kopieën of Video's voor volwassenen.<br /><br/></li><li>&mdash;Geen webpagina's retour neren met tekst, afbeeldingen of Video's voor volwassenen.</li></ul><br /> De standaardwaarde is Moderate.<br /><br /> **OPMERKING:** Als de aanvraag afkomstig `safeSearch` is van een markt waarvoor het volwassen beleid van Bing is ingesteld op strikt, negeert Bing de `safeSearch` waarde en wordt strikt gebruikt.<br/><br/>**OPMERKING:** Als u de `site:` operator query gebruikt, is het mogelijk dat het antwoord inhoud voor volwassenen bevat, ongeacht de `safeSearch` query parameter is ingesteld op. Gebruik `site:` alleen als u zich bewust bent van de inhoud op de site en uw scenario de mogelijkheid van inhoud voor volwassenen ondersteunt. |Tekenreeks|Nee|  
|<a name="setlang" />setLang|De taal die moet worden gebruikt voor gebruikersinterfacetekenreeksen. Geef de taal op met behulp van de tweeletterige ISO 639-1 taalcode. De taalcode voor Nederlands is bijvoorbeeld NL. De standaardwaarde is EN (Engels).<br /><br /> Hoewel dit optioneel is, moet u altijd de taal opgeven. Doorgaans stelt u `setLang` in op dezelfde taal die is opgegeven door `mkt`, tenzij de gebruiker wil dat gebruikersinterfacetekenreeksen in een andere taal worden weergegeven.<br /><br /> Deze parameter en de header [Accept-Language](#acceptlanguage) sluiten elkaar uit&mdash;geef ze niet beide op.<br /><br /> Een gebruikersinterfacetekenreeks is een tekenreeks die wordt gebruikt als label in een gebruikersinterface. Er zijn maar weinig gebruikersinterfacetekenreeksen in de JSON-antwoordobjecten. De opgegeven taal wordt ook toegepast op koppelingen naar Bing.com-eigenschappen in de antwoordobjecten.|Tekenreeks|Nee| 


## <a name="response-objects"></a>Antwoord objecten  
Het antwoord schema is een [webpagina] of ErrorResponse, zoals in de Webzoekopdrachten-API. Als de aanvraag mislukt, is het object op het hoogste niveau het [ErrorResponse](#errorresponse) -object.


|Object|Description|  
|------------|-----------------|  
|[WebPage]|JSON-object op het hoogste niveau dat kenmerken van de preview bevat.|  
|Feit|JSON-object op het hoogste niveau dat feiten bevat.| 
|[Entiteiten|JSON-object op het hoogste niveau dat entiteits gegevens bevat.| 

  
### <a name="error"></a>Fout  
Hiermee definieert u de fout die is opgetreden.  
  
|Element|Description|type|  
|-------------|-----------------|----------|  
|<a name="error-code" />gecodeerd|De fout code waarmee de fout categorie wordt aangeduid. Zie [fout codes](#error-codes)voor een lijst met mogelijke codes.|Tekenreeks|  
|<a name="error-message" />Bericht|Een beschrijving van de fout.|Tekenreeks|  
|<a name="error-moredetails" />moreDetails|Een beschrijving die extra informatie biedt over de fout.|Tekenreeks|  
|<a name="error-parameter" />parameter|De query parameter in de aanvraag die de fout heeft veroorzaakt.|Tekenreeks|  
|<a name="error-subcode" />subCode|De fout code die de fout identificeert. Als `code` bijvoorbeeld InvalidRequest is, `subCode` kan ParameterInvalid of ParameterInvalidValue zijn. |Tekenreeks|  
|<a name="error-value" />Value|De waarde van de query parameter die ongeldig is.|Reeks|  
  

### <a name="errorresponse"></a>ErrorResponse  
Het object op het hoogste niveau dat het antwoord bevat wanneer de aanvraag is mislukt.  
  
|Name|Value|type|  
|----------|-----------|----------|  
|_type|Type hint.|Reeks|  
|<a name="errors" />bufferoverschrijdingsfouten|Een lijst met fouten die de redenen beschrijven waarom de aanvraag is mislukt.|[Optreedt](#error)|  

  
  
### <a name="license"></a>Licentie  
Hiermee wordt de licentie gedefinieerd waarmee de tekst of foto kan worden gebruikt.  
  
|Name|Value|type|  
|----------|-----------|----------|  
|name|De naam van de licentie.|Tekenreeks|  
|url|De URL naar een website waar de gebruiker meer informatie over de licentie kan krijgen.<br /><br /> Gebruik de naam en URL om een Hyper link te maken.|Reeks|  
  

### <a name="licenseattribution"></a>LicenseAttribution  
Hiermee wordt een contractuele regel voor licentie toewijzing gedefinieerd.  
  
|Name|Value|type|  
|----------|-----------|----------|  
|_type|Een type hint, die is ingesteld op LicenseAttribution.|Tekenreeks|  
|licentie|De licentie waaronder de inhoud kan worden gebruikt.|[Houders](#license)|  
|licenseNotice|De licentie die moet worden weer gegeven naast het doel veld. Bijvoorbeeld ' tekst onder CC-BY-SA-licentie '.<br /><br /> Gebruik de naam en URL van de licentie in `license` het veld om een Hyper Link naar de website te maken met een beschrijving van de details van de licentie. Vervang vervolgens de licentie naam in de `licenseNotice` teken reeks (bijvoorbeeld CC-by-sa) door de Hyper link die u zojuist hebt gemaakt.|Reeks|  
|mustBeCloseToContent|Een Booleaanse waarde die bepaalt of de inhoud van de regel moet worden geplaatst in de buurt van het veld waarop de regel van toepassing is. Als dit het **geval**is, moet de inhoud dicht in de buurt worden geplaatst. Alsdeze eigenschap onwaar is of dit veld niet bestaat, kan de inhoud worden geplaatst op de keuze van de aanroeper.|Boolean-waarde|  
|targetPropertyName|De naam van het veld waarop de regel van toepassing is.|Reeks|  
  

### <a name="link"></a>Koppelen  
Hiermee worden de onderdelen van een Hyper Link gedefinieerd.  
  
|Name|Waarde|type|  
|----------|-----------|----------|  
|_type|Type hint.|Tekenreeks|  
|text|De weergave tekst.|Reeks|  
|url|EEN URL. Gebruik de URL en weergave tekst om een Hyper link te maken.|Tekenreeks|  
  

### <a name="linkattribution"></a>LinkAttribution  
Hiermee wordt een contractuele regel voor koppelings toewijzing gedefinieerd.  
  
|Name|Value|type|  
|----------|-----------|----------|  
|_type|Een type hint, die is ingesteld op LinkAttribution.|Tekenreeks|  
|mustBeCloseToContent|Een Booleaanse waarde die bepaalt of de inhoud van de regel moet worden geplaatst in de buurt van het veld waarop de regel van toepassing is. Als dit het **geval**is, moet de inhoud dicht in de buurt worden geplaatst. Alsdeze eigenschap onwaar is of dit veld niet bestaat, kan de inhoud worden geplaatst op de keuze van de aanroeper.|Boolean-waarde|  
|targetPropertyName|De naam van het veld waarop de regel van toepassing is.<br /><br /> Als er geen doel is opgegeven, wordt de toewijzing toegepast op de entiteit als geheel en moet deze direct na de entiteits presentatie worden weer gegeven. Als er meerdere regels voor tekst en koppelings toewijzing zijn die geen doel opgeven, moet u deze samen voegen en weer geven met behulp van een label "gegevens van:". Bijvoorbeeld ' gegevens van < Provider NAME1\> &#124; < Provider naam2\>'.|Tekenreeks|  
|text|De tekst van de toewijzing.|Tekenreeks|  
|url|De URL naar de website van de provider. Gebruik `text` en URL voor het maken van een Hyper link.|Reeks|  
  
  
### <a name="mediaattribution"></a>MediaAttribution  
Definieert een contractuele regel voor media-toewijzing.  
  
|Name|Value|type|  
|----------|-----------|----------|  
|_type|Een type hint, die is ingesteld op MediaAttribution.|Tekenreeks|  
|mustBeCloseToContent|Een Booleaanse waarde die bepaalt of de inhoud van de regel moet worden geplaatst in de buurt van het veld waarop de regel van toepassing is. Als dit het **geval**is, moet de inhoud dicht in de buurt worden geplaatst. Alsdeze eigenschap onwaar is of dit veld niet bestaat, kan de inhoud worden geplaatst op de keuze van de aanroeper.|Boolean-waarde|  
|targetPropertyName|De naam van het veld waarop de regel van toepassing is.|Tekenreeks|  
|url|De URL die u gebruikt om een Hyper Link van de media-inhoud te maken. Als het doel bijvoorbeeld een afbeelding is, gebruikt u de URL om de afbeelding op te geven.|Reeks|  
  
  
  
### <a name="organization"></a>Organisatie  
Hiermee wordt een uitgever gedefinieerd.  
  
Houd er rekening mee dat een uitgever hun naam of hun website of beide kan opgeven.  
  
|Name|Waarde|type|  
|----------|-----------|----------|  
|name|De naam van de uitgever.|Reeks|  
|url|De URL naar de website van de uitgever.<br /><br /> Houd er rekening mee dat de uitgever mogelijk geen website levert.|Tekenreeks|  
  
  

### <a name="webpage"></a>WebPage  
Definieert informatie over een webpagina in de preview-versie.  
  
|Name|Waarde|type|  
|----------|-----------|----------|
|name|De pagina titel, niet noodzakelijkerwijs de HTML-titel|Reeks|
|url|De URL die werkelijk is verkend (aanvraag is mogelijk doorlopende omleidingen)|Tekenreeks|  
|description|Korte beschrijving van de pagina en inhoud|Tekenreeks|  
|isFamilyFriendly|De meest nauw keurige voor items in de web-index; bij realtime ophalen worden deze detectie alleen gebaseerd op de URL en niet op de pagina-inhoud|boolean|
|primaryImageOfPage/contentUrl|De URL naar een representatieve afbeelding die moet worden meegenomen in de preview-versie|Reeks| 
  
  
### <a name="querycontext"></a>QueryContext  
Hiermee wordt de query context gedefinieerd die door Bing voor de aanvraag wordt gebruikt.  
  
|Element|Description|type|  
|-------------|-----------------|----------|  
|adultIntent|Een Booleaanse waarde die aangeeft of de opgegeven query volwassen intentie heeft. De waarde is **True** als de query volwassen intentie heeft; anders **False**.|Boolean-waarde|  
|alterationOverrideQuery|De query teken reeks die moet worden gebruikt om Bing te dwingen de oorspronkelijke teken reeks te gebruiken. Als de query reeks bijvoorbeeld *Saling Downwind*is, is de query reeks opheffen *+ Saling Downwind*. Vergeet niet om de query teken reeks te coderen die resulteert in *% 2Bsaling + Downwind*.<br /><br /> Dit veld wordt alleen opgenomen als de oorspronkelijke query reeks een spel fout bevat.|Tekenreeks|  
|alteredQuery|De query teken reeks die door Bing wordt gebruikt om de query uit te voeren. Bing gebruikt de gewijzigde query reeks als de oorspronkelijke query reeks spel fouten bevat. Als de query reeks bijvoorbeeld is `saling downwind`, `sailing downwind`wordt de gewijzigde query reeks.<br /><br /> Dit veld wordt alleen opgenomen als de oorspronkelijke query reeks een spel fout bevat.|Tekenreeks|  
|askUserForLocation|Een Booleaanse waarde die aangeeft of Bing de locatie van de gebruiker nodig heeft om nauw keurige resultaten te leveren. Als u de locatie van de gebruiker hebt opgegeven met behulp van de [x-MSEdge-client](#clientip) en [x-Search-locatie](#location) headers, kunt u dit veld negeren.<br /><br /> Voor locatie bewuste query's, zoals ' nu weer geven ' of ' restaurants vlakbij ' waarvoor de locatie van de gebruiker voor nauw keurige resultaten nodig is, wordt dit veld ingesteld op **True**.<br /><br /> Voor locatie bewuste query's die de locatie bevatten (bijvoorbeeld ' Seattle weer '), is dit veld ingesteld op **Onwaar**. Dit veld wordt ook ingesteld op **Onwaar** voor query's die geen locatie ondervinden, zoals ' best verkochte '.|Boolean-waarde|  
|originalQuery|De query reeks zoals opgegeven in de aanvraag.|Tekenreeks|  

### <a name="identifiable"></a>Persoonlijke

|Name|Waarde|type|  
|-------------|-----------------|----------|
|id|Een resource-id|Reeks|
 
### <a name="rankinggroup"></a>RankingGroup
Hiermee wordt een groep met zoek resultaten gedefinieerd, zoals Mainline.

|Name|Waarde|type|  
|-------------|-----------------|----------|
|items|Een lijst met zoek resultaten die in de groep moeten worden weer gegeven.|RankingItem|

### <a name="rankingitem"></a>RankingItem
Hiermee wordt een item in de zoek resultaten gedefinieerd dat moet worden weer gegeven.

|Name|Value|type|  
|-------------|-----------------|----------|
|resultIndex|Een op nul gebaseerde index van het item in het antwoord dat moet worden weer gegeven. Als het item dit veld niet bevat, geeft u alle items in het antwoord weer. U kunt bijvoorbeeld alle nieuws artikelen in het nieuws antwoord weer geven.|Geheel getal|
|answerType|Het antwoord dat het item bevat dat moet worden weer gegeven. Bijvoorbeeld nieuws.<br /><br />Gebruik het type om het antwoord te vinden in het SearchResponse-object. Het type is de naam van een SearchResponse-veld.<br /><br /> Gebruik echter alleen het antwoord type als dit object het veld waarde bevat. anders negeert u deze.|Tekenreeks|
|textualIndex|De index van het antwoord in textualAnswers dat moet worden weer gegeven.| Niet-ondertekend geheel getal|
|value|De ID waarmee een antwoord wordt weer gegeven of een item van een antwoord dat moet worden weer gegeven. Als de ID een antwoord identificeert, geeft u alle items van het antwoord weer.|Persoonlijke|

### <a name="rankingresponse"></a>RankingResponse  
Definieert waar de inhoud van de pagina met zoek resultaten moet worden geplaatst en in welke volg orde.  
  
|Name|Waarde|  
|----------|-----------|  
|<a name="ranking-mainline" />mainline|De zoek resultaten worden weer gegeven in de Mainline.|  
|<a name="ranking-pole" />pool|De zoek resultaten die de meest zicht bare behandeling moeten worden geboden (bijvoorbeeld weer gegeven boven de mainline en Sidebar).|  
|<a name="ranking-sidebar" />zijbalk|De zoek resultaten die in de zijbalk worden weer gegeven.| 


### <a name="searchresponse"></a>SearchResponse  
Hiermee definieert u het object op het hoogste niveau dat het antwoord bevat wanneer de aanvraag is voltooid.  
  
Houd er rekening mee dat als de service een DOS-aanval (Denial of service) vermoedt, de aanvraag slaagt (HTTP-status code is 200 OK). de hoofd tekst van de reactie is echter leeg.  
  
|Name|Value|type|  
|----------|-----------|----------|  
|_type|Type hint, die is ingesteld op SearchResponse.|Reeks|  
|WebPage|Een JSON-object dat de preview definieert|string|  
  
  
### <a name="textattribution"></a>TextAttribution  
Hiermee definieert u een contractuele regel voor de toewijzing van onbewerkte tekst.  
  
|Name|Value|type|  
|----------|-----------|----------|  
|_type|Een type hint, die is ingesteld op TextAttribution.|Tekenreeks|  
|text|De tekst van de toewijzing.<br /><br /> Tekst toewijzing is van toepassing op de entiteit als geheel en moet direct na de presentatie van de entiteit worden weer gegeven. Als er meerdere tekst-of koppelings regels zijn die geen doel opgeven, moet u deze samen voegen en weer geven met behulp van een label "gegevens van:".|Tekenreeks| 


## <a name="error-codes"></a>Foutcodes

Hier volgen de mogelijke HTTP-status codes die een aanvraag retourneert.  
  
|Statuscode|Description|  
|-----------------|-----------------|  
|200|Voltooid.|  
|400|Een van de query parameters ontbreekt of is ongeldig.|  
|401|De abonnements sleutel ontbreekt of is ongeldig.|  
|403|De gebruiker is geverifieerd (bijvoorbeeld omdat er een geldige abonnements sleutel is gebruikt), maar ze hebben geen machtiging voor de aangevraagde resource.<br /><br /> Bing kan ook deze status retour neren als de aanroeper het quotum voor query's per maand heeft overschreden.|  
|410|De aanvraag heeft HTTP gebruikt in plaats van het HTTPS-protocol. HTTPS is het enige ondersteunde protocol.|  
|429|De aanroeper heeft het quotum van de query's per seconde overschreden.|  
|500|Onverwachte server fout.|

Als de aanvraag mislukt, bevat het antwoord een [ErrorResponse](#errorresponse) -object, dat een lijst bevat met [fout](#error) objecten die beschrijven wat de oorzaak van het probleem is. Als de fout is gerelateerd aan een para meter, `parameter` identificeert het veld de para meter die het probleem vormt. En als de fout is gerelateerd aan een parameter waarde, identificeert het `value` veld de waarde die niet geldig is.

```json
{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidRequest", 
      "subCode": "ParameterMissing", 
      "message": "Required parameter is missing.", 
      "parameter": "q" 
    }
  ]
}

{
  "_type": "ErrorResponse", 
  "errors": [
    {
      "code": "InvalidAuthorization", 
      "subCode": "AuthorizationMissing", 
      "message": "Authorization is required.", 
      "moreDetails": "Subscription key is not recognized."
    }
  ]
}
```

Hier volgen de mogelijke fout code en waarden voor de subfout code.

|Code|SubCode|Description
|-|-|-
|ServerError|UnexpectedError<br/>ResourceError<br/>Niet geïmplementeerd|HTTP-status code is 500.
|InvalidRequest|ParameterMissing<br/>ParameterInvalidValue<br/>HttpNotAllowed<br/>Geblokkeerd|Bing retourneert InvalidRequest wanneer een deel van de aanvraag ongeldig is. Een vereiste para meter ontbreekt bijvoorbeeld of een parameter waarde is niet geldig.<br/><br/>Als de fout ParameterMissing of ParameterInvalidValue is, is de HTTP-status code 400.<br/><br/>Als u het HTTP-protocol gebruikt in plaats van HTTPS, retourneert Bing HttpNotAllowed en de HTTP-status code is 410.
|RateLimitExceeded|Geen subcodes|Bing retourneert RateLimitExceeded wanneer u het quotum voor query's per seconde (QPS) of query's per maand (QPM) overschrijdt.<br/><br/>Als u QPS overschrijdt, retourneert Bing de HTTP-status code 429 en als u QPM overschrijdt, retourneert Bing 403.
|InvalidAuthorization|AuthorizationMissing<br/>AuthorizationRedundancy|Bing retourneert InvalidAuthorization wanneer Bing de oproepende functie niet kan verifiëren. De `Ocp-Apim-Subscription-Key` koptekst ontbreekt bijvoorbeeld of de abonnements sleutel is niet geldig.<br/><br/>Redundantie treedt op als u meer dan één verificatie methode opgeeft.<br/><br/>Als de fout InvalidAuthorization is, is de HTTP-status code 401.
|InsufficientAuthorization|AuthorizationDisabled<br/>AuthorizationExpired|Bing retourneert InsufficientAuthorization wanneer de aanroeper geen machtigingen heeft voor toegang tot de resource. Dit kan gebeuren als de abonnements sleutel is uitgeschakeld of is verlopen. <br/><br/>Als de fout InsufficientAuthorization is, is de HTTP-status code 403.

## <a name="next-steps"></a>Volgende stappen
- [Snelstart voor C#](c-sharp-quickstart.md)
- [Snelstart voor Java](java-quickstart.md)
- [Snelstart voor Node](node-quickstart.md)
- [Snelstartgids voor Python](python-quickstart.md)

