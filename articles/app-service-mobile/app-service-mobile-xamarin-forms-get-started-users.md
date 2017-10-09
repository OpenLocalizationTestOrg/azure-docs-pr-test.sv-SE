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
# <a name="add-authentication-tooyour-xamarin-forms-app"></a><span data-ttu-id="d6bfe-103">Lägg till autentisering tooyour Xamarin Forms app</span><span class="sxs-lookup"><span data-stu-id="d6bfe-103">Add authentication tooyour Xamarin Forms app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

## <a name="overview"></a><span data-ttu-id="d6bfe-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d6bfe-104">Overview</span></span>
<span data-ttu-id="d6bfe-105">Det här avsnittet beskrivs hur du tooauthenticate användare av en Apptjänst Mobile App från klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-105">This topic shows you how tooauthenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="d6bfe-106">I kursen får du lägger till autentisering hello Xamarin Forms snabbstartsprojekt en identitetsleverantör som stöds av App Service.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-106">In this tutorial, you add authentication to hello Xamarin Forms quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="d6bfe-107">Efter att har autentiseras och auktoriseras av din Mobilapp hello användar-ID-värde visas och du kommer att kunna tooaccess begränsad tabelldata.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-107">After being successfully authenticated and authorized by your Mobile App, hello user ID value is displayed, and you will be able tooaccess restricted table data.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d6bfe-108">Krav</span><span class="sxs-lookup"><span data-stu-id="d6bfe-108">Prerequisites</span></span>
<span data-ttu-id="d6bfe-109">För hello bästa resultat med den här självstudiekursen, rekommenderar vi att du först slutföra hello [skapa en app i Xamarin Forms] [ 1] kursen.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-109">For hello best result with this tutorial, we recommend that you first complete hello [Create a Xamarin Forms app][1] tutorial.</span></span> <span data-ttu-id="d6bfe-110">När den här kursen har du en Xamarin Forms-projekt som är en app för flera plattformar TodoList.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-110">After you complete this tutorial, you will have a Xamarin Forms project that is a multi-platform TodoList app.</span></span>

<span data-ttu-id="d6bfe-111">Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello autentisering tillägget paketet tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-111">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="d6bfe-112">Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps][2].</span><span class="sxs-lookup"><span data-stu-id="d6bfe-112">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps][2].</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="d6bfe-113">Registrera din app för autentisering och konfigurera Apptjänster</span><span class="sxs-lookup"><span data-stu-id="d6bfe-113">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="d6bfe-114"><a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="d6bfe-114"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="d6bfe-115">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-115">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="d6bfe-116">Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-116">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="d6bfe-117">I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-117">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="d6bfe-118">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-118">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="d6bfe-119">Det bör vara unikt tooyour mobila program.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-119">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="d6bfe-120">tooenable hello omdirigering på serversidan hello:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-120">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="d6bfe-121">I hello [Azure portal], väljer du din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-121">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="d6bfe-122">Klicka på hello **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-122">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="d6bfe-123">I hello **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-123">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="d6bfe-124">Hej **url_scheme_of_your_app** i den här strängen är hello URL-schema för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-124">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="d6bfe-125">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="d6bfe-125">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="d6bfe-126">Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-126">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="d6bfe-127">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-127">Click **OK**.</span></span>

5. <span data-ttu-id="d6bfe-128">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-128">Click **Save**.</span></span>

## <a name="restrict-permissions-tooauthenticated-users"></a><span data-ttu-id="d6bfe-129">Begränsa behörigheter tooauthenticated användare</span><span class="sxs-lookup"><span data-stu-id="d6bfe-129">Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

## <a name="add-authentication-toohello-portable-class-library"></a><span data-ttu-id="d6bfe-130">Lägg till autentisering toohello bärbar klassbiblioteket</span><span class="sxs-lookup"><span data-stu-id="d6bfe-130">Add authentication toohello portable class library</span></span>
<span data-ttu-id="d6bfe-131">Mobile Apps använder hello [LoginAsync] [ 3] metod på hello [MobileServiceClient] [ 4] toosign i en användare med App Service autentisering.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-131">Mobile Apps uses hello [LoginAsync][3] extension method on hello [MobileServiceClient][4] toosign in a user with App Service authentication.</span></span> <span data-ttu-id="d6bfe-132">Det här exemplet används en hanterad server autentiseringsflödet som visar hello provider i inloggningsgränssnittet i hello app.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-132">This sample uses a server-managed authentication flow that displays hello provider's sign-in interface in hello app.</span></span> <span data-ttu-id="d6bfe-133">Mer information finns i [Server-hanterade autentisering][5].</span><span class="sxs-lookup"><span data-stu-id="d6bfe-133">For more information, see [Server-managed authentication][5].</span></span> <span data-ttu-id="d6bfe-134">För att ge en bättre användarupplevelse i din produktionsapp, bör du istället använda [klienten hanteras autentisering][6].</span><span class="sxs-lookup"><span data-stu-id="d6bfe-134">To provide a better user experience in your production app, you should consider instead using [Client-managed authentication][6].</span></span>

<span data-ttu-id="d6bfe-135">tooauthenticate med Xamarin Forms projekt definiera en **IAuthenticate** hello bärbara Class Library för hello app-gränssnitt.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-135">tooauthenticate with a Xamarin Forms project, define an **IAuthenticate** interface in hello Portable Class Library for hello app.</span></span> <span data-ttu-id="d6bfe-136">Lägg sedan till en **inloggning** knappen toohello användargränssnitt som definierats i hello bärbara klassbiblioteket, som du klickar på toostart autentisering.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-136">Then add a **Sign-in** button toohello user interface defined in hello Portable Class Library, which you click toostart authentication.</span></span> <span data-ttu-id="d6bfe-137">Data läses från hello mobilappsserverdel efter en lyckad autentisering.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-137">Data is loaded from hello mobile app backend after successful authentication.</span></span>

<span data-ttu-id="d6bfe-138">Implementera hello **IAuthenticate** gränssnitt för varje plattform som stöds av din app.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-138">Implement hello **IAuthenticate** interface for each platform supported by your app.</span></span>

1. <span data-ttu-id="d6bfe-139">Öppna i Visual Studio eller Xamarin Studio App.cs från hello-projekt med **bärbar** i hello namn som är bärbara klassbiblioteket projektet, och Lägg sedan till följande hello `using` instruktionen:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-139">In Visual Studio or Xamarin Studio, open App.cs from hello project with **Portable** in hello name, which is Portable Class Library project, then  add hello following `using` statement:</span></span>

        using System.Threading.Tasks;
2. <span data-ttu-id="d6bfe-140">Lägg till följande hello i App.cs, `IAuthenticate` gränssnitt definition omedelbart före hello `App` klassen definition.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-140">In App.cs, add hello following `IAuthenticate` interface definition immediately before hello `App` class definition.</span></span>

        public interface IAuthenticate
        {
            Task<bool> Authenticate();
        }
3. <span data-ttu-id="d6bfe-141">tooinitialize hello gränssnitt med en plattformsspecifik-implementering, lägga till hello efter statiska medlemmarna toohello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-141">tooinitialize hello interface with a platform-specific implementation, add hello following static members toohello **App** class.</span></span>

        public static IAuthenticate Authenticator { get; private set; }

        public static void Init(IAuthenticate authenticator)
        {
            Authenticator = authenticator;
        }
4. <span data-ttu-id="d6bfe-142">Öppna TodoList.xaml från hello bärbara klassbiblioteket projekt, Lägg till följande hello **knappen** element i hello *buttonsPanel* layout-elementet efter befintlig hello-knapp:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-142">Open TodoList.xaml from hello Portable Class Library project, add hello following **Button** element in hello *buttonsPanel* layout element, after hello existing button:</span></span>

          <Button x:Name="loginButton" Text="Sign-in" MinimumHeightRequest="30"
            Clicked="loginButton_Clicked"/>

    <span data-ttu-id="d6bfe-143">Den här knappen utlöser server-hanterade autentisering med mobilappsserverdelen.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-143">This button triggers server-managed authentication with your mobile app backend.</span></span>
5. <span data-ttu-id="d6bfe-144">Öppna TodoList.xaml.cs från hello bärbara klassbiblioteket projekt och lägger till följande fält toohello hello `TodoList` klass:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-144">Open TodoList.xaml.cs from hello Portable Class Library project, then add hello following field toohello `TodoList` class:</span></span>

        // Track whether hello user has authenticated.
        bool authenticated = false;
6. <span data-ttu-id="d6bfe-145">Ersätt hello **OnAppearing** metod med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-145">Replace hello **OnAppearing** method with hello following code:</span></span>

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

    <span data-ttu-id="d6bfe-146">Den här koden ser till att data uppdateras bara från hello-tjänsten när du har autentiserats.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-146">This code makes sure that data is only refreshed from hello service after you have been authenticated.</span></span>
7. <span data-ttu-id="d6bfe-147">Lägg till följande hanterare för hello hello **Clicked** händelse toohello **TodoList** klass:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-147">Add hello following handler for hello **Clicked** event toohello **TodoList** class:</span></span>

        async void loginButton_Clicked(object sender, EventArgs e)
        {
            if (App.Authenticator != null)
                authenticated = await App.Authenticator.Authenticate();

            // Set syncItems tootrue toosynchronize hello data on startup when offline is enabled.
            if (authenticated == true)
                await RefreshItems(true, syncItems: false);
        }
8. <span data-ttu-id="d6bfe-148">Spara dina ändringar och återskapa hello bärbara klassbiblioteket projektet verifierar utan fel.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-148">Save your changes and rebuild hello Portable Class Library project verifying no errors.</span></span>

## <a name="add-authentication-toohello-android-app"></a><span data-ttu-id="d6bfe-149">Lägg till autentisering toohello Android-app</span><span class="sxs-lookup"><span data-stu-id="d6bfe-149">Add authentication toohello Android app</span></span>
<span data-ttu-id="d6bfe-150">Det här avsnittet visas hur tooimplement hello **IAuthenticate** gränssnitt i hello Android-app-projekt.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-150">This section shows how tooimplement hello **IAuthenticate** interface in hello Android app project.</span></span> <span data-ttu-id="d6bfe-151">Hoppa över det här avsnittet om du inte stöder Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-151">Skip this section if you are not supporting Android devices.</span></span>

1. <span data-ttu-id="d6bfe-152">Högerklicka i Visual Studio eller Xamarin Studio hello **droid** projektet sedan **Set as StartUp Project**.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-152">In Visual Studio or Xamarin Studio, right-click hello **droid** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="d6bfe-153">Tryck på F5 toostart hello projekt i hello felsökning och sedan kontrollera att ett undantag med en statuskod 401 (obehörig) aktiveras när appen startar.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-153">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="d6bfe-154">hello 401 kod skapas eftersom åtkomst på hello serverdel är endast begränsad tooauthorized användare.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-154">hello 401 code is produced because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="d6bfe-155">Öppna MainActivity.cs hello Android-projekt och lägga till följande hello `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-155">Open MainActivity.cs in hello Android project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="d6bfe-156">Uppdatera hello **MainActivity** klassen tooimplement hello **IAuthenticate** gränssnitt, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-156">Update hello **MainActivity** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity, IAuthenticate
5. <span data-ttu-id="d6bfe-157">Uppdatera hello **MainActivity** klassen genom att lägga till en **MobileServiceUser** fältet och en **autentisera** metod, vilket krävs av hello **IAuthenticate**  gränssnitt, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-157">Update hello **MainActivity** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

    <span data-ttu-id="d6bfe-158">Om du använder en identitetsleverantör än Facebook, välja ett annat värde för [MobileServiceAuthenticationProvider][7].</span><span class="sxs-lookup"><span data-stu-id="d6bfe-158">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider][7].</span></span>

6. <span data-ttu-id="d6bfe-159">Lägg till följande kod i hello <application> nod i AndroidManifest.xml:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-159">Add hello following code inside <application> node of AndroidManifest.xml:</span></span>

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

1. <span data-ttu-id="d6bfe-160">Lägg till följande kod toohello hello **OnCreate** metod för hello **MainActivity** klassen före hello anrop för`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-160">Add hello following code toohello **OnCreate** method of hello **MainActivity** class before hello call too`LoadApplication()`:</span></span>

        // Initialize hello authenticator before loading hello app.
        App.Init((IAuthenticate)this);

    <span data-ttu-id="d6bfe-161">Den här koden garanterar hello authenticator är initierad innan hello app belastning.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-161">This code ensures hello authenticator is initialized before hello app loads.</span></span>
2. <span data-ttu-id="d6bfe-162">Återskapa hello app, köra den och sedan logga in med hello autentiseringsprovider du har valt och kontrollera att du ska kunna tooaccess data som en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-162">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="add-authentication-toohello-ios-app"></a><span data-ttu-id="d6bfe-163">Lägg till autentisering toohello iOS-app</span><span class="sxs-lookup"><span data-stu-id="d6bfe-163">Add authentication toohello iOS app</span></span>
<span data-ttu-id="d6bfe-164">Det här avsnittet visas hur tooimplement hello **IAuthenticate** gränssnittet i hello iOS-app-projekt.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-164">This section shows how tooimplement hello **IAuthenticate** interface in hello iOS app project.</span></span> <span data-ttu-id="d6bfe-165">Hoppa över det här avsnittet om du inte stöder iOS-enheter.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-165">Skip this section if you are not supporting iOS devices.</span></span>

1. <span data-ttu-id="d6bfe-166">Högerklicka i Visual Studio eller Xamarin Studio hello **iOS** projektet sedan **Set as StartUp Project**.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-166">In Visual Studio or Xamarin Studio, right-click hello **iOS** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="d6bfe-167">Tryck på F5 toostart hello projekt i hello felsökning och sedan kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-167">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="d6bfe-168">hello 401 svar produceras eftersom åtkomst på hello serverdel är endast begränsad tooauthorized användare.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-168">hello 401 response is produced because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="d6bfe-169">Öppna AppDelegate.cs i hello iOS-projektet och Lägg till följande hello `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-169">Open AppDelegate.cs in hello iOS project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
4. <span data-ttu-id="d6bfe-170">Uppdatera hello **AppDelegate** klassen tooimplement hello **IAuthenticate** gränssnitt, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-170">Update hello **AppDelegate** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public partial class AppDelegate : global::Xamarin.Forms.Platform.iOS.FormsApplicationDelegate, IAuthenticate
5. <span data-ttu-id="d6bfe-171">Uppdatera hello **AppDelegate** klassen genom att lägga till en **MobileServiceUser** fältet och en **autentisera** metod, vilket krävs av hello **IAuthenticate**  gränssnitt, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-171">Update hello **AppDelegate** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

    <span data-ttu-id="d6bfe-172">Välj ett annat värde för [MobileServiceAuthenticationProvider] om du använder en identitetsleverantör än Facebook.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-172">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

6. <span data-ttu-id="d6bfe-173">Uppdatera hello AppDelegate klassen genom att lägga till metodöverlagringen OpenUrl (UIApplication app NSUrl url NSDictionary alternativ)</span><span class="sxs-lookup"><span data-stu-id="d6bfe-173">Update hello AppDelegate class by adding OpenUrl(UIApplication app, NSUrl url, NSDictionary options) method overload</span></span>

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(url);
        }

6. <span data-ttu-id="d6bfe-174">Lägg till följande rad med kod toohello hello **FinishedLaunching** innan hello anrop för`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-174">Add hello following line of code toohello **FinishedLaunching** method before hello call too`LoadApplication()`:</span></span>

        App.Init(this);

    <span data-ttu-id="d6bfe-175">Den här koden garanterar hello authenticator initieras innan hello app har lästs in.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-175">This code ensures hello authenticator is initialized before hello app is loaded.</span></span>

6. <span data-ttu-id="d6bfe-176">Lägg till **{url_scheme_of_your_app}** tooURL scheman i Info.plist.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-176">Add **{url_scheme_of_your_app}** tooURL Schemes in Info.plist.</span></span>

7. <span data-ttu-id="d6bfe-177">Återskapa hello app, köra den och sedan logga in med hello autentiseringsprovider du har valt och kontrollera att du ska kunna tooaccess data som en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-177">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="add-authentication-toowindows-10-including-phone-app-projects"></a><span data-ttu-id="d6bfe-178">Lägg till autentisering tooWindows 10 (inklusive Phone) app-projekt</span><span class="sxs-lookup"><span data-stu-id="d6bfe-178">Add authentication tooWindows 10 (including Phone) app projects</span></span>
<span data-ttu-id="d6bfe-179">Det här avsnittet visas hur tooimplement hello **IAuthenticate** gränssnittet i hello Windows 10-app-projekt.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-179">This section shows how tooimplement hello **IAuthenticate** interface in hello Windows 10 app projects.</span></span> <span data-ttu-id="d6bfe-180">hello samma steg gäller för universella Windowsplattformen (UWP) projekt, men använder hello **UWP** projekt (med noterats ändringar).</span><span class="sxs-lookup"><span data-stu-id="d6bfe-180">hello same steps apply for Universal Windows Platform (UWP) projects, but using hello **UWP** project (with noted changes).</span></span> <span data-ttu-id="d6bfe-181">Hoppa över det här avsnittet om du inte stöder Windows-enheter.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-181">Skip this section if you are not supporting Windows devices.</span></span>

1. <span data-ttu-id="d6bfe-182">”I Visual Studio högerklickar du på antingen hello **UWP** projektet sedan **Set as StartUp Project**.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-182">"In Visual Studio, right-click either hello **UWP** project, then **Set as StartUp Project**.</span></span>
2. <span data-ttu-id="d6bfe-183">Tryck på F5 toostart hello projekt i hello felsökning och sedan kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-183">Press F5 toostart hello project in hello debugger, then verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="d6bfe-184">hello 401-svar som beror på att åtkomst på hello serverdel är endast begränsad tooauthorized användare.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-184">hello 401 response happens because access on hello backend is restricted tooauthorized users only.</span></span>
3. <span data-ttu-id="d6bfe-185">Öppna MainPage.xaml.cs för Windows hello för app-projekt och Lägg till följande hello `using` instruktioner:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-185">Open MainPage.xaml.cs for hello Windows app project and add hello following `using` statements:</span></span>

        using Microsoft.WindowsAzure.MobileServices;
        using System.Threading.Tasks;
        using Windows.UI.Popups;
        using <your_Portable_Class_Library_namespace>;

    <span data-ttu-id="d6bfe-186">Ersätt `<your_Portable_Class_Library_namespace>` med hello namnområde för din bärbara klassbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-186">Replace `<your_Portable_Class_Library_namespace>` with hello namespace for your portable class library.</span></span>
4. <span data-ttu-id="d6bfe-187">Uppdatera hello **MainPage** klassen tooimplement hello **IAuthenticate** gränssnitt, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-187">Update hello **MainPage** class tooimplement hello **IAuthenticate** interface, as follows:</span></span>

        public sealed partial class MainPage : IAuthenticate
5. <span data-ttu-id="d6bfe-188">Uppdatera hello **MainPage** klassen genom att lägga till en **MobileServiceUser** fältet och en **autentisera** metod, vilket krävs av hello **IAuthenticate** gränssnitt, enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-188">Update hello **MainPage** class by adding a **MobileServiceUser** field and an **Authenticate** method, which is required by hello **IAuthenticate** interface, as follows:</span></span>

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

    <span data-ttu-id="d6bfe-189">Välj ett annat värde för [MobileServiceAuthenticationProvider] om du använder en identitetsleverantör än Facebook.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-189">If you are using an identity provider other than Facebook, choose a different value for [MobileServiceAuthenticationProvider].</span></span>

1. <span data-ttu-id="d6bfe-190">Lägg till följande kodrad i hello konstruktorn för hello hello **MainPage** klassen före hello anrop för`LoadApplication()`:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-190">Add hello following line of code in hello constructor for hello **MainPage** class before hello call too`LoadApplication()`:</span></span>

        // Initialize hello authenticator before loading hello app.
        <your_Portable_Class_Library_namespace>.App.Init(this);

    <span data-ttu-id="d6bfe-191">Ersätt `<your_Portable_Class_Library_namespace>` med hello namnområde för din bärbara klassbiblioteket.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-191">Replace `<your_Portable_Class_Library_namespace>` with hello namespace for your portable class library.</span></span>

3. <span data-ttu-id="d6bfe-192">Om du använder **UWP**, Lägg till följande hello **OnActivated** metod åsidosätter toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-192">If you are using **UWP**, add hello following **OnActivated** method override toohello **App** class:</span></span>

       protected override void OnActivated(IActivatedEventArgs args)
       {
           base.OnActivated(args);

            if (args.Kind == ActivationKind.Protocol)
            {
                ProtocolActivatedEventArgs protocolArgs = args as ProtocolActivatedEventArgs;
                TodoItemManager.DefaultManager.CurrentClient.ResumeWithURL(protocolArgs.Uri);
            }

       }

   <span data-ttu-id="d6bfe-193">Lägg till hello villkorlig koden från hello föregående kodfragment när hello metodersättningen redan finns.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-193">When hello method override already exists, add hello conditional code from hello preceding snippet.</span></span>  <span data-ttu-id="d6bfe-194">Den här koden krävs inte för universella Windows-projekt.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-194">This code is not required for Universal Windows projects.</span></span>

3. <span data-ttu-id="d6bfe-195">Lägg till **{url_scheme_of_your_app}** i Package.appxmanifest.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-195">Add **{url_scheme_of_your_app}** in Package.appxmanifest.</span></span> 

4. <span data-ttu-id="d6bfe-196">Återskapa hello app, köra den och sedan logga in med hello autentiseringsprovider du har valt och kontrollera att du ska kunna tooaccess data som en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-196">Rebuild hello app, run it, then sign in with hello authentication provider you chose and verify you are able tooaccess data as an authenticated user.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6bfe-197">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6bfe-197">Next steps</span></span>
<span data-ttu-id="d6bfe-198">Nu när du har slutfört den här självstudiekursen för grundläggande autentisering, Överväg fortsätter tooone av hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="d6bfe-198">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="d6bfe-199">Lägg till push-meddelanden tooyour app</span><span class="sxs-lookup"><span data-stu-id="d6bfe-199">Add push notifications tooyour app</span></span>](app-service-mobile-xamarin-forms-get-started-push.md)

  <span data-ttu-id="d6bfe-200">Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobilapp backend toouse Azure Notification Hubs toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-200">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="d6bfe-201">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="d6bfe-201">Enable offline sync for your app</span></span>](app-service-mobile-xamarin-forms-get-started-offline-data.md)

  <span data-ttu-id="d6bfe-202">Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-202">Learn how tooadd offline support your app using a Mobile App backend.</span></span> <span data-ttu-id="d6bfe-203">Offlinesynkronisering kan slutet användare toointeract med en mobil app - visa, lägga till eller ändra data – även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="d6bfe-203">Offline sync allows end users toointeract with a mobile app - viewing, adding, or modifying data - even when there is no network connection.</span></span>

<!-- Images. -->

<!-- URLs. -->
[1]: app-service-mobile-xamarin-forms-get-started.md
[2]: app-service-mobile-dotnet-backend-how-to-use-server-sdk.md
[3]: https://msdn.microsoft.com/library/azure/dn268341(v=azure.10).aspx
[4]: https://msdn.microsoft.com/library/azure/JJ553674(v=azure.10).aspx
[5]: app-service-mobile-dotnet-how-to-use-client-library.md#serverflow
[6]: app-service-mobile-dotnet-how-to-use-client-library.md#clientflow
[7]: https://msdn.microsoft.com/library/azure/jj730936(v=azure.10).aspx
