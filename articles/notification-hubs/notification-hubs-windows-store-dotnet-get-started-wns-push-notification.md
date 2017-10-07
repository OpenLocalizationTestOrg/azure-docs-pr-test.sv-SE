---
title: "aaaGet igång med Azure Notification Hubs för Windows Universal-plattformen appar | Microsoft Docs"
description: "I kursen får du lära dig hur toouse Azure Notification Hubs toopush meddelanden tooa program för universella Windows-plattformen."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: erikre
ms.assetid: cf307cf3-8c58-4628-9c63-8751e6a0ef43
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: 11056842d205522ed493dc61c76ecf78ebb5a363
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a>Komma igång med Notification Hubs för Windows Universal-plattformsappar
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa universella Windowsplattformen (UWP) app.

I den här självstudiekursen skapar du en tom Windows Store-app som tar emot push-meddelanden med hjälp av hello Windows Push Notification Service (WNS). När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.

## <a name="before-you-begin"></a>Innan du börjar
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

hello slutförts koden för den här kursen finns på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Krav
Den här kursen kräver hello följande:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) eller senare
* [Universal Windows App Development Tools har installerats](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Ett aktivt Azure-konto <br/>Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Ett aktivt Windows Store-konto

Du måste slutföra de här självstudierna innan du påbörjar någon annan kurs om Notification Hubs för Windows Universal-plattformsappar.

## <a name="register-your-app-for-hello-windows-store"></a>Registrera din app för hello Windows Store
toosend push-meddelanden tooUWP appar, måste du associera din app toohello Windows Store. Sedan måste du konfigurera din notification hub toointegrate med WNS.

1. Om du inte redan har registrerat appen navigerar toohello [Windows Dev Center](https://dev.windows.com/overview), logga in med ditt Microsoft-konto och klicka sedan på **skapar en ny app**.

2. Ange ett namn för appen och klicka på **Reservera appnamn**. Detta skapar en ny Windows Store-registrering för din app.

3. I Visual Studio kan du skapa ett nytt Visual C# Store Apps-projekt med hjälp av Windows Universal hello **tom App** mall och klicka på **OK**.

4. Acceptera hello standardinställningar för hello mål och minsta plattformsversioner.

5. I Solution Explorer, högerklicka på hello Windows Store-app-projekt, klickar du på **Store**, och klicka sedan på **associera appen med hello Store...** . hello **associera din App med hello Windows Store** guiden visas.

6. Logga in med ditt Microsoft-konto hello i guiden.

7. Klicka på hello-app som du registrerade i steg 2, **nästa**, och klicka sedan på **associera**. Detta lägger till hello som krävs för Windows Store-registrering information toohello programmanifestet.

8. Tillbaka på hello [Windows Dev Center](http://dev.windows.com/overview) för din nya app, klickar du på **Services**, klickar du på **Push-meddelanden**, och klicka sedan på **WNS/MPNS**.

9. Klicka på **Nytt meddelande**.

10. Klicka på mallen **Blank (Toast)** (Tom (popup)) och klicka sedan på **OK**.

11. Ange ett **meddelandenamn** och ett meddelande för **visuell kontext**. Klicka sedan på **Spara som utkast**.

12. Navigera toohello [Programregistreringsportalen](http://apps.dev.microsoft.com) och logga in.

13. Klicka på programnamnet. Anteckna hello **Programhemlighet** lösenord och hello **säkerhetsidentifierare (SID) för paketet** finns i hello **Windows Store** plattform inställningar.

     > [AZURE.WARNING]
    Hej programhemlighet och paket-SID är viktiga säkerhetsuppgifter. Lämna aldrig ut dessa uppgifter till någon och distribuera dem inte tillsammans med din app.

## <a name="configure-your-notification-hub"></a>Konfigurera meddelandehubben
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Välj hello <b>Notification Services</b> alternativet och hello <b>Windows (WNS)</b> alternativet. Ange hello <b>programhemlighet</b> lösenordet i hello <b>säkerhetsnyckeln</b> fältet. Ange din <b>paket-SID</b> värde som du fick från WNS i hello föregående avsnitt och klickar sedan på <b>spara</b>.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Din meddelandehubb har nu konfigurerat toowork med WNS och du har hello anslutning strängar tooregister din app och skicka meddelanden.

## <a name="connect-your-app-toohello-notification-hub"></a>Ansluta din app toohello notification hub
1. Högerklicka på hello lösning i Visual Studio och klicka sedan på **hantera NuGet-paket**.
   
    Detta visar hello **hantera NuGet-paket** dialogrutan.
2. Sök efter `WindowsAzure.Messaging.Managed` och på **installera**, och Godkänn hello villkor för användning.
   
    ![][20]
   
    Detta hämtar, installerar och lägger till en Azure Messaging-biblioteket på referens toohello för Windows med hjälp av hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">NuGet-paketet WindowsAzure.Messaging.Managed</a>.
3. Öppna hello projektfilen App.XAML.CS och Lägg till följande hello `using` instruktioner. 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. Även i App.xaml.cs lägger du till följande hello **InitNotificationsAsync** metoden definition toohello **App** klass:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("< your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Den här koden hämtar hello URI-kanalen för hello appen från WNS och registrerar sedan denna URI med meddelandehubben.
   
   > [!NOTE]
   > Se till att tooreplace hello platshållaren ”hubbnamn” med namnet hello hello meddelandehubb som visas i hello Azure-portalen. Ersätt även hello anslutning sträng platshållaren med hello **DefaultListenSharedAccessSignature** anslutningssträngen som du fick från hello **åtkomstprinciper** sidan i din Meddelandehubb i en föregående avsnitt.
   > 
   > 
5. Hello överst i hello **OnLaunched** händelsehanteraren i App.xaml.cs Lägg till följande anrop toohello nya hello **InitNotificationsAsync** metod:
   
        InitNotificationsAsync();
   
    Detta garanterar hello kanalen URI registreras i meddelandehubben varje gång hello-programmet startades.
6. Tryck på hello **F5** viktiga toorun hello app. En dialogruta som innehåller hello registreringsnyckel visas.

Appen är nu redo tooreceive popup-meddelanden.

## <a name="send-notifications"></a>Skicka meddelanden
Du kan snabbt testa ta emot meddelanden i appen genom att skicka meddelanden i hello [Azure Portal](https://portal.azure.com/) med hello **prova att skicka** knappen på hello meddelandehubb som visas i hello-skärmen nedan.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Push-meddelanden skickas vanligtvis i en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek. Du kan också använda hello REST API direkt toosend meddelande meddelanden om ett bibliotek inte är tillgänglig för din serverdel. 

I den här självstudiekursen kommer vi enkelhet och hur du testar klientappen genom att skicka meddelanden med hello .NET SDK för meddelandehubbar i ett konsolprogram i stället för en serverdelstjänst. Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers] självstudiekurs som hello nästa steg för att skicka meddelanden från en ASP.NET-serverdel. Hello följande metoder kan dock användas för att skicka meddelanden:

* **REST-gränssnittet**: du kan använda meddelanden på alla backend-plattformar med hello [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure Notification Hubs .NET SDK**: hello Nuget Package Manager för Visual Studio, köra [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js** : [hur toouse Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps**: ett exempel på hur toosend meddelanden från en Azure Mobile App som är integrerad med Notification Hubs finns [Lägg till push-meddelanden för Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java / PHP**: ett exempel på hur hello toosend meddelanden med hjälp av REST API: er, se ”hur toouse Notification Hubs från Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-console-app"></a>(Valfritt) Skicka meddelanden från en konsolapp
toosend meddelanden med hjälp av en .NET-konsolprogram Följ dessa steg. 

1. Högerklicka på hello lösning, Välj **Lägg till** och **nytt projekt...** , och sedan under **Visual C#**, klickar du på **Windows** och **konsolprogram**, och klicka på **OK**.
   
    Detta lägger till en ny Visual C# konsolen programmet toohello lösning. Du kan också göra detta i en separat lösning.

2. I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.
   
    Detta visar hello Package Manager-konsolen i Visual Studio.
3. Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Öppna hello Program.cs-filen och Lägg till följande hello `using` instruktionen:
   
        using Microsoft.Azure.NotificationHubs;
5. I hello **programmet** klassen, Lägg till följande metod hello:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure tooreplace hello "hub name" placeholder with hello name of hello notification hub that as it appears in hello Azure Portal. Also, replace hello connection string placeholder with hello **DefaultFullSharedAccessSignature** connection string that you obtained from hello **Access Policies** page of your Notification Hub in hello section called "Configure your notification hub."
   
   > [!NOTE]
   > Kontrollera att du använder hello anslutningssträngen som har **fullständig** åt inte **lyssna** åtkomst. hello strängen med lyssna-åtkomst har inte behörighet toosend meddelanden.
   > 
   > 
6. Lägg till följande rader i hello hello **Main** metoden:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Högerklicka på projektet för konsolprogrammet hello i Visual Studio och klicka på **Set as StartUp Project** tooset den som hello Startprojekt. Tryck på hello **F5** nyckeln toorun hello program.
   
    Du får ett popup-meddelande på alla registrerade enheter. Klicka på eller knackar hello popup-banderollen läser in hello app.

Du hittar alla nyttolaster för hello stöds i hello [toast catalog], [panelen katalog], och [badge översikt] avsnitt på MSDN.

## <a name="next-steps"></a>Nästa steg
I det här enkla exemplet skickade du broadcast-meddelanden tooall dina Windows-enheter med hjälp av hello-portalen eller ett konsolprogram. Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers] självstudiekurs som hello nästa steg. Den visar hur toosend meddelanden från en ASP.NET-backend med taggar tootarget specifika användare.

Om du vill toosegment in användarna efter intressegrupper, se [använda Notification Hubs toosend senaste nytt]. 

toolearn mer allmän information om Notification Hubs finns i [riktlinjer om Notification Hubs](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[använda Notification Hubs toopush meddelanden toousers]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[använda Notification Hubs toosend senaste nytt]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[panelen katalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge översikt]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
