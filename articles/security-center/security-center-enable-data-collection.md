---
title: Gegevens verzameling in Azure Security Center | Microsoft Docs
description: In dit artikel wordt beschreven hoe u een Log Analytics Agent installeert en hoe u een Log Analytics werk ruimte instelt waarin de verzamelde gegevens worden opgeslagen.
services: security-center
author: memildin
manager: rkarlin
ms.service: security-center
ms.topic: conceptual
ms.date: 09/10/2019
ms.author: memildin
ms.openlocfilehash: 795661912633f0d225aef4de8ea7620a8766e096
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/03/2019
ms.locfileid: "74766987"
---
# <a name="data-collection-in-azure-security-center"></a>Gegevens verzameling in Azure Security Center
Security Center verzamelt gegevens van uw virtuele machines van Azure (Vm's), schaal sets voor virtuele machines, IaaS containers en niet-Azure-computers (inclusief on-premises) om te controleren op beveiligings problemen en bedreigingen. Gegevens worden verzameld met behulp van de Log Analytics-agent, die verschillende aan beveiliging gerelateerde configuraties en gebeurtenis logboeken van de computer leest en de gegevens naar uw werk ruimte kopieert voor analyse. Voor beelden van dergelijke gegevens zijn: besturingssysteem type en-versie, logboeken van besturings systemen (Windows-gebeurtenis Logboeken), actieve processen, computer naam, IP-adressen en aangemelde gebruiker. De Log Analytics-agent kopieert ook crash dump bestanden naar uw werk ruimte.

Gegevens verzameling is vereist om inzicht te krijgen in ontbrekende updates, onjuiste geconfigureerde beveiligings instellingen voor het besturings systeem, status van Endpoint Protection en detectie van status en bedreigingen. 

In dit artikel wordt beschreven hoe u een Log Analytics Agent installeert en hoe u een Log Analytics werk ruimte instelt waarin de verzamelde gegevens worden opgeslagen. Beide bewerkingen zijn vereist voor het inschakelen van gegevens verzameling. 

> [!NOTE]
> - Het verzamelen van gegevens is alleen nodig voor reken bronnen (Vm's, schaal sets voor virtuele machines, IaaS containers en niet-Azure-computers). U kunt profiteren van Azure Security Center, zelfs als u geen agents inricht. u hebt echter beperkte beveiliging en de mogelijkheden die hierboven worden vermeld, worden niet ondersteund.  
> - Zie [ondersteunde platforms in azure Security Center](security-center-os-coverage.md)voor een lijst met ondersteunde platforms.
> - Het opslaan van gegevens in Log Analytics, ongeacht of u een nieuwe of bestaande werk ruimte gebruikt, kan er extra kosten in rekening worden gebracht voor gegevens opslag. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

## Automatische inrichting van de Log Analytics-agent inschakelen<a name="auto-provision-mma"></a>

Als u de gegevens van de computers wilt verzamelen, moet de Log Analytics-agent zijn geïnstalleerd. De installatie van de agent kan automatisch worden uitgevoerd (aanbevolen) of u kunt de agent hand matig installeren.  

>[!NOTE]
> Automatische inrichting is standaard uitgeschakeld. Als u Security Center wilt instellen op het standaard installeren van automatische inrichting, stelt u deze **in op aan**.
>

Als automatische inrichting is ingeschakeld, wordt Security Center de Log Analytics agent ingericht op alle ondersteunde Azure-Vm's en eventuele nieuwe virtuele machines die worden gemaakt. Automatische inrichting wordt sterk aanbevolen, maar er is ook hand matige installatie van de agent beschikbaar. [Meer informatie over het installeren van de log Analytics agent-extensie](#manual-agent).



Automatische inrichting van de Log Analytics-agent inschakelen:
1. Selecteer in het hoofd menu van Security Center **prijzen & instellingen**.
2. Klik op het betreffende abonnement

   ![Abonnement selecteren][7]

3. Selecteer **gegevens verzameling**.
4. Onder **automatische inrichting**selecteert u aan om automatische **inrichting in te** scha kelen.
5. Selecteer **Opslaan**.

   ![Automatische inrichting inschakelen][1]

>[!NOTE]
> - Zie voor instructies over het inrichten van een vooraf bestaande installatie [automatische inrichting in het geval van een bestaande Agent installatie](#preexisting).
> - Zie [de log Analytics agent-extensie hand matig installeren](#manual-agent)voor instructies over het hand matig inrichten.
> - Zie [automatische inrichting uitschakelen](#offprovisioning)voor instructies over het uitschakelen van automatische inrichting.
> - Zie voor instructies over het onboarden van Security Center met behulp van Power shell de Azure Security Center voor bereiding op het [uitvoeren van](security-center-powershell-onboarding.md)een onboarding.
>

## <a name="workspace-configuration"></a>Werkruimte configuratie
Gegevens die worden verzameld door Security Center worden opgeslagen in een of meer werkruimten van Log Analytics. U kunt ervoor kiezen om gegevens te verzamelen die zijn verzameld uit virtuele Azure-machines die zijn opgeslagen in werk ruimten die zijn gemaakt door Security Center of in een bestaande werk ruimte die u hebt gemaakt. 

Werkruimte configuratie is ingesteld per abonnement en veel abonnementen kunnen dezelfde werk ruimte gebruiken.

### <a name="using-a-workspace-created-by-security-center"></a>Een werk ruimte gebruiken die is gemaakt door Security Center

Security Center kan automatisch een standaard werkruimte maken waarin de gegevens worden opgeslagen. 

Een werk ruimte selecteren die is gemaakt door Security Center:

1. Onder **standaard werkruimte configuratie**selecteert u werk ruimte (s) gebruiken die zijn gemaakt door Security Center.
   ![prijs categorie selecteren][10] 

1. Klik op **Opslaan**.<br>
    Security Center maakt een nieuwe resource groep en standaardwerk ruimte in die geolocatie en verbindt de agent met die werk ruimte. De naam Conventie voor de werk ruimte en de resource groep is:<br>
   **Werk ruimte: DefaultWorkspace-[abonnement-ID]-[geo]<br> resource groep: DefaultResourceGroup-[geo]**

   Als een abonnement Vm's uit meerdere geolocaties bevat, worden Security Center meerdere werk ruimten gemaakt. Er zijn meerdere werk ruimten gemaakt voor het onderhouden van regels voor gegevens bescherming.
1. Security Center wordt automatisch een Security Center oplossing op de werk ruimte ingeschakeld per de prijs categorie die voor het abonnement is ingesteld. 

> [!NOTE]
> De Log Analytics prijs categorie van werk ruimten die zijn gemaakt door Security Center heeft geen invloed op Security Center facturering. De facturering voor Security Center is altijd gebaseerd op uw Security Center-beveiligingsbeleid en de oplossingen die in een werkruimte zijn geïnstalleerd. Voor de gratis versie schakelt Azure Security Center de oplossing *SecurityCenterFree* in voor de standaardwerkruimte. Voor de standaardversie schakelt Azure Security Center de oplossing *Security* in voor de standaardwerkruimte.
> Bij het opslaan van gegevens in Log Analytics worden mogelijk extra kosten in rekening gebracht voor gegevens opslag. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

Zie voor meer informatie over bestaande log Analytics-accounts [bestaande log Analytics-klanten](security-center-faq.md#existingloganalyticscust).

### <a name="using-an-existing-workspace"></a>Een bestaande werk ruimte gebruiken

Als u al een bestaande Log Analytics-werk ruimte hebt, wilt u mogelijk dezelfde werk ruimte gebruiken.

Als u uw bestaande Log Analytics-werk ruimte wilt gebruiken, moet u lees-en schrijf machtigingen hebben voor de werk ruimte.

> [!NOTE]
> Oplossingen die zijn ingeschakeld in de bestaande werk ruimte worden toegepast op virtuele Azure-machines die ermee zijn verbonden. Voor betaalde oplossingen kan dit leiden tot extra kosten. Zorg ervoor dat de geselecteerde werk ruimte zich in de juiste geografische regio bevindt.
> Bij het opslaan van gegevens in log Analytics worden mogelijk extra kosten in rekening gebracht voor gegevens opslag. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.

Een bestaande Log Analytics-werk ruimte selecteren:

1. Onder **standaard werkruimte configuratie**selecteert **u een andere werk ruimte gebruiken**.

   ![Bestaande werk ruimte selecteren][2]

2. Selecteer in de vervolg keuzelijst een werk ruimte om de verzamelde gegevens op te slaan.

   > [!NOTE]
   > In het vervolg keuzemenu zijn alle werk ruimten beschikbaar voor al uw abonnementen. Zie de selectie van de [werk ruimte](security-center-enable-data-collection.md#cross-subscription-workspace-selection) voor meerdere abonnementen voor meer informatie. U moet gemachtigd zijn om toegang te krijgen tot de werk ruimte.
   >
   >

3. Selecteer **Opslaan**.
4. Nadat u **Opslaan**hebt geselecteerd, wordt u gevraagd of u de bewaakte vm's die eerder zijn verbonden met een standaardwerk ruimte, opnieuw wilt configureren.

   - Selecteer **Nee** als u de nieuwe werkruimte instellingen alleen op nieuwe vm's wilt Toep assen. De nieuwe werk ruimte-instellingen zijn alleen van toepassing op nieuwe agent installaties. nieuw gedetecteerde Vm's waarop de Log Analytics-agent niet is geïnstalleerd.
   - Selecteer **Ja** als u de nieuwe werkruimte instellingen wilt Toep assen op alle vm's. Daarnaast wordt elke VM die is verbonden met een Security Center gemaakte werk ruimte opnieuw verbonden met de nieuwe doel werkruimte.

   > [!NOTE]
   > Als u Ja selecteert, moet u de werk ruimte (s) die zijn gemaakt door Security Center, pas verwijderen als alle virtuele machines opnieuw zijn verbonden met de nieuwe doel werkruimte. Deze bewerking mislukt als er te vroeg een werk ruimte wordt verwijderd.
   >
   >

   - Selecteer **Annuleren** om de bewerking te annuleren.

     ![Bestaande werk ruimte selecteren][3]

5. Selecteer de prijs categorie voor de gewenste werk ruimte die u wilt instellen voor de Log Analytics agent. <br>Stel de prijs categorie voor de werk ruimte in om een bestaande werk ruimte te gebruiken. Hiermee wordt een Security Center-oplossing geïnstalleerd op de werk ruimte als deze nog niet aanwezig is.

    a.  Selecteer in het hoofd menu van Security Center **prijzen & instellingen**.
     
    b.  Selecteer de gewenste werk ruimte waarin u de agent wilt verbinden.
        ![Selecteer werk ruimte][7] c. Stel de prijs categorie in.
        ![prijs categorie selecteren][9]
   
   >[!NOTE]
   >Als er al een **beveiligings** -of **SecurityCenterFree** -oplossing is ingeschakeld voor de werk ruimte, wordt de prijs automatisch ingesteld. 

## <a name="cross-subscription-workspace-selection"></a>Selectie van werk ruimte voor meerdere abonnementen
Wanneer u een werk ruimte selecteert waarin u uw gegevens wilt opslaan, zijn alle werk ruimten beschikbaar in al uw abonnementen. Het voordeel hiervan is dat u gegevens kunt verzamelen van virtuele machines die in verschillende abonnementen worden uitgevoerd en deze kunt opslaan in de werkruimte van uw keuze. Deze optie is handig als u een centrale werkruimte gebruikt in uw organisatie en u deze ook wilt gebruiken voor het verzamelen van beveiligingsgegevens. Zie [toegang tot werk ruimten beheren](https://docs.microsoft.com/azure/log-analytics/log-analytics-manage-access)voor meer informatie over het beheren van werk ruimten.


## <a name="data-collection-tier"></a>Gegevensverzamelings niveau
Het selecteren van een laag voor gegevensverzameling in Azure Security Center heeft alleen invloed op de opslag van beveiligingsgebeurtenissen in de Log Analytics-werkruimte. De Log Analytics-agent verzamelt en analyseert nog steeds de beveiligings gebeurtenissen die vereist zijn voor de bedreigings detecties van de Azure Security Center, ongeacht welke laag beveiligings gebeurtenissen u in uw Log Analytics-werk ruimte wilt opslaan (indien van toepassing). Als u ervoor kiest om beveiligingsgebeurtenissen op te slaan in uw werkruimte, kunt u gebeurtenissen vanuit die werkruimte onderzoeken, zoeken en controleren. 
> [!NOTE]
> Bij het opslaan van gegevens in log Analytics worden mogelijk extra kosten in rekening gebracht voor gegevens opslag. Zie de pagina [prijzen](https://azure.microsoft.com/pricing/details/security-center/) voor meer informatie.
> 
> U kunt het juiste filter beleid kiezen voor uw abonnementen en werk ruimten van vier sets gebeurtenissen die moeten worden opgeslagen in uw werk ruimte: 

- **Geen** : opslag voor beveiligings gebeurtenissen uitschakelen. Dit is de standaardinstelling.
- **Mini maal** : een kleiner aantal gebeurtenissen voor klanten die het gebeurtenis volume willen minimaliseren.
- **Algemeen** : dit is een set gebeurtenissen die aan de meeste klanten voldoet, en waarmee een volledige audittrail kan worden gevolgd.
- **Alle gebeurtenissen** : voor klanten die willen controleren of alle gebeurtenissen zijn opgeslagen.


> [!NOTE]
> Deze beveiligings gebeurtenissen sets zijn alleen beschikbaar in de Standard-laag van Security Center. Bekijk de pagina [Prijzen](security-center-pricing.md) voor meer informatie over de tariefopties van Security Center.
Deze sets zijn ontworpen om typische scenario's te verhelpen. Controleer welk abonnement aan uw behoeften voldoet voordat u deze implementeert.
>
>

Voor het bepalen van de gebeurtenissen die deel uitmaken van de **gemeen schappelijke** en **minimale** gebeurtenis sets, hebben we met klanten en industrie normen gewerkt om meer te weten te komen over de niet-gefilterde frequentie van elke gebeurtenis en hun gebruik. We hebben de volgende richt lijnen in dit proces gebruikt:

- **Mini maal** : Zorg ervoor dat deze set alleen gebeurtenissen omvat die kunnen wijzen op een geslaagde schending en belang rijke gebeurtenissen met een zeer laag volume. Deze set bevat bijvoorbeeld de gebruiker geslaagde en mislukte aanmeldingen (gebeurtenis-Id's 4624, 4625), maar bevat geen afmelding die belang rijk is voor controle, maar niet zinvol voor detectie en relatief hoog volume. Het meren deel van het gegevens volume van deze set is de aanmeldings gebeurtenissen en de gebeurtenis voor het maken van een proces (gebeurtenis-ID 4688).
- **Algemeen** : Geef een volledige controle spoor voor gebruikers op in deze set. Deze set bevat bijvoorbeeld zowel gebruikers aanmeldingen als gebruikers afmeldingen (gebeurtenis-ID 4634). Er zijn controle acties zoals wijzigingen in de beveiligings groep, Kerberos-bewerkingen van de Key-domein controller en andere gebeurtenissen die worden aanbevolen door organisaties van de branche.

Gebeurtenissen met een zeer laag volume zijn opgenomen in de gemeen schappelijke set als de belangrijkste motivatie om deze te kiezen voor alle gebeurtenissen om het volume te verminderen en om specifieke gebeurtenissen niet uit te filteren.

Hier volgt een volledige uitsplitsing van de gebeurtenis-Id's van de beveiligings-en app-kluis voor elke set:

| Gegevenslaag | Verzamelde gebeurtenis indicatoren |
| --- | --- |
| Minimale | 1102, 4624, 4625, 4657, 4663, 4688, 4700, 4702, 4719, 4720, 4722, 4723, 4724, 4727, 4728, 4732, 4735, 4737, 4739, 4740, 4754, 4755, |
| | 4756, 4767, 4799, 4825, 4946, 4948, 4956, 5024, 5033, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222 |
| Algemeen | 1, 299, 300, 324, 340, 403, 404, 410, 411, 412, 413, 431, 500, 501, 1100, 1102, 1107, 1108, 4608, 4610, 4611, 4614, 4622, |
| |  4624, 4625, 4634, 4647, 4648, 4649, 4657, 4661, 4662, 4663, 4665, 4666, 4667, 4688, 4670, 4672, 4673, 4674, 4675, 4689, 4697, |
| | 4700, 4702, 4704, 4705, 4716, 4717, 4718, 4719, 4720, 4722, 4723, 4724, 4725, 4726, 4727, 4728, 4729, 4733, 4732, 4735, 4737, |
| | 4738, 4739, 4740, 4742, 4744, 4745, 4746, 4750, 4751, 4752, 4754, 4755, 4756, 4757, 4760, 4761, 4762, 4764, 4767, 4768, 4771, |
| | 4774, 4778, 4779, 4781, 4793, 4797, 4798, 4799, 4800, 4801, 4802, 4803, 4825, 4826, 4870, 4886, 4887, 4888, 4893, 4898, 4902, |
| | 4904, 4905, 4907, 4931, 4932, 4933, 4946, 4948, 4956, 4985, 5024, 5033, 5059, 5136, 5137, 5140, 5145, 5632, 6144, 6145, 6272, |
| | 6273, 6278, 6416, 6423, 6424, 8001, 8002, 8003, 8004, 8005, 8006, 8007, 8222, 26401, 30004 |

> [!NOTE]
> - Als u gebruikmaakt van groepsbeleid object (GPO), is het raadzaam om het proces voor het maken van controle beleid-gebeurtenis 4688 en het veld *commandline* binnen gebeurtenis 4688 in te scha kelen. Zie [Veelgestelde vragen](security-center-faq.md#what-happens-when-data-collection-is-enabled)over de Security Center voor meer informatie over het maken van processen met de gebeurtenis 4688. Zie [aanbevelingen voor controle beleid](https://docs.microsoft.com/windows-server/identity/ad-ds/plan/security-best-practices/audit-policy-recommendations)voor meer informatie over deze controle beleidsregels.
> -  Om het verzamelen van gegevens voor [adaptieve toepassings besturings elementen](security-center-adaptive-application.md)in te scha kelen, configureert Security Center een lokaal AppLocker-beleid in de controle modus om alle toepassingen toe te staan. Dit zorgt ervoor dat AppLocker gebeurtenissen genereert die vervolgens door Security Center worden verzameld en gebruikt. Het is belang rijk te weten dat dit beleid niet wordt geconfigureerd op computers waarop al een geconfigureerd AppLocker-beleid is ingesteld. 
> - Als u [gebeurtenis-ID 5156](https://www.ultimatewindowssecurity.com/securitylog/encyclopedia/event.aspx?eventID=5156)voor het Windows-filter platform wilt verzamelen, moet u de verbinding voor het [controleren van filter platform](https://docs.microsoft.com/windows/security/threat-protection/auditing/audit-filtering-platform-connection) inschakelen (auditpol/set subcategory stellen: "filter platform Connection"/success: Enable)
>

Het filter beleid kiezen:
1. Selecteer op de pagina **gegevens verzameling** het filter beleid onder **beveiligings gebeurtenissen**.
2. Selecteer **Opslaan**.

   ![Filter beleid kiezen][5]

### Automatische inrichting in het geval van een reeds bestaande Agent installatie<a name="preexisting"></a> 

In de volgende use-cases wordt aangegeven hoe automatische inrichting werkt in gevallen waarin er al een agent of uitbrei ding is geïnstalleerd. 

- Log Analytics agent is geïnstalleerd op de computer, maar niet als een uitbrei ding (direct agent)<br>
Als de Log Analytics-agent rechtstreeks op de virtuele machine is geïnstalleerd (niet als een Azure-extensie), wordt Security Center de Log Analytics agent-extensie geïnstalleerd en kan de Log Analytics agent worden bijgewerkt naar de nieuwste versie.
De agent die is geïnstalleerd, blijft rapporteren aan de al geconfigureerde werk ruimte (n) en rapporteert bovendien aan de werk ruimte die is geconfigureerd in Security Center (multi-multihoming wordt ondersteund op Windows-computers).
Als de geconfigureerde werk ruimte een gebruikers werkruimte is (niet Security Center de standaard werkruimte), moet u de oplossing ' Security/securityFree ' installeren voor Security Center om te beginnen met het verwerken van gebeurtenissen van Vm's en computers die aan die werk ruimte rapporteren.<br>
<br>
Voor Linux-machines wordt agent-multi-multihoming nog niet ondersteund. als er een bestaande agent wordt gedetecteerd, treedt er geen automatische inrichting op en wordt de configuratie van de computer niet gewijzigd.
<br>
Voor bestaande machines op abonnementen die voor Security Center vóór 2019-03-17 worden uitgevoerd, wordt de uitbrei ding van de Log Analytics agent niet geïnstalleerd en wordt de computer niet beïnvloed als er een bestaande agent wordt gedetecteerd. Zie voor deze computers de aanbeveling ' bewakings agent status problemen oplossen op uw computers ' om de installatie problemen van de agent op deze computers op te lossen.

  
- System Center Operations Manager agent is geïnstalleerd op de computer<br>
In Security Center wordt de Log Analytics agent-extensie naast elkaar geïnstalleerd op de bestaande Operations Manager. De bestaande Operations Manager-agent blijft normaal aan de Operations Manager-server rapporteren. Houd er rekening mee dat de Operations Manager agent en Log Analytics agent gemeen schappelijke runtime-bibliotheken delen, die tijdens dit proces worden bijgewerkt naar de meest recente versie.
Opmerking: als Operations Manager agent versie 2012 is geïnstalleerd, **moet u niet** automatisch inrichten inschakelen op.<br>

- Er is een vooraf bestaande VM-extensie aanwezig<br>
    - Wanneer de bewakings agent is geïnstalleerd als een uitbrei ding, staat de extensie configuratie slechts aan één werk ruimte toe. Security Center worden bestaande verbindingen met gebruikers ruimten niet overschreven. Security Center worden beveiligings gegevens van de virtuele machine opgeslagen in de werk ruimte die al is verbonden, op voor waarde dat de oplossing Security of securityFree is geïnstalleerd. Security Center kunt de extensie versie in dit proces upgraden naar de meest recente versie.  
    - Als u wilt zien naar welke werk ruimte de bestaande extensie gegevens verzendt, voert u de test uit om de [connectiviteit met Azure Security Center te valideren](https://blogs.technet.microsoft.com/yuridiogenes/2017/10/13/validating-connectivity-with-azure-security-center/). U kunt ook Log Analytics-werk ruimten openen, een werk ruimte selecteren, de virtuele machine selecteren en kijken naar de Log Analytics agent-verbinding. 
    - Als u een omgeving hebt waarin de Log Analytics-agent is geïnstalleerd op client werkstations en rapportage aan een bestaande Log Analytics-werk ruimte, controleert u de lijst met [besturings systemen die worden ondersteund door Azure Security Center](security-center-os-coverage.md) om ervoor te zorgen dat uw besturings systeem wordt ondersteund. Zie [bestaande log Analytics-klanten](security-center-faq.md#existingloganalyticscust)voor meer informatie.
 
### Automatische inrichting uitschakelen<a name="offprovisioning"></a>
U kunt op elk gewenst moment automatische inrichting van resources uitschakelen door deze instelling uit te scha kelen in het beveiligings beleid. 


1. Ga terug naar het hoofd menu van Security Center en selecteer het beveiligings beleid.
2. Klik op **Instellingen bewerken** in de rij van het abonnement waarvoor u automatisch inrichten wilt uitschakelen.
3. Selecteer op de Blade **beveiligings beleid – gegevens verzameling** onder **automatische inrichting** de optie **uitschakelen**.
4. Selecteer **Opslaan**.

   ![Automatische inrichting uitschakelen][6]

Als automatisch inrichten is uitgeschakeld (uitgeschakeld), wordt de sectie standaard configuratie van werk ruimte niet weer gegeven.

Als u automatische inrichting uitschakelt nadat dit eerder is gedaan:
-   Agents worden niet ingericht op nieuwe Vm's.
-   Security Center stopt met het verzamelen van gegevens uit de standaardwerk ruimte.
 
> [!NOTE]
>  Als u automatische inrichting uitschakelt, wordt de Log Analytics-agent niet verwijderd van Azure-Vm's waar de agent is ingericht. Zie [Hoe kan ik OMS-extensies verwijderen die door Security Center zijn geïnstalleerd](security-center-faq.md#remove-oms)voor meer informatie over het verwijderen van de OMS-extensie.
>
    
## Hand matige agent inrichting<a name="manual-agent"></a>
 
Er zijn verschillende manieren om de Log Analytics agent hand matig te installeren. Wanneer u hand matig installeert, zorgt u ervoor dat u automatische inrichting uitschakelt.

### <a name="operations-management-suite-vm-extension-deployment"></a>Implementatie van VM-extensie van Operations Management Suite 

U kunt de Log Analytics-agent hand matig installeren, zodat Security Center beveiligings gegevens van uw virtuele machines kan verzamelen en aanbevelingen en waarschuwingen kunt geven.
1. Selecteer automatische inrichting – uit.
2. Maak een werk ruimte en stel de prijs categorie in voor de werk ruimte die u wilt instellen voor de Log Analytics-agent:

   a.  Selecteer in het hoofd menu van Security Center de optie **beveiligings beleid**.
     
   b.  Selecteer de werk ruimte waarin u de agent wilt verbinden. Zorg ervoor dat de werk ruimte zich in hetzelfde abonnement bevindt dat u in Security Center gebruikt en dat u lees-en schrijf machtigingen hebt voor de werk ruimte.
       werk ruimte ![selecteren][8]
3. Stel de prijs categorie in.
   ![prijs categorie selecteren][9] 
   >[!NOTE]
   >Als er al een **beveiligings** -of **SecurityCenterFree** -oplossing is ingeschakeld voor de werk ruimte, wordt de prijs automatisch ingesteld. 
   > 

4. Als u de agents op nieuwe Vm's wilt implementeren met behulp van een resource manager-sjabloon, installeert u de OMS-extensie van de virtuele machine:

   a.  [De OMS-extensie voor virtuele machines voor Windows installeren](../virtual-machines/extensions/oms-windows.md)
    
   b.  [De OMS-extensie van de virtuele machine voor Linux installeren](../virtual-machines/extensions/oms-linux.md)
5. Als u de uitbrei dingen op bestaande Vm's wilt implementeren, volgt u de instructies in [gegevens verzamelen over Azure virtual machines](../azure-monitor/learn/quick-collect-azurevm.md).

   > [!NOTE]
   > De sectie **gebeurtenis-en prestatie gegevens verzamelen** is optioneel.
   >
6. Als u Power shell wilt gebruiken om de uitbrei ding te implementeren, gebruikt u het volgende Power shell-voor beeld:
   
   [!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]
   
   1. Ga naar **log Analytics** en klik op **Geavanceerde instellingen**.
    
      ![Log Analytics instellen][11]

   2. Kopieer de waarden van de **WorkspaceID** en de **primaire sleutel**.
  
      ![Waarden kopiëren][12]

   3. Vul de open bare configuratie en de persoonlijke configuratie in met de volgende waarden:
     
           $PublicConf = @{
               "workspaceId"= "<WorkspaceID value>"
           }
 
           $PrivateConf = @{
               "workspaceKey"= "<Primary key value>"
           }

      - Wanneer u installeert op een Windows-VM:
        
            Set-AzVMExtension -ResourceGroupName $vm.ResourceGroupName -VMName $vm.Name -Name "MicrosoftMonitoringAgent" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "MicrosoftMonitoringAgent" -TypeHandlerVersion '1.0' -Location $vm.Location -settings $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True 
    
      - Wanneer u installeert op een virtuele Linux-machine:
        
            Set-AzVMExtension -ResourceGroupName $vm1.ResourceGroupName -VMName $vm1.Name -Name "OmsAgentForLinux" -Publisher "Microsoft.EnterpriseCloud.Monitoring" -ExtensionType "OmsAgentForLinux" -TypeHandlerVersion '1.0' -Location $vm.Location -Settingstring $PublicConf -ProtectedSettingString $PrivateConf -ForceRerun True`

> [!NOTE]
> Zie voor instructies over het onboarden van Security Center met behulp van Power shell de Azure Security Center voor bereiding op het [uitvoeren van](security-center-powershell-onboarding.md)een onboarding.

## <a name="troubleshooting"></a>Problemen oplossen

-   Zie [agent status problemen controleren](security-center-troubleshooting-guide.md#mon-agent)voor informatie over het vaststellen van automatische inrichting van installatie problemen.

-  Zie [problemen met de netwerk vereisten](security-center-troubleshooting-guide.md#mon-network-req)voor de bewakings agent oplossen om de netwerk vereisten voor de bewakings agent te identificeren.
-   Zie problemen met de voorbereidings [procedure van Operations Management Suite oplossen](https://support.microsoft.com/help/3126513/how-to-troubleshoot-operations-management-suite-onboarding-issues)voor informatie over hand matige onboarding-problemen.

- Om niet-bewaakte Vm's en computers problemen te identificeren:

    Een virtuele machine of computer wordt niet bewaakt door Security Center als op de computer niet de micro soft Monitoring Agent-extensie wordt uitgevoerd. Op een computer is mogelijk al een lokale agent geïnstalleerd, bijvoorbeeld de OMS direct-agent of de System Center Operations Manager-agent. Computers met deze agents worden geïdentificeerd als niet-bewaakt omdat deze agents niet volledig worden ondersteund in Security Center. De Microsoft Monitoring Agent-extensie is vereist om volledig te profiteren van alle mogelijkheden van Security Center.

    Zie [agent health issues](security-center-troubleshooting-guide.md#mon-agent)(Engelstalig) voor meer informatie over de redenen waarom Security Center vm's en computers die zijn geïnitialiseerd voor automatische inrichting niet kunnen controleren.


## <a name="next-steps"></a>Volgende stappen
In dit artikel wordt uitgelegd hoe het verzamelen en automatisch inrichten van gegevens in Security Center werkt. Zie de volgende onderwerpen voor meer informatie over het Beveiligingscentrum:

* [Azure Security Center FAQ](security-center-faq.md): raadpleeg veelgestelde vragen over het gebruik van de service.
* [Security health monitoring in Azure Security Center](security-center-monitoring.md): ontdek hoe u de status van uw Azure-resources kunt monitoren.



<!--Image references-->
[1]: ./media/security-center-enable-data-collection/enable-automatic-provisioning.png
[2]: ./media/security-center-enable-data-collection/use-another-workspace.png
[3]: ./media/security-center-enable-data-collection/reconfigure-monitored-vm.png
[5]: ./media/security-center-enable-data-collection/data-collection-tiers.png
[6]: ./media/security-center-enable-data-collection/disable-data-collection.png
[7]: ./media/security-center-enable-data-collection/select-subscription.png
[8]: ./media/security-center-enable-data-collection/manual-provision.png
[9]: ./media/security-center-enable-data-collection/pricing-tier.png
[10]: ./media/security-center-enable-data-collection/workspace-selection.png
[11]: ./media/security-center-enable-data-collection/log-analytics.png
[12]: ./media/security-center-enable-data-collection/log-analytics2.png
