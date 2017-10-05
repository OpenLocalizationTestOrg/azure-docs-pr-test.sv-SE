---
title: "Använda Pig med Hadoop med PowerShell i HDInsight - Azure | Microsoft Docs"
description: "Lär dig mer om att skicka Pig-jobb till ett Hadoop-kluster i HDInsight med hjälp av Azure PowerShell."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 737089c1-b494-4387-9def-7b4dac3be532
ms.service: hdinsight
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.custom: H1Hack27Feb2017,hdinsightactive
ms.openlocfilehash: 28904b07609ffb40a8195278fd1afd3957896733
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-azure-powershell-to-run-pig-jobs-with-hdinsight"></a><span data-ttu-id="76b1d-103">Kör jobb för Pig med HDInsight med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="76b1d-103">Use Azure PowerShell to run Pig jobs with HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="76b1d-104">Det här dokumentet innehåller ett exempel på hur Azure PowerShell för att skicka Pig-jobb till en Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="76b1d-104">This document provides an example of using Azure PowerShell to submit Pig jobs to a Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="76b1d-105">Pig kan du skriva MapReduce-jobb med hjälp av ett språk (Pig Latin) som modeller Datatransformationer i stället för att mappa och minska funktioner.</span><span class="sxs-lookup"><span data-stu-id="76b1d-105">Pig allows you to write MapReduce jobs by using a language (Pig Latin) that models data transformations, rather than map and reduce functions.</span></span>

> [!NOTE]
> <span data-ttu-id="76b1d-106">Det här dokumentet ger inte en detaljerad beskrivning av vad de Pig Latin-rapporterna som används i exemplen göra.</span><span class="sxs-lookup"><span data-stu-id="76b1d-106">This document does not provide a detailed description of what the Pig Latin statements used in the examples do.</span></span> <span data-ttu-id="76b1d-107">Information om den Pig Latin som används i det här exemplet finns [använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="76b1d-107">For information about the Pig Latin used in this example, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

## <span data-ttu-id="76b1d-108"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="76b1d-108"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="76b1d-109">**Ett Azure HDInsight-kluster**</span><span class="sxs-lookup"><span data-stu-id="76b1d-109">**An Azure HDInsight cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="76b1d-110">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="76b1d-110">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="76b1d-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="76b1d-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="76b1d-112">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="76b1d-112">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <span data-ttu-id="76b1d-113"><a id="powershell"></a>Köra Pig-jobb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="76b1d-113"><a id="powershell"></a>Run Pig jobs using PowerShell</span></span>

<span data-ttu-id="76b1d-114">Azure PowerShell innehåller *cmdlets* som gör det möjligt att köra Pig-jobb via fjärranslutning på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="76b1d-114">Azure PowerShell provides *cmdlets* that allow you to remotely run Pig jobs on HDInsight.</span></span> <span data-ttu-id="76b1d-115">Internt, PowerShell använder REST-anrop till [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) körs på HDInsight-klustret.</span><span class="sxs-lookup"><span data-stu-id="76b1d-115">Internally, PowerShell uses REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) running on the HDInsight cluster.</span></span>

<span data-ttu-id="76b1d-116">Följande cmdlets används när Pig-jobb som körs på en fjärransluten HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="76b1d-116">The following cmdlets are used when running Pig jobs on a remote HDInsight cluster:</span></span>

* <span data-ttu-id="76b1d-117">**Login-AzureRmAccount**: autentiserar Azure PowerShell för att din Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="76b1d-117">**Login-AzureRmAccount**: Authenticates Azure PowerShell to your Azure Subscription</span></span>
* <span data-ttu-id="76b1d-118">**Nya AzureRmHDInsightPigJobDefinition**: skapar ett *jobbet definition* med hjälp av de angivna Pig Latin-instruktionerna</span><span class="sxs-lookup"><span data-stu-id="76b1d-118">**New-AzureRmHDInsightPigJobDefinition**: Creates a *job definition* by using the specified Pig Latin statements</span></span>
* <span data-ttu-id="76b1d-119">**Start-AzureRmHDInsightJob**: skickar jobbdefinitionen till HDInsight, startar jobbet och returnerar ett *jobbet* objekt som kan användas för att kontrollera status för jobbet</span><span class="sxs-lookup"><span data-stu-id="76b1d-119">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job</span></span>
* <span data-ttu-id="76b1d-120">**Vänta AzureRmHDInsightJob**: använder jobbobjektet för att kontrollera status för jobbet.</span><span class="sxs-lookup"><span data-stu-id="76b1d-120">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="76b1d-121">Den väntar tills jobbet har slutförts eller väntetiden har överskridits.</span><span class="sxs-lookup"><span data-stu-id="76b1d-121">It waits until the job has completed, or the wait time has been exceeded.</span></span>
* <span data-ttu-id="76b1d-122">**Get-AzureRmHDInsightJobOutput**: används för att hämta utdata för jobbet</span><span class="sxs-lookup"><span data-stu-id="76b1d-122">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job</span></span>

<span data-ttu-id="76b1d-123">Följande steg visar hur du använder dessa cmdlets för att köra ett jobb på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="76b1d-123">The following steps demonstrate how to use these cmdlets to run a job on your HDInsight cluster.</span></span>

1. <span data-ttu-id="76b1d-124">Med hjälp av en redigerare, spara följande kod som **pigjob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="76b1d-124">Using an editor, save the following code as **pigjob.ps1**.</span></span>

    <span data-ttu-id="76b1d-125">[!code-powershell[huvudsakliga](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]</span><span class="sxs-lookup"><span data-stu-id="76b1d-125">[!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]</span></span>

1. <span data-ttu-id="76b1d-126">Öppna en ny Windows PowerShell-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="76b1d-126">Open a new Windows PowerShell command prompt.</span></span> <span data-ttu-id="76b1d-127">Ändra kataloger till platsen för den **pigjob.ps1** filen och sedan använder du följande kommando för att köra skriptet:</span><span class="sxs-lookup"><span data-stu-id="76b1d-127">Change directories to the location of the **pigjob.ps1** file, then use the following command to run the script:</span></span>

        .\pigjob.ps1

    <span data-ttu-id="76b1d-128">Du uppmanas att logga in på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="76b1d-128">You are prompted to log in to your Azure subscription.</span></span> <span data-ttu-id="76b1d-129">Du uppmanas sedan HTTPs/Admin kontonamn och lösenord för HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="76b1d-129">Then, you are asked for the HTTPs/Admin account name and password for the HDInsight cluster.</span></span>

2. <span data-ttu-id="76b1d-130">När jobbet är slutfört, bör den returnera information som liknar följande text:</span><span class="sxs-lookup"><span data-stu-id="76b1d-130">When the job completes, it should return information similar to the following text:</span></span>

        Start the Pig job ...
        Wait for the Pig job to complete ...
        Display the standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="76b1d-131"><a id="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="76b1d-131"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="76b1d-132">Om ingen information returneras när jobbet är slutfört, har ett fel inträffat under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="76b1d-132">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="76b1d-133">Om du vill visa information om fel för det här jobbet att lägga till följande kommando i slutet av den **pigjob.ps1** filen, spara den och kör det igen.</span><span class="sxs-lookup"><span data-stu-id="76b1d-133">To view error information for this job, add the following command to the end of the **pigjob.ps1** file, save it, and then run it again.</span></span>

    # Print the output of the Pig job.
    Write-Host "Display the standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

<span data-ttu-id="76b1d-134">Detta returnerar informationen som har skrivits till STDERR på servern när du körde jobbet och det kan hjälpa att avgöra varför jobbet inte.</span><span class="sxs-lookup"><span data-stu-id="76b1d-134">This returns the information that was written to STDERR on the server when you ran the job, and it may help determine why the job is failing.</span></span>

## <span data-ttu-id="76b1d-135"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="76b1d-135"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="76b1d-136">Du kan se ger Azure PowerShell ett enkelt sätt att köra Pig-jobb på ett HDInsight-kluster, övervaka jobbstatus och hämta utdata.</span><span class="sxs-lookup"><span data-stu-id="76b1d-136">As you can see, Azure PowerShell provides an easy way to run Pig jobs on an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <span data-ttu-id="76b1d-137"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="76b1d-137"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="76b1d-138">Allmän information om Pig i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="76b1d-138">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="76b1d-139">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="76b1d-139">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="76b1d-140">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="76b1d-140">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="76b1d-141">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="76b1d-141">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="76b1d-142">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="76b1d-142">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
