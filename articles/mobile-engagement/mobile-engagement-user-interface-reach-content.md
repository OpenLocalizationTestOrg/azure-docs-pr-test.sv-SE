---
title: "Användargränssnittet för Azure Mobile Engagement - Reach-innehåll"
description: "Lär dig att hantera unika innehållet i de olika typerna av kampanjer för push-meddelanden i Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: add64f06-43c9-475c-8722-51cd00bb844b
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3741a43b74af5846e95e42d8a7b533621e780f2d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-manage-the-unique-content-of-the-different-types-of-push-notification-campaigns"></a><span data-ttu-id="8786f-103">Så här hanterar du unikt innehållet i de olika typerna av kampanjer för push-meddelande</span><span class="sxs-lookup"><span data-stu-id="8786f-103">How to manage the unique content of the different types of push notification campaigns</span></span>
<span data-ttu-id="8786f-104">Du kan använda avsnittet innehåll för en ny kampanj räckvidden för att ändra innehållet i dina meddelanden, avsökningar, Data-push både och paneler (endast Windows Phone).</span><span class="sxs-lookup"><span data-stu-id="8786f-104">You can use the Content section of a new reach campaign to modify the content of your Announcements, Polls, Data Pushes, and Tiles (Windows Phone only).</span></span> <span data-ttu-id="8786f-105">Push-kampanjer innehåll inställningen är specifik för typ av kampanj.</span><span class="sxs-lookup"><span data-stu-id="8786f-105">The content setting of Push campaigns is specific to the type of campaign.</span></span> 

### <a name="content-types"></a><span data-ttu-id="8786f-106">Typer av innehåll:</span><span class="sxs-lookup"><span data-stu-id="8786f-106">Content types:</span></span>
* <span data-ttu-id="8786f-107">Meddelanden</span><span class="sxs-lookup"><span data-stu-id="8786f-107">Announcements</span></span>
* <span data-ttu-id="8786f-108">Omröstningar</span><span class="sxs-lookup"><span data-stu-id="8786f-108">Polls</span></span>
* <span data-ttu-id="8786f-109">Data push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="8786f-109">Data pushes</span></span>
* <span data-ttu-id="8786f-110">Paneler (endast Windows Phone)</span><span class="sxs-lookup"><span data-stu-id="8786f-110">Tiles (Windows Phone Only)</span></span>

## <a name="content-of-announcements"></a><span data-ttu-id="8786f-111">Innehållet i meddelanden</span><span class="sxs-lookup"><span data-stu-id="8786f-111">Content of Announcements</span></span>
 ![Reach-Content1][30] 

### <a name="choose-the-type-of-your-announcement"></a><span data-ttu-id="8786f-113">Välj typ av ditt meddelande:</span><span class="sxs-lookup"><span data-stu-id="8786f-113">Choose the type of your announcement:</span></span>
* <span data-ttu-id="8786f-114">Endast meddelande: det är ett enkelt standard meddelande.</span><span class="sxs-lookup"><span data-stu-id="8786f-114">Notification only: It is a simple standard notification.</span></span> <span data-ttu-id="8786f-115">Vilket innebär att om en användare klickar på den, ingen extra vy visas, men endast den åtgärd som är kopplade till den sker.</span><span class="sxs-lookup"><span data-stu-id="8786f-115">Meaning that if a user clicks on it, no additional view will appear, but only the action associated to it will occur.</span></span>
* <span data-ttu-id="8786f-116">SMS-meddelande: det är ett meddelande som snabbt tillkallar användaren måste ha en titt på textvyn.</span><span class="sxs-lookup"><span data-stu-id="8786f-116">Text announcement: It is a notification that engages the user to have a look at a text view.</span></span>
* <span data-ttu-id="8786f-117">Web meddelande: det är ett meddelande som snabbt tillkallar användaren att titta på en webbvy.</span><span class="sxs-lookup"><span data-stu-id="8786f-117">Web announcement: It is a notification that engages the user to have a look at a web view.</span></span>

### <a name="see-also"></a><span data-ttu-id="8786f-118">Se även</span><span class="sxs-lookup"><span data-stu-id="8786f-118">See also</span></span>
* <span data-ttu-id="8786f-119">[Nå - hur Tos - meddelanden][Link 3]</span><span class="sxs-lookup"><span data-stu-id="8786f-119">[Reach - How Tos - Announcements][Link 3]</span></span> 

### <a name="about-web-view-announcements"></a><span data-ttu-id="8786f-120">Om Webbvyaviseringar:</span><span class="sxs-lookup"><span data-stu-id="8786f-120">About Web View Announcements:</span></span>
<span data-ttu-id="8786f-121">Förekomster av mönstret ”{deviceid}” i HTML-kod eller JavaScript-kod som du anger här ersätts automatiskt av ID: t för den enhet som visar ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="8786f-121">Occurrences of the pattern "{deviceid}" in the HTML code or JavaScript code you provide here will be automatically replaced by the identifier of the device displaying the announcement.</span></span> <span data-ttu-id="8786f-122">Detta är ett enkelt sätt att hämta Azure Mobile Engagement enhetsidentifierare i en extern webbtjänst som ditt BackOffice är värd.</span><span class="sxs-lookup"><span data-stu-id="8786f-122">This is an easy way to retrieve Azure Mobile Engagement device identifiers in an external web service hosted on your back office.</span></span>
<span data-ttu-id="8786f-123">Om du vill skapa en webbvy i helskärm (utan de standardknappar för Åtgärd och Avsluta som vi tillhandahåller) kan du använda följande funktioner från din webbvyaviserings JavaScript-kod:</span><span class="sxs-lookup"><span data-stu-id="8786f-123">If you want to create a full screen web view (without the default Action and Exit buttons we provide) you can use the following functions from your web view announcement's JavaScript code:</span></span> 

* <span data-ttu-id="8786f-124">genomför Meddelandeåtgärden: ReachContent.actionContent()</span><span class="sxs-lookup"><span data-stu-id="8786f-124">perform the announcement action: ReachContent.actionContent()</span></span>
* <span data-ttu-id="8786f-125">Avsluta från meddelandet: ReachContent.exitContent()</span><span class="sxs-lookup"><span data-stu-id="8786f-125">exit from the announcement: ReachContent.exitContent()</span></span>

### <a name="choose-your-action"></a><span data-ttu-id="8786f-126">Välj åtgärden:</span><span class="sxs-lookup"><span data-stu-id="8786f-126">Choose your Action:</span></span>
### <a name="about-action-urls"></a><span data-ttu-id="8786f-127">Om åtgärden webbadresser:</span><span class="sxs-lookup"><span data-stu-id="8786f-127">About Action URLs:</span></span>
<span data-ttu-id="8786f-128">Varje URL som kan tolkas av en målenhets operativsystem kan användas som en åtgärds-URL.</span><span class="sxs-lookup"><span data-stu-id="8786f-128">Any URL that can be interpreted by a targeted device's operating system can be used as an action URL.</span></span>
<span data-ttu-id="8786f-129">Varje dedikerad URL som ditt program eventuellt stöder (t.ex. för att få användarna att går direkt till en viss skärm) kan också användas som en åtgärds-URL.</span><span class="sxs-lookup"><span data-stu-id="8786f-129">Any dedicated URL that your application might support (e.g. to make users jump to a particular screen) can also be used as an action URL.</span></span>
<span data-ttu-id="8786f-130">Varje förekomst av {deviceid}-mönstret ersätts automatiskt av ID: t för den enhet som genomför åtgärden.</span><span class="sxs-lookup"><span data-stu-id="8786f-130">Each occurrence of the {deviceid} pattern is automatically replaced by the identifier of the device performing the action.</span></span> <span data-ttu-id="8786f-131">Detta kan användas för att lätt kunna hämta Azure Mobile Engagement enhetsidentifierare via en extern webbtjänst som ditt BackOffice är värd.</span><span class="sxs-lookup"><span data-stu-id="8786f-131">This can be used to easily retrieve Azure Mobile Engagement device identifiers via an external web service hosted on your back office.</span></span>

* <span data-ttu-id="8786f-132">**Android och iOS åtgärder**</span><span class="sxs-lookup"><span data-stu-id="8786f-132">**Android + iOS actions**</span></span>
  * <span data-ttu-id="8786f-133">Öppna en webbsida</span><span class="sxs-lookup"><span data-stu-id="8786f-133">Open a web page</span></span>
  * <span data-ttu-id="8786f-134">http://\[web site domäner\]</span><span class="sxs-lookup"><span data-stu-id="8786f-134">http://\[web-site-domain\]</span></span> 
  * <span data-ttu-id="8786f-135">Exempel: http://www.azure.com</span><span class="sxs-lookup"><span data-stu-id="8786f-135">Example:http://www.azure.com</span></span>
  * <span data-ttu-id="8786f-136">Skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="8786f-136">Send an e-mail</span></span>
  * <span data-ttu-id="8786f-137">mailto:\[e-post-mottagaren\]? ämne =\[ämne\]& body =\[meddelande\]</span><span class="sxs-lookup"><span data-stu-id="8786f-137">mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\]</span></span> 
  * <span data-ttu-id="8786f-138">Example:mailto:foo@example.com? ämne = helg % 20from % 20Azure % 20Mobile % 20Engagement! & body = bra % 20stuff!</span><span class="sxs-lookup"><span data-stu-id="8786f-138">Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!</span></span>
  * <span data-ttu-id="8786f-139">Skicka ett SMS</span><span class="sxs-lookup"><span data-stu-id="8786f-139">Send a SMS</span></span>
  * <span data-ttu-id="8786f-140">SMS:\[-telefonnummer\]</span><span class="sxs-lookup"><span data-stu-id="8786f-140">sms:\[phone-number\]</span></span> 
  * <span data-ttu-id="8786f-141">Exempel: sms:2125551212</span><span class="sxs-lookup"><span data-stu-id="8786f-141">Example:sms:2125551212</span></span>
  * <span data-ttu-id="8786f-142">Ring upp ett telefonnummer</span><span class="sxs-lookup"><span data-stu-id="8786f-142">Dial a phone number</span></span>
  * <span data-ttu-id="8786f-143">Tel:\[-telefonnummer\]</span><span class="sxs-lookup"><span data-stu-id="8786f-143">tel:\[phone-number\]</span></span> 
  * <span data-ttu-id="8786f-144">Exempel: tel:2125551212</span><span class="sxs-lookup"><span data-stu-id="8786f-144">Example:tel:2125551212</span></span>
* <span data-ttu-id="8786f-145">**Android endast åtgärder**</span><span class="sxs-lookup"><span data-stu-id="8786f-145">**Android only actions**</span></span>
  * <span data-ttu-id="8786f-146">Hämta ett program på Play Store</span><span class="sxs-lookup"><span data-stu-id="8786f-146">Download an application on the Play Store</span></span>
  * <span data-ttu-id="8786f-147">market://details?ID=\[app-paket\]</span><span class="sxs-lookup"><span data-stu-id="8786f-147">market://details?id=\[app package\]</span></span> 
  * <span data-ttu-id="8786f-148">Exempel: market://details?id=com.microsoft.office.word</span><span class="sxs-lookup"><span data-stu-id="8786f-148">Example:market://details?id=com.microsoft.office.word</span></span>
  * <span data-ttu-id="8786f-149">Starta en geolokaliserad sökning</span><span class="sxs-lookup"><span data-stu-id="8786f-149">Start a geo-localized search</span></span>
  * <span data-ttu-id="8786f-150">GEO:0 0? q =\[sökfråga\]</span><span class="sxs-lookup"><span data-stu-id="8786f-150">geo:0,0?q=\[search query\]</span></span> 
  * <span data-ttu-id="8786f-151">Exempel: geo:0, 0? q = starbucks paris</span><span class="sxs-lookup"><span data-stu-id="8786f-151">Example:geo:0,0?q=starbucks,paris</span></span>
* <span data-ttu-id="8786f-152">**endast åtgärder för iOS**</span><span class="sxs-lookup"><span data-stu-id="8786f-152">**iOS only actions**</span></span>
  * <span data-ttu-id="8786f-153">Hämta ett program på App Store</span><span class="sxs-lookup"><span data-stu-id="8786f-153">Download an application on the App Store</span></span>
  * <span data-ttu-id="8786f-154">http://iTunes.Apple.com/ [Land] /app/ [programnamn] /id [app-id]? huvudmålservern = 8</span><span class="sxs-lookup"><span data-stu-id="8786f-154">http://itunes.apple.com/[country]/app/[app name]/id[app id]?mt=8</span></span> 
  * <span data-ttu-id="8786f-155">Exempel: http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8</span><span class="sxs-lookup"><span data-stu-id="8786f-155">Example:http://itunes.apple.com/fr/app/briquet-virtuel/id430154748?mt=8</span></span>
  * <span data-ttu-id="8786f-156">Windows-åtgärder</span><span class="sxs-lookup"><span data-stu-id="8786f-156">Windows Actions</span></span>
  * <span data-ttu-id="8786f-157">Öppna en webbsida</span><span class="sxs-lookup"><span data-stu-id="8786f-157">Open a web page</span></span>
  * <span data-ttu-id="8786f-158">http://\[web site domäner\]</span><span class="sxs-lookup"><span data-stu-id="8786f-158">http://\[web-site-domain\]</span></span> 
  * <span data-ttu-id="8786f-159">Exempel: http://www.azure.com</span><span class="sxs-lookup"><span data-stu-id="8786f-159">Example:http://www.azure.com</span></span>
  * <span data-ttu-id="8786f-160">Skicka ett e-postmeddelande</span><span class="sxs-lookup"><span data-stu-id="8786f-160">Send an e-mail</span></span>
  * <span data-ttu-id="8786f-161">mailto:\[e-post-mottagaren\]? ämne =\[ämne\]& body =\[meddelande\]</span><span class="sxs-lookup"><span data-stu-id="8786f-161">mailto:\[e-mail-recipient\]?subject=\[subject\]&body=\[message\]</span></span> 
  * <span data-ttu-id="8786f-162">Example:mailto:foo@example.com? ämne = helg % 20from % 20Azure % 20Mobile % 20Engagement! & body = bra % 20stuff!</span><span class="sxs-lookup"><span data-stu-id="8786f-162">Example:mailto:foo@example.com?subject=Greetings%20from%20Azure%20Mobile%20Engagement!&body=Good%20stuff!</span></span>
  * <span data-ttu-id="8786f-163">Skicka ett SMS (Skype Store App krävs)</span><span class="sxs-lookup"><span data-stu-id="8786f-163">Send a SMS (Skype Store App required)</span></span>
  * <span data-ttu-id="8786f-164">SMS:\[-telefonnummer\]</span><span class="sxs-lookup"><span data-stu-id="8786f-164">sms:\[phone-number\]</span></span> 
  * <span data-ttu-id="8786f-165">Exempel: sms:2125551212</span><span class="sxs-lookup"><span data-stu-id="8786f-165">Example:sms:2125551212</span></span>
  * <span data-ttu-id="8786f-166">Ring upp ett telefonnummer (Skype Store App krävs)</span><span class="sxs-lookup"><span data-stu-id="8786f-166">Dial a phone number (Skype Store App required)</span></span>
  * <span data-ttu-id="8786f-167">Tel:\[-telefonnummer\]</span><span class="sxs-lookup"><span data-stu-id="8786f-167">tel:\[phone-number\]</span></span> 
  * <span data-ttu-id="8786f-168">Exempel: tel:2125551212</span><span class="sxs-lookup"><span data-stu-id="8786f-168">Example:tel:2125551212</span></span>
  * <span data-ttu-id="8786f-169">Hämta ett program på Play Store</span><span class="sxs-lookup"><span data-stu-id="8786f-169">Download an application on the Play Store</span></span>
  * <span data-ttu-id="8786f-170">MS-windows-store: PDP? PFN =\[app paket-ID\]</span><span class="sxs-lookup"><span data-stu-id="8786f-170">ms-windows-store:PDP?PFN=\[app package ID\]</span></span> 
  * <span data-ttu-id="8786f-171">Exempel: ms-windows-store: PDP? PFN = 4d91298a-07cb-40fb-aecc-4cb5615d53c1</span><span class="sxs-lookup"><span data-stu-id="8786f-171">Example:ms-windows-store:PDP?PFN=4d91298a-07cb-40fb-aecc-4cb5615d53c1</span></span>
  * <span data-ttu-id="8786f-172">Starta en bingkartssökning</span><span class="sxs-lookup"><span data-stu-id="8786f-172">Start a bingmaps search</span></span>
  * <span data-ttu-id="8786f-173">bingmaps:? q =\[sökfråga\]</span><span class="sxs-lookup"><span data-stu-id="8786f-173">bingmaps:?q=\[search query\]</span></span> 
  * <span data-ttu-id="8786f-174">Exempel: bingmaps:? q = starbucks paris</span><span class="sxs-lookup"><span data-stu-id="8786f-174">Example:bingmaps:?q=starbucks,paris</span></span>
  * <span data-ttu-id="8786f-175">Använd ett anpassat schema</span><span class="sxs-lookup"><span data-stu-id="8786f-175">Use a custom scheme</span></span>
  * <span data-ttu-id="8786f-176">\[anpassat schema\]://\[anpassade schemat parametrar\]</span><span class="sxs-lookup"><span data-stu-id="8786f-176">\[custom scheme\]://\[custom scheme params\]</span></span> 
  * <span data-ttu-id="8786f-177">Exempel: myCustomProtocol://myCustomParams</span><span class="sxs-lookup"><span data-stu-id="8786f-177">Example:myCustomProtocol://myCustomParams</span></span>
  * <span data-ttu-id="8786f-178">Använd paketdata (Store App för tillägg läsa krävs)</span><span class="sxs-lookup"><span data-stu-id="8786f-178">Use a package data (Store App for extension read required)</span></span>
  * <span data-ttu-id="8786f-179">\[mappen\]\[data\].\[ tillägg\]</span><span class="sxs-lookup"><span data-stu-id="8786f-179">\[folder\]\[data\].\[extension\]</span></span> 
  * <span data-ttu-id="8786f-180">Example:myfolderdata.txt</span><span class="sxs-lookup"><span data-stu-id="8786f-180">Example:myfolderdata.txt</span></span>

### <a name="build-a-tracking-url"></a><span data-ttu-id="8786f-181">Skapa en spårning-URL:</span><span class="sxs-lookup"><span data-stu-id="8786f-181">Build a Tracking URL:</span></span>
* <span data-ttu-id="8786f-182">Se avsnittet ”inställningar” i den <UI Documentation> för instruktioner om hur du skapar en spårning URL som gör att användarna kan ladda ned en av dina andra program.</span><span class="sxs-lookup"><span data-stu-id="8786f-182">See the “Settings” section of the <UI Documentation> for instruction on building a tracking URL that will allow users to download one of your other applications.</span></span>

### <a name="define-the-texts-of-your-announcement"></a><span data-ttu-id="8786f-183">Definiera texterna för ditt meddelande</span><span class="sxs-lookup"><span data-stu-id="8786f-183">Define the texts of your announcement</span></span>
<span data-ttu-id="8786f-184">Fyll i rubrik, innehåll och knappen texterna för ditt meddelande.</span><span class="sxs-lookup"><span data-stu-id="8786f-184">Fill in the title, content, and button texts of your announcement.</span></span> <span data-ttu-id="8786f-185">Du kan rikta en målgrupp framtida kampanjens på reach-feedback på hur användare svarat på kampanjen.</span><span class="sxs-lookup"><span data-stu-id="8786f-185">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="8786f-186">Målgrupper kan baseras på feedback på om den här kampanjen har precis pushas, besvarade, hanterats eller stängts.</span><span class="sxs-lookup"><span data-stu-id="8786f-186">Audience targeting can be based on the feedback of whether this campaign was just pushed, replied, actioned, or exited.</span></span>

### <a name="see-also"></a><span data-ttu-id="8786f-187">Se även</span><span class="sxs-lookup"><span data-stu-id="8786f-187">See also</span></span>
* <span data-ttu-id="8786f-188">[UI-dokumentationen – Reach - nytt Push kriterium][Link 28]</span><span class="sxs-lookup"><span data-stu-id="8786f-188">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-polls"></a><span data-ttu-id="8786f-189">Innehållet i avsökningar</span><span class="sxs-lookup"><span data-stu-id="8786f-189">Content of Polls</span></span>
![Reach-Content2][31] 

<span data-ttu-id="8786f-191">Fyll i rubrik, beskrivning och knappen texterna för ditt meddelande.</span><span class="sxs-lookup"><span data-stu-id="8786f-191">Fill in the title, description, and button texts of your announcement.</span></span> <span data-ttu-id="8786f-192">Lägg sedan till frågor och alternativ för svar på dina frågor.</span><span class="sxs-lookup"><span data-stu-id="8786f-192">Then, add questions and choices for the answers to your questions.</span></span>
<span data-ttu-id="8786f-193">Du kan rikta en målgrupp framtida kampanjens på reach-feedback på hur användare svarat på kampanjen.</span><span class="sxs-lookup"><span data-stu-id="8786f-193">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="8786f-194">Målgrupper kan baseras på om den här kampanjen har precis pushas, besvarade, hanterats eller stängts.</span><span class="sxs-lookup"><span data-stu-id="8786f-194">Audience targeting can be based on whether this campaign was just pushed, replied, actioned, or exited.</span></span> <span data-ttu-id="8786f-195">Målgrupper kan även baseras på avsökning besvara feedback, där fråga och svar val användas som kriterier.</span><span class="sxs-lookup"><span data-stu-id="8786f-195">Audience targeting can also be based on Poll answer feedback, where the question and answer choice are used as criteria.</span></span>

### <a name="see-also"></a><span data-ttu-id="8786f-196">Se även</span><span class="sxs-lookup"><span data-stu-id="8786f-196">See also</span></span>
* <span data-ttu-id="8786f-197">[UI-dokumentationen – Reach - nytt Push kriterium][Link 28]</span><span class="sxs-lookup"><span data-stu-id="8786f-197">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-data-pushes"></a><span data-ttu-id="8786f-198">Innehållet i Data push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="8786f-198">Content of Data Pushes</span></span>
![Reach-Content3][32] 

### <a name="choose-the-type-of-your-data"></a><span data-ttu-id="8786f-200">Välj typ av data:</span><span class="sxs-lookup"><span data-stu-id="8786f-200">Choose the type of your data:</span></span>
* <span data-ttu-id="8786f-201">Text</span><span class="sxs-lookup"><span data-stu-id="8786f-201">Text</span></span>
* <span data-ttu-id="8786f-202">Binära data</span><span class="sxs-lookup"><span data-stu-id="8786f-202">Binary data</span></span>
* <span data-ttu-id="8786f-203">Base64-data</span><span class="sxs-lookup"><span data-stu-id="8786f-203">Base64 data</span></span>

### <a name="define-the-content-of-your-data"></a><span data-ttu-id="8786f-204">Definiera dina datas innehåll</span><span class="sxs-lookup"><span data-stu-id="8786f-204">Define the content of your data</span></span>
* <span data-ttu-id="8786f-205">Om du valde för att skicka textdata, kopiera och klistra in texten i rutan ”innehåll”.</span><span class="sxs-lookup"><span data-stu-id="8786f-205">If you selected to push text data, copy and paste the text into the "content" box.</span></span>
* <span data-ttu-id="8786f-206">Om du har valt för att skicka eller binary base64-data kan du använda knappen ”ladda upp filen” för att överföra din fil.</span><span class="sxs-lookup"><span data-stu-id="8786f-206">If you selected to push either binary or base64 data, use the "upload your file" button to upload your file.</span></span>
* <span data-ttu-id="8786f-207">Du kan rikta en målgrupp framtida kampanjens på reach-feedback på hur användare svarat på kampanjen.</span><span class="sxs-lookup"><span data-stu-id="8786f-207">You can target an audience of a future campaign based on the reach feedback of how users responded to this campaign.</span></span> <span data-ttu-id="8786f-208">Målgrupper kan baseras på om den här kampanjen har precis pushas, besvarade, hanterats eller stängts.</span><span class="sxs-lookup"><span data-stu-id="8786f-208">Audience targeting can be based on whether this campaign was just pushed, replied, actioned, or exited.</span></span>

### <a name="see-also"></a><span data-ttu-id="8786f-209">Se även</span><span class="sxs-lookup"><span data-stu-id="8786f-209">See also</span></span>
* <span data-ttu-id="8786f-210">[UI-dokumentationen – Reach - nytt Push kriterium][Link 28]</span><span class="sxs-lookup"><span data-stu-id="8786f-210">[UI Documentation - Reach - New Push Criterion][Link 28]</span></span>

## <a name="content-of-tiles-windows-phone-only"></a><span data-ttu-id="8786f-211">Innehållet i paneler (endast Windows Phone)</span><span class="sxs-lookup"><span data-stu-id="8786f-211">Content of Tiles (Windows Phone only)</span></span>
![Reach-Content4][33]

### <a name="define-the-content-of-your-tile"></a><span data-ttu-id="8786f-213">Definiera din panels innehåll</span><span class="sxs-lookup"><span data-stu-id="8786f-213">Define the content of your tile</span></span>
<span data-ttu-id="8786f-214">Nyttolasten i panelen är text som ska visas i panelen för din app på Windows Phone-enheter.</span><span class="sxs-lookup"><span data-stu-id="8786f-214">The tile payload is the text to be displayed in the tile of your app on Windows Phone devices.</span></span>
<span data-ttu-id="8786f-215">En sida vid sida-push är Microsoft Push Notification Service (MPNS) version av en intern push för Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="8786f-215">A tile push is the Microsoft Push Notification Service (MPNS) version of a native push for Windows Phone.</span></span> <span data-ttu-id="8786f-216">Panelen push-typen är den enda push-typ som inte har ett svar och så målgruppen för framtida kampanjer inte kan byggas på resultatet av en panel push-kampanj.</span><span class="sxs-lookup"><span data-stu-id="8786f-216">The tile push type is the only push type that does not have a response and so the audience of future campaigns can't be built on the results of a tile push campaign.</span></span> 

### <a name="see-also"></a><span data-ttu-id="8786f-217">Se även</span><span class="sxs-lookup"><span data-stu-id="8786f-217">See also</span></span>
* <span data-ttu-id="8786f-218">[API-dokumentationen - Reach-API - interna Push][Link 4]</span><span class="sxs-lookup"><span data-stu-id="8786f-218">[API Documentation - Reach API - Native Push][Link 4]</span></span>

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

