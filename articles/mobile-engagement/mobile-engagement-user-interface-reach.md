---
title: "aaaAzure användargränssnitt för Mobile Engagement - Reach"
description: "Lär dig hur tooreach ut toohello användare på ditt program med push-meddelanden med Azure Mobile Engagement"
services: mobile-engagement
documentationcenter: 
author: piyushjo
manager: erikre
editor: 
ms.assetid: d96e2590-08dd-4481-a352-2c18f26a1643
ms.service: mobile-engagement
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: mobile-multiple
ms.workload: mobile
ms.date: 08/19/2016
ms.author: piyushjo
ms.openlocfilehash: 40d5162ddeccec82c2c9f5b0d72b4cb10c9ddc38
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooreach-out-toohello-users-of-your-application-with-push-notifications"></a><span data-ttu-id="72c47-103">Hur tooreach ut toohello användare av ditt program med push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="72c47-103">How tooreach out toohello users of your application with push notifications</span></span>
<span data-ttu-id="72c47-104">Den här artikeln beskriver hello **NÅ** för hello **Mobile Engagement** portal.</span><span class="sxs-lookup"><span data-stu-id="72c47-104">This article describes hello **REACH** tab of hello **Mobile Engagement** portal.</span></span> <span data-ttu-id="72c47-105">Du använder hello **Mobile Engagement** portal toomonitor och hantera dina mobila appar.</span><span class="sxs-lookup"><span data-stu-id="72c47-105">You use hello **Mobile Engagement** portal toomonitor and manage your mobile apps.</span></span> <span data-ttu-id="72c47-106">Observera att toostart med hello-portalen måste du först toocreate en **Azure Mobile Engagement** konto.</span><span class="sxs-lookup"><span data-stu-id="72c47-106">Note that toostart using hello portal you first need toocreate an **Azure Mobile Engagement** account.</span></span> <span data-ttu-id="72c47-107">Mer information finns i [skapa ett Azure Mobile Engagement-konto](mobile-engagement-create.md).</span><span class="sxs-lookup"><span data-stu-id="72c47-107">For more information, see [Create an Azure Mobile Engagement account](mobile-engagement-create.md).</span></span>

<span data-ttu-id="72c47-108">hello nå avsnitt i Användargränssnittet är hello hello Push kampanj hanteringsverktyg som du kan skapa/redigera/aktivera/Slutför/Övervakare och få statistik på Push notification-kampanjer och funktioner som också kan komma åt via hello nå API (och vissa delar av hello låg nivå Push-API).</span><span class="sxs-lookup"><span data-stu-id="72c47-108">hello Reach section of hello UI is hello Push campaign management tool where you can create/edit/activate/finish/monitor and get statistics on Push notification campaigns and features that can also be accessed via hello Reach API (and some elements of hello low level Push API).</span></span> <span data-ttu-id="72c47-109">Kom ihåg att om du använder hello API: er eller hello Användargränssnittet måste toointegrate räckvidd i programmet för varje plattform med hello SDK innan du kan använda och Azure Mobile Engagement nå kampanjer.</span><span class="sxs-lookup"><span data-stu-id="72c47-109">Remember that whether you are using hello APIs or hello UI, you will need toointegrate both Azure Mobile Engagement and Reach into your application for each platform with hello SDK before you can use Reach campaigns.</span></span>

> [!NOTE]
> <span data-ttu-id="72c47-110">Många avsnitt av hello **Mobile Engagement** portalens användargränssnitt innehåller hello **Visa hjälp** knappen.</span><span class="sxs-lookup"><span data-stu-id="72c47-110">Many sections of hello **Mobile Engagement** portal UI contain hello **SHOW HELP** button.</span></span> <span data-ttu-id="72c47-111">Tryck på den här knappen tooget mer detaljerad information om ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="72c47-111">Press this button tooget more contextual information about a section.</span></span>
> 
> 

## <a name="four-types-of-push-notifications"></a><span data-ttu-id="72c47-112">Fyra typer av Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="72c47-112">Four types of Push notifications</span></span>
1. <span data-ttu-id="72c47-113">Meddelanden - Tillåt toosend reklam meddelanden toousers som omdirigerar dem tooanother plats i din app eller toosend dem tooa webbsidan eller lagras utanför din app.</span><span class="sxs-lookup"><span data-stu-id="72c47-113">Announcements - allow you toosend advertising messages toousers that redirect them tooanother location inside your app or toosend them tooa webpage or store outside of your app.</span></span> 
2. <span data-ttu-id="72c47-114">Omröstningar - Tillåt toogather information från slutanvändarna genom att ställa frågor till dem..</span><span class="sxs-lookup"><span data-stu-id="72c47-114">Polls - allow you toogather information from end users by asking them questions.</span></span>
3. <span data-ttu-id="72c47-115">Data-push - Tillåt toosend en binär fil eller base64-datafil.</span><span class="sxs-lookup"><span data-stu-id="72c47-115">Data Pushes - allow you toosend a binary or base64 data file.</span></span> <span data-ttu-id="72c47-116">hello informationen som finns i en data-push skickas tooyour programmet toomodify användarnas aktuella upplevelse i din app.</span><span class="sxs-lookup"><span data-stu-id="72c47-116">hello information contained in a data push is sent tooyour application toomodify your users' current experience in your app.</span></span> <span data-ttu-id="72c47-117">Programmet måste toobe kan tooprocess hello data i en data-push.</span><span class="sxs-lookup"><span data-stu-id="72c47-117">Your application needs toobe able tooprocess hello data in a data push.</span></span>

## <a name="campaign-details"></a><span data-ttu-id="72c47-118">Information om kampanjer</span><span class="sxs-lookup"><span data-stu-id="72c47-118">Campaign Details</span></span>
<span data-ttu-id="72c47-119">Du kan redigera, klona, ta bort eller aktivera kampanjer som inte har aktiverats ännu hovrar över deras namn och du kan klicka på tooopen dem.</span><span class="sxs-lookup"><span data-stu-id="72c47-119">You can edit, clone, delete, or activate campaigns that have not been activated yet by hovering over their names or you can click tooopen them.</span></span> <span data-ttu-id="72c47-120">Du kan också klona kampanjer som redan har aktiverats av användaren håller muspekaren över deras namn eller klicka tooopen dem.</span><span class="sxs-lookup"><span data-stu-id="72c47-120">You can clone campaigns that have already been activated by hovering over their names or you can click tooopen them.</span></span> <span data-ttu-id="72c47-121">Du kan inte ändra en kampanj när den väl har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="72c47-121">However, you can't change a campaign once it has been activated.</span></span>

![Reach1][18]

## <a name="reach-feedback"></a><span data-ttu-id="72c47-123">Nå Feedback</span><span class="sxs-lookup"><span data-stu-id="72c47-123">Reach Feedback</span></span>
<span data-ttu-id="72c47-124">Klicka på **statistik** toosee hello information om en Reach-kampanj.</span><span class="sxs-lookup"><span data-stu-id="72c47-124">Click on **Statistics** toosee hello details of a Reach campaign.</span></span> <span data-ttu-id="72c47-125">Hej **enkel** vyn innehåller en bild i hello form av stapeldiagram kolumn om vad som hänt efter en kampanj har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="72c47-125">hello **Simple** view provides a visual representation in hello form of a column bar graph about what happened after a campaign was activated.</span></span> <span data-ttu-id="72c47-126">Hej **Avancerat** vyn innehåller mer detaljerad information om hello push-kampanj.</span><span class="sxs-lookup"><span data-stu-id="72c47-126">hello **Advanced** view provides more granular details about hello push campaign.</span></span> <span data-ttu-id="72c47-127">Dessa uppgifter är inte tillgängligt om du skickar ett testkampanj d.v.s. en push-skickade tooa testenhet.</span><span class="sxs-lookup"><span data-stu-id="72c47-127">These details will not be available if you are sending a test campaign i.e. a push sent tooa test device.</span></span> <span data-ttu-id="72c47-128">Här visas hur du ska tolka dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="72c47-128">Here is how you should interpret these details:</span></span>

1. <span data-ttu-id="72c47-129">**Pushas** – detta anger hello antal meddelanden som pushas toohello enheter.</span><span class="sxs-lookup"><span data-stu-id="72c47-129">**Pushed** - This specifies hello number of messages pushed toohello devices.</span></span> <span data-ttu-id="72c47-130">Det här antalet beror på hello målgrupp du angav när du skapar hello push-kampanj.</span><span class="sxs-lookup"><span data-stu-id="72c47-130">This number will depend on hello target audience you specified while creating hello push campaign.</span></span> <span data-ttu-id="72c47-131">Om du inte anger någon målgrupp, skickas den här push ut tooall hello registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="72c47-131">If you do not specify any target audience, then this push will be sent out tooall hello registered devices.</span></span> <span data-ttu-id="72c47-132">Precis som alla andra push-tjänster, vi inte push-meddelanden hello direkt toohello enheter men i stället push-installera dem toohello respektive plattform specifika Push Notification Services (PNS - WNS-APN/GCM) så att de kan tillhandahålla hello meddelanden toohello enheter.</span><span class="sxs-lookup"><span data-stu-id="72c47-132">Like all other push services, we do not push hello notifications directly toohello devices but instead push them toohello respective platform specific Push Notification Services (PNS - APNS/GCM/WNS) so that they can deliver hello notifications toohello devices.</span></span> 
2. <span data-ttu-id="72c47-133">**Leverera** – detta anger hello antal meddelanden som har levererats av hello PNS toohello enhet och bekräftas som tagits emot av Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="72c47-133">**Delivered** - This specifies hello number of messages which are successfully delivered by hello PNS toohello device and acknowledged as received by Mobile Engagement SDK.</span></span> 
   
   <span data-ttu-id="72c47-134">*Orsaker till levererade antalet är mindre än antalet intryckt:*</span><span class="sxs-lookup"><span data-stu-id="72c47-134">*Reasons for Delivered count being less than Pushed count:*</span></span>
   
   1. <span data-ttu-id="72c47-135">Om hello användaren har avinstallerats hello app från hello enhet men hello PNS inte känner till den för närvarande hello vi skicka hello push toohello PNS bort hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="72c47-135">If hello user has uninstalled hello app from hello device but hello PNS doesn't know about it at hello time we send hello push toohello PNS then hello message will be dropped.</span></span>
   2. <span data-ttu-id="72c47-136">Om hello enhet har hello app men hello enheter själva var offline under långa perioder misslyckas hello PNS toodeliver hello meddelandet toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="72c47-136">If hello device has hello app but hello devices themselves were offline for long periods of time, then hello PNS will fail toodeliver hello message toohello device.</span></span> 
   3. <span data-ttu-id="72c47-137">Om hello-meddelande levereras toohello enhet men hello Mobile Engagement SDK i hello app inte kan identifiera hello innehållet i hello-meddelande, släpper meddelandet.</span><span class="sxs-lookup"><span data-stu-id="72c47-137">If hello message does get delivered toohello device but hello Mobile Engagement SDK in hello app doesn’t recognize hello content of hello message, then it drops that message.</span></span> <span data-ttu-id="72c47-138">Detta kan inträffa om hello anpassning av hello-meddelande i hello app genererar ett undantag som vi fånga i hello SDK och släpp hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="72c47-138">This could happen if hello customization of hello notification in hello app generates an exception which we catch in hello SDK and drop hello message.</span></span> <span data-ttu-id="72c47-139">Detta kan också inträffa om hello appen på hello enhet med en version av hello Mobile Engagement SDK som inte kan toounderstand hello nyare version av hello push-meddelande skickas från hello plattform, men detta är endast när hello app uppgraderades efter hello-meddelande skickades från hello service plattform.</span><span class="sxs-lookup"><span data-stu-id="72c47-139">This could also occur if hello app on hello device is using a version of hello Mobile Engagement SDK which is not able toounderstand hello newer version of hello push message sent from hello platform but this is only when hello app was upgraded after hello notification was dispatched from hello service platform.</span></span> <span data-ttu-id="72c47-140">Hej **Avancerat** fliken talar om hur många meddelanden har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="72c47-140">hello **Advanced** tab will tell how many messages were dropped.</span></span> 
   4. <span data-ttu-id="72c47-141">På iOS-enheter meddelanden ibland inte levereras om antingen hello-enheten är för låg batterinivå eller om hello app förbrukar för mycket energi vid bearbetning av fjärr-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="72c47-141">On iOS devices, messages sometimes do not get delivered if either hello device is on low battery or if hello app is consuming significant amount of power when processing remote notifications.</span></span> <span data-ttu-id="72c47-142">Detta är en begränsning av hello iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="72c47-142">This is a limitation of hello iOS devices.</span></span>   
3. <span data-ttu-id="72c47-143">**Visas** – detta anger hello antal meddelanden som har visas toohello app användaren på hello-enhet i hello form av en systemavisering push/out-för-app i hello meddelandecentret eller ett meddelande i appen i hello mobila App.</span><span class="sxs-lookup"><span data-stu-id="72c47-143">**Displayed** - This specifies hello number of messages which are successfully shown toohello app user on hello device in hello form of a system push/out-of-app notification in hello notification center or an in-app notification within hello mobile app.</span></span>  <span data-ttu-id="72c47-144">Hej **Avancerat** fliken talar om hur många har systemmeddelanden och hur många har meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="72c47-144">hello **Advanced** tab will tell you how many were system notifications and how many were in-app notifications.</span></span> 
   
   <span data-ttu-id="72c47-145">*Orsaker till visas antalet är mindre än antalet levererade (väntar toobe visas)*</span><span class="sxs-lookup"><span data-stu-id="72c47-145">*Reasons for Displayed count being less than Delivered count (waiting toobe displayed)*</span></span>
   
   1. <span data-ttu-id="72c47-146">Om hello meddelandekampanj hade ett slutdatum på det är möjligt att hello meddelandet levererades men när hello tid kommer tooopen och visa det toohello app användaren, har den gått så att det har aldrig visas.</span><span class="sxs-lookup"><span data-stu-id="72c47-146">If hello notification campaign had an end date on it then it is possible that hello notification was delivered but when hello time came tooopen and display it toohello app user, it was already expired so it was never displayed.</span></span>   
   2. <span data-ttu-id="72c47-147">Om hello-meddelande är ett meddelande i appen sedan visas hello-meddelande bara när hello app användare öppnar hello app.</span><span class="sxs-lookup"><span data-stu-id="72c47-147">If hello notification is an in-app notification then hello notification is only displayed when hello app user opens hello app.</span></span> <span data-ttu-id="72c47-148">I fall där hello app användaren inte har öppnat hello app hello SDK rapporterar att hello-meddelande kunde levereras men har ännu inte visas tills hello appen öppnas.</span><span class="sxs-lookup"><span data-stu-id="72c47-148">In cases where hello app user hasn't opened hello app, hello SDK will report that hello notification was delivered but not yet displayed until hello app is opened.</span></span> 
   3. <span data-ttu-id="72c47-149">Om hello-meddelande är ett meddelande i appen och konfigurerat toobe som visas på en specifik aktivitet/skärm och sedan också hello-meddelande kommer att rapporteras som levereras men ännu inte levereras till öppnar hello användare hello-appen på en viss skärm.</span><span class="sxs-lookup"><span data-stu-id="72c47-149">If hello notification is an in-app notification and configured toobe shown on a specific activity/screen then also hello notification will be reported as delivered but not yet delivered until hello user opens hello app on a specific screen.</span></span> 
4. <span data-ttu-id="72c47-150">**Användarinteraktioner** – detta anger hello antal meddelanden som hello app användare har har åtgärdat med och ta hälsningsmeddelande som har hanterats eller stängts.</span><span class="sxs-lookup"><span data-stu-id="72c47-150">**User Interactions** - This specifies hello number of messages which hello app user has interacted with and will include hello messages which are either actioned or exited.</span></span> 
   
   * <span data-ttu-id="72c47-151">*hello app användaren kan åtgärda ett meddelande i något av följande sätt hello:*</span><span class="sxs-lookup"><span data-stu-id="72c47-151">*hello app user can action a notification in either of hello following ways:*</span></span>
     
     1. <span data-ttu-id="72c47-152">Om hello meddelande är ett system/out-för-app-meddelande eller ett meddelande i appen skickas som endast meddelande och hello app användaren klickar på hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="72c47-152">If hello notification is a system/out-of-app notification or an in-app notification sent as notification-only then hello app user clicks on hello notification.</span></span>
     2. <span data-ttu-id="72c47-153">Om hello-meddelande är ett meddelande i appen med en text eller en webbvy eller avsökningar hello sedan app användare klickar du på hello åtgärdsknappen i hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="72c47-153">If hello notification is an in-app notification with a text or web-view or polls then hello app user clicks on hello Action button in hello notification.</span></span>
     3. <span data-ttu-id="72c47-154">Om hello-meddelande är ett meddelande i appen med en webbvy sedan hello app användaren klickar på en URL i hello webbvy [Android endast]</span><span class="sxs-lookup"><span data-stu-id="72c47-154">If hello notification is an in-app notification with a web-view then hello app user clicks on a URL in hello web view [Android Only]</span></span>
   * <span data-ttu-id="72c47-155">*hello app användaren kan avsluta ett meddelande i något av följande sätt hello:*</span><span class="sxs-lookup"><span data-stu-id="72c47-155">*hello app user can exit a notification in either of hello following ways:*</span></span>
     
     1. <span data-ttu-id="72c47-156">Klicka hello Stäng i hello-meddelande direkt.</span><span class="sxs-lookup"><span data-stu-id="72c47-156">Clicking hello close button on hello notification directly.</span></span> 
     2. <span data-ttu-id="72c47-157">Svepa in eller ta bort hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="72c47-157">Swiping away or deleting hello notification.</span></span> 
     3. <span data-ttu-id="72c47-158">Meddelanden i appen med text/webbinnehåll och avsökningar är vanligtvis visas toohello app användare i två steg.</span><span class="sxs-lookup"><span data-stu-id="72c47-158">In-app notifications with text/web content and polls are typically displayed toohello app user in a two-step process.</span></span> <span data-ttu-id="72c47-159">Ett meddelande visas först och när de klickar på den de se hello efterföljande text/web/avsökning innehåll.</span><span class="sxs-lookup"><span data-stu-id="72c47-159">They see a notification first and when they click on it, they see hello subsequent text/web/poll content.</span></span> <span data-ttu-id="72c47-160">hello app användaren kan avsluta ett meddelande i någon av de här stegen och hello information i Avancerat läge för hello samlar in det här.</span><span class="sxs-lookup"><span data-stu-id="72c47-160">hello app user can exit a notification in either of these steps and hello details in hello Advanced view captures this.</span></span> 
5. <span data-ttu-id="72c47-161">**Åtgärdade** – detta anger hello antal meddelanden som uttryckligen åtgärdade av hello app användare.</span><span class="sxs-lookup"><span data-stu-id="72c47-161">**Actioned** - This specifies hello number of messages which were explicitly actioned by hello app user.</span></span> <span data-ttu-id="72c47-162">Detta är mest intressanta hello-nummer som det här visar hur många användare som appen har intresserad av du push-installeras i hello-meddelande hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="72c47-162">This is hello most interesting number as this tells how many app users were interested by hello message you pushed out in hello notification.</span></span> 

> [!NOTE]
> <span data-ttu-id="72c47-163">På iOS & Windows-plattformar om hello användare har hello app öppna och hello kampanj har kampanj ”när som helst” är det möjligt att både från appen och i appen meddelanden visas på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="72c47-163">On iOS & Windows platforms, if hello user has hello app open and hello campaign was an "AnyTime" campaign then it is possible that both out of app and in-app notifications are displayed at hello same time.</span></span> <span data-ttu-id="72c47-164">Detta kan orsaka en högre än hello levererade visas antalet.</span><span class="sxs-lookup"><span data-stu-id="72c47-164">This may cause a Displayed count higher than hello Delivered.</span></span> <span data-ttu-id="72c47-165">Om hello användaren interagerar eller åtgärder hello-meddelande, sedan även hello användaren interaktioner/Actioned antalet kan vara större än levererade.</span><span class="sxs-lookup"><span data-stu-id="72c47-165">If hello user interacts or actions hello notification, then even hello User Interactions/Actioned count could be greater than Delivered.</span></span> 
> 
> 

![Reach2][19]

## <a name="see-also"></a><span data-ttu-id="72c47-167">Se även</span><span class="sxs-lookup"><span data-stu-id="72c47-167">See also</span></span>
* <span data-ttu-id="72c47-168">[Begrepp][Link 6]</span><span class="sxs-lookup"><span data-stu-id="72c47-168">[Concepts][Link 6]</span></span>
* <span data-ttu-id="72c47-169">[Felsöka Guide Service][Link 24]</span><span class="sxs-lookup"><span data-stu-id="72c47-169">[Troubleshooting Guide Service][Link 24]</span></span>

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

