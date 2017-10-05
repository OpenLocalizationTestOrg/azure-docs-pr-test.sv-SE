---
title: "Använda Hadoop Hive med PowerShell i HDInsight - Azure | Microsoft Docs"
description: "Använd PowerShell för att köra Hive-frågor i Hadoop på HDInsight."
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
ms.openlocfilehash: e1cb2e4a1fc82fb43082e79a5feba71b81b3eaa8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="run-hive-queries-using-powershell"></a><span data-ttu-id="7bebe-103">Köra Hive-frågor med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7bebe-103">Run Hive queries using PowerShell</span></span>
[!INCLUDE [hive-selector](../../includes/hdinsight-selector-use-hive.md)]

<span data-ttu-id="7bebe-104">Det här dokumentet innehåller ett exempel på att använda Azure PowerShell i Azure-resursgrupp läge för att köra Hive-frågor i en Hadoop på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7bebe-104">This document provides an example of using Azure PowerShell in the Azure Resource Group mode to run Hive queries in a Hadoop on HDInsight cluster.</span></span>

> [!NOTE]
> <span data-ttu-id="7bebe-105">Det här dokumentet ger inte en detaljerad beskrivning av HiveQL-instruktioner som används i exemplen gör.</span><span class="sxs-lookup"><span data-stu-id="7bebe-105">This document does not provide a detailed description of what the HiveQL statements that are used in the examples do.</span></span> <span data-ttu-id="7bebe-106">Information om HiveQL som används i det här exemplet finns [använda Hive med Hadoop i HDInsight](hdinsight-use-hive.md).</span><span class="sxs-lookup"><span data-stu-id="7bebe-106">For information on the HiveQL that is used in this example, see [Use Hive with Hadoop on HDInsight](hdinsight-use-hive.md).</span></span>

<span data-ttu-id="7bebe-107">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="7bebe-107">**Prerequisites**</span></span>

* <span data-ttu-id="7bebe-108">**Ett Azure HDInsight-kluster**: Det spelar ingen roll om klustret är Windows eller Linux-baserade.</span><span class="sxs-lookup"><span data-stu-id="7bebe-108">**An Azure HDInsight cluster**: It does not matter whether the cluster is Windows or Linux-based.</span></span>

  > [!IMPORTANT]
  > <span data-ttu-id="7bebe-109">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="7bebe-109">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="7bebe-110">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="7bebe-110">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

* <span data-ttu-id="7bebe-111">**En arbetsstation med Azure PowerShell**.</span><span class="sxs-lookup"><span data-stu-id="7bebe-111">**A workstation with Azure PowerShell**.</span></span>

[!INCLUDE [upgrade-powershell](../../includes/hdinsight-use-latest-powershell.md)]

## <a name="run-hive-queries-using-azure-powershell"></a><span data-ttu-id="7bebe-112">Köra Hive-frågor med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7bebe-112">Run Hive queries using Azure PowerShell</span></span>

<span data-ttu-id="7bebe-113">Azure PowerShell innehåller *cmdlets* som gör det möjligt att köra Hive-frågor via fjärranslutning på HDInsight.</span><span class="sxs-lookup"><span data-stu-id="7bebe-113">Azure PowerShell provides *cmdlets* that allow you to remotely run Hive queries on HDInsight.</span></span> <span data-ttu-id="7bebe-114">Internt, cmdletarna gör REST-anrop till [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) på HDInsight-kluster.</span><span class="sxs-lookup"><span data-stu-id="7bebe-114">Internally, the cmdlets make REST calls to [WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) on the HDInsight cluster.</span></span>

<span data-ttu-id="7bebe-115">Följande cmdlets som används när du kör Hive-frågor i en fjärransluten HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="7bebe-115">The following cmdlets are used when running Hive queries in a remote HDInsight cluster:</span></span>

* <span data-ttu-id="7bebe-116">**Lägg till AzureRmAccount**: autentiserar till Azure-prenumeration i Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="7bebe-116">**Add-AzureRmAccount**: Authenticates Azure PowerShell to your Azure subscription</span></span>
* <span data-ttu-id="7bebe-117">**Nya AzureRmHDInsightHiveJobDefinition**: skapar ett *jobbet definition* med hjälp av de angivna HiveQL-instruktionerna</span><span class="sxs-lookup"><span data-stu-id="7bebe-117">**New-AzureRmHDInsightHiveJobDefinition**: Creates a *job definition* by using the specified HiveQL statements</span></span>
* <span data-ttu-id="7bebe-118">**Start-AzureRmHDInsightJob**: skickar jobbdefinitionen till HDInsight, startar jobbet och returnerar ett *jobbet* objekt som kan användas för att kontrollera status för jobbet</span><span class="sxs-lookup"><span data-stu-id="7bebe-118">**Start-AzureRmHDInsightJob**: Sends the job definition to HDInsight, starts the job, and returns a *job* object that can be used to check the status of the job</span></span>
* <span data-ttu-id="7bebe-119">**Vänta AzureRmHDInsightJob**: använder jobbobjektet för att kontrollera status för jobbet.</span><span class="sxs-lookup"><span data-stu-id="7bebe-119">**Wait-AzureRmHDInsightJob**: Uses the job object to check the status of the job.</span></span> <span data-ttu-id="7bebe-120">Den väntar tills jobbet slutförs eller väntetiden har överskridits.</span><span class="sxs-lookup"><span data-stu-id="7bebe-120">It waits until the job completes or the wait time is exceeded.</span></span>
* <span data-ttu-id="7bebe-121">**Get-AzureRmHDInsightJobOutput**: används för att hämta utdata för jobbet</span><span class="sxs-lookup"><span data-stu-id="7bebe-121">**Get-AzureRmHDInsightJobOutput**: Used to retrieve the output of the job</span></span>
* <span data-ttu-id="7bebe-122">**Anropa AzureRmHDInsightHiveJob**: används för att köra HiveQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7bebe-122">**Invoke-AzureRmHDInsightHiveJob**: Used to run HiveQL statements.</span></span> <span data-ttu-id="7bebe-123">Den här cmdlet-block frågan har slutförts, och returnerar sedan resultatet</span><span class="sxs-lookup"><span data-stu-id="7bebe-123">This cmdlet blocks the query completes, then returns the results</span></span>
* <span data-ttu-id="7bebe-124">**Använd AzureRmHDInsightCluster**: Anger det aktuella klustret ska användas för den **Invoke-AzureRmHDInsightHiveJob** kommando</span><span class="sxs-lookup"><span data-stu-id="7bebe-124">**Use-AzureRmHDInsightCluster**: Sets the current cluster to use for the **Invoke-AzureRmHDInsightHiveJob** command</span></span>

<span data-ttu-id="7bebe-125">Följande steg visar hur du använder dessa cmdlets för att köra ett jobb i HDInsight-kluster:</span><span class="sxs-lookup"><span data-stu-id="7bebe-125">The following steps demonstrate how to use these cmdlets to run a job in your HDInsight cluster:</span></span>

1. <span data-ttu-id="7bebe-126">Med hjälp av en redigerare, spara följande kod som **hivejob.ps1**.</span><span class="sxs-lookup"><span data-stu-id="7bebe-126">Using an editor, save the following code as **hivejob.ps1**.</span></span>

    <span data-ttu-id="7bebe-127">[!code-powershell[huvudsakliga](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]</span><span class="sxs-lookup"><span data-stu-id="7bebe-127">[!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=5-42)]</span></span>

2. <span data-ttu-id="7bebe-128">Öppna ett nytt **Azure PowerShell** kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="7bebe-128">Open a new **Azure PowerShell** command prompt.</span></span> <span data-ttu-id="7bebe-129">Ändra kataloger till platsen för den **hivejob.ps1** filen och sedan använder du följande kommando för att köra skriptet:</span><span class="sxs-lookup"><span data-stu-id="7bebe-129">Change directories to the location of the **hivejob.ps1** file, then use the following command to run the script:</span></span>

        .\hivejob.ps1

    <span data-ttu-id="7bebe-130">När skriptet körs, uppmanas du att ange klusternamnet och HTTPS/Admin-autentiseringsuppgifter för klustret.</span><span class="sxs-lookup"><span data-stu-id="7bebe-130">When the script runs, you are prompted to enter the cluster name and the HTTPS/Admin account credentials for the cluster.</span></span> <span data-ttu-id="7bebe-131">Du kan också uppmanas att logga in på Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="7bebe-131">You may also be prompted to log in to your Azure subscription.</span></span>

3. <span data-ttu-id="7bebe-132">När jobbet är slutfört returnerar den information liknar följande thext:</span><span class="sxs-lookup"><span data-stu-id="7bebe-132">When the job completes, it returns information similar to the following thext:</span></span>

        Display the standard output...
        2012-02-03      18:35:34        SampleClass0    [ERROR] incorrect       id
        2012-02-03      18:55:54        SampleClass1    [ERROR] incorrect       id
        2012-02-03      19:25:27        SampleClass4    [ERROR] incorrect       id

4. <span data-ttu-id="7bebe-133">Som tidigare nämnts **Invoke-Hive** kan användas för att köra en fråga och vänta på svar.</span><span class="sxs-lookup"><span data-stu-id="7bebe-133">As mentioned earlier, **Invoke-Hive** can be used to run a query and wait for the response.</span></span> <span data-ttu-id="7bebe-134">Använd följande skript för att se hur Invoke-Hive fungerar:</span><span class="sxs-lookup"><span data-stu-id="7bebe-134">Use the following script to see how Invoke-Hive works:</span></span>

    <span data-ttu-id="7bebe-135">[!code-powershell[huvudsakliga](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]</span><span class="sxs-lookup"><span data-stu-id="7bebe-135">[!code-powershell[main](../../powershell_scripts/hdinsight/use-hive/use-hive.ps1?range=50-71)]</span></span>

    <span data-ttu-id="7bebe-136">Det ser ut som följande utdata:</span><span class="sxs-lookup"><span data-stu-id="7bebe-136">The output looks like the following text:</span></span>

        2012-02-03    18:35:34    SampleClass0    [ERROR]    incorrect    id
        2012-02-03    18:55:54    SampleClass1    [ERROR]    incorrect    id
        2012-02-03    19:25:27    SampleClass4    [ERROR]    incorrect    id

   > [!NOTE]
   > <span data-ttu-id="7bebe-137">Du kan använda Azure PowerShell för längre HiveQL frågor **här strängar** cmdlet eller HiveQL skriptfiler.</span><span class="sxs-lookup"><span data-stu-id="7bebe-137">For longer HiveQL queries, you can use the Azure PowerShell **Here-Strings** cmdlet or HiveQL script files.</span></span> <span data-ttu-id="7bebe-138">Följande utdrag visar hur du använder den **Invoke-Hive** för att köra en HiveQL-skriptfil.</span><span class="sxs-lookup"><span data-stu-id="7bebe-138">The following snippet shows how to use the **Invoke-Hive** cmdlet to run a HiveQL script file.</span></span> <span data-ttu-id="7bebe-139">HiveQL skriptfilen måste överföras till wasb: / /.</span><span class="sxs-lookup"><span data-stu-id="7bebe-139">The HiveQL script file must be uploaded to wasb://.</span></span>
   >
   > `Invoke-AzureRmHDInsightHiveJob -File "wasb://<ContainerName>@<StorageAccountName>/<Path>/query.hql"`
   >
   > <span data-ttu-id="7bebe-140">Mer information om **här strängar**, se <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">med hjälp av Windows PowerShell här-strängar</a>.</span><span class="sxs-lookup"><span data-stu-id="7bebe-140">For more information about **Here-Strings**, see <a href="http://technet.microsoft.com/library/ee692792.aspx" target="_blank">Using Windows PowerShell Here-Strings</a>.</span></span>

## <a name="troubleshooting"></a><span data-ttu-id="7bebe-141">Felsökning</span><span class="sxs-lookup"><span data-stu-id="7bebe-141">Troubleshooting</span></span>

<span data-ttu-id="7bebe-142">Om ingen information returneras när jobbet är slutfört, har ett fel inträffat under bearbetning.</span><span class="sxs-lookup"><span data-stu-id="7bebe-142">If no information is returned when the job completes, an error may have occurred during processing.</span></span> <span data-ttu-id="7bebe-143">Om du vill visa information om fel för det här jobbet, lägger du till följande i slutet av den **hivejob.ps1** filen, spara den och kör det igen.</span><span class="sxs-lookup"><span data-stu-id="7bebe-143">To view error information for this job, add the following to the end of the **hivejob.ps1** file, save it, and then run it again.</span></span>

```powershell
# Print the output of the Hive job.
Get-AzureRmHDInsightJobOutput `
        -Clustername $clusterName `
        -JobId $job.JobId `
        -HttpCredential $creds `
        -DisplayOutputType StandardError
```

<span data-ttu-id="7bebe-144">Denna cmdlet returnerar informationen som skrivs till STDERR på servern när du körde jobbet.</span><span class="sxs-lookup"><span data-stu-id="7bebe-144">This cmdlet returns the information that is written to STDERR on the server when you ran the job.</span></span>

## <a name="summary"></a><span data-ttu-id="7bebe-145">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="7bebe-145">Summary</span></span>

<span data-ttu-id="7bebe-146">Du kan se ger Azure PowerShell ett enkelt sätt att köra Hive-frågor i ett HDInsight-kluster, övervaka jobbstatus och hämta utdata.</span><span class="sxs-lookup"><span data-stu-id="7bebe-146">As you can see, Azure PowerShell provides an easy way to run Hive queries in an HDInsight cluster, monitor the job status, and retrieve the output.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7bebe-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7bebe-147">Next steps</span></span>

<span data-ttu-id="7bebe-148">Allmän information om Hive i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="7bebe-148">For general information about Hive in HDInsight:</span></span>

* [<span data-ttu-id="7bebe-149">Använda Hive med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7bebe-149">Use Hive with Hadoop on HDInsight</span></span>](hdinsight-use-hive.md)

<span data-ttu-id="7bebe-150">Information om andra sätt kan du arbeta med Hadoop i HDInsight:</span><span class="sxs-lookup"><span data-stu-id="7bebe-150">For information about other ways you can work with Hadoop on HDInsight:</span></span>

* [<span data-ttu-id="7bebe-151">Använda Pig med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7bebe-151">Use Pig with Hadoop on HDInsight</span></span>](hdinsight-use-pig.md)
* [<span data-ttu-id="7bebe-152">Använda MapReduce med Hadoop i HDInsight</span><span class="sxs-lookup"><span data-stu-id="7bebe-152">Use MapReduce with Hadoop on HDInsight</span></span>](hdinsight-use-mapreduce.md)
