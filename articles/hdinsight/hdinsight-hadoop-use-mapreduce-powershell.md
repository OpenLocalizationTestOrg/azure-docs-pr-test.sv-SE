---
title: aaaUse MapReduce och PowerShell med Hadoop - Azure HDInsight | Microsoft Docs
description: "Lär dig hur toouse PowerShell tooremotely kör MapReduce-jobb med Hadoop på HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 21b56d32-1785-4d44-8ae8-94467c12cfba
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 59524f0e8813d4c017f92bccb2e50d4c018acf71
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-mapreduce-jobs-with-hadoop-on-hdinsight-using-powershell"></a><span data-ttu-id="ceb73-103">Kör jobb för MapReduce med Hadoop i HDInsight med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="ceb73-103">Run MapReduce jobs with Hadoop on HDInsight using PowerShell</span></span>

[!INCLUDE [mapreduce-selector](../../includes/hdinsight-selector-use-mapreduce.md)]

<span data-ttu-id="ceb73-104">Det här dokumentet innehåller ett exempel på hur Azure PowerShell toorun ett MapReduce-jobb i en Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ceb73-104">This document provides an example of using Azure PowerShell toorun a MapReduce job in a Hadoop on HDInsight cluster.</span></span>

## <span data-ttu-id="ceb73-105"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="ceb73-105"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="ceb73-106">**Ett kluster i Azure HDInsight (Hadoop på HDInsight)**</span><span class="sxs-lookup"><span data-stu-id="ceb73-106">**An Azure HDInsight (Hadoop on HDInsight) cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="ceb73-107">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="ceb73-107">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="ceb73-108">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="ceb73-108">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="ceb73-109">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="ceb73-109">**A workstation with Azure PowerShell**.</span></span>

## <span data-ttu-id="ceb73-110"><a id="powershell"></a>Kör ett MapReduce-jobb med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="ceb73-110"><a id="powershell"></a>Run a MapReduce job using Azure PowerShell</span></span>

<span data-ttu-id="ceb73-111">Azure PowerShell innehåller *cmdlets* som gör det möjligt tooremotely kör MapReduce-jobb i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="ceb73-111">Azure PowerShell provides *cmdlets* that allow you tooremotely run MapReduce jobs on HDInsight.</span></span> <span data-ttu-id="ceb73-112">Internt, detta kan åstadkommas med hjälp av REST-anrop för[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (kallades Templeton) körs på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ceb73-112">Internally, this is accomplished by using REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) (formerly called Templeton) running on hello HDInsight cluster.</span></span>

<span data-ttu-id="ceb73-113">hello används följande cmdletar när du kör MapReduce-jobb i en fjärransluten HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ceb73-113">hello following cmdlets are used when running MapReduce jobs in a remote HDInsight cluster.</span></span>

* <span data-ttu-id="ceb73-114">**Login-AzureRmAccount**: autentiserar Azure PowerShell tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ceb73-114">**Login-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure subscription.</span></span>

* <span data-ttu-id="ceb73-115">**Nya AzureRmHDInsightMapReduceJobDefinition**: skapar en ny *jobbet definition* anges med hjälp av hello MapReduce information.</span><span class="sxs-lookup"><span data-stu-id="ceb73-115">**New-AzureRmHDInsightMapReduceJobDefinition**: Creates a new *job definition* by using hello specified MapReduce information.</span></span>

* <span data-ttu-id="ceb73-116">**Start-AzureRmHDInsightJob**: skickar hello jobbet definition tooHDInsight, startar hello jobb och returnerar ett *jobbet* objekt som kan vara används toocheck hello status för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="ceb73-116">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job.</span></span>

* <span data-ttu-id="ceb73-117">**Vänta AzureRmHDInsightJob**: använder hello objektet toocheck hello jobbstatus för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="ceb73-117">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="ceb73-118">Den väntar tills hello jobbet slutförs eller hello väntetiden har överskridits.</span><span class="sxs-lookup"><span data-stu-id="ceb73-118">It waits until hello job completes or hello wait time is exceeded.</span></span>

* <span data-ttu-id="ceb73-119">**Get-AzureRmHDInsightJobOutput**: används tooretrieve hello utdata för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="ceb73-119">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job.</span></span>

<span data-ttu-id="ceb73-120">hello följande steg visar hur toouse dessa cmdlets toorun ett jobb i HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="ceb73-120">hello following steps demonstrate how toouse these cmdlets toorun a job in your HDInsight cluster.</span></span>

1. <span data-ttu-id="ceb73-121">Med hjälp av en redigerare, spara hello efter koden som **mapreducejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="ceb73-121">Using an editor, save hello following code as **mapreducejob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-mapreduce/use-mapreduce.ps1?range=5-69)]

2. <span data-ttu-id="ceb73-122">Öppna ett nytt **Azure PowerShell** kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ceb73-122">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="ceb73-123">Ändra kataloger toohello placeringen hello **mapreducejob.ps1** filen och sedan använda hello följande kommandoskript toorun hello:</span><span class="sxs-lookup"><span data-stu-id="ceb73-123">Change directories toohello location of hello **mapreducejob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\mapreducejob.ps1

    <span data-ttu-id="ceb73-124">När du kör skriptet hello efterfrågas hello namnet på hello HDInsight-kluster och hello HTTPS/Admin kontonamn och lösenord för hello-kluster.</span><span class="sxs-lookup"><span data-stu-id="ceb73-124">When you run hello script, you are prompted for hello name of hello HDInsight cluster and hello HTTPS/Admin account name and password for hello cluster.</span></span> <span data-ttu-id="ceb73-125">Du kanske också ange tooauthenticate tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ceb73-125">You may also be prompted tooauthenticate tooyour Azure subscription.</span></span>

3. <span data-ttu-id="ceb73-126">När hello jobbet är slutfört, visas utdata liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="ceb73-126">When hello job completes, you receive output similar toohello following text:</span></span>

        Cluster         : CLUSTERNAME
        ExitCode        : 0
        Name            : wordcount
        PercentComplete : map 100% reduce 100%
        Query           :
        State           : Completed
        StatusDirectory : f1ed2028-afe8-402f-a24b-13cc17858097
        SubmissionTime  : 12/5/2014 8:34:09 PM
        JobId           : job_1415949758166_0071

    <span data-ttu-id="ceb73-127">Den här utdata visar hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="ceb73-127">This output indicates that hello job completed successfully.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ceb73-128">Om hello **ExitCode** är ett värde än 0, se [felsökning](#troubleshooting).</span><span class="sxs-lookup"><span data-stu-id="ceb73-128">If hello **ExitCode** is a value other than 0, see [Troubleshooting](#troubleshooting).</span></span>

    <span data-ttu-id="ceb73-129">Det här exemplet lagras också hello hämtade filer tooan **output.txt** fil i hello kör du hello skript från.</span><span class="sxs-lookup"><span data-stu-id="ceb73-129">This example also stores hello downloaded files tooan **output.txt** file in hello directory that you run hello script from.</span></span>

### <a name="view-output"></a><span data-ttu-id="ceb73-130">Visa utdata</span><span class="sxs-lookup"><span data-stu-id="ceb73-130">View output</span></span>

<span data-ttu-id="ceb73-131">Öppna hello **output.txt** fil i en text editor toosee hello ord och räknar producerade av hello jobb.</span><span class="sxs-lookup"><span data-stu-id="ceb73-131">Open hello **output.txt** file in a text editor toosee hello words and counts produced by hello job.</span></span>

> [!NOTE]
> <span data-ttu-id="ceb73-132">hello utdatafilerna till ett MapReduce-jobb är oföränderliga.</span><span class="sxs-lookup"><span data-stu-id="ceb73-132">hello output files of a MapReduce job are immutable.</span></span> <span data-ttu-id="ceb73-133">Om du kör det här exemplet måste så toochange hello namnet på utdatafilen hello.</span><span class="sxs-lookup"><span data-stu-id="ceb73-133">So if you rerun this sample, you need toochange hello name of hello output file.</span></span>

## <span data-ttu-id="ceb73-134"><a id="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="ceb73-134"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="ceb73-135">Om ingen information returneras när hello jobbet är slutfört, har ett fel inträffat under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="ceb73-135">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="ceb73-136">tooview felinformation för det här jobbet kan lägga till hello efter kommandot toohello hello **mapreducejob.ps1** filen, spara den och kör det igen.</span><span class="sxs-lookup"><span data-stu-id="ceb73-136">tooview error information for this job, add hello following command toohello end of hello **mapreducejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print hello output of hello WordCount job.
Write-Host "Display hello standard output ..." -ForegroundColor Green
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $wordCountJob.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="ceb73-137">Denna cmdlet returnerar hello information som har skrivits tooSTDERR på hello server när du körde hello jobbet och det kan hjälpa att avgöra varför hello jobb som misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ceb73-137">This cmdlet returns hello information that was written tooSTDERR on hello server when you ran hello job, and it may help determine why hello job is failing.</span></span>

## <span data-ttu-id="ceb73-138"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="ceb73-138"><a id="summary"></a>Summary</span></span>

<span data-ttu-id="ceb73-139">Som du kan se tillhandahåller Azure PowerShell ett enkelt sätt toorun MapReduce-jobb på ett HDInsight-kluster och jobbstatus för övervakaren hello hämta hello utdata.</span><span class="sxs-lookup"><span data-stu-id="ceb73-139">As you can see, Azure PowerShell provides an easy way toorun MapReduce jobs on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="ceb73-140"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ceb73-140"><a id="nextsteps"></a>Next steps</span></span>

<span data-ttu-id="ceb73-141">Allmän information om MapReduce-jobb i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ceb73-141">For general information about MapReduce jobs in HDInsight:</span></span>

* [<span data-ttu-id="ceb73-142">Använda MapReduce på Hadoop och HDInsight</span><span class="sxs-lookup"><span data-stu-id="ceb73-142">Use MapReduce on HDInsight Hadoop</span></span>](hdinsight-use-mapreduce.md)

<span data-ttu-id="ceb73-143">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="ceb73-143">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="ceb73-144">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ceb73-144">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="ceb73-145">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="ceb73-145">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
