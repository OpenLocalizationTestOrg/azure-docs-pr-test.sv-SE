---
title: aaaExplore data i SQL Server-dator i Azure | Microsoft Docs
description: "Hur tooexplore data som lagras i en SQL Server-VM på Azure."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: ccbb3085-af9e-4ec2-9df2-15dcab261d05
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
ms.openlocfilehash: fcc449fc0d0e49be9b673cfb2de347cf44804017
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a><span data-ttu-id="a91d3-103">Utforska data i en virtuell dator med SQL Server på Azure</span><span class="sxs-lookup"><span data-stu-id="a91d3-103">Explore data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="a91d3-104">Det här dokumentet beskriver hur tooexplore data som lagras i en SQL Server-VM på Azure.</span><span class="sxs-lookup"><span data-stu-id="a91d3-104">This document covers how tooexplore data that is stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="a91d3-105">Detta kan göras med data wrangling med hjälp av SQL eller med ett programmeringsspråk som Python.</span><span class="sxs-lookup"><span data-stu-id="a91d3-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

<span data-ttu-id="a91d3-106">hello följande **menyn** länkar tootopics som beskriver hur toouse verktyg tooexplore data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="a91d3-106">hello following **menu** links tootopics that describe how toouse tools tooexplore data from various storage environments.</span></span> <span data-ttu-id="a91d3-107">Det här är ett steg i hello Cortana Analytics processen (CAP).</span><span class="sxs-lookup"><span data-stu-id="a91d3-107">This task is a step in hello Cortana Analytics Process (CAP).</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> <span data-ttu-id="a91d3-108">hello exempel SQL-instruktioner i det här dokumentet förutsätter att data är i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a91d3-108">hello sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="a91d3-109">Om det inte finns toohello moln datavetenskap processen kartan toolearn hur toomove dina data tooSQL Server.</span><span class="sxs-lookup"><span data-stu-id="a91d3-109">If it isn't, refer toohello cloud data science process map toolearn how toomove your data tooSQL Server.</span></span>
> 
> 

## <span data-ttu-id="a91d3-110"><a name="sql-dataexploration"></a>Utforska SQL-data med SQL-skript</span><span class="sxs-lookup"><span data-stu-id="a91d3-110"><a name="sql-dataexploration"></a>Explore SQL data with SQL scripts</span></span>
<span data-ttu-id="a91d3-111">Här följer några exempel SQL-skript som kan använda tooexplore datalager i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="a91d3-111">Here are a few sample SQL scripts that can be used tooexplore data stores in SQL Server.</span></span>

1. <span data-ttu-id="a91d3-112">Hämta hello antal observationer per dag</span><span class="sxs-lookup"><span data-stu-id="a91d3-112">Get hello count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="a91d3-113">Hämta hello nivåer i en kategoriska kolumn</span><span class="sxs-lookup"><span data-stu-id="a91d3-113">Get hello levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="a91d3-114">Hämta hello antalet nivåer i kombination med två kategoriska kolumner</span><span class="sxs-lookup"><span data-stu-id="a91d3-114">Get hello number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="a91d3-115">Hämta hello distribution för numeriska kolumner</span><span class="sxs-lookup"><span data-stu-id="a91d3-115">Get hello distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> <span data-ttu-id="a91d3-116">En praktisk t.ex, du kan använda hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) och finns toohello IPNB med titeln [NYC Data wrangling IPython anteckningsboken och SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) för en slutpunkt till slutpunkt-hanteringspaketen.</span><span class="sxs-lookup"><span data-stu-id="a91d3-116">For a practical example, you can use hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <span data-ttu-id="a91d3-117"><a name="python"></a>Utforska SQL-data med Python</span><span class="sxs-lookup"><span data-stu-id="a91d3-117"><a name="python"></a>Explore SQL data with Python</span></span>
<span data-ttu-id="a91d3-118">Med hjälp av Python tooexplore data och generera funktioner när hello data finns i SQL Server är liknande tooprocessing data i Azure blob med Python, enligt beskrivningen i [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="a91d3-118">Using Python tooexplore data and generate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python, as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="a91d3-119">hello data måste toobe läses in från hello databas i en pandas DataFrame och sedan behandlas vidare.</span><span class="sxs-lookup"><span data-stu-id="a91d3-119">hello data needs toobe loaded from hello database into a pandas DataFrame and then can be processed further.</span></span> <span data-ttu-id="a91d3-120">Vi dokumentera hello process för att ansluta toohello databasen och läsa in hello data i hello DataFrame i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="a91d3-120">We document hello process of connecting toohello database and loading hello data into hello DataFrame in this section.</span></span>

<span data-ttu-id="a91d3-121">hello följande Anslutningssträngens format kan vara används tooconnect tooa SQL Server-databas från Python med pyodbc (Ersätt servername, dbname, användarnamn och lösenord med dina specifika värden):</span><span class="sxs-lookup"><span data-stu-id="a91d3-121">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="a91d3-122">Hej [Pandas biblioteket](http://pandas.pydata.org/) i Python ger en omfattande uppsättning datastrukturer och verktyg för analys av data till datamanipulering för Python-programmering.</span><span class="sxs-lookup"><span data-stu-id="a91d3-122">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="a91d3-123">hello läser följande kod hello resultatet som returneras från en SQL Server-databas till en Pandas data ram:</span><span class="sxs-lookup"><span data-stu-id="a91d3-123">hello following code reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="a91d3-124">Nu kan du arbeta med hello Pandas DataFrame som beskrivs i avsnittet hello [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="a91d3-124">Now you can work with hello Pandas DataFrame as covered in hello topic [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="cortana-analytics-process-in-action-example"></a><span data-ttu-id="a91d3-125">Cortana Analytics processen i åtgärden exempel</span><span class="sxs-lookup"><span data-stu-id="a91d3-125">Cortana Analytics Process in Action Example</span></span>
<span data-ttu-id="a91d3-126">En slutpunkt till slutpunkt genomgången exempel på hello Cortana Analytics Process som använder en offentlig dataset finns [hello Team av vetenskapliga data i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="a91d3-126">For an end-to-end walkthrough example of hello Cortana Analytics Process using a public dataset, see [hello Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

