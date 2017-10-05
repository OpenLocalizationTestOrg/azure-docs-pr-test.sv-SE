---
title: "Azure API för fakturering Enterprise - fakturering punkter | Microsoft Docs"
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
ms.openlocfilehash: c6880b79189e0683387a7aafbd6fa4805b3b42ef
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a><span data-ttu-id="e609f-103">Reporting API: er för företagskunder - fakturering punkter</span><span class="sxs-lookup"><span data-stu-id="e609f-103">Reporting APIs for Enterprise customers - Billing Periods</span></span>

<span data-ttu-id="e609f-104">API: et för fakturering perioder returnerar en lista över fakturering punkter som har förbrukningsdata för den angivna registreringen i omvänd kronologisk ordning.</span><span class="sxs-lookup"><span data-stu-id="e609f-104">The Billing Periods API returns a list of billing periods that have consumption data for the specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="e609f-105">Varje Period innehåller en egenskap som pekar på API-väg för de fyra datauppsättningarna - BalanceSummary, UsageDetails, Marktplace kostnader och Prisdokument.</span><span class="sxs-lookup"><span data-stu-id="e609f-105">Each Period contains a property pointing to the API route for the four sets of data - BalanceSummary, UsageDetails, Marktplace Charges, and PriceSheet.</span></span> <span data-ttu-id="e609f-106">Om perioden inte har data är motsvarande egenskap null.</span><span class="sxs-lookup"><span data-stu-id="e609f-106">If the period does not have data, the corresponding property is null.</span></span> 


##<a name="request"></a><span data-ttu-id="e609f-107">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="e609f-107">Request</span></span> 
<span data-ttu-id="e609f-108">Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="e609f-108">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> 

|<span data-ttu-id="e609f-109">Metod</span><span class="sxs-lookup"><span data-stu-id="e609f-109">Method</span></span> | <span data-ttu-id="e609f-110">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="e609f-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="e609f-111">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="e609f-111">GET</span></span>| <span data-ttu-id="e609f-112">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / billingperiods</span><span class="sxs-lookup"><span data-stu-id="e609f-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingperiods</span></span>|

> [!Note]
> <span data-ttu-id="e609f-113">Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.</span><span class="sxs-lookup"><span data-stu-id="e609f-113">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="e609f-114">Svar</span><span class="sxs-lookup"><span data-stu-id="e609f-114">Response</span></span>
 
    
    
      [
            {
                "billingPeriodId": "201704",
                "billingStart": "2017-04-01T00:00:00Z",
                "billingEnd": "2017-04-30T11:59:59Z",
                "balanceSummary": "/v1/enrollments/100/billingperiods/201704/balancesummary",
                "usageDetails": "/v1/enrollments/100/billingperiods/201704/usagedetails",
                "marketplaceCharges": "/v1/enrollments/100/billingperiods/201704/marketplacecharges",
                "priceSheet": "/v1/enrollments/100/billingperiods/201704/pricesheet"
            },          
            ....
      ]
    

<span data-ttu-id="e609f-115">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="e609f-115">**Response property definitions**</span></span>

|<span data-ttu-id="e609f-116">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="e609f-116">Property Name</span></span>| <span data-ttu-id="e609f-117">Typ</span><span class="sxs-lookup"><span data-stu-id="e609f-117">Type</span></span>| <span data-ttu-id="e609f-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e609f-118">Description</span></span>
|-|-|-|
|<span data-ttu-id="e609f-119">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="e609f-119">billingPeriodId</span></span>| <span data-ttu-id="e609f-120">Sträng</span><span class="sxs-lookup"><span data-stu-id="e609f-120">string</span></span>| <span data-ttu-id="e609f-121">Unikt Id som representerar en viss period för fakturering</span><span class="sxs-lookup"><span data-stu-id="e609f-121">The unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="e609f-122">billingStart</span><span class="sxs-lookup"><span data-stu-id="e609f-122">billingStart</span></span>| <span data-ttu-id="e609f-123">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="e609f-123">datetime</span></span>| <span data-ttu-id="e609f-124">ISO 8601-sträng som representerar perioden startdatum</span><span class="sxs-lookup"><span data-stu-id="e609f-124">ISO 8601 string representing the period start date</span></span>|
|<span data-ttu-id="e609f-125">billingEnd</span><span class="sxs-lookup"><span data-stu-id="e609f-125">billingEnd</span></span>| <span data-ttu-id="e609f-126">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="e609f-126">datetime</span></span>| <span data-ttu-id="e609f-127">ISO 8601-sträng som representerar slutdatumet</span><span class="sxs-lookup"><span data-stu-id="e609f-127">ISO 8601 string representing the period end date</span></span>|
|<span data-ttu-id="e609f-128">balanceSummary</span><span class="sxs-lookup"><span data-stu-id="e609f-128">balanceSummary</span></span>| <span data-ttu-id="e609f-129">Sträng</span><span class="sxs-lookup"><span data-stu-id="e609f-129">string</span></span>| <span data-ttu-id="e609f-130">URL-sökvägen som omdirigerar till saldo sammanfattning-data för den här perioden</span><span class="sxs-lookup"><span data-stu-id="e609f-130">The URL path that routes to the Balance Summary data for this period</span></span>|
|<span data-ttu-id="e609f-131">usageDetails</span><span class="sxs-lookup"><span data-stu-id="e609f-131">usageDetails</span></span>| <span data-ttu-id="e609f-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="e609f-132">string</span></span>| <span data-ttu-id="e609f-133">URL-sökvägen som omdirigerar till användningsinformation data för denna period</span><span class="sxs-lookup"><span data-stu-id="e609f-133">The URL path that routes to the Usage Details data for this period</span></span>|
|<span data-ttu-id="e609f-134">marketplaceCharges</span><span class="sxs-lookup"><span data-stu-id="e609f-134">marketplaceCharges</span></span>| <span data-ttu-id="e609f-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="e609f-135">string</span></span>| <span data-ttu-id="e609f-136">URL-sökvägen som omdirigerar till Marketplace-debiteringar data för denna period</span><span class="sxs-lookup"><span data-stu-id="e609f-136">The URL path that routes to the Marketplace Charges data for this period</span></span>|
|<span data-ttu-id="e609f-137">Prisdokument</span><span class="sxs-lookup"><span data-stu-id="e609f-137">priceSheet</span></span>| <span data-ttu-id="e609f-138">Sträng</span><span class="sxs-lookup"><span data-stu-id="e609f-138">string</span></span>| <span data-ttu-id="e609f-139">URL-sökvägen som omdirigerar till Prisdokument data för denna period</span><span class="sxs-lookup"><span data-stu-id="e609f-139">The URL path that routes to the PriceSheet data for this period</span></span>|

<br/>
## <a name="see-also"></a><span data-ttu-id="e609f-140">Se även</span><span class="sxs-lookup"><span data-stu-id="e609f-140">See also</span></span>

* [<span data-ttu-id="e609f-141">Belastningsutjämning och sammanfattning API</span><span class="sxs-lookup"><span data-stu-id="e609f-141">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="e609f-142">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="e609f-142">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="e609f-143">Marketplace Store kostnad API</span><span class="sxs-lookup"><span data-stu-id="e609f-143">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="e609f-144">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="e609f-144">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)