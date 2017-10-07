---
title: "aaaDebug och analysera Hadoop-tjänster med heap - Dumpar i Azure | Microsoft Docs"
description: "Automatiskt samla in Dumpar heap för Hadoop-tjänster och placera i hello Azure Blob storage-konto för att felsöka och analys."
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
manager: jhubbard
editor: cgronlun
ms.assetid: e4ec4ebb-fd32-4668-8382-f956581485c4
ms.service: hdinsight
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2017
ms.author: jgao
ROBOTS: NOINDEX
ms.openlocfilehash: 70fbc2d6d97d35b0d7b1d9149673b02ae1878eb7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="collect-heap-dumps-in-blob-storage-toodebug-and-analyze-hadoop-services"></a><span data-ttu-id="6d2e2-103">Samla in heap Dumpar i Blob storage toodebug och analysera Hadoop-tjänster</span><span class="sxs-lookup"><span data-stu-id="6d2e2-103">Collect heap dumps in Blob storage toodebug and analyze Hadoop services</span></span>
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="6d2e2-104">Heap Dumpar innehåller en ögonblicksbild av hello programmets minne, inklusive hello värdena för variabler som för närvarande hello hello dump har skapats.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-104">Heap dumps contain a snapshot of hello application's memory, including hello values of variables at hello time hello dump was created.</span></span> <span data-ttu-id="6d2e2-105">Så att de är användbara för att diagnostisera problem som uppstår vid körning.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-105">So they are useful for diagnosing problems that occur at run-time.</span></span> <span data-ttu-id="6d2e2-106">Heap minnesdumpar kan samlas in för Hadoop-tjänster och placeras inuti hello Azure Blob storage-konto för en användare under HDInsightHeapDumps automatiskt /.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-106">Heap dumps can be automatically collected for Hadoop services and placed inside hello Azure Blob storage account of a user under HDInsightHeapDumps/.</span></span>

<span data-ttu-id="6d2e2-107">hello samling heap Dumpar för olika tjänster måste vara aktiverat för tjänster på enskilda kluster.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-107">hello collection of heap dumps for various services must be enabled for services on individual clusters.</span></span> <span data-ttu-id="6d2e2-108">hello standardvärdet för den här funktionen är toobe ut för ett kluster.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-108">hello default for this feature is toobe off for a cluster.</span></span> <span data-ttu-id="6d2e2-109">Dessa heap minnesdumpar kan vara stora, så det är lämpligt toomonitor hello Blob storage-konto där de sparas när hello samling har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-109">These heap dumps can be large, so it is advisable toomonitor hello Blob storage account where they are being saved once hello collection has been enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6d2e2-110">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="6d2e2-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="6d2e2-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="6d2e2-112">hello information i den här artikeln gäller bara tooWindows-baserade HDInsight.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-112">hello information in this article only applies tooWindows-based HDInsight.</span></span> <span data-ttu-id="6d2e2-113">Mer information om Linux-baserat HDInsight finns [aktivera heap Dumpar för Hadoop-tjänster på Linux-baserat HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span><span class="sxs-lookup"><span data-stu-id="6d2e2-113">For information on Linux-based HDInsight, see [Enable heap dumps for Hadoop services on Linux-based HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span></span>


## <a name="eligible-services-for-heap-dumps"></a><span data-ttu-id="6d2e2-114">Tjänster för heap minnesdumpar</span><span class="sxs-lookup"><span data-stu-id="6d2e2-114">Eligible services for heap dumps</span></span>
<span data-ttu-id="6d2e2-115">Du kan aktivera heap Dumpar för hello följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="6d2e2-115">You can enable heap dumps for hello following services:</span></span>

* <span data-ttu-id="6d2e2-116">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="6d2e2-116">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="6d2e2-117">**hive** -hiveserver2 metastore, derbyserver</span><span class="sxs-lookup"><span data-stu-id="6d2e2-117">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="6d2e2-118">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="6d2e2-118">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="6d2e2-119">**yarn** -resurshanteraren, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="6d2e2-119">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="6d2e2-120">**hdfs** -datanode secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="6d2e2-120">**hdfs** - datanode, secondarynamenode, namenode</span></span>

## <a name="configuration-elements-that-enable-heap-dumps"></a><span data-ttu-id="6d2e2-121">Konfigurationselement som möjliggör heap minnesdumpar</span><span class="sxs-lookup"><span data-stu-id="6d2e2-121">Configuration elements that enable heap dumps</span></span>
<span data-ttu-id="6d2e2-122">tooturn på heap Dumpar för en tjänst behöver du tooset hello lämpliga konfigurationselement i hello-avsnittet för tjänsten, som anges av **service_name**.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-122">tooturn on heap dumps for a service, you need tooset hello appropriate configuration elements in hello section for that service, which is specified by **service_name**.</span></span>

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

<span data-ttu-id="6d2e2-123">Hej värdet för **service_name** kan vara någon av hello-tjänster som listas här: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resurshanteraren, nodemanager, timelineserver, datanode secondarynamenode, eller namenode.</span><span class="sxs-lookup"><span data-stu-id="6d2e2-123">hello value of **service_name** can be any of hello services listed here: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, or namenode.</span></span>

## <a name="enable-using-azure-powershell"></a><span data-ttu-id="6d2e2-124">Aktivera med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="6d2e2-124">Enable using Azure PowerShell</span></span>
<span data-ttu-id="6d2e2-125">Till exempel tooturn på heap Dumpar med hjälp av Azure PowerShell för jobhistoryserver du kan använda hello följande skript:</span><span class="sxs-lookup"><span data-stu-id="6d2e2-125">For example, tooturn on heap dumps by using Azure PowerShell for jobhistoryserver, you can use hello following script:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a><span data-ttu-id="6d2e2-126">Aktivera med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="6d2e2-126">Enable using .NET SDK</span></span>
<span data-ttu-id="6d2e2-127">Till exempel tooturn på heap Dumpar med hello Azure HDInsight .NET SDK för jobhistoryserver du kan använda hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="6d2e2-127">For example, tooturn on heap dumps by using hello Azure HDInsight .NET SDK for jobhistoryserver, you can use hello following code:</span></span>

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
