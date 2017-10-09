---
title: "aaaManage enkel inloggning för Azure AD Application Proxy | Microsoft Docs"
description: "Lär dig mer om hello grunderna för enkel inloggning med Application Proxy"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/23/2017
ms.author: kgremban
ms.reviewer: harshja
ms.custom: it-pro
ms.openlocfilehash: a278751a5cb1bf98c970a4e5d2eb3edc3b784096
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-does-azure-ad-application-proxy-provide-single-sign-on"></a><span data-ttu-id="3c11d-103">Hur tillhandahåller Azure AD Application Proxy enkel inloggning?</span><span class="sxs-lookup"><span data-stu-id="3c11d-103">How does Azure AD Application Proxy provide single sign-on?</span></span>

<span data-ttu-id="3c11d-104">Enkel inloggning är en nyckelfaktor för Azure AD Application Proxy.</span><span class="sxs-lookup"><span data-stu-id="3c11d-104">Single sign-on is a key element of Azure AD Application Proxy.</span></span>  <span data-ttu-id="3c11d-105">Det ger hello bästa användarupplevelsen eftersom användarna bara har toosign i tooAzure Active Directory i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="3c11d-105">It provides hello best user experience because your users only have toosign in tooAzure Active Directory in hello cloud.</span></span> <span data-ttu-id="3c11d-106">När de autentiserar tooAzure Active Directory hanterar hello Application Proxy connector hello autentisering toohello lokalt program.</span><span class="sxs-lookup"><span data-stu-id="3c11d-106">Once they authenticate tooAzure Active Directory, hello Application Proxy connector handles hello authentication toohello on-premises application.</span></span> <span data-ttu-id="3c11d-107">hello backend-programmet kan inte se hello skillnaden mellan en fjärransluten användare logga in via Application Proxy och en vanlig användning på en domänansluten enhet.</span><span class="sxs-lookup"><span data-stu-id="3c11d-107">hello backend application can't tell hello difference between a remote user signing in through Application Proxy and a regular use on a domain-joined device.</span></span> 

<span data-ttu-id="3c11d-108">toouse Azure Active Directory för enkel inloggning tooyour program behöver du tooselect **Azure Active Directory** som hello före autentiseringsmetod.</span><span class="sxs-lookup"><span data-stu-id="3c11d-108">toouse Azure Active Directory for single sign-on tooyour applications, you need tooselect **Azure Active Directory** as hello pre-authentication method.</span></span> <span data-ttu-id="3c11d-109">Om du väljer **Passthrough** användarna inte autentisera tooAzure Active Directory alls, men är riktat raka toohello program.</span><span class="sxs-lookup"><span data-stu-id="3c11d-109">If you select **Passthrough** then your users don't authenticate tooAzure Active Directory at all, but are directed straight toohello application.</span></span> <span data-ttu-id="3c11d-110">Du kan konfigurera den här inställningen när du först publicera ett program eller navigera tooyour programmet hello Azure-portalen och redigera hello Application Proxy-inställningar.</span><span class="sxs-lookup"><span data-stu-id="3c11d-110">You can configure this setting when you first publish an application, or navigate tooyour application in hello Azure portal and edit hello Application Proxy settings.</span></span> 

<span data-ttu-id="3c11d-111">toosee enkel inloggning alternativen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="3c11d-111">toosee your single sign-on options, follow these steps:</span></span>

1. <span data-ttu-id="3c11d-112">Logga in toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="3c11d-112">Sign in toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="3c11d-113">Navigera för**Azure Active Directory** > **företagsprogram** > **alla program**.</span><span class="sxs-lookup"><span data-stu-id="3c11d-113">Navigate too**Azure Active Directory** > **Enterprise applications** > **All applications**.</span></span>
3. <span data-ttu-id="3c11d-114">Välj hello app vars enkel inloggning alternativen du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="3c11d-114">Select hello app whose single sign-on options you want toomanage.</span></span>
4. <span data-ttu-id="3c11d-115">Välj **enkel inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3c11d-115">Select **Single sign-on**.</span></span>

   ![Listrutan för enkel inloggning](./media/application-proxy-sso-overview/single-sign-on-mode.png)

<span data-ttu-id="3c11d-117">hello listrutan visas fem alternativ för enkel inloggning tooyour program:</span><span class="sxs-lookup"><span data-stu-id="3c11d-117">hello dropdown menu shows five options for single sign-on tooyour application:</span></span>

* <span data-ttu-id="3c11d-118">Azure AD enkel inloggning inaktiverad</span><span class="sxs-lookup"><span data-stu-id="3c11d-118">Azure AD single sign-on disabled</span></span>
* <span data-ttu-id="3c11d-119">Lösenordsbaserade inloggning</span><span class="sxs-lookup"><span data-stu-id="3c11d-119">Password-based sign-on</span></span>
* <span data-ttu-id="3c11d-120">Länkade inloggning</span><span class="sxs-lookup"><span data-stu-id="3c11d-120">Linked sign-on</span></span>
* <span data-ttu-id="3c11d-121">Integrerad Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="3c11d-121">Integrated Windows Authentication</span></span>
* <span data-ttu-id="3c11d-122">Rubrik-baserade inloggning</span><span class="sxs-lookup"><span data-stu-id="3c11d-122">Header-based sign-on</span></span>

## <a name="azure-ad-single-sign-on-disabled"></a><span data-ttu-id="3c11d-123">Azure AD enkel inloggning inaktiverad</span><span class="sxs-lookup"><span data-stu-id="3c11d-123">Azure AD single sign-on disabled</span></span>

<span data-ttu-id="3c11d-124">Om du inte vill toouse Azure Active Directory-integrering för enkel inloggning tooyour program, Välj **Azure AD enkel inloggning inaktiverat**.</span><span class="sxs-lookup"><span data-stu-id="3c11d-124">If you don't want toouse Azure Active Directory integration for single sign-on tooyour application, choose **Azure AD single sign-on disabled**.</span></span> <span data-ttu-id="3c11d-125">Med det här alternativet kan dina användare kan autentiseras två gånger.</span><span class="sxs-lookup"><span data-stu-id="3c11d-125">With this option selected, your users may authenticate twice.</span></span> <span data-ttu-id="3c11d-126">Först de autentisera tooAzure Active Directory och logga sedan in själva toohello programmet.</span><span class="sxs-lookup"><span data-stu-id="3c11d-126">First, they authenticate tooAzure Active Directory and then sign in toohello application itself.</span></span> 

<span data-ttu-id="3c11d-127">Det här alternativet är bra om lokala program kräver inte användare tooauthenticate, men du vill ha tooadd Azure Active Directory som en säkerhetsnivå för fjärråtkomst.</span><span class="sxs-lookup"><span data-stu-id="3c11d-127">This option is a good choice if your on-premises application doesn't require users tooauthenticate, but you want tooadd Azure Active Directory as a layer of security for remote access.</span></span> 

## <a name="password-based-sign-on"></a><span data-ttu-id="3c11d-128">Lösenordsbaserade inloggning</span><span class="sxs-lookup"><span data-stu-id="3c11d-128">Password-based sign-on</span></span>

<span data-ttu-id="3c11d-129">Om du vill toouse Azure Active Directory som ett valv lösenord för lokala program väljer **lösenordsbaserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3c11d-129">If you want toouse Azure Active Directory as a password vault for your on-premises applications, choose **Password-based sign-on**.</span></span> <span data-ttu-id="3c11d-130">Det här alternativet är bra om programmet autentiserar med användarnamn/lösenord kombinationsruta i stället för åtkomst-token eller huvuden.</span><span class="sxs-lookup"><span data-stu-id="3c11d-130">This option is a good choice if your application authenticates with a username/password combo instead of access tokens or headers.</span></span> <span data-ttu-id="3c11d-131">Med lösenordsbaserade inloggning måste användarna toosign i toohello programmet hello första gången de komma åt den.</span><span class="sxs-lookup"><span data-stu-id="3c11d-131">With password-based sign-on, your users need toosign in toohello application hello first time they access it.</span></span> <span data-ttu-id="3c11d-132">Efter det tillhandahåller Azure Active Directory hello användarnamn och lösenord för hello användares räkning.</span><span class="sxs-lookup"><span data-stu-id="3c11d-132">After that, Azure Active Directory supplies hello username and password on behalf of hello user.</span></span> 

<span data-ttu-id="3c11d-133">Information om hur du konfigurerar lösenordsbaserade inloggning finns i [lösenord vaulting för enkel inloggning med Application Proxy](application-proxy-sso-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="3c11d-133">For information about setting up password-based sign-on, see [Password vaulting for single sign-on with Application Proxy](application-proxy-sso-azure-portal.md).</span></span>

## <a name="linked-sign-on"></a><span data-ttu-id="3c11d-134">Länkade inloggning</span><span class="sxs-lookup"><span data-stu-id="3c11d-134">Linked sign-on</span></span>

<span data-ttu-id="3c11d-135">Om du redan har en enkel inloggning lösning har ställts in för dina lokala identiteter väljer **länkade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3c11d-135">If you already have a single sign-on solution set up for your on-premises identities, choose **Linked sign-on**.</span></span> <span data-ttu-id="3c11d-136">Det här alternativet kan Azure Active Directory tooleverage befintliga SSO lösningar, men fortfarande ger användarna fjärråtkomst toohello program.</span><span class="sxs-lookup"><span data-stu-id="3c11d-136">This option enables Azure Active Directory tooleverage existing SSO solutions, but still gives your users remote access toohello application.</span></span> 

<span data-ttu-id="3c11d-137">Information om länkade inloggning (tidigare kallat befintliga enkel inloggning) finns [vad är programåtkomst och enkel inloggning med Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span><span class="sxs-lookup"><span data-stu-id="3c11d-137">For information about linked sign-on (formally known as existing single sign-on), see [What is application access and single sign-on with Azure Active Directory?](active-directory-appssoaccess-whatis.md#how-does-single-sign-on-with-azure-active-directory-work).</span></span>

## <a name="integrated-windows-authentication"></a><span data-ttu-id="3c11d-138">Integrerad Windows-autentisering</span><span class="sxs-lookup"><span data-stu-id="3c11d-138">Integrated Windows Authentication</span></span>

<span data-ttu-id="3c11d-139">Välj om lokala program använder integrerad Windows-Authentication(IWA) eller om du vill toouse Kerberos-begränsad delegering (KCD) för enkel inloggning **integrerad Windows-autentisering**.</span><span class="sxs-lookup"><span data-stu-id="3c11d-139">If your on-premises applications use Integrated Windows Authentication(IWA) or if you want toouse Kerberos Constrained Delegation (KCD) for single sign-on, choose **Integrated Windows Authentication**.</span></span> <span data-ttu-id="3c11d-140">Användarna behöver bara tooauthenticate tooAzure Active Directory med det här alternativet och sedan hello Application Proxy connector personifierar hello användaren tooget Kerberos-token och logga in toohello program.</span><span class="sxs-lookup"><span data-stu-id="3c11d-140">With this option, your users only need tooauthenticate tooAzure Active Directory, and then hello Application Proxy connector impersonates hello user tooget a Kerberos token and sign in toohello application.</span></span> 

<span data-ttu-id="3c11d-141">Information om hur du konfigurerar integrerad Windows-autentisering finns i [Kerberos-begränsad delegering för enkel inloggning med Application Proxy](active-directory-application-proxy-sso-using-kcd.md).</span><span class="sxs-lookup"><span data-stu-id="3c11d-141">For information about setting up Integrated Windows Authentication, see [Kerberos Constrained Delegation for single sign-on with Application Proxy](active-directory-application-proxy-sso-using-kcd.md).</span></span>

## <a name="header-based-sign-on"></a><span data-ttu-id="3c11d-142">Rubrik-baserade inloggning</span><span class="sxs-lookup"><span data-stu-id="3c11d-142">Header-based sign-on</span></span> 

<span data-ttu-id="3c11d-143">Om dina program använder huvuden för autentisering, Välj **huvud-baserade inloggning**.</span><span class="sxs-lookup"><span data-stu-id="3c11d-143">If your applications use headers for authentication, choose **Header-based sign-on**.</span></span> <span data-ttu-id="3c11d-144">Med det här alternativet behöver användarna bara tooauthentication hello Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="3c11d-144">With this option, your users only need tooauthentication hello Azure Active Directory.</span></span> <span data-ttu-id="3c11d-145">Microsoft-partner med en autentiseringstjänst från tredje part kallas PingAccess som översättas hello Azure Active Directory-åtkomsttoken till en huvudformat för hello program.</span><span class="sxs-lookup"><span data-stu-id="3c11d-145">Microsoft partners with a third-party authentication service called PingAccess, which translated hello Azure Active Directory access token into a header format for hello application.</span></span> 

<span data-ttu-id="3c11d-146">Information om hur du konfigurerar huvud-baserad autentisering finns [huvud-baserad autentisering för enkel inloggning med Application Proxy](application-proxy-ping-access.md).</span><span class="sxs-lookup"><span data-stu-id="3c11d-146">For information about setting up header-based authentication, see [Header-based authentication for single sign-on with Application Proxy](application-proxy-ping-access.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c11d-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c11d-147">Next steps</span></span>

- [<span data-ttu-id="3c11d-148">Lösenord vaulting för enkel inloggning med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="3c11d-148">Password vaulting for single sign-on with Application Proxy</span></span>](application-proxy-sso-azure-portal.md)
- [<span data-ttu-id="3c11d-149">Kerberos-begränsad delegering för enkel inloggning med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="3c11d-149">Kerberos Constrained Delegation for single sign-on with Application Proxy</span></span>](active-directory-application-proxy-sso-using-kcd.md)
- [<span data-ttu-id="3c11d-150">Rubrik-baserad autentisering för enkel inloggning med Application Proxy</span><span class="sxs-lookup"><span data-stu-id="3c11d-150">Header-based authentication for single sign-on with Application Proxy</span></span>](application-proxy-ping-access.md) 
