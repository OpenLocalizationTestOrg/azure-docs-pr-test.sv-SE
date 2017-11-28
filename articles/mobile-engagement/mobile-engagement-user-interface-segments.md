---
title: "Användargränssnittet för Azure Mobile Engagement - segment"
description: "Lär dig att skapa och hantera användarsegment för att identifiera användningsmönster med Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: 6a4f2205-4a3c-406e-a04f-5e6f2a36653f
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 087f4a1fef420abe9669f8dfe2b84c7a847ce263
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-create-and-manage-segments-of-users-to-identify-usage-patterns"></a><span data-ttu-id="9917d-103">Skapa och hantera användarsegment för att identifiera användningsmönster</span><span class="sxs-lookup"><span data-stu-id="9917d-103">How to create and manage segments of users to identify usage patterns</span></span>
<span data-ttu-id="9917d-104">Den här artikeln beskriver den **segment** för den **Mobile Engagement** portal.</span><span class="sxs-lookup"><span data-stu-id="9917d-104">This article describes the **SEGMENTS** tab of the **Mobile Engagement** portal.</span></span> <span data-ttu-id="9917d-105">Du använder den **Mobile Engagement** portalen för att övervaka och hantera dina mobila appar.</span><span class="sxs-lookup"><span data-stu-id="9917d-105">You use the **Mobile Engagement** portal to monitor and manage your mobile apps.</span></span>

<span data-ttu-id="9917d-106">Avsnittet segment i Användargränssnittet kan du arbeta på segmentera dina användare baserat på olika beteenden och analyser som du kan hämta från programmet och kan också komma åt via API segment.</span><span class="sxs-lookup"><span data-stu-id="9917d-106">The Segments section of the UI allows you to work on segmenting your users based on the different behavior and analytics that you can get from the application and can also access through the Segments API.</span></span> <span data-ttu-id="9917d-107">Segment beräknas först 24 timmar efter att de skapas och de recomputed per dygn baserat på den senaste informationen om analytics.</span><span class="sxs-lookup"><span data-stu-id="9917d-107">Segments are first computed 24 hours after they are created, and they are recomputed every 24 hours based on the latest analytics information.</span></span> <span data-ttu-id="9917d-108">När ett segment beräknas, visas ett ”Day dag tidigare” diagram varje dag.</span><span class="sxs-lookup"><span data-stu-id="9917d-108">Once a segment is calculated, it displays a "Day to day history" chart each day.</span></span>

> [!NOTE]
> <span data-ttu-id="9917d-109">Många avsnitt i den **Mobile Engagement** portal Användargränssnittet innehåller den **Visa hjälp** knappen.</span><span class="sxs-lookup"><span data-stu-id="9917d-109">Many sections of the **Mobile Engagement** portal UI contain the **SHOW HELP** button.</span></span> <span data-ttu-id="9917d-110">Tryck på knappen för att få mer information om ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="9917d-110">Press this button to get more contextual information about a section.</span></span>
> 
> 

## <a name="create-segments"></a><span data-ttu-id="9917d-111">Skapa segment</span><span class="sxs-lookup"><span data-stu-id="9917d-111">Create Segments</span></span>
<span data-ttu-id="9917d-112">Du kan skapa ett segment baserat på upp till 10 kriterier på en viss period upp till 60 dagar bakåt i tiden från avsnittet analytics.</span><span class="sxs-lookup"><span data-stu-id="9917d-112">You can create a segment based on up to 10 criteria on a specific period up to 60 days in the past from the analytics section.</span></span> <span data-ttu-id="9917d-113">Du kan till exempel skapa ett segment baserat på de personer som har visat vissa sidor eller söka efter vissa innehållstyper i appen under de senaste 10 dagarna.</span><span class="sxs-lookup"><span data-stu-id="9917d-113">For example, you can create a segment based on the people who have viewed certain pages or searched for specific content within your app within the last 10 days.</span></span> <span data-ttu-id="9917d-114">Den här informationen finns i avsnittet analytics.</span><span class="sxs-lookup"><span data-stu-id="9917d-114">This information is available in the analytics section.</span></span> <span data-ttu-id="9917d-115">Så är fallet bör använda du den för att skapa ett segment och sedan skapa ett push-meddelande för vilka den här delmängd av alla användare ska få dem att gå tillbaka till programmet.</span><span class="sxs-lookup"><span data-stu-id="9917d-115">So, you can use it to create a segment, and then set up a push notification to target this subset of users to get them to come back to the application.</span></span> 

> [!NOTE]
> <span data-ttu-id="9917d-116">När ett segment har beräknats kan inte redigeras; Det kan bara klona (kopierade) eller förstörs (borttagna).</span><span class="sxs-lookup"><span data-stu-id="9917d-116">Once a segment has been calculated, it cannot be edited; it can only be cloned (copied) or destroyed (deleted).</span></span> <span data-ttu-id="9917d-117">Ett segment kan klonas i samma program (med samma AppID) och även kan klonas i andra program (med en annan AppID).</span><span class="sxs-lookup"><span data-stu-id="9917d-117">A segment can be cloned within the same application (with the same AppID), and it can also be cloned into other applications (with a different AppID).</span></span> 

 ![segments1][35] 

## <a name="examples-segments"></a><span data-ttu-id="9917d-119">Exempel segment</span><span class="sxs-lookup"><span data-stu-id="9917d-119">Examples Segments</span></span>
 ![segments2][36]

<span data-ttu-id="9917d-121">Segment kan du segmentera slutanvändare av ditt program.</span><span class="sxs-lookup"><span data-stu-id="9917d-121">Segments allow you to segment the end-users of your application.</span></span>
<span data-ttu-id="9917d-122">Segmentera dina användare är en viktig marknadsföringsstrategi.</span><span class="sxs-lookup"><span data-stu-id="9917d-122">Segmenting your users is an important marketing strategy.</span></span> <span data-ttu-id="9917d-123">Azure Mobile Engagement kan du hämta historiska data och skapa anpassade segment.</span><span class="sxs-lookup"><span data-stu-id="9917d-123">Azure Mobile Engagement allows you to get historic data and create custom segments.</span></span> <span data-ttu-id="9917d-124">Detta kraftfulla verktyg kan du vill veta mer om kundernas upplevelse i ditt program.</span><span class="sxs-lookup"><span data-stu-id="9917d-124">This powerful tool enables you to learn about your customers’ experience in your application.</span></span> <span data-ttu-id="9917d-125">Du kan enkelt analysera segmenten och använda segment som push-mål.</span><span class="sxs-lookup"><span data-stu-id="9917d-125">You can easily analyze your segments and use your segments as push targets.</span></span>
<span data-ttu-id="9917d-126">Ett vanligt användningsfall är att du vill skicka ett push ett meddelande om att uppmana slutanvändarna att bedöma ditt program i arkivet.</span><span class="sxs-lookup"><span data-stu-id="9917d-126">A common use-case is that you want to send a push a notification to encourage your end-users to rate your application in the store.</span></span> <span data-ttu-id="9917d-127">Du kan skapa ett segment som anger bara användare som har använt ditt program varje dag för den senaste månaden och har haft en bra användarmiljö i stället för att skicka ett meddelande till alla dina slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="9917d-127">Rather than sending a notification to all your end-users, you can create a segment that would specify only users that have used your application every day for the last month and have had a great user experience.</span></span> <span data-ttu-id="9917d-128">När du skickar färre, avgränsade push-meddelanden får du en bättre avkastning på investeringen.</span><span class="sxs-lookup"><span data-stu-id="9917d-128">When you send fewer, highly targeted push notifications, you get a better ROI.</span></span>

 ![segments3][37]

### <a name="segments-you-can-create-based-on-the-major-azure-mobile-engagement-elements"></a><span data-ttu-id="9917d-130">Du kan skapa segment baserat på viktiga Azure Mobile Engagement-element:</span><span class="sxs-lookup"><span data-stu-id="9917d-130">Segments you can create based on the major Azure Mobile Engagement elements:</span></span>
* <span data-ttu-id="9917d-131">Händelse: skapa ett segment som är riktad mot en specifika händelser för det program som har hänt fler än två gånger per vecka.</span><span class="sxs-lookup"><span data-stu-id="9917d-131">Event: create a segment that targets one specific event of the application that happened more than twice a week.</span></span> 
* <span data-ttu-id="9917d-132">Sessionen: skapa ett segment med användare som har använt programmet mer än 5 gånger föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="9917d-132">Session: create a segment of users that have used the application more than 5 times last week.</span></span>
* <span data-ttu-id="9917d-133">Aktiviteten: skapa ett segment med användare som har använt en sida eller innehåll mer eller mindre än 10 tid senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="9917d-133">Activity: create a segment of users that have used one page or content more or less than 10 time last month.</span></span>
* <span data-ttu-id="9917d-134">Jobbet: skapa ett segment med användare som har slutfört ett jobb som är mer än två gånger om dagen.</span><span class="sxs-lookup"><span data-stu-id="9917d-134">Job: create a segment of users that have completed a job more than twice a day.</span></span>
* <span data-ttu-id="9917d-135">Krasch: skapa ett segment för alla användare som har fått en krasch fler än 10 gånger föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="9917d-135">Crash: create a segment of all the users that have had a crash more than 10 times last week.</span></span> <span data-ttu-id="9917d-136">(Du kan push segmentet med en apology eller även om kuponger!)</span><span class="sxs-lookup"><span data-stu-id="9917d-136">(You could push this segment with an apology or even a coupon!)</span></span>
* <span data-ttu-id="9917d-137">Fel: skapa ett segment med alla användare som har ett fel uppstått mer än 100 gånger senaste tre dagarna.</span><span class="sxs-lookup"><span data-stu-id="9917d-137">Error: create a segment of all the users that have had an error more than 100 times last 3 days.</span></span>
* <span data-ttu-id="9917d-138">App-Info: skapa ett segment som riktar sig till en anpassad App-Info som inträffade under de senaste 25 dagarna.</span><span class="sxs-lookup"><span data-stu-id="9917d-138">App Info: create a segment that targets a custom App Info that happened during the last 25 days.</span></span>
  
  ![segments4][38]

<span data-ttu-id="9917d-140">Om du vill använda Segment optimalt, måste du har gjort en anpassad integrering av SDK i ditt program med en märkning plan för ”App-Info”-taggar.</span><span class="sxs-lookup"><span data-stu-id="9917d-140">To use Segment in an optimal way, you must have done a customized integration of the SDK in your application with a tagging plan of "App Info" tags.</span></span>
<span data-ttu-id="9917d-141">Gå sedan till sidan för gränssnittet, Markera programmet som du vill använda och klicka på avsnittet ”segment”.</span><span class="sxs-lookup"><span data-stu-id="9917d-141">Then, go to the home page of the interface, select the application you want and click on the "Segments" section.</span></span>

1. <span data-ttu-id="9917d-142">Välj avsnittet ”segment”.</span><span class="sxs-lookup"><span data-stu-id="9917d-142">Select the "Segments" section.</span></span>
2. <span data-ttu-id="9917d-143">Klicka på ”nytt segment” för att skapa ett nytt segment.</span><span class="sxs-lookup"><span data-stu-id="9917d-143">Click on the "New segment" button to create a new segment.</span></span>

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a><span data-ttu-id="9917d-144">Den verkliga livslängd exempel: Skapa ett enkelt segment baserat på ”sessionsinformation”</span><span class="sxs-lookup"><span data-stu-id="9917d-144">Real Life Example: Create a simple segment based on "Session" information</span></span>
<span data-ttu-id="9917d-145">Skapa ett segment av slutanvändare som har använt appen minst 50 gånger under den senaste veckan.</span><span class="sxs-lookup"><span data-stu-id="9917d-145">Create a segment of all the end-users that have used your app at least 50 times in the last week.</span></span> <span data-ttu-id="9917d-146">Hitta de slutanvändare som har använt minst 30 sekunder i din app per session därifrån.</span><span class="sxs-lookup"><span data-stu-id="9917d-146">From there, find only the end-users that have spent at least 30 seconds in your app per session.</span></span> <span data-ttu-id="9917d-147">Det här alternativet visas alla slutanvändare som har en positiv upplevelse i din app.</span><span class="sxs-lookup"><span data-stu-id="9917d-147">This will show all the end-users who have a positive experience in your app.</span></span> <span data-ttu-id="9917d-148">Segmentet som har skapats kan sedan användas för att skicka ett meddelande till dessa slutanvändarna kan be dem att betygsätta appen i store.</span><span class="sxs-lookup"><span data-stu-id="9917d-148">Then, the segment created could be used to push a notification to these end-users to ask them to rate your app in the store.</span></span>

 ![segments5][39]

1. <span data-ttu-id="9917d-150">Namnge ditt segment för att snabbt hitta dem i listan ”Segment”.</span><span class="sxs-lookup"><span data-stu-id="9917d-150">Give your segment a name in order to find it quickly in the "Segment" list.</span></span>
2. <span data-ttu-id="9917d-151">Klicka på knappen ”Skapa”.</span><span class="sxs-lookup"><span data-stu-id="9917d-151">Click on the "Create" button.</span></span>
   
   ![segments6][40]

<span data-ttu-id="9917d-153">Markera Session.</span><span class="sxs-lookup"><span data-stu-id="9917d-153">Select Session.</span></span>

 ![segments7][41]

1. <span data-ttu-id="9917d-155">Välj perioden för ”senaste veckan”.</span><span class="sxs-lookup"><span data-stu-id="9917d-155">Select the period of "Last week".</span></span>
2. <span data-ttu-id="9917d-156">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="9917d-156">Click next.</span></span>
   
   ![segments8][42]
3. <span data-ttu-id="9917d-158">Väljer du den Operator som är relevant i listan: =; ≥ ≤.</span><span class="sxs-lookup"><span data-stu-id="9917d-158">Select the Operator that is relevant among the list: =; ≥, ≤.</span></span>
4. <span data-ttu-id="9917d-159">Ange det antal som du vill.</span><span class="sxs-lookup"><span data-stu-id="9917d-159">Enter the Count you want.</span></span>
5. <span data-ttu-id="9917d-160">Välj den förekomsten som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="9917d-160">Select the Occurrence you want.</span></span> 
6. <span data-ttu-id="9917d-161">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="9917d-161">Click next.</span></span>
   <span data-ttu-id="9917d-162">Det här exemplet anges så som under den senaste veckan matchar användare som har gjort minst 50 sessioner.</span><span class="sxs-lookup"><span data-stu-id="9917d-162">This example is set so that over the last week, match users that have made at least 50 sessions.</span></span>
   
   ![segments9][43]

<span data-ttu-id="9917d-164">Du kan välja längden sessioner som villkor för Session-segmentering.</span><span class="sxs-lookup"><span data-stu-id="9917d-164">For the Session segmentation, you can choose the length per session as a criteria.</span></span>

1. <span data-ttu-id="9917d-165">Välj operatorn i listan.</span><span class="sxs-lookup"><span data-stu-id="9917d-165">Select the Operator from the list.</span></span>
2. <span data-ttu-id="9917d-166">Ange längden per session.</span><span class="sxs-lookup"><span data-stu-id="9917d-166">Provide the Length per session.</span></span>
3. <span data-ttu-id="9917d-167">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="9917d-167">Click Next.</span></span>
   <span data-ttu-id="9917d-168">I det här exemplet står det att över alla sessioner som har varit segmenterade i avsnittet förekomsten, väljer du de användare som har använt mer än 30 sekunder per session.</span><span class="sxs-lookup"><span data-stu-id="9917d-168">In this example, it says that over all the sessions that have been segmented on the occurrence section, select only the users that have spent more than 30 seconds per session.</span></span>
   
   ![segments10][44]

<span data-ttu-id="9917d-170">Namnge ditt kriterium för att hämta i fullständig tratten och klicka på Slutför.</span><span class="sxs-lookup"><span data-stu-id="9917d-170">Name your criterion in order to retrieve it in the complete funnel, and click Finish.</span></span>

 ![segments11][45]

<span data-ttu-id="9917d-172">När du har konfigurerat dina villkor, visas den i segmentet tratten.</span><span class="sxs-lookup"><span data-stu-id="9917d-172">When you have finished setting up your criterion, it will appear in the segment funnel.</span></span>
<span data-ttu-id="9917d-173">Eftersom ett segment är baserat på analysdata, beräknas segment en gång per dag.</span><span class="sxs-lookup"><span data-stu-id="9917d-173">Because a segment is based on analytics data, segments are computed once per day.</span></span>
<span data-ttu-id="9917d-174">I det här exemplet 47,7% av totalt slutanvändarna matchade kriteriet.</span><span class="sxs-lookup"><span data-stu-id="9917d-174">In this example, 47,7% of the total end-users matched the criterion.</span></span> <span data-ttu-id="9917d-175">De ska vara de användare som har haft en bra upplevelse och kommer sannolikt att tillhandahålla en högre klassificering om du överför ett meddelande som ber dem att betygsätta appen i butiken.</span><span class="sxs-lookup"><span data-stu-id="9917d-175">They should be the users who have had a good experience and will be likely to provide a higher rating if you push them a notification asking them to rate the app in the store.</span></span>

## <a name="see-also"></a><span data-ttu-id="9917d-176">Se även</span><span class="sxs-lookup"><span data-stu-id="9917d-176">See also</span></span>
* <span data-ttu-id="9917d-177">[Begrepp][Link 6]</span><span class="sxs-lookup"><span data-stu-id="9917d-177">[Concepts][Link 6]</span></span>
* <span data-ttu-id="9917d-178">[Felsöka Guide Service][Link 24]</span><span class="sxs-lookup"><span data-stu-id="9917d-178">[Troubleshooting Guide Service][Link 24]</span></span>

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
[Link 27]: ../mobile-engagement-how-tos-first-push.md
[Link 28]: ../mobile-engagement-how-tos-test-campaign.md
[Link 29]: ../mobile-engagement-how-tos-personalize-push.md
[Link 30]: ../mobile-engagement-how-tos-differentiate-push.md
[Link 31]: ../mobile-engagement-how-tos-schedule-campaign.md
[Link 32]: ../mobile-engagement-how-tos-text-view.md
[Link 33]: ../mobile-engagement-how-tos-web-view.md

