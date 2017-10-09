---
title: "aaaAzure användargränssnitt för Mobile Engagement - segment"
description: "Lär dig hur toocreate och hantera segment av användare tooidentify användningsmönster med Azure Mobile Engagement"
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
ms.openlocfilehash: bb214c45d05ebfbf243978658a7e331d4a7c6e0e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-and-manage-segments-of-users-tooidentify-usage-patterns"></a><span data-ttu-id="41d89-103">Hur toocreate och hantera segment av användare tooidentify användningsmönster</span><span class="sxs-lookup"><span data-stu-id="41d89-103">How toocreate and manage segments of users tooidentify usage patterns</span></span>
<span data-ttu-id="41d89-104">Den här artikeln beskriver hello **segment** för hello **Mobile Engagement** portal.</span><span class="sxs-lookup"><span data-stu-id="41d89-104">This article describes hello **SEGMENTS** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="41d89-105">Du använder hello **Mobile Engagement** portal toomonitor och hantera dina mobila appar.</span><span class="sxs-lookup"><span data-stu-id="41d89-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span>

<span data-ttu-id="41d89-106">hello segment avsnittet av hello Användargränssnittet kan du toowork på segmentera dina användare baserat på hello olika beteenden och analyser som du kan hämta från hello programmet och kan också komma åt via hello segment API.</span><span class="sxs-lookup"><span data-stu-id="41d89-106">hello Segments section of hello UI allows you toowork on segmenting your users based on hello different behavior and analytics that you can get from hello application and can also access through hello Segments API.</span></span> <span data-ttu-id="41d89-107">Segment beräknas först 24 timmar efter att de skapas och de recomputed per dygn baserat på hello senaste analytics informationen.</span><span class="sxs-lookup"><span data-stu-id="41d89-107">Segments are first computed 24 hours after they are created, and they are recomputed every 24 hours based on hello latest analytics information.</span></span> <span data-ttu-id="41d89-108">När ett segment beräknas visar ett diagram med ”dag tooday historik” varje dag.</span><span class="sxs-lookup"><span data-stu-id="41d89-108">Once a segment is calculated, it displays a "Day tooday history" chart each day.</span></span>

> [!NOTE]
> <span data-ttu-id="41d89-109">Många avsnitt av hello **Mobile Engagement** portalens användargränssnitt innehåller hello **Visa hjälp** knappen.</span><span class="sxs-lookup"><span data-stu-id="41d89-109">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="41d89-110">Tryck på den här knappen tooget mer detaljerad information om ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="41d89-110">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="create-segments"></a><span data-ttu-id="41d89-111">Skapa segment</span><span class="sxs-lookup"><span data-stu-id="41d89-111">Create Segments</span></span>
<span data-ttu-id="41d89-112">Du kan skapa ett segment baserat på too10 kriterier i en viss period in too60 dagar i hello tidigare från hello analytics avsnitt.</span><span class="sxs-lookup"><span data-stu-id="41d89-112">You can create a segment based on up too10 criteria on a specific period up too60 days in hello past from hello analytics section.</span></span> <span data-ttu-id="41d89-113">Du kan till exempel skapa ett segment baserat på hello personer som har visat vissa sidor eller söka efter vissa innehållstyper i din app i hello senaste 10 dagarna.</span><span class="sxs-lookup"><span data-stu-id="41d89-113">For example, you can create a segment based on hello people who have viewed certain pages or searched for specific content within your app within hello last 10 days.</span></span> <span data-ttu-id="41d89-114">Den här informationen finns i avsnittet för hello analytics.</span><span class="sxs-lookup"><span data-stu-id="41d89-114">This information is available in hello analytics section.</span></span> <span data-ttu-id="41d89-115">Så du kan använda den toocreate ett segment och sedan konfigurera tootarget en push-meddelanden med den här delmängden av användare tooget dem toocome tillbaka toohello program.</span><span class="sxs-lookup"><span data-stu-id="41d89-115">So, you can use it toocreate a segment, and then set up a push notification tootarget this subset of users tooget them toocome back toohello application.</span></span> 

> [!NOTE]
> <span data-ttu-id="41d89-116">När ett segment har beräknats kan inte redigeras; Det kan bara klona (kopierade) eller förstörs (borttagna).</span><span class="sxs-lookup"><span data-stu-id="41d89-116">Once a segment has been calculated, it cannot be edited; it can only be cloned (copied) or destroyed (deleted).</span></span> <span data-ttu-id="41d89-117">Ett segment kan klonas i hello samma program (med hello samma AppID), och även kan klonas i andra program (med en annan AppID).</span><span class="sxs-lookup"><span data-stu-id="41d89-117">A segment can be cloned within hello same application (with hello same AppID), and it can also be cloned into other applications (with a different AppID).</span></span> 

 ![segments1][35] 

## <a name="examples-segments"></a><span data-ttu-id="41d89-119">Exempel segment</span><span class="sxs-lookup"><span data-stu-id="41d89-119">Examples Segments</span></span>
 ![segments2][36]

<span data-ttu-id="41d89-121">Segment kan du toosegment hello slutanvändare av ditt program.</span><span class="sxs-lookup"><span data-stu-id="41d89-121">Segments allow you toosegment hello end-users of your application.</span></span>
<span data-ttu-id="41d89-122">Segmentera dina användare är en viktig marknadsföringsstrategi.</span><span class="sxs-lookup"><span data-stu-id="41d89-122">Segmenting your users is an important marketing strategy.</span></span> <span data-ttu-id="41d89-123">Azure Mobile Engagement kan du tooget historiska data och skapa anpassade segment.</span><span class="sxs-lookup"><span data-stu-id="41d89-123">Azure Mobile Engagement allows you tooget historic data and create custom segments.</span></span> <span data-ttu-id="41d89-124">Detta kraftfulla verktyg kan du toolearn om kundernas upplevelse i ditt program.</span><span class="sxs-lookup"><span data-stu-id="41d89-124">This powerful tool enables you toolearn about your customers’ experience in your application.</span></span> <span data-ttu-id="41d89-125">Du kan enkelt analysera segmenten och använda segment som push-mål.</span><span class="sxs-lookup"><span data-stu-id="41d89-125">You can easily analyze your segments and use your segments as push targets.</span></span>
<span data-ttu-id="41d89-126">Är ett vanligt användningsfall som du vill toosend push tooencourage ett meddelande som dina slutanvändare toorate ditt program i hello store.</span><span class="sxs-lookup"><span data-stu-id="41d89-126">A common use-case is that you want toosend a push a notification tooencourage your end-users toorate your application in hello store.</span></span> <span data-ttu-id="41d89-127">I stället för att skicka ett meddelande tooall slutanvändarna kan skapa du ett segment som anger enbart användare som har använt ditt program varje dag för hello förra månaden och har haft en bra användarmiljö.</span><span class="sxs-lookup"><span data-stu-id="41d89-127">Rather than sending a notification tooall your end-users, you can create a segment that would specify only users that have used your application every day for hello last month and have had a great user experience.</span></span> <span data-ttu-id="41d89-128">När du skickar färre, avgränsade push-meddelanden får du en bättre avkastning på investeringen.</span><span class="sxs-lookup"><span data-stu-id="41d89-128">When you send fewer, highly targeted push notifications, you get a better ROI.</span></span>

 ![segments3][37]

### <a name="segments-you-can-create-based-on-hello-major-azure-mobile-engagement-elements"></a><span data-ttu-id="41d89-130">Du kan skapa segment baserat på hello viktiga Azure Mobile Engagement-element:</span><span class="sxs-lookup"><span data-stu-id="41d89-130">Segments you can create based on hello major Azure Mobile Engagement elements:</span></span>
* <span data-ttu-id="41d89-131">Händelse: skapa ett segment som är riktad mot en specifika händelser för hello-program som har hänt fler än två gånger per vecka.</span><span class="sxs-lookup"><span data-stu-id="41d89-131">Event: create a segment that targets one specific event of hello application that happened more than twice a week.</span></span> 
* <span data-ttu-id="41d89-132">Sessionen: skapa ett segment med användare som har använt hello programmet mer än 5 gånger föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="41d89-132">Session: create a segment of users that have used hello application more than 5 times last week.</span></span>
* <span data-ttu-id="41d89-133">Aktiviteten: skapa ett segment med användare som har använt en sida eller innehåll mer eller mindre än 10 tid senaste månaden.</span><span class="sxs-lookup"><span data-stu-id="41d89-133">Activity: create a segment of users that have used one page or content more or less than 10 time last month.</span></span>
* <span data-ttu-id="41d89-134">Jobbet: skapa ett segment med användare som har slutfört ett jobb som är mer än två gånger om dagen.</span><span class="sxs-lookup"><span data-stu-id="41d89-134">Job: create a segment of users that have completed a job more than twice a day.</span></span>
* <span data-ttu-id="41d89-135">Krasch: skapa ett segment med alla hello-användare som har fått en krasch fler än 10 gånger föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="41d89-135">Crash: create a segment of all hello users that have had a crash more than 10 times last week.</span></span> <span data-ttu-id="41d89-136">(Du kan push segmentet med en apology eller även om kuponger!)</span><span class="sxs-lookup"><span data-stu-id="41d89-136">(You could push this segment with an apology or even a coupon!)</span></span>
* <span data-ttu-id="41d89-137">Fel: skapa ett segment med alla hello-användare som har ett fel uppstått mer än 100 gånger senaste tre dagarna.</span><span class="sxs-lookup"><span data-stu-id="41d89-137">Error: create a segment of all hello users that have had an error more than 100 times last 3 days.</span></span>
* <span data-ttu-id="41d89-138">App-Info: skapa ett segment som riktar sig till en anpassad App-Info som inträffade under hello senaste 25 dagarna.</span><span class="sxs-lookup"><span data-stu-id="41d89-138">App Info: create a segment that targets a custom App Info that happened during hello last 25 days.</span></span>
  
  ![segments4][38]

<span data-ttu-id="41d89-140">toouse Segment optimalt, du måste har gjort en anpassad integrering av hello SDK i ditt program med en märkning plan för ”App-Info”-taggar.</span><span class="sxs-lookup"><span data-stu-id="41d89-140">toouse Segment in an optimal way, you must have done a customized integration of hello SDK in your application with a tagging plan of "App Info" tags.</span></span>
<span data-ttu-id="41d89-141">Gå sedan toohello startsidan för hello-gränssnittet, Välj hello program du vill använda och klicka på hello ”segment” avsnittet.</span><span class="sxs-lookup"><span data-stu-id="41d89-141">Then, go toohello home page of hello interface, select hello application you want and click on hello "Segments" section.</span></span>

1. <span data-ttu-id="41d89-142">Markera hello ”segment”.</span><span class="sxs-lookup"><span data-stu-id="41d89-142">Select hello "Segments" section.</span></span>
2. <span data-ttu-id="41d89-143">Klicka på hello ”nytt segment” knappen toocreate ett nytt segment.</span><span class="sxs-lookup"><span data-stu-id="41d89-143">Click on hello "New segment" button toocreate a new segment.</span></span>

## <a name="real-life-example-create-a-simple-segment-based-on-session-information"></a><span data-ttu-id="41d89-144">Den verkliga livslängd exempel: Skapa ett enkelt segment baserat på ”sessionsinformation”</span><span class="sxs-lookup"><span data-stu-id="41d89-144">Real Life Example: Create a simple segment based on "Session" information</span></span>
<span data-ttu-id="41d89-145">Skapa ett segment av alla hello slutanvändare som har använt appen minst 50 gånger i hello föregående vecka.</span><span class="sxs-lookup"><span data-stu-id="41d89-145">Create a segment of all hello end-users that have used your app at least 50 times in hello last week.</span></span> <span data-ttu-id="41d89-146">Hitta endast hello slutanvändare som har använt minst 30 sekunder i din app per session därifrån.</span><span class="sxs-lookup"><span data-stu-id="41d89-146">From there, find only hello end-users that have spent at least 30 seconds in your app per session.</span></span> <span data-ttu-id="41d89-147">Det här alternativet visas alla hello slutanvändare som har en positiv upplevelse i din app.</span><span class="sxs-lookup"><span data-stu-id="41d89-147">This will show all hello end-users who have a positive experience in your app.</span></span> <span data-ttu-id="41d89-148">Sedan hello segment skapas kunde använda toopush ett meddelande toothese slutanvändare tooask dem toorate din app i hello lagra.</span><span class="sxs-lookup"><span data-stu-id="41d89-148">Then, hello segment created could be used toopush a notification toothese end-users tooask them toorate your app in hello store.</span></span>

 ![segments5][39]

1. <span data-ttu-id="41d89-150">Namnge ditt segment i ordning toofind det snabbt i hello ”Segment”-listan.</span><span class="sxs-lookup"><span data-stu-id="41d89-150">Give your segment a name in order toofind it quickly in hello "Segment" list.</span></span>
2. <span data-ttu-id="41d89-151">Klicka på hello ”skapa”.</span><span class="sxs-lookup"><span data-stu-id="41d89-151">Click on hello "Create" button.</span></span>
   
   ![segments6][40]

<span data-ttu-id="41d89-153">Markera Session.</span><span class="sxs-lookup"><span data-stu-id="41d89-153">Select Session.</span></span>

 ![segments7][41]

1. <span data-ttu-id="41d89-155">Välj hello period för ”senaste veckan”.</span><span class="sxs-lookup"><span data-stu-id="41d89-155">Select hello period of "Last week".</span></span>
2. <span data-ttu-id="41d89-156">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="41d89-156">Click next.</span></span>
   
   ![segments8][42]
3. <span data-ttu-id="41d89-158">Välj hello Operator som är relevant hello listan: =; ≥ ≤.</span><span class="sxs-lookup"><span data-stu-id="41d89-158">Select hello Operator that is relevant among hello list: =; ≥, ≤.</span></span>
4. <span data-ttu-id="41d89-159">Ange hello antal som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="41d89-159">Enter hello Count you want.</span></span>
5. <span data-ttu-id="41d89-160">Välj hello förekomsten som du vill använda.</span><span class="sxs-lookup"><span data-stu-id="41d89-160">Select hello Occurrence you want.</span></span> 
6. <span data-ttu-id="41d89-161">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="41d89-161">Click next.</span></span>
   <span data-ttu-id="41d89-162">Det här exemplet är inställd så att över hello förra veckan matchar användare som har gjort minst 50 sessioner.</span><span class="sxs-lookup"><span data-stu-id="41d89-162">This example is set so that over hello last week, match users that have made at least 50 sessions.</span></span>
   
   ![segments9][43]

<span data-ttu-id="41d89-164">Hello Session segmentering du hello längd sessioner som ett villkor.</span><span class="sxs-lookup"><span data-stu-id="41d89-164">For hello Session segmentation, you can choose hello length per session as a criteria.</span></span>

1. <span data-ttu-id="41d89-165">Välj hello operatorn hello-listan.</span><span class="sxs-lookup"><span data-stu-id="41d89-165">Select hello Operator from hello list.</span></span>
2. <span data-ttu-id="41d89-166">Ange hello längd per session.</span><span class="sxs-lookup"><span data-stu-id="41d89-166">Provide hello Length per session.</span></span>
3. <span data-ttu-id="41d89-167">Klicka på Nästa.</span><span class="sxs-lookup"><span data-stu-id="41d89-167">Click Next.</span></span>
   <span data-ttu-id="41d89-168">I det här exemplet står det att över alla hello sessioner som har varit segmenterade på hello förekomsten avsnittet väljer endast hello-användare som har använt mer än 30 sekunder per session.</span><span class="sxs-lookup"><span data-stu-id="41d89-168">In this example, it says that over all hello sessions that have been segmented on hello occurrence section, select only hello users that have spent more than 30 seconds per session.</span></span>
   
   ![segments10][44]

<span data-ttu-id="41d89-170">Namnge ditt kriterium i ordning tooretrieve i hello slutföra tratt och klicka på Slutför.</span><span class="sxs-lookup"><span data-stu-id="41d89-170">Name your criterion in order tooretrieve it in hello complete funnel, and click Finish.</span></span>

 ![segments11][45]

<span data-ttu-id="41d89-172">När du har konfigurerat dina villkor, visas den i hello segment tratten.</span><span class="sxs-lookup"><span data-stu-id="41d89-172">When you have finished setting up your criterion, it will appear in hello segment funnel.</span></span>
<span data-ttu-id="41d89-173">Eftersom ett segment är baserat på analysdata, beräknas segment en gång per dag.</span><span class="sxs-lookup"><span data-stu-id="41d89-173">Because a segment is based on analytics data, segments are computed once per day.</span></span>
<span data-ttu-id="41d89-174">I det här exemplet 47,7% av totalt hello-slutanvändare matchade hello kriterium.</span><span class="sxs-lookup"><span data-stu-id="41d89-174">In this example, 47,7% of hello total end-users matched hello criterion.</span></span> <span data-ttu-id="41d89-175">De bör vara hello-användare som har haft en bra upplevelse och kommer troligen tooprovide en högre klassificeringen om du överför ett meddelande frågar dem toorate hello app i hello store.</span><span class="sxs-lookup"><span data-stu-id="41d89-175">They should be hello users who have had a good experience and will be likely tooprovide a higher rating if you push them a notification asking them toorate hello app in hello store.</span></span>

## <a name="see-also"></a><span data-ttu-id="41d89-176">Se även</span><span class="sxs-lookup"><span data-stu-id="41d89-176">See also</span></span>
* <span data-ttu-id="41d89-177">[Begrepp][Link 6]</span><span class="sxs-lookup"><span data-stu-id="41d89-177">[Concepts][Link 6]</span></span>
* <span data-ttu-id="41d89-178">[Felsöka Guide Service][Link 24]</span><span class="sxs-lookup"><span data-stu-id="41d89-178">[Troubleshooting Guide Service][Link 24]</span></span>

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

