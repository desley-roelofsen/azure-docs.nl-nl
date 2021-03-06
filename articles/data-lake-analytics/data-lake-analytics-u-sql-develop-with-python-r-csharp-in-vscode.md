---
title: U-SQL-taken uitvoeren in Python, R en C# -Azure data Lake Analytics
description: Meer informatie over het gebruik van code achter met python, C# R en het verzenden van een taak in azure data Lake.
services: data-lake-analytics
ms.service: data-lake-analytics
author: jejiang
ms.author: jejiang
ms.reviewer: jasonwhowell
ms.topic: conceptual
ms.date: 11/22/2017
ms.openlocfilehash: cb3ddf0c4147fa982e8ab0f9d440292d12803d35
ms.sourcegitcommit: 0486aba120c284157dfebbdaf6e23e038c8a5a15
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 09/26/2019
ms.locfileid: "71309711"
---
# <a name="develop-u-sql-with-python-r-and-c-for-azure-data-lake-analytics-in-visual-studio-code"></a>U-SQL ontwikkelen met python, R en C# voor Azure data Lake Analytics in Visual Studio code
Meer informatie over het gebruik van Visual Studio code (VSCode) voor het schrijven van C# python, R en code achter met U-SQL en het verzenden van taken naar Azure data Lake service. Zie [de Azure data Lake-Hulpprogram ma's voor Visual Studio gebruiken](data-lake-analytics-data-lake-tools-for-vscode.md)voor meer informatie over Azure data Lake-hulpprogram Ma's voor VSCode.

Voordat u code-behind aangepaste code schrijft, moet u een map of een werk ruimte openen in VSCode.


## <a name="prerequisites-for-python-and-r"></a>Vereisten voor python en R
Registreer python-en R-extensies-assembly's voor uw ADL-account. 
1. Open uw account in de portal.
   - Selecteer **Overzicht**. 
   - Klik op **voorbeeld script**.
2. Klik op **meer**.
3. Selecteer **Installeer U-SQL-extensies**. 
4. Er wordt een bevestigings bericht weer gegeven wanneer de U-SQL-uitbrei dingen zijn geïnstalleerd. 

   ![De omgeving instellen voor python en R](./media/data-lake-analytics-data-lake-tools-for-vscode/setup-the-enrionment-for-python-and-r.png)

   > [!Note]
   > Voor de beste ervaring met python-en R-taal service installeert u VSCode python en R-extensie. 

## <a name="develop-python-file"></a>Python-bestand ontwikkelen
1. Klik op het **nieuwe bestand** in uw werk ruimte.
2. Schrijf uw code in U-SQL. Hier volgt een voor beeld van code.
    ```U-SQL
    REFERENCE ASSEMBLY [ExtPython];
    @t  = 
        SELECT * FROM 
        (VALUES
            ("D1","T1","A1","@foo Hello World @bar"),
            ("D2","T2","A2","@baz Hello World @beer")
        ) AS 
            D( date, time, author, tweet );

    @m  =
        REDUCE @t ON date
        PRODUCE date string, mentions string
        USING new Extension.Python.Reducer("pythonSample.usql.py", pyVersion : "3.5.1");

    OUTPUT @m
        TO "/tweetmentions.csv"
        USING Outputters.Csv();
    ```
    
3. Klik met de rechter muisknop op een script bestand en **Selecteer ADL: Python-code achter bestand**genereren. 
4. Het **xxx.usql.py** -bestand wordt gegenereerd in de werkmap. Schrijf uw code in een python-bestand. Hier volgt een voor beeld van code.

    ```Python
    def get_mentions(tweet):
        return ';'.join( ( w[1:] for w in tweet.split() if w[0]=='@' ) )

    def usqlml_main(df):
        del df['time']
        del df['author']
        df['mentions'] = df.tweet.apply(get_mentions)
        del df['tweet']
        return df
    ```
5. Klik met de rechter muisknop in **USQL** -bestand, klik op **script compileren** of **taak verzenden** om taak uit te voeren.

## <a name="develop-r-file"></a>R-bestand ontwikkelen
1. Klik op het **nieuwe bestand** in uw werk ruimte.
2. Schrijf uw code in U-SQL-bestand. Hier volgt een voor beeld van code.
    ```U-SQL
    DEPLOY RESOURCE @"/usqlext/samples/R/my_model_LM_Iris.rda";
    DECLARE @IrisData string = @"/usqlext/samples/R/iris.csv";
    DECLARE @OutputFilePredictions string = @"/my/R/Output/LMPredictionsIris.txt";
    DECLARE @PartitionCount int = 10;

    @InputData =
        EXTRACT SepalLength double,
                SepalWidth double,
                PetalLength double,
                PetalWidth double,
                Species string
        FROM @IrisData
        USING Extractors.Csv();

    @ExtendedData =
        SELECT Extension.R.RandomNumberGenerator.GetRandomNumber(@PartitionCount) AS Par,
            SepalLength,
            SepalWidth,
            PetalLength,
            PetalWidth
        FROM @InputData;

    // Predict Species

    @RScriptOutput =
        REDUCE @ExtendedData
        ON Par
        PRODUCE Par,
                fit double,
                lwr double,
                upr double
        READONLY Par
        USING new Extension.R.Reducer(scriptFile : "RClusterRun.usql.R", rReturnType : "dataframe", stringsAsFactors : false);
    OUTPUT @RScriptOutput
    TO @OutputFilePredictions
    USING Outputters.Tsv();
    ```
3. Klik met de rechter muisknop in **USQL** -bestand en **Selecteer ADL: R-code achter bestand**genereren. 
4. Het bestand **xxx. usql. r** wordt gegenereerd in de werkmap. Schrijf uw code in het R-bestand. Hier volgt een voor beeld van code.

    ```R
    load("my_model_LM_Iris.rda")
    outputToUSQL=data.frame(predict(lm.fit, inputFromUSQL, interval="confidence"))
    ```
5. Klik met de rechter muisknop in **USQL** -bestand, klik op **script compileren** of **taak verzenden** om taak uit te voeren.

## <a name="develop-c-file"></a>Bestand C# ontwikkelen
Een code-behind bestand is een C# bestand dat is gekoppeld aan één U-SQL-script. U kunt een script definiëren dat is toegewezen aan UDO, UDA, UDT en UDF in het code-behind-bestand. De UDO, UDA, UDT en UDF kunnen rechtstreeks in het script worden gebruikt zonder eerst de assembly te registreren. Het code-behind-bestand wordt opgeslagen in dezelfde map als het bijbehorende peering U-SQL-script bestand. Als het script xxx. usql heet, wordt de onderliggende code benoemd als xxx.usql.cs. Als u het code-behind-bestand hand matig verwijdert, is de functie voor code achter uitgeschakeld voor het bijbehorende U-SQL-script. Zie [voor meer informatie over het schrijven van klant code voor u-SQL-script schrijven en het gebruik van aangepaste code in U-SQL: Door de gebruiker gedefinieerde]( https://blogs.msdn.microsoft.com/visualstudio/2015/10/28/writing-and-using-custom-code-in-u-sql-user-defined-functions/)functies.

1. Klik op het **nieuwe bestand** in uw werk ruimte.
2. Schrijf uw code in U-SQL-bestand. Hier volgt een voor beeld van code.
    ```U-SQL
    @a = 
        EXTRACT 
            Iid int,
        Starts DateTime,
        Region string,
        Query string,
        DwellTime int,
        Results string,
        ClickedUrls string 
        FROM @"/Samples/Data/SearchLog.tsv" 
        USING Extractors.Tsv();

    @d =
        SELECT DISTINCT Region 
        FROM @a;

    @d1 = 
        PROCESS @d
        PRODUCE 
            Region string,
        Mkt string
        USING new USQLApplication_codebehind.MyProcessor();

    OUTPUT @d1 
        TO @"/output/SearchLogtest.txt" 
        USING Outputters.Tsv();
    ```
3. Klik met de rechter muisknop in **USQL** -bestand en **Selecteer ADL: CS-code achter bestand**genereren. 
4. Het **xxx.usql.cs** -bestand wordt gegenereerd in de werkmap. Schrijf uw code in het CS-bestand. Hier volgt een voor beeld van code.

    ```CS
    namespace USQLApplication_codebehind
    {
        [SqlUserDefinedProcessor]

        public class MyProcessor : IProcessor
        {
            public override IRow Process(IRow input, IUpdatableRow output)
            {
                output.Set(0, input.Get<string>(0));
                output.Set(1, input.Get<string>(0));
                return output.AsReadOnly();
            } 
        }
    }
    ```
5. Klik met de rechter muisknop in **USQL** -bestand, klik op **script compileren** of **taak verzenden** om taak uit te voeren.

## <a name="next-steps"></a>Volgende stappen
* [De Azure Data Lake-tools gebruiken voor Visual Studio-code](data-lake-analytics-data-lake-tools-for-vscode.md)
* [Lokale U-SQL-uitvoering en lokale fout opsporing met Visual Studio code](data-lake-tools-for-vscode-local-run-and-debug.md)
* [Aan de slag met Data Lake Analytics met behulp van Power shell](data-lake-analytics-get-started-powershell.md)
* [Aan de slag met Data Lake Analytics met behulp van de Azure Portal](data-lake-analytics-get-started-portal.md)
* [Data Lake-Hulpprogram Ma's voor Visual Studio gebruiken voor het ontwikkelen van U-SQL-toepassingen](data-lake-analytics-data-lake-tools-get-started.md)
* [Data Lake Analytics-catalogus (U-SQL) gebruiken](data-lake-analytics-use-u-sql-catalog.md)
