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
# <a name="collect-heap-dumps-in-blob-storage-toodebug-and-analyze-hadoop-services"></a>Samla in heap Dumpar i Blob storage toodebug och analysera Hadoop-tjänster
[!INCLUDE [heapdump-selector](../../includes/hdinsight-selector-heap-dump.md)]

Heap Dumpar innehåller en ögonblicksbild av hello programmets minne, inklusive hello värdena för variabler som för närvarande hello hello dump har skapats. Så att de är användbara för att diagnostisera problem som uppstår vid körning. Heap minnesdumpar kan samlas in för Hadoop-tjänster och placeras inuti hello Azure Blob storage-konto för en användare under HDInsightHeapDumps automatiskt /.

hello samling heap Dumpar för olika tjänster måste vara aktiverat för tjänster på enskilda kluster. hello standardvärdet för den här funktionen är toobe ut för ett kluster. Dessa heap minnesdumpar kan vara stora, så det är lämpligt toomonitor hello Blob storage-konto där de sparas när hello samling har aktiverats.

> [!IMPORTANT]
> Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement). hello information i den här artikeln gäller bara tooWindows-baserade HDInsight. Mer information om Linux-baserat HDInsight finns [aktivera heap Dumpar för Hadoop-tjänster på Linux-baserat HDInsight](hdinsight-hadoop-collect-debug-heap-dump-linux.md)


## <a name="eligible-services-for-heap-dumps"></a>Tjänster för heap minnesdumpar
Du kan aktivera heap Dumpar för hello följande tjänster:

* **hcatalog** -tempelton
* **hive** -hiveserver2 metastore, derbyserver
* **mapreduce** -jobhistoryserver
* **yarn** -resurshanteraren, nodemanager, timelineserver
* **hdfs** -datanode secondarynamenode, namenode

## <a name="configuration-elements-that-enable-heap-dumps"></a>Konfigurationselement som möjliggör heap minnesdumpar
tooturn på heap Dumpar för en tjänst behöver du tooset hello lämpliga konfigurationselement i hello-avsnittet för tjänsten, som anges av **service_name**.

    "javaargs.<service_name>.XX:+HeapDumpOnOutOfMemoryError" = "-XX:+HeapDumpOnOutOfMemoryError",
    "javaargs.<service_name>.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\Dumps\<service_name>_%date:~4,2%%date:~7,2%%date:~10,2%%time:~0,2%%time:~3,2%%time:~6,2%.hprof"

Hej värdet för **service_name** kan vara någon av hello-tjänster som listas här: tempelton, hiveserver2, metastore, derbyserver, jobhistoryserver, resurshanteraren, nodemanager, timelineserver, datanode secondarynamenode, eller namenode.

## <a name="enable-using-azure-powershell"></a>Aktivera med hjälp av Azure PowerShell
Till exempel tooturn på heap Dumpar med hjälp av Azure PowerShell för jobhistoryserver du kan använda hello följande skript:

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

    $MapRedConfigValues = new-object 'Microsoft.WindowsAzure.Management.HDInsight.Cmdlet.DataObjects.AzureHDInsightMapReduceConfiguration'

    $MapRedConfigValues.Configuration = @{ "javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError"="-XX:+HeapDumpOnOutOfMemoryError" ; "javaargs.jobhistoryserver.XX:HeapDumpPath" = "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof" }

## <a name="enable-using-net-sdk"></a>Aktivera med hjälp av .NET SDK
Till exempel tooturn på heap Dumpar med hello Azure HDInsight .NET SDK för jobhistoryserver du kan använda hello följande kod:

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:+HeapDumpOnOutOfMemoryError", "-XX:+HeapDumpOnOutOfMemoryError"));

    clusterInfo.MapReduceConfiguration.ConfigurationCollection.Add(new KeyValuePair<string, string>("javaargs.jobhistoryserver.XX:HeapDumpPath", "-XX:HeapDumpPath=c:\\Dumps\\jobhistoryserver_%date:~4,2%_%date:~7,2%_%date:~10,2%_%time:~0,2%_%time:~3,2%_%time:~6,2%.hprof"));
