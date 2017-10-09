---
title: aaaAzure Notification Hubs Secure Push
description: "Lär dig hur säker toosend push-meddelanden i Azure. Kodexempel som skrivits i C# med hjälp av hello .NET-API."
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: b6fe16c96d28d75ff698278409fb012472ba6396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a>Azure Notification Hubs säker Push
> [!div class="op_single_selector"]
> * [Windows Universal](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [iOS](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [Android](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a>Översikt
Stöd för push-meddelanden i Microsoft Azure kan du tooaccess en lätt att använda flera plattformar, skalats ut push-infrastruktur som förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar.

På grund av tooregulatory eller säkerhet begränsningar kanske ibland ett program tooinclude någonting i hello-meddelande som kan överföras via hello standard push-infrastruktur. Den här självstudiekursen beskrivs hur tooachieve hello samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan hello klientenheter och hello-appserverdelen.

På en hög nivå är hello flödet följande:

1. hello appens serverdel:
   * Lagrar säker nyttolast i backend-databas.
   * Skickar hello-ID för meddelande toohello enheten (ingen säker information skickas).
2. hello appen på hello enhet, när du tar emot hello-meddelande:
   * hello enheten kontaktar hello backend-begärande hello säker nyttolast.
   * hello app kan du visa hello-nyttolast som ett meddelande på hello enhet.

Det är viktigt toonote att vi antar hello enheten i hello föregående flöde (och i den här självstudiekursen) lagrar en autentiseringstoken i lokal lagring när hello användare loggar in. Detta garanterar en helt integrerad upplevelse som hello enhet kan hämta hello-meddelande säker nyttolast med denna token. Om ditt program inte kan lagra autentiseringstoken på hello enhet eller om dessa token kan ha gått, ska hello enhetsapp vid mottagning av hello-meddelande visa ett allmänt meddelande fråga hello användaren toolaunch hello app. hello app sedan autentiserar hello användare och visar hello notification nyttolast.

Den här säkra Push-kursen visar hur toosend ett push-meddelande på ett säkert sätt. hello kursen bygger på hello [meddela användare](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) självstudier, så bör du genomföra hello stegen i den här självstudiekursen först.

> [!NOTE]
> Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).
> Observera också att Windows Phone 8.1 kräver autentiseringsuppgifter för Windows (inte Windows Phone) och bakgrundsaktiviteter inte fungerar på Windows Phone 8.0 eller Silverlight 8.1. För Windows Store-program, du kan ta emot meddelanden via en bakgrundsaktivitet endast om hello app är aktiverad för låsskärm (klicka på hello kryssrutan i hello Appmanifest).
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-windows-phone-project"></a>Ändra hello Windows Phone-projekt
1. I hello **NotifyUserWindowsPhone** projekt, Lägg till följande kod tooApp.xaml.cs tooregister hello push bakgrundsaktivitet hello. Lägg till följande kodrad i slutet av hello av hello hello `OnLaunched()` metoden:
   
        RegisterBackgroundTask();
2. Lägg till följande kod direkt efter hello hello fortfarande i App.xaml.cs `OnLaunched()` metoden:
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. Lägg till följande hello `using` instruktioner överst hello i hello App.xaml.cs fil:
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. Från hello **filen** -menyn i Visual Studio klickar du på **spara alla**.

## <a name="create-hello-push-background-component"></a>Skapa hello Push bakgrund komponent
hello nästa steg är toocreate hello push bakgrund komponent.

1. I Solution Explorer högerklickar du på hello översta noden hello-lösning (**lösning SecurePush** i det här fallet), klicka på **Lägg till**, klicka på **nytt projekt**.
2. Expandera **Store-appar**, klicka på **Windows Phone appar**, klicka på **Windows Runtime-komponent (Windows Phone)**. Namnet hello projektet **PushBackgroundComponent**, och klicka sedan på **OK** toocreate hello projektet.
   
    ![][12]
3. I Solution Explorer högerklickar du på hello **PushBackgroundComponent (Windows Phone 8.1)** projektet och klicka sedan på **Lägg till**, klicka på **klassen**. Namnge hello nya klassen **PushBackgroundTask.cs**. Klicka på **Lägg till** toogenerate hello-klassen.
4. Ersätt hello hela innehållet i hello **PushBackgroundComponent** definitionen för namnområdet med hello följande kod, ersätter hello platshållare `{back-end endpoint}` med hello backend-slutpunkt som erhålls vid distribution av din backend:
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store hello content received from hello notification so it can be retrieved from hello UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. I Solution Explorer högerklickar du på hello **PushBackgroundComponent (Windows Phone 8.1)** projektet och klicka sedan på **hantera NuGet-paket**.
6. Hello vänster, klickar du på **Online**.
7. I hello **Sök** skriver **http-klienten**.
8. I listan över sökresultat hello klickar du på **klientbibliotek för Microsoft HTTP**, och klicka sedan på **installera**. Slutför hello-installationen.
9. Tillbaka i hello NuGet **Sök** skriver **Json.net**. Installera hello **Json.NET** paketet och sedan Stäng hello NuGet Package Manager-fönstret.
10. Lägg till följande hello `using` instruktioner överst hello i hello **PushBackgroundTask.cs** fil:
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. I Solution Explorer i hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt, högerklicka på **referenser**, klicka på **Lägg till referens...** . I dialogrutan för Reference Manager hello kryssrutan hello bredvid för**PushBackgroundComponent**, och klicka sedan på **OK**.
12. I Solution Explorer dubbelklickar du på **Package.appxmanifest** i hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt. Under **meddelanden**, ange **popup-kompatibel** för**Ja**.
    
    ![][3]
13. Fortfarande i **Package.appxmanifest**, klicka på hello **deklarationer** menyn hello övre delen. I hello **tillgängliga deklarationer** listrutan, klickar du på **bakgrundsaktiviteter**, och klicka sedan på **Lägg till**.
14. I **Package.appxmanifest**under **egenskaper**, kontrollera **Push notification**.
15. I **Package.appxmanifest**under **Appinställningar**, typen **PushBackgroundComponent.PushBackgroundTask** i hello **startpunkten** fält.
    
    ![][13]
16. Från hello **filen** -menyn klickar du på **spara alla**.

## <a name="run-hello-application"></a>Kör hello program
toorun Hej program, hello följande:

1. I Visual Studio kör hello **AppBackend** Web API-program. En ASP.NET-webbsida visas.
2. I Visual Studio kör hello **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-app. hello Windows Phone-emulatorn kör och läser in hello appen automatiskt.
3. I hello **NotifyUserWindowsPhone** app UI, ange ett användarnamn och lösenord. Dessa kan vara valfri sträng, men de måste vara hello samma värde.
4. I hello **NotifyUserWindowsPhone** app-Gränssnittet klickar du på **logga in och registrera**. Klicka på **skicka push**.

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
