---
title: Privé-eind punten gebruiken met Azure Storage | Microsoft Docs
description: Overzicht van persoonlijke eind punten voor beveiligde toegang tot opslag accounts van virtuele netwerken.
services: storage
author: santoshc
ms.service: storage
ms.topic: article
ms.date: 09/25/2019
ms.author: santoshc
ms.reviewer: santoshc
ms.subservice: common
ms.openlocfilehash: fff92057bc9812a5ef1488a46ed469382ad3ace3
ms.sourcegitcommit: 5aefc96fd34c141275af31874700edbb829436bb
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/04/2019
ms.locfileid: "74806878"
---
# <a name="using-private-endpoints-for-azure-storage-preview"></a>Privé-eind punten gebruiken voor Azure Storage (preview-versie)

U kunt [privé-eind punten](../../private-link/private-endpoint-overview.md) voor uw Azure Storage-accounts gebruiken om clients op een virtueel netwerk (VNet) toe te staan om veilig toegang te krijgen tot gegevens via een [privé-koppeling](../../private-link/private-link-overview.md). Het persoonlijke eind punt gebruikt een IP-adres uit de VNet-adres ruimte voor uw Storage-account service. Netwerk verkeer tussen de clients in het VNet en het opslag account gaat over het VNet en een persoonlijke koppeling in het micro soft-backbone-netwerk, waardoor de bloot stelling van het open bare Internet wordt voor komen.

Door gebruik te maken van privé-eind punten voor uw opslag account kunt u:
- Beveilig uw opslag account door de opslag firewall zodanig te configureren dat alle verbindingen op het open bare eind punt voor de opslag service worden geblokkeerd.
- Verbeter de beveiliging van het virtuele netwerk (VNet) door u in staat te stellen exfiltration van gegevens van het VNet te blok keren.
- Maak veilig verbinding met opslag accounts vanuit on-premises netwerken die verbinding maken met het VNet met behulp van [VPN-](../../vpn-gateway/vpn-gateway-about-vpngateways.md) of [expressroutes waaraan](../../expressroute/expressroute-locations.md) met privé-peering.

## <a name="conceptual-overview"></a>Conceptueel overzicht
![Overzicht van privé-eind punten voor Azure Storage](media/storage-private-endpoints/storage-private-endpoints-overview.jpg)

Een persoonlijk eind punt is een speciale netwerk interface voor een Azure-service in uw [Virtual Network](../../virtual-network/virtual-networks-overview.md) (VNet). Wanneer u een persoonlijk eind punt maakt voor uw opslag account, biedt het een beveiligde verbinding tussen clients op uw VNet en uw opslag. Het persoonlijke eind punt krijgt een IP-adres uit het IP-adres bereik van uw VNet. De verbinding tussen het persoonlijke eind punt en de opslag service maakt gebruik van een beveiligde persoonlijke koppeling.

Toepassingen in het VNet kunnen naadloos verbinding maken met de opslag service via het persoonlijke eind punt, **met behulp van dezelfde verbindings reeksen en autorisatie mechanismen die ze anders zouden gebruiken**. Privé-eind punten kunnen worden gebruikt met alle protocollen die worden ondersteund door het opslag account, inclusief REST en SMB.

Privé-eind punten kunnen worden gemaakt in subnetten die gebruikmaken van [service-eind punten](../../virtual-network/virtual-network-service-endpoints-overview.md). Clients in een subnet kunnen daarom verbinding maken met één opslag account met behulp van een persoonlijk eind punt, terwijl service-eind punten worden gebruikt voor toegang tot anderen.

Wanneer u een privé-eindpunt voor een opslagservice in uw VNet maakt, wordt er een aanvraag voor goedkeuring verzonden naar de eigenaar van het opslagaccount. Als de gebruiker die het persoonlijke eind punt wil maken ook eigenaar van het opslag account is, wordt deze aanvraag voor toestemming automatisch goedgekeurd.

Eigen aren van opslag accounts kunnen toestemmings aanvragen en de persoonlijke eind punten beheren via het tabblad*privé-eind punten*voor het opslag account in de [Azure Portal](https://portal.azure.com).

> [!TIP]
> Als u de toegang tot uw opslag account wilt beperken via het persoonlijke eind punt, configureert u de firewall voor opslag om toegang via het open bare eind punt te weigeren of te beheren.

U kunt uw opslag account beveiligen, zodat alleen verbindingen van uw VNet worden geaccepteerd door [de opslag firewall te configureren om de](storage-network-security.md#change-the-default-network-access-rule) toegang tot het open bare eind punt standaard te weigeren. U hebt geen firewall regel nodig om verkeer van een VNet met een persoonlijk eind punt toe te staan, omdat de opslag firewall alleen de toegang via het open bare eind punt beheert. In plaats daarvan wordt gebruikgemaakt van de toestemming stroom voor het verlenen van toegang tot de opslag service aan persoonlijke eind punten.

### <a name="private-endpoints-for-storage-service"></a>Persoonlijke eind punten voor de opslag service

Wanneer u het persoonlijke eind punt maakt, moet u het opslag account en de opslag service opgeven waarmee de verbinding tot stand wordt gebracht. U hebt een apart persoonlijk eind punt nodig voor elke opslag service in een opslag account waartoe u toegang nodig hebt, met name [blobs](../blobs/storage-blobs-overview.md), [Data Lake Storage Gen2](../blobs/data-lake-storage-introduction.md), [bestanden](../files/storage-files-introduction.md), [wacht rijen](../queues/storage-queues-introduction.md), [tabellen](../tables/table-storage-overview.md)of [statische websites](../blobs/storage-blob-static-website.md).

> [!TIP]
> Maak een apart persoonlijk eind punt voor het secundaire exemplaar van de opslag service voor betere Lees prestaties op de RA-GRS-accounts.

Voor lees Beschik baarheid voor een [geografisch redundant opslag account met lees toegang](storage-redundancy-grs.md#read-access-geo-redundant-storage)hebt u afzonderlijke persoonlijke eind punten nodig voor zowel de primaire als de secundaire instantie van de service. U hoeft geen persoonlijk eind punt te maken voor het secundaire exemplaar voor **failover**. Het persoonlijke eind punt maakt na een failover automatisch verbinding met het nieuwe primaire exemplaar.

#### <a name="resources"></a>Bronnen

Raadpleeg de volgende artikelen voor meer gedetailleerde informatie over het maken van een persoonlijk eind punt voor uw opslag account:

- [Een persoonlijke verbinding maken met een opslag account vanuit de ervaring voor het opslag account in de Azure Portal](../../private-link/create-private-endpoint-storage-portal.md)
- [Maak een persoonlijk eind punt met behulp van het privé koppelings centrum in de Azure Portal](../../private-link/create-private-endpoint-portal.md)
- [Een persoonlijk eind punt maken met behulp van Azure CLI](../../private-link/create-private-endpoint-cli.md)
- [Een persoonlijk eind punt maken met Azure PowerShell](../../private-link/create-private-endpoint-powershell.md)

### <a name="connecting-to-private-endpoints"></a>Verbinding maken met privé-eind punten

Clients op een VNet met behulp van het privé-eind punt moeten hetzelfde connection string gebruiken voor het opslag account, wanneer clients verbinding maken met het open bare eind punt. We vertrouwen op DNS-omzetting om de verbindingen van het VNet naar het opslag account via een persoonlijke koppeling automatisch te routeren.

> [!IMPORTANT]
> Gebruik hetzelfde connection string om verbinding te maken met het opslag account met behulp van privé-eind punten, zoals u anders zou gebruiken. Maak geen verbinding met het opslag account met behulp van de subdomein-URL '*privatelink*'.

Er wordt standaard een [privé-DNS-zone](../../dns/private-dns-overview.md) gekoppeld aan het VNet met de vereiste updates voor de privé-eind punten. Als u echter uw eigen DNS-server gebruikt, moet u mogelijk aanvullende wijzigingen aanbrengen in uw DNS-configuratie. In de volgende sectie over [DNS-wijzigingen](#dns-changes-for-private-endpoints) worden de vereiste updates voor persoonlijke eind punten beschreven.

## <a name="dns-changes-for-private-endpoints"></a>DNS-wijzigingen voor privé-eind punten

Wanneer u een persoonlijk eind punt maakt, wordt de DNS CNAME-bron record voor het opslag account bijgewerkt naar een alias in een subdomein met het voor voegsel '*privatelink*'. Standaard maken we ook een [privé-DNS-zone](../../dns/private-dns-overview.md), die overeenkomt met het subdomein '*privatelink*', met de DNS a-bron records voor de privé-eind punten.

Wanneer u de URL van het opslag eindpunt oplost van buiten het VNet met het persoonlijke eind punt, wordt dit omgezet in het open bare eind punt van de opslag service. Wanneer het is opgelost vanuit het VNet dat het persoonlijke eind punt host, wordt de URL van het opslag eindpunt omgezet naar het IP-adres van het privé-eind punt.

Voor het bovenstaande voor beeld is de DNS-bron records voor het opslag account ' StorageAccountA ', wanneer deze zijn omgezet van buiten het VNet dat als host fungeert voor het persoonlijke eind punt:

| Naam                                                  | Type  | Waarde                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | CNAME | \> open bare eind punt van \<Storage-service                   |
| \> open bare eind punt van \<Storage-service                   | A     | \<open bare IP-adres van opslag service\>                 |

Zoals eerder vermeld, kunt u de toegang tot clients buiten het VNet via het open bare eind punt met behulp van de opslag firewall weigeren of beheren.

De DNS-bron records voor StorageAccountA, wanneer deze zijn opgelost door een client in de VNet die als host fungeert voor het persoonlijke eind punt, zijn:

| Naam                                                  | Type  | Waarde                                                 |
| :---------------------------------------------------- | :---: | :---------------------------------------------------- |
| ``StorageAccountA.blob.core.windows.net``             | CNAME | ``StorageAccountA.privatelink.blob.core.windows.net`` |
| ``StorageAccountA.privatelink.blob.core.windows.net`` | A     | 10.1.1.5                                              |

Deze aanpak maakt het mogelijk toegang tot het opslag account te krijgen **met behulp van dezelfde Connection String** voor clients op het VNet dat als host fungeert voor de privé-eind punten, evenals clients buiten het vnet.

Als u een aangepaste DNS-server gebruikt in uw netwerk, moeten clients de FQDN-naam voor het eind punt van het opslag account kunnen omzetten naar het IP-adres van het privé-eind punt. U moet uw DNS-server zo configureren dat uw privé-koppelings subdomein wordt overgedragen aan de privé-DNS-zone voor het VNet, of dat u de A-records voor '*StorageAccountA.privatelink.blob.core.Windows.net*' configureert met het IP-adres van het privé-eind punt.

> [!TIP]
> Wanneer u een aangepaste of lokale DNS-server gebruikt, moet u de DNS-server zo configureren dat de naam van het opslag account in het subdomein ' privatelink ' wordt omgezet in het IP-adres van het privé-eind punt. U kunt dit doen door het subdomein ' privatelink ' te delegeren aan de privé-DNS-zone van het VNet of door de DNS-zone op de DNS-server te configureren en de DNS A-records toe te voegen.

De aanbevolen DNS-zone namen voor privé-eind punten voor opslag Services zijn:

| Opslag service        | Zone naam                            |
| :--------------------- | :----------------------------------- |
| Blob service           | `privatelink.blob.core.windows.net`  |
| Data Lake Storage Gen2 | `privatelink.dfs.core.windows.net`   |
| Bestandsservice           | `privatelink.file.core.windows.net`  |
| Queue-service          | `privatelink.queue.core.windows.net` |
| Table service          | `privatelink.table.core.windows.net` |
| Statische websites        | `privatelink.web.core.windows.net`   |

#### <a name="resources"></a>Bronnen

Raadpleeg de volgende artikelen voor meer informatie over het configureren van uw eigen DNS-server voor de ondersteuning van persoonlijke eind punten:

- [Naamomzetting voor resources in virtuele Azure-netwerken](/azure/virtual-network/virtual-networks-name-resolution-for-vms-and-role-instances#name-resolution-that-uses-your-own-dns-server)
- [DNS-configuratie voor privé-eind punten](/azure/private-link/private-endpoint-overview#dns-configuration)

## <a name="pricing"></a>Prijzen

Zie [prijzen voor persoonlijke Azure-koppelingen](https://azure.microsoft.com/pricing/details/private-link)voor prijs informatie.

## <a name="known-issues"></a>Bekende problemen

### <a name="copy-blob-support"></a>Ondersteuning voor BLOB kopiëren

Tijdens de preview-versie bieden we geen ondersteuning voor het [kopiëren van BLOB](https://docs.microsoft.com/rest/api/storageservices/Copy-Blob) -opdrachten die worden verleend aan opslag accounts die toegankelijk zijn via persoonlijke eind punten wanneer het bron opslag account wordt beveiligd door een firewall.

### <a name="storage-access-constraints-for-clients-in-vnets-with-private-endpoints"></a>Toegangs beperkingen voor opslag voor clients in VNets met privé-eind punten

Clients in VNets met bestaande persoonlijke eind punten geven gezichts beperkingen bij het openen van andere opslag accounts met persoonlijke eind punten. Stel bijvoorbeeld dat een VNet N1 een persoonlijk eind punt heeft voor een opslag account a1 voor, zeggen, de BLOB-service. Als opslag account a2 een persoonlijk eind punt in een VNet N2 heeft voor de BLOB-service, moeten clients in VNet N1 ook toegang krijgen tot de BLOB-service van account a2 met behulp van een persoonlijk eind punt. Als het opslag account a2 geen privé-eind punten voor de BLOB-service heeft, kunnen clients in VNet N1 toegang krijgen tot de BLOB-service zonder een persoonlijk eind punt.

Deze beperking is het gevolg van de DNS-wijzigingen die zijn aangebracht wanneer account a2 een persoonlijk eind punt maakt.

### <a name="network-security-group-rules-for-subnets-with-private-endpoints"></a>Regels voor netwerk beveiligings groepen voor subnetten met persoonlijke eind punten

Op dit moment kunt u geen NSG-regels ( [netwerk beveiligings groep](../../virtual-network/security-overview.md) ) configureren voor subnetten met persoonlijke eind punten. Een beperkte tijdelijke oplossing voor dit probleem is het implementeren van uw toegangs regels voor privé-eind punten op de bron-subnetten, hoewel deze benadering mogelijk een hogere beheer overhead nodig heeft.
