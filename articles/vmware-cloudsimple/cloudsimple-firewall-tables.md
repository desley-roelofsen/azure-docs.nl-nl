---
title: Azure VMware-oplossing op CloudSimple-firewall tabellen
description: Meer informatie over CloudSimple-firewall tabellen en firewall regels voor de privécloud.
author: sharaths-cs
ms.author: dikamath
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 89bef6cef48f2b972aa3f931008b0db84431b832
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 08/21/2019
ms.locfileid: "69877718"
---
# <a name="firewall-tables-overview"></a>Overzicht van Firewall tabellen

Een firewall tabel bevat regels voor het filteren van netwerk verkeer naar en van de Privécloud. U kunt Firewall tabellen Toep assen op een VLAN/subnet. De regels bepalen netwerk verkeer tussen een bron netwerk of IP-adres en een bestemmings netwerk of IP-adres.

## <a name="firewall-rules"></a>Firewallregels

In de volgende tabel worden de para meters in een firewall regel beschreven.

| Eigenschap | Details |
| ---------| --------|
| **Name** | Een naam die een unieke identificatie vormt van de firewall regel en het doel ervan. |
| **prioriteit** | Een getal tussen 100 en 4096, met 100 de hoogste prioriteit. Regels worden in volg orde van prioriteit verwerkt. Wanneer het verkeer een regel overeenkomst tegen komt, stopt de verwerking van de regel. Als gevolg hiervan worden regels met een lagere prioriteit die dezelfde kenmerken hebben als regels met hogere prioriteiten, niet verwerkt.  Wees voorzichtig om conflicterende regels te voor komen. |
| **Status bijhouden** | Bijhouden kan stateless (Privécloud, Internet of VPN) of stateful (openbaar IP-adres) zijn.  |
| **Protocol** | De opties zijn onder andere TCP of UDP. Als ICMP vereist is, gebruikt u een. |
| **Richting** | Hiermee wordt aangegeven of de regel van toepassing is op binnenkomend of uitgaand verkeer. |
| **Actie** | Het type verkeer dat in de regel is gedefinieerd, toestaan of weigeren. |
| **Bron** | Een IP-adres, een CIDR-blok (Classless Inter-Domain Routing) (10.0.0.0/24) of een wille keurig.  Als u een bereik, een servicetag of een toepassings beveiligings groep opgeeft, kunt u minder beveiligings regels maken. |
| **Bron poort** | Poort van waaruit het netwerk verkeer afkomstig is.  U kunt een afzonderlijke poort of een bereik van poorten opgeven, zoals 443 of 8000-8080. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken. |
| **Destination** | Een IP-adres, een CIDR-blok (Classless Inter-Domain Routing) (10.0.0.0/24) of een wille keurig.  Als u een bereik, een servicetag of een toepassings beveiligings groep opgeeft, kunt u minder beveiligings regels maken.  |
| **Doel poort** | Poort waarnaar het netwerk verkeer loopt.  U kunt een afzonderlijke poort of een bereik van poorten opgeven, zoals 443 of 8000-8080. Als u bereiken opgeeft, hoeft u minder beveiligingsregels te maken.|

### <a name="stateless"></a>Stateless

Een staatloze regel ziet er alleen uit bij afzonderlijke pakketten en filtert deze op basis van de regel.  
Er zijn mogelijk extra regels vereist voor de verkeers stroom in de omgekeerde richting.  Gebruik stateless regels voor verkeer tussen de volgende punten:

* Subnetten van persoonlijke Clouds
* On-premises subnet en een persoonlijk Cloud subnet
* Internet verkeer van de Privécloud

### <a name="stateful"></a>Stateful

 Een stateful regel is op de hoogte van de verbindingen die het passeren. Voor bestaande verbindingen wordt een stroomrecord gemaakt. Communicatie wordt toegestaan of geweigerd op basis van de verbindingsstatus van de stroomrecord.  Gebruik dit regel type voor open bare IP-adressen voor het filteren van verkeer van het internet.

### <a name="default-rules"></a>Standaardregels

In elke firewall tabel worden de volgende standaard regels gemaakt.

|Priority|Name|Status bijhouden|Direction|Verkeers type|Protocol|Source|Bronpoort|Doel|Doelpoort|Action|
|--------|----|--------------|---------|------------|--------|------|-----------|-----------|----------------|------|
|65000|alles-naar-Internet toestaan|Stateful|Uitgaande|Openbaar IP-of Internet verkeer|Alle|Any|Any|Any|Any|Allow|
|65001|weigeren: alle van Internet|Stateful|Inkomend|Openbaar IP-of Internet verkeer|Alle|Any|Any|Any|Any|Weigeren|
|65002|alles-naar-intranet toestaan|Stateless|Uitgaande|Intern of VPN-verkeer in de privécloud|Alle|Any|Any|Any|Any|Allow|
|65003|toestaan-alles-uit-intranet|Stateless|Inkomend|Intern of VPN-verkeer in de privécloud|Alle|Any|Any|Any|Any|Allow|

## <a name="next-steps"></a>Volgende stappen

* [Firewall tabellen en-regels instellen](firewall.md)
