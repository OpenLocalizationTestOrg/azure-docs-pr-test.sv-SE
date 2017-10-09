---
title: "aaaHow tooconfigure Google-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Google-autentisering för din App Services-tjänstprogrammet."
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
ms.openlocfilehash: 9175c40b78c859e9e191504c41cd0bb9a3380ccd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-google-login"></a><span data-ttu-id="b81ac-103">Hur tooconfigure din Apptjänst programmet toouse Google-inloggning</span><span class="sxs-lookup"><span data-stu-id="b81ac-103">How tooconfigure your App Service application toouse Google login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="b81ac-104">Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Google som en autentiseringsprovider.</span><span class="sxs-lookup"><span data-stu-id="b81ac-104">This topic shows you how tooconfigure Azure App Service toouse Google as an authentication provider.</span></span>

<span data-ttu-id="b81ac-105">Du måste ha ett Google-konto som har en verifierad e-postadress toocomplete hello proceduren i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b81ac-105">toocomplete hello procedure in this topic, you must have a Google account that has a verified email address.</span></span> <span data-ttu-id="b81ac-106">Gå toocreate ett nytt Google-konto för[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span><span class="sxs-lookup"><span data-stu-id="b81ac-106">toocreate a new Google account, go too[accounts.google.com](http://go.microsoft.com/fwlink/p/?LinkId=268302).</span></span>

## <span data-ttu-id="b81ac-107"><a name="register"></a>Registrera programmet med Google</span><span class="sxs-lookup"><span data-stu-id="b81ac-107"><a name="register"> </a>Register your application with Google</span></span>
1. <span data-ttu-id="b81ac-108">Logga in toohello [Azure-portalen], och navigera tooyour program.</span><span class="sxs-lookup"><span data-stu-id="b81ac-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="b81ac-109">Kopiera ditt **URL**, som du använder senare tooconfigure Google-app.</span><span class="sxs-lookup"><span data-stu-id="b81ac-109">Copy your **URL**, which you use later tooconfigure your Google app.</span></span>
2. <span data-ttu-id="b81ac-110">Navigera toohello [Google API: er](http://go.microsoft.com/fwlink/p/?LinkId=268303) logga in med autentiseringsuppgifterna för ditt Google-konto, klickar du på **skapa projekt**, ange en **projektnamn**, klicka på  **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-110">Navigate toohello [Google apis](http://go.microsoft.com/fwlink/p/?LinkId=268303) website, sign in with your Google account credentials, click **Create Project**, provide a **Project name**, then click **Create**.</span></span>
3. <span data-ttu-id="b81ac-111">Under **sociala API: er** klickar du på **Google + API** och sedan **aktivera**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-111">Under **Social APIs** click **Google+ API** and then **Enable**.</span></span>
4. <span data-ttu-id="b81ac-112">I vänster navigering hello **autentiseringsuppgifter** > **OAuth-medgivande skärmen**och välj din **e-postadress**, ange en **produktnamn**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-112">In hello left navigation, **Credentials** > **OAuth consent screen**, then select your **Email address**,  enter a **Product Name**, and click **Save**.</span></span>
5. <span data-ttu-id="b81ac-113">I hello **autentiseringsuppgifter** klickar du på **skapa autentiseringsuppgifter** > **OAuth-klient-ID**och välj **webbprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-113">In hello **Credentials** tab, click **Create credentials** > **OAuth client ID**, then select **Web application**.</span></span>
6. <span data-ttu-id="b81ac-114">Klistra in hello Apptjänst **URL** du kopierade tidigare i **behörighet JavaScript ursprung**, klistra sedan in din omdirigering URI i **behörighet omdirigerings-URI**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-114">Paste hello App Service **URL** you copied earlier into **Authorized JavaScript Origins**, then paste your redirect URI into **Authorized Redirect URI**.</span></span> <span data-ttu-id="b81ac-115">Hej omdirigerings-URI: N är hello URL för ditt program läggas till med hello sökvägen */.auth/login/google/callback*.</span><span class="sxs-lookup"><span data-stu-id="b81ac-115">hello redirect URI is hello URL of your application appended with hello path, */.auth/login/google/callback*.</span></span> <span data-ttu-id="b81ac-116">Till exempel `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span><span class="sxs-lookup"><span data-stu-id="b81ac-116">For example, `https://contoso.azurewebsites.net/.auth/login/google/callback`.</span></span> <span data-ttu-id="b81ac-117">Kontrollera att du använder hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="b81ac-117">Make sure that you are using hello HTTPS scheme.</span></span> <span data-ttu-id="b81ac-118">Klicka sedan på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-118">Then click **Create**.</span></span>
7. <span data-ttu-id="b81ac-119">På nästa skärm hello anteckna hello värdena för hello klient-ID och klienten hemlighet.</span><span class="sxs-lookup"><span data-stu-id="b81ac-119">On hello next screen, make a note of hello values of hello client ID and client secret.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="b81ac-120">Hej klienthemlighet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="b81ac-120">hello client secret is an important security credential.</span></span> <span data-ttu-id="b81ac-121">Dela den här hemligheten med någon eller inte distribuera inom ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="b81ac-121">Do not share this secret with anyone or distribute it within a client application.</span></span>


## <span data-ttu-id="b81ac-122"><a name="secrets"></a>Lägga till Google information tooyour program</span><span class="sxs-lookup"><span data-stu-id="b81ac-122"><a name="secrets"> </a>Add Google information tooyour application</span></span>
1. <span data-ttu-id="b81ac-123">Tillbaka i hello [Azure-portalen], navigera tooyour program.</span><span class="sxs-lookup"><span data-stu-id="b81ac-123">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="b81ac-124">Klicka på **inställningar**, och sedan **autentisering / auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-124">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="b81ac-125">Om hello autentisering / auktoriseringsfunktionen är inte aktiverad, aktivera hello växeln för**på**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-125">If hello Authentication / Authorization feature is not enabled, turn hello switch too**On**.</span></span>
3. <span data-ttu-id="b81ac-126">Klicka på **Google**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-126">Click **Google**.</span></span> <span data-ttu-id="b81ac-127">Klistra in i hello App-ID och App hemlighet värden som du hämtade tidigare och välja att aktivera alla scope som krävs för ditt program.</span><span class="sxs-lookup"><span data-stu-id="b81ac-127">Paste in hello App ID and App Secret values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="b81ac-128">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-128">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="b81ac-129">Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er.</span><span class="sxs-lookup"><span data-stu-id="b81ac-129">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="b81ac-130">Du måste auktorisera användare i din Appkod.</span><span class="sxs-lookup"><span data-stu-id="b81ac-130">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="b81ac-131">(Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Google, ange **åtgärd tootake när begäran inte har autentiserats** för**Google**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-131">(Optional) toorestrict access tooyour site tooonly users authenticated by Google, set **Action tootake when request is not authenticated** too**Google**.</span></span> <span data-ttu-id="b81ac-132">Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooGoogle för autentisering.</span><span class="sxs-lookup"><span data-stu-id="b81ac-132">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooGoogle for authentication.</span></span>
5. <span data-ttu-id="b81ac-133">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="b81ac-133">Click **Save**.</span></span>

<span data-ttu-id="b81ac-134">Nu är du redo toouse Google för autentisering i appen.</span><span class="sxs-lookup"><span data-stu-id="b81ac-134">You are now ready toouse Google for authentication in your app.</span></span>

## <span data-ttu-id="b81ac-135"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="b81ac-135"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Anchors. -->

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-google-authentication/mobile-app-google-settings.png

<!-- URLs. -->

[Google apis]: http://go.microsoft.com/fwlink/p/?LinkId=268303

[Azure-portalen]: https://portal.azure.com/

