---
title: 'P2S-configuratie bestanden voor VPN-client maken & installeren: certificaat verificatie'
titleSuffix: Azure VPN Gateway
description: Windows-, Linux-, Linux-, strongSwan-en Mac OS X VPN-client configuratie bestanden maken en installeren voor P2S-certificaat verificatie.
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: article
ms.date: 09/12/2019
ms.author: cherylmc
ms.openlocfilehash: ad6e912f976095ae7d8fd5276b0f1365566c181a
ms.sourcegitcommit: 5cfe977783f02cd045023a1645ac42b8d82223bd
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/17/2019
ms.locfileid: "74143792"
---
# <a name="create-and-install-vpn-client-configuration-files-for-native-azure-certificate-authentication-p2s-configurations"></a>VPN-client configuratie bestanden maken en installeren voor systeem eigen Azure-certificaat verificatie P2S-configuraties

Configuratie bestanden voor VPN-clients zijn opgenomen in een zip-bestand. Configuratie bestanden bieden de instellingen die zijn vereist voor een systeem eigen Windows, Mac IKEv2-VPN of Linux-clients om verbinding te maken met een VNet via punt-naar-site-verbindingen die gebruikmaken van systeem eigen Azure-certificaat authenticatie.

Client configuratie bestanden zijn specifiek voor de VPN-configuratie voor het VNet. Als er wijzigingen zijn aangebracht in de punt-naar-site-VPN-configuratie nadat u de configuratie bestanden voor de VPN-client hebt gegenereerd, zoals het type VPN-protocol of het verificatie type, moet u nieuwe VPN-client configuratie bestanden voor uw gebruikers apparaten genereren. 

* Voor meer informatie over point-to-site-verbindingen leest u [About Point-to-Site VPN](point-to-site-about.md) (Over point-to-site-VPN).
* Zie [Configure openvpn for P2S](vpn-gateway-howto-openvpn.md) (Engelstalig) en [Configure openvpn clients](vpn-gateway-howto-openvpn-clients.md)(Engelstalig) voor openvpn instructies.

>[!IMPORTANT]
>[!INCLUDE [TLS](../../includes/vpn-gateway-tls-change.md)]
>

## <a name="generate"></a>Configuratiebestanden voor VPN-clients genereren

Voordat u begint, moet u ervoor zorgen dat alle gebruikers met een verbinding een geldig certificaat hebben geïnstalleerd op het apparaat van de gebruiker. Zie [een client certificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md)voor meer informatie over het installeren van een client certificaat.

U kunt client configuratie bestanden genereren met behulp van Power shell of met behulp van de Azure Portal. Beide methoden retour neren hetzelfde zip-bestand. Pak het bestand uit om de volgende mappen weer te geven:

  * **WindowsAmd64** en **WindowsX86**, die respectievelijk de Windows 32-bits en 64-bits installatie pakketten bevatten. Het **WindowsAmd64** Installer-pakket is voor alle ondersteunde 64-bits Windows-clients, niet alleen AMD.
  * **Algemeen**, dat algemene informatie bevat die wordt gebruikt voor het maken van uw eigen VPN-client configuratie. De algemene map wordt opgegeven als IKEv2 of SSTP + IKEv2 is geconfigureerd op de gateway. Als alleen SSTP is geconfigureerd, is de generieke map niet aanwezig.

### <a name="zipportal"></a>Bestanden genereren met behulp van de Azure Portal

1. Navigeer in het Azure Portal naar de virtuele netwerk gateway voor het virtuele netwerk waarmee u verbinding wilt maken.
2. Klik op de pagina virtuele netwerk gateway op **punt-naar-site-configuratie**.
3. Klik boven aan de pagina punt-naar-site-configuratie op **VPN-client downloaden**. Het duurt enkele minuten voordat het client configuratie pakket is gegenereerd.
4. Uw browser geeft aan dat een zip-bestand voor de client configuratie beschikbaar is. Deze heeft dezelfde naam als uw gateway. Pak het bestand uit om de mappen weer te geven.

### <a name="zipps"></a>Bestanden genereren met behulp van Power shell

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

1. Bij het genereren van configuratie bestanden voor VPN-clients is de waarde '-EapTls '. Genereer de configuratie bestanden voor de VPN-client met de volgende opdracht:

   ```azurepowershell-interactive
   $profile=New-AzVpnClientConfiguration -ResourceGroupName "TestRG" -Name "VNet1GW" -AuthenticationMethod "EapTls"

   $profile.VPNProfileSASUrl
   ```
2. Kopieer de URL naar uw browser om het zip-bestand te downloaden en pak het bestand uit om de mappen weer te geven.

## <a name="installwin"></a>Windows

U kunt hetzelfde configuratie pakket voor de VPN-client gebruiken op elke Windows-client computer, mits de versie overeenkomt met de architectuur van de client. Zie de sectie punt-naar-site van de [Veelgestelde vragen over VPN gateway](vpn-gateway-vpn-faq.md#P2S)voor de lijst met ondersteunde client besturingssystemen.

>[!NOTE]
>U moet over beheerders rechten beschikken voor de Windows-client computer waarmee u verbinding wilt maken.
>
>

Gebruik de volgende stappen om de systeem eigen Windows VPN-client te configureren voor verificatie van certificaten:

1. Selecteer de VPN-clientconfiguratiebestanden die overeenkomen met de architectuur van de Windows-computer. Kies voor een architectuur met 64-bits processor het installatiepakket ‘VpnClientSetupAmd64’. Kies voor een architectuur met 32-bits processor het installatiepakket ‘VpnClientSetupAmdX86’. 
2. Dubbelklik op het pakket om het te installeren. Als u een SmartScreen-melding ziet, klikt u op **Meer info** en vervolgens op **Toch uitvoeren**.
3. Navigeer op de clientcomputer naar **Netwerkinstellingen** en klik op **VPN**. De VPN-verbinding bevat de naam van het virtuele netwerk waarmee verbinding wordt gemaakt. 
4. Controleer of u een clientcertificaat op de clientcomputer hebt geïnstalleerd voordat u verbinding probeert te maken. Een clientcertificaat is vereist voor verificatie als u het systeemeigen verificatietype met Azure-certificaat gebruikt. Zie [certificaten genereren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert)voor meer informatie over het genereren van certificaten. Zie [een client certificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md)voor meer informatie over het installeren van een client certificaat.

## <a name="installmac"></a>Mac (OS X)

 U moet de systeemeigen IKEv2 VPN-client handmatig configureren op elke Mac die verbinding met Azure zal maken. Azure biedt geen mobileconfig-bestand voor verificatie van systeemeigen Azure-certificaten. De **algemene** bevat alle informatie die u nodig hebt voor configuratie. Als u de map Generic niet ziet in uw download, is IKEv2 waarschijnlijk niet geselecteerd als tunneltype. Houd er rekening mee dat de basis-SKU VPN gateway geen ondersteuning biedt voor IKEv2. Genereer het zip-bestand opnieuw om de map Generic te verkrijgen nadat IKEv2 is geselecteerd.<br>De map Generic bevat de volgende bestanden:

* **VpnSettings. XML**, dat belang rijke instellingen bevat, zoals het server adres en het tunnel type. 
* **VpnServerRoot. CER**, dat het basis certificaat bevat dat is vereist voor het valideren van de Azure VPN gateway tijdens de configuratie van de P2S-verbinding.

Voer de volgende stappen uit om de systeem eigen VPN-client te configureren voor verificatie via een certificaat. U moet deze stappen uitvoeren op elke Mac die verbinding maakt met Azure:

1. Importeer het basis certificaat van de **VpnServerRoot** naar uw Mac. U kunt dit doen door het bestand te kopiëren naar uw Mac en erop te dubbel klikken. Klik op **toevoegen** om te importeren.

   ![certificaat toevoegen](./media/point-to-site-vpn-client-configuration-azure-cert/addcert.png)
  
    >[!NOTE]
    >Als u dubbelklikt op het certificaat, wordt het dialoog venster **toevoegen** niet weer gegeven, maar wordt het certificaat in de juiste opslag geïnstalleerd. U kunt het certificaat controleren in de sleutel voor aanmelding bij de categorie certificaten.
    >
  
2. Controleer of u een client certificaat hebt geïnstalleerd dat is uitgegeven door het basis certificaat dat u hebt geüpload naar Azure wanneer u P2S-instellingen hebt geconfigureerd. Dit wijkt af van de VPNServerRoot die u in de vorige stap hebt geïnstalleerd. Het client certificaat wordt gebruikt voor verificatie en is vereist. Zie [certificaten genereren](vpn-gateway-howto-point-to-site-resource-manager-portal.md#generatecert)voor meer informatie over het genereren van certificaten. Zie [een client certificaat installeren](point-to-site-how-to-vpn-client-install-azure-cert.md)voor meer informatie over het installeren van een client certificaat.
3. Open het dialoog venster **netwerk** onder **netwerk voorkeuren** en klik op **+** om een nieuw VPN-client VERBINDINGS profiel te maken voor een P2S-verbinding met Azure VNet.

   De **Interface** waarde is ' VPN ' en de waarde van het **VPN-type** is ' IKEv2 '. Geef een naam op voor het profiel in het veld **service naam** en klik vervolgens op **maken** om het VPN-client verbindings profiel te maken.

   ![netwerk](./media/point-to-site-vpn-client-configuration-azure-cert/network.png)
4. Kopieer in de map **Algemeen** , in het bestand **VpnSettings. XML** , de waarde van de **VpnServer** -tag. Plak deze waarde in de velden **server adres** en **externe ID** van het profiel.

   ![Server gegevens](./media/point-to-site-vpn-client-configuration-azure-cert/server.png)
5. Klik op **verificatie-instellingen** en selecteer **certificaat**. 

   ![verificatie-instellingen](./media/point-to-site-vpn-client-configuration-azure-cert/authsettings.png)
6. Klik op **selecteren...** om het client certificaat te kiezen dat u voor verificatie wilt gebruiken. Dit is het certificaat dat u in stap 2 hebt geïnstalleerd.

   ![certificaat](./media/point-to-site-vpn-client-configuration-azure-cert/certificate.png)
7. **Kies een identiteit** om een lijst met certificaten weer te geven waaruit u kunt kiezen. Selecteer het juiste certificaat en klik vervolgens op **door gaan**.

   ![identity](./media/point-to-site-vpn-client-configuration-azure-cert/identity.png)
8. Geef in het veld **lokale id** de naam van het certificaat op (uit stap 6). In dit voor beeld is het ' ikev2Client.com '. Klik vervolgens op de knop **Toep assen** om de wijzigingen op te slaan.

   ![Toep assen](./media/point-to-site-vpn-client-configuration-azure-cert/applyconnect.png)
9. Klik in het dialoog venster **netwerk** op **Toep assen** om alle wijzigingen op te slaan. Klik vervolgens op **verbinding maken** om de P2S-verbinding met Azure VNet te starten.

## <a name="linuxgui"></a>Linux (strongSwan GUI)

### <a name="installstrongswan"></a>StrongSwan installeren

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="genlinuxcerts"></a>Certificaten genereren

Als u nog geen certificaten hebt gegenereerd, gebruikt u de volgende stappen:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install"></a>Installeren en configureren

De volgende instructies zijn gemaakt op Ubuntu 18.0.4. Ubuntu 16.0.10 biedt geen ondersteuning voor strongSwan-GUI. Als u Ubuntu 16.0.10 wilt gebruiken, moet u de [opdracht regel](#linuxinstallcli)gebruiken. De onderstaande voor beelden komen mogelijk niet overeen met de schermen die u ziet, afhankelijk van uw versie van Linux en strongSwan.

1. Open de **Terminal** om **strongswan** en de bijbehorende netwerk beheerder te installeren door de opdracht in het voor beeld uit te voeren.

   ```
   sudo apt install network-manager-strongswan
   ```
2. Selecteer **instellingen** en selecteer vervolgens **netwerk**.

   ![verbindingen bewerken](./media/point-to-site-vpn-client-configuration-azure-cert/editconnections.png)
3. Klik op de knop **+** om een nieuwe verbinding te maken.

   ![een verbinding toevoegen](./media/point-to-site-vpn-client-configuration-azure-cert/addconnection.png)
4. Selecteer **IPSec/IKEv2 (strongswan)** in het menu en dubbel klik op. In deze stap kunt u de naam van uw verbinding wijzigen.

   ![Kies een verbindings type](./media/point-to-site-vpn-client-configuration-azure-cert/choosetype.png)
5. Open het **VpnSettings. XML-** bestand in de **algemene** map die zich in de gedownloade client configuratie bestanden bevindt. Zoek het label **VpnServer** en kopieer de naam, te beginnen met ' azuregateway ' en eindigt met '. cloudapp.net '.

   ![naam kopiëren](./media/point-to-site-vpn-client-configuration-azure-cert/vpnserver.png)
6. Plak deze naam in het **adres** veld van uw nieuwe VPN-verbinding in de sectie **Gateway** . Selecteer vervolgens het mappictogram aan het einde van het veld **certificaat** , blader naar de **algemene** map en selecteer het **VpnServerRoot** -bestand.
7. Selecteer in het gedeelte **client** van de verbinding voor **verificatie**de optie **certificaat/persoonlijke sleutel**. Voor het **certificaat** en de **persoonlijke sleutel**kiest u het certificaat en de persoonlijke sleutel die u eerder hebt gemaakt. Selecteer bij **Opties** **een intern IP-adres aanvragen**. Klik vervolgens op **Toevoegen**.

   ![een intern IP-adres aanvragen](./media/point-to-site-vpn-client-configuration-azure-cert/turnon.png)
8. Zet de verbinding **aan**.

## <a name="linuxinstallcli"></a>Linux (strongSwan CLI)

### <a name="install-strongswan"></a>StrongSwan installeren

[!INCLUDE [install strongSwan](../../includes/vpn-gateway-strongswan-install-include.md)]

### <a name="generate-certificates"></a>Certificaten genereren

Als u nog geen certificaten hebt gegenereerd, gebruikt u de volgende stappen:

[!INCLUDE [strongSwan certificates](../../includes/vpn-gateway-strongswan-certificates-include.md)]

### <a name="install-and-configure"></a>Installeren en configureren

1. Down load het VPNClient-pakket van Azure Portal.
2. Pak het bestand uit.
3. Kopieer of verplaats de VpnServerRoot. CER uit de **algemene** map naar/etc/IPSec.d/cacerts.
4. Kopieer of verplaats CP-client. p12 naar/etc/IPSec.d/private/. Dit bestand is een client certificaat voor Azure VPN Gateway.
5. Open het bestand VpnSettings. XML en kopieer de `<VpnServer>` waarde. In de volgende stap gaat u deze waarde gebruiken.
6. Wijzig de waarden in het onderstaande voor beeld en voeg het voor beeld toe aan de/etc/IPSec.conf-configuratie.
  
   ```
   conn azure
         keyexchange=ikev2
         type=tunnel
         leftfirewall=yes
         left=%any
         leftauth=eap-tls
         leftid=%client # use the DNS alternative name prefixed with the %
         right= Enter the VPN Server value here# Azure VPN gateway address
         rightid=% # Enter the VPN Server value here# Azure VPN gateway FQDN with %
         rightsubnet=0.0.0.0/0
         leftsourceip=%config
         auto=add
   ```
6. Voeg het volgende toe aan */etc/IPSec.Secrets*.

   ```
   : P12 client.p12 'password' # key filename inside /etc/ipsec.d/private directory
   ```

7. Voer de volgende opdrachten uit:

   ```
   # ipsec restart
   # ipsec up azure
   ```

## <a name="next-steps"></a>Volgende stappen

Ga terug naar het artikel om [uw P2S-configuratie te volt ooien](vpn-gateway-howto-point-to-site-rm-ps.md).

Raadpleeg de volgende artikelen om problemen met P2S-verbindingen op te lossen:

  * [Problemen met Azure Point-to-site-verbindingen oplossen](vpn-gateway-troubleshoot-vpn-point-to-site-connection-problems.md)
  * [Problemen met VPN-verbindingen van Mac OS X VPN-clients oplossen](vpn-gateway-troubleshoot-point-to-site-osx-ikev2.md)
