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
# <a name="use-notification-hubs-toosend-breaking-news"></a>Använda Notification Hubs toosend senaste nytt
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast senaste nyheterna meddelanden tooa Windows Phone 8.0/8.1 Silverlight-app. Om du utvecklar för Windows Store eller Windows Phone 8.1-app, se tootoohello [Windows Universal](notification-hubs-windows-notification-dotnet-push-xplat-segmented-wns.md) version. När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier. Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, osv.

Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben. När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande. Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg. Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).

## <a name="prerequisites"></a>Krav
Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs]. Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs].

## <a name="add-category-selection-toohello-app"></a>Lägg till kategori markeringen toohello app
hello första steget är tooadd hello UI-element tooyour befintliga huvudsidan som möjliggör hello användaren tooselect kategorier tooregister. hello kategorier som är markerad som en användare som lagras på hello enhet. När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.

1. Öppna projektfilen för hello MainPage.xaml sedan ersätta hello **rutnätet** element med namnet `TitlePanel` och `ContentPanel` med hello följande kod:
   
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
2. I hello-projekt, skapa en ny klass med namnet **meddelanden**, lägga till hello **offentliga** modifieraren toohello klassen definition och sedan lägga till följande hello **med** instruktioner toohello nya kodfil:
   
        using Microsoft.Phone.Notification;
        using Microsoft.WindowsAzure.Messaging;
        using System.IO.IsolatedStorage;
        using System.Windows;
3. Kopiera hello följande kod i hello nya **meddelanden** klass:
   
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

    Den här klassen används hello isolerad lagring toostore hello kategorier av att den här enheten är tooreceive nyheter. Den innehåller också metoder tooregister för dessa kategorier med hjälp av en [mallen](notification-hubs-templates-cross-platform-push-messages.md) registreringen av meddelanden.


1. Lägg till följande egenskapen toohello hello i hello projektfilen App.XAML.CS, **App** klass. Ersätt hello `<hub name>` och `<connection string with listen access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
   > [!NOTE]
   > Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp. Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden. hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.
   > 
   > 
2. Lägg till följande rad hello i din MainPage.xaml.cs:
   
        using Windows.UI.Popups;
3. Lägg till hello följande metod i projektfilen för hello MainPage.xaml.cs:
   
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
   
    Den här metoden skapar en lista över kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrera hello motsvarande taggar med meddelandehubben. När kategorier ändras, återskapas hello registrering med hello nya kategorier.

Appen är nu kan toostore en uppsättning kategorier i lokal lagring på hello enheten och registrerar med hello notification hub när hello användarändringar hello val av kategorier.

## <a name="register-for-notifications"></a>Registrera dig för meddelanden
De här stegen registrera hello meddelandehubben på Start med hello kategorier som har lagrats i lokal lagring.

> [!NOTE]
> Eftersom URI som tilldelats av hello Microsoft Push Notification Service (MPNS) hello-kanalen kan ändras när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel. Det här exemplet registrerar för meddelanden varje gång hello appen startas. För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.
> 
> 

1. Öppna filen för hello App.xaml.cs och Lägg till hello **asynkrona** modifierare för**Application_Launching** metoden och Ersätt hello Notification Hubs registration kod som du lade till i [Kom igång med Notification Hubs] med hello följande kod:
   
        private async void Application_Launching(object sender, LaunchingEventArgs e)
        {
            var result = await notifications.SubscribeToCategories();
   
            if (result != null)
                System.Windows.Deployment.Current.Dispatcher.BeginInvoke(() =>
                {
                    MessageBox.Show("Registration Id :" + result.RegistrationId, "Registered", MessageBoxButton.OK);
                });
        }
   
    Detta säkerställer att varje gång hello appen startar hämtas hello kategorier från lokal lagring och begär en registrering för dessa kategorier.
2. Lägg till följande kod som implementerar hello hello i hello MainPage.xaml.cs projektfilen, **OnNavigatedTo** metod:
   
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
   
    Den här uppdateringar hello huvudsidan baserat på hello status för tidigare sparats kategorier.

hello appen är nu klar och kan lagra en uppsättning kategorier i hello enheten lokal lagring används tooregister hello meddelandehubben varje gång hello användarändringar hello val av kategorier. Nu ska definierar vi en serverdel som kan skicka kategori meddelanden toothis app.

## <a name="sending-tagged-notifications"></a>Skicka taggade meddelanden
[!INCLUDE [notification-hubs-send-categories-template](../../includes/notification-hubs-send-categories-template.md)]

## <a name="run-hello-app-and-generate-notifications"></a>Kör hello app och generera meddelanden
1. Tryck på F5 toocompile och starta hello app i Visual Studio.
   
    ![][1]
   
    Obs hello appen Användargränssnittet innehåller en uppsättning växlar som kan du välja hello kategorier toosubscribe till.
2. Aktivera en eller flera kategorier växlar och klicka sedan på **prenumerera**.
   
    hello appen konverterar hello valda kategorier till taggar och begär en ny registrering av enheten för hello valt taggar från hello meddelandehubben. hello registrerade kategorier returneras och visas i en dialogruta.
   
    ![][2]
3. När du har fått en bekräftelse att kategorierna var prenumeration slutförts kör hello konsolen app toosend meddelanden för varje kategorier. Kontrollera att endast ett meddelande för hello kategorier som du prenumererar på.
   
    ![][3]

Du har slutfört det här avsnittet.

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

