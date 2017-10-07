---
title: aaaTrack B2B-meddelanden i Operations Management Suite - Azure Logic Apps | Microsoft Docs
description: "Spåra B2B-kommunikation för ditt konto och logik integrationsappar i Operations Management Suite (OMS) med Azure logganalys"
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
ms.openlocfilehash: f385a72008b19408bb45d61c440df0505b688175
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="track-b2b-communication-in-hello-microsoft-operations-management-suite-oms"></a>Spåra B2B-kommunikation i hello Microsoft Operations Management Suite (OMS)

När du har skapat B2B-kommunikation mellan två kör affärsprocesser eller program via kontot integration dessa enheter kan utbyta meddelanden med varandra. toocheck om dessa meddelanden bearbetas på rätt sätt, kan du spåra AS2-, X12, och EDIFACT meddelanden med [Azure logganalys](../log-analytics/log-analytics-overview.md) i hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Du kan till exempel använda dessa webbaserade spårningsfunktioner för spårning av meddelanden:

* Antalet meddelanden och status
* Bekräftelser status
* Korrelera meddelanden med bekräftelser
* Detaljerade felbeskrivningar för fel
* Sökfunktioner

## <a name="requirements"></a>Krav

* En logikapp som har konfigurerats med diagnostikloggning. Läs [hur toocreate en logikapp](logic-apps-create-a-logic-app.md) och [hur tooset in loggning för att logikapp](../logic-apps/logic-apps-monitor-your-logic-apps.md#azure-diagnostics).

* Ett integration-konto som har konfigurerats med övervakning och loggning. Läs [hur toocreate kontonamn integration](../logic-apps/logic-apps-enterprise-integration-create-integration-account.md) och [hur tooset in övervakning och loggning för det kontot](../logic-apps/logic-apps-monitor-b2b-message.md).

* Om du inte redan har gjort [publicera diagnostikdata tooLog Analytics](../logic-apps/logic-apps-track-b2b-messages-omsportal.md) i OMS.

> [!NOTE]
> När du har uppfyllt kraven för hello tidigare, bör du ha en arbetsyta i hello [Operations Management Suite (OMS)](../operations-management-suite/operations-management-suite-overview.md). Du bör använda hello samma OMS-arbetsyta för att spåra dina B2B-kommunikation i OMS. 
>  
> Lär dig mer om du inte har en OMS-arbetsyta [hur toocreate en OMS-arbetsyta](../log-analytics/log-analytics-get-started.md).

## <a name="add-hello-logic-apps-b2b-solution-toohello-operations-management-suite-oms"></a>Lägg till hello Logic Apps B2B-lösning toohello Operations Management Suite (OMS)

toohave OMS spåra B2B-meddelanden för din logikapp, måste du lägga till hello **Logic Apps B2B** lösning toohello OMS-portalen. Lär dig mer om [att lägga till lösningar tooOMS](../log-analytics/log-analytics-get-started.md).

1. I hello [Azure-portalen](https://portal.azure.com), Välj **fler tjänster**. Sök efter ”logganalys” och välj sedan **logganalys** som visas här:

   ![Hitta logganalys](media/logic-apps-track-b2b-messages-omsportal/browseloganalytics.png)

2. Under **logganalys**, söka efter och välj din OMS-arbetsyta. 

   ![Välj din OMS-arbetsyta](media/logic-apps-track-b2b-messages-omsportal/selectla.png)

3. Under **Management**, Välj **OMS-portalen**.

   ![Välj OMS-portalen](media/logic-apps-track-b2b-messages-omsportal/omsportalpage.png)

4. Välj när hello OMS-startsidan öppnas **lösningar galleriet**.    

   ![Välj lösningar galleri](media/logic-apps-track-b2b-messages-omsportal/omshomepage1.png)

5. Under **alla lösningar för**, söka efter och välj **Logic Apps B2B**.     

   ![Välj Logic Apps B2B](media/logic-apps-track-b2b-messages-omsportal/omshomepage2.png)

6. Under **Logic Apps B2B**, Välj **Lägg till**.

   ![Välj Lägg till](media/logic-apps-track-b2b-messages-omsportal/omshomepage3.png)

   Startsidan för hello OMS hello panelen för **Logic Apps B2B-meddelanden** visas nu. 
   Den här panelen uppdaterar antal för hello-meddelande när dina B2B-meddelanden bearbetas.

   ![OMS-startsidan, panelen Logic Apps B2B-meddelanden](media/logic-apps-track-b2b-messages-omsportal/omshomepage4.png)

<a name="message-status-details"></a>

## <a name="track-message-status-and-details-in-hello-operations-management-suite"></a>Spåra meddelandestatus och information i hello Operations Management Suite

1. Du kan visa hello status och information om dessa meddelanden när dina B2B-meddelanden bearbetas. På startsidan för hello OMS väljer hello **Logic Apps B2B-meddelanden** panelen.

   ![Antal uppdaterade meddelande](media/logic-apps-track-b2b-messages-omsportal/omshomepage6.png)

   > [!NOTE]
   > Som standard hello **Logic Apps B2B-meddelanden** innehåller data baserat på en dag. toochange hello data omfång tooa olika intervall, Välj hello scope kontroll hello överst på hello OMS sidan:
   > 
   > ![Ändra omfång för data](media/logic-apps-track-b2b-messages-omsportal/change-interval.png)
   >

2. När status för hello-meddelande visas instrumentpanelen kan du kan visa mer information om en specifik meddelandetyp som visar data baserat på en dag. Välj hello panelen för **AS2**, **X12**, eller **EDIFACT**.

   ![Visa status för meddelandet](media/logic-apps-track-b2b-messages-omsportal/omshomepage5.png)

   En lista med meddelanden visas för din valda sida vid sida. 
   toolearn mer om hello egenskaper för varje meddelandetyp, se dessa beskrivningar för egenskapen meddelande:

   * [Egenskaper för AS2-meddelande](#as2-message-properties)
   * [X12 meddelande egenskaper](#x12-message-properties)
   * [Egenskaper för EDIFACT-meddelande](#EDIFACT-message-properties)

   Till exempel är här en lista över AS2-meddelande kan se ut:

   ![Visa AS2-meddelanden](media/logic-apps-track-b2b-messages-omsportal/as2messagelist.png)

3. tooview eller export hello indata och utdata för specifika meddelanden markera dessa meddelanden, och välj **hämta**. När du uppmanas spara hello ZIP-filen tooyour lokala datorn och sedan extrahera filen. 

   hello extraherade mappen innehåller en mapp för varje vald meddelande. 
   Om du ställer in bekräftelser även hello meddelandet mappen filer med information om bekräftelse. 
   Varje meddelandemapp har minst filerna: 
   
   * Läsbart filer med hello indata nyttolasten och utdata nyttolast-information
   * Kodade filer med hello indata och utdata

   För varje meddelandetyp hittar du hello mappen och namnet filformat här:

   * [AS2 mapp- och name-format](#as2-folder-file-names)
   * [X12 mapp- och name format](#x12-folder-file-names)
   * [EDIFACT mapp- och name-format](#edifact-folder-file-names)

   ![Hämta meddelandefiler](media/logic-apps-track-b2b-messages-omsportal/download-messages.png)

4. tooview alla åtgärder som har hello samma körnings-ID, på hello **loggen Sök** väljer du ett meddelande från hello meddelandelistan.

   Du kan sortera åtgärderna av kolumn, eller söka efter specifika resultat.

   ![Åtgärder med hello samma körnings-ID](media/logic-apps-track-b2b-messages-omsportal/logsearch.png)

   * Välj toosearch resultat med färdiga frågor **Favoriter**.

   * Läs [hur toobuild frågor genom att lägga till filter](logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md). 
   Eller Läs mer om [hur toofind data med loggen söker i logganalys](../log-analytics/log-analytics-log-searches.md).

   * toochange fråga i uppdateringen hello frågan med hello kolumner och värden som du vill toouse som filter i sökrutan hello.

<a name="message-list-property-descriptions"></a>

## <a name="property-descriptions-and-name-formats-for-as2-x12-and-edifact-messages"></a>Egenskapsbeskrivningar och format för AS2-, X 12- och EDIFACT-meddelanden

För varje meddelandetyp följer hello Egenskapsbeskrivningar och format för hämtade meddelandefiler.

<a name="as2-message-properties"></a>

### <a name="as2-message-property-descriptions"></a>AS2 meddelandet Egenskapsbeskrivningar

Här följer beskrivningar av hello-egenskapen för varje AS2-meddelande.

| Egenskap | Beskrivning |
| --- | --- |
| Avsändaren | hello gäst partner som anges i **tar emot inställningar**, eller hello värden partner som anges i **skicka inställningar** för ett AS2-avtal |
| Mottagaren | hello värden partner som anges i **tar emot inställningar**, eller hello gäst partner som anges i **skicka inställningar** för ett AS2-avtal |
| Logikapp | Hej logikapp där hello AS2 åtgärder ställa in |
| Status | hello status för AS2-meddelande <br>Lyckade = mottagna eller skickade ett ogiltigt AS2-meddelande. Ingen MDN har ställts in. <br>Lyckade = mottagna eller skickade ett ogiltigt AS2-meddelande. MDN ställa in och tas emot eller MDN skickas. <br>Det gick inte = tagits emot ett ogiltigt AS2-meddelande. Ingen MDN har ställts in. <br>Väntande = mottagna eller skickade ett ogiltigt AS2-meddelande. MDN ställs in och MDN förväntas. |
| Ack | Hej MDN meddelandestatus <br>Godkänt = tagits emot eller skickats positivt MDN. <br>Väntande = väntar på tooreceive eller skicka en MDN. <br>Avvisade = mottagna eller skickas en negativ MDN. <br>Krävs inte = MDN har inte ställts in i hello avtal. |
| Riktning | hello AS2 riktning |
| Korrelations-ID | hello-ID som korrelerar alla hello utlösare och åtgärder i en logikapp |
| Meddelande-ID | hello AS2-meddelande-ID från hello AS2 meddelandehuvuden |
| tidsstämpel | hello tid när hello AS2-åtgärden bearbetas hello-meddelande |
|          |             |

<a name="as2-folder-file-names"></a>

### <a name="as2-name-formats-for-downloaded-message-files"></a>AS2 namnformat för hämtade meddelandefiler

Här följer hello format för varje mapp för hämtade AS2-meddelanden och filer.

| Mapp eller fil | Format för namn |
| :------------- | :---------- |
| Meddelande-mappen | [avsändaren] \_[mottagaren]\_AS2\_[Korrelations-ID]\_[meddelande-ID]\_[tidsstämpel] |
| Indata, utdata och om att ställa in bekräftelse filer | **Nyttolasten indata**: [avsändaren]\_[mottagaren]\_AS2\_[Korrelations-ID]\_input_payload.txt </p>**Utdata nyttolast**: [avsändaren]\_[mottagaren]\_AS2\_[Korrelations-ID]\_utdata\_payload.txt </p></p>**Indata**: [avsändaren]\_[mottagaren]\_AS2\_[Korrelations-ID]\_inputs.txt </p></p>**Utdata**: [avsändaren]\_[mottagaren]\_AS2\_[Korrelations-ID]\_outputs.txt |
|          |             |

<a name="x12-message-properties"></a>

### <a name="x12-message-property-descriptions"></a>X12 meddelande Egenskapsbeskrivningar

Nedan följer beskrivningar av hello-egenskapen för varje X12 meddelande.

| Egenskap | Beskrivning |
| --- | --- |
| Avsändaren | hello gäst partner som anges i **tar emot inställningar**, eller hello värden partner som anges i **skicka inställningar** för ett X12 avtal |
| Mottagaren | hello värden partner som anges i **tar emot inställningar**, eller hello gäst partner som anges i **skicka inställningar** för ett X12 avtal |
| Logikapp | Hej logikapp där hello X12 åtgärder ställa in |
| Status | status för hello X12 meddelande <br>Lyckade = mottagna eller skickas en giltig X12 meddelande. Inga funktionella ack har ställts in. <br>Lyckade = mottagna eller skickas en giltig X12 meddelande. Funktionella ack ställa in och tas emot eller en funktionell ack skickas. <br>Det gick inte = mottagna eller skickade ett ogiltigt X12 meddelande. <br>Väntande = mottagna eller skickas en giltig X12 meddelande. Funktionella ack har konfigurerats och en fungerande ack förväntas. |
| Ack | Funktionella Ack (997)-status <br>Godkänt = mottagna eller skickas en positiv funktionella bekräftelse. <br>Avvisade = mottagna eller skickas en negativ funktionella bekräftelse. <br>Väntande = förväntas en funktionell ack men inte tagits emot. <br>= Fram en funktionell ack men kan inte skicka toopartner. <br>Krävs inte = funktionella ack har inte ställts in. |
| Riktning | Hej X12 riktning |
| Korrelations-ID | hello-ID som korrelerar alla hello utlösare och åtgärder i en logikapp |
| Typen av meddelande | hello EDI X 12 meddelandetypen |
| ICN | hello Interchange Kontrollnumret för hälsningsmeddelande X12 |
| TSCN | hello ange kontroll transaktionsnumret för hälsningsmeddelande X12 |
| tidsstämpel | hello tid när hello X12 åtgärden bearbetas hello-meddelande |
|          |             |

<a name="x12-folder-file-names"></a>

### <a name="x12-name-formats-for-downloaded-message-files"></a>X12 namn format för hämtade meddelandefiler

Här är hello format för varje hämtat X12 mappar och filer.

| Mapp eller fil | Format för namn |
| :------------- | :---------- |
| Meddelande-mappen | [avsändaren] \_[mottagaren]\_X12\_[interchange-kontroll-nummer]\_[globala-kontroll-nummer]\_[-set-kontroll-transaktionsnumret]\_[tidsstämpel] |
| Indata, utdata och om att ställa in bekräftelse filer | **Nyttolasten indata**: [avsändaren]\_[mottagaren]\_X12\_[interchange-kontroll-nummer]\_input_payload.txt </p>**Utdata nyttolast**: [avsändaren]\_[mottagaren]\_X12\_[interchange-kontroll-nummer]\_utdata\_payload.txt </p></p>**Indata**: [avsändaren]\_[mottagaren]\_X12\_[interchange-kontroll-nummer]\_inputs.txt </p></p>**Utdata**: [avsändaren]\_[mottagaren]\_X12\_[interchange-kontroll-nummer]\_outputs.txt |
|          |             |

<a name="EDIFACT-message-properties"></a>

### <a name="edifact-message-property-descriptions"></a>EDIFACT meddelandet Egenskapsbeskrivningar

Här följer beskrivningar av hello-egenskapen för varje EDIFACT-meddelande.

| Egenskap | Beskrivning |
| --- | --- |
| Avsändaren | hello gäst partner som anges i **tar emot inställningar**, eller hello värden partner som anges i **skicka inställningar** för ett EDIFACT-avtal |
| Mottagaren | hello värden partner som anges i **tar emot inställningar**, eller hello gäst partner som anges i **skicka inställningar** för ett EDIFACT-avtal |
| Logikapp | Hej logikapp där hello EDIFACT åtgärder ställa in |
| Status | hello status för EDIFACT-meddelande <br>Lyckade = mottagna eller skickade ett ogiltigt EDIFACT-meddelande. Inga funktionella ack har ställts in. <br>Lyckade = mottagna eller skickade ett ogiltigt EDIFACT-meddelande. Funktionella ack ställa in och tas emot eller en funktionell ack skickas. <br>Det gick inte = mottagna eller skickade ett ogiltigt EDIFACT-meddelande <br>Väntande = mottagna eller skickade ett ogiltigt EDIFACT-meddelande. Funktionella ack har konfigurerats och en fungerande ack förväntas. |
| Ack | Funktionella Ack (997)-status <br>Godkänt = mottagna eller skickas en positiv funktionella bekräftelse. <br>Avvisade = mottagna eller skickas en negativ funktionella bekräftelse. <br>Väntande = förväntas en funktionell ack men inte tagits emot. <br>= Fram en funktionell ack men kan inte skicka toopartner. <br>Krävs inte = funktionella Ack har inte ställts in. |
| Riktning | hello EDIFACT riktning |
| Korrelations-ID | hello-ID som korrelerar alla hello utlösare och åtgärder i en logikapp |
| Typen av meddelande | hello EDIFACT meddelandetypen |
| ICN | hello Interchange Kontrollnumret för hello EDIFACT-meddelande |
| TSCN | hello ange kontroll transaktionsnumret för hello EDIFACT-meddelande |
| tidsstämpel | hello tid när hello EDIFACT-åtgärden bearbetas hello-meddelande |
|          |               |

<a name="edifact-folder-file-names"></a>

### <a name="edifact-name-formats-for-downloaded-message-files"></a>EDIFACT namnformat för hämtade meddelandefiler

Här följer hello format för varje mapp för hämtade EDIFACT-meddelanden och filer.

| Mapp eller fil | Format för namn |
| :------------- | :---------- |
| Meddelande-mappen | [avsändaren] \_[mottagaren]\_EDIFACT\_[interchange-kontroll-nummer]\_[globala-kontroll-nummer]\_[-set-kontroll-transaktionsnumret]\_[tidsstämpel] |
| Indata, utdata och om att ställa in bekräftelse filer | **Nyttolasten indata**: [avsändaren]\_[mottagaren]\_EDIFACT\_[interchange-kontroll-nummer]\_input_payload.txt </p>**Utdata nyttolast**: [avsändaren]\_[mottagaren]\_EDIFACT\_[interchange-kontroll-nummer]\_utdata\_payload.txt </p></p>**Indata**: [avsändaren]\_[mottagaren]\_EDIFACT\_[interchange-kontroll-nummer]\_inputs.txt </p></p>**Utdata**: [avsändaren]\_[mottagaren]\_EDIFACT\_[interchange-kontroll-nummer]\_outputs.txt |
|          |             |

## <a name="next-steps"></a>Nästa steg

* [Frågan för B2B-meddelanden i Operations Management Suite](../logic-apps/logic-apps-track-b2b-messages-omsportal-query-filter-control-number.md)
* [AS2-spårningsscheman](../logic-apps/logic-apps-track-integration-account-as2-tracking-schemas.md)
* [X12-spårningsscheman](../logic-apps/logic-apps-track-integration-account-x12-tracking-schema.md)
* [Spårning av anpassade scheman](../logic-apps/logic-apps-track-integration-account-custom-tracking-schema.md)