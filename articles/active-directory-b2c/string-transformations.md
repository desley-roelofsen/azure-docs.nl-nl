---
title: Voor beelden van teken reeksen voor claim transformatie voor aangepaste beleids regels
titleSuffix: Azure AD B2C
description: String-voor beelden van claim transformatie voor het IEF-schema (Identity experience Framework) van Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: cb3b10ca67ab283b999e4fff8e3bb79ae3b59745
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74950813"
---
# <a name="string-claims-transformations"></a>Teken reeks claim transformaties

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

In dit artikel vindt u voor beelden van het gebruik van de teken reeks claim transformaties van het Framework schema voor identiteits ervaring in Azure Active Directory B2C (Azure AD B2C). Zie [ClaimsTransformations](claimstransformations.md)voor meer informatie.

## <a name="assertstringclaimsareequal"></a>AssertStringClaimsAreEqual

Vergelijk twee claims en verwerp een uitzonde ring als deze niet gelijk zijn aan de opgegeven vergelijking inputClaim1, inputClaim2 en stringComparison.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Het eerste claim type, dat moet worden vergeleken. |
| InputClaim | inputClaim2 | string | Tweede claim type, dat moet worden vergeleken. |
| InputParameter | stringComparison | string | teken reeks vergelijking, een van de waarden: Ordinal, OrdinalIgnoreCase. |

De **AssertStringClaimsAreEqual** -claim transformatie wordt altijd uitgevoerd op basis van een [validatie technische profiel](validation-technical-profile.md) dat wordt aangeroepen door een [zelf-bevestigd technisch profiel](self-asserted-technical-profile.md). De meta gegevens van het zelfondertekende technische profiel **UserMessageIfClaimsTransformationStringsAreNotEqual** bepalen het fout bericht dat aan de gebruiker wordt gepresenteerd.

![AssertStringClaimsAreEqual-uitvoering](./media/string-transformations/assert-execution.png)

U kunt deze claim transformatie gebruiken om ervoor te zorgen dat twee ClaimTypes dezelfde waarde hebben. Als dat niet het geval is, wordt er een fout bericht gegenereerd. In het volgende voor beeld wordt gecontroleerd of het **strongAuthenticationEmailAddress** claim type gelijk is aan **e-mail** claim type. Anders wordt er een fout bericht gegenereerd.

```XML
<ClaimsTransformation Id="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" TransformationMethod="AssertStringClaimsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="strongAuthenticationEmailAddress" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
  </InputParameters>
</ClaimsTransformation>
```


Met het technische profiel voor niet **-interactieve** validatie wordt de **AssertEmailAndStrongAuthenticationEmailAddressAreEqual** -claim transformatie aangeroepen.
```XML
<TechnicalProfile Id="login-NonInteractive">
  ...
  <OutputClaimsTransformations>
    <OutputClaimsTransformation ReferenceId="AssertEmailAndStrongAuthenticationEmailAddressAreEqual" />
  </OutputClaimsTransformations>
</TechnicalProfile>
```

Het zelfondertekende technische profiel aanroept het technische profiel voor validatie **aanmelding-niet-interactief** .

```XML
<TechnicalProfile Id="SelfAsserted-LocalAccountSignin-Email">
  <Metadata>
    <Item Key="UserMessageIfClaimsTransformationStringsAreNotEqual">Custom error message the email addresses you provided are not the same.</Item>
  </Metadata>
  <ValidationTechnicalProfiles>
    <ValidationTechnicalProfile ReferenceId="login-NonInteractive" />
  </ValidationTechnicalProfiles>
</TechnicalProfile>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
    - Invoer parameters:
  - **stringComparison**: ordinalIgnoreCase
- Resultaat: er is een fout opgetreden

## <a name="changecase"></a>ChangeCase

Wijzigt het hoofdletter gebruik van de gegeven claim in onderste of hoofd letters, afhankelijk van de operator.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Het claim type dat moet worden gewijzigd. |
| InputParameter | toCase | string | Een van de volgende waarden: `LOWER` of `UPPER`. |
| OutputClaim | outputClaim | string | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen. |

Gebruik deze claim transformatie om een wille keurig teken reeks claim type te wijzigen in lager of hoofd letters.

```XML
<ClaimsTransformation Id="ChangeToLower" TransformationMethod="ChangeCase">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="inputClaim1" />
  </InputClaims>
<InputParameters>
  <InputParameter Id="toCase" DataType="string" Value="LOWER" />
</InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="email" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **e-mail**: SomeOne@contoso.com
- Invoer parameters:
    - **toCase**: lager
- Uitvoer claims:
  - **e-mail**: someone@contoso.com

## <a name="createstringclaim"></a>CreateStringClaim

Hiermee wordt een teken reeks claim gemaakt op basis van de opgegeven invoer parameter in het beleid.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
|----- | ----------------------- | --------- | ----- |
| InputParameter | waarde | string | De teken reeks die moet worden ingesteld |
| OutputClaim | createdClaim | string | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen, met de waarde die is opgegeven in de invoer parameter. |

Gebruik deze claim transformatie om een teken reeks claim waarde in te stellen.

```XML
<ClaimsTransformation Id="CreateTermsOfService" TransformationMethod="CreateStringClaim">
  <InputParameters>
    <InputParameter Id="value" DataType="string" Value="Contoso terms of service..." />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="TOS" TransformationClaimType="createdClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer parameter:
    - **waarde**: contoso Service voorwaarden...
- Uitvoer claims:
    - **createdClaim**: het TOS claim type bevat de ' contoso-Service voorwaarden... ' Value.

## <a name="compareclaims"></a>CompareClaims

Bepaal of een claim van een teken reeks gelijk is aan een andere. Het resultaat is een nieuw Boolean-claim type met de waarde `true` of `false`.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Eerste claim type, dat moet worden vergeleken. |
| InputClaim | inputClaim2 | string | Tweede claim type, dat moet worden vergeleken. |
| InputParameter | operator | string | Mogelijke waarden: `EQUAL` of `NOT EQUAL`. |
| InputParameter | ignoreCase | booleaans | Hiermee geeft u op of deze vergelijking het hoofdletter gebruik moet negeren van de teken reeksen die worden vergeleken. |
| OutputClaim | outputClaim | booleaans | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen. |

Gebruik deze claim transformatie om te controleren of een claim gelijk is aan een andere claim. De volgende claim transformatie controleert bijvoorbeeld of de waarde van de **e-mail** claim gelijk is aan de **geverifieerde. e-mail** claim.

```XML
<ClaimsTransformation Id="CheckEmail" TransformationMethod="CompareClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="Email" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="Verified.Email" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="operator" DataType="string" Value="NOT EQUAL" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="SameEmailAddress" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **inputClaim1**: someone@contoso.com
  - **inputClaim2**: someone@outlook.com
- Invoer parameters:
    - **operator**: niet gelijk aan
    - **ignoreCase**: True
- Uitvoer claims:
    - **output claim**: True

## <a name="compareclaimtovalue"></a>CompareClaimToValue

Hiermee wordt bepaald of een claim waarde gelijk is aan de waarde van de invoer parameter.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim1 | string | Het claim type, dat moet worden vergeleken. |
| InputParameter | operator | string | Mogelijke waarden: `EQUAL` of `NOT EQUAL`. |
| InputParameter | compareTo | string | teken reeks vergelijking, een van de waarden: Ordinal, OrdinalIgnoreCase. |
| InputParameter | ignoreCase | booleaans | Hiermee geeft u op of deze vergelijking het hoofdletter gebruik moet negeren van de teken reeksen die worden vergeleken. |
| OutputClaim | outputClaim | booleaans | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen. |

U kunt deze claim transformatie gebruiken om te controleren of een claim gelijk is aan een waarde die u hebt opgegeven. De volgende claim transformatie controleert bijvoorbeeld of de waarde van de claim **termsOfUseConsentVersion** gelijk is aan `v1`.

```XML
<ClaimsTransformation Id="IsTermsOfUseConsentRequiredForVersion" TransformationMethod="CompareClaimToValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim1" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="compareTo" DataType="string" Value="V1" />
    <InputParameter Id="operator" DataType="string" Value="not equal" />
    <InputParameter Id="ignoreCase" DataType="string" Value="true" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentRequired" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld
- Invoer claims:
    - **inputClaim1**: v1
- Invoer parameters:
    - **CompareTo**: v1
    - **operator**: gelijk
    - **ignoreCase**: True
- Uitvoer claims:
    - **output claim**: True

## <a name="createrandomstring"></a>CreateRandomString

Hiermee maakt u een wille keurige teken reeks met de generator wille keurige getallen. Als de generator voor wille keurige getallen van het type `integer`is, kunt u desgewenst een Seed-para meter en een maximum aantal opgeven. Met een optionele para meter voor teken reeks notatie kan de uitvoer worden opgemaakt met behulp van deze, en een optionele base64-para meter geeft aan of de uitvoer Base64 Encoded randomGeneratorType [GUID, integer] output claim (teken reeks) is.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputParameter | randomGeneratorType | string | Hiermee geeft u de wille keurige waarde die moet worden gegenereerd, `GUID` (globale unieke ID) of `INTEGER` (een getal). |
| InputParameter | stringFormat | string | Beschrijving Format teer de wille keurige waarde. |
| InputParameter | base64 | booleaans | Beschrijving Zet de wille keurige waarde om in base64. Als de teken reeks indeling wordt toegepast, wordt de waarde na de teken reeks indeling gecodeerd in base64. |
| InputParameter | maximumNumber | int | Beschrijving Alleen voor `INTEGER` randomGeneratorType. Geef het maximum aantal op. |
| InputParameter | meerder  | int | Beschrijving Alleen voor `INTEGER` randomGeneratorType. Geef het Seed op voor de wille keurige waarde. Opmerking: hetzelfde zaad levert dezelfde reeks wille keurige getallen op. |
| OutputClaim | outputClaim | string | De ClaimTypes die wordt geproduceerd nadat deze claim transformatie is aangeroepen. De wille keurige waarde. |

In het volgende voor beeld wordt een algemene unieke ID gegenereerd. Deze claim transformatie wordt gebruikt om de wille keurige UPN (User Principle Name) te maken.

```XML
<ClaimsTransformation Id="CreateRandomUPNUserName" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="GUID" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Voorbeeld

- Invoer parameters:
    - **randomGeneratorType**: GUID
- Uitvoer claims:
    - **output claim**: bc8bedd2-aaa3-411e-bdee-2f1810b73dfc

In het volgende voor beeld wordt een wille keurige waarde voor geheel getal gegenereerd tussen 0 en 1000. De waarde wordt opgemaakt als OTP_ {wille keurige waarde}.

```XML
<ClaimsTransformation Id="SetRandomNumber" TransformationMethod="CreateRandomString">
  <InputParameters>
    <InputParameter Id="randomGeneratorType" DataType="string" Value="INTEGER" />
    <InputParameter Id="maximumNumber" DataType="int" Value="1000" />
    <InputParameter Id="stringFormat" DataType="string" Value="OTP_{0}" />
    <InputParameter Id="base64" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="randomNumber" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer parameters:
    - **randomGeneratorType**: geheel getal
    - **maximumNumber**: 1000
    - **stringFormat**: OTP_{0}
    - **Base64**: onwaar
- Uitvoer claims:
    - **output claim**: OTP_853


## <a name="formatstringclaim"></a>FormatStringClaim

Een claim opmaken volgens de gegeven teken reeks voor opmaak. Deze trans formatie maakt C# gebruik van de `String.Format` methode.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |string |Het claim type dat fungeert als teken reeks notatie {0} para meter. |
| InputParameter | stringFormat | string | De teken reeks notatie, met inbegrip van de para meter {0}. |
| OutputClaim | outputClaim | string | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen. |

Gebruik deze claim transformatie voor het format teren van een wille keurige teken reeks met één para meter {0}. In het volgende voor beeld wordt een **userPrincipalName**gemaakt. Alle technische profielen van de sociale ID-provider, zoals `Facebook-OAUTH` roept de **CreateUserPrincipalName** aan om een **userPrincipalName**te genereren.

```XML
<ClaimsTransformation Id="CreateUserPrincipalName" TransformationMethod="FormatStringClaim">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="upnUserName" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="cpim_{0}@{RelyingPartyTenantId}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="userPrincipalName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
    - **input claim**: 5164db16-3eee-4629-bfda-dcc3326790e9
- Invoer parameters:
    - **stringFormat**:  cpim_{0}@{RelyingPartyTenantId}
- Uitvoer claims:
  - **outputClaim**: cpim_5164db16-3eee-4629-bfda-dcc3326790e9@b2cdemo.onmicrosoft.com

## <a name="formatstringmultipleclaims"></a>FormatStringMultipleClaims

Indeling van twee claims volgens de gegeven teken reeks voor opmaak. Deze trans formatie maakt C# gebruik van de methode **String. Format** .

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim |string | Het claim type dat fungeert als teken reeks notatie {0} para meter. |
| InputClaim | inputClaim | string | Het claim type dat fungeert als teken reeks notatie {1} para meter. |
| InputParameter | stringFormat | string | De teken reeks notatie, met inbegrip van de para meters {0} en {1}. |
| OutputClaim | outputClaim | string | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen. |

Gebruik deze claim transformatie om een wille keurige teken reeks te Format teren met twee para meters, {0} en {1}. In het volgende voor beeld wordt een **DisplayName** gemaakt met de opgegeven indeling:

```XML
<ClaimsTransformation Id="CreateDisplayNameFromFirstNameAndLastName" TransformationMethod="FormatStringMultipleClaims">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="givenName" TransformationClaimType="inputClaim1" />
    <InputClaim ClaimTypeReferenceId="surName" TransformationClaimType="inputClaim2" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="stringFormat" DataType="string" Value="{0} {1}" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="displayName" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
    - **inputClaim1**: Joe
    - **inputClaim2**: Fernando
- Invoer parameters:
    - **stringFormat**: {0} {1}
- Uitvoer claims:
    - **output claim**: Joe Fernando

## <a name="getmappedvaluefromlocalizedcollection"></a>GetMappedValueFromLocalizedCollection

Een item opzoeken op basis van een verzameling claim **beperkingen** .

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | mapFromClaim | string | De claim die de tekst bevat die moet worden opgezocht in de **restrictionValueClaim** claims met de **beperkings** verzameling.  |
| OutputClaim | restrictionValueClaim | string | De claim die de **beperkings** verzameling bevat. Nadat de claim transformatie is aangeroepen, bevat de waarde van deze claim de waarde van het geselecteerde item. |

In het volgende voor beeld wordt de beschrijving van het fout bericht opgezocht op basis van de fout code. De **responseMsg** -claim bevat een verzameling fout berichten die aan de eind gebruiker moeten worden gepresenteerd of naar de Relying Party worden verzonden.

```XML
<ClaimType Id="responseMsg">
  <DisplayName>Error message: </DisplayName>
  <DataType>string</DataType>
  <UserInputType>Paragraph</UserInputType>
  <Restriction>
    <Enumeration Text="B2C_V1_90001" Value="You cant sign in because you are a minor" />
    <Enumeration Text="B2C_V1_90002" Value="This action can only be performed by gold members" />
    <Enumeration Text="B2C_V1_90003" Value="You have not been enabled for this operation" />
  </Restriction>
</ClaimType>
```
De claim transformatie zoekt de tekst van het item en retourneert de waarde ervan. Als de beperking is gelokaliseerd met behulp van `<LocalizedCollection>`, retourneert de trans formatie van claims de gelokaliseerde waarde.

```XML
<ClaimsTransformation Id="GetResponseMsgMappedToResponseCode" TransformationMethod="GetMappedValueFromLocalizedCollection">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="responseCode" TransformationClaimType="mapFromClaim" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="responseMsg" TransformationClaimType="restrictionValueClaim" />        
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
    - **mapFromClaim**: B2C_V1_90001
- Uitvoer claims:
    - **restrictionValueClaim**: u kunt zich niet aanmelden omdat u een kleine.

## <a name="lookupvalue"></a>LookupValue

Zoek een claim waarde op uit een lijst met waarden op basis van de waarde van een andere claim.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputParameterId | string | De claim die de opzoek waarde bevat |
| InputParameter | |string | Verzameling invoer parameters. |
| InputParameter | errorOnFailedLookup | booleaans | Controleren of er een fout wordt geretourneerd als er geen overeenkomende zoek actie is. |
| OutputClaim | inputParameterId | string | De ClaimTypes die wordt geproduceerd nadat deze claim transformatie is aangeroepen. De waarde van de overeenkomende id. |

In het volgende voor beeld wordt de domein naam in een van de input parameters-verzamelingen opgezocht. De claim transformatie zoekt de domein naam in de id en retourneert de waarde (een toepassings-ID).

```XML
 <ClaimsTransformation Id="DomainToClientId" TransformationMethod="LookupValue">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="inputParameterId" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="contoso.com" DataType="string" Value="13c15f79-8fb1-4e29-a6c9-be0d36ff19f1" />
    <InputParameter Id="microsoft.com" DataType="string" Value="0213308f-17cb-4398-b97e-01da7bd4804e" />
    <InputParameter Id="test.com" DataType="string" Value="c7026f88-4299-4cdb-965d-3f166464b8a9" />
    <InputParameter Id="errorOnFailedLookup" DataType="boolean" Value="false" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainAppId" TransformationClaimType="outputClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
    - **inputParameterId**: test.com
- Invoer parameters:
    - **contoso.com**: 13c15f79-8fb1-4e29-a6c9-be0d36ff19f1
    - **Microsoft.com**: 0213308f-17cb-4398-b97e-01da7bd4804e
    - **test.com**: c7026f88-4299-4cdb-965d-3f166464b8a9
    - **errorOnFailedLookup**: False
- Uitvoer claims:
    - **output claim**: c7026f88-4299-4cdb-965d-3f166464b8a9

## <a name="nullclaim"></a>NullClaim

De waarde van een bepaalde claim opschonen.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| OutputClaim | claim_to_null | string | De waarde van de claim moet NULL zijn. |

Gebruik deze claim transformatie om overbodige gegevens te verwijderen uit de eigenschap Bag van claims. De sessie cookie is dus kleiner. In het volgende voor beeld wordt de waarde van het claim type `TermsOfService` verwijderd.

```XML
<ClaimsTransformation Id="SetTOSToNull" TransformationMethod="NullClaim">
  <OutputClaims>
  <OutputClaim ClaimTypeReferenceId="TermsOfService" TransformationClaimType="claim_to_null" />
  </OutputClaims>
</ClaimsTransformation>
```

- Invoer claims:
    - **output claim**: Welkom bij de app contoso. Als u doorgaat met bladeren en deze website gebruikt, gaat u ermee akkoord dat u voldoet aan de volgende voor waarden...
- Uitvoer claims:
    - **output claim**: null

## <a name="parsedomain"></a>ParseDomain

Hiermee wordt het domein gedeelte van een e-mail adres opgehaald.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | emailAddress | string | Het claim type dat het e-mail adres bevat. |
| OutputClaim | domein | string | Het claim type dat is geproduceerd nadat deze claim transformatie is aangeroepen-het domein. |

Gebruik deze claim transformatie voor het parseren van de domein naam na het @-symbool van de gebruiker. Dit kan handig zijn bij het verwijderen van persoons gegevens (PII) uit controle gegevens. De volgende claim transformatie laat zien hoe u de domein naam kunt parseren vanuit een **e-mail** claim.

```XML
<ClaimsTransformation Id="SetDomainName" TransformationMethod="ParseDomain">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="email" TransformationClaimType="emailAddress" />
  </InputClaims>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="domainName" TransformationClaimType="domain" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
  - **emailAddress**: joe@outlook.com
- Uitvoer claims:
    - **domein**: Outlook.com

## <a name="setclaimsifstringsareequal"></a>SetClaimsIfStringsAreEqual

Controleert of een teken reeks claim en `matchTo` invoer parameter gelijk zijn, en stelt de uitvoer claims in met de waarde die aanwezig is in `stringMatchMsg` en `stringMatchMsgCode` invoer parameters, samen met de resultaat uitvoer-claim, die moet worden ingesteld als `true` of `false` op basis van het resultaat van de vergelijking.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | inputClaim | string | Het claim type, dat moet worden vergeleken. |
| InputParameter | matchTo | string | De teken reeks die moet worden vergeleken met `inputClaim`. |
| InputParameter | stringComparison | string | Mogelijke waarden: `Ordinal` of `OrdinalIgnoreCase`. |
| InputParameter | stringMatchMsg | string | De eerste waarde die moet worden ingesteld als teken reeksen gelijk zijn. |
| InputParameter | stringMatchMsgCode | string | De tweede waarde die moet worden ingesteld als teken reeksen gelijk zijn. |
| OutputClaim | outputClaim1 | string | Als teken reeksen gelijk zijn, bevat deze uitvoer claim de waarde van `stringMatchMsg` invoer parameter. |
| OutputClaim | outputClaim2 | string | Als teken reeksen gelijk zijn, bevat deze uitvoer claim de waarde van `stringMatchMsgCode` invoer parameter. |
| OutputClaim | stringCompareResultClaim | booleaans | Het type claim voor uitvoer resultaat dat moet worden ingesteld als `true` of `false` op basis van het resultaat van de vergelijking. |

U kunt deze claim transformatie gebruiken om te controleren of een claim gelijk is aan de waarde die u hebt opgegeven. De volgende claim transformatie controleert bijvoorbeeld of de waarde van de claim **termsOfUseConsentVersion** gelijk is aan `v1`. Zo ja, wijzig de waarde in `v2`.

```XML
<ClaimsTransformation Id="CheckTheTOS" TransformationMethod="SetClaimsIfStringsAreEqual">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="inputClaim" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="v1" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="stringMatchMsg" DataType="string" Value="B2C_V1_90005" />
    <InputParameter Id="stringMatchMsgCode" DataType="string" Value="The TOS is upgraded to v2" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersion" TransformationClaimType="outputClaim1" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeCode" TransformationClaimType="outputClaim2" />
    <OutputClaim ClaimTypeReferenceId="termsOfUseConsentVersionUpgradeResult" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```
### <a name="example"></a>Voorbeeld

- Invoer claims:
    - **inputClaim**: v1
- Invoer parameters:
    - **matchTo**: v1
    - **stringComparison**: ordinalIgnoreCase
    - **stringMatchMsg**: B2C_V1_90005
    - **stringMatchMsgCode**: de TOS wordt bijgewerkt naar v2
- Uitvoer claims:
    - **outputClaim1**: B2C_V1_90005
    - **outputClaim2**: de TOS wordt bijgewerkt naar v2
    - **stringCompareResultClaim**: True

## <a name="setclaimsifstringsmatch"></a>SetClaimsIfStringsMatch

Controleert of een teken reeks claim en `matchTo` invoer parameter gelijk zijn, en stelt de uitvoer claims in met de waarde die aanwezig is in `outputClaimIfMatched` invoer parameter, samen met resultaat uitvoer claim vergelijken, die moet worden ingesteld als `true` of `false` op basis van het resultaat van de vergelijking.

| Item | TransformationClaimType | Gegevenstype | Opmerkingen |
| ---- | ----------------------- | --------- | ----- |
| InputClaim | claimToMatch | string | Het claim type, dat moet worden vergeleken. |
| InputParameter | matchTo | string | De teken reeks die moet worden vergeleken met input claim. |
| InputParameter | stringComparison | string | Mogelijke waarden: `Ordinal` of `OrdinalIgnoreCase`. |
| InputParameter | outputClaimIfMatched | string | De waarde die moet worden ingesteld als teken reeksen gelijk zijn. |
| OutputClaim | outputClaim | string | Als teken reeksen gelijk zijn, bevat deze uitvoer claim de waarde van `outputClaimIfMatched` invoer parameter. Of null, als de teken reeksen niet overeenkomen. |
| OutputClaim | stringCompareResultClaim | booleaans | Het type claim voor uitvoer resultaat dat moet worden ingesteld als `true` of `false` op basis van het resultaat van de vergelijking. |

De volgende claim transformatie controleert bijvoorbeeld of de waarde van de claim **ageGroup** gelijk is aan `Minor`. Als dit het geval is, retourneert u de waarde in `B2C_V1_90001`.

```XML
<ClaimsTransformation Id="SetIsMinor" TransformationMethod="SetClaimsIfStringsMatch">
  <InputClaims>
    <InputClaim ClaimTypeReferenceId="ageGroup" TransformationClaimType="claimToMatch" />
  </InputClaims>
  <InputParameters>
    <InputParameter Id="matchTo" DataType="string" Value="Minor" />
    <InputParameter Id="stringComparison" DataType="string" Value="ordinalIgnoreCase" />
    <InputParameter Id="outputClaimIfMatched" DataType="string" Value="B2C_V1_90001" />
  </InputParameters>
  <OutputClaims>
    <OutputClaim ClaimTypeReferenceId="isMinor" TransformationClaimType="outputClaim" />
    <OutputClaim ClaimTypeReferenceId="isMinorResponseCode" TransformationClaimType="stringCompareResultClaim" />
  </OutputClaims>
</ClaimsTransformation>
```

### <a name="example"></a>Voorbeeld

- Invoer claims:
    - **claimToMatch**: Minor
- Invoer parameters:
    - **matchTo**: Minor
    - **stringComparison**: ordinalIgnoreCase
    - **outputClaimIfMatched**: B2C_V1_90001
- Uitvoer claims:
    - **isMinorResponseCode**: B2C_V1_90001
    - **isMinor**: True

