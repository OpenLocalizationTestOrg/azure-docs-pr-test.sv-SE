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
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Så här: övervaka appar i Azure App Service
[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) har inbyggda funktioner som övervakning för den [Azure Portal](https://portal.azure.com).
Detta omfattar möjligheten att granska **kvoter** och **mått** för en app, samt App Service-plan, ställa in **aviseringar** och även **skalning**automatiskt baserat på de här måtten.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Förstå kvoter och mått
### <a name="quotas"></a>Kvoter
Program som finns i App Service regleras vissa *gränser* för de resurser som de kan använda. Gränserna som definieras av den **programtjänstplanen** associerat med appen.

Om programmet finns i en **lediga** eller **delade** planera sedan gränser för de resurser som kan använda appen definieras av **kvoter**.

Om programmet finns i en **grundläggande**, **Standard** eller **Premium** planera sedan gränser för de resurser som de kan använda ställs in med den **storlek**(Small, Medium, stora) och **instansen antal** (1, 2, 3,...) av den **programtjänstplanen**.

**Kvoter** för **lediga** eller **delade** appar är:

* **CPU(short)**
  * Mängden CPU som tillåts för det här programmet i en 5 minuter. Den här kvoten anger igen var femte minut.
* **CPU(Day)**
  * Totalt antal processorer som tillåts för det här programmet i en dag. Den här kvoten anger igen var 24: e timme vid midnatt UTC.
* **Minne**
  * Totala mängden minne som tillåts för det här programmet.
* **Bandbredd**
  * Totalt antal utgående bandbredd som tillåts för det här programmet i en dag.
    Den här kvoten anger igen var 24: e timme vid midnatt UTC.
* **Filsystem**
  * Totalt antal tillåtet lagringsutrymme.

Kvoten som endast till appar som finns på **grundläggande**, **Standard** och **Premium** planer är **Filesystem**.

Mer information om specifika kvoter, gränser och funktioner som är tillgängliga för olika App Service SKU hittar du här: [Tjänstbegränsningarna för Azure-prenumeration](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kvoter
Om ett program i användningen överskrider den **CPU (korta)**, **CPU (dag)**, eller **bandbredd** kvoten och programmet kommer att stoppas tills kvoten anger igen. Under denna tid kan alla förfrågningar som leder till en **HTTP 403**.
![][http403]

Om programmet **minne** kvot har överskridits och programmet blir inte startats.

Om den **Filesystem** kvot har överskridits och sedan någon skriva misslyckas åtgärden, inklusive skrivs till loggarna.

Kvoter kan ökas eller tas bort från din app genom att uppgradera din programtjänstplan.

### <a name="metrics"></a>Mått
**Mått** innehåller information om appen eller App Service-plan beteende.

För en **programmet**, tillgängliga mått är:

* **Genomsnittlig svarstid**
  * Genomsnittlig tid för appen att betjäna förfrågningar i ms.
* **Genomsnittlig minne arbetsminne**
  * Genomsnittlig mängd minne i MIB som används av appen.
* **CPU-tid**
  * Mängden CPU i sekunder som används av appen. Mer information om den här mått finns: [vs CPU CPU-tid i procent](#cpu-time-vs-cpu-percentage)
* **Data i**
  * Mängden inkommande bandbredd som används av appen i MIB.
* **Ut data**
  * Mängden utgående bandbredd som används av appen i MIB.
* **HTTP-2xx**
  * Antal begäranden som resulterar i en http-statuskod > = 200 men < 300.
* **HTTP-3xx**
  * Antal begäranden som resulterar i en http-statuskod > = 300 men < 400.
* **HTTP 401**
  * Antal begäranden som ledde till ett HTTP 401-statuskod.
* **HTTP 403**
  * Antal begäranden som ledde till ett 403 HTTP-statuskod.
* **HTTP 404**
  * Antal begäranden som ledde till ett 404 HTTP-statuskod.
* **HTTP 406**
  * Antal begäranden som resulterar i 406 HTTP-statuskod.
* **HTTP-4xx**
  * Antal begäranden som resulterar i en http-statuskod > = 400 men < 500.
* **HTTP-fel**
  * Antal begäranden som resulterar i en http-statuskod > = 500, men < 600.
* **Arbetsminnet för minne**
  * Aktuell mängd minne som används av appen i MIB.
* **Begäranden**
  * Totalt antal begäranden oavsett deras resulterande HTTP-statuskod.

För en **programtjänstplanen**, tillgängliga mått är:

> [!NOTE]
> App Service-plan mått är bara tillgängliga för planer i **grundläggande**, **Standard** och **Premium** SKU.
> 
> 

* **CPU-procent**
  * Genomsnittlig CPU som används i alla instanser av planen.
* **Minnesprocent**
  * Det genomsnittliga minne som används i alla instanser av planen.
* **Data i**
  * Genomsnittlig inkommande bandbredden som används i alla instanser av planen.
* **Ut data**
  * Den genomsnittliga utgående bandbredden som används i alla instanser av planen.
* **Diskkölängd**
  * Det genomsnittliga antalet både Skriv- och läsbegäranden som köade på lagring. En hög diskkölängd är en indikation på ett program som kan långsammare på grund av mycket i/o-disk.
* **HTTP-Kölängd**
  * Genomsnittligt antal HTTP-begäranden som hade till kön innan uppfylls. En hög eller ökande HTTP Kölängd är ett symtom på en plan vid hög belastning.

### <a name="cpu-time-vs-cpu-percentage"></a>Vs CPU CPU-tid i procent
<!-- To do: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Det finns 2 mått som avspeglar CPU-användning. **CPU-tid** och **CPU-procent**

**CPU-tid** är användbart för appar som finns i **lediga** eller **delade** planer eftersom en av sina kvoter definieras i CPU minuter som används av appen.

**CPU-procent** å andra sidan är användbart för appar som finns i **grundläggande**, **standard** och **premium** planer eftersom de kan skaländras ut och det här måttet är en bra indikation på den totala användningen i alla instanser.

## <a name="metrics-granularity-and-retention-policy"></a>Mått granularitet och bevarandeprincip
Mått för ett program och apptjänstplan loggas och sammanställs av tjänsten med följande granulariteter och bevarandeprinciper:

* **Minut** granularitet mått bevaras för **48 timmarna**
* **Timme** granularitet mått bevaras för **30 dagar**
* **Dag** granularitet mått bevaras för **90 dagar**

## <a name="monitoring-quotas-and-metrics-in-the-azure-portal"></a>Övervakning av kvoter och mått i Azure-portalen.
Du kan granska status för de olika **kvoter** och **mått** påverkar ett program i den [Azure Portal](https://portal.azure.com).

![][quotas]
**Kvoter** hittar du under Inställningar >**kvoter**. UX kan du granska: (1) kvoter namn, (2) intervallet för återställning, (3) den aktuella gränsen och (4) aktuella värde.

![][metrics]
**Mått** kan vara åtkomst direkt från resursbladet. Du kan också anpassa i schemat: (1) **klickar du på** på och välj (2) **redigera diagram**.
Härifrån kan du ändra (3) **tidsintervallet**, (4) **diagramtypen**, 5 **mått** ska visas.  

Du kan lära dig mer om här mått: [övervaka tjänsten](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Aviseringar och Autoskala
Mätvärden för en App eller App Service-plan kan vara kopplad till aviseringar, mer information om det finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Stöd för Apptjänst-appar finns i basic, standard eller premium Apptjänstplaner **Autoskala**. Detta kan du konfigurera regler som övervaka App Service-plan mått och kan öka eller minska instansantalet att ange ytterligare resurser som behövs eller sparar pengar när programmet är över etablera. Du kan lära dig mer om automatisk skalning här: [så skala](../monitoring-and-diagnostics/insights-how-to-scale.md) och här [bästa praxis för Azure-Monitor autoskalning](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Om du vill komma igång med Azure Apptjänst innan du registrerar dig för ett Azure-konto kan du gå till [Prova Apptjänst](https://azure.microsoft.com/try/app-service/). Där kan du direkt skapa en tillfällig startwebbapp i Apptjänst. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide till övergången från Webbplatser till App Service finns i: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
