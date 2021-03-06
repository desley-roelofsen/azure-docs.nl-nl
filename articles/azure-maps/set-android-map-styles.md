---
title: Stijl functionaliteiten toewijzen in Azure Maps | Microsoft Docs
description: Meer informatie over het Azure Maps van stijl functionaliteit voor Android SDK.
author: walsehgal
ms.author: v-musehg
ms.date: 04/26/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 5bcc73c63e7c9d804d01df98551aa51b81d98d07
ms.sourcegitcommit: adc1072b3858b84b2d6e4b639ee803b1dda5336a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/10/2019
ms.locfileid: "70844846"
---
# <a name="set-map-style-using-azure-maps-android-sdk"></a>Kaart stijl instellen met Azure Maps Android SDK

In dit artikel ziet u twee manieren om kaart stijlen in te stellen met behulp van de Azure Maps Android SDK. Azure Maps heeft zes verschillende kaarten stijlen waaruit u kunt kiezen. Zie [ondersteunde kaart stijlen in azure Maps](./supported-map-styles.md)voor meer informatie over ondersteunde kaart stijlen.


## <a name="prerequisites"></a>Vereisten

Om het proces in dit artikel te volt ooien, moet u [Azure Maps ANDROID SDK](https://docs.microsoft.com/azure/azure-maps/how-to-use-android-map-control-library) installeren om een kaart te laden.


## <a name="set-map-style-in-the-layout"></a>Kaart stijl instellen in de lay-out

U kunt een kaart stijl instellen in het indelings bestand voor uw activiteiten klasse. Wijzig de **indeling res > > activity_main. XML**, zodat deze er als volgt uitziet:

```XML
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    >

    <com.microsoft.azure.maps.mapcontrol.MapControl
        android:id="@+id/mapcontrol"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:mapcontrol_centerLat="47.602806"
        app:mapcontrol_centerLng="-122.329330"
        app:mapcontrol_zoom="12"
        app:mapcontrol_style="grayscale_dark"
        />

</FrameLayout>
```

Met `mapcontrol_style` het bovenstaande kenmerk stelt u de kaart stijl in op **grayscale_dark**. 

<center>

![stijl-grayscale_dark](./media/set-android-map-styles/grayscale-dark.png)</center>

## <a name="set-map-style-in-the-activity-class"></a>Kaart stijl instellen in de klasse activiteit

Kaart stijl kan worden ingesteld in de klasse activity. Kopieer het volgende code fragment in de methode **onCreate ()** van uw `MainActivity.java` klasse. Hiermee wordt de kaart stijl ingesteld op **satellite_road_labels**.

```Java
mapControl.onReady(map -> {
    //Set the camera of the map.
    map.setCamera(center(47.64, -122.33), zoom(14));

    //Set the style of the map.
    map.setStyle(style(MapStyle.SATELLITE));
});
```

<center>

![style-satellite-road-labels](./media/set-android-map-styles/satellite-road-labels.png)</center>