---
title: Azure Monitor configureren voor het verzamelen van gegevens van containers en agents | Microsoft Docs
description: In dit artikel wordt beschreven hoe u de Azure Monitor voor containers-agent kunt configureren voor het beheren van de logboek verzameling van stdout/stderr en omgevings variabelen.
ms.topic: conceptual
ms.date: 10/15/2019
ms.openlocfilehash: 0bde696f39af22f864500e0c79b5e03ca66cc7f0
ms.sourcegitcommit: f4f626d6e92174086c530ed9bf3ccbe058639081
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 12/25/2019
ms.locfileid: "75405678"
---
# <a name="configure-agent-data-collection-for-azure-monitor-for-containers"></a>Gegevens verzameling van agents voor Azure Monitor voor containers configureren

Azure Monitor voor containers worden stdout-, stderr-en omgevings variabelen verzameld van container werkbelastingen die zijn geïmplementeerd op beheerde Kubernetes-clusters die worden gehost op de Azure Kubernetes service (AKS) van de container agent. U kunt instellingen voor het verzamelen van agent gegevens configureren door een aangepaste Kubernetes ConfigMaps te maken om deze ervaring te beheren. 

In dit artikel wordt beschreven hoe u ConfigMap maakt en hoe u gegevens verzameling kunt configureren op basis van uw vereisten.

## <a name="configmap-file-settings-overview"></a>Overzicht van ConfigMap-Bestands instellingen

Er wordt een sjabloon ConfigMap-bestand geleverd waarmee u het eenvoudig kunt bewerken met uw aanpassingen zonder dat u het helemaal opnieuw hoeft te maken. Voordat u begint, raadpleegt u de Kubernetes-documentatie over [ConfigMaps](https://kubernetes.io/docs/tasks/configure-pod-container/configure-pod-configmap/) en leert u hoe u ConfigMaps maakt, configureert en implementeert. Hierdoor kunt u stderr en stdout per naam ruimte of in het hele cluster filteren, en omgevings variabelen voor elke container die wordt uitgevoerd op alle verschillende knoop punten in het cluster.

>[!IMPORTANT]
>De minimale agent versie die wordt ondersteund voor het verzamelen van stdout-, stderr-en omgevings variabelen van container werkbelastingen, is ciprod06142019 of hoger. Als u de versie van de agent wilt controleren, selecteert u een knoop punt op het tabblad **knoop** punt en noteert u in het deel venster Eigenschappen de waarde van de **Agent-installatie kopie label** eigenschap. Zie [release opmerkingen](https://github.com/microsoft/Docker-Provider/tree/ci_feature_prod)voor de agent voor meer informatie over de agent versies en wat er in elke release is opgenomen.

### <a name="data-collection-settings"></a>Instellingen voor gegevens verzameling

Hieronder vindt u de instellingen die kunnen worden geconfigureerd om het verzamelen van gegevens te beheren.

|Sleutel |Gegevenstype |Waarde |Beschrijving |
|----|----------|------|------------|
|`schema-version` |Teken reeks (hoofdletter gevoelig) |v1 |Dit is de schema versie die door de agent wordt gebruikt bij het parseren van deze ConfigMap. Momenteel ondersteunde schema versie v1. Het wijzigen van deze waarde wordt niet ondersteund en wordt afgewezen wanneer ConfigMap wordt geëvalueerd.|
|`config-version` |Tekenreeks | | Ondersteunt de mogelijkheid om de versie van dit configuratie bestand in uw bron beheersysteem of opslag plaats bij te houden. Het maximum aantal toegestane tekens is 10 en alle andere tekens worden afgekapt. |
|`[log_collection_settings.stdout] enabled =` |Booleaans | true of false | Hiermee wordt bepaald of de collectie stdout container log is ingeschakeld. Als deze optie is ingesteld op `true` en er geen naam ruimten worden uitgesloten voor de stdout-logboek verzameling (`log_collection_settings.stdout.exclude_namespaces` onderstaande instelling), worden stdout-logboeken verzameld van alle containers op alle knoop punten in het cluster. Als dat niet is opgegeven in ConfigMaps, is de standaard waarde `enabled = true`. |
|`[log_collection_settings.stdout] exclude_namespaces =`|Tekenreeks | Door komma's gescheiden matrix |Matrix van Kubernetes-naam ruimten waarvoor stdout-logboeken worden niet verzameld. Deze instelling is alleen effectief als `log_collection_settings.stdout.enabled` is ingesteld op `true`. Als dat niet is opgegeven in ConfigMap, is de standaard waarde `exclude_namespaces = ["kube-system"]`.|
|`[log_collection_settings.stderr] enabled =` |Booleaans | true of false |Hiermee wordt bepaald of de logboek verzameling voor de stderr-container is ingeschakeld. Als deze optie is ingesteld op `true` en er geen naam ruimten worden uitgesloten voor de stdout-logboek verzameling (`log_collection_settings.stderr.exclude_namespaces` instelling), worden stderr-logboeken verzameld van alle containers op alle knoop punten in het cluster. Als dat niet is opgegeven in ConfigMaps, is de standaard waarde `enabled = true`. |
|`[log_collection_settings.stderr] exclude_namespaces =` |Tekenreeks |Door komma's gescheiden matrix |Matrix van Kubernetes-naam ruimten waarvoor stderr-logboeken niet zullen worden verzameld. Deze instelling is alleen effectief als `log_collection_settings.stdout.enabled` is ingesteld op `true`. Als dat niet is opgegeven in ConfigMap, is de standaard waarde `exclude_namespaces = ["kube-system"]`. |
| `[log_collection_settings.env_var] enabled =` |Booleaans | true of false | Met deze instelling bepaalt u de verzameling van omgevings variabelen op alle Helen/knoop punten in het cluster en wordt de standaard waarde `enabled = true` wanneer deze niet is opgegeven in ConfigMaps. Als verzameling van omgevings variabelen globaal is ingeschakeld, kunt u deze uitschakelen voor een specifieke container door de omgevings variabele in te stellen `AZMON_COLLECT_ENV` op **Onwaar** , met een Dockerfile-instelling of in het [configuratie bestand voor de pod](https://kubernetes.io/docs/tasks/inject-data-application/define-environment-variable-container/) onder de sectie **env:** . Als verzameling van omgevings variabelen globaal is uitgeschakeld, kunt u het verzamelen van een bepaalde container niet inschakelen (dat wil zeggen, de enige onderdrukking die kan worden toegepast op het niveau van de container om de verzameling uit te scha kelen wanneer deze al is ingeschakeld.) |
| `[log_collection_settings.enrich_container_logs] enabled =` |Booleaans | true of false | Met deze instelling wordt de uitname van het container logboek gecontroleerd om de naam en eigenschaps waarden van de afbeelding in te vullen voor elke logboek record die wordt geschreven naar de ContainerLog-tabel voor alle container Logboeken in het cluster. De standaard instelling is `enabled = false` wanneer deze niet is opgegeven in ConfigMap. |

ConfigMaps is een globale lijst en er kan slechts één ConfigMap op de agent worden toegepast. U kunt de verzamelingen niet overConfigMapsen.

## <a name="configure-and-deploy-configmaps"></a>ConfigMaps configureren en implementeren

Voer de volgende stappen uit om uw ConfigMap-configuratie bestand te configureren en te implementeren in uw cluster.

1. [Down load](https://github.com/microsoft/OMS-docker/blob/ci_feature_prod/Kubernetes/container-azm-ms-agentconfig.yaml) het sjabloon bestand ConfigMap yaml en sla het op als container-AZM-MS-agentconfig. yaml.  

2. Bewerk het ConfigMap yaml-bestand met uw aanpassingen om stdout-, stderr-en/of omgevings variabelen te verzamelen.

    - Als u specifieke naam ruimten wilt uitsluiten voor de stdout-logboek verzameling, configureert u de sleutel/waarde met behulp van het volgende voor beeld: `[log_collection_settings.stdout] enabled = true exclude_namespaces = ["my-namespace-1", "my-namespace-2"]`.
    
    - Als u de verzameling van omgevings variabelen voor een specifieke container wilt uitschakelen, stelt u de sleutel/waarde `[log_collection_settings.env_var] enabled = true` om het verzamelen van variabelen globaal in te scha kelen en volgt u de stappen [hier](container-insights-manage-agent.md#how-to-disable-environment-variable-collection-on-a-container) om de configuratie voor de specifieke container te volt ooien.
    
    - Als u het cluster voor het verzamelen van stderr-logboeken wilt uitschakelen, configureert u de sleutel/waarde met behulp van het volgende voor beeld: `[log_collection_settings.stderr] enabled = false`.

3. Maak ConfigMap door de volgende kubectl-opdracht uit te voeren: `kubectl apply -f <configmap_yaml_file.yaml>`.
    
    Voorbeeld: `kubectl apply -f container-azm-ms-agentconfig.yaml`. 
    
    Het kan een paar minuten duren voordat de configuratie wijziging is doorgevoerd en alle omsagent in het cluster opnieuw worden opgestart. Het opnieuw opstarten is een rolling start voor alle omsagent-peulen, niet allemaal tegelijk opnieuw opstarten. Wanneer het opnieuw opstarten is voltooid, wordt er een bericht weer gegeven dat er ongeveer als volgt uitziet: `configmap "container-azm-ms-agentconfig" created`.

## <a name="verify-configuration"></a>Configuratie controleren 

Als u wilt controleren of de configuratie is toegepast, gebruikt u de volgende opdracht om de logboeken te controleren vanuit een agent Pod: `kubectl logs omsagent-fdf58 -n=kube-system`. Als er configuratie fouten zijn van de omsagent Peul, worden de volgende fouten weer gegeven in de uitvoer:

``` 
***************Start Config Processing******************** 
config::unsupported/missing config schema version - 'v21' , using defaults
```

Fouten met betrekking tot het Toep assen van configuratie wijzigingen zijn ook beschikbaar voor controle. De volgende opties zijn beschikbaar voor extra probleem oplossing van configuratie wijzigingen:

- Vanuit een agent pod-logboeken met dezelfde `kubectl logs` opdracht. 

- Vanuit Live-Logboeken. In Live logboeken worden fouten weer gegeven die vergelijkbaar zijn met de volgende:

    ```
    config::error::Exception while parsing config map for log collection/env variable settings: \nparse error on value \"$\" ($end), using defaults, please check config map for errors
    ```

- Vanuit de **KubeMonAgentEvents** -tabel in uw log Analytics-werk ruimte. Gegevens worden elk uur verzonden met *fout* code voor configuratie fouten. Als er geen fouten zijn, heeft de vermelding in de tabel gegevens met *informatie*over de ernst, die geen fouten rapporteert. De eigenschap **Tags** bevat meer informatie over de Pod en de container-id waarop de fout is opgetreden en ook de eerste instantie, het laatste exemplaar en het aantal voor het afgelopen uur.

Fouten zorgen ervoor dat omsagent het bestand niet kan parseren, waardoor het opnieuw wordt gestart en de standaard configuratie wordt gebruikt. Nadat u de fout (en) in ConfigMap hebt gecorrigeerd, slaat u het yaml-bestand op en past u de bijgewerkte ConfigMaps toe door de volgende opdracht uit te voeren: `kubectl apply -f <configmap_yaml_file.yaml`.

## <a name="applying-updated-configmap"></a>Bijgewerkte ConfigMap Toep assen

Als u al een ConfigMap op uw cluster hebt geïmplementeerd en u deze wilt bijwerken met een nieuwere configuratie, kunt u het ConfigMap-bestand dat u eerder hebt gebruikt, bewerken en vervolgens op dezelfde opdracht Toep assen als voorheen `kubectl apply -f <configmap_yaml_file.yaml`.

Het kan een paar minuten duren voordat de configuratie wijziging is doorgevoerd en alle omsagent in het cluster opnieuw worden opgestart. Het opnieuw opstarten is een rolling start voor alle omsagent-peulen, niet allemaal tegelijk opnieuw opstarten. Wanneer het opnieuw opstarten is voltooid, wordt er een bericht weer gegeven dat er ongeveer als volgt uitziet: `configmap "container-azm-ms-agentconfig" updated`.

## <a name="verifying-schema-version"></a>De schema versie wordt gecontroleerd

Ondersteunde configuratie schema versies zijn beschikbaar als pod-aantekening (schema-versies) in de omsagent-pod. U kunt deze weer geven met de volgende kubectl-opdracht: `kubectl describe pod omsagent-fdf58 -n=kube-system`

De uitvoer ziet er ongeveer als volgt uit met het aantekening schema-versies:

```
    Name:           omsagent-fdf58
    Namespace:      kube-system
    Node:           aks-agentpool-95673144-0/10.240.0.4
    Start Time:     Mon, 10 Jun 2019 15:01:03 -0700
    Labels:         controller-revision-hash=589cc7785d
                    dsName=omsagent-ds
                    pod-template-generation=1
    Annotations:    agentVersion=1.10.0.1
                  dockerProviderVersion=5.0.0-0
                    schema-versions=v1 
```

## <a name="next-steps"></a>Volgende stappen

- Azure Monitor voor containers bevat geen vooraf gedefinieerde set met waarschuwingen. Raadpleeg de procedures [voor het maken van prestatie waarschuwingen met Azure monitor voor containers](container-insights-alerts.md) voor meer informatie over het maken van aanbevolen waarschuwingen voor hoog CPU-en geheugen gebruik ter ondersteuning van uw DevOps-of operationele processen en procedures.

- Als bewaking is ingeschakeld voor het verzamelen van het status-en resource gebruik van uw AKS-of hybride cluster en werk belastingen die hierop worden uitgevoerd, leert [u hoe u Azure monitor gebruikt](container-insights-analyze.md) voor containers.

- Bekijk de [voor beelden van logboek query's](container-insights-log-search.md#search-logs-to-analyze-data) om vooraf gedefinieerde query's en voor beelden te bekijken voor het evalueren of aanpassen van waarschuwingen, het visualiseren of analyseren van uw clusters.
