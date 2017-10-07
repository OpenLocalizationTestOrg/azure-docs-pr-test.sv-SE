---
title: aaaAzure Data Lake Store Hive prestanda justera riktlinjer | Microsoft Docs
description: Azure Data Lake Store Hive prestandajustering riktlinjer
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
ms.openlocfilehash: e44daeb6ad3b64e893c709df63b56444a330729f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="afd3a-103">Prestandajustering för Hive i HDInsight och Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="afd3a-103">Performance tuning guidance for Hive on HDInsight and Azure Data Lake Store</span></span>

<span data-ttu-id="afd3a-104">hello standardinställningarna har ställts in tooprovide goda prestanda i många olika användningsfall.</span><span class="sxs-lookup"><span data-stu-id="afd3a-104">hello default settings have been set tooprovide good performance across many different use cases.</span></span>  <span data-ttu-id="afd3a-105">För i/o-intensiva frågor vara Hive ögonen öppna tooget bättre prestanda med ADLS.</span><span class="sxs-lookup"><span data-stu-id="afd3a-105">For I/O intensive queries, Hive can be tuned tooget better performance with ADLS.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="afd3a-106">Krav</span><span class="sxs-lookup"><span data-stu-id="afd3a-106">Prerequisites</span></span>

* <span data-ttu-id="afd3a-107">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="afd3a-107">**An Azure subscription**.</span></span> <span data-ttu-id="afd3a-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="afd3a-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="afd3a-109">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="afd3a-109">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="afd3a-110">Anvisningar för hur toocreate en, se [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="afd3a-110">For instructions on how toocreate one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="afd3a-111">**Azure HDInsight-kluster** med åtkomst tooa Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="afd3a-111">**Azure HDInsight cluster** with access tooa Data Lake Store account.</span></span> <span data-ttu-id="afd3a-112">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="afd3a-112">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="afd3a-113">Kontrollera att du kan aktivera Fjärrskrivbord för hello kluster.</span><span class="sxs-lookup"><span data-stu-id="afd3a-113">Make sure you enable Remote Desktop for hello cluster.</span></span>
* <span data-ttu-id="afd3a-114">**Kör Hive i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="afd3a-114">**Running Hive on HDInsight**.</span></span>  <span data-ttu-id="afd3a-115">toolearn om hur du kör Hive-jobb i HDInsight, se [använda Hive i HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span><span class="sxs-lookup"><span data-stu-id="afd3a-115">toolearn about running Hive jobs on HDInsight, see [Use Hive on HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span></span>
* <span data-ttu-id="afd3a-116">**Prestandajustering riktlinjer för ADLS**.</span><span class="sxs-lookup"><span data-stu-id="afd3a-116">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="afd3a-117">Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="afd3a-117">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>

## <a name="parameters"></a><span data-ttu-id="afd3a-118">Parametrar</span><span class="sxs-lookup"><span data-stu-id="afd3a-118">Parameters</span></span>

<span data-ttu-id="afd3a-119">Här följer hello viktigaste inställningar tootune för bättre prestanda för ADLS:</span><span class="sxs-lookup"><span data-stu-id="afd3a-119">Here are hello most important settings tootune for improved ADLS performance:</span></span>

* <span data-ttu-id="afd3a-120">**hive.tez.container.size** – hello mängden minne som används av varje uppgifter</span><span class="sxs-lookup"><span data-stu-id="afd3a-120">**hive.tez.container.size** – hello amount of memory used by each tasks</span></span>

* <span data-ttu-id="afd3a-121">**tez.Grouping.min storlek** – minsta storlek varje mapper</span><span class="sxs-lookup"><span data-stu-id="afd3a-121">**tez.grouping.min-size** – minimum size of each mapper</span></span>

* <span data-ttu-id="afd3a-122">**tez.Grouping.Max storlek** – maximal storlek för varje mapper</span><span class="sxs-lookup"><span data-stu-id="afd3a-122">**tez.grouping.max-size** – maximum size of each mapper</span></span>

* <span data-ttu-id="afd3a-123">**hive.Exec.Reducer.bytes.per.Reducer** – storlek varje reducer</span><span class="sxs-lookup"><span data-stu-id="afd3a-123">**hive.exec.reducer.bytes.per.reducer** – size of each reducer</span></span>

<span data-ttu-id="afd3a-124">**hive.tez.container.size** -hello behållarens storlek avgör hur mycket minne som är tillgänglig för varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="afd3a-124">**hive.tez.container.size** - hello container size determines how much memory is available for each task.</span></span>  <span data-ttu-id="afd3a-125">Detta är hello huvudsakliga indata för styra hello samtidighet i Hive.</span><span class="sxs-lookup"><span data-stu-id="afd3a-125">This is hello main input for controlling hello concurrency in Hive.</span></span>  

<span data-ttu-id="afd3a-126">**tez.Grouping.min storlek** – den här parametern kan tooset hello minimala storleken för varje mappning.</span><span class="sxs-lookup"><span data-stu-id="afd3a-126">**tez.grouping.min-size** – This parameter allows you tooset hello minimum size of each mapper.</span></span>  <span data-ttu-id="afd3a-127">Om hello antalet mappers som Tez väljer är mindre än hello värdet på parametern använder Tez hello-värdet som anges här.</span><span class="sxs-lookup"><span data-stu-id="afd3a-127">If hello number of mappers that Tez chooses is smaller than hello value of this parameter, then Tez will use hello value set here.</span></span>  

<span data-ttu-id="afd3a-128">**tez.Grouping.Max storlek** – hello-parametern kan du tooset hello maximal storlek för varje mappning.</span><span class="sxs-lookup"><span data-stu-id="afd3a-128">**tez.grouping.max-size** – hello parameter allows you tooset hello maximum size of each mapper.</span></span>  <span data-ttu-id="afd3a-129">Om hello antalet mappers som Tez väljer är större än hello värdet på parametern använder Tez hello-värdet som anges här.</span><span class="sxs-lookup"><span data-stu-id="afd3a-129">If hello number of mappers that Tez chooses is larger than hello value of this parameter, then Tez will use hello value set here.</span></span>  

<span data-ttu-id="afd3a-130">**hive.Exec.Reducer.bytes.per.Reducer** – den här parametern anger hello storleken för varje reducer.</span><span class="sxs-lookup"><span data-stu-id="afd3a-130">**hive.exec.reducer.bytes.per.reducer** – This parameter sets hello size of each reducer.</span></span>  <span data-ttu-id="afd3a-131">Som standard är varje reducer 256MB.</span><span class="sxs-lookup"><span data-stu-id="afd3a-131">By default, each reducer is 256MB.</span></span>  

## <a name="guidance"></a><span data-ttu-id="afd3a-132">Riktlinjer</span><span class="sxs-lookup"><span data-stu-id="afd3a-132">Guidance</span></span>

<span data-ttu-id="afd3a-133">**Ange hive.exec.reducer.bytes.per.reducer** – hello standardvärdet fungerar bra när hello data är okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="afd3a-133">**Set hive.exec.reducer.bytes.per.reducer** – hello default value works well when hello data is uncompressed.</span></span>  <span data-ttu-id="afd3a-134">För data som komprimerats, bör du minska hello reducer hello storlek.</span><span class="sxs-lookup"><span data-stu-id="afd3a-134">For data that is compressed, you should reduce hello size of hello reducer.</span></span>  

<span data-ttu-id="afd3a-135">**Ange hive.tez.container.size** – på varje nod minne har angetts av yarn.nodemanager.resource.memory mb och korrekt ska anges på HDI-klustret som standard.</span><span class="sxs-lookup"><span data-stu-id="afd3a-135">**Set hive.tez.container.size** – In each node, memory is specified by yarn.nodemanager.resource.memory-mb and should be correctly set on HDI cluster by default.</span></span>  <span data-ttu-id="afd3a-136">Mer information om hur hello lämpliga minne i YARN finns [efter](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span><span class="sxs-lookup"><span data-stu-id="afd3a-136">For additional information on setting hello appropriate memory in YARN, see this [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span></span>

<span data-ttu-id="afd3a-137">I/o-intensiva arbetsbelastningar kan utnyttja mer parallellitet genom att minska hello Tez behållarens storlek.</span><span class="sxs-lookup"><span data-stu-id="afd3a-137">I/O intensive workloads can benefit from more parallelism by decreasing hello Tez container size.</span></span> <span data-ttu-id="afd3a-138">Detta ger hello användaren fler behållare, vilket ökar samtidighet.</span><span class="sxs-lookup"><span data-stu-id="afd3a-138">This gives hello user more containers which increases concurrency.</span></span>  <span data-ttu-id="afd3a-139">Vissa Hive-frågor kräver dock en stor mängd minne (t.ex. MapJoin).</span><span class="sxs-lookup"><span data-stu-id="afd3a-139">However, some Hive queries require a significant amount of memory (e.g. MapJoin).</span></span>  <span data-ttu-id="afd3a-140">Om hello aktiviteten inte har tillräckligt med minne, får du en out-of minne undantag under körningen.</span><span class="sxs-lookup"><span data-stu-id="afd3a-140">If hello task does not have enough memory, you will get an out of memory exception during runtime.</span></span>  <span data-ttu-id="afd3a-141">Om du får slut på minne undantag, bör du öka hello minne.</span><span class="sxs-lookup"><span data-stu-id="afd3a-141">If you receive out of memory exceptions, then you should increase hello memory.</span></span>   

<span data-ttu-id="afd3a-142">hello antalet samtidiga aktiviteter som körs eller parallellitet kommer att begränsas av hello totalt YARN-minne.</span><span class="sxs-lookup"><span data-stu-id="afd3a-142">hello concurrent number of tasks running or parallelism will be bounded by hello total YARN memory.</span></span>  <span data-ttu-id="afd3a-143">hello antal YARN behållare styr hur många samtidiga uppgifter kan köras.</span><span class="sxs-lookup"><span data-stu-id="afd3a-143">hello number of YARN containers will dictate how many concurrent tasks can run.</span></span>  <span data-ttu-id="afd3a-144">Du kan gå tooAmbari toofind hello YARN minne per nod.</span><span class="sxs-lookup"><span data-stu-id="afd3a-144">toofind hello YARN memory per node, you can go tooAmbari.</span></span>  <span data-ttu-id="afd3a-145">Navigera tooYARN och visa hello konfigurationerna fliken.  Hej YARN minne visas i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="afd3a-145">Navigate tooYARN and view hello Configs tab.  hello YARN memory is displayed in this window.</span></span>  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
<span data-ttu-id="afd3a-146">hello viktiga tooimproving prestanda med hjälp av ADLS är tooincrease hello samtidighet så mycket som möjligt.</span><span class="sxs-lookup"><span data-stu-id="afd3a-146">hello key tooimproving performance using ADLS is tooincrease hello concurrency as much as possible.</span></span>  <span data-ttu-id="afd3a-147">Tez beräknar automatiskt hello antalet uppgifter som ska skapas, så du inte behöver tooset den.</span><span class="sxs-lookup"><span data-stu-id="afd3a-147">Tez automatically calculates hello number of tasks that should be created so you do not need tooset it.</span></span>   

## <a name="example-calculation"></a><span data-ttu-id="afd3a-148">Exempel beräkning</span><span class="sxs-lookup"><span data-stu-id="afd3a-148">Example Calculation</span></span>

<span data-ttu-id="afd3a-149">Anta att du har en 8 noder D14 kluster.</span><span class="sxs-lookup"><span data-stu-id="afd3a-149">Let’s say you have an 8 node D14 cluster.</span></span>  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a><span data-ttu-id="afd3a-150">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="afd3a-150">Limitations</span></span>
<span data-ttu-id="afd3a-151">**ADLS-begränsning**</span><span class="sxs-lookup"><span data-stu-id="afd3a-151">**ADLS throttling**</span></span> 

<span data-ttu-id="afd3a-152">UIf du träffar hello begränsar bandbredd under förutsättning av ADLS, börjar du toosee aktivitet fel.</span><span class="sxs-lookup"><span data-stu-id="afd3a-152">UIf you hit hello limits of bandwidth provided by ADLS, you would start toosee task failures.</span></span> <span data-ttu-id="afd3a-153">Det kan identifieras av sett bandbreddsbegränsning fel i loggarna för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="afd3a-153">This could be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="afd3a-154">Du kan minska hello parallellitet genom att öka storleken på Tez-behållare.</span><span class="sxs-lookup"><span data-stu-id="afd3a-154">You can decrease hello parallelism by increasing Tez container size.</span></span>  <span data-ttu-id="afd3a-155">Om du behöver mer samtidighet för jobbet, kontaktar du oss.</span><span class="sxs-lookup"><span data-stu-id="afd3a-155">If you need more concurrency for your job, please contact us.</span></span>   

<span data-ttu-id="afd3a-156">toocheck om du har komma begränsats måste tooenable hello felsökningsloggning på hello på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="afd3a-156">toocheck if you are getting throttled, you need tooenable hello debug logging on hello client side.</span></span> <span data-ttu-id="afd3a-157">Här visas hur du kan göra det:</span><span class="sxs-lookup"><span data-stu-id="afd3a-157">Here’s how you can do that:</span></span>

1. <span data-ttu-id="afd3a-158">Placera hello efter egenskap i hello log4j egenskaper i Hive-config. Detta kan göras från Ambari vyn: log4j.logger.com.microsoft.azure.datalake.store=DEBUG starta om alla hello noder/för hello config tootake effekt.</span><span class="sxs-lookup"><span data-stu-id="afd3a-158">Put hello following property in hello log4j properties in Hive config. This can be done from Ambari view: log4j.logger.com.microsoft.azure.datalake.store=DEBUG Restart all hello nodes/service for hello config tootake effect.</span></span>

2. <span data-ttu-id="afd3a-159">Om du har komma begränsats visas hello HTTP 429 felkod i loggfilen för hello hive.</span><span class="sxs-lookup"><span data-stu-id="afd3a-159">If you are getting throttled, you’ll see hello HTTP 429 error code in hello hive log file.</span></span> <span data-ttu-id="afd3a-160">hello hive-loggfilen finns i /tmp/&lt;användare&gt;/hive.log</span><span class="sxs-lookup"><span data-stu-id="afd3a-160">hello hive log file is in /tmp/&lt;user&gt;/hive.log</span></span>

## <a name="further-information-on-hive-tuning"></a><span data-ttu-id="afd3a-161">Mer information om Hive-inställning</span><span class="sxs-lookup"><span data-stu-id="afd3a-161">Further information on Hive tuning</span></span>

<span data-ttu-id="afd3a-162">Här följer några bloggar som hjälper dig att finjustera Hive-frågor:</span><span class="sxs-lookup"><span data-stu-id="afd3a-162">Here are a few blogs that will help tune your Hive queries:</span></span>
* [<span data-ttu-id="afd3a-163">Optimera Hive-frågor för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="afd3a-163">Optimize Hive queries for Hadoop in HDInsight</span></span>](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [<span data-ttu-id="afd3a-164">Felsökning av Hive frågeprestanda</span><span class="sxs-lookup"><span data-stu-id="afd3a-164">Troubleshooting Hive query performance</span></span>](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [<span data-ttu-id="afd3a-165">Ignite prata på optimera Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="afd3a-165">Ignite talk on optimize Hive on HDInsight</span></span>](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
