---
title: "Komma igång med Azure Notification Hubs för Windows Universal-plattformsappar | Microsoft Docs"
description: "I de här självstudierna kommer du att få lära dig hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Windows Universal-plattformsapp."
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
ms.openlocfilehash: 9b50f1cca81348b69f7ff2d702c6c72871afe0a0
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a>Komma igång med Notification Hubs för Windows Universal-plattformsappar
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a>Översikt
I de här självstudierna beskrivs hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Windows Universal-plattformsapp.

I denna självstudiekurs skapar du en tom Windows Store-app som tar emot push-meddelanden med hjälp av Windows Push Notification Service (WNS). När du är klar kan du använda meddelandehubben för att sända push-meddelanden till alla enheter som kör appen.

## <a name="before-you-begin"></a>Innan du börjar
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

Den färdiga koden för den här självstudiekursen hittar du på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).

## <a name="prerequisites"></a>Krav
För den här kursen behöver du följande:

* [Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) eller senare
* [Universal Windows App Development Tools har installerats](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* Ett aktivt Azure-konto <br/>Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).
* Ett aktivt Windows Store-konto

Du måste slutföra de här självstudierna innan du påbörjar någon annan kurs om Notification Hubs för Windows Universal-plattformsappar.

## <a name="register-your-app-for-the-windows-store"></a>Registrera din app för Windows Store
Om du vill skicka push-meddelanden till UWP-appar, måste du associera din app med Windows Store. Sedan måste du konfigurera meddelandehubben för att integrera den med WNS.

1. Om du inte redan har registrerat appen navigerar du till [Windows Dev Center](https://dev.windows.com/overview), loggar in med ditt Microsoft-konto och klickar sedan på **Skapa en ny app**.

2. Ange ett namn för appen och klicka på **Reservera appnamn**. Detta skapar en ny Windows Store-registrering för din app.

3. Skapa ett nytt Visual C# Store Apps-projekt i Visual Studio med hjälp av Windows Universal-mallen **Tom app** och klicka sedan på **OK**.

4. Acceptera standardinställningarna för mål- och minsta plattformsversioner.

5. I Solution Explorer högerklickar du på approjektet för Windows Store och sedan klickar du på **Store**. Slutligen klickar du på **Associera app med Store...**. Guiden **Associera din app med Windows Store** visas.

6. I guiden loggar du in med ditt Microsoft-konto.

7. Klicka på den app som du registrerade i steg 2, sedan på **Nästa** och slutligen på **Associera**. Detta lägger till den registreringsinformation som krävs för Windows Store i programmanifestet.

8. Tillbaka på sidan [Windows Dev Center](http://dev.windows.com/overview) för din nya app klickar du på **Tjänster**, klickar på **Push-meddelanden** och sedan på **WNS/MPNS**.

9. Klicka på **Nytt meddelande**.

10. Klicka på mallen **Blank (Toast)** (Tom (popup)) och klicka sedan på **OK**.

11. Ange ett **meddelandenamn** och ett meddelande för **visuell kontext**. Klicka sedan på **Spara som utkast**.

12. Gå till [programregistreringsportalen](http://apps.dev.microsoft.com) och logga in.

13. Klicka på programnamnet. Skriv ner lösenordet för **apphemligheten** och **paketsäkerhets-ID:t (SID)** som du hittar i plattformsinställningarna för **Windows Store**.

     > [AZURE.WARNING]
    Programhemligheten och paket-SID:et är viktiga säkerhetsuppgifter. Lämna aldrig ut dessa uppgifter till någon och distribuera dem inte tillsammans med din app.

## <a name="configure-your-notification-hub"></a>Konfigurera meddelandehubben
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p>Välj alternativet <b>Notification Services</b> och <b>Windows (WNS)</b>. Ange lösenordet för <b>Apphemlighet</b> i fältet <b>Säkerhetsnyckel</b>. Ange ditt <b>Paket-SID</b>-värde som du fick från WNS i föregående avsnitt och klicka sedan på <b>Spara</b>.</p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

Din meddelandehubb har nu konfigurerats för att fungera med WNS och du har anslutningssträngar för att registrera din app och skicka meddelanden.

## <a name="connect-your-app-to-the-notification-hub"></a>Anslut appen till meddelandehubben
1. Högerklicka på lösningen i Vision Studio och klicka sedan på **Hantera NuGet-paket**.
   
    Dialogrutan **Hantera NuGet-paket** öppnas.
2. Leta rätt på `WindowsAzure.Messaging.Managed` och klicka på **Installera**. Godkänn sedan användningsvillkoren.
   
    ![][20]
   
    Den här åtgärden hämtar, installerar och lägger till en referens i Azure Messaging-biblioteket för Windows med hjälp av <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">NuGet-paketet WindowsAzure.Messaging.Managed</a>.
3. Öppna projektfilen App.xaml.cs och lägg till följande `using`-uttryck: 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. I filen App.xaml.cs ska du även lägga till följande **InitNotificationsAsync**-metoddefinitionen i **app**-klassen:
   
        private async void InitNotificationsAsync()
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            var hub = new NotificationHub("< your hub name>", "<Your DefaultListenSharedAccessSignature connection string>");
            var result = await hub.RegisterNativeAsync(channel.Uri);
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
   
        }
   
    Den här koden hämtar URI-kanalen för appen från WNS och registrerar sedan denna med din meddelandehubb.
   
   > [!NOTE]
   > Ersätt platshållaren ”ditt hubbnamn” med namnet på den meddelandehubb som visas i Azure Portal. Byt även ut platshållaren för anslutningssträngen mot anslutningssträngen **DefaultListenSharedAccessSignature** som du fick från sidan **Åtkomstpoliyer** i ett föregående avsnittet.
   > 
   > 
5. Längst upp i händelsehanteraren **OnLaunched** i App.xaml.cs lägger du till följande anrop till den nya **InitNotificationsAsync**-metoden:
   
        InitNotificationsAsync();
   
    Detta garanterar även att URI-kanalen registreras i meddelandehubben varje gång appen startas.
6. Kör appen genom att trycka på tangenten **F5**. En dialogruta som innehåller registreringsnyckeln visas.

Appen är nu redo att ta emot popup-meddelanden.

## <a name="send-notifications"></a>Skicka meddelanden
Du kan snabbt testa att ta emot meddelanden i appen genom att skicka meddelanden [Azure Portal](https://portal.azure.com/) med knappen **Testsänd** i meddelandehubben, så som visas på skärmen nedan.

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

Push-meddelanden skickas vanligtvis i en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek. Du kan också använda REST-API:et direkt för att skicka meddelanden om ett bibliotek inte är tillgängligt för din serverdel. 

I den här enkla självstudiekursen visar vi hur du testar klientappen genom att skicka meddelanden med .NET SDK för meddelandehubbar i ett konsolprogram i stället för med en serverdelstjänst. Vi rekommenderar att du går vidare med självstudiekursen [Använda Notification Hubs för att skicka push-meddelanden till användare] som nästa steg i att skicka meddelanden från en ASP.NET-serverdel. Följande åtgärder kan dock användas för att skicka meddelanden:

* **REST-gränssnitt**: Du kan använda meddelanden på alla serverdelsplattformar med [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).
* **Microsoft Azure Notification Hubs .NET SDK**: I Nuget Package Manager för Visual Studio kör du [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).
* **Node.js**: [Använda Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).
* **Azure Mobile Apps**: För ett exempel på hur man skickar meddelanden från en Azure Mobile App som är integrerad med Notification Hubs, kan du gå till [Lägg till Push-meddelanden för Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).
* **Java/PHP**: Ett exempel på hur du skickar meddelanden med hjälp av REST-API finns i avsnittet Använda Notification Hubs från Java/PHP ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).

## <a name="optional-send-notifications-from-a-console-app"></a>(Valfritt) Skicka meddelanden från en konsolapp
Följ anvisningarna nedan om du vill skicka meddelanden med hjälp av ett .NET-konsolprogram. 

1. Högerklicka på lösningen, välj **Lägg till** och **Nytt projekt...** och klicka sedan på **Windows** och **Konsolprogram** under **Visual C#**. Slutligen klickar du på **OK**.
   
    Detta lägger till ett nytt konsolprogram för Visual C# i lösningen. Du kan också göra detta i en separat lösning.

2. I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.
   
    Då visas Package Manager-konsolen i Visual Studio.
3. I fönstret Package Manager-konsol ställer du in **standardprojektet** till det nya projektet för konsolprogrammet. Sedan kör du följande kommando i konsolfönstret:
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    Då läggs en referens till i Azure Notification Hubs SDK med hjälp av <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs-NuGet-paketet</a>.
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. Öppna filen Program.cs och lägg till följande `using`-uttryck:
   
        using Microsoft.Azure.NotificationHubs;
5. Lägg till följande metod i **Program**-klassen:
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure to replace the "hub name" placeholder with the name of the notification hub that as it appears in the Azure Portal. Also, replace the connection string placeholder with the **DefaultFullSharedAccessSignature** connection string that you obtained from the **Access Policies** page of your Notification Hub in the section called "Configure your notification hub."
   
   > [!NOTE]
   > Kontrollera att du använder anslutningssträngen som har **fullständig** åtkomst, inte enbart åtkomst för att **lyssna**. Strängen med lyssna-åtkomst har inte behörighet att skicka meddelanden.
   > 
   > 
6. Lägg till följande rader i **Main**-metoden:
   
         SendNotificationAsync();
         Console.ReadLine();
7. Högerklicka på konsolprogramprojektet i Visual Studio och klicka på **Konfigurera som startprojekt** för att ställa in det som startprojekt. Tryck på tangenten **F5** för att köra appen.
   
    Du får ett popup-meddelande på alla registrerade enheter. Appen läses in när du klickar eller knackar på popup-banderollen.

Du hittar alla nyttolaster som stöds under ämnena [toast catalog] (katalog över popup-meddelanden), [tile catalog] (katalog över paneler) och [badge overview] (översikt över aktivitetsikoner) på MSDN.

## <a name="next-steps"></a>Nästa steg
I det här enkla exemplet skickar du broadcast-meddelanden till alla dina Windows-enheter med hjälp av portalen eller ett konsolprogram. Vi rekommenderar att du går vidare med självstudiekursen [Använda Notification Hubs för att skicka push-meddelanden till användare] som nästa steg. Den visar hur du skickar meddelanden från en ASP.NET-backend med hjälp av taggar för att rikta in dig på vissa specifika användare.

Om du vill dela in användarna efter intressegrupper, kan du gå till [Använda Notification Hubs för att skicka de senaste nyheterna]. 

Om du vill få mer allmän information om Notification Hubs kan du läsa [Riktlinjer om Notification Hubs](notification-hubs-push-notification-overview.md).

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

[Använda Notification Hubs för att skicka push-meddelanden till användare]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md
[Använda Notification Hubs för att skicka de senaste nyheterna]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md

[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx
[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx
[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx
