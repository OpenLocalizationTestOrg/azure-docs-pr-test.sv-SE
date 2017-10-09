---
title: "aaaAzure användargränssnitt för Mobile Engagement - Övervakare"
description: "Lär dig hur toomonitor realtidsdata om ditt program med Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: b91ad89a-b89d-4377-abb0-cc2d16a2836d
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 3a581e4166bc88e6ee7aa784d4047c94533685b5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toomonitor-real-time-data-about-your-application"></a><span data-ttu-id="6fce5-103">Hur toomonitor realtidsdata för appen</span><span class="sxs-lookup"><span data-stu-id="6fce5-103">How toomonitor real-time data about your application</span></span>
<span data-ttu-id="6fce5-104">Den här artikeln beskriver hello **ÖVERVAKAREN** för hello **Mobile Engagement** portal.</span><span class="sxs-lookup"><span data-stu-id="6fce5-104">This article describes hello **MONITOR** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="6fce5-105">Du använder hello **Mobile Engagement** portal toomonitor och hantera dina mobila appar.</span><span class="sxs-lookup"><span data-stu-id="6fce5-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span> <span data-ttu-id="6fce5-106">Observera att toostart med hello-portalen måste du först toocreate en **Azure Mobile Engagement** konto.</span><span class="sxs-lookup"><span data-stu-id="6fce5-106">Note that toostart using hello portal you first need toocreate an **Azure Mobile Engagement** account.</span></span> 

<span data-ttu-id="6fce5-107">hello övervakaren avsnitt i hello Användargränssnittet innehåller realtidsanalys information och tillåter tooset meddelanden när tröskelvärden uppnås för de flesta hello samma information som är tillgängliga i hello tidigare [ANALYTICS](mobile-engagement-user-interface-analytics.md) avsnitt i hello Användargränssnittet.</span><span class="sxs-lookup"><span data-stu-id="6fce5-107">hello Monitor section of hello UI provides real-time analytics information and allows you tooset alerts when thresholds are reached for most of hello same information that is available historically in hello [ANALYTICS](mobile-engagement-user-interface-analytics.md) section of hello UI.</span></span> <span data-ttu-id="6fce5-108">Se hello **ordlista** avsnitt i hello [begrepp](http://go.microsoft.com/fwlink/?LinkId=525555) avsnittet definitioner av uttryck och förkortningar i analyser och övervakning (till exempel hello följande: aktiva användare, nya användare, behålls användare, Session, Användaren sökväg diagram, mappa användare, spårning URL: er, trender, aktivitet, händelse, jobb, fel, Extra information, krascher och App-info).</span><span class="sxs-lookup"><span data-stu-id="6fce5-108">See hello **Glossary** section in hello [Concepts](http://go.microsoft.com/fwlink/?LinkId=525555) topic for definitions of terms and abbreviations in Analytics and Monitoring (such as hello following: Active User, New user, Retained User, Session, User Path Graph, Users Map, Tracking URLs, Trends, Activity, Event, Job, Error, Extra Info, Crash, and App-info).</span></span>

> [!NOTE]
> <span data-ttu-id="6fce5-109">Många avsnitt av hello **Mobile Engagement** portalens användargränssnitt innehåller hello **Visa hjälp** knappen.</span><span class="sxs-lookup"><span data-stu-id="6fce5-109">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="6fce5-110">Tryck på den här knappen tooget mer detaljerad information om ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6fce5-110">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="monitor---sessions-jobs-events-errors-and-crashes"></a><span data-ttu-id="6fce5-111">Övervakaren - sessioner, jobb, händelser, fel och krascher</span><span class="sxs-lookup"><span data-stu-id="6fce5-111">Monitor - Sessions, Jobs, Events, Errors, and Crashes</span></span>
<span data-ttu-id="6fce5-112">Du kan se hur många användare är för närvarande i sessionen och specifika skärmar eller göra vissa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="6fce5-112">You can see how many users are currently in session and on specific screens or doing specific actions.</span></span> <span data-ttu-id="6fce5-113">Du kan visa användaraktivitet dividerat med sessioner, jobb, händelser, fel och krascher.</span><span class="sxs-lookup"><span data-stu-id="6fce5-113">You can view user activity divided by Sessions, Jobs, Events, Errors, and Crashes.</span></span> <span data-ttu-id="6fce5-114">Du kan se hello aktuella informationen och visa hello information från hello timma, dag eller vecka.</span><span class="sxs-lookup"><span data-stu-id="6fce5-114">You can see hello current information and show hello information from hello last hour, day, or week.</span></span> <span data-ttu-id="6fce5-115">Du kan se all information som hello i varje kategori eller sortera efter specifika hello Session, jobb, händelse, fel och krascher.</span><span class="sxs-lookup"><span data-stu-id="6fce5-115">You can see all of hello information in each category or sort by hello specific Session, Job, Event, Error, and Crash.</span></span>  <span data-ttu-id="6fce5-116">Live övervakning är användbara toouse under händelser, t.ex en Push-kampanj toosee om det finns en uptick i praktiken rätt när du skickar Push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="6fce5-116">Live monitoring is helpful toouse during events such as a Push campaign toosee if there is an uptick in action right after you send your Push notification.</span></span>

![Monitor1][14]  

## <a name="troubleshooting-with-monitor---events---details"></a><span data-ttu-id="6fce5-118">Felsöka med Monitor - händelser - information</span><span class="sxs-lookup"><span data-stu-id="6fce5-118">Troubleshooting with Monitor - Events - Details</span></span>
<span data-ttu-id="6fce5-119">Generera en händelse i ditt program från enheten test och hitta i övervakaren - händelser - information är en av hello enklaste sätt toofind enheten ID för testenhet och tooconfirm som Azure Mobile Engagement-integreringen av Analytics, övervakning, och Segment fungerar från ditt program.</span><span class="sxs-lookup"><span data-stu-id="6fce5-119">Generating an event in your application from your test device and finding it in Monitor - Events - Details is one of hello easiest ways toofind your Device ID for your test device and tooconfirm that Azure Mobile Engagement integration of Analytics, Monitoring, and Segments is working from your application.</span></span> <span data-ttu-id="6fce5-120">När du har hello enhets-ID för enheten test kan lägga du tooyour testenheter i ”Mina konto--enheter”.</span><span class="sxs-lookup"><span data-stu-id="6fce5-120">Once you have hello Device ID of your test device, you can add it tooyour test devices in “My Account - Devices”.</span></span> <span data-ttu-id="6fce5-121">Om du inte kan generera en händelse, se till att Azure Mobile Engagement korrekt är integrerat i din Android/iOS/Web/Windows/Windows Phone-app med hello SDK.</span><span class="sxs-lookup"><span data-stu-id="6fce5-121">If you can't generate an event, make sure that Azure Mobile Engagement is correctly integrated in your Android/iOS/Web/Windows/Windows Phone app with hello SDK.</span></span>

<span data-ttu-id="6fce5-122">Mer information finns: [SDK-dokumentationen][Link 5]</span><span class="sxs-lookup"><span data-stu-id="6fce5-122">For more information, see:  [SDK Documentation][Link 5]</span></span>

![Monitor2][15]  

## <a name="troubleshooting-with-monitor---crashes---details"></a><span data-ttu-id="6fce5-124">Felsöka med Monitor kraschar - - information</span><span class="sxs-lookup"><span data-stu-id="6fce5-124">Troubleshooting with Monitor - Crashes - Details</span></span>
<span data-ttu-id="6fce5-125">Du kan granska information om kraschen uppges om din app från övervakaren - krascher - information toohelp avgöra varför en app kraschar.</span><span class="sxs-lookup"><span data-stu-id="6fce5-125">You can review crash information about your app from Monitor - Crashes - Details toohelp determine why your app is crashing.</span></span> <span data-ttu-id="6fce5-126">Du bör också söka efter kända problem med varje version av hello SDK i hello viktig information för varje version av hello SDK för Android/iOS/Web/Windows/Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="6fce5-126">You should also look up known issues with each version of hello SDK in hello release notes for each version of hello SDK for Android/iOS/Web/Windows/Windows Phone.</span></span>

<span data-ttu-id="6fce5-127">Mer information finns: [SDK-dokumentation – viktig information][Link 5]</span><span class="sxs-lookup"><span data-stu-id="6fce5-127">For more information, see:  [SDK Documentation - Release notes][Link 5]</span></span>

![Monitor3][16]

## <a name="monitor---alerts"></a><span data-ttu-id="6fce5-129">Övervakaren - aviseringar</span><span class="sxs-lookup"><span data-stu-id="6fce5-129">Monitor - Alerts</span></span>
<span data-ttu-id="6fce5-130">Du kan också ange villkor för aviseringar som skickas automatiskt tooyou via e-post eller snabbmeddelande.</span><span class="sxs-lookup"><span data-stu-id="6fce5-130">You can also specify conditions for Alerts that will be automatically sent tooyou via e-mail or instant message.</span></span> <span data-ttu-id="6fce5-131">(XMPP-kompatibla tjänster som Googles GTalk eller Apples iChat stöds.) Aviseringar baseras på en fördefinierad identifiering tröskel större än (>) eller mindre än (<) ett visst antal sessioner, jobb, händelser, fel eller krascher per sekund, minut eller timme.</span><span class="sxs-lookup"><span data-stu-id="6fce5-131">(Any XMPP-compliant services like Google's GTalk or Apple's iChat are supported.) Alerts are based on a pre-defined detection threshold greater than (>) or less than (<) a specific number of Sessions, Jobs, Events, Errors, or Crashes per second, minute, or hour.</span></span> <span data-ttu-id="6fce5-132">Aviseringar kan övervaka alla aktiviteter av en viss typ, eller bara övervaka en specifik aktivitet jobbet, händelse eller fel.</span><span class="sxs-lookup"><span data-stu-id="6fce5-132">Alerts can monitor all activities of a given type, or just monitor a specific Job, Event, or Error activity.</span></span> 

<span data-ttu-id="6fce5-133">Du kan även ange en minsta Identifieringstakt som är hello minsta mängd minuter som separeras två meddelanden för hello samma varning toomake säker att när aviseringen utlöses aldrig får du mer än 1 meddelanden per intervall som anges.</span><span class="sxs-lookup"><span data-stu-id="6fce5-133">You can also specify a Minimum Detection Rate, which is hello minimum amount of minutes that will separate two notifications for hello same alert toomake sure that when your alert is triggered, you will never receive more than 1 notification per interval specified.</span></span>

![Monitor4][17]

## <a name="see-also"></a><span data-ttu-id="6fce5-135">Se även</span><span class="sxs-lookup"><span data-stu-id="6fce5-135">See also</span></span>
* <span data-ttu-id="6fce5-136">[Begrepp][Link 6]</span><span class="sxs-lookup"><span data-stu-id="6fce5-136">[Concepts][Link 6]</span></span>
* <span data-ttu-id="6fce5-137">[Felsöka Guide Service][Link 24]</span><span class="sxs-lookup"><span data-stu-id="6fce5-137">[Troubleshooting Guide Service][Link 24]</span></span>

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
