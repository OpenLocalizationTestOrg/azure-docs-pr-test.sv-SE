---
title: "Azure Notification Hubs Meddelandeanvändare med .NET-serverdel"
description: "Lär dig mer om att skicka säkra push-meddelanden i Azure. Kodexempel som skrivits i C# med hjälp av .NET-API."
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
ms.openlocfilehash: c0b963ef661612b1a176dd8e5f01d56e61eb5acb
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a><span data-ttu-id="c3f78-104">Azure Notification Hubs Meddelandeanvändare med .NET-serverdel</span><span class="sxs-lookup"><span data-stu-id="c3f78-104">Azure Notification Hubs Notify Users with .NET backend</span></span>
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a><span data-ttu-id="c3f78-105">Översikt</span><span class="sxs-lookup"><span data-stu-id="c3f78-105">Overview</span></span>
<span data-ttu-id="c3f78-106">Stöd för push-meddelanden i Azure kan du få ett enkelt att använda och multiplatform skalats ut push-infrastruktur, vilket förenklar implementeringen av push-meddelanden för konsument- och enterprise-program för mobila plattformar.</span><span class="sxs-lookup"><span data-stu-id="c3f78-106">Push notification support in Azure enables you to access an easy-to-use, multiplatform, and scaled-out push infrastructure, which greatly simplifies the implementation of push notifications for both consumer and enterprise applications for mobile platforms.</span></span> <span data-ttu-id="c3f78-107">Denna självstudie visar hur du använder Azure-meddelandehubbar för att skicka push-meddelanden till en specifik app-användare på en viss enhet.</span><span class="sxs-lookup"><span data-stu-id="c3f78-107">This tutorial shows you how to use Azure Notification Hubs to send push notifications to a specific app user on a specific device.</span></span> <span data-ttu-id="c3f78-108">En ASP.NET-WebAPI-serverdel används för att autentisera klienter.</span><span class="sxs-lookup"><span data-stu-id="c3f78-108">An ASP.NET WebAPI backend is used to authenticate clients.</span></span> <span data-ttu-id="c3f78-109">Med hjälp av autentiserade klientanvändare och tagg automatiskt läggas till av serverdelen registreringen av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c3f78-109">Using the authenticated client user, and tag will be automatically added by the backend to notification registration.</span></span> <span data-ttu-id="c3f78-110">Den här etiketten används för att skicka av serverdelen att generera meddelanden för en viss användare.</span><span class="sxs-lookup"><span data-stu-id="c3f78-110">This tag will be used to send by the backend to generate notifications for a specific user.</span></span> <span data-ttu-id="c3f78-111">Mer information om registrering för meddelanden med hjälp av en appserverdel finns i avsnittet vägledning [registrering från din Apps serverdel](http://msdn.microsoft.com/library/dn743807.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3f78-111">For more information on registering for notifications using an app backend, see the guidance topic [Registering from your app backend](http://msdn.microsoft.com/library/dn743807.aspx).</span></span> <span data-ttu-id="c3f78-112">Den här kursen bygger på meddelandehubben och projekt som du skapade i den [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-112">This tutorial builds on the notification hub and project that you created in the [Get started with Notification Hubs] tutorial.</span></span>

<span data-ttu-id="c3f78-113">Den här kursen är också nödvändigt för att den [Secure Push] kursen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-113">This tutorial is also the prerequisite to the [Secure Push] tutorial.</span></span> <span data-ttu-id="c3f78-114">När du har slutfört stegen i den här självstudiekursen, kan du fortsätta till den [Secure Push] kursen visar hur du ändrar koden i självstudierna för att skicka ett push-meddelande på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="c3f78-114">After you have completed the steps in this tutorial, you can proceed to the [Secure Push] tutorial, which shows how to modify the code in this tutorial to send a push notification securely.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c3f78-115">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c3f78-115">Before you begin</span></span>
<span data-ttu-id="c3f78-116">Vi tar din feedback på allvar.</span><span class="sxs-lookup"><span data-stu-id="c3f78-116">We do take your feedback seriously.</span></span> <span data-ttu-id="c3f78-117">Om du har problem med att slutföra det här ämnet, eller om du har tips på hur innehållet kan förbättras, tar vi tacksamt emot din feedback längst ner på sidan.</span><span class="sxs-lookup"><span data-stu-id="c3f78-117">If you have any difficulties completing this topic, or recommendations for improving this content, we would appreciate your feedback at the bottom of the page.</span></span>

<span data-ttu-id="c3f78-118">Den färdiga koden för den här självstudiekursen hittar du på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span><span class="sxs-lookup"><span data-stu-id="c3f78-118">The completed code for this tutorial can be found on GitHub [here](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="c3f78-119">Krav</span><span class="sxs-lookup"><span data-stu-id="c3f78-119">Prerequisites</span></span>
<span data-ttu-id="c3f78-120">Innan du börjar den här självstudiekursen, måste du redan har slutfört de här kurserna i Mobile Services:</span><span class="sxs-lookup"><span data-stu-id="c3f78-120">Before you start this tutorial, you must have already completed these Mobile Services tutorials:</span></span>

* <span data-ttu-id="c3f78-121">[Kom igång med Notification Hubs]</span><span class="sxs-lookup"><span data-stu-id="c3f78-121">[Get started with Notification Hubs]</span></span><br/><span data-ttu-id="c3f78-122">Du skapar din meddelandehubb och reservera namnet på appen och registrera dig för att ta emot meddelanden i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-122">You create your notification hub and reserve the app name and register to receive notifications in this tutorial.</span></span> <span data-ttu-id="c3f78-123">Den här kursen förutsätter att du redan har slutfört de här stegen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-123">This tutorial assumes you have already completed these steps.</span></span> <span data-ttu-id="c3f78-124">Om inte, följer du stegen i [komma igång med Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); specifikt avsnitten [registrera din app för Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) och [konfigurera din Meddelandehubb](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span><span class="sxs-lookup"><span data-stu-id="c3f78-124">If not, please follow the steps in [Getting Started with Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); specifically, the sections [Register your app for the Windows Store](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) and [Configure your Notification Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span></span> <span data-ttu-id="c3f78-125">Särskilt, se till att du har angett den **paket-SID** och **Klienthemlighet** i portalen av värdena i den **konfigurera** för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="c3f78-125">In particular, make sure that you have entered the **Package SID** and **Client Secret** values in the portal, in the **Configure** tab for your notification hub.</span></span> <span data-ttu-id="c3f78-126">Den här proceduren konfiguration beskrivs i avsnittet [konfigurera din Meddelandehubb](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span><span class="sxs-lookup"><span data-stu-id="c3f78-126">This configuration procedure is described in the section [Configure your Notification Hub](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub).</span></span> <span data-ttu-id="c3f78-127">Detta är ett viktigt steg: om autentiseringsuppgifter på portalen inte matchar dem som anges för namnet på appen du väljer, push-meddelande kommer att misslyckas.</span><span class="sxs-lookup"><span data-stu-id="c3f78-127">This is an important step: if the credentials on the portal do not match those specified for the app name you choose, the push notification will not succeed.</span></span>

> [!NOTE]
> <span data-ttu-id="c3f78-128">Om du använder Mobile Apps i Azure App Service som backend-tjänst finns i [Mobile Apps version](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) i den här kursen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-128">If you are using Mobile Apps in Azure App Service as your backend service, see the [Mobile Apps version](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) of this tutorial.</span></span>
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-the-code-for-the-client-project"></a><span data-ttu-id="c3f78-129">Uppdatera koden för klientprojektet</span><span class="sxs-lookup"><span data-stu-id="c3f78-129">Update the code for the client project</span></span>
<span data-ttu-id="c3f78-130">I det här avsnittet kan du uppdatera koden i projektet som du har slutförts för den [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-130">In this section, you update the code in the project you completed for the [Get started with Notification Hubs] tutorial.</span></span> <span data-ttu-id="c3f78-131">Den redan vara som är kopplade till arkivet och konfigurerats för meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="c3f78-131">The should already be associated with the store and configured for your notification hub.</span></span> <span data-ttu-id="c3f78-132">I det här avsnittet kan du lägga till kod för att anropa den nya WebAPI-serverdelen och använda den för registrering och skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c3f78-132">In this section, you will add code to call the new WebAPI backend and use it for registering and sending notifications.</span></span>

1. <span data-ttu-id="c3f78-133">Öppna i Visual Studio den lösning som du skapade för den [Kom igång med Notification Hubs] kursen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-133">In Visual Studio, open the the solution you created for the [Get started with Notification Hubs] tutorial.</span></span>
2. <span data-ttu-id="c3f78-134">I Solution Explorer högerklickar du på den **(Windows 8.1)** projektet och klicka sedan på **hantera NuGet-paket**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-134">In Solution Explorer, right-click the **(Windows 8.1)** project and then click **Manage NuGet Packages**.</span></span>
3. <span data-ttu-id="c3f78-135">Klicka på den vänstra sidan **Online**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-135">On the left-hand side, click **Online**.</span></span>
4. <span data-ttu-id="c3f78-136">I den **Sök** skriver **http-klienten**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-136">In the **Search** box, type **Http Client**.</span></span>
5. <span data-ttu-id="c3f78-137">Klicka i resultatlistan **klientbibliotek för Microsoft HTTP**, och klicka sedan på **installera**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-137">In the results list, click **Microsoft HTTP Client Libraries**, and then click **Install**.</span></span> <span data-ttu-id="c3f78-138">Slutför installationen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-138">Complete the installation.</span></span>
6. <span data-ttu-id="c3f78-139">Tillbaka i NuGet **Sök** skriver **Json.net**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-139">Back in the NuGet **Search** box, type **Json.net**.</span></span> <span data-ttu-id="c3f78-140">Installera den **Json.NET** paketera och stäng sedan NuGet Package Manager-fönstret.</span><span class="sxs-lookup"><span data-stu-id="c3f78-140">Install the **Json.NET** package, and then close the NuGet Package Manager window.</span></span>
7. <span data-ttu-id="c3f78-141">Upprepa stegen ovan för den **(Windows Phone 8.1)** projekt för att installera den **JSON.NET** NuGet-paket för Windows Phone-projektet.</span><span class="sxs-lookup"><span data-stu-id="c3f78-141">Repeat the steps above for the **(Windows Phone 8.1)** project to install the **JSON.NET** NuGet package for the Windows Phone project.</span></span>
8. <span data-ttu-id="c3f78-142">I Solution Explorer i den **(Windows 8.1)** projektet genom att dubbelklicka på **MainPage.xaml** att öppna den i Visual Studio-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="c3f78-142">In Solution Explorer, in the **(Windows 8.1)** project, double-click **MainPage.xaml** to open it in the Visual Studio editor.</span></span>
9. <span data-ttu-id="c3f78-143">I den **MainPage.xaml** XML-kod, ersätter den `<Grid>` avsnitt med följande kod.</span><span class="sxs-lookup"><span data-stu-id="c3f78-143">In the **MainPage.xaml** XML code, replace the `<Grid>` section with the following code.</span></span> <span data-ttu-id="c3f78-144">Den här koden lägger till en textruta för användarnamn och lösenord som användaren autentiseras med.</span><span class="sxs-lookup"><span data-stu-id="c3f78-144">This code adds a username and password textbox that the user will authenticate with.</span></span> <span data-ttu-id="c3f78-145">Det ger också textrutor för meddelandet och username-tagg som ska ta emot meddelandet:</span><span class="sxs-lookup"><span data-stu-id="c3f78-145">It also adds textboxes for the notification message and the username tag that should receive the notification:</span></span>
   
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
   
                    <TextBlock Grid.Row="6" Grid.ColumnSpan="3" Text="Username Tag To Send To" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="ToUserTagTextBox" Grid.Row="7" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <TextBlock Grid.Row="8" Grid.ColumnSpan="3" Text="Enter Notification Message" FontSize="24" Margin="20,0,20,0"/>
                    <TextBox Name="NotificationMessageTextBox" Grid.Row="9" Grid.ColumnSpan="3" Margin="20,0,20,0" TextWrapping="Wrap" />
                    <Button Grid.Row="10" Grid.ColumnSpan="3" HorizontalAlignment="Center" Content="2. Send push" Click="PushClick" Name="SendPushButton" />
                </Grid>
            </StackPanel>
        </Grid>
10. <span data-ttu-id="c3f78-146">I Solution Explorer i den **(Windows Phone 8.1)** projektet öppnar **MainPage.xaml** och ersätter Windows Phone 8.1 `<Grid>` avsnitt med samma koden ovan.</span><span class="sxs-lookup"><span data-stu-id="c3f78-146">In Solution Explorer, in the **(Windows Phone 8.1)** project, open **MainPage.xaml** and replace the Windows Phone 8.1 `<Grid>` section with that same code above.</span></span> <span data-ttu-id="c3f78-147">Gränssnittet bör likna nyheter som visas nedan.</span><span class="sxs-lookup"><span data-stu-id="c3f78-147">The interface should look similar to whats shown below.</span></span>
    
    ![][13]
11. <span data-ttu-id="c3f78-148">I Solution Explorer öppnar den **MainPage.xaml.cs** för den **(Windows 8.1)** och **(Windows Phone 8.1)** projekt.</span><span class="sxs-lookup"><span data-stu-id="c3f78-148">In Solution Explorer, open the **MainPage.xaml.cs** file for the **(Windows 8.1)** and **(Windows Phone 8.1)** projects.</span></span> <span data-ttu-id="c3f78-149">Lägg till följande `using` instruktioner överst i båda filerna:</span><span class="sxs-lookup"><span data-stu-id="c3f78-149">Add the following `using` statements at the top of both files:</span></span>
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. <span data-ttu-id="c3f78-150">I **MainPage.xaml.cs** för den **(Windows 8.1)** och **(Windows Phone 8.1)** projekt, Lägg till följande medlem till den `MainPage` klass.</span><span class="sxs-lookup"><span data-stu-id="c3f78-150">In **MainPage.xaml.cs** for the **(Windows 8.1)** and **(Windows Phone 8.1)** projects, add the following member to the `MainPage` class.</span></span> <span data-ttu-id="c3f78-151">Se till att ersätta `<Enter Your Backend Endpoint>` med den faktiska backend-slutpunkt som hämtats tidigare.</span><span class="sxs-lookup"><span data-stu-id="c3f78-151">Be sure to replace `<Enter Your Backend Endpoint>` with the your actual backend endpoint obtained previously.</span></span> <span data-ttu-id="c3f78-152">Till exempel `http://mybackend.azurewebsites.net`.</span><span class="sxs-lookup"><span data-stu-id="c3f78-152">For example, `http://mybackend.azurewebsites.net`.</span></span>
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. <span data-ttu-id="c3f78-153">Lägg till koden nedan i klassen MainPage i **MainPage.xaml.cs** för den **(Windows 8.1)** och **(Windows Phone 8.1)** projekt.</span><span class="sxs-lookup"><span data-stu-id="c3f78-153">Add the code below to the MainPage class in **MainPage.xaml.cs** for the **(Windows 8.1)** and **(Windows Phone 8.1)** projects.</span></span>
    
    <span data-ttu-id="c3f78-154">Den `PushClick` metod är Klicka hanterare för den **skicka Push-** knappen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-154">The `PushClick` method is the click handler for the **Send Push** button.</span></span> <span data-ttu-id="c3f78-155">Anropar serverdelen för att utlösa en avisering till alla enheter med en tagg för användarnamn som matchar den `to_tag` parameter.</span><span class="sxs-lookup"><span data-stu-id="c3f78-155">It calls the backend to trigger a notification to all devices with a username tag that matches the `to_tag` parameter.</span></span> <span data-ttu-id="c3f78-156">Meddelandet skickas som JSON-innehåll i begärandetexten.</span><span class="sxs-lookup"><span data-stu-id="c3f78-156">The notification message is sent as JSON content in the request body.</span></span>
    
    <span data-ttu-id="c3f78-157">Den `LoginAndRegisterClick` metod är Klicka hanterare för den **logga in och registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-157">The `LoginAndRegisterClick` method is the click handler for the **Log in and register** button.</span></span> <span data-ttu-id="c3f78-158">Lagrar grundläggande autentiseringstoken i lokal lagring (Observera att detta motsvarar alla token som använder din autentiseringsschema), använder sedan `RegisterClient` att registrera för meddelanden med serverdelen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-158">It stores the basic authentication token in local storage (note that this represents any token your authentication scheme uses), then uses `RegisterClient` to register for notifications using the backend.</span></span>

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
                    MessageDialog alert = new MessageDialog(ex.Message, "Failed to send " + pns + " message");
                    alert.ShowAsync();
                }
            }
        }

        private async void LoginAndRegisterClick(object sender, RoutedEventArgs e)
        {
            SetAuthenticationTokenInLocalStorage();

            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();

            // The "username:<user name>" tag gets automatically added by the message handler in the backend.
            // The tag passed here can be whatever other tags you may want to use.
            try
            {
                // The device handle used will be different depending on the device and PNS. 
                // Windows devices use the channel uri as the PNS handle.
                await new RegisterClient(BACKEND_ENDPOINT).RegisterAsync(channel.Uri, new string[] { "myTag" });

                var dialog = new MessageDialog("Registered as: " + UsernameTextBox.Text);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
                SendPushButton.IsEnabled = true;
            }
            catch (Exception ex)
            {
                MessageDialog alert = new MessageDialog(ex.Message, "Failed to register with RegisterClient");
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



1. <span data-ttu-id="c3f78-159">I Solution Explorer under den **delade** projektet öppnar den **App.xaml.cs** fil.</span><span class="sxs-lookup"><span data-stu-id="c3f78-159">In Solution Explorer, under the **Shared** project, open the **App.xaml.cs** file.</span></span> <span data-ttu-id="c3f78-160">Hitta anropet till `InitNotificationsAsync()` i den `OnLaunched()` händelsehanterare.</span><span class="sxs-lookup"><span data-stu-id="c3f78-160">Find the call to `InitNotificationsAsync()` in the `OnLaunched()` event handler.</span></span> <span data-ttu-id="c3f78-161">Kommentera ut eller ta bort anropet till `InitNotificationsAsync()`.</span><span class="sxs-lookup"><span data-stu-id="c3f78-161">Comment out or delete the call to `InitNotificationsAsync()`.</span></span> <span data-ttu-id="c3f78-162">Knappen hanteraren lades till ovan ska initiera notification registreringar.</span><span class="sxs-lookup"><span data-stu-id="c3f78-162">The button handler added above will initialize notification registrations.</span></span>

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. <span data-ttu-id="c3f78-163">I Solution Explorer högerklickar du på den **delade** projektet och klicka sedan på **Lägg till**, och klicka sedan på **klassen**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-163">In Solution Explorer, right-click the **Shared** project, then click **Add**, and then click **Class**.</span></span> <span data-ttu-id="c3f78-164">Klassen namnet **RegisterClient.cs**, klicka på **OK** att skapa klassen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-164">Name the class **RegisterClient.cs**, then click **OK** to generate the class.</span></span>
   
   <span data-ttu-id="c3f78-165">Den här klassen radbryts REST-anrop som krävs för att kontakta appens serverdel för att kunna registrera för push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c3f78-165">This class will wrap the REST calls required to contact the app backend, in order to register for push notifications.</span></span> <span data-ttu-id="c3f78-166">Den lagrar även lokalt på *registrationIds* skapas av Meddelandehubben som anges i [registrering från din Apps serverdel](http://msdn.microsoft.com/library/dn743807.aspx).</span><span class="sxs-lookup"><span data-stu-id="c3f78-166">It also locally stores the *registrationIds* created by the Notification Hub as detailed in [Registering from your app backend](http://msdn.microsoft.com/library/dn743807.aspx).</span></span> <span data-ttu-id="c3f78-167">Observera att den använder en autentiseringstoken som lagras i lokal lagring när du klickar på den **logga in och registrera** knappen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-167">Note that it uses an authorization token stored in local storage when you click the **Log in and register** button.</span></span>
2. <span data-ttu-id="c3f78-168">Lägg till följande `using` instruktioner överst i filen RegisterClient.cs:</span><span class="sxs-lookup"><span data-stu-id="c3f78-168">Add the following `using` statements at the top of the RegisterClient.cs file:</span></span>
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. <span data-ttu-id="c3f78-169">Lägg till följande kod i `RegisterClient`-klassdefinitionen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-169">Add the following code inside the `RegisterClient` class definition.</span></span>
   
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
4. <span data-ttu-id="c3f78-170">Spara alla ändringar.</span><span class="sxs-lookup"><span data-stu-id="c3f78-170">Save all your changes.</span></span>

## <a name="testing-the-application"></a><span data-ttu-id="c3f78-171">Testa programmet</span><span class="sxs-lookup"><span data-stu-id="c3f78-171">Testing the Application</span></span>
1. <span data-ttu-id="c3f78-172">Starta programmet på både Windows 8.1 och Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="c3f78-172">Launch the application on both Windows 8.1 and Windows Phone 8.1.</span></span> <span data-ttu-id="c3f78-173">Du kan köra instansen i emulatorn eller en enhet för Windows Phone 8.1.</span><span class="sxs-lookup"><span data-stu-id="c3f78-173">For Windows Phone 8.1 you can run the instance in the emulator or an actual device.</span></span>
2. <span data-ttu-id="c3f78-174">I Windows 8.1-instans av appen anger du en **användarnamn** och **lösenord** som visas på skärmen nedan.</span><span class="sxs-lookup"><span data-stu-id="c3f78-174">In the Windows 8.1 instance of the app, enter a **Username** and **Password** as shown in the screen below.</span></span> <span data-ttu-id="c3f78-175">Det bör skiljer sig från det användarnamn och lösenord som du anger på Windows Phone.</span><span class="sxs-lookup"><span data-stu-id="c3f78-175">It should differ from the user name and password you enter on Windows Phone.</span></span>
3. <span data-ttu-id="c3f78-176">Klicka på **logga in och registrera** och verifiera en dialogruta visar att du har loggat in.</span><span class="sxs-lookup"><span data-stu-id="c3f78-176">Click **Log in and register** and verify a dialog shows that you have logged in.</span></span> <span data-ttu-id="c3f78-177">Detta kommer också att aktivera den **skicka Push-** knappen.</span><span class="sxs-lookup"><span data-stu-id="c3f78-177">This will also enable the **Send Push** button.</span></span>
   
    ![][14]
4. <span data-ttu-id="c3f78-178">Ange sträng en användare på Windows Phone 8.1-instans i både den **användarnamn** och **lösenord** fält klicka **logga in och registrera**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-178">On the Windows Phone 8.1 instance, enter a user name string in both the **Username** and **Password** fields then click **Login and register**.</span></span>
5. <span data-ttu-id="c3f78-179">I den **mottagaren användarnamn taggen** anger det användarnamn som är registrerad på Windows 8.1.</span><span class="sxs-lookup"><span data-stu-id="c3f78-179">Then in the **Recipient Username Tag** field, enter the user name registered on Windows 8.1.</span></span> <span data-ttu-id="c3f78-180">Ange ett meddelande och klicka på **skicka Push-**.</span><span class="sxs-lookup"><span data-stu-id="c3f78-180">Enter a notification message and click **Send Push**.</span></span>
   
    ![][16]
6. <span data-ttu-id="c3f78-181">Endast de enheter som har registrerats med taggen matchande användarnamn får meddelandet.</span><span class="sxs-lookup"><span data-stu-id="c3f78-181">Only the devices that have registered with the matching username tag receive the notification message.</span></span>
   
    ![][15]

## <a name="next-steps"></a><span data-ttu-id="c3f78-182">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3f78-182">Next Steps</span></span>
* <span data-ttu-id="c3f78-183">Om du vill dela in användarna efter intressegrupper, kan du gå till [Använda Notification Hubs för att skicka de senaste nyheterna].</span><span class="sxs-lookup"><span data-stu-id="c3f78-183">If you want to segment your users by interest groups, see [Use Notification Hubs to send breaking news].</span></span>
* <span data-ttu-id="c3f78-184">Mer information om hur du använder Notification Hubs finns [riktlinjer om Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="c3f78-184">To learn more about how to use Notification Hubs, see [Notification Hubs Guidance].</span></span>

[9]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push9.png
[10]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push10.png
[11]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push11.png
[12]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-secure-push12.png
[13]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-ui.png
[14]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-windows-instance-username.png
[15]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-notification-received.png
[16]: ./media/notification-hubs-aspnet-backend-windows-dotnet-notify-users/notification-hubs-wp-send-message.png



<!-- URLs. -->
<span data-ttu-id="c3f78-185">[Kom igång med Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md</span><span class="sxs-lookup"><span data-stu-id="c3f78-185">[Get started with Notification Hubs]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md</span></span>
<span data-ttu-id="c3f78-186">[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md</span><span class="sxs-lookup"><span data-stu-id="c3f78-186">[Secure Push]: notification-hubs-aspnet-backend-windows-dotnet-wns-secure-push-notification.md</span></span>
<span data-ttu-id="c3f78-187">[Använda Notification Hubs för att skicka de senaste nyheterna]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span><span class="sxs-lookup"><span data-stu-id="c3f78-187">[Use Notification Hubs to send breaking news]: notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md</span></span>
<span data-ttu-id="c3f78-188">[riktlinjer om Notification Hubs]: http://msdn.microsoft.com/library/jj927170.aspx</span><span class="sxs-lookup"><span data-stu-id="c3f78-188">[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx</span></span>
