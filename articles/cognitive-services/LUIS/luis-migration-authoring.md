---
title: Migreren naar Azure-resource voor ontwerpen
titleSuffix: Azure Cognitive Services
description: Migreer naar een Azure authoring-bron sleutel.
services: cognitive-services
author: diberry
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 12/05/2019
ms.author: diberry
ms.openlocfilehash: 6e1005e3d9c3769de3249f3244d65a656edc963e
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74891742"
---
# <a name="migrate-to-an-azure-resource-authoring-key"></a>Migreren naar een Azure-resource bewerkings sleutel

Language Understanding-ontwerp verificatie (LUIS) is gewijzigd van een e-mail account in een Azure-resource. Als dat nog niet is vereist, wordt het overschakelen naar een Azure-resource in de toekomst afgedwongen.

## <a name="why-migrate"></a>Voordelen van migratie

Met een Azure-resource voor het ontwerpen kunt u, als eigenaar van de resource, de toegang tot ontwerpen beheren. U kunt het maken en benoemen van resources voor het beheren van verschillende groepen auteurs.

Stel dat u de eigenaar bent van twee LUIS-apps en dat u verschillende leden hebt die deel uitmaken van elke app. U kunt twee verschillende ontwerp resources maken en elke app toewijzen aan elke afzonderlijke resource. Wijs elk lid vervolgens als Inzender toe aan de juiste ontwerp bron, afhankelijk van de app waarmee ze samen werken. De Azure-ontwerp bron bepaalt de autorisatie.

> [!Note]
> Vóór de migratie worden mede-auteurs deel _nemers_ genoemd op het niveau van de Luis-app. Na de migratie wordt de Azure-rol van _Inzender_ gebruikt voor dezelfde functionaliteit, maar op het Azure-resource niveau.

## <a name="what-is-migrating"></a>Wat is migratie?

Migratie omvat:

* Alle gebruikers van LUIS, eigen aren en mede werkers.
* **Alle** apps.
* Een **eenrichtings migratie.**

De eigenaar kan geen subset van te migreren apps kiezen en het proces kan niet onomkeerbaar zijn.

De migratie is niet:

* Een proces waarmee samen werkers worden verzameld en automatisch worden verplaatst of toegevoegd aan de Azure-ontwerp bron. Als eigenaar van de app moet u deze stap volt ooien. Voor deze stap zijn machtigingen vereist voor de juiste resource.
* Een proces voor het maken en toewijzen van een Voorspellings runtime-resource. Als u een Voorspellings runtime-resource nodig hebt, is dat [een afzonderlijk proces](luis-how-to-azure-subscription.md#create-resources-in-the-azure-portal) en is deze ongewijzigd.

## <a name="how-are-the-apps-migrating"></a>Hoe worden de apps gemigreerd?

De [Luis-Portal](https://www.luis.ai) biedt het migratie proces.

U wordt gevraagd om te migreren als:

* U hebt apps in het e-mail verificatie systeem voor het ontwerpen.
* En u bent de eigenaar van de app.

U kunt het migratie proces vertragen door het venster af te sluiten. U wordt regel matig gemigreerd voordat u migreert of de deadline voor migratie wordt door gegeven. U kunt het migratie proces starten vanuit het vergrendelings pictogram van de bovenste navigatie balk.

## <a name="migration-for-the-app-owner"></a>Migratie voor de eigenaar van de app

### <a name="before-you-migrate"></a>Voordat u migreert

* **Vereist**, u moet een Azure- [abonnement](https://azure.microsoft.com/free/)hebben. Voor een deel van het abonnements proces zijn facturerings gegevens vereist. U kunt echter de prijs categorie gratis (`F0`) gebruiken wanneer u LUIS gebruikt.
* U **kunt ook**een back-up maken van de apps uit de lijst met apps van de Luis-portal door elke app te exporteren of de export- [API](https://westus.dev.cognitive.microsoft.com/docs/services/5890b47c39e2bb17b84a55ff/operations/5890b47c39e2bb052c5b9c40)te gebruiken.
* U **kunt desgewenst**de collaborator's-lijst van elke app opslaan. Deze e-mail lijst wordt aangeboden als onderdeel van het migratie proces.


Het **ontwerpen van uw Luis-app is gratis**, wat wordt aangegeven door de laag `F0`. Meer informatie [over prijs categorieën](luis-boundaries.md#key-limits).

Als u geen Azure-abonnement hebt, [meldt u zich](https://azure.microsoft.com/free/)aan.

### <a name="migration-steps"></a>Migratiestappen

Volg [deze migratie stappen](luis-migration-authoring-steps.md).

### <a name="after-you-migrate"></a>Nadat u hebt gemigreerd

Na het migratie proces zijn al uw LUIS-apps nu toegewezen aan één LUIS-ontwerp bron.

U kunt meer ontwerp resources maken en toewijzen via de pagina **Manage-> Azure-resources** in de _Luis-Portal_.

U kunt mede werkers toevoegen aan de ontwerp bron vanuit het _Azure Portal_op de pagina **Access Control (IAM)** voor die resource. Zie [toegang tot de Inzender toevoegen](luis-migration-authoring-steps.md#after-the-migration-process-add-contributors-to-your-authoring-resource) voor meer informatie.

|Portal|Doel|
|--|--|
|[Azure](https://azure.microsoft.com/free/)|* Voor spelling-en ontwerp resources maken.<br>* Mede werkers toewijzen.|
|[LUIS](https://www.luis.ai)|* Migreren naar nieuwe ontwerp resources.<br>* Het toewijzen of opheffen van voor spelling-en ontwerp resources voor apps op de pagina **beheer van > Azure-resources** .|

## <a name="migration-for-the-app-contributor"></a>Migratie voor de app-bijdrager

Elke gebruiker van LUIS moet worden gemigreerd, met inbegrip van deel nemers/mede werkers. Een samen werker moet migreren om toegang te hebben tot de app.

> [!Note]
> Als de eigenaar van de LUIS-app is gemigreerd en de samen werker als bijdrager is toegevoegd aan de Azure-resource, heeft de mede werker nog steeds geen toegang tot de app, tenzij deze ook worden gemigreerd.

### <a name="before-the-app-is-migrated"></a>Voordat de app wordt gemigreerd

U kunt ervoor kiezen om een app waarvoor u een samen werker bent te exporteren, en vervolgens de app weer in LUIS te importeren. Het import proces maakt een nieuwe app met een nieuwe app-ID waarvoor u de eigenaar bent.

### <a name="after-the-app-is-migrated"></a>Nadat de app is gemigreerd

De eigenaar van de app moet [uw e-mail adres toevoegen aan de Azure-ontwerp bron als samen werker](luis-how-to-collaborate.md#add-contributor-to-azure-authoring-resource).

Na het migratie proces zijn de apps die u hebt aangeschaft, beschikbaar op de pagina **mijn apps** van de Luis-Portal.

## <a name="troubleshooting"></a>Problemen oplossen

* LUIS-ontwerp sleutels zijn alleen zichtbaar in de LUIS-Portal nadat het migratie proces is voltooid. Als u de ontwerp sleutels maakt, zoals met de LUIS CLI, moet de gebruiker nog steeds het migratie proces volt ooien.
* Als een gemigreerde gebruiker een niet-gemigreerde gebruiker toevoegt als Inzender voor hun Azure-resource, heeft de niet-gemigreerde gebruiker geen toegang tot de apps tenzij ze worden gemigreerd.
* Als een niet-gemigreerde gebruiker geen eigenaar is van een app, maar hij een samen werker is van andere apps die eigendom zijn van anderen en de eigen aren het migratie proces hebben ondergaan, moet deze gebruiker migreren om toegang te hebben tot de apps.
* Als een niet-gemigreerde gebruiker een andere gemigreerde gebruiker aan de app heeft toegevoegd, treedt er een fout op wanneer u een gemigreerde gebruiker niet als een samen werker aan een app kunt toevoegen. De niet-gemigreerde gebruiker moet dan het migratie proces door lopen en een Azure-resource maken en de gemigreerde gebruiker als bijdrager aan die resource toevoegen.

## <a name="next-steps"></a>Volgende stappen

* [Uw app migreren naar een ontwerp bron](luis-migration-authoring-steps.md)
