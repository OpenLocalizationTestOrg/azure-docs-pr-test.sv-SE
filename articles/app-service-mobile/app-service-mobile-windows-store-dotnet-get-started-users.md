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
# <a name="add-authentication-tooyour-windows-app"></a><span data-ttu-id="e56d9-103">Lägg till autentisering tooyour Windows-app</span><span class="sxs-lookup"><span data-stu-id="e56d9-103">Add authentication tooyour Windows app</span></span>
[!INCLUDE [app-service-mobile-selector-get-started-users](../../includes/app-service-mobile-selector-get-started-users.md)]

<span data-ttu-id="e56d9-104">Det här avsnittet beskrivs hur du tooadd molnbaserad autentisering tooyour mobila appar.</span><span class="sxs-lookup"><span data-stu-id="e56d9-104">This topic shows you how tooadd cloud-based authentication tooyour mobile app.</span></span> <span data-ttu-id="e56d9-105">I kursen får du lägga till autentisering toohello universella Windowsplattformen (UWP) snabbstartsprojekt för Mobilappar som använder en identitetsleverantör som stöds av Azure App Service.</span><span class="sxs-lookup"><span data-stu-id="e56d9-105">In this tutorial, you add authentication toohello Universal Windows Platform (UWP) quickstart project for Mobile Apps using an identity provider that is supported by Azure App Service.</span></span> <span data-ttu-id="e56d9-106">Efter att kunna autentiseras och auktoriseras av din mobilappsserverdel visas hello användar-ID-värdet.</span><span class="sxs-lookup"><span data-stu-id="e56d9-106">After being successfully authenticated and authorized by your Mobile App backend, hello user ID value is displayed.</span></span>

<span data-ttu-id="e56d9-107">Den här kursen är baserad på hello Mobile Apps Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="e56d9-107">This tutorial is based on hello Mobile Apps quickstart.</span></span> <span data-ttu-id="e56d9-108">Du måste slutföra kursen hello [Kom igång med Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span><span class="sxs-lookup"><span data-stu-id="e56d9-108">You must first complete hello tutorial [Get started with Mobile Apps](app-service-mobile-windows-store-dotnet-get-started.md).</span></span>

## <span data-ttu-id="e56d9-109"><a name="register"></a>Registrera din app för autentisering och konfigurera hello Apptjänst</span><span class="sxs-lookup"><span data-stu-id="e56d9-109"><a name="register"></a>Register your app for authentication and configure hello App Service</span></span>
[!INCLUDE [app-service-mobile-register-authentication](../../includes/app-service-mobile-register-authentication.md)]

## <span data-ttu-id="e56d9-110"><a name="redirecturl"></a>Lägg till din app toohello tillåtna externa omdirigerings-URL</span><span class="sxs-lookup"><span data-stu-id="e56d9-110"><a name="redirecturl"></a>Add your app toohello Allowed External Redirect URLs</span></span>

<span data-ttu-id="e56d9-111">Säker autentisering måste du definiera en ny URL-schema för din app.</span><span class="sxs-lookup"><span data-stu-id="e56d9-111">Secure authentication requires that you define a new URL scheme for your app.</span></span> <span data-ttu-id="e56d9-112">Detta tillåter hello autentisering system tooredirect tillbaka tooyour appen när hello-autentiseringen är klar.</span><span class="sxs-lookup"><span data-stu-id="e56d9-112">This allows hello authentication system tooredirect back tooyour app once hello authentication process is complete.</span></span> <span data-ttu-id="e56d9-113">I den här självstudiekursen kommer vi använda hello URL-schema _appname_ i hela.</span><span class="sxs-lookup"><span data-stu-id="e56d9-113">In this tutorial, we use hello URL scheme _appname_ throughout.</span></span> <span data-ttu-id="e56d9-114">Du kan dock använda alla URL-schema som du väljer.</span><span class="sxs-lookup"><span data-stu-id="e56d9-114">However, you can use any URL scheme you choose.</span></span> <span data-ttu-id="e56d9-115">Det bör vara unikt tooyour mobila program.</span><span class="sxs-lookup"><span data-stu-id="e56d9-115">It should be unique tooyour mobile application.</span></span> <span data-ttu-id="e56d9-116">tooenable hello omdirigering på serversidan hello:</span><span class="sxs-lookup"><span data-stu-id="e56d9-116">tooenable hello redirection on hello server side:</span></span>

1. <span data-ttu-id="e56d9-117">I hello [Azure portal], väljer du din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="e56d9-117">In hello [Azure portal], select your App Service.</span></span>

2. <span data-ttu-id="e56d9-118">Klicka på hello **autentisering / auktorisering** menyalternativet.</span><span class="sxs-lookup"><span data-stu-id="e56d9-118">Click hello **Authentication / Authorization** menu option.</span></span>

3. <span data-ttu-id="e56d9-119">I hello **tillåtna externa omdirigerings-URL: er**, ange `url_scheme_of_your_app://easyauth.callback`.</span><span class="sxs-lookup"><span data-stu-id="e56d9-119">In hello **Allowed External Redirect URLs**, enter `url_scheme_of_your_app://easyauth.callback`.</span></span>  <span data-ttu-id="e56d9-120">Hej **url_scheme_of_your_app** i den här strängen är hello URL-schema för din mobila program.</span><span class="sxs-lookup"><span data-stu-id="e56d9-120">hello **url_scheme_of_your_app** in this string is hello URL Scheme for your mobile application.</span></span>  <span data-ttu-id="e56d9-121">Det bör följa den normala URL specifikation för ett protokoll (Använd bokstäver och siffror och börja med en bokstav).</span><span class="sxs-lookup"><span data-stu-id="e56d9-121">It should follow normal URL specification for a protocol (use letters and numbers only, and start with a letter).</span></span>  <span data-ttu-id="e56d9-122">Du bör anteckna hello sträng som du väljer när du behöver tooadjust mobilprogram koden med hello URL-schemat på flera platser.</span><span class="sxs-lookup"><span data-stu-id="e56d9-122">You should make a note of hello string that you choose as you will need tooadjust your mobile application code with hello URL Scheme in several places.</span></span>

4. <span data-ttu-id="e56d9-123">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e56d9-123">Click **OK**.</span></span>

5. <span data-ttu-id="e56d9-124">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e56d9-124">Click **Save**.</span></span>

## <span data-ttu-id="e56d9-125"><a name="permissions"></a>Begränsa behörigheter tooauthenticated användare</span><span class="sxs-lookup"><span data-stu-id="e56d9-125"><a name="permissions"></a>Restrict permissions tooauthenticated users</span></span>
[!INCLUDE [app-service-mobile-restrict-permissions-dotnet-backend](../../includes/app-service-mobile-restrict-permissions-dotnet-backend.md)]

<span data-ttu-id="e56d9-126">Nu kan du kontrollera att anonym åtkomst tooyour backend har inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="e56d9-126">Now, you can verify that anonymous access tooyour backend has been disabled.</span></span> <span data-ttu-id="e56d9-127">Med hello UWP-appsprojektet som hello uppstart projekt, distribuera och köra hello app; Kontrollera att ett undantag med en statuskod 401 (obehörig) utlöses efter hello appen startar.</span><span class="sxs-lookup"><span data-stu-id="e56d9-127">With hello UWP app project set as hello start-up project, deploy and run hello app; verify that an unhandled exception with a status code of 401 (Unauthorized) is raised after hello app starts.</span></span> <span data-ttu-id="e56d9-128">Detta beror på att hello app försöker tooaccess Mobile App koden som en oautentiserad användare, men hello *TodoItem* tabellen nu kräver autentisering.</span><span class="sxs-lookup"><span data-stu-id="e56d9-128">This happens because hello app attempts tooaccess your Mobile App Code as an unauthenticated user, but hello *TodoItem* table now requires authentication.</span></span>

<span data-ttu-id="e56d9-129">Därefter uppdaterar du hello app tooauthenticate användare innan du begär resurser från din Apptjänst.</span><span class="sxs-lookup"><span data-stu-id="e56d9-129">Next, you will update hello app tooauthenticate users before requesting resources from your App Service.</span></span>

## <span data-ttu-id="e56d9-130"><a name="add-authentication"></a>Lägg till autentisering toohello app</span><span class="sxs-lookup"><span data-stu-id="e56d9-130"><a name="add-authentication"></a>Add authentication toohello app</span></span>
1. <span data-ttu-id="e56d9-131">Filen MainPage.xaml.cs i hello UWP-appsprojektet och Lägg till följande kodfragment hello:</span><span class="sxs-lookup"><span data-stu-id="e56d9-131">In hello UWP app project file MainPage.xaml.cs and add hello following code snippet:</span></span>
   
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
   
    <span data-ttu-id="e56d9-132">Den här koden autentiserar hello användare med en inloggning med Facebook.</span><span class="sxs-lookup"><span data-stu-id="e56d9-132">This code authenticates hello user with a Facebook login.</span></span> <span data-ttu-id="e56d9-133">Om du använder en identitetsleverantör än Facebook, ändra hello värdet på **MobileServiceAuthenticationProvider** ovan toohello värde för leverantören.</span><span class="sxs-lookup"><span data-stu-id="e56d9-133">If you are using an identity provider other than Facebook, change hello value of **MobileServiceAuthenticationProvider** above toohello value for your provider.</span></span>
2. <span data-ttu-id="e56d9-134">Ersätt hello **OnNavigatedTo()** metod i MainPage.xaml.cs.</span><span class="sxs-lookup"><span data-stu-id="e56d9-134">Replace hello **OnNavigatedTo()** method in MainPage.xaml.cs.</span></span> <span data-ttu-id="e56d9-135">Sedan lägger du till en **inloggning** knappen toohello app som utlöser verifiering.</span><span class="sxs-lookup"><span data-stu-id="e56d9-135">Next, you will add a **Sign in** button toohello app that triggers authentication.</span></span>

        protected override async void OnNavigatedTo(NavigationEventArgs e)
        {
            if (e.Parameter is Uri)
            {
                App.MobileService.ResumeWithURL(e.Parameter as Uri);
            }
        }

3. <span data-ttu-id="e56d9-136">Lägg till följande kodfragment toohello MainPage.xaml.cs hello:</span><span class="sxs-lookup"><span data-stu-id="e56d9-136">Add hello following code snippet toohello MainPage.xaml.cs:</span></span>
   
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
4. <span data-ttu-id="e56d9-137">Öppna projektfilen för hello MainPage.xaml, leta upp hello-element som definierar hello **spara** knappen och Ersätt den med hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="e56d9-137">Open hello MainPage.xaml project file, locate hello element that defines hello **Save** button and replace it with hello following code:</span></span>
   
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
5. <span data-ttu-id="e56d9-138">Lägg till följande kodfragment toohello App.xaml.cs hello:</span><span class="sxs-lookup"><span data-stu-id="e56d9-138">Add hello following code snippet toohello App.xaml.cs:</span></span>

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
6. <span data-ttu-id="e56d9-139">Öppna filen Package.appxmanifest, navigera för**deklarationer**i **tillgängliga deklarationer** listrutan, Välj **protokollet** och klicka på **Lägg till** knappen.</span><span class="sxs-lookup"><span data-stu-id="e56d9-139">Open Package.appxmanifest file, navigate too**Declarations**, in **Available Declarations** dropdown list, select **Protocol** and click **Add** button.</span></span> <span data-ttu-id="e56d9-140">Nu konfigurera hello **egenskaper** av hello **protokollet** deklaration.</span><span class="sxs-lookup"><span data-stu-id="e56d9-140">Now configure hello **Properties** of hello **Protocol** declaration.</span></span> <span data-ttu-id="e56d9-141">I **visningsnamn**, lägga till hello namn som du vill toodisplay toousers för programmet.</span><span class="sxs-lookup"><span data-stu-id="e56d9-141">In **Display name**, add hello name you wish toodisplay toousers of your application.</span></span> <span data-ttu-id="e56d9-142">I **namn**, lägga till {url_scheme_of_your_app}.</span><span class="sxs-lookup"><span data-stu-id="e56d9-142">In **Name**, add your {url_scheme_of_your_app}.</span></span>
7. <span data-ttu-id="e56d9-143">Tryck på hello F5 viktiga toorun hello appen klickar du på hello **inloggning** knappen och logga in på hello appen med din valda identitetsprovider.</span><span class="sxs-lookup"><span data-stu-id="e56d9-143">Press hello F5 key toorun hello app, click hello **Sign in** button, and sign into hello app with your chosen identity provider.</span></span> <span data-ttu-id="e56d9-144">När inloggningen är klar, hello appen körs utan fel och du kan tooquery din serverdel och göra uppdateringar toodata.</span><span class="sxs-lookup"><span data-stu-id="e56d9-144">After your sign-in is successful, hello app runs without errors and you are able tooquery your backend and make updates toodata.</span></span>

## <span data-ttu-id="e56d9-145"><a name="tokens"></a>Lagra hello autentiseringstoken på hello-klient</span><span class="sxs-lookup"><span data-stu-id="e56d9-145"><a name="tokens"></a>Store hello authentication token on hello client</span></span>
<span data-ttu-id="e56d9-146">hello föregående exempel visade en standard inloggning, vilket kräver hello klienten toocontact båda hello identitetsleverantör och hello Apptjänst varje gång hello appen startas.</span><span class="sxs-lookup"><span data-stu-id="e56d9-146">hello previous example showed a standard sign-in, which requires hello client toocontact both hello identity provider and hello App Service every time that hello app starts.</span></span> <span data-ttu-id="e56d9-147">Är inte bara den här metoden ineffektiv, kan du köra till användning-relaterar problem många kunder försök toostart app på hello samtidigt.</span><span class="sxs-lookup"><span data-stu-id="e56d9-147">Not only is this method inefficient, you can run into usage-relates issues should many customers try toostart you app at hello same time.</span></span> <span data-ttu-id="e56d9-148">Är det bättre toocache hello autentiseringstoken returneras av din App Service och försök toouse detta innan du börjar använda en provider-baserad inloggning.</span><span class="sxs-lookup"><span data-stu-id="e56d9-148">A better approach is toocache hello authorization token returned by your App Service and try toouse this first before using a provider-based sign-in.</span></span>

> [!NOTE]
> <span data-ttu-id="e56d9-149">Du kan cachelagra hello-token som utfärdas av Apptjänster oavsett om du använder klient-hanteras eller service autentisering.</span><span class="sxs-lookup"><span data-stu-id="e56d9-149">You can cache hello token issued by App Services regardless of whether you are using client-managed or service-managed authentication.</span></span> <span data-ttu-id="e56d9-150">Den här kursen använder autentisering som hanteras av tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e56d9-150">This tutorial uses service-managed authentication.</span></span>
> 
> 

[!INCLUDE [mobile-windows-universal-dotnet-authenticate-app-with-token](../../includes/mobile-windows-universal-dotnet-authenticate-app-with-token.md)]

## <a name="next-steps"></a><span data-ttu-id="e56d9-151">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e56d9-151">Next steps</span></span>
<span data-ttu-id="e56d9-152">Nu när du har slutfört den här självstudiekursen för grundläggande autentisering, Överväg fortsätter tooone av hello följande kurser:</span><span class="sxs-lookup"><span data-stu-id="e56d9-152">Now that you completed this basic authentication tutorial, consider continuing on tooone of hello following tutorials:</span></span>

* [<span data-ttu-id="e56d9-153">Lägg till push-meddelanden tooyour app</span><span class="sxs-lookup"><span data-stu-id="e56d9-153">Add push notifications tooyour app</span></span>](app-service-mobile-windows-store-dotnet-get-started-push.md)  
  <span data-ttu-id="e56d9-154">Lär dig hur tooadd push-meddelanden stöder tooyour app och konfigurera din Mobilapp backend toouse Azure Notification Hubs toosend push-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="e56d9-154">Learn how tooadd push notifications support tooyour app and configure your Mobile App backend toouse Azure Notification Hubs toosend push notifications.</span></span>
* [<span data-ttu-id="e56d9-155">Aktivera offlinesynkronisering av appen</span><span class="sxs-lookup"><span data-stu-id="e56d9-155">Enable offline sync for your app</span></span>](app-service-mobile-windows-store-dotnet-get-started-offline-data.md)  
  <span data-ttu-id="e56d9-156">Lär dig hur tooadd offline stöder din app att använda en mobilappsserverdel.</span><span class="sxs-lookup"><span data-stu-id="e56d9-156">Learn how tooadd offline support your app using an Mobile App backend.</span></span> <span data-ttu-id="e56d9-157">Offlinesynkronisering kan slutanvändarna toointeract med en mobil app&mdash;visa, lägga till eller ändra data&mdash;även om det inte finns någon nätverksanslutning.</span><span class="sxs-lookup"><span data-stu-id="e56d9-157">Offline sync allows end-users toointeract with a mobile app&mdash;viewing, adding, or modifying data&mdash;even when there is no network connection.</span></span>

<!-- URLs. -->
[Get started with your mobile app]: app-service-mobile-windows-store-dotnet-get-started.md
