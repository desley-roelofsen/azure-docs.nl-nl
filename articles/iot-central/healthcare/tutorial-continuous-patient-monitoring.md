---
title: Een voortdurende app voor het controleren van patiënten maken met Azure IoT Central | Microsoft Docs
description: Leer hoe u een continue patiënten-bewakings toepassing maakt met behulp van Azure IoT Central-toepassings sjablonen.
author: philmea
ms.author: philmea
ms.date: 09/24/2019
ms.topic: tutorial
ms.service: iot-central
services: iot-central
manager: eliotgra
ms.openlocfilehash: 97a215d8f111753c8fcc857fe4c48956c1236b3b
ms.sourcegitcommit: d47a30e54c5c9e65255f7ef3f7194a07931c27df
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73027450"
---
# <a name="tutorial-deploy-and-walkthrough-a-continuous-patient-monitoring-app-template"></a>Zelf studie: een sjabloon voor het controleren van doorlopende patiënten implementeren en bewaken

[!INCLUDE [iot-central-pnp-original](../../../includes/iot-central-pnp-original-note.md)]

In deze zelf studie ziet u, als een oplossings functie, hoe u aan de slag kunt gaan door een IoT Central continue patiënten-toepassings sjabloon te implementeren. U leert hoe u de sjabloon implementeert, wat wordt opgenomen in het vak en wat u mogelijk wilt doen.

In deze zelfstudie leert u het volgende:

> [!div class="checklist"]
> * Een toepassings sjabloon maken
> * De toepassings sjabloon door lopen

## <a name="create-an-application-template"></a>Een toepassings sjabloon maken

Ga naar de [website van Azure IOT Central Application Manager](https://apps.azureiotcentral.com/). Selecteer **samen stellen** in de navigatie balk aan de linkerkant en klik vervolgens op het tabblad **gezondheids zorg** . 

>[!div class="mx-imgBorder"] 
>![gezondheids zorg](media/app-manager-health.png) app manager

Klik op de knop **app maken** om de toepassing te gaan maken en meld u aan met een persoonlijk, werk-of school account van micro soft. Hiermee gaat u naar de **nieuwe toepassings** pagina.

>[!div class="mx-imgBorder"] 
>![gezondheids zorg](media/app-manager-health-create.png) maken

Uw toepassing maken:

1. In azure IoT Central wordt automatisch een toepassings naam voorgesteld op basis van de sjabloon die u hebt geselecteerd. U kunt deze naam accepteren of uw eigen beschrijvende toepassings naam invoeren, zoals een **voortdurende patiënt bewaking**. Azure IoT Central genereert ook een uniek URL-voor voegsel voor u op basis van de naam van de toepassing. Als u wilt, kunt u dit URL-voor voegsel wijzigen in iets eenvoudiger te onthouden.

2. U kunt aangeven of u een **proef** toepassing of een toepassing met **betalen per gebruik** wilt maken. **Proef** toepassingen zijn zeven dagen gratis beschikbaar voordat ze verlopen en Maxi maal vijf gratis apparaten toestaan. Voordat ze verlopen, kunnen ze op elk gewenst moment worden geconverteerd naar betalen per gebruik. Als u een proef toepassing maakt, moet u uw contact gegevens invoeren en kiezen of u informatie en tips van micro soft wilt ontvangen. **Betalen per gebruik** -toepassingen bieden ondersteuning voor Maxi maal twee gratis apparaten en u moet uw Azure-abonnements gegevens invoeren.

3. Selecteer onder aan de pagina **maken** om uw toepassing te implementeren.

## <a name="walk-through-the-application-template"></a>De toepassings sjabloon door lopen

### <a name="dashboards"></a>Dashboards

Na de implementatie van de app-sjabloon gaat u eerst naar het **Lamna in de gaten**. Lamna gezondheids zorg is een fictief ziekenhuis systeem dat twee zieken huizen bevat: Woodgrove ziekenhuis en Burkville zieken huis. Op dit dash board van de operator voor Woodgrove ziekenhuis ziet u informatie en telemetrie over de apparaten in deze sjabloon, samen met een set opdrachten, taken en acties die u kunt uitvoeren. Vanuit het dash board kunt u het volgende doen:

* Zie telemetrie van apparaten en eigenschappen, zoals het **accu niveau** van het apparaat of de **connectiviteits** status.

* Bekijk het **vloer plan** en de locatie van het Smart vitale patch-apparaat.

* De Smart vitale patch **opnieuw inrichten** voor een nieuwe patiënt.

* Bekijk een voor beeld van een **provider dashboard** dat een ziekenhuis Care team kan zien om hun patiënten bij te houden.

* Wijzig de **status** van de patiënt van uw apparaat om aan te geven of het apparaat wordt gebruikt voor een in-patiënten of extern scenario.

>[!div class="mx-imgBorder"] 
>![Lamna in-patiënten](media/lamna-in-patient.png)

U kunt ook klikken op **naar het dash board van de externe patiënt** om het tweede operator dashboard te zien dat voor Burkville ziekenhuis wordt gebruikt. Dit dash board bevat een soort gelijke set acties, telemetrie en informatie. Bovendien ziet u dat er meerdere apparaten worden gebruikt en de mogelijkheid hebben om **de firmware** op elk apparaat bij te werken.

>[!div class="mx-imgBorder"] 
>Lamna](media/lamna-remote.png) externe ![

Op beide Dash boards kunt u altijd een koppeling naar deze documentatie maken.

### <a name="device-templates"></a>Apparaatinstellingen

Als u op het tabblad **device templates** klikt, ziet u dat er twee verschillende typen apparaten zijn die deel uitmaken van de sjabloon:

* **Smart vitale patch**: dit apparaat vertegenwoordigt een patch die een groot aantal vitale tekens meet die kunnen worden gebruikt voor het bewaken van patiënten in en buiten het zieken huis. Als u op de sjabloon klikt, ziet u dat naast het verzenden van apparaatgegevens, zoals het accu niveau en de Tempe ratuur van het apparaat, ook de status gegevens van patiënten, zoals het Ademhalings tempo en de bloed druk, worden verzonden.

* **Krul haak**: dit apparaat vertegenwoordigt een knie accolade die patiënten mogelijk gebruiken bij het herstellen van een chirurgische. Als u op deze sjabloon klikt, worden de mogelijkheden, zoals het aantal bewegingen en de versnelling, weer gegeven naast de apparaatgegevens.

>[!div class="mx-imgBorder"] 
>![Smart vitale patch Device-sjabloon](media/smart-vitals-device-template.png)

Als u op het tabblad **Apparaatgroepen** klikt, ziet u ook dat voor deze Apparaatinstellingen automatisch apparaatgroepen worden gemaakt.

### <a name="rules"></a>Regels

Wanneer u naar het tabblad regels gaat, ziet u drie regels die voor komen in de toepassings sjabloon:

* **Hoge beugel-Tempe ratuur**: deze regel wordt geactiveerd wanneer de apparaats temperatuur van de slimme knie accolade groter is dan 95&deg;F over een periode van vijf minuten. U kunt deze regel gebruiken om het patiënten-en Care team te waarschuwen en het apparaat op afstand af te koelen.

* **Herfst gedetecteerd**: deze regel wordt geactiveerd als er een patiënt wordt gedetecteerd. U kunt deze regel gebruiken om een actie te configureren voor het implementeren van een operationeel team om de patiënt te helpen die zich heeft voordoen.

* **Accu laag patch**: deze regel wordt geactiveerd wanneer het accu niveau op het apparaat lager is dan 10%. U kunt deze regel gebruiken om een melding naar de patiënt te activeren om het apparaat in rekening te brengen.

>[!div class="mx-imgBorder"] 
>![accolade met hoge regel](media/brace-temp-rule.png)

### <a name="devices"></a>Apparaten

Klik op het tabblad **apparaten** en selecteer vervolgens een exemplaar van de **slimme knie accolade**. U ziet dat er drie weer gaven zijn om informatie over het geselecteerde apparaat dat u hebt geselecteerd, te kunnen verkennen. Deze weer gaven worden gemaakt en gepubliceerd bij het bouwen van de sjabloon voor het apparaat voor uw apparaat. Dit betekent dat ze consistent zijn op alle apparaten waarmee u verbinding maakt of simuleert.

In de **Dashboard** weergave wordt een overzicht gegeven van de telemetrie en de eigenschappen die afkomstig zijn van het apparaat waarop de operator georiënteerd is.

Op het tabblad **Eigenschappen** kunt u de eigenschappen van de Cloud en het lezen/schrijven van apparaten bewerken.

Op het tabblad **opdrachten** kunt u opdrachten uitvoeren die zijn gemodelleerd als onderdeel van de sjabloon voor het apparaat.

>[!div class="mx-imgBorder"] 
>![-accolades weer geven](media/knee-brace-dashboard.png)

## <a name="clean-up-resources"></a>Resources opschonen

Als u deze toepassing niet meer wilt gebruiken, verwijdert u de toepassing door naar **beheer > toepassings instellingen** te gaan en op **verwijderen**te klikken.

>[!div class="mx-imgBorder"] 
>app-](media/admin-delete.png) ![verwijderen

## <a name="next-steps"></a>Volgende stappen

Ga naar het volgende artikel voor meer informatie over het maken van een provider dashboard dat verbinding maakt met uw IoT Central-toepassing.

> [!div class="nextstepaction"]
> [Een provider dashboard bouwen](howto-health-data-triage.md)