---
title: Videobestanden streamen met Azure Media Services - .NET | Microsoft Docs
description: Volg de stappen in deze zelf studie om een nieuw Azure Media Services-account te maken, een bestand te coderen en te streamen naar Azure Media Player.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
keywords: azure media services, streamen
ms.service: media-services
ms.workload: media
ms.topic: tutorial
ms.custom: mvc
ms.date: 08/19/2019
ms.author: juliako
ms.openlocfilehash: 7f997865ba33a51c3e3aa7a4c7e990037be9e534
ms.sourcegitcommit: 36e9cbd767b3f12d3524fadc2b50b281458122dc
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/20/2019
ms.locfileid: "69637339"
---
# <a name="tutorial-encode-a-remote-file-based-on-url-and-stream-the-video---net"></a>Zelfstudie: Een extern bestand coderen op basis van URL en de video-.NET streamen

In deze zelf studie leert u hoe eenvoudig het is om Video's te coderen en te streamen op een groot aantal verschillende browsers en apparaten met behulp van Azure Media Services. De invoerinhoud kan worden opgegeven met HTTPS-URL's, SAS-URL's of paden naar bestanden in Azure Blob-opslag.
Met het voorbeeld in dit onderwerp wordt inhoud gecodeerd die u toegankelijk maakt via een HTTPS-URL. Op dit moment biedt AMS v3 geen ondersteuning voor gesegmenteerde overdrachtscodering via HTTPS-URL's.

Aan het einde van de zelf studie kunt u een video streamen.  

![De video afspelen](./media/stream-files-dotnet-quickstart/final-video.png)

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Vereisten

- Als u niet beschikt over Visual Studio, kunt u [Visual Studio Community 2017](https://www.visualstudio.com/thank-you-downloading-visual-studio/?sku=Community&rel=15) downloaden.
- [Een Azure Media Services-account maken](create-account-cli-how-to.md).<br/>Vergeet niet de waarden die u hebt gebruikt voor de namen van de resourcegroep en het Media Services-account.
- Volg de stappen in [Access Azure Media Services API with the Azure CLI](access-api-cli-how-to.md) (Toegang tot de Azure Media Services-API met de Azure CLI) en sla de referenties op. U hebt deze nodig voor toegang tot de API.

## <a name="download-and-configure-the-sample"></a>Het voorbeeld downloaden en configureren

Gebruik de volgende opdracht om een GitHub-opslagplaats te klonen op uw computer die het .NET-voorbeeld voor het streamen van video bevat:  

 ```bash
 git clone https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts.git
 ```

Het voorbeeld bevindt zich in de map [EncodeAndStreamFiles](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/tree/master/AMSV3Quickstarts/EncodeAndStreamFiles).

Open [appsettings.json](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/appsettings.json) in het project dat u hebt gedownload. Vervang de waarden door referenties die u hebt verkregen via [toegang tot API's](access-api-cli-how-to.md).

In het voorbeeld worden de volgende acties uitgevoerd:

1. Er wordt een **transformatie** gemaakt (eerst wordt gecontroleerd of de opgegeven transformatie bestaat). 
2. Er wordt een uitvoer**asset** gemaakt die wordt gebruikt als uitvoer van de coderings**taak**.
3. De invoer van de **taak** wordt gemaakt en is gebaseerd op een HTTPS-URL.
4. De coderings**taak** wordt met behulp van de eerder gemaakte invoer en uitvoer verzonden.
5. De status van de taak wordt gecontroleerd.
6. Er wordt een **streaming-locator** gemaakt.
7. Er worden streaming-URL's samengesteld.

Als u meer wilt weten over de werking van de functies in het voorbeeld, bekijkt u de code en leest u de opmerkingen in [dit bronbestand](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

## <a name="run-the-sample-app"></a>De voorbeeld-app uitvoeren

Wanneer u de app uitvoert, worden URL's weergegeven die kunnen worden gebruikt om de video met verschillende protocollen af te spelen. 

1. Druk op Ctrl + F5 om de toepassing *EncodeAndStreamFiles* uit te voeren.
2. Kies het **HLS**-protocol van Apple (eindigt op *manifest(format=m3u8-aapl)* ) en kopieer de streaming-URL uit de console.

![Output](./media/stream-files-tutorial-with-api/output.png)

In de [broncode](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs) van het voorbeeld kunt u zien hoe de URL is samengesteld. Als u de URL wilt samenstellen, moet u de hostnaam van het streaming-eindpunt en het pad van de streaming-locator samenvoegen.  

## <a name="test-with-azure-media-player"></a>Testen met Azure Media Player

In dit artikel gebruiken we Azure Media Player om de stream te testen. 

> [!NOTE]
> Als een speler wordt gehost op een https-site, moet u de URL bijwerken naar 'https'.

1. Open een browser en ga naar [https://aka.ms/azuremediaplayer/](https://aka.ms/azuremediaplayer/).
2. Plak in het vak **URL:** een van de streaming URL's die u hebt verkregen door het uitvoeren van de toepassing. 
 
     U kunt de URL plakken in de HLS-, Dash-, of Smooth-indeling. Azure Media Player schakelt over op naar een geschikt streaming-protocol zodat de stream automatisch op uw apparaat wordt afgespeeld.
3. Klik op **Update Player**.

Azure Media Player kan worden gebruikt voor testdoeleinden, maar mag niet worden gebruikt in een productieomgeving. 

## <a name="clean-up-resources"></a>Resources opschonen

Als u een van de resources in de resource groep niet meer nodig hebt, met inbegrip van de Media Services-en opslag accounts die u voor deze zelf studie hebt gemaakt, verwijdert u de resource groep.

Voer de volgende CLI-opdracht uit:

```azurecli
az group delete --name amsResourceGroup
```

## <a name="examine-the-code"></a>De code controleren

Als u meer wilt weten over de werking van de functies in het voorbeeld, bekijkt u de code en leest u de opmerkingen in [dit bronbestand](https://github.com/Azure-Samples/media-services-v3-dotnet-quickstarts/blob/master/AMSV3Quickstarts/EncodeAndStreamFiles/Program.cs).

De zelfstudie [Bestanden uploaden, coderen en streamen](stream-files-tutorial-with-api.md) bevat een geavanceerder voorbeeld voor het streamen van bestanden, met een meer gedetailleerde uitleg. 

### <a name="job-error-codes"></a>Foutcodes in taak

Zie [Foutcodes](https://docs.microsoft.com/rest/api/media/jobs/get#joberrorcode).

## <a name="multithreading"></a>Multithreading

De SDK's van Azure Media Services v3 zijn niet thread-safe. Als u werkt met een multi-threaded toepassing, moet u per thread een nieuw AzureMediaServicesClient-object genereren.

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelfstudie: Bestanden uploaden, coderen en streamen](stream-files-tutorial-with-api.md)
