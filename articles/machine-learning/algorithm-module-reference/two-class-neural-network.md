---
title: 'Neural Network met twee klassen: Naslag informatie over modules'
titleSuffix: Azure Machine Learning
description: Meer informatie over het gebruik van de Neural-netwerk module met twee klassen in Azure Machine Learning om een Neural-netwerk model te maken dat kan worden gebruikt om een doel te voors pellen met slechts twee waarden.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 10/22/2019
ms.openlocfilehash: d8e27d13de6b1ac6940972022f66811eb75c8986
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73485889"
---
# <a name="two-class-neural-network-module"></a>Neural-netwerk module met twee klassen

In dit artikel wordt een module in Azure Machine Learning Designer (preview) beschreven.

Gebruik deze module voor het maken van een Neural-netwerk model dat kan worden gebruikt voor het voors pellen van een doel met slechts twee waarden.

Classificatie met behulp van Neural-netwerken is een bewaakte leer methode en vereist daarom een *gecodeerde gegevensset*, die een kolom Label bevat. U kunt dit Neural-netwerk model bijvoorbeeld gebruiken om binaire resultaten te voors pellen, zoals of een patiënt al een bepaalde ziekte heeft of een machine waarschijnlijk niet kan worden uitgevoerd binnen een bepaald tijd venster.  

Nadat u het model hebt gedefinieerd, traint u het met een gecodeerde gegevensset en het model als invoer voor het [trainen van modellen](./train-model.md). Het getrainde model kan vervolgens worden gebruikt om waarden voor nieuwe invoer te voors pellen.

### <a name="more-about-neural-networks"></a>Meer informatie over Neural-netwerken

Een Neural-netwerk bestaat uit een reeks onderling verbonden lagen. De invoer is de eerste laag en is verbonden met een uitvoer laag door een acyclische grafiek die bestaat uit gewogen randen en knoop punten.

Tussen de invoer-en uitvoer lagen kunt u meerdere verborgen lagen invoegen. De meeste voorspellende taken kunnen eenvoudig met slechts één of enkele verborgen lagen worden uitgevoerd. Het recente onderzoek heeft echter geleerd dat diepe Neural-netwerken (DNN) met veel lagen effectief kunnen zijn in complexe taken zoals afbeeldingen of spraak herkenning. De opeenvolgende lagen worden gebruikt voor het model leren van de toenemende niveaus van semantische diepte.

De relatie tussen invoer en uitvoer wordt geleerd van het trainen van het Neural-netwerk op de invoer gegevens. De richting van de grafiek loopt van de invoer via de verborgen laag en naar de uitvoer laag. Alle knoop punten in een laag worden verbonden door de gewogen randen op knoop punten in de volgende laag.

Voor het berekenen van de uitvoer van het netwerk voor een bepaalde invoer wordt een waarde berekend op elk knoop punt in de verborgen lagen en in de uitvoer laag. De waarde wordt ingesteld door de gewogen som van de waarden van de knoop punten uit de vorige laag te berekenen. Een activerings functie wordt vervolgens toegepast op die gewogen som.
  
## <a name="how-to-configure"></a>Configureren

1.  Voeg de **Neural-netwerk module met twee klassen** toe aan de pijp lijn. U kunt deze module vinden onder **machine learning**, **initialiseren**, in de categorie **classificatie** .  
  
2.  Geef op hoe u wilt dat het model wordt getraind door de optie **trainer modus maken** in te stellen.  
  
    -   **Eén para meter**: Kies deze optie als u al weet hoe u het model wilt configureren.  

3.  Selecteer bij **verborgen laag specificatie**het type netwerk architectuur dat moet worden gemaakt.  
  
    -   **Volledig gekoppelde Case**: maakt gebruik van de standaard Neural-netwerk architectuur, die als volgt is gedefinieerd voor Neural-netwerken met twee klassen:
  
        -   Heeft één verborgen laag.
  
        -   De uitvoer laag is volledig verbonden met de verborgen laag en de verborgen laag is volledig verbonden met de invoer laag.
  
        -   Het aantal knoop punten in de invoer laag is gelijk aan het aantal functies in de trainings gegevens.
  
        -   Het aantal knoop punten in de verborgen laag wordt ingesteld door de gebruiker. De standaardwaarde is 100.
  
        -   Het aantal knoop punten is gelijk aan het aantal klassen. Voor een Neural-netwerk met twee klassen betekent dit dat alle invoer waarden moeten worden toegewezen aan een van de twee knoop punten in de uitvoer laag.

5.  Voor **leer snelheid**definieert u de grootte van de stap die bij elke herhaling moet worden uitgevoerd, vóór correctie. Een hogere waarde voor het leer tempo kan ertoe leiden dat het model sneller wordt geconvergeerd, maar dat het lokale minima kan overschrijden.

6.  Geef voor **aantal Learning-iteraties**het maximum aantal keren op dat het algoritme de trainings aanvragen moet verwerken.

7.  Voor **de eerste wegings diameter**geeft u de knooppunt gewichten aan het begin van het leer proces op.

8.  Geef voor **de momentum**een gewicht op dat moet worden toegepast tijdens het leren naar knoop punten uit eerdere iteraties  

10. Selecteer de optie **voor beelden in wille keurige** volg orde om cases van elkaar af te plaatsen tussen herhalingen. Als u deze optie uitschakelt, worden cases in precies dezelfde volg orde verwerkt telkens wanneer u de pijp lijn uitvoert.
  
11. Typ voor **wille keurig getal zaad**een waarde die moet worden gebruikt als seed.
  
     Het opgeven van een Seed-waarde is handig als u de Herhaal baarheid wilt garanderen voor uitvoeringen van dezelfde pijp lijn.  Anders wordt een waarde van een systeem klok gebruikt als seed, wat kan leiden tot enigszins verschillende resultaten wanneer u de pijp lijn uitvoert.
  
13. Voeg een gecodeerde gegevensset toe aan de pijp lijn en Verbind een van de [trainings modules](module-reference.md).  
  
    -   Als u de **modus trainer maken** instelt op **één para meter**, gebruikt u de module [Train model](train-model.md) .  
  
14. Voer de pijplijn uit.

## <a name="results"></a>Resultaten

Nadat de training is voltooid:

+ Als u een samen vatting van de para meters van het model wilt weer geven, samen met de functie gewichten die zijn geleerd uit training en andere para meters van het Neural-netwerk, klikt u met de rechter muisknop op de uitvoer van [Train model](./train-model.md)en selecteert u **visualiseren**.  

+ Als u een moment opname van het getrainde model wilt opslaan, klikt u met de rechter muisknop op de **getrainde model** uitvoer en selecteert u **Opslaan als opgeleid model**. Dit model wordt niet bijgewerkt op opeenvolgende uitvoeringen van dezelfde pijp lijn.


## <a name="next-steps"></a>Volgende stappen

Bekijk de [set met modules die beschikbaar zijn](module-reference.md) voor Azure machine learning. 
