---
title: 'Azure Active Directory B2C: WeChat konfiguration | Microsoft Docs'
description: "Ange registrering och inloggning för konsumenter med WeChat konton i dina program som skyddas av Azure Active Directory B2C."
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
ms.openlocfilehash: a54aec23d951610118246e9f70cdd27752ef39a6
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-b2c-provide-sign-up-and-sign-in-to-consumers-with-wechat-accounts"></a><span data-ttu-id="ce5ff-103">Azure Active Directory B2C: Ange registrering och inloggning för konsumenter med WeChat konton</span><span class="sxs-lookup"><span data-stu-id="ce5ff-103">Azure Active Directory B2C: Provide sign-up and sign-in to consumers with WeChat accounts</span></span>

> [!NOTE]
> <span data-ttu-id="ce5ff-104">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-104">This feature is in preview.</span></span>
> 

## <a name="create-a-wechat-application"></a><span data-ttu-id="ce5ff-105">Skapa ett WeChat program</span><span class="sxs-lookup"><span data-stu-id="ce5ff-105">Create a WeChat application</span></span>

<span data-ttu-id="ce5ff-106">Om du vill använda WeChat som en identitetsleverantör i Azure Active Directory (AD Azure) B2C måste du skapa ett WeChat program och ange rätt parametrar.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-106">To use WeChat as an identity provider in Azure Active Directory (Azure AD) B2C, you need to create a WeChat application and supply it with the right parameters.</span></span> <span data-ttu-id="ce5ff-107">Du behöver en WeChat-konto för att göra detta.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-107">You need a WeChat account to do this.</span></span> <span data-ttu-id="ce5ff-108">Om du inte har någon kan skaffa du en genom att registrera dig genom en av sina mobila appar eller genom att använda QT-nummer.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-108">If you don’t have one, you can get one by signing up through one of their mobile apps or by using your QQ number.</span></span> <span data-ttu-id="ce5ff-109">Efter det att få ditt konto som har registrerats med utvecklarprogram WeChat.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-109">After that, get your account registered with the WeChat developer program.</span></span> <span data-ttu-id="ce5ff-110">Du hittar mer information [här](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span><span class="sxs-lookup"><span data-stu-id="ce5ff-110">You can find more information [here](http://kf.qq.com/faq/161220Brem2Q161220uUjERB.html).</span></span>

### <a name="register-a-wechat-application"></a><span data-ttu-id="ce5ff-111">Registrera en WeChat-program</span><span class="sxs-lookup"><span data-stu-id="ce5ff-111">Register a WeChat application</span></span>

1. <span data-ttu-id="ce5ff-112">Gå till [https://open.weixin.qq.com/](https://open.weixin.qq.com/) och logga in.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-112">Go to [https://open.weixin.qq.com/](https://open.weixin.qq.com/) and log in.</span></span>
2. <span data-ttu-id="ce5ff-113">Klicka på**管理中心**(management center).</span><span class="sxs-lookup"><span data-stu-id="ce5ff-113">Click on **管理中心** (management center).</span></span>
3. <span data-ttu-id="ce5ff-114">Följ de nödvändiga stegen för att registrera ett nytt program.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-114">Follow the necessary steps to register a new application.</span></span>
4. <span data-ttu-id="ce5ff-115">För**授权回调域**(återanrop URL), ange `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-115">For **授权回调域** (callback URL), enter `https://login.microsoftonline.com/te/{tenant_name}/oauth2/authresp`.</span></span> <span data-ttu-id="ce5ff-116">Till exempel om din `tenant_name` är contoso.onmicrosoft.com, ange URL som ska vara `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-116">For example, if your `tenant_name` is contoso.onmicrosoft.com, set the URL to be `https://login.microsoftonline.com/te/contoso.onmicrosoft.com/oauth2/authresp`.</span></span>
5. <span data-ttu-id="ce5ff-117">Hitta och kopiera den **APP-ID** och **APPKEY**.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-117">Find and copy the **APP ID** and **APP KEY**.</span></span> <span data-ttu-id="ce5ff-118">Du behöver dessa senare.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-118">You will need these later.</span></span>

## <a name="configure-wechat-as-an-identity-provider-in-your-tenant"></a><span data-ttu-id="ce5ff-119">Konfigurera WeChat som en identitetsleverantör i din klientorganisation</span><span class="sxs-lookup"><span data-stu-id="ce5ff-119">Configure WeChat as an identity provider in your tenant</span></span>
1. <span data-ttu-id="ce5ff-120">Följ dessa steg för att [gå till B2C-funktionsbladet](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-120">Follow these steps to [navigate to the B2C features blade](active-directory-b2c-app-registration.md#navigate-to-b2c-settings) on the Azure portal.</span></span>
2. <span data-ttu-id="ce5ff-121">Klicka på B2C-funktionsbladet **identitetsleverantörer**.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-121">On the B2C features blade, click **Identity providers**.</span></span>
3. <span data-ttu-id="ce5ff-122">Klicka på **+Lägg till** överst på bladet.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-122">Click **+Add** at the top of the blade.</span></span>
4. <span data-ttu-id="ce5ff-123">Ange ett eget **namn** för providerkonfigurationen identitet.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-123">Provide a friendly **Name** for the identity provider configuration.</span></span> <span data-ttu-id="ce5ff-124">Till exempel ange ”WeChat”.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-124">For example, enter "WeChat".</span></span>
5. <span data-ttu-id="ce5ff-125">Klicka på **identitet providertyp**väljer **WeChat**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-125">Click **Identity provider type**, select **WeChat**, and click **OK**.</span></span>
6. <span data-ttu-id="ce5ff-126">Klicka på **ställa in den här identitetsleverantören.**</span><span class="sxs-lookup"><span data-stu-id="ce5ff-126">Click **Set up this identity provider**</span></span>
7. <span data-ttu-id="ce5ff-127">Ange den **Appkey** som du kopierade tidigare som den **klient-ID**.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-127">Enter the **App Key** that you copied earlier as the **Client ID**.</span></span>
8. <span data-ttu-id="ce5ff-128">Ange den **App hemlighet** som du kopierade tidigare som den **Klienthemlighet**.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-128">Enter the **App Secret** that you copied earlier as the **Client Secret**.</span></span>
9. <span data-ttu-id="ce5ff-129">Klicka på **OK** och klicka sedan på **skapa** att spara konfigurationen WeChat.</span><span class="sxs-lookup"><span data-stu-id="ce5ff-129">Click **OK** and then click **Create** to save your WeChat configuration.</span></span>

