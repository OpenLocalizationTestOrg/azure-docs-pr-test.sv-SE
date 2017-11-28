---
title: aaaSend plattformsoberoende meddelanden toousers med Notification Hubs (ASP.NET)
description: "Lär dig hur toouse Meddelandehubbar mallar toosend, i en enskild begäran, ett plattformsoberoende meddelande som riktar sig till alla plattformar."
services: notification-hubs
documentationcenter: 
author: ysxu
manager: erikre
editor: 
ms.assetid: 11d2131b-f683-47fd-a691-4cdfc696f62b
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: multiple
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: f105b871b809e739dd5c05ea819ad135e842ebb0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-cross-platform-notifications-toousers-with-notification-hubs"></a><span data-ttu-id="33277-103">Skicka plattformsoberoende meddelanden toousers med Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="33277-103">Send cross-platform notifications toousers with Notification Hubs</span></span>
<span data-ttu-id="33277-104">I de föregående hello-självstudierna [meddela användare med Notification Hubs], du har lärt dig hur toopush meddelanden tooall enheter som registrerats av en viss autentiserade användare.</span><span class="sxs-lookup"><span data-stu-id="33277-104">In hello previous tutorial [Notify users with Notification Hubs], you learned how toopush notifications tooall devices registered by a specific authenticated user.</span></span> <span data-ttu-id="33277-105">I den här självstudien har flera begäranden krävs toosend klientplattform ett meddelande tooeach som stöds.</span><span class="sxs-lookup"><span data-stu-id="33277-105">In that tutorial, multiple requests were required toosend a notification tooeach supported client platform.</span></span> <span data-ttu-id="33277-106">Notification Hubs stöder mallar, vilket gör att du kan ange hur en specifik enhet vill tooreceive meddelanden.</span><span class="sxs-lookup"><span data-stu-id="33277-106">Notification Hubs supports templates, which let you specify how a specific device wants tooreceive notifications.</span></span> <span data-ttu-id="33277-107">Detta förenklar skicka plattformsoberoende meddelanden.</span><span class="sxs-lookup"><span data-stu-id="33277-107">This simplifies sending cross-platform notifications.</span></span> <span data-ttu-id="33277-108">Det här avsnittet visar hur tootake nytta av mallar toosend i en enskild begäran, ett plattformsoberoende meddelande som riktar sig till alla plattformar.</span><span class="sxs-lookup"><span data-stu-id="33277-108">This topic demonstrates how tootake advantage of templates toosend, in a single request, a platform-agnostic notification that targets all platforms.</span></span> <span data-ttu-id="33277-109">Mer detaljerad information om mallar finns [översikt över Azure Notification Hubs][Templates].</span><span class="sxs-lookup"><span data-stu-id="33277-109">For more detailed information about templates, see [Azure Notification Hubs Overview][Templates].</span></span>
> [!IMPORTANT]
> <span data-ttu-id="33277-110">Projekt för Windows Phone 8.1 och tidigare stöds inte med hjälp av Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="33277-110">Windows Phone projects 8.1 and earlier are not supported using Visual Studio 2017.</span></span> <span data-ttu-id="33277-111">Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).</span><span class="sxs-lookup"><span data-stu-id="33277-111">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span>

> [!NOTE]
> <span data-ttu-id="33277-112">Notification Hubs gör att en enhet tooregister flera mallar med hello samma tagg.</span><span class="sxs-lookup"><span data-stu-id="33277-112">Notification Hubs allows a device tooregister multiple templates with hello same tag.</span></span> <span data-ttu-id="33277-113">I det här fallet levereras ett inkommande meddelande som mål att taggen resulterar i flera meddelanden toohello enhet, en för varje mall.</span><span class="sxs-lookup"><span data-stu-id="33277-113">In this case, an incoming message targeting that tag results in multiple notifications delivered toohello device, one for each template.</span></span> <span data-ttu-id="33277-114">Det här kan du toodisplay hello samma meddelande i flera visuella meddelanden, till exempel både som en Aktivitetsikon och som ett popup-meddelande i en Windows Store-app.</span><span class="sxs-lookup"><span data-stu-id="33277-114">This enables you toodisplay hello same message in multiple visual notifications, such as both as a badge and as a toast notification in a Windows Store app.</span></span>
> 
> 

<span data-ttu-id="33277-115">Utför följande steg toosend plattformsoberoende meddelanden med hjälp av mallar hello:</span><span class="sxs-lookup"><span data-stu-id="33277-115">Complete hello following steps toosend cross-platform notifications using templates:</span></span>

1. <span data-ttu-id="33277-116">Hello Solution Explorer i Visual Studio, expandera hello **domänkontrollanter** mapp och sedan öppna hello RegisterController.cs fil.</span><span class="sxs-lookup"><span data-stu-id="33277-116">In hello Solution Explorer in Visual Studio, expand hello **Controllers** folder, then open hello RegisterController.cs file.</span></span>
2. <span data-ttu-id="33277-117">Hitta hello kodblock i hello **placera** metod som skapar en ny registrering ersätta hello `switch` innehåll med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="33277-117">Locate hello block of code in hello **Put** method that creates a new registration replace hello `switch` content with hello following code:</span></span>
   
        switch (deviceUpdate.Platform)
        {
            case "mpns":
                var toastTemplate = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                    "<wp:Notification xmlns:wp=\"WPNotification\">" +
                       "<wp:Toast>" +
                            "<wp:Text1>$(message)</wp:Text1>" +
                       "</wp:Toast> " +
                    "</wp:Notification>";
                registration = new MpnsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "wns":
                toastTemplate = @"<toast><visual><binding template=""ToastText01""><text id=""1"">$(message)</text></binding></visual></toast>";
                registration = new WindowsTemplateRegistrationDescription(deviceUpdate.Handle, toastTemplate);
                break;
            case "apns":
                var alertTemplate = "{\"aps\":{\"alert\":\"$(message)\"}}";
                registration = new AppleTemplateRegistrationDescription(deviceUpdate.Handle, alertTemplate);
                break;
            case "gcm":
                var messageTemplate = "{\"data\":{\"message\":\"$(message)\"}}";
                registration = new GcmTemplateRegistrationDescription(deviceUpdate.Handle, messageTemplate);
                break;
            default:
                throw new HttpResponseException(HttpStatusCode.BadRequest);
        }
   
    <span data-ttu-id="33277-118">Den här koden anropar hello plattformsspecifika metoden toocreate en mall för registrering i stället för en intern registrering.</span><span class="sxs-lookup"><span data-stu-id="33277-118">This code calls hello platform-specific method toocreate a template registration instead of a native registration.</span></span> <span data-ttu-id="33277-119">Befintliga registreringar behöver inte ändras eftersom mallen registreringar härledd från interna registreringar.</span><span class="sxs-lookup"><span data-stu-id="33277-119">Existing registrations need not be modified because template registrations derive from native registrations.</span></span>
3. <span data-ttu-id="33277-120">I hello **meddelanden** styrenhet, Ersätt hello **sendNotification** metod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="33277-120">In hello **Notifications** controller, replace hello **sendNotification** method with hello following code:</span></span>
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    <span data-ttu-id="33277-121">Den här koden skickar ett meddelande tooall plattformar vid hello samma tid och utan att behöva toospecify en inbyggd nyttolast.</span><span class="sxs-lookup"><span data-stu-id="33277-121">This code sends a notification tooall platforms at hello same time and without having toospecify a native payload.</span></span> <span data-ttu-id="33277-122">Notification Hubs bygger och ger hello rätt nyttolast tooevery enheten med hello tillhandahålls *taggen* värdet som anges i hello registrerade mallar.</span><span class="sxs-lookup"><span data-stu-id="33277-122">Notification Hubs builds and delivers hello correct payload tooevery device with hello provided *tag* value, as specified in hello registered templates.</span></span>
4. <span data-ttu-id="33277-123">Publicera projektet WebApi-serverdel.</span><span class="sxs-lookup"><span data-stu-id="33277-123">Re-publish your WebApi back-end project.</span></span>
5. <span data-ttu-id="33277-124">Kör hello-klientappen igen och kontrollera att registreringen lyckas.</span><span class="sxs-lookup"><span data-stu-id="33277-124">Run hello client app again and verify that registration succeeds.</span></span>
6. <span data-ttu-id="33277-125">(Valfritt) Distribuera hello app tooa andra klientenheten och sedan köra hello app.</span><span class="sxs-lookup"><span data-stu-id="33277-125">(Optional) Deploy hello client app tooa second device, then run hello app.</span></span>
   
    <span data-ttu-id="33277-126">Observera att visas ett meddelande på varje enhet.</span><span class="sxs-lookup"><span data-stu-id="33277-126">Note that a notification is displayed on each device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="33277-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33277-127">Next Steps</span></span>
<span data-ttu-id="33277-128">Nu när du har slutfört den här kursen lär du dig mer om Notification Hubs och mallar i följande avsnitt:</span><span class="sxs-lookup"><span data-stu-id="33277-128">Now that you have completed this tutorial, find out more about Notification Hubs and templates in these topics:</span></span>

* <span data-ttu-id="33277-129">**[Använda Notification Hubs toosend senaste nytt]**</span><span class="sxs-lookup"><span data-stu-id="33277-129">**[Use Notification Hubs toosend breaking news]**</span></span> <br/><span data-ttu-id="33277-130">Visar ett annat scenario för med hjälp av mallar</span><span class="sxs-lookup"><span data-stu-id="33277-130">Demonstrates another scenario for using templates</span></span>
* <span data-ttu-id="33277-131">**[Översikt över Azure Notification Hubs][Templates]**</span><span class="sxs-lookup"><span data-stu-id="33277-131">**[Azure Notification Hubs Overview][Templates]**</span></span><br/><span data-ttu-id="33277-132">Översiktsavsnittet detaljerad mer information om mallar.</span><span class="sxs-lookup"><span data-stu-id="33277-132">Overview topic has more detailed information on templates.</span></span>

<!-- Anchors. -->

<!-- Images. -->




<!-- URLs. -->
[Push toousers ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Push toousers Mobile Services]: /manage/services/notification-hubs/notify-users/
[Visual Studio 2012 Express for Windows 8]: http://go.microsoft.com/fwlink/?LinkId=257546

[Använda Notification Hubs toosend senaste nytt]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[Azure Notification Hubs]: http://go.microsoft.com/fwlink/p/?LinkId=314257
[meddela användare med Notification Hubs]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Templates]: http://go.microsoft.com/fwlink/p/?LinkId=317339
[Notification Hub How toofor Windows Store]: http://msdn.microsoft.com/library/windowsazure/jj927172.aspx
