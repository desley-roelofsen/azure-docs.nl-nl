---
title: Verzamelen van gegevens van een met Azure Log Analytics-agent | Microsoft Docs
description: In dit onderwerp vindt u informatie over het verzamelen van gegevens en controleren van computers die worden gehost in Azure, on-premises of andere cloudomgeving met Log Analytics.
ms.service: azure-monitor
ms.subservice: logs
ms.topic: conceptual
author: MGoedtel
ms.author: magoedte
ms.date: 11/21/2019
ms.openlocfilehash: 33ba07ac8d89546856666cc7ab94fae650020001
ms.sourcegitcommit: 8a2949267c913b0e332ff8675bcdfc049029b64b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74306527"
---
# <a name="collect-log-data-with-the-log-analytics-agent"></a>Logboek gegevens verzamelen met de Log Analytics-agent

De Azure Log Analytics-agent, voorheen de micro soft Monitoring Agent (MMA) of OMS Linux-agent genoemd, is ontwikkeld voor uitgebreid beheer van on-premises computers, computers die worden bewaakt door [System Center Operations Manager](https://docs.microsoft.com/system-center/scom/)en virtuele machines in elke Cloud. De Windows-en Linux-agents zijn gekoppeld aan een Azure Monitor en verzamelen verzamelde logboek gegevens van verschillende bronnen in uw Log Analytics-werk ruimte, evenals eventuele unieke Logboeken of meet waarden zoals gedefinieerd in een bewakings oplossing. 

Dit artikel bevat een gedetailleerd overzicht van de agent, system en netwerkvereisten en de verschillende implementatiemethoden.

## <a name="overview"></a>Overzicht

![Log Analytics-agent communicatie diagram](./media/log-analytics-agent/log-analytics-agent-01.png)

Voordat u de verzamelde gegevens analyseert en bijwerkt, moet u eerst agents installeren en verbinden voor alle computers die u wilt verzenden van gegevens naar de Azure Monitor-service. U kunt agents installeren op uw Azure-VM's met behulp van de Azure Log Analytics VM-extensie voor Windows en Linux, en voor computers in een hybride omgeving met behulp van setup, vanaf de opdrachtregel, of met Desired State Configuration (DSC) in Azure Automation. 

De agent voor Linux en Windows communiceert uitgaande van de TCP-poort 443 met de Azure Monitor-service. als de computer verbinding maakt via een firewall of proxy server om via internet te communiceren, raadpleegt u de vereisten hieronder voor meer informatie over de netwerk configuratie Vereist. Als uw IT-beveiligings beleid niet toestaat dat computers in het netwerk verbinding maken met internet, kunt u een [log Analytics gateway](gateway.md) instellen en vervolgens de agent configureren om via de gateway verbinding te maken met Azure monitor-Logboeken. De agent kan vervolgens configuratie-informatie ontvangen en verzamelde gegevens, afhankelijk van de regels voor gegevens verzameling en bewakings oplossingen die u hebt ingeschakeld in uw werk ruimte. 

Wanneer u de Log Analytics-agents gebruikt om gegevens te verzamelen, moet u het volgende weten om de implementatie van de agent te plannen:

* Als u gegevens van Windows-agents wilt verzamelen, kunt u [elke agent configureren om te rapporteren aan een of meer werk ruimten](agent-windows.md), zelfs wanneer deze wordt gerapporteerd aan een System Center Operations Manager-beheer groep. De Windows-agent kan Maxi maal vier werk ruimten rapporteren.
* De Linux-agent biedt geen ondersteuning voor multi-multihoming en kan slechts aan één werk ruimte rapporteren.
* De Windows-agent ondersteunt de [FIPS 140-standaard](https://docs.microsoft.com/windows/security/threat-protection/fips-140-validation), terwijl de Linux-agent deze niet ondersteunt.  

Als u System Center Operations Manager 2012 R2 of hoger gebruikt:

* Elke Operations Manager-beheer groep kan worden [verbonden met slechts één werk ruimte](om-agents.md).
* Linux-computers die rapporteren aan een beheer groep moeten worden geconfigureerd om rechtstreeks te rapporteren aan een Log Analytics-werk ruimte. Als uw Linux-computers al rechtstreeks aan een werk ruimte rapporteren en u deze wilt controleren met Operations Manager, voert u de volgende stappen uit om [aan een Operations Manager beheer groep te rapporteren](agent-manage.md#configure-agent-to-report-to-an-operations-manager-management-group).
* U kunt de Log Analytics Windows-agent op de Windows-computer installeren en deze rapporteren aan zowel Operations Manager geïntegreerd met een werk ruimte als een andere werk ruimte.

De agent voor Linux en Windows is niet alleen om verbinding te maken met Azure Monitor, maar ondersteunt ook Azure Automation voor het hosten van de Hybrid Runbook worker-rol en andere services, zoals [Wijzigingen bijhouden](../../automation/change-tracking.md), [updatebeheer](../../automation/automation-update-management.md)en [Azure Security Center](../../security-center/security-center-intro.md). Zie [Azure Automation Hybrid Runbook worker](../../automation/automation-hybrid-runbook-worker.md)voor meer informatie over de functie Hybrid Runbook Worker.  

## <a name="supported-windows-operating-systems"></a>Ondersteunde Windows-besturingssystemen

De volgende versies van het Windows-besturingssysteem worden officieel ondersteund voor de Windows-agent:

* Windows Server 2019
* Windows Server 2008 SP2 (x64), 2008 R2, 2012, 2012 R2, 2016, versie 1709 en 1803
* Windows 7 SP1, Windows 8 Enter prise en Pro, en Windows 10 Enter prise en Pro

>[!NOTE]
>Hoewel de Log Analytics-agent voor Windows is ontworpen om server bewakings scenario's te ondersteunen, kunnen we u realiseren dat Windows-client wordt uitgevoerd voor de ondersteuning van werk belastingen die zijn geconfigureerd en geoptimaliseerd voor het besturings systeem van de server. De agent biedt ondersteuning voor Windows-clients, maar onze bewakings oplossingen zijn niet gericht op client bewakings scenario's, tenzij expliciet wordt vermeld.

## <a name="supported-linux-operating-systems"></a>Ondersteunde Linux-besturingssystemen

In deze sectie bevat informatie over de ondersteunde Linux-distributies.

Beginnen met versies die na augustus 2018 wordt uitgebracht, maken we de volgende wijzigingen aan ons ondersteuningsmodel:  

* Alleen de server-versies worden ondersteund, een niet-client.  
* Nieuwe versies van door [Azure Linux geviseerde distributies](../../virtual-machines/linux/endorsed-distros.md) worden altijd ondersteund.  
* Alle secundaire versies worden ondersteund voor elke primaire versie weergegeven.
* De versies die zijn geslaagd voor de datum van de fabrikant einde van ondersteuning worden niet ondersteund.  
* Nieuwe versies van AMI worden niet ondersteund.  
* Alleen versies met SSL 1.x standaard worden ondersteund.

>[!NOTE]
>Als u een distributie of een versie die wordt momenteel niet ondersteund en wordt niet afgestemd op onze ondersteuningsmodel gebruikt, raden wij dat u deze opslagplaats splitst, waarmee wordt bevestigd dat Microsoft-ondersteuning geen hulp bij gesplitste agent versies geeft.

* Amazon Linux 2017.09 (x 64)
* CentOS Linux 6 (x86/x64) en 7 (x 64)  
* Oracle Linux 6 en 7 (x86/x64) 
* Red Hat Enterprise Linux Server 6 (x86/x64) en 7 (x 64)
* Debian GNU/Linux 8 en 9 (x86/x64)
* Ubuntu 14.04 LTS (x86/x64), 16.04 LTS (x86/x64) en 18.04 LTS (x64)
* SUSE Linux Enterprise Server 12 (x64) en 15 (x64)

>[!NOTE]
>OpenSSL 1.1.0 wordt alleen ondersteund op x86_x64 platforms (64-bits) en OpenSSL ouder dan 1.x wordt niet ondersteund op elk platform.
>

### <a name="agent-prerequisites"></a>Agent vereisten

De volgende tabel geeft een overzicht van de pakketten die vereist zijn voor de ondersteunde Linux-distributies waarop de agent wordt geïnstalleerd.

|Vereist pakket |Beschrijving |Minimale versie |
|-----------------|------------|----------------|
|Glibc |    GNU C-bibliotheek | 2.5-12 
|Openssl    | OpenSSL-bibliotheken | 1.0. x of 1.1. x |
|Ezelsoor | Krul webclient | 7.15.5 |
|Python-ctypes | | 
|PAM | Pluggable verificatie modules | | 

>[!NOTE]
>Rsyslog of syslog-ng is vereist voor het verzamelen van syslog-berichten. De standaard syslog-daemon op versie 5 van Red Hat Enterprise Linux, CentOS en Oracle Linux-versie (sysklog) wordt niet ondersteund voor de verzameling van syslog. Als u syslog-gegevens uit deze versie van deze distributies wilt verzamelen, moet de rsyslog-daemon worden geïnstalleerd en geconfigureerd om sysklog te vervangen.

## <a name="tls-12-protocol"></a>TLS 1.2-protocol

We raden u ten zeerste aan de agent te configureren voor het gebruik van ten minste Transport Layer Security (TLS) 1,2 om de beveiliging van gegevens die onderweg zijn naar Azure Monitor logboeken te garanderen. Er zijn oudere versies van TLS/Secure Sockets Layer (SSL) gevonden die kwetsbaar zijn en terwijl ze nog steeds werken om achterwaartse compatibiliteit mogelijk te maken, worden ze **niet aanbevolen**.  Raadpleeg voor meer informatie [veilig verzenden van gegevens met behulp van TLS 1,2](../../azure-monitor/platform/data-security.md#sending-data-securely-using-tls-12). 

## <a name="network-firewall-requirements"></a>Netwerkvereisten voor de firewall

Hieronder vindt u de informatie over de proxy-en firewall configuratie die is vereist voor de Linux-en Windows-agent om te communiceren met Azure Monitor-Logboeken.  

|Agentresource|Poorten |Richting |HTTPS-controle overslaan|
|------|---------|--------|--------|   
|*.ods.opinsights.azure.com |Poort 443 |Uitgaand|Ja |  
|*.oms.opinsights.azure.com |Poort 443 |Uitgaand|Ja |  
|*.blob.core.windows.net |Poort 443 |Uitgaand|Ja |  

Zie [Azure Government Management](../../azure-government/documentation-government-services-monitoringandmanagement.md#azure-monitor-logs)voor informatie over de firewall die vereist is voor Azure Government. 

Als u van plan bent de Azure Automation Hybrid Runbook Worker te gebruiken om verbinding te maken met de Automation-Service voor het gebruik van runbooks of beheer oplossingen in uw omgeving, moet deze toegang hebben tot het poort nummer en de Url's die worden beschreven in [uw netwerk configureren voor de Hybrid Runbook worker](../../automation/automation-hybrid-runbook-worker.md#network-planning). 

De Windows-en Linux-agent ondersteunt communicatie via een proxy server of Log Analytics gateway naar Azure Monitor met behulp van het HTTPS-protocol.  Zowel eenvoudige als anonieme verificatie (gebruikersnaam en wachtwoord) worden ondersteund.  Voor de Windows-agent die rechtstreeks is verbonden met de service, wordt de proxy configuratie opgegeven tijdens de installatie of [na de implementatie](agent-manage.md#update-proxy-settings) vanuit het configuratie scherm of met Power shell.  

Voor de Linux-agent wordt de proxy server tijdens de installatie of [na de installatie](agent-manage.md#update-proxy-settings) opgegeven door het configuratie bestand proxy. conf te wijzigen.  Waarde voor de Linux-agent proxyconfiguratie heeft de volgende syntaxis:

`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
> Als uw proxy-server niet vereist is om te verifiëren, moet de Linux-agent nog steeds bieden een pseudo-gebruiker en wachtwoord. Dit kan een gebruikersnaam of het wachtwoord zijn.

|Eigenschap| Beschrijving |
|--------|-------------|
|Protocol | https |
|user | Optioneel de gebruikersnaam voor proxy-verificatie |
|wachtwoord | Optionele wachtwoord voor proxy-verificatie |
|proxyhost | Adres of FQDN-naam van de proxy-server/Log Analytics-gateway |
|poort | Optionele poortnummer voor de proxy-server/Log Analytics-gateway |

Bijvoorbeeld: `https://user01:password@proxy01.contoso.com:30443`

> [!NOTE]
> Als u speciale tekens zoals '\@' in uw wacht woord gebruikt, ontvangt u een proxy verbindings fout omdat de waarde onjuist is geparseerd.  U kunt dit probleem omzeilen door het wacht woord in de URL te coderen met een hulp programma zoals [UrlDecode](https://www.urldecoder.org/).  

## <a name="install-and-configure-agent"></a>Agent installeren en configureren

U kunt de computers in uw Azure-abonnement of hybride omgeving rechtstreeks met Azure Monitor logboeken verbinden met behulp van verschillende methoden, afhankelijk van uw vereisten. De volgende tabel ziet u elke methode om te bepalen welke het beste werkt in uw organisatie.

|Bron | Methode | Beschrijving|
|-------|-------------|-------------|
|Azure VM| -Log Analytics VM-extensie voor [Windows](../../virtual-machines/extensions/oms-windows.md) of [Linux](../../virtual-machines/extensions/oms-linux.md) met behulp van Azure CLI of met een Azure Resource Manager sjabloon<br>- [hand matig van de Azure Portal](../../azure-monitor/learn/quick-collect-azurevm.md?toc=/azure/azure-monitor/toc.json)<br>- [Azure Security Center automatische inrichting](../../security-center/security-center-enable-data-collection.md)| -De uitbrei ding installeert de Log Analytics agent op virtuele machines van Azure en registreert deze in een bestaande Azure Monitor-werk ruimte.<br>-Azure Security Center kunt de Log Analytics agent inrichten op alle ondersteunde Azure-Vm's en eventuele nieuwe virtuele machines die worden gemaakt als u deze inschakelt om te controleren op beveiligings problemen en bedreigingen. Als deze functie is ingeschakeld, wordt er een nieuwe of bestaande virtuele machine zonder geïnstalleerde agent ingericht.|
| Hybride Windows-computer|- [hand matige installatie](agent-windows.md)<br>- [Azure Automation DSC](agent-windows.md#install-the-agent-using-dsc-in-azure-automation)<br>- [Resource Manager-sjabloon met Azure stack](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/MicrosoftMonitoringAgent-ext-win) |Installeer de micro soft Monitoring Agent vanaf de opdracht regel of met behulp van een geautomatiseerde methode, zoals Azure Automation DSC, [System Center Configuration Manager](https://docs.microsoft.com/sccm/apps/deploy-use/deploy-applications)of met een Azure Resource Manager-sjabloon als u Microsoft Azure stack in uw Data Center hebt geïmplementeerd.| 
| Hybride Linux-computer| [Hand matige installatie](../../azure-monitor/learn/quick-collect-linux-computer.md)|Installeer de agent voor Linux aanroepen van een wrapper-scripts die worden gehost op GitHub. | 
| System Center Operations Manager|[Operations Manager integreren met Log Analytics](../../azure-monitor/platform/om-agents.md) | Configureer de integratie tussen Operations Manager en Azure Monitor logboeken voor het door sturen van verzamelde gegevens van Windows-computers die rapporteren aan een beheer groep.|  

## <a name="next-steps"></a>Volgende stappen

* Bekijk [gegevens bronnen](../../azure-monitor/platform/agent-data-sources.md) om inzicht te krijgen in de gegevens bronnen die beschikbaar zijn voor het verzamelen van gegevens uit uw Windows-of Linux-systeem. 

* Meer informatie over [logboek query's](../../azure-monitor/log-query/log-query-overview.md) voor het analyseren van de gegevens die zijn verzameld uit gegevens bronnen en oplossingen. 

* Meer informatie over het [controleren van oplossingen](../../azure-monitor/insights/solutions.md) voor het toevoegen van functionaliteit aan Azure monitor en het verzamelen van gegevens in de log Analytics-werk ruimte.
