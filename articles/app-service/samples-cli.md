---
title: CLI-voorbeelden
description: Zoek naar Azure CLI-voor beelden voor een aantal algemene App Service scenario's. Meer informatie over het automatiseren van uw App Service-implementatie-of beheer taken.
tags: azure-service-management
ms.assetid: 53e6a15a-370a-48df-8618-c6737e26acec
ms.topic: sample
ms.date: 12/12/2017
ms.custom: mvc
ms.openlocfilehash: 0b3acb1b421962cde7d90398f42bdfeefda578e3
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/10/2019
ms.locfileid: "74971499"
---
# <a name="cli-samples-for-azure-app-service"></a>CLI-voorbeelden voor Azure App Service

De volgende tabel bevat koppelingen naar bash-scripts die zijn gebouwd met behulp van de Azure CLI.

| | |
|-|-|
|**App maken**||
| [Een app maken en bestanden implementeren met FTP](./scripts/cli-deploy-ftp.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app en implementeert u een bestand naar de app met behulp van FTP. |
| [Een app maken en code implementeren vanuit GitHub](./scripts/cli-deploy-github.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app en implementeert u code vanuit een openbare GitHub-opslagplaats. |
| [Een app maken met continue implementatie vanuit GitHub](./scripts/cli-continuous-deployment-github.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app met continue publicatie vanuit een GitHub-opslagplaats waarvan u de eigenaar bent. |
| [Een app maken en code implementeren vanuit een lokale Git-opslagplaats](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app en configureert u het pushen van code vanuit een lokale Git-opslagplaats. |
| [Een app maken en code implementeren in een faseringsomgeving](./scripts/cli-deploy-staging-environment.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app met een implementatiesite voor het faseren van codewijzigingen. |
| [Een ASP.NET Core-app maken in een Docker-container](./scripts/cli-linux-docker-aspnetcore.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app onder Linux en wordt er een Docker-installatiekopie vanuit Docker Hub geladen. |
|**App configureren**||
| [Een aangepast domein toewijzen aan een app](./scripts/cli-configure-custom-domain.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app en wijst u er een aangepaste domeinnaam aan toe. |
| [Een aangepast SSL-certificaat verbinden met een app](./scripts/cli-configure-ssl-certificate.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app en verbindt u het SSL-certificaat van een aangepaste domeinnaam met de app. |
|**App schalen**||
| [Een app handmatig schalen](./scripts/cli-scale-manual.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app en schaalt u deze over twee exemplaren. |
| [Een app wereldwijd schalen met een architectuur voor hoge beschikbaarheid](./scripts/cli-scale-high-availability.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u twee App Service-apps in twee verschillende geografische regio's en maakt u deze met behulp van Azure Traffic Manager beschikbaar via één eindpunt. |
|**App beveiligen**||
| [Integreren met Azure-toepassing gateway](./scripts/cli-integrate-app-service-with-application-gateway.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app en integreert u deze met Application Gateway met behulp van service-eind punten en toegangs beperkingen. |
|**App verbinden met resources**||
| [Een app verbinden met een SQL-database](./scripts/cli-connect-to-sql.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app en een SQL-database, waarna u de verbindingsreeks van de database toevoegt aan de app-instellingen. |
| [Een app verbinden met een opslagaccount](./scripts/cli-connect-to-storage.md?toc=%2fcli%2fazure%2ftoc.json)| Hiermee maakt u een App Service-app en een opslagaccount, waarna u de verbindingsreeks van de opslag toevoegt aan de app-instellingen. |
| [Een app verbinden met Azure Cache voor Redis](./scripts/cli-connect-to-redis.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app en een Azure Cache voor Redis en voegt u de details van de redis-verbinding toe aan de app-instellingen. |
| [Een app verbinden met Cosmos DB](./scripts/cli-connect-to-documentdb.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app en een Cosmos DB en voegt u de details van de Cosmos DB-verbinding toe aan de app-instellingen. |
|**Back-up van app maken en terugzetten**||
| [Een back-up maken van een app](./scripts/cli-backup-onetime.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app en maakt u er een eenmalige back-up van. |
| [Een geplande back-up maken voor een app](./scripts/cli-backup-scheduled.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app en stelt u een geplande back-up in voor de app. |
| [Een app terugzetten vanuit een back-up](./scripts/cli-backup-restore.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee zet u een App Service-app terug vanuit een back-up. |
|**App controleren**||
| [Een app bewaken met webserverlogboeken](./scripts/cli-monitor.md?toc=%2fcli%2fazure%2ftoc.json) | Hiermee maakt u een App Service-app, schakelt u logboekregistratie voor de app in en downloadt u de logboeken naar uw lokale computer. |
| | |