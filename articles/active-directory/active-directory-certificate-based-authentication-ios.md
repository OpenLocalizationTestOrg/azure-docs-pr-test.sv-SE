---
title: "Azure Active Directory certifikatbaserad autentisering på iOS | Microsoft Docs"
description: "Mer information om scenarierna som stöds och kraven för att konfigurera certifikatbaserad autentisering i lösningar med iOS-enheter"
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
ms.openlocfilehash: c781f3f054fad5c5092fed5058c932fd4e97cf35
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-ios"></a><span data-ttu-id="2a323-103">Azure Active Directory certifikatbaserad autentisering på iOS</span><span class="sxs-lookup"><span data-stu-id="2a323-103">Azure Active Directory certificate-based authentication on iOS</span></span>

<span data-ttu-id="2a323-104">Certifikatbaserad autentisering (CBA) gör att du ska kunna autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till:</span><span class="sxs-lookup"><span data-stu-id="2a323-104">Certificate-based authentication (CBA) enables you to be authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="2a323-105">Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="2a323-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="2a323-106">Exchange ActiveSync (EAS) klienter</span><span class="sxs-lookup"><span data-stu-id="2a323-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="2a323-107">Konfigurera den här funktionen eliminerar behovet av att ange en kombination av användarnamn och lösenord i vissa e-post och Microsoft Office-program på din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="2a323-107">Configuring this feature eliminates the need to enter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="2a323-108">Det här avsnittet innehåller kraven och scenarierna som stöds för att konfigurera CBA på en iOS(Android) enhet för användare av klienter i Office 365 Enterprise, företag, Education, som tillhör amerikanska myndigheter, Kina och Tyskland planer.</span><span class="sxs-lookup"><span data-stu-id="2a323-108">This topic provides you with the requirements and the supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>

<span data-ttu-id="2a323-109">Den här funktionen är tillgängliga i förhandsversionen i Office 365 US Government skydd och federala planer.</span><span class="sxs-lookup"><span data-stu-id="2a323-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>




## <a name="office-mobile-applications-support"></a><span data-ttu-id="2a323-110">Stöd för mobila Office-program</span><span class="sxs-lookup"><span data-stu-id="2a323-110">Office mobile applications support</span></span>

| <span data-ttu-id="2a323-111">Program</span><span class="sxs-lookup"><span data-stu-id="2a323-111">Apps</span></span> | <span data-ttu-id="2a323-112">Support</span><span class="sxs-lookup"><span data-stu-id="2a323-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="2a323-113">Azure Information Protection-app</span><span class="sxs-lookup"><span data-stu-id="2a323-113">Azure Information Protection app</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="2a323-115">Microsoft Teams</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="2a323-117">OneNote</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="2a323-119">OneDrive</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="2a323-121">Outlook</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-123">Power BI-appar</span><span class="sxs-lookup"><span data-stu-id="2a323-123">Power BI mobile apps</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-125">Skype för företag</span><span class="sxs-lookup"><span data-stu-id="2a323-125">Skype for Business</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-127">Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="2a323-127">Word / Excel / PowerPoint</span></span> |![Markera][1] |
| <span data-ttu-id="2a323-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="2a323-129">Yammer</span></span> |![Markera][1] |


## <a name="requirements"></a><span data-ttu-id="2a323-131">Krav</span><span class="sxs-lookup"><span data-stu-id="2a323-131">Requirements</span></span> 

<span data-ttu-id="2a323-132">Enhetens OS-version måste vara iOS 9 och senare</span><span class="sxs-lookup"><span data-stu-id="2a323-132">The device OS version must be iOS 9 and above</span></span> 

<span data-ttu-id="2a323-133">En federationsserver konfigureras.</span><span class="sxs-lookup"><span data-stu-id="2a323-133">A federation server must be configured.</span></span>  

<span data-ttu-id="2a323-134">Microsoft Authenticator krävs för Office-program på iOS.</span><span class="sxs-lookup"><span data-stu-id="2a323-134">Microsoft Authenticator is required for Office applications on iOS.</span></span>  

<span data-ttu-id="2a323-135">För Azure Active Directory att återkalla ett certifikat för har AD FS-token följande krav:</span><span class="sxs-lookup"><span data-stu-id="2a323-135">For Azure Active Directory to revoke a client certificate, the ADFS token must have the following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="2a323-136">(Serienumret för klientens certifikat)</span><span class="sxs-lookup"><span data-stu-id="2a323-136">(The serial number of the client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="2a323-137">(Sträng för utfärdaren av klientcertifikatet)</span><span class="sxs-lookup"><span data-stu-id="2a323-137">(The string for the issuer of the client certificate)</span></span> 

<span data-ttu-id="2a323-138">Dessa anspråk till uppdateringstoken som infogas i Azure Active Directory om de är tillgängliga i AD FS-token (eller andra SAML-token).</span><span class="sxs-lookup"><span data-stu-id="2a323-138">Azure Active Directory adds these claims to the refresh token if they are available in the ADFS token (or any other SAML token).</span></span> <span data-ttu-id="2a323-139">När uppdateringstoken som måste verifieras, används den här informationen för att kontrollera återkallningen.</span><span class="sxs-lookup"><span data-stu-id="2a323-139">When the refresh token needs to be validated, this information is used to check the revocation.</span></span> 

<span data-ttu-id="2a323-140">Som bästa praxis bör du uppdatera ADFS-felsidor med följande:</span><span class="sxs-lookup"><span data-stu-id="2a323-140">As a best practice, you should update the ADFS error pages with the following:</span></span>

* <span data-ttu-id="2a323-141">Krav för att installera Microsoft Authenticator på iOS</span><span class="sxs-lookup"><span data-stu-id="2a323-141">The requirement for installing the Microsoft Authenticator on iOS</span></span>
* <span data-ttu-id="2a323-142">Anvisningar om hur du hämtar ett användarcertifikat.</span><span class="sxs-lookup"><span data-stu-id="2a323-142">Instructions on how to get a user certificate.</span></span> 

<span data-ttu-id="2a323-143">Mer information finns i [anpassa AD FS Sign-in-sidor](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="2a323-143">For more details, see [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>

<span data-ttu-id="2a323-144">Vissa Office-appar (med modern autentisering är aktiverat) skicka '*uppmana = inloggningen*' till Azure AD i sin begäran.</span><span class="sxs-lookup"><span data-stu-id="2a323-144">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ to Azure AD in their request.</span></span> <span data-ttu-id="2a323-145">Som standard översätter det Azure AD i begäran till ADFS till '*wauth = usernamepassworduri*' (frågar AD FS för att göra U/P auth) och '*wfresh = 0*' (frågar AD FS att ignorera SSO tillstånd och gör en ny autentisering).</span><span class="sxs-lookup"><span data-stu-id="2a323-145">By default, Azure AD translates this in the request to ADFS to ‘*wauth=usernamepassworduri*’ (asks ADFS to do U/P auth) and ‘*wfresh=0*’ (asks ADFS to ignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="2a323-146">Om du vill aktivera certifikatbaserad autentisering för dessa appar måste du ändra standardbeteendet för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="2a323-146">If you want to enable certificate-based authentication for these apps, you need to modify the default Azure AD behavior.</span></span> <span data-ttu-id="2a323-147">Anger den '*PromptLoginBehavior*”i federerade domäninställningarna till'*inaktiverad*'.</span><span class="sxs-lookup"><span data-stu-id="2a323-147">Just set the ‘*PromptLoginBehavior*’ in your federated domain settings to ‘*Disabled*‘.</span></span> <span data-ttu-id="2a323-148">Du kan använda den [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) för att utföra den här uppgiften:</span><span class="sxs-lookup"><span data-stu-id="2a323-148">You can use the [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet to perform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`
  

## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="2a323-149">Stöd för Exchange ActiveSync-klienter</span><span class="sxs-lookup"><span data-stu-id="2a323-149">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="2a323-150">Intern iOS e-postklient stöds på iOS 9 eller senare.</span><span class="sxs-lookup"><span data-stu-id="2a323-150">On iOS 9 or later, the native iOS mail client is supported.</span></span> <span data-ttu-id="2a323-151">Kontakta din programutvecklaren för att avgöra om den här funktionen stöds för alla Exchange ActiveSync-program.</span><span class="sxs-lookup"><span data-stu-id="2a323-151">For all other Exchange ActiveSync applications, to determine if this feature is supported, contact your application developer.</span></span>  


## <a name="next-steps"></a><span data-ttu-id="2a323-152">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2a323-152">Next steps</span></span>

<span data-ttu-id="2a323-153">Om du vill konfigurera certifikatbaserad autentisering i din miljö, se [komma igång med certifikatbaserad autentisering på Android](active-directory-certificate-based-authentication-get-started.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="2a323-153">If you want to configure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>


<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-ios/ic195031.png
