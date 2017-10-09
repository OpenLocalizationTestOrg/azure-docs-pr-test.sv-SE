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
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Fastställa Twitter trender avsnitt med Apache Storm på HDInsight

Lär dig hur toouse Trident toocreate en Storm-topologi som avgör trender avsnitt (hash-taggar) på Twitter.

Trident är en abstraktion på hög nivå som tillhandahåller verktyg, till exempel kopplingar, aggregeringar, gruppering, funktioner och filter. Trident lägger dessutom till primitiver för att göra tillståndskänslig, inkrementell bearbetning. hello-exempel som används i det här dokumentet är en Trident-topologi med en anpassad kanal och funktion. Flera inbyggda funktioner som tillhandahålls av Trident används också.

## <a name="requirements"></a>Krav

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java och hello JDK 1.8</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven 3.</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Ett Twitter-utvecklarkonto

## <a name="download-hello-project"></a>Ladda ned hello-projekt

Använd hello följande kod tooclone hello-projekt lokalt.

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-hello-topology"></a>Förstå hello-topologi

hello följande diagram visar av hur data flödar genom den här topologin:

![topologi](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> Det här diagrammet är en förenklad vy av hello-topologi. Flera instanser av hello komponenter är fördelade på hello noder i klustret hello.


hello Trident-kod som implementerar hello topologin är som följer:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Den här koden utför hello följande åtgärder:

1. Skapar en dataström från hello-kanal. hello kanal hämtar tweets från Twitter och filtrerar dem för specifika nyckelord (kärlek, musik och kaffe i det här exemplet).

2. HashtagExtractor, en egen funktion är används tooextract hash-taggarna från varje tweet. hello hash-taggar är angivna toohello dataströmmen.

3. hello dataströmmen är grupperade efter hash-tagg och skickades tooan aggregator. Den här aggregator skapar en beräkning av hur många gånger varje hash-tagg har uppstått. Informationen sparas i minnet. Slutligen genereras en ny ström som innehåller hello hash-tagg och hello antal.

4. Hej **FirstN** sammansättningen är tillämpade tooreturn endast hello 10 högsta värden, baserat på hello antalet fält.

> [!NOTE]
> Mer information om hur du arbetar med Trident finns hello [Trident API översikt](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentet.

### <a name="hello-spout"></a>hello kanal

hello kanal **TwitterSpout**, använder [Twitter4j](http://twitter4j.org/en/) tooretrieve tweets från Twitter. Skapa ett filter för hello ord __gillar__, **musik**, och **kaffe**. Inkommande tweets (status) som matchar filtret hello lagras i en länkad blockerande kö. Slutligen objekt hämtas av hello kön och orsakat toohello topologi.

### <a name="hello-hashtagextractor"></a>Hej HashtagExtractor

tooextract hash-taggar [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) är används tooretrieve alla hash-taggar som finns i hello tweet. De är då skickade toohello dataströmmen.

## <a name="configure-twitter"></a>Konfigurera Twitter

Använd hello följande steg tooregister ett nytt Twitter-program och hämta hello konsumenten och åtkomst-token information som behövs för tooread från Twitter:

1. Gå för[Twitter appar](https://apps.twitter.com) och klicka på hello **Skapa ny app** knappen. När du fyller i hello formulär kan lämna hello **motringning URL** fältet tomt.

2. När hello app har skapats klickar du på hello **nycklar och åtkomst-token** fliken.

3. Kopiera hello **konsumenten nyckeln** och **konsumenthemlighet** information.

4. Längst ned hello hello-sidan, Välj **skapa åtkomst-token** om det finns inga token. När hello token har skapats, kopiera hello **åtkomst-Token** och **åtkomst-Token hemlighet** information.

5. I hello **TwitterSpoutTopology** projekt som du tidigare klonade, öppna hello **resources/twitter4j.properties** filen, lägga till hello information du samlade in i föregående steg i hello och spara hello .

## <a name="build-hello-topology"></a>Skapa hello-topologi

Använd hello följande kod toobuild hello projektet:

        cd [directoryname]
        mvn compile

## <a name="test-hello-topology"></a>Testa hello-topologi

Använd hello följande kommando lokalt tootest hello-topologi:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Du bör se felsökningsinformation som innehåller hello hash-taggar och räknar skickade efter hello topologi när hello-topologi har startat. hello utdata ska se ut ungefär toohello följande text:

    DEBUG: [Quicktellervalentine, 7]
    DEBUG: [GRAMMYs, 7]
    DEBUG: [AskSam, 7]
    DEBUG: [poppunk, 1]
    DEBUG: [rock, 1]
    DEBUG: [punkrock, 1]
    DEBUG: [band, 1]
    DEBUG: [punk, 1]
    DEBUG: [indonesiapunkrock, 1]

## <a name="next-steps"></a>Nästa steg

Nu när du har testat lokalt hello-topologi kan du identifiera hur toodeploy hello topologi: [distribuera och hantera Apache Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Du kan även vara intresserad av hello följande Storm-avsnitt:

* [Utveckla Java-topologier för Storm på HDInsight med Maven](hdinsight-storm-develop-java-topology.md)
* [Utveckla C#-topologier för Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Fler exempel i Storm för HDInsight:

* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)

