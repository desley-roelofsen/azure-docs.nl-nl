---
title: 'Regressie van beslissings bos: module verwijzing'
titleSuffix: Azure Machine Learning
description: Meer informatie over het gebruik van de Perceptron-module met twee klassen in Azure Machine Learning om een machine learning model te maken op basis van het gemiddelde Perceptron-algoritme.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: 379cddd9654cc897b49fd085d8df55fcd77a7ce8
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73490361"
---
# <a name="two-class-averaged-perceptron-module"></a>Gemiddelde Perceptron-module met twee klassen

In dit artikel wordt een module in Azure Machine Learning Designer (preview) beschreven.

Gebruik deze module om een machine learning model te maken op basis van het gemiddelde Perceptron-algoritme.  
  
Dit classificatie algoritme is een leer methode met toezicht en vereist een *gecodeerde gegevensset*, die een kolom Label bevat. U kunt het model trainen door het model en de gecodeerde gegevensset op te geven als invoer voor het [trainen van modellen](./train-model.md). Het getrainde model kan vervolgens worden gebruikt om waarden te voors pellen voor de nieuwe invoer voorbeelden.  

### <a name="about-averaged-perceptron-models"></a>Over gemiddelde Perceptron-modellen

De *methode gemiddeld Perceptron* is een vroege en eenvoudige versie van een Neural-netwerk. In deze benadering worden de invoer ingedeeld in verschillende mogelijke uitvoer op basis van een lineaire functie en vervolgens gecombineerd met een set gewichten die zijn afgeleid van de functie Vector, dus de naam ' Perceptron '.

De eenvoudiger Perceptron modellen zijn geschikt voor het leren van lineaire opvallende patronen, terwijl Neural netwerken (met name Neural Networks) een complexere klassen grenzen kunnen vormen. Perceptrons zijn echter sneller en omdat de cases serieel worden verwerkt, kan Perceptrons worden gebruikt met doorlopende training.

## <a name="how-to-configure-two-class-averaged-perceptron"></a>Gemiddelde Perceptron configureren voor twee klassen

1.  Voeg de **gemiddelde Perceptron-module met twee klassen** toe aan de pijp lijn.  

2.  Geef op hoe u wilt dat het model wordt getraind door de optie **trainer modus maken** in te stellen.  
  
    -   **Eén para meter**: als u weet hoe u het model wilt configureren, geeft u een specifieke set waarden als argumenten op.
  
3.  Geef voor het **leer tempo**een waarde op voor het *leer tempo*. De waarden voor het leer tempo bepalen de grootte van de stap die wordt gebruikt in stochastische verloop Daal telkens wanneer het model wordt getest en gecorrigeerd.
  
     Door het aantal kleiner te maken, test u het model vaker, met het risico dat u mogelijk vastloopt in een lokaal vaste waarde. Door de stap groter te maken, kunt u sneller convergeren, tegen het risico van een overschrijding van de echte minima.
  
4.  Voor het **maximum aantal herhalingen**typt u het aantal keren dat het algoritme de trainings gegevens moet onderzoeken.  
  
     Als u een vroege regel matig stopt, levert dat betere generalisatie. Het verhogen van het aantal iteraties verbetert de fitting, tegen het risico van overmontage.
  
5.  Voor **wille keurig getal zaad**typt u optioneel een geheel getal dat moet worden gebruikt als seed. Het gebruik van een Seed wordt aanbevolen als u de reproduceer baarheid van de pijp lijn in uitvoeringen wilt garanderen.  
  
1.  Verbind een trainings gegevensset en een van de trainings modules:
  
    -   Als u de **modus trainer maken** instelt op **één para meter**, gebruikt u de module [Train model](train-model.md) .

## <a name="results"></a>Resultaten

Nadat de training is voltooid:

+ Als u een samen vatting van de para meters van het model wilt weer geven, samen met de functie gewichten die zijn geleerd van training, klikt u met de rechter muisknop op de uitvoer van [Train model](./train-model.md).


## <a name="next-steps"></a>Volgende stappen

Bekijk de [set met modules die beschikbaar zijn](module-reference.md) voor Azure machine learning. 