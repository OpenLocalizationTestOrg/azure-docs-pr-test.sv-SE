---
title: "Användargränssnittet för Azure Mobile Engagement - Reach"
description: "Lär dig att nå ut till användare av ditt program med push-meddelanden med Azure Mobile Engagement"
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
ms.openlocfilehash: ce30456e41ff1a2f4824bcb64246ee115fdd1ef7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-reach-out-to-the-users-of-your-application-with-push-notifications"></a><span data-ttu-id="a525c-103">Hur du nå ut till användare av ditt program med push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="a525c-103">How to reach out to the users of your application with push notifications</span></span>
<span data-ttu-id="a525c-104">Den här artikeln beskriver den **NÅ** för den **Mobile Engagement** portal.</span><span class="sxs-lookup"><span data-stu-id="a525c-104">This article describes the **REACH** tab of the **Mobile Engagement** portal.</span></span> <span data-ttu-id="a525c-105">Du använder den **Mobile Engagement** portalen för att övervaka och hantera dina mobila appar.</span><span class="sxs-lookup"><span data-stu-id="a525c-105">You use the **Mobile Engagement** portal to monitor and manage your mobile apps.</span></span> <span data-ttu-id="a525c-106">Observera att börja använda portalen måste du först skapa en **Azure Mobile Engagement** konto.</span><span class="sxs-lookup"><span data-stu-id="a525c-106">Note that to start using the portal you first need to create an **Azure Mobile Engagement** account.</span></span> <span data-ttu-id="a525c-107">Mer information finns i [skapa ett Azure Mobile Engagement-konto](mobile-engagement-create.md).</span><span class="sxs-lookup"><span data-stu-id="a525c-107">For more information, see [Create an Azure Mobile Engagement account](mobile-engagement-create.md).</span></span>

<span data-ttu-id="a525c-108">Avsnittet Reach i Användargränssnittet är Push-kampanj hanteringsverktyg som du kan skapa/redigera/aktivera/Slutför/Övervakare och få statistik på Push notification-kampanjer och funktioner som kan även nås via API: et nå (och vissa delar av låga Push-API) .</span><span class="sxs-lookup"><span data-stu-id="a525c-108">The Reach section of the UI is the Push campaign management tool where you can create/edit/activate/finish/monitor and get statistics on Push notification campaigns and features that can also be accessed via the Reach API (and some elements of the low level Push API).</span></span> <span data-ttu-id="a525c-109">Kom ihåg att om du använder API: erna eller Användargränssnittet, behöver du integrera räckvidd i programmet för varje plattform med SDK innan du kan använda och Azure Mobile Engagement nå kampanjer.</span><span class="sxs-lookup"><span data-stu-id="a525c-109">Remember that whether you are using the APIs or the UI, you will need to integrate both Azure Mobile Engagement and Reach into your application for each platform with the SDK before you can use Reach campaigns.</span></span>

> [!NOTE]
> <span data-ttu-id="a525c-110">Många avsnitt i den **Mobile Engagement** portal Användargränssnittet innehåller den **Visa hjälp** knappen.</span><span class="sxs-lookup"><span data-stu-id="a525c-110">Many sections of the **Mobile Engagement** portal UI contain the **SHOW HELP** button.</span></span> <span data-ttu-id="a525c-111">Tryck på knappen för att få mer information om ett avsnitt.</span><span class="sxs-lookup"><span data-stu-id="a525c-111">Press this button to get more contextual information about a section.</span></span>
> 
> 

## <a name="four-types-of-push-notifications"></a><span data-ttu-id="a525c-112">Fyra typer av Push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="a525c-112">Four types of Push notifications</span></span>
1. <span data-ttu-id="a525c-113">Meddelanden – kan du skicka reklam meddelanden till användare som omdirigerar dem till en annan plats i din app och skicka dem till en webbsida eller store utanför din app.</span><span class="sxs-lookup"><span data-stu-id="a525c-113">Announcements - allow you to send advertising messages to users that redirect them to another location inside your app or to send them to a webpage or store outside of your app.</span></span> 
2. <span data-ttu-id="a525c-114">Omröstningar - kan du samla in information från slutanvändarna genom att ställa frågor till dem..</span><span class="sxs-lookup"><span data-stu-id="a525c-114">Polls - allow you to gather information from end users by asking them questions.</span></span>
3. <span data-ttu-id="a525c-115">Data-push - kan du skicka en binär fil eller base64-datafil.</span><span class="sxs-lookup"><span data-stu-id="a525c-115">Data Pushes - allow you to send a binary or base64 data file.</span></span> <span data-ttu-id="a525c-116">Informationen i en data-push skickas till appen och ändra din aktuella användarupplevelsen i din app.</span><span class="sxs-lookup"><span data-stu-id="a525c-116">The information contained in a data push is sent to your application to modify your users' current experience in your app.</span></span> <span data-ttu-id="a525c-117">Programmet måste kunna bearbeta data i en data-push.</span><span class="sxs-lookup"><span data-stu-id="a525c-117">Your application needs to be able to process the data in a data push.</span></span>

## <a name="campaign-details"></a><span data-ttu-id="a525c-118">Information om kampanjer</span><span class="sxs-lookup"><span data-stu-id="a525c-118">Campaign Details</span></span>
<span data-ttu-id="a525c-119">Du kan redigera klona, ta bort eller aktivera kampanjer som inte har aktiverats ännu hovrar över deras namn eller kan du öppna dem..</span><span class="sxs-lookup"><span data-stu-id="a525c-119">You can edit, clone, delete, or activate campaigns that have not been activated yet by hovering over their names or you can click to open them.</span></span> <span data-ttu-id="a525c-120">Du kan också klona kampanjer som redan har aktiverats av användaren håller muspekaren över deras namn eller kan du öppna dem..</span><span class="sxs-lookup"><span data-stu-id="a525c-120">You can clone campaigns that have already been activated by hovering over their names or you can click to open them.</span></span> <span data-ttu-id="a525c-121">Du kan inte ändra en kampanj när den väl har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="a525c-121">However, you can't change a campaign once it has been activated.</span></span>

![Reach1][18]

## <a name="reach-feedback"></a><span data-ttu-id="a525c-123">Nå Feedback</span><span class="sxs-lookup"><span data-stu-id="a525c-123">Reach Feedback</span></span>
<span data-ttu-id="a525c-124">Klicka på **statistik** visas detaljerad information om en Reach-kampanj.</span><span class="sxs-lookup"><span data-stu-id="a525c-124">Click on **Statistics** to see the details of a Reach campaign.</span></span> <span data-ttu-id="a525c-125">Den **enkel** vyn innehåller en bild i form av ett stapeldiagram i kolumnen om vad som hänt efter en kampanj har aktiverats.</span><span class="sxs-lookup"><span data-stu-id="a525c-125">The **Simple** view provides a visual representation in the form of a column bar graph about what happened after a campaign was activated.</span></span> <span data-ttu-id="a525c-126">Den **Avancerat** vyn innehåller mer detaljerad information om push-kampanj.</span><span class="sxs-lookup"><span data-stu-id="a525c-126">The **Advanced** view provides more granular details about the push campaign.</span></span> <span data-ttu-id="a525c-127">Dessa uppgifter är inte tillgängligt om du skickar ett testkampanj d.v.s. en push skickas till en testenhet.</span><span class="sxs-lookup"><span data-stu-id="a525c-127">These details will not be available if you are sending a test campaign i.e. a push sent to a test device.</span></span> <span data-ttu-id="a525c-128">Här visas hur du ska tolka dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="a525c-128">Here is how you should interpret these details:</span></span>

1. <span data-ttu-id="a525c-129">**Pushas** – detta anger antalet meddelanden som har pushats till enheter.</span><span class="sxs-lookup"><span data-stu-id="a525c-129">**Pushed** - This specifies the number of messages pushed to the devices.</span></span> <span data-ttu-id="a525c-130">Det här antalet beror på den målgrupp som du angav när du skapar push-kampanj.</span><span class="sxs-lookup"><span data-stu-id="a525c-130">This number will depend on the target audience you specified while creating the push campaign.</span></span> <span data-ttu-id="a525c-131">Om du inte anger någon målgrupp, sedan skickas den här push till registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="a525c-131">If you do not specify any target audience, then this push will be sent out to all the registered devices.</span></span> <span data-ttu-id="a525c-132">Precis som alla push-tjänster vi inte push-meddelanden direkt till enheter men i stället push-installera dem respektive plattforms specifika Push Notification Services (PNS - WNS-APN/GCM) så att de kan skicka meddelanden till enheterna.</span><span class="sxs-lookup"><span data-stu-id="a525c-132">Like all other push services, we do not push the notifications directly to the devices but instead push them to the respective platform specific Push Notification Services (PNS - APNS/GCM/WNS) so that they can deliver the notifications to the devices.</span></span> 
2. <span data-ttu-id="a525c-133">**Leverera** – detta anger antalet meddelanden som har levererats av pns-systemet till enheten och bekräftas som tagits emot av Mobile Engagement SDK.</span><span class="sxs-lookup"><span data-stu-id="a525c-133">**Delivered** - This specifies the number of messages which are successfully delivered by the PNS to the device and acknowledged as received by Mobile Engagement SDK.</span></span> 
   
   <span data-ttu-id="a525c-134">*Orsaker till levererade antalet är mindre än antalet intryckt:*</span><span class="sxs-lookup"><span data-stu-id="a525c-134">*Reasons for Delivered count being less than Pushed count:*</span></span>
   
   1. <span data-ttu-id="a525c-135">Om användaren har avinstalleras appen från enheten och pns-systemet inte känner till den när vi skickar push-meddelandet till pns-systemet bort meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a525c-135">If the user has uninstalled the app from the device but the PNS doesn't know about it at the time we send the push to the PNS then the message will be dropped.</span></span>
   2. <span data-ttu-id="a525c-136">Om enheten har appen men själva enheterna var offline under långa tidsperioder, misslyckas pns-systemet att leverera meddelandet till enheten.</span><span class="sxs-lookup"><span data-stu-id="a525c-136">If the device has the app but the devices themselves were offline for long periods of time, then the PNS will fail to deliver the message to the device.</span></span> 
   3. <span data-ttu-id="a525c-137">Om meddelandet levereras till enheten men Mobile Engagement SDK i appen inte kan identifiera innehållet i meddelandet, släpper meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a525c-137">If the message does get delivered to the device but the Mobile Engagement SDK in the app doesn’t recognize the content of the message, then it drops that message.</span></span> <span data-ttu-id="a525c-138">Detta kan inträffa om anpassning av meddelanden i appen genererar ett undantag som vi fånga i SDK och släpp meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a525c-138">This could happen if the customization of the notification in the app generates an exception which we catch in the SDK and drop the message.</span></span> <span data-ttu-id="a525c-139">Detta kan även uppstå om appen på enheten använder en version av Mobile Engagement SDK som inte kan förstå den nya versionen av push-meddelande som skickas från plattformen men detta är endast när appen har uppgraderats när meddelandet skickades från t han service plattform.</span><span class="sxs-lookup"><span data-stu-id="a525c-139">This could also occur if the app on the device is using a version of the Mobile Engagement SDK which is not able to understand the newer version of the push message sent from the platform but this is only when the app was upgraded after the notification was dispatched from the service platform.</span></span> <span data-ttu-id="a525c-140">Den **Avancerat** fliken talar om hur många meddelanden har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a525c-140">The **Advanced** tab will tell how many messages were dropped.</span></span> 
   4. <span data-ttu-id="a525c-141">På iOS-enheter meddelanden ibland inte levereras om antingen enheten är för låg batterinivå eller om appen förbrukar för mycket energi vid bearbetning av fjärr-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="a525c-141">On iOS devices, messages sometimes do not get delivered if either the device is on low battery or if the app is consuming significant amount of power when processing remote notifications.</span></span> <span data-ttu-id="a525c-142">Detta är en begränsning av iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="a525c-142">This is a limitation of the iOS devices.</span></span>   
3. <span data-ttu-id="a525c-143">**Visas** – detta anger antalet meddelanden som har upp till app-användare på enheten i form av en systemavisering push/out-för-app i meddelandecentret eller ett meddelande i appen i mobilappen.</span><span class="sxs-lookup"><span data-stu-id="a525c-143">**Displayed** - This specifies the number of messages which are successfully shown to the app user on the device in the form of a system push/out-of-app notification in the notification center or an in-app notification within the mobile app.</span></span>  <span data-ttu-id="a525c-144">Den **Avancerat** fliken talar om hur många har systemmeddelanden och hur många har meddelanden i appen.</span><span class="sxs-lookup"><span data-stu-id="a525c-144">The **Advanced** tab will tell you how many were system notifications and how many were in-app notifications.</span></span> 
   
   <span data-ttu-id="a525c-145">*Orsaker till visas antalet är mindre än levererade antal (väntar på att visas)*</span><span class="sxs-lookup"><span data-stu-id="a525c-145">*Reasons for Displayed count being less than Delivered count (waiting to be displayed)*</span></span>
   
   1. <span data-ttu-id="a525c-146">Om meddelandekampanj hade ett slutdatum på det är det möjligt att meddelandet levererades men vid tiden kommer att öppna och visa det app-användaren kan det har gått så att det har aldrig visas.</span><span class="sxs-lookup"><span data-stu-id="a525c-146">If the notification campaign had an end date on it then it is possible that the notification was delivered but when the time came to open and display it to the app user, it was already expired so it was never displayed.</span></span>   
   2. <span data-ttu-id="a525c-147">Om meddelandet är ett meddelande i appen och sedan meddelandet visas bara när app-användare öppnar appen.</span><span class="sxs-lookup"><span data-stu-id="a525c-147">If the notification is an in-app notification then the notification is only displayed when the app user opens the app.</span></span> <span data-ttu-id="a525c-148">I fall där appen användaren inte har öppnat appen rapporterar SDK att meddelandet kunde levereras men har ännu inte visas förrän den har öppnats.</span><span class="sxs-lookup"><span data-stu-id="a525c-148">In cases where the app user hasn't opened the app, the SDK will report that the notification was delivered but not yet displayed until the app is opened.</span></span> 
   3. <span data-ttu-id="a525c-149">Om meddelandet är ett meddelande i appen och konfigurerats för att visas på en specifik aktivitet/skärm och sedan också meddelandet kommer att rapporteras som levererats men ännu inte levereras till öppnar användaren appen på en viss skärm.</span><span class="sxs-lookup"><span data-stu-id="a525c-149">If the notification is an in-app notification and configured to be shown on a specific activity/screen then also the notification will be reported as delivered but not yet delivered until the user opens the app on a specific screen.</span></span> 
4. <span data-ttu-id="a525c-150">**Användarinteraktioner** – detta anger antalet meddelanden som appen användaren har har åtgärdat med och meddelanden som har hanterats eller stängts tas med.</span><span class="sxs-lookup"><span data-stu-id="a525c-150">**User Interactions** - This specifies the number of messages which the app user has interacted with and will include the messages which are either actioned or exited.</span></span> 
   
   * <span data-ttu-id="a525c-151">*App-användaren kan åtgärda ett meddelande i något av följande sätt:*</span><span class="sxs-lookup"><span data-stu-id="a525c-151">*The app user can action a notification in either of the following ways:*</span></span>
     
     1. <span data-ttu-id="a525c-152">Om meddelandet är ett system/out-för-app-meddelande eller en app meddelande meddelande endast sedan app användaren klickar på meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a525c-152">If the notification is a system/out-of-app notification or an in-app notification sent as notification-only then the app user clicks on the notification.</span></span>
     2. <span data-ttu-id="a525c-153">Om meddelandet är ett meddelande i appen med en text eller Webbvy eller avsökningar sedan app användaren klickar på knappen åtgärd i meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a525c-153">If the notification is an in-app notification with a text or web-view or polls then the app user clicks on the Action button in the notification.</span></span>
     3. <span data-ttu-id="a525c-154">Om meddelandet är ett meddelande i appen med en webbvy app användaren klickar på en URL i webbvyn [Android endast]</span><span class="sxs-lookup"><span data-stu-id="a525c-154">If the notification is an in-app notification with a web-view then the app user clicks on a URL in the web view [Android Only]</span></span>
   * <span data-ttu-id="a525c-155">*App-användaren kan avsluta ett meddelande i något av följande sätt:*</span><span class="sxs-lookup"><span data-stu-id="a525c-155">*The app user can exit a notification in either of the following ways:*</span></span>
     
     1. <span data-ttu-id="a525c-156">Klicka på knappen Stäng på meddelandet direkt.</span><span class="sxs-lookup"><span data-stu-id="a525c-156">Clicking the close button on the notification directly.</span></span> 
     2. <span data-ttu-id="a525c-157">Svepa in eller ta bort meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a525c-157">Swiping away or deleting the notification.</span></span> 
     3. <span data-ttu-id="a525c-158">Meddelanden i appen med text/webbinnehåll och avsökningar visas vanligtvis till app-användare i två steg.</span><span class="sxs-lookup"><span data-stu-id="a525c-158">In-app notifications with text/web content and polls are typically displayed to the app user in a two-step process.</span></span> <span data-ttu-id="a525c-159">Ett meddelande visas först och när de klickar på den de se efterföljande text/web/avsökning innehållet.</span><span class="sxs-lookup"><span data-stu-id="a525c-159">They see a notification first and when they click on it, they see the subsequent text/web/poll content.</span></span> <span data-ttu-id="a525c-160">App-användaren kan avsluta ett meddelande i någon av de här stegen och informationen i Avancerat läge samlar in det här.</span><span class="sxs-lookup"><span data-stu-id="a525c-160">The app user can exit a notification in either of these steps and the details in the Advanced view captures this.</span></span> 
5. <span data-ttu-id="a525c-161">**Åtgärdade** – detta anger antalet meddelanden som uttryckligen åtgärdade av app-användare.</span><span class="sxs-lookup"><span data-stu-id="a525c-161">**Actioned** - This specifies the number of messages which were explicitly actioned by the app user.</span></span> <span data-ttu-id="a525c-162">Detta är det mest intressanta antalet detta anger hur många användare som appen har intresse av du push-installeras i meddelandet för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="a525c-162">This is the most interesting number as this tells how many app users were interested by the message you pushed out in the notification.</span></span> 

> [!NOTE]
> <span data-ttu-id="a525c-163">På iOS & Windows-plattformar, om användaren har appen öppna och kampanjen var kampanj ”när som helst” och det är möjligt att båda utanför appen och meddelanden i appen visas på samma gång.</span><span class="sxs-lookup"><span data-stu-id="a525c-163">On iOS & Windows platforms, if the user has the app open and the campaign was an "AnyTime" campaign then it is possible that both out of app and in-app notifications are displayed at the same time.</span></span> <span data-ttu-id="a525c-164">Detta kan orsaka en högre än levererat visas antalet.</span><span class="sxs-lookup"><span data-stu-id="a525c-164">This may cause a Displayed count higher than the Delivered.</span></span> <span data-ttu-id="a525c-165">Om användaren interagerar eller åtgärder meddelandet och även antalet användare interaktioner/Actioned kan vara större än levererade.</span><span class="sxs-lookup"><span data-stu-id="a525c-165">If the user interacts or actions the notification, then even the User Interactions/Actioned count could be greater than Delivered.</span></span> 
> 
> 

![Reach2][19]

## <a name="see-also"></a><span data-ttu-id="a525c-167">Se även</span><span class="sxs-lookup"><span data-stu-id="a525c-167">See also</span></span>
* <span data-ttu-id="a525c-168">[Begrepp][Link 6]</span><span class="sxs-lookup"><span data-stu-id="a525c-168">[Concepts][Link 6]</span></span>
* <span data-ttu-id="a525c-169">[Felsöka Guide Service][Link 24]</span><span class="sxs-lookup"><span data-stu-id="a525c-169">[Troubleshooting Guide Service][Link 24]</span></span>

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

