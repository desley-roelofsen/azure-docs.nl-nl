---
title: Problemen met module fouten oplossen
titleSuffix: Azure Machine Learning
description: Problemen met module-uitzonde ringen in Azure Machine Learning Designer oplossen met fout codes
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: reference
author: xiaoharper
ms.author: zhanxia
ms.date: 12/03/2019
ms.openlocfilehash: d491a67a44a2cb01e4968c4d31d20dee4b75a60a
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74980281"
---
# <a name="exceptions-and-error-codes-for-the-designer-preview"></a>Uitzonde ringen en fout codes voor de ontwerp functie (preview-versie)

In dit artikel worden de fout berichten en uitzonderings codes in Azure Machine Learning Designer (preview) beschreven om u te helpen bij het oplossen van problemen met uw machine learning pijp lijnen.

Er zijn twee manieren om de volledige tekst van een fout bericht in de ontwerp functie te verkrijgen:  

- Klik op de koppeling, **Bekijk het uitvoer logboek**in het rechterdeel venster en schuif naar beneden. Het gedetailleerde fout bericht wordt weer gegeven in de laatste twee regels van het venster.  
  
- Selecteer de module met de fout en klik op de rode X. Alleen de relevante fout tekst wordt weer gegeven.

## <a name="error-0001"></a>Fout 0001  
 Uitzonde ring treedt op als een of meer opgegeven kolommen met gegevensset niet zijn gevonden.  

 U ontvangt deze fout melding als er een kolom selectie is gemaakt voor een module, maar de geselecteerde kolom (men) niet aanwezig zijn in de gegevensset voor invoer. Deze fout kan optreden als u hand matig hebt getypt in een kolom naam of als de kolom kiezer een voorgestelde kolom heeft opgegeven die niet in uw gegevensset voor komt toen u de pijp lijn uitvoerde.  

**Oplossing:** Ga terug naar de module met deze uitzonde ring en controleer of de kolom naam of-namen juist zijn en of alle kolommen waarnaar wordt verwezen bestaan.  

|Uitzonderings berichten|
|------------------------|
|Een of meer opgegeven kolommen zijn niet gevonden.|
|Er is geen kolom met de naam of index {column_id} gevonden.|
|De kolom met de naam of index {column_id} bestaat niet in {arg_name_missing_column}.|
|De kolom met de naam of index {column_id} bestaat niet in {arg_name_missing_column}, maar bestaat niet in {arg_name_has_column}.|


## <a name="error-0002"></a>Fout 0002  
 Uitzonde ring treedt op als een of meer para meters niet kunnen worden geparseerd of geconverteerd van een opgegeven type naar vereist door het type doel methode.  

 Deze fout treedt op in Azure Machine Learning wanneer u een para meter opgeeft als invoer en het waardetype verschilt van het verwachte type en er geen impliciete conversie kan worden uitgevoerd.  

**Oplossing:** Controleer de module vereisten en bepaal welk waardetype vereist is (teken reeks, geheel getal, dubbel enzovoort)  

|Uitzonderings berichten|
|------------------------|
|Parseren van para meter is mislukt.|
|Kan para meter {arg_name_or_column} niet parseren.|
|Kan de para meter {arg_name_or_column} niet converteren naar {to_type}.|
|Kan de para meter {arg_name_or_column} niet converteren van {from_type} naar {to_type}.|
|Het converteren van de parameter waarde {arg_name_or_column} van {arg_value} van {from_type} naar {to_type} is mislukt.|
|Kan de waarde {arg_value} in kolom {arg_name_or_column} niet converteren van {from_type} naar {to_type} met het gebruik van de indeling {fmt}.|


## <a name="error-0003"></a>Fout 0003  
 Uitzonde ring treedt op als een of meer invoer waarden null of leeg zijn.  

 Deze fout wordt weer gegeven in Azure Machine Learning als invoer of para meters voor een module null of leeg zijn.  Deze fout kan optreden als u bijvoorbeeld geen waarde voor een para meter hebt getypt. Dit kan ook gebeuren als u een gegevensset hebt gekozen met ontbrekende waarden of een lege gegevensset.  

**Oplossing:**

+ Open de module die de uitzonde ring heeft veroorzaakt en controleer of alle invoer waarden zijn opgegeven. Zorg ervoor dat alle vereiste invoer waarden zijn opgegeven. 
+ Zorg ervoor dat de gegevens die worden geladen vanuit Azure Storage toegankelijk zijn en dat de account naam of-sleutel niet is gewijzigd.  
+ Controleer de invoer gegevens voor ontbrekende waarden of nullen.
+ Als u een query op een gegevens bron gebruikt, controleert u of de gegevens worden geretourneerd in de indeling die u verwacht. 
+ Controleer op typfouten of andere wijzigingen in de specificatie van gegevens.
  
|Uitzonderings berichten|
|------------------------|
|Een of meer invoer waarden zijn null of leeg.|
|De invoer{0}is null of leeg.|


## <a name="error-0004"></a>Fout 0004  
 Uitzonde ring treedt op als de para meter kleiner dan of gelijk aan een specifieke waarde is.  

 Deze fout wordt weer gegeven in Azure Machine Learning als de para meter in het bericht onder een grens waarde valt die vereist is voor de module om de gegevens te verwerken.  

**Oplossing:** Ga naar de module die de uitzonde ring opgooit en wijzig de para meter in een hogere waarde dan de opgegeven waarden.  

|Uitzonderings berichten|
|------------------------|
|De para meter moet groter zijn dan de grens waarde.|
|De waarde van de para meter{0}moet groter zijn dan {1}.|
|De para meter{0}heeft de waarde{1}die groter moet zijn dan {2}.|


## <a name="error-0005"></a>Fout 0005  
 Uitzonde ring treedt op als de para meter kleiner is dan een specifieke waarde.  

 U ontvangt deze fout in Azure Machine Learning als de para meter in het bericht lager is dan of gelijk is aan een grens waarde die vereist is voor de module om de gegevens te verwerken.  

**Oplossing:** Ga naar de module die de uitzonde ring opgooit en wijzig de para meter in groter dan of gelijk aan de opgegeven waarde.  

|Uitzonderings berichten|
|------------------------|
|De para meter moet groter dan of gelijk aan de grens waarde zijn.|
|De waarde voor de para meter {arg_name} moet groter dan of gelijk aan {target_val} zijn.|
|De para meter {arg_name} heeft de waarde {true_val} die groter moet zijn dan of gelijk is aan {target_val}.|


## <a name="error-0006"></a>Fout 0006  
 Uitzonde ring treedt op als de para meter groter is dan of gelijk is aan de opgegeven waarde.  

 Deze fout wordt weer gegeven in Azure Machine Learning als de para meter in het bericht groter is dan of gelijk is aan een grens waarde die vereist is voor de module om de gegevens te verwerken.  

**Oplossing:** Ga naar de module om de uitzonde ring op te treden en wijzig de para meter zo dat deze kleiner is dan de opgegeven waarde.  

|Uitzonderings berichten|
|------------------------|
|De para meters komen niet overeen. Een van de para meters moet kleiner zijn dan de andere.|
|De waarde van de para meter{0}moet kleiner zijn dan de waarde van de para meter{1}.|
|De para meter{0}heeft de waarde{1}die kleiner moet zijn dan {2}.|


## <a name="error-0007"></a>Fout 0007  
 Uitzonde ring treedt op als de para meter groter is dan een specifieke waarde.  

 Deze fout wordt weer gegeven in Azure Machine Learning als u in de eigenschappen van de module een waarde hebt opgegeven die groter is dan is toegestaan. U kunt bijvoorbeeld een gegevens opgeven die zich buiten het bereik van ondersteunde datums bevinden, maar u kunt ook aangeven dat er vijf kolommen moeten worden gebruikt wanneer er slechts drie kolommen beschikbaar zijn. 

 Deze fout kan ook worden weer gegeven als u twee gegevens sets opgeeft die op een of andere manier moeten overeenkomen. Als u bijvoorbeeld de naam van kolommen wijzigt en de kolommen op index opgeeft, moet het aantal namen dat u opgeeft overeenkomen met het aantal kolom indexen. Een ander voor beeld is mogelijk een reken kundige bewerking die gebruikmaakt van twee kolommen, waarbij de kolommen hetzelfde aantal rijen moeten bevatten. 

**Oplossing:**

 + Open de betreffende module en controleer de instellingen van de numerieke eigenschappen.
 + Zorg ervoor dat alle parameter waarden binnen het ondersteunde bereik van waarden voor die eigenschap vallen.
 + Als de module meerdere invoer heeft, moet u ervoor zorgen dat de invoer dezelfde grootte heeft.
<!-- + If the module has multiple properties that can be set, ensure that related properties have appropriate values. For example, when using [Group Data into Bins](group-data-into-bins.md), if you use the option to specify custom bin edges, the number of bins must match the number of values you provide as bin boundaries.-->
 + Controleer of de gegevensset of gegevens bron is gewijzigd. Soms mislukt een waarde die heeft gewerkt met een eerdere versie van de gegevens nadat het aantal kolommen, de kolom gegevens typen of de grootte van de gegevens is gewijzigd.  

|Uitzonderings berichten|
|------------------------|
|De para meters komen niet overeen. Een van de para meters moet kleiner zijn dan of gelijk zijn aan een andere.|
|De waarde van de para meter{0}moet kleiner zijn dan of gelijk zijn aan de waarde van de para meter{1}.|
|De para meter{0}heeft de waarde{1}die kleiner moet zijn dan of gelijk is aan {2}.|


## <a name="error-0008"></a>Fout 0008  
 Uitzonde ring treedt op als de para meter niet binnen het bereik valt.  

 U ontvangt deze fout in Azure Machine Learning als de para meter in het bericht buiten de grenzen valt die vereist zijn voor de module om de gegevens te verwerken.  

 Deze fout wordt bijvoorbeeld weer gegeven als u [rijen toevoegen](add-rows.md) probeert te gebruiken om twee gegevens sets met een verschillend aantal kolommen te combi neren.  

**Oplossing:** Ga naar de module om de uitzonde ring te genereren en wijzig de para meter in het opgegeven bereik.  

|Uitzonderings berichten|
|------------------------|
|De parameter waarde bevindt zich niet in het opgegeven bereik.|
|De waarde voor de para meter {arg_name} valt niet binnen het bereik.|
|De waarde voor de para meter {arg_name} moet in het bereik [{a}, {b}] liggen.|
|De waarde voor de para meter {arg_name} valt niet binnen het bereik. gemotiveerd|


## <a name="error-0009"></a>Fout 0009  
 Uitzonde ring treedt op wanneer de naam van het Azure-opslag account of de container naam onjuist is opgegeven.  

Deze fout treedt op in Azure Machine Learning Designer wanneer u para meters opgeeft voor een Azure Storage-account, maar de naam of het wacht woord niet kan worden omgezet. Fouten met betrekking tot wacht woorden of account namen kunnen om verschillende redenen optreden:

 + Het account is van het verkeerde type. Sommige nieuwe account typen worden niet ondersteund voor gebruik met Machine Learning Designer. Zie [gegevens importeren](import-data.md) voor meer informatie.
 + U hebt de onjuiste account naam opgegeven
 + Het account bestaat niet meer
 + Het wacht woord voor het opslag account is onjuist of gewijzigd
 + U hebt geen container naam opgegeven, of de container bestaat niet
 + U hebt niet volledig het bestandspad opgegeven (pad naar de blob)
   

**Oplossing:**

Dergelijke problemen treden vaak op wanneer u de account naam, het wacht woord of het pad van de container hand matig wilt invoeren. U wordt aangeraden de nieuwe wizard te gebruiken voor de module [gegevens importeren](import-data.md) , waarmee u namen kunt opzoeken en controleren.

Controleer ook of het account, de container of de blob is verwijderd. Gebruik een ander Azure-opslag programma om te controleren of de account naam en het wacht woord juist zijn ingevoerd en of de container bestaat. 

Sommige nieuwere account typen worden niet ondersteund door Azure Machine Learning. De nieuwe opslag typen ' hot ' of ' koud ' kunnen bijvoorbeeld niet worden gebruikt voor machine learning. Zowel klassieke opslag accounts als opslag accounts die zijn gemaakt als "algemeen gebruik" werken prima.

Als het volledige pad naar een blob is opgegeven, controleert u of het pad is opgegeven als **container/blobnaam**en of de container en de BLOB aanwezig zijn in het account.  

 Het pad mag geen voorloop streep bevatten. Bijvoorbeeld **/container/BLOB** is onjuist en moet worden ingevoerd als **container/BLOB**.  


|Uitzonderings berichten|
|------------------------|
|De naam van het Azure-opslag account of de container naam is onjuist.|
|De naam van het Azure-opslag account "{0}" of container naam "{1}" is onjuist; Er wordt een container naam van de indeling container/BLOB verwacht.|


## <a name="error-0010"></a>Fout 0010  
 Uitzonde ring treedt op als invoer gegevens sets kolom namen bevatten die moeten overeenkomen, maar niet.  

 Deze fout wordt weer gegeven in Azure Machine Learning als de kolom index in het bericht andere kolom namen in de twee invoer gegevens sets bevat.  

**Oplossing:** Gebruik [meta gegevens bewerken](edit-metadata.md) of wijzig de oorspronkelijke gegevensset zodat deze dezelfde kolom naam heeft voor de opgegeven kolom index.  

|Uitzonderings berichten|
|------------------------|
|Kolommen met een bijbehorende index in invoer gegevens sets hebben verschillende namen.|
|Kolom namen zijn niet hetzelfde voor kolom {0} (op nul gebaseerd) van invoer gegevens sets ({1} en {2} respectievelijk).|


## <a name="error-0011"></a>Fout 0011  
 Uitzonde ring treedt op als door gegeven kolom set argument niet van toepassing is op een van de gegevensset-kolommen.  

 Deze fout wordt weer gegeven in Azure Machine Learning als de opgegeven kolom selectie niet overeenkomt met een van de kolommen in de opgegeven gegevensset.  

 U kunt deze fout ook ophalen als u geen kolom hebt geselecteerd en ten minste één kolom is vereist om de module te laten werken.  

**Oplossing:** Wijzig de kolom selectie in de module zodat deze van toepassing is op de kolommen in de gegevensset.  

 Als de module vereist dat u een specifieke kolom selecteert, zoals een kolom Label, controleert u of de rechter kolom is geselecteerd.  

 Als er ongepaste kolommen zijn geselecteerd, verwijdert u deze en voert u de pijp lijn opnieuw uit.  

|Uitzonderings berichten|
|------------------------|
|De opgegeven kolomset is niet van toepassing op een van de gegevensset-kolommen.|
|Opgegeven column set "{0}" is niet van toepassing op een van de gegevensset-kolommen.|


## <a name="error-0012"></a>Fout 0012  
 Uitzonde ring treedt op als het exemplaar van de klasse niet kan worden gemaakt met de door gegeven set argumenten.  

**Oplossing:** Deze fout kan niet optreden door de gebruiker en zal in een toekomstige versie worden afgeschaft.  

|Uitzonderings berichten|
|------------------------|
|Een niet-traind model, Train model eerst.|
|Niet-traind model ({arg_name}), gebruik getraind model.|


## <a name="error-0013"></a>Fout 0013  
 Uitzonde ring treedt op als de kenniser die is door gegeven aan de module een ongeldig type is.  

 Deze fout treedt op wanneer een getraind model niet compatibel is met de verbonden Score-module. <!--For example, connecting the output of [Train Matchbox Recommender](train-matchbox-recommender.md) to [Score Model](score-model.md) (instead of [Score Matchbox Recommender](score-matchbox-recommender.md)) will generate this error when the pipeline is run.  -->

**Oplossing:**

Bepaal het type leerer dat wordt geproduceerd door de trainings module en bepaal de Score module die geschikt is voor de kenniser. 

Als het model is getraind met een van de gespecialiseerde trainings modules, koppelt u het getrainde model alleen aan de bijbehorende gespecialiseerde Score module. 


|Modeltype|Trainings module| Score module|
|----|----|----|
|een classificatie|[Model trainen](train-model.md) |[Score model](score-model.md)|
|een regressie model|[Model trainen](train-model.md) |[Score model](score-model.md)|

<!--| clustering models| [Train Clustering Model](train-clustering-model.md) or [Sweep Clustering](sweep-clustering.md)| [Assign Data to Clusters](assign-data-to-clusters.md)|
| anomaly detection - One-Class SVM | [Train Anomaly Detection Model](train-anomaly-detection-model.md) |[Score Model](score-model.md)|
| anomaly detection - PCA |[Train Model](train-model.md) |[Score Model](score-model.md) </br> Some additional steps are required to evaluate the model. |
| anomaly detection - time series|  [Time Series Anomaly Detection](time-series-anomaly-detection.md) |Model trains from data and generates scores. The module does not create a trained learner and no additional scoring is required. |
| recommendation model| [Train Matchbox Recommender](train-matchbox-recommender.md) | [Score Matchbox Recommender](score-matchbox-recommender.md) |
| image classification | [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md) | [Score Model](score-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) | [Score Vowpal Wabbit Version 7-4 Model](score-vowpal-wabbit-version-7-4-model.md) |   
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 7-10 Model](train-vowpal-wabbit-version-7-10-model.md) | [Score Vowpal Wabbit Version 7-10 Model](score-vowpal-wabbit-version-7-10-model.md) |
|Vowpal Wabbit models| [Train Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) | [Score Vowpal Wabbit Version 8 Model](score-vowpal-wabbit-version-8-model.md) |-->

|Uitzonderings berichten|
|------------------------|
|De leerer van een ongeldig type is door gegeven.|
|De leerer {arg_name} heeft een ongeldig type.|
|De leerer {arg_name} heeft een ongeldig type {learner_type}.|


## <a name="error-0014"></a>Fout 0014  
 Uitzonde ring treedt op als het aantal kolom unieke waarden groter is dan is toegestaan.  

 Deze fout treedt op wanneer een kolom te veel unieke waarden bevat.  U ziet deze fout bijvoorbeeld als u opgeeft dat een kolom moet worden verwerkt als categorische-gegevens, maar er zijn te veel unieke waarden in de kolom om te voor komen dat de verwerking is voltooid. Deze fout kan ook worden weer geven als het aantal unieke waarden in twee invoer niet overeenkomt.   

**Oplossing:**

Open de module die de fout heeft gegenereerd en Identificeer de kolommen die als invoer worden gebruikt. Voor sommige modules kunt u met de rechter muisknop op de gegevensset-invoer klikken en **visualiseren** selecteren om statistieken op te halen voor afzonderlijke kolommen, inclusief het aantal unieke waarden en de distributie.

Voor kolommen die u wilt gebruiken voor groepering of categorisatie, neemt u stappen om het aantal unieke waarden in kolommen te verminderen. U kunt op verschillende manieren beperken, afhankelijk van het gegevens type van de kolom. 
<!--
+ For text data, you might be able to use [Preprocess Text](preprocess-text.md) to collapse similar entries. 
+ For numeric data, you can create a smaller number of bins using [Group Data into Bins](group-data-into-bins.md), remove or truncate values using [Clip Values](clip-values.md), or use machine learning methods such as [Principal Component Analysis](principal-component-analysis.md) or [Learning with Counts](data-transformation-learning-with-counts.md) to reduce the dimensionality of the data.  
-->
> [!TIP]
> Kunt u geen oplossing vinden die overeenkomt met uw scenario? U kunt feedback geven in dit onderwerp met de naam van de module die de fout heeft gegenereerd, en het gegevens type en de kardinaliteit van de kolom. We gebruiken de informatie om meer gerichte probleemoplossings stappen te bieden voor veelvoorkomende scenario's.   

|Uitzonderings berichten|
|------------------------|
|De hoeveelheid van de kolom unieke waarden is groter dan toegestaan.|
|Aantal unieke waarden in kolom: {column_name} is groter dan is toegestaan.|
|Aantal unieke waarden in kolom: {column_name} overschrijdt het aantal Tuples van {beperking}.|


## <a name="error-0015"></a>Fout 0015  
 Uitzonde ring treedt op als de database verbinding is mislukt.  

 Deze fout wordt weer gegeven als u een onjuiste SQL-account naam, een wacht woord, een database server of een database naam opgeeft, of als er geen verbinding met de data base kan worden gemaakt vanwege problemen met de data base of server.  

**Oplossing:** Controleer of de account naam, het wacht woord, de database server en de data base correct zijn ingevoerd en of het opgegeven account het juiste machtigings niveau heeft. Controleer of de Data Base op dit moment toegankelijk is.  

|Uitzonderings berichten|
|------------------------|
|Fout bij het maken van de database verbinding.|
|Fout bij het maken van een database verbinding: {0}.|


## <a name="error-0016"></a>Fout 0016  
 Uitzonde ring treedt op als invoer gegevens sets die zijn door gegeven aan de module compatibele kolom typen moeten hebben, maar dat niet.  

 U ontvangt deze fout in Azure Machine Learning als de typen kolommen die in twee of meer gegevens sets zijn door gegeven, niet met elkaar compatibel zijn.  

**Oplossing:** [Meta gegevens bewerken](edit-metadata.md) gebruiken of de oorspronkelijke invoer gegevensset wijzigen<!--, or use [Convert to Dataset](convert-to-dataset.md)--> om ervoor te zorgen dat de typen kolommen compatibel zijn.  

|Uitzonderings berichten|
|------------------------|
|Kolommen met een bijbehorende index in invoer gegevens sets hebben incompatibele typen.|
|De kolommen {first_col_names} zijn incompatibel tussen de trein-en test gegevens.|
|De kolommen {first_col_names} en {second_col_names} zijn incompatibel.|
|Kolom element typen zijn niet compatibel voor kolom {first_col_names} (op nul gebaseerd) van invoer gegevens sets ({first_dataset_names} en {second_dataset_names} respectievelijk).|


## <a name="error-0017"></a>Fout 0017  
 Uitzonde ring treedt op als een geselecteerde kolom een gegevens type gebruikt dat niet door de huidige module wordt ondersteund.  

 U kunt deze fout bijvoorbeeld ontvangen in Azure Machine Learning als uw kolom selectie een kolom bevat met een gegevens type dat niet kan worden verwerkt door de module, zoals een teken reeks kolom voor een wiskundige bewerking, of een score kolom met een categorische-functie kolom Vereist.  

**Oplossing:**
 1. Bepaal de kolom waarin het probleem zich voordoet.
 2. Bekijk de vereisten van de module.
 3. Wijzig de kolom zodat deze voldoet aan de vereisten. U moet mogelijk een aantal van de volgende modules gebruiken om wijzigingen aan te brengen, afhankelijk van de kolom en de conversie die u probeert uit te voeren:
    + Gebruik [meta gegevens bewerken](edit-metadata.md) om het gegevens type van kolommen te wijzigen, of om het kolom gebruik van de functie te wijzigen in numeriek, categorische naar niet-categorische, enzovoort.
<!--    + Use [Convert to Dataset](convert-to-dataset.md) to ensure that all included columns use data types that are supported by Azure Machine Learning.  If you cannot convert the columns, consider removing them from the input dataset.
    + Use the [Apply SQL Transformation](apply-sql-transformation.md) or [Execute R Script](execute-r-script.md) modules to cast or convert any columns that cannot be modified using [Edit Metadata](edit-metadata.md). These modules provide more flexibility for working with datetime data types.
    + For numeric data types, you can use the [Apply Math Operation](apply-math-operation.md) module to round or truncate values, or use the [Clip Values](clip-values.md) module to remove out of range values.  -->
 4. Als laatste redmiddel moet u de oorspronkelijke invoer gegevensset mogelijk wijzigen.

> [!TIP]
> Kunt u geen oplossing vinden die overeenkomt met uw scenario? U kunt feedback geven in dit onderwerp met de naam van de module die de fout heeft gegenereerd, en het gegevens type en de kardinaliteit van de kolom. We gebruiken de informatie om meer gerichte probleemoplossings stappen te bieden voor veelvoorkomende scenario's. 

|Uitzonderings berichten|
|------------------------|
|Kan kolom van het huidige type niet verwerken. Het type wordt niet ondersteund door de module.|
|Kan kolom van het type {col_type} niet verwerken. Het type wordt niet ondersteund door de module.|
|Kan kolom {col_name} van het type {col_type} niet verwerken. Het type wordt niet ondersteund door de module.|
|Kan kolom {col_name} van het type {col_type} niet verwerken. Het type wordt niet ondersteund door de module. Parameter naam: {arg_name}.|


## <a name="error-0018"></a>Fout 0018  
 Uitzonde ring treedt op als de ingevoerde gegevensset ongeldig is.  

**Oplossing:** Deze fout in Azure Machine Learning kan worden weer gegeven in veel contexten, dus er is geen enkele oplossing. In het algemeen geeft de fout aan dat de gegevens die zijn opgegeven als invoer voor een module, een onjuist aantal kolommen hebben of dat het gegevens type niet overeenkomt met de vereisten van de module. Bijvoorbeeld:  

-   Voor de module is een kolom Label vereist, maar er is geen kolom gemarkeerd als label of u hebt nog geen kolom label geselecteerd.  
  
-   De module vereist dat de gegevens categorische zijn, maar dat de gegevens numeriek zijn.  

<!---   The module requires a specific data type. For example, ratings provided to [Train Matchbox Recommender](train-matchbox-recommender.md) can be either numeric or categorical, but cannot be floating point numbers.  -->

-   De gegevens hebben de verkeerde indeling.  
  
-   Geïmporteerde gegevens bevatten ongeldige tekens, ongeldige waarden of waarden buiten het bereik.  
-   De kolom is leeg of bevat te veel ontbrekende waarden.  

 Als u wilt bepalen welke vereisten en hoe uw gegevens mogelijk zijn, raadpleegt u het Help-onderwerp voor de module die de gegevensset gaat gebruiken als invoer.  

 <!--We also recommend that you use [Summarize Data](summarize-data.md) or [Compute Elementary Statistics](compute-elementary-statistics.md) to profile your data, and use these modules to fix metadata and clean values: [Edit Metadata](edit-metadata.md) and [Clean Missing Data](clean-missing-data.md), [Clip Values](clip-values.md)-->.  

|Uitzonderings berichten|
|------------------------|
|De gegevensset is niet geldig.|
|{dataset1} bevat ongeldige gegevens.|
|{dataset1} en {dataset2} moeten consistent zijn columnwise.|
|{dataset1} bevat ongeldige gegevens {reason}.|
|{dataset1} bevat {invalid_data_category}. {troubleshoot_hint}|


## <a name="error-0019"></a>Fout 0019  
 Uitzonde ring treedt op als wordt verwacht dat de kolom gesorteerde waarden bevat, maar dit is niet het geval.  

 Deze fout wordt weer gegeven in Azure Machine Learning als de opgegeven kolom waarden niet in de juiste volg orde staan.  

**Oplossing:** Sorteer de kolom waarden door de invoer gegevensset hand matig te wijzigen en de module opnieuw uit te voeren.  

|Uitzonderings berichten|
|------------------------|
|De waarden in de kolom worden niet gesorteerd.|
|De waarden in de kolom{0}zijn niet gesorteerd.|
|De waarden in de kolom{0}van de gegevensset{1}zijn niet gesorteerd.|


## <a name="error-0020"></a>Fout 0020  
 Uitzonde ring treedt op als het aantal kolommen in sommige gegevens sets die aan de module zijn door gegeven, te klein is.  

 U ontvangt deze fout in Azure Machine Learning als er onvoldoende kolommen zijn geselecteerd voor een module.  

**Oplossing:** Ga terug naar de module en zorg ervoor dat voor de kolom kiezer het juiste aantal kolommen is geselecteerd.  

|Uitzonderings berichten|
|------------------------|
|Het aantal kolommen in de invoer gegevensset is kleiner dan het toegestane minimum.|
|Het aantal kolommen in de invoer gegevensset is kleiner dan het toegestane minimum van {required_columns_count} kolom (men).|
|Het aantal kolommen in de invoer gegevensset {arg_name} is kleiner dan het toegestane minimum van {required_columns_count} kolom (men).|


## <a name="error-0021"></a>Fout 0021  
 Uitzonde ring treedt op als het aantal rijen in een deel van de gegevens sets die aan de module zijn door gegeven, te klein is.  

 Deze fout wordt weer gegeven in Azure Machine Learning wanneer er onvoldoende rijen in de gegevensset zijn om de opgegeven bewerking uit te voeren. U kunt deze fout bijvoorbeeld zien als de gegevensset leeg is of als u een bewerking probeert uit te voeren waarvoor een minimum aantal rijen moet worden opgegeven. Deze bewerkingen kunnen onder meer (maar niet beperkt tot) groeperingen of classificatie zijn gebaseerd op statistische methoden, bepaalde soorten binning en leren met aantallen.  

**Oplossing:**

 + Open de module die de fout heeft geretourneerd en controleer de invoer-gegevensset en module-eigenschappen. 
 + Controleer of de ingevoerde gegevensset niet leeg is en of er voldoende rijen met gegevens zijn om te voldoen aan de vereisten die in de Help van module zijn beschreven.  
 + Als uw gegevens worden geladen vanuit een externe bron, moet u ervoor zorgen dat de gegevens bron beschikbaar is en dat er geen fout is of een wijziging in de definitie van de gegevens die ervoor zorgt dat het import proces minder rijen krijgt.
 + Als u een bewerking uitvoert op de gegevens upstream van de module die van invloed kan zijn op het type gegevens of het aantal waarden, zoals het opschonen, splitsen of samen voegen van bewerkingen, controleert u de uitvoer van deze bewerkingen om het aantal geretourneerde rijen te bepalen.  

|Uitzonderings berichten|
|------------------------|
|Het aantal rijen in de invoer gegevensset is kleiner dan het toegestane minimum.|
|Het aantal rijen in de invoer gegevensset is kleiner dan het toegestane minimum van {required_rows_count} rij (en).|
|Het aantal rijen in de invoer gegevensset is kleiner dan het toegestane minimum van {required_rows_count} rij (en). gemotiveerd|
|Het aantal rijen in de invoer gegevensset {arg_name} is kleiner dan het toegestane minimum van {required_rows_count} rij (en).|
|Het aantal rijen in de invoer gegevensset {arg_name} is {actual_rows_count}, kleiner dan het toegestane minimum van {required_rows_count} rij (en).|
|Het aantal rijen {row_type} in de invoer gegevensset {arg_name} is {actual_rows_count}, kleiner dan het toegestane minimum van {required_rows_count} rij (en).|


## <a name="error-0022"></a>Fout 0022  
 Uitzonde ring treedt op als het aantal geselecteerde kolommen in de invoer-gegevensset niet gelijk is aan het verwachte aantal.  

 Deze fout in Azure Machine Learning kan optreden wanneer de downstream-module of-bewerking een specifiek aantal kolommen of invoer waarden vereist, en u hebt te weinig of te veel kolommen of invoer waarden opgegeven. Bijvoorbeeld:  

-   U geeft een kolom of sleutel kolom met één label op en per ongeluk meerdere kolommen geselecteerd.  
  
-   U wijzigt de naam van kolommen, maar geeft meer of minder namen op dan er kolommen zijn.  
  
-   Het aantal kolommen in de bron of het doel is gewijzigd of komt niet overeen met het aantal kolommen dat door de module wordt gebruikt.  
  
-   U hebt een door komma's gescheiden lijst met waarden voor invoer opgegeven, maar het aantal waarden komt niet overeen, of meerdere invoer wordt niet ondersteund.  

**Oplossing:** Ga naar de module en controleer de kolom selectie om ervoor te zorgen dat het juiste aantal kolommen is geselecteerd. Controleer de uitvoer van upstream-modules en de vereisten van stroomafwaartse bewerkingen.  

 Als u een van de opties voor kolom selectie hebt gebruikt waarmee meerdere kolommen kunnen worden geselecteerd (kolom indexen, alle functies, alle cijfers, enzovoort), moet u het exacte aantal kolommen valideren dat door de selectie wordt geretourneerd.  

 <!--If you are trying to specify a comma-separated list of datasets as inputs to [Unpack Zipped Datasets](unpack-zipped-datasets.md), unpack only one dataset at a time. Multiple inputs are not supported.  -->

 Controleer of het aantal of het type van de upstream-kolommen niet is gewijzigd.  

 Als u een aanbevelings-gegevensset gebruikt voor het trainen van een model, moet u er rekening mee houden dat de aanbevolener een beperkt aantal kolommen verwacht, overeenkomend met gebruikers-en gebruikers items. Verwijder aanvullende kolommen voordat u het model uitoefent of de aanbevelings gegevens sets splitst. Zie [gesplitste gegevens](split-data.md)voor meer informatie.  

|Uitzonderings berichten|
|------------------------|
|Het aantal geselecteerde kolommen in de invoer-gegevensset is niet gelijk aan het verwachte aantal.|
|Het aantal geselecteerde kolommen in de invoer-gegevensset is niet gelijk aan {0}.|
|Het kolom selectie patroon '{0}' bevat het aantal geselecteerde kolommen in de ingevoerde gegevensset die niet gelijk is aan {1}.|
|Het kolom selectie patroon '{0}' wordt verwacht om {1} kolom (men) op te geven die zijn geselecteerd in de invoer gegevensset, maar {2} kolom (men) is/worden opgegeven.|


## <a name="error-0023"></a>Fout 0023  

Uitzonde ring treedt op als de doel kolom van de invoer gegevensset niet geldig is voor de huidige trainer-module.  

Deze fout in Azure Machine Learning treedt op als de doel kolom (zoals geselecteerd in de module parameters) niet van het geldige gegevens type is, alle ontbrekende waarden bevat of niet categorische zoals verwacht.  

**Oplossing:** Ga naar de module-invoer om de inhoud van de kolom Label/doel te controleren. Zorg ervoor dat deze niet alle ontbrekende waarden bevat. Als de module doel kolom verwacht te categorische, moet u ervoor zorgen dat er meer dan één afzonderlijke waarde in de doel kolom is.  

|Uitzonderings berichten|
|------------------------|
|De invoer gegevensset bevat een niet-ondersteunde doel kolom.|
|De invoer gegevensset bevat een niet-ondersteunde doel kolom{0}.|
|De invoer gegevensset bevat een niet-ondersteunde doel kolom{0}voor meer informatie over type {1}.|


## <a name="error-0024"></a>Fout 0024  
Uitzonde ring treedt op als DataSet geen kolom Label bevat.  

 Deze fout in Azure Machine Learning treedt op wanneer de module een label kolom vereist en de DataSet geen kolom label heeft. De evaluatie van een gescoorde gegevensset vereist bijvoorbeeld gewoonlijk dat er een kolom Label aanwezig is voor het berekenen van nauw keurigheid van gegevens.  

Het kan ook gebeuren dat een label kolom aanwezig is in de gegevensset, maar niet correct is gedetecteerd door Azure Machine Learning.

**Oplossing:**

+ Open de module die de fout heeft gegenereerd en controleer of er een label kolom aanwezig is. De naam of het gegevens type van de kolom is niet van belang, op voor waarde dat de kolom een enkel resultaat (of een afhankelijke variabele) bevat die u wilt voors pellen. Als u niet zeker weet welke kolom het label heeft, zoekt u naar een algemene naam, bijvoorbeeld *klasse* of *doel*. 
+  Als de gegevensset geen kolom Label bevat, is het mogelijk dat de kolom Label expliciet of per ongeluk een upstream heeft verwijderd. Het kan ook zijn dat de gegevensset niet de uitvoer van een upstream-Score module is.
+ Als u de kolom expliciet wilt markeren als de kolom Label, voegt u de module [meta gegevens bewerken](edit-metadata.md) toe en verbindt u de gegevensset. Selecteer alleen de kolom Label en selecteer **Label** in de vervolg keuzelijst **velden** . 
+ Als de verkeerde kolom als label is gekozen, kunt u **label wissen** uit de **velden** selecteren om de meta gegevens van de kolom te herstellen. 
  
|Uitzonderings berichten|
|------------------------|
|De gegevensset bevat geen label kolom.|
|Er bevindt zich geen label kolom in {dataset_name}.|


## <a name="error-0025"></a>Fout 0,025  
 Uitzonde ring treedt op als DataSet geen score kolom bevat.  

 Deze fout in Azure Machine Learning treedt op als de invoer voor het Evalueer model geen geldige Score kolommen bevat. De gebruiker probeert bijvoorbeeld een gegevensset te evalueren voordat deze met een juist getraind model werd gescoord of de Score kolom is expliciet verwijderd uit de upstream. Deze uitzonde ring treedt ook op als de Score kolommen op de twee gegevens sets incompatibel zijn. U kunt bijvoorbeeld proberen de nauw keurigheid van een lineaire regressor hierop te vergelijken met een binaire classifier.  

**Oplossing:** Ga naar het Evalueer model en controleer of het een of meer Score kolommen bevat. Als dat niet het geval is, wordt de gegevensset niet beoordeeld of zijn de Score kolommen verwijderd in een upstream-module.  

|Uitzonderings berichten|
|------------------------|
|Gegevensset bevat geen kolom met scores.|
|Er bevindt zich geen kolom met scores in{0}.|
|Er bevindt zich geen score kolom in{0}die wordt geproduceerd door een{1}. Geef de gegevensset een score met het juiste type kenniser.|


## <a name="error-0026"></a>Fout 0026  
 Uitzonde ring treedt op als kolommen met dezelfde naam niet zijn toegestaan.  

 Deze fout in Azure Machine Learning treedt op als meerdere kolommen dezelfde naam hebben. Een van de manieren waarop deze fout kan worden weer gegeven, is als de gegevensset geen koprij heeft en de kolom namen automatisch worden toegewezen: Col0, Kol1, enzovoort.  

**Oplossing:** Als kolommen dezelfde naam hebben, voegt u een module voor het [bewerken van meta gegevens](edit-metadata.md) in tussen de invoer-gegevensset en de module. Gebruik de kolom kiezer in [meta gegevens bewerken](edit-metadata.md) om de kolommen te selecteren waarvan u de naam wilt wijzigen, en typ de nieuwe namen in het tekstvak **nieuwe kolom namen** .  

|Uitzonderings berichten|
|------------------------|
|Er zijn gelijke kolom namen opgegeven in argumenten. Gelijke kolom namen zijn niet toegestaan per module.|
|Gelijke kolom namen in argumenten{0}en{1}zijn niet toegestaan. Geef een andere naam op.|


## <a name="error-0027"></a>Fout 0027  
 Uitzonde ring treedt op wanneer twee objecten van dezelfde grootte moeten zijn, maar niet.  

 Dit is een veelvoorkomende fout in Azure Machine Learning en kan worden veroorzaakt door veel voor waarden.  

**Oplossing:** Er is geen specifieke oplossing. U kunt echter controleren op voor waarden, zoals de volgende:  

-   Als u de naam van kolommen wijzigt, moet u ervoor zorgen dat elke lijst (de invoer kolommen en de lijst met nieuwe namen) hetzelfde aantal items heeft.  
  
-   Als u twee gegevens sets samenvoegt of koppelt, moet u ervoor zorgen dat ze hetzelfde schema hebben.  
  
-   Als u twee gegevens sets met meerdere kolommen samenvoegt, moet u ervoor zorgen dat de sleutel kolommen hetzelfde gegevens type hebben en selecteert u de optie **dubbele waarden toestaan en kolom volgorde behouden in selectie**.  

|Uitzonderings berichten|
|------------------------|
|De grootte van de door gegeven objecten is inconsistent.|
|De grootte van '{0}' is inconsistent met de grootte van '{1}'.|


## <a name="error-0028"></a>Fout 0028  
 Uitzonde ring treedt op in het geval dat kolom sets dubbele kolom namen bevat en niet is toegestaan.  

 Deze fout in Azure Machine Learning treedt op wanneer kolom namen worden gedupliceerd. Dit is niet uniek.  

**Oplossing:** Als er kolommen dezelfde naam hebben, voegt u een exemplaar van de [meta gegevens bewerken](edit-metadata.md) toe tussen de invoer-gegevensset en de module die de fout heeft opgevallen. Gebruik de kolom kiezer in [meta gegevens bewerken](edit-metadata.md) om de kolommen te selecteren waarvan u de naam wilt wijzigen en typ de namen van de nieuwe kolommen in het tekstvak **nieuwe kolom namen** . Als u de naam van meerdere kolommen wijzigt, moet u ervoor zorgen dat de waarden die u in de **nieuwe kolom namen** typt, uniek zijn.  

|Uitzonderings berichten|
|------------------------|
|De kolomset bevat dubbele kolom namen.|
|De naam {duplicated_name} is gedupliceerd.|
|De naam {duplicated_name} is gedupliceerd in {arg_name}.|
|De naam {duplicated_name} is gedupliceerd. Details: {Details}|


## <a name="error-0029"></a>Fout 0029  
 Uitzonde ring treedt op wanneer ongeldige URI is door gegeven.  

 Deze fout in Azure Machine Learning treedt op als er een ongeldige URI is door gegeven.  U ontvangt deze fout melding als aan een van de volgende voor waarden wordt voldaan:  

-   De open bare of SAS-URI voor Azure Blob Storage voor lezen of schrijven bevat een fout.  
  
-   Het tijd venster voor de SA'S is verlopen.  
  
-   De web-URL via de HTTP-bron vertegenwoordigt een bestand of een loop back-URI.  
  
-   De web-URL via HTTP bevat een onjuist ingedeelde URL.  
  
-   De URL kan niet worden omgezet door de externe bron.  

**Oplossing:** Ga naar de module en controleer de indeling van de URI. Als de gegevens bron een web-URL is via HTTP, controleert u of de bedoelde bron geen bestand of een loop back-URI (localhost) is.  

|Uitzonderings berichten|
|------------------------|
|Ongeldige URI is door gegeven.|
|De URI{0}is ongeldig.|


## <a name="error-0030"></a>Fout 0030  
 Uitzonde ring treedt op wanneer het niet mogelijk is om een bestand te downloaden.  

 Deze uitzonde ring in Azure Machine Learning treedt op wanneer het niet mogelijk is om een bestand te downloaden. Deze uitzonde ring wordt weer gegeven wanneer een poging tot het lezen van een HTTP-bron is mislukt na drie (3) pogingen.  

**Oplossing:** Controleer of de URI naar de HTTP-bron juist is en of de site momenteel toegankelijk is via internet.  

|Uitzonderings berichten|
|------------------------|
|Kan een bestand niet downloaden.|
|Fout bij het downloaden van het bestand: {0}.|


## <a name="error-0031"></a>Fout 0031  
 Uitzonde ring treedt op als het aantal kolommen in de kolomset kleiner is dan nodig is.  

 Deze fout in Azure Machine Learning treedt op als het aantal geselecteerde kolommen kleiner is dan nodig is.  Deze fout wordt weer gegeven als het minimale vereiste aantal kolommen niet is geselecteerd.  

**Oplossing:** Voeg extra kolommen aan de kolom selectie toe met behulp van de **kolom kiezer**.  

|Uitzonderings berichten|
|------------------------|
|Het aantal kolommen in de kolomset is kleiner dan vereist.|
|U moet mini maal {0} kolom (men) opgeven. Het werkelijke aantal opgegeven kolommen is {1}.|


## <a name="error-0032"></a>Fout 0032  
 Uitzonde ring treedt op als argument geen getal is.  

 Deze fout wordt weer gegeven in Azure Machine Learning als het argument een double of NaN is.  

**Oplossing:** Wijzig het opgegeven argument om een geldige waarde te gebruiken.  

|Uitzonderings berichten|
|------------------------|
|Het argument is geen getal.|
|'{0}' is geen getal.|


## <a name="error-0033"></a>Fout 0033  
 Uitzonde ring treedt op als argument oneindig is.  

 Deze fout in Azure Machine Learning treedt op als het argument oneindig is. Deze fout treedt op als het argument `double.NegativeInfinity` of `double.PositiveInfinity`is.  

**Oplossing:** Wijzig het opgegeven argument in een geldige waarde.  

|Uitzonderings berichten|
|------------------------|
|Argument moet eindig zijn.|
|'{0}' is niet eindig.|


## <a name="error-0034"></a>Fout 0034  
 Uitzonde ring treedt op als er meer dan één beoordeling bestaat voor een gegeven combi natie van gebruikers items.  

 Deze fout in Azure Machine Learning treedt op als een gebruiker-item paar meer dan één classificatie waarde heeft.  

**Oplossing:** Zorg ervoor dat het gebruikers-item paar slechts één waarderings waarde bezit.  

|Uitzonderings berichten|
|------------------------|
|Er bestaat meer dan één classificatie voor de waarde (n) in de gegevensset.|
|Meer dan één classificatie voor de gebruiker {user} en item {item} in de Voorspellings gegevens tabel.|
|Meer dan één classificatie voor gebruiker {user} en item {item} in {DataSet}.|


## <a name="error-0035"></a>Fout 0035  
 Uitzonde ring treedt op als er geen functies voor een gegeven gebruiker of item zijn opgegeven.  

 Deze fout in Azure Machine Learning treedt op wanneer u een aanbevelings model wilt gebruiken om te scoren, maar er geen functie Vector kan worden gevonden.  

**Oplossing:**

De matchbox-aanbeveling heeft bepaalde vereisten waaraan moet worden voldaan wanneer u de onderdelen van het item of de gebruikers functies gebruikt.  Deze fout geeft aan dat er een functie Vector ontbreekt voor een gebruiker of een item dat u als invoer hebt opgegeven. Zorg ervoor dat er een vector met functies beschikbaar is in de gegevens voor elke gebruiker of elk item.  

 Als u bijvoorbeeld een aanbevelings model hebt getraind met behulp van functies zoals de leeftijd, locatie of baten van de gebruiker, maar nu ook scores wilt maken voor nieuwe gebruikers die tijdens de training niet werden gezien, moet u een vergelijk bare set functies opgeven (te weten, leeftijd, locatie en inkomsten waarden) voor de nieuwe gebruikers om passende voor spellingen te maken. 

 Als u geen functies voor deze gebruikers hebt, kunt u de functie techniek gebruiken om de juiste functies te genereren.  Als u bijvoorbeeld geen afzonderlijke gebruikers leeftijd of inkomsten hebt, kunt u geschatte waarden genereren om te gebruiken voor een groep gebruikers. 

<!--When you are scoring from a recommendation mode, you can use item or user features only if you previously used item or user features during training. For more information, see [Score Matchbox Recommender](score-matchbox-recommender.md).

For general information about how the Matchbox recommendation algorithm works, and how to prepare a dataset of item features or user features, see [Train Matchbox Recommender](train-matchbox-recommender.md).  -->

 > [!TIP]
 > De oplossing is niet van toepassing op uw aanvraag? U kunt feedback over dit artikel verzenden en informatie geven over het scenario, met inbegrip van de module en het aantal rijen in de kolom. Deze informatie wordt gebruikt om in de toekomst meer gedetailleerde stappen voor probleem oplossing te bieden.

|Uitzonderings berichten|
|------------------------|
|Er zijn geen functies opgegeven voor een vereiste gebruiker of een vereist item.|
|Functies voor {0} vereist, maar niet opgegeven.|


## <a name="error-0036"></a>Fout 0036  
 Uitzonde ring treedt op als er meerdere functie vectoren voor een bepaalde gebruiker of een item zijn opgegeven.  

 Deze fout in Azure Machine Learning treedt op als een functie Vector meermaals is gedefinieerd.  

**Oplossing:** Zorg ervoor dat de functie Vector niet meer dan één keer is gedefinieerd.  

|Uitzonderings berichten|
|------------------------|
|Dubbele functie definitie voor een gebruiker of een item.|
|Dubbele functie definitie voor {0}.|


## <a name="error-0037"></a>Fout 0037  
 Uitzonde ring treedt op als er meerdere label kolommen zijn opgegeven en er slechts één is toegestaan.  

 Deze fout in Azure Machine Learning treedt op als er meer dan één kolom is geselecteerd als de nieuwe label kolom. Voor de meeste bewaakte leer algoritmen moet één kolom worden gemarkeerd als het doel of label.  

**Oplossing:** Zorg ervoor dat u één kolom selecteert als de nieuwe label kolom.  

|Uitzonderings berichten|
|------------------------|
|Er zijn meerdere label kolommen opgegeven.|
|Er zijn meerdere label kolommen opgegeven in {dataset_name}.|


## <a name="error-0039"></a>Fout 0039  
 Uitzonde ring treedt op als een bewerking is mislukt.  

 Deze fout in Azure Machine Learning treedt op wanneer een interne bewerking niet kan worden voltooid.  

**Oplossing:** Deze fout wordt veroorzaakt door veel omstandigheden en er is geen specifiek verhaal.  
 De volgende tabel bevat algemene berichten voor deze fout, gevolgd door een specifieke beschrijving van de voor waarde. 

 Als er geen details beschikbaar zijn, [verzendt u feedback](https://social.msdn.microsoft.com/forums/azure/home?forum=MachineLearning) en geeft u informatie op over de modules die de fout hebben gegenereerd en de bijbehorende voor waarden.

|Uitzonderings berichten|
|------------------------|
|De bewerking is mislukt.|
|Fout bij het volt ooien van de bewerking: {failed_operation}.|
|Fout bij het volt ooien van de bewerking: {failed_operation}. Reden: {reason}.|


## <a name="error-0042"></a>Fout 0042  
 Uitzonde ring treedt op wanneer het niet mogelijk is om een kolom naar een ander type te converteren.  

 Deze fout in Azure Machine Learning treedt op wanneer het niet mogelijk is om een kolom te converteren naar het opgegeven type.  U ontvangt deze fout melding als een module een bepaald gegevens type vereist, zoals datetime, Text, een getal met een drijvende komma of een geheel getal, maar het is niet mogelijk om een bestaande kolom te converteren naar het vereiste type.  

U kunt bijvoorbeeld een kolom selecteren en proberen deze te converteren naar een numeriek gegevens type voor gebruik in een reken kundige bewerking. deze fout wordt opgehaald als de kolom ongeldige gegevens bevat. 

Een andere reden hiervoor is dat u deze fout kunt krijgen als u een kolom met drijvende-komma getallen of een groot aantal unieke waarden wilt gebruiken als een categorische kolom. 

**Oplossing:**

+ Open de Help-pagina voor de module die de fout heeft gegenereerd en controleer de vereisten voor het gegevens type.
+ Bekijk de gegevens typen van de kolommen in de invoer gegevensset.
+ Inspecteer gegevens die afkomstig zijn van zogenaamde schema-less-gegevens bronnen.
+ Controleer de gegevensset op ontbrekende waarden of speciale tekens die de conversie naar het gewenste gegevens type kunnen blok keren. 
    + Numerieke gegevens typen moeten consistent zijn: Controleer bijvoorbeeld op drijvende-komma getallen in een kolom met gehele getallen.
    + Zoek naar tekst reeksen of N.V.T. waarden in een kolom met getallen. 
    + Booleaanse waarden kunnen worden geconverteerd naar een geschikte weer gave, afhankelijk van het vereiste gegevens type.
    + Tekst kolommen voor niet-Unicode-tekens, tabtekens of besturings tekens controleren
    + Datetime-gegevens moeten consistent zijn om model fouten te voor komen, maar opschonen kan ingewikkeld zijn vanwege de vele notaties. Overweeg het gebruik van <!--the [Execute R Script](execute-r-script.md) or -->[Voer python-script](execute-python-script.md) modules uit om opschoning uit te voeren.  
+ Wijzig indien nodig de waarden in de invoer-gegevensset zodat de kolom kan worden geconverteerd. Aanpassing kan binning, afkap ping of Afrondings bewerkingen, eliminatie van uitschieters of toerekening van ontbrekende waarden bevatten. Raadpleeg de volgende artikelen voor enkele veelvoorkomende scenario's voor gegevens transformatie in machine learning:
    + [Ontbrekende gegevens opschonen](clean-missing-data.md)
    + [Gegevens normaliseren](normalize-data.md)
<!--+ [Clip Values](clip-values.md) 
    + [Group Data Into Bins](group-data-into-bins.md)
  -->

> [!TIP]
> Is de oplossing onduidelijk of niet van toepassing op uw aanvraag? U kunt feedback over dit artikel verzenden en informatie geven over het scenario, met inbegrip van de module en het gegevens type van de kolom. Deze informatie wordt gebruikt om in de toekomst meer gedetailleerde stappen voor probleem oplossing te bieden.  

|Uitzonderings berichten|
|------------------------|
|Conversie niet toegestaan.|
|Kan kolom van het type {0} niet converteren naar een kolom van het type {1}.|
|Kan kolom{2}van het type {0} niet converteren naar een kolom van het type {1}.|
|Kan kolom{2}van het type {0} niet converteren naar kolom{3}van het type {1}.|


## <a name="error-0044"></a>Fout 0044  
 Uitzonde ring treedt op wanneer het niet mogelijk is om element type van kolom van de bestaande waarden af te leiden.  

 Deze fout in Azure Machine Learning treedt op wanneer het niet mogelijk is om het type van een kolom of kolommen in een gegevensset af te leiden. Dit gebeurt meestal bij het samen voegen van twee of meer gegevens sets met verschillende element typen. Als Azure Machine Learning geen gemeen schappelijk type kunt bepalen dat alle waarden in een kolom of kolommen kan vertegenwoordigen zonder gegevens verlies, wordt deze fout gegenereerd.  

**Oplossing:** Zorg ervoor dat alle waarden in een bepaalde kolom in beide gegevens sets die worden gecombineerd, van hetzelfde type zijn (numeriek, Booleaans, categorische, teken reeks, datum, enz.) of naar hetzelfde type kunnen worden gedwongen.  

|Uitzonderings berichten|
|------------------------|
|Element type van de kolom kan niet worden afgeleid.|
|Kan element type niet afleiden voor kolom{0}--alle elementen zijn null-verwijzingen.|
|Kan element type niet afleiden voor kolom{0}van gegevensset{1}--alle elementen zijn null-verwijzingen.|


## <a name="error-0045"></a>Fout 0045  
 Uitzonde ring treedt op wanneer het niet mogelijk is om een kolom te maken vanwege gemengde element typen in de bron.  

 Deze fout in Azure Machine Learning wordt gemaakt wanneer de element typen van twee gegevens sets die worden gecombineerd, verschillend zijn.  

**Oplossing:** Zorg ervoor dat alle waarden in een bepaalde kolom in beide gegevens sets worden gecombineerd van hetzelfde type (numeriek, Booleaans, categorische, teken reeks, datum, enzovoort).  

|Uitzonderings berichten|
|------------------------|
|Kan geen kolom met gemengde element typen maken.|
|Kan geen kolom maken met de id {column_id} van gemengde element typen: \ n\tType van gegevens [{row_1}, {column_id}] is {type_1}. Het type gegevens [{row_2}, {column_id}] is {type_2}.|
|Kan geen kolom maken met de id {column_id} van gemengde element typen: \ n\tType in segment {chunk_id_1} is {type_1}. Het type in segment {chunk_id_2} is {type_2} met chunk grootte: {chunk_size}.|


## <a name="error-0046"></a>Fout 0046  
 Uitzonde ring treedt op wanneer het niet mogelijk is om een map te maken op het opgegeven pad.  

 Deze fout in Azure Machine Learning treedt op wanneer het niet mogelijk is om een map te maken op het opgegeven pad. Deze fout wordt weer gegeven als een deel van het pad naar de uitvoermap voor een Hive-query onjuist of niet toegankelijk is.  

**Oplossing:** Ga naar de module en controleer of het mappad juist is ingedeeld en of het toegankelijk is met de huidige referenties.  

|Uitzonderings berichten|
|------------------------|
|Geef een geldige uitvoermap op.|
|Map: {0} kan niet worden gemaakt. Geef een geldig pad op.|


## <a name="error-0047"></a>Fout 0047  
 Uitzonde ring treedt op als aantal functie kolommen in sommige gegevens sets die zijn door gegeven aan de module te klein is.  

 Deze fout in Azure Machine Learning treedt op als de invoer-gegevensset voor de training niet het minimale aantal kolommen bevat dat vereist is voor de algoritme. De gegevensset is meestal leeg of bevat alleen trainings kolommen.  

**Oplossing:** Ga naar de invoer gegevensset om ervoor te zorgen dat er een of meer extra kolommen van de kolom Label zijn.  

|Uitzonderings berichten|
|------------------------|
|Het aantal functie kolommen in de invoer gegevensset is lager dan het toegestane minimum.|
|Het aantal functie kolommen in de invoer gegevensset is kleiner dan het toegestane minimum van {required_columns_count} kolom (men).|
|Het aantal functie kolommen in de invoer gegevensset {arg_name} is kleiner dan het toegestane minimum van {required_columns_count} kolom (men).|


## <a name="error-0048"></a>Fout 0048  
 Uitzonde ring treedt op wanneer het niet mogelijk is om een bestand te openen.  

 Deze fout in Azure Machine Learning treedt op wanneer het niet mogelijk is om een bestand te openen voor lezen of schrijven. Deze fout kan om de volgende redenen worden weer gegeven:  

-   De container of het bestand (BLOB) bestaat niet  
  
-   Het toegangs niveau van het bestand of de container biedt geen toegang tot het bestand  
  
-   Het bestand is te groot om te lezen of heeft een onjuiste indeling  

**Oplossing:** Ga naar de module en het bestand dat u probeert te lezen.  

 Controleer of de namen van de container en het bestand juist zijn.  

 Gebruik de klassieke Azure-portal of een Azure-opslag programma om te controleren of u gemachtigd bent om toegang te krijgen tot het bestand.  

  <!--If you are trying to read an image file, make sure that it meets the requirements for image files in terms of size, number of pixels, and so forth. For more information, see [Import Images](import-images.md).  -->

|Uitzonderings berichten|
|------------------------|
|Kan geen bestand openen.|
|Fout bij het openen van het bestand: {0}.|
|Fout bij het openen van het bestand: {0}. Uitzonderings bericht van de opslag: {1}.|


## <a name="error-0049"></a>Fout 0049  
 Uitzonde ring treedt op wanneer het niet mogelijk is om een bestand te parseren.  

 Deze fout in Azure Machine Learning treedt op wanneer het niet mogelijk is om een bestand te parseren. U ontvangt deze fout melding als de bestands indeling die is geselecteerd in de module [gegevens importeren](import-data.md) niet overeenkomt met de daad werkelijke indeling van het bestand, of als het bestand een onherkenbaar teken bevat.  

**Oplossing:** Ga naar de module en corrigeer de selectie van de bestands indeling als deze niet overeenkomt met de indeling van het bestand. Controleer, indien mogelijk, het bestand om te bevestigen dat het geen ongeldige tekens bevat.  

|Uitzonderings berichten|
|------------------------|
|Een bestand kan niet worden geparseerd.|
|Fout bij het parseren van het bestand {file_format}.|
|Er is een fout opgetreden tijdens het parseren van het bestand {file_format}: {file_name}.|
|Fout bij het parseren van het bestand {file_format}. Reden: {failure_reason}.|
|Er is een fout opgetreden tijdens het parseren van het bestand {file_format}: {file_name}. Reden: {failure_reason}.|


## <a name="error-0052"></a>Fout 0052  
 Uitzonde ring treedt op als de Azure Storage-account sleutel onjuist is opgegeven.  

 Deze fout in Azure Machine Learning treedt op als de sleutel die wordt gebruikt voor toegang tot het Azure Storage-account onjuist is. U kunt deze fout bijvoorbeeld zien als de Azure-opslag sleutel is afgekapt bij het kopiëren en plakken of als de verkeerde sleutel is gebruikt.  

 Zie [toegangs sleutels voor opslag weer geven, kopiëren en opnieuw genereren](https://azure.microsoft.com/documentation/articles/storage-create-storage-account-classic-portal/)voor meer informatie over het ophalen van de sleutel voor een Azure-opslag account.  

**Oplossing:** Ga naar de module en controleer of de Azure Storage-sleutel juist is voor het account. Kopieer de sleutel opnieuw vanuit de klassieke Azure-Portal, indien nodig.  

|Uitzonderings berichten|
|------------------------|
|De sleutel van het Azure-opslag account is onjuist.|


## <a name="error-0053"></a>Fout 0053  
 Uitzonde ring treedt op wanneer er geen gebruikers functies of items zijn voor matchbox-aanbevelingen.  

 Deze fout in Azure Machine Learning wordt gemaakt wanneer een functie Vector niet kan worden gevonden.  

**Oplossing:** Zorg ervoor dat er een functie Vector aanwezig is in de ingevoerde gegevensset.  

|Uitzonderings berichten|
|------------------------|
|Gebruikers onderdelen of/en items zijn vereist, maar niet opgegeven.|


## <a name="error-0056"></a>Fout 0056  
 Uitzonde ring treedt op als de kolommen die u hebt geselecteerd voor een bewerking, de vereisten schenden.  

 Deze fout in Azure Machine Learning treedt op wanneer u kolommen kiest voor een bewerking waarvoor de kolom van een bepaald gegevens type is. 

 Deze fout kan ook optreden als de kolom het juiste gegevens type is, maar de module die u gebruikt vereist dat de kolom ook is gemarkeerd als een functie, label of categorische kolom.  

  <!--For example, the [Convert to Indicator Values](convert-to-indicator-values.md) module requires that columns be categorical, and will raise this error if you select a feature column or label column.  -->

**Oplossing:**

1.  Controleer het gegevens type van de kolommen die momenteel zijn geselecteerd. 

2. Controleer of de geselecteerde kolommen categorische, label of functie kolommen zijn.  
  
3.  Raadpleeg het Help-onderwerp voor de module waarin u de kolom selectie hebt gemaakt om te bepalen of er specifieke vereisten gelden voor het gegevens type of het kolom gebruik.  
  
3.  Gebruik [meta gegevens bewerken](edit-metadata.md) om het kolom Type voor de duur van deze bewerking te wijzigen. Zorg ervoor dat u het kolom type weer op de oorspronkelijke waarde wijzigt, met behulp van een ander exemplaar van [meta gegevens bewerken](edit-metadata.md)als u dit nodig hebt voor downstream-bewerkingen.  

|Uitzonderings berichten|
|------------------------|
|Een of meer geselecteerde kolommen bevinden zich niet in een categorie die is toegestaan.|
|De kolom met de naam {col_name} bevindt zich niet in een toegestane categorie.|


## <a name="error-0057"></a>Fout 0057  
 Uitzonde ring treedt op bij het maken van een bestand of BLOB die al bestaat.  

 Deze uitzonde ring treedt op wanneer u de module [gegevens exporteren](export-data.md) of een andere module gebruikt om resultaten van een pijp lijn in azure machine learning naar Azure Blob-opslag op te slaan, maar u probeert een bestand of BLOB te maken die al bestaat.   

**Oplossing:**

 Deze fout wordt alleen weer gegeven als u eerder de eigenschap **schrijf modus van Azure Blob Storage** hebt ingesteld op **fout**. In deze module wordt een fout gegenereerd als u probeert een gegevensset te schrijven naar een blob die al bestaat.

 - Open de module-eigenschappen en wijzig de eigenschap **schrijf modus van Azure Blob Storage** in **overschrijven**.
 - U kunt ook de naam van een andere bestemmings-BLOB of-bestand typen en ervoor zorgen dat u een BLOB opgeeft die nog niet bestaat.  

|Uitzonderings berichten|
|------------------------|
|Het bestand of de BLOB bestaat al.|
|Het bestand of de blob "{0}" bestaat al.|


## <a name="error-0058"></a>Fout 0058  
 Deze fout in Azure Machine Learning treedt op als de DataSet de verwachte Label kolom niet bevat.  

 Deze uitzonde ring kan ook optreden als de opgegeven label kolom niet overeenkomt met de gegevens of het data type dat wordt verwacht door de cursist of onjuiste waarden heeft. Deze uitzonde ring wordt bijvoorbeeld gemaakt wanneer u een label kolom met een werkelijke waarde gebruikt bij het trainen van een binaire classificatie.  

**Oplossing:** De oplossing is afhankelijk van de leerer of trainer die u gebruikt, en de gegevens typen van de kolommen in uw gegevensset. Controleer eerst de vereisten van de module machine learning algoritme of training.  

 Ga naar de invoer gegevensset. Controleer of de kolom die u verwacht te behandelen, het juiste gegevens type heeft voor het model dat u maakt.  

 Controleer de invoer voor ontbrekende waarden en verwijder deze indien nodig.  

 Voeg, indien nodig, de module [meta gegevens bewerken](edit-metadata.md) toe en zorg ervoor dat de kolom Label is gemarkeerd als een label.  

|Uitzonderings berichten|
|------------------------|
|De kolom waarden van de label kolom en de Score van het gescoorde label zijn niet vergelijkbaar.|
|De kolom Label is niet zoals verwacht in {dataset_name}.|
|De kolom Label is niet zoals verwacht in {dataset_name}, {Reason}.|
|De label kolom {column_name} wordt niet verwacht in {dataset_name}.|
|De label kolom {column_name} wordt niet verwacht in {dataset_name}, {Reason}.|


## <a name="error-0059"></a>Fout 0059  
 Uitzonde ring treedt op als een kolom index die is opgegeven in een kolom kiezer, niet kan worden geparseerd.  

 Deze fout in Azure Machine Learning treedt op als een kolom index die is opgegeven bij het gebruik van de kolom kiezer, niet kan worden geparseerd.  Deze fout wordt weer gegeven wanneer de kolom index een ongeldige indeling heeft die niet kan worden geparseerd.  

**Oplossing:** Wijzig de kolom index om een geldige index waarde te gebruiken.  

|Uitzonderings berichten|
|------------------------|
|Een of meer opgegeven kolom indexen of index bereiken kunnen niet worden geparseerd.|
|De kolom index of het bereik{0}kan niet worden geparseerd.|


## <a name="error-0060"></a>Fout 0060  
 Uitzonde ring treedt op wanneer een buiten bereik kolom bereik is opgegeven in een kolom kiezer.  

 Deze fout in Azure Machine Learning treedt op wanneer een out-of-Range-kolom bereik is opgegeven in de kolom kiezer. Deze fout wordt weer gegeven als het kolom bereik in de kolom kiezer niet overeenkomt met de kolommen in de gegevensset.  

**Oplossing:** Wijzig het kolom bereik in de kolom kiezer zodat dit overeenkomt met de kolommen in de gegevensset.  

|Uitzonderings berichten|
|------------------------|
|Ongeldig of buiten bereik opgegeven kolom index bereik.|
|Het kolom bereik{0}is ongeldig of valt buiten het bereik.|


## <a name="error-0061"></a>Fout 0061  
 Er treedt een uitzonde ring op bij het toevoegen van een rij aan een DataTable met een verschillend aantal kolommen dan de tabel.  

 Deze fout in Azure Machine Learning treedt op wanneer u een rij probeert toe te voegen aan een gegevensset met een ander aantal kolommen dan de gegevensset.  U ontvangt deze fout melding als de rij die wordt toegevoegd aan de gegevensset, een verschillend aantal kolommen uit de invoer gegevensset heeft.  De rij kan niet worden toegevoegd aan de gegevensset als het aantal kolommen niet overeenkomt.  

**Oplossing:** Wijzig de invoer-gegevensset zodat hetzelfde aantal kolommen hetzelfde is als de toegevoegde rij, of wijzig de rij die wordt toegevoegd aan hetzelfde aantal kolommen als de gegevensset.  

|Uitzonderings berichten|
|------------------------|
|Alle tabellen moeten hetzelfde aantal kolommen hebben.|
|De kolommen in het segment {chunk_id_1} wijken af van het segment {chunk_id_2} met de segment grootte: {chunk_size}.|
|Het aantal kolommen in het bestand {filename_1} (Count = {column_count_1}) wijkt af van het bestand {filename_2} (Count = {column_count_2}).|


## <a name="error-0062"></a>Fout 0062  
 Er treedt een uitzonde ring op wanneer u twee modellen met verschillende typen kennissen wilt vergelijken.  

 Deze fout in Azure Machine Learning wordt gemaakt wanneer de metrische gegevens voor de evaluatie van twee verschillende gescoorde gegevens sets niet kunnen worden vergeleken. In dit geval is het niet mogelijk om de effectiviteit te vergelijken van de modellen die worden gebruikt voor het produceren van de twee gescoorde gegevens sets.  

**Oplossing:** Controleer of de gescoorde resultaten worden geproduceerd door hetzelfde soort machine learning model (binaire classificatie, regressie, classificatie met meerdere klassen, aanbeveling, Clustering, afwijkings detectie enz.) Alle modellen die u vergelijkt, moeten hetzelfde type leerder hebben.  

|Uitzonderings berichten|
|------------------------|
|Alle modellen moeten hetzelfde informatie type hebben.|
|Incompatibel leerer type: {actual_learner_type}. Verwachte kennis typen zijn: {expected_learner_type_list}.|


## <a name="error-0064"></a>Fout 0064  
 Uitzonde ring treedt op als de naam van het Azure Storage-account of de opslag sleutel onjuist is opgegeven.  

 Deze fout in Azure Machine Learning treedt op als de naam van het Azure Storage-account of de opslag sleutel onjuist is opgegeven. Deze fout wordt weer gegeven als u een onjuiste account naam of een onjuist wacht woord voor het opslag account opgeeft. Dit kan gebeuren als u de account naam of het wacht woord hand matig invoert. Dit kan ook gebeuren als het account is verwijderd.  

**Oplossing:** Controleer of de account naam en het wacht woord juist zijn ingevoerd en of het account bestaat.  

|Uitzonderings berichten|
|------------------------|
|De naam van het Azure-opslag account of de opslag sleutel is onjuist.|
|De naam van het Azure-opslag account {account_name} of de opslag sleutel voor de account naam is onjuist.|


## <a name="error-0065"></a>Fout 0065  
 Uitzonde ring treedt op als de naam van de Azure-Blob onjuist is opgegeven.  

 Deze fout in Azure Machine Learning treedt op als de naam van de Azure-Blob onjuist is opgegeven.  De volgende fout wordt weer gegeven als:  

-   De blob is niet gevonden in de opgegeven container.  

 <!---   The fully qualified name of the blob specified for output in one of the [Learning with Counts](data-transformation-learning-with-counts.md) modules is greater than 512 characters.  -->

-   Alleen de container is opgegeven als bron in een aanvraag voor het [importeren van gegevens](import-data.md) wanneer de indeling Excel of CSV met code ring was. het samen voegen van de inhoud van alle blobs in een container is niet toegestaan met deze notaties.  
  
-   Een SAS-URI bevat niet de naam van een geldige blob.  

**Oplossing:** Ga naar de module om de uitzonde ring te genereren. Controleer of de opgegeven BLOB bestaat in de container in het opslag account en of de machtigingen de BLOB kunnen bekijken. Controleer of de invoer van het formulier **containerName/filename** is als u Excel of CSV met coderings indelingen hebt. Controleer of een SAS-URI de naam van een geldige BLOB bevat.  

|Uitzonderings berichten|
|------------------------|
|De naam van de Azure Storage-blob is onjuist.|
|De naam {blob_name} van de Azure Storage-blob is onjuist.|
|De naam van de Azure Storage-blob met het voor voegsel {blob_name_prefix} bestaat niet.|
|Kan geen Azure Storage-blobs vinden onder de container {container_name}.|
|Kan geen Azure Storage-blobs vinden met het Joker teken {blob_wildcard_path}.|


## <a name="error-0066"></a>Fout 0066  
 Uitzonde ring treedt op als een bron niet naar een Azure-Blob kan worden geüpload.  

 Deze fout in Azure Machine Learning treedt op als een bron niet naar een Azure-Blob kan worden geüpload.  <!--You will receive this message if [Train Vowpal Wabbit 7-4 Model](train-vowpal-wabbit-version-7-4-model.md) encounters an error attempting to save either the model or the hash created when training the model.--> Beide worden opgeslagen in hetzelfde Azure-opslag account als het account dat het invoer bestand bevat.  

**Oplossing:** Ga naar de module. Controleer of de naam van het Azure-account, de opslag sleutel en de container juist zijn en of het account gemachtigd is om naar de container te schrijven.  

|Uitzonderings berichten|
|------------------------|
|De resource kan niet worden geüpload naar Azure Storage.|
|Het bestand '{0}' kan niet worden geüpload naar Azure Storage als '{1}'.|


## <a name="error-0067"></a>Fout 0067  
 Uitzonde ring treedt op als een dataset een verschillend aantal kolommen heeft dan verwacht.  

 Deze fout in Azure Machine Learning treedt op als een dataset een ander aantal kolommen heeft dan verwacht.  Deze fout wordt weer gegeven wanneer het aantal kolommen in de gegevensset afwijkt van het aantal kolommen dat de module verwacht tijdens de uitvoering.  

**Oplossing:** Wijzig de invoer gegevensset of de para meters.  

|Uitzonderings berichten|
|------------------------|
|Er is een onverwacht aantal kolommen in de DataTable.|
|Er is een onverwacht aantal kolommen in de gegevensset {dataset_name}.|
|De kolom (men) {expected_column_count} werd verwacht, maar er zijn {actual_column_count} kolom (men) gevonden.|
|In de invoer-gegevensset {dataset_name} werd {expected_column_count} verwacht, maar er zijn {actual_column_count} kolom (men) gevonden.|


## <a name="error-0068"></a>Fout 0068  
 Uitzonde ring treedt op als het opgegeven Hive-script onjuist is.  

 Deze fout in Azure Machine Learning treedt op als er syntaxis fouten in het script van een Hive QL zijn of als de Hive-interpreter een fout ontdekt tijdens het uitvoeren van de query of het script.  

**Oplossing:**

Het fout bericht van Hive wordt normaal gesp roken weer gegeven in het fouten logboek, zodat u actie kunt ondernemen op basis van de specifieke fout. 

+ Open de module en Inspecteer de query op fouten.  
+ Controleer of de query correct werkt buiten Azure Machine Learning door u aan te melden bij de Hive-console van uw Hadoop-cluster en de query uit te voeren.  
+ Probeer opmerkingen in het Hive-script op een afzonderlijke regel te plaatsen, in tegens telling tot het combi neren van uitvoer bare instructies en opmerkingen op één regel.  

### <a name="resources"></a>Bronnen

Raadpleeg de volgende artikelen voor meer informatie over Hive-query's voor machine learning:

+ [Hive-tabellen maken en gegevens laden vanuit Azure Blob Storage](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-move-hive-tables)
+ [Gegevens in tabellen verkennen met hive-query's](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-explore-data-hive-tables)
+ [Functies maken voor gegevens in een Hadoop-cluster met behulp van Hive-query's](https://docs.microsoft.com/azure/machine-learning/machine-learning-data-science-create-features-hive)
+ [Hive voor SQL-gebruikers Cheat Sheet (PDF)](http://hortonworks.com/wp-content/uploads/2013/05/hql_cheat_sheet.pdf)

  
|Uitzonderings berichten|
|------------------------|
|Het Hive-script is onjuist.|
|Het Hive-script {0} is niet juist.|


## <a name="error-0069"></a>Fout 0069  
 Uitzonde ring treedt op als het opgegeven SQL-script onjuist is.  

 Deze fout in Azure Machine Learning treedt op als het opgegeven SQL-script syntaxis problemen bevat of als de kolommen of tabel die is opgegeven in het script, ongeldig zijn. 

 Deze fout wordt weer gegeven als de SQL-engine een fout tegen komt tijdens het uitvoeren van de query of het script. Het SQL-fout bericht wordt normaal gesp roken weer gegeven in het fouten logboek, zodat u actie kunt ondernemen op basis van de specifieke fout.  

**Oplossing:** Ga naar de module en Inspecteer de SQL-query op fouten.  

 Controleer of de query correct werkt buiten Azure ML door u rechtstreeks aan te melden bij de database server en de query uit te voeren.  

 Als er een gegenereerd SQL-bericht wordt gemeld door de module-uitzonde ring, actie ondernemen op basis van de gerapporteerde fout. De fout berichten bevatten bijvoorbeeld soms specifieke richt lijnen voor de waarschijnlijke fout:
+ Een *dergelijke kolom of ontbrekende data base*geeft aan dat u een ongeldige kolom naam hebt opgegeven. Als u zeker weet dat de naam van de kolom juist is, gebruikt u accolades of aanhalings tekens om de kolom-id op te sluiten.
+ *SQL-logische fout in de buurt van \<SQL-tref woord\>* , wat aangeeft dat u een syntaxis fout hebt vóór het opgegeven tref woord

  
|Uitzonderings berichten|
|------------------------|
|SQL-script is onjuist.|
|De SQL-query{0}is niet juist.|
|De SQL-query{0}is niet juist:{1}.|


## <a name="error-0070"></a>Fout 0070  
 Uitzonde ring treedt op bij een poging toegang te krijgen tot een niet-bestaande Azure-tabel.  

 Deze fout in Azure Machine Learning treedt op wanneer u een niet-bestaande Azure-tabel probeert te openen. Deze fout wordt weer gegeven als u een tabel in azure Storage opgeeft die niet bestaat als u leest van of schrijft naar Azure Table Storage. Dit kan gebeuren als u de naam van de gewenste tabel onjuist typt of als de doel naam en het opslag type niet overeenkomen. U kunt bijvoorbeeld een tabel lezen, maar u kunt in plaats daarvan de naam van een BLOB opgeven.  

**Oplossing:** Ga naar de module om te controleren of de naam van de tabel juist is.  

|Uitzonderings berichten|
|------------------------|
|De Azure-tabel bestaat niet.|
|De Azure-tabel{0}bestaat niet.|


## <a name="error-0072"></a>Fout 0072  
 Uitzonde ring treedt op in het geval van een time-out voor de verbinding.  

 Deze fout in Azure Machine Learning treedt op wanneer er een time-out optreedt voor een verbinding. U ontvangt deze fout melding als er momenteel verbindings problemen zijn met de gegevens bron of het doel, zoals een trage Internet verbinding, of als de gegevensset groot is en/of de SQL-query die in de gegevens wordt gelezen, complexe verwerking uitvoert.  

**Oplossing:** Bepaal of er momenteel problemen zijn met trage verbindingen met Azure Storage of Internet.  

|Uitzonderings berichten|
|------------------------|
|Verbindingstime-out opgetreden.|


## <a name="error-0073"></a>Fout 0073  
 Uitzonde ring treedt op als er een fout optreedt tijdens het converteren van een kolom naar een ander type.  

 Deze fout in Azure Machine Learning treedt op wanneer het niet mogelijk is om een kolom naar een ander type te converteren.  U ontvangt deze fout melding als een module een bepaald type vereist en het niet mogelijk is om de kolom te converteren naar het nieuwe type.  

**Oplossing:** Wijzig de invoer gegevensset zodat de kolom kan worden geconverteerd op basis van de interne uitzonde ring.  

|Uitzonderings berichten|
|------------------------|
|Kan kolom niet converteren.|
|Kan de kolom niet converteren naar {0}.|


## <a name="error-0075"></a>Fout 0075  
Uitzonde ring treedt op wanneer een ongeldige binning-functie wordt gebruikt wanneer een gegevensset wordt quantizing.  

Deze fout in Azure Machine Learning treedt op wanneer u gegevens op basis van een niet-ondersteunde methode probeert te bin of wanneer de parameter combinaties ongeldig zijn.  

**Oplossing:**

Fout afhandeling voor deze gebeurtenis is geïntroduceerd in een eerdere versie van Azure Machine Learning die meer aanpassing van binning-methoden had toegestaan. Momenteel zijn alle binning-methoden gebaseerd op een selectie uit een vervolg keuzelijst, zodat deze fout niet meer kan worden weer geven.

 <!--If you get this error when using the [Group Data into Bins](group-data-into-bins.md) module, consider reporting the issue in the [Azure Machine Learning forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=MachineLearning), providing the data types, parameter settings, and the exact error message.  -->

|Uitzonderings berichten|
|------------------------|
|Er is een ongeldige binning-functie gebruikt.|


## <a name="error-0077"></a>Fout 0077  
 Uitzonde ring treedt op wanneer er een onbekende schrijf modus voor BLOB-bestanden is door gegeven.  

 Deze fout in Azure Machine Learning treedt op als een ongeldig argument wordt door gegeven in de specificaties voor een doel of bron van een blob-bestand.  

**Oplossing:** In bijna alle modules die gegevens importeren of exporteren naar en van Azure Blob-opslag, worden parameter waarden die de schrijf modus beheren, toegewezen met behulp van een vervolg keuzelijst. Daarom is het niet mogelijk een ongeldige waarde door te geven en deze fout zou niet moeten worden weer gegeven. Deze fout wordt in een latere versie afgeschaft.  

|Uitzonderings berichten|
|------------------------|
|Niet-ondersteunde BLOB-schrijf modus.|
|Niet-ondersteunde BLOB-schrijf modus: {0}.|


## <a name="error-0078"></a>Fout 0078  
 Uitzonde ring treedt op wanneer de HTTP-optie voor [import gegevens](import-data.md) een 3xx-status code ontvangt die omleiding aangeeft.  

 Deze fout in Azure Machine Learning treedt op wanneer de HTTP-optie voor [import gegevens](import-data.md) een 3xx-status code (301, 302, 304, etc.) ontvangt die omleiding aangeeft. Deze fout wordt weer gegeven als u probeert verbinding te maken met een HTTP-bron waarmee de browser wordt omgeleid naar een andere pagina. Uit veiligheids overwegingen zijn het omleiden van websites niet toegestaan als gegevens bronnen voor Azure Machine Learning.  

**Oplossing:** Als de website een vertrouwde website is, voert u de omgeleide URL rechtstreeks in.  

|Uitzonderings berichten|
|------------------------|
|HTTP-omleiding is niet toegestaan.|


## <a name="error-0079"></a>Fout 0079  
 Uitzonde ring treedt op als de naam van de Azure storage-container onjuist is opgegeven.  

 Deze fout in Azure Machine Learning treedt op als de naam van de Azure storage-container onjuist is opgegeven. Deze fout wordt weer gegeven als u niet zowel de container als de BLOB (bestands naam) hebt opgegeven met **het pad naar de blob die begint met de container** optie bij het schrijven naar Azure Blob Storage.  

**Oplossing:** Ga naar de module [gegevens exporteren](export-data.md) en controleer of het opgegeven pad naar de BLOB zowel de container als de bestands naam bevat in de indeling **container/filename**.  

|Uitzonderings berichten|
|------------------------|
|De naam van de Azure storage-container is onjuist.|
|De naam van de Azure storage-container "{0}" is onjuist; Er wordt een container naam van de indeling container/BLOB verwacht.|


## <a name="error-0080"></a>Fout 0080  
 Uitzonde ring treedt op wanneer de kolom met alle ontbrekende waarden niet is toegestaan door de module.  

 Deze fout in Azure Machine Learning wordt gemaakt wanneer een of meer van de kolommen die door de module worden gebruikt, alle ontbrekende waarden bevat. Als een module bijvoorbeeld statistische statistieken voor elke kolom computing, kan deze niet worden gebruikt voor een kolom die geen gegevens bevat. In dergelijke gevallen wordt het uitvoeren van de module gestopt met deze uitzonde ring.  

**Oplossing:** Ga naar de invoer gegevensset en verwijder alle kolommen die alle ontbrekende waarden bevatten.  

|Uitzonderings berichten|
|------------------------|
|Kolommen met alle waarden ontbreken, zijn niet toegestaan.|
|Voor de kolom {0} zijn alle waarden ontbreken.|


## <a name="error-0081"></a>Fout 0081  
 Uitzonde ring treedt op in de PCA-module als het aantal dimensies dat moet worden verkleind, gelijk is aan het aantal functie kolommen in de invoer gegevensset, dat ten minste één sparse-functie kolom bevat.  

 Deze fout in Azure Machine Learning wordt gemaakt als aan de volgende voor waarden wordt voldaan: (a) de invoer gegevensset heeft ten minste één sparse kolom en (b) het uiteindelijke aantal aangevraagde dimensies is hetzelfde als het aantal invoer dimensies.  

**Oplossing:** Overweeg het aantal dimensies in de uitvoer te verminderen om minder te zijn dan het aantal dimensies in de invoer. Dit is gebruikelijk in toepassingen van PCA.   <!--For more information, see [Principal Component Analysis](principal-component-analysis.md).  -->

|Uitzonderings berichten|
|------------------------|
|Voor een gegevensset met sparse-functie kolommen is het aantal dimensies dat moet worden beperkt kleiner dan het aantal functie kolommen.|


## <a name="error-0082"></a>Fout 0082  
 Uitzonde ring treedt op wanneer een model niet kan worden gedeserialiseerd.  

 Deze fout in Azure Machine Learning treedt op wanneer een opgeslagen machine learning model of trans formatie niet kan worden geladen met een nieuwere versie van de Azure Machine Learning-runtime als gevolg van een wijziging van het resultaat.  

**Oplossing:** De trainings pijplijn die het model of de trans formatie produceert, moet opnieuw worden uitgevoerd en het model of de trans formatie moet opnieuw worden opgeslagen.  

|Uitzonderings berichten|
|------------------------|
|Model kan niet worden gedeserialiseerd, omdat het waarschijnlijk met een oudere serialisatie-indeling is geserialiseerd. Probeer het model opnieuw te trainen en op te slaan.|


## <a name="error-0083"></a>Fout 0083  
 Uitzonde ring treedt op als de gegevensset die wordt gebruikt voor de training, niet kan worden gebruikt voor een concreet type leerder.  

 Deze fout in Azure Machine Learning wordt gemaakt wanneer de gegevensset niet compatibel is met de cursist die wordt getraind. De gegevensset kan bijvoorbeeld ten minste één ontbrekende waarde in elke rij bevatten. als gevolg hiervan wordt de hele gegevensset tijdens de training overgeslagen. In andere gevallen verwachten sommige machine learning-algoritmen, zoals afwijkings detectie, niet dat labels aanwezig zijn en kan deze uitzonde ring worden gegenereerd als labels aanwezig zijn in de gegevensset.  

**Oplossing:** Raadpleeg de documentatie van de kenniser die wordt gebruikt om de vereisten voor de invoer gegevensset te controleren. Bekijk de kolommen om te zien of alle vereiste kolommen aanwezig zijn.  

|Uitzonderings berichten|
|------------------------|
|De gegevensset die voor de training wordt gebruikt, is ongeldig.|
|{0} bevat ongeldige gegevens voor training.|
|{0} bevat ongeldige gegevens voor training. Type informatieer: {1}.|
|{0} bevat ongeldige gegevens voor training. Type informatieer: {1}. Reden: {2}.|


## <a name="error-0084"></a>Fout 0084  
 Uitzonde ring treedt op wanneer scores die zijn gegenereerd op basis van een R-script worden geëvalueerd. Dit wordt momenteel niet ondersteund.  

 Deze fout in Azure Machine Learning treedt op als u probeert een van de modules te gebruiken voor het evalueren van een model met uitvoer van een R-script dat scores bevat.  

**Oplossing:**

|Uitzonderings berichten|
|------------------------|
|Het evalueren van scores die zijn geproduceerd door het aangepaste model wordt momenteel niet ondersteund.|


## <a name="error-0085"></a>Fout 0085  
 Uitzonde ring treedt op wanneer de script evaluatie mislukt met een fout.  

 Deze fout in Azure Machine Learning treedt op wanneer u een aangepast script uitvoert dat syntaxis fouten bevat.  

**Oplossing:** Controleer uw code in een externe editor en controleer op fouten.  

|Uitzonderings berichten|
|------------------------|
|Er is een fout opgetreden tijdens de evaluatie van het script.|
|De volgende fout is opgetreden tijdens het uitvoeren van een script, Raadpleeg het uitvoer logboek voor meer informatie:----------begin van fout bericht van {script_language}-interpreter----------{Message}----------einde van het fout bericht van {script_language}  Interpreter----------|


## <a name="error-0090"></a>Fout 0090  
 Uitzonde ring treedt op wanneer het maken van de Hive-tabel is mislukt.  

 Deze fout in Azure Machine Learning treedt op wanneer u [gegevens exporteren](export-data.md) of een andere optie gebruikt om gegevens op te slaan in een HDInsight-cluster en de opgegeven Hive-tabel kan niet worden gemaakt.  

**Oplossing:** Controleer de naam van het Azure Storage-account dat aan het cluster is gekoppeld en controleer of u hetzelfde account in de module eigenschappen gebruikt.  

|Uitzonderings berichten|
|------------------------|
|De Hive-tabel kan niet worden gemaakt. Voor een HDInsight-cluster moet u ervoor zorgen dat de naam van het Azure Storage-account die aan het cluster is gekoppeld, overeenkomt met wat wordt door gegeven via de module parameter.|
|De Hive-tabel{0}kan niet worden gemaakt. Voor een HDInsight-cluster moet u ervoor zorgen dat de naam van het Azure Storage-account die aan het cluster is gekoppeld, overeenkomt met wat wordt door gegeven via de module parameter.|
|De Hive-tabel{0}kan niet worden gemaakt. Controleer voor een HDInsight-cluster of de naam van het Azure Storage-account dat is gekoppeld aan het cluster{1}is.|


## <a name="error-0102"></a>Fout 0102  
 Wordt gegenereerd wanneer een ZIP-bestand niet kan worden geëxtraheerd.  

 Deze fout in Azure Machine Learning treedt op wanneer u een gezipt pakket importeert met de extensie. zip, maar het pakket geen zip-bestand is, of als het bestand geen gebruik maakt van een ondersteunde zip-indeling.  

**Oplossing:** Zorg ervoor dat het geselecteerde bestand een geldig zip-bestand is en dat het is gecomprimeerd met een van de ondersteunde compressie algoritmen.  

 Als deze fout optreedt bij het importeren van gegevens sets in gecomprimeerde vorm, controleert u of alle opgenomen bestanden een van de ondersteunde bestands indelingen gebruiken en in Unicode-indeling zijn.  <!--For more information, see [Unpack Zipped Datasets](unpack-zipped-datasets.md).  -->

 Lees de gewenste bestanden naar een nieuwe gecomprimeerde map en probeer opnieuw de aangepaste module toe te voegen.  

|Uitzonderings berichten|
|------------------------|
|Het opgegeven ZIP-bestand heeft niet de juiste indeling.|


## <a name="error-0105"></a>Fout 0105  
 Deze fout wordt weer gegeven wanneer een module definitie bestand een niet-ondersteund parameter type bevat  
  
 Deze fout in Azure Machine Learning wordt gemaakt wanneer u een XML-definitie voor een aangepaste module maakt en het type van een para meter of argument in de definitie komt niet overeen met een ondersteund type.  
  
**Oplossing:** Zorg ervoor dat de eigenschap type van een **ARG** -element in het XML-definitie bestand van de aangepaste module een ondersteund type is.  
  
|Uitzonderings berichten|  
|------------------------|  
|Niet-ondersteund parameter type.|  
|Niet-ondersteund parameter type '{0}' opgegeven.|  


## <a name="error-0125"></a>Fout 0125  
 Wordt gegenereerd wanneer het schema voor meerdere gegevens sets niet overeenkomt.  

**Oplossing:**

|Uitzonderings berichten|
|------------------------|
|Het gegevensset-schema komt niet overeen.|


## <a name="error-0127"></a>Fout 0127  
 De pixel grootte van de afbeelding overschrijdt de toegestane limiet  

 Deze fout treedt op als u afbeeldingen uit een afbeeldings gegevensset voor classificatie leest en de installatie kopieën groter zijn dan het model kan verwerken.  

 <!--**Resolution:**
 For more information about the image size and other requirements, see these topics:  

-   [Import Images](import-images.md)  
  
-   [Pretrained Cascade Image Classification](pretrained-cascade-image-classification.md)  -->

|Uitzonderings berichten|
|------------------------|
|De pixel grootte van de afbeelding overschrijdt de toegestane limiet.|
|De pixel grootte van de afbeelding in het bestand{0}overschrijdt de toegestane limiet:{1}.|


## <a name="error-0128"></a>Fout 0128  
 Het aantal voorwaardelijke kansen voor categorische kolommen overschrijdt de limiet.  

**Oplossing:**

|Uitzonderings berichten|
|------------------------|
|Het aantal voorwaardelijke kansen voor categorische kolommen overschrijdt de limiet.|
|Het aantal voorwaardelijke kansen voor categorische kolommen overschrijdt de limiet. De kolommen{0}en{1}zijn het problematische paar.|


## <a name="error-0129"></a>Fout 0129  
 Het aantal kolommen in de gegevensset overschrijdt de toegestane limiet.  

**Oplossing:**

|Uitzonderings berichten|
|------------------------|
|Het aantal kolommen in de gegevensset overschrijdt de toegestane limiet.|
|Het aantal kolommen in de gegevensset in {dataset_name} is groter dan toegestaan.|
|Het aantal kolommen in de gegevensset in {dataset_name} overschrijdt de toegestane limiet van {component_name}.|
|Het aantal kolommen in de gegevensset in {dataset_name} overschrijdt de toegestane limiet voor {limit_columns_count} van {component_name}.|


## <a name="error-0134"></a>Fout 0134
Uitzonde ring treedt op als de label kolom ontbreekt of als er onvoldoende rijen met labels zijn.  

Deze fout treedt op wanneer voor de module een label kolom is vereist, maar er geen waarde in de kolom selectie is opgegeven, of als de kolom label te veel waarden bevat.

Deze fout kan ook optreden wanneer een eerdere bewerking de gegevensset wijzigt, zodat er onvoldoende rijen beschikbaar zijn voor een stroomafwaartse bewerking. Stel dat u een expressie in de **partitie-en voorbeeld** module gebruikt om een gegevensset te verdelen op basis van waarden. Als er geen overeenkomsten worden gevonden voor uw expressie, zou een van de gegevens sets die het resultaat zijn van de partitie, leeg zijn.

Oplossing: 

 Als u een kolom Label in de kolom selectie opneemt, maar deze niet wordt herkend, gebruikt u de module [meta gegevens bewerken](edit-metadata.md) om deze te markeren als een label kolom.

  <!--Use the [Summarize Data](summarize-data.md) module to generate a report that shows how many values are missing in each column. -->Vervolgens kunt u de module [clean Missing Data](clean-missing-data.md) gebruiken om rijen te verwijderen met ontbrekende waarden in de kolom Label. 

 Controleer de invoer gegevens sets om er zeker van te zijn dat ze geldige gegevens bevatten, en voldoende rijen om te voldoen aan de vereisten van de bewerking. Bij veel algoritmen wordt een fout bericht gegenereerd als er een minimum aantal rijen met gegevens nodig is, maar de gegevens slechts een paar rijen of alleen een koptekst bevatten.

|Uitzonderings berichten|
|------------------------|
|Uitzonde ring treedt op als de label kolom ontbreekt of als er onvoldoende rijen met labels zijn.|
|Er treedt een uitzonde ring op wanneer de label kolom ontbreekt of korter is dan {required_rows_count} gelabelde rijen.|
|Uitzonde ring treedt op wanneer de label kolom in de gegevensset {dataset_name} ontbreekt of korter is dan {required_rows_count} gelabelde rijen.|


## <a name="error-0138"></a>Fout 0138  
 Het geheugen is uitgeput, kan de uitvoering van de module niet volt ooien. Het downsamplen van de gegevensset kan helpen om het probleem op te lossen.  

 Deze fout treedt op wanneer voor de module die wordt uitgevoerd meer geheugen nodig is dan beschikbaar is in de Azure-container. Dit kan gebeuren als u met een grote gegevensset werkt en de huidige bewerking niet in het geheugen past.  

**Oplossing:** Als u een grote gegevensset probeert te lezen en de bewerking niet kan worden voltooid, kan het downsamplen van de gegevensset helpen.  

  <!--If you use the visualizations on datasets to check the cardinality of columns, only some rows are sampled. To get a full report, use [Summarize Data](summarize-data.md). You can also use the [Apply SQL Transformation](apply-sql-transformation.md) to check for the number of unique values in each column.  

 Sometimes transient loads can lead to this error. Machine support also changes over time. 

 Try using [Principal Component Analysis](principal-component-analysis.md) or one of the provided feature selection methods to reduce your dataset to a smaller set of more feature-rich columns: [Feature Selection](feature-selection-modules.md)  -->

|Uitzonderings berichten|
|------------------------|
|Het geheugen is uitgeput, kan de uitvoering van de module niet volt ooien.|
|Het geheugen is uitgeput, kan de uitvoering van de module niet volt ooien. Details: {Details}|


## <a name="error-0141"></a>Fout 0141  
 Uitzonde ring treedt op als het aantal geselecteerde numerieke kolommen en unieke waarden in de categorische-en reeks kolommen te klein is.  

 Deze fout in Azure Machine Learning treedt op wanneer er onvoldoende unieke waarden in de geselecteerde kolom zijn om de bewerking uit te voeren.  

**Oplossing:** Sommige bewerkingen voeren statistische bewerkingen uit op functie-en categorische-kolommen. als er onvoldoende waarden zijn, kan de bewerking mislukken of kan een ongeldig resultaat worden geretourneerd. Controleer uw gegevensset om te zien hoeveel waarden er in de kolommen functie en label staan, en bepaal of de bewerking die u probeert uit te voeren, statistisch geldig is.  

 Als de bron-gegevensset geldig is, kunt u ook controleren of een bewerking voor het door lopen van gegevens of de meta gegevens van de gegevens is gewijzigd en dat er waarden zijn verwijderd.  

 Als de upstream-bewerkingen splitsen, steek proeven of resampling omvatten, controleert u of de uitvoer het verwachte aantal rijen en waarden bevat.  

|Uitzonderings berichten|
|------------------------|
|Het aantal geselecteerde numerieke kolommen en unieke waarden in de categorische-en reeks kolommen is te klein.|
|Het totale aantal geselecteerde numerieke kolommen en unieke waarden in de categorische-en teken reeks kolommen (momenteel {0}) moet ten minste {1}zijn.|


## <a name="error-0154"></a>Fout 0154  
 Uitzonde ring treedt op wanneer de gebruiker gegevens probeert toe te voegen op sleutel kolommen met een niet-compatibel kolom Type.

|Uitzonderings berichten|
|------------------------|
|De element typen van de sleutel kolom zijn niet compatibel.|
|De element typen van de sleutel kolom zijn niet compatibel. (links: {keys_left}; rechts: {keys_right})|


## <a name="error-0155"></a>Fout 0155  
 Uitzonde ring treedt op wanneer de kolom namen van de gegevensset geen teken reeks zijn.

|Uitzonderings berichten|
|------------------------|
|Kolom namen zijn geen teken reeks.|
|Kolom namen: {column_names} zijn geen teken reeks.|


## <a name="error-0156"></a>Fout 0156  
 Uitzonde ring treedt op wanneer het lezen van gegevens uit Azure SQL Database is mislukt.

|Uitzonderings berichten|
|------------------------|
|Kan geen gegevens lezen uit Azure SQL Database.|
|Kan geen gegevens lezen uit Azure SQL Database: {detailed_message} DB: {database_server_name}: {database_name} query: {sql_statement}|


## <a name="error-0157"></a>Fout 0157  
 De gegevens opslag is niet gevonden.

|Uitzonderings berichten|
|------------------------|
|Data Store-gegevens zijn ongeldig.|
|Data Store-gegevens zijn ongeldig. Kan geen AzureML-gegevens archief {datastore_name} ophalen in de werk ruimte {workspace_name}.|


## <a name="error-1000"></a>Fout 1000  
Uitzonde ring interne bibliotheek.  

Deze fout wordt gegeven voor het vastleggen van anderszins onverwerkte interne Engine fouten. Daarom kan de oorzaak van deze fout afwijken, afhankelijk van de module die de fout heeft gegenereerd.  

Als u meer hulp nodig hebt, raden we u aan het gedetailleerde bericht te plaatsen dat bij de fout aan het Azure Machine Learning forum wordt geleverd, samen met een beschrijving van het scenario, inclusief de gegevens die worden gebruikt als invoer. Deze feedback helpt ons bij het bepalen van de prioriteit van fouten en het identificeren van de belangrijkste problemen voor verdere werkzaamheden.  

|Uitzonderings berichten|
|------------------------|
|Bibliotheek uitzondering.|
|Uitzonde ring van bibliotheek: {0}.|
|Onbekende bibliotheek uitzondering: {0}. {1}.|

