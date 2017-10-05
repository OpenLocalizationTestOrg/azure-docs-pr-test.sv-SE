---
title: Utforska data i Azure blob storage med Pandas | Microsoft Docs
description: "Så här utforska data som lagras i Azure blob-behållaren med hjälp av Pandas."
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
ms.openlocfilehash: e1b33b17270122a38228484a56c8324c5b4505a0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="explore-data-in-azure-blob-storage-with-pandas"></a><span data-ttu-id="854fe-103">Utforska data i Azure Blob Storage med Pandas</span><span class="sxs-lookup"><span data-stu-id="854fe-103">Explore data in Azure blob storage with Pandas</span></span>
<span data-ttu-id="854fe-104">Det här dokumentet innehåller information om hur att utforska data som lagras i Azure blob-behållaren med [Pandas](http://pandas.pydata.org/) Python-paketet.</span><span class="sxs-lookup"><span data-stu-id="854fe-104">This document covers how to explore data that is stored in Azure blob container using [Pandas](http://pandas.pydata.org/) Python package.</span></span>

<span data-ttu-id="854fe-105">Följande **menyn** länkar till avsnitt som beskriver hur du använder Verktyg för att utforska data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="854fe-105">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span> <span data-ttu-id="854fe-106">Den här uppgiften är ett steg i den [datavetenskap Process]().</span><span class="sxs-lookup"><span data-stu-id="854fe-106">This task is a step in the [Data Science Process]().</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

## <a name="prerequisites"></a><span data-ttu-id="854fe-107">Krav</span><span class="sxs-lookup"><span data-stu-id="854fe-107">Prerequisites</span></span>
<span data-ttu-id="854fe-108">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="854fe-108">This article assumes that you have:</span></span>

* <span data-ttu-id="854fe-109">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="854fe-109">Created an Azure storage account.</span></span> <span data-ttu-id="854fe-110">Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="854fe-110">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="854fe-111">Lagra data i ett Azure blob storage-konto.</span><span class="sxs-lookup"><span data-stu-id="854fe-111">Stored your data in an Azure blob storage account.</span></span> <span data-ttu-id="854fe-112">Om du behöver mer information, se [flytta data till och från Azure Storage](../storage/common/storage-moving-data.md)</span><span class="sxs-lookup"><span data-stu-id="854fe-112">If you need instructions, see [Moving data to and from Azure Storage](../storage/common/storage-moving-data.md)</span></span>

## <a name="load-the-data-into-a-pandas-dataframe"></a><span data-ttu-id="854fe-113">Läsa in data i en Pandas DataFrame</span><span class="sxs-lookup"><span data-stu-id="854fe-113">Load the data into a Pandas DataFrame</span></span>
<span data-ttu-id="854fe-114">Om du vill utforska och ändra en datamängd, måste den först hämtas från blob-källan till en lokal fil som sedan kan läsas in i en Pandas DataFrame.</span><span class="sxs-lookup"><span data-stu-id="854fe-114">To explore and manipulate a dataset, it must first be downloaded from the blob source to a local file, which can then be loaded in a Pandas DataFrame.</span></span> <span data-ttu-id="854fe-115">Här följer de steg för den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="854fe-115">Here are the steps to follow for this procedure:</span></span>

1. <span data-ttu-id="854fe-116">Hämta data från Azure blob med följande Python-kodexempel med blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="854fe-116">Download the data from Azure blob with the following Python code sample using blob service.</span></span> <span data-ttu-id="854fe-117">Ersätt variabeln i följande kod med din specifika värden:</span><span class="sxs-lookup"><span data-stu-id="854fe-117">Replace the variable in the following code with your specific values:</span></span> 
   
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
2. <span data-ttu-id="854fe-118">Läsa data i en Pandas data-ram från den hämta filen.</span><span class="sxs-lookup"><span data-stu-id="854fe-118">Read the data into a Pandas data-frame from the downloaded file.</span></span>
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="854fe-119">Du är nu redo att utforska data och generera funktioner på denna dataset.</span><span class="sxs-lookup"><span data-stu-id="854fe-119">Now you are ready to explore the data and generate features on this dataset.</span></span>

## <span data-ttu-id="854fe-120"><a name="blob-dataexploration"></a>Exempel på undersökning av data med hjälp av Pandas</span><span class="sxs-lookup"><span data-stu-id="854fe-120"><a name="blob-dataexploration"></a>Examples of data exploration using Pandas</span></span>
<span data-ttu-id="854fe-121">Här följer några exempel på hur du kan utforska data med hjälp av Pandas:</span><span class="sxs-lookup"><span data-stu-id="854fe-121">Here are a few examples of ways to explore data using Pandas:</span></span>

1. <span data-ttu-id="854fe-122">Granska de **antal rader och kolumner**</span><span class="sxs-lookup"><span data-stu-id="854fe-122">Inspect the **number of rows and columns**</span></span> 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="854fe-123">**Inspektera** de första eller sista få **rader** i följande datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="854fe-123">**Inspect** the first or last few **rows** in the following dataset:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="854fe-124">Kontrollera den **datatyp** varje kolumn har importerats som använder följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="854fe-124">Check the **data type** each column was imported as using the following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="854fe-125">Kontrollera den **grundläggande statistik** för kolumnerna i följande data</span><span class="sxs-lookup"><span data-stu-id="854fe-125">Check the **basic stats** for the columns in the data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="854fe-126">Titta på hur många poster för varje värde i kolumnen enligt följande</span><span class="sxs-lookup"><span data-stu-id="854fe-126">Look at the number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="854fe-127">**Antal saknade värden** jämfört med det faktiska antalet poster i varje kolumn med hjälp av följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="854fe-127">**Count missing values** versus the actual number of entries in each column using the following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="854fe-128">Om du har **saknade värden** för en viss kolumn i data, kan du släppa dem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="854fe-128">If you have **missing values** for a specific column in the data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="854fe-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="854fe-129">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="854fe-130">Ett annat sätt att ersätta saknade värden är med funktionen läge:</span><span class="sxs-lookup"><span data-stu-id="854fe-130">Another way to replace missing values is with the mode function:</span></span>
   
     <span data-ttu-id="854fe-131">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< kolumnnamn >: .mode()[0]}) dataframe_blobdata ['< kolumnnamn >']</span><span class="sxs-lookup"><span data-stu-id="854fe-131">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="854fe-132">Skapa en **histogram** ritas med hjälp av variabeln antal lagerplatser ska ritas distribution av en variabel</span><span class="sxs-lookup"><span data-stu-id="854fe-132">Create a **histogram** plot using variable number of bins to plot the distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="854fe-133">Titta på **korrelationer** mellan variabler med hjälp av en scatterplot eller inbyggda korrelationen</span><span class="sxs-lookup"><span data-stu-id="854fe-133">Look at **correlations** between variables using a scatterplot or using the built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

