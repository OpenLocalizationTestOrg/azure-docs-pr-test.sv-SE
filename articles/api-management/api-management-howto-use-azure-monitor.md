---
title: aaaMonitor API-hantering med Azure-Monitor | Microsoft Docs
description: "Lär dig hur toomonitor Azure API Management tjänst med hjälp av Azure-Monitor."
services: api-management
documentationcenter: 
author: miaojiang
manager: erikre
editor: 
ms.assetid: 2fa193cd-ea71-4b33-a5ca-1f55e5351e23
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/23/2017
ms.author: apimpm
ms.openlocfilehash: 5012d8ed57ea4f94ea6bc1b7c4e1102516ec4414
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-api-management-with-azure-monitor"></a><span data-ttu-id="133a2-103">Övervakare för API Management med Azure Övervakare</span><span class="sxs-lookup"><span data-stu-id="133a2-103">Monitor API Management with Azure Monitor</span></span>
<span data-ttu-id="133a2-104">Azure övervakaren är en Azure-tjänst som tillhandahåller en enda källa för övervakning av alla Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="133a2-104">Azure Monitor is an Azure service that provides a single source for monitoring all your Azure resources.</span></span> <span data-ttu-id="133a2-105">Med Azure-Monitor kan du visualisera, fråga, vidarebefordra, arkivera och vidta åtgärder på hello mått och loggar från Azure-resurser som API-hantering.</span><span class="sxs-lookup"><span data-stu-id="133a2-105">With Azure Monitor, you can visualize, query, route, archive, and take actions on hello metrics and logs coming from Azure resources such as API Management.</span></span> 

<span data-ttu-id="133a2-106">Hej följande videoklipp visar hur toomonitor API Management med hjälp av Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="133a2-106">hello following video shows how toomonitor API Management using Azure Monitor.</span></span> <span data-ttu-id="133a2-107">Läs mer om Azure-Monitor [Kom igång med Azure-Monitor].</span><span class="sxs-lookup"><span data-stu-id="133a2-107">For more information about Azure Monitor, see [Get Started with Azure Monitor].</span></span> 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a><span data-ttu-id="133a2-108">Mått</span><span class="sxs-lookup"><span data-stu-id="133a2-108">Metrics</span></span>
<span data-ttu-id="133a2-109">API-hantering för närvarande skickar fem mått och vi planerar tooadd mer i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="133a2-109">API Management currently emits five metrics and we plan tooadd more in hello future.</span></span> <span data-ttu-id="133a2-110">De här måtten släpps varje minut, vilket ger dig nära realtid insyn i hello tillstånd och hälsotillståndet för dina API: er.</span><span class="sxs-lookup"><span data-stu-id="133a2-110">These metrics are emitted every minute, giving you near real-time visibility into hello state and health of your APIs.</span></span> <span data-ttu-id="133a2-111">Nedan följer en sammanfattning av hello mått:</span><span class="sxs-lookup"><span data-stu-id="133a2-111">Following is a summary of hello metrics:</span></span>
* <span data-ttu-id="133a2-112">Totalt antal begäranden som Gateway: hello antalet API-begäranden i hello period.</span><span class="sxs-lookup"><span data-stu-id="133a2-112">Total Gateway Requests: hello number of API requests in hello period.</span></span> 
* <span data-ttu-id="133a2-113">Slutförda förfrågningar som Gateway: hello antalet API-begäranden som tas emot lyckade HTTP-svarskoder inklusive 304, 307 och mindre än 301 (till exempel 200).</span><span class="sxs-lookup"><span data-stu-id="133a2-113">Successful Gateway Requests: hello number of API requests that received successful HTTP response codes including 304, 307 and anything smaller than 301 (for example, 200).</span></span> 
* <span data-ttu-id="133a2-114">Misslyckade begäranden för gatewayen: hello antalet API-begäranden som tas emot felaktiga HTTP-svarskoder inklusive 400 och något som är större än 500.</span><span class="sxs-lookup"><span data-stu-id="133a2-114">Failed Gateway Requests: hello number of API requests that received erroneous HTTP response codes including 400 and anything larger than 500.</span></span>
* <span data-ttu-id="133a2-115">Obehörig Gateway begäranden: hello antalet API-begäranden som tas emot HTTP-svarskoder inklusive 401, 403 och 429.</span><span class="sxs-lookup"><span data-stu-id="133a2-115">Unauthorized Gateway Requests: hello number of API requests that received HTTP response codes including 401, 403, and 429.</span></span> 
* <span data-ttu-id="133a2-116">Andra Gateway-begäranden: hello antalet API-begäranden som tas emot HTTP-svarskoder som inte tillhör tooany av hello föregående kategorier (till exempel 418).</span><span class="sxs-lookup"><span data-stu-id="133a2-116">Other Gateway Requests: hello number of API requests that received HTTP response codes that do not belong tooany of hello preceding categories (for example, 418).</span></span>

<span data-ttu-id="133a2-117">Du kan komma åt mått i API Management-tjänsten eller åtkomst mätvärden för alla dina Azure-resurser i Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="133a2-117">You can access metrics in your API Management service, or access metrics of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="133a2-118">tooview mått i API Management-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="133a2-118">tooview metrics in your API Management service:</span></span>
1. <span data-ttu-id="133a2-119">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="133a2-119">Open hello Azure portal.</span></span>
2. <span data-ttu-id="133a2-120">Gå tooyour API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="133a2-120">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="133a2-121">Klicka på **mått**.</span><span class="sxs-lookup"><span data-stu-id="133a2-121">Click **Metrics**.</span></span>

![Mått bladet][metrics-blade]

<span data-ttu-id="133a2-123">Mer information om hur toouse statistik, se [översikt av mått].</span><span class="sxs-lookup"><span data-stu-id="133a2-123">For more information about how toouse Metrics, see [Overview of Metrics].</span></span>

## <a name="activity-logs"></a><span data-ttu-id="133a2-124">Aktivitetsloggar</span><span class="sxs-lookup"><span data-stu-id="133a2-124">Activity Logs</span></span>
<span data-ttu-id="133a2-125">Aktivitetsloggar ger kunskaper om hello-åtgärder som utfördes på din API Management-tjänster.</span><span class="sxs-lookup"><span data-stu-id="133a2-125">Activity logs provide insight into hello operations that were performed on your API Management services.</span></span> <span data-ttu-id="133a2-126">Det som tidigare kallades ”granskningsloggar” eller ”arbetsloggarna”.</span><span class="sxs-lookup"><span data-stu-id="133a2-126">It was previously known as "audit logs" or "operational logs".</span></span> <span data-ttu-id="133a2-127">Använder aktivitetsloggar, kan du bestämma hello ”vad som, och när” för alla skrivåtgärder (PUT, POST, ta bort) vidtas för API Management-tjänster.</span><span class="sxs-lookup"><span data-stu-id="133a2-127">Using activity logs, you can determine hello "what, who, and when" for any write operations (PUT, POST, DELETE) taken on your API Management services.</span></span> 

> [!NOTE]
> <span data-ttu-id="133a2-128">Aktivitetsloggar inkluderar inte läsåtgärder (GET) eller åtgärder som utförs i hello klassiska Publisher-portalen eller med hjälp av hello ursprungliga Management API: er.</span><span class="sxs-lookup"><span data-stu-id="133a2-128">Activity logs do not include read (GET) operations or operations performed in hello classic Publisher Portal or using hello original Management APIs.</span></span>

<span data-ttu-id="133a2-129">Du kan komma åt aktivitetsloggar i API Management-tjänsten eller komma åt loggarna för alla dina Azure-resurser i Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="133a2-129">You can access activity logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="133a2-130">tooview aktivitet loggar i API Management-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="133a2-130">tooview activity logs in your API Management service:</span></span>
1. <span data-ttu-id="133a2-131">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="133a2-131">Open hello Azure portal.</span></span>
2. <span data-ttu-id="133a2-132">Gå tooyour API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="133a2-132">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="133a2-133">Klicka på **aktivitetsloggen**.</span><span class="sxs-lookup"><span data-stu-id="133a2-133">Click **Activity log**.</span></span>

![Aktiviteten loggar bladet][activity-logs-blade]

<span data-ttu-id="133a2-135">Mer information om hur toouse statistik, se [översikt aktivitetsloggar].</span><span class="sxs-lookup"><span data-stu-id="133a2-135">For more information about how toouse Metrics, see [Overview of Activity Logs].</span></span>

## <a name="alerts"></a><span data-ttu-id="133a2-136">Aviseringar</span><span class="sxs-lookup"><span data-stu-id="133a2-136">Alerts</span></span>
<span data-ttu-id="133a2-137">Du kan konfigurera tooreceive varningar baserat på mått och aktivitet.</span><span class="sxs-lookup"><span data-stu-id="133a2-137">You can configure tooreceive alerts based on metrics and activity logs.</span></span> <span data-ttu-id="133a2-138">Azure övervakaren kan en avisering toodo hello efter när den utlöser tooconfigure:</span><span class="sxs-lookup"><span data-stu-id="133a2-138">Azure Monitor allows you tooconfigure an alert toodo hello following when it triggers:</span></span>

* <span data-ttu-id="133a2-139">Skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="133a2-139">Send an email notification</span></span>
* <span data-ttu-id="133a2-140">anropa en webhook</span><span class="sxs-lookup"><span data-stu-id="133a2-140">Call a webhook</span></span>
* <span data-ttu-id="133a2-141">Anropa en Azure Logikapp</span><span class="sxs-lookup"><span data-stu-id="133a2-141">Invoke an Azure Logic App</span></span>

<span data-ttu-id="133a2-142">Du kan konfigurera Varningsregler i API Management-tjänsten, eller i Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="133a2-142">You can configure alert rules in your API Management service, or in Azure Monitor.</span></span> <span data-ttu-id="133a2-143">tooconfigure dem i API-hantering:</span><span class="sxs-lookup"><span data-stu-id="133a2-143">tooconfigure them in API Management:</span></span> 
1. <span data-ttu-id="133a2-144">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="133a2-144">Open hello Azure portal.</span></span>
2. <span data-ttu-id="133a2-145">Gå tooyour API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="133a2-145">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="133a2-146">Klicka på **Varna regler**.</span><span class="sxs-lookup"><span data-stu-id="133a2-146">Click **Alert rules**.</span></span>

![Varningsregler bladet][alert-rules-blade]

<span data-ttu-id="133a2-148">Mer information om hur du använder aviseringar finns [översikt över aviseringar].</span><span class="sxs-lookup"><span data-stu-id="133a2-148">For more information about using Alerts, see [Overview of Alerts].</span></span>

## <a name="diagnostic-logs"></a><span data-ttu-id="133a2-149">Diagnostikloggar</span><span class="sxs-lookup"><span data-stu-id="133a2-149">Diagnostic Logs</span></span>
<span data-ttu-id="133a2-150">Diagnostikloggar innehåller omfattande information om åtgärder och fel som är viktiga för granskning, samt i felsökningssyfte.</span><span class="sxs-lookup"><span data-stu-id="133a2-150">Diagnostic logs provide rich information about operations and errors that are important for auditing as well as troubleshooting purposes.</span></span> <span data-ttu-id="133a2-151">Diagnostik loggar skiljer sig från aktivitetsloggar.</span><span class="sxs-lookup"><span data-stu-id="133a2-151">Diagnostics logs differ from activity logs.</span></span> <span data-ttu-id="133a2-152">Aktivitetsloggar ger insikter om hello-åtgärder som utfördes på Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="133a2-152">Activity logs provide insights into hello operations that were performed on your Azure resources.</span></span> <span data-ttu-id="133a2-153">Diagnostik-loggarna ger information om åtgärder som din resurs utförde sig själv.</span><span class="sxs-lookup"><span data-stu-id="133a2-153">Diagnostics logs provide insight into operations that your resource performed itself.</span></span>

<span data-ttu-id="133a2-154">API-hantering för närvarande tillhandahåller diagnostik loggar (batchar per timme) för enskilda API begäran med varje post med hello följande struktur:</span><span class="sxs-lookup"><span data-stu-id="133a2-154">API Management currently provides diagnostics logs (batched hourly) about individual API request with each entry having hello following structure:</span></span>

```
{
    "Tenant": "",
      "DeploymentName": "",
      "time": "",
      "resourceId": "",
      "category": "GatewayLogs",
      "operationName": "Microsoft.ApiManagement/GatewayLogs",
      "durationMs": ,
      "Level": ,
      "properties": "{
          "ApiId": "",
          "OperationId": "",
          "ProductId": "",
          "SubscriptionId": "",
          "Method": "",
          "Url": "",
          "RequestSize": ,
          "ServiceTime": "",
          "BackendMethod": "",
          "BackendUrl": "",
          "BackendResponseCode": ,
          "ResponseCode": ,
          "ResponseSize": ,
          "Cache": "",
          "UserId"
      }"
 }
```

<span data-ttu-id="133a2-155">Du kan komma åt diagnostikloggar i API Management-tjänsten eller komma åt loggarna för alla dina Azure-resurser i Azure-Monitor.</span><span class="sxs-lookup"><span data-stu-id="133a2-155">You can access diagnostic logs in your API Management service, or access logs of all your Azure resources in Azure Monitor.</span></span> <span data-ttu-id="133a2-156">tooview diagnostikloggar i API Management-tjänsten:</span><span class="sxs-lookup"><span data-stu-id="133a2-156">tooview diagnostic logs in your API Management service:</span></span>
1. <span data-ttu-id="133a2-157">Öppna hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="133a2-157">Open hello Azure portal.</span></span>
2. <span data-ttu-id="133a2-158">Gå tooyour API Management-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="133a2-158">Go tooyour API Management service.</span></span>
3. <span data-ttu-id="133a2-159">Klicka på **diagnostiska loggen**.</span><span class="sxs-lookup"><span data-stu-id="133a2-159">Click **Diagnostic log**.</span></span>

![Bladet Diagnostikloggar][diagnostic-logs-blade]

<span data-ttu-id="133a2-161">Mer information om hur toouse statistik, se [översikt av diagnostikloggar].</span><span class="sxs-lookup"><span data-stu-id="133a2-161">For more information about how toouse Metrics, see [Overview of Diagnostic Logs].</span></span>

## <a name="next-step"></a><span data-ttu-id="133a2-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="133a2-162">Next Step</span></span>

* <span data-ttu-id="133a2-163">[Kom igång med Azure Monitor]</span><span class="sxs-lookup"><span data-stu-id="133a2-163">[Get Started with Azure Monitor]</span></span>
* <span data-ttu-id="133a2-164">[Översikt över mått]</span><span class="sxs-lookup"><span data-stu-id="133a2-164">[Overview of Metrics]</span></span>
* <span data-ttu-id="133a2-165">[Översikt över aktivitetsloggar]</span><span class="sxs-lookup"><span data-stu-id="133a2-165">[Overview of Activity Logs]</span></span>
* <span data-ttu-id="133a2-166">[Översikt över diagnostikloggar]</span><span class="sxs-lookup"><span data-stu-id="133a2-166">[Overview of Diagnostic Logs]</span></span>
* <span data-ttu-id="133a2-167">[Översikt över aviseringar]</span><span class="sxs-lookup"><span data-stu-id="133a2-167">[Overview of Alerts]</span></span>

[Kom igång med Azure Monitor]: ../monitoring-and-diagnostics/monitoring-get-started.md
[Översikt över mått]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[Översikt över aktivitetsloggar]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[Översikt över diagnostikloggar]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[Översikt över aviseringar]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
