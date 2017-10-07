---
title: "aaaCreate funktioner för data i SQL Server med SQL och Python | Microsoft Docs"
description: "Bearbetning av Data från SQL Azure"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: 
ms.assetid: bf1f4a6c-7711-4456-beb7-35fdccd46a44
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev;fashah;garye
ms.openlocfilehash: 223352edb0377a159333e7528ad03c43902e6f45
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-features-for-data-in-sql-server-using-sql-and-python"></a><span data-ttu-id="f6ec5-103">Skapa funktioner för data i SQL Server med SQL och Python</span><span class="sxs-lookup"><span data-stu-id="f6ec5-103">Create features for data in SQL Server using SQL and Python</span></span>
<span data-ttu-id="f6ec5-104">Det här dokumentet beskrivs hur toogenerate funktioner för data som lagras i en SQL Server-VM på Azure som hjälper algoritmer lära sig mer effektivt från hello data.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-104">This document shows how toogenerate features for data stored in a SQL Server VM on Azure that help algorithms learn more efficiently from hello data.</span></span> <span data-ttu-id="f6ec5-105">Detta kan göras med hjälp av SQL eller genom att använda ett programmeringsspråk som Python, som visas här.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-105">This can be done by using SQL or by using a programming language like Python, both of which are demonstrated here.</span></span>

[!INCLUDE [cap-create-features-data-selector](../../includes/cap-create-features-selector.md)]

<span data-ttu-id="f6ec5-106">Detta **menyn** länkar tootopics som beskriver hur toocreate funktioner för data i olika miljöer.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-106">This **menu** links tootopics that describe how toocreate features for data in various environments.</span></span> <span data-ttu-id="f6ec5-107">Den här uppgiften är ett steg i hello [Team Data vetenskap processen (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span><span class="sxs-lookup"><span data-stu-id="f6ec5-107">This task is a step in hello [Team Data Science Process (TDSP)](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/).</span></span>

> [!NOTE]
> <span data-ttu-id="f6ec5-108">Ett praktiskt exempel, finns hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) och finns toohello IPNB med titeln [NYC Data wrangling IPython anteckningsboken och SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) för en slutpunkt till slutpunkt-hanteringspaketen.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-108">For a practical example, you can consult hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="f6ec5-109">Krav</span><span class="sxs-lookup"><span data-stu-id="f6ec5-109">Prerequisites</span></span>
<span data-ttu-id="f6ec5-110">Den här artikeln förutsätter att du har:</span><span class="sxs-lookup"><span data-stu-id="f6ec5-110">This article assumes that you have:</span></span>

* <span data-ttu-id="f6ec5-111">Skapa ett Azure storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-111">Created an Azure storage account.</span></span> <span data-ttu-id="f6ec5-112">Om du behöver mer information, se [skapa ett Azure Storage-konto](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span><span class="sxs-lookup"><span data-stu-id="f6ec5-112">If you need instructions, see [Create an Azure Storage account](../storage/common/storage-create-storage-account.md#create-a-storage-account)</span></span>
* <span data-ttu-id="f6ec5-113">Lagrade data i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-113">Stored your data in SQL Server.</span></span> <span data-ttu-id="f6ec5-114">Om du inte har Se [flytta data tooan Azure SQL Database för Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) anvisningar för hur toomove hello data där.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-114">If you have not, see [Move data tooan Azure SQL Database for Azure Machine Learning](machine-learning-data-science-move-sql-azure.md) for instructions on how toomove hello data there.</span></span>

## <span data-ttu-id="f6ec5-115"><a name="sql-featuregen"></a>Funktionen Generation med SQL</span><span class="sxs-lookup"><span data-stu-id="f6ec5-115"><a name="sql-featuregen"></a>Feature Generation with SQL</span></span>
<span data-ttu-id="f6ec5-116">I det här avsnittet beskrivs olika sätt att skapa funktioner med hjälp av SQL:</span><span class="sxs-lookup"><span data-stu-id="f6ec5-116">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="f6ec5-117">Antal baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="f6ec5-117">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="f6ec5-118">Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="f6ec5-118">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="f6ec5-119">Lansera hello funktioner från en enda kolumn</span><span class="sxs-lookup"><span data-stu-id="f6ec5-119">Rolling out hello features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="f6ec5-120">När du skapar ytterligare funktioner, kan du lägga till dem som kolumner toohello befintlig tabell eller skapa en ny tabell med hello ytterligare funktioner och primärnyckel som kan förenas med hello ursprungliga tabellen.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-120">Once you generate additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, that can be joined with hello original table.</span></span>
> 
> 

### <span data-ttu-id="f6ec5-121"><a name="sql-countfeature"></a>Antal baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="f6ec5-121"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="f6ec5-122">Det här dokumentet visar två sätt att generera antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-122">This document demonstrates two ways of generating count features.</span></span> <span data-ttu-id="f6ec5-123">hello första metoden använder Villkorlig summering och hello andra metoden använder hello uttrycket 'where'.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-123">hello first method uses conditional sum and hello second method uses hello 'where\` clause.</span></span> <span data-ttu-id="f6ec5-124">De kan sedan kopplas med hello ursprungliga tabellen (med primärnyckelkolumnerna) toohave antal funktioner tillsammans med hello ursprungliga data.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-124">These can then be joined with hello original table (using primary key columns) toohave count features alongside hello original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3>

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename>
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2>

### <span data-ttu-id="f6ec5-125"><a name="sql-binningfeature"></a>Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="f6ec5-125"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="f6ec5-126">hello följande exempel visas hur toogenerate binned funktioner av diskretisering (med 5 lagerplatser) en numerisk kolumn som kan användas som en funktion i stället:</span><span class="sxs-lookup"><span data-stu-id="f6ec5-126">hello following example shows how toogenerate binned features by binning (using 5 bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="f6ec5-127"><a name="sql-featurerollout"></a>Lansera hello funktioner från en enda kolumn</span><span class="sxs-lookup"><span data-stu-id="f6ec5-127"><a name="sql-featurerollout"></a>Rolling out hello features from a single column</span></span>
<span data-ttu-id="f6ec5-128">I det här avsnittet visar vi hur tooroll ut en kolumn i en tabell toogenerate ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-128">In this section, we demonstrate how tooroll-out a single column in a table toogenerate additional features.</span></span> <span data-ttu-id="f6ec5-129">hello exemplet förutsätter att det finns en latitud och longitud kolumn i hello tabell från vilken du försöker toogenerate funktioner.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-129">hello example assumes that there is a latitude or longitude column in hello table from which you are trying toogenerate features.</span></span>

<span data-ttu-id="f6ec5-130">Här är en kort introduktion om latitud/longitud platsdata (resurstilldelas från stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span><span class="sxs-lookup"><span data-stu-id="f6ec5-130">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow `http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude`).</span></span> <span data-ttu-id="f6ec5-131">Detta är användbart toounderstand innan featurizing hello Platsfältet:</span><span class="sxs-lookup"><span data-stu-id="f6ec5-131">This is useful toounderstand before featurizing hello location field:</span></span>

* <span data-ttu-id="f6ec5-132">hello logga talar om för oss om vi är norr eller Syd, Öst eller Väst på hello jordglob.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-132">hello sign tells us whether we are north or south, east or west on hello globe.</span></span>
* <span data-ttu-id="f6ec5-133">Ett annat värde än noll hundratals siffra visar vi använder longitud inte latitud!</span><span class="sxs-lookup"><span data-stu-id="f6ec5-133">A nonzero hundreds digit tells us we're using longitude, not latitude!</span></span>
* <span data-ttu-id="f6ec5-134">hello flera siffror ger en position tooabout 1 000 kilometer.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-134">hello tens digit gives a position tooabout 1,000 kilometers.</span></span> <span data-ttu-id="f6ec5-135">Det ger oss användbar information om vilka kontinent eller oceanen vi finns på.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-135">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="f6ec5-136">hello enheter siffra (en decimal grad) ger en position upp too111 kilometer (60 sjömil, om 69 miles).</span><span class="sxs-lookup"><span data-stu-id="f6ec5-136">hello units digit (one decimal degree) gives a position up too111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="f6ec5-137">Det kan berätta ungefär vad eller det stora land vi finns i.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-137">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="f6ec5-138">hello första decimaltecknet är värda too11.1 km: den kan skilja hello positionen för en stor ort från en närliggande stora stad.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-138">hello first decimal place is worth up too11.1 km: it can distinguish hello position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="f6ec5-139">hello andra decimaltecknet är värda too1.1 km: den kan skilja en village från hello nästa.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-139">hello second decimal place is worth up too1.1 km: it can separate one village from hello next.</span></span>
* <span data-ttu-id="f6ec5-140">hello tredje decimaltecknet är värda upp too110 m: den kan identifiera en stor jordbruket institutionella plats.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-140">hello third decimal place is worth up too110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="f6ec5-141">hello fjärde decimaltecknet är värda upp too11 m: mark kan identifiera.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-141">hello fourth decimal place is worth up too11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="f6ec5-142">Det är jämförbar toohello vanliga korrektheten i en okorrigerade GPS-enhet utan störningar.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-142">It is comparable toohello typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="f6ec5-143">hello femte decimaltecknet är värda upp too1.1 m: den skilja träd från varandra.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-143">hello fifth decimal place is worth up too1.1 m: it distinguish trees from each other.</span></span> <span data-ttu-id="f6ec5-144">Precision toothis nivå med kommersiella GPS-enheter kan endast uppnås med differentiell korrigering.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-144">Accuracy toothis level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="f6ec5-145">hello sjätte decimaltecknet är värda upp too0.11 m: kan du använda den för att utforma strukturer i detalj för att utforma landskap, skapa vägar.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-145">hello sixth decimal place is worth up too0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="f6ec5-146">Det bör vara mer än tillräckligt bra för spårning av glaciers och floder.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-146">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="f6ec5-147">Detta kan uppnås genom att göra painstaking åtgärder med GPS, till exempel differentially korrigerade GPS.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-147">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="f6ec5-148">hello platsinformation kan kan vara featurized på följande sätt att skilja ut region, plats och information om ort.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-148">hello location information can can be featurized as follows, separating out region, location and city information.</span></span> <span data-ttu-id="f6ec5-149">Observera att en gång kan även anropa en REST-slutpunkt, till exempel Bing Maps API finns på `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello region/distrikt information.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-149">Note that once can also call a REST end point such as Bing Maps API available at `https://msdn.microsoft.com/library/ff701710.aspx` tooget hello region/district information.</span></span>

    select
        <location_columnname>
        ,round(<location_columnname>,0) as l1        
        ,l2=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 1 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),1,1) else '0' end     
        ,l3=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 2 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),2,1) else '0' end     
        ,l4=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 3 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),3,1) else '0' end     
        ,l5=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 4 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),4,1) else '0' end     
        ,l6=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 5 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),5,1) else '0' end     
        ,l7=case when LEN (PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1)) >= 6 then substring(PARSENAME(round(ABS(<location_columnname>) - FLOOR(ABS(<location_columnname>)),6),1),6,1) else '0' end     
    from <tablename>

<span data-ttu-id="f6ec5-150">hello ovanstående plats baserat funktioner kan vara ytterligare används toogenerate ytterligare antal funktioner som tidigare beskrivits.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-150">hello above location based features can be further used toogenerate additional count features as described earlier.</span></span>

> [!TIP]
> <span data-ttu-id="f6ec5-151">Du kan via programmering Infoga hello poster med hjälp av ditt språk.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-151">You can programmatically insert hello records using your language of choice.</span></span> <span data-ttu-id="f6ec5-152">Du kanske måste tooinsert hello data i segment tooimprove skrivåtgärder effektivitet [kolla hello exempel på hur toodo denna med pyodbc här](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span><span class="sxs-lookup"><span data-stu-id="f6ec5-152">You may need tooinsert hello data in chunks tooimprove write efficiency [Check out hello example of how toodo this using pyodbc here](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python).</span></span>
> <span data-ttu-id="f6ec5-153">Ett annat alternativ är tooinsert data i hello databas med hjälp av [BCP-verktyget](https://msdn.microsoft.com/library/ms162802.aspx)</span><span class="sxs-lookup"><span data-stu-id="f6ec5-153">Another alternative is tooinsert data in hello database using [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx)</span></span>
> 
> 

### <span data-ttu-id="f6ec5-154"><a name="sql-aml"></a>Ansluta tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="f6ec5-154"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="f6ec5-155">hello nya funktionen kan läggas till som en befintlig tabell tooan för kolumn eller lagras i en ny tabell och kopplas till hello ursprungliga tabellen för machine learning.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-155">hello newly generated feature can be added as a column tooan existing table or stored in a new table and joined with hello original table for machine learning.</span></span> <span data-ttu-id="f6ec5-156">Funktioner kan genereras eller komma åt om redan har skapats med hjälp av hello [importera Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) modul i Azure ML enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="f6ec5-156">Features can be generated or accessed if already created, using hello [Import Data](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) module in Azure ML as shown below:</span></span>

![azureml-läsare](./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png)

## <span data-ttu-id="f6ec5-158"><a name="python"></a>Med hjälp av programmeringsspråk som Python</span><span class="sxs-lookup"><span data-stu-id="f6ec5-158"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="f6ec5-159">Använda Python toogenerate funktioner när hello data i SQL Server är liknande tooprocessing data i Azure blob använder Python enligt beskrivningen i [processen Azure Blob-data i du datavetenskap miljö](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="f6ec5-159">Using Python toogenerate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python as documented in [Process Azure Blob data in you data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="f6ec5-160">hello data måste toobe läses in från hello databasen i ett pandas data och sedan behandlas vidare.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-160">hello data needs toobe loaded from hello database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="f6ec5-161">Vi dokumentera hello process för att ansluta databasen toohello och hello data läses in i hello data ram i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-161">We document hello process of connecting toohello database and loading hello data into hello data frame in this section.</span></span>

<span data-ttu-id="f6ec5-162">hello följande Anslutningssträngens format kan vara används tooconnect tooa SQL Server-databas från Python med pyodbc (Ersätt servername, dbname, användarnamn och lösenord med dina specifika värden):</span><span class="sxs-lookup"><span data-stu-id="f6ec5-162">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="f6ec5-163">Hej [Pandas biblioteket](http://pandas.pydata.org/) i Python ger en omfattande uppsättning datastrukturer och verktyg för analys av data till datamanipulering för Python-programmering.</span><span class="sxs-lookup"><span data-stu-id="f6ec5-163">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="f6ec5-164">hello koden nedan läser hello resultatet som returneras från en SQL Server-databas till en Pandas data ram:</span><span class="sxs-lookup"><span data-stu-id="f6ec5-164">hello code below reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="f6ec5-165">Nu kan du arbeta med hello Pandas data ram som beskrivs i avsnitt [skapa funktioner för Azure blob storage-data med hjälp av Panda](machine-learning-data-science-create-features-blob.md).</span><span class="sxs-lookup"><span data-stu-id="f6ec5-165">Now you can work with hello Pandas data frame as covered in topics [Create features for Azure blob storage data using Panda](machine-learning-data-science-create-features-blob.md).</span></span>

