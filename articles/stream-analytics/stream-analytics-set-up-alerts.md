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
# <a name="set-up-alerts-for-azure-stream-analytics-jobs"></a>Konfigurera aviseringar för Azure Stream Analytics-jobb
## <a name="introduction-monitor-page"></a>: Övervakaren introduktionssida
Du kan ställa in aviseringar tootrigger en avisering när ett mått når ett villkor som du anger. Du kan till exempel skapa en avisering om ett villkor som hello nedan:

`If there are zero input events in hello last 5 minutes, send email notification toosa-admin@example.com`

Regler kan ställas in på mätvärden via hello-portalen eller kan konfigureras [programmässigt](https://code.msdn.microsoft.com/windowsazure/Receive-Email-Notifications-199e2c9a) över Åtgärdsloggar data.

## <a name="set-up-alerts-in-hello-azure-portal"></a>Konfigurera aviseringar i hello Azure-portalen
1. Öppna en avisering om du vill toocreate hello Stream Analytics-jobbet i hello Azure-portalen. 

2. I hello **jobbet** bladet, klickar du på hello **övervakning** avsnitt.  

3. I hello **mått** bladet, klickar du på hello **Lägg till avisering** kommando.

      ![Konfigurationen av Azure portal](./media/stream-analytics-set-up-alerts/06-stream-analytics-set-up-alerts.png)  

4. Ange ett namn och en beskrivning.

5. Använda hello väljare toodefine hello villkor under vilka hello avisering ska skickas.

6. Ange information om där hello aviseringen ska gå.

      ![Ställa in en avisering för ett Azure Streaming Analytics-jobb](./media/stream-analytics-set-up-alerts/stream-analytics-add-alert.png)  

Mer information om hur du konfigurerar aviseringar i hello Azure-portalen finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md).  


## <a name="get-help"></a>Få hjälp
Om du behöver mer hjälp kan du besöka vårt [Azure Stream Analytics-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=AzureStreamAnalytics)

## <a name="next-steps"></a>Nästa steg
* [Introduktion tooAzure Stream Analytics](stream-analytics-introduction.md)
* [Komma igång med Azure Stream Analytics](stream-analytics-get-started.md)
* [Skala Azure Stream Analytics-jobb](stream-analytics-scale-jobs.md)
* [Referens för Azure Stream Analytics-frågespråket](https://msdn.microsoft.com/library/azure/dn834998.aspx)
* [Referens för Azure Stream Analytics Management REST API](https://msdn.microsoft.com/library/azure/dn835031.aspx)

