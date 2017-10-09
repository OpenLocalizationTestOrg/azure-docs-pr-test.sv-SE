---
title: aaaSample data i Azure blob storage | Microsoft Docs
description: Exempeldata i Azure Blob Storage
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: e8d9ad2c-86c5-43d6-80b8-d355b5c0dccf
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: cceadf1fb1fb4804fc5b5a3da55c82854651026e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="4ef22-103"><a name="heading"></a>Exempeldata i Azure blob storage</span><span class="sxs-lookup"><span data-stu-id="4ef22-103"><a name="heading"></a>Sample data in Azure blob storage</span></span>
<span data-ttu-id="4ef22-104">Det här dokumentet beskriver provtagning data som lagras i Azure blob storage genom att hämta den programmässigt och provtagning den med hjälp av procedurerna som skrivits i Python.</span><span class="sxs-lookup"><span data-stu-id="4ef22-104">This document covers sampling data stored in Azure blob storage by downloading it programmatically and then sampling it using procedures written in Python.</span></span>

<span data-ttu-id="4ef22-105">hello följande **menyn** länkar tootopics som beskriver hur toosample data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="4ef22-105">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="4ef22-106">**Varför exempel dina data?**</span><span class="sxs-lookup"><span data-stu-id="4ef22-106">**Why sample your data?**</span></span>
<span data-ttu-id="4ef22-107">Om hello dataset som du planerar tooanalyze är stor, är det vanligtvis en god idé toodown sampel hello data tooreduce den tooa mindre men representativt och mer användarvänlig storlek.</span><span class="sxs-lookup"><span data-stu-id="4ef22-107">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="4ef22-108">Detta underlättar data förstå undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="4ef22-108">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="4ef22-109">Roll i hello Cortana Analytics processen är tooenable snabb prototyper hello databearbetning funktioner och maskininlärning modeller.</span><span class="sxs-lookup"><span data-stu-id="4ef22-109">Its role in hello Cortana Analytics Process is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="4ef22-110">Sampling uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="4ef22-110">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <a name="download-and-down-sample-data"></a><span data-ttu-id="4ef22-111">Hämta och ned exempeldata</span><span class="sxs-lookup"><span data-stu-id="4ef22-111">Download and down-sample data</span></span>
1. <span data-ttu-id="4ef22-112">Hämta hello data från Azure blob storage med hjälp av hello blob-tjänsten från följande exempelkod för Python hello:</span><span class="sxs-lookup"><span data-stu-id="4ef22-112">Download hello data from Azure blob storage using hello blob service from hello following sample Python code:</span></span> 
   
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

2. <span data-ttu-id="4ef22-113">Läsa data i en Pandas data-ram från hello-fil som hämtas ovan.</span><span class="sxs-lookup"><span data-stu-id="4ef22-113">Read data into a Pandas data-frame from hello file downloaded above.</span></span>
   
        import pandas as pd
   
        #directly ready from file on disk
        dataframe_blobdata = pd.read_csv(LOCALFILE)

3. <span data-ttu-id="4ef22-114">Ned-sample hello data med hjälp av hello `numpy`'s `random.choice` på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4ef22-114">Down-sample hello data using hello `numpy`'s `random.choice` as follows:</span></span>
   
        # A 1 percent sample
        sample_ratio = 0.01 
        sample_size = np.round(dataframe_blobdata.shape[0] * sample_ratio)
        sample_rows = np.random.choice(dataframe_blobdata.index.values, sample_size)
        dataframe_blobdata_sample = dataframe_blobdata.ix[sample_rows]

<span data-ttu-id="4ef22-115">Nu kan du arbeta med hello ovanför data ram med hello 1 procent exempel för vidare undersökning och funktionen generation.</span><span class="sxs-lookup"><span data-stu-id="4ef22-115">Now you can work with hello above data frame with hello 1 Percent sample for further exploration and feature generation.</span></span>

## <span data-ttu-id="4ef22-116"><a name="heading"></a>Överföra data och läsa in den i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4ef22-116"><a name="heading"></a>Upload data and read it into Azure Machine Learning</span></span>
<span data-ttu-id="4ef22-117">Du kan använda följande exempeldata kod toodown sampel hello hello och använda dem direkt i Azure Machine Learning:</span><span class="sxs-lookup"><span data-stu-id="4ef22-117">You can use hello following sample code toodown-sample hello data and use it directly in Azure Machine Learning:</span></span>

1. <span data-ttu-id="4ef22-118">Skriva hello ram tooa lokala datafilen</span><span class="sxs-lookup"><span data-stu-id="4ef22-118">Write hello data frame tooa local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)

2. <span data-ttu-id="4ef22-119">Överför hello lokal fil tooan Azure blob med hello följande exempelkod:</span><span class="sxs-lookup"><span data-stu-id="4ef22-119">Upload hello local file tooan Azure blob using hello following sample code:</span></span>
   
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
            print ("Something went wrong with uploading toohello blob:"+ BLOBNAME)

3. <span data-ttu-id="4ef22-120">Läsa hello data från hello Azure blob med hjälp av Azure Machine Learning [importera Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) enligt hello bilden nedan:</span><span class="sxs-lookup"><span data-stu-id="4ef22-120">Read hello data from hello Azure blob using Azure Machine Learning [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) as shown in hello image below:</span></span>

![läsaren blob](./media/machine-learning-data-science-sample-data-blob/reader_blob.png)

