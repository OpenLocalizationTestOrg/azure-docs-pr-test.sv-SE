---
title: "Skalbar datavetenskap med Azure Data Lake: en slutpunkt till slutpunkt genomgången | Microsoft Docs"
description: "Hur toouse Azure Data Lake toodo data från kartläggning av naturresurser och binär klassificering av åtgärder på en datamängd."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 91a8207f-1e57-4570-b7fc-7c5fa858ffeb
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/30/2017
ms.author: bradsev;weig
ms.openlocfilehash: 8b05457ae7045a7aaed350a7502469f2247161e0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="scalable-data-science-with-azure-data-lake-an-end-to-end-walkthrough"></a><span data-ttu-id="18937-103">Skalbar datavetenskap med Azure Data Lake: en genomgång för slutpunkt till slutpunkt</span><span class="sxs-lookup"><span data-stu-id="18937-103">Scalable Data Science with Azure Data Lake: An end-to-end Walkthrough</span></span>
<span data-ttu-id="18937-104">Den här genomgången visar hur toouse Azure Data Lake toodo datagranskning och binär klassificering uppgifter på ett sampel från hello NYC Taxitransport resa och avgiften dataset toopredict huruvida ett tips utgår genom en avgiften.</span><span class="sxs-lookup"><span data-stu-id="18937-104">This walkthrough shows how toouse Azure Data Lake toodo data exploration and binary classification tasks on a sample of hello NYC taxi trip and fare dataset toopredict whether or not a tip will be paid by a fare.</span></span> <span data-ttu-id="18937-105">Den vägleder dig genom stegen för hello av hello [Team datavetenskap Process](http://aka.ms/datascienceprocess), slutpunkt-till-slutpunkt, från data förvärv toomodel utbildning och toohello distribution av en webbtjänst som publicerar hello modellen.</span><span class="sxs-lookup"><span data-stu-id="18937-105">It walks you through hello steps of hello [Team Data Science Process](http://aka.ms/datascienceprocess), end-to-end, from data acquisition toomodel training, and then toohello deployment of a web service that publishes hello model.</span></span>

### <a name="azure-data-lake-analytics"></a><span data-ttu-id="18937-106">Azure Data Lake Analytics</span><span class="sxs-lookup"><span data-stu-id="18937-106">Azure Data Lake Analytics</span></span>
<span data-ttu-id="18937-107">Hej [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) har alla hello funktioner krävs toomake det enkelt för data forskare toostore data för alla storlek, form och hastighet och tooconduct databearbetning, avancerade analyser och machine learning modellering med hög skalbarhet på ett kostnadseffektivt sätt.</span><span class="sxs-lookup"><span data-stu-id="18937-107">hello [Microsoft Azure Data Lake](https://azure.microsoft.com/solutions/data-lake/) has all hello capabilities required toomake it easy for data scientists toostore data of any size, shape and speed, and tooconduct data processing, advanced analytics, and machine learning modeling with high scalability in a cost-effective way.</span></span>   <span data-ttu-id="18937-108">Du betalar per projekt-basis, bara när data bearbetas faktiskt.</span><span class="sxs-lookup"><span data-stu-id="18937-108">You pay on a per-job basis, only when data is actually being processed.</span></span> <span data-ttu-id="18937-109">Azure Data Lake Analytics innehåller U-SQL, ett språk som överlappande hello deklarativa kompetensen hos SQL med hello expressiva kraften i C# tooprovide skalbara distribuerade frågan kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="18937-109">Azure Data Lake Analytics includes U-SQL, a language that blends hello declarative nature of SQL with hello expressive power of C# tooprovide scalable distributed query capability.</span></span> <span data-ttu-id="18937-110">Du kan använda tooprocess Ostrukturerade data genom att använda schemat på läsa, infoga egen kod och användardefinierade funktioner (UDF) och innehåller utökningsbarhet tooenable bra metataggkontroll kontroll över hur tooexecute i större skala.</span><span class="sxs-lookup"><span data-stu-id="18937-110">It enables you tooprocess unstructured data by applying schema on read, insert custom logic and user defined functions (UDFs), and includes extensibility tooenable fine grained control over how tooexecute at scale.</span></span> <span data-ttu-id="18937-111">toolearn mer om hello designfilosofin bakom U-SQL finns [Visual Studio blogginlägget](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span><span class="sxs-lookup"><span data-stu-id="18937-111">toolearn more about hello design philosophy behind U-SQL, see [Visual Studio blog post](https://blogs.msdn.microsoft.com/visualstudio/2015/09/28/introducing-u-sql-a-language-that-makes-big-data-processing-easy/).</span></span>

<span data-ttu-id="18937-112">Datasjöanalys är också en viktig del av Cortana Analytics Suite och fungerar med Azure SQL Data Warehouse, Power BI och Data Factory.</span><span class="sxs-lookup"><span data-stu-id="18937-112">Data Lake Analytics is also a key part of Cortana Analytics Suite and works with Azure SQL Data Warehouse, Power BI, and Data Factory.</span></span> <span data-ttu-id="18937-113">Detta ger dig en fullständigt moln stordata och avancerade analysplattform.</span><span class="sxs-lookup"><span data-stu-id="18937-113">This gives you a complete cloud big data and advanced analytics platform.</span></span>

<span data-ttu-id="18937-114">Den här genomgången börjar genom att beskriva hello krav och resurser som behövs toocomplete hello uppgifter med Data Lake Analytics som bildar hello av vetenskapliga data och hur tooinstall dem.</span><span class="sxs-lookup"><span data-stu-id="18937-114">This walkthrough begins by describing hello prerequisites and resources that are needed toocomplete hello tasks with Data Lake Analytics that form hello data science process and how tooinstall them.</span></span> <span data-ttu-id="18937-115">Därefter beskrivs hello databearbetning steg med U-SQL och avslutar genom att visa hur toouse Python och Hive med Azure Machine Learning Studio toobuild och distribuera förutsägelsemodeller hello.</span><span class="sxs-lookup"><span data-stu-id="18937-115">Then it outlines hello data processing steps using U-SQL and concludes by showing how toouse Python and Hive with Azure Machine Learning Studio toobuild and deploy hello predictive models.</span></span> 

### <a name="u-sql-and-visual-studio"></a><span data-ttu-id="18937-116">U-SQL och Visual Studio</span><span class="sxs-lookup"><span data-stu-id="18937-116">U-SQL and Visual Studio</span></span>
<span data-ttu-id="18937-117">Den här genomgången rekommenderar att du använder Visual Studio tooedit U-SQL-skript tooprocess hello dataset.</span><span class="sxs-lookup"><span data-stu-id="18937-117">This walkthrough recommends using Visual Studio tooedit U-SQL scripts tooprocess hello dataset.</span></span> <span data-ttu-id="18937-118">hello U-SQL-skript som beskrivs här och anges i en separat fil.</span><span class="sxs-lookup"><span data-stu-id="18937-118">hello U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="18937-119">hello processen omfattar vill föra in, utforska och hello datasampling.</span><span class="sxs-lookup"><span data-stu-id="18937-119">hello process includes ingesting, exploring, and sampling hello data.</span></span> <span data-ttu-id="18937-120">Den visar även hur toorun U-SQL skripta jobbet från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18937-120">It also shows how toorun a U-SQL scripted job from hello Azure portal.</span></span> <span data-ttu-id="18937-121">Hive-tabeller skapas för hello data i en associerad HDInsight-kluster toofacilitate hello byggnad och distribution av en binär klassificering modell i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="18937-121">Hive tables are created for hello data in an associated HDInsight cluster toofacilitate hello building and deployment of a binary classification model in Azure Machine Learning Studio.</span></span>  

### <a name="python"></a><span data-ttu-id="18937-122">Python</span><span class="sxs-lookup"><span data-stu-id="18937-122">Python</span></span>
<span data-ttu-id="18937-123">Den här genomgången innehåller också ett avsnitt som visar hur toobuild och distribuera en förutsägelsemodell med Azure Machine Learning Studio Python.</span><span class="sxs-lookup"><span data-stu-id="18937-123">This walkthrough also contains a section that shows how toobuild and deploy a predictive model using Python with Azure Machine Learning Studio.</span></span>  <span data-ttu-id="18937-124">Vi ger en Jupyter-anteckningsbok med hello Python-skript för de här stegen i den här processen.</span><span class="sxs-lookup"><span data-stu-id="18937-124">We provide a Jupyter notebook with hello Python scripts for these steps in this process.</span></span> <span data-ttu-id="18937-125">hello anteckningsboken innehåller koden för vissa ytterligare funktionen engineering steg och modeller konstruktion som multiklass-baserad klassificering och regressionen modeling dessutom toohello binär klassificering modellen som beskrivs här.</span><span class="sxs-lookup"><span data-stu-id="18937-125">hello notebook includes code for some additional feature engineering steps and models construction such as multiclass classification and regression modeling in addition toohello binary classification model outlined here.</span></span> <span data-ttu-id="18937-126">hello regression aktivitet är toopredict hello hello tips baserat på andra tips funktioner.</span><span class="sxs-lookup"><span data-stu-id="18937-126">hello regression task is toopredict hello amount of hello tip based on other tip features.</span></span> 

### <a name="azure-machine-learning"></a><span data-ttu-id="18937-127">Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="18937-127">Azure Machine Learning</span></span>
<span data-ttu-id="18937-128">Azure Machine Learning Studio är används toobuild och distribuera förutsägelsemodeller hello.</span><span class="sxs-lookup"><span data-stu-id="18937-128">Azure Machine Learning Studio is used toobuild and deploy hello predictive models.</span></span> <span data-ttu-id="18937-129">Detta görs med hjälp av två sätt: först med Python-skript och sedan Hive-tabeller i ett kluster i HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="18937-129">This is done using two approaches: first with Python scripts and then with Hive tables on an HDInsight (Hadoop) cluster.</span></span>

### <a name="scripts"></a><span data-ttu-id="18937-130">Skript</span><span class="sxs-lookup"><span data-stu-id="18937-130">Scripts</span></span>
<span data-ttu-id="18937-131">Endast hello huvudsakliga steg beskrivs i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="18937-131">Only hello principal steps are outlined in this walkthrough.</span></span> <span data-ttu-id="18937-132">Du kan hämta hello fullständig **U-SQL-skript** och **Jupyter-anteckningsbok** från [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="18937-132">You can download hello full **U-SQL script** and **Jupyter Notebook** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="18937-133">Krav</span><span class="sxs-lookup"><span data-stu-id="18937-133">Prerequisites</span></span>
<span data-ttu-id="18937-134">Innan du börjar dessa avsnitt, måste du ha hello följande:</span><span class="sxs-lookup"><span data-stu-id="18937-134">Before you begin these topics, you must have hello following:</span></span>

* <span data-ttu-id="18937-135">En Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="18937-135">An Azure subscription.</span></span> <span data-ttu-id="18937-136">Om du inte redan har en, se [hämta kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span><span class="sxs-lookup"><span data-stu-id="18937-136">If you do not already have one, see [Get Azure free trial](https://azure.microsoft.com/documentation/videos/get-azure-free-trial-for-testing-hadoop-in-hdinsight/).</span></span>
* <span data-ttu-id="18937-137">(Rekommenderas) Visual Studio 2013 eller senare.</span><span class="sxs-lookup"><span data-stu-id="18937-137">[Recommended] Visual Studio 2013 or later.</span></span> <span data-ttu-id="18937-138">Om du inte redan har en av dessa versioner är installerad, kan du hämta en kostnadsfri Community-version från [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span><span class="sxs-lookup"><span data-stu-id="18937-138">If you do not already have one of these versions installed, you can download a free Community version from [Visual Studio Community](https://www.visualstudio.com/vs/community/).</span></span>

> [!NOTE]
> <span data-ttu-id="18937-139">Du kan också använda hello Azure Portal toosubmit Azure Data Lake frågor i stället för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18937-139">Instead of Visual Studio, you can also use hello Azure Portal toosubmit Azure Data Lake queries.</span></span> <span data-ttu-id="18937-140">Vi ger instruktioner för hur toodo så både med Visual Studio och på hello portal hello under rubriken **bearbeta data med U-SQL**.</span><span class="sxs-lookup"><span data-stu-id="18937-140">We will provide instructions on how toodo so both with Visual Studio and on hello portal in hello section titled **Process data with U-SQL**.</span></span> 
> 
> 


## <a name="prepare-data-science-environment-for-azure-data-lake"></a><span data-ttu-id="18937-141">Förbered datavetenskap miljö för Azure Data Lake</span><span class="sxs-lookup"><span data-stu-id="18937-141">Prepare data science environment for Azure Data Lake</span></span>
<span data-ttu-id="18937-142">tooprepare hello datavetenskap miljö för den här genomgången skapa hello följande resurser:</span><span class="sxs-lookup"><span data-stu-id="18937-142">tooprepare hello data science environment for this walkthrough, create hello following resources:</span></span>

* <span data-ttu-id="18937-143">Azure Data Lake Store (ADLS)</span><span class="sxs-lookup"><span data-stu-id="18937-143">Azure Data Lake Store (ADLS)</span></span> 
* <span data-ttu-id="18937-144">Azure Data Lake Analytics (ADLA)</span><span class="sxs-lookup"><span data-stu-id="18937-144">Azure Data Lake Analytics (ADLA)</span></span>
* <span data-ttu-id="18937-145">Azure Blob storage-konto</span><span class="sxs-lookup"><span data-stu-id="18937-145">Azure Blob storage account</span></span>
* <span data-ttu-id="18937-146">Azure Machine Learning Studio-konto</span><span class="sxs-lookup"><span data-stu-id="18937-146">Azure Machine Learning Studio account</span></span>
* <span data-ttu-id="18937-147">Azure Data Lake-verktyg för Visual Studio (rekommenderas)</span><span class="sxs-lookup"><span data-stu-id="18937-147">Azure Data Lake Tools for Visual Studio (Recommended)</span></span>

<span data-ttu-id="18937-148">Det här avsnittet innehåller instruktioner om hur toocreate av dessa resurser.</span><span class="sxs-lookup"><span data-stu-id="18937-148">This section provides instructions on how toocreate each of these resources.</span></span> <span data-ttu-id="18937-149">Om du väljer toouse Hive-tabeller med Azure Machine Learning, i stället för Python, toobuild en modell, måste du också tooprovision ett kluster i HDInsight (Hadoop).</span><span class="sxs-lookup"><span data-stu-id="18937-149">If you choose toouse Hive tables with Azure Machine Learning, instead of Python, toobuild a model,you will also need tooprovision an HDInsight (Hadoop) cluster.</span></span> <span data-ttu-id="18937-150">Den här alternativa proceduren i beskrivs i hello lämpligt avsnitt nedan.</span><span class="sxs-lookup"><span data-stu-id="18937-150">This alternative procedure in described in hello appropriate section below.</span></span>


> [!NOTE]
> <span data-ttu-id="18937-151">Hej **Azure Data Lake Store** skapas antingen separat eller när du skapar hello **Azure Data Lake Analytics** som hello standardlagring.</span><span class="sxs-lookup"><span data-stu-id="18937-151">hello **Azure Data Lake Store** can be created either separately or when you create hello **Azure Data Lake Analytics** as hello default storage.</span></span> <span data-ttu-id="18937-152">Refererar till instruktioner för att skapa dessa resurser separat nedan, men hello Data Lake storage-konto behöver inte skapas separat.</span><span class="sxs-lookup"><span data-stu-id="18937-152">Instructions are referenced for creating each of these resources separately below, but hello Data Lake storage account need not be created separately.</span></span>
>
> 

### <a name="create-an-azure-data-lake-store"></a><span data-ttu-id="18937-153">Skapa ett Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="18937-153">Create an Azure Data Lake Store</span></span>


<span data-ttu-id="18937-154">Skapa ett ADLS från hello [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="18937-154">Create an ADLS from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="18937-155">Mer information finns i [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="18937-155">For details, see [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="18937-156">Vara säker på att tooset upp hello kluster-AAD-identitet i hello **DataSource** bladet för hello **valfri konfiguration** bladet beskrivs det.</span><span class="sxs-lookup"><span data-stu-id="18937-156">Be sure tooset up hello Cluster AAD Identity in hello **DataSource** blade of hello **Optional Configuration** blade described there.</span></span> 

 ![3](./media/machine-learning-data-science-process-data-lake-walkthrough/3-create-ADLS.PNG)

### <a name="create-an-azure-data-lake-analytics-account"></a><span data-ttu-id="18937-158">Skapa ett Azure Data Lake Analytics-konto</span><span class="sxs-lookup"><span data-stu-id="18937-158">Create an Azure Data Lake Analytics account</span></span>
<span data-ttu-id="18937-159">Skapa ett konto för ADLA från hello [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="18937-159">Create an ADLA account from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="18937-160">Mer information finns i [Självstudier: Kom igång med Azure Data Lake Analytics med hjälp av Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span><span class="sxs-lookup"><span data-stu-id="18937-160">For details, see [Tutorial: get started with Azure Data Lake Analytics using Azure Portal](../data-lake-analytics/data-lake-analytics-get-started-portal.md).</span></span> 

 ![4](./media/machine-learning-data-science-process-data-lake-walkthrough/4-create-ADLA-new.PNG)

### <a name="create-an-azure-blob-storage-account"></a><span data-ttu-id="18937-162">Skapa ett Azure Blob storage-konto</span><span class="sxs-lookup"><span data-stu-id="18937-162">Create an Azure Blob storage account</span></span>
<span data-ttu-id="18937-163">Skapa ett Azure Blob storage-konto från hello [Azure Portal](http://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="18937-163">Create an Azure Blob storage account from hello [Azure Portal](http://portal.azure.com).</span></span> <span data-ttu-id="18937-164">Mer information finns i hello skapa ett lagringskonto i avsnittet [om Azure storage-konton](../storage/common/storage-create-storage-account.md).</span><span class="sxs-lookup"><span data-stu-id="18937-164">For details, see hello Create a storage account section in [About Azure storage accounts](../storage/common/storage-create-storage-account.md).</span></span>

 ![5](./media/machine-learning-data-science-process-data-lake-walkthrough/5-Create-Azure-Blob.PNG)

### <a name="set-up-an-azure-machine-learning-studio-account"></a><span data-ttu-id="18937-166">Konfigurera ett konto i Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="18937-166">Set up an Azure Machine Learning Studio account</span></span>
<span data-ttu-id="18937-167">Logga in/till Azure Machine Learning Studio från hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) sidan.</span><span class="sxs-lookup"><span data-stu-id="18937-167">Sign up/into Azure Machine Learning Studio from hello [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) page.</span></span> <span data-ttu-id="18937-168">Klicka på hello **Kom igång nu** knappen och välj sedan en ”ledigt arbetsyta” eller ”Standard arbetsytan”.</span><span class="sxs-lookup"><span data-stu-id="18937-168">Click on hello **Get started now** button and then choose a "Free Workspace" or "Standard Workspace".</span></span> <span data-ttu-id="18937-169">När det här kommer du att kunna toocreate experiment i Azure ML Studio.</span><span class="sxs-lookup"><span data-stu-id="18937-169">After this you will be able toocreate experiments in Azure ML Studio.</span></span>  

### <a name="install-azure-data-lake-tools-recommended"></a><span data-ttu-id="18937-170">Installera Azure Data Lake-verktyg (rekommenderas)</span><span class="sxs-lookup"><span data-stu-id="18937-170">Install Azure Data Lake Tools [Recommended]</span></span>
<span data-ttu-id="18937-171">Installera Azure Data Lake-verktyg för din version av Visual Studio från [Azure Data Lake-verktyg för Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span><span class="sxs-lookup"><span data-stu-id="18937-171">Install Azure Data Lake Tools for your version of Visual Studio from [Azure Data Lake Tools for Visual Studio](https://www.microsoft.com/download/details.aspx?id=49504).</span></span>

 ![6](./media/machine-learning-data-science-process-data-lake-walkthrough/6-install-ADL-tools-VS.PNG)

<span data-ttu-id="18937-173">Öppna Visual Studio när hello installationen slutförs.</span><span class="sxs-lookup"><span data-stu-id="18937-173">After hello installation finishes successfully, open up Visual Studio.</span></span> <span data-ttu-id="18937-174">Du bör se Data Lake hello fliken hello menyn hello överst.</span><span class="sxs-lookup"><span data-stu-id="18937-174">You should see hello Data Lake tab hello menu at hello top.</span></span> <span data-ttu-id="18937-175">Azure-resurser ska visas i hello vänstra panelen när du loggar in på ditt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="18937-175">Your Azure resources should appear in hello left panel when you sign into your Azure account.</span></span>

 ![7](./media/machine-learning-data-science-process-data-lake-walkthrough/7-install-ADL-tools-VS-done.PNG)

## <a name="hello-nyc-taxi-trips-dataset"></a><span data-ttu-id="18937-177">hello NYC Taxi resor dataset</span><span class="sxs-lookup"><span data-stu-id="18937-177">hello NYC Taxi Trips dataset</span></span>
<span data-ttu-id="18937-178">hello uppgifter som vi använde här är en offentligt tillgängliga dataset--hello [NYC Taxi resor dataset](http://www.andresmh.com/nyctaxitrips/).</span><span class="sxs-lookup"><span data-stu-id="18937-178">hello data set we used here is a publicly available dataset -- hello [NYC Taxi Trips dataset](http://www.andresmh.com/nyctaxitrips/).</span></span> <span data-ttu-id="18937-179">hello NYC Taxi resa data består av cirka 20GB komprimerat CSV-filer (~ 48GB okomprimerade), registrera mer än 173 miljoner enskilda resor och hello priser för varje resa.</span><span class="sxs-lookup"><span data-stu-id="18937-179">hello NYC Taxi Trip data consists of about 20GB of compressed CSV files (~48GB uncompressed), recording more than 173 million individual trips and hello fares paid for each trip.</span></span> <span data-ttu-id="18937-180">Varje resa post omfattar hello hämtning och Samlingsbibliotek platser och gånger anonym hacka () körkortsnummer och hello medallion (taxi's unikt id) nummer.</span><span class="sxs-lookup"><span data-stu-id="18937-180">Each trip record includes hello pickup and drop-off locations and times, anonymized hack (driver's) license number, and hello medallion (taxi’s unique id) number.</span></span> <span data-ttu-id="18937-181">hello data omfattar alla resor hello år 2013 och finns i följande två datamängder för varje månad hello:</span><span class="sxs-lookup"><span data-stu-id="18937-181">hello data covers all trips in hello year 2013 and is provided in hello following two datasets for each month:</span></span>

* <span data-ttu-id="18937-182">hello 'trip_data' CSV innehåller resa information, till exempel antal passagerare, hämtning och dropoff punkter, resa varaktighet och resa längd.</span><span class="sxs-lookup"><span data-stu-id="18937-182">hello 'trip_data' CSV contains trip details, such as number of passengers, pickup and dropoff points, trip duration, and trip length.</span></span> <span data-ttu-id="18937-183">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="18937-183">Here are a few sample records:</span></span>
  
       medallion,hack_license,vendor_id,rate_code,store_and_fwd_flag,pickup_datetime,dropoff_datetime,passenger_count, trip_time_in_secs,trip_distance,pickup_longitude,pickup_latitude,dropoff_longitude,dropoff_latitude
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,1,N,2013-01-01 15:11:48,2013-01-01 15:18:10,4,382,1.00,-73.978165,40.757977,-73.989838,40.751171
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-06 00:18:35,2013-01-06 00:22:54,1,259,1.50,-74.006683,40.731781,-73.994499,40.75066
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,1,N,2013-01-05 18:49:41,2013-01-05 18:54:23,1,282,1.10,-74.004707,40.73777,-74.009834,40.726002
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:54:15,2013-01-07 23:58:20,2,244,.70,-73.974602,40.759945,-73.984734,40.759388
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,1,N,2013-01-07 23:25:03,2013-01-07 23:34:24,1,560,2.10,-73.97625,40.748528,-74.002586,40.747868
* <span data-ttu-id="18937-184">hello 'trip_fare' CSV innehåller information om hello avgiften betalat för varje förflyttning, till exempel betalningssätt, avgiften belopp, tillägg och skatter, tips och vägtullar och hello totalbelopp betald.</span><span class="sxs-lookup"><span data-stu-id="18937-184">hello 'trip_fare' CSV contains details of hello fare paid for each trip, such as payment type, fare amount, surcharge and taxes, tips and tolls, and hello total amount paid.</span></span> <span data-ttu-id="18937-185">Här följer några Exempelposter:</span><span class="sxs-lookup"><span data-stu-id="18937-185">Here are a few sample records:</span></span>
  
       medallion, hack_license, vendor_id, pickup_datetime, payment_type, fare_amount, surcharge, mta_tax, tip_amount, tolls_amount, total_amount
       89D227B655E5C82AECF13C3F540D4CF4,BA96DE419E711691B9445D6A6307C170,CMT,2013-01-01 15:11:48,CSH,6.5,0,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-06 00:18:35,CSH,6,0.5,0.5,0,0,7
       0BD7C8F5BA12B88E0B67BED28BEA73D8,9FD8F69F0804BDB5549F40E9DA1BE472,CMT,2013-01-05 18:49:41,CSH,5.5,1,0.5,0,0,7
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:54:15,CSH,5,0.5,0.5,0,0,6
       DFD2202EE08F7A8DC9A57B02ACB81FE2,51EE87E3205C985EF8431D850C786310,CMT,2013-01-07 23:25:03,CSH,9.5,0.5,0.5,0,0,10.5

<span data-ttu-id="18937-186">hello Unik nyckel toojoin resa\_data och resa\_avgiften består av följande tre fält hello: medallion hackare\_licens och hämtning\_datetime.</span><span class="sxs-lookup"><span data-stu-id="18937-186">hello unique key toojoin trip\_data and trip\_fare is composed of hello following three fields: medallion, hack\_license and pickup\_datetime.</span></span> <span data-ttu-id="18937-187">hello raw CSV-filer kan nås från en offentlig Azure storage blob.</span><span class="sxs-lookup"><span data-stu-id="18937-187">hello raw CSV files can be accessed from a public Azure storage blob.</span></span> <span data-ttu-id="18937-188">Hej U-SQL-skript för den här kopplingen är i hello [koppla resa och avgiften tabeller](#join) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="18937-188">hello U-SQL script for this join is in hello [Join trip and fare tables](#join) section.</span></span>

## <a name="process-data-with-u-sql"></a><span data-ttu-id="18937-189">Bearbeta data med U-SQL</span><span class="sxs-lookup"><span data-stu-id="18937-189">Process data with U-SQL</span></span>
<span data-ttu-id="18937-190">hello databearbetning aktiviteter visas i det här avsnittet inkluderar vill föra in, kontrollera kvalitet, utforska och hello datasampling.</span><span class="sxs-lookup"><span data-stu-id="18937-190">hello data processing tasks illustrated in this section include ingesting, checking quality, exploring, and sampling hello data.</span></span> <span data-ttu-id="18937-191">Vi också visa hur toojoin resa och avgiften tabeller.</span><span class="sxs-lookup"><span data-stu-id="18937-191">We also show how toojoin trip and fare tables.</span></span> <span data-ttu-id="18937-192">hello sista avsnittet visar kör ett skript U-SQL-jobb från hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18937-192">hello final section shows run a U-SQL scripted job from hello Azure portal.</span></span> <span data-ttu-id="18937-193">Här följer länkar tooeach avsnitt:</span><span class="sxs-lookup"><span data-stu-id="18937-193">Here are links tooeach subsection:</span></span>

* [<span data-ttu-id="18937-194">Datapåfyllning: läsa data från offentliga blob</span><span class="sxs-lookup"><span data-stu-id="18937-194">Data ingestion: read in data from public blob</span></span>](#ingest)
* [<span data-ttu-id="18937-195">Data quality kontroller</span><span class="sxs-lookup"><span data-stu-id="18937-195">Data quality checks</span></span>](#quality)
* [<span data-ttu-id="18937-196">Datagranskning</span><span class="sxs-lookup"><span data-stu-id="18937-196">Data exploration</span></span>](#explore)
* [<span data-ttu-id="18937-197">Koppla resa och avgiften tabeller</span><span class="sxs-lookup"><span data-stu-id="18937-197">Join trip and fare tables</span></span>](#join)
* [<span data-ttu-id="18937-198">Data provtagning</span><span class="sxs-lookup"><span data-stu-id="18937-198">Data sampling</span></span>](#sample)
* [<span data-ttu-id="18937-199">Kör U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="18937-199">Run U-SQL jobs</span></span>](#run)

<span data-ttu-id="18937-200">hello U-SQL-skript som beskrivs här och anges i en separat fil.</span><span class="sxs-lookup"><span data-stu-id="18937-200">hello U-SQL scripts are described here and provided in a separate file.</span></span> <span data-ttu-id="18937-201">Du kan hämta hello fullständig **U-SQL-skript** från [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span><span class="sxs-lookup"><span data-stu-id="18937-201">You can download hello full **U-SQL scripts** from [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough).</span></span>

<span data-ttu-id="18937-202">tooexecute U-SQL, öppna Visual Studio klickar du på **filen--> Ny--> projekt**, Välj **U-SQL-projekt**, namnge och spara den tooa mapp.</span><span class="sxs-lookup"><span data-stu-id="18937-202">tooexecute U-SQL, Open Visual Studio, click **File --> New --> Project**, choose **U-SQL Project**, name and save it tooa folder.</span></span>

![8](./media/machine-learning-data-science-process-data-lake-walkthrough/8-create-USQL-project.PNG)

> [!NOTE]
> <span data-ttu-id="18937-204">Det är möjligt toouse hello Azure Portal tooexecute U-SQL i stället för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18937-204">It is possible toouse hello Azure Portal tooexecute U-SQL instead of Visual Studio.</span></span> <span data-ttu-id="18937-205">Du kan navigera toohello Azure Data Lake Analytics-resurs på hello portal och skicka frågor direkt som i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="18937-205">You can navigate toohello Azure Data Lake Analytics resource on hello portal and submit queries directly as illustrated in hello following figure.</span></span>
> 
> 

![9](./media/machine-learning-data-science-process-data-lake-walkthrough/9-portal-submit-job.PNG)

### <span data-ttu-id="18937-207"><a name="ingest"></a>Datapåfyllning: Läsa data från offentliga blob</span><span class="sxs-lookup"><span data-stu-id="18937-207"><a name="ingest"></a>Data Ingestion: Read in data from public blob</span></span>
<span data-ttu-id="18937-208">hello platsen för hello data i hello Azure blob hänvisas till som  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**  och extraheras med **Extractors.Csv()**.</span><span class="sxs-lookup"><span data-stu-id="18937-208">hello location of hello data in hello Azure blob is referenced as **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** and can be extracted using **Extractors.Csv()**.</span></span> <span data-ttu-id="18937-209">Ersätt behållarens namn och lagringskontonamn i följande skript för container_name@blob_storage_account_name i hello wasb adress.</span><span class="sxs-lookup"><span data-stu-id="18937-209">Substitute your own container name and storage account name in following scripts for container_name@blob_storage_account_name in hello wasb address.</span></span> <span data-ttu-id="18937-210">Eftersom hello filnamn har samma format kan vi använda **resa\_data_ {\*\}.csv** tooread i alla 12 resa filer.</span><span class="sxs-lookup"><span data-stu-id="18937-210">Since hello file names are in same format, we can use **trip\_data_{\*\}.csv** tooread in all 12 trip files.</span></span> 

    ///Read in Trip data
    @trip0 =
        EXTRACT 
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string
    // This is reading 12 trip data from blob
    FROM "wasb://container_name@blob_storage_account_name.blob.core.windows.net/nyctaxitrip/trip_data_{*}.csv"
    USING Extractors.Csv();

<span data-ttu-id="18937-211">Eftersom det inte finns rubriker på första raden i hello, vi behöver tooremove hello huvuden och ändra kolumnens datatyp till lämplig de.</span><span class="sxs-lookup"><span data-stu-id="18937-211">Since there are headers in hello first row, we need tooremove hello headers and change column types into appropriate ones.</span></span> <span data-ttu-id="18937-212">Vi kan antingen spara hello bearbetas data tooAzure lagring av Data Lake med hjälp av **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ eller tooAzure Blob storage-konto med hjälp av  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name** .</span><span class="sxs-lookup"><span data-stu-id="18937-212">We can either save hello processed data tooAzure Data Lake Storage using **swebhdfs://data_lake_storage_name.azuredatalakestorage.net/folder_name/file_name**_ or tooAzure Blob storage account using  **wasb://container_name@blob_storage_account_name.blob.core.windows.net/blob_name**.</span></span> 

    // change data types
    @trip =
        SELECT 
        medallion,
        hack_license,
        vendor_id,
        rate_code,
        store_and_fwd_flag,
        DateTime.Parse(pickup_datetime) AS pickup_datetime,
        DateTime.Parse(dropoff_datetime) AS dropoff_datetime,
        Int32.Parse(passenger_count) AS passenger_count,
        Double.Parse(trip_time_in_secs) AS trip_time_in_secs,
        Double.Parse(trip_distance) AS trip_distance,
        (pickup_longitude==string.Empty ? 0: float.Parse(pickup_longitude)) AS pickup_longitude,
        (pickup_latitude==string.Empty ? 0: float.Parse(pickup_latitude)) AS pickup_latitude,
        (dropoff_longitude==string.Empty ? 0: float.Parse(dropoff_longitude)) AS dropoff_longitude,
        (dropoff_latitude==string.Empty ? 0: float.Parse(dropoff_latitude)) AS dropoff_latitude
    FROM @trip0
    WHERE medallion != "medallion";

    ////output data tooADL
    OUTPUT @trip   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_trip.csv"
    USING Outputters.Csv(); 

    ////Output data tooblob
    OUTPUT @trip   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_trip.csv"
    USING Outputters.Csv();  

<span data-ttu-id="18937-213">På liknande sätt kan vi läsa i hello avgiften datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="18937-213">Similarly we can read in hello fare data sets.</span></span> <span data-ttu-id="18937-214">Högerklicka på Azure Data Lake Store kan du välja toolook data i **Azure Portal--> Data Explorer** eller **Utforskaren** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="18937-214">Right click Azure Data Lake Store, you can choose toolook at your data in **Azure Portal --> Data Explorer** or **File Explorer** within Visual Studio.</span></span> 

 ![10](./media/machine-learning-data-science-process-data-lake-walkthrough/10-data-in-ADL-VS.PNG)

 ![11](./media/machine-learning-data-science-process-data-lake-walkthrough/11-data-in-ADL.PNG)

### <span data-ttu-id="18937-217"><a name="quality"></a>Data quality kontroller</span><span class="sxs-lookup"><span data-stu-id="18937-217"><a name="quality"></a>Data quality checks</span></span>
<span data-ttu-id="18937-218">När resa och avgiften tabeller har lästs i, kan du göra data quality kontroller i hello följande sätt.</span><span class="sxs-lookup"><span data-stu-id="18937-218">After trip and fare tables have been read in, data quality checks can be done in hello following way.</span></span> <span data-ttu-id="18937-219">hello resulterande CSV-filer kan vara utdata tooAzure Blob storage eller Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="18937-219">hello resulting CSV files can be output tooAzure Blob storage or Azure Data Lake Store.</span></span> 

<span data-ttu-id="18937-220">Hitta hello antalet medallions och antalet medallions unika:</span><span class="sxs-lookup"><span data-stu-id="18937-220">Find hello number of medallions and unique number of medallions:</span></span>

    ///check hello number of medallions and unique number of medallions
    @trip2 =
        SELECT
        medallion,
        vendor_id,
        pickup_datetime.Month AS pickup_month
        FROM @trip;

    @ex_1 =
        SELECT
        pickup_month, 
        COUNT(medallion) AS cnt_medallion,
        COUNT(DISTINCT(medallion)) AS unique_medallion
        FROM @trip2
        GROUP BY pickup_month;
        OUTPUT @ex_1   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_1.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="18937-221">Hitta de medallions som har fler än 100 resor:</span><span class="sxs-lookup"><span data-stu-id="18937-221">Find those medallions that had more than 100 trips:</span></span>

    ///find those medallions that had more than 100 trips
    @ex_2 =
        SELECT medallion,
               COUNT(medallion) AS cnt_medallion
        FROM @trip2
        //where pickup_datetime >= "2013-01-01t00:00:00.0000000" and pickup_datetime <= "2013-04-01t00:00:00.0000000"
        GROUP BY medallion
        HAVING COUNT(medallion) > 100;
        OUTPUT @ex_2   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_2.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="18937-222">Hitta de ogiltiga posterna vad gäller pickup_longitude:</span><span class="sxs-lookup"><span data-stu-id="18937-222">Find those invalid records in terms of pickup_longitude:</span></span>

    ///find those invalid records in terms of pickup_longitude
    @ex_3 =
        SELECT COUNT(medallion) AS cnt_invalid_pickup_longitude
        FROM @trip
        WHERE
        pickup_longitude <- 90 OR pickup_longitude > 90;
        OUTPUT @ex_3   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_3.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="18937-223">Hitta saknade värden för vissa variabler:</span><span class="sxs-lookup"><span data-stu-id="18937-223">Find missing values for some variables:</span></span>

    //check missing values
    @res =
        SELECT *,
               (medallion == null? 1 : 0) AS missing_medallion
        FROM @trip;

    @trip_summary6 =
        SELECT 
            vendor_id,
        SUM(missing_medallion) AS medallion_empty, 
        COUNT(medallion) AS medallion_total,
        COUNT(DISTINCT(medallion)) AS medallion_total_unique  
        FROM @res
        GROUP BY vendor_id;
    OUTPUT @trip_summary6
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_16.csv"
    USING Outputters.Csv();



### <span data-ttu-id="18937-224"><a name="explore"></a>Datagranskning</span><span class="sxs-lookup"><span data-stu-id="18937-224"><a name="explore"></a>Data exploration</span></span>
<span data-ttu-id="18937-225">Vi kan göra vissa data från kartläggning av naturresurser tooget en bättre förståelse av hello data.</span><span class="sxs-lookup"><span data-stu-id="18937-225">We can do some data exploration tooget a better understanding of hello data.</span></span>

<span data-ttu-id="18937-226">Hitta hello distribution av lutad och icke-lutad resor:</span><span class="sxs-lookup"><span data-stu-id="18937-226">Find hello distribution of tipped and non-tipped trips:</span></span>

    ///tipped vs. not tipped distribution
    @tip_or_not =
        SELECT *,
               (tip_amount > 0 ? 1: 0) AS tipped
        FROM @fare;

    @ex_4 =
        SELECT tipped,
               COUNT(*) AS tip_freq
        FROM @tip_or_not
        GROUP BY tipped;
        OUTPUT @ex_4   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_4.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="18937-227">Hitta hello distribution av tips med avklippt värden: 0,5,10 och 20 dollar.</span><span class="sxs-lookup"><span data-stu-id="18937-227">Find hello distribution of tip amount with cut-off values: 0,5,10,and 20 dollars.</span></span>

    //tip class/range distribution
    @tip_class =
        SELECT *,
               (tip_amount >20? 4: (tip_amount >10? 3:(tip_amount >5 ? 2:(tip_amount > 0 ? 1: 0)))) AS tip_class
        FROM @fare;
    @ex_5 =
        SELECT tip_class,
               COUNT(*) AS tip_freq
        FROM @tip_class
        GROUP BY tip_class;
        OUTPUT @ex_5   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_5.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="18937-228">Hitta grundläggande statistik resa avstånd:</span><span class="sxs-lookup"><span data-stu-id="18937-228">Find basic statistics of trip distance:</span></span>

    // find basic statistics for trip_distance
    @trip_summary4 =
        SELECT 
            vendor_id,
            COUNT(*) AS cnt_row,
            MIN(trip_distance) AS min_trip_distance,
            MAX(trip_distance) AS max_trip_distance,
            AVG(trip_distance) AS avg_trip_distance 
        FROM @trip
        GROUP BY vendor_id;
    OUTPUT @trip_summary4
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_14.csv"
    USING Outputters.Csv();

<span data-ttu-id="18937-229">Hitta hello percentiler resa avstånd:</span><span class="sxs-lookup"><span data-stu-id="18937-229">Find hello percentiles of trip distance:</span></span>

    // find percentiles of trip_distance
    @trip_summary3 =
        SELECT DISTINCT vendor_id AS vendor,
                        PERCENTILE_DISC(0.25) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.5) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc,
                        PERCENTILE_DISC(0.75) WITHIN GROUP(ORDER BY trip_distance) OVER(PARTITION BY vendor_id) AS median_trip_distance_disc
        FROM @trip;
       // group by vendor_id;
    OUTPUT @trip_summary3
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_13.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="18937-230"><a name="join"></a>Koppla resa och avgiften tabeller</span><span class="sxs-lookup"><span data-stu-id="18937-230"><a name="join"></a>Join trip and fare tables</span></span>
<span data-ttu-id="18937-231">Resa och avgiften tabeller kan anslutas med medallion, hack_license och pickup_time.</span><span class="sxs-lookup"><span data-stu-id="18937-231">Trip and fare tables can be joined by medallion, hack_license, and pickup_time.</span></span>

    //join trip and fare table

    @model_data_full =
    SELECT t.*, 
    f.payment_type, f.fare_amount, f.surcharge, f.mta_tax, f.tolls_amount,  f.total_amount, f.tip_amount,
    (f.tip_amount > 0 ? 1: 0) AS tipped,
    (f.tip_amount >20? 4: (f.tip_amount >10? 3:(f.tip_amount >5 ? 2:(f.tip_amount > 0 ? 1: 0)))) AS tip_class
    FROM @trip AS t JOIN  @fare AS f
    ON   (t.medallion == f.medallion AND t.hack_license == f.hack_license AND t.pickup_datetime == f.pickup_datetime)
    WHERE   (pickup_longitude != 0 AND dropoff_longitude != 0 );

    //// output tooblob
    OUTPUT @model_data_full   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 

    ////output data tooADL
    OUTPUT @model_data_full   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_7_full_data.csv"
    USING Outputters.Csv(); 


<span data-ttu-id="18937-232">Beräkna hello antal poster, genomsnittlig tips belopp, varians av tips, procentandelen lutad resor för varje nivå av passagerare antalet.</span><span class="sxs-lookup"><span data-stu-id="18937-232">For each level of passenger count, calculate hello number of records, average tip amount, variance of tip amount, percentage of tipped trips.</span></span>

    // contigency table
    @trip_summary8 =
        SELECT passenger_count,
               COUNT(*) AS cnt,
               AVG(tip_amount) AS avg_tip_amount,
               VAR(tip_amount) AS var_tip_amount,
               SUM(tipped) AS cnt_tipped,
               (float)SUM(tipped)/COUNT(*) AS pct_tipped
        FROM @model_data_full
        GROUP BY passenger_count;
        OUTPUT @trip_summary8
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_17.csv"
    USING Outputters.Csv();


### <span data-ttu-id="18937-233"><a name="sample"></a>Data provtagning</span><span class="sxs-lookup"><span data-stu-id="18937-233"><a name="sample"></a>Data sampling</span></span>
<span data-ttu-id="18937-234">Vi markerar slumpmässigt du först 0,1% av hello data från hello kopplade tabellen:</span><span class="sxs-lookup"><span data-stu-id="18937-234">First we randomly select 0.1% of hello data from hello joined table:</span></span>

    //random select 1/1000 data for modeling purpose
    @addrownumberres_randomsample =
    SELECT *,
            ROW_NUMBER() OVER() AS rownum
    FROM @model_data_full;

    @model_data_random_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_randomsample
    WHERE rownum % 1000 == 0;

    OUTPUT @model_data_random_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_7_random_1_1000.csv"
    USING Outputters.Csv(); 

<span data-ttu-id="18937-235">Vi gör stratified sampling av binär variabeln tip_class:</span><span class="sxs-lookup"><span data-stu-id="18937-235">Then we do stratified sampling by binary variable tip_class:</span></span>

    //stratified random select 1/1000 data for modeling purpose
    @addrownumberres_stratifiedsample =
    SELECT *,
            ROW_NUMBER() OVER(PARTITION BY tip_class) AS rownum
    FROM @model_data_full;

    @model_data_stratified_sample_1_1000 =
    SELECT *
    FROM @addrownumberres_stratifiedsample
    WHERE rownum % 1000 == 0;
    //// output tooblob
    OUTPUT @model_data_stratified_sample_1_1000   
    too"wasb://container_name@blob_storage_account_name.blob.core.windows.net/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 
    ////output data tooADL
    OUTPUT @model_data_stratified_sample_1_1000   
    too"swebhdfs://data_lake_storage_name.azuredatalakestore.net/nyctaxi_folder/demo_ex_9_stratified_1_1000.csv"
    USING Outputters.Csv(); 


### <span data-ttu-id="18937-236"><a name="run"></a>Kör U-SQL-jobb</span><span class="sxs-lookup"><span data-stu-id="18937-236"><a name="run"></a>Run U-SQL jobs</span></span>
<span data-ttu-id="18937-237">När du har redigerat U-SQL-skript, kan du skicka dem toohello servern med hjälp av Azure Data Lake Analytics-kontot.</span><span class="sxs-lookup"><span data-stu-id="18937-237">When you finish editing U-SQL scripts, you can submit them toohello server using your Azure Data Lake Analytics account.</span></span> <span data-ttu-id="18937-238">Klicka på **Data Lake**, **skicka jobbet**, Välj din **Analytics-konto**, Välj **parallellitet**, och klicka på **skicka**  knappen.</span><span class="sxs-lookup"><span data-stu-id="18937-238">Click **Data Lake**, **Submit Job**, select your **Analytics Account**, choose **Parallelism**, and click **Submit** button.</span></span>  

 ![12](./media/machine-learning-data-science-process-data-lake-walkthrough/12-submit-USQL.PNG)

<span data-ttu-id="18937-240">När jobbet hello uppfyller har visas hello status för jobbet i Visual Studio för övervakning.</span><span class="sxs-lookup"><span data-stu-id="18937-240">When hello job is complied successfully, hello status of your job will be displayed in Visual Studio for monitoring.</span></span> <span data-ttu-id="18937-241">När hello jobbet är klar kan du även replay hello körning jobbprocess och ta reda på hello bottleneck steg tooimprove jobbet effektiviteten.</span><span class="sxs-lookup"><span data-stu-id="18937-241">After hello job finishes running, you can even replay hello job execution process and find out hello bottleneck steps tooimprove your job efficiency.</span></span> <span data-ttu-id="18937-242">Du kan även gå tooAzure Portal toocheck hello status för U-SQL-jobb.</span><span class="sxs-lookup"><span data-stu-id="18937-242">You can also go tooAzure Portal toocheck hello status of your U-SQL jobs.</span></span>

 ![13](./media/machine-learning-data-science-process-data-lake-walkthrough/13-USQL-running-v2.PNG)

 ![14](./media/machine-learning-data-science-process-data-lake-walkthrough/14-USQL-jobs-portal.PNG)

<span data-ttu-id="18937-245">Nu kan du kontrollera hello utdatafilerna i Azure Blob storage eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="18937-245">Now you can check hello output files in either Azure Blob storage or Azure Portal.</span></span> <span data-ttu-id="18937-246">Vi använder hello stratified exempeldata för våra modellering i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="18937-246">We will use hello stratified sample data for our modeling in hello next step.</span></span>

 ![15](./media/machine-learning-data-science-process-data-lake-walkthrough/15-U-SQL-output-csv.PNG)

 ![16](./media/machine-learning-data-science-process-data-lake-walkthrough/16-U-SQL-output-csv-portal.PNG)

## <a name="build-and-deploy-models-in-azure-machine-learning"></a><span data-ttu-id="18937-249">Skapa och distribuera modeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="18937-249">Build and deploy models in Azure Machine Learning</span></span>
<span data-ttu-id="18937-250">Ser du två alternativ som är tillgängliga för dig toopull data i Azure Machine Learning toobuild och</span><span class="sxs-lookup"><span data-stu-id="18937-250">We demonstrate two options available for you toopull data into Azure Machine Learning toobuild and</span></span> 

* <span data-ttu-id="18937-251">I hello första alternativet, använder du hello provtagning data som har skrivits tooan Azure Blob (i hello **Data provtagning** steget ovan) och använda Python toobuild och distribuera modeller från Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="18937-251">In hello first option, you use hello sampled data that has been written tooan Azure Blob (in hello **Data sampling** step above) and use Python toobuild and deploy models from Azure Machine Learning.</span></span> 
* <span data-ttu-id="18937-252">I andra alternativet hello fråga du hello data i Azure Data Lake direkt med en Hive-fråga.</span><span class="sxs-lookup"><span data-stu-id="18937-252">In hello second option, you query hello data in Azure Data Lake directly using a Hive query.</span></span> <span data-ttu-id="18937-253">Det här alternativet kräver att du skapar ett nytt kluster i HDInsight eller Använd ett befintligt HDInsight-kluster där hello Hive tabeller punkt toohello NY Taxi data i Azure Data Lake Storage.</span><span class="sxs-lookup"><span data-stu-id="18937-253">This option requires that you create a new HDInsight cluster or use an existing HDInsight cluster where hello Hive tables point toohello NY Taxi data in Azure Data Lake Storage.</span></span>  <span data-ttu-id="18937-254">Diskuterar vi båda alternativen nedan.</span><span class="sxs-lookup"><span data-stu-id="18937-254">We discuss both these options below.</span></span> 

## <a name="option-1-use-python-toobuild-and-deploy-machine-learning-models"></a><span data-ttu-id="18937-255">Alternativ 1: Använd Python toobuild och distribuera machine learning-modeller</span><span class="sxs-lookup"><span data-stu-id="18937-255">Option 1: Use Python toobuild and deploy machine learning models</span></span>
<span data-ttu-id="18937-256">toobuild och distribuera machine learning-modeller använder Python, skapa en Jupyter-anteckningsbok på den lokala datorn eller i Azure Machine Learning Studio.</span><span class="sxs-lookup"><span data-stu-id="18937-256">toobuild and deploy machine learning models using Python, create a Jupyter Notebook on your local machine or in Azure Machine Learning Studio.</span></span> <span data-ttu-id="18937-257">Hej Jupyter-anteckningsbok som finns på [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) innehåller Hej fullständiga koden tooexplore, visualisera data, funktionen tekniker, modellering och distribution.</span><span class="sxs-lookup"><span data-stu-id="18937-257">hello Jupyter Notebook  provided on [GitHub](https://github.com/Azure/Azure-MachineLearning-DataScience/tree/master/Misc/AzureDataLakeWalkthrough) contains hello full code tooexplore, visualize data, feature engineering, modeling and deployment.</span></span> <span data-ttu-id="18937-258">I den här artikeln visar vi bara hello modellering och distribution.</span><span class="sxs-lookup"><span data-stu-id="18937-258">In this article, we show just hello modeling and deployment.</span></span> 

### <a name="import-python-libraries"></a><span data-ttu-id="18937-259">Importera Python-bibliotek</span><span class="sxs-lookup"><span data-stu-id="18937-259">Import Python libraries</span></span>
<span data-ttu-id="18937-260">Exempel Jupyter Notebook i ordning toorun hello eller Hej Python skriptfilen, hello följande Python-paket som behövs.</span><span class="sxs-lookup"><span data-stu-id="18937-260">In order toorun hello sample Jupyter Notebook or hello Python script file, hello following Python packages are needed.</span></span> <span data-ttu-id="18937-261">Om du använder hello tjänsten för AzureML-anteckningsboken har paketen förinstallerat.</span><span class="sxs-lookup"><span data-stu-id="18937-261">If you are using hello AzureML Notebook service, these packages have been pre-installed.</span></span>

    import pandas as pd
    from pandas import Series, DataFrame
    import numpy as np
    import matplotlib.pyplot as plt
    from time import time
    import pyodbc
    import os
    from azure.storage.blob import BlobService
    import tables
    import time
    import zipfile
    import random
    import sklearn
    from sklearn.linear_model import LogisticRegression
    from sklearn.cross_validation import train_test_split
    from sklearn import metrics
    from __future__ import division
    from sklearn import linear_model
    from azureml import services


### <a name="read-in-hello-data-from-blob"></a><span data-ttu-id="18937-262">Läs i hello data från blob</span><span class="sxs-lookup"><span data-stu-id="18937-262">Read in hello data from blob</span></span>
* <span data-ttu-id="18937-263">Anslutningssträng</span><span class="sxs-lookup"><span data-stu-id="18937-263">Connection String</span></span>   
  
        CONTAINERNAME = 'test1'
        STORAGEACCOUNTNAME = 'XXXXXXXXX'
        STORAGEACCOUNTKEY = 'YYYYYYYYYYYYYYYYYYYYYYYYYYYY'
        BLOBNAME = 'demo_ex_9_stratified_1_1000_copy.csv'
        blob_service = BlobService(account_name=STORAGEACCOUNTNAME,account_key=STORAGEACCOUNTKEY)
* <span data-ttu-id="18937-264">Läs i som text</span><span class="sxs-lookup"><span data-stu-id="18937-264">Read in as text</span></span>
  
        t1 = time.time()
        data = blob_service.get_blob_to_text(CONTAINERNAME,BLOBNAME).split("\n")
        t2 = time.time()
        print(("It takes %s seconds tooread in "+BLOBNAME) % (t2 - t1))
  
  ![17](./media/machine-learning-data-science-process-data-lake-walkthrough/17-python_readin_csv.PNG)    
* <span data-ttu-id="18937-266">Lägg till kolumnnamn och avgränsa kolumner</span><span class="sxs-lookup"><span data-stu-id="18937-266">Add column names and separate columns</span></span>
  
        colnames = ['medallion','hack_license','vendor_id','rate_code','store_and_fwd_flag','pickup_datetime','dropoff_datetime',
        'passenger_count','trip_time_in_secs','trip_distance','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'payment_type', 'fare_amount', 'surcharge', 'mta_tax', 'tolls_amount',  'total_amount', 'tip_amount', 'tipped', 'tip_class', 'rownum']
        df1 = pd.DataFrame([sub.split(",") for sub in data], columns = colnames)
* <span data-ttu-id="18937-267">Ändra vissa kolumner toonumeric</span><span class="sxs-lookup"><span data-stu-id="18937-267">Change some columns toonumeric</span></span>
  
        cols_2_float = ['trip_time_in_secs','pickup_longitude','pickup_latitude','dropoff_longitude','dropoff_latitude',
        'fare_amount', 'surcharge','mta_tax','tolls_amount','total_amount','tip_amount', 'passenger_count','trip_distance'
        ,'tipped','tip_class','rownum']
        for col in cols_2_float:
            df1[col] = df1[col].astype(float)

### <a name="build-machine-learning-models"></a><span data-ttu-id="18937-268">Skapa machine learning-modeller</span><span class="sxs-lookup"><span data-stu-id="18937-268">Build machine learning models</span></span>
<span data-ttu-id="18937-269">Här skapa vi en binär klassificering modellen toopredict om en resa lutad eller inte.</span><span class="sxs-lookup"><span data-stu-id="18937-269">Here we build a binary classification model toopredict whether a trip is tipped or not.</span></span> <span data-ttu-id="18937-270">Du kan hitta andra två modeller i hello Jupyter-anteckningsbok: multiklass-baserad klassificering och regression modeller.</span><span class="sxs-lookup"><span data-stu-id="18937-270">In hello Jupyter Notebook you can find other two models: multiclass classification, and regression models.</span></span>

* <span data-ttu-id="18937-271">Vi måste först toocreate dummy variabler som kan användas i scikit-Läs modeller</span><span class="sxs-lookup"><span data-stu-id="18937-271">First we need toocreate dummy variables that can be used in scikit-learn models</span></span>
  
        df1_payment_type_dummy = pd.get_dummies(df1['payment_type'], prefix='payment_type_dummy')
        df1_vendor_id_dummy = pd.get_dummies(df1['vendor_id'], prefix='vendor_id_dummy')
* <span data-ttu-id="18937-272">Skapa data ram för hello modellering</span><span class="sxs-lookup"><span data-stu-id="18937-272">Create data frame for hello modeling</span></span>
  
        cols_to_keep = ['tipped', 'trip_distance', 'passenger_count']
        data = df1[cols_to_keep].join([df1_payment_type_dummy,df1_vendor_id_dummy])
  
        X = data.iloc[:,1:]
        Y = data.tipped
* <span data-ttu-id="18937-273">Träning och testning 60-40 delning</span><span class="sxs-lookup"><span data-stu-id="18937-273">Training and testing 60-40 split</span></span>
  
        X_train, X_test, Y_train, Y_test = train_test_split(X, Y, test_size=0.4, random_state=0)
* <span data-ttu-id="18937-274">Logistic Regression i träningsmängden</span><span class="sxs-lookup"><span data-stu-id="18937-274">Logistic Regression in training set</span></span>
  
        model = LogisticRegression()
        logit_fit = model.fit(X_train, Y_train)
        print ('Coefficients: \n', logit_fit.coef_)
        Y_train_pred = logit_fit.predict(X_train)
  
       ![c1](./media/machine-learning-data-science-process-data-lake-walkthrough/c1-py-logit-coefficient.PNG)
* <span data-ttu-id="18937-275">Poängsätta tester datauppsättning</span><span class="sxs-lookup"><span data-stu-id="18937-275">Score testing data set</span></span>
  
        Y_test_pred = logit_fit.predict(X_test)
* <span data-ttu-id="18937-276">Beräkna utvärdering mått</span><span class="sxs-lookup"><span data-stu-id="18937-276">Calculate Evaluation metrics</span></span>
  
        fpr_train, tpr_train, thresholds_train = metrics.roc_curve(Y_train, Y_train_pred)
        print fpr_train, tpr_train, thresholds_train
  
        fpr_test, tpr_test, thresholds_test = metrics.roc_curve(Y_test, Y_test_pred) 
        print fpr_test, tpr_test, thresholds_test
  
        #AUC
        print metrics.auc(fpr_train,tpr_train)
        print metrics.auc(fpr_test,tpr_test)
  
        #Confusion Matrix
        print metrics.confusion_matrix(Y_train,Y_train_pred)
        print metrics.confusion_matrix(Y_test,Y_test_pred)
  
       ![c2](./media/machine-learning-data-science-process-data-lake-walkthrough/c2-py-logit-evaluation.PNG)

### <a name="build-web-service-api-and-consume-it-in-python"></a><span data-ttu-id="18937-277">Skapa-webbtjänstens API och använda den i Python</span><span class="sxs-lookup"><span data-stu-id="18937-277">Build Web Service API and consume it in Python</span></span>
<span data-ttu-id="18937-278">Vi vill toooperationalize hello maskininlärning modellen när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="18937-278">We want toooperationalize hello machine learning model after it has been built.</span></span> <span data-ttu-id="18937-279">Vi använder här hello binär logistic modell som exempel.</span><span class="sxs-lookup"><span data-stu-id="18937-279">Here we use hello binary logistic model as an example.</span></span> <span data-ttu-id="18937-280">Se till att hello scikit-Läs mer om versionen i din lokala dator 0.15.1.</span><span class="sxs-lookup"><span data-stu-id="18937-280">Make sure hello scikit-learn version in your local machine is 0.15.1.</span></span> <span data-ttu-id="18937-281">Du har inte tooworry om detta om du använder Azure ML studio tjänst.</span><span class="sxs-lookup"><span data-stu-id="18937-281">You don't have tooworry about this if you use Azure ML studio service.</span></span>

* <span data-ttu-id="18937-282">Hitta din arbetsyta autentiseringsuppgifter från Azure ML studio inställningar.</span><span class="sxs-lookup"><span data-stu-id="18937-282">Find your workspace credentials from Azure ML studio settings.</span></span> <span data-ttu-id="18937-283">I Azure Machine Learning Studio klickar du på **inställningar** --> **namn** --> **auktorisering token**.</span><span class="sxs-lookup"><span data-stu-id="18937-283">In Azure Machine Learning Studio, click **Settings** --> **Name** --> **Authorization Tokens**.</span></span> 
  
    ![C3](./media/machine-learning-data-science-process-data-lake-walkthrough/c3-workspace-id.PNG)

        workspaceid = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'
        auth_token = 'xxxxxxxxxxxxxxxxxxxxxxxxxxx'

* <span data-ttu-id="18937-285">Skapa webbtjänst</span><span class="sxs-lookup"><span data-stu-id="18937-285">Create Web Service</span></span>
  
        @services.publish(workspaceid, auth_token) 
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float, payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(int) #0, or 1
        def predictNYCTAXI(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            inputArray = [trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH, payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS]
            return logit_fit.predict(inputArray)
* <span data-ttu-id="18937-286">Hämta autentiseringsuppgifter för web service</span><span class="sxs-lookup"><span data-stu-id="18937-286">Get web service credentials</span></span>
  
        url = predictNYCTAXI.service.url
        api_key =  predictNYCTAXI.service.api_key
  
        print url
        print api_key
  
        @services.service(url, api_key)
        @services.types(trip_distance = float, passenger_count = float, payment_type_dummy_CRD = float, payment_type_dummy_CSH=float,payment_type_dummy_DIS = float, payment_type_dummy_NOC = float, payment_type_dummy_UNK = float, vendor_id_dummy_CMT = float, vendor_id_dummy_VTS = float)
        @services.returns(float)
        def NYCTAXIPredictor(trip_distance, passenger_count, payment_type_dummy_CRD, payment_type_dummy_CSH,payment_type_dummy_DIS, payment_type_dummy_NOC, payment_type_dummy_UNK, vendor_id_dummy_CMT, vendor_id_dummy_VTS ):
            pass
* <span data-ttu-id="18937-287">Anropa-webbtjänstens API.</span><span class="sxs-lookup"><span data-stu-id="18937-287">Call Web service API.</span></span> <span data-ttu-id="18937-288">Du har toowait 5-10 sekunder efter hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="18937-288">You have toowait 5-10 seconds after hello previous step.</span></span>
  
        NYCTAXIPredictor(1,2,1,0,0,0,0,0,1)
  
       ![c4](./media/machine-learning-data-science-process-data-lake-walkthrough/c4-call-API.PNG)

## <a name="option-2-create-and-deploy-models-directly-in-azure-machine-learning"></a><span data-ttu-id="18937-289">Alternativ 2: Skapa och distribuera modeller direkt i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="18937-289">Option 2: Create and deploy models directly in Azure Machine Learning</span></span>
<span data-ttu-id="18937-290">Azure Machine Learning Studio kan läsa data direkt från Azure Data Lake Store och sedan använda toocreate och distribuera modeller.</span><span class="sxs-lookup"><span data-stu-id="18937-290">Azure Machine Learning Studio can read data directly from Azure Data Lake Store and then be used toocreate and deploy models.</span></span> <span data-ttu-id="18937-291">Den här metoden använder en Hive-tabell som pekar på hello Azure Data Lake Store.</span><span class="sxs-lookup"><span data-stu-id="18937-291">This approach uses a Hive table that points at hello Azure Data Lake Store.</span></span> <span data-ttu-id="18937-292">Detta kräver att ett separat Azure HDInsight-kluster etableras, på vilken hello Hive tabellen har skapats.</span><span class="sxs-lookup"><span data-stu-id="18937-292">This requires that a separate Azure HDInsight cluster be provisioned, on which hello Hive table is created.</span></span> <span data-ttu-id="18937-293">Hej följande avsnitt visas hur toodo detta.</span><span class="sxs-lookup"><span data-stu-id="18937-293">hello following sections show how toodo this.</span></span> 

### <a name="create-an-hdinsight-linux-cluster"></a><span data-ttu-id="18937-294">Skapa ett HDInsight Linux-kluster</span><span class="sxs-lookup"><span data-stu-id="18937-294">Create an HDInsight Linux Cluster</span></span>
<span data-ttu-id="18937-295">Skapa ett HDInsight-kluster (Linux) från hello [Azure Portal](http://portal.azure.com). Mer information finns i hello **skapar ett HDInsight-kluster med åtkomst tooAzure Datasjölager** i avsnittet [skapar ett HDInsight-kluster med Data Lake Store med hjälp av Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="18937-295">Create an HDInsight Cluster (Linux) from hello [Azure Portal](http://portal.azure.com).For details, see hello **Create an HDInsight cluster with access tooAzure Data Lake Store** section in [Create an HDInsight cluster with Data Lake Store using Azure Portal](../data-lake-store/data-lake-store-hdinsight-hadoop-use-portal.md).</span></span>

 ![18](./media/machine-learning-data-science-process-data-lake-walkthrough/18-create_HDI_cluster.PNG)

### <a name="create-hive-table-in-hdinsight"></a><span data-ttu-id="18937-297">Skapa Hive-tabellen i HDInsight</span><span class="sxs-lookup"><span data-stu-id="18937-297">Create Hive table in HDInsight</span></span>
<span data-ttu-id="18937-298">Nu skapar vi Hive-tabeller toobe används i Azure Machine Learning Studio i hello HDInsight-kluster med hello data som lagras i Azure Data Lake Store i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="18937-298">Now we create Hive tables toobe used in Azure Machine Learning Studio in hello HDInsight cluster using hello data stored in Azure Data Lake Store in hello previous step.</span></span> <span data-ttu-id="18937-299">Gå toohello HDInsight-kluster som precis har skapat.</span><span class="sxs-lookup"><span data-stu-id="18937-299">Go toohello HDInsight cluster just created.</span></span> <span data-ttu-id="18937-300">Klicka på **inställningar** --> **egenskaper** --> **kluster-AAD-identitet** --> **ADLS-åtkomst**, Kontrollera att ditt Azure Data Lake Store-konto har lagts till i hello lista med läs, skriva och köra rättigheter.</span><span class="sxs-lookup"><span data-stu-id="18937-300">Click **Settings** --> **Properties** --> **Cluster AAD Identity** --> **ADLS Access**, make sure your Azure Data Lake Store account is added in hello list with read, write and execute rights.</span></span> 

 ![19](./media/machine-learning-data-science-process-data-lake-walkthrough/19-HDI-cluster-add-ADLS.PNG)

<span data-ttu-id="18937-302">Klicka på **instrumentpanelen** nästa toohello **inställningar** knappen och ett fönster visas.</span><span class="sxs-lookup"><span data-stu-id="18937-302">Then click **Dashboard** next toohello **Settings** button and a window will pop up.</span></span> <span data-ttu-id="18937-303">Klicka på **Hive-vy** i hello övre högra hörnet av hello sida och du ser hello **frågeredigeraren**.</span><span class="sxs-lookup"><span data-stu-id="18937-303">Click **Hive View** in hello upper right corner of hello page and you will see hello **Query Editor**.</span></span>

 ![20](./media/machine-learning-data-science-process-data-lake-walkthrough/20-HDI-dashboard.PNG)

 ![21](./media/machine-learning-data-science-process-data-lake-walkthrough/21-Hive-Query-Editor-v2.PNG)

<span data-ttu-id="18937-306">Klistra in följande hello Hive skript toocreate en tabell.</span><span class="sxs-lookup"><span data-stu-id="18937-306">Paste in hello following Hive scripts toocreate a table.</span></span> <span data-ttu-id="18937-307">hello datakällan lagras i Azure Data Lake Store referensen i det här sättet: **adl://data_lake_store_name.azuredatalakestore.net:443/mappnamn/filnamn**.</span><span class="sxs-lookup"><span data-stu-id="18937-307">hello location of data source is in Azure Data Lake Store reference in this way: **adl://data_lake_store_name.azuredatalakestore.net:443/folder_name/file_name**.</span></span>

    CREATE EXTERNAL TABLE nyc_stratified_sample
    (
        medallion string,
        hack_license string,
        vendor_id string,
        rate_code string,
        store_and_fwd_flag string,
        pickup_datetime string,
        dropoff_datetime string,
        passenger_count string,
        trip_time_in_secs string,
        trip_distance string,
        pickup_longitude string,
        pickup_latitude string,
        dropoff_longitude string,
        dropoff_latitude string,
      payment_type string,
      fare_amount string,
      surcharge string,
      mta_tax string,
      tolls_amount string,
      total_amount string,
      tip_amount string,
      tipped string,
      tip_class string,
      rownum string
      )
    ROW FORMAT DELIMITED FIELDS TERMINATED BY ',' lines terminated by '\n'
    LOCATION 'adl://data_lake_storage_name.azuredatalakestore.net:443/nyctaxi_folder/demo_ex_9_stratified_1_1000_copy.csv';


<span data-ttu-id="18937-308">När hello frågan är klar visas resultaten hello så här:</span><span class="sxs-lookup"><span data-stu-id="18937-308">When hello query finishes running, you will see hello results like this:</span></span>

 ![22](./media/machine-learning-data-science-process-data-lake-walkthrough/22-Hive-Query-results.PNG)

### <a name="build-and-deploy-models-in-azure-machine-learning-studio"></a><span data-ttu-id="18937-310">Skapa och distribuera modeller i Azure Machine Learning Studio</span><span class="sxs-lookup"><span data-stu-id="18937-310">Build and deploy models in Azure Machine Learning Studio</span></span>
<span data-ttu-id="18937-311">Vi är nu redo toobuild och distribuera en modell som beräknar huruvida ett tips är betald med Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="18937-311">We are now ready toobuild and deploy a model that predicts whether or not a tip is paid with Azure Machine Learning.</span></span> <span data-ttu-id="18937-312">Hej stratified exempeldata är klar toobe som används i denna binär klassificering (tips eller inte) problem.</span><span class="sxs-lookup"><span data-stu-id="18937-312">hello stratified sample data is ready toobe used in this binary classification (tip or not) problem.</span></span> <span data-ttu-id="18937-313">Hej förutsägelsemodeller med multiklass-baserad klassificering (tip_class) och regression (tip_amount) kan också skapats och distribuerats med Azure Machine Learning Studio, men här vi bara visar hur toohandle hello case med hello binär klassificering modellen.</span><span class="sxs-lookup"><span data-stu-id="18937-313">hello predictive models using multiclass classification (tip_class) and regression (tip_amount) can also be built and deployed with Azure Machine Learning Studio, but here we only show how toohandle hello case using hello binary classification model.</span></span>

1. <span data-ttu-id="18937-314">Hämta hello data till Azure ML med hello **importera Data** modulen är tillgängliga i hello **Data ingående och utgående** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="18937-314">Get hello data into Azure ML using hello **Import Data** module, available in hello **Data Input and Output** section.</span></span> <span data-ttu-id="18937-315">Mer information finns i hello [importera Data modulen](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) referenssida.</span><span class="sxs-lookup"><span data-stu-id="18937-315">For more information, see hello [Import Data module](https://msdn.microsoft.com/library/azure/4e1b0fe6-aded-4b3f-a36f-39b8862b9004/) reference page.</span></span>
2. <span data-ttu-id="18937-316">Välj **Hive-fråga** som hello **datakällan** i hello **egenskaper** panelen.</span><span class="sxs-lookup"><span data-stu-id="18937-316">Select **Hive Query** as hello **Data source** in hello **Properties** panel.</span></span>
3. <span data-ttu-id="18937-317">Klistra in hello följande Hive-skript i hello **Hive databasfrågan** redigeraren</span><span class="sxs-lookup"><span data-stu-id="18937-317">Paste hello following Hive script in hello **Hive database query** editor</span></span>
   
        select * from nyc_stratified_sample;
4. <span data-ttu-id="18937-318">Ange hello URI för HDInsight-klustret (detta finns i Azure Portal), Hadoop-autentiseringsuppgifter, platsen för utdata och Azure namn/nyckelbehållare för lagringskontonamnet.</span><span class="sxs-lookup"><span data-stu-id="18937-318">Enter hello URI of HDInsight cluster (this can be found in Azure Portal), Hadoop credentials, location of output data, and Azure storage account name/key/container name.</span></span>
   
   ![23](./media/machine-learning-data-science-process-data-lake-walkthrough/23-reader-module-v3.PNG)  

<span data-ttu-id="18937-320">Ett exempel på en binär klassificering experiment som läser data från Hive-tabell visas i hello bilden nedan.</span><span class="sxs-lookup"><span data-stu-id="18937-320">An example of a binary classification experiment reading data from Hive table is shown in hello figure below.</span></span>

 ![24](./media/machine-learning-data-science-process-data-lake-walkthrough/24-AML-exp.PNG)

<span data-ttu-id="18937-322">När du har skapat hello experimentet klickar du på **konfigurera Web Service** --> **förutsägande webbtjänst**</span><span class="sxs-lookup"><span data-stu-id="18937-322">After hello experiment is created, click  **Set Up Web Service** --> **Predictive Web Service**</span></span>

 ![25](./media/machine-learning-data-science-process-data-lake-walkthrough/25-AML-exp-deploy.PNG)

<span data-ttu-id="18937-324">Kör hello skapas automatiskt bedömningen experiment, när den är klar klickar du på **distribuera webbtjänsten**</span><span class="sxs-lookup"><span data-stu-id="18937-324">Run hello automatically created scoring experiment, when it finishes, click **Deploy Web Service**</span></span>

 ![26](./media/machine-learning-data-science-process-data-lake-walkthrough/26-AML-exp-deploy-web.PNG)

<span data-ttu-id="18937-326">hello web service instrumentpanelen visas snart:</span><span class="sxs-lookup"><span data-stu-id="18937-326">hello web service dashboard will be displayed shortly:</span></span>

 ![27](./media/machine-learning-data-science-process-data-lake-walkthrough/27-AML-web-api.PNG)

## <a name="summary"></a><span data-ttu-id="18937-328">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="18937-328">Summary</span></span>
<span data-ttu-id="18937-329">Du har skapat en datavetenskap miljö för att skapa skalbara lösningar för slutpunkt till slutpunkt i Azure Data Lake genom att slutföra den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="18937-329">By completing this walkthrough you have created a data science environment for building scalable end-to-end solutions in Azure Data Lake.</span></span> <span data-ttu-id="18937-330">Den här miljön har använt tooanalyze en stor offentliga datauppsättning tar genom hello kanoniska stegen för hello datavetenskap processen från datainsamling genom modellen utbildning och sedan toohello distribution av hello modell som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="18937-330">This environment was used tooanalyze a large public dataset, taking it through hello canonical steps of hello Data Science Process, from data acquisition through model training, and then toohello deployment of hello model as a web service.</span></span> <span data-ttu-id="18937-331">U-SQL har använt tooprocess, utforska och hello exempeldata.</span><span class="sxs-lookup"><span data-stu-id="18937-331">U-SQL was used tooprocess, explore and sample hello data.</span></span> <span data-ttu-id="18937-332">Python och Hive användes med Azure Machine Learning Studio toobuild och distribuera förutsägelsemodeller.</span><span class="sxs-lookup"><span data-stu-id="18937-332">Python and Hive were used with Azure Machine Learning Studio toobuild and deploy predictive models.</span></span>

## <a name="whats-next"></a><span data-ttu-id="18937-333">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="18937-333">What's next?</span></span>
<span data-ttu-id="18937-334">hello utbildningsvägar för den [Team Data vetenskap processen (TDSP)](http://aka.ms/datascienceprocess) ger länkar tootopics som beskriver varje steg i hello advanced analytics processen.</span><span class="sxs-lookup"><span data-stu-id="18937-334">hello learning path for the [Team Data Science Process (TDSP)](http://aka.ms/datascienceprocess) provides links tootopics describing each step in hello advanced analytics process.</span></span> <span data-ttu-id="18937-335">Det finns ett antal genomgång specificerade på hello [Team datavetenskap Process genomgång](data-science-process-walkthroughs.md) sidan den samlade hur toouse resurser och tjänster i olika förutsägelseanalyser scenarier:</span><span class="sxs-lookup"><span data-stu-id="18937-335">There are a series of walkthroughs itemized on hello [Team Data Science Process walkthroughs](data-science-process-walkthroughs.md) page that showcase how toouse resources and services in various predictive analytics scenarios:</span></span>

* [<span data-ttu-id="18937-336">hello Team av vetenskapliga data i praktiken: använda SQL Data Warehouse</span><span class="sxs-lookup"><span data-stu-id="18937-336">hello Team Data Science Process in action: using SQL Data Warehouse</span></span>](machine-learning-data-science-process-sqldw-walkthrough.md)
* [<span data-ttu-id="18937-337">hello Team av vetenskapliga data i praktiken: med HDInsight Hadoop-kluster</span><span class="sxs-lookup"><span data-stu-id="18937-337">hello Team Data Science Process in action: using HDInsight Hadoop clusters</span></span>](machine-learning-data-science-process-hive-walkthrough.md)
* [<span data-ttu-id="18937-338">hello Team datavetenskap Process: använder SQL Server</span><span class="sxs-lookup"><span data-stu-id="18937-338">hello Team Data Science Process: using SQL Server</span></span>](machine-learning-data-science-process-sql-walkthrough.md)
* [<span data-ttu-id="18937-339">Översikt över hello vetenskap av data med hjälp av Väck på Azure HDInsight</span><span class="sxs-lookup"><span data-stu-id="18937-339">Overview of hello Data Science Process using Spark on Azure HDInsight</span></span>](machine-learning-data-science-spark-overview.md)

