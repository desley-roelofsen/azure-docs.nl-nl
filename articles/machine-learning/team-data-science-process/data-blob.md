---
title: Azure Blob-gegevens verwerken met geavanceerde analyses-team data Science process
description: Gegevens verkennen en functies genereren op basis van gegevens die zijn opgeslagen in Azure Blob-opslag met behulp van geavanceerde analyses.
services: machine-learning
author: marktab
manager: cgronlun
editor: cgronlun
ms.service: machine-learning
ms.subservice: team-data-science-process
ms.topic: article
ms.date: 11/13/2017
ms.author: tdsp
ms.custom: seodec18, previous-author=deguhath, previous-ms.author=deguhath
ms.openlocfilehash: d056226ce8ade93e63d7bca49b975a6983dc126a
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: MT
ms.contentlocale: nl-NL
ms.lasthandoff: 11/04/2019
ms.locfileid: "73492417"
---
# <a name="heading"></a>Azure Blob-gegevens verwerken met geavanceerde analyses
In dit document vindt u informatie over het verkennen van gegevens en het genereren van functies van gegevens die zijn opgeslagen in Azure Blob Storage. 

## <a name="load-the-data-into-a-pandas-data-frame"></a>De gegevens laden in een Pandas-gegevens frame
Als u een gegevensset wilt verkennen en bewerken, moet deze worden gedownload van de BLOB-bron naar een lokaal bestand dat vervolgens kan worden geladen in een object met een Panda-gegevens frame. Dit zijn de stappen die u moet volgen voor deze procedure:

1. Down load de gegevens van de Azure-Blob met de volgende python-voorbeeld code met behulp van de BLOB-service. Vervang de variabele in de onderstaande code door uw specifieke waarden: 
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        LOCALFILENAME= <local_file_name>        
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        #download from blob
        t1=time.time()
        blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
        blob_service.get_blob_to_path(CONTAINERNAME,BLOBNAME,LOCALFILENAME)
        t2=time.time()
        print(("It takes %s seconds to download "+blobname) % (t2 - t1))
2. Lees de gegevens in een Panda-gegevens frame van het gedownloade bestand.
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

U bent nu klaar om de gegevens te verkennen en functies in deze gegevensset te genereren.

## <a name="blob-dataexploration"></a>Gegevens verkennen
Hier volgen enkele voor beelden van manieren om gegevens te verkennen met behulp van Panda:

1. Het aantal rijen en kolommen controleren 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Inspecteer de eerste of laatste paar rijen in de gegevensset, zoals hieronder wordt beschreven:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Controleer het gegevens type dat elke kolom heeft geïmporteerd, zoals met behulp van de volgende voorbeeld code
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Controleer als volgt de basis statistieken voor de kolommen in de gegevensset
   
        dataframe_blobdata.describe()
5. Ga als volgt te kijken naar het aantal items voor elke kolom waarde
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Aantal ontbrekende waarden ten opzichte van het werkelijke aantal vermeldingen in elke kolom met de volgende voorbeeld code
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Als u de waarden voor een bepaalde kolom in de gegevens ontbreken, kunt u deze als volgt verwijderen:
   
        dataframe_blobdata_noNA = dataframe_blobdata.dropna()
        dataframe_blobdata_noNA.shape
   
   Een andere manier om ontbrekende waarden te vervangen, is met de functie mode:
   
        dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})        
8. Een histogram maken met behulp van een variabel aantal opslag locaties om de distributie van een variabele uit te zetten    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Bekijk correlaties tussen variabelen met behulp van een scatterplot of gebruik de ingebouwde correlatie functie
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Onderdelen genereren
We kunnen als volgt functies genereren met behulp van python:

### <a name="blob-countfeature"></a>Genereren van functie op basis van indicator waarde
Categorische-functies kunnen als volgt worden gemaakt:

1. Inspecteer de verdeling van de kolom categorische:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Indicator waarden genereren voor elk van de kolom waarden
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. De indicator kolom koppelen aan het oorspronkelijke gegevens frame 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. De oorspronkelijke variabele zelf verwijderen:
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>Binning-functie generatie
Voor het genereren van binning-functies worden de volgende stappen uitgevoerd:

1. Een reeks kolommen toevoegen aan de bin een numerieke kolom
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Binning converteren naar een reeks Booleaanse variabelen
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Voeg tot slot de dummy variabelen weer aan het oorspronkelijke gegevens frame toe
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="sql-featuregen"></a>Gegevens schrijven naar een Azure-Blob en gebruiken in Azure Machine Learning
Nadat u de gegevens hebt bekeken en de benodigde functies hebt gemaakt, kunt u de gegevens (sampled of featurized) uploaden naar een Azure-Blob en deze gebruiken in Azure Machine Learning met behulp van de volgende stappen: Houd er rekening mee dat er aanvullende functies kunnen worden gemaakt op de Azure-machine Learning Studio (klassiek) ook. 

1. Het gegevens frame naar een lokaal bestand schrijven
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Upload de gegevens naar Azure Blob als volgt:
   
        from azure.storage.blob import BlobService
        import tables
   
        STORAGEACCOUNTNAME= <storage_account_name>
        LOCALFILENAME= <local_file_name>
        STORAGEACCOUNTKEY= <storage_account_key>
        CONTAINERNAME= <container_name>
        BLOBNAME= <blob_name>
   
        output_blob_service=BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)    
        localfileprocessed = os.path.join(os.getcwd(),LOCALFILENAME) #assuming file is in current working directory
   
        try:
   
        #perform upload
        output_blob_service.put_block_blob_from_path(CONTAINERNAME,BLOBNAME,localfileprocessed)
   
        except:            
            print ("Something went wrong with uploading blob:"+BLOBNAME)
3. De gegevens kunnen nu vanuit de BLOB worden gelezen met behulp van de module Azure Machine Learning [gegevens importeren][import-data] , zoals in het onderstaande scherm wordt weer gegeven:

![Lees-BLOB][1]

[1]: ./media/data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

