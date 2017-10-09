---
title: aaaAzure fakturering Enterprise API - Prisdokument | Microsoft Docs
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
ms.openlocfilehash: 4cfe694c63fba266d117054b046d9caacb3b7197
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---price-sheet"></a><span data-ttu-id="0c41a-103">Reporting API: er för företagskunder - Prisdokument</span><span class="sxs-lookup"><span data-stu-id="0c41a-103">Reporting APIs for Enterprise customers - Price Sheet</span></span>

<span data-ttu-id="0c41a-104">hello Price Sheet API ger hello tillämpliga hastighet för varje mätaren för hello registrering och fakturering Period.</span><span class="sxs-lookup"><span data-stu-id="0c41a-104">hello Price Sheet API provides hello applicable rate for each Meter for hello given Enrollment and Billing Period.</span></span>

##<a name="request"></a><span data-ttu-id="0c41a-105">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="0c41a-105">Request</span></span>
<span data-ttu-id="0c41a-106">Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="0c41a-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="0c41a-107">Om en faktureringsperiod anges sedan returneras data för hello aktuella fakturering tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="0c41a-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span>

|<span data-ttu-id="0c41a-108">Metod</span><span class="sxs-lookup"><span data-stu-id="0c41a-108">Method</span></span> | <span data-ttu-id="0c41a-109">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="0c41a-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="0c41a-110">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="0c41a-110">GET</span></span>|<span data-ttu-id="0c41a-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / prisdokument</span><span class="sxs-lookup"><span data-stu-id="0c41a-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/pricesheet</span></span>|
|<span data-ttu-id="0c41a-112">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="0c41a-112">GET</span></span>|<span data-ttu-id="0c41a-113">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / prisdokument</span><span class="sxs-lookup"><span data-stu-id="0c41a-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/pricesheet</span></span>|

> [!Note]
> <span data-ttu-id="0c41a-114">toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.</span><span class="sxs-lookup"><span data-stu-id="0c41a-114">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="0c41a-115">Svar</span><span class="sxs-lookup"><span data-stu-id="0c41a-115">Response</span></span>

    
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
><span data-ttu-id="0c41a-116">Om du använder hello Preview API är meterId fältet inte tillgängligt.</span><span class="sxs-lookup"><span data-stu-id="0c41a-116">If you are using hello Preview API, meterId field is not available.</span></span>
>

<span data-ttu-id="0c41a-117">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="0c41a-117">**Response property definitions**</span></span>

|<span data-ttu-id="0c41a-118">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="0c41a-118">Property Name</span></span>| <span data-ttu-id="0c41a-119">Typ</span><span class="sxs-lookup"><span data-stu-id="0c41a-119">Type</span></span>| <span data-ttu-id="0c41a-120">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="0c41a-120">Description</span></span>
|-|-|-|
|<span data-ttu-id="0c41a-121">id</span><span class="sxs-lookup"><span data-stu-id="0c41a-121">id</span></span>| <span data-ttu-id="0c41a-122">Sträng</span><span class="sxs-lookup"><span data-stu-id="0c41a-122">string</span></span>| <span data-ttu-id="0c41a-123">hello unikt Id som representerar ett visst Prisdokument objekt (mätaren av fakturering period)</span><span class="sxs-lookup"><span data-stu-id="0c41a-123">hello unique Id that represents a particular PriceSheet item (meter by billing period)</span></span>|
|<span data-ttu-id="0c41a-124">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="0c41a-124">billingPeriodId</span></span>| <span data-ttu-id="0c41a-125">Sträng</span><span class="sxs-lookup"><span data-stu-id="0c41a-125">string</span></span>| <span data-ttu-id="0c41a-126">hello unikt Id som representerar en viss period för fakturering</span><span class="sxs-lookup"><span data-stu-id="0c41a-126">hello unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="0c41a-127">meterId</span><span class="sxs-lookup"><span data-stu-id="0c41a-127">meterId</span></span>| <span data-ttu-id="0c41a-128">Sträng</span><span class="sxs-lookup"><span data-stu-id="0c41a-128">string</span></span>| <span data-ttu-id="0c41a-129">hello identifierare för hello mätaren.</span><span class="sxs-lookup"><span data-stu-id="0c41a-129">hello identifier for hello meter.</span></span> <span data-ttu-id="0c41a-130">Det kan vara mappade toohello användning meterId.</span><span class="sxs-lookup"><span data-stu-id="0c41a-130">It can be mapped toohello usage meterId.</span></span>|
|<span data-ttu-id="0c41a-131">meterName</span><span class="sxs-lookup"><span data-stu-id="0c41a-131">meterName</span></span>| <span data-ttu-id="0c41a-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="0c41a-132">string</span></span>| <span data-ttu-id="0c41a-133">hello mätaren namn</span><span class="sxs-lookup"><span data-stu-id="0c41a-133">hello meter name</span></span>|
|<span data-ttu-id="0c41a-134">unitOfMeasure</span><span class="sxs-lookup"><span data-stu-id="0c41a-134">unitOfMeasure</span></span>| <span data-ttu-id="0c41a-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="0c41a-135">string</span></span>| <span data-ttu-id="0c41a-136">hello enhet för att mäta hello-tjänsten</span><span class="sxs-lookup"><span data-stu-id="0c41a-136">hello Unit of Measure for measuring hello service</span></span>|
|<span data-ttu-id="0c41a-137">includedQuantity</span><span class="sxs-lookup"><span data-stu-id="0c41a-137">includedQuantity</span></span>| <span data-ttu-id="0c41a-138">Decimal</span><span class="sxs-lookup"><span data-stu-id="0c41a-138">decimal</span></span>| <span data-ttu-id="0c41a-139">Antal som ingår</span><span class="sxs-lookup"><span data-stu-id="0c41a-139">Quantity that is included</span></span> |
|<span data-ttu-id="0c41a-140">partNumber</span><span class="sxs-lookup"><span data-stu-id="0c41a-140">partNumber</span></span>| <span data-ttu-id="0c41a-141">Sträng</span><span class="sxs-lookup"><span data-stu-id="0c41a-141">string</span></span>| <span data-ttu-id="0c41a-142">hello artikelnumret som är associerade med hello mätare</span><span class="sxs-lookup"><span data-stu-id="0c41a-142">hello part number associated with hello Meter</span></span>|
|<span data-ttu-id="0c41a-143">Enhetspris</span><span class="sxs-lookup"><span data-stu-id="0c41a-143">unitPrice</span></span>| <span data-ttu-id="0c41a-144">Decimal</span><span class="sxs-lookup"><span data-stu-id="0c41a-144">decimal</span></span>| <span data-ttu-id="0c41a-145">hello enhetspriset för hello mätare</span><span class="sxs-lookup"><span data-stu-id="0c41a-145">hello unit price for hello meter</span></span>|
|<span data-ttu-id="0c41a-146">currencyCode</span><span class="sxs-lookup"><span data-stu-id="0c41a-146">currencyCode</span></span>| <span data-ttu-id="0c41a-147">Sträng</span><span class="sxs-lookup"><span data-stu-id="0c41a-147">string</span></span>| <span data-ttu-id="0c41a-148">hello valutakoden för hello Enhetspris</span><span class="sxs-lookup"><span data-stu-id="0c41a-148">hello currency code for hello unitPrice</span></span>|
<br/>
## <a name="see-also"></a><span data-ttu-id="0c41a-149">Se även</span><span class="sxs-lookup"><span data-stu-id="0c41a-149">See also</span></span>

* [<span data-ttu-id="0c41a-150">Fakturering punkter API</span><span class="sxs-lookup"><span data-stu-id="0c41a-150">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="0c41a-151">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="0c41a-151">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md)

* [<span data-ttu-id="0c41a-152">Belastningsutjämning och sammanfattning API</span><span class="sxs-lookup"><span data-stu-id="0c41a-152">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="0c41a-153">Marketplace Store kostnad API</span><span class="sxs-lookup"><span data-stu-id="0c41a-153">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md)
