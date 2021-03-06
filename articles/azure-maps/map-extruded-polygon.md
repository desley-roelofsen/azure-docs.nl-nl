---
title: Een veelhoek met een 3D-laag toevoegen aan Azure Maps | Microsoft Docs
description: Een polygoon laag voor 3D toevoegen aan de Azure Maps Web-SDK.
author: walsehgal
ms.author: v-musehg
ms.date: 10/08/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: e6858359549f6a54513eda7bc692adcbc7d7e71b
ms.sourcegitcommit: 8cf199fbb3d7f36478a54700740eb2e9edb823e8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/25/2019
ms.locfileid: "74484341"
---
# <a name="add-an-extrusion-polygon-layer-to-the-map"></a>Een polygoon laag met extrusie toevoegen aan de kaart

In dit artikel leest u hoe u de laag met de polygoon extrusie kunt gebruiken om gebieden van `Polygon` en `MultiPolygon` functie geometrie te renderen als geëxtrudeerde vormen op de kaart. De websdk van Azure Maps biedt ook ondersteuning voor het maken van cirkel geometrie zoals gedefinieerd in het [uitgebreide GEOjson-schema](extend-geojson.md#circle). Deze cirkels worden omgezet in veelhoeken wanneer ze op de kaart worden weer gegeven. Alle functie-geometrieën kunnen ook eenvoudig worden bijgewerkt als deze met de [Atlas wordt verpakt. ](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.shape?view=azure-iot-typescript-latest)Klasse van vorm.


## <a name="use-a-polygon-extrusion-layer"></a>Een laag met een polygoon effect gebruiken

Wanneer een [laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest) met het 3D-effect is verbonden met de gegevens bron en op de kaart wordt geladen, worden de gebieden van een `Polygon` en `MultiPolygon` functies als geëxtrudeerde vormen weer gegeven. De eigenschappen `height` en `base` van de laag met het polygoon 3D-effect bepalen de basis afstand van de grond en hoogte van de geëxtrudeerde vorm in **meters**. De volgende code laat zien hoe u een veelhoek maakt, voegt deze toe aan een gegevens bron en geeft deze weer met behulp van de klasse van de polygoon laag.

> [!Note]
> De `base` waarde die is gedefinieerd in de Layer extrusie laag moet kleiner zijn dan of gelijk zijn aan die van de `height`.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Geëxtrudeerde veelhoek" src="https://codepen.io/azuremaps/embed/wvvBpvE?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bekijk de pen <a href='https://codepen.io/azuremaps/pen/wvvBpvE'>geëxtrudeerde veelhoek</a> per Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.</iframe>


## <a name="add-data-driven-multipolygons"></a>Op gegevens gebaseerde multiveelhoeken toevoegen

Een choropleth-kaart kan worden weer gegeven met de laag 3D-extrusie door de `height` en `fillColor` eigenschappen in te stellen op basis van de meting van de statistische variabele in de `Polygon` en `MultiPolygon` functie geometrie. Het volgende code voorbeeld toont een geëxtrudeerde choropleth toewijzing van de U. S op basis van de meting van de populatie densiteit per status.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Geëxtrudeerde choropleth-kaart" src="https://codepen.io/azuremaps/embed/eYYYNox?height=265&theme-id=0&default-tab=result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Zie het <a href='https://codepen.io/azuremaps/pen/eYYYNox'>3D-overzicht</a> van de pen, gesorteerd op Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-a-circle-to-the-map"></a>Een cirkel aan de kaart toevoegen

Azure Maps maakt gebruik van een uitgebreide versie van het geojson-schema met een definitie voor cirkels zoals [hier](https://docs.microsoft.com/azure/azure-maps/extend-geojson#circle)wordt beschreven. Een 3D-cirkel kan worden weer gegeven op de kaart door een `point`-functie te maken met de eigenschap `subType` van `Circle` en een genummerde `Radius` eigenschap die de RADIUS in **meters**vertegenwoordigt. Bijvoorbeeld:

```Javascript
{
    "type": "Feature",
    "geometry": {
        "type": "Point",
        "coordinates": [-105.203135, 39.664087]
    },
    "properties": {
        "subType": "Circle",
        "radius": 1000
    }
} 
```

Met de Azure Maps Web-SDK worden deze `Point` functies geconverteerd naar `Polygon` functies onder de motorkap en kunnen ze worden weer gegeven op de kaart met behulp van de Layer extrusie-laag zoals wordt weer gegeven in het volgende code voorbeeld.

<br/>

<iframe height="500" style="width: 100%;" scrolling="no" title="Drone-lucht ruimte veelhoek" src="https://codepen.io/azuremaps/embed/zYYYrxo?height=265&theme-id=0&default-tab=js,result&editable=true" frameborder="no" allowtransparency="true" allowfullscreen="true">
Bekijk de pen <a href='https://codepen.io/azuremaps/pen/zYYYrxo'>drone lucht spatie veelhoek</a> per Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="customize-a-polygon-extrusion-layer"></a>Een laag met een polygoon effect aanpassen

De laag met het polygoon reliëf heeft verschillende stijlen. Hier volgt een hulp programma om het uit te proberen.

<br/>

<iframe height='700' scrolling='no' title='PoogBRJ' src='//codepen.io/azuremaps/embed/PoogBRJ/?height=700&theme-id=0&default-tab=result' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/PoogBRJ/'>PoogBRJ</a> by Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [Polygoon](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.data.polygon?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [polygoon-3D-laag](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.layer.polygonextrusionlayer?view=azure-maps-typescript-latest)

Aanvullende bronnen:

> [!div class="nextstepaction"]
> [Uitbrei ding voor geojson-specificatie Azure Maps](extend-geojson.md#circle)
