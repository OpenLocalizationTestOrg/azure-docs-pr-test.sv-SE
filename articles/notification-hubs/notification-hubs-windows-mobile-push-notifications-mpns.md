---
title: "aaaSending push-meddelanden med Azure Notification Hubs på Windows Phone | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toopush meddelanden tooa Windows Phone 8 eller Windows Phone 8.1 Silverlight-program."
services: notification-hubs
documentationcenter: windows
keywords: "push-meddelande, pushmeddelande, push för windows phone"
author: ysxu
manager: erikre
editor: erikre
ms.assetid: d872d8dc-4658-4d65-9e71-fa8e34fae96e
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 1a0ad238fe7788ae2e4f47f02d113391af03dd1d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a>Skicka push-meddelanden med Azure Notification Hubs på Windows Phone
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
> [!NOTE]
> toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto. Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).
> 
> 

Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Windows Phone 8 eller Windows Phone 8.1 Silverlight-program. Om du utvecklar för Windows Phone 8.1 (utan Silverlight), referera toohello [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) version.
I den här självstudiekursen skapar du en tom Windows Phone 8-app som tar emot push-meddelanden med hjälp av hello Microsoft Push Notification Service (MPNS). När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.

> [!NOTE]
> hello Notification Hubs för Windows Phone SDK stöder inte användning av hello Windows Push Notification Service (WNS) med Windows Phone 8.1 Silverlight-appar. toouse WNS (istället för MPNS) med Windows Phone 8.1 Silverlight-appar, följ hello [Notification Hubs – självstudiekurs för Windows Phone Silverlight], som använder REST API: er.
> 
> 

hello kursen visar hello enkelt scenario för sändning med Notification Hubs.

## <a name="prerequisites"></a>Krav
Den här kursen kräver hello följande:

* [Visual Studio 2012 Express för Windows Phone], eller en senare version.

Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Windows Phone 8-appar.

## <a name="create-your-notification-hub"></a>Skapa din meddelandehubb
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Klicka på hello <b>Notification Services</b> avsnitt (inom <i>inställningar</i>), klicka på <b>Windows Phone (MPNS)</b> och klicka sedan på hello <b>aktivera ej autentiserade push </b> kryssrutan.</p>
</li>
</ol>

&emsp;&emsp;![Azure-portalen – Aktivera ej autentiserade push-meddelanden](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

Din hubb finns nu skapats och konfigurerats toosend oautentiserad meddelande för Windows Phone.

> [!NOTE]
> I den här kursen används MPNS i ej autentiserat läge. MPNS ej autentiserat läge har begränsningar för meddelanden som du kan skicka tooeach kanal. Notification Hubs stöder [MPNS i autentiserat läge](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) genom att låta dig tooupload ditt certifikat.
> 
> 

## <a name="connecting-your-app-toohello-notification-hub"></a>Ansluta din app toohello notification hub
1. Skapa en ny Windows Phone 8-app i Visual Studio.
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    I Visual Studio 2013 Update 2 eller senare kan du istället skapa en Silverlight-app för Windows Phone.
   
    ![Visual Studio – Nytt projekt – Tom app – Silverlight för Windows Phone][11]
2. Högerklicka på hello lösning i Visual Studio och klicka sedan på **hantera NuGet-paket**.
   
    Detta visar hello **hantera NuGet-paket** dialogrutan.
3. Sök efter `WindowsAzure.Messaging.Managed` och på **installera**, och acceptera hello villkor för användning.
   
    ![Visual Studio – NuGet Package Manager][20]
   
    Detta hämtar, installerar och lägger till en Azure Messaging-biblioteket på referens toohello för Windows med hjälp av hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">NuGet-paketet WindowsAzure.Messaging.Managed</a>.
4. Öppna hello filen App.xaml.cs och Lägg till följande hello `using` instruktioner:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. Lägg till hello följande kod hello överst i **Application_Launching** metod i App.xaml.cs:
   
        var channel = HttpNotificationChannel.Find("MyPushChannel");
        if (channel == null)
        {
            channel = new HttpNotificationChannel("MyPushChannel");
            channel.Open();
            channel.BindToShellToast();
        }
   
        channel.ChannelUriUpdated += new EventHandler<NotificationChannelUriEventArgs>(async (o, args) =>
        {
            var hub = new NotificationHub("<hub name>", "<connection string>");
            var result = await hub.RegisterNativeAsync(args.ChannelUri.ToString());
   
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
            {
                MessageBox.Show("Registration :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
            });
        });
   
   > [!NOTE]
   > Hej värdet **MyPushChannel** är ett index som har använt toolookup en befintlig kanal i hello [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) samling. Om det inte finns någon sådan där, skapar du en ny post med det namnet.
   > 
   > 
   
    Kontrollera att tooinsert hello namnet på anslutningssträngen NAV- och hello kallas **DefaultListenSharedAccessSignature** som du fick i hello föregående avsnitt.
    Den här koden hämtar hello URI-kanalen för hello appen från MPNS och registrerar sedan denna URI med meddelandehubben. Det garanterar även hello kanalen URI registreras i meddelandehubben varje gång hello-programmet startades.
   
   > [!NOTE]
   > Den här självstudiekursen skickar en popup-meddelande toohello enhet. När du skickar ett panelmeddelande måste du istället anropa hello **BindToShellTile** metod på hello-kanal. toosupport både popup-meddelande och panelen meddelanden, anropar du både **BindToShellTile** och **BindToShellToast**.
   > 
   > 
6. I Solution Explorer expanderar **egenskaper**öppnar hello `WMAppManifest.xml` filen, klickar på hello **funktioner** fliken och kontrollera att den hello **ID_CAP_PUSH_NOTIFICATION** är markerad.
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt toosend a push notification toohello app will fail.
7. Tryck på hello `F5` viktiga toorun hello app.
   
    Ett registreringsmeddelande visas i hello app.
8. Stäng hello app.  
   
   > [!NOTE]
   > tooreceive en push-meddelandensom hello programmet måste inte körs i förgrunden hello.
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a>Skicka push-meddelanden från serverdelen
Du kan skicka push-meddelanden med Notification Hubs från alla serverdelar via hello offentliga <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-gränssnittet</a>. I den här självstudiekursen kommer du att skicka push-meddelanden via ett .NET-konsolprogram. 

Ett exempel på hur toosend push-meddelanden från en ASP.NET-WebAPI-serverdel som är integrerad med Notification Hubs, se [Azure Notification Hubs Meddelandeanvändare med .NET-serverdel](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).  

Ett exempel på hur toosend push-meddelanden med hjälp av hello [REST API: er](https://msdn.microsoft.com/library/azure/dn223264.aspx), kolla [hur toouse Notification Hubs från Java](notification-hubs-java-push-notification-tutorial.md) och [hur toouse Notification Hubs från PHP](notification-hubs-php-push-notification-tutorial.md) .

1. Högerklicka på hello lösning, Välj **Lägg till** och **nytt projekt...** , och sedan under **Visual C#**, klickar du på **Windows** och **konsolprogram**, och klicka på **OK**.
   
       ![Visual Studio - New Project - Console Application][6]
   
    Detta lägger till en ny Visual C# konsolen programmet toohello lösning. Du kan också göra detta i en separat lösning.
2. Klicka på **Verktyg**, **Library Package Manager** och slutligen **Package Manager-konsolen**.
   
    Detta visar hello Package Manager-konsolen.
3. I hello **Pakethanterarkonsolen** fönster, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.
4. Öppna hello `Program.cs` och Lägg till följande hello `using` instruktionen:
   
        using Microsoft.Azure.NotificationHubs;
5. I hello `Program` klassen, Lägg till följande metod hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            string toast = "<?xml version=\"1.0\" encoding=\"utf-8\"?>" +
                "<wp:Notification xmlns:wp=\"WPNotification\">" +
                   "<wp:Toast>" +
                        "<wp:Text1>Hello from a .NET App!</wp:Text1>" +
                   "</wp:Toast> " +
                "</wp:Notification>";
            await hub.SendMpnsNativeNotificationAsync(toast);
        }
   
    Se till att tooreplace hello `<hub name>` med hello namnet på hello meddelandehubb som visas i hello-portalen. Dessutom måste du ersätta hello anslutning sträng platshållaren med hello anslutningssträng som heter **DefaultFullSharedAccessSignature** att du fick i hello avsnittet ”Konfigurera din meddelandehubb”.
   
   > [!NOTE]
   > Kontrollera att du använder hello-anslutningssträngen med **fullständig** åt inte **lyssna** åtkomst. hello strängen med lyssna-åtkomst har inte behörighet toosend push-meddelanden.
   > 
   > 
6. Lägg till följande rad i hello din `Main` metoden:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Med Windows Phone-emulatorn och din app stängd, Ställ in hello projektet för konsolprogrammet som hello standard Startprojekt och tryck sedan på hello `F5` viktiga toorun hello app.
   
    Du får ett push-meddelandensom popup. Om du knackar på popup-banderollen hello läser in hello app.

Du hittar alla möjliga nyttolaster för hello i hello [toast catalog] och [panelen katalog] avsnitt på MSDN.

## <a name="next-steps"></a>Nästa steg
I det här enkla exemplet skickade du push-meddelanden tooall din Windows Phone 8-enheter. 

I order tootarget specifika användare, se toohello [använda Notification Hubs toopush meddelanden toousers] kursen. 

Om du vill att toosegment användarna efter intressegrupper, kan du läsa [använda Notification Hubs toosend senaste nytt]. 

Läs mer om hur toouse Meddelandehubbar i [riktlinjer om Notification Hubs].

<!-- Images. -->
[6]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png
[7]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal.png
[8]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-from-portal2.png
[9]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal.png
[10]: ./media/notification-hubs-windows-phone-get-started/notification-hub-select-from-portal2.png
[11]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-silverlight-app.png
[12]: ./media/notification-hubs-windows-phone-get-started/notification-hub-connection-strings.png

[13]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-wp-app.png
[14]: ./media/notification-hubs-windows-phone-get-started/mobile-app-enable-push-wp8.png
[15]: ./media/notification-hubs-windows-phone-get-started/notification-hub-pushauth.png
[20]: ./media/notification-hubs-windows-phone-get-started/notification-hub-windows-universal-app-install-package.png
[213]: ./media/notification-hubs-windows-phone-get-started/notification-hub-create-console-app.png





<!-- URLs. -->
[Visual Studio 2012 Express för Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374
[riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
[använda Notification Hubs toopush meddelanden toousers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[använda Notification Hubs toosend senaste nytt]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md
[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx
[panelen katalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx
[Notification Hubs – självstudiekurs för Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp

