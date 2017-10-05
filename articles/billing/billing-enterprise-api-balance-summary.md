---
title: "Azure Billing Enterprise API - saldot och översikt | Microsoft Docs"
description: "Läs mer om Azure Billing-användning och RateCard APIs som används för att ge insikter om Azure resursförbrukning och trender."
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
ms.openlocfilehash: f6b149f0e656d2263705048aa5b644f5bb4a5712
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a><span data-ttu-id="e3016-103">Reporting API: er för företagskunder - saldot och sammanfattning</span><span class="sxs-lookup"><span data-stu-id="e3016-103">Reporting APIs for Enterprise customers - Balance and Summary</span></span>

<span data-ttu-id="e3016-104">Ger information om saldon, nya inköp, Azure Marketplace serviceavgifter, justeringar och överförbrukning avgifter månatligen saldo och översikt över API.</span><span class="sxs-lookup"><span data-stu-id="e3016-104">The Balance and Summary API offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments, and overage charges.</span></span>


##<a name="request"></a><span data-ttu-id="e3016-105">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="e3016-105">Request</span></span> 
<span data-ttu-id="e3016-106">Allmänna rubrikegenskaper för som ska läggas till anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="e3016-106">Common header properties that need to be added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="e3016-107">Om en faktureringsperiod anges returnerade data för den aktuella faktureringsperioden.</span><span class="sxs-lookup"><span data-stu-id="e3016-107">If a billing period is not specified, then data for the current billing period is returned.</span></span>

|<span data-ttu-id="e3016-108">Metod</span><span class="sxs-lookup"><span data-stu-id="e3016-108">Method</span></span> | <span data-ttu-id="e3016-109">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="e3016-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="e3016-110">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="e3016-110">GET</span></span>| <span data-ttu-id="e3016-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="e3016-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/balancesummary</span></span>|
|<span data-ttu-id="e3016-112">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="e3016-112">GET</span></span>| <span data-ttu-id="e3016-113">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="e3016-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/balancesummary</span></span>|

> [!Note]
> <span data-ttu-id="e3016-114">Ersätt v2 med v1 i URL: en ovan om du vill använda förhandsversionen av API.</span><span class="sxs-lookup"><span data-stu-id="e3016-114">To use the preview version of API, replace v2 with v1 in the above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="e3016-115">Svar</span><span class="sxs-lookup"><span data-stu-id="e3016-115">Response</span></span>

        {
            "id": "enrollments/100/billingperiods/201507/balancesummaries",
            "billingPeriodId": 201507,
            "currencyCode": "USD",
            "beginningBalance": 0,
            "endingBalance": 1.1,
            "newPurchases": 1,
            "adjustments": 1.1,
            "utilized": 1.1,
            "serviceOverage": 1,
            "chargesBilledSeparately": 1,
            "totalOverage": 1,
            "totalUsage": 1.1,
            "azureMarketplaceServiceCharges": 1,
            "newPurchasesDetails": [
                {
                "name": "",
                "value": 1
                }
            ],
            "adjustmentDetails": [
                {
                "name": "Promo Credit",
                "value": 1.1
                },
                {
                "name": "SIE Credit",
                "value": 1.0
                }
            ]
        }


<span data-ttu-id="e3016-116">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="e3016-116">**Response property definitions**</span></span>

|<span data-ttu-id="e3016-117">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="e3016-117">Property Name</span></span>| <span data-ttu-id="e3016-118">Typ</span><span class="sxs-lookup"><span data-stu-id="e3016-118">Type</span></span>| <span data-ttu-id="e3016-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e3016-119">Description</span></span>
|-|-|-|
|<span data-ttu-id="e3016-120">id</span><span class="sxs-lookup"><span data-stu-id="e3016-120">id</span></span>|<span data-ttu-id="e3016-121">Sträng</span><span class="sxs-lookup"><span data-stu-id="e3016-121">string</span></span>|<span data-ttu-id="e3016-122">Unikt Id för en specifik faktureringsperiod och registrering</span><span class="sxs-lookup"><span data-stu-id="e3016-122">The unique Id for a specific billing period and enrollment</span></span>|
|<span data-ttu-id="e3016-123">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="e3016-123">billingPeriodId</span></span>|<span data-ttu-id="e3016-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="e3016-124">string</span></span> |<span data-ttu-id="e3016-125">Faktureringsperiod Id</span><span class="sxs-lookup"><span data-stu-id="e3016-125">The billing period Id</span></span>|
|<span data-ttu-id="e3016-126">currencyCode</span><span class="sxs-lookup"><span data-stu-id="e3016-126">currencyCode</span></span>|<span data-ttu-id="e3016-127">Sträng</span><span class="sxs-lookup"><span data-stu-id="e3016-127">string</span></span> |<span data-ttu-id="e3016-128">Koden</span><span class="sxs-lookup"><span data-stu-id="e3016-128">The currency code</span></span>|
|<span data-ttu-id="e3016-129">beginningBalance</span><span class="sxs-lookup"><span data-stu-id="e3016-129">beginningBalance</span></span>|<span data-ttu-id="e3016-130">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-130">decimal</span></span>| <span data-ttu-id="e3016-131">Det ingående saldot för faktureringsperioden</span><span class="sxs-lookup"><span data-stu-id="e3016-131">The beginning balance for the billing period</span></span>|
|<span data-ttu-id="e3016-132">endingBalance</span><span class="sxs-lookup"><span data-stu-id="e3016-132">endingBalance</span></span>|<span data-ttu-id="e3016-133">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-133">decimal</span></span>| <span data-ttu-id="e3016-134">Den utgående balansen för faktureringsperioden (för öppna perioder detta uppdateras dagligen)</span><span class="sxs-lookup"><span data-stu-id="e3016-134">The ending balance for the billing period (for open periods this will be updated daily)</span></span>|
|<span data-ttu-id="e3016-135">newPurchases</span><span class="sxs-lookup"><span data-stu-id="e3016-135">newPurchases</span></span>|<span data-ttu-id="e3016-136">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-136">decimal</span></span>| <span data-ttu-id="e3016-137">Nya inköp Totalmängd</span><span class="sxs-lookup"><span data-stu-id="e3016-137">Total new purchase amount</span></span>|
|<span data-ttu-id="e3016-138">justering av</span><span class="sxs-lookup"><span data-stu-id="e3016-138">adjustments</span></span>|<span data-ttu-id="e3016-139">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-139">decimal</span></span>| <span data-ttu-id="e3016-140">Total justeringsbelopp</span><span class="sxs-lookup"><span data-stu-id="e3016-140">Total adjustment amount</span></span>|
|<span data-ttu-id="e3016-141">utnyttjade</span><span class="sxs-lookup"><span data-stu-id="e3016-141">utilized</span></span>|<span data-ttu-id="e3016-142">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-142">decimal</span></span>| <span data-ttu-id="e3016-143">Total användning för åtagande</span><span class="sxs-lookup"><span data-stu-id="e3016-143">Total Commitment usage</span></span>|
|<span data-ttu-id="e3016-144">serviceOverage</span><span class="sxs-lookup"><span data-stu-id="e3016-144">serviceOverage</span></span>|<span data-ttu-id="e3016-145">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-145">decimal</span></span>| <span data-ttu-id="e3016-146">Överförbrukning för Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="e3016-146">Overage for Azure services</span></span>|
|<span data-ttu-id="e3016-147">chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="e3016-147">chargesBilledSeparately</span></span>|<span data-ttu-id="e3016-148">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-148">decimal</span></span>| <span data-ttu-id="e3016-149">Avgifter debiteras separat</span><span class="sxs-lookup"><span data-stu-id="e3016-149">Charges Billed separately</span></span>|
|<span data-ttu-id="e3016-150">totalOverage</span><span class="sxs-lookup"><span data-stu-id="e3016-150">totalOverage</span></span>|<span data-ttu-id="e3016-151">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-151">decimal</span></span>| <span data-ttu-id="e3016-152">serviceOverage + chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="e3016-152">serviceOverage + chargesBilledSeparately</span></span>|
|<span data-ttu-id="e3016-153">totalUsage</span><span class="sxs-lookup"><span data-stu-id="e3016-153">totalUsage</span></span>|<span data-ttu-id="e3016-154">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-154">decimal</span></span>| <span data-ttu-id="e3016-155">Azure-tjänsten åtagande + total överförbrukning</span><span class="sxs-lookup"><span data-stu-id="e3016-155">Azure service commitment + total Overage</span></span>|
|<span data-ttu-id="e3016-156">azureMarketplaceServiceCharges</span><span class="sxs-lookup"><span data-stu-id="e3016-156">azureMarketplaceServiceCharges</span></span>|<span data-ttu-id="e3016-157">Decimal</span><span class="sxs-lookup"><span data-stu-id="e3016-157">decimal</span></span>| <span data-ttu-id="e3016-158">Totalt antal kostnader för Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="e3016-158">Total charges for Azure Marketplace</span></span>|
|<span data-ttu-id="e3016-159">newPurchasesDetails</span><span class="sxs-lookup"><span data-stu-id="e3016-159">newPurchasesDetails</span></span>|<span data-ttu-id="e3016-160">JSON-Strängmatrisen av namn-värde-par</span><span class="sxs-lookup"><span data-stu-id="e3016-160">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="e3016-161">Lista över nya inköp</span><span class="sxs-lookup"><span data-stu-id="e3016-161">List of new purchases</span></span>|
|<span data-ttu-id="e3016-162">adjustmentDetails</span><span class="sxs-lookup"><span data-stu-id="e3016-162">adjustmentDetails</span></span>|<span data-ttu-id="e3016-163">JSON-Strängmatrisen av namn-värde-par</span><span class="sxs-lookup"><span data-stu-id="e3016-163">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="e3016-164">Lista över justeringar (kampanj kredit, SIE kredit osv.)</span><span class="sxs-lookup"><span data-stu-id="e3016-164">List of Adjustments (Promo credit, SIE credit etc.)</span></span> |


<br/>
## <a name="see-also"></a><span data-ttu-id="e3016-165">Se även</span><span class="sxs-lookup"><span data-stu-id="e3016-165">See also</span></span>

* [<span data-ttu-id="e3016-166">Fakturering punkter API</span><span class="sxs-lookup"><span data-stu-id="e3016-166">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="e3016-167">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="e3016-167">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="e3016-168">Marketplace Store kostnad API</span><span class="sxs-lookup"><span data-stu-id="e3016-168">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="e3016-169">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="e3016-169">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)