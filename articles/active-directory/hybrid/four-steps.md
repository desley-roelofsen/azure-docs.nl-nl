---
title: Vier stappen voor een sterke identiteits-Foundation-Azure AD
description: In dit onderwerp worden vier stappen voor Hybrid Identity-klanten beschreven die kunnen worden uitgevoerd om een sterke identiteits basis te bouwen.
services: active-directory
author: martincoetzer
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 06/20/2019
ms.subservice: hybrid
ms.author: martinco
ms.collection: M365-identity-device-management
ms.openlocfilehash: d3eb98f543e17981be0d5b9ab08fa4e146659b47
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74206782"
---
# <a name="four-steps-to-a-strong-identity-foundation-with-azure-active-directory"></a>Vier stappen voor een sterke identiteits basis met Azure Active Directory

Het beheren van de toegang tot apps en gegevens is niet langer afhankelijk van de traditionele strategieën voor netwerk beveiligings grenzen zoals perimeter netwerken en firewalls vanwege de snelle verplaatsing van apps naar de Cloud. Organisaties moeten nu hun identiteits oplossing vertrouwen om te bepalen wie en wat toegang heeft tot de apps en gegevens van de organisatie. Met meer organisaties kunnen werk nemers hun eigen apparaten gebruiken om hun apparaten te laten werken en vanaf elke locatie verbinding te maken met internet. Het is belang rijk om ervoor te zorgen dat deze apparaten voldoen aan het beleid en dat ze veilig zijn geworden in de identiteits oplossing die een organisatie kiest om te implementeren. In de huidige digitale werk plek [is de identiteit het primaire besturings vlak](https://www.microsoft.com/security/technology/identity-access-management?rtc=1) van een organisatie die naar de Cloud gaat.

Bij het aannemen van een hybride identiteits oplossing voor Azure Active Directory (Azure AD) krijgen organisaties toegang tot Premium-functies die de productiviteit vergren delen via Automation, delegering, self-service en mogelijkheden voor eenmalige aanmelding. Zo hebben uw werk nemers toegang tot bedrijfs bronnen vanaf elke locatie die ze nodig hebben om hun werk te kunnen doen, terwijl uw IT-team deze toegang kan regelen door ervoor te zorgen dat de juiste mensen toegang hebben tot de juiste bronnen om een veilige productiviteit tot stand te brengen.

Op basis van onze informatie kunt u met deze controle lijst met aanbevolen procedures snel aanbevolen acties implementeren voor het bouwen van een *sterke* identiteits basis in uw organisatie:

* Eenvoudig verbinding maken met apps
* Eén identiteit voor elke gebruiker automatisch instellen
* Geef uw gebruikers veilig
* Operationeel maken uw inzichten

## <a name="step-1---connect-to-apps-easily"></a>Stap 1: Maak eenvoudig verbinding met apps

Als u uw apps verbindt met Azure AD, kunt u de productiviteit en beveiliging van eind gebruikers verbeteren door eenmalige aanmelding (SSO) in te scha kelen en gebruikers in te richten. Door uw apps op één locatie te beheren, kunt u met Azure AD de administratieve overhead minimaliseren en één controle punt voor uw beveiligings-en nalevings beleid.

In deze sectie worden de opties beschreven voor het beheren van gebruikers toegang tot apps, het inschakelen van veilige externe toegang tot interne apps en de voor delen van het migreren van uw apps naar Azure AD.

### <a name="make-apps-available-to-your-users-seamlessly"></a>Apps naadloos beschikbaar maken voor uw gebruikers

Met Azure AD kunnen beheerders [toepassingen toevoegen](https://docs.microsoft.com/azure/active-directory/manage-apps/add-application-portal) aan de galerie met bedrijfs toepassingen in de [Azure Portal](https://portal.azure.com/). Door toepassingen toe te voegen aan de Enter prise-toepassings galerie, is het eenvoudiger voor u om toepassingen te configureren voor het gebruik van Azure AD als uw ID-provider. U kunt hiermee ook gebruikers toegang tot de toepassing beheren met beleid voor voorwaardelijke toegang en eenmalige aanmelding (SSO) configureren voor toepassingen, zodat gebruikers hun wacht woorden niet herhaaldelijk hoeven in te voeren en automatisch worden aangemeld bij on-premises en Cloud toepassingen.

Zodra toepassingen zijn toegevoegd aan de Azure AD-galerie, kunnen gebruikers apps zien die aan hen zijn toegewezen en vervolgens naar behoefte andere apps aanvragen. Azure AD biedt [verschillende methoden](https://docs.microsoft.com/azure/active-directory/manage-apps/end-user-experiences) voor gebruikers om toegang te krijgen tot hun apps:

* Toegangs venster/mijn apps
* Office 365-start programma voor apps
* Directe aanmelding bij federatieve apps
* Directe aanmeldings koppelingen

Zie voor meer informatie over gebruikers toegang tot apps, **stap 3--uw gebruikers** in dit artikel stimuleren.

### <a name="migrate-apps-from-active-directory-federation-services-to-azure-ad"></a>Apps migreren van Active Directory Federation Services naar Azure AD

Als u de configuratie van eenmalige aanmelding migreert van Active Directory Federation Services (ADFS) naar Azure AD, worden extra mogelijkheden voor beveiliging, een consistente beheer baarheid en samen werking mogelijk. Voor optimale resultaten raden wij u aan uw apps te migreren van AD FS naar Azure AD. Door de verificatie en autorisatie van uw toepassing naar Azure AD te brengen, beschikt u over de volgende voor delen:

* Kosten beheren
* Risico beheren
* Productiviteit verhogen
* Naleving en beheer van adres sering

Zie voor meer informatie het onderwerp [uw toepassingen migreren naar Azure Active Directorye](https://aka.ms/migrateapps/whitepaper) White Paper.

### <a name="enable-secure-remote-access-to-apps"></a>Veilige externe toegang tot apps inschakelen

[Azure AD-toepassingsproxy](https://docs.microsoft.com/azure/active-directory/manage-apps/what-is-application-proxy) biedt organisaties een eenvoudige oplossing voor het publiceren van on-premises apps naar de Cloud voor externe gebruikers die op een veilige manier toegang moeten hebben tot interne apps. Na een eenmalige aanmelding bij Azure AD, hebben gebruikers toegang tot zowel Cloud-als on-premises toepassingen via externe Url's of een interne toepassings Portal.

Azure AD-toepassingsproxy biedt de volgende voor delen:

* Azure AD uitbreiden naar on-premises resources
  * Beveiliging en beveiliging in de Cloud schalen
  * Functies zoals voorwaardelijke toegang en Multi-Factor Authentication die eenvoudig zijn in te scha kelen
* Geen onderdelen in het perimeter netwerk, zoals VPN-en traditionele reverse proxy-oplossingen
* Geen binnenkomende verbindingen vereist
* Eenmalige aanmelding (SSO) op apparaten, resources en apps in de Cloud en on-premises
* Biedt eind gebruikers de mogelijkheid om overal en altijd productief te zijn

### <a name="discover-shadow-it-with-microsoft-cloud-app-security"></a>Schaduw IT met Microsoft Cloud App Security ontdekken

In moderne ondernemingen zijn IT-afdelingen vaak niet op de hoogte van alle Cloud toepassingen die door de gebruikers worden gebruikt om hun werk uit te voeren. Wanneer IT-beheerders worden gevraagd hoeveel Cloud-apps ze hun werk nemers gebruiken, hebben ze gemiddeld 30 of 40. In werkelijkheid is het gemiddelde groter dan 1.000 afzonderlijke apps die worden gebruikt door werk nemers in uw organisatie. 80% van de werk nemers gebruiken niet-goedgekeurde apps die niet zijn gecontroleerd en die mogelijk niet compatibel zijn met uw beveiligings-en nalevings beleid.

Met [Microsoft Cloud app Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) (MCAS) kunt u nuttige apps identificeren die populair zijn bij gebruikers die ze kunnen erkennen en toevoegen aan de galerie met bedrijfs toepassingen, zodat gebruikers profiteren van mogelijkheden als SSO en voorwaardelijke toegang.

<em>"**Cloud app Security** helpt ons ervoor te zorgen dat onze mensen onze Cloud-en SaaS-toepassingen op de juiste wijze gebruiken, op manieren die ondersteuning bieden voor het basis beleid voor de beveiliging van Accenture."</em> --- [John blasi, Director beheren, Information Security, Accenture](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

Naast het detecteren van schaduw, kan MCAS ook het risico niveau van apps bepalen, voor komen dat onbevoegde toegang tot Bedrijfs gegevens, mogelijke gegevens lekken en andere beveiligings Risico's die inherent zijn aan de toepassingen.

## <a name="step-2---establish-one-identity-for-every-user-automatically"></a>Stap 2: een identiteit voor elke gebruiker automatisch instellen

Door on-premises en Cloud directory's samen te brengen in een Azure AD hybride identiteits oplossing, kunt u uw bestaande on-premises Active Directory investering opnieuw gebruiken door uw bestaande identiteiten in de cloud in te richten. De oplossing synchroniseert on-premises identiteiten met Azure AD, terwijl de on-premises Active Directory worden uitgevoerd met een bestaande governance-oplossing als de primaire bron van waarheid voor identiteiten. De Azure AD hybride identiteits oplossing van micro soft bevat on-premises en Cloud mogelijkheden, waarmee een algemene gebruikers-id voor verificatie en autorisatie wordt gemaakt voor alle bronnen, ongeacht hun locatie.

Door uw on-premises directory's met Azure AD te integreren, kunnen uw gebruikers productiever worden en voor komt u dat gebruikers meerdere accounts gebruiken in apps en services door een algemene identiteit op te geven voor toegang tot zowel Cloud-als on-premises resources. Het gebruik van meerdere accounts is een knel punt voor eind gebruikers en het eerlijk. In het perspectief van eind gebruikers moeten meerdere accounts meerdere wacht woorden onthouden. Om dit te voor komen, hebben veel gebruikers hetzelfde wacht woord voor elk account opnieuw gebruiken, wat niet het gevolg is van een beveiligings perspectief. Het hergebruik van een IT-perspectief leidt vaak tot het opnieuw instellen van wacht woorden en de helpdesk kosten samen met de klachten van de eind gebruiker.

Azure AD Connect is het hulp programma dat wordt gebruikt voor om uw on-premises identiteiten te synchroniseren met Azure AD, dat vervolgens kan worden gebruikt voor toegang tot Cloud toepassingen. Zodra de identiteiten zich in azure AD bevinden, kunnen ze worden ingericht voor SaaS-toepassingen zoals Sales Force of concur.

In deze sectie worden aanbevelingen weer geven voor hoge Beschik baarheid, moderne authenticatie voor de Cloud en het verminderen van uw on-premises footprint.

> [!NOTE]
> Als u meer wilt weten over Azure AD Connect, raadpleegt u [Wat is Azure AD Connect Sync?](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-whatis)

### <a name="set-up-a-staging-server-for-azure-ad-connect-and-keep-it-up-to-date"></a>Een staging-server instellen voor Azure AD Connect en deze up-to-date houden

Azure AD Connect speelt een belang rijke rol in het inrichtings proces. Als de synchronisatie server om een of andere reden offline gaat, worden wijzigingen in on-premises niet in de Cloud bijgewerkt en veroorzaken er toegangs problemen voor gebruikers. Het is belang rijk dat u een failover-strategie definieert waarmee beheerders snel de synchronisatie kunnen hervatten nadat de synchronisatie server offline gaat.

Om hoge Beschik baarheid te bieden in het geval uw primaire Azure AD Connect server offline gaat, is het raadzaam om een afzonderlijke [staging-server](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sync-staging-server) voor Azure AD Connect te implementeren. Als u een server implementeert, kan de beheerder de staging-server promo veren tot productie door een eenvoudige configuratie-switch. Als er een stand-by-server in de faserings modus is geconfigureerd, kunt u ook nieuwe configuratie wijzigingen testen en implementeren en een nieuwe server introduceren als u de oude wilt uit bedrijf nemen.

> [!TIP]
> Azure AD Connect wordt regel matig bijgewerkt. Daarom wordt het ten zeerste aanbevolen dat u de staging-server up-to-date blijft om te kunnen profiteren van de prestatie verbeteringen, oplossingen voor fouten en nieuwe mogelijkheden die elke nieuwe versie biedt.

### <a name="enable-cloud-authentication"></a>Cloud verificatie inschakelen

Organisaties met een on-premises Active Directory moeten hun Directory uitbreiden naar Azure AD met behulp van Azure AD Connect en de juiste verificatie methode configureren. Het [kiezen van de juiste verificatie methode](https://docs.microsoft.com/azure/security/fundamentals/choose-ad-authn) voor uw organisatie is de eerste stap bij het verplaatsen van apps naar de Cloud. Het is een essentieel onderdeel omdat hiermee de toegang tot alle Cloud gegevens en resources wordt beheerd.

De eenvoudigste en aanbevolen methode voor het inschakelen van Cloud verificatie voor on-premises Directory-objecten in azure AD is het inschakelen van de [synchronisatie van wacht woord-hash](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-password-hash-synchronization) (PHS). Sommige organisaties kunnen overwegen om [Pass-Through-verificatie](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-pta-quick-start) (PTA) in te scha kelen.

Of u kiest voor PHS of PTA, vergeet niet om [naadloze eenmalige aanmelding](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-sso) in te scha kelen zodat gebruikers toegang krijgen tot Cloud-apps zonder dat ze hun gebruikers naam en wacht woord in de app blijven gebruiken wanneer ze Windows 7-en 8-apparaten in uw bedrijfs netwerk gebruikt. Zonder eenmalige aanmelding moeten gebruikers toepassingsspecifieke wacht woorden onthouden en zich aanmelden bij elke toepassing. De IT-afdeling moet ook gebruikers accounts maken en bijwerken voor elke toepassing, zoals Office 365, box en Sales Force. Gebruikers moeten hun wacht woord onthouden, plus de tijd om zich aan te melden bij elke toepassing. Het bieden van een gestandaardiseerd mechanisme voor eenmalige aanmelding bij de hele onderneming is essentieel voor de beste gebruikers ervaring, vermindering van het risico, de mogelijkheid om te rapporteren en te voor komen.

Voor organisaties die al AD FS of een andere on-premises verificatie provider gebruiken, gaat u naar Azure AD, omdat uw ID-provider de complexiteit kan verminderen en de beschik baarheid kan verbeteren. Tenzij u specifieke use cases voor het gebruik van Federatie hebt, raden wij u aan de migratie uit te voeren van Federated Authentication naar PHS, naadloze SSO of PTA en naadloze SSO om te profiteren van de voor delen van een gereduceerde on-premises ruimte en de flexibiliteit die de Cloud biedt Verbeterde gebruikers ervaring. Zie voor meer informatie [migreren van Federatie naar wacht woord hash synchronisatie voor Azure Active Directory](https://docs.microsoft.com/azure/active-directory/hybrid/plan-migrate-adfs-password-hash-sync).

### <a name="enable-automatic-deprovisioning-of-accounts"></a>Automatische onttoewijzing van accounts inschakelen

Het inschakelen van automatische inrichting en ongedaan maken van de inrichting van uw toepassingen is de beste strategie voor het beheren van de levens cyclus van identiteiten op meerdere systemen. Azure AD biedt ondersteuning voor [geautomatiseerde, op beleid gebaseerde inrichting en](https://docs.microsoft.com/azure/active-directory/manage-apps/configure-automatic-user-provisioning-portal) het ongedaan maken van de inrichting van gebruikers accounts voor diverse populaire SaaS-toepassingen, zoals ServiceNow en Sales Force, en andere die het [scim 2,0-protocol](https://docs.microsoft.com/azure/active-directory/manage-apps/use-scim-to-provision-users-and-groups)implementeren. In tegens telling tot traditionele inrichtings oplossingen waarvoor aangepaste code of hand matig uploaden van CSV-bestanden vereist is, wordt de inrichtings service gehost in de Cloud en worden de vooraf geïntegreerde connectors geleverd die kunnen worden ingesteld en beheerd met behulp van de Azure Portal. Een belang rijk voor deel van het automatisch ongedaan maken van de inrichting is dat het uw organisatie helpt beveiligen door de identiteit van gebruikers direct te verwijderen uit Key SaaS-apps wanneer ze de organisatie verlaten.

Zie [Gebruikers inrichten en de inrichting ongedaan maken voor SaaS-toepassingen met Azure Active Directory](https://docs.microsoft.com/azure/active-directory/manage-apps/user-provisioning)voor meer informatie over automatische toewijzing van gebruikers accounts en hoe deze werkt.

## <a name="step-3---empower-your-users-securely"></a>Stap 3: Geef uw gebruikers veilig

In de huidige digitale werk plek is het belang rijk om de beveiliging met productiviteit te verbalanceren. Eind gebruikers pushen echter vaak een back-up op beveiligings maatregelen die hun productiviteit en toegang tot Cloud-apps vertragen. Om dit te helpen aanpakken, biedt Azure AD selfservice mogelijkheden waarmee gebruikers productief kunnen blijven terwijl de administratieve overhead wordt geminimaliseerd.

In deze sectie vindt u de aanbevelingen voor het verwijderen van wrijving van uw organisatie door uw gebruikers te voorzien van de resterende Vigilant.

### <a name="enable-self-service-password-reset-for-all-users"></a>Selfservice voor wacht woord opnieuw instellen inschakelen voor alle gebruikers

De [self-service voor wachtwoord herstel](https://docs.microsoft.com/azure/active-directory/authentication/quickstart-sspr) (SSPR) van Azure biedt een eenvoudige manier om gebruikers toe te staan hun wacht woorden of accounts te herstellen en te ontgrendelen zonder tussen komst van de beheerder. Het systeem biedt gedetailleerde rapporten zodat u kunt volgen wanneer gebruikers het systeem openen. U ontvangt ook meldingen om u te waarschuwen over misbruik.

Standaard worden accounts door Azure AD ontgrendeld wanneer het wacht woord opnieuw wordt ingesteld. Wanneer u echter Azure AD Connect [integratie on-premises](https://docs.microsoft.com/azure/active-directory/authentication/concept-sspr-howitworks#on-premises-integration)inschakelt, hebt u ook de mogelijkheid om deze twee bewerkingen te scheiden, zodat gebruikers hun account kunnen ontgrendelen zonder het wacht woord opnieuw in te stellen.

### <a name="ensure-all-users-are-registered-for-mfa-and-sspr"></a>Zorg ervoor dat alle gebruikers zijn geregistreerd voor MFA en SSPR

Azure biedt rapporten die door u en uw organisatie kunnen worden gebruikt om ervoor te zorgen dat gebruikers worden geregistreerd voor MFA en SSPR. Gebruikers die zich niet hebben geregistreerd, moeten mogelijk worden getraind voor het proces.

Het rapport voor MFA [-aanmeldingen](https://docs.microsoft.com/azure/active-directory/authentication/howto-mfa-reporting) bevat informatie over het gebruik van MFA en geeft inzicht in hoe MFA werkt in uw organisatie. U hebt toegang tot activiteiten voor aanmelden (en controles en risico detecties) voor Azure AD is essentieel voor het oplossen van problemen, gebruiks analyses en forensische onderzoek.

Op dezelfde manier kan het [rapport voor Self-Service wachtwoord beheer](https://docs.microsoft.com/azure/active-directory/authentication/howto-sspr-reporting) worden gebruikt om te bepalen wie (of niet) is geregistreerd voor SSPR.

### <a name="self-service-app-management"></a>Self-service app-beheer

Voordat uw gebruikers toepassingen zelf kunnen detecteren vanuit hun toegangs venster, moet u de toegang van [selfservice toepassingen](https://docs.microsoft.com/azure/active-directory/manage-apps/access-panel-manage-self-service-access) inschakelen voor alle toepassingen waarvoor u gebruikers de mogelijkheid wilt bieden om zichzelf te detecteren en toegang tot te vragen. Toegang voor selfservice toepassingen is een uitstekende manier om gebruikers de mogelijkheid te bieden om zelf toepassingen te detecteren en de bedrijfs groep toe te staan om de toegang tot deze toepassingen goed te keuren. U kunt de bedrijfs groep toestaan om de referenties te beheren die aan deze gebruikers zijn toegewezen voor [wacht woord voor eenmalige aanmelding voor toepassingen](https://docs.microsoft.com/azure/active-directory/manage-apps/application-sign-in-problem-password-sso-gallery#configure-password-sso-for-an-azure-ad-gallery-app) , direct vanaf hun toegangs Vensters.

### <a name="self-service-group-management"></a>Groepsbeheer via selfservice

Het toewijzen van gebruikers aan toepassingen is het meest geschikt voor het gebruik van groepen, omdat ze grote flexibiliteit en mogelijkheid bieden om op schaal te beheren:

* Kenmerk-gebaseerd op het gebruik van een dynamisch groepslid maatschap
* Delegeren naar app-eigen aren

Azure AD biedt de mogelijkheid om toegang tot resources te beheren met behulp van beveiligings groepen en Office 365-groepen. Deze groepen kunnen worden beheerd door een groeps eigenaar die lidmaatschaps aanvragen kan goed keuren of weigeren en het beheer van groepslid maatschap kan overdragen. Met deze functie wordt het [beheer van self-service groep](https://docs.microsoft.com/azure/active-directory/users-groups-roles/groups-self-service-management)genoemd, zodat groeps eigenaren die geen beheerdersrol hebben toegewezen, groepen kunnen maken en beheren zonder dat ze moeten vertrouwen op beheerders om hun aanvragen te kunnen verwerken.

## <a name="step-4---operationalize-your-insights"></a>Stap 4-operationeel maken uw inzichten

Controle en logboek registratie van gebeurtenissen met betrekking tot beveiliging en gerelateerde waarschuwingen zijn essentiële onderdelen van een efficiënte strategie om ervoor te zorgen dat gebruikers productief blijven en uw organisatie veilig is. Beveiligings logboeken en rapporten kunnen u helpen bij het beantwoorden van vragen zoals:

* Gebruikt u waarvoor u betaalt?
* Is er iets verdacht of schadelijk in mijn Tenant?
* Wie is van invloed op een beveiligings incident?

Beveiligings logboeken en rapporten bieden een elektronische record van verdachte activiteiten en helpen u bij het detecteren van patronen die kunnen wijzen op geslaagde, externe indringing van het netwerk en interne aanvallen. U kunt controle gebruiken om gebruikers activiteiten te bewaken, naleving van regelgeving te documenteren, forensische analyse uit te voeren en meer. Waarschuwingen bieden meldingen van beveiligings gebeurtenissen.

### <a name="assign-least-privileged-admin-roles-for-operations"></a>Ten minste bevoegde beheerders rollen toewijzen voor bewerkingen

Net zoals u op de hoogte bent van uw aanpak van bewerkingen, zijn er een aantal beheer niveaus die u kunt overwegen. Het eerste niveau plaatst de belasting van het beheer van uw globale beheerder (s). Het gebruik van de rol globale beheerder kan altijd geschikt zijn voor kleinere bedrijven. Maar voor grotere organisaties met helpdesk medewerkers en beheerders die verantwoordelijk zijn voor specifieke taken, kan het toewijzen van de rol van globale beheerder een beveiligings risico vormen, omdat deze personen de mogelijkheid biedt om taken te beheren die boven en buiten vallen wat ze moeten kunnen doen.

In dit geval moet u rekening houden met het volgende beheer niveau. Met Azure AD kunt u eind gebruikers aanwijzen als ' beperkte beheerders ' die taken in functies met minder bevoegdheden kunnen beheren. U kunt bijvoorbeeld uw helpdesk medewerker de rol van [beveiligings lezer](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#security-reader) geven om hen de mogelijkheid te bieden om beveiligings functies met alleen-lezen toegang te beheren. Het is ook handig om de rol [authenticatie beheerder](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles#authentication-administrator) toe te wijzen aan personen om hen de mogelijkheid te geven niet-wachtwoord referenties opnieuw in te stellen of Azure service Health te lezen en te configureren.

Zie [Administrator role permissions in azure Active Directory](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles)voor meer informatie.

### <a name="monitor-hybrid-components-azure-ad-connect-sync-ad-fs-using-azure-ad-connect-health"></a>Hybride onderdelen (Azure AD Connect Sync, AD FS) bewaken met behulp van Azure AD Connect Health

Azure AD Connect en AD FS zijn essentiële onderdelen die het beheer en de verificatie van de levens cyclus kunnen verstoren en uiteindelijk leiden tot storingen. Daarom moet u Azure AD Connect Health implementeren voor het bewaken en rapporteren van deze onderdelen.

Ga voor meer informatie naar [Monitor AD FS met behulp van Azure AD Connect Health](https://docs.microsoft.com/azure/active-directory/hybrid/how-to-connect-health-adfs).

### <a name="use-azure-monitor-to-collect-data-logs-for-analytics"></a>Azure Monitor gebruiken om gegevens logboeken te verzamelen voor analyse

[Azure monitor](https://docs.microsoft.com/azure/azure-monitor/overview) is een uniforme bewakings portal voor alle Azure AD-logboeken, waarmee uitgebreide inzichten, geavanceerde analyses en slimme machine learning worden geboden. Met Azure Monitor kunt u metrische gegevens en Logboeken in de portal en via Api's gebruiken om meer inzicht te krijgen in de status en prestaties van uw resources. Er wordt één venster glas ervaring in de portal ingeschakeld, terwijl een breed scala aan product integraties via Api's en opties voor gegevens export wordt ingeschakeld die traditionele SIEM-systemen van derden ondersteunen. Azure Monitor biedt u ook de mogelijkheid om waarschuwings regels te configureren om u op de hoogte te stellen of om geautomatiseerde acties uit te voeren op problemen die van invloed zijn op uw resources.

![Azure Monitor](./media/four-steps/image1.png)

### <a name="create-custom-dashboards-for-your-leadership-and-your-day-to-day"></a>Aangepaste Dash boards maken voor uw leiderschap en dag tot dag

Organisaties die geen SIEM-oplossing hebben, kunnen het [Power bi-inhouds pakket](https://docs.microsoft.com/azure/active-directory/reports-monitoring/howto-power-bi-content-pack) voor Azure ad downloaden. Het Power BI inhouds pakket bevat vooraf ontwikkelde rapporten waarmee u kunt begrijpen hoe uw gebruikers Azure AD-functies aannemen en gebruiken, waarmee u inzicht kunt krijgen in alle activiteiten in uw Directory. U kunt ook uw eigen [aangepaste dash board](https://docs.microsoft.com/power-bi/service-dashboards) maken en delen met uw leiderschaps team om te rapporteren over dagelijkse activiteiten. Dash boards zijn een uitstekende manier om uw bedrijf te bewaken en al uw belangrijkste metrische gegevens in één oogopslag te bekijken. De visualisaties op een dash board kunnen afkomstig zijn uit één onderliggende gegevensset of veel, en uit één onderliggend rapport of veel. Een dash board is een combi natie van on-premises en Cloud gegevens, waarbij een geconsolideerde weer gave wordt geboden, ongeacht waar de gegevens zich bevinden.

![Aangepast dash board Power BI](./media/four-steps/image2.png)

### <a name="understand-your-support-call-drivers"></a>Meer informatie over uw ondersteunings oproep Stuur Programma's

Wanneer u een hybride identiteits oplossing implementeert, zoals beschreven in dit artikel, moet u uiteindelijk een verlaging in uw ondersteunings oproepen opmerken. Veelvoorkomende problemen, zoals verg eten wacht woorden en account vergrendelingen, worden verholpen door de selfservice voor wachtwoord herstel van Azure te implementeren, terwijl het inschakelen van selfservice toepassings toegang gebruikers in staat stelt om toegang te krijgen tot toepassingen zonder te hoeven vertrouwen van uw IT-mede werkers.

Als u geen verlaging ondervindt in ondersteunings oproepen, raden we u aan om uw ondersteunings gespreks Stuur Programma's te analyseren in een poging om te bevestigen of SSPR of selfservice voor toegang tot de toepassing correct is geconfigureerd, of als er andere nieuwe problemen zijn die systematisch kunnen worden uitgebreide.

*"In onze digitale trans formatie-reis hebben we een betrouw bare provider voor identiteits-en toegangs beheer nodig om een krachtige, maar veilige integratie tussen VS, partners en Cloud serviceproviders te vergemakkelijken voor een effectief ecosysteem. Azure AD is de beste optie om de benodigde mogelijkheden en zicht baarheid aan te bieden die ons heeft ingeschakeld om Risico's te detecteren en erop te reageren. "* --- [Yazan Almasri, Global Information Security Director, Aramex](https://customers.microsoft.com/story/aramex-azure-active-directory-travel-transportation-united-arab-emirates-en)

### <a name="monitor-your-usage-of-apps-to-drive-insights"></a>Uw gebruik van apps controleren om inzichten te verkrijgen

Naast het detecteren van schaduw, kunt u het gebruik van apps in uw organisatie bewaken met behulp van [Microsoft Cloud app Security](https://docs.microsoft.com/cloud-app-security/what-is-cloud-app-security) uw organisatie helpen om optimaal te profiteren van de belofte van Cloud toepassingen. Het biedt u de controle over uw assets door betere zicht baarheid in de activiteiten en de beveiliging van essentiële gegevens in Cloud toepassingen te verbeteren. Als u het gebruik van apps in uw organisatie bewaken met behulp van MCAS, kunt u de volgende vragen beantwoorden:

* In welke niet-goedgekeurde apps worden werk nemers gebruikt om gegevens op te slaan?
* Waar en wanneer worden gevoelige gegevens opgeslagen in de Cloud?
* Wie heeft toegang tot gevoelige gegevens in de Cloud?

*"Met Cloud App Security kunnen we snel afwijkingen herkennen en actie ondernemen."* --- [Eric LePenske, Senior Manager, Information Security, Accenture](https://customers.microsoft.com/story/accenture-professional-services-cloud-app-security)

## <a name="summary"></a>Samenvatting

Er zijn veel aspecten voor het implementeren van een hybride identiteits oplossing, maar deze controle lijst met vier stappen helpt u snel een infra structuur voor identiteiten te maken waarmee gebruikers productiever en veiliger kunnen zijn.

* Eenvoudig verbinding maken met apps
* Eén identiteit voor elke gebruiker automatisch instellen
* Geef uw gebruikers veilig
* Operationeel maken uw inzichten

We hopen dat dit document een handig schema is voor het opzetten van een sterke identiteits basis voor uw organisatie.

## <a name="identity-checklist"></a>Controle lijst voor identiteit

U wordt aangeraden de volgende controle lijst af te drukken voor referentie wanneer u begint met uw reis naar een meer solide identiteits basis in uw organisatie.

### <a name="today"></a>Huidige

|Klaar?|Item|
|:-|:-|
||Self-service voor wachtwoord herstel (SSPR) testen voor een groep|
||Hybride onderdelen bewaken met behulp van Azure AD Connect Health|
||Ten minste bevoegde beheerders rollen toewijzen voor bewerking|
||Schaduw IT met Microsoft Cloud App Security ontdekken|
||Azure Monitor gebruiken om gegevens logboeken te verzamelen voor analyse|

### <a name="next-two-weeks"></a>Volgende twee weken

|Klaar?|Item|
|:-|:-|
||Een app beschikbaar maken voor uw gebruikers|
||Prototype van Azure AD inrichten voor een SaaS-app van Choice|
||Een staging-server instellen voor Azure AD Connect en deze up-to-date houden|
||Beginnen met het migreren van apps van ADFS naar Azure AD|
||Aangepaste Dash boards maken voor uw leiderschap en dag tot dag|

### <a name="next-month"></a>Volgende maand

|Klaar?|Item|
|:-|:-|
||Uw gebruik van apps controleren om inzichten te verkrijgen|
||Pilot beveiligde externe toegang tot apps|
||Zorg ervoor dat alle gebruikers zijn geregistreerd voor MFA en SSPR|
||Cloud verificatie inschakelen|

### <a name="next-three-months"></a>Volgende drie maanden

|Klaar?|Item|
|:-|:-|
||Self-service app-beheer inschakelen|
||Self-service groeps beheer inschakelen|
||Uw gebruik van apps controleren om inzichten te verkrijgen|
||Meer informatie over uw ondersteunings oproep Stuur Programma's|

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe u uw beveiligde postuur kunt verhogen met behulp van de mogelijkheden van Azure Active Directory en deze controle lijst [voor vijf stappen-vijf stap om uw identiteits infrastructuur te beveiligen](https://aka.ms/securitysteps).

Lees hoe u met de identiteits functies in azure AD uw overgang naar het beheer van de cloud kunt versnellen door de oplossingen en mogelijkheden te bieden waarmee organisaties snel meer hun identiteits beheer van traditionele on-premises systemen kunnen overnemen en verplaatsen naar Azure AD: [hoe Azure ad het beheer van de Cloud biedt voor on-premises workloads](https://aka.ms/cloudgoverned).
