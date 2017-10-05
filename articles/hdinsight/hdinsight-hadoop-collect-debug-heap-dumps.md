---
title: "Felsöka och analysera Hadoop-tjänster med heap - Dumpar i Azure | Microsoft Docs"
description: "Automatiskt samla in Dumpar heap för Hadoop-tjänster och placera i Azure Blob storage-konto för att felsöka och analys."
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
ms.openlocfilehash: 6d1d4d47d279eb7a1f0bf1f587445683f0ace7a0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="collect-heap-dumps-in-blob-storage-to-debug-and-analyze-hadoop-services"></a><span data-ttu-id="ec42a-103">Samla in heap Dumpar i Blob storage för att felsöka och analysera Hadoop-tjänster</span><span class="sxs-lookup"><span data-stu-id="ec42a-103">Collect heap dumps in Blob storage to debug and analyze Hadoop services</span></span>
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

<span data-ttu-id="ec42a-104">Heap Dumpar innehåller en ögonblicksbild av programmets minne, inklusive värdena för variabler vid tiden för dumpen skapades.</span><span class="sxs-lookup"><span data-stu-id="ec42a-104">Heap dumps contain a snapshot of the application's memory, including the values of variables at the time the dump was created.</span></span> <span data-ttu-id="ec42a-105">Så att de är användbara för att diagnostisera problem som uppstår vid körning.</span><span class="sxs-lookup"><span data-stu-id="ec42a-105">So they are useful for diagnosing problems that occur at run-time.</span></span> <span data-ttu-id="ec42a-106">Heap minnesdumpar kan samlas in för Hadoop-tjänster och placeras inuti Azure Blob storage-konto för en användare under HDInsightHeapDumps automatiskt /.</span><span class="sxs-lookup"><span data-stu-id="ec42a-106">Heap dumps can be automatically collected for Hadoop services and placed inside the Azure Blob storage account of a user under HDInsightHeapDumps/.</span></span>

<span data-ttu-id="ec42a-107">Insamling av heap Dumpar för olika tjänster måste vara aktiverat för tjänster på enskilda kluster.</span><span class="sxs-lookup"><span data-stu-id="ec42a-107">The collection of heap dumps for various services must be enabled for services on individual clusters.</span></span> <span data-ttu-id="ec42a-108">Standardvärdet för den här funktionen ska vara inaktiverat för ett kluster.</span><span class="sxs-lookup"><span data-stu-id="ec42a-108">The default for this feature is to be off for a cluster.</span></span> <span data-ttu-id="ec42a-109">Dessa heap minnesdumpar kan vara stora, så är det lämpligt att övervaka Blob storage-konto där de sparas när samlingen har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="ec42a-109">These heap dumps can be large, so it is advisable to monitor the Blob storage account where they are being saved once the collection has been enabled.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="ec42a-110">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="ec42a-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ec42a-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ec42a-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span> <span data-ttu-id="ec42a-112">Informationen i den här artikeln gäller bara för Windows-baserade HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ec42a-112">The information in this article only applies to Windows-based HDInsight.</span></span> <span data-ttu-id="ec42a-113">Mer information om Linux-baserat HDInsight finns [aktivera heap Dumpar för Hadoop-tjänster på Linux-baserat HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span><span class="sxs-lookup"><span data-stu-id="ec42a-113">For information on Linux-based HDInsight, see [Enable heap dumps for Hadoop services on Linux-based HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)</span></span>


## <a name="eligible-services-for-heap-dumps"></a><span data-ttu-id="ec42a-114">Tjänster för heap minnesdumpar</span><span class="sxs-lookup"><span data-stu-id="ec42a-114">Eligible services for heap dumps</span></span>
<span data-ttu-id="ec42a-115">Du kan aktivera heap Dumpar för följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="ec42a-115">You can enable heap dumps for the following services:</span></span>

* <span data-ttu-id="ec42a-116">**hcatalog** -tempelton</span><span class="sxs-lookup"><span data-stu-id="ec42a-116">**hcatalog** - tempelton</span></span>
* <span data-ttu-id="ec42a-117">**hive** -hiveserver2 metastore, derbyserver</span><span class="sxs-lookup"><span data-stu-id="ec42a-117">**hive** - hiveserver2, metastore, derbyserver</span></span>
* <span data-ttu-id="ec42a-118">**mapreduce** -jobhistoryserver</span><span class="sxs-lookup"><span data-stu-id="ec42a-118">**mapreduce** - jobhistoryserver</span></span>
* <span data-ttu-id="ec42a-119">**yarn** -resurshanteraren, nodemanager, timelineserver</span><span class="sxs-lookup"><span data-stu-id="ec42a-119">**yarn** - resourcemanager, nodemanager, timelineserver</span></span>
* <span data-ttu-id="ec42a-120">**hdfs** -datanode secondarynamenode, namenode</span><span class="sxs-lookup"><span data-stu-id="ec42a-120">**hdfs** - datanode, secondarynamenode, namenode</span></span>

## <a name="configuration-elements-that-enable-heap-dumps"></a><span data-ttu-id="ec42a-121">Konfigurationselement som möjliggör heap minnesdumpar</span><span class="sxs-lookup"><span data-stu-id="ec42a-121">Configuration elements that enable heap dumps</span></span>
<span data-ttu-id="ec42a-122">Om du vill aktivera heap Dumpar för en tjänst måste du ange lämpliga konfigurationselement i avsnittet för tjänsten, som anges av **service_name**.</span><span class="sxs-lookup"><span data-stu-id="ec42a-122">To turn on heap dumps for a service, you need to set the appropriate configuration elements in the section for that service, which is specified by **service_name**.</span></span>

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

<span data-ttu-id="ec42a-123">Värdet för **service_name** kan vara någon av tjänsterna som anges här: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resurshanteraren, nodemanager, timelineserver, datanode secondarynamenode, eller namenode.</span><span class="sxs-lookup"><span data-stu-id="ec42a-123">The value of **service_name** can be any of the services listed here: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resourcemanager, nodemanager, timelineserver, datanode, secondarynamenode, or namenode.</span></span>

## <a name="enable-using-azure-powershell"></a><span data-ttu-id="ec42a-124">Aktivera med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ec42a-124">Enable using Azure PowerShell</span></span>
<span data-ttu-id="ec42a-125">Du kan exempelvis använda följande skript om du vill aktivera heap Dumpar med hjälp av Azure PowerShell för jobhistoryserver:</span><span class="sxs-lookup"><span data-stu-id="ec42a-125">For example, to turn on heap dumps by using Azure PowerShell for jobhistoryserver, you can use the following script:</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a><span data-ttu-id="ec42a-126">Aktivera med hjälp av .NET SDK</span><span class="sxs-lookup"><span data-stu-id="ec42a-126">Enable using .NET SDK</span></span>
<span data-ttu-id="ec42a-127">Om du vill aktivera heap Dumpar med hjälp av Azure HDInsight .NET SDK för jobhistoryserver kan du till exempel använda följande kod:</span><span class="sxs-lookup"><span data-stu-id="ec42a-127">For example, to turn on heap dumps by using the Azure HDInsight .NET SDK for jobhistoryserver, you can use the following code:</span></span>

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
