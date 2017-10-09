---
title: "aaaTwitter trender avsnitt med Apache Storm på HDInsight | Microsoft Docs"
description: "Lär dig hur toouse Trident toocreate en Apache Storm-topologi som bestämmer trender information på Twitter baserat på hash-taggar."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 63b280ea-5c07-4dc8-a35f-dccf5a96ba93
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 04/14/2017
ms.author: larryfr
ms.openlocfilehash: 0281b495d10833c63868b36856c96369b139c553
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a><span data-ttu-id="02917-103">Fastställa Twitter trender avsnitt med Apache Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="02917-103">Determine Twitter trending topics with Apache Storm on HDInsight</span></span>

<span data-ttu-id="02917-104">Lär dig hur toouse Trident toocreate en Storm-topologi som avgör trender avsnitt (hash-taggar) på Twitter.</span><span class="sxs-lookup"><span data-stu-id="02917-104">Learn how toouse Trident toocreate a Storm topology that determines trending topics (hash tags) on Twitter.</span></span>

<span data-ttu-id="02917-105">Trident är en abstraktion på hög nivå som tillhandahåller verktyg, till exempel kopplingar, aggregeringar, gruppering, funktioner och filter.</span><span class="sxs-lookup"><span data-stu-id="02917-105">Trident is a high-level abstraction that provides tools such as joins, aggregations, grouping, functions, and filters.</span></span> <span data-ttu-id="02917-106">Trident lägger dessutom till primitiver för att göra tillståndskänslig, inkrementell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="02917-106">Additionally, Trident adds primitives for doing stateful, incremental processing.</span></span> <span data-ttu-id="02917-107">hello-exempel som används i det här dokumentet är en Trident-topologi med en anpassad kanal och funktion.</span><span class="sxs-lookup"><span data-stu-id="02917-107">hello example used in this document is a Trident topology with a custom spout and function.</span></span> <span data-ttu-id="02917-108">Flera inbyggda funktioner som tillhandahålls av Trident används också.</span><span class="sxs-lookup"><span data-stu-id="02917-108">It also uses several built-in functions provided by Trident.</span></span>

## <a name="requirements"></a><span data-ttu-id="02917-109">Krav</span><span class="sxs-lookup"><span data-stu-id="02917-109">Requirements</span></span>

* <span data-ttu-id="02917-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java och hello JDK 1.8</a></span><span class="sxs-lookup"><span data-stu-id="02917-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java and hello JDK 1.8</a></span></span>

* <span data-ttu-id="02917-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven 3.</a></span><span class="sxs-lookup"><span data-stu-id="02917-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a></span></span>

* <span data-ttu-id="02917-112"><a href="http://git-scm.com/" target="_blank">Git</a></span><span class="sxs-lookup"><span data-stu-id="02917-112"><a href="http://git-scm.com/" target="_blank">Git</a></span></span>

* <span data-ttu-id="02917-113">Ett Twitter-utvecklarkonto</span><span class="sxs-lookup"><span data-stu-id="02917-113">A Twitter developer account</span></span>

## <a name="download-hello-project"></a><span data-ttu-id="02917-114">Ladda ned hello-projekt</span><span class="sxs-lookup"><span data-stu-id="02917-114">Download hello project</span></span>

<span data-ttu-id="02917-115">Använd hello följande kod tooclone hello-projekt lokalt.</span><span class="sxs-lookup"><span data-stu-id="02917-115">Use hello following code tooclone hello project locally.</span></span>

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-hello-topology"></a><span data-ttu-id="02917-116">Förstå hello-topologi</span><span class="sxs-lookup"><span data-stu-id="02917-116">Understanding hello topology</span></span>

<span data-ttu-id="02917-117">hello följande diagram visar av hur data flödar genom den här topologin:</span><span class="sxs-lookup"><span data-stu-id="02917-117">hello following diagram shows of how data flows through this topology:</span></span>

![topologi](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> <span data-ttu-id="02917-119">Det här diagrammet är en förenklad vy av hello-topologi.</span><span class="sxs-lookup"><span data-stu-id="02917-119">This diagram is a simplified view of hello topology.</span></span> <span data-ttu-id="02917-120">Flera instanser av hello komponenter är fördelade på hello noder i klustret hello.</span><span class="sxs-lookup"><span data-stu-id="02917-120">Multiple instances of hello components are distributed across hello nodes in hello cluster.</span></span>


<span data-ttu-id="02917-121">hello Trident-kod som implementerar hello topologin är som följer:</span><span class="sxs-lookup"><span data-stu-id="02917-121">hello Trident code that implements hello topology is as follows:</span></span>

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

<span data-ttu-id="02917-122">Den här koden utför hello följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="02917-122">This code performs hello following actions:</span></span>

1. <span data-ttu-id="02917-123">Skapar en dataström från hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="02917-123">Creates a stream from hello spout.</span></span> <span data-ttu-id="02917-124">hello kanal hämtar tweets från Twitter och filtrerar dem för specifika nyckelord (kärlek, musik och kaffe i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="02917-124">hello spout retrieves tweets from Twitter, and filters them for specific keywords (love, music, and coffee in this example).</span></span>

2. <span data-ttu-id="02917-125">HashtagExtractor, en egen funktion är används tooextract hash-taggarna från varje tweet.</span><span class="sxs-lookup"><span data-stu-id="02917-125">HashtagExtractor, a custom function, is used tooextract hash tags from each tweet.</span></span> <span data-ttu-id="02917-126">hello hash-taggar är angivna toohello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="02917-126">hello hash tags are emitted toohello stream.</span></span>

3. <span data-ttu-id="02917-127">hello dataströmmen är grupperade efter hash-tagg och skickades tooan aggregator.</span><span class="sxs-lookup"><span data-stu-id="02917-127">hello stream is grouped by hash tag, and passed tooan aggregator.</span></span> <span data-ttu-id="02917-128">Den här aggregator skapar en beräkning av hur många gånger varje hash-tagg har uppstått.</span><span class="sxs-lookup"><span data-stu-id="02917-128">This aggregator creates a count of how many times each hash tag has occurred.</span></span> <span data-ttu-id="02917-129">Informationen sparas i minnet.</span><span class="sxs-lookup"><span data-stu-id="02917-129">This data is persisted in memory.</span></span> <span data-ttu-id="02917-130">Slutligen genereras en ny ström som innehåller hello hash-tagg och hello antal.</span><span class="sxs-lookup"><span data-stu-id="02917-130">Finally, a new stream is emitted that contains hello hash tag and hello count.</span></span>

4. <span data-ttu-id="02917-131">Hej **FirstN** sammansättningen är tillämpade tooreturn endast hello 10 högsta värden, baserat på hello antalet fält.</span><span class="sxs-lookup"><span data-stu-id="02917-131">hello **FirstN** assembly is applied tooreturn only hello top 10 values, based on hello count field.</span></span>

> [!NOTE]
> <span data-ttu-id="02917-132">Mer information om hur du arbetar med Trident finns hello [Trident API översikt](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="02917-132">For more information on working with Trident, see hello [Trident API overview](http://storm.apache.org/releases/current/Trident-API-Overview.html) document.</span></span>

### <a name="hello-spout"></a><span data-ttu-id="02917-133">hello kanal</span><span class="sxs-lookup"><span data-stu-id="02917-133">hello spout</span></span>

<span data-ttu-id="02917-134">hello kanal **TwitterSpout**, använder [Twitter4j](http://twitter4j.org/en/) tooretrieve tweets från Twitter.</span><span class="sxs-lookup"><span data-stu-id="02917-134">hello spout, **TwitterSpout**, uses [Twitter4j](http://twitter4j.org/en/) tooretrieve tweets from Twitter.</span></span> <span data-ttu-id="02917-135">Skapa ett filter för hello ord __gillar__, **musik**, och **kaffe**.</span><span class="sxs-lookup"><span data-stu-id="02917-135">A filter is created for hello words __love__, **music**, and **coffee**.</span></span> <span data-ttu-id="02917-136">Inkommande tweets (status) som matchar filtret hello lagras i en länkad blockerande kö.</span><span class="sxs-lookup"><span data-stu-id="02917-136">Incoming tweets (status) that match hello filter are stored in a linked blocking queue.</span></span> <span data-ttu-id="02917-137">Slutligen objekt hämtas av hello kön och orsakat toohello topologi.</span><span class="sxs-lookup"><span data-stu-id="02917-137">Finally, items are pulled off hello queue and emitted toohello topology.</span></span>

### <a name="hello-hashtagextractor"></a><span data-ttu-id="02917-138">Hej HashtagExtractor</span><span class="sxs-lookup"><span data-stu-id="02917-138">hello HashtagExtractor</span></span>

<span data-ttu-id="02917-139">tooextract hash-taggar [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) är används tooretrieve alla hash-taggar som finns i hello tweet.</span><span class="sxs-lookup"><span data-stu-id="02917-139">tooextract hash tags, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) is used tooretrieve all hash tags that are contained in hello tweet.</span></span> <span data-ttu-id="02917-140">De är då skickade toohello dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="02917-140">These are then emitted toohello stream.</span></span>

## <a name="configure-twitter"></a><span data-ttu-id="02917-141">Konfigurera Twitter</span><span class="sxs-lookup"><span data-stu-id="02917-141">Configure Twitter</span></span>

<span data-ttu-id="02917-142">Använd hello följande steg tooregister ett nytt Twitter-program och hämta hello konsumenten och åtkomst-token information som behövs för tooread från Twitter:</span><span class="sxs-lookup"><span data-stu-id="02917-142">Use hello following steps tooregister a new Twitter application and obtain hello consumer and access token information needed tooread from Twitter:</span></span>

1. <span data-ttu-id="02917-143">Gå för[Twitter appar](https://apps.twitter.com) och klicka på hello **Skapa ny app** knappen.</span><span class="sxs-lookup"><span data-stu-id="02917-143">Go too[Twitter Apps](https://apps.twitter.com) and click hello **Create new app** button.</span></span> <span data-ttu-id="02917-144">När du fyller i hello formulär kan lämna hello **motringning URL** fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="02917-144">When filling in hello form, leave hello **Callback URL** field empty.</span></span>

2. <span data-ttu-id="02917-145">När hello app har skapats klickar du på hello **nycklar och åtkomst-token** fliken.</span><span class="sxs-lookup"><span data-stu-id="02917-145">When hello app is created, click hello **Keys and Access Tokens** tab.</span></span>

3. <span data-ttu-id="02917-146">Kopiera hello **konsumenten nyckeln** och **konsumenthemlighet** information.</span><span class="sxs-lookup"><span data-stu-id="02917-146">Copy hello **Consumer Key** and **Consumer Secret** information.</span></span>

4. <span data-ttu-id="02917-147">Längst ned hello hello-sidan, Välj **skapa åtkomst-token** om det finns inga token.</span><span class="sxs-lookup"><span data-stu-id="02917-147">At hello bottom of hello page, select **Create my access token** if no tokens exist.</span></span> <span data-ttu-id="02917-148">När hello token har skapats, kopiera hello **åtkomst-Token** och **åtkomst-Token hemlighet** information.</span><span class="sxs-lookup"><span data-stu-id="02917-148">When hello tokens have been created, copy hello **Access Token** and **Access Token Secret** information.</span></span>

5. <span data-ttu-id="02917-149">I hello **TwitterSpoutTopology** projekt som du tidigare klonade, öppna hello **resources/twitter4j.properties** filen, lägga till hello information du samlade in i föregående steg i hello och spara hello .</span><span class="sxs-lookup"><span data-stu-id="02917-149">In hello **TwitterSpoutTopology** project you previously cloned, open hello **resources/twitter4j.properties** file, add hello information you gathered in hello previous steps, and then save hello file.</span></span>

## <a name="build-hello-topology"></a><span data-ttu-id="02917-150">Skapa hello-topologi</span><span class="sxs-lookup"><span data-stu-id="02917-150">Build hello topology</span></span>

<span data-ttu-id="02917-151">Använd hello följande kod toobuild hello projektet:</span><span class="sxs-lookup"><span data-stu-id="02917-151">Use hello following code toobuild hello project:</span></span>

        cd [directoryname]
        mvn compile

## <a name="test-hello-topology"></a><span data-ttu-id="02917-152">Testa hello-topologi</span><span class="sxs-lookup"><span data-stu-id="02917-152">Test hello topology</span></span>

<span data-ttu-id="02917-153">Använd hello följande kommando lokalt tootest hello-topologi:</span><span class="sxs-lookup"><span data-stu-id="02917-153">Use hello following command tootest hello topology locally:</span></span>

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

<span data-ttu-id="02917-154">Du bör se felsökningsinformation som innehåller hello hash-taggar och räknar skickade efter hello topologi när hello-topologi har startat.</span><span class="sxs-lookup"><span data-stu-id="02917-154">After hello topology starts, you should see debug information that contains hello hash tags and counts emitted by hello topology.</span></span> <span data-ttu-id="02917-155">hello utdata ska se ut ungefär toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="02917-155">hello output should appear similar toohello following text:</span></span>

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a><span data-ttu-id="02917-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="02917-156">Next steps</span></span>

<span data-ttu-id="02917-157">Nu när du har testat lokalt hello-topologi kan du identifiera hur toodeploy hello topologi: [distribuera och hantera Apache Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span><span class="sxs-lookup"><span data-stu-id="02917-157">Now that you have tested hello topology locally, discover how toodeploy hello topology: [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span></span>

<span data-ttu-id="02917-158">Du kan även vara intresserad av hello följande Storm-avsnitt:</span><span class="sxs-lookup"><span data-stu-id="02917-158">You may also be interested in hello following Storm topics:</span></span>

* [<span data-ttu-id="02917-159">Utveckla Java-topologier för Storm på HDInsight med Maven</span><span class="sxs-lookup"><span data-stu-id="02917-159">Develop Java topologies for Storm on HDInsight using Maven</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="02917-160">Utveckla C#-topologier för Storm på HDInsight med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="02917-160">Develop C# topologies for Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="02917-161">Fler exempel i Storm för HDInsight:</span><span class="sxs-lookup"><span data-stu-id="02917-161">For more Storm examples for HDInsight:</span></span>

* [<span data-ttu-id="02917-162">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="02917-162">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

