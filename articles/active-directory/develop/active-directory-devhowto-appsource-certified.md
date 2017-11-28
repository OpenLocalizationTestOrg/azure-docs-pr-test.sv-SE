---
title: "aaaHow tooget AppSource certifierad för Azure Active Directory | Microsoft Docs"
description: "Information om hur tooget programmet AppSource certifierad för Azure Active Directory."
services: active-directory
documentationcenter: 
author: andretms
manager: mbaldwin
editor: 
ms.assetid: 21206407-49f8-4c0b-84d1-c25e17cd4183
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 08/03/2017
ms.author: andret
ms.custom: aaddev
ms.openlocfilehash: e9f07e9220afcba1120b987122fe770fe5225eed
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooget-appsource-certified-for-azure-active-directory"></a><span data-ttu-id="39257-103">Hur tooget AppSource certifierad för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="39257-103">How tooget AppSource Certified for Azure Active Directory</span></span>
<span data-ttu-id="39257-104">[Microsoft AppSource](https://appsource.microsoft.com/) är ett mål för business användare toodiscover försök och hantera line-of-business SaaS-program (fristående SaaS och tillägg tooexisting Microsoft SaaS-produkter).</span><span class="sxs-lookup"><span data-stu-id="39257-104">[Microsoft AppSource](https://appsource.microsoft.com/) is a destination for business users toodiscover, try, and manage line-of-business SaaS applications (standalone SaaS and add-on tooexisting Microsoft SaaS products).</span></span>

<span data-ttu-id="39257-105">toolist fristående SaaS-program på AppSource ditt program måste acceptera enkel inloggning från arbetskonton från alla företag eller organisation som har Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="39257-105">toolist a standalone SaaS application on AppSource, your application must accept single sign-on from work accounts from any company or organization that has Azure Active Directory.</span></span> <span data-ttu-id="39257-106">hello inloggningsprocessen måste använda hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) eller [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokoll.</span><span class="sxs-lookup"><span data-stu-id="39257-106">hello sign-in process must use hello [OpenID Connect](./active-directory-protocols-openid-connect-code.md) or [OAuth 2.0](./active-directory-protocols-oauth-code.md) protocols.</span></span> <span data-ttu-id="39257-107">SAML-integration accepteras inte för AppSource certifiering.</span><span class="sxs-lookup"><span data-stu-id="39257-107">SAML integration is not accepted for AppSource certification.</span></span>

## <a name="guides-and-code-samples"></a><span data-ttu-id="39257-108">Guider och kodexempel</span><span class="sxs-lookup"><span data-stu-id="39257-108">Guides and code samples</span></span>
<span data-ttu-id="39257-109">Om du vill toolearn om hur toointegrate ditt program med Azure Active Directory med öppna ID connect, Följ våra guider och kodexempel i hello [Utvecklarhandbok för Azure Active Directory](./active-directory-developers-guide.md#get-started "komma igång med Azure AD för utvecklare").</span><span class="sxs-lookup"><span data-stu-id="39257-109">If you want toolearn about how toointegrate your application with Azure Active Directory using Open ID connect, follow our guides and code samples in hello [Azure Active Directory developer's guide](./active-directory-developers-guide.md#get-started "Get Started with Azure AD for developers").</span></span>

## <a name="multi-tenant-applications"></a><span data-ttu-id="39257-110">Program med flera klienter</span><span class="sxs-lookup"><span data-stu-id="39257-110">Multi-tenant applications</span></span>

<span data-ttu-id="39257-111">Ett program som accepterar inloggningar från användare från företaget eller organisationen som har Azure Active Directory utan att kräva en separat instans, konfiguration och distribution kallas en *flera innehavare programmet*.</span><span class="sxs-lookup"><span data-stu-id="39257-111">An application that accepts sign-ins from users from any company or organization that have Azure Active Directory without requiring a separate instance, configuration, or deployment is known as a *multi-tenant application*.</span></span> <span data-ttu-id="39257-112">AppSource rekommenderar att program implementera multitenans tooenable hello *enkelklickning* kostnadsfria utvärderingsversionen.</span><span class="sxs-lookup"><span data-stu-id="39257-112">AppSource recommends that applications implement multi-tenancy tooenable hello *single-click* free trial experience.</span></span>

<span data-ttu-id="39257-113">I ordning tooenable multitenans för ditt program:</span><span class="sxs-lookup"><span data-stu-id="39257-113">In order tooenable multi-tenancy on your application:</span></span>
- <span data-ttu-id="39257-114">Ange `Multi-Tenanted` egenskapen för`Yes` för registrering av applikation på hello [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (som standard konfigureras program som har skapats i hello Azure-portalen som *stöd för en innehavare*)</span><span class="sxs-lookup"><span data-stu-id="39257-114">Set `Multi-Tenanted` property too`Yes` on your application registration's information in hello [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (by default, applications created in hello Azure Portal are configured as *single-tenant*)</span></span>
- <span data-ttu-id="39257-115">Uppdatera din kod toosend begäranden toohello '`common`-slutpunkt (uppdatera hello slutpunkten från *https://login.microsoftonline.com/ {yourtenant}* för*https://login.microsoftonline.com/common*)</span><span class="sxs-lookup"><span data-stu-id="39257-115">Update your code toosend requests toohello '`common`' endpoint (update hello endpoint from *https://login.microsoftonline.com/{yourtenant}* too*https://login.microsoftonline.com/common*)</span></span>
- <span data-ttu-id="39257-116">För vissa plattformar som ASP.NET, behöver du också tooupdate din kod tooaccept flera certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="39257-116">For some platforms, like ASP.NET, you need also tooupdate your code tooaccept multiple issuers</span></span>

<span data-ttu-id="39257-117">Läs mer om multitenans: [hur hello toosign i alla Azure Active Directory (AD) användare med flera innehavare programmönster](./active-directory-devhowto-multi-tenant-overview.md).</span><span class="sxs-lookup"><span data-stu-id="39257-117">For more information about multi-tenancy, see: [How toosign in any Azure Active Directory (AD) user using hello multi-tenant application pattern](./active-directory-devhowto-multi-tenant-overview.md).</span></span>

### <a name="single-tenant-applications"></a><span data-ttu-id="39257-118">Stöd för en innehavare program</span><span class="sxs-lookup"><span data-stu-id="39257-118">Single-tenant applications</span></span>
<span data-ttu-id="39257-119">Program som accepterar endast inloggningar från användare av en definierad Azure Active Directory-instans kallas *stöd för en innehavare programmet*.</span><span class="sxs-lookup"><span data-stu-id="39257-119">Applications that only accept sign-ins from users of a defined Azure Active Directory instance are known as *single-tenant application*.</span></span> <span data-ttu-id="39257-120">Externa användare (inklusive arbetet eller skolan konton från andra organisationer eller personligt konto) kan logga in tooa stöd för en innehavare programmet när du lägger till varje användare som *gästkontot* toohello Azure Active Directory-instans som programmet hello är registrerat.</span><span class="sxs-lookup"><span data-stu-id="39257-120">External users (including Work or School accounts from other organizations, or personal account) can sign in tooa single-tenant application after adding each user as *guest account* toohello Azure Active Directory instance that hello application is registered.</span></span> <span data-ttu-id="39257-121">Du kan lägga till användare som gäst konton tooan Azure Active Directory via hello [ *Azure AD B2B-samarbete* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - och det kan göras [genom att programmera](../active-directory-b2b-code-samples.md).</span><span class="sxs-lookup"><span data-stu-id="39257-121">You can add users as guest accounts tooan Azure Active Directory via hello [*Azure AD B2B collaboration*](../active-directory-b2b-what-is-azure-ad-b2b.md) - and it can be done [programatically](../active-directory-b2b-code-samples.md).</span></span> <span data-ttu-id="39257-122">När du lägger till en användare som gäst konto tooan Azure Active Directory, skickas ett e-postinbjudan toohello användare, som har tooaccept hello inbjudan genom att klicka på hello länken i e-postinbjudan hello.</span><span class="sxs-lookup"><span data-stu-id="39257-122">When you add a user as guest account tooan Azure Active Directory, an invitation email is sent toohello user, who has tooaccept hello invitation by clicking on hello link in hello invitation email.</span></span> <span data-ttu-id="39257-123">Inbjudningar som skickas tooan ytterligare användare i organisationen bjuda in som även är medlem i hello partnerorganisation är inte obligatoriska tooaccept en inbjudan toosign i.</span><span class="sxs-lookup"><span data-stu-id="39257-123">Invitations that are sent tooan additional user in an inviting organization that is also a member of hello partner organization are not required tooaccept an invitation toosign in.</span></span>

<span data-ttu-id="39257-124">Stöd för en innehavare program kan du aktivera hello *kontakta mig* upplevelse, men om du vill tooenable hello enkelklickning / kostnadsfria utvärderingsversionen som AppSource rekommenderar, aktiverar du multitenans för ditt program i stället.</span><span class="sxs-lookup"><span data-stu-id="39257-124">Single-tenant applications can enable hello *Contact Me* experience, but if you want tooenable hello single-click/ free trial experience that AppSource recommends, enable multi-tenancy on your application instead.</span></span>


## <a name="appsource-trial-experiences"></a><span data-ttu-id="39257-125">AppSource utvärderingsversion upplevelser</span><span class="sxs-lookup"><span data-stu-id="39257-125">AppSource trial experiences</span></span>

### <a name="free-trial-customer-led-trial-experience"></a><span data-ttu-id="39257-126">Kostnadsfri utvärderingsversion (kund-ledde utvärderingsversionen)</span><span class="sxs-lookup"><span data-stu-id="39257-126">Free Trial (Customer-led trial experience)</span></span> 
<span data-ttu-id="39257-127">Hej *kunden ledde utvärderingsversion* är hello-upplevelse som AppSource rekommenderar som den erbjuder ett enda musklick åtkomst tooyour program.</span><span class="sxs-lookup"><span data-stu-id="39257-127">hello *customer-led trial* is hello experience that AppSource recommends as it offers a single-click access tooyour application.</span></span> <span data-ttu-id="39257-128">Under en illustration av hur det här upplevelsen ser ut som:</span><span class="sxs-lookup"><span data-stu-id="39257-128">Below an illustration of how this experience looks like:</span></span><br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="39257-129">Användaren hittar programmet i AppSource webbplats</span><span class="sxs-lookup"><span data-stu-id="39257-129">User finds your application in AppSource Web Site</span></span></li><li><span data-ttu-id="39257-130">'Kostnadsfri utvärderingsversion' alternativet väljs</span><span class="sxs-lookup"><span data-stu-id="39257-130">Selects ‘Free trial’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li><span data-ttu-id="39257-131">AppSource dirigerar användaren tooa URL på din webbplats</span><span class="sxs-lookup"><span data-stu-id="39257-131">AppSource redirects user tooa URL in your web site</span></span></li><li><span data-ttu-id="39257-132">Webbplatsen startar hello <i>single-sign-on</i> automatiskt (på sidinläsning)</span><span class="sxs-lookup"><span data-stu-id="39257-132">Your web site starts hello <i>single-sign-on</i> process automatically (on page load)</span></span></li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="39257-133">Användaren är omdirigerade tooMicrosoft inloggning sida</span><span class="sxs-lookup"><span data-stu-id="39257-133">User is redirected tooMicrosoft Sign-in page</span></span></li><li><span data-ttu-id="39257-134">Användaren anger autentiseringsuppgifter toosign i</span><span class="sxs-lookup"><span data-stu-id="39257-134">User provides credentials toosign in</span></span></li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="39257-135">Användaren lämnar samtycke för ditt program</span><span class="sxs-lookup"><span data-stu-id="39257-135">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="39257-136">Inloggningen har slutförts och användaren är omdirigerade tillbaka tooyour webbplats</span><span class="sxs-lookup"><span data-stu-id="39257-136">Sign-in completes and user is redirected back tooyour web site</span></span></li><li><span data-ttu-id="39257-137">Användaren startar hello kostnadsfri utvärderingsversion</span><span class="sxs-lookup"><span data-stu-id="39257-137">User starts hello free trial</span></span></li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a><span data-ttu-id="39257-138">Kontakta mig (Partner ledde utvärderingsversionen)</span><span class="sxs-lookup"><span data-stu-id="39257-138">Contact Me (Partner-led trial experience)</span></span>
<span data-ttu-id="39257-139">Hej *partner utvärderingsversionen* kan användas när en manuell eller en långsiktig åtgärd behöver toohappen tooprovision hello användare / företaget: exempelvis programmet måste tooprovision virtuella datorer, databasinstanser, eller åtgärder som tar mycket tid toocomplete.</span><span class="sxs-lookup"><span data-stu-id="39257-139">hello *partner trial experience* can be used when a manual or a long-term operation needs toohappen tooprovision hello user/ company: for example, your application needs tooprovision virtual machines, database instances, or operations that take much time toocomplete.</span></span> <span data-ttu-id="39257-140">I det här fallet när användaren väljer hello *begära utvärderingsversion* knappen och fyller i ett formulär, AppSource skickar du hello användares kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="39257-140">In this case, after user selects hello *'Request Trial'* button and fills out a form, AppSource sends you hello user's contact information.</span></span> <span data-ttu-id="39257-141">Vid mottagning av den här informationen kan du sedan etablera hello miljö och skicka hello instruktioner toohello användare på hur tooaccess hello utvärderingsversionen:</span><span class="sxs-lookup"><span data-stu-id="39257-141">Upon receiving this information, you then provision hello environment and send hello instructions toohello user on how tooaccess hello trial experience:</span></span><br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="39257-142">Användaren hittar programmet i AppSource-webbplats</span><span class="sxs-lookup"><span data-stu-id="39257-142">User finds your application in AppSource web site</span></span></li><li><span data-ttu-id="39257-143">Kontakta mig alternativet automatiskt</span><span class="sxs-lookup"><span data-stu-id="39257-143">Selects ‘Contact Me’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li><span data-ttu-id="39257-144">Fyller i ett formulär med kontaktinformation</span><span class="sxs-lookup"><span data-stu-id="39257-144">Fills out a form with contact information</span></span></li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td><span data-ttu-id="39257-145">Du får användarinformation</span><span class="sxs-lookup"><span data-stu-id="39257-145">You receive user information</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td><span data-ttu-id="39257-146">Konfigurationsmiljö</span><span class="sxs-lookup"><span data-stu-id="39257-146">Setup environment</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td><span data-ttu-id="39257-147">Kontakta användaren med testversionen information</span><span class="sxs-lookup"><span data-stu-id="39257-147">Contact user with trial info</span></span></td>
        </tr>
        </table><br/><br/>
        <ul><li><span data-ttu-id="39257-148">Du får användaren information och utvärderingsversion instans av installationsprogrammet</span><span class="sxs-lookup"><span data-stu-id="39257-148">You receive user's information and setup trial instance</span></span></li><li><span data-ttu-id="39257-149">Du kan skicka hello hyperlink tooaccess dina programanvändare toohello</span><span class="sxs-lookup"><span data-stu-id="39257-149">You send hello hyperlink tooaccess your application toohello user</span></span></li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="39257-150">Användare har åtkomst till programmet och fullständig hello single-sign-on-processen</span><span class="sxs-lookup"><span data-stu-id="39257-150">User accesses your application and complete hello single-sign-on process</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="39257-151">Användaren lämnar samtycke för ditt program</span><span class="sxs-lookup"><span data-stu-id="39257-151">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="39257-152">Inloggningen har slutförts och användaren är omdirigerade tillbaka tooyour webbplats</span><span class="sxs-lookup"><span data-stu-id="39257-152">Sign-in completes and user is redirected back tooyour web site</span></span></li><li><span data-ttu-id="39257-153">Användaren startar hello kostnadsfri utvärderingsversion</span><span class="sxs-lookup"><span data-stu-id="39257-153">User starts hello free trial</span></span></li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a><span data-ttu-id="39257-154">Mer information</span><span class="sxs-lookup"><span data-stu-id="39257-154">More information</span></span>
<span data-ttu-id="39257-155">Läs mer om hello AppSource utvärderingsversionen [den här videon](https://aka.ms/trialexperienceforwebapps).</span><span class="sxs-lookup"><span data-stu-id="39257-155">For more information about hello AppSource trial experience, see [this video](https://aka.ms/trialexperienceforwebapps).</span></span> 
 
## <a name="next-steps"></a><span data-ttu-id="39257-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="39257-156">Next Steps</span></span>

- <span data-ttu-id="39257-157">Mer information om hur du skapar program som stöder Azure Active Directory inloggningar finns [Autentiseringsscenarier för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span><span class="sxs-lookup"><span data-stu-id="39257-157">For more information on building applications that support Azure Active Directory sign-ins, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span></span> 

- <span data-ttu-id="39257-158">Mer information om hur toolist SaaS-program i AppSource, gå finns [AppSource partnerinformation](https://appsource.microsoft.com/partners)</span><span class="sxs-lookup"><span data-stu-id="39257-158">For information on how toolist your SaaS application in AppSource, go see [AppSource Partner Information](https://appsource.microsoft.com/partners)</span></span>


## <a name="get-support"></a><span data-ttu-id="39257-159">Support</span><span class="sxs-lookup"><span data-stu-id="39257-159">Get Support</span></span>
<span data-ttu-id="39257-160">Vi använder för Azure Active Directory-integrering [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) med tooprovide hello community-support.</span><span class="sxs-lookup"><span data-stu-id="39257-160">For Azure Active Directory integration, we use [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) with hello community tooprovide support.</span></span> 

<span data-ttu-id="39257-161">Vi rekommenderar starkt att du be dina frågor på Stack Overflow först och bläddra befintliga problem toosee om någon har frågat din fråga innan.</span><span class="sxs-lookup"><span data-stu-id="39257-161">We highly recommend you ask your questions on Stack Overflow first and browse existing issues toosee if someone has asked your question before.</span></span> <span data-ttu-id="39257-162">Kontrollera att dina frågor eller kommentarer är märkta med `[azure-active-directory]`.</span><span class="sxs-lookup"><span data-stu-id="39257-162">Make sure that your questions or comments are tagged with `[azure-active-directory]`.</span></span>

<span data-ttu-id="39257-163">Använd hello följande kommentarer avsnittet tooprovide feedback och hjälp oss att förfina och utforma innehållet.</span><span class="sxs-lookup"><span data-stu-id="39257-163">Use hello following comments section tooprovide feedback and help us refine and shape our content.</span></span>

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->