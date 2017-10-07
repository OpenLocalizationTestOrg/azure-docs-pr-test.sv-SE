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
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a><span data-ttu-id="9c715-103">Utforska data i Azure Blob Storage med Pandas</span><span class="sxs-lookup"><span data-stu-id="9c715-103">Explore data in Azure blob storage with Pandas</span></span>
<span data-ttu-id="9c715-104">Det här dokumentet beskriver hur tooexplore data som lagras i Azure blob-behållaren med hjälp av [Pandas](http://pandas.pydata.org/) Python-paketet.</span><span class="sxs-lookup"><span data-stu-id="9c715-104">This document covers how tooexplore data that is stored in Azure blob container using [Pandas](http://pandas.pydata.org/) Python package.</span></span>

<span data-ttu-id="9c715-105">hello följande **menyn** länkar tootopics som beskriver hur toouse verktyg tooexplore data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="9c715-105">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span> <span data-ttu-id="9c715-106">Den här uppgiften är ett steg i hello [datavetenskap Process]().</span><span class="sxs-lookup"><span data-stu-id="9c715-106">This task is a step in hello [Data Science Process]().</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="9c715-107">Krav</span><span class="sxs-lookup"><span data-stu-id="9c715-107">Prerequisites</span></span>
<span data-ttu-id="9c715-108">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="9c715-108">This article assumes that you have:</span></span>

* <span data-ttu-id="9c715-109">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9c715-109">Created an Azure storage account.</span></span> <span data-ttu-id="9c715-110">Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="9c715-110">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="9c715-111">Lagra data i ett Azure blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="9c715-111">Stored your data in an Azure blob storage account.</span></span> <span data-ttu-id="9c715-112">Om du behöver mer information, se [flytta data tooand från Azure Storage](../storage/common/storage-moving-data.md)</span><span class="sxs-lookup"><span data-stu-id="9c715-112">If you need instructions, see [Moving data tooand from Azure Storage](../storage/common/storage-moving-data.md)</span></span>

## <a name="load-hello-data-into-a-pandas-dataframe"></a><span data-ttu-id="9c715-113">Läs in hello data till en Pandas DataFrame</span><span class="sxs-lookup"><span data-stu-id="9c715-113">Load hello data into a Pandas DataFrame</span></span>
<span data-ttu-id="9c715-114">tooexplore och ändra en datamängd, måste först hämtas från hello blob tooa lokala källfilen, som sedan kan läsas in i en Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="9c715-114">tooexplore and manipulate a dataset, it must first be downloaded from hello blob source tooa local file, which can then be loaded in a Pandas DataFrame.</span></span> <span data-ttu-id="9c715-115">Här följer hello steg toofollow för den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="9c715-115">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="9c715-116">Ladda ned hello data från Azure blob med hello följande kodexempel för Python med hjälp av blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9c715-116">Download hello data from Azure blob with hello following Python code sample using blob service.</span></span> <span data-ttu-id="9c715-117">Ersätt hello variabel i hello följande kod med dina specifika värden:</span><span class="sxs-lookup"><span data-stu-id="9c715-117">Replace hello variable in hello following code with your specific values:</span></span> 
   
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
2. <span data-ttu-id="9c715-118">Läs hello data till en Pandas data-ram från hello ned filen.</span><span class="sxs-lookup"><span data-stu-id="9c715-118">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="9c715-119">Nu är klar tooexplore hello data och generera funktioner på denna dataset.</span><span class="sxs-lookup"><span data-stu-id="9c715-119">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="9c715-120"><a name="blob-dataexploration"></a>Exempel på undersökning av data med hjälp av Pandas</span><span class="sxs-lookup"><span data-stu-id="9c715-120"><a name="blob-dataexploration"></a>Examples of data exploration using Pandas</span></span>
<span data-ttu-id="9c715-121">Här följer några exempel på hur tooexplore data med hjälp av Pandas:</span><span class="sxs-lookup"><span data-stu-id="9c715-121">Here are a few examples of ways tooexplore data using Pandas:</span></span>

1. <span data-ttu-id="9c715-122">Inspektera hello **antal rader och kolumner**</span><span class="sxs-lookup"><span data-stu-id="9c715-122">Inspect hello **number of rows and columns**</span></span> 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="9c715-123">**Inspektera** hello första eller sista några **rader** i hello följande datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="9c715-123">**Inspect** hello first or last few **rows** in hello following dataset:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="9c715-124">Kontrollera hello **datatyp** varje kolumn har importerats som använder hello följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="9c715-124">Check hello **data type** each column was imported as using hello following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="9c715-125">Kontrollera hello **grundläggande statistik** för hello kolumner i hello datauppsättning enligt följande</span><span class="sxs-lookup"><span data-stu-id="9c715-125">Check hello **basic stats** for hello columns in hello data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="9c715-126">Titta på följande sätt på hello antal poster för varje värde i kolumnen</span><span class="sxs-lookup"><span data-stu-id="9c715-126">Look at hello number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="9c715-127">**Antal saknade värden** jämfört med hello faktiska antalet poster i varje kolumn med hello följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="9c715-127">**Count missing values** versus hello actual number of entries in each column using hello following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="9c715-128">Om du har **saknade värden** för en viss kolumn i hello data kan du släppa dem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="9c715-128">If you have **missing values** for a specific column in hello data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="9c715-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="9c715-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="9c715-130">Ett annat sätt tooreplace värden som saknas är med hello läge funktionen:</span><span class="sxs-lookup"><span data-stu-id="9c715-130">Another way tooreplace missing values is with hello mode function:</span></span>
   
     <span data-ttu-id="9c715-131">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< kolumnnamn >: .mode()[0]}) dataframe_blobdata ['< kolumnnamn >']</span><span class="sxs-lookup"><span data-stu-id="9c715-131">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="9c715-132">Skapa en **histogram** ritas med variabeln antal lagerplatser tooplot hello distribution av en variabel</span><span class="sxs-lookup"><span data-stu-id="9c715-132">Create a **histogram** plot using variable number of bins tooplot hello distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="9c715-133">Titta på **korrelationer** mellan variabler med hjälp av en scatterplot eller hello inbyggda korrelationsfunktion</span><span class="sxs-lookup"><span data-stu-id="9c715-133">Look at **correlations** between variables using a scatterplot or using hello built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

