---
title: aaaAzure fakturering Enterprise API - Marketplace-debiteringar | Microsoft Docs
description: "Läs mer om hello Reporting-API: er som gör att Azure Enterprise-kunder toopull förbrukningsdata programmässigt."
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
ms.openlocfilehash: cdf2836b52df06a4bf5ed71a476fe33662c5363c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---marketplace-store-charge"></a><span data-ttu-id="86d5f-103">Reporting API: er för företagskunder - Marketplace Store kostnad</span><span class="sxs-lookup"><span data-stu-id="86d5f-103">Reporting APIs for Enterprise customers - Marketplace Store Charge</span></span>

<span data-ttu-id="86d5f-104">hello Marketplace Store kostnad API returnerar hello användningsbaserad marketplace avgifter sammanställning per dag för hello angivna fakturering Period eller start- och slutdatum (en gång avgifter inte ingår).</span><span class="sxs-lookup"><span data-stu-id="86d5f-104">hello Marketplace Store Charge API returns hello usage-based marketplace charges breakdown by day for hello specified Billing Period or start and end dates (one time fees are not included).</span></span>

##<a name="request"></a><span data-ttu-id="86d5f-105">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="86d5f-105">Request</span></span> 
<span data-ttu-id="86d5f-106">Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="86d5f-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="86d5f-107">Om en faktureringsperiod anges sedan returneras data för hello aktuella fakturering tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="86d5f-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span> <span data-ttu-id="86d5f-108">Anpassade tidsintervall kan anges med hello start och sluta datum parametrarna i hello formatet ÅÅÅÅ-MM-dd hello maximala stöds tid intervallet är 36 månader.</span><span class="sxs-lookup"><span data-stu-id="86d5f-108">Custom time ranges can be specified with hello start and end date parameters that are in hello format yyyy-MM-dd, hello maximum supported time range is 36 months.</span></span>  

|<span data-ttu-id="86d5f-109">Metod</span><span class="sxs-lookup"><span data-stu-id="86d5f-109">Method</span></span> | <span data-ttu-id="86d5f-110">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="86d5f-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="86d5f-111">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="86d5f-111">GET</span></span>|<span data-ttu-id="86d5f-112">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="86d5f-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacecharges</span></span>|
|<span data-ttu-id="86d5f-113">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="86d5f-113">GET</span></span>|<span data-ttu-id="86d5f-114">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / marketplacecharges</span><span class="sxs-lookup"><span data-stu-id="86d5f-114">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/marketplacecharges</span></span>|
|<span data-ttu-id="86d5f-115">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="86d5f-115">GET</span></span>|<span data-ttu-id="86d5f-116">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / marketplacechargesbycustomdate? startTime = 2017-01-01 & endTime = 10-01-2017</span><span class="sxs-lookup"><span data-stu-id="86d5f-116">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/marketplacechargesbycustomdate?startTime=2017-01-01&endTime=2017-01-10</span></span>|

> [!Note]
> <span data-ttu-id="86d5f-117">toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.</span><span class="sxs-lookup"><span data-stu-id="86d5f-117">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="86d5f-118">Svar</span><span class="sxs-lookup"><span data-stu-id="86d5f-118">Response</span></span>
 
    
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
    

<span data-ttu-id="86d5f-119">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="86d5f-119">**Response property definitions**</span></span>

|<span data-ttu-id="86d5f-120">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="86d5f-120">Property Name</span></span>| <span data-ttu-id="86d5f-121">Typ</span><span class="sxs-lookup"><span data-stu-id="86d5f-121">Type</span></span>| <span data-ttu-id="86d5f-122">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="86d5f-122">Description</span></span>
|-|-|-|
|<span data-ttu-id="86d5f-123">id</span><span class="sxs-lookup"><span data-stu-id="86d5f-123">id</span></span>|<span data-ttu-id="86d5f-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-124">string</span></span>|<span data-ttu-id="86d5f-125">Unikt Id för kostnad hello marketplace-objektet</span><span class="sxs-lookup"><span data-stu-id="86d5f-125">Unique Id for hello marketplace charge item</span></span>|
|<span data-ttu-id="86d5f-126">subscriptionGuid</span><span class="sxs-lookup"><span data-stu-id="86d5f-126">subscriptionGuid</span></span>|<span data-ttu-id="86d5f-127">GUID</span><span class="sxs-lookup"><span data-stu-id="86d5f-127">Guid</span></span>|<span data-ttu-id="86d5f-128">hello prenumeration Guid</span><span class="sxs-lookup"><span data-stu-id="86d5f-128">hello Subscription Guid</span></span>|
|<span data-ttu-id="86d5f-129">SubscriptionName</span><span class="sxs-lookup"><span data-stu-id="86d5f-129">subscriptionName</span></span>|<span data-ttu-id="86d5f-130">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-130">string</span></span>|<span data-ttu-id="86d5f-131">hello prenumerationsnamn</span><span class="sxs-lookup"><span data-stu-id="86d5f-131">hello Subscription Name</span></span>|
|<span data-ttu-id="86d5f-132">meterId</span><span class="sxs-lookup"><span data-stu-id="86d5f-132">meterId</span></span>|<span data-ttu-id="86d5f-133">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-133">string</span></span>|<span data-ttu-id="86d5f-134">ID för hello orsakat mätare</span><span class="sxs-lookup"><span data-stu-id="86d5f-134">Id for hello emitted Meter</span></span>|
|<span data-ttu-id="86d5f-135">usageStartDate</span><span class="sxs-lookup"><span data-stu-id="86d5f-135">usageStartDate</span></span>|<span data-ttu-id="86d5f-136">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="86d5f-136">DateTime</span></span>|<span data-ttu-id="86d5f-137">Starttid för hello användning post</span><span class="sxs-lookup"><span data-stu-id="86d5f-137">Start time for hello usage record</span></span>|
|<span data-ttu-id="86d5f-138">usageEndDate</span><span class="sxs-lookup"><span data-stu-id="86d5f-138">usageEndDate</span></span>|<span data-ttu-id="86d5f-139">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="86d5f-139">DateTime</span></span>|<span data-ttu-id="86d5f-140">Sluttid för hello användning post</span><span class="sxs-lookup"><span data-stu-id="86d5f-140">End time for hello usage record</span></span>|
|<span data-ttu-id="86d5f-141">offerName</span><span class="sxs-lookup"><span data-stu-id="86d5f-141">offerName</span></span>|<span data-ttu-id="86d5f-142">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-142">string</span></span>|<span data-ttu-id="86d5f-143">Hej erbjudandenamn</span><span class="sxs-lookup"><span data-stu-id="86d5f-143">hello Offer name</span></span>|
|<span data-ttu-id="86d5f-144">resourceGroup</span><span class="sxs-lookup"><span data-stu-id="86d5f-144">resourceGroup</span></span>|<span data-ttu-id="86d5f-145">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-145">string</span></span>|<span data-ttu-id="86d5f-146">hello resurs grupp</span><span class="sxs-lookup"><span data-stu-id="86d5f-146">hello resource Group</span></span>|
|<span data-ttu-id="86d5f-147">InstanceId</span><span class="sxs-lookup"><span data-stu-id="86d5f-147">instanceId</span></span>|<span data-ttu-id="86d5f-148">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-148">string</span></span>|<span data-ttu-id="86d5f-149">Instans-Id</span><span class="sxs-lookup"><span data-stu-id="86d5f-149">Instance Id</span></span>|
|<span data-ttu-id="86d5f-150">additionalInfo</span><span class="sxs-lookup"><span data-stu-id="86d5f-150">additionalInfo</span></span>|<span data-ttu-id="86d5f-151">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-151">string</span></span>|<span data-ttu-id="86d5f-152">Ytterligare information JSON-strängen</span><span class="sxs-lookup"><span data-stu-id="86d5f-152">Additional info JSON string</span></span>|
|<span data-ttu-id="86d5f-153">tags</span><span class="sxs-lookup"><span data-stu-id="86d5f-153">tags</span></span>|<span data-ttu-id="86d5f-154">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-154">string</span></span>|<span data-ttu-id="86d5f-155">Taggen JSON-strängen</span><span class="sxs-lookup"><span data-stu-id="86d5f-155">Tag JSON string</span></span>|
|<span data-ttu-id="86d5f-156">orderNumber</span><span class="sxs-lookup"><span data-stu-id="86d5f-156">orderNumber</span></span>|<span data-ttu-id="86d5f-157">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-157">string</span></span>|<span data-ttu-id="86d5f-158">hello ordningsnummer</span><span class="sxs-lookup"><span data-stu-id="86d5f-158">hello order number</span></span>|
|<span data-ttu-id="86d5f-159">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="86d5f-159">unitOfMeasure</span></span>|<span data-ttu-id="86d5f-160">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-160">string</span></span>|<span data-ttu-id="86d5f-161">Enheten för hello mätare</span><span class="sxs-lookup"><span data-stu-id="86d5f-161">Unit of measure for hello meter</span></span>|
|<span data-ttu-id="86d5f-162">CostCenter</span><span class="sxs-lookup"><span data-stu-id="86d5f-162">costCenter</span></span>|<span data-ttu-id="86d5f-163">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-163">string</span></span>|<span data-ttu-id="86d5f-164">hello kostnad center</span><span class="sxs-lookup"><span data-stu-id="86d5f-164">hello cost center</span></span>|
|<span data-ttu-id="86d5f-165">accountId</span><span class="sxs-lookup"><span data-stu-id="86d5f-165">accountId</span></span>|<span data-ttu-id="86d5f-166">int</span><span class="sxs-lookup"><span data-stu-id="86d5f-166">int</span></span>|<span data-ttu-id="86d5f-167">hello konto-Id</span><span class="sxs-lookup"><span data-stu-id="86d5f-167">hello account Id</span></span>|
|<span data-ttu-id="86d5f-168">Kontonamn</span><span class="sxs-lookup"><span data-stu-id="86d5f-168">accountName</span></span>|<span data-ttu-id="86d5f-169">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-169">string</span></span> |<span data-ttu-id="86d5f-170">hello kontonamn</span><span class="sxs-lookup"><span data-stu-id="86d5f-170">hello Account Name</span></span>|
|<span data-ttu-id="86d5f-171">accountOwnerId</span><span class="sxs-lookup"><span data-stu-id="86d5f-171">accountOwnerId</span></span>|<span data-ttu-id="86d5f-172">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-172">string</span></span>|<span data-ttu-id="86d5f-173">hello konto ägar-Id</span><span class="sxs-lookup"><span data-stu-id="86d5f-173">hello Account Owner Id</span></span>|
|<span data-ttu-id="86d5f-174">departmentId</span><span class="sxs-lookup"><span data-stu-id="86d5f-174">departmentId</span></span>|<span data-ttu-id="86d5f-175">int</span><span class="sxs-lookup"><span data-stu-id="86d5f-175">int</span></span>|<span data-ttu-id="86d5f-176">hello avdelning Id</span><span class="sxs-lookup"><span data-stu-id="86d5f-176">hello department Id</span></span>|
|<span data-ttu-id="86d5f-177">DepartmentName</span><span class="sxs-lookup"><span data-stu-id="86d5f-177">departmentName</span></span>|<span data-ttu-id="86d5f-178">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-178">string</span></span>|<span data-ttu-id="86d5f-179">hello avdelningsnamn</span><span class="sxs-lookup"><span data-stu-id="86d5f-179">hello department name</span></span>|
|<span data-ttu-id="86d5f-180">publisherName</span><span class="sxs-lookup"><span data-stu-id="86d5f-180">publisherName</span></span>|<span data-ttu-id="86d5f-181">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-181">string</span></span>|<span data-ttu-id="86d5f-182">hello utgivarens namn</span><span class="sxs-lookup"><span data-stu-id="86d5f-182">hello publisher name</span></span>|
|<span data-ttu-id="86d5f-183">planName</span><span class="sxs-lookup"><span data-stu-id="86d5f-183">planName</span></span>|<span data-ttu-id="86d5f-184">Sträng</span><span class="sxs-lookup"><span data-stu-id="86d5f-184">string</span></span>|<span data-ttu-id="86d5f-185">hello namn</span><span class="sxs-lookup"><span data-stu-id="86d5f-185">hello Plan name</span></span>|
|<span data-ttu-id="86d5f-186">consumedQuantity</span><span class="sxs-lookup"><span data-stu-id="86d5f-186">consumedQuantity</span></span>|<span data-ttu-id="86d5f-187">Decimal</span><span class="sxs-lookup"><span data-stu-id="86d5f-187">decimal</span></span>|<span data-ttu-id="86d5f-188">Förbrukade antal under den här tiden</span><span class="sxs-lookup"><span data-stu-id="86d5f-188">Consumed Quantity during this time period</span></span>|
|<span data-ttu-id="86d5f-189">resourceRate</span><span class="sxs-lookup"><span data-stu-id="86d5f-189">resourceRate</span></span>|<span data-ttu-id="86d5f-190">Decimal</span><span class="sxs-lookup"><span data-stu-id="86d5f-190">decimal</span></span>|<span data-ttu-id="86d5f-191">Enhetspriset för hello mätare</span><span class="sxs-lookup"><span data-stu-id="86d5f-191">Unit price for hello meter</span></span>|
|<span data-ttu-id="86d5f-192">extendedCost</span><span class="sxs-lookup"><span data-stu-id="86d5f-192">extendedCost</span></span>|<span data-ttu-id="86d5f-193">Decimal</span><span class="sxs-lookup"><span data-stu-id="86d5f-193">decimal</span></span>|<span data-ttu-id="86d5f-194">Beräknad kostnad som baseras på Förbrukat antal och utökad kostnad</span><span class="sxs-lookup"><span data-stu-id="86d5f-194">Estimated charge based on Consumed Quantity and Extended cost</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="86d5f-195">Se även</span><span class="sxs-lookup"><span data-stu-id="86d5f-195">See also</span></span>

* [<span data-ttu-id="86d5f-196">Fakturering punkter API</span><span class="sxs-lookup"><span data-stu-id="86d5f-196">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="86d5f-197">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="86d5f-197">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="86d5f-198">Belastningsutjämning och sammanfattning API</span><span class="sxs-lookup"><span data-stu-id="86d5f-198">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="86d5f-199">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="86d5f-199">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)