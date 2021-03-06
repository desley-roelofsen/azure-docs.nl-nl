---
title: Meer informatie over de Windows-Hybrid Runbook Worker status in azure Updatebeheer
description: Meer informatie over het oplossen van problemen met de Hybrid Runbook Worker in Windows die ondersteuning bieden voor Updatebeheer.
services: automation
author: mgoedtel
ms.author: magoedte
ms.date: 12/03/2019
ms.topic: conceptual
ms.service: automation
ms.subservice: update-management
manager: carmonm
ms.openlocfilehash: 284376759e9e2da1f42bd04eea6e564c9690d4a9
ms.sourcegitcommit: c38a1f55bed721aea4355a6d9289897a4ac769d2
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/05/2019
ms.locfileid: "74850122"
---
# <a name="understand-the-windows-hybrid-runbook-worker-health-in-update-management"></a>Inzicht in de status van de Windows-Hybrid Runbook Worker in Updatebeheer

Er kunnen veel redenen zijn waarom uw computer niet **gereed** is in updatebeheer. In Updatebeheer kunt u de status van een Hybrid Runbook Worker agent controleren om het onderliggende probleem te bepalen. In dit artikel wordt beschreven hoe u de probleem oplosser voor Azure-machines uitvoert vanaf de Azure Portal-en niet-Azure-machines in het [offline scenario](#troubleshoot-offline).

De volgende lijst geeft een overzicht van de drie gereedheids statussen die een machine kan hebben:

* **Gereed** : de Hybrid Runbook Worker is geïmplementeerd en is minder dan 1 uur geleden voor het laatst gezien.
* De **verbinding is verbroken** : de Hybrid Runbook Worker is geïmplementeerd en is meer dan 1 uur geleden voor het laatst gezien.
* **Niet geconfigureerd** : de Hybrid Runbook Worker is niet gevonden of is niet gereed voor onboarding.

> [!NOTE]
> Er kan een lichte vertraging optreden tussen de Azure Portal weer geven en de huidige status van de machine.

## <a name="start-the-troubleshooter"></a>De probleemoplosser voor starten

Voor Azure-machines klikt u op de koppeling **problemen oplossen** onder de gereedheids kolom van de **Update Agent** in de portal, wordt de pagina **problemen met Update agent oplossen** gestart. Voor niet-Azure-computers brengt de koppeling u naar dit artikel. Zie de [offline-instructies](#troubleshoot-offline) voor het oplossen van problemen met een niet-Azure-machine.

![Update beheer lijst van virtuele machines](../media/update-agent-issues/vm-list.png)

> [!NOTE]
> De virtuele machine moet worden uitgevoerd om de status van de Hybrid Runbook Worker te controleren. Als de VM niet wordt uitgevoerd, wordt **de knop VM starten** weer gegeven.

Selecteer op de pagina **Update Agent voor problemen oplossen** de optie **controles uitvoeren** om de probleem oplosser te starten. De probleem Oplosser gebruikt de [opdracht uitvoeren](../../virtual-machines/windows/run-command.md) om een script uit te voeren op de computer om afhankelijkheden te controleren. Wanneer de probleem Oplosser is voltooid, wordt het resultaat van de controles geretourneerd.

![Problemen met de pagina Update agent oplossen](../media/update-agent-issues/troubleshoot-page.png)

De resultaten worden weer gegeven op de pagina wanneer ze klaar zijn. In de secties controles wordt weer gegeven wat er in elke controle is opgenomen.

![Problemen met de Update Agent controleren](../media/update-agent-issues/update-agent-checks.png)

## <a name="prerequisite-checks"></a>Controles van vereisten

### <a name="operating-system"></a>Besturingssysteem

Met de controle van het besturings systeem wordt gecontroleerd of op de Hybrid Runbook Worker een van de volgende besturings systemen wordt uitgevoerd:

|Besturingssysteem  |Opmerkingen  |
|---------|---------|
|Windows Server 2008 R2 RTM, Windows Server 2008 | Ondersteunt alleen update-evaluaties.         |
|Windows Server 2008 R2 SP1 en hoger |.NET Framework 4,6 of hoger is vereist. ([Down load de .NET Framework](/dotnet/framework/install/guide-for-developers))<br/> Windows Power shell 5,1 is vereist.  ([Down load Windows Management Framework 5,1](https://www.microsoft.com/download/details.aspx?id=54616))        |

### <a name="net-462"></a>.NET-4.6.2

Met de .NET Framework controle wordt gecontroleerd of er mini maal [.NET Framework 4.6.2](https://www.microsoft.com/en-us/download/details.aspx?id=53345) is geïnstalleerd op het systeem.

### <a name="wmf-51"></a>WMF 5.1

De WMF-controle controleert of het systeem de vereiste versie van het Windows Management Framework (WMF)- [Windows Management framework 5,1](https://www.microsoft.com/download/details.aspx?id=54616)bevat.

### <a name="tls-12"></a>TLS 1.2

Met deze controle wordt bepaald of u gebruikmaakt van TLS 1,2 om uw communicatie te versleutelen. TLS 1,0 wordt niet meer ondersteund door het platform. We raden aan dat clients TLS 1,2 gebruiken om te communiceren met Updatebeheer.

## <a name="connectivity-checks"></a>Controles voor connectiviteit

### <a name="registration-endpoint"></a>Eindpunt voor clientregistratie

Met deze controle wordt bepaald of de agent goed kan communiceren met de Agent service.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met het eindpunt voor clientregistratie toestaan. Zie [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning)voor een lijst met adressen en poorten die moeten worden geopend.

### <a name="operations-endpoint"></a>Operations-eindpunt

Met deze controle wordt bepaald of de agent goed kan communiceren met de taak runtime gegevens service.

Proxy- en firewallinstellingen configuraties moeten de Hybrid Runbook Worker-agent kan communiceren met de taak Runtime Data-Service toestaan. Zie [netwerk planning voor Hybrid Workers](../automation-hybrid-runbook-worker.md#network-planning)voor een lijst met adressen en poorten die moeten worden geopend.

## <a name="vm-service-health-checks"></a>Status controles van de VM-service

### <a name="monitoring-agent-service-status"></a>Status van Monitoring Agent-service

Met deze controle wordt bepaald of `HealthService`, de micro soft Monitoring Agent, wordt uitgevoerd op de computer.

Zie [micro soft monitoring agent wordt niet uitgevoerd](hybrid-runbook-worker.md#mma-not-running)voor meer informatie over het oplossen van problemen met de service.

Zie [micro soft Monitoring Agent installeren en configureren](../../azure-monitor/learn/quick-collect-windows-computer.md#install-the-agent-for-windows)om micro soft monitoring agent opnieuw te installeren.

### <a name="monitoring-agent-service-events"></a>Service gebeurtenissen van monitoring agent

Met deze controle wordt bepaald of `4502` gebeurtenissen in de afgelopen 24 uur worden weer gegeven in het Azure Operations Manager-logboek op de computer.

Raadpleeg de [gids voor probleem oplossing](hybrid-runbook-worker.md#event-4502) voor deze gebeurtenis voor meer informatie over deze gebeurtenis.

## <a name="access-permissions-checks"></a>Controles van toegangs machtigingen

### <a name="machinekeys-folder-access"></a>Toegang tot mappen MachineKeys

De toegangs controle voor de cryptografie mappen bepaalt of het lokale systeem account toegang heeft tot C:\ProgramData\Microsoft\Crypto\RSA.

## <a name="troubleshoot-offline"></a>Problemen met offline oplossen

U kunt de probleem Oplosser op een Hybrid Runbook Worker offline gebruiken door het script lokaal uit te voeren. U kunt het script, [Troubleshooting-WindowsUpdateAgentRegistration](https://www.powershellgallery.com/packages/Troubleshoot-WindowsUpdateAgentRegistration), ophalen in de PowerShell Gallery. U moet WMF 4,0 of hoger hebben geïnstalleerd om het script uit te voeren. Zie [verschillende versies van Power Shell installeren](https://docs.microsoft.com/powershell/scripting/install/installing-powershell)voor informatie over het downloaden van de meest recente versie van Power shell.

De uitvoer van dit script ziet eruit als in het volgende voor beeld:

```output
RuleId                      : OperatingSystemCheck
RuleGroupId                 : prerequisites
RuleName                    : Operating System
RuleGroupName               : Prerequisite Checks
RuleDescription             : The Windows Operating system must be version 6.1.7601 (Windows Server 2008 R2 SP1) or higher
CheckResult                 : Passed
CheckResultMessage          : Operating System version is supported
CheckResultMessageId        : OperatingSystemCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : DotNetFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : .NET Framework 4.5+
RuleGroupName               : Prerequisite Checks
RuleDescription             : .NET Framework version 4.5 or higher is required
CheckResult                 : Passed
CheckResultMessage          : .NET Framework version 4.5+ is found
CheckResultMessageId        : DotNetFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {}

RuleId                      : WindowsManagementFrameworkInstalledCheck
RuleGroupId                 : prerequisites
RuleName                    : WMF 5.1
RuleGroupName               : Prerequisite Checks
RuleDescription             : Windows Management Framework version 4.0 or higher is required (version 5.1 or higher is preferable)
CheckResult                 : Passed
CheckResultMessage          : Detected Windows Management Framework version: 5.1.17763.1
CheckResultMessageId        : WindowsManagementFrameworkInstalledCheck.Passed
CheckResultMessageArguments : {5.1.17763.1}

RuleId                      : AutomationAgentServiceConnectivityCheck1
RuleGroupId                 : connectivity
RuleName                    : Registration endpoint
RuleGroupName               : connectivity
RuleDescription             :
CheckResult                 : Failed
CheckResultMessage          : Unable to find Workspace registration information in registry
CheckResultMessageId        : AutomationAgentServiceConnectivityCheck1.Failed.NoRegistrationFound
CheckResultMessageArguments : {}

RuleId                      : AutomationJobRuntimeDataServiceConnectivityCheck
RuleGroupId                 : connectivity
RuleName                    : Operations endpoint
RuleGroupName               : connectivity
RuleDescription             : Proxy and firewall configuration must allow Automation Hybrid Worker agent to communicate with eus2-jobruntimedata-prod-su1.azure-automation.net
CheckResult                 : Passed
CheckResultMessage          : TCP Test for eus2-jobruntimedata-prod-su1.azure-automation.net (port 443) succeeded
CheckResultMessageId        : AutomationJobRuntimeDataServiceConnectivityCheck.Passed
CheckResultMessageArguments : {eus2-jobruntimedata-prod-su1.azure-automation.net}

RuleId                      : MonitoringAgentServiceRunningCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service status
RuleGroupName               : VM Service Health Checks
RuleDescription             : HealthService must be running on the machine
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service (HealthService) is not running
CheckResultMessageId        : MonitoringAgentServiceRunningCheck.Failed
CheckResultMessageArguments : {Microsoft Monitoring Agent, HealthService}

RuleId                      : MonitoringAgentServiceEventsCheck
RuleGroupId                 : servicehealth
RuleName                    : Monitoring Agent service events
RuleGroupName               : VM Service Health Checks
RuleDescription             : Event Log must not have event 4502 logged in the past 24 hours
CheckResult                 : Failed
CheckResultMessage          : Microsoft Monitoring Agent service Event Log (Operations Manager) does not exist on the machine
CheckResultMessageId        : MonitoringAgentServiceEventsCheck.Failed.NoLog
CheckResultMessageArguments : {Microsoft Monitoring Agent, Operations Manager, 4502}

RuleId                      : CryptoRsaMachineKeysFolderAccessCheck
RuleGroupId                 : permissions
RuleName                    : Crypto RSA MachineKeys Folder Access
RuleGroupName               : Access Permission Checks
RuleDescription             : SYSTEM account must have WRITE and MODIFY access to 'C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys'
CheckResult                 : Passed
CheckResultMessage          : Have permissions to access C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys
CheckResultMessageId        : CryptoRsaMachineKeysFolderAccessCheck.Passed
CheckResultMessageArguments : {C:\\ProgramData\\Microsoft\\Crypto\\RSA\\MachineKeys}

RuleId                      : TlsVersionCheck
RuleGroupId                 : prerequisites
RuleName                    : TLS 1.2
RuleGroupName               : Prerequisite Checks
RuleDescription             : Client and Server connections must support TLS 1.2
CheckResult                 : Passed
CheckResultMessage          : TLS 1.2 is enabled by default on the Operating System.
CheckResultMessageId        : TlsVersionCheck.Passed.EnabledByDefault
CheckResultMessageArguments : {}
```

## <a name="next-steps"></a>Volgende stappen

Zie [problemen met Hybrid Runbook Workers oplossen](hybrid-runbook-worker.md)om meer problemen met uw Hybrid runbook Workers op te lossen.
