---
title: Verbinding maken met een Node.js Mongoose-toepassing met Azure Cosmos DB
description: Informatie over het gebruik van het Mongoose-Framework voor het opslaan en beheren van gegevens in Azure Cosmos DB.
ms.service: cosmos-db
ms.subservice: cosmosdb-mongo
ms.devlang: nodejs
ms.topic: conceptual
ms.date: 11/18/2019
author: sivethe
ms.author: sivethe
ms.custom: seodec18
ms.openlocfilehash: be3fd42f33fd66fe2bf5a773eafafba5d6982706
ms.sourcegitcommit: dbde4aed5a3188d6b4244ff7220f2f75fce65ada
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/19/2019
ms.locfileid: "74184782"
---
# <a name="connect-a-nodejs-mongoose-application-to-azure-cosmos-db"></a>Verbinding maken met een Node.js Mongoose-toepassing met Azure Cosmos DB

In deze zelf studie wordt gedemonstreerd hoe u het [Mongoose-Framework](https://mongoosejs.com/) kunt gebruiken bij het opslaan van gegevens in Cosmos db. We gebruiken de Azure Cosmos DB-API voor MongoDB voor deze walkthrough. Voor wie niet bekend is met Mongoose: dit is een framework voor objectmodellering voor MongoDB in Node.js en biedt een ongecompliceerde, op schema's gebaseerde oplossing voor het modelleren van uw toepassingsgegevens.

Cosmos DB is de wereld wijd gedistribueerde multi-model database service van micro soft. U kunt snel databases maken van documenten, sleutel/waarde-paren en grafieken en hier query's op uitvoeren. Deze databases genieten allemaal het voordeel van de globale distributie en horizontale schaalmogelijkheden die ten grondslag liggen aan Cosmos DB.

## <a name="prerequisites"></a>Vereisten

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cosmos-db-emulator-docdb-api](../../includes/cosmos-db-emulator-docdb-api.md)]

[Node.js](https://nodejs.org/) versie v0.10.29 of hoger.

## <a name="create-a-cosmos-account"></a>Een Cosmos-account maken

We gaan een Cosmos-account maken. Als u al een account hebt dat u wilt gebruiken, kunt u door gaan om uw node. js-toepassing in te stellen. Als u de Azure Cosmos DB-emulator gebruikt, volgt u de stappen op [Azure Cosmos DB-emulator](local-emulator.md) om de emulator in te stellen en gaat u verder met het instellen van de node. js-toepassing.

[!INCLUDE [cosmos-db-create-dbaccount-mongodb](../../includes/cosmos-db-create-dbaccount-mongodb.md)]

## <a name="set-up-your-nodejs-application"></a>Uw Node.js-toepassing instellen

>[!Note]
> Als u alleen de voorbeeldcode wilt doorlopen in plaats van de toepassing zelf te installeren, kopieert u het [voorbeeld](https://github.com/Azure-Samples/Mongoose_CosmosDB) dat voor deze zelfstudie is gebruikt en bouwt u de Node.js Mongoose-toepassing voor Azure Cosmos DB.

1. Voer de volgende opdracht uit in een knooppuntopdrachtprompt om een Node.js-toepassing in de map van uw keuze te maken.

    ```npm init```

    Beantwoord de vragen en uw project is klaar voor gebruik.

1. Voeg een nieuw bestand toe aan de map en noem het ```index.js```.
1. Installeer de vereiste pakketten met een van de ```npm install```-opties:
   * Mongoose: ```npm install mongoose@5 --save```

     > [!Note]
     > De onderstaande Mongoose voorbeeld van de verbinding is gebaseerd op Mongoose 5 +, die is gewijzigd sinds de eerdere versies.
    
   * Dotenv (als u uw geheimen wilt laden uit een .env-bestand): ```npm install dotenv --save```

     >[!Note]
     > Met de ```--save```-markering wordt de afhankelijkheid toegevoegd aan het package.json-bestand.

1. Importeer de afhankelijkheden in uw index.js-bestand.
    ```JavaScript
   var mongoose = require('mongoose');
   var env = require('dotenv').config();   //Use the .env file to load the variables
    ```

1. Voeg uw Cosmos-DB-verbindingsreeks en de naam van de Cosmos DB toe aan het ```.env```-bestand. Vervang de tijdelijke aanduidingen {Cosmos-account-name} en {dbname} door de naam van uw eigen Cosmos-account en de naam van de data base, zonder de accolades.

    ```JavaScript
   # You can get the following connection details from the Azure portal. You can find the details on the Connection string pane of your Azure Cosmos account.

   COSMODDB_USER = "<Azure Cosmos account's user name>"
   COSMOSDB_PASSWORD = "<Azure Cosmos account passowrd>"
   COSMOSDB_DBNAME = "<Azure Cosmos database name>"
   COSMOSDB_HOST= "<Azure Cosmos Host name>"
   COSMOSDB_PORT=10255
    ```

1. Maak verbinding met Cosmos DB met behulp van het Mongoose-Framework door de volgende code toe te voegen aan het einde van index. js.
    ```JavaScript
   mongoose.connect("mongodb://"+process.env.COSMOSDB_HOST+":"+process.env.COSMOSDB_PORT+"/"+process.env.COSMOSDB_DBNAME+"?ssl=true&replicaSet=globaldb", {
      auth: {
        user: process.env.COSMODDB_USER,
        password: process.env.COSMOSDB_PASSWORD
      }
    })
    .then(() => console.log('Connection to CosmosDB successful'))
    .catch((err) => console.error(err));
    ```
    >[!Note]
    > Hier worden de omgevingsvariabelen geladen met behulp van process.env. {variableName} met het npm-pakket 'dotenv'.

    Zodra u verbonden bent met Azure Cosmos DB, kunt u objectmodellen gaan instellen in Mongoose.

## <a name="caveats-to-using-mongoose-with-cosmos-db"></a>Voor behoud van het gebruik van Mongoose met Cosmos DB

Voor elk model dat u maakt, maakt Mongoose een nieuwe verzameling. Gezien het facturerings model per verzameling van Cosmos DB, is het echter mogelijk niet de meest rendabele manier om te gaan, als u meerdere object modellen hebt die met sparse gevuld zijn.

In deze zelfstudie worden beide modellen besproken. Eerst bespreken we het opslaan van één soort gegevens per verzameling. Dit is het de facto gedrag voor Mongoose.

Mongoose heeft ook een concept genaamd [discriminators](https://mongoosejs.com/docs/discriminators.html). Discriminators zijn een mechanisme voor schema-overname. Hiermee kunt u meerdere modellen met overlappende schema's boven op dezelfde onderliggende MongoDB-verzameling hebben.

U kunt de verschillende gegevensmodellen opslaan in dezelfde verzameling en vervolgens een filtercomponent gebruiken op het moment dat de query voor het ophalen van de benodigde gegevens wordt uitgevoerd.

### <a name="one-collection-per-object-model"></a>Eén verzameling per objectmodel

Standaard maakt Mongoose telkens wanneer u een objectmodel maakt een MongoDB-verzameling. In deze sectie wordt beschreven hoe u dit kunt doen met de API van Azure Cosmos DB voor MongoDB. Deze methode wordt aanbevolen als u object modellen hebt met grote hoeveel heden gegevens. Dit is het standaardmodel voor Mongoose, dus mogelijk bent u hiermee bekend als u bekend bent met Mongoose.

1. Open uw ```index.js``` opnieuw.

1. Maak de schemadefinitie voor 'Family'.

    ```JavaScript
    const Family = mongoose.model('Family', new mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
            gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }));
    ```

1. Maak een object voor 'Family'.

    ```JavaScript
    const family = new Family({
        lastName: "Volum",
        parents: [
            { firstName: "Thomas" },
            { firstName: "Mary Kay" }
        ],
        children: [
            { firstName: "Ryan", gender: "male", grade: 8 },
            { firstName: "Patrick", gender: "male", grade: 7 }
        ],
        pets: [
            { givenName: "Blackie" }
        ],
        address: { country: "USA", state: "WA", city: "Seattle" }
    });
    ```

1. Ten slotte gaan we het object opslaan in Cosmos DB. Hiermee maakt u een onderliggende verzameling.

    ```JavaScript
    family.save((err, saveFamily) => {
        console.log(JSON.stringify(saveFamily));
    });
    ```

1. We gaan nu een ander schema en object maken. Deze keer maken we er een voor 'Vacation Destinations' waarin de 'families' mogelijk geïnteresseerd in zijn.
   1. Net als de laatste keer maken we het schema
      ```JavaScript
      const VacationDestinations = mongoose.model('VacationDestinations', new mongoose.Schema({
       name: String,
       country: String
      }));
      ```

   1. Maak een voorbeeldobject (u kunt meerdere objecten toevoegen aan dit schema) en sla het op.
      ```JavaScript
      const vacaySpot = new VacationDestinations({
       name: "Honolulu",
       country: "USA"
      });

      vacaySpot.save((err, saveVacay) => {
       console.log(JSON.stringify(saveVacay));
      });
      ```

1. Nu ziet u in het Azure Portal twee verzamelingen die zijn gemaakt in Cosmos DB.

    ![Node. js-zelf studie: scherm afbeelding van de Azure Portal, met een Azure Cosmos DB account, waarbij meerdere verzamelings namen zijn gemarkeerd-knooppunt database][multiple-coll]

1. Tot slot lezen we de gegevens van Cosmos DB. Aangezien we gebruikmaken van het standaardmodel van Mongoose, zijn de leesbewerkingen hetzelfde als alle andere leesbewerkingen met Mongoose.

    ```JavaScript
    Family.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family: " + JSON.stringify(fam)));
    });
    ```

### <a name="using-mongoose-discriminators-to-store-data-in-a-single-collection"></a>Mongoose-discriminators gebruiken voor het opslaan van gegevens in één enkele verzameling

In deze methode gebruiken we [Mongoose Discriminators](https://mongoosejs.com/docs/discriminators.html) om de kosten van elke verzameling te optimaliseren. Met discriminators kunt u een onderscheidende 'sleutel' definiëren, zodat u kunt opslaan in, onderscheid maken tussen en filteren op verschillende objectmodellen.

Hier maken we een basisobjectmodel, definiëren we een onderscheidende sleutel en voegen we 'Family' en 'VacationDestinations' toe aan het basismodel.

1. We gaan de basisconfiguratie instellen en de discriminatorsleutel definiëren.

    ```JavaScript
    const baseConfig = {
        discriminatorKey: "_type", //If you've got a lot of different data types, you could also consider setting up a secondary index here.
        collection: "alldata"   //Name of the Common Collection
    };
    ```

1. Vervolgens gaan we het algemene objectmodel definiëren

    ```JavaScript
    const commonModel = mongoose.model('Common', new mongoose.Schema({}, baseConfig));
    ```

1. We definiëren nu het model 'Family'. U ziet hier dat we ```commonModel.discriminator``` gebruiken in plaats van ```mongoose.model```. We voegen de basisconfiguratie ook toe aan het mongoose-schema. Hier is de discriminatorKey dus ```FamilyType```.

    ```JavaScript
    const Family_common = commonModel.discriminator('FamilyType', new     mongoose.Schema({
        lastName: String,
        parents: [{
            familyName: String,
            firstName: String,
            gender: String
        }],
        children: [{
            familyName: String,
            firstName: String,
           gender: String,
            grade: Number
        }],
        pets:[{
            givenName: String
        }],
        address: {
            country: String,
            state: String,
            city: String
        }
    }, baseConfig));
    ```

1. Op dezelfde manier gaan we nu een ander schema toevoegen voor de 'VacationDestinations'. Hier is de discriminatorKey ```VacationDestinationsType```.

    ```JavaScript
    const Vacation_common = commonModel.discriminator('VacationDestinationsType', new mongoose.Schema({
        name: String,
        country: String
    }, baseConfig));
    ```

1. Tot slot maken we objecten voor het model en slaan het op.
   1. We gaan objecten toevoegen aan het model 'Family'.
      ```JavaScript
      const family_common = new Family_common({
       lastName: "Volum",
       parents: [
           { firstName: "Thomas" },
           { firstName: "Mary Kay" }
       ],
       children: [
           { firstName: "Ryan", gender: "male", grade: 8 },
           { firstName: "Patrick", gender: "male", grade: 7 }
       ],
       pets: [
           { givenName: "Blackie" }
       ],
       address: { country: "USA", state: "WA", city: "Seattle" }
      });

      family_common.save((err, saveFamily) => {
       console.log("Saved: " + JSON.stringify(saveFamily));
      });
      ```

   1. Vervolgens voegen we objecten toe aan het model 'VacationDestinations' en slaan het op.
      ```JavaScript
      const vacay_common = new Vacation_common({
       name: "Honolulu",
       country: "USA"
      });

      vacay_common.save((err, saveVacay) => {
       console.log("Saved: " + JSON.stringify(saveVacay));
      });
      ```

1. Als u nu terug naar Azure Portal gaat, ziet u dat er slechts één verzameling met de naam ```alldata``` met zowel 'Family'- als 'VacationDestinations'-gegevens heeft.

    ![Node. js-zelf studie: scherm afbeelding van de Azure Portal, met een Azure Cosmos DB account, waarbij de naam van de verzameling is gemarkeerd-knooppunt database][alldata]

1. U ziet ook dat elk object een ander kenmerk heeft dat wordt aangeroepen als ```__type```, waarmee u onderscheid tussen de twee verschillende objectmodellen kunt maken.

1. Tot slot lezen we de gegevens die zijn opgeslagen in Azure Cosmos DB. Mongoose zorgt voor het filteren van gegevens op basis van het model. U hoeft dus niets anders te doen tijdens het lezen van gegevens. Geef alleen uw model op (in dit geval ```Family_common```) en Mongoose zorgt dat wordt gefilterd op 'DiscriminatorKey'.

    ```JavaScript
    Family_common.find({ 'children.gender' : "male"}, function(err, foundFamily){
        foundFamily.forEach(fam => console.log("Found Family (using discriminator): " + JSON.stringify(fam)));
    });
    ```

Zoals u ziet, is het eenvoudig om met Mongoose-discriminators te werken. Als u een app hebt die gebruikmaakt van het Mongoose-Framework, is deze zelf studie een manier om uw toepassing te laten werken met de API van Azure Cosmos voor MongoDB, zonder dat er te veel wijzigingen zijn vereist.

## <a name="clean-up-resources"></a>Resources opschonen

[!INCLUDE [cosmosdb-delete-resource-group](../../includes/cosmos-db-delete-resource-group.md)]

## <a name="next-steps"></a>Volgende stappen

- Meer informatie over het [gebruik van Studio 3T](mongodb-mongochef.md) met de API voor MongoDB van Azure Cosmos DB.
- Meer informatie over het [gebruik van Robo 3T](mongodb-robomongo.md) met de API voor MongoDB van Azure Cosmos DB.
- Verken [voorbeelden](mongodb-samples.md) van MongoDB met de API voor MongoDB van Azure Cosmos DB.

[alldata]: ./media/mongodb-mongoose/mongo-collections-alldata.png
[multiple-coll]: ./media/mongodb-mongoose/mongo-mutliple-collections.png
