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
# <a name="get-started-with-azure-mobile-engagement-for-web-apps"></a><span data-ttu-id="8b65f-103">Komma igång med Azure Mobile Engagement för Web Apps</span><span class="sxs-lookup"><span data-stu-id="8b65f-103">Get started with Azure Mobile Engagement for Web Apps</span></span>
[!INCLUDE [Hero tutorial switcher](../../includes/mobile-engagement-hero-tutorial-switcher.md)]

<span data-ttu-id="8b65f-104">Det här avsnittet beskrivs hur du toouse Azure Mobile Engagement toounderstand Web App-användning.</span><span class="sxs-lookup"><span data-stu-id="8b65f-104">This topic shows you how toouse Azure Mobile Engagement toounderstand your Web App usage.</span></span>

> [!NOTE]
> <span data-ttu-id="8b65f-105">hello Azure Mobile Engagement-tjänsten kommer att dras tillbaka mars 2018 och är för närvarande bara tillgänglig tooexisting kunder.</span><span class="sxs-lookup"><span data-stu-id="8b65f-105">hello Azure Mobile Engagement service will be retired March 2018 and is currently only available tooexisting customers.</span></span> <span data-ttu-id="8b65f-106">Mer information finns i [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span><span class="sxs-lookup"><span data-stu-id="8b65f-106">For more information, see [Mobile Engagement](https://azure.microsoft.com/en-us/services/mobile-engagement/).</span></span>

<span data-ttu-id="8b65f-107">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="8b65f-107">This tutorial requires hello following:</span></span>

* <span data-ttu-id="8b65f-108">Visual Studio 2015 eller något annat redigeringsprogram</span><span class="sxs-lookup"><span data-stu-id="8b65f-108">Visual Studio 2015 or any other editor of your choice</span></span>
* [<span data-ttu-id="8b65f-109">Web SDK</span><span class="sxs-lookup"><span data-stu-id="8b65f-109">Web SDK</span></span>](http://aka.ms/P7b453)

<span data-ttu-id="8b65f-110">Web SDK är en förhandsversion och endast stöder Analytics tillfället hello och skicka webbläsare eller i appen push-meddelanden stöder inte ännu.</span><span class="sxs-lookup"><span data-stu-id="8b65f-110">This Web SDK is in Preview and only supports Analytics at hello moment and doesn't support sending browser or in-app push notifications yet.</span></span> 

> [!NOTE]
> <span data-ttu-id="8b65f-111">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="8b65f-111">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="8b65f-112">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="8b65f-112">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="8b65f-113">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span><span class="sxs-lookup"><span data-stu-id="8b65f-113">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fmobile-engagement-web-app-get-started).</span></span>
> 
> 

## <a name="setup-mobile-engagement-for-your-web-app"></a><span data-ttu-id="8b65f-114">Konfigurera Mobile Engagement för din Web-app</span><span class="sxs-lookup"><span data-stu-id="8b65f-114">Setup Mobile Engagement for your Web app</span></span>
[!INCLUDE [Create Mobile Engagement App in Portal](../../includes/mobile-engagement-create-app-in-portal-new.md)]

## <span data-ttu-id="8b65f-115"><a id="connecting-app"></a>Ansluta appen toohello Mobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="8b65f-115"><a id="connecting-app"></a>Connect your app toohello Mobile Engagement backend</span></span>
<span data-ttu-id="8b65f-116">Den här kursen behandlas en ”grundläggande integration”, vilket är hello minsta uppsättningen som krävs toocollect data.</span><span class="sxs-lookup"><span data-stu-id="8b65f-116">This tutorial presents a "basic integration," which is hello minimal set required toocollect data.</span></span>

<span data-ttu-id="8b65f-117">Vi skapar en enkel webbapp med Visual Studio toodemonstrate hello integration trots att du kan följa hello steg med alla webbprogram som skapats utanför Visual Studio också.</span><span class="sxs-lookup"><span data-stu-id="8b65f-117">We will create a basic web app with Visual Studio toodemonstrate hello integration though you can follow hello steps with any web application created outside of Visual Studio also.</span></span> 

### <a name="create-a-new-web-app"></a><span data-ttu-id="8b65f-118">Skapa en ny Web-app</span><span class="sxs-lookup"><span data-stu-id="8b65f-118">Create a new Web App</span></span>
<span data-ttu-id="8b65f-119">hello förutsätter följande steg hello användning av Visual Studio 2015 om hello stegen är ungefär som i tidigare versioner av Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="8b65f-119">hello following steps assume hello use of Visual Studio 2015 though hello steps are similar in earlier versions of Visual Studio.</span></span> 

1. <span data-ttu-id="8b65f-120">Starta Visual Studio och i hello **Start** väljer **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="8b65f-120">Start Visual Studio, and in hello **Home** screen, select **New Project**.</span></span>
2. <span data-ttu-id="8b65f-121">Välj i popup-fönstret hello **Web** -> **ASP.Net-webbprogram**.</span><span class="sxs-lookup"><span data-stu-id="8b65f-121">In hello pop-up, select **Web** -> **ASP.Net Web Application**.</span></span> <span data-ttu-id="8b65f-122">Fyll i hello app **namn**, **plats** och **lösningsnamn**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8b65f-122">Fill in hello app **Name**, **Location** and  **Solution name**, and then click **OK**.</span></span>
3. <span data-ttu-id="8b65f-123">I hello **Välj en mall** popup-fönstret Välj **tom** under **ASP.Net 4.5 mallar** och på **OK**.</span><span class="sxs-lookup"><span data-stu-id="8b65f-123">In hello **Select a template** popup, select **Empty** under **ASP.Net 4.5 Templates** and click **OK**.</span></span> 

<span data-ttu-id="8b65f-124">Nu har du skapat ett nytt tomt Web App-projekt där vi integrerar hello Azure Mobile Engagement Web SDK.</span><span class="sxs-lookup"><span data-stu-id="8b65f-124">You have now created a new blank Web App project into which we will integrate hello Azure Mobile Engagement Web SDK.</span></span>

### <a name="connect-your-app-toomobile-engagement-backend"></a><span data-ttu-id="8b65f-125">Ansluta appen tooMobile Engagement-serverdelen</span><span class="sxs-lookup"><span data-stu-id="8b65f-125">Connect your app tooMobile Engagement backend</span></span>
1. <span data-ttu-id="8b65f-126">Skapa en ny mapp som kallas **javascript** i din lösning och Lägg till hello Web SDK JS filen **azure engagement.js** till den.</span><span class="sxs-lookup"><span data-stu-id="8b65f-126">Create a new folder called **javascript** in your solution and add hello Web SDK JS file **azure-engagement.js** into it.</span></span> 
2. <span data-ttu-id="8b65f-127">Lägg till en ny fil med namnet **main.js** i den här javascript-mappen med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="8b65f-127">Add a new file called **main.js** in this javascript folder with hello following code.</span></span> <span data-ttu-id="8b65f-128">Se till att tooupdate hello anslutningssträngen.</span><span class="sxs-lookup"><span data-stu-id="8b65f-128">Make sure tooupdate hello connection string.</span></span> <span data-ttu-id="8b65f-129">Detta `azureEngagement` objektet kommer att använda tooaccess Web SDK-metoder.</span><span class="sxs-lookup"><span data-stu-id="8b65f-129">This `azureEngagement` object will be used tooaccess Web SDK methods.</span></span> 
   
        var azureEngagement = {
            debug: true,
            connectionString: 'xxxxx'
        };
   
    ![Visual Studio med js-filer][1]

## <a name="enable-real-time-monitoring"></a><span data-ttu-id="8b65f-131">Aktivera realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="8b65f-131">Enable real-time monitoring</span></span>
<span data-ttu-id="8b65f-132">Du måste skicka minst en aktivitet toohello Mobile Engagement-serverdelen i ordning toostart skicka data och se till att hello användarna är aktiva.</span><span class="sxs-lookup"><span data-stu-id="8b65f-132">In order toostart sending data and ensuring that hello users are active, you must send at least one Activity toohello Mobile Engagement backend.</span></span> <span data-ttu-id="8b65f-133">En aktivitet i hello kontexten för ett webbprogram är en webbsida.</span><span class="sxs-lookup"><span data-stu-id="8b65f-133">An activity in hello context of a web app is a web page.</span></span> 

1. <span data-ttu-id="8b65f-134">Skapa en ny sida som kallas **home.html** i din lösning och ange den som hello startar sidan för webbappen.</span><span class="sxs-lookup"><span data-stu-id="8b65f-134">Create a new page called **home.html** in your solution and set it as hello starting page for your web app.</span></span> 
2. <span data-ttu-id="8b65f-135">Inkludera hello två JavaScript-skript vi har lagt till tidigare i den här sidan genom att lägga till följande hello inom hello body-taggen.</span><span class="sxs-lookup"><span data-stu-id="8b65f-135">Include hello two javascripts we added earlier in this page by adding hello following within hello body tag.</span></span> 
   
        <script type="text/javascript" src="javascript/main.js"></script>
        <script type="text/javascript" src="javascript/azure-engagement.js"></script>
3. <span data-ttu-id="8b65f-136">Uppdatera hello brödtext taggen toocall Engagementagent's `startActivity` metod</span><span class="sxs-lookup"><span data-stu-id="8b65f-136">Update hello body tag toocall EngagementAgent's `startActivity` method</span></span>
   
        <body onload="engagement.agent.startActivity('Home')">
4. <span data-ttu-id="8b65f-137">Så här kommer din **home.html** att se ut</span><span class="sxs-lookup"><span data-stu-id="8b65f-137">Here is what your **home.html** will look like</span></span>
   
        <html>
        <head>
            ...
        </head>
        <body onload="engagement.agent.startActivity('Home')">
            <script type="text/javascript" src="javascript/main.js"></script>
            <script type="text/javascript" src="javascript/azure-engagement.js"></script>
        </body>
        </html>

## <a name="connect-app-with-real-time-monitoring"></a><span data-ttu-id="8b65f-138">Anslut appen med realtidsövervakning</span><span class="sxs-lookup"><span data-stu-id="8b65f-138">Connect app with real-time monitoring</span></span>
[!INCLUDE [Connect app with real-time monitoring](../../includes/mobile-engagement-connect-app-with-monitor.md)]

  ![][2]

## <a name="extend-analytics"></a><span data-ttu-id="8b65f-139">Utökad analys</span><span class="sxs-lookup"><span data-stu-id="8b65f-139">Extend analytics</span></span>
<span data-ttu-id="8b65f-140">Här är alla hello-metoder som är tillgängliga med webbtjänst-SDK som du kan använda för analys av:</span><span class="sxs-lookup"><span data-stu-id="8b65f-140">Here are all hello methods currently available with Web SDK that you can use for analytics:</span></span>

1. <span data-ttu-id="8b65f-141">Aktiviteter/webbsidor:</span><span class="sxs-lookup"><span data-stu-id="8b65f-141">Activities/Web pages:</span></span>
   
        engagement.agent.startActivity(name);
        engagement.agent.endActivity();
2. <span data-ttu-id="8b65f-142">Händelser</span><span class="sxs-lookup"><span data-stu-id="8b65f-142">Events</span></span>
   
        engagement.agent.sendEvent(name, extras);
3. <span data-ttu-id="8b65f-143">Fel</span><span class="sxs-lookup"><span data-stu-id="8b65f-143">Errors</span></span>
   
        engagement.agent.sendError(name, extras);
4. <span data-ttu-id="8b65f-144">Jobb</span><span class="sxs-lookup"><span data-stu-id="8b65f-144">Jobs</span></span>
   
        engagement.agent.startJob(name);
        engagement.agent.endJob(name);

<!-- Images. -->
[1]: ./media/mobile-engagement-web-app-get-started/visual-studio-solution-js.png
[2]: ./media/mobile-engagement-web-app-get-started/session.png

