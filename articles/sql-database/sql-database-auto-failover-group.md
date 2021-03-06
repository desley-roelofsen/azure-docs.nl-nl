---
title: Failover-groepen
description: Groeps beleidsobjecten voor automatische failover is een SQL Database functie waarmee u replicatie en automatische/gecoördineerde failover kunt beheren van een groep data bases op een SQL Database Server of alle data bases in het beheerde exemplaar.
services: sql-database
ms.service: sql-database
ms.subservice: high-availability
ms.custom: ''
ms.devlang: ''
ms.topic: conceptual
author: anosov1960
ms.author: sashan
ms.reviewer: mathoma, carlrab
ms.date: 11/07/2019
ms.openlocfilehash: 470e9a9c36b6b4ec2e40db5dfc47ae03fb6b5aa8
ms.sourcegitcommit: 4c831e768bb43e232de9738b363063590faa0472
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/23/2019
ms.locfileid: "74421384"
---
# <a name="use-auto-failover-groups-to-enable-transparent-and-coordinated-failover-of-multiple-databases"></a>Gebruik groepen voor automatische failover om transparante en gecoördineerde failover van meerdere data bases mogelijk te maken

Groepen voor automatische failover is een SQL Database functie waarmee u replicatie en failover kunt beheren van een groep data bases op een SQL Database-Server of voor alle data bases in een beheerd exemplaar naar een andere regio. Het is een declaratieve abstractie boven op de bestaande [actieve geo-replicatie](sql-database-active-geo-replication.md) functie, ontworpen om de implementatie en het beheer van geo-gerepliceerde data bases op schaal te vereenvoudigen. U kunt failover hand matig initiëren of u kunt deze overdragen aan de SQL Database-service op basis van een door de gebruiker gedefinieerd beleid. Met deze laatste optie kunt u automatisch meerdere gerelateerde data bases in een secundaire regio herstellen na een onherstelbare fout of een andere niet-geplande gebeurtenis waardoor het volledige of gedeeltelijke verlies van de beschik baarheid van de SQL Database-Service in de primaire regio wordt veroorzaakt. Een failover-groep kan een of meer data bases bevatten, die meestal worden gebruikt door dezelfde toepassing. Daarnaast kunt u de Lees bare secundaire data bases gebruiken om werk belastingen met alleen-lezen query's te offloaden. Omdat voor groepen voor automatische failover meerdere data bases zijn vereist, moeten deze data bases worden geconfigureerd op de primaire server. Automatische failover-groepen ondersteunen replicatie van alle data bases in de groep naar slechts één secundaire server in een andere regio.

> [!NOTE]
> Wanneer u werkt met één of gegroepeerde Data bases op een SQL Database Server en u meerdere secundaire zones in dezelfde of verschillende regio's wilt, gebruikt u [actieve geo-replicatie](sql-database-active-geo-replication.md). 

Als u gebruikmaakt van groepen voor automatische failover met automatische failoverbeleid, wordt een storing die van invloed is op een of meer van de data bases in de groep, in automatische failover veroorzaakt. Dit zijn doorgaans incidenten die niet kunnen worden verholpen door de ingebouwde automatische maximale Beschik baarheid. De voor beelden van failover-triggers zijn een incident dat wordt veroorzaakt door een SQL-Tenant ring of besturings element dat niet beschikbaar is vanwege een geheugenlek van een OS-kernel op verschillende reken knooppunten, of een incident dat wordt veroorzaakt door een of meer tenants, omdat er een onjuiste netwerk kabel werd geknipt tijdens de ro utine hardware uit bedrijf nemen.  Zie [SQL database hoge Beschik baarheid](sql-database-high-availability.md)voor meer informatie.

Daarnaast bieden automatische-failover-groepen alleen-lezen-en alleen-lezen listener-eind punten die ongewijzigd blijven tijdens failovers. Ongeacht of u hand matige of automatische failover hebt geactiveerd, schakelt failover alle secundaire data bases in de groep over naar primair. Nadat de data base-failover is voltooid, wordt de DNS-record automatisch bijgewerkt om de eind punten om te leiden naar de nieuwe regio. Zie [overzicht van bedrijfs continuïteit](sql-database-business-continuity.md)voor de specifieke RPO-en RTO-gegevens.

Wanneer u groepen voor automatische failover gebruikt met automatische failoverbeleid, resulteert elke storing die van invloed is op data bases in de SQL Database Server of het beheerde exemplaar tot een automatische failover. U kunt de groep voor automatische failover beheren met:

- [Azure Portal](sql-database-implement-geo-distributed-database.md)
- [Power shell: failover-groep](scripts/sql-database-add-single-db-to-failover-group-powershell.md)
- [Rest API: failovergroep](https://docs.microsoft.com/rest/api/sql/failovergroups).

Zorg er na een failover voor dat de verificatie vereisten voor uw server en Data Base zijn geconfigureerd op de nieuwe primaire. Zie [SQL database Security na nood herstel](sql-database-geo-replication-security-config.md)voor meer informatie.

Voor een echte bedrijfs continuïteit is het toevoegen van database redundantie tussen data centers slechts een deel van de oplossing. Het herstellen van een toepassing (Service) End-to-end na een onherstelbare fout vereist het herstellen van alle onderdelen die de service en eventuele afhankelijke services vormen. Voor beelden van deze onderdelen zijn de client software (bijvoorbeeld een browser met een aangepaste Java script), web-front-ends, opslag en DNS. Het is van essentieel belang dat alle onderdelen zich in dezelfde storingen bevinden en beschikbaar komen binnen de beoogde herstel tijd (RTO) van uw toepassing. Daarom moet u alle afhankelijke services identificeren en inzicht krijgen in de garanties en mogelijkheden die ze bieden. Vervolgens moet u de juiste stappen nemen om ervoor te zorgen dat uw service functioneert tijdens de failover van de services waarvan deze afhankelijk is. Zie [cloud oplossingen ontwerpen voor herstel na nood gevallen met actieve geo-replicatie](sql-database-designing-cloud-solutions-for-disaster-recovery.md)voor meer informatie over het ontwerpen van oplossingen voor herstel na nood gevallen.

## <a name="auto-failover-group-terminology-and-capabilities"></a>Terminologie en mogelijkheden van groep voor automatische failover

- **Failovergroep (mist)**

  Een failovergroep is een benoemde groep data bases die wordt beheerd door een enkele SQL Database Server of binnen één beheerd exemplaar en waarvoor een failover kan worden uitgevoerd als een eenheid naar een andere regio voor het geval alle of enkele primaire data bases niet beschikbaar zijn vanwege een storing in de primaire regio. Bij het maken van beheerde exemplaren bevat een failovergroep alle gebruikers databases in het exemplaar en kan er slechts één failovergroep op een exemplaar worden geconfigureerd.
  
  > [!IMPORTANT]
  > De naam van de failovergroep moet globaal uniek zijn binnen het `.database.windows.net` domein.

- **SQL Database-servers**

     Met SQL Database-servers kunnen sommige of alle gebruikers databases op één SQL Database-Server in een failover-groep worden geplaatst. Daarnaast ondersteunt een SQL Database-Server meerdere failover-groepen op één SQL Database-Server.

- **Basis**

  De SQL Database Server of het beheerde exemplaar dat als host fungeert voor de primaire data bases in de failovergroep.

- **Primaire**

  De SQL Database Server of het beheerde exemplaar dat als host fungeert voor de secundaire data bases in de failovergroep. De secundaire kan zich niet in dezelfde regio bevinden als de primaire.

- **Afzonderlijke data bases aan een failovergroep toevoegen**

  U kunt verschillende afzonderlijke data bases op dezelfde SQL Database-Server in dezelfde failovergroep plaatsen. Als u één data base aan de failovergroep toevoegt, wordt er automatisch een secundaire data base gemaakt met dezelfde editie en reken grootte op de secundaire server.  U hebt de server opgegeven toen de failovergroep werd gemaakt. Als u een Data Base toevoegt die al een secundaire Data Base op de secundaire server heeft, wordt die geo-replicatie koppeling overgenomen door de groep. Wanneer u een Data Base toevoegt die al een secundaire Data Base bevat op een server die geen deel uitmaakt van de failovergroep, wordt er een nieuw secundair gemaakt op de secundaire server.

  > [!IMPORTANT]
  > Zorg ervoor dat de secundaire server geen data base met dezelfde naam heeft als deze een bestaande secundaire data base is. In failover-groepen voor het beheerde exemplaar worden alle gebruikers databases gerepliceerd. U kunt geen subset van gebruikers databases kiezen voor replicatie in de failovergroep.

- **Data bases in elastische pool toevoegen aan failovergroep**

  U kunt alle of meerdere data bases binnen een elastische pool in dezelfde failovergroep plaatsen. Als de primaire data base zich in een elastische pool bevindt, wordt het secundaire apparaat automatisch in de elastische pool gemaakt met dezelfde naam (secundaire groep). U moet ervoor zorgen dat de secundaire server een elastische pool met dezelfde exacte naam en voldoende vrije capaciteit bevat voor het hosten van de secundaire data bases die worden gemaakt door de failovergroep. Als u een data base in de groep toevoegt die al een secundaire data base in de secundaire groep heeft, wordt die geo-replicatie koppeling overgenomen door de groep. Wanneer u een Data Base toevoegt die al een secundaire Data Base bevat op een server die geen deel uitmaakt van de failovergroep, wordt er een nieuwe secundaire in de secundaire groep gemaakt.
  
- **DNS-zone**

  Een unieke ID die automatisch wordt gegenereerd wanneer een nieuwe instantie wordt gemaakt. Een SAN-certificaat (multi-Domain) voor dit exemplaar is ingericht voor het verifiëren van de client verbindingen met een wille keurig exemplaar in dezelfde DNS-zone. De twee beheerde exemplaren in dezelfde failovergroep moeten de DNS-zone delen.
  
  > [!NOTE]
  > Een DNS-zone-ID is niet vereist voor failover-groepen die zijn gemaakt voor SQL Database-servers.

- **Listener voor lezen/schrijven van failover-groep**

  Een DNS CNAME-record die verwijst naar de URL van de huidige primaire. Het wordt automatisch gemaakt wanneer de failovergroep wordt gemaakt en de SQL-workload voor lezen en schrijven kan transparant opnieuw verbinding maken met de primaire data base wanneer de primaire wijzigingen na een failover worden uitgevoerd. Wanneer de failovergroep op een SQL Database Server wordt gemaakt, wordt de DNS CNAME-record voor de URL van de listener gevormd als `<fog-name>.database.windows.net`. Wanneer de failovergroep wordt gemaakt op een beheerd exemplaar, wordt de DNS CNAME-record voor de URL van de listener gevormd als `<fog-name>.zone_id.database.windows.net`.

- **Listener voor alleen-lezen van failover-groep**

  Een DNS CNAME-record die verwijst naar de alleen-lezen listener die verwijst naar de URL van de secundaire. Het wordt automatisch gemaakt wanneer de failovergroep wordt gemaakt en de alleen-lezen SQL-workload kan transparant worden verbonden met de secundaire met behulp van de opgegeven taakverdelings regels. Wanneer de failovergroep op een SQL Database Server wordt gemaakt, wordt de DNS CNAME-record voor de URL van de listener gevormd als `<fog-name>.secondary.database.windows.net`. Wanneer de failovergroep wordt gemaakt op een beheerd exemplaar, wordt de DNS CNAME-record voor de URL van de listener gevormd als `<fog-name>.zone_id.secondary.database.windows.net`.

- **Beleid voor automatische failover**

  Een failover-groep is standaard geconfigureerd met een automatisch failoverbeleid. De SQL Database-service activeert failover nadat de fout is gedetecteerd en de respijt periode is verlopen. Het systeem moet controleren of de uitval niet kan worden verholpen door de ingebouwde [infra structuur met hoge Beschik baarheid van de SQL database-service](sql-database-high-availability.md) vanwege de schaal van de impact. Als u de werk stroom van de failover wilt beheren vanuit de toepassing, kunt u automatische failover uitschakelen.
  
  > [!NOTE]
  > Omdat verificatie van de omvang van de storing en hoe snel deze kan worden verholpen, acties van het operationele team worden uitgevoerd, kan de respijt periode niet meer dan één uur worden ingesteld.  Deze beperking is van toepassing op alle data bases in de failovergroep, ongeacht hun gegevens synchronisatie status. 

- **Alleen-lezen failoverbeleid**

  De failover van de alleen-lezen listener is standaard uitgeschakeld. Het zorgt ervoor dat de prestaties van de primaire server niet worden beïnvloed wanneer de secundaire offline is. Het betekent echter ook dat de alleen-lezen sessies geen verbinding kunnen maken tot het secundaire bestand is hersteld. Als u uitval tijd niet kunt gebruiken voor de alleen-lezen sessies en u de primaire alleen-lezen-en lees-schrijf bewerkingen wilt uitvoeren tegen onkosten van de mogelijke prestatie vermindering van de primaire, kunt u failover inschakelen voor de alleen-lezen listener door de eigenschap `AllowReadOnlyFailoverToPrimary` te configureren. In dat geval wordt het alleen-lezen verkeer automatisch omgeleid naar de primaire als de secundaire niet beschikbaar is.

- **Geplande failover**

   De geplande failover voert een volledige synchronisatie uit tussen de primaire en secundaire data bases vóór de secundaire switches naar de primaire rol. Dit garandeert geen gegevens verlies. Geplande failover wordt gebruikt in de volgende scenario's:

  - Herstel na nood gevallen (DR) in productie uitvoeren wanneer het gegevens verlies niet acceptabel is
  - De data bases naar een andere regio verplaatsen
  - De data bases retour neren naar de primaire regio nadat de storing is verholpen (failback).

- **Niet-geplande failover**

   Bij een niet-geplande of geforceerde failover wordt de secundaire functie direct overgeschakeld naar de primaire rol zonder synchronisatie met de primaire. Met deze bewerking wordt gegevens verlies veroorzaakt. Niet-geplande failover wordt gebruikt als herstel methode tijdens storingen wanneer de primaire niet toegankelijk is. Wanneer de oorspronkelijke primaire primair weer online is, wordt automatisch opnieuw verbinding gemaakt zonder synchronisatie en wordt een nieuwe secundaire.

- **Hand matige failover**

  U kunt failover op elk gewenst moment hand matig initiëren, ongeacht de automatische failover-configuratie. Als er geen automatische failoverbeleid is geconfigureerd, moet er hand matige failover worden uitgevoerd om data bases in de failovergroep naar de secundaire te herstellen. U kunt geforceerde of een gebruiks vriendelijke failover initiëren (met volledige gegevens synchronisatie). Deze laatste kan worden gebruikt om de primaire naar de secundaire regio te verplaatsen. Wanneer de failover is voltooid, worden de DNS-records automatisch bijgewerkt om ervoor te zorgen dat de verbinding met de nieuwe primaire

- **Respijt periode met gegevens verlies**

  Omdat de primaire en secundaire data bases worden gesynchroniseerd met asynchrone replicatie, kan de failover leiden tot gegevens verlies. U kunt het beleid voor automatische failover aanpassen zodat de tolerantie van uw toepassing wordt aangepast aan gegevens verlies. Door `GracePeriodWithDataLossHours`te configureren, kunt u bepalen hoe lang het systeem wacht voordat de failover wordt gestart, waardoor gegevens verlies waarschijnlijk wordt veroorzaakt.

- **Meerdere failover-groepen**

  U kunt meerdere failover-groepen voor hetzelfde paar servers configureren om de schaal van failovers te bepalen. Elke groep failover onafhankelijk. Als uw multi tenant-toepassing gebruikmaakt van elastische Pools, kunt u deze mogelijkheid gebruiken om de primaire en secundaire data bases in elke groep te combi neren. Op deze manier kunt u de impact van een storing beperken tot slechts de helft van de tenants.

  > [!NOTE]
  > Het beheerde exemplaar biedt geen ondersteuning voor meerdere failover-groepen.
  
## <a name="permissions"></a>Machtigingen

Machtigingen voor een failovergroep worden beheerd via [op rollen gebaseerd toegangs beheer (RBAC)](../role-based-access-control/overview.md). De rol [SQL Server Inzender](../role-based-access-control/built-in-roles.md#sql-server-contributor) heeft alle benodigde machtigingen voor het beheren van failover-groepen.

### <a name="create-failover-group"></a>Failovergroep maken

Als u een failovergroep wilt maken, moet u RBAC schrijf toegang hebben tot de primaire en secundaire servers en naar alle data bases in de failovergroep. Voor een beheerd exemplaar hebt u RBAC-schrijf toegang nodig voor het primaire en secundaire beheerde exemplaar, maar de machtigingen voor afzonderlijke data bases zijn niet relevant, omdat afzonderlijke beheerde exemplaar databases niet kunnen worden toegevoegd aan of verwijderd uit een failovergroep. 

### <a name="update-a-failover-group"></a>Een failovergroep bijwerken

Als u een failovergroep wilt bijwerken, moet u RBAC schrijf toegang hebben tot de failovergroep en alle data bases op de huidige primaire server of een beheerd exemplaar.  

### <a name="failover-a-failover-group"></a>Failover van een failovergroep

Als u een failover wilt uitvoeren, moet u RBAC-schrijf toegang hebben tot de failovergroep op de nieuwe primaire server of het beheerde exemplaar.

## <a name="best-practices-of-using-failover-groups-with-single-databases-and-elastic-pools"></a>Aanbevolen procedures voor het gebruik van failover-groepen met afzonderlijke data bases en elastische Pools

De groep voor automatische failover moet worden geconfigureerd op de primaire SQL Database-Server en wordt verbonden met de secundaire SQL Database-Server in een andere Azure-regio. De groepen kunnen alle of sommige data bases op deze servers bevatten. Het volgende diagram illustreert een typische configuratie van een geo-redundante Cloud toepassing met behulp van meerdere data bases en een groep voor automatische failover.

![automatische failover](./media/sql-database-auto-failover-group/auto-failover-group.png)

> [!NOTE]
> Zie [Eén data base toevoegen aan een failovergroep](sql-database-single-database-failover-group-tutorial.md) voor een gedetailleerde stapsgewijze zelf studie een enkele data base toevoegen aan een failovergroep.

Houd bij het ontwerpen van een service met bedrijfs continuïteit de volgende algemene richt lijnen:

- **Een of meer failover-groepen gebruiken om failover van meerdere data bases te beheren**

  Er kunnen een of meer failover-groepen worden gemaakt tussen twee servers in verschillende regio's (primaire en secundaire servers). Elke groep kan een of meer data bases bevatten die als een eenheid worden hersteld in het geval dat alle of enkele primaire data bases niet beschikbaar zijn vanwege een storing in de primaire regio. De groep failover maakt geo-secundaire data base met dezelfde service doelstelling als de primaire. Als u een bestaande geo-replicatie relatie aan de failovergroep toevoegt, zorg er dan voor dat de geo-Secondary is geconfigureerd met dezelfde servicelaag en reken grootte als de primaire.
  
  > [!IMPORTANT]
  > Het maken van failover-groepen tussen twee servers in verschillende abonnementen wordt momenteel niet ondersteund voor afzonderlijke data bases en elastische Pools. Als u de primaire of secundaire server naar een ander abonnement verplaatst nadat de failovergroep is gemaakt, kan dit leiden tot fouten in de failover-aanvragen en andere bewerkingen.

- **Listener voor read-write gebruiken voor OLTP-workload**

  Bij het uitvoeren van OLTP-bewerkingen, gebruikt u `<fog-name>.database.windows.net` als de server-URL en worden de verbindingen automatisch naar de primaire verbinding geleid. Deze URL wordt niet gewijzigd na de failover. Opmerking voor de failover moet de DNS-record worden bijgewerkt, zodat de client verbindingen alleen worden omgeleid naar de nieuwe primaire server nadat de DNS-cache van de client is vernieuwd.

- **Alleen-lezen-listener gebruiken voor alleen-lezen werk belasting**

  Als u een logisch geïsoleerde alleen-lezen werk belasting hebt die tolerant is voor bepaalde verouderde gegevens, kunt u de secundaire data base in de toepassing gebruiken. Voor alleen-lezen sessies gebruikt u `<fog-name>.secondary.database.windows.net` als de server-URL en wordt de verbinding automatisch omgeleid naar de secundaire. U wordt ook aangeraden om in connection string Lees intentie op te geven met behulp van `ApplicationIntent=ReadOnly`. Als u er zeker van wilt zijn dat de workload alleen-lezen opnieuw verbinding kan maken na een failover of als de secundaire server offline gaat, moet u de eigenschap `AllowReadOnlyFailoverToPrimary` van het failover-beleid configureren.

- **Voor bereidingen voor prestatie vermindering**

  De beslissing van SQL-failover is onafhankelijk van de rest van de toepassing of andere services die worden gebruikt. De toepassing kan worden ' Mixed ' met enkele onderdelen in één regio en een deel van een andere. Zorg ervoor dat u de redundante toepassing implementeert in de DR-regio en volg deze [richt lijnen voor netwerk beveiliging](#failover-groups-and-network-security)om de degradatie te voor komen.

  > [!NOTE]
  > De toepassing in de DR-regio hoeft geen andere connection string te gebruiken.  

- **Voorbereiden op gegevens verlies**

  Als er een storing wordt gedetecteerd, wacht SQL op de periode die u hebt opgegeven door `GracePeriodWithDataLossHours`. De standaard waarde is 1 uur. Als u geen gegevens verlies kunt veroorloven, moet u `GracePeriodWithDataLossHours` instellen op een voldoende groot getal, zoals 24 uur. Gebruik hand matige failover van de groep om een failback uit te geven van de secundaire naar de primaire.

  > [!IMPORTANT]
  > Elastische Pools met 800 of minder Dtu's en meer dan 250-data bases die gebruikmaken van geo-replicatie, kunnen problemen ondervinden, inclusief langere, geplande failovers en verminderde prestaties.  Deze problemen treden waarschijnlijk vaker op voor het schrijven van intensieve workloads, wanneer geo-replicatie-eind punten op grote schaal worden gescheiden door geografie, of wanneer meerdere secundaire eind punten worden gebruikt voor elke Data Base.  Symptomen van deze problemen worden aangegeven wanneer de geo-replicatie vertraging na verloop van tijd toeneemt.  Deze vertraging kan worden bewaakt met behulp van [sys. dm_geo_replication_link_status](/sql/relational-databases/system-dynamic-management-views/sys-dm-geo-replication-link-status-azure-sql-database).  Als deze problemen optreden, zijn de mogelijke oplossingen het verhogen van het aantal groeps Dtu's of het verminderen van het aantal geo-gerepliceerde data bases in dezelfde groep.

## <a name="best-practices-of-using-failover-groups-with-managed-instances"></a>Aanbevolen procedures voor het gebruik van failover-groepen met beheerde exemplaren

De groep voor automatische failover moet worden geconfigureerd op het primaire exemplaar en wordt verbonden met het secundaire exemplaar in een andere Azure-regio.  Alle data bases in het exemplaar worden gerepliceerd naar het secundaire exemplaar.

Het volgende diagram illustreert een typische configuratie van een geo-redundante Cloud toepassing met behulp van beheerde exemplaren en de groep voor automatische failover.

![automatische failover](./media/sql-database-auto-failover-group/auto-failover-group-mi.png)

> [!NOTE]
> Zie [Managed instance toevoegen aan een failovergroep](sql-database-managed-instance-failover-group-tutorial.md) voor een gedetailleerde stapsgewijze zelf studie voor het toevoegen van een beheerd exemplaar voor het gebruik van een failovergroep.

Als uw toepassing gebruikmaakt van een beheerd exemplaar als gegevenslaag, volgt u deze algemene richt lijnen bij het ontwerpen voor bedrijfs continuïteit:

- **Het secundaire exemplaar maken in dezelfde DNS-zone als het primaire exemplaar**

  Om ervoor te zorgen dat een niet-onderbroken connectiviteit met het primaire exemplaar na een failover wordt gegarandeerd, moeten de primaire en secundaire exemplaren zich in dezelfde DNS-zone bevindt. Hiermee wordt gegarandeerd dat hetzelfde multi-Domain (SAN)-certificaat kan worden gebruikt voor het verifiëren van de client verbindingen met een van de twee exemplaren in de failovergroep. Wanneer uw toepassing gereed is voor productie-implementatie, maakt u een secundair exemplaar in een andere regio en zorgt u ervoor dat de DNS-zone wordt gedeeld met het primaire exemplaar. U kunt dit doen door een `DNS Zone Partner` optionele para meter op te geven met behulp van de Azure Portal, Power shell of de REST API.

> [!IMPORTANT]
> Het eerste exemplaar dat in het subnet wordt gemaakt, bepaalt de DNS-zone voor alle volgende instanties in hetzelfde subnet. Dit betekent dat twee exemplaren van hetzelfde subnet geen deel kunnen uitmaken van verschillende DNS-zones.

  Zie [een secundaire beheerde instantie maken](sql-database-managed-instance-failover-group-tutorial.md#3---create-a-secondary-managed-instance)voor meer informatie over het maken van het secundaire exemplaar in dezelfde DNS-zone als het primaire exemplaar.

- **Replicatie verkeer tussen twee instanties inschakelen**

  Omdat elk exemplaar is geïsoleerd in een eigen VNet, moet twee richtings verkeer tussen deze VNets worden toegestaan. Zie [Azure VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md)

- **Een failover-groep maken tussen beheerde instanties in verschillende abonnementen**

  U kunt in twee verschillende abonnementen een failover-groep maken tussen beheerde exemplaren. Wanneer u Power shell API gebruikt, kunt u dit doen door de para meter `PartnerSubscriptionId` op te geven voor het secundaire exemplaar. Wanneer u REST API gebruikt, kan elke exemplaar-ID die is opgenomen in de para meter `properties.managedInstancePairs` een eigen subscriptionID hebben.
  
  > [!IMPORTANT]
  > Azure Portal biedt geen ondersteuning voor failover-groepen in verschillende abonnementen.

- **Een failovergroep configureren voor het beheren van de failover van het hele exemplaar**

  De failovergroep beheert de failover van alle data bases in het exemplaar. Wanneer een groep wordt gemaakt, wordt elke data base in het exemplaar automatisch geo-gerepliceerd naar het secundaire exemplaar. U kunt geen failover-groepen gebruiken om een gedeeltelijke failover van een subset van de data bases te initiëren.

  > [!IMPORTANT]
  > Als een Data Base uit het primaire exemplaar wordt verwijderd, wordt deze ook automatisch neergezet op het secundaire geo-exemplaar.

- **Listener voor read-write gebruiken voor OLTP-workload**

  Bij het uitvoeren van OLTP-bewerkingen, gebruikt u `<fog-name>.zone_id.database.windows.net` als de server-URL en worden de verbindingen automatisch naar de primaire verbinding geleid. Deze URL wordt niet gewijzigd na de failover. De failover omvat het bijwerken van de DNS-record, zodat de client verbindingen alleen worden omgeleid naar de nieuwe primaire server nadat de DNS-cache van de client is vernieuwd. Omdat het secundaire exemplaar de DNS-zone met de primaire share deelt, kan de client toepassing opnieuw verbinding maken met het certificaat met behulp van dezelfde SAN-certificaten.

- **Rechtstreeks verbinding maken met geo-gerepliceerd secundair voor alleen-lezen query's**

  Als u een logisch geïsoleerde alleen-lezen werk belasting hebt die tolerant is voor bepaalde verouderde gegevens, kunt u de secundaire data base in de toepassing gebruiken. Als u rechtstreeks verbinding wilt maken met het secundaire geo-gerepliceerde secundair, gebruikt u `server.secondary.zone_id.database.windows.net` als de server-URL en wordt de verbinding rechtstreeks gemaakt naar het geo-gerepliceerde secundair.

  > [!NOTE]
  > In bepaalde service lagen ondersteunt Azure SQL Database het gebruik van [alleen-lezen replica's](sql-database-read-scale-out.md) om werk belastingen voor alleen-lezen query's te verdelen met behulp van de capaciteit van één alleen-lezen replica en met behulp van de `ApplicationIntent=ReadOnly`-para meter in de Connection String. Wanneer u een secundaire geo-replicatie hebt geconfigureerd, kunt u deze mogelijkheid gebruiken om verbinding te maken met een alleen-lezen replica op de primaire locatie of op de geo-gerepliceerde locatie.
  > - Gebruik `<fog-name>.zone_id.database.windows.net`om verbinding te maken met een alleen-lezen replica op de primaire locatie.
  > - Gebruik `<fog-name>.secondary.zone_id.database.windows.net`om verbinding te maken met een alleen-lezen replica op de secundaire locatie.

- **Voor bereidingen voor prestatie vermindering**

  De beslissing van SQL-failover is onafhankelijk van de rest van de toepassing of andere services die worden gebruikt. De toepassing kan worden ' Mixed ' met enkele onderdelen in één regio en een deel van een andere. Zorg ervoor dat u de redundante toepassing implementeert in de DR-regio en volg deze [richt lijnen voor netwerk beveiliging](#failover-groups-and-network-security)om de degradatie te voor komen.

- **Voorbereiden op gegevens verlies**

  Als er een storing wordt gedetecteerd, wordt door SQL automatisch een failover voor lezen/schrijven geactiveerd als er geen gegevens verloren zijn op het beste van de kennis. Anders wordt gewacht op de periode die u hebt opgegeven door `GracePeriodWithDataLossHours`. Als u `GracePeriodWithDataLossHours`hebt opgegeven, moet u voor bereid zijn op gegevens verlies. Over het algemeen is het voor de beschik baarheid van Azure van belang. Als u geen gegevens verlies kunt veroorloven, moet u toegevoegd instellen op een voldoende groot getal, zoals 24 uur.

  De DNS-update van de listener voor lezen-schrijven gebeurt onmiddellijk nadat de failover is gestart. Deze bewerking resulteert niet in gegevens verlies. Het proces van het wisselen van database rollen kan echter tot vijf minuten duren onder normale omstandigheden. Totdat de data bases in het nieuwe primaire exemplaar zijn voltooid, hebben ze nog steeds het kenmerk alleen-lezen. Als failover wordt gestart met behulp van Power shell, is de gehele bewerking synchroon. Als het wordt gestart met behulp van de Azure Portal, wordt in de gebruikers interface de voltooiings status weer geven. Als deze is gestart met behulp van de REST API, gebruikt u het polling mechanisme van de standaard Azure Resource Manager om te controleren of het is voltooid.

  > [!IMPORTANT]
  > Gebruik hand matige failover van groep om Primaries terug te zetten naar de oorspronkelijke locatie. Wanneer de storing die de failover veroorzaakte, wordt verholpen, kunt u de primaire data bases naar de oorspronkelijke locatie verplaatsen. Als u dit wilt doen, moet u de hand matige failover van de groep initiëren.

- **Bekende beperkingen van failover-groepen erkennen**

  De naam van een Data Base wordt niet ondersteund voor exemplaren in een failovergroep. U moet een failovergroep tijdelijk verwijderen om de naam van een Data Base te kunnen wijzigen.

## <a name="failover-groups-and-network-security"></a>Failover-groepen en netwerk beveiliging

Voor sommige toepassingen moeten de beveiligings regels vereisen dat het netwerk toegang tot de gegevenslaag wordt beperkt tot een specifiek onderdeel of andere onderdelen, zoals een VM, webservice, enzovoort. Deze vereiste vormt enkele uitdagingen voor het ontwerpen van bedrijfs continuïteit en het gebruik van de failover-groepen. Houd rekening met de volgende opties bij het implementeren van dergelijke beperkte toegang.

### <a name="using-failover-groups-and-virtual-network-rules"></a>Failover-groepen en regels voor virtuele netwerken gebruiken

Als u [Virtual Network Service-eind punten en-regels](sql-database-vnet-service-endpoint-rule-overview.md) gebruikt om de toegang tot uw SQL database te beperken, moet u er rekening mee houden dat elk Virtual Network Service-eind punt alleen van toepassing is op één Azure-regio. Met het eind punt kunnen andere regio's geen communicatie van het subnet accepteren. Daarom kunnen alleen de client toepassingen die in dezelfde regio worden geïmplementeerd, verbinding maken met de primaire data base. Omdat de failover resulteert in de SQL-Client sessies die worden omgeleid naar een server in een andere (secundaire) regio, zullen deze sessies mislukken als ze afkomstig zijn van een client buiten die regio. Het automatische failoverbeleid kan daarom niet worden ingeschakeld als de deelnemende servers zijn opgenomen in de Virtual Network-regels. Voer de volgende stappen uit om hand matige failover te ondersteunen:

1. Richt de redundante kopieën in van de front-end onderdelen van uw toepassing (webservice, virtuele machines, enzovoort) in de secundaire regio
2. De regels voor het [virtuele netwerk](sql-database-vnet-service-endpoint-rule-overview.md) afzonderlijk configureren voor de primaire en secundaire server
3. De [front-end-failover inschakelen met behulp van de Traffic Manager-configuratie](sql-database-designing-cloud-solutions-for-disaster-recovery.md#scenario-1-using-two-azure-regions-for-business-continuity-with-minimal-downtime)
4. Start de hand matige failover wanneer de onderbreking wordt gedetecteerd. Deze optie is geoptimaliseerd voor de toepassingen die een consistente latentie vereisen tussen de front-end en de gegevenslaag en ondersteuning bieden voor herstel wanneer een front-end, een gegevenslaag of beide worden beïnvloed door de storing.

> [!NOTE]
> Als u de **alleen-lezen listener** gebruikt om taken te verdelen over een alleen-lezen werk belasting, moet u ervoor zorgen dat deze werk belasting wordt uitgevoerd in een virtuele machine of in een andere resource in de secundaire regio zodat deze verbinding kan maken met de secundaire data base.

### <a name="using-failover-groups-and-sql-database-firewall-rules"></a>Failover-groepen en firewall regels voor SQL database gebruiken

Als failover is vereist voor uw bedrijfs continuïteits plan met behulp van groepen met automatische failover, kunt u de toegang tot uw SQL database beperken met behulp van de traditionele firewall regels. Voer de volgende stappen uit om automatische failover te ondersteunen:

1. [Een openbaar IP-adres maken](../virtual-network/virtual-network-public-ip-address.md#create-a-public-ip-address)
2. [Maak een open bare Load Balancer](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-a-basic-load-balancer) en wijs hieraan het open bare IP-adres toe.
3. [Een virtueel netwerk en de virtuele machines](../load-balancer/quickstart-create-basic-load-balancer-portal.md#create-back-end-servers) voor uw front-end-onderdelen maken
4. [Netwerk beveiligings groep maken](../virtual-network/security-overview.md) en binnenkomende verbindingen configureren.
5. Zorg ervoor dat de uitgaande verbindingen met Azure SQL database zijn geopend met behulp van SQL- [service Tags](../virtual-network/security-overview.md#service-tags).
6. Maak een [SQL database firewall regel](sql-database-firewall-configure.md) om binnenkomend verkeer toe te staan van het open bare IP-adres dat u in stap 1 hebt gemaakt.

Zie [uitgaande verbindingen van Load Balancer](../load-balancer/load-balancer-outbound-connections.md)voor meer informatie over het configureren van uitgaande toegang en welk IP-adres moet worden gebruikt in de firewall regels.

De bovenstaande configuratie zorgt ervoor dat de automatische failover geen verbindingen van de front-end-onderdelen blokkeert en er wordt van uitgegaan dat de toepassing de langere latentie tussen de front-end en de gegevenslaag kan verdragen.

> [!IMPORTANT]
> Om de bedrijfs continuïteit voor regionale uitval te garanderen, moet u ervoor zorgen dat u geografische redundantie voor zowel de front-end-onderdelen als de data bases hebt.

## <a name="enabling-geo-replication-between-managed-instances-and-their-vnets"></a>Geo-replicatie tussen beheerde instanties en hun VNets inschakelen

Wanneer u een failovergroep instelt tussen de primaire en secundaire beheerde instanties in twee verschillende regio's, wordt elk exemplaar geïsoleerd met behulp van een onafhankelijk virtueel netwerk. Als u replicatie verkeer tussen deze VNets wilt toestaan, moet u ervoor zorgen dat aan deze vereisten wordt voldaan:

1. De twee beheerde exemplaren moeten zich in verschillende Azure-regio's bevindt.
1. De twee beheerde exemplaren moeten dezelfde servicelaag hebben en dezelfde opslag grootte hebben.
1. Uw secundaire beheerde exemplaar moet leeg zijn (geen gebruikers databases).
1. De virtuele netwerken die worden gebruikt door de beheerde instanties moeten worden verbonden via een [VPN gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) of Express route. Wanneer twee virtuele netwerken verbinding maken via een on-premises netwerk, zorg er dan voor dat er geen firewall regel is die poorten 5022 en 11000-11999 blokkeert. Globale VNet-peering wordt niet ondersteund.
1. De twee VNets van het beheerde exemplaar kunnen geen overlappende IP-adressen hebben.
1. U moet uw netwerk beveiligings groepen (NSG) zodanig instellen dat de poorten 5022 en het bereik 11000 ~ 12000 zijn geopend als inkomend en uitgaand voor verbindingen van het andere beheerde subnet. Dit is om replicatie verkeer tussen de instanties toe te staan

   > [!IMPORTANT]
   > Onjuist geconfigureerde NSG-beveiligings regels leiden tot vastgelopen database Kopieer bewerkingen.

1. Het secundaire exemplaar is geconfigureerd met de juiste DNS-zone-ID. DNS-zone is een eigenschap van een beheerd exemplaar en een virtueel cluster en de bijbehorende ID is opgenomen in het adres van de hostnaam. De zone-ID wordt gegenereerd als een wille keurige teken reeks wanneer het eerste beheerde exemplaar wordt gemaakt in elk VNet en dezelfde ID wordt toegewezen aan alle andere exemplaren in hetzelfde subnet. Zodra het is toegewezen, kan de DNS-zone niet worden gewijzigd. Beheerde instanties die zijn opgenomen in dezelfde failovergroep, moeten de DNS-zone delen. Dit doet u door de zone-ID van het primaire exemplaar door te geven als waarde voor de para meter DnsZonePartner bij het maken van het secundaire exemplaar. 

   > [!NOTE]
   > Zie [een beheerd exemplaar toevoegen aan een failovergroep](sql-database-managed-instance-failover-group-tutorial.md)voor een gedetailleerde zelf studie over het configureren van failover-groepen met een beheerd exemplaar.

## <a name="upgrading-or-downgrading-a-primary-database"></a>Een primaire data base bijwerken of downgrade uitvoeren

U kunt een upgrade of downgrade van een primaire Data Base naar een andere reken grootte (binnen dezelfde servicelaag, niet tussen Algemeen en Bedrijfskritiek) zonder de verbinding van secundaire data bases te verbreken. Wanneer u een upgrade uitvoert, raden we u aan om eerst alle secundaire data bases bij te werken en vervolgens een upgrade uit te voeren van de primaire. Wanneer u een downgrade uitvoert, maakt u de volg orde ongedaan: downgradet u eerst de primaire data base. Wanneer u de data base bijwerkt of downgradet naar een andere servicelaag, wordt deze aanbeveling afgedwongen.

Deze volg orde wordt aangeraden om het probleem te voor komen waarbij de secundaire bij een lagere SKU overbelast is en opnieuw moet worden geseed tijdens een upgrade of downgrade proces. U kunt het probleem ook vermijden door de primaire alleen-lezen te maken, tegen kosten die van invloed zijn op alle werk belastingen voor lezen/schrijven op basis van de primaire.

> [!NOTE]
> Als u een secundaire Data Base hebt gemaakt als onderdeel van de configuratie van de failovergroep, wordt het niet aanbevolen de secundaire data base te downgradeen. Zo zorgt u ervoor dat uw gegevenslaag voldoende capaciteit heeft om uw normale werk belasting te verwerken nadat de failover is geactiveerd.

## <a name="preventing-the-loss-of-critical-data"></a>Het verlies van kritieke gegevens voor komen

Vanwege de hoge latentie van Wide Area Networks, gebruikt doorlopende kopieën een mechanisme voor asynchrone replicatie. Asynchrone replicatie maakt enkele gegevens verlies onvermijdbaar als er een fout optreedt. Voor sommige toepassingen is het echter mogelijk dat er geen gegevens verloren gaan. Een ontwikkelaar van een toepassing kan deze essentiële updates beveiligen door de [sp_wait_for_database_copy_sync](/sql/relational-databases/system-stored-procedures/active-geo-replication-sp-wait-for-database-copy-sync) -systeem procedure onmiddellijk aan te roepen nadat de trans actie is doorgevoerd. Het aanroepen van `sp_wait_for_database_copy_sync` blokkeert de aanroepende thread totdat de laatste vastgelegde trans actie is verzonden naar de secundaire data base. Er wordt echter niet gewacht tot de verzonden trans acties moeten worden herhaald en op de secundaire moeten worden doorgevoerd. `sp_wait_for_database_copy_sync` ligt binnen het bereik van een specifieke koppeling voor doorlopende kopieën. Elke gebruiker met de verbindings rechten voor de primaire data base kan deze procedure aanroepen.

> [!NOTE]
> `sp_wait_for_database_copy_sync` voor komt gegevens verlies na een failover, maar biedt geen volledige synchronisatie voor lees toegang. De vertraging veroorzaakt door een `sp_wait_for_database_copy_sync` procedure aanroep kan aanzienlijk zijn en is afhankelijk van de grootte van het transactie logboek op het moment van de aanroep.

## <a name="failover-groups-and-point-in-time-restore"></a>Failover-groepen en herstel naar een bepaald tijdstip

Zie Point-in-time [herstel (PITR)](sql-database-recovery-using-backups.md#point-in-time-restore)voor informatie over het gebruik van herstel naar een bepaald tijdstip met failover-groepen.

## <a name="programmatically-managing-failover-groups"></a>Programmatisch beheer van failover-groepen

Zoals eerder besproken, kunnen automatische failover-groepen en actieve geo-replicatie ook programmatisch worden beheerd met behulp van Azure PowerShell en de REST API. De volgende tabellen bevatten een beschrijving van de beschik bare opdrachten. Actieve geo-replicatie bevat een set Azure Resource Manager Api's voor beheer, met inbegrip van de [Azure SQL database-rest API](https://docs.microsoft.com/rest/api/sql/) en [Azure PowerShell-cmdlets](https://docs.microsoft.com/powershell/azure/overview). Deze Api's vereisen het gebruik van resource groepen en bieden beveiliging op basis van rollen (RBAC). Zie [Access Control op basis van rollen](../role-based-access-control/overview.md)voor meer informatie over het implementeren van toegangs rollen.

# <a name="powershelltabazure-powershell"></a>[PowerShell](#tab/azure-powershell)

### <a name="manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>SQL database failover beheren met afzonderlijke data bases en elastische Pools

| Cmdlet | Beschrijving |
| --- | --- |
| [New-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/new-azsqldatabasefailovergroup) |Met deze opdracht maakt u een failovergroep en registreert u deze op de primaire en secundaire servers|
| [Remove-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/remove-azsqldatabasefailovergroup) | Hiermee wordt de failovergroep van de server verwijderd en worden alle secundaire data bases verwijderd die zijn opgenomen in de groep |
| [Get-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/get-azsqldatabasefailovergroup) | Hiermee wordt de configuratie van de failovergroep opgehaald |
| [Set-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/set-azsqldatabasefailovergroup) |Hiermee wijzigt u de configuratie van de failovergroep |
| [Switch-AzSqlDatabaseFailoverGroup](/powershell/module/az.sql/switch-azsqldatabasefailovergroup) | De failover van de failovergroep naar de secundaire server wordt geactiveerd |
| [Add-AzSqlDatabaseToFailoverGroup](/powershell/module/az.sql/add-azsqldatabasetofailovergroup)|Voegt een of meer data bases toe aan een Azure SQL Database-failovergroep|

### <a name="manage-sql-database-failover-groups-with-managed-instances"></a>SQL database failover-groepen beheren met beheerde exemplaren

| Cmdlet | Beschrijving |
| --- | --- |
| [New-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/new-azsqldatabaseinstancefailovergroup) |Met deze opdracht maakt u een failovergroep en registreert u deze op de primaire en secundaire servers|
| [Set-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/set-azsqldatabaseinstancefailovergroup) |Hiermee wijzigt u de configuratie van de failovergroep|
| [Get-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/get-azsqldatabaseinstancefailovergroup) |Hiermee wordt de configuratie van de failovergroep opgehaald|
| [Switch-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/switch-azsqldatabaseinstancefailovergroup) |De failover van de failovergroep naar de secundaire server wordt geactiveerd|
| [Remove-AzSqlDatabaseInstanceFailoverGroup](/powershell/module/az.sql/remove-azsqldatabaseinstancefailovergroup) | Hiermee verwijdert u een failovergroep|

# <a name="azure-clitabazure-cli"></a>[Azure CLI](#tab/azure-cli)

### <a name="manage-sql-database-failover-with-single-databases-and-elastic-pools"></a>SQL database failover beheren met afzonderlijke data bases en elastische Pools

| Opdracht | Beschrijving |
| --- | --- |
| [AZ SQL failover-Group Create](/cli/azure/sql/failover-group#az-sql-failover-group-create) |Met deze opdracht maakt u een failovergroep en registreert u deze op de primaire en secundaire servers|
| [AZ SQL failover-Group Delete](/cli/azure/sql/failover-group#az-sql-failover-group-delete) | Hiermee wordt de failovergroep van de server verwijderd en worden alle secundaire data bases verwijderd die zijn opgenomen in de groep |
| [AZ SQL failover-Group show](/cli/azure/sql/failover-group#az-sql-failover-group-show) | Hiermee wordt de configuratie van de failovergroep opgehaald |
| [AZ SQL failover-Group Update](/cli/azure/sql/failover-group#az-sql-failover-group-update) |Hiermee wijzigt u de configuratie van de failovergroep en/of voegt u een of meer data bases toe aan een Azure SQL Database failover-groep|
| [AZ SQL failover-groeps Set-Primary](/cli/azure/sql/failover-group#az-sql-failover-group-set-primary) | De failover van de failovergroep naar de secundaire server wordt geactiveerd |

### <a name="manage-sql-database-failover-groups-with-managed-instances"></a>SQL database failover-groepen beheren met beheerde exemplaren

| Opdracht | Beschrijving |
| --- | --- |
| [AZ SQL Instance-failover-groep Create](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-create) | Met deze opdracht maakt u een failovergroep en registreert u deze op de primaire en secundaire servers|
| [AZ SQL Instance-update van failover-groep](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-update) | Hiermee wijzigt u de configuratie van de failovergroep|
| [AZ SQL Instance-failover-Group show](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-show) | Hiermee wordt de configuratie van de failovergroep opgehaald|
| [AZ SQL Instance-failover-groep Set-Primary](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-set-primary) | De failover van de failovergroep naar de secundaire server wordt geactiveerd|
| [AZ SQL Instance-failover-groep verwijderen](/cli/azure/sql/instance-failover-group#az-sql-instance-failover-group-delete) | Hiermee verwijdert u een failovergroep |

* * *

> [!IMPORTANT]
> Zie [een failover-groep configureren en failover voor één data base](scripts/sql-database-add-single-db-to-failover-group-powershell.md)voor een voorbeeld script.

### <a name="rest-api-manage-sql-database-failover-groups-with-single-and-pooled-databases"></a>REST API: SQL database-failover-groepen beheren met één en gepoolde data bases

| API | Beschrijving |
| --- | --- |
| [Failovergroep maken of bijwerken](https://docs.microsoft.com/rest/api/sql/failovergroups/createorupdate) | Hiermee wordt een failovergroep gemaakt of bijgewerkt |
| [Failovergroep verwijderen](https://docs.microsoft.com/rest/api/sql/failovergroups/delete) | Hiermee wordt de failover-groep van de server verwijderd |
| [Failover (gepland)](https://docs.microsoft.com/rest/api/sql/failovergroups/failover) | Failover van de huidige primaire server naar deze server. |
| [Forceren van gegevens verlies door failover toestaan](https://docs.microsoft.com/rest/api/sql/failovergroups/forcefailoverallowdataloss) |ails van de huidige primaire server naar deze server. Deze bewerking kan leiden tot verlies van gegevens. |
| [Failovergroep ophalen](https://docs.microsoft.com/rest/api/sql/failovergroups/get) | Hiermee wordt een failovergroep opgehaald. |
| [Failover-groepen weer geven per server](https://docs.microsoft.com/rest/api/sql/failovergroups/listbyserver) | Geeft een lijst van de failover-groepen op een server. |
| [Failovergroep bijwerken](https://docs.microsoft.com/rest/api/sql/failovergroups/update) | Hiermee wordt een failovergroep bijgewerkt. |

### <a name="rest-api-manage-failover-groups-with-managed-instances"></a>REST API: failover-groepen beheren met beheerde exemplaren

| API | Beschrijving |
| --- | --- |
| [Failovergroep maken of bijwerken](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/createorupdate) | Hiermee wordt een failovergroep gemaakt of bijgewerkt |
| [Failovergroep verwijderen](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/delete) | Hiermee wordt de failover-groep van de server verwijderd |
| [Failover (gepland)](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/failover) | Failover van de huidige primaire server naar deze server. |
| [Forceren van gegevens verlies door failover toestaan](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/forcefailoverallowdataloss) |ails van de huidige primaire server naar deze server. Deze bewerking kan leiden tot verlies van gegevens. |
| [Failovergroep ophalen](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/get) | Hiermee wordt een failovergroep opgehaald. |
| [Failover-groepen weer geven-lijst per locatie](https://docs.microsoft.com/rest/api/sql/instancefailovergroups/listbylocation) | Hiermee worden de failover-groepen op een locatie weer gegeven. |

## <a name="next-steps"></a>Volgende stappen

- Zie voor gedetailleerde zelf studies
    - [Eén data base aan een failovergroep toevoegen](sql-database-single-database-failover-group-tutorial.md)
    - [Elastische pool toevoegen aan een failovergroep](sql-database-elastic-pool-failover-group-tutorial.md)
    - [Een beheerd exemplaar toevoegen aan een failovergroep](sql-database-managed-instance-failover-group-tutorial.md)
- Zie voor voorbeeld scripts:
  - [Power shell gebruiken voor het configureren van actieve geo-replicatie voor één data base in Azure SQL Database](scripts/sql-database-setup-geodr-and-failover-database-powershell.md)
  - [Power shell gebruiken voor het configureren van actieve geo-replicatie voor een gegroepeerde Data base in Azure SQL Database](scripts/sql-database-setup-geodr-and-failover-pool-powershell.md)
  - [Power shell gebruiken om een Azure SQL Database afzonderlijke data base toe te voegen aan een failovergroep](scripts/sql-database-add-single-db-to-failover-group-powershell.md)
- Zie [overzicht van bedrijfs continuïteit](sql-database-business-continuity.md) voor een overzicht en scenario's voor bedrijfs continuïteit
- Zie [SQL database automatische back-ups](sql-database-automated-backups.md)voor meer informatie over Azure SQL database automatische back-ups.
- Zie [een Data Base herstellen vanuit de door de service geïnitieerde back-ups](sql-database-recovery-using-backups.md)voor meer informatie over het gebruik van automatische back-ups voor herstel.
- Zie [SQL database beveiliging na nood herstel](sql-database-geo-replication-security-config.md)voor meer informatie over de verificatie vereisten voor een nieuwe primaire server en data base.
