---
title: Kan geen knoop punten toevoegen aan een Azure HDInsight-cluster
description: Problemen oplossen waarom kan geen knoop punten toevoegen aan Apache Hadoop cluster in azure HDInsight
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/31/2019
ms.openlocfilehash: 254af667ce0893855191a2f785cc77e560408d73
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087295"
---
# <a name="scenario-unable-to-add-nodes-to-azure-hdinsight-cluster"></a>Scenario: Kan geen knoop punten toevoegen aan een Azure HDInsight-cluster

In dit artikel worden de stappen beschreven voor het oplossen van problemen en mogelijke oplossingen voor problemen bij het werken met Azure HDInsight-clusters.

## <a name="issue"></a>Probleem

Kan geen knoop punten toevoegen aan een Azure HDInsight-cluster.

## <a name="cause"></a>Oorzaak

Redenen kunnen variëren.

## <a name="resolution"></a>Oplossing

Gebruik de functie [cluster grootte](../hdinsight-scaling-best-practices.md) om het aantal extra kernen te berekenen dat nodig is voor het cluster. Dit wordt gebaseerd op het totale aantal cores in de nieuwe werkrolknooppunten. Probeer vervolgens een of meer van de volgende stappen:

* Controleer of er kernen beschikbaar zijn op de locatie van het cluster.

* Bekijk het aantal beschikbare cores op andere locaties. Overweeg het cluster opnieuw te maken op een andere locatie met voldoende beschikbare cores.

* Als u het aantal cores voor een specifieke locatie wilt verhogen, maakt u een ondersteuningsticket aan voor verhoging van het aantal HDInsight-cores.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de [ondersteunings abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
