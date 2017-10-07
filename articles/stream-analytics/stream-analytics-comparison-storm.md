---
title: "Analytics-plattformar: Apache Storm jämförelse tooStream Analytics | Microsoft Docs"
description: "Få råd om att välja en molnplattform analytics med hjälp av en Apache Storm jämförelse tooStream Analytics. Förstå funktioner och skillnader."
keywords: "Analytics-plattformen, analytics plattformar, analytics molnplattform, storm jämförelse"
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: b9aac017-9866-4d0a-b98f-6f03881e9339
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 06/27/2017
ms.author: samacha
ms.openlocfilehash: 5a0ec5b2439596f0da962f04b776472031660062
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="choosing-a-streaming-analytics-platform-comparing-apache-storm-and-azure-stream-analytics"></a><span data-ttu-id="897cd-105">Om du väljer en streaming analytics-plattform: jämföra Apache Storm och Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="897cd-105">Choosing a streaming analytics platform: comparing Apache Storm and Azure Stream Analytics</span></span>
<span data-ttu-id="897cd-106">Azure tillhandahåller flera lösningar för att analysera strömmande data: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) och [Apache Storm på Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span><span class="sxs-lookup"><span data-stu-id="897cd-106">Azure provides multiple solutions for analyzing streaming data: [Azure Streaming Analytics](https://docs.microsoft.com/azure/stream-analytics/) and [Apache Storm on Azure HDInsight](https://azure.microsoft.com/services/hdinsight/apache-storm/).</span></span> <span data-ttu-id="897cd-107">Båda analytics-plattformar tillhandahåller hello fördelarna med en PaaS-lösning.</span><span class="sxs-lookup"><span data-stu-id="897cd-107">Both analytics platforms provide hello benefits of a PaaS solution.</span></span> <span data-ttu-id="897cd-108">Men hello plattformar har vissa viktiga skillnader i deras funktioner samt i hur du konfigurerar och hanteras.</span><span class="sxs-lookup"><span data-stu-id="897cd-108">But hello platforms have some significant differences in their capabilities as well as in how you configure and manage them.</span></span> 

<span data-ttu-id="897cd-109">Den här artikeln innehåller en sida-vid-sida-jämförelse av funktioner toohelp du välja mellan Apache Storm och Azure Stream Analytics som en plattform i molnet analytics.</span><span class="sxs-lookup"><span data-stu-id="897cd-109">This article provides a side-by-side comparison of features toohelp you choose between Apache Storm and Azure Stream Analytics as a cloud analytics platform.</span></span> 

## <a name="general-features"></a><span data-ttu-id="897cd-110">Allmänna funktioner</span><span class="sxs-lookup"><span data-stu-id="897cd-110">General features</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-111">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-111">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="897cd-112">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-112">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="897cd-113">
                    <strong>Apache Storm på HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-113">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-114">
                    <strong>Öppna datakällan?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-114">
                    <strong>Open source?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-115">Nej.</span><span class="sxs-lookup"><span data-stu-id="897cd-115">No.</span></span> <span data-ttu-id="897cd-116">Azure Stream Analytics är en Microsoft-generiska erbjudande.</span><span class="sxs-lookup"><span data-stu-id="897cd-116">Azure Stream Analytics is a Microsoft proprietary offering.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-117">Ja.</span><span class="sxs-lookup"><span data-stu-id="897cd-117">Yes.</span></span> <span data-ttu-id="897cd-118">Apache Storm är en teknik för Apache licensierad.</span><span class="sxs-lookup"><span data-stu-id="897cd-118">Apache Storm is an Apache licensed technology.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-119">
                    <strong>Microsoft support?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-119">
                    <strong>Microsoft support?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-120">Ja</span><span class="sxs-lookup"><span data-stu-id="897cd-120">Yes</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-121">Ja</span><span class="sxs-lookup"><span data-stu-id="897cd-121">Yes</span></span> </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-122">
                    <strong>Maskinvarukrav</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-122">
                    <strong>Hardware requirements</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-123">Ingen.</span><span class="sxs-lookup"><span data-stu-id="897cd-123">None.</span></span> <span data-ttu-id="897cd-124">Azure Stream Analytics är en Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="897cd-124">Azure Stream Analytics is an Azure Service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-125">Ingen.</span><span class="sxs-lookup"><span data-stu-id="897cd-125">None.</span></span> <span data-ttu-id="897cd-126">Apache Storm är ett Azure-tjänst.</span><span class="sxs-lookup"><span data-stu-id="897cd-126">Apache Storm is an Azure Service.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-127">
                    <strong>Översta enhet</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-127">
                    <strong>Top-level unit</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-128">Användare distribuera och övervaka strömmande jobb.</span><span class="sxs-lookup"><span data-stu-id="897cd-128">Users deploy and monitor streaming jobs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-129">Användare distribuera och övervaka ett hela kluster, vilket kan vara värd för flera samtidiga jobb samt andra arbetsbelastningar (inklusive batch).</span><span class="sxs-lookup"><span data-stu-id="897cd-129">Users deploy and monitor a whole cluster, which can host multiple Storm jobs as well as other workloads (including batch).</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-130">
                    <strong>Priser</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-130">
                    <strong>Pricing</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-131">Prissätts av bearbetade data och hello antalet strömningsenheter krävs per timme som hello jobbet körs.</span><span class="sxs-lookup"><span data-stu-id="897cd-131">Priced by volume of data processed and hello number of streaming units required per hour that hello job is running.</span></span> 
                </p>
                    <p><span data-ttu-id="897cd-132">Mer information finns i <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics priser</a>.</span><span class="sxs-lookup"><span data-stu-id="897cd-132">For more information, see <a href="http://azure.microsoft.com/pricing/details/stream-analytics/">Stream Analytics Pricing</a>.</span></span></p>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-133">hello-enhet från inköpsdatumet kluster-baserad och debiteras baserat på hello tiden hello klustret körs, oberoende av jobb som har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="897cd-133">hello unit of purchase is cluster-based, and is charged based on hello time hello cluster is running, independent of jobs deployed.</span></span>
                </p>
                <p>
<span data-ttu-id="897cd-134">Mer information finns i <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight priser</a>.</span><span class="sxs-lookup"><span data-stu-id="897cd-134">For more information, see <a href="http://azure.microsoft.com/pricing/details/hdinsight/">HDInsight pricing</a>.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="authoring"></a><span data-ttu-id="897cd-135">Redigering</span><span class="sxs-lookup"><span data-stu-id="897cd-135">Authoring</span></span>

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-136">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-136">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="897cd-137">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-137">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="897cd-138">
                    <strong>Apache Storm på HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-138">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-139">
                    <strong>Funktioner: SQL DSL?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-139">
                    <strong>Capabilities: SQL DSL?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-140">Ja.</span><span class="sxs-lookup"><span data-stu-id="897cd-140">Yes.</span></span> <span data-ttu-id="897cd-141">Stream Analytics ger ett SQL-liknande språk för att skapa transformationer.</span><span class="sxs-lookup"><span data-stu-id="897cd-141">Stream Analytics provides a SQL-like language for creating transformations.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-142">Nej.</span><span class="sxs-lookup"><span data-stu-id="897cd-142">No.</span></span> <span data-ttu-id="897cd-143">Användare skriva koden i Java- eller C# eller använda Trident-API: er.</span><span class="sxs-lookup"><span data-stu-id="897cd-143">Users write code in Java or C#, or use Trident APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-144">
                    <strong>Funktioner: Temporala operatorer?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-144">
                    <strong>Capabilities: Temporal operators?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-145">Fönsteraggregeringar och temporala kopplingar stöds som standard.</span><span class="sxs-lookup"><span data-stu-id="897cd-145">Windowed aggregates and temporal joins are supported by default.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-146">Temporala operatorer måste implementeras av hello användare.</span><span class="sxs-lookup"><span data-stu-id="897cd-146">Temporal operators must be implemented by hello user.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-147">
                    <strong>Utvecklingsarbetet</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-147">
                    <strong>Development experience</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-148">Användare kan skapa, felsöka och övervaka jobb genom hello Azure-portalen med exempeldata som härletts från en direktsänd dataström.</span><span class="sxs-lookup"><span data-stu-id="897cd-148">Users can create, debug, and monitor jobs through hello Azure portal, using sample data derived from a live stream.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-149">Användare med hjälp av .NET kan utveckla, felsöka och övervaka via Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="897cd-149">Users using .NET can develop, debug, and monitor through Visual Studio.</span></span> <span data-ttu-id="897cd-150">Användare som använder Java eller andra språk kan använda hello IDE efter eget val.</span><span class="sxs-lookup"><span data-stu-id="897cd-150">Users using Java or other languages can use hello IDE of their choice.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-151">
                    <strong>Stöd för fjärrfelsökning</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-151">
                    <strong>Debugging support</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-152">Grundläggande jobbet status och funktion loggarna är tillgängliga toohelp felsökning.</span><span class="sxs-lookup"><span data-stu-id="897cd-152">Basic job status and operations logs are available toohelp debug.</span></span> <span data-ttu-id="897cd-153">Stream Analytics kan för närvarande inte användare som anger vilket innehåll eller hur mycket innehåll som ingår i hello-loggar (d.v.s. utförligt läge).</span><span class="sxs-lookup"><span data-stu-id="897cd-153">Stream Analytics currently does not let users specify what content or how much content is included in hello logs (i.e., verbose mode).</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-154">Detaljerade loggar är tillgängliga.</span><span class="sxs-lookup"><span data-stu-id="897cd-154">Detailed logs are available.</span></span> <span data-ttu-id="897cd-155">Användarna kan öppna loggar i Visual Studio eller genom att logga i toohello kluster och direkt åtkomst till hello loggar.</span><span class="sxs-lookup"><span data-stu-id="897cd-155">Users can access logs in Visual Studio or by logging in toohello cluster and accessing hello logs directly.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-156">
                    <strong>Utökningsbarhet med anpassad kod?</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-156">
                    <strong>Extensibility using custom code?</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-157">Stöd för delvis med JavaScript UDF: er.</span><span class="sxs-lookup"><span data-stu-id="897cd-157">Partially support with JavaScript UDFs.</span></span> <span data-ttu-id="897cd-158">Mer information finns i <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF-integration</a>.</span><span class="sxs-lookup"><span data-stu-id="897cd-158">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-javascript-user-defined-functions">JavaScript UDF integration</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-159">Ja.</span><span class="sxs-lookup"><span data-stu-id="897cd-159">Yes.</span></span> <span data-ttu-id="897cd-160">Användare kan skriva anpassade kod i C#, Java eller andra språk som stöds på Storm.</span><span class="sxs-lookup"><span data-stu-id="897cd-160">Users can write custom code in C#, Java, or any other language supported on Storm.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="data-sources-inputs-and-outputs"></a><span data-ttu-id="897cd-161">Datakällor (indata) och utdata</span><span class="sxs-lookup"><span data-stu-id="897cd-161">Data sources (inputs) and outputs</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-162">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-162">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="897cd-163">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-163">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="897cd-164">
                    <strong>Apache Storm på HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-164">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-165">
                 <strong>Inkommande datakällor</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-165">
                 <strong>Input data sources</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="897cd-166">Azure Event Hubs och Azure Blob storage.</span><span class="sxs-lookup"><span data-stu-id="897cd-166">Azure Event Hubs and Azure Blob storage.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-167">Kopplingar är tillgängliga för Händelsehubbar i Azure, Azure Service Bus, Kafka och mycket mer.</span><span class="sxs-lookup"><span data-stu-id="897cd-167">Connectors are available for Azure Event Hubs, Azure Service Bus, Kafka, and more.</span></span> <span data-ttu-id="897cd-168">Användare kan skapa ytterligare anslutningar med anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="897cd-168">Users can create additional connectors using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-169">
                    <strong>Indata-format</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-169">
                    <strong>Input data formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-170">Avro JSON, CSV</span><span class="sxs-lookup"><span data-stu-id="897cd-170">Avro, JSON, CSV</span></span> </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-171">Användare kan implementera valfritt format med anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="897cd-171">Users can implement any format using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-172">
                    <strong>Utdata</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-172">
                    <strong>Outputs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-173">Ett direktuppspelningsjobb kan ha flera utdata.</span><span class="sxs-lookup"><span data-stu-id="897cd-173">A streaming job can have multiple outputs.</span></span> <span data-ttu-id="897cd-174">Stöds utdata är Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL DB och Power BI.</span><span class="sxs-lookup"><span data-stu-id="897cd-174">Supported outputs are Azure Event Hubs, Azure Blob storage, Azure Table storage, Azure SQL DB, and Power BI.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-175">Storm stöder många utdata i en topologi och varje utdata kan ha en egen kod för nedströms bearbetning.</span><span class="sxs-lookup"><span data-stu-id="897cd-175">Storm supports many outputs in a topology, and each output can have custom logic for downstream processing.</span></span> <span data-ttu-id="897cd-176">Storm innehåller kopplingar för Power BI, Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL och HBase.</span><span class="sxs-lookup"><span data-stu-id="897cd-176">Storm includes connectors for Power BI, Azure Event Hubs, Azure Blob storage, Azure Cosmos DB, SQL, and HBase.</span></span> <span data-ttu-id="897cd-177">Användare kan skapa ytterligare anslutningar med anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="897cd-177">Users can create additional connectors using custom code.</span></span>    
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-178">
                    <strong>Datakodning format</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-178">
                    <strong>Data-encoding formats</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-179">Data måste vara formaterad med UTF-8.</span><span class="sxs-lookup"><span data-stu-id="897cd-179">Data must be formatted using UTF-8.</span></span>
                </p>
            </td>   
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-180">Användare kan implementera en data-kodningsformat med anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="897cd-180">Users can implement any data encoding format using custom code.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="management-and-operations"></a><span data-ttu-id="897cd-181">Hantering och åtgärder</span><span class="sxs-lookup"><span data-stu-id="897cd-181">Management and operations</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-182">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-182">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="897cd-183">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-183">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="897cd-184">
                    <strong>Apache Storm på HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-184">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-185">
                    <strong>Distributionsmodell för jobb</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-185">
                    <strong>Job deployment model</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-186">Azure-portalen, PowerShell och REST API: er.</span><span class="sxs-lookup"><span data-stu-id="897cd-186">Azure portal, PowerShell, and REST APIs.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-187">Azure-portalen, PowerShell, Visual Studio och REST API: er.</span><span class="sxs-lookup"><span data-stu-id="897cd-187">Azure portal, PowerShell, Visual Studio, and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-188">
                    <strong>Övervaka (statistik, räknare, etc.)</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-188">
                    <strong>Monitoring (stats, counters, etc.)</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-189">Övervakning har implementerats med hjälp av Azure-portalen och REST API: er.</span><span class="sxs-lookup"><span data-stu-id="897cd-189">Monitoring is implemented using Azure portal and REST APIs.</span></span> <span data-ttu-id="897cd-190">Användare kan också konfigurera Azure-aviseringar.</span><span class="sxs-lookup"><span data-stu-id="897cd-190">Users can also configure Azure alerts.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-191">Övervakning har implementerats med hjälp av hello Storm-Användargränssnittet och REST API: er.</span><span class="sxs-lookup"><span data-stu-id="897cd-191">Monitoring is implemented using hello Storm UI and REST APIs.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-192">
                    <strong>Skalbarhet</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-192">
                    <strong>Scalability</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-193">Skalbarhet bestäms av hello antalet enheter för strömning (SUs) för varje jobb.</span><span class="sxs-lookup"><span data-stu-id="897cd-193">Scalability is determined by hello number of Streaming Units (SUs) for each job.</span></span> <span data-ttu-id="897cd-194">Varje Streaming Unit bearbetar in too1 MB per sekund, med ett maximalt 50 enheter.</span><span class="sxs-lookup"><span data-stu-id="897cd-194">Each Streaming Unit processes up too1 MB/second, with a maximum 50 units.</span></span> <span data-ttu-id="897cd-195">Mer information finns i <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">skala tooincrease genomströmning</a>.</span><span class="sxs-lookup"><span data-stu-id="897cd-195">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-jobs">Scale tooincrease throughput</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-196">Skalbarhet bestäms av hello antalet noder i hello HDInsight Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="897cd-196">Scalability is determined by hello number of nodes in hello HDInsight Storm cluster.</span></span> <span data-ttu-id="897cd-197">hello övre gränsen för hello antalet noder definieras av hello användarens Azure kvot.</span><span class="sxs-lookup"><span data-stu-id="897cd-197">hello top limit on hello number of nodes is defined by hello user's Azure quota.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-198">
                    <strong>Databearbetning gränser</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-198">
                    <strong>Data processing limits</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-199">Användare kan öka databearbetning eller optimera kostnader genom att öka eller minska hello antal enheter för strömning med en övre gräns på 1 GB per sekund.</span><span class="sxs-lookup"><span data-stu-id="897cd-199">Users can increase data processing or optimize costs by increasing or decreasing hello number of Streaming Units, with an upper limit of 1 GB/second.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-200">Användare kan skala klusterstorleken uppåt eller nedåt.</span><span class="sxs-lookup"><span data-stu-id="897cd-200">Users can scale cluster size up or down.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-201">
                    <strong>Stoppa/Fortsätt</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-201">
                    <strong>Stop/Resume</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-202">Stoppa och återuppta från senaste plats som har stoppats.</span><span class="sxs-lookup"><span data-stu-id="897cd-202">Stop and resume from last place stopped.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-203">Stoppar och återupptar från senaste placera stoppad baserat på en vattenstämpel.</span><span class="sxs-lookup"><span data-stu-id="897cd-203">Stop and resume from last place stopped based on a watermark.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-204">
                    <strong>Uppdatering för tjänsten och framework</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-204">
                    <strong>Service and framework update</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-205">Automatisk korrigering utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="897cd-205">Automatic patching with no downtime.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-206">Automatisk korrigering utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="897cd-206">Automatic patching with no downtime.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-207">
                    <strong>Kontinuitet för företag genom en hög tillgänglighet med garanterad servicenivåavtal</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-207">
                    <strong>Business continuity through a Highly Available Service with guaranteed SLAs</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <ul>
                <li><span data-ttu-id="897cd-208">SLA för 99,9% drifttid</span><span class="sxs-lookup"><span data-stu-id="897cd-208">SLA of 99.9% uptime</span></span></li>
                <li><span data-ttu-id="897cd-209">Automatisk återställning efter fel</span><span class="sxs-lookup"><span data-stu-id="897cd-209">Auto-recovery from failures</span></span></li>
                <li><span data-ttu-id="897cd-210">Inbyggda återställning av tillståndskänslig temporala operatorer</span><span class="sxs-lookup"><span data-stu-id="897cd-210">Built-in recovery of stateful temporal operators</span></span></li>
                </ul>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-211">SLA för 99,9% drifttid för hello Storm-kluster.</span><span class="sxs-lookup"><span data-stu-id="897cd-211">SLA of 99.9% uptime of hello Storm cluster.</span></span> 
                </p>
                <p>
<span data-ttu-id="897cd-212">Apache Storm är en feltolerant strömmande plattform.</span><span class="sxs-lookup"><span data-stu-id="897cd-212">Apache Storm is a fault-tolerant streaming platform.</span></span> <span data-ttu-id="897cd-213">Det är dock hello användarens ansvar tooensure att strömning jobb körs utan avbrott.</span><span class="sxs-lookup"><span data-stu-id="897cd-213">However, it is hello user's responsibility tooensure that streaming jobs run uninterrupted.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>

## <a name="advanced-features"></a><span data-ttu-id="897cd-214">Avancerade funktioner</span><span class="sxs-lookup"><span data-stu-id="897cd-214">Advanced features</span></span> ##

<table border="1" cellspacing="0" cellpadding="0">
    <tbody>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-215">
                    <strong> </strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-215">
                    <strong> </strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p><span data-ttu-id="897cd-216">
                    <strong>Azure Stream Analytics</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-216">
                    <strong>Azure Stream Analytics</strong>
                </span></span></p>
            </td>
            <td width="246" valign="top">
                <p><span data-ttu-id="897cd-217">
                    <strong>Apache Storm på HDInsight</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-217">
                    <strong>Apache Storm on HDInsight</strong>
                </span></span></p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-218">
                    <strong>Sen ankomst och out-ordning händelsehantering</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-218">
                    <strong>Late arrival and out-of-order event handling</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-219">Inbyggda konfigurerbara principer kan ordna om händelser, ta bort händelser eller ändra tidpunkt för händelsen.</span><span class="sxs-lookup"><span data-stu-id="897cd-219">Built-in configurable policies can reorder events, drop events, or adjust event time.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-220">Användare måste implementera logik toohandle det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="897cd-220">Users must implement logic toohandle this scenario.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-221">
                    <strong>Referensdata</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-221">
                    <strong>Reference data</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-222">Referensdata är tillgänglig från Azure Blob storage med maximalt 100 MB i cacheminnet.</span><span class="sxs-lookup"><span data-stu-id="897cd-222">Reference data is available from Azure Blob storage with a maximum of 100 MB of in-memory cache.</span></span> <span data-ttu-id="897cd-223">Referensdata uppdateras av hello-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="897cd-223">Reference data is refreshed by hello service.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-224">Ingen begränsning datastorleken.</span><span class="sxs-lookup"><span data-stu-id="897cd-224">No limits on data size.</span></span> <span data-ttu-id="897cd-225">Kopplingar är tillgängliga för HBase, Azure Cosmos DB, SQL Server och Azure.</span><span class="sxs-lookup"><span data-stu-id="897cd-225">Connectors are available for HBase, Azure Cosmos DB, SQL Server, and Azure.</span></span> <span data-ttu-id="897cd-226">Användare kan skapa ytterligare anslutningar med anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="897cd-226">Users can create additional connectors using custom code.</span></span> <span data-ttu-id="897cd-227">Referensdata måste uppdateras med anpassad kod.</span><span class="sxs-lookup"><span data-stu-id="897cd-227">Reference data must be refreshed using custom code.</span></span>
                </p>
            </td>
        </tr>
        <tr>
            <td width="174" valign="top">
                <p><span data-ttu-id="897cd-228">
                    <strong>Integrering med Machine Learning</strong>
                </span><span class="sxs-lookup"><span data-stu-id="897cd-228">
                    <strong>Integration with Machine Learning</strong>
                </span></span></p>
            </td>
            <td width="204" valign="top">
                <p>
<span data-ttu-id="897cd-229">Publicerade Azure Machine Learning-modeller kan konfigureras som funktioner när jobbet skapas.</span><span class="sxs-lookup"><span data-stu-id="897cd-229">Published Azure Machine Learning models can be configured as functions during job creation.</span></span> <span data-ttu-id="897cd-230">Mer information finns i <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">skala för Machine Learning funktioner</a>.</span><span class="sxs-lookup"><span data-stu-id="897cd-230">For more information, see <a href="https://docs.microsoft.com/azure/stream-analytics/stream-analytics-scale-with-machine-learning-functions">Scale for Machine Learning functions</a>.</span></span>
                </p>
            </td>
            <td width="246" valign="top">
                <p>
<span data-ttu-id="897cd-231">Tillgängliga via Storm bultar.</span><span class="sxs-lookup"><span data-stu-id="897cd-231">Available through Storm Bolts.</span></span>
                </p>
            </td>
        </tr>
    </tbody>
</table>
