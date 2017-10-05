---
title: "Användargränssnittet för Azure Mobile Engagement - Reach-kampanj"
description: "Laern hur du skapar och hanterar push-meddelande kampanjer som använder Azure Mobile Engagement"
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
ms.openlocfilehash: fc88db8db11d1ed12fa95c2087c9a32b21bf4de5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-push-notification-campaigns"></a><span data-ttu-id="fa4dd-103">Hur du skapar och hanterar kampanjer för push-meddelande</span><span class="sxs-lookup"><span data-stu-id="fa4dd-103">How to create and manage push notification campaigns</span></span>
<span data-ttu-id="fa4dd-104">Du kan använda Reach-avsnittet i Användargränssnittet för att skapa en ny Push-kampanj med en formel genom att ange den information som du måste skicka ett push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-104">You can use the Reach section of the UI to create a new Push campaign with a complex formula by providing all the information you need to send a push notification.</span></span> <span data-ttu-id="fa4dd-105">Alternativen för en Push-kampanj varierar något beroende på vilka kampanjtyper av fyra: meddelanden, avsökningar, Data-push både och paneler (endast Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="fa4dd-105">The options of a Push campaign vary slightly depending on the four campaign types: Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span>

### <a name="option-applies-to"></a><span data-ttu-id="fa4dd-106">Alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-106">Option Applies to:</span></span>
* <span data-ttu-id="fa4dd-107">Språk: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-107">Languages:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="fa4dd-108">Kampanj: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-108">Campaign:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="fa4dd-109">Meddelande: Meddelanden, undersökningar</span><span class="sxs-lookup"><span data-stu-id="fa4dd-109">Notification:     Announcements, Polls</span></span>
* <span data-ttu-id="fa4dd-110">Innehåll: Unikt för varje Kampanjtyp av</span><span class="sxs-lookup"><span data-stu-id="fa4dd-110">Content:    Unique for each campaign type</span></span>
* <span data-ttu-id="fa4dd-111">Målgrupp: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-111">Audience:     All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="fa4dd-112">Tidsintervall: meddelanden, undersökningar, paneler</span><span class="sxs-lookup"><span data-stu-id="fa4dd-112">Time frame:     Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="fa4dd-113">Test: Alla (meddelanden, undersökningar, Data push-meddelanden, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-113">Test:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>

![Reach-Campaign1][20]

## <a name="languages"></a><span data-ttu-id="fa4dd-115">Språk</span><span class="sxs-lookup"><span data-stu-id="fa4dd-115">Languages</span></span>
<span data-ttu-id="fa4dd-116">Du kan använda listrutan språk för att skicka en annan version av din Push till enheter som är inställda på olika språk.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-116">You can use the Languages drop-down menu to send a different version of your Push to devices that are set to use different languages.</span></span> <span data-ttu-id="fa4dd-117">Som standard får alla enheter samma Push oavsett vilket språk som de är inställd på att använda.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-117">By default, all devices will receive the same Push regardless of what language they are set to use.</span></span> <span data-ttu-id="fa4dd-118">Användare med sina enheter som har angetts till ett annat språk får som standard språkversion av push-meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-118">Users with their device set to a different language will receive the Default Language version of the Push.</span></span> <span data-ttu-id="fa4dd-119">Många av alternativ för push-kampanj kan du ange alternativa innehåll för varje ytterligare språk som du väljer.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-119">Many of the push campaign options allow you to specify alternate content for each of the additional languages you select.</span></span> 

![Reach-Campaign2][21]

### <a name="language-differences-apply-to"></a><span data-ttu-id="fa4dd-121">Språkskillnader gäller för:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-121">Language differences apply to:</span></span>
* <span data-ttu-id="fa4dd-122">Språk: Unika språk kan väljas förutom standardspråk</span><span class="sxs-lookup"><span data-stu-id="fa4dd-122">Languages:    Unique languages may be selected in addition to the default language</span></span>
* <span data-ttu-id="fa4dd-123">Kampanj: Samma för alla språk</span><span class="sxs-lookup"><span data-stu-id="fa4dd-123">Campaign:    Same for all languages</span></span>
* <span data-ttu-id="fa4dd-124">Meddelande: Unika för varje språk utöver standardspråk</span><span class="sxs-lookup"><span data-stu-id="fa4dd-124">Notification:    Unique for each language in addition to the default language</span></span>
* <span data-ttu-id="fa4dd-125">Innehåll: Unika för varje språk utöver standardspråk</span><span class="sxs-lookup"><span data-stu-id="fa4dd-125">Content:    Unique for each language in addition to the default language</span></span>
* <span data-ttu-id="fa4dd-126">Målgrupp: Filtreras efter ett villkor för olika språk</span><span class="sxs-lookup"><span data-stu-id="fa4dd-126">Audience:     May be filtered by a separate language criterion</span></span>
* <span data-ttu-id="fa4dd-127">Tidsintervall: samma för alla språk</span><span class="sxs-lookup"><span data-stu-id="fa4dd-127">Time frame:     Same for all languages</span></span>
* <span data-ttu-id="fa4dd-128">Test: Kan skickas till alla språk samtidigt</span><span class="sxs-lookup"><span data-stu-id="fa4dd-128">Test:    May be sent to each language at a time</span></span>

### <a name="supported-languages"></a><span data-ttu-id="fa4dd-129">Språk som stöds:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-129">Supported Languages:</span></span>
* <span data-ttu-id="fa4dd-130">Arabiska (ar)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-130">Arabic (ar)</span></span> 
* <span data-ttu-id="fa4dd-131">Bulgariska (bg)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-131">Bulgarian (bg)</span></span> 
* <span data-ttu-id="fa4dd-132">Katalanska (ca)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-132">Catalan (ca)</span></span> 
* <span data-ttu-id="fa4dd-133">Kinesiska (zh)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-133">Chinese (zh)</span></span> 
* <span data-ttu-id="fa4dd-134">Kroatiska (hr)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-134">Croatian (hr)</span></span> 
* <span data-ttu-id="fa4dd-135">Czech (CS)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-135">Czech (cs)</span></span> 
* <span data-ttu-id="fa4dd-136">Danska (da)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-136">Danish (da)</span></span> 
* <span data-ttu-id="fa4dd-137">Nederländska (nl)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-137">Dutch (nl)</span></span> 
* <span data-ttu-id="fa4dd-138">Engelska (en)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-138">English (en)</span></span> 
* <span data-ttu-id="fa4dd-139">Finska (fi)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-139">Finnish (fi)</span></span> 
* <span data-ttu-id="fa4dd-140">Franska (fr)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-140">French (fr)</span></span> 
* <span data-ttu-id="fa4dd-141">Tyska (de)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-141">German (de)</span></span> 
* <span data-ttu-id="fa4dd-142">Grekiska (el)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-142">Greek (el)</span></span> 
* <span data-ttu-id="fa4dd-143">Hebreiska (han)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-143">Hebrew (he)</span></span> 
* <span data-ttu-id="fa4dd-144">Hindi (hög)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-144">Hindi (hi)</span></span> 
* <span data-ttu-id="fa4dd-145">Ungerska (hu)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-145">Hungarian (hu)</span></span> 
* <span data-ttu-id="fa4dd-146">Indonesiska (id)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-146">Indonesian (id)</span></span> 
* <span data-ttu-id="fa4dd-147">Italienska (it)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-147">Italian (it)</span></span> 
* <span data-ttu-id="fa4dd-148">Japanska (ja)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-148">Japanese (ja)</span></span> 
* <span data-ttu-id="fa4dd-149">Koreanska (ko)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-149">Korean (ko)</span></span> 
* <span data-ttu-id="fa4dd-150">Lettiska (lv)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-150">Latvian (lv)</span></span> 
* <span data-ttu-id="fa4dd-151">Litauiska (lt)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-151">Lithuanian (lt)</span></span> 
* <span data-ttu-id="fa4dd-152">Malajiska (macrolanguage) (ms)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-152">Malay (macrolanguage) (ms)</span></span> 
* <span data-ttu-id="fa4dd-153">Norska Bokmål (nb)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-153">Norwegian Bokmål (nb)</span></span> 
* <span data-ttu-id="fa4dd-154">Polska (pl)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-154">Polish (pl)</span></span> 
* <span data-ttu-id="fa4dd-155">Portugisiska (pt)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-155">Portuguese (pt)</span></span> 
* <span data-ttu-id="fa4dd-156">Rumänska (ro)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-156">Romanian (ro)</span></span> 
* <span data-ttu-id="fa4dd-157">Ryska (ru)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-157">Russian (ru)</span></span> 
* <span data-ttu-id="fa4dd-158">Serbiska (sr)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-158">Serbian (sr)</span></span> 
* <span data-ttu-id="fa4dd-159">Slovakiska (sk)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-159">Slovak (sk)</span></span> 
* <span data-ttu-id="fa4dd-160">Slovenska (sl)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-160">Slovenian (sl)</span></span> 
* <span data-ttu-id="fa4dd-161">Spanska (es)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-161">Spanish (es)</span></span> 
* <span data-ttu-id="fa4dd-162">Svenska (sv)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-162">Swedish (sv)</span></span> 
* <span data-ttu-id="fa4dd-163">Tagalog (tl)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-163">Tagalog (tl)</span></span> 
* <span data-ttu-id="fa4dd-164">Thailändska (th)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-164">Thai (th)</span></span> 
* <span data-ttu-id="fa4dd-165">Turkiska (tr)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-165">Turkish (tr)</span></span> 
* <span data-ttu-id="fa4dd-166">Ukrainska (Storbritannien)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-166">Ukrainian (uk)</span></span> 
* <span data-ttu-id="fa4dd-167">Vietnamesiska (vi)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-167">Vietnamese (vi)</span></span> 

## <a name="campaign"></a><span data-ttu-id="fa4dd-168">Kampanj</span><span class="sxs-lookup"><span data-stu-id="fa4dd-168">Campaign</span></span>
<span data-ttu-id="fa4dd-169">Du kan använda kampanj-avsnittet för att ange namn och kategori för kampanjen samt som om du planerar att ignorera målgrupp för en Push-kampanj och skicka den här kampanjen via API: et nå (och vissa element med låg nivå Push-API) i stället.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-169">You can use the Campaign section to set the name and category of your campaign as well as if you plan to ignore the audience section of a Push campaign and send this campaign via the Reach API (and some elements with the low level Push API) instead.</span></span> <span data-ttu-id="fa4dd-170">Kategorier kan användas med en anpassad aviseringsmall för kontrollen i appen meddelanden baserat på fördefinierade inställningar.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-170">Categories can be used with a custom notification template to control in-app notifications based on predefined settings.</span></span> <span data-ttu-id="fa4dd-171">Du kan hämta en lista över dina befintliga ”kategorier” via Reach-API.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-171">You can get a list of your existing “Categories” via the Reach API.</span></span>

> [!WARNING]
> <span data-ttu-id="fa4dd-172">Om du använder alternativet ”Ignorera målgrupp, push skickas till användarna via API: et” i avsnittet ”kampanj” i en Reach-kampanj kampanjen skickas inte automatiskt, måste skicka manuellt via Reach-API.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-172">If you use the "Ignore Audience, push will be sent to users via the API" option in the "Campaign" section of a Reach campaign, the campaign will NOT automatically send, you will need to send it manually via the Reach API.</span></span>

![Reach-Campaign3][22]

### <a name="option-applies-to"></a><span data-ttu-id="fa4dd-174">Alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-174">Option Applies to:</span></span>
* <span data-ttu-id="fa4dd-175">Namn: alla</span><span class="sxs-lookup"><span data-stu-id="fa4dd-175">Name:    All</span></span>
* <span data-ttu-id="fa4dd-176">Kategori: Meddelanden, undersökningar</span><span class="sxs-lookup"><span data-stu-id="fa4dd-176">Category:    Announcements, Polls</span></span>
* <span data-ttu-id="fa4dd-177">Ignorera målgrupp, push skickas till användare via API: alla</span><span class="sxs-lookup"><span data-stu-id="fa4dd-177">Ignore Audience, push will be sent to users via the API:    All</span></span>

## <a name="notification"></a><span data-ttu-id="fa4dd-178">Avisering</span><span class="sxs-lookup"><span data-stu-id="fa4dd-178">Notification</span></span>
<span data-ttu-id="fa4dd-179">Du kan använda avsnittet meddelande så här anger du grundläggande inställningar för din push inklusive: rubriken för push-meddelandet, meddelandet, en bild i appen, eller om det är stängas.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-179">You can use the Notification section to set basic settings for your push including: The title of the Push, the message, an in-app image, or if it is dismissible.</span></span> <span data-ttu-id="fa4dd-180">Många Meddelandeinställningar är specifika för plattformen för enheten.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-180">Many notification settings are specific to the platform of your device.</span></span> <span data-ttu-id="fa4dd-181">Du kan välja om din push skickas ”app” eller ”out of app” eller båda.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-181">You can select whether your push will be sent "in app" or "out of app" or both.</span></span> <span data-ttu-id="fa4dd-182">(Kom ihåg att användare kan ”välja” eller ”CEIP” ”out-of-app” push-meddelanden på operativsystemet nivå på sina enheter och Azure Mobile Engagement kommer inte att kunna åsidosätta den här inställningen.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-182">(Remember that users can "opt-in" or "opt-out" of "out of app" Pushes at the Operating System level on their devices, and Azure Mobile Engagement will not be able to override this setting.</span></span> <span data-ttu-id="fa4dd-183">Kom ihåg att nå API: et hanterar ”i app” och ”out-of-app” push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-183">Also remember that the Reach API handles "in app" and "out of app" Pushes.</span></span> <span data-ttu-id="fa4dd-184">Push-API kan användas för att hantera ”utanför appen” push-meddelanden för.) Push-meddelanden kan anpassas med bilder eller HTML-innehåll, inklusive djuplänkar för att länka utanför appen eller till en annan plats i din App (Android SDK 2.1.0 eller senare avsiktshantering kategorier krävs).</span><span class="sxs-lookup"><span data-stu-id="fa4dd-184">The Push API can be used to handle "out of app" pushes too.) Pushes can be customized with pictures or HTML content, including deep links for linking outside of your App or to another location in your App (Android SDK 2.1.0 or later intent categories required).</span></span> <span data-ttu-id="fa4dd-185">Du kan ändra ikonen eller iOS-skylt och skicka text eller web innehållet (ett popup-fönster med HTML-innehåll, URL: en länk till en annan plats i eller utanför appen).</span><span class="sxs-lookup"><span data-stu-id="fa4dd-185">You can change the icon or iOS badge, and send either text or web content (a popup with html content, URL link to another location either inside or outside of the app).</span></span> <span data-ttu-id="fa4dd-186">Du kan också göra Android-enheter ring eller vibrerar med push-meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-186">You can also make Android devices ring or vibrate with the Push.</span></span> <span data-ttu-id="fa4dd-187">(Kom ihåg att du måste rätt SDK behörigheter i din Android-manifestfilen om du vill ringa eller vibrerar en enhet.) Det finns för närvarande inga branschen standard för Android ”översikt” storlekar eftersom skärmstorlek skiljer sig på varje enhet, men 400 × 100 bilder fungerar på nästan alla skärmstorlek.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-187">(Remember that you will need the correct SDK permissions in your Android manifest file to ring or vibrate a device.) There is currently no industry standard for Android "Big Picture" sizes, since screen sizes are different on every device, but 400x100 pictures work on almost any screen size.</span></span>

### <a name="delivery-types"></a><span data-ttu-id="fa4dd-188">Leverans typer:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-188">Delivery Types:</span></span>
* <span data-ttu-id="fa4dd-189">Endast utanför appen: meddelandet levereras när användaren inte använda programmet.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-189">Out of app only: the notification will be delivered when the user does not use the application.</span></span>
* <span data-ttu-id="fa4dd-190">Out-of-appavisering som endast kräver ett certifikat från Apple eller Google (APN eller GCM-certifikat).</span><span class="sxs-lookup"><span data-stu-id="fa4dd-190">The out of app only notification requires a certificate from Apple or Google (APNS or GCM certificate).</span></span>
* <span data-ttu-id="fa4dd-191">I appen bara: meddelandet visas endast när programmet körs.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-191">In-app only: The notification appears only when the application is running.</span></span>
* <span data-ttu-id="fa4dd-192">Meddelandet använder systemets Capptain leverans till användaren.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-192">The notification uses the Capptain delivery system to reach the user.</span></span> <span data-ttu-id="fa4dd-193">Du kan anpassa layout/visningen av dina push.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-193">You can fully customize the visual layout/display of your push.</span></span>
* <span data-ttu-id="fa4dd-194">När som helst: Detta alternativ gör att du skickar ett meddelande som antingen programmet körs eller inte.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-194">Anytime: This option ensures that you send a notification either the application is running or not.</span></span>

![Reach-Campaign4][23]

### <a name="option-applies-to"></a><span data-ttu-id="fa4dd-196">Alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-196">Option Applies to:</span></span>
* <span data-ttu-id="fa4dd-197">Meddelande: Meddelanden, undersökningar</span><span class="sxs-lookup"><span data-stu-id="fa4dd-197">Notification:     Announcements, Polls</span></span>

## <a name="content"></a><span data-ttu-id="fa4dd-198">Innehåll</span><span class="sxs-lookup"><span data-stu-id="fa4dd-198">Content</span></span>
<span data-ttu-id="fa4dd-199">Du kan använda avsnittet innehåll för att ändra innehållet i dina meddelanden, avsökningar, Data-push både och paneler (endast Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="fa4dd-199">You can use the Content section to modify the content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="fa4dd-200">Push-kampanjer innehåll inställningen är specifik för typ av kampanj.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-200">The Content setting of Push campaigns is specific to the type of campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="fa4dd-201">Se även</span><span class="sxs-lookup"><span data-stu-id="fa4dd-201">See also</span></span>
* <span data-ttu-id="fa4dd-202">[UI-dokumentation – nå - Push-innehåll][Link 29]</span><span class="sxs-lookup"><span data-stu-id="fa4dd-202">[UI Documentation - Reach - Push Content][Link 29]</span></span>

![Reach-Campaign5][24]

## <a name="audience"></a><span data-ttu-id="fa4dd-204">Målgrupp</span><span class="sxs-lookup"><span data-stu-id="fa4dd-204">Audience</span></span>
<span data-ttu-id="fa4dd-205">Du kan använda målgrupp för att definiera en standard lista över objekt att begränsa din kampanj eller gränser din kampanj baserat på egna villkor.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-205">You can use the Audience section to define a standard list of items to limit your campaign or limits your campaign based on customized criteria.</span></span> <span data-ttu-id="fa4dd-206">Alternativ för att begränsa användarna standarduppsättning kan du push till nya eller gamla användare eller endast interna push-användare.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-206">The standard set of options to Limit your Audience allows you to push to either new or old users or native push users only.</span></span> <span data-ttu-id="fa4dd-207">Du kan också ange en kvot för att begränsa antalet användare som tar emot push-meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-207">You can also set a quota to limit the number of users who receive the push.</span></span> <span data-ttu-id="fa4dd-208">Du kan redigera uttrycket för hur din kampanj filtreras för att inkludera en eller flera villkor till målet användare manuellt.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-208">You can manually Edit the expression for how your campaign is filtered to include one or more criterion to target users.</span></span> <span data-ttu-id="fa4dd-209">Du kan skriva en målgruppsuttrycket manuellt.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-209">You can manually type an audience expression.</span></span> <span data-ttu-id="fa4dd-210">Ett sådant uttryck måste uttryckligen definiera relationen mellan kriterierna.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-210">Such an expression must explicitly define the relation between criteria.</span></span> <span data-ttu-id="fa4dd-211">Ett kriterium beskrivs av en identifierare som måste börja med en bokstav och får inte innehålla blanksteg.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-211">A criterion is described by an identifier that must start with a capital letter and cannot contain spaces.</span></span> <span data-ttu-id="fa4dd-212">Relationen mellan kriterierna kan beskrivas med 'och', 'eller', 'not-operatorer som '(',')'.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-212">The relation between the criteria can be described using 'and', 'or', 'not' operators as well as '(', ')'.</span></span> <span data-ttu-id="fa4dd-213">Exempel: ”Criterion1 or (Criterion1 och inte Criterion2)”.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-213">Example: "Criterion1 or (Criterion1 and not Criterion2)".</span></span>

> [!NOTE]
> <span data-ttu-id="fa4dd-214">Har många ingår i kampanjer, kan serversidan genomsökning som mål vara långsam, särskilt om du försöker starta flera kampanjer på samma gång.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-214">With a large audience included in campaigns, the server side targeting scan can be slow, especially if you attempt to start multiple campaigns at the same time.</span></span>

* <span data-ttu-id="fa4dd-215">Om det är möjligt endast starta en kampanj i taget.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-215">If possible, only start one campaign at a time.</span></span>
* <span data-ttu-id="fa4dd-216">Endast starta på de fyra kampanjer i taget.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-216">At the most, only start four campaigns at a time.</span></span>
* <span data-ttu-id="fa4dd-217">Skicka bara till aktiva användare (kryssrutan ”engagera bara användare som kan nås med hjälp av interna Push” och ”engagera bara aktiva användare”) så att dina användare som fortfarande ha installerat appen och använda den behöver genomsökas.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-217">Push only to your active users (checkbox "Engage only users who can be reached using Native Push" and "Engage only active users") so that only your users who still have the app installed and use it will need to be scanned.</span></span>
  <span data-ttu-id="fa4dd-218">Du kan använda knappen simulera för att ta reda på hur många användare får den här Push när användarna har definierats.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-218">Once your audience is defined, you can use the simulate button to find out how many users will receive this Push.</span></span> <span data-ttu-id="fa4dd-219">Detta kommer beräkna antalet kända användare som kan vara mål för den här målgruppen (detta är en uppskattning baserad på ett slumpvist urval av användare).</span><span class="sxs-lookup"><span data-stu-id="fa4dd-219">This will compute the number of known users potentially targeted by this audience (this is an estimate based on a random sample of users).</span></span> <span data-ttu-id="fa4dd-220">Tänk på att användare som har avinstallerat programmet också ingår i denna målgrupp, men kan inte nås.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-220">Be aware that users who have uninstalled the application are also part of this audience, but cannot be reached.</span></span>

### <a name="see-also"></a><span data-ttu-id="fa4dd-221">Se även</span><span class="sxs-lookup"><span data-stu-id="fa4dd-221">See also</span></span>
* <span data-ttu-id="fa4dd-222">[UI-dokumentationen – Reach - nytt Push kriterium][Link 28]</span><span class="sxs-lookup"><span data-stu-id="fa4dd-222">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

![Reach-Campaign6][25]

### <a name="edit-expression"></a><span data-ttu-id="fa4dd-224">Redigera uttryck</span><span class="sxs-lookup"><span data-stu-id="fa4dd-224">Edit expression</span></span>
![Reach-Campaign7][26]

### <a name="limit-your-audience-option-applies-to"></a><span data-ttu-id="fa4dd-226">Gräns för din målgrupp alternativet gäller för:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-226">Limit your audience option applies to:</span></span>
* <span data-ttu-id="fa4dd-227">Engagera bara en delmängd av användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-227">Engage only a subset of users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="fa4dd-228">Engagera bara gamla användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-228">Engage only old users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="fa4dd-229">Engagera bara nya användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-229">Engage only new users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="fa4dd-230">Engagera bara inaktiva användare: meddelanden, undersökningar, paneler</span><span class="sxs-lookup"><span data-stu-id="fa4dd-230">Engage only idle users:    Announcements, Polls, Tiles</span></span>
* <span data-ttu-id="fa4dd-231">Engagera bara aktiva användare: alla (meddelanden, avsökningar, skickar Data, paneler)</span><span class="sxs-lookup"><span data-stu-id="fa4dd-231">Engage only active users:    All (Announcements, Polls, Data Pushes, Tiles)</span></span>
* <span data-ttu-id="fa4dd-232">Engagera bara användare som kan nås med systemspecifika push-aviseringar: meddelanden, avsökningar</span><span class="sxs-lookup"><span data-stu-id="fa4dd-232">Engage only users who can be reached using Native Push:     Announcements, Polls</span></span>

## <a name="time-frame"></a><span data-ttu-id="fa4dd-233">Tidsintervall</span><span class="sxs-lookup"><span data-stu-id="fa4dd-233">Time Frame</span></span>
<span data-ttu-id="fa4dd-234">Du kan använda avsnittet tidsram för att ange när push-meddelandet ska skickas eller tidsperioden du kan lämna tomt om du vill starta kampanjen direkt.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-234">You can use the Time Frame section to set when the push will be sent or you can leave the time frame blank to start the campaign immediately.</span></span> <span data-ttu-id="fa4dd-235">Kom ihåg att tidszonen för de slutanvändare kan starta med kampanjen en dag tidigare än vad du förväntar dig för dina slutanvändare i Asien och skicka små batchar av push-meddelanden i taget tills alla tidszoner i världen matchar den tidsperiod som angetts för din kampanj.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-235">Remember that using the end-users' time zone may start the campaign a day earlier than you expect for your end-users in Asia and send small batches of pushes at a time until all time zones in the world match the time frame set for your campaign.</span></span> <span data-ttu-id="fa4dd-236">Med hjälp av slutanvändarens tidszon kan också orsaka fördröjningar i kampanjer eftersom den har begära tiden från telefonen innan du startar push-meddelandet.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-236">Using the end users' time zone can also cause delays in campaigns since it has to request the time from the phone before starting the push.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4dd-237">Kampanjer utan ett slutdatum kan cachelagra push-meddelanden lokalt och fortfarande visa dem efter att du manuellt fullständig kampanjer.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-237">Campaigns without an end date can cache pushes locally and still display them after you manually complete campaigns.</span></span> <span data-ttu-id="fa4dd-238">Att undvika det här beteendet specifik sluttid för kampanjer.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-238">To avoid this behavior, specific an end time for campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="fa4dd-239">Se även</span><span class="sxs-lookup"><span data-stu-id="fa4dd-239">See also</span></span>
* <span data-ttu-id="fa4dd-240">[Nå - hur Tos-planering][Link 3]</span><span class="sxs-lookup"><span data-stu-id="fa4dd-240">[Reach - How Tos – Scheduling][Link 3]</span></span> 

![Reach-Campaign8][27]

### <a name="settings-apply-to"></a><span data-ttu-id="fa4dd-242">Inställningar som gäller för:</span><span class="sxs-lookup"><span data-stu-id="fa4dd-242">Settings Apply to:</span></span>
* <span data-ttu-id="fa4dd-243">Tidsintervall: meddelanden, undersökningar, paneler</span><span class="sxs-lookup"><span data-stu-id="fa4dd-243">Time frame:     Announcements, Polls, Tiles</span></span>

## <a name="test"></a><span data-ttu-id="fa4dd-244">Testa</span><span class="sxs-lookup"><span data-stu-id="fa4dd-244">Test</span></span>
<span data-ttu-id="fa4dd-245">Du kan använda avsnittet Test för att skicka den här push till test enheten innan du sparar kampanjen.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-245">You can use the Test section to send this push to your own test device before saving the campaign.</span></span> <span data-ttu-id="fa4dd-246">Om du har konfigurerat en anpassad språk för den här kampanjen kan testa du push-meddelandet på varje språk.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-246">If you have configured any custom languages for this campaign, you can test the push in each language.</span></span> <span data-ttu-id="fa4dd-247">Du kan konfigurera en testenhet från ”mitt konto”.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-247">You can setup a test device from “My Account”.</span></span>

> [!NOTE]
> <span data-ttu-id="fa4dd-248">Inga serversidan data loggas när du använder knappen till ”test” push-meddelanden, data loggas endast för verkliga push-kampanjer.</span><span class="sxs-lookup"><span data-stu-id="fa4dd-248">No server side data is logged when you use the button to "test" pushes, data is only logged for real push campaigns.</span></span>

### <a name="see-also"></a><span data-ttu-id="fa4dd-249">Se även</span><span class="sxs-lookup"><span data-stu-id="fa4dd-249">See also</span></span>
* <span data-ttu-id="fa4dd-250">[UI-dokumentation – mitt konto][Link 14]</span><span class="sxs-lookup"><span data-stu-id="fa4dd-250">[UI Documentation - My Account][Link 14]</span></span>

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

