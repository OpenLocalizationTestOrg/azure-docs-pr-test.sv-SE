---
title: 'aaaAzure fakturering Enterprise-API: er | Microsoft Docs'
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
ms.openlocfilehash: 017cecc57ad6bdeb402b5d9d57fc95df9b033a42
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-reporting-apis-for-enterprise-customers"></a><span data-ttu-id="e021c-103">Översikt över Reporting API: er för Enterprise-kunder</span><span class="sxs-lookup"><span data-stu-id="e021c-103">Overview of Reporting APIs for Enterprise customers</span></span>
<span data-ttu-id="e021c-104">hello Reporting API: er kan Azure Enterprise-kunder tooprogrammatically pull användnings- och faktureringsinformation i Verktyg för analys av önskade data.</span><span class="sxs-lookup"><span data-stu-id="e021c-104">hello Reporting APIs enable Enterprise Azure customers tooprogrammatically pull consumption and billing data into preferred data analysis tools.</span></span> 

## <a name="enabling-data-access-toohello-api"></a><span data-ttu-id="e021c-105">Aktivera data access toohello API: et</span><span class="sxs-lookup"><span data-stu-id="e021c-105">Enabling data access toohello API</span></span>
* <span data-ttu-id="e021c-106">**Generera eller hämta hello API-nyckel** - inloggningen toohello Enterprise portal och följ hello kursen under Hjälp - Reporting API: er.</span><span class="sxs-lookup"><span data-stu-id="e021c-106">**Generate or retrieve hello API key** - Log in toohello Enterprise portal and follow hello tutorial under Help - Reporting APIs.</span></span> <span data-ttu-id="e021c-107">hello förklaras första under den här hjälpartikeln hur toogenerate eller hämta hello API-nyckel för hello angivna registreringen.</span><span class="sxs-lookup"><span data-stu-id="e021c-107">hello first section under this help article explains how toogenerate or retrieve hello API key for hello specified enrollment.</span></span>
* <span data-ttu-id="e021c-108">**Skicka nycklar i hello API** -hello API-nyckeln måste toobe som skickades för varje anrop för autentisering och auktorisering.</span><span class="sxs-lookup"><span data-stu-id="e021c-108">**Passing keys in hello API** - hello API key needs toobe passed for each call for Authentication and Authorization.</span></span> <span data-ttu-id="e021c-109">hello måste följande egenskap toobe toohello HTTP-huvuden</span><span class="sxs-lookup"><span data-stu-id="e021c-109">hello following property needs toobe toohello HTTP headers</span></span>

|<span data-ttu-id="e021c-110">Begäran sidhuvud nyckel</span><span class="sxs-lookup"><span data-stu-id="e021c-110">Request Header Key</span></span> | <span data-ttu-id="e021c-111">Värde</span><span class="sxs-lookup"><span data-stu-id="e021c-111">Value</span></span>|
|-|-|
|<span data-ttu-id="e021c-112">Auktorisering</span><span class="sxs-lookup"><span data-stu-id="e021c-112">Authorization</span></span>| <span data-ttu-id="e021c-113">Ange hello värde i det här formatet: **ägar {API_KEY}**</span><span class="sxs-lookup"><span data-stu-id="e021c-113">Specify hello value in this format: **bearer {API_KEY}**</span></span> <br/> <span data-ttu-id="e021c-114">Exempel: ägar eyr... 09</span><span class="sxs-lookup"><span data-stu-id="e021c-114">Example: bearer eyr....09</span></span>|

## <a name="consumption-apis"></a><span data-ttu-id="e021c-115">API: er för förbrukning</span><span class="sxs-lookup"><span data-stu-id="e021c-115">Consumption APIs</span></span>
<span data-ttu-id="e021c-116">Swagger-slutpunkten är tillgänglig [här](https://consumption.azure.com/swagger/ui/index) i hello API: er som beskrivs nedan som ska aktivera enkel introspection hello API och hello möjlighet toogenerate klientenheter SDK: er med hjälp av [AutoRest](https://github.com/Azure/AutoRest) eller [ Swagger CodeGen](http://swagger.io/swagger-codegen/).</span><span class="sxs-lookup"><span data-stu-id="e021c-116">A Swagger endpoint is available [here](https://consumption.azure.com/swagger/ui/index) for hello APIs described below which should enable easy introspection of hello API and hello ability toogenerate client SDKs using [AutoRest](https://github.com/Azure/AutoRest) or [Swagger CodeGen](http://swagger.io/swagger-codegen/).</span></span> <span data-ttu-id="e021c-117">Data från 1 maj 2014 är tillgängliga via den här API: T.</span><span class="sxs-lookup"><span data-stu-id="e021c-117">Data beginning May 1, 2014 is available through this API.</span></span> 

* <span data-ttu-id="e021c-118">**Belastningsutjämning och sammanfattning** - hello [saldo och översikt över API](billing-enterprise-api-balance-summary.md) ger information om saldon, nya inköp, avgifter för Azure Marketplace, justeringar och överförbrukning avgifter månatligen.</span><span class="sxs-lookup"><span data-stu-id="e021c-118">**Balance and Summary** - hello [Balance and Summary API](billing-enterprise-api-balance-summary.md) offers a monthly summary of information on balances, new purchases, Azure Marketplace service charges, adjustments and overage charges.</span></span>

* <span data-ttu-id="e021c-119">**Användningsinformation** - hello [användning detalj API](billing-enterprise-api-usage-detail.md) erbjuder en daglig sammanställning av förbrukade antalen och beräknade kostnader genom en registrering.</span><span class="sxs-lookup"><span data-stu-id="e021c-119">**Usage Details** - hello [Usage Detail API](billing-enterprise-api-usage-detail.md) offers a daily breakdown of consumed quantities and estimated charges by an Enrollment.</span></span> <span data-ttu-id="e021c-120">hello resultatet innehåller också information om instanser, mätare och avdelningar.</span><span class="sxs-lookup"><span data-stu-id="e021c-120">hello result also includes information on instances, meters and departments.</span></span> <span data-ttu-id="e021c-121">hello API kan efterfrågas med fakturering punkt eller genom att start- och slutdatum.</span><span class="sxs-lookup"><span data-stu-id="e021c-121">hello API can be queried by Billing period or by a specified start and end date.</span></span> 

* <span data-ttu-id="e021c-122">**Marketplace Store kostnad** - hello [Marketplace Store kostnad API](billing-enterprise-api-marketplace-storecharge.md) returnerar hello användningsbaserad marketplace avgifter sammanställning per dag för hello angivna fakturering Period eller start- och slutdatum (en gång avgifter inte ingår) .</span><span class="sxs-lookup"><span data-stu-id="e021c-122">**Marketplace Store Charge** - hello [Marketplace Store Charge API](billing-enterprise-api-marketplace-storecharge.md) returns hello usage-based marketplace charges breakdown by day for hello specified Billing Period or start and end dates (one time fees are not included).</span></span>

* <span data-ttu-id="e021c-123">**Prisdokument** - hello [Price Sheet API](billing-enterprise-api-pricesheet.md) innehåller hello tillämpliga hastighet för varje mätaren för hello registrering och fakturering Period.</span><span class="sxs-lookup"><span data-stu-id="e021c-123">**Price Sheet** - hello [Price Sheet API](billing-enterprise-api-pricesheet.md) provides hello applicable rate for each Meter for hello given Enrollment and Billing Period.</span></span> 

## <a name="helper-apis"></a><span data-ttu-id="e021c-124">Helper-API: er</span><span class="sxs-lookup"><span data-stu-id="e021c-124">Helper APIs</span></span>
 <span data-ttu-id="e021c-125">**Lista över fakturering punkter** - hello [fakturering punkter API](billing-enterprise-api-billing-periods.md) returnerar en lista över fakturering punkter som har förbrukningsdata för hello angivna registrering i omvänd kronologisk ordning.</span><span class="sxs-lookup"><span data-stu-id="e021c-125">**List Billing Periods** - hello [Billing Periods API](billing-enterprise-api-billing-periods.md) returns a list of billing periods that have consumption data for hello specified Enrollment in reverse chronological order.</span></span> <span data-ttu-id="e021c-126">Varje Period innehåller en egenskap som pekar toohello API väg för hello fyra datauppsättningar - BalanceSummary, UsageDetails, Marketplace-debiteringar och Prisdokument.</span><span class="sxs-lookup"><span data-stu-id="e021c-126">Each Period contains a property pointing toohello API route for hello four sets of data - BalanceSummary, UsageDetails, Marketplace Charges, and Price Sheet.</span></span>


## <a name="api-response-codes"></a><span data-ttu-id="e021c-127">API-svarskoder</span><span class="sxs-lookup"><span data-stu-id="e021c-127">API Response Codes</span></span>  
|<span data-ttu-id="e021c-128">Svarets statuskod</span><span class="sxs-lookup"><span data-stu-id="e021c-128">Response Status Code</span></span>|<span data-ttu-id="e021c-129">Meddelande</span><span class="sxs-lookup"><span data-stu-id="e021c-129">Message</span></span>|<span data-ttu-id="e021c-130">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e021c-130">Description</span></span>|
|-|-|-|
|<span data-ttu-id="e021c-131">200</span><span class="sxs-lookup"><span data-stu-id="e021c-131">200</span></span>| <span data-ttu-id="e021c-132">OKEJ</span><span class="sxs-lookup"><span data-stu-id="e021c-132">OK</span></span>|<span data-ttu-id="e021c-133">Inga fel</span><span class="sxs-lookup"><span data-stu-id="e021c-133">No error</span></span>|
|<span data-ttu-id="e021c-134">401</span><span class="sxs-lookup"><span data-stu-id="e021c-134">401</span></span>| <span data-ttu-id="e021c-135">Behörighet saknas</span><span class="sxs-lookup"><span data-stu-id="e021c-135">Unauthorized</span></span>| <span data-ttu-id="e021c-136">API-nyckel hittades inte, ogiltig upphört att gälla osv.</span><span class="sxs-lookup"><span data-stu-id="e021c-136">API Key not found, Invalid, Expired etc.</span></span>|
|<span data-ttu-id="e021c-137">404</span><span class="sxs-lookup"><span data-stu-id="e021c-137">404</span></span>| <span data-ttu-id="e021c-138">Inte tillgänglig</span><span class="sxs-lookup"><span data-stu-id="e021c-138">Unavailable</span></span>| <span data-ttu-id="e021c-139">Rapporten slutpunkten hittades inte</span><span class="sxs-lookup"><span data-stu-id="e021c-139">Report endpoint not found</span></span>|
|<span data-ttu-id="e021c-140">400</span><span class="sxs-lookup"><span data-stu-id="e021c-140">400</span></span>| <span data-ttu-id="e021c-141">Felaktig begäran</span><span class="sxs-lookup"><span data-stu-id="e021c-141">Bad Request</span></span>| <span data-ttu-id="e021c-142">Ogiltiga parametrar – datumintervall, EA siffror osv.</span><span class="sxs-lookup"><span data-stu-id="e021c-142">Invalid params – Date ranges, EA numbers etc.</span></span>|
|<span data-ttu-id="e021c-143">500</span><span class="sxs-lookup"><span data-stu-id="e021c-143">500</span></span>| <span data-ttu-id="e021c-144">Serverfel</span><span class="sxs-lookup"><span data-stu-id="e021c-144">Server Error</span></span>| <span data-ttu-id="e021c-145">Unexoected fel vid bearbetning av begäran</span><span class="sxs-lookup"><span data-stu-id="e021c-145">Unexoected error processing request</span></span>| 









