---
title: 'Azure Active Directory B2C: Google + configuration | Microsoft Docs'
description: "Ange tooconsumers för registrering och inloggning med Google + konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 4dcca66f-29e4-4b4d-8840-50baad736bd7
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 6ef66eb17777acd95b5f4745ed6097c77e37663b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-google-accounts"></a><span data-ttu-id="0496e-103">Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Google + konton</span><span class="sxs-lookup"><span data-stu-id="0496e-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Google+ accounts</span></span>
## <a name="create-a-google-application"></a><span data-ttu-id="0496e-104">Skapa ett Google +-program</span><span class="sxs-lookup"><span data-stu-id="0496e-104">Create a Google+ application</span></span>
<span data-ttu-id="0496e-105">toouse Google + som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate Google +-programmet och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="0496e-105">toouse Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Google+ application and supply it with hello right parameters.</span></span> <span data-ttu-id="0496e-106">Du behöver ett Google + konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="0496e-106">You need a Google+ account toodo this.</span></span> <span data-ttu-id="0496e-107">Om du inte har något du kan hämta den på [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span><span class="sxs-lookup"><span data-stu-id="0496e-107">If you don’t have one, you can get it at [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span></span>

1. <span data-ttu-id="0496e-108">Gå toohello [Google utvecklare konsolen](https://console.developers.google.com/) och logga in med Google + autentiseringsuppgifterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="0496e-108">Go toohello [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2. <span data-ttu-id="0496e-109">Klicka på **skapa projekt**, ange en **projektnamn**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0496e-109">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>
   
    ![Google + - komma igång](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - nytt projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. <span data-ttu-id="0496e-112">Klicka på **API-hanterare** och klicka sedan på **autentiseringsuppgifter** i hello vänstra navigeringsrutan.</span><span class="sxs-lookup"><span data-stu-id="0496e-112">Click **API Manager** and then click **Credentials** in hello left navigation.</span></span>
4. <span data-ttu-id="0496e-113">Klicka på hello **OAuth-medgivande skärmen** fliken hello överst.</span><span class="sxs-lookup"><span data-stu-id="0496e-113">Click hello **OAuth consent screen** tab at hello top.</span></span>
   
    ![Google + - autentiseringsuppgifter](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. <span data-ttu-id="0496e-115">Välj eller ange en giltig **e-postadress**, ange en **produktnamn**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="0496e-115">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. <span data-ttu-id="0496e-117">Klicka på **nya autentiseringsuppgifter** och välj sedan **OAuth-klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="0496e-117">Click **New credentials** and then choose **OAuth client ID**.</span></span>
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. <span data-ttu-id="0496e-119">Under **programtyp**väljer **webbprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="0496e-119">Under **Application type**, select **Web application**.</span></span>
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. <span data-ttu-id="0496e-121">Ange en **namn** för ditt program ange `https://login.microsoftonline.com` i hello **behörighet JavaScript ursprung** fältet och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **auktoriserad omdirigerings-URI: er**fält.</span><span class="sxs-lookup"><span data-stu-id="0496e-121">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in hello **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Authorized redirect URIs** field.</span></span> <span data-ttu-id="0496e-122">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="0496e-122">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="0496e-123">Hej **{klient}** värdet är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="0496e-123">hello **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="0496e-124">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0496e-124">Click **Create**.</span></span>
   
    ![Google + - skapar klient-ID](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. <span data-ttu-id="0496e-126">Kopiera hello värdena för **klient-ID** och **klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="0496e-126">Copy hello values of **Client ID** and **Client secret**.</span></span> <span data-ttu-id="0496e-127">Du måste båda tooconfigure Google + som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="0496e-127">You will need both of them tooconfigure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="0496e-128">**Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="0496e-128">**Client secret** is an important security credential.</span></span>
   
    ![Google + - klienthemlighet](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="0496e-130">Konfigurera Google + som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="0496e-130">Configure Google+ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="0496e-131">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0496e-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="0496e-132">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="0496e-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="0496e-133">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="0496e-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="0496e-134">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="0496e-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="0496e-135">Till exempel ange ”G +”.</span><span class="sxs-lookup"><span data-stu-id="0496e-135">For example, enter "G+".</span></span>
5. <span data-ttu-id="0496e-136">Klicka på **identitet providertyp**väljer **Google**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0496e-136">Click **Identity provider type**, select **Google**, and click **OK**.</span></span>
6. <span data-ttu-id="0496e-137">Klicka på **ställa in den här identitetsleverantör** och ange hello klient-ID och klienten hemligheten för hello Google +-programmet som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0496e-137">Click **Set up this identity provider** and enter hello client ID and client secret of hello Google+ application that you created earlier.</span></span>
7. <span data-ttu-id="0496e-138">Klicka på **OK** och klicka sedan på **skapa** toosave Google +-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0496e-138">Click **OK** and then click **Create** toosave your Google+ configuration.</span></span>

