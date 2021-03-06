---
title: Container voor opslag FSLogix-profiel Windows virtueel bureau blad-Azure
description: Opties voor het opslaan van uw FSLogix-profiel voor Windows virtueel bureau blad op Azure Storage.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: helohr
ms.openlocfilehash: 3577b459a1b6612d24346428512cfbc509515f52
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607411"
---
# <a name="storage-options-for-fslogix-profile-containers-in-windows-virtual-desktop"></a>Opslag opties voor FSLogix-profiel containers in virtueel bureau blad van Windows

Azure biedt meerdere opslag oplossingen die u kunt gebruiken om uw FSLogix-profiel container op te slaan. In dit artikel worden opslag oplossingen vergeleken Azure Storage aanbiedingen voor FSLogix profiel container gebruikers profielen van Windows.

Windows Virtual Desktop biedt FSLogix-profiel containers als de aanbevolen gebruikers profiel oplossing. FSLogix is ontworpen om profielen te roamen in omgevingen met externe computers, zoals Windows virtueel bureau blad. Bij het aanmelden wordt deze container dynamisch gekoppeld aan de computer omgeving met behulp van een systeem eigen ondersteunde virtuele harde schijf (VHD) en een virtuele harde schijf (VHDX) van Hyper-V. Het gebruikers profiel is onmiddellijk beschikbaar en wordt in het systeem weer gegeven op dezelfde manier als een systeem eigen gebruikers profiel.

De volgende tabel bevat een vergelijking van de opslag oplossingen Azure Storage aanbiedingen voor FSLogix profiel container gebruikers profielen van Windows-virtueel bureau blad.

## <a name="azure-platform-details"></a>Details van het Azure-platform

|Functies|Azure Files|Azure NetApp Files|Storage Spaces Direct|
|--------|-----------|------------------|---------------------|
|Platform service|Ja, Azure-systeem eigen oplossing|Ja, Azure-systeem eigen oplossing|Nee, zelf beheerd|
|Regionale beschikbaarheid|Alle regio's|[Regio's selecteren](https://azure.microsoft.com/global-infrastructure/services/?products=netapp&regions=all)|Alle regio's|
|Redundantie|Lokaal redundante/zone-redundant/geo-redundant|Lokaal redundant|Lokaal redundante/zone-redundant/geo-redundant|
|Lagen en prestaties|Standard<br>Premium<br>Maxi maal 100.000 IOPS per share met 5 GBps per share op ongeveer 3 MS latentie|Standard<br>Premium<br>Laag<br>Tot 320k (16 KB) IOPS met 4,5 GBps per volume bij ongeveer 1 MS-latentie|Standard-HDD: Maxi maal 500 IOPS per schijf limieten<br>Standard-SSD: Maxi maal 4.000 IOPS per schijf limieten<br>Premium-SSD: Maxi maal 20.000 IOPS per schijf limieten<br>Premium-schijven voor Opslagruimten Direct worden aanbevolen|
|Capaciteit|100 TiB per share|100 TiB per volume, Maxi maal 12,5 PiB per abonnement|Maxi maal 32 TiB per schijf|
|Vereiste infra structuur|Minimale share grootte van 1 GiB|Minimale capaciteits pool 4 TiB, minimale volume grootte 100 GiB|Twee Vm's op Azure IaaS (+ Cloudwitness) of ten minste drie Vm's zonder en kosten voor schijven|
|Protocollen|SMB 2.1/3. en REST|NFSv3, NFSv 4.1 (preview), SMB 3. x/2. x|NFSv3, NFSv 4.1, SMB 3,1|

## <a name="azure-management-details"></a>Details van Azure-beheer

|Functies|Azure Files|Azure NetApp Files|Storage Spaces Direct|
|--------|-----------|------------------|---------------------|
|Toegang|Cloud, on-premises en hybride (Azure-bestands synchronisatie)|Cloud, on-premises (via ExpressRoute)|Cloud, on-premises|
|Back-up maken|Integratie van Azure backup snap shot|Moment opnamen Azure NetApp Files|Integratie van Azure backup snap shot|
|Veiligheid en naleving|[Alle door Azure ondersteunde certificaten](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|ISO voltooid|[Alle door Azure ondersteunde certificaten](https://www.microsoft.com/trustcenter/compliance/complianceofferings)|
|Integratie van Azure Active Directory|Azure Active Directory en Azure Active Directory Domain Services|[Azure Active Directory Domain Services en systeem eigen Active Directory](../azure-netapp-files/azure-netapp-files-faqs.md#does-azure-netapp-files-support-azure-active-directory)|Alleen systeem eigen Active Directory-of Azure Active Directory Domain Services-ondersteuning|

Als u uw opslag methode hebt gekozen, raadpleegt u de [prijzen voor Windows virtueel bureau blad](https://azure.microsoft.com/pricing/details/virtual-desktop/) voor informatie over onze prijs plannen.

## <a name="next-steps"></a>Volgende stappen

Zie de tabel in [FSLogix-profiel containers en Azure files](fslogix-containers-azure-files.md)voor meer informatie over FSLogix-profiel containers, gebruikers profiel schijven en andere gebruikers profiel technologieën.

Als u klaar bent om uw eigen FSLogix-profiel containers te maken, kunt u aan de slag met een van deze zelf studies:

- [Aan de slag met FSLogix-profiel containers op Azure Files in Windows virtueel bureau blad](https://techcommunity.microsoft.com/t5/Windows-IT-Pro-Blog/Getting-started-with-FSLogix-profile-containers-on-Azure-Files/ba-p/746477)
- [Een FSLogix-profiel container maken voor een hostgroep met Azure NetApp-bestanden](create-fslogix-profile-container.md)
- De instructies [voor het implementeren van een met twee knoop punten opslagruimten direct scale-out Bestands server voor UPD-opslag in azure](https://docs.microsoft.com/windows-server/remote/remote-desktop-services/rds-storage-spaces-direct-deployment) , zijn ook van toepassing wanneer u een FSLogix-profiel container gebruikt in plaats van een gebruikers profiel schijf

U kunt ook beginnen met het begin en uw eigen virtueel-bureaublad oplossing van Windows instellen bij het [maken van een Tenant in Windows virtueel bureau blad](tenant-setup-azure-active-directory.md).
