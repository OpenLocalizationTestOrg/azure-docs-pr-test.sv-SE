---
title: "aaaScenario - skapa en kund insikter instrumentpanel med Azure serverlösa | Microsoft Docs"
description: "Ett exempel på hur du kan skapa en instrumentpanel toomanage kund feedback, sociala data och mycket mer med Azure Logikappar och Azure Functions."
keywords: 
services: logic-apps
author: jeffhollan
manager: anneta
editor: 
documentationcenter: 
ms.assetid: d565873c-6b1b-4057-9250-cf81a96180ae
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/29/2017
ms.author: jehollan
ms.openlocfilehash: db175e895e37aa795a9c34bf4d65566bf68f8c37
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-real-time-customer-insights-dashboard-with-azure-logic-apps-and-azure-functions"></a>Skapa en realtid kunden insikter instrumentpanel med Azure Logikappar och Azure Functions

Azure-serverlösa verktyg tillhandahålla kraftfulla funktioner tooquickly bygg- och program i hello molnet utan att behöva toothink om infrastruktur.  I det här scenariot ska vi skapa en instrumentpanel tootrigger på kundfeedback, analysera feedback med machine learning och publicera insikter en källa som Power BI eller Azure Data Lake.

## <a name="overview-of-hello-scenario-and-tools-used"></a>Översikt över scenariot hello och verktyg som används

I ordning tooimplement den här lösningen, vi använder hello två viktiga komponenter av serverlösa appar i Azure: [Azure Functions](https://azure.microsoft.com/services/functions/) och [Azure Logikappar](https://azure.microsoft.com/services/logic-apps/).

Logic Apps är en serverlösa arbetsflödesmotorn i hello molnet.  Det ger orchestration över serverlösa komponenter och även ansluter tooover 100 tjänster och API: er.  I det här scenariot skapar vi en logik app tootrigger kommentarer från kunder.  Hello kopplingar som kan underlätta korsreagerande toocustomer feedback bland Outlook.com, Office 365, undersökning apa, Twitter och en HTTP-begäran [från ett webbformulär](https://blogs.msdn.microsoft.com/logicapps/2017/01/30/calling-a-logic-app-from-an-html-form/).  Hello arbetsflödet nedan, kommer vi övervaka en hashtaggar på Twitter.

Funktioner ger serverlösa beräkning i hello molnet.  I det här scenariot ska vi använda Azure Functions tooflag tweets från kunder baserat på ett antal fördefinierade nyckelord.

hello hela lösningen kan vara [skapa i Visual Studio](logic-apps-deploy-from-vs.md) och [distribueras som en del av en resursmall för](logic-apps-create-deploy-template.md).  Det finns också videogenomgång av hello scenariot [på Channel 9](http://aka.ms/logicappsdemo).

## <a name="build-hello-logic-app-tootrigger-on-customer-data"></a>Skapa hello logik app tootrigger på kundinformation

Efter [att skapa en logikapp](logic-apps-create-a-logic-app.md) i Visual Studio eller hello Azure-portalen:

1. Lägga till en utlösare för **på nya Tweets** från Twitter
2. Konfigurera hello utlösaren toolisten tootweets på ett nyckelord eller en hashtaggar.

   > [!NOTE]
   > hello upprepning egenskapen hello utlösaren avgör hur ofta hello logikapp söker efter nya objekt på avsökning-baserade utlösare

   ![Exempel på Twitter-utlösare][1]

Den här appen används nu på alla nya tweets.  Vi kan sedan ta tweet data och lära dig mer hello sentiment uttryckt.  Detta vi använder hello [Azure kognitiva Service](https://azure.microsoft.com/services/cognitive-services/) toodetect sentiment text.

1. Klicka på **nytt steg**
1. Välj eller söka efter Hej **textanalys** koppling
1. Välj hello **identifiera Sentiment** igen
1. Om du uppmanas ange en giltig nyckel kognitiva tjänster för hello Text Analytics-tjänsten
1. Lägg till hello **Tweet Text** som hello text tooanalyze.

Nu när vi har hello tweet data och insikter på hello tweet kan ett antal andra kopplingar vara relevanta:
* Power BI - Lägg till rader tooStreaming Dataset: Visa tweets realtid på en Power BI-instrumentpanel.
* Azure Data Lake - tilläggsfilen: lägga till kunden data tooan Azure Data Lake dataset tooinclude i analytics-jobb.
* SQL - lägga till rader: lagra data i en databas för senare hämtning.
* Slack - skicka meddelande: varning en slack-kanal på negativ feedback som kräver åtgärder.

En Azure-funktion kan också vara används toodo mer anpassad compute ovanpå hello data.

## <a name="enriching-hello-data-with-an-azure-function"></a>Utöka hello data med en Azure-funktion

Innan vi kan skapa en funktion måste toohave en funktionsapp i vår Azure-prenumeration.  Information om hur du skapar en Azure-funktion i hello portal kan [finns här](../azure-functions/functions-create-first-azure-function-azure-portal.md)

För en funktion toobe anropas direkt från en logikapp, den behöver toohave HTTP aktivera bindning.  Vi rekommenderar att du använder hello **HttpTrigger** mall.

I det här scenariot är hello begärandetexten för hello Azure-funktion hello tweet text.  I Funktionskoden hello, bara definiera logik på om hello tweet texten innehåller ett nyckelord eller en fras.  själva hello-funktionen kan hållas enligt enkla eller avancerade som behövs för hello scenario.

Hello slutet av funktionen hello, bara returnera ett svar toohello logikapp med data.  Detta kan bero på ett enkelt booleskt värde (t.ex. `containsKeyword`), eller ett komplext objekt.

![Steg för konfigurerade Azure-funktion][2]

> [!TIP]
> Vid åtkomst till ett komplext svar från en funktion i en logikapp instruktionen hello parsa JSON.

När du har sparat hello funktionen går att lägga till hello logikapp skapade ovan.  I hello logikapp:

1. Klicka på tooadd en **nytt steg**
1. Välj hello **Azure Functions** koppling
1. Välj toochoose en befintlig funktion och bläddra toohello funktionen skapas
1. Skicka i hello **Tweet Text** för hello **brödtext i begäran**

## <a name="running-and-monitoring-hello-solution"></a>Kör och övervakningslösning hello

En av hello fördelarna med redigering serverlösa orkestreringarna i Logic Apps är hello omfattande felsöknings- och övervakningsfunktionerna.  Alla kör (aktuella eller historiska) kan visas från Visual Studio och hello Azure-portalen eller via hello REST-API och SDK.

Ett av hello enklaste sätt tootest en logikapp använder hello **kör** knappen i hello designer.  Klicka på **kör** fortsätter toopoll hello utlösaren 5 sekunder tills en händelse identifieras, och ger en live-vyn som hello kör fortskrider.

Tidigare kör Historik kan visas på hello översikt blad i hello Azure-portalen, eller via hello Visual Studio Cloud Explorer.

## <a name="creating-a-deployment-template-for-automated-deployments"></a>Skapa en Distributionsmall för automatiserad distribution

När en lösning har utvecklats, kan de hämtats och distribueras via en Azure-distribution mallen tooany Azure-region i hello world.  Detta är användbart för båda ändra parametrarna för olika versioner av det här arbetsflödet, men också för att integrera den här lösningen i en version och utgåva pipeline.  Information om hur du skapar en Distributionsmall finns [i den här artikeln](logic-apps-create-deploy-template.md).

Azure Functions kan också införlivas i mallen för distribution av hello - så hello hela lösningen med alla beroenden kan hanteras som en mall.  Ett exempel på en mall för distribution av funktionen finns i hello [Azure quickstart mallen databasen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-function-app-create-dynamic).

## <a name="next-steps"></a>Nästa steg

* [Visa andra exempel och scenarier för Logikappar i Azure](logic-apps-examples-and-scenarios.md)
* [Titta på en videogenomgång om hur du skapar den här lösningen slutpunkt till slutpunkt](http://aka.ms/logicappsdemo)
* [Lär dig hur toohandle och catch-undantag inom en logikapp](logic-apps-exception-handling.md)

<!-- Image References -->
[1]: ./media/logic-apps-scenario-social-serverless/twitter.png
[2]: ./media/logic-apps-scenario-social-serverless/function.png