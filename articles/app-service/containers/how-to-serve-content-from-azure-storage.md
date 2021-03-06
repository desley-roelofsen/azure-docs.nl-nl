---
title: Aangepaste opslag container aan linux koppelen
description: Meer informatie over het koppelen van een aangepaste netwerk share aan uw Linux-container in Azure App Service. Bestanden delen tussen apps, statische inhoud extern beheren en lokaal toegang krijgen, enzovoort.
author: msangapu-msft
ms.topic: article
ms.date: 2/04/2019
ms.author: msangapu
ms.openlocfilehash: 00c60edeefa5fd8d1304aa5fc301a3b0304f5ca3
ms.sourcegitcommit: 265f1d6f3f4703daa8d0fc8a85cbd8acf0a17d30
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/02/2019
ms.locfileid: "74671791"
---
# <a name="attach-azure-storage-containers-to-linux-containers"></a>Azure Storage containers koppelen aan linux-containers

In deze hand leiding wordt beschreven hoe u netwerk shares koppelt aan App Service op Linux met behulp van [Azure Storage](/azure/storage/common/storage-introduction). Voor delen zijn onder andere beveiligde inhoud, draag baarheid van inhoud, permanente opslag, toegang tot meerdere apps en meerdere overdrachts methoden.

## <a name="prerequisites"></a>Vereisten

- Een bestaande web-app (App Service op Linux of Web App for Containers).
- [Azure cli](/cli/azure/install-azure-cli) (2.0.46 of hoger).

## <a name="create-azure-storage"></a>Azure Storage maken

> [!NOTE]
> Azure Storage is niet-standaard opslag en wordt afzonderlijk gefactureerd, niet opgenomen in de web-app.
>
> Uw eigen opslag biedt geen ondersteuning voor het gebruik van de opslag firewall-configuratie vanwege beperkingen van de infra structuur.
>

Maak een Azure [Azure-opslag account](https://docs.microsoft.com/azure/storage/common/storage-quickstart-create-account?tabs=azure-cli).

```azurecli
#Create Storage Account
az storage account create --name <storage_account_name> --resource-group myResourceGroup

#Create Storage Container
az storage container create --name <storage_container_name> --account-name <storage_account_name>
```

## <a name="upload-files-to-azure-storage"></a>Bestanden uploaden naar Azure Storage

Als u een lokale map wilt uploaden naar het opslag account, gebruikt u de [`az storage blob upload-batch`](https://docs.microsoft.com/cli/azure/storage/blob?view=azure-cli-latest#az-storage-blob-upload-batch) opdracht zoals in het volgende voor beeld:

```azurecli
az storage blob upload-batch -d <full_path_to_local_directory> --account-name <storage_account_name> --account-key "<access_key>" -s <source_location_name>
```

## <a name="link-storage-to-your-web-app-preview"></a>Opslag koppelen aan uw web-app (preview)

> [!CAUTION]
> Als u een bestaande map in een web-app aan een opslag account koppelt, wordt de inhoud van de map verwijderd. Als u bestanden migreert voor een bestaande app, maakt u een back-up van uw app en de bijbehorende inhoud voordat u begint.
>

Als u een opslag account wilt koppelen aan een map in uw App Service-app, gebruikt u de [`az webapp config storage-account add`](https://docs.microsoft.com/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-add) opdracht. Het opslag type kan AzureBlob of Azure files zijn. U gebruikt AzureBlob voor deze container.

```azurecli
az webapp config storage-account add --resource-group <group_name> --name <app_name> --custom-id <custom_id> --storage-type AzureBlob --share-name <share_name> --account-name <storage_account_name> --access-key "<access_key>" --mount-path <mount_path_directory>
```

U moet dit doen voor alle andere directory's die u aan een opslag account wilt koppelen.

## <a name="verify"></a>Verifiëren

Zodra een opslag container is gekoppeld aan een web-app, kunt u dit controleren door de volgende opdracht uit te voeren:

```azurecli
az webapp config storage-account list --resource-group <resource_group> --name <app_name>
```

## <a name="use-custom-storage-in-docker-compose"></a>Aangepaste opslag gebruiken in docker-samen stellen

Azure Storage kunnen worden gekoppeld met apps met meerdere containers met behulp van de aangepaste ID. Voer [`az webapp config storage-account list --name <app_name> --resource-group <resource_group>`](/cli/azure/webapp/config/storage-account?view=azure-cli-latest#az-webapp-config-storage-account-list)uit om de naam van de aangepaste ID weer te geven.

Wijs in uw *docker-Compose. yml* -bestand de optie `volumes` toe aan `custom-id`. Bijvoorbeeld:

```yaml
wordpress:
  image: wordpress:latest
  volumes:
  - <custom-id>:<path_in_container>
```

## <a name="next-steps"></a>Volgende stappen

- [Web-apps configureren in azure app service](../configure-common.md).
