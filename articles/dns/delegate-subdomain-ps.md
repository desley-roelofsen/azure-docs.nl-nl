---
title: Een subdomein-Azure PowerShell-Azure DNS delegeren
description: Met dit leer traject kunt u aan de slag met het overdragen van een Azure DNS subdomein met behulp van Azure PowerShell.
services: dns
author: asudbring
ms.service: dns
ms.topic: article
ms.date: 2/7/2019
ms.author: allensu
ms.openlocfilehash: b6023adb65469dedeeba664a51917306e634146f
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74212508"
---
# <a name="delegate-an-azure-dns-subdomain-using-azure-powershell"></a>Een Azure DNS subdomein delegeren met behulp van Azure PowerShell

U kunt Azure PowerShell gebruiken om een DNS-subdomein te delegeren. Als u bijvoorbeeld eigenaar bent van het domein contoso.com, kunt u een subdomein met de naam *engineering* delegeren naar een andere, afzonderlijke zone die u afzonderlijk kunt beheren vanuit de zone contoso.com.

Als u wilt, kunt u een subdomein delegeren met behulp van [Azure Portal](delegate-subdomain.md).

> [!NOTE]
> Contoso.com wordt in dit artikel als voor beeld gebruikt. Vervang uw eigen domeinnaam door contoso.com.

Als u nog geen abonnement op Azure hebt, maakt u een [gratis account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) aan voordat u begint.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prerequisites"></a>Vereisten

Als u een Azure DNS subdomein wilt delegeren, moet u uw open bare domein eerst delegeren naar Azure DNS. Zie [een domein delegeren voor Azure DNS](./dns-delegate-domain-azure-dns.md) voor instructies over het configureren van uw naam servers voor delegering. Zodra uw domein is overgedragen aan uw Azure DNS zone, kunt u uw subdomein delegeren.

## <a name="create-a-zone-for-your-subdomain"></a>Een zone maken voor uw subdomein

Maak eerst de zone voor het **technische** subdomein.

`New-AzDnsZone -ResourceGroupName <resource group name> -Name engineering.contoso.com`

## <a name="note-the-name-servers"></a>Noteer de naam servers

Noteer vervolgens de vier naam servers voor het subdomein van de technische afdeling.

`Get-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -RecordType NS`

## <a name="create-a-test-record"></a>Een test record maken

Maak een **A** -record in de technische zone die moet worden gebruikt voor het testen.

   `New-AzDnsRecordSet -ZoneName engineering.contoso.com -ResourceGroupName <resource group name> -Name www -RecordType A -ttl 3600 -DnsRecords (New-AzDnsRecordConfig -IPv4Address 10.10.10.10)`.

## <a name="create-an-ns-record"></a>Een NS-record maken

Maak vervolgens een NS-record (naam server) voor de **technische** zone in de zone contoso.com.

```azurepowershell
$Records = @()
$Records += New-AzDnsRecordConfig -Nsdname <name server 1 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 2 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 3 noted previously>
$Records += New-AzDnsRecordConfig -Nsdname <name server 4 noted previously>
$RecordSet = New-AzDnsRecordSet -Name engineering -RecordType NS -ResourceGroupName <resource group name> -TTL 3600 -ZoneName contoso.com -DnsRecords $Records
```

## <a name="test-the-delegation"></a>De delegering testen

Gebruik Nslookup om de overdracht te testen.

1. Open een Power shell-venster.
2. Typ `nslookup www.engineering.contoso.com.` op de opdracht prompt
3. U ontvangt een niet-bindend antwoord met het adres **10.10.10.10**.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het [configureren van omgekeerde DNS voor services die worden gehost in azure](dns-reverse-dns-for-azure-services.md).