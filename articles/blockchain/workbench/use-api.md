---
title: REST-Api's van Azure Block Chain Workbench gebruiken
description: Scenario's voor het gebruik van de preview-versie van Azure Block Chain Workbench REST API
ms.date: 10/14/2019
ms.topic: article
ms.reviewer: brendal
ms.openlocfilehash: 188bbb9a9f6d289a7950ff74596352dff36e79f2
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/22/2019
ms.locfileid: "74324191"
---
# <a name="using-the-azure-blockchain-workbench-preview-rest-api"></a>De preview-versie van Azure Block Chain Workbench gebruiken REST API

De preview-versie van Azure Block Chain Workbench REST API biedt ontwikkel aars en informatie werkers een manier om uitgebreide integraties te bouwen met block Chain-toepassingen. In dit document worden verschillende belangrijke methoden van de REST-API van Workbench beschreven. Stel dat een ontwikkelaar een aangepaste blockchain-client wil maken. Met deze blockchain-client kunnen ingelogde gebruikers hun toegewezen blockchain-applicaties bekijken en gebruiken. De client stelt gebruikers in de gelegenheid exemplaren van contracten te bekijken en actie te ondernemen op slimme contracten. De client gebruikt de Workbench REST API in de context van de ingelogde gebruiker om de volgende acties uit te voeren:

* Lijst met toepassingen weergeven
* Lijst met werkstromen voor een toepassing weergeven
* Lijst met exemplaren van slimme contracten voor een werkstroom weergeven
* Lijst met beschikbare acties voor een contract weergeven
* Een actie voor een contract uitvoeren

De voor beelden van Block Chain-toepassingen die in de scenario's worden gebruikt, kunnen worden [gedownload van github](https://github.com/Azure-Samples/blockchain).

## <a name="blockchain-workbench-api-endpoint"></a>Block Chain Workbench API-eind punt

Block Chain Workbench-Api's worden geopend via een eind punt voor uw implementatie. De URL van het API-eind punt ophalen voor uw implementatie:

1. Meld u aan bij [Azure Portal](https://portal.azure.com).
1. Selecteer **resource groepen**in het navigatie deel venster aan de linkerkant.
1. Kies de resource groep met de naam van uw geïmplementeerde Block Chain Workbench.
1. Selecteer de kolomkop **type** om de lijst alfabetisch te sorteren op type.
1. Er zijn twee resources met het type **app service**. Selecteer de resource van het type **app service** *met* het achtervoegsel-API.
1. Kopieer in het **overzicht**van app service de **URL** -waarde, die de URL van het API-eind punt vertegenwoordigt naar uw geïmplementeerde Block Chain Workbench.

    ![URL van app Service API-eind punt](media/use-api/app-service-api.png)

## <a name="list-applications"></a>Lijst met toepassingen weergeven

Zodra een gebruiker zich bij de blockchain-client heeft aangemeld, is de eerste taak om alle Blockchain Workbench-toepassingen voor de gebruiker op te halen. In dit scenario heeft de gebruiker toegang tot twee toepassingen:

1. [Asset overboeking](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/asset-transfer/readme.md)
2. [Koel transport](https://github.com/Azure-Samples/blockchain/blob/master/blockchain-workbench/application-and-smart-contract-samples/refrigerated-transportation/readme.md)

Gebruik de [GET-API voor toepassingen](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/applicationsget):

``` http
GET /api/v1/applications
Authorization : Bearer {access token}
```

In het antwoord wordt een lijst weer gegeven met alle Block Chain-toepassingen waartoe een gebruiker toegang heeft in Block Chain Workbench. Block Chain Workbench-beheerders krijgen elke Block Chain-toepassing. Beheerders van niet-Workbench krijgen alle blockchains waarvoor ze ten minste één gekoppelde toepassingsrol of een gekoppelde rol van een slimme contract instantie hebben.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications?skip=2",
    "applications": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Allows transfer of assets between a buyer and a seller, with appraisal/inspection functionality",
            "displayName": "Asset Transfer",
            "createdByUserId": 1,
            "createdDtTm": "2018-04-28T05:59:14.4733333",
            "enabled": true,
            "applicationRoles": null
        },
        {
            "id": 2,
            "name": "RefrigeratedTransportation",
            "description": "Application to track end-to-end transportation of perishable goods.",
            "displayName": "Refrigerated Transportation",
            "createdByUserId": 7,
            "createdDtTm": "2018-04-28T18:25:38.71",
            "enabled": true,
            "applicationRoles": null
        }
    ]
}
```

## <a name="list-workflows-for-an-application"></a>Lijst met werkstromen voor een toepassing weergeven

Zodra een gebruiker de toepasselijke Block Chain-toepassing (zoals **Asset overboeking**) selecteert, haalt de Block Chain-client alle werk stromen van de specifieke Block Chain-toepassing op. Gebruikers kunnen vervolgens de toepasselijke werkstroom selecteren voordat alle exemplaren van de slimme contracten voor de werkstroom worden weergegeven. Elke blockchain-toepassing heeft een of meer werkstromen en elke werkstroom heeft nul of meer exemplaren van slimme contracten. Voor een blockchain-clienttoepassing die slechts één werkstroom heeft, raden we aan de gebruikerservaringsstroom over te slaan, zodat gebruikers de juiste werkstroom kunnen selecteren. In dit geval heeft **Asset overboeking** slechts één werk stroom, ook wel **Asset-overdracht**genoemd.

Gebruik de [GET-API voor werkstromen van toepassingen](https://docs.microsoft.com/rest/api/azure-blockchain-workbench/applications/workflowsget):

``` http
GET /api/v1/applications/{applicationId}/workflows
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle werkstromen van de opgegeven blockchain-toepassing waartoe een gebruiker toegang heeft in Blockchain Workbench. Blockchain Workbench-beheerders krijgen elke blockchain-werkstroom. Niet-Workbench-beheerders krijgen alle werkstromen waarvoor ze ten minste één bijbehorende toepassingsrol hebben of die zijn gekoppeld aan een instantierol van een slim contract.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/applications/1/workflows?skip=1",
    "workflows": [
        {
            "id": 1,
            "name": "AssetTransfer",
            "description": "Handles the business logic for the asset transfer scenario",
            "displayName": "Asset Transfer",
            "applicationId": 1,
            "constructorId": 1,
            "startStateId": 1
        }
    ]
}
```

## <a name="list-smart-contract-instances-for-a-workflow"></a>Lijst met exemplaren van slimme contracten voor een werkstroom weergeven

Zodra een gebruiker de toepasselijke werk stroom selecteert, wordt de **Block Chain-client**voor de opgegeven werk stroom alle slimme contract instanties opgehaald. U kunt deze informatie gebruiken om alle slimme contractinstanties voor de werkstroom te tonen. Of u kunt gebruikers toestaan om diep in een van de getoonde slimme contractinstanties te duiken. Stel dat een gebruiker in dit voorbeeld interactie wil hebben met een van de exemplaren van een slim contract om er vervolgens een bepaalde actie op uit te voeren.

Gebruik de [GET-API voor contracten](/rest/api/azure-blockchain-workbench/contractsv2/contractsget):

``` http
GET api/v1/contracts?workflowId={workflowId}
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle exemplaren van slimme contracten van de opgegeven werkstroom. Workbench-beheerders krijgen alle slimme contractinstanties. Niet-Workbench-beheerders krijgen alle slimme contractexemplaren waarvoor ze ten minste één bijbehorende toepassingsrol hebben of die zijn gekoppeld aan een slimme contractinstantie-rol.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts?skip=3&workflowId=1",
    "contracts": [
        {
            "id": 1,
            "provisioningStatus": 2,
            "connectionID": 1,
            "ledgerIdentifier": "0xbcb6127be062acd37818af290c0e43479a153a1c",
            "deployedByUserId": 1,
            "workflowId": 1,
            "contractCodeId": 1,
            "contractProperties": [
                {
                    "workflowPropertyId": 1,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 2,
                    "value": "My first car"
                },
                {
                    "workflowPropertyId": 3,
                    "value": "54321"
                },
                {
                    "workflowPropertyId": 4,
                    "value": "0"
                },
                {
                    "workflowPropertyId": 5,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 6,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 7,
                    "value": "0x0000000000000000000000000000000000000000"
                },
                {
                    "workflowPropertyId": 8,
                    "value": "0xd882530eb3d6395e697508287900c7679dbe02d7"
                }
            ],
            "transactions": [
                {
                    "id": 1,
                    "connectionId": 1,
                    "transactionHash": "0xf3abb829884dc396e03ae9e115a770b230fcf41bb03d39457201449e077080f4",
                    "blockID": 241,
                    "from": "0xd882530eb3d6395e697508287900c7679dbe02d7",
                    "to": null,
                    "value": 0,
                    "isAppBuilderTx": true
                }
            ],
            "contractActions": [
                {
                    "id": 1,
                    "userId": 1,
                    "provisioningStatus": 2,
                    "timestamp": "2018-04-29T23:41:14.9333333",
                    "parameters": [
                        {
                            "name": "Description",
                            "value": "My first car"
                        },
                        {
                            "name": "Price",
                            "value": "54321"
                        }
                    ],
                    "workflowFunctionId": 1,
                    "transactionId": 1,
                    "workflowStateId": 1
                }
            ]
        }
    ]
}
```

## <a name="list-available-actions-for-a-contract"></a>Lijst met beschikbare acties voor een contract weergeven

Zodra een gebruiker heeft besloten om een contract diep te vinden, kan de Block Chain-client vervolgens de beschik bare gebruikers acties weer geven op basis van de status van het contract. In dit voorbeeld kijkt de gebruiker naar alle beschikbare acties voor een nieuw slim contract die hij of zij heeft gemaakt:

* Modify: hiermee kan de gebruiker de beschrijving en prijs van een asset aanpassen.
* Beëindigen: hiermee kan de gebruiker het contract van een asset beëindigen.

Gebruik de [GET-API voor contractacties](/rest/api/azure-blockchain-workbench/contractsv2/contractactionget):

``` http
GET /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
```

Het antwoord bestaat uit een lijst met alle acties die een gebruiker kan uitvoeren op basis van de huidige status van het opgegeven exemplaar van het slimme contract. Gebruikers zien alle toepasselijke acties als de gebruiker een bijbehorende toepassingsrol heeft of een rol voor het exemplaar van het slimme contract voor de huidige status van het opgegeven exemplaar van het slimme contract.

``` http
HTTP/1.1 200 OK
Content-type: application/json
{
    "nextLink": "/api/v1/contracts/1/actions?skip=2",
    "workflowFunctions": [
        {
            "id": 2,
            "name": "Modify",
            "description": "Modify the description/price attributes of this asset transfer instance",
            "displayName": "Modify",
            "parameters": [
                {
                    "id": 1,
                    "name": "description",
                    "description": "The new description of the asset",
                    "displayName": "Description",
                    "type": {
                        "id": 2,
                        "name": "string",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                },
                {
                    "id": 2,
                    "name": "price",
                    "description": "The new price of the asset",
                    "displayName": "Price",
                    "type": {
                        "id": 3,
                        "name": "money",
                        "elementType": null,
                        "elementTypeId": 0
                    }
                }
            ],
            "workflowId": 1
        },
        {
            "id": 3,
            "name": "Terminate",
            "description": "Used to cancel this particular instance of asset transfer",
            "displayName": "Terminate",
            "parameters": [],
            "workflowId": 1
        }
    ]
}
```

## <a name="execute-an-action-for-a-contract"></a>Een actie voor een contract uitvoeren

Een gebruiker kan vervolgens besluiten om actie te ondernemen voor het opgegeven exemplaar van een slim contract. Overweeg in dit geval het scenario waarbij een gebruiker de beschrijving en prijs van een asset wil wijzigen in de volgende actie:

* Beschrijving: 'My updated car'
* Prijs: 54321

Gebruik de [POST-API voor contractacties](/rest/api/azure-blockchain-workbench/contractsv2/contractactionpost):

``` http
POST /api/v1/contracts/{contractId}/actions
Authorization: Bearer {access token}
actionInformation: {
    "workflowFunctionId": 2,
    "workflowActionParameters": [
        {
            "name": "description",
            "value": "My updated car"
        },
        {
            "name": "price",
            "value": "54321"
        }
    ]
}
```

Gebruikers kunnen alleen de actie uitvoeren die mogelijk is op basis van de huidige status van het opgegeven exemplaar van het slimme contract en de bijbehorende toepassingsrol van de gebruiker of de rol van het exemplaar van het slimme contract. Als het posten is gelukt, wordt dit aangegeven met een HTTP 200 OK-antwoord zonder verdere toelichting.

``` http
HTTP/1.1 200 OK
Content-type: application/json
```

## <a name="next-steps"></a>Volgende stappen

> [!div class="nextstepaction"]
> [Naslaginformatie over de REST-API van Azure Blockchain Workbench](https://docs.microsoft.com/rest/api/azure-blockchain-workbench)