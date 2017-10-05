---
title: "Kom igång med Mobile Apps i Xamarin Android-autentisering"
description: "Lär dig hur du använder Mobilappar för att autentisera användare för Xamarin Android-appen via en mängd olika identitetsleverantörer, inklusive AAD, Google, Facebook, Twitter och Microsoft."
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
ms.openlocfilehash: 8f9a1109018c708d52cdcb7b8bce43861cecd31c
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-xamarinandroid-app"></a><span data-ttu-id="99c65-103">Lägg till autentisering i Xamarin.Android-app</span><span class="sxs-lookup"><span data-stu-id="99c65-103">Add authentication to your Xamarin.Android app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="99c65-104">Det här avsnittet visar hur du autentiserar användare i en Mobilapp från klientprogrammet.</span><span class="sxs-lookup"><span data-stu-id="99c65-104">This topic shows you how to authenticate users of a Mobile App from your client application.</span></span> <span data-ttu-id="99c65-105">I kursen får du lägger till autentisering i Snabbstart-projektet som en identitetsleverantör som stöds av Azure Mobile Apps.</span><span class="sxs-lookup"><span data-stu-id="99c65-105">In this tutorial, you add authentication to the quickstart project using an identity provider that is supported by Azure Mobile Apps.</span></span> <span data-ttu-id="99c65-106">Efter att kunna autentiseras och auktoriseras i Mobile App visas användar-ID-värdet.</span><span class="sxs-lookup"><span data-stu-id="99c65-106">After being successfully authenticated and authorized in the Mobile App, the user ID value is displayed.</span></span>

<span data-ttu-id="99c65-107">Den här kursen är baserad på Mobilapp Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="99c65-107">This tutorial is based on the Mobile App quickstart.</span></span> <span data-ttu-id="99c65-108">Du måste också slutföra kursen [skapa en Xamarin.Android-app].</span><span class="sxs-lookup"><span data-stu-id="99c65-108">You must also first complete the tutorial [Create a Xamarin.Android app].</span></span> <span data-ttu-id="99c65-109">Om du inte använder serverprojekt hämtade Snabbstart, måste du lägga till tillägget autentiseringspaket projektet.</span><span class="sxs-lookup"><span data-stu-id="99c65-109">If you do not use the downloaded quick start server project, you must add the authentication extension package to your project.</span></span> <span data-ttu-id="99c65-110">Mer information om server tilläggspaket finns [arbeta med serverdelen .NET SDK för Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span><span class="sxs-lookup"><span data-stu-id="99c65-110">For more information about server extension packages, see [Work with the .NET backend server SDK for Azure Mobile Apps](app-service-mobile-dotnet-backend-how-to-use-server-sdk.md).</span></span>

## <span data-ttu-id="99c65-111"><a name="register"></a>Registrera din app för autentisering och konfigurera Apptjänster</span><span class="sxs-lookup"><span data-stu-id="99c65-111"><a name="register"></a>Register your app for authentication and configure App Services</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="99c65-112"><a name="redirecturl"></a>Lägg till din app i tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="99c65-112"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="99c65-113">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="99c65-113">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="99c65-114">Detta gör att autentiseringssystemet kan omdirigera tillbaka till din app när autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="99c65-114">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="99c65-115">I den här självstudiekursen kommer vi använda URL-schemat _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="99c65-115">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="99c65-116">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="99c65-116">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="99c65-117">Det bör vara unikt för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="99c65-117">It should be unique to your mobile application.</span></span> <span data-ttu-id="99c65-118">Du vill aktivera omdirigering på serversidan:</span><span class="sxs-lookup"><span data-stu-id="99c65-118">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="99c65-119">Välj din Apptjänst i [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="99c65-119">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="99c65-120">Klicka på den **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="99c65-120">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="99c65-121">I den **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="99c65-121">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="99c65-122">Den **url_scheme_of_your_app** i den här strängen är URL-schemat för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="99c65-122">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="99c65-123">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="99c65-123">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="99c65-124">Du bör anteckna den sträng som du väljer när du behöver justera mobilprogram koden med URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="99c65-124">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="99c65-125">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="99c65-125">Click **OK**.</span></span>

5. <span data-ttu-id="99c65-126">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="99c65-126">Click **Save**.</span></span>

## <span data-ttu-id="99c65-127"><a name="permissions"></a>Begränsa behörighet för autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="99c65-127"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="99c65-128">Kör klientprojektet i Visual Studio eller Xamarin Studio på en enhet eller emulator.</span><span class="sxs-lookup"><span data-stu-id="99c65-128">In Visual Studio or Xamarin Studio, run the client project on a device or emulator.</span></span> <span data-ttu-id="99c65-129">Kontrollera att ett undantag med en statuskod 401 (obehörig) aktiveras när appen startar.</span><span class="sxs-lookup"><span data-stu-id="99c65-129">Verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="99c65-130">Detta beror på att appen försöker få åtkomst till din mobilappsserverdel som en oautentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="99c65-130">This happens because the app attempts to access your Mobile App backend as an unauthenticated user.</span></span> <span data-ttu-id="99c65-131">Den *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="99c65-131">The *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="99c65-132">Därefter uppdaterar du klientappen till begäran resurser från serverdelen för Mobilappen med en autentiserad användare.</span><span class="sxs-lookup"><span data-stu-id="99c65-132">Next, you will update the client app to request resources from the Mobile App backend with an authenticated user.</span></span>

## <span data-ttu-id="99c65-133"><a name="add-authentication"></a>Lägg till autentisering i appen</span><span class="sxs-lookup"><span data-stu-id="99c65-133"><a name="add-authentication"></a>Add authentication to the app</span></span>
<span data-ttu-id="99c65-134">Appen har uppdaterats så att användare måste trycker du på den **inloggning** knappen och autentisera innan data visas.</span><span class="sxs-lookup"><span data-stu-id="99c65-134">The app is updated to require users to tap the **Sign in** button and authenticate before data is displayed.</span></span>

1. <span data-ttu-id="99c65-135">Lägg till följande kod i den **TodoActivity** klass:</span><span class="sxs-lookup"><span data-stu-id="99c65-135">Add the following code to the **TodoActivity** class:</span></span>
   
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
                //Hide the button after authentication succeeds.
                FindViewById<Button>(Resource.Id.buttonLoginUser).Visibility = ViewStates.Gone;
   
                // Load the data.
                OnRefreshItemsSelected();
            }
        }
   
    <span data-ttu-id="99c65-136">Detta skapar en ny metod för att autentisera en användare och en metod hanterare för en ny **inloggning** knappen.</span><span class="sxs-lookup"><span data-stu-id="99c65-136">This creates a new method to authenticate a user and a method handler for a new **Sign in** button.</span></span> <span data-ttu-id="99c65-137">I ovanstående exempelkoden användaren autentiseras med hjälp av en inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="99c65-137">The user in the example code above is authenticated by using a Facebook login.</span></span> <span data-ttu-id="99c65-138">En dialogruta för att visa det användar-ID som autentiseras en gång.</span><span class="sxs-lookup"><span data-stu-id="99c65-138">A dialog is used to display the user ID once authenticated.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="99c65-139">Om du använder en identitetsleverantör än Facebook, ändra värdet som skickas till **LoginAsync** ovan till något av följande: *MicrosoftAccount*, *Twitter*,  *Google*, eller *WindowsAzureActiveDirectory*.</span><span class="sxs-lookup"><span data-stu-id="99c65-139">If you are using an identity provider other than Facebook, change the value passed to **LoginAsync** above to one of the following: *MicrosoftAccount*, *Twitter*, *Google*, or *WindowsAzureActiveDirectory*.</span></span>
   > 
   > 
2. <span data-ttu-id="99c65-140">I den **OnCreate** metod, ta bort eller kommenterar ut följande kodrad:</span><span class="sxs-lookup"><span data-stu-id="99c65-140">In the **OnCreate** method, delete or comment-out the following line of code:</span></span>
   
        OnRefreshItemsSelected ();
3. <span data-ttu-id="99c65-141">Lägg till följande i filen Activity_To_Do.axml *LoginUser* knappen definitionen innan den befintliga *AddItem* knappen:</span><span class="sxs-lookup"><span data-stu-id="99c65-141">In the Activity_To_Do.axml file, add the following *LoginUser* button definition before the existing *AddItem* button:</span></span>
   
          <Button
            android:id="@+id/buttonLoginUser"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:onClick="LoginUser"
            android:text="@string/login_button_text" />
4. <span data-ttu-id="99c65-142">Lägg till följande element i filen Strings.xml resurser:</span><span class="sxs-lookup"><span data-stu-id="99c65-142">Add the following element to the Strings.xml resources file:</span></span>
   
        <string name="login_button_text">Sign in</string>
5. <span data-ttu-id="99c65-143">Öppna filen AndroidManifest.xml, Lägg till följande kod inuti `<application>` XML-elementet:</span><span class="sxs-lookup"><span data-stu-id="99c65-143">Open the AndroidManifest.xml file, add the following code inside `<application>` XML element:</span></span>

        <activity android:name="com.microsoft.windowsazure.mobileservices.authentication.RedirectUrlActivity" android:launchMode="singleTop" android:noHistory="true">
          <intent-filter>
            <action android:name="android.intent.action.VIEW" />
            <category android:name="android.intent.category.DEFAULT" />
            <category android:name="android.intent.category.BROWSABLE" />
            <data android:scheme="{url_scheme_of_your_app}" android:host="easyauth.callback" />
          </intent-filter>
        </activity>

6. <span data-ttu-id="99c65-144">I Visual Studio eller Xamarin Studio kör klientprojektet på en enhet eller emulator och logga in med ditt valda identitetsleverantör.</span><span class="sxs-lookup"><span data-stu-id="99c65-144">In Visual Studio or Xamarin Studio, run the client project on a device or emulator and sign in with your chosen identity provider.</span></span> <span data-ttu-id="99c65-145">När du har loggat in, visas inloggnings-ID och listan över todo-objekt och du kan göra uppdateringar till data.</span><span class="sxs-lookup"><span data-stu-id="99c65-145">When you are successfully logged-in, the app will display your login ID and the list of todo items, and you can make updates to the data.</span></span>

<!-- URLs. -->
<span data-ttu-id="99c65-146">[skapa en Xamarin.Android-app]: app-service-mobile-xamarin-android-get-started.md</span><span class="sxs-lookup"><span data-stu-id="99c65-146">[Create a Xamarin.Android app]: app-service-mobile-xamarin-android-get-started.md</span></span>