---
title: aaaExplore data i en SQL Server-dator i Azure | Microsoft Docs
description: "Utforska data och skapa funktioner i en virtuell dator i SQL Server på Azure"
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: 
ms.assetid: 3949fb2c-ffab-49fb-908d-27d5e42f743b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: fashah;garye;bradsev
ms.openlocfilehash: 67f4b058b0f6557ee15fd42795c918d68f1a9871
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <span data-ttu-id="b0de0-103"><a name="heading"></a>Bearbeta Data i SQL Server-dator på Azure</span><span class="sxs-lookup"><span data-stu-id="b0de0-103"><a name="heading"></a>Process Data in SQL Server Virtual Machine on Azure</span></span>
<span data-ttu-id="b0de0-104">Det här dokumentet beskriver hur tooexplore data och generera data som lagras i en SQL Server-VM på Azure-funktioner.</span><span class="sxs-lookup"><span data-stu-id="b0de0-104">This document covers how tooexplore data and generate features for data stored in a SQL Server VM on Azure.</span></span> <span data-ttu-id="b0de0-105">Detta kan göras med data wrangling med hjälp av SQL eller med ett programmeringsspråk som Python.</span><span class="sxs-lookup"><span data-stu-id="b0de0-105">This can be done by data wrangling using SQL or by using a programming language like Python.</span></span>

> [!NOTE]
> <span data-ttu-id="b0de0-106">hello exempel SQL-instruktioner i det här dokumentet förutsätter att data är i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0de0-106">hello sample SQL statements in this document assume that data is in SQL Server.</span></span> <span data-ttu-id="b0de0-107">Om det inte finns toohello moln datavetenskap processen kartan toolearn hur toomove dina data tooSQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0de0-107">If it isn't, refer toohello cloud data science process map toolearn how toomove your data tooSQL Server.</span></span>
> 
> 

## <span data-ttu-id="b0de0-108"><a name="SQL"></a>Med SQL</span><span class="sxs-lookup"><span data-stu-id="b0de0-108"><a name="SQL"></a>Using SQL</span></span>
<span data-ttu-id="b0de0-109">Vi beskriver hello följande data wrangling uppgifter i det här avsnittet med hjälp av SQL:</span><span class="sxs-lookup"><span data-stu-id="b0de0-109">We describe hello following data wrangling tasks in this section using SQL:</span></span>

1. [<span data-ttu-id="b0de0-110">Datagranskning</span><span class="sxs-lookup"><span data-stu-id="b0de0-110">Data Exploration</span></span>](#sql-dataexploration)
2. [<span data-ttu-id="b0de0-111">Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="b0de0-111">Feature Generation</span></span>](#sql-featuregen)

### <span data-ttu-id="b0de0-112"><a name="sql-dataexploration"></a>Datagranskning</span><span class="sxs-lookup"><span data-stu-id="b0de0-112"><a name="sql-dataexploration"></a>Data Exploration</span></span>
<span data-ttu-id="b0de0-113">Här följer några exempel SQL-skript som kan använda tooexplore datalager i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="b0de0-113">Here are a few sample SQL scripts that can be used tooexplore data stores in SQL Server.</span></span>

> [!NOTE]
> <span data-ttu-id="b0de0-114">En praktisk t.ex, du kan använda hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) och finns toohello IPNB med titeln [NYC Data wrangling IPython anteckningsboken och SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) för en slutpunkt till slutpunkt-hanteringspaketen.</span><span class="sxs-lookup"><span data-stu-id="b0de0-114">For a practical example, you can use hello [NYC Taxi dataset](http://www.andresmh.com/nyctaxitrips/) and refer toohello IPNB titled [NYC Data wrangling using IPython Notebook and SQL Server](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-sql-walkthrough.ipynb) for an end-to-end walk-through.</span></span>
> 
> 

1. <span data-ttu-id="b0de0-115">Hämta hello antal observationer per dag</span><span class="sxs-lookup"><span data-stu-id="b0de0-115">Get hello count of observations per day</span></span>
   
    `SELECT CONVERT(date, <date_columnname>) as date, count(*) as c from <tablename> group by CONVERT(date, <date_columnname>)` 
2. <span data-ttu-id="b0de0-116">Hämta hello nivåer i en kategoriska kolumn</span><span class="sxs-lookup"><span data-stu-id="b0de0-116">Get hello levels in a categorical column</span></span>
   
    `select  distinct <column_name> from <databasename>`
3. <span data-ttu-id="b0de0-117">Hämta hello antalet nivåer i kombination med två kategoriska kolumner</span><span class="sxs-lookup"><span data-stu-id="b0de0-117">Get hello number of levels in combination of two categorical columns</span></span> 
   
    `select <column_a>, <column_b>,count(*) from <tablename> group by <column_a>, <column_b>`
4. <span data-ttu-id="b0de0-118">Hämta hello distribution för numeriska kolumner</span><span class="sxs-lookup"><span data-stu-id="b0de0-118">Get hello distribution for numerical columns</span></span>
   
    `select <column_name>, count(*) from <tablename> group by <column_name>`

### <span data-ttu-id="b0de0-119"><a name="sql-featuregen"></a>Funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="b0de0-119"><a name="sql-featuregen"></a>Feature Generation</span></span>
<span data-ttu-id="b0de0-120">I det här avsnittet beskrivs olika sätt att skapa funktioner med hjälp av SQL:</span><span class="sxs-lookup"><span data-stu-id="b0de0-120">In this section, we describe ways of generating features using SQL:</span></span>  

1. [<span data-ttu-id="b0de0-121">Antal baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="b0de0-121">Count based Feature Generation</span></span>](#sql-countfeature)
2. [<span data-ttu-id="b0de0-122">Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="b0de0-122">Binning Feature Generation</span></span>](#sql-binningfeature)
3. [<span data-ttu-id="b0de0-123">Lansera hello funktioner från en enda kolumn</span><span class="sxs-lookup"><span data-stu-id="b0de0-123">Rolling out hello features from a single column</span></span>](#sql-featurerollout)

> [!NOTE]
> <span data-ttu-id="b0de0-124">När du skapar ytterligare funktioner, kan du lägga till dem som kolumner toohello befintlig tabell eller skapa en ny tabell med hello ytterligare funktioner och primärnyckel som kan förenas med hello ursprungliga tabellen.</span><span class="sxs-lookup"><span data-stu-id="b0de0-124">Once you generate additional features, you can either add them as columns toohello existing table or create a new table with hello additional features and primary key, that can be joined with hello original table.</span></span> 
> 
> 

### <span data-ttu-id="b0de0-125"><a name="sql-countfeature"></a>Antal baserat funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="b0de0-125"><a name="sql-countfeature"></a>Count based Feature Generation</span></span>
<span data-ttu-id="b0de0-126">hello visar följande exempel två sätt att generera antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="b0de0-126">hello following examples demonstrate two ways of generating count features.</span></span> <span data-ttu-id="b0de0-127">hello första metoden använder Villkorlig summering och hello andra metoden använder hello uttrycket 'where'.</span><span class="sxs-lookup"><span data-stu-id="b0de0-127">hello first method uses conditional sum and hello second method uses hello 'where' clause.</span></span> <span data-ttu-id="b0de0-128">De kan sedan kopplas med hello ursprungliga tabellen (med primärnyckelkolumnerna) toohave antal funktioner tillsammans med hello ursprungliga data.</span><span class="sxs-lookup"><span data-stu-id="b0de0-128">These can then be joined with hello original table (using primary key columns) toohave count features alongside hello original data.</span></span>

    select <column_name1>,<column_name2>,<column_name3>, COUNT(*) as Count_Features from <tablename> group by <column_name1>,<column_name2>,<column_name3> 

    select <column_name1>,<column_name2> , sum(1) as Count_Features from <tablename> 
    where <column_name3> = '<some_value>' group by <column_name1>,<column_name2> 

### <span data-ttu-id="b0de0-129"><a name="sql-binningfeature"></a>Diskretisering funktionen Generation</span><span class="sxs-lookup"><span data-stu-id="b0de0-129"><a name="sql-binningfeature"></a>Binning Feature Generation</span></span>
<span data-ttu-id="b0de0-130">hello följande exempel visas hur toogenerate binned funktioner av diskretisering (med fem lagerplatser) en numerisk kolumn som kan användas som en funktion i stället:</span><span class="sxs-lookup"><span data-stu-id="b0de0-130">hello following example shows how toogenerate binned features by binning (using five bins) a numerical column that can be used as a feature instead:</span></span>

    `SELECT <column_name>, NTILE(5) OVER (ORDER BY <column_name>) AS BinNumber from <tablename>`


### <span data-ttu-id="b0de0-131"><a name="sql-featurerollout"></a>Lansera hello funktioner från en enda kolumn</span><span class="sxs-lookup"><span data-stu-id="b0de0-131"><a name="sql-featurerollout"></a>Rolling out hello features from a single column</span></span>
<span data-ttu-id="b0de0-132">I det här avsnittet visar vi hur tooroll i en kolumn i en tabell toogenerate ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="b0de0-132">In this section, we demonstrate how tooroll out a single column in a table toogenerate additional features.</span></span> <span data-ttu-id="b0de0-133">hello exemplet förutsätter att det finns en latitud och longitud kolumn i hello tabell från vilken du försöker toogenerate funktioner.</span><span class="sxs-lookup"><span data-stu-id="b0de0-133">hello example assumes that there is a latitude or longitude column in hello table from which you are trying toogenerate features.</span></span>

<span data-ttu-id="b0de0-134">Här är en kort introduktion om latitud/longitud platsdata (resurstilldelas från stackoverflow [hur toomeasure hello riktighet latitud och longitud?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span><span class="sxs-lookup"><span data-stu-id="b0de0-134">Here is a brief primer on latitude/longitude location data (resourced from stackoverflow [How toomeasure hello accuracy of latitude and longitude?](http://gis.stackexchange.com/questions/8650/how-to-measure-the-accuracy-of-latitude-and-longitude)).</span></span> <span data-ttu-id="b0de0-135">Detta är användbart toounderstand innan featurizing hello Platsfältet:</span><span class="sxs-lookup"><span data-stu-id="b0de0-135">This is useful toounderstand before featurizing hello location field:</span></span>

* <span data-ttu-id="b0de0-136">hello logga talar om för oss om vi är norr eller Syd, Öst eller Väst på hello jordglob.</span><span class="sxs-lookup"><span data-stu-id="b0de0-136">hello sign tells us whether we are north or south, east or west on hello globe.</span></span>
* <span data-ttu-id="b0de0-137">Ett annat värde än noll hundratals siffra talar om för oss att vi använder longitud inte latitud!</span><span class="sxs-lookup"><span data-stu-id="b0de0-137">A nonzero hundreds digit tells us that we're using longitude, not latitude!</span></span>
* <span data-ttu-id="b0de0-138">hello flera siffror ger en position tooabout 1 000 kilometer.</span><span class="sxs-lookup"><span data-stu-id="b0de0-138">hello tens digit gives a position tooabout 1,000 kilometers.</span></span> <span data-ttu-id="b0de0-139">Det ger oss användbar information om vilka kontinent eller oceanen vi finns på.</span><span class="sxs-lookup"><span data-stu-id="b0de0-139">It gives us useful information about what continent or ocean we are on.</span></span>
* <span data-ttu-id="b0de0-140">hello enheter siffra (en decimal grad) ger en position upp too111 kilometer (60 sjömil, om 69 miles).</span><span class="sxs-lookup"><span data-stu-id="b0de0-140">hello units digit (one decimal degree) gives a position up too111 kilometers (60 nautical miles, about 69 miles).</span></span> <span data-ttu-id="b0de0-141">Det kan berätta ungefär vad eller det stora land vi finns i.</span><span class="sxs-lookup"><span data-stu-id="b0de0-141">It can tell us roughly what large state or country we are in.</span></span>
* <span data-ttu-id="b0de0-142">hello första decimaltecknet är värda too11.1 km: den kan skilja hello positionen för en stor ort från en närliggande stora stad.</span><span class="sxs-lookup"><span data-stu-id="b0de0-142">hello first decimal place is worth up too11.1 km: it can distinguish hello position of one large city from a neighboring large city.</span></span>
* <span data-ttu-id="b0de0-143">hello andra decimaltecknet är värda too1.1 km: den kan skilja en village från hello nästa.</span><span class="sxs-lookup"><span data-stu-id="b0de0-143">hello second decimal place is worth up too1.1 km: it can separate one village from hello next.</span></span>
* <span data-ttu-id="b0de0-144">hello tredje decimaltecknet är värda upp too110 m: den kan identifiera en stor jordbruket institutionella plats.</span><span class="sxs-lookup"><span data-stu-id="b0de0-144">hello third decimal place is worth up too110 m: it can identify a large agricultural field or institutional campus.</span></span>
* <span data-ttu-id="b0de0-145">hello fjärde decimaltecknet är värda upp too11 m: mark kan identifiera.</span><span class="sxs-lookup"><span data-stu-id="b0de0-145">hello fourth decimal place is worth up too11 m: it can identify a parcel of land.</span></span> <span data-ttu-id="b0de0-146">Det är jämförbar toohello vanliga korrektheten i en okorrigerade GPS-enhet utan störningar.</span><span class="sxs-lookup"><span data-stu-id="b0de0-146">It is comparable toohello typical accuracy of an uncorrected GPS unit with no interference.</span></span>
* <span data-ttu-id="b0de0-147">hello femte decimaltecknet är värda upp too1.1 m: den särskiljer träd från varandra.</span><span class="sxs-lookup"><span data-stu-id="b0de0-147">hello fifth decimal place is worth up too1.1 m: it distinguishes trees from each other.</span></span> <span data-ttu-id="b0de0-148">Precision toothis nivå med kommersiella GPS-enheter kan endast uppnås med differentiell korrigering.</span><span class="sxs-lookup"><span data-stu-id="b0de0-148">Accuracy toothis level with commercial GPS units can only be achieved with differential correction.</span></span>
* <span data-ttu-id="b0de0-149">hello sjätte decimaltecknet är värda upp too0.11 m: kan du använda den för att utforma strukturer i detalj för att utforma landskap, skapa vägar.</span><span class="sxs-lookup"><span data-stu-id="b0de0-149">hello sixth decimal place is worth up too0.11 m: you can use this for laying out structures in detail, for designing landscapes, building roads.</span></span> <span data-ttu-id="b0de0-150">Det bör vara mer än tillräckligt bra för spårning av glaciers och floder.</span><span class="sxs-lookup"><span data-stu-id="b0de0-150">It should be more than good enough for tracking movements of glaciers and rivers.</span></span> <span data-ttu-id="b0de0-151">Detta kan uppnås genom att göra painstaking åtgärder med GPS, till exempel differentially korrigerade GPS.</span><span class="sxs-lookup"><span data-stu-id="b0de0-151">This can be achieved by taking painstaking measures with GPS, such as differentially corrected GPS.</span></span>

<span data-ttu-id="b0de0-152">hello platsinformation kan vara featurized på följande sätt att skilja ut region, plats och information om ort.</span><span class="sxs-lookup"><span data-stu-id="b0de0-152">hello location information can be featurized as follows, separating out region, location, and city information.</span></span> <span data-ttu-id="b0de0-153">Observera att du kan också anropa en REST-slutpunkt, till exempel Bing Maps API finns på [hitta en plats med en punkt](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello region/distrikt information.</span><span class="sxs-lookup"><span data-stu-id="b0de0-153">Note that you can also call a REST end point such as Bing Maps API available at [Find a Location by Point](https://msdn.microsoft.com/library/ff701710.aspx) tooget hello region/district information.</span></span>

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

<span data-ttu-id="b0de0-154">Dessa funktioner för plats-baserad kan vara mer används toogenerate ytterligare antal funktioner som tidigare beskrivits.</span><span class="sxs-lookup"><span data-stu-id="b0de0-154">These location-based features can be further used toogenerate additional count features as described earlier.</span></span> 

> [!TIP]
> <span data-ttu-id="b0de0-155">Du kan via programmering Infoga hello poster med hjälp av ditt språk.</span><span class="sxs-lookup"><span data-stu-id="b0de0-155">You can programmatically insert hello records using your language of choice.</span></span> <span data-ttu-id="b0de0-156">Du kanske måste tooinsert hello data i segment tooimprove skrivåtgärder effektivitet (ett exempel på hur toodo denna med pyodbc finns [A HelloWorld exempel tooaccess SQLServer med python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span><span class="sxs-lookup"><span data-stu-id="b0de0-156">You may need tooinsert hello data in chunks tooimprove write efficiency (for an example of how toodo this using pyodbc, see [A HelloWorld sample tooaccess SQLServer with python](https://code.google.com/p/pypyodbc/wiki/A_HelloWorld_sample_to_access_mssql_with_python)).</span></span> <span data-ttu-id="b0de0-157">Ett annat alternativ är tooinsert data i hello-databas med hjälp av hello [BCP-verktyget](https://msdn.microsoft.com/library/ms162802.aspx).</span><span class="sxs-lookup"><span data-stu-id="b0de0-157">Another alternative is tooinsert data in hello database using hello [BCP utility](https://msdn.microsoft.com/library/ms162802.aspx).</span></span>
> 
> 

### <span data-ttu-id="b0de0-158"><a name="sql-aml"></a>Ansluta tooAzure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="b0de0-158"><a name="sql-aml"></a>Connecting tooAzure Machine Learning</span></span>
<span data-ttu-id="b0de0-159">hello nya funktionen kan läggas till som en befintlig tabell tooan för kolumn eller lagras i en ny tabell och kopplas till hello ursprungliga tabellen för machine learning.</span><span class="sxs-lookup"><span data-stu-id="b0de0-159">hello newly generated feature can be added as a column tooan existing table or stored in a new table and joined with hello original table for machine learning.</span></span> <span data-ttu-id="b0de0-160">Funktioner kan genereras eller komma åt om redan har skapats med hjälp av hello [importera Data] [ import-data] modul i Azure Machine Learning enligt nedan:</span><span class="sxs-lookup"><span data-stu-id="b0de0-160">Features can be generated or accessed if already created, using hello [Import Data][import-data] module in Azure Machine Learning as shown below:</span></span>

![azureml-läsare][1] 

## <span data-ttu-id="b0de0-162"><a name="python"></a>Med hjälp av programmeringsspråk som Python</span><span class="sxs-lookup"><span data-stu-id="b0de0-162"><a name="python"></a>Using a programming language like Python</span></span>
<span data-ttu-id="b0de0-163">Med hjälp av Python tooexplore data och generera funktioner när hello data finns i SQL Server är liknande tooprocessing data i Azure blob använder Python enligt beskrivningen i [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="b0de0-163">Using Python tooexplore data and generate features when hello data is in SQL Server is similar tooprocessing data in Azure blob using Python as documented in [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span> <span data-ttu-id="b0de0-164">hello data måste toobe läses in från hello databasen i ett pandas data och sedan behandlas vidare.</span><span class="sxs-lookup"><span data-stu-id="b0de0-164">hello data needs toobe loaded from hello database into a pandas data frame and then can be processed further.</span></span> <span data-ttu-id="b0de0-165">Vi dokumentera hello process för att ansluta databasen toohello och hello data läses in i hello data ram i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b0de0-165">We document hello process of connecting toohello database and loading hello data into hello data frame in this section.</span></span>

<span data-ttu-id="b0de0-166">hello följande Anslutningssträngens format kan vara används tooconnect tooa SQL Server-databas från Python med pyodbc (Ersätt servername, dbname, användarnamn och lösenord med dina specifika värden):</span><span class="sxs-lookup"><span data-stu-id="b0de0-166">hello following connection string format can be used tooconnect tooa SQL Server database from Python using pyodbc (replace servername, dbname, username, and password with your specific values):</span></span>

    #Set up hello SQL Azure connection
    import pyodbc    
    conn = pyodbc.connect('DRIVER={SQL Server};SERVER=<servername>;DATABASE=<dbname>;UID=<username>;PWD=<password>')

<span data-ttu-id="b0de0-167">Hej [Pandas biblioteket](http://pandas.pydata.org/) i Python ger en omfattande uppsättning datastrukturer och verktyg för analys av data till datamanipulering för Python-programmering.</span><span class="sxs-lookup"><span data-stu-id="b0de0-167">hello [Pandas library](http://pandas.pydata.org/) in Python provides a rich set of data structures and data analysis tools for data manipulation for Python programming.</span></span> <span data-ttu-id="b0de0-168">hello koden nedan läser hello resultatet som returneras från en SQL Server-databas till en Pandas data ram:</span><span class="sxs-lookup"><span data-stu-id="b0de0-168">hello code below reads hello results returned from a SQL Server database into a Pandas data frame:</span></span>

    # Query database and load hello returned results in pandas data frame
    data_frame = pd.read_sql('''select <columnname1>, <cloumnname2>... from <tablename>''', conn)

<span data-ttu-id="b0de0-169">Nu kan du arbeta med hello Pandas data ram som beskrivs i artikeln hello [processen Azure Blob-data i datavetenskap miljön](machine-learning-data-science-process-data-blob.md).</span><span class="sxs-lookup"><span data-stu-id="b0de0-169">Now you can work with hello Pandas data frame as covered in hello article [Process Azure Blob data in your data science environment](machine-learning-data-science-process-data-blob.md).</span></span>

## <a name="azure-data-science-in-action-example"></a><span data-ttu-id="b0de0-170">Azure datavetenskap i åtgärden exempel</span><span class="sxs-lookup"><span data-stu-id="b0de0-170">Azure Data Science in Action Example</span></span>
<span data-ttu-id="b0de0-171">En slutpunkt till slutpunkt genomgången exempel på hello Azure vetenskap av data med hjälp av en offentlig dataset finns [Azure datavetenskap hur fungerar](machine-learning-data-science-process-sql-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="b0de0-171">For an end-to-end walkthrough example of hello Azure Data Science Process using a public dataset, see [Azure Data Science Process in Action](machine-learning-data-science-process-sql-walkthrough.md).</span></span>

[1]: ./media/machine-learning-data-science-process-sql-server-virtual-machine/reader_db_featurizedinput.png


<!-- Module References -->
[import-data]: https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/

