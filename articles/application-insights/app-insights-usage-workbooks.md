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
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a><span data-ttu-id="0620d-103">Undersöka och dela användningsdata med interaktiva arbetsböcker i Application Insights</span><span class="sxs-lookup"><span data-stu-id="0620d-103">Investigate and share usage data with interactive workbooks in Application Insights</span></span>

<span data-ttu-id="0620d-104">Kombinera arbetsböcker [Azure Application Insights](app-insights-overview.md) datavisualiseringar, [Analytics-frågor](app-insights-analytics.md), och text i interaktiva dokument.</span><span class="sxs-lookup"><span data-stu-id="0620d-104">Workbooks combine [Azure Application Insights](app-insights-overview.md) data visualizations, [Analytics queries](app-insights-analytics.md), and text into interactive documents.</span></span> <span data-ttu-id="0620d-105">Arbetsböcker som kan redigeras av andra användare med åtkomst toohello samma Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="0620d-105">Workbooks are editable by other team members with access toohello same Azure resource.</span></span> <span data-ttu-id="0620d-106">Detta innebär att hello frågor och kontroller används toocreate en arbetsbok är tillgängliga tooother personer som läser hello arbetsboken så att de enkelt tooexplore, utöka och Sök efter fel.</span><span class="sxs-lookup"><span data-stu-id="0620d-106">This means hello queries and controls used toocreate a workbook are available tooother people reading hello workbook, making them easy tooexplore, extend, and check for mistakes.</span></span>

<span data-ttu-id="0620d-107">Arbetsböcker som är användbara för scenarier som:</span><span class="sxs-lookup"><span data-stu-id="0620d-107">Workbooks are helpful for scenarios like:</span></span>

* <span data-ttu-id="0620d-108">Utforska hello användning av din app när du inte vet hello mätvärden för intresse i förväg: antal användare, kvarhållningsgrad, konvertering priser och så vidare. Till skillnad från andra analytics-verktyg för användning i Application Insights kan arbetsböcker du kombinera flera typer av grafik och analyser, gör dem utmärkt för den här typen av Friform undersökning.</span><span class="sxs-lookup"><span data-stu-id="0620d-108">Exploring hello usage of your app when you don't know hello metrics of interest in advance: numbers of users, retention rates, conversion rates, etc. Unlike other usage analytics tools in Application Insights, workbooks let you combine multiple kinds of visualizations and analyses, making them great for this kind of free-form exploration.</span></span>
* <span data-ttu-id="0620d-109">Förklarar tooyour team hur en funktion som nyligen utgivna utförs av visar användare antal för viktiga interaktioner och andra mått.</span><span class="sxs-lookup"><span data-stu-id="0620d-109">Explaining tooyour team how a newly released feature is performing, by showing user counts for key interactions and other metrics.</span></span>
* <span data-ttu-id="0620d-110">Dela hello resultaten av en A / B experiment i din app med andra medlemmar i gruppen.</span><span class="sxs-lookup"><span data-stu-id="0620d-110">Sharing hello results of an A/B experiment in your app with other members of your team.</span></span> <span data-ttu-id="0620d-111">Du kan förklara hello mål för hello experimentera med text och sedan visa varje mått för användning och Analytics-fråga används tooevaluate hello experiment, tillsammans med tydliga pratbubblor för om varje mått har över - eller nedan-mål.</span><span class="sxs-lookup"><span data-stu-id="0620d-111">You can explain hello goals for hello experiment with text, then show each usage metric and Analytics query used tooevaluate hello experiment, along with clear call-outs for whether each metric was above- or below-target.</span></span>
* <span data-ttu-id="0620d-112">Rapportering hello effekten av ett avbrott på hello användning av din app genom att kombinera data, text förklaring och en beskrivning av nästa steg tooprevent avbrott i hello framtida.</span><span class="sxs-lookup"><span data-stu-id="0620d-112">Reporting hello impact of an outage on hello usage of your app, combining data, text explanation, and a discussion of next steps tooprevent outages in hello future.</span></span>

> [!NOTE]
> <span data-ttu-id="0620d-113">Application Insights-resursen måste innehålla sidvisningar eller anpassade händelser toouse arbetsböcker.</span><span class="sxs-lookup"><span data-stu-id="0620d-113">Your Application Insights resource must contain page views or custom events toouse workbooks.</span></span> <span data-ttu-id="0620d-114">[Lär dig hur tooset upp appen toocollect sidan visar automatiskt med hello Application Insights JavaScript SDK](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="0620d-114">[Learn how tooset up your app toocollect page views automatically with hello Application Insights JavaScript SDK](app-insights-javascript.md).</span></span>
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a><span data-ttu-id="0620d-115">Redigera, ordna om, kloning och ta bort arbetsboken avsnitt</span><span class="sxs-lookup"><span data-stu-id="0620d-115">Editing, rearranging, cloning, and deleting workbook sections</span></span>

<span data-ttu-id="0620d-116">En arbetsbok är en gjorda av avsnitt: oberoende redigerbara användning visualiseringar, diagram, tabeller, text eller Analytics frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="0620d-116">A workbook is a made of sections: independently editable usage visualizations, charts, tables, text, or Analytics query results.</span></span>

<span data-ttu-id="0620d-117">tooedit hello innehållet i en arbetsbok i avsnittet klickar du på hello **redigera** nedan och toohello höger i hello arbetsboken avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0620d-117">tooedit hello contents of a workbook section, click hello **Edit** button below and toohello right of hello workbook section.</span></span>

![Application Insights arbetsböcker avsnittet redigeringskontroller](./media/app-insights-usage-workbooks/editing-controls.png)

1. <span data-ttu-id="0620d-119">När du är klar redigerar ett avsnitt, klickar du på **klar redigera** i hello nedre vänstra hörnet av hello-avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0620d-119">When you're done editing a section, click **Done Editing** in hello bottom left corner of hello section.</span></span>

2. <span data-ttu-id="0620d-120">toocreate en dubblett av ett avsnitt, klickar du på hello **klona det här avsnittet** ikon.</span><span class="sxs-lookup"><span data-stu-id="0620d-120">toocreate a duplicate of a section, click hello **Clone this section** icon.</span></span> <span data-ttu-id="0620d-121">Att skapa dubbla avsnitt är en bra tooway tooiterate på en fråga utan att förlora tidigare iterationer.</span><span class="sxs-lookup"><span data-stu-id="0620d-121">Creating duplicate sections is a great tooway tooiterate on a query without losing previous iterations.</span></span>

3. <span data-ttu-id="0620d-122">toomove ett avsnitt i en arbetsbok klickar du på hello **Flytta upp** eller **Flytta ned** ikon.</span><span class="sxs-lookup"><span data-stu-id="0620d-122">toomove up a section in a workbook, click hello **Move up** or **Move down** icon.</span></span>

4. <span data-ttu-id="0620d-123">tooremove ett avsnitt permanent, klicka på hello **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="0620d-123">tooremove a section permanently, click hello **Remove** icon.</span></span>

## <a name="adding-usage-data-visualization-sections"></a><span data-ttu-id="0620d-124">Lägga till användning data visualiseringen avsnitt</span><span class="sxs-lookup"><span data-stu-id="0620d-124">Adding usage data visualization sections</span></span>

<span data-ttu-id="0620d-125">Arbetsböcker erbjuder fyra typer av inbyggda användning analytics visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="0620d-125">Workbooks offer four types of built-in usage analytics visualizations.</span></span> <span data-ttu-id="0620d-126">Varje svar på vanliga frågor om hello användning av din app.</span><span class="sxs-lookup"><span data-stu-id="0620d-126">Each answers a common question about hello usage of your app.</span></span> <span data-ttu-id="0620d-127">tooadd tabeller och diagram än dessa fyra avsnitt och lägga till Analytics-fråga (se nedan).</span><span class="sxs-lookup"><span data-stu-id="0620d-127">tooadd tables and charts other than these four sections, add Analytics query sections (see below).</span></span>

<span data-ttu-id="0620d-128">tooadd en användare, sessioner, händelser eller kvarhållning avsnittet tooyour arbetsboken, Använd hello **Lägg till användare** eller andra motsvarande knappen längst ned hello hello arbetsboken eller längst ned hello i ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0620d-128">tooadd a Users, Sessions, Events, or Retention section tooyour workbook, use hello **Add Users** or other corresponding button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

![Avsnittet för användare i arbetsböcker](./media/app-insights-usage-workbooks/users-section.png)

<span data-ttu-id="0620d-130">**Användare** avsnitt besvara ”hur många användare visas en sida eller använda vissa funktion i Min plats”?</span><span class="sxs-lookup"><span data-stu-id="0620d-130">**Users** sections answer "How many users viewed some page or used some feature of my site?"</span></span>

<span data-ttu-id="0620d-131">**Sessioner** avsnitt besvara ”hur många sessioner användarna lägger visar vissa sida eller en funktion i Min webbplats”?</span><span class="sxs-lookup"><span data-stu-id="0620d-131">**Sessions** sections answer "How many sessions did users spend viewing some page or using some feature of my site?"</span></span>

<span data-ttu-id="0620d-132">**Händelser** avsnitt besvara ”hur många gånger användarna visa vissa sidan eller använda vissa funktioner i Min plats”?</span><span class="sxs-lookup"><span data-stu-id="0620d-132">**Events** sections answer "How many times did users view some page or use some feature of my site?"</span></span>

<span data-ttu-id="0620d-133">De olika typerna tre avsnitt ger hello samma uppsättning kontroller och visualiseringar:</span><span class="sxs-lookup"><span data-stu-id="0620d-133">Each of these three section types offers hello same sets of controls and visualizations:</span></span>

* [<span data-ttu-id="0620d-134">Lär dig mer om hur du redigerar avsnitten användare, sessioner och händelser</span><span class="sxs-lookup"><span data-stu-id="0620d-134">Learn more about editing Users, Sessions, and Events sections</span></span>](app-insights-usage-segmentation.md)
* <span data-ttu-id="0620d-135">Växla hello huvuddiagram, histogram rutnät, automatisk insikter och exempel användare visualiseringar med hello **visa diagrammet**, **Visa rutnät**, **visa insikter**, och **Prov för dessa användare** kryssrutorna hello överst i varje avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0620d-135">Toggle hello main chart, histogram grids, automatic insights, and sample users visualizations using hello **Show Chart**, **Show Grid**, **Show Insights**, and **Sample of These Users** checkboxes at hello top of each section.</span></span>

![Kvarhållning avsnitt i arbetsböcker](./media/app-insights-usage-workbooks/retention-section.png)

<span data-ttu-id="0620d-137">**Kvarhållning** avsnitt besvara ”till personer som visade vissa sidan eller vissa funktion som används på en dag eller en vecka, hur många Kom tillbaka senare dag eller vecka”?</span><span class="sxs-lookup"><span data-stu-id="0620d-137">**Retention** sections answer "Of people who viewed some page or used some feature on one day or week, how many came back in a subsequent day or week?"</span></span>

* [<span data-ttu-id="0620d-138">Lär dig mer om hur du redigerar kvarhållning avsnitt</span><span class="sxs-lookup"><span data-stu-id="0620d-138">Learn more about editing Retention sections</span></span>](app-insights-usage-retention.md)
* <span data-ttu-id="0620d-139">Växla hello valfria övergripande kvarhållning diagram med hjälp av hello **visa övergripande kvarhållning diagram** kryssrutan hello överst i hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="0620d-139">Toggle hello optional Overall Retention chart using hello **Show overall retention chart** checkbox at hello top of hello section.</span></span>

## <a name="adding-application-insights-analytics-sections"></a><span data-ttu-id="0620d-140">Lägga till Application Insights Analytics avsnitt</span><span class="sxs-lookup"><span data-stu-id="0620d-140">Adding Application Insights Analytics sections</span></span>

![Analytics-avsnittet i arbetsböcker](./media/app-insights-usage-workbooks/analytics-section.png)

<span data-ttu-id="0620d-142">tooadd en Application Insights Analytics query avsnittet tooyour arbetsboken använder hello **lägga till Analytics query** knappen längst ned hello hello arbetsboken eller längst ned hello i ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0620d-142">tooadd an Application Insights Analytics query section tooyour workbook, use hello **Add Analytics query** button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

<span data-ttu-id="0620d-143">Analytics query avsnitt kan du lägga till valfri frågor över Application Insights-data i arbetsböcker.</span><span class="sxs-lookup"><span data-stu-id="0620d-143">Analytics query sections let you add arbitrary queries over your Application Insights data into workbooks.</span></span> <span data-ttu-id="0620d-144">Den här flexibiliteten innebär Analytics query avsnitt ska vara din gå toofor genom att besvara frågor om din plats än hello fyra som anges ovan för användare, sessioner, händelser och kvarhållning, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="0620d-144">This flexibility means Analytics query sections should be your go-toofor answering any questions about your site other than hello four listed above for Users, Sessions, Events, and Retention, like:</span></span>

* <span data-ttu-id="0620d-145">Hur många undantag har din plats throw under hello samma tidsperioden som en nedgång i användningen?</span><span class="sxs-lookup"><span data-stu-id="0620d-145">How many exceptions did your site throw during hello same time period as a decline in usage?</span></span>
* <span data-ttu-id="0620d-146">Vad hette hello distribution av sidinläsningstider för användare som visar vissa sida?</span><span class="sxs-lookup"><span data-stu-id="0620d-146">What was hello distribution of page load times for users viewing some page?</span></span>
* <span data-ttu-id="0620d-147">Hur många användare visas en uppsättning sidor på webbplatsen, men inte en annan uppsättning sidor?</span><span class="sxs-lookup"><span data-stu-id="0620d-147">How many users viewed some set of pages on your site, but not some other set of pages?</span></span> <span data-ttu-id="0620d-148">Detta kan vara användbart toounderstand om du har kluster för användare som använder olika delar av din webbplats funktioner (använda hello `join` operator med hello `kind=leftanti` modifierare i hello Log Analytics query language).</span><span class="sxs-lookup"><span data-stu-id="0620d-148">This can be useful toounderstand if you have clusters of users who use different subsets of your site's functionality (use hello `join` operator with hello `kind=leftanti` modifier in hello Log Analytics query language).</span></span>

<span data-ttu-id="0620d-149">Använd hello [logganalys fråga Språkreferens](https://docs.loganalytics.io/) toolearn mer om hur du skriver frågor.</span><span class="sxs-lookup"><span data-stu-id="0620d-149">Use hello [Log Analytics query language reference](https://docs.loganalytics.io/) toolearn more about writing queries.</span></span>

## <a name="adding-text-and-markdown-sections"></a><span data-ttu-id="0620d-150">Lägga till text och Markdown-avsnitt</span><span class="sxs-lookup"><span data-stu-id="0620d-150">Adding text and Markdown sections</span></span>

<span data-ttu-id="0620d-151">Lägga till rubriker, beskrivningar och kommentarer tooyour arbetsböcker kan göra en uppsättning tabeller och diagram till en.</span><span class="sxs-lookup"><span data-stu-id="0620d-151">Adding headings, explanations, and commentary tooyour workbooks helps turn a set of tables and charts into a narrative.</span></span> <span data-ttu-id="0620d-152">Delar av texten i arbetsböcker stöder hello [markdownsyntax](https://daringfireball.net/projects/markdown/) för textformatering som rubriker, fetstil, kursiv stil och punktlista.</span><span class="sxs-lookup"><span data-stu-id="0620d-152">Text sections in workbooks support hello [Markdown syntax](https://daringfireball.net/projects/markdown/) for text formatting, like headings, bold, italics, and bulleted lists.</span></span>

<span data-ttu-id="0620d-153">tooadd text avsnittet tooyour arbetsboken använder hello **lägga till text** knappen längst ned hello hello arbetsboken eller längst ned hello i ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="0620d-153">tooadd a text section tooyour workbook, use hello **Add text** button at hello bottom of hello workbook, or at hello bottom of any section.</span></span>

## <a name="saving-and-sharing-workbooks-with-your-team"></a><span data-ttu-id="0620d-154">Spara och dela arbetsböcker med din grupp</span><span class="sxs-lookup"><span data-stu-id="0620d-154">Saving and sharing workbooks with your team</span></span>

<span data-ttu-id="0620d-155">Arbetsböcker som sparas i en Application Insights-resurs, antingen i hello **Mina rapporter** avsnitt som är privata tooyou eller i hello **delade rapporter** avsnitt som är tillgänglig tooeveryone med åtkomst toohello Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="0620d-155">Workbooks are saved within an Application Insights resource, either in hello **My Reports** section that's private tooyou or in hello **Shared Reports** section that's accessible tooeveryone with access toohello Application Insights resource.</span></span> <span data-ttu-id="0620d-156">tooview alla hello arbetsböcker i hello resurs, klicka på hello **öppna** knapp i Åtgärdsfältet hello.</span><span class="sxs-lookup"><span data-stu-id="0620d-156">tooview all hello workbooks in hello resource, click hello **Open** button in hello action bar.</span></span>

<span data-ttu-id="0620d-157">tooshare en arbetsbok som för närvarande i **Mina rapporter**:</span><span class="sxs-lookup"><span data-stu-id="0620d-157">tooshare a workbook that's currently in **My Reports**:</span></span>

1. <span data-ttu-id="0620d-158">Klicka på **öppna** i Åtgärdsfältet hello</span><span class="sxs-lookup"><span data-stu-id="0620d-158">Click **Open** in hello action bar</span></span>
2. <span data-ttu-id="0620d-159">Klicka på knappen ”...” hello bredvid hello arbetsboken du vill tooshare</span><span class="sxs-lookup"><span data-stu-id="0620d-159">Click hello "..." button beside hello workbook you want tooshare</span></span>
3. <span data-ttu-id="0620d-160">Klicka på **flytta tooShared rapporter**.</span><span class="sxs-lookup"><span data-stu-id="0620d-160">Click **Move tooShared Reports**.</span></span>

<span data-ttu-id="0620d-161">tooshare en arbetsbok med en länk eller via e-post, klickar du på **resursen** i Åtgärdsfältet hello.</span><span class="sxs-lookup"><span data-stu-id="0620d-161">tooshare a workbook with a link or via email, click **Share** in hello action bar.</span></span> <span data-ttu-id="0620d-162">Tänk på att mottagarna av hello länk behöver åtkomst till toothis resurs i hello Azure portal tooview hello arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="0620d-162">Keep in mind that recipients of hello link need access toothis resource in hello Azure portal tooview hello workbook.</span></span> <span data-ttu-id="0620d-163">toomake redigering, måste du använda minst bidragsgivarbehörighet för hello resurs.</span><span class="sxs-lookup"><span data-stu-id="0620d-163">toomake edits, recipients need at least Contributor permissions for hello resource.</span></span>

<span data-ttu-id="0620d-164">toopin en länk tooa arbetsboken tooan Azure instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="0620d-164">toopin a link tooa workbook tooan Azure Dashboard:</span></span>

1. <span data-ttu-id="0620d-165">Klicka på **öppna** i Åtgärdsfältet hello</span><span class="sxs-lookup"><span data-stu-id="0620d-165">Click **Open** in hello action bar</span></span>
2. <span data-ttu-id="0620d-166">Klicka på knappen ”...” hello bredvid hello arbetsboken du vill toopin</span><span class="sxs-lookup"><span data-stu-id="0620d-166">Click hello "..." button beside hello workbook you want toopin</span></span>
3. <span data-ttu-id="0620d-167">Klicka på **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="0620d-167">Click **Pin toodashboard**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0620d-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0620d-168">Next steps</span></span>

## <a name="next-steps"></a><span data-ttu-id="0620d-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="0620d-169">Next steps</span></span>
- <span data-ttu-id="0620d-170">tooenable användning inträffar, börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="0620d-170">tooenable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="0620d-171">Om du redan skickar anpassade händelser eller sidvyer utforska hello användning verktyg toolearn hur användare att använda tjänsten.</span><span class="sxs-lookup"><span data-stu-id="0620d-171">If you already send custom events or page views, explore hello Usage tools toolearn how users use your service.</span></span>
    - [<span data-ttu-id="0620d-172">Användare, sessioner, händelser</span><span class="sxs-lookup"><span data-stu-id="0620d-172">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="0620d-173">Trattar</span><span class="sxs-lookup"><span data-stu-id="0620d-173">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="0620d-174">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="0620d-174">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="0620d-175">Användarflöden</span><span class="sxs-lookup"><span data-stu-id="0620d-175">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="0620d-176">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="0620d-176">Add user context</span></span>](app-insights-usage-send-user-context.md)
    
