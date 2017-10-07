---
title: "aaaAzure fakturering Enterprise API - saldot och översikt | Microsoft Docs"
description: "Läs mer om Azure Billing användnings- och RateCard APIs som används tooprovide insikter om Azure resursförbrukning och trender."
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
ms.openlocfilehash: b031de2c347e5abeacd11743cc96024434518918
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---balance-and-summary"></a><span data-ttu-id="439e2-103">Reporting API: er för företagskunder - saldot och sammanfattning</span><span class="sxs-lookup"><span data-stu-id="439e2-103">Reporting APIs for Enterprise customers - Balance and Summary</span></span>

<span data-ttu-id="439e2-104">ger information om saldon, nya inköp, Azure Marketplace serviceavgifter, justeringar och överförbrukning avgifter månatligen hello saldo och översikt över API.</span><span class="sxs-lookup"><span data-stu-id="439e2-104">hello Balance and Summary API offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments, and overage charges.</span></span>


##<a name="request"></a><span data-ttu-id="439e2-105">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="439e2-105">Request</span></span> 
<span data-ttu-id="439e2-106">Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="439e2-106">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> <span data-ttu-id="439e2-107">Om en faktureringsperiod anges sedan returneras data för hello aktuella fakturering tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="439e2-107">If a billing period is not specified, then data for hello current billing period is returned.</span></span>

|<span data-ttu-id="439e2-108">Metod</span><span class="sxs-lookup"><span data-stu-id="439e2-108">Method</span></span> | <span data-ttu-id="439e2-109">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="439e2-109">Request URI</span></span>|
|-|-|
|<span data-ttu-id="439e2-110">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="439e2-110">GET</span></span>| <span data-ttu-id="439e2-111">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="439e2-111">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/balancesummary</span></span>|
|<span data-ttu-id="439e2-112">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="439e2-112">GET</span></span>| <span data-ttu-id="439e2-113">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} /billingPeriods/ {billingPeriod} / balancesummary</span><span class="sxs-lookup"><span data-stu-id="439e2-113">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingPeriods/{billingPeriod}/balancesummary</span></span>|

> [!Note]
> <span data-ttu-id="439e2-114">toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.</span><span class="sxs-lookup"><span data-stu-id="439e2-114">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="439e2-115">Svar</span><span class="sxs-lookup"><span data-stu-id="439e2-115">Response</span></span>

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


<span data-ttu-id="439e2-116">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="439e2-116">**Response property definitions**</span></span>

|<span data-ttu-id="439e2-117">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="439e2-117">Property Name</span></span>| <span data-ttu-id="439e2-118">Typ</span><span class="sxs-lookup"><span data-stu-id="439e2-118">Type</span></span>| <span data-ttu-id="439e2-119">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="439e2-119">Description</span></span>
|-|-|-|
|<span data-ttu-id="439e2-120">id</span><span class="sxs-lookup"><span data-stu-id="439e2-120">id</span></span>|<span data-ttu-id="439e2-121">Sträng</span><span class="sxs-lookup"><span data-stu-id="439e2-121">string</span></span>|<span data-ttu-id="439e2-122">hello unikt Id för en specifik faktureringsperiod och registrering</span><span class="sxs-lookup"><span data-stu-id="439e2-122">hello unique Id for a specific billing period and enrollment</span></span>|
|<span data-ttu-id="439e2-123">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="439e2-123">billingPeriodId</span></span>|<span data-ttu-id="439e2-124">Sträng</span><span class="sxs-lookup"><span data-stu-id="439e2-124">string</span></span> |<span data-ttu-id="439e2-125">hello fakturering period-Id</span><span class="sxs-lookup"><span data-stu-id="439e2-125">hello billing period Id</span></span>|
|<span data-ttu-id="439e2-126">currencyCode</span><span class="sxs-lookup"><span data-stu-id="439e2-126">currencyCode</span></span>|<span data-ttu-id="439e2-127">Sträng</span><span class="sxs-lookup"><span data-stu-id="439e2-127">string</span></span> |<span data-ttu-id="439e2-128">hello valutakoden</span><span class="sxs-lookup"><span data-stu-id="439e2-128">hello currency code</span></span>|
|<span data-ttu-id="439e2-129">beginningBalance</span><span class="sxs-lookup"><span data-stu-id="439e2-129">beginningBalance</span></span>|<span data-ttu-id="439e2-130">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-130">decimal</span></span>| <span data-ttu-id="439e2-131">hello ingående saldo för hello faktureringsperioden</span><span class="sxs-lookup"><span data-stu-id="439e2-131">hello beginning balance for hello billing period</span></span>|
|<span data-ttu-id="439e2-132">endingBalance</span><span class="sxs-lookup"><span data-stu-id="439e2-132">endingBalance</span></span>|<span data-ttu-id="439e2-133">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-133">decimal</span></span>| <span data-ttu-id="439e2-134">hello utgående balans för hello faktureringsperioden (för öppna perioder detta uppdateras dagligen)</span><span class="sxs-lookup"><span data-stu-id="439e2-134">hello ending balance for hello billing period (for open periods this will be updated daily)</span></span>|
|<span data-ttu-id="439e2-135">newPurchases</span><span class="sxs-lookup"><span data-stu-id="439e2-135">newPurchases</span></span>|<span data-ttu-id="439e2-136">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-136">decimal</span></span>| <span data-ttu-id="439e2-137">Nya inköp Totalmängd</span><span class="sxs-lookup"><span data-stu-id="439e2-137">Total new purchase amount</span></span>|
|<span data-ttu-id="439e2-138">justering av</span><span class="sxs-lookup"><span data-stu-id="439e2-138">adjustments</span></span>|<span data-ttu-id="439e2-139">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-139">decimal</span></span>| <span data-ttu-id="439e2-140">Total justeringsbelopp</span><span class="sxs-lookup"><span data-stu-id="439e2-140">Total adjustment amount</span></span>|
|<span data-ttu-id="439e2-141">utnyttjade</span><span class="sxs-lookup"><span data-stu-id="439e2-141">utilized</span></span>|<span data-ttu-id="439e2-142">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-142">decimal</span></span>| <span data-ttu-id="439e2-143">Total användning för åtagande</span><span class="sxs-lookup"><span data-stu-id="439e2-143">Total Commitment usage</span></span>|
|<span data-ttu-id="439e2-144">serviceOverage</span><span class="sxs-lookup"><span data-stu-id="439e2-144">serviceOverage</span></span>|<span data-ttu-id="439e2-145">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-145">decimal</span></span>| <span data-ttu-id="439e2-146">Överförbrukning för Azure-tjänster</span><span class="sxs-lookup"><span data-stu-id="439e2-146">Overage for Azure services</span></span>|
|<span data-ttu-id="439e2-147">chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="439e2-147">chargesBilledSeparately</span></span>|<span data-ttu-id="439e2-148">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-148">decimal</span></span>| <span data-ttu-id="439e2-149">Avgifter debiteras separat</span><span class="sxs-lookup"><span data-stu-id="439e2-149">Charges Billed separately</span></span>|
|<span data-ttu-id="439e2-150">totalOverage</span><span class="sxs-lookup"><span data-stu-id="439e2-150">totalOverage</span></span>|<span data-ttu-id="439e2-151">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-151">decimal</span></span>| <span data-ttu-id="439e2-152">serviceOverage + chargesBilledSeparately</span><span class="sxs-lookup"><span data-stu-id="439e2-152">serviceOverage + chargesBilledSeparately</span></span>|
|<span data-ttu-id="439e2-153">totalUsage</span><span class="sxs-lookup"><span data-stu-id="439e2-153">totalUsage</span></span>|<span data-ttu-id="439e2-154">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-154">decimal</span></span>| <span data-ttu-id="439e2-155">Azure-tjänsten åtagande + total överförbrukning</span><span class="sxs-lookup"><span data-stu-id="439e2-155">Azure service commitment + total Overage</span></span>|
|<span data-ttu-id="439e2-156">azureMarketplaceServiceCharges</span><span class="sxs-lookup"><span data-stu-id="439e2-156">azureMarketplaceServiceCharges</span></span>|<span data-ttu-id="439e2-157">Decimal</span><span class="sxs-lookup"><span data-stu-id="439e2-157">decimal</span></span>| <span data-ttu-id="439e2-158">Totalt antal kostnader för Azure Marketplace</span><span class="sxs-lookup"><span data-stu-id="439e2-158">Total charges for Azure Marketplace</span></span>|
|<span data-ttu-id="439e2-159">newPurchasesDetails</span><span class="sxs-lookup"><span data-stu-id="439e2-159">newPurchasesDetails</span></span>|<span data-ttu-id="439e2-160">JSON-Strängmatrisen av namn-värde-par</span><span class="sxs-lookup"><span data-stu-id="439e2-160">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="439e2-161">Lista över nya inköp</span><span class="sxs-lookup"><span data-stu-id="439e2-161">List of new purchases</span></span>|
|<span data-ttu-id="439e2-162">adjustmentDetails</span><span class="sxs-lookup"><span data-stu-id="439e2-162">adjustmentDetails</span></span>|<span data-ttu-id="439e2-163">JSON-Strängmatrisen av namn-värde-par</span><span class="sxs-lookup"><span data-stu-id="439e2-163">JSON string array of Name Value pairs</span></span>|<span data-ttu-id="439e2-164">Lista över justeringar (kampanj kredit, SIE kredit osv.)</span><span class="sxs-lookup"><span data-stu-id="439e2-164">List of Adjustments (Promo credit, SIE credit etc.)</span></span> |


<br/>
## <a name="see-also"></a><span data-ttu-id="439e2-165">Se även</span><span class="sxs-lookup"><span data-stu-id="439e2-165">See also</span></span>

* [<span data-ttu-id="439e2-166">Fakturering punkter API</span><span class="sxs-lookup"><span data-stu-id="439e2-166">Billing Periods API</span></span>](billing-enterprise-api-billing-periods.md)

* [<span data-ttu-id="439e2-167">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="439e2-167">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="439e2-168">Marketplace Store kostnad API</span><span class="sxs-lookup"><span data-stu-id="439e2-168">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="439e2-169">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="439e2-169">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)