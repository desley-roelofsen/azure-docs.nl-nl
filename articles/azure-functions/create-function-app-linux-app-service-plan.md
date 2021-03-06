---
title: Een functie-app in Linux maken op basis van de Azure Portal
description: Leer hoe u uw eerste serverloze Azure-functie kunt maken met behulp van Azure Portal.
ms.topic: quickstart
ms.date: 02/28/2019
ms.openlocfilehash: d48bfc8888afe6b7a46151d2030519f752169551
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75410503"
---
# <a name="create-a-function-app-on-linux-in-an-azure-app-service-plan"></a>Een functie-app in Linux maken in een Azure App Service plan

Met Azure Functions kunt u uw functies voor Linux hosten in een standaardcontainer van Azure App Service. In dit artikel wordt uitgelegd hoe u de [Azure Portal](https://portal.azure.com) gebruikt voor het maken van een door Linux gehoste functie-app die wordt uitgevoerd in een [app service plan](functions-scale.md#app-service-plan). U kunt ook [uw eigen aangepaste container](functions-create-function-linux-custom-image.md) gebruiken.

![Functie-app maken in Azure Portal](./media/create-function-app-linux-app-service-plan/function-app-in-portal-editor.png)

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="sign-in-to-azure"></a>Aanmelden bij Azure

Meld u met uw Azure-account aan bij Azure Portal op <https://portal.azure.com>.

## <a name="create-a-function-app"></a>Een functie-app maken

U moet beschikken over een functie-app om de uitvoering van uw functies in Linux te hosten. De functie-app biedt een omgeving waarin uw functiecode kan worden uitgevoerd. U kunt er functies mee groeperen in een logische eenheid, zodat u resources eenvoudiger kunt beheren, implementeren en delen. In dit artikel maakt u een App Service plan wanneer u de functie-app maakt.

1. Selecteer de knop **Een resource maken** in de linkerbovenhoek van Azure Portal. Selecteer vervolgens **Compute** > **Functie-app**.

    ![Een functie-app maken in Azure Portal](./media/create-function-app-linux-app-service-plan/function-app-create-flow.png)

2. Gebruik voor de functie-app de instellingen die in de tabel onder de afbeelding zijn opgegeven.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/create-function-app-linux-app-service-plan/function-app-create-flow2.png)

    | Instelling      | Voorgestelde waarde  | Beschrijving                                        |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Naam van app** | Wereldwijd unieke naam | Naam waarmee uw nieuwe functie-app wordt aangeduid. Geldige tekens zijn `a-z`, `0-9` en `-`.  | 
    | **Abonnement** | Uw abonnement | Het abonnement waarmee deze nieuwe functie-app is gemaakt. | 
    | **[Resourcegroep](../azure-resource-manager/management/overview.md)** |  myResourceGroup | Naam voor de nieuwe resourcegroep waarin uw functie-app moet worden gemaakt. |
    | **Besturingssysteem** | Linux | De functie-app wordt uitgevoerd op Linux. |
    | **Publiceren** | Coderen | De standaard-Linux-container voor uw **runtime stack** wordt gebruikt. U hoeft alleen maar uw functie-app project code op te geven. Een andere optie is om een aangepaste [docker-installatie kopie](functions-create-function-linux-custom-image.md)te publiceren. |
    | **[Hostingplan](functions-scale.md)** | App Service-plan | Hostingabonnement dat definieert hoe resources worden toegewezen aan uw functie-app. Wanneer u in een App Service-abonnement uitvoert, kunt u de [schaal baarheid van de functie-app](functions-scale.md)beheren.  |
    | **App Service-plan/-locatie** | Plan maken | Kies **Nieuw maken** en geef een naam op voor het **app service plan** . Kies een **locatie** in een [regio](https://azure.microsoft.com/regions/) bij u in de buurt of in de buurt van andere services die uw functies gebruiken. Kies de gewenste **[prijs categorie](https://azure.microsoft.com/pricing/details/app-service/linux/)** . <br/>U kunt niet zowel Linux-als Windows-functie-apps uitvoeren in hetzelfde App Service-abonnement. |
    | **Runtimestack** | Voorkeurstaal | Kies een runtime die uw favoriete functieprogrammeertaal ondersteunt. Kies **.NET** voor C#- en F#-functies. |
    | **[Opslag](../storage/common/storage-quickstart-create-account.md)** |  Wereldwijd unieke naam |  Maak een opslagaccount die wordt gebruikt door uw functie-app. Namen van opslagaccounts moeten tussen 3 en 24 tekens lang zijn en mogen alleen cijfers en kleine letters bevatten. U kunt ook een bestaand account gebruiken dat voldoet aan de [vereisten voor een opslagaccount](functions-scale.md#storage-account-requirements). |
    | **[Application Insights](functions-monitoring.md)** | Ingeschakeld | Application Insights is standaard uitgeschakeld. We raden u aan Application Insights-integratie nu in te scha kelen en een hosting locatie te kiezen in de buurt van uw App Service plan locatie. Zie [Azure functions bewaken](functions-monitoring.md)als u dit later wilt doen.  |

3. Selecteer **Maken** om de functie-app in te richten en te implementeren.

4. Selecteer het meldingspictogram in de rechterbovenhoek van de portal en zoek het bericht **Implementatie voltooid**.

    ![Instellingen voor nieuwe functie-apps definiëren](./media/create-function-app-linux-app-service-plan/function-app-create-notification.png)

5. Selecteer **Naar de resource gaan** om uw nieuwe functie-app te bekijken.

Vervolgens maakt u een functie in de nieuwe functie-app. Zelfs nadat de functie-app beschikbaar is, kan het enkele minuten duren voordat deze volledig is geïnitialiseerd.

## <a name="create-function"></a>Een door HTTP geactiveerde functie maken

In deze sectie wordt beschreven hoe u een functie maakt in de nieuwe functie-app in de portal.

> [!NOTE]
> De portal-ontwikkelings ervaring kan handig zijn om Azure Functions uit te proberen. Voor de meeste scenario's kunt u uw functies lokaal ontwikkelen en het project publiceren naar uw functie-app met behulp van [Visual Studio code](functions-create-first-function-vs-code.md#create-an-azure-functions-project) of de [Azure functions core tools](functions-run-local.md#create-a-local-functions-project).  

1. Klik in de nieuwe functie-app op het tabblad **overzicht** en nadat het is geladen, selecteert u de **nieuwe functie**.

    ![Een nieuwe functie maken op het tabblad Overzicht](./media/create-function-app-linux-app-service-plan/overview-create-function.png)

1. Kies op het tabblad **Quick** start **in-portal**en selecteer **door gaan**.

    ![Kies uw ontwikkel platform voor de functie.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-choose-portal.png)

1. Kies **WebHook + API** en selecteer vervolgens **Maken**.

    ![De Quick Start van Azure Functions in Azure Portal.](./media/create-function-app-linux-app-service-plan/function-app-quickstart-node-webhook.png)

Een functie wordt gemaakt met een taalspecifieke sjabloon voor een door HTTP getriggerde functie.

U kunt de nieuwe functie nu uitvoeren door een HTTP-aanvraag te verzenden.

## <a name="test-the-function"></a>De functie testen

1. Klik in de nieuwe functie rechtsboven op **</> Functie-URL ophalen**, selecteer **Standaard (functietoets)** en klik vervolgens op **Kopiëren**. 

    ![De functie-URL vanuit Azure Portal kopiëren](./media/create-function-app-linux-app-service-plan/function-app-develop-tab-testing.png)

2. Plak de URL van de functie in de adresbalk van uw browser. Voeg waarde `&name=<yourname>` voor de querytekenreeks toe aan het einde van deze URL, en druk op de toets `Enter` op het toetsenbord om de aanvraag uit te voeren. U ziet het geretourneerde antwoord van de functie nu in de browser.  

    Het volgende voorbeeld toont het antwoord in de browser:

    ![Het antwoord van de functie in de browser.](./media/create-function-app-linux-app-service-plan/function-app-browser-testing.png)

    De aanvraag-URL bevat een sleutel die standaard is vereist, en waarmee u via HTTP toegang hebt tot de functie.

3. Wanneer uw functie wordt uitgevoerd, wordt traceringsinformatie naar de logboeken geschreven. Als u de traceringsuitvoer van de vorige uitvoering wilt zien, gaat u terug naar de functie in de portal en klikt u op de pijl onder aan het scherm om de **logboeken** uit te vouwen.

   ![De viewer voor functielogboeken in Azure Portal.](./media/create-function-app-linux-app-service-plan/function-view-logs.png)

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [Clean-up resources](../../includes/functions-quickstart-cleanup.md)]

## <a name="next-steps"></a>Volgende stappen

U hebt een functie-app met een eenvoudige door HTTP geactiveerde functie gemaakt.  

[!INCLUDE [Next steps note](../../includes/functions-quickstart-next-steps.md)]

Zie [Azure Functions HTTP bindings](functions-bindings-http-webhook.md) (Azure Functions-HTTP-bindingen) voor meer informatie.
