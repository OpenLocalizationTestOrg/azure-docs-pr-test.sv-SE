---
title: aaaCreate en ny Azure Application Insights-resurs | Microsoft Docs
description: "Manuellt konfigurera Application Insights-övervakning för ett nytt live program."
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 878b007e-161c-4e36-8ab2-3d7047d8a92d
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 12/02/2016
ms.author: bwren
ms.openlocfilehash: 3aba7045e1f8fe43d473f0be01dd52106ab992a8
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-application-insights-resource"></a><span data-ttu-id="165b9-103">Skapa en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="165b9-103">Create an Application Insights resource</span></span>
<span data-ttu-id="165b9-104">Azure Application Insights visar data om ditt program i en Microsoft Azure *resurs*.</span><span class="sxs-lookup"><span data-stu-id="165b9-104">Azure Application Insights displays data about your application in a Microsoft Azure *resource*.</span></span> <span data-ttu-id="165b9-105">Skapa en ny resurs är därför en del av [konfigurera Application Insights toomonitor ett nytt program][start].</span><span class="sxs-lookup"><span data-stu-id="165b9-105">Creating a new resource is therefore part of [setting up Application Insights toomonitor a new application][start].</span></span> <span data-ttu-id="165b9-106">I många fall kan att skapa en resurs göras automatiskt med hello IDE.</span><span class="sxs-lookup"><span data-stu-id="165b9-106">In many cases, creating a resource can be done automatically by hello IDE.</span></span> <span data-ttu-id="165b9-107">Men i vissa fall kan du skapa en resurs manuellt, till exempel toohave separata resurser för utveckling och produktion versioner av programmet.</span><span class="sxs-lookup"><span data-stu-id="165b9-107">But in some cases, you create a resource manually - for example, toohave separate resources for development and production builds of your application.</span></span>

<span data-ttu-id="165b9-108">När du har skapat hello resurs kan du hämta dess instrumentation nyckel och använder som tooconfigure hello SDK i hello program.</span><span class="sxs-lookup"><span data-stu-id="165b9-108">After you have created hello resource, you get its instrumentation key and use that tooconfigure hello SDK in hello application.</span></span> <span data-ttu-id="165b9-109">viktiga hello resurslänkar hello telemetri toohello resurs.</span><span class="sxs-lookup"><span data-stu-id="165b9-109">hello resource key links hello telemetry toohello resource.</span></span>

## <a name="sign-up-toomicrosoft-azure"></a><span data-ttu-id="165b9-110">Registrera dig tooMicrosoft Azure</span><span class="sxs-lookup"><span data-stu-id="165b9-110">Sign up tooMicrosoft Azure</span></span>
<span data-ttu-id="165b9-111">Om du inte har en [Microsoft konto måste du skaffa ett nu](http://live.com).</span><span class="sxs-lookup"><span data-stu-id="165b9-111">If you haven't got a [Microsoft account, get one now](http://live.com).</span></span> <span data-ttu-id="165b9-112">(Om du använder tjänster som Outlook.com, OneDrive, Windows Phone och XBox Live du redan har ett Microsoft-konto.)</span><span class="sxs-lookup"><span data-stu-id="165b9-112">(If you use services like Outlook.com, OneDrive, Windows Phone, or XBox Live, you already have a Microsoft account.)</span></span>

<span data-ttu-id="165b9-113">Du måste också en prenumeration för[Microsoft Azure](http://azure.com).</span><span class="sxs-lookup"><span data-stu-id="165b9-113">You also need a subscription too[Microsoft Azure](http://azure.com).</span></span> <span data-ttu-id="165b9-114">Om din grupp eller organisation har en Azure-prenumeration, hello ägare kan lägga till dig tooit, med ditt Windows Live ID.</span><span class="sxs-lookup"><span data-stu-id="165b9-114">If your team or organization has an Azure subscription, hello owner can add you tooit, using your Windows Live ID.</span></span> <span data-ttu-id="165b9-115">Du kan endast debiteras du använder.</span><span class="sxs-lookup"><span data-stu-id="165b9-115">You're only charged for what you use.</span></span> <span data-ttu-id="165b9-116">grundläggande hello standardplanen kan under en viss experiment används utan kostnad.</span><span class="sxs-lookup"><span data-stu-id="165b9-116">hello default basic plan allows for a certain amount of experimental use free of charge.</span></span>

<span data-ttu-id="165b9-117">När du har fått åtkomst tooa prenumeration, logga in tooApplication insikter på [http://portal.azure.com](https://portal.azure.com), och använda Live ID-toologin.</span><span class="sxs-lookup"><span data-stu-id="165b9-117">When you've got access tooa subscription, log in tooApplication Insights at [http://portal.azure.com](https://portal.azure.com), and use your Live ID toologin.</span></span>

## <a name="create-an-application-insights-resource"></a><span data-ttu-id="165b9-118">Skapa en Application Insights-resurs</span><span class="sxs-lookup"><span data-stu-id="165b9-118">Create an Application Insights resource</span></span>
<span data-ttu-id="165b9-119">I hello [portal.azure.com](https://portal.azure.com), Lägg till Application Insights-resurs:</span><span class="sxs-lookup"><span data-stu-id="165b9-119">In hello [portal.azure.com](https://portal.azure.com), add an Application Insights resource:</span></span>

![Klicka på Nytt, Application Insights](./media/app-insights-create-new-resource/01-new.png)

* <span data-ttu-id="165b9-121">**Programtyp** påverkar vad som visas på hello översikt bladet och hello-egenskaper som är tillgängliga i [mått explorer][metrics].</span><span class="sxs-lookup"><span data-stu-id="165b9-121">**Application type** affects what you see on hello overview blade and hello properties available in [metric explorer][metrics].</span></span> <span data-ttu-id="165b9-122">Välj Allmänt om du inte ser typen av app.</span><span class="sxs-lookup"><span data-stu-id="165b9-122">If you don't see your type of app, choose General.</span></span>
* <span data-ttu-id="165b9-123">**Prenumerationen** är din betalningskonto i Azure.</span><span class="sxs-lookup"><span data-stu-id="165b9-123">**Subscription** is your payment account in Azure.</span></span>
* <span data-ttu-id="165b9-124">**Resursgruppen** är för att hantera egenskaper i syfte att underlätta som åtkomstkontroll.</span><span class="sxs-lookup"><span data-stu-id="165b9-124">**Resource group** is a convenience for managing properties like access control.</span></span> <span data-ttu-id="165b9-125">Om du redan har skapat andra Azure-resurser kan du välja tooput nya resursen i hello samma grupp.</span><span class="sxs-lookup"><span data-stu-id="165b9-125">If you have already created other Azure resources, you can choose tooput this new resource in hello same group.</span></span>
* <span data-ttu-id="165b9-126">**Plats** är där vi behåller dina data.</span><span class="sxs-lookup"><span data-stu-id="165b9-126">**Location** is where we keep your data.</span></span>
* <span data-ttu-id="165b9-127">**PIN-kod toodashboard** placerar en snabb åtkomst-panelen för din resurs på Azure-startsidan.</span><span class="sxs-lookup"><span data-stu-id="165b9-127">**Pin toodashboard** puts a quick-access tile for your resource on your Azure Home page.</span></span> <span data-ttu-id="165b9-128">Rekommenderas.</span><span class="sxs-lookup"><span data-stu-id="165b9-128">Recommended.</span></span>

<span data-ttu-id="165b9-129">När appen har skapats, öppnas ett nytt blad.</span><span class="sxs-lookup"><span data-stu-id="165b9-129">When your app has been created, a new blade opens.</span></span> <span data-ttu-id="165b9-130">Det här bladet är där du ser prestanda- och användningsdata om din app.</span><span class="sxs-lookup"><span data-stu-id="165b9-130">This blade is where you see performance and usage data about your app.</span></span> 

<span data-ttu-id="165b9-131">tooget tillbaka tooit nästa gång du loggar in tooAzure, leta efter din app Snabbkurs panelen på hello starta board (startsidan).</span><span class="sxs-lookup"><span data-stu-id="165b9-131">tooget back tooit next time you log in tooAzure, look for your app's quick-start tile on hello start board (home screen).</span></span> <span data-ttu-id="165b9-132">Eller klicka på Bläddra toofind den.</span><span class="sxs-lookup"><span data-stu-id="165b9-132">Or click Browse toofind it.</span></span>

## <a name="copy-hello-instrumentation-key"></a><span data-ttu-id="165b9-133">Kopiera hello instrumentation nyckeln</span><span class="sxs-lookup"><span data-stu-id="165b9-133">Copy hello instrumentation key</span></span>
<span data-ttu-id="165b9-134">hello instrumentation nyckel identifierar hello-resurs som du skapade.</span><span class="sxs-lookup"><span data-stu-id="165b9-134">hello instrumentation key identifies hello resource that you created.</span></span> <span data-ttu-id="165b9-135">Du behöver den toogive toohello SDK.</span><span class="sxs-lookup"><span data-stu-id="165b9-135">You need it toogive toohello SDK.</span></span>

![Klicka på Essentials, hello Instrumentation nyckeln CTRL + C](./media/app-insights-create-new-resource/02-props.png)

## <a name="install-hello-sdk-in-your-app"></a><span data-ttu-id="165b9-137">Installera hello SDK i din app</span><span class="sxs-lookup"><span data-stu-id="165b9-137">Install hello SDK in your app</span></span>
<span data-ttu-id="165b9-138">Installera hello Application Insights SDK i din app.</span><span class="sxs-lookup"><span data-stu-id="165b9-138">Install hello Application Insights SDK in your app.</span></span> <span data-ttu-id="165b9-139">Det här steget beror kraftigt på hello typ av ditt program.</span><span class="sxs-lookup"><span data-stu-id="165b9-139">This step depends heavily on hello type of your application.</span></span> 

<span data-ttu-id="165b9-140">Använd hello instrumentation viktiga tooconfigure [hello SDK som du installerar programmet][start].</span><span class="sxs-lookup"><span data-stu-id="165b9-140">Use hello instrumentation key tooconfigure [hello SDK that you install in your application][start].</span></span>

<span data-ttu-id="165b9-141">hello SDK innehåller standard moduler som skickar telemetri utan att behöva toowrite någon kod.</span><span class="sxs-lookup"><span data-stu-id="165b9-141">hello SDK includes standard modules that send telemetry without you having toowrite any code.</span></span> <span data-ttu-id="165b9-142">tootrack användaråtgärder eller diagnostisera problem i detalj, [använder hello API] [ api] toosend egna telemetri.</span><span class="sxs-lookup"><span data-stu-id="165b9-142">tootrack user actions or diagnose issues in more detail, [use hello API][api] toosend your own telemetry.</span></span>

## <span data-ttu-id="165b9-143"><a name="monitor"></a>Se telemetridata</span><span class="sxs-lookup"><span data-stu-id="165b9-143"><a name="monitor"></a>See telemetry data</span></span>
<span data-ttu-id="165b9-144">Stäng hello snabb start bladet tooreturn tooyour programmet bladet i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="165b9-144">Close hello quick start blade tooreturn tooyour application blade in hello Azure portal.</span></span>

<span data-ttu-id="165b9-145">Klicka på hello Sök panelen toosee [diagnostiska Sök][diagnostic], där hello första händelser visas.</span><span class="sxs-lookup"><span data-stu-id="165b9-145">Click hello Search tile toosee [Diagnostic Search][diagnostic], where hello first events appear.</span></span> 

<span data-ttu-id="165b9-146">Om du väntar mer data, klickar du på **uppdatera** efter några sekunder.</span><span class="sxs-lookup"><span data-stu-id="165b9-146">If you're expecting more data, click **Refresh** after a few seconds  .</span></span>

## <a name="creating-a-resource-automatically"></a><span data-ttu-id="165b9-147">Skapa en resurs automatiskt</span><span class="sxs-lookup"><span data-stu-id="165b9-147">Creating a resource automatically</span></span>
<span data-ttu-id="165b9-148">Du kan skriva en [PowerShell-skript](app-insights-powershell.md) toocreate en resurs automatiskt.</span><span class="sxs-lookup"><span data-stu-id="165b9-148">You can write a [PowerShell script](app-insights-powershell.md) toocreate a resource automatically.</span></span>

## <a name="next-steps"></a><span data-ttu-id="165b9-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="165b9-149">Next steps</span></span>
* [<span data-ttu-id="165b9-150">Skapa en instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="165b9-150">Create a dashboard</span></span>](app-insights-dashboards.md)
* [<span data-ttu-id="165b9-151">Diagnostiksökning</span><span class="sxs-lookup"><span data-stu-id="165b9-151">Diagnostic Search</span></span>](app-insights-diagnostic-search.md)
* [<span data-ttu-id="165b9-152">Utforska mått</span><span class="sxs-lookup"><span data-stu-id="165b9-152">Explore metrics</span></span>](app-insights-metrics-explorer.md)
* [<span data-ttu-id="165b9-153">Skriv analysfrågor</span><span class="sxs-lookup"><span data-stu-id="165b9-153">Write Analytics queries</span></span>](app-insights-analytics.md)

<!--Link references-->

[api]: app-insights-api-custom-events-metrics.md
[diagnostic]: app-insights-diagnostic-search.md
[metrics]: app-insights-metrics-explorer.md
[start]: app-insights-overview.md

