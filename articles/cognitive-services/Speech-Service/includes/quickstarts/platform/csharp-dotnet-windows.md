---
title: 'Snelstartgids: Speech SDK for .NET Framework (Windows) platform Setup-Speech Service'
titleSuffix: Azure Cognitive Services
description: Gebruik deze hand leiding voor het instellen van uw C# platform voor onder .NET Framework voor Windows met de Speech Service SDK.
services: cognitive-services
author: markamos
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: quickstart
ms.date: 10/10/2019
ms.author: erhopf
ms.openlocfilehash: a858a078f8e22a7176fc0eeb09ae0133e2ea11a4
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74818556"
---
In deze hand leiding wordt beschreven hoe u de [Speech SDK](~/articles/cognitive-services/speech-service/speech-sdk.md) voor .NET Framework (Windows) installeert.

[!INCLUDE [License Notice](~/includes/cognitive-services-speech-service-license-notice.md)]

## <a name="prerequisites"></a>Vereisten

Voor deze snelstart zijn de volgende zaken vereist:

* [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/)

## <a name="create-a-visual-studio-project-and-install-the-speech-sdk"></a>Een Visual Studio-project maken en de spraak-SDK installeren

U moet het [Speech SDK NuGet-pakket](https://aka.ms/csspeech/nuget) installeren, zodat u ernaar kunt verwijzen in uw code. Hiervoor moet u mogelijk eerst een **HelloWorld** -project maken. Als u al een project hebt met de werk belasting **.net desktop Development** , kunt u dit project gebruiken en [NuGet Package Manager gebruiken om de Speech SDK te installeren](#use-nuget-package-manager-to-install-the-speech-sdk).

### <a name="create-helloworld-project"></a>Het project HelloWorld maken

1. Open Visual Studio 2019.

1. Selecteer in het Start venster **een nieuw project maken**. 

1. Kies in het venster **een nieuw project maken** de optie **console-app (.NET Framework)** en selecteer vervolgens **volgende**.

1. Voer in het venster **uw nieuwe project configureren** *HelloWorld* in bij **project naam**, kies of maak het mappad op **locatie**en selecteer vervolgens **maken**.

1. Selecteer in de menu balk van Visual Studio **extra** > **Hulpprogram Ma's en functies ophalen**, waarmee Visual Studio Installer wordt geopend en het dialoog venster **wijzigen** wordt weer gegeven.

1. Controleer of de werk belasting van **.net desktop Development** beschikbaar is. Als de werk belasting niet is geïnstalleerd, schakelt u het selectie vakje ernaast in en selecteert u **wijzigen** om de installatie te starten. Het downloaden en installeren kan een paar minuten duren.

   Als het selectie vakje naast **.net desktop Development** al is geselecteerd, selecteert u **sluiten** om het dialoog venster af te sluiten.

   ![.NET-desktopontwikkeling inschakelen](~/articles/cognitive-services/speech-service/media/sdk/vs-enable-net-desktop-workload.png)

1. Sluit Visual Studio Installer.

### <a name="use-nuget-package-manager-to-install-the-speech-sdk"></a>NuGet Package Manager gebruiken voor het installeren van de Speech SDK

1. Klik in de Solution Explorer met de rechter muisknop op het project **HelloWorld** en selecteer vervolgens **NuGet-pakketten beheren** om de NuGet-pakket manager weer te geven.

   ![NuGet Package Manager](~/articles/cognitive-services/speech-service/media/sdk/vs-nuget-package-manager.png)

1. Zoek in de rechter bovenhoek de vervolg keuzelijst **pakket bron** en zorg ervoor dat **`nuget.org`** is geselecteerd.

1. Selecteer in de linkerbovenhoek de optie **Bladeren**.

1. Typ *micro soft. CognitiveServices. speech* in het zoekvak en selecteer **Enter**.

1. Selecteer in de zoek resultaten het pakket **micro soft. CognitiveServices. speech** en selecteer vervolgens **installeren** om de nieuwste stabiele versie te installeren.

   ![Het NuGet-pakket micro soft. CognitiveServices. speech installeren](~/articles/cognitive-services/speech-service/media/sdk/qs-csharp-dotnet-windows-03-nuget-install-1.0.0.png)

1. Accepteer alle overeenkomsten en licenties om de installatie te starten.

   Nadat het pakket is geïnstalleerd, wordt er een bevestiging weer gegeven in het console venster van **Package Manager** .

U kunt nu door gaan naar de [volgende stappen](#next-steps) .

## <a name="next-steps"></a>Volgende stappen

[!INCLUDE [windows](../quickstart-list.md)]
