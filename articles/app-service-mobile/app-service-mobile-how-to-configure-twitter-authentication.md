---
title: "aaaHow tooconfigure Twitter-autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Twitter-autentisering för din App Services-tjänstprogrammet."
services: app-service
documentationcenter: 
author: mattchenderson
manager: syntaxc4
editor: 
ms.assetid: c6dc91d7-30f6-448c-9f2d-8e91104cde73
ms.service: app-service-mobile
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: 0d16ac44d7b54e3567b793a904059d31ab1d8911
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-twitter-login"></a><span data-ttu-id="aedfb-103">Hur tooconfigure din Apptjänst programmet toouse Twitter-inloggning</span><span class="sxs-lookup"><span data-stu-id="aedfb-103">How tooconfigure your App Service application toouse Twitter login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="aedfb-104">Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Twitter som en autentiseringsprovider.</span><span class="sxs-lookup"><span data-stu-id="aedfb-104">This topic shows you how tooconfigure Azure App Service toouse Twitter as an authentication provider.</span></span>

<span data-ttu-id="aedfb-105">Du måste ha ett Twitter-konto som har ett verifierad e-postadressen och telefonnumret toocomplete hello proceduren i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="aedfb-105">toocomplete hello procedure in this topic, you must have a Twitter account that has a verified email address and phone number.</span></span> <span data-ttu-id="aedfb-106">Gå toocreate ett nytt Twitter-konto för<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span><span class="sxs-lookup"><span data-stu-id="aedfb-106">toocreate a new Twitter account, go too<a href="http://go.microsoft.com/fwlink/p/?LinkID=268287" target="_blank">twitter.com</a>.</span></span>

## <span data-ttu-id="aedfb-107"><a name="register"></a>Registrera programmet med Twitter</span><span class="sxs-lookup"><span data-stu-id="aedfb-107"><a name="register"> </a>Register your application with Twitter</span></span>
1. <span data-ttu-id="aedfb-108">Logga in toohello [Azure-portalen], och navigera tooyour program.</span><span class="sxs-lookup"><span data-stu-id="aedfb-108">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="aedfb-109">Kopiera ditt **URL**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-109">Copy your **URL**.</span></span> <span data-ttu-id="aedfb-110">Du använder den här tooconfigure Twitter-app.</span><span class="sxs-lookup"><span data-stu-id="aedfb-110">You will use this tooconfigure your Twitter app.</span></span>
2. <span data-ttu-id="aedfb-111">Navigera toohello [Twitter utvecklare] webbplats, logga in med dina autentiseringsuppgifter för Twitter-konto och klicka på **Skapa ny App**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-111">Navigate toohello [Twitter Developers] website, sign in with your Twitter account credentials, and click **Create New App**.</span></span>
3. <span data-ttu-id="aedfb-112">Typen i hello **namn** och en **beskrivning** för din nya app.</span><span class="sxs-lookup"><span data-stu-id="aedfb-112">Type in hello **Name** and a **Description** for your new app.</span></span> <span data-ttu-id="aedfb-113">Klistra in i ditt program **URL** för hello **webbplats** värde.</span><span class="sxs-lookup"><span data-stu-id="aedfb-113">Paste in your application's **URL** for hello **Website** value.</span></span> <span data-ttu-id="aedfb-114">Sedan för hello **motringning URL**, klistra in hello **motringning URL** du kopierade tidigare.</span><span class="sxs-lookup"><span data-stu-id="aedfb-114">Then, for hello **Callback URL**, paste hello **Callback URL** you copied earlier.</span></span> <span data-ttu-id="aedfb-115">Här hittar du Mobilapp läggas till med hello sökvägen */.auth/login/twitter/callback*.</span><span class="sxs-lookup"><span data-stu-id="aedfb-115">This is your Mobile App gateway appended with hello path, */.auth/login/twitter/callback*.</span></span> <span data-ttu-id="aedfb-116">Till exempel `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span><span class="sxs-lookup"><span data-stu-id="aedfb-116">For example, `https://contoso.azurewebsites.net/.auth/login/twitter/callback`.</span></span> <span data-ttu-id="aedfb-117">Kontrollera att du använder hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="aedfb-117">Make sure that you are using hello HTTPS scheme.</span></span>
4. <span data-ttu-id="aedfb-118">Läs och acceptera hello hello nedre hello sida.</span><span class="sxs-lookup"><span data-stu-id="aedfb-118">At hello bottom hello page, read and accept hello terms.</span></span> <span data-ttu-id="aedfb-119">Klicka på **skapa programmet Twitter**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-119">Then click **Create your Twitter application**.</span></span> <span data-ttu-id="aedfb-120">Den här registren hello appen visar hello programinformation.</span><span class="sxs-lookup"><span data-stu-id="aedfb-120">This registers hello app displays hello application details.</span></span>
5. <span data-ttu-id="aedfb-121">Klicka på hello **inställningar** markerar **Tillåt det här programmet används toobe toosign in med Twitter**, klicka på **uppdateringsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-121">Click hello **Settings** tab, check **Allow this application toobe used toosign in with Twitter**, then click **Update Settings**.</span></span>
6. <span data-ttu-id="aedfb-122">Välj hello **nycklar och åtkomst-token** fliken. Anteckna hello värdena för **konsumentnyckel (API-nyckel)** och **konsumenten hemligheten (API hemliga)**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-122">Select hello **Keys and Access Tokens** tab. Make a note of hello values of **Consumer Key (API Key)** and **Consumer secret (API Secret)**.</span></span>
   
   > [!NOTE]
   > <span data-ttu-id="aedfb-123">hello konsumenthemlighet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="aedfb-123">hello consumer secret is an important security credential.</span></span> <span data-ttu-id="aedfb-124">Dela den här hemligheten med någon eller inte distribuera med din app.</span><span class="sxs-lookup"><span data-stu-id="aedfb-124">Do not share this secret with anyone or distribute it with your app.</span></span>
   > 
   > 

## <span data-ttu-id="aedfb-125"><a name="secrets"></a>Lägg till Twitter information tooyour program</span><span class="sxs-lookup"><span data-stu-id="aedfb-125"><a name="secrets"> </a>Add Twitter information tooyour application</span></span>
1. <span data-ttu-id="aedfb-126">Tillbaka i hello [Azure-portalen], navigera tooyour program.</span><span class="sxs-lookup"><span data-stu-id="aedfb-126">Back in hello [Azure portal], navigate tooyour application.</span></span> <span data-ttu-id="aedfb-127">Klicka på **inställningar**, och sedan **autentisering / auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-127">Click **Settings**, and then **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="aedfb-128">Om hello autentisering / auktoriseringsfunktionen är inte aktiverad, aktivera hello växeln för**på**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-128">If hello Authentication / Authorization feature is not enabled, turn hello switch too**On**.</span></span>
3. <span data-ttu-id="aedfb-129">Klicka på **Twitter**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-129">Click **Twitter**.</span></span> <span data-ttu-id="aedfb-130">Klistra in i hello App-ID och App hemlighet värden som du hämtade tidigare.</span><span class="sxs-lookup"><span data-stu-id="aedfb-130">Paste in hello App ID and App Secret values which you obtained previously.</span></span> <span data-ttu-id="aedfb-131">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-131">Then click **OK**.</span></span>
   
   ![][1]
   
   <span data-ttu-id="aedfb-132">Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er.</span><span class="sxs-lookup"><span data-stu-id="aedfb-132">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="aedfb-133">Du måste auktorisera användare i din Appkod.</span><span class="sxs-lookup"><span data-stu-id="aedfb-133">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="aedfb-134">(Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Twitter, ange **åtgärd tootake när begäran inte har autentiserats** för**Twitter**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-134">(Optional) toorestrict access tooyour site tooonly users authenticated by Twitter, set **Action tootake when request is not authenticated** too**Twitter**.</span></span> <span data-ttu-id="aedfb-135">Detta kräver att alla förfrågningar autentiseras, och alla begäranden som inte har autentiserats är omdirigerade tooTwitter för autentisering.</span><span class="sxs-lookup"><span data-stu-id="aedfb-135">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooTwitter for authentication.</span></span>
5. <span data-ttu-id="aedfb-136">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="aedfb-136">Click **Save**.</span></span>

<span data-ttu-id="aedfb-137">Nu är du redo toouse Twitter för autentisering i appen.</span><span class="sxs-lookup"><span data-stu-id="aedfb-137">You are now ready toouse Twitter for authentication in your app.</span></span>

## <span data-ttu-id="aedfb-138"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="aedfb-138"><a name="related-content"> </a>Related Content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-twitter-authentication/app-service-twitter-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-twitter-authentication/mobile-app-twitter-settings.png

<!-- URLs. -->

[Twitter utvecklare]: http://go.microsoft.com/fwlink/p/?LinkId=268300
[Azure-portalen]: https://portal.azure.com/
[xamarin]: ../app-services-mobile-app-xamarin-ios-get-started-users.md
