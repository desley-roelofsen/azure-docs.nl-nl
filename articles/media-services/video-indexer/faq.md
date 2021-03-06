---
title: Veelgestelde vragen over Video Indexer-Azure
titleSuffix: Azure Media Services
description: In dit artikel vindt u antwoorden op veelgestelde vragen over Azure Media Services Video Indexer.
services: media-services
author: Juliako
manager: femila
ms.service: media-services
ms.subservice: video-indexer
ms.topic: article
ms.date: 05/15/2019
ms.author: juliako
ms.openlocfilehash: fc5457fd44bd27731cad6269ee95aed838a3d755
ms.sourcegitcommit: 8bd85510aee664d40614655d0ff714f61e6cd328
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/06/2019
ms.locfileid: "74892784"
---
# <a name="frequently-asked-questions"></a>Veelgestelde vragen

In dit artikel vindt u antwoorden op veelgestelde vragen over Video Indexer.

## <a name="general-questions"></a>Algemene vragen

### <a name="what-is-video-indexer"></a>Wat is Video Indexer?

Video Indexer is een kunst matige intelligentie service die deel uitmaakt van Microsoft Azure Media Services. Video Indexer biedt een indeling van meerdere machine learning modellen waarmee u gemakkelijk diep inzicht kunt verkrijgen op basis van een video. Video Indexer maakt gebruik van meerdere kanalen van de video om geavanceerde en nauw keurige inzichten te bieden: Audio, spraak en visuele elementen. De inzichten van Video Indexer kunnen op verschillende manieren worden gebruikt, zoals het verbeteren van de detectie van inhoud en toegankelijkheid, het maken van nieuwe verdiensten maximaliseren-mogelijkheden of het bouwen van nieuwe ervaringen die gebruikmaken van de inzichten. Video Indexer biedt een webinterface voor het testen, configureren en aanpassen van modellen in uw account. Ontwikkel aars kunnen een op REST gebaseerde API gebruiken om Video Indexer te integreren in het productie systeem. 

### <a name="what-can-i-do-with-video-indexer"></a>Wat kan ik doen met Video Indexer?

Enkele van de bewerkingen die Video Indexer op media bestanden kunnen uitvoeren, zijn:

* Het identificeren en uitpakken van spraak en het identificeren van luid sprekers.
* Het identificeren en uitpakken van tekst op het scherm in een video.
* Objecten in een video bestand detecteren.
* Identificeren van merken (bijvoorbeeld micro soft) van audio tracks en tekst op het scherm in een video.
* Het detecteren en herkennen van gezichten van een Data Base met beroemdheden en een door de gebruiker gedefinieerde data base van gezichten.
* Het uitpakken van onderwerpen die worden besproken, maar niet noodzakelijkerwijs vermeld in audio-en video-inhoud.
* Ondertiteling of ondertiteling maken op basis van het audio spoor.

Zie [overzicht](video-indexer-overview.md)voor meer informatie en meer video indexer functies.

### <a name="how-do-i-get-started-with-video-indexer"></a>Hoe kan ik aan de slag met Video Indexer?

Video Indexer bevat een gratis proef versie van 600 minuten in de web-interface en 2.400 minuten via de API. U kunt u [Aanmelden bij de video indexer-webinterface](https://www.videoindexer.ai/) en deze zelf uitproberen met een webidentiteit en zonder dat u een Azure-abonnement hoeft in te stellen. 

Als u Video's en audio op schaal wilt indexeren, kunt u Video Indexer verbinden met een betaald Microsoft Azure-abonnement. Op de pagina met [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) vindt u meer informatie over de prijzen.

Meer informatie vindt u [in aan de slag.](video-indexer-get-started.md)

### <a name="do-i-need-coding-skills-to-use-video-indexer"></a>Heb ik codeer vaardigheden nodig om Video Indexer te gebruiken?

U kunt de webinterface van Video Indexer gebruiken om uw account te evalueren, te configureren en te beheren zonder dat hiervoor **code ring vereist**is.  Wanneer u klaar bent voor het ontwikkelen van complexere toepassingen, kunt u de [video indexer-API](https://api-portal.videoindexer.ai/) gebruiken om video indexer te integreren in uw eigen toepassingen, websites of [aangepaste werk stromen met behulp van serverloze technologieën zoals Azure Logic apps](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/) of Azure functions.

### <a name="do-i-need-machine-learning-skills-to-use-video-indexer"></a>Heb ik machine learning vaardig heden nodig om Video Indexer te gebruiken?

Nee, Video Indexer biedt de integratie van meerdere machine learning modellen in één pijp lijn. Als u een video-of audio bestand indexeert via Video Indexer, wordt een volledige set inzichten opgehaald die op één gedeelde tijd lijn is geëxtraheerd zonder enige machine learning vaardig heden of kennis over algoritmen die nodig zijn voor het onderdeel van de klant.

### <a name="what-media-formats-does-video-indexer-support"></a>Welke media-indelingen ondersteunt Video Indexer?

Video Indexer biedt ondersteuning voor de meeste algemene media-indelingen. Raadpleeg de lijst met [Azure Media Encoder standaard indelingen](https://docs.microsoft.com/azure/media-services/latest/media-encoder-standard-formats) voor meer informatie.

### <a name="how-to-do-i-upload-a-media-into-video-indexer"></a>Hoe kan ik een medium uploaden naar Video Indexer?

In de Video Indexer-webportal kunt u een media bestand uploaden met behulp van het dialoog venster voor het uploaden van bestanden of door een URL aan te wijzen die het bron bestand rechtstreeks host (Zie [voor beeld](https://nimbuscdn-nimbuspm.streaming.mediaservices.windows.net/2b533311-b215-4409-80af-529c3e853622/Ignite-short.mp4)). Een URL die als host fungeert voor de media-inhoud die gebruikmaakt van een iFrame of invoeg code, werkt niet (Zie [voor beeld](https://www.videoindexer.ai/accounts/7e1282e8-083c-46ab-8c20-84cae3dc289d/videos/5cfa29e152/?t=4.11)). De Video Indexer-API vereist dat u het invoer bestand via een URL of een byte matrix opgeeft. Uploads via een URL die de API gebruikt, zijn beperkt tot 10 GB, maar hebben geen tijds duur limiet. Lees voor meer informatie deze [hand leiding](https://docs.microsoft.com/azure/media-services/video-indexer/upload-index-videos).

### <a name="how-long-does-it-take-video-indexer-to-extract-insights-from-media"></a>Hoe lang duurt het Video Indexer om inzichten te extra heren van media?

De hoeveelheid tijd die nodig is om een video-of audio bestand te indexeren, zowel met de Video Indexer-API als met de Video Indexer-webinterface, is afhankelijk van meerdere para meters, zoals de bestands lengte en de kwaliteit, het aantal inzichten in het bestand, het aantal beschik bare [gereserveerde eenheden](https://docs.microsoft.com/azure/media-services/previous/media-services-scale-media-processing-overview) en of het [streaming-eind punt](https://docs.microsoft.com/azure/media-services/previous/media-services-streaming-endpoints-overview) is ingeschakeld of niet. We raden u aan een paar test bestanden met uw eigen inhoud uit te voeren en zo een beter idee te krijgen.

### <a name="can-i-create-customized-workflows-to-automate-processes-with-video-indexer"></a>Kan ik aangepaste werk stromen maken voor het automatiseren van processen met Video Indexer?

Ja, u kunt Video Indexer integreren in serverloze technologieën zoals Logic Apps, flow en [Azure functions](https://azure.microsoft.com/services/functions/). U kunt [hier](https://azure.microsoft.com/blog/logic-apps-flow-connectors-will-make-automating-video-indexer-simpler-than-ever/)meer informatie vinden over de [logische app](https://azure.microsoft.com/services/logic-apps/) en [flow](https://flow.microsoft.com/en-us/) connectors voor video indexer. 

### <a name="in-which-azure-regions-is-video-indexer-available"></a>In welke Azure-regio's is video indexer beschikbaar?

Op de pagina [regio's](https://azure.microsoft.com/global-infrastructure/services/?products=cognitive-services&regions=all) kunt u zien welke Azure-regio's video indexer beschikbaar zijn.

### <a name="what-is-the-sla-for-video-indexer"></a>Wat is de SLA voor Video Indexer?

De SLA van Azure media service behandelt Video Indexer en is te vinden op de [Sla](https://azure.microsoft.com/support/legal/sla/media-services/v1_2/) -pagina. De SLA is alleen van toepassing op Video Indexer betaalde accounts en is niet van toepassing op de gratis proef versie.

## <a name="privacy-questions"></a>Privacy-vragen

### <a name="are-video-and-audio-files-indexed-by-video-indexer-stored"></a>Worden video-en audio bestanden geïndexeerd door Video Indexer opgeslagen?

Ja, tenzij u het bestand verwijdert uit Video Indexer, met behulp van de Video Indexer website of API, worden uw video-en audio bestanden opgeslagen. Voor de gratis proef versie worden de video-en audio bestanden die u indexeert, opgeslagen in de Azure-regio VS-Oost. Anders worden uw video-en audio bestanden opgeslagen in het opslag account van uw Azure-abonnement.

### <a name="can-i-delete-my-files-that-are-stored-in-video-indexer-portal"></a>Kan ik mijn bestanden verwijderen die zijn opgeslagen in Video Indexer Portal?

Ja, u kunt altijd uw video-en audio bestanden verwijderen, evenals alle meta gegevens en inzichten die door Video Indexer worden geëxtraheerd. Wanneer u een bestand verwijdert uit Video Indexer, worden het bestand en de bijbehorende meta gegevens en inzichten permanent verwijderd uit Video Indexer. Als u echter uw eigen back-upoplossing hebt geïmplementeerd in azure Storage, blijft het bestand in uw Azure-opslag.

### <a name="can-i-control-user-access-to-my-video-indexer-account"></a>Kan ik de gebruikers toegang tot mijn Video Indexer account beheren?

Ja, alleen account beheerders kunnen personen uitnodigen en uitnodigen voor hun account, maar ook toewijzen met bewerkings rechten en met alleen-lezen toegang.

### <a name="who-has-access-to-my-video-and-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Wie heeft toegang tot mijn video-en audio bestanden die zijn geïndexeerd en/of opgeslagen door Video Indexer en de door u uitgepakte meta gegevens en inzichten?

Uw video-of audio-inhoud die openbaar is als privacy-instelling, is toegankelijk voor iedereen met de koppeling naar uw video-of audio-inhoud en inzichten. Uw video-of audio-inhoud die als privacyinstelling is ingesteld, kan alleen worden gebruikt door gebruikers die zijn uitgenodigd voor het account van de video-of audio-inhoud. De privacyinstelling van uw inhoud is ook van toepassing op de meta gegevens en inzichten die Video Indexer uittreksels. U wijst de privacyinstelling toe wanneer u uw video-of audio bestand uploadt. U kunt de privacyinstelling ook na het indexeren wijzigen.

### <a name="what-access-does-microsoft-have-to-my-video-or-audio-files-that-have-been-indexed-andor-stored-by-video-indexer-and-the-metadata-and-insights-that-were-extracted"></a>Welke toegang heeft micro soft voor mijn video-of audio bestanden die zijn geïndexeerd en/of opgeslagen door Video Indexer en de door u uitgepakte meta gegevens en inzichten?

Op basis van de [Azure Online Services-voor waarden](https://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=31) (OST) hebt u volledig eigenaar van uw inhoud. micro soft heeft alleen toegang tot uw inhoud en de meta gegevens en inzichten die video indexer worden geëxtraheerd uit uw inhoud volgens de OST en de privacyverklaring van micro soft.

### <a name="are-the-custom-models-that-i-build-in-my-video-indexer-account-available-to-other-accounts"></a>Zijn de aangepaste modellen die ik in mijn Video Indexer-account heb gemaakt, beschikbaar voor andere accounts?

 Nee, de aangepaste modellen die u in uw account maakt, zijn niet beschikbaar voor een ander account. Met Video Indexer kunt u op dit moment aangepaste [merken](customize-brands-model-overview.md), [talen](customize-language-model-overview.md)en [persoons](customize-person-model-overview.md) modellen bouwen in uw account. Deze modellen zijn alleen beschikbaar in het account waarin u de modellen hebt gemaakt.
  
### <a name="is-the-content-indexed-by-video-indexer-kept-within-the-azure-region-where-i-am-using-video-indexer"></a>Wordt de inhoud geïndexeerd door Video Indexer gehouden in de Azure-regio waar ik Video Indexer gebruik?

Ja, de inhoud en de inzichten worden bewaard in de Azure-regio, tenzij u een hand matige configuratie in uw Azure-abonnement hebt die gebruikmaakt van meerdere Azure-regio's. 

### <a name="what-is-the-privacy-policy-for-video-indexer"></a>Wat is het privacybeleid voor Video Indexer?

Video Indexer valt onder de [privacyverklaring van micro soft](https://privacy.microsoft.com/privacystatement). De privacyverklaring bevat een uitleg van de persoonlijke gegevens van micro soft-processen, de manier waarop micro soft deze verwerkt en voor welke doelen micro soft dit verwerkt. Ga naar het [vertrouwens centrum van micro soft](https://www.microsoft.com/trustcenter)voor meer informatie over privacy.

### <a name="what-certifications-does-video-indexer-have"></a>Welke certificeringen heeft Video Indexer?

Video Indexer heeft momenteel de SOC-certificering. Raadpleeg het [vertrouwens centrum van micro soft](https://www.microsoft.com/trustcenter/compliance/complianceofferings?product=Azure)om de certificering van video indexer te controleren.

## <a name="api-questions"></a>API-vragen

### <a name="what-apis-does-video-indexer-offer"></a>Welke Api's biedt Video Indexer?

Met de Api's van Video Indexer kan indexering worden uitgebreid, worden meta gegevens, het beheer van activa, de vertaling, het insluiten, het aanpassen van modellen en meer uitgepakt. Raadpleeg de [video indexer ontwikkelaars Portal](https://api-portal.videoindexer.ai/)voor meer gedetailleerde informatie over het gebruik van de video INDEXER-API.

### <a name="what-client-sdks-does-video-indexer-offer"></a>Welke client-Sdk's Video Indexer aanbieding?

Er zijn momenteel geen client-Sdk's beschikbaar. Het Video Indexer-team werkt aan de Sdk's en is van plan om ze binnenkort te leveren.

### <a name="how-do-i-get-started-with-video-indexers-api"></a>Hoe kan ik aan de slag met de Video Indexer-API?

Volg [de zelf studie: aan de slag met de video indexer-API](video-indexer-use-apis.md).

### <a name="what-is-the-difference-between-the-video-indexer-api-and-the-azure-media-service-v3-api"></a>Wat is het verschil tussen de Video Indexer API en de Azure Media Service V3 API?

Er zijn momenteel enkele overlap pingen in functies die worden aangeboden door de Video Indexer-API en de Azure Media Service V3 API. U kunt [hier](compare-video-indexer-with-media-services-presets.md)meer informatie vinden over het vergelijken van beide services.

### <a name="what-is-an-api-access-token-and-why-do-i-need-it"></a>Wat is een API-toegangs token en waarom heb ik dit nodig?

De Video Indexer-API bevat een autorisatie-API en een operations API. De autorisaties-API bevat aanroepen waarmee u een toegangs token kunt opgeven. Elke aanroep naar de Operations-API moet worden gekoppeld aan een toegangstoken dat overeenkomt met het autorisatiebereik van de aanroep.

Toegangs tokens zijn nodig voor het gebruik van de Video Indexer-Api's om veiligheids redenen. Dit zorgt ervoor dat alle aanroepen afkomstig zijn van u of gebruikers met toegangs machtigingen voor uw account. 

### <a name="what-is-the-difference-between-account-access-token-user-access-token-and-video-access-token"></a>Wat is het verschil tussen account toegangs token, toegangs token voor gebruikers en video toegangs token?

* Account niveau: toegangs tokens op account niveau bieden u de mogelijkheid om bewerkingen uit te voeren op het niveau van de account of het video niveau. U kunt bijvoorbeeld een video uploaden, alle Video's weer geven en video inzichten ophalen.
* Gebruikers niveau: toegangs tokens op gebruikers niveau bieden u de mogelijkheid om bewerkingen op het gebruikers niveau uit te voeren. Hiermee kunt u bijvoorbeeld gekoppelde accounts ophalen.
* Video niveau – toegangs tokens op video niveau bieden u de mogelijkheid om bewerkingen uit te voeren op een specifieke video. Hiermee kunt u bijvoorbeeld inzichten in een video krijgen, ondertiteling downloaden, widgets ophalen, enzovoort.

### <a name="how-often-do-i-need-to-get-a-new-access-token-when-do-access-tokens-expire"></a>Hoe vaak moet ik een nieuw toegangs token verkrijgen? Wanneer verlopen toegangs tokens?

Toegangs tokens verlopen elk uur, dus u moet elk uur een nieuw toegangs token genereren. 

## <a name="billing-questions"></a>Vragen over facturering

### <a name="how-much-does-video-indexer-cost"></a>Hoeveel kost het Video Indexer?

Video Indexer maakt gebruik van een eenvoudig prijs model voor betalen per gebruik op basis van de duur van de inhouds invoer die u indexeert. Er kunnen extra kosten in rekening worden gebracht voor het coderen, streamen, opslaan, netwerk gebruik en gereserveerde media-eenheden. Zie de pagina met [prijzen](https://azure.microsoft.com/pricing/details/cognitive-services/video-indexer/) voor meer informatie.

### <a name="when-am-i-billed-for-using-video-indexer"></a>Wanneer ben ik gefactureerd voor het gebruik van Video Indexer?

Wanneer u een video opstuurt om deze te laten indexeren, geeft u aan dat de video, de audio of beide moeten worden geanalyseerd. Op basis hiervan wordt bepaald voor welke SKU's kosten in rekening worden gebracht. Als er tijdens de verwerking een kritieke fout optreedt, wordt er als antwoord een foutcode geretourneerd. In een dergelijk geval worden er geen kosten in rekening gebracht.  Kritieke fouten kunnen worden veroorzaakt door een bug in onze code of een kritiek probleem met een interne afhankelijkheid van de service. Bij fouten zoals onjuiste verificatie of de onjuiste extractie van inzichten, wordt er gewoon een antwoord geretourneerd; deze fouten zijn niet kritiek. Als er een geldig antwoord wordt geretourneerd (geen foutcode), worden er kosten in rekening gebracht.
 
### <a name="does-video-indexer-offer-a-free-trial"></a>Biedt Video Indexer een gratis proef versie?

Ja, Video Indexer biedt een gratis proef versie die volledige service-en API-functionaliteit biedt. Er is een quotum van 600 minuten aan Video's voor web-interface gebruikers en 2.400 minuten voor API-gebruikers. 

## <a name="next-steps"></a>Volgende stappen

[Overzicht](video-indexer-overview.md)
