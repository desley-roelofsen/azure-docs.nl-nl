---
title: De implementatie van uw Azure Monitor-logboeken ontwerpen | Microsoft Docs
description: In dit artikel worden de overwegingen en aanbevelingen beschreven voor klanten die de implementatie van een werk ruimte in Azure Monitor voorbereiden.
ms.service: azure-monitor
ms.subservice: ''
ms.topic: conceptual
author: MGoedtel
ms.author: magoedte
ms.date: 09/20/2019
ms.openlocfilehash: 373c498b9ce58062e42f4318c9fa94688556d8c5
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74894212"
---
# <a name="designing-your-azure-monitor-logs-deployment"></a>De implementatie van uw Azure Monitor-logboeken ontwerpen

Azure Monitor worden [logboek](data-platform-logs.md) gegevens opgeslagen in een log Analytics-werk ruimte, een Azure-resource en een container waarin gegevens worden verzameld, geaggregeerd en fungeert als een administratieve grens. Hoewel u een of meer werk ruimten in uw Azure-abonnement kunt implementeren, zijn er verschillende overwegingen die u moet begrijpen om ervoor te zorgen dat uw eerste implementatie voldoet aan onze richt lijnen om u te voorzien van een kosten effectief, beheersbaar en schaalbaar implementatie die aan uw organisatie behoeften voldoet.

Gegevens in een werk ruimte zijn ingedeeld in tabellen, die elk verschillende soorten gegevens opslaan en een eigen unieke set eigenschappen hebben op basis van de resource waarmee de gegevens worden gegenereerd. De meeste gegevens bronnen schrijven naar hun eigen tabellen in een Log Analytics-werk ruimte.

![Voor beeld van werk ruimte gegevens model](./media/design-logs-deployment/logs-data-model-01.png)

Een Log Analytics-werk ruimte biedt:

* Een geografische locatie voor de opslag van gegevens.
* Gegevens isolatie door verschillende gebruikers toegangs rechten te verlenen volgens een van onze aanbevolen ontwerp strategieën.
* Bereik voor configuratie van instellingen, zoals de [prijs categorie](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#changing-pricing-tier), [retentie](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#change-the-data-retention-period)en het beperken van [gegevens](https://docs.microsoft.com/azure/azure-monitor/platform/manage-cost-storage#manage-your-maximum-daily-data-volume).

Dit artikel bevat een gedetailleerd overzicht van de overwegingen voor het ontwerpen en migreren, het overzicht van toegangs beheer en een uitleg van de ontwerp implementaties die wij voor uw IT-organisatie raden.



## <a name="important-considerations-for-an-access-control-strategy"></a>Belang rijke overwegingen voor een strategie voor toegangs beheer

Het identificeren van het aantal werk ruimten dat u nodig hebt, is van invloed op een of meer van de volgende vereisten:

* U bent een wereld wijd bedrijf en u hebt logboek gegevens nodig die in specifieke regio's zijn opgeslagen voor gegevens-soevereiniteit of nalevings redenen.
* U gebruikt Azure en wilt kosten voor de overdracht van uitgaande gegevens voorkomen door een werkruimte in dezelfde regio te hebben als de Azure-resource die deze beheert.
* U beheert meerdere afdelingen of bedrijfs groepen en u wilt dat elk van de eigen gegevens wordt weer gegeven, maar niet de gegevens van anderen. Er is ook geen zakelijke vereiste voor een geconsolideerde weer gave van meerdere afdelingen of bedrijfs groepen.

De IT-organisaties zijn tegenwoordig gemodelleerd volgens een gecentraliseerd, gedecentraliseerd of een in-tussen hybride van beide structuren. Als gevolg hiervan zijn de volgende implementatie modellen voor werk ruimten vaak gebruikt om toe te wijzen aan een van deze organisatie structuren:

* **Gecentraliseerd**: alle logboeken worden opgeslagen in een centrale werk ruimte en beheerd door één team, met Azure monitor een gedifferentieerde toegang per team bieden. In dit scenario is het eenvoudig om te beheren, te zoeken naar resources en logboeken te cross-correleren. De werk ruimte kan aanzienlijk toenemen, afhankelijk van de hoeveelheid gegevens die uit meerdere resources in uw abonnement is verzameld, met extra administratieve overhead voor het onderhouden van toegangs beheer voor verschillende gebruikers.
* **Gedecentraliseerd**: elk team heeft hun eigen werk ruimte die is gemaakt in een resource groep die het eigendom is van en het beheer, en logboek gegevens worden gescheiden per resource. In dit scenario kan de werk ruimte veilig worden bewaard en kan toegangs beheer consistent zijn met toegang tot bronnen, maar het is lastig om logboeken te intercorreleren. Gebruikers die een brede weer gave van veel resources nodig hebben, kunnen de gegevens niet op een zinvolle manier analyseren.
* **Hybride**: vereisten voor naleving van beveiligings controle zijn dit scenario verder ingewik kelder omdat veel organisaties beide implementatie modellen parallel implementeren. Dit resulteert doorgaans in een complexe, dure en moeilijk te onderhouden configuratie met hiaten in de logboeken dekking.

Wanneer u de Log Analytics-agents gebruikt om gegevens te verzamelen, moet u het volgende weten om de implementatie van de agent te plannen:

* Als u gegevens van Windows-agents wilt verzamelen, kunt u [elke agent configureren om te rapporteren aan een of meer werk ruimten](../../azure-monitor/platform/agent-windows.md), zelfs wanneer deze wordt gerapporteerd aan een System Center Operations Manager-beheer groep. De Windows-agent kan Maxi maal vier werk ruimten rapporteren.
* De Linux-agent biedt geen ondersteuning voor multi-multihoming en kan slechts aan één werk ruimte rapporteren.

Als u System Center Operations Manager 2012 R2 of hoger gebruikt:

* Elke Operations Manager-beheer groep kan worden [verbonden met slechts één werk ruimte](../platform/om-agents.md). 
* Linux-computers die rapporteren aan een beheer groep moeten worden geconfigureerd om rechtstreeks te rapporteren aan een Log Analytics-werk ruimte. Als uw Linux-computers al rechtstreeks aan een werk ruimte rapporteren en u deze wilt controleren met Operations Manager, voert u de volgende stappen uit om [aan een Operations Manager beheer groep te rapporteren](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group). 
* U kunt de Log Analytics Windows-agent op de Windows-computer installeren en deze rapporteren aan zowel Operations Manager geïntegreerd met een werk ruimte als een andere werk ruimte.

## <a name="access-control-overview"></a>Overzicht van toegangsbeheer

Met op rollen gebaseerd toegangs beheer (RBAC) kunt u gebruikers en groepen alleen de hoeveelheid toegang verlenen die nodig zijn om te werken met bewakings gegevens in een werk ruimte. Op die manier kunt u met uw IT-bedrijfs model uitlijnen met één werk ruimte om verzamelde gegevens op te slaan die zijn ingeschakeld voor al uw resources. U kunt bijvoorbeeld toegang verlenen aan uw team dat verantwoordelijk is voor infrastructuur services die worden gehost op Azure virtual machines (Vm's), en als gevolg hiervan hebben ze alleen toegang tot de logboeken die door de Vm's worden gegenereerd. Dit is het nieuwe bron-context logboek model. De basis voor dit model geldt voor elke logboek record die wordt gegenereerd door een Azure-resource en wordt automatisch aan deze resource gekoppeld. Logboeken worden doorgestuurd naar een centrale werk ruimte die op de bronnen bereik en RBAC respecteert.

De gegevens waartoe een gebruiker toegang heeft, wordt bepaald door een combi natie van factoren die in de volgende tabel worden weer gegeven. Elk wordt beschreven in de volgende secties.

| Multi-factor Authentication | Beschrijving |
|:---|:---|
| [Toegangsmodus](#access-mode) | De methode die de gebruiker gebruikt voor toegang tot de werk ruimte.  Hiermee definieert u het bereik van de beschik bare gegevens en de toegangs beheer modus die wordt toegepast. |
| [Toegangs beheer modus](#access-control-mode) | Instelling in de werk ruimte die definieert of machtigingen worden toegepast op het niveau van de werk ruimte of de resource. |
| [Machtigingen](manage-access.md) | Machtigingen die worden toegepast op afzonderlijke of groepen gebruikers voor de werk ruimte of resource. Hiermee definieert u welke gegevens de gebruiker toegang heeft. |
| [RBAC op tabel niveau](manage-access.md#table-level-rbac) | Optionele gedetailleerde machtigingen die van toepassing zijn op alle gebruikers, ongeacht de toegangs modus of de toegangs beheer modus. Hiermee definieert u welke gegevens typen een gebruiker kan openen. |

## <a name="access-mode"></a>Toegangsmodus

De *toegangs modus* verwijst naar hoe een gebruiker toegang heeft tot een log Analytics-werk ruimte en definieert het bereik van de gegevens waartoe ze toegang hebben. 

Gebruikers hebben twee opties om toegang tot de gegevens te krijgen:

* **Werk ruimte-context**: u kunt alle logboeken weer geven in de werk ruimte waarvoor u machtigingen hebt. Query's in deze modus zijn gericht op alle gegevens in alle tabellen in de werk ruimte. Dit is de toegangs modus die wordt gebruikt wanneer logboeken worden geopend met de werk ruimte als het bereik, bijvoorbeeld wanneer u **Logboeken** selecteert in het **Azure monitor** menu in de Azure Portal.

    ![Context van Log Analytics in werk ruimte](./media/design-logs-deployment/query-from-workspace.png)

* **Resource-context**: wanneer u de werk ruimte voor een bepaalde resource, resource groep of abonnement opent, bijvoorbeeld wanneer u **Logboeken** selecteert in een Resource menu in het Azure Portal, kunt u Logboeken voor alleen resources weer geven in alle tabellen waartoe u toegang hebt. Query's in deze modus zijn alleen van toepassing op gegevens die aan die resource zijn gekoppeld. Met deze modus kunt u ook granulaire RBAC.

    ![Log Analytics context van resource](./media/design-logs-deployment/query-from-resource.png)

    > [!NOTE]
    > Logboeken zijn alleen beschikbaar voor resource context query's als ze goed zijn gekoppeld aan de betreffende resource. Momenteel hebben de volgende resources beperkingen:
    > - Computers buiten Azure
    > - Service Fabric
    > - Application Insights
    >
    > U kunt testen of Logboeken goed zijn gekoppeld aan hun resource door een query uit te voeren en de records te controleren waarin u bent geïnteresseerd. Als de juiste resource-ID wordt weer gegeven in de eigenschap [_ResourceId](log-standard-properties.md#_resourceid) , zijn de gegevens beschikbaar voor resource gerichte query's.

Azure Monitor bepaalt automatisch de juiste modus, afhankelijk van de context waarin u de zoek opdracht in Logboeken uitvoert. Het bereik wordt altijd weer gegeven in de linkerbovenhoek van Log Analytics.

### <a name="comparing-access-modes"></a>De toegangs modi vergelijken

De volgende tabel bevat een overzicht van de toegangs modi:

| | Werk ruimte-context | Resource-context |
|:---|:---|:---|
| Voor wie is elk model bedoeld? | Centraal beheer. Beheerders die gegevens verzameling en gebruikers moeten configureren die toegang nodig hebben tot een groot aantal verschillende bronnen. Dit is ook vereist voor gebruikers die toegang moeten hebben tot logboeken voor bronnen buiten Azure. | Toepassings teams. Beheerders van Azure-resources die worden bewaakt. |
| Wat heeft een gebruiker nodig om logboeken weer te geven? | Machtigingen voor de werk ruimte. Zie **machtigingen voor werk ruimten** in [toegang beheren via werkruimte machtigingen](manage-access.md#manage-access-using-workspace-permissions). | Lees toegang tot de resource. Zie **resource machtigingen** in [toegang beheren met Azure-machtigingen](manage-access.md#manage-access-using-azure-permissions). Machtigingen kunnen worden overgenomen (bijvoorbeeld van de container resource groep) of rechtstreeks worden toegewezen aan de resource. De machtigingen voor de logboeken voor de resource worden automatisch toegewezen. |
| Wat is het bereik van machtigingen? | werk ruimte. Gebruikers met toegang tot de werk ruimte kunnen alle logboeken in de werk ruimte opvragen van tabellen waarvoor ze machtigingen hebben. Zie [Table Access Control](manage-access.md#table-level-rbac) | Azure-resource. De gebruiker kan Logboeken zoeken voor specifieke resources, resource groepen of abonnementen waartoe ze toegang hebben vanuit een wille keurige werk ruimte, maar geen logboeken kunnen doorzoeken voor andere resources. |
| Hoe kan de gebruiker toegang krijgen tot logboeken? | <ul><li>Start **Logboeken** vanuit **Azure monitor** menu.</li></ul> <ul><li>Start **Logboeken** vanuit **log Analytics werk ruimten**.</li></ul> <ul><li>Vanuit Azure Monitor [werkmappen](../visualizations.md#workbooks).</li></ul> | <ul><li>**Logboeken** starten vanuit het menu voor de Azure-resource</li></ul> <ul><li>Start **Logboeken** vanuit **Azure monitor** menu.</li></ul> <ul><li>Start **Logboeken** vanuit **log Analytics werk ruimten**.</li></ul> <ul><li>Vanuit Azure Monitor [werkmappen](../visualizations.md#workbooks).</li></ul> |

## <a name="access-control-mode"></a>Toegangs beheer modus

De *Access Control-modus* is een instelling voor elke werk ruimte die definieert hoe machtigingen voor de werk ruimte worden bepaald.

* **Werkruimte machtigingen vereisen**: in deze besturings modus is granulaire RBAC niet toegestaan. Een gebruiker heeft alleen toegang tot de werk ruimte als deze machtigingen voor de werk ruimte of specifieke tabellen hebben.

    Als een gebruiker toegang heeft tot de werk ruimte na de context modus van de werk ruimte, hebben ze toegang tot alle gegevens in een tabel waaraan toegang is verleend. Als een gebruiker toegang heeft tot de werk ruimte die volgt op de resource-context modus, hebben ze alleen toegang tot gegevens voor die resource in een tabel waaraan ze toegang hebben verleend.

    Dit is de standaard instelling voor alle werk ruimten die zijn gemaakt vóór 2019 maart.

* **Resource-of werkruimte machtigingen gebruiken**: deze besturings modus maakt granulaire RBAC mogelijk. Gebruikers kunnen toegang krijgen tot gegevens die zijn gekoppeld aan resources die ze kunnen weer geven door Azure `read`-machtiging toe te wijzen. 

    Wanneer een gebruiker toegang heeft tot de werk ruimte in de werk ruimte-context modus, zijn werkruimte machtigingen van toepassing. Wanneer een gebruiker de werk ruimte in de resource context modus opent, worden alleen resource machtigingen gecontroleerd en worden de machtigingen voor de werk ruimte genegeerd. Schakel RBAC voor een gebruiker in door deze uit de werkruimte machtigingen te verwijderen en de machtigingen van de resource te herkennen.

    Dit is de standaard instelling voor alle werk ruimten die na maart 2019 zijn gemaakt.

    > [!NOTE]
    > Als een gebruiker alleen resource machtigingen heeft voor de werk ruimte, hebben ze alleen toegang tot de werk ruimte via de resource-context modus als de toegangs modus voor de werk ruimte is ingesteld op het **gebruik van resource-of werkruimte machtigingen**.

Zie de [modus toegangs beheer configureren](manage-access.md#configure-access-control-mode)voor meer informatie over het wijzigen van de toegangs beheer modus in de portal, met Power shell of het gebruik van een resource manager-sjabloon.

## <a name="ingestion-volume-rate-limit"></a>Frequentie limiet opname volume

Azure Monitor is een grootschalige gegevens service waarmee duizenden klanten elke maand terabytes aan gegevens verzenden in een groei tempo. De standaard drempel voor opname snelheden is ingesteld op **500 MB/min** per werk ruimte. Als u gegevens met een hoger snelheid naar één werk ruimte verzendt, worden sommige gegevens verwijderd en wordt er om de 6 uur een gebeurtenis verzonden naar de *bewerkings* tabel in uw werk ruimte, terwijl de drempel waarde blijft overschreden. Als uw opname volume de frequentie limiet blijft overschrijden of als u verwacht dat deze kort te bereiken, kunt u een verhoging van uw werk ruimte aanvragen door een ondersteunings aanvraag te openen.
 
Als u een melding wilt ontvangen over een dergelijke gebeurtenis in uw werk ruimte, maakt u een [waarschuwings regel](alerts-log.md) voor het logboek met behulp van de volgende query met de logica van een waarschuwing op basis van het aantal resultaten dat is gelukt dan nul.

``` Kusto
Operation
|where OperationCategory == "Ingestion"
|where Detail startswith "The rate of data crossed the threshold"
``` 


## <a name="recommendations"></a>Aanbevelingen

![Voor beeld van resource-context ontwerp](./media/design-logs-deployment/workspace-design-resource-context-01.png)

Dit scenario heeft betrekking op één werkruimte ontwerp in uw IT-organisatie abonnement dat niet wordt beperkt door de gegevens soevereiniteit of de naleving van de regelgeving, of moet worden toegewezen aan de regio's waar uw resources in worden geïmplementeerd. Hiermee kunnen uw organisaties beveiliging en IT-beheerders teams gebruikmaken van de verbeterde integratie met Azure Access Management en veiligere toegangs beheer.

Alle resources, bewakings oplossingen en inzichten zoals Application Insights en Azure Monitor voor VM's, ondersteunende infra structuur en toepassingen die worden onderhouden door de verschillende teams, zijn geconfigureerd om hun verzamelde logboek gegevens door te sturen naar de IT-organisaties gecentraliseerde gedeelde werk ruimte. Gebruikers van elk team krijgen toegang tot logboeken voor bronnen waartoe ze toegang hebben gekregen.

Wanneer u uw werkruimte architectuur hebt geïmplementeerd, kunt u dit afdwingen op Azure-resources met [Azure Policy](../../governance/policy/overview.md). Het biedt een manier om beleid te definiëren en te controleren op naleving van uw Azure-resources, zodat ze al hun resource logboeken naar een bepaalde werk ruimte verzenden. Met virtuele machines of virtuele-machine schaal sets van Azure kunt u bijvoorbeeld bestaande beleids regels gebruiken om de naleving van de werk ruimte te evalueren en resultaten te rapporteren, of om te herstellen als deze niet compatibel is.  

## <a name="workspace-consolidation-migration-strategy"></a>Strategie voor consolidatie migratie van werk ruimte

Voor klanten die al meerdere werk ruimten hebben geïmplementeerd en die geïnteresseerd zijn in het samen voegen van het model voor toegang tot de resource context, raden we u aan een incrementele benadering te gebruiken om naar het aanbevolen toegangs model te migreren, en u probeert dit niet te bereiken snel of agressief. Na een gefaseerde benadering van het plannen, migreren, valideren en buiten gebruik stellen van een redelijke tijd lijn, kunt u ongeplande incidenten of onverwachte gevolgen voor uw Cloud bewerkingen vermijden. Als u geen gegevens retentie beleid voor naleving of zakelijke redenen hebt, moet u de juiste tijds duur bepalen om gegevens te bewaren in de werk ruimte die u migreert tijdens het proces. Terwijl u resources opnieuw configureert om te rapporteren aan de gedeelde werk ruimte, kunt u de gegevens in de oorspronkelijke werk ruimte nog steeds analyseren als dat nodig is. Wanneer de migratie is voltooid en u de gegevens in de oorspronkelijke werk ruimte voor het einde van de Bewaar periode wilt behouden, moet u deze niet verwijderen.

Houd bij het plannen van de migratie naar dit model rekening met het volgende:

* Begrijp welke industriële voor schriften en interne beleids regels met betrekking tot het bewaren van gegevens u moet naleven.
* Zorg ervoor dat uw toepassings teams kunnen werken in de bestaande resource-context functionaliteit.
* Bepaal welke toegang wordt verleend aan resources voor uw toepassings teams en test in een ontwikkel omgeving voordat u de productie implementeert.
* Configureer de werk ruimte om **resource-of werkruimte machtigingen te gebruiken**.
* De machtiging Application teams verwijderen om de werk ruimte te lezen en er query's op uit te zoeken.
* Schakel alle bewakings oplossingen, inzichten zoals Azure Monitor voor containers en/of Azure Monitor voor VM's, uw Automation-account (s) en beheer oplossingen, zoals Updatebeheer, start/stop Vm's, enzovoort, in die zijn geïmplementeerd in de oorspronkelijke werk ruimte.

## <a name="next-steps"></a>Volgende stappen

Als u de beveiligings machtigingen en-besturings elementen wilt implementeren die worden aanbevolen in deze hand leiding, raadpleegt u de [toegang tot logboeken beheren](manage-access.md).