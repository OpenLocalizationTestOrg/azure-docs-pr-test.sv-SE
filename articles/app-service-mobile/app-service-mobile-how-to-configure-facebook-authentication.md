---
title: "aaaHow tooconfigure Facebook-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Facebook-autentisering för din App Services-tjänstprogrammet."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: b6b4f062-fcb4-47b3-b75a-ec4cb51a62fd
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 53d03445a2ad17de1d2f69f5e770d14385b48ad4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-facebook-login"></a><span data-ttu-id="eaf4a-103">Hur tooconfigure din Apptjänst programmet toouse Facebook-inloggning</span><span class="sxs-lookup"><span data-stu-id="eaf4a-103">How tooconfigure your App Service application toouse Facebook login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="eaf4a-104">Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Facebook som en autentiseringsprovider.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-104">This topic shows you how tooconfigure Azure App Service toouse Facebook as an authentication provider.</span></span>

<span data-ttu-id="eaf4a-105">Du måste ha en Facebook-konto som har en verifierad e-postadress och ett mobiltelefonnummer toocomplete hello proceduren i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-105">toocomplete hello procedure in this topic, you must have a Facebook account that has a verified email address and a mobile phone number.</span></span> <span data-ttu-id="eaf4a-106">Gå toocreate ett nytt Facebook-konto för[facebook.com].</span><span class="sxs-lookup"><span data-stu-id="eaf4a-106">toocreate a new Facebook account, go too[facebook.com].</span></span>

## <span data-ttu-id="eaf4a-107"><a name="register"></a>Registrera programmet med Facebook</span><span class="sxs-lookup"><span data-stu-id="eaf4a-107"><a name="register"> </a>Register your application with Facebook</span></span>
1. <span data-ttu-id="eaf4a-108">Logga in toohello [Azure-portalen], och navigera tooyour program.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="eaf4a-109">Kopiera ditt **URL**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-109">Copy your **URL**.</span></span> <span data-ttu-id="eaf4a-110">Du använder den här tooconfigure Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-110">You will use this tooconfigure your Facebook app.</span></span>
2. <span data-ttu-id="eaf4a-111">Navigera i ett nytt webbläsarfönster toohello [Facebook utvecklare] webbplatsen och logga in med dina Facebook kontoautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-111">In another browser window, navigate toohello [Facebook Developers] website and sign-in with your Facebook account credentials.</span></span>
3. <span data-ttu-id="eaf4a-112">(Valfritt) Om du inte redan har registrerat, klickar du på **appar** > **registrera som en utvecklare**, acceptera hello princip och gör hello-registrering.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-112">(Optional) If you have not already registered, click **Apps** > **Register as a Developer**, then accept hello policy and follow hello registration steps.</span></span>
4. <span data-ttu-id="eaf4a-113">Klicka på **Mina appar** > **lägga till en ny App** > **webbplats** > **hoppa över och skapa App-ID**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-113">Click **My Apps** > **Add a New App** > **Website** > **Skip and Create App ID**.</span></span> 
5. <span data-ttu-id="eaf4a-114">I **visningsnamn**, ange ett unikt namn för din app typen din **kontakta e-post**, Välj en **kategori** för din app, klicka sedan på **skapa App-ID**och slutföra hello säkerhetskontrollen.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-114">In **Display Name**, type a unique name for your app, type your **Contact Email**, choose a **Category** for your app, then click **Create App ID** and complete hello security check.</span></span> <span data-ttu-id="eaf4a-115">Då kommer du instrumentpanelen för utvecklare av toohello för din nya Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-115">This takes you toohello developer dashboard for your new Facebook app.</span></span>
6. <span data-ttu-id="eaf4a-116">Klicka på under ”Facebook inloggning”, **Kom igång**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-116">Under "Facebook Login," click **Get Started**.</span></span> <span data-ttu-id="eaf4a-117">Lägg till programmets **omdirigerings-URI** för**omdirigerings-URI: er för giltig OAuth**, klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-117">Add your application's **Redirect URI** too**Valid OAuth redirect URIs**, then click **Save Changes**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="eaf4a-118">Din omdirigering URI är hello URL för ditt program läggas till med hello sökvägen */.auth/login/facebook/callback*.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-118">Your redirect URI is hello URL of your application appended with hello path, */.auth/login/facebook/callback*.</span></span> <span data-ttu-id="eaf4a-119">Till exempel `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-119">For example, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span></span> <span data-ttu-id="eaf4a-120">Kontrollera att du använder hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-120">Make sure that you are using hello HTTPS scheme.</span></span>
   > 
   > 
7. <span data-ttu-id="eaf4a-121">I hello vänstra navigeringsfönstret klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-121">In hello left-hand navigation, click **Settings**.</span></span> <span data-ttu-id="eaf4a-122">På hello **App hemlighet** klickar **visa**, ange ditt lösenord om det begärs och anteckna hello värdena för **App-ID** och **App hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-122">On hello **App Secret** field, click **Show**, provide your password if requested, then make a note of hello values of **App ID** and **App Secret**.</span></span> <span data-ttu-id="eaf4a-123">Använd dessa senare tooconfigure ditt program i Azure.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-123">You use these later tooconfigure your application in Azure.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="eaf4a-124">hello app hemlighet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-124">hello app secret is an important security credential.</span></span> <span data-ttu-id="eaf4a-125">Dela den här hemligheten med någon eller inte distribuera inom ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-125">Do not share this secret with anyone or distribute it within a client application.</span></span>
   > 
   > 
8. <span data-ttu-id="eaf4a-126">hello Facebook-konto som har använt tooregister hello program är administratör för hello app.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-126">hello Facebook account which was used tooregister hello application is an administrator of hello app.</span></span> <span data-ttu-id="eaf4a-127">Endast administratörer kan nu logga in på det här programmet.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-127">At this point, only administrators can sign into this application.</span></span> <span data-ttu-id="eaf4a-128">tooauthenticate andra Facebook-konton klickar du på **App granska** och aktivera **Se < appens-namn > offentliga** tooenable allmän allmän åtkomst med hjälp av Facebook-autentisering.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-128">tooauthenticate other Facebook accounts, click **App Review** and enable **Make <your-app-name> public** tooenable general public access using Facebook authentication.</span></span>

## <span data-ttu-id="eaf4a-129"><a name="secrets"></a>Lägg till Facebook information tooyour program</span><span class="sxs-lookup"><span data-stu-id="eaf4a-129"><a name="secrets"> </a>Add Facebook information tooyour application</span></span>
1. <span data-ttu-id="eaf4a-130">Tillbaka i hello [Azure-portalen], navigera tooyour program.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-130">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="eaf4a-131">Klicka på **inställningar** > **autentisering / auktorisering**, och se till att **App autentiseringen av tjänsten** är **på**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-131">Click **Settings** > **Authentication / Authorization**, and make sure that **App Service Authentication** is **On**.</span></span>
2. <span data-ttu-id="eaf4a-132">Klicka på **Facebook**, klistra in i hello App-ID och App hemlighet värden som du hämtade tidigare, om du vill aktivera alla scope som behövs i programmet och sedan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-132">Click **Facebook**, paste in hello App ID and App Secret values which you obtained previously, optionally enable any scopes needed by your application, then click **OK**.</span></span>
   
    ![][0]
   
    <span data-ttu-id="eaf4a-133">Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-133">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="eaf4a-134">Du måste auktorisera användare i din Appkod.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-134">You must authorize users in your app code.</span></span>
3. <span data-ttu-id="eaf4a-135">(Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Facebook, ange **åtgärd tootake när begäran inte har autentiserats** för**Facebook**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-135">(Optional) toorestrict access tooyour site tooonly users authenticated by Facebook, set **Action tootake when request is not authenticated** too**Facebook**.</span></span> <span data-ttu-id="eaf4a-136">Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooFacebook för autentisering.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-136">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooFacebook for authentication.</span></span>
4. <span data-ttu-id="eaf4a-137">När konfigurera autentisering klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-137">When done configuring authentication, click **Save**.</span></span>

<span data-ttu-id="eaf4a-138">Nu är du redo toouse Facebook för autentisering i appen.</span><span class="sxs-lookup"><span data-stu-id="eaf4a-138">You are now ready toouse Facebook for authentication in your app.</span></span>

## <span data-ttu-id="eaf4a-139"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="eaf4a-139"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
[Facebook utvecklare]: http://go.microsoft.com/fwlink/p/?LinkId=268286
[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
[Azure-portalen]: https://portal.azure.com/
