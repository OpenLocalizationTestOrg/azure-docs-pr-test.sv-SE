---
title: Azure Billing Enterprise API - Marketplace-debiteringar | Microsoft Docs
description: "Läs mer om Reporting API: erna som gör att företag Azure-kunder att dra förbrukningsdata programmässigt."
services: 
documentationcenter: 
author: aedwin
manager: aedwin
editor: 
tags: billing
ms.assetid: 3e817b43-0696-400c-a02e-47b7817f9b77
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 04/25/2017
ms.author: aedwin
ms.openlocfilehash: 5539623f7ae35e14b6dafe6fdf9efe4bcaba4fd3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a><span data-ttu-id="d5b04-103">Reporting API: er för företagskunder - Marketplace Store kostnad</span><span class="sxs-lookup"><span data-stu-id="d5b04-103">Reporting APIs for Enterprise customers - Marketplace Store Charge</span></span>

<span data-ttu-id="d5b04-104">Marketplace Store kostnad API returnerar användningsbaserad marketplace avgifter sammanställning per dag för fakturering tidsperioden eller start- och slutdatum (en gång avgifter inte ingår).</span><span class="sxs-lookup"><span data-stu-id="d5b04-104">The Marketplace Store Charge API returns the usage-based marketplace charges breakdown by day for the specified Billing Period or start and end dates (one time fees are not included).</span></span>

##<a name="request"></a><span data-ttu-id="d5b04-105">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="d5b04-105">Request</span></span> 
<span data-ttu-id="d5b04-106">Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="d5b04-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="d5b04-107">Om en faktureringsperiod anges returnerade data för den aktuella faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="d5b04-107">If a billing period is not specified, then data for the current billing period is returned.</span></span> <span data-ttu-id="d5b04-108">Anpassade tidsintervall kan anges med början och slutar datumparametrar som är i formatet ÅÅÅÅ-MM-dd, högsta tidsintervallet som stöds är 36 månader.</span><span class="sxs-lookup"><span data-stu-id="d5b04-108">Custom time ranges can be specified with the start and end date parameters that are in the format yyyy-MM-dd, the maximum supported time range is 36 months.</span></span>  

|<span data-ttu-id="d5b04-109">Metod</span><span class="sxs-lookup"><span data-stu-id="d5b04-109">Method</span></span> | <span data-ttu-id="d5b04-110">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="d5b04-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="d5b04-111">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="d5b04-111">GET</span></span>|<span data-ttu-id="d5b04-112">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="d5b04-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacecharges</span></span>|
|<span data-ttu-id="d5b04-113">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="d5b04-113">GET</span></span>|<span data-ttu-id="d5b04-114">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="d5b04-114">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/marketplacecharges</span></span>|
|<span data-ttu-id="d5b04-115">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="d5b04-115">GET</span></span>|<span data-ttu-id="d5b04-116">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 10-01-2017</span><span class="sxs-lookup"><span data-stu-id="d5b04-116">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacechargesbycustomdate?startTime=2017-01-01&endTime=2017-01-10</span></span>|

> [!Note]
> <span data-ttu-id="d5b04-117">Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.</span><span class="sxs-lookup"><span data-stu-id="d5b04-117">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="d5b04-118">Svar</span><span class="sxs-lookup"><span data-stu-id="d5b04-118">Response</span></span>
 
    
        [
            {
                "id": "id",
                "subscriptionGuid": "00000000-0000-0000-0000-000000000000",
                "subscriptionName": "subName",
                "meterId": "2core",
                "usageStartDate": "2015-09-17T00:00:00Z",
                "usageEndDate": "2015-09-17T23:59:59Z",
                "offerName": "Virtual LoadMaster™ (VLM) for Azure",
                "resourceGroup": "Res group",
                "instanceId": "id",
                "additionalInfo": "{\"ImageType\":null,\"ServiceType\":\"Medium\"}",
                "tags": "",
                "orderNumber": "order",
                "unitOfMeasure": "",
                "costCenter": "100",
                "accountId": 100,
                "accountName": "Account Name",
                "accountOwnerId": "account@live.com",
                "departmentId": 101,
                "departmentName": "Department 1",
                "publisherName": "Publisher 1",
                "planName": "Plan name",
                "consumedQuantity": 1.15,
                "resourceRate": 0.1,
                "extendedCost": 1.11
            },
            ...
        ]
    

<span data-ttu-id="d5b04-119">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="d5b04-119">**Response property definitions**</span></span>

|<span data-ttu-id="d5b04-120">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="d5b04-120">Property Name</span></span>| <span data-ttu-id="d5b04-121">Typ</span><span class="sxs-lookup"><span data-stu-id="d5b04-121">Type</span></span>| <span data-ttu-id="d5b04-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="d5b04-122">Description</span></span>
|-|-|-|
|<span data-ttu-id="d5b04-123">id</span><span class="sxs-lookup"><span data-stu-id="d5b04-123">id</span></span>|<span data-ttu-id="d5b04-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-124">string</span></span>|<span data-ttu-id="d5b04-125">Unikt Id för kostnad marketplace-objektet</span><span class="sxs-lookup"><span data-stu-id="d5b04-125">Unique Id for the marketplace charge item</span></span>|
|<span data-ttu-id="d5b04-126">subscriptionGuid</span><span class="sxs-lookup"><span data-stu-id="d5b04-126">subscriptionGuid</span></span>|<span data-ttu-id="d5b04-127">GUID</span><span class="sxs-lookup"><span data-stu-id="d5b04-127">Guid</span></span>|<span data-ttu-id="d5b04-128">Guid för prenumeration</span><span class="sxs-lookup"><span data-stu-id="d5b04-128">The Subscription Guid</span></span>|
|<span data-ttu-id="d5b04-129">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="d5b04-129">subscriptionName</span></span>|<span data-ttu-id="d5b04-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-130">string</span></span>|<span data-ttu-id="d5b04-131">Prenumerationsnamnet</span><span class="sxs-lookup"><span data-stu-id="d5b04-131">The Subscription Name</span></span>|
|<span data-ttu-id="d5b04-132">meterId</span><span class="sxs-lookup"><span data-stu-id="d5b04-132">meterId</span></span>|<span data-ttu-id="d5b04-133">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-133">string</span></span>|<span data-ttu-id="d5b04-134">ID för den angivna mätaren</span><span class="sxs-lookup"><span data-stu-id="d5b04-134">Id for the emitted Meter</span></span>|
|<span data-ttu-id="d5b04-135">usageStartDate</span><span class="sxs-lookup"><span data-stu-id="d5b04-135">usageStartDate</span></span>|<span data-ttu-id="d5b04-136">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="d5b04-136">DateTime</span></span>|<span data-ttu-id="d5b04-137">Starttid för posten i användning</span><span class="sxs-lookup"><span data-stu-id="d5b04-137">Start time for the usage record</span></span>|
|<span data-ttu-id="d5b04-138">usageEndDate</span><span class="sxs-lookup"><span data-stu-id="d5b04-138">usageEndDate</span></span>|<span data-ttu-id="d5b04-139">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="d5b04-139">DateTime</span></span>|<span data-ttu-id="d5b04-140">Sluttid för posten användning</span><span class="sxs-lookup"><span data-stu-id="d5b04-140">End time for the usage record</span></span>|
|<span data-ttu-id="d5b04-141">offerName</span><span class="sxs-lookup"><span data-stu-id="d5b04-141">offerName</span></span>|<span data-ttu-id="d5b04-142">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-142">string</span></span>|<span data-ttu-id="d5b04-143">Namnet på erbjudande</span><span class="sxs-lookup"><span data-stu-id="d5b04-143">The Offer name</span></span>|
|<span data-ttu-id="d5b04-144">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="d5b04-144">resourceGroup</span></span>|<span data-ttu-id="d5b04-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-145">string</span></span>|<span data-ttu-id="d5b04-146">Resursen grupp</span><span class="sxs-lookup"><span data-stu-id="d5b04-146">The resource Group</span></span>|
|<span data-ttu-id="d5b04-147">InstanceId</span><span class="sxs-lookup"><span data-stu-id="d5b04-147">instanceId</span></span>|<span data-ttu-id="d5b04-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-148">string</span></span>|<span data-ttu-id="d5b04-149">Instans-Id</span><span class="sxs-lookup"><span data-stu-id="d5b04-149">Instance Id</span></span>|
|<span data-ttu-id="d5b04-150">additionalInfo</span><span class="sxs-lookup"><span data-stu-id="d5b04-150">additionalInfo</span></span>|<span data-ttu-id="d5b04-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-151">string</span></span>|<span data-ttu-id="d5b04-152">Ytterligare information JSON-strängen</span><span class="sxs-lookup"><span data-stu-id="d5b04-152">Additional info JSON string</span></span>|
|<span data-ttu-id="d5b04-153">tags</span><span class="sxs-lookup"><span data-stu-id="d5b04-153">tags</span></span>|<span data-ttu-id="d5b04-154">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-154">string</span></span>|<span data-ttu-id="d5b04-155">Taggen JSON-strängen</span><span class="sxs-lookup"><span data-stu-id="d5b04-155">Tag JSON string</span></span>|
|<span data-ttu-id="d5b04-156">orderNumber</span><span class="sxs-lookup"><span data-stu-id="d5b04-156">orderNumber</span></span>|<span data-ttu-id="d5b04-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-157">string</span></span>|<span data-ttu-id="d5b04-158">Ordningstalet</span><span class="sxs-lookup"><span data-stu-id="d5b04-158">The order number</span></span>|
|<span data-ttu-id="d5b04-159">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="d5b04-159">unitOfMeasure</span></span>|<span data-ttu-id="d5b04-160">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-160">string</span></span>|<span data-ttu-id="d5b04-161">Enheten för mätaren</span><span class="sxs-lookup"><span data-stu-id="d5b04-161">Unit of measure for the meter</span></span>|
|<span data-ttu-id="d5b04-162">CostCenter</span><span class="sxs-lookup"><span data-stu-id="d5b04-162">costCenter</span></span>|<span data-ttu-id="d5b04-163">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-163">string</span></span>|<span data-ttu-id="d5b04-164">Kostnadsställe</span><span class="sxs-lookup"><span data-stu-id="d5b04-164">The cost center</span></span>|
|<span data-ttu-id="d5b04-165">accountId</span><span class="sxs-lookup"><span data-stu-id="d5b04-165">accountId</span></span>|<span data-ttu-id="d5b04-166">int</span><span class="sxs-lookup"><span data-stu-id="d5b04-166">int</span></span>|<span data-ttu-id="d5b04-167">Konto-Id</span><span class="sxs-lookup"><span data-stu-id="d5b04-167">The account Id</span></span>|
|<span data-ttu-id="d5b04-168">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="d5b04-168">accountName</span></span>|<span data-ttu-id="d5b04-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-169">string</span></span> |<span data-ttu-id="d5b04-170">Namnet på kontot</span><span class="sxs-lookup"><span data-stu-id="d5b04-170">The Account Name</span></span>|
|<span data-ttu-id="d5b04-171">accountOwnerId</span><span class="sxs-lookup"><span data-stu-id="d5b04-171">accountOwnerId</span></span>|<span data-ttu-id="d5b04-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-172">string</span></span>|<span data-ttu-id="d5b04-173">Ägar-Id för kontot</span><span class="sxs-lookup"><span data-stu-id="d5b04-173">The Account Owner Id</span></span>|
|<span data-ttu-id="d5b04-174">departmentId</span><span class="sxs-lookup"><span data-stu-id="d5b04-174">departmentId</span></span>|<span data-ttu-id="d5b04-175">int</span><span class="sxs-lookup"><span data-stu-id="d5b04-175">int</span></span>|<span data-ttu-id="d5b04-176">Avdelning Id</span><span class="sxs-lookup"><span data-stu-id="d5b04-176">The department Id</span></span>|
|<span data-ttu-id="d5b04-177">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="d5b04-177">departmentName</span></span>|<span data-ttu-id="d5b04-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-178">string</span></span>|<span data-ttu-id="d5b04-179">Ett avdelningsnamn</span><span class="sxs-lookup"><span data-stu-id="d5b04-179">The department name</span></span>|
|<span data-ttu-id="d5b04-180">publisherName</span><span class="sxs-lookup"><span data-stu-id="d5b04-180">publisherName</span></span>|<span data-ttu-id="d5b04-181">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-181">string</span></span>|<span data-ttu-id="d5b04-182">Utgivarens namn</span><span class="sxs-lookup"><span data-stu-id="d5b04-182">The publisher name</span></span>|
|<span data-ttu-id="d5b04-183">planName</span><span class="sxs-lookup"><span data-stu-id="d5b04-183">planName</span></span>|<span data-ttu-id="d5b04-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="d5b04-184">string</span></span>|<span data-ttu-id="d5b04-185">Namn för energischema</span><span class="sxs-lookup"><span data-stu-id="d5b04-185">The Plan name</span></span>|
|<span data-ttu-id="d5b04-186">consumedQuantity</span><span class="sxs-lookup"><span data-stu-id="d5b04-186">consumedQuantity</span></span>|<span data-ttu-id="d5b04-187">Decimal</span><span class="sxs-lookup"><span data-stu-id="d5b04-187">decimal</span></span>|<span data-ttu-id="d5b04-188">Förbrukade antal under den här tiden</span><span class="sxs-lookup"><span data-stu-id="d5b04-188">Consumed Quantity during this time period</span></span>|
|<span data-ttu-id="d5b04-189">resourceRate</span><span class="sxs-lookup"><span data-stu-id="d5b04-189">resourceRate</span></span>|<span data-ttu-id="d5b04-190">Decimal</span><span class="sxs-lookup"><span data-stu-id="d5b04-190">decimal</span></span>|<span data-ttu-id="d5b04-191">Enhetspriset för mätaren</span><span class="sxs-lookup"><span data-stu-id="d5b04-191">Unit price for the meter</span></span>|
|<span data-ttu-id="d5b04-192">extendedCost</span><span class="sxs-lookup"><span data-stu-id="d5b04-192">extendedCost</span></span>|<span data-ttu-id="d5b04-193">Decimal</span><span class="sxs-lookup"><span data-stu-id="d5b04-193">decimal</span></span>|<span data-ttu-id="d5b04-194">Beräknad kostnad som baseras på Förbrukat antal och utökad kostnad</span><span class="sxs-lookup"><span data-stu-id="d5b04-194">Estimated charge based on Consumed Quantity and Extended cost</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="d5b04-195">Se även</span><span class="sxs-lookup"><span data-stu-id="d5b04-195">See also</span></span>

* [<span data-ttu-id="d5b04-196">Fakturering punkter API</span><span class="sxs-lookup"><span data-stu-id="d5b04-196">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="d5b04-197">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="d5b04-197">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="d5b04-198">Belastningsutjämning och sammanfattning API</span><span class="sxs-lookup"><span data-stu-id="d5b04-198">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="d5b04-199">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="d5b04-199">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)