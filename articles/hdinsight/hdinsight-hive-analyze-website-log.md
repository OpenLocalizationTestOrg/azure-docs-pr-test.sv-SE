---
title: "aaaUse Hive med Hadoop för analys av webbplatsloggar - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse Hive med HDInsight tooanalyze webbplats loggar. Du måste använda en loggfil som indata till ett HDInsight-tabell och använda HiveQL tooquery hello data."
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 6fb7b5c2-8df4-40b1-a9e2-6815080004f9
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/17/2016
ms.author: nitinme
ROBOTS: NOINDEX
ms.openlocfilehash: 9cbce3cc8cf8bc3ad104dc4ca6a5628802c8fe89
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-hive-with-windows-based-hdinsight-tooanalyze-logs-from-websites"></a>Använda Hive med Windows-baserade HDInsight tooanalyze loggar från webbplatser
Lär dig hur toouse HiveQL med HDInsight tooanalyze loggar från en webbplats. Webbplatslogganalys kan vara används toosegment målgruppen baserat på liknande aktiviteter kan kategorisera besökare efter målgrupp och toofind hello innehåll de visa, hello webbplatser de kommer från och så vidare.

> [!IMPORTANT]
> hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster. HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4. Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

I det här exemplet använder du ett HDInsight kluster tooanalyze webbplats loggen filer tooget inblick i hello frekvens av besök toohello webbplats från externa webbplatser under en dag. Du måste också skapa en sammanfattning av de webbplatsfel som hello användare uppleva. Du får lära dig hur du:

* Ansluta tooa Azure Blob storage som innehåller loggfiler för webbplatsen.
* Skapa HIVE-tabeller tooquery dessa loggar.
* Skapa HIVE-frågor tooanalyze hello data.
* Använda Microsoft Excel tooconnect tooHDInsight (med hjälp av öppna databasen anslutning (ODBC) tooretrieve hello analyserade data.

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a>Krav
* Du måste har etablerat ett Hadoop-kluster i Azure HDInsight. Instruktioner finns i [etablera HDInsight-kluster][hdinsight-provision].
* Du måste ha Microsoft Excel 2013 eller Excel 2010 installerat.
* Du måste ha [Microsoft Hive ODBC-drivrutinen](http://www.microsoft.com/download/details.aspx?id=40886) tooimport data från Hive i Excel.

## <a name="toorun-hello-sample"></a>toorun hello-exempel
1. Från hello [Azure Portal](https://portal.azure.com/), hello startsidan (om du har Fäst det hello-kluster), klicka på hello klustret panelen som du vill använda toorun hello exempel.
2. Från hello klustret bladet under **snabblänkar**, klickar du på **Klusterinstrumentpanel**, och sedan hello **Klusterinstrumentpanel** bladet, klickar du på **HDInsight-kluster Instrumentpanelen**. Alternativt kan du direkt öppna hello instrumentpanelen genom att använda hello följande URL:

         https://<clustername>.azurehdinsight.net

    När du uppmanas, autentisera med Hej administratörsanvändarnamn och lösenord som du använde vid etablering av hello klustret.
3. Från hello webbsida som öppnas, klickar du på hello **komma igång-galleriet** fliken och sedan under hello **lösningar med exempeldata** kategori, klicka på hello **Webbplatslogganalys** exempel.
4. Följ instruktionerna på hello webbsida toofinish hello sampel hello.

## <a name="next-steps"></a>Nästa steg
Försök hello följande exempel: [analysera sensordata med Hive med HDInsight](hdinsight-hive-analyze-sensor-data.md).

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
