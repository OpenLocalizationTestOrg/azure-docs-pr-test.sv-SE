---
title: "aaaAzure användargränssnitt för Mobile Engagement - nå kriterium"
description: "Lär dig hur toouse målobjekt kriterier toosend push-kampanjer tooa väljer delmängd av dina användare med Azure Mobile Engagement"
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
ms.openlocfilehash: d956add1b7edc1d49451596019c5a4dec098d724
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-targeting-criteria-toosend-push-campaigns-tooa-select-subset-of-your-users"></a><span data-ttu-id="5c9ae-103">Hur toouse målobjekt kriterier toosend push-kampanjer tooa väljer delmängd av dina användare</span><span class="sxs-lookup"><span data-stu-id="5c9ae-103">How toouse targeting criteria toosend push campaigns tooa select subset of your users</span></span>
<span data-ttu-id="5c9ae-104">Målobjekt för användarna efter specifika villkor med hello ”villkor” knappen är en av hello mest kraftfulla koncept i Azure Mobile Engagement som hjälper till att du skickar relevanta push-meddelanden att hello kunder svarar tooinstead av massutskick alla.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-104">Targeting your audience by specific criteria with hello "New Criteria" button is one of hello most powerful concepts in Azure Mobile Engagement that helps you send relevant push notifications that hello customers will respond tooinstead of Spamming everyone.</span></span> <span data-ttu-id="5c9ae-105">Du kan begränsa målgruppen utifrån kriterier som standard och simulera push-meddelanden toodetermine hur många personer får hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-105">You can limit your audience based on standard criteria and simulate pushes toodetermine how many people will receive hello notification.</span></span>

<span data-ttu-id="5c9ae-106">**Se även:**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-106">**See also:**</span></span>

* <span data-ttu-id="5c9ae-107">[UI-dokumentationen – Reach - nya Push-kampanj][Link 27]</span><span class="sxs-lookup"><span data-stu-id="5c9ae-107">[UI Documentation - Reach - New Push Campaign][Link 27]</span></span>

## <a name="audience-criteria-can-include"></a><span data-ttu-id="5c9ae-108">Målgruppen villkor kan omfatta:</span><span class="sxs-lookup"><span data-stu-id="5c9ae-108">Audience criteria can include:</span></span>
* <span data-ttu-id="5c9ae-109">** Technicals: ** du kan rikta utifrån hello samma teknisk information som du kan se i hello Analytics och övervaka avsnitt.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-109">**Technicals: ** You can target based on hello same technical information you can see in hello Analytics and Monitor sections.</span></span> <span data-ttu-id="5c9ae-110">**Se även:** [UI-dokumentation – Analytics][Link 15], [UI-dokumentation – Övervakare][Link 16]</span><span class="sxs-lookup"><span data-stu-id="5c9ae-110">**See also:** [UI Documentation - Analytics][Link 15],  [UI Documentation - Monitor][Link 16]</span></span>
* <span data-ttu-id="5c9ae-111">**Plats:** program som använder ”realtid plats reporting” med Geobegränsning kan använda geografiska plats som ett villkor tootarget en målgrupp från hello GPS-plats.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-111">**Location:** Applications that use "Real time location reporting" with Geo-Fencing can use Geo-Location as a criteria tootarget an audience from hello GPS location.</span></span> <span data-ttu-id="5c9ae-112">Anrop för ”lazy området plats Reporting” också vara används tootarget en målgrupp från hello mobiltelefon platsen (”realtid plats reporting” och ”Lazy området plats rapportering” måste aktiveras från hello SDK).</span><span class="sxs-lookup"><span data-stu-id="5c9ae-112">"Lazy Area Location Reporting" call also be used tootarget an audience from hello cell phone location ("Real time location reporting" and "Lazy Area Location Reporting" must be activated from hello SDK).</span></span> <span data-ttu-id="5c9ae-113">**Se även:** [SDK-dokumentationen – iOS - Integration][Link 5], [SDK-dokumentationen - Android - integrering][Link 5]</span><span class="sxs-lookup"><span data-stu-id="5c9ae-113">**See also:** [SDK Documentation - iOS -  Integration][Link 5], [SDK Documentation - Android -  Integration][Link 5]</span></span>
* <span data-ttu-id="5c9ae-114">**Reach Feedback:** du kan inrikta dig på din målgrupp baserat på deras feedback från tidigare reach-meddelanden via reach feedback från meddelanden, avsökningar och skickar Data.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-114">**Reach Feedback:** You can target your audience based on their feedback from previous reach notifications through reach feedback from Announcements, Polls, and Data Pushes.</span></span> <span data-ttu-id="5c9ae-115">Detta gör att du toobetter mål kan användarna när två eller tre nå kampanjer än du hello första gången.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-115">This enables you toobetter target your audience after two or three reach campaigns than you could hello first time.</span></span> <span data-ttu-id="5c9ae-116">Det kan också användas toofilter ut användare som redan tagit emot ett meddelande med liknande innehåll genom att ange en kampanj tooNOT skickas toousers som redan fått en specifik tidigare kampanj.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-116">It can also be used toofilter out users who already received a notification with similar content, by setting a campaign tooNOT be sent toousers who already received a specific previous campaign.</span></span> <span data-ttu-id="5c9ae-117">Du kan även utesluta användare som ingår en viss kampanj som fortfarande är aktivt tar emot nya push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-117">You can even exclude users who are included a specific campaign that is still active from receiving new Pushes.</span></span> <span data-ttu-id="5c9ae-118">**Se även:** [UI Documentation - Reach - Push-innehållet][Link 29]</span><span class="sxs-lookup"><span data-stu-id="5c9ae-118">**See also:** [UI Documentation -  Reach - Push Content][Link 29]</span></span>
* <span data-ttu-id="5c9ae-119">**Installera spårning:** kan du spåra information baserat på om dina användare installerat appen.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-119">**Install Tracking:** You can track information based on where your users installed your App.</span></span> <span data-ttu-id="5c9ae-120">**Se även:** [UI-dokumentation – inställningar][Link 20]</span><span class="sxs-lookup"><span data-stu-id="5c9ae-120">**See also:** [UI Documentation -  Settings][Link 20]</span></span>
* <span data-ttu-id="5c9ae-121">**Användarprofil:** du kan målet baserat på standardanvändare information och du kan målet baserat på hello anpassade appinfo som du har skapat.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-121">**User Profile:** You can target based on standard user information and you can target based on hello custom app info that you have created.</span></span> <span data-ttu-id="5c9ae-122">Detta omfattar användare som för närvarande är inloggad och användare som har besvarat frågor du har angett dem tooset i hello själva appen i stället för bara hur de har svarat tooprevious kampanjer.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-122">This includes users who are currently logged in and users that have answered specific questions you have asked them tooset in hello app itself instead of just how they have responded tooprevious campaigns.</span></span> <span data-ttu-id="5c9ae-123">Alla App-Info har definierats för din app visas i listan.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-123">All of your App Info's defined for your app show up on this list.</span></span>
* <span data-ttu-id="5c9ae-124">Segment: Du kan också mål baserat på segment som du har skapat baserat på specifika användarbeteende som innehåller flera villkor.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-124">Segments: You can also target based on segments that you have created based on specific user behavior containing multiple criteria.</span></span> <span data-ttu-id="5c9ae-125">Alla segment har definierats för din app visas på den här listan.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-125">All of your segments defined for your app show up on this list.</span></span> <span data-ttu-id="5c9ae-126">**Se även:** [UI-dokumentation – segment][Link 18]</span><span class="sxs-lookup"><span data-stu-id="5c9ae-126">**See also:** [UI Documentation -  Segments][Link 18]</span></span>
* <span data-ttu-id="5c9ae-127">**App-Info:** anpassad App Info taggar kan skapas från ”inställningar” tootrack användarnas beteende.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-127">**App Info:** Custom App Info Tags can be created from “Settings” tootrack user behavior.</span></span> <span data-ttu-id="5c9ae-128">**Se även:** [UI-dokumentation – inställningar][Link 20]</span><span class="sxs-lookup"><span data-stu-id="5c9ae-128">**See also:** [UI Documentation -  Settings][Link 20]</span></span>

## <a name="example"></a><span data-ttu-id="5c9ae-129">Exempel:</span><span class="sxs-lookup"><span data-stu-id="5c9ae-129">Example:</span></span>
<span data-ttu-id="5c9ae-130">Om du vill toopush köpa ett meddelande endast toohello underordnade uppsättning dina användare som har utfört en app åtgärd.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-130">If you want toopush an announcement only toohello sub-set of your users that have performed an in-app purchase action.</span></span>

1. <span data-ttu-id="5c9ae-131">Gå tooyour programinställningssidan väljer hello ”App-info”-menyn och väljer du ”ny app-info”</span><span class="sxs-lookup"><span data-stu-id="5c9ae-131">Go tooyour application settings page, select hello "App info" menu and select "New app info"</span></span>
2. <span data-ttu-id="5c9ae-132">Registrera en ny booleskt appinfo som kallas ”inAppPurchase”</span><span class="sxs-lookup"><span data-stu-id="5c9ae-132">Register a new Boolean app info called "inAppPurchase"</span></span>
3. <span data-ttu-id="5c9ae-133">Kontrollera ditt program ange appinfon för ”true” när hello användare har en köp i appen (med hjälp av hello sendAppInfo (”inAppPurchase”,...) funktionen)</span><span class="sxs-lookup"><span data-stu-id="5c9ae-133">Make your application set this app info too"true" when hello user successfully performs an in-app purchase (by using hello sendAppInfo("inAppPurchase", ...) function)</span></span>
4. <span data-ttu-id="5c9ae-134">Om du inte vill toodo detta från ditt program kan göra du det från din serverdel med hello device API)</span><span class="sxs-lookup"><span data-stu-id="5c9ae-134">If you don't want toodo this from your application, you can do it from your backend by using hello device API)</span></span>
5. <span data-ttu-id="5c9ae-135">Du måste sedan bara toocreate ditt meddelande med ett villkor som begränsar din målgrupp toousers med ”inAppPurchase” anges för ”true”)</span><span class="sxs-lookup"><span data-stu-id="5c9ae-135">Then, you just need toocreate your announcement, with a criterion limiting your audience toousers having "inAppPurchase" set too"true")</span></span>

> [!NOTE]
> <span data-ttu-id="5c9ae-136">Målobjekt för baserat på kriterier än taggar för app-info kräver Azure Mobile Engagement toogather information från användarnas enheter innan hello push skickas och det kan det uppstå en fördröjning.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-136">Targeting based on criteria other than app info tags requires Azure Mobile Engagement toogather information from your users' devices before hello push is sent and so can cause a delay.</span></span> <span data-ttu-id="5c9ae-137">Komplexa push-konfiguration som alternativ (t.ex. uppdatera Aktivitetsikoner) kan också vänta med push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-137">Complex push configuration options (like updating badges) can also delay pushes.</span></span> <span data-ttu-id="5c9ae-138">Hello absolut snabbaste metoden i Azure Mobile Engagement är att använda en ”en bild” kampanj från hello Push-API.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-138">Using a "one shot" campaign from hello Push API is hello absolute fastest push method in Azure Mobile Engagement.</span></span> <span data-ttu-id="5c9ae-139">Med bara app info taggar som push villkor för en Reach-kampanj (antingen från hello nå API eller hello UI) är hello nästa snabbaste metoden eftersom taggar för app-info lagras på hello på serversidan.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-139">Using only app info tags as push criteria for a Reach campaign (either from hello Reach API or hello UI) is hello next fastest method since app info tags are stored on hello server side.</span></span> <span data-ttu-id="5c9ae-140">Med andra målobjekt kriterier för en push-kampanj är hello mest flexibla men långsammaste push metoden eftersom Azure Mobile Engagement har tooquery hello enheter i ordning toosend hello kampanj.</span><span class="sxs-lookup"><span data-stu-id="5c9ae-140">Using other targeting criteria for a push campaign is hello most flexible but slowest push method since Azure Mobile Engagement has tooquery hello devices in order toosend hello campaign.</span></span>

![Reach-Criterion1][29] 

## <a name="criterion-options-apply-to"></a><span data-ttu-id="5c9ae-142">Alternativ för villkor gäller:</span><span class="sxs-lookup"><span data-stu-id="5c9ae-142">Criterion Options Apply to:</span></span>
* <span data-ttu-id="5c9ae-143">**Technicals**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-143">**Technicals**</span></span>     
* <span data-ttu-id="5c9ae-144">Inbyggd name: Firmware namn</span><span class="sxs-lookup"><span data-stu-id="5c9ae-144">Firmware name:    Firmware name</span></span>
* <span data-ttu-id="5c9ae-145">Version på inbyggd programvara: version på inbyggd programvara</span><span class="sxs-lookup"><span data-stu-id="5c9ae-145">Firmware version:    Firmware version</span></span>
* <span data-ttu-id="5c9ae-146">Enhetsmodell: enhetsmodell</span><span class="sxs-lookup"><span data-stu-id="5c9ae-146">Device model:    Device model</span></span>
* <span data-ttu-id="5c9ae-147">Enhetstillverkare: enhetstillverkare</span><span class="sxs-lookup"><span data-stu-id="5c9ae-147">Device manufacturer:    Device manufacturer</span></span>
* <span data-ttu-id="5c9ae-148">Programversion: programversion</span><span class="sxs-lookup"><span data-stu-id="5c9ae-148">Application version:    Application version</span></span>
* <span data-ttu-id="5c9ae-149">Operatör name: operatör namn, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-149">Carrier name:    Carrier name, undefined</span></span>
* <span data-ttu-id="5c9ae-150">Operatör land: operatör land, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-150">Carrier country:    Carrier country, undefined</span></span>
* <span data-ttu-id="5c9ae-151">Nätverkstyp: typen</span><span class="sxs-lookup"><span data-stu-id="5c9ae-151">Network type:    Network type</span></span>
* <span data-ttu-id="5c9ae-152">Språk: språk</span><span class="sxs-lookup"><span data-stu-id="5c9ae-152">Locale:    Locale</span></span>
* <span data-ttu-id="5c9ae-153">Skärmstorlek: skärmstorlek</span><span class="sxs-lookup"><span data-stu-id="5c9ae-153">Screen size:    Screen size</span></span>
* <span data-ttu-id="5c9ae-154">**Plats**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-154">**Location**</span></span>      
* <span data-ttu-id="5c9ae-155">Senast kända område: land, Region, ort</span><span class="sxs-lookup"><span data-stu-id="5c9ae-155">Last known area:    Country, Region, Locality</span></span>
* <span data-ttu-id="5c9ae-156">Realtid geobegränsning: listan över PoI (namn, åtgärder), cirkulär POI (namn, latitud, long, RADIUS-i mätare)</span><span class="sxs-lookup"><span data-stu-id="5c9ae-156">Real time geo-fencing:    List of POIs (Name, Actions), Circular POI (Name, Latitude, Longitude, Radius in meters)</span></span>
* <span data-ttu-id="5c9ae-157">**Nå feedback**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-157">**Reach feedback**</span></span>     
* <span data-ttu-id="5c9ae-158">Meddelande feedback: meddelande, feedback</span><span class="sxs-lookup"><span data-stu-id="5c9ae-158">Announcement feedback:    Announcement, feedback</span></span>
* <span data-ttu-id="5c9ae-159">Avsöka feedback: avsökning feedback</span><span class="sxs-lookup"><span data-stu-id="5c9ae-159">Poll feedback:    Poll, feedback</span></span>
* <span data-ttu-id="5c9ae-160">Söka svar feedback: söka svar feedback, fråga, val</span><span class="sxs-lookup"><span data-stu-id="5c9ae-160">Poll answer feedback:    Poll answer feedback, question, choice</span></span>
* <span data-ttu-id="5c9ae-161">Data Push feedback: Data-Push, feedback</span><span class="sxs-lookup"><span data-stu-id="5c9ae-161">Data Push feedback:    Data Push, feedback</span></span>
* <span data-ttu-id="5c9ae-162">**Installera spårning**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-162">**Install Tracking**</span></span>     
* <span data-ttu-id="5c9ae-163">Arkiv: Arkiv Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-163">Store:    Store, Undefined</span></span>
* <span data-ttu-id="5c9ae-164">Källa: Källa, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-164">Source:    Source, Undefined</span></span>
* <span data-ttu-id="5c9ae-165">**Användarprofil**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-165">**User profile**</span></span>     
* <span data-ttu-id="5c9ae-166">Kön: man eller kvinnligt Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-166">Gender:    male or female, undefined</span></span>
* <span data-ttu-id="5c9ae-167">Födelsedatum datum: operatör, datum, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-167">Birth date:    operator, date, undefined</span></span>
* <span data-ttu-id="5c9ae-168">Anmäl: SANT eller FALSKT, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-168">Opt-in:    true or false, undefined</span></span>
* <span data-ttu-id="5c9ae-169">**App-Info**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-169">**App Info**</span></span>      
* <span data-ttu-id="5c9ae-170">Sträng: Sträng, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-170">String:    String, undefined</span></span>
* <span data-ttu-id="5c9ae-171">Datum: operatör, datum, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-171">Date:    operator, date, undefined</span></span>
* <span data-ttu-id="5c9ae-172">Heltal: operatorn, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-172">Integer:    operator, number, undefined</span></span>
* <span data-ttu-id="5c9ae-173">Booleskt värde: true eller false, Odefinierad</span><span class="sxs-lookup"><span data-stu-id="5c9ae-173">Boolean:    true or false, undefined</span></span>
* <span data-ttu-id="5c9ae-174">**Segment**</span><span class="sxs-lookup"><span data-stu-id="5c9ae-174">**Segment**</span></span>    
* <span data-ttu-id="5c9ae-175">Namnet på segment (från listrutan) undantag (target användare som inte är en del av det här segmentet).</span><span class="sxs-lookup"><span data-stu-id="5c9ae-175">Name of Segments (from dropdown list), Exclusion (target users that are not a part of this segment).</span></span>

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

