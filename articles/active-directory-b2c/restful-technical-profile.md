---
title: Een onderliggend technisch profiel definiëren in een aangepast beleid
titleSuffix: Azure AD B2C
description: Definieer een onderliggend technisch profiel in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: aa14854807727506f5d697d7871c97e219c096a3
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74950881"
---
# <a name="define-a-restful-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een onderliggend technisch profiel definiëren in een Azure Active Directory B2C aangepast beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) biedt ondersteuning voor uw eigen REST-service. Azure AD B2C verzendt gegevens naar de REST-service in een claim verzameling met invoer en ontvangt gegevens terug in een verzameling uitvoer claims. Met de ondersteunings integratie van de REST-service kunt u het volgende doen:

- **Gebruikers invoer valideren** : hiermee voor komt u dat er ongeldige gegevens worden bewaard in azure AD B2C. Als de waarde van de gebruiker ongeldig is, retourneert de REST-service een fout bericht dat de gebruiker de opdracht geeft een vermelding op te geven. U kunt bijvoorbeeld controleren of het e-mail adres van de gebruiker bestaat in de data base van uw klant.
- **Invoer claims overschrijven** : Hiermee kunt u de waarden in invoer claims opnieuw indelen. Als een gebruiker bijvoorbeeld de eerste naam in alle kleine letters of hoofd letters typt, kunt u de naam alleen met de eerste letter in een letter type Format teren.
- **Verrijkende gebruikers gegevens** : Hiermee kunt u verder integreren met zakelijke line-of-business-toepassingen. Zo kan uw REST-service bijvoorbeeld het e-mail adres van de gebruiker ontvangen, een query uitvoeren op de data base van de klant en het loyaliteits nummer van de gebruiker retour neren om Azure AD B2C. De retour claims kunnen worden opgeslagen, geëvalueerd in de volgende Orchestration-stappen of opgenomen in het toegangs token.
- **Aangepaste bedrijfs logica uitvoeren** : maakt het mogelijk om Push meldingen te verzenden, zakelijke data bases bij te werken, een gebruikers migratie proces uit te voeren, machtigingen te beheren, data bases te controleren en andere acties uit te voeren.

Uw beleid kan invoer claims naar uw REST API verzenden. De REST API kan ook uitvoer claims retour neren die u later in uw beleid kunt gebruiken of een fout bericht kan genereren. U kunt op de volgende manieren de integratie met de REST-services ontwerpen:

- **Validatie technische profiel** : een validatie technisch profiel roept de rest-service aan. Het validatie-technische profiel valideert de door de gebruiker verschafte gegevens voordat de gebruikers traject doorgaat. Met het technische profiel voor validatie wordt een fout bericht weer gegeven op een zelfbevestigende pagina en geretourneerd in uitvoer claims.
- **Claim uitwisseling** : er wordt een aanroep uitgevoerd naar de rest-service via een Orchestration-stap. In dit scenario is er geen gebruikers interface om het fout bericht weer te geven. Als uw REST API een fout retourneert, wordt de gebruiker teruggeleid naar de Relying Party-toepassing met het fout bericht.

## <a name="protocol"></a>Protocol

Het **naam** kenmerk van het **protocol** element moet worden ingesteld op `Proprietary`. Het kenmerk **handler** moet de volledig gekwalificeerde naam van de assembly van de protocolhandler bevatten die wordt gebruikt door Azure AD B2C: `Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null`.

In het volgende voor beeld wordt een onderliggend technisch profiel weer gegeven:

```XML
<TechnicalProfile Id="REST-UserMembershipValidator">
  <DisplayName>Validate user input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  ...
```

## <a name="input-claims"></a>Invoer claims

Het **InputClaims** -element bevat een lijst met claims die moeten worden verzonden naar de rest API. U kunt de naam van uw claim ook toewijzen aan de naam die is gedefinieerd in de REST API. In het volgende voor beeld ziet u de toewijzing tussen uw beleid en de REST API. De naam van **de claim wordt** verzonden naar de rest API als **FirstName**, **terwijl de achternaam wordt** verzonden als **LastName**. De **e-mail** claim is ingesteld als is.

```XML
<InputClaims>
  <InputClaim ClaimTypeReferenceId="email" />
  <InputClaim ClaimTypeReferenceId="givenName" PartnerClaimType="firstName" />
  <InputClaim ClaimTypeReferenceId="surname" PartnerClaimType="lastName" />
</InputClaims>
```

Het **InputClaimsTransformations** -element kan een verzameling **InputClaimsTransformation** -elementen bevatten die worden gebruikt om de invoer claims te wijzigen of nieuwe te genereren voordat deze naar de rest API worden verzonden.

## <a name="output-claims"></a>Uitvoer claims

Het **OutputClaims** -element bevat een lijst met claims die zijn geretourneerd door de rest API. Mogelijk moet u de naam van de claim die in uw beleid is gedefinieerd, toewijzen aan de naam die is gedefinieerd in de REST API. U kunt ook claims toevoegen die niet worden geretourneerd door de REST API ID-provider, op voor waarde dat u het kenmerk `DefaultValue` hebt ingesteld.

Het **OutputClaimsTransformations** -element kan een verzameling **OutputClaimsTransformation** -elementen bevatten die worden gebruikt voor het wijzigen van de uitvoer claims of voor het genereren van nieuwe.

In het volgende voor beeld ziet u de claim die wordt geretourneerd door de REST API:

- De **MembershipId** -claim die is toegewezen aan de naam van de **loyaltyNumber** -claim.

Het technische profiel retourneert ook claims die niet worden geretourneerd door de ID-provider:

- De **loyaltyNumberIsNew** -claim waarvoor een standaard waarde is ingesteld op `true`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="loyaltyNumber" PartnerClaimType="MembershipId" />
  <OutputClaim ClaimTypeReferenceId="loyaltyNumberIsNew" DefaultValue="true" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| ServiceUrl | Ja | De URL van het REST API-eind punt. |
| AuthenticationType | Ja | Het type verificatie dat wordt uitgevoerd door de claim provider voor de REST. Mogelijke waarden: `None`, `Basic`of `ClientCertificate`. De waarde `None` geeft aan dat de REST API niet anoniem is. De waarde `Basic` geeft aan dat de REST API is beveiligd met HTTP Basic-verificatie. Alleen geverifieerde gebruikers, met inbegrip van Azure AD B2C, hebben toegang tot uw API. De waarde `ClientCertificate` (aanbevolen) geeft aan dat de REST API de toegang beperkt met behulp van verificatie op basis van client certificaten. Alleen services met de juiste certificaten, zoals Azure AD B2C hebben toegang tot uw service. |
| SendClaimsIn | Nee | Hiermee wordt aangegeven hoe de invoer claims worden verzonden naar de claim provider voor de REST. Mogelijke waarden: `Body` (standaard), `Form`, `Header`of `QueryString`. De `Body` waarde is de invoer claim die wordt verzonden in de hoofd tekst van de aanvraag in de JSON-indeling. De `Form` waarde is de invoer claim die in de hoofd tekst van de aanvraag wordt verzonden in een ampersand ' & ' gescheiden sleutel waarde-indeling. De `Header` waarde is de invoer claim die in de aanvraag header wordt verzonden. De `QueryString` waarde is de invoer claim die wordt verzonden in de query teken reeks van de aanvraag. |
| ClaimsFormat | Nee | Hiermee geeft u de indeling voor de uitvoer claims op. Mogelijke waarden: `Body` (standaard), `Form`, `Header`of `QueryString`. De `Body` waarde is de uitvoer claim die in de hoofd tekst van de aanvraag wordt verzonden in de JSON-indeling. De `Form` waarde is de uitvoer claim die in de hoofd tekst van de aanvraag wordt verzonden in een ampersand ' & ' gescheiden sleutel waarde-indeling. De `Header` waarde is de uitvoer claim die in de aanvraag header wordt verzonden. De `QueryString` waarde is de uitvoer claim die wordt verzonden in de query teken reeks van de aanvraag. |
| DebugMode | Nee | Voert het technische profiel in de foutopsporingsmodus. In de foutopsporingsmodus kan de REST API meer informatie retour neren. Zie de sectie fout bericht over het retour neren. |

## <a name="cryptographic-keys"></a>Cryptografische sleutels

Als het type verificatie is ingesteld op `None`, wordt het element **CryptographicKeys** niet gebruikt.

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">None</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
</TechnicalProfile>
```

Als het type verificatie is ingesteld op `Basic`, bevat het element **CryptographicKeys** de volgende kenmerken:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| BasicAuthenticationUsername | Ja | De gebruikers naam die wordt gebruikt voor verificatie. |
| BasicAuthenticationPassword | Ja | Het wacht woord dat wordt gebruikt voor verificatie. |

In het volgende voor beeld ziet u een technisch profiel met basis verificatie:

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">Basic</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="BasicAuthenticationUsername" StorageReferenceId="B2C_1A_B2cRestClientId" />
    <Key Id="BasicAuthenticationPassword" StorageReferenceId="B2C_1A_B2cRestClientSecret" />
  </CryptographicKeys>
</TechnicalProfile>
```

Als het type verificatie is ingesteld op `ClientCertificate`, bevat het **CryptographicKeys** -element het volgende kenmerk:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| ClientCertificate | Ja | Het x509-certificaat (RSA key set) dat moet worden gebruikt voor verificatie. |

```XML
<TechnicalProfile Id="REST-API-SignUp">
  <DisplayName>Validate user's input data and return loyaltyNumber claim</DisplayName>
  <Protocol Name="Proprietary" Handler="Web.TPEngine.Providers.RestfulProvider, Web.TPEngine, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null" />
  <Metadata>
    <Item Key="ServiceUrl">https://your-app-name.azurewebsites.NET/api/identity/signup</Item>
    <Item Key="AuthenticationType">ClientCertificate</Item>
    <Item Key="SendClaimsIn">Body</Item>
  </Metadata>
  <CryptographicKeys>
    <Key Id="ClientCertificate" StorageReferenceId="B2C_1A_B2cRestClientCertificate" />
  </CryptographicKeys>
</TechnicalProfile>
```

## <a name="returning-error-message"></a>Fout bericht retour neren

Uw REST API moet mogelijk een fout bericht retour neren, zoals ' de gebruiker is niet gevonden in het CRM-systeem '. Als er een fout optreedt, moet de REST API een HTTP 409-fout bericht retour neren (status code voor de conflict reactie) met de volgende kenmerken:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| versie | Ja | 1.0.0 |
| status | Ja | 409 |
| code | Nee | Een fout code van de REST-eindpunt provider, die wordt weer gegeven wanneer `DebugMode` is ingeschakeld. |
| requestId | Nee | Een aanvraag-id van de REST-eindpunt provider, die wordt weer gegeven wanneer `DebugMode` is ingeschakeld. |
| userMessage | Ja | Een fout bericht dat wordt weer gegeven aan de gebruiker. |
| developerMessage | Nee | De uitgebreide beschrijving van het probleem en hoe het kan worden opgelost, dat wordt weer gegeven wanneer `DebugMode` is ingeschakeld. |
| moreInfo | Nee | Een URI die verwijst naar extra informatie, die wordt weer gegeven wanneer `DebugMode` is ingeschakeld. |

In het volgende voor beeld ziet u een REST API die een fout bericht retourneert dat is ingedeeld in JSON:

```JSON
{
  "version": "1.0.0",
  "status": 409,
  "code": "API12345",
  "requestId": "50f0bd91-2ff4-4b8f-828f-00f170519ddb",
  "userMessage": "Message for the user",
  "developerMessage": "Verbose description of problem and how to fix it.",
  "moreInfo": "https://restapi/error/API12345/moreinfo"
}
```

In het volgende voor beeld C# ziet u een-klasse die een fout bericht retourneert:

```csharp
public class ResponseContent
{
  public string version { get; set; }
  public int status { get; set; }
  public string code { get; set; }
  public string userMessage { get; set; }
  public string developerMessage { get; set; }
  public string requestId { get; set; }
  public string moreInfo { get; set; }
}
```

## <a name="examples"></a>Voorbeelden:
- [REST API claims-uitwisselingen integreren in uw Azure AD B2C gebruikers traject als validatie van gebruikers invoer](active-directory-b2c-custom-rest-api-netfw.md)
- [Beveilig uw REST-services met behulp van HTTP-basis verificatie](active-directory-b2c-custom-rest-api-netfw-secure-basic.md)
- [Uw REST-service beveiligen met behulp van client certificaten](active-directory-b2c-custom-rest-api-netfw-secure-cert.md)
- [Walkthrough: REST API claims-uitwisselingen integreren in uw Azure AD B2C gebruikers door voeren als validatie op gebruikers invoer](active-directory-b2c-rest-api-validation-custom.md)

 














