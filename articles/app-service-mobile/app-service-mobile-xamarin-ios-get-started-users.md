---
title: "Komma igång med autentisering för Mobile Apps i Xamarin-iOS"
description: "Lär dig hur du använder Mobilappar för att autentisera användare av Xamarin iOS-app via en mängd olika identitetsleverantörer, inklusive AAD, Google, Facebook, Twitter och Microsoft."
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
ms.openlocfilehash: 454b2df5a9bf8cfba93befea54370957ab044d95
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinios-app"></a><span data-ttu-id="2df90-103">Lägga till autentisering i en Xamarin.iOS-app</span><span class="sxs-lookup"><span data-stu-id="2df90-103">Add authentication to your Xamarin.iOS app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="2df90-104">Det här avsnittet visar hur du autentiserar användare i en Apptjänst Mobile App från klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="2df90-104">This topic shows you how to authenticate users of an App Service Mobile App from your client application.</span></span> <span data-ttu-id="2df90-105">I kursen får du lägger till autentisering Xamarin.iOS quickstart projektet med en identitetsleverantör som stöds av App Service.</span><span class="sxs-lookup"><span data-stu-id="2df90-105">In this tutorial, you add authentication to the Xamarin.iOS quickstart project using an identity provider that is supported by App Service.</span></span> <span data-ttu-id="2df90-106">Efter att har autentiseras och auktoriseras av din Mobilapp användar-ID-värde visas och du kommer att kunna få åtkomst till begränsade tabelldata.</span><span class="sxs-lookup"><span data-stu-id="2df90-106">After being successfully authenticated and authorized by your Mobile App, the user ID value is displayed and you will be able to access restricted table data.</span></span>

<span data-ttu-id="2df90-107">Du måste slutföra kursen [skapa en Xamarin.iOS-app].</span><span class="sxs-lookup"><span data-stu-id="2df90-107">You must first complete the tutorial [Create a Xamarin.iOS app].</span></span> <span data-ttu-id="2df90-108">Om du inte använder serverprojekt hämtade Snabbstart, måste du lägga till tillägget autentiseringspaket projektet.</span><span class="sxs-lookup"><span data-stu-id="2df90-108">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="2df90-109">Mer information om server tilläggspaket finns [arbeta med serverdelen .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="2df90-109">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <a name="register-your-app-for-authentication-and-configure-app-services"></a><span data-ttu-id="2df90-110">Registrera din app för autentisering och konfigurera Apptjänster</span><span class="sxs-lookup"><span data-stu-id="2df90-110">Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <a name="add-your-app-to-the-allowed-external-redirect-urls"></a><span data-ttu-id="2df90-111">Lägg till din app i tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="2df90-111">Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="2df90-112">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="2df90-112">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="2df90-113">Detta gör att autentiseringssystemet kan omdirigera tillbaka till din app när autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="2df90-113">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="2df90-114">I den här självstudiekursen kommer vi använda URL-schemat _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="2df90-114">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="2df90-115">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="2df90-115">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="2df90-116">Det bör vara unikt för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="2df90-116">It should be unique to your mobile application.</span></span> <span data-ttu-id="2df90-117">Du vill aktivera omdirigering på serversidan:</span><span class="sxs-lookup"><span data-stu-id="2df90-117">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="2df90-118">Välj din Apptjänst i [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="2df90-118">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="2df90-119">Klicka på den **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="2df90-119">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="2df90-120">I den **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="2df90-120">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="2df90-121">Den **url_scheme_of_your_app** i den här strängen är URL-schemat för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="2df90-121">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="2df90-122">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="2df90-122">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="2df90-123">Du bör anteckna den sträng som du väljer när du behöver justera mobilprogram koden med URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="2df90-123">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="2df90-124">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="2df90-124">Click **OK**.</span></span>

5. <span data-ttu-id="2df90-125">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="2df90-125">Click **Save**.</span></span>

## <a name="restrict-permissions-to-authenticated-users"></a><span data-ttu-id="2df90-126">Begränsa behörighet för autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="2df90-126">Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="2df90-127">&nbsp;&nbsp;4.</span><span class="sxs-lookup"><span data-stu-id="2df90-127">&nbsp;&nbsp;4.</span></span> <span data-ttu-id="2df90-128">Kör klientprojektet i Visual Studio eller Xamarin Studio på en enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="2df90-128">In Visual Studio or Xamarin Studio, run the client project on a device or emulator.</span></span> <span data-ttu-id="2df90-129">Kontrollera att ett undantag med en statuskod 401 (obehörig) aktiveras när appen startar.</span><span class="sxs-lookup"><span data-stu-id="2df90-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="2df90-130">Felet loggas i konsolen för felsökning.</span><span class="sxs-lookup"><span data-stu-id="2df90-130">The failure is logged to the console of the debugger.</span></span> <span data-ttu-id="2df90-131">Därför bör du se felet i utdatafönstret i Visual Studio.</span><span class="sxs-lookup"><span data-stu-id="2df90-131">So in Visual Studio, you should see the failure in the output window.</span></span>

<span data-ttu-id="2df90-132">&nbsp;&nbsp;Felet obehörig beror på att appen försöker få åtkomst till din mobilappsserverdel som en oautentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="2df90-132">&nbsp;&nbsp;This unauthorized failure happens because the app attempts to access your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="2df90-133">Den *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="2df90-133">The *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="2df90-134">Därefter uppdaterar du klientappen till begäran resurser från serverdelen för Mobilappen med en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="2df90-134">Next, you will update the client app to request resources from the Mobile App backend with an authenticated user.</span></span>

## <a name="add-authentication-to-the-app"></a><span data-ttu-id="2df90-135">Lägg till autentisering i appen</span><span class="sxs-lookup"><span data-stu-id="2df90-135">Add authentication to the app</span></span>
<span data-ttu-id="2df90-136">I det här avsnittet ska du ändra appen för att visas en inloggningsskärm innan data.</span><span class="sxs-lookup"><span data-stu-id="2df90-136">In this section, you will modify the app to display a login screen before displaying data.</span></span> <span data-ttu-id="2df90-137">När appen startar inte kan inte ansluta till din Apptjänst och visas inte några data.</span><span class="sxs-lookup"><span data-stu-id="2df90-137">When the app starts, it will not not connect to your App Service and will not display any data.</span></span> <span data-ttu-id="2df90-138">När först gången utför användaren uppdatera-gest inloggningsskärmen visas. listan över todo-objekt ska visas efter genomförd inloggning.</span><span class="sxs-lookup"><span data-stu-id="2df90-138">After the first time that the user performs the refresh gesture, the login screen will appear; after successful login the list of todo items will be displayed.</span></span>

1. <span data-ttu-id="2df90-139">Öppna filen i klientprojektet **QSTodoService.cs** och Lägg till följande med instruktionen och `MobileServiceUser` med accessorn för att klassen QSTodoService:</span><span class="sxs-lookup"><span data-stu-id="2df90-139">In the client project, open the file **QSTodoService.cs** and add the following using statement and `MobileServiceUser` with accessor to the QSTodoService class:</span></span>
 
        using UIKit;
       
        // Logged in user
        private MobileServiceUser user;
        public MobileServiceUser User { get { return user; } }
2. <span data-ttu-id="2df90-140">Lägga till nya metod med namnet **autentisera** till **QSTodoService** med definitionen av följande:</span><span class="sxs-lookup"><span data-stu-id="2df90-140">Add new method named **Authenticate** to **QSTodoService** with the following definition:</span></span>

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

    >[AZURE.NOTE] <span data-ttu-id="2df90-141">Om du använder en identitetsleverantör än en Facebook, ändra värdet som skickas till **LoginAsync** ovan till något av följande: _MicrosoftAccount_, _Twitter_,  _Google_, eller _WindowsAzureActiveDirectory_.</span><span class="sxs-lookup"><span data-stu-id="2df90-141">If you are using an identity provider other than a Facebook, change the value passed to **LoginAsync** above to one of the following: _MicrosoftAccount_, _Twitter_, _Google_, or _WindowsAzureActiveDirectory_.</span></span>

3. <span data-ttu-id="2df90-142">Öppna **QSTodoListViewController.cs**.</span><span class="sxs-lookup"><span data-stu-id="2df90-142">Open **QSTodoListViewController.cs**.</span></span> <span data-ttu-id="2df90-143">Ändra metoddefinitionen av **ViewDidLoad** tar bort anropet till **RefreshAsync()** mot slutet:</span><span class="sxs-lookup"><span data-stu-id="2df90-143">Modify the method definition of **ViewDidLoad** removing the call to **RefreshAsync()** near the end:</span></span>
   
        public override async void ViewDidLoad ()
        {
            base.ViewDidLoad ();
   
            todoService = QSTodoService.DefaultService;
            await todoService.InitializeStoreAsync();
   
            RefreshControl.ValueChanged += async (sender, e) => {
                await RefreshAsync();
            }
   
            // Comment out the call to RefreshAsync
            // await RefreshAsync();
        }
4. <span data-ttu-id="2df90-144">Ändra metoden **RefreshAsync** att autentisera om det **användaren** -egenskapen är null.</span><span class="sxs-lookup"><span data-stu-id="2df90-144">Modify the method **RefreshAsync** to authenticate if the **User** property is null.</span></span> <span data-ttu-id="2df90-145">Lägg till följande kod högst upp i metoddefinition:</span><span class="sxs-lookup"><span data-stu-id="2df90-145">Add the following code at the top of the method definition:</span></span>
   
        // start of RefreshAsync method
        if (todoService.User == null) {
            await QSTodoService.DefaultService.Authenticate(this);
            if (todoService.User == null) {
                Console.WriteLine("couldn't login!!");
                return;
            }
        }
        // rest of RefreshAsync method
5. <span data-ttu-id="2df90-146">Öppna **AppDelegate.cs**, lägger du till följande metod:</span><span class="sxs-lookup"><span data-stu-id="2df90-146">Open **AppDelegate.cs**, add the following method:</span></span>

        public static Func<NSUrl, bool> ResumeWithURL;

        public override bool OpenUrl(UIApplication app, NSUrl url, NSDictionary options)
        {
            return ResumeWithURL != null && ResumeWithURL(url);
        }
6. <span data-ttu-id="2df90-147">Öppna **Info.plist** fil, gå till **URL typer** i den **Avancerat** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="2df90-147">Open **Info.plist** file, navigate to **URL Types** in the **Advanced** section.</span></span> <span data-ttu-id="2df90-148">Nu konfigurera den **identifierare** och **URL-scheman** URL-typen och klicka på **Lägg till URL-typen**.</span><span class="sxs-lookup"><span data-stu-id="2df90-148">Now configure the **Identifier** and the **URL Schemes** of your URL Type and click **Add URL Type**.</span></span> <span data-ttu-id="2df90-149">**URL-scheman** bör vara samma som {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="2df90-149">**URL Schemes** should be the same as your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="2df90-150">I Visual Studio eller Xamarin Studio ansluten till din Xamarin skapa värd på en Mac som kör klientprojektet inriktning på en enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="2df90-150">In Visual Studio or Xamarin Studio connected to your Xamarin Build Host on your Mac, run the client project targeting a device or emulator.</span></span> <span data-ttu-id="2df90-151">Kontrollera att inga data visas i appen.</span><span class="sxs-lookup"><span data-stu-id="2df90-151">Verify that the app displays no data.</span></span>
   
    <span data-ttu-id="2df90-152">Utföra en uppdatering gest genom att dra nedåt i listan över objekt, vilket leder till inloggningssidan visas.</span><span class="sxs-lookup"><span data-stu-id="2df90-152">Perform the refresh gesture by pulling down the list of items, which will cause the login screen to appear.</span></span> <span data-ttu-id="2df90-153">När du har angett giltiga autentiseringsuppgifter, visas listan över todo-objekt och du kan göra uppdateringar till data.</span><span class="sxs-lookup"><span data-stu-id="2df90-153">Once you have successfully entered valid credentials, the app will display the list of todo items, and you can make updates to the data.</span></span>

<!-- URLs. -->
[Submit an app page]: http://go.microsoft.com/fwlink/p/?LinkID=266582
[My Applications]: http://go.microsoft.com/fwlink/p/?LinkId=262039
<span data-ttu-id="2df90-154">[skapa en Xamarin.iOS-app]: app-service-mobile-xamarin-ios-get-started.md</span><span class="sxs-lookup"><span data-stu-id="2df90-154">[Create a Xamarin.iOS app]: app-service-mobile-xamarin-ios-get-started.md</span></span>