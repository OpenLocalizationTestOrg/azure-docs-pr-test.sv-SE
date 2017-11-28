---
title: "Användargränssnittet för Azure Mobile Engagement - Reach kriterium"
description: "Lär dig hur du använder målobjekt kriterier för att skicka push-kampanjer till en del av dina användare med Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: a4ed03a0-55b1-4dd8-b0bd-c475005afb66
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 803b44721d0ab1ac7b5a8074e18857fc57adb724
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-targeting-criteria-to-send-push-campaigns-to-a-select-subset-of-your-users"></a><span data-ttu-id="bdfce-103">Hur du använder målobjekt kriterier för att skicka push-kampanjer till en del av dina användare</span><span class="sxs-lookup"><span data-stu-id="bdfce-103">How to use targeting criteria to send push campaigns to a select subset of your users</span></span>
<span data-ttu-id="bdfce-104">Målobjekt för användarna efter specifika villkor med knappen ”villkor” är en av de mest kraftfulla begrepp som hjälper till att du skickar relevanta push-meddelanden i Azure Mobile Engagement att kunderna svarar på i stället för att skicka alla.</span><span class="sxs-lookup"><span data-stu-id="bdfce-104">Targeting your audience by specific criteria with the "New Criteria" button is one of the most powerful concepts in Azure Mobile Engagement that helps you send relevant push notifications that the customers will respond to instead of Spamming everyone.</span></span> <span data-ttu-id="bdfce-105">Du kan begränsa målgruppen utifrån kriterier som standard och simulera push-meddelanden att avgöra hur många som tar emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="bdfce-105">You can limit your audience based on standard criteria and simulate pushes to determine how many people will receive the notification.</span></span>

<span data-ttu-id="bdfce-106">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="bdfce-106">**See also:**</span></span>

* <span data-ttu-id="bdfce-107">[UI-dokumentationen – Reach - nya Push-kampanj][Link 27]</span><span class="sxs-lookup"><span data-stu-id="bdfce-107">[UI Documentation - Reach - New Push Campaign][Link 27]</span></span>

## <a name="audience-criteria-can-include"></a><span data-ttu-id="bdfce-108">Målgruppen villkor kan omfatta:</span><span class="sxs-lookup"><span data-stu-id="bdfce-108">Audience criteria can include:</span></span>
* <span data-ttu-id="bdfce-109">** Technicals: ** mål baserat på samma teknisk information hittar du i avsnitten Analytics och Övervakare.</span><span class="sxs-lookup"><span data-stu-id="bdfce-109">**Technicals: ** You can target based on the same technical information you can see in the Analytics and Monitor sections.</span></span> <span data-ttu-id="bdfce-110">**Se även:** [UI-dokumentation – Analytics][Link 15], [UI-dokumentation – Övervakare][Link 16]</span><span class="sxs-lookup"><span data-stu-id="bdfce-110">**See also:** [UI Documentation - Analytics][Link 15],  [UI Documentation - Monitor][Link 16]</span></span>
* <span data-ttu-id="bdfce-111">**Plats:** program som använder ”realtid plats reporting” med Geobegränsning kan använda geografiska plats som ett villkor för att rikta en målgrupp från GPS-plats.</span><span class="sxs-lookup"><span data-stu-id="bdfce-111">**Location:** Applications that use "Real time location reporting" with Geo-Fencing can use Geo-Location as a criteria to target an audience from the GPS location.</span></span> <span data-ttu-id="bdfce-112">”Lazy området plats Reporting” anrop också användas för att rikta en målgrupp från mobiltelefonen plats (”realtid plats reporting” och ”Lazy området plats rapportering” måste aktiveras från SDK).</span><span class="sxs-lookup"><span data-stu-id="bdfce-112">"Lazy Area Location Reporting" call also be used to target an audience from the cell phone location ("Real time location reporting" and "Lazy Area Location Reporting" must be activated from the SDK).</span></span> <span data-ttu-id="bdfce-113">**Se även:** [SDK-dokumentationen – iOS - Integration][Link 5], [SDK-dokumentationen - Android - integrering][Link 5]</span><span class="sxs-lookup"><span data-stu-id="bdfce-113">**See also:** [SDK Documentation - iOS -  Integration][Link 5], [SDK Documentation - Android -  Integration][Link 5]</span></span>
* <span data-ttu-id="bdfce-114">**Reach Feedback:** du kan inrikta dig på din målgrupp baserat på deras feedback från tidigare reach-meddelanden via reach feedback från meddelanden, avsökningar och skickar Data.</span><span class="sxs-lookup"><span data-stu-id="bdfce-114">**Reach Feedback:** You can target your audience based on their feedback from previous reach notifications through reach feedback from Announcements, Polls, and Data Pushes.</span></span> <span data-ttu-id="bdfce-115">Detta gör det möjligt att bättre mål målgruppen efter två eller tre nå kampanjer än du skulle kunna första gången.</span><span class="sxs-lookup"><span data-stu-id="bdfce-115">This enables you to better target your audience after two or three reach campaigns than you could the first time.</span></span> <span data-ttu-id="bdfce-116">Det kan också användas för att filtrera bort användare som redan tagit emot ett meddelande med liknande innehåll genom att ange en kampanj som inte ska skickas till användare som redan tagit emot en specifik tidigare kampanj.</span><span class="sxs-lookup"><span data-stu-id="bdfce-116">It can also be used to filter out users who already received a notification with similar content, by setting a campaign to NOT be sent to users who already received a specific previous campaign.</span></span> <span data-ttu-id="bdfce-117">Du kan även utesluta användare som ingår en viss kampanj som fortfarande är aktivt tar emot nya push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bdfce-117">You can even exclude users who are included a specific campaign that is still active from receiving new Pushes.</span></span> <span data-ttu-id="bdfce-118">**Se även:** [UI Documentation - Reach - Push-innehållet][Link 29]</span><span class="sxs-lookup"><span data-stu-id="bdfce-118">**See also:** [UI Documentation -  Reach - Push Content][Link 29]</span></span>
* <span data-ttu-id="bdfce-119">**Installera spårning:** kan du spåra information baserat på om dina användare installerat appen.</span><span class="sxs-lookup"><span data-stu-id="bdfce-119">**Install Tracking:** You can track information based on where your users installed your App.</span></span> <span data-ttu-id="bdfce-120">**Se även:** [UI-dokumentation – inställningar][Link 20]</span><span class="sxs-lookup"><span data-stu-id="bdfce-120">**See also:** [UI Documentation -  Settings][Link 20]</span></span>
* <span data-ttu-id="bdfce-121">**Användarprofil:** du kan målet baserat på standardanvändare information och du kan målet baserat på den information som anpassad app som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="bdfce-121">**User Profile:** You can target based on standard user information and you can target based on the custom app info that you have created.</span></span> <span data-ttu-id="bdfce-122">Detta omfattar användare som för närvarande är inloggad och användare som har besvarat frågor du har angett dem som i själva appen i stället för bara hur de har svarat på tidigare kampanjer.</span><span class="sxs-lookup"><span data-stu-id="bdfce-122">This includes users who are currently logged in and users that have answered specific questions you have asked them to set in the app itself instead of just how they have responded to previous campaigns.</span></span> <span data-ttu-id="bdfce-123">Alla App-Info har definierats för din app visas i listan.</span><span class="sxs-lookup"><span data-stu-id="bdfce-123">All of your App Info's defined for your app show up on this list.</span></span>
* <span data-ttu-id="bdfce-124">Segment: Du kan också mål baserat på segment som du har skapat baserat på specifika användarbeteende som innehåller flera villkor.</span><span class="sxs-lookup"><span data-stu-id="bdfce-124">Segments: You can also target based on segments that you have created based on specific user behavior containing multiple criteria.</span></span> <span data-ttu-id="bdfce-125">Alla segment har definierats för din app visas på den här listan.</span><span class="sxs-lookup"><span data-stu-id="bdfce-125">All of your segments defined for your app show up on this list.</span></span> <span data-ttu-id="bdfce-126">**Se även:** [UI-dokumentation – segment][Link 18]</span><span class="sxs-lookup"><span data-stu-id="bdfce-126">**See also:** [UI Documentation -  Segments][Link 18]</span></span>
* <span data-ttu-id="bdfce-127">**App-Info:** anpassad App Info taggar kan skapas från ”inställningar” för att spåra användarens beteende.</span><span class="sxs-lookup"><span data-stu-id="bdfce-127">**App Info:** Custom App Info Tags can be created from “Settings” to track user behavior.</span></span> <span data-ttu-id="bdfce-128">**Se även:** [UI-dokumentation – inställningar][Link 20]</span><span class="sxs-lookup"><span data-stu-id="bdfce-128">**See also:** [UI Documentation -  Settings][Link 20]</span></span>

## <a name="example"></a><span data-ttu-id="bdfce-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="bdfce-129">Example:</span></span>
<span data-ttu-id="bdfce-130">Om du vill skicka ett meddelande endast till de underordnade dina användare som har utfört en åtgärd för köp i appen.</span><span class="sxs-lookup"><span data-stu-id="bdfce-130">If you want to push an announcement only to the sub-set of your users that have performed an in-app purchase action.</span></span>

1. <span data-ttu-id="bdfce-131">Gå till inställningssidan för program, väljer ”App-info”-menyn och väljer ”nya app-info”</span><span class="sxs-lookup"><span data-stu-id="bdfce-131">Go to your application settings page, select the "App info" menu and select "New app info"</span></span>
2. <span data-ttu-id="bdfce-132">Registrera en ny booleskt appinfo som kallas ”inAppPurchase”</span><span class="sxs-lookup"><span data-stu-id="bdfce-132">Register a new Boolean app info called "inAppPurchase"</span></span>
3. <span data-ttu-id="bdfce-133">Göra programmet ange appinfon till ”true” när användaren utför har ett inköp i appen (med hjälp av sendAppInfo (”inAppPurchase”,...) funktionen)</span><span class="sxs-lookup"><span data-stu-id="bdfce-133">Make your application set this app info to "true" when the user successfully performs an in-app purchase (by using the sendAppInfo("inAppPurchase", ...) function)</span></span>
4. <span data-ttu-id="bdfce-134">Om du inte vill göra det från ditt program, kan du göra det från din serverdel med hjälp av device API)</span><span class="sxs-lookup"><span data-stu-id="bdfce-134">If you don't want to do this from your application, you can do it from your backend by using the device API)</span></span>
5. <span data-ttu-id="bdfce-135">Sedan måste du skapa ditt meddelande med ett kriterium begränsa användarna till användare som har ”inAppPurchase” inställd på ”true”)</span><span class="sxs-lookup"><span data-stu-id="bdfce-135">Then, you just need to create your announcement, with a criterion limiting your audience to users having "inAppPurchase" set to "true")</span></span>

> [!NOTE]
> <span data-ttu-id="bdfce-136">Målobjekt för baserat på kriterier än taggar för app-info kräver Azure Mobile Engagement att samla in information från användarnas enheter innan push skickas och så kan orsaka en fördröjning.</span><span class="sxs-lookup"><span data-stu-id="bdfce-136">Targeting based on criteria other than app info tags requires Azure Mobile Engagement to gather information from your users' devices before the push is sent and so can cause a delay.</span></span> <span data-ttu-id="bdfce-137">Komplexa push-konfiguration som alternativ (t.ex. uppdatera Aktivitetsikoner) kan också vänta med push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bdfce-137">Complex push configuration options (like updating badges) can also delay pushes.</span></span> <span data-ttu-id="bdfce-138">Använda en ”en bild” kampanj från Push-API: et är absolut snabbaste metoden i Azure Mobile Engagement.</span><span class="sxs-lookup"><span data-stu-id="bdfce-138">Using a "one shot" campaign from the Push API is the absolute fastest push method in Azure Mobile Engagement.</span></span> <span data-ttu-id="bdfce-139">Med bara app info taggar som push villkor för en Reach-kampanj (antingen från API: et nå eller Användargränssnittet) är den snabbaste metoden nästa eftersom taggar för app-info lagras på serversidan.</span><span class="sxs-lookup"><span data-stu-id="bdfce-139">Using only app info tags as push criteria for a Reach campaign (either from the Reach API or the UI) is the next fastest method since app info tags are stored on the server side.</span></span> <span data-ttu-id="bdfce-140">Med andra målobjekt kriterier för en push-kampanj är den mest flexibla men långsammaste push-metoden eftersom Azure Mobile Engagement har fråga enheterna för att kunna skicka kampanjen.</span><span class="sxs-lookup"><span data-stu-id="bdfce-140">Using other targeting criteria for a push campaign is the most flexible but slowest push method since Azure Mobile Engagement has to query the devices in order to send the campaign.</span></span>

![Reach-Criterion1][29] 

## <a name="criterion-options-apply-to"></a><span data-ttu-id="bdfce-142">Alternativ för villkor gäller:</span><span class="sxs-lookup"><span data-stu-id="bdfce-142">Criterion Options Apply to:</span></span>
* <span data-ttu-id="bdfce-143">**Technicals**</span><span class="sxs-lookup"><span data-stu-id="bdfce-143">**Technicals**</span></span>     
* <span data-ttu-id="bdfce-144">Inbyggd name: Firmware namn</span><span class="sxs-lookup"><span data-stu-id="bdfce-144">Firmware name:    Firmware name</span></span>
* <span data-ttu-id="bdfce-145">Version på inbyggd programvara: version på inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="bdfce-145">Firmware version:    Firmware version</span></span>
* <span data-ttu-id="bdfce-146">Enhetsmodell: enhetsmodell</span><span class="sxs-lookup"><span data-stu-id="bdfce-146">Device model:    Device model</span></span>
* <span data-ttu-id="bdfce-147">Enhetstillverkare: enhetstillverkare</span><span class="sxs-lookup"><span data-stu-id="bdfce-147">Device manufacturer:    Device manufacturer</span></span>
* <span data-ttu-id="bdfce-148">Programversion: programversion</span><span class="sxs-lookup"><span data-stu-id="bdfce-148">Application version:    Application version</span></span>
* <span data-ttu-id="bdfce-149">Operatör name: operatör namn, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-149">Carrier name:    Carrier name, undefined</span></span>
* <span data-ttu-id="bdfce-150">Operatör land: operatör land, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-150">Carrier country:    Carrier country, undefined</span></span>
* <span data-ttu-id="bdfce-151">Nätverkstyp: typen</span><span class="sxs-lookup"><span data-stu-id="bdfce-151">Network type:    Network type</span></span>
* <span data-ttu-id="bdfce-152">Språk: språk</span><span class="sxs-lookup"><span data-stu-id="bdfce-152">Locale:    Locale</span></span>
* <span data-ttu-id="bdfce-153">Skärmstorlek: skärmstorlek</span><span class="sxs-lookup"><span data-stu-id="bdfce-153">Screen size:    Screen size</span></span>
* <span data-ttu-id="bdfce-154">**Plats**</span><span class="sxs-lookup"><span data-stu-id="bdfce-154">**Location**</span></span>      
* <span data-ttu-id="bdfce-155">Senast kända område: land, Region, ort</span><span class="sxs-lookup"><span data-stu-id="bdfce-155">Last known area:    Country, Region, Locality</span></span>
* <span data-ttu-id="bdfce-156">Realtid geobegränsning: listan över PoI (namn, åtgärder), cirkulär POI (namn, latitud, long, RADIUS-i mätare)</span><span class="sxs-lookup"><span data-stu-id="bdfce-156">Real time geo-fencing:    List of POIs (Name, Actions), Circular POI (Name, Latitude, Longitude, Radius in meters)</span></span>
* <span data-ttu-id="bdfce-157">**Nå feedback**</span><span class="sxs-lookup"><span data-stu-id="bdfce-157">**Reach feedback**</span></span>     
* <span data-ttu-id="bdfce-158">Meddelande feedback: meddelande, feedback</span><span class="sxs-lookup"><span data-stu-id="bdfce-158">Announcement feedback:    Announcement, feedback</span></span>
* <span data-ttu-id="bdfce-159">Avsöka feedback: avsökning feedback</span><span class="sxs-lookup"><span data-stu-id="bdfce-159">Poll feedback:    Poll, feedback</span></span>
* <span data-ttu-id="bdfce-160">Söka svar feedback: söka svar feedback, fråga, val</span><span class="sxs-lookup"><span data-stu-id="bdfce-160">Poll answer feedback:    Poll answer feedback, question, choice</span></span>
* <span data-ttu-id="bdfce-161">Data Push feedback: Data-Push, feedback</span><span class="sxs-lookup"><span data-stu-id="bdfce-161">Data Push feedback:    Data Push, feedback</span></span>
* <span data-ttu-id="bdfce-162">**Installera spårning**</span><span class="sxs-lookup"><span data-stu-id="bdfce-162">**Install Tracking**</span></span>     
* <span data-ttu-id="bdfce-163">Arkiv: Arkiv Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-163">Store:    Store, Undefined</span></span>
* <span data-ttu-id="bdfce-164">Källa: Källa, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-164">Source:    Source, Undefined</span></span>
* <span data-ttu-id="bdfce-165">**Användarprofil**</span><span class="sxs-lookup"><span data-stu-id="bdfce-165">**User profile**</span></span>     
* <span data-ttu-id="bdfce-166">Kön: man eller kvinnligt Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-166">Gender:    male or female, undefined</span></span>
* <span data-ttu-id="bdfce-167">Födelsedatum datum: operatör, datum, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-167">Birth date:    operator, date, undefined</span></span>
* <span data-ttu-id="bdfce-168">Anmäl: SANT eller FALSKT, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-168">Opt-in:    true or false, undefined</span></span>
* <span data-ttu-id="bdfce-169">**App-Info**</span><span class="sxs-lookup"><span data-stu-id="bdfce-169">**App Info**</span></span>      
* <span data-ttu-id="bdfce-170">Sträng: Sträng, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-170">String:    String, undefined</span></span>
* <span data-ttu-id="bdfce-171">Datum: operatör, datum, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-171">Date:    operator, date, undefined</span></span>
* <span data-ttu-id="bdfce-172">Heltal: operatorn, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-172">Integer:    operator, number, undefined</span></span>
* <span data-ttu-id="bdfce-173">Booleskt värde: true eller false, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="bdfce-173">Boolean:    true or false, undefined</span></span>
* <span data-ttu-id="bdfce-174">**Segment**</span><span class="sxs-lookup"><span data-stu-id="bdfce-174">**Segment**</span></span>    
* <span data-ttu-id="bdfce-175">Namnet på segment (från listrutan) undantag (target användare som inte är en del av det här segmentet).</span><span class="sxs-lookup"><span data-stu-id="bdfce-175">Name of Segments (from dropdown list), Exclusion (target users that are not a part of this segment).</span></span>

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

