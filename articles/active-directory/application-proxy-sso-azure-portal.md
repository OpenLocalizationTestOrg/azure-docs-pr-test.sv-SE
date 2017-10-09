---
title: aaaSingle inloggning tooapps med Azure AD Application Proxy | Microsoft Docs
description: "Aktivera enkel inloggning för din publicerade lokala program med Azure AD Application Proxy i hello Azure-portalen."
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
ms.openlocfilehash: 5ff288d36163b74215677d9e34c93c985ac33d54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="password-vaulting-for-single-sign-on-with-application-proxy"></a><span data-ttu-id="377db-103">Lösenord vaulting för enkel inloggning med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="377db-103">Password vaulting for single sign-on with Application Proxy</span></span>

<span data-ttu-id="377db-104">Azure Active Directory Application Proxy hjälper dig att öka produktiviteten genom att publicera lokala program så att fjärranslutna anställda kan på ett säkert sätt komma åt dem för.</span><span class="sxs-lookup"><span data-stu-id="377db-104">Azure Active Directory Application Proxy helps you improve productivity by publishing on-premises applications so that remote employees can securely access them, too.</span></span> <span data-ttu-id="377db-105">I hello Azure-portalen, kan du också konfigurera enkel inloggning (SSO) toothese appar.</span><span class="sxs-lookup"><span data-stu-id="377db-105">In hello Azure portal, you can also set up single sign-on (SSO) toothese apps.</span></span> <span data-ttu-id="377db-106">Användarna behöver bara tooauthenticate med Azure AD och de kan komma åt enterprise-programmet utan att behöva toosign igen.</span><span class="sxs-lookup"><span data-stu-id="377db-106">Your users only need tooauthenticate with Azure AD, and they can access your enterprise application without having toosign in again.</span></span>

<span data-ttu-id="377db-107">Application Proxy stöder flera [enkel inloggning lägen](application-proxy-sso-overview.md).</span><span class="sxs-lookup"><span data-stu-id="377db-107">Application Proxy supports several [single sign-on modes](application-proxy-sso-overview.md).</span></span> <span data-ttu-id="377db-108">Lösenordsbaserade inloggning är avsedd för program som använder en kombination av användarnamn/lösenord för autentisering.</span><span class="sxs-lookup"><span data-stu-id="377db-108">Password-based sign-on is intended for applications that use a username/password combination for authentication.</span></span> <span data-ttu-id="377db-109">Användarna har toosign i toohello lokalt program en gång när du konfigurerar lösenordsbaserade inloggning för ditt program.</span><span class="sxs-lookup"><span data-stu-id="377db-109">When you configure password-based sign-on for your application, your users have toosign in toohello on-premises application once.</span></span> <span data-ttu-id="377db-110">Efter det Azure Active Directory lagrar hello inloggningsinformation och ger automatiskt den toohello programmet när användarna få fjärråtkomst till den.</span><span class="sxs-lookup"><span data-stu-id="377db-110">After that, Azure Active Directory stores hello sign-in information and automatically provides it toohello application when your users access it remotely.</span></span> 

<span data-ttu-id="377db-111">Du bör redan har publicerats och testat din app med Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="377db-111">You should already have published and tested your app with Application Proxy.</span></span> <span data-ttu-id="377db-112">Om inte, gör hello i [publicera program med Azure AD Application Proxy](application-proxy-publish-azure-portal.md) sedan återvända hit.</span><span class="sxs-lookup"><span data-stu-id="377db-112">If not, follow hello steps in [Publish applications using Azure AD Application Proxy](application-proxy-publish-azure-portal.md) then come back here.</span></span> 

## <a name="set-up-password-vaulting-for-your-application"></a><span data-ttu-id="377db-113">Konfigurera lösenord vaulting för ditt program</span><span class="sxs-lookup"><span data-stu-id="377db-113">Set up password vaulting for your application</span></span>

1. <span data-ttu-id="377db-114">Logga in toohello [Azure-portalen](https://portal.azure.com) som administratör.</span><span class="sxs-lookup"><span data-stu-id="377db-114">Sign in toohello [Azure portal](https://portal.azure.com) as an administrator.</span></span>
2. <span data-ttu-id="377db-115">Välj **Azure Active Directory** > **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="377db-115">Select **Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="377db-116">Välj hello listan hello app som du vill tooset upp med enkel inloggning.</span><span class="sxs-lookup"><span data-stu-id="377db-116">From hello list, select hello app that you want tooset up with SSO.</span></span>  
4. <span data-ttu-id="377db-117">Välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="377db-117">Select **Single sign-on**.</span></span>

   ![Välja enkel inloggning](./media/application-proxy-sso-azure-portal/select-sso.png)

5. <span data-ttu-id="377db-119">Hello SSO-läge, Välj **lösenordsbaserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="377db-119">For hello SSO mode, choose **Password-based Sign-on**.</span></span>
6. <span data-ttu-id="377db-120">För hello inloggnings-URL, anger du hello URL för hello sida där användare kan ange sina användarnamn och lösenord toosign tooyour app utanför hello företagsnätverket.</span><span class="sxs-lookup"><span data-stu-id="377db-120">For hello Sign-on URL, enter hello URL for hello page where users enter their username and password toosign in tooyour app outside of hello corporate network.</span></span> <span data-ttu-id="377db-121">Detta kan vara hello extern URL som du skapade när du har publicerat hello appen via Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="377db-121">This may be hello External URL that you created when you published hello app through Application Proxy.</span></span> 

   ![Välj lösenordsbaserade inloggning och ange URL: en](./media/application-proxy-sso-azure-portal/password-sso.png)

7. <span data-ttu-id="377db-123">Välj **Spara**.</span><span class="sxs-lookup"><span data-stu-id="377db-123">Select **Save**.</span></span>

<!-- Need toorepro?
7. hello page should tell you that a sign-in form was successfully detected at hello provided URL. If it doesn't, select **Configure [your app name] Password Single Sign-on Settings** and choose **Manually detect sign-in fields**. Follow hello instructions toopoint out where hello sign-in credentials go. 
-->

## <a name="test-your-app"></a><span data-ttu-id="377db-124">Testa din app</span><span class="sxs-lookup"><span data-stu-id="377db-124">Test your app</span></span>

<span data-ttu-id="377db-125">Gå tooexternal URL som du konfigurerade för fjärråtkomst tooyour program.</span><span class="sxs-lookup"><span data-stu-id="377db-125">Go tooexternal URL that you configured for remote access tooyour application.</span></span> <span data-ttu-id="377db-126">Logga in med dina autentiseringsuppgifter för appen (eller hello autentiseringsuppgifter för ett testkonto som du skapat med åtkomst).</span><span class="sxs-lookup"><span data-stu-id="377db-126">Sign in with your credentials for that app (or hello credentials for a test account that you set up with access).</span></span> <span data-ttu-id="377db-127">När du loggar in korrekt, bör du vara kan tooleave hello app och gå tillbaka utan att ange dina autentiseringsuppgifter igen.</span><span class="sxs-lookup"><span data-stu-id="377db-127">Once you sign in successfully, you should be able tooleave hello app and come back without entering your credentials again.</span></span> 

## <a name="next-steps"></a><span data-ttu-id="377db-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="377db-128">Next steps</span></span>

- <span data-ttu-id="377db-129">Läs mer om andra sätt tooimplement [enkel inloggning med Application Proxy](application-proxy-sso-overview.md)</span><span class="sxs-lookup"><span data-stu-id="377db-129">Read about other ways tooimplement [Single sign-on with Application Proxy](application-proxy-sso-overview.md)</span></span>
- <span data-ttu-id="377db-130">Lär dig mer om [säkerhetsaspekter för att komma åt appar med Azure AD Application Proxy](application-proxy-security-considerations.md)</span><span class="sxs-lookup"><span data-stu-id="377db-130">Learn about [Security considerations for accessing apps remotely with Azure AD Application Proxy](application-proxy-security-considerations.md)</span></span>