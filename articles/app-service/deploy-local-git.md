---
title: Implementeren vanuit lokale Git-opslag plaats
description: Meer informatie over het inschakelen van lokale Git-implementatie naar Azure App Service. Een van de eenvoudigste manieren om code te implementeren vanaf uw lokale computer.
ms.assetid: ac50a623-c4b8-4dfd-96b2-a09420770063
ms.topic: article
ms.date: 06/18/2019
ms.reviewer: dariac
ms.custom: seodec18
ms.openlocfilehash: 91d5827a08a600c19c24ac0a96a5f4e3e98e22f2
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671766"
---
# <a name="local-git-deployment-to-azure-app-service"></a>Lokale Git-implementatie naar Azure App Service

In deze hand leiding wordt uitgelegd hoe u uw app kunt implementeren in [Azure app service](overview.md) vanuit een Git-opslag plaats op uw lokale computer.

## <a name="prerequisites"></a>Vereisten

Volg de stappen in deze hand leiding:

- [!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]
  
- [Installeer Git](https://www.git-scm.com/downloads).
  
- Een lokale Git-opslag plaats hebben met code die u wilt implementeren. Als u een voor beeld van een opslag plaats wilt downloaden, voert u de volgende opdracht uit in het lokale terminal venster:
  
  ```bash
  git clone https://github.com/Azure-Samples/nodejs-docs-hello-world.git
  ```

[!INCLUDE [Prepare repository](../../includes/app-service-deploy-prepare-repo.md)]

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="deploy-with-kudu-build-server"></a>Implementeren met kudu build server

De eenvoudigste manier om lokale Git-implementatie voor uw app in te scha kelen met de kudu-server App Service build, is door gebruik te maken van Azure Cloud Shell. 

### <a name="configure-a-deployment-user"></a>Een implementatiegebruiker configureren

[!INCLUDE [Configure a deployment user](../../includes/configure-deployment-user-no-h.md)]

### <a name="get-the-deployment-url"></a>De implementatie-URL ophalen

Als u de URL wilt ophalen voor het inschakelen van lokale Git-implementatie voor een bestaande app, voert u [`az webapp deployment source config-local-git`](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-local-git) uit in de Cloud shell. Vervang \<naam van de app > en \<groeps naam > door de namen van uw app en de bijbehorende Azure-resource groep.

```azurecli-interactive
az webapp deployment source config-local-git --name <app-name> --resource-group <group-name>
```

Als u een nieuwe app met Git wilt maken, voert u [`az webapp create`](/cli/azure/webapp?view=azure-cli-latest#az-webapp-create) in het Cloud shell uit met de para meter `--deployment-local-git`. Vervang \<app-naam >, \<groeps naam > en \<plan naam > met de namen voor uw nieuwe Git-app, de Azure-resource groep en het Azure App Service plan.

```azurecli-interactive
az webapp create --name <app-name> --resource-group <group-name> --plan <plan-name> --deployment-local-git
```

Een van de opdrachten retourneert een URL zoals: `https://<deployment-username>@<app-name>.scm.azurewebsites.net/<app-name>.git`. Gebruik deze URL om uw app te implementeren in de volgende stap.

In plaats van deze URL op account niveau te gebruiken, kunt u ook lokale Git inschakelen met behulp van referenties op app-niveau. Azure App Service worden deze referenties automatisch gegenereerd voor elke app. 

Haal de app-referenties op door de volgende opdracht uit te voeren in de Cloud Shell. Vervang \<app-naam > en \<groeps naam > door de naam van de app en de naam van de Azure-resource groep.

```azurecli-interactive
az webapp deployment list-publishing-credentials --name <app-name> --resource-group <group-name> --query scmUri --output tsv
```

Gebruik de URL die in de volgende stap wordt gebruikt om uw app te implementeren.

### <a name="deploy-the-web-app"></a>De web-app implementeren

1. Open een lokaal Terminal venster voor uw lokale Git-opslag plaats en voeg een Azure Remote toe. Vervang in de volgende opdracht \<URL-> door de gebruikersspecifieke URL van de implementatie of de app-specifieke URL die u uit de vorige stap hebt gekregen.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Pushen naar de externe Azure-`git push azure master`. 
   
1. Voer in het venster **Git-referentie beheer** uw wacht woord voor de [implementatie gebruiker](#configure-a-deployment-user)in, niet uw aanmeldings wachtwoord voor Azure.
   
1. Controleer de uitvoer. U kunt runtime-specifieke automatisering zien, zoals MSBuild voor ASP.NET, `npm install` voor node. js en `pip install` voor python. 
   
1. Blader naar uw app in de Azure Portal om te controleren of de inhoud is geïmplementeerd.

## <a name="deploy-with-azure-pipelines-builds"></a>Implementeren met Azure pipelines-builds

Als uw account over de benodigde machtigingen beschikt, kunt u Azure-pijp lijnen (preview) instellen om lokale Git-implementatie in te scha kelen voor uw app. 

- Uw Azure-account moet machtigingen hebben om naar Azure Active Directory te schrijven en een service te maken. 
  
- Uw Azure-account moet de rol **eigenaar** hebben in uw Azure-abonnement.

- U moet een beheerder zijn in het Azure DevOps-project dat u wilt gebruiken.

Lokale Git-implementatie voor uw app inschakelen met Azure-pijp lijnen (preview):

1. Ga naar de pagina Azure App Service-app in de [Azure Portal](https://portal.azure.com)en selecteer **implementatie centrum** in het linkermenu.
   
1. Selecteer **lokale Git**op de pagina **implementatie centrum** en selecteer **door gaan**. 
   
   ![Selecteer lokale Git en selecteer door gaan](media/app-service-deploy-local-git/portal-enable.png)
   
1. Selecteer op de pagina **Build** -provider **Azure-pijp lijnen (preview)** en selecteer vervolgens **door gaan**. 
   
   ![Selecteer Azure-pijp lijnen (preview) en selecteer vervolgens door gaan.](media/app-service-deploy-local-git/pipeline-builds.png)

1. Configureer op de pagina **configureren** een nieuwe Azure DevOps-organisatie of geef een bestaande organisatie op en selecteer **door gaan**.
   
   > [!NOTE]
   > Als uw bestaande Azure DevOps-organisatie niet wordt weer gegeven, moet u deze mogelijk koppelen aan uw Azure-abonnement. Zie [uw CD release-pijp lijn definiëren](/azure/devops/pipelines/apps/cd/deploy-webdeploy-webapps#cd)voor meer informatie.
   
1. Afhankelijk van de [prijs categorie](https://azure.microsoft.com/pricing/details/app-service/plans/)van uw app service-abonnement, ziet u mogelijk de pagina **implementeren naar staging** . Kies of u [implementatie sleuven wilt inschakelen](deploy-staging-slots.md)en selecteer **door gaan**.
   
1. Controleer de instellingen op de pagina **samen vatting** en selecteer vervolgens **volt ooien**.
   
1. Wanneer de Azure-pijp lijn gereed is, kopieert u de URL van de Git-opslag plaats vanaf de pagina **Deployment Center** , zodat u deze kunt gebruiken in de volgende stap. 
   
   ![De URL van de Git-opslag plaats kopiëren](media/app-service-deploy-local-git/vsts-repo-ready.png)

1. Voeg in het lokale terminal venster een externe Azure-Data Bank toe aan uw lokale Git-opslag plaats. Vervang in de opdracht \<URL-> door de URL van de Git-opslag plaats die u hebt ontvangen van de vorige stap.
   
   ```bash
   git remote add azure <url>
   ```
   
1. Pushen naar de externe Azure-`git push azure master`. 
   
1. Meld u op de pagina **Git-referentie beheer** aan met uw VisualStudio.com-gebruikers naam. Zie [overzicht van Azure DevOps Services-verificatie](/vsts/git/auth-overview?view=vsts)voor andere verificatie methoden.
   
1. Nadat de implementatie is voltooid, bekijkt u de voortgang van de build op `https://<azure_devops_account>.visualstudio.com/<project_name>/_build`en de implementatie voortgang op `https://<azure_devops_account>.visualstudio.com/<project_name>/_release`.
   
1. Blader naar uw app in de Azure Portal om te controleren of de inhoud is geïmplementeerd.

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="troubleshoot-deployment"></a>Problemen met implementatie oplossen

Mogelijk worden de volgende veelvoorkomende fout berichten weer geven wanneer u Git gebruikt om te publiceren naar een App Service-app in Azure:

|Bericht|Oorzaak|Resolutie
---|---|---|
|`Unable to access '[siteURL]': Failed to connect to [scmAddress]`|De app is niet actief.|Start de app in het Azure Portal. Git-implementatie is niet beschikbaar wanneer de web-app is gestopt.|
|`Couldn't resolve host 'hostname'`|De adres gegevens voor de externe Azure-computer zijn onjuist.|Gebruik de opdracht `git remote -v` om alle externe-en de bijbehorende URL weer te geven. Controleer of de URL voor de externe Azure juist is. Als dat nodig is, kunt u deze extern verwijderen en opnieuw maken met de juiste URL.|
|`No refs in common and none specified; doing nothing. Perhaps you should specify a branch such as 'master'.`|U hebt geen vertakking opgegeven tijdens `git push`, of u hebt de `push.default` waarde niet ingesteld in `.gitconfig`.|Voer `git push` opnieuw uit, waarbij u de hoofd vertakking opgeeft: `git push azure master`.|
|`src refspec [branchname] does not match any.`|U hebt geprobeerd naar een andere vertakking dan Master te pushen op de externe Azure-server.|Voer `git push` opnieuw uit, waarbij u de hoofd vertakking opgeeft: `git push azure master`.|
|`RPC failed; result=22, HTTP code = 5xx.`|Deze fout kan optreden als u probeert een grote Git-opslag plaats via HTTPS te pushen.|Wijzig de Git-configuratie op de lokale computer om de `postBuffer` groter te maken. Bijvoorbeeld: `git config --global http.postBuffer 524288000`.|
|`Error - Changes committed to remote repository but your web app not updated.`|U hebt een node. js-app geïmplementeerd met een _package. json_ -bestand dat aanvullende vereiste modules specificeert.|Raadpleeg de `npm ERR!` fout berichten vóór deze fout voor meer context over de fout. Hieronder vindt u de bekende oorzaken van deze fout en de bijbehorende `npm ERR!` berichten:<br /><br />**Onjuist gevormd pakket. JSON-bestand**: `npm ERR! Couldn't read dependencies.`<br /><br />**Systeem eigen module heeft geen binaire distributie voor Windows**:<br />`npm ERR! \cmd "/c" "node-gyp rebuild"\ failed with 1` <br />of <br />`npm ERR! [modulename@version] preinstall: \make || gmake\ `|

## <a name="additional-resources"></a>Aanvullende bronnen

- [Documentatie voor project kudu](https://github.com/projectkudu/kudu/wiki)
- [Continue implementatie naar Azure App Service](deploy-continuous-deployment.md)
- [Voor beeld: een web-app maken en code implementeren vanuit een lokale Git-opslag plaats (Azure CLI)](./scripts/cli-deploy-local-git.md?toc=%2fcli%2fazure%2ftoc.json)
- [Voor beeld: een web-app maken en code implementeren vanuit een lokale Git-opslag plaats (Power shell)](./scripts/powershell-deploy-local-git.md?toc=%2fpowershell%2fmodule%2ftoc.json)
