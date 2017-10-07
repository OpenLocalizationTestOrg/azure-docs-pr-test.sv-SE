---
title: aaaExplore data i Azure blob storage med Pandas | Microsoft Docs
description: "Hur tooexplore data som lagras i Azure blob-behållaren med Pandas."
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: feaa9e54-01e0-48c8-a917-1eba0f9d9ec7
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: 28f3c0aebf2300006066c4b19dcb1f0a76a1deb2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a>Utforska data i Azure Blob Storage med Pandas
Det här dokumentet beskriver hur tooexplore data som lagras i Azure blob-behållaren med hjälp av [Pandas](http://pandas.pydata.org/) Python-paketet.

hello följande **menyn** länkar tootopics som beskriver hur toouse verktyg tooexplore data från olika miljöer för lagring. Den här uppgiften är ett steg i hello [datavetenskap Process]().

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har:

* Skapa ett Azure storage-konto. Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)
* Lagra data i ett Azure blob storage-konto. Om du behöver mer information, se [flytta data tooand från Azure Storage](../storage/common/storage-moving-data.md)

## <a name="load-hello-data-into-a-pandas-dataframe"></a>Läs in hello data till en Pandas DataFrame
tooexplore och ändra en datamängd, måste först hämtas från hello blob tooa lokala källfilen, som sedan kan läsas in i en Pandas DataFrame. Här följer hello steg toofollow för den här proceduren:

1. Ladda ned hello data från Azure blob med hello följande kodexempel för Python med hjälp av blob-tjänsten. Ersätt hello variabel i hello följande kod med dina specifika värden: 
   
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

## <a name="blob-dataexploration"></a>Exempel på undersökning av data med hjälp av Pandas
Här följer några exempel på hur tooexplore data med hjälp av Pandas:

1. Inspektera hello **antal rader och kolumner** 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. **Inspektera** hello första eller sista några **rader** i hello följande datauppsättningen:
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. Kontrollera hello **datatyp** varje kolumn har importerats som använder hello följande exempelkod
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. Kontrollera hello **grundläggande statistik** för hello kolumner i hello datauppsättning enligt följande
   
        dataframe_blobdata.describe()
5. Titta på följande sätt på hello antal poster för varje värde i kolumnen
   
        dataframe_blobdata['<column_name>'].value_counts()
6. **Antal saknade värden** jämfört med hello faktiska antalet poster i varje kolumn med hello följande exempelkod
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. Om du har **saknade värden** för en viss kolumn i hello data kan du släppa dem på följande sätt:
   
     dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape
   
   Ett annat sätt tooreplace värden som saknas är med hello läge funktionen:
   
     dataframe_blobdata_mode = dataframe_blobdata.fillna ({< kolumnnamn >: .mode()[0]}) dataframe_blobdata ['< kolumnnamn >']        
8. Skapa en **histogram** ritas med variabeln antal lagerplatser tooplot hello distribution av en variabel    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. Titta på **korrelationer** mellan variabler med hjälp av en scatterplot eller hello inbyggda korrelationsfunktion
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

