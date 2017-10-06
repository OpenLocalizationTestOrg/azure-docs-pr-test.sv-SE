---
title: "aaaAzure Active Directory certifikatbaserad autentisering på Android | Microsoft Docs"
description: "Lär dig mer om hello stöds scenarier och hello kraven för att konfigurera certifikatbaserad autentisering i lösningar med Android-enheter"
services: active-directory
author: MarkusVi
documentationcenter: na
manager: femila
ms.assetid: c6ad7640-8172-4541-9255-770f39ecce0e
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/28/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: 148275fa3da610530c278fcd57e02e907f735d9a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a><span data-ttu-id="96443-103">Azure Active Directory certifikatbaserad autentisering på Android</span><span class="sxs-lookup"><span data-stu-id="96443-103">Azure Active Directory certificate-based authentication on Android</span></span>


<span data-ttu-id="96443-104">Certifikatbaserad autentisering (CBA) kan du toobe autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till:</span><span class="sxs-lookup"><span data-stu-id="96443-104">Certificate-based authentication (CBA) enables you toobe authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="96443-105">Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="96443-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="96443-106">Exchange ActiveSync (EAS) klienter</span><span class="sxs-lookup"><span data-stu-id="96443-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="96443-107">Konfigurera den här funktionen eliminerar hello måste tooenter användarnamnet och lösenordet till vissa e-post och Microsoft Office-program på din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="96443-107">Configuring this feature eliminates hello need tooenter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="96443-108">Det här avsnittet ger dig hello krav och hello stöds scenarier för att konfigurera CBA på en iOS(Android) enhet för användare av klienter i Office 365 Enterprise, företag, Education, som tillhör amerikanska myndigheter, Kina och Tyskland planer.</span><span class="sxs-lookup"><span data-stu-id="96443-108">This topic provides you with hello requirements and hello supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>



<span data-ttu-id="96443-109">Den här funktionen är tillgängliga i förhandsversionen i Office 365 US Government skydd och federala planer.</span><span class="sxs-lookup"><span data-stu-id="96443-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>


## <a name="office-mobile-applications-support"></a><span data-ttu-id="96443-110">Stöd för mobila Office-program</span><span class="sxs-lookup"><span data-stu-id="96443-110">Office mobile applications support</span></span>
| <span data-ttu-id="96443-111">Program</span><span class="sxs-lookup"><span data-stu-id="96443-111">Apps</span></span> | <span data-ttu-id="96443-112">Support</span><span class="sxs-lookup"><span data-stu-id="96443-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="96443-113">Azure Information Protection-app</span><span class="sxs-lookup"><span data-stu-id="96443-113">Azure Information Protection app</span></span> |![Markera][1] |
| <span data-ttu-id="96443-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="96443-115">Microsoft Teams</span></span> |![Markera][1] |
| <span data-ttu-id="96443-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="96443-117">OneNote</span></span> |![Markera][1] |
| <span data-ttu-id="96443-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="96443-119">OneDrive</span></span> |![Markera][1] |
| <span data-ttu-id="96443-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="96443-121">Outlook</span></span> |![Markera][1] |
| <span data-ttu-id="96443-123">Power BI-appar</span><span class="sxs-lookup"><span data-stu-id="96443-123">Power BI mobile apps</span></span> |![Markera][1] |
| <span data-ttu-id="96443-125">Skype för företag</span><span class="sxs-lookup"><span data-stu-id="96443-125">Skype for Business</span></span> |![Markera][1] |
| <span data-ttu-id="96443-127">Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="96443-127">Word / Excel / PowerPoint</span></span> |![Markera][1] |
| <span data-ttu-id="96443-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="96443-129">Yammer</span></span> |![Markera][1] |


### <a name="implementation-requirements"></a><span data-ttu-id="96443-131">Implementeringskrav för</span><span class="sxs-lookup"><span data-stu-id="96443-131">Implementation requirements</span></span>

<span data-ttu-id="96443-132">hello enhetens OS-version måste vara Android 5.0 (Lollipop) och senare.</span><span class="sxs-lookup"><span data-stu-id="96443-132">hello device OS version must be Android 5.0 (Lollipop) and above.</span></span> 

<span data-ttu-id="96443-133">En federationsserver konfigureras.</span><span class="sxs-lookup"><span data-stu-id="96443-133">A federation server must be configured.</span></span>  

<span data-ttu-id="96443-134">För Azure Active Directory toorevoke ett klientcertifikat har hello AD FS-token hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="96443-134">For Azure Active Directory toorevoke a client certificate, hello ADFS token must have hello following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="96443-135">(hello serienumret för hello klientcertifikat)</span><span class="sxs-lookup"><span data-stu-id="96443-135">(hello serial number of hello client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="96443-136">(hello sträng för hello utfärdaren av klientcertifikatet hello)</span><span class="sxs-lookup"><span data-stu-id="96443-136">(hello string for hello issuer of hello client certificate)</span></span> 

<span data-ttu-id="96443-137">Azure Active Directory lägger till dessa anspråk toohello uppdateringstoken om de är tillgängliga i hello AD FS-token (eller andra SAML-token).</span><span class="sxs-lookup"><span data-stu-id="96443-137">Azure Active Directory adds these claims toohello refresh token if they are available in hello ADFS token (or any other SAML token).</span></span> <span data-ttu-id="96443-138">När hello uppdateringstoken måste toobe verifieras, är den här informationen används toocheck hello återkallade certifikat.</span><span class="sxs-lookup"><span data-stu-id="96443-138">When hello refresh token needs toobe validated, this information is used toocheck hello revocation.</span></span> 

<span data-ttu-id="96443-139">Som bästa praxis bör du uppdatera hello ADFS-felsidor med instruktioner om hur tooget ett användarcertifikat.</span><span class="sxs-lookup"><span data-stu-id="96443-139">As a best practice, you should update hello ADFS error pages with instructions on how tooget a user certificate.</span></span>  
<span data-ttu-id="96443-140">Mer information finns i [anpassa hello AD FS Sign in sidor](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="96443-140">For more details, see [Customizing hello AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>  

<span data-ttu-id="96443-141">Vissa Office-appar (med modern autentisering är aktiverat) skicka '*uppmana = inloggningen*' tooAzure AD i sin begäran.</span><span class="sxs-lookup"><span data-stu-id="96443-141">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ tooAzure AD in their request.</span></span> <span data-ttu-id="96443-142">Standard Azure AD omvandlas i hello begäran tooADFS för '*wauth = usernamepassworduri*' (frågar ADFS toodo U/P auth) och '*wfresh = 0*' (frågar ADFS tooignore SSO tillstånd och gör en ny autentisering) .</span><span class="sxs-lookup"><span data-stu-id="96443-142">By default, Azure AD translates this in hello request tooADFS too‘*wauth=usernamepassworduri*’ (asks ADFS toodo U/P auth) and ‘*wfresh=0*’ (asks ADFS tooignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="96443-143">Om du vill tooenable certifikatbaserad autentisering för dessa appar, måste toomodify hello Azure AD standardbeteendet.</span><span class="sxs-lookup"><span data-stu-id="96443-143">If you want tooenable certificate-based authentication for these apps, you need toomodify hello default Azure AD behavior.</span></span> <span data-ttu-id="96443-144">Bara ange hello '*PromptLoginBehavior*' i inställningarna för federerade domänen för '*inaktiverad*'.</span><span class="sxs-lookup"><span data-stu-id="96443-144">Just set hello ‘*PromptLoginBehavior*’ in your federated domain settings too‘*Disabled*‘.</span></span> <span data-ttu-id="96443-145">Du kan använda hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet tooperform aktiviteten:</span><span class="sxs-lookup"><span data-stu-id="96443-145">You can use hello [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet tooperform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`



## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="96443-146">Stöd för Exchange ActiveSync-klienter</span><span class="sxs-lookup"><span data-stu-id="96443-146">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="96443-147">Vissa Exchange ActiveSync-program på Android 5.0 (Lollipop) eller senare stöds.</span><span class="sxs-lookup"><span data-stu-id="96443-147">Certain Exchange ActiveSync applications on Android 5.0 (Lollipop) or later are supported.</span></span> <span data-ttu-id="96443-148">toodetermine om e-postprogrammet stöder den här funktionen kan du kontakta din programutvecklaren.</span><span class="sxs-lookup"><span data-stu-id="96443-148">toodetermine if your email application does support this feature, please contact your application developer.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="96443-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="96443-149">Next steps</span></span>

<span data-ttu-id="96443-150">Om du vill tooconfigure certifikatbaserad autentisering i din miljö, se [komma igång med certifikatbaserad autentisering på Android](active-directory-certificate-based-authentication-get-started.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="96443-150">If you want tooconfigure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
