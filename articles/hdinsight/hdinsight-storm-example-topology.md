---
title: "aaaExample Apache Storm-topologier på HDInsight | Microsoft Docs"
description: "En lista över exempel på Storm-topologier skapas och testas med Apache Storm på HDInsight inklusive grundläggande C# och Java-topologier och arbetar med Händelsehubbar."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: f9b1bdff-5928-4705-a76d-52fd200917cb
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/07/2017
ms.author: larryfr
ms.openlocfilehash: b20299112f6489b7c99360f0a539fc732703c64a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="example-storm-topologies-and-components-for-apache-storm-on-hdinsight"></a>Exempel på Storm-topologier och komponenter för Apache Storm på HDInsight

hello följer en lista över exempel skapas och hanteras av Microsoft för användning med Apache Storm på HDInsight. De här exemplen omfattar en mängd ämnen, från att skapa grundläggande C# och Java-topologier tooworking med Azure-tjänster, till exempel Händelsehubbar, Cosmos DB, Power BI, SQL-databas, HBase på HDInsight och Azure Storage. Några exempel visas också hur toowork med Azure- eller även icke-Microsoft-teknik som SignalR och Socket.IO.

| Beskrivning | Visar | Språk/Framework |
|:--- |:--- |:--- |
| [Skriva tooAzure Data Lake Store från Apache Storm](hdinsight-storm-write-data-lake-store.md) |Skriva tooAzure Data Lake Store |Java |
| [Event Hub-kanalen och bult källa](https://github.com/apache/storm/tree/master/external/storm-eventhubs) |Källa för hello Event Hub-kanalen och bult |Java |
| [Utveckla Java-baserad topologier för Apache Storm på HDInsight][5797064f] |Maven |Java |
| [Utveckla C#-topologier för Apache Storm på HDInsight med Visual Studio][16fce2d1] |HDInsight Tools för Visual Studio |C#, Java |
| [Skapa flera dataströmmar i en C# Storm-topologi][ec5a4064] |Flera strömmar |C# |
| [Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (C#)][844d1d81] |Händelsehubbar |C# och Java |
| [Bearbeta händelser från Azure Event Hubs med Storm på HDInsight (Java)](hdinsight-storm-develop-java-event-hub-topology.md) |Händelsehubbar |Java |
| [Använd Power Bi toovisualize data från en Storm-topologi][94d15238] |Power BI |C# |
| [Analysera sensordata med Storm och HBase i HDInsight][ab894747] |Event Hubs, HBase, Socket.IO, Web instrumentpanelen |C#, Java, JavaScript, HTML |
| [Bearbeta vehicle sensordata från Händelsehubbar med Storm på HDInsight][246ee964] |Event Hubs Cosmos DB Azure Storage Blob (WASB) |C#, Java |
| [Extrahering, transformering och inläsning (ETL) från Azure Event Hubs tooHBase med Storm på HDInsight][b4b68194] |Händelsehubbar, HBase |C# |
| [Mallprojekt C# Storm-topologi för att arbeta med Azure-tjänster från Storm på HDInsight][ce0c02a2] |Händelsen NAV, Cosmos DB SQL-databas, HBase, SignalR |C#, Java |
| [Skalbarhet prestandamått för att läsa från Azure Event Hubs med Storm på HDInsight][d6c540e3] |Genomströmningen, Event Hubs SQL-databas |C#, Java |
| [Samordna händelser med Storm och HBase på HDInsight](hdinsight-storm-correlation-topology.md) |HBase |C# |
| [Använda Python med Storm på HDInsight](hdinsight-storm-develop-python-topology.md) |Python-komponenter med en topologi som |Python |
| [Använda Kafka med Storm på HDInsight](hdinsight-apache-storm-with-kafka.md) | Apache Storm läsning och skrivning tooApache Kafka | Java |

### <a name="next-steps"></a>Nästa steg

* [Komma igång med Apache Storm i HDInsight][2b8c3488]
* [Lär dig hur toodeploy och hantera Storm-topologier med Storm på HDInsight][6eb0d3b8]

[2b8c3488]: hdinsight-apache-storm-tutorial-get-started-linux.md "Lär dig hur toocreate ett Storm på HDInsight-kluster och använder hello exempeltopologier för Storm-instrumentpanelen toodeploy."
[6eb0d3b8]: hdinsight-storm-deploy-monitor-topology.md "Lär dig hur toodeploy och hantera topologier med hjälp av hello webbaserad Storm-instrumentpanel och Storm-Användargränssnittet eller hello HDInsight Tools för Visual Studio."
[16fce2d1]: hdinsight-storm-develop-csharp-visual-studio-topology.md "Lär dig hur toocreate C# Storm-topologier med hjälp av hello HDInsight Tools för Visual Studio."
[5797064f]: hdinsight-storm-develop-java-topology.md "Lär dig hur toocreate Storm-topologier i Java, med Maven, genom att skapa en grundläggande wordcount-topologi."
[94d15238]: hdinsight-storm-power-bi-topology.md "Visar hur toowrite data tooPower BI från en C#-topologi, sedan skapa ett diagram och en instrumentpanel från hello data."
[ec5a4064]: https://github.com/Blackmist/csharp-storm-example "Visar en grundläggande Storm-topologi som utför en wordcount implementeras i C#. Detta demonstrerar hur toocreate flera dataströmmar inom en C#-topologi."
[844d1d81]: hdinsight-storm-develop-csharp-event-hub-topology.md "Lär dig hur tooread och skriva data från Azure Event Hubs med Storm på HDInsight."
[ab894747]: hdinsight-storm-sensor-data-analysis.md "Lär dig hur toouse Apache Storm på HDInsight tooprocess sensordata från Azure Event Hubs visualisera den med hjälp av D3.js och lagra (valfritt) tooHBase."
[246ee964]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/IotExample/README.md "Lär dig hur toouse Storm-topologi tooread meddelanden från Azure Event Hubs läsa dokument från Azure Cosmos DB för data refererar till och spara data tooAzure lagring."
[d6c540e3]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/EventCountExample "Flera topologier toodemonstrate dataflöde vid läsning från Azure Event Hubs och lagra tooSQL databasen med Apache Storm på HDInsight."
[b4b68194]: https://github.com/hdinsight/hdinsight-storm-examples/blob/master/RealTimeETLExample "Lär dig hur tooread data från Azure Event Hubs mängd & transformering hello data och sedan lagra den tooHBase på HDInsight."
[ce0c02a2]: https://github.com/hdinsight/hdinsight-storm-examples/tree/master/templates/HDInsightStormExamples "Projektet innehåller mallar för kanaler, bultar och topologier toointeract med olika Azure-tjänster som Händelsehubbar, Cosmos-DB och SQL-databas."

