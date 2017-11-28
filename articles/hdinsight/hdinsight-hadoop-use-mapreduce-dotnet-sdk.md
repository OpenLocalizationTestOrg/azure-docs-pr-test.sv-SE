---
title: aaaSubmit MapReduce-jobb med HDInsight .NET SDK - Azure | Microsoft Docs
description: "Lär dig hur toosubmit MapReduce-jobb tooAzure HDInsight Hadoop med HDInsight .NET SDK."
editor: cgronlun
manager: jhubbard
services: hdinsight
documentationcenter: 
tags: azure-portal
author: mumian
ms.assetid: c85e44b0-85fd-4185-ad1c-c34a9fe5ef44
ms.service: hdinsight
ms.custom: hdinsightactive
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: jgao
ms.openlocfilehash: d00e31400b8fa47982c31d00bfdcdb304bcb0b59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="0689f-103">Kör jobb för MapReduce med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0689f-103">Run MapReduce jobs using HDInsight .NET SDK</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="0689f-104">Lär dig hur toosubmit MapReduce-jobb med HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="0689f-104">Learn how toosubmit MapReduce jobs using HDInsight .NET SDK.</span></span> <span data-ttu-id="0689f-105">HDInsight kluster medföljer jar-filen med vissa MapReduce-exempel.</span><span class="sxs-lookup"><span data-stu-id="0689f-105">HDInsight clusters come with a jar file with some MapReduce samples.</span></span> <span data-ttu-id="0689f-106">hello jar-filen är */example/jars/hadoop-mapreduce-examples.jar*.</span><span class="sxs-lookup"><span data-stu-id="0689f-106">hello jar file is */example/jars/hadoop-mapreduce-examples.jar*.</span></span>  <span data-ttu-id="0689f-107">Ett exempel som hello är *wordcount*.</span><span class="sxs-lookup"><span data-stu-id="0689f-107">One of hello samples is *wordcount*.</span></span> <span data-ttu-id="0689f-108">Du kan utveckla en C#-konsolen programmet toosubmit ett wordcount-jobb.</span><span class="sxs-lookup"><span data-stu-id="0689f-108">You develop a C# console application toosubmit a wordcount job.</span></span>  <span data-ttu-id="0689f-109">hello jobbet läser hello */example/data/gutenberg/davinci.txt* fil och matar ut hello resultat för*/example/data/davinciwordcount*.</span><span class="sxs-lookup"><span data-stu-id="0689f-109">hello job reads hello */example/data/gutenberg/davinci.txt* file, and outputs hello results too*/example/data/davinciwordcount*.</span></span>  <span data-ttu-id="0689f-110">Om du vill toorerun hello program måste du rensa hello utdatamapp.</span><span class="sxs-lookup"><span data-stu-id="0689f-110">If you want toorerun hello application, you must clean up hello output folder.</span></span>

> [!NOTE]
> <span data-ttu-id="0689f-111">hello steg i den här artikeln måste utföras från en Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="0689f-111">hello steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="0689f-112">Använd hello flikväljaren visas på hello överkant hello artikeln för information om hur du använder en Linux-, OS X- eller Unix-klienten toowork med Hive.</span><span class="sxs-lookup"><span data-stu-id="0689f-112">For information on using a Linux, OS X, or Unix client toowork with Hive, use hello tab selector shown on hello top of hello article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="0689f-113">Krav</span><span class="sxs-lookup"><span data-stu-id="0689f-113">Prerequisites</span></span>
<span data-ttu-id="0689f-114">Innan du börjar den här artikeln, måste du ha hello följande objekt:</span><span class="sxs-lookup"><span data-stu-id="0689f-114">Before you begin this article, you must have hello following items:</span></span>

* <span data-ttu-id="0689f-115">**Ett Hadoop-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="0689f-115">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="0689f-116">Se [komma igång med Linux-baserade Hadoop i HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="0689f-116">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="0689f-117">**Visual Studio 2013/2015/2017**.</span><span class="sxs-lookup"><span data-stu-id="0689f-117">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="0689f-118">Skicka MapReduce-jobb med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="0689f-118">Submit MapReduce jobs using HDInsight .NET SDK</span></span>
<span data-ttu-id="0689f-119">Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="0689f-119">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="0689f-120">**tooSubmit jobb**</span><span class="sxs-lookup"><span data-stu-id="0689f-120">**tooSubmit jobs**</span></span>

1. <span data-ttu-id="0689f-121">Skapa ett C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="0689f-121">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="0689f-122">Kör följande kommando hello från hello Nuget Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="0689f-122">From hello Nuget Package Manager Console, run hello following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="0689f-123">Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="0689f-123">Use hello following code:</span></span>
   
        using System.Collections.Generic;
        using System.IO;
        using System.Text;
        using System.Threading;
        using Microsoft.Azure.Management.HDInsight.Job;
        using Microsoft.Azure.Management.HDInsight.Job.Models;
        using Hyak.Common;
        using Microsoft.WindowsAzure.Storage;
        using Microsoft.WindowsAzure.Storage.Blob;

        namespace SubmitHDInsightJobDotNet
        {
            class Program
            {
                private static HDInsightJobManagementClient _hdiJobManagementClient;
   
                private const string existingClusterName = "<Your HDInsight Cluster Name>";
                private const string existingClusterUri = existingClusterName + ".azurehdinsight.net";
                private const string existingClusterUsername = "<Cluster Username>";
                private const string existingClusterPassword = "<Cluster User Password>";
   
                private const string defaultStorageAccountName = "<Default Storage Account Name>"; //<StorageAccountName>.blob.core.windows.net
                private const string defaultStorageAccountKey = "<Default Storage Account Key>";
                private const string defaultStorageContainerName = "<Default Blob Container Name>";

                private const string sourceFile = "/example/data/gutenberg/davinci.txt";  
                private const string outputFolder = "/example/data/davinciwordcount";
   
                static void Main(string[] args)
                {
                    System.Console.WriteLine("hello application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER toocontinue ...");
                    System.Console.ReadLine();
                }
   
                private static void SubmitMRJob()
                {
                    List<string> args = new List<string> { { "/example/data/gutenberg/davinci.txt" }, { "/example/data/davinciwordcount" } };
   
                    var paras = new MapReduceJobSubmissionParameters
                    {
                        JarFile = @"/example/jars/hadoop-mapreduce-examples.jar",
                        JarClass = "wordcount",
                        Arguments = args
                    };
   
                    System.Console.WriteLine("Submitting hello MR job toohello cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
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
                    System.Console.WriteLine("Job output is: ");
                    var storageAccess = new AzureStorageAccess(defaultStorageAccountName, defaultStorageAccountKey,
                        defaultStorageContainerName);
        
                    if (jobDetail.ExitValue == 0)
                    {
                        // Create hello storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create hello blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference tooa previously created container.
                        CloudBlobContainer container = blobClient.GetContainerReference(defaultStorageContainerName);
        
                        CloudBlockBlob blockBlob = container.GetBlockBlobReference(outputFolder.Substring(1) + "/part-r-00000");
        
                        using (var stream = blockBlob.OpenRead())
                        {
                            using (StreamReader reader = new StreamReader(stream))
                            {
                                while (!reader.EndOfStream)
                                {
                                    System.Console.WriteLine(reader.ReadLine());
                                }
                            }
                        }
                    }
                    else
                    {
                        // fetch stderr output in case of failure
                        var output = _hdiJobManagementClient.JobManagement.GetJobErrorLogs(jobId, storageAccess); 
        
                        using (var reader = new StreamReader(output, Encoding.UTF8))
                        {
                            string value = reader.ReadToEnd();
                            System.Console.WriteLine(value);
                        }
        
                    }
                }
            }
        }
4. <span data-ttu-id="0689f-124">Tryck på **F5** toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="0689f-124">Press **F5** toorun hello application.</span></span>

<span data-ttu-id="0689f-125">toorun hello jobb igen, måste du ändra hello jobbet utdata mappnamn, i hello exemplet är det ”/ davinciwordcount-exempel/data”.</span><span class="sxs-lookup"><span data-stu-id="0689f-125">toorun hello job again, you must change hello job output folder name, in hello sample, it is "/example/data/davinciwordcount".</span></span>

<span data-ttu-id="0689f-126">När hello jobbet slutförs ordentligt, skrivs hello programmet hello innehåll hello utdatafilen ”del-r-00000”.</span><span class="sxs-lookup"><span data-stu-id="0689f-126">When hello job completes successfully, hello application prints hello content of hello output file "part-r-00000".</span></span>

## <a name="next-steps"></a><span data-ttu-id="0689f-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0689f-127">Next steps</span></span>
<span data-ttu-id="0689f-128">I den här artikeln har du lärt dig flera olika sätt toocreate ett HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0689f-128">In this article, you have learned several ways toocreate an HDInsight cluster.</span></span> <span data-ttu-id="0689f-129">toolearn se fler hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="0689f-129">toolearn more, see hello following articles:</span></span>

* <span data-ttu-id="0689f-130">För att skicka ett Hive-jobb finns [köra Hive-frågor med HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="0689f-130">For submitting a Hive job, see [Run Hive queries using HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>
* <span data-ttu-id="0689f-131">För att skapa HDInsight-kluster, se [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="0689f-131">For creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="0689f-132">För att hantera HDInsight-kluster, se [hantera Hadoop-kluster i HDInsight](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="0689f-132">For managing HDInsight clusters, see [Manage Hadoop clusters in HDInsight](hdinsight-administer-use-portal-linux.md).</span></span>
* <span data-ttu-id="0689f-133">Learning hello HDInsight .NET SDK finns [HDInsight .NET SDK referens](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="0689f-133">For learning hello HDInsight .NET SDK, see [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>
* <span data-ttu-id="0689f-134">För icke-interaktiv autentisering tooAzure, se [skapa icke-interaktiv autentisering .NET HDInsight-program](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="0689f-134">For non-interactive authenticate tooAzure, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

