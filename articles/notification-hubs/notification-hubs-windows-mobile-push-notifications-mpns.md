---
title: "Skicka push-meddelanden med Azure Notification Hubs på Windows Phone | Microsoft Docs"
description: "I den här självstudiekursen kommer du att få lära dig hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Silverlight-app på Windows Phone 8 eller Windows Phone 8.1."
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
ms.openlocfilehash: f0bfe81f849813d146d644b32490af657b1071b5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a><span data-ttu-id="14a4e-104">Skicka push-meddelanden med Azure Notification Hubs på Windows Phone</span><span class="sxs-lookup"><span data-stu-id="14a4e-104">Sending push notifications with Azure Notification Hubs on Windows Phone</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="14a4e-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="14a4e-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="14a4e-106">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-106">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="14a4e-107">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="14a4e-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="14a4e-108">Mer information om den kostnadsfria utvärderingsversionen av Azure finns [här](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="14a4e-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="14a4e-109">I den här självstudiekursen beskrivs hur du använder Azure Notification Hubs för att skicka push-meddelanden till en Silverlight-app på Windows Phone 8 eller Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="14a4e-109">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a Windows Phone 8 or Windows Phone 8.1 Silverlight application.</span></span> <span data-ttu-id="14a4e-110">Om du vill skicka meddelanden till Windows Phone 8.1 (utan Silverlight), ska du använda versionen för [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span><span class="sxs-lookup"><span data-stu-id="14a4e-110">If you are targeting Windows Phone 8.1 (non-Silverlight), then refer to the [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) version.</span></span>
<span data-ttu-id="14a4e-111">I denna självstudiekurs skapar du en tom Windows Phone 8-app som tar emot push-meddelanden med hjälp av Microsoft Push Notification Service (MPNS).</span><span class="sxs-lookup"><span data-stu-id="14a4e-111">In this tutorial, you create a blank Windows Phone 8 app that receives push notifications by using the Microsoft Push Notification Service (MPNS).</span></span> <span data-ttu-id="14a4e-112">När du är klar kan du använda meddelandehubben för att sända push-meddelanden till alla enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-112">When you're finished, you'll be able to use your notification hub to broadcast push notifications to all the devices running your app.</span></span>

> [!NOTE]
> <span data-ttu-id="14a4e-113">SDK:erna för Windows Phone på Notification Hubs stöder inte användning av Windows Push Notification Service (WNS) med Silverlight-appar för Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="14a4e-113">The Notification Hubs Windows Phone SDK does not support using the Windows Push Notification Service (WNS) with Windows Phone 8.1 Silverlight apps.</span></span> <span data-ttu-id="14a4e-114">Om du vill använda WNS (istället för MPNS) med Silverlight-appar för Windows Phone 8.1, ska du följa anvisningarna i [Notification Hubs – självstudiekurs för Windows Phone Silverlight]. Där används istället REST-API:er.</span><span class="sxs-lookup"><span data-stu-id="14a4e-114">To use WNS (instead of MPNS) with Windows Phone 8.1 Silverlight apps, follow the [Notification Hubs - Windows Phone Silverlight tutorial], which uses REST APIs.</span></span>
> 
> 

<span data-ttu-id="14a4e-115">I den här självstudiekursen visas ett enkelt scenario för sändning med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="14a4e-115">The tutorial demonstrates the simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="14a4e-116">Krav</span><span class="sxs-lookup"><span data-stu-id="14a4e-116">Prerequisites</span></span>
<span data-ttu-id="14a4e-117">För den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="14a4e-117">This tutorial requires the following:</span></span>

* <span data-ttu-id="14a4e-118">[Visual Studio 2012 Express för Windows Phone], eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="14a4e-118">[Visual Studio 2012 Express for Windows Phone], or a later version.</span></span>

<span data-ttu-id="14a4e-119">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Windows Phone 8-appar.</span><span class="sxs-lookup"><span data-stu-id="14a4e-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Phone 8 apps.</span></span>

## <a name="create-your-notification-hub"></a><span data-ttu-id="14a4e-120">Skapa din meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="14a4e-120">Create your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="14a4e-121">Klicka på avsnittet <b>Notification Services</b> (inne i <i>Inställningar</i>) och sedan på <b>Windows Phone (MPNS)</b>. Därefter markerar du kryssrutan <b>Aktivera ej autentiserade push-meddelanden</b>.</span><span class="sxs-lookup"><span data-stu-id="14a4e-121">Click the <b>Notification Services</b> section (within <i>Settings</i>), click on <b>Windows Phone (MPNS)</b> and then click the <b>Enable unauthenticated push</b> check box.</span></span></p>
</li>
</ol>

&emsp;&emsp;![Azure-portalen – Aktivera ej autentiserade push-meddelanden](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

<span data-ttu-id="14a4e-123">Hubben har nu skapats och konfigurerats för att skicka ej autentiserade meddelanden till Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="14a4e-123">Your hub is now created and configured to send unauthenticated notification for Windows Phone.</span></span>

> [!NOTE]
> <span data-ttu-id="14a4e-124">I den här kursen används MPNS i ej autentiserat läge.</span><span class="sxs-lookup"><span data-stu-id="14a4e-124">This tutorial uses MPNS in unauthenticated mode.</span></span> <span data-ttu-id="14a4e-125">MPNS i ej autentiserat läge har begränsningar för vilka meddelanden du kan skicka till varje kanal.</span><span class="sxs-lookup"><span data-stu-id="14a4e-125">MPNS unauthenticated mode comes with restrictions on notifications that you can send to each channel.</span></span> <span data-ttu-id="14a4e-126">Notification Hubs stöder [MPNS i autentiserat läge](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) genom att låta dig överföra ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="14a4e-126">Notification Hubs supports [MPNS authenticated mode](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) by allowing you to upload your certificate.</span></span>
> 
> 

## <a name="connecting-your-app-to-the-notification-hub"></a><span data-ttu-id="14a4e-127">Ansluta appen till meddelandehubben</span><span class="sxs-lookup"><span data-stu-id="14a4e-127">Connecting your app to the notification hub</span></span>
1. <span data-ttu-id="14a4e-128">Skapa en ny Windows Phone 8-app i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="14a4e-128">In Visual Studio, create a new Windows Phone 8 application.</span></span>
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    <span data-ttu-id="14a4e-129">I Visual Studio 2013 Update 2 eller senare kan du istället skapa en Silverlight-app för Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="14a4e-129">In Visual Studio 2013 Update 2 or later, you instead create a Windows Phone Silverlight application.</span></span>
   
    ![Visual Studio – Nytt projekt – Tom app – Silverlight för Windows Phone][11]
2. <span data-ttu-id="14a4e-131">Högerklicka på lösningen i Vision Studio och klicka sedan på **Hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="14a4e-131">In Visual Studio, right-click the solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="14a4e-132">Dialogrutan **Hantera NuGet-paket** öppnas.</span><span class="sxs-lookup"><span data-stu-id="14a4e-132">This displays the **Manage NuGet Packages** dialog box.</span></span>
3. <span data-ttu-id="14a4e-133">Leta rätt på `WindowsAzure.Messaging.Managed` och klicka på **Installera**. Godkänn sedan användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="14a4e-133">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and then accept the terms of use.</span></span>
   
    ![Visual Studio – NuGet Package Manager][20]
   
    <span data-ttu-id="14a4e-135">Den här åtgärden hämtar, installerar och lägger till en referens i Azure Messaging-biblioteket för Windows med hjälp av <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">NuGet-paketet WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="14a4e-135">This downloads, installs, and adds a reference to the Azure Messaging library for Windows by using the <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
4. <span data-ttu-id="14a4e-136">Öppna filen App.xaml.cs och lägg till följande `using`-uttryck:</span><span class="sxs-lookup"><span data-stu-id="14a4e-136">Open the file App.xaml.cs and add the following `using` statements:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. <span data-ttu-id="14a4e-137">Lägg till följande kod högst upp i metoden **Application_Launching** i App.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="14a4e-137">Add the following code at the top of **Application_Launching** method in App.xaml.cs:</span></span>
   
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
   > <span data-ttu-id="14a4e-138">Värdet **MyPushChannel** är ett index som används för att söka efter en befintlig kanal i samlingen [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx).</span><span class="sxs-lookup"><span data-stu-id="14a4e-138">The value **MyPushChannel** is an index that is used to lookup an existing channel in the [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) collection.</span></span> <span data-ttu-id="14a4e-139">Om det inte finns någon sådan där, skapar du en ny post med det namnet.</span><span class="sxs-lookup"><span data-stu-id="14a4e-139">If there isn't one there, create a new entry with that name.</span></span>
   > 
   > 
   
    <span data-ttu-id="14a4e-140">Infoga namnet på din hubb och anslutningssträngen med namnet **DefaultListenSharedAccessSignature** som du fick i förra avsnittet.</span><span class="sxs-lookup"><span data-stu-id="14a4e-140">Make sure to insert the name of your hub and the connection string called **DefaultListenSharedAccessSignature** that you obtained in the previous section.</span></span>
    <span data-ttu-id="14a4e-141">Den här koden hämtar URI-kanalen för appen från MPNS och registrerar sedan denna i din meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="14a4e-141">This code retrieves the channel URI for the app from MPNS, and then registers that channel URI with your notification hub.</span></span> <span data-ttu-id="14a4e-142">Det garanterar även att URI-kanalen registreras i meddelandehubben varje gång appen startas.</span><span class="sxs-lookup"><span data-stu-id="14a4e-142">It also guarantees that the channel URI is registered in your notification hub each time the application is launched.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="14a4e-143">I denna självstudiekurs visar vi hur du skickar ett popup-meddelande till enheter.</span><span class="sxs-lookup"><span data-stu-id="14a4e-143">This tutorial sends a toast notification to the device.</span></span> <span data-ttu-id="14a4e-144">Om du vill skicka ett panelmeddelande måste du istället anropa metoden **BindToShellTile** på kanalen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-144">When you send a tile notification, you must instead call the **BindToShellTile** method on the channel.</span></span> <span data-ttu-id="14a4e-145">Om du vill använda både popup- och panelmeddelanden, anropar du både **BindToShellTile** och **BindToShellToast**.</span><span class="sxs-lookup"><span data-stu-id="14a4e-145">To support both toast and tile notifications, call both **BindToShellTile** and  **BindToShellToast**.</span></span>
   > 
   > 
6. <span data-ttu-id="14a4e-146">I Solution Explorer expanderar du **Egenskaper**, öppnar `WMAppManifest.xml`-filen, klickar på fliken **Funktioner** och kontrollerar att funktionen **ID_CAP_PUSH_NOTIFICATION** är markerad.</span><span class="sxs-lookup"><span data-stu-id="14a4e-146">In Solution Explorer, expand **Properties**, open the `WMAppManifest.xml` file, click the **Capabilities** tab, and make sure that the **ID_CAP_PUSH_NOTIFICATION** capability is checked.</span></span>
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt to send a push notification to the app will fail.
7. <span data-ttu-id="14a4e-147">Kör appen genom att trycka på tangenten `F5`.</span><span class="sxs-lookup"><span data-stu-id="14a4e-147">Press the `F5` key to run the app.</span></span>
   
    <span data-ttu-id="14a4e-148">Ett registreringsmeddelande visas i appen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-148">A registration message is displayed in the app.</span></span>
8. <span data-ttu-id="14a4e-149">Stäng appen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-149">Close the app.</span></span>  
   
   > [!NOTE]
   > <span data-ttu-id="14a4e-150">Om du vill få ett push-meddelande av popup-typ får appen inte köras i förgrunden.</span><span class="sxs-lookup"><span data-stu-id="14a4e-150">To receive a toast push notification, the application must not be running in the foreground.</span></span>
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a><span data-ttu-id="14a4e-151">Skicka push-meddelanden från serverdelen</span><span class="sxs-lookup"><span data-stu-id="14a4e-151">Send push notifications from your backend</span></span>
<span data-ttu-id="14a4e-152">Du kan skicka push-meddelanden med hjälp av Notification Hubs från vilken serverdel som helst via det offentliga <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-gränssnittet</a>.</span><span class="sxs-lookup"><span data-stu-id="14a4e-152">You can send push notifications by using Notification Hubs from any backend via the public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="14a4e-153">I den här självstudiekursen kommer du att skicka push-meddelanden via ett .NET-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="14a4e-153">In this tutorial, you send push notifications using a .NET console application.</span></span> 

<span data-ttu-id="14a4e-154">Om du vill se ett exempel på hur du skickar push-meddelanden från en ASP.NET-WebAPI-serverdel som är integrerad med Notification Hubs, kan du gå till [Meddela användare via en .NET-serverdel i Azure Notification Hubs](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span><span class="sxs-lookup"><span data-stu-id="14a4e-154">For an example of how to send push notifications from an ASP.NET WebAPI backend that's integrated with Notification Hubs, see [Azure Notification Hubs Notify Users with .NET backend](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span></span>  

<span data-ttu-id="14a4e-155">Om du vill se ett exempel på hur du kan skicka push-meddelanden med hjälp av [REST-API:er](https://msdn.microsoft.com/library/azure/dn223264.aspx), kan du gå till [Använda Notification Hubs från Java](notification-hubs-java-push-notification-tutorial.md) och [Använda Notification Hubs från PHP](notification-hubs-php-push-notification-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="14a4e-155">For an example of how to send push notifications by using the [REST APIs](https://msdn.microsoft.com/library/azure/dn223264.aspx), check out [How to use Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) and [How to use Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

1. <span data-ttu-id="14a4e-156">Högerklicka på lösningen, välj **Lägg till** och **Nytt projekt...** och klicka sedan på **Windows** och **Konsolprogram** under **Visual C#**. Slutligen klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="14a4e-156">Right-click the solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
       ![Visual Studio - New Project - Console Application][6]
   
    <span data-ttu-id="14a4e-157">Detta lägger till ett nytt konsolprogram för Visual C# i lösningen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-157">This adds a new Visual C# console application to the solution.</span></span> <span data-ttu-id="14a4e-158">Du kan också göra detta i en separat lösning.</span><span class="sxs-lookup"><span data-stu-id="14a4e-158">You can also do this in a separate solution.</span></span>
2. <span data-ttu-id="14a4e-159">Klicka på **Verktyg**, **Library Package Manager** och slutligen **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="14a4e-159">Click **Tools**, click **Library Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="14a4e-160">Detta öppnar Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-160">This displays the Package Manager Console.</span></span>
3. <span data-ttu-id="14a4e-161">I fönstret för **Package Manager-konsolen** ställer du in **standardprojektet** till det nya projektet för konsolprogrammet. Sedan kör du följande kommando i konsolfönstret:</span><span class="sxs-lookup"><span data-stu-id="14a4e-161">In the **Package Manager Console** window, set the **Default project** to your new console application project, and then in the console window, execute the following command:</span></span>
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   <span data-ttu-id="14a4e-162">Då läggs en referens till i Azure Notification Hubs SDK med hjälp av <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs-NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="14a4e-162">This adds a reference to the Azure Notification Hubs SDK using the <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="14a4e-163">Öppna filen `Program.cs` och lägg till följande `using`-uttryck:</span><span class="sxs-lookup"><span data-stu-id="14a4e-163">Open the `Program.cs` file and add the following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="14a4e-164">Lägg till följande metod i `Program`-klassen:</span><span class="sxs-lookup"><span data-stu-id="14a4e-164">In the `Program` class, add the following method:</span></span>
   
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
   
    <span data-ttu-id="14a4e-165">Ersätt platshållaren `<hub name>` med namnet på den meddelandehubb som visas i portalen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-165">Make sure to replace the `<hub name>` placeholder with the name of the notification hub that appears in the portal.</span></span> <span data-ttu-id="14a4e-166">Dessutom måste du ersätta platshållaren för anslutningssträngen med den anslutningssträng som heter **DefaultFullSharedAccessSignature**. Du fick denna i avsnittet ”Konfigurera din meddelandehubb”.</span><span class="sxs-lookup"><span data-stu-id="14a4e-166">Also, replace the connection string placeholder with the connection string called **DefaultFullSharedAccessSignature** that you obtained in the section "Configure your notification hub."</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="14a4e-167">Kontrollera att du använder anslutningssträngen med **fullständig** åtkomst, inte enbart åtkomst för att **lyssna**.</span><span class="sxs-lookup"><span data-stu-id="14a4e-167">Make sure that you use the connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="14a4e-168">Strängen med lyssna-åtkomst har inte behörighet att skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="14a4e-168">The listen-access string does not have permissions to send push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="14a4e-169">Lägg till följande rad i `Main`-metoden:</span><span class="sxs-lookup"><span data-stu-id="14a4e-169">Add the following line in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="14a4e-170">Starta Windows Phone-emulatorn och stäng appen. Konfigurera konsolprogramprojektet som förvalt startprojekt och tryck sedan på tangenten `F5` för att köra appen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-170">With your Windows Phone emulator running and your app closed, set the console application project as the default startup project, and then press the `F5` key to run the app.</span></span>
   
    <span data-ttu-id="14a4e-171">Du får ett push-meddelandensom popup.</span><span class="sxs-lookup"><span data-stu-id="14a4e-171">You will receive a toast push notification.</span></span> <span data-ttu-id="14a4e-172">Appen läses in när du knackar på popup-banderollen.</span><span class="sxs-lookup"><span data-stu-id="14a4e-172">Tapping the toast banner loads the app.</span></span>

<span data-ttu-id="14a4e-173">Du hittar alla möjliga nyttolaster under ämnena [toast catalog] (katalog över popup-meddelanden) och [tile catalog] (katalog över paneler) på MSDN.</span><span class="sxs-lookup"><span data-stu-id="14a4e-173">You can find all the possible payloads in the [toast catalog] and [tile catalog] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="14a4e-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="14a4e-174">Next steps</span></span>
<span data-ttu-id="14a4e-175">I det här enkla exemplet skickade du push-meddelanden till alla dina Windows Phone 8-enheter.</span><span class="sxs-lookup"><span data-stu-id="14a4e-175">In this simple example, you broadcasted push notifications to all your Windows Phone 8 devices.</span></span> 

<span data-ttu-id="14a4e-176">Mer information om hur du riktar in dig på specifika användare finns i självstudiekursen [Använda Notification Hubs för att skicka push-meddelanden till användare].</span><span class="sxs-lookup"><span data-stu-id="14a4e-176">In order to target specific users, refer to the [Use Notification Hubs to push notifications to users] tutorial.</span></span> 

<span data-ttu-id="14a4e-177">Om du vill dela in användarna efter intressegrupper läser du [Använda Notification Hubs för att skicka de senaste nyheterna].</span><span class="sxs-lookup"><span data-stu-id="14a4e-177">If you want to segment your users by interest groups, you can read [Use Notification Hubs to send breaking news].</span></span> 

<span data-ttu-id="14a4e-178">Mer information om hur du använder Notification Hubs finns i [Riktlinjer om Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="14a4e-178">Learn more about how to use Notification Hubs in [Notification Hubs Guidance].</span></span>

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
<span data-ttu-id="14a4e-179">[Visual Studio 2012 Express för Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374</span><span class="sxs-lookup"><span data-stu-id="14a4e-179">[Visual Studio 2012 Express for Windows Phone]: https://go.microsoft.com/fwLink/p/?LinkID=268374</span></span>
<span data-ttu-id="14a4e-180">[Riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx</span><span class="sxs-lookup"><span data-stu-id="14a4e-180">[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx</span></span>
[MPNS authenticated mode]: http://msdn.microsoft.com/library/windowsphone/develop/ff941099(v=vs.105).aspx
<span data-ttu-id="14a4e-181">[Använda Notification Hubs för att skicka push-meddelanden till användare]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span><span class="sxs-lookup"><span data-stu-id="14a4e-181">[Use Notification Hubs to push notifications to users]: notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md</span></span>
<span data-ttu-id="14a4e-182">[Använda Notification Hubs för att skicka de senaste nyheterna]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md</span><span class="sxs-lookup"><span data-stu-id="14a4e-182">[Use Notification Hubs to send breaking news]: notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md</span></span>
<span data-ttu-id="14a4e-183">[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="14a4e-183">[toast catalog]: http://msdn.microsoft.com/library/windowsphone/develop/jj662938(v=vs.105).aspx</span></span>
<span data-ttu-id="14a4e-184">[tile catalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx</span><span class="sxs-lookup"><span data-stu-id="14a4e-184">[tile catalog]: http://msdn.microsoft.com/library/windowsphone/develop/hh202948(v=vs.105).aspx</span></span>
<span data-ttu-id="14a4e-185">[Notification Hubs – självstudiekurs för Windows Phone Silverlight]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp</span><span class="sxs-lookup"><span data-stu-id="14a4e-185">[Notification Hubs - Windows Phone Silverlight tutorial]: https://github.com/Azure/azure-notificationhubs-samples/tree/master/PushToSLPhoneApp</span></span>

