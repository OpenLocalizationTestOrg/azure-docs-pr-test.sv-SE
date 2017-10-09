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
# <span data-ttu-id="4890c-103"><a name="heading"></a>Bearbeta Azure blob-data med avancerade analyser</span><span class="sxs-lookup"><span data-stu-id="4890c-103"><a name="heading"></a>Process Azure blob data with advanced analytics</span></span>
<span data-ttu-id="4890c-104">Det här dokumentet beskriver utforska data och genererar funktioner från data som lagras i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="4890c-104">This document covers exploring data and generating features from data stored in Azure Blob storage.</span></span> 

## <a name="load-hello-data-into-a-pandas-data-frame"></a><span data-ttu-id="4890c-105">Läs in hello data till en Pandas data ram</span><span class="sxs-lookup"><span data-stu-id="4890c-105">Load hello data into a Pandas data frame</span></span>
<span data-ttu-id="4890c-106">I ordning tooexplore och ändra en datamängd, den måste hämtas från hello blob tooa lokala källfil som sedan kan läsas in i en Pandas data ram.</span><span class="sxs-lookup"><span data-stu-id="4890c-106">In order tooexplore and manipulate a dataset, it must be downloaded from hello blob source tooa local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="4890c-107">Här följer hello steg toofollow för den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="4890c-107">Here are hello steps toofollow for this procedure:</span></span>

1. <span data-ttu-id="4890c-108">Ladda ned hello data från Azure blob med hello följande Python exempelkod med blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4890c-108">Download hello data from Azure blob with hello following sample Python code using blob service.</span></span> <span data-ttu-id="4890c-109">Ersätt hello variabel i hello koden nedan med dina specifika värden:</span><span class="sxs-lookup"><span data-stu-id="4890c-109">Replace hello variable in hello code below with your specific values:</span></span> 
   
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
2. <span data-ttu-id="4890c-110">Läs hello data till en Pandas data-ram från hello ned filen.</span><span class="sxs-lookup"><span data-stu-id="4890c-110">Read hello data into a Pandas data-frame from hello downloaded file.</span></span>
   
        #LOCALFILE is hello file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="4890c-111">Nu är klar tooexplore hello data och generera funktioner på denna dataset.</span><span class="sxs-lookup"><span data-stu-id="4890c-111">Now you are ready tooexplore hello data and generate features on this dataset.</span></span>

## <span data-ttu-id="4890c-112"><a name="blob-dataexploration"></a>Datagranskning</span><span class="sxs-lookup"><span data-stu-id="4890c-112"><a name="blob-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="4890c-113">Här följer några exempel på hur tooexplore data med hjälp av Pandas:</span><span class="sxs-lookup"><span data-stu-id="4890c-113">Here are a few examples of ways tooexplore data using Pandas:</span></span>

1. <span data-ttu-id="4890c-114">Inspektera hello antalet rader och kolumner</span><span class="sxs-lookup"><span data-stu-id="4890c-114">Inspect hello number of rows and columns</span></span> 
   
        print 'hello size of hello data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="4890c-115">Inspektera hello första eller sista raderna i hello dataset enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="4890c-115">Inspect hello first or last few rows in hello dataset as below:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="4890c-116">Kontrollera hello datatyp varje kolumn har importerats som använder hello följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="4890c-116">Check hello data type each column was imported as using hello following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="4890c-117">Kontrollera hello grundläggande statistik för hello kolumner i datauppsättning hello enligt följande</span><span class="sxs-lookup"><span data-stu-id="4890c-117">Check hello basic stats for hello columns in hello data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="4890c-118">Titta på följande sätt på hello antal poster för varje värde i kolumnen</span><span class="sxs-lookup"><span data-stu-id="4890c-118">Look at hello number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="4890c-119">Antal saknade värden jämfört med hello faktiska antalet poster i varje kolumn med hello följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="4890c-119">Count missing values versus hello actual number of entries in each column using hello following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="4890c-120">Om du har värden som saknas för en viss kolumn i hello data, kan du släppa dem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4890c-120">If you have missing values for a specific column in hello data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="4890c-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="4890c-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="4890c-122">Ett annat sätt tooreplace värden som saknas är med hello läge funktionen:</span><span class="sxs-lookup"><span data-stu-id="4890c-122">Another way tooreplace missing values is with hello mode function:</span></span>
   
     <span data-ttu-id="4890c-123">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< kolumnnamn >: .mode()[0]}) dataframe_blobdata ['< kolumnnamn >']</span><span class="sxs-lookup"><span data-stu-id="4890c-123">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="4890c-124">Skapa ett histogram ritytans med variabeln antal lagerplatser tooplot hello distribution av en variabel</span><span class="sxs-lookup"><span data-stu-id="4890c-124">Create a histogram plot using variable number of bins tooplot hello distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="4890c-125">Titta på korrelationer mellan variabler med hjälp av en scatterplot eller hello inbyggda korrelationsfunktion</span><span class="sxs-lookup"><span data-stu-id="4890c-125">Look at correlations between variables using a scatterplot or using hello built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <span data-ttu-id="4890c-126"><a name="blob-featuregen"></a>Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="4890c-126"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="4890c-127">Vi kan generera funktioner med Python på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4890c-127">We can generate features using Python as follows:</span></span>

### <span data-ttu-id="4890c-128"><a name="blob-countfeature"></a>Indikatorvärdet baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="4890c-128"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="4890c-129">Kategoriska funktioner kan skapas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4890c-129">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="4890c-130">Inspektera hello distribution av hello kategoriska kolumn:</span><span class="sxs-lookup"><span data-stu-id="4890c-130">Inspect hello distribution of hello categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="4890c-131">Generera indikator värden för varje hello kolumnvärdena</span><span class="sxs-lookup"><span data-stu-id="4890c-131">Generate indicator values for each of hello column values</span></span>
   
        #generate hello indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="4890c-132">Anslut hello indikator kolumn med hello ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="4890c-132">Join hello indicator column with hello original data frame</span></span> 
   
            #Join hello dummy variables back toohello original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="4890c-133">Ta bort hello ursprungliga själva variabeln:</span><span class="sxs-lookup"><span data-stu-id="4890c-133">Remove hello original variable itself:</span></span>
   
        #Remove hello original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="4890c-134"><a name="blob-binningfeature"></a>Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="4890c-134"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="4890c-135">För att generera binned funktioner fortsätta enligt följande:</span><span class="sxs-lookup"><span data-stu-id="4890c-135">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="4890c-136">Lägga till en sekvens av kolumner toobin en numerisk kolumn</span><span class="sxs-lookup"><span data-stu-id="4890c-136">Add a sequence of columns toobin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="4890c-137">Konvertera diskretisering tooa sekvens med booleska variabler</span><span class="sxs-lookup"><span data-stu-id="4890c-137">Convert binning tooa sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="4890c-138">Slutligen tillbaka koppling hello dummy variabler toohello ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="4890c-138">Finally, Join hello dummy variables back toohello original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <span data-ttu-id="4890c-139"><a name="sql-featuregen"></a>När data skrevs tillbaka tooAzure blob och använda i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="4890c-139"><a name="sql-featuregen"></a>Writing data back tooAzure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="4890c-140">När du har arbetat hello data och skapa hello nödvändiga funktioner, kan du överföra hello data (provtagning eller featurized) tooan Azure blob- och använda den i Azure Machine Learning med hjälp av följande hello: Observera att du kan skapa ytterligare funktioner i hello Azure Machine Learning Studio samt.</span><span class="sxs-lookup"><span data-stu-id="4890c-140">After you have explored hello data and created hello necessary features, you can upload hello data (sampled or featurized) tooan Azure blob and consume it in Azure Machine Learning using hello following steps: Note that additional features can be created in hello Azure Machine Learning Studio as well.</span></span> 

1. <span data-ttu-id="4890c-141">Skriva hello datafilen ram toolocal</span><span class="sxs-lookup"><span data-stu-id="4890c-141">Write hello data frame toolocal file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="4890c-142">Överför hello data tooAzure blob på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="4890c-142">Upload hello data tooAzure blob as follows:</span></span>
   
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
3. <span data-ttu-id="4890c-143">Nu hello data kan läsas från hello blob med hello Azure Machine Learning [importera Data] [ import-data] modulen som visas i hello-skärmen nedan:</span><span class="sxs-lookup"><span data-stu-id="4890c-143">Now hello data can be read from hello blob using hello Azure Machine Learning [Import Data][import-data] module as shown in hello screen below:</span></span>

![läsaren blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

