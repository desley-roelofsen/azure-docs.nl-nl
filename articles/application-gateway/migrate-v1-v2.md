---
title: Migreren van v1 naar v2-Azure-toepassing gateway
description: Dit artikel laat u zien hoe u Azure-toepassing gateway en Web Application firewall van v1 naar v2 kunt migreren
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 11/14/2019
ms.author: victorh
ms.openlocfilehash: 719686cb123355359391c5cb1e517ff9cfd88371
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74231731"
---
# <a name="migrate-azure-application-gateway-and-web-application-firewall-from-v1-to-v2"></a>Azure-toepassing gateway en Web Application firewall migreren van v1 naar v2

[Azure-toepassing gateway en Web Application firewall (WAF) v2](application-gateway-autoscaling-zone-redundant.md) is nu beschikbaar en biedt aanvullende functies, zoals automatisch schalen en beschik baarheid van zone redundantie. Bestaande V1-gateways worden echter niet automatisch bijgewerkt naar V2. Als u van v1 naar v2 wilt migreren, volgt u de stappen in dit artikel.

Er zijn twee fasen in een migratie:

1. De configuratie migreren
2. Het client verkeer migreren

In dit artikel wordt beschreven hoe u de configuratie migreert. Migratie van client verkeer varieert afhankelijk van uw specifieke omgeving. Er zijn echter algemene aanbevelingen [van](#migrate-client-traffic)een hoog niveau.

## <a name="migration-overview"></a>Overzicht migratie

Er is een Azure PowerShell script beschikbaar dat het volgende doet:

* Hiermee maakt u een nieuwe Standard_v2 of WAF_v2 gateway in een subnet van een virtueel netwerk dat u opgeeft.
* Hiermee wordt de configuratie die is gekoppeld aan de V1 Standard-of WAF-gateway, naadloos gekopieerd naar de zojuist gemaakte Standard_V2 of WAF_V2 gateway.

### <a name="caveatslimitations"></a>Caveats\Limitations

* De nieuwe v2-gateway heeft nieuwe open bare en privé IP-adressen. Het is niet mogelijk om de IP-adressen die zijn gekoppeld aan de bestaande v1-gateway naadloos te verplaatsen naar v2. U kunt echter een bestaand (niet-toegewezen) openbaar of privé IP-adres toewijzen aan de nieuwe v2-gateway.
* U moet een IP-adres ruimte opgeven voor een ander subnet in het virtuele netwerk waarin uw v1-gateway zich bevindt. Het script kan de v2-gateway niet maken in bestaande subnetten die al een v1-gateway hebben. Als het bestaande subnet al een v2-gateway heeft, kan dit echter nog steeds werken omdat er voldoende IP-adres ruimte beschikbaar is.
* Als u een SSL-configuratie wilt migreren, moet u alle SSL-certificaten opgeven die worden gebruikt in uw v1-gateway.
* Als de FIPS-modus is ingeschakeld voor uw v1-gateway, wordt deze niet gemigreerd naar de nieuwe v2-gateway. De FIPS-modus wordt niet ondersteund in v2.
* v2 biedt geen ondersteuning voor IPv6, zodat de voor IPv6 ingeschakelde v1-gateways niet worden gemigreerd. Als u het script uitvoert, wordt het mogelijk niet voltooid.
* Als de V1-gateway alleen een persoonlijk IP-adres heeft, maakt het script een openbaar IP-adres en een privé-IP-adres voor de nieuwe v2-gateway. v2-gateways ondersteunen momenteel alleen particuliere IP-adressen.

## <a name="download-the-script"></a>Het script downloaden

Down load het migratie script van de [PowerShell Gallery](https://www.powershellgallery.com/packages/AzureAppGWMigration).

## <a name="use-the-script"></a>Het script gebruiken

Er zijn twee opties voor u afhankelijk van de instellingen en voor keuren van uw lokale Power shell-omgeving:

* Als u de Azure AZ-modules niet hebt geïnstalleerd of als u niet van mening bent dat u de Azure AZ-modules verwijdert, is de beste optie om het script uit te voeren met de optie `Install-Script`.
* Als u de Azure AZ-modules wilt blijven gebruiken, is het verstandig om het script te downloaden en het rechtstreeks uit te voeren.

Als u wilt weten of u de Azure AZ-modules hebt geïnstalleerd, voert u `Get-InstalledModule -Name az`uit. Als er geen geïnstalleerde AZ-modules worden weer geven, kunt u de `Install-Script`-methode gebruiken.

### <a name="install-using-the-install-script-method"></a>Installeren met behulp van de methode install-script

Als u deze optie wilt gebruiken, moet u de Azure AZ-modules niet op uw computer installeren. Als ze zijn geïnstalleerd, wordt een fout weer gegeven in de volgende opdracht. U kunt de Azure AZ-modules verwijderen of de andere optie gebruiken om het script hand matig te downloaden en uit te voeren.
  
Voer het script uit met de volgende opdracht:

`Install-Script -Name AzureAppGWMigration`

Met deze opdracht worden ook de vereiste AZ-modules geïnstalleerd.  

### <a name="install-using-the-script-directly"></a>Rechtstreeks installeren met behulp van het script

Als er sommige Azure AZ-modules zijn geïnstalleerd en deze niet kunnen verwijderen (of niet wilt verwijderen), kunt u het script hand matig downloaden met het tabblad **hand matig downloaden** in de koppeling voor het downloaden van het script. Het script wordt gedownload als een onbewerkt nupkg-bestand. Zie [hand matig downloaden van pakketten](/powershell/scripting/gallery/how-to/working-with-packages/manual-download)als u het script wilt installeren vanuit dit nupkg-bestand.

Het script uitvoeren:

1. Gebruik `Connect-AzAccount` om verbinding te maken met Azure.

1. Gebruik `Import-Module Az` om de AZ-modules te importeren.

1. Voer `Get-Help AzureAppGWMigration.ps1` uit om de vereiste para meters te controleren:

   ```
   AzureAppGwMigration.ps1
    -resourceId <v1 application gateway Resource ID>
    -subnetAddressRange <subnet space you want to use>
    -appgwName <string to use to append>
    -sslCertificates <comma-separated SSLCert objects as above>
    -trustedRootCertificates <comma-separated Trusted Root Cert objects as above>
    -privateIpAddress <private IP string>
    -publicIpResourceId <public IP name string>
    -validateMigration -enableAutoScale
   ```

   Para meters voor het script:
   * **resourceId: [teken reeks]: vereist** -dit is de Azure-resource-id voor uw bestaande Standard v1-of WAF v1-gateway. Als u deze teken reeks waarde wilt vinden, gaat u naar de Azure Portal, selecteert u de toepassings gateway of de WAF-resource en klikt u op de koppeling **Eigenschappen** voor de gateway. De resource-ID bevindt zich op deze pagina.

     U kunt ook de volgende Azure PowerShell opdrachten uitvoeren om de resource-ID op te halen:

     ```azurepowershell
     $appgw = Get-AzApplicationGateway -Name <v1 gateway name> -ResourceGroupName <resource group Name> 
     $appgw.Id
     ```

   * **subnetAddressRange: [teken reeks]: vereist** -dit is de IP-adres ruimte die u hebt toegewezen (of die u wilt toewijzen) voor een nieuw subnet dat uw nieuwe v2-gateway bevat. Dit moet worden opgegeven in de CIDR-notatie. Bijvoorbeeld: 10.0.0.0/24. U hoeft dit subnet niet vooraf te maken. Het script maakt het voor u als het niet bestaat.
   * **appgwName: [teken reeks]: optioneel**. Dit is een teken reeks die u opgeeft om te gebruiken als de naam voor de nieuwe Standard_v2 of WAF_v2 gateway. Als deze para meter niet wordt opgegeven, wordt de naam van uw bestaande v1-gateway gebruikt met het achtervoegsel *_v2* toegevoegd.
   * **sslCertificates: [PSApplicationGatewaySslCertificate]: optioneel**.  Een door komma's gescheiden lijst met PSApplicationGatewaySslCertificate-objecten die u maakt om de SSL-certificaten van uw v1-gateway te vertegenwoordigen, moet worden geüpload naar de nieuwe v2-gateway. Voor elk van de SSL-certificaten die zijn geconfigureerd voor uw Standard v1-of WAF v1-gateway, kunt u een nieuw PSApplicationGatewaySslCertificate-object maken via de `New-AzApplicationGatewaySslCertificate`-opdracht die hier wordt weer gegeven. U hebt het pad naar uw SSL-certificaat bestand en het wacht woord nodig.

       Deze para meter is alleen optioneel als u geen HTTPS-listeners hebt geconfigureerd voor uw v1-gateway of WAF. Als u ten minste één HTTPS-listener-installatie hebt, moet u deze para meter opgeven.

      ```azurepowershell  
      $password = ConvertTo-SecureString <cert-password> -AsPlainText -Force
      $mySslCert1 = New-AzApplicationGatewaySslCertificate -Name "Cert01" `
        -CertificateFile <Cert-File-Path-1> `
        -Password $password 
      $mySslCert2 = New-AzApplicationGatewaySslCertificate -Name "Cert02" `
        -CertificateFile <Cert-File-Path-2> `
        -Password $password
      ```

      U kunt in het vorige voor beeld door geven `$mySslCert1, $mySslCert2` (gescheiden door komma's) als waarden voor deze para meter in het script.
   * **trustedRootCertificates: [PSApplicationGatewayTrustedRootCertificate]: optioneel**. Een door komma's gescheiden lijst met PSApplicationGatewayTrustedRootCertificate-objecten die u maakt om de [vertrouwde basis certificaten](ssl-overview.md) te vertegenwoordigen voor verificatie van uw back-end-instanties vanuit uw v2-gateway.  

      Zie [New-AzApplicationGatewayTrustedRootCertificate](https://docs.microsoft.com/powershell/module/Az.Network/New-AzApplicationGatewayTrustedRootCertificate?view=azps-2.1.0&viewFallbackFrom=azps-2.0.0)als u een lijst met PSApplicationGatewayTrustedRootCertificate-objecten wilt maken.
   * **privateIpAddress: [teken reeks]: optioneel**. Een specifiek privé-IP-adres dat u wilt koppelen aan uw nieuwe v2-gateway.  Dit moet afkomstig zijn uit hetzelfde VNet dat u toewijst voor uw nieuwe v2-gateway. Als dit niet is opgegeven, wordt door het script een persoonlijk IP-adres voor uw v2-gateway toegewezen.
    * **publicIpResourceId: [teken reeks]: optioneel**. De resourceId van een open bare IP-adres resource (standaard-SKU) in uw abonnement dat u wilt toewijzen aan de nieuwe v2-gateway. Als dit niet is opgegeven, wijst het script een nieuw openbaar IP-adres toe aan dezelfde resource groep. De naam is de naam van de v2-gateway met *-IP* toegevoegd.
   * **validateMigration: [Switch]: optioneel**. Gebruik deze para meter als u wilt dat het script enkele validaties op basis van de configuratie van de 2-en de configuratie kopie van de v2-gateway. Standaard wordt er geen validatie uitgevoerd.
   * **enableAutoScale: [Switch]: optioneel**. Gebruik deze para meter als u wilt dat het script automatisch schalen inschakelt op de nieuwe v2-gateway nadat deze is gemaakt. Automatisch schalen is standaard uitgeschakeld. U kunt dit later altijd hand matig inschakelen op de zojuist gemaakte v2-gateway.

1. Voer het script uit met de juiste para meters. Het kan vijf tot zeven minuten duren voordat de bewerking is voltooid.

    **Voorbeeld**

   ```azurepowershell
   AzureAppGWMigration.ps1 `
      -resourceId /subscriptions/8b1d0fea-8d57-4975-adfb-308f1f4d12aa/resourceGroups/MyResourceGroup/providers/Microsoft.Network/applicationGateways/myv1appgateway `
      -subnetAddressRange 10.0.0.0/24 `
      -appgwname "MynewV2gw" `
      -sslCertificates $Certs `
      -trustedRootCertificates $trustedCert `
      -privateIpAddress "10.0.0.1" `
      -publicIpResourceId "MyPublicIP" `
      -validateMigration -enableAutoScale
   ```

## <a name="migrate-client-traffic"></a>Client verkeer migreren

Controleer eerst of het script een nieuwe v2-gateway heeft gemaakt met de exacte configuratie die is gemigreerd van uw v1-gateway. U kunt dit controleren vanuit het Azure Portal.

Verzend ook een kleine hoeveelheid verkeer via de v2-gateway als hand matige test.
  
Hier volgen enkele scenario's waarin uw huidige toepassings gateway (standaard) client verkeer kan ontvangen en onze aanbevelingen voor elk van deze opties:

* **Een aangepaste DNS-zone (bijvoorbeeld contoso.com) die verwijst naar het front-end-IP-adres (met behulp van een A-record) dat is gekoppeld aan uw Standard v1-of WAF v1-gateway**.

    U kunt uw DNS-record bijwerken zodat deze verwijst naar het frontend-IP-of DNS-label dat is gekoppeld aan uw Standard_v2 Application Gateway. Afhankelijk van de TTL die op uw DNS-record is geconfigureerd, kan het enige tijd duren voordat al uw client verkeer naar uw nieuwe v2-gateway wordt gemigreerd.
* **Een aangepaste DNS-zone (bijvoorbeeld contoso.com) die verwijst naar het DNS-label (bijvoorbeeld: *myappgw.eastus.cloudapp.Azure.com* met een CNAME-record) die is gekoppeld aan uw v1-gateway**.

   U hebt twee opties:

  * Als u open bare IP-adressen in uw toepassings gateway gebruikt, kunt u een bewaakte, gedetailleerde migratie uitvoeren met behulp van een Traffic Manager profiel om het verkeer (gewogen verkeers routerings methode) incrementeel door te sturen naar de nieuwe v2-gateway.

    U kunt dit doen door de DNS-labels van zowel de v1-als v2-toepassings gateways toe te voegen aan het [Traffic Manager-profiel](../traffic-manager/traffic-manager-routing-methods.md#weighted-traffic-routing-method)en uw aangepaste DNS-record (bijvoorbeeld `www.contoso.com`) te CNAME aan het Traffic Manager domein (bijvoorbeeld contoso.trafficmanager.net).
  * U kunt ook uw aangepaste DNS-domein record bijwerken zodat deze verwijst naar het DNS-label van de nieuwe v2-toepassings gateway. Afhankelijk van de TTL die op uw DNS-record is geconfigureerd, kan het enige tijd duren voordat al uw client verkeer naar uw nieuwe v2-gateway wordt gemigreerd.
* **Uw clients maken verbinding met het frontend-IP-adres van uw toepassings gateway**.

   Werk uw clients bij voor het gebruik van de IP-adressen die zijn gekoppeld aan de zojuist gemaakte v2-toepassings gateway. U wordt aangeraden geen IP-adressen rechtstreeks te gebruiken. Overweeg het gebruik van het DNS-naam label (bijvoorbeeld yourgateway.eastus.cloudapp.azure.com) dat is gekoppeld aan uw toepassings gateway en die u kunt CNAME koppelen aan uw eigen aangepaste DNS-zone (bijvoorbeeld contoso.com).

## <a name="common-questions"></a>Veelgestelde vragen

### <a name="are-there-any-limitations-with-the-azure-powershell-script-to-migrate-the-configuration-from-v1-to-v2"></a>Zijn er beperkingen met het Azure PowerShell script voor het migreren van de configuratie van v1 naar v2?

Ja. Zie [aanvullende informatie/beperkingen](#caveatslimitations).

### <a name="is-this-article-and-the-azure-powershell-script-applicable-for-application-gateway-waf-product-as-well"></a>Is dit artikel en het Azure PowerShell script ook van toepassing op Application Gateway WAF-product? 

Ja.

### <a name="does-the-azure-powershell-script-also-switch-over-the-traffic-from-my-v1-gateway-to-the-newly-created-v2-gateway"></a>Schakelt het Azure PowerShell script ook over op het verkeer van mijn v1-gateway naar de zojuist gemaakte v2-gateway?

Nee. Met het Azure PowerShell script wordt de configuratie alleen gemigreerd. De werkelijke verkeers migratie is uw verantwoordelijkheid en in uw besturings element.

### <a name="is-the-new-v2-gateway-created-by-the-azure-powershell-script-sized-appropriately-to-handle-all-of-the-traffic-that-is-currently-served-by-my-v1-gateway"></a>Is de nieuwe v2-gateway gemaakt door het Azure PowerShell script op de juiste manier aangepast om alle verkeer te verwerken dat momenteel wordt bediend door mijn v1-gateway?

Met het Azure PowerShell script maakt u een nieuwe v2-gateway met een juiste grootte om het verkeer op uw bestaande v1-gateway af te handelen. Automatisch schalen is standaard uitgeschakeld, maar u kunt automatisch schalen inschakelen wanneer u het script uitvoert.

### <a name="i-configured-my-v1-gateway--to-send-logs-to-azure-storage-does-the-script-replicate-this-configuration-for-v2-as-well"></a>Ik heb mijn v1-gateway geconfigureerd voor het verzenden van logboeken naar Azure Storage. Repliceert het script deze configuratie ook voor v2?

Nee. Deze configuratie wordt niet gerepliceerd door het script voor v2. U moet de logboek configuratie afzonderlijk toevoegen aan de gemigreerde v2-gateway.

### <a name="does-this-script-support-certificates-uploaded-to-azure-keyvault-"></a>Ondersteunt dit script certificaten die zijn geüpload naar Azure-sleutel kluis?

Nee. Het script biedt momenteel geen ondersteuning voor certificaten in de sleutel kluis. Dit wordt echter wel overwogen voor een toekomstige versie.

### <a name="i-ran-into-some-issues-with-using-this-script-how-can-i-get-help"></a>Er zijn problemen opgetreden bij het gebruik van dit script. Hoe kan ik hulp krijgen?
  
U kunt een e-mail verzenden naar appgwmigrationsup@microsoft.com, een ondersteunings aanvraag openen met ondersteuning voor Azure of beide.

## <a name="next-steps"></a>Volgende stappen

[Meer informatie over Application Gateway v2](application-gateway-autoscaling-zone-redundant.md)
