---
title: Slimme detectie-beveiligings detectie pakket met Azure-toepassing inzichten | Microsoft Docs
description: Bewaak de toepassing met Azure-toepassing Insights voor mogelijke beveiligings problemen.
ms.service: azure-monitor
ms.subservice: application-insights
ms.topic: conceptual
author: mrbullwinkle
ms.author: mbullwin
ms.date: 12/12/2017
ms.openlocfilehash: 10c8a38af9e4f04b874bfa75e9e78d241b093117
ms.sourcegitcommit: 8e271271cd8c1434b4254862ef96f52a5a9567fb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/23/2019
ms.locfileid: "72820646"
---
# <a name="application-security-detection-pack-preview"></a>Application Security-detectie pakket (preview-versie)

Application Insights analyseert automatisch de telemetrie die is gegenereerd door uw toepassing en detecteert mogelijke beveiligings problemen. Met deze mogelijkheid kunt u mogelijke beveiligings problemen identificeren en deze afhandelen door de toepassing te corrigeren of door de benodigde beveiligings maatregelen te nemen.

Voor deze functie is geen speciale configuratie vereist, behalve [het configureren van uw app voor het verzenden van telemetrie](https://docs.microsoft.com/azure/application-insights/app-insights-usage-overview).

## <a name="when-would-i-get-this-type-of-smart-detection-notification"></a>Wanneer kan ik dit type Smart Detection-melding krijgen?
Er zijn drie typen beveiligings problemen die worden gedetecteerd:
1. Onveilige URL-toegang: een URL in de toepassing wordt geopend via HTTP en HTTPS. Normaal gesp roken mag een URL die HTTPS-aanvragen accepteert, geen HTTP-aanvragen accepteren. Dit kan duiden op een fout-of beveiligings probleem in uw toepassing.
2. Onbeveiligde vorm: een formulier (of een andere ' POST '-aanvraag) in de toepassing gebruikt HTTP in plaats van HTTPS. Het gebruik van HTTP kan inbreuk maken op de gebruikers gegevens die door het formulier worden verzonden.
3. Verdachte gebruikers activiteit: de toepassing wordt geopend vanuit meerdere landen/regio's door dezelfde gebruiker op ongeveer hetzelfde tijdstip. Bijvoorbeeld: de gebruiker heeft de toepassing geopend vanuit Spanje en de Verenigde Staten binnen hetzelfde uur. Deze detectie duidt op een mogelijk kwaad aardige toegangs poging tot uw toepassing.

## <a name="does-my-app-definitely-have-a-security-issue"></a>Heeft mijn app een beveiligings probleem?
Nee, een melding betekent niet dat uw app over een beveiligings probleem beschikt. Een detectie van de bovenstaande scenario's kan in veel gevallen duiden op een beveiligings probleem. De detectie kan echter een natuurlijke zakelijke rechtvaardiging hebben en kan worden genegeerd.

## <a name="how-do-i-fix-the-insecure-url-access-detection"></a>De detectie van ' onveilige URL-toegang ' Hoe kan ik herstellen?
1. **Sorteren.** De melding bevat het aantal gebruikers dat onbeveiligde Url's heeft geopend en de URL die het meest wordt beïnvloed door onveilige toegang. Dit kan handig zijn bij het toewijzen van een prioriteit aan het probleem.
2. **Ligt.** Welk percentage van de gebruikers heeft onbeveiligde Url's geopend? Hoeveel Url's zijn er betrokken? Deze informatie kan worden opgehaald uit de melding.
3. **Vaststellen.** De detectie biedt de lijst met onveilige aanvragen, evenals de lijsten met Url's en gebruikers die worden beïnvloed, om u te helpen bij het oplossen van het probleem.

## <a name="how-do-i-fix-the-insecure-form-detection"></a>Hoe kan ik de detectie van ' Inveilige formulieren ' oplossen?
1. **Sorteren.** De melding geeft het aantal onveilige formulieren en het aantal gebruikers waarvan de gegevens mogelijk zijn aangetast. Dit kan handig zijn bij het toewijzen van een prioriteit aan het probleem.
2. **Ligt.** Welk formulier is betrokken bij het grootste aantal onveilige verzen dingen en wat is de distributie van onveilige verzen dingen gedurende een bepaalde periode? Deze informatie kan worden opgehaald uit de melding.
3. **Vaststellen.** De detectie biedt de lijst met onveilige formulieren en een uitsplitsing van het aantal onveilige verzen dingen voor elk formulier, om u te helpen bij het oplossen van het probleem.

## <a name="how-do-i-fix-the-suspicious-user-activity-detection"></a>De detectie van verdachte gebruikers activiteiten Hoe kan ik herstellen?
1. **Sorteren.** De melding bevat het aantal verschillende gebruikers dat het verdachte gedrag vertoonde. Dit kan handig zijn bij het toewijzen van een prioriteit aan het probleem.
2. **Ligt.** Van welke landen/regio's zijn de verdachte aanvragen afkomstig? Welke gebruiker was het meest verdacht? Deze informatie kan worden opgehaald uit de melding.
3. **Vaststellen.** De detectie biedt een lijst van verdachte gebruikers en de lijst met landen/regio's voor elke gebruiker, om u te helpen bij het oplossen van het probleem.
