---
title: 'Azure Active Directory B2C: QT konfiguration | Microsoft Docs'
description: Ge registrering och inloggning tooconsumers QT konton i dina program som skyddas av Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: 18c2cf94-8004-4de1-81c2-e45be65ce12d
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 896d6221e01d15de1652a5717cf1f65619101e0c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-qq-accounts"></a><span data-ttu-id="351ae-103">Azure Active Directory B2C: Ge registrering och inloggning tooconsumers QT konton</span><span class="sxs-lookup"><span data-stu-id="351ae-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with QQ accounts</span></span>

> [!NOTE]
> <span data-ttu-id="351ae-104">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="351ae-104">This feature is in preview.</span></span>
> 

## <a name="create-a-qq-application"></a><span data-ttu-id="351ae-105">Skapa ett QT-program</span><span class="sxs-lookup"><span data-stu-id="351ae-105">Create a QQ application</span></span>

<span data-ttu-id="351ae-106">toouse QT som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett QT program och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="351ae-106">toouse QQ as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a QQ application and supply it with hello right parameters.</span></span> <span data-ttu-id="351ae-107">Du behöver en QT konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="351ae-107">You need a QQ account toodo this.</span></span> <span data-ttu-id="351ae-108">Om du inte har någon kan du skaffa ett på [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span><span class="sxs-lookup"><span data-stu-id="351ae-108">If you don’t have one, you can get one at [https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033](https://ssl.zc.qq.com/en/index.html?type=1&ptlang=1033).</span></span>

### <a name="register-for-hello-qq-developer-program"></a><span data-ttu-id="351ae-109">Registrera dig för hello QT utvecklarprogram</span><span class="sxs-lookup"><span data-stu-id="351ae-109">Register for hello QQ developer program</span></span>

1. <span data-ttu-id="351ae-110">Gå toohello [QT developer-portalen](http://open.qq.com) och logga in med autentiseringsuppgifterna för ditt QT.</span><span class="sxs-lookup"><span data-stu-id="351ae-110">Go toohello [QQ developer portal](http://open.qq.com) and sign in with your QQ account credentials.</span></span>
2. <span data-ttu-id="351ae-111">Efter inloggningen gå för[http://open.qq.com/reg](http://open.qq.com/reg) tooregister själv som utvecklare.</span><span class="sxs-lookup"><span data-stu-id="351ae-111">After signing in, go too[http://open.qq.com/reg](http://open.qq.com/reg) tooregister yourself as a developer.</span></span>
3. <span data-ttu-id="351ae-112">Välj menyn hello**个人**(enskilda developer).</span><span class="sxs-lookup"><span data-stu-id="351ae-112">In hello menu, select **个人** (individual developer).</span></span>
4. <span data-ttu-id="351ae-113">Ange hello krävs information till hello format och på**下一步**(nästa steg).</span><span class="sxs-lookup"><span data-stu-id="351ae-113">Enter hello required information into hello form and click **下一步** (next step).</span></span>
5. <span data-ttu-id="351ae-114">Slutföra verifieringsprocessen för hello e-post.</span><span class="sxs-lookup"><span data-stu-id="351ae-114">Complete hello email verification process.</span></span>

> [!NOTE]
> <span data-ttu-id="351ae-115">Du behöver toowait några dagar toobe godkända efter registrering som utvecklare.</span><span class="sxs-lookup"><span data-stu-id="351ae-115">You will need toowait a few days toobe approved after registering as a developer.</span></span> 

### <a name="register-a-qq-application"></a><span data-ttu-id="351ae-116">Registrera en QT-program</span><span class="sxs-lookup"><span data-stu-id="351ae-116">Register a QQ application</span></span>

1. <span data-ttu-id="351ae-117">Gå för[https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span><span class="sxs-lookup"><span data-stu-id="351ae-117">Go too[https://connect.qq.com/index.html](https://connect.qq.com/index.html).</span></span>
2. <span data-ttu-id="351ae-118">Klicka på**应用管理**(hantering).</span><span class="sxs-lookup"><span data-stu-id="351ae-118">Click on **应用管理** (app management).</span></span>
3. <span data-ttu-id="351ae-119">Klicka på**创建应用**(skapa app).</span><span class="sxs-lookup"><span data-stu-id="351ae-119">Click on **创建应用** (create app).</span></span>
4. <span data-ttu-id="351ae-120">Ange information om hello nödvändiga app.</span><span class="sxs-lookup"><span data-stu-id="351ae-120">Enter hello necessary app information.</span></span>
5. <span data-ttu-id="351ae-121">Klicka på**创建应用**(skapa app).</span><span class="sxs-lookup"><span data-stu-id="351ae-121">Click on **创建应用** (create app).</span></span>
6. <span data-ttu-id="351ae-122">Ange information om hello krävs.</span><span class="sxs-lookup"><span data-stu-id="351ae-122">Enter hello required information.</span></span>
7. <span data-ttu-id="351ae-123">För hello**授权回调域**(återanrop URL), ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="351ae-123">For hello **授权回调域** (callback URL) field, enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="351ae-124">Till exempel om din `tenant_name` är contoso.onmicrosoft.com set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="351ae-124">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
8. <span data-ttu-id="351ae-125">Klicka på**创建应用**(skapa app).</span><span class="sxs-lookup"><span data-stu-id="351ae-125">Click on **创建应用** (create app).</span></span>
9. <span data-ttu-id="351ae-126">På sidan Bekräfta hello klickar du på**应用管理**sidan för hantering (hantering) tooreturn toohello app.</span><span class="sxs-lookup"><span data-stu-id="351ae-126">On hello confirmation page, click on **应用管理** (app management) tooreturn toohello app management page.</span></span>
10. <span data-ttu-id="351ae-127">Klicka på**查看**(Visa) nästa toohello app som du nyss skapade.</span><span class="sxs-lookup"><span data-stu-id="351ae-127">Click on **查看** (view) next toohello app you just created.</span></span>
11. <span data-ttu-id="351ae-128">Klicka på**修改**(redigera).</span><span class="sxs-lookup"><span data-stu-id="351ae-128">Click on **修改** (edit).</span></span>
12. <span data-ttu-id="351ae-129">Kopiera från hello överst på sidan för hello, hello **APP-ID** och **APPKEY**.</span><span class="sxs-lookup"><span data-stu-id="351ae-129">From hello top of hello page, copy hello **APP ID** and **APP KEY**.</span></span>

## <a name="configure-qq-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="351ae-130">Konfigurera QT som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="351ae-130">Configure QQ as an identity provider in your tenant</span></span>
1. <span data-ttu-id="351ae-131">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="351ae-131">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="351ae-132">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="351ae-132">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="351ae-133">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="351ae-133">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="351ae-134">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="351ae-134">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="351ae-135">Till exempel ange ”QT”.</span><span class="sxs-lookup"><span data-stu-id="351ae-135">For example, enter "QQ".</span></span>
5. <span data-ttu-id="351ae-136">Klicka på **identitet providertyp**väljer **QT**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="351ae-136">Click **Identity provider type**, select **QQ**, and click **OK**.</span></span>
6. <span data-ttu-id="351ae-137">Klicka på **ställa in den här identitetsleverantören.**</span><span class="sxs-lookup"><span data-stu-id="351ae-137">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="351ae-138">Ange hello **Appkey** som du kopierade tidigare som hello **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="351ae-138">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="351ae-139">Ange hello **App hemlighet** som du kopierade tidigare som hello **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="351ae-139">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="351ae-140">Klicka på **OK** och klicka sedan på **skapa** toosave QT konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="351ae-140">Click **OK** and then click **Create** toosave your QQ configuration.</span></span>

