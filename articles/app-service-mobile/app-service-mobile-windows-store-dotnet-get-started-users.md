---
title: aaaAdd authentication tooyour universella Windowsplattformen (UWP)-appen | Microsoft Docs
description: "Lär dig hur toouse Azure Apptjänst Mobilappar tooauthenticate användare i appen universella Windowsplattformen (UWP) med olika identitetsleverantörer, inklusive: AAD, Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: windows
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 6cffd951-893e-4ce5-97ac-86e3f5ad9466
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-windows
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: ad4477e9509f1c40c33e71818e268f6857fe1e80
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-windows-app"></a>Lägg till autentisering tooyour Windows-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Det här avsnittet beskrivs hur du tooadd molnbaserad autentisering tooyour mobila appar. I kursen får du lägga till autentisering toohello universella Windowsplattformen (UWP) snabbstartsprojekt för Mobilappar som använder en identitetsleverantör som stöds av Azure App Service. Efter att kunna autentiseras och auktoriseras av din mobilappsserverdel visas hello användar-ID-värdet.

Den här kursen är baserad på hello Mobile Apps Snabbstart. Du måste slutföra kursen hello [Kom igång med Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).

## <a name="register"></a>Registrera din app för autentisering och konfigurera hello Apptjänst
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL

Säker autentisering måste du definiera en ny URL-schema för din app. Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar. I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela. Du kan dock använda alla URL-schema som du väljer. Det bör vara unikt tooyour mobila program. tooenable hello omdirigering på serversidan hello:

1. I hello [Azure portal], väljer du din Apptjänst.

2. Klicka på hello **autentisering / auktorisering** menyalternativet.

3. I hello **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.  Hej **url_scheme_of_your_app** i den här strängen är hello URL-schema för din mobila program.  Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).  Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.

4. Klicka på **OK**.

5. Klicka på **Spara**.

## <a name="permissions"></a>Begränsa behörigheter tooauthenticated användare
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

Nu kan du kontrollera att anonym åtkomst tooyour backend har inaktiverats. Med hello UWP-appsprojektet som hello uppstart projekt, distribuera och köra hello app; Kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar. Detta beror på att hello app försöker tooaccess Mobile App koden som en oautentiserad användare, men hello *TodoItem* tabellen nu kräver autentisering.

Därefter uppdaterar du hello app tooauthenticate användare innan du begär resurser från din Apptjänst.

## <a name="add-authentication"></a>Lägg till autentisering toohello app
1. Filen MainPage.xaml.cs i hello UWP-appsprojektet och Lägg till följande kodfragment hello:
   
        // Define a member variable for storing hello signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs hello authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' toohello name of your MobileServiceClient instance.
                // Sign-in using Facebook authentication.
                user = await App.MobileService
                    .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                message =
                    string.Format("You are now signed in - {0}", user.UserId);
   
                success = true;
            }
            catch (InvalidOperationException)
            {
                message = "You must log in. Login Required";
            }
   
            var dialog = new MessageDialog(message);
            dialog.Commands.Add(new UICommand("OK"));
            await dialog.ShowAsync();
            return success;
        }
   
    Den här koden autentiserar hello användare med en inloggning med Facebook. Om du använder en identitetsleverantör än Facebook, ändra hello värdet på **MobileServiceAuthenticationProvider** ovan toohello värde för leverantören.
2. Ersätt hello **OnNavigatedTo()** metod i MainPage.xaml.cs. Sedan lägger du till en **inloggning** knappen toohello app som utlöser verifiering.

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. Lägg till följande kodfragment toohello MainPage.xaml.cs hello:
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login hello user and then load data from hello mobile app.
            if (await AuthenticateAsync())
            {
                // Switch hello buttons and load items from hello mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. Öppna projektfilen för hello MainPage.xaml, leta upp hello-element som definierar hello **spara** knappen och Ersätt den med hello följande kod:
   
        <Button Name="ButtonSave" Visibility="Collapsed" Margin="0,8,8,0" 
                Click="ButtonSave_Click">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Add"/>
                <TextBlock Margin="5">Save</TextBlock>
            </StackPanel>
        </Button>
        <Button Name="ButtonLogin" Visibility="Visible" Margin="0,8,8,0" 
                Click="ButtonLogin_Click" TabIndex="0">
            <StackPanel Orientation="Horizontal">
                <SymbolIcon Symbol="Permissions"/>
                <TextBlock Margin="5">Sign in</TextBlock> 
            </StackPanel>
        </Button>
5. Lägg till följande kodfragment toohello App.xaml.cs hello:

        protected override void OnActivated(IActivatedEventArgs args)
        {
            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                Frame content = Window.Current.Content as Frame;
                if (content.Content.GetType() == typeof(MainPage))
                {
                    content.Navigate(typeof(MainPage), protocolArgs.Uri);
                }
            }
            Window.Current.Activate();
            base.OnActivated(args);
        }
6. Öppna filen Package.appxmanifest, navigera för**deklarationer**i **tillgängliga deklarationer** listrutan, Välj **protokollet** och klicka på **Lägg till** knappen. Nu konfigurera hello **egenskaper** av hello **protokollet** deklaration. I **visningsnamn**, lägga till hello namn som du vill toodisplay toousers för programmet. I **namn**, lägga till {url_scheme_of_your_app}.
7. Tryck på hello F5 viktiga toorun hello appen klickar du på hello **inloggning** knappen och logga in på hello appen med din valda identitetsprovider. När inloggningen är klar, hello appen körs utan fel och du kan tooquery din serverdel och göra uppdateringar toodata.

## <a name="tokens"></a>Lagra hello autentiseringstoken på hello-klient
hello föregående exempel visade en standard inloggning, vilket kräver hello klienten toocontact båda hello identitetsleverantör och hello Apptjänst varje gång hello appen startas. Är inte bara den här metoden ineffektiv, kan du köra till användning-relaterar problem många kunder försök toostart app på hello samtidigt. Är det bättre toocache hello autentiseringstoken returneras av din App Service och försök toouse detta innan du börjar använda en provider-baserad inloggning.

> [!NOTE]
> Du kan cachelagra hello-token som utfärdas av Apptjänster oavsett om du använder klient-hanteras eller service autentisering. Den här kursen använder autentisering som hanteras av tjänsten.
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a>Nästa steg
Nu när du har slutfört den här självstudiekursen för grundläggande autentisering, Överväg fortsätter tooone av hello följande kurser:

* [Lägg till push-meddelanden tooyour app](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobilapp backend toouse Azure Notification Hubs toosend push-meddelanden.
* [Aktivera offlinesynkronisering av appen](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel. Offlinesynkronisering kan slutanvändarna toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
