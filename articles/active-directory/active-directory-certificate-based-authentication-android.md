---
title: "Azure Active Directory certifikatbaserad autentisering på Android | Microsoft Docs"
description: "Mer information om scenarierna som stöds och kraven för att konfigurera certifikatbaserad autentisering i lösningar med Android-enheter"
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
ms.openlocfilehash: 8005bfe821fea25539c84efdccf6c49bd5f1f8ee
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="azure-active-directory-certificate-based-authentication-on-android"></a><span data-ttu-id="eac46-103">Azure Active Directory certifikatbaserad autentisering på Android</span><span class="sxs-lookup"><span data-stu-id="eac46-103">Azure Active Directory certificate-based authentication on Android</span></span>


<span data-ttu-id="eac46-104">Certifikatbaserad autentisering (CBA) gör att du ska kunna autentiseras av Azure Active Directory med ett klientcertifikat på en Windows-, Android eller iOS-enhet när du ansluter din Exchange-onlinekonto till:</span><span class="sxs-lookup"><span data-stu-id="eac46-104">Certificate-based authentication (CBA) enables you to be authenticated by Azure Active Directory with a client certificate on a Windows, Android or iOS device when connecting your Exchange online account to:</span></span> 

* <span data-ttu-id="eac46-105">Mobila Office-program, till exempel Microsoft Outlook och Microsoft Word</span><span class="sxs-lookup"><span data-stu-id="eac46-105">Office mobile applications such as Microsoft Outlook and Microsoft Word</span></span>   
* <span data-ttu-id="eac46-106">Exchange ActiveSync (EAS) klienter</span><span class="sxs-lookup"><span data-stu-id="eac46-106">Exchange ActiveSync (EAS) clients</span></span> 

<span data-ttu-id="eac46-107">Konfigurera den här funktionen eliminerar behovet av att ange en kombination av användarnamn och lösenord i vissa e-post och Microsoft Office-program på din mobila enhet.</span><span class="sxs-lookup"><span data-stu-id="eac46-107">Configuring this feature eliminates the need to enter a username and password combination into certain mail and Microsoft Office applications on your mobile device.</span></span> 

<span data-ttu-id="eac46-108">Det här avsnittet innehåller kraven och scenarierna som stöds för att konfigurera CBA på en iOS(Android) enhet för användare av klienter i Office 365 Enterprise, företag, Education, som tillhör amerikanska myndigheter, Kina och Tyskland planer.</span><span class="sxs-lookup"><span data-stu-id="eac46-108">This topic provides you with the requirements and the supported scenarios for configuring CBA on an iOS(Android) device for users of tenants in Office 365 Enterprise, Business, Education, US Government, China, and Germany plans.</span></span>



<span data-ttu-id="eac46-109">Den här funktionen är tillgängliga i förhandsversionen i Office 365 US Government skydd och federala planer.</span><span class="sxs-lookup"><span data-stu-id="eac46-109">This feature is available in preview in Office 365 US Government Defense and Federal plans.</span></span>


## <a name="office-mobile-applications-support"></a><span data-ttu-id="eac46-110">Stöd för mobila Office-program</span><span class="sxs-lookup"><span data-stu-id="eac46-110">Office mobile applications support</span></span>
| <span data-ttu-id="eac46-111">Program</span><span class="sxs-lookup"><span data-stu-id="eac46-111">Apps</span></span> | <span data-ttu-id="eac46-112">Support</span><span class="sxs-lookup"><span data-stu-id="eac46-112">Support</span></span> |
| --- | --- |
| <span data-ttu-id="eac46-113">Azure Information Protection-app</span><span class="sxs-lookup"><span data-stu-id="eac46-113">Azure Information Protection app</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-115">Microsoft Teams</span><span class="sxs-lookup"><span data-stu-id="eac46-115">Microsoft Teams</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-117">OneNote</span><span class="sxs-lookup"><span data-stu-id="eac46-117">OneNote</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-119">OneDrive</span><span class="sxs-lookup"><span data-stu-id="eac46-119">OneDrive</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-121">Outlook</span><span class="sxs-lookup"><span data-stu-id="eac46-121">Outlook</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-123">Power BI-appar</span><span class="sxs-lookup"><span data-stu-id="eac46-123">Power BI mobile apps</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-125">Skype för företag</span><span class="sxs-lookup"><span data-stu-id="eac46-125">Skype for Business</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-127">Word / Excel / PowerPoint</span><span class="sxs-lookup"><span data-stu-id="eac46-127">Word / Excel / PowerPoint</span></span> |![Markera][1] |
| <span data-ttu-id="eac46-129">Yammer</span><span class="sxs-lookup"><span data-stu-id="eac46-129">Yammer</span></span> |![Markera][1] |


### <a name="implementation-requirements"></a><span data-ttu-id="eac46-131">Implementeringskrav för</span><span class="sxs-lookup"><span data-stu-id="eac46-131">Implementation requirements</span></span>

<span data-ttu-id="eac46-132">Enhetens OS-version måste vara Android 5.0 (Lollipop) och senare.</span><span class="sxs-lookup"><span data-stu-id="eac46-132">The device OS version must be Android 5.0 (Lollipop) and above.</span></span> 

<span data-ttu-id="eac46-133">En federationsserver konfigureras.</span><span class="sxs-lookup"><span data-stu-id="eac46-133">A federation server must be configured.</span></span>  

<span data-ttu-id="eac46-134">För Azure Active Directory att återkalla ett certifikat för har AD FS-token följande krav:</span><span class="sxs-lookup"><span data-stu-id="eac46-134">For Azure Active Directory to revoke a client certificate, the ADFS token must have the following claims:</span></span>  

* `http://schemas.microsoft.com/ws/2008/06/identity/claims/<serialnumber>`  
  <span data-ttu-id="eac46-135">(Serienumret för klientens certifikat)</span><span class="sxs-lookup"><span data-stu-id="eac46-135">(The serial number of the client certificate)</span></span> 
* `http://schemas.microsoft.com/2012/12/certificatecontext/field/<issuer>`  
  <span data-ttu-id="eac46-136">(Sträng för utfärdaren av klientcertifikatet)</span><span class="sxs-lookup"><span data-stu-id="eac46-136">(The string for the issuer of the client certificate)</span></span> 

<span data-ttu-id="eac46-137">Dessa anspråk till uppdateringstoken som infogas i Azure Active Directory om de är tillgängliga i AD FS-token (eller andra SAML-token).</span><span class="sxs-lookup"><span data-stu-id="eac46-137">Azure Active Directory adds these claims to the refresh token if they are available in the ADFS token (or any other SAML token).</span></span> <span data-ttu-id="eac46-138">När uppdateringstoken som måste verifieras, används den här informationen för att kontrollera återkallningen.</span><span class="sxs-lookup"><span data-stu-id="eac46-138">When the refresh token needs to be validated, this information is used to check the revocation.</span></span> 

<span data-ttu-id="eac46-139">Som bästa praxis bör du uppdatera felsidor ADFS med instruktioner om hur du hämtar ett användarcertifikat.</span><span class="sxs-lookup"><span data-stu-id="eac46-139">As a best practice, you should update the ADFS error pages with instructions on how to get a user certificate.</span></span>  
<span data-ttu-id="eac46-140">Mer information finns i [anpassa AD FS Sign-in-sidor](https://technet.microsoft.com/library/dn280950.aspx).</span><span class="sxs-lookup"><span data-stu-id="eac46-140">For more details, see [Customizing the AD FS Sign-in Pages](https://technet.microsoft.com/library/dn280950.aspx).</span></span>  

<span data-ttu-id="eac46-141">Vissa Office-appar (med modern autentisering är aktiverat) skicka '*uppmana = inloggningen*' till Azure AD i sin begäran.</span><span class="sxs-lookup"><span data-stu-id="eac46-141">Some Office apps (with modern authentication enabled) send ‘*prompt=login*’ to Azure AD in their request.</span></span> <span data-ttu-id="eac46-142">Som standard översätter det Azure AD i begäran till ADFS till '*wauth = usernamepassworduri*' (frågar AD FS för att göra U/P auth) och '*wfresh = 0*' (frågar AD FS att ignorera SSO tillstånd och gör en ny autentisering).</span><span class="sxs-lookup"><span data-stu-id="eac46-142">By default, Azure AD translates this in the request to ADFS to ‘*wauth=usernamepassworduri*’ (asks ADFS to do U/P auth) and ‘*wfresh=0*’ (asks ADFS to ignore SSO state and do a fresh authentication).</span></span> <span data-ttu-id="eac46-143">Om du vill aktivera certifikatbaserad autentisering för dessa appar måste du ändra standardbeteendet för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="eac46-143">If you want to enable certificate-based authentication for these apps, you need to modify the default Azure AD behavior.</span></span> <span data-ttu-id="eac46-144">Anger den '*PromptLoginBehavior*”i federerade domäninställningarna till'*inaktiverad*'.</span><span class="sxs-lookup"><span data-stu-id="eac46-144">Just set the ‘*PromptLoginBehavior*’ in your federated domain settings to ‘*Disabled*‘.</span></span> <span data-ttu-id="eac46-145">Du kan använda den [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) för att utföra den här uppgiften:</span><span class="sxs-lookup"><span data-stu-id="eac46-145">You can use the [MSOLDomainFederationSettings](/powershell/module/msonline/set-msoldomainfederationsettings?view=azureadps-1.0) cmdlet to perform this task:</span></span>

`Set-MSOLDomainFederationSettings -domainname <domain> -PromptLoginBehavior Disabled`



## <a name="exchange-activesync-clients-support"></a><span data-ttu-id="eac46-146">Stöd för Exchange ActiveSync-klienter</span><span class="sxs-lookup"><span data-stu-id="eac46-146">Exchange ActiveSync clients support</span></span>
<span data-ttu-id="eac46-147">Vissa Exchange ActiveSync-program på Android 5.0 (Lollipop) eller senare stöds.</span><span class="sxs-lookup"><span data-stu-id="eac46-147">Certain Exchange ActiveSync applications on Android 5.0 (Lollipop) or later are supported.</span></span> <span data-ttu-id="eac46-148">Kontakta din programutvecklaren för att avgöra om e-postprogrammet stöder den här funktionen.</span><span class="sxs-lookup"><span data-stu-id="eac46-148">To determine if your email application does support this feature, please contact your application developer.</span></span> 


## <a name="next-steps"></a><span data-ttu-id="eac46-149">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eac46-149">Next steps</span></span>

<span data-ttu-id="eac46-150">Om du vill konfigurera certifikatbaserad autentisering i din miljö, se [komma igång med certifikatbaserad autentisering på Android](active-directory-certificate-based-authentication-get-started.md) anvisningar.</span><span class="sxs-lookup"><span data-stu-id="eac46-150">If you want to configure certificate-based authentication in your environment, see [Get started with certificate-based authentication on Android](active-directory-certificate-based-authentication-get-started.md) for instructions.</span></span>

<!--Image references-->
[1]: ./media/active-directory-certificate-based-authentication-android/ic195031.png
