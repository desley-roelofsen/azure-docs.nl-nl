---
title: Resource logboeken voor container groepen
description: Meer informatie over het verzenden van resource logboeken en gebeurtenis gegevens van container groepen in Azure Container Instances naar Azure Monitor-logboeken
ms.topic: article
ms.date: 09/02/2019
ms.author: danlep
ms.openlocfilehash: 02f950917f43b514f83bd7e10078c79634c6c751
ms.sourcegitcommit: 85e7fccf814269c9816b540e4539645ddc153e6e
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/26/2019
ms.locfileid: "74533724"
---
# <a name="container-group-and-instance-logging-with-azure-monitor-logs"></a>Logboek registratie van container groepen en instanties met Azure Monitor-logboeken

Log Analytics-werk ruimten bieden een centrale locatie voor het opslaan en opvragen van logboek gegevens van niet alleen Azure-resources, maar ook on-premises resources en resources in andere Clouds. Azure Container Instances bevat ingebouwde ondersteuning voor het verzenden van Logboeken en gebeurtenis gegevens naar Azure Monitor-Logboeken.

Als u logboek-en gebeurtenis gegevens van container groep wilt verzenden naar Azure Monitor-logboeken, moet u een Log Analytics werk ruimte-ID en werkruimte sleutel opgeven bij het maken van een container groep. In de volgende secties wordt beschreven hoe u een containergroep kunt maken die geschikt is voor logboekregistratie en hoe u query's op logboeken kunt uitvoeren.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../includes/azure-monitor-log-analytics-rebrand.md)]

> [!NOTE]
> Op dit moment kunt u alleen gebeurtenis gegevens van Linux-container instanties naar Log Analytics verzenden.

## <a name="prerequisites"></a>Vereisten

Voor het inschakelen van logboekregistratie voor uw containerexemplaren hebt u het volgende nodig:

* [Log Analytics-werkruimte](../azure-monitor/learn/quick-create-workspace.md)
* [Azure CLI](/cli/azure/install-azure-cli) (of [Cloud Shell](/azure/cloud-shell/overview))

## <a name="get-log-analytics-credentials"></a>Log Analytics-referenties verkrijgen

Azure Container Instances heeft toestemming nodig om gegevens te verzenden naar uw Log Analytics-werkruimte. U kunt deze machtiging verlenen en de mogelijkheid tot logboekregistratie inschakelen als u de Log Analytics-werkruimte-id en een van de sleutels ervan opgeeft (primaire of secundaire) op het moment dat u de containergroep maakt.

De Log Analytics-werkruimte-id en de primaire sleutel verkrijgt u als volgt:

1. Navigeer in Azure Portal naar uw Log Analytics-werkruimte.
1. Selecteer **Geavanceerde instellingen** onder **instellingen**
1. Selecteer **Verbonden bronnen** > **Windows-servers** (of **Linux-servers** - de ID en de sleutels zijn voor beide hetzelfde).
1. Noteer het volgende:
   * **WERKRUIMTE-ID**
   * **PRIMAIRE SLEUTEL**

## <a name="create-container-group"></a>Containergroep maken

Nu dat u de Log Analytics-werkruimte-id en de primaire sleutel hebt, kunt u een containergroep maken die geschikt is voor logboekregistratie.

In de volgende voor beelden ziet u twee manieren om een container groep te maken met één [gefluente][fluentd] container: Azure CLI en Azure CLI met een yaml-sjabloon. In de standaardconfiguratie genereert de Fluentd-container verschillende regels met uitvoer. Omdat deze uitvoer naar de Log Analytics-werkruimte wordt verzonden, is deze heel geschikt is om te illustreren hoe logboeken kunnen worden weergegeven en hoe er query's op kunnen worden uitgevoerd.

### <a name="deploy-with-azure-cli"></a>Implementeren met Azure CLI

Als u wilt implementeren met de Azure CLI, geeft u de para meters `--log-analytics-workspace` en `--log-analytics-workspace-key` op in de opdracht [AZ container Create][az-container-create] . Vervang de twee werkruimtewaarden door de waarden die u hebt verkregen in de vorige stap (en werk de naam van de resourcegroep bij) voordat u de volgende opdracht gaat uitvoeren.

```azurecli-interactive
az container create \
    --resource-group myResourceGroup \
    --name mycontainergroup001 \
    --image fluent/fluentd \
    --log-analytics-workspace <WORKSPACE_ID> \
    --log-analytics-workspace-key <WORKSPACE_KEY>
```

### <a name="deploy-with-yaml"></a>Implementeren met YAML

Gebruik deze methode als u liever containergroepen met YAML wilt implementeren. Met de volgende YAML wordt een containergroep met een enkele container gedefinieerd. Kopieer de YAML naar een nieuw bestand en vervang `LOG_ANALYTICS_WORKSPACE_ID` en `LOG_ANALYTICS_WORKSPACE_KEY` door de waarden die u hebt verkregen in de vorige stap. Sla het bestand op als **deploy-aci.yaml**.

```yaml
apiVersion: 2018-10-01
location: eastus
name: mycontainergroup001
properties:
  containers:
  - name: mycontainer001
    properties:
      environmentVariables: []
      image: fluent/fluentd
      ports: []
      resources:
        requests:
          cpu: 1.0
          memoryInGB: 1.5
  osType: Linux
  restartPolicy: Always
  diagnostics:
    logAnalytics:
      workspaceId: LOG_ANALYTICS_WORKSPACE_ID
      workspaceKey: LOG_ANALYTICS_WORKSPACE_KEY
tags: null
type: Microsoft.ContainerInstance/containerGroups
```

Voer vervolgens de volgende opdracht uit om de container groep te implementeren. Vervang `myResourceGroup` door een resource groep in uw abonnement (of maak eerst een resource groep met de naam ' myResourceGroup '):

```azurecli-interactive
az container create --resource-group myResourceGroup --name mycontainergroup001 --file deploy-aci.yaml
```

Kort nadat u de opdracht hebt opgegeven, zou u een reactie van Azure met implementatiedetails moeten ontvangen.

## <a name="view-logs"></a>Logboeken weergeven

Nadat u de containergroep hebt geïmplementeerd, kan het enkele minuten (wel tien minuten) duren voor de eerste logboekvermeldingen worden weergegeven in Azure Portal. Als u de logboeken van de container groep wilt weer geven in de tabel `ContainerInstanceLog_CL`:

1. Navigeer in Azure Portal naar uw Log Analytics-werkruimte.
1. Onder **Algemeen**selecteert u **Logboeken**  
1. Typ de volgende query: `ContainerInstanceLog_CL | limit 50`
1. Selecteer **uitvoeren**

U ziet een aantal resultaten die door de query worden weer gegeven. Als u eerst geen resultaten ziet, wacht u een paar minuten en selecteert u vervolgens de knop **uitvoeren** om de query opnieuw uit te voeren. Logboek vermeldingen worden standaard weer gegeven in **tabel** indeling. U kunt vervolgens een rij uitbreiden om de inhoud van een afzonderlijke logboekvermelding te zien.

![Resultaten van zoekopdrachten in logboeken in Azure Portal][log-search-01]

## <a name="view-events"></a>Evenementen bekijken

U kunt ook gebeurtenissen weer geven voor container instanties in de Azure Portal. Gebeurtenissen bevatten het tijdstip waarop het exemplaar is gemaakt en wanneer het wordt gestart. De gebeurtenis gegevens in de tabel `ContainerEvent_CL` bekijken:

1. Navigeer in Azure Portal naar uw Log Analytics-werkruimte.
1. Onder **Algemeen**selecteert u **Logboeken**  
1. Typ de volgende query: `ContainerEvent_CL | limit 50`
1. Selecteer **uitvoeren**

U ziet een aantal resultaten die door de query worden weer gegeven. Als u eerst geen resultaten ziet, wacht u een paar minuten en selecteert u vervolgens de knop **uitvoeren** om de query opnieuw uit te voeren. Vermeldingen worden standaard weer gegeven in **tabel** indeling. U kunt vervolgens een rij uitbreiden om de inhoud van een afzonderlijke vermelding weer te geven.

![Zoek resultaten voor gebeurtenissen in de Azure Portal][log-search-02]

## <a name="query-container-logs"></a>Query's uitvoeren op containerlogboeken

Azure Monitor-logboeken bevatten een uitgebreide [query taal][query_lang] voor het ophalen van gegevens uit mogelijk duizenden logboek uitvoer.

De basis structuur van een query is de bron tabel (in dit artikel `ContainerInstanceLog_CL` of `ContainerEvent_CL`), gevolgd door een reeks Opera Tors, gescheiden door het sluis teken (`|`). U kunt verschillende operatoren aan elkaar koppelen om de resultaten te verfijnen en geavanceerde functies uit te voeren.

Als u voorbeeld query resultaten wilt weer geven, plakt u de volgende query in het tekstvak query en selecteert u de knop uitvoeren om de query **uit** te voeren. Deze query geeft alle logboekvermeldingen weer waarvan het veld 'Bericht' het woord 'waarschuwen' bevat:

```query
ContainerInstanceLog_CL
| where Message contains "warn"
```

Complexere query's worden ook ondersteund. Met deze query worden bijvoorbeeld de logboekvermeldingen voor de containergroep 'mycontainergroup001' weergegeven die het afgelopen uur zijn gegenereerd:

```query
ContainerInstanceLog_CL
| where (ContainerGroup_s == "mycontainergroup001")
| where (TimeGenerated > ago(1h))
```

## <a name="next-steps"></a>Volgende stappen

### <a name="azure-monitor-logs"></a>Azure Monitor-logboeken

Voor meer informatie over het uitvoeren van query's op logboeken en het configureren van waarschuwingen in Azure Monitor-logboeken, zie:

* [Zoekopdrachten in Azure Monitor-logboeken begrijpen](../log-analytics/log-analytics-log-search.md)
* [Consistente waarschuwingen in Azure Monitor](../azure-monitor/platform/alerts-overview.md)


### <a name="monitor-container-cpu-and-memory"></a>CPU en geheugen bewaken

Voor informatie over het bewaken van CPU- en geheugenresources van containerinstanties, zie:

* [Containerresources in Azure Container Instances bewaken](container-instances-monitor.md).

<!-- IMAGES -->
[log-search-01]: ./media/container-instances-log-analytics/portal-query-01.png
[log-search-02]: ./media/container-instances-log-analytics/portal-query-02.png

<!-- LINKS - External -->
[fluentd]: https://hub.docker.com/r/fluent/fluentd/
[query_lang]: https://aka.ms/LogAnalyticsLanguage

<!-- LINKS - Internal -->
[az-container-create]: /cli/azure/container#az-container-create