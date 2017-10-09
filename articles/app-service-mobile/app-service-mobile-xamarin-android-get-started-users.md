---
title: "aaaGet igång med Mobile Apps i Xamarin Android-autentisering"
description: "Lär dig hur toouse Mobile Apps tooauthenticate användare av Xamarin Android-app via en mängd olika identitetsleverantörer, inklusive AAD, Google, Facebook, Twitter och Microsoft."
services: app-service\mobile
documentationcenter: xamarin
author: ggailey777
manager: panarasi
editor: 
ms.assetid: 570fc12b-46a9-4722-b2e0-0d1c45fb2152
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: mobile-xamarin-android
ms.devlang: dotnet
ms.topic: article
ms.date: 07/05/2017
ms.author: panarasi
ms.openlocfilehash: 500a4efa816e4f6d75d359e31d6357da56a72f6e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-authentication-tooyour-xamarinandroid-app"></a>Lägg till autentisering tooyour Xamarin.Android-app
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

Det här avsnittet beskrivs hur du tooauthenticate användare av en Mobilapp från klientprogrammet. Lägg till autentisering toohello snabbstartsprojekt en identitetsleverantör som stöds av Azure Mobile Apps i den här självstudiekursen. Efter att kunna autentiseras och auktoriseras i hello Mobilapp visas hello användar-ID-värdet.

Den här kursen är baserad på hello Mobilapp Snabbstart. Du måste också slutföra kursen hello [skapa en Xamarin.Android-app]. Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello autentisering tillägget paketet tooyour projekt. Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).

## <a name="register"></a>Registrera din app för autentisering och konfigurera Apptjänster
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

Kör hello klientprojekt i Visual Studio eller Xamarin Studio på en enhet eller emulator. Kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar. Detta beror på att hello app försöker tooaccess serverdelen för Mobilappen som en oautentiserad användare. Hej *TodoItem* tabellen nu kräver autentisering.

Därefter uppdaterar du hello klienten app toorequest resurser från hello mobilappsserverdel med en autentiserad användare.

## <a name="add-authentication"></a>Lägg till autentisering toohello app
hello appen är uppdaterade toorequire användare tootap hello **inloggning** knappen och autentisera innan data visas.

1. Lägg till följande kod toohello hello **TodoActivity** klass:
   
        // Define a authenticated user.
        private MobileServiceUser user;
        private async Task<bool> Authenticate()
        {
                var success = false;
                try
                {
                    // Sign in with Facebook login using a server-managed flow.
                    user = await client.LoginAsync(this,
                        MobileServiceAuthenticationProvider.Facebook, "{url_scheme_of_your_app}");
                    CreateAndShowDialog(string.Format("you are now logged in - {0}",
                        user.UserId), "Logged in!");
   
                    success = true;
                }
                catch (Exception ex)
                {
                    CreateAndShowDialog(ex, "Authentication failed");
                }
                return success;
        }
   
        [Java.Interop.Export()]
        public async void LoginUser(View view)
        {
            // Load data only after authentication succeeds.
            if (await Authenticate())
            {
                //Hide hello button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load hello data.
                OnRefreshItemsSelected();
            }
        }
   
    Detta skapar en ny metod tooauthenticate en användare och en metod hanterare för en ny **inloggning** knappen. hello användaren i hello exempelkod ovan autentiseras med hjälp av en inloggning med Facebook. En dialogruta är används toodisplay hello användar-ID som autentiseras en gång.
   
   > [!NOTE]
   > Om du använder en identitetsleverantör än Facebook ändrar hello-värdet som skickades för**LoginAsync** ovan tooone av hello följande: *MicrosoftAccount*, *Twitter*, *Google*, eller *WindowsAzureActiveDirectory*.
   > 
   > 
2. I hello **OnCreate** metod-, delete- eller kommenterar ut hello följande kodrad:
   
        OnRefreshItemsSelected ();
3. Lägg till följande hello i hello Activity_To_Do.axml filen *LoginUser* knappen definitionen innan hello befintliga *AddItem* knappen:
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. Lägg till följande element toohello Strings.xml resursfilen hello:
   
        <string name="login_button_text">Sign in</string>
5. Öppna hello AndroidManifest.xml filen, lägga till följande kod i hello `<application>` XML-elementet:

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. I Visual Studio eller Xamarin Studio kör hello klientprojektet på en enhet eller emulator och logga in med ditt valda identitetsleverantör. När du har loggat in, hello visas inloggnings-ID och hello listan över todo-objekt, och du kan göra uppdateringar toohello data.

<!-- URLs. -->
[skapa en Xamarin.Android-app]: app-service-mobile-xamarin-android-get-started.md