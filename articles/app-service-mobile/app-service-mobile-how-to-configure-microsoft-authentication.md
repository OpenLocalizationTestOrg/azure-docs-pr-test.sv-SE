---
title: "aaaHow tooconfigure Account autentisering för tillämpningsprogrammet Apptjänster"
description: "Lär dig hur tooconfigure Account autentisering för din App Services-tjänstprogrammet."
author: mattchenderson
services: app-service
documentationcenter: 
manager: syntaxc4
editor: 
ms.assetid: ffbc6064-edf6-474d-971c-695598fd08bf
ms.service: app-service
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: multiple
ms.topic: article
ms.date: 10/01/2016
ms.author: mahender
ms.openlocfilehash: d86d8dab26a189f4454082fc18e44e3fb6e0a01d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooconfigure-your-app-service-application-toouse-microsoft-account-login"></a><span data-ttu-id="129ec-103">Hur tooconfigure Apptjänst programmet toouse Account inloggningen</span><span class="sxs-lookup"><span data-stu-id="129ec-103">How tooconfigure your App Service application toouse Microsoft Account login</span></span>
[!INCLUDE [app-service-mobile-selector-authentication](../../includes/app-service-mobile-selector-authentication.md)]

<span data-ttu-id="129ec-104">Det här avsnittet beskrivs hur du tooconfigure Azure App Service toouse Account som en autentiseringsprovider.</span><span class="sxs-lookup"><span data-stu-id="129ec-104">This topic shows you how tooconfigure Azure App Service toouse Microsoft Account as an authentication provider.</span></span> 

## <span data-ttu-id="129ec-105"><a name="register-microsoft-account"></a>Registrera din app med Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="129ec-105"><a name="register-microsoft-account"> </a>Register your app with Microsoft Account</span></span>
1. <span data-ttu-id="129ec-106">Logga in toohello [Azure-portalen], och navigera tooyour program.</span><span class="sxs-lookup"><span data-stu-id="129ec-106">Log on toohello [Azure portal], and navigate tooyour application.</span></span> <span data-ttu-id="129ec-107">Kopiera ditt **URL**, vilket du använda senare tooconfigure din app med Account.</span><span class="sxs-lookup"><span data-stu-id="129ec-107">Copy your **URL**, which later you use tooconfigure your app with Microsoft Account.</span></span>
2. <span data-ttu-id="129ec-108">Navigera toohello [Mina program] i hello Microsoft Account Developer Center och logga in med ditt Microsoft-konto om det behövs.</span><span class="sxs-lookup"><span data-stu-id="129ec-108">Navigate toohello [My Applications] page in hello Microsoft Account Developer Center, and log on with your Microsoft account, if required.</span></span>
3. <span data-ttu-id="129ec-109">Klicka på **Lägg till en app**, ange ett programnamn och på **skapa program**.</span><span class="sxs-lookup"><span data-stu-id="129ec-109">Click **Add an app**, then type an application name, and click **Create application**.</span></span>
4. <span data-ttu-id="129ec-110">Anteckna hello **program-ID**, som du behöver senare.</span><span class="sxs-lookup"><span data-stu-id="129ec-110">Make a note of hello **Application ID**, as you will need it later.</span></span> 
5. <span data-ttu-id="129ec-111">Klicka på under ”plattformar”, **lägga till plattformen** och välj ”Web”.</span><span class="sxs-lookup"><span data-stu-id="129ec-111">Under "Platforms," click **Add Platform** and select "Web".</span></span>
6. <span data-ttu-id="129ec-112">Under ”omdirigerings-URI: er” ange hello slutpunkten för ditt program, sedan klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="129ec-112">Under "Redirect URIs" supply hello endpoint for your application, then click **Save**.</span></span> 
   
   > [!NOTE]
   > <span data-ttu-id="129ec-113">Din omdirigering URI är hello URL för ditt program läggas till med hello sökvägen */.auth/login/microsoftaccount/callback*.</span><span class="sxs-lookup"><span data-stu-id="129ec-113">Your redirect URI is hello URL of your application appended with hello path, */.auth/login/microsoftaccount/callback*.</span></span> <span data-ttu-id="129ec-114">Till exempel `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span><span class="sxs-lookup"><span data-stu-id="129ec-114">For example, `https://contoso.azurewebsites.net/.auth/login/microsoftaccount/callback`.</span></span>   
   > <span data-ttu-id="129ec-115">Kontrollera att du använder hello HTTPS-schema.</span><span class="sxs-lookup"><span data-stu-id="129ec-115">Make sure that you are using hello HTTPS scheme.</span></span>
   
7. <span data-ttu-id="129ec-116">Klicka på under ”programmet hemligheter”, **generera nya lösenord**.</span><span class="sxs-lookup"><span data-stu-id="129ec-116">Under "Application Secrets," click **Generate New Password**.</span></span> <span data-ttu-id="129ec-117">Anteckna hello-värdet som visas.</span><span class="sxs-lookup"><span data-stu-id="129ec-117">Make note of hello value that appears.</span></span> <span data-ttu-id="129ec-118">När du lämnar sidan hello, kommer den inte att visas igen.</span><span class="sxs-lookup"><span data-stu-id="129ec-118">Once you leave hello page, it will not be displayed again.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="129ec-119">hello lösenordet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="129ec-119">hello password is an important security credential.</span></span> <span data-ttu-id="129ec-120">Dela hello lösenord med någon eller inte distribuera inom ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="129ec-120">Do not share hello password with anyone or distribute it within a client application.</span></span>

## <span data-ttu-id="129ec-121"><a name="secrets"></a>Lägg till Microsoft-konto information tooyour App-tjänstprogram</span><span class="sxs-lookup"><span data-stu-id="129ec-121"><a name="secrets"> </a>Add Microsoft Account information tooyour App Service application</span></span>
1. <span data-ttu-id="129ec-122">Tillbaka i hello [Azure-portalen]navigerar tooyour programmet, klickar du på **inställningar** > **autentisering / auktorisering**.</span><span class="sxs-lookup"><span data-stu-id="129ec-122">Back in hello [Azure portal], navigate tooyour application, click **Settings** > **Authentication / Authorization**.</span></span>
2. <span data-ttu-id="129ec-123">Om hello autentisering / auktorisering är inte aktiverad, växlar den **på**.</span><span class="sxs-lookup"><span data-stu-id="129ec-123">If hello Authentication / Authorization feature is not enabled, switch it **On**.</span></span>
3. <span data-ttu-id="129ec-124">Klicka på **Microsoft-konto**.</span><span class="sxs-lookup"><span data-stu-id="129ec-124">Click **Microsoft Account**.</span></span> <span data-ttu-id="129ec-125">Klistra in i hello program-ID och lösenord värden som du hämtade tidigare och välja att aktivera alla scope som krävs för ditt program.</span><span class="sxs-lookup"><span data-stu-id="129ec-125">Paste in hello Application ID and Password values which you obtained previously, and optionally enable any scopes your application requires.</span></span> <span data-ttu-id="129ec-126">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="129ec-126">Then click **OK**.</span></span>
   
    ![][1]
   
    <span data-ttu-id="129ec-127">Som standard Apptjänst ger autentisering, men begränsar inte auktoriserad åtkomst tooyour plats innehåll och API: er.</span><span class="sxs-lookup"><span data-stu-id="129ec-127">By default, App Service provides authentication but does not restrict authorized access tooyour site content and APIs.</span></span> <span data-ttu-id="129ec-128">Du måste auktorisera användare i din Appkod.</span><span class="sxs-lookup"><span data-stu-id="129ec-128">You must authorize users in your app code.</span></span>
4. <span data-ttu-id="129ec-129">(Valfritt) toorestrict-tooyour plats tooonly användare autentiseras av Microsoft-konto, ange **åtgärd tootake när begäran inte har autentiserats** för**Account**.</span><span class="sxs-lookup"><span data-stu-id="129ec-129">(Optional) toorestrict access tooyour site tooonly users authenticated by Microsoft account, set **Action tootake when request is not authenticated** too**Microsoft Account**.</span></span> <span data-ttu-id="129ec-130">Detta kräver att alla förfrågningar autentiseras och alla oautentiserade begäranden omdirigeras tooMicrosoft kontot för autentisering.</span><span class="sxs-lookup"><span data-stu-id="129ec-130">This requires that all requests be authenticated, and all unauthenticated requests are redirected tooMicrosoft account for authentication.</span></span>
5. <span data-ttu-id="129ec-131">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="129ec-131">Click **Save**.</span></span>

<span data-ttu-id="129ec-132">Nu är du redo toouse Account för autentisering i appen.</span><span class="sxs-lookup"><span data-stu-id="129ec-132">You are now ready toouse Microsoft Account for authentication in your app.</span></span>

## <span data-ttu-id="129ec-133"><a name="related-content"></a>Relaterat innehåll</span><span class="sxs-lookup"><span data-stu-id="129ec-133"><a name="related-content"> </a>Related content</span></span>
[!INCLUDE [app-service-mobile-related-content-get-started-users](../../includes/app-service-mobile-related-content-get-started-users.md)]

<!-- Images. -->

[0]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/app-service-microsoftaccount-redirect.png
[1]: ./media/app-service-mobile-how-to-configure-microsoft-authentication/mobile-app-microsoftaccount-settings.png

<!-- URLs. -->

[Mina program]: http://go.microsoft.com/fwlink/p/?LinkId=262039
[Azure-portalen]: https://portal.azure.com/
