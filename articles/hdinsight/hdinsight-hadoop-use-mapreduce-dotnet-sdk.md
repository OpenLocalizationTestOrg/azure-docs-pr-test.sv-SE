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
# <a name="run-mapreduce-jobs-using-hdinsight-net-sdk"></a>Kör jobb för MapReduce med HDInsight .NET SDK
[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

Lär dig hur toosubmit MapReduce-jobb med HDInsight .NET SDK. HDInsight kluster medföljer jar-filen med vissa MapReduce-exempel. hello jar-filen är */example/jars/hadoop-mapreduce-examples.jar*.  Ett exempel som hello är *wordcount*. Du kan utveckla en C#-konsolen programmet toosubmit ett wordcount-jobb.  hello jobbet läser hello */example/data/gutenberg/davinci.txt* fil och matar ut hello resultat för*/example/data/davinciwordcount*.  Om du vill toorerun hello program måste du rensa hello utdatamapp.

> [!NOTE]
> hello steg i den här artikeln måste utföras från en Windows-klient. Använd hello flikväljaren visas på hello överkant hello artikeln för information om hur du använder en Linux-, OS X- eller Unix-klienten toowork med Hive.
> 
> 

## <a name="prerequisites"></a>Krav
Innan du börjar den här artikeln, måste du ha hello följande objekt:

* **Ett Hadoop-kluster i HDInsight**. Se [komma igång med Linux-baserade Hadoop i HDInsight](./hdinsight-hadoop-linux-tutorial-get-started.md).
* **Visual Studio 2013/2015/2017**.

## <a name="submit-mapreduce-jobs-using-hdinsight-net-sdk"></a>Skicka MapReduce-jobb med HDInsight .NET SDK
Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET. 

**tooSubmit jobb**

1. Skapa ett C#-konsolprogram i Visual Studio.
2. Kör följande kommando hello från hello Nuget Package Manager-konsolen:
   
        Install-Package Microsoft.Azure.Management.HDInsight.Job
3. Använd hello följande kod:
   
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
4. Tryck på **F5** toorun hello program.

toorun hello jobb igen, måste du ändra hello jobbet utdata mappnamn, i hello exemplet är det ”/ davinciwordcount-exempel/data”.

När hello jobbet slutförs ordentligt, skrivs hello programmet hello innehåll hello utdatafilen ”del-r-00000”.

## <a name="next-steps"></a>Nästa steg
I den här artikeln har du lärt dig flera olika sätt toocreate ett HDInsight-kluster. toolearn se fler hello följande artiklar:

* För att skicka ett Hive-jobb finns [köra Hive-frågor med HDInsight .NET SDK](hdinsight-hadoop-use-hive-dotnet-sdk.md).
* För att skapa HDInsight-kluster, se [skapa Linux-baserade Hadoop-kluster i HDInsight](hdinsight-hadoop-provision-linux-clusters.md).
* För att hantera HDInsight-kluster, se [hantera Hadoop-kluster i HDInsight](hdinsight-administer-use-portal-linux.md).
* Learning hello HDInsight .NET SDK finns [HDInsight .NET SDK referens](https://msdn.microsoft.com/library/mt271028.aspx).
* För icke-interaktiv autentisering tooAzure, se [skapa icke-interaktiv autentisering .NET HDInsight-program](hdinsight-create-non-interactive-authentication-dotnet-applications.md).

