---
title: Cognitive Services koppelen aan een vaardig heden
titleSuffix: Azure Cognitive Search
description: Instructies voor het koppelen van een Cognitive Services alles-in-één-abonnement op een AI-pijp lijn in azure Cognitive Search.
manager: nitinme
author: LuisCabrer
ms.author: luisca
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 11/04/2019
ms.openlocfilehash: d65b9b60ce93656c9acdc76c77291114468d345a
ms.sourcegitcommit: 598c5a280a002036b1a76aa6712f79d30110b98d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/15/2019
ms.locfileid: "74113940"
---
# <a name="attach-a-cognitive-services-resource-to-a-skillset-in-azure-cognitive-search"></a>Een Cognitive Services resource koppelen aan een vaardig heden in azure Cognitive Search 

AI-algoritmen stations de [verrijkings pijplijnen](cognitive-search-concept-intro.md) die worden gebruikt voor inhouds transformatie in azure Cognitive Search. Deze algoritmen zijn gebaseerd op Azure Cognitive Services-resources, waaronder [Computer Vision](https://azure.microsoft.com/services/cognitive-services/computer-vision/) voor het analyseren van afbeeldingen en optische teken herkenning (OCR) en [Text Analytics](https://azure.microsoft.com/services/cognitive-services/text-analytics/) voor entiteits herkenning, extractie van sleutel zinnen en andere verrijkingen. Zoals gebruikt door Azure Cognitive Search voor verrijkings doeleinden van documenten, worden de algoritmen in een *vaardigheid*verpakt, in een *vakkennisset*geplaatst en tijdens het indexeren naar een *indexer* verwezen.

U kunt een beperkt aantal gratis documenten verrijken. U kunt ook een factureer bare Cognitive Services resource koppelen aan een *vaardig heden* voor grotere en frequentere workloads. In dit artikel leert u hoe u een factureer bare Cognitive Services resource kunt koppelen aan verrijkte documenten tijdens het [indexeren](search-what-is-an-index.md)van Azure Cognitive Search.

> [!NOTE]
> Factureer bare gebeurtenissen omvatten aanroepen naar Cognitive Services-API's en extractie van afbeeldingen als onderdeel van de fase voor het kraken van documenten in azure Cognitive Search. Er worden geen kosten in rekening gebracht voor de extractie van tekst uit documenten of voor vaardig heden die Cognitive Services niet aanroepen.
>
> Het uitvoeren van factureer bare vaardig heden bevindt zich op de Cognitive Services prijs op basis van [betalen per gebruik](https://azure.microsoft.com/pricing/details/cognitive-services/). Zie de [pagina met prijzen voor Azure Cognitive Search](https://go.microsoft.com/fwlink/?linkid=2042400)voor meer informatie over het ophalen van images.

## <a name="same-region-requirement"></a>Dezelfde regio vereiste

Azure Cognitive Search en Azure Cognitive Services zijn vereist in dezelfde regio. Als dat niet het geval is, ontvangt u dit bericht tijdens de uitvoering: `"Provided key is not a valid CognitiveServices type key for the region of your search service."` 

Er is geen manier om een service te verplaatsen tussen regio's. Als u deze fout ontvangt, moet u een nieuwe Cognitive Services-resource maken in dezelfde regio als Azure Cognitive Search.

> [!NOTE]
> Sommige ingebouwde vaardig heden zijn gebaseerd op niet-regionale Cognitive Services (bijvoorbeeld de [tekst Vertaal vaardigheid](cognitive-search-skill-text-translation.md)). Houd er rekening mee dat als u een van deze vaardig heden aan uw vaardig heden toevoegt die niet gegarandeerd dat uw gegevens in dezelfde regio blijven als uw Azure-Cognitive Search of Cognitive Services-resource. Raadpleeg de [pagina service status](https://aka.ms/allinoneregioninfo) voor meer informatie.

## <a name="use-free-resources"></a>Gratis bronnen gebruiken

U kunt een beperkte, gratis verwerkings optie gebruiken voor het volt ooien van de zelf studie voor AI-verrijking en Snelstartgids.

Gratis (beperkte verrijkingen) resources zijn beperkt tot 20 documenten per dag, per abonnement.

1. Open de wizard gegevens importeren:

   ![De wizard gegevens importeren openen](media/search-get-started-portal/import-data-cmd.png "De wizard gegevens importeren openen")

1. Kies een gegevens bron en blijf **AI-verrijking toevoegen (optioneel)** . Zie [een index maken in de Azure Portal](search-get-started-portal.md)voor een stapsgewijze uitleg van deze wizard.

1. Vouw **Cognitive Services koppelen** uit en selecteer vervolgens **gratis (beperkte verrijkingen)** :

   ![Sectie uitgebreide Cognitive Services toevoegen](./media/cognitive-search-attach-cognitive-services/attach1.png "Sectie uitgebreide Cognitive Services toevoegen")

1. U kunt nu door gaan met de volgende stappen, waaronder **cognitieve vaardig heden toevoegen**.

## <a name="use-billable-resources"></a>Factureer bare resources gebruiken

Voor workloads die meer dan 20 verrijkingen per dag maken, moet u ervoor zorgen dat u een factureer bare Cognitive Services resource koppelt. U wordt aangeraden altijd een factureer bare Cognitive Services resource te koppelen, zelfs als u niet van plan bent om Cognitive Services-API's aan te roepen. Als u een resource koppelt, wordt de dagelijkse limiet overschreven.

Er worden alleen kosten in rekening gebracht voor de vaardig heden die de Cognitive Services-API's aanroepen. U wordt niet gefactureerd voor [aangepaste vaardig heden](cognitive-search-create-custom-skill-example.md)of vaardig heden zoals [tekst fusie](cognitive-search-skill-textmerger.md), [tekst splitsing](cognitive-search-skill-textsplit.md)en [shaper](cognitive-search-skill-shaper.md), die geen API-gebaseerd zijn.

1. Open de wizard gegevens importeren, kies een gegevens bron en blijf **AI-verrijking toevoegen (optioneel)** .

1. Vouw **Cognitive Services koppelen** uit en selecteer vervolgens **nieuwe Cognitive Services resource maken**. Er wordt een nieuw tabblad geopend, zodat u de resource kunt maken:

   ![Een Cognitive Services resource maken](./media/cognitive-search-attach-cognitive-services/cog-services-create.png "Een Cognitive Services-resource maken")

1. Selecteer in de lijst **locatie** de regio waar uw Azure Cognitive Search-service zich bevindt. Zorg ervoor dat u deze regio gebruikt om prestatie redenen. Als u deze regio gebruikt, worden ook de kosten voor uitgaande band breedte tussen regio's verwijderd.

1. Selecteer in de lijst **prijs categorie** de optie **s0** om de alles-in-één-verzameling van Cognitive Services-functies op te halen, met inbegrip van de visie-en taal functies die de ingebouwde vaardig heden van Azure Cognitive Search.

   Voor de S0-laag kunt u tarieven voor specifieke werk belastingen vinden op de [pagina met Cognitive Services prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/).
  
   + Zorg ervoor dat **Cognitive Services** is geselecteerd in de lijst **aanbieding selecteren** .
   + Onder **taal** functies zijn de tarieven voor **Text Analytics standaard** van toepassing op AI-indexering.
   + Onder **Vision** -functies zijn de tarieven voor **Computer Vision S1** van toepassing.

1. Selecteer **maken** om de nieuwe Cognitive Services resource in te richten.

1. Ga terug naar het vorige tabblad, dat de wizard gegevens importeren bevat. Selecteer **vernieuwen** om de Cognitive Services resource weer te geven en selecteer vervolgens de resource:

   ![De Cognitive Services resource selecteren](./media/cognitive-search-attach-cognitive-services/attach2.png "De Cognitive Services resource selecteren")

1. Vouw de sectie **cognitieve vaardig heden toevoegen** uit om de specifieke cognitieve vaardig heden te selecteren die u wilt uitvoeren op uw gegevens. Voltooi de rest van de wizard.

## <a name="attach-an-existing-skillset-to-a-cognitive-services-resource"></a>Een bestaande vaardig heden koppelen aan een Cognitive Services resource

Als u een bestaande vaardig heden hebt, kunt u deze koppelen aan een nieuwe of andere Cognitive Services resource.

1. Selecteer op de pagina **service overzicht** de optie **vaardig heden**:

   ![Tabblad vaardig heden](./media/cognitive-search-attach-cognitive-services/attach-existing1.png "Tabblad vaardig heden")

1. Selecteer de naam van de vaardig heden en selecteer vervolgens een bestaande resource of maak een nieuwe. Selecteer **OK** om uw wijzigingen te bevestigen.

   ![Lijst met vaardig heden-resources](./media/cognitive-search-attach-cognitive-services/attach-existing2.png "Lijst met vaardig heden-resources")

   Houd er rekening mee dat de optie **gratis (beperkte verrijkingen)** u per dag beperkt tot 20 documenten en dat u **nieuwe Cognitive Services resource maken** kunt gebruiken om een nieuwe factureer bare resource in te richten. Als u een nieuwe resource maakt, selecteert u **vernieuwen** om de lijst met Cognitive services resources te vernieuwen en selecteert u vervolgens de resource.

## <a name="attach-cognitive-services-programmatically"></a>Cognitive Services via een programma koppelen

Wanneer u de vaardig heden programmatisch definieert, voegt u een `cognitiveServices` sectie toe aan de vaardig heden. In dat gedeelte moet u de sleutel van de Cognitive Services resource die u wilt koppelen aan de vaardig heden, toevoegen. Houd er rekening mee dat de resource zich in dezelfde regio als uw Azure Cognitive Search-resource moet bevinden. Neem ook `@odata.type`op en stel deze in op `#Microsoft.Azure.Search.CognitiveServicesByKey`.

In het volgende voor beeld ziet u dit patroon. U ziet de `cognitiveServices` sectie aan het einde van de definitie.

```http
PUT https://[servicename].search.windows.net/skillsets/[skillset name]?api-version=2019-05-06
api-key: [admin key]
Content-Type: application/json
```
```json
{
    "name": "skillset name",
    "skills": 
    [
      {
        "@odata.type": "#Microsoft.Skills.Text.EntityRecognitionSkill",
        "categories": [ "Organization" ],
        "defaultLanguageCode": "en",
        "inputs": [
          {
            "name": "text", "source": "/document/content"
          }
        ],
        "outputs": [
          {
            "name": "organizations", "targetName": "organizations"
          }
        ]
      }
    ],
    "cognitiveServices": {
        "@odata.type": "#Microsoft.Azure.Search.CognitiveServicesByKey",
        "description": "mycogsvcs",
        "key": "<your key goes here>"
    }
}
```

## <a name="example-estimate-costs"></a>Voor beeld: kosten schatten

Als u een schatting wilt maken van de kosten die zijn gekoppeld aan het indexeren van cognitieve Zoek opdrachten, begint u met een idee van wat een gemiddeld document lijkt te zijn, zodat u enkele getallen kunt uitvoeren. U kunt bijvoorbeeld ongeveer het volgende doen:

+ 1\.000 Pdf's.
+ Zes pagina's per.
+ Eén afbeelding per pagina (6.000 afbeeldingen).
+ 3\.000 tekens per pagina.

Stel dat u een pijp lijn hebt die bestaat uit het kraken van een document van elke PDF, extractie van afbeeldingen en tekst, optische teken herkenning (OCR) van afbeeldingen en entiteits herkenning van organisaties.

De prijzen die in dit artikel worden weer gegeven, zijn hypothetisch. Ze worden gebruikt om het schattings proces te illustreren. De kosten zijn lager. Zie [Cognitive Services prijzen](https://azure.microsoft.com/pricing/details/cognitive-services)voor de werkelijke prijzen van trans acties.

1. Voor het kraken van documenten met tekst-en afbeeldings inhoud is tekst extractie momenteel gratis. Voor 6.000 installatie kopieën wordt aangenomen dat $1 voor elke 1.000 installatie kopieën worden geëxtraheerd. Dat is een prijs van $6,00 voor deze stap.

2. Voor OCR van 6.000-afbeeldingen in het Engels gebruikt de OCR cognitieve vaardigheid het beste algoritme (DescribeText). Uitgaande van een kosten van $2,50 per 1.000 installatie kopieën, betaalt u $15,00 voor deze stap.

3. Voor het uitpakken van de entiteit hebt u per pagina in totaal drie tekst records. Elke record is 1.000 tekens. Drie tekst records per pagina vermenigvuldigd met 6.000 pagina's is gelijk aan 18.000 tekst records. Als er $2,00 per 1.000-tekst records worden berekend, wordt deze stap $36,00.

U betaalt ongeveer $57,00 tot opname van 1.000 PDF-documenten van dit type met de beschreven vaardig heden.

## <a name="next-steps"></a>Volgende stappen
+ [Pagina met prijzen voor Azure Cognitive Search](https://azure.microsoft.com/pricing/details/search/)
+ [Een vaardig heden definiëren](cognitive-search-defining-skillset.md)
+ [Vaardig heden maken (REST)](https://docs.microsoft.com/rest/api/searchservice/create-skillset)
+ [Verrijkte velden toewijzen](cognitive-search-output-field-mapping.md)
