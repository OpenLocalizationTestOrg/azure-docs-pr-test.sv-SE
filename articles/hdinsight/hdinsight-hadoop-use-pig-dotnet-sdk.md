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
# <a name="run-pig-jobs-using-hello-net-sdk-for-hadoop-in-hdinsight"></a><span data-ttu-id="0f57e-103">Köra Pig-jobb med hjälp av hello .NET SDK för Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f57e-103">Run Pig jobs using hello .NET SDK for Hadoop in HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="0f57e-104">Lär dig hur toouse hello .NET SDK för Hadoop toosubmit Apache Pig jobb tooHadoop på Azure HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0f57e-104">Learn how toouse hello .NET SDK for Hadoop toosubmit Apache Pig jobs tooHadoop on Azure HDInsight.</span></span>

<span data-ttu-id="0f57e-105">Hej HDInsight .NET SDK innehåller .NET-klientbibliotek som gör det enklare toowork med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="0f57e-105">hello HDInsight .NET SDK provides .NET client libraries that makes it easier toowork with HDInsight clusters from .NET.</span></span> <span data-ttu-id="0f57e-106">Pig tillåter toocreate MapReduce åtgärder genom att modellera en serie Datatransformationer.</span><span class="sxs-lookup"><span data-stu-id="0f57e-106">Pig allows you toocreate MapReduce operations by modeling a series of data transformations.</span></span> <span data-ttu-id="0f57e-107">I detta dokument kan du lära dig hur toosubmit toouse en grundläggande C#-program en Pig jobbet tooan HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0f57e-107">In this document, you learn how toouse a basic C# application toosubmit a Pig job tooan HDInsight cluster.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0f57e-108">Krav</span><span class="sxs-lookup"><span data-stu-id="0f57e-108">Prerequisites</span></span>

<span data-ttu-id="0f57e-109">toocomplete hello stegen i den här artikeln, behöver du hello följande.</span><span class="sxs-lookup"><span data-stu-id="0f57e-109">toocomplete hello steps in this article, you need hello following.</span></span>

* <span data-ttu-id="0f57e-110">Ett Azure HDInsight (Hadoop på HDInsight)-kluster (antingen Windows eller Linux-baserade).</span><span class="sxs-lookup"><span data-stu-id="0f57e-110">An Azure HDInsight (Hadoop on HDInsight) cluster (either Windows or Linux-based).</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="0f57e-111">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0f57e-111">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0f57e-112">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0f57e-112">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="0f57e-113">Visual Studio 2012, 2013 eller 2015 2017.</span><span class="sxs-lookup"><span data-stu-id="0f57e-113">Visual Studio 2012, 2013, 2015 or 2017.</span></span>

## <a name="create-hello-application"></a><span data-ttu-id="0f57e-114">Skapa hello program</span><span class="sxs-lookup"><span data-stu-id="0f57e-114">Create hello application</span></span>

<span data-ttu-id="0f57e-115">Hej HDInsight .NET SDK innehåller klientbibliotek för .NET, vilket gör det enklare toowork med HDInsight-kluster från .NET.</span><span class="sxs-lookup"><span data-stu-id="0f57e-115">hello HDInsight .NET SDK provides .NET client libraries, which makes it easier toowork with HDInsight clusters from .NET.</span></span>

1. <span data-ttu-id="0f57e-116">Från hello **filen** -menyn i Visual Studio, markera **ny** och välj sedan **projekt**.</span><span class="sxs-lookup"><span data-stu-id="0f57e-116">From hello **File** menu in Visual Studio, select **New** and then select **Project**.</span></span>

2. <span data-ttu-id="0f57e-117">För hello nya projekt värden typ eller välj hello följande:</span><span class="sxs-lookup"><span data-stu-id="0f57e-117">For hello new project, type or select hello following values:</span></span>

   | <span data-ttu-id="0f57e-118">Egenskap</span><span class="sxs-lookup"><span data-stu-id="0f57e-118">Property</span></span> | <span data-ttu-id="0f57e-119">Värde</span><span class="sxs-lookup"><span data-stu-id="0f57e-119">Value</span></span> |
   | ------ | ------ |
   | <span data-ttu-id="0f57e-120">Kategori</span><span class="sxs-lookup"><span data-stu-id="0f57e-120">Category</span></span> | <span data-ttu-id="0f57e-121">Mallar/Visual C#/Windows</span><span class="sxs-lookup"><span data-stu-id="0f57e-121">Templates/Visual C#/Windows</span></span> |
   | <span data-ttu-id="0f57e-122">Mall</span><span class="sxs-lookup"><span data-stu-id="0f57e-122">Template</span></span> | <span data-ttu-id="0f57e-123">Konsolprogram</span><span class="sxs-lookup"><span data-stu-id="0f57e-123">Console Application</span></span> |
   | <span data-ttu-id="0f57e-124">Namn</span><span class="sxs-lookup"><span data-stu-id="0f57e-124">Name</span></span> | <span data-ttu-id="0f57e-125">SubmitPigJob</span><span class="sxs-lookup"><span data-stu-id="0f57e-125">SubmitPigJob</span></span> |

3. <span data-ttu-id="0f57e-126">Klicka på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="0f57e-126">Click **OK** toocreate hello project.</span></span>

4. <span data-ttu-id="0f57e-127">Från hello **verktyg** väljer du **Library Package Manager** eller **Nuget Package Manager**, och välj sedan **Pakethanterarkonsolen**.</span><span class="sxs-lookup"><span data-stu-id="0f57e-127">From hello **Tools** menu, select **Library Package Manager** or **Nuget Package Manager**, and then select **Package Manager Console**.</span></span>

5. <span data-ttu-id="0f57e-128">tooinstall hello .NET SDK-paket, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="0f57e-128">tooinstall hello .NET SDK packages, use hello following command:</span></span>

        Install-Package Microsoft.Azure.Management.HDInsight.Job

6. <span data-ttu-id="0f57e-129">Dubbelklicka på Solution Explorer **Program.cs** tooopen den.</span><span class="sxs-lookup"><span data-stu-id="0f57e-129">From Solution Explorer, double-click **Program.cs** tooopen it.</span></span> <span data-ttu-id="0f57e-130">Ersätt hello befintlig kod med hello följande.</span><span class="sxs-lookup"><span data-stu-id="0f57e-130">Replace hello existing code with hello following.</span></span>

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

7. <span data-ttu-id="0f57e-131">toostart hello program, tryck på **F5**.</span><span class="sxs-lookup"><span data-stu-id="0f57e-131">toostart hello application, press **F5**.</span></span>

8. <span data-ttu-id="0f57e-132">tooexit hello program, tryck på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="0f57e-132">tooexit hello application, press **ENTER**.</span></span>

## <a name="summary"></a><span data-ttu-id="0f57e-133">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0f57e-133">Summary</span></span>

<span data-ttu-id="0f57e-134">Som du ser kan hello .NET SDK för Hadoop toocreate .NET-program som skicka Pig-jobb tooan HDInsight-kluster och övervaka hello jobbstatus.</span><span class="sxs-lookup"><span data-stu-id="0f57e-134">As you can see, hello .NET SDK for Hadoop allows you toocreate .NET applications that submit Pig jobs tooan HDInsight cluster, and monitor hello job status.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0f57e-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0f57e-135">Next steps</span></span>

<span data-ttu-id="0f57e-136">Information om Pig i HDInsight finns [använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="0f57e-136">For information on Pig in HDInsight, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

<span data-ttu-id="0f57e-137">Mer information om hur du använder Hadoop i HDInsight finns i hello följande dokument:</span><span class="sxs-lookup"><span data-stu-id="0f57e-137">For more information on using Hadoop on HDInsight, see hello following documents:</span></span>

* [<span data-ttu-id="0f57e-138">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f57e-138">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="0f57e-139">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0f57e-139">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)

[preview-portal]: https://portal.azure.com/
