---
title: aaaAzure fakturering Enterprise-API - fakturering punkter | Microsoft Docs
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
ms.openlocfilehash: d4e17f25b22729a7f213306fb019ee0dbeca87ff
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="reporting-apis-for-enterprise-customers---billing-periods"></a><span data-ttu-id="cdbc0-103">Reporting API: er för företagskunder - fakturering punkter</span><span class="sxs-lookup"><span data-stu-id="cdbc0-103">Reporting APIs for Enterprise customers - Billing Periods</span></span>

<span data-ttu-id="cdbc0-104">hello fakturering punkter API returnerar en lista över fakturering punkter som har förbrukningsdata för hello anges registrering i omvänd kronologisk ordning.</span><span class="sxs-lookup"><span data-stu-id="cdbc0-104">hello Billing Periods API returns a list of billing periods that have consumption data for hello specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="cdbc0-105">Varje Period innehåller en egenskap som pekar toohello API väg för hello fyra datauppsättningar - BalanceSummary, UsageDetails, Marktplace kostnader och Prisdokument.</span><span class="sxs-lookup"><span data-stu-id="cdbc0-105">Each Period contains a property pointing toohello API route for hello four sets of data - BalanceSummary, UsageDetails, Marktplace Charges, and PriceSheet.</span></span> <span data-ttu-id="cdbc0-106">Om hello-perioden inte har data är hello motsvarande egenskap null.</span><span class="sxs-lookup"><span data-stu-id="cdbc0-106">If hello period does not have data, hello corresponding property is null.</span></span> 


##<a name="request"></a><span data-ttu-id="cdbc0-107">Förfrågan</span><span class="sxs-lookup"><span data-stu-id="cdbc0-107">Request</span></span> 
<span data-ttu-id="cdbc0-108">Allmänna sidhuvudegenskaper för som behöver toobe läggs anges [här](billing-enterprise-api.md).</span><span class="sxs-lookup"><span data-stu-id="cdbc0-108">Common header properties that need toobe added are specified [here](billing-enterprise-api.md).</span></span> 

|<span data-ttu-id="cdbc0-109">Metod</span><span class="sxs-lookup"><span data-stu-id="cdbc0-109">Method</span></span> | <span data-ttu-id="cdbc0-110">URI-begäran</span><span class="sxs-lookup"><span data-stu-id="cdbc0-110">Request URI</span></span>|
|-|-|
|<span data-ttu-id="cdbc0-111">HÄMTA</span><span class="sxs-lookup"><span data-stu-id="cdbc0-111">GET</span></span>| <span data-ttu-id="cdbc0-112">https://consumption.Azure.com/v2/enrollments/ {enrollmentNumber} / billingperiods</span><span class="sxs-lookup"><span data-stu-id="cdbc0-112">https://consumption.azure.com/v2/enrollments/{enrollmentNumber}/billingperiods</span></span>|

> [!Note]
> <span data-ttu-id="cdbc0-113">toouse hello förhandsversionen av API, Ersätt v2 med v1 hello ovan URL.</span><span class="sxs-lookup"><span data-stu-id="cdbc0-113">toouse hello preview version of API, replace v2 with v1 in hello above URL.</span></span>
>

## <a name="response"></a><span data-ttu-id="cdbc0-114">Svar</span><span class="sxs-lookup"><span data-stu-id="cdbc0-114">Response</span></span>
 
    
    
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
    

<span data-ttu-id="cdbc0-115">**Svaret egenskapsdefinitioner**</span><span class="sxs-lookup"><span data-stu-id="cdbc0-115">**Response property definitions**</span></span>

|<span data-ttu-id="cdbc0-116">Egenskapsnamn</span><span class="sxs-lookup"><span data-stu-id="cdbc0-116">Property Name</span></span>| <span data-ttu-id="cdbc0-117">Typ</span><span class="sxs-lookup"><span data-stu-id="cdbc0-117">Type</span></span>| <span data-ttu-id="cdbc0-118">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="cdbc0-118">Description</span></span>
|-|-|-|
|<span data-ttu-id="cdbc0-119">billingPeriodId</span><span class="sxs-lookup"><span data-stu-id="cdbc0-119">billingPeriodId</span></span>| <span data-ttu-id="cdbc0-120">Sträng</span><span class="sxs-lookup"><span data-stu-id="cdbc0-120">string</span></span>| <span data-ttu-id="cdbc0-121">hello unikt Id som representerar en viss period för fakturering</span><span class="sxs-lookup"><span data-stu-id="cdbc0-121">hello unique Id that represents a particular Billing period</span></span>|
|<span data-ttu-id="cdbc0-122">billingStart</span><span class="sxs-lookup"><span data-stu-id="cdbc0-122">billingStart</span></span>| <span data-ttu-id="cdbc0-123">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="cdbc0-123">datetime</span></span>| <span data-ttu-id="cdbc0-124">ISO 8601-sträng som representerar hello startdatum</span><span class="sxs-lookup"><span data-stu-id="cdbc0-124">ISO 8601 string representing hello period start date</span></span>|
|<span data-ttu-id="cdbc0-125">billingEnd</span><span class="sxs-lookup"><span data-stu-id="cdbc0-125">billingEnd</span></span>| <span data-ttu-id="cdbc0-126">Datum och tid</span><span class="sxs-lookup"><span data-stu-id="cdbc0-126">datetime</span></span>| <span data-ttu-id="cdbc0-127">ISO 8601-sträng som representerar hello slutdatum</span><span class="sxs-lookup"><span data-stu-id="cdbc0-127">ISO 8601 string representing hello period end date</span></span>|
|<span data-ttu-id="cdbc0-128">balanceSummary</span><span class="sxs-lookup"><span data-stu-id="cdbc0-128">balanceSummary</span></span>| <span data-ttu-id="cdbc0-129">Sträng</span><span class="sxs-lookup"><span data-stu-id="cdbc0-129">string</span></span>| <span data-ttu-id="cdbc0-130">hello URL-sökväg som dirigerar toohello saldo översiktsdata för denna period</span><span class="sxs-lookup"><span data-stu-id="cdbc0-130">hello URL path that routes toohello Balance Summary data for this period</span></span>|
|<span data-ttu-id="cdbc0-131">usageDetails</span><span class="sxs-lookup"><span data-stu-id="cdbc0-131">usageDetails</span></span>| <span data-ttu-id="cdbc0-132">Sträng</span><span class="sxs-lookup"><span data-stu-id="cdbc0-132">string</span></span>| <span data-ttu-id="cdbc0-133">hello URL-sökväg som dirigerar toohello användningsinformation data för denna period</span><span class="sxs-lookup"><span data-stu-id="cdbc0-133">hello URL path that routes toohello Usage Details data for this period</span></span>|
|<span data-ttu-id="cdbc0-134">marketplaceCharges</span><span class="sxs-lookup"><span data-stu-id="cdbc0-134">marketplaceCharges</span></span>| <span data-ttu-id="cdbc0-135">Sträng</span><span class="sxs-lookup"><span data-stu-id="cdbc0-135">string</span></span>| <span data-ttu-id="cdbc0-136">hello URL-sökväg som dirigerar toohello Marketplace-debiteringar data för denna period</span><span class="sxs-lookup"><span data-stu-id="cdbc0-136">hello URL path that routes toohello Marketplace Charges data for this period</span></span>|
|<span data-ttu-id="cdbc0-137">Prisdokument</span><span class="sxs-lookup"><span data-stu-id="cdbc0-137">priceSheet</span></span>| <span data-ttu-id="cdbc0-138">Sträng</span><span class="sxs-lookup"><span data-stu-id="cdbc0-138">string</span></span>| <span data-ttu-id="cdbc0-139">hello URL-sökväg som dirigerar toohello Prisdokument data för denna period</span><span class="sxs-lookup"><span data-stu-id="cdbc0-139">hello URL path that routes toohello PriceSheet data for this period</span></span>|

<br/>
## <a name="see-also"></a><span data-ttu-id="cdbc0-140">Se även</span><span class="sxs-lookup"><span data-stu-id="cdbc0-140">See also</span></span>

* [<span data-ttu-id="cdbc0-141">Belastningsutjämning och sammanfattning API</span><span class="sxs-lookup"><span data-stu-id="cdbc0-141">Balance and Summary API</span></span>](billing-enterprise-api-balance-summary.md)

* [<span data-ttu-id="cdbc0-142">Information om användning av API</span><span class="sxs-lookup"><span data-stu-id="cdbc0-142">Usage Detail API</span></span>](billing-enterprise-api-usage-detail.md) 

* [<span data-ttu-id="cdbc0-143">Marketplace Store kostnad API</span><span class="sxs-lookup"><span data-stu-id="cdbc0-143">Marketplace Store Charge API</span></span>](billing-enterprise-api-marketplace-storecharge.md) 

* [<span data-ttu-id="cdbc0-144">Price Sheet API</span><span class="sxs-lookup"><span data-stu-id="cdbc0-144">Price Sheet API</span></span>](billing-enterprise-api-pricesheet.md)