---
title: "aaaNotification lokaliserade bryta nyheter kurs om Händelsehubbar"
description: "Lär dig hur toouse Azure Notification Hubs toosend lokaliserade senaste nyheterna meddelanden."
services: notification-hubs
documentationcenter: windows
author: ysxu
manager: erikre
editor: 
ms.assetid: c454f5a3-a06b-45ac-91c7-f91210889b25
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 06/29/2016
ms.author: yuaxu
ms.openlocfilehash: d273a6b384df311dea7b76ca83ccd94d9a989c4e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-notification-hubs-toosend-localized-breaking-news"></a><span data-ttu-id="bf73e-103">Använda Notification Hubs toosend lokaliserade senaste nyheterna</span><span class="sxs-lookup"><span data-stu-id="bf73e-103">Use Notification Hubs toosend localized breaking news</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bf73e-104">Windows Store C#</span><span class="sxs-lookup"><span data-stu-id="bf73e-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="bf73e-105">iOS</span><span class="sxs-lookup"><span data-stu-id="bf73e-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="bf73e-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="bf73e-106">Overview</span></span>
<span data-ttu-id="bf73e-107">Det här avsnittet beskrivs hur du toouse hello **mallen** funktion i Azure Notification Hubs toobroadcast bryter nyheter meddelanden som har översatts av språk och enheten.</span><span class="sxs-lookup"><span data-stu-id="bf73e-107">This topic shows you how toouse hello **template** feature of Azure Notification Hubs toobroadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="bf73e-108">I den här kursen börjar du med hello Windows Store-app som skapats i [använda Notification Hubs toosend senaste nytt].</span><span class="sxs-lookup"><span data-stu-id="bf73e-108">In this tutorial you start with hello Windows Store app created in [Use Notification Hubs toosend breaking news].</span></span> <span data-ttu-id="bf73e-109">När du är klar, kommer du att kunna tooregister kategorier som du är intresserad av att ange ett språk i vilka tooreceive hello meddelanden och ta emot endast push-meddelanden för hello valda kategorier på det språket.</span><span class="sxs-lookup"><span data-stu-id="bf73e-109">When complete, you will be able tooregister for categories you are interested in, specify a language in which tooreceive hello notifications, and receive only push notifications for hello selected categories in that language.</span></span>

<span data-ttu-id="bf73e-110">Det finns två delar toothis scenariot:</span><span class="sxs-lookup"><span data-stu-id="bf73e-110">There are two parts toothis scenario:</span></span>

* <span data-ttu-id="bf73e-111">hello Windows Store-app kan klienten enheter toospecify ett språk och toosubscribe toodifferent bryter nyhetskategorier.</span><span class="sxs-lookup"><span data-stu-id="bf73e-111">hello Windows Store app allows client devices toospecify a language, and toosubscribe toodifferent breaking news categories;</span></span>
* <span data-ttu-id="bf73e-112">hello backend-sänder hello-meddelanden med hjälp av hello **taggen** och **mallen** feautres i Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="bf73e-112">hello back-end broadcasts hello notifications, using hello **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="bf73e-113">Krav</span><span class="sxs-lookup"><span data-stu-id="bf73e-113">Prerequisites</span></span>
<span data-ttu-id="bf73e-114">Du måste redan har slutfört hello [använda Notification Hubs toosend senaste nytt] självstudier och ha hello-kod som är tillgängliga, eftersom den här kursen bygger direkt på koden.</span><span class="sxs-lookup"><span data-stu-id="bf73e-114">You must have already completed hello [Use Notification Hubs toosend breaking news] tutorial and have hello code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="bf73e-115">Du måste också Visual Studio 2012 eller senare.</span><span class="sxs-lookup"><span data-stu-id="bf73e-115">You also need Visual Studio 2012 or later.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="bf73e-116">Mall-begrepp</span><span class="sxs-lookup"><span data-stu-id="bf73e-116">Template concepts</span></span>
<span data-ttu-id="bf73e-117">I [använda Notification Hubs toosend senaste nytt] du skapat en app som används för **taggar** toosubscribe toonotifications för olika nyhetskategorier.</span><span class="sxs-lookup"><span data-stu-id="bf73e-117">In [Use Notification Hubs toosend breaking news] you built an app that used **tags** toosubscribe toonotifications for different news categories.</span></span>
<span data-ttu-id="bf73e-118">Många appar dock mål flera marknader och kräver lokalisering.</span><span class="sxs-lookup"><span data-stu-id="bf73e-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="bf73e-119">Detta innebär att hello hello meddelanden själva innehållet har toobe lokaliserade och levererat toohello korrigera uppsättning enheter.</span><span class="sxs-lookup"><span data-stu-id="bf73e-119">This means that hello content of hello notifications themselves have toobe localized and delivered toohello correct set of devices.</span></span>
<span data-ttu-id="bf73e-120">I det här avsnittet visas hur toouse hello **mallen** funktion i Notification Hubs tooeasily leverera lokaliserade senaste nyheterna meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bf73e-120">In this topic we will show how toouse hello **template** feature of Notification Hubs tooeasily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="bf73e-121">Obs: enkelriktade toosend lokaliserade meddelanden är toocreate flera versioner av varje tagg.</span><span class="sxs-lookup"><span data-stu-id="bf73e-121">Note: one way toosend localized notifications is toocreate multiple versions of each tag.</span></span> <span data-ttu-id="bf73e-122">Till exempel toosupport engelska, franska och Mandarin kan vi behöver tre olika taggar för world news: ”world_en”, ”world_fr” och ”world_ch”.</span><span class="sxs-lookup"><span data-stu-id="bf73e-122">For instance, toosupport English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="bf73e-123">Vi har sedan toosend en lokaliserad version av hello world news tooeach taggar.</span><span class="sxs-lookup"><span data-stu-id="bf73e-123">We would then have toosend a localized version of hello world news tooeach of these tags.</span></span> <span data-ttu-id="bf73e-124">I det här avsnittet använder vi mallar tooavoid hello spridning av taggar och hello krav flera meddelanden.</span><span class="sxs-lookup"><span data-stu-id="bf73e-124">In this topic we use templates tooavoid hello proliferation of tags and hello requirement of sending multiple messages.</span></span>

<span data-ttu-id="bf73e-125">På en hög nivå mallar är ett sätt toospecify hur en specifik enhet bör få ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="bf73e-125">At a high level, templates are a way toospecify how a specific device should receive a notification.</span></span> <span data-ttu-id="bf73e-126">hello mallen anger hello exakt nyttolastformatet genom att referera tooproperties som ingår i hello-meddelande som skickas av din appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="bf73e-126">hello template specifies hello exact payload format by referring tooproperties that are part of hello message sent by your app back-end.</span></span> <span data-ttu-id="bf73e-127">I vårt fall skickar vi ett språkvariant-oberoende meddelande som innehåller alla språk som stöds:</span><span class="sxs-lookup"><span data-stu-id="bf73e-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="bf73e-128">Vi kommer sedan att säkerställa att registrera enheter med en mall som refererar rätt toohello-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="bf73e-128">Then we will ensure that devices register with a template that refers toohello correct property.</span></span> <span data-ttu-id="bf73e-129">Till exempel en Windows Store-app som vill tooreceive ett enkelt popup-meddelande registreras för hello följande mall med en motsvarande taggar:</span><span class="sxs-lookup"><span data-stu-id="bf73e-129">For instance, a Windows Store app that wants tooreceive a simple toast message will register for hello following template with any corresponding tags:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



<span data-ttu-id="bf73e-130">Mallar är en mycket kraftfull funktion kan du läsa mer om i vår [mallar](notification-hubs-templates-cross-platform-push-messages.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="bf73e-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span> 

## <a name="hello-app-user-interface"></a><span data-ttu-id="bf73e-131">användargränssnittet för hello app</span><span class="sxs-lookup"><span data-stu-id="bf73e-131">hello app user interface</span></span>
<span data-ttu-id="bf73e-132">Vi kommer nu att ändra hello bryta nyheter app som du skapade i avsnittet hello [använda Notification Hubs toosend senaste nytt] toosend lokaliserade senaste nyheterna med hjälp av mallar.</span><span class="sxs-lookup"><span data-stu-id="bf73e-132">We will now modify hello Breaking News app that you created in hello topic [Use Notification Hubs toosend breaking news] toosend localized breaking news using templates.</span></span>

<span data-ttu-id="bf73e-133">I Windows Store-appen:</span><span class="sxs-lookup"><span data-stu-id="bf73e-133">In your Windows Store app:</span></span>

<span data-ttu-id="bf73e-134">Ändra din MainPage.xaml tooinclude en combobox språk:</span><span class="sxs-lookup"><span data-stu-id="bf73e-134">Change your MainPage.xaml tooinclude a locale combobox:</span></span>

    <Grid Margin="120, 58, 120, 80"  
            Background="{StaticResource ApplicationPageBackgroundThemeBrush}">
        <Grid.RowDefinitions>
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
            <RowDefinition />
        </Grid.RowDefinitions>
        <Grid.ColumnDefinitions>
            <ColumnDefinition />
            <ColumnDefinition />
        </Grid.ColumnDefinitions>
        <TextBlock Grid.Row="0" Grid.Column="0" Grid.ColumnSpan="2"  TextWrapping="Wrap" Text="Breaking News" FontSize="42" VerticalAlignment="Top"/>
        <ComboBox Name="Locale" HorizontalAlignment="Left" VerticalAlignment="Center" Width="200" Grid.Row="1" Grid.Column="0">
            <x:String>English</x:String>
            <x:String>French</x:String>
            <x:String>Mandarin</x:String>
        </ComboBox>
        <ToggleSwitch Header="World" Name="WorldToggle" Grid.Row="2" Grid.Column="0"/>
        <ToggleSwitch Header="Politics" Name="PoliticsToggle" Grid.Row="3" Grid.Column="0"/>
        <ToggleSwitch Header="Business" Name="BusinessToggle" Grid.Row="4" Grid.Column="0"/>
        <ToggleSwitch Header="Technology" Name="TechnologyToggle" Grid.Row="2" Grid.Column="1"/>
        <ToggleSwitch Header="Science" Name="ScienceToggle" Grid.Row="3" Grid.Column="1"/>
        <ToggleSwitch Header="Sports" Name="SportsToggle" Grid.Row="4" Grid.Column="1"/>
        <Button Content="Subscribe" HorizontalAlignment="Center" Grid.Row="5" Grid.Column="0" Grid.ColumnSpan="2" Click="SubscribeButton_Click" />
    </Grid>

## <a name="building-hello-windows-store-client-app"></a><span data-ttu-id="bf73e-135">Skapa hello Windows Store-klientappen</span><span class="sxs-lookup"><span data-stu-id="bf73e-135">Building hello Windows Store client app</span></span>
1. <span data-ttu-id="bf73e-136">Klassen meddelanden, lägga till ett språk parametern tooyour *StoreCategoriesAndSubscribe* och *SubscribeToCateories* metoder.</span><span class="sxs-lookup"><span data-stu-id="bf73e-136">In your Notifications class, add a locale parameter tooyour  *StoreCategoriesAndSubscribe* and *SubscribeToCateories* methods.</span></span>
   
        public async Task<Registration> StoreCategoriesAndSubscribe(string locale, IEnumerable<string> categories)
        {
            ApplicationData.Current.LocalSettings.Values["categories"] = string.Join(",", categories);
            ApplicationData.Current.LocalSettings.Values["locale"] = locale;
            return await SubscribeToCategories(categories);
        }
   
        public async Task<Registration> SubscribeToCategories(string locale, IEnumerable<string> categories = null)
        {
            var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
   
            if (categories == null)
            {
                categories = RetrieveCategories();
            }
   
            // Using a template registration. This makes supporting notifications across other platforms much easier.
            // Using hello localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    <span data-ttu-id="bf73e-137">Observera att i stället för att anropa hello *RegisterNativeAsync* metoden som vi kallar *RegisterTemplateAsync*: vi registrerar formatet specifika meddelanden i vilka hello mallen beror på hello språk.</span><span class="sxs-lookup"><span data-stu-id="bf73e-137">Note that instead of calling hello *RegisterNativeAsync* method we call *RegisterTemplateAsync*: we are registering a specific notification format in which hello template depends on hello locale.</span></span> <span data-ttu-id="bf73e-138">Vi också ange ett namn för mallen hello (”localizedWNSTemplateExample”), eftersom det kan vara bra tooregister mer än en mall (till exempel en för popup-meddelanden) och en för paneler och vi behöver tooname dem i ordning toobe kan tooupdate eller tar bort dem.</span><span class="sxs-lookup"><span data-stu-id="bf73e-138">We also provide a name for hello template ("localizedWNSTemplateExample"), because we might want tooregister more than one template (for instance one for toast notifications and one for tiles) and we need tooname them in order toobe able tooupdate or delete them.</span></span>
   
    <span data-ttu-id="bf73e-139">Observera att om en enhet registrerar flera mallar med hello samma tagg, ett inkommande meddelande som mål att taggen resulterar i flera meddelanden levereras toohello enhet (en för varje mall).</span><span class="sxs-lookup"><span data-stu-id="bf73e-139">Note that if a device registers multiple templates with hello same tag, an incoming message targeting that tag will result in multiple notifications delivered toohello device (one for each template).</span></span> <span data-ttu-id="bf73e-140">Detta är användbart när hello samma logiska meddelande har tooresult i flera visuella meddelanden, till exempel visar både en Aktivitetsikon och ett popup-meddelande i en Windows Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="bf73e-140">This behavior is useful when hello same logical message has tooresult in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
2. <span data-ttu-id="bf73e-141">Lägg till hello följa metoden tooretrieve hello lagrade språk:</span><span class="sxs-lookup"><span data-stu-id="bf73e-141">Add hello following method tooretrieve hello stored locale:</span></span>
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. <span data-ttu-id="bf73e-142">Uppdatera din knapp i din MainPage.xaml.cs klickar du på hanteraren genom att hämta hello aktuellt värde för kombinationsruta för hello språk och den toohello anropet toohello meddelanden klassen enligt:</span><span class="sxs-lookup"><span data-stu-id="bf73e-142">In your MainPage.xaml.cs, update your button click handler by retrieving hello current value of hello Locale combo box and providing it toohello call toohello Notifications class, as shown:</span></span>
   
        private async void SubscribeButton_Click(object sender, RoutedEventArgs e)
        {
            var locale = (string)Locale.SelectedItem;
   
            var categories = new HashSet<string>();
            if (WorldToggle.IsOn) categories.Add("World");
            if (PoliticsToggle.IsOn) categories.Add("Politics");
            if (BusinessToggle.IsOn) categories.Add("Business");
            if (TechnologyToggle.IsOn) categories.Add("Technology");
            if (ScienceToggle.IsOn) categories.Add("Science");
            if (SportsToggle.IsOn) categories.Add("Sports");
   
            var result = await ((App)Application.Current).notifications.StoreCategoriesAndSubscribe(locale,
                 categories);
   
            var dialog = new MessageDialog("Locale: " + locale + " Subscribed to: " + 
                string.Join(",", categories) + " on registration Id: " + result.RegistrationId);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
        }
4. <span data-ttu-id="bf73e-143">Kontrollera slutligen att tooupdate i filen App.xaml.cs, din `InitNotificationsAsync` metoden tooretrieve hello nationella inställningar och använda den när du prenumererar:</span><span class="sxs-lookup"><span data-stu-id="bf73e-143">Finally, in your App.xaml.cs file, make sure tooupdate your `InitNotificationsAsync` method tooretrieve hello locale and use it when subscribing:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays hello registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a><span data-ttu-id="bf73e-144">Skicka lokaliserade meddelanden från serverdelen</span><span class="sxs-lookup"><span data-stu-id="bf73e-144">Send localized notifications from your back-end</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[hello app user interface]: #ui
[Building hello Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
[använda Notification Hubs toosend senaste nytt]: /manage/services/notification-hubs/breaking-news-dotnet

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications tooapp users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-toofor iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-toofor Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
