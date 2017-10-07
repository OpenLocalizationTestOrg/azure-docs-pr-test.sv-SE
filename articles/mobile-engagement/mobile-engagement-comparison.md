---
title: "aaaComparing Azure Mobile Engagement med andra liknande Azure-tjänster"
description: "Jämföra Azure Mobile Engagement med andra liknande Azure-tjänster - HockeyApp, AppInsights, Notification Hubs"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: 1f114775-3a9a-4dd4-8d59-b10d1da9a68b
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 0859ae9980d0fa96ffbc0edfbd795ccc58d2c843
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="comparing-azure-mobile-engagement-with-other-similar-azure-services"></a>Jämföra Azure Mobile Engagement med andra liknande Azure-tjänster
hello lista över tjänster som erbjuds av Microsoft Azure expanderande någonsin och ibland kanske du undrar hur är Azure Mobile Engagement skiljer sig den här tjänsten som du läser eller talas om. Den här artikeln försöker tooclear hello förvirring och dirigerar toochoose Azure Mobile Engagement när det är mest lämpliga för din användning. 

Azure Mobile Engagement är en tjänst som är avsedda specifikt **för digitala marknadsförare/CMOs** men kan användas av alla **mobilappen ägare eller utgivare** som vill tooincrease hello användning, kvarhållning och Monetarisering av sina mobila appar. 

*Det är en programvara som en tjänst (SaaS) användaren plattform som tillhandahåller datadriven statistik för appanvändning, användarsegmentering, och möjliggör kontextmedvetna push-meddelanden och meddelanden i appen.* 

Bryta ned den här definitionen kan har vi hello följande viktiga egenskaper som visar även dess unika värdeförslag:

1. **Kontextmedvetna push-meddelanden och meddelanden i appen**
   
   Detta är hello core fokus för hello produkt - utför mål- och anpassade push-meddelanden. Och för den här toohappen vi samlar in omfattande beteendeanalys data. 
2. **Datadriven statistik för appanvändning**
   
   Vi ger plattformsoberoende SDK toocollect hello beteendeanalys om hello appanvändare. Observera hello termen beteendeanalys (mot prestanda analytics) eftersom vi fokusera på hur hello app använder hello app. Vi samlar in grundläggande analytics prestandadata om fel, krascher etc men som inte hello core fokus hello produkten. 
3. **Användarsegmentering**
   
   När du har samlat in data i appen användarnas beteendeanalys vi att du toosegment målgruppen utifrån olika parametrar och insamlade data tooenable du toorun riktade push-kampanjer. 
4. **Programvara som en-tjänst (SaaS):**
   
   Vi har en portal separat från hello Azure-hanteringsportalen som är optimerade toointeract och visa omfattande beteendeanalys om hello appanvändare och köra marknadsföringskampanjer push. hello produkten är inriktat tooget du kommer ingen tid!   

Med den här uppsättningen skillnad i hand här är hur vi Jämför med andra liknande till synes Azure-erbjudanden – Observera hello artikeln inte kan göra en funktionsjämförelse men tar en mer scenariot baserade metoden toodefine vilka produkten fungerar bäst:

1. [HockeyApp](https://azure.microsoft.com/services/hockeyapp/) är hello Microsofts mobila DevOps-lösning. Du kan distribuera versioner toobeta Testare samla in kraschdata och hämta användarfeedback. Det är integrerat med Visual Studio Team Services aktiverar enkelt skapa distributioner och arbetsuppgiftsintegrering. 
   
   hello fokuserar här på DevOps och att samla in analytics prestandadata om hello mobila appar. Kan det sluta med att integrera båda HockeyApps och Mobile Engagement i din app och som inte ovanliga eftersom även om det inte finns några överlappar varandra i hello insamlade data, hello core hello produkter fokuserar på olika och de hjälper med att uppnå olika mål för dig.  
2. [Application Insights](../application-insights/app-insights-overview.md) om din mobila app har en server-side sedan du kommer att använda Application Insights toomonitor hello web server-side för din app men för hello klienten sida mobila appar, bör du använda HockeyApp. 
3. [Notification Hubs](https://azure.microsoft.com/services/notification-hubs/) ger en förenklad tjänst i hello meningsfullt att behöver du en SDK integrerat i hello mobilappen och du kan ha fullständig kontroll över din mobila app och kan skicka push-meddelanden med grundläggande funktioner för märkning. Det är bra för alla app-ägare som behöver mindre om mål och mer information skickas/kommunicerar direkt tootheir app-användare (broadcast tooa stor befolkning). 
   
   Observera hello fokus här skickas blixtsnabb snabb meddelanden med grundläggande segmentering kapacitet. 

Låt oss ta vissa scenarier:

1. Tim är en del av hello digitala marknadsföringsgruppen i Adventure Works-butiken som publicerar mobilappar för användare. Tims målet är att hello användare som hämtar sina mobila appar fortsätter toouse tooensure den och ofta. Detta inte bara ökar ett varumärke bifoga med hello appanvändare men ökar Monetarisering när hello appanvändare gör inköp med hello mobilappen. För den här Tim måste toosend riktade meddelanden toohello appanvändare, som de användbara, tooencourage dem tooopen hello app och gå tillbaka tooit ofta. Detta är Tim hittar Mobile Engagement användbart. 
2. Joann är en del av hello Utvecklingsteamet av hello mobila appar på Adventure Works och är ute efter en toolog Produktinformation om kraschar, undantag, och hämta prestanda telemetri från en app. Detta är Joann hittar HockeyApp användbart. Joann kan integrera båda HockeyApp för sitt developer fokuserar scenarier och Azure Mobile Engagement för Tim i hello samma app tooget hello bästa av båda. 
3. Robin är en del av hello Utvecklingsteamet hello mobila appar på Nyheter i Contoso-nätverket och alla hon vill är toosend ut bryta nyheter aviseringar tooall användare utan mycket segmentering så snart hello nyheter avisering utlöses. Detta är Robin hittar Meddelandehubbar användbart. 
   I det här scenariot går det dock att som hello mobila appar utvecklas, det är ett krav toodo mycket bättre segmentering och få information om hello app användarens beteende. För tillfället har Robin tooevaluate Azure Mobile Engagement. 

toorecap, hello syftet med Mobile Engagement är inte bara toocollect analytics - inte ”ännu en Analytics produkt från Microsoft”. Det handlar om att skicka riktade push-meddelanden och för den här riktad vi samlar in beteendeanalys data men förblir hello fokus skicka push-meddelanden som att Hej de flesta meningsfullt toohello appanvändare så att det inte kommer som skräppost. 

Mer information – ta en titt på den här [kort video](mobile-engagement-overview.md) om Mobile Engagement i en nutshell. 

