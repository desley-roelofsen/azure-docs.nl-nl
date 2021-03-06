---
title: Model Hyperparameters afstemmen
titleSuffix: Azure Machine Learning service
description: Meer informatie over het gebruik van de module Tune model Hyper parameters in de Azure Machine Learning-service om een parameter opruiming uit te voeren op een model om de optimale parameter instellingen te bepalen.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: likebupt
ms.author: keli19
ms.date: 10/16/2019
ms.openlocfilehash: 834907528e4062de583197cf91e98372b96451d7
ms.sourcegitcommit: 2d3740e2670ff193f3e031c1e22dcd9e072d3ad9
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/16/2019
ms.locfileid: "74131219"
---
# <a name="tune-model-hyperparameters"></a>Model Hyperparameters afstemmen

In dit artikel wordt beschreven hoe u de module Tune model Hyper parameters gebruikt in Azure Machine Learning Designer (preview). Het doel is om de optimale Hyper parameters te bepalen voor een machine learning model. De module bouwt en test meerdere modellen met behulp van verschillende combi Naties van instellingen. Hiermee worden metrische gegevens van alle modellen vergeleken om de combi Naties van instellingen te verkrijgen. 

De termen *para meter* en *afstemming* kunnen verwarrend zijn. De *para meters* van het model zijn wat u in het deel venster Eigenschappen hebt ingesteld. In principe voert deze module een *opruiming* van de para meters uit op basis van de opgegeven parameter instellingen. Het leert een optimale set _Hyper parameters_, die mogelijk verschillend is voor elke specifieke beslissings structuur, gegevensset of regressie methode. Het proces van het vinden van de optimale configuratie wordt soms *afstemming*genoemd. 

De module ondersteunt de volgende methode voor het vinden van de optimale instellingen voor een model: *geïntegreerde trein en afstemming.* In deze methode configureert u een set para meters die moeten worden gebruikt. U kunt de module vervolgens op meerdere combi Naties laten lopen. De module meet de nauw keurigheid totdat er een ' beste ' model wordt gevonden. Met de meeste leer modules kunt u kiezen welke para meters tijdens het trainings proces moeten worden gewijzigd en wat vast moet blijven.

Afhankelijk van hoe lang het afstemmings proces moet worden uitgevoerd, kunt u besluiten om alle combi Naties uitvoerig te testen. Of u kunt het proces verkorten door een raster van parameter combinaties te maken en een wille keurige subset van het parameter raster te testen.

Met deze methode wordt een getraind model gegenereerd dat u kunt opslaan voor hergebruik.  

> [!TIP] 
> U kunt een gerelateerde taak uitvoeren. Voordat u begint met het afstemmen, moet u de functie selectie Toep assen om de kolommen of variabelen te bepalen die de hoogste gegevens waarde hebben.

## <a name="how-to-configure-tune-model-hyperparameters"></a>Optimalisatie model Hyper parameters configureren  

Voor het leren van de optimale Hyper parameters voor een machine learning model is een aanzienlijk gebruik van pijp lijnen vereist.

### <a name="train-a-model-by-using-a-parameter-sweep"></a>Een model trainen met behulp van een parameter sweep  

In deze sectie wordt beschreven hoe u een eenvoudige para meter-sweep uitvoert, waarmee een model wordt getraind met behulp van de Hyper parameters-module model.

1.  Voeg de module model Hyper parameters afstemmen toe aan uw pijp lijn in de ontwerp functie.

2.  Verbind een niet-traind model met de meest linkse invoer. 

3. Stel de optie **trainer modus maken** in op **parameter bereik**. Gebruik de **opbouw functie voor bereik** om een reeks waarden op te geven die u wilt gebruiken in de para meter sweep.  

    Bijna alle modules voor classificatie en regressie bieden ondersteuning voor het opruimen van geïntegreerde para meters. Voor meer informatie over het configureren van een parameter bereik, kunt u alleen de beschik bare parameter waarden testen.

    U kunt de waarde voor een of meer para meters hand matig instellen en vervolgens de resterende para meters opruimen. Dit kan enige tijd besparen.

4.  Voeg de gegevensset toe die u voor training wilt gebruiken en verbind deze met de middelste invoer van het model Hyper parameters voor het afstemmen.  

    Als u een gecodeerde gegevensset hebt, kunt u deze koppelen aan de meest rechtse invoer poort (**optionele validatie gegevensset**). Zo kunt u de nauw keurigheid meten tijdens de training en het afstemmen.

5.  Kies in het deel venster **Eigenschappen** van stem model Hyper parameters een waarde voor de modus voor het afwijzen van de **para meter**. Met deze optie bepaalt u hoe de para meters worden geselecteerd.

    - **Volledig raster**: wanneer u deze optie selecteert, wordt de module herhaald volgens een raster dat vooraf is gedefinieerd door het systeem, om verschillende combi Naties te proberen en de beste kenniser te identificeren. Deze optie is handig wanneer u niet weet wat de beste parameter instellingen zijn en u alle mogelijke combi Naties van waarden wilt proberen.

    - **Wille keurige sweep**: wanneer u deze optie selecteert, selecteert de module wille keurig parameter waarden boven een door het systeem gedefinieerd bereik. U moet het maximum aantal runs opgeven dat door de module moet worden uitgevoerd. Deze optie is handig als u de prestaties van het model wilt verbeteren door de metrische gegevens van uw keuze te gebruiken, maar nog steeds computer bronnen te besparen.    

6.  Open de kolom kiezer voor de kolom **Label**en kies een kolom met één label.

7.  Kies het aantal uitvoeringen:

    1. **Maximum aantal uitvoeringen op wille keurige sweep**: als u een wille keurige sweep kiest, kunt u opgeven hoe vaak het model moet worden getraind met behulp van een wille keurige combi natie van parameter waarden.

    2. **Maximum aantal uitvoeringen op wille keurig raster**: deze optie bepaalt ook het aantal iteraties voor een wille keurige steek proef van parameter waarden, maar de waarden worden niet wille keurig van het opgegeven bereik gegenereerd. In plaats daarvan maakt de module een matrix van alle mogelijke combi Naties van parameter waarden. Vervolgens neemt het een wille keurige steek proef over de matrix. Deze methode is efficiënter en minder gevoelig voor regionale oversteek proeven of ondersampling.

8.  Voor de **rang schikking**kiest u één metrische waarde voor het classificeren van de modellen.

    Wanneer u een parameter sweep uitvoert, berekent de module alle toepasselijke metrische gegevens voor het model type en retourneert deze in het rapport met **resultaten van opruimen** . In de module worden afzonderlijke metrische gegevens gebruikt voor regressie-en classificatie modellen.

    De metriek die u kiest, bepaalt echter hoe de modellen worden gerangschikt. Alleen het bovenste model, zoals geclassificeerd door de gekozen metriek, wordt uitgevoerd als een getraind model om te gebruiken voor een score.

9.  Voer voor **wille keurige Seed**een getal in dat moet worden gebruikt voor het starten van de parameter sweep. 

10. Voer de pijplijn uit.

## <a name="results-of-hyperparameter-tuning"></a>Resultaten van afstemming tuning

Wanneer de training is voltooid:

+ Als u een set nauw keurige metrische gegevens voor het beste model wilt weer geven, klikt u met de rechter muisknop op de module, selecteert u opschonings **resultaten**en selecteert u vervolgens **visualiseren**.

    De uitvoer bevat alle nauwkeurigheids gegevens die van toepassing zijn op het model type, maar de metrische gegevens die u hebt geselecteerd voor de rang schikking bepalen welk model als ' Best ' wordt beschouwd.

+ Als u het model wilt gebruiken om te scoren in andere pijp lijnen zonder het afstemmings proces te herhalen, klikt u met de rechter muisknop op de model uitvoer en selecteert u **Opslaan als opgeleid model**. 


## <a name="technical-notes"></a>Technische opmerkingen

Deze sectie bevat implementatie details en tips.

### <a name="how-a-parameter-sweep-works"></a>Hoe een para meter sweep werkt

Wanneer u een parameter sweep instelt, definieert u het bereik van uw zoek opdracht. De zoek opdracht kan een eindig aantal para meters gebruiken die wille keurig zijn geselecteerd. Het is ook mogelijk dat een volledige zoek opdracht wordt uitgevoerd op een parameter ruimte die u definieert.

+ **Wille keurige sweep**: met deze optie wordt een model getraind met behulp van een ingesteld aantal herhalingen. 

  U geeft een reeks waarden op die moeten worden herhaald en de module gebruikt een wille keurig gekozen subset van die waarden. Waarden worden gekozen met vervanging, wat betekent dat getallen die eerder zijn gekozen, niet worden verwijderd uit de groep beschik bare getallen. De kans dat een wille keurige waarde wordt geselecteerd, blijft in alle fasen hetzelfde.  

+ **Volledig raster**: de optie voor het gebruik van het hele raster betekent dat elke combi natie wordt getest. Deze optie is het meest uitgebreid, maar de meeste tijd is vereist. 

### <a name="controlling-the-length-and-complexity-of-training"></a>De lengte en complexiteit van training bepalen

Het uitvoeren van meerdere combi Naties van instellingen kan tijdrovend zijn, dus de module biedt verschillende manieren om het proces te beperken:

+ Beperk het aantal iteraties dat wordt gebruikt voor het testen van een model.
+ Beperk de parameter ruimte.
+ Beperk het aantal iteraties en de parameter ruimte.

We raden u aan om met de instellingen een pijp lijn te nemen om de meest efficiënte opleidings methode voor een bepaalde gegevensset en model te bepalen.

### <a name="choosing-an-evaluation-metric"></a>Een evaluatie-metric kiezen

Aan het einde van de test presenteert het model een rapport met de nauw keurigheid voor elk model, zodat u de metrische resultaten kunt bekijken:

- Voor alle binaire classificatie modellen wordt een uniforme set metrische gegevens gebruikt.
- Nauw keurigheid wordt gebruikt voor alle classificatie modellen met meerdere klassen.
- Er wordt een andere set metrische gegevens gebruikt voor regressie modellen. 

Tijdens de training moet u echter *één* metrische waarde kiezen om te gebruiken bij het rangschikken van de modellen die tijdens het afstemmings proces worden gegenereerd. Het kan zijn dat de beste meet waarde varieert, afhankelijk van het probleem van uw bedrijf en de kosten voor fout-positieven en onwaare negatieven.

#### <a name="metrics-used-for-binary-classification"></a>Metrische gegevens die worden gebruikt voor binaire classificatie

-   **Nauw keurigheid** is het aandeel van de werkelijke resultaten tot het totale aantal cases.  

-   **Nauw keurigheid** is het aandeel van de werkelijke resultaten tot positieve resultaten.  

-   **Intrekken** is de Fractie van alle juiste resultaten ten opzichte van alle resultaten.  

-   **F-Score** is een meting die de nauw keurigheid van de precisie en intrekken vergelijkt.  

-   **AUC** is een waarde die het gebied onder de curve vertegenwoordigt wanneer fout-positieven worden getekend op de x-as en waar positieven worden getekend op de y-as.  

-   Het **gemiddelde logboek verlies** is het verschil tussen de twee waarschijnlijke verdelingen: de werkelijke waarde en de eerste in het model.  

#### <a name="metrics-used-for-regression"></a>Metrische gegevens die worden gebruikt voor regressie

-   **Gemiddelde absolute fout** berekent alle fouten in het model, waarbij *fout* de afstand aangeeft van de voorspelde waarde van de waarde True. Het is vaak afgekort tot *Mae*.  

-   **Wortel van kwadraat fout** meet het gemiddelde van de kwadraten van de fouten en neemt vervolgens de hoofdmap van die waarde. Het is vaak afgekort tot *RMSE*.  

-   **Relatieve absolute fout** vertegenwoordigt de fout als een percentage van de waarde True.  

-   **Bij relatieve kwadratische fout** wordt het totale aantal kwadraten genormaliseerd door te delen door het totale aantal gekwadrateerde fouten van de voorspelde waarden.  

-   De **determinatie coëfficiënt** is één getal dat aangeeft hoe goed de gegevens in een model passen. Een waarde van één betekent dat het model exact overeenkomt met de gegevens. De waarde 0 betekent dat de gegevens wille keurig of anderszins niet op het model passen. Dit wordt vaak *r<sup>2</sup>* , *r<sup>2</sup>* of *r-kwadraat*genoemd.  

### <a name="modules-that-dont-support-a-parameter-sweep"></a>Modules die geen para meter-sweep ondersteunen

Bijna alle leer Azure Machine Learning ondersteunen Kruis validatie met een geïntegreerde para meter-sweep, waarmee u de para meters voor pijp lijn kunt kiezen. Als de cursist geen ondersteuning biedt voor het instellen van een reeks waarden, kunt u deze nog steeds gebruiken voor kruis validatie. In dit geval wordt een bereik van toegestane waarden geselecteerd voor de sweep. 


## <a name="next-steps"></a>Volgende stappen

Bekijk de [set beschik bare modules](module-reference.md) voor de Azure machine learning-service. 

