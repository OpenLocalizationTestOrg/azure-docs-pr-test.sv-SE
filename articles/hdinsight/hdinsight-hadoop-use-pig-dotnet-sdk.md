---
title: "aaaRun Apache Pig-jobb med .NET SDK för Hadoop - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur toouse hello .NET SDK för Hadoop toosubmit Pig-jobb tooHadoop på HDInsight."
services: hdinsight
documentationcenter: .net
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: fa11d49a-328c-47e7-b16d-e7ed2a453195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/15/2017
ms.author: larryfr
ms.openlocfilehash: 1d4ceebd7c168372d23fe29a088f04676686de30
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-pig-jobs-using-hello-net-sdk-for-hadoop-in-hdinsight"></a>Köra Pig-jobb med hjälp av hello .NET SDK för Hadoop i HDInsight

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

Lär dig hur toouse hello .NET SDK för Hadoop toosubmit Apache Pig jobb tooHadoop på Azure HDInsight.

Hej HDInsight .NET SDK innehåller .NET-klientbibliotek som gör det enklare toowork med HDInsight-kluster från .NET. Pig tillåter toocreate MapReduce åtgärder genom att modellera en serie Datatransformationer. I detta dokument kan du lära dig hur toosubmit toouse en grundläggande C#-program en Pig jobbet tooan HDInsight-kluster.

## <a name="prerequisites"></a>Krav

toocomplete hello stegen i den här artikeln, behöver du hello följande.

* Ett Azure HDInsight (Hadoop på HDInsight)-kluster (antingen Windows eller Linux-baserade).

  > [!IMPORTANT]
  > Linux är hello endast operativsystem på HDInsight version 3.4 eller senare. Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).

* Visual Studio 2012, 2013 eller 2015 2017.

## <a name="create-hello-application"></a>Skapa hello program

Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET.

1. Från hello **filen** -menyn i Visual Studio, markera **ny** och välj sedan **projekt**.

2. För hello nya projekt värden typ eller välj hello följande:

   | Egenskap | Värde |
   | ------ | ------ |
   | Kategori | Mallar/Visual C#/Windows |
   | Mall | Konsolprogram |
   | Namn | SubmitPigJob |

3. Klicka på **OK** toocreate hello projektet.

4. Från hello **verktyg** väljer du **Library Package Manager** eller **Nuget Package Manager**, och välj sedan **Pakethanterarkonsolen**.

5. tooinstall hello .NET SDK-paket, Använd hello följande kommando:

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. Dubbelklicka på Solution Explorer **Program.cs** tooopen den. Ersätt hello befintlig kod med hello följande.

    ```csharp
    using Microsoft.Azure.Management.HDInsight.Job;
    using Microsoft.Azure.Management.HDInsight.Job.Models;
    using Hyak.Common;

    namespace SubmitHDInsightJobDotNet
    {
        class Program
        {
            private static HDInsightJobManagementClient _hdiJobManagementClient;

            private const string ExistingClusterName = "<Your HDInsight Cluster Name>";
            private const string ExistingClusterUri = ExistingClusterName + ".azurehdinsight.net";
            private const string ExistingClusterUsername = "<Cluster Username>";
            private const string ExistingClusterPassword = "<Cluster User Password>";

            static void Main(string[] args)
            {
                System.Console.WriteLine("hello application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER toocontinue ...");
                System.Console.ReadLine();
            }

            private static void SubmitPigJob()
            {
                var parameters = new PigJobSubmissionParameters
                {
                    Query = @"LOGS = LOAD '/example/data/sample.log';
                                LEVELS = foreach LOGS generate REGEX_EXTRACT($0, '(TRACE|DEBUG|INFO|WARN|ERROR|FATAL)', 1)  as LOGLEVEL;
                                FILTEREDLEVELS = FILTER LEVELS by LOGLEVEL is not null;
                                GROUPEDLEVELS = GROUP FILTEREDLEVELS by LOGLEVEL;
                                FREQUENCIES = foreach GROUPEDLEVELS generate group as LOGLEVEL, COUNT(FILTEREDLEVELS.LOGLEVEL) as COUNT;
                                RESULT = order FREQUENCIES by COUNT desc;
                                DUMP RESULT;"
                };

                System.Console.WriteLine("Submitting hello Pig job toohello cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that hello response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating hello response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. toostart hello program, tryck på **F5**.

8. tooexit hello program, tryck på **RETUR**.

## <a name="summary"></a>Sammanfattning

Som du ser kan hello .NET SDK för Hadoop toocreate .NET-program som skicka Pig-jobb tooan HDInsight-kluster och övervaka hello jobbstatus.

## <a name="next-steps"></a>Nästa steg

Information om Pig i HDInsight finns [använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md).

Mer information om hur du använder Hadoop i HDInsight finns i hello följande dokument:

* [Använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md)
* [Använda MapReduce med Hadoop i HDInsight](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
