---
title: "Felsöka analyser i Azure Application Insights | Microsoft Docs"
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
ms.openlocfilehash: 02df117908fc1790e8cfb9ec0a7218c1b8be856c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="troubleshoot-analytics-in-application-insights"></a><span data-ttu-id="33195-104">Felsökningsanalys i Application Insights</span><span class="sxs-lookup"><span data-stu-id="33195-104">Troubleshoot Analytics in Application Insights</span></span>
<span data-ttu-id="33195-105">Problem med [Application Insights Analytics](app-insights-analytics.md)?</span><span class="sxs-lookup"><span data-stu-id="33195-105">Problems with [Application Insights Analytics](app-insights-analytics.md)?</span></span> <span data-ttu-id="33195-106">Börja här.</span><span class="sxs-lookup"><span data-stu-id="33195-106">Start here.</span></span> <span data-ttu-id="33195-107">Analytics är ett kraftfullt sökverktyg av Azure Application Insights.</span><span class="sxs-lookup"><span data-stu-id="33195-107">Analytics is the powerful search tool of Azure Application Insights.</span></span>

## <a name="limits"></a><span data-ttu-id="33195-108">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="33195-108">Limits</span></span>
* <span data-ttu-id="33195-109">För närvarande är frågeresultat begränsade till bara via en vecka med senaste data.</span><span class="sxs-lookup"><span data-stu-id="33195-109">At present, query results are limited to just over a week of past data.</span></span>
* <span data-ttu-id="33195-110">Webbläsare som vi testa på: senaste versionerna av Chrome, kant och Internet Explorer.</span><span class="sxs-lookup"><span data-stu-id="33195-110">Browsers we test on: latest editions of Chrome, Edge, and Internet Explorer.</span></span>

## <a name="known-incompatible-browser-extensions"></a><span data-ttu-id="33195-111">Kända inkompatibla webbläsartillägg</span><span class="sxs-lookup"><span data-stu-id="33195-111">Known incompatible browser extensions</span></span>
* <span data-ttu-id="33195-112">Ghostery</span><span class="sxs-lookup"><span data-stu-id="33195-112">Ghostery</span></span>

<span data-ttu-id="33195-113">Inaktivera tillägget eller Använd en annan webbläsare.</span><span class="sxs-lookup"><span data-stu-id="33195-113">Disable the extension or use a different browser.</span></span>

## <span data-ttu-id="33195-114"><a name="e-a"></a>”Fel”</span><span class="sxs-lookup"><span data-stu-id="33195-114"><a name="e-a"></a> "Unexpected error"</span></span>
![Oväntat felskärmen](./media/app-insights-analytics-troubleshooting/010.png)

<span data-ttu-id="33195-116">Internt fel inträffade under portal runtime-undantag.</span><span class="sxs-lookup"><span data-stu-id="33195-116">Internal error occurred during portal runtime – unhandled exception.</span></span>

* <span data-ttu-id="33195-117">Rensa webbläsarens cacheminne.</span><span class="sxs-lookup"><span data-stu-id="33195-117">Clean the browser's cache.</span></span> 

## <span data-ttu-id="33195-118"><a name="e-b"></a>403... Försök att läsa in</span><span class="sxs-lookup"><span data-stu-id="33195-118"><a name="e-b"></a>403 ... please try to reload</span></span>
![403... Försök att läsa in](./media/app-insights-analytics-troubleshooting/020.png)

<span data-ttu-id="33195-120">En autentisering relaterade fel uppstod (under autentisering eller under åtkomst-token generering).</span><span class="sxs-lookup"><span data-stu-id="33195-120">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="33195-121">Portalen har kanske ingen möjlighet att återställa utan att ändra inställningarna för webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="33195-121">The portal may have no way to  recover without changing browser settings.</span></span>

* <span data-ttu-id="33195-122">Kontrollera [cookies från tredje part är aktiverade](#cookies) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="33195-122">Verify [third party cookies are enabled](#cookies) in the browser.</span></span> 

## <span data-ttu-id="33195-123"><a name="authentication"></a>403... Kontrollera säkerhetszon</span><span class="sxs-lookup"><span data-stu-id="33195-123"><a name="authentication"></a>403 ... verify security zone</span></span>
![403.. .verify säkerhetszon](./media/app-insights-analytics-troubleshooting/030.png)

<span data-ttu-id="33195-125">En autentisering relaterade fel uppstod (under autentisering eller under åtkomst-token generering).</span><span class="sxs-lookup"><span data-stu-id="33195-125">An authentication related error occurred (during authentication or during access token generation).</span></span> <span data-ttu-id="33195-126">Portalen har kanske ingen möjlighet att återställa utan att ändra inställningarna för webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="33195-126">The portal may have no way to  recover without changing browser settings.</span></span>

1. <span data-ttu-id="33195-127">Kontrollera [cookies från tredje part är aktiverade](#cookies) i webbläsaren.</span><span class="sxs-lookup"><span data-stu-id="33195-127">Verify [third party cookies are enabled](#cookies) in the browser.</span></span> 
2. <span data-ttu-id="33195-128">Du använder en favorit, bokmärke eller sparade länken för att öppna Analytics-portalen?</span><span class="sxs-lookup"><span data-stu-id="33195-128">Did you use a favorite, bookmark or saved link to open the Analytics portal?</span></span> <span data-ttu-id="33195-129">Är du loggade in med andra autentiseringsuppgifter än som du använde när du sparade länken?</span><span class="sxs-lookup"><span data-stu-id="33195-129">Are you signed in with different credentials than you used when you saved the link?</span></span>
3. <span data-ttu-id="33195-130">Försök med en i-privat/incognito webbläsarfönster (när du stänger alla fönster).</span><span class="sxs-lookup"><span data-stu-id="33195-130">Try using an in-private/incognito browser window (after closing all such windows).</span></span> <span data-ttu-id="33195-131">Du måste ange dina autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="33195-131">You'll have to provide your credentials.</span></span> 
4. <span data-ttu-id="33195-132">Öppna ett nytt webbläsarfönster som (vanlig) och gå till [Azure](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="33195-132">Open another (ordinary) browser window and go to [Azure](https://portal.azure.com).</span></span> <span data-ttu-id="33195-133">Logga ut. Öppna din länk och logga in med rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="33195-133">Sign out. Then open your link and sign in with the correct credentials.</span></span>
5. <span data-ttu-id="33195-134">Edge- och Internet Explorer-användare kan också få det här felet när betrodd Zoninställningar inte stöds.</span><span class="sxs-lookup"><span data-stu-id="33195-134">Edge and Internet Explorer users can also get this error when trusted zone settings are not supported.</span></span>
   
    <span data-ttu-id="33195-135">Kontrollera att båda [Analytics-portalen](https://analytics.applicationinsights.io) och [Azure Active Directory-portalen](https://portal.azure.com) finns i samma säkerhetszon:</span><span class="sxs-lookup"><span data-stu-id="33195-135">Verify both [Analytics portal](https://analytics.applicationinsights.io) and [Azure Active Directory portal](https://portal.azure.com) are in the same security zone:</span></span>
   
   * <span data-ttu-id="33195-136">I Internet Explorer, öppna **Internetalternativ**, **säkerhet**, **tillförlitliga platser**, **platser**:</span><span class="sxs-lookup"><span data-stu-id="33195-136">In Internet Explorer, open **Internet Options**, **Security**, **Trusted sites**, **Sites**:</span></span>
     
     ![Dialogrutan Internetalternativ, lägger till en plats i betrodda platser](./media/app-insights-analytics-troubleshooting/033.png)
     
     <span data-ttu-id="33195-138">I listan över webbplatser om någon av följande webbadresser ingår, kontrollerar du att de andra ingår också:</span><span class="sxs-lookup"><span data-stu-id="33195-138">In the Websites list, if any of the following URLs are included, make sure that the others are included also:</span></span>
     
     <span data-ttu-id="33195-139">https://Analytics.applicationinsights.IO</span><span class="sxs-lookup"><span data-stu-id="33195-139">https://analytics.applicationinsights.io</span></span><br/>
     <span data-ttu-id="33195-140">https://login.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="33195-140">https://login.microsoftonline.com</span></span><br/>
     <span data-ttu-id="33195-141">https://login.windows.net</span><span class="sxs-lookup"><span data-stu-id="33195-141">https://login.windows.net</span></span>

## <span data-ttu-id="33195-142"><a name="e-d"></a>404 ... Resursen hittades inte</span><span class="sxs-lookup"><span data-stu-id="33195-142"><a name="e-d"></a>404 ... Resource not found</span></span>
![404... resursen hittades inte](./media/app-insights-analytics-troubleshooting/040.png)

<span data-ttu-id="33195-144">Programresursen har tagits bort från Application Insights och är inte tillgänglig längre.</span><span class="sxs-lookup"><span data-stu-id="33195-144">Application resource was deleted from Application Insights and isn’t available anymore.</span></span> <span data-ttu-id="33195-145">Detta kan inträffa om du har sparat Webbadressen till sidan Analytics.</span><span class="sxs-lookup"><span data-stu-id="33195-145">This can happen if you saved the URL to the Analytics page.</span></span>

## <span data-ttu-id="33195-146"><a name="e-e"></a>403 ... Ingen åtkomstbehörighet för</span><span class="sxs-lookup"><span data-stu-id="33195-146"><a name="e-e"></a>403 ... No authorization</span></span>
![403... inte behörighet](./media/app-insights-analytics-troubleshooting/050.png)

<span data-ttu-id="33195-148">Du har inte behörighet att öppna det här programmet i Analytics.</span><span class="sxs-lookup"><span data-stu-id="33195-148">You don't have permission to open this application in Analytics.</span></span>

* <span data-ttu-id="33195-149">Fick du länken från någon annan?</span><span class="sxs-lookup"><span data-stu-id="33195-149">Did you get the link from someone else?</span></span> <span data-ttu-id="33195-150">Be dem att se till att du befinner dig i den [läsare eller deltagare för den här resursgruppen](app-insights-resources-roles-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="33195-150">Ask them to make sure you are in the [readers or contributors for this resource group](app-insights-resources-roles-access-control.md).</span></span>
* <span data-ttu-id="33195-151">Sparades länken med andra autentiseringsuppgifter?</span><span class="sxs-lookup"><span data-stu-id="33195-151">Did you save the link using different credentials?</span></span> <span data-ttu-id="33195-152">Öppna den [Azure-portalen](https://portal.azure.com), logga ut och försök sedan den här länken anger rätt autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="33195-152">Open the [Azure portal](https://portal.azure.com), sign out, and then try this link again, providing the correct credentials.</span></span>

## <span data-ttu-id="33195-153"><a name="html-storage"></a>403 ... HTML5-lagring</span><span class="sxs-lookup"><span data-stu-id="33195-153"><a name="html-storage"></a>403 ... HTML5 Storage</span></span>
<span data-ttu-id="33195-154">Vår portal använder HTML5 localStorage och sessionStorage.</span><span class="sxs-lookup"><span data-stu-id="33195-154">Our portal uses HTML5 localStorage and sessionStorage.</span></span>

* <span data-ttu-id="33195-155">Chrome: Inställningar, sekretess, inställningar för innehåll.</span><span class="sxs-lookup"><span data-stu-id="33195-155">Chrome: Settings, privacy, content settings.</span></span>
* <span data-ttu-id="33195-156">Internet Explorer: Internet-alternativ, fliken Avancerat, säkerhet, aktivera DOM-lagring</span><span class="sxs-lookup"><span data-stu-id="33195-156">Internet Explorer: Internet Options, Advanced tab, Security, Enable DOM Storage</span></span>

![403... försöker aktivera HTML5-lagring](./media/app-insights-analytics-troubleshooting/060.png)

## <span data-ttu-id="33195-158"><a name="e-g"></a>404 ... Det gick inte att hitta prenumerationen</span><span class="sxs-lookup"><span data-stu-id="33195-158"><a name="e-g"></a>404 ... Subscription not found</span></span>
![404 ... Det gick inte att hitta prenumerationen](./media/app-insights-analytics-troubleshooting/070.png)

<span data-ttu-id="33195-160">URL: en är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="33195-160">The URL is invalid.</span></span> 

* <span data-ttu-id="33195-161">Öppna app-resurs i [Application Insights-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="33195-161">Open the app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="33195-162">Använd sedan knappen Analytics.</span><span class="sxs-lookup"><span data-stu-id="33195-162">Then use the Analytics button.</span></span>

## <span data-ttu-id="33195-163"><a name="e-h"></a>404... sidan finns inte</span><span class="sxs-lookup"><span data-stu-id="33195-163"><a name="e-h"></a>404 ... page doesn't exist</span></span>
![404 ... Sidan finns inte](./media/app-insights-analytics-troubleshooting/080.png)

<span data-ttu-id="33195-165">URL: en är ogiltig.</span><span class="sxs-lookup"><span data-stu-id="33195-165">The URL is invalid.</span></span>

* <span data-ttu-id="33195-166">Öppna app-resurs i [Application Insights-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="33195-166">Open the app resource in [Application Insights portal](https://portal.azure.com).</span></span> <span data-ttu-id="33195-167">Använd sedan knappen Analytics.</span><span class="sxs-lookup"><span data-stu-id="33195-167">Then use the Analytics button.</span></span>

## <span data-ttu-id="33195-168"><a name="cookies"></a>Aktivera cookies från tredje part</span><span class="sxs-lookup"><span data-stu-id="33195-168"><a name="cookies"></a>Enable third-party cookies</span></span>
  <span data-ttu-id="33195-169">Se [inaktivera cookies från tredje part](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), men vi behöver meddelande **aktivera** dem.</span><span class="sxs-lookup"><span data-stu-id="33195-169">See [how to disable third party cookies](http://www.digitalcitizen.life/how-disable-third-party-cookies-all-major-browsers), but notice we need to **enable** them.</span></span>


[!INCLUDE [app-insights-analytics-footer](../../includes/app-insights-analytics-footer.md)]

