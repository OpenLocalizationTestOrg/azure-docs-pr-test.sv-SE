---
title: "aaaSample Data i SQL Server på Azure | Microsoft Docs"
description: "Exempeldata i SQLServer på Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 33c030d4-5cca-4cc9-99d7-2bd13a3926af
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: dc7f9529c771f6deb633775557e64a04b774f5b1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="3969a-103"><a name="heading"></a>Exempeldata i SQL Server på Azure</span><span class="sxs-lookup"><span data-stu-id="3969a-103"><a name="heading"></a>Sample data in SQL Server on Azure</span></span>
<span data-ttu-id="3969a-104">Det här dokumentet beskrivs hur toosample data lagras i SQL Server på Azure med hjälp av SQL eller hello Python-programmeringsspråket.</span><span class="sxs-lookup"><span data-stu-id="3969a-104">This document shows how toosample data stored in SQL Server on Azure using either SQL or hello Python programming language.</span></span> <span data-ttu-id="3969a-105">Den visar även hur toomove exempeldata i Azure Machine Learning genom att spara den tooa filen, överföra den tooan Azure blob och läs sedan i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="3969a-105">It also shows how toomove sampled data into Azure Machine Learning by saving it tooa file, uploading it tooan Azure blob, and then reading it into Azure Machine Learning Studio.</span></span>

<span data-ttu-id="3969a-106">hello Python provtagning använder hello [pyodbc](https://code.google.com/p/pyodbc/) ODBC-biblioteket tooconnect tooSQL Server på Azure och hello [Pandas](http://pandas.pydata.org/) biblioteket toodo hello provtagning.</span><span class="sxs-lookup"><span data-stu-id="3969a-106">hello Python sampling uses hello [pyodbc](https://code.google.com/p/pyodbc/) ODBC library tooconnect tooSQL Server on Azure and hello [Pandas](http://pandas.pydata.org/) library toodo hello sampling.</span></span>

> [!NOTE]
> <span data-ttu-id="3969a-107">hello SQL exempelkoden i det här dokumentet förutsätter att hello data är i en SQL Server på Azure.</span><span class="sxs-lookup"><span data-stu-id="3969a-107">hello sample SQL code in this document assumes that hello data is in a SQL Server on Azure.</span></span> <span data-ttu-id="3969a-108">Om det inte finns för[flytta data tooSQL Server på Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) avsnittet för instruktioner om hur toomove dina data tooSQL Server på Azure.</span><span class="sxs-lookup"><span data-stu-id="3969a-108">If it is not, please refer too[Move data tooSQL Server on Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) topic for instructions on how toomove your data tooSQL Server on Azure.</span></span>
> 
> 

<span data-ttu-id="3969a-109">hello följande **menyn** länkar tootopics som beskriver hur toosample data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="3969a-109">hello following **menu** links tootopics that describe how toosample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="3969a-110">**Varför exempel dina data?**</span><span class="sxs-lookup"><span data-stu-id="3969a-110">**Why sample your data?**</span></span>
<span data-ttu-id="3969a-111">Om hello dataset som du planerar tooanalyze är stor, är det vanligtvis en god idé toodown sampel hello data tooreduce den tooa mindre men representativt och mer användarvänlig storlek.</span><span class="sxs-lookup"><span data-stu-id="3969a-111">If hello dataset you plan tooanalyze is large, it's usually a good idea toodown-sample hello data tooreduce it tooa smaller but representative and more manageable size.</span></span> <span data-ttu-id="3969a-112">Detta underlättar data förstå undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="3969a-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="3969a-113">Roll i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) är tooenable snabb prototyper hello databearbetning funktioner och maskininlärning modeller.</span><span class="sxs-lookup"><span data-stu-id="3969a-113">Its role in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is tooenable fast prototyping of hello data processing functions and machine learning models.</span></span>

<span data-ttu-id="3969a-114">Sampling uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="3969a-114">This sampling task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <span data-ttu-id="3969a-115"><a name="SQL"></a>Med SQL</span><span class="sxs-lookup"><span data-stu-id="3969a-115"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="3969a-116">Det här avsnittet beskrivs flera metoder som använder SQL tooperform obundet slumpmässigt urval mot hello data i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="3969a-116">This section describes several methods using SQL tooperform simple random sampling against hello data in hello database.</span></span> <span data-ttu-id="3969a-117">Välj en metod baserat på datastorleken på din och dess distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="3969a-117">Please choose a method based on your data size and its distribution.</span></span>

<span data-ttu-id="3969a-118">hello två objekt nedan visar hur toouse newid i SQL Server-tooperform hello provtagning.</span><span class="sxs-lookup"><span data-stu-id="3969a-118">hello two items below show how toouse newid in SQL Server tooperform hello sampling.</span></span> <span data-ttu-id="3969a-119">hello metod du väljer beror på hur slumpmässiga du vill hello exempel toobe (pk_id i hello exempelkoden nedan antas toobe en automatiskt genererad primärnyckel).</span><span class="sxs-lookup"><span data-stu-id="3969a-119">hello method you choose depends on how random you want hello sample toobe (pk_id in hello sample code below is assumed toobe an auto-generated primary key).</span></span>

1. <span data-ttu-id="3969a-120">Mindre strikta slumpmässigt prov</span><span class="sxs-lookup"><span data-stu-id="3969a-120">Less strict random sample</span></span>
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. <span data-ttu-id="3969a-121">Fler slumpmässigt prov</span><span class="sxs-lookup"><span data-stu-id="3969a-121">More random sample</span></span> 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

<span data-ttu-id="3969a-122">Tablesample kan vara används för provtagning som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="3969a-122">Tablesample can be used for sampling as well as demonstrated below.</span></span> <span data-ttu-id="3969a-123">Detta kan vara en bättre metod om dina data är stor (förutsatt att data på olika sidor inte korreleras) och för hello frågan toocomplete i rimlig tid.</span><span class="sxs-lookup"><span data-stu-id="3969a-123">This may be a better approach if your data size is large (assuming that data on different pages is not correlated) and for hello query toocomplete in a reasonable time.</span></span>

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> <span data-ttu-id="3969a-124">Du kan utforska och generera funktioner från den här samplade data genom att lagra det i en ny tabell</span><span class="sxs-lookup"><span data-stu-id="3969a-124">You can explore and generate features from this sampled data by storing it in a new table</span></span>
> 
> 

### <span data-ttu-id="3969a-125"><a name="sql-aml"></a>Ansluta tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3969a-125"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="3969a-126">Du kan använda hello exempelfrågor ovan direkt i hello Azure Machine Learning [importera Data] [ import-data] modulen toodown sampel hello data på hello föra och sätta den i ett Azure Machine Learning-experiment.</span><span class="sxs-lookup"><span data-stu-id="3969a-126">You can directly  use hello sample queries above in hello Azure Machine Learning [Import Data][import-data] module toodown-sample hello data on hello fly and bring it into an Azure Machine Learning experiment.</span></span> <span data-ttu-id="3969a-127">En skärmbild av med hello reader modulen tooread hello provtagning data visas nedan:</span><span class="sxs-lookup"><span data-stu-id="3969a-127">A screen shot of using hello reader module tooread hello sampled data is shown below:</span></span>

![läsaren sql][1]

## <span data-ttu-id="3969a-129"><a name="python"></a>Med hjälp av programmeringsspråk för hello Python</span><span class="sxs-lookup"><span data-stu-id="3969a-129"><a name="python"></a>Using hello Python programming language</span></span>
<span data-ttu-id="3969a-130">Det här avsnittet visas hur du använder hello [pyodbc biblioteket](https://code.google.com/p/pyodbc/) tooestablish ODBC ansluta tooa SQL server-databas i Python.</span><span class="sxs-lookup"><span data-stu-id="3969a-130">This section demonstrates using hello [pyodbc library](https://code.google.com/p/pyodbc/) tooestablish an ODBC connect tooa SQL server database in Python.</span></span> <span data-ttu-id="3969a-131">hello databasanslutningssträng är följande: (Ersätt servername, dbname, användarnamn och lösenord med din konfiguration):</span><span class="sxs-lookup"><span data-stu-id="3969a-131">hello database connection string is as follows: (replace servername, dbname, username and password with your configuration):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="3969a-132">Hej [Pandas](http://pandas.pydata.org/) i Python-bibliotek innehåller ett stort utbud av datastrukturer och verktyg för analys av data för datamanipulering för Python-programmering.</span><span class="sxs-lookup"><span data-stu-id="3969a-132">hello [Pandas](http://pandas.pydata.org/) library in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="3969a-133">hello koden nedan läser ett 0,1% exempel hello data från en tabell i Azure SQL-databas i en Pandas data:</span><span class="sxs-lookup"><span data-stu-id="3969a-133">hello code below reads a 0.1% sample of hello data from a table in Azure SQL database into a Pandas data :</span></span>

    import pandas as pd

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

<span data-ttu-id="3969a-134">Nu kan du arbeta med hello provtagning data i hello Pandas data ram.</span><span class="sxs-lookup"><span data-stu-id="3969a-134">You can now work with hello sampled data in hello Pandas data frame.</span></span> 

### <span data-ttu-id="3969a-135"><a name="python-aml"></a>Ansluta tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="3969a-135"><a name="python-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="3969a-136">Du kan använda följande kod toosave hello ned sampla tooa exempeldatafil hello och överföra den tooan Azure blob.</span><span class="sxs-lookup"><span data-stu-id="3969a-136">You can use hello following sample code toosave hello down-sampled data tooa file and upload it tooan Azure blob.</span></span> <span data-ttu-id="3969a-137">hello data i blob hello kan direkt läsas in i ett Azure Machine Learning-Experiment med hello [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="3969a-137">hello data in hello blob can be directly read into an Azure Machine Learning Experiment using hello [Import Data][import-data] module.</span></span> <span data-ttu-id="3969a-138">hello stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="3969a-138">hello steps are as follows:</span></span> 

1. <span data-ttu-id="3969a-139">Skriva hello pandas ram tooa lokala datafilen</span><span class="sxs-lookup"><span data-stu-id="3969a-139">Write hello pandas data frame tooa local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="3969a-140">Ladda upp en lokal fil tooAzure blob</span><span class="sxs-lookup"><span data-stu-id="3969a-140">Upload local file tooAzure blob</span></span>
   
        from azure.storage import BlobService
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
3. <span data-ttu-id="3969a-141">Läsa data från Azure blob med hjälp av Azure Machine Learning [importera Data] [ import-data] modulen som visas i fälten för hello-skärmen nedan:</span><span class="sxs-lookup"><span data-stu-id="3969a-141">Read data from Azure blob using Azure Machine Learning [Import Data][import-data] module as shown in hello screen grab below:</span></span>

![läsaren blob][2]

## <a name="hello-team-data-science-process-in-action-example"></a><span data-ttu-id="3969a-143">hello Team av vetenskapliga data i åtgärden exempel</span><span class="sxs-lookup"><span data-stu-id="3969a-143">hello Team Data Science Process in Action example</span></span>
<span data-ttu-id="3969a-144">En slutpunkt till slutpunkt genomgången exempel på hello Team av vetenskapliga data med en offentlig dataset finns [Team vetenskap av data i praktiken: med hjälp av SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="3969a-144">For an end-to-end walkthrough example of hello Team Data Science Process a using a public dataset, see [Team Data Science Process in Action: using SQL Sever](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
