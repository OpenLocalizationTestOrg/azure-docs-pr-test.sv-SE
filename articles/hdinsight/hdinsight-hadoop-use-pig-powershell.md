---
title: aaaUse Hadoop Pig med PowerShell i HDInsight - Azure | Microsoft Docs
description: "Lär dig hur toosubmit Pig jobb tooa Hadoop-kluster i HDInsight med hjälp av Azure PowerShell."
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
ms.openlocfilehash: 771617df203011eaec715a0dba6f5014a42877f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-azure-powershell-toorun-pig-jobs-with-hdinsight"></a><span data-ttu-id="3804b-103">Använda Azure PowerShell toorun Pig-jobb med HDInsight</span><span class="sxs-lookup"><span data-stu-id="3804b-103">Use Azure PowerShell toorun Pig jobs with HDInsight</span></span>

[!INCLUDE [pig-selector](../../includes/hdinsight-selector-use-pig.md)]

<span data-ttu-id="3804b-104">Det här dokumentet innehåller ett exempel på hur Azure PowerShell toosubmit Pig jobb tooa Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3804b-104">This document provides an example of using Azure PowerShell toosubmit Pig jobs tooa Hadoop on HDInsight cluster.</span></span> <span data-ttu-id="3804b-105">Pig kan du toowrite MapReduce-jobb med hjälp av språk (Pig Latin) som modeller Datatransformationer, snarare än mappa och minska funktioner.</span><span class="sxs-lookup"><span data-stu-id="3804b-105">Pig allows you toowrite MapReduce jobs by using a language (Pig Latin) that models data transformations, rather than map and reduce functions.</span></span>

> [!NOTE]
> <span data-ttu-id="3804b-106">Det här dokumentet ger inte en detaljerad beskrivning av vad hello Pig Latin-uttryck som används i hello exempel göra.</span><span class="sxs-lookup"><span data-stu-id="3804b-106">This document does not provide a detailed description of what hello Pig Latin statements used in hello examples do.</span></span> <span data-ttu-id="3804b-107">Information om hello Pig Latin används i det här exemplet finns [använda Pig med Hadoop i HDInsight](hdinsight-use-pig.md).</span><span class="sxs-lookup"><span data-stu-id="3804b-107">For information about hello Pig Latin used in this example, see [Use Pig with Hadoop on HDInsight](hdinsight-use-pig.md).</span></span>

## <span data-ttu-id="3804b-108"><a id="prereq"></a>Förhandskrav</span><span class="sxs-lookup"><span data-stu-id="3804b-108"><a id="prereq"></a>Prerequisites</span></span>

* <span data-ttu-id="3804b-109">**Ett Azure HDInsight-kluster**</span><span class="sxs-lookup"><span data-stu-id="3804b-109">**An Azure HDInsight cluster**</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="3804b-110">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="3804b-110">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="3804b-111">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="3804b-111">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="3804b-112">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="3804b-112">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <span data-ttu-id="3804b-113"><a id="powershell"></a>Köra Pig-jobb med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="3804b-113"><a id="powershell"></a>Run Pig jobs using PowerShell</span></span>

<span data-ttu-id="3804b-114">Azure PowerShell innehåller *cmdlets* som gör det möjligt tooremotely köra Pig-jobb i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="3804b-114">Azure PowerShell provides *cmdlets* that allow you tooremotely run Pig jobs on HDInsight.</span></span> <span data-ttu-id="3804b-115">Internt, PowerShell använder REST-anrop för[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) körs på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3804b-115">Internally, PowerShell uses REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) running on hello HDInsight cluster.</span></span>

<span data-ttu-id="3804b-116">hello används följande cmdletar när Pig-jobb som körs på en fjärransluten HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="3804b-116">hello following cmdlets are used when running Pig jobs on a remote HDInsight cluster:</span></span>

* <span data-ttu-id="3804b-117">**Login-AzureRmAccount**: autentiserar Azure PowerShell tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="3804b-117">**Login-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure Subscription</span></span>
* <span data-ttu-id="3804b-118">**Nya AzureRmHDInsightPigJobDefinition**: skapar ett *jobbet definition* anges med hjälp av hello Pig Latin-instruktioner</span><span class="sxs-lookup"><span data-stu-id="3804b-118">**New-AzureRmHDInsightPigJobDefinition**: Creates a *job definition* by using hello specified Pig Latin statements</span></span>
* <span data-ttu-id="3804b-119">**Start-AzureRmHDInsightJob**: skickar hello jobbet definition tooHDInsight, startar hello jobb och returnerar ett *jobbet* objekt som kan vara används toocheck hello status för hello jobb</span><span class="sxs-lookup"><span data-stu-id="3804b-119">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job</span></span>
* <span data-ttu-id="3804b-120">**Vänta AzureRmHDInsightJob**: använder hello objektet toocheck hello jobbstatus för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="3804b-120">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="3804b-121">Den väntar tills hello jobbet har slutförts eller hello väntetiden har överskridits.</span><span class="sxs-lookup"><span data-stu-id="3804b-121">It waits until hello job has completed, or hello wait time has been exceeded.</span></span>
* <span data-ttu-id="3804b-122">**Get-AzureRmHDInsightJobOutput**: används tooretrieve hello utdata för hello jobb</span><span class="sxs-lookup"><span data-stu-id="3804b-122">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job</span></span>

<span data-ttu-id="3804b-123">hello följande steg visar hur toouse dessa cmdlets toorun ett jobb på ditt HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3804b-123">hello following steps demonstrate how toouse these cmdlets toorun a job on your HDInsight cluster.</span></span>

1. <span data-ttu-id="3804b-124">Med hjälp av en redigerare, spara hello efter koden som **pigjob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="3804b-124">Using an editor, save hello following code as **pigjob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-pig/use-pig.ps1?range=5-51)]

1. <span data-ttu-id="3804b-125">Öppna en ny Windows PowerShell-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="3804b-125">Open a new Windows PowerShell command prompt.</span></span> <span data-ttu-id="3804b-126">Ändra kataloger toohello placeringen hello **pigjob.ps1** filen och sedan använda hello följande kommandoskript toorun hello:</span><span class="sxs-lookup"><span data-stu-id="3804b-126">Change directories toohello location of hello **pigjob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\pigjob.ps1

    <span data-ttu-id="3804b-127">Du kan ange toolog i tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="3804b-127">You are prompted toolog in tooyour Azure subscription.</span></span> <span data-ttu-id="3804b-128">Du uppmanas sedan hello HTTPs/Admin kontonamn och lösenord för hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="3804b-128">Then, you are asked for hello HTTPs/Admin account name and password for hello HDInsight cluster.</span></span>

2. <span data-ttu-id="3804b-129">När hello jobbet har slutförts, måste den returnera information liknande toohello följande text:</span><span class="sxs-lookup"><span data-stu-id="3804b-129">When hello job completes, it should return information similar toohello following text:</span></span>

        Start hello Pig job ...
        Wait for hello Pig job toocomplete ...
        Display hello standard output ...
        (TRACE,816)
        (DEBUG,434)
        (INFO,96)
        (WARN,11)
        (ERROR,6)
        (FATAL,2)

## <span data-ttu-id="3804b-130"><a id="troubleshooting"></a>Felsökning</span><span class="sxs-lookup"><span data-stu-id="3804b-130"><a id="troubleshooting"></a>Troubleshooting</span></span>

<span data-ttu-id="3804b-131">Om ingen information returneras när hello jobbet är slutfört, har ett fel inträffat under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="3804b-131">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="3804b-132">tooview felinformation för det här jobbet kan lägga till hello efter kommandot toohello hello **pigjob.ps1** filen, spara den och kör det igen.</span><span class="sxs-lookup"><span data-stu-id="3804b-132">tooview error information for this job, add hello following command toohello end of hello **pigjob.ps1** file, save it, and then run it again.</span></span>

    # Print hello output of hello Pig job.
    Write-Host "Display hello standard error output ..." -ForegroundColor Green
    Get-AzureRmHDInsightJobOutput `
            -Clustername $clusterName `
            -JobId $pigJob.JobId `
            -HttpCredential $creds `
            -DisplayOutputType StandardError

<span data-ttu-id="3804b-133">Hello information som har skrivits tooSTDERR på hello server när du körde hello jobbet returneras och det kan hjälpa att avgöra varför hello jobb som misslyckas.</span><span class="sxs-lookup"><span data-stu-id="3804b-133">This returns hello information that was written tooSTDERR on hello server when you ran hello job, and it may help determine why hello job is failing.</span></span>

## <span data-ttu-id="3804b-134"><a id="summary"></a>Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3804b-134"><a id="summary"></a>Summary</span></span>
<span data-ttu-id="3804b-135">Som du kan se tillhandahåller Azure PowerShell ett enkelt sätt toorun Pig-jobb på ett HDInsight-kluster och jobbstatus för övervakaren hello hämta hello utdata.</span><span class="sxs-lookup"><span data-stu-id="3804b-135">As you can see, Azure PowerShell provides an easy way toorun Pig jobs on an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <span data-ttu-id="3804b-136"><a id="nextsteps"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3804b-136"><a id="nextsteps"></a>Next steps</span></span>
<span data-ttu-id="3804b-137">Allmän information om Pig i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="3804b-137">For general information about Pig in HDInsight:</span></span>

* [<span data-ttu-id="3804b-138">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3804b-138">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)

<span data-ttu-id="3804b-139">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="3804b-139">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="3804b-140">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3804b-140">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)
* [<span data-ttu-id="3804b-141">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="3804b-141">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
