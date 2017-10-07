---
title: "aaaCreate funktioner för Azure blob storage-data med hjälp av Panda | Microsoft Docs"
description: "Hur toocreate funktioner för data som lagras i Azure blob-behållare med hello Panda Python-paketet."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 676b5fb0-4c89-4516-b3a8-e78ae3ca078d
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;garye
ms.openlocfilehash: 8594046c5d76a36ad87fc77e407752489d30afcc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a>Skapa funktioner för Azure Blob Storage-data med Panda
Det här dokumentet beskrivs hur toocreate funktioner för data som lagras i Azure blob-behållaren med hjälp av hello [Pandas](http://pandas.pydata.org/) Python-paketet. Efter beskriver hur tooload hello data till en Panda data ram, den visar hur toogenerate kategoriska funktioner med hjälp av Python-skript med indikatorn värden och diskretisering funktioner.

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

Detta **menyn** länkar tootopics som beskriver hur toocreate funktioner för data i olika miljöer. Den här uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har skapat ett Azure blob storage-konto och har sparat dina data. Om du behöver instruktioner tooset ett konto, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)

## <a name="load-hello-data-into-a-pandas-data-frame"></a>Läs in hello data till en Pandas data ram
Utforska i ordning toodo och ändra en datamängd, den måste hämtas från hello blob tooa lokala källfil som sedan kan läsas in i en Pandas data ram. Här följer hello steg toofollow för den här proceduren:

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

## <a name="blob-featuregen"></a>Funktionen Generation
hello följande två avsnitt visar hur toogenerate kategoriska funktioner med indikatorvärdena och diskretisering funktioner med hjälp av Python-skript.

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
3. Nu hello data kan läsas från hello blob med hello Azure Machine Learning [importera Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulen som visas i hello-skärmen nedan:

![läsaren blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

