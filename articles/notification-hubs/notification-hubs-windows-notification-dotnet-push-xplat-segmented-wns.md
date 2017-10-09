---
title: aaaUse Meddelandehubbar toosend senaste nytt (Windows Universal)
description: "Använd Azure Notification Hubs med taggar i hello registrering toosend bryter nyheter tooa universell Windows-app."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: 994d2eed-f62e-433c-bf65-4afebf1c0561
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: f102d286d2c7bd387fcfa2c7eab2ba31a0298517
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-breaking-news"></a><span data-ttu-id="dcc56-103">Använda Notification Hubs toosend senaste nytt</span><span class="sxs-lookup"><span data-stu-id="dcc56-103">Use Notification Hubs toosend breaking news</span></span>
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a><span data-ttu-id="dcc56-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="dcc56-104">Overview</span></span>
<span data-ttu-id="dcc56-105">Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast bryter nyheter meddelanden tooa Windows Store eller Windows Phone 8.1 (utan Silverlight)-app.</span><span class="sxs-lookup"><span data-stu-id="dcc56-105">This topic shows you how toouse Azure Notification Hubs toobroadcast breaking news notifications tooa Windows Store or Windows Phone 8.1 (non-Silverlight) app.</span></span> <span data-ttu-id="dcc56-106">Om du utvecklar för Windows Phone 8.1 Silverlight, se toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) version.</span><span class="sxs-lookup"><span data-stu-id="dcc56-106">If you are targeting Windows Phone 8.1 Silverlight, please refer toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) version.</span></span> <span data-ttu-id="dcc56-107">När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="dcc56-107">When complete, you will be able tooregister for breaking news categories you are interested in, and receive only push notifications for those categories.</span></span> <span data-ttu-id="dcc56-108">Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, och så vidare.</span><span class="sxs-lookup"><span data-stu-id="dcc56-108">This scenario is a common pattern for many apps where notifications have toobe sent toogroups of users that have previously declared interest in them, e.g. RSS reader, apps for music fans, and so on.</span></span> 

<span data-ttu-id="dcc56-109">Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="dcc56-109">Broadcast scenarios are enabled by including one or more *tags* when creating a registration in hello notification hub.</span></span> <span data-ttu-id="dcc56-110">När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="dcc56-110">When notifications are sent tooa tag, all devices that have registered for hello tag will receive hello notification.</span></span> <span data-ttu-id="dcc56-111">Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg.</span><span class="sxs-lookup"><span data-stu-id="dcc56-111">Because tags are simply strings, they do not have toobe provisioned in advance.</span></span> <span data-ttu-id="dcc56-112">Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).</span><span class="sxs-lookup"><span data-stu-id="dcc56-112">For more information about tags, refer too[Notification Hubs Routing and Tag Expressions](notification-hubs-tags-segment-push-message.md).</span></span>

> [!NOTE]
> <span data-ttu-id="dcc56-113">Windows Store och Windows Phone-projekt version 8.1 och tidigare stöds inte i Visual Studio-2017.</span><span class="sxs-lookup"><span data-stu-id="dcc56-113">Windows Store and Windows Phone projects version 8.1 and earlier are not supported in Visual Studio 2017.</span></span>  <span data-ttu-id="dcc56-114">Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet).</span><span class="sxs-lookup"><span data-stu-id="dcc56-114">For more information, see [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs).</span></span> 

## <a name="prerequisites"></a><span data-ttu-id="dcc56-115">Krav</span><span class="sxs-lookup"><span data-stu-id="dcc56-115">Prerequisites</span></span>
<span data-ttu-id="dcc56-116">Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="dcc56-116">This topic builds on hello app you created in [Get started with Notification Hubs][get-started].</span></span> <span data-ttu-id="dcc56-117">Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].</span><span class="sxs-lookup"><span data-stu-id="dcc56-117">Before starting this tutorial, you must have already completed [Get started with Notification Hubs][get-started].</span></span>

## <a name="add-category-selection-toohello-app"></a><span data-ttu-id="dcc56-118">Lägg till kategori markeringen toohello app</span><span class="sxs-lookup"><span data-stu-id="dcc56-118">Add category selection toohello app</span></span>
<span data-ttu-id="dcc56-119">hello första steget är tooadd hello UI-element tooyour befintliga huvudsidan som möjliggör hello användaren tooselect kategorier tooregister.</span><span class="sxs-lookup"><span data-stu-id="dcc56-119">hello first step is tooadd hello UI elements tooyour existing main page that enable hello user tooselect categories tooregister.</span></span> <span data-ttu-id="dcc56-120">hello kategorier som är markerad som en användare som lagras på hello enhet.</span><span class="sxs-lookup"><span data-stu-id="dcc56-120">hello categories selected by a user are stored on hello device.</span></span> <span data-ttu-id="dcc56-121">När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.</span><span class="sxs-lookup"><span data-stu-id="dcc56-121">When hello app starts, a device registration is created in your notification hub with hello selected categories as tags.</span></span>

1. <span data-ttu-id="dcc56-122">Öppna projektfilen för hello MainPage.xaml och sedan kopiera hello följande kod i hello **rutnätet** element:</span><span class="sxs-lookup"><span data-stu-id="dcc56-122">Open hello MainPage.xaml project file, then copy hello following code in hello **Grid** element:</span></span>
   
        <Grid>
            <Grid.RowDefinitions>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
                <RowDefinition/>
            </Grid.RowDefinitions>
            <Grid.ColumnDefinitions>
                <ColumnDefinition/>
                <ColumnDefinition/>
            </Grid.ColumnDefinitions>
            <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="1" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="2" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="3" Grid.Column="0" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="1" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="2" Grid.Column="1" HorizontalAlignment="Center"/>
            <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="3" Grid.Column="1" HorizontalAlignment="Center"/>
            <Button Name="SubscribeButton" Content="Subscribe" HorizontalAlignment="Center" Grid.Row="4" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click"/>
        </Grid>
2. <span data-ttu-id="dcc56-123">Högerklicka på hello **delade** projekt och Lägg till en ny klass med namnet **meddelanden**, lägga till hello **offentliga** modifieraren toohello klassen definition och sedan lägga till följande hello **med** instruktioner toohello nya kodfil:</span><span class="sxs-lookup"><span data-stu-id="dcc56-123">Right click hello **Shared** project and add a new class named **Notifications**, add hello **public** modifier toohello class definition, then add hello following **using** statements toohello new code file:</span></span>
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. <span data-ttu-id="dcc56-124">Kopiera hello följande kod i hello nya **meddelanden** klass:</span><span class="sxs-lookup"><span data-stu-id="dcc56-124">Copy hello following code into hello new **Notifications** class:</span></span>
   
        private NotificationHub hub;
   
        public Notifications(string hubName, string listenConnectionString)
        {
            hub = new NotificationHub(hubName, listenConnectionString);
        }
   
        public async Task<Registration> StoreCategoriesAndSubscribe(IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            return await SubscribeToCategories(categories);
        }
   
        public IEnumerable<string> RetrieveCategories()
        {
            var categories = (string) ApplicationData.Current.LocalSettings.Values["categories"];
            return categories != null ? categories.Split(','): new string[0];
        }
   
        public async Task<Registration> SubscribeToCategories(IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration toosupport notifications across platforms.
            // Any template notifications that contain messageParam and a corresponding tag expression
            // will be delivered for this registration.
   
            const string templateBodyWNS = "<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(messageParam)</text></binding></visual></toast>";
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "simpleWNSTemplateExample",
                    categories);
        }
   
    <span data-ttu-id="dcc56-125">Den här klassen används hello lokal lagring toostore hello kategorier av nyheter som att den här enheten har tooreceive.</span><span class="sxs-lookup"><span data-stu-id="dcc56-125">This class uses hello local storage toostore hello categories of news that this device has tooreceive.</span></span> <span data-ttu-id="dcc56-126">Observera att i stället för att anropa hello *RegisterNativeAsync* metoden som vi kallar *RegisterTemplateAsync* tooregister för hello kategorier med hjälp av en mall för registrering.</span><span class="sxs-lookup"><span data-stu-id="dcc56-126">Note that instead of calling hello *RegisterNativeAsync* method we call *RegisterTemplateAsync* tooregister for hello categories using a template registration.</span></span> 
   
    <span data-ttu-id="dcc56-127">Vi också ange ett namn för mallen hello (”simpleWNSTemplateExample”), eftersom det kan vara bra tooregister mer än en mall (till exempel en för popup-meddelanden) och en för paneler och vi behöver tooname dem i ordning toobe kan tooupdate eller tar bort dem.</span><span class="sxs-lookup"><span data-stu-id="dcc56-127">We also provide a name for hello template ("simpleWNSTemplateExample"), because we might want tooregister more than one template (for instance one for toast notifications and one for tiles) and we need tooname them in order toobe able tooupdate or delete them.</span></span>
   
    <span data-ttu-id="dcc56-128">Observera att om en enhet registrerar flera mallar med hello samma tagg, ett inkommande meddelande som mål att taggen resulterar i flera meddelanden levereras toohello enhet (en för varje mall).</span><span class="sxs-lookup"><span data-stu-id="dcc56-128">Note that if a device registers multiple templates with hello same tag, an incoming message targeting that tag will result in multiple notifications delivered toohello device (one for each template).</span></span> <span data-ttu-id="dcc56-129">Detta är användbart när hello samma logiska meddelande har tooresult i flera visuella meddelanden, till exempel visar både en Aktivitetsikon och ett popup-meddelande i en Windows Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="dcc56-129">This behavior is useful when hello same logical message has tooresult in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
   
    <span data-ttu-id="dcc56-130">Mer information om mallar finns [mallar](notification-hubs-templates-cross-platform-push-messages.md).</span><span class="sxs-lookup"><span data-stu-id="dcc56-130">For more information on templates, see [Templates](notification-hubs-templates-cross-platform-push-messages.md).</span></span>
4. <span data-ttu-id="dcc56-131">Lägg till följande egenskapen toohello hello i hello projektfilen App.XAML.CS, **App** klass:</span><span class="sxs-lookup"><span data-stu-id="dcc56-131">In hello App.xaml.cs project file, add hello following property toohello **App** class:</span></span>
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    <span data-ttu-id="dcc56-132">Den här egenskapen är används toocreate och åtkomst till en **meddelanden** instans.</span><span class="sxs-lookup"><span data-stu-id="dcc56-132">This property is used toocreate and access a **Notifications** instance.</span></span>
   
    <span data-ttu-id="dcc56-133">Ersätt hello i hello ovan kod `<hub name>` och `<connection string with listen access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.</span><span class="sxs-lookup"><span data-stu-id="dcc56-133">In hello above code, replace hello `<hub name>` and `<connection string with listen access>` placeholders with your notification hub name and hello connection string for *DefaultListenSharedAccessSignature* that you obtained earlier.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="dcc56-134">Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp.</span><span class="sxs-lookup"><span data-stu-id="dcc56-134">Because credentials that are distributed with a client app are not generally secure, you should only distribute hello key for listen access with your client app.</span></span> <span data-ttu-id="dcc56-135">Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dcc56-135">Listen access enables your app tooregister for notifications, but existing registrations cannot be modified and notifications cannot be sent.</span></span> <span data-ttu-id="dcc56-136">hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.</span><span class="sxs-lookup"><span data-stu-id="dcc56-136">hello full access key is used in a secured backend service for sending notifications and changing existing registrations.</span></span>
   > 
   > 
5. <span data-ttu-id="dcc56-137">Lägg till följande rad hello i din MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="dcc56-137">In your MainPage.xaml.cs, add hello following line:</span></span>
   
        using Windows.UI.Popups;
6. <span data-ttu-id="dcc56-138">Lägg till hello följande metod i projektfilen för hello MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="dcc56-138">In hello MainPage.xaml.cs project file, add hello following method:</span></span>
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(categories);
   
            var dialog = new MessageDialog("Subscribed to: " + string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
   
    <span data-ttu-id="dcc56-139">Den här metoden skapar en lista över kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrera hello motsvarande taggar med meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="dcc56-139">This method creates a list of categories and uses hello **Notifications** class toostore hello list in hello local storage and register hello corresponding tags with your notification hub.</span></span> <span data-ttu-id="dcc56-140">När kategorier ändras, återskapas hello registrering med hello nya kategorier.</span><span class="sxs-lookup"><span data-stu-id="dcc56-140">When categories are changed, hello registration is recreated with hello new categories.</span></span>

<span data-ttu-id="dcc56-141">Appen är nu kan toostore en uppsättning kategorier i lokal lagring på hello enheten och registrerar med hello notification hub när hello användarändringar hello val av kategorier.</span><span class="sxs-lookup"><span data-stu-id="dcc56-141">Your app is now able toostore a set of categories in local storage on hello device and register with hello notification hub whenever hello user changes hello selection of categories.</span></span>

## <a name="register-for-notifications"></a><span data-ttu-id="dcc56-142">Registrera dig för meddelanden</span><span class="sxs-lookup"><span data-stu-id="dcc56-142">Register for notifications</span></span>
<span data-ttu-id="dcc56-143">De här stegen registrera hello meddelandehubben på Start med hello kategorier som har lagrats i lokal lagring.</span><span class="sxs-lookup"><span data-stu-id="dcc56-143">These steps register with hello notification hub on startup using hello categories that have been stored in local storage.</span></span>

> [!NOTE]
> <span data-ttu-id="dcc56-144">Eftersom URI som tilldelats av hello Windows Notification Service (WNS) hello-kanalen kan ändras när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel.</span><span class="sxs-lookup"><span data-stu-id="dcc56-144">Because hello channel URI assigned by hello Windows Notification Service (WNS) can change at any time, you should register for notifications frequently tooavoid notification failures.</span></span> <span data-ttu-id="dcc56-145">Det här exemplet registrerar för meddelande varje gång hello appen startas.</span><span class="sxs-lookup"><span data-stu-id="dcc56-145">This example registers for notification every time that hello app starts.</span></span> <span data-ttu-id="dcc56-146">För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.</span><span class="sxs-lookup"><span data-stu-id="dcc56-146">For apps that are run frequently, more than once a day, you can probably skip registration toopreserve bandwidth if less than a day has passed since hello previous registration.</span></span>
> 
> 

1. <span data-ttu-id="dcc56-147">Öppna hello App.xaml.cs fil- och update hello **InitNotificationsAsync** metoden toouse hello `notifications` klassen toosubscribe baserat på kategorier.</span><span class="sxs-lookup"><span data-stu-id="dcc56-147">Open hello App.xaml.cs file and update hello **InitNotificationsAsync** method toouse hello `notifications` class toosubscribe based on categories.</span></span>
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    <span data-ttu-id="dcc56-148">Detta säkerställer att varje gång hello appen startar hämtas hello kategorier från lokal lagring och begär en registrering för dessa kategorier.</span><span class="sxs-lookup"><span data-stu-id="dcc56-148">This makes sure that every time hello app starts it retrieves hello categories from local storage and requests a registeration for these categories.</span></span> <span data-ttu-id="dcc56-149">Hej **InitNotificationsAsync** metod skapades som en del av hello [Kom igång med Notification Hubs] [ get-started] kursen.</span><span class="sxs-lookup"><span data-stu-id="dcc56-149">hello **InitNotificationsAsync** method was created as part of hello [Get started with Notification Hubs][get-started] tutorial.</span></span>
2. <span data-ttu-id="dcc56-150">Lägg till följande kod toohello hello i hello MainPage.xaml.cs projektfilen, *OnNavigatedTo* metod:</span><span class="sxs-lookup"><span data-stu-id="dcc56-150">In hello MainPage.xaml.cs project file, add hello following code toohello *OnNavigatedTo* method:</span></span>
   
        protected override void OnNavigatedTo(NavigationEventArgs e)
        {
            var categories = ((App)Application.Current).notifications.RetrieveCategories();
   
            if (categories.Contains("World")) WorldToggle.IsOn = true;
            if (categories.Contains("Politics")) PoliticsToggle.IsOn = true;
            if (categories.Contains("Business")) BusinessToggle.IsOn = true;
            if (categories.Contains("Technology")) TechnologyToggle.IsOn = true;
            if (categories.Contains("Science")) ScienceToggle.IsOn = true;
            if (categories.Contains("Sports")) SportsToggle.IsOn = true;
        }
   
    <span data-ttu-id="dcc56-151">Den här uppdateringar hello huvudsidan baserat på hello status för tidigare sparats kategorier.</span><span class="sxs-lookup"><span data-stu-id="dcc56-151">This updates hello main page based on hello status of previously saved categories.</span></span>

<span data-ttu-id="dcc56-152">hello appen är nu klar och kan lagra en uppsättning kategorier i hello enheten lokal lagring används tooregister hello meddelandehubben varje gång hello användarändringar hello val av kategorier.</span><span class="sxs-lookup"><span data-stu-id="dcc56-152">hello app is now complete and can store a set of categories in hello device local storage used tooregister with hello notification hub whenever hello user changes hello selection of categories.</span></span> <span data-ttu-id="dcc56-153">Nu ska definierar vi en serverdel som kan skicka kategori meddelanden toothis app.</span><span class="sxs-lookup"><span data-stu-id="dcc56-153">Next, we will define a backend that can send category notifications toothis app.</span></span>

## <a name="sending-tagged-notifications"></a><span data-ttu-id="dcc56-154">Skicka taggade meddelanden</span><span class="sxs-lookup"><span data-stu-id="dcc56-154">Sending tagged notifications</span></span>
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a><span data-ttu-id="dcc56-155">Kör hello app och generera meddelanden</span><span class="sxs-lookup"><span data-stu-id="dcc56-155">Run hello app and generate notifications</span></span>
1. <span data-ttu-id="dcc56-156">Tryck på F5 toocompile och starta hello app i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="dcc56-156">In Visual Studio, press F5 toocompile and start hello app.</span></span>
   
    ![][1]
   
    <span data-ttu-id="dcc56-157">Obs hello appen Användargränssnittet innehåller en uppsättning växlar som kan du välja hello kategorier toosubscribe till.</span><span class="sxs-lookup"><span data-stu-id="dcc56-157">Note that hello app UI provides a set of toggles that lets you choose hello categories toosubscribe to.</span></span>
2. <span data-ttu-id="dcc56-158">Aktivera en eller flera kategorier växlar och klicka sedan på **prenumerera**.</span><span class="sxs-lookup"><span data-stu-id="dcc56-158">Enable one or more categories toggles, then click **Subscribe**.</span></span>
   
    <span data-ttu-id="dcc56-159">hello appen konverterar hello valda kategorier till taggar och begär en ny registrering av enheten för hello valt taggar från hello meddelandehubben.</span><span class="sxs-lookup"><span data-stu-id="dcc56-159">hello app converts hello selected categories into tags and requests a new device registration for hello selected tags from hello notification hub.</span></span> <span data-ttu-id="dcc56-160">hello registrerade kategorier returneras och visas i en dialogruta.</span><span class="sxs-lookup"><span data-stu-id="dcc56-160">hello registered categories are returned and displayed in a dialog.</span></span>
   
    ![][19]
3. <span data-ttu-id="dcc56-161">Skicka ett nytt meddelande från hello-serverdel i något av följande sätt hello:</span><span class="sxs-lookup"><span data-stu-id="dcc56-161">Send a new notification from hello backend in one of hello following ways:</span></span>
   
   * <span data-ttu-id="dcc56-162">**Konsolapp:** starta hello-konsolapp.</span><span class="sxs-lookup"><span data-stu-id="dcc56-162">**Console app:** start hello console app.</span></span>
   * <span data-ttu-id="dcc56-163">**Java/PHP:** kör din app eller skript.</span><span class="sxs-lookup"><span data-stu-id="dcc56-163">**Java/PHP:** run your app/script.</span></span>
     
     <span data-ttu-id="dcc56-164">Meddelanden om hello valda kategorier visas som popup-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dcc56-164">Notifications for hello selected categories appear as toast notifications.</span></span>
     
     ![][14]

## <a name="next-steps"></a><span data-ttu-id="dcc56-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dcc56-165">Next steps</span></span>
<span data-ttu-id="dcc56-166">I den här självstudiekursen vi lärt dig hur toobroadcast senaste nytt efter kategori.</span><span class="sxs-lookup"><span data-stu-id="dcc56-166">In this tutorial we learned how toobroadcast breaking news by category.</span></span> <span data-ttu-id="dcc56-167">Överväg att fylla i en av följande kurser som markerar andra scenarion med avancerad Meddelandehubbar hello:</span><span class="sxs-lookup"><span data-stu-id="dcc56-167">Consider completing one of hello following tutorials that highlight other advanced Notification Hubs scenarios:</span></span>

* <span data-ttu-id="dcc56-168">[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]</span><span class="sxs-lookup"><span data-stu-id="dcc56-168">[Use Notification Hubs toobroadcast localized breaking news]</span></span>
  
    <span data-ttu-id="dcc56-169">Lär dig hur lokaliserade tooexpand hello bryta nyheter app tooenable skicka meddelanden.</span><span class="sxs-lookup"><span data-stu-id="dcc56-169">Learn how tooexpand hello breaking news app tooenable sending localized notifications.</span></span>

<!-- Anchors. -->
[Add category selection toohello app]: #adding-categories
[Register for notifications]: #register
[Send notifications from your back-end]: #send
[Run hello app and generate notifications]: #test-app
[Next Steps]: #next-steps

<!-- Images. -->
[1]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-breakingnews-win1.png

[14]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-toast-2.png


[19]: ./media/notification-hubs-windows-store-dotnet-send-breaking-news/notification-hub-windows-reg-2.png

<!-- URLs.-->
[get-started]: /manage/services/notification-hubs/getting-started-windows-dotnet/
[Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]: /manage/services/notification-hubs/breaking-news-localized-dotnet/
[Notify users with Notification Hubs]: /manage/services/notification-hubs/notify-users
[Mobile Service]: /develop/mobile/tutorials/get-started/
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
