---
title: "aaaSet in aviseringar för frågor i Stream Analytics | Microsoft Docs"
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
ms.openlocfilehash: 7b1d90d1468311186567c8518e0283ea6b88c3f3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a><span data-ttu-id="a653f-104">Konfigurera aviseringar för Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="a653f-104">Set up alerts for Azure Stream Analytics jobs</span></span>
## <a name="introduction-monitor-page"></a><span data-ttu-id="a653f-105">: Övervakaren introduktionssida</span><span class="sxs-lookup"><span data-stu-id="a653f-105">Introduction: Monitor page</span></span>
<span data-ttu-id="a653f-106">Du kan ställa in aviseringar tootrigger en avisering när ett mått når ett villkor som du anger.</span><span class="sxs-lookup"><span data-stu-id="a653f-106">You can set up alerts tootrigger an alert when a metric reaches a condition that you specify.</span></span> <span data-ttu-id="a653f-107">Du kan till exempel skapa en avisering om ett villkor som hello nedan:</span><span class="sxs-lookup"><span data-stu-id="a653f-107">For example, you might set up an alert for a condition like hello following:</span></span>

`If there are zero input events in hello last 5 minutes, send email notification toosa-admin@example.com`

<span data-ttu-id="a653f-108">Regler kan ställas in på mätvärden via hello-portalen eller kan konfigureras [programmässigt](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) över Åtgärdsloggar data.</span><span class="sxs-lookup"><span data-stu-id="a653f-108">Rules can be set up on metrics through hello portal, or can be configured [programmatically](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) over Operation Logs data.</span></span>

## <a name="set-up-alerts-in-hello-azure-portal"></a><span data-ttu-id="a653f-109">Konfigurera aviseringar i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="a653f-109">Set up alerts in hello Azure portal</span></span>
1. <span data-ttu-id="a653f-110">Öppna en avisering om du vill toocreate hello Stream Analytics-jobbet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="a653f-110">In hello Azure portal, open hello Stream Analytics job you want toocreate an alert for.</span></span> 

2. <span data-ttu-id="a653f-111">I hello **jobbet** bladet, klickar du på hello **övervakning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a653f-111">In hello **Job** blade, click hello **Monitoring** section.</span></span>  

3. <span data-ttu-id="a653f-112">I hello **mått** bladet, klickar du på hello **Lägg till avisering** kommando.</span><span class="sxs-lookup"><span data-stu-id="a653f-112">In hello **Metric** blade, click hello **Add alert** command.</span></span>

      ![Konfigurationen av Azure portal](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. <span data-ttu-id="a653f-114">Ange ett namn och en beskrivning.</span><span class="sxs-lookup"><span data-stu-id="a653f-114">Enter a name and a description.</span></span>

5. <span data-ttu-id="a653f-115">Använda hello väljare toodefine hello villkor under vilka hello avisering ska skickas.</span><span class="sxs-lookup"><span data-stu-id="a653f-115">Use hello selectors toodefine hello condition under which hello alert will be sent.</span></span>

6. <span data-ttu-id="a653f-116">Ange information om där hello aviseringen ska gå.</span><span class="sxs-lookup"><span data-stu-id="a653f-116">Provide information about where hello alert should go.</span></span>

      ![Ställa in en avisering för ett Azure Streaming Analytics-jobb](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

<span data-ttu-id="a653f-118">Mer information om hur du konfigurerar aviseringar i hello Azure-portalen finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span><span class="sxs-lookup"><span data-stu-id="a653f-118">For more detail on configuring alerts in hello Azure portal, see [Receive alert notifications](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).</span></span>  


## <a name="get-help"></a><span data-ttu-id="a653f-119">Få hjälp</span><span class="sxs-lookup"><span data-stu-id="a653f-119">Get help</span></span>
<span data-ttu-id="a653f-120">Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span><span class="sxs-lookup"><span data-stu-id="a653f-120">For further assistance, try our [Azure Stream Analytics forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)</span></span>

## <a name="next-steps"></a><span data-ttu-id="a653f-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a653f-121">Next steps</span></span>
* [<span data-ttu-id="a653f-122">Introduktion tooAzure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a653f-122">Introduction tooAzure Stream Analytics</span></span>](stream-analytics-introduction.md)
* [<span data-ttu-id="a653f-123">Komma igång med Azure Stream Analytics</span><span class="sxs-lookup"><span data-stu-id="a653f-123">Get started using Azure Stream Analytics</span></span>](stream-analytics-get-started.md)
* [<span data-ttu-id="a653f-124">Skala Azure Stream Analytics-jobb</span><span class="sxs-lookup"><span data-stu-id="a653f-124">Scale Azure Stream Analytics jobs</span></span>](stream-analytics-scale-jobs.md)
* [<span data-ttu-id="a653f-125">Referens för Azure Stream Analytics-frågespråket</span><span class="sxs-lookup"><span data-stu-id="a653f-125">Azure Stream Analytics Query Language Reference</span></span>](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [<span data-ttu-id="a653f-126">Referens för Azure Stream Analytics Management REST API</span><span class="sxs-lookup"><span data-stu-id="a653f-126">Azure Stream Analytics Management REST API Reference</span></span>](https://msdn.microsoft.com/library/azure/dn835031.aspx)

