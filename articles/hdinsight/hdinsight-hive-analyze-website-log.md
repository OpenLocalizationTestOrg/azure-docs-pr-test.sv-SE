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
# <a name="use-hive-with-windows-based-hdinsight-tooanalyze-logs-from-websites"></a><span data-ttu-id="0a29c-104">Använda Hive med Windows-baserade HDInsight tooanalyze loggar från webbplatser</span><span class="sxs-lookup"><span data-stu-id="0a29c-104">Use Hive with Windows-based HDInsight tooanalyze logs from websites</span></span>
<span data-ttu-id="0a29c-105">Lär dig hur toouse HiveQL med HDInsight tooanalyze loggar från en webbplats.</span><span class="sxs-lookup"><span data-stu-id="0a29c-105">Learn how toouse HiveQL with HDInsight tooanalyze logs from a website.</span></span> <span data-ttu-id="0a29c-106">Webbplatslogganalys kan vara används toosegment målgruppen baserat på liknande aktiviteter kan kategorisera besökare efter målgrupp och toofind hello innehåll de visa, hello webbplatser de kommer från och så vidare.</span><span class="sxs-lookup"><span data-stu-id="0a29c-106">Website log analysis can be used toosegment your audience based on similar activities, categorize site visitors by demographics, and toofind out hello content they view, hello websites they come from, and so on.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0a29c-107">hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0a29c-107">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="0a29c-108">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="0a29c-108">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="0a29c-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0a29c-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0a29c-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0a29c-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

<span data-ttu-id="0a29c-111">I det här exemplet använder du ett HDInsight kluster tooanalyze webbplats loggen filer tooget inblick i hello frekvens av besök toohello webbplats från externa webbplatser under en dag.</span><span class="sxs-lookup"><span data-stu-id="0a29c-111">In this sample, you will use an HDInsight cluster tooanalyze website log files tooget insight into hello frequency of visits toohello website from external websites in a day.</span></span> <span data-ttu-id="0a29c-112">Du måste också skapa en sammanfattning av de webbplatsfel som hello användare uppleva.</span><span class="sxs-lookup"><span data-stu-id="0a29c-112">You'll also generate a summary of website errors that hello users experience.</span></span> <span data-ttu-id="0a29c-113">Du får lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="0a29c-113">You will learn how to:</span></span>

* <span data-ttu-id="0a29c-114">Ansluta tooa Azure Blob storage som innehåller loggfiler för webbplatsen.</span><span class="sxs-lookup"><span data-stu-id="0a29c-114">Connect tooa Azure Blob storage, which contains website log files.</span></span>
* <span data-ttu-id="0a29c-115">Skapa HIVE-tabeller tooquery dessa loggar.</span><span class="sxs-lookup"><span data-stu-id="0a29c-115">Create HIVE tables tooquery those logs.</span></span>
* <span data-ttu-id="0a29c-116">Skapa HIVE-frågor tooanalyze hello data.</span><span class="sxs-lookup"><span data-stu-id="0a29c-116">Create HIVE queries tooanalyze hello data.</span></span>
* <span data-ttu-id="0a29c-117">Använda Microsoft Excel tooconnect tooHDInsight (med hjälp av öppna databasen anslutning (ODBC) tooretrieve hello analyserade data.</span><span class="sxs-lookup"><span data-stu-id="0a29c-117">Use Microsoft Excel tooconnect tooHDInsight (by using open database connectivity (ODBC) tooretrieve hello analyzed data.</span></span>

![HDI. Samples.Website.Log.Analysis][img-hdi-weblogs-sample]

## <a name="prerequisites"></a><span data-ttu-id="0a29c-119">Krav</span><span class="sxs-lookup"><span data-stu-id="0a29c-119">Prerequisites</span></span>
* <span data-ttu-id="0a29c-120">Du måste har etablerat ett Hadoop-kluster i Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0a29c-120">You must have provisioned a Hadoop cluster on Azure HDInsight.</span></span> <span data-ttu-id="0a29c-121">Instruktioner finns i [etablera HDInsight-kluster][hdinsight-provision].</span><span class="sxs-lookup"><span data-stu-id="0a29c-121">For instructions, see [Provision HDInsight Clusters][hdinsight-provision].</span></span>
* <span data-ttu-id="0a29c-122">Du måste ha Microsoft Excel 2013 eller Excel 2010 installerat.</span><span class="sxs-lookup"><span data-stu-id="0a29c-122">You must have Microsoft Excel 2013 or Excel 2010 installed.</span></span>
* <span data-ttu-id="0a29c-123">Du måste ha [Microsoft Hive ODBC-drivrutinen](http://www.microsoft.com/download/details.aspx?id=40886) tooimport data från Hive i Excel.</span><span class="sxs-lookup"><span data-stu-id="0a29c-123">You must have [Microsoft Hive ODBC Driver](http://www.microsoft.com/download/details.aspx?id=40886) tooimport data from Hive into Excel.</span></span>

## <a name="toorun-hello-sample"></a><span data-ttu-id="0a29c-124">toorun hello-exempel</span><span class="sxs-lookup"><span data-stu-id="0a29c-124">toorun hello sample</span></span>
1. <span data-ttu-id="0a29c-125">Från hello [Azure Portal](https://portal.azure.com/), hello startsidan (om du har Fäst det hello-kluster), klicka på hello klustret panelen som du vill använda toorun hello exempel.</span><span class="sxs-lookup"><span data-stu-id="0a29c-125">From hello [Azure Portal](https://portal.azure.com/), from hello Startboard (if you pinned hello cluster there), click hello cluster tile on which you want toorun hello sample.</span></span>
2. <span data-ttu-id="0a29c-126">Från hello klustret bladet under **snabblänkar**, klickar du på **Klusterinstrumentpanel**, och sedan hello **Klusterinstrumentpanel** bladet, klickar du på **HDInsight-kluster Instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="0a29c-126">From hello cluster blade, under **Quick Links**, click **Cluster Dashboard**, and then from hello **Cluster Dashboard** blade, click **HDInsight Cluster Dashboard**.</span></span> <span data-ttu-id="0a29c-127">Alternativt kan du direkt öppna hello instrumentpanelen genom att använda hello följande URL:</span><span class="sxs-lookup"><span data-stu-id="0a29c-127">Alternatively, you can directly open hello dashboard by using hello following URL:</span></span>

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="0a29c-128">När du uppmanas, autentisera med Hej administratörsanvändarnamn och lösenord som du använde vid etablering av hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0a29c-128">When prompted, authenticate by using hello administrator user name and password you used when provisioning hello cluster.</span></span>
3. <span data-ttu-id="0a29c-129">Från hello webbsida som öppnas, klickar du på hello **komma igång-galleriet** fliken och sedan under hello **lösningar med exempeldata** kategori, klicka på hello **Webbplatslogganalys** exempel.</span><span class="sxs-lookup"><span data-stu-id="0a29c-129">From hello web page that opens, click hello **Getting Started Gallery** tab, and then under hello **Solutions with Sample Data** category, click hello **Website Log Analysis** sample.</span></span>
4. <span data-ttu-id="0a29c-130">Följ instruktionerna på hello webbsida toofinish hello sampel hello.</span><span class="sxs-lookup"><span data-stu-id="0a29c-130">Follow hello instructions provided on hello web page toofinish hello sample.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0a29c-131">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0a29c-131">Next steps</span></span>
<span data-ttu-id="0a29c-132">Försök hello följande exempel: [analysera sensordata med Hive med HDInsight](hdinsight-hive-analyze-sensor-data.md).</span><span class="sxs-lookup"><span data-stu-id="0a29c-132">Try hello following sample: [Analyzing sensor data using Hive with HDInsight](hdinsight-hive-analyze-sensor-data.md).</span></span>

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-sensor-data-sample]: ../hdinsight-use-hive-sensor-data-analysis.md

[img-hdi-weblogs-sample]: ./media/hdinsight-hive-analyze-website-log/hdinsight-weblogs-sample.png
