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
# <a name="add-authentication-tooyour-xamarinios-app"></a><span data-ttu-id="4b2b5-103">Lägg till autentisering tooyour Xamarin.iOS-app</span><span class="sxs-lookup"><span data-stu-id="4b2b5-103">Add authentication tooyour Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="4b2b5-104">Det här avsnittet beskrivs hur du tooauthenticate användare av en Apptjänst Mobile App från klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-104">This topic shows you how tooauthenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="4b2b5-105">Lägg till autentisering toohello Xamarin.iOS snabbstartsprojekt en identitetsleverantör som stöds av Apptjänst i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-105">In this tutorial, you add authentication toohello Xamarin.iOS quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="4b2b5-106">Efter att har autentiseras och auktoriseras av din Mobilapp hello användar-ID-värde visas och du kommer att kunna tooaccess begränsad tabelldata.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-106">After being successfully authenticated and authorized by your Mobile App, hello user ID value is displayed and you will be able tooaccess restricted table data.</span></span>

<span data-ttu-id="4b2b5-107">Du måste slutföra kursen hello [skapa en Xamarin.iOS-app].</span><span class="sxs-lookup"><span data-stu-id="4b2b5-107">You must first complete hello tutorial [Create a Xamarin.iOS app].</span></span> <span data-ttu-id="4b2b5-108">Om du inte använder hello laddat ned Snabbstart serverprojekt, måste du lägga till hello autentisering tillägget paketet tooyour projekt.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-108">If you do not use hello downloaded quick start server project, you must add hello authentication extension package tooyour project.</span></span> <span data-ttu-id="4b2b5-109">Mer information om server tilläggspaket finns [arbeta med serverdelen för hello .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="4b2b5-109">For more information about server extension packages, see [Work with hello .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="4b2b5-110">Registrera din app för autentisering och konfigurera Apptjänster</span><span class="sxs-lookup"><span data-stu-id="4b2b5-110">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-toohello-allowed-external-redirect-urls"></a><span data-ttu-id="4b2b5-111">Lägg till din app toohello tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="4b2b5-111">Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="4b2b5-112">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-112">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="4b2b5-113">Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-113">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="4b2b5-114">I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-114">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="4b2b5-115">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-115">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="4b2b5-116">Det bör vara unikt tooyour mobila program.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-116">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="4b2b5-117">tooenable hello omdirigering på serversidan hello:</span><span class="sxs-lookup"><span data-stu-id="4b2b5-117">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="4b2b5-118">I hello [Azure portal], väljer du din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-118">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="4b2b5-119">Klicka på hello **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-119">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="4b2b5-120">I hello **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-120">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="4b2b5-121">Hej **url_scheme_of_your_app** i den här strängen är hello URL-schema för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-121">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="4b2b5-122">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="4b2b5-122">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="4b2b5-123">Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-123">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="4b2b5-124">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-124">Click **OK**.</span></span>

5. <span data-ttu-id="4b2b5-125">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-125">Click **Save**.</span></span>

## <a name="restrict-permissions-tooauthenticated-users"></a><span data-ttu-id="4b2b5-126">Begränsa behörigheter tooauthenticated användare</span><span class="sxs-lookup"><span data-stu-id="4b2b5-126">Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="4b2b5-127">&nbsp;&nbsp;4.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-127">&nbsp;&nbsp;4.</span></span> <span data-ttu-id="4b2b5-128">Kör hello klientprojekt i Visual Studio eller Xamarin Studio på en enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-128">In Visual Studio or Xamarin Studio, run hello client project on a device or emulator.</span></span> <span data-ttu-id="4b2b5-129">Kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="4b2b5-130">hello misslyckade är loggade toohello konsol hello felsökare.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-130">hello failure is logged toohello console of hello debugger.</span></span> <span data-ttu-id="4b2b5-131">Därför bör du se hello fel i utdatafönstret hello i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-131">So in Visual Studio, you should see hello failure in hello output window.</span></span>

<span data-ttu-id="4b2b5-132">&nbsp;&nbsp;Felet obehörig beror hello app försöker tooaccess serverdelen för Mobilappen som en oautentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-132">&nbsp;&nbsp;This unauthorized failure happens because hello app attempts tooaccess your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="4b2b5-133">Hej *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-133">hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="4b2b5-134">Därefter uppdaterar du hello klienten app toorequest resurser från hello mobilappsserverdel med en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-134">Next, you will update hello client app toorequest resources from hello Mobile App backend with an authenticated user.</span></span>

## <a name="add-authentication-toohello-app"></a><span data-ttu-id="4b2b5-135">Lägg till autentisering toohello app</span><span class="sxs-lookup"><span data-stu-id="4b2b5-135">Add authentication toohello app</span></span>
<span data-ttu-id="4b2b5-136">I det här avsnittet ska du ändra hello app toodisplay en inloggningsskärm innan data.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-136">In this section, you will modify hello app toodisplay a login screen before displaying data.</span></span> <span data-ttu-id="4b2b5-137">När hello app startar inte ansluter tooyour Apptjänst inte och visas inte några data.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-137">When hello app starts, it will not not connect tooyour App Service and will not display any data.</span></span> <span data-ttu-id="4b2b5-138">Efter hello första gången utför hello användaren hello uppdatera gest hello inloggningsskärmen visas. efter genomförd inloggning ska hello lista över todo-objekt visas.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-138">After hello first time that hello user performs hello refresh gesture, hello login screen will appear; after successful login hello list of todo items will be displayed.</span></span>

1. <span data-ttu-id="4b2b5-139">Öppna hello-filen i hello klientprojektet **QSTodoService.cs** och Lägg till följande hello-instruktion och `MobileServiceUser` med accessor toohello QSTodoService klass:</span><span class="sxs-lookup"><span data-stu-id="4b2b5-139">In hello client project, open hello file **QSTodoService.cs** and add hello following using statement and `MobileServiceUser` with accessor toohello QSTodoService class:</span></span>
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. <span data-ttu-id="4b2b5-140">Lägga till nya metod med namnet **autentisera** för**QSTodoService** med hello definitionen:</span><span class="sxs-lookup"><span data-stu-id="4b2b5-140">Add new method named **Authenticate** too**QSTodoService** with hello following definition:</span></span>

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

    >[AZURE.NOTE] <span data-ttu-id="4b2b5-141">Om du använder en identitetsleverantör än en Facebook ändrar hello-värdet som skickades för**LoginAsync** ovan tooone av hello följande: _MicrosoftAccount_, _Twitter_, _Google_, eller _WindowsAzureActiveDirectory_.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-141">If you are using an identity provider other than a Facebook, change hello value passed too**LoginAsync** above tooone of hello following: _MicrosoftAccount_, _Twitter_, _Google_, or _WindowsAzureActiveDirectory_.</span></span>

3. <span data-ttu-id="4b2b5-142">Öppna **QSTodoListViewController.cs**.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-142">Open **QSTodoListViewController.cs**.</span></span> <span data-ttu-id="4b2b5-143">Ändra hello metoddefinition av **ViewDidLoad** tar bort hello anrop för**RefreshAsync()** nära hello slut:</span><span class="sxs-lookup"><span data-stu-id="4b2b5-143">Modify hello method definition of **ViewDidLoad** removing hello call too**RefreshAsync()** near hello end:</span></span>
   
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
4. <span data-ttu-id="4b2b5-144">Ändra hello metoden **RefreshAsync** tooauthenticate om hello **användaren** -egenskapen är null.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-144">Modify hello method **RefreshAsync** tooauthenticate if hello **User** property is null.</span></span> <span data-ttu-id="4b2b5-145">Lägg till följande kod hello överst i hello metoddefinition hello:</span><span class="sxs-lookup"><span data-stu-id="4b2b5-145">Add hello following code at hello top of hello method definition:</span></span>
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. <span data-ttu-id="4b2b5-146">Öppna **AppDelegate.cs**, Lägg till följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="4b2b5-146">Open **AppDelegate.cs**, add hello following method:</span></span>

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. <span data-ttu-id="4b2b5-147">Öppna **Info.plist** fil, navigera för**URL typer** i hello **Avancerat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-147">Open **Info.plist** file, navigate too**URL Types** in hello **Advanced** section.</span></span> <span data-ttu-id="4b2b5-148">Nu konfigurera hello **identifierare** och hello **URL-scheman** URL-typen och klicka på **Lägg till URL-typen**.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-148">Now configure hello **Identifier** and hello **URL Schemes** of your URL Type and click **Add URL Type**.</span></span> <span data-ttu-id="4b2b5-149">**URL-scheman** ska vara hello samma som {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-149">**URL Schemes** should be hello same as your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="4b2b5-150">I Visual Studio eller Xamarin Studio anslutna tooyour Xamarin skapa värden på en Mac som kör hello klientprojektet inriktning på en enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-150">In Visual Studio or Xamarin Studio connected tooyour Xamarin Build Host on your Mac, run hello client project targeting a device or emulator.</span></span> <span data-ttu-id="4b2b5-151">Kontrollera appen hello visar inga data.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-151">Verify that hello app displays no data.</span></span>
   
    <span data-ttu-id="4b2b5-152">Utföra hello uppdatera gest genom att dra nedåt hello listan över objekt, vilket leder till hello inloggningen skärmen tooappear.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-152">Perform hello refresh gesture by pulling down hello list of items, which will cause hello login screen tooappear.</span></span> <span data-ttu-id="4b2b5-153">När du har angett giltiga autentiseringsuppgifter, hello app visar hello lista över todo-objekt och du kan göra uppdateringar toohello data.</span><span class="sxs-lookup"><span data-stu-id="4b2b5-153">Once you have successfully entered valid credentials, hello app will display hello list of todo items, and you can make updates toohello data.</span></span>

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[skapa en Xamarin.iOS-app]: app-service-mobile-xamarin-ios-get-started.md