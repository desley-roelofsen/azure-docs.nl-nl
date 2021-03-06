---
title: 'Snelstartgids: een aangepaste opdracht maken met para meters (preview)-spraak service'
titleSuffix: Azure Cognitive Services
description: In dit artikel voegt u para meters toe aan een toepassing voor aangepaste opdrachten.
services: cognitive-services
author: donkim
manager: yetian
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 12/09/2019
ms.author: donkim
ms.openlocfilehash: 50132593ce3301094ea39546f5661df06a716503
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74976584"
---
# <a name="quickstart-create-a-custom-command-with-parameters-preview"></a>Snelstartgids: een aangepaste opdracht maken met para meters (preview)

In het [vorige artikel](./quickstart-custom-speech-commands-create-new.md)hebben we een nieuw project met aangepaste opdrachten gemaakt om te reageren op opdrachten zonder para meters.

In dit artikel wordt deze toepassing uitgebreid met para meters zodat het mogelijk is om meerdere apparaten in te scha kelen en uit te scha kelen.

## <a name="create-parameters"></a>Para meters maken

1. Open het project dat [we eerder hebben gemaakt](./quickstart-custom-speech-commands-create-new.md)
1. Omdat de opdracht nu wordt verwerkt en uitgeschakeld, wijzigt u de naam van de opdracht in ' TurnOnOff '
   - Beweeg de muis aanwijzer over de naam van de opdracht en selecteer het bewerkings pictogram om de naam te wijzigen
1. Maak een nieuwe para meter om aan te geven of de gebruiker het apparaat wil in-of uitschakelen
   - Selecteer het `+` pictogram naast het gedeelte para meters

   > [!div class="mx-imgBorder"]
   > ![para meter maken](media/custom-speech-commands/create-on-off-parameter.png)

   | Instelling            | Voorgestelde waarde | Beschrijving                                                                                               |
   | ------------------ | --------------- | --------------------------------------------------------------------------------------------------------- |
   | Naam               | OnOff           | Een beschrijvende naam voor uw para meter                                                                     |
   | Is wereld wijd          | Uitgeschakeld       | Selectie vakje dat aangeeft of een waarde voor deze para meter globaal moet worden toegepast op alle opdrachten in het project |
   | Verplicht           | wel         | Selectie vakje dat aangeeft of een waarde voor deze para meter vereist is voordat de opdracht wordt voltooid          |
   | Antwoord sjabloon  | Aan of uit?      | Een prompt om te vragen naar de waarde van deze para meter als deze niet bekend is                                       |
   | Type               | Tekenreeks          | Het type para meter, zoals getal, teken reeks of datum/tijd                                               |
   | Configuratie      | Lijst met teken reeksen     | Voor teken reeksen beperkt een reeks lijst invoer naar een reeks mogelijke waarden                                      |
   | Waarden van de lijst met teken reeksen | aan, uit         | Voor een para meter van een teken reeks lijst, de reeks mogelijke waarden en hun synoniemen                                |

   - Selecteer vervolgens nogmaals het `+` pictogram om een tweede para meter toe te voegen om de naam van de apparaten weer te geven. Voor dit voor beeld is een tv en een ventilator

   | Instelling            | Voorgestelde waarde   | Beschrijving                                                                                               |
   | ------------------ | ----------------- | --------------------------------------------------------------------------------------------------------- |
   | Naam               | SubjectDevice     | Een beschrijvende naam voor uw para meter                                                                     |
   | Is wereld wijd          | Uitgeschakeld         | Selectie vakje dat aangeeft of een waarde voor deze para meter globaal moet worden toegepast op alle opdrachten in het project |
   | Verplicht           | wel           | Selectie vakje dat aangeeft of een waarde voor deze para meter vereist is voordat de opdracht wordt voltooid          |
   | Antwoord sjabloon  | Welk apparaat?     | Een prompt om te vragen naar de waarde van deze para meter als deze niet bekend is                                       |
   | Type               | Tekenreeks            | Het type para meter, zoals getal, teken reeks of datum/tijd                                               |
   | Configuratie      | Lijst met teken reeksen       | Voor teken reeksen beperkt een reeks lijst invoer naar een reeks mogelijke waarden                                      |
   | Waarden van de lijst met teken reeksen | TV, ventilator           | Voor een para meter van een teken reeks lijst, de reeks mogelijke waarden en hun synoniemen                                |
   | Synoniemen (TV)      | televisie, informatie | Optionele synoniemen voor elke mogelijke waarde van een para meter van een teken reeks lijst                                      |

## <a name="add-sample-sentences"></a>Voorbeeld zinnen toevoegen

Met para meters is het handig om voorbeeld zinnen toe te voegen die betrekking hebben op alle mogelijke combi Naties. Bijvoorbeeld:

1. Volledige parameter informatie-`"turn {OnOff} the {SubjectDevice}"`
1. Gedeeltelijke parameter informatie-`"turn it {OnOff}"`
1. Geen parameter informatie-`"turn something"`

Met een voor beeld van zinnen met verschillende hoeveel heden informatie kan de toepassing voor aangepaste opdrachten zowel oplossingen met één afronding als multi-turn oplossen met gedeeltelijke informatie.

In dat doen kunt u de voorbeeld zinnen bewerken om de para meters te gebruiken zoals hieronder wordt voorgesteld.

> [!TIP]
> Gebruik accolades in de voor beelden van de editor om de para meters te raadplegen. - `turn {OnOff} the {SubjectDevice}` gebruik Tab-aanvulling om te verwijzen naar eerder gemaakte para meters.

> [!div class="mx-imgBorder"]
> ![voorbeeld zinnen met para meters](media/custom-speech-commands/create-parameter-sentences.png)

```
turn {OnOff} the {SubjectDevice}
{SubjectDevice} {OnOff}
turn it {OnOff}
turn something {OnOff}
turn something
```

## <a name="add-parameters-to-completion-rule"></a>Para meters toevoegen aan voltooiings regel

Wijzig de voltooiings regel die u hebt gemaakt in [de vorige Snelstartgids](./quickstart-custom-speech-commands-create-new.md):

1. Voeg een nieuwe voor waarde toe en selecteer de vereiste para meter. Selecteer `OnOff` en `SubjectDevice`
1. Bewerk de actie voor spraak reacties om `OnOff` en `SubjectDevice`te gebruiken:

   ```
   Ok, turning {OnOff} the {SubjectDevice}
   ```

## <a name="try-it-out"></a>Probeer het

Open het deel venster chat testen en probeer enkele interacties.

- Invoer: de TV uitschakelen
- Uitvoer: OK, de TV uitschakelen

- Invoer: de televisie uitschakelen
- Uitvoer: OK, de TV uitschakelen

- Invoer: uitschakelen
- Uitvoer: welk apparaat?
- Invoer: de TV
- Uitvoer: OK, de TV uitschakelen

## <a name="next-steps"></a>Volgende stappen
> [!div class="nextstepaction"]
> [Quick Start: verbinding maken met een aangepaste opdracht toepassing met de spraak-SDK (preview)](./quickstart-custom-speech-commands-speech-sdk.md)

