---
title: EmptyDir volume koppelen aan container groep
description: Meer informatie over het koppelen van een emptyDir-volume voor het delen van gegevens tussen de containers in een container groep in Azure Container Instances
ms.topic: article
ms.date: 02/08/2018
ms.openlocfilehash: 955423b685ebb3979271c7c2dc7e835a16100c2b
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552454"
---
# <a name="mount-an-emptydir-volume-in-azure-container-instances"></a>Een emptyDir-volume koppelen in Azure Container Instances

Meer informatie over het koppelen van een *emptyDir* -volume voor het delen van gegevens tussen de containers in een container groep in azure container instances. Gebruik *emptyDir* -volumes als tijdelijke caches voor uw door de container geplaatste workloads.

> [!NOTE]
> Het koppelen van een *emptyDir* -volume is momenteel beperkt tot Linux-containers. Terwijl we aan de slag gaan met het toevoegen van alle functies aan Windows-containers, kunt u de huidige platform verschillen vinden in het [overzicht](container-instances-overview.md#linux-and-windows-containers).

## <a name="emptydir-volume"></a>emptyDir volume

Het *emptyDir* -volume biedt een Beschrijf bare Directory die toegankelijk is voor elke container in een container groep. Containers in de groep kunnen dezelfde bestanden in het volume lezen en schrijven en kunnen worden gekoppeld met dezelfde of verschillende paden in elke container.

Een voor beeld van een *emptyDir* -volume:

* Scratch ruimte
* Controle punten uitvoeren tijdens langlopende taken
* Gegevens opslaan die zijn opgehaald door een zijspan wagen en worden bediend door een toepassings container

Gegevens in een *emptyDir* -volume blijven behouden via container crashes. Containers die opnieuw worden opgestart, zijn echter niet gegarandeerd de gegevens in een *emptyDir* -volume op te slaan. Als u een container groep stopt, wordt het *emptyDir* -volume niet persistent gemaakt.

## <a name="mount-an-emptydir-volume"></a>Een emptyDir-volume koppelen

Als u een emptyDir-volume in een container exemplaar wilt koppelen, moet u implementeren met behulp van een [Azure Resource Manager sjabloon](/azure/templates/microsoft.containerinstance/containergroups).

Vul eerst de `volumes`-matrix in het gedeelte `properties` van de container groep van de sjabloon. Voor elke container in de container groep waarin u het *emptyDir* -volume wilt koppelen, vult u de `volumeMounts`-matrix in het gedeelte `properties` van de container definitie.

De volgende Resource Manager-sjabloon maakt bijvoorbeeld een container groep die bestaat uit twee containers, die elk het *emptyDir* -volume koppelen:

<!-- https://github.com/Azure/azure-docs-json-samples/blob/master/container-instances/aci-deploy-volume-emptydir.json -->
[!code-json[volume-emptydir](~/azure-docs-json-samples/container-instances/aci-deploy-volume-emptydir.json)]

Zie [groepen met meerdere containers implementeren in azure container instances](container-instances-multi-container-group.md)om een voor beeld te zien van implementatie van container instanties met een Azure Resource Manager sjabloon.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het koppelen van andere volume typen in Azure Container Instances:

* [Een Azure-bestandsshare koppelen in Azure Container Instances](container-instances-volume-azure-files.md)
* [Koppelen van een volume gitRepo in Azure Containerexemplaren](container-instances-volume-gitrepo.md)
* [Een geheim volume koppelen in Azure Container Instances](container-instances-volume-secret.md)