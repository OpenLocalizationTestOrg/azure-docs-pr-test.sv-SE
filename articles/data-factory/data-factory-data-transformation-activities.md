---
title: 'Data Transformation: Processen & Transformera data | Microsoft Docs'
description: "Lär dig hur tootransform data eller bearbeta data i Azure Data Factory med Hadoop, Machine Learning eller Azure Data Lake Analytics."
keywords: Dataomvandling av, bearbetning av data, transformera data transformation aktivitet
services: data-factory
documentationcenter: 
author: sharonlo101
manager: jhubbard
editor: monicar
ms.assetid: 39786731-1e4b-40a4-81b7-d06e127427aa
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/16/2017
ms.author: shlo
ms.openlocfilehash: 917d617259699b0e71de3a0e0c17463d00f2e0a0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="transform-data-in-azure-data-factory"></a>Transformera data i Azure Data Factory
> [!div class="op_single_selector"]
> * [Hive](data-factory-hive-activity.md)  
> * [Pig](data-factory-pig-activity.md)  
> * [MapReduce](data-factory-map-reduce.md)  
> * [Hadoop Streaming](data-factory-hadoop-streaming-activity.md)
> * [Machine Learning](data-factory-azure-ml-batch-execution-activity.md) 
> * [Lagrad procedur](data-factory-stored-proc-activity.md)
> * [Data Lake Analytics U-SQL](data-factory-usql-activity.md)
> * [Anpassade .NET](data-factory-use-custom-activities.md)

## <a name="overview"></a>Översikt
Den här artikeln förklarar data transformation aktiviteter i Azure Data Factory att du kan använda tootransform och bearbetar dina rådata i förutsägelser och insikter. En omvandling aktivitet körs i en datormiljö, till exempel Azure HDInsight-kluster eller ett Azure Batch. Det ger länkar tooarticles med detaljerad information om varje aktivitet för omvandling.

Data Factory stöder hello följande data transformation aktiviteter som kan läggas till för[pipelines](data-factory-create-pipelines.md) antingen individuellt eller härledda med en annan aktivitet.

> [!NOTE]
> En genomgång med stegvisa instruktioner finns [skapar en pipeline med Hive omvandling](data-factory-build-your-first-pipeline.md) artikel.  
> 
> 

## <a name="hdinsight-hive-activity"></a>HDInsight Hive-aktivitet
Hej HDInsight Hive aktivitet i en Data Factory-pipelinen kör Hive-frågor på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster. Se [Hive aktiviteten](data-factory-hive-activity.md) artikeln för information om den här aktiviteten. 

## <a name="hdinsight-pig-activity"></a>HDInsight Pig-aktivitet
Hej HDInsight Pig aktivitet i en Data Factory-pipelinen kör Pig frågor på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster. Se [Pig aktiviteten](data-factory-pig-activity.md) artikeln för information om den här aktiviteten. 

## <a name="hdinsight-mapreduce-activity"></a>HDInsight MapReduce activity
Hej HDInsight MapReduce aktivitet i en Data Factory-pipelinen kör MapReduce program på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster. Se [MapReduce Activity](data-factory-map-reduce.md) artikeln för information om den här aktiviteten.

## <a name="hdinsight-streaming-activity"></a>HDInsight Streaming activity
Hej HDInsight Streaming Activity i Data Factory-pipelinen kör Hadoop Streaming program på egen hand eller på begäran Windows/Linux-baserade HDInsight-kluster. Se [HDInsight Streaming activity](data-factory-hadoop-streaming-activity.md) för ytterligare information om den här aktiviteten.

## <a name="hdinsight-spark-activity"></a>HDInsight Apache Spark-aktivitet
hello HDInsight Spark-aktivitet i en Data Factory-pipelinen körs Spark-program på din egen HDInsight-kluster. Mer information finns i [anropa Spark-program från Azure Data Factory](data-factory-spark.md). 

## <a name="machine-learning-activities"></a>Machine Learning-aktiviteter
Azure Data Factory aktiverar du tooeasily skapa pipelines som använder en publicerad Azure Machine Learning-webbtjänsten för förutsägelseanalys. Med hjälp av hello [Batchkörningsaktivitet](data-factory-azure-ml-batch-execution-activity.md#invoking-a-web-service-using-batch-execution-activity) i Azure Data Factory-pipelinen, du kan anropa en Machine Learning web service toomake förutsägelser på hello data i en batch.

Över tiden ange hello förutsägelsemodeller i hello Machine Learning bedömningsprofil experiment måste toobe retrained med nya datauppsättningar. När du är klar med omtränings vill du tooupdate hello bedömningen webbtjänst med hello retrained Machine Learning-modellen. Du kan använda hello [Uppdateringsresursaktivitet](data-factory-azure-ml-batch-execution-activity.md#updating-models-using-update-resource-activity) tooupdate hello-webbtjänst med hello nyligen tränade modellen.  

Se [används Machine Learning aktiviteter](data-factory-azure-ml-batch-execution-activity.md) mer information om dessa Machine Learning-aktiviteter. 

## <a name="stored-procedure-activity"></a>Aktiviteten lagrad procedur
Du kan använda hello lagrade proceduren för SQL Server-aktivitet i en Data Factory-pipelinen tooinvoke en lagrad procedur i någon av följande datalager hello: Azure SQL Database, Azure SQL Data Warehouse, SQL Server-databas i ditt företag eller en Azure VM. Se [lagrade Proceduraktiviteten](data-factory-stored-proc-activity.md) artikeln för information.  

## <a name="data-lake-analytics-u-sql-activity"></a>U-SQL-aktivitet för Data Lake Analytics
Data Lake Analytics U-SQL-aktivitet körs ett U-SQL-skript i ett Azure Data Lake Analytics-kluster. Se [Data Analytics U-SQL-aktivitet](data-factory-usql-activity.md) artikeln för information. 

## <a name="net-custom-activity"></a>.NET-anpassad aktivitet
Om du behöver tootransform data på ett sätt som inte stöds av Data Factory kan du skapa en anpassad aktivitet med din egen databearbetning logik och använder hello aktivitet i hello pipeline. Du kan konfigurera hello anpassade .NET aktiviteten toorun med hjälp av Azure Batch-tjänsten eller ett Azure HDInsight-kluster. Se [använda anpassade aktiviteter](data-factory-use-custom-activities.md) artikeln för information. 

Du kan skapa en anpassad aktivitet toorun R-skript på ditt HDInsight-kluster med R installerat. Se [Run R Script using Azure Data Factory](https://github.com/Azure/Azure-DataFactory/tree/master/Samples/RunRScriptUsingADFSample) (Köra R-skript med Azure Data Factory). 

## <a name="compute-environments"></a>Compute-miljöer
Du skapar en länkad tjänst för hello beräkning miljö och sedan använda hello länkade tjänsten när du definierar en transformation-aktivitet. Det finns två typer av beräknings-miljöer som stöds av Data Factory. 

1. **På begäran**: I det här fallet hello datormiljö hanteras helt av Data Factory. Det automatiskt skapas av hello Data Factory-tjänsten innan ett jobb har skickats tooprocess data och tas bort när hello jobbet har slutförts. Du kan konfigurera och styra detaljerade inställningar för hello på begäran beräknings-miljö för jobbkörningen klusterhantering och startprogram åtgärder. 
2. **Ta med din egen**: I det här fallet kan du registrera din egen datormiljö (till exempel HDInsight-kluster) som en länkad tjänst i Data Factory. hello datormiljö hanteras av dig och hello Data Factory-tjänsten använder den tooexecute hello aktiviteter. 

Se [Compute länkade tjänster](data-factory-compute-linked-services.md) artikel toolearn om compute-tjänster som stöds av Data Factory. 

## <a name="summary"></a>Sammanfattning
Azure Data Factory stöder hello följande data transformation aktiviteter och hello beräkning miljöer för hello aktiviteter. hello omvandling aktiviteter kan vara tillagda toopipelines antingen individuellt eller härledda med en annan aktivitet.

| Datatransformeringsaktivitet | Compute-miljö |
|:--- |:--- |
| [Hive](data-factory-hive-activity.md) |HDInsight [Hadoop] |
| [Pig](data-factory-pig-activity.md) |HDInsight [Hadoop] |
| [MapReduce](data-factory-map-reduce.md) |HDInsight [Hadoop] |
| [Hadoop Streaming](data-factory-hadoop-streaming-activity.md) |HDInsight [Hadoop] |
| [Machine Learning-aktiviteter: batchkörning och resursuppdatering](data-factory-azure-ml-batch-execution-activity.md) |Azure VM |
| [Lagrad procedur](data-factory-stored-proc-activity.md) |Azure SQL, Azure SQL Data Warehouse eller SQL Server |
| [Data Lake Analytics U-SQL](data-factory-usql-activity.md) |Azure Data Lake Analytics |
| [DotNet](data-factory-use-custom-activities.md) |HDInsight [Hadoop] eller Azure Batch |

