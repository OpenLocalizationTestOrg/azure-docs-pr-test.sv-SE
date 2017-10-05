---
title: 'Azure Active Directory B2C: Google + configuration | Microsoft Docs'
description: "Ange registrering och inloggning för konsumenter med Google + konton i dina program som skyddas av Azure Active Directory B2C."
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
ms.openlocfilehash: 6ab73e5c79742ab548733f5712dee1e28461db9f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-google-accounts"></a><span data-ttu-id="0426d-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med Google + konton</span><span class="sxs-lookup"><span data-stu-id="0426d-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Google+ accounts</span></span>
## <a name="create-a-google-application"></a><span data-ttu-id="0426d-104">Skapa ett Google +-program</span><span class="sxs-lookup"><span data-stu-id="0426d-104">Create a Google+ application</span></span>
<span data-ttu-id="0426d-105">Om du vill använda Google + som en identitetsleverantör i Azure Active Directory (AD Azure) B2C, måste du skapa ett Google +-program och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="0426d-105">To use Google+ as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Google+ application and supply it with the right parameters.</span></span> <span data-ttu-id="0426d-106">Du behöver ett Google +-konto om du vill.</span><span class="sxs-lookup"><span data-stu-id="0426d-106">You need a Google+ account to do this.</span></span> <span data-ttu-id="0426d-107">Om du inte har något du kan hämta den på [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span><span class="sxs-lookup"><span data-stu-id="0426d-107">If you don’t have one, you can get it at [https://accounts.google.com/SignUp](https://accounts.google.com/SignUp).</span></span>

1. <span data-ttu-id="0426d-108">Gå till den [Google utvecklare konsolen](https://console.developers.google.com/) och logga in med Google + autentiseringsuppgifterna för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="0426d-108">Go to the [Google Developers Console](https://console.developers.google.com/) and sign in with your Google+ account credentials.</span></span>
2. <span data-ttu-id="0426d-109">Klicka på **skapa projekt**, ange en **projektnamn**, och klicka sedan på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="0426d-109">Click **Create project**, enter a **Project name**, and then click **Create**.</span></span>
   
    ![Google + - komma igång](./media/active-directory-b2c-setup-goog-app/google-get-started.png)
   
    ![Google + - nytt projekt](./media/active-directory-b2c-setup-goog-app/google-new-project.png)
3. <span data-ttu-id="0426d-112">Klicka på **API-hanterare** och klicka sedan på **autentiseringsuppgifter** i det vänstra navigeringsfönstret.</span><span class="sxs-lookup"><span data-stu-id="0426d-112">Click **API Manager** and then click **Credentials** in the left navigation.</span></span>
4. <span data-ttu-id="0426d-113">Klicka på den **OAuth-medgivande skärmen** högst upp.</span><span class="sxs-lookup"><span data-stu-id="0426d-113">Click the **OAuth consent screen** tab at the top.</span></span>
   
    ![Google + - autentiseringsuppgifter](./media/active-directory-b2c-setup-goog-app/google-add-cred.png)
5. <span data-ttu-id="0426d-115">Välj eller ange en giltig **e-postadress**, ange en **produktnamn**, och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="0426d-115">Select or specify a valid **Email address**, provide a **Product name**, and click **Save**.</span></span>
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-consent-screen.png)
6. <span data-ttu-id="0426d-117">Klicka på **nya autentiseringsuppgifter** och välj sedan **OAuth-klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="0426d-117">Click **New credentials** and then choose **OAuth client ID**.</span></span>
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-add-oauth2-client-id.png)
7. <span data-ttu-id="0426d-119">Under **programtyp**väljer **webbprogrammet**.</span><span class="sxs-lookup"><span data-stu-id="0426d-119">Under **Application type**, select **Web application**.</span></span>
   
    ![Google + - skärmen för OAuth-medgivande](./media/active-directory-b2c-setup-goog-app/google-web-app.png)
8. <span data-ttu-id="0426d-121">Ange en **namn** för ditt program ange `https://login.microsoftonline.com` i den **behörighet JavaScript ursprung** fältet och `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i den **auktoriserad omdirigerings-URI: er** fältet.</span><span class="sxs-lookup"><span data-stu-id="0426d-121">Provide a **Name** for your application, enter `https://login.microsoftonline.com` in the **Authorized JavaScript origins** field, and `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Authorized redirect URIs** field.</span></span> <span data-ttu-id="0426d-122">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="0426d-122">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span> <span data-ttu-id="0426d-123">Den **{klient}** värdet är skiftlägeskänsligt.</span><span class="sxs-lookup"><span data-stu-id="0426d-123">The **{tenant}** value is case-sensitive.</span></span> <span data-ttu-id="0426d-124">Klicka på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="0426d-124">Click **Create**.</span></span>
   
    ![Google + - skapar klient-ID](./media/active-directory-b2c-setup-goog-app/google-create-client-id.png)
9. <span data-ttu-id="0426d-126">Kopiera värdena i **klient-ID** och **klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="0426d-126">Copy the values of **Client ID** and **Client secret**.</span></span> <span data-ttu-id="0426d-127">Du måste båda för att konfigurera Google + som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="0426d-127">You will need both of them to configure Google+ as an identity provider in your tenant.</span></span> <span data-ttu-id="0426d-128">**Klienthemlighet** är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="0426d-128">**Client secret** is an important security credential.</span></span>
   
    ![Google + - klienthemlighet](./media/active-directory-b2c-setup-goog-app/google-client-secret.png)

## <a name="configure-google-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="0426d-130">Konfigurera Google + som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="0426d-130">Configure Google+ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="0426d-131">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="0426d-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="0426d-132">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="0426d-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="0426d-133">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="0426d-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="0426d-134">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="0426d-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="0426d-135">Till exempel ange ”G +”.</span><span class="sxs-lookup"><span data-stu-id="0426d-135">For example, enter "G+".</span></span>
5. <span data-ttu-id="0426d-136">Klicka på **identitet providertyp**väljer **Google**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="0426d-136">Click **Identity provider type**, select **Google**, and click **OK**.</span></span>
6. <span data-ttu-id="0426d-137">Klicka på **ställa in den här identitetsleverantör** och ange klient-ID och klienthemlighet på Google +-programmet som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="0426d-137">Click **Set up this identity provider** and enter the client ID and client secret of the Google+ application that you created earlier.</span></span>
7. <span data-ttu-id="0426d-138">Klicka på **OK** och klicka sedan på **skapa** att spara din Google +-konfiguration.</span><span class="sxs-lookup"><span data-stu-id="0426d-138">Click **OK** and then click **Create** to save your Google+ configuration.</span></span>

