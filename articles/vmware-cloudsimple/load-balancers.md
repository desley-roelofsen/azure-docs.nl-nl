---
title: 'Azure VMware-oplossing door CloudSimple: een oplossing voor taak verdeling kiezen voor CloudSimple-persoonlijke Clouds'
description: Hierin worden de opties voor taak verdeling beschreven voor het implementeren van een toepassing in een Privécloud
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: d26eb0160316737c9ad31d98c8cf23bdcad42d32
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/20/2019
ms.locfileid: "74206503"
---
# <a name="choose-a-load-balancing-solution-for-cloudsimple-private-clouds"></a>Een taakverdelings oplossing kiezen voor CloudSimple persoonlijke Clouds

Wanneer u een toepassing implementeert in een Privécloud in CloudSimple, kunt u kiezen uit verschillende opties voor taak verdeling.

U kunt een virtuele-of software-load balancer in uw CloudSimple-privécloud kiezen of zelfs Azure N7 gebruiken load balancer die worden uitgevoerd in uw Azure-abonnement om de front-end van uw weblaag te starten die wordt uitgevoerd in de Privécloud van CloudSimple. Hier worden enkele opties vermeld:

## <a name="virtual-load-balancers"></a>Virtuele load balancers

U kunt virtuele load balancer-apparaten implementeren in uw VMware-omgeving via de vCenter-interface en ze configureren om het verkeer van de front-end van uw toepassing te beëindigen.

Enkele populaire leveranciers zijn: NginX: http://nginx.org/en/docs/http/load_balancing.html F5-BigIP-Traffic Manager: https://www.f5.com/products/big-ip-services/virtual-editions Citrix ADC: https://www.citrix.com/products/citrix-adc/

## <a name="azure-l7-load-balancer"></a>Azure N7-load balancer

Wanneer u Azure-toepassing gateway als een N7-load balancer gebruikt voor uw toepassing die in een Privécloud wordt uitgevoerd, hoeft u de load balancer software niet te beheren. De load balancer-software wordt beheerd door Azure. Alle virtuele machines op de weblaag in de Privécloud gebruiken privé-IP-adressen en vereisen geen aanvullende NAT-regels of open bare IP-adressen voor het omzetten van namen. Vm's met een weblaag communiceren met de Azure-toepassing-gateway via een particuliere verbinding met lage latentie en een hoge band breedte.

Voor meer informatie over het configureren van deze oplossing raadpleegt u de hand leiding voor oplossingen over het gebruik van Azure-toepassing gateway als een N7-load balancer.

## <a name="azure-internal-load-balancer"></a>Interne load balancer van Azure

Als u ervoor kiest om uw toepassing uit te voeren in een hybride implementatie waarbij de web-front-end-laag wordt uitgevoerd in een Azure-vNet in uw Azure-abonnement en de DB-laag van de toepassing wordt uitgevoerd in virtuele VMware-machines in CloudSimple-Privécloud, kunt u de interne belasting van Azure gebruiken de balancer (N4 load balancer) vóór de Vm's van de DB-laag voor verkeer beheer.

Zie de documentatie van Azure [Internal Load Balancer](../load-balancer/load-balancer-overview.md#internalloadbalancer) voor meer informatie.

## <a name="global-server-load-balancer"></a>load balancer globale server

Als u op zoek bent naar een op DNS gebaseerde load balancer, kunt u gebruikmaken van oplossingen van derden die beschikbaar zijn in azure Marketplace of met de systeem eigen Azure-oplossing.

Azure Traffic Manager is een op DNS gebaseerd verkeer load balancer waarmee u verkeer optimaal kunt distribueren naar Services in wereld wijde Azure-regio's en on-premises, met hoge Beschik baarheid en reactie tijd. Zie Azure [Traffic Manager](../traffic-manager/traffic-manager-configure-geographic-routing-method.md) -documentatie voor meer informatie.
