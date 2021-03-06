---
title: bestand opnemen
description: bestand opnemen
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 05/25/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: a050ce62f745591608249b41ba56992d8fd35204
ms.sourcegitcommit: a5ebf5026d9967c4c4f92432698cb1f8651c03bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/08/2019
ms.locfileid: "74935923"
---
## <a name="register-extensions"></a>Extensies registreren

Met uitzonde ring van HTTP-en timer-triggers, worden function-bindingen in runtime versie 2. x en hoger geïmplementeerd als uitbreidings pakketten. In versie 2. x en voorbij de Azure Functions runtime moet u de uitbrei dingen expliciet registreren voor de bindings typen die in uw functies worden gebruikt. De uitzonde ringen hierop zijn HTTP-bindingen en timer triggers, waarvoor geen uitbrei dingen zijn vereist.

U kunt ervoor kiezen om bindings uitbreidingen afzonderlijk te installeren of u kunt een referentie voor een uitbreidings bundel toevoegen aan het JSON-project bestand van de host. Met uitbreidings bundels wordt de kans verwijderd om compatibiliteits problemen met het pakket op te lossen wanneer u meerdere bindings typen gebruikt. Het is de aanbevolen methode voor het registreren van bindings uitbreidingen. Extensie bundels verwijdert ook de vereiste van het installeren van de .NET Core 2. x SDK. 

### <a name="extension-bundles"></a>Uitbreidings bundels

[!INCLUDE [Register extensions](functions-extension-bundles.md)]

Zie [Azure functions bindings uitbreidingen registreren](../articles/azure-functions/functions-bindings-register.md#extension-bundles)voor meer informatie. U moet uitbreidings bundels toevoegen aan de host. json voordat u bindingen toevoegt aan het bestand functions. json.

### <a name="register-individual-extensions"></a>Afzonderlijke uitbrei dingen registreren

Als u uitbrei dingen wilt installeren die zich niet in een bundel bevinden, kunt u hand matig afzonderlijke extensie pakketten voor specifieke bindingen registreren. 

> [!NOTE]
> Als u de extensies hand matig wilt registreren met behulp van `func extensions install`, moet u de .NET Core 2. x SDK hebben geïnstalleerd.

Nadat u het bestand *Function. json* hebt bijgewerkt met alle bindingen die uw functie nodig heeft, voert u de volgende opdracht uit in de projectmap.

```bash
func extensions install
```

Met de opdracht leest u het bestand *Function. json* om te zien welke pakketten u nodig hebt, installeert u ze en bouwt u het project extensies opnieuw op. Er worden nieuwe bindingen aan de huidige versie toegevoegd, maar bestaande bindingen worden niet bijgewerkt. Gebruik de optie `--force` om bestaande bindingen bij te werken naar de meest recente versie bij het installeren van nieuwe.