---
title: Systeemvereisten voor Microsoft Azure Data Box zware | Microsoft Docs
description: Meer informatie over de software en netwerkvereisten voor uw Azure Data Box zware
services: databox
author: alkohli
ms.service: databox
ms.subservice: heavy
ms.topic: article
ms.date: 07/03/2019
ms.author: alkohli
ms.openlocfilehash: 6097dba9193fce4c8158137b488080107881994a
ms.sourcegitcommit: 64798b4f722623ea2bb53b374fb95e8d2b679318
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/11/2019
ms.locfileid: "67839784"
---
# <a name="azure-data-box-heavy-system-requirements"></a>Azure Data Box zware systeemvereisten

Dit artikel beschrijft de belangrijke systeemvereisten voor uw Azure Data Box zware apparaat en de clients die verbinding maken met het apparaat. Het is raadzaam om dat u de informatie zorgvuldig controleren voordat u uw Data Box-zwaar implementeert, en vervolgens terug naar deze zo nodig tijdens de implementatie en het volgende gebruik verwijzen.

De systeemvereisten zijn onder andere:

* **Softwarevereisten voor hosts die verbinding maken met gegevens in het zware** -beschrijving van de ondersteunde platforms, browsers voor de lokale web-UI, SMB-clients en eventuele bijkomende vereisten voor hosts die verbinding met de Data Box maken kunnen.
* **Netwerkvereisten voor de Data Box-zwaar** -vindt u informatie over de vereisten voor de optimale werking van de gegevens in het zware apparaat.

## <a name="software-requirements"></a>Softwarevereisten

De vereisten voor bevatten de gegevens op de ondersteunde besturingssystemen, de ondersteunde browsers voor de lokale web-UI en de SMB-clients.

### <a name="supported-operating-systems-for-clients"></a>Ondersteunde besturingssystemen voor clients

[!INCLUDE [data-box-supported-os-clients](../../includes/data-box-supported-os-clients.md)]

### <a name="supported-file-systems-for-linux-clients"></a>Ondersteunde bestandssystemen voor Linux-clients

[!INCLUDE [data-box-supported-file-systems-clients](../../includes/data-box-supported-file-systems-clients.md)]

### <a name="supported-storage-accounts"></a>Ondersteunde opslagaccounts

[!INCLUDE [data-box-supported-storage-accounts](../../includes/data-box-supported-storage-accounts.md)]

### <a name="supported-storage-types"></a>Ondersteunde opslagtypen

[!INCLUDE [data-box-supported-storage-types](../../includes/data-box-supported-storage-types.md)]

### <a name="supported-web-browsers"></a>Ondersteunde webbrowsers

[!INCLUDE [data-box-supported-web-browsers](../../includes/data-box-supported-web-browsers.md)]

## <a name="networking-requirements"></a>Netwerkvereisten

Uw datacenter moet een netwerk met hoge snelheid hebben. Voor de snelste kopie snelheden worden bereikt, kunnen twee 40-GbE-verbindingen parallel (één per knooppunt) worden gebruikt. Als u geen 40-GbE beschikbaar hebt, wordt u aangeraden dat u ten minste twee 10 GbE-verbindingen (één per knooppunt hebt).

### <a name="port-requirements"></a>Port requirements for Windows

De volgende tabel staan de poorten die moeten worden geopend in uw firewall om toe te staan voor SMB- of NFS-verkeer. In deze tabel *in* of *inkomende* verwijst naar de richting van welke binnenkomende client aanvragen toegang tot uw apparaat. *Uit* of *uitgaande* verwijst naar de richting waarin uw gegevens in het zware apparaat extern, gegevens dan de implementatie verzendt: bijvoorbeeld uitgaand naar het Internet.

[!INCLUDE [data-box-port-requirements](../../includes/data-box-port-requirements.md)]

## <a name="next-steps"></a>Volgende stappen

* [Implementeren van uw Azure Data Box](data-box-deploy-ordered.md)
