---
title: Beheerde container registers
description: Kennismaking met de Azure Container Registry-service, waarmee u cloudgebaseerde, beheerde en persoonlijke Docker-registers kunt maken.
author: stevelas
ms.topic: overview
ms.date: 06/28/2019
ms.author: stevelas
ms.custom: seodec18, mvc
ms.openlocfilehash: 2ceae0a6d6eb4dc989a53b35dc4a2f64472a5f54
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74892971"
---
# <a name="introduction-to-private-docker-container-registries-in-azure"></a>Inleiding tot privé-Docker-containerregisters in Azure

Azure Container Registry is een beheerde, persoonlijke docker-register service op basis van het open source-docker-REGI ster 2,0. Azure-container registers maken en onderhouden om uw persoonlijke docker-container installatie kopieën op te slaan en te beheren.

Gebruik Azure-container registers met uw bestaande pijp lijnen voor het ontwikkelen en implementeren van containers, of gebruik Azure Container Registry taken om container installatie kopieën in azure te maken. Bouw op aanvraag of volledig Automatiseer builds met triggers, zoals het door voeren van de bron code en updates van de basis installatie kopie.

Zie voor meer informatie over docker-en register concepten het [docker-overzicht](https://docs.docker.com/engine/docker-overview/) en [over registers, opslag plaatsen en installatie kopieën](container-registry-concepts.md).

## <a name="use-cases"></a>Use cases

Haal installatiekopieën op vanuit een Azure-containerregister en push ze naar verschillende implementatiedoelen:

* **Schaalbare indelingssystemen** die toepassingen in een container beheren in hostclusters, waaronder [Kubernetes](https://kubernetes.io/docs/), [DC/OS](https://docs.mesosphere.com/) en [Docker Swarm](https://docs.docker.com/swarm/).
* **Azure-services** die het bouwen en uitvoeren van toepassingen op schaal ondersteunen, waaronder [Azure Kubernetes Service (AKS)](../aks/index.yml), [App Service](../app-service/index.yml), [Batch](../batch/index.yml), [Service Fabric](/azure/service-fabric/) en andere.

Ontwikkelaars kunnen ook naar een containerregister pushen als onderdeel van een ontwikkelingswerkstroom met containers. Als u bijvoorbeeld een container register wilt richten op een continue integratie-en leverings hulp programma, zoals [Azure-pijp lijnen](/azure/devops/pipelines/ecosystems/containers/acr-template) of [Jenkins](https://jenkins.io/).

Configureer ACR-taken zodanig dat toepassings installatie kopieën automatisch opnieuw worden opgebouwd wanneer hun basis installatie kopieën worden bijgewerkt, of Automatiseer installatie kopieën wanneer uw team code doorvoert naar een Git-opslag plaats. Maak taken met meerdere stappen om het bouwen, testen en bijwerken van meerdere container installatie kopieën parallel in de cloud te automatiseren.

Azure biedt hulp middelen zoals Azure Command-Line Interface, Azure Portal en API-ondersteuning voor het beheren van uw Azure-container registers. Installeer eventueel de [docker-extensie voor Visual Studio code](https://code.visualstudio.com/docs/azure/docker) en de [Azure-account](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) extensie om met uw Azure-container registers te werken. Pull-en push-installatie kopieën naar een Azure container Registry, of voer ACR-taken uit, allemaal in Visual Studio code.

## <a name="key-features"></a>Belangrijke functies

* **Register-sku's** : Maak een of meer container registers in uw Azure-abonnement. Registers zijn beschikbaar in drie Sku's: [Basic, Standard en Premium](container-registry-skus.md), die allemaal ondersteuning bieden voor integratie van webhooks, register verificatie met Azure Active Directory en functionaliteit voor verwijderen. Maak een register op dezelfde Azure-locatie als uw implementaties om te profiteren van lokale opslag dichtbij in het netwerk van uw containerinstallatiekopieën. Gebruik de [geo-replicatie](container-registry-geo-replication.md)functie van Premium-registers voor geavanceerde replicatie- en distributiescenario's voor containerinstallatiekopieën. 

  U kunt [toegang beheren](container-registry-authentication.md) tot een containerregister met behulp van een Azure-identiteit, een door Azure Active Directory ondersteunde [service-principal](../active-directory/develop/app-objects-and-service-principals.md) of een opgegeven beheeraccount. Meld u aan bij het REGI ster met de Azure CLI of de standaard `docker login` opdracht.

* **Ondersteunde installatie kopieën en artefacten** : gegroepeerd in een opslag plaats is elke afbeelding een alleen-lezen moment opname van een met docker compatibele container. Azure-containerregisters kunnen zowel Windows- als Linux-installatiekopieën bevatten. U beheert de namen van de installatiekopieën voor al uw containerimplementaties. Gebruik standaard-[Docker-opdrachten](https://docs.docker.com/engine/reference/commandline/) om installatiekopieën naar een opslagplaats te pushen of een installatiekopie uit een opslagplaats op te halen. Naast docker-container installatie kopieën worden met Azure Container Registry [gerelateerde inhouds indelingen](container-registry-image-formats.md) opgeslagen, zoals [helm-grafieken](container-registry-helm-repos.md) en installatie kopieën die zijn gemaakt in de indeling van het [opening container Initiative (OCI)-afbeelding](https://github.com/opencontainers/image-spec/blob/master/spec.md).

* **Azure container Registry taken** : gebruik [Azure container Registry taken](container-registry-tasks-overview.md) (ACR taken) om het bouwen, testen, pushen en implementeren van installatie kopieën in azure te stroom lijnen. Gebruik bijvoorbeeld ACR-taken om uw ontwikkeling binnenste lus uit te breiden naar de Cloud door `docker build` bewerkingen over te dragen naar Azure. Configureer buildtaken om uw container-OS- en frameworkpatchingpijplijn te automatiseren en automatisch installatiekopieën te maken wanneer uw team code met bronbeheer doorvoert.

  [Taken met meerdere stappen](container-registry-tasks-overview.md#multi-step-tasks) bieden taak definitie en uitvoering op basis van een stap voor het bouwen, testen en bijwerken van container installatie kopieën in de Cloud. Taakstappen definiëren afzonderlijke ontwikkel- en pushbewerkingen voor containerinstallatiekopieën. Ze kunnen ook de uitvoering definiëren van een of meer containers, waarbij elke stap de container als uitvoeringsomgeving gebruikt.

## <a name="next-steps"></a>Volgende stappen

* [Een containerregister maken met Azure Portal](container-registry-get-started-portal.md)
* [Een containerregister maken met de Azure-CLI](container-registry-get-started-azure-cli.md)
* [Container builds en onderhoud automatiseren met ACR-taken](container-registry-tasks-overview.md)