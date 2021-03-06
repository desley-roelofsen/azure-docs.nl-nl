---
title: Algemene vragen over de Azure Site Recovery-service
description: In dit artikel worden populaire algemene vragen over Azure Site Recovery beschreven.
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/14/2019
ms.author: raynew
ms.openlocfilehash: fb88d28ea47495dcbdb0844901a03ee7efa4e4eb
ms.sourcegitcommit: a22cb7e641c6187315f0c6de9eb3734895d31b9d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/14/2019
ms.locfileid: "74078527"
---
# <a name="general-questions-about-azure-site-recovery"></a>Algemene vragen over Azure Site Recovery

In dit artikel vindt u een overzicht van veelgestelde vragen over Azure Site Recovery. Raadpleeg deze artikelen voor specifieke scenario's

- [Vragen over herstel na nood gevallen voor Azure VM naar Azure](azure-to-azure-common-questions.md)
- [Vragen over de nood herstel van een VMware-VM naar Azure](vmware-azure-common-questions.md)
- [Vragen over herstel na nood gevallen voor Hyper-V-VM'S naar Azure](hyper-v-azure-common-questions.md)
 
## <a name="general"></a>Algemeen

### <a name="what-does-site-recovery-do"></a>Wat doet Site Recovery?
Site Recovery draagt bij aan uw strategie voor bedrijfs continuïteit en nood herstel (BCDR) door de replicatie van Azure-Vm's tussen regio's, on-premises virtuele machines en fysieke servers naar Azure en on-premises machines te organiseren en te automatiseren naar een secundair Data Center. [Meer informatie](site-recovery-overview.md).

### <a name="can-i-protect-a-virtual-machine-that-has-a-docker-disk"></a>Kan ik een virtuele machine beveiligen die een docker-schijf heeft?

Nee, dit is een niet-ondersteund scenario.

## <a name="service-providers"></a>Serviceproviders

### <a name="im-a-service-provider-does-site-recovery-work-for-dedicated-and-shared-infrastructure-models"></a>Ik ben een service provider. Werkt Site Recovery voor specifieke en gedeelde infrastructuur modellen?
Ja, Site Recovery werkt voor zowel toegewezen als gedeelde infrastructuurmodellen.

### <a name="for-a-service-provider-is-the-identity-of-my-tenant-shared-with-the-site-recovery-service"></a>Voor een service provider is de identiteit van mijn Tenant gedeeld met de Site Recovery-service?
Nee. De Tenant-id blijft anoniem. Uw tenants hebben geen toegang nodig tot de Site Recovery-portal. Alleen de beheerder van de serviceprovider communiceert met de portal.

### <a name="will-tenant-application-data-ever-go-to-azure"></a>Gaat de gegevens van de Tenant toepassing ooit naar Azure?
Bij replicatie tussen sites van serviceproviders worden er nooit toepassingsgegevens naar Azure overgezet. Gegevens worden in transit versleuteld en direct gerepliceerd tussen de sites van de service provider.

Als u naar Azure repliceert, worden er toepassingsgegevens verzonden naar Azure-opslag, maar niet naar de Site Recovery-service. Gegevens worden in transit versleuteld en blijven versleuteld in Azure.

### <a name="will-my-tenants-receive-a-bill-for-any-azure-services"></a>Ontvangen mijn tenants een factuur voor Azure-services?
Nee. Azure-services worden rechtstreeks aan de serviceprovider gefactureerd. Serviceproviders zijn zelf verantwoordelijk voor het genereren van facturen voor hun tenants.

### <a name="if-im-replicating-to-azure-do-we-need-to-run-virtual-machines-in-azure-at-all-times"></a>Als ik naar Azure repliceer, moet ik dan altijd een virtuele machine in Azure uitvoeren?
Nee, gegevens worden gerepliceerd naar Azure Storage in uw abonnement. Wanneer u een failovertest (details voor DR) of een werkelijke failover uitvoert, maakt Site Recovery automatisch virtuele machines in uw abonnement.

### <a name="do-you-ensure-tenant-level-isolation-when-i-replicate-to-azure"></a>Wordt er gezorgd voor isolatie op tenantniveau wanneer ik naar Azure repliceer?
Ja.

### <a name="what-platforms-do-you-currently-support"></a>Welke platforms worden er momenteel ondersteund?
We ondersteunen implementaties van Azure Pack, Cloud platform System en System Center op basis van (2012 en hoger). Meer [informatie](https://technet.microsoft.com/library/dn850370.aspx) over Azure Pack en integratie van site Recovery.

### <a name="do-you-support-single-azure-pack-and-single-vmm-server-deployments"></a>Worden implementaties van één Azure Pack en één VMM-server ondersteund?
Ja, u kunt virtuele Hyper-V-machines repliceren naar Azure of tussen sites van service providers.  Houd er rekening mee dat als u tussen sites van service providers repliceert Azure runbook-integratie niet beschikbaar is.

## <a name="pricing"></a>Prijzen

### <a name="where-can-i-find-pricing-information"></a>Waar vind ik prijs informatie?
Bekijk de [site Recovery prijs](https://azure.microsoft.com/pricing/details/site-recovery/) gegevens.


### <a name="how-can-i-calculate-approximate-charges-during-the-use-of-site-recovery"></a>Hoe kan ik geschatte kosten berekenen tijdens het gebruik van Site Recovery?

U kunt de [prijs calculator](https://aka.ms/asr_pricing_calculator) gebruiken om de kosten te ramen tijdens het gebruik van site Recovery.

Voor een gedetailleerde schatting van de kosten voert u het hulp programma Deployment planner voor [VMware](https://aka.ms/siterecovery_deployment_planner) of [Hyper-V](https://aka.ms/asr-deployment-planner)uit en gebruikt u het [rapport kosten raming](https://aka.ms/asr_DP_costreport).


### <a name="managed-disks-are-now-used-to-replicate-vmware-vms-and-physical-servers-do-i-incur-additional-charges-for-the-cache-storage-account-with-managed-disks"></a>Beheerde schijven worden nu gebruikt voor het repliceren van virtuele VMware-machines en fysieke servers. Worden er extra kosten in rekening gebracht voor het cache-opslag account met Managed disks?

Nee, er zijn geen extra kosten voor de cache. Wanneer u naar een standaard-opslag account repliceert, maakt deze cache-opslag deel uit van hetzelfde doel-opslag account.

### <a name="i-have-been-an-azure-site-recovery-user-for-over-a-month-do-i-still-get-the-first-31-days-free-for-every-protected-instance"></a>Ik ben al meer dan een maand gebruiker van Azure Site Recovery. Krijg ik nog steeds de eerste 31 dagen gratis voor elke beschermde instantie?

Ja. Voor elk beschermd exemplaar worden geen Azure Site Recovery-kosten in rekening gebracht tijdens de eerste 31 dagen. Als u bijvoorbeeld 10 instanties voor de afgelopen zes maanden hebt beveiligd en u een elfde exemplaar verbindt met Azure Site Recovery, zijn er gedurende de eerste 31 dagen geen kosten in rekening gebracht voor het elfde exemplaar. Voor de eerste tien exemplaren blijven Azure Site Recovery kosten in rekening gebracht omdat ze al meer dan 31 dagen zijn beveiligd.

### <a name="during-the-first-31-days-will-i-incur-any-other-azure-charges"></a>Worden er gedurende de eerste 31 dagen andere Azure-kosten in rekening gebracht?

Ja, hoewel Site Recovery gratis is gedurende de eerste 31 dagen van een beschermd exemplaar, worden er mogelijk kosten in rekening gebracht voor Azure Storage, opslag transacties en gegevens overdracht. Voor een herstelde virtuele machine worden mogelijk ook Azure-rekenkosten in rekening gebracht.


### <a name="is-there-a-cost-associated-to-perform-disaster-recovery-drillstest-failover"></a>Zijn er kosten verbonden aan het uitvoeren van nood herstel oefeningen/testfailover?

Er zijn geen afzonderlijke kosten voor DR-oefeningen. Er worden kosten berekend nadat de virtuele machine is gemaakt na de testfailover.



## <a name="security"></a>Beveiliging

### <a name="is-replication-data-sent-to-the-site-recovery-service"></a>Worden er replicatiegegevens verzonden naar de Site Recovery-service?
Nee, Site Recovery worden geen gerepliceerde gegevens onderschept en bevatten geen informatie over wat er op uw virtuele machines of fysieke servers wordt uitgevoerd.
Er worden alleen replicatiegegevens uitgewisseld tussen uw on-premises Hyper-V-hosts, VMware-hypervisors of fysieke servers en de Azure-opslag of uw secundaire site. Met Site Recovery kunnen deze gegevens niet worden onderschept. Alleen de metagegevens die nodig zijn om replicatie en failover te organiseren, worden naar de Site Recovery-service verzonden.  

Site Recovery is ISO 27001:2013, 27018, HIPAA, DPA gecertificeerd en is het proces van SOC2-en FedRAMP JAB-evaluaties.

### <a name="for-compliance-reasons-even-our-on-premises-metadata-must-remain-within-the-same-geographic-region-can-site-recovery-help-us"></a>Voor nalevings redenen moeten zelfs onze on-premises meta gegevens binnen dezelfde geografische regio blijven. Kan Site Recovery ons helpen?
Ja. Wanneer u een Site Recovery kluis maakt in een regio, zorgen we ervoor dat alle meta gegevens die we nodig hebben om de replicatie en failover in te scha kelen en te organiseren, binnen de geografische grens van die regio blijven.

### <a name="does-site-recovery-encrypt-replication"></a>Wordt replicatie met Site Recovery versleuteld?
Voor virtuele machines en fysieke servers moet replicatie tussen on-premises sites worden gerepliceerd-in-transit wordt ondersteund. Voor virtuele machines en fysieke servers die worden gerepliceerd naar Azure, wordt zowel versleuteling als door Voer [(in Azure)](https://docs.microsoft.com/azure/storage/storage-service-encryption) ondersteund.




## <a name="disaster-recovery"></a>Herstel na noodgeval

### <a name="what-can-site-recovery-protect"></a>Wat kan Site Recovery beschermen?
* **Azure vm's**: site Recovery kunt elke workload repliceren die wordt uitgevoerd op een ondersteunde Azure VM
* **Virtuele Hyper-v-machines**: site Recovery kunnen elke werk belasting die wordt uitgevoerd op een hyper-v-VM, beveiligen.
* **Fysieke servers**: site Recovery kunt fysieke servers met Windows of Linux beveiligen.
* **Virtuele VMware-machines**: site Recovery kunt elke werk belasting die wordt uitgevoerd in een VMware-VM, beveiligen.

### <a name="what-workloads-can-i-protect-with-site-recovery"></a>Welke workloads kan ik met Site Recovery beveiligen?
U kunt Site Recovery gebruiken om de meeste werk belastingen te beveiligen die worden uitgevoerd op een ondersteunde virtuele machine of op een fysieke server. Site Recovery biedt ondersteuning voor toepassings bewuste replicatie, zodat apps kunnen worden hersteld naar een intelligente status. Het wordt geïntegreerd met micro soft-toepassingen, zoals share point, Exchange, Dynamics, SQL Server en Active Directory, en werkt nauw samen met toonaangevende leveranciers, waaronder Oracle, SAP, IBM en Red Hat. Lees [hier](site-recovery-workload.md) meer informatie over workloadbeveiliging.

### <a name="can-i-manage-disaster-recovery-for-my-branch-offices-with-site-recovery"></a>Kan ik met Site Recovery herstel na noodgeval voor mijn filialen beheren?
Ja. Wanneer u Site Recovery gebruikt om replicatie en failover in uw filialen te organiseren, krijgt u een uniforme indeling en weer gave van al uw filialen op een centrale locatie. Vanuit het hoofdkantoor kunt u eenvoudig failovers uitvoeren en herstel na noodgeval beheren voor alle filialen. Het is niet nodig de afzonderlijke filialen te bezoeken.


### <a name="is-disaster-recovery-supported-for-azure-vms"></a>Wordt nood herstel ondersteund voor virtuele Azure-machines?

Ja, Site Recovery ondersteunt nood gevallen voor Azure-Vm's tussen Azure-regio's. [Bekijk veelgestelde vragen](azure-to-azure-common-questions.md) over nood herstel voor Azure VM.

### <a name="is-disaster-recovery-supported-for-vmware-vms"></a>Wordt nood herstel ondersteund voor virtuele VMware-machines?

Ja, Site Recovery ondersteunt herstel na nood gevallen van on-premises VMware-Vm's. [Bekijk veelgestelde vragen](vmware-azure-common-questions.md) over nood herstel van virtuele VMware-machines.

### <a name="is-disaster-recovery-supported-for-hyper-v-vms"></a>Wordt herstel na nood gevallen ondersteund voor virtuele Hyper-V-machines?
Ja, Site Recovery ondersteunt herstel na nood gevallen van on-premises Hyper-V-Vm's. [Bekijk veelgestelde vragen](hyper-v-azure-common-questions.md) voor herstel na nood gevallen voor virtuele Hyper-V-machines.

## <a name="is-disaster-recovery-supported-for-physical-servers"></a>Wordt nood herstel ondersteund voor fysieke servers?
Ja, Site Recovery ondersteunt herstel na nood gevallen van on-premises fysieke servers met Windows en Linux naar Azure of naar een secundaire site. Meer informatie over vereisten voor herstel na nood gevallen naar [Azure](vmware-physical-azure-support-matrix.md#replicated-machines)en naar[een secundaire site](vmware-physical-secondary-support-matrix.md#replicated-vm-support).
Houd er rekening mee dat fysieke servers na een failover worden uitgevoerd als virtuele machines in Azure. Failback van Azure naar een on-premises fysieke server wordt momenteel niet ondersteund. U kunt alleen een failback uitvoeren naar een virtuele VMware-machine.





## <a name="replication"></a>Replicatie

### <a name="can-i-replicate-over-a-site-to-site-vpn-to-azure"></a>Kan ik een site-naar-site-VPN-verbinding naar Azure repliceren?
Azure Site Recovery gegevens repliceren naar een Azure-opslag account of beheerde schijven, via een openbaar eind punt. Replicatie is niet meer dan een site-naar-site-VPN. 

### <a name="why-cant-i-replicate-over-vpn"></a>Waarom kan ik niet repliceren via VPN?

Wanneer u naar Azure repliceert, bereikt het replicatie verkeer de open bare eind punten van een Azure Storage. Daarom kunt u alleen repliceren via het open bare Internet met ExpressRoute (micro soft-peering of een bestaande open bare peering), en VPN werkt niet.

### <a name="can-i-use-riverbed-steelheads-for-replication"></a>Kan ik Riverbed SteelHeads gebruiken voor replicatie?

Onze partner, Riverbed, biedt gedetailleerde richt lijnen voor het werken met Azure Site Recovery. Bekijk de [hand leiding](https://community.riverbed.com/s/article/DOC-4627)voor de oplossing.

### <a name="can-i-use-expressroute-to-replicate-virtual-machines-to-azure"></a>Kan ik ExpressRoute gebruiken om virtuele machines te repliceren naar Azure?
Ja, [ExpressRoute kan worden gebruikt](concepts-expressroute-with-site-recovery.md) om on-premises virtuele machines te repliceren naar Azure.

- Azure Site Recovery repliceert gegevens naar een Azure Storage over een openbaar eind punt. U moet [micro soft-peering](../expressroute/expressroute-circuit-peerings.md#microsoftpeering) instellen of een bestaande [open bare peering](../expressroute/expressroute-circuit-peerings.md#publicpeering) (afgeschaft voor nieuwe circuits) gebruiken om ExpressRoute te gebruiken voor de replicatie van site Recovery.
- Micro soft-peering is het aanbevolen routerings domein voor replicatie.
- Replicatie wordt niet ondersteund voor persoonlijke peering.
- Als u VMware-machines of fysieke machines beveiligt, moet u ervoor zorgen dat ook aan de [netwerk vereisten](vmware-azure-configuration-server-requirements.md#network-requirements) voor de configuratie server wordt voldaan. De configuratie server voor de indeling van Site Recovery replicatie vereist dat er verbinding wordt met specifieke Url's. ExpressRoute kan niet worden gebruikt voor deze verbinding.
- Nadat de failover van de virtuele machines naar een virtueel Azure-netwerk is uitgevoerd, kunt u deze openen met behulp van de instelling voor [persoonlijke peering](../expressroute/expressroute-circuit-peerings.md#privatepeering) met het virtuele netwerk van Azure.


### <a name="if-i-replicate-to-azure-what-kind-of-storage-account-or-managed-disk-do-i-need"></a>Als ik naar Azure repliceer, wat voor soort opslag account of Managed Disk heb ik nodig?

U hebt een LRS-of GRS-opslag nodig. GRS wordt aanbevolen, omdat de gegevens dan flexibel zijn te gebruiken als er sprake is van regionale uitval of als de primaire regio niet kan worden hersteld. Het account moet zich in dezelfde regio bevinden als de Recovery Services-kluis. Premium-opslag wordt ondersteund voor virtuele VMware-machines, Hyper-V-machines en fysieke server replicatie wanneer u Site Recovery implementeert in de Azure Portal. Beheerde schijven bieden alleen ondersteuning voor LRS.

### <a name="how-often-can-i-replicate-data"></a>Hoe vaak kan ik gegevens repliceren?
* **Hyper-V:** Virtuele Hyper-V-machines kunnen elke 30 seconden worden gerepliceerd (met uitzonde ring van Premium-opslag), vijf minuten of 15 minuten.
* **Azure-vm's, virtuele VMware-machines, fysieke servers:** Een replicatie frequentie is hier niet relevant. Replicatie is doorlopend.

### <a name="can-i-extend-replication-from-existing-recovery-site-to-another-tertiary-site"></a>Kan ik replicatie van de bestaande herstel site naar een andere tertiaire site uitbreiden?
Uitgebreide of gekoppelde replicatie wordt niet ondersteund. Deze functie aanvragen in het [Feedback forum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6097959).

### <a name="can-i-do-an-offline-replication-the-first-time-i-replicate-to-azure"></a>Kan ik de eerste keer dat ik naar Azure repliceer, offline replicatie uitvoeren?
Nee, dit wordt niet ondersteund. Vraag deze functie aan in het [Feedback forum](https://feedback.azure.com/forums/256299-site-recovery/suggestions/6227386-support-for-offline-replication-data-transfer-from).

### <a name="can-i-exclude-specific-disks-from-replication"></a>Kan ik bepaalde schijven uitsluiten van replicatie?
Dit wordt ondersteund bij het repliceren van virtuele VMware-machines en virtuele Hyper-V-machines naar Azure, met behulp van de Azure Portal.

### <a name="can-i-replicate-virtual-machines-with-dynamic-disks"></a>Kan ik virtuele machines met dynamische schijven repliceren?
Dynamische schijven worden ondersteund bij het repliceren van virtuele Hyper-V-machines en bij het repliceren van VMware-Vm's en fysieke machines naar Azure. De schijf met het besturings systeem moet een standaard schijf zijn.


### <a name="can-i-throttle-bandwidth-allotted-for-replication-traffic"></a>Kan ik de band breedte beperken die is toegewezen aan replicatie verkeer?
Ja. Meer informatie over het beperken van de band breedte vindt u in deze artikelen:

* [Capaciteits planning voor het repliceren van virtuele VMware-machines en fysieke servers](site-recovery-plan-capacity-vmware.md)
* [Capaciteits planning voor het repliceren van virtuele Hyper-V-machines naar Azure](site-recovery-capacity-planning-for-hyper-v-replication.md)



## <a name="failover"></a>Failover
### <a name="if-im-failing-over-to-azure-how-do-i-access-the-azure-vms-after-failover"></a>Als ik een failover naar Azure krijg, krijg ik dan na een failover toegang tot de Azure-Vm's?

U kunt de virtuele Azure-machines benaderen via een beveiligde internetverbinding, via een VPN tussen sites of via Azure ExpressRoute. U moet een aantal dingen voorbereiden om verbinding te kunnen maken. [Meer informatie](site-recovery-test-failover-to-azure.md#prepare-to-connect-to-azure-vms-after-failover).


### <a name="if-i-fail-over-to-azure-how-does-azure-make-sure-my-data-is-resilient"></a>Als ik een failover naar Azure krijg, moet Azure ervoor zorgen dat mijn gegevens flexibel zijn?
Azure is ontworpen voor herstelbaarheid. Site Recovery is al ontworpen voor failover naar een secundair Azure-Data Center, conform de SLA van Azure. Als dit het geval is, zorgen we ervoor dat uw meta gegevens en kluizen binnen dezelfde geografische regio blijven die u voor uw kluis hebt gekozen.  

### <a name="if-im-replicating-between-two-datacenters-what-happens-if-my-primary-datacenter-experiences-an-unexpected-outage"></a>Als ik een replicatie tussen twee data centers vind? wat gebeurt er als mijn primaire Data Center een onverwachte onderbreking ondervindt?
U kunt een ongeplande failover vanuit de secundaire site activeren. Site Recovery heeft geen connectiviteit vanuit de primaire site nodig om de failover uit te voeren.

### <a name="is-failover-automatic"></a>Vindt failover automatisch plaats?
Failover wordt niet automatisch uitgevoerd. U initieert failovers met één klik in de portal of u kunt [site Recovery Power shell](/powershell/module/az.recoveryservices) gebruiken om een failover te activeren. Failback is een eenvoudige actie in de Site Recovery Portal.

Als u wilt automatiseren, kunt u de on-premises Orchestrator of Operations Manager gebruiken om een fout in de virtuele machine op te sporen en vervolgens de failover activeren met behulp van de SDK.

* [Meer](site-recovery-create-recovery-plans.md) informatie over herstel plannen.
* [Lees meer](site-recovery-failover.md) over failover.
* [Meer](site-recovery-failback-azure-to-vmware.md) informatie over het mislukken van back-ups van virtuele VMware-machines en fysieke servers

### <a name="if-my-on-premises-host-is-not-responding-or-crashed-can-i-fail-back-to-a-different-host"></a>Als mijn on-premises host niet reageert of vastloopt, kan ik dan een failback uitvoeren naar een andere host?
Ja, u kunt het herstel van de alternatieve locatie gebruiken om naar een andere host van Azure te failback.

* [Voor virtuele VMware-machines](concepts-types-of-failback.md#alternate-location-recovery-alr)
* [Voor virtuele Hyper-V-machines](hyper-v-azure-failback.md#perform-failback)

## <a name="automation"></a>Automation

### <a name="can-i-automate-site-recovery-scenarios-with-an-sdk"></a>Kan ik Site Recovery scenario's automatiseren met een SDK?
Ja. U kunt Site Recovery-werkstromen automatiseren met de Rest API-, PowerShell- of Azure-SDK. Momenteel ondersteunde scenario's voor de implementatie van Site Recovery met behulp van Power shell:

* [Virtuele Hyper-V-machines in VMMs-Clouds repliceren naar Azure PowerShell Resource Manager](hyper-v-vmm-powershell-resource-manager.md)
* [Virtuele Hyper-V-machines zonder VMM repliceren naar Azure PowerShell Resource Manager](hyper-v-azure-powershell-resource-manager.md)
* [VMware naar Azure repliceren met Power shell Resource Manager](vmware-azure-disaster-recovery-powershell.md)

## <a name="componentprovider-upgrade"></a>Upgrade van onderdeel/provider

### <a name="where-can-i-find-the-release-notesupdate-rollups-of-site-recovery-upgrades"></a>Waar vind ik de release opmerkingen/update pakketten van Site Recovery upgrades

[Meer](site-recovery-whats-new.md) informatie over nieuwe updates en het [ophalen van Rollup-informatie](service-updates-how-to.md).

## <a name="next-steps"></a>Volgende stappen
* Lees het [Site Recovery-overzicht](site-recovery-overview.md)

