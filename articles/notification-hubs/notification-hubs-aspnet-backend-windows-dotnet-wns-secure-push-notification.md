---
title: aaaAzure Notification Hubs Secure Push
description: "Lär dig hur säker toosend push-meddelanden i Azure. Kodexempel som skrivits i C# med hjälp av hello .NET-API."
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
services: notification-hubs
ms.assetid: 5aef50f4-80b3-460e-a9a7-7435001273bd
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: b6fe16c96d28d75ff698278409fb012472ba6396
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="4bd2a-104">Azure Notification Hubs säker Push</span><span class="sxs-lookup"><span data-stu-id="4bd2a-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4bd2a-105">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="4bd2a-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="4bd2a-106">iOS</span><span class="sxs-lookup"><span data-stu-id="4bd2a-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="4bd2a-107">Android</span><span class="sxs-lookup"><span data-stu-id="4bd2a-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="4bd2a-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="4bd2a-108">Overview</span></span>
<span data-ttu-id="4bd2a-109">Stöd för push-meddelanden i Microsoft Azure kan du tooaccess en lätt att använda flera plattformar, skalats ut push-infrastruktur som förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-109">Push notification support in Microsoft Azure enables you tooaccess an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="4bd2a-110">På grund av tooregulatory eller säkerhet begränsningar kanske ibland ett program tooinclude någonting i hello-meddelande som kan överföras via hello standard push-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-110">Due tooregulatory or security constraints, sometimes an application might want tooinclude something in hello notification that cannot be transmitted through hello standard push notification infrastructure.</span></span> <span data-ttu-id="4bd2a-111">Den här självstudiekursen beskrivs hur tooachieve hello samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan hello klientenheter och hello-appserverdelen.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-111">This tutorial describes how tooachieve hello same experience by sending sensitive information through a secure, authenticated connection between hello client device and hello app backend.</span></span>

<span data-ttu-id="4bd2a-112">På en hög nivå är hello flödet följande:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-112">At a high level, hello flow is as follows:</span></span>

1. <span data-ttu-id="4bd2a-113">hello appens serverdel:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-113">hello app back-end:</span></span>
   * <span data-ttu-id="4bd2a-114">Lagrar säker nyttolast i backend-databas.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="4bd2a-115">Skickar hello-ID för meddelande toohello enheten (ingen säker information skickas).</span><span class="sxs-lookup"><span data-stu-id="4bd2a-115">Sends hello ID of this notification toohello device (no secure information is sent).</span></span>
2. <span data-ttu-id="4bd2a-116">hello appen på hello enhet, när du tar emot hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-116">hello app on hello device, when receiving hello notification:</span></span>
   * <span data-ttu-id="4bd2a-117">hello enheten kontaktar hello backend-begärande hello säker nyttolast.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-117">hello device contacts hello back-end requesting hello secure payload.</span></span>
   * <span data-ttu-id="4bd2a-118">hello app kan du visa hello-nyttolast som ett meddelande på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-118">hello app can show hello payload as a notification on hello device.</span></span>

<span data-ttu-id="4bd2a-119">Det är viktigt toonote att vi antar hello enheten i hello föregående flöde (och i den här självstudiekursen) lagrar en autentiseringstoken i lokal lagring när hello användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-119">It is important toonote that in hello preceding flow (and in this tutorial), we assume that hello device stores an authentication token in local storage, after hello user logs in.</span></span> <span data-ttu-id="4bd2a-120">Detta garanterar en helt integrerad upplevelse som hello enhet kan hämta hello-meddelande säker nyttolast med denna token.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-120">This guarantees a completely seamless experience, as hello device can retrieve hello notification’s secure payload using this token.</span></span> <span data-ttu-id="4bd2a-121">Om ditt program inte kan lagra autentiseringstoken på hello enhet eller om dessa token kan ha gått, ska hello enhetsapp vid mottagning av hello-meddelande visa ett allmänt meddelande fråga hello användaren toolaunch hello app.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-121">If your application does not store authentication tokens on hello device, or if these tokens can be expired, hello device app, upon receiving hello notification should display a generic notification prompting hello user toolaunch hello app.</span></span> <span data-ttu-id="4bd2a-122">hello app sedan autentiserar hello användare och visar hello notification nyttolast.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-122">hello app then authenticates hello user and shows hello notification payload.</span></span>

<span data-ttu-id="4bd2a-123">Den här säkra Push-kursen visar hur toosend ett push-meddelande på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-123">This Secure Push tutorial shows how toosend a push notification securely.</span></span> <span data-ttu-id="4bd2a-124">hello kursen bygger på hello [meddela användare](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) självstudier, så bör du genomföra hello stegen i den här självstudiekursen först.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-124">hello tutorial builds on hello [Notify Users](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial, so you should complete hello steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="4bd2a-125">Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span><span class="sxs-lookup"><span data-stu-id="4bd2a-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span></span>
> <span data-ttu-id="4bd2a-126">Observera också att Windows Phone 8.1 kräver autentiseringsuppgifter för Windows (inte Windows Phone) och bakgrundsaktiviteter inte fungerar på Windows Phone 8.0 eller Silverlight 8.1.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-126">Also, note that Windows Phone 8.1 requires Windows (not Windows Phone) credentials, and that background tasks do not work on Windows Phone 8.0 or Silverlight 8.1.</span></span> <span data-ttu-id="4bd2a-127">För Windows Store-program, du kan ta emot meddelanden via en bakgrundsaktivitet endast om hello app är aktiverad för låsskärm (klicka på hello kryssrutan i hello Appmanifest).</span><span class="sxs-lookup"><span data-stu-id="4bd2a-127">For Windows Store applications, you can receive notifications via a background task only if hello app is lock-screen enabled (click hello checkbox in hello Appmanifest).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-hello-windows-phone-project"></a><span data-ttu-id="4bd2a-128">Ändra hello Windows Phone-projekt</span><span class="sxs-lookup"><span data-stu-id="4bd2a-128">Modify hello Windows Phone Project</span></span>
1. <span data-ttu-id="4bd2a-129">I hello **NotifyUserWindowsPhone** projekt, Lägg till följande kod tooApp.xaml.cs tooregister hello push bakgrundsaktivitet hello.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-129">In hello **NotifyUserWindowsPhone** project, add hello following code tooApp.xaml.cs tooregister hello push background task.</span></span> <span data-ttu-id="4bd2a-130">Lägg till följande kodrad i slutet av hello av hello hello `OnLaunched()` metoden:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-130">Add hello following line of code at hello end of hello `OnLaunched()` method:</span></span>
   
        RegisterBackgroundTask();
2. <span data-ttu-id="4bd2a-131">Lägg till följande kod direkt efter hello hello fortfarande i App.xaml.cs `OnLaunched()` metoden:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-131">Still in App.xaml.cs, add hello following code immediately after hello `OnLaunched()` method:</span></span>
   
        private async void RegisterBackgroundTask()
        {
            if (!Windows.ApplicationModel.Background.BackgroundTaskRegistration.AllTasks.Any(i => i.Value.Name == "PushBackgroundTask"))
            {
                var result = await BackgroundExecutionManager.RequestAccessAsync();
                var builder = new BackgroundTaskBuilder();
   
                builder.Name = "PushBackgroundTask";
                builder.TaskEntryPoint = typeof(PushBackgroundComponent.PushBackgroundTask).FullName;
                builder.SetTrigger(new Windows.ApplicationModel.Background.PushNotificationTrigger());
                BackgroundTaskRegistration task = builder.Register();
            }
        }
3. <span data-ttu-id="4bd2a-132">Lägg till följande hello `using` instruktioner överst hello i hello App.xaml.cs fil:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-132">Add hello following `using` statements at hello top of hello App.xaml.cs file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. <span data-ttu-id="4bd2a-133">Från hello **filen** -menyn i Visual Studio klickar du på **spara alla**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-133">From hello **File** menu in Visual Studio, click **Save All**.</span></span>

## <a name="create-hello-push-background-component"></a><span data-ttu-id="4bd2a-134">Skapa hello Push bakgrund komponent</span><span class="sxs-lookup"><span data-stu-id="4bd2a-134">Create hello Push Background Component</span></span>
<span data-ttu-id="4bd2a-135">hello nästa steg är toocreate hello push bakgrund komponent.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-135">hello next step is toocreate hello push background component.</span></span>

1. <span data-ttu-id="4bd2a-136">I Solution Explorer högerklickar du på hello översta noden hello-lösning (**lösning SecurePush** i det här fallet), klicka på **Lägg till**, klicka på **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-136">In Solution Explorer, right-click hello top-level node of hello solution (**Solution SecurePush** in this case), then click **Add**, then click **New Project**.</span></span>
2. <span data-ttu-id="4bd2a-137">Expandera **Store-appar**, klicka på **Windows Phone appar**, klicka på **Windows Runtime-komponent (Windows Phone)**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-137">Expand **Store Apps**, then click **Windows Phone Apps**, then click **Windows Runtime Component (Windows Phone)**.</span></span> <span data-ttu-id="4bd2a-138">Namnet hello projektet **PushBackgroundComponent**, och klicka sedan på **OK** toocreate hello projektet.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-138">Name hello project **PushBackgroundComponent**, and then click **OK** toocreate hello project.</span></span>
   
    ![][12]
3. <span data-ttu-id="4bd2a-139">I Solution Explorer högerklickar du på hello **PushBackgroundComponent (Windows Phone 8.1)** projektet och klicka sedan på **Lägg till**, klicka på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-139">In Solution Explorer, right-click hello **PushBackgroundComponent (Windows Phone 8.1)** project, then click **Add**, then click **Class**.</span></span> <span data-ttu-id="4bd2a-140">Namnge hello nya klassen **PushBackgroundTask.cs**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-140">Name hello new class **PushBackgroundTask.cs**.</span></span> <span data-ttu-id="4bd2a-141">Klicka på **Lägg till** toogenerate hello-klassen.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-141">Click **Add** toogenerate hello class.</span></span>
4. <span data-ttu-id="4bd2a-142">Ersätt hello hela innehållet i hello **PushBackgroundComponent** definitionen för namnområdet med hello följande kod, ersätter hello platshållare `{back-end endpoint}` med hello backend-slutpunkt som erhålls vid distribution av din backend:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-142">Replace hello entire contents of hello **PushBackgroundComponent** namespace definition with hello following code, substituting hello placeholder `{back-end endpoint}` with hello back-end endpoint obtained while deploying your back-end:</span></span>
   
        public sealed class Notification
            {
                public int Id { get; set; }
                public string Payload { get; set; }
                public bool Read { get; set; }
            }
   
            public sealed class PushBackgroundTask : IBackgroundTask
            {
                private string GET_URL = "{back-end endpoint}/api/notifications/";
   
                async void IBackgroundTask.Run(IBackgroundTaskInstance taskInstance)
                {
                    // Store hello content received from hello notification so it can be retrieved from hello UI.
                    RawNotification raw = (RawNotification)taskInstance.TriggerDetails;
                    var notificationId = raw.Content;
   
                    // retrieve content
                    BackgroundTaskDeferral deferral = taskInstance.GetDeferral();
                    var httpClient = new HttpClient();
                    var settings = ApplicationData.Current.LocalSettings.Values;
                    httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                    var notificationString = await httpClient.GetStringAsync(GET_URL + notificationId);
   
                    var notification = JsonConvert.DeserializeObject<Notification>(notificationString);
   
                    ShowToast(notification);
   
                    deferral.Complete();
                }
   
                private void ShowToast(Notification notification)
                {
                    ToastTemplateType toastTemplate = ToastTemplateType.ToastText01;
                    XmlDocument toastXml = ToastNotificationManager.GetTemplateContent(toastTemplate);
                    XmlNodeList toastTextElements = toastXml.GetElementsByTagName("text");
                    toastTextElements[0].AppendChild(toastXml.CreateTextNode(notification.Payload));
                    ToastNotification toast = new ToastNotification(toastXml);
                    ToastNotificationManager.CreateToastNotifier().Show(toast);
                }
            }
5. <span data-ttu-id="4bd2a-143">I Solution Explorer högerklickar du på hello **PushBackgroundComponent (Windows Phone 8.1)** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-143">In Solution Explorer, right-click hello **PushBackgroundComponent (Windows Phone 8.1)** project and then click **Manage NuGet Packages**.</span></span>
6. <span data-ttu-id="4bd2a-144">Hello vänster, klickar du på **Online**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-144">On hello left-hand side, click **Online**.</span></span>
7. <span data-ttu-id="4bd2a-145">I hello **Sök** skriver **http-klienten**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-145">In hello **Search** box, type **Http Client**.</span></span>
8. <span data-ttu-id="4bd2a-146">I listan över sökresultat hello klickar du på **klientbibliotek för Microsoft HTTP**, och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-146">In hello results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="4bd2a-147">Slutför hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-147">Complete hello installation.</span></span>
9. <span data-ttu-id="4bd2a-148">Tillbaka i hello NuGet **Sök** skriver **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-148">Back in hello NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="4bd2a-149">Installera hello **Json.NET** paketet och sedan Stäng hello NuGet Package Manager-fönstret.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-149">Install hello **Json.NET** package, then close hello NuGet Package Manager window.</span></span>
10. <span data-ttu-id="4bd2a-150">Lägg till följande hello `using` instruktioner överst hello i hello **PushBackgroundTask.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-150">Add hello following `using` statements at hello top of hello **PushBackgroundTask.cs** file:</span></span>
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. <span data-ttu-id="4bd2a-151">I Solution Explorer i hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt, högerklicka på **referenser**, klicka på **Lägg till referens...** . I dialogrutan för Reference Manager hello kryssrutan hello bredvid för**PushBackgroundComponent**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-151">In Solution Explorer, in hello **NotifyUserWindowsPhone (Windows Phone 8.1)** project, right-click **References**, then click **Add Reference...**. In hello Reference Manager dialog, check hello box next too**PushBackgroundComponent**, and then click **OK**.</span></span>
12. <span data-ttu-id="4bd2a-152">I Solution Explorer dubbelklickar du på **Package.appxmanifest** i hello **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-152">In Solution Explorer, double-click **Package.appxmanifest** in hello **NotifyUserWindowsPhone (Windows Phone 8.1)** project.</span></span> <span data-ttu-id="4bd2a-153">Under **meddelanden**, ange **popup-kompatibel** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-153">Under **Notifications**, set **Toast Capable** too**Yes**.</span></span>
    
    ![][3]
13. <span data-ttu-id="4bd2a-154">Fortfarande i **Package.appxmanifest**, klicka på hello **deklarationer** menyn hello övre delen.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-154">Still in **Package.appxmanifest**, click hello **Declarations** menu near hello top.</span></span> <span data-ttu-id="4bd2a-155">I hello **tillgängliga deklarationer** listrutan, klickar du på **bakgrundsaktiviteter**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-155">In hello **Available Declarations** dropdown, click **Background Tasks**, and then click **Add**.</span></span>
14. <span data-ttu-id="4bd2a-156">I **Package.appxmanifest**under **egenskaper**, kontrollera **Push notification**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-156">In **Package.appxmanifest**, under **Properties**, check **Push notification**.</span></span>
15. <span data-ttu-id="4bd2a-157">I **Package.appxmanifest**under **Appinställningar**, typen **PushBackgroundComponent.PushBackgroundTask** i hello **startpunkten** fält.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-157">In **Package.appxmanifest**, under **App Settings**, type **PushBackgroundComponent.PushBackgroundTask** in hello **Entry Point** field.</span></span>
    
    ![][13]
16. <span data-ttu-id="4bd2a-158">Från hello **filen** -menyn klickar du på **spara alla**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-158">From hello **File** menu, click **Save All**.</span></span>

## <a name="run-hello-application"></a><span data-ttu-id="4bd2a-159">Kör hello program</span><span class="sxs-lookup"><span data-stu-id="4bd2a-159">Run hello Application</span></span>
<span data-ttu-id="4bd2a-160">toorun Hej program, hello följande:</span><span class="sxs-lookup"><span data-stu-id="4bd2a-160">toorun hello application, do hello following:</span></span>

1. <span data-ttu-id="4bd2a-161">I Visual Studio kör hello **AppBackend** Web API-program.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-161">In Visual Studio, run hello **AppBackend** Web API application.</span></span> <span data-ttu-id="4bd2a-162">En ASP.NET-webbsida visas.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-162">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="4bd2a-163">I Visual Studio kör hello **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-app.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-163">In Visual Studio, run hello **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone app.</span></span> <span data-ttu-id="4bd2a-164">hello Windows Phone-emulatorn kör och läser in hello appen automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-164">hello Windows Phone emulator runs and loads hello app automatically.</span></span>
3. <span data-ttu-id="4bd2a-165">I hello **NotifyUserWindowsPhone** app UI, ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-165">In hello **NotifyUserWindowsPhone** app UI, enter a username and password.</span></span> <span data-ttu-id="4bd2a-166">Dessa kan vara valfri sträng, men de måste vara hello samma värde.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-166">These can be any string, but they must be hello same value.</span></span>
4. <span data-ttu-id="4bd2a-167">I hello **NotifyUserWindowsPhone** app-Gränssnittet klickar du på **logga in och registrera**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-167">In hello **NotifyUserWindowsPhone** app UI, click **Log in and register**.</span></span> <span data-ttu-id="4bd2a-168">Klicka på **skicka push**.</span><span class="sxs-lookup"><span data-stu-id="4bd2a-168">Then click **Send push**.</span></span>

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
