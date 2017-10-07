---
title: aaaUse Hadoop Hive med PowerShell i HDInsight - Azure | Microsoft Docs
description: "Använd PowerShell toorun Hive-frågor i Hadoop i HDInsight."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: cb795b7c-bcd0-497a-a7f0-8ed18ef49195
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/16/2017
ms.author: larryfr
ms.openlocfilehash: 9e0b72a25c5b12431f837b1a34a63ecc06223528
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="run-hive-queries-using-powershell"></a><span data-ttu-id="0c2f4-103">Köra Hive-frågor med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c2f4-103">Run Hive queries using PowerShell</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="0c2f4-104">Det här dokumentet innehåller ett exempel på hur Azure PowerShell i hello Azure-resursgrupp läge toorun Hive-frågor i en Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-104">This document provides an example of using Azure PowerShell in hello Azure Resource Group mode toorun Hive queries in a Hadoop on HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="0c2f4-105">Det här dokumentet ger inte en detaljerad beskrivning av vad hello HiveQL-instruktioner som används i hello exempel göra.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-105">This document does not provide a detailed description of what hello HiveQL statements that are used in hello examples do.</span></span> <span data-ttu-id="0c2f4-106">Mer information om hello HiveQL som används i det här exemplet finns [använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="0c2f4-106">For information on hello HiveQL that is used in this example, see [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="0c2f4-107">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="0c2f4-107">**Prerequisites**</span></span>

* <span data-ttu-id="0c2f4-108">**Ett Azure HDInsight-kluster**: Det spelar ingen roll om hello klustret är Windows eller Linux-baserade.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-108">**An Azure HDInsight cluster**: It does not matter whether hello cluster is Windows or Linux-based.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="0c2f4-109">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-109">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="0c2f4-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="0c2f4-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="0c2f4-111">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-111">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a><span data-ttu-id="0c2f4-112">Köra Hive-frågor med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="0c2f4-112">Run Hive queries using Azure PowerShell</span></span>

<span data-ttu-id="0c2f4-113">Azure PowerShell innehåller *cmdlets* som gör det möjligt tooremotely kör Hive-frågor i HDInsight.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-113">Azure PowerShell provides *cmdlets* that allow you tooremotely run Hive queries on HDInsight.</span></span> <span data-ttu-id="0c2f4-114">Internt hello cmdlets gör REST-anrop för[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) på hello HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-114">Internally, hello cmdlets make REST calls too[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on hello HDInsight cluster.</span></span>

<span data-ttu-id="0c2f4-115">hello används följande cmdletar när du kör Hive-frågor i en fjärransluten HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-115">hello following cmdlets are used when running Hive queries in a remote HDInsight cluster:</span></span>

* <span data-ttu-id="0c2f4-116">**Lägg till AzureRmAccount**: autentiserar Azure PowerShell tooyour Azure-prenumeration</span><span class="sxs-lookup"><span data-stu-id="0c2f4-116">**Add-AzureRmAccount**: Authenticates Azure PowerShell tooyour Azure subscription</span></span>
* <span data-ttu-id="0c2f4-117">**Nya AzureRmHDInsightHiveJobDefinition**: skapar ett *jobbet definition* anges med hjälp av hello HiveQL-instruktioner</span><span class="sxs-lookup"><span data-stu-id="0c2f4-117">**New-AzureRmHDInsightHiveJobDefinition**: Creates a *job definition* by using hello specified HiveQL statements</span></span>
* <span data-ttu-id="0c2f4-118">**Start-AzureRmHDInsightJob**: skickar hello jobbet definition tooHDInsight, startar hello jobb och returnerar ett *jobbet* objekt som kan vara används toocheck hello status för hello jobb</span><span class="sxs-lookup"><span data-stu-id="0c2f4-118">**Start-AzureRmHDInsightJob**: Sends hello job definition tooHDInsight, starts hello job, and returns a *job* object that can be used toocheck hello status of hello job</span></span>
* <span data-ttu-id="0c2f4-119">**Vänta AzureRmHDInsightJob**: använder hello objektet toocheck hello jobbstatus för hello jobb.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-119">**Wait-AzureRmHDInsightJob**: Uses hello job object toocheck hello status of hello job.</span></span> <span data-ttu-id="0c2f4-120">Den väntar tills hello jobbet slutförs eller hello väntetiden har överskridits.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-120">It waits until hello job completes or hello wait time is exceeded.</span></span>
* <span data-ttu-id="0c2f4-121">**Get-AzureRmHDInsightJobOutput**: används tooretrieve hello utdata för hello jobb</span><span class="sxs-lookup"><span data-stu-id="0c2f4-121">**Get-AzureRmHDInsightJobOutput**: Used tooretrieve hello output of hello job</span></span>
* <span data-ttu-id="0c2f4-122">**Anropa AzureRmHDInsightHiveJob**: används toorun HiveQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-122">**Invoke-AzureRmHDInsightHiveJob**: Used toorun HiveQL statements.</span></span> <span data-ttu-id="0c2f4-123">Den här cmdlet block hello frågan har slutförts, och returnerar sedan hello resultat</span><span class="sxs-lookup"><span data-stu-id="0c2f4-123">This cmdlet blocks hello query completes, then returns hello results</span></span>
* <span data-ttu-id="0c2f4-124">**Använd AzureRmHDInsightCluster**: Anger hello aktuella klustret toouse för hello **Invoke-AzureRmHDInsightHiveJob** kommando</span><span class="sxs-lookup"><span data-stu-id="0c2f4-124">**Use-AzureRmHDInsightCluster**: Sets hello current cluster toouse for hello **Invoke-AzureRmHDInsightHiveJob** command</span></span>

<span data-ttu-id="0c2f4-125">hello följande steg visar hur toouse dessa cmdlets toorun ett jobb i HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-125">hello following steps demonstrate how toouse these cmdlets toorun a job in your HDInsight cluster:</span></span>

1. <span data-ttu-id="0c2f4-126">Med hjälp av en redigerare, spara hello efter koden som **hivejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-126">Using an editor, save hello following code as **hivejob.ps1**.</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]

2. <span data-ttu-id="0c2f4-127">Öppna ett nytt **Azure PowerShell** kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-127">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="0c2f4-128">Ändra kataloger toohello placeringen hello **hivejob.ps1** filen och sedan använda hello följande kommandoskript toorun hello:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-128">Change directories toohello location of hello **hivejob.ps1** file, then use hello following command toorun hello script:</span></span>

        .\hivejob.ps1

    <span data-ttu-id="0c2f4-129">När hello skriptet körs, är du tillfrågas tooenter hello klustrets namn och hello HTTPS/Admin autentiseringsuppgifter för hello klustret.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-129">When hello script runs, you are prompted tooenter hello cluster name and hello HTTPS/Admin account credentials for hello cluster.</span></span> <span data-ttu-id="0c2f4-130">Du kanske också ange toolog i tooyour Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-130">You may also be prompted toolog in tooyour Azure subscription.</span></span>

3. <span data-ttu-id="0c2f4-131">När hello jobbet är slutfört, returnerar information liknande toohello följande thext:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-131">When hello job completes, it returns information similar toohello following thext:</span></span>

        Display hello standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. <span data-ttu-id="0c2f4-132">Som tidigare nämnts **Invoke-Hive** kan använda toorun en fråga och vänta på hello svar.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-132">As mentioned earlier, **Invoke-Hive** can be used toorun a query and wait for hello response.</span></span> <span data-ttu-id="0c2f4-133">Använd följande skript toosee hur Invoke-Hive fungerar hello:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-133">Use hello following script toosee how Invoke-Hive works:</span></span>

    [!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]

    <span data-ttu-id="0c2f4-134">hello utdata ser ut så hello följande text:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-134">hello output looks like hello following text:</span></span>

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > <span data-ttu-id="0c2f4-135">Du kan använda hello Azure PowerShell för längre HiveQL frågor **här strängar** cmdlet eller HiveQL skriptfiler.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-135">For longer HiveQL queries, you can use hello Azure PowerShell **Here-Strings** cmdlet or HiveQL script files.</span></span> <span data-ttu-id="0c2f4-136">hello följande fragment visas hur toouse hello **Invoke-Hive** cmdlet toorun en HiveQL-skriptfil.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-136">hello following snippet shows how toouse hello **Invoke-Hive** cmdlet toorun a HiveQL script file.</span></span> <span data-ttu-id="0c2f4-137">Hej HiveQL skriptfilen måste vara överförs toowasb: / /.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-137">hello HiveQL script file must be uploaded toowasb://.</span></span>
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > <span data-ttu-id="0c2f4-138">Mer information om **här strängar**, se <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">med hjälp av Windows PowerShell här-strängar</a>.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-138">For more information about **Here-Strings**, see <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell Here-Strings</a>.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="0c2f4-139">Felsökning</span><span class="sxs-lookup"><span data-stu-id="0c2f4-139">Troubleshooting</span></span>

<span data-ttu-id="0c2f4-140">Om ingen information returneras när hello jobbet är slutfört, har ett fel inträffat under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-140">If no information is returned when hello job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="0c2f4-141">tooview felinformation för det här jobbet kan lägga till hello efter toohello hello **hivejob.ps1** filen, spara den och kör det igen.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-141">tooview error information for this job, add hello following toohello end of hello **hivejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print hello output of hello Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="0c2f4-142">Denna cmdlet returnerar hello information som skrivs tooSTDERR på hello server när du körde hello jobbet.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-142">This cmdlet returns hello information that is written tooSTDERR on hello server when you ran hello job.</span></span>

## <a name="summary"></a><span data-ttu-id="0c2f4-143">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="0c2f4-143">Summary</span></span>

<span data-ttu-id="0c2f4-144">Som du kan se Azure PowerShell tillhandahåller ett enkelt sätt toorun Hive-frågor i ett HDInsight-kluster, övervaka hello jobbstatusen och hämta hello utdata.</span><span class="sxs-lookup"><span data-stu-id="0c2f4-144">As you can see, Azure PowerShell provides an easy way toorun Hive queries in an HDInsight cluster, monitor hello job status, and retrieve hello output.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0c2f4-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0c2f4-145">Next steps</span></span>

<span data-ttu-id="0c2f4-146">Allmän information om Hive i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-146">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="0c2f4-147">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c2f4-147">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="0c2f4-148">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="0c2f4-148">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="0c2f4-149">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c2f4-149">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="0c2f4-150">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="0c2f4-150">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
