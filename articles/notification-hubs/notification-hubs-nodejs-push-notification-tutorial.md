---
title: aaaSending push-meddelanden med Azure Notification Hub och Node.js
description: "Lär dig hur toouse Meddelandehubbar toosend push-meddelanden från ett Node.js-program."
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
ms.openlocfilehash: 151d224fa6dd07e4acdc3a4887c4e95ee03168c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="sending-push-notifications-with-azure-notification-hubs-and-nodejs"></a><span data-ttu-id="f0770-104">Skicka push-meddelanden med Azure Notification Hubs och Node.js</span><span class="sxs-lookup"><span data-stu-id="f0770-104">Sending push notifications with Azure Notification Hubs and Node.js</span></span>
[!INCLUDE [notification-hubs-backend-how-to-selector](../../includes/notification-hubs-backend-how-to-selector.md)]

## <a name="overview"></a><span data-ttu-id="f0770-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="f0770-105">Overview</span></span>
> [!IMPORTANT]
> <span data-ttu-id="f0770-106">toocomplete den här självstudiekursen kommer du måste ha ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f0770-106">toocomplete this tutorial, you must have an active Azure account.</span></span> <span data-ttu-id="f0770-107">Om du inte har något konto kan skapa du ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="f0770-107">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="f0770-108">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span><span class="sxs-lookup"><span data-stu-id="f0770-108">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A643EE910&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fen-us%2Fdocumentation%2Farticles%2Fnotification-hubs-nodejs-how-to-use-notification-hubs).</span></span>
> 
> 

<span data-ttu-id="f0770-109">Den här guiden visar hur toosend push-meddelanden med hjälp av hello i Azure Notification Hubs direkt från ett Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="f0770-109">This guide will show you how toosend push notifications with hello help of Azure Notification Hubs directly from a Node.js application.</span></span> 

<span data-ttu-id="f0770-110">hello scenarier beskrivs skickar push-meddelanden tooapplications på hello följande plattformar:</span><span class="sxs-lookup"><span data-stu-id="f0770-110">hello scenarios covered include sending push notifications tooapplications on hello following platforms:</span></span>

* <span data-ttu-id="f0770-111">Android</span><span class="sxs-lookup"><span data-stu-id="f0770-111">Android</span></span>
* <span data-ttu-id="f0770-112">iOS</span><span class="sxs-lookup"><span data-stu-id="f0770-112">iOS</span></span>
* <span data-ttu-id="f0770-113">Windows Phone</span><span class="sxs-lookup"><span data-stu-id="f0770-113">Windows Phone</span></span>
* <span data-ttu-id="f0770-114">Universella Windows-plattformen</span><span class="sxs-lookup"><span data-stu-id="f0770-114">Universal Windows Platform</span></span> 

<span data-ttu-id="f0770-115">Mer information om notification hubs finns hello [nästa steg](#next) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="f0770-115">For more information on notification hubs, see hello [Next Steps](#next) section.</span></span>

## <a name="what-are-notification-hubs"></a><span data-ttu-id="f0770-116">Vad är Notification Hub?</span><span class="sxs-lookup"><span data-stu-id="f0770-116">What are Notification Hubs?</span></span>
<span data-ttu-id="f0770-117">Azure Notification Hubs innehåller en lätt att använda flera plattformar, skalbar infrastruktur för att skicka push-meddelanden toomobile enheter.</span><span class="sxs-lookup"><span data-stu-id="f0770-117">Azure Notification Hubs provide an easy-to-use, multi-platform, scalable infrastructure for sending push notifications toomobile devices.</span></span> <span data-ttu-id="f0770-118">Mer information om hello-infrastruktur, finns hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="f0770-118">For details on hello service infrastructure, see hello [Azure Notification Hubs](http://msdn.microsoft.com/library/windowsazure/jj927170.aspx) page.</span></span>

## <a name="create-a-nodejs-application"></a><span data-ttu-id="f0770-119">Skapa en Node.js-program</span><span class="sxs-lookup"><span data-stu-id="f0770-119">Create a Node.js Application</span></span>
<span data-ttu-id="f0770-120">hello första steget i den här kursen är att skapa en ny tom Node.js-program.</span><span class="sxs-lookup"><span data-stu-id="f0770-120">hello first step in this tutorial is creating a new blank Node.js application.</span></span> <span data-ttu-id="f0770-121">Instruktioner om hur du skapar en Node.js-program finns i [skapa och distribuera en Node.js-programmet tooAzure webbplats][nodejswebsite], [Node.js Molntjänsten] [ Node.js Cloud Service] med Windows PowerShell eller [webbplatsen med WebMatrix].</span><span class="sxs-lookup"><span data-stu-id="f0770-121">For instructions on creating a Node.js application, see [Create and deploy a Node.js application tooAzure Web Site][nodejswebsite], [Node.js Cloud Service][Node.js Cloud Service] using Windows PowerShell, or [Web Site with WebMatrix].</span></span>

## <a name="configure-your-application-toouse-notification-hubs"></a><span data-ttu-id="f0770-122">Konfigurera ditt program tooUse Notification Hubs</span><span class="sxs-lookup"><span data-stu-id="f0770-122">Configure Your Application tooUse Notification Hubs</span></span>
<span data-ttu-id="f0770-123">toouse Azure Notification Hubs, behöver du toodownload och Använd hello Node.js [azure-paketet](https://www.npmjs.com/package/azure), som innehåller en inbyggd grupp helper bibliotek som kommunicerar med hello push notification REST services.</span><span class="sxs-lookup"><span data-stu-id="f0770-123">toouse Azure Notification Hubs, you need toodownload and use hello Node.js [azure package](https://www.npmjs.com/package/azure), which includes a built-in set of helper libraries that communicate with hello push notification REST services.</span></span>

### <a name="use-node-package-manager-npm-tooobtain-hello-package"></a><span data-ttu-id="f0770-124">Använd noden Package Manager (NPM) tooobtain hello-paket</span><span class="sxs-lookup"><span data-stu-id="f0770-124">Use Node Package Manager (NPM) tooobtain hello package</span></span>
1. <span data-ttu-id="f0770-125">Använd ett kommandoradsgränssnitt som **PowerShell** (Windows), **Terminal** (Mac), eller **Bash** (Linux) och gå toohello mappen där du skapade ditt tomt program .</span><span class="sxs-lookup"><span data-stu-id="f0770-125">Use a command-line interface such as **PowerShell** (Windows), **Terminal** (Mac), or **Bash** (Linux) and navigate toohello folder where you created your blank application.</span></span>
2. <span data-ttu-id="f0770-126">Typen **npm installera azure-sb** i hello kommandofönstret.</span><span class="sxs-lookup"><span data-stu-id="f0770-126">Type **npm install azure-sb** in hello command window.</span></span>
3. <span data-ttu-id="f0770-127">Du kan köra hello manuellt **ls** eller **dir** kommandot tooverify som en **nod\_moduler** mappen har skapats.</span><span class="sxs-lookup"><span data-stu-id="f0770-127">You can manually run hello **ls** or **dir** command tooverify that a **node\_modules** folder was created.</span></span> <span data-ttu-id="f0770-128">Hitta hello inuti den mappen **azure** paket som innehåller hello bibliotek som du behöver tooaccess hello Notification Hub.</span><span class="sxs-lookup"><span data-stu-id="f0770-128">Inside that folder, find hello **azure** package, which contains hello libraries you need tooaccess hello Notification Hub.</span></span>

> [!NOTE]
> <span data-ttu-id="f0770-129">Du kan lära dig mer om att installera NPM på hello officiella [NPM blogg](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span><span class="sxs-lookup"><span data-stu-id="f0770-129">You can learn more about installing NPM on hello official [NPM blog](http://blog.npmjs.org/post/85484771375/how-to-install-npm).</span></span> 
> 
> 

### <a name="import-hello-module"></a><span data-ttu-id="f0770-130">Importera hello-modul</span><span class="sxs-lookup"><span data-stu-id="f0770-130">Import hello module</span></span>
<span data-ttu-id="f0770-131">Med hjälp av en textredigerare, Lägg till följande toohello överkant hello hello **server.js** -filen för programmet hello:</span><span class="sxs-lookup"><span data-stu-id="f0770-131">Using a text editor, add hello following toohello top of hello **server.js** file of hello application:</span></span>

    var azure = require('azure');

### <a name="setup-an-azure-notification-hub-connection"></a><span data-ttu-id="f0770-132">Skapa en anslutning med Azure Notification Hub</span><span class="sxs-lookup"><span data-stu-id="f0770-132">Setup an Azure Notification Hub connection</span></span>
<span data-ttu-id="f0770-133">Hej **NotificationHubService** objekt kan du arbeta med notification hubs.</span><span class="sxs-lookup"><span data-stu-id="f0770-133">hello **NotificationHubService** object lets you work with notification hubs.</span></span> <span data-ttu-id="f0770-134">hello följande kod skapar en **NotificationHubService** objekt för hello nofication hubb med namnet **hubname**.</span><span class="sxs-lookup"><span data-stu-id="f0770-134">hello following code creates a **NotificationHubService** object for hello nofication hub named **hubname**.</span></span> <span data-ttu-id="f0770-135">Lägg till den hello övre delen av hello **server.js** filen efter hello instruktionen tooimport hello azure modulen:</span><span class="sxs-lookup"><span data-stu-id="f0770-135">Add it near hello top of hello **server.js** file, after hello statement tooimport hello azure module:</span></span>

    var notificationHubService = azure.createNotificationHubService('hubname','connectionstring');

<span data-ttu-id="f0770-136">Hej anslutning **connectionstring** värde kan hämtas från hello [Azure Portal] genom att utföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="f0770-136">hello connection **connectionstring** value can be obtained from hello [Azure Portal] by performing hello following steps:</span></span>

1. <span data-ttu-id="f0770-137">Hello vänstra navigeringsfönstret, klicka på **Bläddra**.</span><span class="sxs-lookup"><span data-stu-id="f0770-137">In hello left navigation pane, click **Browse**.</span></span>
2. <span data-ttu-id="f0770-138">Välj **Meddelandehubbar**, och sedan hitta hello hubb gärna toouse för hello exemplet.</span><span class="sxs-lookup"><span data-stu-id="f0770-138">Select **Notification Hubs**, and then find hello hub you wish toouse for hello sample.</span></span> <span data-ttu-id="f0770-139">Du kan se toohello [Windows Store komma igång-kursen](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) om du behöver hjälp med att skapa en ny Meddelandehubb.</span><span class="sxs-lookup"><span data-stu-id="f0770-139">You can refer toohello [Windows Store Getting Started tutorial](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md) if you need help creating a new Notification Hub.</span></span>
3. <span data-ttu-id="f0770-140">Välj **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f0770-140">Select **Settings**.</span></span>
4. <span data-ttu-id="f0770-141">Klicka på **åtkomstprinciper**.</span><span class="sxs-lookup"><span data-stu-id="f0770-141">Click on **Access Policies**.</span></span> <span data-ttu-id="f0770-142">Båda anslutningssträngar för delade och fullständig åtkomst visas.</span><span class="sxs-lookup"><span data-stu-id="f0770-142">You will see both shared and full access connection strings.</span></span>

![Azure Portal – Notification hub](./media/notification-hubs-nodejs-how-to-use-notification-hubs/notification-hubs-portal.png)

> [!NOTE]
> <span data-ttu-id="f0770-144">Du kan också hämta hello anslutningssträngen med hello **Get-AzureSbNamespace** cmdlet som tillhandahålls av [Azure PowerShell](/powershell/azureps-cmdlets-docs) eller hello **azure sb namnområde visa** med Hej [Azure-kommandoradsgränssnittet (Azure CLI)](../cli-install-nodejs.md).</span><span class="sxs-lookup"><span data-stu-id="f0770-144">You can also retrieve hello connection string using hello **Get-AzureSbNamespace** cmdlet provided by [Azure PowerShell](/powershell/azureps-cmdlets-docs) or hello **azure sb namespace show** command with hello [Azure Command-Line Interface (Azure CLI)](../cli-install-nodejs.md).</span></span>
> 
> 

## <a name="general-architecture"></a><span data-ttu-id="f0770-145">Allmän arkitektur</span><span class="sxs-lookup"><span data-stu-id="f0770-145">General architecture</span></span>
<span data-ttu-id="f0770-146">Hej **NotificationHubService** objekt visar hello följande objektinstanser för att skicka push-meddelanden toospecific enheter och program:</span><span class="sxs-lookup"><span data-stu-id="f0770-146">hello **NotificationHubService** object exposes hello following object instances for sending push notifications toospecific devices and applications:</span></span>

* <span data-ttu-id="f0770-147">**Android** -Använd hello **GcmService** -objekt som finns på **notificationHubService.gcm**</span><span class="sxs-lookup"><span data-stu-id="f0770-147">**Android** - use hello **GcmService** object, which is available at **notificationHubService.gcm**</span></span>
* <span data-ttu-id="f0770-148">**iOS** -Använd hello **ApnsService** -objekt som är tillgängliga i **notificationHubService.apns**</span><span class="sxs-lookup"><span data-stu-id="f0770-148">**iOS** - use hello **ApnsService** object, which is accessible at **notificationHubService.apns**</span></span>
* <span data-ttu-id="f0770-149">**Windows Phone** -Använd hello **MpnsService** -objekt som finns på **notificationHubService.mpns**</span><span class="sxs-lookup"><span data-stu-id="f0770-149">**Windows Phone** - use hello **MpnsService** object, which is available at **notificationHubService.mpns**</span></span>
* <span data-ttu-id="f0770-150">**Uwp** -Använd hello **WnsService** -objekt som finns på **notificationHubService.wns**</span><span class="sxs-lookup"><span data-stu-id="f0770-150">**Universal Windows Platform** - use hello **WnsService** object, which is available at **notificationHubService.wns**</span></span>

### <a name="how-to-send-push-notifications-tooandroid-applications"></a><span data-ttu-id="f0770-151">Så här: skicka push-meddelanden tooAndroid program</span><span class="sxs-lookup"><span data-stu-id="f0770-151">How to: Send push notifications tooAndroid applications</span></span>
<span data-ttu-id="f0770-152">Hej **GcmService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooAndroid program.</span><span class="sxs-lookup"><span data-stu-id="f0770-152">hello **GcmService** object provides a **send** method that can be used toosend push notifications tooAndroid applications.</span></span> <span data-ttu-id="f0770-153">Hej **skicka** metoden godkänner hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f0770-153">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="f0770-154">**Taggar** -hello taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="f0770-154">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="f0770-155">Om ingen tagg skickas hello-meddelande tooall klienter.</span><span class="sxs-lookup"><span data-stu-id="f0770-155">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="f0770-156">**Nyttolasten** -hello-meddelande JSON eller raw sträng nyttolast.</span><span class="sxs-lookup"><span data-stu-id="f0770-156">**Payload** - hello message's JSON or raw string payload.</span></span>
* <span data-ttu-id="f0770-157">**Motringning** -hello Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="f0770-157">**Callback** - hello callback function.</span></span>

<span data-ttu-id="f0770-158">Finns mer information om hello nyttolastformatet hello **nyttolast** avsnitt i hello [implementera GCM-Server](http://developer.android.com/google/gcm/server.html#payload) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f0770-158">For more information on hello payload format, see hello **Payload** section of hello [Implementing GCM Server](http://developer.android.com/google/gcm/server.html#payload) document.</span></span>

<span data-ttu-id="f0770-159">hello följande kod använder hello **GcmService** instans som exponeras av hello **NotificationHubService** toosend tooall en push-meddelande registrerade klienter.</span><span class="sxs-lookup"><span data-stu-id="f0770-159">hello following code uses hello **GcmService** instance exposed by hello **NotificationHubService** toosend a push notification tooall registered clients.</span></span>

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

### <a name="how-to-send-push-notifications-tooios-applications"></a><span data-ttu-id="f0770-160">Så här: skicka push-meddelanden tooiOS program</span><span class="sxs-lookup"><span data-stu-id="f0770-160">How to: Send push notifications tooiOS applications</span></span>
<span data-ttu-id="f0770-161">Samma som beskrivs ovan, Android-program hello **ApnsService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooiOS program.</span><span class="sxs-lookup"><span data-stu-id="f0770-161">Same as with Android applications described above, hello **ApnsService** object provides a **send** method that can be used toosend push notifications tooiOS applications.</span></span> <span data-ttu-id="f0770-162">Hej **skicka** metoden godkänner hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f0770-162">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="f0770-163">**Taggar** -hello taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="f0770-163">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="f0770-164">Om ingen tagg skickas hello-meddelande tooall klienter.</span><span class="sxs-lookup"><span data-stu-id="f0770-164">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="f0770-165">**Nyttolasten** – hello-meddelande JSON eller string nyttolast.</span><span class="sxs-lookup"><span data-stu-id="f0770-165">**Payload** - hello message's JSON or string payload.</span></span>
* <span data-ttu-id="f0770-166">**Motringning** -hello Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="f0770-166">**Callback** - hello callback function.</span></span>

<span data-ttu-id="f0770-167">Mer information hello nyttolastformat, finns hello **meddelande nyttolast** avsnitt i hello [lokala Programmeringsguide och Push Notification](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="f0770-167">For more information hello payload format, see hello **Notification Payload** section of hello [Local and Push Notification Programming Guide](http://developer.apple.com/library/ios/#documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/ApplePushService/ApplePushService.html) document.</span></span>

<span data-ttu-id="f0770-168">hello följande kod använder hello **ApnsService** instans som exponeras av hello **NotificationHubService** toosend en avisering visas tooall klienter:</span><span class="sxs-lookup"><span data-stu-id="f0770-168">hello following code uses hello **ApnsService** instance exposed by hello **NotificationHubService** toosend an alert message tooall clients:</span></span>

    var payload={
        alert: 'Hello!'
      };
    notificationHubService.apns.send(null, payload, function(error){
      if(!error){
         // notification sent
      }
    });

### <a name="how-to-send-push-notifications-toowindows-phone-applications"></a><span data-ttu-id="f0770-169">Så här: skicka push-meddelanden tooWindows Phone-program</span><span class="sxs-lookup"><span data-stu-id="f0770-169">How to: Send push notifications tooWindows Phone applications</span></span>
<span data-ttu-id="f0770-170">Hej **MpnsService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooWindows Phone-program.</span><span class="sxs-lookup"><span data-stu-id="f0770-170">hello **MpnsService** object provides a **send** method that can be used toosend push notifications tooWindows Phone applications.</span></span> <span data-ttu-id="f0770-171">Hej **skicka** metoden godkänner hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f0770-171">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="f0770-172">**Taggar** -hello taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="f0770-172">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="f0770-173">Om ingen tagg skickas hello-meddelande tooall klienter.</span><span class="sxs-lookup"><span data-stu-id="f0770-173">If no tag is provided, hello notification will be sent tooall clients.</span></span>
* <span data-ttu-id="f0770-174">**Nyttolasten** -hello-meddelande nyttolast för XML.</span><span class="sxs-lookup"><span data-stu-id="f0770-174">**Payload** - hello message's XML payload.</span></span>
* <span data-ttu-id="f0770-175">**Målnamn**  -  `toast` för popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f0770-175">**TargetName** - `toast` for toast notifications.</span></span> <span data-ttu-id="f0770-176">`token`för sida vid sida-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="f0770-176">`token` for tile notifications.</span></span>
* <span data-ttu-id="f0770-177">**NotificationClass** -hello prioritet av hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="f0770-177">**NotificationClass** - hello priority of hello notification.</span></span> <span data-ttu-id="f0770-178">Se hello **HTTP-huvudet element** avsnitt i hello [Push-meddelanden från en server](http://msdn.microsoft.com/library/hh221551.aspx) dokument för giltiga värden.</span><span class="sxs-lookup"><span data-stu-id="f0770-178">See hello **HTTP Header Elements** section of hello [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) document for valid values.</span></span>
* <span data-ttu-id="f0770-179">**Alternativ** – valfritt begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="f0770-179">**Options** - optional request headers.</span></span>
* <span data-ttu-id="f0770-180">**Motringning** -hello Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="f0770-180">**Callback** - hello callback function.</span></span>

<span data-ttu-id="f0770-181">En lista över giltiga **TargetName**, **NotificationClass** och Rubrikalternativ, kolla hello [Push-meddelanden från en server](http://msdn.microsoft.com/library/hh221551.aspx) sidan.</span><span class="sxs-lookup"><span data-stu-id="f0770-181">For a list of valid **TargetName**, **NotificationClass** and header options, check out hello [Push notifications from a server](http://msdn.microsoft.com/library/hh221551.aspx) page.</span></span>

<span data-ttu-id="f0770-182">Följande exempelkod hello använder hello **MpnsService** instans som exponeras av hello **NotificationHubService** toosend en push-meddelandensom:</span><span class="sxs-lookup"><span data-stu-id="f0770-182">hello following sample code uses hello **MpnsService** instance exposed by hello **NotificationHubService** toosend a toast push notification:</span></span>

    var payload = '<?xml version="1.0" encoding="utf-8"?><wp:Notification xmlns:wp="WPNotification"><wp:Toast><wp:Text1>string</wp:Text1><wp:Text2>string</wp:Text2></wp:Toast></wp:Notification>';
    notificationHubService.mpns.send(null, payload, 'toast', 22, function(error){
      if(!error){
        //notification sent
      }
    });

### <a name="how-to-send-push-notifications-toouniversal-windows-platform-uwp-applications"></a><span data-ttu-id="f0770-183">Så här: skicka push-meddelanden tooUniversal Windowsplattformen (UWP) program</span><span class="sxs-lookup"><span data-stu-id="f0770-183">How to: Send push notifications tooUniversal Windows Platform (UWP) applications</span></span>
<span data-ttu-id="f0770-184">Hej **WnsService** objektet innehåller en **skicka** metod som kan använda toosend push-meddelanden tooUniversal Windows Platform program.</span><span class="sxs-lookup"><span data-stu-id="f0770-184">hello **WnsService** object provides a **send** method that can be used toosend push notifications tooUniversal Windows Platform applications.</span></span>  <span data-ttu-id="f0770-185">Hej **skicka** metoden godkänner hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="f0770-185">hello **send** method accepts hello following parameters:</span></span>

* <span data-ttu-id="f0770-186">**Taggar** -hello taggen identifierare.</span><span class="sxs-lookup"><span data-stu-id="f0770-186">**Tags** - hello tag identifier.</span></span> <span data-ttu-id="f0770-187">Om ingen tagg skickas hello-meddelande tooall registrerade klienter.</span><span class="sxs-lookup"><span data-stu-id="f0770-187">If no tag is provided, hello notification will be sent tooall registered clients.</span></span>
* <span data-ttu-id="f0770-188">**Nyttolasten** -hello nyttolast för XML-meddelande.</span><span class="sxs-lookup"><span data-stu-id="f0770-188">**Payload** - hello XML message payload.</span></span>
* <span data-ttu-id="f0770-189">**Typen** -hello meddelandetyp.</span><span class="sxs-lookup"><span data-stu-id="f0770-189">**Type** - hello notification type.</span></span>
* <span data-ttu-id="f0770-190">**Alternativ** – valfritt begärandehuvuden.</span><span class="sxs-lookup"><span data-stu-id="f0770-190">**Options** - optional request headers.</span></span>
* <span data-ttu-id="f0770-191">**Motringning** -hello Återanropsfunktionen.</span><span class="sxs-lookup"><span data-stu-id="f0770-191">**Callback** - hello callback function.</span></span>

<span data-ttu-id="f0770-192">En lista över giltiga typer och begärandehuvuden finns [Push notification service begärande- och svarshuvuden](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0770-192">For a list of valid types and request headers, see [Push notification service request and response headers](http://msdn.microsoft.com/library/windows/apps/hh465435.aspx).</span></span>

<span data-ttu-id="f0770-193">hello följande kod använder hello **WnsService** instans som exponeras av hello **NotificationHubService** toosend en popup push notification tooa UWP-appen:</span><span class="sxs-lookup"><span data-stu-id="f0770-193">hello following code uses hello **WnsService** instance exposed by hello **NotificationHubService** toosend a toast push notification tooa UWP app:</span></span>

    var payload = '<toast><visual><binding template="ToastText01"><text id="1">Hello!</text></binding></visual></toast>';
    notificationHubService.wns.send(null, payload , 'wns/toast', function(error){
      if(!error){
         // notification sent
      }
    });

## <a name="next-steps"></a><span data-ttu-id="f0770-194">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f0770-194">Next Steps</span></span>
<span data-ttu-id="f0770-195">hello exempel kodavsnitt ovan kan du tooeasily build service infrastruktur toodeliver push-meddelanden tooa flera olika enheter.</span><span class="sxs-lookup"><span data-stu-id="f0770-195">hello sample snippets above allow you tooeasily build service infrastructure toodeliver push notifications tooa wide variety of devices.</span></span> <span data-ttu-id="f0770-196">Nu när du har lärt dig grunderna hello i Notification Hubs med node.js, följa dessa länkar toolearn mer om hur du kan utöka dessa ytterligare funktioner.</span><span class="sxs-lookup"><span data-stu-id="f0770-196">Now that you've learned hello basics of using Notification Hubs with node.js, follow these links toolearn more about how you can extend these capabilities further.</span></span>

* <span data-ttu-id="f0770-197">Se hello MSDN-referens för [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span><span class="sxs-lookup"><span data-stu-id="f0770-197">See hello MSDN Reference for [Azure Notification Hubs](https://msdn.microsoft.com/library/azure/jj927170.aspx).</span></span>
* <span data-ttu-id="f0770-198">Besök hello [Azure SDK för noden] lagringsplats på GitHub för fler exempel och implementeringsdetaljer.</span><span class="sxs-lookup"><span data-stu-id="f0770-198">Visit hello [Azure SDK for Node] repository on GitHub for more samples and implementation details.</span></span>

[Azure SDK för noden]: https://github.com/WindowsAzure/azure-sdk-for-node
[Next Steps]: #nextsteps
[What are Service Bus Topics and Subscriptions?]: #what-are-service-bus-topics
[Create a Service Namespace]: #create-a-service-namespace
[Obtain hello Default Management Credentials for hello Namespace]: #obtain-default-credentials
[Create a Node.js Application]: #Create_a_Nodejs_Application
[Configure Your Application tooUse Service Bus]: #Configure_Your_Application_to_Use_Service_Bus
[How to: Create a Topic]: #How_to_Create_a_Topic
[How to: Create Subscriptions]: #How_to_Create_Subscriptions
[How to: Send Messages tooa Topic]: #How_to_Send_Messages_to_a_Topic
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
[webbplatsen med WebMatrix]: /develop/nodejs/tutorials/web-site-with-webmatrix/
[Node.js Cloud Service]: ../cloud-services/cloud-services-nodejs-develop-deploy-app.md
[Previous Management Portal]: .media/notification-hubs-nodejs-how-to-use-notification-hubs/previous-portal.png
[nodejswebsite]: /develop/nodejs/tutorials/create-a-website-(mac)/
[Node.js Cloud Service with Storage]: /develop/nodejs/tutorials/web-app-with-storage/
[Node.js Web Application with Storage]: /develop/nodejs/tutorials/web-site-with-storage/
[Azure Portal]: https://portal.azure.com
