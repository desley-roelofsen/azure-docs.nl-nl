---
title: Predikaten en PredicateValidations
titleSuffix: Azure AD B2C
description: Voor komen dat ongeldige gegevens worden toegevoegd aan uw Azure AD B2C-Tenant met behulp van aangepast beleid in Azure Active Directory B2C.
services: active-directory-b2c
author: mmacy
manager: celestedg
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 10/28/2019
ms.author: marsma
ms.subservice: B2C
ms.openlocfilehash: a09478bd2e32a1ab484b85fec33ae03878ebb10c
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951017"
---
# <a name="predicates-and-predicatevalidations"></a>Predikaten en PredicateValidations

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Met de elementen **predikaten** en **PredicateValidations** kunt u een validatie proces uitvoeren om ervoor te zorgen dat alleen goed gevormde gegevens worden ingevoerd in uw Azure Active Directory B2C (Azure AD B2C)-Tenant.

In het volgende diagram ziet u de relatie tussen de elementen:

![Diagram van de relatie met de validatie van predikaten en predikaten](./media/predicates/predicates.png)

## <a name="predicates"></a>Predicaten

Het **predicaat** element definieert een basis validatie om de waarde van een claim type te controleren en retourneert `true` of `false`. De validatie wordt uitgevoerd met behulp van een opgegeven **methode** -element en een set **parameter** elementen die relevant zijn voor de methode. Een predikaat kan bijvoorbeeld controleren of de lengte van een teken reeks claim waarde binnen het opgegeven minimum-en maximum aantal para meters ligt, of dat een teken reeks claim waarde een tekenset bevat. Het element **UserHelpText** biedt een fout bericht voor gebruikers als de controle mislukt. De waarde van het element **UserHelpText** kan worden gelokaliseerd met behulp van [taal aanpassing](localization.md).

Het element **predikaten** moet direct na het **ClaimsSchema** -element binnen het element [BuildingBlocks](buildingblocks.md) worden weer gegeven.

Het element **predikaten** bevat het volgende element:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| Predicaat | 1: n | Een lijst met predikaten. |

Het **predicaat** -element bevat de volgende kenmerken:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor het predicaat. Andere elementen kunnen deze id in het beleid gebruiken. |
| Methode | Ja | Het type methode dat moet worden gebruikt voor validatie. Mogelijke waarden: **IsLengthRange**, **MatchesRegex**, **IncludesCharacters**of **IsDateRange**. De waarde **IsLengthRange** controleert of de lengte van een teken reeks claim waarde binnen het opgegeven minimum-en maximum aantal para meters ligt. De waarde **MatchesRegex** controleert of een teken reeks claim waarde overeenkomt met een reguliere expressie. De waarde **IncludesCharacters** controleert of een teken reeks claim waarde een tekenset bevat. De waarde **IsDateRange** controleert of een datum claim waarde ligt tussen een bereik van de opgegeven minimum-en maximum parameters. |

Het **predicaat** -element bevat de volgende elementen:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| UserHelpText | 1:1 | Een fout bericht voor gebruikers als de controle mislukt. Deze teken reeks kan worden gelokaliseerd met behulp van de [taal aanpassing](localization.md) |
| Parameters | 1:1 | De para meters voor het methode type van de teken reeks validatie. |

Het element **para meters** bevat de volgende elementen:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| Parameter | 1: n | De para meters voor het methode type van de teken reeks validatie. |

Het **parameter** element bevat de volgende kenmerken:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| Id | 1:1 | De id van de para meter. |

In het volgende voor beeld ziet u een `IsLengthRange` methode met de para meters `Minimum` en `Maximum` waarmee het lengte bereik van de teken reeks wordt opgegeven:

```XML
<Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
  <UserHelpText>The password must be between 8 and 64 characters.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
      <Parameter Id="Maximum">64</Parameter>
  </Parameters>
</Predicate>
```

In het volgende voor beeld ziet u een `MatchesRegex` methode met de para meter `RegularExpression` die een reguliere expressie specificeert:

```XML
<Predicate Id="PIN" Method="MatchesRegex">
  <UserHelpText>The password must be numbers only.</UserHelpText>
  <Parameters>
    <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
  </Parameters>
</Predicate>
```

In het volgende voor beeld ziet u een `IncludesCharacters` methode met de para meter `CharacterSet` die de set tekens bevat:

```XML
<Predicate Id="Lowercase" Method="IncludesCharacters">
  <UserHelpText>a lowercase letter</UserHelpText>
  <Parameters>
    <Parameter Id="CharacterSet">a-z</Parameter>
  </Parameters>
</Predicate>
```

In het volgende voor beeld ziet u een `IsDateRange` methode met de para meters `Minimum` en `Maximum` waarmee het datum bereik met een notatie van `yyyy-MM-dd` en `Today`wordt opgegeven.

```XML
<Predicate Id="DateRange" Method="IsDateRange" HelpText="The date must be between 1970-01-01 and today.">
  <Parameters>
    <Parameter Id="Minimum">1970-01-01</Parameter>
    <Parameter Id="Maximum">Today</Parameter>
  </Parameters>
</Predicate>
```

## <a name="predicatevalidations"></a>PredicateValidations

Hoewel de predikaten de validatie definiëren voor het controleren op basis van een claim type, wordt in de **PredicateValidations** groep een reeks predikaten gevormd om een gebruikers invoer validatie te vormen die kan worden toegepast op een claim type. Elk **PredicateValidation** -element bevat een set **PredicateGroup** -elementen die een set **PredicateReference** -elementen bevatten die naar een **predikaat**verwijzen. Om de validatie door te voeren, moet de waarde van de claim alle tests van een predikaat door geven onder alle **PredicateGroup** met hun set **PredicateReference** -elementen.

Het element **PredicateValidations** moet direct na het element **predikaten** worden weer gegeven binnen het [BuildingBlocks](buildingblocks.md) -element.

```XML
<PredicateValidations>
  <PredicateValidation Id="">
    <PredicateGroups>
      <PredicateGroup Id="">
        <UserHelpText></UserHelpText>
        <PredicateReferences MatchAtLeast="">
          <PredicateReference Id="" />
          ...
        </PredicateReferences>
      </PredicateGroup>
      ...
    </PredicateGroups>
  </PredicateValidation>
...
</PredicateValidations>
```

Het element **PredicateValidations** bevat het volgende element:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| PredicateValidation | 1: n | Een lijst met validatie van predikaten. |

Het element **PredicateValidation** bevat het volgende kenmerk:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor de validatie van predikaten. Het element **claim** type kan deze id in het beleid gebruiken. |

Het element **PredicateValidation** bevat het volgende element:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| PredicateGroups | 1: n | Een lijst met predicaat groepen. |

Het element **PredicateGroups** bevat het volgende element:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| PredicateGroup | 1: n | Een lijst met predikaten. |

Het element **PredicateGroup** bevat het volgende kenmerk:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor de predicaat groep.  |

Het **PredicateGroup** -element bevat de volgende elementen:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| UserHelpText | 1:1 |  Een beschrijving van het predikaat waarmee gebruikers kunnen zien welke waarde ze moeten typen. |
| PredicateReferences | 1: n | Een lijst met predikaten-verwijzingen. |

Het **PredicateReferences** -element bevat de volgende kenmerken:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| MatchAtLeast | Nee | Hiermee geeft u op dat de waarde moet overeenkomen met een groot aantal predicaat definities voor de invoer die moet worden geaccepteerd. |

Het **PredicateReferences** -element bevat de volgende elementen:

| Element | Instanties | Beschrijving |
| ------- | ----------- | ----------- |
| PredicateReference | 1: n | Een verwijzing naar een predikaat. |

Het **PredicateReference** -element bevat de volgende kenmerken:

| Kenmerk | Verplicht | Beschrijving |
| --------- | -------- | ----------- |
| Id | Ja | Een id die wordt gebruikt voor de validatie van predikaten.  |


## <a name="configure-password-complexity"></a>Wachtwoord complexiteit configureren

Met **predikaten** en **PredicateValidationsInput** kunt u de complexiteits vereisten bepalen voor wacht woorden die door een gebruiker worden verschaft bij het maken van een account. Azure AD B2C maakt standaard gebruik van sterke wacht woorden. Azure AD B2C biedt ook ondersteuning voor configuratie opties voor het beheren van de complexiteit van wacht woorden die klanten kunnen gebruiken. U kunt wachtwoord complexiteit definiëren met behulp van deze predicaat elementen:

- **IsLengthBetween8And64** met behulp van de methode `IsLengthRange`, wordt gevalideerd of het wacht woord tussen 8 en 64 tekens lang moet zijn.
- Met de methode `IncludesCharacters` wordt in **kleine letters** gecontroleerd of het wacht woord een kleine letter bevat.
- **Hoofd letters** met de methode `IncludesCharacters`, valideert of het wacht woord een hoofd letter bevat.
- **Met de** methode `IncludesCharacters`, wordt gevalideerd of het wacht woord een cijfer bevat.
- **Met de** methode `IncludesCharacters` wordt gecontroleerd of het wacht woord een van de symbool tekens bevat.
- **Pincode** met behulp van de methode `MatchesRegex`, valideert of het wacht woord alleen cijfers bevat.
- **AllowedAADCharacters** met behulp van de methode `MatchesRegex` valideert of het wacht woord alleen een ongeldig teken heeft opgegeven.
- **DisallowedWhitespace** met behulp van de methode `MatchesRegex` valideert of het wacht woord niet begint of eindigt met een spatie.

```XML
<Predicates>
  <Predicate Id="IsLengthBetween8And64" Method="IsLengthRange">
    <UserHelpText>The password must be between 8 and 64 characters.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">8</Parameter>
      <Parameter Id="Maximum">64</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Lowercase" Method="IncludesCharacters">
    <UserHelpText>a lowercase letter</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">a-z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Uppercase" Method="IncludesCharacters">
    <UserHelpText>an uppercase letter</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">A-Z</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Number" Method="IncludesCharacters">
    <UserHelpText>a digit</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">0-9</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="Symbol" Method="IncludesCharacters">
    <UserHelpText>a symbol</UserHelpText>
    <Parameters>
      <Parameter Id="CharacterSet">@#$%^&amp;*\-_+=[]{}|\\:',.?/`~"();!</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="PIN" Method="MatchesRegex">
    <UserHelpText>The password must be numbers only.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">^[0-9]+$</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="AllowedAADCharacters" Method="MatchesRegex">
    <UserHelpText>An invalid character was provided.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">(^([0-9A-Za-z\d@#$%^&amp;*\-_+=[\]{}|\\:',?/`~"();! ]|(\.(?!@)))+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>

  <Predicate Id="DisallowedWhitespace" Method="MatchesRegex">
    <UserHelpText>The password must not begin or end with a whitespace character.</UserHelpText>
    <Parameters>
      <Parameter Id="RegularExpression">(^\S.*\S$)|(^\S+$)|(^$)</Parameter>
    </Parameters>
  </Predicate>
```

Nadat u de basis validaties hebt gedefinieerd, kunt u deze combi neren en een set wachtwoord beleidsregels maken die u in uw beleid kunt gebruiken:

- **SimplePassword** valideert de DisallowedWhitespace, AllowedAADCharacters en IsLengthBetween8And64
- **StrongPassword** valideert de DisallowedWhitespace, AllowedAADCharacters, IsLengthBetween8And64. De laatste groeps `CharacterClasses` voert een extra verzameling predikaten uit met `MatchAtLeast` ingesteld op 3. Het wacht woord van de gebruiker moet tussen 8 en 16 tekens lang zijn en drie van de volgende tekens bevatten: kleine letters, hoofd letters, cijfer of symbool.
- **CustomPassword** valideert alleen DisallowedWhitespace, AllowedAADCharacters. De gebruiker kan dus elke wille keurige lengte opgeven, op voor waarde dat de tekens geldig zijn.

```XML
<PredicateValidations>
  <PredicateValidation Id="SimplePassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="StrongPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
       </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="LengthGroup">
        <PredicateReferences>
          <PredicateReference Id="IsLengthBetween8And64" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="CharacterClasses">
        <UserHelpText>The password must have at least 3 of the following:</UserHelpText>
        <PredicateReferences MatchAtLeast="3">
          <PredicateReference Id="Lowercase" />
          <PredicateReference Id="Uppercase" />
          <PredicateReference Id="Number" />
          <PredicateReference Id="Symbol" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>

  <PredicateValidation Id="CustomPassword">
    <PredicateGroups>
      <PredicateGroup Id="DisallowedWhitespaceGroup">
        <PredicateReferences>
          <PredicateReference Id="DisallowedWhitespace" />
        </PredicateReferences>
      </PredicateGroup>
      <PredicateGroup Id="AllowedAADCharactersGroup">
        <PredicateReferences>
          <PredicateReference Id="AllowedAADCharacters" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Voeg in uw claim type het element **PredicateValidationReference** toe en geef de id op als een van de validaties voor het predicaat, zoals SimplePassword, StrongPassword of CustomPassword.

```XML
<ClaimType Id="password">
  <DisplayName>Password</DisplayName>
  <DataType>string</DataType>
  <AdminHelpText>Enter password</AdminHelpText>
  <UserHelpText>Enter password</UserHelpText>
  <UserInputType>Password</UserInputType>
  <PredicateValidationReference Id="StrongPassword" />
</ClaimType>
```

Hieronder ziet u hoe de elementen worden georganiseerd wanneer Azure AD B2C het fout bericht weergeeft:

![Diagram van het voor beeld van een predikaat en PredicateGroup-wachtwoord complexiteit](./media/predicates/predicates-pass.png)

## <a name="configure-a-date-range"></a>Een datum bereik configureren

Met de **predikaten** en **PredicateValidations** elementen kunt u de minimum-en maximum datum waarden van de **UserInputType** bepalen met behulp van een `DateTimeDropdown`. Als u dit wilt doen, maakt u een **predikaat** met de `IsDateRange` methode en geeft u de minimale en maximale para meters op.

```XML
<Predicates>
  <Predicate Id="DateRange" Method="IsDateRange">
    <UserHelpText>The date must be between 01-01-1980 and today.</UserHelpText>
    <Parameters>
      <Parameter Id="Minimum">1980-01-01</Parameter>
      <Parameter Id="Maximum">Today</Parameter>
    </Parameters>
  </Predicate>
</Predicates>
```

Voeg een **PredicateValidation** toe met een verwijzing naar het `DateRange` predicaat.

```XML
<PredicateValidations>
  <PredicateValidation Id="CustomDateRange">
    <PredicateGroups>
      <PredicateGroup Id="DateRangeGroup">
        <PredicateReferences>
          <PredicateReference Id="DateRange" />
        </PredicateReferences>
      </PredicateGroup>
    </PredicateGroups>
  </PredicateValidation>
</PredicateValidations>
```

Voeg in uw claim type het element **PredicateValidationReference** toe en geef de id op als `CustomDateRange`.

```XML
<ClaimType Id="dateOfBirth">
  <DisplayName>Date of Birth</DisplayName>
  <DataType>date</DataType>
  <AdminHelpText>The user's date of birth.</AdminHelpText>
  <UserHelpText>Your date of birth.</UserHelpText>
  <UserInputType>DateTimeDropdown</UserInputType>
  <PredicateValidationReference Id="CustomDateRange" />
</ClaimType>
 ```
