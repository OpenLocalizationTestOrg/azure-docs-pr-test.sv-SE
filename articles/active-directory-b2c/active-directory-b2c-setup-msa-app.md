---
title: "Azure Active Directory B2C: Konfigurationen för Microsoft-kontot | Microsoft Docs"
description: "Ange tooconsumers för registrering och inloggning med Microsoft-konton i dina program som skyddas av Azure Active Directory B2C."
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
ms.openlocfilehash: bec4777f003c459030f68c35b24f0e4bcddf84ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-microsoft-accounts"></a><span data-ttu-id="4cd2a-103">Azure Active Directory B2C: Ange tooconsumers för registrering och inloggning med Microsoft-konton</span><span class="sxs-lookup"><span data-stu-id="4cd2a-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Microsoft accounts</span></span>
## <a name="create-a-microsoft-account-application"></a><span data-ttu-id="4cd2a-104">Skapa ett program för Microsoft-konto</span><span class="sxs-lookup"><span data-stu-id="4cd2a-104">Create a Microsoft account application</span></span>
<span data-ttu-id="4cd2a-105">toouse Microsoft-kontot som en identitetsleverantör i Azure Active Directory (AD Azure) B2C kan du behöver toocreate ett program för Microsoft-konto och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-105">toouse Microsoft account as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Microsoft account application and supply it with hello right parameters.</span></span> <span data-ttu-id="4cd2a-106">Du behöver ett Microsoft-konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-106">You need a Microsoft account toodo this.</span></span> <span data-ttu-id="4cd2a-107">Om du inte har något du kan hämta den på [https://www.live.com/](https://www.live.com/).</span><span class="sxs-lookup"><span data-stu-id="4cd2a-107">If you don’t have one, you can get it at [https://www.live.com/](https://www.live.com/).</span></span>

1. <span data-ttu-id="4cd2a-108">Gå toohello [Microsoft Programregistreringsportalen](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) och logga in med ditt Microsoft-kontouppgifter.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-108">Go toohello [Microsoft Application Registration Portal](https://apps.dev.microsoft.com/?referrer=https://azure.microsoft.com/documentation/articles&deeplink=/appList) and sign in with your Microsoft account credentials.</span></span>
2. <span data-ttu-id="4cd2a-109">Klicka på **Lägg till en app**.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-109">Click **Add an app**.</span></span>
   
    ![Microsoft-konto – Lägg till en ny app](./media/active-directory-b2c-setup-msa-app/msa-add-new-app.png)
3. <span data-ttu-id="4cd2a-111">Ange en **namn** för dina program och klickar på **skapa program**.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-111">Provide a **Name** for your application and click **Create application**.</span></span>
   
    ![Microsoft-konto - programnamn](./media/active-directory-b2c-setup-msa-app/msa-app-name.png)
4. <span data-ttu-id="4cd2a-113">Kopiera hello värdet för **program-Id**. Du behöver den tooconfigure Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-113">Copy hello value of **Application Id**. You will need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span>
   
    ![Microsoft-konto - program-Id](./media/active-directory-b2c-setup-msa-app/msa-app-id.png)
5. <span data-ttu-id="4cd2a-115">Klicka på **Lägg till plattformen** och välj **Web**.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-115">Click on **Add platform** and choose **Web**.</span></span>
   
    ![Microsoft-konto – Lägg till plattformen](./media/active-directory-b2c-setup-msa-app/msa-add-platform.png)
   
    ![Microsoft-konto - webbtjänst](./media/active-directory-b2c-setup-msa-app/msa-web.png)
6. <span data-ttu-id="4cd2a-118">Ange `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` i hello **omdirigerings-URI: er** fältet.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-118">Enter `https://login.microsoftonline.com/te/{tenant}/oauth2/authresp` in hello **Redirect URIs** field.</span></span> <span data-ttu-id="4cd2a-119">Ersätt **{klient}** med din klient namn (till exempel contosob2c.onmicrosoft.com).</span><span class="sxs-lookup"><span data-stu-id="4cd2a-119">Replace **{tenant}** with your tenant's name (for example, contosob2c.onmicrosoft.com).</span></span>
   
    ![Microsoft-konto - omdirigerings-URL](./media/active-directory-b2c-setup-msa-app/msa-redirect-url.png)
7. <span data-ttu-id="4cd2a-121">Klicka på **generera nya lösenord** under hello **programmet hemligheter** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-121">Click on **Generate New Password** under hello **Application Secrets** section.</span></span> <span data-ttu-id="4cd2a-122">Kopiera hello nytt lösenord visas på skärmen.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-122">Copy hello new password displayed on screen.</span></span> <span data-ttu-id="4cd2a-123">Du behöver den tooconfigure Microsoft-konto som en identitetsleverantör i din klient.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-123">You will need it tooconfigure Microsoft account as an identity provider in your tenant.</span></span> <span data-ttu-id="4cd2a-124">Lösenordet är en viktig säkerhetsuppgift för autentisering.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-124">This password is an important security credential.</span></span>
   
    ![Microsoft-konto – Skapa nytt lösenord](./media/active-directory-b2c-setup-msa-app/msa-generate-new-password.png)
   
    ![Microsoft-konto - nytt lösenord](./media/active-directory-b2c-setup-msa-app/msa-new-password.png)
8. <span data-ttu-id="4cd2a-127">Hello kryssrutan som anger att kontrollsumman **Live SDK stöd** under hello **avancerade alternativ** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-127">Check hello box that says **Live SDK support** under hello **Advanced Options** section.</span></span> <span data-ttu-id="4cd2a-128">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-128">Click **Save**.</span></span>
   
    ![Microsoft-konto - stöd för Live SDK](./media/active-directory-b2c-setup-msa-app/msa-live-sdk-support.png)

## <a name="configure-microsoft-account-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="4cd2a-130">Konfigurera Microsoft-konto som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="4cd2a-130">Configure Microsoft account as an identity provider in your tenant</span></span>
1. <span data-ttu-id="4cd2a-131">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="4cd2a-132">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="4cd2a-133">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="4cd2a-134">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="4cd2a-135">Till exempel ange ”MSA”.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-135">For example, enter "MSA".</span></span>
5. <span data-ttu-id="4cd2a-136">Klicka på **identitet providertyp**väljer **Microsoft-konto**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-136">Click **Identity provider type**, select **Microsoft account**, and click **OK**.</span></span>
6. <span data-ttu-id="4cd2a-137">Klicka på **ställa in den här identitetsleverantör** och ange hello program-Id och lösenord för hello Microsoft-konto-program som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-137">Click **Set up this identity provider** and enter hello Application Id and password of hello Microsoft account application that you created earlier.</span></span>
7. <span data-ttu-id="4cd2a-138">Klicka på **OK** och klicka sedan på **skapa** toosave ditt Microsoft-konto konfiguration.</span><span class="sxs-lookup"><span data-stu-id="4cd2a-138">Click **OK** and then click **Create** toosave your Microsoft account configuration.</span></span>

