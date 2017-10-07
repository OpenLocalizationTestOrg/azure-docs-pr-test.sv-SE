---
title: "aaaGet igång med Azure Mobile Engagement för Web Apps | Microsoft Docs"
description: "Lär dig hur toouse Azure Mobile Engagement med analyser och push-meddelanden för webbprogram."
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
ms.openlocfilehash: a84c96cac13bf3b85e72aef55da5c91693e1766c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a>Komma igång med Azure Mobile Engagement för Web Apps
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand Web App-användning.

> [!NOTE]
> hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder. Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).

Den här kursen kräver hello följande:

* Visual Studio 2015 eller något annat redigeringsprogram
* [Web SDK](http://aka.ms/P7b453)

Web SDK är en förhandsversion och endast stöder Analytics tillfället hello och skicka webbläsare eller i appen push-meddelanden stöder inte ännu. 

> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a>Konfigurera Mobile Engagement för din Web-app
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen
Den här kursen behandlas en ”grundläggande integration”, vilket är hello minsta uppsättningen som krävs toocollect data.

Vi skapar en enkel webbapp med Visual Studio toodemonstrate hello integration trots att du kan följa hello steg med alla webbprogram som skapats utanför Visual Studio också. 

### <a name="create-a-new-web-app"></a>Skapa en ny Web-app
hello förutsätter följande steg hello användning av Visual Studio 2015 om hello stegen är ungefär som i tidigare versioner av Visual Studio. 

1. Starta Visual Studio och i hello **Start** väljer **nytt projekt**.
2. Välj i popup-fönstret hello **Web** -> **ASP.Net-webbprogram**. Fyll i hello app **namn**, **plats** och **lösningsnamn**, och klicka sedan på **OK**.
3. I hello **Välj en mall** popup-fönstret Välj **tom** under **ASP.Net 4.5 mallar** och på **OK**. 

Nu har du skapat ett nytt tomt Web App-projekt där vi integrerar hello Azure Mobile Engagement Web SDK.

### <a name="connect-your-app-toomobile-engagement-backend"></a>Ansluta appen tooMobile Engagement-serverdelen
1. Skapa en ny mapp som kallas **javascript** i din lösning och Lägg till hello Web SDK JS filen **azure engagement.js** till den. 
2. Lägg till en ny fil med namnet **main.js** i den här javascript-mappen med hello följande kod. Se till att tooupdate hello anslutningssträngen. Detta `azureEngagement` objektet kommer att använda tooaccess Web SDK-metoder. 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio med js-filer][1]

## <a name="enable-real-time-monitoring"></a>Aktivera realtidsövervakning
Du måste skicka minst en aktivitet toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva. En aktivitet i hello kontexten för ett webbprogram är en webbsida. 

1. Skapa en ny sida som kallas **home.html** i din lösning och ange den som hello startar sidan för webbappen. 
2. Inkludera hello två JavaScript-skript vi har lagt till tidigare i den här sidan genom att lägga till följande hello inom hello body-taggen. 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. Uppdatera hello brödtext taggen toocall Engagementagent's `startActivity` metod
   
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
Här är alla hello-metoder som är tillgängliga med webbtjänst-SDK som du kan använda för analys av:

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

