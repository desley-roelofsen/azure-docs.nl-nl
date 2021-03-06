---
title: De wijzigings feed gebruiken Estimator-Azure Cosmos DB
description: Meer informatie over het gebruik van de Change feed Estimator voor het analyseren van de voortgang van de processor voor wijzigings invoer
author: ealsur
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 08/15/2019
ms.author: maquaran
ms.openlocfilehash: 0023f68400b36b9abd3b9d4a789895e79f67aa03
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/28/2019
ms.locfileid: "70092954"
---
# <a name="use-the-change-feed-estimator"></a>De Estimator van de wijzigings feed gebruiken

In dit artikel wordt beschreven hoe u de voortgang van uw [feeds](./change-feed-processor.md) voor wijzigings instanties kunt bewaken terwijl de wijzigings feed wordt gelezen.

## <a name="why-is-monitoring-progress-important"></a>Waarom is de bewakings voortgang belang rijk?

De wijzigings verwerkings processor fungeert als een pointer die door uw [wijzigings feed](./change-feed.md) doorloopt en de wijzigingen voor de implementatie van een gemachtigde bezorgt. 

De implementatie van de wijzigings feed-processor kan wijzigingen verwerken met een bepaald percentage op basis van de beschik bare resources zoals CPU, geheugen, netwerk, enzovoort.

Als dit percentage langzamer is dan de snelheid waarmee uw wijzigingen worden aangebracht in de Azure Cosmos-container, begint de processor met de vertraging.

Door dit scenario te identificeren, kunt u beter begrijpen of we de implementatie van de wijzigings feed moeten schalen.

## <a name="implement-the-change-feed-estimator"></a>De Estimator voor de wijzigings feed implementeren

Net als bij de [Change feed-processor](./change-feed-processor.md)werkt de Estimator van de wijzigings feed als een push model. In het Estimator wordt het verschil gemeten tussen het laatst verwerkte item (gedefinieerd door de status van de leases-container) en de laatste wijziging in de container, en deze waarde naar een gemachtigde pushen. Het interval waarmee de meting wordt uitgevoerd, kan ook worden aangepast met een standaard waarde van 5 seconden.

Als u bijvoorbeeld de processor voor de wijzigings feed als volgt definieert:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartProcessorEstimator)]

De juiste manier om een Estimator te initialiseren om te meten dat de processor `GetChangeFeedEstimatorBuilder` als zodanig wordt gebruikt:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=StartEstimator)]

Waarbij zowel de processor als de Estimator dezelfde `leaseContainer` en dezelfde naam hebben.

De andere twee para meters zijn de gemachtigde. er wordt dan een getal weer gegeven dat aangeeft hoeveel **wijzigingen in behandeling moeten worden gelezen** door de processor en het tijds interval waarop deze meting moet worden uitgevoerd.

Een voor beeld van een gemachtigde die de schatting ontvangt, is:

[!code-csharp[Main](~/samples-cosmosdb-dotnet-v3/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed/Program.cs?name=EstimationDelegate)]

U kunt deze schatting verzenden naar uw bewakings oplossing en deze gebruiken om te begrijpen hoe de voortgang in de loop van de tijd gedraagt.

> [!NOTE]
> De Estimator van de wijzigings feed hoeft niet te worden geïmplementeerd als onderdeel van de processor voor wijzigings invoer en niet deel uitmaken van hetzelfde project. Het kan onafhankelijk zijn en in een volledig ander exemplaar worden uitgevoerd. U hoeft alleen dezelfde naam en lease configuratie te gebruiken.

## <a name="additional-resources"></a>Aanvullende resources

* [Azure Cosmos DB SDK](sql-api-sdk-dotnet.md)
* [Voor beelden van gebruik op GitHub](https://github.com/Azure/azure-cosmos-dotnet-v3/tree/master/Microsoft.Azure.Cosmos.Samples/Usage/ChangeFeed)
* [Aanvullende voor beelden op GitHub](https://github.com/Azure-Samples/cosmos-dotnet-change-feed-processor)

## <a name="next-steps"></a>Volgende stappen

U kunt nu door gaan met meer informatie over het wijzigen van de feed-processor in de volgende artikelen:

* [Overzicht van de processor voor wijzigings invoer](change-feed-processor.md)
* [Start tijd voor wijzigen van feed-processor](how-to-configure-change-feed-start-time.md)
