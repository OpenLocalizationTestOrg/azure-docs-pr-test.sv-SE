---
title: Skicka push-meddelanden med Azure Notification Hubs och Node.js
description: "Lär dig hur du använder Notification Hubs för att skicka push-meddelanden från en Node.js-program."
keywords: push-meddelande, push notifications,node.js push ios push
services: notification-hubs
documentationcenter: nodejs
author: ysxu
manager: erikre
editor: 
ms.assetid: ded4749c-6c39-4ff8-b2cf-1927b3e92f93
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: javascript
ms.topic: article
ms.date: 10/25/2016
ms.author: yuaxu
ms.openlocfilehash: dc4987b16b2e930641c6c90eff8b65c1bf8d573c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a><span data-ttu-id="43600-104">Skicka push-meddelanden med Azure Notification Hubs och Node.js</span><span class="sxs-lookup"><span data-stu-id="43600-104">Sending push notifications with Azure Notification Hubs and Node.js</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a><span data-ttu-id="43600-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="43600-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="43600-106">Du måste ha ett aktivt Azure-konto för att slutföra den här kursen.</span><span class="sxs-lookup"><span data-stu-id="43600-106">To complete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="43600-107">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="43600-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="43600-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span><span class="sxs-lookup"><span data-stu-id="43600-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span></span>
> 
> 

<span data-ttu-id="43600-109">Den här guiden visar hur du skickar push-meddelanden med hjälp av Azure Notification Hubs direkt från ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="43600-109">This guide will show you how to send push notifications with the help of Azure Notification Hubs directly from a Node.js application.</span></span> 

<span data-ttu-id="43600-110">Scenarier som tas upp inkluderar skicka push-meddelanden till program på följande plattformar:</span><span class="sxs-lookup"><span data-stu-id="43600-110">The scenarios covered include sending push notifications to applications on the following platforms:</span></span>

* <span data-ttu-id="43600-111">Android</span><span class="sxs-lookup"><span data-stu-id="43600-111">Android</span></span>
* <span data-ttu-id="43600-112">iOS</span><span class="sxs-lookup"><span data-stu-id="43600-112">iOS</span></span>
* <span data-ttu-id="43600-113">Windows Phone</span><span class="sxs-lookup"><span data-stu-id="43600-113">Windows Phone</span></span>
* <span data-ttu-id="43600-114">Universella Windows-plattformen</span><span class="sxs-lookup"><span data-stu-id="43600-114">Universal Windows Platform</span></span> 

<span data-ttu-id="43600-115">Mer information om notification hubs finns i [nästa steg](#next) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="43600-115">For more information on notification hubs, see the [Next Steps](#next) section.</span></span>

## <a name="what-are-notification-hubs"></a><span data-ttu-id="43600-116">Vad är Notification Hub?</span><span class="sxs-lookup"><span data-stu-id="43600-116">What are Notification Hubs?</span></span>
<span data-ttu-id="43600-117">Azure Notification Hubs innehåller en lätt att använda flera plattformar, skalbar infrastruktur för att skicka push-meddelanden till mobila enheter.</span><span class="sxs-lookup"><span data-stu-id="43600-117">Azure Notification Hubs provide an easy-to-use, multi-platform, scalable infrastructure for sending push notifications to mobile devices.</span></span> <span data-ttu-id="43600-118">Mer information om-infrastruktur, finns det [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="43600-118">For details on the service infrastructure, see the [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) page.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="43600-119">Skapa en Node.js-program</span><span class="sxs-lookup"><span data-stu-id="43600-119">Create a Node.js Application</span></span>
<span data-ttu-id="43600-120">Det första steget i den här kursen är att skapa en ny tom Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="43600-120">The first step in this tutorial is creating a new blank Node.js application.</span></span> <span data-ttu-id="43600-121">Instruktioner om hur du skapar en Node.js-program finns i [skapa och distribuera ett Node.js-program till Azure-webbplats][nodejswebsite], [Node.js Molntjänsten] [ Node.js Cloud Service] med Windows PowerShell eller [webbplatsen med WebMatrix].</span><span class="sxs-lookup"><span data-stu-id="43600-121">For instructions on creating a Node.js application, see [Create and deploy a Node.js application to Azure Web Site][nodejswebsite], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or [Web Site with WebMatrix].</span></span>

## <a name="configure-your-application-to-use-notification-hubs"></a><span data-ttu-id="43600-122">Konfigurera ditt program du använder Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="43600-122">Configure Your Application to Use Notification Hubs</span></span>
<span data-ttu-id="43600-123">Om du vill använda Azure Notification Hubs, du behöver hämta och använda Node.js [azure-paketet](https://www.npmjs.com/package/azure), som innehåller en inbyggd grupp helper bibliotek som kommunicerar med push notification REST-tjänster.</span><span class="sxs-lookup"><span data-stu-id="43600-123">To use Azure Notification Hubs, you need to download and use the Node.js [azure package](https://www.npmjs.com/package/azure), which includes a built-in set of helper libraries that communicate with the push notification REST services.</span></span>

### <a name="use-node-package-manager-npm-to-obtain-the-package"></a><span data-ttu-id="43600-124">Använd noden Package Manager (NPM) för att hämta paketet</span><span class="sxs-lookup"><span data-stu-id="43600-124">Use Node Package Manager (NPM) to obtain the package</span></span>
1. <span data-ttu-id="43600-125">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Linux) och navigera till mappen där du skapade ditt tomt program.</span><span class="sxs-lookup"><span data-stu-id="43600-125">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Linux) and navigate to the folder where you created your blank application.</span></span>
2. <span data-ttu-id="43600-126">Typen **npm installera azure-sb** i kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="43600-126">Type **npm install azure-sb** in the command window.</span></span>
3. <span data-ttu-id="43600-127">Du kan köra manuellt på **ls** eller **dir** kommando för att kontrollera att en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="43600-127">You can manually run the **ls** or **dir** command to verify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="43600-128">Inuti den mappen hitta den **azure** paket som innehåller de bibliotek som du behöver åtkomst till Meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="43600-128">Inside that folder, find the **azure** package, which contains the libraries you need to access the Notification Hub.</span></span>

> [!NOTE]
> <span data-ttu-id="43600-129">Du kan lära dig mer om att installera NPM på officiellt [NPM blogg](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span><span class="sxs-lookup"><span data-stu-id="43600-129">You can learn more about installing NPM on the official [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span></span> 
> 
> 

### <a name="import-the-module"></a><span data-ttu-id="43600-130">Importera modulen</span><span class="sxs-lookup"><span data-stu-id="43600-130">Import the module</span></span>
<span data-ttu-id="43600-131">Med hjälp av en textredigerare, Lägg till följande överst i den **server.js** filen för programmet:</span><span class="sxs-lookup"><span data-stu-id="43600-131">Using a text editor, add the following to the top of the **server.js** file of the application:</span></span>

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a><span data-ttu-id="43600-132">Skapa en anslutning med Azure Notification Hub</span><span class="sxs-lookup"><span data-stu-id="43600-132">Setup an Azure Notification Hub connection</span></span>
<span data-ttu-id="43600-133">Den **NotificationHubService** objekt kan du arbeta med notification hubs.</span><span class="sxs-lookup"><span data-stu-id="43600-133">The **NotificationHubService** object lets you work with notification hubs.</span></span> <span data-ttu-id="43600-134">Följande kod skapar en **NotificationHubService** objekt för nofication hubben med namnet **hubname**.</span><span class="sxs-lookup"><span data-stu-id="43600-134">The following code creates a **NotificationHubService** object for the nofication hub named **hubname**.</span></span> <span data-ttu-id="43600-135">Lägg till den övre delen av den **server.js** filen när instruktionen för att importera modulen för azure:</span><span class="sxs-lookup"><span data-stu-id="43600-135">Add it near the top of the **server.js** file, after the statement to import the azure module:</span></span>

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

<span data-ttu-id="43600-136">Anslutningen **connectionstring** värde kan hämtas från den [Azure Portal] genom att utföra följande steg:</span><span class="sxs-lookup"><span data-stu-id="43600-136">The connection **connectionstring** value can be obtained from the [Azure Portal] by performing the following steps:</span></span>

1. <span data-ttu-id="43600-137">I det vänstra navigeringsfönstret klickar du på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="43600-137">In the left navigation pane, click **Browse**.</span></span>
2. <span data-ttu-id="43600-138">Välj **Meddelandehubbar**, och sen hittar du vill använda för hubben.</span><span class="sxs-lookup"><span data-stu-id="43600-138">Select **Notification Hubs**, and then find the hub you wish to use for the sample.</span></span> <span data-ttu-id="43600-139">Du kan referera till den [Windows Store komma igång-kursen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) om du behöver hjälp med att skapa en ny Meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="43600-139">You can refer to the [Windows Store Getting Started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) if you need help creating a new Notification Hub.</span></span>
3. <span data-ttu-id="43600-140">Välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="43600-140">Select **Settings**.</span></span>
4. <span data-ttu-id="43600-141">Klicka på **åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="43600-141">Click on **Access Policies**.</span></span> <span data-ttu-id="43600-142">Båda anslutningssträngar för delade och fullständig åtkomst visas.</span><span class="sxs-lookup"><span data-stu-id="43600-142">You will see both shared and full access connection strings.</span></span>

![Azure Portal – Notification hub](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> <span data-ttu-id="43600-144">Du kan också hämta den anslutningssträng genom att använda den **Get-AzureSbNamespace** cmdlet som tillhandahålls av [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller **azure sb namnområde visa** kommandot med de [Azure kommandoradsverktyget gränssnitt (Azure CLI)](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="43600-144">You can also retrieve the connection string using the **Get-AzureSbNamespace** cmdlet provided by [Azure PowerShell](/powershell/azureps-cmdlets-docs) or the **azure sb namespace show** command with the [Azure Command-Line Interface (Azure CLI)](../cli-install-nodejs.md).</span></span>
> 
> 

## <a name="general-architecture"></a><span data-ttu-id="43600-145">Allmän arkitektur</span><span class="sxs-lookup"><span data-stu-id="43600-145">General architecture</span></span>
<span data-ttu-id="43600-146">Den **NotificationHubService** objektet innehåller följande objektinstanser för att skicka push-meddelanden till specifika enheter och program:</span><span class="sxs-lookup"><span data-stu-id="43600-146">The **NotificationHubService** object exposes the following object instances for sending push notifications to specific devices and applications:</span></span>

* <span data-ttu-id="43600-147">**Android** – Använd den **GcmService** -objekt som finns på **notificationHubService.gcm**</span><span class="sxs-lookup"><span data-stu-id="43600-147">**Android** - use the **GcmService** object, which is available at **notificationHubService.gcm**</span></span>
* <span data-ttu-id="43600-148">**iOS** -använder den **ApnsService** -objekt som är tillgängliga i **notificationHubService.apns**</span><span class="sxs-lookup"><span data-stu-id="43600-148">**iOS** - use the **ApnsService** object, which is accessible at **notificationHubService.apns**</span></span>
* <span data-ttu-id="43600-149">**Windows Phone** -använder den **MpnsService** -objekt som finns på **notificationHubService.mpns**</span><span class="sxs-lookup"><span data-stu-id="43600-149">**Windows Phone** - use the **MpnsService** object, which is available at **notificationHubService.mpns**</span></span>
* <span data-ttu-id="43600-150">**Uwp** -använder den **WnsService** -objekt som finns på **notificationHubService.wns**</span><span class="sxs-lookup"><span data-stu-id="43600-150">**Universal Windows Platform** - use the **WnsService** object, which is available at **notificationHubService.wns**</span></span>

### <a name="how-to-send-push-notifications-to-android-applications"></a><span data-ttu-id="43600-151">Så här: skicka push-meddelanden till Android-program</span><span class="sxs-lookup"><span data-stu-id="43600-151">How to: Send push notifications to Android applications</span></span>
<span data-ttu-id="43600-152">Den **GcmService** objektet innehåller en **skicka** metod som kan användas för att skicka push-meddelanden till Android-program.</span><span class="sxs-lookup"><span data-stu-id="43600-152">The **GcmService** object provides a **send** method that can be used to send push notifications to Android applications.</span></span> <span data-ttu-id="43600-153">Den **skicka** metoden accepterar följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="43600-153">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="43600-154">**Taggar** -taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="43600-154">**Tags** - the tag identifier.</span></span> <span data-ttu-id="43600-155">Om ingen tagg anges, skickas meddelandet till alla klienter.</span><span class="sxs-lookup"><span data-stu-id="43600-155">If no tag is provided, the notification will be sent to all clients.</span></span>
* <span data-ttu-id="43600-156">**Nyttolasten** -meddelandets JSON eller raw sträng nyttolast.</span><span class="sxs-lookup"><span data-stu-id="43600-156">**Payload** - the message's JSON or raw string payload.</span></span>
* <span data-ttu-id="43600-157">**Motringning** -Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="43600-157">**Callback** - the callback function.</span></span>

<span data-ttu-id="43600-158">Mer information om nyttolastformatet finns i **nyttolast** avsnitt i den [implementera GCM-Server](http://developer.android.com/google/gcm/server.html#payload) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="43600-158">For more information on the payload format, see the **Payload** section of the [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload) document.</span></span>

<span data-ttu-id="43600-159">I följande kod används den **GcmService** instans som exponeras av den **NotificationHubService** att skicka ett push-meddelande till alla registrerade klienter.</span><span class="sxs-lookup"><span data-stu-id="43600-159">The following code uses the **GcmService** instance exposed by the **NotificationHubService** to send a push notification to all registered clients.</span></span>

    var payload = {
      data: {
        message: 'Hello!'
      }
    };
    notificationHubService.gcm.send(null, payload, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-ios-applications"></a><span data-ttu-id="43600-160">Så här: skicka push-meddelanden i iOS-program</span><span class="sxs-lookup"><span data-stu-id="43600-160">How to: Send push notifications to iOS applications</span></span>
<span data-ttu-id="43600-161">Samma som med Android program som beskrivs ovan, den **ApnsService** objektet innehåller en **skicka** metod som kan användas för att skicka push-meddelanden till iOS-program.</span><span class="sxs-lookup"><span data-stu-id="43600-161">Same as with Android applications described above, the **ApnsService** object provides a **send** method that can be used to send push notifications to iOS applications.</span></span> <span data-ttu-id="43600-162">Den **skicka** metoden accepterar följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="43600-162">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="43600-163">**Taggar** -taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="43600-163">**Tags** - the tag identifier.</span></span> <span data-ttu-id="43600-164">Om ingen tagg anges, skickas meddelandet till alla klienter.</span><span class="sxs-lookup"><span data-stu-id="43600-164">If no tag is provided, the notification will be sent to all clients.</span></span>
* <span data-ttu-id="43600-165">**Nyttolasten** -meddelandets JSON eller sträng nyttolast.</span><span class="sxs-lookup"><span data-stu-id="43600-165">**Payload** - the message's JSON or string payload.</span></span>
* <span data-ttu-id="43600-166">**Motringning** -Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="43600-166">**Callback** - the callback function.</span></span>

<span data-ttu-id="43600-167">Mer information om nyttolastformatet finns i **meddelande nyttolast** avsnitt i den [lokala Programmeringsguide och Push Notification](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="43600-167">For more information the payload format, see The **Notification Payload** section of the [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) document.</span></span>

<span data-ttu-id="43600-168">I följande kod används den **ApnsService** instans som exponeras av den **NotificationHubService** att skicka ett meddelande till alla klienter:</span><span class="sxs-lookup"><span data-stu-id="43600-168">The following code uses the **ApnsService** instance exposed by the **NotificationHubService** to send an alert message to all clients:</span></span>

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-windows-phone-applications"></a><span data-ttu-id="43600-169">Så här: skicka push-meddelanden till Windows Phone-program</span><span class="sxs-lookup"><span data-stu-id="43600-169">How to: Send push notifications to Windows Phone applications</span></span>
<span data-ttu-id="43600-170">Den **MpnsService** objektet innehåller en **skicka** metod som kan användas för att skicka push-meddelanden till Windows Phone-program.</span><span class="sxs-lookup"><span data-stu-id="43600-170">The **MpnsService** object provides a **send** method that can be used to send push notifications to Windows Phone applications.</span></span> <span data-ttu-id="43600-171">Den **skicka** metoden accepterar följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="43600-171">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="43600-172">**Taggar** -taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="43600-172">**Tags** - the tag identifier.</span></span> <span data-ttu-id="43600-173">Om ingen tagg anges, skickas meddelandet till alla klienter.</span><span class="sxs-lookup"><span data-stu-id="43600-173">If no tag is provided, the notification will be sent to all clients.</span></span>
* <span data-ttu-id="43600-174">**Nyttolasten** -meddelandets nyttolast för XML.</span><span class="sxs-lookup"><span data-stu-id="43600-174">**Payload** - the message's XML payload.</span></span>
* <span data-ttu-id="43600-175">**Målnamn**  -  `toast` för popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="43600-175">**TargetName** - `toast` for toast notifications.</span></span> <span data-ttu-id="43600-176">`token`för sida vid sida-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="43600-176">`token` for tile notifications.</span></span>
* <span data-ttu-id="43600-177">**NotificationClass** -prioriteten för meddelandet.</span><span class="sxs-lookup"><span data-stu-id="43600-177">**NotificationClass** - The priority of the notification.</span></span> <span data-ttu-id="43600-178">Finns det **HTTP-huvudet element** avsnitt i den [Push-meddelanden från en server](http://msdn.microsoft.com/library/hh221551.aspx) dokument för giltiga värden.</span><span class="sxs-lookup"><span data-stu-id="43600-178">See the **HTTP Header Elements** section of the [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) document for valid values.</span></span>
* <span data-ttu-id="43600-179">**Alternativ** – valfritt begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="43600-179">**Options** - optional request headers.</span></span>
* <span data-ttu-id="43600-180">**Motringning** -Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="43600-180">**Callback** - the callback function.</span></span>

<span data-ttu-id="43600-181">En lista över giltiga **TargetName**, **NotificationClass** och Rubrikalternativ, ta en titt på [Push-meddelanden från en server](http://msdn.microsoft.com/library/hh221551.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="43600-181">For a list of valid **TargetName**, **NotificationClass** and header options, check out the [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) page.</span></span>

<span data-ttu-id="43600-182">Följande exempel koden använder den **MpnsService** instans som exponeras av den **NotificationHubService** att skicka ett popup-push-meddelande:</span><span class="sxs-lookup"><span data-stu-id="43600-182">The following sample code uses the **MpnsService** instance exposed by the **NotificationHubService** to send a toast push notification:</span></span>

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-to-universal-windows-platform-uwp-applications"></a><span data-ttu-id="43600-183">Så här: skicka push-meddelanden för universella Windowsplattformen (UWP)-program</span><span class="sxs-lookup"><span data-stu-id="43600-183">How to: Send push notifications to Universal Windows Platform (UWP) applications</span></span>
<span data-ttu-id="43600-184">Den **WnsService** objektet innehåller en **skicka** metod som kan användas för att skicka push-meddelanden till Uwp-program.</span><span class="sxs-lookup"><span data-stu-id="43600-184">The **WnsService** object provides a **send** method that can be used to send push notifications to Universal Windows Platform applications.</span></span>  <span data-ttu-id="43600-185">Den **skicka** metoden accepterar följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="43600-185">The **send** method accepts the following parameters:</span></span>

* <span data-ttu-id="43600-186">**Taggar** -taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="43600-186">**Tags** - the tag identifier.</span></span> <span data-ttu-id="43600-187">Om ingen tagg anges, skickas meddelandet till alla registrerade klienter.</span><span class="sxs-lookup"><span data-stu-id="43600-187">If no tag is provided, the notification will be sent to all registered clients.</span></span>
* <span data-ttu-id="43600-188">**Nyttolasten** -nyttolast för XML-meddelande.</span><span class="sxs-lookup"><span data-stu-id="43600-188">**Payload** - the XML message payload.</span></span>
* <span data-ttu-id="43600-189">**Typen** -meddelandetyp.</span><span class="sxs-lookup"><span data-stu-id="43600-189">**Type** - the notification type.</span></span>
* <span data-ttu-id="43600-190">**Alternativ** – valfritt begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="43600-190">**Options** - optional request headers.</span></span>
* <span data-ttu-id="43600-191">**Motringning** -Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="43600-191">**Callback** - the callback function.</span></span>

<span data-ttu-id="43600-192">En lista över giltiga typer och begärandehuvuden finns [Push notification service begärande- och svarshuvuden](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span><span class="sxs-lookup"><span data-stu-id="43600-192">For a list of valid types and request headers, see [Push notification service request and response headers](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span></span>

<span data-ttu-id="43600-193">I följande kod används den **WnsService** instans som exponeras av den **NotificationHubService** att skicka ett popup-push-meddelande till en UWP-app:</span><span class="sxs-lookup"><span data-stu-id="43600-193">The following code uses the **WnsService** instance exposed by the **NotificationHubService** to send a toast push notification to a UWP app:</span></span>

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a><span data-ttu-id="43600-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="43600-194">Next Steps</span></span>
<span data-ttu-id="43600-195">Fragmenten exemplet ovan kan du enkelt skapa infrastruktur för att leverera push-meddelanden till en mängd olika enheter.</span><span class="sxs-lookup"><span data-stu-id="43600-195">The sample snippets above allow you to easily build service infrastructure to deliver push notifications to a wide variety of devices.</span></span> <span data-ttu-id="43600-196">Nu när du har lärt dig grunderna i att använda Notification Hubs med node.js kan du följa dessa länkar om du vill veta mer om hur du kan utöka dessa ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="43600-196">Now that you've learned the basics of using Notification Hubs with node.js, follow these links to learn more about how you can extend these capabilities further.</span></span>

* <span data-ttu-id="43600-197">Finns i MSDN-referens för [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span><span class="sxs-lookup"><span data-stu-id="43600-197">See the MSDN Reference for [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span></span>
* <span data-ttu-id="43600-198">Besök den [Azure SDK för noden] lagringsplats på GitHub för fler exempel och implementeringsdetaljer.</span><span class="sxs-lookup"><span data-stu-id="43600-198">Visit the [Azure SDK for Node] repository on GitHub for more samples and implementation details.</span></span>

<span data-ttu-id="43600-199">[Azure SDK för noden]: https://github.com/WindowsAzure/azure-sdk-for-node</span><span class="sxs-lookup"><span data-stu-id="43600-199">[Azure SDK for Node]: https://github.com/WindowsAzure/azure-sdk-for-node</span></span>
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain the Default Management Credentials for the Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application to Use Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages to a Topic]: #How_to_Send_Messages_to_a_Topic
[How to: Receive Messages from a Subscription]: #How_to_Receive_Messages_from_a_Subscription
[How to: Handle Application Crashes and Unreadable Messages]: #How_to_Handle_Application_Crashes_and_Unreadable_Messages
[How to: Delete Topics and Subscriptions]: #How_to_Delete_Topics_and_Subscriptions
[1]: #Next_Steps
[Topic Concepts]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-topics-01.png
[Azure Classic Portal]: http://manage.windowsazure.com
[image]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-03.png
[2]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-04.png
[3]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-05.png
[4]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-06.png
[5]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/sb-queues-07.png
[SqlFilter.SqlExpression]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.sqlexpression.aspx
[Azure Service Bus Notification Hubs]: http://msdn.microsoft.com/library/windowsazure/jj927170.aspx
[SqlFilter]: http://msdn.microsoft.com/library/windowsazure/microsoft.servicebus.messaging.sqlfilter.aspx
<span data-ttu-id="43600-200">[webbplatsen med WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/</span><span class="sxs-lookup"><span data-stu-id="43600-200">[Web Site with WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/</span></span>
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
<span data-ttu-id="43600-201">[Azure Portal]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="43600-201">[Azure Portal]: https://portal.azure.com</span></span>
