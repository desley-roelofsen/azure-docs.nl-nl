---
title: Problemen met de verbinding met Azure voor herstel na nood gevallen met Azure Site Recovery oplossen
description: Fouten en problemen oplossen bij het repliceren van virtuele Azure-machines voor herstel na nood gevallen
services: site-recovery
author: asgang
manager: rochakm
ms.service: site-recovery
ms.topic: article
ms.date: 08/05/2019
ms.author: asgang
ms.openlocfilehash: 4995a4086c471a06fe859febfd2d1af7fbb22a76
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2019
ms.locfileid: "73622455"
---
# <a name="troubleshoot-azure-to-azure-vm-network-connectivity-issues"></a>Problemen met Azure-naar-Azure-VM-netwerk connectiviteit oplossen

In dit artikel worden veelvoorkomende problemen met de netwerk verbinding beschreven wanneer u virtuele Azure-machines van de ene regio naar een andere regio repliceert en herstelt. Zie de [connectiviteits vereisten voor het repliceren van virtuele Azure-machines](azure-to-azure-about-networking.md)voor meer informatie over netwerk vereisten.

Voor een goede werking van Site Recovery replicatie is uitgaande verbinding met specifieke Url's of IP-adresbereiken vereist van de virtuele machine. Als uw virtuele machine zich achter een firewall bevindt of gebruikmaakt van regels voor netwerk beveiligings groepen (NSG) om de uitgaande connectiviteit te beheren, kunt u een van deze problemen tegen komen.

**URL** | **Details**  
--- | ---
*.blob.core.windows.net | Vereist zodat gegevens kunnen worden geschreven naar het cache-opslag account in de bron regio van de virtuele machine. Als u alle cache opslag accounts voor uw Vm's weet, kunt u de specifieke Url's van het opslag account (bijvoorbeeld cache1.blob.core.windows.net en cache2.blob.core.windows.net) in plaats van *. blob.core.windows.net weer geven.
login.microsoftonline.com | Vereist voor autorisatie en verificatie voor de Url's van de Site Recovery-service.
*.hypervrecoverymanager.windowsazure.com | Vereist zodat de Site Recovery service communicatie kan worden uitgevoerd vanaf de virtuele machine. U kunt de bijbehorende ' Site Recovery IP ' gebruiken als uw firewall proxy Ip's ondersteunt.
*.servicebus.windows.net | Vereist zodat de Site Recovery bewakings-en diagnostische gegevens van de virtuele machine kunnen worden geschreven. U kunt de overeenkomende Site Recovery monitoring IP gebruiken als uw firewall proxy Ip's ondersteunt.

## <a name="outbound-connectivity-for-site-recovery-urls-or-ip-ranges-error-code-151037-or-151072"></a>Uitgaande connectiviteit voor Site Recovery Url's of IP-adresbereiken (fout code 151037 of 151072)

## <a name="issue-1-failed-to-register-azure-virtual-machine-with-site-recovery-151195-br"></a>Probleem 1: de virtuele machine van Azure kan niet worden geregistreerd met Site Recovery (151195) </br>
- **Mogelijke oorzaak** </br>
  - Er kan geen verbinding tot stand worden gebracht met Site Recovery-eind punten vanwege een DNS-omzettings fout.
  - Dit komt vaker voor tijdens het opnieuw beveiligen, wanneer u een failover hebt uitgevoerd voor de virtuele machine maar de DNS-server niet bereikbaar is vanuit de DR-regio.

- **Opgelost**
   - Als u aangepaste DNS gebruikt, moet u ervoor zorgen dat de DNS-server toegankelijk is vanuit het gebied voor nood herstel. Als u wilt controleren of u een aangepaste DNS-server hebt, gaat u naar de VM > nood herstel netwerk > DNS-servers. Probeer vanaf de virtuele machine toegang te krijgen tot de DNS-server. Als de service niet toegankelijk is, maakt u deze toegankelijk door een failover uit te voeren voor de DNS-server of door de regel van de site tussen DR-netwerk en DNS te maken.

    ![com-fout](./media/azure-to-azure-troubleshoot-errors/custom_dns.png)


## <a name="issue-2-site-recovery-configuration-failed-151196"></a>Probleem 2: configuratie van Site Recovery is mislukt (151196)

> [!NOTE]
> Als de virtuele machines zich achter de **standaard** interne Load Balancer bevinden, heeft deze standaard geen toegang tot O365 ip's (dat wil zeggen, login.microsoftonline.com). Wijzig de waarde in **Basic** interne Load Balancer type of maak uitgaande toegang zoals vermeld in het [artikel](https://aka.ms/lboutboundrulescli).

- **Mogelijke oorzaak** </br>
  - Er kan geen verbinding tot stand worden gebracht met Office 365-authenticatie en identiteits IP4-eind punten.

- **Opgelost**
  - Azure Site Recovery vereiste toegang tot Office 365 Ip's-bereiken voor verificatie.
    Als u Azure Network Security Group (NSG) regels/firewall proxy gebruikt voor het beheren van uitgaande netwerk connectiviteit op de VM, moet u communicatie met O365 bereiken toestaan. Een NSG [-regel (Azure AD)](../virtual-network/security-overview.md#service-tags) op basis van Azure Active Directory een service label maken voor toegang tot alle IP-adressen die overeenkomen met Azure AD
      - Als nieuwe adressen in de toekomst worden toegevoegd aan Azure AD, moet u nieuwe NSG-regels maken.

### <a name="example-nsg-configuration"></a>Voor beeld van NSG-configuratie

In dit voor beeld ziet u hoe u NSG-regels configureert voor replicatie van een virtuele machine.

- Als u NSG-regels gebruikt om de uitgaande connectiviteit te beheren, gebruikt u de regels HTTPS-uitgaand toestaan op poort: 443 voor alle vereiste IP-adresbereiken.
- In het voor beeld wordt ervan uitgegaan dat de bron locatie van de virtuele machine ' vs-Oost ' is en de doel locatie ' centraal VS ' is.

### <a name="nsg-rules---east-us"></a>NSG-regels-VS-Oost

1. Maak een uitgaande HTTPS (443)-beveiligings regel voor ' storage. oostelijke ' op de NSG, zoals wordt weer gegeven in de onderstaande scherm afbeelding.

      ![opslag code](./media/azure-to-azure-about-networking/storage-tag.png)

2. Maak een uitgaande HTTPS (443) beveiligings regel voor ' AzureActiveDirectory ' op de NSG, zoals weer gegeven in de onderstaande scherm afbeelding.

      ![Aad-tag](./media/azure-to-azure-about-networking/aad-tag.png)

3. Maak regels voor uitgaande HTTPS (443) voor de Site Recovery IP-adressen die overeenkomen met de doel locatie:

   **Locatie** | **Site Recovery IP-adres** |  **IP-adres van Site Recovery bewaking**
    --- | --- | ---
   US - centraal | 40.69.144.231 | 52.165.34.144

### <a name="nsg-rules---central-us"></a>NSG-regels-VS-Centraal

Deze regels zijn vereist zodat replicatie kan worden ingeschakeld vanuit de doel regio naar de bron regio na failover:

1. Maak een uitgaande HTTPS (443) beveiligings regel voor ' opslag. Centraalus ' op de NSG.

2. Maak een uitgaande HTTPS (443) beveiligings regel voor ' AzureActiveDirectory ' op de NSG.

3. Maak regels voor uitgaande HTTPS (443) voor de Site Recovery IP-adressen die overeenkomen met de bron locatie:

   **Locatie** | **Site Recovery IP-adres** |  **IP-adres van Site Recovery bewaking**
    --- | --- | ---
   US - centraal | 13.82.88.226 | 104.45.147.24
## <a name="issue-3-site-recovery-configuration-failed-151197"></a>Probleem 3: configuratie van Site Recovery is mislukt (151197)
- **Mogelijke oorzaak** </br>
  - Er kan geen verbinding tot stand worden gebracht met Azure Site Recovery service-eind punten.

- **Opgelost**
  - Azure Site Recovery vereist toegang tot de [IP-bereiken van Site Recovery](https://docs.microsoft.com/azure/site-recovery/azure-to-azure-about-networking#outbound-connectivity-for-ip-address-ranges) afhankelijk van de regio. Zorg ervoor dat de vereiste IP-bereiken toegankelijk zijn vanaf de virtuele machine.


## <a name="issue-4-a2a-replication-failed-when-the-network-traffic-goes-through-on-premises-proxy-server-151072"></a>Probleem 4: A2A-replicatie mislukt wanneer het netwerk verkeer via on-premises proxy server loopt (151072)
- **Mogelijke oorzaak** </br>
  - De aangepaste proxy instellingen zijn ongeldig en de proxy-instellingen van Internet Explorer zijn niet automatisch gedetecteerd door de Azure Site Recovery Mobility Service-agent


- **Opgelost**
  1. Mobility Service-agent detecteert de proxy-instellingen van Internet Explorer op Windows en/etc/environment in Linux.
  2. Als u liever proxy alleen instelt voor Azure Site Recovery Mobility-service, kunt u de proxy gegevens opgeven in ProxyInfo. conf op:</br>
     - ``/usr/local/InMage/config/`` in ***Linux***
     - ``C:\ProgramData\Microsoft Azure Site Recovery\Config`` in ***Windows***
  3. ProxyInfo.conf moet de proxy-instellingen in de volgende INI-indeling hebben.</br>
                *webtoepassingsproxy*</br>
                *Adres =http://1.2.3.4*</br>
                *Poort = 567*</br>
  4. Azure Site Recovery Mobility Service-agent ondersteunt alleen ***niet-geverifieerde proxy's***.

### <a name="fix-the-problem"></a>Het probleem oplossen
Volg de stappen in het [document voor netwerk](site-recovery-azure-to-azure-networking-guidance.md)ondersteuning om [de vereiste url's](azure-to-azure-about-networking.md#outbound-connectivity-for-urls) of de [vereiste IP-bereiken](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges)toe te staan.


## <a name="next-steps"></a>Volgende stappen
[Virtuele Azure-machines repliceren](site-recovery-replicate-azure-to-azure.md)
