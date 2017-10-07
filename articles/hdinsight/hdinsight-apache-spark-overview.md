---
title: "aaaIntroduction tooSpark på Azure HDInsight | Microsoft Docs"
description: "Den här artikeln innehåller en introduktion tooSpark på HDInsight och hello olika scenarier där du kan använda Spark-kluster i HDInsight."
keywords: "Vad är apache spark, spark-kluster, introduktion toospark spark i hdinsight"
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 82334b9e-4629-4005-8147-19f875c8774e
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 05/12/2017
ms.author: nitinme
ms.openlocfilehash: 41996e733618b8534469fa239b980ac50161a535
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="introduction-toospark-on-hdinsight"></a>Introduktion tooSpark på HDInsight

Den här artikeln ger en introduktion tooSpark på HDInsight. <a href="http://spark.apache.org/" target="_blank">Apache Spark</a> är ett ramverk för parallellbearbetning med öppen källkod som stöder minnesintern bearbetning tooboost hello prestanda hos ett stort program för stordataanalys. Spark-kluster i HDInsight är kompatibelt med Azure Storage (WASB) samt Azure Data Lake Store så att dina befintliga data som lagras i Azure enkelt kan bearbetas via Spark-kluster.

När du skapar ett Spark-kluster i HDInsight skapas Azure-beräkningsresurser med Spark installerat och konfigurerat. Det tar bara ungefär tio minuter toocreate ett Spark-kluster i HDInsight. hello data toobe bearbetas lagras i Azure Storage eller Azure Data Lake Store. Se [Använda Azure Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).

**toocreate ett Spark-kluster i HDInsight**, se [Snabbstart: skapa ett Spark-kluster i HDInsight och köra interaktiva frågor med Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="what-is-apache-spark-on-azure-hdinsight"></a>Vad är Apache Spark i Azure HDInsight?
Med Spark-kluster HDInsight får du tillgång till en helt hanterad Spark-tjänst. Fördelarna med att skapa ett Spark-kluster i HDInsight visas här.

| Funktion | Beskrivning |
| --- | --- |
| Enkelt att skapa Spark-kluster |Du kan skapa ett nytt Spark-kluster i HDInsight i minuter med hjälp av hello Azure-portalen, Azure PowerShell eller hello HDInsight .NET SDK. Se [Komma igång med Spark-kluster i HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md) |
| Användbarhet |Spark-kluster i HDInsight innehåller Jupyter och Zeppelin-anteckningsböcker. Du kan använda dem för interaktiv databehandling och visualisering.|
| REST API:er |Spark-kluster i HDInsight innehåller [Livius](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server), en REST API-baserad Spark jobbet server tooremotely skicka och övervaka jobb. |
| Stöd för Azure Data Lake Store | Spark-kluster i HDInsight kan vara konfigurerade toouse Azure Data Lake Store som ett ytterligare lagringsutrymme, samt primära lagring (endast med 3.5 HDInsight-kluster). Mer information om Data Lake Store finns i [Översikt över Azure Data Lake Store](../data-lake-store/data-lake-store-overview.md). |
| Integrering med Azure-tjänster |Spark-kluster i HDInsight levereras med en koppling tooAzure Händelsehubbar. Kunder kan skapa strömmade program med hjälp av hello Händelsehubbar dessutom för[Kafka](http://kafka.apache.org/), som finns redan som en del av Spark. |
| Stöd för R Server | Du kan konfigurera en R Server med HDInsight Spark klustret toorun distribuerade R-beräkningar med hello hastigheter Spark-klustret har kapacitet. Mer information finns i [Komma igång med R Server på HDInsight](hdinsight-hadoop-r-server-get-started.md). |
| Integrering med tredje parts IDEs | HDInsight tillhandahåller plugin-program för IDEs som IntelliJ IDEA och Eclipse att du kan använda toocreate och skicka program tooan HDInsight Spark-kluster. Mer information finns i [Använd Azure Toolkit för IntelliJ IDEA](hdinsight-apache-spark-intellij-tool-plugin.md) och [Använd Azure Toolkit för Eclipse](hdinsight-apache-spark-eclipse-tool-plugin.md).|
| Samtidiga frågor |Spark-kluster i HDInsight har stöd för samtidiga frågor. Detta gör att flera frågor från en användare eller flera frågor från olika användare och program tooshare hello samma klusterresurser. |
| Cachelagring i SSD:er |Du kan välja att toocache data antingen i minnet eller i SSD: er anslutna toohello klusternoder. Cachelagring i minnet ger bästa frågeprestanda-hello men kan vara dyrt. cachelagring i SSD: er är ett bra alternativ för att förbättra frågeprestanda utan hello måste toocreate ett kluster med en storlek som är nödvändiga toofit hello hela datauppsättningen i minnet. |
| Integrering med BI-verktyg |Spark-kluster för HDInsight tillhandahåller anslutningsappar för BI-verktyg som [Power BI](http://www.powerbi.com/) och [Tableau](http://www.tableau.com/products/desktop) för dataanalys. |
| Förinstallerade Anaconda-bibliotek |Spark-kluster i HDInsight kommer med förinstallerade Anaconda-bibliotek. [Anaconda](http://docs.continuum.io/anaconda/) ger Stäng too200 bibliotek för machine learning, dataanalys, visualisering, osv. |
| Skalbarhet |Men du kan ange hello antalet noder i klustret när du skapar, om du vill toogrow eller krympa hello toomatch klusterarbetsbelastning. Alla HDInsight-kluster kan du toochange hello antalet noder i klustret hello. Spark-kluster kan också att tas bort utan förlust av data, eftersom alla hello data lagras i Azure Storage eller Azure Data Lake Store. |
| Dygnet runt-support hela veckan |Spark-kluster på HDInsight levereras med dygnet runt-support på företagsnivå veckans alla dagar och ett serviceavtal för 99,9 % drifttid. |

## <a name="what-are-hello-use-cases-for-spark-on-hdinsight"></a>Vad är hello användningsområden för Spark i HDInsight?
Spark-kluster i HDInsight aktivera hello följande viktiga scenarier.

### <a name="interactive-data-analysis-and-bi"></a>Interaktiv dataanalys och BI
[Titta på en genomgång](hdinsight-apache-spark-use-bi-tools.md)

Apache Spark i HDInsight lagrar data i Azure Storage eller Azure Data Lake Store. Affärsexperter och beslutsfattare kan analysera och skapa rapporter över dessa data och använda Microsoft Power BI toobuild interaktiva rapporter utifrån analyserade hello data. Analytiker kan starta från Ostrukturerade/halvstrukturerade data i klusterlagring, definiera ett schema för hello data med anteckningsböcker och sedan skapa datamodeller med Microsoft Power BI. Spark-kluster i HDInsight har också stöd för ett antal BI-verktyg från tredje part, bland annat Tableau. Det gör det till en utmärkt plattform för dataanalytiker, affärsexperter och beslutsfattare.

### <a name="spark-machine-learning"></a>Spark-maskininlärning
[Titta på en genomgång: Förutsäga temperaturer i en byggnad med hjälp av HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)

[Titta på en genomgång: Förutsäga resultatet av en livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)

Med Apache Spark medföljer Machine Learning-biblioteket [MLlib](http://spark.apache.org/mllib/) som är byggt på Spark som du kan använda från ett Spark-kluster i HDInsight. Spark-kluster i HDInsight innehåller Anaconda, en Python-distribution med en rad olika paket för Machine Learning. Ihop med det inbyggda stödet för Jupyter- och Zeppelin-anteckningsböcker ger det en förstklassig miljö att skapa Machine Learning-program i.

### <a name="spark-streaming-and-real-time-data-analysis"></a>Spark-strömning och dataanalys i realtid
[Titta på en genomgång](hdinsight-apache-spark-eventhub-streaming.md)

Spark-kluster i HDInsight innehåller omfattande stöd för att skapa lösningar för realtidsanalys. Spark har kopplingar tooingest data från flera källor som Kafka, Flume, Twitter, ZeroMQ och TCP sockets, Spark i HDInsight du dessutom förstklassig stöd för att föra in data från Azure Event Hubs. Händelsehubbar är hello används mest kötjänsten på Azure. Det inbyggda stödet för Event Hubs gör Spark-kluster i HDInsight till en perfekt plattform för att skapa en pipeline för realtidsanalyser.

## <a name="next-steps"></a>Vilka komponenter medföljer som en del i ett Spark-kluster?
Spark-kluster i HDInsight innehåller hello följande komponenter som är tillgängliga på hello klustren som standard.

* [Spark Core](https://spark.apache.org/docs/1.5.1/). Omfattar Spark Core, Spark SQL, Spark-API:er för strömning, GraphX och MLlib.
* [Anaconda](http://docs.continuum.io/anaconda/)
* [Livy](https://github.com/cloudera/hue/tree/master/apps/spark/java#welcome-to-livy-the-rest-spark-server)
* [Jupyter Notebook](https://jupyter.org)
* [Zeppelin Notebook](http://zeppelin-project.org/)

Spark-kluster i HDInsight tillhandahåller även en [ODBC-drivrutinen](http://go.microsoft.com/fwlink/?LinkId=616229) för anslutningen tooSpark kluster i HDInsight från BI-verktyg som Microsoft Power BI och Tableau.

## <a name="where-do-i-start"></a>Vad ska jag börja med?
Börja med att skapa ett Spark-kluster i HDInsight. Se [Snabbstart: skapa ett Spark-kluster i HDInsight Linux och köra exempelprogram med Jupyter](hdinsight-apache-spark-jupyter-spark-sql.md). 

## <a name="next-steps"></a>Nästa steg
### <a name="scenarios"></a>Scenarier
* [Spark med BI: Utföra interaktiv dataanalys med hjälp av Spark i HDInsight med BI-verktyg](hdinsight-apache-spark-use-bi-tools.md)
* [Spark med Machine Learning: Använda Spark i HDInsight för analys av byggnadstemperatur med HVAC-data](hdinsight-apache-spark-ipython-notebook-machine-learning.md)
* [Spark med Machine Learning: använda Spark i HDInsight toopredict livsmedelskontroll](hdinsight-apache-spark-machine-learning-mllib-ipython.md)
* [Spark Streaming: Använda Spark i HDInsight för att bygga program för strömning i realtid](hdinsight-apache-spark-eventhub-streaming.md)
* [Webbplatslogganalys med Spark i HDInsight](hdinsight-apache-spark-custom-library-website-log-analysis.md)

### <a name="create-and-run-applications"></a>Skapa och köra program
* [Skapa ett fristående program med hjälp av Scala](hdinsight-apache-spark-create-standalone-application.md)
* [Köra jobb via fjärranslutning på ett Spark-kluster med Livy](hdinsight-apache-spark-livy-rest-interface.md)

### <a name="tools-and-extensions"></a>Verktyg och tillägg
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toocreate och skicka Spark Scala-appar](hdinsight-apache-spark-intellij-tool-plugin.md)
* [Använda HDInsight Tools-Plugin för IntelliJ IDEA toodebug Spark-program via fjärranslutning](hdinsight-apache-spark-intellij-tool-plugin-debug-jobs-remotely.md)
* [Använda Zeppelin-anteckningsböcker med ett Spark-kluster i HDInsight](hdinsight-apache-spark-zeppelin-notebook.md)
* [Kernlar som är tillgängliga för Jupyter Notebook i Spark-klustret för HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md)
* [Använda externa paket med Jupyter-anteckningsböcker](hdinsight-apache-spark-jupyter-notebook-use-external-packages.md)
* [Installera Jupyter på datorn och ansluta tooan HDInsight Spark-kluster](hdinsight-apache-spark-jupyter-notebook-install-locally.md)

### <a name="manage-resources"></a>Hantera resurser
* [Hantera resurser för hello Apache Spark-kluster i Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Följa och felsöka jobb som körs i ett Apache Spark-kluster i HDInsight](hdinsight-apache-spark-job-debugging.md)
* [Kända problem i Apache Spark i Azure HDInsight](hdinsight-apache-spark-known-issues.md).
