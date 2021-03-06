---
title: Uitzonderings beheer-Microsoft Threat Modeling Tool-Azure | Microsoft Docs
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
ms.openlocfilehash: b8fad566b54ab645660011ad3188394b6f8190b0
ms.sourcegitcommit: 85b3973b104111f536dc5eccf8026749084d8789
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/01/2019
ms.locfileid: "68728073"
---
# <a name="security-frame-exception-management--mitigations"></a>Beveiligings frame: Uitzonderings beheer | Oplossingen 
| Product/Service | Artikel |
| --------------- | ------- |
| **WCF** | <ul><li>[WCF: serviceDebug-knoop punt niet in configuratie bestand toevoegen](#servicedebug)</li><li>[WCF: serviceMetadata-knoop punt niet in configuratie bestand toevoegen](#servicemetadata)</li></ul> |
| **Web-API** | <ul><li>[Zorg ervoor dat de juiste uitzonde ring wordt verwerkt in de ASP.NET-Web-API](#exception)</li></ul> |
| **Webtoepassing** | <ul><li>[Geen beveiligings Details beschikbaar maken in fout berichten](#messages)</li><li>[Standaard pagina voor fout afhandeling implementeren](#default)</li><li>[Implementatie methode instellen op Retail in IIS](#deployment)</li><li>[Uitzonde ringen moeten veilig mislukken](#fail)</li></ul> |

## <a id="servicedebug"></a>WCF: serviceDebug-knoop punt niet in configuratie bestand toevoegen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Algemeen, NET Framework 3 |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Konink rijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_debug_information) |
| **Stappen** | WCF-services (Windows Communication Framework) kunnen worden geconfigureerd om fout opsporingsgegevens weer te geven. Fout opsporingsgegevens mogen niet worden gebruikt in productie omgevingen. De `<serviceDebug>` tag definieert of de functie fout opsporingsgegevens is ingeschakeld voor een WCF-service. Als het kenmerk includeExceptionDetailInFaults is ingesteld op True, worden er uitzonderings gegevens van de toepassing geretourneerd aan clients. Aanvallers kunnen gebruikmaken van de aanvullende informatie die ze van de uitvoer van de fout opsporing gebruiken om aanvallen te koppelen die zijn gericht op het Framework, de data base of andere bronnen die door de toepassing worden gebruikt. |

### <a name="example"></a>Voorbeeld
Het volgende configuratie bestand bevat de `<serviceDebug>` tag: 
```
<configuration> 
<system.serviceModel> 
<behaviors> 
<serviceBehaviors> 
<behavior name=""MyServiceBehavior""> 
<serviceDebug includeExceptionDetailInFaults=""True"" httpHelpPageEnabled=""True""/> 
... 
```
Fout opsporingsgegevens in de service uitschakelen. Dit kan worden bereikt door de `<serviceDebug>` tag te verwijderen uit het configuratie bestand van uw toepassing. 

## <a id="servicemetadata"></a>WCF: serviceMetadata-knoop punt niet in configuratie bestand toevoegen

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | WCF | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | Algemeen, NET Framework 3 |
| **Referentie**              | [MSDN](https://msdn.microsoft.com/library/ff648500.aspx), [Fortify Konink rijk](https://vulncat.fortify.com/en/detail?id=desc.config.dotnet.wcf_misconfiguration_service_enumeration) |
| **Stappen** | Openbaar weer geven van informatie over een service kan kwaadwillende personen een waardevol inzicht bieden in de manier waarop ze de service kunnen exploiteren. Met `<serviceMetadata>` de tag wordt de functie voor het publiceren van meta gegevens ingeschakeld. Meta gegevens van de service kunnen gevoelige informatie bevatten die niet openbaar toegankelijk moeten zijn. Geef mini maal alleen vertrouwde gebruikers toegang tot de meta gegevens en zorg ervoor dat er geen onnodige informatie beschikbaar is. Nog beter, de mogelijkheid om meta gegevens te publiceren volledig uitschakelen. Een veilige WCF-configuratie bevat `<serviceMetadata>` geen tag. |

## <a id="exception"></a>Zorg ervoor dat de juiste uitzonde ring wordt verwerkt in de ASP.NET-Web-API

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Web-API | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | MVC 5, MVC 6 |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Afhandeling van uitzonde ringen in ASP.net Web-API](https://www.asp.net/web-api/overview/error-handling/exception-handling), [Model validatie in ASP.net-Web-API](https://www.asp.net/web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api) |
| **Stappen** | Standaard worden de meeste niet-onderschepte uitzonde ringen in ASP.NET Web-API omgezet in een HTTP-antwoord met de status code`500, Internal Server Error`|

### <a name="example"></a>Voorbeeld
Voor het beheren van de status code die wordt geretourneerd `HttpResponseException` door de API, kan worden gebruikt, zoals hieronder wordt weer gegeven: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        throw new HttpResponseException(HttpStatusCode.NotFound);
    }
    return item;
}
```

### <a name="example"></a>Voorbeeld
Voor meer controle over het antwoord op de uitzonde ring kan de `HttpResponseMessage` klasse worden gebruikt, zoals hieronder wordt weer gegeven: 
```csharp
public Product GetProduct(int id)
{
    Product item = repository.Get(id);
    if (item == null)
    {
        var resp = new HttpResponseMessage(HttpStatusCode.NotFound)
        {
            Content = new StringContent(string.Format("No product with ID = {0}", id)),
            ReasonPhrase = "Product ID Not Found"
        }
        throw new HttpResponseException(resp);
    }
    return item;
}
```
Om niet-verwerkte uitzonde ringen te ondervangen `HttpResponseException`die niet van het type zijn, kunnen uitzonderings filters worden gebruikt. Uitzonderings filters `System.Web.Http.Filters.IExceptionFilter` implementeren de interface. De eenvoudigste manier om een uitzonderings filter te schrijven, is om `System.Web.Http.Filters.ExceptionFilterAttribute` te worden afgeleid van de-klasse en de OnException-methode te overschrijven. 

### <a name="example"></a>Voorbeeld
Hier volgt een filter waarmee uitzonde ringen worden omgezet `NotImplementedException` in HTTP-status code: `501, Not Implemented` 
```csharp
namespace ProductStore.Filters
{
    using System;
    using System.Net;
    using System.Net.Http;
    using System.Web.Http.Filters;

    public class NotImplExceptionFilterAttribute : ExceptionFilterAttribute 
    {
        public override void OnException(HttpActionExecutedContext context)
        {
            if (context.Exception is NotImplementedException)
            {
                context.Response = new HttpResponseMessage(HttpStatusCode.NotImplemented);
            }
        }
    }
}
```

Er zijn verschillende manieren om een uitzonderings filter voor web-API'S te registreren:
- Op actie
- Per controller
- Globally

### <a name="example"></a>Voorbeeld
Als u het filter wilt Toep assen op een specifieke actie, voegt u het filter als een kenmerk toe aan de actie: 
```csharp
public class ProductsController : ApiController
{
    [NotImplExceptionFilter]
    public Contact GetContact(int id)
    {
        throw new NotImplementedException("This method is not implemented");
    }
}
```
### <a name="example"></a>Voorbeeld
Als u het filter wilt Toep assen op alle acties op `controller`een, voegt u het filter als een kenmerk `controller` toe aan de klasse: 

```csharp
[NotImplExceptionFilter]
public class ProductsController : ApiController
{
    // ...
}
```

### <a name="example"></a>Voorbeeld
Als u het filter globaal wilt Toep assen op alle web API-controllers, voegt u een exemplaar `GlobalConfiguration.Configuration.Filters` van het filter toe aan de verzameling. Uitzonderings filters in deze verzameling zijn van toepassing op elke web-API-controller actie. 
```csharp
GlobalConfiguration.Configuration.Filters.Add(
    new ProductStore.NotImplExceptionFilterAttribute());
```

### <a name="example"></a>Voorbeeld
Voor model validatie kan de status van het model worden door gegeven aan de methode CreateErrorResponse, zoals hieronder wordt weer gegeven: 
```csharp
public HttpResponseMessage PostProduct(Product item)
{
    if (!ModelState.IsValid)
    {
        return Request.CreateErrorResponse(HttpStatusCode.BadRequest, ModelState);
    }
    // Implementation not shown...
}
```

Controleer de koppelingen in het gedeelte met verwijzingen voor meer informatie over buitengewone verwerking en model validatie in ASP.NET Web-API 

## <a id="messages"></a>Geen beveiligings Details beschikbaar maken in fout berichten

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | N/A  |
| **Stappen** | <p>Algemene fout berichten worden rechtstreeks aan de gebruiker gegeven zonder dat gevoelige toepassings gegevens worden opgenomen. Voor beelden van gevoelige gegevens zijn:</p><ul><li>Server namen</li><li>Verbindingsreeksen</li><li>Gebruikers namen</li><li>Wachtwoorden</li><li>SQL-procedures</li><li>Details van dynamische SQL-fouten</li><li>Stack tracering en regels code</li><li>Variabelen die in het geheugen zijn opgeslagen</li><li>Stations en maplocaties</li><li>Toepassings installatie punten</li><li>Configuratie-instellingen van host</li><li>Overige details van interne toepassing</li></ul><p>Het overvullen van alle fouten binnen een toepassing en het leveren van algemene fout berichten, en het inschakelen van aangepaste fouten in IIS helpt te voor komen dat gegevens worden vrijgegeven. SQL Server Data Base en .NET Exception Handling, onder andere architecturen voor het afhandelen van fouten, zijn met name uitgebreid en zeer nuttig voor een kwaadwillende gebruiker die uw toepassing profileert. De inhoud van een klasse die is afgeleid van de .NET-uitzonderings klasse niet rechtstreeks weer geven en ervoor zorgen dat u de juiste uitzonderings afhandeling hebt, zodat een onverwachte uitzonde ring niet per ongeluk direct naar de gebruiker wordt verhoogd.</p><ul><li>Zorg ervoor dat algemene fout berichten rechtstreeks aan de gebruiker worden verstrekt, waardoor specifieke details direct in het uitzonde ring/fout bericht worden gevonden.</li><li>De inhoud van een .NET-uitzonderings klasse niet rechtstreeks aan de gebruiker weer geven</li><li>Alle fout berichten overlappen en, indien nodig, de gebruiker op de hoogte stellen via een algemeen fout bericht dat naar de client van de toepassing wordt verzonden</li><li>Bewaart de inhoud van de uitzonderings klasse niet rechtstreeks aan de gebruiker, met name de `.ToString()`retour waarde van of de waarden van de eigenschappen Message of StackTrace. Deze gegevens veilig vastleggen en een meer Innocuous-bericht voor de gebruiker weer geven</li></ul>|

## <a id="default"></a>Standaard pagina voor fout afhandeling implementeren

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Het dialoog venster instellingen voor ASP.NET-fout pagina's bewerken](https://technet.microsoft.com/library/dd569096(WS.10).aspx) |
| **Stappen** | <p>Wanneer een ASP.NET-toepassing mislukt en een interne server fout van HTTP/1. x 500 veroorzaakt, of een functie configuratie (zoals het filteren van aanvragen) voor komt dat een pagina wordt weer gegeven, wordt er een fout bericht gegenereerd. Beheerders kunnen kiezen of de toepassing een vriendelijk bericht moet weer geven voor de client, een gedetailleerd fout bericht naar de client of een gedetailleerd fout bericht dat alleen wordt localhost. De `<customErrors>` tag in web. config heeft drie modi:</p><ul><li>**Waarop** Hiermee geeft u op dat aangepaste fouten worden ingeschakeld. Als er geen defaultRedirect-kenmerk is opgegeven, zien gebruikers een algemene fout. De aangepaste fouten worden weer gegeven op de externe clients en op de lokale host</li><li>**Office** Hiermee geeft u op dat aangepaste fouten moeten worden uitgeschakeld. De gedetailleerde ASP.NET-fouten worden weer gegeven op de externe clients en op de lokale host</li><li>**RemoteOnly:** Hiermee geeft u op dat aangepaste fouten alleen worden weer gegeven voor de externe clients en dat er ASP.NET-fouten worden weer gegeven aan de lokale host. Dit is de standaard waarde</li></ul><p>Open het `web.config` bestand voor de toepassing/site en zorg ervoor dat het label ofwel `<customErrors mode="RemoteOnly" />` of `<customErrors mode="On" />` is gedefinieerd.</p>|

## <a id="deployment"></a>Implementatie methode instellen op Retail in IIS

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Implementatie |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [implementatie-element (ASP.NET-instellingen schema)](https://msdn.microsoft.com/library/ms228298(VS.80).aspx) |
| **Stappen** | <p>De `<deployment retail>` switch is bedoeld voor gebruik door de productie-IIS-servers. Deze schakel optie wordt gebruikt voor het uitvoeren van toepassingen met de best mogelijke prestaties en mogelijke lekkage van beveiligings gegevens door de mogelijkheid van de toepassing om tracerings uitvoer op een pagina te genereren uit te scha kelen en de mogelijkheid om gedetailleerde fout berichten weer te geven in te scha kelen naar eind gebruikers en de schakel optie voor fout opsporing uit te scha kelen.</p><p>Vaak worden de switches en opties die gericht zijn op ontwikkel aars, zoals het traceren van mislukte aanvragen en fout opsporing, ingeschakeld tijdens de actieve ontwikkeling. Het is raadzaam om de implementatie methode op een productie server in te stellen op retail. Open het bestand Machine. config en zorg ervoor `<deployment retail="true" />` dat het is ingesteld op waar.</p>|

## <a id="fail"></a>Uitzonde ringen moeten veilig mislukken

| Titel                   | Details      |
| ----------------------- | ------------ |
| **Onderdeel**               | Webtoepassing | 
| **SDL-fase**               | Ontwikkelen |  
| **Toepasselijke technologieën** | Encarta |
| **Eigenschappen**              | N/A  |
| **Referentie**              | [Veilig uitvoeren](https://www.owasp.org/index.php/Fail_securely) |
| **Stappen** | De toepassing moet veilig kunnen worden uitgevoerd. Een methode die een Booleaanse waarde retourneert, op basis van een bepaalde beslissing, moet een uitzonderings blok zorgvuldig hebben gemaakt. Er zijn veel logische fouten, omdat er sprake is van een kneep van beveiligings problemen, wanneer het uitzonderings blok onvoorzichtig wordt geschreven.|

### <a name="example"></a>Voorbeeld
```csharp
        public static bool ValidateDomain(string pathToValidate, Uri currentUrl)
        {
            try
            {
                if (!string.IsNullOrWhiteSpace(pathToValidate))
                {
                    var domain = RetrieveDomain(currentUrl);
                    var replyPath = new Uri(pathToValidate);
                    var replyDomain = RetrieveDomain(replyPath);

                    if (string.Compare(domain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                    {
                        //// Adding additional check to enable CMS urls if they are not hosted on same domain.
                        if (!string.IsNullOrWhiteSpace(Utilities.CmsBase))
                        {
                            var cmsDomain = RetrieveDomain(new Uri(Utilities.Base.Trim()));
                            if (string.Compare(cmDomain, replyDomain, StringComparison.OrdinalIgnoreCase) != 0)
                            {
                                return false;
                            }
                            else
                            {
                                return true;
                            }
                        }

                        return false;
                    }
                }

                return true;
            }
            catch (UriFormatException ex)
            {
                LogHelper.LogException("Utilities:ValidateDomain", ex);
                return true;
            }
        }
```
Met de bovenstaande methode wordt altijd waar geretourneerd als er een uitzonde ring optreedt. Als de eind gebruiker een onjuist gevormde URL biedt, de browser respecteert `Uri()` , maar de constructor niet, wordt er een uitzonde ring gegenereerd en wordt het slacht offer naar de geldige, maar onjuist gevormde URL geleid. 
