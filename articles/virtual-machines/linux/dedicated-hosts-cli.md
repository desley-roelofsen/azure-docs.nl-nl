---
title: Met de CLI toegewezen Azure-hosts implementeren
description: Implementeer Vm's op toegewezen hosts met behulp van de Azure CLI.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/29/2019
ms.author: cynthn
ms.openlocfilehash: ece9967321cfca44b102d78722f0df3d8f980bdb
ms.sourcegitcommit: 49cf9786d3134517727ff1e656c4d8531bbbd332
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/13/2019
ms.locfileid: "74036406"
---
# <a name="preview-deploy-vms-to-dedicated-hosts-using-the-azure-cli"></a>Voor beeld: Vm's implementeren op toegewezen hosts met behulp van de Azure CLI
 

Dit artikel begeleidt u bij het maken van een toegewezen Azure- [host](dedicated-hosts.md) voor het hosten van uw virtuele machines (vm's). 

Zorg ervoor dat u Azure CLI-versie 2.0.70 of hoger hebt geïnstalleerd en u hebt aangemeld bij een Azure-account met behulp van `az login`. 

> [!IMPORTANT]
> Exclusieve Azure-hosts bevindt zich momenteel in de open bare preview.
> Deze preview-versie wordt aangeboden zonder service level agreement en wordt niet aanbevolen voor productieworkloads. Misschien worden bepaalde functies niet ondersteund of zijn de mogelijkheden ervan beperkt. Zie [Supplemental Terms of Use for Microsoft Azure Previews (Aanvullende gebruiksvoorwaarden voor Microsoft Azure-previews)](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) voor meer informatie.
>
> **Bekende preview-beperkingen**
> - Virtuele-machine schaal sets worden momenteel niet ondersteund op toegewezen hosts.
> - De eerste release van de preview-versie ondersteunt de volgende VM-reeksen: DSv3 en ESv3. 
 

## <a name="create-resource-group"></a>Een resourcegroep maken 
Een Azure-resourcegroep is een logische container waarin Azure-resources worden geïmplementeerd en beheerd. Maak de resource groep met AZ Group Create. In het volgende voor beeld wordt een resource groep met de naam *myDHResourceGroup* gemaakt op de locatie *VS-Oost* .

```bash
az group create --name myDHResourceGroup --location eastus 
```
 
## <a name="create-a-host-group"></a>Een hostgroep maken 

Een **hostgroep** is een resource die een verzameling toegewezen hosts vertegenwoordigt. U maakt een hostgroep in een regio en een beschikbaarheids zone en voegt hierop hosts toe. Bij het plannen van hoge Beschik baarheid zijn er extra opties. U kunt een of beide van de volgende opties gebruiken met uw toegewezen hosts: 
- Beschik over meerdere beschikbaarheids zones. In dit geval moet u een hostgroep hebben in elk van de zones die u wilt gebruiken.
- Over meerdere fout domeinen die zijn toegewezen aan fysieke racks. 
 
In beide gevallen moet u het aantal fout domeinen voor uw hostgroep opgeven. Als u geen fout domeinen in uw groep wilt beslaan, gebruikt u het aantal fouten domein 1. 

U kunt er ook voor kiezen om zowel beschikbaarheids zones als fout domeinen te gebruiken. 

In dit voor beeld wordt [AZ VM host Group Create](/cli/azure/vm/host/group#az-vm-host-group-create) gebruikt om een hostgroep te maken met behulp van zowel beschikbaarheids zones als fout domeinen. 

```bash
az vm host group create \
   --name myHostGroup \
   -g myDHResourceGroup \
   -z 1 \
   --platform-fault-domain-count 2 
``` 

### <a name="other-examples"></a>Andere voor beelden

U kunt ook [AZ VM host Group Create](/cli/azure/vm/host/group#az-vm-host-group-create) gebruiken om een hostgroep te maken in beschikbaarheids zone 1 (en geen fout domeinen).

```bash
az vm host group create \
   --name myAZHostGroup \
   -g myDHResourceGroup \
   -z 1 \
   --platform-fault-domain-count 1 
```
 
Het volgende maakt gebruik van [AZ VM host Group Create](/cli/azure/vm/host/group#az-vm-host-group-create) om een hostgroep te maken met behulp van alleen fout domeinen (die moeten worden gebruikt in regio's waar beschikbaarheids zones niet worden ondersteund). 

```bash
az vm host group create \
   --name myFDHostGroup \
   -g myDHResourceGroup \
   --platform-fault-domain-count 2 
```
 
## <a name="create-a-host"></a>Een host maken 

We gaan nu een toegewezen host maken in de hostgroep. Naast een naam voor de host, moet u de SKU voor de host opgeven. Host SKU legt de ondersteunde VM-serie en de generatie van de hardware voor uw specifieke host vast.  Tijdens de preview worden de volgende SKU-waarden voor de host ondersteund: DSv3_Type1 en ESv3_Type1.


Zie voor meer informatie over de Sku's en prijzen van de host de [Azure dedicated host prijzen](https://aka.ms/ADHPricing).

Gebruik [AZ VM host Create](/cli/azure/vm/host#az-vm-host-create) om een host te maken. Als u het aantal fout domeinen voor uw hostgroep instelt, wordt u gevraagd om het fout domein voor uw host op te geven.  

```bash
az vm host create \
   --host-group myHostGroup \
   --name myHost \
   --sku DSv3-Type1 \
   --platform-fault-domain 1 \
   -g myDHResourceGroup
```


 
## <a name="create-a-virtual-machine"></a>Een virtuele machine maken 
Maak een virtuele machine binnen een speciale host met behulp van [AZ VM Create](/cli/azure/vm#az-vm-create). Als u een beschikbaarheids zone hebt opgegeven bij het maken van uw hostgroep, moet u dezelfde zone gebruiken bij het maken van de virtuele machine.

```bash
az vm create \
   -n myVM \
   --image debian \
   --generate-ssh-keys \
   --host-group myHostGroup \
   --host myHost \
   --generate-ssh-keys \
   --size Standard_D4s_v3 \
   -g myDHResourceGroup \
   --zone 1
```
 
> [!WARNING]
> Als u een virtuele machine maakt op een host die onvoldoende bronnen heeft, wordt de virtuele machine gemaakt met de status mislukt. 


## <a name="check-the-status-of-the-host"></a>Controleer de status van de host

U kunt de status van de host controleren en het aantal virtuele machines dat u nog steeds op de host kunt implementeren met behulp van [AZ VM host Get-instance-View](/cli/azure/vm/host#az-vm-host-get-instance-view).

```bash
az vm host get-instance-view \
   -g myDHResourceGroup \
   --host-group myHostGroup \
   --name myHost
```
 De uitvoer ziet er ongeveer als volgt uit:
 
```json
{
  "autoReplaceOnFailure": true,
  "hostId": "6de80643-0f45-4e94-9a4c-c49d5c777b62",
  "id": "/subscriptions/10101010-1010-1010-1010-101010101010/resourceGroups/myDHResourceGroup/providers/Microsoft.Compute/hostGroups/myHostGroup/hosts/myHost",
  "instanceView": {
    "assetId": "12345678-1234-1234-abcd-abc123456789",
    "availableCapacity": {
      "allocatableVms": [
        {
          "count": 31.0,
          "vmSize": "Standard_D2s_v3"
        },
        {
          "count": 15.0,
          "vmSize": "Standard_D4s_v3"
        },
        {
          "count": 7.0,
          "vmSize": "Standard_D8s_v3"
        },
        {
          "count": 3.0,
          "vmSize": "Standard_D16s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32-8s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32-16s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D32s_v3"
        },
        {
          "count": 1.0,
          "vmSize": "Standard_D48s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64-16s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64-32s_v3"
        },
        {
          "count": 0.0,
          "vmSize": "Standard_D64s_v3"
        }
      ]
    },
    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "displayStatus": "Provisioning succeeded",
        "level": "Info",
        "message": null,
        "time": "2019-07-24T21:22:40.604754+00:00"
      },
      {
        "code": "HealthState/available",
        "displayStatus": "Host available",
        "level": "Info",
        "message": null,
        "time": null
      }
    ]
  },
  "licenseType": null,
  "location": "eastus2",
  "name": "myHost",
  "platformFaultDomain": 1,
  "provisioningState": "Succeeded",
  "provisioningTime": "2019-07-24T21:22:40.604754+00:00",
  "resourceGroup": "myDHResourceGroup",
  "sku": {
    "capacity": null,
    "name": "DSv3-Type1",
    "tier": null
  },
  "tags": null,
  "type": null,
  "virtualMachines": [
    {
      "id": "/subscriptions/10101010-1010-1010-1010-101010101010/resourceGroups/MYDHRESOURCEGROUP/providers/Microsoft.Compute/virtualMachines/MYVM",
      "resourceGroup": "MYDHRESOURCEGROUP"
    }
  ]
}

```
 
## <a name="export-as-a-template"></a>Exporteren als een sjabloon 
U kunt een sjabloon exporteren als u nu een extra ontwikkel omgeving met dezelfde para meters of een productie omgeving wilt maken die overeenkomt met deze. Resource Manager maakt gebruik van JSON-sjablonen waarmee alle para meters voor uw omgeving worden gedefinieerd. U bouwt volledige omgevingen door te verwijzen naar deze JSON-sjabloon. U kunt JSON-sjablonen hand matig maken of een bestaande omgeving exporteren om de JSON-sjabloon voor u te maken. Gebruik [AZ Group export](/cli/azure/group#az-group-export) om de resource groep te exporteren.

```bash
az group export --name myDHResourceGroup > myDHResourceGroup.json 
```

Met deze opdracht maakt u het `myDHResourceGroup.json`-bestand in de huidige werkmap. Wanneer u een omgeving maakt op basis van deze sjabloon, wordt u gevraagd om alle resource namen. U kunt deze namen invullen in het sjabloon bestand door de para meter `--include-parameter-default-value` toe te voegen aan de `az group export` opdracht. Bewerk de JSON-sjabloon om de resource namen op te geven of maak een JSON-bestand waarin de resource namen worden opgegeven.
 
Gebruik [AZ Group Deployment Create](/cli/azure/group/deployment#az-group-deployment-create)om een omgeving te maken op basis van uw sjabloon.

```bash
az group deployment create \ 
    --resource-group myNewResourceGroup \ 
    --template-file myDHResourceGroup.json 
```


## <a name="clean-up"></a>Opruimen 

Er worden kosten in rekening gebracht voor uw specifieke hosts, zelfs wanneer er geen virtuele machines zijn geïmplementeerd. U moet alle hosts die u momenteel gebruikt, verwijderen om kosten te besparen.  

U kunt een host alleen verwijderen als er geen virtuele machines meer worden gebruikt. Verwijder de virtuele machines met [AZ VM delete](/cli/azure/vm#az-vm-delete).

```bash
az vm delete -n myVM -g myDHResourceGroup
```

Nadat u de Vm's hebt verwijderd, kunt u de host verwijderen met [AZ VM host delete](/cli/azure/vm/host#az-vm-host-delete).

```bash
az vm host delete -g myDHResourceGroup --host-group myHostGroup --name myHost 
```
 
Zodra u al uw hosts hebt verwijderd, kunt u de hostgroep verwijderen met [AZ VM host group delete](/cli/azure/vm/host/group#az-vm-host-group-delete).  
 
```bash
az vm host group delete -g myDHResourceGroup --host-group myHostGroup  
```
 
U kunt ook de hele resource groep verwijderen in één opdracht. Hiermee verwijdert u alle resources die zijn gemaakt in de groep, inclusief alle Vm's, hosts en hostgroepen.
 
```bash
az group delete -n myDHResourceGroup 
```

## <a name="next-steps"></a>Volgende stappen

- Zie voor meer informatie het overzicht [gespecialiseerde hosts](dedicated-hosts.md) .

- U kunt ook toegewezen hosts maken met behulp van de [Azure Portal](dedicated-hosts-portal.md).

- [Hier](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)vindt u een voor beeld van een sjabloon, die zowel zones als fout domeinen gebruikt voor maximale tolerantie in een regio.