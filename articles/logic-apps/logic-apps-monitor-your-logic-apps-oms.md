---
title: "aaaMonitor och få insikter om din logikapp köras med hjälp av OMS - Azure Logic Apps | Microsoft Docs"
description: "Övervaka logik appen körs med logganalys och Operations Management Suite (OMS) tooget insikter och bättre felsökning information för felsökning och diagnostik"
author: divyaswarnkar
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/9/2017
ms.author: LADocs; divswa
ms.openlocfilehash: a76fd6d1ff5c0010550be0f991514ce95f659fd6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-get-insights-about-logic-app-runs-with-operations-management-suite-oms-and-log-analytics"></a>Övervaka och få insikter om logik appen körs med Operations Management Suite (OMS) och logganalys

Övervakning och bättre felsökning information kan du aktivera logganalys på hello samtidigt när du skapar en logikapp. Log Analytics tillhandahåller diagnostik loggning och övervakning för din logikapp körs hello Operations Management Suite (OMS)-portalen. När du lägger till hello Logic Apps Management lösning tooOMS hämta aggregerad status för din logik app körs och en specifik information som status, körningstid, omsändning status och Korrelations-ID: N.

Det här avsnittet visar hur tooturn på logganalys eller installera hello Logic Apps hanteringslösning i OMS så att du kan visa runtime händelser och data för din logikapp kör.

 > [!TIP]
 > toomonitor dina befintliga logikappar, Följ dessa steg för [aktivera diagnostikloggning och skicka logik app runtime data tooOMS](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

## <a name="requirements"></a>Krav

Innan du börjar måste du toohave en OMS-arbetsyta. Läs [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md). 

## <a name="turn-on-diagnostics-logging-when-creating-logic-apps"></a>Aktivera diagnostikloggning när du skapar logikappar

1. I [Azure-portalen](https://portal.azure.com), skapa en logikapp. Välj **nya** > **företagsintegration** > **Logikapp** > **skapa**.

   ![Skapa en logikapp](media/logic-apps-monitor-your-logic-apps-oms/find-logic-apps-azure.png)

2. I hello **skapa logikapp** utför dessa uppgifter som visas:

   1. Ange ett namn för din logikapp och välj din Azure-prenumeration. 
   2. Skapa eller välj en Azure-resursgrupp.
   3. Ange **logganalys** för**på**. 
   Välj hello OMS-arbetsyta där du vill skicka data för din logikapp för körs. 
   4. När du är klar kan du välja **PIN-kod toodashboard** > **skapa**.

      ![Skapa logikapp](./media/logic-apps-monitor-your-logic-apps-oms/create-logic-app.png)

      När du har slutfört det här steget Azure skapar din logikapp nu som är associerade med din OMS-arbetsyta. 
      Det här steget installerar också automatiskt hello Logic Apps hanteringslösning i OMS-arbetsyta.

3. tooview logikappen körs i OMS, [fortsätta med de här stegen](#view-logic-app-runs-oms).

## <a name="install-hello-logic-apps-management-solution-in-oms"></a>Installera hello Logic Apps hanteringslösning i OMS

Om du redan aktiverat logganalys när du skapade din logikapp, hoppar du över det här steget. Du har redan hello Logic Apps hanteringslösning installerats i OMS.

1. I hello [Azure-portalen](https://portal.azure.com), Välj **fler tjänster**. Sök efter ”logganalys” som filter och välja **logganalys** som visas:

   ![Välj ”logganalys”](media/logic-apps-monitor-your-logic-apps-oms/find-log-analytics.png)

2. Under **logganalys**, söka efter och välj din OMS-arbetsyta. 

   ![Välj din OMS-arbetsyta](media/logic-apps-monitor-your-logic-apps-oms/select-logic-app.png)

3. Under **Management**, Välj **OMS-portalen**.

   ![Välj ”OMS-portalen”](media/logic-apps-monitor-your-logic-apps-oms/oms-portal-page.png)

4. På startsidan OMS om hello uppgradera banderoll visas, väljer du hello banderoll så att du först uppgradera din OMS-arbetsyta. Välj **lösningar galleriet**.

   ![Välj ”lösningar galleriet”](media/logic-apps-monitor-your-logic-apps-oms/solutions-gallery.png)

5. Under **alla lösningar för**, söka efter och välj hello panelen för hello **Logic Apps Management** lösning.

   ![Välj ”Logic Apps Management”](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-management-tile2.png)

6. tooinstall hello lösning i OMS-arbetsyta, Välj **Lägg till**.

   ![Välj ”Lägg till” för ”Logic Apps Management”](media/logic-apps-monitor-your-logic-apps-oms/add-logic-apps-management-solution.png)

<a name="view-logic-app-runs-oms"></a>

## <a name="view-your-logic-app-runs-in-your-oms-workspace"></a>Visa din logikapp som körs i din OMS-arbetsyta

1. tooview hello antal och status för din logikapp körs gå toohello översiktssidan för din OMS-arbetsyta. Granska hello information om hello **Logic Apps Management** panelen.

   ![Översikt över panelen visar logik app kör antal och status](media/logic-apps-monitor-your-logic-apps-oms/overview.png)

   > [!Note]
   > Om den här uppgraderingen banderoll visas i stället för hello Logic Apps Management panelen, väljer du hello banderoll så att du först uppgradera din OMS-arbetsyta.
  
   > ![Uppgradera ”OMS-arbetsytan”](media/logic-apps-monitor-your-logic-apps-oms/oms-upgrade-banner.png)

2. tooview en sammanfattning med mer information om logik appen körs, Välj hello **Logic Apps Management** panelen.

   Här kan grupperas logik appen körs efter namn eller Körstatus.

   ![Översikt över statusen för din logikapp körs](media/logic-apps-monitor-your-logic-apps-oms/logic-apps-runs-summary.png)
   
3. tooview alla hello körs för en specifik logikapp eller status, Välj hello rad för en logikapp eller status.

   Här är ett exempel som visar alla hello körs för en specifik logikapp:

   ![Vyn körs för en logikapp eller status](media/logic-apps-monitor-your-logic-apps-oms/logic-app-run-details.png)

   > [!NOTE]
   > Hej **omsändning** kolumnen visar ”Ja” för Kör som uppstår vid körningen av en skickad igen.

4. toofilter dessa resultat, du kan utföra både på klientsidan och serversidan filtrering.

   * Klientsidans filter: Välj hello filter som du vill använda för varje kolumn. 
   Här följer några exempel:

     ![Exempel kolumnfiltren](media/logic-apps-monitor-your-logic-apps-oms/filters.png)

   * Serversidan filter: toochoose en viss tid fönster eller toolimit hello antal som visas, Använd hello scope kontroll hello överst på hello sidan. 
   Som standard visas endast 1 000 poster i taget. 
   
     ![Ändra hello tidsfönstret](media/logic-apps-monitor-your-logic-apps-oms/change-interval.png)
 
5. alla tooview hello åtgärder och deras information för en specifik kör, väljer en rad som öppnas hello loggen söksidan. 

   * tooview informationen i en tabell, Välj **tabellen**.
   * Du kan redigera hello frågesträngen hello sökfältet toochange hello frågan. 
   En bättre upplevelse, Välj **Advanced Analytics**.

     ![Visa information för en logikapp som körs och åtgärder](media/logic-apps-monitor-your-logic-apps-oms/log-search-page.png)

     Här hello Azure logganalys på sidan kan du uppdatera frågor och visa hello fås hello tabell. 
     Den här frågan använder [Kusto frågespråk](https://docs.loganalytics.io/learn/tutorials/getting_started_with_queries.html), som du kan redigera om du vill tooview olika resultat. 

     ![Azure logganalys - frågevy](media/logic-apps-monitor-your-logic-apps-oms/query.png)

## <a name="next-steps"></a>Nästa steg

* [Övervaka B2B-meddelanden](../logic-apps/logic-apps-monitor-b2b-message.md)
