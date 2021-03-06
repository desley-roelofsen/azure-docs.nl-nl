---
title: NSG-stroom logboeken visualiseren-elastische stack
titleSuffix: Azure Network Watcher
description: Beheer van stroom logboeken voor netwerk beveiligings groepen in azure beheren en analyseren met Network Watcher en elastische stack.
services: network-watcher
documentationcenter: na
author: mattreatMSFT
manager: vitinnan
editor: ''
ms.assetid: e9b2dcad-4da4-4d6b-aee2-6d0afade0cb8
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: mareat
ms.openlocfilehash: 53cbfe08d310f7244134e1ae31b18644a83c63d3
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277752"
---
# <a name="visualize-azure-network-watcher-nsg-flow-logs-using-open-source-tools"></a>Azure Network Watcher NSG-stroom logboeken visualiseren met open-source-hulpprogram ma's

Stroom logboeken voor netwerk beveiligings groepen bieden informatie die kan worden gebruikt voor het begrijpen van binnenkomend en uitgaand IP-verkeer op netwerk beveiligings groepen. In deze stroom logboeken worden uitgaande en inkomende stromen weer gegeven per regel, de NIC waarop de stroom van toepassing is, 5 tupel informatie over de stroom (bron/doel-IP, bron/doel poort, Protocol) en als het verkeer is toegestaan of geweigerd.

Deze stroom logboeken kunnen lastig zijn om hand matig te parseren en inzicht te krijgen in. Er zijn echter verschillende open source-hulpprogram ma's waarmee u deze gegevens kunt visualiseren. In dit artikel wordt een oplossing geboden voor het visualiseren van deze logboeken met behulp van de elastische stack, waarmee u uw stroom logboeken snel kunt indexeren en visualiseren op een Kibana-dash board.

> [!Warning]  
> De volgende stappen werken met stroom logboeken versie 1. Zie [Inleiding tot flow-logboek registratie voor netwerk beveiligings groepen](network-watcher-nsg-flow-logging-overview.md)voor meer informatie. De volgende instructies werken niet met versie 2 van de logboek bestanden, zonder aanpassing.

## <a name="scenario"></a>Scenario

In dit artikel gaan we een oplossing instellen waarmee u stroom logboeken van netwerk beveiligings groepen kunt visualiseren met behulp van de elastische stack.  Met een Logstash-invoer-invoeg toepassing worden de stroom logboeken rechtstreeks opgehaald uit de opslag-blob die is geconfigureerd voor de betreffende stroom Logboeken. Vervolgens worden de stroom logboeken met behulp van de elastische stack geïndexeerd en gebruikt om een Kibana-dash board te maken om de informatie te visualiseren.

![scenario][scenario]

## <a name="steps"></a>Stappen

### <a name="enable-network-security-group-flow-logging"></a>Logboek registratie van stroom van netwerk beveiligings groep inschakelen
Voor dit scenario moet u logboek registratie van stroom netwerk beveiligings groep ingeschakeld hebben op ten minste één netwerk beveiligings groep in uw account. Raadpleeg het volgende artikel [Inleiding tot stroom logboeken voor netwerk beveiligings groepen](network-watcher-nsg-flow-logging-overview.md)voor instructies over het inschakelen van Logboeken voor netwerk beveiligings stromen.

### <a name="set-up-the-elastic-stack"></a>De elastische stack instellen
Door NSG-stroom logboeken te verbinden met de elastische stack, kunnen we een Kibana-dash board maken waarmee we in de logboeken inzichten kunnen zoeken, tekenen, analyseren en afleiden.

#### <a name="install-elasticsearch"></a>Elasticsearch installeren

1. Voor de elastische stack van versie 5,0 en hoger is Java 8 vereist. Voer de opdracht uit `java -version` om uw versie te controleren. Als Java niet is geïnstalleerd, raadpleegt u de documentatie op de [Azure-Suppored JDKs](https://aka.ms/azure-jdks).
2. Down load het juiste binaire pakket voor uw systeem:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.2.0.deb
   sudo dpkg -i elasticsearch-5.2.0.deb
   sudo /etc/init.d/elasticsearch start
   ```

   Andere installatie methoden vindt u op [Elasticsearch-installatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/elasticsearch-installation.html) .

3. Controleer of Elasticsearch wordt uitgevoerd met de opdracht:

    ```bash
    curl http://127.0.0.1:9200
    ```

    Er wordt een antwoord weer gegeven dat er ongeveer als volgt uitziet:

    ```json
    {
    "name" : "Angela Del Toro",
    "cluster_name" : "elasticsearch",
    "version" : {
        "number" : "5.2.0",
        "build_hash" : "8ff36d139e16f8720f2947ef62c8167a888992fe",
        "build_timestamp" : "2016-01-27T13:32:39Z",
        "build_snapshot" : false,
        "lucene_version" : "6.1.0"
    },
    "tagline" : "You Know, for Search"
    }
    ```

Raadpleeg [installatie-instructies](https://www.elastic.co/guide/en/elasticsearch/reference/5.2/_installation.html)voor meer instructies voor het installeren van elastisch zoeken.

### <a name="install-logstash"></a>Logstash installeren

1. Voer de volgende opdrachten uit om Logstash te installeren:

    ```bash
    curl -L -O https://artifacts.elastic.co/downloads/logstash/logstash-5.2.0.deb
    sudo dpkg -i logstash-5.2.0.deb
    ```
2. Vervolgens moet u Logstash configureren om de stroom logboeken te openen en te parseren. Maak een logstash. conf-bestand met behulp van:

    ```bash
    sudo touch /etc/logstash/conf.d/logstash.conf
    ```

3. Voeg de volgende inhoud toe aan het bestand:

   ```
   input {
      azureblob
        {
            storage_account_name => "mystorageaccount"
            storage_access_key => "VGhpcyBpcyBhIGZha2Uga2V5Lg=="
            container => "insights-logs-networksecuritygroupflowevent"
            codec => "json"
            # Refer https://docs.microsoft.com/azure/network-watcher/network-watcher-read-nsg-flow-logs
            # Typical numbers could be 21/9 or 12/2 depends on the nsg log file types
            file_head_bytes => 12
            file_tail_bytes => 2
            # Enable / tweak these settings when event is too big for codec to handle.
            # break_json_down_policy => "with_head_tail"
            # break_json_batch_count => 2
        }
      }

      filter {
        split { field => "[records]" }
        split { field => "[records][properties][flows]"}
        split { field => "[records][properties][flows][flows]"}
        split { field => "[records][properties][flows][flows][flowTuples]"}

     mutate{
      split => { "[records][resourceId]" => "/"}
      add_field => {"Subscription" => "%{[records][resourceId][2]}"
                    "ResourceGroup" => "%{[records][resourceId][4]}"
                    "NetworkSecurityGroup" => "%{[records][resourceId][8]}"}
      convert => {"Subscription" => "string"}
      convert => {"ResourceGroup" => "string"}
      convert => {"NetworkSecurityGroup" => "string"}
      split => { "[records][properties][flows][flows][flowTuples]" => ","}
      add_field => {
                  "unixtimestamp" => "%{[records][properties][flows][flows][flowTuples][0]}"
                  "srcIp" => "%{[records][properties][flows][flows][flowTuples][1]}"
                  "destIp" => "%{[records][properties][flows][flows][flowTuples][2]}"
                  "srcPort" => "%{[records][properties][flows][flows][flowTuples][3]}"
                  "destPort" => "%{[records][properties][flows][flows][flowTuples][4]}"
                  "protocol" => "%{[records][properties][flows][flows][flowTuples][5]}"
                  "trafficflow" => "%{[records][properties][flows][flows][flowTuples][6]}"
                  "traffic" => "%{[records][properties][flows][flows][flowTuples][7]}"
                   }
      convert => {"unixtimestamp" => "integer"}
      convert => {"srcPort" => "integer"}
      convert => {"destPort" => "integer"}        
     }

     date{
       match => ["unixtimestamp" , "UNIX"]
     }
    }
   output {
     stdout { codec => rubydebug }
     elasticsearch {
       hosts => "localhost"
       index => "nsg-flow-logs"
     }
   }  
   ```

Raadpleeg de [officiële documentatie](https://www.elastic.co/guide/en/beats/libbeat/5.2/logstash-installation.html)voor meer instructies voor het installeren van Logstash.

### <a name="install-the-logstash-input-plugin-for-azure-blob-storage"></a>De Logstash-invoer-invoeg toepassing voor Azure Blob Storage installeren

Met deze Logstash-invoeg toepassing kunt u rechtstreeks toegang krijgen tot de stroom logboeken van het opgegeven opslag account. Als u deze invoeg toepassing wilt installeren, voert u de volgende opdracht uit in de standaard Logstash-installatie directory (in dit geval/usr/share/logstash/bin):

```bash
logstash-plugin install logstash-input-azureblob
```

Voer de volgende opdracht uit om Logstash te starten:

```bash
sudo /etc/init.d/logstash start
```

Raadpleeg de [documentatie](https://github.com/Azure/azure-diagnostics-tools/tree/master/Logstash/logstash-input-azureblob)voor meer informatie over deze invoeg toepassing.

### <a name="install-kibana"></a>Kibana installeren

1. Voer de volgende opdrachten uit om Kibana te installeren:

   ```bash
   curl -L -O https://artifacts.elastic.co/downloads/kibana/kibana-5.2.0-linux-x86_64.tar.gz
   tar xzvf kibana-5.2.0-linux-x86_64.tar.gz
   ```

2. Gebruik de volgende opdrachten om Kibana uit te voeren:

   ```bash
   cd kibana-5.2.0-linux-x86_64/
   ./bin/kibana
   ```

3. Als u uw Kibana web interface wilt weer geven, gaat u naar `http://localhost:5601`
4. Voor dit scenario is het index patroon dat wordt gebruikt voor de stroom logboeken ' NSG-flow-logs '. U kunt het index patroon wijzigen in de sectie uitvoer van het bestand logstash. conf.
5. Als u het Kibana-dash board extern wilt weer geven, maakt u een inkomende NSG-regel waarmee toegang tot **poort 5601**wordt toegestaan.

### <a name="create-a-kibana-dashboard"></a>Een Kibana-dash board maken

In de volgende afbeelding ziet u een voor beeld van een dash board om trends en Details in uw waarschuwingen weer te geven:

![afbeelding 1][1]

Down load het [Dashboard bestand](https://aka.ms/networkwatchernsgflowlogdashboard), het [visualisatie bestand](https://aka.ms/networkwatchernsgflowlogvisualizations)en het [opgeslagen Zoek bestand](https://aka.ms/networkwatchernsgflowlogsearch).

Ga op het tabblad **beheer** van Kibana naar **opgeslagen objecten** en importeer alle drie de bestanden. Op het tabblad **dash board** kunt u het voorbeeld dashboard openen en laden.

U kunt ook uw eigen visualisaties en dash boards maken die zijn afgestemd op metrische gegevens van uw eigen interesse. Lees meer over het maken van Kibana-visualisaties in de [officiële documentatie](https://www.elastic.co/guide/en/kibana/current/visualize.html)van Kibana.

### <a name="visualize-nsg-flow-logs"></a>NSG-stroom logboeken visualiseren

Het voorbeeld Dashboard bevat verschillende visualisaties van de stroom logboeken:

1. Stromen op beslissing/richting in de time-time-reeks grafieken met het aantal stromen gedurende de periode. U kunt de tijds eenheid en de duur van beide visualisaties bewerken. Stromen per beslissing toont het aandeel van het toestaan of weigeren van beslissingen, terwijl stromen op richting het aandeel van binnenkomend en uitgaand verkeer toont. Met deze visualisaties kunt u de trends van verkeer in de loop van de tijd bekijken en zoeken naar pieken of ongebruikelijke patronen.

   ![afbeelding 2][2]

2. Stromen op basis van het doel/bron poort – cirkel diagrammen met de uitsplitsing van stromen naar hun respectieve poorten. In deze weer gave ziet u de meest gebruikte poorten. Als u op een specifieke poort in het cirkel diagram klikt, wordt de rest van het dash board gefilterd op stromen van die poort.

   ![figure3][3]

3. Aantal stromen en vroegste logboek tijd – metrische gegevens waarmee u het aantal vastgelegde stromen en de datum van het vroegst vastgelegde logboek wordt weer gegeven.

   ![figure4][4]

4. Stromen op NSG en regel: een staaf diagram met de verdeling van stromen binnen elke NSG, evenals de verdeling van regels binnen elke NSG. Hier kunt u zien welke NSG en regels het meeste verkeer hebben gegenereerd.

   ![figure5][5]

5. Top 10 van bron-en doel-IP-adressen: staaf diagrammen met de Top 10 van de bron-en doel-IP-adressen. U kunt deze grafieken aanpassen zodat meer of minder Top Ip's worden weer gegeven. Hier ziet u de meest voorkomende IP-adressen, evenals de beslissing over verkeer (toestaan of weigeren) voor elk IP-adres.

   ![figure6][6]

6. Stroom Tuples: deze tabel bevat de informatie die is opgenomen in elke stroom-tuple, evenals de bijbehorende NGS en regel.

   ![figure7][7]

Met de query balk boven aan het dash board kunt u het dash board filteren op basis van een para meter van de stromen, zoals abonnements-ID, resource groepen, regel of enige andere interessante variabele. Raadpleeg de [officiële documentatie](https://www.elastic.co/guide/en/beats/packetbeat/current/kibana-queries-filters.html) voor meer informatie over de query's en filters van Kibana.

## <a name="conclusion"></a>Conclusie

Door de stroom logboeken van de netwerk beveiligings groep te combi neren met de elastische stack, hebben we een krachtige en aanpas bare manier om ons netwerk verkeer te visualiseren. Deze Dash boards bieden u de mogelijkheid om snel inzicht te krijgen en te delen over uw netwerk verkeer, en om te filteren op eventuele afwijkingen. Met Kibana kunt u deze Dash boards aanpassen en specifieke visualisaties maken om te voldoen aan de vereisten voor beveiliging, controle en naleving.

## <a name="next-steps"></a>Volgende stappen

Meer informatie over het visualiseren van uw NSG-stroom logboeken met Power BI door [visuals NSG flow-logboeken te bezoeken met Power bi](network-watcher-visualize-nsg-flow-logs-power-bi.md)

<!--Image references-->

[scenario]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/scenario.png
[1]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-open-source-tools/figure7.png
