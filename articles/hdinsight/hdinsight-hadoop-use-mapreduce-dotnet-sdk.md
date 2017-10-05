---
title: Skicka MapReduce-jobb med HDInsight .NET SDK - Azure | Microsoft Docs
description: "Lär dig mer om att skicka MapReduce-jobb på Azure HDInsight Hadoop med HDInsight .NET SDK."
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
ms.openlocfilehash: 015435270c31bafea0ebf5303b459338755c1410
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="122f9-103">Kör jobb för MapReduce med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="122f9-103">Run MapReduce jobs using HDInsight .NET SDK</span></span>
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="122f9-104">Lär dig mer om att skicka MapReduce-jobb med HDInsight .NET SDK.</span><span class="sxs-lookup"><span data-stu-id="122f9-104">Learn how to submit MapReduce jobs using HDInsight .NET SDK.</span></span> <span data-ttu-id="122f9-105">HDInsight kluster medföljer jar-filen med vissa MapReduce-exempel.</span><span class="sxs-lookup"><span data-stu-id="122f9-105">HDInsight clusters come with a jar file with some MapReduce samples.</span></span> <span data-ttu-id="122f9-106">Jar-filen är */example/jars/hadoop-mapreduce-examples.jar*.</span><span class="sxs-lookup"><span data-stu-id="122f9-106">The jar file is */example/jars/hadoop-mapreduce-examples.jar*.</span></span>  <span data-ttu-id="122f9-107">Ett exempel är *wordcount*.</span><span class="sxs-lookup"><span data-stu-id="122f9-107">One of the samples is *wordcount*.</span></span> <span data-ttu-id="122f9-108">Du kan utveckla en C#-konsolapp att skicka ett wordcount-jobb.</span><span class="sxs-lookup"><span data-stu-id="122f9-108">You develop a C# console application to submit a wordcount job.</span></span>  <span data-ttu-id="122f9-109">Jobbet läser den */example/data/gutenberg/davinci.txt* filen och skickar resultatet till */example/data/davinciwordcount*.</span><span class="sxs-lookup"><span data-stu-id="122f9-109">The job reads the */example/data/gutenberg/davinci.txt* file, and outputs the results to */example/data/davinciwordcount*.</span></span>  <span data-ttu-id="122f9-110">Om du vill köra programmet måste du rensa den utgående mappen.</span><span class="sxs-lookup"><span data-stu-id="122f9-110">If you want to rerun the application, you must clean up the output folder.</span></span>

> [!NOTE]
> <span data-ttu-id="122f9-111">Stegen i den här artikeln måste utföras från en Windows-klient.</span><span class="sxs-lookup"><span data-stu-id="122f9-111">The steps in this article must be performed from a Windows client.</span></span> <span data-ttu-id="122f9-112">Använd flikväljaren visas överst i artikeln för information om hur du använder en Linux-, OS X- eller Unix-klient ska fungera med Hive.</span><span class="sxs-lookup"><span data-stu-id="122f9-112">For information on using a Linux, OS X, or Unix client to work with Hive, use the tab selector shown on the top of the article.</span></span>
> 
> 

## <a name="prerequisites"></a><span data-ttu-id="122f9-113">Krav</span><span class="sxs-lookup"><span data-stu-id="122f9-113">Prerequisites</span></span>
<span data-ttu-id="122f9-114">Innan du börjar den här artikeln, måste du ha följande:</span><span class="sxs-lookup"><span data-stu-id="122f9-114">Before you begin this article, you must have the following items:</span></span>

* <span data-ttu-id="122f9-115">**Ett Hadoop-kluster i HDInsight**.</span><span class="sxs-lookup"><span data-stu-id="122f9-115">**A Hadoop cluster in HDInsight**.</span></span> <span data-ttu-id="122f9-116">Se [komma igång med Linux-baserade Hadoop i HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="122f9-116">See [Get started using Linux-based Hadoop in HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).</span></span>
* <span data-ttu-id="122f9-117">**Visual Studio 2013/2015/2017**.</span><span class="sxs-lookup"><span data-stu-id="122f9-117">**Visual Studio 2013/2015/2017**.</span></span>

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a><span data-ttu-id="122f9-118">Skicka MapReduce-jobb med HDInsight .NET SDK</span><span class="sxs-lookup"><span data-stu-id="122f9-118">Submit MapReduce jobs using HDInsight .NET SDK</span></span>
<span data-ttu-id="122f9-119">HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det lättare att arbeta med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="122f9-119">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span> 

<span data-ttu-id="122f9-120">**Skicka jobb**</span><span class="sxs-lookup"><span data-stu-id="122f9-120">**To Submit jobs**</span></span>

1. <span data-ttu-id="122f9-121">Skapa ett C#-konsolprogram i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="122f9-121">Create a C# console application in Visual Studio.</span></span>
2. <span data-ttu-id="122f9-122">Kör följande kommando från Nuget Package Manager-konsolen:</span><span class="sxs-lookup"><span data-stu-id="122f9-122">From the Nuget Package Manager Console, run the following command:</span></span>
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. <span data-ttu-id="122f9-123">Använd följande kod:</span><span class="sxs-lookup"><span data-stu-id="122f9-123">Use the following code:</span></span>
   
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
                    System.Console.WriteLine("The application is running ...");
   
                    var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = existingClusterUsername, Password = existingClusterPassword };
                    _hdiJobManagementClient = new HDInsightJobManagementClient(existingClusterUri, clusterCredentials);
   
                    SubmitMRJob();
   
                    System.Console.WriteLine("Press ENTER to continue ...");
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
   
                    System.Console.WriteLine("Submitting the MR job to the cluster...");
                    var jobResponse = _hdiJobManagementClient.JobManagement.SubmitMapReduceJob(paras);
                    var jobId = jobResponse.JobSubmissionJsonResponse.Id;
                    System.Console.WriteLine("Response status code is " + jobResponse.StatusCode);
                    System.Console.WriteLine("JobId is " + jobId);
   
                    System.Console.WriteLine("Waiting for the job completion ...");
   
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
                        // Create the storage account object
                        CloudStorageAccount storageAccount = CloudStorageAccount.Parse("DefaultEndpointsProtocol=https;AccountName=" + 
                            defaultStorageAccountName + 
                            ";AccountKey=" + defaultStorageAccountKey);
        
                        // Create the blob client.
                        CloudBlobClient blobClient = storageAccount.CreateCloudBlobClient();
        
                        // Retrieve reference to a previously created container.
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
4. <span data-ttu-id="122f9-124">Tryck på **F5** för att köra programmet.</span><span class="sxs-lookup"><span data-stu-id="122f9-124">Press **F5** to run the application.</span></span>

<span data-ttu-id="122f9-125">Du kan köra jobbet igen måste du ändra mappnamnet utdata för jobbet i det här exemplet är ”/ davinciwordcount-exempel/data”.</span><span class="sxs-lookup"><span data-stu-id="122f9-125">To run the job again, you must change the job output folder name, in the sample, it is "/example/data/davinciwordcount".</span></span>

<span data-ttu-id="122f9-126">När jobbet har slutförts, skrivs innehållet i filen ”del-r-00000” i programmet.</span><span class="sxs-lookup"><span data-stu-id="122f9-126">When the job completes successfully, the application prints the content of the output file "part-r-00000".</span></span>

## <a name="next-steps"></a><span data-ttu-id="122f9-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="122f9-127">Next steps</span></span>
<span data-ttu-id="122f9-128">I den här artikeln har du lärt dig att skapa ett HDInsight-kluster på flera olika sätt.</span><span class="sxs-lookup"><span data-stu-id="122f9-128">In this article, you have learned several ways to create an HDInsight cluster.</span></span> <span data-ttu-id="122f9-129">Mer information finns i följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="122f9-129">To learn more, see the following articles:</span></span>

* <span data-ttu-id="122f9-130">För att skicka ett Hive-jobb finns [köra Hive-frågor med HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="122f9-130">For submitting a Hive job, see [Run Hive queries using HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).</span></span>
* <span data-ttu-id="122f9-131">För att skapa HDInsight-kluster, se [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span><span class="sxs-lookup"><span data-stu-id="122f9-131">For creating HDInsight clusters, see [Create Linux-based Hadoop clusters in HDInsight](hdinsight-hadoop-provision-linux-clusters.md).</span></span>
* <span data-ttu-id="122f9-132">För att hantera HDInsight-kluster, se [hantera Hadoop-kluster i HDInsight](hdinsight-administer-use-portal-linux.md).</span><span class="sxs-lookup"><span data-stu-id="122f9-132">For managing HDInsight clusters, see [Manage Hadoop clusters in HDInsight](hdinsight-administer-use-portal-linux.md).</span></span>
* <span data-ttu-id="122f9-133">För HDInsight .NET SDK, se [HDInsight .NET SDK referens](https://msdn.microsoft.com/library/mt271028.aspx).</span><span class="sxs-lookup"><span data-stu-id="122f9-133">For learning the HDInsight .NET SDK, see [HDInsight .NET SDK reference](https://msdn.microsoft.com/library/mt271028.aspx).</span></span>
* <span data-ttu-id="122f9-134">För icke-interaktiv autentisera till Azure, se [skapa icke-interaktiv autentisering .NET HDInsight-program](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span><span class="sxs-lookup"><span data-stu-id="122f9-134">For non-interactive authenticate to Azure, see [Create non-interactive authentication .NET HDInsight applications](hdinsight-create-non-interactive-authentication-dotnet-applications.md).</span></span>

