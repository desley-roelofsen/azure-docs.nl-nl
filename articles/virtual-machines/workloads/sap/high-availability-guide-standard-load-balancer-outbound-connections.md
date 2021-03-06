---
title: Connectiviteit van open bare eind punten voor Virtual Machines met behulp van Azure Standard Load Balancer in scenario's met hoge Beschik baarheid van SAP
description: Connectiviteit van open bare eind punten voor Virtual Machines met behulp van Azure Standard Load Balancer in scenario's met hoge Beschik baarheid van SAP
services: virtual-machines-windows,virtual-network,storage,
documentationcenter: saponazure
author: rdeltcheva
manager: juergent
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-windows
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 10/28/2019
ms.author: radeltch
ms.openlocfilehash: ae2fb4c13633fa2ac22510a98e193bd9f01efb12
ms.sourcegitcommit: 38251963cf3b8c9373929e071b50fd9049942b37
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/29/2019
ms.locfileid: "73045380"
---
# <a name="public-endpoint-connectivity-for-virtual-machines-using-azure-standard-load-balancer-in-sap-high-availability-scenarios"></a>Connectiviteit van open bare eind punten voor Virtual Machines met behulp van Azure Standard Load Balancer in scenario's met hoge Beschik baarheid van SAP

Het bereik van dit artikel is het beschrijven van configuraties, waarmee de uitgaande verbinding met open bare eind punten kan worden ingeschakeld. De configuraties zijn voornamelijk in de context van hoge Beschik baarheid met pacemaker voor SUSE/RHEL.  

Als u pacemaker gebruikt met Azure Fence agent in uw oplossing met hoge Beschik baarheid, moeten de virtuele machines een uitgaande verbinding hebben met de Azure-beheer-API.  
Het artikel bevat verschillende opties waarmee u de optie kunt selecteren die het meest geschikt is voor uw scenario.  

## <a name="overview"></a>Overzicht

Bij het implementeren van hoge Beschik baarheid voor SAP-oplossingen via clusters, is een van de benodigde onderdelen [Azure Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview). Azure biedt twee load balancer Sku's: Standard en Basic.

Standard Azure load balancer biedt enkele voor delen ten opzichte van de basis load balancer. Het werkt bijvoorbeeld in azure-beschikbaarheids zones, het heeft betere controle en logboek registratie mogelijkheden voor eenvoudiger probleem oplossing, minder latentie. De functie HA-poorten heeft betrekking op alle poorten, dat wil zeggen dat het niet langer nodig is om alle afzonderlijke poorten weer te geven.  

Er zijn enkele belang rijke verschillen tussen de basis-en de standaard-SKU van Azure load balancer. Een van deze is de afhandeling van uitgaand verkeer naar het open bare eind punt. Zie [Load BALANCER SKU-vergelijking](https://docs.microsoft.com/azure/load-balancer/load-balancer-overview#skus)voor een volledige basis versus standaard SKU Load Balancer vergelijking.  
 
Wanneer Vm's zonder open bare IP-adressen in de back-end-groep van intern (geen openbaar IP-adres load balancer) worden geplaatst, is er geen uitgaande verbinding met open bare eind punten, tenzij er een extra configuratie wordt uitgevoerd.  

Als aan een virtuele machine een openbaar IP-adres is toegewezen of als de virtuele machine zich in de back-endadresgroep van een load balancer met een openbaar IP-adres bevindt, heeft deze een uitgaande verbinding met open bare eind punten.  

SAP-systemen bevatten vaak gevoelige Bedrijfs gegevens. Het is zelden acceptabel voor Vm's die fungeren als host voor SAP van een openbaar IP-adres. Op hetzelfde moment zijn er scenario's waarin uitgaande verbindingen van de VM naar open bare eind punten zouden moeten worden gemaakt.  

Voor beelden van scenario's waarbij toegang tot het open bare Azure-eind punt is vereist:  
- Azure Fence agent gebruiken als een omheinings mechanisme in pacemaker-clusters
- Azure Backup
- Azure Site Recovery  
- De open bare opslag plaats gebruiken voor het patchen van het besturings systeem
- De SAP-toepassings gegevens stroom vereist mogelijk een uitgaande verbinding naar het open bare eind punt

Als uw SAP-implementatie geen uitgaande verbinding met open bare eind punten nodig heeft, hoeft u de aanvullende configuratie niet te implementeren. Het is voldoende om interne standaard SKU Azure Load Balancer te maken voor uw scenario met hoge Beschik baarheid, ervan uitgaande dat er voor de inkomende connectiviteit van open bare eind punten ook geen verbinding nodig is.  

> [!Note]
> Wanneer Vm's zonder open bare IP-adressen in de back-endadresgroep van intern (geen openbaar IP-adres load balancer) worden geplaatst, is er geen uitgaande Internet verbinding, tenzij er aanvullende configuratie wordt uitgevoerd om route ring naar open bare eind punten toe te staan.  
>Als de virtuele machines een openbaar IP-adres hebben of al aanwezig zijn in de back-endadresgroep van de Azure Load Balancer met een openbaar IP-adres, heeft de virtuele machine al een uitgaande verbinding met open bare eind punten.


Lees eerst de volgende documenten:

* Azure Standard Load Balancer
  * [Overzicht van azure Standard Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-standard-overview) -uitgebreid overzicht van de Azure Standard Load Balancer, belang rijke principes, concepten en zelf studies 
  * [Uitgaande verbindingen in azure](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#scenarios) : scenario's voor het bezorgen van uitgaande connectiviteit in azure
  * [Uitgaande regels van Load Balancer](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview): hierin worden de concepten van Load Balancer regels voor uitgaande verbindingen en het maken van uitgaande regels uitgelegd
* Azure Firewall
  * [Overzicht van Azure firewall](https://docs.microsoft.com/azure/firewall/overview)-overzicht van Azure firewall
  * [Zelf studie: Azure firewall implementeren en configureren](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal) -instructies voor het configureren van Azure Firewall via Azure Portal
* [Virtuele netwerken-door de gebruiker gedefinieerde regels](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#user-defined) : Azure-routerings concepten en-regels  
* [Service tags voor beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) : Vereenvoudig uw netwerk beveiligings groepen en firewall configuratie met Service Tags

## <a name="additional-external-azure-standard-load-balancer-for-outbound-connections-to-internet"></a>Extra externe Azure-Standard Load Balancer voor uitgaande verbindingen met Internet

Een optie voor het behalen van uitgaande verbindingen naar open bare eind punten, zonder inkomende connectiviteit met de virtuele machine vanaf een openbaar eind punt toe te staan, is het maken van een tweede load balancer met een openbaar IP-adres, het toevoegen van de virtuele machines aan de back-end-groep van de tweede load balancer en het definiëren van alleen [Uitgaande regels](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-rules-overview).  
Gebruik [netwerk beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/security-overview) om de open bare eind punten te beheren die toegankelijk zijn voor uitgaande oproepen van de virtuele machine.  
Zie scenario 2 in [uitgaande verbindingen](https://docs.microsoft.com/azure/load-balancer/load-balancer-outbound-connections#scenarios)voor documenten voor meer informatie.  
De configuratie zou er als volgt uitzien:  

![Connectiviteit met open bare eind punten beheren met netwerk beveiligings groepen](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-public.png)

### <a name="important-considerations"></a>Belang rijke overwegingen

- U kunt één extra open bare Load Balancer voor meerdere Vm's in hetzelfde subnet gebruiken om uitgaande connectiviteit naar het open bare eind punt te verzorgen en de kosten te optimaliseren  
- Gebruik [netwerk beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/security-overview) om te bepalen welke open bare eind punten toegankelijk zijn vanaf de vm's. U kunt de netwerk beveiligings groep toewijzen aan het subnet of aan elke virtuele machine. Gebruik waar mogelijk [service Tags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) om de complexiteit van de beveiligings regels te reduceren.  
- Met de Azure Standard Load Balancer met een openbaar IP-adres en de uitgaande regels is directe toegang tot het open bare eind punt toegestaan. Als u beveiligings vereisten hebt om al het uitgaande verkeer door te geven via gecentraliseerde bedrijfs oplossing voor controle en logboek registratie, kunt u mogelijk niet aan de vereisten voldoen met dit scenario.  

>[!TIP]
>Gebruik waar mogelijk [service Tags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) om de complexiteit van de netwerk beveiligings groep te verminderen. 

### <a name="deployment-steps"></a>Implementatiestappen

1. Load Balancer maken  
   1. Klik in de [Azure Portal](https://portal.azure.com) op alle resources, toevoegen en zoek naar **Load Balancer**  
   1. Klik op **Maken**. 
   1. Load Balancer naam **MyPublicILB**  
   1. Selecteer **openbaar** als type, **standaard** als SKU  
   1. Selecteer **openbaar IP-adres maken** en opgeven als een naam **MyPublicILBFrondEndIP**  
   1. **Zone redundante** als beschikbaarheids zone selecteren  
   1. Klik op controleren en maken en vervolgens op maken  
2. Maak de back- **MyBackendPoolOfPublicILB** en voeg de vm's toe.  
   1. Het virtuele netwerk selecteren  
   1. Selecteer de Vm's en hun IP-adressen en voeg deze toe aan de back-end-groep  
3. [Uitgaande regels maken](https://docs.microsoft.com/azure/load-balancer/configure-load-balancer-outbound-cli#create-outbound-rule). Het is momenteel niet mogelijk om uitgaande regels te maken op basis van de Azure Portal. U kunt regels voor uitgaande verbindingen maken met [Azure cli](https://docs.microsoft.com/azure/cloud-shell/overview?view=azure-cli-latest).  

   ```
    az network lb outbound-rule create --address-pool MyBackendPoolOfPublicILB --frontend-ip-configs MyPublicILBFrondEndIP --idle-timeout 30 --lb-name MyPublicILB --name MyOutBoundRules  --outbound-ports 10000 --enable-tcp-reset true --protocol All --resource-group MyResourceGroup
   ```

4. Maak regels voor netwerk beveiligings groepen om de toegang tot specifieke open bare eind punten te beperken. Als er een bestaande netwerk beveiligings groep bestaat, kunt u deze aanpassen. In het onderstaande voor beeld ziet u hoe u alleen toegang tot de Azure-beheer-API kunt toestaan: 
   1. Ga naar de netwerk beveiligings groep
   1. Klik op uitgaande beveiligings regels
   1. Voeg een regel toe om alle uitgaande toegang tot **Internet**te **weigeren** .
   1. Voeg een regel toe om toegang tot **Cloud** **toe te staan** , met een lagere prioriteit dan de prioriteit van de regel om alle Internet toegang te weigeren.


   De regels voor uitgaande beveiliging zien er als volgt uit: 

   ![Uitgaande verbinding met tweede Load Balancer met een openbaar IP-adres](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-network-security-groups.png)

   Zie [beveiligings groepen ](https://docs.microsoft.com/azure/virtual-network/security-overview)voor meer informatie over Azure-netwerk beveiligings groepen. 

## <a name="azure-firewall-for-outbound-connections-to-internet"></a>Azure Firewall voor uitgaande verbindingen met Internet

Een andere optie voor het behalen van uitgaande connectiviteit naar open bare eind punten, zonder inkomende connectiviteit met de virtuele machine vanaf een openbaar eind punt toe te staan, is met Azure Firewall. Azure Firewall is een beheerde service met ingebouwde hoge Beschik baarheid en kan meerdere Beschikbaarheidszones omvatten.  
U moet ook de door de [gebruiker gedefinieerde route](https://docs.microsoft.com/azure/virtual-network/virtual-networks-udr-overview#custom-routes)implementeren die is gekoppeld aan het subnet waar vm's en de azure-Load Balancer worden geïmplementeerd, naar de Azure-firewall wijzen om het verkeer via de Azure firewall te routeren.  
Zie [Azure firewall implementeren en configureren](https://docs.microsoft.com/azure/firewall/tutorial-firewall-deploy-portal)voor meer informatie over het implementeren van Azure firewall.  

De architectuur ziet er als volgt uit:

![Uitgaande verbinding met Azure Firewall](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-firewall.png)

### <a name="important-considerations"></a>Belang rijke overwegingen

- Azure firewall is een native Cloud service, met ingebouwde hoge Beschik baarheid en ondersteunt de zonegebonden-implementatie.
- Vereist extra subnet met de naam AzureFirewallSubnet. 
- Als het overdragen van grote gegevens sets van het virtuele netwerk met de SAP-Vm's, naar een virtuele machine in een ander virtueel netwerk of naar een openbaar eind punt, is het mogelijk niet de juiste oplossing. Een voor beeld hiervan is het kopiëren van grote back-ups over virtuele netwerken. Zie Azure Firewall prijzen voor meer informatie.  
- Als de oplossing bedrijfs firewall niet Azure Firewall is en u beveiligings vereisten hebt om al het uitgaande verkeer uit te geven als gecentraliseerde bedrijfs oplossing, is deze oplossing mogelijk niet praktisch.  

>[!TIP]
>Gebruik waar mogelijk [service Tags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) om de complexiteit van de Azure firewall regels te verminderen.  

### <a name="deployment-steps"></a>Implementatiestappen

1. Bij de implementaties tappen wordt ervan uitgegaan dat u al een virtueel netwerk en een subnet hebt gedefinieerd voor uw virtuele machines.  
2. Maak subnet **AzureFirewallSubnet** in dezelfde Virtual Network, waarbij de vm's en de Standard Load Balancer worden geïmplementeerd.  
   1. Ga in Azure Portal naar de Virtual Network: Klik op alle resources, zoek naar de Virtual Network, klik op de Virtual Network en selecteer subnetten.  
   1. Klik op subnet toevoegen. Voer **AzureFirewallSubnet** in als naam. Voer het juiste adres bereik in. Sla op.  
3. Azure Firewall maken.  
   1. Klik in Azure Portal alle resources selecteren op toevoegen, firewall, maken. Selecteer resource groep (Selecteer dezelfde resource groep, waarbij de Virtual Network is).  
   1. Voer een naam in voor de Azure Firewall resource. Bijvoorbeeld **MyAzureFirewall**.  
   1. Selecteer regio en selecteer ten minste twee beschikbaarheids zones, uitgelijnd met de beschikbaarheids zones waar uw virtuele machines worden geïmplementeerd.  
   1. Selecteer uw Virtual Network, waarbij de SAP-Vm's en de Azure Standard Load Balancer worden geïmplementeerd.  
   1. Openbaar IP-adres: Klik op maken en voer een naam in. Voor instantie **MyFirewallPublicIP**.  
4. Maak Azure Firewall regel om uitgaande connectiviteit met opgegeven open bare eind punten toe te staan. Het voor beeld laat zien hoe u toegang tot het open bare eind punt van de Azure Management API kunt toestaan.  
   1. Selecteer regels, verzameling van netwerk regels en klik vervolgens op verzameling netwerk regels toevoegen.  
   1. Naam: **MyOutboundRule**, voer prioriteit in, Selecteer actie **toestaan**.  
   1. Service: naam **ToAzureAPI**.  Protocol: Selecteer **een**. Bron adres: Voer het bereik in voor uw subnet, waarbij de Vm's en Standard Load Balancer worden geïmplementeerd voor instance: **11.97.0.0/24**. Doel poorten: Voer <b>*</b>in.  
   1. Opslaan
   1. Selecteer overzicht als u nog steeds op de Azure Firewall hebt gepositioneerd. Noteer het privé-IP-adres van de Azure Firewall.  
5. Route naar Azure Firewall maken  
   1. In Azure Portal alle resources selecteren en klik vervolgens op toevoegen, route tabel, maken.  
   1. Voer de naam MyRouteTable in, selecteer abonnement, resource groep en locatie (die overeenkomt met de locatie van uw virtuele netwerk en firewall).  
   1. Opslaan  

   De firewall regel ziet er als volgt uit: ![uitgaande verbinding met Azure Firewall](./media/high-availability-guide-standard-load-balancer/high-availability-guide-standard-load-balancer-firewall-rule.png)

6. Een door de gebruiker gedefinieerde route maken op basis van het subnet van uw Vm's naar het privé-IP-adres van **MyAzureFirewall**.
   1. Wanneer u op de route tabel hebt gepositioneerd, klikt u op routes. Selecteer toevoegen. 
   1. Route naam: ToMyAzureFirewall, adres voorvoegsel: **0.0.0.0/0**. Type volgende hop: Selecteer een virtueel apparaat. Adres van volgende hop: Voer het privé-IP-adres in van de firewall die u hebt geconfigureerd: **11.97.1.4**.  
   1. Opslaan

## <a name="using-proxy-for-pacemaker-calls-to-azure-management-api"></a>Proxy gebruiken voor pacemaker-aanroepen naar Azure Management API

U kunt proxy gebruiken om pacemaker-aanroepen naar de open bare Azure Management API-eind punt toe te staan.  

### <a name="important-considerations"></a>Belang rijke overwegingen

  - Als er al een bedrijfs proxy aanwezig is, kunt u uitgaande oproepen naar open bare eind punten routeren. Uitgaande aanroepen naar open bare eind punten gaan door naar het bedrijfs controlepunt.  
  - Zorg ervoor dat de proxy configuratie uitgaande connectiviteit met de Azure-beheer-API toestaat: https://management.azure.com  
  - Zorg ervoor dat er een route van de virtuele machines naar de proxy is  
  - De proxy verwerkt alleen HTTP/HTTPS-aanroepen. Als er extra behoefte is aan het maken van uitgaande oproepen naar het open bare eind punt via verschillende protocollen (zoals RFC), is alternatieve oplossing vereist  
  - De proxy oplossing moet Maxi maal beschikbaar zijn om instabiliteit in het pacemaker-cluster te voor komen  
  - Afhankelijk van de locatie van de proxy kan dit extra latentie in de aanroepen van de Azure Fence-agent naar de Azure Management API veroorzaken. Als uw bedrijfs proxy zich nog steeds op de locatie bevindt, terwijl uw pacemaker-cluster zich in azure bevindt, meet latentie en houd er rekening mee dat deze oplossing geschikt is voor u  
  - Als er nog geen hoge beschik bare bedrijfs proxy aanwezig is, raden we u aan deze optie niet te kiezen omdat de klant extra kosten en complexiteit zou oplopen. Als u echter besluit extra proxy oplossingen te implementeren, moet u ervoor zorgen dat de proxy Maxi maal beschikbaar is en dat de latentie van de Vm's naar de proxy laag is voor het toestaan van uitgaande verbindingen van pacemaker naar de open bare API van Azure.  

### <a name="pacemaker-configuration-with-proxy"></a>Pacemaker-configuratie met proxy 

Er zijn veel verschillende proxy opties beschikbaar in de branche. Stapsgewijze instructies voor de proxy-implementatie vallen buiten het bereik van dit document. In het onderstaande voor beeld gaan we ervan uit dat uw proxy reageert op **MyProxyService** en luistert naar poort **MyProxyPort**.  
Voer de volgende stappen uit op alle cluster knooppunten om pacemaker toe te staan om te communiceren met de Azure Management API:  

1. Bewerk het pacemaker-configuratie bestand/etc/sysconfig/pacemaker en voeg de volgende regels toe (alle cluster knooppunten):  
   ```
   sudo vi /etc/sysconfig/pacemaker
   # Add the following lines
   http_proxy=http://MyProxyService:MyProxyPort
   https_proxy=http://MyProxyService:MyProxyPort
   ```

2. Start de pacemaker-service op **alle** cluster knooppunten opnieuw.  
  - SUSE  
     ```
     # Place the cluster in maintenance mode
     sudo pcs property set maintenance-mode=true
     #Restart on all nodes
     sudo systemctl restart pacemaker
     # Take the cluster out of maintenance mode
     sudo pcs property set maintenance-mode=false
     ```

  - Red Hat  
     ```
     # Place the cluster in maintenance mode
     sudo pcs property set maintenance-mode=true
     #Restart on all nodes
     sudo systemctl restart pacemaker
     # Take the cluster out of maintenance mode
     sudo pcs property set maintenance-mode=false
     ```

## <a name="next-steps"></a>Volgende stappen

* [Meer informatie over het configureren van pacemaker op SUSE in azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-pacemaker)
* [Meer informatie over het configureren van pacemaker op Red Hat in azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-pacemaker)
