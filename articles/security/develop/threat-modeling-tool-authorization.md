---
title: Autorisatie-Microsoft Threat Modeling Tool-Azure | Microsoft Docs
description: oplossingen voor bedreigingen die worden blootgesteld aan de Threat Modeling Tool
services: security
documentationcenter: na
author: jegeib
manager: jegeib
editor: jegeib
ms.assetid: na
ms.service: security
ms.subservice: security-develop
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/07/2017
ms.author: jegeib
ms.openlocfilehash: 75bbce0f1e9787e55880ccac80dacb5457e1f2c0
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68728371"
---
# <a name="security-frame-authorization--mitigations"></a>Beveiligings frame: Autorisatie | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Grens van computer vertrouwen** | <ul><li>[Zorg ervoor dat de juiste Acl's zijn geconfigureerd om onbevoegde toegang tot gegevens op het apparaat te beperken](#acl-restricted-access)</li><li>[Zorg ervoor dat gevoelige gebruikersspecifieke toepassings inhoud wordt opgeslagen in de map gebruikers profiel](#sensitive-directory)</li><li>[Zorg ervoor dat de geïmplementeerde toepassingen met de minste bevoegdheden worden uitgevoerd](#deployed-privileges)</li></ul> |
| **Webtoepassing** | <ul><li>[Volg orde van opeenvolgende stappen afdwingen bij het verwerken van bedrijfs logica stromen](#sequential-logic)</li><li>[Implementeer het mechanisme voor het beperken van de frequentie om opsomming te voor komen](#rate-enumeration)</li><li>[Zorg ervoor dat de juiste autorisatie plaatsvindt en dat het beginsel van de minimale bevoegdheden wordt gevolgd](#principle-least-privilege)</li><li>[Beslissingen met betrekking tot bedrijfs logica en bron toegang mogen niet worden gebaseerd op de para meters voor de inkomende aanvraag](#logic-request-parameters)</li><li>[Zorg ervoor dat de inhoud en bronnen niet worden besteld of toegankelijk via geforceerde-Browsing](#enumerable-browsing)</li></ul> |
| **Database** | <ul><li>[Zorg ervoor dat accounts met minimale bevoegdheden worden gebruikt om verbinding te maken met de database server](#privileged-server)</li><li>[Implementeer beveiliging op RIJNIVEAU om te voor komen dat tenants toegang krijgen tot de gegevens van elkaar](#rls-tenants)</li><li>[De sysadmin-rol mag alleen geldige benodigde gebruikers hebben](#sysadmin-users)</li></ul> |
| **IoT-Cloud gateway** | <ul><li>[Verbinding maken met de Cloud gateway met behulp van de tokens met minimale privileges](#cloud-least-privileged)</li></ul> |
| **Azure Event Hub** | <ul><li>[Een SAS-sleutel voor alleen-lezen toegang gebruiken voor het genereren van de tokens van het apparaat](#sendonly-sas)</li><li>[Gebruik geen toegangs tokens die directe toegang bieden tot de Event hub](#access-tokens-hub)</li><li>[Verbinding maken met Event hub met SAS-sleutels waarvoor de minimale machtigingen zijn vereist](#sas-minimum-permissions)</li></ul> |
| **Azure Document DB** | <ul><li>[Resource tokens gebruiken om waar mogelijk verbinding te maken met Azure Cosmos DB](#resource-docdb)</li></ul> |
| **Azure-vertrouwens grens** | <ul><li>[Verfijnd toegangs beheer voor een Azure-abonnement met RBAC inschakelen](#grained-rbac)</li></ul> |
| **Grens van Service Fabric vertrouwen** | <ul><li>[De toegang van clients tot cluster bewerkingen beperken met RBAC](#cluster-rbac)</li></ul> |
| **Dynamics CRM** | <ul><li>[Beveiligings modellering uitvoeren en beveiliging op veld niveau gebruiken indien vereist](#modeling-field)</li></ul> |
| **Dynamics CRM-Portal** | <ul><li>[Beveiligings modellen van portal-accounts uitvoeren, zodat het beveiligings model voor de portal verschilt van de rest van CRM](#portal-security)</li></ul> |
| **Azure Storage** | <ul><li>[Verleen verfijnde machtigingen voor een bereik van entiteiten in azure Table Storage](#permission-entities)</li><li>[Op rollen gebaseerde Access Control (RBAC) inschakelen voor een Azure-opslag account met behulp van Azure Resource Manager](#rbac-azure-manager)</li></ul> |
| **Mobiele client** | <ul><li>[Impliciete detectie van jailbreak of DFS implementeren](#rooting-detection)</li></ul> |
| **WCF** | <ul><li>[Zwakke klasse-verwijzing in WCF](#weak-class-wcf)</li><li>[WCF-autorisatie beheer implementeren](#wcf-authz)</li></ul> |
| **Web-API** | <ul><li>[Het juiste autorisatie mechanisme implementeren in ASP.NET Web-API](#authz-aspnet)</li></ul> |
| **IoT-apparaat** | <ul><li>[Verificatie controles uitvoeren op het apparaat als dit verschillende acties ondersteunt waarvoor verschillende machtigings niveaus zijn vereist](#device-permission)</li></ul> |
| **IoT-veld Gateway** | <ul><li>[Verificatie controles uitvoeren in de veld Gateway als deze verschillende acties ondersteunt waarvoor verschillende machtigings niveaus zijn vereist](#field-permission)</li></ul> |

## <a id="acl-restricted-access"></a>Zorg ervoor dat de juiste Acl's zijn geconfigureerd om onbevoegde toegang tot gegevens op het apparaat te beperken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van computer vertrouwen | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Zorg ervoor dat de juiste Acl's zijn geconfigureerd om onbevoegde toegang tot gegevens op het apparaat te beperken|

## <a id="sensitive-directory"></a>Zorg ervoor dat gevoelige gebruikersspecifieke toepassings inhoud wordt opgeslagen in de map gebruikers profiel

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van computer vertrouwen | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Zorg ervoor dat gevoelige gebruikersspecifieke toepassings inhoud wordt opgeslagen in de map van het gebruikers profiel. Dit is om te voor komen dat meerdere gebruikers van de computer toegang hebben tot de gegevens van elkaar.|

## <a id="deployed-privileges"></a>Zorg ervoor dat de geïmplementeerde toepassingen met de minste bevoegdheden worden uitgevoerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van computer vertrouwen | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Zorg ervoor dat de geïmplementeerde toepassing met de minste bevoegdheden is uitgevoerd. |

## <a id="sequential-logic"></a>Volg orde van opeenvolgende stappen afdwingen bij het verwerken van bedrijfs logica stromen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Als u wilt controleren of deze fase is uitgevoerd door een authentieke gebruiker, wilt u de toepassing afdwingen om alleen bedrijfs logica stromen in sequentiële volg orde te verwerken, waarbij alle stappen worden verwerkt in realistische menselijke tijd en niet zijn verwerkt in de juiste volg orde, overgeslagen stappen , verwerkte stappen van een andere gebruiker of te snel ingediende trans acties.|

## <a id="rate-enumeration"></a>Implementeer het mechanisme voor het beperken van de frequentie om opsomming te voor komen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Zorg ervoor dat de gevoelige id's wille keurig zijn. Implementeer CAPTCHA Control op anonieme pagina's. Zorg ervoor dat de fout en uitzonde ring specifieke gegevens niet mogen onthullen|

## <a id="principle-least-privilege"></a>Zorg ervoor dat de juiste autorisatie plaatsvindt en dat het beginsel van de minimale bevoegdheden wordt gevolgd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | <p>Het principe betekent dat een gebruikers account alleen de bevoegdheden geeft die essentieel zijn voor de werking van die gebruikers. Een back-upgebruiker hoeft bijvoorbeeld geen software te installeren: daarom heeft de back-upgebruiker alleen rechten voor het uitvoeren van back-up-en back-uptoepassingen. Alle andere bevoegdheden, zoals het installeren van nieuwe software, worden geblokkeerd. Het principe geldt ook voor een personal computer gebruiker die meestal in een normaal gebruikers account werkt en een Privileged, met een wacht woord beveiligd account (dat wil zeggen, een super gebruiker) wordt geopend wanneer de situatie dit absoluut vereist. </p><p>Dit principe kan ook worden toegepast op uw webtoepassingen. In plaats van alleen afhankelijk te zijn van op rollen gebaseerde verificatie methoden met behulp van sessies, willen we in plaats daarvan machtigingen toewijzen aan gebruikers met behulp van een verificatie systeem op basis van een Data Base. We gebruiken nog steeds sessies om te bepalen of de gebruiker correct is aangemeld, maar nu alleen in plaats van de gebruiker met een specifieke rol toe te wijzen om te controleren welke acties hij of zij privileged heeft om uit te voeren op het systeem. Ook een Big Pro van deze methode is, wanneer een gebruiker aan minder privileges moet worden toegewezen, de wijzigingen worden toegepast op het moment dat de toewijzing niet afhankelijk is van de sessie die u eerst zou moeten verlopen.</p>|

## <a id="logic-request-parameters"></a>Beslissingen met betrekking tot bedrijfs logica en bron toegang mogen niet worden gebaseerd op de para meters voor de inkomende aanvraag

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Wanneer u controleert of een gebruiker is beperkt tot het controleren van bepaalde gegevens, moeten de toegangs beperkingen aan de server zijde worden verwerkt. De gebruikers-id moet worden opgeslagen in een sessie variabele bij het aanmelden en moet worden gebruikt voor het ophalen van gebruikers gegevens uit de data base |

### <a name="example"></a>Voorbeeld
```SQL
SELECT data 
FROM personaldata 
WHERE userID=:id < - session var 
```
Een mogelijke aanvaller kan nu niet knoeien en de bewerking van de toepassing wijzigen omdat de id voor het ophalen van de gegevens aan de server zijde wordt verwerkt.

## <a id="enumerable-browsing"></a>Zorg ervoor dat de inhoud en bronnen niet worden besteld of toegankelijk via geforceerde-Browsing

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | <p>Gevoelige statische en configuratie bestanden mogen niet worden bewaard in de hoofdmap van het web. Voor inhoud die niet openbaar moet zijn, moeten de juiste toegangs controles worden toegepast of moeten de inhoud zelf worden verwijderd.</p><p>Daarnaast wordt geforceerde Browse doorgaans gecombineerd met de technieken voor het verzamelen van gegevens door te proberen om mappen en bestanden op een server te inventariseren. Kwaadwillende personen kunnen controleren op alle variaties van de meeste bestaande bestanden. Een zoek opdracht voor het zoeken naar wacht woorden omvat bijvoorbeeld bestanden zoals psswd. txt, password. htm, password. dat en andere variaties.</p><p>Om dit te verhelpen, moet u de mogelijkheden voor detectie van beveiligings pogingen opnemen.</p>|

## <a id="privileged-server"></a>Zorg ervoor dat accounts met minimale bevoegdheden worden gebruikt om verbinding te maken met de database server

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [SQL database machtigingen hiërarchie](https://msdn.microsoft.com/library/ms191465) [SQL database Beveilig bare items](https://msdn.microsoft.com/library/ms190401) |
| **Stappen** | U moet mini maal privileged accounts gebruiken om verbinding te maken met de data base. Het aanmelden van een toepassing moet worden beperkt in de-data base en moet alleen geselecteerde opgeslagen procedures uitvoeren. De aanmelding van de toepassing mag geen directe toegang tot de tabel hebben. |

## <a id="rls-tenants"></a>Implementeer beveiliging op RIJNIVEAU om te voor komen dat tenants toegang krijgen tot de gegevens van elkaar

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Sql Azure, OnPrem |
| **Eigenschappen**              | SQL-versie-V12, SQL-versie-MsSQL2016 |
| **Referentie**              | [Beveiliging op RIJNIVEAU SQL Server](https://msdn.microsoft.com/library/azure/dn765131.aspx) |
| **Stappen** | <p>Met beveiliging op rijniveau kunnen klanten de toegang tot rijen in een databasetabel beheren op basis van de kenmerken van de gebruiker die een query uitvoert (bijvoorbeeld groepslidmaatschap of uitvoeringscontext).</p><p>Beveiliging op RIJNIVEAU vereenvoudigt het ontwerp en de code ring van beveiliging in uw toepassing. Met RLS kunt u beperkingen instellen voor de toegang tot gegevens in rijen. U kunt bijvoorbeeld bepalen dat werkrollen alleen toegang hebben tot de rijen met gegevens die relevant zijn voor hun afdeling, of de toegang van klanten beperken tot de gegevens die relevant zijn voor hun bedrijf.</p><p>De logica van de toegangs beperking bevindt zich in de database laag, in plaats van dat de gegevens in een andere toepassingslaag worden verwijderd. Het database systeem past de toegangs beperkingen telkens toe wanneer de toegang tot gegevens vanuit een wille keurige laag wordt gestart. Dit maakt het beveiligings systeem betrouwbaarder en betrouwbaarder door de surface area van het beveiligings systeem te verminderen.</p><p>|

Houd er rekening mee dat beveiliging op rijniveau als een out-of-the-box-database functie alleen van toepassing is op SQL Server vanaf 2016 en Azure SQL database. Als de out-of-Box-functie voor beveiliging op rijniveau niet is geïmplementeerd, moet worden gegarandeerd dat gegevens toegang is beperkt met behulp van weer gaven en procedures

## <a id="sysadmin-users"></a>De sysadmin-rol mag alleen geldige benodigde gebruikers hebben

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [SQL database machtigingen hiërarchie](https://msdn.microsoft.com/library/ms191465) [SQL database Beveilig bare items](https://msdn.microsoft.com/library/ms190401) |
| **Stappen** | Leden van de vaste serverrol SysAdmin moeten zeer beperkt zijn en nooit accounts bevatten die door toepassingen worden gebruikt.  Controleer de lijst met gebruikers in de rol en Verwijder overbodige accounts|

## <a id="cloud-least-privileged"></a>Verbinding maken met de Cloud gateway met behulp van de tokens met minimale privileges

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-Cloud gateway | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | Gateway keuze-Azure IoT Hub |
| **Referentie**              | [IOT hub-Access Control](https://azure.microsoft.com/documentation/articles/iot-hub-devguide/#Security) |
| **Stappen** | Geef mini maal bevoegdheden machtigingen voor verschillende onderdelen die verbinding maken met Cloud gateway (IoT Hub). Typisch voor beeld is: het onderdeel voor Apparaatbeheer/inrichting maakt gebruik van registryread/write, gebeurtenis processor (ASA) maakt gebruik van service Connect. Afzonderlijke apparaten maken verbinding met de referenties van het apparaat|

## <a id="sendonly-sas"></a>Een SAS-sleutel voor alleen-lezen toegang gebruiken voor het genereren van de tokens van het apparaat

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Overzicht van Event Hubs verificatie en beveiligings model](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Een SAS-sleutel wordt gebruikt voor het genereren van afzonderlijke tokens voor apparaten. Een SAS-sleutel met machtigingen voor verzenden gebruiken tijdens het genereren van het token voor een bepaalde uitgever|

## <a id="access-tokens-hub"></a>Gebruik geen toegangs tokens die directe toegang bieden tot de Event hub

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Overzicht van Event Hubs verificatie en beveiligings model](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Een token dat directe toegang verleent aan de Event Hub, mag niet aan het apparaat worden verstrekt. Het gebruik van een token met minimale privileges voor het apparaat dat alleen toegang tot een uitgever geeft, helpt u bij het identificeren en op de zwarte modus als er een Rogue of aangetast apparaat is gevonden.|

## <a id="sas-minimum-permissions"></a>Verbinding maken met Event hub met SAS-sleutels waarvoor de minimale machtigingen zijn vereist

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Event Hub | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Overzicht van Event Hubs verificatie en beveiligings model](https://azure.microsoft.com/documentation/articles/event-hubs-authentication-and-security-model-overview/) |
| **Stappen** | Geef ten minste bevoegdheden aan voor verschillende back-end-toepassingen die verbinding maken met de Event hub. Genereer afzonderlijke SAS-sleutels voor elke back-endtoepassing en geef alleen de vereiste machtigingen: verzenden, ontvangen of beheren.|

## <a id="resource-docdb"></a>Resource tokens gebruiken om waar mogelijk verbinding te maken met Cosmos DB

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Document DB | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Een bron token is gekoppeld aan een Azure Cosmos DB machtigings resource en legt de relatie vast tussen de gebruiker van een Data Base en de machtiging die gebruiker heeft voor een specifieke Azure Cosmos DB toepassings resource (bijvoorbeeld verzameling, document). Gebruik altijd een bron token om toegang te krijgen tot de Azure Cosmos DB als de client niet kan worden vertrouwd met de verwerkings Master of alleen-lezen sleutels, zoals een toepassing voor eind gebruikers, zoals een mobiele of desktop-client. Gebruik hoofd sleutel of alleen-lezen sleutels van back-end-toepassingen die deze sleutels veilig kunnen opslaan.|

## <a id="grained-rbac"></a>Verfijnd toegangs beheer voor een Azure-abonnement met RBAC inschakelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure-vertrouwens grens | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Roltoewijzingen gebruiken voor het beheer van de toegang tot de resources van uw Azure-abonnement](https://azure.microsoft.com/documentation/articles/role-based-access-control-configure/)  |
| **Stappen** | Met op rollen gebaseerd toegangsbeheer (RBAC) beschikt u over geavanceerd toegangsbeheer voor Azure. Met RBAC kunt u alleen de toegangsrechten aan gebruikers verlenen die ze nodig hebben om hun taken uit te voeren.|

## <a id="cluster-rbac"></a>De toegang van clients tot cluster bewerkingen beperken met RBAC

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van Service Fabric vertrouwen | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | Omgeving-Azure |
| **Referentie**              | [Op rollen gebaseerd toegangs beheer voor Service Fabric-clients](https://azure.microsoft.com/documentation/articles/service-fabric-cluster-security-roles/) |
| **Stappen** | <p>Azure Service Fabric ondersteunt twee verschillende typen toegangs beheer voor clients die zijn verbonden met een Service Fabric cluster: beheerder en gebruiker. Met toegangs beheer kan de Cluster beheerder de toegang tot bepaalde cluster bewerkingen voor verschillende groepen gebruikers beperken, waardoor het cluster beter is beveiligd.</p><p>Beheerders hebben volledige toegang tot beheer mogelijkheden (inclusief Lees-en schrijf mogelijkheden). Gebruikers hebben standaard alleen lees toegang tot beheer mogelijkheden (bijvoorbeeld query mogelijkheden) en de mogelijkheid om toepassingen en services op te lossen.</p><p>U geeft de twee client rollen (beheerder en client) op het moment van maken van een cluster op door afzonderlijke certificaten te bieden.</p>|

## <a id="modeling-field"></a>Beveiligings modellering uitvoeren en beveiliging op veld niveau gebruiken indien vereist

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Beveiligings modellering uitvoeren en beveiliging op veld niveau gebruiken indien vereist|

## <a id="portal-security"></a>Beveiligings modellen van portal-accounts uitvoeren, zodat het beveiligings model voor de portal verschilt van de rest van CRM

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM-Portal | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Beveiligings modellen van portal-accounts uitvoeren, zodat het beveiligings model voor de portal verschilt van de rest van CRM|

## <a id="permission-entities"></a>Verleen verfijnde machtigingen voor een bereik van entiteiten in azure Table Storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | Para-tabel |
| **Referentie**              | [Toegang tot objecten in uw Azure Storage-account delegeren met SAS](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_data-plane-security) |
| **Stappen** | In bepaalde bedrijfs scenario's kan Azure Table Storage vereist zijn om gevoelige gegevens op te slaan die aan verschillende partijen zijn opgelegd. Bijvoorbeeld gevoelige gegevens die betrekking hebben op verschillende landen/regio's. In dergelijke gevallen kunnen SAS-hand tekeningen worden samengesteld door de partitie-en rijlabels op te geven, zodat een gebruiker toegang heeft tot gegevens die specifiek zijn voor een bepaald land of bepaalde regio.| 

## <a id="rbac-azure-manager"></a>Op rollen gebaseerde Access Control (RBAC) inschakelen voor een Azure-opslag account met behulp van Azure Resource Manager

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Uw opslag account beveiligen met Access Control op basis van rollen (RBAC)](https://azure.microsoft.com/documentation/articles/storage-security-guide/#management-plane-security) |
| **Stappen** | <p>Wanneer u een nieuw opslag account maakt, selecteert u een implementatie model van klassiek of Azure Resource Manager. Het klassieke model voor het maken van resources in azure staat alleen de toegang tot het abonnement toe, en is op zijn beurt het opslag account.</p><p>Met het Azure Resource Manager model plaatst u het opslag account in een resource groep en beheert u de toegang tot het beheer vlak van het betreffende opslag account met behulp van Azure Active Directory. U kunt bijvoorbeeld specifieke gebruikers de mogelijkheid geven om toegang te krijgen tot de sleutels van het opslag account, terwijl andere gebruikers informatie over het opslag account kunnen bekijken, maar geen toegang hebben tot de sleutel van het opslag account.</p>|

## <a id="rooting-detection"></a>Impliciete detectie van jailbreak of DFS implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele client | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | <p>De toepassing moet zijn eigen configuratie en gebruikers gegevens beveiligen als de telefoon is geroot of jailbroken is verbroken. Root-of-jailbroken-onderbreking impliceert ongeoorloofde toegang, wat normale gebruikers niet op hun eigen telefoons doen. Daarom moet de toepassing impliciet detectie logica hebben bij het opstarten van de toepassing, om te detecteren of de telefoon is geroot.</p><p>De detectie logica kan eenvoudig toegang krijgen tot bestanden waartoe alleen een hoofd gebruiker toegang heeft, bijvoorbeeld:</p><ul><li>/system/app/Superuser.apk</li><li>/sbin/su</li><li>/system/bin/su</li><li>/system/xbin/su</li><li>/data/local/xbin/su</li><li>/data/local/bin/su</li><li>/system/sd/xbin/su</li><li>/system/bin/failsafe/su</li><li>/data/local/su</li></ul><p>Als de toepassing toegang heeft tot een van deze bestanden, ziet u dat de toepassing wordt uitgevoerd als hoofd gebruiker.</p>|

## <a id="weak-class-wcf"></a>Zwakke klasse-verwijzing in WCF

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Algemeen, NET Framework 3 |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Konink rijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Stappen** | <p>Het systeem gebruikt een zwakke klasse-verwijzing, waardoor een aanvaller niet-geautoriseerde code kan uitvoeren. Het programma verwijst naar een door de gebruiker gedefinieerde klasse die niet uniek is geïdentificeerd. Wanneer .NET deze zwakst geïdentificeerde klasse laadt, zoekt het laad programma voor het CLR-type naar de klasse op de volgende locaties in de opgegeven volg orde:</p><ol><li>Als de assembly van het type bekend is, doorzoekt de loader de omleidings locaties van het configuratie bestand, GAC, de huidige assembly met configuratie-informatie en de map Application base</li><li>Als de assembly onbekend is, zoekt het laad programma de huidige assembly, mscorlib en de locatie die wordt geretourneerd door de gebeurtenis-handler TypeResolve</li><li>Deze CLR-Zoek volgorde kan worden gewijzigd met hooks, zoals het type door sturen en de AppDomain. TypeResolve-gebeurtenis</li></ol><p>Als een aanvaller de CLR-Zoek volgorde exploiteert door een alternatieve klasse te maken met dezelfde naam en deze te plaatsen op een andere locatie waar de CLR eerst wordt geladen, wordt de door de aanvaller geleverde code per ongeluk uitgevoerd</p>|

### <a name="example"></a>Voorbeeld
Het `<behaviorExtensions/>` element van het WCF-configuratie bestand hieronder geeft WCF opdracht om een aangepaste gedrags klasse toe te voegen aan een bepaalde WCF-extensie.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""MyBehavior"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```
Het gebruik van volledig gekwalificeerde (sterke) namen is een unieke aanduiding van een type en verbetert de beveiliging van uw systeem. Gebruik volledig gekwalificeerde assembly namen bij het registreren van typen in de machine. config-en app. config-bestanden.

### <a name="example"></a>Voorbeeld
Het `<behaviorExtensions/>` element van het WCF-configuratie bestand hieronder geeft WCF de aangepaste gedrags klasse met een sterke verwijzing aan een bepaalde WCF-extensie toe te voegen.
```
<system.serviceModel>
    <extensions>
        <behaviorExtensions>
            <add name=""myBehavior"" type=""Microsoft.ServiceModel.Samples.MyBehaviorSection, MyBehavior,
            Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"" />
        </behaviorExtensions>
    </extensions>
</system.serviceModel>
```

## <a id="wcf-authz"></a>WCF-autorisatie beheer implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Algemeen, NET Framework 3 |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Konink rijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference) |
| **Stappen** | <p>Deze service maakt geen gebruik van een autorisatie besturings element. Wanneer een client een bepaalde WCF-service aanroept, biedt WCF verschillende autorisatie schema's waarmee wordt gecontroleerd of de aanroeper gemachtigd is om de service methode op de server uit te voeren. Als autorisatie besturings elementen niet zijn ingeschakeld voor WCF-services, kan een geverifieerde gebruiker de escalatie van bevoegdheden behaalt.</p>|

### <a name="example"></a>Voorbeeld
De volgende configuratie zorgt ervoor dat WCF het autorisatie niveau van de client niet controleert bij het uitvoeren van de service:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""None"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
Gebruik een service autorisatie schema om te controleren of de aanroeper van de service methode hiervoor is gemachtigd. WCF biedt twee modi en staat de definitie van een aangepast autorisatie schema toe. De UseWindowsGroups-modus maakt gebruik van Windows-rollen en-gebruikers en de UseAspNetRoles-modus maakt gebruik van een ASP.NET-rolprovider, zoals SQL Server, om te verifiëren.

### <a name="example"></a>Voorbeeld
De volgende configuratie zorgt ervoor dat de client deel uitmaakt van de groep Administrators voordat u de service toevoegen uitvoert:
```
<behaviors>
    <serviceBehaviors>
        <behavior>
            ...
            <serviceAuthorization principalPermissionMode=""UseWindowsGroups"" />
        </behavior>
    </serviceBehaviors>
</behaviors>
```
De service wordt vervolgens als volgt gedeclareerd:
```
[PrincipalPermission(SecurityAction.Demand,
Role = ""Builtin\\Administrators"")]
public double Add(double n1, double n2)
{
double result = n1 + n2;
return result;
}
```

## <a id="authz-aspnet"></a>Het juiste autorisatie mechanisme implementeren in ASP.NET Web-API

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Algemeen, MVC5 |
| **Eigenschappen**              | N.v.t., ID-provider-ADFS, ID-provider-Azure AD |
| **Referentie**              | [Verificatie en autorisatie in ASP.NET Web-API](https://www.asp.net/web-api/overview/security/authentication-and-authorization-in-aspnet-web-api) |
| **Stappen** | <p>De functie-informatie voor de toepassings gebruikers kan worden afgeleid van Azure AD-of ADFS-claims als de toepassing is gebaseerd op de identiteits provider of de toepassing zelf. In elk van deze gevallen moet de implementatie van de aangepaste autorisatie de gegevens van de gebruikersrol valideren.</p><p>De functie-informatie voor de toepassings gebruikers kan worden afgeleid van Azure AD-of ADFS-claims als de toepassing is gebaseerd op de identiteits provider of de toepassing zelf. In elk van deze gevallen moet de implementatie van de aangepaste autorisatie de gegevens van de gebruikersrol valideren.</p>

### <a name="example"></a>Voorbeeld
```csharp
[AttributeUsage(AttributeTargets.Class | AttributeTargets.Method, Inherited = true, AllowMultiple = true)]
public class ApiAuthorizeAttribute : System.Web.Http.AuthorizeAttribute
{
        public async override Task OnAuthorizationAsync(HttpActionContext actionContext, CancellationToken cancellationToken)
        {
            if (actionContext == null)
            {
                throw new Exception();
            }

            if (!string.IsNullOrEmpty(base.Roles))
            {
                bool isAuthorized = ValidateRoles(actionContext);
                if (!isAuthorized)
                {
                    HandleUnauthorizedRequest(actionContext);
                }
            }

            base.OnAuthorization(actionContext);
        }

public bool ValidateRoles(actionContext)
{
   //Authorization logic here; returns true or false
}

}
```
Alle controllers en actie methoden die moeten worden beveiligd, moeten worden voorzien van het bovenstaande kenmerk.
```csharp
[ApiAuthorize]
public class CustomController : ApiController
{
     //Application code goes here
}
```

## <a id="device-permission"></a>Verificatie controles uitvoeren op het apparaat als dit verschillende acties ondersteunt waarvoor verschillende machtigings niveaus zijn vereist

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-apparaat | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | <p>Het apparaat moet de oproepende functie autoriseren om te controleren of de aanroeper over de vereiste machtigingen beschikt om de gevraagde actie uit te voeren. Voor beeld: Hiermee is het apparaat een slimme deur vergrendeling die kan worden bewaakt vanuit de Cloud, plus de functionaliteit, zoals het op afstand vergren delen van de deur.</p><p>De slimme deur vergrendeling biedt de mogelijkheid om de vergrendelings functionaliteit alleen te ontgrendelen wanneer iemand fysiek in de buurt van een kaart komt. In dit geval moet de implementatie van de externe opdracht en het beheer op een zodanige manier worden uitgevoerd dat deze geen functionaliteit biedt om de deur te ontgrendelen omdat de Cloud gateway niet is gemachtigd om een opdracht te verzenden om de deur te ontgrendelen.</p>|

## <a id="field-permission"></a>Verificatie controles uitvoeren in de veld Gateway als deze verschillende acties ondersteunt waarvoor verschillende machtigings niveaus zijn vereist

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | IoT-veld Gateway | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | De veld Gateway moet de aanroeper toestemming geven om te controleren of de aanroeper over de vereiste machtigingen beschikt om de gevraagde actie uit te voeren. Bijvoorbeeld, er moeten andere machtigingen zijn voor een gebruikers interface/-API van de beheerder die wordt gebruikt om een veld Gateway v/s-apparaten te configureren waarmee er verbinding mee wordt gemaakt.|
