---
title: Container installatie kopie implementeren vanaf Azure Container Registry
description: Meer informatie over het implementeren van containers in Azure Container Instances met behulp van container installatie kopieën in een Azure container Registry.
services: container-instances
ms.topic: article
ms.date: 01/04/2019
ms.author: danlep
ms.custom: mvc
ms.openlocfilehash: adc2c95874c1cc20e49506891c9972ebcfe71f94
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533293"
---
# <a name="deploy-to-azure-container-instances-from-azure-container-registry"></a>Implementeren in Azure Container Instances van Azure Container Registry

[Azure container Registry](../container-registry/container-registry-intro.md) is een beheerde container register service in azure die wordt gebruikt voor het opslaan van privé-docker-container installatie kopieën. In dit artikel wordt beschreven hoe u container installatie kopieën die zijn opgeslagen in een Azure container Registry, kunt implementeren in Azure Container Instances.

## <a name="prerequisites"></a>Vereisten

**Azure container Registry**: u hebt een Azure container Registry en ten minste één container installatie kopie in het REGI ster nodig om de stappen in dit artikel te volt ooien. Als u een REGI ster nodig hebt, raadpleegt u [een container register maken met de Azure cli](../container-registry/container-registry-get-started-azure-cli.md).

**Azure cli**: de opdracht regel voorbeelden in dit artikel maken gebruik van de [Azure cli](/cli/azure/) en zijn geformatteerd voor de bash-shell. U kunt [de Azure cli lokaal installeren](/cli/azure/install-azure-cli) of de [Azure Cloud shell][cloud-shell-bash]gebruiken.

## <a name="configure-registry-authentication"></a>Registerverificatie configureren

In elk productie scenario moet de toegang tot een Azure container Registry worden gegeven met [service-principals](../container-registry/container-registry-auth-service-principal.md). Met Service-principals kunt u [toegangs beheer op basis van rollen](../container-registry/container-registry-roles.md) bieden aan uw container installatie kopieën. U kunt bijvoorbeeld een service-principal configureren met alleen pull-toegang tot een register.

In de volgende sectie maakt u een Azure-sleutel kluis en een Service-Principal en slaat u de referenties van de Service-Principal op in de kluis. 

### <a name="create-key-vault"></a>Sleutelkluis maken

Als u nog geen kluis hebt in [Azure Key Vault](../key-vault/key-vault-overview.md), kunt u met de volgende opdrachten één maken met de Azure CLI.

Werk de `RES_GROUP` variabele met de naam van een bestaande resource groep waarin u de sleutel kluis wilt maken en `ACR_NAME` met de naam van uw container register. Geef een naam op voor de nieuwe sleutel kluis in `AKV_NAME`. De kluis naam moet uniek zijn binnen Azure en moet 3-24 alfanumerieke tekens lang zijn, beginnen met een letter, eindigen met een letter of cijfer en mag geen opeenvolgende afbreek streepjes bevatten.

```azurecli
RES_GROUP=myresourcegroup # Resource Group name
ACR_NAME=myregistry       # Azure Container Registry registry name
AKV_NAME=mykeyvault       # Azure Key Vault vault name

az keyvault create -g $RES_GROUP -n $AKV_NAME
```

### <a name="create-service-principal-and-store-credentials"></a>Service-principal maken en referenties opslaan

U moet nu een service-principal maken en de referenties ervan opslaan in uw sleutelkluis.

De volgende opdracht maakt gebruik [van AZ AD SP create-for-RBAC][az-ad-sp-create-for-rbac] om de service-principal te maken en [AZ sleutel kluis Secret set][az-keyvault-secret-set] om het **wacht woord** van de Service-Principal op te slaan in de kluis.

```azurecli
# Create service principal, store its password in AKV (the registry *password*)
az keyvault secret set \
  --vault-name $AKV_NAME \
  --name $ACR_NAME-pull-pwd \
  --value $(az ad sp create-for-rbac \
                --name http://$ACR_NAME-pull \
                --scopes $(az acr show --name $ACR_NAME --query id --output tsv) \
                --role acrpull \
                --query password \
                --output tsv)
```

Het argument `--role` in de voorgaande opdracht configureert de service-principal met de rol *acrpull*, die de principal alleen-push-toegang tot het register geeft. Als u zowel push-als pull-toegang wilt geven, wijzigt u het argument `--role` in *acrpush*.

Sla vervolgens de *AppID* van de Service-Principal op in de kluis. Dit is de **gebruikers naam** die u doorgeeft aan Azure container Registry voor authenticatie.

```azurecli
# Store service principal ID in AKV (the registry *username*)
az keyvault secret set \
    --vault-name $AKV_NAME \
    --name $ACR_NAME-pull-usr \
    --value $(az ad sp show --id http://$ACR_NAME-pull --query appId --output tsv)
```

U hebt een Azure Key Vault gemaakt en er twee geheimen in opgeslagen:

* `$ACR_NAME-pull-usr`: de service principal-ID, voor gebruik als de **gebruikersnaam** van het containerregister.
* `$ACR_NAME-pull-pwd`: het service principal-wachtwoord, voor gebruik als het **wachtwoord** van het containerregister.

U kunt nu op naam naar deze geheime gegevens verwijzen wanneer u of uw toepassingen en services installatiekopieën uit het register halen.

## <a name="deploy-container-with-azure-cli"></a>Container implementeren met Azure CLI

Nu de referenties van de Service-Principal zijn opgeslagen in Azure Key Vault geheimen, kunnen uw toepassingen en services deze gebruiken om toegang te krijgen tot uw persoonlijke REGI ster.

Haal eerst de naam van de aanmeldings server van het REGI ster op met behulp van de opdracht [AZ ACR show][az-acr-show] . De naam van de aanmeldings server is allemaal kleine letters en is vergelijkbaar met `myregistry.azurecr.io`.

```azurecli
ACR_LOGIN_SERVER=$(az acr show --name $ACR_NAME --resource-group $RES_GROUP --query "loginServer" --output tsv)
```

Voer de volgende opdracht [AZ container Create][az-container-create] uit om een container exemplaar te implementeren. De opdracht gebruikt de referenties van de service-principal die zijn opgeslagen in Azure Key Vault om te verifiëren bij het container register en gaat ervan uit dat u de installatie kopie van [ACI-HelloWorld](container-instances-quickstart.md) eerder hebt gepusht naar uw REGI ster. Werk de `--image` waarde bij als u een andere installatie kopie wilt gebruiken uit het REGI ster.

```azurecli
az container create \
    --name aci-demo \
    --resource-group $RES_GROUP \
    --image $ACR_LOGIN_SERVER/aci-helloworld:v1 \
    --registry-login-server $ACR_LOGIN_SERVER \
    --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) \
    --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) \
    --dns-name-label aci-demo-$RANDOM \
    --query ipAddress.fqdn
```

De `--dns-name-label` waarde moet uniek zijn in azure, dus de voor gaande opdracht voegt een wille keurig getal toe aan het DNS-naam label van de container. De uitvoer van de opdracht geeft bijvoorbeeld de Fully Qualified Domain Name (FQDN) van de container weer:

```console
$ az container create --name aci-demo --resource-group $RES_GROUP --image $ACR_LOGIN_SERVER/aci-helloworld:v1 --registry-login-server $ACR_LOGIN_SERVER --registry-username $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-usr --query value -o tsv) --registry-password $(az keyvault secret show --vault-name $AKV_NAME -n $ACR_NAME-pull-pwd --query value -o tsv) --dns-name-label aci-demo-$RANDOM --query ipAddress.fqdn
"aci-demo-25007.eastus.azurecontainer.io"
```

Zodra de container is gestart, kunt u naar de FQDN in uw browser navigeren om te controleren of de toepassing correct wordt uitgevoerd.

## <a name="deploy-with-azure-resource-manager-template"></a>Implementeren met Azure Resource Manager sjabloon

U kunt de eigenschappen van uw Azure Container Registry opgeven in een Azure Resource Manager sjabloon door de eigenschap `imageRegistryCredentials` op te nemen in de definitie van de container groep:

```JSON
"imageRegistryCredentials": [
  {
    "server": "imageRegistryLoginServer",
    "username": "imageRegistryUsername",
    "password": "imageRegistryPassword"
  }
]
```

Zie [Azure Key Vault gebruiken om een veilige parameter waarde door te geven tijdens de implementatie](../azure-resource-manager/resource-manager-keyvault-parameter.md)voor meer informatie over het verwijzen naar Azure Key Vault geheimen in een resource manager-sjabloon.

## <a name="deploy-with-azure-portal"></a>Implementeren met Azure Portal

Als u container installatie kopieën in een Azure container Registry onderhoudt, kunt u eenvoudig een container maken in Azure Container Instances met behulp van de Azure Portal. Wanneer u de portal gebruikt om een container exemplaar te implementeren vanuit een container register, moet u het [beheerders account](../container-registry/container-registry-authentication.md#admin-account)van het REGI ster inschakelen. Het beheerders account is ontworpen voor één gebruiker voor toegang tot het REGI ster, voornamelijk voor test doeleinden. 

1. Ga in het Azure Portal naar het container register.

1. Als u wilt controleren of het beheerders account is ingeschakeld, selecteert u **toegangs sleutels**en selecteert u onder **gebruiker met beheerders** rechten **inschakelen**.

1. Selecteer **opslag**plaatsen, selecteer de opslag plaats waaruit u wilt implementeren, klik met de rechter muisknop op de tag voor de container installatie kopie die u wilt implementeren en selecteer **instantie uitvoeren**.

    ![' Instantie uitvoeren ' in Azure Container Registry in het Azure Portal][acr-runinstance-contextmenu]

1. Voer een naam in voor de container en een naam voor de resource groep. U kunt desgewenst ook de standaard waarden wijzigen.

    ![Menu maken voor Azure Container Instances][acr-create-deeplink]

1. Zodra de implementatie is voltooid, kunt u naar de container groep in het deel venster meldingen navigeren om het IP-adres en andere eigenschappen te vinden.

    ![Detail weergave voor Azure Container Instances container groep][aci-detailsview]

## <a name="next-steps"></a>Volgende stappen

Zie [verifiëren met een Azure container Registry](../container-registry/container-registry-authentication.md)voor meer informatie over Azure container Registry-verificatie.

<!-- IMAGES -->
[acr-create-deeplink]: ./media/container-instances-using-azure-container-registry/acr-create-deeplink.png
[aci-detailsview]: ./media/container-instances-using-azure-container-registry/aci-detailsview.png
[acr-runinstance-contextmenu]: ./media/container-instances-using-azure-container-registry/acr-runinstance-contextmenu.png

<!-- LINKS - External -->
[cloud-shell-bash]: https://shell.azure.com/bash
[cloud-shell-try-it]: https://shell.azure.com/powershell

<!-- LINKS - Internal -->
[az-acr-show]: /cli/azure/acr#az-acr-show
[az-ad-sp-create-for-rbac]: /cli/azure/ad/sp#az-ad-sp-create-for-rbac
[az-container-create]: /cli/azure/container#az-container-create
[az-keyvault-secret-set]: /cli/azure/keyvault/secret#az-keyvault-secret-set
