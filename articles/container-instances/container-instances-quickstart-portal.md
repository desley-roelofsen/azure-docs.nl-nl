---
title: Snelstartgids-docker-container implementeren naar container exemplaar-Portal
description: In deze Quick Start gebruikt u de Azure Portal om snel een in een container geplaatste web-app te implementeren die wordt uitgevoerd in een geïsoleerd Azure-container exemplaar
services: container-instances
author: dlepow
manager: gwallace
ms.service: container-instances
ms.topic: quickstart
ms.date: 04/17/2019
ms.author: danlep
ms.custom: seodec18, mvc
ms.openlocfilehash: e0c5ba57c7664a64c1b11bed215f419f31630d39
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533534"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-azure-portal"></a>Snelstartgids: een container exemplaar in azure implementeren met behulp van de Azure Portal

Gebruik Azure Container Instances om serverloze docker-containers in azure uit te voeren met eenvoud en snelheid. Implementeer een toepassing op een container exemplaar op aanvraag wanneer u geen volledig container Orchestration-platform zoals Azure Kubernetes service nodig hebt.

In deze Quick Start gebruikt u de Azure Portal om een geïsoleerde docker-container te implementeren en de toepassing beschikbaar te maken met een Fully Qualified Domain Name (FQDN). Na het configureren van een paar instellingen en de implementatie van de container kunt u bladeren naar de toepassing die wordt uitgevoerd:

![App die is geïmplementeerd in Azure Container Instances, weergegeven in de browser][aci-portal-07]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u aan bij Azure Portal op https://portal.azure.com.

Als u nog geen abonnement op Azure hebt, maak dan een [gratis account][azure-free-account] aan voordat u begint.

## <a name="create-a-container-instance"></a>Een containerinstantie maken

Selecteer **Een nieuwe resource maken** > **Containers** > **Container Instances**.

![Begin met het maken van een nieuwe containerinstantie in Azure Portal][aci-portal-01]

Voer op de pagina **basis beginselen** de volgende waarden in de tekst vakken **resource groep**, **container naam**en **container installatie kopie** in. Laat de andere waarden op de standaardwaarden staan en selecteer vervolgens **OK**.

* Resourcegroep: **Nieuwe maken** > `myresourcegroup`
* Containernaam: `mycontainer`
* Containerinstallatiekopie: `mcr.microsoft.com/azuredocs/aci-helloworld`

![Basisinstellingen configureren voor een nieuwe containerinstantie in Azure Portal][aci-portal-03]

Voor deze Quick Start gebruikt u de standaard instelling voor het **type installatie kopie** van **openbaar** voor het implementeren van de open bare micro soft `aci-helloworld`-installatie kopie. Deze Linux-installatie kopie verpakt een kleine web-app die is geschreven in node. js, die een statische HTML-pagina vormt.

Geef op de pagina **netwerk** een **DNS-naam label** voor uw container op. De naam moet uniek zijn binnen de Azure-regio waarin u het container exemplaar maakt. De container zal openbaar bereikbaar zijn op `<dns-name-label>.<region>.azurecontainer.io`. Als u een foutbericht 'DNS-naamlabel niet beschikbaar' ontvangt, probeert u een ander DNS-naamlabel.

![Een nieuwe containerinstantie configureren in Azure Portal][aci-portal-04]

Wijzig de standaard instellingen en selecteer vervolgens **controleren + maken**.

Wanneer de validatie is voltooid, ziet u een overzicht van de containerinstellingen. Selecteer **maken** om uw container implementatie aanvraag in te dienen.

![Overzicht van de instellingen voor een nieuwe containerinstantie in Azure Portal][aci-portal-05]

Wanneer de implementatie is gestart, wordt een melding weergegeven dat de implementatie wordt uitgevoerd. Er wordt nog een melding weergegeven wanneer de containergroep is geïmplementeerd.

Open het overzicht voor de container groep door te navigeren naar **resource groepen** > **myresourcegroup** > **mycontainer**. Noteer de **FQDN** (de FQDN-naam) van de container-instantie, evenals de **Status**.

![Overzicht van containergroepen in Azure Portal][aci-portal-06]

Wanneer de **Status** eenmaal *Uitvoeren* is, gaat u naar de FQDN van de container in uw browser.

![App die is geïmplementeerd met Azure Container Instances, weergegeven in de browser][aci-portal-07]

Gefeliciteerd! Als u slechts enkele instellingen configureert, hebt u een openbaar toegankelijke toepassing in Azure Container Instances geïmplementeerd.

## <a name="view-container-logs"></a>Containerlogboeken ophalen

Het weergeven van de logboeken voor een exemplaar van de container is handig bij het oplossen van problemen met de container of de toepassing die wordt uitgevoerd.

Selecteer om de container-logboeken weer te geven onder **Instellingen** de optie **Containers** en vervolgens **Logboeken**. De HTTP GET-aanvraag wordt als het goed is gegenereerd wanneer u de toepassing in uw browser hebt bekeken.

![Container-logboeken in Azure Portal][aci-portal-11]

## <a name="clean-up-resources"></a>Resources opschonen

Wanneer u met de container klaar bent, selecteert u **Overzicht** voor de container-instantie *mycontainer* en vervolgens **Verwijderen**.

![De containerinstantie verwijderen in de Azure-portal][aci-portal-09]

Selecteer **Ja** als het bevestigingsvenster verschijnt.

![Een containerinstantie in de Azure-portal verwijderen][aci-portal-10]

## <a name="next-steps"></a>Volgende stappen

In deze Quick Start hebt u een Azure-container exemplaar gemaakt op basis van een open bare micro soft-installatie kopie. Als u zelf een containerinstallatiekopie wilt bouwen en deze wilt implementeren met behulp van een privé Azure Container-register, gaat u verder met de zelfstudie voor Azure Container Instances.

> [!div class="nextstepaction"]
> [Zelfstudie voor Azure Container Instances](./container-instances-tutorial-prepare-app.md)

<!-- IMAGES -->
[aci-portal-01]: ./media/container-instances-quickstart-portal/qs-portal-01.png
[aci-portal-03]: ./media/container-instances-quickstart-portal/qs-portal-03.png
[aci-portal-04]: ./media/container-instances-quickstart-portal/qs-portal-04.png
[aci-portal-05]: ./media/container-instances-quickstart-portal/qs-portal-05.png
[aci-portal-06]: ./media/container-instances-quickstart-portal/qs-portal-06.png
[aci-portal-07]: ./media/container-instances-quickstart-portal/qs-portal-07.png
[aci-portal-08]: ./media/container-instances-quickstart-portal/qs-portal-08.png
[aci-portal-09]: ./media/container-instances-quickstart-portal/qs-portal-09.png
[aci-portal-10]: ./media/container-instances-quickstart-portal/qs-portal-10.png
[aci-portal-11]: ./media/container-instances-quickstart-portal/qs-portal-11.png

<!-- LINKS - External -->
[azure-free-account]: https://azure.microsoft.com/free/