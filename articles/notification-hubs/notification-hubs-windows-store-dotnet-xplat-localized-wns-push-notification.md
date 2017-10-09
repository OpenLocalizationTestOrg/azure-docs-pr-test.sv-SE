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
# <a name="use-notification-hubs-toosend-localized-breaking-news"></a>Använda Notification Hubs toosend lokaliserade senaste nyheterna
> [!div class="op_single_selector"]
> * [Windows Store C#](notification-hubs-windows-store-dotnet-xplat-localized-wns-push-notification.md)
> * [iOS](notification-hubs-ios-xplat-localized-apns-push-notification.md)
> 
> 

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur du toouse hello **mallen** funktion i Azure Notification Hubs toobroadcast bryter nyheter meddelanden som har översatts av språk och enheten. I den här kursen börjar du med hello Windows Store-app som skapats i [använda Notification Hubs toosend senaste nytt]. När du är klar, kommer du att kunna tooregister kategorier som du är intresserad av att ange ett språk i vilka tooreceive hello meddelanden och ta emot endast push-meddelanden för hello valda kategorier på det språket.

Det finns två delar toothis scenariot:

* hello Windows Store-app kan klienten enheter toospecify ett språk och toosubscribe toodifferent bryter nyhetskategorier.
* hello backend-sänder hello-meddelanden med hjälp av hello **taggen** och **mallen** feautres i Azure Notification Hubs.

## <a name="prerequisites"></a>Krav
Du måste redan har slutfört hello [använda Notification Hubs toosend senaste nytt] självstudier och ha hello-kod som är tillgängliga, eftersom den här kursen bygger direkt på koden.

Du måste också Visual Studio 2012 eller senare.

## <a name="template-concepts"></a>Mall-begrepp
I [använda Notification Hubs toosend senaste nytt] du skapat en app som används för **taggar** toosubscribe toonotifications för olika nyhetskategorier.
Många appar dock mål flera marknader och kräver lokalisering. Detta innebär att hello hello meddelanden själva innehållet har toobe lokaliserade och levererat toohello korrigera uppsättning enheter.
I det här avsnittet visas hur toouse hello **mallen** funktion i Notification Hubs tooeasily leverera lokaliserade senaste nyheterna meddelanden.

Obs: enkelriktade toosend lokaliserade meddelanden är toocreate flera versioner av varje tagg. Till exempel toosupport engelska, franska och Mandarin kan vi behöver tre olika taggar för world news: ”world_en”, ”world_fr” och ”world_ch”. Vi har sedan toosend en lokaliserad version av hello world news tooeach taggar. I det här avsnittet använder vi mallar tooavoid hello spridning av taggar och hello krav flera meddelanden.

På en hög nivå mallar är ett sätt toospecify hur en specifik enhet bör få ett meddelande. hello mallen anger hello exakt nyttolastformatet genom att referera tooproperties som ingår i hello-meddelande som skickas av din appens serverdel. I vårt fall skickar vi ett språkvariant-oberoende meddelande som innehåller alla språk som stöds:

    {
        "News_English": "...",
        "News_French": "...",
        "News_Mandarin": "..."
    }

Vi kommer sedan att säkerställa att registrera enheter med en mall som refererar rätt toohello-egenskapen. Till exempel en Windows Store-app som vill tooreceive ett enkelt popup-meddelande registreras för hello följande mall med en motsvarande taggar:

    <toast>
      <visual>
        <binding template=\"ToastText01\">
          <text id=\"1\">$(News_English)</text>
        </binding>
      </visual>
    </toast>



Mallar är en mycket kraftfull funktion kan du läsa mer om i vår [mallar](notification-hubs-templates-cross-platform-push-messages.md) artikel. 

## <a name="hello-app-user-interface"></a>användargränssnittet för hello app
Vi kommer nu att ändra hello bryta nyheter app som du skapade i avsnittet hello [använda Notification Hubs toosend senaste nytt] toosend lokaliserade senaste nyheterna med hjälp av mallar.

I Windows Store-appen:

Ändra din MainPage.xaml tooinclude en combobox språk:

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

## <a name="building-hello-windows-store-client-app"></a>Skapa hello Windows Store-klientappen
1. Klassen meddelanden, lägga till ett språk parametern tooyour *StoreCategoriesAndSubscribe* och *SubscribeToCateories* metoder.
   
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
   
    Observera att i stället för att anropa hello *RegisterNativeAsync* metoden som vi kallar *RegisterTemplateAsync*: vi registrerar formatet specifika meddelanden i vilka hello mallen beror på hello språk. Vi också ange ett namn för mallen hello (”localizedWNSTemplateExample”), eftersom det kan vara bra tooregister mer än en mall (till exempel en för popup-meddelanden) och en för paneler och vi behöver tooname dem i ordning toobe kan tooupdate eller tar bort dem.
   
    Observera att om en enhet registrerar flera mallar med hello samma tagg, ett inkommande meddelande som mål att taggen resulterar i flera meddelanden levereras toohello enhet (en för varje mall). Detta är användbart när hello samma logiska meddelande har tooresult i flera visuella meddelanden, till exempel visar både en Aktivitetsikon och ett popup-meddelande i en Windows Store-programmet.
2. Lägg till hello följa metoden tooretrieve hello lagrade språk:
   
        public string RetrieveLocale()
        {
            var locale = (string) ApplicationData.Current.LocalSettings.Values["locale"];
            return locale != null ? locale : "English";
        }
3. Uppdatera din knapp i din MainPage.xaml.cs klickar du på hanteraren genom att hämta hello aktuellt värde för kombinationsruta för hello språk och den toohello anropet toohello meddelanden klassen enligt:
   
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
4. Kontrollera slutligen att tooupdate i filen App.xaml.cs, din `InitNotificationsAsync` metoden tooretrieve hello nationella inställningar och använda den när du prenumererar:
   
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

## <a name="send-localized-notifications-from-your-back-end"></a>Skicka lokaliserade meddelanden från serverdelen
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
