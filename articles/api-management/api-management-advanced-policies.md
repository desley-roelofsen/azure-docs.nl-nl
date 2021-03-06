---
title: Geavanceerde beleids regels voor Azure API Management | Microsoft Docs
description: Meer informatie over het geavanceerde beleid dat beschikbaar is voor gebruik in azure API Management.
services: api-management
documentationcenter: ''
author: vladvino
manager: erikre
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 11/28/2017
ms.author: apimpm
ms.openlocfilehash: 225f26ac2133f45fe7eba9e39d64d0cfe9e20766
ms.sourcegitcommit: cf36df8406d94c7b7b78a3aabc8c0b163226e1bc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/09/2019
ms.locfileid: "73885291"
---
# <a name="api-management-advanced-policies"></a>Geavanceerde beleids regels API Management

In dit onderwerp vindt u een verwijzing naar de volgende API Management-beleids regels. Zie [beleid in API Management](https://go.microsoft.com/fwlink/?LinkID=398186)voor meer informatie over het toevoegen en configureren van beleid.

## <a name="AdvancedPolicies"></a>Geavanceerde beleids regels

-   [Controle stroom](api-management-advanced-policies.md#choose) -voorwaardelijk past beleids instructies toe op basis van de resultaten van de evaluatie van Boole- [expressies](api-management-policy-expressions.md).
-   [Aanvraag door sturen](#ForwardRequest) : stuurt de aanvraag door naar de back-end-service.
-   [Gelijktijdigheid beperken](#LimitConcurrency) : hiermee voor komt u dat beleid wordt uitgevoerd door meer dan het opgegeven aantal aanvragen per keer.
-   [Aanmelden bij Event hub](#log-to-eventhub) : verzendt berichten in de opgegeven indeling naar een event hub die is gedefinieerd door een logboek registratie-entiteit.
-   Verwaak [antwoord](#mock-response) : Hiermee wordt de uitvoering van de pijp lijn afgebroken en wordt een gesimuleerde respons rechtstreeks naar de oproepende functie geretourneerd.
-   [Nieuwe](#Retry) pogingen: de uitvoering van de Inge sloten beleids instructies opnieuw proberen, indien en tot aan de voor waarde wordt voldaan. De uitvoering wordt herhaald op de opgegeven tijds intervallen en tot het opgegeven aantal nieuwe pogingen.
-   [Retour antwoord](#ReturnResponse) : Hiermee wordt de uitvoering van de pijp lijn afgebroken en wordt het opgegeven antwoord rechtstreeks naar de aanroeper geretourneerd.
-   Eenrichtings [aanvraag verzenden](#SendOneWayRequest) : Hiermee verzendt u een aanvraag naar de opgegeven URL zonder te wachten op een reactie.
-   [Aanvraag verzenden](#SendRequest) : Hiermee verzendt u een aanvraag naar de opgegeven URL.
-   [Http-proxy instellen](#SetHttpProxy) : Hiermee kunt u doorgestuurde aanvragen via een http-proxy routeren.
-   [Set-aanvraag methode](#SetRequestMethod) : Hiermee kunt u de HTTP-methode voor een aanvraag wijzigen.
-   [Status code instellen](#SetStatus) : Hiermee wijzigt u de HTTP-status code in de opgegeven waarde.
-   [Set variable](api-management-advanced-policies.md#set-variable) : persistent een waarde in een benoemde [context](api-management-policy-expressions.md#ContextVariables) variabele voor later toegang.
-   [Trace](#Trace) : voegt aangepaste traceringen toe aan de [API-Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) -uitvoer, Application Insights-Telerichtingen en Diagnostische logboeken.
-   [Wacht](#Wait) op een Inge sloten [verzend aanvraag](api-management-advanced-policies.md#SendRequest), haal de [waarde uit het cache geheugen](api-management-caching-policies.md#GetFromCacheByKey)of Controleer of [controle stroom](api-management-advanced-policies.md#choose) beleid is voltooid voordat u doorgaat.

## <a name="choose"></a>Controle stroom

In het `choose` beleid worden inge sloten beleids overzichten toegepast op basis van het resultaat van de evaluatie van Boole-expressies, vergelijkbaar met een if-then-else-of een switch-constructie in een programmeer taal.

### <a name="ChoosePolicyStatement"></a>Beleids verklaring

```xml
<choose>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <when condition="Boolean expression | Boolean constant">
        <!— one or more policy statements to be applied if the above condition is true  -->
    </when>
    <otherwise>
        <!— one or more policy statements to be applied if none of the above conditions are true  -->
</otherwise>
</choose>
```

Het controle stroom beleid moet ten minste één `<when/>` element bevatten. Het `<otherwise/>`-element is optioneel. De voor waarden in `<when/>` elementen worden geëvalueerd in de volg orde waarin ze in het beleid worden weer gegeven. Beleids verklaring (en) die zijn Inge sloten in het eerste `<when/>`-element met voorwaarde kenmerk is gelijk aan `true` wordt toegepast. Beleids regels die zijn Inge sloten in het element `<otherwise/>`, indien aanwezig, worden toegepast als alle kenmerken van de `<when/>` element voorwaarde `false`zijn.

### <a name="examples"></a>Voorbeelden

#### <a name="ChooseExample"></a>Hierbij

In het volgende voor beeld ziet u een [set-variabele](api-management-advanced-policies.md#set-variable) beleid en twee controle stroom beleidsregels.

Het set-variabele beleid bevindt zich in de sectie binnenkomend en maakt een `isMobile` Boole- [context](api-management-policy-expressions.md#ContextVariables) variabele die is ingesteld op True als de `User-Agent` aanvraag header de tekst `iPad` of het `iPhone`bevat.

Het eerste controle stroom beleid bevindt zich ook in de sectie binnenkomend en past voorwaardelijk een van twee [set para meter beleid voor query reeksen](api-management-transformation-policies.md#SetQueryStringParameter) toe, afhankelijk van de waarde van de variabele `isMobile` context.

Het tweede controle stroom beleid bevindt zich in de sectie voor uitgaand verkeer en past op voor waarde het [conversie bestand XML naar JSON-beleid toe](api-management-transformation-policies.md#ConvertXMLtoJSON) als `isMobile` is ingesteld op `true`.

```xml
<policies>
    <inbound>
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />
        <base />
        <choose>
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">
                <set-query-parameter name="mobile" exists-action="override">
                    <value>true</value>
                </set-query-parameter>
            </when>
            <otherwise>
                <set-query-parameter name="mobile" exists-action="override">
                    <value>false</value>
                </set-query-parameter>
            </otherwise>
        </choose>
    </inbound>
    <outbound>
        <base />
        <choose>
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>
            </when>
        </choose>
    </outbound>
</policies>
```

#### <a name="example"></a>Voorbeeld

In dit voor beeld ziet u hoe u het filteren van inhoud uitvoert door gegevens elementen te verwijderen uit het antwoord dat is ontvangen van de back-end-service wanneer u het `Starter` product gebruikt. Zie voor een demonstratie van het configureren en gebruiken van dit beleid [Cloud cover aflevering 177: meer API management functies met Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) en Fast-forward to 34:30. Begin om 31:50 om een overzicht te zien van [de donkerly Forecast API](https://developer.forecast.io/) die wordt gebruikt voor deze demo.

```xml
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->
<choose>
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">
    <set-body>@{
        var response = context.Response.Body.As<JObject>();
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {
          response.Property (key).Remove ();
        }
        return response.ToString();
      }
    </set-body>
  </when>
</choose>
```

### <a name="elements"></a>Elementen

| Element   | Beschrijving                                                                                                                                                                                                                                                               | Vereist |
| --------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- |
| desgewenst    | Hoofd element.                                                                                                                                                                                                                                                             | Ja      |
| Als      | De voor waarde die moet worden gebruikt voor de `if` of `ifelse` onderdelen van het `choose`-beleid. Als het `choose` beleid meerdere `when` secties heeft, worden ze opeenvolgend geëvalueerd. Zodra de `condition` van een wanneer-element naar `true`wordt geëvalueerd, worden er geen verdere `when` voor waarden geëvalueerd. | Ja      |
| tenzij | Bevat het beleids fragment dat moet worden gebruikt als aan geen van de `when` omstandigheden `true`worden geëvalueerd.                                                                                                                                                                               | Nee       |

### <a name="attributes"></a>Kenmerken

| Kenmerk                                              | Beschrijving                                                                                               | Vereist |
| ------------------------------------------------------ | --------------------------------------------------------------------------------------------------------- | -------- |
| condition = "Boole- &#124; expressie Booleaanse constante" | De booleaanse expressie of-constante die moet worden geëvalueerd wanneer de insluitende `when`-beleids instructie wordt geëvalueerd. | Ja      |

### <a name="ChooseUsage"></a>Belasting

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="ForwardRequest"></a>Aanvraag door sturen

Het `forward-request` beleid stuurt de inkomende aanvraag door naar de back-end-service die is opgegeven in de [context](api-management-policy-expressions.md#ContextVariables)van de aanvraag. De URL van de back-end-service is opgegeven in de API- [instellingen](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) en kan worden gewijzigd met het beleid voor het [instellen van back-upservices](api-management-transformation-policies.md) .

> [!NOTE]
> Als u dit beleid verwijdert, worden de aanvragen die niet worden doorgestuurd naar de back-end-service en het beleid in de sectie voor uitgaande verbindingen onmiddellijk geëvalueerd wanneer het beleid is voltooid in de sectie binnenkomend.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<forward-request timeout="time in seconds" follow-redirects="true | false" buffer-request-body="true | false" />
```

### <a name="examples"></a>Voorbeelden

#### <a name="example"></a>Voorbeeld

Het volgende API-niveau beleid stuurt alle API-aanvragen door naar de back-end-service met een time-outinterval van 60 seconden.

```xml
<!-- api level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <forward-request timeout="60"/>
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

#### <a name="example"></a>Voorbeeld

Dit beleid op bewerking niveau gebruikt het `base`-element om het back-end-beleid over te nemen van het bereik van het bovenliggende API-niveau.

```xml
<!-- operation level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <base/>
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

#### <a name="example"></a>Voorbeeld

Dit beleid op bewerking niveau stuurt expliciet alle aanvragen door naar de back-end-service met een time-out van 120 en neemt het back-upbeleid van het bovenliggende API-niveau niet over.

```xml
<!-- operation level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <forward-request timeout="120"/>
        <!-- effective policy. note the absence of <base/> -->
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

#### <a name="example"></a>Voorbeeld

Dit beleid op bewerking niveau stuurt geen aanvragen door naar de back-end-service.

```xml
<!-- operation level -->
<policies>
    <inbound>
        <base/>
    </inbound>
    <backend>
        <!-- no forwarding to backend -->
    </backend>
    <outbound>
        <base/>
    </outbound>
</policies>

```

### <a name="elements"></a>Elementen

| Element         | Beschrijving   | Vereist |
| --------------- | ------------- | -------- |
| door sturen-aanvraag | Hoofd element. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk                               | Beschrijving                                                                                                      | Vereist | Standaard     |
| --------------------------------------- | ---------------------------------------------------------------------------------------------------------------- | -------- | ----------- |
| timeout = "geheel getal"                       | De hoeveelheid tijd in seconden die moet worden gewacht voordat de HTTP-antwoord headers worden geretourneerd door de back-end-service voordat een time-outfout optreedt. De minimum waarde is 0 seconden. Waarden die groter zijn dan 240 seconden, worden mogelijk niet geaccepteerd omdat de onderliggende netwerk infrastructuur na deze tijd niet-actieve verbindingen kan verwijderen. | Nee       | Geen |
| follow-redirects = "True &#124; False"    | Hiermee geeft u op of omleidingen van de back-end-service worden gevolgd door de gateway of worden geretourneerd naar de aanroeper.      | Nee       | onwaar       |
| buffer-Request-Body = "True &#124; False" | Wanneer de aanvraag is ingesteld op ' True ', wordt de buffer opgeslagen en wordt deze opnieuw gebruikt bij [opnieuw proberen](api-management-advanced-policies.md#Retry). | Nee       | onwaar       |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** back-end
-   **Beleids bereik:** alle bereiken

## <a name="LimitConcurrency"></a>Gelijktijdigheid beperken

Het `limit-concurrency` beleid voor komt dat het Inge sloten beleid wordt uitgevoerd door meer dan het opgegeven aantal aanvragen op elk gewenst moment. Bij overschrijding van dat aantal, zullen nieuwe aanvragen niet onmiddellijk worden uitgevoerd met een status code van 429 te veel aanvragen.

### <a name="LimitConcurrencyStatement"></a>Beleids verklaring

```xml
<limit-concurrency key="expression" max-count="number">
        <!— nested policy statements -->
</limit-concurrency>
```

### <a name="examples"></a>Voorbeelden

#### <a name="example"></a>Voorbeeld

In het volgende voor beeld ziet u hoe u het aantal aanvragen dat wordt doorgestuurd naar een back-end kunt beperken op basis van de waarde van een context variabele.

```xml
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Elementen

| Element           | Beschrijving   | Vereist |
| ----------------- | ------------- | -------- |
| limiet-gelijktijdigheid | Hoofd element. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk | Beschrijving                                                                                        | Vereist | Standaard |
| --------- | -------------------------------------------------------------------------------------------------- | -------- | ------- |
| sleutel       | Een teken reeks. Expressie toegestaan. Hiermee geeft u het gelijktijdigheids bereik op. Kan worden gedeeld door meerdere beleids regels. | Ja      | N.v.t.     |
| maximum aantal | Een geheel getal. Hiermee geeft u een maximum aantal aanvragen op dat het beleid mag invoeren.           | Ja      | N.v.t.     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="log-to-eventhub"></a>Aanmelden bij Event hub

Het `log-to-eventhub` beleid verzendt berichten in de opgegeven indeling naar een event hub die is gedefinieerd door een logboek registratie-entiteit. Zoals de naam al aangeeft, wordt het beleid gebruikt voor het opslaan van de geselecteerde aanvraag-of antwoord context gegevens voor online-of offline analyse.

> [!NOTE]
> Zie [API Management gebeurtenissen registreren met Azure Event hubs](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/)voor een stapsgewijze hand leiding voor het configureren van een event hub en logboek registratie van gebeurtenissen.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">
  Expression returning a string to be logged
</log-to-eventhub>

```

### <a name="example"></a>Voorbeeld

Een wille keurige teken reeks kan worden gebruikt als de waarde die moet worden aangemeld Event Hubs. In dit voor beeld worden de datum en tijd, de naam van de implementatie service, de aanvraag-id, het IP-adres en de bewerkings naam voor alle inkomende aanroepen geregistreerd in de Event Hub logger geregistreerd met de `contoso-logger`-id.

```xml
<policies>
  <inbound>
    <log-to-eventhub logger-id ='contoso-logger'>
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )
    </log-to-eventhub>
  </inbound>
  <outbound>
  </outbound>
</policies>
```

### <a name="elements"></a>Elementen

| Element         | Beschrijving                                                                     | Vereist |
| --------------- | ------------------------------------------------------------------------------- | -------- |
| aanmelden bij eventhub | Hoofd element. De waarde van dit element is de teken reeks die moet worden aangemeld bij uw Event Hub. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk     | Beschrijving                                                               | Vereist                                                             |
| ------------- | ------------------------------------------------------------------------- | -------------------------------------------------------------------- |
| logger-id     | De id van de logboek registratie die is geregistreerd bij uw API Management service.         | Ja                                                                  |
| partitie-id  | Hiermee geeft u de index op van de partitie waarnaar berichten worden verzonden.             | Optioneel. Dit kenmerk mag niet worden gebruikt als `partition-key` wordt gebruikt. |
| partitie sleutel | Hiermee geeft u de waarde op die wordt gebruikt voor partitie toewijzing wanneer berichten worden verzonden. | Optioneel. Dit kenmerk mag niet worden gebruikt als `partition-id` wordt gebruikt.  |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="mock-response"></a>Reactie van model

De `mock-response`, zoals de naam al aangeeft, wordt gebruikt voor het aftrekken van Api's en bewerkingen. De normale uitvoering van de pijp lijn wordt afgebroken en er wordt een gesimuleerde reactie op de oproepende functie geretourneerd. Het beleid probeert altijd antwoorden te retour neren van de hoogste betrouw baarheid. De voor beelden van antwoord inhoud, indien beschikbaar, worden weer gegeven. Er worden voorbeeld reacties van schema's gegenereerd, wanneer schema's worden gegeven en voor beelden niet. Als er geen voor beelden of schema's worden gevonden, worden antwoorden zonder inhoud geretourneerd.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<mock-response status-code="code" content-type="media type"/>

```

### <a name="examples"></a>Voorbeelden

```xml
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>
```

### <a name="elements"></a>Elementen

| Element       | Beschrijving   | Vereist |
| ------------- | ------------- | -------- |
| model-reactie | Hoofd element. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk    | Beschrijving                                                                                           | Vereist | Standaard |
| ------------ | ----------------------------------------------------------------------------------------------------- | -------- | ------- |
| status-code  | Hiermee geeft u de antwoord status code op en wordt gebruikt om het bijbehorende voor beeld of schema te selecteren.                 | Nee       | 200     |
| inhouds type | Hiermee geeft u de waarde van `Content-Type`-antwoord header op en wordt gebruikt om het bijbehorende voor beeld of schema te selecteren. | Nee       | Geen    |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, op fouten

-   **Beleids bereik:** alle bereiken

## <a name="Retry"></a>Voeren

Het `retry` beleid voert het onderliggende beleid eenmaal uit en probeert vervolgens de uitvoering ervan totdat de poging tot `condition` opnieuw wordt `false` of het `count` opnieuw wordt uitgevoerd.

### <a name="policy-statement"></a>Beleids verklaring

```xml

<retry
    condition="boolean expression or literal"
    count="number of retry attempts"
    interval="retry interval in seconds"
    max-interval="maximum retry interval in seconds"
    delta="retry interval delta in seconds"
    first-fast-retry="boolean expression or literal">
        <!-- One or more child policies. No restrictions -->
</retry>

```

### <a name="example"></a>Voorbeeld

In het volgende voor beeld wordt het door sturen van aanvragen tot tien keer opnieuw geprobeerd met behulp van een exponentieel nieuwe algoritme. Omdat `first-fast-retry` is ingesteld op ONWAAR, zijn alle nieuwe pogingen onderhevig aan het exponentieel nieuwe algoritme.

```xml

<retry
    condition="@(context.Response.StatusCode == 500)"
    count="10"
    interval="10"
    max-interval="100"
    delta="10"
    first-fast-retry="false">
        <forward-request buffer-request-body="true" />
</retry>

```

### <a name="elements"></a>Elementen

| Element | Beschrijving                                                         | Vereist |
| ------- | ------------------------------------------------------------------- | -------- |
| retry   | Hoofd element. Kan elk ander beleid als onderliggende elementen bevatten. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk        | Beschrijving                                                                                                                                           | Vereist | Standaard |
| ---------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| regeling        | Een letterlijke Booleaanse waarde of [expressie](api-management-policy-expressions.md) die aangeeft of nieuwe pogingen moeten worden gestopt (`false`) of blijven (`true`).      | Ja      | N.v.t.     |
| count            | Een positief getal dat het maximum aantal pogingen aangeeft dat moet worden geprobeerd.                                                                                | Ja      | N.v.t.     |
| interval         | Een positief getal in seconden dat het wacht interval tussen nieuwe pogingen aangeeft.                                                                 | Ja      | N.v.t.     |
| Max-interval     | Een positief getal in seconden voor het maximale wacht interval tussen nieuwe pogingen. Het wordt gebruikt voor het implementeren van een exponentieel nieuwe algoritme. | Nee       | N.v.t.     |
| Delta            | Een positief getal in seconden voor de toename van het wacht interval. Het wordt gebruikt om de algoritmen voor lineaire en exponentiële pogingen te implementeren.             | Nee       | N.v.t.     |
| eerste snelle poging | Als deze is ingesteld op `true`, wordt de eerste poging om het opnieuw te proberen onmiddellijk uitgevoerd.                                                                                  | Nee       | `false` |

> [!NOTE]
> Wanneer alleen de `interval` is opgegeven, worden nieuwe pogingen tot een **vast** interval uitgevoerd.
> Wanneer alleen `interval` en `delta` zijn opgegeven, wordt een algoritme voor een nieuwe poging voor een **lineair** interval gebruikt, waarbij de wacht tijd tussen nieuwe pogingen wordt berekend volgens de volgende formule-`interval + (count - 1)*delta`.
> Wanneer de `interval`, `max-interval` en `delta` zijn opgegeven, wordt het algoritme voor het opnieuw proberen van **exponentiële** interval toegepast, waarbij de wacht tijd tussen nieuwe pogingen exponentieel toeneemt van de waarde van `interval` tot de waarde `max-interval` volgens de volgende formule-`min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) . Houd er rekening mee dat de beperkingen voor het onderliggende beleid worden overgenomen door dit beleid.

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="ReturnResponse"></a>Antwoord retour neren

Het `return-response`-beleid breekt de uitvoering van de pijp lijn af en retourneert een standaard-of aangepast antwoord naar de aanroeper. Standaard antwoord wordt `200 OK` zonder hoofd tekst. Aangepaste antwoorden kunnen worden opgegeven via een context variabele of beleids instructies. Wanneer beide zijn opgegeven, wordt het antwoord dat is opgenomen in de context variabele, gewijzigd door de beleids verklaringen voordat ze worden geretourneerd naar de aanroeper.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<return-response response-variable-name="existing context variable">
  <set-header/>
  <set-body/>
  <set-status/>
</return-response>

```

### <a name="example"></a>Voorbeeld

```xml
<return-response>
   <set-status code="401" reason="Unauthorized"/>
   <set-header name="WWW-Authenticate" exists-action="override">
      <value>Bearer error="invalid_token"</value>
   </set-header>
</return-response>

```

### <a name="elements"></a>Elementen

| Element         | Beschrijving                                                                               | Vereist |
| --------------- | ----------------------------------------------------------------------------------------- | -------- |
| retour-antwoord | Hoofd element.                                                                             | Ja      |
| set-header      | Een [ingestelde header-](api-management-transformation-policies.md#SetHTTPheader) beleids instructie. | Nee       |
| set-Body        | Een instructie [set-Body-](api-management-transformation-policies.md#SetBody) beleid.         | Nee       |
| set-status      | Een instructie van een [set-status](api-management-advanced-policies.md#SetStatus) beleid.           | Nee       |

### <a name="attributes"></a>Kenmerken

| Kenmerk              | Beschrijving                                                                                                                                                                          | Vereist  |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ | --------- |
| reactie-variabele-naam | De naam van de context variabele waarnaar wordt verwezen, bijvoorbeeld een upstream-beleid voor [verzenden/aanvragen](api-management-advanced-policies.md#SendRequest) en een `Response`-object bevat | Optioneel. |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="SendOneWayRequest"></a>Een eenrichtings aanvraag verzenden

Het `send-one-way-request` beleid verzendt de opgegeven aanvraag naar de opgegeven URL zonder te wachten op een reactie.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<send-one-way-request mode="new | copy">
  <url>...</url>
  <method>...</method>
  <header name="" exists-action="override | skip | append | delete">...</header>
  <body>...</body>
  <authentication-certificate thumbprint="thumbprint" />
</send-one-way-request>

```

### <a name="example"></a>Voorbeeld

Dit voorbeeld beleid toont een voor beeld van het gebruik van het `send-one-way-request`-beleid om een bericht te verzenden naar een toegestane vertragings ruimte als de HTTP-antwoord code groter is dan of gelijk is aan 500. Zie [externe services van de Azure API Management-service gebruiken](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)voor meer informatie over dit voor beeld.

```xml
<choose>
    <when condition="@(context.Response.StatusCode >= 500)">
      <send-one-way-request mode="new">
        <set-url>https://hooks.slack.com/services/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</set-url>
        <set-method>POST</set-method>
        <set-body>@{
                return new JObject(
                        new JProperty("username","APIM Alert"),
                        new JProperty("icon_emoji", ":ghost:"),
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                context.Request.Method,
                                                context.Request.Url.Path + context.Request.Url.QueryString,
                                                context.Request.Url.Host,
                                                context.Response.StatusCode,
                                                context.Response.StatusReason,
                                                context.User.Email
                                                ))
                        ).ToString();
            }</set-body>
      </send-one-way-request>
    </when>
</choose>

```

### <a name="elements"></a>Elementen

| Element                    | Beschrijving                                                                                                 | Vereist                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------- |
| Send-eenrichtings aanvraag       | Hoofd element.                                                                                               | Ja                             |
| url                        | De URL van de aanvraag.                                                                                     | Geen if-modus = kopiëren; anders Ja. |
| method                     | De HTTP-methode voor de aanvraag.                                                                            | Geen if-modus = kopiëren; anders Ja. |
| koptekst                     | Aanvraag header. Meerdere koptekst elementen gebruiken voor meerdere aanvraag headers.                                  | Nee                              |
| organen                       | De aanvraag tekst.                                                                                           | Nee                              |
| verificatie-certificaat | [Certificaat dat moet worden gebruikt voor client verificatie](api-management-authentication-policies.md#ClientCertificate) | Nee                              |

### <a name="attributes"></a>Kenmerken

| Kenmerk     | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Vereist | Standaard  |
| ------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------- |
| mode = "teken reeks" | Hiermee wordt bepaald of dit een nieuwe aanvraag of een kopie van de huidige aanvraag is. In de modus uitgaand, wordt in modus = kopiëren de hoofd tekst van de aanvraag niet geïnitialiseerd.                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Nee       | Nieuw      |
| naam          | Hiermee geeft u de naam van de in te stellen header op.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Ja      | N.v.t.      |
| exists-actie | Hiermee geeft u op welke actie moet worden ondernomen wanneer de header al is opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -Override: vervangt de waarde van de bestaande header.<br />-Skip-vervangt niet de bestaande waarde van de header.<br />-append-de waarde wordt toegevoegd aan de bestaande waarde van de header.<br />-delete: verwijdert de header uit de aanvraag.<br /><br /> Wanneer deze instelling is ingesteld op `override` het aanmelden van meerdere vermeldingen met dezelfde naam resulteert in de header die wordt ingesteld op basis van alle vermeldingen (die meerdere keren worden weer gegeven). alleen waarden die worden weer gegeven, worden ingesteld in het resultaat. | Nee       | Overschrijven |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="SendRequest"></a>Aanvraag verzenden

Het `send-request` beleid verzendt de opgegeven aanvraag naar de opgegeven URL, die niet langer is dan de ingestelde time-outwaarde.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error
="false|true">
  <set-url>...</set-url>
  <set-method>...</set-method>
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>
  <set-body>...</set-body>
  <authentication-certificate thumbprint="thumbprint" />
</send-request>

```

### <a name="example"></a>Voorbeeld

In dit voor beeld ziet u één manier om een referentie token te verifiëren met een autorisatie server. Zie [externe services van de Azure API Management-service gebruiken](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)voor meer informatie over dit voor beeld.

```xml
<inbound>
  <!-- Extract Token from Authorization header parameter -->
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />

  <!-- Send request to Token Server to validate token (see RFC 7662) -->
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>
    <set-method>POST</set-method>
    <set-header name="Authorization" exists-action="override">
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>
    </set-header>
    <set-header name="Content-Type" exists-action="override">
      <value>application/x-www-form-urlencoded</value>
    </set-header>
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>
  </send-request>

  <choose>
        <!-- Check active property in response -->
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
            <!-- Return 401 Unauthorized with http-problem payload -->
            <return-response>
                <set-status code="401" reason="Unauthorized" />
                <set-header name="WWW-Authenticate" exists-action="override">
                    <value>Bearer error="invalid_token"</value>
                </set-header>
            </return-response>
        </when>
    </choose>
  <base />
</inbound>

```

### <a name="elements"></a>Elementen

| Element                    | Beschrijving                                                                                                 | Vereist                        |
| -------------------------- | ----------------------------------------------------------------------------------------------------------- | ------------------------------- |
| verzenden/aanvragen               | Hoofd element.                                                                                               | Ja                             |
| url                        | De URL van de aanvraag.                                                                                     | Geen if-modus = kopiëren; anders Ja. |
| method                     | De HTTP-methode voor de aanvraag.                                                                            | Geen if-modus = kopiëren; anders Ja. |
| koptekst                     | Aanvraag header. Meerdere koptekst elementen gebruiken voor meerdere aanvraag headers.                                  | Nee                              |
| organen                       | De aanvraag tekst.                                                                                           | Nee                              |
| verificatie-certificaat | [Certificaat dat moet worden gebruikt voor client verificatie](api-management-authentication-policies.md#ClientCertificate) | Nee                              |

### <a name="attributes"></a>Kenmerken

| Kenmerk                       | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Vereist | Standaard  |
| ------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | -------- |
| mode = "teken reeks"                   | Hiermee wordt bepaald of dit een nieuwe aanvraag of een kopie van de huidige aanvraag is. In de modus uitgaand, wordt in modus = kopiëren de hoofd tekst van de aanvraag niet geïnitialiseerd.                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Nee       | Nieuw      |
| Response-variabele-name = "string" | De naam van de context variabele waarmee een antwoord object wordt ontvangen. Als de variabele niet bestaat, wordt deze gemaakt na een geslaagde uitvoering van het beleid en wordt deze beschikbaar via [`context.Variable`](api-management-policy-expressions.md#ContextVariables) verzameling.                                                                                                                                                                                                                                                                                                                          | Ja      | N.v.t.      |
| timeout = "geheel getal"               | Het time-outinterval in seconden voordat de aanroep van de URL mislukt.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           | Nee       | 60       |
| negeren-fout                    | Indien waar en de aanvraag resulteert in een fout:<br /><br /> -Als de reactie variabele-name is opgegeven, bevat deze een null-waarde.<br />-Als Response-variabele-name niet is opgegeven, context. De aanvraag wordt niet bijgewerkt.                                                                                                                                                                                                                                                                                                                                                                                   | Nee       | onwaar    |
| naam                            | Hiermee geeft u de naam van de in te stellen header op.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | Ja      | N.v.t.      |
| exists-actie                   | Hiermee geeft u op welke actie moet worden ondernomen wanneer de header al is opgegeven. Dit kenmerk moet een van de volgende waarden hebben.<br /><br /> -Override: vervangt de waarde van de bestaande header.<br />-Skip-vervangt niet de bestaande waarde van de header.<br />-append-de waarde wordt toegevoegd aan de bestaande waarde van de header.<br />-delete: verwijdert de header uit de aanvraag.<br /><br /> Wanneer deze instelling is ingesteld op `override` het aanmelden van meerdere vermeldingen met dezelfde naam resulteert in de header die wordt ingesteld op basis van alle vermeldingen (die meerdere keren worden weer gegeven). alleen waarden die worden weer gegeven, worden ingesteld in het resultaat. | Nee       | Overschrijven |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="SetHttpProxy"></a>HTTP-proxy instellen

Met het `proxy`-beleid kunt u aanvragen die via een HTTP-proxy worden doorgestuurd naar back-endservers routeren. Alleen HTTP (niet HTTPS) wordt ondersteund tussen de gateway en de proxy. Alleen basis-en NTLM-verificatie.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<proxy url="http://hostname-or-ip:port" username="username" password="password" />

```

### <a name="example"></a>Voorbeeld

Let op het gebruik van [Eigenschappen](api-management-howto-properties.md) als waarden van de gebruikers naam en het wacht woord om te voor komen dat gevoelige informatie wordt opgeslagen in het beleids document.

```xml
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />

```

### <a name="elements"></a>Elementen

| Element | Beschrijving  | Vereist |
| ------- | ------------ | -------- |
| webtoepassingsproxy   | Hoofd element | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk         | Beschrijving                                            | Vereist | Standaard |
| ----------------- | ------------------------------------------------------ | -------- | ------- |
| URL = "teken reeks"      | Proxy-URL in de vorm van http://host:port.             | Ja      | N.v.t.     |
| username = "teken reeks" | De gebruikers naam die moet worden gebruikt voor verificatie met de proxy. | Nee       | N.v.t.     |
| Password = "teken reeks" | Het wacht woord dat moet worden gebruikt voor verificatie met de proxy. | Nee       | N.v.t.     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** binnenkomend

-   **Beleids bereik:** alle bereiken

## <a name="SetRequestMethod"></a>Set-aanvraag methode

Met het `set-method`-beleid kunt u de HTTP-aanvraag methode voor een aanvraag wijzigen.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<set-method>METHOD</set-method>

```

### <a name="example"></a>Voorbeeld

In dit voorbeeld beleid dat het `set-method`-beleid gebruikt, ziet u een voor beeld van het verzenden van een bericht naar een toegestane vertragings ruimte als de HTTP-antwoord code groter is dan of gelijk is aan 500. Zie [externe services van de Azure API Management-service gebruiken](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/)voor meer informatie over dit voor beeld.

```xml
<choose>
    <when condition="@(context.Response.StatusCode >= 500)">
      <send-one-way-request mode="new">
        <set-url>https://hooks.slack.com/services/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</set-url>
        <set-method>POST</set-method>
        <set-body>@{
                return new JObject(
                        new JProperty("username","APIM Alert"),
                        new JProperty("icon_emoji", ":ghost:"),
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",
                                                context.Request.Method,
                                                context.Request.Url.Path + context.Request.Url.QueryString,
                                                context.Request.Url.Host,
                                                context.Response.StatusCode,
                                                context.Response.StatusReason,
                                                context.User.Email
                                                ))
                        ).ToString();
            }</set-body>
      </send-one-way-request>
    </when>
</choose>

```

### <a name="elements"></a>Elementen

| Element    | Beschrijving                                                       | Vereist |
| ---------- | ----------------------------------------------------------------- | -------- |
| set-methode | Hoofd element. De waarde van het element specificeert de HTTP-methode. | Ja      |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, op fouten

-   **Beleids bereik:** alle bereiken

## <a name="SetStatus"></a>Status code instellen

Met het `set-status`-beleid wordt de HTTP-status code ingesteld op de opgegeven waarde.

### <a name="policy-statement"></a>Beleids verklaring

```xml
<set-status code="" reason=""/>

```

### <a name="example"></a>Voorbeeld

In dit voor beeld ziet u hoe u een respons van 401 retourneert als het autorisatie token ongeldig is. Zie [externe services gebruiken vanuit de Azure API Management-service](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/) voor meer informatie.

```xml
<choose>
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">
    <return-response response-variable-name="existing response variable">
      <set-status code="401" reason="Unauthorized" />
      <set-header name="WWW-Authenticate" exists-action="override">
        <value>Bearer error="invalid_token"</value>
      </set-header>
    </return-response>
  </when>
</choose>

```

### <a name="elements"></a>Elementen

| Element    | Beschrijving   | Vereist |
| ---------- | ------------- | -------- |
| set-status | Hoofd element. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk       | Beschrijving                                                | Vereist | Standaard |
| --------------- | ---------------------------------------------------------- | -------- | ------- |
| code = "geheel getal"  | De HTTP-status code die moet worden geretourneerd.                            | Ja      | N.v.t.     |
| reden = "teken reeks" | Een beschrijving van de reden voor het retour neren van de status code. | Ja      | N.v.t.     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** uitgaand, back-end, op fout
-   **Beleids bereik:** alle bereiken

## <a name="set-variable"></a>Variabele instellen

Het `set-variable` beleid declareert een [context](api-management-policy-expressions.md#ContextVariables) variabele en wijst deze toe aan een waarde die is opgegeven via een [expressie](api-management-policy-expressions.md) of een letterlijke teken reeks. Als de expressie een letterlijke waarde bevat, wordt deze geconverteerd naar een teken reeks en wordt het type van de waarden `System.String`.

### <a name="set-variablePolicyStatement"></a>Beleids verklaring

```xml
<set-variable name="variable name" value="Expression | String literal" />
```

### <a name="set-variableExample"></a>Hierbij

In het volgende voor beeld ziet u een ingesteld variabelen beleid in de sectie binnenkomend. Met dit set-variabele beleid maakt u een `isMobile` Boole- [context](api-management-policy-expressions.md#ContextVariables) variabele die is ingesteld op True als de `User-Agent` aanvraag header de tekst `iPad` of `iPhone`bevat.

```xml
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />
```

### <a name="elements"></a>Elementen

| Element      | Beschrijving   | Vereist |
| ------------ | ------------- | -------- |
| set-variabele | Hoofd element. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk | Beschrijving                                                              | Vereist |
| --------- | ------------------------------------------------------------------------ | -------- |
| naam      | De naam van de variabele.                                                | Ja      |
| waarde     | De waarde van de variabele. Dit kan een expressie of een letterlijke waarde zijn. | Ja      |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout
-   **Beleids bereik:** alle bereiken

### <a name="set-variableAllowedTypes"></a>Toegestane typen

Expressies die in het `set-variable`-beleid worden gebruikt, moeten een van de volgende basis typen retour neren.

-   System. Boolean
-   Systeem. SByte
-   System. byte
-   System. UInt16
-   System. UInt32
-   System. UInt64
-   System. Int16
-   System. Int32
-   System. Int64
-   Systeem. decimaal
-   System. single
-   Systeem. Double
-   System. GUID
-   System. String
-   System. char
-   System. DateTime
-   System. time span
-   System. byte?
-   System. UInt16?
-   System. UInt32?
-   System. UInt64?
-   System. Int16?
-   System. Int32?
-   Systeem. Int64?
-   Systeem. decimaal?
-   System. single?
-   Systeem. Double?
-   System. GUID?
-   System. String?
-   System. char?
-   System. DateTime?

## <a name="Trace"></a>Tracerings

Het `trace`-beleid voegt een aangepaste tracering toe aan de API-Inspector-uitvoer, Application Insights-omelementen en/of diagnostische Logboeken. 

* Het beleid voegt een aangepaste tracering toe aan de [API-Inspector](https://azure.microsoft.com/documentation/articles/api-management-howto-api-inspector/) -uitvoer wanneer tracering wordt geactiveerd, d.w.z. `Ocp-Apim-Trace` aanvraag header is aanwezig en ingesteld op True en de `Ocp-Apim-Subscription-Key` aanvraag header is aanwezig en bevat een geldige sleutel die tracering mogelijk maakt. 
* Het beleid maakt een telemetrie [traceren](https://docs.microsoft.com/azure/azure-monitor/app/data-model-trace-telemetry) in Application Insights, wanneer [Application Insights integratie](https://docs.microsoft.com/azure/api-management/api-management-howto-app-insights) is ingeschakeld en het `severity` niveau dat is opgegeven in het beleid, hoger is dan het `verbosity` niveau dat is opgegeven in de diagnostische instelling. 
* Het beleid voegt een eigenschap in de logboek vermelding toe wanneer [Diagnostische logboeken](https://docs.microsoft.com/azure/api-management/api-management-howto-use-azure-monitor#diagnostic-logs) zijn ingeschakeld en het Ernst niveau dat is opgegeven in het beleid, hoger is dan het niveau van de uitgebreidheid dat is opgegeven in de diagnostische instelling.  


### <a name="policy-statement"></a>Beleids verklaring

```xml

<trace source="arbitrary string literal" severity="verbose|information|error">
    <message>String literal or expressions</message>
    <metadata name="string literal or expressions" value="string literal or expressions"/>
</trace>

```

### <a name="traceExample"></a>Hierbij

```xml
<trace source="PetStore API" severity="verbose">
    <message>@((string)context.Variables["clientConnectionID"])</message>
    <metadata name="Operation Name" value="New-Order"/>
</trace>
```

### <a name="elements"></a>Elementen

| Element | Beschrijving   | Vereist |
| ------- | ------------- | -------- |
| tracerings   | Hoofd element. | Ja      |
| message | Een teken reeks of expressie die moet worden vastgelegd. | Ja |
| metagegevens | Hiermee voegt u een aangepaste eigenschap toe aan de Application Insights [Trace](https://docs.microsoft.com/azure/azure-monitor/app/data-model-trace-telemetry) telemetrie. | Nee |

### <a name="attributes"></a>Kenmerken

| Kenmerk | Beschrijving                                                                             | Vereist | Standaard |
| --------- | --------------------------------------------------------------------------------------- | -------- | ------- |
| source    | Letterlijke teken reeks die betekenisvol is voor de traceer viewer en het opgeven van de bron van het bericht. | Ja      | N.v.t.     |
| Ernst    | Hiermee geeft u het Ernst niveau van de tracering. Toegestane waarden zijn `verbose`, `information`, `error` (van laagste naar hoogste). | Nee      | Uitgebreide     |
| naam    | De naam van de eigenschap. | Ja      | N.v.t.     |
| waarde    | Waarde van de eigenschap. | Ja      | N.v.t.     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) .

-   **Beleids secties:** inkomend, uitgaand, back-end, op fout

-   **Beleids bereik:** alle bereiken

## <a name="Wait"></a>Bewerking

Het `wait` beleid voert de direct onderliggende beleids regels parallel uit en wacht totdat alle of een van de direct onderliggende beleids regels is voltooid voordat deze is voltooid. Het wacht beleid kan net zo lang zijn als het directe onderliggende beleid [verzend aanvraag](api-management-advanced-policies.md#SendRequest), [waarde ophalen uit cache](api-management-caching-policies.md#GetFromCacheByKey)en beleid voor [controle stromen](api-management-advanced-policies.md#choose) .

### <a name="policy-statement"></a>Beleids verklaring

```xml
<wait for="all|any">
  <!--Wait policy can contain send-request, cache-lookup-value,
        and choose policies as child elements -->
</wait>

```

### <a name="example"></a>Voorbeeld

In het volgende voor beeld zijn er twee `choose`-beleid als direct onderliggend beleid van het beleid `wait`. Elk van deze `choose`-beleids regels worden parallel uitgevoerd. Elk `choose`-beleid probeert een in cache opgeslagen waarde op te halen. Als er een cache ontbreekt, wordt een back-end-service aangeroepen om de waarde op te geven. In dit voor beeld wordt het `wait`-beleid pas voltooid wanneer alle direct onderliggende beleids regels zijn voltooid, omdat het kenmerk `for` is ingesteld op `all`. In dit voor beeld worden de context variabelen (`execute-branch-one`, `value-one`, `execute-branch-two`en `value-two`) gedeclareerd buiten het bereik van dit voorbeeld beleid.

```xml
<wait for="all">
  <choose>
    <when condition="@((bool)context.Variables["execute-branch-one="])">
      <cache-lookup-value key="key-one" variable-name="value-one" />
      <choose>
        <when condition="@(!context.Variables.ContainsKey("value-one="))">
          <send-request mode="new" response-variable-name="value-one">
            <set-url>https://backend-one</set-url>
            <set-method>GET</set-method>
          </send-request>
        </when>
      </choose>
    </when>
  </choose>
  <choose>
    <when condition="@((bool)context.Variables["execute-branch-two="])">
      <cache-lookup-value key="key-two" variable-name="value-two" />
      <choose>
        <when condition="@(!context.Variables.ContainsKey("value-two="))">
          <send-request mode="new" response-variable-name="value-two">
            <set-url>https://backend-two</set-url>
            <set-method>GET</set-method>
          </send-request>
        </when>
      </choose>
    </when>
  </choose>
</wait>

```

### <a name="elements"></a>Elementen

| Element | Beschrijving                                                                                                   | Vereist |
| ------- | ------------------------------------------------------------------------------------------------------------- | -------- |
| Bewerking    | Hoofd element. Mag alleen onderliggende elementen `send-request`, `cache-lookup-value`en `choose`-beleid bevatten. | Ja      |

### <a name="attributes"></a>Kenmerken

| Kenmerk | Beschrijving                                                                                                                                                                                                                                                                                                                                                                                                            | Vereist | Standaard |
| --------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------- | ------- |
| zo       | Hiermee wordt bepaald of het `wait` beleid wacht totdat alle direct onderliggende beleids regels zijn voltooid of slechts één. Toegestane waarden zijn:<br /><br /> - `all`: wacht tot alle direct onderliggende beleids regels zijn voltooid<br />-wille keurig-wacht tot het direct onderliggende beleid is voltooid. Zodra het eerste direct onderliggende beleid is voltooid, wordt het `wait`-beleid voltooid en wordt de uitvoering van elk ander direct onderliggend beleid beëindigd. | Nee       | all     |

### <a name="usage"></a>Gebruik

Dit beleid kan worden gebruikt in de volgende beleids [secties](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) en [bereiken](https://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes).

-   **Beleids secties:** inkomend, uitgaand, back-end
-   **Beleids bereik:** alle bereiken

## <a name="next-steps"></a>Volgende stappen

Zie voor meer informatie over het gebruik van beleid:

-   [Beleid in API Management](api-management-howto-policies.md)
-   [Beleidsexpressies](api-management-policy-expressions.md)
-   [Beleids verwijzing](api-management-policy-reference.md) voor een volledige lijst met beleids instructies en hun instellingen
-   [Voor beelden van beleid](policy-samples.md)
