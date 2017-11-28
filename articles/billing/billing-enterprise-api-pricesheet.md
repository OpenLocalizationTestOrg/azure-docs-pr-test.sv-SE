---
title: Azure Billing Enterprise API - Prisdokument | Microsoft Docs
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
ms.openlocfilehash: 2e7d6e883abe4cee13bc5f684baf2a1ea9c6c397
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a><span data-ttu-id="aea47-103">Reporting API: er för företagskunder - Prisdokument</span><span class="sxs-lookup"><span data-stu-id="aea47-103">Reporting APIs for Enterprise customers - Price Sheet</span></span>

<span data-ttu-id="aea47-104">Price Sheet API ger den tillämpliga hastigheten för varje mätaren för den angivna registrering och fakturering Period.</span><span class="sxs-lookup"><span data-stu-id="aea47-104">The Price Sheet API provides the applicable rate for each Meter for the given Enrollment and Billing Period.</span></span>

##<a name="request"></a><span data-ttu-id="aea47-105">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="aea47-105">Request</span></span>
<span data-ttu-id="aea47-106">Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="aea47-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="aea47-107">Om en faktureringsperiod anges returnerade data för den aktuella faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="aea47-107">If a billing period is not specified, then data for the current billing period is returned.</span></span>

|<span data-ttu-id="aea47-108">Metod</span><span class="sxs-lookup"><span data-stu-id="aea47-108">Method</span></span> | <span data-ttu-id="aea47-109">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="aea47-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="aea47-110">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="aea47-110">GET</span></span>|<span data-ttu-id="aea47-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / prisdokument</span><span class="sxs-lookup"><span data-stu-id="aea47-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet</span></span>|
|<span data-ttu-id="aea47-112">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="aea47-112">GET</span></span>|<span data-ttu-id="aea47-113">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / prisdokument</span><span class="sxs-lookup"><span data-stu-id="aea47-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet</span></span>|

> [!Note]
> <span data-ttu-id="aea47-114">Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.</span><span class="sxs-lookup"><span data-stu-id="aea47-114">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="aea47-115">Svar</span><span class="sxs-lookup"><span data-stu-id="aea47-115">Response</span></span>

    
        [
            {
                "id": "enrollments/57354989/billingperiods/201601/products/343/pricesheets",
                "billingPeriodId": "201704",
                "meterId": "dc210ecb-97e8-4522-8134-2385494233c0",
                "meterName": "A1 VM",
                "unitOfMeasure": "100 Hours",
                "includedQuantity": 0,
                "partNumber": "N7H-00015",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            {
                "id": "enrollments/57354989/billingperiods/201601/products/2884/pricesheets",
                "billingPeriodId": "201404",
                "meterId": "dc210ecb-97e8-4522-8134-5385494233c0",
                "meterName": "Locally Redundant Storage Premium Storage - Snapshots - AU East",
                "unitOfMeasure": "100 GB",
                "includedQuantity": 0,
                "partNumber": "N9H-00402",
                "unitPrice": 0.00,
                "currencyCode": "USD"
            },
            ...
        ]
    

> [!Note]
><span data-ttu-id="aea47-116">MeterId fältet är inte tillgängligt om du använder API: et för förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="aea47-116">If you are using the Preview API, meterId field is not available.</span></span>
>

<span data-ttu-id="aea47-117">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="aea47-117">**Response property definitions**</span></span>

|<span data-ttu-id="aea47-118">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="aea47-118">Property Name</span></span>| <span data-ttu-id="aea47-119">Typ</span><span class="sxs-lookup"><span data-stu-id="aea47-119">Type</span></span>| <span data-ttu-id="aea47-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="aea47-120">Description</span></span>
|-|-|-|
|<span data-ttu-id="aea47-121">id</span><span class="sxs-lookup"><span data-stu-id="aea47-121">id</span></span>| <span data-ttu-id="aea47-122">Sträng</span><span class="sxs-lookup"><span data-stu-id="aea47-122">string</span></span>| <span data-ttu-id="aea47-123">Unikt Id som representerar ett visst Prisdokument objekt (mätaren av fakturering period)</span><span class="sxs-lookup"><span data-stu-id="aea47-123">The unique Id that represents a particular PriceSheet item (meter by billing period)</span></span>|
|<span data-ttu-id="aea47-124">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="aea47-124">billingPeriodId</span></span>| <span data-ttu-id="aea47-125">Sträng</span><span class="sxs-lookup"><span data-stu-id="aea47-125">string</span></span>| <span data-ttu-id="aea47-126">Unikt Id som representerar en viss period för fakturering</span><span class="sxs-lookup"><span data-stu-id="aea47-126">The unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="aea47-127">meterId</span><span class="sxs-lookup"><span data-stu-id="aea47-127">meterId</span></span>| <span data-ttu-id="aea47-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="aea47-128">string</span></span>| <span data-ttu-id="aea47-129">Identifierare för mätaren.</span><span class="sxs-lookup"><span data-stu-id="aea47-129">The identifier for the meter.</span></span> <span data-ttu-id="aea47-130">Det går att mappa till meterId för användning.</span><span class="sxs-lookup"><span data-stu-id="aea47-130">It can be mapped to the usage meterId.</span></span>|
|<span data-ttu-id="aea47-131">meterName</span><span class="sxs-lookup"><span data-stu-id="aea47-131">meterName</span></span>| <span data-ttu-id="aea47-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="aea47-132">string</span></span>| <span data-ttu-id="aea47-133">Namnet på mätaren</span><span class="sxs-lookup"><span data-stu-id="aea47-133">The meter name</span></span>|
|<span data-ttu-id="aea47-134">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="aea47-134">unitOfMeasure</span></span>| <span data-ttu-id="aea47-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="aea47-135">string</span></span>| <span data-ttu-id="aea47-136">Enhet för mätning av tjänsten</span><span class="sxs-lookup"><span data-stu-id="aea47-136">The Unit of Measure for measuring the service</span></span>|
|<span data-ttu-id="aea47-137">includedQuantity</span><span class="sxs-lookup"><span data-stu-id="aea47-137">includedQuantity</span></span>| <span data-ttu-id="aea47-138">Decimal</span><span class="sxs-lookup"><span data-stu-id="aea47-138">decimal</span></span>| <span data-ttu-id="aea47-139">Antal som ingår</span><span class="sxs-lookup"><span data-stu-id="aea47-139">Quantity that is included</span></span> |
|<span data-ttu-id="aea47-140">partNumber</span><span class="sxs-lookup"><span data-stu-id="aea47-140">partNumber</span></span>| <span data-ttu-id="aea47-141">Sträng</span><span class="sxs-lookup"><span data-stu-id="aea47-141">string</span></span>| <span data-ttu-id="aea47-142">Artikelnumret som är associerad med mätaren</span><span class="sxs-lookup"><span data-stu-id="aea47-142">The part number associated with the Meter</span></span>|
|<span data-ttu-id="aea47-143">Enhetspris</span><span class="sxs-lookup"><span data-stu-id="aea47-143">unitPrice</span></span>| <span data-ttu-id="aea47-144">Decimal</span><span class="sxs-lookup"><span data-stu-id="aea47-144">decimal</span></span>| <span data-ttu-id="aea47-145">Enhetspriset för mätaren</span><span class="sxs-lookup"><span data-stu-id="aea47-145">The unit price for the meter</span></span>|
|<span data-ttu-id="aea47-146">currencyCode</span><span class="sxs-lookup"><span data-stu-id="aea47-146">currencyCode</span></span>| <span data-ttu-id="aea47-147">Sträng</span><span class="sxs-lookup"><span data-stu-id="aea47-147">string</span></span>| <span data-ttu-id="aea47-148">Koden för Enhetspris</span><span class="sxs-lookup"><span data-stu-id="aea47-148">The currency code for the unitPrice</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="aea47-149">Se även</span><span class="sxs-lookup"><span data-stu-id="aea47-149">See also</span></span>

* [<span data-ttu-id="aea47-150">Fakturering punkter API</span><span class="sxs-lookup"><span data-stu-id="aea47-150">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="aea47-151">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="aea47-151">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md)

* [<span data-ttu-id="aea47-152">Belastningsutjämning och sammanfattning API</span><span class="sxs-lookup"><span data-stu-id="aea47-152">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="aea47-153">Marketplace Store kostnad API</span><span class="sxs-lookup"><span data-stu-id="aea47-153">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md)
