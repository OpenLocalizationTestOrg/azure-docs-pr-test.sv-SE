---
title: 'Azure Active Directory B2C: Weibo konfiguration | Microsoft Docs'
description: "Ange registrering och inloggning för konsumenter med Weibo konton i dina program som skyddas av Azure Active Directory B2C."
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 1860de34-94cb-4ceb-851e-102f930f7230
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 00c5d3781455c80b33bdbb4c872ae354531baf3e
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-weibo-accounts"></a><span data-ttu-id="be90c-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med Weibo konton</span><span class="sxs-lookup"><span data-stu-id="be90c-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with Weibo accounts</span></span>

> [!NOTE]
> <span data-ttu-id="be90c-104">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="be90c-104">This feature is in preview.</span></span> <span data-ttu-id="be90c-105">Använd inte den här identitetsleverantör i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="be90c-105">Do not use this identity provider in your production environment.</span></span>
> 

## <a name="create-a-weibo-application"></a><span data-ttu-id="be90c-106">Skapa ett Weibo program</span><span class="sxs-lookup"><span data-stu-id="be90c-106">Create a Weibo application</span></span>

<span data-ttu-id="be90c-107">Om du vill använda Weibo som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett Weibo program och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="be90c-107">To use Weibo as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a Weibo application and supply it with the right parameters.</span></span> <span data-ttu-id="be90c-108">Du behöver en Weibo-konto för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="be90c-108">You need a Weibo account to do this.</span></span> <span data-ttu-id="be90c-109">Om du inte har någon kan du skaffa ett på [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span><span class="sxs-lookup"><span data-stu-id="be90c-109">If you don’t have one, you can get one at [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span></span>

### <a name="register-for-the-weibo-developer-program"></a><span data-ttu-id="be90c-110">Registrera dig för utvecklarprogram Weibo</span><span class="sxs-lookup"><span data-stu-id="be90c-110">Register for the Weibo developer program</span></span>

1. <span data-ttu-id="be90c-111">Gå till den [Weibo utvecklarportalen](http://open.weibo.com/) och logga in med autentiseringsuppgifterna för ditt Weibo.</span><span class="sxs-lookup"><span data-stu-id="be90c-111">Go to the [Weibo developer portal](http://open.weibo.com/) and sign in with your Weibo account credentials.</span></span>
2. <span data-ttu-id="be90c-112">När du har loggat in, klicka på ditt namn i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="be90c-112">After signing in, click on your display name in the top-right corner.</span></span>
3. <span data-ttu-id="be90c-113">Välj i listrutan,**编辑开发者信息**(redigera information för utvecklare).</span><span class="sxs-lookup"><span data-stu-id="be90c-113">In the dropdown, select **编辑开发者信息** (edit developer information).</span></span>
4. <span data-ttu-id="be90c-114">Ange nödvändig information i formuläret och klicka på**提交**(skicka).</span><span class="sxs-lookup"><span data-stu-id="be90c-114">Enter the required information into the form and click **提交** (submit).</span></span>
5. <span data-ttu-id="be90c-115">Slutföra verifieringsprocessen för e-post.</span><span class="sxs-lookup"><span data-stu-id="be90c-115">Complete the email verification process.</span></span>
6. <span data-ttu-id="be90c-116">Gå till den [identitet bekräftelsesidan](http://open.weibo.com/developers/identity/edit).</span><span class="sxs-lookup"><span data-stu-id="be90c-116">Go to the [identity verification page](http://open.weibo.com/developers/identity/edit).</span></span>
7. <span data-ttu-id="be90c-117">Ange nödvändig information i formuläret och klicka på**提交**(skicka).</span><span class="sxs-lookup"><span data-stu-id="be90c-117">Enter the required information into the form and click **提交** (submit).</span></span>

### <a name="register-a-weibo-application"></a><span data-ttu-id="be90c-118">Registrera en Weibo-program</span><span class="sxs-lookup"><span data-stu-id="be90c-118">Register a Weibo application</span></span>

1. <span data-ttu-id="be90c-119">Gå till den [nya Weibo app registreringssidan](http://open.weibo.com/apps/new).</span><span class="sxs-lookup"><span data-stu-id="be90c-119">Go to the [new Weibo app registration page](http://open.weibo.com/apps/new).</span></span>
2. <span data-ttu-id="be90c-120">Ange information om programmet.</span><span class="sxs-lookup"><span data-stu-id="be90c-120">Enter the necessary application information.</span></span>
3. <span data-ttu-id="be90c-121">Klicka på**创建**(skapa).</span><span class="sxs-lookup"><span data-stu-id="be90c-121">Click **创建** (create).</span></span>
4. <span data-ttu-id="be90c-122">Kopiera värdena i **Appkey** och **App hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="be90c-122">Copy the values of **App Key** and **App Secret**.</span></span> <span data-ttu-id="be90c-123">Du behöver det senare.</span><span class="sxs-lookup"><span data-stu-id="be90c-123">You will need this later.</span></span>
5. <span data-ttu-id="be90c-124">Överför fotona som krävs och ange nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="be90c-124">Upload the required photos and enter the necessary information.</span></span>
6. <span data-ttu-id="be90c-125">Klicka på**保存以上信息**(spara).</span><span class="sxs-lookup"><span data-stu-id="be90c-125">Click **保存以上信息** (save).</span></span>
7. <span data-ttu-id="be90c-126">Klicka på**高级信息**(Avancerat information).</span><span class="sxs-lookup"><span data-stu-id="be90c-126">Click **高级信息** (advanced information).</span></span>
8. <span data-ttu-id="be90c-127">Klicka på**编辑**(redigera) bredvid fältet för OAuth2.0**授权设置**(omdirigera URL).</span><span class="sxs-lookup"><span data-stu-id="be90c-127">Click **编辑** (edit) next to the field for OAuth2.0 **授权设置** (redirect URL).</span></span>
9. <span data-ttu-id="be90c-128">Ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` för OAuth2.0**授权设置**(omdirigera URL).</span><span class="sxs-lookup"><span data-stu-id="be90c-128">Enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` for OAuth2.0 **授权设置** (redirect URL).</span></span> <span data-ttu-id="be90c-129">Till exempel om din `tenant_name` är contoso.onmicrosoft.com, ange URL som ska vara `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="be90c-129">For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
10. <span data-ttu-id="be90c-130">Klicka på**提交**(skicka).</span><span class="sxs-lookup"><span data-stu-id="be90c-130">Click **提交** (submit).</span></span>  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="be90c-131">Konfigurera Weibo som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="be90c-131">Configure Weibo as an identity provider in your tenant</span></span>
1. <span data-ttu-id="be90c-132">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="be90c-132">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="be90c-133">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="be90c-133">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="be90c-134">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="be90c-134">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="be90c-135">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="be90c-135">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="be90c-136">Till exempel ange ”Weibo”.</span><span class="sxs-lookup"><span data-stu-id="be90c-136">For example, enter "Weibo".</span></span>
5. <span data-ttu-id="be90c-137">Klicka på **identitet providertyp**väljer **Weibo**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="be90c-137">Click **Identity provider type**, select **Weibo**, and click **OK**.</span></span>
6. <span data-ttu-id="be90c-138">Klicka på **ställa in den här identitetsleverantören.**</span><span class="sxs-lookup"><span data-stu-id="be90c-138">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="be90c-139">Ange den **Appkey** som du kopierade tidigare som den **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="be90c-139">Enter the **App Key** that you copied earlier as the **Client ID**.</span></span>
8. <span data-ttu-id="be90c-140">Ange den **App hemlighet** som du kopierade tidigare som den **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="be90c-140">Enter the **App Secret** that you copied earlier as the **Client Secret**.</span></span>
9. <span data-ttu-id="be90c-141">Klicka på **OK** och klicka sedan på **skapa** att spara konfigurationen Weibo.</span><span class="sxs-lookup"><span data-stu-id="be90c-141">Click **OK** and then click **Create** to save your Weibo configuration.</span></span>

