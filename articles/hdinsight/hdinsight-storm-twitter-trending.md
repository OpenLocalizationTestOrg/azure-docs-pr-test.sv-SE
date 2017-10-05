---
title: "Twitter trender avsnitt med Apache Storm på HDInsight | Microsoft Docs"
description: "Lär dig hur du använder Trident för att skapa en Apache Storm-topologi som bestämmer trender information på Twitter baserat på hash-taggar."
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
ms.openlocfilehash: d588221586f151319436525c5098b0bb2694e5f9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a><span data-ttu-id="34dc9-103">Fastställa Twitter trender avsnitt med Apache Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="34dc9-103">Determine Twitter trending topics with Apache Storm on HDInsight</span></span>

<span data-ttu-id="34dc9-104">Lär dig hur du använder Trident för att skapa en Storm-topologi som bestämmer trender avsnitt (hash-taggar) på Twitter.</span><span class="sxs-lookup"><span data-stu-id="34dc9-104">Learn how to use Trident to create a Storm topology that determines trending topics (hash tags) on Twitter.</span></span>

<span data-ttu-id="34dc9-105">Trident är en abstraktion på hög nivå som tillhandahåller verktyg, till exempel kopplingar, aggregeringar, gruppering, funktioner och filter.</span><span class="sxs-lookup"><span data-stu-id="34dc9-105">Trident is a high-level abstraction that provides tools such as joins, aggregations, grouping, functions, and filters.</span></span> <span data-ttu-id="34dc9-106">Trident lägger dessutom till primitiver för att göra tillståndskänslig, inkrementell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="34dc9-106">Additionally, Trident adds primitives for doing stateful, incremental processing.</span></span> <span data-ttu-id="34dc9-107">Exemplet i det här dokumentet är en Trident-topologi med en anpassad kanal och funktion.</span><span class="sxs-lookup"><span data-stu-id="34dc9-107">The example used in this document is a Trident topology with a custom spout and function.</span></span> <span data-ttu-id="34dc9-108">Flera inbyggda funktioner som tillhandahålls av Trident används också.</span><span class="sxs-lookup"><span data-stu-id="34dc9-108">It also uses several built-in functions provided by Trident.</span></span>

## <a name="requirements"></a><span data-ttu-id="34dc9-109">Krav</span><span class="sxs-lookup"><span data-stu-id="34dc9-109">Requirements</span></span>

* <span data-ttu-id="34dc9-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java och JDK 1.8</a></span><span class="sxs-lookup"><span data-stu-id="34dc9-110"><a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java and the JDK 1.8</a></span></span>

* <span data-ttu-id="34dc9-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven 3.</a></span><span class="sxs-lookup"><span data-stu-id="34dc9-111"><a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven</a></span></span>

* <span data-ttu-id="34dc9-112"><a href="http://git-scm.com/" target="_blank">Git</a></span><span class="sxs-lookup"><span data-stu-id="34dc9-112"><a href="http://git-scm.com/" target="_blank">Git</a></span></span>

* <span data-ttu-id="34dc9-113">Ett Twitter-utvecklarkonto</span><span class="sxs-lookup"><span data-stu-id="34dc9-113">A Twitter developer account</span></span>

## <a name="download-the-project"></a><span data-ttu-id="34dc9-114">Hämta projektet</span><span class="sxs-lookup"><span data-stu-id="34dc9-114">Download the project</span></span>

<span data-ttu-id="34dc9-115">Du kan använda följande kod för att klona projektet lokalt.</span><span class="sxs-lookup"><span data-stu-id="34dc9-115">Use the following code to clone the project locally.</span></span>

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-the-topology"></a><span data-ttu-id="34dc9-116">Förstå topologin</span><span class="sxs-lookup"><span data-stu-id="34dc9-116">Understanding the topology</span></span>

<span data-ttu-id="34dc9-117">Följande diagram visar av hur data flödar genom den här topologin:</span><span class="sxs-lookup"><span data-stu-id="34dc9-117">The following diagram shows of how data flows through this topology:</span></span>

![topologi](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> <span data-ttu-id="34dc9-119">Det här diagrammet är en förenklad vy av topologin.</span><span class="sxs-lookup"><span data-stu-id="34dc9-119">This diagram is a simplified view of the topology.</span></span> <span data-ttu-id="34dc9-120">Flera instanser av komponenterna som är fördelade på noderna i klustret.</span><span class="sxs-lookup"><span data-stu-id="34dc9-120">Multiple instances of the components are distributed across the nodes in the cluster.</span></span>


<span data-ttu-id="34dc9-121">Trident-kod som implementerar topologin är som följer:</span><span class="sxs-lookup"><span data-stu-id="34dc9-121">The Trident code that implements the topology is as follows:</span></span>

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

<span data-ttu-id="34dc9-122">Den här koden utförs följande åtgärder:</span><span class="sxs-lookup"><span data-stu-id="34dc9-122">This code performs the following actions:</span></span>

1. <span data-ttu-id="34dc9-123">Skapar en ström från kanal.</span><span class="sxs-lookup"><span data-stu-id="34dc9-123">Creates a stream from the spout.</span></span> <span data-ttu-id="34dc9-124">Kanal hämtar tweets från Twitter och filtrerar dem för specifika nyckelord (kärlek, musik och kaffe i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="34dc9-124">The spout retrieves tweets from Twitter, and filters them for specific keywords (love, music, and coffee in this example).</span></span>

2. <span data-ttu-id="34dc9-125">HashtagExtractor, en egen funktion används för att extrahera hash-taggar från varje tweet.</span><span class="sxs-lookup"><span data-stu-id="34dc9-125">HashtagExtractor, a custom function, is used to extract hash tags from each tweet.</span></span> <span data-ttu-id="34dc9-126">Hash-taggar orsakat till dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="34dc9-126">The hash tags are emitted to the stream.</span></span>

3. <span data-ttu-id="34dc9-127">Dataströmmen är grupperade efter hash-tagg och skickas till en aggregator.</span><span class="sxs-lookup"><span data-stu-id="34dc9-127">The stream is grouped by hash tag, and passed to an aggregator.</span></span> <span data-ttu-id="34dc9-128">Den här aggregator skapar en beräkning av hur många gånger varje hash-tagg har uppstått.</span><span class="sxs-lookup"><span data-stu-id="34dc9-128">This aggregator creates a count of how many times each hash tag has occurred.</span></span> <span data-ttu-id="34dc9-129">Informationen sparas i minnet.</span><span class="sxs-lookup"><span data-stu-id="34dc9-129">This data is persisted in memory.</span></span> <span data-ttu-id="34dc9-130">Slutligen genereras en ny ström som innehåller den hash-taggen och antal.</span><span class="sxs-lookup"><span data-stu-id="34dc9-130">Finally, a new stream is emitted that contains the hash tag and the count.</span></span>

4. <span data-ttu-id="34dc9-131">Den **FirstN** sammansättningen används för att returnera bara de översta 10 värdena, baserat på fältet Antal.</span><span class="sxs-lookup"><span data-stu-id="34dc9-131">The **FirstN** assembly is applied to return only the top 10 values, based on the count field.</span></span>

> [!NOTE]
> <span data-ttu-id="34dc9-132">Mer information om hur du arbetar med Trident finns i [Trident API översikt](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="34dc9-132">For more information on working with Trident, see the [Trident API overview](http://storm.apache.org/releases/current/Trident-API-Overview.html) document.</span></span>

### <a name="the-spout"></a><span data-ttu-id="34dc9-133">Kanal</span><span class="sxs-lookup"><span data-stu-id="34dc9-133">The spout</span></span>

<span data-ttu-id="34dc9-134">Kanal, **TwitterSpout**, använder [Twitter4j](http://twitter4j.org/en/) att hämta tweets från Twitter.</span><span class="sxs-lookup"><span data-stu-id="34dc9-134">The spout, **TwitterSpout**, uses [Twitter4j](http://twitter4j.org/en/) to retrieve tweets from Twitter.</span></span> <span data-ttu-id="34dc9-135">Skapa ett filter för ord __gillar__, **musik**, och **kaffe**.</span><span class="sxs-lookup"><span data-stu-id="34dc9-135">A filter is created for the words __love__, **music**, and **coffee**.</span></span> <span data-ttu-id="34dc9-136">Inkommande tweets (status) som matchar filtret som lagras i en länkad blockerande kö.</span><span class="sxs-lookup"><span data-stu-id="34dc9-136">Incoming tweets (status) that match the filter are stored in a linked blocking queue.</span></span> <span data-ttu-id="34dc9-137">Slutligen objekt hämtas från kön och orsakat till i topologin.</span><span class="sxs-lookup"><span data-stu-id="34dc9-137">Finally, items are pulled off the queue and emitted to the topology.</span></span>

### <a name="the-hashtagextractor"></a><span data-ttu-id="34dc9-138">HashtagExtractor</span><span class="sxs-lookup"><span data-stu-id="34dc9-138">The HashtagExtractor</span></span>

<span data-ttu-id="34dc9-139">Extrahera hash-taggar [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) används för att hämta alla hash-taggar som ingår i tweet.</span><span class="sxs-lookup"><span data-stu-id="34dc9-139">To extract hash tags, [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) is used to retrieve all hash tags that are contained in the tweet.</span></span> <span data-ttu-id="34dc9-140">Dessa släpps sedan till dataströmmen.</span><span class="sxs-lookup"><span data-stu-id="34dc9-140">These are then emitted to the stream.</span></span>

## <a name="configure-twitter"></a><span data-ttu-id="34dc9-141">Konfigurera Twitter</span><span class="sxs-lookup"><span data-stu-id="34dc9-141">Configure Twitter</span></span>

<span data-ttu-id="34dc9-142">Använd följande steg för att registrera en ny Twitter-program och få konsumenten och åtkomst-token information som behövs för att läsa från Twitter:</span><span class="sxs-lookup"><span data-stu-id="34dc9-142">Use the following steps to register a new Twitter application and obtain the consumer and access token information needed to read from Twitter:</span></span>

1. <span data-ttu-id="34dc9-143">Gå till [Twitter appar](https://apps.twitter.com) och klicka på den **Skapa ny app** knappen.</span><span class="sxs-lookup"><span data-stu-id="34dc9-143">Go to [Twitter Apps](https://apps.twitter.com) and click the **Create new app** button.</span></span> <span data-ttu-id="34dc9-144">När du fyller i formuläret lämnar den **motringning URL** fältet tomt.</span><span class="sxs-lookup"><span data-stu-id="34dc9-144">When filling in the form, leave the **Callback URL** field empty.</span></span>

2. <span data-ttu-id="34dc9-145">När appen har skapats klickar du på den **nycklar och åtkomst-token** fliken.</span><span class="sxs-lookup"><span data-stu-id="34dc9-145">When the app is created, click the **Keys and Access Tokens** tab.</span></span>

3. <span data-ttu-id="34dc9-146">Kopiera den **konsumenten nyckeln** och **konsumenthemlighet** information.</span><span class="sxs-lookup"><span data-stu-id="34dc9-146">Copy the **Consumer Key** and **Consumer Secret** information.</span></span>

4. <span data-ttu-id="34dc9-147">Längst ned på sidan Välj **skapa åtkomst-token** om det finns inga token.</span><span class="sxs-lookup"><span data-stu-id="34dc9-147">At the bottom of the page, select **Create my access token** if no tokens exist.</span></span> <span data-ttu-id="34dc9-148">När du har skapat token kopiera den **åtkomst-Token** och **åtkomst-Token hemlighet** information.</span><span class="sxs-lookup"><span data-stu-id="34dc9-148">When the tokens have been created, copy the **Access Token** and **Access Token Secret** information.</span></span>

5. <span data-ttu-id="34dc9-149">I den **TwitterSpoutTopology** projekt som du tidigare klonas, öppnar den **resources/twitter4j.properties** filen, lägga till den information du samlade in i föregående steg och spara sedan filen.</span><span class="sxs-lookup"><span data-stu-id="34dc9-149">In the **TwitterSpoutTopology** project you previously cloned, open the **resources/twitter4j.properties** file, add the information you gathered in the previous steps, and then save the file.</span></span>

## <a name="build-the-topology"></a><span data-ttu-id="34dc9-150">Skapa topologin</span><span class="sxs-lookup"><span data-stu-id="34dc9-150">Build the topology</span></span>

<span data-ttu-id="34dc9-151">Använd följande kod för att skapa projektet:</span><span class="sxs-lookup"><span data-stu-id="34dc9-151">Use the following code to build the project:</span></span>

        cd [directoryname]
        mvn compile

## <a name="test-the-topology"></a><span data-ttu-id="34dc9-152">Testa topologin</span><span class="sxs-lookup"><span data-stu-id="34dc9-152">Test the topology</span></span>

<span data-ttu-id="34dc9-153">Använd följande kommando för att testa topologin lokalt:</span><span class="sxs-lookup"><span data-stu-id="34dc9-153">Use the following command to test the topology locally:</span></span>

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

<span data-ttu-id="34dc9-154">Du bör se felsökningsinformation som innehåller hashen-taggar och räknar skickade av topologi när topologin har startat.</span><span class="sxs-lookup"><span data-stu-id="34dc9-154">After the topology starts, you should see debug information that contains the hash tags and counts emitted by the topology.</span></span> <span data-ttu-id="34dc9-155">Resultatet bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="34dc9-155">The output should appear similar to the following text:</span></span>

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a><span data-ttu-id="34dc9-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="34dc9-156">Next steps</span></span>

<span data-ttu-id="34dc9-157">Nu när du har testat lokalt topologin kan identifiera hur du distribuerar topologin: [distribuera och hantera Apache Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span><span class="sxs-lookup"><span data-stu-id="34dc9-157">Now that you have tested the topology locally, discover how to deploy the topology: [Deploy and manage Apache Storm topologies on HDInsight](hdinsight-storm-deploy-monitor-topology.md).</span></span>

<span data-ttu-id="34dc9-158">Du kanske även är intresserad av i följande Storm-avsnitt:</span><span class="sxs-lookup"><span data-stu-id="34dc9-158">You may also be interested in the following Storm topics:</span></span>

* [<span data-ttu-id="34dc9-159">Utveckla Java-topologier för Storm på HDInsight med Maven</span><span class="sxs-lookup"><span data-stu-id="34dc9-159">Develop Java topologies for Storm on HDInsight using Maven</span></span>](hdinsight-storm-develop-java-topology.md)
* [<span data-ttu-id="34dc9-160">Utveckla C#-topologier för Storm på HDInsight med Visual Studio</span><span class="sxs-lookup"><span data-stu-id="34dc9-160">Develop C# topologies for Storm on HDInsight using Visual Studio</span></span>](hdinsight-storm-develop-csharp-visual-studio-topology.md)

<span data-ttu-id="34dc9-161">Fler exempel i Storm för HDInsight:</span><span class="sxs-lookup"><span data-stu-id="34dc9-161">For more Storm examples for HDInsight:</span></span>

* [<span data-ttu-id="34dc9-162">Exempeltopologier för Storm på HDInsight</span><span class="sxs-lookup"><span data-stu-id="34dc9-162">Example topologies for Storm on HDInsight</span></span>](hdinsight-storm-example-topology.md)

