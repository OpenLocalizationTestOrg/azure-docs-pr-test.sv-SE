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
# <a name="create-features-for-azure-blob-storage-data-using-panda"></a><span data-ttu-id="e6eab-103">Skapa funktioner för Azure Blob Storage-data med Panda</span><span class="sxs-lookup"><span data-stu-id="e6eab-103">Create features for Azure blob storage data using Panda</span></span>
<span data-ttu-id="e6eab-104">Det här dokumentet beskrivs hur toocreate funktioner för data som lagras i Azure blob-behållaren med hjälp av hello [Pandas](http://pandas.pydata.org/) Python-paketet.</span><span class="sxs-lookup"><span data-stu-id="e6eab-104">This document shows how toocreate features for data that is stored in Azure blob container using hello [Pandas](http://pandas.pydata.org/) Python package.</span></span> <span data-ttu-id="e6eab-105">Efter beskriver hur tooload hello data till en Panda data ram, den visar hur toogenerate kategoriska funktioner med hjälp av Python-skript med indikatorn värden och diskretisering funktioner.</span><span class="sxs-lookup"><span data-stu-id="e6eab-105">After outlining how tooload hello data into a Panda data frame, it shows how toogenerate categorical features using Python scripts with indicator values and binning features.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="e6eab-106">Detta **menyn** länkar tootopics som beskriver hur toocreate funktioner för data i olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="e6eab-106">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="e6eab-107">Den här uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="e6eab-107">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e6eab-108">Krav</span><span class="sxs-lookup"><span data-stu-id="e6eab-108">Prerequisites</span></span>
<span data-ttu-id="e6eab-109">Den här artikeln förutsätter att du har skapat ett Azure blob storage-konto och har sparat dina data.</span><span class="sxs-lookup"><span data-stu-id="e6eab-109">This article assumes that you have created an Azure blob storage account and have stored your data there.</span></span> <span data-ttu-id="e6eab-110">Om du behöver instruktioner tooset ett konto, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="e6eab-110">If you need instructions tooset up an account, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>

## <a name="load-hello-data-into-a-pandas-data-frame"></a><span data-ttu-id="e6eab-111">Läs in hello data till en Pandas data ram</span><span class="sxs-lookup"><span data-stu-id="e6eab-111">Load hello data into a Pandas data frame</span></span>
<span data-ttu-id="e6eab-112">Utforska i ordning toodo och ändra en datamängd, den måste hämtas från hello blob tooa lokala källfil som sedan kan läsas in i en Pandas data ram.</span><span class="sxs-lookup"><span data-stu-id="e6eab-112">In order toodo explore and manipulate a dataset, it must be downloaded from hello blob source tooa local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="e6eab-113">Här följer hello steg toofollow för den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="e6eab-113">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="e6eab-114">Ladda ned hello data från Azure blob med hello följande Python exempelkod med blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e6eab-114">Download hello data from Azure blob with hello following sample Python code using blob service.</span></span> <span data-ttu-id="e6eab-115">Ersätt hello variabel i hello koden nedan med dina specifika värden:</span><span class="sxs-lookup"><span data-stu-id="e6eab-115">Replace hello variable in hello code below with your specific values:</span></span>
   
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
2. <span data-ttu-id="e6eab-116">Läs hello data till en Pandas data-ram från hello ned filen.</span><span class="sxs-lookup"><span data-stu-id="e6eab-116">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="e6eab-117">Nu är klar tooexplore hello data och generera funktioner på denna dataset.</span><span class="sxs-lookup"><span data-stu-id="e6eab-117">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="e6eab-118"><a name="blob-featuregen"></a>Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="e6eab-118"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="e6eab-119">hello följande två avsnitt visar hur toogenerate kategoriska funktioner med indikatorvärdena och diskretisering funktioner med hjälp av Python-skript.</span><span class="sxs-lookup"><span data-stu-id="e6eab-119">hello next two sections show how toogenerate categorical features with indicator values and binning features using Python scripts.</span></span>

### <span data-ttu-id="e6eab-120"><a name="blob-countfeature"></a>Indikatorvärdet baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="e6eab-120"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="e6eab-121">Kategoriska funktioner kan skapas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="e6eab-121">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="e6eab-122">Inspektera hello distribution av hello kategoriska kolumn:</span><span class="sxs-lookup"><span data-stu-id="e6eab-122">Inspect hello distribution of hello categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="e6eab-123">Generera indikator värden för varje hello kolumnvärdena</span><span class="sxs-lookup"><span data-stu-id="e6eab-123">Generate indicator values for each of hello column values</span></span>
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="e6eab-124">Anslut hello indikator kolumn med hello ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="e6eab-124">Join hello indicator column with hello original data frame</span></span>
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="e6eab-125">Ta bort hello ursprungliga själva variabeln:</span><span class="sxs-lookup"><span data-stu-id="e6eab-125">Remove hello original variable itself:</span></span>
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="e6eab-126"><a name="blob-binningfeature"></a>Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="e6eab-126"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="e6eab-127">För att generera binned funktioner fortsätta enligt följande:</span><span class="sxs-lookup"><span data-stu-id="e6eab-127">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="e6eab-128">Lägga till en sekvens av kolumner toobin en numerisk kolumn</span><span class="sxs-lookup"><span data-stu-id="e6eab-128">Add a sequence of columns toobin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="e6eab-129">Konvertera diskretisering tooa sekvens med booleska variabler</span><span class="sxs-lookup"><span data-stu-id="e6eab-129">Convert binning tooa sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="e6eab-130">Slutligen tillbaka koppling hello dummy variabler toohello ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="e6eab-130">Finally, Join hello dummy variables back toohello original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)

## <span data-ttu-id="e6eab-131"><a name="sql-featuregen"></a>När data skrevs tillbaka tooAzure blob och använda i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="e6eab-131"><a name="sql-featuregen"></a>Writing data back tooAzure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="e6eab-132">När du har arbetat hello data och skapa hello nödvändiga funktioner, kan du överföra hello data (provtagning eller featurized) tooan Azure blob- och använda den i Azure Machine Learning med hjälp av följande hello: Observera att du kan skapa ytterligare funktioner i hello Azure Machine Learning Studio samt.</span><span class="sxs-lookup"><span data-stu-id="e6eab-132">After you have explored hello data and created hello necessary features, you can upload hello data (sampled or featurized) tooan Azure blob and consume it in Azure Machine Learning using hello following steps: Note that additional features can be created in hello Azure Machine Learning Studio as well.</span></span>

1. <span data-ttu-id="e6eab-133">Skriva hello datafilen ram toolocal</span><span class="sxs-lookup"><span data-stu-id="e6eab-133">Write hello data frame toolocal file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="e6eab-134">Överför hello data tooAzure blob på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="e6eab-134">Upload hello data tooAzure blob as follows:</span></span>
   
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
3. <span data-ttu-id="e6eab-135">Nu hello data kan läsas från hello blob med hello Azure Machine Learning [importera Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modulen som visas i hello-skärmen nedan:</span><span class="sxs-lookup"><span data-stu-id="e6eab-135">Now hello data can be read from hello blob using hello Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module as shown in hello screen below:</span></span>

![läsaren blob](./media/machine-learning-data-science-process-data-blob/reader_blob.png)

