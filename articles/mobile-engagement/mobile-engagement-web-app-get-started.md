---
title: "Komma igång med Azure Mobile Engagement för Web Apps | Microsoft Docs"
description: "Lär dig hur du använder Azure Mobile Engagement med analyser och push-meddelanden för Web Apps."
services: mobile-engagement
documentationcenter: Mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 04afe53a-4caf-4c80-bd75-20cc630cd75c
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: js
ms.topic: hero-article
ms.date: 06/01/2016
ms.author: piyushjo
ms.openlocfilehash: abcb04e4e0a3ae4fdba3a4ded20b3846ac3b21e6
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Komma igång med Azure Mobile Engagement för Web Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här ämnet visar hur du använder Azure Mobile Engagement för att förstå din webbappanvändning.

> [!NOTE]
> Tjänsten Azure Mobile Engagement kommer att dras tillbaka i mars 2018 och är för närvarande endast tillgänglig för befintliga kunder. Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

För den här kursen behöver du följande:

* Visual Studio 2015 eller något annat redigeringsprogram
* [Web SDK](http://aka.ms/P7b453)

Web SDK är i förhandsgranskningsläge med endast stöd för Analytics för tillfället och saknar stöd för att skicka webbläsar-push-meddelanden och push-meddelanden i appar. 

> [!NOTE]
> Du måste ha ett aktivt Azure-konto för att slutföra den här kursen. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a>Konfigurera Mobile Engagement för din Web-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Anslut appen till Mobile Engagement-serverdelen
I de här självstudierna behandlas en ”grundläggande integration”, vilket är den minsta uppsättningen som krävs för att samla in data.

Vi skapar en grundläggande webbapp med Visual Studio för att demonstrera integrationen, men du kan även följa stegen med alla typer av webbprogram som har skapats utanför Visual Studio. 

### <a name="create-a-new-web-app"></a>Skapa en ny Web-app
I följande steg används Visual Studio 2015, men stegen är ganska lika i tidigare versioner av Visual Studio. 

1. Starta Visual Studio och välj **New Project** (Nytt projekt) på **startskärmen**.
2. Välj **Web** -> **ASP.Net-webbapp** i popup-fönstret. Ange appens **Namn** och **Plats** samt **Lösningsnamn** och klicka sedan på **OK**.
3. I popup-fönstret **Välj en mall** väljer du **Tom** under **ASP.Net 4.5-mallar** och klickar på **OK**. 

Du har nu skapat ett nytt tomt Windows App-projekt i vilket vi kommer att integrera Azure Mobile Engagement Web SDK.

### <a name="connect-your-app-to-mobile-engagement-backend"></a>Ansluta appen till Mobile Engagement-serverdelen
1. Skapa en ny mapp som kallas **javascript** i din lösning och lägg till Web SDK JS-filen **azure engagement.js** i den. 
2. Lägg till en ny fil med namnet **main.js** i denna javascript-mapp med följande kod. Se till att uppdatera anslutningssträngen. Detta `azureEngagement`-objekt används för att få åtkomst till Web SDK-metoder. 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio med js-filer][1]

## <a name="enable-real-time-monitoring"></a>Aktivera realtidsövervakning
För att kunna börja skicka data och försäkra dig om att användarna är aktiva måste du skicka minst en aktivitet till Mobile Engagement-serverdelen. En aktivitet i kontexten för en webbapp är en webbsida. 

1. Skapa en ny sida med namnet **home.html** i din lösning och ange den som startsida för din webbapp. 
2. Inkludera de två JavaScript-skript som vi lade till tidigare på den här sidan genom att lägga till följande i brödtextstaggen. 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. Uppdatera brödtextstaggen för att anropa EngagementAgents `startActivity`-metod
   
        <body onload="engagement.agent.startActivity('Home')">
4. Så här kommer din **home.html** att se ut
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a>Anslut appen med realtidsövervakning
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a>Utökad analys
Här visas alla metoder som för närvarande finns tillgängliga med Web SDK som du kan använda för analys:

1. Aktiviteter/webbsidor:
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. Händelser
   
        engagement.agent.sendEvent(name, extras);
3. Fel
   
        engagement.agent.sendError(name, extras);
4. Jobb
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

