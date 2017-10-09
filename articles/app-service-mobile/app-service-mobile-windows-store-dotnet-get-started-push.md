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
# <a name="add-push-notifications-tooyour-windows-app"></a><span data-ttu-id="42d51-103">Lägg till push-meddelanden tooyour Windows-app</span><span class="sxs-lookup"><span data-stu-id="42d51-103">Add push notifications tooyour Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-push](../../includes/app-service-mobile-selector-get-started-push.md)]

## <a name="overview"></a><span data-ttu-id="42d51-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="42d51-104">Overview</span></span>
<span data-ttu-id="42d51-105">I kursen får du lägga till push-meddelanden toohello [Windows Snabbstart](app-service-mobile-windows-store-dotnet-get-started.md) projekt så att ett push-meddelande skickas toohello enheten varje gång en post infogas.</span><span class="sxs-lookup"><span data-stu-id="42d51-105">In this tutorial, you add push notifications toohello [Windows quick start](app-service-mobile-windows-store-dotnet-get-started.md) project so that a push notification is sent toohello device every time a record is inserted.</span></span>

<span data-ttu-id="42d51-106">Om du inte använder hello laddat ned Snabbstart serverprojekt och du behöver hello tilläggspaket för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="42d51-106">If you do not use hello downloaded quick start server project, you will need hello push notification extension package.</span></span> <span data-ttu-id="42d51-107">Se [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="42d51-107">See [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md) for more information.</span></span>

## <span data-ttu-id="42d51-108"><a name="configure-hub"></a>Konfigurera en Meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="42d51-108"><a name="configure-hub"></a>Configure a Notification Hub</span></span>
[!INCLUDE [app-service-mobile-configure-notification-hub](../../includes/app-service-mobile-configure-notification-hub.md)]

## <a name="register-your-app-for-push-notifications"></a><span data-ttu-id="42d51-109">Registrera din app för push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="42d51-109">Register your app for push notifications</span></span>
<span data-ttu-id="42d51-110">Du behöver toosubmit din app toohello Windows Store och sedan konfigurera din server projektet toointegrate med Windows Notification Services (WNS) toosend push.</span><span class="sxs-lookup"><span data-stu-id="42d51-110">You need toosubmit your app toohello Windows Store, then configure your server project toointegrate with Windows Notification Services (WNS) toosend push.</span></span>

1. <span data-ttu-id="42d51-111">I Visual Studio Solution Explorer, högerklicka på hello UWP-app-projekt, klickar du på **Store** > **associera appen med hello Store...** .</span><span class="sxs-lookup"><span data-stu-id="42d51-111">In Visual Studio Solution Explorer, right-click hello UWP app project, click **Store** > **Associate App with hello Store...**.</span></span>

    ![Associera appen med Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/notification-hub-associate-uwp-app.png)
2. <span data-ttu-id="42d51-113">Hello i guiden klickar du på **nästa**, logga in med ditt Microsoft-konto, Skriv ett namn för din app i **reservera ett nytt namn för appen**, klicka på **reservera**.</span><span class="sxs-lookup"><span data-stu-id="42d51-113">In hello wizard, click **Next**, sign in with your Microsoft account, type a name for your app in **Reserve a new app name**, then click **Reserve**.</span></span>
3. <span data-ttu-id="42d51-114">När hello appen registreras har skapats, Välj hello nya namn, klickar du på **nästa**, och klicka sedan på **associera**.</span><span class="sxs-lookup"><span data-stu-id="42d51-114">After hello app registration is successfully created, select hello new app name, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="42d51-115">Detta lägger till hello som krävs för Windows Store-registrering information toohello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="42d51-115">This adds hello required Windows Store registration information toohello application manifest.</span></span>  
4. <span data-ttu-id="42d51-116">Navigera toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), logga in med ditt Microsoft-konto, klickar du på hello nya appregistrering i **Mina appar**, expandera **Services**  >  **Push-meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="42d51-116">Navigate toohello [Windows Dev Center](https://dev.windows.com/en-us/overview), sign-in with your Microsoft account, click hello new app registration in **My apps**, then expand **Services** > **Push notifications**.</span></span>
5. <span data-ttu-id="42d51-117">I hello **Push-meddelanden** klickar du på **webbplatsen Live-tjänster** under **Microsoft Azure Mobile Services**.</span><span class="sxs-lookup"><span data-stu-id="42d51-117">In hello **Push notifications** page, click **Live Services site** under **Microsoft Azure Mobile Services**.</span></span>
6. <span data-ttu-id="42d51-118">I hello registreringssidan anteckna värdet för hello under **programmet hemligheter** och hello **paket-SID**, som du sedan använder tooconfigure mobilappsserverdelen.</span><span class="sxs-lookup"><span data-stu-id="42d51-118">In hello registration page, make a note of hello value under **Application secrets** and hello **Package SID**, which you will next use tooconfigure your mobile app backend.</span></span>

    ![Associera appen med Windows Store](./media/app-service-mobile-windows-store-dotnet-get-started-push/app-service-mobile-uwp-app-push-auth.png)

   > [!IMPORTANT]
   > <span data-ttu-id="42d51-120">Hej klienthemligheten och paket-SID är viktiga säkerhetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="42d51-120">hello client secret and package SID are important security credentials.</span></span> <span data-ttu-id="42d51-121">Lämna aldrig ut dessa uppgifter till någon och distribuera dem inte tillsammans med din app.</span><span class="sxs-lookup"><span data-stu-id="42d51-121">Do not share these values with anyone or distribute them with your app.</span></span> <span data-ttu-id="42d51-122">Hej **program-Id** används med hello hemliga tooconfigure Account autentisering.</span><span class="sxs-lookup"><span data-stu-id="42d51-122">hello **Application Id** is used with hello secret tooconfigure Microsoft Account authentication.</span></span>
   >
   >

## <a name="configure-hello-backend-toosend-push-notifications"></a><span data-ttu-id="42d51-123">Konfigurera hello backend toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="42d51-123">Configure hello backend toosend push notifications</span></span>
[!INCLUDE [app-service-mobile-configure-wns](../../includes/app-service-mobile-configure-wns.md)]

## <span data-ttu-id="42d51-124"><a id="update-service"></a>Uppdatera hello server toosend push-meddelanden</span><span class="sxs-lookup"><span data-stu-id="42d51-124"><a id="update-service"></a>Update hello server toosend push notifications</span></span>
<span data-ttu-id="42d51-125">Anvisningarna hello nedan som matchar din serverdel projekttypen&mdash;antingen [.NET-serverdel](#dotnet) eller [Node.js-serverdel](#nodejs).</span><span class="sxs-lookup"><span data-stu-id="42d51-125">Use hello procedure below that matches your backend project type&mdash;either [.NET backend](#dotnet) or [Node.js backend](#nodejs).</span></span>

### <span data-ttu-id="42d51-126"><a name="dotnet"></a>.NET serverdelsprojektet</span><span class="sxs-lookup"><span data-stu-id="42d51-126"><a name="dotnet"></a>.NET backend project</span></span>
1. <span data-ttu-id="42d51-127">Högerklicka på hello serverprojekt i Visual Studio och klicka på **hantera NuGet-paket**, söka efter Microsoft.Azure.NotificationHubs och sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="42d51-127">In Visual Studio, right-click hello server project and click **Manage NuGet Packages**, search for Microsoft.Azure.NotificationHubs, then click **Install**.</span></span> <span data-ttu-id="42d51-128">Detta installerar hello Meddelandehubbar klientbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="42d51-128">This installs hello Notification Hubs client library.</span></span>
2. <span data-ttu-id="42d51-129">Expandera **domänkontrollanter**, öppna TodoItemController.cs och Lägg till följande hello med-uttryck:</span><span class="sxs-lookup"><span data-stu-id="42d51-129">Expand **Controllers**, open TodoItemController.cs, and add hello following using statements:</span></span>

        using System.Collections.Generic;
        using Microsoft.Azure.NotificationHubs;
        using Microsoft.Azure.Mobile.Server.Config;
3. <span data-ttu-id="42d51-130">I hello **PostTodoItem** metod, lägga till hello följande kod efter hello-anropet för**InsertAsync**:</span><span class="sxs-lookup"><span data-stu-id="42d51-130">In hello **PostTodoItem** method, add hello following code after hello call too**InsertAsync**:</span></span>

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

    <span data-ttu-id="42d51-131">Den här koden visar hello notification hub toosend ett push-meddelande när ett nytt objekt är infogning.</span><span class="sxs-lookup"><span data-stu-id="42d51-131">This code tells hello notification hub toosend a push notification after a new item is insertion.</span></span>
4. <span data-ttu-id="42d51-132">Publicera hello serverprojekt.</span><span class="sxs-lookup"><span data-stu-id="42d51-132">Republish hello server project.</span></span>

### <span data-ttu-id="42d51-133"><a name="nodejs"></a>Node.js serverdelsprojektet</span><span class="sxs-lookup"><span data-stu-id="42d51-133"><a name="nodejs"></a>Node.js backend project</span></span>
1. <span data-ttu-id="42d51-134">Om du inte redan gjort det, [hämta hello snabbstartsprojekt](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) eller annan använda hello [online redigeraren i hello Azure-portalen](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span><span class="sxs-lookup"><span data-stu-id="42d51-134">If you haven't already done so, [download hello quickstart project](app-service-mobile-node-backend-how-to-use-server-sdk.md#download-quickstart) or else use hello [online editor in hello Azure portal](app-service-mobile-node-backend-how-to-use-server-sdk.md#online-editor).</span></span>
2. <span data-ttu-id="42d51-135">Ersätt hello befintlig kod i hello todoitem.js filen med hello följande:</span><span class="sxs-lookup"><span data-stu-id="42d51-135">Replace hello existing code in hello todoitem.js file with hello following:</span></span>

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

    <span data-ttu-id="42d51-136">Detta skickar ett popup-meddelande med WNS som innehåller hello item.text när nya todo-objektet infogas.</span><span class="sxs-lookup"><span data-stu-id="42d51-136">This sends a WNS toast notification that contains hello item.text when a new todo item is inserted.</span></span>
3. <span data-ttu-id="42d51-137">Publicera om hello serverprojekt när du redigerar hello-fil på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="42d51-137">When editing hello file on your local computer, republish hello server project.</span></span>

## <span data-ttu-id="42d51-138"><a id="update-app"></a>Lägg till push-meddelanden tooyour app</span><span class="sxs-lookup"><span data-stu-id="42d51-138"><a id="update-app"></a>Add push notifications tooyour app</span></span>
<span data-ttu-id="42d51-139">Appen måste registrera för push-meddelanden på Start.</span><span class="sxs-lookup"><span data-stu-id="42d51-139">Next, your app must register for push notifications on start-up.</span></span> <span data-ttu-id="42d51-140">När du redan har aktiverat autentisering Kontrollera hello användaren loggar in innan du försöker tooregister för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="42d51-140">When you have already enabled authentication, make sure that hello user signs-in before trying tooregister for push notifications.</span></span>

1. <span data-ttu-id="42d51-141">Öppna hello **App.xaml.cs** projekt filen och Lägg till följande hello `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="42d51-141">Open hello **App.xaml.cs** project file and add hello following `using` statements:</span></span>

        using System.Threading.Tasks;
        using Windows.Networking.PushNotifications;
2. <span data-ttu-id="42d51-142">I Hej samma fil, Lägg till följande hello **InitNotificationsAsync** metoden definition toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="42d51-142">In hello same file, add hello following **InitNotificationsAsync** method definition toohello **App** class:</span></span>

        private async Task InitNotificationsAsync()
        {
            // Get a channel URI from WNS.
            var channel = await PushNotificationChannelManager
                .CreatePushNotificationChannelForApplicationAsync();

            // Register hello channel URI with Notification Hubs.
            await App.MobileService.GetPush().RegisterAsync(channel.Uri);
        }

    <span data-ttu-id="42d51-143">Den här koden hämtar hello ChannelURI för hello appen från WNS och registrerar sedan denna ChannelURI med din App Service Mobile App.</span><span class="sxs-lookup"><span data-stu-id="42d51-143">This code retrieves hello ChannelURI for hello app from WNS, and then registers that ChannelURI with your App Service Mobile App.</span></span>
3. <span data-ttu-id="42d51-144">Hello överst i hello **OnLaunched** händelsehanteraren i **App.xaml.cs**, lägga till hello **asynkrona** modifieraren toohello metoddefinition och Lägg till följande hello anropa toohello nya  **InitNotificationsAsync** metod som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="42d51-144">At hello top of hello **OnLaunched** event handler in **App.xaml.cs**, add hello **async** modifier toohello method definition and add hello following call toohello new **InitNotificationsAsync** method, as in hello following example:</span></span>

        protected async override void OnLaunched(LaunchActivatedEventArgs e)
        {
            await InitNotificationsAsync();

            // ...
        }

    <span data-ttu-id="42d51-145">Detta garanterar att hello tillfällig ChannelURI registreras varje gång hello-programmet startades.</span><span class="sxs-lookup"><span data-stu-id="42d51-145">This guarantees that hello short-lived ChannelURI is registered each time hello application is launched.</span></span>
4. <span data-ttu-id="42d51-146">Återskapa projektet UWP-app.</span><span class="sxs-lookup"><span data-stu-id="42d51-146">Rebuild your UWP app project.</span></span> <span data-ttu-id="42d51-147">Appen är nu redo tooreceive popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="42d51-147">Your app is now ready tooreceive toast notifications.</span></span>

## <span data-ttu-id="42d51-148"><a id="test"></a>Testa push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="42d51-148"><a id="test"></a>Test push notifications in your app</span></span>
[!INCLUDE [app-service-mobile-windows-universal-test-push](../../includes/app-service-mobile-windows-universal-test-push.md)]

## <span data-ttu-id="42d51-149"><a id="more"></a>Nästa steg</span><span class="sxs-lookup"><span data-stu-id="42d51-149"><a id="more"></a>Next steps</span></span>
<span data-ttu-id="42d51-150">Mer information om push-meddelanden:</span><span class="sxs-lookup"><span data-stu-id="42d51-150">Learn more about push notifications:</span></span>

* [<span data-ttu-id="42d51-151">Hur toouse hello hanterade klienten för Azure Mobile Apps</span><span class="sxs-lookup"><span data-stu-id="42d51-151">How toouse hello managed client for Azure Mobile Apps</span></span>](app-service-mobile-dotnet-how-to-use-client-library.md#pushnotifications)  
  <span data-ttu-id="42d51-152">Mallar ger dig flexibilitet toosend plattformsoberoende push-meddelanden och lokaliserade push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="42d51-152">Templates give you flexibility toosend cross-platform pushes and localized pushes.</span></span> <span data-ttu-id="42d51-153">Lär dig hur tooregister mallar.</span><span class="sxs-lookup"><span data-stu-id="42d51-153">Learn how tooregister templates.</span></span>
* [<span data-ttu-id="42d51-154">Diagnostisera problem för push-meddelande</span><span class="sxs-lookup"><span data-stu-id="42d51-154">Diagnose push notification issues</span></span>](../notification-hubs/notification-hubs-push-notification-fixer.md)  
  <span data-ttu-id="42d51-155">Det finns olika orsaker till varför meddelanden kan hämta bort eller inte hamnar på enheter.</span><span class="sxs-lookup"><span data-stu-id="42d51-155">There are various reasons why notifications may get dropped or do not end up on devices.</span></span> <span data-ttu-id="42d51-156">Det här avsnittet visas hur tooanalyze och ta reda på hello rot orsakar fel för push-meddelande.</span><span class="sxs-lookup"><span data-stu-id="42d51-156">This topic shows you how tooanalyze and figure out hello root cause of push notification failures.</span></span>

<span data-ttu-id="42d51-157">Tänk fortsätter tooone av hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="42d51-157">Consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="42d51-158">Lägg till autentisering tooyour app</span><span class="sxs-lookup"><span data-stu-id="42d51-158">Add authentication tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-users.md)  
  <span data-ttu-id="42d51-159">Lär dig hur tooauthenticate användare i appen med en identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="42d51-159">Learn how tooauthenticate users of your app with an identity provider.</span></span>
* [<span data-ttu-id="42d51-160">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="42d51-160">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="42d51-161">Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="42d51-161">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="42d51-162">Offlinesynkronisering kan slutanvändarna toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="42d51-162">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- Anchors. -->

<!-- URLs. -->
[Azure Portal]: https://portal.azure.com/

<!-- Images. -->
