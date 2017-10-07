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
# <a name="azure-notification-hubs-notify-users-with-net-backend"></a>Azure Notification Hubs Meddelandeanvändare med .NET-serverdel
[!INCLUDE [notification-hubs-selector-aspnet-backend-notify-users](../../includes/notification-hubs-selector-aspnet-backend-notify-users.md)]

## <a name="overview"></a>Översikt
Stöd för push-meddelanden i Azure kan du tooaccess ett enkelt att använda och multiplatform skalats ut push-infrastruktur, vilket förenklar hello implementering av push-meddelanden för konsument- och enterprise-program för mobila enheter plattformar. Den här kursen visar hur toouse Azure Notification Hubs toosend push-meddelanden tooa specifik app användare på en specifik enhet. En ASP.NET-WebAPI-serverdel är används tooauthenticate klienter. Hello autentiserade klientanvändare och tagg läggas automatiskt till av hello backend toonotification registrering. Den här taggen kommer att använda toosend av hello backend toogenerate meddelanden för en viss användare. Mer information om registrering för meddelanden med hjälp av en appserverdel avsnittet hello vägledning [registrering från din Apps serverdel](http://msdn.microsoft.com/library/dn743807.aspx). Den här kursen bygger på hello meddelandehubb och projekt som du skapade i hello [Kom igång med Notification Hubs] kursen.

Den här kursen är också hello nödvändiga toohello [Secure Push] kursen. När du har slutfört hello stegen i den här självstudiekursen, kan du fortsätta toohello [Secure Push] kursen visar hur toomodify hello kod i den här självstudiekursen toosend ett push-meddelande på ett säkert sätt.

## <a name="before-you-begin"></a>Innan du börjar
Vi tar din feedback på allvar. Om du har slutfört det här ämnet, eller för att förbättra det här innehållet problem, skulle vi uppskattar din feedback på hello hello sidans nederkant.

hello slutförts koden för den här kursen finns på GitHub [här](https://github.com/Azure/azure-notificationhubs-samples/tree/master/dotnet/NotifyUsers). 

## <a name="prerequisites"></a>Krav
Innan du börjar den här självstudiekursen, måste du redan har slutfört de här kurserna i Mobile Services:

* [Kom igång med Notification Hubs]<br/>Du skapar din meddelandehubb och reservera appnamn hälsningspaket och registrera tooreceive meddelanden i den här kursen. Den här kursen förutsätter att du redan har slutfört de här stegen. Annars följer du stegen hello i [komma igång med Notification Hubs (Windows Store)](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md); specifikt hello avsnitt [registrera din app för Windows Store för hello](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#register-your-app-for-the-windows-store) och [konfigurera din Meddelandehubb](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Särskilt, se till att du har angett hello **paket-SID** och **Klienthemlighet** värden i hello-portalen i hello **konfigurera** för meddelandehubben. Den här proceduren konfiguration beskrivs i hello [konfigurera din Meddelandehubb](notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md#configure-your-notification-hub). Detta är ett viktigt steg: om hello autentiseringsuppgifter på hello portal inte matchar dem som specificerats för hello appnamn du väljer, hello push-meddelande kommer att misslyckas.

> [!NOTE]
> Om du använder Mobile Apps i Azure App Service som backend-tjänst finns hello [Mobile Apps version](../app-service-mobile/app-service-mobile-windows-store-dotnet-get-started-push.md) i den här kursen.
> 
> 

&nbsp;

[!INCLUDE [notification-hubs-aspnet-backend-notifyusers](../../includes/notification-hubs-aspnet-backend-notifyusers.md)]

## <a name="update-hello-code-for-hello-client-project"></a>Uppdatera hello kod för hello klientprojektet
I det här avsnittet kan du uppdatera hello koden i hello-projekt som du har slutfört för hello [Kom igång med Notification Hubs] kursen. hello bör redan associerad med hello store och konfigurerats för meddelandehubben. I det här avsnittet ska du lägga till kod-toocall hello nya WebAPI-serverdel och använda den för registrering och skicka meddelanden.

1. Öppna i Visual Studio hello hello lösningen som du skapade för hello [Kom igång med Notification Hubs] kursen.
2. I Solution Explorer högerklickar du på hello **(Windows 8.1)** projektet och klicka sedan på **hantera NuGet-paket**.
3. Hello vänster, klickar du på **Online**.
4. I hello **Sök** skriver **http-klienten**.
5. I listan över sökresultat hello klickar du på **klientbibliotek för Microsoft HTTP**, och klicka sedan på **installera**. Slutför hello-installationen.
6. Tillbaka i hello NuGet **Sök** skriver **Json.net**. Installera hello **Json.NET** paket- och stänger sedan hello NuGet Package Manager-fönstret.
7. Upprepa hello stegen ovan för hello **(Windows Phone 8.1)** projekt tooinstall hello **JSON.NET** NuGet-paketet för hello Windows Phone-projekt.
8. I Solution Explorer i hello **(Windows 8.1)** projektet genom att dubbelklicka på **MainPage.xaml** tooopen i hello Visual Studio-redigeraren.
9. I hello **MainPage.xaml** XML-kod, Ersätt hello `<Grid>` avsnitt med hello följande kod. Den här koden lägger till en textruta för användarnamn och lösenord som hello användaren autentiseras med. Det ger också textrutor för hello-meddelande och hello username-tagg som hello-meddelande:
   
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
10. I Solution Explorer i hello **(Windows Phone 8.1)** projektet öppnar **MainPage.xaml** och Ersätt hello Windows Phone 8.1 `<Grid>` avsnitt med samma koden ovan. hello-gränssnittet ska se ut ungefär toowhats visas nedan.
    
    ![][13]
11. I Solution Explorer öppnar du hello **MainPage.xaml.cs** -filen för hello **(Windows 8.1)** och **(Windows Phone 8.1)** projekt. Lägg till följande hello `using` instruktioner överst hello i båda filerna:
    
        using System.Net.Http;
        using Windows.Storage;
        using System.Net.Http.Headers;
        using Windows.Networking.PushNotifications;
        using Windows.UI.Popups;
        using System.Threading.Tasks;
12. I **MainPage.xaml.cs** för hello **(Windows 8.1)** och **(Windows Phone 8.1)** projekt, Lägg till följande medlem toohello hello `MainPage` klass. Vara säker på att tooreplace `<Enter Your Backend Endpoint>` med hello faktiska backend-slutpunkt hämtats tidigare. Till exempel `http://mybackend.azurewebsites.net`.
    
        private static string BACKEND_ENDPOINT = "<Enter Your Backend Endpoint>";
13. Lägg till hello kod nedan toohello MainPage klassen i **MainPage.xaml.cs** för hello **(Windows 8.1)** och **(Windows Phone 8.1)** projekt.
    
    Hej `PushClick` metoden är hello klickar du på hanterare för hello **skicka Push-** knappen. Anropar hello backend tootrigger ett meddelande tooall enheter med en tagg för användarnamn som matchar hello `to_tag` parameter. hello-meddelande skickas som JSON-innehåll i hello frågans brödtext.
    
    Hej `LoginAndRegisterClick` metoden är hello klickar du på hanterare för hello **logga in och registrera** knappen. Lagrar den grundläggande hello autentiseringstoken i lokal lagring (Observera att detta motsvarar alla token som använder din autentiseringsschema), använder sedan `RegisterClient` tooregister för meddelanden med hello backend.

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



1. I Solution Explorer under hello **delade** projekt, öppna hello **App.xaml.cs** fil. Hitta hello anrop för`InitNotificationsAsync()` i hello `OnLaunched()` händelsehanterare. Kommentera ut eller ta bort hello anrop för`InitNotificationsAsync()`. hello knappen hanteraren lades till ovan ska initiera notification registreringar.

        protected override void OnLaunched(LaunchActivatedEventArgs e)
        {
            //InitNotificationsAsync();


1. I Solution Explorer högerklickar du på hello **delade** projektet och klicka sedan på **Lägg till**, och klicka sedan på **klassen**. Namnge hello klassen **RegisterClient.cs**, klicka på **OK** toogenerate hello klass.
   
   Den här klassen radbryts hello REST-anrop krävs toocontact hello app backend i ordning tooregister för push-meddelanden. Den lagrar även lokalt hello *registrationIds* skapas av hello Notification Hub enligt anvisningarna i [registrering från din Apps serverdel](http://msdn.microsoft.com/library/dn743807.aspx). Observera att den använder en autentiseringstoken som lagras i lokal lagring när du klickar på hello **logga in och registrera** knappen.
2. Lägg till följande hello `using` instruktioner överst hello i hello RegisterClient.cs fil:
   
       using Windows.Storage;
       using System.Net;
       using System.Net.Http;
       using System.Net.Http.Headers;
       using Newtonsoft.Json;
       using System.Threading.Tasks;
       using System.Linq;
3. Lägg till följande kod i hello hello `RegisterClient` klassen definition.
   
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
4. Spara alla ändringar.

## <a name="testing-hello-application"></a>Testa hello program
1. Starta hello program både på Windows 8.1 och Windows Phone 8.1. Du kan köra hello instans i hello emulator eller en enhet för Windows Phone 8.1.
2. Ange i hello Windows 8.1 instans av hello app, en **användarnamn** och **lösenord** enligt hello-skärmen nedan. Den måste skilja sig från hello användarnamn och lösenord som du anger på Windows Phone.
3. Klicka på **logga in och registrera** och verifiera en dialogruta visar att du har loggat in. Detta aktiverar också hello **skicka Push-** knappen.
   
    ![][14]
4. Ange en användare namnsträngen på hello Windows Phone 8.1-instansen i både hello **användarnamn** och **lösenord** fält klicka **inloggning och registrera**.
5. I hello **mottagaren användarnamn taggen** anger hello användarnamn som är registrerad på Windows 8.1. Ange ett meddelande och klicka på **skicka Push-**.
   
    ![][16]
6. Endast hello-enheter som har registrerats med hello matchande användarnamn tagg får hello-meddelande.
   
    ![][15]

## <a name="next-steps"></a>Nästa steg
* Om du vill toosegment in användarna efter intressegrupper, se [använda Notification Hubs toosend senaste nytt].
* Mer om hur toolearn toouse Notification Hubs finns [riktlinjer om Notification Hubs].

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
