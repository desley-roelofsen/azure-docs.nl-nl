---
title: 'Snelstartgids: een Linux python-app maken'
description: Ga aan de slag met Linux-apps op Azure App Service door uw eerste python-app te implementeren in een Linux-container in App Service.
ms.topic: quickstart
ms.date: 10/22/2019
ms.custom: seo-python-october2019
experimental: false
experiment_id: 1e304dc9-5add-4b
ms.openlocfilehash: 67fbffbe96bc32b6ec38fa75c1e754c7f11d38d6
ms.sourcegitcommit: 48b7a50fc2d19c7382916cb2f591507b1c784ee5
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2019
ms.locfileid: "74687473"
---
# <a name="quickstart-create-a-python-app-in-azure-app-service-on-linux"></a>Snelstartgids: een python-app maken in Azure App Service in Linux

In deze Quick Start implementeert u een Python-web-app voor [app service op Linux](app-service-linux-intro.md), de uiterst schaal bare webhostingservice met self-patch functie. U gebruikt de lokale [Azure-opdracht regel interface (CLI)](/cli/azure/install-azure-cli) op een Mac-, Linux-of Windows-computer. De web-app die u configureert, maakt gebruik van een gratis App Service laag, zodat u geen kosten in de loop van dit artikel opdoet.

Als u liever apps implementeert via een IDE, raadpleegt u [python-Apps implementeren in app service van Visual Studio code](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="prerequisites"></a>Vereisten

- Azure-abonnement: [Maak er gratis een](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio)
- <a href="https://www.python.org/downloads/" target="_blank">Python 3,7</a> (python 3,6 wordt ook ondersteund)
- <a href="https://git-scm.com/downloads" target="_blank">Git</a>
- <a href="https://docs.microsoft.com/cli/azure/install-azure-cli" target="_blank">Azure CLI</a>

## <a name="download-the-sample"></a>Het voorbeeld downloaden

Voer in een Terminal venster de volgende opdracht uit om de voorbeeld toepassing te klonen op uw lokale computer. 

```terminal
git clone https://github.com/Azure-Samples/python-docs-hello-world
```

Ga vervolgens naar die map:

```terminal
cd python-docs-hello-world
```

De opslag plaats bevat een *Application.py* -bestand dat aangeeft app service dat de code een kolf-app bevat. Zie [Opstartprocessen en aanpassingen van container](how-to-configure-python.md) voor meer informatie.

## <a name="run-the-sample"></a>De voorbeeldtoepassing uitvoeren

Gebruik in een Terminal venster de onderstaande opdrachten (afhankelijk van uw besturings systeem) om de vereiste afhankelijkheden te installeren en de ingebouwde ontwikkel server te starten. 

# <a name="bashtabbash"></a>[Bash](#tab/bash)

```bash
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
FLASK_APP=application.py
flask run
```

# <a name="powershelltabpowershell"></a>[PowerShell](#tab/powershell)

```powershell
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
Set-Item Env:FLASK_APP ".\application.py"
flask run
```

# <a name="cmdtabcmd"></a>[Cmd](#tab/cmd)

```cmd
py -3 -m venv env
env\scripts\activate
pip install -r requirements.txt
SET FLASK_APP=application.py
flask run
```

---

Open een webbrowser en ga naar de voor beeld-app op `http://localhost:5000/`. In de app wordt het bericht **Hallo wereld!** weer gegeven.

![Een voor beeld van een python-app lokaal uitvoeren](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

Druk in het Terminal venster op **Ctrl**+**C** om de webserver af te sluiten.

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

De Azure CLI biedt u een groot aantal handige opdrachten die u vanaf een lokale terminal kunt gebruiken om Azure-resources in te richten en te beheren vanaf de opdracht regel. U kunt opdrachten gebruiken om dezelfde taken uit te voeren als de Azure Portal in een browser. U kunt ook CLI-opdrachten in scripts gebruiken om beheer processen te automatiseren.

Als u Azure-opdrachten in de Azure CLI wilt uitvoeren, moet u zich eerst aanmelden met de opdracht `az login`. Met deze opdracht wordt een browser geopend om uw referenties te verzamelen.

```terminal
az login
```

## <a name="deploy-the-sample"></a>Het voor beeld implementeren

Met de [`az webapp up`](/cli/azure/webapp#az-webapp-up) opdracht maakt u de web-app op app service en implementeert u de code.

Voer de volgende opdracht uit in de map *python-docs-Hello-World* met de voorbeeld code `az webapp up`. Vervang `<app-name>` door een wereld wijd unieke app-naam (*geldige tekens zijn `a-z`, `0-9`en `-`* ). Vervang ook `<location-name>` door een Azure-regio zoals **centralus**, **EastAsia**, **Europa West**, **koreasouth**, **brazilsouth**, **centralindia**, enzovoort. (U kunt een lijst met toegestane regio's voor uw Azure-account ophalen door de [`az account locations-list`](/cli/azure/appservice?view=azure-cli-latest.md#az-appservice-list-locations) opdracht uit te voeren.)


```terminal
az webapp up --sku F1 -n <app-name> -l <location-name>
```

Het kan enkele minuten duren voordat deze opdracht is uitgevoerd. De opdracht geeft informatie weer die lijkt op het volgende voorbeeld:

```output
The behavior of this command has been altered by the following extension: webapp
Creating Resource group 'appsvc_rg_Linux_centralus' ...
Resource group creation complete
Creating App service plan 'appsvc_asp_Linux_centralus' ...
App service plan creation complete
Creating app '<app-name>' ....
Webapp creation complete
Creating zip with contents of dir /home/username/quickstart/python-docs-hello-world ...
Preparing to deploy contents to app.
All done.
{
  "app_url": "https:/<app-name>.azurewebsites.net",
  "location": "Central US",
  "name": "<app-name>",
  "os": "Linux",
  "resourcegroup": "appsvc_rg_Linux_centralus ",
  "serverfarm": "appsvc_asp_Linux_centralus",
  "sku": "BASIC",
  "src_path": "/home/username/quickstart/python-docs-hello-world ",
  "version_detected": "-",
  "version_to_create": "python|3.7"
}
```

[!INCLUDE [AZ Webapp Up Note](../../../includes/app-service-web-az-webapp-up-note.md)]

## <a name="browse-to-the-app"></a>Bladeren naar de app

Blader naar de geïmplementeerde toepassing in uw webbrowser op de URL `http://<app-name>.azurewebsites.net`.

In de python-voorbeeld code wordt een Linux-container uitgevoerd in App Service met behulp van een ingebouwde installatie kopie.

![Een voor beeld van een python-app in azure uitvoeren](./media/quickstart-python/run-hello-world-sample-python-app-in-browser.png)

**Gefeliciteerd!** U hebt uw python-app geïmplementeerd op App Service op Linux.

## <a name="redeploy-updates"></a>Updates opnieuw implementeren

Open *Application.py* in uw favoriete code-editor en wijzig de `return`-instructie op de laatste regel zodat deze overeenkomt met de volgende code. De `print`-instructie is hier opgenomen om logboek registratie-uitvoer te genereren waarmee u in de volgende sectie kunt werken. 

```python
print("Handling request to home page.")
return "Hello Azure!"
```

Sla uw wijzigingen op en sluit de editor af. 

Implementeer de app opnieuw met behulp van de volgende `az webapp up` opdracht, met dezelfde opdracht die u hebt gebruikt om de app de eerste keer te implementeren, waarbij u `<app-name>` en `<location-name>` vervangt door de namen die u eerder hebt gebruikt. 

```terminal
az webapp up --sku F1 -n <app-name> -l <location-name>
```

Zodra de implementatie is voltooid, gaat u terug naar het browser venster open naar `http://<app-name>.azurewebsites.net` en vernieuwt u de pagina, waarin het gewijzigde bericht moet worden weer gegeven:

![Een bijgewerkte voor beeld-app voor python uitvoeren in azure](./media/quickstart-python/run-updated-hello-world-sample-python-app-in-browser.png)

> [!TIP]
> Visual Studio code biedt krachtige extensies voor python en Azure App Service, waarmee het proces van het implementeren van Python-web-apps naar App Service wordt vereenvoudigd. Zie voor meer informatie [python-Apps implementeren in app service van Visual Studio code](/azure/python/tutorial-deploy-app-service-on-linux-01).

## <a name="stream-logs"></a>Logboeken streamen

U hebt toegang tot de console logboeken die zijn gegenereerd in de app en de container waarin deze wordt uitgevoerd. Logboeken bevatten een uitvoer die wordt gegenereerd met behulp van `print`-instructies.

Schakel eerst container logboek registratie in door de volgende opdracht uit te voeren in een Terminal, waarbij u `<app-name>` vervangt door de naam van uw app en `<resource-group-name>` met de naam van de resource groep die wordt weer gegeven in de uitvoer van de `az webapp up` opdracht die u hebt gebruikt (zoals ' appsvc_rg_Linux_centralus '):

```terminal
az webapp log config --name <app-name> --resource-group <resource-group-name> --docker-container-logging filesystem
```

Zodra de container logboek registratie is ingeschakeld, voert u de volgende opdracht uit om de logboek stroom weer te geven:

```terminal
az webapp log tail --name <app-name> --resource-group <resource-group-name>
```

Vernieuw de app in de browser om console logboeken te genereren, die regels moeten bevatten die vergelijkbaar zijn met de volgende tekst. Als u de uitvoer niet onmiddellijk ziet, probeert u het over 30 seconden opnieuw.

```output
2019-10-23T12:40:03.815574424Z Handling request to home page.
2019-10-23T12:40:03.815602424Z 172.16.0.1 - - [23/Oct/2019:12:40:03 +0000] "GET / HTTP/1.1" 200 12 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/78.0.3904.63 Safari/537.36 Edg/78.0.276.19"
```

U kunt de logboek bestanden ook vanuit de browser controleren op `https://<app-name>.scm.azurewebsites.net/api/logs/docker`.

Als u het streamen van Logboeken op elk gewenst moment wilt stoppen, typt u `Ctrl`+`C`.

## <a name="manage-the-azure-app"></a>De Azure-app beheren

Ga naar <a href="https://portal.azure.com" target="_blank">Azure Portal</a> om de app te beheren die u hebt gemaakt. Zoek en selecteer **app Services**.

![Navigeer naar App Services in het Azure Portal](./media/quickstart-python/navigate-to-app-services-in-the-azure-portal.png)

Selecteer de naam van uw Azure-app.

![Navigeer naar uw python-app in App Services in het Azure Portal](./media/quickstart-python/navigate-to-app-in-app-services-in-the-azure-portal.png)

De pagina Overzicht van uw app wordt weergegeven. Hier kunt u algemene beheertaken uitvoeren, zoals bladeren, stoppen, starten, opnieuw opstarten en verwijderen.

![Beheer uw python-app op de pagina overzicht in de Azure Portal](./media/quickstart-python/manage-an-app-in-app-services-in-the-azure-portal.png)

Het App Service menu bevat verschillende pagina's voor het configureren van uw app.

## <a name="clean-up-resources"></a>Resources opschonen

In de voorgaande stappen hebt u Azure-resources in een resourcegroep gemaakt. De resource groep heeft een naam als ' appsvc_rg_Linux_CentralUS ', afhankelijk van uw locatie. Als u een andere App Service SKU gebruikt dan de gratis F1-laag, zullen deze resources lopende kosten oplopen.

Als u deze resources in de toekomst niet meer nodig hebt, verwijdert u de resource groep door de volgende opdracht uit te voeren, waarbij u `<resource-group-name>` vervangt door de resource groep die wordt weer gegeven in de uitvoer van de `az webapp up` opdracht, zoals ' appsvc_rg_Linux_centralus '. Het volt ooien van de opdracht kan een minuut duren.

```terminal
az group delete -n <resource-group-name>
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Zelf studie: python (Django) Web-app met PostgreSQL](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [Python-app configureren](how-to-configure-python.md)

> [!div class="nextstepaction"]
> [Zelf studie: python-app uitvoeren in een aangepaste container](tutorial-custom-docker-image.md)
