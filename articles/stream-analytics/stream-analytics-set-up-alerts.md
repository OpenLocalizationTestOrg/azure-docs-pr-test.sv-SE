---
title: "Konfigurera aviseringar för frågor i Stream Analytics | Microsoft Docs"
description: "Förstå Stream Analytics aviseringar"
keywords: Konfigurera aviseringar
services: stream-analytics
documentationcenter: 
author: samacha
manager: jhubbard
editor: cgronlun
ms.assetid: 9952e2cf-b335-4a5c-8f45-8d3e1eda2e20
ms.service: stream-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-services
ms.date: 06/26/2017
ms.author: samacha
ms.openlocfilehash: cdbd7da36f3eb2795fc4548915ebbf18200ca44d
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Konfigurera aviseringar för Azure Stream Analytics-jobb
## <a name="introduction-monitor-page"></a>: Övervakaren introduktionssida
Du kan konfigurera aviseringar för att utlösa en avisering när ett mått når ett villkor som du anger. Du kan till exempel skapa en avisering om ett villkor som liknar följande:

`If there are zero input events in the last 5 minutes, send email notification to sa-admin@example.com`

Regler kan ställas in på mätvärden via portalen eller kan konfigureras [programmässigt](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) över Åtgärdsloggar data.

## <a name="set-up-alerts-in-the-azure-portal"></a>Konfigurera aviseringar i Azure-portalen
1. Öppna Stream Analytics-jobbet som du vill skapa en avisering för i Azure-portalen. 

2. I den **jobbet** bladet, klickar du på den **övervakning** avsnitt.  

3. I den **mått** bladet, klickar du på den **Lägg till avisering** kommando.

      ![Konfigurationen av Azure portal](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Ange ett namn och en beskrivning.

5. Använd väljare för att definiera de villkor under vilka aviseringen ska skickas.

6. Ange information om aviseringen ska var.

      ![Ställa in en avisering för ett Azure Streaming Analytics-jobb](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Mer information om hur du konfigurerar aviseringar i Azure portal finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion till Azure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-get-started.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

