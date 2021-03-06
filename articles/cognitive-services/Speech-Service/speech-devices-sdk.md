---
title: Over de speech-apparaten SDK-Speech-Service
titleSuffix: Azure Cognitive Services
description: Aan de slag met de SDK van de apparaten spraak. De speech-service werkt met een groot aantal verschillende apparaten en audio bronnen. De spraak-apparaten-SDK is een vooraf afgestemd bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 07/05/2019
ms.author: erhopf
ms.openlocfilehash: 7f6793cab3d2d7cc54252bae3a7393f836256bdd
ms.sourcegitcommit: 6c01e4f82e19f9e423c3aaeaf801a29a517e97a0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74815525"
---
# <a name="about-the-speech-devices-sdk"></a>Over de Speech Devices SDK

De [Speech-Service](overview.md) werkt met een groot aantal verschillende apparaten en audio bronnen. Nu kunt u uw spraaktoepassingen-naar een hoger niveau met overeenkomende hardware en software uitvoeren. De spraak-apparaten-SDK is een pretuned bibliotheek die gekoppeld met speciaal ontwikkelde, microfoon SDK in de matrix.

De SDK van de apparaten spraak kan u helpen:

- Snel test nieuwe stem-scenario's.
- Meer de Speech cloud-gebaseerde service voor het eenvoudig integreren in uw apparaat.
- Maak een uitstekende gebruikerservaring voor uw klanten.

De SDK van de apparaten spraak verbruikt de [spraak SDK](speech-sdk.md). De spraak-SDK wordt gebruikt voor het verzenden van de audio die wordt verwerkt door onze geavanceerde verwerking van audio-algoritme van de microfoon-matrix van het apparaat naar de [spraakservice](overview.md). Meerkanaalse audio wordt gebruikt voor nauwkeurigere ver-veld [spraakherkenning](speech-to-text.md) via ruis onderdrukking, echo annulering beamforming en dereverberation.

U kunt ook de speech apparaten SDK gebruiken om omgevings apparaten te bouwen die uw eigen [aangepaste tref woord](speech-devices-sdk-create-kws.md) hebben, zodat de hint die een gebruikers interactie initieert, uniek is voor uw merk.

De SDK voor spraak apparaten vereenvoudigt het maken van een groot aantal scenario's met spraak mogelijkheden, zoals [spraak assistenten](https://aka.ms/bots/speech/va), bestel systemen voor stations, [conversatie transcriptie](conversation-transcription-service.md)en slimme sprekers. U kunt reageren op gebruikers met tekst, spreken terug naar deze in een standaard of [aangepaste gesproken](how-to-customize-voice-font.md), zoekresultaten, bieden [vertalen](speech-translation.md) in andere talen en meer. We kijken ernaar uit om te zien wat u bouwen!

## <a name="get-the-speech-devices-sdk"></a>De Speech Devices SDK ophalen

### <a name="android"></a>Android

Down load voor Android-apparaten de nieuwste versie van de [Android speech-apparaten SDK](https://aka.ms/sdsdk-download-android).

### <a name="windows"></a>Windows

Voor Windows wordt de voorbeeld toepassing meegeleverd als een cross-platform Java-toepassing. Down load de nieuwste versie van de software voor [jre speech-apparaten](https://aka.ms/sdsdk-download-JRE).
De toepassing is gebouwd met het Speech SDK-pakket en de eclips Java IDE (v4) op 64-bits Windows. De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

### <a name="linux"></a>Linux

Voor Linux wordt de voorbeeld toepassing meegeleverd als een cross-platform Java-toepassing. Down load de nieuwste versie van de software voor [jre speech-apparaten](https://aka.ms/sdsdk-download-JRE).
De toepassing is gebouwd met het Speech SDK-pakket en de eclips Java IDE (v4) op 64-bits Linux (Ubuntu 16,04, Ubuntu 18,04, Debian 9). De toepassing wordt uitgevoerd op een 64-bit Java 8 runtime-omgeving (JRE).

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Uw spraak apparaat kiezen](get-speech-devices-sdk.md)
> [!div class="nextstepaction"]
> [Gratis een abonnements sleutel voor een spraak service ophalen](get-started.md)
