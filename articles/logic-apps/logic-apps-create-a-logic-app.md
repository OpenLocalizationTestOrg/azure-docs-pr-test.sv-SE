---
title: "aaaCreate första arbetsflödet mellan molnappar & molntjänster - Azure Logic Apps | Microsoft Docs"
description: "Automatisera affärsprocesser för systemintegrering och EAI-scenarion (Enterprise Application Integration) genom att skapa och köra arbetsflöden i Azure Logic Apps"
author: jeffhollan
manager: anneta
editor: 
services: logic-apps
keywords: workflow, cloud apps, cloud services, business processes, system integration, enterprise application integration, EAI
documentationcenter: 
ms.assetid: ce3582b5-9c58-4637-9379-75ff99878dcd
ms.service: logic-apps
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 03/31/2017
ms.author: LADocs; jehollan; estfan
ms.openlocfilehash: 17ec589b1c8923b5ad3e6479fc856b6ac81754ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-your-first-logic-app-workflow-tooautomate-processes-between-cloud-apps-and-cloud-services"></a>Skapa din första logikapp tooautomate arbetsflödesprocesser mellan molnappar och molntjänster

Utan att skriva kod kan du automatisera affärsprocesser enklare och snabbare när du skapar och kör arbetsflöden med [Azure Logic Apps](logic-apps-what-are-logic-apps.md). Den här första exemplet visar hur toocreate ett grundläggande logiken app arbetsflöde som söker en RSS-feed för nytt innehåll på en webbplats. När nya objekt visas i hello webbplats feed skickar hello logikapp e-post från ett Outlook eller Gmail-konto.

toocreate och kör en logikapp, vad du behöver:

* En Azure-prenumeration. Om du inte har en prenumeration kan du [börja med ett kostnadsfritt Azure-konto](https://azure.microsoft.com/free/). Annars kan du [registrera dig för en prenumeration enligt principen Betala per användning](https://azure.microsoft.com/pricing/purchase-options/).

  Din Azure-prenumeration används för fakturering av användning av logikappar. Lär dig hur [användningsmätning](../logic-apps/logic-apps-pricing.md) och [prissättning](https://azure.microsoft.com/pricing/details/logic-apps) fungerar för Azure Logic Apps.

Det här exemplet kräver också dessa objekt:

* Ett Outlook.com-, Office 365 Outlook- eller Gmail-konto

    > [!TIP]
    > Om du har ett personligt [Microsoft-konto](https://account.microsoft.com/account) har du ett Outlook.com-konto. Om du har ett Azure-arbetskonto eller -skolkonto har du annars ett **Office 365 Outlook**-konto.

* En länk tooa webbplatsens RSS-feed. Det här exemplet används hello [RSS-feed för det här numret från hello CNN.com webbplats](http://rss.cnn.com/rss/cnn_topstories.rss):`http://rss.cnn.com/rss/cnn_topstories.rss`

## <a name="add-a-trigger-that-starts-your-workflow"></a>Lägg till en utlösare som startar arbetsflödet

En [ *utlösaren* ](./logic-apps-what-are-logic-apps.md#logic-app-concepts) är en händelse som startar logik app arbetsflödet och hello första element som din logikapp måste.

1. Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen").

2. Välj hello vänstra menyn **ny** > **Enterprise Integration** > **Logikapp** som visas här:

     ![Azure Portal, Ny, Enterprise-integration, Logikapp](media/logic-apps-create-a-logic-app/azure-portal-create-logic-app.png)

   > [!TIP]
   > Du kan också välja **ny**, Skriv i sökrutan hello `logic app`, och tryck på RETUR. Välj **Logikapp** > **Skapa**.

3. Namnge logikappen och välj din Azure-prenumeration. Skapa eller välj nu en Azure-resursgrupp som hjälper dig att organisera och hantera relaterade Azure-resurser. Välj slutligen hello datacenter plats som värd för din logikapp. När du är klar kan du välja **PIN-kod toodashboard** och sedan **skapa**.

     ![Information om logikappar](media/logic-apps-create-a-logic-app/logic-app-settings.png)

   > [!NOTE]
   > När du väljer **PIN-kod toodashboard**, logikappen visas på hello Azure instrumentpanelen efter distributionen och öppnas automatiskt. Om din logikapp inte visas på instrumentpanelen hello på hello **alla resurser** panelen, väljer **finns mer**, och välj din logikapp. Eller välj hello vänstra menyn **fler tjänster**. Välj **Logic Apps** under **Enterprise-integration** och välj sedan din logikapp.

4. När du öppnar din logikapp för hello första gången, visar hello logik App Designer mallar som du kan använda tooget igång. Välj nu **Tom logikapp** så att du kan bygga din logikapp från början.

    hello logik App Designer öppnas och visar tillgängliga tjänster och *utlösare* som du kan använda i din logikapp.

5. Skriv i sökrutan hello `RSS`, och välj den här utlösaren: **RSS - när en feed objekt publiceras** 

    ![RSS-utlösare](media/logic-apps-create-a-logic-app/rss-trigger.png)

6. Ange hello länk hello webbplats RSS-feed som du vill tootrack. 

     Du kan också ändra **Frekvens** och **Intervall**. 
     Inställningarna avgör hur ofta logikappen ska söka efter nya objekt och returnera alla objekt som hittas under det angivna tidsintervallet.

     I det här exemplet ska vi Kontrollera att varje dag för aktuellt Bokförd toohello CNN webbplats.

     ![Konfigurera utlösare med RSS-flöde, frekvens och intervall](media/logic-apps-create-a-logic-app/rss-trigger-setup.png)

7. Spara ditt arbete nu. (På hello designer kommandofältet väljer **spara**.)

   ![Spara din logikapp](media/logic-apps-create-a-logic-app/save-logic-app.png)

   När du sparar, logikappen lanseras, men för närvarande logikappen endast söker efter nya objekt i angivna hello RSS-feed. 
   toomake utlöses för det här exemplet är mer användbar vi lägga till en åtgärd som din logikapp utför efter utlösaren.

## <a name="add-an-action-that-responds-tooyour-trigger"></a>Lägg till en åtgärd som svarar tooyour utlösare

En [*åtgärd*](./logic-apps-what-are-logic-apps.md#logic-app-concepts) är en aktivitet som utförs av logikapparbetsflödet. När du lägger till en utlösare tooyour logikapp, du kan lägga till en åtgärd tooperform åtgärder med data som genereras av den här utlösaren. I vårt exempel vi nu lägga till en åtgärd som skickar e-postmeddelande när nya objekt visas i hello webbplats RSS-feed.

1. Hello designer under utlösaren, Välj **nytt steg** > **lägga till en åtgärd** som visas här:

   ![Lägga till en åtgärd](media/logic-apps-create-a-logic-app/add-new-action.png)

   Hej designer visar [tillgängliga kopplingar](../connectors/apis-list.md) så att du kan välja en tooperform åtgärd när utlösaren utlöses.

2. Baserat på ditt e-postkonto, åtgärderna hello för Outlook eller Gmail.

   * Ange toosend e-post från kontot Outlook i sökrutan hello `outlook`. Under **Tjänster** väljer du **Outlook.com** för personliga Microsoft-konton eller välj **Office 365 Outlook** Azure-arbetskonton eller -skolkonton. 
   Gå till **Åtgärder** och välj **Skicka ett e-postmeddelande**.

       ![Välj Outlook-åtgärden Skicka ett e-postmeddelande](media/logic-apps-create-a-logic-app/actions.png)

   * Ange toosend e-post från kontot Gmail i sökrutan hello `gmail`. 
   Gå till **Åtgärder** och välj **Skicka e-post**.

       ![Välj ”Gmail – Skicka e-post”](media/logic-apps-create-a-logic-app/actions-gmail.png)

3. När du uppmanas att ange autentiseringsuppgifter kan du logga in med hello användarnamnet och lösenordet för ditt e-postkonto. 

4. Ange hello information för den här åtgärden som hello mål e-postadress och välj hello parametrar för hello data tooinclude i hello e-post, till exempel:

   ![Välj data tooinclude i e-post](media/logic-apps-create-a-logic-app/rss-action-setup.png)

    Så om du väljer Outlook kan logikappen exempelvis se ut så här:

    ![Slutförd logikapp](media/logic-apps-create-a-logic-app/save-run-complete-logic-app.png)

5.  Spara ändringarna. (På hello designer kommandofältet väljer **spara**.)

6. Du kan nu manuellt köra din logikapp för testning. På hello designer kommandofältet väljer **kör**. Annars kan du låta din logikapp Kontrollera hello angetts baserat på hello-schema som du ställer in en RSS-feed.

   Om din logikapp hittar nya objekt, skickar hello logikapp e-postmeddelande som innehåller valda data. 
   Om det finns inga nya objekt logikappen hoppar över hello-åtgärd som skickar e-postmeddelande.

7. toomonitor och kontrollera din logikapp är kör och utlösa historik på logiken app-menyn väljer **översikt**.

   ![Övervaka och visa logikappens körnings- och utlösningshistorik](media/logic-apps-create-a-logic-app/logic-app-run-trigger-history.png)

   > [!TIP]
   > Om du inte hittar hello data som du förväntar dig i hello kommandofält försöka **uppdatera**.

   Mer information om status för din logikapp toolearn eller kör och utlösa historiken eller toodiagnose din logikapp, se [felsöka din logikapp](logic-apps-diagnosing-failures.md).

      > [!NOTE]
      > Logikappen fortsätter att köra tills du stänger av den. tooturn av din app just nu på logiken app-menyn, Välj **översikt**. Välj på hello kommandofältet **inaktivera**.

Gratulerar! Du har precis konfigurerat och kört din första baslogikapp. Du fick också lära dig hur enkelt du kan skapa arbetsflöden för att automatisera processer och integrera molnappar och molntjänster – utan att skriva kod.

## <a name="manage-your-logic-app"></a>Hantera din logikapp

toomanage din app kan du utföra uppgifter som Kontrollera hello status, redigera, visa historik, inaktivera eller ta bort din logikapp.

1. Logga in toohello [Azure-portalen](https://portal.azure.com "Azure-portalen").

2. Välj hello vänstra menyn **fler tjänster**. Välj **Logikappar** under **Enterprise-integration**. Välj din logikapp. 

   Hello logik app menyn hittar du dessa hanteringsaktiviteter för logik app:

   |Aktivitet|Steg| 
   |:---|:---| 
   | Visa appens status, körningshistorik och allmän information| Välj **Översikt**.| 
   | Redigera din app | Välj **Logic App Designer**. | 
   | Visa arbetsflödet för JSON-definitionen för appen | Välj **Logikapp kodvy**. | 
   | Visa åtgärder som utförs i logikappen | Välj **Aktivitetslogg**. | 
   | Visa tidigare versioner för din logikapp | Välj **Versioner**. | 
   | Inaktivera din app tillfälligt | Välj **översikt**, på hello kommandofältet Välj **inaktivera**. | 
   | Ta bort din app | Välj **översikt**, på hello kommandofältet Välj **ta bort**. Ange logikappens namn och välj **Ta bort**. | 

## <a name="get-help"></a>Få hjälp

tooask frågor besvara frågor och lär dig vilka andra Azure Logikappar användarna gör besök hello [Logikappar i Azure-forum](https://social.msdn.microsoft.com/Forums/en-US/home?forum=azurelogicapps).

toohelp förbättra Azure Logic Apps och kopplingar, rösta eller skicka in förslag på hello [Azure Logikappar användare feedbackwebbplats](http://aka.ms/logicapps-wish).

## <a name="next-steps"></a>Nästa steg

*  [Lägga till villkor och kör arbetsflöden](../logic-apps/logic-apps-use-logic-app-features.md)
*    [Mallar för logikappar](../logic-apps/logic-apps-use-logic-app-templates.md)
*  [Skapa logikappar från Azure Resource Manager-mallar](../logic-apps/logic-apps-arm-provision.md)
