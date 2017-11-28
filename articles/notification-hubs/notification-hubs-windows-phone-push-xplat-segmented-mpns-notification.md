---
title: "Använda Notification Hubs för att skicka de senaste nyheterna (Windows Phone)"
description: "Använd Azure Notification Hubs för att använda taggen i registreringar för att skicka de senaste nyheterna till en Windows Phone-app."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 42726bf5-cc82-438d-9eaa-238da3322d80
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows-phone
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: 3a6a69bf555c7267d3fbeb03ff6c03054991960f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-breaking-news"></a><span data-ttu-id="6cfd9-103">Använda Notification Hubs för att skicka de senaste nyheterna</span><span class="sxs-lookup"><span data-stu-id="6cfd9-103">Use Notification Hubs to send breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="6cfd9-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="6cfd9-104">Overview</span></span>
<span data-ttu-id="6cfd9-105">Det här avsnittet visar hur du använder Azure Notification Hubs för att sända senaste nyheterna meddelanden till en Windows Phone 8.0/8.1 Silverlight-app.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-105">This topic shows you how to use Azure Notification Hubs to broadcast breaking news notifications to a Windows Phone 8.0/8.1 Silverlight app.</span></span> <span data-ttu-id="6cfd9-106">Om du utvecklar för Windows Store eller Windows Phone 8.1-app, se till att den [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-106">If you are targeting Windows Store or Windows Phone 8.1 app, please refer to to the [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version.</span></span> <span data-ttu-id="6cfd9-107">När du är klar kommer du att kunna registrera för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-107">When complete, you will be able to register for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="6cfd9-108">Det här scenariot är ett vanligt mönster för många appar där meddelanden har skickas till grupper av användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-108">This scenario is a common pattern for many apps where notifications have to be sent to groups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="6cfd9-109">Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in the notification hub.</span></span> <span data-ttu-id="6cfd9-110">När meddelanden skickas till en tagg för tar alla enheter som har registrerats för taggen emot meddelandet.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-110">When notifications are sent to a tag, all devices that have registered for the tag will receive the notification.</span></span> <span data-ttu-id="6cfd9-111">Eftersom taggar är bara strängar, behöver de inte etableras i förväg.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-111">Because tags are simply strings, they do not have to be provisioned in advance.</span></span> <span data-ttu-id="6cfd9-112">Mer information om taggar finns i [Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="6cfd9-112">For more information about tags, refer to [Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6cfd9-113">Krav</span><span class="sxs-lookup"><span data-stu-id="6cfd9-113">Prerequisites</span></span>
<span data-ttu-id="6cfd9-114">Det här avsnittet bygger på appen som du skapade i [Kom igång med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="6cfd9-114">This topic builds on the app you created in [Get started with Notification Hubs].</span></span> <span data-ttu-id="6cfd9-115">Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="6cfd9-115">Before starting this tutorial, you must have already completed [Get started with Notification Hubs].</span></span>

## <a name="add-category-selection-to-the-app"></a><span data-ttu-id="6cfd9-116">Lägg till kategori markeringen i appen</span><span class="sxs-lookup"><span data-stu-id="6cfd9-116">Add category selection to the app</span></span>
<span data-ttu-id="6cfd9-117">Det första steget är att lägga till de UI-element i din befintliga huvudsidan som gör att användaren kan välja kategorier för att registrera.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-117">The first step is to add the UI elements to your existing main page that enable the user to select categories to register.</span></span> <span data-ttu-id="6cfd9-118">Vilka kategorier av en användare som lagras på enheten.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-118">The categories selected by a user are stored on the device.</span></span> <span data-ttu-id="6cfd9-119">När appen startar skapas en enhetsregistrering i din meddelandehubb med de valda kategorierna som taggar.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-119">When the app starts, a device registration is created in your notification hub with the selected categories as tags.</span></span>

1. <span data-ttu-id="6cfd9-120">Öppna projektfilen MainPage.xaml och Ersätt den **rutnätet** element med namnet `TitlePanel` och `ContentPanel` med följande kod:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-120">Open the MainPage.xaml project file, then replace the **Grid** elements named `TitlePanel` and `ContentPanel` with the following code:</span></span>
   
        <StackPanel x:Name="TitlePanel" Grid.Row="0" Margin="12,17,0,28">
            <TextBlock Text="Breaking News" Style="{StaticResource PhoneTextNormalStyle}" Margin="12,0"/>
            <TextBlock Text="Categories" Margin="9,-7,0,0" Style="{StaticResource PhoneTextTitle1Style}"/>
        </StackPanel>
   
        <Grid Name="ContentPanel" Grid.Row="1" Margin="12,0,12,0">
            <Grid.RowDefinitions>
                <RowDefinition Height="auto"/>
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
                <RowDefinition Height="auto" />
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition />
                <ColumnDefinition />
            </Grid.ColumnDefinitions>
            <CheckBox Name="WorldCheckBox" Grid.Row="0" Grid.Column="0">World</CheckBox>
            <CheckBox Name="PoliticsCheckBox" Grid.Row="1" Grid.Column="0">Politics</CheckBox>
            <CheckBox Name="BusinessCheckBox" Grid.Row="2" Grid.Column="0">Business</CheckBox>
            <CheckBox Name="TechnologyCheckBox" Grid.Row="0" Grid.Column="1">Technology</CheckBox>
            <CheckBox Name="ScienceCheckBox" Grid.Row="1" Grid.Column="1">Science</CheckBox>
            <CheckBox Name="SportsCheckBox" Grid.Row="2" Grid.Column="1">Sports</CheckBox>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="3" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
        </Grid>
2. <span data-ttu-id="6cfd9-121">I projektet, skapar du en ny klass med namnet **meddelanden**, lägga till den **offentliga** modifierare till klassdefinitionen, och Lägg sedan till följande **med** instruktioner till den nya kodfilen :</span><span class="sxs-lookup"><span data-stu-id="6cfd9-121">In the project, create a new class named **Notifications**, add the **public** modifier to the class definition, then add the following **using** statements to the new code file:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. <span data-ttu-id="6cfd9-122">Kopiera följande kod till den nya **meddelanden** klass:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-122">Copy the following code into the new **Notifications** class:</span></span>
   
        private NotificationHub hub;
   
        // Registration task to complete registration in the ChannelUriUpdated event handler
        private TaskCompletionSource<Registration> registrationTask;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string)IsolatedStorageSettings.ApplicationSettings["categories"];
            return categories != null ? categories.Split(',') : new string[0];
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            var categoriesAsString = string.Join(",", categories);
            var settings = IsolatedStorageSettings.ApplicationSettings;
            if (!settings.Contains("categories"))
            {
                settings.Add("categories", categoriesAsString);
            }
            else
            {
                settings["categories"] = categoriesAsString;
            }
            settings.Save();
   
            return await SubscribeToCategories();
        }
   
        public async Task<Registration> SubscribeToCategories()
        {
            registrationTask = new TaskCompletionSource<Registration>();
   
            var channel = HttpNotificationChannel.Find("MyPushChannel");
   
            if (channel == null)
            {
                channel = new HttpNotificationChannel("MyPushChannel");
                channel.Open();
                channel.BindToShellToast();
                channel.ChannelUriUpdated += channel_ChannelUriUpdated;
   
                // This is optional, used to receive notifications while the app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete the registrationTask here.  
            // If it is null, the registrationTask will be completed in the ChannelUriUpdated event handler.
            if (channel.ChannelUri != null)
            {
                await RegisterTemplate(channel.ChannelUri);
            }
   
            return await registrationTask.Task;
        }
   
        async void channel_ChannelUriUpdated(object sender, NotificationChannelUriEventArgs e)
        {
            await RegisterTemplate(e.ChannelUri);
        }
   
        async Task<Registration> RegisterTemplate(Uri channelUri)
        {
            // Using a template registration to support notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // The stored categories tags are passed with the template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used to receive notifications while the app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out the information that was part of the message.
            foreach (string key in e.Collection.Keys)
            {
                message.AppendFormat("{0}: {1}\n", key, e.Collection[key]);
   
                if (string.Compare(
                    key,
                    "wp:Param",
                    System.Globalization.CultureInfo.InvariantCulture,
                    System.Globalization.CompareOptions.IgnoreCase) == 0)
                {
                    relativeUri = e.Collection[key];
                }
            }
   
            // Display a dialog of all the fields in the toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    <span data-ttu-id="6cfd9-123">Den här klassen använder isolerad lagring för att lagra kategorier av nyheter som den här enheten ska ta emot.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-123">This class uses the isolated storage to store the categories of news that this device is to receive.</span></span> <span data-ttu-id="6cfd9-124">Den innehåller också metoder för att registrera dig för dessa kategorier med hjälp av en [mallen](notification-hubs-templates-cross-platform-push-messages.md) registreringen av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-124">It also contains methods to register for these categories using a [template](notification-hubs-templates-cross-platform-push-messages.md) notification registration.</span></span>


1. <span data-ttu-id="6cfd9-125">Lägg till följande egenskapen i projektfilen App.xaml.cs den **App** klass.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-125">In the App.xaml.cs project file, add the following property to the **App** class.</span></span> <span data-ttu-id="6cfd9-126">Ersätt den `<hub name>` och `<connection string with listen access>` platshållarna med namnet på din meddelandehubb och anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-126">Replace the `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and the connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > <span data-ttu-id="6cfd9-127">Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast nyckel för lyssna åtkomst med din klientapp.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-127">Because credentials that are distributed with a client app are not generally secure, you should only distribute the key for listen access with your client app.</span></span> <span data-ttu-id="6cfd9-128">Lyssna åtkomst aktiverar inte går att ändra din app att registrera för meddelanden, men befintliga registreringar och går inte att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-128">Listen access enables your app to register for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="6cfd9-129">Fullständig åtkomst-nyckeln används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-129">The full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
2. <span data-ttu-id="6cfd9-130">Lägg till följande rad i din MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-130">In your MainPage.xaml.cs, add the following line:</span></span>
   
        using Windows.UI.Popups;
3. <span data-ttu-id="6cfd9-131">Lägg till följande metod i projektfilen MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-131">In the MainPage.xaml.cs project file, add the following method:</span></span>
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
          var categories = new HashSet<string>();
          if (WorldCheckBox.IsChecked == true) categories.Add("World");
          if (PoliticsCheckBox.IsChecked == true) categories.Add("Politics");
          if (BusinessCheckBox.IsChecked == true) categories.Add("Business");
          if (TechnologyCheckBox.IsChecked == true) categories.Add("Technology");
          if (ScienceCheckBox.IsChecked == true) categories.Add("Science");
          if (SportsCheckBox.IsChecked == true) categories.Add("Sports");
   
          var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
          MessageBox.Show("Subscribed to: " + string.Join(",", categories) + " on registration id : " +
             result.RegistrationId);
        }
   
    <span data-ttu-id="6cfd9-132">Den här metoden skapar en lista över kategorier och använder den **meddelanden** klass som lagrar listan i enhetens lokala lagring och registrera motsvarande taggar med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-132">This method creates a list of categories and uses the **Notifications** class to store the list in the local storage and register the corresponding tags with your notification hub.</span></span> <span data-ttu-id="6cfd9-133">När kategorier ändras, återskapas registreringen med de nya kategorierna.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-133">When categories are changed, the registration is recreated with the new categories.</span></span>

<span data-ttu-id="6cfd9-134">Appen är nu kunna lagra en uppsättning kategorier i lokal lagring på enheten och registrera med notification hub när användaren ändrar valet av kategorier.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-134">Your app is now able to store a set of categories in local storage on the device and register with the notification hub whenever the user changes the selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="6cfd9-135">Registrera dig för meddelanden</span><span class="sxs-lookup"><span data-stu-id="6cfd9-135">Register for notifications</span></span>
<span data-ttu-id="6cfd9-136">De här stegen registrera med notification hub vid start med hjälp av kategorier som har lagrats i lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-136">These steps register with the notification hub on startup using the categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="6cfd9-137">Eftersom URI som tilldelats av Microsoft Push Notification Service (MPNS)-kanalen kan ändras när som helst, bör du registrera dig för meddelanden ofta att undvika fel i meddelande.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-137">Because the channel URI assigned by the Microsoft Push Notification Service (MPNS) can change at any time, you should register for notifications frequently to avoid notification failures.</span></span> <span data-ttu-id="6cfd9-138">Det här exemplet registrerar för meddelanden varje gång appen startar.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-138">This example registers for notifications every time that the app starts.</span></span> <span data-ttu-id="6cfd9-139">För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering för att bevara bandbredd om mindre än en dag har gått sedan den tidigare registreringen.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-139">For apps that are run frequently, more than once a day, you can probably skip registration to preserve bandwidth if less than a day has passed since the previous registration.</span></span>
> 
> 

1. <span data-ttu-id="6cfd9-140">Öppna filen App.xaml.cs och Lägg till den **asynkrona** modifierare till **Application_Launching** metod och Ersätt Notification Hubs registration kod som du lade till i [Kom igång med Notification Hubs] med följande kod:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-140">Open the App.xaml.cs file and add the **async** modifier to **Application_Launching** method and replace the Notification Hubs registration code that you added in [Get started with Notification Hubs] with the following code:</span></span>
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    <span data-ttu-id="6cfd9-141">Detta säkerställer att varje gång appen startas hämtas kategorier från lokal lagring och begär en registrering för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-141">This makes sure that every time the app starts it retrieves the categories from local storage and requests a registration for these categories.</span></span>
2. <span data-ttu-id="6cfd9-142">Lägg till följande kod som implementerar i projektfilen MainPage.xaml.cs den **OnNavigatedTo** metod:</span><span class="sxs-lookup"><span data-stu-id="6cfd9-142">In the MainPage.xaml.cs project file, add the following code that implements the **OnNavigatedTo** method:</span></span>
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldCheckBox.IsChecked = true;
            if (categories.Contains("Politics")) PoliticsCheckBox.IsChecked = true;
            if (categories.Contains("Business")) BusinessCheckBox.IsChecked = true;
            if (categories.Contains("Technology")) TechnologyCheckBox.IsChecked = true;
            if (categories.Contains("Science")) ScienceCheckBox.IsChecked = true;
            if (categories.Contains("Sports")) SportsCheckBox.IsChecked = true;
        }
   
    <span data-ttu-id="6cfd9-143">Detta uppdaterar huvudsidan baserat på status för tidigare sparad kategorier.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-143">This updates the main page based on the status of previously saved categories.</span></span>

<span data-ttu-id="6cfd9-144">Appen är nu klar och kan lagra en uppsättning kategorier i enhetens lokala lagring som används för att registrera med notification hub när användaren ändrar valet av kategorier.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-144">The app is now complete and can store a set of categories in the device local storage used to register with the notification hub whenever the user changes the selection of categories.</span></span> <span data-ttu-id="6cfd9-145">Nu ska definierar vi en serverdel som kategori meddelanden kan skickas till den här appen.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-145">Next, we will define a backend that can send category notifications to this app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="6cfd9-146">Skicka taggade meddelanden</span><span class="sxs-lookup"><span data-stu-id="6cfd9-146">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-the-app-and-generate-notifications"></a><span data-ttu-id="6cfd9-147">Kör appen och generera meddelanden</span><span class="sxs-lookup"><span data-stu-id="6cfd9-147">Run the app and generate notifications</span></span>
1. <span data-ttu-id="6cfd9-148">Tryck på F5 för att kompilera och starta appen i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-148">In Visual Studio, press F5 to compile and start the app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="6cfd9-149">Observera att appen Användargränssnittet innehåller en uppsättning växlar som låter dig välja kategorier för att prenumerera på.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-149">Note that the app UI provides a set of toggles that lets you choose the categories to subscribe to.</span></span>
2. <span data-ttu-id="6cfd9-150">Aktivera en eller flera kategorier växlar och klicka sedan på **prenumerera**.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-150">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="6cfd9-151">Appen konverterar valda kategorier till taggar och begär en ny enhetsregistrering för de valda taggarna från meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-151">The app converts the selected categories into tags and requests a new device registration for the selected tags from the notification hub.</span></span> <span data-ttu-id="6cfd9-152">Registrerade kategorier returneras och visas i en dialogruta.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-152">The registered categories are returned and displayed in a dialog.</span></span>
   
    ![][2]
3. <span data-ttu-id="6cfd9-153">Köra konsolapp-för att skicka meddelanden för varje kategorier efter att ha mottagit en bekräftelse att kategorierna var prenumeration som har slutförts.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-153">After receiving a confirmation that your categories were subscription completed, run the console app to send notifications for each categories.</span></span> <span data-ttu-id="6cfd9-154">Kontrollera att endast ett meddelande i kategorier som du prenumererar på.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-154">Verify you only receive a notification for the categories you have subscribed to.</span></span>
   
    ![][3]

<span data-ttu-id="6cfd9-155">Du har slutfört det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="6cfd9-155">You have completed this topic.</span></span>

<!--##Next steps

In this tutorial we learned how to broadcast breaking news by category. Consider completing one of the following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs to broadcast localized breaking news]

    Learn how to expand the breaking news app to enable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how to push notifications to specific authenticated users. This is a good solution for sending notifications only to specific users.
-->

<!-- Anchors. -->
[Add category selection to the app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run the app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
<span data-ttu-id="6cfd9-156">[Kom igång med Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/</span><span class="sxs-lookup"><span data-stu-id="6cfd9-156">[Get started with Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/</span></span>
[Use Notification Hubs to broadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for Windows Phone]: ??

