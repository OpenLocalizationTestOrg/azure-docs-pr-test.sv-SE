---
title: "aaaUnderstand och Lös WebHCat-fel i HDInsight - Azure | Microsoft Docs"
description: "Lär dig hur tooabout vanliga fel som returneras av WebHCat i HDInsight och tooresolve dem."
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
ms.openlocfilehash: 0071a1e9ed448ae146b93c8f4f518e31b95d27c9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-resolve-errors-received-from-webhcat-on-hdinsight"></a><span data-ttu-id="befc3-103">Förstå och åtgärda fel togs emot från WebHCat på HDInsight</span><span class="sxs-lookup"><span data-stu-id="befc3-103">Understand and resolve errors received from WebHCat on HDInsight</span></span>

<span data-ttu-id="befc3-104">Lär dig mer om felmeddelanden när du använder WebHCat med HDInsight och hur tooresolve dem.</span><span class="sxs-lookup"><span data-stu-id="befc3-104">Learn about errors received when using WebHCat with HDInsight, and how tooresolve them.</span></span> <span data-ttu-id="befc3-105">WebHCat används internt av klientsidan verktyg som Azure PowerShell och hello Data Lake-verktyg för Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="befc3-105">WebHCat is used internally by client-side tools such as Azure PowerShell and hello Data Lake Tools for Visual Studio.</span></span>

## <a name="what-is-webhcat"></a><span data-ttu-id="befc3-106">Vad är WebHCat</span><span class="sxs-lookup"><span data-stu-id="befc3-106">What is WebHCat</span></span>

<span data-ttu-id="befc3-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) är en REST-API för [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog)en tabell och lagring lagringshanteringslager för Hadoop.</span><span class="sxs-lookup"><span data-stu-id="befc3-107">[WebHCat](https://cwiki.apache.org/confluence/display/Hive/WebHCat) is a REST API for [HCatalog](https://cwiki.apache.org/confluence/display/Hive/HCatalog), a table, and storage management layer for Hadoop.</span></span> <span data-ttu-id="befc3-108">WebHCat är aktiverat som standard i HDInsight-kluster och används av olika verktyg toosubmit jobb, hämta jobbstatus osv utan att logga i toohello kluster.</span><span class="sxs-lookup"><span data-stu-id="befc3-108">WebHCat is enabled by default on HDInsight clusters, and is used by various tools toosubmit jobs, get job status, etc. without logging in toohello cluster.</span></span>

## <a name="modifying-configuration"></a><span data-ttu-id="befc3-109">Ändra konfigurationen</span><span class="sxs-lookup"><span data-stu-id="befc3-109">Modifying configuration</span></span>

> [!IMPORTANT]
> <span data-ttu-id="befc3-110">Flera hello-fel som beskrivs i det här dokumentet inträffa konfigurerade maximalt har överskridits.</span><span class="sxs-lookup"><span data-stu-id="befc3-110">Several of hello errors listed in this document occur because a configured maximum has been exceeded.</span></span> <span data-ttu-id="befc3-111">När hello upplösning steg nämns att du kan ändra ett värde, måste du använda något av följande tooperform hello ändra hello:</span><span class="sxs-lookup"><span data-stu-id="befc3-111">When hello resolution step mentions that you can change a value, you must use one of hello following tooperform hello change:</span></span>

* <span data-ttu-id="befc3-112">För **Windows** kluster: använda ett skript åtgärd tooconfigure hello värde när klustret skapas.</span><span class="sxs-lookup"><span data-stu-id="befc3-112">For **Windows** clusters: Use a script action tooconfigure hello value during cluster creation.</span></span> <span data-ttu-id="befc3-113">Mer information finns i [utveckla skriptåtgärder](hdinsight-hadoop-script-actions.md).</span><span class="sxs-lookup"><span data-stu-id="befc3-113">For more information, see [Develop script actions](hdinsight-hadoop-script-actions.md).</span></span>

* <span data-ttu-id="befc3-114">För **Linux** kluster: Använd Ambari (web eller REST API) toomodify hello värde.</span><span class="sxs-lookup"><span data-stu-id="befc3-114">For **Linux** clusters: Use Ambari (web or REST API) toomodify hello value.</span></span> <span data-ttu-id="befc3-115">Mer information finns i [hantera HDInsight med Ambari](hdinsight-hadoop-manage-ambari.md)</span><span class="sxs-lookup"><span data-stu-id="befc3-115">For more information, see [Manage HDInsight using Ambari](hdinsight-hadoop-manage-ambari.md)</span></span>

> [!IMPORTANT]
> <span data-ttu-id="befc3-116">Linux är hello endast operativsystem på HDInsight version 3.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="befc3-116">Linux is hello only operating system used on HDInsight version 3.4 or greater.</span></span> <span data-ttu-id="befc3-117">Mer information finns i [HDInsight-avveckling på Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span><span class="sxs-lookup"><span data-stu-id="befc3-117">For more information, see [HDInsight retirement on Windows](hdinsight-component-versioning.md#hdinsight-windows-retirement).</span></span>

### <a name="default-configuration"></a><span data-ttu-id="befc3-118">Standardkonfigurationen</span><span class="sxs-lookup"><span data-stu-id="befc3-118">Default configuration</span></span>

<span data-ttu-id="befc3-119">Om hello följande standardvärden överskrids den WebHCat prestanda försämras eller orsaka fel:</span><span class="sxs-lookup"><span data-stu-id="befc3-119">If hello following default values are exceeded, it can degrade WebHCat performance or cause errors:</span></span>

| <span data-ttu-id="befc3-120">Inställning</span><span class="sxs-lookup"><span data-stu-id="befc3-120">Setting</span></span> | <span data-ttu-id="befc3-121">Vad verktyget gör</span><span class="sxs-lookup"><span data-stu-id="befc3-121">What it does</span></span> | <span data-ttu-id="befc3-122">Standardvärde</span><span class="sxs-lookup"><span data-stu-id="befc3-122">Default value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="befc3-123">[yarn.Scheduler.Capacity.maximum-program][maximum-applications]</span><span class="sxs-lookup"><span data-stu-id="befc3-123">[yarn.scheduler.capacity.maximum-applications][maximum-applications]</span></span> |<span data-ttu-id="befc3-124">Hej maximalt antal jobb som kan vara aktiva samtidigt (väntande eller körs)</span><span class="sxs-lookup"><span data-stu-id="befc3-124">hello maximum number of jobs that can be active concurrently (pending or running)</span></span> |<span data-ttu-id="befc3-125">10 000</span><span class="sxs-lookup"><span data-stu-id="befc3-125">10,000</span></span> |
| <span data-ttu-id="befc3-126">[templeton.Exec.Max procs][max-procs]</span><span class="sxs-lookup"><span data-stu-id="befc3-126">[templeton.exec.max-procs][max-procs]</span></span> |<span data-ttu-id="befc3-127">hello maximala antalet förfrågningar som hanteras samtidigt</span><span class="sxs-lookup"><span data-stu-id="befc3-127">hello maximum number of requests that can be served concurrently</span></span> |<span data-ttu-id="befc3-128">20</span><span class="sxs-lookup"><span data-stu-id="befc3-128">20</span></span> |
| <span data-ttu-id="befc3-129">[mapreduce.jobhistory.Max-ålder-ms][max-age-ms]</span><span class="sxs-lookup"><span data-stu-id="befc3-129">[mapreduce.jobhistory.max-age-ms][max-age-ms]</span></span> |<span data-ttu-id="befc3-130">hello antalet dagar som jobbhistorik bevaras</span><span class="sxs-lookup"><span data-stu-id="befc3-130">hello number of days that job history are retained</span></span> |<span data-ttu-id="befc3-131">7 dagar</span><span class="sxs-lookup"><span data-stu-id="befc3-131">7 days</span></span> |

## <a name="too-many-requests"></a><span data-ttu-id="befc3-132">För många begäranden</span><span class="sxs-lookup"><span data-stu-id="befc3-132">Too many requests</span></span>

<span data-ttu-id="befc3-133">**HTTP-statuskod**: 429</span><span class="sxs-lookup"><span data-stu-id="befc3-133">**HTTP Status code**: 429</span></span>

| <span data-ttu-id="befc3-134">Orsak</span><span class="sxs-lookup"><span data-stu-id="befc3-134">Cause</span></span> | <span data-ttu-id="befc3-135">Lösning</span><span class="sxs-lookup"><span data-stu-id="befc3-135">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="befc3-136">Du har överskridit hello högsta antal samtidiga begäranden hanteras av WebHCat per minut (standard är 20)</span><span class="sxs-lookup"><span data-stu-id="befc3-136">You have exceeded hello maximum concurrent requests served by WebHCat per minute (default 20)</span></span> |<span data-ttu-id="befc3-137">Minska din arbetsbelastning tooensure att du inte skicka fler än hello maximalt antal samtidiga begäranden eller öka hello samtidig förfrågan genom att ändra `templeton.exec.max-procs`.</span><span class="sxs-lookup"><span data-stu-id="befc3-137">Reduce your workload tooensure that you do not submit more than hello maximum number of concurrent requests or increase hello concurrent request limit by modifying `templeton.exec.max-procs`.</span></span> <span data-ttu-id="befc3-138">Mer information finns i [ändra konfiguration](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="befc3-138">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |

## <a name="server-unavailable"></a><span data-ttu-id="befc3-139">Servern är inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="befc3-139">Server unavailable</span></span>

<span data-ttu-id="befc3-140">**HTTP-statuskod**: 503</span><span class="sxs-lookup"><span data-stu-id="befc3-140">**HTTP Status code**: 503</span></span>

| <span data-ttu-id="befc3-141">Orsak</span><span class="sxs-lookup"><span data-stu-id="befc3-141">Cause</span></span> | <span data-ttu-id="befc3-142">Lösning</span><span class="sxs-lookup"><span data-stu-id="befc3-142">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="befc3-143">Statuskoden genereras vanligtvis under växling mellan hello primära och sekundära HeadNode för hello-kluster</span><span class="sxs-lookup"><span data-stu-id="befc3-143">This status code usually occurs during failover between hello primary and secondary HeadNode for hello cluster</span></span> |<span data-ttu-id="befc3-144">Vänta två minuter och försök igen hello</span><span class="sxs-lookup"><span data-stu-id="befc3-144">Wait two minutes, then retry hello operation</span></span> |

## <a name="bad-request-content-could-not-find-job"></a><span data-ttu-id="befc3-145">Felaktig begäran innehåll: Det gick inte att hitta jobbet</span><span class="sxs-lookup"><span data-stu-id="befc3-145">Bad request Content: Could not find job</span></span>

<span data-ttu-id="befc3-146">**HTTP-statuskod**: 400</span><span class="sxs-lookup"><span data-stu-id="befc3-146">**HTTP Status code**: 400</span></span>

| <span data-ttu-id="befc3-147">Orsak</span><span class="sxs-lookup"><span data-stu-id="befc3-147">Cause</span></span> | <span data-ttu-id="befc3-148">Lösning</span><span class="sxs-lookup"><span data-stu-id="befc3-148">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="befc3-149">Jobbinformation har rensats av hello jobbhistorik rengöringsband</span><span class="sxs-lookup"><span data-stu-id="befc3-149">Job details have been cleaned up by hello job history cleaner</span></span> |<span data-ttu-id="befc3-150">hello loggperioden för jobbhistorik är 7 dagar.</span><span class="sxs-lookup"><span data-stu-id="befc3-150">hello default retention period for job history is 7 days.</span></span> <span data-ttu-id="befc3-151">hello loggperioden kan ändras genom att ändra `mapreduce.jobhistory.max-age-ms`.</span><span class="sxs-lookup"><span data-stu-id="befc3-151">hello default retention period can be changed by modifying `mapreduce.jobhistory.max-age-ms`.</span></span> <span data-ttu-id="befc3-152">Mer information finns i [ändra konfiguration](#modifying-configuration)</span><span class="sxs-lookup"><span data-stu-id="befc3-152">For more information, see [Modifying configuration](#modifying-configuration)</span></span> |
| <span data-ttu-id="befc3-153">Jobbet har avslutats på grund av tooa växling vid fel</span><span class="sxs-lookup"><span data-stu-id="befc3-153">Job has been killed due tooa failover</span></span> |<span data-ttu-id="befc3-154">Försök jobbet för in tootwo i minuter</span><span class="sxs-lookup"><span data-stu-id="befc3-154">Retry job submission for up tootwo minutes</span></span> |
| <span data-ttu-id="befc3-155">Ett ogiltigt jobb-id har använts</span><span class="sxs-lookup"><span data-stu-id="befc3-155">An Invalid job id was used</span></span> |<span data-ttu-id="befc3-156">Kontrollera om hello jobb-id är korrekt</span><span class="sxs-lookup"><span data-stu-id="befc3-156">Check if hello job id is correct</span></span> |

## <a name="bad-gateway"></a><span data-ttu-id="befc3-157">Ogiltig gateway</span><span class="sxs-lookup"><span data-stu-id="befc3-157">Bad gateway</span></span>

<span data-ttu-id="befc3-158">**HTTP-statuskod**: 502</span><span class="sxs-lookup"><span data-stu-id="befc3-158">**HTTP Status code**: 502</span></span>

| <span data-ttu-id="befc3-159">Orsak</span><span class="sxs-lookup"><span data-stu-id="befc3-159">Cause</span></span> | <span data-ttu-id="befc3-160">Lösning</span><span class="sxs-lookup"><span data-stu-id="befc3-160">Resolution</span></span> |
| --- | --- |
| <span data-ttu-id="befc3-161">Internt skräpinsamling sker inom hello WebHCat-processen</span><span class="sxs-lookup"><span data-stu-id="befc3-161">Internal garbage collection is occurring within hello WebHCat process</span></span> |<span data-ttu-id="befc3-162">Vänta tills skräp samling toofinish eller hello WebHCat-tjänsten</span><span class="sxs-lookup"><span data-stu-id="befc3-162">Wait for garbage collection toofinish or restart hello WebHCat service</span></span> |
| <span data-ttu-id="befc3-163">Tidsgränsen nåddes för väntar på svar från hello ResourceManager-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="befc3-163">Time out waiting on a response from hello ResourceManager service.</span></span> <span data-ttu-id="befc3-164">Det här felet kan inträffa om hello antalet aktiva program går hello konfigurerade maximala (standard 10 000-tal)</span><span class="sxs-lookup"><span data-stu-id="befc3-164">This error can occur when hello number of active applications goes hello configured maximum (default 10,000)</span></span> |<span data-ttu-id="befc3-165">Vänta tills pågående jobb toocomplete eller öka hello samtidiga jobb genom att ändra `yarn.scheduler.capacity.maximum-applications`.</span><span class="sxs-lookup"><span data-stu-id="befc3-165">Wait for currently running jobs toocomplete or increase hello concurrent job limit by modifying `yarn.scheduler.capacity.maximum-applications`.</span></span> <span data-ttu-id="befc3-166">Mer information finns i hello [ändra configuration](#modifying-configuration) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="befc3-166">For more information, see hello [Modifying configuration](#modifying-configuration) section.</span></span> |
| <span data-ttu-id="befc3-167">Försök tooretrieve alla jobb via hello [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) anrop när `Fields` har angetts för`*`</span><span class="sxs-lookup"><span data-stu-id="befc3-167">Attempting tooretrieve all jobs through hello [GET /jobs](https://cwiki.apache.org/confluence/display/Hive/WebHCat+Reference+Jobs) call while `Fields` is set too`*`</span></span> |<span data-ttu-id="befc3-168">Inte hämtar *alla* Jobbdetaljer.</span><span class="sxs-lookup"><span data-stu-id="befc3-168">Do not retrieve *all* job details.</span></span> <span data-ttu-id="befc3-169">I stället använda `jobid` tooretrieve information om jobb som endast är större än vissa jobb-id. Eller Använd inte`Fields`</span><span class="sxs-lookup"><span data-stu-id="befc3-169">Instead use `jobid` tooretrieve details for jobs only greater than certain job id. Or, do not use `Fields`</span></span> |
| <span data-ttu-id="befc3-170">Hej WebHCat-tjänsten är inte tillgänglig under HeadNode växling vid fel</span><span class="sxs-lookup"><span data-stu-id="befc3-170">hello WebHCat service is down during HeadNode failover</span></span> |<span data-ttu-id="befc3-171">Vänta i två minuter och försök sedan hello åtgärden</span><span class="sxs-lookup"><span data-stu-id="befc3-171">Wait for two minutes and retry hello operation</span></span> |
| <span data-ttu-id="befc3-172">Det finns mer än 500 väntande jobb skicka via WebHCat</span><span class="sxs-lookup"><span data-stu-id="befc3-172">There are more than 500 pending jobs submitted through WebHCat</span></span> |<span data-ttu-id="befc3-173">Vänta tills väntar jobben har slutförts innan du skickar flera jobb</span><span class="sxs-lookup"><span data-stu-id="befc3-173">Wait until currently pending jobs have completed before submitting more jobs</span></span> |

[maximum-applications]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.1.3/bk_system-admin-guide/content/setting_application_limits.html
[max-procs]: https://hive.apache.org/javadocs/hcat-r0.5.0/configuration.html
[max-age-ms]: http://docs.hortonworks.com/HDPDocuments/HDP2/HDP-2.0.6.0/ds_Hadoop/hadoop-mapreduce-client/hadoop-mapreduce-client-core/mapred-default.xml
