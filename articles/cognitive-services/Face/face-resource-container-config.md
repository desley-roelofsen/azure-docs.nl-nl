---
title: Containers configureren-FACE-API
titleSuffix: Azure Cognitive Services
description: Het face container runtime-omgeving wordt geconfigureerd met behulp van de `docker run` opdracht argumenten. Er zijn zowel vereiste als optionele instellingen.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.custom: seodec18
ms.service: cognitive-services
ms.subservice: face-api
ms.topic: conceptual
ms.date: 11/07/2019
ms.author: dapine
ms.openlocfilehash: 78fd2aa977062d2f0d6b981140f3db5b263e4651
ms.sourcegitcommit: 018e3b40e212915ed7a77258ac2a8e3a660aaef8
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/07/2019
ms.locfileid: "73795039"
---
# <a name="configure-face-docker-containers"></a>Face docker-containers configureren

Het **Face** container runtime-omgeving wordt geconfigureerd met behulp van de `docker run` opdracht argumenten. Deze container heeft verschillende vereiste instellingen, samen met enkele optionele instellingen. Er zijn verschillende [voor beelden](#example-docker-run-commands) van de opdracht beschikbaar. De container-specifieke instellingen zijn de facturerings instellingen. 

## <a name="configuration-settings"></a>Configuratie-instellingen

[!INCLUDE [Container shared configuration settings table](../../../includes/cognitive-services-containers-configuration-shared-settings-table.md)]

> [!IMPORTANT]
> De instellingen [`ApiKey`](#apikey-configuration-setting), [`Billing`](#billing-configuration-setting)en [`Eula`](#eula-setting) worden samen gebruikt en u moet geldige waarden opgeven voor alle drie deze. anders kan de container niet worden gestart. Zie [facturering](face-how-to-install-containers.md#billing)voor meer informatie over het gebruik van deze configuratie-instellingen voor het instantiëren van een container.

## <a name="apikey-configuration-setting"></a>Configuratie-instelling ApiKey

Met de instelling `ApiKey` geeft u de Azure-resource sleutel op die wordt gebruikt om de facturerings gegevens voor de container bij te houden. U moet een waarde opgeven voor de ApiKey en de waarde moet een geldige sleutel zijn voor de _Cognitive Services_ bron die is opgegeven voor de configuratie-instelling [`Billing`](#billing-configuration-setting) .

Deze instelling bevindt zich op de volgende locatie:

* Azure Portal: **Cognitive Services** Resource Management onder **sleutels**

## <a name="applicationinsights-setting"></a>ApplicationInsights-instelling

[!INCLUDE [Container shared configuration ApplicationInsights settings](../../../includes/cognitive-services-containers-configuration-shared-settings-application-insights.md)]

## <a name="billing-configuration-setting"></a>Instelling facturerings configuratie

Met de instelling `Billing` geeft u de eindpunt-URI op van de _Cognitive Services_ resource op Azure die wordt gebruikt om de facturerings gegevens voor de container te meten. U moet een waarde opgeven voor deze configuratie-instelling en de waarde moet een geldige eindpunt-URI zijn voor een _Cognitive Services_ resource in Azure. De container rapporteert het gebruik ongeveer elke 10 tot 15 minuten.

Deze instelling bevindt zich op de volgende locatie:

* Azure Portal: overzicht van **Cognitive Services** , gelabelde `Endpoint`

Vergeet niet om het _gezichts_ routering toe te voegen aan de URI van het eind punt zoals weer gegeven in het voor beeld. 

|Vereist| Naam | Gegevenstype | Beschrijving |
|--|------|-----------|-------------|
|Ja| `Billing` | Tekenreeks | URL van het facturerings eindpunt. Zie [vereiste para meters verzamelen](face-how-to-install-containers.md#gathering-required-parameters)voor meer informatie over het verkrijgen van de facturerings-URI. Zie [aangepaste subdomein namen voor Cognitive Services](../cognitive-services-custom-subdomains.md)voor meer informatie en een volledige lijst met regionale eind punten. |

<!-- specific to face only -->

## <a name="cloudai-configuration-settings"></a>Configuratie-instellingen voor CloudAI

De configuratie-instellingen in de sectie `CloudAI` bieden opties die specifiek zijn voor de container. De volgende instellingen en objecten worden ondersteund voor de face-container in de sectie `CloudAI`

| Naam | Gegevenstype | Beschrijving |
|------|-----------|-------------|
| `Storage` | Object | Het opslag scenario dat wordt gebruikt door de face-container. Zie [opslag scenario-instellingen](#storage-scenario-settings) voor meer informatie over opslag scenario's en de bijbehorende instellingen voor het object `Storage`. |

### <a name="storage-scenario-settings"></a>Instellingen voor opslag scenario

Afhankelijk van wat er is opgeslagen, worden de blob-, cache-, meta gegevens-en wachtrij gegevens in de face-container opgeslagen. Bijvoorbeeld: opleidings indexen en resultaten voor een grote groep personen worden opgeslagen als BLOB-gegevens. De face-container biedt twee verschillende opslag scenario's bij het werken met en het opslaan van deze typen gegevens:

* Geheugen  
  Alle vier de typen gegevens worden opgeslagen in het geheugen. Ze worden niet gedistribueerd en blijven niet behouden. Als de face-container is gestopt of verwijderd, worden alle gegevens in de opslag voor die container vernietigd.  
  Dit is het standaard opslag scenario voor de face-container.
* Azure  
  De face-container gebruikt Azure Storage en Azure Cosmos DB om deze vier typen gegevens te verdelen over permanente opslag. BLOB-en wachtrij gegevens worden verwerkt door Azure Storage. Meta gegevens en cache gegevens worden verwerkt door Azure Cosmos DB. Als de face-container is gestopt of verwijderd, blijven alle gegevens in de opslag voor die container opgeslagen in Azure Storage en Azure Cosmos DB.  
  Voor de resources die worden gebruikt door het Azure Storage-scenario gelden de volgende aanvullende vereisten
  * De Azure Storage Resource moet het StorageV2-account type gebruiken
  * De Azure Cosmos DB resource moet de Azure Cosmos DB-API voor MongoDB gebruiken

De opslag scenario's en de bijbehorende configuratie-instellingen worden beheerd door het `Storage`-object, onder de sectie `CloudAI` configuratie. De volgende configuratie-instellingen zijn beschikbaar in het `Storage`-object:

| Naam | Gegevenstype | Beschrijving |
|------|-----------|-------------|
| `StorageScenario` | Tekenreeks | Het opslag scenario dat door de container wordt ondersteund. De volgende waarden zijn beschikbaar<br/>`Memory`-standaard waarde. Container maakt gebruik van niet-permanente, niet-gedistribueerde en in-Memory opslag, voor tijdelijk gebruik met één knoop punt. Als de container wordt gestopt of verwijderd, wordt de opslag voor die container vernietigd.<br/>`Azure`-container gebruikt Azure-resources voor opslag. Als de container wordt gestopt of verwijderd, wordt de opslag voor die container bewaard.|
| `ConnectionStringOfAzureStorage` | Tekenreeks | De connection string voor de Azure Storage resource die wordt gebruikt door de container.<br/>Deze instelling is alleen van toepassing als `Azure` is opgegeven voor de configuratie-instelling `StorageScenario`. |
| `ConnectionStringOfCosmosMongo` | Tekenreeks | De MongoDB-connection string voor de Azure Cosmos DB resource die wordt gebruikt door de container.<br/>Deze instelling is alleen van toepassing als `Azure` is opgegeven voor de configuratie-instelling `StorageScenario`. |

Met de volgende opdracht geeft u bijvoorbeeld het scenario voor Azure Storage op en geeft u voor beelden van verbindings reeksen voor de Azure Storage en Cosmos DB resources die worden gebruikt voor het opslaan van gegevens voor de face-container.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

Het opslag scenario wordt afzonderlijk van invoer koppels en uitvoer koppelingen verwerkt. U kunt een combi natie van deze functies voor één container opgeven. Met de volgende opdracht definieert u bijvoorbeeld een docker BIND koppelen aan de map `D:\Output` op de hostcomputer als uitvoer koppeling. vervolgens wordt een container geïnstantieerd van de installatie kopie van het gezicht, waarbij logboek bestanden in JSON-indeling worden opgeslagen in de uitvoer koppeling. De opdracht geeft ook het scenario voor Azure Storage op en biedt voor beelden van verbindings reeksen voor de Azure Storage en Cosmos DB resources die worden gebruikt voor het opslaan van gegevens voor de container face.

  ```Docker
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 --mount type=bind,source=D:\Output,destination=/output containerpreview.azurecr.io/microsoft/cognitive-services-face Eula=accept Billing=https://westcentralus.api.cognitive.microsoft.com/face/v1.0 ApiKey=0123456789 Logging:Disk:Format=json CloudAI:Storage:StorageScenario=Azure CloudAI:Storage:ConnectionStringOfCosmosMongo="mongodb://samplecosmosdb:0123456789@samplecosmosdb.documents.azure.com:10255/?ssl=true&replicaSet=globaldb" CloudAI:Storage:ConnectionStringOfAzureStorage="DefaultEndpointsProtocol=https;AccountName=sampleazurestorage;AccountKey=0123456789;EndpointSuffix=core.windows.net"
  ```

## <a name="eula-setting"></a>Gebruiksrecht overeenkomst instellen

[!INCLUDE [Container shared configuration eula settings](../../../includes/cognitive-services-containers-configuration-shared-settings-eula.md)]

## <a name="fluentd-settings"></a>Gefluente instellingen

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-fluentd.md)]

## <a name="http-proxy-credentials-settings"></a>Instellingen voor http-proxy referenties

[!INCLUDE [Container shared configuration fluentd settings](../../../includes/cognitive-services-containers-configuration-shared-settings-http-proxy.md)]

## <a name="logging-settings"></a>Instellingen voor logboek registratie
 
[!INCLUDE [Container shared configuration logging settings](../../../includes/cognitive-services-containers-configuration-shared-settings-logging.md)]

## <a name="mount-settings"></a>Koppelings instellingen

Gebruik bindings koppelingen om gegevens van en naar de container te lezen en te schrijven. U kunt een invoer koppeling of uitvoer koppeling opgeven door de optie `--mount` op te geven in de opdracht [docker run](https://docs.docker.com/engine/reference/commandline/run/) .

De face-containers gebruiken geen invoer-of uitvoer koppelingen om training of service gegevens op te slaan. 

De exacte syntaxis van de locatie voor het koppelen van de host varieert, afhankelijk van het besturings systeem van de host. Daarnaast is de koppel locatie van de [hostcomputer](face-how-to-install-containers.md#the-host-computer)mogelijk niet toegankelijk als gevolg van een conflict tussen de machtigingen die worden gebruikt door het docker-service account en de machtigingen voor het koppelen van de host-locatie. 

|Optioneel| Naam | Gegevenstype | Beschrijving |
|-------|------|-----------|-------------|
|Niet toegestaan| `Input` | Tekenreeks | Bij face-containers worden deze niet gebruikt.|
|Optioneel| `Output` | Tekenreeks | Het doel van de uitvoer koppeling. De standaard waarde is `/output`. Dit is de locatie van de logboeken. Dit omvat container Logboeken. <br><br>Voorbeeld:<br>`--mount type=bind,src=c:\output,target=/output`|

## <a name="example-docker-run-commands"></a>Voor beeld van docker-opdrachten uitvoeren 

De volgende voor beelden gebruiken de configuratie-instellingen om te laten zien hoe u `docker run`-opdrachten schrijft en gebruikt.  Als de container eenmaal wordt uitgevoerd, blijft deze actief totdat u deze [stopt](face-how-to-install-containers.md#stop-the-container) .

* **Regel voortzettings teken**: de docker-opdrachten in de volgende secties gebruiken de back slash, `\`, als een regel voortzettings teken. Vervang of verwijder dit op basis van de vereisten van uw host-besturings systeem. 
* **Argument volgorde**: Wijzig de volg orde van de argumenten niet, tenzij u bekend bent met docker-containers.

Vervang {_argument_name_} door uw eigen waarden:

| Tijdelijke aanduiding | Waarde | Notatie of voor beeld |
|-------------|-------|---|
| **{API_KEY}** | De eindpunt sleutel van de `Face` resource op de pagina Azure `Face`-sleutels. | `xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx` |
| **{ENDPOINT_URI}** | De waarde van het facturerings eindpunt is beschikbaar op de pagina overzicht van Azure `Face`.| Zie [vereiste para meters](face-how-to-install-containers.md#gathering-required-parameters) voor expliciete voor beelden verzamelen. |

[!INCLUDE [subdomains-note](../../../includes/cognitive-services-custom-subdomains-note.md)]

> [!IMPORTANT]
> De opties `Eula`, `Billing`en `ApiKey` moeten worden opgegeven om de container uit te voeren. anders wordt de container niet gestart.  Zie [facturering](face-how-to-install-containers.md#billing)voor meer informatie.
> De ApiKey-waarde is de **sleutel** van de pagina Azure `Cognitive Services` resource sleutels. 

## <a name="face-container-docker-examples"></a>Voor beelden van Face container docker

De volgende docker-voor beelden zijn voor de container face. 

### <a name="basic-example"></a>Basis voorbeeld 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 \
  containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={ENDPOINT_URI} \
  ApiKey={API_KEY} 
  ```

### <a name="logging-example"></a>Voor beeld van logboek registratie 

  ```
  docker run --rm -it -p 5000:5000 --memory 4g --cpus 1 containerpreview.azurecr.io/microsoft/cognitive-services-face \
  Eula=accept \
  Billing={ENDPOINT_URI} ApiKey={API_KEY} \
  Logging:Console:LogLevel:Default=Information
  ```

## <a name="next-steps"></a>Volgende stappen

* Meer [informatie over het installeren en uitvoeren van containers](face-how-to-install-containers.md)
