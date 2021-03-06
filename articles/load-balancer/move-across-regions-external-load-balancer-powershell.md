---
title: Verplaats Azure externe Load Balancer naar een andere Azure-regio met behulp van Azure PowerShell
description: Gebruik Azure Resource Manager sjabloon om externe Azure-Load Balancer van een Azure-regio naar een andere te verplaatsen met behulp van Azure PowerShell.
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/17/2019
ms.author: allensu
ms.openlocfilehash: d8dfbf3f86a2233571a99c4ad832ef7bd3c3ed48
ms.sourcegitcommit: 8ef0a2ddaece5e7b2ac678a73b605b2073b76e88
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/17/2019
ms.locfileid: "71077579"
---
# <a name="move-azure-external-load-balancer-to-another-region-using-azure-powershell"></a>Externe Azure-Load Balancer verplaatsen naar een andere regio met behulp van Azure PowerShell

Er zijn verschillende scenario's waarin u uw bestaande externe load balancer van de ene naar de andere regio wilt verplaatsen. U kunt bijvoorbeeld een externe load balancer maken met dezelfde configuratie voor testen. Het is ook mogelijk dat u een externe load balancer naar een andere regio wilt verplaatsen als onderdeel van de planning voor nood herstel.

Externe Azure load balancers kunnen niet van de ene regio naar de andere worden verplaatst. U kunt echter een Azure Resource Manager sjabloon gebruiken om de bestaande configuratie en het open bare IP-adres van een externe load balancer te exporteren.  U kunt de resource vervolgens in een andere regio zetten door de load balancer en het open bare IP-adres naar een sjabloon te exporteren, de para meters te wijzigen zodat deze overeenkomen met de doel regio en vervolgens de sjablonen te implementeren in de nieuwe regio.  Zie [resource groepen exporteren naar sjablonen](https://docs.microsoft.com/azure/azure-resource-manager/manage-resource-groups-powershell#export-resource-groups-to-templates) voor meer informatie over Resource Manager en sjablonen


## <a name="prerequisites"></a>Vereisten

- Zorg ervoor dat de externe Azure-load balancer zich bevindt in de Azure-regio van waaruit u wilt verplaatsen.

- Externe Azure load balancers kunnen niet tussen regio's worden verplaatst.  U moet de nieuwe load balancer koppelen aan resources in de doel regio.

- Als u een externe load balancer configuratie wilt exporteren en een sjabloon wilt implementeren voor het maken van een externe load balancer in een andere regio, hebt u de rol netwerk bijdrager of hoger nodig.
   
- Identificeer de bron netwerk indeling en alle resources die u momenteel gebruikt. Deze indeling bevat, maar is niet beperkt tot load balancers, netwerk beveiligings groepen, open bare Ip's en virtuele netwerken.

- Controleer of u met uw Azure-abonnement externe load balancers kunt maken in de doel regio die wordt gebruikt. Neem contact op met ondersteuning voor het inschakelen van het vereiste quotum.

- Zorg ervoor dat uw abonnement voldoende bronnen heeft ter ondersteuning van het toevoegen van load balancers voor dit proces.  Zie [Azure-abonnement en service limieten, quota's en beperkingen](https://docs.microsoft.com/azure/azure-subscription-service-limits#networking-limits)


## <a name="prepare-and-move"></a>Voorbereiden en verplaatsen
De volgende stappen laten zien hoe u de externe load balancer voorbereidt voor de verplaatsing met behulp van een resource manager-sjabloon en de externe load balancer configuratie verplaatst naar de doel regio met behulp van Azure PowerShell.  Als onderdeel van dit proces moet de open bare IP-configuratie van de externe load balancer worden opgenomen en moet ik dit eerst doen voordat de externe load balancer wordt verplaatst.


[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

### <a name="export-the-public-ip-template-and-deploy-from-azure-powershell"></a>De open bare IP-sjabloon exporteren en implementeren vanuit Azure PowerShell

1. Meld u aan bij uw Azure-abonnement met de opdracht [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) en volg de instructies op het scherm:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```
2. Haal de resource-ID op van het open bare IP-adres dat u wilt verplaatsen naar de doel regio en plaats deze in een variabele met [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourcePubIPID = (Get-AzPublicIPaddress -Name <source-public-ip-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Exporteer de open bare bron-IP naar een. JSON-bestand in de map waar u de opdracht [export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)uitvoert:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceVNETID -IncludeParameterDefaultValue
   ```

4. Het bestand dat u hebt gedownload krijgt de naam van de resource groep waaruit de resource is geëxporteerd.  Zoek het bestand dat is geëxporteerd uit de opdracht  **\<met de naam resource-group-name >. json** en open het in een editor naar keuze:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Als u de para meter van de naam van het open bare IP-adres wilt bewerken, wijzigt u de eigenschap **DefaultValue** van de bron-open bare IP-naam in de naam van het open bare IP-adres. Controleer of de naam tussen aanhalings tekens is:
    
    ```json
        {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "publicIPAddresses_myVM1pubIP_name": {
        "defaultValue": "<target-publicip-name>",
        "type": "String"
        }
    }

    ```

6. Als u de doel regio wilt bewerken waar het open bare IP-adres wordt verplaatst, wijzigt u de eigenschap **Location** onder resources:

    ```json
            "resources": [
            {
            "type": "Microsoft.Network/publicIPAddresses",
            "apiVersion": "2019-06-01",
            "name": "[parameters('publicIPAddresses_myPubIP_name')]",
            "location": "<target-region>",
            "sku": {
                "name": "Basic",
                "tier": "Regional"
            },
            "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
               }
               }
             ]             
    ```
  
7. Als u regio codes wilt ophalen, kunt u de Azure PowerShell cmdlet [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) gebruiken door de volgende opdracht uit te voeren:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
8. U kunt ook andere para meters in de sjabloon wijzigen als u ervoor kiest en zijn optioneel, afhankelijk van uw vereisten:

    * **SKU** : u kunt de SKU van het open bare IP-adres in de configuratie wijzigen van standaard in Basic of Basic naar Standard door de eigenschap **SKU** > **name** te wijzigen in het  **\<bestand resource-group-name >. json** :

         ```json
            "resources": [
                   {
                    "type": "Microsoft.Network/publicIPAddresses",
                    "apiVersion": "2019-06-01",
                    "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                    "location": "<target-region>",
                    "sku": {
                        "name": "Basic",
                        "tier": "Regional"
                    },
         ```

         Zie [een openbaar IP-adres maken, wijzigen of verwijderen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)voor meer informatie over de verschillen tussen open bare ip's van de Basic-en Standard-SKU.

    * **Toewijzings methode voor openbaar IP-adres** en **time-out voor inactiviteit** : u kunt beide opties in de sjabloon wijzigen door de eigenschap **publicIPAllocationMethod** van **dynamisch** naar **statisch** of **statisch** naar dynamisch te veranderen. U kunt de time-out voor inactiviteit wijzigen door de eigenschap **idleTimeoutInMinutes** te wijzigen in de gewenste hoeveelheid.  De standaard waarde is **4**:

         ```json
         "resources": [
                {
                "type": "Microsoft.Network/publicIPAddresses",
                "apiVersion": "2019-06-01",
                "name": "[parameters('publicIPAddresses_myPubIP_name')]",
                "location": "<target-region>",
                  "sku": {
                  "name": "Basic",
                  "tier": "Regional"
                 },
                "properties": {
                "provisioningState": "Succeeded",
                "resourceGuid": "7549a8f1-80c2-481a-a073-018f5b0b69be",
                "ipAddress": "52.177.6.204",
                "publicIPAddressVersion": "IPv4",
                "publicIPAllocationMethod": "Dynamic",
                "idleTimeoutInMinutes": 4,
                "ipTags": []
                   }
                }            
         ```

        Zie [een openbaar IP-adres maken, wijzigen of verwijderen](https://docs.microsoft.com/azure/virtual-network/virtual-network-public-ip-address)voor meer informatie over de toewijzings methoden en de time-outwaarden voor inactiviteit.


9. Sla het bestand  **\<met de resource-group-name >. json** op.

10. Maak een resource groep in de doel regio voor het open bare doel-IP-adres dat moet worden geïmplementeerd met [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0).
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Implementeer het bewerkte  **\<resource-group-name >. json-** bestand in de resource groep die u in de vorige stap hebt gemaakt met behulp van [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0):

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Als u wilt controleren of de resources zijn gemaakt in de doel regio, gebruikt u [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) en [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzPublicIPAddress -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```

### <a name="export-the-external-load-balancer-template-and-deploy-from-azure-powershell"></a>De externe load balancer sjabloon exporteren en implementeren vanuit Azure PowerShell

1. Meld u aan bij uw Azure-abonnement met de opdracht [Connect-AzAccount](https://docs.microsoft.com/powershell/module/az.accounts/connect-azaccount?view=azps-2.5.0) en volg de instructies op het scherm:
    
    ```azurepowershell-interactive
    Connect-AzAccount
    ```

2. Haal de bron-ID op van de externe load balancer die u wilt verplaatsen naar de doel regio en plaats deze in een variabele met [Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0):

    ```azurepowershell-interactive
    $sourceExtLBID = (Get-AzLoadBalancer -Name <source-external-lb-name> -ResourceGroupName <source-resource-group-name>).Id

    ```
3. Exporteer de bron externe load balancer configuratie naar een. JSON-bestand naar de map waarin u de opdracht [export-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/export-azresourcegroup?view=azps-2.6.0)uitvoert:
   
   ```azurepowershell-interactive
   Export-AzResourceGroup -ResourceGroupName <source-resource-group-name> -Resource $sourceExtLBID -IncludeParameterDefaultValue
   ```
4. Het bestand dat u hebt gedownload krijgt de naam van de resource groep waaruit de resource is geëxporteerd.  Zoek het bestand dat is geëxporteerd uit de opdracht  **\<met de naam resource-group-name >. json** en open het in een editor naar keuze:
   
   ```azurepowershell
   notepad.exe <source-resource-group-name>.json
   ```

5. Als u de para meter van de naam van de externe load balancer wilt bewerken, wijzigt u de eigenschap **DefaultValue** van de bron externe Load Balancer naam in de naam van de externe doel Load Balancer en controleert u of de naam tussen aanhalings tekens is:

    ```json
        "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
        "contentVersion": "1.0.0.0",
        "parameters": {
        "loadBalancers_myLoadbalancer_ext_name": {
        "defaultValue": "<target-external-lb-name>",
        "type": "String"
            },
        "publicIPAddresses_myPubIP_in_externalid": {
        "defaultValue": "<target-publicIP-resource-ID>",
        "type": "String"
            },

    ```

6.  Als u de waarde van de open bare doel-IP die hierboven is verplaatst, wilt bewerken, moet u eerst de resource-id ophalen en deze vervolgens kopiëren en plakken in het bestand met de naam van de  **\<resource groep >. json** .  Gebruik [Get-AzPublicIPAddress](https://docs.microsoft.com/powershell/module/az.network/get-azpublicipaddress?view=azps-2.6.0)om de id te verkrijgen:

    ```azurepowershell-interactive
    $targetPubIPID = (Get-AzPublicIPaddress -Name <target-public-ip-name> -ResourceGroupName <target-resource-group-name>).Id
    ```
    Typ de variabele en druk op ENTER om de resource-ID weer te geven.  Markeer het pad naar de ID en kopieer het naar het klem bord:

    ```powershell
    PS C:\> $targetPubIPID
    /subscriptions/7668d659-17fc-4ffd-85ba-9de61fe977e8/resourceGroups/myResourceGroupLB-Move/providers/Microsoft.Network/publicIPAddresses/myPubIP-in-move
    ```

7.  Plak in het  **\<bestand resource-group-name >. json** de **resource-id** uit de variabele in plaats van de **DefaultValue** in de tweede para meter voor de externe ID van het open bare IP-adres en zorg ervoor dat u het pad tussen aanhalings tekens plaatst:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
            "loadBalancers_myLoadbalancer_ext_name": {
            "defaultValue": "<target-external-lb-name>",
            "type": "String"
            },
            "publicIPAddresses_myPubIP_in_externalid": {
            "defaultValue": "<target-publicIP-resource-ID>",
            "type": "String"
            },

    ```

8.  Als u uitgaande NAT en regels voor uitgaande verbindingen voor de load balancer hebt geconfigureerd, wordt in dit bestand een derde vermelding weer gegeven voor de externe ID voor het uitgaande open bare IP-adres.  Herhaal de bovenstaande stappen in de **doel regio** om de id voor het uitgaande open bare IP-adres op te halen en plak die vermelding in de  **\<resource-group-name >. json-** bestand:

    ```json
            "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
            "contentVersion": "1.0.0.0",
            "parameters": {
                "loadBalancers_myLoadbalancer_ext_name": {
                "defaultValue": "<target-external-lb-name>",
                "type": "String"
            },
                "publicIPAddresses_myPubIP_in_externalid": {
                "defaultValue": "<target-publicIP-resource-ID>",
                "type": "String"
            },
                "publicIPAddresses_myPubIP_out_externalid": {
                "defaultValue": "<target-publicIP-outbound-resource-ID>",
                "type": "String"
            }
        },
    ```

10. Als u de doel regio wilt bewerken waar de configuratie van de externe Load Balancer wordt verplaatst, wijzigt u de eigenschap **Location** onder **resources** in het  **\<bestand resource-group-name >. json** :

    ```json
        "resources": [
            {
                "type": "Microsoft.Network/loadBalancers",
                "apiVersion": "2019-06-01",
                "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
                "location": "<target-external-lb-region>",
                "sku": {
                    "name": "Standard",
                    "tier": "Regional"
                },
    ```

11. Als u regio codes wilt ophalen, kunt u de Azure PowerShell cmdlet [Get-AzLocation](https://docs.microsoft.com/powershell/module/az.resources/get-azlocation?view=azps-1.8.0) gebruiken door de volgende opdracht uit te voeren:

    ```azurepowershell-interactive

    Get-AzLocation | format-table
    
    ```
12. U kunt ook andere para meters in de sjabloon wijzigen als u ervoor kiest en zijn optioneel, afhankelijk van uw vereisten:
    
    * **SKU** : u kunt de SKU van de externe Load Balancer in de configuratie wijzigen van standaard in Basic of Basic naar Standard door de eigenschap **SKU** > -**naam** te wijzigen in de **\<resource-group-name >. json**bestand:

        ```json
        "resources": [
        {
            "type": "Microsoft.Network/loadBalancers",
            "apiVersion": "2019-06-01",
            "name": "[parameters('loadBalancers_myLoadBalancer_name')]",
            "location": "<target-external-lb-region>",
            "sku": {
                "name": "Standard",
                "tier": "Regional"
            },
        ```
      Zie [overzicht van Azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) voor meer informatie over de verschillen tussen de Basic-en Standard SKU load balancers

    * Taakverdelings **regels** : u kunt taakverdelings regels toevoegen aan of verwijderen uit de configuratie door vermeldingen toe te voegen aan of te verwijderen uit de sectie **loadBalancingRules** van de  **\<resource-group-name >. json-** bestand:

        ```json
        "loadBalancingRules": [
                    {
                        "name": "myInboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 80,
                            "backendPort": 80,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false,
                            "loadDistribution": "Default",
                            "disableOutboundSnat": true,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolInbound')]"
                            },
                            "probe": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/probes/myHTTPProbe')]"
                            }
                        }
                    }
                ]
        ```
       Zie [Wat is Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) voor meer informatie over taakverdelings regels.

    * **Tests** : u kunt een test toevoegen aan of verwijderen uit de Load Balancer in de configuratie door vermeldingen toe te voegen aan of te verwijderen uit het gedeelte **tests** van het  **\<bestand resource-group-name >. json** :

        ```json
        "probes": [
                    {
                        "name": "myHTTPProbe",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "protocol": "Http",
                            "port": 80,
                            "requestPath": "/",
                            "intervalInSeconds": 15,
                            "numberOfProbes": 2
                        }
                    }
                ],
        ```
       Zie [Load Balancer Health probe](https://docs.microsoft.com/azure/load-balancer/load-balancer-custom-probe-overview) (Engelstalig) voor meer informatie over Azure Load Balancer status tests

    * **Binnenkomende NAT-regels** : u kunt binnenkomende NAT-regels voor de Load Balancer toevoegen of verwijderen door vermeldingen toe te voegen aan de sectie **inboundNatRules** van de  **\<resource-group-name >. json-** bestand:

        ```json
        "inboundNatRules": [
                    {
                        "name": "myInboundNATRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "frontendIPConfiguration": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                            },
                            "frontendPort": 4422,
                            "backendPort": 3389,
                            "enableFloatingIP": false,
                            "idleTimeoutInMinutes": 4,
                            "protocol": "Tcp",
                            "enableTcpReset": false
                        }
                    }
                ]
        ```
        Als u het toevoegen of verwijderen van een binnenkomende NAT-regel wilt volt ooien, moet de regel aanwezig zijn of worden verwijderd als een **type** -eigenschap aan het einde van de  **\<resource-group-name >. json-** bestand:

        ```json
        {
            "type": "Microsoft.Network/loadBalancers/inboundNatRules",
            "apiVersion": "2019-06-01",
            "name": "[concat(parameters('loadBalancers_myLoadBalancer_name'), '/myInboundNATRule')]",
            "dependsOn": [
                "[resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name'))]"
            ],
            "properties": {
                "provisioningState": "Succeeded",
                "frontendIPConfiguration": {
                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPinbound')]"
                },
                "frontendPort": 4422,
                "backendPort": 3389,
                "enableFloatingIP": false,
                "idleTimeoutInMinutes": 4,
                "protocol": "Tcp",
                "enableTcpReset": false
            }
        }
        ```
        Zie [Wat is Azure Load Balancer?](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview) voor meer informatie over binnenkomende NAT-regels.

    * **Regels voor uitgaande verbindingen** : u kunt in de configuratie uitgaande regels toevoegen of verwijderen door de eigenschap **outboundRules** in het  **\<bestand resource-group-name >. json** te bewerken:

        ```json
        "outboundRules": [
                    {
                        "name": "myOutboundRule",
                        "etag": "W/\"39e5e9cd-2d6d-491f-83cf-b37a259d86b6\"",
                        "properties": {
                            "provisioningState": "Succeeded",
                            "allocatedOutboundPorts": 10000,
                            "protocol": "All",
                            "enableTcpReset": false,
                            "idleTimeoutInMinutes": 15,
                            "backendAddressPool": {
                                "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/backendAddressPools/myBEPoolOutbound')]"
                            },
                            "frontendIPConfigurations": [
                                {
                                    "id": "[concat(resourceId('Microsoft.Network/loadBalancers', parameters('loadBalancers_myLoadBalancer_name')), '/frontendIPConfigurations/myfrontendIPoutbound')]"
                                }
                            ]
                        }
                    }
                ]
        ```

         Zie [Load Balancer regels voor uitgaande verbindingen](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview) voor meer informatie over uitgaande regels

13. Sla het bestand  **\<met de resource-group-name >. json** op.
    
10. Maak of een resource groep in de doel regio voor de externe doel load balancer die moet worden geïmplementeerd met [New-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroup?view=azps-2.6.0). De bestaande resource groep van boven kan ook opnieuw worden gebruikt als onderdeel van dit proces:
    
    ```azurepowershell-interactive
    New-AzResourceGroup -Name <target-resource-group-name> -location <target-region>
    ```
11. Implementeer het bewerkte  **\<resource-group-name >. json-** bestand in de resource groep die u in de vorige stap hebt gemaakt met behulp van [New-AzResourceGroupDeployment](https://docs.microsoft.com/powershell/module/az.resources/new-azresourcegroupdeployment?view=azps-2.6.0):

    ```azurepowershell-interactive

    New-AzResourceGroupDeployment -ResourceGroupName <target-resource-group-name> -TemplateFile <source-resource-group-name>.json
    
    ```

12. Als u wilt controleren of de resources zijn gemaakt in de doel regio, gebruikt u [Get-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/get-azresourcegroup?view=azps-2.6.0) en [Get-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/get-azloadbalancer?view=azps-2.6.0):
    
    ```azurepowershell-interactive

    Get-AzResourceGroup -Name <target-resource-group-name>

    ```

    ```azurepowershell-interactive

    Get-AzLoadBalancer -Name <target-publicip-name> -ResourceGroupName <target-resource-group-name>

    ```

## <a name="discard"></a>Verwijderen 

Als u na de implementatie wilt beginnen of het open bare IP-adres wilt negeren en load balancer in het doel wilt verwijderen, verwijdert u de resource groep die is gemaakt in het doel en het verplaatste open bare IP-adres en load balancer worden verwijderd.  Gebruik [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0)om de resource groep te verwijderen:

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

## <a name="clean-up"></a>Opruimen

Als u de wijzigingen wilt door voeren en de NSG wilt verplaatsen, verwijdert u de bron-NSG of de resource groep, gebruikt u [Remove-AzResourceGroup](https://docs.microsoft.com/powershell/module/az.resources/remove-azresourcegroup?view=azps-2.6.0) of [Remove-AzPublicIpAddress](https://docs.microsoft.com/powershell/module/az.network/remove-azpublicipaddress?view=azps-2.6.0) en [Remove-AzLoadBalancer](https://docs.microsoft.com/powershell/module/az.network/remove-azloadbalancer?view=azps-2.6.0)

```azurepowershell-interactive

Remove-AzResourceGroup -Name <resource-group-name>

```

``` azurepowershell-interactive

Remove-AzLoadBalancer -name <load-balancer> -ResourceGroupName <resource-group-name>

Remove-AzPublicIpAddress -Name <public-ip> -ResourceGroupName <resource-group-name>


```

## <a name="next-steps"></a>Volgende stappen

In deze zelf studie hebt u een Azure-netwerk beveiligings groep verplaatst van de ene regio naar een andere en de bron resources opgeschoond.  Raadpleeg voor meer informatie over het verplaatsen van resources tussen regio's en herstel na nood gevallen in Azure:


- [Resources verplaatsen naar een nieuwe resourcegroep of een nieuw abonnement](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-move-resources)
- [Virtuele Azure-machines verplaatsen naar een andere regio](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-tutorial-migrate)
