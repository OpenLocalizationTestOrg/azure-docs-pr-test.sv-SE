---
title: "aaaCheck status, ställa in loggning och få varningar – Azure Logic Apps | Microsoft Docs"
description: "Övervaka statusen och prestanda för logic apps loggar diagnostikdata och ställa in aviseringar"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: 5c1b1e15-3b6c-49dc-98a6-bdbe7cb75339
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 07/21/2017
ms.author: LADocs; jehollan
ms.openlocfilehash: 81f186e11a669b710f4c06089597eb5a76f7a44e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-status-set-up-diagnostics-logging-and-turn-on-alerts-for-azure-logic-apps"></a>Övervaka status, konfigurera diagnostikloggning och aktivera aviseringar för Azure Logic Apps

När du [skapa och köra en logikapp](../logic-apps/logic-apps-create-a-logic-app.md), du kan kontrollera dess körs historik, utlösare historik, status och prestanda. För händelseövervakning och bättre felsökning, ställa in [diagnostikloggning](#azure-diagnostics) för din logikapp. På så sätt kan du kan [söka efter och visa händelser](#find-events), t.ex. utlösaren händelser, kör händelser och händelser för åtgärden. Du kan också använda detta [diagnostikdata med andra tjänster](#extend-diagnostic-data), t.ex. Azure Storage och Händelsehubbar i Azure. 

tooget meddelanden om fel eller andra möjliga problem, Ställ in [aviseringar](#add-azure-alerts). Du kan till exempel skapa en avisering som identifierar ”när fler än fem körs inte på en timme”. Du kan även konfigurera övervakning, spårning och loggning programmässigt med hjälp av [egenskaper och inställningar för Azure-diagnostik händelse](#diagnostic-event-properties).

## <a name="view-runs-and-trigger-history-for-your-logic-app"></a>Visa körs och utlösa historik för din logikapp

1. toofind logikappen i hello [Azure-portalen](https://portal.azure.com), på hello Azure huvudmenyn, Välj **fler tjänster**. Sök efter ”logikappar” i hello sökrutan och väljer **logikappar**.

   ![Hitta din logikapp](./media/logic-apps-monitor-your-logic-apps/find-your-logic-app.png)

   hello Azure-portalen visar hello logikappar som är associerade med din Azure-prenumeration. 

2. Välj din logikapp och sedan **översikt**.

   hello Azure-portalen visar hello körs historik och utlösare historik för din logikapp. Exempel:

   ![Logikapp körs historik och utlösare historik](media/logic-apps-monitor-your-logic-apps/overview.png)

   * **Kör historik** visar alla hello körs för din logikapp. 
   * **Utlösa historik** visar alla hello Utlösaraktivitet för din logikapp.

   Status finns i [felsöka din logikapp](../logic-apps/logic-apps-diagnosing-failures.md).

   > [!TIP]
   > Om du inte hittar hello data som du förväntar dig hello i verktygsfältet väljer du **uppdatera**.

3. tooview hello steg från en specifik kördes under **körs historik**, Välj som körs. 

   hello visas Övervakare varje steg i som körs. Exempel:

   ![Åtgärder för en specifik kör](media/logic-apps-monitor-your-logic-apps/monitor-view-updated.png)

4. tooget mer information om hello kör, Välj **kör information**. Den här informationen sammanfattar hello steg, status, indata och utdata för hello kör. 

   ![Välj ”Kör information”](media/logic-apps-monitor-your-logic-apps/run-details.png)

   Du kan till exempel få hello körs **Korrelations-ID**, som du kan behöva när du använder hello [REST API för Logic Apps](https://docs.microsoft.com/rest/api/logic).

5. tooget information om ett specifikt steg, väljer du steget. Nu kan du granska information som indata, utdata och eventuella fel som inträffade för steget. Exempel:

   ![Steg-information](media/logic-apps-monitor-your-logic-apps/monitor-view-details.png)
   
   > [!NOTE]
   > Alla runtime information och händelser krypteras inom hello Logic Apps-tjänsten. De dekrypteras endast när en användare begär tooview data. Du kan också styra åtkomst toothese händelser med [rollbaserad åtkomstkontroll (RBAC)](../active-directory/role-based-access-control-what-is.md).

6. tooget information om en specifik utlösare, gå tillbaka toohello **översikt** fönstret. Under **utlösa historik**väljer hello-utlösare. Nu kan du granska information som indata och utdata, till exempel:

   ![Utlösaren händelseinformationen för utdata](media/logic-apps-monitor-your-logic-apps/trigger-details.png)

<a name="azure-diagnostics"></a>

## <a name="turn-on-diagnostics-logging-for-your-logic-app"></a>Aktivera loggning för din logikapp diagnostik

För bättre felsökning med runtime information och händelser som du kan ställa in diagnostik loggning med [Azure logganalys](../log-analytics/log-analytics-overview.md). Log Analytics är en tjänst i [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md) som övervakar molnet och lokala miljöer toohelp du upprätthålla sin tillgänglighet och prestanda. 

Innan du börjar måste du toohave en OMS-arbetsyta. Läs [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md).

1. I hello [Azure-portalen](https://portal.azure.com), söka efter och välj din logikapp. 

2. Hello logik app bladet menyn under **övervakning**, Välj **diagnostik** > **diagnostikinställningar**.

   ![Gå tooMonitoring, diagnostik, diagnostikinställningar](media/logic-apps-monitor-your-logic-apps/logic-app-diagnostics.png)

3. Under **diagnostikinställningarna**, Välj **på**.

   ![Aktivera diagnostikloggar](media/logic-apps-monitor-your-logic-apps/turn-on-diagnostics-logic-app.png)

4. Nu välja hello OMS arbetsytan och händelse kategori för loggning som visas:

   1. Välj **skicka tooLog Analytics**. 
   2. Under **logganalys**, Välj **konfigurera**. 
   3. Under **OMS arbetsytor**, Välj hello OMS arbetsytan toouse för loggning.
   4. Under **loggen**väljer hello **WorkflowRuntime** kategori.
   5. Välj hello mått intervall.
   6. När du är klar väljer **spara**.

   ![Välj OMS-arbetsytan och data för loggning](media/logic-apps-monitor-your-logic-apps/send-diagnostics-data-log-analytics-workspace.png)

Nu hittar du händelser och andra data för utlösaren händelser, kör händelser och händelser för åtgärden.

<a name="find-events"></a>

## <a name="find-events-and-data-for-your-logic-app"></a>Sök efter händelser och data för din logikapp

toofind och visa händelser i din logikapp som utlösare händelser, kör händelser, och åtgärden händelser, Följ dessa steg.

1. I hello [Azure-portalen](https://portal.azure.com), Välj **fler tjänster**. Sök efter ”logganalys” och välj sedan **logganalys** som visas här:

   ![Välj ”logganalys”](media/logic-apps-monitor-your-logic-apps/browseloganalytics.png)

2. Under **logganalys**, söka efter och välj din OMS-arbetsyta. 

   ![Välj din OMS-arbetsyta](media/logic-apps-monitor-your-logic-apps/selectla.png)

3. Under **Management**, Välj **OMS-portalen**.

   ![Välj ”OMS-portalen”](media/logic-apps-monitor-your-logic-apps/omsportalpage.png)

4. Välj på startsidan OMS **loggen Sök**.

   ![På startsidan OMS väljer du ”loggen Sök”](media/logic-apps-monitor-your-logic-apps/logsearch.png)

   ELLER

   ![Välj ”Sök loggen” på hello OMS-menyn](media/logic-apps-monitor-your-logic-apps/logsearch-2.png)

5. Ange ett fält som du vill toofind, i hello sökrutan och tryck på **RETUR**. När du börjar skriva in visas OMS möjliga matchningar och åtgärder som du kan använda. 

   Till exempel toofind hello topp 10 händelser som inträffade, ange och välj sökfrågan: **kategori = WorkflowRuntime | topp 10**

   ![Ange söksträngen](media/logic-apps-monitor-your-logic-apps/oms-start-query.png)

   Lär dig mer om [hur toofind data i logganalys](../log-analytics/log-analytics-log-searches.md).

6. Välj hello tidsram på hello resultatsidan i vänstra hello-fältet som du vill tooview.
toorefine frågan genom att lägga till ett filter, Välj **+ Lägg till**.

   ![Välj tidsram för frågeresultat](media/logic-apps-monitor-your-logic-apps/query-results.png)

7. Under **Lägg till filter**, ange hello Filternamn så att du kan hitta hello filter som du vill använda. Välj hello filter och välj **+ Lägg till**.

   Det här exemplet använder hello ordet ”status” toofind misslyckades händelser under **AzureDiagnostics**.
   Här hello filter för **status_s** har redan valts.

   ![Välj filter](media/logic-apps-monitor-your-logic-apps/log-search-add-filter.png)

8. I vänster hello-fältet väljer hello filtervärdet som du vill toouse och välj **tillämpa**.

   ![Välj filtervärdet, välj ”Använd”](media/logic-apps-monitor-your-logic-apps/log-search-apply-filter.png)

9. Returnerar nu toohello fråga som du utvecklar. Frågan har uppdaterats med ditt valda filter och värdet. Tidigare resultaten filtreras nu för.

   ![Returnera tooyour fråga med filtrerade resultat](media/logic-apps-monitor-your-logic-apps/log-search-query-filtered-results.png)

10. toosave frågan för framtida användning, Välj **spara**. Läs [hur toosave frågan](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md#save-oms-query).

<a name="extend-diagnostic-data"></a>

## <a name="extend-how-and-where-you-use-diagnostic-data-with-other-services"></a>Utöka hur och när du använder diagnostikdata med andra tjänster

Du kan utöka hur du använder din logikapp diagnostikdata med andra Azure-tjänster, till exempel tillsammans med Azure Log Analytics: 

* [Arkivera Azure Diagnostics loggar i Azure-lagring](../monitoring-and-diagnostics/monitoring-archive-diagnostic-logs.md)
* [Dataströmmen Azure Diagnostics loggar tooAzure Händelsehubbar](../monitoring-and-diagnostics/monitoring-stream-diagnostic-logs-to-event-hubs.md) 

Du kan sedan get realtid övervakning med hjälp av telemetri och analyser från andra tjänster som [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) och [Power BI](../log-analytics/log-analytics-powerbi.md). Exempel:

* [Dataströmmen data från Händelsehubbar tooStream Analytics](../stream-analytics/stream-analytics-define-inputs.md)
* [Analysera strömmande data med Stream Analytics och skapa en instrumentpanel för analys i realtid i Power BI](../stream-analytics/stream-analytics-power-bi-dashboard.md)

Baserat på hello-alternativ som du vill konfigurera, ska du se till att du första [skapa ett Azure storage-konto](../storage/common/storage-create-storage-account.md) eller [skapar en Azure händelsehubb](../event-hubs/event-hubs-create.md). Välj sedan hello alternativ för där du vill toosend diagnostikdata:

![Skicka data tooAzure lagring konto eller event hub](./media/logic-apps-monitor-your-logic-apps/storage-account-event-hubs.png)

> [!NOTE]
> Kvarhållningsperioder gäller bara när du väljer toouse ett lagringskonto.

<a name="add-azure-alerts"></a>

## <a name="set-up-alerts-for-your-logic-app"></a>Konfigurera aviseringar för din logikapp

ställa in toomonitor specifika mått eller överskred tröskelvärden för din logikapp [aviseringar i Azure](../monitoring-and-diagnostics/monitoring-overview-alerts.md). Lär dig mer om [mått i Azure](../monitoring-and-diagnostics/monitoring-overview-metrics.md). 

tooset in aviseringar utan [Azure logganalys](../log-analytics/log-analytics-overview.md), Följ dessa steg. För mer avancerade kriterier som aviseringar och åtgärder, [konfigurera logganalys](#azure-diagnostics) för.

1. Hello logik app bladet menyn under **övervakning**, Välj **diagnostik** > **Varna regler** > **Lägg till avisering**som visas här:

   ![Lägga till en avisering för din logikapp](media/logic-apps-monitor-your-logic-apps/set-up-alerts.png)

2. På hello **lägga till en varningsregel** bladet skapa aviseringen som visas:

   1. Under **resurs**, Välj din logikapp, om det inte redan är markerad. 
   2. Ange ett namn och beskrivning för aviseringen.
   3. Välj en **mått** eller den händelse som du vill tootrack.
   4. Välj en **villkoret**, ange en **tröskelvärdet** för hello mått och välj hello **Period** för övervakning av det här måttet.
   5. Välj om e-toosend för hello aviseringen. 
   6. Ange andra e-postadresser för att skicka hello avisering. 
   Du kan också ange en Webhooksadressen där du vill toosend hello avisering.

   Till exempel regeln skickar en avisering när fem eller fler körningar misslyckas på en timme:

   ![Skapa mått varningsregel](media/logic-apps-monitor-your-logic-apps/create-alert-rule.png)

> [!TIP]
> toorun en logikapp från en varning och du kan inkludera hello [begäran utlösaren](../connectors/connectors-native-reqres.md) i arbetsflödet, där du kan utföra åtgärder som de här exemplen:
> 
> * [Bokför tooSlack](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-slack-with-logic-app)
> * [Skicka en text](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-text-message-with-logic-app)
> * [Lägg till en meddelandekö tooa](https://github.com/Azure/azure-quickstart-templates/tree/master/201-alert-to-queue-with-logic-app)

<a name="diagnostic-event-properties"></a>

## <a name="azure-diagnostics-event-settings-and-details"></a>Azure Diagnostics händelse inställningar och information

Varje diagnostiska händelsen innehåller information om din logikapp och att händelsen, till exempel hello status, starttid, Sluttid och så vidare. tooprogrammatically Konfigurera övervakning, spårning och loggning, organisationsenheten kan använda dessa uppgifter med hello [REST API: et för Azure Logikappar](https://docs.microsoft.com/rest/api/logic) och hello [REST API för Azure-diagnostik](../monitoring-and-diagnostics/monitoring-supported-metrics.md#microsoftlogicworkflows).

Till exempel hello `ActionCompleted` händelsen har hello `clientTrackingId` och `trackedProperties` egenskaper som du kan använda för att spåra och övervaka:

``` json
{
    "time": "2016-07-09T17:09:54.4773148Z",
    "workflowId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP",
    "resourceId": "/SUBSCRIPTIONS/<subscription-ID>/RESOURCEGROUPS/MYRESOURCEGROUP/PROVIDERS/MICROSOFT.LOGIC/WORKFLOWS/MYLOGICAPP/RUNS/08587361146922712057/ACTIONS/HTTP",
    "category": "WorkflowRuntime",
    "level": "Information",
    "operationName": "Microsoft.Logic/workflows/workflowActionCompleted",
    "properties": {
        "$schema": "2016-06-01",
        "startTime": "2016-07-09T17:09:53.4336305Z",
        "endTime": "2016-07-09T17:09:53.5430281Z",
        "status": "Succeeded",
        "code": "OK",
        "resource": {
            "subscriptionId": "<subscription-ID>",
            "resourceGroupName": "MyResourceGroup",
            "workflowId": "cff00d5458f944d5a766f2f9ad142553",
            "workflowName": "MyLogicApp",
            "runId": "08587361146922712057",
            "location": "westus",
            "actionName": "Http"
        },
        "correlation": {
            "actionTrackingId": "e1931543-906d-4d1d-baed-dee72ddf1047",
            "clientTrackingId": "<my-custom-tracking-ID>"
        },
        "trackedProperties": {
            "myTrackedProperty": "<value>"
        }
    }
}
```

* `clientTrackingId`: Om det inte angetts i Azure genererar detta ID och korrelerar händelser över en logikapp som körs automatiskt, inklusive alla kapslade arbetsflöden som anropas från hello logikapp. Du kan ange detta ID från en utlösare manuellt genom att skicka en `x-ms-client-tracking-id` huvud med din anpassade ID-värde i hello utlösarbegäran. Du kan använda en utlösare för begäran, http-utlösaren eller webhook-utlösare.

* `trackedProperties`: tootrack indata eller utdata i diagnostikdata, du kan lägga till egenskaper för spårade tooactions i din logikapp JSON-definitionen. Spårade egenskaper kan spåra bara en enda åtgärd indata och utdata, men du kan använda hello `correlation` egenskaper för händelser toocorrelate över åtgärder körs.

  tootrack en eller flera egenskaper, lägga till hello `trackedProperties` avsnittet och hello egenskaper för definition av toohello åtgärd. Anta att du vill tootrack data som en ”order-ID” i din telemetri:

  ``` json
  "myAction": {
    "type": "http",
    "inputs": {
        "uri": "http://uri",
        "headers": {
            "Content-Type": "application/json"
        },
        "body": "@triggerBody()"
    },
    "trackedProperties": {
        "myActionHTTPStatusCode": "@action()['outputs']['statusCode']",
        "myActionHTTPValue": "@action()['outputs']['body']['<content>']",
        "transactionId": "@action()['inputs']['body']['<content>']"
    }
  }
  ```

## <a name="next-steps"></a>Nästa steg

* [Skapa mallar för logik app-distribution och versionshantering](../logic-apps/logic-apps-create-deploy-template.md)
* [B2B-scenarier med Enterprise-Integrationspaket](../logic-apps/logic-apps-enterprise-integration-overview.md)
* [Övervaka B2B-meddelanden](../logic-apps/logic-apps-monitor-b2b-message.md)