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
# <a name="getting-started-with-notification-hubs-for-windows-universal-platform-apps"></a><span data-ttu-id="6b2e0-103">Komma igång med Notification Hubs för Windows Universal-plattformsappar</span><span class="sxs-lookup"><span data-stu-id="6b2e0-103">Getting started with Notification Hubs for Windows Universal Platform Apps</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="6b2e0-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6b2e0-104">Overview</span></span>
<span data-ttu-id="6b2e0-105">I de här självstudierna beskrivs hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Windows Universal-plattformsapp.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-105">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a Universal Windows Platform (UWP) app.</span></span>

<span data-ttu-id="6b2e0-106">I denna självstudiekurs skapar du en tom Windows Store-app som tar emot push-meddelanden med hjälp av Windows Push Notification Service (WNS).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-106">In this tutorial, you create a blank Windows Store app that receives push notifications by using the Windows Push Notification Service (WNS).</span></span> <span data-ttu-id="6b2e0-107">När du är klar kan du använda meddelandehubben för att sända push-meddelanden till alla enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-107">When you're finished, you'll be able to use your notification hub to broadcast push notifications to all the devices running your app.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="6b2e0-108">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="6b2e0-108">Before you begin</span></span>
[!INCLUDE [notification-hubs-hero-slug](../../includes/notification-hubs-hero-slug.md)]

<span data-ttu-id="6b2e0-109">Den färdiga koden för den här självstudiekursen hittar du på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-109">The completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/GetStartedWindowsUniversal).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6b2e0-110">Krav</span><span class="sxs-lookup"><span data-stu-id="6b2e0-110">Prerequisites</span></span>
<span data-ttu-id="6b2e0-111">För den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-111">This tutorial requires the following:</span></span>

* <span data-ttu-id="6b2e0-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) eller senare</span><span class="sxs-lookup"><span data-stu-id="6b2e0-112">[Microsoft Visual Studio Community 2015](https://www.visualstudio.com/products/visual-studio-community-vs) or later</span></span>
* [<span data-ttu-id="6b2e0-113">Universal Windows App Development Tools har installerats</span><span class="sxs-lookup"><span data-stu-id="6b2e0-113">Universal Windows App Development Tools installed</span></span>](https://msdn.microsoft.com/windows/uwp/get-started/get-set-up)
* <span data-ttu-id="6b2e0-114">Ett aktivt Azure-konto</span><span class="sxs-lookup"><span data-stu-id="6b2e0-114">An active Azure account</span></span> <br/><span data-ttu-id="6b2e0-115">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-115">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="6b2e0-116">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-116">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-store-dotnet-get-started%2F).</span></span>
* <span data-ttu-id="6b2e0-117">Ett aktivt Windows Store-konto</span><span class="sxs-lookup"><span data-stu-id="6b2e0-117">An active Windows Store account</span></span>

<span data-ttu-id="6b2e0-118">Du måste slutföra de här självstudierna innan du påbörjar någon annan kurs om Notification Hubs för Windows Universal-plattformsappar.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-118">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Universal Platform apps.</span></span>

## <a name="register-your-app-for-the-windows-store"></a><span data-ttu-id="6b2e0-119">Registrera din app för Windows Store</span><span class="sxs-lookup"><span data-stu-id="6b2e0-119">Register your app for the Windows Store</span></span>
<span data-ttu-id="6b2e0-120">Om du vill skicka push-meddelanden till UWP-appar, måste du associera din app med Windows Store.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-120">To send push notifications to UWP apps, you must associate your app to the Windows Store.</span></span> <span data-ttu-id="6b2e0-121">Sedan måste du konfigurera meddelandehubben för att integrera den med WNS.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-121">You must then configure your notification hub to integrate with WNS.</span></span>

1. <span data-ttu-id="6b2e0-122">Om du inte redan har registrerat appen navigerar du till [Windows Dev Center](https://dev.windows.com/overview), loggar in med ditt Microsoft-konto och klickar sedan på **Skapa en ny app**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-122">If you have not already registered your app, navigate to the [Windows Dev Center](https://dev.windows.com/overview), sign in with your Microsoft account, and then click **Create a new app**.</span></span>

2. <span data-ttu-id="6b2e0-123">Ange ett namn för appen och klicka på **Reservera appnamn**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-123">Type a name for your app and click **Reserve app name**.</span></span> <span data-ttu-id="6b2e0-124">Detta skapar en ny Windows Store-registrering för din app.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-124">This creates a new Windows Store registration for your app.</span></span>

3. <span data-ttu-id="6b2e0-125">Skapa ett nytt Visual C# Store Apps-projekt i Visual Studio med hjälp av Windows Universal-mallen **Tom app** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-125">In Visual Studio, create a new Visual C# Store Apps project by using the Windows Universal **Blank App** template and click **OK**.</span></span>

4. <span data-ttu-id="6b2e0-126">Acceptera standardinställningarna för mål- och minsta plattformsversioner.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-126">Accept the defaults for the target and minimum platform versions.</span></span>

5. <span data-ttu-id="6b2e0-127">I Solution Explorer högerklickar du på approjektet för Windows Store och sedan klickar du på **Store**. Slutligen klickar du på **Associera app med Store...**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-127">In Solution Explorer, right-click the Windows Store app project, click **Store**, and then click **Associate App with the Store...**.</span></span> <span data-ttu-id="6b2e0-128">Guiden **Associera din app med Windows Store** visas.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-128">The **Associate Your App with the Windows Store** wizard appears.</span></span>

6. <span data-ttu-id="6b2e0-129">I guiden loggar du in med ditt Microsoft-konto.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-129">In the wizard, sign in with your Microsoft account.</span></span>

7. <span data-ttu-id="6b2e0-130">Klicka på den app som du registrerade i steg 2, sedan på **Nästa** och slutligen på **Associera**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-130">Click the app that you registered in step 2, click **Next**, and then click **Associate**.</span></span> <span data-ttu-id="6b2e0-131">Detta lägger till den registreringsinformation som krävs för Windows Store i programmanifestet.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-131">This adds the required Windows Store registration information to the application manifest.</span></span>

8. <span data-ttu-id="6b2e0-132">Tillbaka på sidan [Windows Dev Center](http://dev.windows.com/overview) för din nya app klickar du på **Tjänster**, klickar på **Push-meddelanden** och sedan på **WNS/MPNS**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-132">Back on the [Windows Dev Center](http://dev.windows.com/overview) page for your new app, click **Services**, click **Push notifications**, and then click **WNS/MPNS**.</span></span>

9. <span data-ttu-id="6b2e0-133">Klicka på **Nytt meddelande**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-133">Click **New Notification**.</span></span>

10. <span data-ttu-id="6b2e0-134">Klicka på mallen **Blank (Toast)** (Tom (popup)) och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-134">Click **Blank (Toast)** template and then click **OK**.</span></span>

11. <span data-ttu-id="6b2e0-135">Ange ett **meddelandenamn** och ett meddelande för **visuell kontext**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-135">Enter a notification **Name** and Visual **Context** message.</span></span> <span data-ttu-id="6b2e0-136">Klicka sedan på **Spara som utkast**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-136">Then click **Save as draft**.</span></span>

12. <span data-ttu-id="6b2e0-137">Gå till [programregistreringsportalen](http://apps.dev.microsoft.com) och logga in.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-137">Navigate to the [Application Registration Portal](http://apps.dev.microsoft.com) and log in.</span></span>

13. <span data-ttu-id="6b2e0-138">Klicka på programnamnet.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-138">Click on your application name.</span></span> <span data-ttu-id="6b2e0-139">Skriv ner lösenordet för **apphemligheten** och **paketsäkerhets-ID:t (SID)** som du hittar i plattformsinställningarna för **Windows Store**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-139">Make a note of the **Application Secret** password and the **Package security identifier (SID)** located in the **Windows Store** platform settings.</span></span>

     > [AZURE.WARNING]
    <span data-ttu-id="6b2e0-140">Programhemligheten och paket-SID:et är viktiga säkerhetsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-140">The application secret and package SID are important security credentials.</span></span> <span data-ttu-id="6b2e0-141">Lämna aldrig ut dessa uppgifter till någon och distribuera dem inte tillsammans med din app.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-141">Do not share these values with anyone or distribute them with your app.</span></span>

## <a name="configure-your-notification-hub"></a><span data-ttu-id="6b2e0-142">Konfigurera meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="6b2e0-142">Configure your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="6b2e0-143">Välj alternativet <b>Notification Services</b> och <b>Windows (WNS)</b>.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-143">Select the <b>Notification Services</b> option and the <b>Windows (WNS)</b> option.</span></span> <span data-ttu-id="6b2e0-144">Ange lösenordet för <b>Apphemlighet</b> i fältet <b>Säkerhetsnyckel</b>.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-144">Then enter the <b>Application secret</b> password in the <b>Security Key</b> field.</span></span> <span data-ttu-id="6b2e0-145">Ange ditt <b>Paket-SID</b>-värde som du fick från WNS i föregående avsnitt och klicka sedan på <b>Spara</b>.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-145">Enter your <b>Package SID</b> value that you obtained from WNS in the previous section, and then click <b>Save</b>.</span></span></p>
</li>
</ol>

&emsp;&emsp;![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-configure-wns.png)

<span data-ttu-id="6b2e0-146">Din meddelandehubb har nu konfigurerats för att fungera med WNS och du har anslutningssträngar för att registrera din app och skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-146">Your notification hub is now configured to work with WNS, and you have the connection strings to register your app and send notifications.</span></span>

## <a name="connect-your-app-to-the-notification-hub"></a><span data-ttu-id="6b2e0-147">Anslut appen till meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="6b2e0-147">Connect your app to the notification hub</span></span>
1. <span data-ttu-id="6b2e0-148">Högerklicka på lösningen i Vision Studio och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-148">In Visual Studio, right-click the solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="6b2e0-149">Dialogrutan **Hantera NuGet-paket** öppnas.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-149">This displays the **Manage NuGet Packages** dialog box.</span></span>
2. <span data-ttu-id="6b2e0-150">Leta rätt på `WindowsAzure.Messaging.Managed` och klicka på **Installera**. Godkänn sedan användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-150">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and accept the terms of use.</span></span>
   
    ![][20]
   
    <span data-ttu-id="6b2e0-151">Den här åtgärden hämtar, installerar och lägger till en referens i Azure Messaging-biblioteket för Windows med hjälp av <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">NuGet-paketet WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-151">This downloads, installs, and adds a reference to the Azure Messaging library for Windows by using the <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
3. <span data-ttu-id="6b2e0-152">Öppna projektfilen App.xaml.cs och lägg till följande `using`-uttryck:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-152">Open the App.xaml.cs project file and add the following `using` statements.</span></span> 
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.UI.Popups;
4. <span data-ttu-id="6b2e0-153">I filen App.xaml.cs ska du även lägga till följande **InitNotificationsAsync**-metoddefinitionen i **app**-klassen:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-153">Also in App.xaml.cs, add the following **InitNotificationsAsync** method definition to the **App** class:</span></span>
   
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
   
    <span data-ttu-id="6b2e0-154">Den här koden hämtar URI-kanalen för appen från WNS och registrerar sedan denna med din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-154">This code retrieves the channel URI for the app from WNS, and then registers that channel URI with your notification hub.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="6b2e0-155">Ersätt platshållaren ”ditt hubbnamn” med namnet på den meddelandehubb som visas i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-155">Make sure to replace the "your hub name" placeholder with the name of the notification hub that appears in the Azure Portal.</span></span> <span data-ttu-id="6b2e0-156">Byt även ut platshållaren för anslutningssträngen mot anslutningssträngen **DefaultListenSharedAccessSignature** som du fick från sidan **Åtkomstpoliyer** i ett föregående avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-156">Also replace the connection string placeholder with the **DefaultListenSharedAccessSignature** connection string that you obtained from the **Access Polices** page of your Notification Hub in a previous section.</span></span>
   > 
   > 
5. <span data-ttu-id="6b2e0-157">Längst upp i händelsehanteraren **OnLaunched** i App.xaml.cs lägger du till följande anrop till den nya **InitNotificationsAsync**-metoden:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-157">At the top of the **OnLaunched** event handler in App.xaml.cs, add the following call to the new **InitNotificationsAsync** method:</span></span>
   
        InitNotificationsAsync();
   
    <span data-ttu-id="6b2e0-158">Detta garanterar även att URI-kanalen registreras i meddelandehubben varje gång appen startas.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-158">This guarantees that the channel URI is registered in your notification hub each time the application is launched.</span></span>
6. <span data-ttu-id="6b2e0-159">Kör appen genom att trycka på tangenten **F5**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-159">Press the **F5** key to run the app.</span></span> <span data-ttu-id="6b2e0-160">En dialogruta som innehåller registreringsnyckeln visas.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-160">A pop-up dialog that contains the registration key is displayed.</span></span>

<span data-ttu-id="6b2e0-161">Appen är nu redo att ta emot popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-161">Your app is now ready to receive toast notifications.</span></span>

## <a name="send-notifications"></a><span data-ttu-id="6b2e0-162">Skicka meddelanden</span><span class="sxs-lookup"><span data-stu-id="6b2e0-162">Send notifications</span></span>
<span data-ttu-id="6b2e0-163">Du kan snabbt testa att ta emot meddelanden i appen genom att skicka meddelanden [Azure Portal](https://portal.azure.com/) med knappen **Testsänd** i meddelandehubben, så som visas på skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-163">You can quickly test receiving notifications in your app by sending notifications in the [Azure Portal](https://portal.azure.com/) using the **Test Send** button on the notification hub, as shown in the screen below.</span></span>

![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-test-send-wns.png)

<span data-ttu-id="6b2e0-164">Push-meddelanden skickas vanligtvis i en serverdelstjänst som Mobile Services eller ASP.NET med hjälp av ett kompatibelt bibliotek.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-164">Push notifications are normally sent in a back-end service like Mobile Services or ASP.NET using a compatible library.</span></span> <span data-ttu-id="6b2e0-165">Du kan också använda REST-API:et direkt för att skicka meddelanden om ett bibliotek inte är tillgängligt för din serverdel.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-165">You can also use the REST API directly to send notification messages if a library is not available for your back-end.</span></span> 

<span data-ttu-id="6b2e0-166">I den här enkla självstudiekursen visar vi hur du testar klientappen genom att skicka meddelanden med .NET SDK för meddelandehubbar i ett konsolprogram i stället för med en serverdelstjänst.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-166">In this tutorial, we will keep it simple and just demonstrate testing your client app by sending notifications using the .NET SDK for notification hubs in a console application instead of a backend service.</span></span> <span data-ttu-id="6b2e0-167">Vi rekommenderar att du går vidare med självstudiekursen [Använda Notification Hubs för att skicka push-meddelanden till användare] som nästa steg i att skicka meddelanden från en ASP.NET-serverdel.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-167">We recommend the [Use Notification Hubs to push notifications to users] tutorial as the next step for sending notifications from an ASP.NET backend.</span></span> <span data-ttu-id="6b2e0-168">Följande åtgärder kan dock användas för att skicka meddelanden:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-168">However, the following approaches can be used for sending notifications:</span></span>

* <span data-ttu-id="6b2e0-169">**REST-gränssnitt**: Du kan använda meddelanden på alla serverdelsplattformar med [REST-gränssnittet](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-169">**REST Interface**:  You can support notification on any backend platform using  the [REST interface](http://msdn.microsoft.com/library/windowsazure/dn223264.aspx).</span></span>
* <span data-ttu-id="6b2e0-170">**Microsoft Azure Notification Hubs .NET SDK**: I Nuget Package Manager för Visual Studio kör du [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-170">**Microsoft Azure Notification Hubs .NET SDK**: In the Nuget Package Manager for Visual Studio, run [Install-Package Microsoft.Azure.NotificationHubs](https://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/).</span></span>
* <span data-ttu-id="6b2e0-171">**Node.js**: [Använda Notification Hubs från Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-171">**Node.js** : [How to use Notification Hubs from Node.js](notification-hubs-nodejs-push-notification-tutorial.md).</span></span>
* <span data-ttu-id="6b2e0-172">**Azure Mobile Apps**: För ett exempel på hur man skickar meddelanden från en Azure Mobile App som är integrerad med Notification Hubs, kan du gå till [Lägg till Push-meddelanden för Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-172">**Azure Mobile Apps**: For an example of how to send notifications from an Azure Mobile App that's integrated with Notification Hubs, see [Add push notifications for Mobile Apps](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md).</span></span>
* <span data-ttu-id="6b2e0-173">**Java/PHP**: Ett exempel på hur du skickar meddelanden med hjälp av REST-API finns i avsnittet Använda Notification Hubs från Java/PHP ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-173">**Java / PHP**: For an example of how to send notifications by using the REST APIs, see "How to use Notification Hubs from Java/PHP" ([Java](notification-hubs-java-push-notification-tutorial.md) | [PHP](notification-hubs-php-push-notification-tutorial.md)).</span></span>

## <a name="optional-send-notifications-from-a-console-app"></a><span data-ttu-id="6b2e0-174">(Valfritt) Skicka meddelanden från en konsolapp</span><span class="sxs-lookup"><span data-stu-id="6b2e0-174">(Optional) Send notifications from a console app</span></span>
<span data-ttu-id="6b2e0-175">Följ anvisningarna nedan om du vill skicka meddelanden med hjälp av ett .NET-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-175">To send notifications by using a .NET console application follow these steps.</span></span> 

1. <span data-ttu-id="6b2e0-176">Högerklicka på lösningen, välj **Lägg till** och **Nytt projekt...** och klicka sedan på **Windows** och **Konsolprogram** under **Visual C#**. Slutligen klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-176">Right-click the solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
    <span data-ttu-id="6b2e0-177">Detta lägger till ett nytt konsolprogram för Visual C# i lösningen.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-177">This adds a new Visual C# console application to the solution.</span></span> <span data-ttu-id="6b2e0-178">Du kan också göra detta i en separat lösning.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-178">You can also do this in a separate solution.</span></span>

2. <span data-ttu-id="6b2e0-179">I Visual Studio klickar du på **Verktyg**, **NuGet Package Manager** och sedan på **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-179">In Visual Studio, click **Tools**, click **NuGet Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="6b2e0-180">Då visas Package Manager-konsolen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-180">This displays the Package Manager Console in Visual Studio.</span></span>
3. <span data-ttu-id="6b2e0-181">I fönstret Package Manager-konsol ställer du in **standardprojektet** till det nya projektet för konsolprogrammet. Sedan kör du följande kommando i konsolfönstret:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-181">In the Package Manager Console window, set the **Default project** to your new console application project, and then in the console window, execute the following command:</span></span>
   
        Install-Package Microsoft.Azure.NotificationHubs
   
    <span data-ttu-id="6b2e0-182">Då läggs en referens till i Azure Notification Hubs SDK med hjälp av <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs-NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-182">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
   
    ![](./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-package-manager.png)
4. <span data-ttu-id="6b2e0-183">Öppna filen Program.cs och lägg till följande `using`-uttryck:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-183">Open the Program.cs file and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="6b2e0-184">Lägg till följande metod i **Program**-klassen:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-184">In the **Program** class, add the following method:</span></span>
   
        private static async void SendNotificationAsync()
        {
            NotificationHubClient hub = NotificationHubClient
                .CreateClientFromConnectionString("<connection string with full access>", "<hub name>");
            var toast = @"<toast><visual><binding template=""ToastText01""><text id=""1"">Hello from a .NET App!</text></binding></visual></toast>";
            await hub.SendWindowsNativeNotificationAsync(toast);
        }
   
       Make sure to replace the "hub name" placeholder with the name of the notification hub that as it appears in the Azure Portal. Also, replace the connection string placeholder with the **DefaultFullSharedAccessSignature** connection string that you obtained from the **Access Policies** page of your Notification Hub in the section called "Configure your notification hub."
   
   > [!NOTE]
   > <span data-ttu-id="6b2e0-185">Kontrollera att du använder anslutningssträngen som har **fullständig** åtkomst, inte enbart åtkomst för att **lyssna**.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-185">Make sure that you use the connection string that has **Full** access, not **Listen** access.</span></span> <span data-ttu-id="6b2e0-186">Strängen med lyssna-åtkomst har inte behörighet att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-186">The listen-access string does not have permissions to send notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="6b2e0-187">Lägg till följande rader i **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="6b2e0-187">Add the following lines in the **Main** method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="6b2e0-188">Högerklicka på konsolprogramprojektet i Visual Studio och klicka på **Konfigurera som startprojekt** för att ställa in det som startprojekt.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-188">Right-click the console application project in Visual Studio, and click **Set as StartUp Project** to set it as the startup project.</span></span> <span data-ttu-id="6b2e0-189">Tryck på tangenten **F5** för att köra appen.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-189">Then press the **F5** key to run the application.</span></span>
   
    <span data-ttu-id="6b2e0-190">Du får ett popup-meddelande på alla registrerade enheter.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-190">You will receive a toast notification on all registered devices.</span></span> <span data-ttu-id="6b2e0-191">Appen läses in när du klickar eller knackar på popup-banderollen.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-191">Clicking or tapping the toast banner loads the app.</span></span>

<span data-ttu-id="6b2e0-192">Du hittar alla nyttolaster som stöds under ämnena [toast catalog] (katalog över popup-meddelanden), [tile catalog] (katalog över paneler) och [badge overview] (översikt över aktivitetsikoner) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-192">You can find all the supported payloads in the [toast catalog], [tile catalog], and [badge overview] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6b2e0-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6b2e0-193">Next steps</span></span>
<span data-ttu-id="6b2e0-194">I det här enkla exemplet skickar du broadcast-meddelanden till alla dina Windows-enheter med hjälp av portalen eller ett konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-194">In this simple example, you sent broadcast notifications to all your Windows devices using the portal or a console app.</span></span> <span data-ttu-id="6b2e0-195">Vi rekommenderar att du går vidare med självstudiekursen [Använda Notification Hubs för att skicka push-meddelanden till användare] som nästa steg.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-195">We recommend the [Use Notification Hubs to push notifications to users] tutorial as the next step.</span></span> <span data-ttu-id="6b2e0-196">Den visar hur du skickar meddelanden från en ASP.NET-backend med hjälp av taggar för att rikta in dig på vissa specifika användare.</span><span class="sxs-lookup"><span data-stu-id="6b2e0-196">It will show you how to send notifications from an ASP.NET backend using tags to target specific users.</span></span>

<span data-ttu-id="6b2e0-197">Om du vill dela in användarna efter intressegrupper, kan du gå till [Använda Notification Hubs för att skicka de senaste nyheterna].</span><span class="sxs-lookup"><span data-stu-id="6b2e0-197">If you want to segment your users by interest groups, see [Use Notification Hubs to send breaking news].</span></span> 

<span data-ttu-id="6b2e0-198">Om du vill få mer allmän information om Notification Hubs kan du läsa [Riktlinjer om Notification Hubs](notification-hubs-push-notification-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6b2e0-198">To learn more general information about Notification Hubs, see [Notification Hubs Guidance](notification-hubs-push-notification-overview.md).</span></span>

<!-- Images. -->
[13]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-create-console-app.png
[14]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-toast.png
[19]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-reg.png
[20]: ./media/notification-hubs-windows-store-dotnet-get-started/notification-hub-windows-universal-app-install-package.png

<!-- URLs. -->

<span data-ttu-id="6b2e0-199">[Använda Notification Hubs för att skicka push-meddelanden till användare]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span><span class="sxs-lookup"><span data-stu-id="6b2e0-199">[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span></span>
<span data-ttu-id="6b2e0-200">[Använda Notification Hubs för att skicka de senaste nyheterna]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span><span class="sxs-lookup"><span data-stu-id="6b2e0-200">[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span></span>

<span data-ttu-id="6b2e0-201">[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx</span><span class="sxs-lookup"><span data-stu-id="6b2e0-201">[toast catalog]: http://msdn.microsoft.com/library/windows/apps/hh761494.aspx</span></span>
<span data-ttu-id="6b2e0-202">[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx</span><span class="sxs-lookup"><span data-stu-id="6b2e0-202">[tile catalog]: http://msdn.microsoft.com/library/windows/apps/hh761491.aspx</span></span>
<span data-ttu-id="6b2e0-203">[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx</span><span class="sxs-lookup"><span data-stu-id="6b2e0-203">[badge overview]: http://msdn.microsoft.com/library/windows/apps/hh779719.aspx</span></span>
