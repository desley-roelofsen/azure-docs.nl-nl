---
title: 'Koppelings gegevens: module verwijzing'
titleSuffix: Azure Machine Learning
description: Meer informatie over het gebruik van de module koppeling toevoegen aan Azure Machine Learning voor het samen voegen van gegevens sets.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: peterlu
ms.date: 11/19/2019
ms.openlocfilehash: b07bde671be73af2a351353d9794907972a022e7
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74232626"
---
# <a name="join-data"></a>Gegevens samenvoegen

In dit artikel wordt beschreven hoe u de module voor **samen** voegen in azure machine learning Designer (preview) gebruikt om twee gegevens sets samen te voegen met behulp van een Data Base-stijl-koppelings bewerking.  

## <a name="how-to-configure-join-data"></a>Koppelings gegevens configureren

Als u een koppeling wilt uitvoeren op twee gegevens sets, moeten deze worden gerelateerd aan een sleutel kolom. Samengestelde sleutels met behulp van meerdere kolommen worden ook ondersteund. 

1. Voeg de gegevens sets toe die u wilt combi neren en sleep de module **samen voegen met gegevens** naar de pijp lijn. 

    U kunt de module in de categorie **gegevens transformatie** vinden onder **bewerken**.

1. Verbind de gegevens sets met de module voor **samen voegen** . 
 
1. Selecteer **starten kolom selecteren** om de sleutel kolom (men) te kiezen. Denk eraan dat u kolommen kiest voor de linker-en rechter invoer.

    Voor één sleutel:

    Selecteer één sleutel kolom voor beide invoer.
    
    Voor een samengestelde sleutel:

    Selecteer alle sleutel kolommen van links invoer en rechts invoer in dezelfde volg orde. De module **gegevens samen voegen** voegt de tabellen samen wanneer alle sleutel kolommen overeenkomen. Schakel de optie **dubbele waarden toestaan en kolom volgorde in selectie behouden in** als de volg orde van de kolom niet gelijk is aan de oorspronkelijke tabel. 

    ![kolom-selector](media/module/join-data-column-selector.png)


1. Selecteer de optie **hoofdletter gebruik** als u de hoofdletter gevoeligheid voor een tekst kolom koppeling wilt behouden. 
   
1. Gebruik de vervolg keuzelijst **type samen voegen** om op te geven hoe de gegevens sets moeten worden gecombineerd.  
  
    * **Inner join**: een *inner join* is de meest voorkomende join-bewerking. Het retourneert de gecombineerde rijen alleen wanneer de waarden van de sleutel kolommen overeenkomen.  
  
    * **Left outer join**: een *left outer join* retourneert samengevoegde rijen voor alle rijen uit de linkertabel. Wanneer een rij in de linkertabel geen overeenkomende rijen in de rechter tabel heeft, bevat de geretourneerde rij ontbrekende waarden voor alle kolommen uit de rechter tabel. U kunt ook een vervangings waarde voor ontbrekende waarden opgeven.  
  
    * **Volledige outer join**: een *full outer join* retourneert alle rijen uit de linkertabel (**Tabel1**) en uit de rechter tabel (**tabel2**).  
  
         Voor elk van de rijen in een tabel die geen overeenkomende rijen heeft, bevat het resultaat een rij met ontbrekende waarden.  
  
    * **Linker semi-koppeling**: een *Left semi-koppeling* retourneert alleen de waarden uit de linkertabel wanneer de waarden van de sleutel kolommen overeenkomen.  

1. Voor de optie **behoud de juiste sleutel kolommen in een gekoppelde tabel**:

    * Selecteer deze optie om de sleutels uit beide invoer tabellen weer te geven.
    * Schakel deze optie uit als u de sleutel kolommen alleen wilt retour neren van de invoer links.

1. Voer de pijp lijn uit of selecteer de module samenvoegings gegevens en de geselecteerde **uitvoering** om de koppeling uit te voeren.

1. Als u de resultaten wilt weer geven, klikt u met de rechter muisknop op de **samenvoeg gegevens** > **resultaten gegevensset** > **visualiseren**.

## <a name="next-steps"></a>Volgende stappen

Bekijk de [set met modules die beschikbaar zijn](module-reference.md) voor Azure machine learning. 