---
title: "aaaAzure Meddelandeanvändare Notification Hubs med .NET-serverdel"
description: "Lär dig hur säker toosend push-meddelanden i Azure. Kodexempel som skrivits i C# med hjälp av hello .NET-API."
documentationcenter: windows
author: ysxu
manager: erikre
services: notification-hubs
editor: 
ms.assetid: 012529f2-fdbc-43c4-8634-2698164b5880
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 10/03/2016
ms.author: yuaxu
ms.openlocfilehash: a366181faa81e78adf4de61435ef2790c3aa29d1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a><span data-ttu-id="bcd7b-104">Azure Notification Hubs Meddelandeanvändare med .NET-serverdel</span><span class="sxs-lookup"><span data-stu-id="bcd7b-104">Azure Notification Hubs Notify Users with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="bcd7b-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="bcd7b-105">Overview</span></span>
<span data-ttu-id="bcd7b-106">Stöd för push-meddelanden i Azure kan du tooaccess ett enkelt att använda och multiplatform skalats ut push-infrastruktur, vilket förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-106">Push notification support in Azure enables you tooaccess an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies hello implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="bcd7b-107">Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa specifik app användare på en specifik enhet.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-107">This tutorial shows you how toouse Azure Notification Hubs toosend push notifications tooa specific app user on a specific device.</span></span> <span data-ttu-id="bcd7b-108">En ASP.NET-WebAPI-serverdel är används tooauthenticate klienter.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-108">An ASP.NET WebAPI backend is used tooauthenticate clients.</span></span> <span data-ttu-id="bcd7b-109">Hello autentiserade klientanvändare och tagg läggas automatiskt till av hello backend toonotification registrering.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-109">Using hello authenticated client user, and tag will be automatically added by hello backend toonotification registration.</span></span> <span data-ttu-id="bcd7b-110">Den här taggen kommer att använda toosend av hello backend toogenerate meddelanden för en viss användare.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-110">This tag will be used toosend by hello backend toogenerate notifications for a specific user.</span></span> <span data-ttu-id="bcd7b-111">Mer information om registrering för meddelanden med hjälp av en appserverdel avsnittet hello vägledning [registrering från din Apps serverdel](http://msdn.microsoft.com/library/dn743807.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcd7b-111">For more information on registering for notifications using an app backend, see hello guidance topic [Registering from your app backend](http://msdn.microsoft.com/library/dn743807.aspx).</span></span> <span data-ttu-id="bcd7b-112">Den här kursen bygger på hello meddelandehubb och projekt som du skapade i hello [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-112">This tutorial builds on hello notification hub and project that you created in hello [Get started with Notification Hubs] tutorial.</span></span>

<span data-ttu-id="bcd7b-113">Den här kursen är också hello nödvändiga toohello [Secure Push] kursen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-113">This tutorial is also hello prerequisite toohello [Secure Push] tutorial.</span></span> <span data-ttu-id="bcd7b-114">När du har slutfört hello stegen i den här självstudiekursen, kan du fortsätta toohello [Secure Push] kursen visar hur toomodify hello kod i den här självstudiekursen toosend ett push-meddelande på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-114">After you have completed hello steps in this tutorial, you can proceed toohello [Secure Push] tutorial, which shows how toomodify hello code in this tutorial toosend a push notification securely.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="bcd7b-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="bcd7b-115">Before you begin</span></span>
<span data-ttu-id="bcd7b-116">Vi tar din feedback på allvar.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-116">We do take your feedback seriously.</span></span> <span data-ttu-id="bcd7b-117">Om du har slutfört det här ämnet, eller för att förbättra det här innehållet problem, skulle vi uppskattar din feedback på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-117">If you have any difficulties completing this topic, or recommendations for improving this content, we would appreciate your feedback at hello bottom of hello page.</span></span>

<span data-ttu-id="bcd7b-118">hello slutförts koden för den här kursen finns på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span><span class="sxs-lookup"><span data-stu-id="bcd7b-118">hello completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="bcd7b-119">Krav</span><span class="sxs-lookup"><span data-stu-id="bcd7b-119">Prerequisites</span></span>
<span data-ttu-id="bcd7b-120">Innan du börjar den här självstudiekursen, måste du redan har slutfört de här kurserna i Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="bcd7b-120">Before you start this tutorial, you must have already completed these Mobile Services tutorials:</span></span>

* <span data-ttu-id="bcd7b-121">[Kom igång med Notification Hubs]</span><span class="sxs-lookup"><span data-stu-id="bcd7b-121">[Get started with Notification Hubs]</span></span><br/><span data-ttu-id="bcd7b-122">Du skapar din meddelandehubb och reservera appnamn hälsningspaket och registrera tooreceive meddelanden i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-122">You create your notification hub and reserve hello app name and register tooreceive notifications in this tutorial.</span></span> <span data-ttu-id="bcd7b-123">Den här kursen förutsätter att du redan har slutfört de här stegen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-123">This tutorial assumes you have already completed these steps.</span></span> <span data-ttu-id="bcd7b-124">Annars följer du stegen hello i [komma igång med Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); specifikt hello avsnitt [registrera din app för Windows Store för hello](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) och [konfigurera din Meddelandehubb](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span><span class="sxs-lookup"><span data-stu-id="bcd7b-124">If not, please follow hello steps in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); specifically, hello sections [Register your app for hello Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) and [Configure your Notification Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span></span> <span data-ttu-id="bcd7b-125">Särskilt, se till att du har angett hello **paket-SID** och **Klienthemlighet** värden i hello-portalen i hello **konfigurera** för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-125">In particular, make sure that you have entered hello **Package SID** and **Client Secret** values in hello portal, in hello **Configure** tab for your notification hub.</span></span> <span data-ttu-id="bcd7b-126">Den här proceduren konfiguration beskrivs i hello [konfigurera din Meddelandehubb](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span><span class="sxs-lookup"><span data-stu-id="bcd7b-126">This configuration procedure is described in hello section [Configure your Notification Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span></span> <span data-ttu-id="bcd7b-127">Detta är ett viktigt steg: om hello autentiseringsuppgifter på hello portal inte matchar dem som specificerats för hello appnamn du väljer, hello push-meddelande kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-127">This is an important step: if hello credentials on hello portal do not match those specified for hello app name you choose, hello push notification will not succeed.</span></span>

> [!NOTE]
> <span data-ttu-id="bcd7b-128">Om du använder Mobile Apps i Azure App Service som backend-tjänst finns hello [Mobile Apps version](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-128">If you are using Mobile Apps in Azure App Service as your backend service, see hello [Mobile Apps version](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) of this tutorial.</span></span>
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-hello-code-for-hello-client-project"></a><span data-ttu-id="bcd7b-129">Uppdatera hello kod för hello klientprojektet</span><span class="sxs-lookup"><span data-stu-id="bcd7b-129">Update hello code for hello client project</span></span>
<span data-ttu-id="bcd7b-130">I det här avsnittet kan du uppdatera hello koden i hello-projekt som du har slutfört för hello [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-130">In this section, you update hello code in hello project you completed for hello [Get started with Notification Hubs] tutorial.</span></span> <span data-ttu-id="bcd7b-131">hello bör redan associerad med hello store och konfigurerats för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-131">hello should already be associated with hello store and configured for your notification hub.</span></span> <span data-ttu-id="bcd7b-132">I det här avsnittet ska du lägga till kod-toocall hello nya WebAPI-serverdel och använda den för registrering och skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-132">In this section, you will add code toocall hello new WebAPI backend and use it for registering and sending notifications.</span></span>

1. <span data-ttu-id="bcd7b-133">Öppna i Visual Studio hello hello lösningen som du skapade för hello [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-133">In Visual Studio, open hello hello solution you created for hello [Get started with Notification Hubs] tutorial.</span></span>
2. <span data-ttu-id="bcd7b-134">I Solution Explorer högerklickar du på hello **(Windows 8.1)** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-134">In Solution Explorer, right-click hello **(Windows 8.1)** project and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="bcd7b-135">Hello vänster, klickar du på **Online**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-135">On hello left-hand side, click **Online**.</span></span>
4. <span data-ttu-id="bcd7b-136">I hello **Sök** skriver **http-klienten**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-136">In hello **Search** box, type **Http Client**.</span></span>
5. <span data-ttu-id="bcd7b-137">I listan över sökresultat hello klickar du på **klientbibliotek för Microsoft HTTP**, och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-137">In hello results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="bcd7b-138">Slutför hello-installationen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-138">Complete hello installation.</span></span>
6. <span data-ttu-id="bcd7b-139">Tillbaka i hello NuGet **Sök** skriver **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-139">Back in hello NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="bcd7b-140">Installera hello **Json.NET** paket- och stänger sedan hello NuGet Package Manager-fönstret.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-140">Install hello **Json.NET** package, and then close hello NuGet Package Manager window.</span></span>
7. <span data-ttu-id="bcd7b-141">Upprepa hello stegen ovan för hello **(Windows Phone 8.1)** projekt tooinstall hello **JSON.NET** NuGet-paketet för hello Windows Phone-projekt.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-141">Repeat hello steps above for hello **(Windows Phone 8.1)** project tooinstall hello **JSON.NET** NuGet package for hello Windows Phone project.</span></span>
8. <span data-ttu-id="bcd7b-142">I Solution Explorer i hello **(Windows 8.1)** projektet genom att dubbelklicka på **MainPage.xaml** tooopen i hello Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-142">In Solution Explorer, in hello **(Windows 8.1)** project, double-click **MainPage.xaml** tooopen it in hello Visual Studio editor.</span></span>
9. <span data-ttu-id="bcd7b-143">I hello **MainPage.xaml** XML-kod, Ersätt hello `<Grid>` avsnitt med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-143">In hello **MainPage.xaml** XML code, replace hello `<Grid>` section with hello following code.</span></span> <span data-ttu-id="bcd7b-144">Den här koden lägger till en textruta för användarnamn och lösenord som hello användaren autentiseras med.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-144">This code adds a username and password textbox that hello user will authenticate with.</span></span> <span data-ttu-id="bcd7b-145">Det ger också textrutor för hello-meddelande och hello username-tagg som hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="bcd7b-145">It also adds textboxes for hello notification message and hello username tag that should receive hello notification:</span></span>
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition Height="Auto"/>
                <RowDefinition Height="*"/>
            </Grid.RowDefinitions>
   
            <TextBlock Grid.Row="0" Text="Notify Users" HorizontalAlignment="Center" FontSize="48"/>
   
            <StackPanel Grid.Row="1" VerticalAlignment="Center">
                <Grid>
                    <Grid.RowDefinitions>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                        <RowDefinition Height="Auto"/>
                    </Grid.RowDefinitions>
                    <Grid.ColumnDefinitions>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                        <ColumnDefinition></ColumnDefinition>
                    </Grid.ColumnDefinitions>
                    <TextBlock Grid.Row="0" Grid.ColumnSpan="3" Text="Username" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="UsernameTextBox" Grid.Row="1" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
                    <TextBlock Grid.Row="2" Grid.ColumnSpan="3" Text="Password" FontSize="24" Margin="20,0,20,0" />
                    <PasswordBox Name="PasswordTextBox" Grid.Row="3" Grid.ColumnSpan="3" Margin="20,0,20,0"/>
   
                    <Button Grid.Row="4" Grid.ColumnSpan="3" HorizontalAlignment="Center" VerticalAlignment="Center"
                                Content="1. Login and register" Click="LoginAndRegisterClick" Margin="0,0,0,20"/>
   
                    <ToggleButton Name="toggleWNS" Grid.Row="5" Grid.Column="0" HorizontalAlignment="Right" Content="WNS" IsChecked="True" />
                    <ToggleButton Name="toggleGCM" Grid.Row="5" Grid.Column="1" HorizontalAlignment="Center" Content="GCM" />
                    <ToggleButton Name="toggleAPNS" Grid.Row="5" Grid.Column="2" HorizontalAlignment="Left" Content="APNS" />
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag tooSend To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. <span data-ttu-id="bcd7b-146">I Solution Explorer i hello **(Windows Phone 8.1)** projektet öppnar **MainPage.xaml** och Ersätt hello Windows Phone 8.1 `<Grid>` avsnitt med samma koden ovan.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-146">In Solution Explorer, in hello **(Windows Phone 8.1)** project, open **MainPage.xaml** and replace hello Windows Phone 8.1 `<Grid>` section with that same code above.</span></span> <span data-ttu-id="bcd7b-147">hello-gränssnittet ska se ut ungefär toowhats visas nedan.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-147">hello interface should look similar toowhats shown below.</span></span>
    
    ![][13]
11. <span data-ttu-id="bcd7b-148">I Solution Explorer öppnar du hello **MainPage.xaml.cs** -filen för hello **(Windows 8.1)** och **(Windows Phone 8.1)** projekt.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-148">In Solution Explorer, open hello **MainPage.xaml.cs** file for hello **(Windows 8.1)** and **(Windows Phone 8.1)** projects.</span></span> <span data-ttu-id="bcd7b-149">Lägg till följande hello `using` instruktioner överst hello i båda filerna:</span><span class="sxs-lookup"><span data-stu-id="bcd7b-149">Add hello following `using` statements at hello top of both files:</span></span>
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. <span data-ttu-id="bcd7b-150">I **MainPage.xaml.cs** för hello **(Windows 8.1)** och **(Windows Phone 8.1)** projekt, Lägg till följande medlem toohello hello `MainPage` klass.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-150">In **MainPage.xaml.cs** for hello **(Windows 8.1)** and **(Windows Phone 8.1)** projects, add hello following member toohello `MainPage` class.</span></span> <span data-ttu-id="bcd7b-151">Vara säker på att tooreplace `<Enter Your Backend Endpoint>` med hello faktiska backend-slutpunkt hämtats tidigare.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-151">Be sure tooreplace `<Enter Your Backend Endpoint>` with hello your actual backend endpoint obtained previously.</span></span> <span data-ttu-id="bcd7b-152">Till exempel `http://mybackend.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-152">For example, `http://mybackend.azurewebsites.net`.</span></span>
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. <span data-ttu-id="bcd7b-153">Lägg till hello kod nedan toohello MainPage klassen i **MainPage.xaml.cs** för hello **(Windows 8.1)** och **(Windows Phone 8.1)** projekt.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-153">Add hello code below toohello MainPage class in **MainPage.xaml.cs** for hello **(Windows 8.1)** and **(Windows Phone 8.1)** projects.</span></span>
    
    <span data-ttu-id="bcd7b-154">Hej `PushClick` metoden är hello klickar du på hanterare för hello **skicka Push-** knappen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-154">hello `PushClick` method is hello click handler for hello **Send Push** button.</span></span> <span data-ttu-id="bcd7b-155">Anropar hello backend tootrigger ett meddelande tooall enheter med en tagg för användarnamn som matchar hello `to_tag` parameter.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-155">It calls hello backend tootrigger a notification tooall devices with a username tag that matches hello `to_tag` parameter.</span></span> <span data-ttu-id="bcd7b-156">hello-meddelande skickas som JSON-innehåll i hello frågans brödtext.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-156">hello notification message is sent as JSON content in hello request body.</span></span>
    
    <span data-ttu-id="bcd7b-157">Hej `LoginAndRegisterClick` metoden är hello klickar du på hanterare för hello **logga in och registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-157">hello `LoginAndRegisterClick` method is hello click handler for hello **Log in and register** button.</span></span> <span data-ttu-id="bcd7b-158">Lagrar den grundläggande hello autentiseringstoken i lokal lagring (Observera att detta motsvarar alla token som använder din autentiseringsschema), använder sedan `RegisterClient` tooregister för meddelanden med hello backend.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-158">It stores hello basic authentication token in local storage (note that this represents any token your authentication scheme uses), then uses `RegisterClient` tooregister for notifications using hello backend.</span></span>

        private async void PushClick(object sender, RoutedEventArgs e)
        {
            if (toggleWNS.IsChecked.Value)
            {
                await sendPush("wns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleGCM.IsChecked.Value)
            {
                await sendPush("gcm", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);
            }
            if (toggleAPNS.IsChecked.Value)
            {
                await sendPush("apns", ToUserTagTextBox.Text, this.NotificationMessageTextBox.Text);

            }
        }

        private async Task sendPush(string pns, string userTag, string message)
        {
            var POST_URL = BACKEND_ENDPOINT + "/api/notifications?pns=" +
                pns + "&to_tag=" + userTag;

            using (var httpClient = new HttpClient())
            {
                var settings = ApplicationData.Current.LocalSettings.Values;
                httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);

                try
                {
                    await httpClient.PostAsync(POST_URL, new StringContent("\"" + message + "\"",
                        System.Text.Encoding.UTF8, "application/json"));
                }
                catch (Exception ex)
                {
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed toosend " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // hello "username:<user name>" tag gets automatically added by hello message handler in hello backend.
            // hello tag passed here can be whatever other tags you may want toouse.
            try
            {
                // hello device handle used will be different depending on hello device and PNS. 
                // Windows devices use hello channel uri as hello PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed tooregister with RegisterClient");
                alert.ShowAsync();
            }
        }

        private void SetAuthenticationTokenInLocalStorage()
        {
            string username = UsernameTextBox.Text;
            string password = PasswordTextBox.Password;

            var token = Convert.ToBase64String(System.Text.Encoding.UTF8.GetBytes(username + ":" + password));
            ApplicationData.Current.LocalSettings.Values["AuthenticationToken"] = token;
        }



1. <span data-ttu-id="bcd7b-159">I Solution Explorer under hello **delade** projekt, öppna hello **App.xaml.cs** fil.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-159">In Solution Explorer, under hello **Shared** project, open hello **App.xaml.cs** file.</span></span> <span data-ttu-id="bcd7b-160">Hitta hello anrop för`InitNotificationsAsync()` i hello `OnLaunched()` händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-160">Find hello call too`InitNotificationsAsync()` in hello `OnLaunched()` event handler.</span></span> <span data-ttu-id="bcd7b-161">Kommentera ut eller ta bort hello anrop för`InitNotificationsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-161">Comment out or delete hello call too`InitNotificationsAsync()`.</span></span> <span data-ttu-id="bcd7b-162">hello knappen hanteraren lades till ovan ska initiera notification registreringar.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-162">hello button handler added above will initialize notification registrations.</span></span>

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. <span data-ttu-id="bcd7b-163">I Solution Explorer högerklickar du på hello **delade** projektet och klicka sedan på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-163">In Solution Explorer, right-click hello **Shared** project, then click **Add**, and then click **Class**.</span></span> <span data-ttu-id="bcd7b-164">Namnge hello klassen **RegisterClient.cs**, klicka på **OK** toogenerate hello klass.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-164">Name hello class **RegisterClient.cs**, then click **OK** toogenerate hello class.</span></span>
   
   <span data-ttu-id="bcd7b-165">Den här klassen radbryts hello REST-anrop krävs toocontact hello app backend i ordning tooregister för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-165">This class will wrap hello REST calls required toocontact hello app backend, in order tooregister for push notifications.</span></span> <span data-ttu-id="bcd7b-166">Den lagrar även lokalt hello *registrationIds* skapas av hello Notification Hub enligt anvisningarna i [registrering från din Apps serverdel](http://msdn.microsoft.com/library/dn743807.aspx).</span><span class="sxs-lookup"><span data-stu-id="bcd7b-166">It also locally stores hello *registrationIds* created by hello Notification Hub as detailed in [Registering from your app backend](http://msdn.microsoft.com/library/dn743807.aspx).</span></span> <span data-ttu-id="bcd7b-167">Observera att den använder en autentiseringstoken som lagras i lokal lagring när du klickar på hello **logga in och registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-167">Note that it uses an authorization token stored in local storage when you click hello **Log in and register** button.</span></span>
2. <span data-ttu-id="bcd7b-168">Lägg till följande hello `using` instruktioner överst hello i hello RegisterClient.cs fil:</span><span class="sxs-lookup"><span data-stu-id="bcd7b-168">Add hello following `using` statements at hello top of hello RegisterClient.cs file:</span></span>
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. <span data-ttu-id="bcd7b-169">Lägg till följande kod i hello hello `RegisterClient` klassen definition.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-169">Add hello following code inside hello `RegisterClient` class definition.</span></span>
   
       private string POST_URL;
   
       private class DeviceRegistration
       {
           public string Platform { get; set; }
           public string Handle { get; set; }
           public string[] Tags { get; set; }
       }
   
       public RegisterClient(string backendEndpoint)
       {
           POST_URL = backendEndpoint + "/api/register";
       }
   
       public async Task RegisterAsync(string handle, IEnumerable<string> tags)
       {
           var regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
   
           var deviceRegistration = new DeviceRegistration
           {
               Platform = "wns",
               Handle = handle,
               Tags = tags.ToArray<string>()
           };
   
           var statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
   
           if (statusCode == HttpStatusCode.Gone)
           {
               // regId is expired, deleting from local storage & recreating
               var settings = ApplicationData.Current.LocalSettings.Values;
               settings.Remove("__NHRegistrationId");
               regId = await RetrieveRegistrationIdOrRequestNewOneAsync();
               statusCode = await UpdateRegistrationAsync(regId, deviceRegistration);
           }
   
           if (statusCode != HttpStatusCode.Accepted)
           {
               // log or throw
               throw new System.Net.WebException(statusCode.ToString());
           }
       }
   
       private async Task<HttpStatusCode> UpdateRegistrationAsync(string regId, DeviceRegistration deviceRegistration)
       {
           using (var httpClient = new HttpClient())
           {
               var settings = ApplicationData.Current.LocalSettings.Values;
               httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string) settings["AuthenticationToken"]);
   
               var putUri = POST_URL + "/" + regId;
   
               string json = JsonConvert.SerializeObject(deviceRegistration);
                               var response = await httpClient.PutAsync(putUri, new StringContent(json, Encoding.UTF8, "application/json"));
               return response.StatusCode;
           }
       }
   
       private async Task<string> RetrieveRegistrationIdOrRequestNewOneAsync()
       {
           var settings = ApplicationData.Current.LocalSettings.Values;
           if (!settings.ContainsKey("__NHRegistrationId"))
           {
               using (var httpClient = new HttpClient())
               {
                   httpClient.DefaultRequestHeaders.Authorization = new AuthenticationHeaderValue("Basic", (string)settings["AuthenticationToken"]);
   
                   var response = await httpClient.PostAsync(POST_URL, new StringContent(""));
                   if (response.IsSuccessStatusCode)
                   {
                       string regId = await response.Content.ReadAsStringAsync();
                       regId = regId.Substring(1, regId.Length - 2);
                       settings.Add("__NHRegistrationId", regId);
                   }
                   else
                   {
                       throw new System.Net.WebException(response.StatusCode.ToString());
                   }
               }
           }
           return (string)settings["__NHRegistrationId"];
   
       }
4. <span data-ttu-id="bcd7b-170">Spara alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-170">Save all your changes.</span></span>

## <a name="testing-hello-application"></a><span data-ttu-id="bcd7b-171">Testa hello program</span><span class="sxs-lookup"><span data-stu-id="bcd7b-171">Testing hello Application</span></span>
1. <span data-ttu-id="bcd7b-172">Starta hello program både på Windows 8.1 och Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-172">Launch hello application on both Windows 8.1 and Windows Phone 8.1.</span></span> <span data-ttu-id="bcd7b-173">Du kan köra hello instans i hello emulator eller en enhet för Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-173">For Windows Phone 8.1 you can run hello instance in hello emulator or an actual device.</span></span>
2. <span data-ttu-id="bcd7b-174">Ange i hello Windows 8.1 instans av hello app, en **användarnamn** och **lösenord** enligt hello-skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-174">In hello Windows 8.1 instance of hello app, enter a **Username** and **Password** as shown in hello screen below.</span></span> <span data-ttu-id="bcd7b-175">Den måste skilja sig från hello användarnamn och lösenord som du anger på Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-175">It should differ from hello user name and password you enter on Windows Phone.</span></span>
3. <span data-ttu-id="bcd7b-176">Klicka på **logga in och registrera** och verifiera en dialogruta visar att du har loggat in.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-176">Click **Log in and register** and verify a dialog shows that you have logged in.</span></span> <span data-ttu-id="bcd7b-177">Detta aktiverar också hello **skicka Push-** knappen.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-177">This will also enable hello **Send Push** button.</span></span>
   
    ![][14]
4. <span data-ttu-id="bcd7b-178">Ange en användare namnsträngen på hello Windows Phone 8.1-instansen i både hello **användarnamn** och **lösenord** fält klicka **inloggning och registrera**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-178">On hello Windows Phone 8.1 instance, enter a user name string in both hello **Username** and **Password** fields then click **Login and register**.</span></span>
5. <span data-ttu-id="bcd7b-179">I hello **mottagaren användarnamn taggen** anger hello användarnamn som är registrerad på Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-179">Then in hello **Recipient Username Tag** field, enter hello user name registered on Windows 8.1.</span></span> <span data-ttu-id="bcd7b-180">Ange ett meddelande och klicka på **skicka Push-**.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-180">Enter a notification message and click **Send Push**.</span></span>
   
    ![][16]
6. <span data-ttu-id="bcd7b-181">Endast hello-enheter som har registrerats med hello matchande användarnamn tagg får hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="bcd7b-181">Only hello devices that have registered with hello matching username tag receive hello notification message.</span></span>
   
    ![][15]

## <a name="next-steps"></a><span data-ttu-id="bcd7b-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bcd7b-182">Next Steps</span></span>
* <span data-ttu-id="bcd7b-183">Om du vill toosegment in användarna efter intressegrupper, se [använda Notification Hubs toosend senaste nytt].</span><span class="sxs-lookup"><span data-stu-id="bcd7b-183">If you want toosegment your users by interest groups, see [Use Notification Hubs toosend breaking news].</span></span>
* <span data-ttu-id="bcd7b-184">Mer om hur toolearn toouse Notification Hubs finns [riktlinjer om Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="bcd7b-184">toolearn more about how toouse Notification Hubs, see [Notification Hubs Guidance].</span></span>

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
[Kom igång med Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md
[använda Notification Hubs toosend senaste nytt]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md
[riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx
