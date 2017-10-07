---
title: aaaAzure Mobile Engagement Web SDK integration | Microsoft Docs
description: "Hej senaste uppdateringarna och procedurer för hello Azure Mobile Engagement Web SDK"
services: mobile-engagement
documentationcenter: mobile
author: piyushjo
manager: erikre
editor: 
ms.assetid: b5daa2a2-942b-489d-aa1d-568c3b25e56f
ms.service: mobile-engagement
ms.workload: mobile
ms.tgt_pltfrm: web
ms.devlang: js
ms.topic: article
ms.date: 02/29/2016
ms.author: piyushjo
ms.openlocfilehash: 99613b68b615bec4ddcfcc8e4e0133ce9d887bad
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="integrate-azure-mobile-engagement-in-a-web-application"></a><span data-ttu-id="fd86e-103">Integrera Azure Mobile Engagement i ett webbprogram</span><span class="sxs-lookup"><span data-stu-id="fd86e-103">Integrate Azure Mobile Engagement in a web application</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fd86e-104">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="fd86e-104">Windows Universal</span></span>](mobile-engagement-windows-store-integrate-engagement.md)
> * [<span data-ttu-id="fd86e-105">Windows Phone Silverlight</span><span class="sxs-lookup"><span data-stu-id="fd86e-105">Windows Phone Silverlight</span></span>](mobile-engagement-windows-phone-integrate-engagement.md)
> * [<span data-ttu-id="fd86e-106">iOS</span><span class="sxs-lookup"><span data-stu-id="fd86e-106">iOS</span></span>](mobile-engagement-ios-integrate-engagement.md)
> * [<span data-ttu-id="fd86e-107">Android</span><span class="sxs-lookup"><span data-stu-id="fd86e-107">Android</span></span>](mobile-engagement-android-integrate-engagement.md)
> 
> 

<span data-ttu-id="fd86e-108">hello beskrivs i den här artikeln hello enklaste sättet tooactivate hello analyser och övervakningsfunktionerna i Azure Mobile Engagement i ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="fd86e-108">hello procedures in this article describe hello simplest way tooactivate hello analytics and monitoring functions in Azure Mobile Engagement in your web application.</span></span>

<span data-ttu-id="fd86e-109">Följ hello steg tooactivate hello loggen rapporter som är nödvändiga toocompute all statistik om användare, sessioner, aktiviteter, krascher och technicals.</span><span class="sxs-lookup"><span data-stu-id="fd86e-109">Follow hello steps tooactivate hello log reports that are needed toocompute all statistics about users, sessions, activities, crashes, and technicals.</span></span> <span data-ttu-id="fd86e-110">För program-beroende statistik, till exempel händelser, fel och jobb, måste du aktivera loggen rapporter manuellt med hjälp av hello Azure Mobile Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="fd86e-110">For application-dependent statistics, such as events, errors, and jobs, you must activate log reports manually by using hello Azure Mobile Engagement API.</span></span> <span data-ttu-id="fd86e-111">Mer information lär du dig [hur toouse hello avancerade Mobile Engagement API-märkning i ett webbprogram](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="fd86e-111">For more information, learn [how toouse hello advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="introduction"></a><span data-ttu-id="fd86e-112">Introduktion</span><span class="sxs-lookup"><span data-stu-id="fd86e-112">Introduction</span></span>
<span data-ttu-id="fd86e-113">[Hämta hello Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).</span><span class="sxs-lookup"><span data-stu-id="fd86e-113">[Download hello Azure Mobile Engagement Web SDK](http://aka.ms/P7b453).</span></span>
<span data-ttu-id="fd86e-114">hello Mobile Engagement Web SDK levereras som en JavaScript-fil, azure-engagement.js som du har tooinclude på varje sida i din webbplats eller programmet.</span><span class="sxs-lookup"><span data-stu-id="fd86e-114">hello Mobile Engagement Web SDK is shipped as a single JavaScript file, azure-engagement.js, which you have tooinclude in each page of your site or web application.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="fd86e-115">Innan du kör det här skriptet måste du köra ett skript eller kod fragment som du skriver tooconfigure Mobile Engagement för ditt program.</span><span class="sxs-lookup"><span data-stu-id="fd86e-115">Before you run this script, you must run a script or code snippet that you write tooconfigure Mobile Engagement for your application.</span></span>
> 
> 

## <a name="browser-compatibility"></a><span data-ttu-id="fd86e-116">Webbläsarkompatibilitet</span><span class="sxs-lookup"><span data-stu-id="fd86e-116">Browser compatibility</span></span>
<span data-ttu-id="fd86e-117">hello Mobile Engagement Web SDK använder inbyggd JSON kodning och avkodning dessutom toocross domän AJAX-begäranden (förlita dig på hello W3C CORS specification).</span><span class="sxs-lookup"><span data-stu-id="fd86e-117">hello Mobile Engagement Web SDK uses native JSON encoding and decoding, in addition toocross-domain AJAX requests (relying on hello W3C CORS specification).</span></span> <span data-ttu-id="fd86e-118">Den är kompatibel med hello följande webbläsare:</span><span class="sxs-lookup"><span data-stu-id="fd86e-118">It's compatible with hello following browsers:</span></span>

* <span data-ttu-id="fd86e-119">Microsoft Edge 12 +</span><span class="sxs-lookup"><span data-stu-id="fd86e-119">Microsoft Edge 12+</span></span>
* <span data-ttu-id="fd86e-120">Internet Explorer 10 +</span><span class="sxs-lookup"><span data-stu-id="fd86e-120">Internet Explorer 10+</span></span>
* <span data-ttu-id="fd86e-121">Firefox 3.5 +</span><span class="sxs-lookup"><span data-stu-id="fd86e-121">Firefox 3.5+</span></span>
* <span data-ttu-id="fd86e-122">Chrome 4 +</span><span class="sxs-lookup"><span data-stu-id="fd86e-122">Chrome 4+</span></span>
* <span data-ttu-id="fd86e-123">Safari 6 +</span><span class="sxs-lookup"><span data-stu-id="fd86e-123">Safari 6+</span></span>
* <span data-ttu-id="fd86e-124">Opera 12 +</span><span class="sxs-lookup"><span data-stu-id="fd86e-124">Opera 12+</span></span>

## <a name="configure-mobile-engagement"></a><span data-ttu-id="fd86e-125">Konfigurera Mobile Engagement</span><span class="sxs-lookup"><span data-stu-id="fd86e-125">Configure Mobile Engagement</span></span>
<span data-ttu-id="fd86e-126">Skriva ett skript som skapar en global `azureEngagement` JavaScript-objekt enligt följande exempel hello.</span><span class="sxs-lookup"><span data-stu-id="fd86e-126">Write a script that creates a global `azureEngagement` JavaScript object, as in hello following example.</span></span> <span data-ttu-id="fd86e-127">Eftersom din plats kan ha flera sidor, förutsätter det här exemplet att det här skriptet ingår i varje sida.</span><span class="sxs-lookup"><span data-stu-id="fd86e-127">Because your site might have multiples pages, this example assumes that this script is included in every page.</span></span> <span data-ttu-id="fd86e-128">I det här exemplet hello JavaScript-objekt med namnet `azure-engagement-conf.js`.</span><span class="sxs-lookup"><span data-stu-id="fd86e-128">In this example, hello JavaScript object is named `azure-engagement-conf.js`.</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
    };

<span data-ttu-id="fd86e-129">Hej `connectionString` värdet för ditt program visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fd86e-129">hello `connectionString` value for your application is displayed in hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="fd86e-130">`appVersionName`och `appVersionCode` är valfria.</span><span class="sxs-lookup"><span data-stu-id="fd86e-130">`appVersionName` and `appVersionCode` are optional.</span></span> <span data-ttu-id="fd86e-131">Vi rekommenderar dock att du konfigurerar dem så att analytics kan bearbeta versionsinformation.</span><span class="sxs-lookup"><span data-stu-id="fd86e-131">However, we recommend that you configure them so that analytics can process version information.</span></span>
> 
> 

## <a name="include-mobile-engagement-scripts-in-your-pages"></a><span data-ttu-id="fd86e-132">Inkludera Mobile Engagement skript i sidorna</span><span class="sxs-lookup"><span data-stu-id="fd86e-132">Include Mobile Engagement scripts in your pages</span></span>
<span data-ttu-id="fd86e-133">Lägga till Mobile Engagement skript tooyour sidor i något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="fd86e-133">Add Mobile Engagement scripts tooyour pages in one of hello following ways:</span></span>

    <head>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </head>

<span data-ttu-id="fd86e-134">Eller:</span><span class="sxs-lookup"><span data-stu-id="fd86e-134">Or this:</span></span>

    <body>
      ...
      <script type="text/javascript" src="azure-engagement-conf.js"></script>
      <script type="text/javascript" src="azure-engagement.js"></script>
      ...
    </body>

## <a name="alias"></a><span data-ttu-id="fd86e-135">Alias</span><span class="sxs-lookup"><span data-stu-id="fd86e-135">Alias</span></span>
<span data-ttu-id="fd86e-136">Efter hello Mobile Engagement Web SDK skript har lästs in, skapar den hello **engagement** alias tooaccess hello SDK-API: er.</span><span class="sxs-lookup"><span data-stu-id="fd86e-136">After hello Mobile Engagement Web SDK script is loaded, it creates hello **engagement** alias tooaccess hello SDK APIs.</span></span> <span data-ttu-id="fd86e-137">Du kan inte använda den här alias toodefine hello SDK-konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fd86e-137">You cannot use this alias toodefine hello SDK configuration.</span></span> <span data-ttu-id="fd86e-138">Det här aliaset används som en referens i den här dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="fd86e-138">This alias is used as a reference in this documentation.</span></span>

<span data-ttu-id="fd86e-139">Observera att om hello Standardalias i konflikt med en annan global variabel från din sida, du kan ändra den i hello konfiguration enligt följande innan du läser in hello Mobile Engagement Web SDK:</span><span class="sxs-lookup"><span data-stu-id="fd86e-139">Note that if hello default alias conflicts with another global variable from your page, you can redefine it in hello configuration as follows before you load hello Mobile Engagement Web SDK:</span></span>

    window.azureEngagement = {
      connectionString: 'Endpoint={appCollection}.{domain};AppId={appId};SdkKey={sdkKey}',
      appVersionName: '1.0.0',
      appVersionCode: 1
      alias:'anotherAlias'
    };

## <a name="basic-reporting"></a><span data-ttu-id="fd86e-140">Grundläggande reporting</span><span class="sxs-lookup"><span data-stu-id="fd86e-140">Basic reporting</span></span>
<span data-ttu-id="fd86e-141">Grundläggande rapportering i Mobile Engagement innehåller statistik för session-nivå, till exempel statistik om användare, sessioner, aktiviteter och krascher.</span><span class="sxs-lookup"><span data-stu-id="fd86e-141">Basic reporting in Mobile Engagement covers session-level statistics, such as statistics about users, sessions, activities, and crashes.</span></span>

### <a name="session-tracking"></a><span data-ttu-id="fd86e-142">Sessionsspårning</span><span class="sxs-lookup"><span data-stu-id="fd86e-142">Session tracking</span></span>
<span data-ttu-id="fd86e-143">En Mobile Engagement-session är uppdelad i en serie aktiviteter, alla identifierade med ett namn.</span><span class="sxs-lookup"><span data-stu-id="fd86e-143">A Mobile Engagement session is divided into a sequence of activities, each identified by a name.</span></span>

<span data-ttu-id="fd86e-144">I klassiska webbplats rekommenderar vi att du deklarera en annan aktivitet på varje sida i din webbplats.</span><span class="sxs-lookup"><span data-stu-id="fd86e-144">In a classic website, we recommend that you declare a different activity on each page of your site.</span></span> <span data-ttu-id="fd86e-145">För en webbplatsen eller programmet i vilka hello sidan aldrig ändras, kanske du vill tootrack hello aktiviteter i mindre skala, såsom hello-sidan.</span><span class="sxs-lookup"><span data-stu-id="fd86e-145">For a website or web application in which hello current page never changes, you might want tootrack hello activities on a smaller scale, such as within hello page.</span></span>

<span data-ttu-id="fd86e-146">Antingen sätt, toostart eller ändra hello aktuella användaraktivitet, anrop hello `engagement.agent.startActivity` funktion.</span><span class="sxs-lookup"><span data-stu-id="fd86e-146">Either way, toostart or change hello current user activity, call hello `engagement.agent.startActivity` function.</span></span> <span data-ttu-id="fd86e-147">Exempel:</span><span class="sxs-lookup"><span data-stu-id="fd86e-147">For example:</span></span>

    <body onload="yourOnload()">

    <!-- -->

    yourOnload = function() {
      [...]
      engagement.agent.startActivity('welcome');
    };

<span data-ttu-id="fd86e-148">hello Mobile Engagement-servern avslutar automatiskt en öppen session inom tre minuter efter hello appen på sidan är stängd.</span><span class="sxs-lookup"><span data-stu-id="fd86e-148">hello Mobile Engagement server automatically ends an open session within three minutes after hello application page is closed.</span></span>

<span data-ttu-id="fd86e-149">Alternativt kan du avsluta en session manuellt genom att anropa `engagement.agent.endActivity`.</span><span class="sxs-lookup"><span data-stu-id="fd86e-149">Alternatively, you can end a session manually by calling `engagement.agent.endActivity`.</span></span> <span data-ttu-id="fd86e-150">Anger hello aktuella användare aktiviteten too'Idle ”.</span><span class="sxs-lookup"><span data-stu-id="fd86e-150">This sets hello current user activity too'Idle.'</span></span>  <span data-ttu-id="fd86e-151">hello-sessionen avslutas 10 sekunder senare om ett nytt anrop för`engagement.agent.startActivity` återupptar hello-sessionen.</span><span class="sxs-lookup"><span data-stu-id="fd86e-151">hello session will end 10 seconds later unless a new call too`engagement.agent.startActivity` resumes hello session.</span></span>

<span data-ttu-id="fd86e-152">Du kan konfigurera hello 10 sekunder fördröjning i hello globala engagement objekt på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="fd86e-152">You can configure hello 10-second delay in hello global engagement object, as follows:</span></span>

    engagement.sessionTimeout = 2000; // 2 seconds
    // or
    engagement.sessionTimeout = 0; // end hello session as soon as endActivity is called

> [!NOTE]
> <span data-ttu-id="fd86e-153">Du kan inte använda `engagement.agent.endActivity` i hello `onunload` motringning eftersom du inte göra AJAX-anrop i det här skedet.</span><span class="sxs-lookup"><span data-stu-id="fd86e-153">You cannot use `engagement.agent.endActivity` in hello `onunload` callback because you cannot make AJAX calls at this stage.</span></span>
> 
> 

## <a name="advanced-reporting"></a><span data-ttu-id="fd86e-154">Avancerad rapportering</span><span class="sxs-lookup"><span data-stu-id="fd86e-154">Advanced reporting</span></span>
<span data-ttu-id="fd86e-155">Om du vill tooreport programspecifika händelser, fel och jobb, måste du eventuellt toouse hello Mobile Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="fd86e-155">Optionally, if you want tooreport application-specific events, errors, and jobs, you need toouse hello Mobile Engagement API.</span></span> <span data-ttu-id="fd86e-156">Du kommer åt hello Mobile Engagement API via hello `engagement.agent` objekt.</span><span class="sxs-lookup"><span data-stu-id="fd86e-156">You access hello Mobile Engagement API through hello `engagement.agent` object.</span></span>

<span data-ttu-id="fd86e-157">Du kan komma åt alla hello avancerade funktioner i Mobile Engagement i hello Mobile Engagement-API.</span><span class="sxs-lookup"><span data-stu-id="fd86e-157">You can access all of hello advanced capabilities in Mobile Engagement in hello Mobile Engagement API.</span></span> <span data-ttu-id="fd86e-158">hello API beskrivs i artikel hello [hur toouse hello avancerade Mobile Engagement API-märkning i ett webbprogram](mobile-engagement-web-use-engagement-api.md).</span><span class="sxs-lookup"><span data-stu-id="fd86e-158">hello API is detailed in hello article [How toouse hello advanced Mobile Engagement tagging API in a web application](mobile-engagement-web-use-engagement-api.md).</span></span>

## <a name="customize-hello-urls-used-for-ajax-calls"></a><span data-ttu-id="fd86e-159">Anpassa hello URL: er används för AJAX-anrop</span><span class="sxs-lookup"><span data-stu-id="fd86e-159">Customize hello URLs used for AJAX calls</span></span>
<span data-ttu-id="fd86e-160">Du kan anpassa URL: er som hello Mobile Engagement Web SDK använder.</span><span class="sxs-lookup"><span data-stu-id="fd86e-160">You can customize URLs that hello Mobile Engagement Web SDK uses.</span></span> <span data-ttu-id="fd86e-161">Till exempel tooredefine hello loggnings-URL (hello SDK slutpunkten för loggning), kan du åsidosätta hello konfiguration som detta:</span><span class="sxs-lookup"><span data-stu-id="fd86e-161">For example, tooredefine hello log URL (hello SDK endpoint for logging), you can override hello configuration like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...        
        getLoggerUrl: function() {
        return 'someProxy/log';
        }
      }
    };

<span data-ttu-id="fd86e-162">Om URL-funktioner returnerar en sträng som börjar med `/`, `//`, `http://`, eller `https://`, hello standardschema används inte.</span><span class="sxs-lookup"><span data-stu-id="fd86e-162">If your URL functions return a string that begins with `/`, `//`, `http://`, or `https://`, hello default scheme is not used.</span></span> <span data-ttu-id="fd86e-163">Som standard hello `https://` schema används för dessa URL: er.</span><span class="sxs-lookup"><span data-stu-id="fd86e-163">By default, hello `https://` scheme is used for those URLs.</span></span> <span data-ttu-id="fd86e-164">Om du vill toocustomize hello standardschema åsidosätta hello konfiguration, så här:</span><span class="sxs-lookup"><span data-stu-id="fd86e-164">If you want toocustomize hello default scheme, override hello configuration, like this:</span></span>

    window.azureEngagement = {
      ...
      urls: {
        ...         
        scheme: '//'
      }
    };
