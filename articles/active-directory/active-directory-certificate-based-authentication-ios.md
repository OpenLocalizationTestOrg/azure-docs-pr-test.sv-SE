---
title: "aaaAzure Active Directory certifikatbaserad autentisering på iOS | Microsoft Docs"
description: "Lär dig mer om hello stöds scenarier och hello kraven för att konfigurera certifikatbaserad autentisering i lösningar med iOS-enheter"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: 26a6fc54-0153-44fb-b970-9b432c99e9f9
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/24/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 4486ff5239c2897b3bc187053f31d74807430301
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a><span data-ttu-id="7061b-103">Azure Active Directory certifikatbaserad autentisering på iOS</span><span class="sxs-lookup"><span data-stu-id="7061b-103">Azure Active Directory certificate-based authentication on iOS</span></span>

<span data-ttu-id="7061b-104">Certifikatbaserad autentisering (CBA) kan du toobe autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till:</span><span class="sxs-lookup"><span data-stu-id="7061b-104">Certificate-based authentication (CBA) enables you toobe authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="7061b-105">Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="7061b-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="7061b-106">Exchange ActiveSync (EAS) klienter</span><span class="sxs-lookup"><span data-stu-id="7061b-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="7061b-107">Konfigurera den här funktionen eliminerar hello måste tooenter användarnamnet och lösenordet till vissa e-post och Microsoft Office-program på din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="7061b-107">Configuring this feature eliminates hello need tooenter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="7061b-108">Det här avsnittet ger dig hello krav och hello stöds scenarier för att konfigurera CBA på en iOS(Android) enhet för användare av klienter i Office 365 Enterprise, företag, Education, som tillhör amerikanska myndigheter, Kina och Tyskland planer.</span><span class="sxs-lookup"><span data-stu-id="7061b-108">This topic provides you with hello requirements and hello supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>

<span data-ttu-id="7061b-109">Den här funktionen är tillgängliga i förhandsversionen i Office 365 US Government skydd och federala planer.</span><span class="sxs-lookup"><span data-stu-id="7061b-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>




## <a name="office-mobile-applications-support"></a><span data-ttu-id="7061b-110">Stöd för mobila Office-program</span><span class="sxs-lookup"><span data-stu-id="7061b-110">Office mobile applications support</span></span>

| <span data-ttu-id="7061b-111">Program</span><span class="sxs-lookup"><span data-stu-id="7061b-111">Apps</span></span> | <span data-ttu-id="7061b-112">Support</span><span class="sxs-lookup"><span data-stu-id="7061b-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="7061b-113">Azure Information Protection-app</span><span class="sxs-lookup"><span data-stu-id="7061b-113">Azure Information Protection app</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="7061b-115">Microsoft Teams</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="7061b-117">OneNote</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="7061b-119">OneDrive</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="7061b-121">Outlook</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-123">Power BI-appar</span><span class="sxs-lookup"><span data-stu-id="7061b-123">Power BI mobile apps</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-125">Skype för företag</span><span class="sxs-lookup"><span data-stu-id="7061b-125">Skype for Business</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-127">Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="7061b-127">Word / Excel / PowerPoint</span></span> |![Markera][1] |
| <span data-ttu-id="7061b-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="7061b-129">Yammer</span></span> |![Markera][1] |


## <a name="requirements"></a><span data-ttu-id="7061b-131">Krav</span><span class="sxs-lookup"><span data-stu-id="7061b-131">Requirements</span></span> 

<span data-ttu-id="7061b-132">hello enhetens OS-version måste vara iOS 9 och senare</span><span class="sxs-lookup"><span data-stu-id="7061b-132">hello device OS version must be iOS 9 and above</span></span> 

<span data-ttu-id="7061b-133">En federationsserver konfigureras.</span><span class="sxs-lookup"><span data-stu-id="7061b-133">A federation server must be configured.</span></span>  

<span data-ttu-id="7061b-134">Microsoft Authenticator krävs för Office-program på iOS.</span><span class="sxs-lookup"><span data-stu-id="7061b-134">Microsoft Authenticator is required for Office applications on iOS.</span></span>  

<span data-ttu-id="7061b-135">För Azure Active Directory toorevoke ett klientcertifikat har hello AD FS-token hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="7061b-135">For Azure Active Directory toorevoke a client certificate, hello ADFS token must have hello following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="7061b-136">(hello serienumret för hello klientcertifikat)</span><span class="sxs-lookup"><span data-stu-id="7061b-136">(hello serial number of hello client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="7061b-137">(hello sträng för hello utfärdaren av klientcertifikatet hello)</span><span class="sxs-lookup"><span data-stu-id="7061b-137">(hello string for hello issuer of hello client certificate)</span></span> 

<span data-ttu-id="7061b-138">Azure Active Directory lägger till dessa anspråk toohello uppdateringstoken om de är tillgängliga i hello AD FS-token (eller andra SAML-token).</span><span class="sxs-lookup"><span data-stu-id="7061b-138">Azure Active Directory adds these claims toohello refresh token if they are available in hello ADFS token (or any other SAML token).</span></span> <span data-ttu-id="7061b-139">När hello uppdateringstoken måste toobe verifieras, är den här informationen används toocheck hello återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="7061b-139">When hello refresh token needs toobe validated, this information is used toocheck hello revocation.</span></span> 

<span data-ttu-id="7061b-140">Som bästa praxis bör du uppdatera hello ADFS-felsidor med hello följande:</span><span class="sxs-lookup"><span data-stu-id="7061b-140">As a best practice, you should update hello ADFS error pages with hello following:</span></span>

* <span data-ttu-id="7061b-141">hello krav för att installera hello Microsoft Authenticator på iOS</span><span class="sxs-lookup"><span data-stu-id="7061b-141">hello requirement for installing hello Microsoft Authenticator on iOS</span></span>
* <span data-ttu-id="7061b-142">Instruktioner om hur tooget ett användarcertifikat.</span><span class="sxs-lookup"><span data-stu-id="7061b-142">Instructions on how tooget a user certificate.</span></span> 

<span data-ttu-id="7061b-143">Mer information finns i [anpassa hello AD FS Sign in sidor](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="7061b-143">For more details, see [Customizing hello AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>

<span data-ttu-id="7061b-144">Vissa Office-appar (med modern autentisering är aktiverat) skicka '*uppmana = inloggningen*' tooAzure AD i sin begäran.</span><span class="sxs-lookup"><span data-stu-id="7061b-144">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ tooAzure AD in their request.</span></span> <span data-ttu-id="7061b-145">Standard Azure AD omvandlas i hello begäran tooADFS för '*wauth = usernamepassworduri*' (frågar ADFS toodo U/P auth) och '*wfresh = 0*' (frågar ADFS tooignore SSO tillstånd och gör en ny autentisering) .</span><span class="sxs-lookup"><span data-stu-id="7061b-145">By default, Azure AD translates this in hello request tooADFS too‘*wauth=usernamepassworduri*’ (asks ADFS toodo U/P auth) and ‘*wfresh=0*’ (asks ADFS tooignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="7061b-146">Om du vill tooenable certifikatbaserad autentisering för dessa appar, måste toomodify hello Azure AD standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="7061b-146">If you want tooenable certificate-based authentication for these apps, you need toomodify hello default Azure AD behavior.</span></span> <span data-ttu-id="7061b-147">Bara ange hello '*PromptLoginBehavior*' i inställningarna för federerade domänen för '*inaktiverad*'.</span><span class="sxs-lookup"><span data-stu-id="7061b-147">Just set hello ‘*PromptLoginBehavior*’ in your federated domain settings too‘*Disabled*‘.</span></span> <span data-ttu-id="7061b-148">Du kan använda hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet tooperform aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="7061b-148">You can use hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet tooperform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="7061b-149">Stöd för Exchange ActiveSync-klienter</span><span class="sxs-lookup"><span data-stu-id="7061b-149">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="7061b-150">Hello interna iOS e-postklient stöds på iOS 9 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7061b-150">On iOS 9 or later, hello native iOS mail client is supported.</span></span> <span data-ttu-id="7061b-151">För alla Exchange ActiveSync-program toodetermine om den här funktionen stöds kontaktar du din programutvecklaren.</span><span class="sxs-lookup"><span data-stu-id="7061b-151">For all other Exchange ActiveSync applications, toodetermine if this feature is supported, contact your application developer.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="7061b-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7061b-152">Next steps</span></span>

<span data-ttu-id="7061b-153">Om du vill tooconfigure certifikatbaserad autentisering i din miljö, se [komma igång med certifikatbaserad autentisering på Android](active-directory-certificate-based-authentication-get-started.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="7061b-153">If you want tooconfigure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
