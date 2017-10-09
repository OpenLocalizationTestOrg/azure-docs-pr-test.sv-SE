---
title: aaaUse Meddelandehubbar toosend senaste nytt (Windows Phone)
description: "Använd Azure Notification Hubs toouse tagg i registreringar toosend bryter nyheter tooa Windows Phone-app."
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
ms.openlocfilehash: 3519a8701105f88198afe288e59e9204420234db
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="79f6e-103">Använda Notification Hubs toosend senaste nytt</span><span class="sxs-lookup"><span data-stu-id="79f6e-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="79f6e-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="79f6e-104">Overview</span></span>
<span data-ttu-id="79f6e-105">Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast senaste nyheterna meddelanden tooa Windows Phone 8.0/8.1 Silverlight-app.</span><span class="sxs-lookup"><span data-stu-id="79f6e-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooa Windows Phone 8.0/8.1 Silverlight app.</span></span> <span data-ttu-id="79f6e-106">Om du utvecklar för Windows Store eller Windows Phone 8.1-app, se tootoohello [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version.</span><span class="sxs-lookup"><span data-stu-id="79f6e-106">If you are targeting Windows Store or Windows Phone 8.1 app, please refer tootoohello [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version.</span></span> <span data-ttu-id="79f6e-107">När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="79f6e-107">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="79f6e-108">Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.</span><span class="sxs-lookup"><span data-stu-id="79f6e-108">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, etc.</span></span>

<span data-ttu-id="79f6e-109">Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="79f6e-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="79f6e-110">När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="79f6e-110">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="79f6e-111">Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg.</span><span class="sxs-lookup"><span data-stu-id="79f6e-111">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="79f6e-112">Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="79f6e-112">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="79f6e-113">Krav</span><span class="sxs-lookup"><span data-stu-id="79f6e-113">Prerequisites</span></span>
<span data-ttu-id="79f6e-114">Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="79f6e-114">This topic builds on hello app you created in [Get started with Notification Hubs].</span></span> <span data-ttu-id="79f6e-115">Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs].</span><span class="sxs-lookup"><span data-stu-id="79f6e-115">Before starting this tutorial, you must have already completed [Get started with Notification Hubs].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="79f6e-116">Lägg till kategori markeringen toohello app</span><span class="sxs-lookup"><span data-stu-id="79f6e-116">Add category selection toohello app</span></span>
<span data-ttu-id="79f6e-117">hello första steget är tooadd hello UI-element tooyour befintliga huvudsidan som möjliggör hello användaren tooselect kategorier tooregister.</span><span class="sxs-lookup"><span data-stu-id="79f6e-117">hello first step is tooadd hello UI elements tooyour existing main page that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="79f6e-118">hello kategorier som är markerad som en användare som lagras på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="79f6e-118">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="79f6e-119">När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.</span><span class="sxs-lookup"><span data-stu-id="79f6e-119">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="79f6e-120">Öppna projektfilen för hello MainPage.xaml sedan ersätta hello **rutnätet** element med namnet `TitlePanel` och `ContentPanel` med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="79f6e-120">Open hello MainPage.xaml project file, then replace hello **Grid** elements named `TitlePanel` and `ContentPanel` with hello following code:</span></span>
   
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
2. <span data-ttu-id="79f6e-121">I hello-projekt, skapa en ny klass med namnet **meddelanden**, lägga till hello **offentliga** modifieraren toohello klassen definition och sedan lägga till följande hello **med** instruktioner toohello nya kodfil:</span><span class="sxs-lookup"><span data-stu-id="79f6e-121">In hello project, create a new class named **Notifications**, add hello **public** modifier toohello class definition, then add hello following **using** statements toohello new code file:</span></span>
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. <span data-ttu-id="79f6e-122">Kopiera hello följande kod i hello nya **meddelanden** klass:</span><span class="sxs-lookup"><span data-stu-id="79f6e-122">Copy hello following code into hello new **Notifications** class:</span></span>
   
        private NotificationHub hub;
   
        // Registration task toocomplete registration in hello ChannelUriUpdated event handler
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
   
                // This is optional, used tooreceive notifications while hello app is running.
                channel.ShellToastNotificationReceived += channel_ShellToastNotificationReceived;
            }
   
            // If channel.ChannelUri is not null, we will complete hello registrationTask here.  
            // If it is null, hello registrationTask will be completed in hello ChannelUriUpdated event handler.
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
            // Using a template registration toosupport notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyMPNS = "<wp:Notification xmlns:wp=\"WPNotification\">" +
                                                "<wp:Toast>" +
                                                    "<wp:Text1>$(messageParam)</wp:Text1>" +
                                                "</wp:Toast>" +
                                            "</wp:Notification>";
   
            // hello stored categories tags are passed with hello template registration.
   
            registrationTask.SetResult(await hub.RegisterTemplateAsync(channelUri.ToString(), 
                templateBodyMPNS, "simpleMPNSTemplateExample", this.RetrieveCategories()));
   
            return await registrationTask.Task;
        }
   
        // This is optional. It is used tooreceive notifications while hello app is running.
        void channel_ShellToastNotificationReceived(object sender, NotificationEventArgs e)
        {
            StringBuilder message = new StringBuilder();
            string relativeUri = string.Empty;
   
            message.AppendFormat("Received Toast {0}:\n", DateTime.Now.ToShortTimeString());
   
            // Parse out hello information that was part of hello message.
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
   
            // Display a dialog of all hello fields in hello toast.
            System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() => 
            { 
                MessageBox.Show(message.ToString()); 
            });
        }

    <span data-ttu-id="79f6e-123">Den här klassen används hello isolerad lagring toostore hello kategorier av att den här enheten är tooreceive nyheter.</span><span class="sxs-lookup"><span data-stu-id="79f6e-123">This class uses hello isolated storage toostore hello categories of news that this device is tooreceive.</span></span> <span data-ttu-id="79f6e-124">Den innehåller också metoder tooregister för dessa kategorier med hjälp av en [mallen](notification-hubs-templates-cross-platform-push-messages.md) registreringen av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="79f6e-124">It also contains methods tooregister for these categories using a [template](notification-hubs-templates-cross-platform-push-messages.md) notification registration.</span></span>


1. <span data-ttu-id="79f6e-125">Lägg till följande egenskapen toohello hello i hello projektfilen App.XAML.CS, **App** klass.</span><span class="sxs-lookup"><span data-stu-id="79f6e-125">In hello App.xaml.cs project file, add hello following property toohello **App** class.</span></span> <span data-ttu-id="79f6e-126">Ersätt hello `<hub name>` och `<connection string with listen access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="79f6e-126">Replace hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > <span data-ttu-id="79f6e-127">Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp.</span><span class="sxs-lookup"><span data-stu-id="79f6e-127">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="79f6e-128">Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="79f6e-128">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="79f6e-129">hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.</span><span class="sxs-lookup"><span data-stu-id="79f6e-129">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
2. <span data-ttu-id="79f6e-130">Lägg till följande rad hello i din MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="79f6e-130">In your MainPage.xaml.cs, add hello following line:</span></span>
   
        using Windows.UI.Popups;
3. <span data-ttu-id="79f6e-131">Lägg till hello följande metod i projektfilen för hello MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="79f6e-131">In hello MainPage.xaml.cs project file, add hello following method:</span></span>
   
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
   
    <span data-ttu-id="79f6e-132">Den här metoden skapar en lista över kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrera hello motsvarande taggar med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="79f6e-132">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="79f6e-133">När kategorier ändras, återskapas hello registrering med hello nya kategorier.</span><span class="sxs-lookup"><span data-stu-id="79f6e-133">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="79f6e-134">Appen är nu kan toostore en uppsättning kategorier i lokal lagring på hello enheten och registrerar med hello notification hub när hello användarändringar hello val av kategorier.</span><span class="sxs-lookup"><span data-stu-id="79f6e-134">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="79f6e-135">Registrera dig för meddelanden</span><span class="sxs-lookup"><span data-stu-id="79f6e-135">Register for notifications</span></span>
<span data-ttu-id="79f6e-136">De här stegen registrera hello meddelandehubben på Start med hello kategorier som har lagrats i lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="79f6e-136">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="79f6e-137">Eftersom URI som tilldelats av hello Microsoft Push Notification Service (MPNS) hello-kanalen kan ändras när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel.</span><span class="sxs-lookup"><span data-stu-id="79f6e-137">Because hello channel URI assigned by hello Microsoft Push Notification Service (MPNS) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="79f6e-138">Det här exemplet registrerar för meddelanden varje gång hello appen startas.</span><span class="sxs-lookup"><span data-stu-id="79f6e-138">This example registers for notifications every time that hello app starts.</span></span> <span data-ttu-id="79f6e-139">För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.</span><span class="sxs-lookup"><span data-stu-id="79f6e-139">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="79f6e-140">Öppna filen för hello App.xaml.cs och Lägg till hello **asynkrona** modifierare för**Application_Launching** metoden och Ersätt hello Notification Hubs registration kod som du lade till i [Kom igång med Notification Hubs] med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="79f6e-140">Open hello App.xaml.cs file and add hello **async** modifier too**Application_Launching** method and replace hello Notification Hubs registration code that you added in [Get started with Notification Hubs] with hello following code:</span></span>
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    <span data-ttu-id="79f6e-141">Detta säkerställer att varje gång hello appen startar hämtas hello kategorier från lokal lagring och begär en registrering för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="79f6e-141">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registration for these categories.</span></span>
2. <span data-ttu-id="79f6e-142">Lägg till följande kod som implementerar hello hello i hello MainPage.xaml.cs projektfilen, **OnNavigatedTo** metod:</span><span class="sxs-lookup"><span data-stu-id="79f6e-142">In hello MainPage.xaml.cs project file, add hello following code that implements hello **OnNavigatedTo** method:</span></span>
   
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
   
    <span data-ttu-id="79f6e-143">Den här uppdateringar hello huvudsidan baserat på hello status för tidigare sparats kategorier.</span><span class="sxs-lookup"><span data-stu-id="79f6e-143">This updates hello main page based on hello status of previously saved categories.</span></span>

<span data-ttu-id="79f6e-144">hello appen är nu klar och kan lagra en uppsättning kategorier i hello enheten lokal lagring används tooregister hello meddelandehubben varje gång hello användarändringar hello val av kategorier.</span><span class="sxs-lookup"><span data-stu-id="79f6e-144">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="79f6e-145">Nu ska definierar vi en serverdel som kan skicka kategori meddelanden toothis app.</span><span class="sxs-lookup"><span data-stu-id="79f6e-145">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="79f6e-146">Skicka taggade meddelanden</span><span class="sxs-lookup"><span data-stu-id="79f6e-146">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="79f6e-147">Kör hello app och generera meddelanden</span><span class="sxs-lookup"><span data-stu-id="79f6e-147">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="79f6e-148">Tryck på F5 toocompile och starta hello app i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="79f6e-148">In Visual Studio, press F5 toocompile and start hello app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="79f6e-149">Obs hello appen Användargränssnittet innehåller en uppsättning växlar som kan du välja hello kategorier toosubscribe till.</span><span class="sxs-lookup"><span data-stu-id="79f6e-149">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="79f6e-150">Aktivera en eller flera kategorier växlar och klicka sedan på **prenumerera**.</span><span class="sxs-lookup"><span data-stu-id="79f6e-150">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="79f6e-151">hello appen konverterar hello valda kategorier till taggar och begär en ny registrering av enheten för hello valt taggar från hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="79f6e-151">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="79f6e-152">hello registrerade kategorier returneras och visas i en dialogruta.</span><span class="sxs-lookup"><span data-stu-id="79f6e-152">hello registered categories are returned and displayed in a dialog.</span></span>
   
    ![][2]
3. <span data-ttu-id="79f6e-153">När du har fått en bekräftelse att kategorierna var prenumeration slutförts kör hello konsolen app toosend meddelanden för varje kategorier.</span><span class="sxs-lookup"><span data-stu-id="79f6e-153">After receiving a confirmation that your categories were subscription completed, run hello console app toosend notifications for each categories.</span></span> <span data-ttu-id="79f6e-154">Kontrollera att endast ett meddelande för hello kategorier som du prenumererar på.</span><span class="sxs-lookup"><span data-stu-id="79f6e-154">Verify you only receive a notification for hello categories you have subscribed to.</span></span>
   
    ![][3]

<span data-ttu-id="79f6e-155">Du har slutfört det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="79f6e-155">You have completed this topic.</span></span>

<!--##Next steps

In this tutorial we learned how toobroadcast breaking news by category. Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:

+ [Use Notification Hubs toobroadcast localized breaking news]

    Learn how tooexpand hello breaking news app tooenable sending localized notifications.

+ [Notify users with Notification Hubs]

    Learn how toopush notifications toospecific authenticated users. This is a good solution for sending notifications only toospecific users.
-->

<!-- Anchors. -->
[Add category selection toohello app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run hello app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-breakingnews.png
[2]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-registration.png
[3]: ./media/notification-hubs-windows-phone-send-breaking-news/notification-hub-toast.png



<!-- URLs.-->
[Kom igång med Notification Hubs]: /manage/services/notification-hubs/get-started-notification-hubs-wp8/
[Use Notification Hubs toobroadcast localized breaking news]: ../breakingnews-localized-wp8.md
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users/
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Phone]: ??

