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
# <a name="monitor-api-management-with-azure-monitor"></a>Övervakare för API Management med Azure Övervakare
Azure övervakaren är en Azure-tjänst som tillhandahåller en enda källa för övervakning av alla Azure-resurser. Med Azure-Monitor kan du visualisera, fråga, vidarebefordra, arkivera och vidta åtgärder på hello mått och loggar från Azure-resurser som API-hantering. 

Hej följande videoklipp visar hur toomonitor API Management med hjälp av Azure-Monitor. Läs mer om Azure-Monitor [Kom igång med Azure-Monitor]. 


> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Monitor-API-Management-with-Azure-Monitor/player]
>
>
 
## <a name="metrics"></a>Mått
API-hantering för närvarande skickar fem mått och vi planerar tooadd mer i hello framtida. De här måtten släpps varje minut, vilket ger dig nära realtid insyn i hello tillstånd och hälsotillståndet för dina API: er. Nedan följer en sammanfattning av hello mått:
* Totalt antal begäranden som Gateway: hello antalet API-begäranden i hello period. 
* Slutförda förfrågningar som Gateway: hello antalet API-begäranden som tas emot lyckade HTTP-svarskoder inklusive 304, 307 och mindre än 301 (till exempel 200). 
* Misslyckade begäranden för gatewayen: hello antalet API-begäranden som tas emot felaktiga HTTP-svarskoder inklusive 400 och något som är större än 500.
* Obehörig Gateway begäranden: hello antalet API-begäranden som tas emot HTTP-svarskoder inklusive 401, 403 och 429. 
* Andra Gateway-begäranden: hello antalet API-begäranden som tas emot HTTP-svarskoder som inte tillhör tooany av hello föregående kategorier (till exempel 418).

Du kan komma åt mått i API Management-tjänsten eller åtkomst mätvärden för alla dina Azure-resurser i Azure-Monitor. tooview mått i API Management-tjänsten:
1. Öppna hello Azure-portalen.
2. Gå tooyour API Management-tjänsten.
3. Klicka på **mått**.

![Mått bladet][metrics-blade]

Mer information om hur toouse statistik, se [översikt av mått].

## <a name="activity-logs"></a>Aktivitetsloggar
Aktivitetsloggar ger kunskaper om hello-åtgärder som utfördes på din API Management-tjänster. Det som tidigare kallades ”granskningsloggar” eller ”arbetsloggarna”. Använder aktivitetsloggar, kan du bestämma hello ”vad som, och när” för alla skrivåtgärder (PUT, POST, ta bort) vidtas för API Management-tjänster. 

> [!NOTE]
> Aktivitetsloggar inkluderar inte läsåtgärder (GET) eller åtgärder som utförs i hello klassiska Publisher-portalen eller med hjälp av hello ursprungliga Management API: er.

Du kan komma åt aktivitetsloggar i API Management-tjänsten eller komma åt loggarna för alla dina Azure-resurser i Azure-Monitor. tooview aktivitet loggar i API Management-tjänsten:
1. Öppna hello Azure-portalen.
2. Gå tooyour API Management-tjänsten.
3. Klicka på **aktivitetsloggen**.

![Aktiviteten loggar bladet][activity-logs-blade]

Mer information om hur toouse statistik, se [översikt aktivitetsloggar].

## <a name="alerts"></a>Aviseringar
Du kan konfigurera tooreceive varningar baserat på mått och aktivitet. Azure övervakaren kan en avisering toodo hello efter när den utlöser tooconfigure:

* Skicka ett e-postmeddelande
* anropa en webhook
* Anropa en Azure Logikapp

Du kan konfigurera Varningsregler i API Management-tjänsten, eller i Azure-Monitor. tooconfigure dem i API-hantering: 
1. Öppna hello Azure-portalen.
2. Gå tooyour API Management-tjänsten.
3. Klicka på **Varna regler**.

![Varningsregler bladet][alert-rules-blade]

Mer information om hur du använder aviseringar finns [översikt över aviseringar].

## <a name="diagnostic-logs"></a>Diagnostikloggar
Diagnostikloggar innehåller omfattande information om åtgärder och fel som är viktiga för granskning, samt i felsökningssyfte. Diagnostik loggar skiljer sig från aktivitetsloggar. Aktivitetsloggar ger insikter om hello-åtgärder som utfördes på Azure-resurser. Diagnostik-loggarna ger information om åtgärder som din resurs utförde sig själv.

API-hantering för närvarande tillhandahåller diagnostik loggar (batchar per timme) för enskilda API begäran med varje post med hello följande struktur:

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

Du kan komma åt diagnostikloggar i API Management-tjänsten eller komma åt loggarna för alla dina Azure-resurser i Azure-Monitor. tooview diagnostikloggar i API Management-tjänsten:
1. Öppna hello Azure-portalen.
2. Gå tooyour API Management-tjänsten.
3. Klicka på **diagnostiska loggen**.

![Bladet Diagnostikloggar][diagnostic-logs-blade]

Mer information om hur toouse statistik, se [översikt av diagnostikloggar].

## <a name="next-step"></a>Nästa steg

* [Kom igång med Azure Monitor]
* [Översikt över mått]
* [Översikt över aktivitetsloggar]
* [Översikt över diagnostikloggar]
* [Översikt över aviseringar]

[Kom igång med Azure Monitor]: ../monitoring-and-diagnostics/monitoring-get-started.md
[Översikt över mått]: ../monitoring-and-diagnostics/monitoring-overview-metrics.md
[Översikt över aktivitetsloggar]: ../monitoring-and-diagnostics/monitoring-overview-activity-logs.md
[Översikt över diagnostikloggar]: ../monitoring-and-diagnostics/monitoring-overview-of-diagnostic-logs.md
[Översikt över aviseringar]: ../monitoring-and-diagnostics/insights-alerts-portal.md



[metrics-blade]: ./media/api-management-azure-monitor/api-management-metrics-blade.png
[activity-logs-blade]: ./media/api-management-azure-monitor/api-management-activity-logs-blade.png
[alert-rules-blade]: ./media/api-management-azure-monitor/api-management-alert-rules-blade.png
[diagnostic-logs-blade]: ./media/api-management-azure-monitor/api-management-diagnostic-logs-blade.png
