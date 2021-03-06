---
title: Security framework - Azure IoT Edge | Microsoft Docs
description: Meer informatie over de beveiliging, verificatie en autorisatie-normen die zijn gebruikt voor het ontwikkelen van Azure IoT Edge en moeten worden beschouwd als u uw oplossing ontwerpt
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 08/30/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 0820d7a5baaf3972b3895472707f6e7d9dfc6c11
ms.sourcegitcommit: ec2eacbe5d3ac7878515092290722c41143f151d
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/31/2019
ms.locfileid: "75552335"
---
# <a name="security-standards-for-azure-iot-edge"></a>Beveiligingsstandaarden voor Azure IoT Edge

Azure IoT Edge worden de Risico's opgelost die inherent zijn bij het verplaatsen van uw gegevens en analyses naar de intelligente rand. Het IoT Edge Security Standards-saldo is flexibel voor verschillende implementatie scenario's met de beveiliging die u van alle Azure-Services verwacht. 

IoT Edge worden uitgevoerd op verschillende mogelijkheden en modellen van hardware, ondersteunt verschillende besturings systemen en is van toepassing op verschillende implementatie scenario's. Het risico van een implementatie scenario is afhankelijk van factoren die eigendom zijn van de oplossing, geografie, gegevens gevoeligheid, privacy, toepassing verticaal en wettelijke vereisten. In plaats van concrete oplossingen voor specifieke scenario's te bieden, is IoT Edge een uitbreidbaar beveiligings raamwerk dat is gebaseerd op goed gegronde principes die zijn ontworpen voor schaal. 
 
Dit artikel bevat een overzicht van het IoT Edge Security-Framework. Zie voor meer informatie, [beveiligen van de intelligente edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).

## <a name="standards"></a>Standaarden

Standaarden bevorderen het gemak en het gemak van de implementatie, beide hallmarks van beveiliging. Een beveiligings oplossing zou zichzelf kunnen onderbouwen op basis van evaluatie om een vertrouwens relatie tot stand te brengen, en mag geen obstakel zijn voor de implementatie. Het ontwerp van het framework om Azure IoT Edge te beveiligen, is gebaseerd op tijdgeteste en door de industrie bewezen beveiligings protocollen voor vertrouwdheid en hergebruik. 

## <a name="authentication"></a>Verificatie

Wanneer u een IoT-oplossing implementeert, moet u weten dat alleen vertrouwde actors, apparaten en modules toegang hebben tot uw oplossing. Verificatie op basis van certificaten is het primaire mechanisme voor verificatie voor het Azure IoT Edge-platform. Dit mechanisme is afgeleid van een reeks standaarden die zijn gebaseerd op de open bare-sleutel infrastructuur (PKiX) door de IETF (Internet Engineering Task Force).     

Alle apparaten, modules en actors die communiceren met het Azure IoT Edge apparaat, hetzij fysiek of via een netwerk verbinding, moeten unieke certificaat identiteiten hebben. Niet elk scenario of onderdeel kan zichzelf onderlenen aan verificatie op basis van certificaten, waardoor de uitbreid baarheid van het beveiligings raamwerk veilige alternatieven biedt. 

Zie [Azure IOT Edge certificaat gebruik](iot-edge-certs.md)voor meer informatie.

## <a name="authorization"></a>Autorisatie

Het principe van minimale bevoegdheden geeft aan dat gebruikers en onderdelen van een systeem alleen toegang moeten hebben tot de minimale set resources en gegevens die nodig zijn om hun rollen uit te voeren. Apparaten, modules en actors moeten alleen toegang hebben tot de bronnen en gegevens binnen hun machtigings bereik, en alleen wanneer de architectuur is toegestaan. Sommige machtigingen kunnen worden geconfigureerd met voldoende bevoegdheden en andere worden op architectuur afgedwongen. Sommige modules kunnen bijvoorbeeld worden gemachtigd om verbinding te maken met Azure IoT Hub. Er is echter geen reden waarom een module in één IoT Edge apparaat toegang moet hebben tot de twee modules in een ander IoT Edge apparaat.

Andere autorisatie schema's zijn onder andere machtigingen voor certificaat ondertekening en op rollen gebaseerd toegangs beheer (RBAC). 

## <a name="attestation"></a>Attestation

Attestation zorgt voor de integriteit van software-bits, die belang rijk is voor het detecteren en voor komen van malware. Het Azure IoT Edge security framework classificeert attestation in drie hoofdcategorieën:

* Statische attestation
* Runtime-attestation
* Software-attestation

### <a name="static-attestation"></a>Statische attestation

Bij statische Attestation wordt de integriteit van alle software op een apparaat gecontroleerd tijdens het opstarten, met inbegrip van het besturings systeem, alle Runtimes en configuratie-informatie. Omdat er tijdens het opstarten statische attestatie plaatsvindt, wordt vaak beveiligd opstarten genoemd. Het beveiligings raamwerk voor IoT Edge-apparaten wordt uitgebreid naar fabrikanten en beschikt over veilige hardware-mogelijkheden die een statische Attestation-proces garanderen. Deze processen zijn onder andere beveiligd opstarten en beveiligde firmware-upgrade. Als u werkt in nauwe samen werking met silicium leveranciers, elimineert u overbodige firmware lagen, waardoor het bedreigings oppervlak wordt geminimaliseerd. 

### <a name="runtime-attestation"></a>Runtime-attestation

Wanneer een systeem een beveiligd opstart proces heeft voltooid, moeten goed ontworpen systemen pogingen voor het invoeren van malware en de juiste tegen maatregelen worden gedetecteerd. Aanvallen van schadelijke software zijn mogelijk gericht op de poorten en interfaces van het systeem. Als kwaad aardige actoren fysieke toegang tot een apparaat hebben, kunnen ze met het apparaat zelf knoeien of gebruikmaken van Side-Channel-aanvallen om toegang te krijgen. Dergelijke Malcontent, ongeacht of er malware of niet-geautoriseerde configuratie wijzigingen zijn, kunnen niet worden gedetecteerd door een statische Attestation omdat deze wordt ingevoegd na het opstart proces. Tegen maatregelen die worden geboden of afgedwongen door de hardware van het apparaat, kunnen deze bedreigingen ook buiten. Het beveiligings raamwerk voor IoT Edge expliciete uitbrei dingen aanroept voor het bestrijden van runtime bedreigingen.  

### <a name="software-attestation"></a>Software-attestation

Alle gezonde systemen, waaronder intelligente systemen, hebben patches en upgrades nodig. Beveiliging is belang rijk voor update processen, anders kunnen ze potentiële bedreigings vectoren zijn. Het beveiligings raamwerk voor IoT Edge-aanroepen voor updates via gemeten en ondertekende pakketten om de integriteit van de pakketten te waarborgen en te verifiëren. Deze standaard is van toepassing op alle besturings systemen en toepassings software-bits. 

## <a name="hardware-root-of-trust"></a>Hardware-vertrouwensbasis

Voor veel intelligente apparaten, met name apparaten die fysiek kunnen worden gebruikt door potentiële schadelijke actors, is Hardware Security de laatste verdedigings linie. Moeilijk te vervalsen hardware is van cruciaal belang voor dergelijke implementaties. Azure IoT Edge moedigt productleveranciers van siliconen aan verschillende soorten vertrouwens basis van de hardware te bieden voor verschillende risico profielen en implementatie scenario's. Het is mogelijk dat de hardware-vertrouwens relatie afkomstig is van algemene beveiligings protocol standaarden zoals Trusted Platform Module (ISO/IEC 11889) en de apparaat-id compositie-engine (dobbel stenen) van Trusted Computing Group. Beveiligde enclave-technologieën zoals TrustZones en software Guard Extensions (SGX) bieden ook een hardware-vertrouwens relatie. 

## <a name="certification"></a>Certificering

Om klanten te helpen weloverwogen beslissingen te nemen bij het inkopen van Azure IoT Edge-apparaten voor hun implementatie, bevat het IoT Edge-Framework certificerings vereisten. Fundamentele aan deze vereisten zijn met betrekking tot beveiligingsclaims-certificeringen en certificeringen die betrekking hebben op de validatie van de implementatie van beveiliging. Een beveiligings claim geeft bijvoorbeeld aan dat het IoT Edge apparaat gebruikmaakt van beveiligde hardware waarmee opstart aanvallen kunnen worden voors tand. Een validatie certificering betekent dat de beveiligde hardware op de juiste wijze is geïmplementeerd om deze waarde in het apparaat aan te bieden. In overeenstemming met het beginsel van de eenvoud, probeert het Framework de belasting van de certificering mini maal te houden.   

## <a name="extensibility"></a>Uitbreidbaarheid

Met IoT-technologie die verschillende soorten trans formaties verstuurt, moet beveiliging parallel worden ontwikkeld om nieuwe scenario's te verhelpen. Het Azure IoT Edge security framework begint met een solide basis waarop deze wordt gemaakt in uitbreidbaarheid in verschillende dimensies om op te nemen: 

* Eerste security services van derden, zoals de Device Provisioning Service voor Azure IoT Hub.
* Services van derden zoals beheerde beveiligings Services voor verschillende verticale toepassingen (zoals industriële of gezondheids zorg) of technologie focus (zoals beveiligings bewaking in netnetwerken of Silicon hardware Attestation Services) via een uitgebreid netwerk van partner.
* Verouderde systemen om op te nemen met alternatieve strategieën, aanpassing, zoals met behulp van veilige technologie dan certificaten voor verificatie en identiteitsbeheer.
* Veilige hardware voor de invoering van opkomende technologieën voor veilige hardware en bijdragen aan silicium partners.

In het eind is het beveiligen van de intelligente Edge vereist voor gezamenlijke bijdragen van een open Community door de gemeen schappelijke interesse in het beveiligen van IoT. Deze bijdragen mogelijk in de vorm van beveiligde technologieën of services. Het Azure IoT Edge security framework biedt een solide basis voor beveiliging dat kan worden uitgebreid voor de maximale dekking te bieden dezelfde mate van vertrouwen en integriteit in de intelligente edge als met Azure-cloud.  

## <a name="next-steps"></a>Volgende stappen

Meer informatie over hoe Azure IoT Edge is [beveiligen van de intelligente edge](https://azure.microsoft.com/blog/securing-the-intelligent-edge/).
