---
title: "Veelgestelde vragen: back-ups maken van SAP HANA-data bases op Azure-Vm's"
description: In dit artikel vindt u antwoorden op veelgestelde vragen over het maken van back-ups van SAP HANA-data bases met behulp van de Azure Backup-service.
ms.topic: conceptual
ms.date: 11/7/2019
ms.openlocfilehash: e5684024668103ccbe13be4af3d7a9336651df77
ms.sourcegitcommit: e50a39eb97a0b52ce35fd7b1cf16c7a9091d5a2a
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74287838"
---
# <a name="frequently-asked-questions--back-up-sap-hana-databases-on-azure-vms"></a>Veelgestelde vragen: back-ups maken van SAP HANA-data bases op virtuele Azure-machines

In dit artikel vindt u antwoorden op veelgestelde vragen over het maken van back-ups van SAP HANA-data bases via de Azure Backup-Service

## <a name="backup"></a>Back-up

### <a name="how-many-full-backups-are-supported-per-day"></a>Hoeveel volledige back-ups worden per dag ondersteund?

Er wordt slechts één volledige back-up per dag ondersteund. U kunt geen differentiële back-up en volledige back-up op dezelfde dag activeren.

### <a name="do-successful-backup-jobs-create-alerts"></a>Maken succesvolle back-uptaken waarschuwingen?

Nee. Succesvolle back-uptaken maken geen waarschuwingen. Er worden alleen waarschuwingen verzonden voor mislukte back-uptaken. Gedetailleerd gedrag voor portal waarschuwingen wordt [hier](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-built-in-monitor)beschreven. Als u echter geïnteresseerd bent over waarschuwingen, zelfs voor geslaagde taken, kunt u [Azure monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)gebruiken.

### <a name="can-i-see-scheduled-backup-jobs-in-the-backup-jobs-menu"></a>Kan ik geplande back-uptaken weer geven in het menu back-uptaken?

In het menu back-uptaak worden alleen ad-hoc back-uptaken weer gegeven. Gebruik [Azure monitor](https://docs.microsoft.com/azure/backup/backup-azure-monitoring-use-azuremonitor)voor geplande taken.

### <a name="are-future-databases-automatically-added-for-backup"></a>Worden toekomstige data bases automatisch toegevoegd voor back-up?

Nee, dit wordt momenteel niet ondersteund.

### <a name="if-i-delete-a-database-from-an-instance-what-will-happen-to-the-backups"></a>Wat gebeurt er met de back-ups als ik een Data Base uit een exemplaar Verwijder?

Als een Data Base uit een SAP HANA-exemplaar wordt verwijderd, worden er nog steeds back-ups van de data base geprobeerd. Dit betekent dat de verwijderde data base wordt weer gegeven als beschadigd onder **Back-upitems** en nog steeds is beveiligd.
De juiste manier om de beveiliging van deze data base te stoppen, is het stoppen van de **back-up met het verwijderen van gegevens** op deze data base.

### <a name="if-i-change-the-name-of-the-database-after-it-has-been-protected-what-will-the-behavior-be"></a>Als ik de naam van de data base Wijzig nadat deze is beveiligd, wat is het gedrag?

Een hernoemde data base wordt behandeld als een nieuwe data base. Daarom wordt deze situatie door de service behandeld alsof de data base niet is gevonden en de back-ups niet kunnen worden uitgevoerd. De hernoemde data base wordt weer gegeven als een nieuwe data base en moet worden geconfigureerd voor beveiliging.

### <a name="what-are-the-prerequisites-to-back-up-sap-hana-databases-on-an-azure-vm"></a>Wat zijn de vereisten voor het maken van een back-up van SAP HANA-data bases op een virtuele machine van Azure?

Raadpleeg de secties [vereisten](tutorial-backup-sap-hana-db.md#prerequisites) en [machtigingen instellen](tutorial-backup-sap-hana-db.md#setting-up-permissions) .

### <a name="what-permissions-should-be-set-for-azure-to-be-able-to-back-up-sap-hana-databases"></a>Welke machtigingen moeten worden ingesteld voor Azure om een back-up te kunnen maken van SAP HANA-data bases?

Als u het script voor voorafgaande registratie uitvoert, worden de vereiste machtigingen ingesteld om Azure back-ups te maken van SAP HANA-data bases. Meer informatie over de machtigingen vindt u [hier](tutorial-backup-sap-hana-db.md#setting-up-permissions).

### <a name="will-backups-work-after-migrating-sap-hana-from-10-to-20"></a>Werkt de back-ups na de migratie van SAP HANA van 1,0 naar 2,0?

Raadpleeg [deze sectie](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database-troubleshoot#upgrading-from-sap-hana-10-to-20) van de hand leiding voor het oplossen van problemen.

## <a name="restore"></a>Herstellen

### <a name="why-cant-i-see-the-hana-system-i-want-my-database-to-be-restored-to"></a>Waarom kan ik het HANA-systeem waarin ik mijn data base wil herstellen niet zien?

Controleer of aan alle vereisten voor het terugzetten naar het doel SAP HANA exemplaar wordt voldaan. Zie voor meer informatie [vereisten: Restore SAP Hana data bases in azure VM](https://docs.microsoft.com/azure/backup/sap-hana-db-restore#prerequisites).

### <a name="why-is-the-overwrite-db-restore-failing-for-my-database"></a>Waarom wordt het terugzetten van de overschrijvings database mislukt voor mijn data base?

Zorg ervoor dat de optie voor **overschrijven afdwingen** is geselecteerd tijdens het herstellen.

### <a name="why-do-i-see-the-source-and-target-systems-for-restore-are-incompatible-error"></a>Waarom zie ik de fout ' bron-en doel systemen voor herstel zijn niet compatibel '?

Raadpleeg de SAP HANA nota [1642148](https://launchpad.support.sap.com/#/notes/1642148) om te zien welke typen herstel bewerkingen momenteel worden ondersteund.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het maken van een back-up van [SAP Hana-data bases](https://docs.microsoft.com/azure/backup/backup-azure-sap-hana-database) die worden uitgevoerd op Azure
