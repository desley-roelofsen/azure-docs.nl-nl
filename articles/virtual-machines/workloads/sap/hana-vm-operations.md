---
title: Configuraties en bewerkingen van infrastructuur SAP HANA op Azure | Microsoft Docs
description: Bedienings handleiding voor SAP HANA systemen die zijn geïmplementeerd op virtuele machines van Azure.
services: virtual-machines-linux,virtual-machines-windows
documentationcenter: ''
author: msjuergent
manager: bburns
editor: ''
tags: azure-resource-manager
keywords: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/01/2019
ms.author: juergent
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 1547f0e600031f558dcc0157df2a35fdf3f9db2c
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74224691"
---
# <a name="sap-hana-infrastructure-configurations-and-operations-on-azure"></a>Configuraties en bewerkingen van SAP HANA-infrastructuur in Azure
Dit document bevat richt lijnen voor het configureren van Azure-infra structuur en-besturings SAP HANA systemen die zijn geïmplementeerd op Azure native virtual machines (Vm's). Het document bevat ook configuratie-informatie voor SAP HANA scale-out voor de M128s VM-SKU. Dit document is niet bedoeld als vervanging van de Standard SAP-documentatie, die de volgende inhoud bevat:

- [SAP-beheer handleiding](https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.02/330e5550b09d4f0f8b6cceb14a64cd22.html)
- [SAP-installatie handleidingen](https://service.sap.com/instguides)
- [SAP-opmerkingen](https://sservice.sap.com/notes)

## <a name="prerequisites"></a>Vereisten
Als u deze hand leiding wilt gebruiken, moet u basis kennis hebben van de volgende Azure-onderdelen:

- [Virtuele machines van Azure](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-vm)
- [Azure-netwerken en virtuele netwerken](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-virtual-network)
- [Azure Storage](https://docs.microsoft.com/azure/virtual-machines/linux/tutorial-manage-disks)

Zie de sectie [SAP on Azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/get-started) van de [Azure-documentatie](https://docs.microsoft.com/azure/)voor meer informatie over SAP NetWeaver en andere SAP-onderdelen op Azure.

## <a name="basic-setup-considerations"></a>Algemene overwegingen bij de installatie
In de volgende secties worden de belangrijkste overwegingen beschreven voor het implementeren van SAP HANA systemen op Azure-Vm's.

### <a name="connect-into-azure-virtual-machines"></a>Verbinding maken met virtuele machines van Azure
Zoals beschreven in de [plannings handleiding voor Azure virtual machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/planning-guide)zijn er twee basis methoden om verbinding te maken met Azure vm's:

- Maak verbinding via internet en open bare eind punten op een Jump VM of op de VM met SAP HANA.
- Maak verbinding via een [VPN-](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal) of Azure- [ExpressRoute](https://azure.microsoft.com/services/expressroute/).

Site-naar-site-connectiviteit via VPN of ExpressRoute is nodig voor productie scenario's. Dit type verbinding is ook nodig voor niet-productie scenario's waarbij wordt gefeedd in productie scenario's waarin SAP-software wordt gebruikt. In de volgende afbeelding ziet u een voor beeld van connectiviteit tussen sites:

![Connectiviteit tussen sites](media/virtual-machines-shared-sap-planning-guide/300-vpn-s2s.png)


### <a name="choose-azure-vm-types"></a>Azure VM-typen kiezen
De Azure VM-typen die kunnen worden gebruikt voor productie scenario's, worden vermeld in de [SAP-documentatie voor IaaS](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html). Voor niet-productie scenario's is er een groter aantal systeem eigen Azure VM-typen beschikbaar.

>[!NOTE]
> Voor niet-productie scenario's gebruikt u de VM-typen die worden vermeld in de [SAP-notitie #1928533](https://launchpad.support.sap.com/#/notes/1928533). Voor het gebruik van virtuele Azure-machines voor productie scenario's controleert u op SAP HANA gecertificeerde Vm's in de lijst met door SAP gepubliceerde [gecertificeerde IaaS-platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

Implementeer de virtuele machines in azure met behulp van:

- Het Azure Portal.
- Azure PowerShell-cmdlets.
- De Azure CLI.

U kunt ook een volledig geïnstalleerd SAP HANA-platform implementeren op de Azure VM-Services via het [SAP-Cloud platform](https://cal.sap.com/). Het installatie proces wordt beschreven in [SAP S/4HANA of BW/4HANA op Azure implementeren](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/cal-s4h) of met de automatisering die [hier](https://github.com/AzureCAT-GSI/SAP-HANA-ARM)is uitgebracht.

>[!IMPORTANT]
> Als u M208xx_v2 Vm's wilt gebruiken, moet u zorgvuldig uw Linux-installatie kopie selecteren in de galerie met installatie kopieën van Azure VM. Lees de [grootten van de virtuele machines geoptimaliseerd voor geheugen](https://docs.microsoft.com/azure/virtual-machines/windows/sizes-memory#mv2-series)om de details te lezen. 
> 


### <a name="storage-configuration-for-sap-hana"></a>Opslag configuratie voor SAP HANA
Lees het document [SAP Hana Azure virtual machine-opslag configuraties](./hana-vm-operations-storage.md) voor opslag configuraties en opslag typen die moeten worden gebruikt met SAP Hana in Azure.


### <a name="set-up-azure-virtual-networks"></a>Virtuele Azure-netwerken instellen
Wanneer u een site-naar-site-verbinding hebt met Azure via VPN of ExpressRoute, moet u ten minste één virtueel Azure-netwerk hebben dat is verbonden via een virtuele gateway naar het VPN-of ExpressRoute-circuit. In eenvoudige implementaties kan de virtuele gateway worden geïmplementeerd in een subnet van het virtuele Azure-netwerk (VNet) dat als host fungeert voor de SAP HANA exemplaren. Als u SAP HANA wilt installeren, maakt u twee extra subnetten in het virtuele Azure-netwerk. Eén subnet fungeert als host voor de virtuele machines om de SAP HANA-exemplaren uit te voeren. Het andere subnet voert JumpBox of Management-Vm's uit voor het hosten van SAP HANA Studio, andere beheer software of uw toepassings software.

> [!IMPORTANT]
> Buiten de functionaliteit, maar belang rijker vanwege de prestaties wordt niet ondersteund voor het configureren van [virtuele Azure-netwerk apparaten](https://azure.microsoft.com/solutions/network-appliances/) in het communicatie traject tussen de SAP-toepassing en de DBMS-laag van een SAP NetWeaver, hybris of S/4HANA SAP-systeem. De communicatie tussen de SAP-toepassingslaag en de DBMS-laag moet direct een van beide zijn. De beperking omvat geen [Azure ASG-en NSG-regels](https://docs.microsoft.com/azure/virtual-network/security-overview) zolang deze ASG-en NSG-regels directe communicatie toestaan. Andere scenario's waarbij Nva's niet worden ondersteund, zijn in communicatie paden tussen Azure-Vm's die Linux pacemaker-cluster knooppunten en SBD-apparaten vertegenwoordigen, zoals wordt beschreven in [hoge Beschik baarheid voor SAP NetWeaver op Azure vm's op SuSE Linux Enterprise Server voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse). Of in communicatie paden tussen virtuele machines van Azure en Windows Server SOFS, zoals beschreven in een [SAP ASCS/SCS-exemplaar op een Windows-failovercluster in een cluster met behulp van een bestands share in azure](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-high-availability-guide-wsfc-file-share). Nva's in communicatie paden kunnen eenvoudig de netwerk latentie tussen twee communicatie partners verdubbelen, kan de door Voer beperken tot kritieke paden tussen de SAP-toepassingslaag en de DBMS-laag. In sommige scenario's met klanten kan Nva's ertoe leiden dat pacemaker Linux-clusters mislukken wanneer de communicatie tussen de Linux pacemaker-cluster knooppunten via een NVA moet communiceren met hun SBD-apparaat.  
> 

> [!IMPORTANT]
> Een ander ontwerp dat **niet** wordt ondersteund, is de schei ding van de SAP-toepassingslaag en de DBMS-laag in verschillende virtuele netwerken [van Azure die niet met elkaar](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) zijn gekoppeld. Het is raadzaam om de SAP-toepassingslaag en de DBMS-laag te scheiden met subnetten in een virtueel Azure-netwerk in plaats van verschillende virtuele netwerken van Azure te gebruiken. Als u besluit de aanbeveling niet te volgen en in plaats daarvan de twee lagen te scheiden in verschillende virtuele netwerken, moeten de twee virtuele netwerken worden [gekoppeld.](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) Houd er rekening mee dat het netwerk verkeer tussen twee [gepeerde](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) virtuele netwerken van Azure onderhevig is aan overdrachts kosten. Met het enorme gegevens volume in veel terabytes tussen de SAP-toepassingslaag en de DBMS-laag kunnen aanzienlijke kosten worden gecumuleerd als de SAP-toepassingslaag en de DBMS-laag worden gescheiden tussen twee peered virtuele netwerken van Azure. 

Wanneer u de Vm's installeert om SAP HANA uit te voeren, moeten de Vm's:

- Er zijn twee virtuele Nic's geïnstalleerd: één NIC om verbinding te maken met het subnet van het beheer en één NIC om verbinding te maken vanuit het on-premises netwerk of andere netwerken, naar het SAP HANA-exemplaar in de Azure VM.
- Statische privé-IP-adressen die worden geïmplementeerd voor virtuele Nic's.

> [!NOTE]
> Wijs statische IP-adressen via Azure toe aan afzonderlijke Vnic's. Wijs geen vaste IP-adressen in het gast besturingssysteem toe aan een vNIC. Sommige Azure-Services, zoals Azure Backup-Service, zijn afhankelijk van het feit dat ten minste de primaire vNIC is ingesteld op DHCP en niet op statische IP-adressen. Zie ook het document [problemen met back-up van Azure Virtual Machine oplossen](https://docs.microsoft.com/azure/backup/backup-azure-vms-troubleshoot#networking). Als u meerdere statische IP-adressen aan een virtuele machine wilt toewijzen, moet u meerdere Vnic's toewijzen aan een virtuele machine.
>
>

Voor implementaties die zich nu voordoen, moet u echter een Virtual Data Center-netwerk architectuur in azure maken. Deze architectuur adviseert de schei ding van de Azure VNet-gateway die is verbonden met on-premises in een afzonderlijk Azure-VNet. Dit afzonderlijke VNet moet het hosten van al het verkeer dat naar een on-premises of het internet blijft. Met deze aanpak kunt u software implementeren voor controle en logboek registratie van verkeer waarmee het virtuele Data Center in Azure in dit afzonderlijke hub VNet wordt ingevoerd. U hebt dus één VNet dat als host fungeert voor alle software en configuraties die betrekking hebben op in-en uitgaand verkeer naar uw Azure-implementatie.

De artikelen [Azure Virtual Data Center: een netwerk perspectief](https://docs.microsoft.com/azure/architecture/vdc/networking-virtual-datacenter) en [Azure Virtual Data Center en het Enter prise Control-vlak](https://docs.microsoft.com/azure/architecture/vdc/) geven meer informatie over de virtuele datacenter benadering en het gerelateerde Azure VNet-ontwerp.


>[!NOTE]
>Voor verkeer dat stroomt tussen een hub VNet en spoke VNet met behulp van [Azure VNet-peering](https://docs.microsoft.com/azure/virtual-network/virtual-network-peering-overview) , worden extra [kosten](https://azure.microsoft.com/pricing/details/virtual-network/)in rekening gebracht. Op basis van deze kosten moet u wellicht overwegen om inbreuk te maken op het uitvoeren van een strikt hub-en spoke-netwerk ontwerp en het uitvoeren van meerdere [Azure ExpressRoute-gateways](https://docs.microsoft.com/azure/expressroute/expressroute-about-virtual-network-gateways) waarmee u verbinding maakt met ' spokes ' om VNet-peering over te slaan. Azure ExpressRoute-gateways introduceren echter ook extra [kosten](https://azure.microsoft.com/pricing/details/vpn-gateway/) . Er kunnen ook extra kosten voor software van derden optreden die u gebruikt voor het vastleggen van netwerk verkeer, controle en bewaking. Afhankelijk van de kosten voor het uitwisselen van gegevens met behulp van VNet-peering aan de ene kant en de kosten die zijn gemaakt door extra Azure ExpressRoute-gateways en aanvullende software licenties, kunt u kiezen voor micro segmentering binnen één VNet met behulp van subnetten als isolatie-eenheid in plaats van VNets.


Zie [IP-adres typen en toewijzings methoden in azure](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)voor een overzicht van de verschillende methoden voor het toewijzen van IP-adressen. 

Voor Vm's met SAP HANA moet u werken met vaste IP-adressen die zijn toegewezen. Reden is dat sommige configuratie kenmerken voor HANA-referentie-IP-adressen.

[Azure-netwerk beveiligings groepen (nsg's)](https://docs.microsoft.com/azure/virtual-network/virtual-networks-nsg) worden gebruikt voor het omleiden van verkeer dat wordt doorgestuurd naar het SAP Hana-exemplaar of de JumpBox. De Nsg's en uiteindelijk [toepassings beveiligings groepen](https://docs.microsoft.com/azure/virtual-network/security-overview#application-security-groups) zijn gekoppeld aan de SAP Hana subnet en het beheer subnet.

De volgende afbeelding toont een overzicht van een ruwe implementatie schema voor SAP HANA na een hub-en spoke-VNet-architectuur:

![Ruw implementatie schema voor SAP HANA](media/hana-vm-operations/hana-simple-networking.PNG)

Als u SAP HANA in azure wilt implementeren zonder een site-naar-site-verbinding, wilt u de SAP HANA instantie van het open bare Internet nog steeds afschermen en verbergen achter een doorstuur proxy. In dit basis scenario is de implementatie afhankelijk van de ingebouwde DNS-services van Azure om hostnamen op te lossen. In een complexere implementatie waarbij open bare IP-adressen worden gebruikt, zijn de ingebouwde DNS-services van Azure vooral belang rijk. Gebruik Azure Nsg's en [Azure nva's](https://azure.microsoft.com/solutions/network-appliances/) om de route ring van het internet naar uw Azure VNet-architectuur in azure te beheren. De volgende afbeelding toont een ruw schema voor het implementeren van SAP HANA zonder een site-naar-site-verbinding in een hub-en spoke-VNet-architectuur:
  
![Ruw implementatie schema voor SAP HANA zonder een site-naar-site-verbinding](media/hana-vm-operations/hana-simple-networking2.PNG)
 

Een andere beschrijving van het gebruik van Azure Nva's voor het beheren en controleren van de toegang vanaf internet zonder de hub-en spoke VNet-architectuur, vindt u in het artikel [Maxi maal beschik bare virtuele netwerk apparaten implementeren](https://docs.microsoft.com/azure/architecture/reference-architectures/dmz/nva-ha).


## <a name="configuring-azure-infrastructure-for-sap-hana-scale-out"></a>Azure-infra structuur voor SAP HANA scale-out configureren
Micro soft heeft een VM-SKU van één M-serie die is gecertificeerd voor een SAP HANA scale-out-configuratie. Het VM-type M128s is gecertificeerd voor een scale-out van Maxi maal 16 knoop punten. Voor wijzigingen in SAP HANA scale-out certificeringen op virtuele machines van Azure controleert u de [lijst gecertificeerde IaaS-platforms](https://www.sap.com/dmc/exp/2014-09-02-hana-hardware/enEN/iaas.html#categories=Microsoft%20Azure).

De minimale OS-Releases voor het implementeren van scale-out configuraties in virtuele Azure-machines zijn:

- SUSE Linux 12 SP3
- Red Hat Linux 7,4

Van de scale-out certificering van 16 knoop punten

- Eén knoop punt is het hoofd knooppunt
- Een maximum van 15 knoop punten zijn werk knooppunten

>[!NOTE]
>In azure VM scale-out-implementaties is het niet mogelijk een stand-by-knoop punt te gebruiken
>

Hoewel Azure een systeem eigen NFS-service heeft met [Azure NetApp files](https://azure.microsoft.com/services/netapp/), is de NFS-service, hoewel ondersteund voor de SAP-toepassingslaag, nog niet gecertificeerd voor SAP Hana. Als gevolg hiervan moeten NFS-shares nog steeds worden geconfigureerd met behulp van de functionaliteit van derden. 


Als gevolg hiervan kunnen **/Hana/data** -en **/Hana/log** -volumes niet worden gedeeld. Deze volumes van de afzonderlijke knoop punten niet delen, voor komt het gebruik van een SAP HANA stand-by-knoop punt in een scale-out configuratie.

Als gevolg hiervan wordt het basis ontwerp voor één knoop punt in een scale-out configuratie als volgt weer geven:

![Basis beginselen van één knoop punt uitschalen](media/hana-vm-operations/scale-out-basics.PNG)

De basis configuratie van een VM-knoop punt voor SAP HANA uitschalen ziet er als volgt uit:

- Voor **/Hana/Shared**moet u een Maxi maal beschik bare NFS-share bouwen. Tot nu toe zijn er verschillende mogelijkheden om een hoge beschik bare share te verkrijgen. Deze worden beschreven in combi natie met SAP net-Weaver:
    - [Hoge Beschik baarheid voor NFS op Azure Vm's op SUSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs)
    - [GlusterFS op Azure VM's op Red Hat Enterprise Linux voor SAP NetWeaver](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-glusterfs)
    - [Hoge Beschik baarheid voor SAP NetWeaver op Azure Vm's op SUSE Linux Enterprise Server met Azure NetApp Files voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-netapp-files)
    - [Azure Virtual Machines hoge Beschik baarheid voor SAP NetWeaver op Red Hat Enterprise Linux met Azure NetApp Files voor SAP-toepassingen](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-rhel-netapp-files)
- Alle andere schijf volumes worden **niet** gedeeld tussen de verschillende knoop punten en zijn **niet** gebaseerd op NFS. Installatie configuraties en-stappen voor het uitschalen van HANA-installaties met niet-gedeelde **/Hana/data** en **/Hana/log** , worden verderop in dit document beschreven.

>[!NOTE]
>Het Maxi maal beschik bare NFS-cluster zoals het wordt weer gegeven in de afbeeldingen wordt beschreven in [hoge Beschik baarheid voor NFS op Azure-vm's op SuSE Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs). Andere mogelijkheden worden beschreven in de bovenstaande lijst.

Het formaat van de volumes voor de knoop punten is hetzelfde als voor omhoog schalen, met uitzonde ring van **/Hana/Shared**. De voorgestelde grootten en typen voor de M128s-VM-SKU zien er als volgt uit:

| VM-SKU | RAM | Met maximaal VM-I/O<br /> Doorvoer | /hana/data | /hana/log | /root volume | /usr/sap | Hana/back-up |
| --- | --- | --- | --- | --- | --- | --- | --- |
| M128s | 2000 GiB | 2000 MB/s |3 x P30 | 2 x P20 | 1 x P6 | 1 x P6 | 2 x P40 |


Controleer of de opslag doorvoer voor de verschillende voorgestelde volumes voldoet aan de werk belasting die u wilt uitvoeren. Als voor de werk belasting hogere volumes zijn vereist voor **/Hana/data** en **/Hana/log**, moet u het aantal Azure Premium Storage vhd's verg Roten. Het formaat van een volume met meer Vhd's dan wordt weer gegeven, verhoogt de IOPS en I/O-door Voer binnen de limieten van het type virtuele machine van Azure. Pas Azure-Write Accelerator ook toe op de schijven die het **/Hana/log** -volume vormen.
 
In het document [SAP Hana TDI-opslag vereisten](https://www.sap.com/documents/2015/03/74cdb554-5a7c-0010-82c7-eda71af511fa.html)is een formule een naam die de grootte van het **/Hana/Shared** -volume definieert voor uitschalen als de geheugen grootte van één worker-knoop punt per vier worker-knoop punten.

Als u de SAP HANA scale-out Certified M128s Azure VM met een ongeveer 2 TB geheugen haalt, kunnen de SAP-aanbevelingen worden samengevoegd zoals:

- Een hoofd knooppunt en Maxi maal vier worker-knoop punten: het **/Hana/Shared** -volume moet 2 TB aan grootte hebben. 
- Eén hoofd knooppunt en vijf tot acht werk knooppunten, de grootte van **/Hana/Shared** moet 4 TB zijn. 
- Er is een hoofd knooppunt en 9 tot 12 werk knooppunten met een grootte van 6 TB voor **/Hana/Shared** vereist. 
- Eén Master-knoop punt en gebruik tussen 12 en 15 werk knooppunten, u moet een **/Hana/Shared** -volume opgeven dat groter is dan 8 TB.

Het andere belang rijke ontwerp dat wordt weer gegeven in de grafische afbeeldingen van de configuratie met één knoop punt voor een scale-out SAP HANA VM is het VNet of de configuratie van het subnet verhogen. SAP beveelt een schei ding van het verkeer van de client/toepassing aan van de communicatie tussen de HANA-knoop punten. Zoals in de grafische afbeelding wordt weer gegeven, wordt dit doel bereikt door twee verschillende Vnic's toe te voegen aan de virtuele machine. Beide Vnic's bevinden zich in verschillende subnetten, hebben twee verschillende IP-adressen. Vervolgens beheert u de stroom van verkeer met routerings regels met behulp van Nsg's of door de gebruiker gedefinieerde routes.

Met name in azure zijn er geen middelen en methoden voor het afdwingen van Quality of service en quota's op specifieke Vnic's. Als gevolg hiervan kan de schei ding van client/toepassing en intra-node-communicatie geen kansen openen om de ene verkeers stroom over de andere te priori teren. In plaats daarvan behoudt de schei ding een beveiligings maatregel bij het afschermen van de intra-knooppunt communicatie van de scale-out-configuraties.  

>[!IMPORTANT]
>SAP raadt het netwerk verkeer ten opzichte van de client-en toepassings kant en verkeer binnen het knoop punt, zoals beschreven in dit document, te scheiden. Daarom is het raadzaam een architectuur te plaatsen, zoals wordt weer gegeven in de laatste grafische oplossing.
>

Vanuit een netwerk punt met de minimale vereiste netwerk architectuur zou er als volgt uitzien:

![Basis beginselen van één knoop punt uitschalen](media/hana-vm-operations/scale-out-networking-overview.PNG)

De limieten die tot nu toe worden ondersteund, zijn 15 werk nemers die zijn opgenomen in het hoofd knooppunt.

Vanuit een opslag punt van de opslag architectuur ziet er als volgt uit:


![Basis beginselen van één knoop punt uitschalen](media/hana-vm-operations/scale-out-storage-overview.PNG)

Het **/Hana/Shared** -volume bevindt zich in de configuratie met Maxi maal beschik bare NFS-share. Dat alle andere stations lokaal zijn gekoppeld aan de afzonderlijke Vm's. 

### <a name="highly-available-nfs-share"></a>Maxi maal beschik bare NFS-share
Het Maxi maal beschik bare NFS-cluster werkt tot nu toe met SUSE Linux. Het document [hoge Beschik baarheid voor NFS op virtuele Azure-machines in Suse Linux Enterprise Server](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/high-availability-guide-suse-nfs) beschrijft hoe u het kunt instellen. Als u het NFS-cluster niet deelt met andere HANA-configuraties buiten de Azure VNet die de instanties van de SAP HANA uitvoeren, installeert u deze in hetzelfde VNet. Installeer de app in een eigen subnet en zorg ervoor dat niet al het wille keurig verkeer toegang heeft tot het subnet. In plaats daarvan wilt u het verkeer naar dat subnet beperken tot de IP-adressen van de virtuele machine die het verkeer naar **/Hana/Shared** volume uitvoeren.

Met betrekking tot de vNIC van een HANA scale-out-VM die het **/Hana/Shared** -verkeer moet routeren, zijn de aanbevelingen:

- Omdat verkeer naar **/Hana/Shared** gemiddeld is, stuurt u het door via de vNIC die is toegewezen aan het client netwerk in de minimale configuratie
- Ten slotte, voor het verkeer naar **/Hana/Shared**, implementeert u een derde subnet in het VNet. u implementeert de SAP Hana scale-out configuratie en wijst een derde vNIC toe die wordt gehost in dat subnet. Gebruik het derde vNIC en het bijbehorende IP-adres voor het verkeer naar de NFS-share. Vervolgens kunt u afzonderlijke toegangs-en routerings regels Toep assen.

>[!IMPORTANT]
>Netwerk verkeer tussen de virtuele machines die SAP HANA hebben in een scale-out-implementatie en de Maxi maal beschik bare NFS kan onder geen enkele omstandigheden worden gerouteerd via een [NVA](https://azure.microsoft.com/solutions/network-appliances/) of soort gelijke virtuele apparaten. Terwijl Azure Nsg's geen dergelijke apparaten zijn. Controleer uw regels voor door sturen om ervoor te zorgen dat Nva's of soort gelijke virtuele apparaten worden uitgesteld wanneer ze toegang krijgen tot de Maxi maal beschik bare NFS-share van de virtuele machines met SAP HANA.
> 

Als u het Maxi maal beschik bare NFS-cluster wilt delen tussen SAP HANA configuraties, verplaatst u al deze HANA-configuraties naar hetzelfde VNet. 
 

### <a name="installing-sap-hana-scale-out-n-azure"></a>SAP HANA scale-out n Azure installeren
Een scale-out SAP-configuratie installeren, moet u de volgende stappen uitvoeren:

- Nieuwe Azure VNet-infra structuur implementeren of aanpassen
- De nieuwe virtuele machines implementeren met behulp van door Azure beheerde Premium Storage volumes
- Een nieuw Maxi maal beschik bare NFS-cluster implementeren of aanpassen
- Netwerk routering aanpassen om ervoor te zorgen dat de communicatie tussen knoop punten tussen Vm's bijvoorbeeld niet via een [NVA](https://azure.microsoft.com/solutions/network-appliances/)wordt gerouteerd. Hetzelfde geldt voor verkeer tussen de virtuele machines en het Maxi maal beschik bare NFS-cluster.
- Installeer het SAP HANA hoofd knooppunt.
- Configuratie parameters van het knoop punt SAP HANA model aanpassen
- Ga door met de installatie van de SAP HANA worker-knoop punten

#### <a name="installation-of-sap-hana-in-scale-out-configuration"></a>Installatie van SAP HANA in scale-out configuratie
Als uw Azure VM-infra structuur wordt geïmplementeerd en alle andere voor bereidingen worden uitgevoerd, moet u de SAP HANA scale-out-configuraties installeren in de volgende stappen:

- Installeer het SAP HANA-hoofd knooppunt volgens de documentatie van SAP
- **Na de installatie moet u het bestand Global. ini wijzigen en de para meter ' basepath_shared = no ' toevoegen aan Global. ini**. Met deze para meter kunnen SAP HANA worden uitgevoerd in scale-out zonder gedeelde **/Hana/data** -en **/Hana/log** -volumes tussen de knoop punten. Details worden beschreven in [SAP Note #2080991](https://launchpad.support.sap.com/#/notes/2080991).
- Nadat u de para meter Global. ini hebt gewijzigd, start u de SAP HANA-instantie opnieuw op
- Voeg extra worker-knoop punten toe. Zie ook <https://help.sap.com/viewer/6b94445c94ae495c83a19646e7c3fd56/2.0.00/en-US/0d9fe701e2214e98ad4f8721f6558c34.html>. Geef het interne netwerk voor SAP HANA communicatie tussen knoop punten op tijdens de installatie of later met behulp van bijvoorbeeld de lokale hdblcm. Zie ook [SAP opmerking #2183363](https://launchpad.support.sap.com/#/notes/2183363)voor gedetailleerdere documentatie. 

Na deze setup-routine wordt de scale-out-configuratie die u hebt geïnstalleerd, gebruikt om niet-gedeelde schijven te gebruiken voor het uitvoeren van **/Hana/data** en **/Hana/log**. Terwijl het **/Hana/Shared** -volume wordt geplaatst op de Maxi maal beschik bare NFS-share.


## <a name="sap-hana-dynamic-tiering-20-for-azure-virtual-machines"></a>SAP HANA Dynamic Tiering 2,0 voor virtuele machines van Azure

Naast de SAP HANA-certificeringen op virtuele machines uit de M-serie van Azure, wordt SAP HANA Dynamic Tiering 2,0 ook ondersteund op Microsoft Azure (Zie SAP HANA koppelingen naar dynamische lagen naar een andere laag). Hoewel er geen verschil is bij het installeren van het product of het besturings systeem, bijvoorbeeld via SAP HANA cockpit binnen een virtuele Azure-machine, zijn er enkele belang rijke items die verplicht zijn voor officiële ondersteuning op Azure. Deze belangrijkste punten worden hieronder beschreven. In het hele artikel wordt de afkorting ' DT 2,0 ' gebruikt in plaats van de volledige naam Dynamic Tiering 2,0.

SAP HANA Dynamic Tiering 2,0 wordt niet ondersteund door SAP BW of S4HANA. Belangrijkste use cases zijn nu de native HANA-toepassingen.


### <a name="overview"></a>Overzicht

In de onderstaande afbeelding vindt u een overzicht van de ondersteuning voor DT 2,0 op Microsoft Azure. Er is een reeks verplichte vereisten die moeten worden gevolgd om te voldoen aan de officiële certificering:

- DT 2,0 moet worden geïnstalleerd op een specifieke virtuele machine van Azure. Het kan niet worden uitgevoerd op dezelfde VM waar SAP HANA wordt uitgevoerd
- SAP HANA-en DT 2,0-Vm's moeten binnen hetzelfde Azure-Vnet worden geïmplementeerd
- De SAP HANA-en DT 2,0 Vm's moeten worden geïmplementeerd met de functie voor versneld netwerken van Azure ingeschakeld
- Het opslag type voor de DT 2,0-Vm's moet Azure Premium Storage
- Er moeten meerdere Azure-schijven zijn gekoppeld aan de DT 2,0 VM
- Het is vereist om een software-RAID/striped volume te maken (via LVM of mdadm) met behulp van striping over de Azure-schijven

Meer informatie vindt u in de volgende secties.

![Overzicht van SAP HANA DT 2,0-architectuur](media/hana-vm-operations/hana-dt-20.PNG)



### <a name="dedicated-azure-vm-for-sap-hana-dt-20"></a>Exclusieve Azure-VM voor SAP HANA DT 2,0

In azure IaaS wordt DT 2,0 alleen ondersteund op een specifieke virtuele machine. Het is niet toegestaan om DT 2,0 uit te voeren op dezelfde virtuele Azure-machine waarop het HANA-exemplaar wordt uitgevoerd. De eerste twee VM-typen kunnen worden gebruikt om SAP HANA DT 2,0 uit te voeren:

- M64-32ms 
- E32sv3 

Zie [hier](https://docs.microsoft.com/azure/virtual-machines/linux/sizes-memory) de beschrijving van het VM-type

Gezien het basis idee van DT 2,0, dat informatie over het offloaden van ' warme ' gegevens om kosten te besparen, is het zinvol om de bijbehorende VM-grootten te gebruiken. Er is geen strikte regel die betrekking heeft op de mogelijke combi Naties. Dit is afhankelijk van de specifieke werk belasting van de klant.

Aanbevolen configuraties zijn:

| SAP HANA VM-type | DT 2,0 VM-type |
| --- | --- | 
| M128ms | M64-32ms |
| M128s | M64-32ms |
| M64ms | E32sv3 |
| M64s | E32sv3 |


Alle combi Naties van SAP HANA-gecertificeerde M-serie-Vm's met ondersteunde DT 2,0 Vm's (M64-32MS en E32sv3) zijn mogelijk.


### <a name="azure-networking-and-sap-hana-dt-20"></a>Azure-netwerken en SAP HANA DT 2,0

Het installeren van DT 2,0 op een toegewezen VM vereist een netwerk doorvoer tussen de DT 2,0 VM en de SAP HANA VM van Mini maal 10 GB. Daarom is het verplicht om alle Vm's binnen hetzelfde Azure-Vnet te plaatsen en Azure versnelde netwerken in te scha kelen.

Meer informatie over versneld netwerken van Azure vindt u [hier](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)

### <a name="vm-storage-for-sap-hana-dt-20"></a>VM-opslag voor SAP HANA DT 2,0

Volgens de best practice richt lijnen voor DT 2,0 moet de i/o-door Voer van de schijf mini maal 50 MB/sec per fysieke kern zijn. Op de specificatie voor de twee typen Azure VM, die worden ondersteund voor DT 2,0, ziet de maximale doorvoer limiet voor de schijf-i/o voor de VM er als volgt uit:

- E32sv3:768 MB/sec. (niet in cache), wat betekent een verhouding van 48 MB/sec per fysieke kern
- M64-32MS: 1000 MB/sec. (niet in cache), wat betekent een verhouding van 62,5 MB/sec per fysieke kern

Het is verplicht om meerdere Azure-schijven aan de DT 2,0-VM te koppelen en een software matige RAID (striping) te maken op het niveau van het besturings systeem om de maximale limiet van de schijf doorvoer per virtuele machine te krijgen. Eén Azure-schijf kan de door Voer niet bieden om de maximale VM-limiet in dit opzicht te bereiken. Azure Premium Storage is verplicht om DT 2,0 uit te voeren. 

- Meer informatie over beschik bare typen Azure-schijven vindt u [hier](../../windows/disks-types.md)
- Meer informatie over het maken van software-RAID via mdadm vindt u [hier](https://docs.microsoft.com/azure/virtual-machines/linux/configure-raid)
- Meer informatie over het configureren van LVM voor het maken van een striped volume voor de maximale door Voer, vindt u [hier](https://docs.microsoft.com/azure/virtual-machines/linux/configure-lvm)

Afhankelijk van de grootte vereisten zijn er verschillende opties om de maximale door Voer van een virtuele machine te bereiken. Hier vindt u mogelijke configuraties van gegevens volume schijven voor elk DT 2,0 VM-type om de maximale doorvoer limiet van de VM te bereikt. De E32sv3-VM moet worden beschouwd als een invoer niveau voor kleinere werk belastingen. Als het niet snel genoeg is om te voor komen dat het nodig is om de grootte van de virtuele machine te wijzigen in M64-32MS.
Omdat de M64-32MS-VM veel geheugen heeft, kan de i/o-belasting de limiet voor het lezen van intensieve werk belastingen mogelijk niet bereiken. Daarom kunnen minder schijven in de Stripe-set voldoende zijn, afhankelijk van de specifieke werk belasting van de klant. Aan de kluis zijde worden de onderstaande schijf configuraties geselecteerd om de maximale door voer te garanderen:


| VM-SKU | Schijf configuratie 1 | Schijf configuratie 2 | Schijf configuratie 3 | Schijf configuratie 4 | Schijf configuratie 5 | 
| ---- | ---- | ---- | ---- | ---- | ---- | 
| M64-32ms | 4 x P50 -> 16 TB | 4 x P40 -> 8 TB | 5 x P30 -> 5 TB | 7 x P20 -> 3.5 TB | 8 x P15 -> 2 TB | 
| E32sv3 | 3 x P50 -> 12 TB | 3 x P40 -> 6 TB | 4 x P30 -> 4 TB | 5 x P20 -> 2.5 TB | 6 x P15 -> 1.5 TB | 


Met name als de werk belasting Lees-intensiever is, kan de IO-prestaties worden verhoogd om de alleen-lezen cache van Azure host in te scha kelen, zoals aanbevolen voor de gegevens volumes van database software. Dat voor het transactie logboek Azure host disk cache de waarde ' geen ' moet zijn. 

Met betrekking tot de grootte van het logboek volume een aanbevolen begin punt is een heuristiek van 15% van de gegevens grootte. Het maken van het logboek volume kan worden uitgevoerd met behulp van verschillende Azure-schijf typen, afhankelijk van de kosten-en doorvoer vereisten. Voor het logboek volume is de hoge I/O-door Voer vereist.  In het geval van het gebruik van het VM-type M64-32MS wordt het ten zeerste aanbevolen [Write Accelerator](https://docs.microsoft.com/azure/virtual-machines/linux/how-to-enable-write-accelerator)in te scha kelen. Azure Write Accelerator biedt optimale schrijf latentie voor de schijf voor het transactie logboek (alleen beschikbaar voor de M-serie). Er zijn enkele items waarmee u rekening moet houden, zoals het maximum aantal schijven per VM-type. [Hier](https://docs.microsoft.com/azure/virtual-machines/windows/how-to-enable-write-accelerator) vindt u meer informatie over write Accelerator.


Hier volgen enkele voor beelden van het formaat van het logboek volume:

| grootte van gegevens volume en schijf type | logboek volume en schijf type configuratie 1 | logboek volume en schijf type configuratie 2 |
| --- | --- | --- |
| 4 x P50 -> 16 TB | 5 x P20 -> 2.5 TB | 3 x P30 -> 3 TB |
| 6 x P15 -> 1.5 TB | 4 x P6 -> 256 GB | 1 x P15 -> 256 GB |


Net als bij SAP HANA scale-out moet de/Hana/shared-map worden gedeeld tussen de SAP HANA virtuele machine en de DT 2,0-VM. Dezelfde architectuur als voor SAP HANA uitschalen met behulp van toegewezen Vm's, die fungeren als een Maxi maal beschik bare NFS-server, wordt aanbevolen. Als u een gedeeld back-upvolume wilt opgeven, kunt u het identieke ontwerp gebruiken. Maar het is wel aan de klant als HA nood zakelijk is, of als het voldoende is voor het gebruik van een specifieke virtuele machine met voldoende opslag capaciteit om als back-upserver te fungeren.



### <a name="links-to-dt-20-documentation"></a>Koppelingen naar documentatie over DT 2,0 

- [Installatie-en update gids voor dynamische lagen SAP HANA](https://help.sap.com/viewer/88f82e0d010e4da1bc8963f18346f46e/2.0.03/en-US)
- [Zelf studies en bronnen voor dynamische lagen SAP HANA](https://help.sap.com/viewer/fb9c3779f9d1412b8de6dd0788fa167b/2.0.03/en-US)
- [Haalbaarheids test SAP HANA Dynamic-laag](https://blogs.sap.com/2017/12/08/sap-hana-dynamic-tiering-delivering-on-low-tco-with-impressive-performance/)
- [Uitbrei dingen voor dynamische lagen van SAP HANA 2,0 SPS 02](https://blogs.sap.com/2017/07/31/sap-hana-2.0-sps-02-dynamic-tiering-enhancements/)




## <a name="operations-for-deploying-sap-hana-on-azure-vms"></a>Bewerkingen voor het implementeren van SAP HANA op Azure-Vm's
In de volgende secties worden enkele bewerkingen beschreven die betrekking hebben op de implementatie van SAP HANA systemen op Azure-Vm's.

### <a name="back-up-and-restore-operations-on-azure-vms"></a>Back-up-en herstel bewerkingen op virtuele Azure-machines
De volgende documenten bevatten informatie over het maken van back-ups en het herstellen van uw SAP HANA-implementatie:

- [Overzicht van back-ups van SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-guide)
- [Back-up op bestands niveau SAP HANA](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-file-level)
- [Bench Mark-moment opname van SAP HANA opslag](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-backup-storage-snapshots)


### <a name="start-and-restart-vms-that-contain-sap-hana"></a>Vm's met SAP HANA starten en opnieuw opstarten
Een prominente functie van de open bare Azure-Cloud is dat u alleen voor uw computing minuten in rekening wordt gebracht. Wanneer u bijvoorbeeld een virtuele machine met SAP HANA afsluit, wordt u alleen gefactureerd voor de opslag kosten in die periode. Er is een andere functie beschikbaar wanneer u vaste IP-adressen voor uw virtuele machines opgeeft in de eerste implementatie. Wanneer u een VM met SAP HANA opnieuw opstart, wordt de VM opnieuw opgestart met de vorige IP-adressen. 


### <a name="use-saprouter-for-sap-remote-support"></a>SAProuter voor SAP-externe ondersteuning gebruiken
Als u een site-naar-site-verbinding hebt tussen uw on-premises locaties en Azure, en u SAP-onderdelen uitvoert, wordt waarschijnlijk al SAProuter uitgevoerd. In dit geval moet u de volgende items voor externe ondersteuning volt ooien:

- Onderhoud het privé-en statische IP-adres van de virtuele machine die als host fungeert voor SAP HANA in de SAProuter-configuratie.
- Configureer de NSG van het subnet dat als host fungeert voor de HANA-VM om verkeer toe te staan via TCP/IP-poort 3299.

Als u verbinding maakt met Azure via internet en u geen SAP-router voor de virtuele machine met SAP HANA hebt, moet u het onderdeel installeren. Installeer SAProuter in een afzonderlijke virtuele machine in het subnet van het beheer. In de volgende afbeelding ziet u een ruw schema voor het implementeren van SAP HANA zonder een site-naar-site-verbinding en met SAProuter:

![Ruwe implementatie schema voor SAP HANA zonder site-naar-site-verbinding en SAProuter](media/hana-vm-operations/hana-simple-networking3.PNG)

Zorg ervoor dat u SAProuter installeert op een afzonderlijke virtuele machine en niet in uw JumpBox-VM. De afzonderlijke virtuele machine moet een statisch IP-adres hebben. Als u uw SAProuter wilt verbinden met de SAProuter die wordt gehost door SAP, neemt u contact op met SAP voor een IP-adres. (De SAProuter die wordt gehost door SAP is het equivalent van het SAProuter-exemplaar dat u op uw virtuele machine installeert.) Gebruik het IP-adres van SAP om uw SAProuter-exemplaar te configureren. In de configuratie-instellingen is de enige vereiste poort TCP-poort 3299.

Raadpleeg de [SAP-documentatie](https://support.sap.com/en/tools/connectivity-tools/remote-support.html)voor meer informatie over het instellen en onderhouden van externe ondersteunings verbindingen via SAProuter.

### <a name="high-availability-with-sap-hana-on-azure-native-vms"></a>Hoge Beschik baarheid met SAP HANA op native Azure-Vm's
Als u SUSE Linux Enterprise Server voor SAP-toepassingen 12 SP1 of hoger gebruikt, kunt u een pacemaker-cluster tot stand brengen met STONITH-apparaten. U kunt de apparaten gebruiken om een SAP HANA configuratie in te stellen die gebruikmaakt van synchrone replicatie met HANA-systeem replicatie en automatische failover. Zie [SAP Hana-hand leiding voor maximale Beschik baarheid voor virtuele Azure-machines](https://docs.microsoft.com/azure/virtual-machines/workloads/sap/sap-hana-availability-overview)voor meer informatie over de installatie procedure.
