---
title: "Hur du konfigurerar Facebook-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur du konfigurerar Facebook-autentisering för tillämpningsprogrammet Apptjänster."
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
ms.openlocfilehash: c1b4c91d384c56c4f55bf8d31ced250f51c0d837
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-facebook-login"></a><span data-ttu-id="f57cf-103">Så här konfigurerar du App Service-programmet för att använda Facebook-inloggning</span><span class="sxs-lookup"><span data-stu-id="f57cf-103">How to configure your App Service application to use Facebook login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="f57cf-104">Det här avsnittet visar hur du konfigurerar Azure App Service för att använda Facebook som en autentiseringsprovider.</span><span class="sxs-lookup"><span data-stu-id="f57cf-104">This topic shows you how to configure Azure App Service to use Facebook as an authentication provider.</span></span>

<span data-ttu-id="f57cf-105">Du måste ha en Facebook-konto som har en verifierad e-postadress och ett mobiltelefonnummer för att slutföra proceduren i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="f57cf-105">To complete the procedure in this topic, you must have a Facebook account that has a verified email address and a mobile phone number.</span></span> <span data-ttu-id="f57cf-106">Om du vill skapa ett nytt Facebook-konto går du till [facebook.com].</span><span class="sxs-lookup"><span data-stu-id="f57cf-106">To create a new Facebook account, go to [facebook.com].</span></span>

## <span data-ttu-id="f57cf-107"><a name="register"></a>Registrera programmet med Facebook</span><span class="sxs-lookup"><span data-stu-id="f57cf-107"><a name="register"> </a>Register your application with Facebook</span></span>
1. <span data-ttu-id="f57cf-108">Logga in på den [Azure-portalen], och navigera till programmet.</span><span class="sxs-lookup"><span data-stu-id="f57cf-108">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="f57cf-109">Kopiera ditt **URL**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-109">Copy your **URL**.</span></span> <span data-ttu-id="f57cf-110">Du använder detta för att konfigurera Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="f57cf-110">You will use this to configure your Facebook app.</span></span>
2. <span data-ttu-id="f57cf-111">I ett nytt webbläsarfönster navigerar du till den [Facebook utvecklare] webbplatsen och logga in med dina Facebook kontoautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="f57cf-111">In another browser window, navigate to the [Facebook Developers] website and sign-in with your Facebook account credentials.</span></span>
3. <span data-ttu-id="f57cf-112">(Valfritt) Om du inte redan har registrerat, klickar du på **appar** > **registrera som en utvecklare**, acceptera principen och gör registreringen.</span><span class="sxs-lookup"><span data-stu-id="f57cf-112">(Optional) If you have not already registered, click **Apps** > **Register as a Developer**, then accept the policy and follow the registration steps.</span></span>
4. <span data-ttu-id="f57cf-113">Klicka på **Mina appar** > **lägga till en ny App** > **webbplats** > **hoppa över och skapa App-ID**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-113">Click **My Apps** > **Add a New App** > **Website** > **Skip and Create App ID**.</span></span> 
5. <span data-ttu-id="f57cf-114">I **visningsnamn**, ange ett unikt namn för din app typen din **kontakta e-post**, Välj en **kategori** för din app, klicka sedan på **skapa App-ID**och slutföra säkerhetskontrollen.</span><span class="sxs-lookup"><span data-stu-id="f57cf-114">In **Display Name**, type a unique name for your app, type your **Contact Email**, choose a **Category** for your app, then click **Create App ID** and complete the security check.</span></span> <span data-ttu-id="f57cf-115">Då kommer du till instrumentpanelen för utvecklare för din nya Facebook-app.</span><span class="sxs-lookup"><span data-stu-id="f57cf-115">This takes you to the developer dashboard for your new Facebook app.</span></span>
6. <span data-ttu-id="f57cf-116">Klicka på under ”Facebook inloggning”, **Kom igång**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-116">Under "Facebook Login," click **Get Started**.</span></span> <span data-ttu-id="f57cf-117">Lägg till programmets **omdirigerings-URI** till **omdirigerings-URI: er för giltig OAuth**, klicka på **spara ändringar**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-117">Add your application's **Redirect URI** to **Valid OAuth redirect URIs**, then click **Save Changes**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="f57cf-118">Din omdirigering URI är URL-Adressen till ditt program läggas till med sökvägen */.auth/login/facebook/callback*.</span><span class="sxs-lookup"><span data-stu-id="f57cf-118">Your redirect URI is the URL of your application appended with the path, */.auth/login/facebook/callback*.</span></span> <span data-ttu-id="f57cf-119">Till exempel `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span><span class="sxs-lookup"><span data-stu-id="f57cf-119">For example, `https://contoso.azurewebsites.net/.auth/login/facebook/callback`.</span></span> <span data-ttu-id="f57cf-120">Kontrollera att du använder HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="f57cf-120">Make sure that you are using the HTTPS scheme.</span></span>
   > 
   > 
7. <span data-ttu-id="f57cf-121">I det vänstra navigeringsfönstret klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-121">In the left-hand navigation, click **Settings**.</span></span> <span data-ttu-id="f57cf-122">På den **App hemlighet** klickar **visa**, ange ditt lösenord om det begärs och anteckna värdena för **App-ID** och **App hemlighet** .</span><span class="sxs-lookup"><span data-stu-id="f57cf-122">On the **App Secret** field, click **Show**, provide your password if requested, then make a note of the values of **App ID** and **App Secret**.</span></span> <span data-ttu-id="f57cf-123">Du använder dessa senare för att konfigurera ditt program i Azure.</span><span class="sxs-lookup"><span data-stu-id="f57cf-123">You use these later to configure your application in Azure.</span></span>
   
   > [!IMPORTANT]
   > <span data-ttu-id="f57cf-124">Hemligheten som app är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="f57cf-124">The app secret is an important security credential.</span></span> <span data-ttu-id="f57cf-125">Dela den här hemligheten med någon eller inte distribuera inom ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="f57cf-125">Do not share this secret with anyone or distribute it within a client application.</span></span>
   > 
   > 
8. <span data-ttu-id="f57cf-126">Facebook-konto som användes för att registrera programmet är administratör för appen.</span><span class="sxs-lookup"><span data-stu-id="f57cf-126">The Facebook account which was used to register the application is an administrator of the app.</span></span> <span data-ttu-id="f57cf-127">Endast administratörer kan nu logga in på det här programmet.</span><span class="sxs-lookup"><span data-stu-id="f57cf-127">At this point, only administrators can sign into this application.</span></span> <span data-ttu-id="f57cf-128">För att autentisera andra Facebook-konton klickar du på **App granska** och aktivera **Se < appens-namn > offentliga** att aktivera allmän allmän åtkomst med hjälp av Facebook-autentisering.</span><span class="sxs-lookup"><span data-stu-id="f57cf-128">To authenticate other Facebook accounts, click **App Review** and enable **Make <your-app-name> public** to enable general public access using Facebook authentication.</span></span>

## <span data-ttu-id="f57cf-129"><a name="secrets"></a>Lägg till Facebook-information för ditt program</span><span class="sxs-lookup"><span data-stu-id="f57cf-129"><a name="secrets"> </a>Add Facebook information to your application</span></span>
1. <span data-ttu-id="f57cf-130">I den [Azure-portalen], navigera till programmet.</span><span class="sxs-lookup"><span data-stu-id="f57cf-130">Back in the [Azure portal], navigate to your application.</span></span> <span data-ttu-id="f57cf-131">Klicka på **inställningar** > **autentisering / auktorisering**, och se till att **App autentiseringen av tjänsten** är **på**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-131">Click **Settings** > **Authentication / Authorization**, and make sure that **App Service Authentication** is **On**.</span></span>
2. <span data-ttu-id="f57cf-132">Klicka på **Facebook**, klistra in i appen hemligheten och App-ID-värden som du fick tidigare, om du vill aktivera alla scope som behövs i programmet och sedan klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-132">Click **Facebook**, paste in the App ID and App Secret values which you obtained previously, optionally enable any scopes needed by your application, then click **OK**.</span></span>
   
    ![][0]
   
    <span data-ttu-id="f57cf-133">Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst till webbplatsens innehåll och API: er.</span><span class="sxs-lookup"><span data-stu-id="f57cf-133">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="f57cf-134">Du måste auktorisera användare i din Appkod.</span><span class="sxs-lookup"><span data-stu-id="f57cf-134">You must authorize users in your app code.</span></span>
3. <span data-ttu-id="f57cf-135">(Valfritt) Om du vill begränsa åtkomsten till din webbplats till enbart användare som autentiseras av Facebook, ange **åtgärd att vidta när begäran inte har autentiserats** till **Facebook**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-135">(Optional) To restrict access to your site to only users authenticated by Facebook, set **Action to take when request is not authenticated** to **Facebook**.</span></span> <span data-ttu-id="f57cf-136">Detta kräver att alla förfrågningar autentiseras och alla oautentiserade begäranden omdirigeras till Facebook för autentisering.</span><span class="sxs-lookup"><span data-stu-id="f57cf-136">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Facebook for authentication.</span></span>
4. <span data-ttu-id="f57cf-137">När konfigurera autentisering klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="f57cf-137">When done configuring authentication, click **Save**.</span></span>

<span data-ttu-id="f57cf-138">Du är nu redo att använda Facebook för autentisering i appen.</span><span class="sxs-lookup"><span data-stu-id="f57cf-138">You are now ready to use Facebook for authentication in your app.</span></span>

## <span data-ttu-id="f57cf-139"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="f57cf-139"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->
[0]: ./media/app-service-mobile-how-to-configure-facebook-authentication/mobile-app-facebook-settings.png

<!-- URLs. -->
<span data-ttu-id="f57cf-140">[Facebook utvecklare]: http://go.microsoft.com/fwlink/p/?LinkId=268286</span><span class="sxs-lookup"><span data-stu-id="f57cf-140">[Facebook Developers]: http://go.microsoft.com/fwlink/p/?LinkId=268286</span></span>
<span data-ttu-id="f57cf-141">[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285</span><span class="sxs-lookup"><span data-stu-id="f57cf-141">[facebook.com]: http://go.microsoft.com/fwlink/p/?LinkId=268285</span></span>
[Get started with authentication]: /en-us/develop/mobile/tutorials/get-started-with-users-dotnet/
<span data-ttu-id="f57cf-142">[Azure-portalen]: https://portal.azure.com/</span><span class="sxs-lookup"><span data-stu-id="f57cf-142">[Azure portal]: https://portal.azure.com/</span></span>
