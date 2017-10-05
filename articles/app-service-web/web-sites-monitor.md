---
title: "Övervaka appar i Azure App Service | Microsoft Docs"
description: "Lär dig hur du övervakar appar i Azure App Service med hjälp av Azure-portalen."
services: app-service
documentationcenter: 
author: btardif
manager: erikre
editor: 
ms.assetid: d273da4e-07de-48e0-b99d-4020d84a425e
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/07/2016
ms.author: byvinyal
ms.openlocfilehash: 25d3776920d683fffedcd8ac6ed0e84dfe875974
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a><span data-ttu-id="915ec-103">Så här: övervaka appar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="915ec-103">How to: Monitor Apps in Azure App Service</span></span>
<span data-ttu-id="915ec-104">[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) har inbyggda funktioner som övervakning för den [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="915ec-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) provides built in monitoring functionality in the [Azure Portal](https://portal.azure.com).</span></span>
<span data-ttu-id="915ec-105">Detta omfattar möjligheten att granska **kvoter** och **mått** för en app, samt App Service-plan, ställa in **aviseringar** och även **skalning**automatiskt baserat på de här måtten.</span><span class="sxs-lookup"><span data-stu-id="915ec-105">This includes the ability to review **quotas** and **metrics** for an app as well as the App Service plan, setting up **alerts** and even **scaling** automatically based on these metrics.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a><span data-ttu-id="915ec-106">Förstå kvoter och mått</span><span class="sxs-lookup"><span data-stu-id="915ec-106">Understanding Quotas and Metrics</span></span>
### <a name="quotas"></a><span data-ttu-id="915ec-107">Kvoter</span><span class="sxs-lookup"><span data-stu-id="915ec-107">Quotas</span></span>
<span data-ttu-id="915ec-108">Program som finns i App Service regleras vissa *gränser* för de resurser som de kan använda.</span><span class="sxs-lookup"><span data-stu-id="915ec-108">Applications hosted in App Service are subject to certain *limits* on the resources they can use.</span></span> <span data-ttu-id="915ec-109">Gränserna som definieras av den **programtjänstplanen** associerat med appen.</span><span class="sxs-lookup"><span data-stu-id="915ec-109">The limits are defined by the **App Service plan** associated with the app.</span></span>

<span data-ttu-id="915ec-110">Om programmet finns i en **lediga** eller **delade** planera sedan gränser för de resurser som kan använda appen definieras av **kvoter**.</span><span class="sxs-lookup"><span data-stu-id="915ec-110">If the application is hosted in a **Free** or **Shared** plan, then the limits on the resources the app can use are defined by **Quotas**.</span></span>

<span data-ttu-id="915ec-111">Om programmet finns i en **grundläggande**, **Standard** eller **Premium** planera sedan gränser för de resurser som de kan använda ställs in med den **storlek**(Small, Medium, stora) och **instansen antal** (1, 2, 3,...) av den **programtjänstplanen**.</span><span class="sxs-lookup"><span data-stu-id="915ec-111">If the application is hosted in a **Basic**, **Standard** or **Premium** plan, then the limits on the resources they can use are set by the **size** (Small, Medium, Large) and **instance count** (1, 2, 3, ...) of the **App Service plan**.</span></span>

<span data-ttu-id="915ec-112">**Kvoter** för **lediga** eller **delade** appar är:</span><span class="sxs-lookup"><span data-stu-id="915ec-112">**Quotas** for **Free** or **Shared** apps are:</span></span>

* <span data-ttu-id="915ec-113">**CPU(short)**</span><span class="sxs-lookup"><span data-stu-id="915ec-113">**CPU(Short)**</span></span>
  * <span data-ttu-id="915ec-114">Mängden CPU som tillåts för det här programmet i en 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="915ec-114">Amount of CPU allowed for this application in a 5-minute interval.</span></span> <span data-ttu-id="915ec-115">Den här kvoten anger igen var femte minut.</span><span class="sxs-lookup"><span data-stu-id="915ec-115">This quota re-sets every 5 minutes.</span></span>
* <span data-ttu-id="915ec-116">**CPU(Day)**</span><span class="sxs-lookup"><span data-stu-id="915ec-116">**CPU(Day)**</span></span>
  * <span data-ttu-id="915ec-117">Totalt antal processorer som tillåts för det här programmet i en dag.</span><span class="sxs-lookup"><span data-stu-id="915ec-117">Total amount of CPU allowed for this application in a day.</span></span> <span data-ttu-id="915ec-118">Den här kvoten anger igen var 24: e timme vid midnatt UTC.</span><span class="sxs-lookup"><span data-stu-id="915ec-118">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="915ec-119">**Minne**</span><span class="sxs-lookup"><span data-stu-id="915ec-119">**Memory**</span></span>
  * <span data-ttu-id="915ec-120">Totala mängden minne som tillåts för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="915ec-120">Total amount of memory allowed for this application.</span></span>
* <span data-ttu-id="915ec-121">**Bandbredd**</span><span class="sxs-lookup"><span data-stu-id="915ec-121">**Bandwidth**</span></span>
  * <span data-ttu-id="915ec-122">Totalt antal utgående bandbredd som tillåts för det här programmet i en dag.</span><span class="sxs-lookup"><span data-stu-id="915ec-122">Total amount of outgoing bandwidth allowed for this application in a day.</span></span>
    <span data-ttu-id="915ec-123">Den här kvoten anger igen var 24: e timme vid midnatt UTC.</span><span class="sxs-lookup"><span data-stu-id="915ec-123">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="915ec-124">**Filsystem**</span><span class="sxs-lookup"><span data-stu-id="915ec-124">**Filesystem**</span></span>
  * <span data-ttu-id="915ec-125">Totalt antal tillåtet lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="915ec-125">Total amount of storage allowed.</span></span>

<span data-ttu-id="915ec-126">Kvoten som endast till appar som finns på **grundläggande**, **Standard** och **Premium** planer är **Filesystem**.</span><span class="sxs-lookup"><span data-stu-id="915ec-126">The only quota applicable to apps hosted on **Basic**, **Standard** and **Premium** plans is **Filesystem**.</span></span>

<span data-ttu-id="915ec-127">Mer information om specifika kvoter, gränser och funktioner som är tillgängliga för olika App Service SKU hittar du här: [Tjänstbegränsningarna för Azure-prenumeration](../azure-subscription-service-limits.md#app-service-limits)</span><span class="sxs-lookup"><span data-stu-id="915ec-127">More information about the specific quotas, limits and features available to the different App Service SKUs can be found here: [Azure Subscription Service Limits](../azure-subscription-service-limits.md#app-service-limits)</span></span>

#### <a name="quota-enforcement"></a><span data-ttu-id="915ec-128">Kvoter</span><span class="sxs-lookup"><span data-stu-id="915ec-128">Quota Enforcement</span></span>
<span data-ttu-id="915ec-129">Om ett program i användningen överskrider den **CPU (korta)**, **CPU (dag)**, eller **bandbredd** kvoten och programmet kommer att stoppas tills kvoten anger igen.</span><span class="sxs-lookup"><span data-stu-id="915ec-129">If an application in its usage exceeds the **CPU (short)**, **CPU (Day)**, or **bandwidth** quota then the application will be stopped until the quota re-sets.</span></span> <span data-ttu-id="915ec-130">Under denna tid kan alla förfrågningar som leder till en **HTTP 403**.</span><span class="sxs-lookup"><span data-stu-id="915ec-130">During this time, all incoming requests will result in an **HTTP 403**.</span></span>
![][http403]

<span data-ttu-id="915ec-131">Om programmet **minne** kvot har överskridits och programmet blir inte startats.</span><span class="sxs-lookup"><span data-stu-id="915ec-131">If the application **memory** quota is exceeded, then the application will be re-started.</span></span>

<span data-ttu-id="915ec-132">Om den **Filesystem** kvot har överskridits och sedan någon skriva misslyckas åtgärden, inklusive skrivs till loggarna.</span><span class="sxs-lookup"><span data-stu-id="915ec-132">If the **Filesystem** quota is exceeded, then any write operation will fail, this includes writing to logs.</span></span>

<span data-ttu-id="915ec-133">Kvoter kan ökas eller tas bort från din app genom att uppgradera din programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="915ec-133">Quotas can be increased or removed from your app by upgrading your App Service plan.</span></span>

### <a name="metrics"></a><span data-ttu-id="915ec-134">Mått</span><span class="sxs-lookup"><span data-stu-id="915ec-134">Metrics</span></span>
<span data-ttu-id="915ec-135">**Mått** innehåller information om appen eller App Service-plan beteende.</span><span class="sxs-lookup"><span data-stu-id="915ec-135">**Metrics** provide information about the app, or App Service plan's behavior.</span></span>

<span data-ttu-id="915ec-136">För en **programmet**, tillgängliga mått är:</span><span class="sxs-lookup"><span data-stu-id="915ec-136">For an **Application**, the available metrics are:</span></span>

* <span data-ttu-id="915ec-137">**Genomsnittlig svarstid**</span><span class="sxs-lookup"><span data-stu-id="915ec-137">**Average Response Time**</span></span>
  * <span data-ttu-id="915ec-138">Genomsnittlig tid för appen att betjäna förfrågningar i ms.</span><span class="sxs-lookup"><span data-stu-id="915ec-138">The average time taken for the app to serve requests in ms.</span></span>
* <span data-ttu-id="915ec-139">**Genomsnittlig minne arbetsminne**</span><span class="sxs-lookup"><span data-stu-id="915ec-139">**Average memory working set**</span></span>
  * <span data-ttu-id="915ec-140">Genomsnittlig mängd minne i MIB som används av appen.</span><span class="sxs-lookup"><span data-stu-id="915ec-140">The average amount of memory in MiBs used by the app.</span></span>
* <span data-ttu-id="915ec-141">**CPU-tid**</span><span class="sxs-lookup"><span data-stu-id="915ec-141">**CPU Time**</span></span>
  * <span data-ttu-id="915ec-142">Mängden CPU i sekunder som används av appen.</span><span class="sxs-lookup"><span data-stu-id="915ec-142">The amount of CPU in seconds consumed by the app.</span></span> <span data-ttu-id="915ec-143">Mer information om den här mått finns: [vs CPU CPU-tid i procent](#cpu-time-vs-cpu-percentage)</span><span class="sxs-lookup"><span data-stu-id="915ec-143">For more information about this metric see: [CPU time vs CPU percentage](#cpu-time-vs-cpu-percentage)</span></span>
* <span data-ttu-id="915ec-144">**Data i**</span><span class="sxs-lookup"><span data-stu-id="915ec-144">**Data In**</span></span>
  * <span data-ttu-id="915ec-145">Mängden inkommande bandbredd som används av appen i MIB.</span><span class="sxs-lookup"><span data-stu-id="915ec-145">The amount of incoming bandwidth consumed by the app in MiBs.</span></span>
* <span data-ttu-id="915ec-146">**Ut data**</span><span class="sxs-lookup"><span data-stu-id="915ec-146">**Data Out**</span></span>
  * <span data-ttu-id="915ec-147">Mängden utgående bandbredd som används av appen i MIB.</span><span class="sxs-lookup"><span data-stu-id="915ec-147">The amount of outgoing bandwidth consumed by the app in MiBs.</span></span>
* <span data-ttu-id="915ec-148">**HTTP-2xx**</span><span class="sxs-lookup"><span data-stu-id="915ec-148">**Http 2xx**</span></span>
  * <span data-ttu-id="915ec-149">Antal begäranden som resulterar i en http-statuskod > = 200 men < 300.</span><span class="sxs-lookup"><span data-stu-id="915ec-149">Count of requests resulting in a http status code >= 200 but < 300.</span></span>
* <span data-ttu-id="915ec-150">**HTTP-3xx**</span><span class="sxs-lookup"><span data-stu-id="915ec-150">**Http 3xx**</span></span>
  * <span data-ttu-id="915ec-151">Antal begäranden som resulterar i en http-statuskod > = 300 men < 400.</span><span class="sxs-lookup"><span data-stu-id="915ec-151">Count of requests resulting in a http status code >= 300 but < 400.</span></span>
* <span data-ttu-id="915ec-152">**HTTP 401**</span><span class="sxs-lookup"><span data-stu-id="915ec-152">**Http 401**</span></span>
  * <span data-ttu-id="915ec-153">Antal begäranden som ledde till ett HTTP 401-statuskod.</span><span class="sxs-lookup"><span data-stu-id="915ec-153">Count of requests resulting in HTTP 401 status code.</span></span>
* <span data-ttu-id="915ec-154">**HTTP 403**</span><span class="sxs-lookup"><span data-stu-id="915ec-154">**Http 403**</span></span>
  * <span data-ttu-id="915ec-155">Antal begäranden som ledde till ett 403 HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="915ec-155">Count of requests resulting in HTTP 403 status code.</span></span>
* <span data-ttu-id="915ec-156">**HTTP 404**</span><span class="sxs-lookup"><span data-stu-id="915ec-156">**Http 404**</span></span>
  * <span data-ttu-id="915ec-157">Antal begäranden som ledde till ett 404 HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="915ec-157">Count of requests resulting in HTTP 404 status code.</span></span>
* <span data-ttu-id="915ec-158">**HTTP 406**</span><span class="sxs-lookup"><span data-stu-id="915ec-158">**Http 406**</span></span>
  * <span data-ttu-id="915ec-159">Antal begäranden som resulterar i 406 HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="915ec-159">Count of requests resulting in HTTP 406 status code.</span></span>
* <span data-ttu-id="915ec-160">**HTTP-4xx**</span><span class="sxs-lookup"><span data-stu-id="915ec-160">**Http 4xx**</span></span>
  * <span data-ttu-id="915ec-161">Antal begäranden som resulterar i en http-statuskod > = 400 men < 500.</span><span class="sxs-lookup"><span data-stu-id="915ec-161">Count of requests resulting in a http status code >= 400 but < 500.</span></span>
* <span data-ttu-id="915ec-162">**HTTP-fel**</span><span class="sxs-lookup"><span data-stu-id="915ec-162">**Http Server Errors**</span></span>
  * <span data-ttu-id="915ec-163">Antal begäranden som resulterar i en http-statuskod > = 500, men < 600.</span><span class="sxs-lookup"><span data-stu-id="915ec-163">Count of requests resulting in a http status code >= 500 but < 600.</span></span>
* <span data-ttu-id="915ec-164">**Arbetsminnet för minne**</span><span class="sxs-lookup"><span data-stu-id="915ec-164">**Memory working set**</span></span>
  * <span data-ttu-id="915ec-165">Aktuell mängd minne som används av appen i MIB.</span><span class="sxs-lookup"><span data-stu-id="915ec-165">Current amount of memory used by the app in MiBs.</span></span>
* <span data-ttu-id="915ec-166">**Begäranden**</span><span class="sxs-lookup"><span data-stu-id="915ec-166">**Requests**</span></span>
  * <span data-ttu-id="915ec-167">Totalt antal begäranden oavsett deras resulterande HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="915ec-167">Total number of requests regardless of their resulting HTTP status code.</span></span>

<span data-ttu-id="915ec-168">För en **programtjänstplanen**, tillgängliga mått är:</span><span class="sxs-lookup"><span data-stu-id="915ec-168">For an **App Service plan**, the available metrics are:</span></span>

> [!NOTE]
> <span data-ttu-id="915ec-169">App Service-plan mått är bara tillgängliga för planer i **grundläggande**, **Standard** och **Premium** SKU.</span><span class="sxs-lookup"><span data-stu-id="915ec-169">App Service plan metrics are only available for plans in **Basic**, **Standard** and **Premium** SKU.</span></span>
> 
> 

* <span data-ttu-id="915ec-170">**CPU-procent**</span><span class="sxs-lookup"><span data-stu-id="915ec-170">**CPU Percentage**</span></span>
  * <span data-ttu-id="915ec-171">Genomsnittlig CPU som används i alla instanser av planen.</span><span class="sxs-lookup"><span data-stu-id="915ec-171">The average CPU used across all instances of the plan.</span></span>
* <span data-ttu-id="915ec-172">**Minnesprocent**</span><span class="sxs-lookup"><span data-stu-id="915ec-172">**Memory Percentage**</span></span>
  * <span data-ttu-id="915ec-173">Det genomsnittliga minne som används i alla instanser av planen.</span><span class="sxs-lookup"><span data-stu-id="915ec-173">The average memory used across all instances of the plan.</span></span>
* <span data-ttu-id="915ec-174">**Data i**</span><span class="sxs-lookup"><span data-stu-id="915ec-174">**Data In**</span></span>
  * <span data-ttu-id="915ec-175">Genomsnittlig inkommande bandbredden som används i alla instanser av planen.</span><span class="sxs-lookup"><span data-stu-id="915ec-175">The average incoming bandwidth used across all instances of the plan.</span></span>
* <span data-ttu-id="915ec-176">**Ut data**</span><span class="sxs-lookup"><span data-stu-id="915ec-176">**Data Out**</span></span>
  * <span data-ttu-id="915ec-177">Den genomsnittliga utgående bandbredden som används i alla instanser av planen.</span><span class="sxs-lookup"><span data-stu-id="915ec-177">The average outgoing bandwidth used across all instances of the plan.</span></span>
* <span data-ttu-id="915ec-178">**Diskkölängd**</span><span class="sxs-lookup"><span data-stu-id="915ec-178">**Disk Queue Length**</span></span>
  * <span data-ttu-id="915ec-179">Det genomsnittliga antalet både Skriv- och läsbegäranden som köade på lagring.</span><span class="sxs-lookup"><span data-stu-id="915ec-179">The average number of both read and write requests that were queued on storage.</span></span> <span data-ttu-id="915ec-180">En hög diskkölängd är en indikation på ett program som kan långsammare på grund av mycket i/o-disk.</span><span class="sxs-lookup"><span data-stu-id="915ec-180">A high disk queue length is an indication of an application that might be slowing down due to excessive disk I/O.</span></span>
* <span data-ttu-id="915ec-181">**HTTP-Kölängd**</span><span class="sxs-lookup"><span data-stu-id="915ec-181">**Http Queue Length**</span></span>
  * <span data-ttu-id="915ec-182">Genomsnittligt antal HTTP-begäranden som hade till kön innan uppfylls.</span><span class="sxs-lookup"><span data-stu-id="915ec-182">The average number of HTTP requests that had to sit on the queue before being fulfilled.</span></span> <span data-ttu-id="915ec-183">En hög eller ökande HTTP Kölängd är ett symtom på en plan vid hög belastning.</span><span class="sxs-lookup"><span data-stu-id="915ec-183">A high or increasing HTTP Queue length is a symptom of a plan under heavy load.</span></span>

### <a name="cpu-time-vs-cpu-percentage"></a><span data-ttu-id="915ec-184">Vs CPU CPU-tid i procent</span><span class="sxs-lookup"><span data-stu-id="915ec-184">CPU time vs CPU percentage</span></span>
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

<span data-ttu-id="915ec-185">Det finns 2 mått som avspeglar CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="915ec-185">There are 2 metrics that reflect CPU usage.</span></span> <span data-ttu-id="915ec-186">**CPU-tid** och **CPU-procent**</span><span class="sxs-lookup"><span data-stu-id="915ec-186">**CPU time** and **CPU percentage**</span></span>

<span data-ttu-id="915ec-187">**CPU-tid** är användbart för appar som finns i **lediga** eller **delade** planer eftersom en av sina kvoter definieras i CPU minuter som används av appen.</span><span class="sxs-lookup"><span data-stu-id="915ec-187">**CPU Time** is useful for apps hosted in **Free** or **Shared** plans since one of their quotas is defined in CPU minutes used by the app.</span></span>

<span data-ttu-id="915ec-188">**CPU-procent** å andra sidan är användbart för appar som finns i **grundläggande**, **standard** och **premium** planer eftersom de kan skaländras ut och det här måttet är en bra indikation på den totala användningen i alla instanser.</span><span class="sxs-lookup"><span data-stu-id="915ec-188">**CPU percentage** on the other hand is useful for apps hosted in **basic**, **standard** and **premium** plans since they can be scaled out and this metric is a good indication of the overall usage across all instances.</span></span>

## <a name="metrics-granularity-and-retention-policy"></a><span data-ttu-id="915ec-189">Mått granularitet och bevarandeprincip</span><span class="sxs-lookup"><span data-stu-id="915ec-189">Metrics Granularity and Retention Policy</span></span>
<span data-ttu-id="915ec-190">Mått för ett program och apptjänstplan loggas och sammanställs av tjänsten med följande granulariteter och bevarandeprinciper:</span><span class="sxs-lookup"><span data-stu-id="915ec-190">Metrics for an application and app service plan are logged and aggregated by the service with the following granularities and retention policies:</span></span>

* <span data-ttu-id="915ec-191">**Minut** granularitet mått bevaras för **48 timmarna**</span><span class="sxs-lookup"><span data-stu-id="915ec-191">**Minute** granularity metrics are retained for **48 hours**</span></span>
* <span data-ttu-id="915ec-192">**Timme** granularitet mått bevaras för **30 dagar**</span><span class="sxs-lookup"><span data-stu-id="915ec-192">**Hour** granularity metrics are retained for **30 days**</span></span>
* <span data-ttu-id="915ec-193">**Dag** granularitet mått bevaras för **90 dagar**</span><span class="sxs-lookup"><span data-stu-id="915ec-193">**Day** granularity metrics are retained for **90 days**</span></span>

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a><span data-ttu-id="915ec-194">Övervakning av kvoter och mått i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="915ec-194">Monitoring Quotas and Metrics in the Azure Portal.</span></span>
<span data-ttu-id="915ec-195">Du kan granska status för de olika **kvoter** och **mått** påverkar ett program i den [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="915ec-195">You can review the status of the different **quotas** and **metrics** affecting an application in the [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="915ec-196">![][quotas]
**Kvoter** hittar du under Inställningar >**kvoter**.</span><span class="sxs-lookup"><span data-stu-id="915ec-196">![][quotas]
**Quotas** can be found under Settings>**Quotas**.</span></span> <span data-ttu-id="915ec-197">UX kan du granska: (1) kvoter namn, (2) intervallet för återställning, (3) den aktuella gränsen och (4) aktuella värde.</span><span class="sxs-lookup"><span data-stu-id="915ec-197">The UX allows you to review: (1) the quotas name, (2) its reset interval, (3) its current limit and (4) current value.</span></span>

<span data-ttu-id="915ec-198">![][metrics]
**Mått** kan vara åtkomst direkt från resursbladet.</span><span class="sxs-lookup"><span data-stu-id="915ec-198">![][metrics]
**Metrics** can be access directly from the resource blade.</span></span> <span data-ttu-id="915ec-199">Du kan också anpassa i schemat: (1) **klickar du på** på och välj (2) **redigera diagram**.</span><span class="sxs-lookup"><span data-stu-id="915ec-199">You can also customize the chart by: (1) **click** on it, and select (2) **edit chart**.</span></span>
<span data-ttu-id="915ec-200">Härifrån kan du ändra (3) **tidsintervallet**, (4) **diagramtypen**, 5 **mått** ska visas.</span><span class="sxs-lookup"><span data-stu-id="915ec-200">From here you can change the (3) **time range**, (4) **chart type**, and (5) **metrics** to display.</span></span>  

<span data-ttu-id="915ec-201">Du kan lära dig mer om här mått: [övervaka tjänsten](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="915ec-201">You can learn more about metrics here: [Monitor service metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

## <a name="alerts-and-autoscale"></a><span data-ttu-id="915ec-202">Aviseringar och Autoskala</span><span class="sxs-lookup"><span data-stu-id="915ec-202">Alerts and Autoscale</span></span>
<span data-ttu-id="915ec-203">Mätvärden för en App eller App Service-plan kan vara kopplad till aviseringar, mer information om det finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="915ec-203">Metrics for an App or App Service plan can be hooked up to alerts, to learn more about this, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span></span>

<span data-ttu-id="915ec-204">Stöd för Apptjänst-appar finns i basic, standard eller premium Apptjänstplaner **Autoskala**.</span><span class="sxs-lookup"><span data-stu-id="915ec-204">App Service apps hosted in basic, standard or premium App Service Plans support **autoscale**.</span></span> <span data-ttu-id="915ec-205">Detta kan du konfigurera regler som övervaka App Service-plan mått och kan öka eller minska instansantalet att ange ytterligare resurser som behövs eller sparar pengar när programmet är över etablera.</span><span class="sxs-lookup"><span data-stu-id="915ec-205">This allows you to configure rules that monitor the App Service plan metrics and can increase or decrease the instance count providing additional resources as needed, or saving money when the application is over-provision.</span></span> <span data-ttu-id="915ec-206">Du kan lära dig mer om automatisk skalning här: [så skala](../monitoring-and-diagnostics/insights-how-to-scale.md) och här [bästa praxis för Azure-Monitor autoskalning](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span><span class="sxs-lookup"><span data-stu-id="915ec-206">You can learn more about auto scale here: [How to Scale](../monitoring-and-diagnostics/insights-how-to-scale.md) and here [Best practices for Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span></span>

> [!NOTE]
> <span data-ttu-id="915ec-207">Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="915ec-207">If you want to get started with Azure App Service before signing up for an Azure account, go to [Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="915ec-208">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="915ec-208">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="915ec-209">Nyheter</span><span class="sxs-lookup"><span data-stu-id="915ec-209">What's changed</span></span>
* <span data-ttu-id="915ec-210">En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="915ec-210">For a guide to the change from Websites to App Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
