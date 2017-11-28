---
title: "Hur du konfigurerar Google-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur du konfigurerar Google-autentisering för tillämpningsprogrammet Apptjänster."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: 2b2f9abf-9120-4aac-ac5b-4a268d9b6e2b
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d6c1707f67d986487e5a45e76ffc9a02ddf16eb1
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-configure-your-app-service-application-to-use-google-login"></a><span data-ttu-id="3410b-103">Så här konfigurerar du din App tjänstprogram att använda Google-inloggning</span><span class="sxs-lookup"><span data-stu-id="3410b-103">How to configure your App Service application to use Google login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="3410b-104">Det här avsnittet visar hur du konfigurerar Azure App Service för att använda Google som en autentiseringsprovider.</span><span class="sxs-lookup"><span data-stu-id="3410b-104">This topic shows you how to configure Azure App Service to use Google as an authentication provider.</span></span>

<span data-ttu-id="3410b-105">Du måste ha ett Google-konto som har en verifierad e-postadress för att slutföra proceduren i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3410b-105">To complete the procedure in this topic, you must have a Google account that has a verified email address.</span></span> <span data-ttu-id="3410b-106">Gå till [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302) om du vill skapa ett nytt Google-konto.</span><span class="sxs-lookup"><span data-stu-id="3410b-106">To create a new Google account, go to [accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>

## <span data-ttu-id="3410b-107"><a name="register"></a>Registrera programmet med Google</span><span class="sxs-lookup"><span data-stu-id="3410b-107"><a name="register"> </a>Register your application with Google</span></span>
1. <span data-ttu-id="3410b-108">Logga in på den [Azure-portalen], och navigera till programmet.</span><span class="sxs-lookup"><span data-stu-id="3410b-108">Log on to the [Azure portal], and navigate to your application.</span></span> <span data-ttu-id="3410b-109">Kopiera ditt **URL**, som du senare använda för att konfigurera din app på Google.</span><span class="sxs-lookup"><span data-stu-id="3410b-109">Copy your **URL**, which you use later to configure your Google app.</span></span>
2. <span data-ttu-id="3410b-110">Navigera till den [Google API: er](http://go.microsoft.com/fwlink/p/?LinkId=268303) logga in med autentiseringsuppgifterna för ditt Google-konto, klickar du på **skapa projekt**, ange en **projektnamn**, klicka på  **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3410b-110">Navigate to the [Google apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) website, sign in with your Google account credentials, click **Create Project**, provide a **Project name**, then click **Create**.</span></span>
3. <span data-ttu-id="3410b-111">Under **sociala API: er** klickar du på **Google + API** och sedan **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="3410b-111">Under **Social APIs** click **Google+ API** and then **Enable**.</span></span>
4. <span data-ttu-id="3410b-112">I det vänstra navigeringsfönstret **autentiseringsuppgifter** > **OAuth-medgivande skärmen**och välj din **e-postadress**, ange en **produktnamn**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="3410b-112">In the left navigation, **Credentials** > **OAuth consent screen**, then select your **Email address**,  enter a **Product Name**, and click **Save**.</span></span>
5. <span data-ttu-id="3410b-113">I den **autentiseringsuppgifter** klickar du på **skapa autentiseringsuppgifter** > **OAuth-klient-ID**och välj **webbprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="3410b-113">In the **Credentials** tab, click **Create credentials** > **OAuth client ID**, then select **Web application**.</span></span>
6. <span data-ttu-id="3410b-114">Klistra in App Service **URL** du kopierade tidigare i **behörighet JavaScript ursprung**, klistra sedan in din omdirigering URI i **behörighet omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="3410b-114">Paste the App Service **URL** you copied earlier into **Authorized JavaScript Origins**, then paste your redirect URI into **Authorized Redirect URI**.</span></span> <span data-ttu-id="3410b-115">Omdirigerings-URI: N är URL-Adressen till ditt program läggas till med sökvägen */.auth/login/google/callback*.</span><span class="sxs-lookup"><span data-stu-id="3410b-115">The redirect URI is the URL of your application appended with the path, */.auth/login/google/callback*.</span></span> <span data-ttu-id="3410b-116">Till exempel `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span><span class="sxs-lookup"><span data-stu-id="3410b-116">For example, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span></span> <span data-ttu-id="3410b-117">Kontrollera att du använder HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="3410b-117">Make sure that you are using the HTTPS scheme.</span></span> <span data-ttu-id="3410b-118">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="3410b-118">Then click **Create**.</span></span>
7. <span data-ttu-id="3410b-119">Anteckna värdena för klient-ID och klienthemlighet på nästa skärm.</span><span class="sxs-lookup"><span data-stu-id="3410b-119">On the next screen, make a note of the values of the client ID and client secret.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="3410b-120">Klienthemligheten är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="3410b-120">The client secret is an important security credential.</span></span> <span data-ttu-id="3410b-121">Dela den här hemligheten med någon eller inte distribuera inom ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="3410b-121">Do not share this secret with anyone or distribute it within a client application.</span></span>


## <span data-ttu-id="3410b-122"><a name="secrets"></a>Lägga till Google information i programmet</span><span class="sxs-lookup"><span data-stu-id="3410b-122"><a name="secrets"> </a>Add Google information to your application</span></span>
1. <span data-ttu-id="3410b-123">I den [Azure-portalen], navigera till programmet.</span><span class="sxs-lookup"><span data-stu-id="3410b-123">Back in the [Azure portal], navigate to your application.</span></span> <span data-ttu-id="3410b-124">Klicka på **inställningar**, och sedan **autentisering / auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="3410b-124">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="3410b-125">Om autentisering / auktorisering är inte aktiverad, aktivera växeln **på**.</span><span class="sxs-lookup"><span data-stu-id="3410b-125">If the Authentication / Authorization feature is not enabled, turn the switch to **On**.</span></span>
3. <span data-ttu-id="3410b-126">Klicka på **Google**.</span><span class="sxs-lookup"><span data-stu-id="3410b-126">Click **Google**.</span></span> <span data-ttu-id="3410b-127">Klistra in i appen hemligheten och App-ID-värden som du hämtade tidigare och välja att aktivera alla scope som krävs för ditt program.</span><span class="sxs-lookup"><span data-stu-id="3410b-127">Paste in the App ID and App Secret values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="3410b-128">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3410b-128">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="3410b-129">Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst till webbplatsens innehåll och API: er.</span><span class="sxs-lookup"><span data-stu-id="3410b-129">By default, App Service provides authentication but does not restrict authorized access to your site content and APIs.</span></span> <span data-ttu-id="3410b-130">Du måste auktorisera användare i din Appkod.</span><span class="sxs-lookup"><span data-stu-id="3410b-130">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="3410b-131">(Valfritt) Om du vill begränsa åtkomsten till din webbplats till enbart användare som autentiseras av Google, ange **åtgärd att vidta när begäran inte har autentiserats** till **Google**.</span><span class="sxs-lookup"><span data-stu-id="3410b-131">(Optional) To restrict access to your site to only users authenticated by Google, set **Action to take when request is not authenticated** to **Google**.</span></span> <span data-ttu-id="3410b-132">Detta kräver att alla förfrågningar autentiseras och alla oautentiserade begäranden omdirigeras till Google för autentisering.</span><span class="sxs-lookup"><span data-stu-id="3410b-132">This requires that all requests be authenticated, and all unauthenticated requests are redirected to Google for authentication.</span></span>
5. <span data-ttu-id="3410b-133">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="3410b-133">Click **Save**.</span></span>

<span data-ttu-id="3410b-134">Du är nu redo att använda Google för autentisering i appen.</span><span class="sxs-lookup"><span data-stu-id="3410b-134">You are now ready to use Google for authentication in your app.</span></span>

## <span data-ttu-id="3410b-135"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="3410b-135"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portalen]: https://portal.azure.com/

