---
title: "aaaAzure användargränssnitt för Mobile Engagement - nå kampanj"
description: Laern hur toocreate och hantera push notification kampanjer med Azure Mobile Engagement
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 2fe124a2-a86f-4136-81ba-a9d298ec798a
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 825e550ace63a34d1a90b10fa976a61eb15a6d04
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-push-notification-campaigns"></a><span data-ttu-id="d28b7-103">Hur toocreate och hantera kampanjer för push-meddelande</span><span class="sxs-lookup"><span data-stu-id="d28b7-103">How toocreate and manage push notification campaigns</span></span>
<span data-ttu-id="d28b7-104">Du kan använda hello Reach-avsnittet i hello UI toocreate en ny Push-kampanj med en formel genom att tillhandahålla alla hello information som du behöver toosend ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="d28b7-104">You can use hello Reach section of hello UI toocreate a new Push campaign with a complex formula by providing all hello information you need toosend a push notification.</span></span> <span data-ttu-id="d28b7-105">hello alternativen för en Push-kampanj varierar något beroende på hello fyra kampanjtyper: meddelanden, avsökningar, Data-push både och paneler (endast Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="d28b7-105">hello options of a Push campaign vary slightly depending on hello four campaign types: Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span>

### <a name="option-applies-to"></a><span data-ttu-id="d28b7-106">Alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="d28b7-106">Option Applies to:</span></span>
* <span data-ttu-id="d28b7-107">Språk: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-107">Languages:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="d28b7-108">Kampanj: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-108">Campaign:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="d28b7-109">Meddelande: Meddelanden, undersökningar</span><span class="sxs-lookup"><span data-stu-id="d28b7-109">Notification:     Announcements, Polls</span></span>
* <span data-ttu-id="d28b7-110">Innehåll: Unikt för varje Kampanjtyp av</span><span class="sxs-lookup"><span data-stu-id="d28b7-110">Content:    Unique for each campaign type</span></span>
* <span data-ttu-id="d28b7-111">Målgrupp: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-111">Audience:     All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="d28b7-112">Tidsintervall: meddelanden, undersökningar, paneler</span><span class="sxs-lookup"><span data-stu-id="d28b7-112">Time frame:     Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="d28b7-113">Test: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-113">Test:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>

![Reach-Campaign1][20]

## <a name="languages"></a><span data-ttu-id="d28b7-115">Språk</span><span class="sxs-lookup"><span data-stu-id="d28b7-115">Languages</span></span>
<span data-ttu-id="d28b7-116">Du kan använda hello språk nedrullningsbara menyn toosend en annan version av din Push-toodevices som ställs toouse olika språk.</span><span class="sxs-lookup"><span data-stu-id="d28b7-116">You can use hello Languages drop-down menu toosend a different version of your Push toodevices that are set toouse different languages.</span></span> <span data-ttu-id="d28b7-117">Som standard får alla enheter hello samma Push oavsett vilket språk de inställda toouse.</span><span class="sxs-lookup"><span data-stu-id="d28b7-117">By default, all devices will receive hello same Push regardless of what language they are set toouse.</span></span> <span data-ttu-id="d28b7-118">Användare med sina enheter ange tooa olika språk får hello standard språkversion av hello Push.</span><span class="sxs-lookup"><span data-stu-id="d28b7-118">Users with their device set tooa different language will receive hello Default Language version of hello Push.</span></span> <span data-ttu-id="d28b7-119">Många av hello push-kampanj alternativ tillåta toospecify alternativa innehåll för varje hello fler språk du väljer.</span><span class="sxs-lookup"><span data-stu-id="d28b7-119">Many of hello push campaign options allow you toospecify alternate content for each of hello additional languages you select.</span></span> 

![Reach-Campaign2][21]

### <a name="language-differences-apply-to"></a><span data-ttu-id="d28b7-121">Språkskillnader gäller för:</span><span class="sxs-lookup"><span data-stu-id="d28b7-121">Language differences apply to:</span></span>
* <span data-ttu-id="d28b7-122">Språk: Unika språk kan väljas i tillägg toohello standardspråk</span><span class="sxs-lookup"><span data-stu-id="d28b7-122">Languages:    Unique languages may be selected in addition toohello default language</span></span>
* <span data-ttu-id="d28b7-123">Kampanj: Samma för alla språk</span><span class="sxs-lookup"><span data-stu-id="d28b7-123">Campaign:    Same for all languages</span></span>
* <span data-ttu-id="d28b7-124">Meddelande: Unika för varje språk dessutom toohello standardspråk</span><span class="sxs-lookup"><span data-stu-id="d28b7-124">Notification:    Unique for each language in addition toohello default language</span></span>
* <span data-ttu-id="d28b7-125">Innehåll: Unika för varje språk dessutom toohello standardspråk</span><span class="sxs-lookup"><span data-stu-id="d28b7-125">Content:    Unique for each language in addition toohello default language</span></span>
* <span data-ttu-id="d28b7-126">Målgrupp: Filtreras efter ett villkor för olika språk</span><span class="sxs-lookup"><span data-stu-id="d28b7-126">Audience:     May be filtered by a separate language criterion</span></span>
* <span data-ttu-id="d28b7-127">Tidsintervall: samma för alla språk</span><span class="sxs-lookup"><span data-stu-id="d28b7-127">Time frame:     Same for all languages</span></span>
* <span data-ttu-id="d28b7-128">Test: Skickas tooeach språk i taget</span><span class="sxs-lookup"><span data-stu-id="d28b7-128">Test:    May be sent tooeach language at a time</span></span>

### <a name="supported-languages"></a><span data-ttu-id="d28b7-129">Språk som stöds:</span><span class="sxs-lookup"><span data-stu-id="d28b7-129">Supported Languages:</span></span>
* <span data-ttu-id="d28b7-130">Arabiska (ar)</span><span class="sxs-lookup"><span data-stu-id="d28b7-130">Arabic (ar)</span></span> 
* <span data-ttu-id="d28b7-131">Bulgariska (bg)</span><span class="sxs-lookup"><span data-stu-id="d28b7-131">Bulgarian (bg)</span></span> 
* <span data-ttu-id="d28b7-132">Katalanska (ca)</span><span class="sxs-lookup"><span data-stu-id="d28b7-132">Catalan (ca)</span></span> 
* <span data-ttu-id="d28b7-133">Kinesiska (zh)</span><span class="sxs-lookup"><span data-stu-id="d28b7-133">Chinese (zh)</span></span> 
* <span data-ttu-id="d28b7-134">Kroatiska (hr)</span><span class="sxs-lookup"><span data-stu-id="d28b7-134">Croatian (hr)</span></span> 
* <span data-ttu-id="d28b7-135">Czech (CS)</span><span class="sxs-lookup"><span data-stu-id="d28b7-135">Czech (cs)</span></span> 
* <span data-ttu-id="d28b7-136">Danska (da)</span><span class="sxs-lookup"><span data-stu-id="d28b7-136">Danish (da)</span></span> 
* <span data-ttu-id="d28b7-137">Nederländska (nl)</span><span class="sxs-lookup"><span data-stu-id="d28b7-137">Dutch (nl)</span></span> 
* <span data-ttu-id="d28b7-138">Engelska (en)</span><span class="sxs-lookup"><span data-stu-id="d28b7-138">English (en)</span></span> 
* <span data-ttu-id="d28b7-139">Finska (fi)</span><span class="sxs-lookup"><span data-stu-id="d28b7-139">Finnish (fi)</span></span> 
* <span data-ttu-id="d28b7-140">Franska (fr)</span><span class="sxs-lookup"><span data-stu-id="d28b7-140">French (fr)</span></span> 
* <span data-ttu-id="d28b7-141">Tyska (de)</span><span class="sxs-lookup"><span data-stu-id="d28b7-141">German (de)</span></span> 
* <span data-ttu-id="d28b7-142">Grekiska (el)</span><span class="sxs-lookup"><span data-stu-id="d28b7-142">Greek (el)</span></span> 
* <span data-ttu-id="d28b7-143">Hebreiska (han)</span><span class="sxs-lookup"><span data-stu-id="d28b7-143">Hebrew (he)</span></span> 
* <span data-ttu-id="d28b7-144">Hindi (hög)</span><span class="sxs-lookup"><span data-stu-id="d28b7-144">Hindi (hi)</span></span> 
* <span data-ttu-id="d28b7-145">Ungerska (hu)</span><span class="sxs-lookup"><span data-stu-id="d28b7-145">Hungarian (hu)</span></span> 
* <span data-ttu-id="d28b7-146">Indonesiska (id)</span><span class="sxs-lookup"><span data-stu-id="d28b7-146">Indonesian (id)</span></span> 
* <span data-ttu-id="d28b7-147">Italienska (it)</span><span class="sxs-lookup"><span data-stu-id="d28b7-147">Italian (it)</span></span> 
* <span data-ttu-id="d28b7-148">Japanska (ja)</span><span class="sxs-lookup"><span data-stu-id="d28b7-148">Japanese (ja)</span></span> 
* <span data-ttu-id="d28b7-149">Koreanska (ko)</span><span class="sxs-lookup"><span data-stu-id="d28b7-149">Korean (ko)</span></span> 
* <span data-ttu-id="d28b7-150">Lettiska (lv)</span><span class="sxs-lookup"><span data-stu-id="d28b7-150">Latvian (lv)</span></span> 
* <span data-ttu-id="d28b7-151">Litauiska (lt)</span><span class="sxs-lookup"><span data-stu-id="d28b7-151">Lithuanian (lt)</span></span> 
* <span data-ttu-id="d28b7-152">Malajiska (macrolanguage) (ms)</span><span class="sxs-lookup"><span data-stu-id="d28b7-152">Malay (macrolanguage) (ms)</span></span> 
* <span data-ttu-id="d28b7-153">Norska Bokmål (nb)</span><span class="sxs-lookup"><span data-stu-id="d28b7-153">Norwegian Bokmål (nb)</span></span> 
* <span data-ttu-id="d28b7-154">Polska (pl)</span><span class="sxs-lookup"><span data-stu-id="d28b7-154">Polish (pl)</span></span> 
* <span data-ttu-id="d28b7-155">Portugisiska (pt)</span><span class="sxs-lookup"><span data-stu-id="d28b7-155">Portuguese (pt)</span></span> 
* <span data-ttu-id="d28b7-156">Rumänska (ro)</span><span class="sxs-lookup"><span data-stu-id="d28b7-156">Romanian (ro)</span></span> 
* <span data-ttu-id="d28b7-157">Ryska (ru)</span><span class="sxs-lookup"><span data-stu-id="d28b7-157">Russian (ru)</span></span> 
* <span data-ttu-id="d28b7-158">Serbiska (sr)</span><span class="sxs-lookup"><span data-stu-id="d28b7-158">Serbian (sr)</span></span> 
* <span data-ttu-id="d28b7-159">Slovakiska (sk)</span><span class="sxs-lookup"><span data-stu-id="d28b7-159">Slovak (sk)</span></span> 
* <span data-ttu-id="d28b7-160">Slovenska (sl)</span><span class="sxs-lookup"><span data-stu-id="d28b7-160">Slovenian (sl)</span></span> 
* <span data-ttu-id="d28b7-161">Spanska (es)</span><span class="sxs-lookup"><span data-stu-id="d28b7-161">Spanish (es)</span></span> 
* <span data-ttu-id="d28b7-162">Svenska (sv)</span><span class="sxs-lookup"><span data-stu-id="d28b7-162">Swedish (sv)</span></span> 
* <span data-ttu-id="d28b7-163">Tagalog (tl)</span><span class="sxs-lookup"><span data-stu-id="d28b7-163">Tagalog (tl)</span></span> 
* <span data-ttu-id="d28b7-164">Thailändska (th)</span><span class="sxs-lookup"><span data-stu-id="d28b7-164">Thai (th)</span></span> 
* <span data-ttu-id="d28b7-165">Turkiska (tr)</span><span class="sxs-lookup"><span data-stu-id="d28b7-165">Turkish (tr)</span></span> 
* <span data-ttu-id="d28b7-166">Ukrainska (Storbritannien)</span><span class="sxs-lookup"><span data-stu-id="d28b7-166">Ukrainian (uk)</span></span> 
* <span data-ttu-id="d28b7-167">Vietnamesiska (vi)</span><span class="sxs-lookup"><span data-stu-id="d28b7-167">Vietnamese (vi)</span></span> 

## <a name="campaign"></a><span data-ttu-id="d28b7-168">Kampanj</span><span class="sxs-lookup"><span data-stu-id="d28b7-168">Campaign</span></span>
<span data-ttu-id="d28b7-169">Du kan använda hello kampanj avsnittet tooset hello namn och kategori för kampanjen samt som om du planerar tooignore hello målgrupp för en Push-kampanj och skicka den här kampanjen via hello nå API (och vissa element med hello låg nivå Push-API) i stället.</span><span class="sxs-lookup"><span data-stu-id="d28b7-169">You can use hello Campaign section tooset hello name and category of your campaign as well as if you plan tooignore hello audience section of a Push campaign and send this campaign via hello Reach API (and some elements with hello low level Push API) instead.</span></span> <span data-ttu-id="d28b7-170">Kategorier kan användas med ett anpassat meddelande mallen toocontrol i appen meddelanden baserat på fördefinierade inställningar.</span><span class="sxs-lookup"><span data-stu-id="d28b7-170">Categories can be used with a custom notification template toocontrol in-app notifications based on predefined settings.</span></span> <span data-ttu-id="d28b7-171">Du kan hämta en lista över dina befintliga ”kategorier” via hello nå API.</span><span class="sxs-lookup"><span data-stu-id="d28b7-171">You can get a list of your existing “Categories” via hello Reach API.</span></span>

> [!WARNING]
> <span data-ttu-id="d28b7-172">Om alternativet hello ”Ignorera målgrupp, push skickas toousers via hello API” hello ”kampanj” avsnittet av en Reach-kampanj hello kampanj skickas inte automatiskt, måste toosend den manuellt via hello nå API.</span><span class="sxs-lookup"><span data-stu-id="d28b7-172">If you use hello "Ignore Audience, push will be sent toousers via hello API" option in hello "Campaign" section of a Reach campaign, hello campaign will NOT automatically send, you will need toosend it manually via hello Reach API.</span></span>

![Reach-Campaign3][22]

### <a name="option-applies-to"></a><span data-ttu-id="d28b7-174">Alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="d28b7-174">Option Applies to:</span></span>
* <span data-ttu-id="d28b7-175">Namn: alla</span><span class="sxs-lookup"><span data-stu-id="d28b7-175">Name:    All</span></span>
* <span data-ttu-id="d28b7-176">Kategori: Meddelanden, undersökningar</span><span class="sxs-lookup"><span data-stu-id="d28b7-176">Category:    Announcements, Polls</span></span>
* <span data-ttu-id="d28b7-177">Ignorera målgrupp, push skickas toousers via hello API: alla</span><span class="sxs-lookup"><span data-stu-id="d28b7-177">Ignore Audience, push will be sent toousers via hello API:    All</span></span>

## <a name="notification"></a><span data-ttu-id="d28b7-178">Avisering</span><span class="sxs-lookup"><span data-stu-id="d28b7-178">Notification</span></span>
<span data-ttu-id="d28b7-179">Du kan använda hello Notification avsnittet tooset grundläggande inställningar för din push inklusive: hello titeln på hello Push hello-meddelande, en bild i appen, eller om det är stängas.</span><span class="sxs-lookup"><span data-stu-id="d28b7-179">You can use hello Notification section tooset basic settings for your push including: hello title of hello Push, hello message, an in-app image, or if it is dismissible.</span></span> <span data-ttu-id="d28b7-180">Många Meddelandeinställningar är särskilda toohello plattform på enheten.</span><span class="sxs-lookup"><span data-stu-id="d28b7-180">Many notification settings are specific toohello platform of your device.</span></span> <span data-ttu-id="d28b7-181">Du kan välja om din push skickas ”app” eller ”out of app” eller båda.</span><span class="sxs-lookup"><span data-stu-id="d28b7-181">You can select whether your push will be sent "in app" or "out of app" or both.</span></span> <span data-ttu-id="d28b7-182">(Kom ihåg att användare kan ”välja” eller ”CEIP” ”out-of-app” push-meddelanden på hello operativsystemet nivå på sina enheter och Azure Mobile Engagement kommer inte att kunna toooverride den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="d28b7-182">(Remember that users can "opt-in" or "opt-out" of "out of app" Pushes at hello Operating System level on their devices, and Azure Mobile Engagement will not be able toooverride this setting.</span></span> <span data-ttu-id="d28b7-183">Kom ihåg att hello Reach-API som hanterar ”i app” och ”out-of-app” push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d28b7-183">Also remember that hello Reach API handles "in app" and "out of app" Pushes.</span></span> <span data-ttu-id="d28b7-184">hello Push-API kan vara används toohandle ”out-of-app” push-meddelanden för.) Push-meddelanden kan anpassas med bilder eller HTML-innehåll, inklusive djuplänkar för att länka utanför din App eller tooanother plats i din App (Android SDK 2.1.0 eller senare avsiktshantering kategorier krävs).</span><span class="sxs-lookup"><span data-stu-id="d28b7-184">hello Push API can be used toohandle "out of app" pushes too.) Pushes can be customized with pictures or HTML content, including deep links for linking outside of your App or tooanother location in your App (Android SDK 2.1.0 or later intent categories required).</span></span> <span data-ttu-id="d28b7-185">Du kan ändra hello ikonen eller iOS-skylt och skicka text eller web innehållet (ett popup-fönster med HTML-innehåll, URL: en länk tooanother plats i eller utanför hello app).</span><span class="sxs-lookup"><span data-stu-id="d28b7-185">You can change hello icon or iOS badge, and send either text or web content (a popup with html content, URL link tooanother location either inside or outside of hello app).</span></span> <span data-ttu-id="d28b7-186">Du kan också göra Android-enheter ring eller vibrerar med hello Push.</span><span class="sxs-lookup"><span data-stu-id="d28b7-186">You can also make Android devices ring or vibrate with hello Push.</span></span> <span data-ttu-id="d28b7-187">(Kom ihåg att du kommer måste hello rätt SDK behörigheter i din Android manifest filen tooring eller vibrerar en enhet.) Det finns för närvarande inga branschen standard för Android ”översikt” storlekar eftersom skärmstorlek skiljer sig på varje enhet, men 400 × 100 bilder fungerar på nästan alla skärmstorlek.</span><span class="sxs-lookup"><span data-stu-id="d28b7-187">(Remember that you will need hello correct SDK permissions in your Android manifest file tooring or vibrate a device.) There is currently no industry standard for Android "Big Picture" sizes, since screen sizes are different on every device, but 400x100 pictures work on almost any screen size.</span></span>

### <a name="delivery-types"></a><span data-ttu-id="d28b7-188">Leverans typer:</span><span class="sxs-lookup"><span data-stu-id="d28b7-188">Delivery Types:</span></span>
* <span data-ttu-id="d28b7-189">Endast utanför appen: hello-meddelande levereras när hello användare inte använda hello programmet.</span><span class="sxs-lookup"><span data-stu-id="d28b7-189">Out of app only: hello notification will be delivered when hello user does not use hello application.</span></span>
* <span data-ttu-id="d28b7-190">hello utanför appavisering som endast kräver ett certifikat från Apple eller Google (APN eller GCM-certifikat).</span><span class="sxs-lookup"><span data-stu-id="d28b7-190">hello out of app only notification requires a certificate from Apple or Google (APNS or GCM certificate).</span></span>
* <span data-ttu-id="d28b7-191">I appen bara: hello-meddelande visas endast när hello program körs.</span><span class="sxs-lookup"><span data-stu-id="d28b7-191">In-app only: hello notification appears only when hello application is running.</span></span>
* <span data-ttu-id="d28b7-192">hello-meddelande använder hello Capptain leverans tooreach hello systemanvändaren.</span><span class="sxs-lookup"><span data-stu-id="d28b7-192">hello notification uses hello Capptain delivery system tooreach hello user.</span></span> <span data-ttu-id="d28b7-193">Du kan anpassa hello layout/visning av din push.</span><span class="sxs-lookup"><span data-stu-id="d28b7-193">You can fully customize hello visual layout/display of your push.</span></span>
* <span data-ttu-id="d28b7-194">När som helst: Det här alternativet innebär att du skickar ett meddelande som antingen hello-program körs eller inte.</span><span class="sxs-lookup"><span data-stu-id="d28b7-194">Anytime: This option ensures that you send a notification either hello application is running or not.</span></span>

![Reach-Campaign4][23]

### <a name="option-applies-to"></a><span data-ttu-id="d28b7-196">Alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="d28b7-196">Option Applies to:</span></span>
* <span data-ttu-id="d28b7-197">Meddelande: Meddelanden, undersökningar</span><span class="sxs-lookup"><span data-stu-id="d28b7-197">Notification:     Announcements, Polls</span></span>

## <a name="content"></a><span data-ttu-id="d28b7-198">Innehåll</span><span class="sxs-lookup"><span data-stu-id="d28b7-198">Content</span></span>
<span data-ttu-id="d28b7-199">Du kan använda hello innehållsavsnitt toomodify hello innehållet i dina meddelanden, avsökningar, Data-push både och paneler (endast Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="d28b7-199">You can use hello Content section toomodify hello content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="d28b7-200">hello innehåll inställningen för Push-kampanjer är särskilda toohello kampanj.</span><span class="sxs-lookup"><span data-stu-id="d28b7-200">hello Content setting of Push campaigns is specific toohello type of campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="d28b7-201">Se även</span><span class="sxs-lookup"><span data-stu-id="d28b7-201">See also</span></span>
* <span data-ttu-id="d28b7-202">[UI-dokumentation – nå - Push-innehåll][Link 29]</span><span class="sxs-lookup"><span data-stu-id="d28b7-202">[UI Documentation - Reach - Push Content][Link 29]</span></span>

![Reach-Campaign5][24]

## <a name="audience"></a><span data-ttu-id="d28b7-204">Målgrupp</span><span class="sxs-lookup"><span data-stu-id="d28b7-204">Audience</span></span>
<span data-ttu-id="d28b7-205">Du kan använda hello målgruppen avsnittet toodefine en standard lista över objekt toolimit din kampanj eller gränser din kampanj baserat på egna villkor.</span><span class="sxs-lookup"><span data-stu-id="d28b7-205">You can use hello Audience section toodefine a standard list of items toolimit your campaign or limits your campaign based on customized criteria.</span></span> <span data-ttu-id="d28b7-206">hello standarduppsättning alternativ tooLimit din målgrupp kan du toopush tooeither nya eller gamla användare eller endast interna push-användare.</span><span class="sxs-lookup"><span data-stu-id="d28b7-206">hello standard set of options tooLimit your Audience allows you toopush tooeither new or old users or native push users only.</span></span> <span data-ttu-id="d28b7-207">Du kan också ange en kvot toolimit hello antalet användare som tar emot hello push.</span><span class="sxs-lookup"><span data-stu-id="d28b7-207">You can also set a quota toolimit hello number of users who receive hello push.</span></span> <span data-ttu-id="d28b7-208">Du kan redigera hello-uttrycket för hur din kampanj är filtrerade tooinclude manuellt en eller flera kriterium tootarget användare.</span><span class="sxs-lookup"><span data-stu-id="d28b7-208">You can manually Edit hello expression for how your campaign is filtered tooinclude one or more criterion tootarget users.</span></span> <span data-ttu-id="d28b7-209">Du kan skriva en målgruppsuttrycket manuellt.</span><span class="sxs-lookup"><span data-stu-id="d28b7-209">You can manually type an audience expression.</span></span> <span data-ttu-id="d28b7-210">Ett sådant uttryck måste uttryckligen definiera hello relationen mellan kriterierna.</span><span class="sxs-lookup"><span data-stu-id="d28b7-210">Such an expression must explicitly define hello relation between criteria.</span></span> <span data-ttu-id="d28b7-211">Ett kriterium beskrivs av en identifierare som måste börja med en bokstav och får inte innehålla blanksteg.</span><span class="sxs-lookup"><span data-stu-id="d28b7-211">A criterion is described by an identifier that must start with a capital letter and cannot contain spaces.</span></span> <span data-ttu-id="d28b7-212">hello relationen mellan hello kriterierna kan beskrivas med 'och', 'eller', 'not-operatorer som '(',')'.</span><span class="sxs-lookup"><span data-stu-id="d28b7-212">hello relation between hello criteria can be described using 'and', 'or', 'not' operators as well as '(', ')'.</span></span> <span data-ttu-id="d28b7-213">Exempel: ”Criterion1 or (Criterion1 och inte Criterion2)”.</span><span class="sxs-lookup"><span data-stu-id="d28b7-213">Example: "Criterion1 or (Criterion1 and not Criterion2)".</span></span>

> [!NOTE]
> <span data-ttu-id="d28b7-214">Har många ingår i kampanjer, hello serversidan riktad genomsökning kan ta lång tid, särskilt om du försöker toostart hello flera kampanjer på samma gång.</span><span class="sxs-lookup"><span data-stu-id="d28b7-214">With a large audience included in campaigns, hello server side targeting scan can be slow, especially if you attempt toostart multiple campaigns at hello same time.</span></span>

* <span data-ttu-id="d28b7-215">Om det är möjligt endast starta en kampanj i taget.</span><span class="sxs-lookup"><span data-stu-id="d28b7-215">If possible, only start one campaign at a time.</span></span>
* <span data-ttu-id="d28b7-216">Vid hello mest endast start fyra kampanjer i taget.</span><span class="sxs-lookup"><span data-stu-id="d28b7-216">At hello most, only start four campaigns at a time.</span></span>
* <span data-ttu-id="d28b7-217">Push-bara tooyour aktiva användare (kryssrutan ”engagera bara användare som kan nås med hjälp av interna Push” och ”engagera bara aktiva användare”) så att dina användare som fortfarande har hello appen har installerats och använda den måste toobe genomsöks.</span><span class="sxs-lookup"><span data-stu-id="d28b7-217">Push only tooyour active users (checkbox "Engage only users who can be reached using Native Push" and "Engage only active users") so that only your users who still have hello app installed and use it will need toobe scanned.</span></span>
  <span data-ttu-id="d28b7-218">När användarna har definierats kan du använda hello simulera knappen toofind ut hur många användare får den här Push.</span><span class="sxs-lookup"><span data-stu-id="d28b7-218">Once your audience is defined, you can use hello simulate button toofind out how many users will receive this Push.</span></span> <span data-ttu-id="d28b7-219">Detta kommer att beräkna hello antal kända användare som kan vara mål för den här målgruppen (detta är en uppskattning baserad på ett slumpvist urval av användare).</span><span class="sxs-lookup"><span data-stu-id="d28b7-219">This will compute hello number of known users potentially targeted by this audience (this is an estimate based on a random sample of users).</span></span> <span data-ttu-id="d28b7-220">Tänk på att användare som har avinstallerat programmet hello ingår också i företagsmiljöer, men kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="d28b7-220">Be aware that users who have uninstalled hello application are also part of this audience, but cannot be reached.</span></span>

### <a name="see-also"></a><span data-ttu-id="d28b7-221">Se även</span><span class="sxs-lookup"><span data-stu-id="d28b7-221">See also</span></span>
* <span data-ttu-id="d28b7-222">[UI-dokumentationen – Reach - nytt Push kriterium][Link 28]</span><span class="sxs-lookup"><span data-stu-id="d28b7-222">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

![Reach-Campaign6][25]

### <a name="edit-expression"></a><span data-ttu-id="d28b7-224">Redigera uttryck</span><span class="sxs-lookup"><span data-stu-id="d28b7-224">Edit expression</span></span>
![Reach-Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a><span data-ttu-id="d28b7-226">Gräns för din målgrupp alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="d28b7-226">Limit your audience option applies to:</span></span>
* <span data-ttu-id="d28b7-227">Engagera bara en delmängd av användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-227">Engage only a subset of users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="d28b7-228">Engagera bara gamla användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-228">Engage only old users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="d28b7-229">Engagera bara nya användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-229">Engage only new users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="d28b7-230">Engagera bara inaktiva användare: meddelanden, undersökningar, paneler</span><span class="sxs-lookup"><span data-stu-id="d28b7-230">Engage only idle users:    Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="d28b7-231">Engagera bara aktiva användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="d28b7-231">Engage only active users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="d28b7-232">Engagera bara användare som kan nås med systemspecifika push-aviseringar: meddelanden, avsökningar</span><span class="sxs-lookup"><span data-stu-id="d28b7-232">Engage only users who can be reached using Native Push:     Announcements, Polls</span></span>

## <a name="time-frame"></a><span data-ttu-id="d28b7-233">Tidsintervall</span><span class="sxs-lookup"><span data-stu-id="d28b7-233">Time Frame</span></span>
<span data-ttu-id="d28b7-234">Du kan använda hello tidsram avsnittet tooset när hello push skickas eller lämna hello tidsram tomt toostart hello kampanj omedelbart.</span><span class="sxs-lookup"><span data-stu-id="d28b7-234">You can use hello Time Frame section tooset when hello push will be sent or you can leave hello time frame blank toostart hello campaign immediately.</span></span> <span data-ttu-id="d28b7-235">Kom ihåg att tidszon hello slutanvändare kan starta med hello kampanj en dag tidigare än vad du förväntar dig för dina slutanvändare i Asien och skicka små batchar av push-meddelanden i taget tills alla tidszoner i hello world matchar hello tidsram som angetts för din kampanj.</span><span class="sxs-lookup"><span data-stu-id="d28b7-235">Remember that using hello end-users' time zone may start hello campaign a day earlier than you expect for your end-users in Asia and send small batches of pushes at a time until all time zones in hello world match hello time frame set for your campaign.</span></span> <span data-ttu-id="d28b7-236">Med hjälp av hello slutanvändarens tidszon kan också orsaka fördröjningar i kampanjer eftersom den har toorequest hello tid från hello phone innan du startar hello push.</span><span class="sxs-lookup"><span data-stu-id="d28b7-236">Using hello end users' time zone can also cause delays in campaigns since it has toorequest hello time from hello phone before starting hello push.</span></span>

> [!NOTE]
> <span data-ttu-id="d28b7-237">Kampanjer utan ett slutdatum kan cachelagra push-meddelanden lokalt och fortfarande visa dem efter att du manuellt fullständig kampanjer.</span><span class="sxs-lookup"><span data-stu-id="d28b7-237">Campaigns without an end date can cache pushes locally and still display them after you manually complete campaigns.</span></span> <span data-ttu-id="d28b7-238">tooavoid detta beteende, specifik sluttid för kampanjer.</span><span class="sxs-lookup"><span data-stu-id="d28b7-238">tooavoid this behavior, specific an end time for campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="d28b7-239">Se även</span><span class="sxs-lookup"><span data-stu-id="d28b7-239">See also</span></span>
* <span data-ttu-id="d28b7-240">[Nå - hur Tos-planering][Link 3]</span><span class="sxs-lookup"><span data-stu-id="d28b7-240">[Reach - How Tos – Scheduling][Link 3]</span></span> 

![Reach-Campaign8][27]

### <a name="settings-apply-to"></a><span data-ttu-id="d28b7-242">Inställningar som gäller för:</span><span class="sxs-lookup"><span data-stu-id="d28b7-242">Settings Apply to:</span></span>
* <span data-ttu-id="d28b7-243">Tidsintervall: meddelanden, undersökningar, paneler</span><span class="sxs-lookup"><span data-stu-id="d28b7-243">Time frame:     Announcements, Polls, Tiles</span></span>

## <a name="test"></a><span data-ttu-id="d28b7-244">Testa</span><span class="sxs-lookup"><span data-stu-id="d28b7-244">Test</span></span>
<span data-ttu-id="d28b7-245">Du kan använda hello Test avsnittet toosend push tooyour egna test enheten innan du sparar hello kampanj.</span><span class="sxs-lookup"><span data-stu-id="d28b7-245">You can use hello Test section toosend this push tooyour own test device before saving hello campaign.</span></span> <span data-ttu-id="d28b7-246">Om du har konfigurerat en anpassad språk för den här kampanjen kan testa du hello push på varje språk.</span><span class="sxs-lookup"><span data-stu-id="d28b7-246">If you have configured any custom languages for this campaign, you can test hello push in each language.</span></span> <span data-ttu-id="d28b7-247">Du kan konfigurera en testenhet från ”mitt konto”.</span><span class="sxs-lookup"><span data-stu-id="d28b7-247">You can setup a test device from “My Account”.</span></span>

> [!NOTE]
> <span data-ttu-id="d28b7-248">Inga data loggas när du använder hello-knappen på serversidan för ”test” push-meddelanden, data loggas endast för verkliga push-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="d28b7-248">No server side data is logged when you use hello button too"test" pushes, data is only logged for real push campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="d28b7-249">Se även</span><span class="sxs-lookup"><span data-stu-id="d28b7-249">See also</span></span>
* <span data-ttu-id="d28b7-250">[UI-dokumentation – mitt konto][Link 14]</span><span class="sxs-lookup"><span data-stu-id="d28b7-250">[UI Documentation - My Account][Link 14]</span></span>

![Reach-Campaign9][28]

<!--Image references-->
[1]: ./media/mobile-engagement-user-interface-navigation/navigation1.png
[2]: ./media/mobile-engagement-user-interface-home/home1.png
[3]: ./media/mobile-engagement-user-interface-home/home2.png
[4]: ./media/mobile-engagement-user-interface-home/home3.png
[5]: ./media/mobile-engagement-user-interface-home/home4.png
[6]: ./media/mobile-engagement-user-interface-home/home5.png
[7]: ./media/mobile-engagement-user-interface-my-account/myaccount1.png
[8]: ./media/mobile-engagement-user-interface-my-account/myaccount2.png
[9]: ./media/mobile-engagement-user-interface-my-account/myaccount3.png
[10]: ./media/mobile-engagement-user-interface-analytics/analytics1.png
[11]: ./media/mobile-engagement-user-interface-analytics/analytics2.png
[12]: ./media/mobile-engagement-user-interface-analytics/analytics3.png
[13]: ./media/mobile-engagement-user-interface-analytics/analytics4.png
[14]: ./media/mobile-engagement-user-interface-monitor/monitor1.png
[15]: ./media/mobile-engagement-user-interface-monitor/monitor2.png
[16]: ./media/mobile-engagement-user-interface-monitor/monitor3.png
[17]: ./media/mobile-engagement-user-interface-monitor/monitor4.png
[18]: ./media/mobile-engagement-user-interface-reach/reach1.png
[19]: ./media/mobile-engagement-user-interface-reach/reach2.png
[20]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign1.png
[21]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign2.png
[22]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign3.png
[23]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign4.png
[24]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign5.png
[25]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign6.png
[26]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign7.png
[27]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign8.png
[28]: ./media/mobile-engagement-user-interface-reach-campaign/Reach-Campaign9.png
[29]: ./media/mobile-engagement-user-interface-reach-criterion/Reach-Criterion1.png
[30]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content1.png
[31]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content2.png
[32]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content3.png
[33]: ./media/mobile-engagement-user-interface-reach-content/Reach-Content4.png
[34]: ./media/mobile-engagement-user-interface-dashboard/dashboard1.png
[35]: ./media/mobile-engagement-user-interface-segments/segments1.png
[36]: ./media/mobile-engagement-user-interface-segments/segments2.png
[37]: ./media/mobile-engagement-user-interface-segments/segments3.png
[38]: ./media/mobile-engagement-user-interface-segments/segments4.png
[39]: ./media/mobile-engagement-user-interface-segments/segments5.png
[40]: ./media/mobile-engagement-user-interface-segments/segments6.png
[41]: ./media/mobile-engagement-user-interface-segments/segments7.png
[42]: ./media/mobile-engagement-user-interface-segments/segments8.png
[43]: ./media/mobile-engagement-user-interface-segments/segments9.png
[44]: ./media/mobile-engagement-user-interface-segments/segments10.png
[45]: ./media/mobile-engagement-user-interface-segments/segments11.png
[46]: ./media/mobile-engagement-user-interface-settings/settings1.png
[47]: ./media/mobile-engagement-user-interface-settings/settings2.png
[48]: ./media/mobile-engagement-user-interface-settings/settings3.png
[49]: ./media/mobile-engagement-user-interface-settings/settings4.png
[50]: ./media/mobile-engagement-user-interface-settings/settings5.png
[51]: ./media/mobile-engagement-user-interface-settings/settings6.png
[52]: ./media/mobile-engagement-user-interface-settings/settings7.png
[53]: ./media/mobile-engagement-user-interface-settings/settings8.png
[54]: ./media/mobile-engagement-user-interface-settings/settings9.png
[55]: ./media/mobile-engagement-user-interface-settings/settings10.png
[56]: ./media/mobile-engagement-user-interface-settings/settings11.png
[57]: ./media/mobile-engagement-user-interface-settings/settings12.png
[58]: ./media/mobile-engagement-user-interface-settings/settings13.png

<!--Link references-->
[Link 1]: mobile-engagement-user-interface.md
[Link 2]: mobile-engagement-troubleshooting-guide.md
[Link 3]: mobile-engagement-how-tos.md
[Link 4]: http://go.microsoft.com/fwlink/?LinkID=525553
[Link 5]: http://go.microsoft.com/fwlink/?LinkID=525554
[Link 6]: http://go.microsoft.com/fwlink/?LinkId=525555
[Link 7]: https://account.windowsazure.com/PreviewFeatures
[Link 8]: https://social.msdn.microsoft.com/Forums/azure/home?forum=azuremobileengagement
[Link 9]: http://azure.microsoft.com/services/mobile-engagement/
[Link 10]: http://azure.microsoft.com/documentation/services/mobile-engagement/
[Link 11]: http://azure.microsoft.com/pricing/details/mobile-engagement/
[Link 12]: mobile-engagement-user-interface-navigation.md
[Link 13]: mobile-engagement-user-interface-home.md
[Link 14]: mobile-engagement-user-interface-my-account.md
[Link 15]: mobile-engagement-user-interface-analytics.md
[Link 16]: mobile-engagement-user-interface-monitor.md
[Link 17]: mobile-engagement-user-interface-reach.md
[Link 18]: mobile-engagement-user-interface-segments.md
[Link 19]: mobile-engagement-user-interface-dashboard.md
[Link 20]: mobile-engagement-user-interface-settings.md
[Link 21]: mobile-engagement-troubleshooting-guide-analytics.md
[Link 22]: mobile-engagement-troubleshooting-guide-apis.md
[Link 23]: mobile-engagement-troubleshooting-guide-push-reach.md
[Link 24]: mobile-engagement-troubleshooting-guide-service.md
[Link 25]: mobile-engagement-troubleshooting-guide-sdk.md
[Link 26]: mobile-engagement-troubleshooting-guide-sr-info.md
[Link 27]: mobile-engagement-user-interface-reach-campaign.md
[Link 28]: mobile-engagement-user-interface-reach-criterion.md
[Link 29]: mobile-engagement-user-interface-reach-content.md

