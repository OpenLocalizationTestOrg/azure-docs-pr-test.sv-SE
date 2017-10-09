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
# <a name="run-hive-queries-using-hdinsight-net-sdk"></a><span data-ttu-id="776d0-103">Köra Hive-frågor med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="776d0-103">Run Hive queries using HDInsight .NET SDK</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="776d0-104">Lär dig hur toosubmit Hive-frågor med HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="776d0-104">Learn how toosubmit Hive queries using HDInsight .NET SDK.</span></span> <span data-ttu-id="776d0-105">Du skriva ett C#-program toosubmit en Hive-fråga för att lista Hive-tabeller och visar hello resultat.</span><span class="sxs-lookup"><span data-stu-id="776d0-105">You write a C# program toosubmit a Hive query for listing Hive tables, and display hello results.</span></span>

> [!NOTE]
> <span data-ttu-id="776d0-106">hello steg i den här artikeln måste utföras från en Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="776d0-106">hello steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="776d0-107">Använd hello flikväljaren visas på hello överkant hello artikeln för information om hur du använder en Linux-, OS X- eller Unix-klienten toowork med Hive.</span><span class="sxs-lookup"><span data-stu-id="776d0-107">For information on using a Linux, OS X, or Unix client toowork with Hive, use hello tab selector shown on hello top of hello article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="776d0-108">Krav</span><span class="sxs-lookup"><span data-stu-id="776d0-108">Prerequisites</span></span>
<span data-ttu-id="776d0-109">Innan du börjar den här artikeln, måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="776d0-109">Before you begin this article, you must have hello following items:</span></span>

* <span data-ttu-id="776d0-110">**Ett Hadoop-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="776d0-110">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="776d0-111">Se [komma igång med Linux-baserade Hadoop i HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="776d0-111">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="776d0-112">**Visual Studio 2013/2015/2017**.</span><span class="sxs-lookup"><span data-stu-id="776d0-112">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-hive-queries-using-hdinsight-net-sdk"></a><span data-ttu-id="776d0-113">Skicka Hive-frågor med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="776d0-113">Submit Hive queries using HDInsight .NET SDK</span></span>
<span data-ttu-id="776d0-114">Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="776d0-114">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="776d0-115">**tooSubmit jobb**</span><span class="sxs-lookup"><span data-stu-id="776d0-115">**tooSubmit jobs**</span></span>

1. <span data-ttu-id="776d0-116">Skapa ett C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="776d0-116">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="776d0-117">Kör följande kommando hello från hello Nuget Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="776d0-117">From hello Nuget Package Manager Console, run hello following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="776d0-118">Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="776d0-118">Use hello following code:</span></span>

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
4. <span data-ttu-id="776d0-119">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="776d0-119">Press **F5** toorun hello application.</span></span>

<span data-ttu-id="776d0-120">hello utdata från programmet hello bör likna följande:</span><span class="sxs-lookup"><span data-stu-id="776d0-120">hello output of hello application shall be similar to:</span></span>

![HDInsight Hadoop Hive jobbutdata](./media/hdinsight-hadoop-use-hive-dotnet-sdk/hdinsight-hadoop-use-hive-net-sdk-output.png)

## <a name="next-steps"></a><span data-ttu-id="776d0-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="776d0-122">Next steps</span></span>
<span data-ttu-id="776d0-123">I den här artikeln har du lärt dig flera olika sätt toocreate ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="776d0-123">In this article, you have learned several ways toocreate an HDInsight cluster.</span></span> <span data-ttu-id="776d0-124">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="776d0-124">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="776d0-125">[Kom igång med Azure HDInsight][hdinsight-get-started]</span><span class="sxs-lookup"><span data-stu-id="776d0-125">[Get started with Azure HDInsight][hdinsight-get-started]</span></span>
* <span data-ttu-id="776d0-126">[Skapa Hadoop-kluster i HDInsight][hdinsight-provision]</span><span class="sxs-lookup"><span data-stu-id="776d0-126">[Create Hadoop clusters in HDInsight][hdinsight-provision]</span></span>
* [<span data-ttu-id="776d0-127">Hantera Hadoop-kluster i HDInsight med hjälp av hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="776d0-127">Manage Hadoop clusters in HDInsight by using hello Azure portal</span></span>](hdinsight-administer-use-management-portal.md)
* [<span data-ttu-id="776d0-128">HDInsight .NET SDK-referens</span><span class="sxs-lookup"><span data-stu-id="776d0-128">HDInsight .NET SDK reference</span></span>](https://msdn.microsoft.com/library/mt271028.aspx)
* [<span data-ttu-id="776d0-129">Använda Pig med HDInsight</span><span class="sxs-lookup"><span data-stu-id="776d0-129">Use Pig with HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="776d0-130">Använda Sqoop med HDInsight</span><span class="sxs-lookup"><span data-stu-id="776d0-130">Use Sqoop with HDInsight</span></span>](hdinsight-use-sqoop-mac-linux.md)
* [<span data-ttu-id="776d0-131">Skapa .NET HDInsight-program med icke-interaktiv autentisering</span><span class="sxs-lookup"><span data-stu-id="776d0-131">Create non-interactive authentication .NET HDInsight applications</span></span>](hdinsight-create-non-interactive-authentication-dotnet-applications.md)

[hdinsight-provision]: hdinsight-hadoop-provision-linux-clusters.md
[hdinsight-get-started]: hdinsight-hadoop-linux-tutorial-get-started.md


