---
title: "Köra Apache Pig-jobb med .NET SDK för Hadoop - Azure HDInsight | Microsoft Docs"
description: "Lär dig hur du använder .NET SDK för Hadoop för att skicka Pig-jobb till Hadoop i HDInsight."
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
ms.openlocfilehash: e40d152821b36852c447d5a3adfd39114edbbace
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="run-pig-jobs-using-the-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="c50db-103">Köra Pig-jobb med hjälp av .NET SDK för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c50db-103">Run Pig jobs using the .NET SDK for Hadoop in HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="c50db-104">Lär dig hur du använder .NET SDK för Hadoop för att skicka Apache Pig-jobb till Hadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="c50db-104">Learn how to use the .NET SDK for Hadoop to submit Apache Pig jobs to Hadoop on Azure HDInsight.</span></span>

<span data-ttu-id="c50db-105">HDInsight .NET SDK innehåller .NET-klientbibliotek som gör det enklare att arbeta med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="c50db-105">The HDInsight .NET SDK provides .NET client libraries that makes it easier to work with HDInsight clusters from .NET.</span></span> <span data-ttu-id="c50db-106">Pig kan du skapa MapReduce åtgärder genom att modellera en serie Datatransformationer.</span><span class="sxs-lookup"><span data-stu-id="c50db-106">Pig allows you to create MapReduce operations by modeling a series of data transformations.</span></span> <span data-ttu-id="c50db-107">Lär dig hur du använder ett grundläggande C#-program för att skicka Pig-jobbet till ett HDInsight-kluster i det här dokumentet.</span><span class="sxs-lookup"><span data-stu-id="c50db-107">In this document, you learn how to use a basic C# application to submit a Pig job to an HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="c50db-108">Krav</span><span class="sxs-lookup"><span data-stu-id="c50db-108">Prerequisites</span></span>

<span data-ttu-id="c50db-109">Du behöver följande för att slutföra stegen i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="c50db-109">To complete the steps in this article, you need the following.</span></span>

* <span data-ttu-id="c50db-110">Ett Azure HDInsight (Hadoop på HDInsight)-kluster (antingen Windows eller Linux-baserade).</span><span class="sxs-lookup"><span data-stu-id="c50db-110">An Azure HDInsight (Hadoop on HDInsight) cluster (either Windows or Linux-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="c50db-111">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="c50db-111">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="c50db-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="c50db-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="c50db-113">Visual Studio 2012, 2013 eller 2015 2017.</span><span class="sxs-lookup"><span data-stu-id="c50db-113">Visual Studio 2012, 2013, 2015 or 2017.</span></span>

## <a name="create-the-application"></a><span data-ttu-id="c50db-114">Skapa programmet</span><span class="sxs-lookup"><span data-stu-id="c50db-114">Create the application</span></span>

<span data-ttu-id="c50db-115">HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det lättare att arbeta med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="c50db-115">The HDInsight .NET SDK provides .NET client libraries, which makes it easier to work with HDInsight clusters from .NET.</span></span>

1. <span data-ttu-id="c50db-116">Från den **filen** -menyn i Visual Studio, markera **ny** och välj sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="c50db-116">From the **File** menu in Visual Studio, select **New** and then select **Project**.</span></span>

2. <span data-ttu-id="c50db-117">Ange eller Välj följande värden för det nya projektet:</span><span class="sxs-lookup"><span data-stu-id="c50db-117">For the new project, type or select the following values:</span></span>

   | <span data-ttu-id="c50db-118">Egenskap</span><span class="sxs-lookup"><span data-stu-id="c50db-118">Property</span></span> | <span data-ttu-id="c50db-119">Värde</span><span class="sxs-lookup"><span data-stu-id="c50db-119">Value</span></span> |
   | ------ | ------ |
   | <span data-ttu-id="c50db-120">Kategori</span><span class="sxs-lookup"><span data-stu-id="c50db-120">Category</span></span> | <span data-ttu-id="c50db-121">Mallar/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="c50db-121">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="c50db-122">Mall</span><span class="sxs-lookup"><span data-stu-id="c50db-122">Template</span></span> | <span data-ttu-id="c50db-123">Konsolprogram</span><span class="sxs-lookup"><span data-stu-id="c50db-123">Console Application</span></span> |
   | <span data-ttu-id="c50db-124">Namn</span><span class="sxs-lookup"><span data-stu-id="c50db-124">Name</span></span> | <span data-ttu-id="c50db-125">SubmitPigJob</span><span class="sxs-lookup"><span data-stu-id="c50db-125">SubmitPigJob</span></span> |

3. <span data-ttu-id="c50db-126">Klicka på **OK** för att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="c50db-126">Click **OK** to create the project.</span></span>

4. <span data-ttu-id="c50db-127">Från den **verktyg** väljer du **Library Package Manager** eller **Nuget Package Manager**, och välj sedan **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="c50db-127">From the **Tools** menu, select **Library Package Manager** or **Nuget Package Manager**, and then select **Package Manager Console**.</span></span>

5. <span data-ttu-id="c50db-128">För att installera .NET SDK-paket, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c50db-128">To install the .NET SDK packages, use the following command:</span></span>

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. <span data-ttu-id="c50db-129">Dubbelklicka på Solution Explorer **Program.cs** att öppna den.</span><span class="sxs-lookup"><span data-stu-id="c50db-129">From Solution Explorer, double-click **Program.cs** to open it.</span></span> <span data-ttu-id="c50db-130">Ersätt den befintliga koden med följande.</span><span class="sxs-lookup"><span data-stu-id="c50db-130">Replace the existing code with the following.</span></span>

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
                System.Console.WriteLine("The application is running ...");

                var clusterCredentials = new BasicAuthenticationCloudCredentials { Username = ExistingClusterUsername, Password = ExistingClusterPassword };
                _hdiJobManagementClient = new HDInsightJobManagementClient(ExistingClusterUri, clusterCredentials);

                SubmitPigJob();

                System.Console.WriteLine("Press ENTER to continue ...");
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

                System.Console.WriteLine("Submitting the Pig job to the cluster...");
                var response = _hdiJobManagementClient.JobManagement.SubmitPigJob(parameters);
                System.Console.WriteLine("Validating that the response is as expected...");
                System.Console.WriteLine("Response status code is " + response.StatusCode);
                System.Console.WriteLine("Validating the response object...");
                System.Console.WriteLine("JobId is " + response.JobSubmissionJsonResponse.Id);
            }
        }
    }
    ```

7. <span data-ttu-id="c50db-131">Du kan börja programmet **F5**.</span><span class="sxs-lookup"><span data-stu-id="c50db-131">To start the application, press **F5**.</span></span>

8. <span data-ttu-id="c50db-132">Om du vill avsluta programmet trycker du på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="c50db-132">To exit the application, press **ENTER**.</span></span>

## <a name="summary"></a><span data-ttu-id="c50db-133">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="c50db-133">Summary</span></span>

<span data-ttu-id="c50db-134">Som du ser kan du skapa .NET-program som skickar Pig-jobb till ett HDInsight-kluster och övervaka jobbstatusen .NET SDK för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="c50db-134">As you can see, the .NET SDK for Hadoop allows you to create .NET applications that submit Pig jobs to an HDInsight cluster, and monitor the job status.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c50db-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c50db-135">Next steps</span></span>

<span data-ttu-id="c50db-136">Information om Pig i HDInsight finns [använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="c50db-136">For information on Pig in HDInsight, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

<span data-ttu-id="c50db-137">Mer information om hur du använder Hadoop i HDInsight finns i följande dokument:</span><span class="sxs-lookup"><span data-stu-id="c50db-137">For more information on using Hadoop on HDInsight, see the following documents:</span></span>

* [<span data-ttu-id="c50db-138">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c50db-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="c50db-139">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="c50db-139">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
