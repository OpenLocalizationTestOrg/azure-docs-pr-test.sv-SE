---
title: "aaaGet igång med autentisering för Mobile Apps i Xamarin Forms app | Microsoft Docs"
description: "Lär dig hur toouse Mobile Apps tooauthenticate användare i Xamarin Forms appen via en mängd olika identitetsleverantörer, inklusive AAD, Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: panarasi
manager: syntaxc4
editor: 
ms.assetid: 9c55e192-c761-4ff2-8d88-72260e9f6179
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin
ms.devlang: dotnet
ms.topic: article
ms.date: 08/07/2017
ms.author: panarasi
ms.openlocfilehash: 7f6716619f33d9cc4f866c41effba8f048dc49fa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarin-forms-app"></a>Lägg till autentisering tooyour Xamarin Forms app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a>Översikt
Det här avsnittet beskrivs hur du tooauthenticate användare av en Apptjänst Mobile App från klientprogrammet. I kursen får du lägger till autentisering hello Xamarin Forms snabbstartsprojekt en identitetsleverantör som stöds av App Service. Efter att har autentiseras och auktoriseras av din Mobilapp hello användar-ID-värde visas och du kommer att kunna tooaccess begränsad tabelldata.

## <a name="prerequisites"></a>Krav
För hello bästa resultat med den här självstudiekursen, rekommenderar vi att du först slutföra hello [skapa en app i Xamarin Forms] [ 1] kursen. När den här kursen har du en Xamarin Forms-projekt som är en app för flera plattformar TodoList.

Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello autentisering tillägget paketet tooyour projekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][2].

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrera din app för autentisering och konfigurera Apptjänster
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL

Säker autentisering måste du definiera en ny URL-schema för din app. Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar. I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela. Du kan dock använda alla URL-schema som du väljer. Det bör vara unikt tooyour mobila program. tooenable hello omdirigering på serversidan hello:

1. I hello [Azure portal], väljer du din Apptjänst.

2. Klicka på hello **autentisering / auktorisering** menyalternativet.

3. I hello **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.  Hej **url_scheme_of_your_app** i den här strängen är hello URL-schema för din mobila program.  Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).  Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.

4. Klicka på **OK**.

5. Klicka på **Spara**.

## <a name="restrict-permissions-tooauthenticated-users"></a>Begränsa behörigheter tooauthenticated användare
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-toohello-portable-class-library"></a>Lägg till autentisering toohello bärbar klassbiblioteket
Mobile Apps använder hello [LoginAsync] [ 3] metod på hello [MobileServiceClient] [ 4] toosign i en användare med App Service autentisering. Det här exemplet används en hanterad server autentiseringsflödet som visar hello provider i inloggningsgränssnittet i hello app. Mer information finns i [Server-hanterade autentisering][5]. För att ge en bättre användarupplevelse i din produktionsapp, bör du istället använda [klienten hanteras autentisering][6].

tooauthenticate med Xamarin Forms projekt definiera en **IAuthenticate** hello bärbara Class Library för hello app-gränssnitt. Lägg sedan till en **inloggning** knappen toohello användargränssnitt som definierats i hello bärbara klassbiblioteket, som du klickar på toostart autentisering. Data läses från hello mobilappsserverdel efter en lyckad autentisering.

Implementera hello **IAuthenticate** gränssnitt för varje plattform som stöds av din app.

1. Öppna i Visual Studio eller Xamarin Studio App.cs från hello-projekt med **bärbar** i hello namn som är bärbara klassbiblioteket projektet, och Lägg sedan till följande hello `using` instruktionen:

        using System.Threading.Tasks;
2. Lägg till följande hello i App.cs, `IAuthenticate` gränssnitt definition omedelbart före hello `App` klassen definition.

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. tooinitialize hello gränssnitt med en plattformsspecifik-implementering, lägga till hello efter statiska medlemmarna toohello **App** klass.

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. Öppna TodoList.xaml från hello bärbara klassbiblioteket projekt, Lägg till följande hello **knappen** element i hello *buttonsPanel* layout-elementet efter befintlig hello-knapp:

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    Den här knappen utlöser server-hanterade autentisering med mobilappsserverdelen.
5. Öppna TodoList.xaml.cs från hello bärbara klassbiblioteket projekt och lägger till följande fält toohello hello `TodoList` klass:

        // Track whether hello user has authenticated.
        bool authenticated = false;
6. Ersätt hello **OnAppearing** metod med hello följande kod:

        protected override async void OnAppearing()
        {
            base.OnAppearing();

            // Refresh items only when authenticated.
            if (authenticated == true)
            {
                // Set syncItems tootrue in order toosynchronize hello data
                // on startup when running in offline mode.
                await RefreshItems(true, syncItems: false);

                // Hide hello Sign-in button.
                this.loginButton.IsVisible = false;
            }
        }

    Den här koden ser till att data uppdateras bara från hello-tjänsten när du har autentiserats.
7. Lägg till följande hanterare för hello hello **Clicked** händelse toohello **TodoList** klass:

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems tootrue toosynchronize hello data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. Spara dina ändringar och återskapa hello bärbara klassbiblioteket projektet verifierar utan fel.

## <a name="add-authentication-toohello-android-app"></a>Lägg till autentisering toohello Android-app
Det här avsnittet visas hur tooimplement hello **IAuthenticate** gränssnitt i hello Android-app-projekt. Hoppa över det här avsnittet om du inte stöder Android-enheter.

1. Högerklicka i Visual Studio eller Xamarin Studio hello **droid** projektet sedan **Set as StartUp Project**.
2. Tryck på F5 toostart hello projekt i hello felsökning och sedan kontrollera att ett undantag med en statuskod 401 (obehörig) aktiveras när appen startar. hello 401 kod skapas eftersom åtkomst på hello serverdel är endast begränsad tooauthorized användare.
3. Öppna MainActivity.cs hello Android-projekt och lägga till följande hello `using` instruktioner:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Uppdatera hello **MainActivity** klassen tooimplement hello **IAuthenticate** gränssnitt, enligt följande:

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. Uppdatera hello **MainActivity** klassen genom att lägga till en **MobileServiceUser** fältet och en **autentisera** metod, vilket krävs av hello **IAuthenticate**  gränssnitt, enligt följande:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                user = await TodoItemManager.DefaultManager.CurrentClient.LoginAsync(this, 
                    MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                if (user != null)
                {
                    message = string.Format("you are now signed-in as {0}.",
                        user.UserId);
                    success = true;
                }
            }
            catch (Exception ex)
            {
                message = ex.Message;
            }

            // Display hello success or failure message.
            AlertDialog.Builder builder = new AlertDialog.Builder(this);
            builder.SetMessage(message);
            builder.SetTitle("Sign-in result");
            builder.Create().Show();

            return success;
        }

    Om du använder en identitetsleverantör än Facebook, välja ett annat värde för [MobileServiceAuthenticationProvider][7].

6. Lägg till följande kod i hello <application> nod i AndroidManifest.xml:

```xml
    <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
      <intent-filter>
        <action android:name="android.intent.action.VIEW" />
        <category android:name="android.intent.category.DEFAULT" />
        <category android:name="android.intent.category.BROWSABLE" />
        <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
      </intent-filter>
    </activity>
```

1. Lägg till följande kod toohello hello **OnCreate** metod för hello **MainActivity** klassen före hello anrop för`LoadApplication()`:

        // Initialize hello authenticator before loading hello app.
        App.Init((IAuthenticate)this);

    Den här koden garanterar hello authenticator är initierad innan hello app belastning.
2. Återskapa hello app, köra den och sedan logga in med hello autentiseringsprovider du har valt och kontrollera att du ska kunna tooaccess data som en autentiserad användare.

## <a name="add-authentication-toohello-ios-app"></a>Lägg till autentisering toohello iOS-app
Det här avsnittet visas hur tooimplement hello **IAuthenticate** gränssnittet i hello iOS-app-projekt. Hoppa över det här avsnittet om du inte stöder iOS-enheter.

1. Högerklicka i Visual Studio eller Xamarin Studio hello **iOS** projektet sedan **Set as StartUp Project**.
2. Tryck på F5 toostart hello projekt i hello felsökning och sedan kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar. hello 401 svar produceras eftersom åtkomst på hello serverdel är endast begränsad tooauthorized användare.
3. Öppna AppDelegate.cs i hello iOS-projektet och Lägg till följande hello `using` instruktioner:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. Uppdatera hello **AppDelegate** klassen tooimplement hello **IAuthenticate** gränssnitt, enligt följande:

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. Uppdatera hello **AppDelegate** klassen genom att lägga till en **MobileServiceUser** fältet och en **autentisera** metod, vilket krävs av hello **IAuthenticate**  gränssnitt, enligt följande:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            var success = false;
            var message = string.Empty;
            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(UIApplication.SharedApplication.KeyWindow.RootViewController,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                        success = true;
                    }
                }
            }
            catch (Exception ex)
            {
               message = ex.Message;
            }

            // Display hello success or failure message.
            UIAlertView avAlert = new UIAlertView("Sign-in result", message, null, "OK", null);
            avAlert.Show();

            return success;
        }

    Välj ett annat värde för [MobileServiceAuthenticationProvider] om du använder en identitetsleverantör än Facebook.

6. Uppdatera hello AppDelegate klassen genom att lägga till metodöverlagringen OpenUrl (UIApplication app NSUrl url NSDictionary alternativ)

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. Lägg till följande rad med kod toohello hello **FinishedLaunching** innan hello anrop för`LoadApplication()`:

        App.Init(this);

    Den här koden garanterar hello authenticator initieras innan hello app har lästs in.

6. Lägg till **{url_scheme_of_your_app}** tooURL scheman i Info.plist.

7. Återskapa hello app, köra den och sedan logga in med hello autentiseringsprovider du har valt och kontrollera att du ska kunna tooaccess data som en autentiserad användare.

## <a name="add-authentication-toowindows-10-including-phone-app-projects"></a>Lägg till autentisering tooWindows 10 (inklusive Phone) app-projekt
Det här avsnittet visas hur tooimplement hello **IAuthenticate** gränssnittet i hello Windows 10-app-projekt. hello samma steg gäller för universella Windowsplattformen (UWP) projekt, men använder hello **UWP** projekt (med noterats ändringar). Hoppa över det här avsnittet om du inte stöder Windows-enheter.

1. ”I Visual Studio högerklickar du på antingen hello **UWP** projektet sedan **Set as StartUp Project**.
2. Tryck på F5 toostart hello projekt i hello felsökning och sedan kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar. hello 401-svar som beror på att åtkomst på hello serverdel är endast begränsad tooauthorized användare.
3. Öppna MainPage.xaml.cs för Windows hello för app-projekt och Lägg till följande hello `using` instruktioner:

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    Ersätt `<your_Portable_Class_Library_namespace>` med hello namnområde för din bärbara klassbiblioteket.
4. Uppdatera hello **MainPage** klassen tooimplement hello **IAuthenticate** gränssnitt, enligt följande:

        public sealed partial class MainPage : IAuthenticate
5. Uppdatera hello **MainPage** klassen genom att lägga till en **MobileServiceUser** fältet och en **autentisera** metod, vilket krävs av hello **IAuthenticate** gränssnitt, enligt följande:

        // Define a authenticated user.
        private MobileServiceUser user;

        public async Task<bool> Authenticate()
        {
            string message = string.Empty;
            var success = false;

            try
            {
                // Sign in with Facebook login using a server-managed flow.
                if (user == null)
                {
                    user = await TodoItemManager.DefaultManager.CurrentClient
                        .LoginAsync(MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    if (user != null)
                    {
                        success = true;
                        message = string.Format("You are now signed-in as {0}.", user.UserId);
                    }
                }

            }
            catch (Exception ex)
            {
                message = string.Format("Authentication Failed: {0}", ex.Message);
            }

            // Display hello success or failure message.
            await new MessageDialog(message, "Sign-in result").ShowAsync();

            return success;
        }

    Välj ett annat värde för [MobileServiceAuthenticationProvider] om du använder en identitetsleverantör än Facebook.

1. Lägg till följande kodrad i hello konstruktorn för hello hello **MainPage** klassen före hello anrop för`LoadApplication()`:

        // Initialize hello authenticator before loading hello app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    Ersätt `<your_Portable_Class_Library_namespace>` med hello namnområde för din bärbara klassbiblioteket.

3. Om du använder **UWP**, Lägg till följande hello **OnActivated** metod åsidosätter toohello **App** klass:

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   Lägg till hello villkorlig koden från hello föregående kodfragment när hello metodersättningen redan finns.  Den här koden krävs inte för universella Windows-projekt.

3. Lägg till **{url_scheme_of_your_app}** i Package.appxmanifest. 

4. Återskapa hello app, köra den och sedan logga in med hello autentiseringsprovider du har valt och kontrollera att du ska kunna tooaccess data som en autentiserad användare.

## <a name="next-steps"></a>Nästa steg
Nu när du har slutfört den här självstudiekursen för grundläggande autentisering, Överväg fortsätter tooone av hello följande kurser:

* [Lägg till push-meddelanden tooyour app](app-service-mobile-xamarin-forms-get-started-push.md)

  Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobilapp backend toouse Azure Notification Hubs toosend push-meddelanden.
* [Aktivera offlinesynkronisering av appen](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel. Offlinesynkronisering kan slutet användare toointeract med en mobil app - visa, lägga till eller ändra data – även om det inte finns någon nätverksanslutning.

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
