---
title: "Azure Active Directory B2C: Konfigurationen för Microsoft-kontot | Microsoft Docs"
description: "Ange registrering och inloggning för konsumenter med Microsoft-konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: swkrish
manager: mbaldwin
editor: bryanla
ms.assetid: 06407322-142c-4cb3-9106-a8d752c4c853
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/06/2016
ms.author: swkrish
ms.openlocfilehash: 59879dc0b3fc1d7af3e2a1f67f1701f451de9126
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-microsoft-accounts"></a><span data-ttu-id="6823b-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med Microsoft-konton</span><span class="sxs-lookup"><span data-stu-id="6823b-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Microsoft accounts</span></span>
## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="6823b-104">Skapa ett program för Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="6823b-104">Create a Microsoft account application</span></span>
<span data-ttu-id="6823b-105">Om du vill använda Microsoft-konto som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett program för Microsoft-konto och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="6823b-105">To use Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Microsoft account application and supply it with the right parameters.</span></span> <span data-ttu-id="6823b-106">Du behöver ett Microsoft-konto för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="6823b-106">You need a Microsoft account to do this.</span></span> <span data-ttu-id="6823b-107">Om du inte har något du kan hämta den på [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="6823b-107">If you don’t have one, you can get it at [https://www.live.com/](https://www.live.com/).</span></span>

1. <span data-ttu-id="6823b-108">Gå till den [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) och logga in med ditt Microsoft-kontouppgifter.</span><span class="sxs-lookup"><span data-stu-id="6823b-108">Go to the [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2. <span data-ttu-id="6823b-109">Klicka på **Lägg till en app**.</span><span class="sxs-lookup"><span data-stu-id="6823b-109">Click **Add an app**.</span></span>
   
    ![Microsoft-konto – Lägg till en ny app](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. <span data-ttu-id="6823b-111">Ange en **namn** för dina program och klickar på **skapa program**.</span><span class="sxs-lookup"><span data-stu-id="6823b-111">Provide a **Name** for your application and click **Create application**.</span></span>
   
    ![Microsoft-konto - programnamn](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. <span data-ttu-id="6823b-113">Kopiera värdet för **program-Id**. Du behöver det för att konfigurera Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="6823b-113">Copy the value of **Application Id**. You will need it to configure Microsoft account as an identity provider in your tenant.</span></span>
   
    ![Microsoft-konto - program-Id](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. <span data-ttu-id="6823b-115">Klicka på **Lägg till plattformen** och välj **Web**.</span><span class="sxs-lookup"><span data-stu-id="6823b-115">Click on **Add platform** and choose **Web**.</span></span>
   
    ![Microsoft-konto – Lägg till plattformen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-konto - webbtjänst](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. <span data-ttu-id="6823b-118">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i den **omdirigerings-URI: er** fältet.</span><span class="sxs-lookup"><span data-stu-id="6823b-118">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in the **Redirect URIs** field.</span></span> <span data-ttu-id="6823b-119">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="6823b-119">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
   
    ![Microsoft-konto - omdirigerings-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. <span data-ttu-id="6823b-121">Klicka på **generera nya lösenord** under den **programmet hemligheter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6823b-121">Click on **Generate New Password** under the **Application Secrets** section.</span></span> <span data-ttu-id="6823b-122">Kopiera det nya lösenordet som visas på skärmen.</span><span class="sxs-lookup"><span data-stu-id="6823b-122">Copy the new password displayed on screen.</span></span> <span data-ttu-id="6823b-123">Du behöver det för att konfigurera Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="6823b-123">You will need it to configure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="6823b-124">Lösenordet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="6823b-124">This password is an important security credential.</span></span>
   
    ![Microsoft-konto – Skapa nytt lösenord](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-konto - nytt lösenord](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. <span data-ttu-id="6823b-127">Markera kryssrutan som säger **Live SDK stöd** under den **avancerade alternativ** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="6823b-127">Check the box that says **Live SDK support** under the **Advanced Options** section.</span></span> <span data-ttu-id="6823b-128">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="6823b-128">Click **Save**.</span></span>
   
    ![Microsoft-konto - stöd för Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="6823b-130">Konfigurera Microsoft-konto som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="6823b-130">Configure Microsoft account as an identity provider in your tenant</span></span>
1. <span data-ttu-id="6823b-131">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="6823b-131">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="6823b-132">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="6823b-132">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="6823b-133">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="6823b-133">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="6823b-134">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="6823b-134">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="6823b-135">Till exempel ange ”MSA”.</span><span class="sxs-lookup"><span data-stu-id="6823b-135">For example, enter "MSA".</span></span>
5. <span data-ttu-id="6823b-136">Klicka på **identitet providertyp**väljer **Microsoft-konto**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="6823b-136">Click **Identity provider type**, select **Microsoft account**, and click **OK**.</span></span>
6. <span data-ttu-id="6823b-137">Klicka på **ställa in den här identitetsleverantör** och ange program-Id och lösenord för programmet Microsoft-konto som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="6823b-137">Click **Set up this identity provider** and enter the Application Id and password of the Microsoft account application that you created earlier.</span></span>
7. <span data-ttu-id="6823b-138">Klicka på **OK** och klicka sedan på **skapa** att spara konfigurationen för Microsoft-kontot.</span><span class="sxs-lookup"><span data-stu-id="6823b-138">Click **OK** and then click **Create** to save your Microsoft account configuration.</span></span>

