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
# <a name="how-to-monitor-apps-in-azure-app-service"></a>Så här: övervaka appar i Azure App Service
[Apptjänst](http://go.microsoft.com/fwlink/?LinkId=529714) har inbyggda funktioner som övervakning för hello [Azure Portal](https://portal.azure.com).
Detta inkluderar hello möjlighet tooreview **kvoter** och **mått** för en app, samt hello App Service-plan, ställa in **aviseringar** och även **skalning** automatiskt baserat på de här måtten.

[!INCLUDE [app-service-web-to-api-and-mobile](../../includes/app-service-web-to-api-and-mobile.md)]

## <a name="understanding-quotas-and-metrics"></a>Förstå kvoter och mått
### <a name="quotas"></a>Kvoter
Program som finns i App Service är ämne toocertain *gränser* för de resurser som de kan använda. hello gränser har definierats av hello **programtjänstplanen** som är associerade med hello app.

Om programmet hello finns i en **lediga** eller **delade** planera sedan hello gränserna för hello resurser hello app kan använda definieras av **kvoter**.

Om programmet hello finns i en **grundläggande**, **Standard** eller **Premium** planera sedan hello gränserna för hello resurser som de kan använda anges av hello **storlek** (Small, Medium, stora) och **instansen antal** (1, 2, 3,...) av hello **programtjänstplanen**.

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

Hej endast kvoten tillämpliga tooapps finns på **grundläggande**, **Standard** och **Premium** planer är **Filesystem**.

Mer information om hello specifika kvoter, gränser och funktioner som är tillgängliga för hello olika App Service SKU: er hittar du här: [Tjänstbegränsningarna för Azure-prenumeration](../azure-subscription-service-limits.md#app-service-limits)

#### <a name="quota-enforcement"></a>Kvoter
Om ett program i användningen överskrider hello **CPU (korta)**, **CPU (dag)**, eller **bandbredd** kvot sedan hello programmet kommer att stoppas tills hello kvoten anger igen. Under denna tid kan alla förfrågningar som leder till en **HTTP 403**.
![][http403]

Om programmet hello **minne** kvot har överskridits och sedan hello programmet blir startats.

Om hello **Filesystem** kvot har överskridits och sedan någon skriva misslyckas åtgärden, inklusive skriva toologs.

Kvoter kan ökas eller tas bort från din app genom att uppgradera din programtjänstplan.

### <a name="metrics"></a>Mått
**Mått** innehåller information om hello appen eller en App Service-plan beteende.

För en **programmet**, hello tillgängliga mått är:

* **Genomsnittlig svarstid**
  * hello Genomsnittlig tid för hello app tooserve begäranden i ms.
* **Genomsnittlig minne arbetsminne**
  * hello genomsnittlig mängd minne i MIB som används av hello app.
* **CPU-tid**
  * hello mycket Processorkraft i sekunder som används av hello app. Mer information om den här mått finns: [vs CPU CPU-tid i procent](#cpu-time-vs-cpu-percentage)
* **Data i**
  * hello mängden inkommande bandbredd som används av hello app i MIB.
* **Ut data**
  * hello mängden utgående bandbredd som används av hello app i MIB.
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
  * Aktuell mängd minne som används av hello app i MIB.
* **Begäranden**
  * Totalt antal begäranden oavsett deras resulterande HTTP-statuskod.

För en **programtjänstplanen**, hello tillgängliga mått är:

> [!NOTE]
> App Service-plan mått är bara tillgängliga för planer i **grundläggande**, **Standard** och **Premium** SKU.
> 
> 

* **CPU-procent**
  * hello Genomsnittlig CPU-användning i alla instanser av hello plan.
* **Minnesprocent**
  * hello genomsnittlig minne som används i alla instanser av hello plan.
* **Data i**
  * hello genomsnittlig inkommande bandbredden som används i alla instanser av hello plan.
* **Ut data**
  * hello medelvärdet utgående bandbredden som används i alla instanser av hello plan.
* **Diskkölängd**
  * hello Genomsnittligt antal både Skriv- och läsbegäranden som köade på lagring. En hög diskkölängd är en indikation på ett program som kan långsammare på grund av i/o för tooexcessive disk.
* **HTTP-Kölängd**
  * hello Genomsnittligt antal HTTP-begäranden som hade toosit hello kön innan uppfylls. En hög eller ökande HTTP Kölängd är ett symtom på en plan vid hög belastning.

### <a name="cpu-time-vs-cpu-percentage"></a>Vs CPU CPU-tid i procent
<!-- toodo: Fix Anchor (#CPU-time-vs.-CPU-percentage) -->

Det finns 2 mått som avspeglar CPU-användning. **CPU-tid** och **CPU-procent**

**CPU-tid** är användbart för appar som finns i **lediga** eller **delade** planer eftersom en av sina kvoter definieras i CPU minuter som används av hello app.

**CPU-procent** på hello andra sidan är användbart för appar som finns i **grundläggande**, **standard** och **premium** planer eftersom de kan skaländras ut och mätvärdet är en bra indikation på hello totala användningen i alla instanser.

## <a name="metrics-granularity-and-retention-policy"></a>Mått granularitet och bevarandeprincip
Mått för ett program och app service-plan loggas och sammanställs efter hello tjänsten med hello efter granulariteter och bevarandeprinciper:

* **Minut** granularitet mått bevaras för **48 timmarna**
* **Timme** granularitet mått bevaras för **30 dagar**
* **Dag** granularitet mått bevaras för **90 dagar**

## <a name="monitoring-quotas-and-metrics-in-hello-azure-portal"></a>Övervakning av kvoter och mått i hello Azure-portalen.
Du kan granska hello status för olika hello **kvoter** och **mått** påverkar ett program i hello [Azure Portal](https://portal.azure.com).

![][quotas]
**Kvoter** hittar du under Inställningar >**kvoter**. hello UX kan du granska: (1) hello kvoter namn, (2) intervallet för återställning, (3) den aktuella gränsen och (4) aktuella värde.

![][metrics]
**Mått** kan vara åtkomst direkt från hello-resursbladet. Du kan också anpassa hello diagram av: (1) **klickar du på** på och välj (2) **redigera diagram**.
Härifrån kan du ändra hello (3) **tidsintervallet**, (4) **diagramtypen**, 5 **mått** toodisplay.  

Du kan lära dig mer om här mått: [övervaka tjänsten](../monitoring-and-diagnostics/insights-how-to-customize-monitoring.md).

## <a name="alerts-and-autoscale"></a>Aviseringar och Autoskala
Mätvärden för en App eller App Service-plan kan vara ansluten tooalerts toolearn mer om det finns [få aviseringar](../monitoring-and-diagnostics/insights-receive-alert-notifications.md)

Stöd för Apptjänst-appar finns i basic, standard eller premium Apptjänstplaner **Autoskala**. Detta ger dig tooconfigure regler som övervaka App Service-plan mått och kan öka eller minska hello instansantal för att ange ytterligare resurser som behövs eller sparar pengar när programmet hello är över etablera. Du kan lära dig mer om automatisk skalning här: [hur tooScale](../monitoring-and-diagnostics/insights-how-to-scale.md) och här [bästa praxis för Azure-Monitor autoskalning](../monitoring-and-diagnostics/insights-autoscale-best-practices.md)

> [!NOTE]
> Om du vill tooget igång med Azure App Service innan du registrerar dig för ett Azure-konto går för[prova App Service](https://azure.microsoft.com/try/app-service/), där kan du direkt skapa en tillfällig startwebbapp i App Service. Inget kreditkort krävs, och du gör inga åtaganden.
> 
> 

## <a name="whats-changed"></a>Nyheter
* En guide toohello övergången från webbplatser tooApp tjänsten finns: [Azure App Service och dess påverkan på befintliga Azure-tjänster](http://go.microsoft.com/fwlink/?LinkId=529714)

[fzilla]:http://go.microsoft.com/fwlink/?LinkId=247914
[vmsizes]:http://go.microsoft.com/fwlink/?LinkID=309169



<!-- Images. -->
[http403]: ./media/web-sites-monitor/http403.png
[quotas]: ./media/web-sites-monitor/quotas.png
[metrics]: ./media/web-sites-monitor/metrics.png
