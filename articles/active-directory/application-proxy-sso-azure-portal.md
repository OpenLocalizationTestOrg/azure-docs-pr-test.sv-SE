---
title: "Enkel inloggning för appar med Azure AD Application Proxy | Microsoft Docs"
description: "Aktivera enkel inloggning för din publicerade lokala program med Azure AD Application Proxy på Azure-portalen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: d94ac3f4-cd33-4c51-9d19-544a528637d4
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/20/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: 9ddc0c1bd5f2cbb24f6761cfd041b820ee6464b8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a><span data-ttu-id="795e2-103">Lösenord vaulting för enkel inloggning med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="795e2-103">Password vaulting for single sign-on with Application Proxy</span></span>

<span data-ttu-id="795e2-104">Azure Active Directory Application Proxy hjälper dig att öka produktiviteten genom att publicera lokala program så att fjärranslutna anställda kan på ett säkert sätt komma åt dem för.</span><span class="sxs-lookup"><span data-stu-id="795e2-104">Azure Active Directory Application Proxy helps you improve productivity by publishing on-premises applications so that remote employees can securely access them, too.</span></span> <span data-ttu-id="795e2-105">I Azure-portalen kan du också ställa in enkel inloggning (SSO) för dessa appar.</span><span class="sxs-lookup"><span data-stu-id="795e2-105">In the Azure portal, you can also set up single sign-on (SSO) to these apps.</span></span> <span data-ttu-id="795e2-106">Användarna behöver bara för att autentisera med Azure AD och de kan komma åt enterprise-programmet utan att behöva logga in igen.</span><span class="sxs-lookup"><span data-stu-id="795e2-106">Your users only need to authenticate with Azure AD, and they can access your enterprise application without having to sign in again.</span></span>

<span data-ttu-id="795e2-107">Application Proxy stöder flera [enkel inloggning lägen](application-proxy-sso-overview.md).</span><span class="sxs-lookup"><span data-stu-id="795e2-107">Application Proxy supports several [single sign-on modes](application-proxy-sso-overview.md).</span></span> <span data-ttu-id="795e2-108">Lösenordsbaserade inloggning är avsedd för program som använder en kombination av användarnamn/lösenord för autentisering.</span><span class="sxs-lookup"><span data-stu-id="795e2-108">Password-based sign-on is intended for applications that use a username/password combination for authentication.</span></span> <span data-ttu-id="795e2-109">När du konfigurerar lösenordsbaserade inloggning för tillämpningsprogrammet har användarna att logga in på en gång programmet lokalt.</span><span class="sxs-lookup"><span data-stu-id="795e2-109">When you configure password-based sign-on for your application, your users have to sign in to the on-premises application once.</span></span> <span data-ttu-id="795e2-110">Efter det Azure Active Directory lagrar information för inloggning och ger den automatiskt till programmet när användarna få fjärråtkomst till den.</span><span class="sxs-lookup"><span data-stu-id="795e2-110">After that, Azure Active Directory stores the sign-in information and automatically provides it to the application when your users access it remotely.</span></span> 

<span data-ttu-id="795e2-111">Du bör redan har publicerats och testat din app med Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="795e2-111">You should already have published and tested your app with Application Proxy.</span></span> <span data-ttu-id="795e2-112">Om inte, följer du stegen i [publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md) sedan återvända hit.</span><span class="sxs-lookup"><span data-stu-id="795e2-112">If not, follow the steps in [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md) then come back here.</span></span> 

## <a name="set-up-password-vaulting-for-your-application"></a><span data-ttu-id="795e2-113">Konfigurera lösenord vaulting för ditt program</span><span class="sxs-lookup"><span data-stu-id="795e2-113">Set up password vaulting for your application</span></span>

1. <span data-ttu-id="795e2-114">Logga in på [Azure Portal](https://portal.azure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="795e2-114">Sign in to the [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="795e2-115">Välj **Azure Active Directory** > **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="795e2-115">Select **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="795e2-116">Välj den app som du vill konfigurera med enkel inloggning i listan.</span><span class="sxs-lookup"><span data-stu-id="795e2-116">From the list, select the app that you want to set up with SSO.</span></span>  
4. <span data-ttu-id="795e2-117">Välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="795e2-117">Select **Single sign-on**.</span></span>

   ![Välja enkel inloggning](./media/application-proxy-sso-azure-portal/select-sso.png)

5. <span data-ttu-id="795e2-119">Läget för enkel inloggning väljer **lösenordsbaserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="795e2-119">For the SSO mode, choose **Password-based Sign-on**.</span></span>
6. <span data-ttu-id="795e2-120">För URL inloggning anger du Webbadressen för sidan där användarna anger sina användarnamn och lösenord för att logga in på din app utanför företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="795e2-120">For the Sign-on URL, enter the URL for the page where users enter their username and password to sign in to your app outside of the corporate network.</span></span> <span data-ttu-id="795e2-121">Detta kan vara en extern URL som du skapade när du har publicerat appen via Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="795e2-121">This may be the External URL that you created when you published the app through Application Proxy.</span></span> 

   ![Välj lösenordsbaserade inloggning och ange URL: en](./media/application-proxy-sso-azure-portal/password-sso.png)

7. <span data-ttu-id="795e2-123">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="795e2-123">Select **Save**.</span></span>

<!-- Need to repro?
7. The page should tell you that a sign-in form was successfully detected at the provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow the instructions to point out where the sign-in credentials go. 
-->

## <a name="test-your-app"></a><span data-ttu-id="795e2-124">Testa din app</span><span class="sxs-lookup"><span data-stu-id="795e2-124">Test your app</span></span>

<span data-ttu-id="795e2-125">Gå till externa URL: en som du konfigurerade för fjärråtkomst till ditt program.</span><span class="sxs-lookup"><span data-stu-id="795e2-125">Go to external URL that you configured for remote access to your application.</span></span> <span data-ttu-id="795e2-126">Logga in med dina autentiseringsuppgifter för appen (eller autentiseringsuppgifterna för ett testkonto som du skapat med åtkomst).</span><span class="sxs-lookup"><span data-stu-id="795e2-126">Sign in with your credentials for that app (or the credentials for a test account that you set up with access).</span></span> <span data-ttu-id="795e2-127">När du loggar in har bör du kunna lämna appen och gå tillbaka utan att ange dina autentiseringsuppgifter igen.</span><span class="sxs-lookup"><span data-stu-id="795e2-127">Once you sign in successfully, you should be able to leave the app and come back without entering your credentials again.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="795e2-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="795e2-128">Next steps</span></span>

- <span data-ttu-id="795e2-129">Läs mer om andra sätt att implementera [enkel inloggning med Application Proxy](application-proxy-sso-overview.md)</span><span class="sxs-lookup"><span data-stu-id="795e2-129">Read about other ways to implement [Single sign-on with Application Proxy](application-proxy-sso-overview.md)</span></span>
- <span data-ttu-id="795e2-130">Lär dig mer om [säkerhetsaspekter för att komma åt appar med Azure AD Application Proxy](application-proxy-security-considerations.md)</span><span class="sxs-lookup"><span data-stu-id="795e2-130">Learn about [Security considerations for accessing apps remotely with Azure AD Application Proxy](application-proxy-security-considerations.md)</span></span>