---
title: Azure Data Lake Store MapReduce prestandajustering riktlinjer | Microsoft Docs
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
ms.openlocfilehash: 9528148792f083cb0e48d356e61cf61762ee954f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-mapreduce-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="511c9-103">Prestandajustering för MapReduce på HDInsight och Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="511c9-103">Performance tuning guidance for MapReduce on HDInsight and Azure Data Lake Store</span></span>


## <a name="prerequisites"></a><span data-ttu-id="511c9-104">Krav</span><span class="sxs-lookup"><span data-stu-id="511c9-104">Prerequisites</span></span>

* <span data-ttu-id="511c9-105">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="511c9-105">**An Azure subscription**.</span></span> <span data-ttu-id="511c9-106">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="511c9-106">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="511c9-107">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="511c9-107">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="511c9-108">Anvisningar om hur du skapar en finns [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="511c9-108">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="511c9-109">**Azure HDInsight-kluster** med åtkomst till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="511c9-109">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="511c9-110">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="511c9-110">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="511c9-111">Kontrollera att du kan aktivera Fjärrskrivbord för klustret.</span><span class="sxs-lookup"><span data-stu-id="511c9-111">Make sure you enable Remote Desktop for the cluster.</span></span>
* <span data-ttu-id="511c9-112">**Använda MapReduce på HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="511c9-112">**Using MapReduce on HDInsight**.</span></span>  <span data-ttu-id="511c9-113">Mer information finns i [använda MapReduce i Hadoop i HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span><span class="sxs-lookup"><span data-stu-id="511c9-113">For more information, see [Use MapReduce in Hadoop on HDInsight](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-mapreduce)</span></span>  
* <span data-ttu-id="511c9-114">**Prestandajustering riktlinjer för ADLS**.</span><span class="sxs-lookup"><span data-stu-id="511c9-114">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="511c9-115">Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="511c9-115">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>  

## <a name="parameters"></a><span data-ttu-id="511c9-116">Parametrar</span><span class="sxs-lookup"><span data-stu-id="511c9-116">Parameters</span></span>

<span data-ttu-id="511c9-117">När du kör MapReduce-jobb, är här de viktigaste parametrarna som du kan konfigurera för att öka prestanda hos ADLS:</span><span class="sxs-lookup"><span data-stu-id="511c9-117">When running MapReduce jobs, here are the most important parameters that you can configure to increase performance on ADLS:</span></span>

* <span data-ttu-id="511c9-118">**Mapreduce.Map.Memory.MB** – hur mycket minne som ska allokeras till varje mapper</span><span class="sxs-lookup"><span data-stu-id="511c9-118">**Mapreduce.map.memory.mb** – The amount of memory to allocate to each mapper</span></span>
* <span data-ttu-id="511c9-119">**Mapreduce.Job.Maps** – antal kartan uppgifter per jobb</span><span class="sxs-lookup"><span data-stu-id="511c9-119">**Mapreduce.job.maps** – The number of map tasks per job</span></span>
* <span data-ttu-id="511c9-120">**Mapreduce.Reduce.Memory.MB** – hur mycket minne som ska allokeras till varje reducer</span><span class="sxs-lookup"><span data-stu-id="511c9-120">**Mapreduce.reduce.memory.mb** – The amount of memory to allocate to each reducer</span></span>
* <span data-ttu-id="511c9-121">**Mapreduce.Job.reduces** – antal minska uppgifter per jobb</span><span class="sxs-lookup"><span data-stu-id="511c9-121">**Mapreduce.job.reduces** – The number of reduce tasks per job</span></span>

<span data-ttu-id="511c9-122">**Mapreduce.Map.Memory / Mapreduce.reduce.memory** siffran ska justeras baserat på hur mycket minne som krävs för kartan och/eller minska aktivitet.</span><span class="sxs-lookup"><span data-stu-id="511c9-122">**Mapreduce.map.memory / Mapreduce.reduce.memory** This number should be adjusted based on how much memory is needed for the map and/or reduce task.</span></span>  <span data-ttu-id="511c9-123">Standardvärdena för mapreduce.map.memory och mapreduce.reduce.memory kan visas i Ambari via Yarn-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="511c9-123">The default values of mapreduce.map.memory and mapreduce.reduce.memory can be viewed in Ambari via the Yarn configuration.</span></span>  <span data-ttu-id="511c9-124">Navigera till YARN i Ambari, och visar fliken konfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="511c9-124">In Ambari, navigate to YARN and view the Configs tab.</span></span>  <span data-ttu-id="511c9-125">YARN-minne visas.</span><span class="sxs-lookup"><span data-stu-id="511c9-125">The YARN memory will be displayed.</span></span>  

<span data-ttu-id="511c9-126">**Mapreduce.Job.Maps / Mapreduce.job.reduces** det avgör det maximala antalet mappers eller förminskningsapparater ska skapas.</span><span class="sxs-lookup"><span data-stu-id="511c9-126">**Mapreduce.job.maps / Mapreduce.job.reduces** This will determine the maximum number of mappers or reducers to be created.</span></span>  <span data-ttu-id="511c9-127">Antal delningar avgör hur många mappers kommer att skapas för MapReduce-jobb.</span><span class="sxs-lookup"><span data-stu-id="511c9-127">The number of splits will determine how many mappers will be created for the MapReduce job.</span></span>  <span data-ttu-id="511c9-128">Därför kan du få mindre mappers du begärt om det finns mindre delningar än antalet mappers som begärdes.</span><span class="sxs-lookup"><span data-stu-id="511c9-128">Therefore, you may get less mappers than you requested if there are less splits than the number of mappers requested.</span></span>       

## <a name="guidance"></a><span data-ttu-id="511c9-129">Riktlinjer</span><span class="sxs-lookup"><span data-stu-id="511c9-129">Guidance</span></span>

<span data-ttu-id="511c9-130">**Steg 1: Bestäm antalet jobb som körs** -standard MapReduce använder hela klustret på jobbet.</span><span class="sxs-lookup"><span data-stu-id="511c9-130">**Step 1: Determine number of jobs running** - By default, MapReduce will use the entire cluster for your job.</span></span>  <span data-ttu-id="511c9-131">Du kan använda mindre klustret med mindre mappers än det finns tillgängliga behållare.</span><span class="sxs-lookup"><span data-stu-id="511c9-131">You can use less of the cluster by using less mappers than there are available containers.</span></span>  <span data-ttu-id="511c9-132">Riktlinjerna i det här dokumentet förutsätter att programmet endast program som körs på klustret.</span><span class="sxs-lookup"><span data-stu-id="511c9-132">The guidance in this document assumes that your application is the only application running on your cluster.</span></span>      

<span data-ttu-id="511c9-133">**Steg 2: Ange mapreduce.map.memory/mapreduce.reduce.memory** – storleken på minnet för kartan och minska uppgifter är beroende av ditt specifika jobb.</span><span class="sxs-lookup"><span data-stu-id="511c9-133">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** –  The size of the memory for map and reduce tasks will be dependent on your specific job.</span></span>  <span data-ttu-id="511c9-134">Du kan minska minnesstorleken om du vill öka parallellkörningen.</span><span class="sxs-lookup"><span data-stu-id="511c9-134">You can reduce the memory size if you want to increase concurrency.</span></span>  <span data-ttu-id="511c9-135">Antalet aktiviteter som körs samtidigt beror på hur många behållare.</span><span class="sxs-lookup"><span data-stu-id="511c9-135">The number of concurrently running tasks depends on the number of containers.</span></span>  <span data-ttu-id="511c9-136">Genom att minska mängden minne per mapper eller reducer kan fler behållare skapas, vilket aktiverar fler mappers eller förminskningsapparater körs samtidigt.</span><span class="sxs-lookup"><span data-stu-id="511c9-136">By decreasing the amount of memory per mapper or reducer, more containers can be created, which enable more mappers or reducers to run concurrently.</span></span>  <span data-ttu-id="511c9-137">Minska mängden minne för mycket kanske vissa processer köras, slut på minne.</span><span class="sxs-lookup"><span data-stu-id="511c9-137">Decreasing the amount of memory too much may cause some processes to run out of memory.</span></span>  <span data-ttu-id="511c9-138">Om du får en heap-fel när du kör jobbet kan öka du minne per mapper eller reducer.</span><span class="sxs-lookup"><span data-stu-id="511c9-138">If you get a heap error when running your job, you should increase the memory per mapper or reducer.</span></span>  <span data-ttu-id="511c9-139">Du bör överväga att lägga till fler behållare lägger till extra administration för varje ytterligare behållare som potentiellt kan försämra prestanda.</span><span class="sxs-lookup"><span data-stu-id="511c9-139">You should consider that adding more containers will add extra overhead for each additional container, which can potentially degrade performance.</span></span>  <span data-ttu-id="511c9-140">Ett annat alternativ är att hämta mer minne med hjälp av ett kluster med större mängder minne eller öka antalet noder i klustret.</span><span class="sxs-lookup"><span data-stu-id="511c9-140">Another alternative is to get more memory by using a cluster that has higher amounts of memory or increasing the number of nodes in your cluster.</span></span>  <span data-ttu-id="511c9-141">Mer minne aktiverar fler behållare som ska användas, vilket innebär att flera samtidiga.</span><span class="sxs-lookup"><span data-stu-id="511c9-141">More memory will enable more containers to be used, which means more concurrency.</span></span>  

<span data-ttu-id="511c9-142">**Steg 3: Bestäm totala YARN minne** - om du vill justera mapreduce.job.maps/mapreduce.job.reduces, bör du mängden totala YARN minne som är tillgängliga för användning.</span><span class="sxs-lookup"><span data-stu-id="511c9-142">**Step 3: Determine Total YARN memory** - To tune mapreduce.job.maps/mapreduce.job.reduces, you should consider the amount of total YARN memory available for use.</span></span>  <span data-ttu-id="511c9-143">Den här informationen är tillgänglig i Ambari.</span><span class="sxs-lookup"><span data-stu-id="511c9-143">This information is available in Ambari.</span></span>  <span data-ttu-id="511c9-144">Navigera till YARN och visar fliken konfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="511c9-144">Navigate to YARN and view the Configs tab.</span></span>  <span data-ttu-id="511c9-145">YARN-minne visas i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="511c9-145">The YARN memory is displayed in this window.</span></span>  <span data-ttu-id="511c9-146">Du bör multiplicera YARN-minne med antalet noder i klustret för att hämta den totala mängden minnet på YARN.</span><span class="sxs-lookup"><span data-stu-id="511c9-146">You should multiply the YARN memory with the number of nodes in your cluster to get the total YARN memory.</span></span>

    Total YARN memory = nodes * YARN memory per node
<span data-ttu-id="511c9-147">Om du använder ett tomt kluster kan minne vara den totala mängden minnet YARN för klustret.</span><span class="sxs-lookup"><span data-stu-id="511c9-147">If you are using an empty cluster, then memory can be the total YARN memory for your cluster.</span></span>  <span data-ttu-id="511c9-148">Om andra program använder minne, kan du välja att bara använda en del av ditt kluster minne genom att minska antalet mappers eller förminskningsapparater antal behållare som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="511c9-148">If other applications are using memory, then you can choose to only use a portion of your cluster’s memory by reducing the number of mappers or reducers to the number of containers you want to use.</span></span>  

<span data-ttu-id="511c9-149">**Steg 4: Beräkna antalet YARN behållare** – YARN-behållare kräver mängden samtidighet som är tillgängliga för projektet.</span><span class="sxs-lookup"><span data-stu-id="511c9-149">**Step 4: Calculate number of YARN containers** – YARN containers dictate the amount of concurrency available for the job.</span></span>  <span data-ttu-id="511c9-150">Ta totalt YARN-minne och delar av mapreduce.map.memory.</span><span class="sxs-lookup"><span data-stu-id="511c9-150">Take total YARN memory and divide that by mapreduce.map.memory.</span></span>  

    # of YARN containers = total YARN memory / mapreduce.map.memory

<span data-ttu-id="511c9-151">**Steg 5: Ange mapreduce.job.maps/mapreduce.job.reduces** sägs mapreduce.job.maps/mapreduce.job.reduces minst antal tillgängliga behållare.</span><span class="sxs-lookup"><span data-stu-id="511c9-151">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces** Set mapreduce.job.maps/mapreduce.job.reduces to at least the number of available containers.</span></span>  <span data-ttu-id="511c9-152">Du kan experimentera ytterligare genom att öka antalet mappers och förminskningsapparater för att se om du får bättre prestanda.</span><span class="sxs-lookup"><span data-stu-id="511c9-152">You can experiment further by increasing the number of mappers and reducers to see if you get better performance.</span></span>  <span data-ttu-id="511c9-153">Tänk på att flera mappers har ytterligare kostnader så har för många mappers kan prestanda försämras.</span><span class="sxs-lookup"><span data-stu-id="511c9-153">Keep in mind that more mappers will have additional overhead so having too many mappers may degrade performance.</span></span>  

<span data-ttu-id="511c9-154">CPU-schemaläggning och CPU-isolering är inaktiverade som standard så att antalet YARN behållare är begränsad av minne.</span><span class="sxs-lookup"><span data-stu-id="511c9-154">CPU scheduling and CPU isolation are turned off by default so the number of YARN containers is constrained by memory.</span></span>

## <a name="example-calculation"></a><span data-ttu-id="511c9-155">Exempel beräkning</span><span class="sxs-lookup"><span data-stu-id="511c9-155">Example Calculation</span></span>

<span data-ttu-id="511c9-156">Anta att du har ett kluster som består av 8 D14 noder och du vill köra ett i/o-intensiva jobb.</span><span class="sxs-lookup"><span data-stu-id="511c9-156">Let’s say you currently have a cluster composed of 8 D14 nodes and you want to run an I/O intensive job.</span></span>  <span data-ttu-id="511c9-157">Här följer de beräkningar som du bör göra:</span><span class="sxs-lookup"><span data-stu-id="511c9-157">Here are the calculations you should do:</span></span>

<span data-ttu-id="511c9-158">**Steg 1: Bestäm antalet jobb som körs** -i vårt exempel antar vi att våra jobbet är den enda körs.</span><span class="sxs-lookup"><span data-stu-id="511c9-158">**Step 1: Determine number of jobs running** - for our example, we assume that our job is the only one running.</span></span>  

<span data-ttu-id="511c9-159">**Steg 2: Ange mapreduce.map.memory/mapreduce.reduce.memory** – i vårt exempel du kör ett i/o-intensiva jobb och bestämmer 3 GB minne för kartan uppgifter är tillräckligt.</span><span class="sxs-lookup"><span data-stu-id="511c9-159">**Step 2: Set mapreduce.map.memory/mapreduce.reduce.memory** – for our example, you are running an I/O intensive job and decide that 3GB of memory for map tasks will be sufficient.</span></span>

    mapreduce.map.memory = 3GB
<span data-ttu-id="511c9-160">**Steg 3: Bestäm totala YARN-minne**</span><span class="sxs-lookup"><span data-stu-id="511c9-160">**Step 3: Determine Total YARN memory**</span></span>

    total memory from the cluster is 8 nodes * 96GB of YARN memory for a D14 = 768GB
<span data-ttu-id="511c9-161">**Steg 4: Beräkna antal YARN-behållare**</span><span class="sxs-lookup"><span data-stu-id="511c9-161">**Step 4: Calculate # of YARN containers**</span></span>

    # of YARN containers = 768GB of available memory / 3 GB of memory =   256

<span data-ttu-id="511c9-162">**Steg 5: Ange mapreduce.job.maps/mapreduce.job.reduces**</span><span class="sxs-lookup"><span data-stu-id="511c9-162">**Step 5: Set mapreduce.job.maps/mapreduce.job.reduces**</span></span>

    mapreduce.map.jobs = 256

## <a name="limitations"></a><span data-ttu-id="511c9-163">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="511c9-163">Limitations</span></span>

<span data-ttu-id="511c9-164">**ADLS-begränsning**</span><span class="sxs-lookup"><span data-stu-id="511c9-164">**ADLS throttling**</span></span>

<span data-ttu-id="511c9-165">Som en tjänst med flera innehavare anger ADLS gränser för nivån bandbredd.</span><span class="sxs-lookup"><span data-stu-id="511c9-165">As a multi-tenant service, ADLS sets account level bandwidth limits.</span></span>  <span data-ttu-id="511c9-166">Om du träffar dessa gränser, startar du att se misslyckade aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="511c9-166">If you hit these limits, you will start to see task failures.</span></span> <span data-ttu-id="511c9-167">Det kan identifieras av sett bandbreddsbegränsning fel i loggarna för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="511c9-167">This can be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="511c9-168">Om du behöver mer bandbredd för jobbet, kontaktar du oss.</span><span class="sxs-lookup"><span data-stu-id="511c9-168">If you need more bandwidth for your job, please contact us.</span></span>   

<span data-ttu-id="511c9-169">Om du vill kontrollera om du har komma begränsats, måste du Aktivera felsökningsloggning på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="511c9-169">To check if you are getting throttled, you need to enable the debug logging on the client side.</span></span> <span data-ttu-id="511c9-170">Här visas hur du kan göra det:</span><span class="sxs-lookup"><span data-stu-id="511c9-170">Here’s how you can do that:</span></span>

1. <span data-ttu-id="511c9-171">Placera följande egenskap i log4j-egenskaper i Ambari > YARN > Config > avancerade yarn log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span><span class="sxs-lookup"><span data-stu-id="511c9-171">Put the following property in the log4j properties in Ambari > YARN > Config > Advanced yarn-log4j: log4j.logger.com.microsoft.azure.datalake.store=DEBUG</span></span>

2. <span data-ttu-id="511c9-172">Starta om alla noder/för konfigurationen ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="511c9-172">Restart all the nodes/service for the config to take effect.</span></span>

3. <span data-ttu-id="511c9-173">Om du har komma begränsats visas felkoden HTTP 429 i loggfilen YARN.</span><span class="sxs-lookup"><span data-stu-id="511c9-173">If you are getting throttled, you’ll see the HTTP 429 error code in the YARN log file.</span></span> <span data-ttu-id="511c9-174">YARN loggfilen finns i /tmp/&lt;användare&gt;/yarn.log</span><span class="sxs-lookup"><span data-stu-id="511c9-174">The YARN log file is in /tmp/&lt;user&gt;/yarn.log</span></span>

## <a name="examples-to-run"></a><span data-ttu-id="511c9-175">Exempel för att köra</span><span class="sxs-lookup"><span data-stu-id="511c9-175">Examples to Run</span></span>

<span data-ttu-id="511c9-176">För att demonstrera hur MapReduce körs på Azure Data Lake Store, visas nedan några exempelkod som körs på ett kluster med följande inställningar:</span><span class="sxs-lookup"><span data-stu-id="511c9-176">To demonstrate how MapReduce runs on Azure Data Lake Store, below is some sample code that was run on a cluster with the following settings:</span></span>

* <span data-ttu-id="511c9-177">16 noden D14v2</span><span class="sxs-lookup"><span data-stu-id="511c9-177">16 node D14v2</span></span>
* <span data-ttu-id="511c9-178">Hadoop-kluster som kör HDI 3,6</span><span class="sxs-lookup"><span data-stu-id="511c9-178">Hadoop cluster running HDI 3.6</span></span>

<span data-ttu-id="511c9-179">Här är några exempel för att köra MapReduce Teragen, Terasort och Teravalidate för en startpunkt.</span><span class="sxs-lookup"><span data-stu-id="511c9-179">For a starting point, here are some example commands to run MapReduce Teragen, Terasort, and Teravalidate.</span></span>  <span data-ttu-id="511c9-180">Du kan justera de här kommandona baserat på dina resurser.</span><span class="sxs-lookup"><span data-stu-id="511c9-180">You can adjust these commands based on your resources.</span></span>

<span data-ttu-id="511c9-181">**Teragen**</span><span class="sxs-lookup"><span data-stu-id="511c9-181">**Teragen**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teragen -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 10000000000 adl://example/data/1TB-sort-input

<span data-ttu-id="511c9-182">**Terasort**</span><span class="sxs-lookup"><span data-stu-id="511c9-182">**Terasort**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar terasort -Dmapreduce.job.maps=2048 -Dmapreduce.map.memory.mb=3072 -Dmapreduce.job.reduces=512 -Dmapreduce.reduce.memory.mb=3072 adl://example/data/1TB-sort-input adl://example/data/1TB-sort-output

<span data-ttu-id="511c9-183">**Teravalidate**</span><span class="sxs-lookup"><span data-stu-id="511c9-183">**Teravalidate**</span></span>

    yarn jar /usr/hdp/current/hadoop-mapreduce-client/hadoop-mapreduce-examples.jar teravalidate -Dmapreduce.job.maps=512 -Dmapreduce.map.memory.mb=3072 adl://example/data/1TB-sort-output adl://example/data/1TB-sort-validate
