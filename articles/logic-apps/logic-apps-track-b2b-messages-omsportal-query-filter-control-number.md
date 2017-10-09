---
title: "aaaQuery för B2B-meddelanden i Operations Management Suite - Azure Logic Apps | Microsoft Docs"
description: "Skapa frågor tootrack AS2-, X 12- och EDIFACT-meddelanden i hello Operations Management Suite"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: LADocs; padmavc
ms.openlocfilehash: aee6644ff19add8f074ed5f1725db87b1d3b74b3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="query-for-as2-x12-and-edifact-messages-in-hello-microsoft-operations-management-suite-oms"></a>Fråga efter AS2-, X 12- och EDIFACT-meddelanden i hello Microsoft Operations Management Suite (OMS)

toofind hello AS2-, X 12 eller EDIFACT-meddelanden som du spårar med [Azure logganalys](../log-analytics/log-analytics-overview.md) i hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md), du kan skapa frågor som filtrerar åtgärder baserat på specifika kriterier. Du kan till exempel hitta meddelanden baserat på specifika interchange kontroll.

## <a name="requirements"></a>Krav

* En logikapp som har konfigurerats med diagnostikloggning. Läs [hur toocreate en logikapp](../logic-apps/logic-apps-create-a-logic-app.md) och [hur tooset in loggning för att logikapp](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Ett integration-konto som har konfigurerats med övervakning och loggning. Läs [hur toocreate kontonamn integration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) och [hur tooset in övervakning och loggning för det kontot](../logic-apps/logic-apps-monitor-b2b-message.md).

* Om du inte redan har gjort [publicera diagnostikdata tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) och [konfigurera meddelandespårning i OMS](../logic-apps/logic-apps-track-b2b-messages-omsportal.md).

> [!NOTE]
> När du har uppfyllt kraven för hello tidigare, bör du ha en arbetsyta i hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Du bör använda hello samma OMS-arbetsyta för att spåra dina B2B-kommunikation i OMS. 
>  
> Lär dig mer om du inte har en OMS-arbetsyta [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md).

## <a name="create-message-queries-with-filters-in-hello-operations-management-suite-portal"></a>Skapa frågor med filter i hello Operations Management Suite-portalen

Det här exemplet visar hur du kan hitta meddelanden baserat på deras interchange kontrollnummer.

> [!TIP] 
> Om du vet namnet på din OMS-arbetsytan gå tooyour arbetsytan startsidan (`https://{your-workspace-name}.portal.mms.microsoft.com`), och starta i steg 4. Annars starta i steg 1.

1. I hello [Azure-portalen](https://portal.azure.com), Välj **fler tjänster**. Sök efter ”logganalys” och välj sedan **logganalys** som visas här:

   ![Hitta logganalys](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/browseloganalytics.png)

2. Under **logganalys**, söka efter och välj din OMS-arbetsyta.

   ![Välj din OMS-arbetsyta](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/selectla.png)

3. Under **Management**, Välj **OMS-portalen**.

   ![Välj OMS-portalen](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/omsportalpage.png)

4. Välj på startsidan OMS **loggen Sök**.

   ![På startsidan OMS väljer du ”loggen Sök”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   ELLER

   ![Välj ”Sök loggen” på hello OMS-menyn](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

5. Ange ett fält som du vill toofind, i hello sökrutan och tryck på **RETUR**. När du börjar skriva in visas OMS möjliga matchningar och åtgärder som du kan använda. Lär dig mer om [hur toofind data i logganalys](../log-analytics/log-analytics-log-searches.md).

   Det här exemplet söker efter händelser med **typ = AzureDiagnostics**.

   ![Börja skriva frågesträng](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-start-query.png)

6. I vänster hello-fältet väljer hello tidsram som du vill tooview. tooadd tooyour filterfråga, Välj **+ Lägg till**.

   ![Lägg till filter tooquery](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/query1.png)

7. Under **Lägg till filter**, ange hello Filternamn så att du kan hitta hello filter som du vill använda. Välj hello filter och välj **+ Lägg till**.

   toofind hello interchange kontrollnummer i det här exemplet söker efter hello ordet ”interchange” och väljer **event_record_messageProperties_interchangeControlNumber_s** som hello filter.

   ![Välj filter](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-add-filter.png)

9. I vänster hello-fältet väljer hello filtervärdet som du vill toouse och välj **tillämpa**.

   Det här exemplet väljer hello interchange kontrollnummer för hälsningsmeddelande som vi vill.

   ![Välj filtervärdet](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-select-filter-value.png)

10. Returnerar nu toohello fråga som du utvecklar. Frågan har uppdaterats med ditt valda filter-händelsen och värdet. Tidigare resultaten filtreras nu för.

    ![Returnera tooyour fråga med filtrerade resultat](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-filtered-results.png)

<a name="save-oms-query"></a>

## <a name="save-your-query-for-future-use"></a>Spara frågan för framtida användning

1. Från din fråga på hello **loggen Sök** väljer **spara**. Namnge din fråga, Välj en kategori och välj **spara**.

   ![Ge din fråga namn och kategori](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-save.png)

2. tooview frågan, Välj **Favoriter**.

   ![Välj ”Favoriter”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-query-favorites.png)

3. Under **sparade sökningar**, Markera frågan så att du kan visa hello resultat. tooupdate hello frågan så att du kan hitta olika resultat redigera hello fråga.

   ![Välj din fråga](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="find-and-run-saved-queries-in-hello-operations-management-suite-portal"></a>Hitta och köra sparade frågor i hello Operations Management Suite-portalen

1. Öppna startsidan för OMS-arbetsytan (`https://{your-workspace-name}.portal.mms.microsoft.com`), och välj **loggen Sök**.

   ![På startsidan OMS väljer du ”loggen Sök”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch.png)

   ELLER

   ![Välj ”Sök loggen” på hello OMS-menyn](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/logsearch-2.png)

2. På hello **loggen Sök** startsidan, Välj **Favoriter**.

   ![Välj ”Favoriter”](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-favorites.png)

3. Under **sparade sökningar**, Markera frågan så att du kan visa hello resultat. tooupdate hello frågan så att du kan hitta olika resultat redigera hello fråga.

   ![Välj din fråga](media/logic-apps-track-b2b-messages-omsportal-query-filter-control-number/oms-log-search-find-favorites.png)

## <a name="next-steps"></a>Nästa steg

* [AS2-spårningsscheman](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-spårningsscheman](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Spårning av anpassade scheman](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)