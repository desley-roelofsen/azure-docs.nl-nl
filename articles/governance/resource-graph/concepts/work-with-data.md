---
title: Werken met grote gegevenssets
description: Meer informatie over het ophalen, opmaken, pagina's en overs laan van records in grote gegevens sets tijdens het werken met Azure resource Graph.
ms.date: 10/18/2019
ms.topic: conceptual
ms.openlocfilehash: 2c6aca0c468630cee79222bc77bdc20dc9d95b19
ms.sourcegitcommit: 8a2949267c913b0e332ff8675bcdfc049029b64b
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/21/2019
ms.locfileid: "74304017"
---
# <a name="working-with-large-azure-resource-data-sets"></a>Werken met grote Azure-resource gegevens sets

Azure resource Graph is ontworpen voor het werken met en het verkrijgen van informatie over resources in uw Azure-omgeving. Met resource grafiek worden deze gegevens snel opgehaald, zelfs wanneer er een query wordt uitgevoerd op duizenden records. Resource grafiek biedt verschillende opties voor het werken met deze grote gegevens sets.

Zie [richt lijnen voor beperkte aanvragen](./guidance-for-throttled-requests.md)voor meer informatie over het werken met query's met een hoge frequentie.

## <a name="data-set-result-size"></a>Grootte van resultaat van gegevensset

Resource grafiek beperkt standaard elke query om alleen **100** records te retour neren. Met dit besturings element worden de gebruiker en de service beschermd tegen onbedoelde query's die zouden leiden tot grote gegevens sets. Deze gebeurtenis treedt meestal op als een klant experimenteert met query's om resources te zoeken en te filteren op de manier die aan hun specifieke behoeften voldoet. Dit besturings element verschilt van het gebruik van de [Top](/azure/kusto/query/topoperator) of het [beperken](/azure/kusto/query/limitoperator) van Azure Data Explorer taal operators om de resultaten te beperken.

> [!NOTE]
> Wanneer u de **eerste keer**gebruikt, is het raadzaam om de resultaten te rangschikken op ten minste één kolom met `asc` of `desc`. Zonder te sorteren, worden de geretourneerde resultaten wille keurig en niet herhaald.

De standaard limiet kan worden overschreven door alle methoden van interactie met de resource grafiek. In de volgende voor beelden ziet u hoe u de maximale grootte van de gegevensset wijzigt in _200_:

```azurecli-interactive
az graph query -q "Resources | project name | order by name asc" --first 200 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name | order by name asc" -First 200
```

In de [rest API](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources)is het besturings element **$Top** en maakt het deel uit van **QueryRequestOptions**.

Het besturings element dat het _meest beperkend_ is, wint. Als uw query bijvoorbeeld gebruikmaakt van de Opera tors **Top** of **Limit** en vervolgens meer records zou opleveren dan **eerst**, zijn de geretourneerde maximum records dus gelijk aan het **eerst**. Als **Top** of **Limit** kleiner is dan **eerst**, zou de opgehaalde recordset de kleinere waarde zijn die wordt geconfigureerd met **boven** of **limiet**.

De **eerste** heeft momenteel een Maxi maal toegestane waarde van _5000_.

## <a name="skipping-records"></a>Records overs Laan

De volgende optie voor het werken met grote gegevens sets is het besturings element **overs Laan** . Met dit besturings element kan uw query overstappen of het gedefinieerde aantal records overs Laan voordat de resultaten worden geretourneerd. **Overs Laan** is handig voor query's waarbij het sorteren resulteert in een zinvolle manier, waarbij de bedoeling is om records ergens in het midden van de resultatenset op te halen. Als de resultaten aan het einde van de geretourneerde gegevensset worden weer gegeven, is het efficiënter om een andere Sorteer configuratie te gebruiken en de resultaten op te halen uit de bovenkant van de gegevensset in plaats daarvan.

> [!NOTE]
> Wanneer u **overs Laan**gebruikt, is het raadzaam om de resultaten te rangschikken op ten minste één kolom met `asc` of `desc`. Zonder te sorteren, worden de geretourneerde resultaten wille keurig en niet herhaald.

In de volgende voor beelden ziet u hoe u de eerste _tien_ records kunt overs Laan waarmee een query zou leiden, in plaats daarvan de geretourneerde resultatenset met de elfde record te starten:

```azurecli-interactive
az graph query -q "Resources | project name | order by name asc" --skip 10 --output table
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project name | order by name asc" -Skip 10
```

In de [rest API](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources)is het besturings element **$Skip** en maakt het deel uit van **QueryRequestOptions**.

## <a name="paging-results"></a>Resultaten pagineren

Wanneer een resultaatset moet worden opgesplitst in kleinere sets records voor verwerking of omdat een resultaatset de Maxi maal toegestane waarde van _1000_ geretourneerde records zou overschrijden, gebruikt u paginering. De [rest API](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources) **QueryResponse** biedt waarden om aan te geven dat een resultatenset is opgesplitst: **resultTruncated** en **$skipToken**.
**resultTruncated** is een Booleaanse waarde die de Consumer informeert als er geen aanvullende records worden geretourneerd in het antwoord. Deze voor waarde kan ook worden geïdentificeerd wanneer de eigenschap **Count** kleiner is dan de eigenschap **totalRecords** . **totalRecords** definieert het aantal records dat overeenkomt met de query.

Als **resultTruncated** is ingesteld op **True**, wordt de eigenschap **$skipToken** in de reactie. Deze waarde wordt gebruikt met dezelfde query-en abonnements waarden om de volgende set records op te halen die overeenkomen met de query.

In de volgende voor beelden ziet u hoe u de eerste 3000 records **overs laat** en de **eerste** 1000 records retourneert nadat deze records zijn overgeslagen met Azure CLI en Azure PowerShell:

```azurecli-interactive
az graph query -q "Resources | project id, name | order by id asc" --first 1000 --skip 3000
```

```azurepowershell-interactive
Search-AzGraph -Query "Resources | project id, name | order by id asc" -First 1000 -Skip 3000
```

> [!IMPORTANT]
> De query moet het veld **id** **projecteren** om de paginering te kunnen uitvoeren. Als deze ontbreekt in de query, bevat het antwoord niet de **$skipToken**.

Zie de [query volgende pagina](/rest/api/azureresourcegraph/resourcegraph(2018-09-01-preview)/resources/resources#next-page-query) in de documenten van de rest API voor een voor beeld.

## <a name="formatting-results"></a>Resultaten van de opmaak

De resultaten van een resource grafiek query worden in twee indelingen, _tabel_ en _ObjectArray_gegeven. De indeling wordt geconfigureerd met de para meter **resultFormat** als onderdeel van de aanvraag opties. De _tabel_ indeling is de standaard waarde voor **resultFormat**.

De resultaten van Azure CLI worden standaard in JSON opgenomen. Resultaten in Azure PowerShell zijn standaard een **PSCustomObject** , maar ze kunnen snel worden GECONVERTEERD naar JSON met behulp van de `ConvertTo-Json`-cmdlet. Voor andere Sdk's kunnen de query resultaten worden geconfigureerd voor het uitvoeren van de _ObjectArray_ -indeling.

### <a name="format---table"></a>Indeling-tabel

De standaard indeling, _tabel_, retourneert resultaten in een JSON-indeling die is ontworpen voor het markeren van het kolom ontwerp en de rijwaarden van de eigenschappen die door de query worden geretourneerd. Deze indeling lijkt veel op de gegevens zoals gedefinieerd in een gestructureerde tabel of werk blad met de kolommen die het eerst worden geïdentificeerd en vervolgens elke rij die gegevens aanduidt die zijn afgestemd op die kolommen.

Hier volgt een voor beeld van een query resultaat met de _tabel_ opmaak:

```json
{
    "totalRecords": 47,
    "count": 1,
    "data": {
        "columns": [{
                "name": "name",
                "type": "string"
            },
            {
                "name": "type",
                "type": "string"
            },
            {
                "name": "location",
                "type": "string"
            },
            {
                "name": "subscriptionId",
                "type": "string"
            }
        ],
        "rows": [
            [
                "veryscaryvm2-nsg",
                "microsoft.network/networksecuritygroups",
                "eastus",
                "11111111-1111-1111-1111-111111111111"
            ]
        ]
    },
    "facets": [],
    "resultTruncated": "true"
}
```

### <a name="format---objectarray"></a>Indeling-ObjectArray

De _ObjectArray_ -indeling retourneert ook resultaten in een JSON-indeling. Dit ontwerp wordt echter uitgelijnd op de algemene relatie sleutel/waarde in JSON waarbij de kolom en de rijgegevens overeenkomen in matrix groepen.

Hier volgt een voor beeld van een query resultaat met de _ObjectArray_ -opmaak:

```json
{
    "totalRecords": 47,
    "count": 1,
    "data": [{
        "name": "veryscaryvm2-nsg",
        "type": "microsoft.network/networksecuritygroups",
        "location": "eastus",
        "subscriptionId": "11111111-1111-1111-1111-111111111111"
    }],
    "facets": [],
    "resultTruncated": "true"
}
```

Hier volgen enkele voor beelden van het instellen van **resultFormat** voor het gebruik van de _ObjectArray_ -indeling:

```csharp
var requestOptions = new QueryRequestOptions( resultFormat: ResultFormat.ObjectArray);
var request = new QueryRequest(subscriptions, "Resources | limit 1", options: requestOptions);
```

```python
request_options = QueryRequestOptions(
    result_format=ResultFormat.object_array
)
request = QueryRequest(query="Resources | limit 1", subscriptions=subs_list, options=request_options)
response = client.resources(request)
```

## <a name="next-steps"></a>Volgende stappen

- Zie de taal die wordt gebruikt in [Start query's](../samples/starter.md).
- Zie Geavanceerd gebruik in [Geavanceerde query's](../samples/advanced.md).
- Meer informatie over hoe u [resources kunt verkennen](explore-resources.md).