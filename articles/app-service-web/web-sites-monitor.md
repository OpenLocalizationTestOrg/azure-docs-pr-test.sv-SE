---
title: aaaMonitor appar i Azure App Service | Microsoft Docs
description: "Lär dig hur toomonitor appar i Azure App Service med hjälp av hello Azure-portalen."
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
ms.openlocfilehash: 80d5a466102a894a49d04ae35aa54cc1d05a58df
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-to-monitor-apps-in-azure-app-service"></a><span data-ttu-id="6a15e-103">Så här: övervaka appar i Azure App Service</span><span class="sxs-lookup"><span data-stu-id="6a15e-103">How to: Monitor Apps in Azure App Service</span></span>
<span data-ttu-id="6a15e-104">[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) har inbyggda funktioner som övervakning för hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a15e-104">[App Service](http://go.microsoft.com/fwlink/?LinkId=529714) provides built in monitoring functionality in hello [Azure Portal](https://portal.azure.com).</span></span>
<span data-ttu-id="6a15e-105">Detta inkluderar hello möjlighet tooreview **kvoter** och **mått** för en app, samt hello App Service-plan, ställa in **aviseringar** och även **skalning** automatiskt baserat på de här måtten.</span><span class="sxs-lookup"><span data-stu-id="6a15e-105">This includes hello ability tooreview **quotas** and **metrics** for an app as well as hello App Service plan, setting up **alerts** and even **scaling** automatically based on these metrics.</span></span>

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a><span data-ttu-id="6a15e-106">Förstå kvoter och mått</span><span class="sxs-lookup"><span data-stu-id="6a15e-106">Understanding Quotas and Metrics</span></span>
### <a name="quotas"></a><span data-ttu-id="6a15e-107">Kvoter</span><span class="sxs-lookup"><span data-stu-id="6a15e-107">Quotas</span></span>
<span data-ttu-id="6a15e-108">Program som finns i App Service är ämne toocertain *gränser* för de resurser som de kan använda.</span><span class="sxs-lookup"><span data-stu-id="6a15e-108">Applications hosted in App Service are subject toocertain *limits* on the resources they can use.</span></span> <span data-ttu-id="6a15e-109">hello gränser har definierats av hello **programtjänstplanen** som är associerade med hello app.</span><span class="sxs-lookup"><span data-stu-id="6a15e-109">hello limits are defined by hello **App Service plan** associated with hello app.</span></span>

<span data-ttu-id="6a15e-110">Om programmet hello finns i en **lediga** eller **delade** planera sedan hello gränserna för hello resurser hello app kan använda definieras av **kvoter**.</span><span class="sxs-lookup"><span data-stu-id="6a15e-110">If hello application is hosted in a **Free** or **Shared** plan, then hello limits on hello resources hello app can use are defined by **Quotas**.</span></span>

<span data-ttu-id="6a15e-111">Om programmet hello finns i en **grundläggande**, **Standard** eller **Premium** planera sedan hello gränserna för hello resurser som de kan använda anges av hello **storlek** (Small, Medium, stora) och **instansen antal** (1, 2, 3,...) av hello **programtjänstplanen**.</span><span class="sxs-lookup"><span data-stu-id="6a15e-111">If hello application is hosted in a **Basic**, **Standard** or **Premium** plan, then hello limits on hello resources they can use are set by hello **size** (Small, Medium, Large) and **instance count** (1, 2, 3, ...) of hello **App Service plan**.</span></span>

<span data-ttu-id="6a15e-112">**Kvoter** för **lediga** eller **delade** appar är:</span><span class="sxs-lookup"><span data-stu-id="6a15e-112">**Quotas** for **Free** or **Shared** apps are:</span></span>

* <span data-ttu-id="6a15e-113">**CPU(short)**</span><span class="sxs-lookup"><span data-stu-id="6a15e-113">**CPU(Short)**</span></span>
  * <span data-ttu-id="6a15e-114">Mängden CPU som tillåts för det här programmet i en 5 minuter.</span><span class="sxs-lookup"><span data-stu-id="6a15e-114">Amount of CPU allowed for this application in a 5-minute interval.</span></span> <span data-ttu-id="6a15e-115">Den här kvoten anger igen var femte minut.</span><span class="sxs-lookup"><span data-stu-id="6a15e-115">This quota re-sets every 5 minutes.</span></span>
* <span data-ttu-id="6a15e-116">**CPU(Day)**</span><span class="sxs-lookup"><span data-stu-id="6a15e-116">**CPU(Day)**</span></span>
  * <span data-ttu-id="6a15e-117">Totalt antal processorer som tillåts för det här programmet i en dag.</span><span class="sxs-lookup"><span data-stu-id="6a15e-117">Total amount of CPU allowed for this application in a day.</span></span> <span data-ttu-id="6a15e-118">Den här kvoten anger igen var 24: e timme vid midnatt UTC.</span><span class="sxs-lookup"><span data-stu-id="6a15e-118">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="6a15e-119">**Minne**</span><span class="sxs-lookup"><span data-stu-id="6a15e-119">**Memory**</span></span>
  * <span data-ttu-id="6a15e-120">Totala mängden minne som tillåts för det här programmet.</span><span class="sxs-lookup"><span data-stu-id="6a15e-120">Total amount of memory allowed for this application.</span></span>
* <span data-ttu-id="6a15e-121">**Bandbredd**</span><span class="sxs-lookup"><span data-stu-id="6a15e-121">**Bandwidth**</span></span>
  * <span data-ttu-id="6a15e-122">Totalt antal utgående bandbredd som tillåts för det här programmet i en dag.</span><span class="sxs-lookup"><span data-stu-id="6a15e-122">Total amount of outgoing bandwidth allowed for this application in a day.</span></span>
    <span data-ttu-id="6a15e-123">Den här kvoten anger igen var 24: e timme vid midnatt UTC.</span><span class="sxs-lookup"><span data-stu-id="6a15e-123">This quota re-sets every 24 hours at midnight UTC.</span></span>
* <span data-ttu-id="6a15e-124">**Filsystem**</span><span class="sxs-lookup"><span data-stu-id="6a15e-124">**Filesystem**</span></span>
  * <span data-ttu-id="6a15e-125">Totalt antal tillåtet lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="6a15e-125">Total amount of storage allowed.</span></span>

<span data-ttu-id="6a15e-126">Hej endast kvoten tillämpliga tooapps finns på **grundläggande**, **Standard** och **Premium** planer är **Filesystem**.</span><span class="sxs-lookup"><span data-stu-id="6a15e-126">hello only quota applicable tooapps hosted on **Basic**, **Standard** and **Premium** plans is **Filesystem**.</span></span>

<span data-ttu-id="6a15e-127">Mer information om hello specifika kvoter, gränser och funktioner som är tillgängliga för hello olika App Service SKU: er hittar du här: [Tjänstbegränsningarna för Azure-prenumeration](../azure-subscription-service-limits.md#app-service-limits)</span><span class="sxs-lookup"><span data-stu-id="6a15e-127">More information about hello specific quotas, limits and features available to hello different App Service SKUs can be found here: [Azure Subscription Service Limits](../azure-subscription-service-limits.md#app-service-limits)</span></span>

#### <a name="quota-enforcement"></a><span data-ttu-id="6a15e-128">Kvoter</span><span class="sxs-lookup"><span data-stu-id="6a15e-128">Quota Enforcement</span></span>
<span data-ttu-id="6a15e-129">Om ett program i användningen överskrider hello **CPU (korta)**, **CPU (dag)**, eller **bandbredd** kvot sedan hello programmet kommer att stoppas tills hello kvoten anger igen.</span><span class="sxs-lookup"><span data-stu-id="6a15e-129">If an application in its usage exceeds hello **CPU (short)**, **CPU (Day)**, or **bandwidth** quota then hello application will be stopped until hello quota re-sets.</span></span> <span data-ttu-id="6a15e-130">Under denna tid kan alla förfrågningar som leder till en **HTTP 403**.</span><span class="sxs-lookup"><span data-stu-id="6a15e-130">During this time, all incoming requests will result in an **HTTP 403**.</span></span>
![][http403]

<span data-ttu-id="6a15e-131">Om programmet hello **minne** kvot har överskridits och sedan hello programmet blir startats.</span><span class="sxs-lookup"><span data-stu-id="6a15e-131">If hello application **memory** quota is exceeded, then hello application will be re-started.</span></span>

<span data-ttu-id="6a15e-132">Om hello **Filesystem** kvot har överskridits och sedan någon skriva misslyckas åtgärden, inklusive skriva toologs.</span><span class="sxs-lookup"><span data-stu-id="6a15e-132">If hello **Filesystem** quota is exceeded, then any write operation will fail, this includes writing toologs.</span></span>

<span data-ttu-id="6a15e-133">Kvoter kan ökas eller tas bort från din app genom att uppgradera din programtjänstplan.</span><span class="sxs-lookup"><span data-stu-id="6a15e-133">Quotas can be increased or removed from your app by upgrading your App Service plan.</span></span>

### <a name="metrics"></a><span data-ttu-id="6a15e-134">Mått</span><span class="sxs-lookup"><span data-stu-id="6a15e-134">Metrics</span></span>
<span data-ttu-id="6a15e-135">**Mått** innehåller information om hello appen eller en App Service-plan beteende.</span><span class="sxs-lookup"><span data-stu-id="6a15e-135">**Metrics** provide information about hello app, or App Service plan's behavior.</span></span>

<span data-ttu-id="6a15e-136">För en **programmet**, hello tillgängliga mått är:</span><span class="sxs-lookup"><span data-stu-id="6a15e-136">For an **Application**, hello available metrics are:</span></span>

* <span data-ttu-id="6a15e-137">**Genomsnittlig svarstid**</span><span class="sxs-lookup"><span data-stu-id="6a15e-137">**Average Response Time**</span></span>
  * <span data-ttu-id="6a15e-138">hello Genomsnittlig tid för hello app tooserve begäranden i ms.</span><span class="sxs-lookup"><span data-stu-id="6a15e-138">hello average time taken for hello app tooserve requests in ms.</span></span>
* <span data-ttu-id="6a15e-139">**Genomsnittlig minne arbetsminne**</span><span class="sxs-lookup"><span data-stu-id="6a15e-139">**Average memory working set**</span></span>
  * <span data-ttu-id="6a15e-140">hello genomsnittlig mängd minne i MIB som används av hello app.</span><span class="sxs-lookup"><span data-stu-id="6a15e-140">hello average amount of memory in MiBs used by hello app.</span></span>
* <span data-ttu-id="6a15e-141">**CPU-tid**</span><span class="sxs-lookup"><span data-stu-id="6a15e-141">**CPU Time**</span></span>
  * <span data-ttu-id="6a15e-142">hello mycket Processorkraft i sekunder som används av hello app.</span><span class="sxs-lookup"><span data-stu-id="6a15e-142">hello amount of CPU in seconds consumed by hello app.</span></span> <span data-ttu-id="6a15e-143">Mer information om den här mått finns: [vs CPU CPU-tid i procent](#cpu-time-vs-cpu-percentage)</span><span class="sxs-lookup"><span data-stu-id="6a15e-143">For more information about this metric see: [CPU time vs CPU percentage](#cpu-time-vs-cpu-percentage)</span></span>
* <span data-ttu-id="6a15e-144">**Data i**</span><span class="sxs-lookup"><span data-stu-id="6a15e-144">**Data In**</span></span>
  * <span data-ttu-id="6a15e-145">hello mängden inkommande bandbredd som används av hello app i MIB.</span><span class="sxs-lookup"><span data-stu-id="6a15e-145">hello amount of incoming bandwidth consumed by hello app in MiBs.</span></span>
* <span data-ttu-id="6a15e-146">**Ut data**</span><span class="sxs-lookup"><span data-stu-id="6a15e-146">**Data Out**</span></span>
  * <span data-ttu-id="6a15e-147">hello mängden utgående bandbredd som används av hello app i MIB.</span><span class="sxs-lookup"><span data-stu-id="6a15e-147">hello amount of outgoing bandwidth consumed by hello app in MiBs.</span></span>
* <span data-ttu-id="6a15e-148">**HTTP-2xx**</span><span class="sxs-lookup"><span data-stu-id="6a15e-148">**Http 2xx**</span></span>
  * <span data-ttu-id="6a15e-149">Antal begäranden som resulterar i en http-statuskod > = 200 men < 300.</span><span class="sxs-lookup"><span data-stu-id="6a15e-149">Count of requests resulting in a http status code >= 200 but < 300.</span></span>
* <span data-ttu-id="6a15e-150">**HTTP-3xx**</span><span class="sxs-lookup"><span data-stu-id="6a15e-150">**Http 3xx**</span></span>
  * <span data-ttu-id="6a15e-151">Antal begäranden som resulterar i en http-statuskod > = 300 men < 400.</span><span class="sxs-lookup"><span data-stu-id="6a15e-151">Count of requests resulting in a http status code >= 300 but < 400.</span></span>
* <span data-ttu-id="6a15e-152">**HTTP 401**</span><span class="sxs-lookup"><span data-stu-id="6a15e-152">**Http 401**</span></span>
  * <span data-ttu-id="6a15e-153">Antal begäranden som ledde till ett HTTP 401-statuskod.</span><span class="sxs-lookup"><span data-stu-id="6a15e-153">Count of requests resulting in HTTP 401 status code.</span></span>
* <span data-ttu-id="6a15e-154">**HTTP 403**</span><span class="sxs-lookup"><span data-stu-id="6a15e-154">**Http 403**</span></span>
  * <span data-ttu-id="6a15e-155">Antal begäranden som ledde till ett 403 HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="6a15e-155">Count of requests resulting in HTTP 403 status code.</span></span>
* <span data-ttu-id="6a15e-156">**HTTP 404**</span><span class="sxs-lookup"><span data-stu-id="6a15e-156">**Http 404**</span></span>
  * <span data-ttu-id="6a15e-157">Antal begäranden som ledde till ett 404 HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="6a15e-157">Count of requests resulting in HTTP 404 status code.</span></span>
* <span data-ttu-id="6a15e-158">**HTTP 406**</span><span class="sxs-lookup"><span data-stu-id="6a15e-158">**Http 406**</span></span>
  * <span data-ttu-id="6a15e-159">Antal begäranden som resulterar i 406 HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="6a15e-159">Count of requests resulting in HTTP 406 status code.</span></span>
* <span data-ttu-id="6a15e-160">**HTTP-4xx**</span><span class="sxs-lookup"><span data-stu-id="6a15e-160">**Http 4xx**</span></span>
  * <span data-ttu-id="6a15e-161">Antal begäranden som resulterar i en http-statuskod > = 400 men < 500.</span><span class="sxs-lookup"><span data-stu-id="6a15e-161">Count of requests resulting in a http status code >= 400 but < 500.</span></span>
* <span data-ttu-id="6a15e-162">**HTTP-fel**</span><span class="sxs-lookup"><span data-stu-id="6a15e-162">**Http Server Errors**</span></span>
  * <span data-ttu-id="6a15e-163">Antal begäranden som resulterar i en http-statuskod > = 500, men < 600.</span><span class="sxs-lookup"><span data-stu-id="6a15e-163">Count of requests resulting in a http status code >= 500 but < 600.</span></span>
* <span data-ttu-id="6a15e-164">**Arbetsminnet för minne**</span><span class="sxs-lookup"><span data-stu-id="6a15e-164">**Memory working set**</span></span>
  * <span data-ttu-id="6a15e-165">Aktuell mängd minne som används av hello app i MIB.</span><span class="sxs-lookup"><span data-stu-id="6a15e-165">Current amount of memory used by hello app in MiBs.</span></span>
* <span data-ttu-id="6a15e-166">**Begäranden**</span><span class="sxs-lookup"><span data-stu-id="6a15e-166">**Requests**</span></span>
  * <span data-ttu-id="6a15e-167">Totalt antal begäranden oavsett deras resulterande HTTP-statuskod.</span><span class="sxs-lookup"><span data-stu-id="6a15e-167">Total number of requests regardless of their resulting HTTP status code.</span></span>

<span data-ttu-id="6a15e-168">För en **programtjänstplanen**, hello tillgängliga mått är:</span><span class="sxs-lookup"><span data-stu-id="6a15e-168">For an **App Service plan**, hello available metrics are:</span></span>

> [!NOTE]
> <span data-ttu-id="6a15e-169">App Service-plan mått är bara tillgängliga för planer i **grundläggande**, **Standard** och **Premium** SKU.</span><span class="sxs-lookup"><span data-stu-id="6a15e-169">App Service plan metrics are only available for plans in **Basic**, **Standard** and **Premium** SKU.</span></span>
> 
> 

* <span data-ttu-id="6a15e-170">**CPU-procent**</span><span class="sxs-lookup"><span data-stu-id="6a15e-170">**CPU Percentage**</span></span>
  * <span data-ttu-id="6a15e-171">hello Genomsnittlig CPU-användning i alla instanser av hello plan.</span><span class="sxs-lookup"><span data-stu-id="6a15e-171">hello average CPU used across all instances of hello plan.</span></span>
* <span data-ttu-id="6a15e-172">**Minnesprocent**</span><span class="sxs-lookup"><span data-stu-id="6a15e-172">**Memory Percentage**</span></span>
  * <span data-ttu-id="6a15e-173">hello genomsnittlig minne som används i alla instanser av hello plan.</span><span class="sxs-lookup"><span data-stu-id="6a15e-173">hello average memory used across all instances of hello plan.</span></span>
* <span data-ttu-id="6a15e-174">**Data i**</span><span class="sxs-lookup"><span data-stu-id="6a15e-174">**Data In**</span></span>
  * <span data-ttu-id="6a15e-175">hello genomsnittlig inkommande bandbredden som används i alla instanser av hello plan.</span><span class="sxs-lookup"><span data-stu-id="6a15e-175">hello average incoming bandwidth used across all instances of hello plan.</span></span>
* <span data-ttu-id="6a15e-176">**Ut data**</span><span class="sxs-lookup"><span data-stu-id="6a15e-176">**Data Out**</span></span>
  * <span data-ttu-id="6a15e-177">hello medelvärdet utgående bandbredden som används i alla instanser av hello plan.</span><span class="sxs-lookup"><span data-stu-id="6a15e-177">hello average outgoing bandwidth used across all instances of hello plan.</span></span>
* <span data-ttu-id="6a15e-178">**Diskkölängd**</span><span class="sxs-lookup"><span data-stu-id="6a15e-178">**Disk Queue Length**</span></span>
  * <span data-ttu-id="6a15e-179">hello Genomsnittligt antal både Skriv- och läsbegäranden som köade på lagring.</span><span class="sxs-lookup"><span data-stu-id="6a15e-179">hello average number of both read and write requests that were queued on storage.</span></span> <span data-ttu-id="6a15e-180">En hög diskkölängd är en indikation på ett program som kan långsammare på grund av i/o för tooexcessive disk.</span><span class="sxs-lookup"><span data-stu-id="6a15e-180">A high disk queue length is an indication of an application that might be slowing down due tooexcessive disk I/O.</span></span>
* <span data-ttu-id="6a15e-181">**HTTP-Kölängd**</span><span class="sxs-lookup"><span data-stu-id="6a15e-181">**Http Queue Length**</span></span>
  * <span data-ttu-id="6a15e-182">hello Genomsnittligt antal HTTP-begäranden som hade toosit hello kön innan uppfylls.</span><span class="sxs-lookup"><span data-stu-id="6a15e-182">hello average number of HTTP requests that had toosit on hello queue before being fulfilled.</span></span> <span data-ttu-id="6a15e-183">En hög eller ökande HTTP Kölängd är ett symtom på en plan vid hög belastning.</span><span class="sxs-lookup"><span data-stu-id="6a15e-183">A high or increasing HTTP Queue length is a symptom of a plan under heavy load.</span></span>

### <a name="cpu-time-vs-cpu-percentage"></a><span data-ttu-id="6a15e-184">Vs CPU CPU-tid i procent</span><span class="sxs-lookup"><span data-stu-id="6a15e-184">CPU time vs CPU percentage</span></span>
<!-- toodo: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

<span data-ttu-id="6a15e-185">Det finns 2 mått som avspeglar CPU-användning.</span><span class="sxs-lookup"><span data-stu-id="6a15e-185">There are 2 metrics that reflect CPU usage.</span></span> <span data-ttu-id="6a15e-186">**CPU-tid** och **CPU-procent**</span><span class="sxs-lookup"><span data-stu-id="6a15e-186">**CPU time** and **CPU percentage**</span></span>

<span data-ttu-id="6a15e-187">**CPU-tid** är användbart för appar som finns i **lediga** eller **delade** planer eftersom en av sina kvoter definieras i CPU minuter som används av hello app.</span><span class="sxs-lookup"><span data-stu-id="6a15e-187">**CPU Time** is useful for apps hosted in **Free** or **Shared** plans since one of their quotas is defined in CPU minutes used by hello app.</span></span>

<span data-ttu-id="6a15e-188">**CPU-procent** på hello andra sidan är användbart för appar som finns i **grundläggande**, **standard** och **premium** planer eftersom de kan skaländras ut och mätvärdet är en bra indikation på hello totala användningen i alla instanser.</span><span class="sxs-lookup"><span data-stu-id="6a15e-188">**CPU percentage** on hello other hand is useful for apps hosted in **basic**, **standard** and **premium** plans since they can be scaled out and this metric is a good indication of hello overall usage across all instances.</span></span>

## <a name="metrics-granularity-and-retention-policy"></a><span data-ttu-id="6a15e-189">Mått granularitet och bevarandeprincip</span><span class="sxs-lookup"><span data-stu-id="6a15e-189">Metrics Granularity and Retention Policy</span></span>
<span data-ttu-id="6a15e-190">Mått för ett program och app service-plan loggas och sammanställs efter hello tjänsten med hello efter granulariteter och bevarandeprinciper:</span><span class="sxs-lookup"><span data-stu-id="6a15e-190">Metrics for an application and app service plan are logged and aggregated by hello service with hello following granularities and retention policies:</span></span>

* <span data-ttu-id="6a15e-191">**Minut** granularitet mått bevaras för **48 timmarna**</span><span class="sxs-lookup"><span data-stu-id="6a15e-191">**Minute** granularity metrics are retained for **48 hours**</span></span>
* <span data-ttu-id="6a15e-192">**Timme** granularitet mått bevaras för **30 dagar**</span><span class="sxs-lookup"><span data-stu-id="6a15e-192">**Hour** granularity metrics are retained for **30 days**</span></span>
* <span data-ttu-id="6a15e-193">**Dag** granularitet mått bevaras för **90 dagar**</span><span class="sxs-lookup"><span data-stu-id="6a15e-193">**Day** granularity metrics are retained for **90 days**</span></span>

## <a name="monitoring-quotas-and-metrics-in-hello-azure-portal"></a><span data-ttu-id="6a15e-194">Övervakning av kvoter och mått i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6a15e-194">Monitoring Quotas and Metrics in hello Azure Portal.</span></span>
<span data-ttu-id="6a15e-195">Du kan granska hello status för olika hello **kvoter** och **mått** påverkar ett program i hello [Azure Portal](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="6a15e-195">You can review hello status of hello different **quotas** and **metrics** affecting an application in hello [Azure Portal](https://portal.azure.com).</span></span>

<span data-ttu-id="6a15e-196">![][quotas]
**Kvoter** hittar du under Inställningar >**kvoter**.</span><span class="sxs-lookup"><span data-stu-id="6a15e-196">![][quotas]
**Quotas** can be found under Settings>**Quotas**.</span></span> <span data-ttu-id="6a15e-197">hello UX kan du granska: (1) hello kvoter namn, (2) intervallet för återställning, (3) den aktuella gränsen och (4) aktuella värde.</span><span class="sxs-lookup"><span data-stu-id="6a15e-197">hello UX allows you to review: (1) hello quotas name, (2) its reset interval, (3) its current limit and (4) current value.</span></span>

<span data-ttu-id="6a15e-198">![][metrics]
**Mått** kan vara åtkomst direkt från hello-resursbladet.</span><span class="sxs-lookup"><span data-stu-id="6a15e-198">![][metrics]
**Metrics** can be access directly from hello resource blade.</span></span> <span data-ttu-id="6a15e-199">Du kan också anpassa hello diagram av: (1) **klickar du på** på och välj (2) **redigera diagram**.</span><span class="sxs-lookup"><span data-stu-id="6a15e-199">You can also customize hello chart by: (1) **click** on it, and select (2) **edit chart**.</span></span>
<span data-ttu-id="6a15e-200">Härifrån kan du ändra hello (3) **tidsintervallet**, (4) **diagramtypen**, 5 **mått** toodisplay.</span><span class="sxs-lookup"><span data-stu-id="6a15e-200">From here you can change hello (3) **time range**, (4) **chart type**, and (5) **metrics** toodisplay.</span></span>  

<span data-ttu-id="6a15e-201">Du kan lära dig mer om här mått: [övervaka tjänsten](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span><span class="sxs-lookup"><span data-stu-id="6a15e-201">You can learn more about metrics here: [Monitor service metrics](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).</span></span>

## <a name="alerts-and-autoscale"></a><span data-ttu-id="6a15e-202">Aviseringar och Autoskala</span><span class="sxs-lookup"><span data-stu-id="6a15e-202">Alerts and Autoscale</span></span>
<span data-ttu-id="6a15e-203">Mätvärden för en App eller App Service-plan kan vara ansluten tooalerts toolearn mer om det finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span><span class="sxs-lookup"><span data-stu-id="6a15e-203">Metrics for an App or App Service plan can be hooked up tooalerts, toolearn more about this, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)</span></span>

<span data-ttu-id="6a15e-204">Stöd för Apptjänst-appar finns i basic, standard eller premium Apptjänstplaner **Autoskala**.</span><span class="sxs-lookup"><span data-stu-id="6a15e-204">App Service apps hosted in basic, standard or premium App Service Plans support **autoscale**.</span></span> <span data-ttu-id="6a15e-205">Detta ger dig tooconfigure regler som övervaka App Service-plan mått och kan öka eller minska hello instansantal för att ange ytterligare resurser som behövs eller sparar pengar när programmet hello är över etablera.</span><span class="sxs-lookup"><span data-stu-id="6a15e-205">This allows you tooconfigure rules that monitor the App Service plan metrics and can increase or decrease hello instance count providing additional resources as needed, or saving money when hello application is over-provision.</span></span> <span data-ttu-id="6a15e-206">Du kan lära dig mer om automatisk skalning här: [hur tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) och här [bästa praxis för Azure-Monitor autoskalning](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span><span class="sxs-lookup"><span data-stu-id="6a15e-206">You can learn more about auto scale here: [How tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) and here [Best practices for Azure Monitor autoscaling](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)</span></span>

> [!NOTE]
> <span data-ttu-id="6a15e-207">Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service.</span><span class="sxs-lookup"><span data-stu-id="6a15e-207">If you want tooget started with Azure App Service before signing up for an Azure account, go too[Try App Service](https://azure.microsoft.com/try/app-service/), where you can immediately create a short-lived starter web app in App Service.</span></span> <span data-ttu-id="6a15e-208">Inget kreditkort krävs, och du gör inga åtaganden.</span><span class="sxs-lookup"><span data-stu-id="6a15e-208">No credit cards required; no commitments.</span></span>
> 
> 

## <a name="whats-changed"></a><span data-ttu-id="6a15e-209">Nyheter</span><span class="sxs-lookup"><span data-stu-id="6a15e-209">What's changed</span></span>
* <span data-ttu-id="6a15e-210">En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)</span><span class="sxs-lookup"><span data-stu-id="6a15e-210">For a guide toohello change from Websites tooApp Service see: [Azure App Service and Its Impact on Existing Azure Services](http://go.microsoft.com/fwlink/?LinkId=529714)</span></span>

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
