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
# <a name="add-authentication-tooyour-xamarinandroid-app"></a><span data-ttu-id="cde17-103">Lägg till autentisering tooyour Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="cde17-103">Add authentication tooyour Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="cde17-104">Det här avsnittet beskrivs hur du tooauthenticate användare av en Mobilapp från klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="cde17-104">This topic shows you how tooauthenticate users of a Mobile App from your client application.</span></span> <span data-ttu-id="cde17-105">Lägg till autentisering toohello snabbstartsprojekt en identitetsleverantör som stöds av Azure Mobile Apps i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="cde17-105">In this tutorial, you add authentication toohello quickstart project using an identity provider that is supported by Azure Mobile Apps.</span></span> <span data-ttu-id="cde17-106">Efter att kunna autentiseras och auktoriseras i hello Mobilapp visas hello användar-ID-värdet.</span><span class="sxs-lookup"><span data-stu-id="cde17-106">After being successfully authenticated and authorized in hello Mobile App, hello user ID value is displayed.</span></span>

<span data-ttu-id="cde17-107">Den här kursen är baserad på hello Mobilapp Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="cde17-107">This tutorial is based on hello Mobile App quickstart.</span></span> <span data-ttu-id="cde17-108">Du måste också slutföra kursen hello [skapa en Xamarin.Android-app].</span><span class="sxs-lookup"><span data-stu-id="cde17-108">You must also first complete hello tutorial [Create a Xamarin.Android app].</span></span> <span data-ttu-id="cde17-109">Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello autentisering tillägget paketet tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="cde17-109">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="cde17-110">Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="cde17-110">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <span data-ttu-id="cde17-111"><a name="register"></a>Registrera din app för autentisering och konfigurera Apptjänster</span><span class="sxs-lookup"><span data-stu-id="cde17-111"><a name="register"></a>Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="cde17-112"><a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="cde17-112"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="cde17-113">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="cde17-113">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="cde17-114">Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="cde17-114">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="cde17-115">I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="cde17-115">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="cde17-116">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="cde17-116">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="cde17-117">Det bör vara unikt tooyour mobila program.</span><span class="sxs-lookup"><span data-stu-id="cde17-117">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="cde17-118">tooenable hello omdirigering på serversidan hello:</span><span class="sxs-lookup"><span data-stu-id="cde17-118">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="cde17-119">I hello [Azure portal], väljer du din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="cde17-119">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="cde17-120">Klicka på hello **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="cde17-120">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="cde17-121">I hello **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="cde17-121">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="cde17-122">Hej **url_scheme_of_your_app** i den här strängen är hello URL-schema för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="cde17-122">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="cde17-123">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="cde17-123">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="cde17-124">Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="cde17-124">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="cde17-125">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="cde17-125">Click **OK**.</span></span>

5. <span data-ttu-id="cde17-126">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cde17-126">Click **Save**.</span></span>

## <span data-ttu-id="cde17-127"><a name="permissions"></a>Begränsa behörigheter tooauthenticated användare</span><span class="sxs-lookup"><span data-stu-id="cde17-127"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="cde17-128">Kör hello klientprojekt i Visual Studio eller Xamarin Studio på en enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="cde17-128">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator.</span></span> <span data-ttu-id="cde17-129">Kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="cde17-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="cde17-130">Detta beror på att hello app försöker tooaccess serverdelen för Mobilappen som en oautentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="cde17-130">This happens because hello app attempts tooaccess your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="cde17-131">Hej *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="cde17-131">hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="cde17-132">Därefter uppdaterar du hello klienten app toorequest resurser från hello mobilappsserverdel med en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="cde17-132">Next, you will update hello client app toorequest resources from hello Mobile App backend with an authenticated user.</span></span>

## <span data-ttu-id="cde17-133"><a name="add-authentication"></a>Lägg till autentisering toohello app</span><span class="sxs-lookup"><span data-stu-id="cde17-133"><a name="add-authentication"></a>Add authentication toohello app</span></span>
<span data-ttu-id="cde17-134">hello appen är uppdaterade toorequire användare tootap hello **inloggning** knappen och autentisera innan data visas.</span><span class="sxs-lookup"><span data-stu-id="cde17-134">hello app is updated toorequire users tootap hello **Sign in** button and authenticate before data is displayed.</span></span>

1. <span data-ttu-id="cde17-135">Lägg till följande kod toohello hello **TodoActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="cde17-135">Add hello following code toohello **TodoActivity** class:</span></span>
   
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
   
    <span data-ttu-id="cde17-136">Detta skapar en ny metod tooauthenticate en användare och en metod hanterare för en ny **inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="cde17-136">This creates a new method tooauthenticate a user and a method handler for a new **Sign in** button.</span></span> <span data-ttu-id="cde17-137">hello användaren i hello exempelkod ovan autentiseras med hjälp av en inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="cde17-137">hello user in hello example code above is authenticated by using a Facebook login.</span></span> <span data-ttu-id="cde17-138">En dialogruta är används toodisplay hello användar-ID som autentiseras en gång.</span><span class="sxs-lookup"><span data-stu-id="cde17-138">A dialog is used toodisplay hello user ID once authenticated.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="cde17-139">Om du använder en identitetsleverantör än Facebook ändrar hello-värdet som skickades för**LoginAsync** ovan tooone av hello följande: *MicrosoftAccount*, *Twitter*, *Google*, eller *WindowsAzureActiveDirectory*.</span><span class="sxs-lookup"><span data-stu-id="cde17-139">If you are using an identity provider other than Facebook, change hello value passed too**LoginAsync** above tooone of hello following: *MicrosoftAccount*, *Twitter*, *Google*, or *WindowsAzureActiveDirectory*.</span></span>
   > 
   > 
2. <span data-ttu-id="cde17-140">I hello **OnCreate** metod-, delete- eller kommenterar ut hello följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="cde17-140">In hello **OnCreate** method, delete or comment-out hello following line of code:</span></span>
   
        OnRefreshItemsSelected ();
3. <span data-ttu-id="cde17-141">Lägg till följande hello i hello Activity_To_Do.axml filen *LoginUser* knappen definitionen innan hello befintliga *AddItem* knappen:</span><span class="sxs-lookup"><span data-stu-id="cde17-141">In hello Activity_To_Do.axml file, add hello following *LoginUser* button definition before hello existing *AddItem* button:</span></span>
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. <span data-ttu-id="cde17-142">Lägg till följande element toohello Strings.xml resursfilen hello:</span><span class="sxs-lookup"><span data-stu-id="cde17-142">Add hello following element toohello Strings.xml resources file:</span></span>
   
        <string name="login_button_text">Sign in</string>
5. <span data-ttu-id="cde17-143">Öppna hello AndroidManifest.xml filen, lägga till följande kod i hello `<application>` XML-elementet:</span><span class="sxs-lookup"><span data-stu-id="cde17-143">Open hello AndroidManifest.xml file, add hello following code inside `<application>` XML element:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. <span data-ttu-id="cde17-144">I Visual Studio eller Xamarin Studio kör hello klientprojektet på en enhet eller emulator och logga in med ditt valda identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="cde17-144">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator and sign in with your chosen identity provider.</span></span> <span data-ttu-id="cde17-145">När du har loggat in, hello visas inloggnings-ID och hello listan över todo-objekt, och du kan göra uppdateringar toohello data.</span><span class="sxs-lookup"><span data-stu-id="cde17-145">When you are successfully logged-in, hello app will display your login ID and hello list of todo items, and you can make updates toohello data.</span></span>

<!-- URLs. -->
[skapa en Xamarin.Android-app]: app-service-mobile-xamarin-android-get-started.md