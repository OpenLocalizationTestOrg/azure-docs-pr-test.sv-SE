---
title: aaaMicrosoft Dynamics CRM och Azure Application Insights | Microsoft Docs
description: "Hämta telemetri från Microsoft Dynamics CRM Online med hjälp av Application Insights. Genomgång av installationen, som hämtar data, visualisering och export."
services: application-insights
documentationcenter: 
author: mazharmicrosoft
manager: carmonm
ms.assetid: 04c66338-687e-49e5-9975-be935f98f156
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 04/16/2017
ms.author: bwren
ms.openlocfilehash: a39398060d6553fb18a26c101f085b7d87443636
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="walkthrough-enabling-telemetry-for-microsoft-dynamics-crm-online-using-application-insights"></a><span data-ttu-id="fedaf-104">Genomgång: Aktivera telemetri för Microsoft Dynamics CRM Online med hjälp av Application Insights</span><span class="sxs-lookup"><span data-stu-id="fedaf-104">Walkthrough: Enabling Telemetry for Microsoft Dynamics CRM Online using Application Insights</span></span>
<span data-ttu-id="fedaf-105">Den här artikeln beskrivs hur du tooget telemetridata från [Microsoft Dynamics CRM Online](https://www.dynamics.com/) med [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span><span class="sxs-lookup"><span data-stu-id="fedaf-105">This article shows you how tooget telemetry data from [Microsoft Dynamics CRM Online](https://www.dynamics.com/) using [Azure Application Insights](https://azure.microsoft.com/services/application-insights/).</span></span> <span data-ttu-id="fedaf-106">Vi går igenom hello Slutför process för att lägga till Application Insights skriptet tooyour program, data och datavisualisering.</span><span class="sxs-lookup"><span data-stu-id="fedaf-106">We’ll walk through hello complete process of adding Application Insights script tooyour application, capturing data, and data visualization.</span></span>

> [!NOTE]
> <span data-ttu-id="fedaf-107">[Bläddra hello exempellösning](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="fedaf-107">[Browse hello sample solution](https://dynamicsandappinsights.codeplex.com/).</span></span>
> 
> 

## <a name="add-application-insights-toonew-or-existing-crm-online-instance"></a><span data-ttu-id="fedaf-108">Lägg till Application Insights toonew eller befintliga CRM Online-instans</span><span class="sxs-lookup"><span data-stu-id="fedaf-108">Add Application Insights toonew or existing CRM Online instance</span></span>
<span data-ttu-id="fedaf-109">toomonitor ditt program kan du lägga till en Application Insights SDK tooyour program.</span><span class="sxs-lookup"><span data-stu-id="fedaf-109">toomonitor your application, you add an Application Insights SDK tooyour application.</span></span> <span data-ttu-id="fedaf-110">hello SDK skickar telemetri toohello [Application Insights-portalen](https://portal.azure.com), där du kan använda våra kraftfulla analys och diagnosverktyg eller exportera hello data toostorage.</span><span class="sxs-lookup"><span data-stu-id="fedaf-110">hello SDK sends telemetry toohello [Application Insights portal](https://portal.azure.com), where you can use our powerful analysis and diagnostic tools, or export hello data toostorage.</span></span>

### <a name="create-an-application-insights-resource-in-azure"></a><span data-ttu-id="fedaf-111">Skapa en Application Insights-resurs i Azure</span><span class="sxs-lookup"><span data-stu-id="fedaf-111">Create an Application Insights resource in Azure</span></span>
1. <span data-ttu-id="fedaf-112">Hämta [ett konto i Microsoft Azure](http://azure.com/pricing).</span><span class="sxs-lookup"><span data-stu-id="fedaf-112">Get [an account in Microsoft Azure](http://azure.com/pricing).</span></span> 
2. <span data-ttu-id="fedaf-113">Logga in på hello [Azure-portalen](https://portal.azure.com) och lägga till en ny Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="fedaf-113">Sign into hello [Azure portal](https://portal.azure.com) and add a new Application Insights resource.</span></span> <span data-ttu-id="fedaf-114">Detta är där dina data bearbetas och visas.</span><span class="sxs-lookup"><span data-stu-id="fedaf-114">This is where your data will be processed and displayed.</span></span>
   
    ![Klicka på +, Developer Services Application Insights.](./media/app-insights-sample-mscrm/01.png)
   
    <span data-ttu-id="fedaf-116">Välj ASP.NET som hello programtyp.</span><span class="sxs-lookup"><span data-stu-id="fedaf-116">Choose ASP.NET as hello application type.</span></span>
3. <span data-ttu-id="fedaf-117">Öppna hello komma igång-sidan och öppna ”övervaka och diagnostisera på klientsidan”.</span><span class="sxs-lookup"><span data-stu-id="fedaf-117">Open hello Getting Started page and open "Monitor and diagnose client side".</span></span>
   
    ![Kodstycke för infogning i din webbsida](./media/app-insights-sample-mscrm/03.png)

<span data-ttu-id="fedaf-119">**Håll hello teckentabellen öppen** medan du hello nästa steg i ett nytt webbläsarfönster.</span><span class="sxs-lookup"><span data-stu-id="fedaf-119">**Keep hello code page open** while you do hello next step in another browser window.</span></span> <span data-ttu-id="fedaf-120">Du behöver hello kod snart.</span><span class="sxs-lookup"><span data-stu-id="fedaf-120">You'll need hello code soon.</span></span> 

### <a name="create-a-javascript-web-resource-in-microsoft-dynamics-crm"></a><span data-ttu-id="fedaf-121">Skapa en webbresurs JavaScript i Microsoft Dynamics CRM</span><span class="sxs-lookup"><span data-stu-id="fedaf-121">Create a JavaScript web resource in Microsoft Dynamics CRM</span></span>
1. <span data-ttu-id="fedaf-122">Öppna din CRM Online-instans och logga in med administratörsbehörighet.</span><span class="sxs-lookup"><span data-stu-id="fedaf-122">Open your CRM Online instance and login with administrator privileges.</span></span>
2. <span data-ttu-id="fedaf-123">Öppna Microsoft Dynamics CRM inställningar, anpassningar, anpassa hello System</span><span class="sxs-lookup"><span data-stu-id="fedaf-123">Open Microsoft Dynamics CRM Settings, Customizations, Customize hello System</span></span>
   
    ![Microsoft Dynamics CRM-inställningar](./media/app-insights-sample-mscrm/04.png)
   
    ![Inställningar > anpassningar](./media/app-insights-sample-mscrm/05.png)

    ![Anpassa hello system alternativet](./media/app-insights-sample-mscrm/06.png)

1. <span data-ttu-id="fedaf-127">Skapa en JavaScript-resurs.</span><span class="sxs-lookup"><span data-stu-id="fedaf-127">Create a JavaScript resource.</span></span>
   
    ![Dialogrutan Ny webbresurs](./media/app-insights-sample-mscrm/07.png)
   
    <span data-ttu-id="fedaf-129">Ge det ett namn, Välj **skript (JScript)** och öppna hello textredigerare.</span><span class="sxs-lookup"><span data-stu-id="fedaf-129">Give it a name, select **Script (JScript)** and open hello text editor.</span></span>
   
    ![Öppna hello textredigerare](./media/app-insights-sample-mscrm/08.png)
2. <span data-ttu-id="fedaf-131">Kopiera hello koden från Application Insights.</span><span class="sxs-lookup"><span data-stu-id="fedaf-131">Copy hello code from Application Insights.</span></span> <span data-ttu-id="fedaf-132">Se till att tooignore skripttaggar vid kopiering.</span><span class="sxs-lookup"><span data-stu-id="fedaf-132">While copying make sure tooignore script tags.</span></span> <span data-ttu-id="fedaf-133">Se nedan skärmbild:</span><span class="sxs-lookup"><span data-stu-id="fedaf-133">Refer below screenshot:</span></span>
   
    ![Ange din instrumentation nyckel](./media/app-insights-sample-mscrm/09.png)
   
    <span data-ttu-id="fedaf-135">hello-koden innehåller hello instrumentation nyckel som identifierar din Application insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="fedaf-135">hello code includes hello instrumentation key that identifies your Application insights resource.</span></span>
3. <span data-ttu-id="fedaf-136">Spara och publicera.</span><span class="sxs-lookup"><span data-stu-id="fedaf-136">Save and publish.</span></span>
   
    ![Spara och publicera](./media/app-insights-sample-mscrm/10.png)

### <a name="instrument-forms"></a><span data-ttu-id="fedaf-138">Betalningsinstrument formulär</span><span class="sxs-lookup"><span data-stu-id="fedaf-138">Instrument Forms</span></span>
1. <span data-ttu-id="fedaf-139">Öppna formuläret för hello-konto i Microsoft CRM Online</span><span class="sxs-lookup"><span data-stu-id="fedaf-139">In Microsoft CRM Online, open hello Account form</span></span>
   
    ![Kontoformuläret](./media/app-insights-sample-mscrm/11.png)
2. <span data-ttu-id="fedaf-141">Öppna hello formuläregenskaper</span><span class="sxs-lookup"><span data-stu-id="fedaf-141">Open hello form Properties</span></span>
   
    ![Egenskaper för formulär](./media/app-insights-sample-mscrm/12.png)
3. <span data-ttu-id="fedaf-143">Lägg till hello JavaScript webbresurs som du skapade</span><span class="sxs-lookup"><span data-stu-id="fedaf-143">Add hello JavaScript web resource that you created</span></span>
   
    ![Menyn Lägg till](./media/app-insights-sample-mscrm/13.png)
   
    ![Lägg till hello webbresurs](./media/app-insights-sample-mscrm/14.png)
4. <span data-ttu-id="fedaf-146">Spara och publicera dina formuläranpassningar.</span><span class="sxs-lookup"><span data-stu-id="fedaf-146">Save and publish your form customizations.</span></span>

## <a name="metrics-captured"></a><span data-ttu-id="fedaf-147">Mått som har hämtats</span><span class="sxs-lookup"><span data-stu-id="fedaf-147">Metrics captured</span></span>
<span data-ttu-id="fedaf-148">Du har nu konfigurerat telemetriinsamling för hello formulär.</span><span class="sxs-lookup"><span data-stu-id="fedaf-148">You have now set up telemetry capture for hello form.</span></span> <span data-ttu-id="fedaf-149">När den används kan data skickas som tooyour Application Insights-resurs.</span><span class="sxs-lookup"><span data-stu-id="fedaf-149">Whenever it is used, data will be sent tooyour Application Insights resource.</span></span>

<span data-ttu-id="fedaf-150">Här följer exempel på hello data som visas.</span><span class="sxs-lookup"><span data-stu-id="fedaf-150">Here are samples of hello data that you'll see.</span></span>

#### <a name="application-health"></a><span data-ttu-id="fedaf-151">Programmets hälsotillstånd</span><span class="sxs-lookup"><span data-stu-id="fedaf-151">Application health</span></span>
![Exempel sidinläsningstiden](./media/app-insights-sample-mscrm/15.png)

![Exempeldiagram sidan vyer](./media/app-insights-sample-mscrm/16.png)

<span data-ttu-id="fedaf-154">Webbläsarundantag:</span><span class="sxs-lookup"><span data-stu-id="fedaf-154">Browser exceptions:</span></span>

![Webbläsaren undantag diagram](./media/app-insights-sample-mscrm/17.png)

<span data-ttu-id="fedaf-156">Klicka på hello diagram tooget detalj:</span><span class="sxs-lookup"><span data-stu-id="fedaf-156">Click hello chart tooget more detail:</span></span>

![Listan över undantag](./media/app-insights-sample-mscrm/18.png)

#### <a name="usage"></a><span data-ttu-id="fedaf-158">Användning</span><span class="sxs-lookup"><span data-stu-id="fedaf-158">Usage</span></span>
![Användare, sessioner och sidvyer](./media/app-insights-sample-mscrm/19.png)

![Sesion diagram](./media/app-insights-sample-mscrm/20.png)

![Webbläsarversioner](./media/app-insights-sample-mscrm/21.png)

#### <a name="browsers"></a><span data-ttu-id="fedaf-162">Webbläsare</span><span class="sxs-lookup"><span data-stu-id="fedaf-162">Browsers</span></span>
![Uppdelning av sidinläsningstiden](./media/app-insights-sample-mscrm/22.png)

![Antal sessioner efter webbläsarversion](./media/app-insights-sample-mscrm/23.png)

#### <a name="geolocation"></a><span data-ttu-id="fedaf-165">Geoplats</span><span class="sxs-lookup"><span data-stu-id="fedaf-165">Geolocation</span></span>
![Sessionsantal efter land](./media/app-insights-sample-mscrm/24.png)

![Sessioner och användare per land](./media/app-insights-sample-mscrm/25.png)

#### <a name="inside-page-view-request"></a><span data-ttu-id="fedaf-168">Inuti vyn begäran</span><span class="sxs-lookup"><span data-stu-id="fedaf-168">Inside page view request</span></span>
![Sidan Visa sammanfattning](./media/app-insights-sample-mscrm/26.png)

![Söka på sidan Visa händelser](./media/app-insights-sample-mscrm/27.png)

![Liknande sidvisningar](./media/app-insights-sample-mscrm/28.png)

![Egenskaper för sidvisning](./media/app-insights-sample-mscrm/29.png)

![Sidor per session](./media/app-insights-sample-mscrm/30.png)

## <a name="sample-code"></a><span data-ttu-id="fedaf-174">Exempelkod</span><span class="sxs-lookup"><span data-stu-id="fedaf-174">Sample code</span></span>
<span data-ttu-id="fedaf-175">[Bläddra hello exempelkod](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="fedaf-175">[Browse hello sample code](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="power-bi"></a><span data-ttu-id="fedaf-176">Power BI</span><span class="sxs-lookup"><span data-stu-id="fedaf-176">Power BI</span></span>
<span data-ttu-id="fedaf-177">Du kan göra djupare analys om du [exportera hello data tooMicrosoft Power BI](app-insights-export-power-bi.md).</span><span class="sxs-lookup"><span data-stu-id="fedaf-177">You can do even deeper analysis if you [export hello data tooMicrosoft Power BI](app-insights-export-power-bi.md).</span></span>

## <a name="sample-microsoft-dynamics-crm-solution"></a><span data-ttu-id="fedaf-178">Exempel Microsoft Dynamics CRM-lösning</span><span class="sxs-lookup"><span data-stu-id="fedaf-178">Sample Microsoft Dynamics CRM Solution</span></span>
<span data-ttu-id="fedaf-179">[Här är hello exempellösning implementerad i Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).</span><span class="sxs-lookup"><span data-stu-id="fedaf-179">[Here is hello sample solution implemented in Microsoft Dynamics CRM](https://dynamicsandappinsights.codeplex.com/).</span></span>

## <a name="learn-more"></a><span data-ttu-id="fedaf-180">Läs mer</span><span class="sxs-lookup"><span data-stu-id="fedaf-180">Learn more</span></span>
* [<span data-ttu-id="fedaf-181">Vad är Application Insights?</span><span class="sxs-lookup"><span data-stu-id="fedaf-181">What is Application Insights?</span></span>](app-insights-overview.md)
* [<span data-ttu-id="fedaf-182">Application Insights för webbsidor</span><span class="sxs-lookup"><span data-stu-id="fedaf-182">Application Insights for web pages</span></span>](app-insights-javascript.md)
* [<span data-ttu-id="fedaf-183">Fler exempel och genomgång</span><span class="sxs-lookup"><span data-stu-id="fedaf-183">More samples and walkthroughs</span></span>](app-insights-code-samples.md)
