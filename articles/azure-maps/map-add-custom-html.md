---
title: Een HTML-markering toevoegen aan Azure Maps | Microsoft Docs
description: Een HTML-markering toevoegen aan de Azure Maps Web-SDK.
author: jingjing-z
ms.author: jinzh
ms.date: 07/29/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: ''
ms.custom: codepen
ms.openlocfilehash: ff18b8646a3845d029e9826c0895b9d3eb16bdde
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/13/2019
ms.locfileid: "68976207"
---
# <a name="add-html-markers-to-the-map"></a>HTML-markeringen toevoegen aan de kaart

Dit artikel laat u zien hoe u een aangepaste HTML, zoals een afbeeldings bestand, aan de kaart kunt toevoegen als een HTML-markering.

> [!NOTE]
> HTML-markeringen maken geen verbinding met gegevens bronnen. Plaatsings gegevens worden direct toegevoegd aan de markering en de markering wordt toegevoegd aan de eigenschap `markers` Maps van een [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest).

> [!IMPORTANT]
> In tegens telling tot de meeste lagen in de Azure Maps web control die WebGL gebruiken voor rendering, gebruiken HTML-markeringen traditionele DOM-elementen voor rendering. Hoe meer HTML-markeringen een pagina heeft toegevoegd, hoe meer DOM-elementen er zijn. Prestaties kunnen afnemen nadat u een paar honderd HTML-markeringen hebt toegevoegd. Voor grotere gegevens sets kunt u het clusteren van uw gegevens of het gebruik van een symbool of een Bubble laag.

## <a name="add-an-html-marker"></a>Een HTML-markering toevoegen

De [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest) -klasse heeft een standaard stijl. U kunt de markering aanpassen door de kleur-en tekst opties van de markering in te stellen. De standaard stijl van de klasse HTML-markering is een SVG-sjabloon met `{color}` een `{text}` en een tijdelijke aanduiding. Stel de kleur-en tekst eigenschappen in de HTML-markerings opties in voor een snelle aanpassing. 

Met de volgende code wordt een HTML-markering gemaakt en wordt de eigenschap Color ingesteld op ' DodgerBlue ' en de eigenschap Text op ' 10 '. Er wordt een pop-upvenster gekoppeld `click` aan de markering en de gebeurtenis wordt gebruikt om de zicht baarheid van de pop-up te scha kelen.

```javascript
//Create a HTML marker and add it to the map.
var marker = new atlas.HtmlMarker({
    color: 'DodgerBlue',
    text: '10',
    position: [0, 0],
    popup: new atlas.Popup({
        content: '<div style="padding:10px">Hello World</div>',
        pixelOffset: [0, -30]
    })
});

map.markers.add(marker);

//Add a click event to toggle the popup.
map.events.add('click',marker, () => {
    marker.togglePopup();
});
```

Hieronder ziet u het volledige programma voor het uitvoeren van code van de bovenstaande functionaliteit.

<br/>

<iframe height='500' scrolling='no' title='Een HTML-markering aan een kaart toevoegen' src='//codepen.io/azuremaps/embed/MVoeVw/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/MVoeVw/'>een HTML-markering toevoegen aan een kaart</a> door Azure Maps<a href='https://codepen.io/azuremaps'>@azuremaps</a>() op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="create-svg-templated-html-marker"></a>Een HTML-markering met SVG-sjabloon maken

De standaard `htmlContent` waarde van een HTML-markering is een SVG-sjabloon `{color}` met `{text}` mappen en daarin plaatsen. U kunt aangepaste SVG-teken reeksen maken en dezelfde tijdelijke aanduidingen toevoegen aan uw SVG, zodat `color` u `text` deze tijdelijke aanduidingen in uw SVG-opties instelt.

<br/>

<iframe height='500' scrolling='no' title='HTML-markering met aangepaste SVG-sjabloon' src='//codepen.io/azuremaps/embed/LXqMWx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/LXqMWx/'>HTML-markering van de pen met aangepaste SVG-sjabloon</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

> [!TIP]
> De Azure Maps Web-SDK biedt verschillende SVG-afbeeldings sjablonen die kunnen worden gebruikt met HTML-markeringen. Zie het document [Image-sjablonen gebruiken](how-to-use-image-templates-web-sdk.md) voor meer informatie.

## <a name="add-a-css-styled-html-marker"></a>Een HTML-markering met CSS-stijl toevoegen

Een van de voor delen van HTML-markeringen is dat er veel fantastische aanpassingen kunnen worden gerealiseerd met behulp van CSS. In dit voor beeld bestaat de inhoud van de HtmlMarker uit HTML en CSS waarmee een geanimeerde pincode wordt gemaakt die in plaats van en pulsen valt.

<br/>

<iframe height='500' scrolling='no' title='HTML-gegevens bron' src='//codepen.io/azuremaps/embed/qJVgMx/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de <a href='https://codepen.io/azuremaps/pen/qJVgMx/'>HTML-gegevens bron</a> van de<a href='https://codepen.io/azuremaps'>@azuremaps</a>pen door Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="draggable-html-markers"></a>Versleep bare HTML-markeringen

Dit voor beeld laat zien hoe u een HTML-markering kunt slepen. HTML-markeringen ondersteunen `drag`, `dragstart`en `dragend` gebeurtenissen.

<br/>

<iframe height='500' scrolling='no' title='Gesleepte HTML-markering' src='//codepen.io/azuremaps/embed/wQZoEV/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de gesleepte <a href='https://codepen.io/azuremaps/pen/wQZoEV/'>HTML-markering</a> van de<a href='https://codepen.io/azuremaps'>@azuremaps</a>pen door Azure Maps () op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="add-mouse-events-to-html-markers"></a>Muis gebeurtenissen toevoegen aan HTML-markeringen

In deze voor beelden ziet u hoe u muis toevoegen en gebeurtenissen sleept naar een HTML-markering.

<br/>

<iframe height='500' scrolling='no' title='Muis gebeurtenissen toevoegen aan HTML-markeringen' src='//codepen.io/azuremaps/embed/RqOKRz/?height=500&theme-id=0&default-tab=js,result&embed-version=2&editable=true' frameborder='no' allowtransparency='true' allowfullscreen='true' style='width: 100%;'>Zie de pen <a href='https://codepen.io/azuremaps/pen/RqOKRz/'>muis gebeurtenissen toevoegen aan HTML-markeringen</a> door Azure Maps (<a href='https://codepen.io/azuremaps'>@azuremaps</a>) op <a href='https://codepen.io'>CodePen</a>.
</iframe>

## <a name="next-steps"></a>Volgende stappen

Meer informatie over de klassen en methoden die in dit artikel worden gebruikt:

> [!div class="nextstepaction"]
> [HtmlMarker](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarker?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerOptions](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkeroptions?view=azure-iot-typescript-latest)

> [!div class="nextstepaction"]
> [HtmlMarkerManager](https://docs.microsoft.com/javascript/api/azure-maps-control/atlas.htmlmarkermanager?view=azure-iot-typescript-latest)

Raadpleeg de volgende artikelen voor meer code voorbeelden om toe te voegen aan uw Maps:

> [!div class="nextstepaction"]
> [Afbeeldings sjablonen gebruiken](how-to-use-image-templates-web-sdk.md)

> [!div class="nextstepaction"]
> [Een symbool laag toevoegen](./map-add-pin.md)

> [!div class="nextstepaction"]
> [Een Bubble laag toevoegen](./map-add-bubble-layer.md)