---
title: Overzicht van Linkerd
description: Een overzicht van Linkerd verkrijgen
author: paulbouwer
ms.service: container-service
ms.topic: article
ms.date: 10/09/2019
ms.author: pabouwer
ms.openlocfilehash: 9d0a65f2cf557120ec5a551494e697c876ad5c59
ms.sourcegitcommit: f29fec8ec945921cc3a89a6e7086127cc1bc1759
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 10/17/2019
ms.locfileid: "72530210"
---
# <a name="linkerd"></a>Linkerd

## <a name="overview"></a>Overzicht

[Linkerd][linkerd] is een eenvoudig te gebruiken en Lightweight service-net.

## <a name="architecture"></a>Architectuur

Linkerd biedt een gegevensvlak dat bestaat uit Ultralight [Linkerd][linkerd-proxy] gespecialiseerde proxy-zijspaners. Deze intelligente proxy's bepalen al het netwerk verkeer in en uit uw gestuurde apps en workloads. De proxy's geven ook metrische gegevens weer via [Prometheus][prometheus] Metrics-eind punten.

Het besturings vlak beheert de configuratie en geaggregeerde telemetrie via de volgende [onderdelen][linkerd-architecture]:

- **Controller** -biedt een API waarmee de Linkerd CLI en het dash board worden gedrivesd. Biedt configuratie voor proxy's.

- **Tik** om realtime controles uit te voeren op aanvragen en antwoorden.

- **Identiteit** : biedt identiteits-en beveiligings mogelijkheden waarmee mTLS tussen services worden toegestaan.

- **Web** : biedt het Linkerd-dash board.


In het volgende architectuur diagram ziet u hoe de verschillende onderdelen binnen het vlak van de gegevens en het besturings element werken.


![Overzicht van Linkerd-onderdelen en-architectuur.](media/servicemesh/linkerd/about-architecture.png)


## <a name="selection-criteria"></a>Selectie criteria

Het is belang rijk dat u de volgende gebieden begrijpt en overweegt om Linkerd te evalueren voor uw workloads:

- [Ontwerp principes](#design-principles)
- [Bieden](#capabilities)
- [Scenario's](#scenarios)


### <a name="design-principles"></a>Ontwerpprincipes

De volgende ontwerp principes [begeleiden][design-principles] het Linkerd-project:

- **U kunt het eenvoudig blijven** gebruiken. dit moet eenvoudig zijn.

- **Minimaliseer de resource vereisten** : minimale prestaties en resource kosten.

- U hoeft **alleen maar** de bestaande toepassingen te onderbreken en geen ingewikkelde configuratie te hoeven uitvoeren.


### <a name="capabilities"></a>Mogelijkheden

Linkerd biedt de volgende mogelijkheden:

- **Net** -ingebouwd in de optie fout opsporing

- **Verkeers beheer** – splitsen, time-outs, nieuwe pogingen, binnenkomend

- **Security** – Encryption (mTLS), certificaten automatisch geroteerd om de 24 uur

- Behulp van behulp van behulp van een gouden metrische waarde, tikken, traceren, service profielen en metrische gegevens van de route, webdashboard met topologie grafieken, Prometheus, grafana


### <a name="scenarios"></a>Scenario's

Linkerd is goed geschikt voor de volgende scenario's:

- Eenvoudig te gebruiken met alleen de essentiële set mogelijkhedens vereisten

- Lage latentie, lage overhead, met focus op de naleving en het eenvoudige verkeer beheer


## <a name="next-steps"></a>Volgende stappen

In de volgende documentatie wordt beschreven hoe u Linkerd kunt installeren in azure Kubernetes service (AKS):

> [!div class="nextstepaction"]
> [Linkerd installeren in azure Kubernetes service (AKS)][linkerd-install]

U kunt ook nog verder Linkerd-functies en-architectuur verkennen:

- [Linkerd-functies][linkerd-features]
- [Linkerd-architectuur][linkerd-architecture]

<!-- LINKS - external -->
[linkerd]: https://linkerd.io/2/overview/
[linkerd-architecture]: https://linkerd.io/2/reference/architecture/
[linkerd-features]: https://linkerd.io/2/features/
[design-principles]: https://linkerd.io/2/design-principles/
[linkerd-proxy]: https://github.com/linkerd/linkerd2-proxy

[grafana]: https://grafana.com/
[prometheus]: https://prometheus.io/

<!-- LINKS - internal -->
[linkerd-install]: ./servicemesh-linkerd-install.md