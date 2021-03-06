---
title: Een OAuth1 technisch profiel definiëren in een aangepast beleid
titleSuffix: Azure AD B2C
description: Definieer een OAuth 1,0 Technical-profiel in een aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: d97d908ddf5d55bf09d96a5ef16fa79a7afde7b4
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951102"
---
# <a name="define-an-oauth1-technical-profile-in-an-azure-active-directory-b2c-custom-policy"></a>Een OAuth1 technisch profiel definiëren in een Azure Active Directory B2C aangepast beleid

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Azure Active Directory B2C (Azure AD B2C) biedt ondersteuning voor de [OAuth 1,0-protocol](https://tools.ietf.org/html/rfc5849) -ID-provider. In dit artikel worden de specifieke specificaties beschreven van een technisch profiel voor interactie met een claim provider die ondersteuning biedt voor dit gestandaardiseerde protocol. Met een OAuth1-technisch profiel kunt u communiceren met een OAuth1-gebaseerde ID-provider, zoals Twitter. Met federeren met de ID-provider kunnen gebruikers zich aanmelden met hun bestaande sociale of bedrijfs identiteiten.

## <a name="protocol"></a>Protocol

Het **naam** kenmerk van het **protocol** element moet worden ingesteld op `OAuth1`. Het protocol voor het technische profiel **Twitter-OAUTH1** is bijvoorbeeld `OAuth1`.

```XML
<TechnicalProfile Id="Twitter-OAUTH1">
  <DisplayName>Twitter</DisplayName>
  <Protocol Name="OAuth1" />
  ...
```

## <a name="input-claims"></a>Invoer claims

De **InputClaims** -en **InputClaimsTransformations** -elementen zijn leeg of ontbreken.

## <a name="output-claims"></a>Uitvoer claims

Het **OutputClaims** -element bevat een lijst met claims die zijn geretourneerd door de OAuth1-ID-provider. Mogelijk moet u de naam van de claim die in uw beleid is gedefinieerd, toewijzen aan de naam die is gedefinieerd in de ID-provider. U kunt ook claims toevoegen die niet worden geretourneerd door de ID-provider zolang u het kenmerk **DefaultValue** hebt ingesteld.

Het **OutputClaimsTransformations** -element kan een verzameling **OutputClaimsTransformation** -elementen bevatten die worden gebruikt voor het wijzigen van de uitvoer claims of voor het genereren van nieuwe.

In het volgende voor beeld worden de claims weer gegeven die zijn geretourneerd door de Twitter-ID-provider:

- De **user_id** claim die is toegewezen aan de **issuerUserId** -claim.
- De **screen_name** claim die is toegewezen aan de **DisplayName** -claim.
- De **e-mail** claim zonder naam toewijzing.

Het technische profiel retourneert ook claims die niet worden geretourneerd door de ID-provider:

- De claim **Identity provider** die de naam van de ID-provider bevat.
- De **authenticationSource** claim met een standaard waarde van `socialIdpAuthentication`.

```xml
<OutputClaims>
  <OutputClaim ClaimTypeReferenceId="issuerUserId" PartnerClaimType="user_id" />
  <OutputClaim ClaimTypeReferenceId="displayName" PartnerClaimType="screen_name" />
  <OutputClaim ClaimTypeReferenceId="email" />
  <OutputClaim ClaimTypeReferenceId="identityProvider" DefaultValue="twitter.com" />
  <OutputClaim ClaimTypeReferenceId="authenticationSource" DefaultValue="socialIdpAuthentication" />
</OutputClaims>
```

## <a name="metadata"></a>Metagegevens

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| client_id | Ja | De toepassings-id van de ID-provider. |
| ProviderName | Nee | De naam van de ID-provider. |
| request_token_endpoint | Ja | De URL van het eind punt van het aanvraag token conform RFC 5849. |
| authorization_endpoint | Ja | De URL van het autorisatie-eind punt conform RFC 5849. |
| access_token_endpoint | Ja | De URL van het eind punt van het token conform RFC 5849. |
| ClaimsEndpoint | Nee | De URL van het eind punt voor gebruikers informatie. |
| ClaimsResponseFormat | Nee | De claim antwoord indeling.|

## <a name="cryptographic-keys"></a>Cryptografische sleutels

Het element **CryptographicKeys** bevat het volgende kenmerk:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| client_secret | Ja | Het client geheim van de identiteits provider toepassing.   |

## <a name="redirect-uri"></a>Omleidings-URI

Wanneer u de omleidings-URL van uw ID-provider configureert, voert u `https://login.microsoftonline.com/te/tenant/policyId/oauth1/authresp`in. Zorg ervoor dat u de **Tenant** vervangt door de naam van uw Tenant (bijvoorbeeld contosob2c.onmicrosoft.com) en **policyId** met de id van uw beleid (bijvoorbeeld b2c_1a_policy). De omleidings-URI moet in alle kleine letters zijn. Voeg een omleidings-URL toe voor alle beleids regels die gebruikmaken van de aanmelding van de identiteits provider.

Als u het **b2clogin.com** -domein gebruikt in plaats van **login.microsoftonline.com** , moet u ervoor zorgen dat u b2clogin.com gebruikt in plaats van login.microsoftonline.com.

Voorbeelden:

- [Twitter toevoegen als een OAuth1-ID-provider met behulp van aangepast beleid](active-directory-b2c-custom-setup-twitter-idp.md)













