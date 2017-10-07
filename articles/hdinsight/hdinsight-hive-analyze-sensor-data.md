---
title: aaaAnalyze sensordata med Hive och Hadoop - Azure HDInsight | Microsoft Docs
description: "Lär dig hur hello tooanalyze sensordata med Hive-fråga konsol med HDInsight (Hadoop) och visualisera hello data i Microsoft Excel med PowerView."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: a8ac160c-1cef-45d9-bf36-7beb5a439105
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: larryfr
ROBOTS: NOINDEX
ms.openlocfilehash: 70e595705c33d9835dc9809161f79c3ac5ece870
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="analyze-sensor-data-using-hello-hive-query-console-on-hadoop-in-hdinsight"></a><span data-ttu-id="0bd07-103">Analysera sensordata med hello Hive-fråga konsolen på Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0bd07-103">Analyze sensor data using hello Hive Query Console on Hadoop in HDInsight</span></span>

<span data-ttu-id="0bd07-104">Lär dig hur hello tooanalyze sensordata med Hive-fråga konsol med HDInsight (Hadoop) och visualisera hello data i Microsoft Excel med hjälp av Power View.</span><span class="sxs-lookup"><span data-stu-id="0bd07-104">Learn how tooanalyze sensor data by using hello Hive Query Console with HDInsight (Hadoop), then visualize hello data in Microsoft Excel by using Power View.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="0bd07-105">hello stegen i det här dokumentet som endast fungerar med Windows-baserade HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0bd07-105">hello steps in this document only work with Windows-based HDInsight clusters.</span></span> <span data-ttu-id="0bd07-106">HDInsight är endast tillgängligt i Windows för versioner som är lägre än HDInsight 3.4.</span><span class="sxs-lookup"><span data-stu-id="0bd07-106">HDInsight is only available on Windows for versions lower than HDInsight 3.4.</span></span> <span data-ttu-id="0bd07-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0bd07-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0bd07-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0bd07-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>


<span data-ttu-id="0bd07-109">I det här exemplet använder Hive tooprocess historiska data och identifiera problem med uppvärmning och luftkonditionering system.</span><span class="sxs-lookup"><span data-stu-id="0bd07-109">In this sample, you use Hive tooprocess historical data and identify problems with heating and air conditioning systems.</span></span> <span data-ttu-id="0bd07-110">Mer specifikt kan du identifiera datorer är inte tooreliably bibehålla en inställd temperatur genom att utföra följande uppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="0bd07-110">Specifically, you identify systems are not able tooreliably maintain a set temperature by performing hello following tasks:</span></span>

* <span data-ttu-id="0bd07-111">Skapa HIVE-tabeller tooquery data som lagras i en fil med kommaavgränsade värden (CSV) filer.</span><span class="sxs-lookup"><span data-stu-id="0bd07-111">Create HIVE tables tooquery data stored in comma-separated value (CSV) files.</span></span>
* <span data-ttu-id="0bd07-112">Skapa HIVE-frågor tooanalyze hello data.</span><span class="sxs-lookup"><span data-stu-id="0bd07-112">Create HIVE queries tooanalyze hello data.</span></span>
* <span data-ttu-id="0bd07-113">tooretrieve hello analyserade data använda Microsoft Excel tooconnect tooHDInsight.</span><span class="sxs-lookup"><span data-stu-id="0bd07-113">tooretrieve hello analyzed data, use Microsoft Excel tooconnect tooHDInsight.</span></span>
* <span data-ttu-id="0bd07-114">toovisualize hello data använda Power View.</span><span class="sxs-lookup"><span data-stu-id="0bd07-114">toovisualize hello data, use Power View.</span></span>

![Ett diagram över hello lösningsarkitektur](./media/hdinsight-hive-analyze-sensor-data/hvac-architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="0bd07-116">Krav</span><span class="sxs-lookup"><span data-stu-id="0bd07-116">Prerequisites</span></span>

* <span data-ttu-id="0bd07-117">Ett kluster i HDInsight (Hadoop): se [skapa Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md) information om hur du skapar ett kluster.</span><span class="sxs-lookup"><span data-stu-id="0bd07-117">An HDInsight (Hadoop) cluster: See [Create Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md) for information about creating a cluster.</span></span>
* <span data-ttu-id="0bd07-118">Microsoft Excel 2013</span><span class="sxs-lookup"><span data-stu-id="0bd07-118">Microsoft Excel 2013</span></span>

  > [!NOTE]
  > <span data-ttu-id="0bd07-119">Microsoft Excel används för datavisualisering med [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span><span class="sxs-lookup"><span data-stu-id="0bd07-119">Microsoft Excel is used for data visualization with [Power View](https://support.office.com/Article/Power-View-Explore-visualize-and-present-your-data-98268d31-97e2-42aa-a52b-a68cf460472e?ui=en-US&rs=en-US&ad=US).</span></span>

* [<span data-ttu-id="0bd07-120">Microsoft Hive ODBC-drivrutinen</span><span class="sxs-lookup"><span data-stu-id="0bd07-120">Microsoft Hive ODBC Driver</span></span>](http://www.microsoft.com/download/details.aspx?id=40886)

## <a name="toorun-hello-sample"></a><span data-ttu-id="0bd07-121">toorun hello-exempel</span><span class="sxs-lookup"><span data-stu-id="0bd07-121">toorun hello sample</span></span>

1. <span data-ttu-id="0bd07-122">Från din webbläsare, navigerar du toohello följande URL:</span><span class="sxs-lookup"><span data-stu-id="0bd07-122">From your web browser, navigate toohello following URL:</span></span> 

         https://<clustername>.azurehdinsight.net

    <span data-ttu-id="0bd07-123">Ersätt `<clustername>` med hello namnet på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0bd07-123">Replace `<clustername>` with hello name of your HDInsight cluster.</span></span>

    <span data-ttu-id="0bd07-124">När du uppmanas, autentisera med Hej administratörsanvändarnamn och lösenord som du använde vid etablering av det här klustret.</span><span class="sxs-lookup"><span data-stu-id="0bd07-124">When prompted, authenticate by using hello administrator user name and password you used when provisioning this cluster.</span></span>

2. <span data-ttu-id="0bd07-125">Från hello webbsida som öppnas, klickar du på hello **komma igång-galleriet** fliken och sedan under hello **lösningar med exempeldata** kategori, klicka på hello **Sensor dataanalys** exempel.</span><span class="sxs-lookup"><span data-stu-id="0bd07-125">From hello web page that opens, click hello **Getting Started Gallery** tab, and then under hello **Solutions with Sample Data** category, click hello **Sensor Data Analysis** sample.</span></span>

    ![Komma igång galleriet bild](./media/hdinsight-hive-analyze-sensor-data/getting-started-gallery.png)

3. <span data-ttu-id="0bd07-127">Följ instruktionerna på hello webbsida toofinish hello sampel hello.</span><span class="sxs-lookup"><span data-stu-id="0bd07-127">Follow hello instructions provided on hello web page toofinish hello sample.</span></span>
