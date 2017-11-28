---
title: 'Azure Active Directory B2C: WeChat konfiguration | Microsoft Docs'
description: Ge registrering och inloggning tooconsumers WeChat konton i dina program som skyddas av Azure Active Directory B2C.
services: active-directory-b2c
documentationcenter: 
author: parakhj
manager: krassk
editor: parakhj
ms.assetid: d2424c66-ba68-4d82-847e-d137683527b0
ms.service: active-directory-b2c
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 3/26/2017
ms.author: parakhj
ms.openlocfilehash: 92cc3579d818d2379a503ccc695138b33a34466d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-tooconsumers-with-wechat-accounts"></a><span data-ttu-id="fff4d-103">Azure Active Directory B2C: Ge registrering och inloggning tooconsumers WeChat konton</span><span class="sxs-lookup"><span data-stu-id="fff4d-103">Azure Active Directory B2C: Provide sign-up and sign-in tooconsumers with WeChat accounts</span></span>

> [!NOTE]
> <span data-ttu-id="fff4d-104">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="fff4d-104">This feature is in preview.</span></span>
> 

## <a name="create-a-wechat-application"></a><span data-ttu-id="fff4d-105">Skapa ett WeChat program</span><span class="sxs-lookup"><span data-stu-id="fff4d-105">Create a WeChat application</span></span>

<span data-ttu-id="fff4d-106">toouse WeChat som en identitetsleverantör i Azure Active Directory (AD Azure) B2C du behöver toocreate ett WeChat program och lämna hello rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="fff4d-106">toouse WeChat as an identity provider in Azure Active Directory (Azure AD) B2C, you need toocreate a WeChat application and supply it with hello right parameters.</span></span> <span data-ttu-id="fff4d-107">Du behöver en WeChat konto toodo detta.</span><span class="sxs-lookup"><span data-stu-id="fff4d-107">You need a WeChat account toodo this.</span></span> <span data-ttu-id="fff4d-108">Om du inte har någon kan skaffa du en genom att registrera dig genom en av sina mobila appar eller genom att använda QT-nummer.</span><span class="sxs-lookup"><span data-stu-id="fff4d-108">If you don’t have one, you can get one by signing up through one of their mobile apps or by using your QQ number.</span></span> <span data-ttu-id="fff4d-109">Efter det att få ditt konto som har registrerats med hello WeChat developer-programmet.</span><span class="sxs-lookup"><span data-stu-id="fff4d-109">After that, get your account registered with hello WeChat developer program.</span></span> <span data-ttu-id="fff4d-110">Du hittar mer information [här](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span><span class="sxs-lookup"><span data-stu-id="fff4d-110">You can find more information [here](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span></span>

### <a name="register-a-wechat-application"></a><span data-ttu-id="fff4d-111">Registrera en WeChat-program</span><span class="sxs-lookup"><span data-stu-id="fff4d-111">Register a WeChat application</span></span>

1. <span data-ttu-id="fff4d-112">Gå för[https://open.weixin.qq.com/](https://open.weixin.qq.com/) och logga in.</span><span class="sxs-lookup"><span data-stu-id="fff4d-112">Go too[https://open.weixin.qq.com/](https://open.weixin.qq.com/) and log in.</span></span>
2. <span data-ttu-id="fff4d-113">Klicka på**管理中心**(management center).</span><span class="sxs-lookup"><span data-stu-id="fff4d-113">Click on **管理中心** (management center).</span></span>
3. <span data-ttu-id="fff4d-114">Följ hello nödvändiga åtgärder tooregister ett nytt program.</span><span class="sxs-lookup"><span data-stu-id="fff4d-114">Follow hello necessary steps tooregister a new application.</span></span>
4. <span data-ttu-id="fff4d-115">För**授权回调域**(återanrop URL), ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="fff4d-115">For **授权回调域** (callback URL), enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="fff4d-116">Till exempel om din `tenant_name` är contoso.onmicrosoft.com set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="fff4d-116">For example, if your `tenant_name` is contoso.onmicrosoft.com, set hello URL toobe `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
5. <span data-ttu-id="fff4d-117">Hitta och kopiera hello **APP-ID** och **APPKEY**.</span><span class="sxs-lookup"><span data-stu-id="fff4d-117">Find and copy hello **APP ID** and **APP KEY**.</span></span> <span data-ttu-id="fff4d-118">Du behöver dessa senare.</span><span class="sxs-lookup"><span data-stu-id="fff4d-118">You will need these later.</span></span>

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="fff4d-119">Konfigurera WeChat som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="fff4d-119">Configure WeChat as an identity provider in your tenant</span></span>
1. <span data-ttu-id="fff4d-120">Följ dessa steg för[navigera toohello B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fff4d-120">Follow these steps too[navigate toohello B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on hello Azure portal.</span></span>
2. <span data-ttu-id="fff4d-121">Klicka på hello B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="fff4d-121">On hello B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="fff4d-122">Klicka på **+ Lägg till** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="fff4d-122">Click **+Add** at hello top of hello blade.</span></span>
4. <span data-ttu-id="fff4d-123">Ange ett eget **namn** för hello identitet Providerkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="fff4d-123">Provide a friendly **Name** for hello identity provider configuration.</span></span> <span data-ttu-id="fff4d-124">Till exempel ange ”WeChat”.</span><span class="sxs-lookup"><span data-stu-id="fff4d-124">For example, enter "WeChat".</span></span>
5. <span data-ttu-id="fff4d-125">Klicka på **identitet providertyp**väljer **WeChat**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="fff4d-125">Click **Identity provider type**, select **WeChat**, and click **OK**.</span></span>
6. <span data-ttu-id="fff4d-126">Klicka på **ställa in den här identitetsleverantören.**</span><span class="sxs-lookup"><span data-stu-id="fff4d-126">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="fff4d-127">Ange hello **Appkey** som du kopierade tidigare som hello **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="fff4d-127">Enter hello **App Key** that you copied earlier as hello **Client ID**.</span></span>
8. <span data-ttu-id="fff4d-128">Ange hello **App hemlighet** som du kopierade tidigare som hello **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="fff4d-128">Enter hello **App Secret** that you copied earlier as hello **Client Secret**.</span></span>
9. <span data-ttu-id="fff4d-129">Klicka på **OK** och klicka sedan på **skapa** toosave WeChat konfigurationen.</span><span class="sxs-lookup"><span data-stu-id="fff4d-129">Click **OK** and then click **Create** toosave your WeChat configuration.</span></span>

