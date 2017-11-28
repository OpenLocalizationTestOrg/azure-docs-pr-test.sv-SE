---
title: "Notification Hubs lokaliserade senaste nyheterna självstudiekursen"
description: "Lär dig hur du använder Azure Notification Hubs för att skicka lokaliserade senaste nyheterna meddelanden."
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
ms.openlocfilehash: e864e832b4c50644bf4062dee29d34ff9fe2774e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-notification-hubs-to-send-localized-breaking-news"></a><span data-ttu-id="9cedc-103">Använda Notification Hubs för att skicka lokaliserade senaste nyheterna</span><span class="sxs-lookup"><span data-stu-id="9cedc-103">Use Notification Hubs to send localized breaking news</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="9cedc-104">Windows Store C#</span><span class="sxs-lookup"><span data-stu-id="9cedc-104">Windows Store C#</span></span>](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [<span data-ttu-id="9cedc-105">iOS</span><span class="sxs-lookup"><span data-stu-id="9cedc-105">iOS</span></span>](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a><span data-ttu-id="9cedc-106">Översikt</span><span class="sxs-lookup"><span data-stu-id="9cedc-106">Overview</span></span>
<span data-ttu-id="9cedc-107">Det här avsnittet visar hur du använder den **mallen** funktion i Azure Notification Hubs för att sända senaste nyheterna meddelanden som har översatts av språk och enheten.</span><span class="sxs-lookup"><span data-stu-id="9cedc-107">This topic shows you how to use the **template** feature of Azure Notification Hubs to broadcast breaking news notifications that have been localized by language and device.</span></span> <span data-ttu-id="9cedc-108">I den här kursen börjar du med Windows Store-app som skapats i [använda Notification Hubs för att skicka de senaste nyheterna].</span><span class="sxs-lookup"><span data-stu-id="9cedc-108">In this tutorial you start with the Windows Store app created in [Use Notification Hubs to send breaking news].</span></span> <span data-ttu-id="9cedc-109">När du är klar kommer du att kunna registrera för kategorier som du vill, ange ett språk som ska ta emot meddelanden och får endast push-meddelanden i valda kategorier på det språket.</span><span class="sxs-lookup"><span data-stu-id="9cedc-109">When complete, you will be able to register for categories you are interested in, specify a language in which to receive the notifications, and receive only push notifications for the selected categories in that language.</span></span>

<span data-ttu-id="9cedc-110">Det finns två delar i det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="9cedc-110">There are two parts to this scenario:</span></span>

* <span data-ttu-id="9cedc-111">Windows Store-appen kan klienten enheter för att ange ett språk och prenumerera på olika senaste nyheterna kategorier.</span><span class="sxs-lookup"><span data-stu-id="9cedc-111">the Windows Store app allows client devices to specify a language, and to subscribe to different breaking news categories;</span></span>
* <span data-ttu-id="9cedc-112">backend-skickar meddelanden med hjälp av den **taggen** och **mallen** feautres i Azure Notification Hubs.</span><span class="sxs-lookup"><span data-stu-id="9cedc-112">the back-end broadcasts the notifications, using the **tag** and **template** feautres of Azure Notification Hubs.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9cedc-113">Krav</span><span class="sxs-lookup"><span data-stu-id="9cedc-113">Prerequisites</span></span>
<span data-ttu-id="9cedc-114">Du måste redan har slutfört den [använda Notification Hubs för att skicka de senaste nyheterna] självstudier och har kod som är tillgängliga, eftersom den här kursen bygger direkt på koden.</span><span class="sxs-lookup"><span data-stu-id="9cedc-114">You must have already completed the [Use Notification Hubs to send breaking news] tutorial and have the code available, because this tutorial builds directly upon that code.</span></span>

<span data-ttu-id="9cedc-115">Du måste också Visual Studio 2012 eller senare.</span><span class="sxs-lookup"><span data-stu-id="9cedc-115">You also need Visual Studio 2012 or later.</span></span>

## <a name="template-concepts"></a><span data-ttu-id="9cedc-116">Mall-begrepp</span><span class="sxs-lookup"><span data-stu-id="9cedc-116">Template concepts</span></span>
<span data-ttu-id="9cedc-117">I [använda Notification Hubs för att skicka de senaste nyheterna] du skapat en app som används för **taggar** att prenumerera på meddelanden om nyheterna i olika kategorier.</span><span class="sxs-lookup"><span data-stu-id="9cedc-117">In [Use Notification Hubs to send breaking news] you built an app that used **tags** to subscribe to notifications for different news categories.</span></span>
<span data-ttu-id="9cedc-118">Många appar dock mål flera marknader och kräver lokalisering.</span><span class="sxs-lookup"><span data-stu-id="9cedc-118">Many apps, however, target multiple markets and require localization.</span></span> <span data-ttu-id="9cedc-119">Detta innebär att innehållet i meddelandena själva måste lokaliserade och levereras till rätt uppsättning enheter.</span><span class="sxs-lookup"><span data-stu-id="9cedc-119">This means that the content of the notifications themselves have to be localized and delivered to the correct set of devices.</span></span>
<span data-ttu-id="9cedc-120">I det här avsnittet visar vi hur du använder den **mallen** funktion i Notification Hubs för att leverera enkelt lokaliserade senaste nyheterna meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9cedc-120">In this topic we will show how to use the **template** feature of Notification Hubs to easily deliver localized breaking news notifications.</span></span>

<span data-ttu-id="9cedc-121">Obs: ett sätt att skicka lokaliserade meddelanden är att skapa flera versioner av varje tagg.</span><span class="sxs-lookup"><span data-stu-id="9cedc-121">Note: one way to send localized notifications is to create multiple versions of each tag.</span></span> <span data-ttu-id="9cedc-122">Till exempel för att stödja engelska, franska och Mandarin, vi behöver tre olika taggar för world news: ”world_en”, ”world_fr” och ”world_ch”.</span><span class="sxs-lookup"><span data-stu-id="9cedc-122">For instance, to support English, French, and Mandarin, we would need three different tags for world news: "world_en", "world_fr", and "world_ch".</span></span> <span data-ttu-id="9cedc-123">Vi har sedan skicka en lokaliserad version av world nyheter till var och en av dessa taggar.</span><span class="sxs-lookup"><span data-stu-id="9cedc-123">We would then have to send a localized version of the world news to each of these tags.</span></span> <span data-ttu-id="9cedc-124">Vi använder mallar för att undvika alltför många taggar och kravet flera meddelanden i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="9cedc-124">In this topic we use templates to avoid the proliferation of tags and the requirement of sending multiple messages.</span></span>

<span data-ttu-id="9cedc-125">Mallar är ett sätt att ange hur en specifik enhet bör få ett meddelande på en hög nivå.</span><span class="sxs-lookup"><span data-stu-id="9cedc-125">At a high level, templates are a way to specify how a specific device should receive a notification.</span></span> <span data-ttu-id="9cedc-126">Mallen anger exakt nyttolastformatet genom att referera till egenskaper som ingår i meddelandet som skickas av din appens serverdel.</span><span class="sxs-lookup"><span data-stu-id="9cedc-126">The template specifies the exact payload format by referring to properties that are part of the message sent by your app back-end.</span></span> <span data-ttu-id="9cedc-127">I vårt fall skickar vi ett språkvariant-oberoende meddelande som innehåller alla språk som stöds:</span><span class="sxs-lookup"><span data-stu-id="9cedc-127">In our case, we will send a locale-agnostic message containing all supported languages:</span></span>

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

<span data-ttu-id="9cedc-128">Vi kommer sedan att säkerställa att registrera enheter med en mall som refererar till egenskapen korrekt.</span><span class="sxs-lookup"><span data-stu-id="9cedc-128">Then we will ensure that devices register with a template that refers to the correct property.</span></span> <span data-ttu-id="9cedc-129">Till exempel registrerar en Windows Store-app som du vill få ett enkelt popup-meddelande för följande mall med en motsvarande taggar:</span><span class="sxs-lookup"><span data-stu-id="9cedc-129">For instance, a Windows Store app that wants to receive a simple toast message will register for the following template with any corresponding tags:</span></span>

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



<span data-ttu-id="9cedc-130">Mallar är en mycket kraftfull funktion kan du läsa mer om i vår [mallar](notification-hubs-templates-cross-platform-push-messages.md) artikel.</span><span class="sxs-lookup"><span data-stu-id="9cedc-130">Templates are a very powerful feature you can learn more about in our [Templates](notification-hubs-templates-cross-platform-push-messages.md) article.</span></span> 

## <a name="the-app-user-interface"></a><span data-ttu-id="9cedc-131">Användargränssnittet för app</span><span class="sxs-lookup"><span data-stu-id="9cedc-131">The app user interface</span></span>
<span data-ttu-id="9cedc-132">Vi kommer nu att ändra bryta nyheter appen som du skapade i avsnittet [använda Notification Hubs för att skicka de senaste nyheterna] att skicka lokaliserade senaste nytt med hjälp av mallar.</span><span class="sxs-lookup"><span data-stu-id="9cedc-132">We will now modify the Breaking News app that you created in the topic [Use Notification Hubs to send breaking news] to send localized breaking news using templates.</span></span>

<span data-ttu-id="9cedc-133">I Windows Store-appen:</span><span class="sxs-lookup"><span data-stu-id="9cedc-133">In your Windows Store app:</span></span>

<span data-ttu-id="9cedc-134">Ändra din MainPage.xaml om du vill inkludera en combobox språk:</span><span class="sxs-lookup"><span data-stu-id="9cedc-134">Change your MainPage.xaml to include a locale combobox:</span></span>

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

## <a name="building-the-windows-store-client-app"></a><span data-ttu-id="9cedc-135">Skapa Windows Store-klientappen</span><span class="sxs-lookup"><span data-stu-id="9cedc-135">Building the Windows Store client app</span></span>
1. <span data-ttu-id="9cedc-136">Klassen meddelanden, lägga till en språkvariantparameter till din *StoreCategoriesAndSubscribe* och *SubscribeToCateories* metoder.</span><span class="sxs-lookup"><span data-stu-id="9cedc-136">In your Notifications class, add a locale parameter to your  *StoreCategoriesAndSubscribe* and *SubscribeToCateories* methods.</span></span>
   
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
            // Using the localized tags based on locale selected.
            string templateBodyWNS = String.Format("<toast><visual><binding template=\"ToastText01\"><text id=\"1\">$(News_{0})</text></binding></visual></toast>", locale);
   
            return await hub.RegisterTemplateAsync(channel.Uri, templateBodyWNS, "localizedWNSTemplateExample", categories);
        }
   
    <span data-ttu-id="9cedc-137">Observera att i stället för att anropa den *RegisterNativeAsync* metoden som vi kallar *RegisterTemplateAsync*: vi registrerar ett visst meddelande format där mallen beror på språket.</span><span class="sxs-lookup"><span data-stu-id="9cedc-137">Note that instead of calling the *RegisterNativeAsync* method we call *RegisterTemplateAsync*: we are registering a specific notification format in which the template depends on the locale.</span></span> <span data-ttu-id="9cedc-138">Vi kan också ange ett namn för mallen (”localizedWNSTemplateExample”), eftersom det kan vara bra att registrera mer än en mall (till exempel en för popup-meddelanden) och en för paneler och behöver ge dem för att kunna uppdatera eller ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="9cedc-138">We also provide a name for the template ("localizedWNSTemplateExample"), because we might want to register more than one template (for instance one for toast notifications and one for tiles) and we need to name them in order to be able to update or delete them.</span></span>
   
    <span data-ttu-id="9cedc-139">Observera att om en enhet registrerar flera mallar med samma tagg, ett inkommande meddelande mål som tagg leder till flera meddelanden levereras till enheten (en för varje mall).</span><span class="sxs-lookup"><span data-stu-id="9cedc-139">Note that if a device registers multiple templates with the same tag, an incoming message targeting that tag will result in multiple notifications delivered to the device (one for each template).</span></span> <span data-ttu-id="9cedc-140">Detta är användbart när samma logiska meddelande måste resultera i flera visuella meddelanden, till exempel visar både en Aktivitetsikon och ett popup-meddelande i en Windows Store-programmet.</span><span class="sxs-lookup"><span data-stu-id="9cedc-140">This behavior is useful when the same logical message has to result in multiple visual notifications, for instance showing both a badge and a toast in a Windows Store application.</span></span>
2. <span data-ttu-id="9cedc-141">Lägg till följande metod för att hämta lagrade språk:</span><span class="sxs-lookup"><span data-stu-id="9cedc-141">Add the following method to retrieve the stored locale:</span></span>
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. <span data-ttu-id="9cedc-142">Uppdatera din knapp i din MainPage.xaml.cs på hanterare av hämtar det aktuella värdet för kombinationsrutan språk och att anropet till klassen meddelanden som visas:</span><span class="sxs-lookup"><span data-stu-id="9cedc-142">In your MainPage.xaml.cs, update your button click handler by retrieving the current value of the Locale combo box and providing it to the call to the Notifications class, as shown:</span></span>
   
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
4. <span data-ttu-id="9cedc-143">Slutligen i filen App.xaml.cs, se till att uppdatera din `InitNotificationsAsync` metoden för att hämta språk och använda den när prenumerera:</span><span class="sxs-lookup"><span data-stu-id="9cedc-143">Finally, in your App.xaml.cs file, make sure to update your `InitNotificationsAsync` method to retrieve the locale and use it when subscribing:</span></span>
   
        private async void InitNotificationsAsync()
        {
            var result = await notifications.SubscribeToCategories(notifications.RetrieveLocale());
   
            // Displays the registration ID so you know it was successful
            if (result.RegistrationId != null)
            {
                var dialog = new MessageDialog("Registration successful: " + result.RegistrationId);
                dialog.Commands.Add(new UICommand("OK"));
                await dialog.ShowAsync();
            }
        }

## <a name="send-localized-notifications-from-your-back-end"></a><span data-ttu-id="9cedc-144">Skicka lokaliserade meddelanden från serverdelen</span><span class="sxs-lookup"><span data-stu-id="9cedc-144">Send localized notifications from your back-end</span></span>
[!INCLUDE [notification-hubs-localized-back-end](../../includes/notification-hubs-localized-back-end.md)]

<!-- Anchors. -->
[Template concepts]: #concepts
[The app user interface]: #ui
[Building the Windows Store client app]: #building-client
[Send notifications from your back-end]: #send
[Next Steps]:#next-steps

<!-- Images. -->

<!-- URLs. -->
[Mobile Service]: /develop/mobile/tutorials/get-started
[Notify users with Notification Hubs: ASP.NET]: /manage/services/notification-hubs/notify-users-aspnet
[Notify users with Notification Hubs: Mobile Services]: /manage/services/notification-hubs/notify-users
<span data-ttu-id="9cedc-145">[använda Notification Hubs för att skicka de senaste nyheterna]: /manage/services/notification-hubs/breaking-news-dotnet</span><span class="sxs-lookup"><span data-stu-id="9cedc-145">[Use Notification Hubs to send breaking news]: /manage/services/notification-hubs/breaking-news-dotnet</span></span>

[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Live SDK for Windows]: http://go.microsoft.com/fwlink/p/?LinkId=262253
[Get started with Mobile Services]: /develop/mobile/tutorials/get-started/#create-new-service
[Get started with data]: /develop/mobile/tutorials/get-started-with-data-dotnet
[Get started with authentication]: /develop/mobile/tutorials/get-started-with-users-dotnet
[Get started with push notifications]: /develop/mobile/tutorials/get-started-with-push-dotnet
[Push notifications to app users]: /develop/mobile/tutorials/push-notifications-to-app-users-dotnet
[Authorize users with scripts]: /develop/mobile/tutorials/authorize-users-in-scripts-dotnet
[JavaScript and HTML]: /develop/mobile/tutorials/get-started-with-push-js

[wns object]: http://go.microsoft.com/fwlink/p/?LinkId=260591
[Notification Hubs Guidance]: http://msdn.microsoft.com/library/jj927170.aspx
[Notification Hubs How-To for iOS]: http://msdn.microsoft.com/library/jj927168.aspx
[Notification Hubs How-To for Windows Store]: http://msdn.microsoft.com/library/jj927172.aspx
