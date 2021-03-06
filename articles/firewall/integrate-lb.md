---
title: Azure Firewall integreren met Azure Standard Load Balancer
description: U kunt een Azure Firewall integreren in een virtueel netwerk met een Azure Standard Load Balancer (openbaar of intern).
services: firewall
author: vhorne
ms.service: firewall
ms.topic: article
ms.date: 11/19/2019
ms.author: victorh
ms.openlocfilehash: 91f34d06532b2d7f56d293df40939212a4f3d68c
ms.sourcegitcommit: 4821b7b644d251593e211b150fcafa430c1accf0
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74167069"
---
# <a name="integrate-azure-firewall-with-azure-standard-load-balancer"></a>Azure Firewall integreren met Azure Standard Load Balancer

U kunt een Azure Firewall integreren in een virtueel netwerk met een Azure Standard Load Balancer (openbaar of intern). 

Het ontwerp van de voor keur is het integreren van een interne load balancer met uw Azure-firewall, omdat dit een veel eenvoudiger ontwerp is. U kunt een open bare load balancer gebruiken als u er al een hebt geïmplementeerd en u deze wilt laten staan. U moet echter wel op de hoogte zijn van een asymmetrisch routerings probleem dat de functionaliteit van het open bare load balancer scenario kan verstoren.

Zie [Wat is Azure Load Balancer?](../load-balancer/load-balancer-overview.md) voor meer informatie over Azure Load Balancer.

## <a name="public-load-balancer"></a>Open bare load balancer

Met een open bare load balancer wordt de load balancer geïmplementeerd met een openbaar frontend-IP-adres.

### <a name="asymmetric-routing"></a>Asymmetrisch routeren

Asymmetrische route ring houdt in dat een pakket één pad naar de bestemming neemt en een ander pad gebruikt bij het retour neren naar de bron. Dit probleem treedt op wanneer een subnet een standaard route heeft naar het privé-IP-adres van de firewall en u een open bare load balancer gebruikt. In dit geval wordt het binnenkomende load balancer verkeer ontvangen via het open bare IP-adres, maar het retour pad gaat via het privé-IP-adres van de firewall. Omdat de firewall is stateful, wordt het terugkerende pakket neergezet omdat de firewall niet op de hoogte is van een dergelijke vastgelegde sessie.

### <a name="fix-the-routing-issue"></a>Het routerings probleem oplossen

Wanneer u een Azure Firewall in een subnet implementeert, is één stap het maken van een standaard route voor het subnet omleiden van pakketten via het privé-IP-adres van de firewall dat zich op de AzureFirewallSubnet bevindt. Zie voor meer informatie [zelf studie: Azure firewall implementeren en configureren met behulp van de Azure Portal](tutorial-firewall-deploy-portal.md#create-a-default-route).

Wanneer u de firewall in uw load balancer-scenario introduceert, wilt u dat uw Internet verkeer binnenkomt via het open bare IP-adres van uw firewall. Vanaf daar past de firewall de firewall regels en nat de pakketten toe aan het open bare IP-adres van uw load balancer. Dit is de plaats waar het probleem optreedt. Pakketten arriveren op het open bare IP-adres van de firewall, maar u kunt teruggaan naar de firewall via het privé-IP-adres (met behulp van de standaard route).
U kunt dit probleem voor komen door een extra hostroute te maken voor het open bare IP-adres van de firewall. Pakketten die naar het open bare IP-adres van de firewall gaan, worden via Internet gerouteerd. Dit voor komt dat de standaard route naar het privé-IP-adres van de firewall wordt geleid.

![Asymmetrisch routeren](media/integrate-lb/Firewall-LB-asymmetric.png)

De volgende routes zijn bijvoorbeeld voor een firewall op openbaar IP-adres 13.86.122.41 en privé IP-adres 10.3.1.4.

![Routetabel](media/integrate-lb/route-table.png)

## <a name="internal-load-balancer"></a>Interne load balancer

Met een interne load balancer wordt de load balancer geïmplementeerd met een privé-frontend-IP-adres.

Er is geen asymmetrisch probleem met de route ring met dit scenario. De binnenkomende pakketten arriveren op het open bare IP-adres van de firewall, worden omgezet naar het privé-IP-adres van de load balancer en vervolgens keert u terug naar het privé-IP-adres van de firewall met hetzelfde retour pad.

U kunt dit scenario dus op dezelfde manier implementeren als het open bare load balancer scenario, maar zonder dat de firewall openbaar IP-adres van de host moet worden gerouteerd.

## <a name="additional-security"></a>Extra beveiliging

Als u de beveiliging van uw scenario met taak verdeling verder wilt verbeteren, kunt u netwerk beveiligings groepen (Nsg's) gebruiken.

U kunt bijvoorbeeld een NSG maken op het back-end-subnet waar de virtuele machines met gelijke taak verdeling zich bevinden. Binnenkomend verkeer dat afkomstig is van het IP-adres/poort van de Firewall toestaan.

Zie voor meer informatie over Nsg's [beveiligings groepen](../virtual-network/security-overview.md).

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [implementeren en configureren van een Azure firewall](tutorial-firewall-deploy-portal.md).