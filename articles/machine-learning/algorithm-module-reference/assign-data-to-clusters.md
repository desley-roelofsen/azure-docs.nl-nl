---
title: 'Gegevens toewijzen aan cluster: module verwijzing'
titleSuffix: Azure Machine Learning
description: Meer informatie over het gebruik van de module gegevens toewijzen aan cluster in Azure Machine Learning voor het cluster model voor scores.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 11/19/2019
ms.openlocfilehash: dd8b23f92f5b24101a7d42ca65f5835a8d4e9ff2
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74214560"
---
# <a name="module-assign-data-to-clusters"></a>Module: gegevens toewijzen aan clusters

In dit artikel wordt beschreven hoe u de module *gegevens toewijzen aan clusters* in azure machine learning Designer (preview) gebruikt. De module genereert voor spellingen via een cluster model dat is getraind met het algoritme *k-means clustering* .

De module gegevens toewijzen aan clusters retourneert een gegevensset die de waarschijnlijke toewijzingen voor elk nieuw gegevens punt bevat. 

## <a name="how-to-use-assign-data-to-clusters"></a>Toewijzing van gegevens aan clusters gebruiken
  
1. Zoek in Azure Machine Learning Designer een eerder getraind cluster model. U kunt een cluster model maken en trainen met behulp van een van de volgende methoden:  
  
    - Configureer de k-means clustering-algoritme met behulp van de [K-betekent cluster](k-means-clustering.md) module en Train het model met behulp van een gegevensset en de module Train clustering model (dit artikel).  
  
    - U kunt ook een bestaand getraind cluster model toevoegen vanuit de groep **opgeslagen modellen** in uw werk ruimte.

2. Koppel het getrainde model aan de linker invoer poort van de **toewijzing van gegevens aan clusters**.  

3. Voeg een nieuwe gegevensset als invoer toe. 

   In deze gegevensset zijn labels optioneel. Over het algemeen is clustering een niet-gecontroleerde leer methode. U wordt niet verwacht dat u de categorieën van tevoren kent. De invoer kolommen moeten echter hetzelfde zijn als de kolommen die zijn gebruikt bij het trainen van het cluster model, of er treedt een fout op.

    > [!TIP]
    > Als u het aantal kolommen wilt beperken dat naar de ontwerp functie wordt geschreven vanuit de cluster voorspellingen, gebruikt u [kolommen selecteren in de gegevensset](select-columns-in-dataset.md)en selecteert u een subset van de kolommen. 
    
4. Schakel het selectie vakje **controleren op toevoegen of uitschakelen voor alleen resultaat alleen** in als u wilt dat de resultaten de volledige invoer gegevensset bevatten, inclusief een kolom waarin de resultaten worden weer gegeven (cluster toewijzingen).
  
    Als u dit selectie vakje uitschakelt, worden alleen de resultaten geretourneerd. Deze optie kan nuttig zijn bij het maken van voor spellingen als onderdeel van een webservice.
  
5.  Voer de pijplijn uit.  
  
### <a name="results"></a>Resultaten

+  Als u de waarden in de gegevensset wilt weer geven, klikt u met de rechter muisknop op de module, selecteert u **resultaat gegevens sets**en selecteert u vervolgens **visualiseren**.

