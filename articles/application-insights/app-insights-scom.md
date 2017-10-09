---
title: aaaSCOM integrering med Application Insights | Microsoft Docs
description: "Om du använder en SCOM, övervaka prestanda och diagnostisera problem med Application Insights. Omfattande instrumentpaneler, smart aviseringar, kraftfulla verktyg för Nätverksdiagnostik och analys frågor."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 606e9d03-c0e6-4a77-80e8-61b75efacde0
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 08/12/2016
ms.author: bwren
ms.openlocfilehash: ee9ad462610fd916098a0e292c5bd44f2a873c10
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="application-performance-monitoring-using-application-insights-for-scom"></a><span data-ttu-id="107f9-104">Prestandaövervakning av program med Application Insights för SCOM</span><span class="sxs-lookup"><span data-stu-id="107f9-104">Application Performance Monitoring using Application Insights for SCOM</span></span>
<span data-ttu-id="107f9-105">Om du använder System Center Operations Manager (SCOM) toomanage dina servrar, kan du övervaka prestanda och diagnostisera prestandaproblem med hello [Azure Application Insights](app-insights-asp-net.md).</span><span class="sxs-lookup"><span data-stu-id="107f9-105">If you use System Center Operations Manager (SCOM) toomanage your servers, you can monitor performance and diagnose performance issues with hello help of [Azure Application Insights](app-insights-asp-net.md).</span></span> <span data-ttu-id="107f9-106">Application Insights övervakar ditt webbprogram inkommande begäranden, utgående REST och SQL-anrop, undantag och loggspårningar.</span><span class="sxs-lookup"><span data-stu-id="107f9-106">Application Insights monitors your web application's incoming requests, outgoing REST and SQL calls, exceptions, and log traces.</span></span> <span data-ttu-id="107f9-107">Det ger instrumentpaneler med mått diagram och smarta aviseringar, samt kraftfulla diagnostiska Sök och analytiska frågor via den här telemetri.</span><span class="sxs-lookup"><span data-stu-id="107f9-107">It provides dashboards with metric charts and smart alerts, as well as powerful diagnostic search and analytical queries over this telemetry.</span></span> 

<span data-ttu-id="107f9-108">Du kan växla på Application Insights-övervakning med hjälp av ett hanteringspaket för SCOM.</span><span class="sxs-lookup"><span data-stu-id="107f9-108">You can switch on Application Insights monitoring by using an SCOM management pack.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="107f9-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="107f9-109">Before you start</span></span>
<span data-ttu-id="107f9-110">Vi förutsätter:</span><span class="sxs-lookup"><span data-stu-id="107f9-110">We assume:</span></span>

* <span data-ttu-id="107f9-111">Du är bekant med SCOM och att du använder SCOM 2012 R2 eller 2016 toomanage din IIS-webbservrar.</span><span class="sxs-lookup"><span data-stu-id="107f9-111">You're familiar with SCOM, and that you use SCOM 2012 R2 or 2016 toomanage your IIS web servers.</span></span>
* <span data-ttu-id="107f9-112">Du har redan installerat på dina servrar för ett webbprogram som du vill toomonitor med Application Insights.</span><span class="sxs-lookup"><span data-stu-id="107f9-112">You have already installed on your servers a web application that you want toomonitor with Application Insights.</span></span>
* <span data-ttu-id="107f9-113">Appen framework-version är .NET 4.5 eller senare.</span><span class="sxs-lookup"><span data-stu-id="107f9-113">App framework version is .NET 4.5 or later.</span></span>
* <span data-ttu-id="107f9-114">Du har åtkomst tooa prenumeration [Microsoft Azure](https://azure.com) och kan logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="107f9-114">You have access tooa subscription in [Microsoft Azure](https://azure.com) and can sign in toohello [Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="107f9-115">Din organisation kan ha en prenumeration och kan lägga till ditt Microsoft-konto tooit.</span><span class="sxs-lookup"><span data-stu-id="107f9-115">Your organization may have a subscription, and can add your Microsoft account tooit.</span></span>

<span data-ttu-id="107f9-116">(hello Utvecklingsteamet kan skapa hello [Application Insights SDK](app-insights-asp-net.md) i hello webbapp.</span><span class="sxs-lookup"><span data-stu-id="107f9-116">(hello development team might build hello [Application Insights SDK](app-insights-asp-net.md) into hello web app.</span></span> <span data-ttu-id="107f9-117">Den här byggning instrumentation ger dem större flexibilitet vid skrivning anpassad telemetri.</span><span class="sxs-lookup"><span data-stu-id="107f9-117">This build-time instrumentation gives them greater flexibility in writing custom telemetry.</span></span> <span data-ttu-id="107f9-118">Men det spelar ingen roll: du kan följa hello steg som beskrivs här med eller utan hello SDK inbyggda.)</span><span class="sxs-lookup"><span data-stu-id="107f9-118">However, it doesn't matter: you can follow hello steps described here either with or without hello SDK built in.)</span></span>

## <a name="one-time-install-application-insights-management-pack"></a><span data-ttu-id="107f9-119">(En gång) Installera Application Insights management pack</span><span class="sxs-lookup"><span data-stu-id="107f9-119">(One time) Install Application Insights management pack</span></span>
<span data-ttu-id="107f9-120">På hello datorn där du kör Operations Manager:</span><span class="sxs-lookup"><span data-stu-id="107f9-120">On hello machine where you run Operations Manager:</span></span>

1. <span data-ttu-id="107f9-121">Avinstallera en tidigare version av hello management pack:</span><span class="sxs-lookup"><span data-stu-id="107f9-121">Uninstall any old version of hello management pack:</span></span>
   1. <span data-ttu-id="107f9-122">Öppna Administration Management Packs i Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="107f9-122">In Operations Manager, open Administration, Management Packs.</span></span> 
   2. <span data-ttu-id="107f9-123">Ta bort gamla hello-versionen.</span><span class="sxs-lookup"><span data-stu-id="107f9-123">Delete hello old version.</span></span>
2. <span data-ttu-id="107f9-124">Hämta och installera hello hanteringspaket hello-katalogen.</span><span class="sxs-lookup"><span data-stu-id="107f9-124">Download and install hello management pack from hello catalog.</span></span>
3. <span data-ttu-id="107f9-125">Starta om Operations Manager.</span><span class="sxs-lookup"><span data-stu-id="107f9-125">Restart Operations Manager.</span></span>

## <a name="create-a-management-pack"></a><span data-ttu-id="107f9-126">Skapa ett hanteringspaket</span><span class="sxs-lookup"><span data-stu-id="107f9-126">Create a management pack</span></span>
1. <span data-ttu-id="107f9-127">Öppna i Operations Manager, **redigering**, **.NET... med Application Insights**, **guiden Lägg till övervakning**, och välj igen **.NET med program... Insikter**.</span><span class="sxs-lookup"><span data-stu-id="107f9-127">In Operations Manager, open **Authoring**, **.NET...with Application Insights**, **Add Monitoring Wizard**, and again choose **.NET...with Application Insights**.</span></span>
   
    ![](./media/app-insights-scom/020.png)
2. <span data-ttu-id="107f9-128">Konfiguration av hello efter din app.</span><span class="sxs-lookup"><span data-stu-id="107f9-128">Name hello configuration after your app.</span></span> <span data-ttu-id="107f9-129">(Du har tooinstrument en app åt gången.)</span><span class="sxs-lookup"><span data-stu-id="107f9-129">(You have tooinstrument one app at a time.)</span></span>
   
    ![](./media/app-insights-scom/030.png)
3. <span data-ttu-id="107f9-130">Hej på samma sida i guiden Skapa ett nytt management pack, eller välj ett paket som du tidigare skapade för Application Insights.</span><span class="sxs-lookup"><span data-stu-id="107f9-130">On hello same wizard page, either create a new management pack, or select a pack that you created for Application Insights earlier.</span></span>
   
     <span data-ttu-id="107f9-131">(hello Application Insights [hanteringspaket](https://technet.microsoft.com/library/cc974491.aspx) är en mall som du skapar en instans.</span><span class="sxs-lookup"><span data-stu-id="107f9-131">(hello Application Insights [management pack](https://technet.microsoft.com/library/cc974491.aspx) is a template, from which you create an instance.</span></span> <span data-ttu-id="107f9-132">Du kan återanvända samma instans senare hello.)</span><span class="sxs-lookup"><span data-stu-id="107f9-132">You can reuse hello same instance later.)</span></span>

    ![Skriv hello namnet på hello app i hello fliken allmänna egenskaper.](./media/app-insights-scom/040.png)

1. <span data-ttu-id="107f9-136">Välj en app som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="107f9-136">Choose one app that you want toomonitor.</span></span> <span data-ttu-id="107f9-137">hello sökfunktionen söker bland appar som är installerade på servrarna.</span><span class="sxs-lookup"><span data-stu-id="107f9-137">hello search feature searches among apps installed on your servers.</span></span>
   
    ![Vilka tooMonitor på fliken klickar du på Lägg till, skriver du en del av hello programnamn, klicka på Sök, väljer hello appen och sedan lägga till, OK.](./media/app-insights-scom/050.png)
   
    <span data-ttu-id="107f9-139">hello övervakning scope Fritextfält kan vara används toospecify en delmängd av dina servrar, om du inte vill toomonitor hello app i alla servrar.</span><span class="sxs-lookup"><span data-stu-id="107f9-139">hello optional Monitoring scope field can be used toospecify a subset of your servers, if you don't want toomonitor hello app in all servers.</span></span>
2. <span data-ttu-id="107f9-140">Du måste ange dina autentiseringsuppgifter toosign i tooMicrosoft Azure på hello nästa sida i guiden.</span><span class="sxs-lookup"><span data-stu-id="107f9-140">On hello next wizard page, you must first provide your credentials toosign in tooMicrosoft Azure.</span></span>
   
    <span data-ttu-id="107f9-141">Välj hello Application Insights-resurs där du vill att hello telemetri data toobe analyseras och visas på den här sidan.</span><span class="sxs-lookup"><span data-stu-id="107f9-141">On this page, you choose hello Application Insights resource where you want hello telemetry data toobe analyzed and displayed.</span></span> 
   
   * <span data-ttu-id="107f9-142">Om programmet hello konfigurerades för Application Insights under utvecklingen, väljer du den befintliga resursen.</span><span class="sxs-lookup"><span data-stu-id="107f9-142">If hello application was configured for Application Insights during development, select its existing resource.</span></span>
   * <span data-ttu-id="107f9-143">Annars skapar du en ny resurs med namnet för hello app.</span><span class="sxs-lookup"><span data-stu-id="107f9-143">Otherwise, create a new resource named for hello app.</span></span> <span data-ttu-id="107f9-144">Om det finns andra appar som är komponenter av hello samma system placeras i hello samma resursgrupp, toomake åtkomst toohello telemetri enklare toomanage.</span><span class="sxs-lookup"><span data-stu-id="107f9-144">If there are other apps that are components of hello same system, put them in hello same resource group, toomake access toohello telemetry easier toomanage.</span></span>
     
     <span data-ttu-id="107f9-145">Du kan ändra inställningarna senare.</span><span class="sxs-lookup"><span data-stu-id="107f9-145">You can change these settings later.</span></span>
     
     ![På fliken för Application Insights-inställningar, klicka på Logga in och ange autentiseringsuppgifterna för ditt Microsoft-konto för Azure.](./media/app-insights-scom/060.png)
3. <span data-ttu-id="107f9-148">Fullständig hello guiden.</span><span class="sxs-lookup"><span data-stu-id="107f9-148">Complete hello wizard.</span></span>
   
    ![Klicka på Skapa](./media/app-insights-scom/070.png)

<span data-ttu-id="107f9-150">Upprepa proceduren för varje app som du vill toomonitor.</span><span class="sxs-lookup"><span data-stu-id="107f9-150">Repeat this procedure for each app that you want toomonitor.</span></span>

<span data-ttu-id="107f9-151">Om du senare behöver toochange inställningar, övervaka hello öppnar egenskaperna för hello från hello redigering fönster.</span><span class="sxs-lookup"><span data-stu-id="107f9-151">If you need toochange settings later, re-open hello properties of hello monitor from hello Authoring window.</span></span>

![I redigering, Välj .NET Application Performance Monitoring med Application Insights, Välj din Övervakare och klickar på Egenskaper.](./media/app-insights-scom/080.png)

## <a name="verify-monitoring"></a><span data-ttu-id="107f9-153">Kontrollera övervakning</span><span class="sxs-lookup"><span data-stu-id="107f9-153">Verify monitoring</span></span>
<span data-ttu-id="107f9-154">hello Övervakare för att du har installerat sökningar för din app på varje server.</span><span class="sxs-lookup"><span data-stu-id="107f9-154">hello monitor that you have installed searches for your app on every server.</span></span> <span data-ttu-id="107f9-155">Om den hittar hello app konfigurerar den Application Insights Status Monitor toomonitor hello app.</span><span class="sxs-lookup"><span data-stu-id="107f9-155">Where it finds hello app, it configures Application Insights Status Monitor toomonitor hello app.</span></span> <span data-ttu-id="107f9-156">Om det behövs installerar först Status Monitor på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="107f9-156">If necessary, it first installs Status Monitor on hello server.</span></span>

<span data-ttu-id="107f9-157">Du kan kontrollera vilka instanser av hello app har identifierats:</span><span class="sxs-lookup"><span data-stu-id="107f9-157">You can verify which instances of hello app it has found:</span></span>

![Öppna i övervakning, Programinsikter](./media/app-insights-scom/100.png)

## <a name="view-telemetry-in-application-insights"></a><span data-ttu-id="107f9-159">Visa telemetri i Application Insights</span><span class="sxs-lookup"><span data-stu-id="107f9-159">View telemetry in Application Insights</span></span>
<span data-ttu-id="107f9-160">I hello [Azure-portalen](https://portal.azure.com), bläddra toohello resursen för din app.</span><span class="sxs-lookup"><span data-stu-id="107f9-160">In hello [Azure portal](https://portal.azure.com), browse toohello resource for your app.</span></span> <span data-ttu-id="107f9-161">Du [Visa diagram som visar telemetri](app-insights-dashboards.md) från din app.</span><span class="sxs-lookup"><span data-stu-id="107f9-161">You [see charts showing telemetry](app-insights-dashboards.md) from your app.</span></span> <span data-ttu-id="107f9-162">(Om den inte visas på huvudsidan för hello ännu, klickar du på direktsänd dataström med mått.)</span><span class="sxs-lookup"><span data-stu-id="107f9-162">(If it hasn't shown up on hello main page yet, click Live Metrics Stream.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="107f9-163">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="107f9-163">Next steps</span></span>
* <span data-ttu-id="107f9-164">[Ställ in en instrumentpanel](app-insights-dashboards.md) toobring tillsammans hello viktigaste diagram övervakning detta och andra appar.</span><span class="sxs-lookup"><span data-stu-id="107f9-164">[Set up a dashboard](app-insights-dashboards.md) toobring together hello most important charts monitoring this and other apps.</span></span>
* [<span data-ttu-id="107f9-165">Lär dig mer om mått</span><span class="sxs-lookup"><span data-stu-id="107f9-165">Learn about metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="107f9-166">Konfigurera aviseringar</span><span class="sxs-lookup"><span data-stu-id="107f9-166">Set up alerts</span></span>](app-insights-alerts.md)
* [<span data-ttu-id="107f9-167">Diagnostisera prestandaproblem</span><span class="sxs-lookup"><span data-stu-id="107f9-167">Diagnosing performance issues</span></span>](app-insights-detect-triage-diagnose.md)
* [<span data-ttu-id="107f9-168">Kraftfulla Analytics-frågor</span><span class="sxs-lookup"><span data-stu-id="107f9-168">Powerful Analytics queries</span></span>](app-insights-analytics.md)
* [<span data-ttu-id="107f9-169">Tillgänglighetstester för webbprogram</span><span class="sxs-lookup"><span data-stu-id="107f9-169">Availability web tests</span></span>](app-insights-monitor-web-app-availability.md)

