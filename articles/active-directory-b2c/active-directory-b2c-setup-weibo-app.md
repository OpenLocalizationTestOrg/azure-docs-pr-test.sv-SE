---
title: 'Azure Active Directory B2C: Weibo konfiguration | Microsoft Docs'
description: Ge registrering och inloggning tooconsumers Weibo konton i dina program som skyddas av Azure Active Directory B2C.
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
ms.openlocfilehash: c0648620f318046c1d9d24feb92a0c5f19c6a91a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-weibo-accounts"></a><span data-ttu-id="dcc17-103">Azure Active Directory B2C: Ge registrering och inloggning tooconsumers Weibo konton</span><span class="sxs-lookup"><span data-stu-id="dcc17-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with Weibo accounts</span></span>

> [!NOTE]
> <span data-ttu-id="dcc17-104">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="dcc17-104">This feature is in preview.</span></span> <span data-ttu-id="dcc17-105">Använd inte den här identitetsleverantör i produktionsmiljön.</span><span class="sxs-lookup"><span data-stu-id="dcc17-105">Do not use this identity provider in your production environment.</span></span>
> 

## <a name="create-a-weibo-application"></a><span data-ttu-id="dcc17-106">Skapa ett Weibo program</span><span class="sxs-lookup"><span data-stu-id="dcc17-106">Create a Weibo application</span></span>

<span data-ttu-id="dcc17-107">toouse Weibo som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett Weibo program och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="dcc17-107">toouse Weibo as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a Weibo application and supply it with hello right parameters.</span></span> <span data-ttu-id="dcc17-108">Du behöver en Weibo konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="dcc17-108">You need a Weibo account toodo this.</span></span> <span data-ttu-id="dcc17-109">Om du inte har någon kan du skaffa ett på [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span><span class="sxs-lookup"><span data-stu-id="dcc17-109">If you don’t have one, you can get one at [http://weibo.com/signup/signup.php?lang=en-us](http://weibo.com/signup/signup.php?lang=en-us).</span></span>

### <a name="register-for-hello-weibo-developer-program"></a><span data-ttu-id="dcc17-110">Registrera dig för hello Weibo utvecklarprogram</span><span class="sxs-lookup"><span data-stu-id="dcc17-110">Register for hello Weibo developer program</span></span>

1. <span data-ttu-id="dcc17-111">Gå toohello [Weibo utvecklarportalen](http://open.weibo.com/) och logga in med autentiseringsuppgifterna för ditt Weibo.</span><span class="sxs-lookup"><span data-stu-id="dcc17-111">Go toohello [Weibo developer portal](http://open.weibo.com/) and sign in with your Weibo account credentials.</span></span>
2. <span data-ttu-id="dcc17-112">När du har loggat in, klicka på ditt namn i hello övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="dcc17-112">After signing in, click on your display name in hello top-right corner.</span></span>
3. <span data-ttu-id="dcc17-113">Välj i listrutan hello**编辑开发者信息**(redigera information för utvecklare).</span><span class="sxs-lookup"><span data-stu-id="dcc17-113">In hello dropdown, select **编辑开发者信息** (edit developer information).</span></span>
4. <span data-ttu-id="dcc17-114">Ange hello krävs information till hello format och på**提交**(skicka).</span><span class="sxs-lookup"><span data-stu-id="dcc17-114">Enter hello required information into hello form and click **提交** (submit).</span></span>
5. <span data-ttu-id="dcc17-115">Slutföra verifieringsprocessen för hello e-post.</span><span class="sxs-lookup"><span data-stu-id="dcc17-115">Complete hello email verification process.</span></span>
6. <span data-ttu-id="dcc17-116">Gå toohello [identitet bekräftelsesidan](http://open.weibo.com/developers/identity/edit).</span><span class="sxs-lookup"><span data-stu-id="dcc17-116">Go toohello [identity verification page](http://open.weibo.com/developers/identity/edit).</span></span>
7. <span data-ttu-id="dcc17-117">Ange hello krävs information till hello format och på**提交**(skicka).</span><span class="sxs-lookup"><span data-stu-id="dcc17-117">Enter hello required information into hello form and click **提交** (submit).</span></span>

### <a name="register-a-weibo-application"></a><span data-ttu-id="dcc17-118">Registrera en Weibo-program</span><span class="sxs-lookup"><span data-stu-id="dcc17-118">Register a Weibo application</span></span>

1. <span data-ttu-id="dcc17-119">Gå toohello [nya Weibo app registreringssidan](http://open.weibo.com/apps/new).</span><span class="sxs-lookup"><span data-stu-id="dcc17-119">Go toohello [new Weibo app registration page](http://open.weibo.com/apps/new).</span></span>
2. <span data-ttu-id="dcc17-120">Ange information om programmet hello.</span><span class="sxs-lookup"><span data-stu-id="dcc17-120">Enter hello necessary application information.</span></span>
3. <span data-ttu-id="dcc17-121">Klicka på**创建**(skapa).</span><span class="sxs-lookup"><span data-stu-id="dcc17-121">Click **创建** (create).</span></span>
4. <span data-ttu-id="dcc17-122">Kopiera hello värdena för **Appkey** och **App hemlighet**.</span><span class="sxs-lookup"><span data-stu-id="dcc17-122">Copy hello values of **App Key** and **App Secret**.</span></span> <span data-ttu-id="dcc17-123">Du behöver det senare.</span><span class="sxs-lookup"><span data-stu-id="dcc17-123">You will need this later.</span></span>
5. <span data-ttu-id="dcc17-124">Överför hello krävs foton och ange hello nödvändig information.</span><span class="sxs-lookup"><span data-stu-id="dcc17-124">Upload hello required photos and enter hello necessary information.</span></span>
6. <span data-ttu-id="dcc17-125">Klicka på**保存以上信息**(spara).</span><span class="sxs-lookup"><span data-stu-id="dcc17-125">Click **保存以上信息** (save).</span></span>
7. <span data-ttu-id="dcc17-126">Klicka på**高级信息**(Avancerat information).</span><span class="sxs-lookup"><span data-stu-id="dcc17-126">Click **高级信息** (advanced information).</span></span>
8. <span data-ttu-id="dcc17-127">Klicka på**编辑**(redigera) nästa toohello fält för OAuth2.0**授权设置**(omdirigera URL).</span><span class="sxs-lookup"><span data-stu-id="dcc17-127">Click **编辑** (edit) next toohello field for OAuth2.0 **授权设置** (redirect URL).</span></span>
9. <span data-ttu-id="dcc17-128">Ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` för OAuth2.0**授权设置**(omdirigera URL).</span><span class="sxs-lookup"><span data-stu-id="dcc17-128">Enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp` for OAuth2.0 **授权设置** (redirect URL).</span></span> <span data-ttu-id="dcc17-129">Till exempel om din `tenant_name` är contoso.onmicrosoft.com set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="dcc17-129">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
10. <span data-ttu-id="dcc17-130">Klicka på**提交**(skicka).</span><span class="sxs-lookup"><span data-stu-id="dcc17-130">Click **提交** (submit).</span></span>  

## <a name="configure-weibo-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="dcc17-131">Konfigurera Weibo som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="dcc17-131">Configure Weibo as an identity provider in your tenant</span></span>
1. <span data-ttu-id="dcc17-132">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="dcc17-132">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="dcc17-133">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="dcc17-133">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="dcc17-134">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="dcc17-134">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="dcc17-135">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="dcc17-135">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="dcc17-136">Till exempel ange ”Weibo”.</span><span class="sxs-lookup"><span data-stu-id="dcc17-136">For example, enter "Weibo".</span></span>
5. <span data-ttu-id="dcc17-137">Klicka på **identitet providertyp**väljer **Weibo**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="dcc17-137">Click **Identity provider type**, select **Weibo**, and click **OK**.</span></span>
6. <span data-ttu-id="dcc17-138">Klicka på **ställa in den här identitetsleverantören.**</span><span class="sxs-lookup"><span data-stu-id="dcc17-138">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="dcc17-139">Ange hello **Appkey** som du kopierade tidigare som hello **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="dcc17-139">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="dcc17-140">Ange hello **App hemlighet** som du kopierade tidigare som hello **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="dcc17-140">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="dcc17-141">Klicka på **OK** och klicka sedan på **skapa** toosave Weibo konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="dcc17-141">Click **OK** and then click **Create** toosave your Weibo configuration.</span></span>

