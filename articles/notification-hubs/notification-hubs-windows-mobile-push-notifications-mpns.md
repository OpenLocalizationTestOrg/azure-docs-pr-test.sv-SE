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
# <a name="sending-push-notifications-with-azure-notification-hubs-on-windows-phone"></a><span data-ttu-id="d0f3e-104">Skicka push-meddelanden med Azure Notification Hubs på Windows Phone</span><span class="sxs-lookup"><span data-stu-id="d0f3e-104">Sending push notifications with Azure Notification Hubs on Windows Phone</span></span>
[!INCLUDE [notification-hubs-selector-get-started](../../includes/notification-hubs-selector-get-started.md)]

## <a name="overview"></a><span data-ttu-id="d0f3e-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="d0f3e-105">Overview</span></span>
> [!NOTE]
> <span data-ttu-id="d0f3e-106">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="d0f3e-107">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="d0f3e-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span><span class="sxs-lookup"><span data-stu-id="d0f3e-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-windows-phone-get-started%2F).</span></span>
> 
> 

<span data-ttu-id="d0f3e-109">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa Windows Phone 8 eller Windows Phone 8.1 Silverlight-program.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-109">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa Windows Phone 8 or Windows Phone 8.1 Silverlight application.</span></span> <span data-ttu-id="d0f3e-110">Om du utvecklar för Windows Phone 8.1 (utan Silverlight), referera toohello [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) version.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-110">If you are targeting Windows Phone 8.1 (non-Silverlight), then refer toohello [Windows Universal](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) version.</span></span>
<span data-ttu-id="d0f3e-111">I den här självstudiekursen skapar du en tom Windows Phone 8-app som tar emot push-meddelanden med hjälp av hello Microsoft Push Notification Service (MPNS).</span><span class="sxs-lookup"><span data-stu-id="d0f3e-111">In this tutorial, you create a blank Windows Phone 8 app that receives push notifications by using hello Microsoft Push Notification Service (MPNS).</span></span> <span data-ttu-id="d0f3e-112">När du är klar, kommer du att kunna toouse din notification hub toobroadcast push-meddelanden tooall hello enheter som kör appen.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-112">When you're finished, you'll be able toouse your notification hub toobroadcast push notifications tooall hello devices running your app.</span></span>

> [!NOTE]
> <span data-ttu-id="d0f3e-113">hello Notification Hubs för Windows Phone SDK stöder inte användning av hello Windows Push Notification Service (WNS) med Windows Phone 8.1 Silverlight-appar.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-113">hello Notification Hubs Windows Phone SDK does not support using hello Windows Push Notification Service (WNS) with Windows Phone 8.1 Silverlight apps.</span></span> <span data-ttu-id="d0f3e-114">toouse WNS (istället för MPNS) med Windows Phone 8.1 Silverlight-appar, följ hello [Notification Hubs – självstudiekurs för Windows Phone Silverlight], som använder REST API: er.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-114">toouse WNS (instead of MPNS) with Windows Phone 8.1 Silverlight apps, follow hello [Notification Hubs - Windows Phone Silverlight tutorial], which uses REST APIs.</span></span>
> 
> 

<span data-ttu-id="d0f3e-115">hello kursen visar hello enkelt scenario för sändning med Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-115">hello tutorial demonstrates hello simple broadcast scenario in using Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d0f3e-116">Krav</span><span class="sxs-lookup"><span data-stu-id="d0f3e-116">Prerequisites</span></span>
<span data-ttu-id="d0f3e-117">Den här kursen kräver hello följande:</span><span class="sxs-lookup"><span data-stu-id="d0f3e-117">This tutorial requires hello following:</span></span>

* <span data-ttu-id="d0f3e-118">[Visual Studio 2012 Express för Windows Phone], eller en senare version.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-118">[Visual Studio 2012 Express for Windows Phone], or a later version.</span></span>

<span data-ttu-id="d0f3e-119">Du måste slutföra den här självstudiekursen innan du påbörjar någon annan kurs om Notification Hubs för Windows Phone 8-appar.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-119">Completing this tutorial is a prerequisite for all other Notification Hubs tutorials for Windows Phone 8 apps.</span></span>

## <a name="create-your-notification-hub"></a><span data-ttu-id="d0f3e-120">Skapa din meddelandehubb</span><span class="sxs-lookup"><span data-stu-id="d0f3e-120">Create your notification hub</span></span>
[!INCLUDE [notification-hubs-portal-create-new-hub](../../includes/notification-hubs-portal-create-new-hub.md)]

<ol start="6">
<li><p><span data-ttu-id="d0f3e-121">Klicka på hello <b>Notification Services</b> avsnitt (inom <i>inställningar</i>), klicka på <b>Windows Phone (MPNS)</b> och klicka sedan på hello <b>aktivera ej autentiserade push </b> kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-121">Click hello <b>Notification Services</b> section (within <i>Settings</i>), click on <b>Windows Phone (MPNS)</b> and then click hello <b>Enable unauthenticated push</b> check box.</span></span></p>
</li>
</ol>

&emsp;&emsp;![Azure-portalen – Aktivera ej autentiserade push-meddelanden](./media/notification-hubs-windows-phone-get-started/azure-portal-unauth.png)

<span data-ttu-id="d0f3e-123">Din hubb finns nu skapats och konfigurerats toosend oautentiserad meddelande för Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-123">Your hub is now created and configured toosend unauthenticated notification for Windows Phone.</span></span>

> [!NOTE]
> <span data-ttu-id="d0f3e-124">I den här kursen används MPNS i ej autentiserat läge.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-124">This tutorial uses MPNS in unauthenticated mode.</span></span> <span data-ttu-id="d0f3e-125">MPNS ej autentiserat läge har begränsningar för meddelanden som du kan skicka tooeach kanal.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-125">MPNS unauthenticated mode comes with restrictions on notifications that you can send tooeach channel.</span></span> <span data-ttu-id="d0f3e-126">Notification Hubs stöder [MPNS i autentiserat läge](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) genom att låta dig tooupload ditt certifikat.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-126">Notification Hubs supports [MPNS authenticated mode](http://msdn.microsoft.com/library/windowsphone/develop/ff941099.aspx) by allowing you tooupload your certificate.</span></span>
> 
> 

## <a name="connecting-your-app-toohello-notification-hub"></a><span data-ttu-id="d0f3e-127">Ansluta din app toohello notification hub</span><span class="sxs-lookup"><span data-stu-id="d0f3e-127">Connecting your app toohello notification hub</span></span>
1. <span data-ttu-id="d0f3e-128">Skapa en ny Windows Phone 8-app i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-128">In Visual Studio, create a new Windows Phone 8 application.</span></span>
   
       ![Visual Studio - New Project - Windows Phone App][13]
   
    <span data-ttu-id="d0f3e-129">I Visual Studio 2013 Update 2 eller senare kan du istället skapa en Silverlight-app för Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-129">In Visual Studio 2013 Update 2 or later, you instead create a Windows Phone Silverlight application.</span></span>
   
    ![Visual Studio – Nytt projekt – Tom app – Silverlight för Windows Phone][11]
2. <span data-ttu-id="d0f3e-131">Högerklicka på hello lösning i Visual Studio och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-131">In Visual Studio, right-click hello solution, and then click **Manage NuGet Packages**.</span></span>
   
    <span data-ttu-id="d0f3e-132">Detta visar hello **hantera NuGet-paket** dialogrutan.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-132">This displays hello **Manage NuGet Packages** dialog box.</span></span>
3. <span data-ttu-id="d0f3e-133">Sök efter `WindowsAzure.Messaging.Managed` och på **installera**, och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-133">Search for `WindowsAzure.Messaging.Managed` and click **Install**, and then accept hello terms of use.</span></span>
   
    ![Visual Studio – NuGet Package Manager][20]
   
    <span data-ttu-id="d0f3e-135">Detta hämtar, installerar och lägger till en Azure Messaging-biblioteket på referens toohello för Windows med hjälp av hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">NuGet-paketet WindowsAzure.Messaging.Managed</a>.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-135">This downloads, installs, and adds a reference toohello Azure Messaging library for Windows by using hello <a href="http://nuget.org/packages/WindowsAzure.Messaging.Managed/">WindowsAzure.Messaging.Managed NuGet package</a>.</span></span>
4. <span data-ttu-id="d0f3e-136">Öppna hello filen App.xaml.cs och Lägg till följande hello `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d0f3e-136">Open hello file App.xaml.cs and add hello following `using` statements:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
5. <span data-ttu-id="d0f3e-137">Lägg till hello följande kod hello överst i **Application_Launching** metod i App.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="d0f3e-137">Add hello following code at hello top of **Application_Launching** method in App.xaml.cs:</span></span>
   
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
   > <span data-ttu-id="d0f3e-138">Hej värdet **MyPushChannel** är ett index som har använt toolookup en befintlig kanal i hello [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) samling.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-138">hello value **MyPushChannel** is an index that is used toolookup an existing channel in hello [HttpNotificationChannel](https://msdn.microsoft.com/library/windows/apps/microsoft.phone.notification.httpnotificationchannel.aspx) collection.</span></span> <span data-ttu-id="d0f3e-139">Om det inte finns någon sådan där, skapar du en ny post med det namnet.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-139">If there isn't one there, create a new entry with that name.</span></span>
   > 
   > 
   
    <span data-ttu-id="d0f3e-140">Kontrollera att tooinsert hello namnet på anslutningssträngen NAV- och hello kallas **DefaultListenSharedAccessSignature** som du fick i hello föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-140">Make sure tooinsert hello name of your hub and hello connection string called **DefaultListenSharedAccessSignature** that you obtained in hello previous section.</span></span>
    <span data-ttu-id="d0f3e-141">Den här koden hämtar hello URI-kanalen för hello appen från MPNS och registrerar sedan denna URI med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-141">This code retrieves hello channel URI for hello app from MPNS, and then registers that channel URI with your notification hub.</span></span> <span data-ttu-id="d0f3e-142">Det garanterar även hello kanalen URI registreras i meddelandehubben varje gång hello-programmet startades.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-142">It also guarantees that hello channel URI is registered in your notification hub each time hello application is launched.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d0f3e-143">Den här självstudiekursen skickar en popup-meddelande toohello enhet.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-143">This tutorial sends a toast notification toohello device.</span></span> <span data-ttu-id="d0f3e-144">När du skickar ett panelmeddelande måste du istället anropa hello **BindToShellTile** metod på hello-kanal.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-144">When you send a tile notification, you must instead call hello **BindToShellTile** method on hello channel.</span></span> <span data-ttu-id="d0f3e-145">toosupport både popup-meddelande och panelen meddelanden, anropar du både **BindToShellTile** och **BindToShellToast**.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-145">toosupport both toast and tile notifications, call both **BindToShellTile** and  **BindToShellToast**.</span></span>
   > 
   > 
6. <span data-ttu-id="d0f3e-146">I Solution Explorer expanderar **egenskaper**öppnar hello `WMAppManifest.xml` filen, klickar på hello **funktioner** fliken och kontrollera att den hello **ID_CAP_PUSH_NOTIFICATION** är markerad.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-146">In Solution Explorer, expand **Properties**, open hello `WMAppManifest.xml` file, click hello **Capabilities** tab, and make sure that hello **ID_CAP_PUSH_NOTIFICATION** capability is checked.</span></span>
   
       ![Visual Studio - Windows Phone App Capabilities][14]
   
       This ensures that your app can receive push notifications. Without it, any attempt toosend a push notification toohello app will fail.
7. <span data-ttu-id="d0f3e-147">Tryck på hello `F5` viktiga toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-147">Press hello `F5` key toorun hello app.</span></span>
   
    <span data-ttu-id="d0f3e-148">Ett registreringsmeddelande visas i hello app.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-148">A registration message is displayed in hello app.</span></span>
8. <span data-ttu-id="d0f3e-149">Stäng hello app.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-149">Close hello app.</span></span>  
   
   > [!NOTE]
   > <span data-ttu-id="d0f3e-150">tooreceive en push-meddelandensom hello programmet måste inte körs i förgrunden hello.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-150">tooreceive a toast push notification, hello application must not be running in hello foreground.</span></span>
   > 
   > 

## <a name="send-push-notifications-from-your-backend"></a><span data-ttu-id="d0f3e-151">Skicka push-meddelanden från serverdelen</span><span class="sxs-lookup"><span data-stu-id="d0f3e-151">Send push notifications from your backend</span></span>
<span data-ttu-id="d0f3e-152">Du kan skicka push-meddelanden med Notification Hubs från alla serverdelar via hello offentliga <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST-gränssnittet</a>.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-152">You can send push notifications by using Notification Hubs from any backend via hello public <a href="http://msdn.microsoft.com/library/windowsazure/dn223264.aspx">REST interface</a>.</span></span> <span data-ttu-id="d0f3e-153">I den här självstudiekursen kommer du att skicka push-meddelanden via ett .NET-konsolprogram.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-153">In this tutorial, you send push notifications using a .NET console application.</span></span> 

<span data-ttu-id="d0f3e-154">Ett exempel på hur toosend push-meddelanden från en ASP.NET-WebAPI-serverdel som är integrerad med Notification Hubs, se [Azure Notification Hubs Meddelandeanvändare med .NET-serverdel](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span><span class="sxs-lookup"><span data-stu-id="d0f3e-154">For an example of how toosend push notifications from an ASP.NET WebAPI backend that's integrated with Notification Hubs, see [Azure Notification Hubs Notify Users with .NET backend](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md).</span></span>  

<span data-ttu-id="d0f3e-155">Ett exempel på hur toosend push-meddelanden med hjälp av hello [REST API: er](https://msdn.microsoft.com/library/azure/dn223264.aspx), kolla [hur toouse Notification Hubs från Java](notification-hubs-java-push-notification-tutorial.md) och [hur toouse Notification Hubs från PHP](notification-hubs-php-push-notification-tutorial.md) .</span><span class="sxs-lookup"><span data-stu-id="d0f3e-155">For an example of how toosend push notifications by using hello [REST APIs](https://msdn.microsoft.com/library/azure/dn223264.aspx), check out [How toouse Notification Hubs from Java](notification-hubs-java-push-notification-tutorial.md) and [How toouse Notification Hubs from PHP](notification-hubs-php-push-notification-tutorial.md).</span></span>

1. <span data-ttu-id="d0f3e-156">Högerklicka på hello lösning, Välj **Lägg till** och **nytt projekt...** , och sedan under **Visual C#**, klickar du på **Windows** och **konsolprogram**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-156">Right-click hello solution, select **Add** and **New Project...**, and then under **Visual C#**, click **Windows** and **Console Application**, and click **OK**.</span></span>
   
       ![Visual Studio - New Project - Console Application][6]
   
    <span data-ttu-id="d0f3e-157">Detta lägger till en ny Visual C# konsolen programmet toohello lösning.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-157">This adds a new Visual C# console application toohello solution.</span></span> <span data-ttu-id="d0f3e-158">Du kan också göra detta i en separat lösning.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-158">You can also do this in a separate solution.</span></span>
2. <span data-ttu-id="d0f3e-159">Klicka på **Verktyg**, **Library Package Manager** och slutligen **Package Manager-konsolen**.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-159">Click **Tools**, click **Library Package Manager**, and then click **Package Manager Console**.</span></span>
   
    <span data-ttu-id="d0f3e-160">Detta visar hello Package Manager-konsolen.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-160">This displays hello Package Manager Console.</span></span>
3. <span data-ttu-id="d0f3e-161">I hello **Pakethanterarkonsolen** fönster, ange hello **standardprojektet** tooyour nya konsolen projektet och sedan köra hello följande kommando i konsolfönstret hello:</span><span class="sxs-lookup"><span data-stu-id="d0f3e-161">In hello **Package Manager Console** window, set hello **Default project** tooyour new console application project, and then in hello console window, execute hello following command:</span></span>
   
       Install-Package Microsoft.Azure.NotificationHubs
   
   <span data-ttu-id="d0f3e-162">Detta lägger till en referens toohello Azure Notification Hubs SDK med hjälp av hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet-paketet</a>.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-162">This adds a reference toohello Azure Notification Hubs SDK using hello <a href="http://www.nuget.org/packages/Microsoft.Azure.NotificationHubs/">Microsoft.Azure.Notification Hubs NuGet package</a>.</span></span>
4. <span data-ttu-id="d0f3e-163">Öppna hello `Program.cs` och Lägg till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="d0f3e-163">Open hello `Program.cs` file and add hello following `using` statement:</span></span>
   
        using Microsoft.Azure.NotificationHubs;
5. <span data-ttu-id="d0f3e-164">I hello `Program` klassen, Lägg till följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="d0f3e-164">In hello `Program` class, add hello following method:</span></span>
   
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
   
    <span data-ttu-id="d0f3e-165">Se till att tooreplace hello `<hub name>` med hello namnet på hello meddelandehubb som visas i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-165">Make sure tooreplace hello `<hub name>` placeholder with hello name of hello notification hub that appears in hello portal.</span></span> <span data-ttu-id="d0f3e-166">Dessutom måste du ersätta hello anslutning sträng platshållaren med hello anslutningssträng som heter **DefaultFullSharedAccessSignature** att du fick i hello avsnittet ”Konfigurera din meddelandehubb”.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-166">Also, replace hello connection string placeholder with hello connection string called **DefaultFullSharedAccessSignature** that you obtained in hello section "Configure your notification hub."</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="d0f3e-167">Kontrollera att du använder hello-anslutningssträngen med **fullständig** åt inte **lyssna** åtkomst.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-167">Make sure that you use hello connection string with **Full** access, not **Listen** access.</span></span> <span data-ttu-id="d0f3e-168">hello strängen med lyssna-åtkomst har inte behörighet toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-168">hello listen-access string does not have permissions toosend push notifications.</span></span>
   > 
   > 
6. <span data-ttu-id="d0f3e-169">Lägg till följande rad i hello din `Main` metoden:</span><span class="sxs-lookup"><span data-stu-id="d0f3e-169">Add hello following line in your `Main` method:</span></span>
   
         SendNotificationAsync();
         Console.ReadLine();
7. <span data-ttu-id="d0f3e-170">Med Windows Phone-emulatorn och din app stängd, Ställ in hello projektet för konsolprogrammet som hello standard Startprojekt och tryck sedan på hello `F5` viktiga toorun hello app.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-170">With your Windows Phone emulator running and your app closed, set hello console application project as hello default startup project, and then press hello `F5` key toorun hello app.</span></span>
   
    <span data-ttu-id="d0f3e-171">Du får ett push-meddelandensom popup.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-171">You will receive a toast push notification.</span></span> <span data-ttu-id="d0f3e-172">Om du knackar på popup-banderollen hello läser in hello app.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-172">Tapping hello toast banner loads hello app.</span></span>

<span data-ttu-id="d0f3e-173">Du hittar alla möjliga nyttolaster för hello i hello [toast catalog] och [panelen katalog] avsnitt på MSDN.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-173">You can find all hello possible payloads in hello [toast catalog] and [tile catalog] topics on MSDN.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d0f3e-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d0f3e-174">Next steps</span></span>
<span data-ttu-id="d0f3e-175">I det här enkla exemplet skickade du push-meddelanden tooall din Windows Phone 8-enheter.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-175">In this simple example, you broadcasted push notifications tooall your Windows Phone 8 devices.</span></span> 

<span data-ttu-id="d0f3e-176">I order tootarget specifika användare, se toohello [använda Notification Hubs toopush meddelanden toousers] kursen.</span><span class="sxs-lookup"><span data-stu-id="d0f3e-176">In order tootarget specific users, refer toohello [Use Notification Hubs toopush notifications toousers] tutorial.</span></span> 

<span data-ttu-id="d0f3e-177">Om du vill att toosegment användarna efter intressegrupper, kan du läsa [använda Notification Hubs toosend senaste nytt].</span><span class="sxs-lookup"><span data-stu-id="d0f3e-177">If you want toosegment your users by interest groups, you can read [Use Notification Hubs toosend breaking news].</span></span> 

<span data-ttu-id="d0f3e-178">Läs mer om hur toouse Meddelandehubbar i [riktlinjer om Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="d0f3e-178">Learn more about how toouse Notification Hubs in [Notification Hubs Guidance].</span></span>

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

