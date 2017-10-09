---
title: aaaAdd push-meddelanden tooyour universella Windowsplattformen (UWP) app | Microsoft Docs
description: "Lär dig hur toouse Azure App Service Mobile Apps och Azure Notification Hubs toosend push-meddelanden tooyour universella Windowsplattformen (UWP) app."
services: app-service\mobile,notification-hubs
documentationcenter: windows
author: ysxu
manager: syntaxc4
editor: 
ms.assetid: 6de1b9d4-bd28-43e4-8db4-94cd3b187aa3
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/12/2016
ms.author: yuaxu
ms.openlocfilehash: 378ce59cab974830c0a3801108b24b30a21ae5cc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-push-notifications-tooyour-windows-app"></a>Lägg till push-meddelanden tooyour Windows-app
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a>Översikt
I kursen får du lägga till push-meddelanden toohello [Windows Snabbstart](app-service-mobile-windows-store-dotnet-get-started.md) projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.

Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande. Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.

## <a name="configure-hub"></a>Konfigurera en Meddelandehubb
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a>Registrera din app för push-meddelanden
Du behöver toosubmit din app toohello Windows Store och sedan konfigurera din server projektet toointegrate med Windows Notification Services (WNS) toosend push.

1. I Visual Studio Solution Explorer, högerklicka på hello UWP-app-projekt, klickar du på **Store** > **associera appen med hello Store...** .

    ![Associera appen med Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. Hello i guiden klickar du på **nästa**, logga in med ditt Microsoft-konto, Skriv ett namn för din app i **reservera ett nytt namn för appen**, klicka på **reservera**.
3. När hello appen registreras har skapats, Välj hello nya namn, klickar du på **nästa**, och klicka sedan på **associera**. Detta lägger till hello som krävs för Windows Store-registrering information toohello programmanifestet.  
4. Navigera toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), logga in med ditt Microsoft-konto, klickar du på hello nya appregistrering i **Mina appar**, expandera **Services**  >  **Push-meddelanden**.
5. I hello **Push-meddelanden** klickar du på **webbplatsen Live-tjänster** under **Microsoft Azure Mobile Services**.
6. I hello registreringssidan anteckna värdet för hello under **programmet hemligheter** och hello **paket-SID**, som du sedan använder tooconfigure mobilappsserverdelen.

    ![Associera appen med Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > Hej klienthemligheten och paket-SID är viktiga säkerhetsuppgifter. Lämna aldrig ut dessa uppgifter till någon och distribuera dem inte tillsammans med din app. Hej **program-Id** används med hello hemliga tooconfigure Account autentisering.
   >
   >

## <a name="configure-hello-backend-toosend-push-notifications"></a>Konfigurera hello backend toosend push-meddelanden
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <a id="update-service"></a>Uppdatera hello server toosend push-meddelanden
Anvisningarna hello nedan som matchar din serverdel projekttypen&mdash;antingen [.NET-serverdel](#dotnet) eller [Node.js-serverdel](#nodejs).

### <a name="dotnet"></a>.NET serverdelsprojektet
1. Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**, söka efter Microsoft.Azure.NotificationHubs och sedan på **installera**. Detta installerar hello Meddelandehubbar klientbiblioteket.
2. Expandera **domänkontrollanter**, öppna TodoItemController.cs och Lägg till följande hello med-uttryck:

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. I hello **PostTodoItem** metod, lägga till hello följande kod efter hello-anropet för**InsertAsync**:

        // Get hello settings for hello server project.
        HttpConfiguration config = this.Configuration;
        MobileAppSettingsDictionary settings =
            this.Configuration.GetMobileAppSettingsProvider().GetMobileAppSettings();

        // Get hello Notification Hubs credentials for hello Mobile App.
        string notificationHubName = settings.NotificationHubName;
        string notificationHubConnection = settings
            .Connections[MobileAppSettingsKeys.NotificationHubConnectionString].ConnectionString;

        // Create hello notification hub client.
        NotificationHubClient hub = NotificationHubClient
            .CreateClientFromConnectionString(notificationHubConnection, notificationHubName);

        // Define a WNS payload
        var windowsToastPayload = @"<toast><visual><binding template=""ToastText01""><text id=""1"">"
                                + item.Text + @"</text></binding></visual></toast>";
        try
        {
            // Send hello push notification.
            var result = await hub.SendWindowsNativeNotificationAsync(windowsToastPayload);

            // Write hello success result toohello logs.
            config.Services.GetTraceWriter().Info(result.State.ToString());
        }
        catch (System.Exception ex)
        {
            // Write hello failure result toohello logs.
            config.Services.GetTraceWriter()
                .Error(ex.Message, null, "Push.SendAsync Error");
        }

    Den här koden visar hello notification hub toosend ett push-meddelande när ett nytt objekt är infogning.
4. Publicera hello serverprojekt.

### <a name="nodejs"></a>Node.js serverdelsprojektet
1. Om du inte redan gjort det, [hämta hello snabbstartsprojekt](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) eller annan använda hello [online redigeraren i hello Azure-portalen](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).
2. Ersätt hello befintlig kod i hello todoitem.js filen med hello följande:

        var azureMobileApps = require('azure-mobile-apps'),
        promises = require('azure-mobile-apps/src/utilities/promises'),
        logger = require('azure-mobile-apps/src/logger');

        var table = azureMobileApps.table();

        table.insert(function (context) {
        // For more information about hello Notification Hubs JavaScript SDK,
        // see http://aka.ms/nodejshubs
        logger.info('Running TodoItem.insert');

        // Define hello WNS payload that contains hello new item Text.
        var payload = "<toast><visual><binding template=\ToastText01\><text id=\"1\">"
                                    + context.item.text + "</text></binding></visual></toast>";

        // Execute hello insert.  hello insert returns hello results as a Promise,
        // Do hello push as a post-execute action within hello promise flow.
        return context.execute()
            .then(function (results) {
                // Only do hello push if configured
                if (context.push) {
                    // Send a WNS native toast notification.
                    context.push.wns.sendToast(null, payload, function (error) {
                        if (error) {
                            logger.error('Error while sending push notification: ', error);
                        } else {
                            logger.info('Push notification sent successfully!');
                        }
                    });
                }
                // Don't forget tooreturn hello results from hello context.execute()
                return results;
            })
            .catch(function (error) {
                logger.error('Error while running context.execute: ', error);
            });
        });

        module.exports = table;

    Detta skickar ett popup-meddelande med WNS som innehåller hello item.text när nya todo-objektet infogas.
3. Publicera om hello serverprojekt när du redigerar hello-fil på den lokala datorn.

## <a id="update-app"></a>Lägg till push-meddelanden tooyour app
Appen måste registrera för push-meddelanden på Start. När du redan har aktiverat autentisering Kontrollera hello användaren loggar in innan du försöker tooregister för push-meddelanden.

1. Öppna hello **App.xaml.cs** projekt filen och Lägg till följande hello `using` instruktioner:

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. I Hej samma fil, Lägg till följande hello **InitNotificationsAsync** metoden definition toohello **App** klass:

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register hello channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    Den här koden hämtar hello ChannelURI för hello appen från WNS och registrerar sedan denna ChannelURI med din App Service Mobile App.
3. Hello överst i hello **OnLaunched** händelsehanteraren i **App.xaml.cs**, lägga till hello **asynkrona** modifieraren toohello metoddefinition och Lägg till följande hello anropa toohello nya  **InitNotificationsAsync** metod som i följande exempel hello:

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    Detta garanterar att hello tillfällig ChannelURI registreras varje gång hello-programmet startades.
4. Återskapa projektet UWP-app. Appen är nu redo tooreceive popup-meddelanden.

## <a id="test"></a>Testa push-meddelanden i appen
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <a id="more"></a>Nästa steg
Mer information om push-meddelanden:

* [Hur toouse hello hanterade klienten för Azure Mobile Apps](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  Mallar ger dig flexibilitet toosend plattformsoberoende push-meddelanden och lokaliserade push-meddelanden. Lär dig hur tooregister mallar.
* [Diagnostisera problem för push-meddelande](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  Det finns olika orsaker till varför meddelanden kan hämta bort eller inte hamnar på enheter. Det här avsnittet visas hur tooanalyze och ta reda på hello rot orsakar fel för push-meddelande.

Tänk fortsätter tooone av hello följande kurser:

* [Lägg till autentisering tooyour app](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  Lär dig hur tooauthenticate användare i appen med en identitetsprovider.
* [Aktivera offlinesynkronisering av appen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel. Offlinesynkronisering kan slutanvändarna toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
