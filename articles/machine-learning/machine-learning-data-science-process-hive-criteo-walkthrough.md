---
title: "hello Team av vetenskapliga data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB | Microsoft Docs"
description: "Med hjälp av hello Team datavetenskap Process för en slutpunkt till slutpunkt-scenario med en HDInsight Hadoop kluster toobuild och distribuera en modell med en stor offentligt tillgängliga (1 TB) datauppsättning"
services: machine-learning,hdinsight
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 72d958c4-3205-49b9-ad82-47998d400d2b
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/29/2017
ms.author: bradsev
ms.openlocfilehash: 59b2af02e7840cb60a4b5b2f2c8ab0611df198ec
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="hello-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a><span data-ttu-id="348e4-103">hello Team av vetenskapliga data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB</span><span class="sxs-lookup"><span data-stu-id="348e4-103">hello Team Data Science Process in action - Using an Azure HDInsight Hadoop Cluster on a 1 TB dataset</span></span>

<span data-ttu-id="348e4-104">I den här genomgången visar hur hello Team datavetenskap Process i ett scenario för slutpunkt till slutpunkt med ett [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) toostore, utforska funktion tekniker och ned exempeldata från en hello offentligt tillgängliga [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="348e4-104">In this walkthrough, we demonstrate using hello Team Data Science Process in an end-to-end scenario with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) toostore, explore, feature engineer, and down sample data from one of hello publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datasets.</span></span> <span data-ttu-id="348e4-105">Vi kan använda Azure Machine Learning toobuild en binär klassificering modell för den här informationen.</span><span class="sxs-lookup"><span data-stu-id="348e4-105">We use Azure Machine Learning toobuild a binary classification model on this data.</span></span> <span data-ttu-id="348e4-106">Vi kan också visa hur toopublish en av dessa modeller som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="348e4-106">We also show how toopublish one of these models as a Web service.</span></span>

<span data-ttu-id="348e4-107">Det är också möjligt toouse en IPython anteckningsboken tooaccomplish hello aktiviteter visas i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="348e4-107">It is also possible toouse an IPython notebook tooaccomplish hello tasks presented in this walkthrough.</span></span> <span data-ttu-id="348e4-108">Användare som vill som den här metoden bör kontakta tootry hello [Criteo genomgången använder en Hive ODBC-anslutning](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="348e4-108">Users who would like tootry this approach should consult hello [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="348e4-109"><a name="dataset"></a>Criteo Dataset-beskrivning</span><span class="sxs-lookup"><span data-stu-id="348e4-109"><a name="dataset"></a>Criteo Dataset Description</span></span>
<span data-ttu-id="348e4-110">Hej Criteo data är en Klicka förutsägelse datamängd som är cirka 370GB gzip komprimerade TVS-filer (~1.3TB okomprimerade), som består av fler än 4.3 miljard poster.</span><span class="sxs-lookup"><span data-stu-id="348e4-110">hello Criteo data is a click prediction dataset that is approximately 370GB of gzip compressed TSV files (~1.3TB uncompressed), comprising more than 4.3 billion records.</span></span> <span data-ttu-id="348e4-111">Det hämtas från 24 dagar i klickar du på data som har gjorts tillgängliga av [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span><span class="sxs-lookup"><span data-stu-id="348e4-111">It is taken from 24 days of click data made available by [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span></span> <span data-ttu-id="348e4-112">I informationssyfte hello data forskare, vi har uppackade data tillgängliga toous tooexperiment med.</span><span class="sxs-lookup"><span data-stu-id="348e4-112">For hello convenience of data scientists, we have unzipped data available toous tooexperiment with.</span></span>

<span data-ttu-id="348e4-113">Varje post i den här datauppsättningen innehåller 40 kolumner:</span><span class="sxs-lookup"><span data-stu-id="348e4-113">Each record in this dataset contains 40 columns:</span></span>

* <span data-ttu-id="348e4-114">hello första kolumnen är en etikett-kolumn som anger om en användare klickar på en **lägga till** (värdet 1) eller inte på en (värde 0)</span><span class="sxs-lookup"><span data-stu-id="348e4-114">hello first column is a label column that indicates whether a user clicks an **add** (value 1) or does not click one (value 0)</span></span>
* <span data-ttu-id="348e4-115">Nästa 13 kolumner är numeriska, och</span><span class="sxs-lookup"><span data-stu-id="348e4-115">next 13 columns are numeric, and</span></span>
* <span data-ttu-id="348e4-116">senaste 26 är kategoriska kolumner</span><span class="sxs-lookup"><span data-stu-id="348e4-116">last 26 are categorical columns</span></span>

<span data-ttu-id="348e4-117">hello-kolumner är anonym och använder en serie av uppräknade namn: ”Kol1” (för hello etikett kolumn) för ' Col40 ”(för hello senaste kategoriska kolumn).</span><span class="sxs-lookup"><span data-stu-id="348e4-117">hello columns are anonymized and use a series of enumerated names: "Col1" (for hello label column) too'Col40" (for hello last categorical column).</span></span>            

<span data-ttu-id="348e4-118">Här är ett utdrag ur hello först 20 kolumner med två observationer (rader) från den här datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="348e4-118">Here is an excerpt of hello first 20 columns of two observations (rows) from this dataset:</span></span>

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

<span data-ttu-id="348e4-119">Det finns värden som saknas i båda hello numeriska och kategoriska kolumnerna i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="348e4-119">There are missing values in both hello numeric and categorical columns in this dataset.</span></span> <span data-ttu-id="348e4-120">Vi beskriver en enkel metod för att hantera hello värden som saknas.</span><span class="sxs-lookup"><span data-stu-id="348e4-120">We describe a simple method for handling hello missing values.</span></span> <span data-ttu-id="348e4-121">Ytterligare information om hello data beskrivs när vi lagrar dem i Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="348e4-121">Additional details of hello data are explored when we store them into Hive tables.</span></span>

<span data-ttu-id="348e4-122">**Definition:** *klickningar (CTR):* detta är hello procent klick i hello data.</span><span class="sxs-lookup"><span data-stu-id="348e4-122">**Definition:** *Clickthrough rate (CTR):* This is hello percentage of clicks in hello data.</span></span> <span data-ttu-id="348e4-123">I den här Criteo datauppsättningen är hello CTR ungefär 3.3% eller 0.033.</span><span class="sxs-lookup"><span data-stu-id="348e4-123">In this Criteo dataset, hello CTR is about 3.3% or 0.033.</span></span>

## <span data-ttu-id="348e4-124"><a name="mltasks"></a>Exempel på förutsägelse uppgifter</span><span class="sxs-lookup"><span data-stu-id="348e4-124"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="348e4-125">Två exempel förutsägelse problem åtgärdas i den här genomgången:</span><span class="sxs-lookup"><span data-stu-id="348e4-125">Two sample prediction problems are addressed in this walkthrough:</span></span>

1. <span data-ttu-id="348e4-126">**Binär klassificering**: beräknar om en användare klickar på en Lägg till:</span><span class="sxs-lookup"><span data-stu-id="348e4-126">**Binary classification**: Predicts whether a user clicked an add:</span></span>
   
   * <span data-ttu-id="348e4-127">Klass 0: Ingen klickar du på</span><span class="sxs-lookup"><span data-stu-id="348e4-127">Class 0: No Click</span></span>
   * <span data-ttu-id="348e4-128">Klass 1: Klicka på</span><span class="sxs-lookup"><span data-stu-id="348e4-128">Class 1: Click</span></span>
2. <span data-ttu-id="348e4-129">**Regression**: beräknar hello sannolikheten för en ad Klicka på funktioner för användaren.</span><span class="sxs-lookup"><span data-stu-id="348e4-129">**Regression**: Predicts hello probability of an ad click from user features.</span></span>

## <span data-ttu-id="348e4-130"><a name="setup"></a>Ange ett HDInsight Hadoop-kluster för datavetenskap</span><span class="sxs-lookup"><span data-stu-id="348e4-130"><a name="setup"></a>Set Up an HDInsight Hadoop cluster for data science</span></span>
<span data-ttu-id="348e4-131">**Obs:** detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="348e4-131">**Note:** This is typically an **Admin** task.</span></span>

<span data-ttu-id="348e4-132">Ställ in Azure datavetenskap miljön för att skapa förutsägelseanalyslösningar med HDInsight-kluster i tre steg:</span><span class="sxs-lookup"><span data-stu-id="348e4-132">Set up your Azure Data Science environment for building predictive analytics solutions with HDInsight clusters in three steps:</span></span>

1. <span data-ttu-id="348e4-133">[Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md): det här lagringskontot är används toostore data i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="348e4-133">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used toostore data in Azure Blob Storage.</span></span> <span data-ttu-id="348e4-134">Här lagras hello data används i HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="348e4-134">hello data used in HDInsight clusters is stored here.</span></span>
2. <span data-ttu-id="348e4-135">[Anpassa Azure HDInsight Hadoop-kluster för datavetenskap](machine-learning-data-science-customize-hadoop-cluster.md): det här steget skapar ett Azure HDInsight Hadoop-kluster med 64-bitars Anaconda Python 2.7 installerad på alla noder.</span><span class="sxs-lookup"><span data-stu-id="348e4-135">[Customize Azure HDInsight Hadoop Clusters for Data Science](machine-learning-data-science-customize-hadoop-cluster.md): This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="348e4-136">Det finns två viktiga steg (beskrivs i det här avsnittet) toocomplete när du anpassar hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="348e4-136">There are two important steps (described in this topic) toocomplete when customizing hello HDInsight cluster.</span></span>
   
   * <span data-ttu-id="348e4-137">Du måste länka hello storage-konto som skapades i steg 1 med ditt HDInsight-kluster när den skapas.</span><span class="sxs-lookup"><span data-stu-id="348e4-137">You must link hello storage account created in step 1 with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="348e4-138">Det här lagringskontot används för att komma åt data som kan bearbetas i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="348e4-138">This storage account is used for accessing data that can be processed within hello cluster.</span></span>
   * <span data-ttu-id="348e4-139">Du måste aktivera fjärråtkomst toohello huvudnod hello klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="348e4-139">You must enable Remote Access toohello head node of hello cluster after it is created.</span></span> <span data-ttu-id="348e4-140">Kom ihåg hello fjärråtkomst autentiseringsuppgifter du anger här (skiljer sig från de som anges för hello klustret när skapades): du behöver toocomplete hello följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="348e4-140">Remember hello remote access credentials you specify here (different from those specified for hello cluster at its creation): you need them toocomplete hello following procedures.</span></span>
3. <span data-ttu-id="348e4-141">[Skapa en arbetsyta för Azure ML](machine-learning-create-workspace.md): den här Azure Machine Learning-arbetsytan används för att skapa machine learning-modeller efter en inledande datagranskning och ned provtagning på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="348e4-141">[Create an Azure ML workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used for building machine learning models after an initial data exploration and down sampling on hello HDInsight cluster.</span></span>

## <span data-ttu-id="348e4-142"><a name="getdata"></a>Hämta och använda data från en offentlig källa</span><span class="sxs-lookup"><span data-stu-id="348e4-142"><a name="getdata"></a>Get and consume data from a public source</span></span>
<span data-ttu-id="348e4-143">Hej [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset kan nås genom att klicka på hello länk, acceptera hello villkor för användning och tillhandahåller ett namn.</span><span class="sxs-lookup"><span data-stu-id="348e4-143">hello [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset can be accessed by clicking on hello link, accepting hello terms of use, and providing a name.</span></span> <span data-ttu-id="348e4-144">En ögonblicksbild av hur det ser ut visas här:</span><span class="sxs-lookup"><span data-stu-id="348e4-144">A snapshot of what this looks like is shown here:</span></span>

![Acceptera Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

<span data-ttu-id="348e4-146">Klicka på **Fortsätt tooDownload** tooread mer om hello datauppsättningen och dess tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="348e4-146">Click **Continue tooDownload** tooread more about hello dataset and its availability.</span></span>

<span data-ttu-id="348e4-147">hello data finns i en offentlig [Azure-blobblagring](../storage/blobs/storage-dotnet-how-to-use-blobs.md) plats: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span><span class="sxs-lookup"><span data-stu-id="348e4-147">hello data resides in a public [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) location: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span></span> <span data-ttu-id="348e4-148">Hej ”wasb” refererar tooAzure Blob-lagringsplats.</span><span class="sxs-lookup"><span data-stu-id="348e4-148">hello "wasb" refers tooAzure Blob Storage location.</span></span> 

1. <span data-ttu-id="348e4-149">hello data i den här offentliga blob storage består av tre undermappar uppackade data.</span><span class="sxs-lookup"><span data-stu-id="348e4-149">hello data in this public blob storage consists of three subfolders of unzipped data.</span></span>
   
   1. <span data-ttu-id="348e4-150">Hej undermapp *raw /-beräkning/* innehåller hello första 21 dagars data - från dag\_00 tooday\_20</span><span class="sxs-lookup"><span data-stu-id="348e4-150">hello subfolder *raw/count/* contains hello first 21 days of data - from day\_00 tooday\_20</span></span>
   2. <span data-ttu-id="348e4-151">Hej undermapp *raw-tåg/* består av en dag av data, dag\_21</span><span class="sxs-lookup"><span data-stu-id="348e4-151">hello subfolder *raw/train/* consists of a single day of data, day\_21</span></span>
   3. <span data-ttu-id="348e4-152">Hej undermapp *rådata/test/* består av två dagars data, dag\_22 och dag\_23</span><span class="sxs-lookup"><span data-stu-id="348e4-152">hello subfolder *raw/test/* consists of two days of data, day\_22 and day\_23</span></span>
2. <span data-ttu-id="348e4-153">För de som vill toostart med hello rådata gzip data, de är också tillgängliga i hello huvudsakliga mappen *rådata /* som day_NN.gz, där NN går från 00 too23.</span><span class="sxs-lookup"><span data-stu-id="348e4-153">For those who want toostart with hello raw gzip data, these are also available in hello main folder *raw/* as day_NN.gz, where NN goes from 00 too23.</span></span>

<span data-ttu-id="348e4-154">En annan metod tooaccess utforska och modellerar data som inte kräver någon lokal hämtningar beskrivs senare i den här genomgången när vi skapar Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="348e4-154">An alternative approach tooaccess, explore, and model this data that does not require any local downloads is explained later in this walkthrough when we create Hive tables.</span></span>

## <span data-ttu-id="348e4-155"><a name="login"></a>Logga in toohello klustret headnode</span><span class="sxs-lookup"><span data-stu-id="348e4-155"><a name="login"></a>Log in toohello cluster headnode</span></span>
<span data-ttu-id="348e4-156">toolog i toohello headnode i hello kluster, Använd hello [Azure-portalen](https://ms.portal.azure.com) toolocate hello klustret.</span><span class="sxs-lookup"><span data-stu-id="348e4-156">toolog in toohello headnode of hello cluster, use hello [Azure portal](https://ms.portal.azure.com) toolocate hello cluster.</span></span> <span data-ttu-id="348e4-157">Klicka på hello HDInsight Elefant ikon på hello kvar och dubbelklicka på hello namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="348e4-157">Click hello HDInsight elephant icon on hello left and then double-click hello name of your cluster.</span></span> <span data-ttu-id="348e4-158">Navigera toohello **Configuration** fliken, klicka på ikonen hello Anslut på hello längst ned på sidan för hello och ange dina autentiseringsuppgifter för fjärråtkomst när du uppmanas.</span><span class="sxs-lookup"><span data-stu-id="348e4-158">Navigate toohello **Configuration** tab, double-click hello CONNECT icon on hello bottom of hello page, and enter your remote access credentials when prompted.</span></span> <span data-ttu-id="348e4-159">Då kommer du toohello headnode hello-klustret.</span><span class="sxs-lookup"><span data-stu-id="348e4-159">This takes you toohello headnode of hello cluster.</span></span>

<span data-ttu-id="348e4-160">Här är en typisk första inloggningen toohello klustret headnode ser ut:</span><span class="sxs-lookup"><span data-stu-id="348e4-160">Here is what a typical first log in toohello cluster headnode looks like:</span></span>

![Logga in toocluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

<span data-ttu-id="348e4-162">Vi kan se hello ”Hadoop kommandoraden”, som våra bestämmer hög grad för hello datagranskning hello vänster.</span><span class="sxs-lookup"><span data-stu-id="348e4-162">On hello left, we see hello "Hadoop Command Line", which is our workhorse for hello data exploration.</span></span> <span data-ttu-id="348e4-163">Vi kan också se två användbara webbadresserna - ”Hadoop Yarn Status” och ”Hadoop namn nod”.</span><span class="sxs-lookup"><span data-stu-id="348e4-163">We also see two useful URLs - "Hadoop Yarn Status" and "Hadoop Name Node".</span></span> <span data-ttu-id="348e4-164">Hej yarn status URL visar jobbförloppet och hello namn nod URL ger information om hello klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="348e4-164">hello yarn status URL shows job progress and hello name node URL gives details on hello cluster configuration.</span></span>

<span data-ttu-id="348e4-165">Nu vi ställs in och klara toobegin första delen av genomgången hello: datagranskning med Hive och förbereda data för Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="348e4-165">Now we are set up and ready toobegin first part of hello walkthrough: data exploration using Hive and getting data ready for Azure Machine Learning.</span></span>

## <span data-ttu-id="348e4-166"><a name="hive-db-tables"></a>Skapa Hive-databasen och tabeller</span><span class="sxs-lookup"><span data-stu-id="348e4-166"><a name="hive-db-tables"></a> Create Hive database and tables</span></span>
<span data-ttu-id="348e4-167">toocreate Hive-tabeller för våra Criteo dataset, öppna hello ***Hadoop kommandoraden*** på hello skrivbord hello huvudnod och ange hello Hive katalogen genom att ange hello kommando</span><span class="sxs-lookup"><span data-stu-id="348e4-167">toocreate Hive tables for our Criteo dataset, open hello ***Hadoop Command Line*** on hello desktop of hello head node, and enter hello Hive directory by entering hello command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="348e4-168">Kör alla Hive-kommandon i den här genomgången från hello Hive bin / directory-fråga.</span><span class="sxs-lookup"><span data-stu-id="348e4-168">Run all Hive commands in this walkthrough from hello Hive bin/ directory prompt.</span></span> <span data-ttu-id="348e4-169">Detta hand tar om eventuella problem med sökväg automatiskt.</span><span class="sxs-lookup"><span data-stu-id="348e4-169">This takes care of any path issues automatically.</span></span> <span data-ttu-id="348e4-170">Vi använder hello termer ”Hive directory prompt” ”, Hive bin / directory prompt”, och ”Hadoop-kommandoraden” synonymt.</span><span class="sxs-lookup"><span data-stu-id="348e4-170">We use hello terms "Hive directory prompt", "Hive bin/ directory prompt", and "Hadoop Command Line" interchangeably.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="348e4-171">tooexecute Hive-fråga kan alltid använda hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="348e4-171">tooexecute any Hive query, one can always use hello following commands:</span></span>
> 
> 

        cd %hive_home%\bin
        hive

<span data-ttu-id="348e4-172">Efter hello Hive REPL visas med en ”hive >” Logga, bara klippa och klistra in hello frågan tooexecute den.</span><span class="sxs-lookup"><span data-stu-id="348e4-172">After hello Hive REPL appears with a "hive >"sign, simply cut and paste hello query tooexecute it.</span></span>

<span data-ttu-id="348e4-173">hello följande kod skapar en databas ”criteo” och sedan genererar 4 tabeller:</span><span class="sxs-lookup"><span data-stu-id="348e4-173">hello following code creates a database "criteo" and then generates 4 tables:</span></span>

* <span data-ttu-id="348e4-174">en *tabell för generering av antal* bygger på dagar dag\_00 tooday\_20,</span><span class="sxs-lookup"><span data-stu-id="348e4-174">a *table for generating counts* built on days day\_00 tooday\_20,</span></span>
* <span data-ttu-id="348e4-175">en *tabell för användning som hello train dataset* bygger på dag\_21, och</span><span class="sxs-lookup"><span data-stu-id="348e4-175">a *table for use as hello train dataset* built on day\_21, and</span></span>
* <span data-ttu-id="348e4-176">två *tabeller som hello testa datauppsättningar* bygger på dag\_22 och dag\_23 respektive.</span><span class="sxs-lookup"><span data-stu-id="348e4-176">two *tables for use as hello test datasets* built on day\_22 and day\_23 respectively.</span></span>

<span data-ttu-id="348e4-177">Vi dela vår testdata i två olika tabeller eftersom en av hello dagar är helgdagar, och vi vill toodetermine om hello modell kan identifiera skillnader mellan en helgdag och icke-helgdagar från hello klickningar.</span><span class="sxs-lookup"><span data-stu-id="348e4-177">We split our test dataset into two different tables because one of hello days is a holiday, and we want toodetermine if hello model can detect differences between a holiday and non-holiday from hello clickthrough rate.</span></span>

<span data-ttu-id="348e4-178">hello skript [exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) visas här informationssyfte:</span><span class="sxs-lookup"><span data-stu-id="348e4-178">hello script [sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) is displayed here for convenience:</span></span>

    CREATE DATABASE IF NOT EXISTS criteo;
    DROP TABLE IF EXISTS criteo.criteo_count;
    CREATE TABLE criteo.criteo_count (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/count';

    DROP TABLE IF EXISTS criteo.criteo_train;
    CREATE TABLE criteo.criteo_train (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/train';

    DROP TABLE IF EXISTS criteo.criteo_test_day_22;
    CREATE TABLE criteo.criteo_test_day_22 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_22';

    DROP TABLE IF EXISTS criteo.criteo_test_day_23;
    CREATE TABLE criteo.criteo_test_day_23 (
    col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
    ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
    LINES TERMINATED BY '\n'
    STORED AS TEXTFILE LOCATION 'wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/test/day_23';

<span data-ttu-id="348e4-179">Vi Observera att dessa tabeller är externa som vi helt enkelt punkt tooAzure Blob Storage (wasb)-platser.</span><span class="sxs-lookup"><span data-stu-id="348e4-179">We note that all these tables are external as we simply point tooAzure Blob Storage (wasb) locations.</span></span>

<span data-ttu-id="348e4-180">**Det finns två sätt tooexecute alla Hive-fråga som vi nämnt nu.**</span><span class="sxs-lookup"><span data-stu-id="348e4-180">**There are two ways tooexecute ANY Hive query that we now mention.**</span></span>

1. <span data-ttu-id="348e4-181">**Med hjälp av hello Hive REPL kommandoradsverktyget**: hello först är tooissue en ”hive” kommandot och kopiera och klistra in en fråga på hello Hive REPL-kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="348e4-181">**Using hello Hive REPL command-line**: hello first is tooissue a "hive" command and copy and paste a query at hello Hive REPL command-line.</span></span> <span data-ttu-id="348e4-182">toodo detta, vill:</span><span class="sxs-lookup"><span data-stu-id="348e4-182">toodo this, do:</span></span>
   
        cd %hive_home%\bin
        hive
   
     <span data-ttu-id="348e4-183">Nu på hello REPL kommandoradsverktyg, kopiera och klistra in körs hello fråga den.</span><span class="sxs-lookup"><span data-stu-id="348e4-183">Now at hello REPL command-line, cutting and pasting hello query executes it.</span></span>
2. <span data-ttu-id="348e4-184">**Spara frågor tooa fil- och köra hello kommando**: hello är andra toosave hello frågor tooa .hql fil ([exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) och sedan problemet hello följande kommando tooexecute hello fråga:</span><span class="sxs-lookup"><span data-stu-id="348e4-184">**Saving queries tooa file and executing hello command**: hello second is toosave hello queries tooa .hql file ([sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) and then issue hello following command tooexecute hello query:</span></span>
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a><span data-ttu-id="348e4-185">Bekräfta databas och tabell skapas</span><span class="sxs-lookup"><span data-stu-id="348e4-185">Confirm database and table creation</span></span>
<span data-ttu-id="348e4-186">Därefter vi bekräfta hello skapandet av hello databasen med följande kommando från hello Hive bin hello / directory prompten:</span><span class="sxs-lookup"><span data-stu-id="348e4-186">Next, we confirm hello creation of hello database with hello following command from hello Hive bin/ directory prompt:</span></span>

        hive -e "show databases;"

<span data-ttu-id="348e4-187">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-187">This gives:</span></span>

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

<span data-ttu-id="348e4-188">Det här bekräftar hello skapandet av hello ny databas, ”criteo”.</span><span class="sxs-lookup"><span data-stu-id="348e4-188">This confirms hello creation of hello new database, "criteo".</span></span>

<span data-ttu-id="348e4-189">toosee vilka tabeller som vi skapade vi bara utfärda hello kommando här från hello Hive bin / directory prompten:</span><span class="sxs-lookup"><span data-stu-id="348e4-189">toosee what tables we created, we simply issue hello command here from hello Hive bin/ directory prompt:</span></span>

        hive -e "show tables in criteo;"

<span data-ttu-id="348e4-190">Vi kan sedan se hello följande utdata:</span><span class="sxs-lookup"><span data-stu-id="348e4-190">We then see hello following output:</span></span>

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <span data-ttu-id="348e4-191"><a name="exploration"></a>Datagranskning i Hive</span><span class="sxs-lookup"><span data-stu-id="348e4-191"><a name="exploration"></a> Data exploration in Hive</span></span>
<span data-ttu-id="348e4-192">Nu är du redo toodo vissa grundläggande datagranskning i Hive.</span><span class="sxs-lookup"><span data-stu-id="348e4-192">Now we are ready toodo some basic data exploration in Hive.</span></span> <span data-ttu-id="348e4-193">Vi börjar med inventering hello antalet exempel i hello tåg och testa datatabeller.</span><span class="sxs-lookup"><span data-stu-id="348e4-193">We begin by counting hello number of examples in hello train and test data tables.</span></span>

### <a name="number-of-train-examples"></a><span data-ttu-id="348e4-194">Antal train-exempel</span><span class="sxs-lookup"><span data-stu-id="348e4-194">Number of train examples</span></span>
<span data-ttu-id="348e4-195">Hej innehållet i [exempel &#95; hive &#95; antal &#95; train &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) visas här:</span><span class="sxs-lookup"><span data-stu-id="348e4-195">hello contents of [sample&#95;hive&#95;count&#95;train&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) are shown here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_train;

<span data-ttu-id="348e4-196">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-196">This yields:</span></span>

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

<span data-ttu-id="348e4-197">Alternativt kan en också utfärda hello följande kommando från hello Hive bin / directory prompten:</span><span class="sxs-lookup"><span data-stu-id="348e4-197">Alternatively, one may also issue hello following command from hello Hive bin/ directory prompt:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-hello-two-test-datasets"></a><span data-ttu-id="348e4-198">Antal test exemplen i hello två test datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="348e4-198">Number of test examples in hello two test datasets</span></span>
<span data-ttu-id="348e4-199">Vi nu antal hello exemplen i hello två test datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="348e4-199">We now count hello number of examples in hello two test datasets.</span></span> <span data-ttu-id="348e4-200">Hej innehållet i [exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 22 &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) finns här:</span><span class="sxs-lookup"><span data-stu-id="348e4-200">hello contents of [sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;22&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) are here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

<span data-ttu-id="348e4-201">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-201">This yields:</span></span>

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

<span data-ttu-id="348e4-202">Vi kan som vanligt också anropa hello skript från hello Hive bin / directory fråga genom att utfärda hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="348e4-202">As usual, we may also call hello script from hello Hive bin/ directory prompt by issuing hello command:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

<span data-ttu-id="348e4-203">Slutligen kan vi se hello många test exemplen i hello testdata baserat på dag\_23.</span><span class="sxs-lookup"><span data-stu-id="348e4-203">Finally, we examine hello number of test examples in hello test dataset based on day\_23.</span></span>

<span data-ttu-id="348e4-204">hello kommandot toodo är liknande toohello som bara visas (se för[exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span><span class="sxs-lookup"><span data-stu-id="348e4-204">hello command toodo this is similar toohello one just shown (refer too[sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;23&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

<span data-ttu-id="348e4-205">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-205">This gives:</span></span>

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-hello-train-dataset"></a><span data-ttu-id="348e4-206">Etikett distribution i hello train datauppsättning</span><span class="sxs-lookup"><span data-stu-id="348e4-206">Label distribution in hello train dataset</span></span>
<span data-ttu-id="348e4-207">hello etikett distribution i hello train dataset är av intresse.</span><span class="sxs-lookup"><span data-stu-id="348e4-207">hello label distribution in hello train dataset is of interest.</span></span> <span data-ttu-id="348e4-208">toosee kan vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; etikett &#95; distri &#95; train &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span><span class="sxs-lookup"><span data-stu-id="348e4-208">toosee this, we show contents of [sample&#95;hive&#95;criteo&#95;label&#95;distribution&#95;train&#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span></span>

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

<span data-ttu-id="348e4-209">Detta ger hello etikett distribution:</span><span class="sxs-lookup"><span data-stu-id="348e4-209">This yields hello label distribution:</span></span>

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

<span data-ttu-id="348e4-210">Observera att hello procentandelen positivt etiketter är ungefär 3.3% (konsekvent med hello ursprungliga datauppsättningen).</span><span class="sxs-lookup"><span data-stu-id="348e4-210">Note that hello percentage of positive labels is about 3.3% (consistent with hello original dataset).</span></span>

### <a name="histogram-distributions-of-some-numeric-variables-in-hello-train-dataset"></a><span data-ttu-id="348e4-211">Histogram distributioner av vissa numeriska variabler i hello träna dataset</span><span class="sxs-lookup"><span data-stu-id="348e4-211">Histogram distributions of some numeric variables in hello train dataset</span></span>
<span data-ttu-id="348e4-212">Vi kan använda registreringsdata intern ”histogram\_numeriska” toofind reda på vilka hello distribution av hello numeriska variabler som ser ut att fungera.</span><span class="sxs-lookup"><span data-stu-id="348e4-212">We can use Hive's native "histogram\_numeric" function toofind out what hello distribution of hello numeric variables looks like.</span></span> <span data-ttu-id="348e4-213">Här följer hello innehållet i [exempel &#95; hive &#95; criteo &#95; histogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span><span class="sxs-lookup"><span data-stu-id="348e4-213">Here are hello contents of [sample&#95;hive&#95;criteo&#95;histogram&#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span></span>

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

<span data-ttu-id="348e4-214">Detta ger hello följande:</span><span class="sxs-lookup"><span data-stu-id="348e4-214">This yields hello following:</span></span>

        26      155878415
        2606    92753
        6755    22086
        11202   6922
        14432   4163
        17815   2488
        21072   1901
        24113   1283
        27429   1225
        30818   906
        34512   723
        38026   387
        41007   290
        43417   312
        45797   571
        49819   428
        53505   328
        56853   527
        61004   160
        65510   3446
        Time taken: 317.851 seconds, Fetched: 20 row(s)

<span data-ttu-id="348e4-215">hello LATERALA Visa - Expandera kombination i Hive fungerar tooproduce en SQL-liknande utdata i stället för vanliga hello-listan.</span><span class="sxs-lookup"><span data-stu-id="348e4-215">hello LATERAL VIEW - explode combination in Hive serves tooproduce a SQL-like output instead of hello usual list.</span></span> <span data-ttu-id="348e4-216">Observera att i hello denna tabell, hello första kolumnen motsvarar toohello bin center och hello andra toohello bin frekvens.</span><span class="sxs-lookup"><span data-stu-id="348e4-216">Note that in hello this table, hello first column corresponds toohello bin center and hello second toohello bin frequency.</span></span>

### <a name="approximate-percentiles-of-some-numeric-variables-in-hello-train-dataset"></a><span data-ttu-id="348e4-217">Ungefärlig percentiler av vissa numeriska variabler i hello träna dataset</span><span class="sxs-lookup"><span data-stu-id="348e4-217">Approximate percentiles of some numeric variables in hello train dataset</span></span>
<span data-ttu-id="348e4-218">Är också hello beräkning av ungefärliga percentiler intressanta med numeriska variabler.</span><span class="sxs-lookup"><span data-stu-id="348e4-218">Also of interest with numeric variables is hello computation of approximate percentiles.</span></span> <span data-ttu-id="348e4-219">Hive datorns inbyggda ”percentil\_ungefärlig” matchar det för oss.</span><span class="sxs-lookup"><span data-stu-id="348e4-219">Hive's native "percentile\_approx" does this for us.</span></span> <span data-ttu-id="348e4-220">Hej innehållet i [exempel &#95; hive &#95; criteo &#95; ungefärliga &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) är:</span><span class="sxs-lookup"><span data-stu-id="348e4-220">hello contents of [sample&#95;hive&#95;criteo&#95;approximate&#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) are:</span></span>

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

<span data-ttu-id="348e4-221">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-221">This yields:</span></span>

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

<span data-ttu-id="348e4-222">Vi Markera hello distribution av percentiler är nära relaterade toohello histogram distribution av en numerisk variabel vanligtvis.</span><span class="sxs-lookup"><span data-stu-id="348e4-222">We remark that hello distribution of percentiles is closely related toohello histogram distribution of any numeric variable usually.</span></span>         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-hello-train-dataset"></a><span data-ttu-id="348e4-223">Hitta antalet unika värden för vissa kategoriska kolumner i hello train datauppsättning</span><span class="sxs-lookup"><span data-stu-id="348e4-223">Find number of unique values for some categorical columns in hello train dataset</span></span>
<span data-ttu-id="348e4-224">Fortsätter hello datagranskning vi nu finns för vissa kategoriska kolumner hello antalet unika värden som de vidtar.</span><span class="sxs-lookup"><span data-stu-id="348e4-224">Continuing hello data exploration, we now find, for some categorical columns, hello number of unique values they take.</span></span> <span data-ttu-id="348e4-225">toodo kan vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span><span class="sxs-lookup"><span data-stu-id="348e4-225">toodo this, we show contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span></span>

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

<span data-ttu-id="348e4-226">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-226">This yields:</span></span>

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

<span data-ttu-id="348e4-227">Vi Observera att Col15 19M unika värden!</span><span class="sxs-lookup"><span data-stu-id="348e4-227">We note that Col15 has 19M unique values!</span></span> <span data-ttu-id="348e4-228">Använda naïve tekniker som ”en-hot kodning” tooencode sådana hög endimensionell kategoriska variabler är omöjligt.</span><span class="sxs-lookup"><span data-stu-id="348e4-228">Using naive techniques like "one-hot encoding" tooencode such high-dimensional categorical variables is infeasible.</span></span> <span data-ttu-id="348e4-229">I synnerhet Vi förklarar och visar en kraftfull, robust teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) för att effektivt hantera problemet.</span><span class="sxs-lookup"><span data-stu-id="348e4-229">In particular, we explain and demonstrate a powerful, robust technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) for tackling this problem efficiently.</span></span>

<span data-ttu-id="348e4-230">Vi avbryta den här underavsnittet genom att titta på hello antalet unika värden för vissa andra kategoriska kolumner samt.</span><span class="sxs-lookup"><span data-stu-id="348e4-230">We end this sub-section by looking at hello number of unique values for some other categorical columns as well.</span></span> <span data-ttu-id="348e4-231">Hej innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95; flera &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) är:</span><span class="sxs-lookup"><span data-stu-id="348e4-231">hello contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;multiple&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) are:</span></span>

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

<span data-ttu-id="348e4-232">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-232">This yields:</span></span>

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

<span data-ttu-id="348e4-233">Igen ser vi att förutom Col20, alla hello andra kolumner har många unika värden.</span><span class="sxs-lookup"><span data-stu-id="348e4-233">Again we see that except for Col20, all hello other columns have many unique values.</span></span>

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-hello-train-dataset"></a><span data-ttu-id="348e4-234">Antal samtidigt förekomsten av par med kategoriska variabler i hello träna dataset</span><span class="sxs-lookup"><span data-stu-id="348e4-234">Co-occurrence counts of pairs of categorical variables in hello train dataset</span></span>

<span data-ttu-id="348e4-235">hello samtidigt förekomsten antal par kategoriska variabler är också av intresse.</span><span class="sxs-lookup"><span data-stu-id="348e4-235">hello co-occurrence counts of pairs of categorical variables is also of interest.</span></span> <span data-ttu-id="348e4-236">Detta kan fastställas med hjälp av hello koden i [exempel &#95; hive &#95; criteo &#95; parad &#95; kategoriska &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span><span class="sxs-lookup"><span data-stu-id="348e4-236">This can be determined using hello code in [sample&#95;hive&#95;criteo&#95;paired&#95;categorical&#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span></span>

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

<span data-ttu-id="348e4-237">Vi omvänd ordning hello antal av deras förekomst och titta på hello högsta 15 i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="348e4-237">We reverse order hello counts by their occurrence and look at hello top 15 in this case.</span></span> <span data-ttu-id="348e4-238">Detta ger oss:</span><span class="sxs-lookup"><span data-stu-id="348e4-238">This gives us:</span></span>

        ad98e872        cea68cd3        8964458
        ad98e872        3dbb483e        8444762
        ad98e872        43ced263        3082503
        ad98e872        420acc05        2694489
        ad98e872        ac4c5591        2559535
        ad98e872        fb1e95da        2227216
        ad98e872        8af1edc8        1794955
        ad98e872        e56937ee        1643550
        ad98e872        d1fade1c        1348719
        ad98e872        977b4431        1115528
        e5f3fd8d        a15d1051        959252
        ad98e872        dd86c04a        872975
        349b3fec        a52ef97d        821062
        e5f3fd8d        a0aaffa6        792250
        265366bf        6f5c7c41        782142
        Time taken: 560.22 seconds, Fetched: 15 row(s)

## <span data-ttu-id="348e4-239"><a name="downsample"></a>Ned hello provdatauppsättningar för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="348e4-239"><a name="downsample"></a> Down sample hello datasets for Azure Machine Learning</span></span>
<span data-ttu-id="348e4-240">Med utforskade hello datauppsättningar och visas hur vi kan göra den här typen av exploatera några variabler (inklusive kombinationer), vi nu ned hello provdatauppsättningar så att vi kan bygga modeller i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="348e4-240">Having explored hello datasets and demonstrated how we may do this type of exploration for any variables (including combinations), we now down sample hello data sets so that we can build models in Azure Machine Learning.</span></span> <span data-ttu-id="348e4-241">Återkalla åtgärdas hello vi fokusera på är: en uppsättning exempel attribut (funktionen värden från Col2 - Col40) får vi förutsäga om Kol1 är 0 (ingen klicka) eller 1 (klicka).</span><span class="sxs-lookup"><span data-stu-id="348e4-241">Recall that hello problem we focus on is: given a set of example attributes (feature values from Col2 - Col40), we predict if Col1 is a 0 (no click) or a 1 (click).</span></span>

<span data-ttu-id="348e4-242">toodown exempel våra tåg och testa datauppsättningar too1% av hello ursprungliga storlek, vi använder registreringsdata interna RAND() funktion.</span><span class="sxs-lookup"><span data-stu-id="348e4-242">toodown sample our train and test datasets too1% of hello original size, we use Hive's native RAND() function.</span></span> <span data-ttu-id="348e4-243">Hej nästa skript [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; train &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) gör detta för hello train datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="348e4-243">hello next script, [sample&#95;hive&#95;criteo&#95;downsample&#95;train&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) does this for hello train dataset:</span></span>

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

<span data-ttu-id="348e4-244">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-244">This yields:</span></span>

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

<span data-ttu-id="348e4-245">Hej skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) matchar för testdata, dag\_22:</span><span class="sxs-lookup"><span data-stu-id="348e4-245">hello script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;22&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) does it for test data, day\_22:</span></span>

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

<span data-ttu-id="348e4-246">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-246">This yields:</span></span>

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


<span data-ttu-id="348e4-247">Slutligen hello skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) matchar för testdata, dag\_23:</span><span class="sxs-lookup"><span data-stu-id="348e4-247">Finally, hello script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;23&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) does it for test data, day\_23:</span></span>

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

<span data-ttu-id="348e4-248">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="348e4-248">This yields:</span></span>

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

<span data-ttu-id="348e4-249">Med detta kan vi är klar toouse våra ned provtagning träna och testa datauppsättningar för att skapa modeller i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="348e4-249">With this, we are ready toouse our down sampled train and test datasets for building models in Azure Machine Learning.</span></span>

<span data-ttu-id="348e4-250">Det finns en sista viktig komponent innan vi vidare tooAzure Maskininlärning är av säkerhetsskäl hello antal tabell.</span><span class="sxs-lookup"><span data-stu-id="348e4-250">There is a final important component before we move on tooAzure Machine Learning, which is concerns hello count table.</span></span> <span data-ttu-id="348e4-251">I hello nästa underavsnitt diskuterar vi detta i viss detalj.</span><span class="sxs-lookup"><span data-stu-id="348e4-251">In hello next sub-section, we discuss this in some detail.</span></span>

## <span data-ttu-id="348e4-252"><a name="count"></a>En kort beskrivning på hello antal tabell</span><span class="sxs-lookup"><span data-stu-id="348e4-252"><a name="count"></a> A brief discussion on hello count table</span></span>
<span data-ttu-id="348e4-253">Som vi såg har flera kategoriska variabler en mycket hög dimensionalitet.</span><span class="sxs-lookup"><span data-stu-id="348e4-253">As we saw, several categorical variables have a very high dimensionality.</span></span> <span data-ttu-id="348e4-254">I vår genomgången presenterar vi en kraftfull teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode dessa variabler på ett effektivt och robust sätt.</span><span class="sxs-lookup"><span data-stu-id="348e4-254">In our walkthrough, we present a powerful technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) tooencode these variables in an efficient, robust manner.</span></span> <span data-ttu-id="348e4-255">Mer information om den här tekniken finns i länken hello.</span><span class="sxs-lookup"><span data-stu-id="348e4-255">More information on this technique is in hello link provided.</span></span>

[!NOTE]
><span data-ttu-id="348e4-256">I den här genomgången ska fokusera vi på med antal tabeller tooproduce compact representationer av hög endimensionell kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="348e4-256">In this walkthrough, we focus on using count tables tooproduce compact representations of high-dimensional categorical features.</span></span> <span data-ttu-id="348e4-257">Detta är inte hello endast hur tooencode kategoriska funktionerna; Mer information om andra metoder berörda användare kan checka ut [en-hot encoding](http://en.wikipedia.org/wiki/One-hot) och [hash-funktionen](http://en.wikipedia.org/wiki/Feature_hashing).</span><span class="sxs-lookup"><span data-stu-id="348e4-257">This is not hello only way tooencode categorical features; for more information on other techniques, interested users can check out [one-hot-encoding](http://en.wikipedia.org/wiki/One-hot) and [feature hashing](http://en.wikipedia.org/wiki/Feature_hashing).</span></span>
>

<span data-ttu-id="348e4-258">toobuild antal tabeller på hello antal data, vi använda hello data i hello mappen raw /-beräkning.</span><span class="sxs-lookup"><span data-stu-id="348e4-258">toobuild count tables on hello count data, we use hello data in hello folder raw/count.</span></span> <span data-ttu-id="348e4-259">I hello modeling avsnittet visar vi användarna tabeller hur toobuild dessa antal för kategoriska funktioner från grunden eller alternativt toouse en förskapad antal tabell för sina explorations.</span><span class="sxs-lookup"><span data-stu-id="348e4-259">In hello modeling section, we show users how toobuild these count tables for categorical features from scratch, or alternatively toouse a pre-built count table for their explorations.</span></span> <span data-ttu-id="348e4-260">I det här när vi refererar för ”inbyggd antal tabeller”, vi menar med hello antalet tabeller som vi tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="348e4-260">In what follows, when we refer too"pre-built count tables", we mean using hello count tables that we provide.</span></span> <span data-ttu-id="348e4-261">Detaljerade anvisningar om hur tooaccess dessa tabeller finns i hello nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="348e4-261">Detailed instructions on how tooaccess these tables are provided in hello next section.</span></span>

## <span data-ttu-id="348e4-262"><a name="aml"></a>Skapa en modell med Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="348e4-262"><a name="aml"></a> Build a model with Azure Machine Learning</span></span>
<span data-ttu-id="348e4-263">Vår modell processen i Azure Machine Learning för att bygga följer de här stegen:</span><span class="sxs-lookup"><span data-stu-id="348e4-263">Our model building process in Azure Machine Learning follows these steps:</span></span>

1. [<span data-ttu-id="348e4-264">Hämta hello data från Hive-tabeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="348e4-264">Get hello data from Hive tables into Azure Machine Learning</span></span>](#step1)
2. [<span data-ttu-id="348e4-265">Skapa hello experiment: Rensa hello data och featurize med antal tabeller</span><span class="sxs-lookup"><span data-stu-id="348e4-265">Create hello experiment: clean hello data and featurize with count tables</span></span>](#step2)
3. [<span data-ttu-id="348e4-266">Skapa, tåg och hello poängsätta modell</span><span class="sxs-lookup"><span data-stu-id="348e4-266">Build, train, and score hello model</span></span>](#step3)
4. [<span data-ttu-id="348e4-267">Utvärdera hello modellen</span><span class="sxs-lookup"><span data-stu-id="348e4-267">Evaluate hello model</span></span>](#step4)
5. [<span data-ttu-id="348e4-268">Publicera hello modell som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="348e4-268">Publish hello model as a web-service</span></span>](#step5)

<span data-ttu-id="348e4-269">Vi är nu redo toobuild modeller i Azure Machine Learning studio.</span><span class="sxs-lookup"><span data-stu-id="348e4-269">Now we are ready toobuild models in Azure Machine Learning studio.</span></span> <span data-ttu-id="348e4-270">Vår nedåt samplade data sparas som Hive-tabeller i hello kluster.</span><span class="sxs-lookup"><span data-stu-id="348e4-270">Our down sampled data is saved as Hive tables in hello cluster.</span></span> <span data-ttu-id="348e4-271">Vi använder hello Azure Machine Learning **importera Data** modulen tooread dessa data.</span><span class="sxs-lookup"><span data-stu-id="348e4-271">We use hello Azure Machine Learning **Import Data** module tooread this data.</span></span> <span data-ttu-id="348e4-272">hello autentiseringsuppgifter tooaccess hello storage-konto för detta kluster finns i vilka sätt.</span><span class="sxs-lookup"><span data-stu-id="348e4-272">hello credentials tooaccess hello storage account of this cluster are provided in what follows.</span></span>

### <span data-ttu-id="348e4-273"><a name="step1"></a>Steg 1: Hämta data från Hive-tabeller i Azure Machine Learning modulen hello importera Data och markera ett maskininlärningsexperiment</span><span class="sxs-lookup"><span data-stu-id="348e4-273"><a name="step1"></a> Step 1: Get data from Hive tables into Azure Machine Learning using hello Import Data module and select it for a machine learning experiment</span></span>
<span data-ttu-id="348e4-274">Starta genom att välja en **+ ny** -> **EXPERIMENT** -> **tomt Experiment**.</span><span class="sxs-lookup"><span data-stu-id="348e4-274">Start by selecting a **+NEW** -> **EXPERIMENT** -> **Blank Experiment**.</span></span> <span data-ttu-id="348e4-275">Sedan från hello **Sök** rutan hello längst upp till vänster, Sök efter ”importera Data”.</span><span class="sxs-lookup"><span data-stu-id="348e4-275">Then, from hello **Search** box on hello top left, search for "Import Data".</span></span> <span data-ttu-id="348e4-276">Dra och släpp hello **importera Data** modul på toohello experiment arbetsytan (hello mellersta delen av hello-skärmen) toouse hello-modulen för dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="348e4-276">Drag and drop hello **Import Data** module on toohello experiment canvas (hello middle portion of hello screen) toouse hello module for data access.</span></span>

<span data-ttu-id="348e4-277">Det här är vad hello **importera Data** ser ut som att hämta data från hello Hive-tabell:</span><span class="sxs-lookup"><span data-stu-id="348e4-277">This is what hello **Import Data** looks like while getting data from hello Hive table:</span></span>

![Importera Data hämtar data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

<span data-ttu-id="348e4-279">För hello **importera Data** modulen hello värdena för hello-parametrar som finns angivna i hello bild är bara exempel på hello till värden du behöver tooprovide.</span><span class="sxs-lookup"><span data-stu-id="348e4-279">For hello **Import Data** module, hello values of hello parameters that are provided in hello graphic are just examples of hello sort of values you need tooprovide.</span></span> <span data-ttu-id="348e4-280">Här är några allmänna riktlinjer om hur toofill Utdataparametern hello anges för hello **importera Data** modul.</span><span class="sxs-lookup"><span data-stu-id="348e4-280">Here is some general guidance on how toofill out hello parameter set for hello **Import Data** module.</span></span>

1. <span data-ttu-id="348e4-281">Välj ”Hive-frågan” för **datakälla**</span><span class="sxs-lookup"><span data-stu-id="348e4-281">Choose "Hive query" for **Data Source**</span></span>
2. <span data-ttu-id="348e4-282">I hello **Hive databasfrågan** , en enkel, MARKERAR du kryssrutan * FROM < din\_databasen\_name.your\_tabell\_name >-räcker.</span><span class="sxs-lookup"><span data-stu-id="348e4-282">In hello **Hive database query** box, a simple SELECT * FROM <your\_database\_name.your\_table\_name> - is enough.</span></span>
3. <span data-ttu-id="348e4-283">**Hcatalog server URI**: om klustret är ”abc” och sedan det här är bara: https://abc.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="348e4-283">**Hcatalog server URI**: If your cluster is "abc", then this is simply: https://abc.azurehdinsight.net</span></span>
4. <span data-ttu-id="348e4-284">**Hadoop användarkontonamnet**: hello användarnamn valt när hello idriftsättning hello klustret.</span><span class="sxs-lookup"><span data-stu-id="348e4-284">**Hadoop user account name**: hello user name chosen at hello time of commissioning hello cluster.</span></span> <span data-ttu-id="348e4-285">(Inte hello fjärråtkomst användarnamn!)</span><span class="sxs-lookup"><span data-stu-id="348e4-285">(NOT hello Remote Access user name!)</span></span>
5. <span data-ttu-id="348e4-286">**Hadoop lösenord**: hello lösenordet för hello användarnamn valt när hello idriftsättning hello klustret.</span><span class="sxs-lookup"><span data-stu-id="348e4-286">**Hadoop user account password**: hello password for hello user name chosen at hello time of commissioning hello cluster.</span></span> <span data-ttu-id="348e4-287">(Inte hello fjärråtkomst lösenord!)</span><span class="sxs-lookup"><span data-stu-id="348e4-287">(NOT hello Remote Access password!)</span></span>
6. <span data-ttu-id="348e4-288">**Platsen för utdata**: Välj ”Azure”</span><span class="sxs-lookup"><span data-stu-id="348e4-288">**Location of output data**: Choose "Azure"</span></span>
7. <span data-ttu-id="348e4-289">**Azure lagringskontonamnet**: hello storage-konto som är associerade med hello-kluster</span><span class="sxs-lookup"><span data-stu-id="348e4-289">**Azure storage account name**: hello storage account associated with hello cluster</span></span>
8. <span data-ttu-id="348e4-290">**Azure lagringskontonyckel**: hello nyckeln för hello storage-konto som är associerade med hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="348e4-290">**Azure storage account key**: hello key of hello storage account associated with hello cluster.</span></span>
9. <span data-ttu-id="348e4-291">**Azure behållarnamn**: om hello klusternamnet är ”abc”, så det är helt enkelt ”abc”, vanligtvis.</span><span class="sxs-lookup"><span data-stu-id="348e4-291">**Azure container name**: If hello cluster name is "abc", then this is simply "abc", usually.</span></span>

<span data-ttu-id="348e4-292">En gång hello **importera Data** har avslutats hämtning av data (visas hello grön skalstreck på hello Module), spara informationen som en datamängd (med ett valfritt namn).</span><span class="sxs-lookup"><span data-stu-id="348e4-292">Once hello **Import Data** finishes getting data (you see hello green tick on hello Module), save this data as a Dataset (with a name of your choice).</span></span> <span data-ttu-id="348e4-293">Det ser ut:</span><span class="sxs-lookup"><span data-stu-id="348e4-293">What this looks like:</span></span>

![Importera Data spara data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

<span data-ttu-id="348e4-295">Högerklicka på hello utgående port för hello **importera Data** modul.</span><span class="sxs-lookup"><span data-stu-id="348e4-295">Right-click hello output port of hello **Import Data** module.</span></span> <span data-ttu-id="348e4-296">Nu visas en **Spara som dataset** alternativet och en **visualisera** alternativet.</span><span class="sxs-lookup"><span data-stu-id="348e4-296">This reveals a **Save as dataset** option and a **Visualize** option.</span></span> <span data-ttu-id="348e4-297">Hej **visualisera** alternativet om du visar 100 datarader hello, tillsammans med en högra panelen som är användbar för vissa sammanfattande statistik.</span><span class="sxs-lookup"><span data-stu-id="348e4-297">hello **Visualize** option, if clicked, displays 100 rows of hello data, along with a right panel that is useful for some summary statistics.</span></span> <span data-ttu-id="348e4-298">toosave data, välja **Spara som dataset** och följ instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="348e4-298">toosave data, simply select **Save as dataset** and follow instructions.</span></span>

<span data-ttu-id="348e4-299">tooselect hello sparade dataset för användning i ett machine learning-experiment hitta hello datauppsättningar som använder hello **Sök** rutan som visas i följande bild hello.</span><span class="sxs-lookup"><span data-stu-id="348e4-299">tooselect hello saved dataset for use in a machine learning experiment, locate hello datasets using hello **Search** box shown in hello following figure.</span></span> <span data-ttu-id="348e4-300">Sedan bara Skriv ut hello namn du gav hello dataset delvis tooaccess den och dra hello dataset till hello åtgärdsfönstervärdens.</span><span class="sxs-lookup"><span data-stu-id="348e4-300">Then simply type out hello name you gave hello dataset partially tooaccess it and drag hello dataset onto hello main panel.</span></span> <span data-ttu-id="348e4-301">Släppa på hello åtgärdsfönstervärdens markeras den för användning i machine learning modellering.</span><span class="sxs-lookup"><span data-stu-id="348e4-301">Dropping it onto hello main panel selects it for use in machine learning modeling.</span></span>

![Drage dataset till hello åtgärdsfönstervärdens](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> <span data-ttu-id="348e4-303">Gör detta för både hello tåg och hello test datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="348e4-303">Do this for both hello train and hello test datasets.</span></span> <span data-ttu-id="348e4-304">Glöm inte heller toouse hello databasens namn och tabellnamn som du gav för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="348e4-304">Also, remember toouse hello database name and table names that you gave for this purpose.</span></span> <span data-ttu-id="348e4-305">hello-värden som används i hello bild är enbart för bilden purposes.* *</span><span class="sxs-lookup"><span data-stu-id="348e4-305">hello values used in hello figure are solely for illustration purposes.**</span></span>
> 
> 

### <span data-ttu-id="348e4-306"><a name="step2"></a>Steg 2: Skapa ett enkelt experiment i Azure Machine Learning toopredict klick / några klick</span><span class="sxs-lookup"><span data-stu-id="348e4-306"><a name="step2"></a> Step 2: Create a simple experiment in Azure Machine Learning toopredict clicks / no clicks</span></span>
<span data-ttu-id="348e4-307">Vårt Azure ML-experiment ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="348e4-307">Our Azure ML experiment looks like this:</span></span>

![Machine Learning-experiment](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

<span data-ttu-id="348e4-309">Vi nu undersöka hello viktiga komponenter av experimentet.</span><span class="sxs-lookup"><span data-stu-id="348e4-309">We now examine hello key components of this experiment.</span></span> <span data-ttu-id="348e4-310">Som en påminnelse om behöver vi toodrag våra sparade träna och testa datauppsättningar på tooour experimentet först.</span><span class="sxs-lookup"><span data-stu-id="348e4-310">As a reminder, we need toodrag our saved train and test datasets on tooour experiment canvas first.</span></span>

#### <a name="clean-missing-data"></a><span data-ttu-id="348e4-311">Rensa Data som saknas</span><span class="sxs-lookup"><span data-stu-id="348e4-311">Clean Missing Data</span></span>
<span data-ttu-id="348e4-312">Hej **Rensa Data som saknas** modulen har namnet antyder: data som saknas på ett sätt som kan vara användardefinierade rensas.</span><span class="sxs-lookup"><span data-stu-id="348e4-312">hello **Clean Missing Data** module does what its name suggests:  it cleans missing data in ways that can be user-specified.</span></span> <span data-ttu-id="348e4-313">Titta på den här modulen finns vi här:</span><span class="sxs-lookup"><span data-stu-id="348e4-313">Looking into this module, we see this:</span></span>

![Rensa data som saknas](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

<span data-ttu-id="348e4-315">Här kan valde vi tooreplace alla värden som saknas med 0.</span><span class="sxs-lookup"><span data-stu-id="348e4-315">Here, we chose tooreplace all missing values with a 0.</span></span> <span data-ttu-id="348e4-316">Det finns även andra alternativ som du kan se genom att titta på hello nedrullningsbara listorna i hello modul.</span><span class="sxs-lookup"><span data-stu-id="348e4-316">There are other options as well, which can be seen by looking at hello dropdowns in hello module.</span></span>

#### <a name="feature-engineering-on-hello-data"></a><span data-ttu-id="348e4-317">Egenskapsval på hello data</span><span class="sxs-lookup"><span data-stu-id="348e4-317">Feature engineering on hello data</span></span>
<span data-ttu-id="348e4-318">Det kan finnas miljontals unika värden för vissa kategoriska funktioner i stora datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="348e4-318">There can be millions of unique values for some categorical features of large datasets.</span></span> <span data-ttu-id="348e4-319">Med hjälp av naïve metoder, till exempel en hot kodning för att representera hög endimensionell kategoriska funktioner är helt unfeasible.</span><span class="sxs-lookup"><span data-stu-id="348e4-319">Using naive methods such as one-hot encoding for representing such high-dimensional categorical features is entirely unfeasible.</span></span> <span data-ttu-id="348e4-320">I den här genomgången visar hur toouse antal funktioner med hjälp av inbyggda Azure Machine Learning-moduler toogenerate komprimera representationer av dessa hög endimensionell kategoriska variabler.</span><span class="sxs-lookup"><span data-stu-id="348e4-320">In this walkthrough, we demonstrate how toouse count features using built-in Azure Machine Learning modules toogenerate compact representations of these high-dimensional categorical variables.</span></span> <span data-ttu-id="348e4-321">hello-slutresultatet blir mindre modellen, snabbare utbildning och resultatmått som är helt jämförbar toousing andra metoder.</span><span class="sxs-lookup"><span data-stu-id="348e4-321">hello end-result is a smaller model size, faster training times, and performance metrics that are quite comparable toousing other techniques.</span></span>

##### <a name="building-counting-transforms"></a><span data-ttu-id="348e4-322">Skapa inventering transformeringar</span><span class="sxs-lookup"><span data-stu-id="348e4-322">Building counting transforms</span></span>
<span data-ttu-id="348e4-323">toobuild antal funktioner, använder vi hello **skapa inventering transformera** modul som finns tillgängliga i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="348e4-323">toobuild count features, we use hello **Build Counting Transform** module that is available in Azure Machine Learning.</span></span> <span data-ttu-id="348e4-324">hello modulen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="348e4-324">hello module looks like this:</span></span>

<span data-ttu-id="348e4-325">![Skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span><span class="sxs-lookup"><span data-stu-id="348e4-325">![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="348e4-326">I hello **antal kolumner** vi rutan Ange de kolumner som vi vill tooperform räknar.</span><span class="sxs-lookup"><span data-stu-id="348e4-326">In hello **Count columns** box, we enter those columns that we wish tooperform counts on.</span></span> <span data-ttu-id="348e4-327">Dessa normalt (som tidigare nämnts) hög endimensionell kategoriska kolumner.</span><span class="sxs-lookup"><span data-stu-id="348e4-327">Typically, these are (as mentioned) high-dimensional categorical columns.</span></span> <span data-ttu-id="348e4-328">Hello början anges som hello Criteo datamängden har 26 kategoriska kolumner: från Col15 tooCol40.</span><span class="sxs-lookup"><span data-stu-id="348e4-328">At hello start, we mentioned that hello Criteo dataset has 26 categorical columns: from Col15 tooCol40.</span></span> <span data-ttu-id="348e4-329">Här kan vi räkna på dem alla och ge sina index (från 15 too40 avgränsade med kommatecken enligt).</span><span class="sxs-lookup"><span data-stu-id="348e4-329">Here, we count on all of them and give their indices (from 15 too40 separated by commas as shown).</span></span>
> 

<span data-ttu-id="348e4-330">toouse hello-modul i Hej MapReduce-läge (lämplig för stora datauppsättningar), vi behöver komma åt tooan HDInsight Hadoop-kluster (en som används för funktionen utforskning kan återanvändas för detta ändamål samt hello) och dess autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="348e4-330">toouse hello module in hello MapReduce mode (appropriate for large datasets), we need access tooan HDInsight Hadoop cluster (hello one used for feature exploration can be reused for this purpose as well) and its credentials.</span></span> <span data-ttu-id="348e4-331">hello tidigare siffror visar hello ifyllda värdena se ut (ersätta hello värden för jämförelseändamål med de som är relevanta för dina egna användningsfall).</span><span class="sxs-lookup"><span data-stu-id="348e4-331">hello  previous figures illustrate what hello filled-in values look like (replace hello values provided for illustration with those relevant for your own use-case).</span></span>

![Modulparametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

<span data-ttu-id="348e4-333">I ovanstående hello bild, visar vi hur tooenter hello indata blob plats.</span><span class="sxs-lookup"><span data-stu-id="348e4-333">In hello figure above, we show how tooenter hello input blob location.</span></span> <span data-ttu-id="348e4-334">Den här platsen har hello data som reserverats för antalet tabeller som bygger på.</span><span class="sxs-lookup"><span data-stu-id="348e4-334">This location has hello data reserved for building count tables on.</span></span>

<span data-ttu-id="348e4-335">När den här modulen är klar vi spara hello-transformering för senare genom att högerklicka på hello modulen och välja hello **Spara som transformeringen** alternativ:</span><span class="sxs-lookup"><span data-stu-id="348e4-335">After this module finishes running, we can save hello transform for later by right-clicking hello module and selecting hello **Save as Transform** option:</span></span>

![Alternativet ”Spara som transformeringen”](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

<span data-ttu-id="348e4-337">I vårt experiment arkitektur som visas ovan motsvarar hello dataset ”ytransform2” exakt tooa Spara antal transformeringen.</span><span class="sxs-lookup"><span data-stu-id="348e4-337">In our experiment architecture shown above, hello dataset "ytransform2" corresponds precisely tooa saved count transform.</span></span> <span data-ttu-id="348e4-338">För hello resten av experimentet, antar vi att hello läsare användes en **skapa inventering transformera** modul på vissa data toogenerate antal och kan sedan använda dessa antal toogenerate antal funktioner på hello tåg och testa datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="348e4-338">For hello remainder of this experiment, we assume that hello reader used a **Build Counting Transform** module on some data toogenerate counts, and can then use those counts toogenerate count features on hello train and test datasets.</span></span>

##### <a name="choosing-what-count-features-tooinclude-as-part-of-hello-train-and-test-datasets"></a><span data-ttu-id="348e4-339">Välja vilka antal funktioner tooinclude som en del av hello tåg och testa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="348e4-339">Choosing what count features tooinclude as part of hello train and test datasets</span></span>
<span data-ttu-id="348e4-340">När vi har ett antal transformera redo hello användaren kan välja vilka funktioner tooinclude i sina tåg och testa datauppsättningar som använder hello **ändra antalet tabellen parametrar** modul.</span><span class="sxs-lookup"><span data-stu-id="348e4-340">Once we have a count transform ready, hello user can choose what features tooinclude in their train and test datasets using hello **Modify Count Table Parameters** module.</span></span> <span data-ttu-id="348e4-341">Vi bara att visa den här modulen här för fullständighetens skull, men i enkelhetens inte utnyttjar den i vårt experiment.</span><span class="sxs-lookup"><span data-stu-id="348e4-341">We just show this module here for completeness, but in interests of simplicity do not actually use it in our experiment.</span></span>

![Ändra antalet tabellen parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

<span data-ttu-id="348e4-343">I det här fallet som kan ses vi har valt toouse bara hello log-oddsen och tooignore hello tillbaka av kolumn.</span><span class="sxs-lookup"><span data-stu-id="348e4-343">In this case, as can be seen, we have chosen toouse just hello log-odds and tooignore hello back off column.</span></span> <span data-ttu-id="348e4-344">Vi kan också ange parametrar som till exempel hello skräp bin tröskelvärdet, hur många startvärden föregående exempel tooadd för utjämning, och om toouse alla Laplacian-brus eller inte.</span><span class="sxs-lookup"><span data-stu-id="348e4-344">We can also set parameters such as hello garbage bin threshold, how many pseudo-prior examples tooadd for smoothing, and whether toouse any Laplacian noise or not.</span></span> <span data-ttu-id="348e4-345">Alla dessa avancerade funktioner och det är toobe anges att hello standardvärdena är en bra utgångspunkt för användare som är ny toothis typ av funktionen.</span><span class="sxs-lookup"><span data-stu-id="348e4-345">All these are advanced features and it is toobe noted that hello default values are a good starting point for users who are new toothis type of feature generation.</span></span>

##### <a name="data-transformation-before-generating-hello-count-features"></a><span data-ttu-id="348e4-346">DTS innan du genererar hello antal funktioner</span><span class="sxs-lookup"><span data-stu-id="348e4-346">Data transformation before generating hello count features</span></span>
<span data-ttu-id="348e4-347">Nu vi fokusera på en viktig aspekt Omforma våra tåg och testa data tidigare tooactually genererar antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="348e4-347">Now we focus on an important point about transforming our train and test data prior tooactually generating count features.</span></span> <span data-ttu-id="348e4-348">Observera att det finns två **köra R-skriptet** moduler som används innan vi installerar hello antal omvandla tooour data.</span><span class="sxs-lookup"><span data-stu-id="348e4-348">Note that there are two **Execute R Script** modules used before we apply hello count transform tooour data.</span></span>

![Köra R-skriptet moduler](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

<span data-ttu-id="348e4-350">Här är hello första R-skriptet:</span><span class="sxs-lookup"><span data-stu-id="348e4-350">Here is hello first R script:</span></span>

![Första R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

<span data-ttu-id="348e4-352">I det här R-skriptet kan vi byta namn på vår kolumner toonames ”Kol1” för ”Col40”.</span><span class="sxs-lookup"><span data-stu-id="348e4-352">In this R script, we rename our columns toonames "Col1" too"Col40".</span></span> <span data-ttu-id="348e4-353">Det beror på att hello antal transformeringen förväntar namnen på det här formatet.</span><span class="sxs-lookup"><span data-stu-id="348e4-353">This is because hello count transform expects names of this format.</span></span>

<span data-ttu-id="348e4-354">I hello andra R-skriptet vi balansera hello distributionen mellan positiva och negativa klasser (klasserna 1 och 0 respektive) av negativt nedsampling hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="348e4-354">In hello second R script, we balance hello distribution between positive and negative classes (classes 1 and 0 respectively) by downsampling hello negative class.</span></span> <span data-ttu-id="348e4-355">hello R-skript här visas hur toodo detta:</span><span class="sxs-lookup"><span data-stu-id="348e4-355">hello R script here shows how toodo this:</span></span>

![Andra R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

<span data-ttu-id="348e4-357">I det här enkla R-skriptet kan vi använda ”pos\_neg\_förhållandet” tooset hello mängden balans mellan hello positiva och negativa hello-klasser.</span><span class="sxs-lookup"><span data-stu-id="348e4-357">In this simple R script, we use "pos\_neg\_ratio" tooset hello amount of balance between hello positive and hello negative classes.</span></span> <span data-ttu-id="348e4-358">Detta är viktigt toodo eftersom förbättra klassen obalans vanligtvis har prestandafördelar för klassificering problem där hello klassen distribution är förvrängd (återkallning att i vårt fall har vi 3.3% positivt klassen och 96.7% negativt klassen).</span><span class="sxs-lookup"><span data-stu-id="348e4-358">This is important toodo since improving class imbalance usually has performance benefits for classification problems where hello class distribution is skewed (recall that in our case, we have 3.3% positive class and 96.7% negative class).</span></span>

##### <a name="applying-hello-count-transformation-on-our-data"></a><span data-ttu-id="348e4-359">Tillämpa hello antal omvandling på våra data</span><span class="sxs-lookup"><span data-stu-id="348e4-359">Applying hello count transformation on our data</span></span>
<span data-ttu-id="348e4-360">Slutligen kan vi använda hello **gäller omvandling** modulen tooapply hello antal transformeringar på vår tåg och testa datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="348e4-360">Finally, we can use hello **Apply Transformation** module tooapply hello count transforms on our train and test datasets.</span></span> <span data-ttu-id="348e4-361">Den här modulen tar hello Spara antal transformeringen som en indata hello träna och testa datauppsättningar som hello andra indata och returnerar data med antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="348e4-361">This module takes hello saved count transform as one input and hello train or test datasets as hello other input, and returns data with count features.</span></span> <span data-ttu-id="348e4-362">Den visas här:</span><span class="sxs-lookup"><span data-stu-id="348e4-362">It is shown here:</span></span>

![Tillämpa omvandling av modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-hello-count-features-look-like"></a><span data-ttu-id="348e4-364">Hämtad hello antal funktioner ser ut</span><span class="sxs-lookup"><span data-stu-id="348e4-364">An excerpt of what hello count features look like</span></span>
<span data-ttu-id="348e4-365">Det är instruktiva toosee vilka hello antal funktioner ser ut i vårt fall.</span><span class="sxs-lookup"><span data-stu-id="348e4-365">It is instructive toosee what hello count features look like in our case.</span></span> <span data-ttu-id="348e4-366">Här beskrivs ett utdrag ur detta:</span><span class="sxs-lookup"><span data-stu-id="348e4-366">Here we show an excerpt of this:</span></span>

![Antal funktioner](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

<span data-ttu-id="348e4-368">I detta utdrag visar vi att för hello kolumner som vi räknas på, vi få fram hello och logga oddsen i tillägg tooany relevanta backoffs.</span><span class="sxs-lookup"><span data-stu-id="348e4-368">In this excerpt, we show that for hello columns that we counted on, we get hello counts and log odds in addition tooany relevant backoffs.</span></span>

<span data-ttu-id="348e4-369">Vi är nu redo toobuild en Azure Machine Learning-modell med hjälp av dessa transformerade data.</span><span class="sxs-lookup"><span data-stu-id="348e4-369">We are now ready toobuild an Azure Machine Learning model using these transformed datasets.</span></span> <span data-ttu-id="348e4-370">I nästa avsnitt hello visar vi hur detta kan göras.</span><span class="sxs-lookup"><span data-stu-id="348e4-370">In hello next section, we show how this can be done.</span></span>

### <span data-ttu-id="348e4-371"><a name="step3"></a>Steg 3: Skapa, träna och betygsätta hello modellen</span><span class="sxs-lookup"><span data-stu-id="348e4-371"><a name="step3"></a> Step 3: Build, train, and score hello model</span></span>

#### <a name="choice-of-learner"></a><span data-ttu-id="348e4-372">Valet av deltagaren</span><span class="sxs-lookup"><span data-stu-id="348e4-372">Choice of learner</span></span>
<span data-ttu-id="348e4-373">Vi måste först toochoose en deltagaren.</span><span class="sxs-lookup"><span data-stu-id="348e4-373">First, we need toochoose a learner.</span></span> <span data-ttu-id="348e4-374">Vi kan gå toouse ett två klassen förstärkta beslutsträd som våra deltagaren.</span><span class="sxs-lookup"><span data-stu-id="348e4-374">We are going toouse a two class boosted decision tree as our learner.</span></span> <span data-ttu-id="348e4-375">Här följer hello standardalternativ för den här deltagaren:</span><span class="sxs-lookup"><span data-stu-id="348e4-375">Here are hello default options for this learner:</span></span>

![Två-Tvåklassförhöjt beslutsträd parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

<span data-ttu-id="348e4-377">Vi kan kommer toochoose hello standardvärden för vårt experiment.</span><span class="sxs-lookup"><span data-stu-id="348e4-377">For our experiment, we are going toochoose hello default values.</span></span> <span data-ttu-id="348e4-378">Vi Observera att hello som standard är vanligtvis beskrivande och ett bra sätt tooget snabb baslinjer på prestanda.</span><span class="sxs-lookup"><span data-stu-id="348e4-378">We note that hello defaults are usually meaningful and a good way tooget quick baselines on performance.</span></span> <span data-ttu-id="348e4-379">Du kan förbättra prestanda genom omfattande parametrar om du väljer tooonce som du har en baslinje.</span><span class="sxs-lookup"><span data-stu-id="348e4-379">You can improve on performance by sweeping parameters if you choose tooonce you have a baseline.</span></span>

#### <a name="train-hello-model"></a><span data-ttu-id="348e4-380">Hej träningsmodell</span><span class="sxs-lookup"><span data-stu-id="348e4-380">Train hello model</span></span>
<span data-ttu-id="348e4-381">För träning, vi bara anropa en **Träningsmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="348e4-381">For training, we simply invoke a **Train Model** module.</span></span> <span data-ttu-id="348e4-382">hello två indata tooit är hello två-Tvåklassförhöjt beslutsträd deltagaren och vår train datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="348e4-382">hello two inputs tooit are hello Two-Class Boosted Decision Tree learner and our train dataset.</span></span> <span data-ttu-id="348e4-383">Detta visas här:</span><span class="sxs-lookup"><span data-stu-id="348e4-383">This is shown here:</span></span>

![Modulen för Train-modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-hello-model"></a><span data-ttu-id="348e4-385">Hej poängmodell</span><span class="sxs-lookup"><span data-stu-id="348e4-385">Score hello model</span></span>
<span data-ttu-id="348e4-386">När vi har en tränad modell vi är klara tooscore på hello testa dataset och tooevaluate dess prestanda.</span><span class="sxs-lookup"><span data-stu-id="348e4-386">Once we have a trained model, we are ready tooscore on hello test dataset and tooevaluate its performance.</span></span> <span data-ttu-id="348e4-387">Vi kan göra detta med hjälp av hello **Poängmodell** modulen som visas i följande bild, tillsammans med hello en **utvärdera modell** modulen:</span><span class="sxs-lookup"><span data-stu-id="348e4-387">We do this by using hello **Score Model** module shown in hello following figure, along with an **Evaluate Model** module:</span></span>

![Modulen Poängsätta modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <span data-ttu-id="348e4-389"><a name="step4"></a>Steg 4: Utvärdera hello modellen</span><span class="sxs-lookup"><span data-stu-id="348e4-389"><a name="step4"></a> Step 4: Evaluate hello model</span></span>
<span data-ttu-id="348e4-390">Slutligen vill vi tooanalyze modellen prestanda.</span><span class="sxs-lookup"><span data-stu-id="348e4-390">Finally, we would like tooanalyze model performance.</span></span> <span data-ttu-id="348e4-391">Vanligtvis är två klass (binär) klassificering problem ett bra mått hello AUC.</span><span class="sxs-lookup"><span data-stu-id="348e4-391">Usually, for two class (binary) classification problems, a good measure is hello AUC.</span></span> <span data-ttu-id="348e4-392">toovisualize kan vi koppla samman hello **Poängmodell** modulen tooan **utvärdera modell** -modul för detta.</span><span class="sxs-lookup"><span data-stu-id="348e4-392">toovisualize this, we hook up hello **Score Model** module tooan **Evaluate Model** module for this.</span></span> <span data-ttu-id="348e4-393">Klicka på **visualisera** på hello **utvärdera modell** modulen ger en bild som hello efter:</span><span class="sxs-lookup"><span data-stu-id="348e4-393">Clicking **Visualize** on hello **Evaluate Model** module yields a graphic like hello following one:</span></span>

![Utvärdera modulen BDT modellen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

<span data-ttu-id="348e4-395">I binär (eller två klassen) klassificering problem, ett bra mått på förutsägelsefunktionen är hello Area Under kurvan (AUC).</span><span class="sxs-lookup"><span data-stu-id="348e4-395">In binary (or two class) classification problems, a good measure of prediction accuracy is hello Area Under Curve (AUC).</span></span> <span data-ttu-id="348e4-396">I följande, visar vi våra resultat med hjälp av den här modellen på vår testdata.</span><span class="sxs-lookup"><span data-stu-id="348e4-396">In what follows, we show our results using this model on our test dataset.</span></span> <span data-ttu-id="348e4-397">tooget detta, högerklicka på hello utdataporten för hello **utvärdera modell** modulen och sedan **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="348e4-397">tooget this, right-click hello output port of hello **Evaluate Model** module and then **Visualize**.</span></span>

![Visualisera modulen utvärdera modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <span data-ttu-id="348e4-399"><a name="step5"></a>Steg 5: Publicera hello modell som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="348e4-399"><a name="step5"></a> Step 5: Publish hello model as a Web service</span></span>
<span data-ttu-id="348e4-400">hello möjlighet toopublish en Azure Machine Learning-modell som webbtjänster med minsta möjliga ansträngning är en viktig funktion för att göra den allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="348e4-400">hello ability toopublish an Azure Machine Learning model as web services with a minimum of fuss is a valuable feature for making it widely available.</span></span> <span data-ttu-id="348e4-401">När det är klart kan vem som helst göra anrop toohello webbtjänst med indata att de behöver förutsägelser för och hello-webbtjänsten använder hello modellen tooreturn dessa förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="348e4-401">Once that is done, anyone can make calls toohello web service with input data that they need predictions for, and hello web service uses hello model tooreturn those predictions.</span></span>

<span data-ttu-id="348e4-402">toodo kan vi spara vår tränad modell som en Trained Model-objektet.</span><span class="sxs-lookup"><span data-stu-id="348e4-402">toodo this, we first save our trained model as a Trained Model object.</span></span> <span data-ttu-id="348e4-403">Detta görs genom att högerklicka på hello **Träningsmodell** modulen och använder hello **Spara som Trained Model** alternativet.</span><span class="sxs-lookup"><span data-stu-id="348e4-403">This is done by right-clicking hello **Train Model** module and using hello **Save as Trained Model** option.</span></span>

<span data-ttu-id="348e4-404">Nu ska vi behöver toocreate ingående och utgående portar för våra webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="348e4-404">Next, we need toocreate input and output ports for our web service:</span></span>

* <span data-ttu-id="348e4-405">en port hämtar data i hello samma formuläret som hello data som vi behöver förutsägelser för</span><span class="sxs-lookup"><span data-stu-id="348e4-405">an input port takes data in hello same form as hello data that we need predictions for</span></span>
* <span data-ttu-id="348e4-406">en utdataporten returnerar hello poängsatta etiketter och hello associerade sannolikhet.</span><span class="sxs-lookup"><span data-stu-id="348e4-406">an output port returns hello Scored Labels and hello associated probabilities.</span></span>

#### <a name="select-a-few-rows-of-data-for-hello-input-port"></a><span data-ttu-id="348e4-407">Välj ett fåtal rader med data för hello indataport</span><span class="sxs-lookup"><span data-stu-id="348e4-407">Select a few rows of data for hello input port</span></span>
<span data-ttu-id="348e4-408">Det är praktiskt toouse en **gäller SQL omvandling** modulen tooselect bara 10 rader tooserve som hello port indata.</span><span class="sxs-lookup"><span data-stu-id="348e4-408">It is convenient toouse an **Apply SQL Transformation** module tooselect just 10 rows tooserve as hello input port data.</span></span> <span data-ttu-id="348e4-409">Välj bara dessa rader med data för våra indataport använda hello SQL-fråga som visas här:</span><span class="sxs-lookup"><span data-stu-id="348e4-409">Select just these rows of data for our input port using hello SQL query shown here:</span></span>

![Indataport data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a><span data-ttu-id="348e4-411">Webbtjänst</span><span class="sxs-lookup"><span data-stu-id="348e4-411">Web service</span></span>
<span data-ttu-id="348e4-412">Nu vi är redo toorun ett litet experiment som kan använda toopublish våra webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="348e4-412">Now we are ready toorun a small experiment that can be used toopublish our web service.</span></span>

#### <a name="generate-input-data-for-webservice"></a><span data-ttu-id="348e4-413">Generera indata för webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="348e4-413">Generate input data for webservice</span></span>
<span data-ttu-id="348e4-414">Som ett zeroth steg eftersom hello antal tabell är stort, vi tar några rader testdata och generera utdata från det antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="348e4-414">As a zeroth step, since hello count table is large, we take a few lines of test data and generate output data from it with count features.</span></span> <span data-ttu-id="348e4-415">Detta kan fungera som hello indata format för våra webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="348e4-415">This can serve as hello input data format for our webservice.</span></span> <span data-ttu-id="348e4-416">Detta visas här:</span><span class="sxs-lookup"><span data-stu-id="348e4-416">This is shown here:</span></span>

![Skapa BDT indata](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> <span data-ttu-id="348e4-418">Hello indata format vi nu använda hello utdata från hello **antal Featurizer** modul.</span><span class="sxs-lookup"><span data-stu-id="348e4-418">For hello input data format, we now use hello OUTPUT of hello **Count Featurizer** module.</span></span> <span data-ttu-id="348e4-419">När detta experimentera är klar kör du spara hello utdata från hello **antal Featurizer** modulen som en datamängd.</span><span class="sxs-lookup"><span data-stu-id="348e4-419">Once this experiment finishes running, save hello output from hello **Count Featurizer** module as a Dataset.</span></span> <span data-ttu-id="348e4-420">Den här datauppsättningen används för hello indata i hello-webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="348e4-420">This Dataset is used for hello input data in hello webservice.</span></span>
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a><span data-ttu-id="348e4-421">Bedömningen experiment för publishing webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="348e4-421">Scoring experiment for publishing webservice</span></span>
<span data-ttu-id="348e4-422">Först visar vi hur det ser ut.</span><span class="sxs-lookup"><span data-stu-id="348e4-422">First, we show what this looks like.</span></span> <span data-ttu-id="348e4-423">grundläggande hello-strukturen är en **Poängmodell** modul som accepterar våra trained model-objektet och några få rader av indata som vi skapade i föregående steg i hello med hello **antal Featurizer** modul.</span><span class="sxs-lookup"><span data-stu-id="348e4-423">hello essential structure is a **Score Model** module that accepts our trained model object and a few lines of input data that we generated in hello previous steps using hello **Count Featurizer** module.</span></span> <span data-ttu-id="348e4-424">Vi använder ”Välj kolumner i datauppsättning” tooproject hello Scored etiketter och hello poäng sannolikhet.</span><span class="sxs-lookup"><span data-stu-id="348e4-424">We use "Select Columns in Dataset" tooproject out hello Scored labels and hello Score probabilities.</span></span>

![Välj kolumner i datauppsättning](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

<span data-ttu-id="348e4-426">Observera hur hello **Välj kolumner i datauppsättning** modul kan användas för ”filtrera' data från en datamängd.</span><span class="sxs-lookup"><span data-stu-id="348e4-426">Notice how hello **Select Columns in Dataset** module can be used for 'filtering' data out from a dataset.</span></span> <span data-ttu-id="348e4-427">Vi visar hello innehållet här:</span><span class="sxs-lookup"><span data-stu-id="348e4-427">We show hello contents here:</span></span>

![Filtrera med hello Välj kolumner i datauppsättning modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

<span data-ttu-id="348e4-429">tooget hello blå indata och utdata portar, klickar du på **förbereda webservice** på hello nedre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="348e4-429">tooget hello blue input and output ports, you simply click **prepare webservice** at hello bottom right.</span></span> <span data-ttu-id="348e4-430">Kör experimentet kan också oss toopublish hello-webbtjänsten: Klicka på hello **publicera WEBBTJÄNSTEN** längst hello nedre högra, visas här:</span><span class="sxs-lookup"><span data-stu-id="348e4-430">Running this experiment also allows us toopublish hello web service: click hello **PUBLISH WEB SERVICE** icon at hello bottom right, shown here:</span></span>

![Publicera Web service](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

<span data-ttu-id="348e4-432">När hello webservice har publicerats kan hämta vi omdirigerade tooa sida som ser ut därför:</span><span class="sxs-lookup"><span data-stu-id="348e4-432">Once hello webservice is published, we get redirected tooa page that looks thus:</span></span>

![Web service-instrumentpanelen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

<span data-ttu-id="348e4-434">Det finns två länkar webservices hello vänster:</span><span class="sxs-lookup"><span data-stu-id="348e4-434">We see two links for webservices on hello left side:</span></span>

* <span data-ttu-id="348e4-435">Hej **frågor och svar** Service (eller Resursposter) är avsedd för enskild förutsägelser och är vad vi utnyttjar i den här workshopen.</span><span class="sxs-lookup"><span data-stu-id="348e4-435">hello **REQUEST/RESPONSE** Service (or RRS) is meant for single predictions and is what we utilize in this workshop.</span></span>
* <span data-ttu-id="348e4-436">Hej **BATCH EXECUTION** Service BES-används för batch förutsägelser och kräver att hello indata används toomake förutsägelser finns i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="348e4-436">hello **BATCH EXECUTION** Service (BES) is used for batch predictions and requires that hello input data used toomake predictions reside in Azure Blob Storage.</span></span>

<span data-ttu-id="348e4-437">Klicka på länken hello **frågor och svar** tar tooa sida som ger oss burk före koden i C#, python och R. Den här koden kan användas för att göra anrop toohello webservice enkelt.</span><span class="sxs-lookup"><span data-stu-id="348e4-437">Clicking on hello link **REQUEST/RESPONSE** takes us tooa page that gives us pre-canned code in C#, python, and R. This code can be conveniently used for making calls toohello webservice.</span></span> <span data-ttu-id="348e4-438">Observera att hello API-nyckel på den här sidan måste toobe som används för autentisering.</span><span class="sxs-lookup"><span data-stu-id="348e4-438">Note that hello API key on this page needs toobe used for authentication.</span></span>

<span data-ttu-id="348e4-439">Det är praktiskt toocopy denna python code över tooa nya cell i hello IPython anteckningsbok.</span><span class="sxs-lookup"><span data-stu-id="348e4-439">It is convenient toocopy this python code over tooa new cell in hello IPython notebook.</span></span>

<span data-ttu-id="348e4-440">Här visar vi en del av python-kod med hello rätt API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="348e4-440">Here we show a segment of python code with hello correct API key.</span></span>

![Python-kod](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

<span data-ttu-id="348e4-442">Observera att vi ersatt hello standard API-nyckel med vår webservices API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="348e4-442">Note that we replaced hello default API key with our webservices's API key.</span></span> <span data-ttu-id="348e4-443">Klicka på **kör** för den här cellen i en IPython anteckningsboken ger hello efter svar:</span><span class="sxs-lookup"><span data-stu-id="348e4-443">Clicking **Run** on this cell in an IPython notebook yields hello following response:</span></span>

![IPython svar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

<span data-ttu-id="348e4-445">Vi kan se att hello två testa exempel har vi frågade om (i hello JSON-ramverket för hello python-skriptet), vi få tillbaka svar i hello form ”Scored etiketterna, Scored sannolikhet”.</span><span class="sxs-lookup"><span data-stu-id="348e4-445">We see that for hello two test examples we asked about (in hello JSON framework of hello python script), we get back answers in hello form "Scored Labels, Scored Probabilities".</span></span> <span data-ttu-id="348e4-446">Observera att i det här fallet vi valde hello standardvärden hello fördefinierad koden innehåller (0 för alla numeriska kolumner och hello strängen ”värde” för alla kategoriska kolumner).</span><span class="sxs-lookup"><span data-stu-id="348e4-446">Note that in this case, we chose hello default values that hello pre-canned code provides (0's for all numeric columns and hello string "value" for all categorical columns).</span></span>

<span data-ttu-id="348e4-447">Detta avslutar våra slutpunkt till slutpunkt genomgången visar hur toohandle storskaliga dataset med hjälp av Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="348e4-447">This concludes our end-to-end walkthrough showing how toohandle large-scale dataset using Azure Machine Learning.</span></span> <span data-ttu-id="348e4-448">Vi igång med en terabyte data, skapas en förutsägelse modell och distribueras som en webbtjänst i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="348e4-448">We started with a terabyte of data, constructed a prediction model and deployed it as a web service in hello cloud.</span></span>

