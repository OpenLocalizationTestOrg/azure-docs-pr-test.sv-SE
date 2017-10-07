---
title: aaaTroubleshoot analyser i Azure Application Insights | Microsoft Docs
description: "Problem med Application Insights analytics? Börja här. "
services: application-insights
documentationcenter: 
author: CFreemanwa
manager: carmonm
ms.assetid: 9bbd5859-3584-4d80-9b6d-d5910fa48baa
ms.service: application-insights
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: na
ms.topic: article
ms.date: 07/11/2016
ms.author: bwren
ms.openlocfilehash: e3be2fbc0237440d3b8a736484434a9f276296c7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a><span data-ttu-id="fafc2-104">Felsökningsanalys i Application Insights</span><span class="sxs-lookup"><span data-stu-id="fafc2-104">Troubleshoot Analytics in Application Insights</span></span>
<span data-ttu-id="fafc2-105">Problem med [Application Insights Analytics](app-insights-analytics.md)?</span><span class="sxs-lookup"><span data-stu-id="fafc2-105">Problems with [Application Insights Analytics](app-insights-analytics.md)?</span></span> <span data-ttu-id="fafc2-106">Börja här.</span><span class="sxs-lookup"><span data-stu-id="fafc2-106">Start here.</span></span> <span data-ttu-id="fafc2-107">Analytics är hello kraftfullt sökverktyg av Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="fafc2-107">Analytics is hello powerful search tool of Azure Application Insights.</span></span>

## <a name="limits"></a><span data-ttu-id="fafc2-108">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="fafc2-108">Limits</span></span>
* <span data-ttu-id="fafc2-109">För närvarande är frågeresultat begränsad toojust över en vecka med senaste data.</span><span class="sxs-lookup"><span data-stu-id="fafc2-109">At present, query results are limited toojust over a week of past data.</span></span>
* <span data-ttu-id="fafc2-110">Webbläsare som vi testa på: senaste versionerna av Chrome, kant och Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="fafc2-110">Browsers we test on: latest editions of Chrome, Edge, and Internet Explorer.</span></span>

## <a name="known-incompatible-browser-extensions"></a><span data-ttu-id="fafc2-111">Kända inkompatibla webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="fafc2-111">Known incompatible browser extensions</span></span>
* <span data-ttu-id="fafc2-112">Ghostery</span><span class="sxs-lookup"><span data-stu-id="fafc2-112">Ghostery</span></span>

<span data-ttu-id="fafc2-113">Inaktivera tillägget hello eller Använd en annan webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fafc2-113">Disable hello extension or use a different browser.</span></span>

## <span data-ttu-id="fafc2-114"><a name="e-a"></a>”Fel”</span><span class="sxs-lookup"><span data-stu-id="fafc2-114"><a name="e-a"></a> "Unexpected error"</span></span>
![Oväntat felskärmen](./media/app-insights-analytics-troubleshooting/010.png)

<span data-ttu-id="fafc2-116">Internt fel inträffade under portal runtime-undantag.</span><span class="sxs-lookup"><span data-stu-id="fafc2-116">Internal error occurred during portal runtime – unhandled exception.</span></span>

* <span data-ttu-id="fafc2-117">Rensa hello webbläsarens cacheminne.</span><span class="sxs-lookup"><span data-stu-id="fafc2-117">Clean hello browser's cache.</span></span> 

## <span data-ttu-id="fafc2-118"><a name="e-b"></a>403... Försök tooreload</span><span class="sxs-lookup"><span data-stu-id="fafc2-118"><a name="e-b"></a>403 ... please try tooreload</span></span>
![403... Försök tooreload](./media/app-insights-analytics-troubleshooting/020.png)

<span data-ttu-id="fafc2-120">En autentisering relaterade fel uppstod (under autentisering eller under åtkomst-token generering).</span><span class="sxs-lookup"><span data-stu-id="fafc2-120">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="fafc2-121">hello-portalen kan ha något sätt återställa för utan att ändra inställningarna för webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="fafc2-121">hello portal may have no way too recover without changing browser settings.</span></span>

* <span data-ttu-id="fafc2-122">Kontrollera [cookies från tredje part är aktiverade](#cookies) i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fafc2-122">Verify [third party cookies are enabled](#cookies) in hello browser.</span></span> 

## <span data-ttu-id="fafc2-123"><a name="authentication"></a>403... Kontrollera säkerhetszon</span><span class="sxs-lookup"><span data-stu-id="fafc2-123"><a name="authentication"></a>403 ... verify security zone</span></span>
![403.. .verify säkerhetszon](./media/app-insights-analytics-troubleshooting/030.png)

<span data-ttu-id="fafc2-125">En autentisering relaterade fel uppstod (under autentisering eller under åtkomst-token generering).</span><span class="sxs-lookup"><span data-stu-id="fafc2-125">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="fafc2-126">hello-portalen kan ha något sätt återställa för utan att ändra inställningarna för webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="fafc2-126">hello portal may have no way too recover without changing browser settings.</span></span>

1. <span data-ttu-id="fafc2-127">Kontrollera [cookies från tredje part är aktiverade](#cookies) i hello webbläsare.</span><span class="sxs-lookup"><span data-stu-id="fafc2-127">Verify [third party cookies are enabled](#cookies) in hello browser.</span></span> 
2. <span data-ttu-id="fafc2-128">Använde du en favorit, bokmärke eller en sparad länk tooopen hello Analytics portal?</span><span class="sxs-lookup"><span data-stu-id="fafc2-128">Did you use a favorite, bookmark or saved link tooopen hello Analytics portal?</span></span> <span data-ttu-id="fafc2-129">Är du loggade in med andra autentiseringsuppgifter än som du använde när du sparade hello länken?</span><span class="sxs-lookup"><span data-stu-id="fafc2-129">Are you signed in with different credentials than you used when you saved hello link?</span></span>
3. <span data-ttu-id="fafc2-130">Försök med en i-privat/incognito webbläsarfönster (när du stänger alla fönster).</span><span class="sxs-lookup"><span data-stu-id="fafc2-130">Try using an in-private/incognito browser window (after closing all such windows).</span></span> <span data-ttu-id="fafc2-131">Har du tooprovide dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fafc2-131">You'll have tooprovide your credentials.</span></span> 
4. <span data-ttu-id="fafc2-132">Öppna ett nytt webbläsarfönster som (vanlig) och gå för[Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fafc2-132">Open another (ordinary) browser window and go too[Azure](https://portal.azure.com).</span></span> <span data-ttu-id="fafc2-133">Logga ut. Öppna länken och logga in med hello rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fafc2-133">Sign out. Then open your link and sign in with hello correct credentials.</span></span>
5. <span data-ttu-id="fafc2-134">Edge- och Internet Explorer-användare kan också få det här felet när betrodd Zoninställningar inte stöds.</span><span class="sxs-lookup"><span data-stu-id="fafc2-134">Edge and Internet Explorer users can also get this error when trusted zone settings are not supported.</span></span>
   
    <span data-ttu-id="fafc2-135">Kontrollera att båda [Analytics-portalen](https://analytics.applicationinsights.io) och [Azure Active Directory-portalen](https://portal.azure.com) i hello samma säkerhetszon:</span><span class="sxs-lookup"><span data-stu-id="fafc2-135">Verify both [Analytics portal](https://analytics.applicationinsights.io) and [Azure Active Directory portal](https://portal.azure.com) are in hello same security zone:</span></span>
   
   * <span data-ttu-id="fafc2-136">I Internet Explorer, öppna **Internetalternativ**, **säkerhet**, **tillförlitliga platser**, **platser**:</span><span class="sxs-lookup"><span data-stu-id="fafc2-136">In Internet Explorer, open **Internet Options**, **Security**, **Trusted sites**, **Sites**:</span></span>
     
     ![Dialogrutan Internetalternativ, lägga till en plats tooTrusted platser](./media/app-insights-analytics-troubleshooting/033.png)
     
     <span data-ttu-id="fafc2-138">I listan över hello-webbplatser, om någon av följande URL: er hello ingår Kontrollera att hello andra ingår också:</span><span class="sxs-lookup"><span data-stu-id="fafc2-138">In hello Websites list, if any of hello following URLs are included, make sure that hello others are included also:</span></span>
     
     <span data-ttu-id="fafc2-139">https://Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="fafc2-139">https://analytics.applicationinsights.io</span></span><br/>
     <span data-ttu-id="fafc2-140">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="fafc2-140">https://login.microsoftonline.com</span></span><br/>
     <span data-ttu-id="fafc2-141">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="fafc2-141">https://login.windows.net</span></span>

## <span data-ttu-id="fafc2-142"><a name="e-d"></a>404 ... Resursen hittades inte</span><span class="sxs-lookup"><span data-stu-id="fafc2-142"><a name="e-d"></a>404 ... Resource not found</span></span>
![404... resursen hittades inte](./media/app-insights-analytics-troubleshooting/040.png)

<span data-ttu-id="fafc2-144">Programresursen har tagits bort från Application Insights och är inte tillgänglig längre.</span><span class="sxs-lookup"><span data-stu-id="fafc2-144">Application resource was deleted from Application Insights and isn’t available anymore.</span></span> <span data-ttu-id="fafc2-145">Detta kan inträffa om du har sparat hello URL toohello Analytics-sida.</span><span class="sxs-lookup"><span data-stu-id="fafc2-145">This can happen if you saved hello URL toohello Analytics page.</span></span>

## <span data-ttu-id="fafc2-146"><a name="e-e"></a>403 ... Ingen åtkomstbehörighet för</span><span class="sxs-lookup"><span data-stu-id="fafc2-146"><a name="e-e"></a>403 ... No authorization</span></span>
![403... inte behörighet](./media/app-insights-analytics-troubleshooting/050.png)

<span data-ttu-id="fafc2-148">Du har inte behörighet tooopen det här programmet i Analytics.</span><span class="sxs-lookup"><span data-stu-id="fafc2-148">You don't have permission tooopen this application in Analytics.</span></span>

* <span data-ttu-id="fafc2-149">Fick du hello länk från någon annan?</span><span class="sxs-lookup"><span data-stu-id="fafc2-149">Did you get hello link from someone else?</span></span> <span data-ttu-id="fafc2-150">Be dem att du arbetar i hello toomake [läsare eller deltagare för den här resursgruppen](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="fafc2-150">Ask them toomake sure you are in hello [readers or contributors for this resource group](app-insights-resources-roles-access-control.md).</span></span>
* <span data-ttu-id="fafc2-151">Sparades hello-länk med andra autentiseringsuppgifter?</span><span class="sxs-lookup"><span data-stu-id="fafc2-151">Did you save hello link using different credentials?</span></span> <span data-ttu-id="fafc2-152">Öppna hello [Azure-portalen](https://portal.azure.com), logga ut och försök sedan den här länken anger hello rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="fafc2-152">Open hello [Azure portal](https://portal.azure.com), sign out, and then try this link again, providing hello correct credentials.</span></span>

## <span data-ttu-id="fafc2-153"><a name="html-storage"></a>403 ... HTML5-lagring</span><span class="sxs-lookup"><span data-stu-id="fafc2-153"><a name="html-storage"></a>403 ... HTML5 Storage</span></span>
<span data-ttu-id="fafc2-154">Vår portal använder HTML5 localStorage och sessionStorage.</span><span class="sxs-lookup"><span data-stu-id="fafc2-154">Our portal uses HTML5 localStorage and sessionStorage.</span></span>

* <span data-ttu-id="fafc2-155">Chrome: Inställningar, sekretess, inställningar för innehåll.</span><span class="sxs-lookup"><span data-stu-id="fafc2-155">Chrome: Settings, privacy, content settings.</span></span>
* <span data-ttu-id="fafc2-156">Internet Explorer: Internet-alternativ, fliken Avancerat, säkerhet, aktivera DOM-lagring</span><span class="sxs-lookup"><span data-stu-id="fafc2-156">Internet Explorer: Internet Options, Advanced tab, Security, Enable DOM Storage</span></span>

![403... Försök tooenable HTML5-lagring](./media/app-insights-analytics-troubleshooting/060.png)

## <span data-ttu-id="fafc2-158"><a name="e-g"></a>404 ... Det gick inte att hitta prenumerationen</span><span class="sxs-lookup"><span data-stu-id="fafc2-158"><a name="e-g"></a>404 ... Subscription not found</span></span>
![404 ... Det gick inte att hitta prenumerationen](./media/app-insights-analytics-troubleshooting/070.png)

<span data-ttu-id="fafc2-160">hello URL är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="fafc2-160">hello URL is invalid.</span></span> 

* <span data-ttu-id="fafc2-161">Öppna hello app resurs i [Application Insights-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fafc2-161">Open hello app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="fafc2-162">Använd sedan hello Analytics-knappen.</span><span class="sxs-lookup"><span data-stu-id="fafc2-162">Then use hello Analytics button.</span></span>

## <span data-ttu-id="fafc2-163"><a name="e-h"></a>404... sidan finns inte</span><span class="sxs-lookup"><span data-stu-id="fafc2-163"><a name="e-h"></a>404 ... page doesn't exist</span></span>
![404 ... Sidan finns inte](./media/app-insights-analytics-troubleshooting/080.png)

<span data-ttu-id="fafc2-165">hello URL är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="fafc2-165">hello URL is invalid.</span></span>

* <span data-ttu-id="fafc2-166">Öppna hello app resurs i [Application Insights-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="fafc2-166">Open hello app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="fafc2-167">Använd sedan hello Analytics-knappen.</span><span class="sxs-lookup"><span data-stu-id="fafc2-167">Then use hello Analytics button.</span></span>

## <span data-ttu-id="fafc2-168"><a name="cookies"></a>Aktivera cookies från tredje part</span><span class="sxs-lookup"><span data-stu-id="fafc2-168"><a name="cookies"></a>Enable third-party cookies</span></span>
  <span data-ttu-id="fafc2-169">Se [hur toodisable tredjeparts-cookies](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), men Observera att vi behöver för**aktivera** dem.</span><span class="sxs-lookup"><span data-stu-id="fafc2-169">See [how toodisable third party cookies](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), but notice we need too**enable** them.</span></span>


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

