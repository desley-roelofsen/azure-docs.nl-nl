---
title: Gevoelige gegevens-Microsoft Threat Modeling Tool-Azure | Microsoft Docs
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
ms.openlocfilehash: d482ae375fca3824213b54c2045d114fa2f0bfbe
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68727870"
---
# <a name="security-frame-sensitive-data--mitigations"></a>Beveiligings frame: Gevoelige gegevens | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **Grens van computer vertrouwen** | <ul><li>[Zorg ervoor dat binaire bestanden worden verborgen als deze gevoelige informatie bevatten](#binaries-info)</li><li>[Het gebruik van EFS (Encrypted File System) wordt gebruikt voor het beveiligen van vertrouwelijke gebruikersspecifieke gegevens](#efs-user)</li><li>[Zorg ervoor dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestands systeem, zijn versleuteld](#filesystem)</li></ul> | 
| **Webtoepassing** | <ul><li>[Zorg ervoor dat gevoelige inhoud niet in de cache wordt opgeslagen in de browser](#cache-browser)</li><li>[Secties van configuratie bestanden van de web-app versleutelen die gevoelige gegevens bevatten](#encrypt-data)</li><li>[Het kenmerk voor automatisch aanvullen van HTML expliciet uitschakelen in gevoelige formulieren en invoer](#autocomplete-input)</li><li>[Zorg ervoor dat gevoelige gegevens die worden weer gegeven op het scherm van de gebruiker, worden gemaskeerd](#data-mask)</li></ul> | 
| **Database** | <ul><li>[Dynamische gegevens maskering implementeren om gebruikers niet-gemachtigde gevoelige gegevens bloot te leggen](#dynamic-users)</li><li>[Zorg ervoor dat wacht woorden worden opgeslagen in de gezouten hash-indeling](#salted-hash)</li><li>[Zorg ervoor dat gevoelige gegevens in database kolommen zijn versleuteld](#db-encrypted)</li><li>[Zorg ervoor dat versleuteling op database niveau (TDE) is ingeschakeld](#tde-enabled)</li><li>[Zorg ervoor dat database back-ups zijn versleuteld](#backup)</li></ul> | 
| **Web-API** | <ul><li>[Zorg ervoor dat gevoelige gegevens die relevant zijn voor web API niet worden opgeslagen in de opslag van de browser](#api-browser)</li></ul> | 
| Azure Document DB | <ul><li>[Gevoelige gegevens versleutelen die zijn opgeslagen in Azure Cosmos DB](#encrypt-docdb)</li></ul> | 
| **Grens van Azure IaaS VM-vertrouwens relatie** | <ul><li>[Azure Disk Encryption gebruiken voor het versleutelen van schijven die worden gebruikt door Virtual Machines](#disk-vm)</li></ul> | 
| **Grens van Service Fabric vertrouwen** | <ul><li>[Geheimen in Service Fabric toepassingen versleutelen](#fabric-apps)</li></ul> | 
| **Dynamics CRM** | <ul><li>[Beveiligings modellen uitvoeren en waar nodig bedrijfs eenheden/teams gebruiken](#modeling-teams)</li><li>[Beperk de toegang tot de share functie op essentiële entiteiten](#entities)</li><li>[Train gebruikers over de Risico's die zijn gekoppeld aan de Dynamics CRM-share functie en goede beveiligings procedures](#good-practices)</li><li>[Een regel voor het ontwikkelen van normen bevatten met betrekking tot configuratie details in uitzonderings beheer](#exception-mgmt)</li></ul> | 
| **Azure Storage** | <ul><li>[Azure Storage-service versleuteling (SSE) gebruiken voor Data-at-rest (preview-versie)](#sse-preview)</li><li>[Versleuteling aan de client zijde gebruiken om gevoelige gegevens op te slaan in Azure Storage](#client-storage)</li></ul> | 
| **Mobiele client** | <ul><li>[Gevoelige of PII-gegevens versleutelen die zijn geschreven naar lokale opslag voor telefoons](#pii-phones)</li><li>[Gegenereerde binaire bestanden voor het distribueren naar eind gebruikers](#binaries-end)</li></ul> | 
| **WCF** | <ul><li>[ClientCredentialType instellen op certificaat of Windows](#cert)</li><li>[WCF-beveiligings modus is niet ingeschakeld](#security)</li></ul> | 

## <a id="binaries-info"></a>Zorg ervoor dat binaire bestanden worden verborgen als deze gevoelige informatie bevatten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van computer vertrouwen | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Zorg ervoor dat binaire bestanden worden verborgen als deze gevoelige informatie bevatten, zoals handels geheimen, gevoelige bedrijfs logica die niet moet worden omgekeerd. Dit is om reverse engineering van assembly's te stoppen. Voor dit `CryptoObfuscator` doel kunt u hulpprogram ma's als gebruiken. |

## <a id="efs-user"></a>Het gebruik van EFS (Encrypted File System) wordt gebruikt voor het beveiligen van vertrouwelijke gebruikersspecifieke gegevens

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van computer vertrouwen | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Het gebruik van EFS (Encrypted File System) wordt gebruikt voor het beveiligen van vertrouwelijke gebruikersspecifieke gegevens van aanvallers met fysieke toegang tot de computer. |

## <a id="filesystem"></a>Zorg ervoor dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestands systeem, zijn versleuteld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van computer vertrouwen | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Zorg ervoor dat gevoelige gegevens die zijn opgeslagen door de toepassing op het bestands systeem, zijn versleuteld (bijvoorbeeld via DPAPI), als EFS niet kan worden afgedwongen |

## <a id="cache-browser"></a>Zorg ervoor dat gevoelige inhoud niet in de cache wordt opgeslagen in de browser

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Generic, Web Forms, MVC5, MVC6 |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Browsers kunnen gegevens opslaan voor caching en geschiedenis. Deze bestanden in de cache worden opgeslagen in een map, zoals de map Tijdelijke Internet bestanden in het geval van Internet Explorer. Wanneer deze pagina's opnieuw worden verwezen, worden ze uit de cache van de browser weer gegeven. Als gevoelige informatie wordt weer gegeven aan de gebruiker (zoals hun adres, creditcard gegevens, sofi-nummer of gebruikers naam), kan deze informatie worden opgeslagen in de cache van de browser en kan daarom worden opgehaald via de cache van de browser of door Druk op de knop terug in de browser. Stel de waarde Cache-Control response header in op ' No-Store ' voor alle pagina's. |

### <a name="example"></a>Voorbeeld
```XML
<configuration>
  <system.webServer>
   <httpProtocol>
    <customHeaders>
        <add name="Cache-Control" value="no-cache" />
        <add name="Pragma" value="no-cache" />
        <add name="Expires" value="-1" />
    </customHeaders>
  </httpProtocol>
 </system.webServer>
</configuration>
```

### <a name="example"></a>Voorbeeld
Dit kan worden geïmplementeerd via een filter. Het volgende voor beeld kan worden gebruikt: 
```csharp
public override void OnActionExecuting(ActionExecutingContext filterContext)
        {
            if (filterContext == null || (filterContext.HttpContext != null && filterContext.HttpContext.Response != null && filterContext.HttpContext.Response.IsRequestBeingRedirected))
            {
                //// Since this is MVC pipeline, this should never be null.
                return;
            }

            var attributes = filterContext.ActionDescriptor.GetCustomAttributes(typeof(System.Web.Mvc.OutputCacheAttribute), false);
            if (attributes == null || **Attributes**.Count() == 0)
            {
                filterContext.HttpContext.Response.Cache.SetNoStore();
                filterContext.HttpContext.Response.Cache.SetCacheability(HttpCacheability.NoCache);
                filterContext.HttpContext.Response.Cache.SetExpires(DateTime.UtcNow.AddHours(-1));
                if (!filterContext.IsChildAction)
                {
                    filterContext.HttpContext.Response.AppendHeader("Pragma", "no-cache");
                }
            }

            base.OnActionExecuting(filterContext);
        }
``` 

## <a id="encrypt-data"></a>Secties van configuratie bestanden van de web-app versleutelen die gevoelige gegevens bevatten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Procedure: Configuratie secties versleutelen in ASP.net 2,0](https://msdn.microsoft.com/library/ff647398.aspx)met behulp van DPAPI, [een beveiligde configuratie provider opgeven](https://msdn.microsoft.com/library/68ze1hb2.aspx) [met behulp van Azure Key Vault om toepassings geheimen te beveiligen](https://azure.microsoft.com/documentation/articles/guidance-multitenant-identity-keyvault/) |
| **Stappen** | Configuratie bestanden zoals web. config, appSettings. json worden vaak gebruikt om gevoelige informatie te bewaren, zoals gebruikers namen, wacht woorden, database verbindings reeksen en versleutelings sleutels. Als u deze informatie niet beveiligt, is uw toepassing kwetsbaar voor aanvallen of kwaadwillende gebruikers die gevoelige informatie verkrijgen, zoals gebruikers namen en wacht woorden voor accounts, database namen en server namen. Op basis van het implementatie type (Azure/on-premises) versleutelt u de gevoelige secties van de configuratie bestanden met behulp van DPAPI of services als Azure Key Vault. |

## <a id="autocomplete-input"></a>Het kenmerk voor automatisch aanvullen van HTML expliciet uitschakelen in gevoelige formulieren en invoer

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [MSDN: kenmerk automatisch aanvullen](https://msdn.microsoft.com/library/ms533486(VS.85).aspx), [met behulp van automatisch aanvullen in HTML](https://msdn.microsoft.com/library/ms533032.aspx), [probleem met HTML-](https://technet.microsoft.com/security/bulletin/MS10-071)opschoning, [automatisch aanvullen.](https://blog.mindedsecurity.com/2011/10/autocompleteagain.html) |
| **Stappen** | Het kenmerk automatisch aanvullen geeft aan of automatisch aanvullen voor een formulier moet worden in-of uitgeschakeld. Als automatisch aanvullen is ingeschakeld, worden de waarden in de browser op basis van de waarden die de gebruiker eerder heeft ingevoerd, in het getypt. Wanneer bijvoorbeeld een nieuwe naam en een nieuw wacht woord wordt ingevoerd in een formulier en het formulier wordt verzonden, wordt u in de browser gevraagd of het wacht woord moet worden opgeslagen. Wanneer het formulier vervolgens wordt weer gegeven, worden de naam en het wacht woord automatisch ingevuld of ingevuld als de naam wordt ingevoerd. Een aanvaller met lokale toegang kan het wacht woord voor lees bare tekst verkrijgen uit de cache van de browser. Automatisch aanvullen is standaard ingeschakeld en moet expliciet worden uitgeschakeld. |

### <a name="example"></a>Voorbeeld
```csharp
<form action="Login.aspx" method="post " autocomplete="off" >
      Social Security Number: <input type="text" name="ssn" />
      <input type="submit" value="Submit" />    
</form>
```

## <a id="data-mask"></a>Zorg ervoor dat gevoelige gegevens die worden weer gegeven op het scherm van de gebruiker, worden gemaskeerd

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Gevoelige gegevens zoals wacht woorden, creditcard nummers, SSN enz. moeten worden gemaskeerd wanneer ze op het scherm worden weer gegeven. Dit is om te voor komen dat onbevoegden toegang krijgen tot de gegevens (zoals een schouder-Surf wachtwoord, ondersteunings personeel dat de SSN-nummers van gebruikers bekijkt). Zorg ervoor dat deze gegevens elementen niet zichtbaar zijn in tekst zonder opmaak en op de juiste wijze worden gemaskeerd. Dit moet zorgvuldig worden uitgevoerd en wordt geaccepteerd als invoer (bijvoorbeeld. invoer type = "wacht woord") en weer gegeven op het scherm (bijvoorbeeld alleen de laatste 4 cijfers van het creditcard nummer weer geven). |

## <a id="dynamic-users"></a>Dynamische gegevens maskering implementeren om gebruikers niet-gemachtigde gevoelige gegevens bloot te leggen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Sql Azure, OnPrem |
| **Eigenschappen**              | SQL-versie-V12, SQL-versie-MsSQL2016 |
| **Referentie**              | [Dynamische gegevens maskering](https://msdn.microsoft.com/library/mt130841) |
| **Stappen** | Het doel van dynamische gegevens maskering is om de bloot stelling van gevoelige gegevens te beperken, zodat gebruikers die geen toegang tot de gegevens mogen hebben, deze kunnen bekijken. Dynamische gegevens maskering is niet bedoeld om te voor komen dat database gebruikers rechtstreeks verbinding kunnen maken met de data base en uitvoeren van uitgebreide query's waarmee delen van de gevoelige gegevens worden blootgesteld. Dynamische gegevens maskering is complementair met andere SQL Server beveiligings functies (controle, versleuteling, beveiliging op rijniveau...) en het wordt ten zeerste aanbevolen om deze functie te gebruiken in combi natie met ze, naast elkaar om de gevoelige gegevens beter te beveiligen in de enddatabase. Houd er rekening mee dat deze functie alleen wordt ondersteund door SQL Server vanaf 2016 en Azure SQL Database. |

## <a id="salted-hash"></a>Zorg ervoor dat wacht woorden worden opgeslagen in de gezouten hash-indeling

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Wacht woord-hashing met behulp van .NET crypto-Api's](https://docs.asp.net/en/latest/security/data-protection/consumer-apis/password-hashing.html) |
| **Stappen** | Wacht woorden mogen niet worden opgeslagen in data bases voor aangepaste gebruikers archieven. Wacht woord-hashes moeten in plaats daarvan worden opgeslagen met zout waarden. Zorg ervoor dat het Salt-netwerk voor de gebruiker altijd uniek is en dat u b-Cryptografie, s-crypt of PBKDF2 toepast voordat u het wacht woord opslaat, met een minimum aantal werk factoren van 150.000 lussen om te voor komen dat de kans op brute kracht wordt afgedwongen.| 

## <a id="db-encrypted"></a>Zorg ervoor dat gevoelige gegevens in database kolommen zijn versleuteld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | SQL-versie-alle |
| **Referentie**              | Het versleutelen [van gevoelige gegevens in SQL Server](https://technet.microsoft.com/library/ff848751(v=sql.105).aspx), [How to: Een kolom met gegevens in SQL Server](https://msdn.microsoft.com/library/ms179331)versleutelen, versleutelen [per certificaat](https://msdn.microsoft.com/library/ms188061) |
| **Stappen** | Gevoelige gegevens zoals creditcard nummers moeten worden versleuteld in de-data base. Gegevens kunnen worden versleuteld met versleuteling op kolom niveau of door een toepassings functie met behulp van de versleutelings functies. |

## <a id="tde-enabled"></a>Zorg ervoor dat versleuteling op database niveau (TDE) is ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Informatie over SQL Server Transparent Data Encryption (TDE)](https://technet.microsoft.com/library/bb934049(v=sql.105).aspx) |
| **Stappen** | De functie Transparent Data Encryption (TDE) in SQL Server helpt bij het versleutelen van gevoelige gegevens in een Data Base en het beveiligen van de sleutels die worden gebruikt om de gegevens te versleutelen met een certificaat. Zo voor komt u dat iedereen zonder de sleutels de gegevens gebruikt. TDE beveiligt gegevens "in rust", wat de gegevens en logboek bestanden zijn. Het biedt de mogelijkheid om te voldoen aan de vele wetten, voor schriften en richt lijnen die in verschillende branches zijn vastgelegd. |

## <a id="backup"></a>Zorg ervoor dat database back-ups zijn versleuteld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Database | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | SQL Azure, OnPrem |
| **Eigenschappen**              | SQL-versie-V12, SQL-versie-MsSQL2014 |
| **Referentie**              | [Back-upversleuteling SQL database](https://msdn.microsoft.com/library/dn449489) |
| **Stappen** | SQL Server beschikt over de mogelijkheid om de gegevens te versleutelen tijdens het maken van een back-up. Als u de versleutelings algoritme en de versleutelings-en coderings provider (een certificaat of asymmetrische sleutel) opgeeft bij het maken van een back-up, kan een versleuteld back- |

## <a id="api-browser"></a>Zorg ervoor dat gevoelige gegevens die relevant zijn voor web API niet worden opgeslagen in de opslag van de browser

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | MVC 5, MVC 6 |
| **Eigenschappen**              | ID-provider-ADFS, ID-provider-Azure AD |
| **Referentie**              | N/A  |
| **Stappen** | <p>In bepaalde implementaties worden gevoelige artefacten die relevant zijn voor de verificatie van web-API'S opgeslagen in de lokale opslag van de browser. Bijvoorbeeld Azure AD-verificatie artefacten, zoals adal. idtoken, adal. nonce. idtoken, adal. Access. token. key, adal. token. Keys, adal. status. login, adal. sessie. State, adal. expire. key, enzovoort.</p><p>Al deze artefacten zijn beschikbaar nadat u zich hebt afgemeld of de browser is gesloten. Als een kwaadwillende persoon toegang krijgt tot deze artefacten, kan hij/zij ze opnieuw gebruiken om toegang te krijgen tot de beveiligde bronnen (Api's). Zorg ervoor dat alle gevoelige artefacten die betrekking hebben op Web API, niet zijn opgeslagen in de opslag ruimte van de browser. In gevallen waarin opslag aan de client zijde niet kan worden voor komen (bijvoorbeeld: single page Applications (SPA) die gebruikmaken van impliciete OpenIdConnect/OAuth-stromen, moeten de toegangs tokens lokaal worden opgeslagen). gebruik opslag keuzes zonder persistentie. bijvoorbeeld liever SessionStorage naar LocalStorage.</p>| 

### <a name="example"></a>Voorbeeld
Het onderstaande java script-fragment is afkomstig uit een aangepaste verificatie bibliotheek die verificatie artefacten in de lokale opslag opslaat. Dergelijke implementaties moeten worden vermeden. 
```javascript
ns.AuthHelper.Authenticate = function () {
window.config = {
instance: 'https://login.microsoftonline.com/',
tenant: ns.Configurations.Tenant,
clientId: ns.Configurations.AADApplicationClientID,
postLogoutRedirectUri: window.location.origin,
cacheLocation: 'localStorage', // enable this for IE, as sessionStorage does not work for localhost.
};
```

## <a id="encrypt-docdb"></a>Gevoelige gegevens versleutelen die zijn opgeslagen in Cosmos DB

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Document DB | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Versleutel gevoelige gegevens op toepassings niveau voordat u deze opslaat in document DB of gevoelige gegevens opslaat in andere opslag oplossingen, zoals Azure Storage of Azure SQL| 

## <a id="disk-vm"></a>Azure Disk Encryption gebruiken voor het versleutelen van schijven die worden gebruikt door Virtual Machines

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van Azure IaaS VM-vertrouwens relatie | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Schijven die worden gebruikt door uw virtuele machines versleutelen met behulp van Azure Disk Encryption](https://azure.microsoft.com/documentation/articles/storage-security-guide/#_using-azure-disk-encryption-to-encrypt-disks-used-by-your-virtual-machines) |
| **Stappen** | <p>Azure Disk Encryption is een nieuwe functie die momenteel als preview-versie beschikbaar is. Met deze functie kunt u de besturingssysteem schijven en gegevens schijven versleutelen die worden gebruikt door een virtuele machine van IaaS. Voor Windows worden de stations versleuteld met de standaard BitLocker-versleutelings technologie. Voor Linux worden de schijven versleuteld met de DM-cryptografie technologie. Dit is geïntegreerd met Azure Key Vault zodat u de schijf versleutelings sleutels kunt beheren en beheren. De oplossing Azure Disk Encryption ondersteunt de volgende drie scenario's voor het versleutelen van de klant:</p><ul><li>Schakel versleuteling in voor nieuwe IaaS-Vm's die zijn gemaakt op basis van door de klant versleutelde VHD-bestanden en door de klant verschafte versleutelings sleutels, die zijn opgeslagen in Azure Key Vault.</li><li>Schakel versleuteling in voor nieuwe IaaS-Vm's die zijn gemaakt op basis van de Azure Marketplace.</li><li>Schakel versleuteling in op bestaande virtuele IaaS-machines die al worden uitgevoerd in Azure.</li></ul>| 

## <a id="fabric-apps"></a>Geheimen in Service Fabric toepassingen versleutelen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Grens van Service Fabric vertrouwen | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | Omgeving-Azure |
| **Referentie**              | [Geheimen beheren in Service Fabric toepassingen](https://azure.microsoft.com/documentation/articles/service-fabric-application-secret-management/) |
| **Stappen** | Geheimen kunnen gevoelige informatie zijn, zoals verbindings reeksen voor opslag, wacht woorden of andere waarden die niet in tekst zonder opmaak moeten worden verwerkt. Gebruik Azure Key Vault om sleutels en geheimen in service Fabric-toepassingen te beheren. |

## <a id="modeling-teams"></a>Beveiligings modellen uitvoeren en waar nodig bedrijfs eenheden/teams gebruiken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Beveiligings modellen uitvoeren en waar nodig bedrijfs eenheden/teams gebruiken |

## <a id="entities"></a>Beperk de toegang tot de share functie op essentiële entiteiten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Beperk de toegang tot de share functie op essentiële entiteiten |

## <a id="good-practices"></a>Train gebruikers over de Risico's die zijn gekoppeld aan de Dynamics CRM-share functie en goede beveiligings procedures

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Train gebruikers over de Risico's die zijn gekoppeld aan de Dynamics CRM-share functie en goede beveiligings procedures |

## <a id="exception-mgmt"></a>Een regel voor het ontwikkelen van normen bevatten met betrekking tot configuratie details in uitzonderings beheer

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Dynamics CRM | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | Een regel voor de ontwikkelings normen op te geven met betrekking tot configuratie details in uitzonderings beheer buiten de ontwikkeling. Test dit als onderdeel van de code beoordelingen of periodieke inspectie.|

## <a id="sse-preview"></a>Azure Storage-service versleuteling (SSE) gebruiken voor Data-at-rest (preview-versie)

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | Para-BLOB |
| **Referentie**              | [Azure Storage-service versleuteling voor Data-at-rest (preview-versie)](https://azure.microsoft.com/documentation/articles/storage-service-encryption/) |
| **Stappen** | <p>Azure Storage service Encryption (SSE) voor Data-at-rest helpt u bij het beveiligen en beschermen van uw gegevens om te voldoen aan de beveiligings-en nalevings verplichtingen van uw organisatie. Met deze functie worden uw gegevens automatisch versleuteld door Azure Storage voordat deze worden opgeslagen en ontsleutelt voordat ze weer worden opgehaald. De versleuteling, ontsleuteling en sleutel beheer zijn volledig transparant voor gebruikers. SSE is alleen van toepassing op blok-blobs, pagina-blobs en toevoeg-blobs. De andere typen gegevens, met inbegrip van tabellen, wacht rijen en bestanden, worden niet versleuteld.</p><p>Werk stroom voor versleuteling en ontsleuteling:</p><ul><li>De klant schakelt versleuteling in voor het opslag account</li><li>Wanneer de klant nieuwe gegevens schrijft (PUT-blob, PUT-blok, PUT-pagina enz.) naar de Blob-opslag; elke schrijf bewerking wordt versleuteld met 256-bits AES-versleuteling, een van de krach tigste blok cijfers die beschikbaar zijn</li><li>Wanneer de klant toegang moet krijgen tot gegevens (GET blob, enz.), worden de gegevens automatisch ontsleuteld voordat deze naar de gebruiker worden geretourneerd.</li><li>Als versleuteling is uitgeschakeld, worden nieuwe schrijf bewerkingen niet langer versleuteld en blijven bestaande versleutelde gegevens versleuteld totdat ze door de gebruiker worden herschreven. Wanneer versleuteling is ingeschakeld, worden schrijf bewerkingen naar de Blob-opslag versleuteld. De status van de gegevens verandert niet wanneer de gebruiker in-of uitschakelt tussen het in-en uitschakelen van versleuteling voor het opslag account</li><li>Alle versleutelings sleutels worden opgeslagen, versleuteld en beheerd door micro soft</li></ul><p>Op dit moment worden de sleutels die worden gebruikt voor de versleuteling, beheerd door micro soft. Micro soft genereert de sleutels oorspronkelijk en beheert de veilige opslag van de sleutels, evenals de normale draaiing zoals gedefinieerd door intern micro soft-beleid. In de toekomst krijgen klanten de mogelijkheid om hun eigen > versleutelings sleutels te beheren en een migratie traject te bieden van door micro soft beheerde sleutels naar door de klant beheerde sleutels.</p>| 

## <a id="client-storage"></a>Versleuteling aan de client zijde gebruiken om gevoelige gegevens op te slaan in Azure Storage

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Azure Storage | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Versleuteling aan client zijde en Azure Key Vault voor Microsoft Azure Storage](https://azure.microsoft.com/documentation/articles/storage-client-side-encryption/), [zelf studie: Blobs versleutelen en ontsleutelen](https://azure.microsoft.com/documentation/articles/storage-encrypt-decrypt-blobs-key-vault/)in Microsoft Azure Storage met behulp van Azure Key Vault, [gegevens veilig opslaan in Azure Blob Storage met Azure](https://blogs.msdn.microsoft.com/partnercatalystteam/2015/06/17/storing-data-securely-in-azure-blob-storage-with-azure-encryption-extensions/) -versleutelings uitbreidingen |
| **Stappen** | <p>Het Azure Storage-client bibliotheek voor .NET Nuget-pakket biedt ondersteuning voor het versleutelen van gegevens in client toepassingen voordat u ze uploadt naar Azure Storage en gegevens ontsleutelt tijdens het downloaden van de client. De bibliotheek ondersteunt ook integratie met Azure Key Vault voor het beheer van sleutels voor opslagaccounts. Hier volgt een korte beschrijving van de werking van versleuteling aan client zijde:</p><ul><li>De Azure Storage client-SDK genereert een coderings sleutel voor inhoud (CEK), een eenmalige symmetrische sleutel gebruiken</li><li>Klant gegevens worden versleuteld met behulp van deze CEK</li><li>De CEK wordt vervolgens gewrappt (versleuteld) met behulp van de sleutel versleutelings sleutel (KEK). De KEK wordt geïdentificeerd aan de hand van een sleutel-id en kan een asymmetrisch sleutel paar of een symmetrische sleutel zijn en kan lokaal worden beheerd of opgeslagen in Azure Key Vault. De Storage-client zelf heeft nooit toegang tot de KEK. Hiermee wordt alleen het sleutel terugloop algoritme aangeroepen dat wordt verschaft door Key Vault. Klanten kunnen ervoor kiezen om aangepaste providers te gebruiken voor sleutel terugloop/uitpakken als ze dat willen</li><li>De versleutelde gegevens worden vervolgens geüpload naar de Azure Storage-service. Controleer de koppelingen in het gedeelte met verwijzingen naar details over implementatie op laag niveau.</li></ul>|

## <a id="pii-phones"></a>Gevoelige of PII-gegevens versleutelen die zijn geschreven naar lokale opslag voor telefoons

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele client | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Algemeen, Xamarin  |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Instellingen en functies op uw apparaten beheren met Microsoft intune beleid](https://docs.microsoft.com/intune/deploy-use/manage-settings-and-features-on-your-devices-with-microsoft-intune-policies), [sleutel hanger valetsleutel](https://components.xamarin.com/view/square.valet) |
| **Stappen** | <p>Als de toepassing gevoelige informatie schrijft, zoals de PII van de gebruiker (e-mail adres, telefoon nummer, voor naam, achternaam, voor keuren, enzovoort) -het bestands systeem van het mobiele apparaat moet worden versleuteld voordat naar het lokale bestands systeem wordt geschreven. Als de toepassing een bedrijfs toepassing is, bekijkt u de mogelijkheid om de toepassing te publiceren met behulp van Windows intune.</p>|

### <a name="example"></a>Voorbeeld
InTune kan worden geconfigureerd met de volgende beveiligings beleidsregels om gevoelige gegevens te beschermen: 
```csharp
Require encryption on mobile device    
Require encryption on storage cards
Allow screen capture
```

### <a name="example"></a>Voorbeeld
Als de toepassing geen bedrijfs toepassing is, gebruikt u de door het platform verschafte sleutel opslag, de basis ketens voor het opslaan van coderings sleutels, waarbij de cryptografische bewerking kan worden uitgevoerd op het bestands systeem. Het volgende code fragment laat zien hoe u met xamarin toegang krijgt tot de sleutel van sleutel keten: 
```csharp
        protected static string EncryptionKey
        {
            get
            {
                if (String.IsNullOrEmpty(_Key))
                {
                    var query = new SecRecord(SecKind.GenericPassword);
                    query.Service = NSBundle.MainBundle.BundleIdentifier;
                    query.Account = "UniqueID";

                    NSData uniqueId = SecKeyChain.QueryAsData(query);
                    if (uniqueId == null)
                    {
                        query.ValueData = NSData.FromString(System.Guid.NewGuid().ToString());
                        var err = SecKeyChain.Add(query);
                        _Key = query.ValueData.ToString();
                    }
                    else
                    {
                        _Key = uniqueId.ToString();
                    }
                }

                return _Key;
            }
        }
```

## <a id="binaries-end"></a>Gegenereerde binaire bestanden voor het distribueren naar eind gebruikers

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Mobiele client | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Versleuteling van crypto grafie voor .net](https://www.ssware.com/cryptoobfuscator/obfuscator-net.htm) |
| **Stappen** | Gegenereerde binaire bestanden (assembly's in APK) moeten worden verborgen om reverse engineering van assembly's te stoppen. Voor dit `CryptoObfuscator` doel kunt u hulpprogram ma's als gebruiken. |

## <a id="cert"></a>ClientCredentialType instellen op certificaat of Windows

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | .NET Framework 3 |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Fortify](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_token) |
| **Stappen** | Het gebruik van een UsernameToken met een lees bare wacht woord via een niet-versleuteld kanaal stelt het wacht woord in voor aanvallers die de SOAP-berichten kunnen sniffen. Service providers die gebruikmaken van de UsernameToken kunnen wacht woorden accepteren die als Lees bare tekst worden verzonden. Het verzenden van Lees bare wacht woorden via een niet-versleuteld kanaal kan de referentie bloot stellen aan aanvallers die het SOAP-bericht kunnen sniffen. | 

### <a name="example"></a>Voorbeeld
De volgende configuratie van de WCF-service provider maakt gebruik van de UsernameToken: 
```
<security mode="Message"> 
<message clientCredentialType="UserName" />
``` 
Stel clientCredentialType in op Certificate of Windows. 

## <a id="security"></a>WCF-beveiligings modus is niet ingeschakeld

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Algemeen, .NET Framework 3 |
| **Eigenschappen**              | Beveiligings modus-Trans Port, beveiligings modus-bericht |
| **Referentie**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Konink rijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_weak_class_reference), [basis principes van het magazine voor de WCF-beveiligings CoDe](https://www.codemag.com/article/0611051) |
| **Stappen** | Er is geen Trans Port-of bericht beveiliging gedefinieerd. Toepassingen die berichten verzenden zonder Trans Port-of bericht beveiliging, kunnen de integriteit of vertrouwelijkheid van de berichten niet garanderen. Wanneer een WCF-beveiligings binding op geen is ingesteld, worden Trans Port-en bericht beveiliging uitgeschakeld. |

### <a name="example"></a>Voorbeeld
De volgende configuratie stelt de beveiligings modus in op geen. 
```
<system.serviceModel> 
  <bindings> 
    <wsHttpBinding> 
      <binding name=""MyBinding""> 
        <security mode=""None""/> 
      </binding> 
  </bindings> 
</system.serviceModel> 
```

### <a name="example"></a>Voorbeeld
De beveiligings modus voor alle service bindingen bestaat uit vijf mogelijke beveiligings modi: 
* Geen. Hiermee schakelt u de beveiliging uit. 
* Portmap. Maakt gebruik van transport beveiliging voor wederzijdse verificatie en bericht beveiliging. 
* Bericht. Gebruikt bericht beveiliging voor wederzijdse verificatie en bericht beveiliging. 
* Zowel. Hiermee kunt u instellingen opgeven voor het Trans Port en beveiliging op bericht niveau (alleen MSMQ ondersteunt dit). 
* TransportWithMessageCredential. Referenties worden door gegeven met het bericht en de beveiliging van de server worden geleverd door de transportlaag. 
* TransportCredentialOnly. Client referenties worden door gegeven aan de transportlaag en er wordt geen bericht beveiliging toegepast. Gebruik Trans Port-en bericht beveiliging om de integriteit en vertrouwelijkheid van berichten te beveiligen. De volgende configuratie geeft aan dat de service transport beveiliging gebruikt met bericht referenties.
  ```
  <system.serviceModel>
  <bindings>
    <wsHttpBinding>
    <binding name=""MyBinding""> 
    <security mode=""TransportWithMessageCredential""/> 
    <message clientCredentialType=""Windows""/> 
    </binding> 
  </bindings> 
  </system.serviceModel> 
  ```
