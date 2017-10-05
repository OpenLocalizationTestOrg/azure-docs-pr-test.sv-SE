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
# <a name="determine-twitter-trending-topics-with-apache-storm-on-hdinsight"></a>Fastställa Twitter trender avsnitt med Apache Storm på HDInsight

Lär dig hur du använder Trident för att skapa en Storm-topologi som bestämmer trender avsnitt (hash-taggar) på Twitter.

Trident är en abstraktion på hög nivå som tillhandahåller verktyg, till exempel kopplingar, aggregeringar, gruppering, funktioner och filter. Trident lägger dessutom till primitiver för att göra tillståndskänslig, inkrementell bearbetning. Exemplet i det här dokumentet är en Trident-topologi med en anpassad kanal och funktion. Flera inbyggda funktioner som tillhandahålls av Trident används också.

## <a name="requirements"></a>Krav

* <a href="http://www.oracle.com/technetwork/java/javase/downloads/index.html" target="_blank">Java och JDK 1.8</a>

* <a href="http://maven.apache.org/what-is-maven.html" target="_blank">Maven 3.</a>

* <a href="http://git-scm.com/" target="_blank">Git</a>

* Ett Twitter-utvecklarkonto

## <a name="download-the-project"></a>Hämta projektet

Du kan använda följande kod för att klona projektet lokalt.

    git clone https://github.com/Blackmist/TwitterTrending

## <a name="understanding-the-topology"></a>Förstå topologin

Följande diagram visar av hur data flödar genom den här topologin:

![topologi](./media/hdinsight-storm-twitter-trending/trident.png)

> [!NOTE]
> Det här diagrammet är en förenklad vy av topologin. Flera instanser av komponenterna som är fördelade på noderna i klustret.


Trident-kod som implementerar topologin är som följer:

    topology.newStream("spout", spout)
        .each(new Fields("tweet"), new HashtagExtractor(), new Fields("hashtag"))
        .groupBy(new Fields("hashtag"))
        .persistentAggregate(new MemoryMapState.Factory(), new Count(), new Fields("count"))
        .newValuesStream()
        .applyAssembly(new FirstN(10, "count"))
        .each(new Fields("hashtag", "count"), new Debug());

Den här koden utförs följande åtgärder:

1. Skapar en ström från kanal. Kanal hämtar tweets från Twitter och filtrerar dem för specifika nyckelord (kärlek, musik och kaffe i det här exemplet).

2. HashtagExtractor, en egen funktion används för att extrahera hash-taggar från varje tweet. Hash-taggar orsakat till dataströmmen.

3. Dataströmmen är grupperade efter hash-tagg och skickas till en aggregator. Den här aggregator skapar en beräkning av hur många gånger varje hash-tagg har uppstått. Informationen sparas i minnet. Slutligen genereras en ny ström som innehåller den hash-taggen och antal.

4. Den **FirstN** sammansättningen används för att returnera bara de översta 10 värdena, baserat på fältet Antal.

> [!NOTE]
> Mer information om hur du arbetar med Trident finns i [Trident API översikt](http://storm.apache.org/releases/current/Trident-API-Overview.html) dokumentet.

### <a name="the-spout"></a>Kanal

Kanal, **TwitterSpout**, använder [Twitter4j](http://twitter4j.org/en/) att hämta tweets från Twitter. Skapa ett filter för ord __gillar__, **musik**, och **kaffe**. Inkommande tweets (status) som matchar filtret som lagras i en länkad blockerande kö. Slutligen objekt hämtas från kön och orsakat till i topologin.

### <a name="the-hashtagextractor"></a>HashtagExtractor

Extrahera hash-taggar [getHashtagEntities](http://twitter4j.org/javadoc/twitter4j/EntitySupport.html#getHashtagEntities--) används för att hämta alla hash-taggar som ingår i tweet. Dessa släpps sedan till dataströmmen.

## <a name="configure-twitter"></a>Konfigurera Twitter

Använd följande steg för att registrera en ny Twitter-program och få konsumenten och åtkomst-token information som behövs för att läsa från Twitter:

1. Gå till [Twitter appar](https://apps.twitter.com) och klicka på den **Skapa ny app** knappen. När du fyller i formuläret lämnar den **motringning URL** fältet tomt.

2. När appen har skapats klickar du på den **nycklar och åtkomst-token** fliken.

3. Kopiera den **konsumenten nyckeln** och **konsumenthemlighet** information.

4. Längst ned på sidan Välj **skapa åtkomst-token** om det finns inga token. När du har skapat token kopiera den **åtkomst-Token** och **åtkomst-Token hemlighet** information.

5. I den **TwitterSpoutTopology** projekt som du tidigare klonas, öppnar den **resources/twitter4j.properties** filen, lägga till den information du samlade in i föregående steg och spara sedan filen.

## <a name="build-the-topology"></a>Skapa topologin

Använd följande kod för att skapa projektet:

        cd [directoryname]
        mvn compile

## <a name="test-the-topology"></a>Testa topologin

Använd följande kommando för att testa topologin lokalt:

    mvn compile exec:java -Dstorm.topology=com.microsoft.example.TwitterTrendingTopology

Du bör se felsökningsinformation som innehåller hashen-taggar och räknar skickade av topologi när topologin har startat. Resultatet bör likna följande:

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

Nu när du har testat lokalt topologin kan identifiera hur du distribuerar topologin: [distribuera och hantera Apache Storm-topologier på HDInsight](hdinsight-storm-deploy-monitor-topology.md).

Du kanske även är intresserad av i följande Storm-avsnitt:

* [Utveckla Java-topologier för Storm på HDInsight med Maven](hdinsight-storm-develop-java-topology.md)
* [Utveckla C#-topologier för Storm på HDInsight med Visual Studio](hdinsight-storm-develop-csharp-visual-studio-topology.md)

Fler exempel i Storm för HDInsight:

* [Exempeltopologier för Storm på HDInsight](hdinsight-storm-example-topology.md)

