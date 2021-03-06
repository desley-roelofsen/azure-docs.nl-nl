---
title: Architectuur van Azure naar Azure voor herstel na nood gevallen in Azure Site Recovery
description: Overzicht van de architectuur die wordt gebruikt bij het instellen van herstel na nood gevallen tussen Azure-regio's voor Azure-Vm's, met behulp van de Azure Site Recovery-service.
services: site-recovery
author: rayne-wiselman
manager: carmonm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 11/05/2019
ms.author: raynew
ms.openlocfilehash: e83c14e5ce337e8a3c4c119acc2397b98afd5b56
ms.sourcegitcommit: 6c2c97445f5d44c5b5974a5beb51a8733b0c2be7
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2019
ms.locfileid: "73621109"
---
# <a name="azure-to-azure-disaster-recovery-architecture"></a>Architectuur voor herstel na noodgevallen van Azure naar Azure


In dit artikel worden de architectuur, onderdelen en processen beschreven die worden gebruikt bij het implementeren van herstel na nood gevallen voor virtuele Azure-machines (Vm's) met behulp van de [Azure site Recovery](site-recovery-overview.md) -service. Als herstel na nood gevallen is ingesteld, repliceert Azure Vm's voortdurend van naar een andere doel regio. Als er een storing optreedt, kunt u een failover uitvoeren voor virtuele machines naar de secundaire regio en daar toegang toe krijgen. Wanneer alles normaal gesp roken opnieuw wordt uitgevoerd, kunt u een failback uitvoeren en op de primaire locatie blijven werken.



## <a name="architectural-components"></a>Architectuuronderdelen

De onderdelen die betrekking hebben op herstel na nood geval voor virtuele Azure-machines, worden in de volgende tabel samenvatten.

**Onderdeel** | **Vereisten**
--- | ---
**Vm's in bron regio** | Een of meer virtuele Azure-machines in een [ondersteunde bron regio](azure-to-azure-support-matrix.md#region-support).<br/><br/> Op Vm's kan elk [ondersteund besturings systeem](azure-to-azure-support-matrix.md#replicated-machine-operating-systems)worden uitgevoerd.
**Opslag van de bron-VM** | Virtuele Azure-machines kunnen worden beheerd of beschikken over niet-beheerde schijven over opslag accounts.<br/><br/>[Meer informatie over](azure-to-azure-support-matrix.md#replicated-machines---storage) ondersteunde Azure-opslag.
**VM-bron netwerken** | Vm's kunnen zich bevinden in een of meer subnetten in een virtueel netwerk (VNet) in de bron regio. Meer [informatie](azure-to-azure-support-matrix.md#replicated-machines---networking) over netwerk vereisten.
**Cache-opslag account** | U hebt een cache-opslag account in het bron netwerk nodig. Tijdens de replicatie worden wijzigingen in de virtuele machine opgeslagen in de cache voordat ze worden verzonden naar de doel opslag.  Cache-opslag accounts moeten standaard zijn.<br/><br/> Het gebruik van een cache zorgt voor een minimale impact op productie toepassingen die worden uitgevoerd op een virtuele machine.<br/><br/> Meer [informatie](azure-to-azure-support-matrix.md#cache-storage) over vereisten voor cache opslag. 
**Doel resources** | Doel bronnen worden gebruikt tijdens de replicatie en wanneer een failover optreedt. Site Recovery kunt de doel resource standaard instellen, maar u kunt ze ook maken/aanpassen.<br/><br/> Controleer in de doel regio of u Vm's kunt maken en of uw abonnement voldoende bronnen heeft ter ondersteuning van VM-grootten die in de doel regio nodig zijn. 

![Bron-en doel replicatie](./media/concepts-azure-to-azure-architecture/enable-replication-step-1.png)

## <a name="target-resources"></a>Doel resources

Wanneer u replicatie voor een virtuele machine inschakelt, geeft Site Recovery u de mogelijkheid om doel resources automatisch te maken. 

**Doel resource** | **Standaard instelling**
--- | ---
**Doel abonnement** | Hetzelfde als het bron abonnement.
**Doel resource groep** | De resource groep waartoe de virtuele machines behoren na een failover.<br/><br/> Het kan zich in een Azure-regio bevinden, met uitzonde ring van de bron regio.<br/><br/> Site Recovery maakt een nieuwe resource groep in de doel regio met het achtervoegsel ' ASR '.<br/><br/>
**Doel-VNet** | Het virtuele netwerk (VNet) waarin gerepliceerde Vm's zich na een failover bevinden. Er wordt een netwerk toewijzing gemaakt tussen de bron-en doel-virtuele netwerken en vice versa.<br/><br/> Site Recovery maakt een nieuw VNet en subnet met het achtervoegsel ' ASR '.
**Doel opslag account** |  Als de virtuele machine geen beheerde schijf gebruikt, is dit het opslag account waarnaar de gegevens worden gerepliceerd.<br/><br/> Site Recovery maakt een nieuw opslag account in de doel regio om het bron-opslag account te spie gelen.
**Beheerde replica schijven** | Als de virtuele machine gebruikmaakt van een beheerde schijf, is dit de beheerde schijven waarnaar gegevens worden gerepliceerd.<br/><br/> Site Recovery maakt met replica beheerde schijven in de opslag regio om de bron te spie gelen.
**Doel beschikbaarheids sets** |  De beschikbaarheidsset waarin het repliceren van Vm's zich na een failover bevindt.<br/><br/> Site Recovery maakt een beschikbaarheidsset in de doel regio met het achtervoegsel ' ASR ' voor virtuele machines die zich in een beschikbaarheidsset op de bron locatie bevinden. Als er een beschikbaarheidsset bestaat, wordt deze gebruikt en wordt er geen nieuwe beschikbaar gemaakt.
**Doel beschikbaarheids zones** | Als de doel regio beschikbaarheids zones ondersteunt, wordt in Site Recovery hetzelfde zone nummer toegewezen als in de bron regio.

### <a name="managing-target-resources"></a>Doel resources beheren

U kunt doel resources als volgt beheren:

- U kunt doel instellingen wijzigen terwijl u replicatie inschakelt.
- U kunt doel instellingen wijzigen nadat de replicatie al is uitgevoerd. De uitzonde ring is het beschikbaarheids type (één instantie, set of zone). Als u deze instelling wilt wijzigen, dient u replicatie uit te scha kelen, de instelling te wijzigen en vervolgens weer in te scha kelen.



## <a name="replication-policy"></a>Beleid voor replicatie 

Wanneer u Azure VM-replicatie inschakelt, Site Recovery standaard een nieuw replicatie beleid maken met de standaard instellingen in de tabel.

**Beleids instelling** | **Details** | **Standaard**
--- | --- | ---
**Bewaar periode van het herstel punt** | Hiermee geeft u op hoelang Site Recovery herstel punten bewaard | 24 uur
**Frequentie van de app-consistente moment opname** | Hoe vaak Site Recovery een app-consistente moment opname gebruikt. | Elke vier uur

### <a name="managing-replication-policies"></a>Replicatie beleid beheren

U kunt de standaard instellingen voor het replicatie beleid als volgt beheren en wijzigen:
- U kunt de instellingen wijzigen terwijl u replicatie inschakelt.
- U kunt op elk gewenst moment een replicatie beleid maken en dit vervolgens Toep assen wanneer u replicatie inschakelt.

### <a name="multi-vm-consistency"></a>Multi-VM-consistentie

Als u wilt dat Vm's samen worden gerepliceerd en gedeelde crash-consistente en toepassings consistente herstel punten op failover hebben, kunt u ze samen in een replicatie groep verzamelen. Multi-VM-consistentie heeft de prestaties van de werk belasting en mag alleen worden gebruikt voor Vm's met werk belastingen die consistentie op alle computers nodig hebben. 



## <a name="snapshots-and-recovery-points"></a>Moment opnamen en herstel punten

Herstel punten worden gemaakt op basis van moment opnamen van VM-schijven die op een bepaald moment worden uitgevoerd. Wanneer u een failover voor een virtuele machine doorvoert, gebruikt u een herstel punt om de virtuele machine op de doel locatie te herstellen.

Als er een failover wordt uitgevoerd, willen we doorgaans controleren of de VM begint zonder beschadiging of verlies van gegevens en dat de gegevens van de virtuele machine consistent zijn voor het besturings systeem en voor apps die worden uitgevoerd op de virtuele machine. Dit is afhankelijk van het type moment opnamen dat is gemaakt.

Site Recovery maakt moment opnamen als volgt:

1. Site Recovery maakt standaard consistente moment opnamen van gegevens en app-consistente moment opnamen als u een frequentie opgeeft.
2. Herstel punten worden gemaakt op basis van de moment opnamen en opgeslagen in overeenstemming met de Bewaar instellingen in het replicatie beleid.

### <a name="consistency"></a>Consistentie

In de volgende tabel worden verschillende soorten consistentie beschreven.

### <a name="crash-consistent"></a>Crash-consistent

**Beschrijving** | **Details** | **Advies**
--- | --- | ---
Een crash consistente moment opname legt gegevens vast die zich op de schijf bevonden toen de moment opname werd gemaakt. Het bevat niets in het geheugen.<br/><br/> Het bevat het equivalent van de gegevens op de schijf die aanwezig zouden zijn als de virtuele machine is vastgelopen of het netsnoer van de server is opgehaald op het moment dat de moment opname werd gemaakt.<br/><br/> Een crash consistent garandeert geen gegevens consistentie voor het besturings systeem of voor apps op de virtuele machine. | Met Site Recovery worden standaard crash consistente herstel punten in elke vijf minuten gemaakt. Deze instelling kan niet worden gewijzigd.<br/><br/>  | Vandaag kunnen de meeste apps goed worden hersteld met crash-consistente punten.<br/><br/> Crash-consistente herstel punten zijn doorgaans voldoende voor de replicatie van besturings systemen en apps, zoals DHCP-servers en afdruk servers.

### <a name="app-consistent"></a>App-consistent

**Beschrijving** | **Details** | **Advies**
--- | --- | ---
App-consistente herstel punten worden gemaakt op basis van app-consistente moment opnamen.<br/><br/> Een app-consistente moment opname bevat alle informatie in een crash consistente moment opname, plus alle gegevens in het geheugen en trans acties die worden uitgevoerd. | App-consistente moment opnamen gebruiken de Volume Shadow Copy Service (VSS):<br/><br/>   1) wanneer een moment opname wordt gestart, voert VSS een Kopieer bewerking voor het schrijven van kopieën uit op het volume.<br/><br/>   2) voordat de koeien wordt uitgevoerd, informeert VSS elke app op de computer die de geheugenresidente gegevens op schijf moet leegmaken.<br/><br/>   3) vervolgens kan de app back-up/herstel na nood gevallen (in dit geval Site Recovery) de momentopname gegevens lezen en door gaan. | App-consistente moment opnamen worden gemaakt op basis van de frequentie die u opgeeft. Deze frequentie moet altijd kleiner zijn dan de instelling voor het bewaren van herstel punten. Als u bijvoorbeeld de herstel punten behoudt met de standaard instelling van 24 uur, stelt u de frequentie in op minder dan 24 uur.<br/><br/>Ze zijn ingewik kelder en nemen meer tijd in beslag dan crash-consistente moment opnamen.<br/><br/> Ze zijn van invloed op de prestaties van apps die worden uitgevoerd op een virtuele machine die is ingeschakeld voor replicatie. 

## <a name="replication-process"></a>Replicatieproces

Wanneer u replicatie voor een virtuele machine van Azure inschakelt, gebeurt het volgende:

1. De extensie van de Site Recovery Mobility-service wordt automatisch geïnstalleerd op de VM.
2. De extensie registreert de virtuele machine met Site Recovery.
3. Doorlopende replicatie begint voor de virtuele machine.  Schijf schrijf bewerkingen worden onmiddellijk overgebracht naar het cache-opslag account in de bron locatie.
4. Site Recovery worden de gegevens in de cache verwerkt, verzonden naar het doel-opslag account of naar de replica Managed disks.
5. Nadat de gegevens zijn verwerkt, worden er om de vijf minuten crash consistente herstel punten gegenereerd. App-consistente herstel punten worden gegenereerd volgens de instelling die is opgegeven in het replicatie beleid.

![Replicatie proces inschakelen, stap 2](./media/concepts-azure-to-azure-architecture/enable-replication-step-2.png)

**Replicatie proces**

## <a name="connectivity-requirements"></a>Connectiviteits vereisten

 De virtuele Azure-machines die u repliceert, hebben uitgaande verbindingen nodig. Site Recovery hebt nooit binnenkomende verbindingen met de virtuele machine nodig. 

### <a name="outbound-connectivity-urls"></a>Uitgaande connectiviteit (Url's)

Als uitgaande toegang voor virtuele machines wordt beheerd met Url's, kunt u deze Url's toestaan.

| **URL** | **Details** |
| ------- | ----------- |
| *.blob.core.windows.net | Hiermee kunnen gegevens van de VM naar het cache-opslagaccount in de bronregio worden geschreven. |
| login.microsoftonline.com | Verzorgt autorisatie en authenticatie voor de URL’s van Site Recovery. |
| *.hypervrecoverymanager.windowsazure.com | Maakt het de VM mogelijk te communiceren met de Site Recovery-service. |
| *.servicebus.windows.net | Maakt het de VM mogelijk bewakings- en diagnosegegevens van Site Recovery te schrijven. |

### <a name="outbound-connectivity-for-ip-address-ranges"></a>Uitgaande connectiviteit voor IP-adresbereiken

Als u de uitgaande connectiviteit voor Vm's met IP-adressen wilt beheren, moet u deze adressen toestaan.
Details van de vereisten voor netwerk connectiviteit vindt u in het [technisch document van netwerken](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) 

#### <a name="source-region-rules"></a>Regels voor bron regio's

**Budgetoverboekingsregel** |  **Details** | **Servicetag**
--- | --- | --- 
HTTPS-uitgaand toestaan: poort 443 | Bereiken toestaan die overeenkomen met opslag accounts in de bron regio | Opslagpad.\<regio-naam >.
HTTPS-uitgaand toestaan: poort 443 | Bereiken toestaan die overeenkomen met Azure Active Directory (Azure AD).<br/><br/> Als Azure AD-adressen in de toekomst worden toegevoegd, moet u nieuwe NSG-regels (netwerk beveiligings groep) maken.  | AzureActiveDirectory
HTTPS-uitgaand toestaan: poort 443 | Toegang tot [site Recovery eind punten](https://aka.ms/site-recovery-public-ips) toestaan die overeenkomen met de doel locatie. 

#### <a name="target-region-rules"></a>Doel regio regels

**Budgetoverboekingsregel** |  **Details** | **Servicetag**
--- | --- | --- 
HTTPS-uitgaand toestaan: poort 443 | Sta bereiken toe die overeenkomen met opslag accounts in de doel regio. | Opslagpad.\<regio-naam >.
HTTPS-uitgaand toestaan: poort 443 | Bereiken toestaan die overeenkomen met Azure AD.<br/><br/> Als Azure AD-adressen in de toekomst worden toegevoegd, moet u nieuwe NSG-regels maken.  | AzureActiveDirectory
HTTPS-uitgaand toestaan: poort 443 | Toegang tot [site Recovery eind punten](https://aka.ms/site-recovery-public-ips) toestaan die overeenkomen met de bron locatie. 


#### <a name="control-access-with-nsg-rules"></a>Toegang beheren met NSG-regels

Als u de connectiviteit van de virtuele machine beheert door netwerk verkeer van en naar Azure-netwerken/-subnetten te filteren met [NSG-regels](https://docs.microsoft.com/azure/virtual-network/security-overview), moet u rekening houden met de volgende vereisten:

- NSG-regels voor de Azure-bron regio moeten uitgaande toegang toestaan voor replicatie verkeer.
- We raden u aan regels te maken in een test omgeving voordat u ze in productie neemt.
- Gebruik [service Tags](https://docs.microsoft.com/azure/virtual-network/security-overview#service-tags) in plaats van afzonderlijke IP-adressen toe te staan.
    - Service Tags vertegenwoordigen een groep IP-adres voorvoegsels die samen worden verzameld om de complexiteit bij het maken van beveiligings regels te minimaliseren.
    - Micro soft werkt automatisch service tags na verloop van tijd bij. 
 
Meer informatie over [uitgaande connectiviteit](azure-to-azure-about-networking.md#outbound-connectivity-for-ip-address-ranges) voor site Recovery en het [beheren van connectiviteit met nsg's](concepts-network-security-group-with-site-recovery.md).


### <a name="connectivity-for-multi-vm-consistency"></a>Connectiviteit voor multi-VM-consistentie

Als u multi-VM-consistentie inschakelt, communiceren machines in de replicatiegroep met elkaar via poort 20004.
- Zorg ervoor dat de interne communicatie tussen de VM's via poort 20004 niet door een firewall-apparaat wordt geblokkeerd.
- Als u wilt dat Linux VM’s deel uitmaken van een replicatiegroep, zorg er dan voor dat het uitgaande verkeer op poort 20004 handmatig wordt geopend volgens de richtlijnen van de specifieke Linux-versie.




## <a name="failover-process"></a>Failoverproces

Wanneer u een failover initieert, worden de virtuele machines gemaakt in de doel resource groep, het virtuele doel netwerk, het doel-subnet en de beschikbaarheidsset doel. Tijdens een failover kunt u elk herstel punt gebruiken.

![Failoverproces](./media/concepts-azure-to-azure-architecture/failover.png)

## <a name="next-steps"></a>Volgende stappen

Een Azure-VM [snel repliceren](azure-to-azure-quickstart.md) naar een secundaire regio.
