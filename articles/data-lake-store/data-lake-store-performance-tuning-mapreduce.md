---
title: aaaAzure Data Lake Store MapReduce prestanda justera riktlinjer | Microsoft Docs
description: Azure Data Lake Store MapReduce prestandajustering riktlinjer
services: data-lake-store
documentationcenter: 
author: stewu
manager: amitkul
editor: stewu
ms.assetid: ebde7b9f-2e51-4d43-b7ab-566417221335
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 12/19/2016
ms.author: stewu
ms.openlocfilehash: e21414a23530e65613c85156af0209c88ec54d2d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="ab65d-103">Prestandajustering för MapReduce på HDInsight och Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="ab65d-103">Performance tuning guidance for MapReduce on HDInsight and Azure Data Lake Store</span></span>


## <a name="prerequisites"></a><span data-ttu-id="ab65d-104">Krav</span><span class="sxs-lookup"><span data-stu-id="ab65d-104">Prerequisites</span></span>

* <span data-ttu-id="ab65d-105">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="ab65d-105">**An Azure subscription**.</span></span> <span data-ttu-id="ab65d-106">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ab65d-106">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ab65d-107">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="ab65d-107">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="ab65d-108">Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="ab65d-108">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="ab65d-109">**Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="ab65d-109">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="ab65d-110">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ab65d-110">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="ab65d-111">Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="ab65d-111">Make sure you enable Remote Desktop for hello cluster.</span></span>
* <span data-ttu-id="ab65d-112">**Använda MapReduce på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="ab65d-112">**Using MapReduce on HDInsight**.</span></span>  <span data-ttu-id="ab65d-113">Mer information finns i [använda MapReduce i Hadoop i HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span><span class="sxs-lookup"><span data-stu-id="ab65d-113">For more information, see [Use MapReduce in Hadoop on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span></span>  
* <span data-ttu-id="ab65d-114">**Prestandajustering riktlinjer för ADLS**.</span><span class="sxs-lookup"><span data-stu-id="ab65d-114">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="ab65d-115">Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="ab65d-115">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>  

## <a name="parameters"></a><span data-ttu-id="ab65d-116">Parametrar</span><span class="sxs-lookup"><span data-stu-id="ab65d-116">Parameters</span></span>

<span data-ttu-id="ab65d-117">När du kör MapReduce-jobb är här hello viktigaste parametrarna som du kan konfigurera tooincrease prestanda på ADLS:</span><span class="sxs-lookup"><span data-stu-id="ab65d-117">When running MapReduce jobs, here are hello most important parameters that you can configure tooincrease performance on ADLS:</span></span>

* <span data-ttu-id="ab65d-118">**Mapreduce.Map.Memory.MB** – hello mängden minne tooallocate tooeach mapper</span><span class="sxs-lookup"><span data-stu-id="ab65d-118">**Mapreduce.map.memory.mb** – hello amount of memory tooallocate tooeach mapper</span></span>
* <span data-ttu-id="ab65d-119">**Mapreduce.Job.Maps** – hello antal kartan uppgifter per jobb</span><span class="sxs-lookup"><span data-stu-id="ab65d-119">**Mapreduce.job.maps** – hello number of map tasks per job</span></span>
* <span data-ttu-id="ab65d-120">**Mapreduce.Reduce.Memory.MB** – hello mängden minne tooallocate tooeach reducer</span><span class="sxs-lookup"><span data-stu-id="ab65d-120">**Mapreduce.reduce.memory.mb** – hello amount of memory tooallocate tooeach reducer</span></span>
* <span data-ttu-id="ab65d-121">**Mapreduce.Job.reduces** – hello antal minska uppgifter per jobb</span><span class="sxs-lookup"><span data-stu-id="ab65d-121">**Mapreduce.job.reduces** – hello number of reduce tasks per job</span></span>

<span data-ttu-id="ab65d-122">**Mapreduce.Map.Memory / Mapreduce.reduce.memory** siffran ska justeras baserat på hur mycket minne som krävs för hello kartan och/eller minska aktivitet.</span><span class="sxs-lookup"><span data-stu-id="ab65d-122">**Mapreduce.map.memory / Mapreduce.reduce.memory** This number should be adjusted based on how much memory is needed for hello map and/or reduce task.</span></span>  <span data-ttu-id="ab65d-123">hello standardvärdena för mapreduce.map.memory och mapreduce.reduce.memory kan visas i Ambari via hello Yarn konfiguration.</span><span class="sxs-lookup"><span data-stu-id="ab65d-123">hello default values of mapreduce.map.memory and mapreduce.reduce.memory can be viewed in Ambari via hello Yarn configuration.</span></span>  <span data-ttu-id="ab65d-124">Navigera tooYARN i Ambari, och visa hello konfigurationerna fliken.  Hej YARN minne visas.</span><span class="sxs-lookup"><span data-stu-id="ab65d-124">In Ambari, navigate tooYARN and view hello Configs tab.  hello YARN memory will be displayed.</span></span>  

<span data-ttu-id="ab65d-125">**Mapreduce.Job.Maps / Mapreduce.job.reduces** det avgör hello maximalt antal mappers eller förminskningsapparater toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="ab65d-125">**Mapreduce.job.maps / Mapreduce.job.reduces** This will determine hello maximum number of mappers or reducers toobe created.</span></span>  <span data-ttu-id="ab65d-126">hello antal delningar avgör hur många mappers kommer att skapas för hello MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="ab65d-126">hello number of splits will determine how many mappers will be created for hello MapReduce job.</span></span>  <span data-ttu-id="ab65d-127">Därför kan du få mindre mappers du begärt om det finns mindre delningar än hello antalet mappers som begärdes.</span><span class="sxs-lookup"><span data-stu-id="ab65d-127">Therefore, you may get less mappers than you requested if there are less splits than hello number of mappers requested.</span></span>       

## <a name="guidance"></a><span data-ttu-id="ab65d-128">Riktlinjer</span><span class="sxs-lookup"><span data-stu-id="ab65d-128">Guidance</span></span>

<span data-ttu-id="ab65d-129">**Steg 1: Bestäm antalet jobb som körs** -standard MapReduce använder hello hela klustret på jobbet.</span><span class="sxs-lookup"><span data-stu-id="ab65d-129">**Step 1: Determine number of jobs running** - By default, MapReduce will use hello entire cluster for your job.</span></span>  <span data-ttu-id="ab65d-130">Du kan använda mindre hello kluster med mindre mappers än det finns tillgängliga behållare.</span><span class="sxs-lookup"><span data-stu-id="ab65d-130">You can use less of hello cluster by using less mappers than there are available containers.</span></span>  <span data-ttu-id="ab65d-131">hello riktlinjerna i det här dokumentet förutsätter att programmet hello endast program som körs på klustret.</span><span class="sxs-lookup"><span data-stu-id="ab65d-131">hello guidance in this document assumes that your application is hello only application running on your cluster.</span></span>      

<span data-ttu-id="ab65d-132">**Steg 2: Ange mapreduce.map.memory/mapreduce.reduce.memory** – hello storlek hello minne för kartan och minska uppgifter är beroende av ditt specifika jobb.</span><span class="sxs-lookup"><span data-stu-id="ab65d-132">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** –  hello size of hello memory for map and reduce tasks will be dependent on your specific job.</span></span>  <span data-ttu-id="ab65d-133">Du kan minska hello minnesstorleken om du vill tooincrease samtidighet.</span><span class="sxs-lookup"><span data-stu-id="ab65d-133">You can reduce hello memory size if you want tooincrease concurrency.</span></span>  <span data-ttu-id="ab65d-134">hello antalet aktiviteter som körs samtidigt beror på hello antal behållare.</span><span class="sxs-lookup"><span data-stu-id="ab65d-134">hello number of concurrently running tasks depends on hello number of containers.</span></span>  <span data-ttu-id="ab65d-135">Genom att minska hello mängden minne per mapper eller reducer kan fler behållare skapas, som gör det möjligt mer mappers eller förminskningsapparater toorun samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ab65d-135">By decreasing hello amount of memory per mapper or reducer, more containers can be created, which enable more mappers or reducers toorun concurrently.</span></span>  <span data-ttu-id="ab65d-136">Minskar hello mängden minne kan för mycket orsaka vissa processer toorun slut på minne.</span><span class="sxs-lookup"><span data-stu-id="ab65d-136">Decreasing hello amount of memory too much may cause some processes toorun out of memory.</span></span>  <span data-ttu-id="ab65d-137">Om du får en heap-fel när du kör jobbet, bör du öka hello minne per mapper eller reducer.</span><span class="sxs-lookup"><span data-stu-id="ab65d-137">If you get a heap error when running your job, you should increase hello memory per mapper or reducer.</span></span>  <span data-ttu-id="ab65d-138">Du bör överväga att lägga till fler behållare lägger till extra administration för varje ytterligare behållare som potentiellt kan försämra prestanda.</span><span class="sxs-lookup"><span data-stu-id="ab65d-138">You should consider that adding more containers will add extra overhead for each additional container, which can potentially degrade performance.</span></span>  <span data-ttu-id="ab65d-139">Ett annat alternativ är tooget mer minne genom att använda ett kluster som har högre mängder minne eller öka hello antalet noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="ab65d-139">Another alternative is tooget more memory by using a cluster that has higher amounts of memory or increasing hello number of nodes in your cluster.</span></span>  <span data-ttu-id="ab65d-140">Mer minne kan flera behållare toobe används, vilket innebär att flera samtidiga.</span><span class="sxs-lookup"><span data-stu-id="ab65d-140">More memory will enable more containers toobe used, which means more concurrency.</span></span>  

<span data-ttu-id="ab65d-141">**Steg 3: Bestäm minne totalt YARN** -tootune mapreduce.job.maps/mapreduce.job.reduces bör du hello minnesmängden totala YARN tillgängliga för användning.</span><span class="sxs-lookup"><span data-stu-id="ab65d-141">**Step 3: Determine Total YARN memory** - tootune mapreduce.job.maps/mapreduce.job.reduces, you should consider hello amount of total YARN memory available for use.</span></span>  <span data-ttu-id="ab65d-142">Den här informationen är tillgänglig i Ambari.</span><span class="sxs-lookup"><span data-stu-id="ab65d-142">This information is available in Ambari.</span></span>  <span data-ttu-id="ab65d-143">Navigera tooYARN och visa hello konfigurationerna fliken.  Hej YARN minne visas i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="ab65d-143">Navigate tooYARN and view hello Configs tab.  hello YARN memory is displayed in this window.</span></span>  <span data-ttu-id="ab65d-144">Du bör multiplicera hello YARN minne med hello antalet noder i klustret tooget hello totala YARN minnet.</span><span class="sxs-lookup"><span data-stu-id="ab65d-144">You should multiply hello YARN memory with hello number of nodes in your cluster tooget hello total YARN memory.</span></span>

    Total YARN memory = nodes * YARN memory per node
<span data-ttu-id="ab65d-145">Om du använder ett tomt kluster kan minne vara hello totalt YARN minne för klustret.</span><span class="sxs-lookup"><span data-stu-id="ab65d-145">If you are using an empty cluster, then memory can be hello total YARN memory for your cluster.</span></span>  <span data-ttu-id="ab65d-146">Om andra program använder minne, kan du välja tooonly används en del av ditt kluster minne genom att minska hello antalet mappers eller förminskningsapparater toohello behållare vill du toouse.</span><span class="sxs-lookup"><span data-stu-id="ab65d-146">If other applications are using memory, then you can choose tooonly use a portion of your cluster’s memory by reducing hello number of mappers or reducers toohello number of containers you want toouse.</span></span>  

<span data-ttu-id="ab65d-147">**Steg 4: Beräkna antalet YARN behållare** – YARN-behållare kräver hello mängden samtidighet för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="ab65d-147">**Step 4: Calculate number of YARN containers** – YARN containers dictate hello amount of concurrency available for hello job.</span></span>  <span data-ttu-id="ab65d-148">Ta totalt YARN-minne och delar av mapreduce.map.memory.</span><span class="sxs-lookup"><span data-stu-id="ab65d-148">Take total YARN memory and divide that by mapreduce.map.memory.</span></span>  

    # of YARN containers = total YARN memory / mapreduce.map.memory

<span data-ttu-id="ab65d-149">**Steg 5: Ange mapreduce.job.maps/mapreduce.job.reduces** ange mapreduce.job.maps/mapreduce.job.reduces tooat minst hello antalet tillgängliga behållare.</span><span class="sxs-lookup"><span data-stu-id="ab65d-149">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces** Set mapreduce.job.maps/mapreduce.job.reduces tooat least hello number of available containers.</span></span>  <span data-ttu-id="ab65d-150">Du kan experimentera ytterligare genom att öka hello antalet mappers och förminskningsapparater toosee om du får bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="ab65d-150">You can experiment further by increasing hello number of mappers and reducers toosee if you get better performance.</span></span>  <span data-ttu-id="ab65d-151">Tänk på att flera mappers har ytterligare kostnader så har för många mappers kan prestanda försämras.</span><span class="sxs-lookup"><span data-stu-id="ab65d-151">Keep in mind that more mappers will have additional overhead so having too many mappers may degrade performance.</span></span>  

<span data-ttu-id="ab65d-152">CPU-schemaläggning och CPU-isolering är inaktiverade som standard så hello antal YARN-behållare är begränsad av minne.</span><span class="sxs-lookup"><span data-stu-id="ab65d-152">CPU scheduling and CPU isolation are turned off by default so hello number of YARN containers is constrained by memory.</span></span>

## <a name="example-calculation"></a><span data-ttu-id="ab65d-153">Exempel beräkning</span><span class="sxs-lookup"><span data-stu-id="ab65d-153">Example Calculation</span></span>

<span data-ttu-id="ab65d-154">Anta att du har ett kluster som består av 8 D14 noder och du vill toorun ett i/o-intensiva jobb.</span><span class="sxs-lookup"><span data-stu-id="ab65d-154">Let’s say you currently have a cluster composed of 8 D14 nodes and you want toorun an I/O intensive job.</span></span>  <span data-ttu-id="ab65d-155">Här följer hello beräkningar som du bör göra:</span><span class="sxs-lookup"><span data-stu-id="ab65d-155">Here are hello calculations you should do:</span></span>

<span data-ttu-id="ab65d-156">**Steg 1: Bestäm antalet jobb som körs** -i vårt exempel antar vi att våra jobbet är hello endast en körs.</span><span class="sxs-lookup"><span data-stu-id="ab65d-156">**Step 1: Determine number of jobs running** - for our example, we assume that our job is hello only one running.</span></span>  

<span data-ttu-id="ab65d-157">**Steg 2: Ange mapreduce.map.memory/mapreduce.reduce.memory** – i vårt exempel du kör ett i/o-intensiva jobb och bestämmer 3 GB minne för kartan uppgifter är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="ab65d-157">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** – for our example, you are running an I/O intensive job and decide that 3GB of memory for map tasks will be sufficient.</span></span>

    mapreduce.map.memory = 3GB
<span data-ttu-id="ab65d-158">**Steg 3: Bestäm totala YARN-minne**</span><span class="sxs-lookup"><span data-stu-id="ab65d-158">**Step 3: Determine Total YARN memory**</span></span>

    total memory from hello cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
<span data-ttu-id="ab65d-159">**Steg 4: Beräkna antal YARN-behållare**</span><span class="sxs-lookup"><span data-stu-id="ab65d-159">**Step 4: Calculate # of YARN containers**</span></span>

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

<span data-ttu-id="ab65d-160">**Steg 5: Ange mapreduce.job.maps/mapreduce.job.reduces**</span><span class="sxs-lookup"><span data-stu-id="ab65d-160">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces**</span></span>

    mapreduce.map.jobs = 256

## <a name="limitations"></a><span data-ttu-id="ab65d-161">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="ab65d-161">Limitations</span></span>

<span data-ttu-id="ab65d-162">**ADLS-begränsning**</span><span class="sxs-lookup"><span data-stu-id="ab65d-162">**ADLS throttling**</span></span>

<span data-ttu-id="ab65d-163">Som en tjänst med flera innehavare anger ADLS gränser för nivån bandbredd.</span><span class="sxs-lookup"><span data-stu-id="ab65d-163">As a multi-tenant service, ADLS sets account level bandwidth limits.</span></span>  <span data-ttu-id="ab65d-164">Om du träffar dessa gränser, startar toosee aktivitet fel.</span><span class="sxs-lookup"><span data-stu-id="ab65d-164">If you hit these limits, you will start toosee task failures.</span></span> <span data-ttu-id="ab65d-165">Det kan identifieras av sett bandbreddsbegränsning fel i loggarna för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="ab65d-165">This can be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="ab65d-166">Om du behöver mer bandbredd för jobbet, kontaktar du oss.</span><span class="sxs-lookup"><span data-stu-id="ab65d-166">If you need more bandwidth for your job, please contact us.</span></span>   

<span data-ttu-id="ab65d-167">toocheck om du har komma begränsats måste tooenable hello felsökningsloggning på hello på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="ab65d-167">toocheck if you are getting throttled, you need tooenable hello debug logging on hello client side.</span></span> <span data-ttu-id="ab65d-168">Här visas hur du kan göra det:</span><span class="sxs-lookup"><span data-stu-id="ab65d-168">Here’s how you can do that:</span></span>

1. <span data-ttu-id="ab65d-169">Placera hello följande egenskap i hello log4j egenskaper i Ambari > YARN > Config > avancerade yarn log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span><span class="sxs-lookup"><span data-stu-id="ab65d-169">Put hello following property in hello log4j properties in Ambari > YARN > Config > Advanced yarn-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span></span>

2. <span data-ttu-id="ab65d-170">Starta om alla hello noder/för hello config tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="ab65d-170">Restart all hello nodes/service for hello config tootake effect.</span></span>

3. <span data-ttu-id="ab65d-171">Om du har komma begränsats visas hello HTTP 429 felkod i hello YARN-loggfilen.</span><span class="sxs-lookup"><span data-stu-id="ab65d-171">If you are getting throttled, you’ll see hello HTTP 429 error code in hello YARN log file.</span></span> <span data-ttu-id="ab65d-172">Hej YARN loggfilen finns i /tmp/&lt;användare&gt;/yarn.log</span><span class="sxs-lookup"><span data-stu-id="ab65d-172">hello YARN log file is in /tmp/&lt;user&gt;/yarn.log</span></span>

## <a name="examples-toorun"></a><span data-ttu-id="ab65d-173">Exempel tooRun</span><span class="sxs-lookup"><span data-stu-id="ab65d-173">Examples tooRun</span></span>

<span data-ttu-id="ab65d-174">toodemonstrate visas hur MapReduce körs på Azure Data Lake Store nedan några exempelkod som körs på ett kluster med hello följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="ab65d-174">toodemonstrate how MapReduce runs on Azure Data Lake Store, below is some sample code that was run on a cluster with hello following settings:</span></span>

* <span data-ttu-id="ab65d-175">16 noden D14v2</span><span class="sxs-lookup"><span data-stu-id="ab65d-175">16 node D14v2</span></span>
* <span data-ttu-id="ab65d-176">Hadoop-kluster som kör HDI 3,6</span><span class="sxs-lookup"><span data-stu-id="ab65d-176">Hadoop cluster running HDI 3.6</span></span>

<span data-ttu-id="ab65d-177">För en startpunkt är här några exempel kommandon toorun MapReduce Teragen Terasort och Teravalidate.</span><span class="sxs-lookup"><span data-stu-id="ab65d-177">For a starting point, here are some example commands toorun MapReduce Teragen, Terasort, and Teravalidate.</span></span>  <span data-ttu-id="ab65d-178">Du kan justera de här kommandona baserat på dina resurser.</span><span class="sxs-lookup"><span data-stu-id="ab65d-178">You can adjust these commands based on your resources.</span></span>

<span data-ttu-id="ab65d-179">**Teragen**</span><span class="sxs-lookup"><span data-stu-id="ab65d-179">**Teragen**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

<span data-ttu-id="ab65d-180">**Terasort**</span><span class="sxs-lookup"><span data-stu-id="ab65d-180">**Terasort**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

<span data-ttu-id="ab65d-181">**Teravalidate**</span><span class="sxs-lookup"><span data-stu-id="ab65d-181">**Teravalidate**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
