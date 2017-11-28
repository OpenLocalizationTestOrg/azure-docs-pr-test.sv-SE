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
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a><span data-ttu-id="7e2a4-103">Komma igång med Notification Hubs för Windows Universal-plattformsappar</span><span class="sxs-lookup"><span data-stu-id="7e2a4-103">Getting started with Notification Hubs for Windows Universal Platform Apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="7e2a4-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="7e2a4-104">Overview</span></span>
<span data-ttu-id="7e2a4-105">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa universella Windowsplattformen (UWP) app.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-105">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Universal Windows Platform (UWP) app.</span></span>

<span data-ttu-id="7e2a4-106">I den här självstudiekursen skapar du en tom Windows Store-app som tar emot push-meddelanden med hjälp av hello Windows Push Notification Service (WNS).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-106">In this tutorial, you create a blank Windows Store app that receives push notifications by using hello Windows Push Notification Service (WNS).</span></span> <span data-ttu-id="7e2a4-107">När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-107">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="7e2a4-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7e2a4-108">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="7e2a4-109">hello slutförts koden för den här kursen finns på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-109">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7e2a4-110">Krav</span><span class="sxs-lookup"><span data-stu-id="7e2a4-110">Prerequisites</span></span>
<span data-ttu-id="7e2a4-111">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-111">This tutorial requires hello following:</span></span>

* <span data-ttu-id="7e2a4-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) eller senare</span><span class="sxs-lookup"><span data-stu-id="7e2a4-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) or later</span></span>
* [<span data-ttu-id="7e2a4-113">Universal Windows App Development Tools har installerats</span><span class="sxs-lookup"><span data-stu-id="7e2a4-113">Universal Windows App Development Tools installed</span></span>](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* <span data-ttu-id="7e2a4-114">Ett aktivt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="7e2a4-114">An active Azure account</span></span> <br/><span data-ttu-id="7e2a4-115">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="7e2a4-116">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span></span>
* <span data-ttu-id="7e2a4-117">Ett aktivt Windows Store-konto</span><span class="sxs-lookup"><span data-stu-id="7e2a4-117">An active Windows Store account</span></span>

<span data-ttu-id="7e2a4-118">Du måste slutföra de här självstudierna innan du påbörjar någon annan kurs om Notification Hubs för Windows Universal-plattformsappar.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Universal Platform apps.</span></span>

## <a name="register-your-app-for-hello-windows-store"></a><span data-ttu-id="7e2a4-119">Registrera din app för hello Windows Store</span><span class="sxs-lookup"><span data-stu-id="7e2a4-119">Register your app for hello Windows Store</span></span>
<span data-ttu-id="7e2a4-120">toosend push-meddelanden tooUWP appar, måste du associera din app toohello Windows Store.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-120">toosend push notifications tooUWP apps, you must associate your app toohello Windows Store.</span></span> <span data-ttu-id="7e2a4-121">Sedan måste du konfigurera din notification hub toointegrate med WNS.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-121">You must then configure your notification hub toointegrate with WNS.</span></span>

1. <span data-ttu-id="7e2a4-122">Om du inte redan har registrerat appen navigerar toohello [Windows Dev Center](https://dev.windows.com/overview), logga in med ditt Microsoft-konto och klicka sedan på **skapar en ny app**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-122">If you have not already registered your app, navigate toohello [Windows Dev Center](https://dev.windows.com/overview), sign in with your Microsoft account, and then click **Create a new app**.</span></span>

2. <span data-ttu-id="7e2a4-123">Ange ett namn för appen och klicka på **Reservera appnamn**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-123">Type a name for your app and click **Reserve app name**.</span></span> <span data-ttu-id="7e2a4-124">Detta skapar en ny Windows Store-registrering för din app.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-124">This creates a new Windows Store registration for your app.</span></span>

3. <span data-ttu-id="7e2a4-125">I Visual Studio kan du skapa ett nytt Visual C# Store Apps-projekt med hjälp av Windows Universal hello **tom App** mall och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-125">In Visual Studio, create a new Visual C# Store Apps project by using hello Windows Universal **Blank App** template and click **OK**.</span></span>

4. <span data-ttu-id="7e2a4-126">Acceptera hello standardinställningar för hello mål och minsta plattformsversioner.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-126">Accept hello defaults for hello target and minimum platform versions.</span></span>

5. <span data-ttu-id="7e2a4-127">I Solution Explorer, högerklicka på hello Windows Store-app-projekt, klickar du på **Store**, och klicka sedan på **associera appen med hello Store...** . hello **associera din App med hello Windows Store** guiden visas.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-127">In Solution Explorer, right-click hello Windows Store app project, click **Store**, and then click **Associate App with hello Store...**. hello **Associate Your App with hello Windows Store** wizard appears.</span></span>

6. <span data-ttu-id="7e2a4-128">Logga in med ditt Microsoft-konto hello i guiden.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-128">In hello wizard, sign in with your Microsoft account.</span></span>

7. <span data-ttu-id="7e2a4-129">Klicka på hello-app som du registrerade i steg 2, **nästa**, och klicka sedan på **associera**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-129">Click hello app that you registered in step 2, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="7e2a4-130">Detta lägger till hello som krävs för Windows Store-registrering information toohello programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-130">This adds hello required Windows Store registration information toohello application manifest.</span></span>

8. <span data-ttu-id="7e2a4-131">Tillbaka på hello [Windows Dev Center](http://dev.windows.com/overview) för din nya app, klickar du på **Services**, klickar du på **Push-meddelanden**, och klicka sedan på **WNS/MPNS**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-131">Back on hello [Windows Dev Center](http://dev.windows.com/overview) page for your new app, click **Services**, click **Push notifications**, and then click **WNS/MPNS**.</span></span>

9. <span data-ttu-id="7e2a4-132">Klicka på **Nytt meddelande**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-132">Click **New Notification**.</span></span>

10. <span data-ttu-id="7e2a4-133">Klicka på mallen **Blank (Toast)** (Tom (popup)) och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-133">Click **Blank (Toast)** template and then click **OK**.</span></span>

11. <span data-ttu-id="7e2a4-134">Ange ett **meddelandenamn** och ett meddelande för **visuell kontext**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-134">Enter a notification **Name** and Visual **Context** message.</span></span> <span data-ttu-id="7e2a4-135">Klicka sedan på **Spara som utkast**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-135">Then click **Save as draft**.</span></span>

12. <span data-ttu-id="7e2a4-136">Navigera toohello [Programregistreringsportalen](http://apps.dev.microsoft.com) och logga in.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-136">Navigate toohello [Application Registration Portal](http://apps.dev.microsoft.com) and log in.</span></span>

13. <span data-ttu-id="7e2a4-137">Klicka på programnamnet.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-137">Click on your application name.</span></span> <span data-ttu-id="7e2a4-138">Anteckna hello **Programhemlighet** lösenord och hello **säkerhetsidentifierare (SID) för paketet** finns i hello **Windows Store** plattform inställningar.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-138">Make a note of hello **Application Secret** password and hello **Package security identifier (SID)** located in hello **Windows Store** platform settings.</span></span>

     > [AZURE.WARNING]
    <span data-ttu-id="7e2a4-139">Hej programhemlighet och paket-SID är viktiga säkerhetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-139">hello application secret and package SID are important security credentials.</span></span> <span data-ttu-id="7e2a4-140">Lämna aldrig ut dessa uppgifter till någon och distribuera dem inte tillsammans med din app.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-140">Do not share these values with anyone or distribute them with your app.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="7e2a4-141">Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="7e2a4-141">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="7e2a4-142">Välj hello <b>Notification Services</b> alternativet och hello <b>Windows (WNS)</b> alternativet.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-142">Select hello <b>Notification Services</b> option and hello <b>Windows (WNS)</b> option.</span></span> <span data-ttu-id="7e2a4-143">Ange hello <b>programhemlighet</b> lösenordet i hello <b>säkerhetsnyckeln</b> fältet.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-143">Then enter hello <b>Application secret</b> password in hello <b>Security Key</b> field.</span></span> <span data-ttu-id="7e2a4-144">Ange din <b>paket-SID</b> värde som du fick från WNS i hello föregående avsnitt och klickar sedan på <b>spara</b>.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-144">Enter your <b>Package SID</b> value that you obtained from WNS in hello previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

<span data-ttu-id="7e2a4-145">Din meddelandehubb har nu konfigurerat toowork med WNS och du har hello anslutning strängar tooregister din app och skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-145">Your notification hub is now configured toowork with WNS, and you have hello connection strings tooregister your app and send notifications.</span></span>

## <a name="connect-your-app-toohello-notification-hub"></a><span data-ttu-id="7e2a4-146">Ansluta din app toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="7e2a4-146">Connect your app toohello notification hub</span></span>
1. <span data-ttu-id="7e2a4-147">Högerklicka på hello lösning i Visual Studio och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-147">In Visual Studio, right-click hello solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="7e2a4-148">Detta visar hello **hantera NuGet-paket** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-148">This displays hello **Manage NuGet Packages** dialog box.</span></span>
2. <span data-ttu-id="7e2a4-149">Sök efter `WindowsAzure.Messaging.Managed` och på **installera**, och Godkänn hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-149">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and accept hello terms of use.</span></span>
   
    ![][20]
   
    <span data-ttu-id="7e2a4-150">Detta hämtar, installerar och lägger till en Azure Messaging-biblioteket på referens toohello för Windows med hjälp av hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">NuGet-paketet WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-150">This downloads, installs, and adds a reference toohello Azure Messaging library for Windows by using hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
3. <span data-ttu-id="7e2a4-151">Öppna hello projektfilen App.XAML.CS och Lägg till följande hello `using` instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-151">Open hello App.xaml.cs project file and add hello following `using` statements.</span></span> 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. <span data-ttu-id="7e2a4-152">Även i App.xaml.cs lägger du till följande hello **InitNotificationsAsync** metoden definition toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-152">Also in App.xaml.cs, add hello following **InitNotificationsAsync** method definition toohello **App** class:</span></span>
   
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
   
    <span data-ttu-id="7e2a4-153">Den här koden hämtar hello URI-kanalen för hello appen från WNS och registrerar sedan denna URI med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-153">This code retrieves hello channel URI for hello app from WNS, and then registers that channel URI with your notification hub.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="7e2a4-154">Se till att tooreplace hello platshållaren ”hubbnamn” med namnet hello hello meddelandehubb som visas i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-154">Make sure tooreplace hello "your hub name" placeholder with hello name of hello notification hub that appears in hello Azure Portal.</span></span> <span data-ttu-id="7e2a4-155">Ersätt även hello anslutning sträng platshållaren med hello **DefaultListenSharedAccessSignature** anslutningssträngen som du fick från hello **åtkomstprinciper** sidan i din Meddelandehubb i en föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-155">Also replace hello connection string placeholder with hello **DefaultListenSharedAccessSignature** connection string that you obtained from hello **Access Polices** page of your Notification Hub in a previous section.</span></span>
   > 
   > 
5. <span data-ttu-id="7e2a4-156">Hello överst i hello **OnLaunched** händelsehanteraren i App.xaml.cs Lägg till följande anrop toohello nya hello **InitNotificationsAsync** metod:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-156">At hello top of hello **OnLaunched** event handler in App.xaml.cs, add hello following call toohello new **InitNotificationsAsync** method:</span></span>
   
        InitNotificationsAsync();
   
    <span data-ttu-id="7e2a4-157">Detta garanterar hello kanalen URI registreras i meddelandehubben varje gång hello-programmet startades.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-157">This guarantees that hello channel URI is registered in your notification hub each time hello application is launched.</span></span>
6. <span data-ttu-id="7e2a4-158">Tryck på hello **F5** viktiga toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-158">Press hello **F5** key toorun hello app.</span></span> <span data-ttu-id="7e2a4-159">En dialogruta som innehåller hello registreringsnyckel visas.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-159">A pop-up dialog that contains hello registration key is displayed.</span></span>

<span data-ttu-id="7e2a4-160">Appen är nu redo tooreceive popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-160">Your app is now ready tooreceive toast notifications.</span></span>

## <a name="send-notifications"></a><span data-ttu-id="7e2a4-161">Skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="7e2a4-161">Send notifications</span></span>
<span data-ttu-id="7e2a4-162">Du kan snabbt testa ta emot meddelanden i appen genom att skicka meddelanden i hello [Azure Portal](https://portal.azure.com/) med hello **prova att skicka** knappen på hello meddelandehubb som visas i hello-skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-162">You can quickly test receiving notifications in your app by sending notifications in hello [Azure Portal](https://portal.azure.com/) using hello **Test Send** button on hello notification hub, as shown in hello screen below.</span></span>

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

<span data-ttu-id="7e2a4-163">Push-meddelanden skickas vanligtvis i en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-163">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="7e2a4-164">Du kan också använda hello REST API direkt toosend meddelande meddelanden om ett bibliotek inte är tillgänglig för din serverdel.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-164">You can also use hello REST API directly toosend notification messages if a library is not available for your back-end.</span></span> 

<span data-ttu-id="7e2a4-165">I den här självstudiekursen kommer vi enkelhet och hur du testar klientappen genom att skicka meddelanden med hello .NET SDK för meddelandehubbar i ett konsolprogram i stället för en serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-165">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using hello .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="7e2a4-166">Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers] självstudiekurs som hello nästa steg för att skicka meddelanden från en ASP.NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-166">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="7e2a4-167">Hello följande metoder kan dock användas för att skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-167">However, hello following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="7e2a4-168">**REST-gränssnittet**: du kan använda meddelanden på alla backend-plattformar med hello [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-168">**REST Interface**:  You can support notification on any backend platform using  hello [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="7e2a4-169">**Microsoft Azure Notification Hubs .NET SDK**: hello Nuget Package Manager för Visual Studio, köra [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-169">**Microsoft Azure Notification Hubs .NET SDK**: In hello Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="7e2a4-170">**Node.js** : [hur toouse Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-170">**Node.js** : [How toouse Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="7e2a4-171">**Azure Mobile Apps**: ett exempel på hur toosend meddelanden från en Azure Mobile App som är integrerad med Notification Hubs finns [Lägg till push-meddelanden för Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-171">**Azure Mobile Apps**: For an example of how toosend notifications from an Azure Mobile App that's integrated with Notification Hubs, see [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="7e2a4-172">**Java / PHP**: ett exempel på hur hello toosend meddelanden med hjälp av REST API: er, se ”hur toouse Notification Hubs från Java/PHP” ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-172">**Java / PHP**: For an example of how toosend notifications by using hello REST APIs, see "How toouse Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-console-app"></a><span data-ttu-id="7e2a4-173">(Valfritt) Skicka meddelanden från en konsolapp</span><span class="sxs-lookup"><span data-stu-id="7e2a4-173">(Optional) Send notifications from a console app</span></span>
<span data-ttu-id="7e2a4-174">toosend meddelanden med hjälp av en .NET-konsolprogram Följ dessa steg.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-174">toosend notifications by using a .NET console application follow these steps.</span></span> 

1. <span data-ttu-id="7e2a4-175">Högerklicka på hello lösning, Välj **Lägg till** och **nytt projekt...** , och sedan under **Visual C#**, klickar du på **Windows** och **konsolprogram**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-175">Right-click hello solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
    <span data-ttu-id="7e2a4-176">Detta lägger till en ny Visual C# konsolen programmet toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-176">This adds a new Visual C# console application toohello solution.</span></span> <span data-ttu-id="7e2a4-177">Du kan också göra detta i en separat lösning.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-177">You can also do this in a separate solution.</span></span>

2. <span data-ttu-id="7e2a4-178">I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-178">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="7e2a4-179">Detta visar hello Package Manager-konsolen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-179">This displays hello Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="7e2a4-180">Hello fönstret Package Manager-konsolen, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-180">In hello Package Manager Console window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="7e2a4-181">Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-181">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="7e2a4-182">Öppna hello Program.cs-filen och Lägg till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-182">Open hello Program.cs file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="7e2a4-183">I hello **programmet** klassen, Lägg till följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-183">In hello **Program** class, add hello following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure tooreplace hello "hub name" placeholder with hello name of hello notification hub that as it appears in hello Azure Portal. Also, replace hello connection string placeholder with hello **DefaultFullSharedAccessSignature** connection string that you obtained from hello **Access Policies** page of your Notification Hub in hello section called "Configure your notification hub."
   
   > [!NOTE]
   > <span data-ttu-id="7e2a4-184">Kontrollera att du använder hello anslutningssträngen som har **fullständig** åt inte **lyssna** åtkomst.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-184">Make sure that you use hello connection string that has **Full** access, not **Listen** access.</span></span> <span data-ttu-id="7e2a4-185">hello strängen med lyssna-åtkomst har inte behörighet toosend meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-185">hello listen-access string does not have permissions toosend notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="7e2a4-186">Lägg till följande rader i hello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="7e2a4-186">Add hello following lines in hello **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="7e2a4-187">Högerklicka på projektet för konsolprogrammet hello i Visual Studio och klicka på **Set as StartUp Project** tooset den som hello Startprojekt.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-187">Right-click hello console application project in Visual Studio, and click **Set as StartUp Project** tooset it as hello startup project.</span></span> <span data-ttu-id="7e2a4-188">Tryck på hello **F5** nyckeln toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-188">Then press hello **F5** key toorun hello application.</span></span>
   
    <span data-ttu-id="7e2a4-189">Du får ett popup-meddelande på alla registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-189">You will receive a toast notification on all registered devices.</span></span> <span data-ttu-id="7e2a4-190">Klicka på eller knackar hello popup-banderollen läser in hello app.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-190">Clicking or tapping hello toast banner loads hello app.</span></span>

<span data-ttu-id="7e2a4-191">Du hittar alla nyttolaster för hello stöds i hello [toast catalog], [panelen katalog], och [badge översikt] avsnitt på MSDN.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-191">You can find all hello supported payloads in hello [toast catalog], [tile catalog], and [badge overview] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="7e2a4-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7e2a4-192">Next steps</span></span>
<span data-ttu-id="7e2a4-193">I det här enkla exemplet skickade du broadcast-meddelanden tooall dina Windows-enheter med hjälp av hello-portalen eller ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-193">In this simple example, you sent broadcast notifications tooall your Windows devices using hello portal or a console app.</span></span> <span data-ttu-id="7e2a4-194">Vi rekommenderar hello [använda Notification Hubs toopush meddelanden toousers] självstudiekurs som hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-194">We recommend hello [Use Notification Hubs toopush notifications toousers] tutorial as hello next step.</span></span> <span data-ttu-id="7e2a4-195">Den visar hur toosend meddelanden från en ASP.NET-backend med taggar tootarget specifika användare.</span><span class="sxs-lookup"><span data-stu-id="7e2a4-195">It will show you how toosend notifications from an ASP.NET backend using tags tootarget specific users.</span></span>

<span data-ttu-id="7e2a4-196">Om du vill toosegment in användarna efter intressegrupper, se [använda Notification Hubs toosend senaste nytt].</span><span class="sxs-lookup"><span data-stu-id="7e2a4-196">If you want toosegment your users by interest groups, see [Use Notification Hubs toosend breaking news].</span></span> 

<span data-ttu-id="7e2a4-197">toolearn mer allmän information om Notification Hubs finns i [riktlinjer om Notification Hubs](notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7e2a4-197">toolearn more general information about Notification Hubs, see [Notification Hubs Guidance](notification-hubs-push-notification-overview.md).</span></span>

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
