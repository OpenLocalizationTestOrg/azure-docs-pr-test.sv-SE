---
title: "aaaInvestigate och dela användningsdata med interaktiva arbetsböcker i Azure Application Insights | Microsoft docs"
description: "Demografisk analys av användare av ditt webbprogram."
services: application-insights
documentationcenter: 
author: numberbycolors
manager: carmonm
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: multiple
ms.topic: article
ms.date: 06/12/2017
ms.author: bwren
ms.openlocfilehash: bdcebe0f97fdad0a0b301df5950dc09698f5a4dd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a>Undersöka och dela användningsdata med interaktiva arbetsböcker i Application Insights

Kombinera arbetsböcker [Azure Application Insights](app-insights-overview.md) datavisualiseringar, [Analytics-frågor](app-insights-analytics.md), och text i interaktiva dokument. Arbetsböcker som kan redigeras av andra användare med åtkomst toohello samma Azure-resurs. Detta innebär att hello frågor och kontroller används toocreate en arbetsbok är tillgängliga tooother personer som läser hello arbetsboken så att de enkelt tooexplore, utöka och Sök efter fel.

Arbetsböcker som är användbara för scenarier som:

* Utforska hello användning av din app när du inte vet hello mätvärden för intresse i förväg: antal användare, kvarhållningsgrad, konvertering priser och så vidare. Till skillnad från andra analytics-verktyg för användning i Application Insights kan arbetsböcker du kombinera flera typer av grafik och analyser, gör dem utmärkt för den här typen av Friform undersökning.
* Förklarar tooyour team hur en funktion som nyligen utgivna utförs av visar användare antal för viktiga interaktioner och andra mått.
* Dela hello resultaten av en A / B experiment i din app med andra medlemmar i gruppen. Du kan förklara hello mål för hello experimentera med text och sedan visa varje mått för användning och Analytics-fråga används tooevaluate hello experiment, tillsammans med tydliga pratbubblor för om varje mått har över - eller nedan-mål.
* Rapportering hello effekten av ett avbrott på hello användning av din app genom att kombinera data, text förklaring och en beskrivning av nästa steg tooprevent avbrott i hello framtida.

> [!NOTE]
> Application Insights-resursen måste innehålla sidvisningar eller anpassade händelser toouse arbetsböcker. [Lär dig hur tooset upp appen toocollect sidan visar automatiskt med hello Application Insights JavaScript SDK](app-insights-javascript.md).
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a>Redigera, ordna om, kloning och ta bort arbetsboken avsnitt

En arbetsbok är en gjorda av avsnitt: oberoende redigerbara användning visualiseringar, diagram, tabeller, text eller Analytics frågeresultat.

tooedit hello innehållet i en arbetsbok i avsnittet klickar du på hello **redigera** nedan och toohello höger i hello arbetsboken avsnitt.

![Application Insights arbetsböcker avsnittet redigeringskontroller](./media/app-insights-usage-workbooks/editing-controls.png)

1. När du är klar redigerar ett avsnitt, klickar du på **klar redigera** i hello nedre vänstra hörnet av hello-avsnitt.

2. toocreate en dubblett av ett avsnitt, klickar du på hello **klona det här avsnittet** ikon. Att skapa dubbla avsnitt är en bra tooway tooiterate på en fråga utan att förlora tidigare iterationer.

3. toomove ett avsnitt i en arbetsbok klickar du på hello **Flytta upp** eller **Flytta ned** ikon.

4. tooremove ett avsnitt permanent, klicka på hello **ta bort** ikon.

## <a name="adding-usage-data-visualization-sections"></a>Lägga till användning data visualiseringen avsnitt

Arbetsböcker erbjuder fyra typer av inbyggda användning analytics visualiseringar. Varje svar på vanliga frågor om hello användning av din app. tooadd tabeller och diagram än dessa fyra avsnitt och lägga till Analytics-fråga (se nedan).

tooadd en användare, sessioner, händelser eller kvarhållning avsnittet tooyour arbetsboken, Använd hello **Lägg till användare** eller andra motsvarande knappen längst ned hello hello arbetsboken eller längst ned hello i ett avsnitt.

![Avsnittet för användare i arbetsböcker](./media/app-insights-usage-workbooks/users-section.png)

**Användare** avsnitt besvara ”hur många användare visas en sida eller använda vissa funktion i Min plats”?

**Sessioner** avsnitt besvara ”hur många sessioner användarna lägger visar vissa sida eller en funktion i Min webbplats”?

**Händelser** avsnitt besvara ”hur många gånger användarna visa vissa sidan eller använda vissa funktioner i Min plats”?

De olika typerna tre avsnitt ger hello samma uppsättning kontroller och visualiseringar:

* [Lär dig mer om hur du redigerar avsnitten användare, sessioner och händelser](app-insights-usage-segmentation.md)
* Växla hello huvuddiagram, histogram rutnät, automatisk insikter och exempel användare visualiseringar med hello **visa diagrammet**, **Visa rutnät**, **visa insikter**, och **Prov för dessa användare** kryssrutorna hello överst i varje avsnitt.

![Kvarhållning avsnitt i arbetsböcker](./media/app-insights-usage-workbooks/retention-section.png)

**Kvarhållning** avsnitt besvara ”till personer som visade vissa sidan eller vissa funktion som används på en dag eller en vecka, hur många Kom tillbaka senare dag eller vecka”?

* [Lär dig mer om hur du redigerar kvarhållning avsnitt](app-insights-usage-retention.md)
* Växla hello valfria övergripande kvarhållning diagram med hjälp av hello **visa övergripande kvarhållning diagram** kryssrutan hello överst i hello-avsnittet.

## <a name="adding-application-insights-analytics-sections"></a>Lägga till Application Insights Analytics avsnitt

![Analytics-avsnittet i arbetsböcker](./media/app-insights-usage-workbooks/analytics-section.png)

tooadd en Application Insights Analytics query avsnittet tooyour arbetsboken använder hello **lägga till Analytics query** knappen längst ned hello hello arbetsboken eller längst ned hello i ett avsnitt.

Analytics query avsnitt kan du lägga till valfri frågor över Application Insights-data i arbetsböcker. Den här flexibiliteten innebär Analytics query avsnitt ska vara din gå toofor genom att besvara frågor om din plats än hello fyra som anges ovan för användare, sessioner, händelser och kvarhållning, t.ex.:

* Hur många undantag har din plats throw under hello samma tidsperioden som en nedgång i användningen?
* Vad hette hello distribution av sidinläsningstider för användare som visar vissa sida?
* Hur många användare visas en uppsättning sidor på webbplatsen, men inte en annan uppsättning sidor? Detta kan vara användbart toounderstand om du har kluster för användare som använder olika delar av din webbplats funktioner (använda hello `join` operator med hello `kind=leftanti` modifierare i hello Log Analytics query language).

Använd hello [logganalys fråga Språkreferens](https://docs.loganalytics.io/) toolearn mer om hur du skriver frågor.

## <a name="adding-text-and-markdown-sections"></a>Lägga till text och Markdown-avsnitt

Lägga till rubriker, beskrivningar och kommentarer tooyour arbetsböcker kan göra en uppsättning tabeller och diagram till en. Delar av texten i arbetsböcker stöder hello [markdownsyntax](https://daringfireball.net/projects/markdown/) för textformatering som rubriker, fetstil, kursiv stil och punktlista.

tooadd text avsnittet tooyour arbetsboken använder hello **lägga till text** knappen längst ned hello hello arbetsboken eller längst ned hello i ett avsnitt.

## <a name="saving-and-sharing-workbooks-with-your-team"></a>Spara och dela arbetsböcker med din grupp

Arbetsböcker som sparas i en Application Insights-resurs, antingen i hello **Mina rapporter** avsnitt som är privata tooyou eller i hello **delade rapporter** avsnitt som är tillgänglig tooeveryone med åtkomst toohello Application Insights-resurs. tooview alla hello arbetsböcker i hello resurs, klicka på hello **öppna** knapp i Åtgärdsfältet hello.

tooshare en arbetsbok som för närvarande i **Mina rapporter**:

1. Klicka på **öppna** i Åtgärdsfältet hello
2. Klicka på knappen ”...” hello bredvid hello arbetsboken du vill tooshare
3. Klicka på **flytta tooShared rapporter**.

tooshare en arbetsbok med en länk eller via e-post, klickar du på **resursen** i Åtgärdsfältet hello. Tänk på att mottagarna av hello länk behöver åtkomst till toothis resurs i hello Azure portal tooview hello arbetsboken. toomake redigering, måste du använda minst bidragsgivarbehörighet för hello resurs.

toopin en länk tooa arbetsboken tooan Azure instrumentpanelen:

1. Klicka på **öppna** i Åtgärdsfältet hello
2. Klicka på knappen ”...” hello bredvid hello arbetsboken du vill toopin
3. Klicka på **PIN-kod toodashboard**.

## <a name="next-steps"></a>Nästa steg

## <a name="next-steps"></a>Nästa steg
- tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).
- Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.
    - [Användare, sessioner, händelser](app-insights-usage-segmentation.md)
    - [Trattar](usage-funnels.md)
    - [Kvarhållning](app-insights-usage-retention.md)
    - [Användarflöden](app-insights-usage-flows.md)
    - [Lägg till användarkontext](app-insights-usage-send-user-context.md)
    
