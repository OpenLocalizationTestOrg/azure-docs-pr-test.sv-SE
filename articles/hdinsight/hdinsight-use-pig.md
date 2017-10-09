---
title: aaaUse Hadoop Pig i HDInsight | Microsoft Docs
description: "Lär dig hur toouse svin med Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: acfeb52b-4b81-4a7d-af77-3e9908407404
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 90850f2c742b8954c66ce277127e01b14fc3906f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-pig-with-hadoop-on-hdinsight"></a>Använda Pig med Hadoop i HDInsight

Lär dig hur toouse [Apache Pig](http://pig.apache.org/) med HDInsight...

Pig är en plattform för att skapa program för Hadoop med hjälp av en procedurmässig språk som kallas *Pig Latin*. Pig är en alternativ tooJava för att skapa *MapReduce* lösningar och ingår i Azure HDInsight. Använd följande tabell toodiscover hello olika sätt att du kan använda Pig med HDInsight hello:

| **Använd den här** om du vill... | .. .an **interaktiva** shell | ... **batch** bearbetning | ... med detta **klustret operativsystem** | .. .from detta **klientoperativsystem** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X eller Windows |
| [REST-API](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux- eller Windows |Linux, Unix, Mac OS X eller Windows |
| [.NET SDK för Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux- eller Windows |Windows (för tillfället) |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux- eller Windows |Windows |
| [Fjärrskrivbord](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 och 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a id="why"></a>Varför använda Pig

En av hello utmaningar för bearbetning av data genom att använda MapReduce i Hadoop implementerar logik för bearbetning med hjälp av endast en karta och minska funktionen. För komplexa bearbetning av du ofta har sammankopplade toobreak bearbetning till flera MapReduce-åtgärder som är tooachieve hello önskat resultat.

Pig kan toodefine bearbetning som en serie transformationer som hello dataflöden via tooproduce hello önskad utdata.

Hej Pig Latin språk kan du toodescribe hello dataflöde från rådata indata via en eller flera omvandlingar tooproduce hello önskad utdata. Pig Latin program följer detta allmänna mönster:

* **Läs in**: Läs data toobe ändras från hello-filsystem

* **Transformera**: ändra hello-data

* **Dump eller lagra**: utdata data toohello skärmbild eller lagra för bearbetning

### <a name="user-defined-functions"></a>Användardefinierade funktioner

Pig Latin stöder även användardefinierade funktioner (UDF), vilket gör att du tooinvoke externa komponenter som implementerar logik som är svår toomodel i Pig Latin.

Läs mer om Pig Latin [Pig Latin referens manuell 1](http://pig.apache.org/docs/r0.7.0/piglatin_ref1.html) och [Pig Latin referens manuell 2](http://pig.apache.org/docs/r0.7.0/piglatin_ref2.html).

Ett exempel på med Pig UDF: er, finns i hello följande dokument:

* [Använda DataFu med Pig i HDInsight](hdinsight-hadoop-use-pig-datafu-udf.md) -DataFu är en samling användbar UDF: er som underhålls av Apache
* [Använda Python med Pig och Hive i HDInsight](hdinsight-python.md)
* [Använda C# med Hive och Pig i HDInsight](hdinsight-hadoop-hive-pig-udf-dotnet-csharp.md)

## <a id="data"></a>Exempeldata

HDInsight tillhandahåller olika exempel datauppsättningar som lagras i hello `/example/data` och `/HdiSamples` kataloger. Dessa kataloger finns i hello standardlagring för klustret. Hej Pig exemplet i det här dokumentet använder hello *log4j* filen från `/example/data/sample.log`.

Varje logg i hello fil består av en rad med fält som innehåller en `[LOG LEVEL]` fältet tooshow hello-typ och hello allvarlighetsgrad, till exempel:

    2012-02-03 20:26:41 SampleClass3 [ERROR] verbose detail for id 1527353937

I föregående exempel hello är hello loggningsnivån fel.

> [!NOTE]
> Du kan också generera en log4j-fil med hjälp av hello [Apache Log4j](http://en.wikipedia.org/wiki/Log4j) loggning av verktyget och sedan ladda upp filen tooyour blobben. Se [överför Data tooHDInsight](hdinsight-upload-data.md) anvisningar. Mer information om hur du använder blobbar i Azure storage med HDInsight finns [använda Azure Blob Storage med HDInsight](hdinsight-hadoop-use-blob-storage.md).

## <a id="job"></a>Exempel jobb

hello följande Pig Latin jobbet läses in hello `sample.log` filen från hello standardlagring för HDInsight-kluster. Därefter utför en serie transformationer som resulterar i en uppräkning av hur många gånger varje nivå i hello inkommande loggdata. hello resultat skrivs tooSTDOUT.

    LOGS = LOAD 'wasb:///example/data/sample.log';
    LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
    FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
    GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
    FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
    RESULT = order FREQUENCIES by COUNT desc;
    DUMP RESULT;

hello visar följande bild en sammanfattning av vad varje transformation har toohello data.

![Grafisk representation av hello omvandlingar][image-hdi-pig-data-transformation]

## <a id="run"></a>Kör jobb för hello Pig Latin

HDInsight kan köra Pig Latin jobb med hjälp av olika metoder. Använd hello efter tabellen toodecide vilken metod som passar dig sedan länken hello en genomgång.

| **Använd den här** om du vill... | .. .an **interaktiva** shell | ... **batch** bearbetning | ... med detta **klustret operativsystem** | .. .from detta **klienten** |
|:--- |:---:|:---:|:--- |:--- |
| [SSH](hdinsight-hadoop-use-pig-ssh.md) |✔ |✔ |Linux |Linux, Unix, Mac OS X eller Windows |
| [CURL](hdinsight-hadoop-use-pig-curl.md) |&nbsp; |✔ |Linux- eller Windows |Linux, Unix, Mac OS X eller Windows |
| [.NET SDK för Hadoop](hdinsight-hadoop-use-pig-dotnet-sdk.md) |&nbsp; |✔ |Linux- eller Windows |Windows (för tillfället) |
| [Windows PowerShell](hdinsight-hadoop-use-pig-powershell.md) |&nbsp; |✔ |Linux- eller Windows |Windows |
| [Fjärrskrivbord](hdinsight-hadoop-use-pig-remote-desktop.md) (HDInsight 3.2 och 3.3) |✔ |✔ |Windows |Windows |

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

## <a name="pig-and-sql-server-integration-services"></a>Pig och SQL Server Integration Services

Du kan använda SQL Server Integration Services (SSIS) toorun Pig-jobbet. hello Azure Funktionspaket för SSIS tillhandahåller hello följande komponenter som arbetar med Pig-jobb i HDInsight.

* [Azure HDInsight Pig-aktivitet][pigtask]

* [Azure prenumeration Anslutningshanteraren][connectionmanager]

Läs mer om hello Azure Funktionspaket för SSIS [här][ssispack].

## <a id="nextsteps"></a>Nästa steg
Nu när du har lärt dig hur länkar toouse Pig med HDInsight, Använd hello följande tooexplore andra sätt toowork med Azure HDInsight.

* [Ladda upp data tooHDInsight][hdinsight-upload-data]
* [Använda Hive med HDInsight][hdinsight-use-hive]
* [Använda Sqoop med HDInsight](hdinsight-use-sqoop.md)
* [Använda Oozie med HDInsight](hdinsight-use-oozie.md)
* [Använda MapReduce-jobb med HDInsight][hdinsight-use-mapreduce]

[apachepig-home]: http://pig.apache.org/
[putty]: http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html
[curl]: http://curl.haxx.se/
[pigtask]: http://msdn.microsoft.com/library/mt146781(v=sql.120).aspx
[connectionmanager]: http://msdn.microsoft.com/library/mt146773(v=sql.120).aspx
[ssispack]: http://msdn.microsoft.com/library/mt146770(v=sql.120).aspx


[hdinsight-upload-data]: hdinsight-upload-data.md

[hdinsight-admin-powershell]: hdinsight-administer-use-powershell.md

[hdinsight-use-hive]: hdinsight-use-hive.md
[hdinsight-use-mapreduce]: hdinsight-use-mapreduce.md

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-submit-jobs]: hdinsight-submit-hadoop-jobs-programmatically.md#mapreduce-sdk

[Powershell-install-configure]: /powershell/azureps-cmdlets-docs

[powershell-start]: http://technet.microsoft.com/library/hh847889.aspx


[image-hdi-pig-data-transformation]: ./media/hdinsight-use-pig/HDI.DataTransformation.gif
