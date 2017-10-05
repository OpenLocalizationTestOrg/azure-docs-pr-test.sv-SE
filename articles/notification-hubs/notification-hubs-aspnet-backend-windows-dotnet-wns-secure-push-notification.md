---
title: "Azure Notification Hubs säker Push"
description: "Lär dig mer om att skicka säkra push-meddelanden i Azure. Kodexempel som skrivits i C# med hjälp av .NET-API."
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
ms.openlocfilehash: 9c626ec1534c4899588150a58c0da57b9d963f6f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-secure-push"></a><span data-ttu-id="ebaaa-104">Azure Notification Hubs säker Push</span><span class="sxs-lookup"><span data-stu-id="ebaaa-104">Azure Notification Hubs Secure Push</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ebaaa-105">Windows Universal</span><span class="sxs-lookup"><span data-stu-id="ebaaa-105">Windows Universal</span></span>](notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md)
> * [<span data-ttu-id="ebaaa-106">iOS</span><span class="sxs-lookup"><span data-stu-id="ebaaa-106">iOS</span></span>](notification-hubs-aspnet-backend-ios-push-apple-apns-secure-notification.md)
> * [<span data-ttu-id="ebaaa-107">Android</span><span class="sxs-lookup"><span data-stu-id="ebaaa-107">Android</span></span>](notification-hubs-aspnet-backend-android-secure-google-gcm-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="ebaaa-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="ebaaa-108">Overview</span></span>
<span data-ttu-id="ebaaa-109">Stöd för push-meddelanden i Microsoft Azure kan du få en lätt att använda flera plattformar, skalats ut push-infrastruktur, vilket förenklar implementeringen av push-meddelanden för konsument- och enterprise-program för mobila plattformar.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-109">Push notification support in Microsoft Azure enables you to access an easy-to-use, multiplatform, scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span>

<span data-ttu-id="ebaaa-110">På grund av reglerande säkerhetsbegränsningar, ibland ett program kan också innehålla något i meddelandet inte kan överföras via de standard push-infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-110">Due to regulatory or security constraints, sometimes an application might want to include something in the notification that cannot be transmitted through the standard push notification infrastructure.</span></span> <span data-ttu-id="ebaaa-111">Den här självstudiekursen beskrivs hur du kan uppnå samma upplevelse genom att skicka känslig information via en säker och autentiserad anslutning mellan klientenheten och appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-111">This tutorial describes how to achieve the same experience by sending sensitive information through a secure, authenticated connection between the client device and the app backend.</span></span>

<span data-ttu-id="ebaaa-112">På en hög nivå är flödet:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-112">At a high level, the flow is as follows:</span></span>

1. <span data-ttu-id="ebaaa-113">På appens serverdel:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-113">The app back-end:</span></span>
   * <span data-ttu-id="ebaaa-114">Lagrar säker nyttolast i backend-databas.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-114">Stores secure payload in back-end database.</span></span>
   * <span data-ttu-id="ebaaa-115">Skickar ID för det här meddelandet till enheten (ingen säker information skickas).</span><span class="sxs-lookup"><span data-stu-id="ebaaa-115">Sends the ID of this notification to the device (no secure information is sent).</span></span>
2. <span data-ttu-id="ebaaa-116">Appen på enheten när du tar emot meddelandet:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-116">The app on the device, when receiving the notification:</span></span>
   * <span data-ttu-id="ebaaa-117">Enheten kontaktar serverdelen begär säker nyttolasten.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-117">The device contacts the back-end requesting the secure payload.</span></span>
   * <span data-ttu-id="ebaaa-118">Appen kan du visa nyttolasten som ett meddelande på enheten.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-118">The app can show the payload as a notification on the device.</span></span>

<span data-ttu-id="ebaaa-119">Det är viktigt att Observera att i det föregående flödet (och i den här självstudiekursen) antar vi att enheten lagrar en autentiseringstoken i lokal lagring när en användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-119">It is important to note that in the preceding flow (and in this tutorial), we assume that the device stores an authentication token in local storage, after the user logs in.</span></span> <span data-ttu-id="ebaaa-120">Detta garanterar en helt integrerad upplevelse som enheten kan hämta den anmälan säker nyttolast med denna token.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-120">This guarantees a completely seamless experience, as the device can retrieve the notification’s secure payload using this token.</span></span> <span data-ttu-id="ebaaa-121">Om ditt program inte kan lagra autentiseringstoken på enheten, eller om dessa token kan ha gått, visas vid mottagning av meddelanden i appen enhet ett allmänt meddelande där användaren uppmanas att starta appen.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-121">If your application does not store authentication tokens on the device, or if these tokens can be expired, the device app, upon receiving the notification should display a generic notification prompting the user to launch the app.</span></span> <span data-ttu-id="ebaaa-122">Appen sedan autentiserar användaren och visar nyttolasten för meddelanden.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-122">The app then authenticates the user and shows the notification payload.</span></span>

<span data-ttu-id="ebaaa-123">Den här säkra Push-kursen visar hur du skickar push-meddelanden på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-123">This Secure Push tutorial shows how to send a push notification securely.</span></span> <span data-ttu-id="ebaaa-124">Kursen bygger på den [meddela användare](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) självstudier, så du måste slutföra stegen i den här självstudiekursen först.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-124">The tutorial builds on the [Notify Users](notification-hubs-aspnet-backend-windows-dotnet-wns-notification.md) tutorial, so you should complete the steps in that tutorial first.</span></span>

> [!NOTE]
> <span data-ttu-id="ebaaa-125">Den här kursen förutsätter att du har skapat och konfigurerat din meddelandehubb enligt beskrivningen i [komma igång med Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span><span class="sxs-lookup"><span data-stu-id="ebaaa-125">This tutorial assumes that you have created and configured your notification hub as described in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md).</span></span>
> <span data-ttu-id="ebaaa-126">Observera också att Windows Phone 8.1 kräver autentiseringsuppgifter för Windows (inte Windows Phone) och bakgrundsaktiviteter inte fungerar på Windows Phone 8.0 eller Silverlight 8.1.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-126">Also, note that Windows Phone 8.1 requires Windows (not Windows Phone) credentials, and that background tasks do not work on Windows Phone 8.0 or Silverlight 8.1.</span></span> <span data-ttu-id="ebaaa-127">För Windows Store-program, du kan ta emot meddelanden via en bakgrundsaktivitet endast om appen är aktiverad för låsskärm (klickar du på kryssrutan i Appmanifest).</span><span class="sxs-lookup"><span data-stu-id="ebaaa-127">For Windows Store applications, you can receive notifications via a background task only if the app is lock-screen enabled (click the checkbox in the Appmanifest).</span></span>
> 
> 

[!INCLUDE [notification-hubs-aspnet-backend-securepush](../../includes/notification-hubs-aspnet-backend-securepush.md)]

## <a name="modify-the-windows-phone-project"></a><span data-ttu-id="ebaaa-128">Ändra Windows Phone-projektet</span><span class="sxs-lookup"><span data-stu-id="ebaaa-128">Modify the Windows Phone Project</span></span>
1. <span data-ttu-id="ebaaa-129">I den **NotifyUserWindowsPhone** projekt, Lägg till följande kod i App.xaml.cs att registrera push-bakgrundsaktivitet.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-129">In the **NotifyUserWindowsPhone** project, add the following code to App.xaml.cs to register the push background task.</span></span> <span data-ttu-id="ebaaa-130">Lägg till följande kodrad i slutet av den `OnLaunched()` metoden:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-130">Add the following line of code at the end of the `OnLaunched()` method:</span></span>
   
        RegisterBackgroundTask();
2. <span data-ttu-id="ebaaa-131">Fortfarande i App.xaml.cs lägger du till följande kod direkt efter den `OnLaunched()` metoden:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-131">Still in App.xaml.cs, add the following code immediately after the `OnLaunched()` method:</span></span>
   
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
3. <span data-ttu-id="ebaaa-132">Lägg till följande `using` instruktioner överst i filen App.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-132">Add the following `using` statements at the top of the App.xaml.cs file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Windows.ApplicationModel.Background;
4. <span data-ttu-id="ebaaa-133">Klicka på **Spara alla** från menyn **Arkiv** i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-133">From the **File** menu in Visual Studio, click **Save All**.</span></span>

## <a name="create-the-push-background-component"></a><span data-ttu-id="ebaaa-134">Skapa Push bakgrund komponent</span><span class="sxs-lookup"><span data-stu-id="ebaaa-134">Create the Push Background Component</span></span>
<span data-ttu-id="ebaaa-135">Nästa steg är att skapa komponenten push bakgrund.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-135">The next step is to create the push background component.</span></span>

1. <span data-ttu-id="ebaaa-136">Högerklicka på den översta noden i lösningen i Solution Explorer (**lösning SecurePush** i det här fallet), klicka sedan på **Lägg till**, klicka sedan på **nytt projekt**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-136">In Solution Explorer, right-click the top-level node of the solution (**Solution SecurePush** in this case), then click **Add**, then click **New Project**.</span></span>
2. <span data-ttu-id="ebaaa-137">Expandera **Store-appar**, klicka på **Windows Phone appar**, klicka på **Windows Runtime-komponent (Windows Phone)**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-137">Expand **Store Apps**, then click **Windows Phone Apps**, then click **Windows Runtime Component (Windows Phone)**.</span></span> <span data-ttu-id="ebaaa-138">Namnge projektet **PushBackgroundComponent**, och klicka sedan på **OK** att skapa projektet.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-138">Name the project **PushBackgroundComponent**, and then click **OK** to create the project.</span></span>
   
    ![][12]
3. <span data-ttu-id="ebaaa-139">I Solution Explorer högerklickar du på den **PushBackgroundComponent (Windows Phone 8.1)** projektet och klicka sedan på **Lägg till**, klicka på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-139">In Solution Explorer, right-click the **PushBackgroundComponent (Windows Phone 8.1)** project, then click **Add**, then click **Class**.</span></span> <span data-ttu-id="ebaaa-140">Kalla den nya klassen **PushBackgroundTask.cs**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-140">Name the new class **PushBackgroundTask.cs**.</span></span> <span data-ttu-id="ebaaa-141">Klicka på **Lägg till** att skapa klassen.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-141">Click **Add** to generate the class.</span></span>
4. <span data-ttu-id="ebaaa-142">Ersätt hela innehållet i den **PushBackgroundComponent** definitionen för namnområdet med följande kod, ersätter platshållaren `{back-end endpoint}` med backend-slutpunkten som erhålls vid distribution av din serverdel:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-142">Replace the entire contents of the **PushBackgroundComponent** namespace definition with the following code, substituting the placeholder `{back-end endpoint}` with the back-end endpoint obtained while deploying your back-end:</span></span>
   
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
                    // Store the content received from the notification so it can be retrieved from the UI.
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
5. <span data-ttu-id="ebaaa-143">I Solution Explorer högerklickar du på den **PushBackgroundComponent (Windows Phone 8.1)** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-143">In Solution Explorer, right-click the **PushBackgroundComponent (Windows Phone 8.1)** project and then click **Manage NuGet Packages**.</span></span>
6. <span data-ttu-id="ebaaa-144">Klicka på den vänstra sidan **Online**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-144">On the left-hand side, click **Online**.</span></span>
7. <span data-ttu-id="ebaaa-145">I den **Sök** skriver **http-klienten**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-145">In the **Search** box, type **Http Client**.</span></span>
8. <span data-ttu-id="ebaaa-146">Klicka i resultatlistan **klientbibliotek för Microsoft HTTP**, och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-146">In the results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="ebaaa-147">Slutför installationen.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-147">Complete the installation.</span></span>
9. <span data-ttu-id="ebaaa-148">Tillbaka i NuGet **Sök** skriver **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-148">Back in the NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="ebaaa-149">Installera den **Json.NET** paketet och sedan stänga fönstret NuGet Package Manager.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-149">Install the **Json.NET** package, then close the NuGet Package Manager window.</span></span>
10. <span data-ttu-id="ebaaa-150">Lägg till följande `using` instruktioner överst i den **PushBackgroundTask.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-150">Add the following `using` statements at the top of the **PushBackgroundTask.cs** file:</span></span>
    
        using Windows.ApplicationModel.Background;
        using Windows.Networking.PushNotifications;
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Newtonsoft.Json;
        using Windows.UI.Notifications;
        using Windows.Data.Xml.Dom;
11. <span data-ttu-id="ebaaa-151">I Solution Explorer i den **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt, högerklicka på **referenser**, klicka på **Lägg till referens...** .</span><span class="sxs-lookup"><span data-stu-id="ebaaa-151">In Solution Explorer, in the **NotifyUserWindowsPhone (Windows Phone 8.1)** project, right-click **References**, then click **Add Reference...**.</span></span> <span data-ttu-id="ebaaa-152">I dialogrutan Reference Manager du kryssrutan bredvid **PushBackgroundComponent**, och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-152">In the Reference Manager dialog, check the box next to **PushBackgroundComponent**, and then click **OK**.</span></span>
12. <span data-ttu-id="ebaaa-153">I Solution Explorer dubbelklickar du på **Package.appxmanifest** i den **NotifyUserWindowsPhone (Windows Phone 8.1)** projekt.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-153">In Solution Explorer, double-click **Package.appxmanifest** in the **NotifyUserWindowsPhone (Windows Phone 8.1)** project.</span></span> <span data-ttu-id="ebaaa-154">Under **meddelanden**, ange **popup-kompatibel** till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-154">Under **Notifications**, set **Toast Capable** to **Yes**.</span></span>
    
    ![][3]
13. <span data-ttu-id="ebaaa-155">Fortfarande i **Package.appxmanifest**, klicka på den **deklarationer** menyn längst upp.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-155">Still in **Package.appxmanifest**, click the **Declarations** menu near the top.</span></span> <span data-ttu-id="ebaaa-156">I den **tillgängliga deklarationer** listrutan, klickar du på **bakgrundsaktiviteter**, och klicka sedan på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-156">In the **Available Declarations** dropdown, click **Background Tasks**, and then click **Add**.</span></span>
14. <span data-ttu-id="ebaaa-157">I **Package.appxmanifest**under **egenskaper**, kontrollera **Push notification**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-157">In **Package.appxmanifest**, under **Properties**, check **Push notification**.</span></span>
15. <span data-ttu-id="ebaaa-158">I **Package.appxmanifest**under **Appinställningar**, typen **PushBackgroundComponent.PushBackgroundTask** i den **startpunkten** fält.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-158">In **Package.appxmanifest**, under **App Settings**, type **PushBackgroundComponent.PushBackgroundTask** in the **Entry Point** field.</span></span>
    
    ![][13]
16. <span data-ttu-id="ebaaa-159">Från **Arkiv**-menyn klickar du på **Spara alla**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-159">From the **File** menu, click **Save All**.</span></span>

## <a name="run-the-application"></a><span data-ttu-id="ebaaa-160">Köra programmet</span><span class="sxs-lookup"><span data-stu-id="ebaaa-160">Run the Application</span></span>
<span data-ttu-id="ebaaa-161">Om du vill köra programmet, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="ebaaa-161">To run the application, do the following:</span></span>

1. <span data-ttu-id="ebaaa-162">I Visual Studio kör den **AppBackend** Web API-program.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-162">In Visual Studio, run the **AppBackend** Web API application.</span></span> <span data-ttu-id="ebaaa-163">En ASP.NET-webbsida visas.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-163">An ASP.NET web page is displayed.</span></span>
2. <span data-ttu-id="ebaaa-164">I Visual Studio kör den **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone-app.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-164">In Visual Studio, run the **NotifyUserWindowsPhone (Windows Phone 8.1)** Windows Phone app.</span></span> <span data-ttu-id="ebaaa-165">Windows Phone-emulatorn kör och appen läses in automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-165">The Windows Phone emulator runs and loads the app automatically.</span></span>
3. <span data-ttu-id="ebaaa-166">I den **NotifyUserWindowsPhone** app UI, ange ett användarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-166">In the **NotifyUserWindowsPhone** app UI, enter a username and password.</span></span> <span data-ttu-id="ebaaa-167">Det kan vara valfri sträng, men de måste ha samma värde.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-167">These can be any string, but they must be the same value.</span></span>
4. <span data-ttu-id="ebaaa-168">I den **NotifyUserWindowsPhone** app-Gränssnittet klickar du på **logga in och registrera**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-168">In the **NotifyUserWindowsPhone** app UI, click **Log in and register**.</span></span> <span data-ttu-id="ebaaa-169">Klicka på **skicka push**.</span><span class="sxs-lookup"><span data-stu-id="ebaaa-169">Then click **Send push**.</span></span>

[3]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push3.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-secure-push/notification-hubs-secure-push13.png
