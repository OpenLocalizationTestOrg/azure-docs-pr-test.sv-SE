---
title: "aaaAutomate Azure Application Insights bearbetar med hjälp av Logic Apps."
description: "Lär dig hur du snabbt kan automatisera repeterbara processer genom att lägga till hello Application Insights connector tooyour logikapp."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/29/2017
ms.author: bwren
ms.openlocfilehash: 8565aadf0644ffb10da8a0964821901907db2e27
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-application-insights-processes-by-using-logic-apps"></a>Automatisera processer för Application Insights med hjälp av Logic Apps

Hittar du själv upprepade gånger körs hello samma frågor på dina telemetri data toocheck om tjänsten fungerar korrekt? Är du söker tooautomate dessa frågor för att hitta trender och avvikelser och skapa egna arbetsflöden runt dem? hello Azure Application Insights connector (förhandsversion) för Logic Apps är hello rätt verktyg för detta ändamål.

Med den här integreringen kan du automatisera många processer utan att skriva en enda rad kod. Du kan skapa en logikapp med hello Application Insights connector tooquickly automatisera processer Application Insights. 

Du kan lägga till ytterligare åtgärder samt. hello Logic Apps-funktionen i Azure Apptjänst tillgängliggör hundratals åtgärder. Till exempel med hjälp av en logikapp, kan du automatiskt skicka ett e-postmeddelande eller skapa ett programfel i Visual Studio Team Services. Du kan också använda en av hello många tillgängliga [mallar](https://docs.microsoft.com/azure/logic-apps/logic-apps-use-logic-app-templates) toohelp påskynda hello processen att skapa din logikapp. 

## <a name="create-a-logic-app-for-application-insights"></a>Skapa en logikapp för Application Insights

I kursen får du lära dig hur toocreate en logikapp som använder hello Analytics autocluster algoritmen toogroup attribut i hello data för ett webbprogram. hello flödet skickar automatiskt hello resultaten via e-post, bara ett exempel på hur du kan använda Application Insights Analytics och Logic Apps tillsammans. 

### <a name="step-1-create-a-logic-app"></a>Steg 1: Skapa en logikapp
1. Logga in toohello [Azure-portalen](https://portal.azure.com).
2. I hello **ny** väljer **webb + mobilt**, och välj sedan **Logikapp**.

    ![Nytt logik app fönster](./media/automate-with-logic-apps/logicapp1.png)

### <a name="step-2-create-a-trigger-for-your-logic-app"></a>Steg 2: Skapa en utlösare för din logikapp
1. I hello **logik App Designer** fönstret under **börja med en gemensam utlösare**väljer **återkommande**.

    ![Logik App Designer fönster](./media/automate-with-logic-apps/logicapp2.png)

2. I hello **frekvens** väljer **dag** och sedan, i hello **intervall** skriver **1**.

    ![Logik App Designer ”återkommande” fönster](./media/automate-with-logic-apps/step2b.png)

### <a name="step-3-add-an-application-insights-action"></a>Steg 3: Lägg till en Application Insights-åtgärd
1. Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.

2. I hello **välja en åtgärd** sökrutan, Skriv **Azure Application Insights**.

3. Under **åtgärder**, klickar du på **Azure Application Insights – visualisera Analytics fråga Preview**.

    ![Logik App Designer ”Välj åtgärden” fönster](./media/automate-with-logic-apps/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a>Steg 4: Anslut tooan Application Insights-resurs

toocomplete det här steget behöver du ett program-ID och en API-nyckel för din resurs. Du kan hämta dem från hello Azure-portalen som visas i följande diagram hello:

![Program-ID i hello Azure-portalen](./media/automate-with-logic-apps/appid.png) 

Ange ett namn för anslutningen, hello program-ID och hello API-nyckel.

![Fönstret för logik App Designer flödet anslutning](./media/automate-with-logic-apps/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a>Steg 5: Ange hello Analytics-fråga och -diagram
I följande exempel hello, hello frågan väljer hello misslyckades-förfrågningar inom hello sista dagen och korrelerar dem med undantag som uppstått som en del av hello igen. Analytics korrelerar hello misslyckades-förfrågningar, baserat på hello operation_Id identifierare. hello frågan segment sedan hello resultat med hjälp av hello autocluster algoritm. 

När du skapar egna frågor kan du kontrollera att de fungerar korrekt i Analytics innan du lägger till den tooyour flöde.

1. I hello **frågan** lägger du till hello följande Analytics-fråga: 

    ```
    requests
    | where timestamp > ago(1d)
    | where success == "False"
    | project name, operation_Id
    | join ( exceptions
        | project problemId, outerMessage, operation_Id
    ) on operation_Id
    | evaluate autocluster()
    ```

2. I hello **diagramtyp** väljer **HTML-tabell**.

    ![Analytics frågefönstret konfiguration](./media/automate-with-logic-apps/flow4.png)

### <a name="step-6-configure-hello-logic-app-toosend-email"></a>Steg 6: Konfigurera hello logik app toosend e-post

1. Klicka på **nytt steg**, och välj sedan **lägga till en åtgärd**.

2. Skriv i sökrutan hello **Office 365 Outlook**.

3. Klicka på **Office 365 Outlook – skicka ett e-post**.

    ![Val av Office 365 Outlook](./media/automate-with-logic-apps/flow2b.png)

4. I hello **skickar ett e-** fönstret hello följande:

   a. Ange hello e-postadressen till mottagaren hello.

   b. Skriv ett ämne för e-post hello.

   c. Klicka någonstans i hello **brödtext** rutan och välj sedan på hello dynamiskt innehåll-menyn som öppnas på höger hello **brödtext**.

   d. Klicka på **visa avancerade alternativ**.

      ![Outlook-konfiguration för Office 365](./media/automate-with-logic-apps/flow5.png)

5. Hello dynamiskt innehåll menyn hello följande:

    a. Välj **Bilagenamn**.

    b. Välj **bifogade filer**.
    
    c. I hello **är HTML** väljer **Ja**.

      ![Skärm för konfiguration av Office 365 e-post](./media/automate-with-logic-apps/flow7.png)

### <a name="step-7-save-and-test-your-logic-app"></a>Steg 7: Spara och testa din logikapp
* Klicka på **spara** toosave ändringarna.

Du kan vänta hello utlösaren toorun hello logikapp eller köra omedelbart hello logikapp genom att välja **kör**.

![Logik app skapa skärmen](./media/automate-with-logic-apps/step7.png)

När logikappen körs får hello mottagare som du angav i hello e-postlistan ett e-postmeddelande som ser ut som följande hello:

![Logik appen e-postmeddelande](./media/automate-with-logic-apps/flow9.png)

## <a name="next-steps"></a>Nästa steg

- Lär dig mer om hur du skapar [Analytics-frågor](app-insights-analytics-using.md).
- Lär dig mer om [Logikappar](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps).



<!--Link references-->





