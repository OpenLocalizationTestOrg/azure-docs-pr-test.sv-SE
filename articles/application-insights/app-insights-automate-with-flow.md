---
title: aaaAutomate Azure Application Insights bearbetar med Microsoft Flow
description: "Lär dig hur du kan använda Microsoft Flow tooquickly automatisera repeterbara processer med hjälp av hello Application Insights connector."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 06/25/2017
ms.author: bwren
ms.openlocfilehash: b34488a6b8b8b0a6add960a67f1426cbbbc13552
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="automate-azure-application-insights-processes-with-hello-connector-for-microsoft-flow"></a>Automatisera Azure Application Insights-processer med hello connector för Microsoft Flow

Hittar du själv körs flera gånger hello samma frågor på dina telemetri data toocheck som din tjänst fungerar korrekt? Är du söker tooautomate dessa frågor för att hitta trender och avvikelser och skapa egna arbetsflöden runt dem? hello Azure Application Insights connector (förhandsversion) för Microsoft Flow är hello rätt verktyg för dessa ändamål.

Med den här integreringen kan du automatisera många processer nu utan att skriva en enda rad kod. När du har skapat ett flöde med hjälp av en Application Insights-åtgärden körs hello flödet automatiskt Application Insights Analytics-fråga. 

Du kan lägga till ytterligare åtgärder samt. Microsoft Flow tillgängliggör hundratals åtgärder. Du kan till exempel använda Microsoft Flow tooautomatically skicka ett e-postmeddelande eller skapa ett programfel i Visual Studio Team Services. Du kan också använda en av hello många [mallar](https://ms.flow.microsoft.com/en-us/connectors/shared_applicationinsights/?slug=azure-application-insights) som är tillgängliga för hello connector för Microsoft Flow. Dessa mallar snabbare hello processen att skapa ett flöde. 

<!--hello Application Insights connector also works with [Azure Power Apps](https://powerapps.microsoft.com/en-us/) and [Azure Logic Apps](https://azure.microsoft.com/services/logic-apps/?v=17.23h). --> 

## <a name="create-a-flow-for-application-insights"></a>Skapa ett flöde för Application Insights

I kursen får du lära dig hur toocreate ett flöde som använder hello Analytics automatiskt klustret algoritmen toogroup attribut i hello data för ett webbprogram. hello flödet skickar automatiskt hello resultaten via e-post, bara ett exempel på hur du kan använda Microsoft Flow och Application Insights Analytics tillsammans. 

### <a name="step-1-create-a-flow"></a>Steg 1: Skapa ett flöde
1. Logga in för[Microsoft Flow](http://flow.microsoft.com), och välj sedan **Mina flödar**.
2. Klicka på **skapa ett flöde från tomt**.

### <a name="step-2-create-a-trigger-for-your-flow"></a>Steg 2: Skapa en utlösare för din flöde
1. Välj **schema**, och välj sedan **schema - upprepning**.
2. I hello **frekvens** väljer **dag**, och i hello **intervall** ange **1**.

    ![Dialogrutan för Microsoft Flow utlösare](./media/app-insights-automate-with-flow/flow1.png)


### <a name="step-3-add-an-application-insights-action"></a>Steg 3: Lägg till en Application Insights-åtgärd
1. Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.
2. Sök efter **Azure Application Insights**.
3. Klicka på **Azure Application Insights – visualisera Analytics fråga Preview**.

    ![Kör Analytics query-fönstret](./media/app-insights-automate-with-flow/flow2.png)

### <a name="step-4-connect-tooan-application-insights-resource"></a>Steg 4: Anslut tooan Application Insights-resurs

toocomplete det här steget behöver du ett program-ID och en API-nyckel för din resurs. Du kan hämta dem från hello Azure-portalen som visas i följande diagram hello:

![Program-ID i hello Azure-portalen](./media/app-insights-automate-with-flow/appid.png) 

- Ange ett namn för anslutningen, tillsammans med hello program-ID och API-nyckeln.

    ![Fönstret för Microsoft Flow-anslutning](./media/app-insights-automate-with-flow/flow3.png)

### <a name="step-5-specify-hello-analytics-query-and-chart-type"></a>Steg 5: Ange hello Analytics-fråga och -diagram
Den här exempelfråga väljer hello misslyckades-förfrågningar inom hello sista dagen och korrelerar dem med undantag som uppstått som en del av hello igen. Analytics korrelerar dem baserat på hello operation_Id identifierare. hello frågan segment sedan hello resultat med hjälp av hello autocluster algoritm. 

När du skapar egna frågor kan du kontrollera att de fungerar korrekt i Analytics innan du lägger till den tooyour flöde.

- Lägg till hello Analytics-fråga efter och välj sedan diagramtyp för hello HTML-tabellen. 

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
    
    ![Analytics frågefönstret konfiguration](./media/app-insights-automate-with-flow/flow4.png)

### <a name="step-6-configure-hello-flow-toosend-email"></a>Steg 6: Konfigurera hello flödet toosend e-post

1. Klicka på **nytt steg**, och klicka sedan på **lägga till en åtgärd**.
2. Sök efter **Office 365 Outlook**.
3. Klicka på **Office 365 Outlook – skicka ett e-post**.

    ![Urvalsfönster för Office 365 Outlook](./media/app-insights-automate-with-flow/flow2b.png)

4. I hello **skickar ett e-** fönstret hello följande:

   a. Ange hello e-postadressen till mottagaren hello.

   b. Skriv ett ämne för e-post hello.

   c. Klicka någonstans i hello **brödtext** rutan och välj sedan på hello dynamiskt innehåll-menyn som öppnas på höger hello **brödtext**.

   d. Klicka på **visa avancerade alternativ**.

    ![Outlook-konfiguration för Office 365](./media/app-insights-automate-with-flow/flow5.png)

5. Hello dynamiskt innehåll menyn hello följande:

    a. Välj **Bilagenamn**.

    b. Välj **bifogade filer**.
    
    c. I hello **är HTML** väljer **Ja**.

    ![Fönster för Office 365 e-konfiguration](./media/app-insights-automate-with-flow/flow7.png)

### <a name="step-7-save-and-test-your-flow"></a>Steg 7: Spara och testa din flöde
- I hello **flöde namnet** , lägga till ett namn för din flöde, och klicka sedan **skapa flödet**.

    ![Skapande av flödet fönster](./media/app-insights-automate-with-flow/flow8.png)

Du kan vänta på hello utlösaren toorun åtgärden eller köra hello flödet direkt av [körs hello utlösare på begäran](https://flow.microsoft.com/blog/run-now-and-six-more-services/).

När hello flödet körs får hello mottagare som du har angett i hello e-postlistan ett e-postmeddelande som ser ut som följande hello:

![E-postexempel](./media/app-insights-automate-with-flow/flow9.png)


## <a name="next-steps"></a>Nästa steg

- Lär dig mer om hur du skapar [Analytics-frågor](app-insights-analytics-using.md).
- Lär dig mer om [Microsoft Flow](https://ms.flow.microsoft.com).



<!--Link references-->





