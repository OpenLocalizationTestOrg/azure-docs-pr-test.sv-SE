---
title: "aaaGet igång med autentisering för Mobile Apps i Xamarin-iOS"
description: "Lär dig hur toouse Mobile Apps tooauthenticate användare av Xamarin iOS-app via en mängd olika identitetsleverantörer, inklusive AAD, Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: syntaxc4
editor: 
ms.assetid: 180cc61b-19c5-48bf-a16c-7181aef3eacc
ms.service: app-service-mobile
ms.workload: na
ms.tgt_pltfrm: mobile-xamarin-ios
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: glenga
ms.openlocfilehash: 6458e9651b03df61c86b88b11953792e04bfa5b2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarinios-app"></a>Lägg till autentisering tooyour Xamarin.iOS-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Det här avsnittet beskrivs hur du tooauthenticate användare av en Apptjänst Mobile App från klientprogrammet. Lägg till autentisering toohello Xamarin.iOS snabbstartsprojekt en identitetsleverantör som stöds av Apptjänst i den här självstudiekursen. Efter att har autentiseras och auktoriseras av din Mobilapp hello användar-ID-värde visas och du kommer att kunna tooaccess begränsad tabelldata.

Du måste slutföra kursen hello [skapa en Xamarin.iOS-app]. Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello autentisering tillägget paketet tooyour projekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register-your-app-for-authentication-and-configure-app-services"></a>Registrera din app för autentisering och konfigurera Apptjänster
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-toohello-allowed-external-redirect-urls"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL

Säker autentisering måste du definiera en ny URL-schema för din app. Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar. I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela. Du kan dock använda alla URL-schema som du väljer. Det bör vara unikt tooyour mobila program. tooenable hello omdirigering på serversidan hello:

1. I hello [Azure portal], väljer du din Apptjänst.

2. Klicka på hello **autentisering / auktorisering** menyalternativet.

3. I hello **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.  Hej **url_scheme_of_your_app** i den här strängen är hello URL-schema för din mobila program.  Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).  Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.

4. Klicka på **OK**.

5. Klicka på **Spara**.

## <a name="restrict-permissions-tooauthenticated-users"></a>Begränsa behörigheter tooauthenticated användare
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

&nbsp;&nbsp;4. Kör hello klientprojekt i Visual Studio eller Xamarin Studio på en enhet eller emulator. Kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar. hello misslyckade är loggade toohello konsol hello felsökare. Därför bör du se hello fel i utdatafönstret hello i Visual Studio.

&nbsp;&nbsp;Felet obehörig beror hello app försöker tooaccess serverdelen för Mobilappen som en oautentiserad användare. Hej *TodoItem* tabellen nu kräver autentisering.

Därefter uppdaterar du hello klienten app toorequest resurser från hello mobilappsserverdel med en autentiserad användare.

## <a name="add-authentication-toohello-app"></a>Lägg till autentisering toohello app
I det här avsnittet ska du ändra hello app toodisplay en inloggningsskärm innan data. När hello app startar inte ansluter tooyour Apptjänst inte och visas inte några data. Efter hello första gången utför hello användaren hello uppdatera gest hello inloggningsskärmen visas. efter genomförd inloggning ska hello lista över todo-objekt visas.

1. Öppna hello-filen i hello klientprojektet **QSTodoService.cs** och Lägg till följande hello-instruktion och `MobileServiceUser` med accessor toohello QSTodoService klass:
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. Lägga till nya metod med namnet **autentisera** för**QSTodoService** med hello definitionen:

        public async Task Authenticate(UIViewController view)
        {
            try
            {
                AppDelegate.ResumeWithURL = url => url.Scheme == "zumoe2etestapp" && client.ResumeWithURL(url);
                user = await client.LoginAsync(view, MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
            }
            catch (Exception ex)
            {
                Console.Error.WriteLine (@"ERROR - AUTHENTICATION FAILED {0}", ex.Message);
            }
        }

    >[AZURE.NOTE] Om du använder en identitetsleverantör än en Facebook ändrar hello-värdet som skickades för**LoginAsync** ovan tooone av hello följande: _MicrosoftAccount_, _Twitter_, _Google_, eller _WindowsAzureActiveDirectory_.

3. Öppna **QSTodoListViewController.cs**. Ändra hello metoddefinition av **ViewDidLoad** tar bort hello anrop för**RefreshAsync()** nära hello slut:
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out hello call tooRefreshAsync
            // await RefreshAsync();
        }
4. Ändra hello metoden **RefreshAsync** tooauthenticate om hello **användaren** -egenskapen är null. Lägg till följande kod hello överst i hello metoddefinition hello:
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. Öppna **AppDelegate.cs**, Lägg till följande metod hello:

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. Öppna **Info.plist** fil, navigera för**URL typer** i hello **Avancerat** avsnitt. Nu konfigurera hello **identifierare** och hello **URL-scheman** URL-typen och klicka på **Lägg till URL-typen**. **URL-scheman** ska vara hello samma som {url_scheme_of_your_app}.
7. I Visual Studio eller Xamarin Studio anslutna tooyour Xamarin skapa värden på en Mac som kör hello klientprojektet inriktning på en enhet eller emulator. Kontrollera appen hello visar inga data.
   
    Utföra hello uppdatera gest genom att dra nedåt hello listan över objekt, vilket leder till hello inloggningen skärmen tooappear. När du har angett giltiga autentiseringsuppgifter, hello app visar hello lista över todo-objekt och du kan göra uppdateringar toohello data.

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[skapa en Xamarin.iOS-app]: app-service-mobile-xamarin-ios-get-started.md