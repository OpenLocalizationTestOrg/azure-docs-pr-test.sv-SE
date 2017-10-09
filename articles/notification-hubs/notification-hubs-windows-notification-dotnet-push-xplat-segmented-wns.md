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
# <a name="use-notification-hubs-toosend-breaking-news"></a>Använda Notification Hubs toosend senaste nytt
[!INCLUDE [notification-hubs-selector-breaking-news](../../includes/notification-hubs-selector-breaking-news.md)]

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur du toouse Azure Notification Hubs toobroadcast bryter nyheter meddelanden tooa Windows Store eller Windows Phone 8.1 (utan Silverlight)-app. Om du utvecklar för Windows Phone 8.1 Silverlight, se toohello [Windows Phone](notification-hubs-windows-phone-push-xplat-segmented-mpns-notification.md) version. När du är klar kommer du att kan tooregister för att analysera nyhetskategorier som du är intresserad av och får endast push-meddelanden för dessa kategorier. Det här scenariot är ett vanligt mönster för många appar där meddelanden har toobe skickas toogroups med användare som har tidigare ha deklarerats intresse för dem, t.ex. RSS-läsare, appar för musik fläktar, och så vidare. 

Broadcast scenarier aktiveras genom att inkludera en eller flera *taggar* när du skapar en registrering i hello meddelandehubben. När meddelanden skickas tooa tagg, får alla enheter som har registrerats för hello tagg hello-meddelande. Eftersom taggar är bara strängar kan har de inte toobe etableras i förväg. Mer information om taggar finns för[Notification Hubs Routning och Tagguttryck](notification-hubs-tags-segment-push-message.md).

> [!NOTE]
> Windows Store och Windows Phone-projekt version 8.1 och tidigare stöds inte i Visual Studio-2017.  Mer information finns i [Visual Studio 2017 Platform Targeting and Compatibility](https://www.visualstudio.com/en-us/productinfo/vs2017-compatibility-vs) (Visual Studio 2017 – målplattform och plattformskompatibilitet). 

## <a name="prerequisites"></a>Krav
Det här avsnittet bygger på hello-app som du skapade i [Kom igång med Notification Hubs][get-started]. Innan du börjar den här kursen ska du måste redan har slutfört [Kom igång med Notification Hubs][get-started].

## <a name="add-category-selection-toohello-app"></a>Lägg till kategori markeringen toohello app
hello första steget är tooadd hello UI-element tooyour befintliga huvudsidan som möjliggör hello användaren tooselect kategorier tooregister. hello kategorier som är markerad som en användare som lagras på hello enhet. När hello appen startar, skapas en enhetsregistrering i meddelandehubben med hello valda kategorier som taggar.

1. Öppna projektfilen för hello MainPage.xaml och sedan kopiera hello följande kod i hello **rutnätet** element:
   
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
2. Högerklicka på hello **delade** projekt och Lägg till en ny klass med namnet **meddelanden**, lägga till hello **offentliga** modifieraren toohello klassen definition och sedan lägga till följande hello **med** instruktioner toohello nya kodfil:
   
        using Windows.Networking.PushNotifications;
        using Microsoft.WindowsAzure.Messaging;
        using Windows.Storage;
        using System.Threading.Tasks;
3. Kopiera hello följande kod i hello nya **meddelanden** klass:
   
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
   
    Den här klassen används hello lokal lagring toostore hello kategorier av nyheter som att den här enheten har tooreceive. Observera att i stället för att anropa hello *RegisterNativeAsync* metoden som vi kallar *RegisterTemplateAsync* tooregister för hello kategorier med hjälp av en mall för registrering. 
   
    Vi också ange ett namn för mallen hello (”simpleWNSTemplateExample”), eftersom det kan vara bra tooregister mer än en mall (till exempel en för popup-meddelanden) och en för paneler och vi behöver tooname dem i ordning toobe kan tooupdate eller tar bort dem.
   
    Observera att om en enhet registrerar flera mallar med hello samma tagg, ett inkommande meddelande som mål att taggen resulterar i flera meddelanden levereras toohello enhet (en för varje mall). Detta är användbart när hello samma logiska meddelande har tooresult i flera visuella meddelanden, till exempel visar både en Aktivitetsikon och ett popup-meddelande i en Windows Store-programmet.
   
    Mer information om mallar finns [mallar](notification-hubs-templates-cross-platform-push-messages.md).
4. Lägg till följande egenskapen toohello hello i hello projektfilen App.XAML.CS, **App** klass:
   
        public Notifications notifications = new Notifications("<hub name>", "<connection string with listen access>");
   
    Den här egenskapen är används toocreate och åtkomst till en **meddelanden** instans.
   
    Ersätt hello i hello ovan kod `<hub name>` och `<connection string with listen access>` -platshållare med notification hub namn och hello anslutningssträngen för *DefaultListenSharedAccessSignature* som du fick tidigare.
   
   > [!NOTE]
   > Eftersom autentiseringsuppgifterna som distribueras med ett klientprogram inte är vanligtvis säker kan distribuera du endast hello nyckel för lyssna åtkomst med din klientapp. Lyssna åtkomst aktiverar inte går att ändra din app tooregister för meddelanden, men befintliga registreringar och går inte att skicka meddelanden. hello fullständig åtkomstnyckel används i en skyddad backend-tjänst för att skicka meddelanden och ändra befintliga registreringar.
   > 
   > 
5. Lägg till följande rad hello i din MainPage.xaml.cs:
   
        using Windows.UI.Popups;
6. Lägg till hello följande metod i projektfilen för hello MainPage.xaml.cs:
   
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
   
    Den här metoden skapar en lista över kategorier och använder hello **meddelanden** klassen toostore hello listan i hello lokal lagring och registrera hello motsvarande taggar med meddelandehubben. När kategorier ändras, återskapas hello registrering med hello nya kategorier.

Appen är nu kan toostore en uppsättning kategorier i lokal lagring på hello enheten och registrerar med hello notification hub när hello användarändringar hello val av kategorier.

## <a name="register-for-notifications"></a>Registrera dig för meddelanden
De här stegen registrera hello meddelandehubben på Start med hello kategorier som har lagrats i lokal lagring.

> [!NOTE]
> Eftersom URI som tilldelats av hello Windows Notification Service (WNS) hello-kanalen kan ändras när som helst, bör du registrera för meddelanden ofta tooavoid meddelandet-fel. Det här exemplet registrerar för meddelande varje gång hello appen startas. För appar som körs ofta mer än en gång om dagen, du kan förmodligen hoppa över registrering toopreserve bandbredd om mindre än en dag har gått sedan hello tidigare registreringen.
> 
> 

1. Öppna hello App.xaml.cs fil- och update hello **InitNotificationsAsync** metoden toouse hello `notifications` klassen toosubscribe baserat på kategorier.
   
        // *** Remove or comment out these lines *** 
        //var channel = await PushNotificationChannelManager.CreatePushNotificationChannelForApplicationAsync();
        //var hub = new NotificationHub("your hub name", "your listen connection string");
        //var result = await hub.RegisterNativeAsync(channel.Uri);
   
        var result = await notifications.SubscribeToCategories();
   
    Detta säkerställer att varje gång hello appen startar hämtas hello kategorier från lokal lagring och begär en registrering för dessa kategorier. Hej **InitNotificationsAsync** metod skapades som en del av hello [Kom igång med Notification Hubs] [ get-started] kursen.
2. Lägg till följande kod toohello hello i hello MainPage.xaml.cs projektfilen, *OnNavigatedTo* metod:
   
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
   
    ![][19]
3. Skicka ett nytt meddelande från hello-serverdel i något av följande sätt hello:
   
   * **Konsolapp:** starta hello-konsolapp.
   * **Java/PHP:** kör din app eller skript.
     
     Meddelanden om hello valda kategorier visas som popup-meddelanden.
     
     ![][14]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen vi lärt dig hur toobroadcast senaste nytt efter kategori. Överväg att fylla i en av följande kurser som markerar andra scenarion med avancerad Meddelandehubbar hello:

* [Använda Notification Hubs toobroadcast lokaliserade senaste nyheterna]
  
    Lär dig hur lokaliserade tooexpand hello bryta nyheter app tooenable skicka meddelanden.

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
