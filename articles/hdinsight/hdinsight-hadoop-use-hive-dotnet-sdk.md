---
title: "aaaRun Hive-frågor med HDInsight .NET SDK - Azure | Microsoft Docs"
description: "Lär dig hur toosubmit Hadoop-jobb tooAzure HDInsight Hadoop med HDInsight .NET SDK."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: 4e291890-f8b4-426c-b5e8-d4fd512ff042
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: 11f07d90405d3e804774610e242813927df59a03
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a>Köra Hive-frågor med HDInsight .NET SDK
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

Lär dig hur toosubmit Hive-frågor med HDInsight .NET SDK. Du skriva ett C#-program toosubmit en Hive-fråga för att lista Hive-tabeller och visar hello resultat.

> [!NOTE]
> hello steg i den här artikeln måste utföras från en Windows-klient. Använd hello flikväljaren visas på hello överkant hello artikeln för information om hur du använder en Linux-, OS X- eller Unix-klienten toowork med Hive.
> 
> 

## <a name="prerequisites"></a>Krav
Innan du börjar den här artikeln, måste du ha hello följande objekt:

* **Ett Hadoop-kluster i HDInsight**. Se [komma igång med Linux-baserade Hadoop i HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-hive-queries-using-hdinsight-net-sdk"></a>Skicka Hive-frågor med HDInsight .NET SDK
Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET. 

**tooSubmit jobb**

1. Skapa ett C#-konsolprogram i Visual Studio.
2. Kör följande kommando hello från hello Nuget Package Manager-konsolen:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Använd hello följande kod:

    ```csharp
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
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
   
                private const string DefaultStorageAccountName = "<Default Storage Account Name>";
                private const string DefaultStorageAccountKey = "<Default Storage Account Key>";
                private const string DefaultStorageContainerName = "<Default Blob Container Name>";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);
   
                    SubmitHiveJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitHiveJob()
                {
                    Dictionary<string, string> defines = new Dictionary<string, string> { { "hive.execution.engine", "tez" }, { "hive.exec.reducers.max", "1" } };
                    List<string> args = new List<string> { { "argA" }, { "argB" } };
                    var parameters = new HiveJobSubmissionParameters
                    {
                        Query = "SHOW TABLES",
                        Defines = defines,
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting hello Hive job toohello cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitHiveJob(parameters);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for hello job completion ...");
   
                    // Wait for job completion
                    var jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    while (!jobDetail.Status.JobComplete)
                    {
                        Thread.Sleep(1000);
                        jobDetail = _hdiJobManagementClient.JobManagement.GetJob(jobId).JobDetail;
                    }
   
                    // Get job output
                    var storageAccess = new AzureStorageAccess(DefaultStorageAccountName, DefaultStorageAccountKey,
                        DefaultStorageContainerName);
                    var output = (jobDetail.ExitValue == 0)
                        ? _hdiJobManagementClient.JobManagement.GetJobOutput(jobId, storageAccess) // fetch stdout output in case of success
                        : _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); // fetch stderr output in case of failure
   
                    System.Console.WriteLine("Job output is: ");
   
                    using (var reader = new StreamReader(output, Encoding.UTF8))
                    {
                        string value = reader.ReadToEnd();
                        System.Console.WriteLine(value);
                    }
                }
            }
        }
    ```
4. Tryck på **F5** toorun hello program.

hello utdata från programmet hello bör likna följande:

![HDInsight Hadoop Hive jobbutdata](./media/hdinsight-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig flera olika sätt toocreate ett HDInsight-kluster. toolearn se fler hello följande artiklar:

* [Kom igång med Azure HDInsight][hdinsight-get-started]
* [Skapa Hadoop-kluster i HDInsight][hdinsight-provision]
* [Hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen](hdinsight-administer-use-management-portal.md)
* [HDInsight .NET SDK-referens](https://msdn.microsoft.com/library/mt271028.aspx)
* [Använda Pig med HDInsight](hdinsight-use-pig.md)
* [Använda Sqoop med HDInsight](hdinsight-use-sqoop-mac-linux.md)
* [Skapa .NET HDInsight-program med icke-interaktiv autentisering](hdinsight-create-non-interactive-authentication-dotnet-applications.md)

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


