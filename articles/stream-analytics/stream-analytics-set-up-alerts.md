---
title: "Konfigurera aviseringar för frågor i Stream Analytics | Microsoft Docs"
description: "Förstå Stream Analytics aviseringar"
keywords: Konfigurera aviseringar
services: stream-analytics
documentationcenter: 
author: jeffstokes72
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/26/2017
ms.author: jeffstok
ms.openlocfilehash: 75b1b256eea7295f5a464996e2f34ae301c715fd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a><span data-ttu-id="48908-104">Konfigurera aviseringar för Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="48908-104">Set up alerts for Azure Stream Analytics jobs</span></span>
## <a name="introduction-monitor-page"></a><span data-ttu-id="48908-105">: Övervakaren introduktionssida</span><span class="sxs-lookup"><span data-stu-id="48908-105">Introduction: Monitor page</span></span>
<span data-ttu-id="48908-106">Du kan konfigurera aviseringar för att utlösa en avisering när ett mått når ett villkor som du anger.</span><span class="sxs-lookup"><span data-stu-id="48908-106">You can set up alerts to trigger an alert when a metric reaches a condition that you specify.</span></span> <span data-ttu-id="48908-107">Du kan till exempel skapa en avisering om ett villkor som liknar följande:</span><span class="sxs-lookup"><span data-stu-id="48908-107">For example, you might set up an alert for a condition like the following:</span></span>

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

<span data-ttu-id="48908-108">Regler kan ställas in på mätvärden via portalen eller kan konfigureras [programmässigt](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) över Åtgärdsloggar data.</span><span class="sxs-lookup"><span data-stu-id="48908-108">Rules can be set up on metrics through the portal, or can be configured [programmatically](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) over Operation Logs data.</span></span>

## <a name="set-up-alerts-in-the-azure-portal"></a><span data-ttu-id="48908-109">Konfigurera aviseringar i Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="48908-109">Set up alerts in the Azure portal</span></span>
1. <span data-ttu-id="48908-110">Öppna Stream Analytics-jobbet som du vill skapa en avisering för i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="48908-110">In the Azure portal, open the Stream Analytics job you want to create an alert for.</span></span> 

2. <span data-ttu-id="48908-111">I den **jobbet** bladet, klickar du på den **övervakning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="48908-111">In the **Job** blade, click the **Monitoring** section.</span></span>  

3. <span data-ttu-id="48908-112">I den **mått** bladet, klickar du på den **Lägg till avisering** kommando.</span><span class="sxs-lookup"><span data-stu-id="48908-112">In the **Metric** blade, click the **Add alert** command.</span></span>

      ![Konfigurationen av Azure portal](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. <span data-ttu-id="48908-114">Ange ett namn och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="48908-114">Enter a name and a description.</span></span>

5. <span data-ttu-id="48908-115">Använd väljare för att definiera de villkor under vilka aviseringen ska skickas.</span><span class="sxs-lookup"><span data-stu-id="48908-115">Use the selectors to define the condition under which the alert will be sent.</span></span>

6. <span data-ttu-id="48908-116">Ange information om aviseringen ska var.</span><span class="sxs-lookup"><span data-stu-id="48908-116">Provide information about where the alert should go.</span></span>

      ![Ställa in en avisering för ett Azure Streaming Analytics-jobb](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

<span data-ttu-id="48908-118">Mer information om hur du konfigurerar aviseringar i Azure portal finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="48908-118">For more detail on configuring alerts in the Azure portal, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>  


## <a name="get-help"></a><span data-ttu-id="48908-119">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="48908-119">Get help</span></span>
<span data-ttu-id="48908-120">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="48908-120">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="48908-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48908-121">Next steps</span></span>
* [<span data-ttu-id="48908-122">Introduktion till Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="48908-122">Introduction to Azure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="48908-123">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="48908-123">Get started using Azure Stream Analytics</span></span>](stream-analytics-get-started.md)
* [<span data-ttu-id="48908-124">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="48908-124">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="48908-125">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="48908-125">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="48908-126">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="48908-126">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

