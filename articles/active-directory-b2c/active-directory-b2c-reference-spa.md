---
title: Eenmalige aanmelding met impliciete stroom
titleSuffix: Azure AD B2C
description: Meer informatie over het toevoegen van een eenmalige aanmelding met de OAuth 2,0 impliciete stroom met Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 07/19/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: 9a7d6a0a4e341158b37de73a74390d87a135d65f
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74947978"
---
# <a name="single-page-sign-in-using-the-oauth-20-implicit-flow-in-azure-active-directory-b2c"></a>Aanmelden met één pagina met de impliciete OAuth 2,0-stroom in Azure Active Directory B2C

Veel moderne toepassingen hebben een front-end app met één pagina die voornamelijk in Java script wordt geschreven. De app wordt vaak geschreven met behulp van een framework zoals reageren, hoek of Vue. js. Apps met één pagina en andere Java script-apps die primair worden uitgevoerd in een browser, hebben een aantal extra uitdagingen voor verificatie:

- De beveiligings kenmerken van deze apps verschillen van traditionele webtoepassingen op de server.
- Veel autorisatie servers en id-providers ondersteunen CORS-aanvragen (cross-Origin Resource Sharing) niet.
- Browser-omleidingen in volledige pagina's weg van de app kunnen invasief zijn voor de gebruikers ervaring.

Ter ondersteuning van deze toepassingen gebruikt Azure Active Directory B2C (Azure AD B2C) de impliciete OAuth 2,0-stroom. De OAuth 2,0-autorisatie impliciete subsidie stroom wordt beschreven in [sectie 4,2 van de OAuth 2,0-specificatie](https://tools.ietf.org/html/rfc6749). In impliciete stroom ontvangt de app tokens rechtstreeks van het Azure Active Directory (Azure AD)-toestemming-eind punt, zonder server-naar-server-uitwisseling. Alle verificatie logica en sessie verwerking worden volledig in de Java script-client uitgevoerd met een omleiding van pagina's of een pop-upvenster.

Azure AD B2C breidt de standaard OAuth 2,0 impliciete stroom uit naar meer dan eenvoudige verificatie en autorisatie. Azure AD B2C introduceert de [para meter beleid](active-directory-b2c-reference-policies.md). Met de para meter beleid kunt u OAuth 2,0 gebruiken om beleids regels toe te voegen aan uw app, zoals aanmelden, aanmelden en gebruikers stromen voor Profiel beheer. In het voor beeld van HTTP-aanvragen in dit artikel wordt **{Tenant}. onmicrosoft. com** als voor beeld gebruikt. Vervang `{tenant}` door de naam van uw Tenant als u er een hebt en ook een gebruikers stroom hebt gemaakt.

De impliciete aanmeldings stroom ziet er ongeveer uit zoals in de volgende afbeelding. Elke stap wordt verderop in het artikel uitvoerig beschreven.

![Swimlane-stijl diagram met de impliciete stroom OpenID Connect Connect](../media/active-directory-v2-flows/convergence_scenarios_implicit.png)

## <a name="send-authentication-requests"></a>Verificatie aanvragen verzenden

Wanneer uw webtoepassing de gebruiker moet verifiëren en een gebruikers stroom kan uitvoeren, kan deze de gebruiker naar het `/authorize`-eind punt sturen. De gebruiker actie onderneemt afhankelijk van de gebruikers stroom.

In deze aanvraag geeft de client aan welke machtigingen moeten worden verkregen van de gebruiker in de para meter `scope` en de gebruikers stroom om uit te voeren. Als u wilt weten hoe de aanvraag werkt, kunt u de aanvraag in een browser plakken en deze uitvoeren. Vervang `{tenant}` met de naam van uw Azure AD B2C-tenant. Vervang `90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6` door de App-ID van de toepassing die u eerder hebt geregistreerd in uw Tenant. Vervang `{policy}` door de naam van een beleid dat u hebt gemaakt in uw Tenant, bijvoorbeeld `b2c_1_sign_in`.

```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=id_token+token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&response_mode=fragment
&scope=openid%20offline_access
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
```

| Parameter | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
|bouw| Ja | De naam van uw Azure AD B2C-Tenant|
|verslaggev| Ja| De gebruikers stroom die moet worden uitgevoerd. Geef de naam op van een gebruikers stroom die u hebt gemaakt in uw Azure AD B2C-Tenant. Bijvoorbeeld: `b2c_1_sign_in`, `b2c_1_sign_up`of `b2c_1_edit_profile`. |
| client_id | Ja | De toepassings-ID die de [Azure Portal](https://portal.azure.com/) toegewezen aan uw toepassing. |
| response_type | Ja | Moet `id_token` voor OpenID Connect Connect-aanmelding bevatten. Het kan ook het antwoord type `token`bevatten. Als u `token`gebruikt, kan uw app direct een toegangs token ontvangen van het toestemming-eind punt, zonder dat er een tweede aanvraag wordt gedaan voor het autoriseren van het eind punt.  Als u het `token`-antwoord type gebruikt, moet de para meter `scope` een bereik bevatten dat aangeeft voor welke resource het token moet worden uitgegeven. |
| redirect_uri | Nee | De omleidings-URI van uw app, waar verificatie reacties kunnen worden verzonden en ontvangen door uw app. De waarde moet exact overeenkomen met een van de omleidings-Uri's die u in de portal hebt geregistreerd, behalve dat de URL moet worden gecodeerd. |
| response_mode | Nee | Hiermee geeft u de methode op die moet worden gebruikt om het resulterende token terug naar uw app te verzenden.  Gebruik `fragment`voor impliciete stromen. |
| scope | Ja | Een lijst met door spaties gescheiden bereiken. Een enkele Scope waarde geeft aan dat er voor Azure AD beide machtigingen worden aangevraagd. Het `openid` bereik geeft een machtiging aan voor het aanmelden van de gebruiker en het ophalen van gegevens over de gebruiker in de vorm van ID-tokens. Het `offline_access` bereik is optioneel voor web-apps. Dit geeft aan dat uw app een vernieuwings token nodig heeft voor lange levens toegang tot bronnen. |
| state | Nee | Een waarde die in de aanvraag is opgenomen en die ook wordt geretourneerd in het token antwoord. Dit kan een teken reeks zijn van alle inhoud die u wilt gebruiken. Normaal gesp roken wordt een wille keurig gegenereerde, unieke waarde gebruikt om vervalsing van aanvragen op meerdere sites te voor komen. De status wordt ook gebruikt voor het coderen van informatie over de status van de gebruiker in de app voordat de verificatie aanvraag is uitgevoerd, zoals op de pagina waarop deze zich bevonden. |
| nonce | Ja | Een waarde die is opgenomen in de aanvraag (gegenereerd door de app) die is opgenomen in de resulterende ID-token als claim. De app kan vervolgens deze waarde verifiëren om token replay-aanvallen te verhelpen. Normaal gesp roken is de waarde een wille keurige, unieke teken reeks die kan worden gebruikt om de oorsprong van de aanvraag te identificeren. |
| verschijnt | Nee | Het type gebruikers interactie dat is vereist. Op dit moment is de enige geldige waarde `login`. Met deze para meter wordt de gebruiker gedwongen om hun referenties in te voeren voor deze aanvraag. Eenmalige aanmelding treedt niet in werking. |

Op dit moment wordt de gebruiker gevraagd om de werk stroom van het beleid te volt ooien. De gebruiker moet mogelijk hun gebruikers naam en wacht woord invoeren, zich aanmelden met een sociale identiteit, zich aanmelden voor de Directory of een ander aantal stappen. Gebruikers acties zijn afhankelijk van de manier waarop de gebruikers stroom is gedefinieerd.

Nadat de gebruiker de gebruikers stroom heeft voltooid, retourneert Azure AD een reactie op uw app op basis van de waarde die u hebt gebruikt voor `redirect_uri`. Hierbij wordt gebruikgemaakt van de methode die is opgegeven in de para meter `response_mode`. Het antwoord is precies hetzelfde voor elk van de gebruikers actie scenario's, onafhankelijk van de uitgevoerde gebruikers stroom.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie die gebruikmaakt van `response_mode=fragment` en `response_type=id_token+token` ziet er als volgt uit, met regel einden voor de Lees baarheid:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&token_type=Bearer
&expires_in=3599
&scope="90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6 offline_access",
&id_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
```

| Parameter | Beschrijving |
| --------- | ----------- |
| access_token | Het toegangs token dat door de app is aangevraagd. |
| token_type | De waarde van het token type. Het enige type dat door Azure AD wordt ondersteund, is Bearer. |
| expires_in | De tijds duur dat het toegangs token geldig is (in seconden). |
| scope | De bereiken waarvoor het token geldig is. U kunt scopes ook gebruiken om tokens in de cache op te slaan voor later gebruik. |
| id_token | Het ID-token dat de app heeft aangevraagd. U kunt het ID-token gebruiken om de identiteit van de gebruiker te verifiëren en een sessie met de gebruiker te starten. Zie de [Azure AD B2C-token verwijzing](active-directory-b2c-reference-tokens.md)voor meer informatie over id-tokens en de inhoud ervan. |
| state | Als de aanvraag een `state`-para meter bevat, moet dezelfde waarde in het antwoord worden weer gegeven. De app moet controleren of de `state` waarden in de aanvraag en het antwoord identiek zijn. |

### <a name="error-response"></a>Fout bericht
Fout reacties kunnen ook worden verzonden naar de omleidings-URI, zodat de app deze op de juiste wijze kan afhandelen:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
error=access_denied
&error_description=the+user+canceled+the+authentication
&state=arbitrary_data_you_can_receive_in_the_response
```

| Parameter | Beschrijving |
| --------- | ----------- |
| error | Een code die wordt gebruikt voor het classificeren van typen fouten die optreden. |
| error_description | Een specifiek fout bericht die u kan helpen bij het identificeren van de hoofd oorzaak van een verificatie fout. |
| state | Als de aanvraag een `state`-para meter bevat, moet dezelfde waarde in het antwoord worden weer gegeven. De app moet controleren of de `state` waarden in de aanvraag en het antwoord identiek zijn.|

## <a name="validate-the-id-token"></a>Het ID-token valideren

Het ontvangen van een ID-token is niet voldoende om de gebruiker te verifiëren. Valideer de hand tekening van het ID-token en controleer de claims in het token volgens de vereisten van uw app. Azure AD B2C maakt gebruik van [JSON Web tokens (JWTs)](https://self-issued.info/docs/draft-ietf-oauth-json-web-token.html) en open bare-sleutel cryptografie om tokens te ondertekenen en te controleren of ze geldig zijn.

Er zijn veel open-source-bibliotheken beschikbaar voor het valideren van JWTs, afhankelijk van de taal die u wilt gebruiken. Overweeg om beschik bare open-source bibliotheken te verkennen in plaats van uw eigen validatie logica te implementeren. U kunt de informatie in dit artikel gebruiken om u te leren hoe u deze bibliotheken correct kunt gebruiken.

Azure AD B2C heeft een OpenID Connect voor het verbinden van meta gegevens. Een app kan het eind punt gebruiken voor het ophalen van informatie over Azure AD B2C tijdens runtime. Deze informatie omvat eind punten, token inhoud en sleutels voor token-ondertekening. Er is een JSON-meta gegevens document voor elke gebruikers stroom in uw Azure AD B2C-Tenant. Het meta gegevens document voor de b2c_1_sign_in gebruikers stroom in de fabrikamb2c.onmicrosoft.com-Tenant bevindt zich bijvoorbeeld op de volgende locatie:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/v2.0/.well-known/openid-configuration
```

Een van de eigenschappen van dit configuratie document is het `jwks_uri`. De waarde voor dezelfde gebruikers stroom is:

```HTTP
https://fabrikamb2c.b2clogin.com/fabrikamb2c.onmicrosoft.com/b2c_1_sign_in/discovery/v2.0/keys
```

Als u wilt bepalen welke gebruikers stroom is gebruikt voor het ondertekenen van een ID-token (en de locatie van de meta gegevens op te halen), hebt u twee opties. Eerst wordt de naam van de gebruikers stroom opgenomen in de `acr` claim in `id_token`. Voor informatie over het parseren van de claims van een ID-token raadpleegt u de [Azure AD B2C-token verwijzing](active-directory-b2c-reference-tokens.md). De andere optie is het coderen van de gebruikers stroom in de waarde van de para meter `state` wanneer u de aanvraag verzendt. Decodeer vervolgens de para meter `state` om te bepalen welke gebruikers stroom is gebruikt. Beide methoden zijn geldig.

Nadat u het meta gegevens document hebt opgehaald uit het eind punt OpenID Connect Connect meta data, kunt u de open bare sleutels van RSA-256 (op dit eind punt) gebruiken om de hand tekening van het ID-token te valideren. Er kunnen op elk gewenst moment meerdere sleutels op dit eind punt worden weer gegeven, die allemaal zijn geïdentificeerd door een `kid`. De header van `id_token` bevat ook een `kid` claim. Hiermee wordt aangegeven welke van deze sleutels is gebruikt voor het ondertekenen van het ID-token. Zie de [Azure AD B2C-token referentie](active-directory-b2c-reference-tokens.md)voor meer informatie over het [valideren van tokens](active-directory-b2c-reference-tokens.md).
<!--TODO: Improve the information on this-->

Nadat u de hand tekening van het ID-token hebt gevalideerd, moeten er verschillende claims worden geverifieerd. Bijvoorbeeld:

* Valideer de `nonce` claim om token replay-aanvallen te voor komen. De waarde moet zijn wat u hebt opgegeven in het aanmeldings verzoek.
* Valideer de `aud` claim om ervoor te zorgen dat het ID-token is uitgegeven voor uw app. De waarde moet de toepassings-ID van uw app zijn.
* Valideer de `iat`-en `exp` claims om ervoor te zorgen dat het ID-token niet is verlopen.

Enkele meer validaties die u moet uitvoeren, worden gedetailleerd beschreven in de [OpenID Connect Connect core-specificatie](https://openid.net/specs/openid-connect-core-1_0.html). Het is ook mogelijk dat u aanvullende claims wilt valideren, afhankelijk van uw scenario. Enkele veelvoorkomende validaties zijn:

* Controleren of de gebruiker of organisatie zich heeft geregistreerd voor de app.
* Controleren of de gebruiker de juiste autorisatie en bevoegdheden heeft.
* Ervoor zorgen dat er een zekere mate van verificatie is opgetreden, bijvoorbeeld door Azure Multi-Factor Authentication te gebruiken.

Zie de [Azure AD B2C-token verwijzing](active-directory-b2c-reference-tokens.md)voor meer informatie over de claims in een id-token.

Nadat u het ID-token hebt gevalideerd, kunt u met de gebruiker beginnen met een sessie. Gebruik in uw app de claims in het ID-token om informatie over de gebruiker te verkrijgen. Deze informatie kan worden gebruikt voor weer gave, records, autorisatie, enzovoort.

## <a name="get-access-tokens"></a>Toegangs tokens ophalen
Als het enige wat uw web-apps moeten doen, gebruikers stromen uitvoeren, kunt u de volgende gedeelten overs Laan. De informatie in de volgende secties is alleen van toepassing op Web-apps die geverifieerde aanroepen naar een web-API moeten maken en die worden beveiligd door Azure AD B2C.

Nu u de gebruiker hebt ondertekend in een app met één pagina, kunt u toegangs tokens verkrijgen voor het aanroepen van web-Api's die zijn beveiligd met Azure AD. Zelfs als u al een token hebt ontvangen met behulp van het antwoord type `token`, kunt u deze methode gebruiken om tokens te verkrijgen voor aanvullende bronnen zonder de gebruiker opnieuw te omleiden om zich opnieuw aan te melden.

In een typische web-app-stroom maakt u een aanvraag voor het `/token`-eind punt. Het eind punt biedt echter geen ondersteuning voor CORS-aanvragen, dus het maken van AJAX-aanroepen om een vernieuwings token op te halen, is geen optie. In plaats daarvan kunt u de impliciete stroom in een verborgen HTML IFRAME-element gebruiken om nieuwe tokens voor andere web-Api's op te halen. Hier volgt een voor beeld met regel einden voor de Lees baarheid:

```HTTP
https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/authorize?
client_id=90c0fe63-bcf2-44d5-8fb7-b8bbc0b29dc6
&response_type=token
&redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
&response_mode=fragment
&state=arbitrary_data_you_can_receive_in_the_response
&nonce=12345
&prompt=none
```

| Parameter | Vereist? | Beschrijving |
| --- | --- | --- |
|bouw| Verplicht | De naam van uw Azure AD B2C-Tenant|
verslaggev| Verplicht| De gebruikers stroom die moet worden uitgevoerd. Geef de naam op van een gebruikers stroom die u hebt gemaakt in uw Azure AD B2C-Tenant. Bijvoorbeeld: `b2c_1_sign_in`, `b2c_1_sign_up`of `b2c_1_edit_profile`. |
| client_id |Verplicht |De toepassings-ID die is toegewezen aan uw app in de [Azure Portal](https://portal.azure.com). |
| response_type |Verplicht |Moet `id_token` voor OpenID Connect Connect-aanmelding bevatten.  Het kan ook het antwoord type `token`bevatten. Als u `token` hier gebruikt, kan uw app direct een toegangs token ontvangen van het eind punt voor toestemming, zonder dat u een tweede aanvraag naar het toestemming eind punt hoeft te doen. Als u het `token`-antwoord type gebruikt, moet de para meter `scope` een bereik bevatten dat aangeeft voor welke resource het token moet worden uitgegeven. |
| redirect_uri |Aanbevolen |De omleidings-URI van uw app, waar verificatie reacties kunnen worden verzonden en ontvangen door uw app. Het moet exact overeenkomen met een van de omleidings-Uri's die u in de portal hebt geregistreerd, met uitzonde ring van de URL-code ring. |
| scope |Verplicht |Een lijst met door spaties gescheiden bereiken.  Voor het ophalen van tokens, neemt u alle scopes op die u nodig hebt voor de gewenste resource. |
| response_mode |Aanbevolen |Hiermee geeft u de methode op die wordt gebruikt om het resulterende token terug naar uw app te verzenden. Gebruik `fragment`voor impliciete stroom. Er kunnen twee andere modi worden opgegeven, `query` en `form_post`, maar werken niet in de impliciete stroom. |
| state |Aanbevolen |Een waarde die is opgenomen in de aanvraag die wordt geretourneerd in de token reactie.  Dit kan een teken reeks zijn van alle inhoud die u wilt gebruiken.  Normaal gesp roken wordt een wille keurig gegenereerde, unieke waarde gebruikt om vervalsing van aanvragen op meerdere sites te voor komen.  De status wordt ook gebruikt om informatie over de status van de gebruiker in de app te coderen voordat de verificatie aanvraag is opgetreden. Bijvoorbeeld de pagina of weer gave waarin de gebruiker zich bevond. |
| nonce |Verplicht |Een waarde die is opgenomen in de aanvraag, gegenereerd door de app, die is opgenomen in de resulterende ID-token als claim.  De app kan vervolgens deze waarde verifiëren om token replay-aanvallen te verhelpen. Normaal gesp roken is de waarde een wille keurige, unieke teken reeks waarmee de oorsprong van de aanvraag wordt geïdentificeerd. |
| verschijnt |Verplicht |Als u tokens in een verborgen iframe wilt vernieuwen en ophalen, gebruikt u `prompt=none` om ervoor te zorgen dat het iframe niet vastloopt op de aanmeldings pagina en direct wordt geretourneerd. |
| login_hint |Verplicht |Als u tokens wilt vernieuwen en ophalen in een verborgen iframe, neemt u de gebruikers naam van de gebruiker op in deze hint om onderscheid te maken tussen meerdere sessies die de gebruiker op een bepaald moment kan hebben. U kunt de gebruikers naam van een eerdere aanmelding extra heren met behulp van de `preferred_username` claim (het `profile` bereik is vereist om de `preferred_username` claim te ontvangen). |
| domain_hint |Verplicht |Deze waarde kan `consumers` of `organizations` zijn.  Voor het vernieuwen en ophalen van tokens in een verborgen iframe, neemt u de `domain_hint` waarde op in de aanvraag.  Extraheer de `tid` claim van het ID-token van een eerdere aanmelding om te bepalen welke waarde moet worden gebruikt (het `profile` bereik is vereist om de `tid` claim te kunnen ontvangen). Als de `tid` claim waarde `9188040d-6c67-4c5b-b112-36a304b66dad`is, gebruikt u `domain_hint=consumers`.  Gebruik anders `domain_hint=organizations`. |

Door de para meter `prompt=none` in te stellen, slaagt of mislukt deze aanvraag onmiddellijk en keert u terug naar uw toepassing.  Een geslaagde reactie wordt verzonden naar uw app op de aangegeven omleidings-URI, met behulp van de methode die is opgegeven in de para meter `response_mode`.

### <a name="successful-response"></a>Geslaagde reactie
Een geslaagde reactie met behulp van `response_mode=fragment` ziet er als volgt uit:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
access_token=eyJ0eXAiOiJKV1QiLCJhbGciOiJSUzI1NiIsIng1dCI6Ik5HVEZ2ZEstZnl0aEV1Q...
&state=arbitrary_data_you_sent_earlier
&token_type=Bearer
&expires_in=3599
&scope=https%3A%2F%2Fapi.contoso.com%2Ftasks.read
```

| Parameter | Beschrijving |
| --- | --- |
| access_token |Het token dat de app heeft aangevraagd. |
| token_type |Het token type is altijd Bearer. |
| state |Als de aanvraag een `state`-para meter bevat, moet dezelfde waarde in het antwoord worden weer gegeven. De app moet controleren of de `state` waarden in de aanvraag en het antwoord identiek zijn. |
| expires_in |Hoe lang het toegangs token geldig is (in seconden). |
| scope |De bereiken waarvoor het toegangs token geldig is. |

### <a name="error-response"></a>Fout bericht
Fout reacties kunnen ook worden verzonden naar de omleidings-URI, zodat de app deze op de juiste wijze kan afhandelen.  In het geval van `prompt=none`ziet een verwachte fout er als volgt uit:

```HTTP
GET https://aadb2cplayground.azurewebsites.net/#
error=user_authentication_required
&error_description=the+request+could+not+be+completed+silently
```

| Parameter | Beschrijving |
| --- | --- |
| error |Een teken reeks voor fout code die kan worden gebruikt voor het classificeren van typen fouten die optreden. U kunt de teken reeks ook gebruiken om te reageren op fouten. |
| error_description |Een specifiek fout bericht die u kan helpen bij het identificeren van de hoofd oorzaak van een verificatie fout. |

Als u deze fout in de iframe-aanvraag ontvangt, moet de gebruiker zich opnieuw aanmelden om een nieuw token op te halen.

## <a name="refresh-tokens"></a>Tokens vernieuwen
ID-tokens en toegangs tokens verlopen beide na een korte periode. Uw app moet worden voor bereid om deze tokens regel matig te vernieuwen.  Als u een van beide typen tokens wilt vernieuwen, voert u dezelfde verborgen iframe-aanvraag uit die we in een eerder voor beeld hebben gebruikt, met behulp van de para meter `prompt=none` om de stappen van Azure AD te beheren.  Als u een nieuwe `id_token` waarde wilt ontvangen, moet u `response_type=id_token` en `scope=openid`gebruiken en een `nonce`-para meter.

## <a name="send-a-sign-out-request"></a>Een afmeldings aanvraag verzenden
Wanneer u de gebruiker van de app wilt ondertekenen, moet u de gebruiker omleiden naar Azure AD om u af te melden. Als u de gebruiker niet omleidt, kunnen ze mogelijk opnieuw worden geverifieerd bij uw app zonder dat ze hun referenties opnieuw in te voeren, omdat ze een geldige sessie voor eenmalige aanmelding met Azure AD hebben.

U kunt de gebruiker gewoon omleiden naar de `end_session_endpoint` die wordt vermeld in hetzelfde OpenID Connect Connect meta data-document dat wordt beschreven in het [ID-token valideren](#validate-the-id-token). Bijvoorbeeld:

```HTTP
GET https://{tenant}.b2clogin.com/{tenant}.onmicrosoft.com/{policy}/oauth2/v2.0/logout?post_logout_redirect_uri=https%3A%2F%2Faadb2cplayground.azurewebsites.net%2F
```

| Parameter | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| bouw | Ja | De naam van uw Azure AD B2C-Tenant |
| verslaggev | Ja | De gebruikers stroom die u wilt gebruiken voor het ondertekenen van de gebruiker uit uw toepassing. |
| post_logout_redirect_uri | Nee | De URL waarnaar de gebruiker wordt omgeleid na een geslaagde afmelding. Als deze niet is opgenomen, wordt in Azure AD B2C de gebruiker een Gene riek bericht weer gegeven. |
| state | Nee | Als de aanvraag een `state`-para meter bevat, moet dezelfde waarde in het antwoord worden weer gegeven. De toepassing moet controleren of de `state` waarden in de aanvraag en het antwoord identiek zijn. |


> [!NOTE]
> Door de gebruiker naar het `end_session_endpoint` te leiden, wordt de status voor eenmalige aanmelding van de gebruiker met Azure AD B2C gewist. De gebruiker wordt echter niet van de gebruikers sessie van de sociale id van de gebruiker ondertekend. Als de gebruiker tijdens een volgende aanmelding dezelfde id-provider selecteert, wordt de gebruiker opnieuw geverifieerd zonder de referenties in te voeren. Als een gebruiker zich wil afmelden bij uw Azure AD B2C-toepassing, betekent dit niet noodzakelijkerwijs dat ze zich bijvoorbeeld volledig willen afmelden bij hun Facebook-account. Voor lokale accounts wordt de sessie van de gebruiker echter correct beëindigd.
>

## <a name="next-steps"></a>Volgende stappen

### <a name="code-sample-hellojs-with-azure-ad-b2c"></a>Code voorbeeld: Hello. js met Azure AD B2C

[Toepassing met één pagina die is gebouwd op Hello. js met Azure AD B2C][github-hello-js-example] (github)

Dit voor beeld op GitHub is bedoeld om u te helpen aan de slag te gaan met Azure AD B2C in een eenvoudige webtoepassing die is gebouwd op [Hello. js][github-hello-js] en met behulp van pop-up-achtige verificatie.

<!-- Links - EXTERNAL -->
[github-hello-js-example]: https://github.com/Azure-Samples/active-directory-b2c-javascript-hellojs-singlepageapp
[github-hello-js]: https://github.com/MrSwitch/hello.js
