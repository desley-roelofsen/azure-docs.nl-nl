---
title: Zelf studie-een groeps sjabloon met meerdere containers implementeren
description: In deze zelf studie leert u hoe u een container groep met meerdere containers in Azure Container Instances kunt implementeren met behulp van een Azure Resource Manager sjabloon met de Azure CLI.
ms.topic: article
ms.date: 04/03/2019
ms.custom: mvc
ms.openlocfilehash: d2b4e20520cad28c5d62118f6c9d10fcc43ac89e
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533626"
---
# <a name="tutorial-deploy-a-multi-container-group-using-a-resource-manager-template"></a>Zelf studie: een groep met meerdere containers implementeren met behulp van een resource manager-sjabloon

> [!div class="op_single_selector"]
> * [YAML](container-instances-multi-container-yaml.md)
> * [Resource Manager](container-instances-multi-container-group.md)

Azure Container Instances ondersteunt de implementatie van meerdere containers op één host met behulp van een [container groep](container-instances-container-groups.md). Een container groep is handig bij het bouwen van een sollicitatie stick voor logboek registratie, bewaking of andere configuraties waarbij een service een tweede gekoppelde procedure nodig heeft.

In deze zelf studie voert u de stappen uit voor het uitvoeren van een eenvoudige configuratie voor twee containers door een Azure Resource Manager-sjabloon te implementeren met behulp van de Azure CLI. In deze zelfstudie leert u procedures om het volgende te doen:

> [!div class="checklist"]
> * Een sjabloon voor een groep met meerdere containers configureren
> * De container groep implementeren
> * De logboeken van de containers weer geven

Een resource manager-sjabloon kan gemakkelijk worden aangepast voor scenario's waarin u aanvullende Azure-service bronnen (bijvoorbeeld een Azure Files share of een virtueel netwerk) moet implementeren met de container groep. 

> [!NOTE]
> Groepen met meerdere containers zijn momenteel beperkt tot Linux-containers. 

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="configure-a-template"></a>Een sjabloon configureren

Begin door de volgende JSON te kopiëren naar een nieuw bestand met de naam `azuredeploy.json`. In Azure Cloud Shell kunt u Visual Studio code gebruiken om het bestand in uw werkmap te maken:

```
code azuredeploy.json
```

In deze resource manager-sjabloon wordt een container groep met twee containers, een openbaar IP-adres en twee blootgestelde poorten gedefinieerd. De eerste container in de groep voert een Internet gerichte webtoepassing uit. De tweede container, de zijspan wagen, maakt een HTTP-aanvraag voor de hoofd webtoepassing via het lokale netwerk van de groep.

```JSON
{
  "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "containerGroupName": {
      "type": "string",
      "defaultValue": "myContainerGroup",
      "metadata": {
        "description": "Container Group name."
      }
    }
  },
  "variables": {
    "container1name": "aci-tutorial-app",
    "container1image": "mcr.microsoft.com/azuredocs/aci-helloworld:latest",
    "container2name": "aci-tutorial-sidecar",
    "container2image": "mcr.microsoft.com/azuredocs/aci-tutorial-sidecar"
  },
  "resources": [
    {
      "name": "[parameters('containerGroupName')]",
      "type": "Microsoft.ContainerInstance/containerGroups",
      "apiVersion": "2018-10-01",
      "location": "[resourceGroup().location]",
      "properties": {
        "containers": [
          {
            "name": "[variables('container1name')]",
            "properties": {
              "image": "[variables('container1image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              },
              "ports": [
                {
                  "port": 80
                },
                {
                  "port": 8080
                }
              ]
            }
          },
          {
            "name": "[variables('container2name')]",
            "properties": {
              "image": "[variables('container2image')]",
              "resources": {
                "requests": {
                  "cpu": 1,
                  "memoryInGb": 1.5
                }
              }
            }
          }
        ],
        "osType": "Linux",
        "ipAddress": {
          "type": "Public",
          "ports": [
            {
              "protocol": "tcp",
              "port": "80"
            },
            {
                "protocol": "tcp",
                "port": "8080"
            }
          ]
        }
      }
    }
  ],
  "outputs": {
    "containerIPv4Address": {
      "type": "string",
      "value": "[reference(resourceId('Microsoft.ContainerInstance/containerGroups/', parameters('containerGroupName'))).ipAddress.ip]"
    }
  }
}
```

Als u een REGI ster van een persoonlijke container installatie kopie wilt gebruiken, voegt u een object toe aan het JSON-document met de volgende indeling. Zie voor een voorbeeld implementatie van deze configuratie de documentatie over de [ACI Resource Manager-sjabloon][template-reference] .

```JSON
"imageRegistryCredentials": [
  {
    "server": "[parameters('imageRegistryLoginServer')]",
    "username": "[parameters('imageRegistryUsername')]",
    "password": "[parameters('imageRegistryPassword')]"
  }
]
```

## <a name="deploy-the-template"></a>De sjabloon implementeren

Een resourcegroep maken met de opdracht [az group create][az-group-create].

```azurecli-interactive
az group create --name myResourceGroup --location eastus
```

Implementeer de sjabloon met de opdracht [AZ Group Deployment Create][az-group-deployment-create] .

```azurecli-interactive
az group deployment create --resource-group myResourceGroup --template-file azuredeploy.json
```

U ontvangt binnen enkele seconden een eerste reactie van Azure.

## <a name="view-deployment-state"></a>Implementatie status weer geven

Als u de status van de implementatie wilt weer geven, gebruikt u de volgende opdracht [AZ container show][az-container-show] :

```azurecli-interactive
az container show --resource-group myResourceGroup --name myContainerGroup --output table
```

Als u de actieve toepassing wilt bekijken, gaat u naar het IP-adres in uw browser. Het IP-adres is bijvoorbeeld `52.168.26.124` in deze voorbeeld uitvoer:

```bash
Name              ResourceGroup    Status    Image                                                                                               IP:ports              Network    CPU/Memory       OsType    Location
----------------  ---------------  --------  --------------------------------------------------------------------------------------------------  --------------------  ---------  ---------------  --------  ----------
myContainerGroup  danlep0318r      Running   mcr.microsoft.com/azuredocs/aci-tutorial-sidecar,mcr.microsoft.com/azuredocs/aci-helloworld:latest  20.42.26.114:80,8080  Public     1.0 core/1.5 gb  Linux     eastus
```

## <a name="view-container-logs"></a>Containerlogboeken ophalen

Bekijk de logboek uitvoer van een container met behulp van de opdracht [AZ container logs][az-container-logs] . Met het argument `--container-name` wordt de container opgegeven waaruit logboeken moeten worden opgehaald. In dit voor beeld wordt de `aci-tutorial-app`-container opgegeven.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-app
```

Uitvoer:

```bash
listening on port 80
::1 - - [21/Mar/2019:23:17:48 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:51 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
::1 - - [21/Mar/2019:23:17:54 +0000] "HEAD / HTTP/1.1" 200 1663 "-" "curl/7.54.0"
```

Als u de logboeken voor de container met zijspaners wilt weer geven, voert u een vergelijk bare opdracht uit die de `aci-tutorial-sidecar`-container opgeeft.

```azurecli-interactive
az container logs --resource-group myResourceGroup --name myContainerGroup --container-name aci-tutorial-sidecar
```

Uitvoer:

```bash
Every 3s: curl -I http://localhost                          2019-03-21 20:36:41

  % Total    % Received % Xferd  Average Speed   Time    Time     Time  Current
                                 Dload  Upload   Total   Spent    Left  Speed
  0  1663    0     0    0     0      0      0 --:--:-- --:--:-- --:--:--     0
HTTP/1.1 200 OK
X-Powered-By: Express
Accept-Ranges: bytes
Cache-Control: public, max-age=0
Last-Modified: Wed, 29 Nov 2017 06:40:40 GMT
ETag: W/"67f-16006818640"
Content-Type: text/html; charset=UTF-8
Content-Length: 1663
Date: Thu, 21 Mar 2019 20:36:41 GMT
Connection: keep-alive
```

Zoals u ziet, maakt de zijspan wagen regel matig een HTTP-aanvraag voor de hoofd webtoepassing via het lokale netwerk van de groep om ervoor te zorgen dat deze wordt uitgevoerd. Dit voor beeld van een zijspan wagen kan worden uitgebreid om een waarschuwing te activeren als het een andere HTTP-antwoord code dan `200 OK`heeft ontvangen.

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een Azure Resource Manager sjabloon gebruikt om een groep met meerdere containers in Azure Container Instances te implementeren. U hebt geleerd hoe u:

> [!div class="checklist"]
> * Een sjabloon voor een groep met meerdere containers configureren
> * De container groep implementeren
> * De logboeken van de containers weer geven

Zie [Azure Resource Manager sjablonen voor Azure container instances](container-instances-samples-rm.md)voor aanvullende sjabloon voorbeelden.

U kunt ook een groep met meerdere containers opgeven met behulp van een [yaml-bestand](container-instances-multi-container-yaml.md). Omdat de indeling van de YAML beknoptere aard is, is implementatie met een YAML-bestand een goede keuze wanneer uw implementatie alleen container instanties bevat.


<!-- LINKS - Internal -->
[aci-tutorial]: ./container-instances-tutorial-prepare-app.md
[az-container-logs]: /cli/azure/container#az-container-logs
[az-container-show]: /cli/azure/container#az-container-show
[az-group-create]: /cli/azure/group#az-group-create
[az-group-deployment-create]: /cli/azure/group/deployment#az-group-deployment-create
[template-reference]: https://docs.microsoft.com/azure/templates/microsoft.containerinstance/containergroups
