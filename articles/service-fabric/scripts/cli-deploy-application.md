---
title: Voorbeeld van de implementatie van Azure Service Fabric CLI (sfctl) met behulp van een script
description: Een toepassing implementeren naar een Azure Service Fabric-cluster met Azure Service Fabric CLI
services: service-fabric
documentationcenter: ''
author: athinanthny
manager: chackdan
editor: ''
tags: azure-service-management
ms.assetid: ''
ms.service: service-fabric
ms.workload: multiple
ms.topic: sample
ms.date: 04/16/2018
ms.author: atsenthi
ms.custom: mvc
ms.openlocfilehash: 7417eaecddad60c940bf01535b8fb24b8cbef80c
ms.sourcegitcommit: 18061d0ea18ce2c2ac10652685323c6728fe8d5f
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/15/2019
ms.locfileid: "69034774"
---
# <a name="deploy-an-application-to-a-service-fabric-cluster"></a>Een toepassing implementeren naar een Service Fabric-cluster

Met dit voorbeeldscript wordt een toepassingspakket naar een installatieopslag van een cluster gekopieerd, wordt het toepassingstype in het cluster geregistreerd en wordt een instantie van het toepassingstype gemaakt. Dit is het moment waarop ook alle eventuele standaardservices worden gemaakt.

Installeer indien nodig ook [Service Fabric CLI](../service-fabric-cli.md).

## <a name="sample-script"></a>Voorbeeldscript

[!code-sh[main](../../../cli_scripts/service-fabric/deploy-application/deploy-application.sh "Deploy an application to a cluster")]

## <a name="clean-up-deployment"></a>Opschonen van implementatie

Wanneer dit is gebeurd, kan het [verwijderscript](cli-remove-application.md) worden gebruikt om de toepassing te verwijderen. Met het verwijderscript wordt de instantie van de toepassing verwijderd, de registratie van het toepassingstype ongedaan gemaakt en het toepassingspakket verwijderd uit de installatiekopieopslag.

## <a name="next-steps"></a>Volgende stappen

Raadpleeg de [Service Fabric CLI-documentatie ](../service-fabric-cli.md)voor meer informatie.

Meer Service Fabric CLI-voorbeelden voor Azure Service Fabric zijn te vinden in de [Voorbeelden van Azure Service Fabric CLI](../samples-cli.md).
