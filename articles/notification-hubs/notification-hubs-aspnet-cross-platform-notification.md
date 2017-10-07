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
# <a name="send-cross-platform-notifications-toousers-with-notification-hubs"></a>Skicka plattformsoberoende meddelanden toousers med Notification Hubs
I de föregående hello-självstudierna [meddela användare med Notification Hubs], du har lärt dig hur toopush meddelanden tooall enheter som registrerats av en viss autentiserade användare. I den här självstudien har flera begäranden krävs toosend klientplattform ett meddelande tooeach som stöds. Notification Hubs stöder mallar, vilket gör att du kan ange hur en specifik enhet vill tooreceive meddelanden. Detta förenklar skicka plattformsoberoende meddelanden. Det här avsnittet visar hur tootake nytta av mallar toosend i en enskild begäran, ett plattformsoberoende meddelande som riktar sig till alla plattformar. Mer detaljerad information om mallar finns [översikt över Azure Notification Hubs][Templates].
> [!IMPORTANT]
> Projekt för Windows Phone 8.1 och tidigare stöds inte med hjälp av Visual Studio 2017. Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).

> [!NOTE]
> Notification Hubs gör att en enhet tooregister flera mallar med hello samma tagg. I det här fallet levereras ett inkommande meddelande som mål att taggen resulterar i flera meddelanden toohello enhet, en för varje mall. Det här kan du toodisplay hello samma meddelande i flera visuella meddelanden, till exempel både som en Aktivitetsikon och som ett popup-meddelande i en Windows Store-app.
> 
> 

Utför följande steg toosend plattformsoberoende meddelanden med hjälp av mallar hello:

1. Hello Solution Explorer i Visual Studio, expandera hello **domänkontrollanter** mapp och sedan öppna hello RegisterController.cs fil.
2. Hitta hello kodblock i hello **placera** metod som skapar en ny registrering ersätta hello `switch` innehåll med hello följande kod:
   
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
   
    Den här koden anropar hello plattformsspecifika metoden toocreate en mall för registrering i stället för en intern registrering. Befintliga registreringar behöver inte ändras eftersom mallen registreringar härledd från interna registreringar.
3. I hello **meddelanden** styrenhet, Ersätt hello **sendNotification** metod med hello följande kod:
   
        public async Task<HttpResponseMessage> Post()
        {
            var user = HttpContext.Current.User.Identity.Name;
            var userTag = "username:" + user;
   
            var notification = new Dictionary<string, string> { { "message", "Hello, " + user } };
            await Notifications.Instance.Hub.SendTemplateNotificationAsync(notification, userTag);
   
            return Request.CreateResponse(HttpStatusCode.OK);
        }
   
    Den här koden skickar ett meddelande tooall plattformar vid hello samma tid och utan att behöva toospecify en inbyggd nyttolast. Notification Hubs bygger och ger hello rätt nyttolast tooevery enheten med hello tillhandahålls *taggen* värdet som anges i hello registrerade mallar.
4. Publicera projektet WebApi-serverdel.
5. Kör hello-klientappen igen och kontrollera att registreringen lyckas.
6. (Valfritt) Distribuera hello app tooa andra klientenheten och sedan köra hello app.
   
    Observera att visas ett meddelande på varje enhet.

## <a name="next-steps"></a>Nästa steg
Nu när du har slutfört den här kursen lär du dig mer om Notification Hubs och mallar i följande avsnitt:

* **[Använda Notification Hubs toosend senaste nytt]** <br/>Visar ett annat scenario för med hjälp av mallar
* **[Översikt över Azure Notification Hubs][Templates]**<br/>Översiktsavsnittet detaljerad mer information om mallar.

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
