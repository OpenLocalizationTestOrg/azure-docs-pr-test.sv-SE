---
title: "aaaQuery exempel på vanliga användningsmönster i Stream Analytics | Microsoft Docs"
description: "Vanliga frågemönster för Azure Stream Analytics"
keywords: "frågan exempel"
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jenniehubbard
editor: cgronlun
ms.assetid: 6b9a7d00-fbcc-42f6-9cbb-8bbf0bbd3d0e
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 08/08/2017
ms.author: jenniehubbard
ms.openlocfilehash: c8f7a8ac661eaf0281f4140b02c42141b73040fe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-examples-for-common-stream-analytics-usage-patterns"></a><span data-ttu-id="ec7d3-104">Exempel på vanliga Stream Analytics användningsmönster fråga</span><span class="sxs-lookup"><span data-stu-id="ec7d3-104">Query examples for common Stream Analytics usage patterns</span></span>
## <a name="introduction"></a><span data-ttu-id="ec7d3-105">Introduktion</span><span class="sxs-lookup"><span data-stu-id="ec7d3-105">Introduction</span></span>
<span data-ttu-id="ec7d3-106">Frågorna i Azure Stream Analytics uttrycks i ett SQL-liknande frågespråk.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-106">Queries in Azure Stream Analytics are expressed in a SQL-like query language.</span></span> <span data-ttu-id="ec7d3-107">De här frågorna dokumenteras i hello [Stream Analytics fråga Språkreferens](https://msdn.microsoft.com/library/azure/dn834998.aspx) guide.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-107">These queries are documented in hello [Stream Analytics query language reference](https://msdn.microsoft.com/library/azure/dn834998.aspx) guide.</span></span> <span data-ttu-id="ec7d3-108">Den här artikeln beskrivs lösningar tooseveral vanliga frågemönster, baserat på verkliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-108">This article outlines solutions tooseveral common query patterns, based on real-world scenarios.</span></span> <span data-ttu-id="ec7d3-109">Det är ett pågående arbete och fortsätter toobe uppdateras med nya mönster kontinuerligt.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-109">It is a work in progress and continues toobe updated with new patterns on an ongoing basis.</span></span>

## <a name="query-example-convert-data-types"></a><span data-ttu-id="ec7d3-110">Frågan exempel: konvertera-datatyper</span><span class="sxs-lookup"><span data-stu-id="ec7d3-110">Query example: Convert data types</span></span>
<span data-ttu-id="ec7d3-111">**Beskrivning**: definiera hello typer av egenskaper på hello Indataströmmen.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-111">**Description**: Define hello types of properties on hello input stream.</span></span>
<span data-ttu-id="ec7d3-112">Till exempel hello bil vikt kommer på hello Indataströmmen som strängar och behöver toobe konverteras för**INT** tooperform **SUMMAN** den.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-112">For example, hello car weight is coming on hello input stream as strings and needs toobe converted too**INT** tooperform **SUM** it up.</span></span>

<span data-ttu-id="ec7d3-113">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-113">**Input**:</span></span>

| <span data-ttu-id="ec7d3-114">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-114">Make</span></span> | <span data-ttu-id="ec7d3-115">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-115">Time</span></span> | <span data-ttu-id="ec7d3-116">Vikt</span><span class="sxs-lookup"><span data-stu-id="ec7d3-116">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-117">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-117">Honda</span></span> |<span data-ttu-id="ec7d3-118">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-118">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="ec7d3-119">"1000"</span><span class="sxs-lookup"><span data-stu-id="ec7d3-119">"1000"</span></span> |
| <span data-ttu-id="ec7d3-120">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-120">Honda</span></span> |<span data-ttu-id="ec7d3-121">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-121">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="ec7d3-122">"2000"</span><span class="sxs-lookup"><span data-stu-id="ec7d3-122">"2000"</span></span> |

<span data-ttu-id="ec7d3-123">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-123">**Output**:</span></span>

| <span data-ttu-id="ec7d3-124">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-124">Make</span></span> | <span data-ttu-id="ec7d3-125">Vikt</span><span class="sxs-lookup"><span data-stu-id="ec7d3-125">Weight</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-126">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-126">Honda</span></span> |<span data-ttu-id="ec7d3-127">3000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-127">3000</span></span> |

<span data-ttu-id="ec7d3-128">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-128">**Solution**:</span></span>

    SELECT
        Make,
        SUM(CAST(Weight AS BIGINT)) AS Weight
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

<span data-ttu-id="ec7d3-129">**Förklaring**: Använd ett **OMVANDLINGEN** instruktionen i hello **vikt** fältet toospecify dess datatyp.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-129">**Explanation**: Use a **CAST** statement in hello **Weight** field toospecify its data type.</span></span> <span data-ttu-id="ec7d3-130">Visa hello lista över vilka datatyper i [datatyper (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span><span class="sxs-lookup"><span data-stu-id="ec7d3-130">See hello list of supported data types in [Data types (Azure Stream Analytics)](https://msdn.microsoft.com/library/azure/dn835065.aspx).</span></span>

## <a name="query-example-use-likenot-like-toodo-pattern-matching"></a><span data-ttu-id="ec7d3-131">Frågan exempel: Använd liknande ej som toodo matchning</span><span class="sxs-lookup"><span data-stu-id="ec7d3-131">Query example: Use Like/Not like toodo pattern matching</span></span>
<span data-ttu-id="ec7d3-132">**Beskrivning**: kontrollerar att ett fältvärde på hello händelse matchar ett visst mönster.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-132">**Description**: Check that a field value on hello event matches a certain pattern.</span></span>
<span data-ttu-id="ec7d3-133">Kontrollera exempelvis att hello resultatet returnerar licens nivåer som börjar med ett och slutar med 9.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-133">For example, check that hello result returns license plates that start with A and end with 9.</span></span>

<span data-ttu-id="ec7d3-134">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-134">**Input**:</span></span>

| <span data-ttu-id="ec7d3-135">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-135">Make</span></span> | <span data-ttu-id="ec7d3-136">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-136">LicensePlate</span></span> | <span data-ttu-id="ec7d3-137">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-137">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-138">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-138">Honda</span></span> |<span data-ttu-id="ec7d3-139">ABC 123</span><span class="sxs-lookup"><span data-stu-id="ec7d3-139">ABC-123</span></span> |<span data-ttu-id="ec7d3-140">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-140">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-141">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-141">Toyota</span></span> |<span data-ttu-id="ec7d3-142">AAA-999</span><span class="sxs-lookup"><span data-stu-id="ec7d3-142">AAA-999</span></span> |<span data-ttu-id="ec7d3-143">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-143">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-144">Nissan</span><span class="sxs-lookup"><span data-stu-id="ec7d3-144">Nissan</span></span> |<span data-ttu-id="ec7d3-145">ABC 369</span><span class="sxs-lookup"><span data-stu-id="ec7d3-145">ABC-369</span></span> |<span data-ttu-id="ec7d3-146">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-146">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="ec7d3-147">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-147">**Output**:</span></span>

| <span data-ttu-id="ec7d3-148">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-148">Make</span></span> | <span data-ttu-id="ec7d3-149">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-149">LicensePlate</span></span> | <span data-ttu-id="ec7d3-150">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-150">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-151">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-151">Toyota</span></span> |<span data-ttu-id="ec7d3-152">AAA-999</span><span class="sxs-lookup"><span data-stu-id="ec7d3-152">AAA-999</span></span> |<span data-ttu-id="ec7d3-153">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-153">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-154">Nissan</span><span class="sxs-lookup"><span data-stu-id="ec7d3-154">Nissan</span></span> |<span data-ttu-id="ec7d3-155">ABC 369</span><span class="sxs-lookup"><span data-stu-id="ec7d3-155">ABC-369</span></span> |<span data-ttu-id="ec7d3-156">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-156">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="ec7d3-157">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-157">**Solution**:</span></span>

    SELECT
        *
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LicensePlate LIKE 'A%9'

<span data-ttu-id="ec7d3-158">**Förklaring**: Använd hello **som** instruktionen toocheck hello **LicensePlate** fältet värde.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-158">**Explanation**: Use hello **LIKE** statement toocheck hello **LicensePlate** field value.</span></span> <span data-ttu-id="ec7d3-159">Det måste börja med en A-, och sedan har ett antal noll eller fler tecken och sedan avslutas med en 9.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-159">It should start with an A, then have any string of zero or more characters, and then end with a 9.</span></span> 

## <a name="query-example-specify-logic-for-different-casesvalues-case-statements"></a><span data-ttu-id="ec7d3-160">Frågan exempel: Ange logik för olika fall/värden (CASE-satser)</span><span class="sxs-lookup"><span data-stu-id="ec7d3-160">Query example: Specify logic for different cases/values (CASE statements)</span></span>
<span data-ttu-id="ec7d3-161">**Beskrivning**: Ange en annan beräkning för ett fält baserat på ett visst kriterium.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-161">**Description**: Provide a different computation for a field, based on a particular criterion.</span></span>
<span data-ttu-id="ec7d3-162">Ange till exempel en sträng beskrivning för hur många bilar av hello samma skickas med ett specialfall för 1.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-162">For example, provide a string description for how many cars of hello same make passed, with a special case for 1.</span></span>

<span data-ttu-id="ec7d3-163">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-163">**Input**:</span></span>

| <span data-ttu-id="ec7d3-164">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-164">Make</span></span> | <span data-ttu-id="ec7d3-165">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-165">Time</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-166">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-166">Honda</span></span> |<span data-ttu-id="ec7d3-167">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-167">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-168">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-168">Toyota</span></span> |<span data-ttu-id="ec7d3-169">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-169">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-170">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-170">Toyota</span></span> |<span data-ttu-id="ec7d3-171">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-171">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="ec7d3-172">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-172">**Output**:</span></span>

| <span data-ttu-id="ec7d3-173">CarsPassed</span><span class="sxs-lookup"><span data-stu-id="ec7d3-173">CarsPassed</span></span> | <span data-ttu-id="ec7d3-174">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-174">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-175">1 Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-175">1 Honda</span></span> |<span data-ttu-id="ec7d3-176">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-176">2015-01-01T00:00:10.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-177">2 Toyotas</span><span class="sxs-lookup"><span data-stu-id="ec7d3-177">2 Toyotas</span></span> |<span data-ttu-id="ec7d3-178">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-178">2015-01-01T00:00:10.0000000Z</span></span> |

<span data-ttu-id="ec7d3-179">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-179">**Solution**:</span></span>

    SELECT
        CASE
            WHEN COUNT(*) = 1 THEN CONCAT('1 ', Make)
            ELSE CONCAT(CAST(COUNT(*) AS NVARCHAR(MAX)), ' ', Make, 's')
        END AS CarsPassed,
        System.TimeStamp AS Time
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)

<span data-ttu-id="ec7d3-180">**Förklaring**: hello **FALLET** satsen gör att vi tooprovide olika beräkning, baserat på vissa kriterier (i vårt fall hello antal hello bilar i hello sammanställd fönstret).</span><span class="sxs-lookup"><span data-stu-id="ec7d3-180">**Explanation**: hello **CASE** clause allows us tooprovide a different computation, based on some criteria (in our case, hello count of hello cars in hello aggregate window).</span></span>

## <a name="query-example-send-data-toomultiple-outputs"></a><span data-ttu-id="ec7d3-181">Frågan exempel: skicka data toomultiple matar ut</span><span class="sxs-lookup"><span data-stu-id="ec7d3-181">Query example: Send data toomultiple outputs</span></span>
<span data-ttu-id="ec7d3-182">**Beskrivning**: skicka data toomultiple utdata mål från ett enda utskriftsjobb.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-182">**Description**: Send data toomultiple output targets from a single job.</span></span>
<span data-ttu-id="ec7d3-183">Till exempel analysera data för en avisering om tröskelvärdesbaserad och arkivlagring alla händelser tooblob.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-183">For example, analyze data for a threshold-based alert and archive all events tooblob storage.</span></span>

<span data-ttu-id="ec7d3-184">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-184">**Input**:</span></span>

| <span data-ttu-id="ec7d3-185">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-185">Make</span></span> | <span data-ttu-id="ec7d3-186">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-186">Time</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-187">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-187">Honda</span></span> |<span data-ttu-id="ec7d3-188">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-188">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-189">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-189">Honda</span></span> |<span data-ttu-id="ec7d3-190">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-190">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-191">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-191">Toyota</span></span> |<span data-ttu-id="ec7d3-192">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-192">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-193">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-193">Toyota</span></span> |<span data-ttu-id="ec7d3-194">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-194">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-195">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-195">Toyota</span></span> |<span data-ttu-id="ec7d3-196">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-196">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="ec7d3-197">**Output1**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-197">**Output1**:</span></span>

| <span data-ttu-id="ec7d3-198">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-198">Make</span></span> | <span data-ttu-id="ec7d3-199">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-199">Time</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-200">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-200">Honda</span></span> |<span data-ttu-id="ec7d3-201">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-201">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-202">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-202">Honda</span></span> |<span data-ttu-id="ec7d3-203">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-203">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-204">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-204">Toyota</span></span> |<span data-ttu-id="ec7d3-205">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-205">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-206">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-206">Toyota</span></span> |<span data-ttu-id="ec7d3-207">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-207">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-208">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-208">Toyota</span></span> |<span data-ttu-id="ec7d3-209">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-209">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="ec7d3-210">**Output2**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-210">**Output2**:</span></span>

| <span data-ttu-id="ec7d3-211">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-211">Make</span></span> | <span data-ttu-id="ec7d3-212">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-212">Time</span></span> | <span data-ttu-id="ec7d3-213">Antal</span><span class="sxs-lookup"><span data-stu-id="ec7d3-213">Count</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-214">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-214">Toyota</span></span> |<span data-ttu-id="ec7d3-215">2015-01-01T00:00:10.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-215">2015-01-01T00:00:10.0000000Z</span></span> |<span data-ttu-id="ec7d3-216">3</span><span class="sxs-lookup"><span data-stu-id="ec7d3-216">3</span></span> |

<span data-ttu-id="ec7d3-217">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-217">**Solution**:</span></span>

    SELECT
        *
    INTO
        ArchiveOutput
    FROM
        Input TIMESTAMP BY Time

    SELECT
        Make,
        System.TimeStamp AS Time,
        COUNT(*) AS [Count]
    INTO
        AlertOutput
    FROM
        Input TIMESTAMP BY Time
    GROUP BY
        Make,
        TumblingWindow(second, 10)
    HAVING
        [Count] >= 3

<span data-ttu-id="ec7d3-218">**Förklaring**: hello **INTO** satsen instruerar Stream Analytics som hello matar ut toowrite hello data toofrom den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-218">**Explanation**: hello **INTO** clause tells Stream Analytics which of hello outputs toowrite hello data toofrom this statement.</span></span>
<span data-ttu-id="ec7d3-219">hello första frågan är en direktlagringsdisk hello data togs emot tooan utdata som vi med namnet **ArchiveOutput**.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-219">hello first query is a pass-through of hello data we received tooan output that we named **ArchiveOutput**.</span></span>
<span data-ttu-id="ec7d3-220">hello andra frågan har några enkla aggregering och filtrering och skickar resultatet hello tooa underordnade aviseringar system.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-220">hello second query does some simple aggregation and filtering, and it sends hello results tooa downstream alerting system.</span></span>

<span data-ttu-id="ec7d3-221">Observera att du kan också återanvända hello resultaten av hello cte (cte-referenser) (som **WITH** instruktioner) i flera instruktioner i utdata.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-221">Note that you can also reuse hello results of hello common table expressions (CTEs) (such as **WITH** statements) in multiple output statements.</span></span> <span data-ttu-id="ec7d3-222">Det här alternativet har hello ytterligare fördelen med att öppna färre läsare toohello Indatakällan.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-222">This option has hello added benefit of opening fewer readers toohello input source.</span></span>
<span data-ttu-id="ec7d3-223">Exempel:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-223">For example:</span></span> 

    WITH AllRedCars AS (
        SELECT
            *
        FROM
            Input TIMESTAMP BY Time
        WHERE
            Color = 'red'
    )
    SELECT * INTO HondaOutput FROM AllRedCars WHERE Make = 'Honda'
    SELECT * INTO ToyotaOutput FROM AllRedCars WHERE Make = 'Toyota'

## <a name="query-example-count-unique-values"></a><span data-ttu-id="ec7d3-224">Frågan exempel: Räkna antalet unika värden</span><span class="sxs-lookup"><span data-stu-id="ec7d3-224">Query example: Count unique values</span></span>
<span data-ttu-id="ec7d3-225">**Beskrivning**: antal hello unikt fältvärden som visas i hello dataströmmen inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-225">**Description**: Count hello number of unique field values that appear in hello stream within a time window.</span></span>
<span data-ttu-id="ec7d3-226">Till exempel hur många unika gör bilar passerat hello avgift monter i ett fönster med 2 sekunder?</span><span class="sxs-lookup"><span data-stu-id="ec7d3-226">For example, how many unique makes of cars passed through hello toll booth in a 2-second window?</span></span>

<span data-ttu-id="ec7d3-227">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-227">**Input**:</span></span>

| <span data-ttu-id="ec7d3-228">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-228">Make</span></span> | <span data-ttu-id="ec7d3-229">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-229">Time</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-230">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-230">Honda</span></span> |<span data-ttu-id="ec7d3-231">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-231">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-232">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-232">Honda</span></span> |<span data-ttu-id="ec7d3-233">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-233">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-234">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-234">Toyota</span></span> |<span data-ttu-id="ec7d3-235">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-235">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-236">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-236">Toyota</span></span> |<span data-ttu-id="ec7d3-237">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-237">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-238">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-238">Toyota</span></span> |<span data-ttu-id="ec7d3-239">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-239">2015-01-01T00:00:03.0000000Z</span></span> |

<span data-ttu-id="ec7d3-240">**Utdata:**</span><span class="sxs-lookup"><span data-stu-id="ec7d3-240">**Output:**</span></span>

| <span data-ttu-id="ec7d3-241">Antal</span><span class="sxs-lookup"><span data-stu-id="ec7d3-241">Count</span></span> | <span data-ttu-id="ec7d3-242">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-242">Time</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-243">2</span><span class="sxs-lookup"><span data-stu-id="ec7d3-243">2</span></span> |<span data-ttu-id="ec7d3-244">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-244">2015-01-01T00:00:02.000Z</span></span> |
| <span data-ttu-id="ec7d3-245">1</span><span class="sxs-lookup"><span data-stu-id="ec7d3-245">1</span></span> |<span data-ttu-id="ec7d3-246">2015-01-01T00:00:04.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-246">2015-01-01T00:00:04.000Z</span></span> |

<span data-ttu-id="ec7d3-247">**Lösning:**</span><span class="sxs-lookup"><span data-stu-id="ec7d3-247">**Solution:**</span></span>

````
SELECT
     COUNT(DISTINCT Make) AS CountMake,
     System.TIMESTAMP AS TIME
FROM Input TIMESTAMP BY TIME
GROUP BY 
     TumblingWindow(second, 2)
````


<span data-ttu-id="ec7d3-248">**Förklaring:**
**COUNT (DISTINKTA gör)** returnerar hello antalet distinkta värden i hello **Se** kolumnen inom ett tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-248">**Explanation:**
**COUNT(DISTINCT Make)** returns hello number of distinct values in hello **Make** column within a time window.</span></span>

## <a name="query-example-determine-if-a-value-has-changed"></a><span data-ttu-id="ec7d3-249">Frågan exempel: fastställa om ett värde har ändrats</span><span class="sxs-lookup"><span data-stu-id="ec7d3-249">Query example: Determine if a value has changed</span></span>
<span data-ttu-id="ec7d3-250">**Beskrivning**: Titta på en tidigare värde toodetermine om den skiljer sig hello aktuellt värde.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-250">**Description**: Look at a previous value toodetermine if it is different than hello current value.</span></span>
<span data-ttu-id="ec7d3-251">Exempelvis är hello tidigare bil på hello avgift väg hello samma göra hello aktuella bilen?</span><span class="sxs-lookup"><span data-stu-id="ec7d3-251">For example, is hello previous car on hello toll road hello same make as hello current car?</span></span>

<span data-ttu-id="ec7d3-252">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-252">**Input**:</span></span>

| <span data-ttu-id="ec7d3-253">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-253">Make</span></span> | <span data-ttu-id="ec7d3-254">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-254">Time</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-255">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-255">Honda</span></span> |<span data-ttu-id="ec7d3-256">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-256">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-257">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-257">Toyota</span></span> |<span data-ttu-id="ec7d3-258">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-258">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="ec7d3-259">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-259">**Output**:</span></span>

| <span data-ttu-id="ec7d3-260">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-260">Make</span></span> | <span data-ttu-id="ec7d3-261">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-261">Time</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-262">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-262">Toyota</span></span> |<span data-ttu-id="ec7d3-263">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-263">2015-01-01T00:00:02.0000000Z</span></span> |

<span data-ttu-id="ec7d3-264">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-264">**Solution**:</span></span>

    SELECT
        Make,
        Time
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(minute, 1)) <> Make

<span data-ttu-id="ec7d3-265">**Förklaring**: Använd **FÖRDRÖJNING** toopeek till hello inkommande dataström tillbaka en händelse och få hello **Se** värde.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-265">**Explanation**: Use **LAG** toopeek into hello input stream one event back and get hello **Make** value.</span></span> <span data-ttu-id="ec7d3-266">Jämför det toohello **Se** värdet på hello händelsen och hello utdatahändelse om de är olika.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-266">Then compare it toohello **Make** value on hello current event and output hello event if they are different.</span></span>

## <a name="query-example-find-hello-first-event-in-a-window"></a><span data-ttu-id="ec7d3-267">Frågan exempel: Sök hello första händelsen i ett fönster</span><span class="sxs-lookup"><span data-stu-id="ec7d3-267">Query example: Find hello first event in a window</span></span>
<span data-ttu-id="ec7d3-268">**Beskrivning**: hitta hello första bil i varje 10 minuters intervall.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-268">**Description**: Find hello first car in every 10-minute interval.</span></span>

<span data-ttu-id="ec7d3-269">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-269">**Input**:</span></span>

| <span data-ttu-id="ec7d3-270">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-270">LicensePlate</span></span> | <span data-ttu-id="ec7d3-271">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-271">Make</span></span> | <span data-ttu-id="ec7d3-272">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-272">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-273">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="ec7d3-273">DXE 5291</span></span> |<span data-ttu-id="ec7d3-274">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-274">Honda</span></span> |<span data-ttu-id="ec7d3-275">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-275">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-276">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="ec7d3-276">YZK 5704</span></span> |<span data-ttu-id="ec7d3-277">Ford</span><span class="sxs-lookup"><span data-stu-id="ec7d3-277">Ford</span></span> |<span data-ttu-id="ec7d3-278">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-278">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-279">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="ec7d3-279">RMV 8282</span></span> |<span data-ttu-id="ec7d3-280">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-280">Honda</span></span> |<span data-ttu-id="ec7d3-281">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-281">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-282">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="ec7d3-282">YHN 6970</span></span> |<span data-ttu-id="ec7d3-283">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-283">Toyota</span></span> |<span data-ttu-id="ec7d3-284">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-284">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-285">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="ec7d3-285">VFE 1616</span></span> |<span data-ttu-id="ec7d3-286">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-286">Toyota</span></span> |<span data-ttu-id="ec7d3-287">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-287">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-288">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="ec7d3-288">QYF 9358</span></span> |<span data-ttu-id="ec7d3-289">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-289">Honda</span></span> |<span data-ttu-id="ec7d3-290">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-290">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-291">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="ec7d3-291">MDR 6128</span></span> |<span data-ttu-id="ec7d3-292">BMW</span><span class="sxs-lookup"><span data-stu-id="ec7d3-292">BMW</span></span> |<span data-ttu-id="ec7d3-293">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-293">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="ec7d3-294">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-294">**Output**:</span></span>

| <span data-ttu-id="ec7d3-295">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-295">LicensePlate</span></span> | <span data-ttu-id="ec7d3-296">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-296">Make</span></span> | <span data-ttu-id="ec7d3-297">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-297">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-298">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="ec7d3-298">DXE 5291</span></span> |<span data-ttu-id="ec7d3-299">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-299">Honda</span></span> |<span data-ttu-id="ec7d3-300">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-300">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-301">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="ec7d3-301">QYF 9358</span></span> |<span data-ttu-id="ec7d3-302">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-302">Honda</span></span> |<span data-ttu-id="ec7d3-303">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-303">2015-07-27T00:12:02.0000000Z</span></span> |

<span data-ttu-id="ec7d3-304">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-304">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) = 1

<span data-ttu-id="ec7d3-305">Nu ska vi ändra hello problemet och hitta hello första bil av en viss gör i varje 10 minuters intervall.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-305">Now let’s change hello problem and find hello first car of a particular make in every 10-minute interval.</span></span>

| <span data-ttu-id="ec7d3-306">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-306">LicensePlate</span></span> | <span data-ttu-id="ec7d3-307">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-307">Make</span></span> | <span data-ttu-id="ec7d3-308">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-308">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-309">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="ec7d3-309">DXE 5291</span></span> |<span data-ttu-id="ec7d3-310">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-310">Honda</span></span> |<span data-ttu-id="ec7d3-311">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-311">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-312">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="ec7d3-312">YZK 5704</span></span> |<span data-ttu-id="ec7d3-313">Ford</span><span class="sxs-lookup"><span data-stu-id="ec7d3-313">Ford</span></span> |<span data-ttu-id="ec7d3-314">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-314">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-315">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="ec7d3-315">YHN 6970</span></span> |<span data-ttu-id="ec7d3-316">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-316">Toyota</span></span> |<span data-ttu-id="ec7d3-317">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-317">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-318">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="ec7d3-318">QYF 9358</span></span> |<span data-ttu-id="ec7d3-319">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-319">Honda</span></span> |<span data-ttu-id="ec7d3-320">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-320">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-321">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="ec7d3-321">MDR 6128</span></span> |<span data-ttu-id="ec7d3-322">BMW</span><span class="sxs-lookup"><span data-stu-id="ec7d3-322">BMW</span></span> |<span data-ttu-id="ec7d3-323">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-323">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="ec7d3-324">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-324">**Solution**:</span></span>

    SELECT 
        LicensePlate,
        Make,
        Time
    FROM 
        Input TIMESTAMP BY Time
    WHERE 
        IsFirst(minute, 10) OVER (PARTITION BY Make) = 1

## <a name="query-example-find-hello-last-event-in-a-window"></a><span data-ttu-id="ec7d3-325">Frågan exempel: Sök hello sista händelsen i ett fönster</span><span class="sxs-lookup"><span data-stu-id="ec7d3-325">Query example: Find hello last event in a window</span></span>
<span data-ttu-id="ec7d3-326">**Beskrivning**: hitta hello senaste bil i varje 10 minuters intervall.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-326">**Description**: Find hello last car in every 10-minute interval.</span></span>

<span data-ttu-id="ec7d3-327">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-327">**Input**:</span></span>

| <span data-ttu-id="ec7d3-328">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-328">LicensePlate</span></span> | <span data-ttu-id="ec7d3-329">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-329">Make</span></span> | <span data-ttu-id="ec7d3-330">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-330">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-331">DXE 5291</span><span class="sxs-lookup"><span data-stu-id="ec7d3-331">DXE 5291</span></span> |<span data-ttu-id="ec7d3-332">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-332">Honda</span></span> |<span data-ttu-id="ec7d3-333">2015-07-27T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-333">2015-07-27T00:00:05.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-334">YZK 5704</span><span class="sxs-lookup"><span data-stu-id="ec7d3-334">YZK 5704</span></span> |<span data-ttu-id="ec7d3-335">Ford</span><span class="sxs-lookup"><span data-stu-id="ec7d3-335">Ford</span></span> |<span data-ttu-id="ec7d3-336">2015-07-27T00:02:17.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-336">2015-07-27T00:02:17.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-337">RMV 8282</span><span class="sxs-lookup"><span data-stu-id="ec7d3-337">RMV 8282</span></span> |<span data-ttu-id="ec7d3-338">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-338">Honda</span></span> |<span data-ttu-id="ec7d3-339">2015-07-27T00:05:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-339">2015-07-27T00:05:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-340">YHN 6970</span><span class="sxs-lookup"><span data-stu-id="ec7d3-340">YHN 6970</span></span> |<span data-ttu-id="ec7d3-341">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-341">Toyota</span></span> |<span data-ttu-id="ec7d3-342">2015-07-27T00:06:00.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-342">2015-07-27T00:06:00.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-343">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="ec7d3-343">VFE 1616</span></span> |<span data-ttu-id="ec7d3-344">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-344">Toyota</span></span> |<span data-ttu-id="ec7d3-345">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-345">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-346">QYF 9358</span><span class="sxs-lookup"><span data-stu-id="ec7d3-346">QYF 9358</span></span> |<span data-ttu-id="ec7d3-347">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-347">Honda</span></span> |<span data-ttu-id="ec7d3-348">2015-07-27T00:12:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-348">2015-07-27T00:12:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-349">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="ec7d3-349">MDR 6128</span></span> |<span data-ttu-id="ec7d3-350">BMW</span><span class="sxs-lookup"><span data-stu-id="ec7d3-350">BMW</span></span> |<span data-ttu-id="ec7d3-351">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-351">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="ec7d3-352">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-352">**Output**:</span></span>

| <span data-ttu-id="ec7d3-353">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-353">LicensePlate</span></span> | <span data-ttu-id="ec7d3-354">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-354">Make</span></span> | <span data-ttu-id="ec7d3-355">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-355">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-356">VFE 1616</span><span class="sxs-lookup"><span data-stu-id="ec7d3-356">VFE 1616</span></span> |<span data-ttu-id="ec7d3-357">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-357">Toyota</span></span> |<span data-ttu-id="ec7d3-358">2015-07-27T00:09:31.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-358">2015-07-27T00:09:31.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-359">MDR 6128</span><span class="sxs-lookup"><span data-stu-id="ec7d3-359">MDR 6128</span></span> |<span data-ttu-id="ec7d3-360">BMW</span><span class="sxs-lookup"><span data-stu-id="ec7d3-360">BMW</span></span> |<span data-ttu-id="ec7d3-361">2015-07-27T00:13:45.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-361">2015-07-27T00:13:45.0000000Z</span></span> |

<span data-ttu-id="ec7d3-362">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-362">**Solution**:</span></span>

    WITH LastInWindow AS
    (
        SELECT 
            MAX(Time) AS LastEventTime
        FROM 
            Input TIMESTAMP BY Time
        GROUP BY 
            TumblingWindow(minute, 10)
    )
    SELECT 
        Input.LicensePlate,
        Input.Make,
        Input.Time
    FROM
        Input TIMESTAMP BY Time 
        INNER JOIN LastInWindow
        ON DATEDIFF(minute, Input, LastInWindow) BETWEEN 0 AND 10
        AND Input.Time = LastInWindow.LastEventTime

<span data-ttu-id="ec7d3-363">**Förklaring**: det finns två steg i hello-frågan.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-363">**Explanation**: There are two steps in hello query.</span></span> <span data-ttu-id="ec7d3-364">hello första en hittar hello senaste tidsstämpel i windows 10 minuter.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-364">hello first one finds hello latest time stamp in 10-minute windows.</span></span> <span data-ttu-id="ec7d3-365">hello andra steg kopplingar hello resultaten av hello första frågan med hello ursprungliga dataströmmen toofind hello händelser som matchar hello senaste tidsstämplar i varje fönster.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-365">hello second step joins hello results of hello first query with hello original stream toofind hello events that match hello last time stamps in each window.</span></span> 

## <a name="query-example-detect-hello-absence-of-events"></a><span data-ttu-id="ec7d3-366">Frågan exempel: identifiera hello frånvaro av händelser</span><span class="sxs-lookup"><span data-stu-id="ec7d3-366">Query example: Detect hello absence of events</span></span>
<span data-ttu-id="ec7d3-367">**Beskrivning**: Kontrollera att en ström som inte har något värde som matchar vissa villkor.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-367">**Description**: Check that a stream has no value that matches a certain criterion.</span></span>
<span data-ttu-id="ec7d3-368">Till exempel 2 på varandra följande bilar från samma hello angett hello avgift väg inom hello sista 90 sekunder?</span><span class="sxs-lookup"><span data-stu-id="ec7d3-368">For example, have 2 consecutive cars from hello same make entered hello toll road within hello last 90 seconds?</span></span>

<span data-ttu-id="ec7d3-369">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-369">**Input**:</span></span>

| <span data-ttu-id="ec7d3-370">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-370">Make</span></span> | <span data-ttu-id="ec7d3-371">LicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-371">LicensePlate</span></span> | <span data-ttu-id="ec7d3-372">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-372">Time</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-373">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-373">Honda</span></span> |<span data-ttu-id="ec7d3-374">ABC 123</span><span class="sxs-lookup"><span data-stu-id="ec7d3-374">ABC-123</span></span> |<span data-ttu-id="ec7d3-375">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-375">2015-01-01T00:00:01.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-376">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-376">Honda</span></span> |<span data-ttu-id="ec7d3-377">AAA-999</span><span class="sxs-lookup"><span data-stu-id="ec7d3-377">AAA-999</span></span> |<span data-ttu-id="ec7d3-378">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-378">2015-01-01T00:00:02.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-379">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-379">Toyota</span></span> |<span data-ttu-id="ec7d3-380">DEF 987</span><span class="sxs-lookup"><span data-stu-id="ec7d3-380">DEF-987</span></span> |<span data-ttu-id="ec7d3-381">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-381">2015-01-01T00:00:03.0000000Z</span></span> |
| <span data-ttu-id="ec7d3-382">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-382">Honda</span></span> |<span data-ttu-id="ec7d3-383">GHI 345</span><span class="sxs-lookup"><span data-stu-id="ec7d3-383">GHI-345</span></span> |<span data-ttu-id="ec7d3-384">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-384">2015-01-01T00:00:04.0000000Z</span></span> |

<span data-ttu-id="ec7d3-385">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-385">**Output**:</span></span>

| <span data-ttu-id="ec7d3-386">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-386">Make</span></span> | <span data-ttu-id="ec7d3-387">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-387">Time</span></span> | <span data-ttu-id="ec7d3-388">CurrentCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-388">CurrentCarLicensePlate</span></span> | <span data-ttu-id="ec7d3-389">FirstCarLicensePlate</span><span class="sxs-lookup"><span data-stu-id="ec7d3-389">FirstCarLicensePlate</span></span> | <span data-ttu-id="ec7d3-390">FirstCarTime</span><span class="sxs-lookup"><span data-stu-id="ec7d3-390">FirstCarTime</span></span> |
| --- | --- | --- | --- | --- |
| <span data-ttu-id="ec7d3-391">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-391">Honda</span></span> |<span data-ttu-id="ec7d3-392">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-392">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="ec7d3-393">AAA-999</span><span class="sxs-lookup"><span data-stu-id="ec7d3-393">AAA-999</span></span> |<span data-ttu-id="ec7d3-394">ABC 123</span><span class="sxs-lookup"><span data-stu-id="ec7d3-394">ABC-123</span></span> |<span data-ttu-id="ec7d3-395">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-395">2015-01-01T00:00:01.0000000Z</span></span> |

<span data-ttu-id="ec7d3-396">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-396">**Solution**:</span></span>

    SELECT
        Make,
        Time,
        LicensePlate AS CurrentCarLicensePlate,
        LAG(LicensePlate, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarLicensePlate,
        LAG(Time, 1) OVER (LIMIT DURATION(second, 90)) AS FirstCarTime
    FROM
        Input TIMESTAMP BY Time
    WHERE
        LAG(Make, 1) OVER (LIMIT DURATION(second, 90)) = Make

<span data-ttu-id="ec7d3-397">**Förklaring**: Använd **FÖRDRÖJNING** toopeek till hello inkommande dataström tillbaka en händelse och få hello **Se** värde.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-397">**Explanation**: Use **LAG** toopeek into hello input stream one event back and get hello **Make** value.</span></span> <span data-ttu-id="ec7d3-398">Jämför det toohello **Se** värdet i hello händelsen och hello utdatahändelse om de är hello samma.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-398">Compare it toohello **MAKE** value in hello current event, and then output hello event if they are hello same.</span></span> <span data-ttu-id="ec7d3-399">Du kan också använda **FÖRDRÖJNING** tooget data om hello tidigare bil.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-399">You can also use **LAG** tooget data about hello previous car.</span></span>

## <a name="query-example-detect-hello-duration-between-events"></a><span data-ttu-id="ec7d3-400">Frågan exempel: identifiera hello varaktigheten mellan händelser</span><span class="sxs-lookup"><span data-stu-id="ec7d3-400">Query example: Detect hello duration between events</span></span>
<span data-ttu-id="ec7d3-401">**Beskrivning**: hitta hello varaktighet för en given händelse.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-401">**Description**: Find hello duration of a given event.</span></span> <span data-ttu-id="ec7d3-402">Till exempel ges en web clickstream bestämma hello tidsåtgången för en funktion.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-402">For example, given a web clickstream, determine hello time spent on a feature.</span></span>

<span data-ttu-id="ec7d3-403">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-403">**Input**:</span></span>  

| <span data-ttu-id="ec7d3-404">Användare</span><span class="sxs-lookup"><span data-stu-id="ec7d3-404">User</span></span> | <span data-ttu-id="ec7d3-405">Funktion</span><span class="sxs-lookup"><span data-stu-id="ec7d3-405">Feature</span></span> | <span data-ttu-id="ec7d3-406">Händelse</span><span class="sxs-lookup"><span data-stu-id="ec7d3-406">Event</span></span> | <span data-ttu-id="ec7d3-407">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-407">Time</span></span> |
| --- | --- | --- | --- |
| user@location.com |<span data-ttu-id="ec7d3-408">RightMenu</span><span class="sxs-lookup"><span data-stu-id="ec7d3-408">RightMenu</span></span> |<span data-ttu-id="ec7d3-409">Start</span><span class="sxs-lookup"><span data-stu-id="ec7d3-409">Start</span></span> |<span data-ttu-id="ec7d3-410">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-410">2015-01-01T00:00:01.0000000Z</span></span> |
| user@location.com |<span data-ttu-id="ec7d3-411">RightMenu</span><span class="sxs-lookup"><span data-stu-id="ec7d3-411">RightMenu</span></span> |<span data-ttu-id="ec7d3-412">End</span><span class="sxs-lookup"><span data-stu-id="ec7d3-412">End</span></span> |<span data-ttu-id="ec7d3-413">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-413">2015-01-01T00:00:08.0000000Z</span></span> |

<span data-ttu-id="ec7d3-414">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-414">**Output**:</span></span>  

| <span data-ttu-id="ec7d3-415">Användare</span><span class="sxs-lookup"><span data-stu-id="ec7d3-415">User</span></span> | <span data-ttu-id="ec7d3-416">Funktion</span><span class="sxs-lookup"><span data-stu-id="ec7d3-416">Feature</span></span> | <span data-ttu-id="ec7d3-417">Varaktighet</span><span class="sxs-lookup"><span data-stu-id="ec7d3-417">Duration</span></span> |
| --- | --- | --- |
| user@location.com |<span data-ttu-id="ec7d3-418">RightMenu</span><span class="sxs-lookup"><span data-stu-id="ec7d3-418">RightMenu</span></span> |<span data-ttu-id="ec7d3-419">7</span><span class="sxs-lookup"><span data-stu-id="ec7d3-419">7</span></span> |

<span data-ttu-id="ec7d3-420">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-420">**Solution**:</span></span>

````
    SELECT
        [user], feature, DATEDIFF(second, LAST(Time) OVER (PARTITION BY [user], feature LIMIT DURATION(hour, 1) WHEN Event = 'start'), Time) as duration
    FROM input TIMESTAMP BY Time
    WHERE
        Event = 'end'
````

<span data-ttu-id="ec7d3-421">**Förklaring**: Använd hello **senaste** fungerar tooretrieve hello senast **tid** värde när hello händelsetyp **starta**.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-421">**Explanation**: Use hello **LAST** function tooretrieve hello last **TIME** value when hello event type was **Start**.</span></span> <span data-ttu-id="ec7d3-422">Hej **senaste** använder funktionen **PARTITION BY [användare]** tooindicate som hello resultatet beräknas per unika användare.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-422">hello **LAST** function uses **PARTITION BY [user]** tooindicate that hello result is computed per unique user.</span></span> <span data-ttu-id="ec7d3-423">hello-frågan har en maximal tröskel 1 timme för hello tidsskillnaden mellan **starta** och **stoppa** händelser, men kan konfigureras vid behov **(GRÄNSEN DURATION(hour, 1)**.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-423">hello query has a 1-hour maximum threshold for hello time difference between **Start** and **Stop** events, but is configurable as needed **(LIMIT DURATION(hour, 1)**.</span></span>

## <a name="query-example-detect-hello-duration-of-a-condition"></a><span data-ttu-id="ec7d3-424">Frågan exempel: identifiera hello varaktighet för ett villkor</span><span class="sxs-lookup"><span data-stu-id="ec7d3-424">Query example: Detect hello duration of a condition</span></span>
<span data-ttu-id="ec7d3-425">**Beskrivning**: ta reda på hur länge ett villkor inträffade.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-425">**Description**: Find out how long a condition occurred.</span></span>
<span data-ttu-id="ec7d3-426">Anta exempelvis att en bugg resulterade i alla bilar med en felaktig vikt (ovanför 20 000 pund).</span><span class="sxs-lookup"><span data-stu-id="ec7d3-426">For example, suppose that a bug resulted in all cars having an incorrect weight (above 20,000 pounds).</span></span> <span data-ttu-id="ec7d3-427">Vi vill toocompute hello varaktighet hello programfel.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-427">We want toocompute hello duration of hello bug.</span></span>

<span data-ttu-id="ec7d3-428">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-428">**Input**:</span></span>

| <span data-ttu-id="ec7d3-429">Kontrollera</span><span class="sxs-lookup"><span data-stu-id="ec7d3-429">Make</span></span> | <span data-ttu-id="ec7d3-430">Tid</span><span class="sxs-lookup"><span data-stu-id="ec7d3-430">Time</span></span> | <span data-ttu-id="ec7d3-431">Vikt</span><span class="sxs-lookup"><span data-stu-id="ec7d3-431">Weight</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-432">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-432">Honda</span></span> |<span data-ttu-id="ec7d3-433">2015-01-01T00:00:01.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-433">2015-01-01T00:00:01.0000000Z</span></span> |<span data-ttu-id="ec7d3-434">2000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-434">2000</span></span> |
| <span data-ttu-id="ec7d3-435">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-435">Toyota</span></span> |<span data-ttu-id="ec7d3-436">2015-01-01T00:00:02.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-436">2015-01-01T00:00:02.0000000Z</span></span> |<span data-ttu-id="ec7d3-437">25000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-437">25000</span></span> |
| <span data-ttu-id="ec7d3-438">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-438">Honda</span></span> |<span data-ttu-id="ec7d3-439">2015-01-01T00:00:03.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-439">2015-01-01T00:00:03.0000000Z</span></span> |<span data-ttu-id="ec7d3-440">26000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-440">26000</span></span> |
| <span data-ttu-id="ec7d3-441">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-441">Toyota</span></span> |<span data-ttu-id="ec7d3-442">2015-01-01T00:00:04.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-442">2015-01-01T00:00:04.0000000Z</span></span> |<span data-ttu-id="ec7d3-443">25000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-443">25000</span></span> |
| <span data-ttu-id="ec7d3-444">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-444">Honda</span></span> |<span data-ttu-id="ec7d3-445">2015-01-01T00:00:05.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-445">2015-01-01T00:00:05.0000000Z</span></span> |<span data-ttu-id="ec7d3-446">26000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-446">26000</span></span> |
| <span data-ttu-id="ec7d3-447">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-447">Toyota</span></span> |<span data-ttu-id="ec7d3-448">2015-01-01T00:00:06.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-448">2015-01-01T00:00:06.0000000Z</span></span> |<span data-ttu-id="ec7d3-449">25000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-449">25000</span></span> |
| <span data-ttu-id="ec7d3-450">Honda</span><span class="sxs-lookup"><span data-stu-id="ec7d3-450">Honda</span></span> |<span data-ttu-id="ec7d3-451">2015-01-01T00:00:07.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-451">2015-01-01T00:00:07.0000000Z</span></span> |<span data-ttu-id="ec7d3-452">26000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-452">26000</span></span> |
| <span data-ttu-id="ec7d3-453">Toyota</span><span class="sxs-lookup"><span data-stu-id="ec7d3-453">Toyota</span></span> |<span data-ttu-id="ec7d3-454">2015-01-01T00:00:08.0000000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-454">2015-01-01T00:00:08.0000000Z</span></span> |<span data-ttu-id="ec7d3-455">2000</span><span class="sxs-lookup"><span data-stu-id="ec7d3-455">2000</span></span> |

<span data-ttu-id="ec7d3-456">**Utdata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-456">**Output**:</span></span>

| <span data-ttu-id="ec7d3-457">StartFault</span><span class="sxs-lookup"><span data-stu-id="ec7d3-457">StartFault</span></span> | <span data-ttu-id="ec7d3-458">EndFault</span><span class="sxs-lookup"><span data-stu-id="ec7d3-458">EndFault</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-459">2015-01-01T00:00:02.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-459">2015-01-01T00:00:02.000Z</span></span> |<span data-ttu-id="ec7d3-460">2015-01-01T00:00:07.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-460">2015-01-01T00:00:07.000Z</span></span> |

<span data-ttu-id="ec7d3-461">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-461">**Solution**:</span></span>

````
    WITH SelectPreviousEvent AS
    (
    SELECT
    *,
        LAG([time]) OVER (LIMIT DURATION(hour, 24)) as previousTime,
        LAG([weight]) OVER (LIMIT DURATION(hour, 24)) as previousWeight
    FROM input TIMESTAMP BY [time]
    )

    SELECT 
        LAG(time) OVER (LIMIT DURATION(hour, 24) WHEN previousWeight < 20000 ) [StartFault],
        previousTime [EndFault]
    FROM SelectPreviousEvent
    WHERE
        [weight] < 20000
        AND previousWeight > 20000
````

<span data-ttu-id="ec7d3-462">**Förklaring**: Använd **FÖRDRÖJNING** tooview hello Indataströmmen under 24 timmar och leta efter instanser där **StartFault** och **StopFault** omfattas av hello vikt < 20000.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-462">**Explanation**: Use **LAG** tooview hello input stream for 24 hours and look for instances where **StartFault** and **StopFault** are spanned by hello weight < 20000.</span></span>

## <a name="query-example-fill-missing-values"></a><span data-ttu-id="ec7d3-463">Frågan exempel: fylla i saknade värden</span><span class="sxs-lookup"><span data-stu-id="ec7d3-463">Query example: Fill missing values</span></span>
<span data-ttu-id="ec7d3-464">**Beskrivning**: för hello ström av händelser som saknar värden, skapar du en dataström med händelser med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-464">**Description**: For hello stream of events that have missing values, produce a stream of events with regular intervals.</span></span>
<span data-ttu-id="ec7d3-465">Till exempel generera en händelse var femte sekund som rapporter hello senast visade datapunkt.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-465">For example, generate an event every 5 seconds that reports hello most recently seen data point.</span></span>

<span data-ttu-id="ec7d3-466">**Indata**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-466">**Input**:</span></span>

| <span data-ttu-id="ec7d3-467">T</span><span class="sxs-lookup"><span data-stu-id="ec7d3-467">t</span></span> | <span data-ttu-id="ec7d3-468">värde</span><span class="sxs-lookup"><span data-stu-id="ec7d3-468">value</span></span> |
| --- | --- |
| <span data-ttu-id="ec7d3-469">”2014-01-01T06:01:00”</span><span class="sxs-lookup"><span data-stu-id="ec7d3-469">"2014-01-01T06:01:00"</span></span> |<span data-ttu-id="ec7d3-470">1</span><span class="sxs-lookup"><span data-stu-id="ec7d3-470">1</span></span> |
| <span data-ttu-id="ec7d3-471">”2014-01-01T06:01:05”</span><span class="sxs-lookup"><span data-stu-id="ec7d3-471">"2014-01-01T06:01:05"</span></span> |<span data-ttu-id="ec7d3-472">2</span><span class="sxs-lookup"><span data-stu-id="ec7d3-472">2</span></span> |
| <span data-ttu-id="ec7d3-473">”2014-01-01T06:01:10”</span><span class="sxs-lookup"><span data-stu-id="ec7d3-473">"2014-01-01T06:01:10"</span></span> |<span data-ttu-id="ec7d3-474">3</span><span class="sxs-lookup"><span data-stu-id="ec7d3-474">3</span></span> |
| <span data-ttu-id="ec7d3-475">”2014-01-01T06:01:15”</span><span class="sxs-lookup"><span data-stu-id="ec7d3-475">"2014-01-01T06:01:15"</span></span> |<span data-ttu-id="ec7d3-476">4</span><span class="sxs-lookup"><span data-stu-id="ec7d3-476">4</span></span> |
| <span data-ttu-id="ec7d3-477">”2014-01-01T06:01:30”</span><span class="sxs-lookup"><span data-stu-id="ec7d3-477">"2014-01-01T06:01:30"</span></span> |<span data-ttu-id="ec7d3-478">5</span><span class="sxs-lookup"><span data-stu-id="ec7d3-478">5</span></span> |
| <span data-ttu-id="ec7d3-479">”2014-01-01T06:01:35”</span><span class="sxs-lookup"><span data-stu-id="ec7d3-479">"2014-01-01T06:01:35"</span></span> |<span data-ttu-id="ec7d3-480">6</span><span class="sxs-lookup"><span data-stu-id="ec7d3-480">6</span></span> |

<span data-ttu-id="ec7d3-481">**Utdata (första 10 raderna)**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-481">**Output (first 10 rows)**:</span></span>

| <span data-ttu-id="ec7d3-482">windowend</span><span class="sxs-lookup"><span data-stu-id="ec7d3-482">windowend</span></span> | <span data-ttu-id="ec7d3-483">lastevent.t</span><span class="sxs-lookup"><span data-stu-id="ec7d3-483">lastevent.t</span></span> | <span data-ttu-id="ec7d3-484">lastevent.Value</span><span class="sxs-lookup"><span data-stu-id="ec7d3-484">lastevent.value</span></span> |
| --- | --- | --- |
| <span data-ttu-id="ec7d3-485">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-485">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="ec7d3-486">2014-01-01T14:01:00.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-486">2014-01-01T14:01:00.000Z</span></span> |<span data-ttu-id="ec7d3-487">1</span><span class="sxs-lookup"><span data-stu-id="ec7d3-487">1</span></span> |
| <span data-ttu-id="ec7d3-488">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-488">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="ec7d3-489">2014-01-01T14:01:05.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-489">2014-01-01T14:01:05.000Z</span></span> |<span data-ttu-id="ec7d3-490">2</span><span class="sxs-lookup"><span data-stu-id="ec7d3-490">2</span></span> |
| <span data-ttu-id="ec7d3-491">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-491">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="ec7d3-492">2014-01-01T14:01:10.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-492">2014-01-01T14:01:10.000Z</span></span> |<span data-ttu-id="ec7d3-493">3</span><span class="sxs-lookup"><span data-stu-id="ec7d3-493">3</span></span> |
| <span data-ttu-id="ec7d3-494">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-494">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="ec7d3-495">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-495">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="ec7d3-496">4</span><span class="sxs-lookup"><span data-stu-id="ec7d3-496">4</span></span> |
| <span data-ttu-id="ec7d3-497">2014-01-01T14:01:20.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-497">2014-01-01T14:01:20.000Z</span></span> |<span data-ttu-id="ec7d3-498">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-498">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="ec7d3-499">4</span><span class="sxs-lookup"><span data-stu-id="ec7d3-499">4</span></span> |
| <span data-ttu-id="ec7d3-500">2014-01-01T14:01:25.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-500">2014-01-01T14:01:25.000Z</span></span> |<span data-ttu-id="ec7d3-501">2014-01-01T14:01:15.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-501">2014-01-01T14:01:15.000Z</span></span> |<span data-ttu-id="ec7d3-502">4</span><span class="sxs-lookup"><span data-stu-id="ec7d3-502">4</span></span> |
| <span data-ttu-id="ec7d3-503">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-503">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="ec7d3-504">2014-01-01T14:01:30.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-504">2014-01-01T14:01:30.000Z</span></span> |<span data-ttu-id="ec7d3-505">5</span><span class="sxs-lookup"><span data-stu-id="ec7d3-505">5</span></span> |
| <span data-ttu-id="ec7d3-506">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-506">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="ec7d3-507">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-507">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="ec7d3-508">6</span><span class="sxs-lookup"><span data-stu-id="ec7d3-508">6</span></span> |
| <span data-ttu-id="ec7d3-509">2014-01-01T14:01:40.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-509">2014-01-01T14:01:40.000Z</span></span> |<span data-ttu-id="ec7d3-510">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-510">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="ec7d3-511">6</span><span class="sxs-lookup"><span data-stu-id="ec7d3-511">6</span></span> |
| <span data-ttu-id="ec7d3-512">2014-01-01T14:01:45.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-512">2014-01-01T14:01:45.000Z</span></span> |<span data-ttu-id="ec7d3-513">2014-01-01T14:01:35.000Z</span><span class="sxs-lookup"><span data-stu-id="ec7d3-513">2014-01-01T14:01:35.000Z</span></span> |<span data-ttu-id="ec7d3-514">6</span><span class="sxs-lookup"><span data-stu-id="ec7d3-514">6</span></span> |

<span data-ttu-id="ec7d3-515">**Lösningen**:</span><span class="sxs-lookup"><span data-stu-id="ec7d3-515">**Solution**:</span></span>

    SELECT
        System.Timestamp AS windowEnd,
        TopOne() OVER (ORDER BY t DESC) AS lastEvent
    FROM
        input TIMESTAMP BY t
    GROUP BY HOPPINGWINDOW(second, 300, 5)


<span data-ttu-id="ec7d3-516">**Förklaring**: den här frågan genererar händelser var femte sekund och utdata hello senaste händelse som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="ec7d3-516">**Explanation**: This query generates events every 5 seconds and outputs hello last event that was received previously.</span></span> <span data-ttu-id="ec7d3-517">Hej [Hopping fönstret](https://msdn.microsoft.com/library/dn835041.aspx "Hopping fönstret--Azure Stream Analytics") varaktighet anger hur långt tillbaka hello fråga Se toofind hello senaste händelse (300 sekunder i det här exemplet).</span><span class="sxs-lookup"><span data-stu-id="ec7d3-517">hello [Hopping window](https://msdn.microsoft.com/library/dn835041.aspx "Hopping window--Azure Stream Analytics") duration determines how far back hello query looks toofind hello latest event (300 seconds in this example).</span></span>

## <a name="get-help"></a><span data-ttu-id="ec7d3-518">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="ec7d3-518">Get help</span></span>
<span data-ttu-id="ec7d3-519">För ytterligare hjälp försök vår [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span><span class="sxs-lookup"><span data-stu-id="ec7d3-519">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ec7d3-520">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec7d3-520">Next steps</span></span>
* [<span data-ttu-id="ec7d3-521">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ec7d3-521">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="ec7d3-522">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="ec7d3-522">Get started using Azure Stream Analytics</span></span>](stream-analytics-real-time-fraud-detection.md)
* [<span data-ttu-id="ec7d3-523">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="ec7d3-523">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="ec7d3-524">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="ec7d3-524">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="ec7d3-525">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="ec7d3-525">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

