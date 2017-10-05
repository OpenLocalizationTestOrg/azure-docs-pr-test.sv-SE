---
title: "Undersöka och dela användningsdata med interaktiva arbetsböcker i Azure Application Insights | Microsoft docs"
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
ms.openlocfilehash: 75028b4fbda43d90f56690a33c7eb624fce049c8
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="investigate-and-share-usage-data-with-interactive-workbooks-in-application-insights"></a><span data-ttu-id="8d3fc-103">Undersöka och dela användningsdata med interaktiva arbetsböcker i Application Insights</span><span class="sxs-lookup"><span data-stu-id="8d3fc-103">Investigate and share usage data with interactive workbooks in Application Insights</span></span>

<span data-ttu-id="8d3fc-104">Kombinera arbetsböcker [Azure Application Insights](app-insights-overview.md) datavisualiseringar, [Analytics-frågor](app-insights-analytics.md), och text i interaktiva dokument.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-104">Workbooks combine [Azure Application Insights](app-insights-overview.md) data visualizations, [Analytics queries](app-insights-analytics.md), and text into interactive documents.</span></span> <span data-ttu-id="8d3fc-105">Arbetsböcker som kan redigeras av andra användare med åtkomst till samma Azure-resurs.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-105">Workbooks are editable by other team members with access to the same Azure resource.</span></span> <span data-ttu-id="8d3fc-106">Det innebär att frågorna och kontroller som används för att skapa en arbetsbok som är tillgängliga för andra personer som läser arbetsboken, gör dem lättare att utforska, utöka och Sök efter fel.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-106">This means the queries and controls used to create a workbook are available to other people reading the workbook, making them easy to explore, extend, and check for mistakes.</span></span>

<span data-ttu-id="8d3fc-107">Arbetsböcker som är användbara för scenarier som:</span><span class="sxs-lookup"><span data-stu-id="8d3fc-107">Workbooks are helpful for scenarios like:</span></span>

* <span data-ttu-id="8d3fc-108">Utforska användningen av din app när du inte vet mätvärdena som är av intresse i förväg: antal användare, kvarhållningsgrad, konvertering priser och så vidare. Till skillnad från andra analytics-verktyg för användning i Application Insights kan arbetsböcker du kombinera flera typer av grafik och analyser, gör dem utmärkt för den här typen av Friform undersökning.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-108">Exploring the usage of your app when you don't know the metrics of interest in advance: numbers of users, retention rates, conversion rates, etc. Unlike other usage analytics tools in Application Insights, workbooks let you combine multiple kinds of visualizations and analyses, making them great for this kind of free-form exploration.</span></span>
* <span data-ttu-id="8d3fc-109">Förklarar hur en funktion som nyligen utgivna utförs för gruppen, som visar användaren antal för viktiga interaktioner och andra mått.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-109">Explaining to your team how a newly released feature is performing, by showing user counts for key interactions and other metrics.</span></span>
* <span data-ttu-id="8d3fc-110">Dela resultatet av en A / B experiment i din app med andra medlemmar i gruppen.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-110">Sharing the results of an A/B experiment in your app with other members of your team.</span></span> <span data-ttu-id="8d3fc-111">Du kan förklarar syftet med experiment med text och sedan visa varje användning mått och Analytics-fråga som används för att utvärdera experiment, tillsammans med tydliga pratbubblor för om varje mått har över - eller nedan-mål.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-111">You can explain the goals for the experiment with text, then show each usage metric and Analytics query used to evaluate the experiment, along with clear call-outs for whether each metric was above- or below-target.</span></span>
* <span data-ttu-id="8d3fc-112">Rapportering effekten av ett avbrott på användningen av din app genom att kombinera data, text förklaring och en beskrivning av nästa steg för att förhindra avbrott i framtiden.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-112">Reporting the impact of an outage on the usage of your app, combining data, text explanation, and a discussion of next steps to prevent outages in the future.</span></span>

> [!NOTE]
> <span data-ttu-id="8d3fc-113">Application Insights-resursen måste innehålla sidvisningar eller anpassade händelser att använda arbetsböcker.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-113">Your Application Insights resource must contain page views or custom events to use workbooks.</span></span> <span data-ttu-id="8d3fc-114">[Lär dig att konfigurera din app att samla in sidvisningar automatiskt med Application Insights JavaScript SDK](app-insights-javascript.md).</span><span class="sxs-lookup"><span data-stu-id="8d3fc-114">[Learn how to set up your app to collect page views automatically with the Application Insights JavaScript SDK](app-insights-javascript.md).</span></span>
> 
> 

## <a name="editing-rearranging-cloning-and-deleting-workbook-sections"></a><span data-ttu-id="8d3fc-115">Redigera, ordna om, kloning och ta bort arbetsboken avsnitt</span><span class="sxs-lookup"><span data-stu-id="8d3fc-115">Editing, rearranging, cloning, and deleting workbook sections</span></span>

<span data-ttu-id="8d3fc-116">En arbetsbok är en gjorda av avsnitt: oberoende redigerbara användning visualiseringar, diagram, tabeller, text eller Analytics frågeresultat.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-116">A workbook is a made of sections: independently editable usage visualizations, charts, tables, text, or Analytics query results.</span></span>

<span data-ttu-id="8d3fc-117">Om du vill redigera innehållet i en arbetsbok-avsnittet, klickar du på den **redigera** knappen nedanför och till höger om avsnittet arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-117">To edit the contents of a workbook section, click the **Edit** button below and to the right of the workbook section.</span></span>

![Application Insights arbetsböcker avsnittet redigeringskontroller](./media/app-insights-usage-workbooks/editing-controls.png)

1. <span data-ttu-id="8d3fc-119">När du är klar redigerar ett avsnitt, klickar du på **klar redigera** i det nedre vänstra hörnet i avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-119">When you're done editing a section, click **Done Editing** in the bottom left corner of the section.</span></span>

2. <span data-ttu-id="8d3fc-120">Om du vill skapa en dubblett av ett avsnitt, klickar du på den **klona det här avsnittet** ikon.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-120">To create a duplicate of a section, click the **Clone this section** icon.</span></span> <span data-ttu-id="8d3fc-121">Att skapa dubbla avsnitt är ett bra sätt att iterera på en fråga utan att förlora tidigare iterationer.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-121">Creating duplicate sections is a great to way to iterate on a query without losing previous iterations.</span></span>

3. <span data-ttu-id="8d3fc-122">Om du vill flytta upp ett avsnitt i en arbetsbok klickar du på den **Flytta upp** eller **Flytta ned** ikon.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-122">To move up a section in a workbook, click the **Move up** or **Move down** icon.</span></span>

4. <span data-ttu-id="8d3fc-123">Om du vill ta bort ett avsnitt permanent klickar du på den **ta bort** ikon.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-123">To remove a section permanently, click the **Remove** icon.</span></span>

## <a name="adding-usage-data-visualization-sections"></a><span data-ttu-id="8d3fc-124">Lägga till användning data visualiseringen avsnitt</span><span class="sxs-lookup"><span data-stu-id="8d3fc-124">Adding usage data visualization sections</span></span>

<span data-ttu-id="8d3fc-125">Arbetsböcker erbjuder fyra typer av inbyggda användning analytics visualiseringar.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-125">Workbooks offer four types of built-in usage analytics visualizations.</span></span> <span data-ttu-id="8d3fc-126">Varje svar på vanliga frågor om användningen av din app.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-126">Each answers a common question about the usage of your app.</span></span> <span data-ttu-id="8d3fc-127">Lägg till Analytics query avsnitt (se nedan) för att lägga till tabeller och diagram än följande fyra avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-127">To add tables and charts other than these four sections, add Analytics query sections (see below).</span></span>

<span data-ttu-id="8d3fc-128">Om du vill lägga till en användare, sessioner, händelser eller kvarhållning avsnittet till din arbetsbok, Använd den **Lägg till användare** eller andra motsvarande knappen längst ned i arbetsboken eller längst ned i ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-128">To add a Users, Sessions, Events, or Retention section to your workbook, use the **Add Users** or other corresponding button at the bottom of the workbook, or at the bottom of any section.</span></span>

![Avsnittet för användare i arbetsböcker](./media/app-insights-usage-workbooks/users-section.png)

<span data-ttu-id="8d3fc-130">**Användare** avsnitt besvara ”hur många användare visas en sida eller använda vissa funktion i Min plats”?</span><span class="sxs-lookup"><span data-stu-id="8d3fc-130">**Users** sections answer "How many users viewed some page or used some feature of my site?"</span></span>

<span data-ttu-id="8d3fc-131">**Sessioner** avsnitt besvara ”hur många sessioner användarna lägger visar vissa sida eller en funktion i Min webbplats”?</span><span class="sxs-lookup"><span data-stu-id="8d3fc-131">**Sessions** sections answer "How many sessions did users spend viewing some page or using some feature of my site?"</span></span>

<span data-ttu-id="8d3fc-132">**Händelser** avsnitt besvara ”hur många gånger användarna visa vissa sidan eller använda vissa funktioner i Min plats”?</span><span class="sxs-lookup"><span data-stu-id="8d3fc-132">**Events** sections answer "How many times did users view some page or use some feature of my site?"</span></span>

<span data-ttu-id="8d3fc-133">De olika typerna tre avsnitt har samma uppsättning kontroller och visualiseringar:</span><span class="sxs-lookup"><span data-stu-id="8d3fc-133">Each of these three section types offers the same sets of controls and visualizations:</span></span>

* [<span data-ttu-id="8d3fc-134">Lär dig mer om hur du redigerar avsnitten användare, sessioner och händelser</span><span class="sxs-lookup"><span data-stu-id="8d3fc-134">Learn more about editing Users, Sessions, and Events sections</span></span>](app-insights-usage-segmentation.md)
* <span data-ttu-id="8d3fc-135">Växla huvuddiagram, histogram rutnät, automatisk insikter och exempel användare visualiseringar med hjälp av den **visa diagrammet**, **Visa rutnät**, **visa insikter**, och **exempel för dessa användare** kryssrutorna längst upp i varje avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-135">Toggle the main chart, histogram grids, automatic insights, and sample users visualizations using the **Show Chart**, **Show Grid**, **Show Insights**, and **Sample of These Users** checkboxes at the top of each section.</span></span>

![Kvarhållning avsnitt i arbetsböcker](./media/app-insights-usage-workbooks/retention-section.png)

<span data-ttu-id="8d3fc-137">**Kvarhållning** avsnitt besvara ”till personer som visade vissa sidan eller vissa funktion som används på en dag eller en vecka, hur många Kom tillbaka senare dag eller vecka”?</span><span class="sxs-lookup"><span data-stu-id="8d3fc-137">**Retention** sections answer "Of people who viewed some page or used some feature on one day or week, how many came back in a subsequent day or week?"</span></span>

* [<span data-ttu-id="8d3fc-138">Lär dig mer om hur du redigerar kvarhållning avsnitt</span><span class="sxs-lookup"><span data-stu-id="8d3fc-138">Learn more about editing Retention sections</span></span>](app-insights-usage-retention.md)
* <span data-ttu-id="8d3fc-139">Växla den valfria övergripande kvarhållning diagram med hjälp av den **visa övergripande kvarhållning diagram** kryssrutan längst upp i avsnittet.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-139">Toggle the optional Overall Retention chart using the **Show overall retention chart** checkbox at the top of the section.</span></span>

## <a name="adding-application-insights-analytics-sections"></a><span data-ttu-id="8d3fc-140">Lägga till Application Insights Analytics avsnitt</span><span class="sxs-lookup"><span data-stu-id="8d3fc-140">Adding Application Insights Analytics sections</span></span>

![Analytics-avsnittet i arbetsböcker](./media/app-insights-usage-workbooks/analytics-section.png)

<span data-ttu-id="8d3fc-142">Om du vill lägga till ett avsnitt för Application Insights Analytics-fråga till din arbetsbok, Använd den **lägga till Analytics query** knappen längst ned i arbetsboken eller längst ned i ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-142">To add an Application Insights Analytics query section to your workbook, use the **Add Analytics query** button at the bottom of the workbook, or at the bottom of any section.</span></span>

<span data-ttu-id="8d3fc-143">Analytics query avsnitt kan du lägga till valfri frågor över Application Insights-data i arbetsböcker.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-143">Analytics query sections let you add arbitrary queries over your Application Insights data into workbooks.</span></span> <span data-ttu-id="8d3fc-144">Den här flexibiliteten innebär Analytics query avsnitt ska vara din gå till för att svara på frågor om din plats än fyra som anges ovan för användare, sessioner, händelser och kvarhållning, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="8d3fc-144">This flexibility means Analytics query sections should be your go-to for answering any questions about your site other than the four listed above for Users, Sessions, Events, and Retention, like:</span></span>

* <span data-ttu-id="8d3fc-145">Hur många undantag webbplatsen utlösa under samma tid som en nedgång i användningen?</span><span class="sxs-lookup"><span data-stu-id="8d3fc-145">How many exceptions did your site throw during the same time period as a decline in usage?</span></span>
* <span data-ttu-id="8d3fc-146">Vad hette sidinläsningstider för användare som visar vissa sida distributionen?</span><span class="sxs-lookup"><span data-stu-id="8d3fc-146">What was the distribution of page load times for users viewing some page?</span></span>
* <span data-ttu-id="8d3fc-147">Hur många användare visas en uppsättning sidor på webbplatsen, men inte en annan uppsättning sidor?</span><span class="sxs-lookup"><span data-stu-id="8d3fc-147">How many users viewed some set of pages on your site, but not some other set of pages?</span></span> <span data-ttu-id="8d3fc-148">Detta kan vara användbar för att förstå om du har kluster av användare som använder olika delar av din webbplats funktioner (använda den `join` operator med det `kind=leftanti` modifierare i Log Analytics-frågespråket).</span><span class="sxs-lookup"><span data-stu-id="8d3fc-148">This can be useful to understand if you have clusters of users who use different subsets of your site's functionality (use the `join` operator with the `kind=leftanti` modifier in the Log Analytics query language).</span></span>

<span data-ttu-id="8d3fc-149">Använd den [logganalys fråga Språkreferens](https://docs.loganalytics.io/) vill veta mer om hur du skriver frågor.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-149">Use the [Log Analytics query language reference](https://docs.loganalytics.io/) to learn more about writing queries.</span></span>

## <a name="adding-text-and-markdown-sections"></a><span data-ttu-id="8d3fc-150">Lägga till text och Markdown-avsnitt</span><span class="sxs-lookup"><span data-stu-id="8d3fc-150">Adding text and Markdown sections</span></span>

<span data-ttu-id="8d3fc-151">Lägga till rubriker, beskrivningar och kommentarer i arbetsböckerna aktivera hjälper till att en uppsättning tabeller och diagram i en.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-151">Adding headings, explanations, and commentary to your workbooks helps turn a set of tables and charts into a narrative.</span></span> <span data-ttu-id="8d3fc-152">Delar av texten i stödet för arbetsböcker i [markdownsyntax](https://daringfireball.net/projects/markdown/) för textformatering som rubriker, fetstil, kursiv stil och punktlista.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-152">Text sections in workbooks support the [Markdown syntax](https://daringfireball.net/projects/markdown/) for text formatting, like headings, bold, italics, and bulleted lists.</span></span>

<span data-ttu-id="8d3fc-153">Lägg till ett textavsnitt i din arbetsbok, använda den **lägga till text** knappen längst ned i arbetsboken eller längst ned i ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-153">To add a text section to your workbook, use the **Add text** button at the bottom of the workbook, or at the bottom of any section.</span></span>

## <a name="saving-and-sharing-workbooks-with-your-team"></a><span data-ttu-id="8d3fc-154">Spara och dela arbetsböcker med din grupp</span><span class="sxs-lookup"><span data-stu-id="8d3fc-154">Saving and sharing workbooks with your team</span></span>

<span data-ttu-id="8d3fc-155">Arbetsböcker sparas i Application Insights-resurs i antingen den **Mina rapporter** avsnitt som är privat dig eller i den **delade rapporter** avsnitt som är tillgänglig för alla med åtkomst till Application Insights-resursen.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-155">Workbooks are saved within an Application Insights resource, either in the **My Reports** section that's private to you or in the **Shared Reports** section that's accessible to everyone with access to the Application Insights resource.</span></span> <span data-ttu-id="8d3fc-156">Visa alla arbetsböcker i resursen, klicka på den **öppna** knappen i åtgärdsfältet.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-156">To view all the workbooks in the resource, click the **Open** button in the action bar.</span></span>

<span data-ttu-id="8d3fc-157">Dela en arbetsbok som för närvarande i **Mina rapporter**:</span><span class="sxs-lookup"><span data-stu-id="8d3fc-157">To share a workbook that's currently in **My Reports**:</span></span>

1. <span data-ttu-id="8d3fc-158">Klicka på **öppna** i Åtgärdsfältet</span><span class="sxs-lookup"><span data-stu-id="8d3fc-158">Click **Open** in the action bar</span></span>
2. <span data-ttu-id="8d3fc-159">Klicka på knappen ”...” bredvid arbetsboken som du vill dela</span><span class="sxs-lookup"><span data-stu-id="8d3fc-159">Click the "..." button beside the workbook you want to share</span></span>
3. <span data-ttu-id="8d3fc-160">Klicka på **flytta till delade rapporter**.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-160">Click **Move to Shared Reports**.</span></span>

<span data-ttu-id="8d3fc-161">Om du vill dela en arbetsbok med en länk eller via e-post, klickar du på **dela** i åtgärdsfältet.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-161">To share a workbook with a link or via email, click **Share** in the action bar.</span></span> <span data-ttu-id="8d3fc-162">Tänk på att mottagarna av länken behöver åtkomst till den här resursen i Azure portal för att visa arbetsboken.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-162">Keep in mind that recipients of the link need access to this resource in the Azure portal to view the workbook.</span></span> <span data-ttu-id="8d3fc-163">Om du vill göra ändringar, måste du använda minst bidragsgivarbehörighet för resursen.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-163">To make edits, recipients need at least Contributor permissions for the resource.</span></span>

<span data-ttu-id="8d3fc-164">Så här fäster du en länk till en arbetsbok i en Azure-instrumentpanel:</span><span class="sxs-lookup"><span data-stu-id="8d3fc-164">To pin a link to a workbook to an Azure Dashboard:</span></span>

1. <span data-ttu-id="8d3fc-165">Klicka på **öppna** i Åtgärdsfältet</span><span class="sxs-lookup"><span data-stu-id="8d3fc-165">Click **Open** in the action bar</span></span>
2. <span data-ttu-id="8d3fc-166">Klicka på knappen ”...” bredvid arbetsboken som du vill fästa</span><span class="sxs-lookup"><span data-stu-id="8d3fc-166">Click the "..." button beside the workbook you want to pin</span></span>
3. <span data-ttu-id="8d3fc-167">Klicka på **fäst på instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-167">Click **Pin to dashboard**.</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d3fc-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d3fc-168">Next steps</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d3fc-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d3fc-169">Next steps</span></span>
- <span data-ttu-id="8d3fc-170">Om du vill aktivera användning upplevelser börja skicka [anpassade händelser](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) eller [sidvisningar](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span><span class="sxs-lookup"><span data-stu-id="8d3fc-170">To enable usage experiences, start sending [custom events](https://docs.microsoft.com/en-us/azure/application-insights/app-insights-api-custom-events-metrics#trackevent) or [page views](https://docs.microsoft.com/azure/application-insights/app-insights-api-custom-events-metrics#page-views).</span></span>
- <span data-ttu-id="8d3fc-171">Om du skickar redan anpassade händelser eller sidvisningar, utforska verktygen användning om du vill veta hur användarna använder din tjänst.</span><span class="sxs-lookup"><span data-stu-id="8d3fc-171">If you already send custom events or page views, explore the Usage tools to learn how users use your service.</span></span>
    - [<span data-ttu-id="8d3fc-172">Användare, sessioner, händelser</span><span class="sxs-lookup"><span data-stu-id="8d3fc-172">Users, Sessions, Events</span></span>](app-insights-usage-segmentation.md)
    - [<span data-ttu-id="8d3fc-173">Trattar</span><span class="sxs-lookup"><span data-stu-id="8d3fc-173">Funnels</span></span>](usage-funnels.md)
    - [<span data-ttu-id="8d3fc-174">Kvarhållning</span><span class="sxs-lookup"><span data-stu-id="8d3fc-174">Retention</span></span>](app-insights-usage-retention.md)
    - [<span data-ttu-id="8d3fc-175">Användarflöden</span><span class="sxs-lookup"><span data-stu-id="8d3fc-175">User Flows</span></span>](app-insights-usage-flows.md)
    - [<span data-ttu-id="8d3fc-176">Lägg till användarkontext</span><span class="sxs-lookup"><span data-stu-id="8d3fc-176">Add user context</span></span>](app-insights-usage-send-user-context.md)
    
