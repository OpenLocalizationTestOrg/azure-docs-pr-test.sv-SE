---
title: Azure Data Lake Store Hive prestandajustering riktlinjer | Microsoft Docs
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
ms.openlocfilehash: e10bf8f7cbae2b81d22823ff74fe652c6bcb2da3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="performance-tuning-guidance-for-hive-on-hdinsight-and-azure-data-lake-store"></a><span data-ttu-id="9b073-103">Prestandajustering för Hive i HDInsight och Azure Data Lake Store</span><span class="sxs-lookup"><span data-stu-id="9b073-103">Performance tuning guidance for Hive on HDInsight and Azure Data Lake Store</span></span>

<span data-ttu-id="9b073-104">Standardinställningarna har ställts in för att tillhandahålla goda prestanda i många olika användningsfall.</span><span class="sxs-lookup"><span data-stu-id="9b073-104">The default settings have been set to provide good performance across many different use cases.</span></span>  <span data-ttu-id="9b073-105">För i/o-intensiva frågor, kan du justera Hive för att få bättre prestanda med ADLS.</span><span class="sxs-lookup"><span data-stu-id="9b073-105">For I/O intensive queries, Hive can be tuned to get better performance with ADLS.</span></span>  

## <a name="prerequisites"></a><span data-ttu-id="9b073-106">Krav</span><span class="sxs-lookup"><span data-stu-id="9b073-106">Prerequisites</span></span>

* <span data-ttu-id="9b073-107">**En Azure-prenumeration**.</span><span class="sxs-lookup"><span data-stu-id="9b073-107">**An Azure subscription**.</span></span> <span data-ttu-id="9b073-108">Se [Hämta en kostnadsfri utvärderingsversion av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="9b073-108">See [Get Azure free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="9b073-109">**Ett Azure Data Lake Store-konto**.</span><span class="sxs-lookup"><span data-stu-id="9b073-109">**An Azure Data Lake Store account**.</span></span> <span data-ttu-id="9b073-110">Anvisningar om hur du skapar en finns [Kom igång med Azure Data Lake Store](data-lake-store-get-started-portal.md)</span><span class="sxs-lookup"><span data-stu-id="9b073-110">For instructions on how to create one, see [Get started with Azure Data Lake Store](data-lake-store-get-started-portal.md)</span></span>
* <span data-ttu-id="9b073-111">**Azure HDInsight-kluster** med åtkomst till ett Data Lake Store-konto.</span><span class="sxs-lookup"><span data-stu-id="9b073-111">**Azure HDInsight cluster** with access to a Data Lake Store account.</span></span> <span data-ttu-id="9b073-112">Se [skapar ett HDInsight-kluster med Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span><span class="sxs-lookup"><span data-stu-id="9b073-112">See [Create an HDInsight cluster with Data Lake Store](data-lake-store-hdinsight-hadoop-use-portal.md).</span></span> <span data-ttu-id="9b073-113">Kontrollera att du kan aktivera Fjärrskrivbord för klustret.</span><span class="sxs-lookup"><span data-stu-id="9b073-113">Make sure you enable Remote Desktop for the cluster.</span></span>
* <span data-ttu-id="9b073-114">**Kör Hive i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="9b073-114">**Running Hive on HDInsight**.</span></span>  <span data-ttu-id="9b073-115">Läs om hur du kör Hive-jobb i HDInsight i [använda Hive i HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span><span class="sxs-lookup"><span data-stu-id="9b073-115">To learn about running Hive jobs on HDInsight, see [Use Hive on HDInsight] (https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-use-hive)</span></span>
* <span data-ttu-id="9b073-116">**Prestandajustering riktlinjer för ADLS**.</span><span class="sxs-lookup"><span data-stu-id="9b073-116">**Performance tuning guidelines on ADLS**.</span></span>  <span data-ttu-id="9b073-117">Allmänna prestanda begrepp finns [Data Lake Store justera Prestandaråd](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span><span class="sxs-lookup"><span data-stu-id="9b073-117">For general performance concepts, see [Data Lake Store Performance Tuning Guidance](https://docs.microsoft.com/en-us/azure/data-lake-store/data-lake-store-performance-tuning-guidance)</span></span>

## <a name="parameters"></a><span data-ttu-id="9b073-118">Parametrar</span><span class="sxs-lookup"><span data-stu-id="9b073-118">Parameters</span></span>

<span data-ttu-id="9b073-119">Här är de viktigaste inställningarna att justera för bättre prestanda för ADLS:</span><span class="sxs-lookup"><span data-stu-id="9b073-119">Here are the most important settings to tune for improved ADLS performance:</span></span>

* <span data-ttu-id="9b073-120">**hive.tez.container.size** – hur mycket minne som används av varje uppgifter</span><span class="sxs-lookup"><span data-stu-id="9b073-120">**hive.tez.container.size** – the amount of memory used by each tasks</span></span>

* <span data-ttu-id="9b073-121">**tez.Grouping.min storlek** – minsta storlek varje mapper</span><span class="sxs-lookup"><span data-stu-id="9b073-121">**tez.grouping.min-size** – minimum size of each mapper</span></span>

* <span data-ttu-id="9b073-122">**tez.Grouping.Max storlek** – maximal storlek för varje mapper</span><span class="sxs-lookup"><span data-stu-id="9b073-122">**tez.grouping.max-size** – maximum size of each mapper</span></span>

* <span data-ttu-id="9b073-123">**hive.Exec.Reducer.bytes.per.Reducer** – storlek varje reducer</span><span class="sxs-lookup"><span data-stu-id="9b073-123">**hive.exec.reducer.bytes.per.reducer** – size of each reducer</span></span>

<span data-ttu-id="9b073-124">**hive.tez.container.size** -behållaren storlek avgör hur mycket minne som är tillgänglig för varje aktivitet.</span><span class="sxs-lookup"><span data-stu-id="9b073-124">**hive.tez.container.size** - The container size determines how much memory is available for each task.</span></span>  <span data-ttu-id="9b073-125">Det här är den huvudsakliga indatan för att styra samtidighet i Hive.</span><span class="sxs-lookup"><span data-stu-id="9b073-125">This is the main input for controlling the concurrency in Hive.</span></span>  

<span data-ttu-id="9b073-126">**tez.Grouping.min storlek** – den här parametern kan du ange den minimala storleken för varje mappning.</span><span class="sxs-lookup"><span data-stu-id="9b073-126">**tez.grouping.min-size** – This parameter allows you to set the minimum size of each mapper.</span></span>  <span data-ttu-id="9b073-127">Om antalet mappers som Tez väljer är mindre än värdet för den här parametern använder Tez värdet som anges här.</span><span class="sxs-lookup"><span data-stu-id="9b073-127">If the number of mappers that Tez chooses is smaller than the value of this parameter, then Tez will use the value set here.</span></span>  

<span data-ttu-id="9b073-128">**tez.Grouping.Max storlek** – parametern kan du ange den maximala storleken för varje mappning.</span><span class="sxs-lookup"><span data-stu-id="9b073-128">**tez.grouping.max-size** – The parameter allows you to set the maximum size of each mapper.</span></span>  <span data-ttu-id="9b073-129">Om antalet mappers som Tez väljer är större än värdet för den här parametern använder Tez värdet som anges här.</span><span class="sxs-lookup"><span data-stu-id="9b073-129">If the number of mappers that Tez chooses is larger than the value of this parameter, then Tez will use the value set here.</span></span>  

<span data-ttu-id="9b073-130">**hive.Exec.Reducer.bytes.per.Reducer** – den här parametern anger storleken på varje reducer.</span><span class="sxs-lookup"><span data-stu-id="9b073-130">**hive.exec.reducer.bytes.per.reducer** – This parameter sets the size of each reducer.</span></span>  <span data-ttu-id="9b073-131">Som standard är varje reducer 256MB.</span><span class="sxs-lookup"><span data-stu-id="9b073-131">By default, each reducer is 256MB.</span></span>  

## <a name="guidance"></a><span data-ttu-id="9b073-132">Riktlinjer</span><span class="sxs-lookup"><span data-stu-id="9b073-132">Guidance</span></span>

<span data-ttu-id="9b073-133">**Ange hive.exec.reducer.bytes.per.reducer** – standardvärdet fungerar bra när data är okomprimerade.</span><span class="sxs-lookup"><span data-stu-id="9b073-133">**Set hive.exec.reducer.bytes.per.reducer** – The default value works well when the data is uncompressed.</span></span>  <span data-ttu-id="9b073-134">För data som komprimerats, bör du minska storleken på reducer.</span><span class="sxs-lookup"><span data-stu-id="9b073-134">For data that is compressed, you should reduce the size of the reducer.</span></span>  

<span data-ttu-id="9b073-135">**Ange hive.tez.container.size** – på varje nod minne har angetts av yarn.nodemanager.resource.memory mb och korrekt ska anges på HDI-klustret som standard.</span><span class="sxs-lookup"><span data-stu-id="9b073-135">**Set hive.tez.container.size** – In each node, memory is specified by yarn.nodemanager.resource.memory-mb and should be correctly set on HDI cluster by default.</span></span>  <span data-ttu-id="9b073-136">Mer information om hur lämpligt minne i YARN finns [efter](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span><span class="sxs-lookup"><span data-stu-id="9b073-136">For additional information on setting the appropriate memory in YARN, see this [post](https://docs.microsoft.com/en-us/azure/hdinsight/hdinsight-hadoop-hive-out-of-memory-error-oom).</span></span>

<span data-ttu-id="9b073-137">I/o-intensiva arbetsbelastningar kan utnyttja mer parallellitet genom att minska storleken för Tez-behållare.</span><span class="sxs-lookup"><span data-stu-id="9b073-137">I/O intensive workloads can benefit from more parallelism by decreasing the Tez container size.</span></span> <span data-ttu-id="9b073-138">Detta ger användaren fler behållare, vilket ökar samtidighet.</span><span class="sxs-lookup"><span data-stu-id="9b073-138">This gives the user more containers which increases concurrency.</span></span>  <span data-ttu-id="9b073-139">Vissa Hive-frågor kräver dock en stor mängd minne (t.ex. MapJoin).</span><span class="sxs-lookup"><span data-stu-id="9b073-139">However, some Hive queries require a significant amount of memory (e.g. MapJoin).</span></span>  <span data-ttu-id="9b073-140">Om aktiviteten inte har tillräckligt med minne, får du en out-of minne undantag under körningen.</span><span class="sxs-lookup"><span data-stu-id="9b073-140">If the task does not have enough memory, you will get an out of memory exception during runtime.</span></span>  <span data-ttu-id="9b073-141">Om du får slut på minne undantag, bör du öka minnet.</span><span class="sxs-lookup"><span data-stu-id="9b073-141">If you receive out of memory exceptions, then you should increase the memory.</span></span>   

<span data-ttu-id="9b073-142">Samtidiga antalet aktiviteter som körs eller parallellitet kommer att begränsas av det totala minnet YARN.</span><span class="sxs-lookup"><span data-stu-id="9b073-142">The concurrent number of tasks running or parallelism will be bounded by the total YARN memory.</span></span>  <span data-ttu-id="9b073-143">Antal YARN behållare styr hur många samtidiga uppgifter kan köras.</span><span class="sxs-lookup"><span data-stu-id="9b073-143">The number of YARN containers will dictate how many concurrent tasks can run.</span></span>  <span data-ttu-id="9b073-144">Du kan gå till Ambari för att hitta YARN-minne per nod.</span><span class="sxs-lookup"><span data-stu-id="9b073-144">To find the YARN memory per node, you can go to Ambari.</span></span>  <span data-ttu-id="9b073-145">Navigera till YARN och visar fliken konfigurationerna.</span><span class="sxs-lookup"><span data-stu-id="9b073-145">Navigate to YARN and view the Configs tab.</span></span>  <span data-ttu-id="9b073-146">YARN-minne visas i det här fönstret.</span><span class="sxs-lookup"><span data-stu-id="9b073-146">The YARN memory is displayed in this window.</span></span>  

        Total YARN memory = nodes * YARN memory per node
        # of YARN containers = Total YARN memory / Tez container size
<span data-ttu-id="9b073-147">Nyckeln till att förbättra prestanda med hjälp av ADLS är att öka parallellkörningen så mycket som möjligt.</span><span class="sxs-lookup"><span data-stu-id="9b073-147">The key to improving performance using ADLS is to increase the concurrency as much as possible.</span></span>  <span data-ttu-id="9b073-148">Tez beräknar automatiskt antalet aktiviteter som bör skapas så att du inte behöver ange den.</span><span class="sxs-lookup"><span data-stu-id="9b073-148">Tez automatically calculates the number of tasks that should be created so you do not need to set it.</span></span>   

## <a name="example-calculation"></a><span data-ttu-id="9b073-149">Exempel beräkning</span><span class="sxs-lookup"><span data-stu-id="9b073-149">Example Calculation</span></span>

<span data-ttu-id="9b073-150">Anta att du har en 8 noder D14 kluster.</span><span class="sxs-lookup"><span data-stu-id="9b073-150">Let’s say you have an 8 node D14 cluster.</span></span>  

    Total YARN memory = nodes * YARN memory per node
    Total YARN memory = 8 nodes * 96GB = 768GB
    # of YARN containers = 768GB / 3072MB = 256

## <a name="limitations"></a><span data-ttu-id="9b073-151">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="9b073-151">Limitations</span></span>
<span data-ttu-id="9b073-152">**ADLS-begränsning**</span><span class="sxs-lookup"><span data-stu-id="9b073-152">**ADLS throttling**</span></span> 

<span data-ttu-id="9b073-153">UIf nådde gränserna för bandbredd som tillhandahålls av ADLS, börjar du vill se misslyckade aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="9b073-153">UIf you hit the limits of bandwidth provided by ADLS, you would start to see task failures.</span></span> <span data-ttu-id="9b073-154">Det kan identifieras av sett bandbreddsbegränsning fel i loggarna för aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="9b073-154">This could be identified by observing throttling errors in task logs.</span></span>  <span data-ttu-id="9b073-155">Du kan minska parallellitet genom att öka storleken på Tez-behållare.</span><span class="sxs-lookup"><span data-stu-id="9b073-155">You can decrease the parallelism by increasing Tez container size.</span></span>  <span data-ttu-id="9b073-156">Om du behöver mer samtidighet för jobbet, kontaktar du oss.</span><span class="sxs-lookup"><span data-stu-id="9b073-156">If you need more concurrency for your job, please contact us.</span></span>   

<span data-ttu-id="9b073-157">Om du vill kontrollera om du har komma begränsats, måste du Aktivera felsökningsloggning på klientsidan.</span><span class="sxs-lookup"><span data-stu-id="9b073-157">To check if you are getting throttled, you need to enable the debug logging on the client side.</span></span> <span data-ttu-id="9b073-158">Här visas hur du kan göra det:</span><span class="sxs-lookup"><span data-stu-id="9b073-158">Here’s how you can do that:</span></span>

1. <span data-ttu-id="9b073-159">Placera följande egenskap i log4j-egenskaper i Hive-config.</span><span class="sxs-lookup"><span data-stu-id="9b073-159">Put the following property in the log4j properties in Hive config.</span></span> <span data-ttu-id="9b073-160">Detta kan göras från Ambari vyn: log4j.logger.com.microsoft.azure.datalake.store=DEBUG starta om alla noder/för konfigurationen ska börja gälla.</span><span class="sxs-lookup"><span data-stu-id="9b073-160">This can be done from Ambari view: log4j.logger.com.microsoft.azure.datalake.store=DEBUG Restart all the nodes/service for the config to take effect.</span></span>

2. <span data-ttu-id="9b073-161">Om du har komma begränsats visas felkoden HTTP 429 i loggfilen hive.</span><span class="sxs-lookup"><span data-stu-id="9b073-161">If you are getting throttled, you’ll see the HTTP 429 error code in the hive log file.</span></span> <span data-ttu-id="9b073-162">Hive loggfilen finns i /tmp/&lt;användare&gt;/hive.log</span><span class="sxs-lookup"><span data-stu-id="9b073-162">The hive log file is in /tmp/&lt;user&gt;/hive.log</span></span>

## <a name="further-information-on-hive-tuning"></a><span data-ttu-id="9b073-163">Mer information om Hive-inställning</span><span class="sxs-lookup"><span data-stu-id="9b073-163">Further information on Hive tuning</span></span>

<span data-ttu-id="9b073-164">Här följer några bloggar som hjälper dig att finjustera Hive-frågor:</span><span class="sxs-lookup"><span data-stu-id="9b073-164">Here are a few blogs that will help tune your Hive queries:</span></span>
* [<span data-ttu-id="9b073-165">Optimera Hive-frågor för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9b073-165">Optimize Hive queries for Hadoop in HDInsight</span></span>](https://azure.microsoft.com/en-us/documentation/articles/hdinsight-hadoop-optimize-hive-query/)
* [<span data-ttu-id="9b073-166">Felsökning av Hive frågeprestanda</span><span class="sxs-lookup"><span data-stu-id="9b073-166">Troubleshooting Hive query performance</span></span>](https://blogs.msdn.microsoft.com/bigdatasupport/2015/08/13/troubleshooting-hive-query-performance-in-hdinsight-hadoop-cluster/)
* [<span data-ttu-id="9b073-167">Ignite prata på optimera Hive i HDInsight</span><span class="sxs-lookup"><span data-stu-id="9b073-167">Ignite talk on optimize Hive on HDInsight</span></span>](https://channel9.msdn.com/events/Machine-Learning-and-Data-Sciences-Conference/Data-Science-Summit-2016/MSDSS25)
