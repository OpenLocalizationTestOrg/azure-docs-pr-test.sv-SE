---
title: "Förstå och lösa WebHCat fel på HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur på cirka vanliga fel som returneras av WebHCat i HDInsight och hur du löser dem."
services: hdinsight
documentationcenter: 
author: Blackmist
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1b3d94b1-207d-4550-aece-21dc45485549
ms.service: hdinsight
ms.custom: hdinsightactive
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/26/2017
ms.author: larryfr
ms.openlocfilehash: 6d8162e0d64ec9fc42690392b7c822593c0c2767
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a><span data-ttu-id="f9a81-103">Förstå och åtgärda fel togs emot från WebHCat på HDInsight</span><span class="sxs-lookup"><span data-stu-id="f9a81-103">Understand and resolve errors received from WebHCat on HDInsight</span></span>

<span data-ttu-id="f9a81-104">Läs mer om felmeddelanden när du använder WebHCat med HDInsight och hur du löser dem..</span><span class="sxs-lookup"><span data-stu-id="f9a81-104">Learn about errors received when using WebHCat with HDInsight, and how to resolve them.</span></span> <span data-ttu-id="f9a81-105">WebHCat används internt av klientsidan verktyg som Azure PowerShell och Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="f9a81-105">WebHCat is used internally by client-side tools such as Azure PowerShell and the Data Lake Tools for Visual Studio.</span></span>

## <a name="what-is-webhcat"></a><span data-ttu-id="f9a81-106">Vad är WebHCat</span><span class="sxs-lookup"><span data-stu-id="f9a81-106">What is WebHCat</span></span>

<span data-ttu-id="f9a81-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) är en REST-API för [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog)en tabell och lagring lagringshanteringslager för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="f9a81-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is a REST API for [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), a table, and storage management layer for Hadoop.</span></span> <span data-ttu-id="f9a81-108">WebHCat är aktiverat som standard i HDInsight-kluster och används av olika verktyg för att skicka jobb, hämta jobbstatus osv utan att logga in på klustret.</span><span class="sxs-lookup"><span data-stu-id="f9a81-108">WebHCat is enabled by default on HDInsight clusters, and is used by various tools to submit jobs, get job status, etc. without logging in to the cluster.</span></span>

## <a name="modifying-configuration"></a><span data-ttu-id="f9a81-109">Ändra konfigurationen</span><span class="sxs-lookup"><span data-stu-id="f9a81-109">Modifying configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f9a81-110">Flera av de fel som anges i det här dokumentet inträffa konfigurerade maximalt har överskridits.</span><span class="sxs-lookup"><span data-stu-id="f9a81-110">Several of the errors listed in this document occur because a configured maximum has been exceeded.</span></span> <span data-ttu-id="f9a81-111">När steget upplösning nämns att du kan ändra ett värde, måste du använda något av följande för att utföra ändringen:</span><span class="sxs-lookup"><span data-stu-id="f9a81-111">When the resolution step mentions that you can change a value, you must use one of the following to perform the change:</span></span>

* <span data-ttu-id="f9a81-112">För **Windows** kluster: Använd en skriptåtgärd för att konfigurera värdet när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="f9a81-112">For **Windows** clusters: Use a script action to configure the value during cluster creation.</span></span> <span data-ttu-id="f9a81-113">Mer information finns i [utveckla skriptåtgärder](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="f9a81-113">For more information, see [Develop script actions](hdinsight-hadoop-script-actions.md).</span></span>

* <span data-ttu-id="f9a81-114">För **Linux** kluster: Använd Ambari (web eller REST API) för att ändra värdet.</span><span class="sxs-lookup"><span data-stu-id="f9a81-114">For **Linux** clusters: Use Ambari (web or REST API) to modify the value.</span></span> <span data-ttu-id="f9a81-115">Mer information finns i [hantera HDInsight med Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="f9a81-115">For more information, see [Manage HDInsight using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="f9a81-116">Linux är det enda operativsystemet som används med HDInsight version 3.4 och senare.</span><span class="sxs-lookup"><span data-stu-id="f9a81-116">Linux is the only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="f9a81-117">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="f9a81-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="default-configuration"></a><span data-ttu-id="f9a81-118">Standardkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="f9a81-118">Default configuration</span></span>

<span data-ttu-id="f9a81-119">Om följande standardvärden överskrids kan försämra WebHCat prestanda eller orsaka fel:</span><span class="sxs-lookup"><span data-stu-id="f9a81-119">If the following default values are exceeded, it can degrade WebHCat performance or cause errors:</span></span>

| <span data-ttu-id="f9a81-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="f9a81-120">Setting</span></span> | <span data-ttu-id="f9a81-121">Vad verktyget gör</span><span class="sxs-lookup"><span data-stu-id="f9a81-121">What it does</span></span> | <span data-ttu-id="f9a81-122">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="f9a81-122">Default value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="f9a81-123">[yarn.Scheduler.Capacity.maximum-program][maximum-applications]</span><span class="sxs-lookup"><span data-stu-id="f9a81-123">[yarn.scheduler.capacity.maximum-applications][maximum-applications]</span></span> |<span data-ttu-id="f9a81-124">Det maximala antalet jobb som kan vara aktiva samtidigt (väntande eller körs)</span><span class="sxs-lookup"><span data-stu-id="f9a81-124">The maximum number of jobs that can be active concurrently (pending or running)</span></span> |<span data-ttu-id="f9a81-125">10 000</span><span class="sxs-lookup"><span data-stu-id="f9a81-125">10,000</span></span> |
| <span data-ttu-id="f9a81-126">[templeton.Exec.Max procs][max-procs]</span><span class="sxs-lookup"><span data-stu-id="f9a81-126">[templeton.exec.max-procs][max-procs]</span></span> |<span data-ttu-id="f9a81-127">Det maximala antalet förfrågningar som hanteras samtidigt</span><span class="sxs-lookup"><span data-stu-id="f9a81-127">The maximum number of requests that can be served concurrently</span></span> |<span data-ttu-id="f9a81-128">20</span><span class="sxs-lookup"><span data-stu-id="f9a81-128">20</span></span> |
| <span data-ttu-id="f9a81-129">[mapreduce.jobhistory.Max-ålder-ms][max-age-ms]</span><span class="sxs-lookup"><span data-stu-id="f9a81-129">[mapreduce.jobhistory.max-age-ms][max-age-ms]</span></span> |<span data-ttu-id="f9a81-130">Antalet dagar som jobbhistorik bevaras</span><span class="sxs-lookup"><span data-stu-id="f9a81-130">The number of days that job history are retained</span></span> |<span data-ttu-id="f9a81-131">7 dagar</span><span class="sxs-lookup"><span data-stu-id="f9a81-131">7 days</span></span> |

## <a name="too-many-requests"></a><span data-ttu-id="f9a81-132">För många begäranden</span><span class="sxs-lookup"><span data-stu-id="f9a81-132">Too many requests</span></span>

<span data-ttu-id="f9a81-133">**HTTP-statuskod**: 429</span><span class="sxs-lookup"><span data-stu-id="f9a81-133">**HTTP Status code**: 429</span></span>

| <span data-ttu-id="f9a81-134">Orsak</span><span class="sxs-lookup"><span data-stu-id="f9a81-134">Cause</span></span> | <span data-ttu-id="f9a81-135">Lösning</span><span class="sxs-lookup"><span data-stu-id="f9a81-135">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="f9a81-136">Du har överskridit de maximala samtidiga begäranden som betjänats med WebHCat per minut (standard är 20)</span><span class="sxs-lookup"><span data-stu-id="f9a81-136">You have exceeded the maximum concurrent requests served by WebHCat per minute (default 20)</span></span> |<span data-ttu-id="f9a81-137">Minska din arbetsbelastning så att du inte skickar mer än det maximala antalet samtidiga begäranden eller öka antal samtidiga begäranden genom att ändra `templeton.exec.max-procs`.</span><span class="sxs-lookup"><span data-stu-id="f9a81-137">Reduce your workload to ensure that you do not submit more than the maximum number of concurrent requests or increase the concurrent request limit by modifying `templeton.exec.max-procs`.</span></span> <span data-ttu-id="f9a81-138">Mer information finns i [ändra konfiguration](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="f9a81-138">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |

## <a name="server-unavailable"></a><span data-ttu-id="f9a81-139">Servern är inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="f9a81-139">Server unavailable</span></span>

<span data-ttu-id="f9a81-140">**HTTP-statuskod**: 503</span><span class="sxs-lookup"><span data-stu-id="f9a81-140">**HTTP Status code**: 503</span></span>

| <span data-ttu-id="f9a81-141">Orsak</span><span class="sxs-lookup"><span data-stu-id="f9a81-141">Cause</span></span> | <span data-ttu-id="f9a81-142">Lösning</span><span class="sxs-lookup"><span data-stu-id="f9a81-142">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="f9a81-143">Statuskoden genereras vanligtvis under växling mellan den primära och sekundära HeadNode för klustret</span><span class="sxs-lookup"><span data-stu-id="f9a81-143">This status code usually occurs during failover between the primary and secondary HeadNode for the cluster</span></span> |<span data-ttu-id="f9a81-144">Vänta två minuter och försök igen</span><span class="sxs-lookup"><span data-stu-id="f9a81-144">Wait two minutes, then retry the operation</span></span> |

## <a name="bad-request-content-could-not-find-job"></a><span data-ttu-id="f9a81-145">Felaktig begäran innehåll: Det gick inte att hitta jobbet</span><span class="sxs-lookup"><span data-stu-id="f9a81-145">Bad request Content: Could not find job</span></span>

<span data-ttu-id="f9a81-146">**HTTP-statuskod**: 400</span><span class="sxs-lookup"><span data-stu-id="f9a81-146">**HTTP Status code**: 400</span></span>

| <span data-ttu-id="f9a81-147">Orsak</span><span class="sxs-lookup"><span data-stu-id="f9a81-147">Cause</span></span> | <span data-ttu-id="f9a81-148">Lösning</span><span class="sxs-lookup"><span data-stu-id="f9a81-148">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="f9a81-149">Jobbinformation har rensats av jobbhistoriken rengöringsband</span><span class="sxs-lookup"><span data-stu-id="f9a81-149">Job details have been cleaned up by the job history cleaner</span></span> |<span data-ttu-id="f9a81-150">Loggperioden för jobbhistorik är 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="f9a81-150">The default retention period for job history is 7 days.</span></span> <span data-ttu-id="f9a81-151">Loggperioden kan ändras genom att ändra `mapreduce.jobhistory.max-age-ms`.</span><span class="sxs-lookup"><span data-stu-id="f9a81-151">The default retention period can be changed by modifying `mapreduce.jobhistory.max-age-ms`.</span></span> <span data-ttu-id="f9a81-152">Mer information finns i [ändra konfiguration](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="f9a81-152">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |
| <span data-ttu-id="f9a81-153">Jobbet har avslutats på grund av en växling vid fel</span><span class="sxs-lookup"><span data-stu-id="f9a81-153">Job has been killed due to a failover</span></span> |<span data-ttu-id="f9a81-154">Försök jobbet i upp till två minuter</span><span class="sxs-lookup"><span data-stu-id="f9a81-154">Retry job submission for up to two minutes</span></span> |
| <span data-ttu-id="f9a81-155">Ett ogiltigt jobb-id har använts</span><span class="sxs-lookup"><span data-stu-id="f9a81-155">An Invalid job id was used</span></span> |<span data-ttu-id="f9a81-156">Kontrollera om jobb-id är korrekt</span><span class="sxs-lookup"><span data-stu-id="f9a81-156">Check if the job id is correct</span></span> |

## <a name="bad-gateway"></a><span data-ttu-id="f9a81-157">Ogiltig gateway</span><span class="sxs-lookup"><span data-stu-id="f9a81-157">Bad gateway</span></span>

<span data-ttu-id="f9a81-158">**HTTP-statuskod**: 502</span><span class="sxs-lookup"><span data-stu-id="f9a81-158">**HTTP Status code**: 502</span></span>

| <span data-ttu-id="f9a81-159">Orsak</span><span class="sxs-lookup"><span data-stu-id="f9a81-159">Cause</span></span> | <span data-ttu-id="f9a81-160">Lösning</span><span class="sxs-lookup"><span data-stu-id="f9a81-160">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="f9a81-161">Internt skräpinsamling sker i WebHCat-processen</span><span class="sxs-lookup"><span data-stu-id="f9a81-161">Internal garbage collection is occurring within the WebHCat process</span></span> |<span data-ttu-id="f9a81-162">Vänta tills skräpinsamling avsluta eller starta om tjänsten WebHCat</span><span class="sxs-lookup"><span data-stu-id="f9a81-162">Wait for garbage collection to finish or restart the WebHCat service</span></span> |
| <span data-ttu-id="f9a81-163">Tidsgränsen nåddes för väntar på svar från ResourceManager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="f9a81-163">Time out waiting on a response from the ResourceManager service.</span></span> <span data-ttu-id="f9a81-164">Det här felet kan inträffa om antalet aktiva program går den konfigurerade maximalt (standard 10 000-tal)</span><span class="sxs-lookup"><span data-stu-id="f9a81-164">This error can occur when the number of active applications goes the configured maximum (default 10,000)</span></span> |<span data-ttu-id="f9a81-165">Vänta tills pågående jobb för att slutföra eller öka gränsen för antal samtidiga jobb genom att ändra `yarn.scheduler.capacity.maximum-applications`.</span><span class="sxs-lookup"><span data-stu-id="f9a81-165">Wait for currently running jobs to complete or increase the concurrent job limit by modifying `yarn.scheduler.capacity.maximum-applications`.</span></span> <span data-ttu-id="f9a81-166">Mer information finns i [ändra configuration](#modifying-configuration) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f9a81-166">For more information, see the [Modifying configuration](#modifying-configuration) section.</span></span> |
| <span data-ttu-id="f9a81-167">Försök att hämta alla jobb via den [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) anrop när `Fields` har angetts till`*`</span><span class="sxs-lookup"><span data-stu-id="f9a81-167">Attempting to retrieve all jobs through the [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) call while `Fields` is set to `*`</span></span> |<span data-ttu-id="f9a81-168">Inte hämtar *alla* Jobbdetaljer.</span><span class="sxs-lookup"><span data-stu-id="f9a81-168">Do not retrieve *all* job details.</span></span> <span data-ttu-id="f9a81-169">I stället använda `jobid` att hämta information om jobb som endast är större än vissa jobb-id.</span><span class="sxs-lookup"><span data-stu-id="f9a81-169">Instead use `jobid` to retrieve details for jobs only greater than certain job id.</span></span> <span data-ttu-id="f9a81-170">Eller Använd inte`Fields`</span><span class="sxs-lookup"><span data-stu-id="f9a81-170">Or, do not use `Fields`</span></span> |
| <span data-ttu-id="f9a81-171">WebHCat-tjänsten har stoppats under HeadNode växling vid fel</span><span class="sxs-lookup"><span data-stu-id="f9a81-171">The WebHCat service is down during HeadNode failover</span></span> |<span data-ttu-id="f9a81-172">Vänta i två minuter och försök igen</span><span class="sxs-lookup"><span data-stu-id="f9a81-172">Wait for two minutes and retry the operation</span></span> |
| <span data-ttu-id="f9a81-173">Det finns mer än 500 väntande jobb skicka via WebHCat</span><span class="sxs-lookup"><span data-stu-id="f9a81-173">There are more than 500 pending jobs submitted through WebHCat</span></span> |<span data-ttu-id="f9a81-174">Vänta tills väntar jobben har slutförts innan du skickar flera jobb</span><span class="sxs-lookup"><span data-stu-id="f9a81-174">Wait until currently pending jobs have completed before submitting more jobs</span></span> |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
