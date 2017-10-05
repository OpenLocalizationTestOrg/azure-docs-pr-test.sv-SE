---
title: "Lägg till autentisering i appen universella Windowsplattformen (UWP) | Microsoft Docs"
description: "Lär dig hur du använder Azure Apptjänst Mobilappar för att autentisera användare i appen universella Windowsplattformen (UWP) med olika identitetsleverantörer, inklusive: AAD, Google, Facebook, Twitter och Microsoft."
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
ms.openlocfilehash: 47da343d4ec956ec2e669757f56e853675f887a3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="add-authentication-to-your-windows-app"></a><span data-ttu-id="9ecb5-103">Lägg till autentisering i Windows-appen</span><span class="sxs-lookup"><span data-stu-id="9ecb5-103">Add authentication to your Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="9ecb5-104">Det här avsnittet visar hur du lägger till molnbaserad autentisering till din mobilapp.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-104">This topic shows you how to add cloud-based authentication to your mobile app.</span></span> <span data-ttu-id="9ecb5-105">I kursen får du lägger till autentisering snabbstartsprojekt universella Windowsplattformen (UWP) för Mobilappar som använder en identitetsleverantör som stöds av Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-105">In this tutorial, you add authentication to the Universal Windows Platform (UWP) quickstart project for Mobile Apps using an identity provider that is supported by Azure App Service.</span></span> <span data-ttu-id="9ecb5-106">Efter att kunna autentiseras och auktoriseras av din mobilappsserverdel visas användar-ID-värdet.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-106">After being successfully authenticated and authorized by your Mobile App backend, the user ID value is displayed.</span></span>

<span data-ttu-id="9ecb5-107">Den här kursen är baserad på Mobile Apps-Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-107">This tutorial is based on the Mobile Apps quickstart.</span></span> <span data-ttu-id="9ecb5-108">Du måste slutföra kursen [Kom igång med Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="9ecb5-108">You must first complete the tutorial [Get started with Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span></span>

## <span data-ttu-id="9ecb5-109"><a name="register"></a>Registrera din app för autentisering och konfigurera App Service</span><span class="sxs-lookup"><span data-stu-id="9ecb5-109"><a name="register"></a>Register your app for authentication and configure the App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="9ecb5-110"><a name="redirecturl"></a>Lägg till din app i tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="9ecb5-110"><a name="redirecturl"></a>Add your app to the Allowed External Redirect URLs</span></span>

<span data-ttu-id="9ecb5-111">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-111">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="9ecb5-112">Detta gör att autentiseringssystemet kan omdirigera tillbaka till din app när autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-112">This allows the authentication system to redirect back to your app once the authentication process is complete.</span></span> <span data-ttu-id="9ecb5-113">I den här självstudiekursen kommer vi använda URL-schemat _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-113">In this tutorial, we use the URL scheme _appname_ throughout.</span></span> <span data-ttu-id="9ecb5-114">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-114">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="9ecb5-115">Det bör vara unikt för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-115">It should be unique to your mobile application.</span></span> <span data-ttu-id="9ecb5-116">Du vill aktivera omdirigering på serversidan:</span><span class="sxs-lookup"><span data-stu-id="9ecb5-116">To enable the redirection on the server side:</span></span>

1. <span data-ttu-id="9ecb5-117">Välj din Apptjänst i [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="9ecb5-117">In the [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="9ecb5-118">Klicka på den **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-118">Click the **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="9ecb5-119">I den **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-119">In the **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="9ecb5-120">Den **url_scheme_of_your_app** i den här strängen är URL-schemat för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-120">The **url_scheme_of_your_app** in this string is the URL Scheme for your mobile application.</span></span>  <span data-ttu-id="9ecb5-121">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="9ecb5-121">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="9ecb5-122">Du bör anteckna den sträng som du väljer när du behöver justera mobilprogram koden med URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-122">You should make a note of the string that you choose as you will need to adjust your mobile application code with the URL Scheme in several places.</span></span>

4. <span data-ttu-id="9ecb5-123">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-123">Click **OK**.</span></span>

5. <span data-ttu-id="9ecb5-124">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-124">Click **Save**.</span></span>

## <span data-ttu-id="9ecb5-125"><a name="permissions"></a>Begränsa behörighet för autentiserade användare</span><span class="sxs-lookup"><span data-stu-id="9ecb5-125"><a name="permissions"></a>Restrict permissions to authenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="9ecb5-126">Nu kan du kontrollera att anonym åtkomst till din serverdel har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-126">Now, you can verify that anonymous access to your backend has been disabled.</span></span> <span data-ttu-id="9ecb5-127">Med UWP-appsprojektet som start-projektet, distribuera och köra appen; Kontrollera att ett undantag med en statuskod 401 (obehörig) aktiveras när appen startar.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-127">With the UWP app project set as the start-up project, deploy and run the app; verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after the app starts.</span></span> <span data-ttu-id="9ecb5-128">Detta beror på att appen försöker få åtkomst till din mobila Appkod som en oautentiserad användare men *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-128">This happens because the app attempts to access your Mobile App Code as an unauthenticated user, but the *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="9ecb5-129">Därefter uppdaterar du appen för att autentisera användare innan du begär resurser från din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-129">Next, you will update the app to authenticate users before requesting resources from your App Service.</span></span>

## <span data-ttu-id="9ecb5-130"><a name="add-authentication"></a>Lägg till autentisering i appen</span><span class="sxs-lookup"><span data-stu-id="9ecb5-130"><a name="add-authentication"></a>Add authentication to the app</span></span>
1. <span data-ttu-id="9ecb5-131">I UWP app projekt filen MainPage.xaml.cs och Lägg till följande kodfragment:</span><span class="sxs-lookup"><span data-stu-id="9ecb5-131">In the UWP app project file MainPage.xaml.cs and add the following code snippet:</span></span>
   
        // Define a member variable for storing the signed-in user. 
        private MobileServiceUser user;
   
        // Define a method that performs the authentication process
        // using a Facebook sign-in. 
        private async System.Threading.Tasks.Task<bool> AuthenticateAsync()
        {
            string message;
            bool success = false;
            try
            {
                // Change 'MobileService' to the name of your MobileServiceClient instance.
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
   
    <span data-ttu-id="9ecb5-132">Den här koden autentiserar användaren med en inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-132">This code authenticates the user with a Facebook login.</span></span> <span data-ttu-id="9ecb5-133">Om du använder en identitetsleverantör än Facebook, ändrar du värdet för **MobileServiceAuthenticationProvider** ovan med värdet för leverantören.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-133">If you are using an identity provider other than Facebook, change the value of **MobileServiceAuthenticationProvider** above to the value for your provider.</span></span>
2. <span data-ttu-id="9ecb5-134">Ersätt den **OnNavigatedTo()** metod i MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-134">Replace the **OnNavigatedTo()** method in MainPage.xaml.cs.</span></span> <span data-ttu-id="9ecb5-135">Sedan lägger du till en **inloggning** knappen till appen som utlöser verifiering.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-135">Next, you will add a **Sign in** button to the app that triggers authentication.</span></span>

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. <span data-ttu-id="9ecb5-136">Lägg till följande kodavsnitt i MainPage.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="9ecb5-136">Add the following code snippet to the MainPage.xaml.cs:</span></span>
   
        private async void ButtonLogin_Click(object sender, RoutedEventArgs e)
        {
            // Login the user and then load data from the mobile app.
            if (await AuthenticateAsync())
            {
                // Switch the buttons and load items from the mobile app.
                ButtonLogin.Visibility = Visibility.Collapsed;
                ButtonSave.Visibility = Visibility.Visible;
                //await InitLocalStoreAsync(); //offline sync support.
                await RefreshTodoItems();
            }
        }
4. <span data-ttu-id="9ecb5-137">Öppna projektfilen MainPage.xaml, leta upp det element som definierar den **spara** knappen och Ersätt den med följande kod:</span><span class="sxs-lookup"><span data-stu-id="9ecb5-137">Open the MainPage.xaml project file, locate the element that defines the **Save** button and replace it with the following code:</span></span>
   
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
5. <span data-ttu-id="9ecb5-138">Lägg till följande kodavsnitt i App.xaml.cs:</span><span class="sxs-lookup"><span data-stu-id="9ecb5-138">Add the following code snippet to the App.xaml.cs:</span></span>

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
6. <span data-ttu-id="9ecb5-139">Öppna filen Package.appxmanifest, gå till **deklarationer**i **tillgängliga deklarationer** listrutan, Välj **protokollet** och klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-139">Open Package.appxmanifest file, navigate to **Declarations**, in **Available Declarations** dropdown list, select **Protocol** and click **Add** button.</span></span> <span data-ttu-id="9ecb5-140">Nu konfigurera den **egenskaper** av den **protokollet** deklaration.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-140">Now configure the **Properties** of the **Protocol** declaration.</span></span> <span data-ttu-id="9ecb5-141">I **visningsnamn**, lägga till namn som du vill visa användare av ditt program.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-141">In **Display name**, add the name you wish to display to users of your application.</span></span> <span data-ttu-id="9ecb5-142">I **namn**, lägga till {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-142">In **Name**, add your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="9ecb5-143">Tryck på F5 för att köra appen klickar du på den **inloggning** knappen och logga in i appen med din valda identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-143">Press the F5 key to run the app, click the **Sign in** button, and sign into the app with your chosen identity provider.</span></span> <span data-ttu-id="9ecb5-144">När inloggningen är klar, appen körs utan fel och du kan fråga din serverdel och göra uppdateringar till data.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-144">After your sign-in is successful, the app runs without errors and you are able to query your backend and make updates to data.</span></span>

## <span data-ttu-id="9ecb5-145"><a name="tokens"></a>Lagra autentiseringstoken på klienten</span><span class="sxs-lookup"><span data-stu-id="9ecb5-145"><a name="tokens"></a>Store the authentication token on the client</span></span>
<span data-ttu-id="9ecb5-146">Föregående exempel visade en standard inloggning, vilket kräver att klienten kontakta både identitetsleverantören och App Service varje gång appen startar.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-146">The previous example showed a standard sign-in, which requires the client to contact both the identity provider and the App Service every time that the app starts.</span></span> <span data-ttu-id="9ecb5-147">Är inte bara den här metoden ineffektiv, kan du köra till användning-relaterar problem bör många kunder försöker starta appen på samma gång.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-147">Not only is this method inefficient, you can run into usage-relates issues should many customers try to start you app at the same time.</span></span> <span data-ttu-id="9ecb5-148">En bättre metod är att cachelagra den autentiseringstoken som returneras av App Service och försök att använda den här första innan du använder en provider-baserad inloggning.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-148">A better approach is to cache the authorization token returned by your App Service and try to use this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="9ecb5-149">Du kan cachelagra den token som utfärdas av Apptjänster oavsett om du använder klient-hanteras eller service autentisering.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-149">You can cache the token issued by App Services regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="9ecb5-150">Den här kursen använder autentisering som hanteras av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-150">This tutorial uses service-managed authentication.</span></span>
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="9ecb5-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9ecb5-151">Next steps</span></span>
<span data-ttu-id="9ecb5-152">Nu när du har slutfört den här självstudiekursen för grundläggande autentisering, Överväg fortsätter in på något av följande kurser:</span><span class="sxs-lookup"><span data-stu-id="9ecb5-152">Now that you completed this basic authentication tutorial, consider continuing on to one of the following tutorials:</span></span>

* [<span data-ttu-id="9ecb5-153">Lägg till push-meddelanden i appen</span><span class="sxs-lookup"><span data-stu-id="9ecb5-153">Add push notifications to your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="9ecb5-154">Läs om hur du lägger till stöd för push-meddelanden i appen och konfigurerar serverdelen för mobilappen så att Azure Notification Hubs används för att skicka push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-154">Learn how to add push notifications support to your app and configure your Mobile App backend to use Azure Notification Hubs to send push notifications.</span></span>
* [<span data-ttu-id="9ecb5-155">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="9ecb5-155">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="9ecb5-156">Läs om hur du lägger till offlinestöd i appen genom en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-156">Learn how to add offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="9ecb5-157">Med offlinesynkronisering kan slutanvändarna interagera med mobilappen och &mdash;se, lägga till och ändra data&mdash; även när det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="9ecb5-157">Offline sync allows end-users to interact with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
