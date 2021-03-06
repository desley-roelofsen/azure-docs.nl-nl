---
title: Apache Spark streaming-toepassing stopt na 24 dagen in azure HDInsight
description: Een Apache Spark streaming-toepassing stopt 24 dagen nadat deze is uitgevoerd en er zijn geen fouten in de logboek bestanden.
ms.service: hdinsight
ms.topic: troubleshooting
author: hrasheed-msft
ms.author: hrasheed
ms.reviewer: jasonh
ms.date: 07/29/2019
ms.openlocfilehash: 946c54f6c1048c5f22b520333daf0da47740f2e4
ms.sourcegitcommit: c79aa93d87d4db04ecc4e3eb68a75b349448cd17
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/18/2019
ms.locfileid: "71087212"
---
# <a name="scenario-apache-spark-streaming-application-stops-after-executing-for-24-days-in-azure-hdinsight"></a>Scenario: Apache Spark streaming-toepassing stopt na het uitvoeren van 24 dagen in azure HDInsight

In dit artikel worden probleemoplossings stappen en mogelijke oplossingen voor problemen beschreven bij het gebruik van Apache Spark-onderdelen in azure HDInsight-clusters.

## <a name="issue"></a>Probleem

Een Apache Spark streaming-toepassing stopt 24 dagen nadat deze is uitgevoerd en er zijn geen fouten in de logboek bestanden.

## <a name="cause"></a>Oorzaak

De `livy.server.session.timeout` waarde bepaalt hoelang Apache livy moet wachten voordat een sessie is voltooid. Zodra de sessie duur de `session.timeout` waarde bereikt, worden de livy-sessie en de toepassing automatisch afgebroken.

## <a name="resolution"></a>Oplossing

Voor langlopende taken verhoogt u de waarde van `livy.server.session.timeout` de Ambari-gebruikers interface. U kunt toegang krijgen tot de livy-configuratie via de Ambari- `https://<yourclustername>.azurehdinsight.net/#/main/services/LIVY/configs`gebruikers interface met behulp van de URL.

Vervang `<yourclustername>` door de naam van uw HDInsight-cluster zoals wordt weer gegeven in de portal.

## <a name="next-steps"></a>Volgende stappen

Als u het probleem niet ziet of als u het probleem niet kunt oplossen, gaat u naar een van de volgende kanalen voor meer ondersteuning:

* Krijg antwoorden van Azure-experts via de [ondersteuning van Azure Community](https://azure.microsoft.com/support/community/).

* Maak verbinding [@AzureSupport](https://twitter.com/azuresupport) met-het officiële Microsoft Azure account voor het verbeteren van de gebruikers ervaring door de Azure-community te verbinden met de juiste resources: antwoorden, ondersteuning en experts.

* Als u meer hulp nodig hebt, kunt u een ondersteunings aanvraag indienen via de [Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade/). Selecteer **ondersteuning** in de menu balk of open de hub **Help en ondersteuning** . Lees voor meer gedetailleerde informatie [hoe u een ondersteunings aanvraag voor Azure maakt](https://docs.microsoft.com/azure/azure-supportability/how-to-create-azure-support-request). De toegang tot abonnementen voor abonnements beheer en facturering is inbegrepen bij uw Microsoft Azure-abonnement en technische ondersteuning wordt geleverd via een van de [ondersteunings abonnementen voor Azure](https://azure.microsoft.com/support/plans/).
