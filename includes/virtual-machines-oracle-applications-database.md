---
author: dlepow
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 07/10/2019
ms.author: danlep
ms.openlocfilehash: 8106b1a77f11874b1b13f41bfc9e62ea800afc98
ms.sourcegitcommit: 4b647be06d677151eb9db7dccc2bd7a8379e5871
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 07/19/2019
ms.locfileid: "68361529"
---
### <a name="database-tier"></a>Database-laag

De data base-laag bevat de data base-exemplaren voor de toepassing. De data base kan een Oracle DB, Oracle RAC of Oracle Exadata data base system zijn. 

Als u Oracle DB wilt gebruiken, kan het data base-exemplaar op Azure worden geïmplementeerd via de Oracle DB installatie kopieën die beschikbaar zijn op de Azure Marketplace. U kunt ook de Interconnect tussen Azure en OCI gebruiken om de Oracle DB te implementeren in een PaaS-model op OCI.

Voor Oracle RAC kunt u Oracle RAC implementeren op Azure CloudSimple in IaaS-model of in OCI in het PaaS-model. Het is raadzaam om een RAC-systeem met twee knoop punten te gebruiken. 

Ten slotte gebruikt u voor Exadata-systemen de OCI Interconnect en implementeert u het Exadata-systeem in OCI. In het bovenstaande voor gaande architectuur diagram ziet u een Exadata-systeem dat is geïmplementeerd in OCI over twee subnetten.

Voor productie scenario's implementeert u meerdere exemplaren van de Data Base over twee beschikbaarheids zones (bij implementatie in Azure) of twee beschik bare domeinen (in OCI). Oracle Active Data Guard gebruiken om de primaire en standby-data bases te synchroniseren.

De data base-laag ontvangt alleen aanvragen van de middelste laag. Het is raadzaam om een netwerk beveiligings groep in te stellen (beveiligings lijst als u de data base in OCI implementeert) zodat alleen aanvragen op poort 1521 van de middelste laag en poort 22 van de Bastion-server om administratieve redenen worden toegestaan.

Voor data bases die zijn geïmplementeerd in OCI, moet een afzonderlijk virtueel Cloud netwerk worden ingesteld met een DRG (Dynamic Routing gateway) die is verbonden met uw FastConnect-circuit.