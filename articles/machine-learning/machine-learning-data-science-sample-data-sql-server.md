---
title: "Exempeldata i SQLServer på Azure | Microsoft Docs"
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
ms.openlocfilehash: 1bdcc7175dac325de1144d805e977264524b3fbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <span data-ttu-id="0f19c-103"><a name="heading"></a>Exempeldata i SQL Server på Azure</span><span class="sxs-lookup"><span data-stu-id="0f19c-103"><a name="heading"></a>Sample data in SQL Server on Azure</span></span>
<span data-ttu-id="0f19c-104">Det här dokumentet visar hur du samla in data som lagras i SQL Server på Azure med hjälp av SQL- eller Python-programmeringsspråket.</span><span class="sxs-lookup"><span data-stu-id="0f19c-104">This document shows how to sample data stored in SQL Server on Azure using either SQL or the Python programming language.</span></span> <span data-ttu-id="0f19c-105">Den visar även hur du flyttar samplade data till Azure Machine Learning genom att spara den till en fil, överföra den till en Azure blob och läsa den i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="0f19c-105">It also shows how to move sampled data into Azure Machine Learning by saving it to a file, uploading it to an Azure blob, and then reading it into Azure Machine Learning Studio.</span></span>

<span data-ttu-id="0f19c-106">Python provtagning använder den [pyodbc](https://code.google.com/p/pyodbc/) ODBC-biblioteket för att ansluta till SQL Server på Azure och [Pandas](http://pandas.pydata.org/) biblioteket för att göra beräkningarna.</span><span class="sxs-lookup"><span data-stu-id="0f19c-106">The Python sampling uses the [pyodbc](https://code.google.com/p/pyodbc/) ODBC library to connect to SQL Server on Azure and the [Pandas](http://pandas.pydata.org/) library to do the sampling.</span></span>

> [!NOTE]
> <span data-ttu-id="0f19c-107">Exempelkoden SQL i det här dokumentet förutsätter att data i en SQL Server på Azure.</span><span class="sxs-lookup"><span data-stu-id="0f19c-107">The sample SQL code in this document assumes that the data is in a SQL Server on Azure.</span></span> <span data-ttu-id="0f19c-108">Om det inte finns [flytta data till SQL Server på Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) avsnittet för instruktioner om hur du flyttar dina data till SQL Server på Azure.</span><span class="sxs-lookup"><span data-stu-id="0f19c-108">If it is not, please refer to [Move data to SQL Server on Azure](machine-learning-data-science-move-sql-server-virtual-machine.md) topic for instructions on how to move your data to SQL Server on Azure.</span></span>
> 
> 

<span data-ttu-id="0f19c-109">Följande **menyn** länkar till avsnitt som beskriver hur du exempeldata från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="0f19c-109">The following **menu** links to topics that describe how to sample data from various storage environments.</span></span> 

[!INCLUDE [cap-sample-data-selector](../../includes/cap-sample-data-selector.md)]

<span data-ttu-id="0f19c-110">**Varför exempel dina data?**</span><span class="sxs-lookup"><span data-stu-id="0f19c-110">**Why sample your data?**</span></span>
<span data-ttu-id="0f19c-111">Om datamängden som du planerar att analysera är stort, men det är vanligtvis en bra idé att ned-sample data för att minska det till en mindre men representativt och mer användarvänlig storlek.</span><span class="sxs-lookup"><span data-stu-id="0f19c-111">If the dataset you plan to analyze is large, it's usually a good idea to down-sample the data to reduce it to a smaller but representative and more manageable size.</span></span> <span data-ttu-id="0f19c-112">Detta underlättar data förstå undersökning och funktionen tekniker.</span><span class="sxs-lookup"><span data-stu-id="0f19c-112">This facilitates data understanding, exploration, and feature engineering.</span></span> <span data-ttu-id="0f19c-113">Roll i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) är att aktivera snabb prototyper för databearbetning funktions- och maskininlärning modeller.</span><span class="sxs-lookup"><span data-stu-id="0f19c-113">Its role in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) is to enable fast prototyping of the data processing functions and machine learning models.</span></span>

<span data-ttu-id="0f19c-114">Sampling uppgiften är ett steg i den [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="0f19c-114">This sampling task is a step in the [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

## <span data-ttu-id="0f19c-115"><a name="SQL"></a>Med SQL</span><span class="sxs-lookup"><span data-stu-id="0f19c-115"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="0f19c-116">Det här avsnittet beskrivs flera metoder som använder SQL för att utföra enkla slumpmässig provtagning mot data i databasen.</span><span class="sxs-lookup"><span data-stu-id="0f19c-116">This section describes several methods using SQL to perform simple random sampling against the data in the database.</span></span> <span data-ttu-id="0f19c-117">Välj en metod baserat på datastorleken på din och dess distributionsplatser.</span><span class="sxs-lookup"><span data-stu-id="0f19c-117">Please choose a method based on your data size and its distribution.</span></span>

<span data-ttu-id="0f19c-118">De två objekten nedan visar hur du använder newid i SQL Server för att utföra beräkningarna.</span><span class="sxs-lookup"><span data-stu-id="0f19c-118">The two items below show how to use newid in SQL Server to perform the sampling.</span></span> <span data-ttu-id="0f19c-119">Vilken metod du väljer beror på hur slumpmässiga du vill att samplet som ska vara (pk_id i koden nedan antas vara en automatiskt genererad primärnyckel).</span><span class="sxs-lookup"><span data-stu-id="0f19c-119">The method you choose depends on how random you want the sample to be (pk_id in the sample code below is assumed to be an auto-generated primary key).</span></span>

1. <span data-ttu-id="0f19c-120">Mindre strikta slumpmässigt prov</span><span class="sxs-lookup"><span data-stu-id="0f19c-120">Less strict random sample</span></span>
   
        select  * from <table_name> where <primary_key> in 
        (select top 10 percent <primary_key> from <table_name> order by newid())
2. <span data-ttu-id="0f19c-121">Fler slumpmässigt prov</span><span class="sxs-lookup"><span data-stu-id="0f19c-121">More random sample</span></span> 
   
        SELECT * FROM <table_name>
        WHERE 0.1 >= CAST(CHECKSUM(NEWID(), <primary_key>) & 0x7fffffff AS float)/ CAST (0x7fffffff AS int)

<span data-ttu-id="0f19c-122">Tablesample kan vara används för provtagning som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="0f19c-122">Tablesample can be used for sampling as well as demonstrated below.</span></span> <span data-ttu-id="0f19c-123">Detta kan vara en bättre metod om dina data är stor (förutsatt att data på olika sidor inte korreleras) och för frågan ska slutföras inom en rimlig tid.</span><span class="sxs-lookup"><span data-stu-id="0f19c-123">This may be a better approach if your data size is large (assuming that data on different pages is not correlated) and for the query to complete in a reasonable time.</span></span>

    SELECT *
    FROM <table_name> 
    TABLESAMPLE (10 PERCENT)

> [!NOTE]
> <span data-ttu-id="0f19c-124">Du kan utforska och generera funktioner från den här samplade data genom att lagra det i en ny tabell</span><span class="sxs-lookup"><span data-stu-id="0f19c-124">You can explore and generate features from this sampled data by storing it in a new table</span></span>
> 
> 

### <span data-ttu-id="0f19c-125"><a name="sql-aml"></a>Ansluta till Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0f19c-125"><a name="sql-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="0f19c-126">Du kan använda exempelfrågor ovan direkt i Azure Machine Learning [importera Data] [ import-data] modulen ned-sample data direkt och sätta den i ett Azure Machine Learning-experiment.</span><span class="sxs-lookup"><span data-stu-id="0f19c-126">You can directly  use the sample queries above in the Azure Machine Learning [Import Data][import-data] module to down-sample the data on the fly and bring it into an Azure Machine Learning experiment.</span></span> <span data-ttu-id="0f19c-127">En skärmbild av med hjälp av modulen läsare för att läsa samplade data visas nedan:</span><span class="sxs-lookup"><span data-stu-id="0f19c-127">A screen shot of using the reader module to read the sampled data is shown below:</span></span>

![läsaren sql][1]

## <span data-ttu-id="0f19c-129"><a name="python"></a>Använder Python programmeringsspråket</span><span class="sxs-lookup"><span data-stu-id="0f19c-129"><a name="python"></a>Using the Python programming language</span></span>
<span data-ttu-id="0f19c-130">Det här avsnittet visas hur du använder den [pyodbc biblioteket](https://code.google.com/p/pyodbc/) att upprätta en ODBC ansluta till en SQL server-databas i Python.</span><span class="sxs-lookup"><span data-stu-id="0f19c-130">This section demonstrates using the [pyodbc library](https://code.google.com/p/pyodbc/) to establish an ODBC connect to a SQL server database in Python.</span></span> <span data-ttu-id="0f19c-131">Anslutningssträngen för databasen är följande: (Ersätt servername, dbname, användarnamn och lösenord med din konfiguration):</span><span class="sxs-lookup"><span data-stu-id="0f19c-131">The database connection string is as follows: (replace servername, dbname, username and password with your configuration):</span></span>

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="0f19c-132">Den [Pandas](http://pandas.pydata.org/) i Python-bibliotek innehåller ett stort utbud av datastrukturer och verktyg för analys av data för datamanipulering för Python-programmering.</span><span class="sxs-lookup"><span data-stu-id="0f19c-132">The [Pandas](http://pandas.pydata.org/) library in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="0f19c-133">Koden nedan läser en 0,1% exempeldata från en tabell i Azure SQL-databas i en Pandas data:</span><span class="sxs-lookup"><span data-stu-id="0f19c-133">The code below reads a 0.1% sample of the data from a table in Azure SQL database into a Pandas data :</span></span>

    import pandas as pd

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select column1, cloumn2... from <table_name> tablesample (0.1 percent)''', conn)

<span data-ttu-id="0f19c-134">Nu kan du arbeta med samplade data i ramen Pandas data.</span><span class="sxs-lookup"><span data-stu-id="0f19c-134">You can now work with the sampled data in the Pandas data frame.</span></span> 

### <span data-ttu-id="0f19c-135"><a name="python-aml"></a>Ansluta till Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="0f19c-135"><a name="python-aml"></a>Connecting to Azure Machine Learning</span></span>
<span data-ttu-id="0f19c-136">Du kan använda följande exempelkod för att spara provtagning ned data till en fil och överför den till en Azure blob.</span><span class="sxs-lookup"><span data-stu-id="0f19c-136">You can use the following sample code to save the down-sampled data to a file and upload it to an Azure blob.</span></span> <span data-ttu-id="0f19c-137">Data i blob direkt kan läsas in i ett Experiment i Azure Machine Learning med hjälp av den [importera Data] [ import-data] modul.</span><span class="sxs-lookup"><span data-stu-id="0f19c-137">The data in the blob can be directly read into an Azure Machine Learning Experiment using the [Import Data][import-data] module.</span></span> <span data-ttu-id="0f19c-138">Stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="0f19c-138">The steps are as follows:</span></span> 

1. <span data-ttu-id="0f19c-139">Skriva ramen pandas data till en lokal fil</span><span class="sxs-lookup"><span data-stu-id="0f19c-139">Write the pandas data frame to a local file</span></span>
   
        dataframe.to_csv(os.path.join(os.getcwd(),LOCALFILENAME), sep='\t', encoding='utf-8', index=False)
2. <span data-ttu-id="0f19c-140">Ladda upp en lokal fil till Azure-blob</span><span class="sxs-lookup"><span data-stu-id="0f19c-140">Upload local file to Azure blob</span></span>
   
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
3. <span data-ttu-id="0f19c-141">Läsa data från Azure blob med hjälp av Azure Machine Learning [importera Data] [ import-data] modulen som visas i den skärmen grab nedan:</span><span class="sxs-lookup"><span data-stu-id="0f19c-141">Read data from Azure blob using Azure Machine Learning [Import Data][import-data] module as shown in the screen grab below:</span></span>

![läsaren blob][2]

## <a name="the-team-data-science-process-in-action-example"></a><span data-ttu-id="0f19c-143">Team vetenskap av data i åtgärden exempel</span><span class="sxs-lookup"><span data-stu-id="0f19c-143">The Team Data Science Process in Action example</span></span>
<span data-ttu-id="0f19c-144">En slutpunkt till slutpunkt genomgången exempel på Team av vetenskapliga data med en offentlig dataset finns [Team vetenskap av data i praktiken: med hjälp av SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="0f19c-144">For an end-to-end walkthrough example of the Team Data Science Process a using a public dataset, see [Team Data Science Process in Action: using SQL Sever](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_database.png
[2]: ./media/machine-learning-data-science-sample-sql-server-virtual-machine/reader_blob.png

[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/
