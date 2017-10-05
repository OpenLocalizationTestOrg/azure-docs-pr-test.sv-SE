---
title: "Team vetenskap av data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB | Microsoft Docs"
description: "Med hjälp av Team datavetenskap Process för en slutpunkt till slutpunkt-scenario med HDInsight Hadoop-kluster för att skapa och distribuera en modell med en stor (1 TB) offentligt tillgängliga datauppsättning"
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
ms.openlocfilehash: 8e6143bca819c9a0484221f8b4feb319aaaa73f5
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="the-team-data-science-process-in-action---using-an-azure-hdinsight-hadoop-cluster-on-a-1-tb-dataset"></a><span data-ttu-id="de6f0-103">Team vetenskap av data i praktiken - med ett Azure HDInsight Hadoop-kluster på en datamängd för 1 TB</span><span class="sxs-lookup"><span data-stu-id="de6f0-103">The Team Data Science Process in action - Using an Azure HDInsight Hadoop Cluster on a 1 TB dataset</span></span>

<span data-ttu-id="de6f0-104">I den här genomgången visar med Team av vetenskapliga data i ett scenario för slutpunkt till slutpunkt med ett [Azure HDInsight Hadoop-kluster](https://azure.microsoft.com/services/hdinsight/) för att lagra, utforska funktion tekniker och ned exempeldata från en av de offentligt tillgängliga [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-104">In this walkthrough, we demonstrate using the Team Data Science Process in an end-to-end scenario with an [Azure HDInsight Hadoop cluster](https://azure.microsoft.com/services/hdinsight/) to store, explore, feature engineer, and down sample data from one of the publicly available [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) datasets.</span></span> <span data-ttu-id="de6f0-105">Vi använder Azure Machine Learning för att skapa en binär klassificering modell för den här informationen.</span><span class="sxs-lookup"><span data-stu-id="de6f0-105">We use Azure Machine Learning to build a binary classification model on this data.</span></span> <span data-ttu-id="de6f0-106">Vi visar även hur du publicerar en av dessa modeller som en webbtjänst.</span><span class="sxs-lookup"><span data-stu-id="de6f0-106">We also show how to publish one of these models as a Web service.</span></span>

<span data-ttu-id="de6f0-107">Det är också möjligt att använda en bärbar dator IPython för att utföra uppgifter som visas i den här genomgången.</span><span class="sxs-lookup"><span data-stu-id="de6f0-107">It is also possible to use an IPython notebook to accomplish the tasks presented in this walkthrough.</span></span> <span data-ttu-id="de6f0-108">Användare som vill använda den här metoden bör kontakta den [Criteo genomgången använder en Hive ODBC-anslutning](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) avsnittet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-108">Users who would like to try this approach should consult the [Criteo walkthrough using a Hive ODBC connection](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/iPythonNotebooks/machine-Learning-data-science-process-hive-walkthrough-criteo.ipynb) topic.</span></span>

## <span data-ttu-id="de6f0-109"><a name="dataset"></a>Criteo Dataset-beskrivning</span><span class="sxs-lookup"><span data-stu-id="de6f0-109"><a name="dataset"></a>Criteo Dataset Description</span></span>
<span data-ttu-id="de6f0-110">Criteo data är en Klicka förutsägelse datamängd som är cirka 370GB gzip komprimerade TVS-filer (~1.3TB okomprimerade), som består av fler än 4.3 miljard poster.</span><span class="sxs-lookup"><span data-stu-id="de6f0-110">The Criteo data is a click prediction dataset that is approximately 370GB of gzip compressed TSV files (~1.3TB uncompressed), comprising more than 4.3 billion records.</span></span> <span data-ttu-id="de6f0-111">Det hämtas från 24 dagar i klickar du på data som har gjorts tillgängliga av [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span><span class="sxs-lookup"><span data-stu-id="de6f0-111">It is taken from 24 days of click data made available by [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/).</span></span> <span data-ttu-id="de6f0-112">För att underlätta för datavetare vi har uppackade tillgängliga för oss att experimentera med data.</span><span class="sxs-lookup"><span data-stu-id="de6f0-112">For the convenience of data scientists, we have unzipped data available to us to experiment with.</span></span>

<span data-ttu-id="de6f0-113">Varje post i den här datauppsättningen innehåller 40 kolumner:</span><span class="sxs-lookup"><span data-stu-id="de6f0-113">Each record in this dataset contains 40 columns:</span></span>

* <span data-ttu-id="de6f0-114">den första kolumnen är en etikett-kolumn som anger om en användare klickar på en **lägga till** (värdet 1) eller inte på en (värde 0)</span><span class="sxs-lookup"><span data-stu-id="de6f0-114">the first column is a label column that indicates whether a user clicks an **add** (value 1) or does not click one (value 0)</span></span>
* <span data-ttu-id="de6f0-115">Nästa 13 kolumner är numeriska, och</span><span class="sxs-lookup"><span data-stu-id="de6f0-115">next 13 columns are numeric, and</span></span>
* <span data-ttu-id="de6f0-116">senaste 26 är kategoriska kolumner</span><span class="sxs-lookup"><span data-stu-id="de6f0-116">last 26 are categorical columns</span></span>

<span data-ttu-id="de6f0-117">Kolumnerna är anonym och använder en serie av uppräknade namn: ”Kol1” (för etikettkolumnen) till ' Col40 ”(för den sista kolumnen kategoriska).</span><span class="sxs-lookup"><span data-stu-id="de6f0-117">The columns are anonymized and use a series of enumerated names: "Col1" (for the label column) to 'Col40" (for the last categorical column).</span></span>            

<span data-ttu-id="de6f0-118">Här är ett utdrag av två observationer (rader) från den här datauppsättningen först 20 kolumner:</span><span class="sxs-lookup"><span data-stu-id="de6f0-118">Here is an excerpt of the first 20 columns of two observations (rows) from this dataset:</span></span>

    Col1    Col2    Col3    Col4    Col5    Col6    Col7    Col8    Col9    Col10    Col11    Col12    Col13    Col14    Col15            Col16            Col17            Col18            Col19        Col20

    0       40      42      2       54      3       0       0       2       16      0       1       4448    4       1acfe1ee        1b2ff61f        2e8b2631        6faef306        c6fc10d3    6fcd6dcb           
    0               24              27      5               0       2       1               3       10064           9a8cb066        7a06385f        417e6103        2170fc56        acf676aa    6fcd6dcb                      

<span data-ttu-id="de6f0-119">Det finns värden som saknas i båda numeriska kategoriska kolumnerna och i denna dataset.</span><span class="sxs-lookup"><span data-stu-id="de6f0-119">There are missing values in both the numeric and categorical columns in this dataset.</span></span> <span data-ttu-id="de6f0-120">Vi beskriver en enkel metod för att hantera värden som saknas.</span><span class="sxs-lookup"><span data-stu-id="de6f0-120">We describe a simple method for handling the missing values.</span></span> <span data-ttu-id="de6f0-121">Ytterligare information om data beskrivs när vi lagrar dem i Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="de6f0-121">Additional details of the data are explored when we store them into Hive tables.</span></span>

<span data-ttu-id="de6f0-122">**Definition:** *klickningar (CTR):* detta är procentandelen av klick i data.</span><span class="sxs-lookup"><span data-stu-id="de6f0-122">**Definition:** *Clickthrough rate (CTR):* This is the percentage of clicks in the data.</span></span> <span data-ttu-id="de6f0-123">I denna dataset för Criteo är CTR ungefär 3.3% eller 0.033.</span><span class="sxs-lookup"><span data-stu-id="de6f0-123">In this Criteo dataset, the CTR is about 3.3% or 0.033.</span></span>

## <span data-ttu-id="de6f0-124"><a name="mltasks"></a>Exempel på förutsägelse uppgifter</span><span class="sxs-lookup"><span data-stu-id="de6f0-124"><a name="mltasks"></a>Examples of prediction tasks</span></span>
<span data-ttu-id="de6f0-125">Två exempel förutsägelse problem åtgärdas i den här genomgången:</span><span class="sxs-lookup"><span data-stu-id="de6f0-125">Two sample prediction problems are addressed in this walkthrough:</span></span>

1. <span data-ttu-id="de6f0-126">**Binär klassificering**: beräknar om en användare klickar på en Lägg till:</span><span class="sxs-lookup"><span data-stu-id="de6f0-126">**Binary classification**: Predicts whether a user clicked an add:</span></span>
   
   * <span data-ttu-id="de6f0-127">Klass 0: Ingen klickar du på</span><span class="sxs-lookup"><span data-stu-id="de6f0-127">Class 0: No Click</span></span>
   * <span data-ttu-id="de6f0-128">Klass 1: Klicka på</span><span class="sxs-lookup"><span data-stu-id="de6f0-128">Class 1: Click</span></span>
2. <span data-ttu-id="de6f0-129">**Regression**: beräknar sannolikheten för en ad Klicka på funktioner för användaren.</span><span class="sxs-lookup"><span data-stu-id="de6f0-129">**Regression**: Predicts the probability of an ad click from user features.</span></span>

## <span data-ttu-id="de6f0-130"><a name="setup"></a>Ange ett HDInsight Hadoop-kluster för datavetenskap</span><span class="sxs-lookup"><span data-stu-id="de6f0-130"><a name="setup"></a>Set Up an HDInsight Hadoop cluster for data science</span></span>
<span data-ttu-id="de6f0-131">**Obs:** detta är vanligtvis en **Admin** aktivitet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-131">**Note:** This is typically an **Admin** task.</span></span>

<span data-ttu-id="de6f0-132">Ställ in Azure datavetenskap miljön för att skapa förutsägelseanalyslösningar med HDInsight-kluster i tre steg:</span><span class="sxs-lookup"><span data-stu-id="de6f0-132">Set up your Azure Data Science environment for building predictive analytics solutions with HDInsight clusters in three steps:</span></span>

1. <span data-ttu-id="de6f0-133">[Skapa ett lagringskonto](../storage/common/storage-create-storage-account.md): det här lagringskontot används för att lagra data i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="de6f0-133">[Create a storage account](../storage/common/storage-create-storage-account.md): This storage account is used to store data in Azure Blob Storage.</span></span> <span data-ttu-id="de6f0-134">Här lagras data som används i HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="de6f0-134">The data used in HDInsight clusters is stored here.</span></span>
2. <span data-ttu-id="de6f0-135">[Anpassa Azure HDInsight Hadoop-kluster för datavetenskap](machine-learning-data-science-customize-hadoop-cluster.md): det här steget skapar ett Azure HDInsight Hadoop-kluster med 64-bitars Anaconda Python 2.7 installerad på alla noder.</span><span class="sxs-lookup"><span data-stu-id="de6f0-135">[Customize Azure HDInsight Hadoop Clusters for Data Science](machine-learning-data-science-customize-hadoop-cluster.md): This step creates an Azure HDInsight Hadoop cluster with 64-bit Anaconda Python 2.7 installed on all nodes.</span></span> <span data-ttu-id="de6f0-136">Det finns två viktiga steg (beskrivs i det här avsnittet) för att slutföra när du anpassar HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-136">There are two important steps (described in this topic) to complete when customizing the HDInsight cluster.</span></span>
   
   * <span data-ttu-id="de6f0-137">Du måste länka storage-konto som skapades i steg 1 med ditt HDInsight-kluster när den skapas.</span><span class="sxs-lookup"><span data-stu-id="de6f0-137">You must link the storage account created in step 1 with your HDInsight cluster when it is created.</span></span> <span data-ttu-id="de6f0-138">Det här lagringskontot används för att komma åt data som kan bearbetas i klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-138">This storage account is used for accessing data that can be processed within the cluster.</span></span>
   * <span data-ttu-id="de6f0-139">Du måste aktivera fjärråtkomst till huvudnod i klustret när den har skapats.</span><span class="sxs-lookup"><span data-stu-id="de6f0-139">You must enable Remote Access to the head node of the cluster after it is created.</span></span> <span data-ttu-id="de6f0-140">Kom ihåg autentiseringsuppgifterna för fjärråtkomst som du anger här (skiljer sig från de som angetts för klustret när skapades): du behöver dem för att slutföra följande procedurer.</span><span class="sxs-lookup"><span data-stu-id="de6f0-140">Remember the remote access credentials you specify here (different from those specified for the cluster at its creation): you need them to complete the following procedures.</span></span>
3. <span data-ttu-id="de6f0-141">[Skapa en arbetsyta för Azure ML](machine-learning-create-workspace.md): den här Azure Machine Learning-arbetsytan används för att skapa machine learning-modeller efter en inledande datagranskning och ned provtagning på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="de6f0-141">[Create an Azure ML workspace](machine-learning-create-workspace.md): This Azure Machine Learning workspace is used for building machine learning models after an initial data exploration and down sampling on the HDInsight cluster.</span></span>

## <span data-ttu-id="de6f0-142"><a name="getdata"></a>Hämta och använda data från en offentlig källa</span><span class="sxs-lookup"><span data-stu-id="de6f0-142"><a name="getdata"></a>Get and consume data from a public source</span></span>
<span data-ttu-id="de6f0-143">Den [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset kan nås genom att klicka på länken acceptera användningsvillkoren och tillhandahåller ett namn.</span><span class="sxs-lookup"><span data-stu-id="de6f0-143">The [Criteo](http://labs.criteo.com/downloads/download-terabyte-click-logs/) dataset can be accessed by clicking on the link, accepting the terms of use, and providing a name.</span></span> <span data-ttu-id="de6f0-144">En ögonblicksbild av hur det ser ut visas här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-144">A snapshot of what this looks like is shown here:</span></span>

![Acceptera Criteo](./media/machine-learning-data-science-process-hive-criteo-walkthrough/hLxfI2E.png)

<span data-ttu-id="de6f0-146">Klicka på **Fortsätt om du vill hämta** du kan läsa mer om datauppsättningen och dess tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-146">Click **Continue to Download** to read more about the dataset and its availability.</span></span>

<span data-ttu-id="de6f0-147">Data som finns i en offentlig [Azure-blobblagring](../storage/blobs/storage-dotnet-how-to-use-blobs.md) plats: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span><span class="sxs-lookup"><span data-stu-id="de6f0-147">The data resides in a public [Azure blob storage](../storage/blobs/storage-dotnet-how-to-use-blobs.md) location: wasb://criteo@azuremlsampleexperiments.blob.core.windows.net/raw/.</span></span> <span data-ttu-id="de6f0-148">”Wasb” refererar till Azure Blob Storage-plats.</span><span class="sxs-lookup"><span data-stu-id="de6f0-148">The "wasb" refers to Azure Blob Storage location.</span></span> 

1. <span data-ttu-id="de6f0-149">Data i den här offentliga blob storage består av tre undermappar uppackade data.</span><span class="sxs-lookup"><span data-stu-id="de6f0-149">The data in this public blob storage consists of three subfolders of unzipped data.</span></span>
   
   1. <span data-ttu-id="de6f0-150">Undermappen *raw /-beräkning/* innehåller de första 21 dagarna data - från dag\_00 dag\_20</span><span class="sxs-lookup"><span data-stu-id="de6f0-150">The subfolder *raw/count/* contains the first 21 days of data - from day\_00 to day\_20</span></span>
   2. <span data-ttu-id="de6f0-151">Undermappen *raw-tåg/* består av en dag av data, dag\_21</span><span class="sxs-lookup"><span data-stu-id="de6f0-151">The subfolder *raw/train/* consists of a single day of data, day\_21</span></span>
   3. <span data-ttu-id="de6f0-152">Undermappen *rådata/test/* består av två dagars data, dag\_22 och dag\_23</span><span class="sxs-lookup"><span data-stu-id="de6f0-152">The subfolder *raw/test/* consists of two days of data, day\_22 and day\_23</span></span>
2. <span data-ttu-id="de6f0-153">För de som du vill börja med raw gzip-data, de är också tillgängliga i mappen huvudsakliga *rådata /* som day_NN.gz, där NN går mellan 00 och 23.</span><span class="sxs-lookup"><span data-stu-id="de6f0-153">For those who want to start with the raw gzip data, these are also available in the main folder *raw/* as day_NN.gz, where NN goes from 00 to 23.</span></span>

<span data-ttu-id="de6f0-154">En annan metod för att komma åt, utforska och modellen dessa data som inte kräver någon lokal hämtningar beskrivs senare i den här genomgången när vi skapar Hive-tabeller.</span><span class="sxs-lookup"><span data-stu-id="de6f0-154">An alternative approach to access, explore, and model this data that does not require any local downloads is explained later in this walkthrough when we create Hive tables.</span></span>

## <span data-ttu-id="de6f0-155"><a name="login"></a>Logga in på klustret headnode</span><span class="sxs-lookup"><span data-stu-id="de6f0-155"><a name="login"></a>Log in to the cluster headnode</span></span>
<span data-ttu-id="de6f0-156">Om du vill logga in på headnode i klustret, Använd den [Azure-portalen](https://ms.portal.azure.com) att hitta klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-156">To log in to the headnode of the cluster, use the [Azure portal](https://ms.portal.azure.com) to locate the cluster.</span></span> <span data-ttu-id="de6f0-157">Klicka på ikonen HDInsight Elefant till vänster och dubbelklicka sedan på namnet på klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-157">Click the HDInsight elephant icon on the left and then double-click the name of your cluster.</span></span> <span data-ttu-id="de6f0-158">Navigera till den **Configuration** fliken, dubbelklicka på ikonen Anslut längst ned på sidan och ange dina autentiseringsuppgifter för fjärråtkomst när du tillfrågas.</span><span class="sxs-lookup"><span data-stu-id="de6f0-158">Navigate to the **Configuration** tab, double-click the CONNECT icon on the bottom of the page, and enter your remote access credentials when prompted.</span></span> <span data-ttu-id="de6f0-159">Då kommer du att headnode i klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-159">This takes you to the headnode of the cluster.</span></span>

<span data-ttu-id="de6f0-160">Här är en typisk första inloggningen till klustret headnode ser ut:</span><span class="sxs-lookup"><span data-stu-id="de6f0-160">Here is what a typical first log in to the cluster headnode looks like:</span></span>

![Logga in på kluster](./media/machine-learning-data-science-process-hive-criteo-walkthrough/Yys9Vvm.png)

<span data-ttu-id="de6f0-162">Vi kan se ”Hadoop kommandoraden”, som våra bestämmer hög grad för undersökning av data till vänster.</span><span class="sxs-lookup"><span data-stu-id="de6f0-162">On the left, we see the "Hadoop Command Line", which is our workhorse for the data exploration.</span></span> <span data-ttu-id="de6f0-163">Vi kan också se två användbara webbadresserna - ”Hadoop Yarn Status” och ”Hadoop namn nod”.</span><span class="sxs-lookup"><span data-stu-id="de6f0-163">We also see two useful URLs - "Hadoop Yarn Status" and "Hadoop Name Node".</span></span> <span data-ttu-id="de6f0-164">URL: en yarn status visar jobbförloppet och den nod URL: ger information om klusterkonfigurationen.</span><span class="sxs-lookup"><span data-stu-id="de6f0-164">The yarn status URL shows job progress and the name node URL gives details on the cluster configuration.</span></span>

<span data-ttu-id="de6f0-165">Nu vi har ställts in och är redo för att börja första delen av genomgången: datagranskning med Hive och förbereda data för Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="de6f0-165">Now we are set up and ready to begin first part of the walkthrough: data exploration using Hive and getting data ready for Azure Machine Learning.</span></span>

## <span data-ttu-id="de6f0-166"><a name="hive-db-tables"></a>Skapa Hive-databasen och tabeller</span><span class="sxs-lookup"><span data-stu-id="de6f0-166"><a name="hive-db-tables"></a> Create Hive database and tables</span></span>
<span data-ttu-id="de6f0-167">Om du vill skapa Hive-tabeller för våra Criteo dataset, öppna den ***Hadoop kommandoraden*** på skrivbordet för huvudnoden, och ange Hive-katalogen genom att ange kommandot</span><span class="sxs-lookup"><span data-stu-id="de6f0-167">To create Hive tables for our Criteo dataset, open the ***Hadoop Command Line*** on the desktop of the head node, and enter the Hive directory by entering the command</span></span>

    cd %hive_home%\bin

> [!NOTE]
> <span data-ttu-id="de6f0-168">Kör alla Hive-kommandon i den här genomgången från Hive bin / directory-fråga.</span><span class="sxs-lookup"><span data-stu-id="de6f0-168">Run all Hive commands in this walkthrough from the Hive bin/ directory prompt.</span></span> <span data-ttu-id="de6f0-169">Detta hand tar om eventuella problem med sökväg automatiskt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-169">This takes care of any path issues automatically.</span></span> <span data-ttu-id="de6f0-170">Vi använder termer ”Hive directory prompt” ”, Hive bin / directory prompt”, och ”Hadoop-kommandoraden” synonymt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-170">We use the terms "Hive directory prompt", "Hive bin/ directory prompt", and "Hadoop Command Line" interchangeably.</span></span>
> 
> [!NOTE]
> <span data-ttu-id="de6f0-171">Om du vill köra Hive-fråga använda en alltid följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="de6f0-171">To execute any Hive query, one can always use the following commands:</span></span>
> 
> 

        cd %hive_home%\bin
        hive

<span data-ttu-id="de6f0-172">När Hive REPL visas med en ”hive >” Logga, bara klipp ut och klistra in frågan för att köra den.</span><span class="sxs-lookup"><span data-stu-id="de6f0-172">After the Hive REPL appears with a "hive >"sign, simply cut and paste the query to execute it.</span></span>

<span data-ttu-id="de6f0-173">Följande kod skapar en databas ”criteo” och sedan genererar 4 tabeller:</span><span class="sxs-lookup"><span data-stu-id="de6f0-173">The following code creates a database "criteo" and then generates 4 tables:</span></span>

* <span data-ttu-id="de6f0-174">en *tabell för generering av antal* bygger på dagar dag\_00 dag\_20,</span><span class="sxs-lookup"><span data-stu-id="de6f0-174">a *table for generating counts* built on days day\_00 to day\_20,</span></span>
* <span data-ttu-id="de6f0-175">en *tabell som ska användas som tåget datauppsättningen* bygger på dag\_21, och</span><span class="sxs-lookup"><span data-stu-id="de6f0-175">a *table for use as the train dataset* built on day\_21, and</span></span>
* <span data-ttu-id="de6f0-176">två *tabeller som test datauppsättningar* bygger på dag\_22 och dag\_23 respektive.</span><span class="sxs-lookup"><span data-stu-id="de6f0-176">two *tables for use as the test datasets* built on day\_22 and day\_23 respectively.</span></span>

<span data-ttu-id="de6f0-177">Vi dela vår testdata i två olika tabeller eftersom en av dagarna är helgdagar, och vi vill att avgöra om modellen identifiera skillnader mellan en helgdag och icke-helgdagar från klickningar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-177">We split our test dataset into two different tables because one of the days is a holiday, and we want to determine if the model can detect differences between a holiday and non-holiday from the clickthrough rate.</span></span>

<span data-ttu-id="de6f0-178">Skriptet [exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) visas här av praktiska skäl:</span><span class="sxs-lookup"><span data-stu-id="de6f0-178">The script [sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql) is displayed here for convenience:</span></span>

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

<span data-ttu-id="de6f0-179">Vi Observera att dessa tabeller är externa som vi helt enkelt pekar på Azure Blob Storage (wasb)-platser.</span><span class="sxs-lookup"><span data-stu-id="de6f0-179">We note that all these tables are external as we simply point to Azure Blob Storage (wasb) locations.</span></span>

<span data-ttu-id="de6f0-180">**Det finns två sätt att köra alla Hive-fråga som vi nämnt nu.**</span><span class="sxs-lookup"><span data-stu-id="de6f0-180">**There are two ways to execute ANY Hive query that we now mention.**</span></span>

1. <span data-ttu-id="de6f0-181">**Med den Hive REPL kommandoradsverktyget**: först är att utfärda en ”hive” kommandot och kopiera och klistra in en fråga på den Hive REPL kommandoraden.</span><span class="sxs-lookup"><span data-stu-id="de6f0-181">**Using the Hive REPL command-line**: The first is to issue a "hive" command and copy and paste a query at the Hive REPL command-line.</span></span> <span data-ttu-id="de6f0-182">Om du vill göra det, gör du:</span><span class="sxs-lookup"><span data-stu-id="de6f0-182">To do this, do:</span></span>
   
        cd %hive_home%\bin
        hive
   
     <span data-ttu-id="de6f0-183">Nu på REPL kommandoradsverktyg kör kopiera och klistra in frågan den.</span><span class="sxs-lookup"><span data-stu-id="de6f0-183">Now at the REPL command-line, cutting and pasting the query executes it.</span></span>
2. <span data-ttu-id="de6f0-184">**Spara frågor till en fil och kommandot**: andra är att spara frågor till en .hql-fil ([exempel &#95; hive &#95; Skapa &#95; criteo &#95; databas &#95; och &#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) och sedan kör du följande kommando för att köra frågan:</span><span class="sxs-lookup"><span data-stu-id="de6f0-184">**Saving queries to a file and executing the command**: The second is to save the queries to a .hql file ([sample&#95;hive&#95;create&#95;criteo&#95;database&#95;and&#95;tables.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_create_criteo_database_and_tables.hql)) and then issue the following command to execute the query:</span></span>
   
        hive -f C:\temp\sample_hive_create_criteo_database_and_tables.hql

### <a name="confirm-database-and-table-creation"></a><span data-ttu-id="de6f0-185">Bekräfta databas och tabell skapas</span><span class="sxs-lookup"><span data-stu-id="de6f0-185">Confirm database and table creation</span></span>
<span data-ttu-id="de6f0-186">Därefter vi bekräfta att databasen med följande kommando från Hive bin / directory prompten:</span><span class="sxs-lookup"><span data-stu-id="de6f0-186">Next, we confirm the creation of the database with the following command from the Hive bin/ directory prompt:</span></span>

        hive -e "show databases;"

<span data-ttu-id="de6f0-187">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-187">This gives:</span></span>

        criteo
        default
        Time taken: 1.25 seconds, Fetched: 2 row(s)

<span data-ttu-id="de6f0-188">Det här bekräftar att skapa den nya databasen ”criteo”.</span><span class="sxs-lookup"><span data-stu-id="de6f0-188">This confirms the creation of the new database, "criteo".</span></span>

<span data-ttu-id="de6f0-189">Om du vill se vilka tabeller som vi skapade vi bara utfärda kommandot här från Hive bin / directory prompten:</span><span class="sxs-lookup"><span data-stu-id="de6f0-189">To see what tables we created, we simply issue the command here from the Hive bin/ directory prompt:</span></span>

        hive -e "show tables in criteo;"

<span data-ttu-id="de6f0-190">Vi kan sedan se följande utdata:</span><span class="sxs-lookup"><span data-stu-id="de6f0-190">We then see the following output:</span></span>

        criteo_count
        criteo_test_day_22
        criteo_test_day_23
        criteo_train
        Time taken: 1.437 seconds, Fetched: 4 row(s)

## <span data-ttu-id="de6f0-191"><a name="exploration"></a>Datagranskning i Hive</span><span class="sxs-lookup"><span data-stu-id="de6f0-191"><a name="exploration"></a> Data exploration in Hive</span></span>
<span data-ttu-id="de6f0-192">Vi är nu redo att göra vissa grundläggande datagranskning i Hive.</span><span class="sxs-lookup"><span data-stu-id="de6f0-192">Now we are ready to do some basic data exploration in Hive.</span></span> <span data-ttu-id="de6f0-193">Vi börjar med att räkna antalet exempel i tåget och testa datatabeller.</span><span class="sxs-lookup"><span data-stu-id="de6f0-193">We begin by counting the number of examples in the train and test data tables.</span></span>

### <a name="number-of-train-examples"></a><span data-ttu-id="de6f0-194">Antal train-exempel</span><span class="sxs-lookup"><span data-stu-id="de6f0-194">Number of train examples</span></span>
<span data-ttu-id="de6f0-195">Innehållet i [exempel &#95; hive &#95; antal &#95; train &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) visas här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-195">The contents of [sample&#95;hive&#95;count&#95;train&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_train_table_examples.hql) are shown here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_train;

<span data-ttu-id="de6f0-196">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-196">This yields:</span></span>

        192215183
        Time taken: 264.154 seconds, Fetched: 1 row(s)

<span data-ttu-id="de6f0-197">Alternativt kan en även kör du följande kommando från lagerplatsen Hive / directory prompten:</span><span class="sxs-lookup"><span data-stu-id="de6f0-197">Alternatively, one may also issue the following command from the Hive bin/ directory prompt:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_train_table_examples.hql

### <a name="number-of-test-examples-in-the-two-test-datasets"></a><span data-ttu-id="de6f0-198">Antal test exemplen i de två test datauppsättningarna</span><span class="sxs-lookup"><span data-stu-id="de6f0-198">Number of test examples in the two test datasets</span></span>
<span data-ttu-id="de6f0-199">Vi nu räkna antalet exempel i två test datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-199">We now count the number of examples in the two test datasets.</span></span> <span data-ttu-id="de6f0-200">Innehållet i [exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 22 &#95; tabellen &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) finns här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-200">The contents of [sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;22&#95;table&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_22_table_examples.hql) are here:</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_22;

<span data-ttu-id="de6f0-201">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-201">This yields:</span></span>

        189747893
        Time taken: 267.968 seconds, Fetched: 1 row(s)

<span data-ttu-id="de6f0-202">Vi kan som vanligt också anropa skriptet från Hive bin / directory kommandoprompt genom att utfärda kommandot:</span><span class="sxs-lookup"><span data-stu-id="de6f0-202">As usual, we may also call the script from the Hive bin/ directory prompt by issuing the command:</span></span>

        hive -f C:\temp\sample_hive_count_criteo_test_day_22_table_examples.hql

<span data-ttu-id="de6f0-203">Slutligen vi undersöka antalet test exemplen i testdata baserat på dag\_23.</span><span class="sxs-lookup"><span data-stu-id="de6f0-203">Finally, we examine the number of test examples in the test dataset based on day\_23.</span></span>

<span data-ttu-id="de6f0-204">Kommandot för att göra detta liknar den som bara visas (se [exempel &#95; hive &#95; antal &#95; criteo &#95; test &#95; dag &#95; 23 &#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span><span class="sxs-lookup"><span data-stu-id="de6f0-204">The command to do this is similar to the one just shown (refer to [sample&#95;hive&#95;count&#95;criteo&#95;test&#95;day&#95;23&#95;examples.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_count_criteo_test_day_23_examples.hql)):</span></span>

        SELECT COUNT(*) FROM criteo.criteo_test_day_23;

<span data-ttu-id="de6f0-205">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-205">This gives:</span></span>

        178274637
        Time taken: 253.089 seconds, Fetched: 1 row(s)

### <a name="label-distribution-in-the-train-dataset"></a><span data-ttu-id="de6f0-206">Etikett distribution i train datauppsättningen</span><span class="sxs-lookup"><span data-stu-id="de6f0-206">Label distribution in the train dataset</span></span>
<span data-ttu-id="de6f0-207">Distributionen av etikett i train datamängden är av intresse.</span><span class="sxs-lookup"><span data-stu-id="de6f0-207">The label distribution in the train dataset is of interest.</span></span> <span data-ttu-id="de6f0-208">Om du vill se den här vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; etikett &#95; distri &#95; train &#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span><span class="sxs-lookup"><span data-stu-id="de6f0-208">To see this, we show contents of [sample&#95;hive&#95;criteo&#95;label&#95;distribution&#95;train&#95;table.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_label_distribution_train_table.hql):</span></span>

        SELECT Col1, COUNT(*) AS CT FROM criteo.criteo_train GROUP BY Col1;

<span data-ttu-id="de6f0-209">Detta ger etikett-distribution:</span><span class="sxs-lookup"><span data-stu-id="de6f0-209">This yields the label distribution:</span></span>

        1       6292903
        0       185922280
        Time taken: 459.435 seconds, Fetched: 2 row(s)

<span data-ttu-id="de6f0-210">Observera att procentandelen positivt etiketter är ungefär 3.3% (konsekvent med den ursprungliga datauppsättningen).</span><span class="sxs-lookup"><span data-stu-id="de6f0-210">Note that the percentage of positive labels is about 3.3% (consistent with the original dataset).</span></span>

### <a name="histogram-distributions-of-some-numeric-variables-in-the-train-dataset"></a><span data-ttu-id="de6f0-211">Histogram distributioner av vissa numeriska variablerna i datauppsättningen train</span><span class="sxs-lookup"><span data-stu-id="de6f0-211">Histogram distributions of some numeric variables in the train dataset</span></span>
<span data-ttu-id="de6f0-212">Vi kan använda registreringsdata intern ”histogram\_numeriska” funktionen för att ta reda på hur distributionen av variablerna numeriska ser ut.</span><span class="sxs-lookup"><span data-stu-id="de6f0-212">We can use Hive's native "histogram\_numeric" function to find out what the distribution of the numeric variables looks like.</span></span> <span data-ttu-id="de6f0-213">Här är innehållet i [exempel &#95; hive &#95; criteo &#95; histogram &#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span><span class="sxs-lookup"><span data-stu-id="de6f0-213">Here are the contents of [sample&#95;hive&#95;criteo&#95;histogram&#95;numeric.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_histogram_numeric.hql):</span></span>

        SELECT CAST(hist.x as int) as bin_center, CAST(hist.y as bigint) as bin_height FROM
            (SELECT
            histogram_numeric(col2, 20) as col2_hist
            FROM
            criteo.criteo_train
            ) a
            LATERAL VIEW explode(col2_hist) exploded_table as hist;

<span data-ttu-id="de6f0-214">Detta ger följande:</span><span class="sxs-lookup"><span data-stu-id="de6f0-214">This yields the following:</span></span>

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

<span data-ttu-id="de6f0-215">LATERALA Visa - Expandera kombination i Hive används för att skapa en SQL-liknande utdata i stället för vanliga listan.</span><span class="sxs-lookup"><span data-stu-id="de6f0-215">The LATERAL VIEW - explode combination in Hive serves to produce a SQL-like output instead of the usual list.</span></span> <span data-ttu-id="de6f0-216">Observera att i den här tabellen, den första kolumnen motsvarar bin center och andra för bin frekvens.</span><span class="sxs-lookup"><span data-stu-id="de6f0-216">Note that in the this table, the first column corresponds to the bin center and the second to the bin frequency.</span></span>

### <a name="approximate-percentiles-of-some-numeric-variables-in-the-train-dataset"></a><span data-ttu-id="de6f0-217">Ungefärlig percentiler av vissa numeriska variablerna i datauppsättningen train</span><span class="sxs-lookup"><span data-stu-id="de6f0-217">Approximate percentiles of some numeric variables in the train dataset</span></span>
<span data-ttu-id="de6f0-218">Är också beräkning av ungefärliga percentiler intressanta med numeriska variabler.</span><span class="sxs-lookup"><span data-stu-id="de6f0-218">Also of interest with numeric variables is the computation of approximate percentiles.</span></span> <span data-ttu-id="de6f0-219">Hive datorns inbyggda ”percentil\_ungefärlig” matchar det för oss.</span><span class="sxs-lookup"><span data-stu-id="de6f0-219">Hive's native "percentile\_approx" does this for us.</span></span> <span data-ttu-id="de6f0-220">Innehållet i [exempel &#95; hive &#95; criteo &#95; ungefärliga &#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) är:</span><span class="sxs-lookup"><span data-stu-id="de6f0-220">The contents of [sample&#95;hive&#95;criteo&#95;approximate&#95;percentiles.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_approximate_percentiles.hql) are:</span></span>

        SELECT MIN(Col2) AS Col2_min, PERCENTILE_APPROX(Col2, 0.1) AS Col2_01, PERCENTILE_APPROX(Col2, 0.3) AS Col2_03, PERCENTILE_APPROX(Col2, 0.5) AS Col2_median, PERCENTILE_APPROX(Col2, 0.8) AS Col2_08, MAX(Col2) AS Col2_max FROM criteo.criteo_train;

<span data-ttu-id="de6f0-221">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-221">This yields:</span></span>

        1.0     2.1418600917169246      2.1418600917169246    6.21887086390288 27.53454893115633       65535.0
        Time taken: 564.953 seconds, Fetched: 1 row(s)

<span data-ttu-id="de6f0-222">Vi Markera att distributionen av percentiler är nära relaterade till histogram distribution av en numerisk variabel vanligtvis.</span><span class="sxs-lookup"><span data-stu-id="de6f0-222">We remark that the distribution of percentiles is closely related to the histogram distribution of any numeric variable usually.</span></span>         

### <a name="find-number-of-unique-values-for-some-categorical-columns-in-the-train-dataset"></a><span data-ttu-id="de6f0-223">Räkna antalet unika värden för vissa kategoriska kolumner i datauppsättningen train</span><span class="sxs-lookup"><span data-stu-id="de6f0-223">Find number of unique values for some categorical columns in the train dataset</span></span>
<span data-ttu-id="de6f0-224">Fortsätter datagranskning finns vi nu, för vissa kategoriska kolumner, antalet unika värden som de vidtar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-224">Continuing the data exploration, we now find, for some categorical columns, the number of unique values they take.</span></span> <span data-ttu-id="de6f0-225">Detta gör vi visa innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span><span class="sxs-lookup"><span data-stu-id="de6f0-225">To do this, we show contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_categoricals.hql):</span></span>

        SELECT COUNT(DISTINCT(Col15)) AS num_uniques FROM criteo.criteo_train;

<span data-ttu-id="de6f0-226">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-226">This yields:</span></span>

        19011825
        Time taken: 448.116 seconds, Fetched: 1 row(s)

<span data-ttu-id="de6f0-227">Vi Observera att Col15 19M unika värden!</span><span class="sxs-lookup"><span data-stu-id="de6f0-227">We note that Col15 has 19M unique values!</span></span> <span data-ttu-id="de6f0-228">Använda naïve tekniker som ”en-hot kodning” är om du vill koda sådana hög endimensionell kategoriska variabler omöjligt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-228">Using naive techniques like "one-hot encoding" to encode such high-dimensional categorical variables is infeasible.</span></span> <span data-ttu-id="de6f0-229">I synnerhet Vi förklarar och visar en kraftfull, robust teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) för att effektivt hantera problemet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-229">In particular, we explain and demonstrate a powerful, robust technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) for tackling this problem efficiently.</span></span>

<span data-ttu-id="de6f0-230">Vi avbryta den här underavsnittet genom att titta på antalet unika värden för vissa andra kategoriska kolumner samt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-230">We end this sub-section by looking at the number of unique values for some other categorical columns as well.</span></span> <span data-ttu-id="de6f0-231">Innehållet i [exempel &#95; hive &#95; criteo &#95; unika &#95; värden &#95; flera &#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) är:</span><span class="sxs-lookup"><span data-stu-id="de6f0-231">The contents of [sample&#95;hive&#95;criteo&#95;unique&#95;values&#95;multiple&#95;categoricals.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_unique_values_multiple_categoricals.hql) are:</span></span>

        SELECT COUNT(DISTINCT(Col16)), COUNT(DISTINCT(Col17)),
        COUNT(DISTINCT(Col18), COUNT(DISTINCT(Col19), COUNT(DISTINCT(Col20))
        FROM criteo.criteo_train;

<span data-ttu-id="de6f0-232">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-232">This yields:</span></span>

        30935   15200   7349    20067   3
        Time taken: 1933.883 seconds, Fetched: 1 row(s)

<span data-ttu-id="de6f0-233">Igen Se vi att de andra kolumnerna förutom Col20, har många unika värden.</span><span class="sxs-lookup"><span data-stu-id="de6f0-233">Again we see that except for Col20, all the other columns have many unique values.</span></span>

### <a name="co-occurrence-counts-of-pairs-of-categorical-variables-in-the-train-dataset"></a><span data-ttu-id="de6f0-234">Samtidigt förekomsten räknar par kategoriska variablerna i datauppsättningen train</span><span class="sxs-lookup"><span data-stu-id="de6f0-234">Co-occurrence counts of pairs of categorical variables in the train dataset</span></span>

<span data-ttu-id="de6f0-235">Samtidigt förekomsten antal par kategoriska variabler är också av intresse.</span><span class="sxs-lookup"><span data-stu-id="de6f0-235">The co-occurrence counts of pairs of categorical variables is also of interest.</span></span> <span data-ttu-id="de6f0-236">Detta kan fastställas med hjälp av koden i [exempel &#95; hive &#95; criteo &#95; parad &#95; kategoriska &#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span><span class="sxs-lookup"><span data-stu-id="de6f0-236">This can be determined using the code in [sample&#95;hive&#95;criteo&#95;paired&#95;categorical&#95;counts.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_paired_categorical_counts.hql):</span></span>

        SELECT Col15, Col16, COUNT(*) AS paired_count FROM criteo.criteo_train GROUP BY Col15, Col16 ORDER BY paired_count DESC LIMIT 15;

<span data-ttu-id="de6f0-237">Vi omvänd ordna antal efter deras förekomst och titta på högsta 15 i det här fallet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-237">We reverse order the counts by their occurrence and look at the top 15 in this case.</span></span> <span data-ttu-id="de6f0-238">Detta ger oss:</span><span class="sxs-lookup"><span data-stu-id="de6f0-238">This gives us:</span></span>

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

## <span data-ttu-id="de6f0-239"><a name="downsample"></a>Ned exempel datamängder för Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="de6f0-239"><a name="downsample"></a> Down sample the datasets for Azure Machine Learning</span></span>
<span data-ttu-id="de6f0-240">Med utforskade datauppsättningar och visas hur vi kan göra den här typen av alla variabler (inklusive kombinationer), vi nu ned exempel exploatera datauppsättningar så att vi kan bygga modeller i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="de6f0-240">Having explored the datasets and demonstrated how we may do this type of exploration for any variables (including combinations), we now down sample the data sets so that we can build models in Azure Machine Learning.</span></span> <span data-ttu-id="de6f0-241">Kom ihåg att vi fokusera på problemet är: en uppsättning exempel attribut (funktionen värden från Col2 - Col40) får vi förutsäga om Kol1 är 0 (ingen klicka) eller 1 (klicka).</span><span class="sxs-lookup"><span data-stu-id="de6f0-241">Recall that the problem we focus on is: given a set of example attributes (feature values from Col2 - Col40), we predict if Col1 is a 0 (no click) or a 1 (click).</span></span>

<span data-ttu-id="de6f0-242">För att ned exempel våra tåg och testa datauppsättningar till 1% av den ursprungliga storleken, funktionen vi registreringsdata interna RAND().</span><span class="sxs-lookup"><span data-stu-id="de6f0-242">To down sample our train and test datasets to 1% of the original size, we use Hive's native RAND() function.</span></span> <span data-ttu-id="de6f0-243">Skriptet nästa [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; train &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) gör detta för tåget datauppsättningen:</span><span class="sxs-lookup"><span data-stu-id="de6f0-243">The next script, [sample&#95;hive&#95;criteo&#95;downsample&#95;train&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_train_dataset.hql) does this for the train dataset:</span></span>

        CREATE TABLE criteo.criteo_train_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        ---Now downsample and store in this table

        INSERT OVERWRITE TABLE criteo.criteo_train_downsample_1perc SELECT * FROM criteo.criteo_train WHERE RAND() <= 0.01;

<span data-ttu-id="de6f0-244">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-244">This yields:</span></span>

        Time taken: 12.22 seconds
        Time taken: 298.98 seconds

<span data-ttu-id="de6f0-245">Skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 22 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) matchar för testdata, dag\_22:</span><span class="sxs-lookup"><span data-stu-id="de6f0-245">The script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;22&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_22_dataset.hql) does it for test data, day\_22:</span></span>

        --- Now for test data (day_22)

        CREATE TABLE criteo.criteo_test_day_22_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 string)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_22_downsample_1perc SELECT * FROM criteo.criteo_test_day_22 WHERE RAND() <= 0.01;

<span data-ttu-id="de6f0-246">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-246">This yields:</span></span>

        Time taken: 1.22 seconds
        Time taken: 317.66 seconds


<span data-ttu-id="de6f0-247">Slutligen skriptet [exempel &#95; hive &#95; criteo &#95; nedsampla &#95; test &#95; dag &#95; 23 &#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) matchar för testdata, dag\_23:</span><span class="sxs-lookup"><span data-stu-id="de6f0-247">Finally, the script [sample&#95;hive&#95;criteo&#95;downsample&#95;test&#95;day&#95;23&#95;dataset.hql](https://github.com/Azure/Azure-MachineLearning-DataScience/blob/master/Misc/DataScienceProcess/DataScienceScripts/sample_hive_criteo_downsample_test_day_23_dataset.hql) does it for test data, day\_23:</span></span>

        --- Finally test data day_23
        CREATE TABLE criteo.criteo_test_day_23_downsample_1perc (
        col1 string,col2 double,col3 double,col4 double,col5 double,col6 double,col7 double,col8 double,col9 double,col10 double,col11 double,col12 double,col13 double,col14 double,col15 string,col16 string,col17 string,col18 string,col19 string,col20 string,col21 string,col22 string,col23 string,col24 string,col25 string,col26 string,col27 string,col28 string,col29 string,col30 string,col31 string,col32 string,col33 string,col34 string,col35 string,col36 string,col37 string,col38 string,col39 string,col40 srical feature; tring)
        ROW FORMAT DELIMITED FIELDS TERMINATED BY '\t'
        LINES TERMINATED BY '\n'
        STORED AS TEXTFILE;

        INSERT OVERWRITE TABLE criteo.criteo_test_day_23_downsample_1perc SELECT * FROM criteo.criteo_test_day_23 WHERE RAND() <= 0.01;

<span data-ttu-id="de6f0-248">Detta ger:</span><span class="sxs-lookup"><span data-stu-id="de6f0-248">This yields:</span></span>

        Time taken: 1.86 seconds
        Time taken: 300.02 seconds

<span data-ttu-id="de6f0-249">Med detta kan är vi redo att använda våra nedåt provade tåg och testa datauppsättningar för att skapa modeller i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="de6f0-249">With this, we are ready to use our down sampled train and test datasets for building models in Azure Machine Learning.</span></span>

<span data-ttu-id="de6f0-250">Det finns en sista viktig komponent innan vi vidare till Azure Machine Learning, vilket är av säkerhetsskäl tabellen antal.</span><span class="sxs-lookup"><span data-stu-id="de6f0-250">There is a final important component before we move on to Azure Machine Learning, which is concerns the count table.</span></span> <span data-ttu-id="de6f0-251">I nästa underavsnitt diskuterar vi detta i viss detalj.</span><span class="sxs-lookup"><span data-stu-id="de6f0-251">In the next sub-section, we discuss this in some detail.</span></span>

## <span data-ttu-id="de6f0-252"><a name="count"></a>En kort beskrivning på tabellen antal</span><span class="sxs-lookup"><span data-stu-id="de6f0-252"><a name="count"></a> A brief discussion on the count table</span></span>
<span data-ttu-id="de6f0-253">Som vi såg har flera kategoriska variabler en mycket hög dimensionalitet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-253">As we saw, several categorical variables have a very high dimensionality.</span></span> <span data-ttu-id="de6f0-254">I vår genomgången presenterar vi en kraftfull teknik som kallas [inlärning med antal](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) att koda dessa variabler på ett effektivt och robust sätt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-254">In our walkthrough, we present a powerful technique called [Learning With Counts](http://blogs.technet.com/b/machinelearning/archive/2015/02/17/big-learning-made-easy-with-counts.aspx) to encode these variables in an efficient, robust manner.</span></span> <span data-ttu-id="de6f0-255">Mer information om den här tekniken finns i länken.</span><span class="sxs-lookup"><span data-stu-id="de6f0-255">More information on this technique is in the link provided.</span></span>

[!NOTE]
><span data-ttu-id="de6f0-256">I den här genomgången ska fokusera vi på använder antal tabeller för att skapa compact representationer av hög endimensionell kategoriska funktioner.</span><span class="sxs-lookup"><span data-stu-id="de6f0-256">In this walkthrough, we focus on using count tables to produce compact representations of high-dimensional categorical features.</span></span> <span data-ttu-id="de6f0-257">Detta är inte det enda sättet att koda kategoriska funktioner; Mer information om andra metoder berörda användare kan checka ut [en-hot encoding](http://en.wikipedia.org/wiki/One-hot) och [hash-funktionen](http://en.wikipedia.org/wiki/Feature_hashing).</span><span class="sxs-lookup"><span data-stu-id="de6f0-257">This is not the only way to encode categorical features; for more information on other techniques, interested users can check out [one-hot-encoding](http://en.wikipedia.org/wiki/One-hot) and [feature hashing](http://en.wikipedia.org/wiki/Feature_hashing).</span></span>
>

<span data-ttu-id="de6f0-258">För att skapa antal tabeller på antalet data använder vi informationen i mappen rådata/antal.</span><span class="sxs-lookup"><span data-stu-id="de6f0-258">To build count tables on the count data, we use the data in the folder raw/count.</span></span> <span data-ttu-id="de6f0-259">I avsnittet modellering vi Visa användare skapa dessa antal tabeller för kategoriska funktioner från början, eller också använda en förskapad antal tabell för sina explorations.</span><span class="sxs-lookup"><span data-stu-id="de6f0-259">In the modeling section, we show users how to build these count tables for categorical features from scratch, or alternatively to use a pre-built count table for their explorations.</span></span> <span data-ttu-id="de6f0-260">I vilka sätt när vi refererar till ”förskapad antal tabeller”, menar vi med antalet tabeller som vi tillhandahåller.</span><span class="sxs-lookup"><span data-stu-id="de6f0-260">In what follows, when we refer to "pre-built count tables", we mean using the count tables that we provide.</span></span> <span data-ttu-id="de6f0-261">Detaljerade anvisningar om hur du kommer åt dessa tabeller finns i nästa avsnitt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-261">Detailed instructions on how to access these tables are provided in the next section.</span></span>

## <span data-ttu-id="de6f0-262"><a name="aml"></a>Skapa en modell med Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="de6f0-262"><a name="aml"></a> Build a model with Azure Machine Learning</span></span>
<span data-ttu-id="de6f0-263">Vår modell processen i Azure Machine Learning för att bygga följer de här stegen:</span><span class="sxs-lookup"><span data-stu-id="de6f0-263">Our model building process in Azure Machine Learning follows these steps:</span></span>

1. [<span data-ttu-id="de6f0-264">Hämta data från Hive-tabeller i Azure Machine Learning</span><span class="sxs-lookup"><span data-stu-id="de6f0-264">Get the data from Hive tables into Azure Machine Learning</span></span>](#step1)
2. [<span data-ttu-id="de6f0-265">Skapa experimentet: rensa data och featurize med antal tabeller</span><span class="sxs-lookup"><span data-stu-id="de6f0-265">Create the experiment: clean the data and featurize with count tables</span></span>](#step2)
3. [<span data-ttu-id="de6f0-266">Skapa, träna och betygsätta modellen</span><span class="sxs-lookup"><span data-stu-id="de6f0-266">Build, train, and score the model</span></span>](#step3)
4. [<span data-ttu-id="de6f0-267">Utvärdera modellen</span><span class="sxs-lookup"><span data-stu-id="de6f0-267">Evaluate the model</span></span>](#step4)
5. [<span data-ttu-id="de6f0-268">Publicera modellen som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="de6f0-268">Publish the model as a web-service</span></span>](#step5)

<span data-ttu-id="de6f0-269">Vi är nu redo att bygga modeller i Azure Machine Learning studio.</span><span class="sxs-lookup"><span data-stu-id="de6f0-269">Now we are ready to build models in Azure Machine Learning studio.</span></span> <span data-ttu-id="de6f0-270">Vår nedåt samplade data sparas som Hive-tabeller i klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-270">Our down sampled data is saved as Hive tables in the cluster.</span></span> <span data-ttu-id="de6f0-271">Vi använder Azure Machine Learning **importera Data** modulen att läsa informationen.</span><span class="sxs-lookup"><span data-stu-id="de6f0-271">We use the Azure Machine Learning **Import Data** module to read this data.</span></span> <span data-ttu-id="de6f0-272">Autentiseringsuppgifter för åtkomst till lagringskontot för det här klustret finns i följande.</span><span class="sxs-lookup"><span data-stu-id="de6f0-272">The credentials to access the storage account of this cluster are provided in what follows.</span></span>

### <span data-ttu-id="de6f0-273"><a name="step1"></a>Steg 1: Hämta data från Hive-tabeller i Azure Machine Learning med hjälp av modulen importera Data och markera ett maskininlärningsexperiment</span><span class="sxs-lookup"><span data-stu-id="de6f0-273"><a name="step1"></a> Step 1: Get data from Hive tables into Azure Machine Learning using the Import Data module and select it for a machine learning experiment</span></span>
<span data-ttu-id="de6f0-274">Starta genom att välja en **+ ny** -> **EXPERIMENT** -> **tomt Experiment**.</span><span class="sxs-lookup"><span data-stu-id="de6f0-274">Start by selecting a **+NEW** -> **EXPERIMENT** -> **Blank Experiment**.</span></span> <span data-ttu-id="de6f0-275">Sedan från den **Sök** rutan längst upp till vänster, Sök efter ”importera Data”.</span><span class="sxs-lookup"><span data-stu-id="de6f0-275">Then, from the **Search** box on the top left, search for "Import Data".</span></span> <span data-ttu-id="de6f0-276">Dra och släpp den **importera Data** modulen in experimentet arbetsytan (mellersta delen av skärmen) ska kunna använda modulen för dataåtkomst.</span><span class="sxs-lookup"><span data-stu-id="de6f0-276">Drag and drop the **Import Data** module on to the experiment canvas (the middle portion of the screen) to use the module for data access.</span></span>

<span data-ttu-id="de6f0-277">Det här är vad den **importera Data** ser ut som att hämta data från Hive-tabell:</span><span class="sxs-lookup"><span data-stu-id="de6f0-277">This is what the **Import Data** looks like while getting data from the Hive table:</span></span>

![Importera Data hämtar data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/i3zRaoj.png)

<span data-ttu-id="de6f0-279">För den **importera Data** modulen värdena för parametrarna som tillhandahålls i bilden är bara exempel på sorteringen av värden som du behöver ange.</span><span class="sxs-lookup"><span data-stu-id="de6f0-279">For the **Import Data** module, the values of the parameters that are provided in the graphic are just examples of the sort of values you need to provide.</span></span> <span data-ttu-id="de6f0-280">Här är några allmänna riktlinjer för hur du fyller i parameteruppsättning för de **importera Data** modul.</span><span class="sxs-lookup"><span data-stu-id="de6f0-280">Here is some general guidance on how to fill out the parameter set for the **Import Data** module.</span></span>

1. <span data-ttu-id="de6f0-281">Välj ”Hive-frågan” för **datakälla**</span><span class="sxs-lookup"><span data-stu-id="de6f0-281">Choose "Hive query" for **Data Source**</span></span>
2. <span data-ttu-id="de6f0-282">I den **Hive databasfrågan** , en enkel, MARKERAR du kryssrutan * FROM < din\_databasen\_name.your\_tabell\_namn >-räcker.</span><span class="sxs-lookup"><span data-stu-id="de6f0-282">In the **Hive database query** box, a simple SELECT * FROM <your\_database\_name.your\_table\_name> - is enough.</span></span>
3. <span data-ttu-id="de6f0-283">**Hcatalog server URI**: om klustret är ”abc” och sedan det här är bara: https://abc.azurehdinsight.net</span><span class="sxs-lookup"><span data-stu-id="de6f0-283">**Hcatalog server URI**: If your cluster is "abc", then this is simply: https://abc.azurehdinsight.net</span></span>
4. <span data-ttu-id="de6f0-284">**Hadoop användarkontonamnet**: användarnamnet som valts vid tidpunkten för idriftsättning klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-284">**Hadoop user account name**: The user name chosen at the time of commissioning the cluster.</span></span> <span data-ttu-id="de6f0-285">(Inte fjärråtkomst användarnamnet!)</span><span class="sxs-lookup"><span data-stu-id="de6f0-285">(NOT the Remote Access user name!)</span></span>
5. <span data-ttu-id="de6f0-286">**Hadoop lösenord**: lösenordet för användarnamnet som valts vid tidpunkten för idriftsättning klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-286">**Hadoop user account password**: The password for the user name chosen at the time of commissioning the cluster.</span></span> <span data-ttu-id="de6f0-287">(Inte fjärråtkomst lösenordet!)</span><span class="sxs-lookup"><span data-stu-id="de6f0-287">(NOT the Remote Access password!)</span></span>
6. <span data-ttu-id="de6f0-288">**Platsen för utdata**: Välj ”Azure”</span><span class="sxs-lookup"><span data-stu-id="de6f0-288">**Location of output data**: Choose "Azure"</span></span>
7. <span data-ttu-id="de6f0-289">**Azure lagringskontonamnet**: lagringskonto som associeras med klustret</span><span class="sxs-lookup"><span data-stu-id="de6f0-289">**Azure storage account name**: The storage account associated with the cluster</span></span>
8. <span data-ttu-id="de6f0-290">**Azure lagringskontonyckel**: nyckeln för lagringskontot som är associerade med klustret.</span><span class="sxs-lookup"><span data-stu-id="de6f0-290">**Azure storage account key**: The key of the storage account associated with the cluster.</span></span>
9. <span data-ttu-id="de6f0-291">**Azure behållarnamn**: Om klusternamnet är ”abc”, så det är helt enkelt ”abc”, vanligtvis.</span><span class="sxs-lookup"><span data-stu-id="de6f0-291">**Azure container name**: If the cluster name is "abc", then this is simply "abc", usually.</span></span>

<span data-ttu-id="de6f0-292">En gång i **importera Data** har avslutats hämtning av data (visas grönt skalstreck i modulen), spara informationen som en datamängd (med ett valfritt namn).</span><span class="sxs-lookup"><span data-stu-id="de6f0-292">Once the **Import Data** finishes getting data (you see the green tick on the Module), save this data as a Dataset (with a name of your choice).</span></span> <span data-ttu-id="de6f0-293">Det ser ut:</span><span class="sxs-lookup"><span data-stu-id="de6f0-293">What this looks like:</span></span>

![Importera Data spara data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oxM73Np.png)

<span data-ttu-id="de6f0-295">Högerklicka på utdataporten för den **importera Data** modul.</span><span class="sxs-lookup"><span data-stu-id="de6f0-295">Right-click the output port of the **Import Data** module.</span></span> <span data-ttu-id="de6f0-296">Nu visas en **Spara som dataset** alternativet och en **visualisera** alternativet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-296">This reveals a **Save as dataset** option and a **Visualize** option.</span></span> <span data-ttu-id="de6f0-297">Den **visualisera** alternativet om du visar 100 rader med data, tillsammans med en högra panelen som är användbar för vissa sammanfattande statistik.</span><span class="sxs-lookup"><span data-stu-id="de6f0-297">The **Visualize** option, if clicked, displays 100 rows of the data, along with a right panel that is useful for some summary statistics.</span></span> <span data-ttu-id="de6f0-298">För att spara data, markerar du bara **Spara som dataset** och följ instruktionerna.</span><span class="sxs-lookup"><span data-stu-id="de6f0-298">To save data, simply select **Save as dataset** and follow instructions.</span></span>

<span data-ttu-id="de6f0-299">Om du vill välja den sparade datamängden för användning i ett machine learning-experiment, hitta datauppsättningar med hjälp av den **Sök** rutan som visas i följande bild.</span><span class="sxs-lookup"><span data-stu-id="de6f0-299">To select the saved dataset for use in a machine learning experiment, locate the datasets using the **Search** box shown in the following figure.</span></span> <span data-ttu-id="de6f0-300">Sedan skriver ut namnet du gav dataset delvis vill komma åt den och dra datauppsättningen till åtgärdsfönstervärdens.</span><span class="sxs-lookup"><span data-stu-id="de6f0-300">Then simply type out the name you gave the dataset partially to access it and drag the dataset onto the main panel.</span></span> <span data-ttu-id="de6f0-301">Släppa på åtgärdsfönstervärdens markeras den för användning i machine learning modellering.</span><span class="sxs-lookup"><span data-stu-id="de6f0-301">Dropping it onto the main panel selects it for use in machine learning modeling.</span></span>

![Drage dataset till åtgärdsfönstervärdens](./media/machine-learning-data-science-process-hive-criteo-walkthrough/cl5tpGw.png)

> [!NOTE]
> <span data-ttu-id="de6f0-303">Gör detta för både tåg och testa datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-303">Do this for both the train and the test datasets.</span></span> <span data-ttu-id="de6f0-304">Dessutom Kom ihåg att använda databasnamnet och tabellnamn som du gav för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="de6f0-304">Also, remember to use the database name and table names that you gave for this purpose.</span></span> <span data-ttu-id="de6f0-305">De värden som används i bilden är endast för bilden purposes.* *</span><span class="sxs-lookup"><span data-stu-id="de6f0-305">The values used in the figure are solely for illustration purposes.**</span></span>
> 
> 

### <span data-ttu-id="de6f0-306"><a name="step2"></a>Steg 2: Skapa ett enkelt experiment i Azure Machine Learning att förutsäga klick / några klick</span><span class="sxs-lookup"><span data-stu-id="de6f0-306"><a name="step2"></a> Step 2: Create a simple experiment in Azure Machine Learning to predict clicks / no clicks</span></span>
<span data-ttu-id="de6f0-307">Vårt Azure ML-experiment ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-307">Our Azure ML experiment looks like this:</span></span>

![Machine Learning-experiment](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xRpVfrY.png)

<span data-ttu-id="de6f0-309">Vi nu undersöka huvudkomponenterna i experimentet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-309">We now examine the key components of this experiment.</span></span> <span data-ttu-id="de6f0-310">Som en påminnelse om behöver vi dra våra sparade tåg och testa datauppsättningar in våra experimentet först.</span><span class="sxs-lookup"><span data-stu-id="de6f0-310">As a reminder, we need to drag our saved train and test datasets on to our experiment canvas first.</span></span>

#### <a name="clean-missing-data"></a><span data-ttu-id="de6f0-311">Rensa Data som saknas</span><span class="sxs-lookup"><span data-stu-id="de6f0-311">Clean Missing Data</span></span>
<span data-ttu-id="de6f0-312">Den **Rensa Data som saknas** modulen har namnet antyder: data som saknas på ett sätt som kan vara användardefinierade rensas.</span><span class="sxs-lookup"><span data-stu-id="de6f0-312">The **Clean Missing Data** module does what its name suggests:  it cleans missing data in ways that can be user-specified.</span></span> <span data-ttu-id="de6f0-313">Titta på den här modulen finns vi här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-313">Looking into this module, we see this:</span></span>

![Rensa data som saknas](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0ycXod6.png)

<span data-ttu-id="de6f0-315">Vi valde här, ersätter alla värden som saknas med 0.</span><span class="sxs-lookup"><span data-stu-id="de6f0-315">Here, we chose to replace all missing values with a 0.</span></span> <span data-ttu-id="de6f0-316">Det finns andra alternativ, vilket kan ses genom att titta på de nedrullningsbara listorna i modulen.</span><span class="sxs-lookup"><span data-stu-id="de6f0-316">There are other options as well, which can be seen by looking at the dropdowns in the module.</span></span>

#### <a name="feature-engineering-on-the-data"></a><span data-ttu-id="de6f0-317">Egenskapsval på data</span><span class="sxs-lookup"><span data-stu-id="de6f0-317">Feature engineering on the data</span></span>
<span data-ttu-id="de6f0-318">Det kan finnas miljontals unika värden för vissa kategoriska funktioner i stora datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-318">There can be millions of unique values for some categorical features of large datasets.</span></span> <span data-ttu-id="de6f0-319">Med hjälp av naïve metoder, till exempel en hot kodning för att representera hög endimensionell kategoriska funktioner är helt unfeasible.</span><span class="sxs-lookup"><span data-stu-id="de6f0-319">Using naive methods such as one-hot encoding for representing such high-dimensional categorical features is entirely unfeasible.</span></span> <span data-ttu-id="de6f0-320">I den här genomgången visar vi hur du använder antal funktioner med hjälp av inbyggda Azure Machine Learning-moduler för att generera compact representationer av dessa hög endimensionell kategoriska variabler.</span><span class="sxs-lookup"><span data-stu-id="de6f0-320">In this walkthrough, we demonstrate how to use count features using built-in Azure Machine Learning modules to generate compact representations of these high-dimensional categorical variables.</span></span> <span data-ttu-id="de6f0-321">Slutresultatet blir mindre modellen, snabbare utbildning och resultatmått som är helt jämförbar med andra metoder.</span><span class="sxs-lookup"><span data-stu-id="de6f0-321">The end-result is a smaller model size, faster training times, and performance metrics that are quite comparable to using other techniques.</span></span>

##### <a name="building-counting-transforms"></a><span data-ttu-id="de6f0-322">Skapa inventering transformeringar</span><span class="sxs-lookup"><span data-stu-id="de6f0-322">Building counting transforms</span></span>
<span data-ttu-id="de6f0-323">För att skapa antalet funktioner som vi använder den **skapa inventering transformera** modul som finns tillgängliga i Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="de6f0-323">To build count features, we use the **Build Counting Transform** module that is available in Azure Machine Learning.</span></span> <span data-ttu-id="de6f0-324">Modulen ser ut så här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-324">The module looks like this:</span></span>

<span data-ttu-id="de6f0-325">![Skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![skapa inventering transformera modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span><span class="sxs-lookup"><span data-stu-id="de6f0-325">![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/e0eqKtZ.png)
![Build Counting Transform module](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OdDN0vw.png)</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="de6f0-326">I den **antal kolumner** vi rutan Ange de kolumner som vi vill utföra antal på.</span><span class="sxs-lookup"><span data-stu-id="de6f0-326">In the **Count columns** box, we enter those columns that we wish to perform counts on.</span></span> <span data-ttu-id="de6f0-327">Dessa normalt (som tidigare nämnts) hög endimensionell kategoriska kolumner.</span><span class="sxs-lookup"><span data-stu-id="de6f0-327">Typically, these are (as mentioned) high-dimensional categorical columns.</span></span> <span data-ttu-id="de6f0-328">I början, som vi nämnt att Criteo datamängden har 26 kategoriska kolumner: från Col15 till Col40.</span><span class="sxs-lookup"><span data-stu-id="de6f0-328">At the start, we mentioned that the Criteo dataset has 26 categorical columns: from Col15 to Col40.</span></span> <span data-ttu-id="de6f0-329">Här kan vi räkna på dem alla och ge sina index (från 15 till 40 avgränsade med kommatecken enligt).</span><span class="sxs-lookup"><span data-stu-id="de6f0-329">Here, we count on all of them and give their indices (from 15 to 40 separated by commas as shown).</span></span>
> 

<span data-ttu-id="de6f0-330">Använda modul i MapReduce-läge (lämplig för stora datauppsättningar), vi behöver åtkomst till ett HDInsight Hadoop-kluster (den som används för funktionen utforskning kan återanvändas för detta ändamål samt) och dess autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="de6f0-330">To use the module in the MapReduce mode (appropriate for large datasets), we need access to an HDInsight Hadoop cluster (the one used for feature exploration can be reused for this purpose as well) and its credentials.</span></span> <span data-ttu-id="de6f0-331">Tidigare siffror visar vilka ifyllda värdena se ut (ersätter värdena för jämförelseändamål med de som är relevanta för dina egna användningsfall).</span><span class="sxs-lookup"><span data-stu-id="de6f0-331">The  previous figures illustrate what the filled-in values look like (replace the values provided for illustration with those relevant for your own use-case).</span></span>

![Modulparametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/05IqySf.png)

<span data-ttu-id="de6f0-333">I bilden ovan visar vi hur du anger inkommande blobbplats.</span><span class="sxs-lookup"><span data-stu-id="de6f0-333">In the figure above, we show how to enter the input blob location.</span></span> <span data-ttu-id="de6f0-334">Den här platsen har reserverats för antalet tabeller som bygger på data.</span><span class="sxs-lookup"><span data-stu-id="de6f0-334">This location has the data reserved for building count tables on.</span></span>

<span data-ttu-id="de6f0-335">När den här modulen är klar vi spara transformeringen för senare genom att högerklicka på modulen och välja den **Spara som transformeringen** alternativ:</span><span class="sxs-lookup"><span data-stu-id="de6f0-335">After this module finishes running, we can save the transform for later by right-clicking the module and selecting the **Save as Transform** option:</span></span>

![Alternativet ”Spara som transformeringen”](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IcVgvHR.png)

<span data-ttu-id="de6f0-337">I vårt experiment arkitektur som visas ovan motsvarar dataset ”ytransform2” exakt en sparad antal transformering.</span><span class="sxs-lookup"><span data-stu-id="de6f0-337">In our experiment architecture shown above, the dataset "ytransform2" corresponds precisely to a saved count transform.</span></span> <span data-ttu-id="de6f0-338">I resten av experimentet vi antar att läsaren som en **skapa inventering transformera** modul på vissa data att generera inventeringar och kan sedan använda dessa antal om du vill generera antal funktioner på tåg och testa datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-338">For the remainder of this experiment, we assume that the reader used a **Build Counting Transform** module on some data to generate counts, and can then use those counts to generate count features on the train and test datasets.</span></span>

##### <a name="choosing-what-count-features-to-include-as-part-of-the-train-and-test-datasets"></a><span data-ttu-id="de6f0-339">Välja vilka antal funktioner för att inkludera som en del av tåg och testa datauppsättningar</span><span class="sxs-lookup"><span data-stu-id="de6f0-339">Choosing what count features to include as part of the train and test datasets</span></span>
<span data-ttu-id="de6f0-340">När vi har ett antal transformera redo användaren kan välja vilka funktioner som ska inkluderas i sina tåg och testa datauppsättningar med hjälp av den **ändra antalet tabellen parametrar** modul.</span><span class="sxs-lookup"><span data-stu-id="de6f0-340">Once we have a count transform ready, the user can choose what features to include in their train and test datasets using the **Modify Count Table Parameters** module.</span></span> <span data-ttu-id="de6f0-341">Vi bara att visa den här modulen här för fullständighetens skull, men i enkelhetens inte utnyttjar den i vårt experiment.</span><span class="sxs-lookup"><span data-stu-id="de6f0-341">We just show this module here for completeness, but in interests of simplicity do not actually use it in our experiment.</span></span>

![Ändra antalet tabellen parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/PfCHkVg.png)

<span data-ttu-id="de6f0-343">I det här fallet som kan ses har vi valt att använda bara log-oddsen och ignorera på baksidan av kolumn.</span><span class="sxs-lookup"><span data-stu-id="de6f0-343">In this case, as can be seen, we have chosen to use just the log-odds and to ignore the back off column.</span></span> <span data-ttu-id="de6f0-344">Vi kan också ange parametrar som skräp bin tröskelvärdet, hur många startvärden föregående exempel för att lägga till för Utjämning och om du vill använda alla Laplacian brus eller inte.</span><span class="sxs-lookup"><span data-stu-id="de6f0-344">We can also set parameters such as the garbage bin threshold, how many pseudo-prior examples to add for smoothing, and whether to use any Laplacian noise or not.</span></span> <span data-ttu-id="de6f0-345">Alla dessa avancerade funktioner och det är att märka att standardvärdena är en bra utgångspunkt för användare som är nya för den här typen av funktionen.</span><span class="sxs-lookup"><span data-stu-id="de6f0-345">All these are advanced features and it is to be noted that the default values are a good starting point for users who are new to this type of feature generation.</span></span>

##### <a name="data-transformation-before-generating-the-count-features"></a><span data-ttu-id="de6f0-346">DTS innan du genererar antal funktioner</span><span class="sxs-lookup"><span data-stu-id="de6f0-346">Data transformation before generating the count features</span></span>
<span data-ttu-id="de6f0-347">Nu vi fokusera på en viktig aspekt Omforma våra tåg och testa data innan du genererar faktiskt antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="de6f0-347">Now we focus on an important point about transforming our train and test data prior to actually generating count features.</span></span> <span data-ttu-id="de6f0-348">Observera att det finns två **köra R-skriptet** moduler som används innan vi använda antal transformeringen i våra data.</span><span class="sxs-lookup"><span data-stu-id="de6f0-348">Note that there are two **Execute R Script** modules used before we apply the count transform to our data.</span></span>

![Köra R-skriptet moduler](./media/machine-learning-data-science-process-hive-criteo-walkthrough/aF59wbc.png)

<span data-ttu-id="de6f0-350">Här är första R-skriptet:</span><span class="sxs-lookup"><span data-stu-id="de6f0-350">Here is the first R script:</span></span>

![Första R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/3hkIoMx.png)

<span data-ttu-id="de6f0-352">I det här R-skriptet kan vi Byt namn på vår kolumner till namn ”Kol1” till ”Col40”.</span><span class="sxs-lookup"><span data-stu-id="de6f0-352">In this R script, we rename our columns to names "Col1" to "Col40".</span></span> <span data-ttu-id="de6f0-353">Det beror på att antalet transformeringen förväntar namnen på det här formatet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-353">This is because the count transform expects names of this format.</span></span>

<span data-ttu-id="de6f0-354">I andra R-skriptet vi Utjämna distributionen mellan positiva och negativa klasser (klasserna 1 och 0 respektive) av nedsampling klassen negativt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-354">In the second R script, we balance the distribution between positive and negative classes (classes 1 and 0 respectively) by downsampling the negative class.</span></span> <span data-ttu-id="de6f0-355">R-skriptet här visas hur du gör detta:</span><span class="sxs-lookup"><span data-stu-id="de6f0-355">The R script here shows how to do this:</span></span>

![Andra R-skriptet](./media/machine-learning-data-science-process-hive-criteo-walkthrough/91wvcwN.png)

<span data-ttu-id="de6f0-357">I det här enkla R-skriptet kan vi använda ”pos\_neg\_förhållandet” ange mängden balans mellan positiva och negativa klasser.</span><span class="sxs-lookup"><span data-stu-id="de6f0-357">In this simple R script, we use "pos\_neg\_ratio" to set the amount of balance between the positive and the negative classes.</span></span> <span data-ttu-id="de6f0-358">Detta är viktigt att utföra eftersom förbättra klassen obalans vanligtvis har prestandafördelarna för klassificering problem där distributionen av klassen är förvrängd (återkallning att i vårt fall har vi 3.3% positivt klassen och 96.7% negativt klassen).</span><span class="sxs-lookup"><span data-stu-id="de6f0-358">This is important to do since improving class imbalance usually has performance benefits for classification problems where the class distribution is skewed (recall that in our case, we have 3.3% positive class and 96.7% negative class).</span></span>

##### <a name="applying-the-count-transformation-on-our-data"></a><span data-ttu-id="de6f0-359">Tillämpa count-transformation på våra data</span><span class="sxs-lookup"><span data-stu-id="de6f0-359">Applying the count transformation on our data</span></span>
<span data-ttu-id="de6f0-360">Slutligen kan vi använda den **gäller omvandling** modul som ska tillämpa antal transformeringar för våra tåg och testa datauppsättningar.</span><span class="sxs-lookup"><span data-stu-id="de6f0-360">Finally, we can use the **Apply Transformation** module to apply the count transforms on our train and test datasets.</span></span> <span data-ttu-id="de6f0-361">Den här modulen tar sparade count-transformeringen som en inmatning och tåg eller test datauppsättningar som andra indata och returnerar data med antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="de6f0-361">This module takes the saved count transform as one input and the train or test datasets as the other input, and returns data with count features.</span></span> <span data-ttu-id="de6f0-362">Den visas här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-362">It is shown here:</span></span>

![Tillämpa omvandling av modul](./media/machine-learning-data-science-process-hive-criteo-walkthrough/xnQvsYf.png)

##### <a name="an-excerpt-of-what-the-count-features-look-like"></a><span data-ttu-id="de6f0-364">Det ser ut som ett utdrag vilka antal funktioner</span><span class="sxs-lookup"><span data-stu-id="de6f0-364">An excerpt of what the count features look like</span></span>
<span data-ttu-id="de6f0-365">Det är nyttigt att se hur antalet funktioner som ser ut i vårt fall.</span><span class="sxs-lookup"><span data-stu-id="de6f0-365">It is instructive to see what the count features look like in our case.</span></span> <span data-ttu-id="de6f0-366">Här beskrivs ett utdrag ur detta:</span><span class="sxs-lookup"><span data-stu-id="de6f0-366">Here we show an excerpt of this:</span></span>

![Antal funktioner](./media/machine-learning-data-science-process-hive-criteo-walkthrough/FO1nNfw.png)

<span data-ttu-id="de6f0-368">Detta utdrag visar att för de kolumner som vi räknas på vi få fram de och loggar oddsen utöver eventuella relevanta backoffs.</span><span class="sxs-lookup"><span data-stu-id="de6f0-368">In this excerpt, we show that for the columns that we counted on, we get the counts and log odds in addition to any relevant backoffs.</span></span>

<span data-ttu-id="de6f0-369">Vi är nu redo att skapa en Azure Machine Learning-modell med hjälp av dessa transformerade data.</span><span class="sxs-lookup"><span data-stu-id="de6f0-369">We are now ready to build an Azure Machine Learning model using these transformed datasets.</span></span> <span data-ttu-id="de6f0-370">I nästa avsnitt visar vi hur detta kan göras.</span><span class="sxs-lookup"><span data-stu-id="de6f0-370">In the next section, we show how this can be done.</span></span>

### <span data-ttu-id="de6f0-371"><a name="step3"></a>Steg 3: Skapa, träna och betygsätta modellen</span><span class="sxs-lookup"><span data-stu-id="de6f0-371"><a name="step3"></a> Step 3: Build, train, and score the model</span></span>

#### <a name="choice-of-learner"></a><span data-ttu-id="de6f0-372">Valet av deltagaren</span><span class="sxs-lookup"><span data-stu-id="de6f0-372">Choice of learner</span></span>
<span data-ttu-id="de6f0-373">Vi måste först välja en deltagaren.</span><span class="sxs-lookup"><span data-stu-id="de6f0-373">First, we need to choose a learner.</span></span> <span data-ttu-id="de6f0-374">Vi kommer att använda ett två klassen förstärkta beslutsträd som våra deltagaren.</span><span class="sxs-lookup"><span data-stu-id="de6f0-374">We are going to use a two class boosted decision tree as our learner.</span></span> <span data-ttu-id="de6f0-375">Här är alternativ för den här deltagaren:</span><span class="sxs-lookup"><span data-stu-id="de6f0-375">Here are the default options for this learner:</span></span>

![Två-Tvåklassförhöjt beslutsträd parametrar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/bH3ST2z.png)

<span data-ttu-id="de6f0-377">För vårt experiment ska vi välja standardvärden.</span><span class="sxs-lookup"><span data-stu-id="de6f0-377">For our experiment, we are going to choose the default values.</span></span> <span data-ttu-id="de6f0-378">Vi Observera att standardvärdena är vanligtvis beskrivande och ett bra sätt att få snabb baslinjer för prestanda.</span><span class="sxs-lookup"><span data-stu-id="de6f0-378">We note that the defaults are usually meaningful and a good way to get quick baselines on performance.</span></span> <span data-ttu-id="de6f0-379">Du kan förbättra prestanda genom omfattande parametrar om du väljer när du har en baslinje.</span><span class="sxs-lookup"><span data-stu-id="de6f0-379">You can improve on performance by sweeping parameters if you choose to once you have a baseline.</span></span>

#### <a name="train-the-model"></a><span data-ttu-id="de6f0-380">Träna modellen</span><span class="sxs-lookup"><span data-stu-id="de6f0-380">Train the model</span></span>
<span data-ttu-id="de6f0-381">För träning, vi bara anropa en **Träningsmodell** modul.</span><span class="sxs-lookup"><span data-stu-id="de6f0-381">For training, we simply invoke a **Train Model** module.</span></span> <span data-ttu-id="de6f0-382">De två indatavärdena till den är två-Tvåklassförhöjt beslutsträd deltagaren och vår train datauppsättning.</span><span class="sxs-lookup"><span data-stu-id="de6f0-382">The two inputs to it are the Two-Class Boosted Decision Tree learner and our train dataset.</span></span> <span data-ttu-id="de6f0-383">Detta visas här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-383">This is shown here:</span></span>

![Modulen för Train-modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/2bZDZTy.png)

#### <a name="score-the-model"></a><span data-ttu-id="de6f0-385">Poängsätt modellen</span><span class="sxs-lookup"><span data-stu-id="de6f0-385">Score the model</span></span>
<span data-ttu-id="de6f0-386">När vi har en tränad modell är vi redo att poängsätta mot testdatauppsättningen och utvärdera dess prestanda.</span><span class="sxs-lookup"><span data-stu-id="de6f0-386">Once we have a trained model, we are ready to score on the test dataset and to evaluate its performance.</span></span> <span data-ttu-id="de6f0-387">Vi kan göra detta med hjälp av den **Poängmodell** modulen som visas i följande bild, tillsammans med en **utvärdera modell** modulen:</span><span class="sxs-lookup"><span data-stu-id="de6f0-387">We do this by using the **Score Model** module shown in the following figure, along with an **Evaluate Model** module:</span></span>

![Modulen Poängsätta modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/fydcv6u.png)

### <span data-ttu-id="de6f0-389"><a name="step4"></a>Steg 4: Utvärdera modellen</span><span class="sxs-lookup"><span data-stu-id="de6f0-389"><a name="step4"></a> Step 4: Evaluate the model</span></span>
<span data-ttu-id="de6f0-390">Slutligen vill vi analysera modellen prestanda.</span><span class="sxs-lookup"><span data-stu-id="de6f0-390">Finally, we would like to analyze model performance.</span></span> <span data-ttu-id="de6f0-391">Vanligtvis är två klass (binär) klassificering problem ett bra mått på AUC.</span><span class="sxs-lookup"><span data-stu-id="de6f0-391">Usually, for two class (binary) classification problems, a good measure is the AUC.</span></span> <span data-ttu-id="de6f0-392">Om du vill visualisera detta vi koppla samman den **Poängmodell** modulen till en **utvärdera modell** -modul för detta.</span><span class="sxs-lookup"><span data-stu-id="de6f0-392">To visualize this, we hook up the **Score Model** module to an **Evaluate Model** module for this.</span></span> <span data-ttu-id="de6f0-393">Klicka på **visualisera** på den **utvärdera modell** modulen ger en bild som den följande:</span><span class="sxs-lookup"><span data-stu-id="de6f0-393">Clicking **Visualize** on the **Evaluate Model** module yields a graphic like the following one:</span></span>

![Utvärdera modulen BDT modellen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/0Tl0cdg.png)

<span data-ttu-id="de6f0-395">I binär (eller två klassen) klassificering problem, ett bra mått på förutsägelsefunktionen är det område i kurvan (AUC).</span><span class="sxs-lookup"><span data-stu-id="de6f0-395">In binary (or two class) classification problems, a good measure of prediction accuracy is the Area Under Curve (AUC).</span></span> <span data-ttu-id="de6f0-396">I följande, visar vi våra resultat med hjälp av den här modellen på vår testdata.</span><span class="sxs-lookup"><span data-stu-id="de6f0-396">In what follows, we show our results using this model on our test dataset.</span></span> <span data-ttu-id="de6f0-397">För att få detta, högerklickar du på utdataporten för den **utvärdera modell** modulen och sedan **visualisera**.</span><span class="sxs-lookup"><span data-stu-id="de6f0-397">To get this, right-click the output port of the **Evaluate Model** module and then **Visualize**.</span></span>

![Visualisera modulen utvärdera modell](./media/machine-learning-data-science-process-hive-criteo-walkthrough/IRfc7fH.png)

### <span data-ttu-id="de6f0-399"><a name="step5"></a>Steg 5: Publicera modellen som en webbtjänst</span><span class="sxs-lookup"><span data-stu-id="de6f0-399"><a name="step5"></a> Step 5: Publish the model as a Web service</span></span>
<span data-ttu-id="de6f0-400">Möjligheten att publicera en Azure Machine Learning-modell som webbtjänster med minsta möjliga ansträngning är en viktig funktion för att göra den allmänt tillgänglig.</span><span class="sxs-lookup"><span data-stu-id="de6f0-400">The ability to publish an Azure Machine Learning model as web services with a minimum of fuss is a valuable feature for making it widely available.</span></span> <span data-ttu-id="de6f0-401">När det är klart, göra vem som helst anrop till webbtjänsten med indata att de behöver förutsägelser för och webbtjänsten använder modellen för att returnera dessa förutsägelser.</span><span class="sxs-lookup"><span data-stu-id="de6f0-401">Once that is done, anyone can make calls to the web service with input data that they need predictions for, and the web service uses the model to return those predictions.</span></span>

<span data-ttu-id="de6f0-402">Detta gör spara vi först vår tränad modell som en Trained Model-objektet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-402">To do this, we first save our trained model as a Trained Model object.</span></span> <span data-ttu-id="de6f0-403">Detta görs genom att högerklicka på den **Träningsmodell** modulen och använder den **Spara som Trained Model** alternativet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-403">This is done by right-clicking the **Train Model** module and using the **Save as Trained Model** option.</span></span>

<span data-ttu-id="de6f0-404">Därefter behöver vi skapa inkommande och utgående portar för våra webbtjänst:</span><span class="sxs-lookup"><span data-stu-id="de6f0-404">Next, we need to create input and output ports for our web service:</span></span>

* <span data-ttu-id="de6f0-405">en port hämtar data i samma formulär som data som vi behöver förutsägelser för</span><span class="sxs-lookup"><span data-stu-id="de6f0-405">an input port takes data in the same form as the data that we need predictions for</span></span>
* <span data-ttu-id="de6f0-406">en utdataporten returnerar poängsatta etiketter och associerade troliga.</span><span class="sxs-lookup"><span data-stu-id="de6f0-406">an output port returns the Scored Labels and the associated probabilities.</span></span>

#### <a name="select-a-few-rows-of-data-for-the-input-port"></a><span data-ttu-id="de6f0-407">Välj ett fåtal rader med data för den inkommande porten</span><span class="sxs-lookup"><span data-stu-id="de6f0-407">Select a few rows of data for the input port</span></span>
<span data-ttu-id="de6f0-408">Är det praktiskt att använda en **gäller SQL omvandling** modulen att välja bara 10 rader att fungera som indataport data.</span><span class="sxs-lookup"><span data-stu-id="de6f0-408">It is convenient to use an **Apply SQL Transformation** module to select just 10 rows to serve as the input port data.</span></span> <span data-ttu-id="de6f0-409">Välj bara dessa rader med data för våra indataport använda SQL-fråga som visas här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-409">Select just these rows of data for our input port using the SQL query shown here:</span></span>

![Indataport data](./media/machine-learning-data-science-process-hive-criteo-walkthrough/XqVtSxu.png)

#### <a name="web-service"></a><span data-ttu-id="de6f0-411">Webbtjänst</span><span class="sxs-lookup"><span data-stu-id="de6f0-411">Web service</span></span>
<span data-ttu-id="de6f0-412">Vi är nu redo att köra ett litet experiment som kan användas för att publicera våra webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="de6f0-412">Now we are ready to run a small experiment that can be used to publish our web service.</span></span>

#### <a name="generate-input-data-for-webservice"></a><span data-ttu-id="de6f0-413">Generera indata för webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="de6f0-413">Generate input data for webservice</span></span>
<span data-ttu-id="de6f0-414">Som ett zeroth steg eftersom tabellen antal är stort, vi tar några rader testdata och generera utdata från det antal funktioner.</span><span class="sxs-lookup"><span data-stu-id="de6f0-414">As a zeroth step, since the count table is large, we take a few lines of test data and generate output data from it with count features.</span></span> <span data-ttu-id="de6f0-415">Detta kan fungera som indata för våra webservice.</span><span class="sxs-lookup"><span data-stu-id="de6f0-415">This can serve as the input data format for our webservice.</span></span> <span data-ttu-id="de6f0-416">Detta visas här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-416">This is shown here:</span></span>

![Skapa BDT indata](./media/machine-learning-data-science-process-hive-criteo-walkthrough/OEJMmst.png)

> [!NOTE]
> <span data-ttu-id="de6f0-418">För formatet indata vi nu använda utdata från den **antal Featurizer** modul.</span><span class="sxs-lookup"><span data-stu-id="de6f0-418">For the input data format, we now use the OUTPUT of the **Count Featurizer** module.</span></span> <span data-ttu-id="de6f0-419">När detta experimentera är klar kör spara utdata från den **antal Featurizer** modulen som en datamängd.</span><span class="sxs-lookup"><span data-stu-id="de6f0-419">Once this experiment finishes running, save the output from the **Count Featurizer** module as a Dataset.</span></span> <span data-ttu-id="de6f0-420">Den här datauppsättningen används för indata i den här webbtjänsten.</span><span class="sxs-lookup"><span data-stu-id="de6f0-420">This Dataset is used for the input data in the webservice.</span></span>
> 
> 

#### <a name="scoring-experiment-for-publishing-webservice"></a><span data-ttu-id="de6f0-421">Bedömningen experiment för publishing webbtjänsten</span><span class="sxs-lookup"><span data-stu-id="de6f0-421">Scoring experiment for publishing webservice</span></span>
<span data-ttu-id="de6f0-422">Först visar vi hur det ser ut.</span><span class="sxs-lookup"><span data-stu-id="de6f0-422">First, we show what this looks like.</span></span> <span data-ttu-id="de6f0-423">Den grundläggande strukturen är en **Poängmodell** modul som accepterar våra trained model-objektet och några få rader av indata som vi skapade i föregående steg med hjälp av den **antal Featurizer** modul.</span><span class="sxs-lookup"><span data-stu-id="de6f0-423">The essential structure is a **Score Model** module that accepts our trained model object and a few lines of input data that we generated in the previous steps using the **Count Featurizer** module.</span></span> <span data-ttu-id="de6f0-424">Vi använder ”Välj kolumner i datauppsättning” för projektet Scored etiketter och troliga poäng.</span><span class="sxs-lookup"><span data-stu-id="de6f0-424">We use "Select Columns in Dataset" to project out the Scored labels and the Score probabilities.</span></span>

![Välj kolumner i datauppsättning](./media/machine-learning-data-science-process-hive-criteo-walkthrough/kRHrIbe.png)

<span data-ttu-id="de6f0-426">Observera hur **Välj kolumner i datauppsättning** modul kan användas för ”filtrera' data från en datamängd.</span><span class="sxs-lookup"><span data-stu-id="de6f0-426">Notice how the **Select Columns in Dataset** module can be used for 'filtering' data out from a dataset.</span></span> <span data-ttu-id="de6f0-427">Vi visa innehållet här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-427">We show the contents here:</span></span>

![Välj kolumner i datauppsättning modulen-filtrering](./media/machine-learning-data-science-process-hive-criteo-walkthrough/oVUJC9K.png)

<span data-ttu-id="de6f0-429">För att få blå ingående och utgående portar, klickar du på **förbereda webservice** längst ned rätt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-429">To get the blue input and output ports, you simply click **prepare webservice** at the bottom right.</span></span> <span data-ttu-id="de6f0-430">Kör experimentet också möjlighet att publicera webbtjänsten: Klicka på den **publicera WEBBTJÄNSTEN** längst ned rätt, visas här:</span><span class="sxs-lookup"><span data-stu-id="de6f0-430">Running this experiment also allows us to publish the web service: click the **PUBLISH WEB SERVICE** icon at the bottom right, shown here:</span></span>

![Publicera Web service](./media/machine-learning-data-science-process-hive-criteo-walkthrough/WO0nens.png)

<span data-ttu-id="de6f0-432">När webbtjänsten har publicerats kan hämta vi dirigeras till en sida som ser ut därför:</span><span class="sxs-lookup"><span data-stu-id="de6f0-432">Once the webservice is published, we get redirected to a page that looks thus:</span></span>

![Web service-instrumentpanelen](./media/machine-learning-data-science-process-hive-criteo-walkthrough/YKzxAA5.png)

<span data-ttu-id="de6f0-434">Det finns två länkar webservices på vänster sida:</span><span class="sxs-lookup"><span data-stu-id="de6f0-434">We see two links for webservices on the left side:</span></span>

* <span data-ttu-id="de6f0-435">Den **frågor och svar** Service (eller Resursposter) är avsedd för enskild förutsägelser och är vad vi utnyttjar i den här workshopen.</span><span class="sxs-lookup"><span data-stu-id="de6f0-435">The **REQUEST/RESPONSE** Service (or RRS) is meant for single predictions and is what we utilize in this workshop.</span></span>
* <span data-ttu-id="de6f0-436">Den **BATCH EXECUTION** Service BES-används för batch förutsägelser och kräver att indata används för att göra förutsägelser som finns i Azure Blob Storage.</span><span class="sxs-lookup"><span data-stu-id="de6f0-436">The **BATCH EXECUTION** Service (BES) is used for batch predictions and requires that the input data used to make predictions reside in Azure Blob Storage.</span></span>

<span data-ttu-id="de6f0-437">Klicka på länken **frågor och svar** tar vi en sida som ger oss före burk koden i C#, python och R. Den här koden kan användas för att göra anrop till webbtjänsten enkelt.</span><span class="sxs-lookup"><span data-stu-id="de6f0-437">Clicking on the link **REQUEST/RESPONSE** takes us to a page that gives us pre-canned code in C#, python, and R. This code can be conveniently used for making calls to the webservice.</span></span> <span data-ttu-id="de6f0-438">Observera att API-nyckel på den här sidan måste användas för autentisering.</span><span class="sxs-lookup"><span data-stu-id="de6f0-438">Note that the API key on this page needs to be used for authentication.</span></span>

<span data-ttu-id="de6f0-439">Det är praktiskt att kopiera koden python över till en ny cell i IPython anteckningsboken.</span><span class="sxs-lookup"><span data-stu-id="de6f0-439">It is convenient to copy this python code over to a new cell in the IPython notebook.</span></span>

<span data-ttu-id="de6f0-440">Här visar vi en del av python-kod med rätt API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="de6f0-440">Here we show a segment of python code with the correct API key.</span></span>

![Python-kod](./media/machine-learning-data-science-process-hive-criteo-walkthrough/f8N4L4g.png)

<span data-ttu-id="de6f0-442">Observera att vi ersatt standard API-nyckel med vår webservices API-nyckel.</span><span class="sxs-lookup"><span data-stu-id="de6f0-442">Note that we replaced the default API key with our webservices's API key.</span></span> <span data-ttu-id="de6f0-443">Klicka på **kör** för den här cellen i en IPython anteckningsboken ger följande svar:</span><span class="sxs-lookup"><span data-stu-id="de6f0-443">Clicking **Run** on this cell in an IPython notebook yields the following response:</span></span>

![IPython svar](./media/machine-learning-data-science-process-hive-criteo-walkthrough/KSxmia2.png)

<span data-ttu-id="de6f0-445">Vi kan se att två test exempel vi frågade om (i JSON-ramverket för python-skriptet), vi få tillbaka svar i formatet ”Scored etiketterna, Scored sannolikhet”.</span><span class="sxs-lookup"><span data-stu-id="de6f0-445">We see that for the two test examples we asked about (in the JSON framework of the python script), we get back answers in the form "Scored Labels, Scored Probabilities".</span></span> <span data-ttu-id="de6f0-446">Observera att i det här fallet vi valde standardvärdena som fördefinierad kod ger (0 för alla numeriska kolumner och strängen ”värde” för alla kategoriska kolumner).</span><span class="sxs-lookup"><span data-stu-id="de6f0-446">Note that in this case, we chose the default values that the pre-canned code provides (0's for all numeric columns and the string "value" for all categorical columns).</span></span>

<span data-ttu-id="de6f0-447">Detta avslutar våra slutpunkt till slutpunkt genomgången visar hur du hanterar storskaliga dataset med hjälp av Azure Machine Learning.</span><span class="sxs-lookup"><span data-stu-id="de6f0-447">This concludes our end-to-end walkthrough showing how to handle large-scale dataset using Azure Machine Learning.</span></span> <span data-ttu-id="de6f0-448">Vi igång med en terabyte data, skapas en förutsägelse modell och distribueras som en webbtjänst i molnet.</span><span class="sxs-lookup"><span data-stu-id="de6f0-448">We started with a terabyte of data, constructed a prediction model and deployed it as a web service in the cloud.</span></span>

