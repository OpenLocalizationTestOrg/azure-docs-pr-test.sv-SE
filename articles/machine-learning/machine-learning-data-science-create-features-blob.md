---
title: "Skapa funktioner för Azure blob storage-data med hjälp av Panda | Microsoft Docs"
description: "Så här skapar du funktioner för data som lagras i Azure blob-behållaren med Panda Python-paketet."
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
ms.openlocfilehash: 2ef2acfea2372ac7fd52d099a2b4203ee2242d81
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a><span data-ttu-id="bccf9-103">Skapa funktioner för Azure Blob Storage-data med Panda</span><span class="sxs-lookup"><span data-stu-id="bccf9-103">Create features for Azure blob storage data using Panda</span></span>
<span data-ttu-id="bccf9-104">Det här dokumentet beskrivs hur du skapar funktioner för data som lagras i Azure blob-behållaren med den [Pandas](http://pandas.pydata.org/) Python-paketet.</span><span class="sxs-lookup"><span data-stu-id="bccf9-104">This document shows how to create features for data that is stored in Azure blob container using the [Pandas](http://pandas.pydata.org/) Python package.</span></span> <span data-ttu-id="bccf9-105">Efter beskriver hur du läser in data i ett Panda data visas hur du skapar kategoriska funktioner med hjälp av Python-skript med indikatorn värden och diskretisering funktioner.</span><span class="sxs-lookup"><span data-stu-id="bccf9-105">After outlining how to load the data into a Panda data frame, it shows how to generate categorical features using Python scripts with indicator values and binning features.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="bccf9-106">Detta **menyn** länkar till avsnitt som beskriver hur du skapar funktioner för data i olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="bccf9-106">This **menu** links to topics that describe how to create features for data in various environments.</span></span> <span data-ttu-id="bccf9-107">Den här uppgiften är ett steg i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="bccf9-107">This task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bccf9-108">Krav</span><span class="sxs-lookup"><span data-stu-id="bccf9-108">Prerequisites</span></span>
<span data-ttu-id="bccf9-109">Den här artikeln förutsätter att du har skapat ett Azure blob storage-konto och har sparat dina data.</span><span class="sxs-lookup"><span data-stu-id="bccf9-109">This article assumes that you have created an Azure blob storage account and have stored your data there.</span></span> <span data-ttu-id="bccf9-110">Om du behöver anvisningar för att konfigurera ett konto, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="bccf9-110">If you need instructions to set up an account, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>

## <a name="load-the-data-into-a-pandas-data-frame"></a><span data-ttu-id="bccf9-111">Läsa in data i ett Pandas data</span><span class="sxs-lookup"><span data-stu-id="bccf9-111">Load the data into a Pandas data frame</span></span>
<span data-ttu-id="bccf9-112">För att kunna utforska och ändra en datamängd, måste den hämtas från blob-källan till en lokal fil som sedan kan läsas in i en Pandas data ram.</span><span class="sxs-lookup"><span data-stu-id="bccf9-112">In order to do explore and manipulate a dataset, it must be downloaded from the blob source to a local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="bccf9-113">Här följer de steg för den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="bccf9-113">Here are the steps to follow for this procedure:</span></span>

1. <span data-ttu-id="bccf9-114">Hämta data från Azure blob med följande Python exempelkod med blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="bccf9-114">Download the data from Azure blob with the following sample Python code using blob service.</span></span> <span data-ttu-id="bccf9-115">Ersätt variabeln i koden nedan med dina specifika värden:</span><span class="sxs-lookup"><span data-stu-id="bccf9-115">Replace the variable in the code below with your specific values:</span></span>
   
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
2. <span data-ttu-id="bccf9-116">Läsa data i en Pandas data-ram från den hämta filen.</span><span class="sxs-lookup"><span data-stu-id="bccf9-116">Read the data into a Pandas data-frame from the downloaded file.</span></span>
   
        #LOCALFILE is the file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="bccf9-117">Du är nu redo att utforska data och generera funktioner på denna dataset.</span><span class="sxs-lookup"><span data-stu-id="bccf9-117">Now you are ready to explore the data and generate features on this dataset.</span></span>

## <span data-ttu-id="bccf9-118"><a name="blob-featuregen"></a>Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="bccf9-118"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="bccf9-119">I följande två avsnitt visar hur du skapar kategoriska funktioner med indikatorn värden och diskretisering funktioner med hjälp av Python-skript.</span><span class="sxs-lookup"><span data-stu-id="bccf9-119">The next two sections show how to generate categorical features with indicator values and binning features using Python scripts.</span></span>

### <span data-ttu-id="bccf9-120"><a name="blob-countfeature"></a>Indikatorvärdet baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="bccf9-120"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="bccf9-121">Kategoriska funktioner kan skapas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bccf9-121">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="bccf9-122">Kontrollera distributionen av kolumnen kategoriska:</span><span class="sxs-lookup"><span data-stu-id="bccf9-122">Inspect the distribution of the categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="bccf9-123">Generera indikator värden för varje kolumnvärdena</span><span class="sxs-lookup"><span data-stu-id="bccf9-123">Generate indicator values for each of the column values</span></span>
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="bccf9-124">Ansluta till kolumnen indikator med den ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="bccf9-124">Join the indicator column with the original data frame</span></span>
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="bccf9-125">Ta bort själva ursprungliga variabeln:</span><span class="sxs-lookup"><span data-stu-id="bccf9-125">Remove the original variable itself:</span></span>
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="bccf9-126"><a name="blob-binningfeature"></a>Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="bccf9-126"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="bccf9-127">För att generera binned funktioner fortsätta enligt följande:</span><span class="sxs-lookup"><span data-stu-id="bccf9-127">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="bccf9-128">Lägga till en sekvens av kolumner till bin en numerisk kolumn</span><span class="sxs-lookup"><span data-stu-id="bccf9-128">Add a sequence of columns to bin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="bccf9-129">Konvertera diskretisering till en sekvens med booleska variabler</span><span class="sxs-lookup"><span data-stu-id="bccf9-129">Convert binning to a sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="bccf9-130">Slutligen ansluta variablerna dummy tillbaka till den ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="bccf9-130">Finally, Join the dummy variables back to the original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

## <span data-ttu-id="bccf9-131"><a name="sql-featuregen"></a>Data skrivs tillbaka till Azure blob och använda i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="bccf9-131"><a name="sql-featuregen"></a>Writing data back to Azure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="bccf9-132">När du har gjort data och skapa nödvändiga funktioner, kan du överföra data (provtagning eller featurized) till ett Azure blob- och använda den i Azure Machine Learning med följande steg: Observera att du kan skapa ytterligare funktioner i Azure Machine Learning Studio samt.</span><span class="sxs-lookup"><span data-stu-id="bccf9-132">After you have explored the data and created the necessary features, you can upload the data (sampled or featurized) to an Azure blob and consume it in Azure Machine Learning using the following steps: Note that additional features can be created in the Azure Machine Learning Studio as well.</span></span>

1. <span data-ttu-id="bccf9-133">Skriva dataramen till en lokal fil</span><span class="sxs-lookup"><span data-stu-id="bccf9-133">Write the data frame to local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="bccf9-134">Överför data till Azure blob på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="bccf9-134">Upload the data to Azure blob as follows:</span></span>
   
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
3. <span data-ttu-id="bccf9-135">Nu går att läsa data från blob med hjälp av Azure Machine Learning [importera Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulen som visas på skärmen nedan:</span><span class="sxs-lookup"><span data-stu-id="bccf9-135">Now the data can be read from the blob using the Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module as shown in the screen below:</span></span>

![läsaren blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

