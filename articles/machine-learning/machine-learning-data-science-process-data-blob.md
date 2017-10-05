---
title: Bearbeta Azure blob-data med avancerade analyser | Microsoft Docs
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
ms.openlocfilehash: 36d950fd81029af82d9f2f652b2f01dba5fc8cc9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="0276a-103"><a name="heading"></a>Bearbeta Azure blob-data med avancerade analyser</span><span class="sxs-lookup"><span data-stu-id="0276a-103"><a name="heading"></a>Process Azure blob data with advanced analytics</span></span>
<span data-ttu-id="0276a-104">Det här dokumentet beskriver utforska data och genererar funktioner från data som lagras i Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="0276a-104">This document covers exploring data and generating features from data stored in Azure Blob storage.</span></span> 

## <a name="load-the-data-into-a-pandas-data-frame"></a><span data-ttu-id="0276a-105">Läsa in data i ett Pandas data</span><span class="sxs-lookup"><span data-stu-id="0276a-105">Load the data into a Pandas data frame</span></span>
<span data-ttu-id="0276a-106">För att utforska och ändra en datamängd, måste den hämtas från blob-källan till en lokal fil som sedan kan läsas in i en Pandas data ram.</span><span class="sxs-lookup"><span data-stu-id="0276a-106">In order to explore and manipulate a dataset, it must be downloaded from the blob source to a local file which can then be loaded in a Pandas data frame.</span></span> <span data-ttu-id="0276a-107">Här följer de steg för den här proceduren:</span><span class="sxs-lookup"><span data-stu-id="0276a-107">Here are the steps to follow for this procedure:</span></span>

1. <span data-ttu-id="0276a-108">Hämta data från Azure blob med följande Python exempelkod med blob-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0276a-108">Download the data from Azure blob with the following sample Python code using blob service.</span></span> <span data-ttu-id="0276a-109">Ersätt variabeln i koden nedan med dina specifika värden:</span><span class="sxs-lookup"><span data-stu-id="0276a-109">Replace the variable in the code below with your specific values:</span></span> 
   
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
2. <span data-ttu-id="0276a-110">Läsa data i en Pandas data-ram från den hämta filen.</span><span class="sxs-lookup"><span data-stu-id="0276a-110">Read the data into a Pandas data-frame from the downloaded file.</span></span>
   
        #LOCALFILE is the file path    
        dataframe_blobdata = pd.read_csv(LOCALFILE)

<span data-ttu-id="0276a-111">Du är nu redo att utforska data och generera funktioner på denna dataset.</span><span class="sxs-lookup"><span data-stu-id="0276a-111">Now you are ready to explore the data and generate features on this dataset.</span></span>

## <span data-ttu-id="0276a-112"><a name="blob-dataexploration"></a>Datagranskning</span><span class="sxs-lookup"><span data-stu-id="0276a-112"><a name="blob-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="0276a-113">Här följer några exempel på hur du kan utforska data med hjälp av Pandas:</span><span class="sxs-lookup"><span data-stu-id="0276a-113">Here are a few examples of ways to explore data using Pandas:</span></span>

1. <span data-ttu-id="0276a-114">Kontrollera antalet rader och kolumner</span><span class="sxs-lookup"><span data-stu-id="0276a-114">Inspect the number of rows and columns</span></span> 
   
        print 'the size of the data is: %d rows and  %d columns' % dataframe_blobdata.shape
2. <span data-ttu-id="0276a-115">Granska de första eller sista raderna i datauppsättningen enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="0276a-115">Inspect the first or last few rows in the dataset as below:</span></span>
   
        dataframe_blobdata.head(10)
   
        dataframe_blobdata.tail(10)
3. <span data-ttu-id="0276a-116">Kontrollera den datatyp som varje kolumn har importerats som använder följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="0276a-116">Check the data type each column was imported as using the following sample code</span></span>
   
        for col in dataframe_blobdata.columns:
            print dataframe_blobdata[col].name, ':\t', dataframe_blobdata[col].dtype
4. <span data-ttu-id="0276a-117">Kontrollera följande grundläggande statistik för kolumner i datauppsättning</span><span class="sxs-lookup"><span data-stu-id="0276a-117">Check the basic stats for the columns in the data set as follows</span></span>
   
        dataframe_blobdata.describe()
5. <span data-ttu-id="0276a-118">Titta på hur många poster för varje värde i kolumnen enligt följande</span><span class="sxs-lookup"><span data-stu-id="0276a-118">Look at the number of entries for each column value as follows</span></span>
   
        dataframe_blobdata['<column_name>'].value_counts()
6. <span data-ttu-id="0276a-119">Antal saknade värden jämfört med det faktiska antalet poster i varje kolumn med hjälp av följande exempelkod</span><span class="sxs-lookup"><span data-stu-id="0276a-119">Count missing values versus the actual number of entries in each column using the following sample code</span></span>
   
        miss_num = dataframe_blobdata.shape[0] - dataframe_blobdata.count()
        print miss_num
7. <span data-ttu-id="0276a-120">Om du har värden som saknas för en viss kolumn i data, kan du släppa dem på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0276a-120">If you have missing values for a specific column in the data, you can drop them as follows:</span></span>
   
     <span data-ttu-id="0276a-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna() dataframe_blobdata_noNA.shape</span><span class="sxs-lookup"><span data-stu-id="0276a-121">dataframe_blobdata_noNA = dataframe_blobdata.dropna()   dataframe_blobdata_noNA.shape</span></span>
   
   <span data-ttu-id="0276a-122">Ett annat sätt att ersätta saknade värden är med funktionen läge:</span><span class="sxs-lookup"><span data-stu-id="0276a-122">Another way to replace missing values is with the mode function:</span></span>
   
     <span data-ttu-id="0276a-123">dataframe_blobdata_mode = dataframe_blobdata.fillna ({< kolumnnamn >: .mode()[0]}) dataframe_blobdata ['< kolumnnamn >']</span><span class="sxs-lookup"><span data-stu-id="0276a-123">dataframe_blobdata_mode = dataframe_blobdata.fillna({'<column_name>':dataframe_blobdata['<column_name>'].mode()[0]})</span></span>        
8. <span data-ttu-id="0276a-124">Skapa ett histogram ritytans med hjälp av variabeln antal lagerplatser ska ritas distribution av en variabel</span><span class="sxs-lookup"><span data-stu-id="0276a-124">Create a histogram plot using variable number of bins to plot the distribution of a variable</span></span>    
   
        dataframe_blobdata['<column_name>'].value_counts().plot(kind='bar')
   
        np.log(dataframe_blobdata['<column_name>']+1).hist(bins=50)
9. <span data-ttu-id="0276a-125">Titta på korrelationer mellan variabler med hjälp av en scatterplot eller inbyggda korrelationen</span><span class="sxs-lookup"><span data-stu-id="0276a-125">Look at correlations between variables using a scatterplot or using the built-in correlation function</span></span>
   
        #relationship between column_a and column_b using scatter plot
        plt.scatter(dataframe_blobdata['<column_a>'], dataframe_blobdata['<column_b>'])
   
        #correlation between column_a and column_b
        dataframe_blobdata[['<column_a>', '<column_b>']].corr()

## <span data-ttu-id="0276a-126"><a name="blob-featuregen"></a>Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="0276a-126"><a name="blob-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="0276a-127">Vi kan generera funktioner med Python på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0276a-127">We can generate features using Python as follows:</span></span>

### <span data-ttu-id="0276a-128"><a name="blob-countfeature"></a>Indikatorvärdet baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="0276a-128"><a name="blob-countfeature"></a>Indicator value based Feature Generation</span></span>
<span data-ttu-id="0276a-129">Kategoriska funktioner kan skapas på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0276a-129">Categorical features can be created as follows:</span></span>

1. <span data-ttu-id="0276a-130">Kontrollera distributionen av kolumnen kategoriska:</span><span class="sxs-lookup"><span data-stu-id="0276a-130">Inspect the distribution of the categorical column:</span></span>
   
        dataframe_blobdata['<categorical_column>'].value_counts()
2. <span data-ttu-id="0276a-131">Generera indikator värden för varje kolumnvärdena</span><span class="sxs-lookup"><span data-stu-id="0276a-131">Generate indicator values for each of the column values</span></span>
   
        #generate the indicator column
        dataframe_blobdata_identity = pd.get_dummies(dataframe_blobdata['<categorical_column>'], prefix='<categorical_column>_identity')
3. <span data-ttu-id="0276a-132">Ansluta till kolumnen indikator med den ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="0276a-132">Join the indicator column with the original data frame</span></span> 
   
            #Join the dummy variables back to the original data frame
            dataframe_blobdata_with_identity = dataframe_blobdata.join(dataframe_blobdata_identity)
4. <span data-ttu-id="0276a-133">Ta bort själva ursprungliga variabeln:</span><span class="sxs-lookup"><span data-stu-id="0276a-133">Remove the original variable itself:</span></span>
   
        #Remove the original column rate_code in df1_with_dummy
        dataframe_blobdata_with_identity.drop('<categorical_column>', axis=1, inplace=True)

### <span data-ttu-id="0276a-134"><a name="blob-binningfeature"></a>Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="0276a-134"><a name="blob-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="0276a-135">För att generera binned funktioner fortsätta enligt följande:</span><span class="sxs-lookup"><span data-stu-id="0276a-135">For generating binned features, we proceed as follows:</span></span>

1. <span data-ttu-id="0276a-136">Lägga till en sekvens av kolumner till bin en numerisk kolumn</span><span class="sxs-lookup"><span data-stu-id="0276a-136">Add a sequence of columns to bin a numeric column</span></span>
   
        bins = [0, 1, 2, 4, 10, 40]
        dataframe_blobdata_bin_id = pd.cut(dataframe_blobdata['<numeric_column>'], bins)
2. <span data-ttu-id="0276a-137">Konvertera diskretisering till en sekvens med booleska variabler</span><span class="sxs-lookup"><span data-stu-id="0276a-137">Convert binning to a sequence of boolean variables</span></span>
   
        dataframe_blobdata_bin_bool = pd.get_dummies(dataframe_blobdata_bin_id, prefix='<numeric_column>')
3. <span data-ttu-id="0276a-138">Slutligen ansluta variablerna dummy tillbaka till den ursprungliga dataramen</span><span class="sxs-lookup"><span data-stu-id="0276a-138">Finally, Join the dummy variables back to the original data frame</span></span>
   
        dataframe_blobdata_with_bin_bool = dataframe_blobdata.join(dataframe_blobdata_bin_bool)    

## <span data-ttu-id="0276a-139"><a name="sql-featuregen"></a>Data skrivs tillbaka till Azure blob och använda i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0276a-139"><a name="sql-featuregen"></a>Writing data back to Azure blob and consuming in Azure Machine Learning</span></span>
<span data-ttu-id="0276a-140">När du har gjort data och skapa nödvändiga funktioner, kan du överföra data (provtagning eller featurized) till ett Azure blob- och använda den i Azure Machine Learning med följande steg: Observera att du kan skapa ytterligare funktioner i Azure Machine Learning Studio samt.</span><span class="sxs-lookup"><span data-stu-id="0276a-140">After you have explored the data and created the necessary features, you can upload the data (sampled or featurized) to an Azure blob and consume it in Azure Machine Learning using the following steps: Note that additional features can be created in the Azure Machine Learning Studio as well.</span></span> 

1. <span data-ttu-id="0276a-141">Skriva dataramen till en lokal fil</span><span class="sxs-lookup"><span data-stu-id="0276a-141">Write the data frame to local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="0276a-142">Överför data till Azure blob på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="0276a-142">Upload the data to Azure blob as follows:</span></span>
   
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
3. <span data-ttu-id="0276a-143">Nu går att läsa data från blob med hjälp av Azure Machine Learning [importera Data] [ import-data] modulen som visas på skärmen nedan:</span><span class="sxs-lookup"><span data-stu-id="0276a-143">Now the data can be read from the blob using the Azure Machine Learning [Import Data][import-data] module as shown in the screen below:</span></span>

![läsaren blob][1]

[1]: ./media/machine-learning-data-science-process-data-blob/reader_blob.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

