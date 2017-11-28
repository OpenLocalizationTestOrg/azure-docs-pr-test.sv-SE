---
title: Utforska data i SQL Server-dator i Azure | Microsoft Docs
description: "Så här utforska data som lagras i en SQL Server-VM på Azure."
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
ms.openlocfilehash: a2be21ef15b9209db1e97150e0297558fa69a7be
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="explore-data-in-sql-server-virtual-machine-on-azure"></a><span data-ttu-id="704c9-103">Utforska data i en virtuell dator med SQL Server på Azure</span><span class="sxs-lookup"><span data-stu-id="704c9-103">Explore data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="704c9-104">Det här dokumentet beskriver hur du utforska data som lagras i en SQL Server-VM på Azure.</span><span class="sxs-lookup"><span data-stu-id="704c9-104">This document covers how to explore data that is stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="704c9-105">Detta kan göras med data wrangling med hjälp av SQL eller med ett programmeringsspråk som Python.</span><span class="sxs-lookup"><span data-stu-id="704c9-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

<span data-ttu-id="704c9-106">Följande **menyn** länkar till avsnitt som beskriver hur du använder Verktyg för att utforska data från olika miljöer för lagring.</span><span class="sxs-lookup"><span data-stu-id="704c9-106">The following **menu** links to topics that describe how to use tools to explore data from various storage environments.</span></span> <span data-ttu-id="704c9-107">Det här är ett steg i Cortana Analytics processen (CAP).</span><span class="sxs-lookup"><span data-stu-id="704c9-107">This task is a step in the Cortana Analytics Process (CAP).</span></span>

[!INCLUDE [cap-explore-data-selector](../../includes/cap-explore-data-selector.md)]

> [!NOTE]
> <span data-ttu-id="704c9-108">Exempel SQL-instruktioner i det här dokumentet förutsätter att data är i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="704c9-108">The sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="704c9-109">Om det inte finns molnet vetenskap processen mappa data att lära dig hur du flyttar dina data till SQL Server.</span><span class="sxs-lookup"><span data-stu-id="704c9-109">If it isn't, refer to the cloud data science process map to learn how to move your data to SQL Server.</span></span>
> 
> 

## <span data-ttu-id="704c9-110"><a name="sql-dataexploration"></a>Utforska SQL-data med SQL-skript</span><span class="sxs-lookup"><span data-stu-id="704c9-110"><a name="sql-dataexploration"></a>Explore SQL data with SQL scripts</span></span>
<span data-ttu-id="704c9-111">Här följer några exempel SQL-skript som kan användas för att utforska data lagras i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="704c9-111">Here are a few sample SQL scripts that can be used to explore data stores in SQL Server.</span></span>

1. <span data-ttu-id="704c9-112">Hämta antal observationer per dag</span><span class="sxs-lookup"><span data-stu-id="704c9-112">Get the count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="704c9-113">Hämta nivåerna i en kategoriska kolumn</span><span class="sxs-lookup"><span data-stu-id="704c9-113">Get the levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="704c9-114">Hämta antalet nivåer i kombination med två kategoriska kolumner</span><span class="sxs-lookup"><span data-stu-id="704c9-114">Get the number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="704c9-115">Hämta distribution för numeriska kolumner</span><span class="sxs-lookup"><span data-stu-id="704c9-115">Get the distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

> [!NOTE]
> <span data-ttu-id="704c9-116">En praktisk t.ex, du kan använda den [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) och referera till IPNB med rubriken [NYC Data wrangling IPython anteckningsboken och SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) för en slutpunkt till slutpunkt-hanteringspaketen.</span><span class="sxs-lookup"><span data-stu-id="704c9-116">For a practical example, you can use the [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer to the IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <span data-ttu-id="704c9-117"><a name="python"></a>Utforska SQL-data med Python</span><span class="sxs-lookup"><span data-stu-id="704c9-117"><a name="python"></a>Explore SQL data with Python</span></span>
<span data-ttu-id="704c9-118">Använda Python att utforska data och generera funktioner om data finns i SQL Server är ungefär som att bearbeta data i Azure blob med Python, enligt beskrivningen i [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="704c9-118">Using Python to explore data and generate features when the data is in SQL Server is similar to processing data in Azure blob using Python, as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="704c9-119">Data måste läsas in från databasen till en pandas DataFrame och sedan behandlas vidare.</span><span class="sxs-lookup"><span data-stu-id="704c9-119">The data needs to be loaded from the database into a pandas DataFrame and then can be processed further.</span></span> <span data-ttu-id="704c9-120">Vi dokumentera processen för att ansluta till databasen och läsa in data i DataFrame i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="704c9-120">We document the process of connecting to the database and loading the data into the DataFrame in this section.</span></span>

<span data-ttu-id="704c9-121">Följande connection-strängformat kan användas för att ansluta till en SQL Server-databas från Python med pyodbc (Ersätt servername, dbname, användarnamn och lösenord med dina specifika värden):</span><span class="sxs-lookup"><span data-stu-id="704c9-121">The following connection string format can be used to connect to a SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up the SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="704c9-122">Den [Pandas biblioteket](http://pandas.pydata.org/) i Python ger en omfattande uppsättning datastrukturer och verktyg för analys av data till datamanipulering för Python-programmering.</span><span class="sxs-lookup"><span data-stu-id="704c9-122">The [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="704c9-123">Följande kod läser resultatet som returneras från en SQL Server-databas till en Pandas data ram:</span><span class="sxs-lookup"><span data-stu-id="704c9-123">The following code reads the results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load the returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="704c9-124">Nu kan du arbeta med Pandas DataFrame som beskrivs i avsnittet [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="704c9-124">Now you can work with the Pandas DataFrame as covered in the topic [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="cortana-analytics-process-in-action-example"></a><span data-ttu-id="704c9-125">Cortana Analytics processen i åtgärden exempel</span><span class="sxs-lookup"><span data-stu-id="704c9-125">Cortana Analytics Process in Action Example</span></span>
<span data-ttu-id="704c9-126">En slutpunkt till slutpunkt genomgången exempel processens Cortana Analytics med hjälp av en offentlig dataset finns [Team datavetenskap Process i praktiken: använder SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="704c9-126">For an end-to-end walkthrough example of the Cortana Analytics Process using a public dataset, see [The Team Data Science Process in action: using SQL Server](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

