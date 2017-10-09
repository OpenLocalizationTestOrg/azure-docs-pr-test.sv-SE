---
title: aaaProcess Azure blob-data med avancerade analyser | Microsoft Docs
description: Bearbeta Data i Azure Blob storage.
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: d8a59078-91d3-4440-b85c-430363c3f4d1
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 5911d4211c4135680555a8cdd99e745499a24215
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="heading"></a>Bearbeta Azure blob-data med avancerade analyser
Det här dokumentet beskriver utforska data och genererar funktioner från data som lagras i Azure Blob storage. 

## <a name="load-hello-data-into-a-pandas-data-frame"></a>Läs in hello data till en Pandas data ram
I ordning tooexplore och ändra en datamängd, den måste hämtas från hello blob tooa lokala källfil som sedan kan läsas in i en Pandas data ram. Här följer hello steg toofollow för den här proceduren:

1. Ladda ned hello data från Azure blob med hello följande Python exempelkod med blob-tjänsten. Ersätt hello variabel i hello koden nedan med dina specifika värden: 
   
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
        print(("It takes %s seconds toodownload "+blobname) % (t2 - t1))
2. Läs hello data till en Pandas data-ram från hello ned filen.
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

Nu är klar tooexplore hello data och generera funktioner på denna dataset.

## <a name="blob-dataexploration"></a>Datagranskning
Här följer några exempel på hur tooexplore data med hjälp av Pandas:

1. Inspektera hello antalet rader och kolumner 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. Inspektera hello första eller sista raderna i hello dataset enligt nedan:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Kontrollera hello datatyp varje kolumn har importerats som använder hello följande exempelkod
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Kontrollera hello grundläggande statistik för hello kolumner i datauppsättning hello enligt följande
   
        dataframe_blobdata.describe()
5. Titta på följande sätt på hello antal poster för varje värde i kolumnen
   
        dataframe_blobdata['<column_name>'].value_counts()
6. Antal saknade värden jämfört med hello faktiska antalet poster i varje kolumn med hello följande exempelkod
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Om du har värden som saknas för en viss kolumn i hello data, kan du släppa dem på följande sätt:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape
   
   Ett annat sätt tooreplace värden som saknas är med hello läge funktionen:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna ({< kolumnnamn >: .mode()[0]}) dataframe_blobdata ['< kolumnnamn >']        
8. Skapa ett histogram ritytans med variabeln antal lagerplatser tooplot hello distribution av en variabel    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Titta på korrelationer mellan variabler med hjälp av en scatterplot eller hello inbyggda korrelationsfunktion
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <a name="blob-featuregen"></a>Funktionen Generation
Vi kan generera funktioner med Python på följande sätt:

### <a name="blob-countfeature"></a>Indikatorvärdet baserat funktionen Generation
Kategoriska funktioner kan skapas på följande sätt:

1. Inspektera hello distribution av hello kategoriska kolumn:
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. Generera indikator värden för varje hello kolumnvärdena
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. Anslut hello indikator kolumn med hello ursprungliga dataramen 
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. Ta bort hello ursprungliga själva variabeln:
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <a name="blob-binningfeature"></a>Diskretisering funktionen Generation
För att generera binned funktioner fortsätta enligt följande:

1. Lägga till en sekvens av kolumner toobin en numerisk kolumn
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. Konvertera diskretisering tooa sekvens med booleska variabler
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. Slutligen tillbaka koppling hello dummy variabler toohello ursprungliga dataramen
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <a name="sql-featuregen"></a>När data skrevs tillbaka tooAzure blob och använda i Azure Machine Learning
När du har arbetat hello data och skapa hello nödvändiga funktioner, kan du överföra hello data (provtagning eller featurized) tooan Azure blob- och använda den i Azure Machine Learning med hjälp av följande hello: Observera att du kan skapa ytterligare funktioner i hello Azure Machine Learning Studio samt. 

1. Skriva hello datafilen ram toolocal
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. Överför hello data tooAzure blob på följande sätt:
   
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
3. Nu hello data kan läsas från hello blob med hello Azure Machine Learning [importera Data] [ import-data] modulen som visas i hello-skärmen nedan:

![läsaren blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

