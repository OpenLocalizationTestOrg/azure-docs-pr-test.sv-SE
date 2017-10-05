---
title: "Hur du hämtar AppSource certifierad för Azure Active Directory | Microsoft Docs"
description: "Information om hur du hämtar programmet AppSource certifierad för Azure Active Directory."
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
ms.openlocfilehash: d8e2f8fc19ff879e6a7b632f033fd0ed9d77392a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="how-to-get-appsource-certified-for-azure-active-directory"></a><span data-ttu-id="aaf5e-103">Hur du hämtar AppSource certifierade för Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="aaf5e-103">How to get AppSource Certified for Azure Active Directory</span></span>
<span data-ttu-id="aaf5e-104">[Microsoft AppSource](https://appsource.microsoft.com/) är mål för användare i verksamheten att upptäcka, försök och hantera line-of-business SaaS-program (fristående SaaS och tillägg till befintliga Microsoft SaaS-produkter).</span><span class="sxs-lookup"><span data-stu-id="aaf5e-104">[Microsoft AppSource](https://appsource.microsoft.com/) is a destination for business users to discover, try, and manage line-of-business SaaS applications (standalone SaaS and add-on to existing Microsoft SaaS products).</span></span>

<span data-ttu-id="aaf5e-105">Om du vill visa en fristående SaaS-program på AppSource måste ditt program acceptera enkel inloggning från arbetskonton från alla företag eller organisation som har Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-105">To list a standalone SaaS application on AppSource, your application must accept single sign-on from work accounts from any company or organization that has Azure Active Directory.</span></span> <span data-ttu-id="aaf5e-106">Logga in processen måste använda den [OpenID Connect](./active-directory-protocols-openid-connect-code.md) eller [OAuth 2.0](./active-directory-protocols-oauth-code.md) protokoll.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-106">The sign-in process must use the [OpenID Connect](./active-directory-protocols-openid-connect-code.md) or [OAuth 2.0](./active-directory-protocols-oauth-code.md) protocols.</span></span> <span data-ttu-id="aaf5e-107">SAML-integration accepteras inte för AppSource certifiering.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-107">SAML integration is not accepted for AppSource certification.</span></span>

## <a name="guides-and-code-samples"></a><span data-ttu-id="aaf5e-108">Guider och kodexempel</span><span class="sxs-lookup"><span data-stu-id="aaf5e-108">Guides and code samples</span></span>
<span data-ttu-id="aaf5e-109">Om du vill lära dig hur du integrerar ditt program med Azure Active Directory med öppna ID connect, Följ våra guider och kodexempel i den [Utvecklarhandbok för Azure Active Directory](./active-directory-developers-guide.md#get-started "Kom igång med Azure AD för utvecklare").</span><span class="sxs-lookup"><span data-stu-id="aaf5e-109">If you want to learn about how to integrate your application with Azure Active Directory using Open ID connect, follow our guides and code samples in the [Azure Active Directory developer's guide](./active-directory-developers-guide.md#get-started "Get Started with Azure AD for developers").</span></span>

## <a name="multi-tenant-applications"></a><span data-ttu-id="aaf5e-110">Program med flera klienter</span><span class="sxs-lookup"><span data-stu-id="aaf5e-110">Multi-tenant applications</span></span>

<span data-ttu-id="aaf5e-111">Ett program som accepterar inloggningar från användare från företaget eller organisationen som har Azure Active Directory utan att kräva en separat instans, konfiguration och distribution kallas en *flera innehavare programmet*.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-111">An application that accepts sign-ins from users from any company or organization that have Azure Active Directory without requiring a separate instance, configuration, or deployment is known as a *multi-tenant application*.</span></span> <span data-ttu-id="aaf5e-112">AppSource rekommenderar att program implementera flera innehavare för att aktivera den *enkelklickning* kostnadsfria utvärderingsversionen.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-112">AppSource recommends that applications implement multi-tenancy to enable the *single-click* free trial experience.</span></span>

<span data-ttu-id="aaf5e-113">För att aktivera flera innehavare för ditt program:</span><span class="sxs-lookup"><span data-stu-id="aaf5e-113">In order to enable multi-tenancy on your application:</span></span>
- <span data-ttu-id="aaf5e-114">Ange `Multi-Tenanted` egenskapen `Yes` för programregistrering av på den [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (som standard konfigureras program som har skapats i Azure Portal som *stöd för en innehavare*)</span><span class="sxs-lookup"><span data-stu-id="aaf5e-114">Set `Multi-Tenanted` property to `Yes` on your application registration's information in the [Azure Portal](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/RegisteredApps) (by default, applications created in the Azure Portal are configured as *single-tenant*)</span></span>
- <span data-ttu-id="aaf5e-115">Uppdatera din kod för att skicka begäranden till den '`common`-slutpunkt (uppdatera slutpunkten från *https://login.microsoftonline.com/ {yourtenant}* till *https://login.microsoftonline.com/common*)</span><span class="sxs-lookup"><span data-stu-id="aaf5e-115">Update your code to send requests to the '`common`' endpoint (update the endpoint from *https://login.microsoftonline.com/{yourtenant}* to *https://login.microsoftonline.com/common*)</span></span>
- <span data-ttu-id="aaf5e-116">För vissa plattformar som ASP.NET, måste du också uppdatera din kod för att godkänna flera certifikatutfärdare</span><span class="sxs-lookup"><span data-stu-id="aaf5e-116">For some platforms, like ASP.NET, you need also to update your code to accept multiple issuers</span></span>

<span data-ttu-id="aaf5e-117">Läs mer om multitenans: [loggar in alla Azure Active Directory (AD)-användare med flera innehavare programmönster](./active-directory-devhowto-multi-tenant-overview.md).</span><span class="sxs-lookup"><span data-stu-id="aaf5e-117">For more information about multi-tenancy, see: [How to sign in any Azure Active Directory (AD) user using the multi-tenant application pattern](./active-directory-devhowto-multi-tenant-overview.md).</span></span>

### <a name="single-tenant-applications"></a><span data-ttu-id="aaf5e-118">Stöd för en innehavare program</span><span class="sxs-lookup"><span data-stu-id="aaf5e-118">Single-tenant applications</span></span>
<span data-ttu-id="aaf5e-119">Program som accepterar endast inloggningar från användare av en definierad Azure Active Directory-instans kallas *stöd för en innehavare programmet*.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-119">Applications that only accept sign-ins from users of a defined Azure Active Directory instance are known as *single-tenant application*.</span></span> <span data-ttu-id="aaf5e-120">Externa användare (inklusive arbetet eller skolan konton från andra organisationer eller personligt konto) kan logga in på en enskild klient programmet när du lägger till varje användare som *gästkontot* till Azure Active Directory-instansen att programmet har registrerats.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-120">External users (including Work or School accounts from other organizations, or personal account) can sign in to a single-tenant application after adding each user as *guest account* to the Azure Active Directory instance that the application is registered.</span></span> <span data-ttu-id="aaf5e-121">Du kan lägga till användare som gästkonton till en Azure Active Directory via den [ *Azure AD B2B-samarbete* ](../active-directory-b2b-what-is-azure-ad-b2b.md) - och det kan göras [genom att programmera](../active-directory-b2b-code-samples.md).</span><span class="sxs-lookup"><span data-stu-id="aaf5e-121">You can add users as guest accounts to an Azure Active Directory via the [*Azure AD B2B collaboration*](../active-directory-b2b-what-is-azure-ad-b2b.md) - and it can be done [programatically](../active-directory-b2b-code-samples.md).</span></span> <span data-ttu-id="aaf5e-122">När du lägger till en användare som gästkontot ett Azure Active Directory, skickas ett e-postinbjudan till användare måste acceptera inbjudan genom att klicka på länken i e-postinbjudan.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-122">When you add a user as guest account to an Azure Active Directory, an invitation email is sent to the user, who has to accept the invitation by clicking on the link in the invitation email.</span></span> <span data-ttu-id="aaf5e-123">Inbjudningar som skickas till en ytterligare användare i organisationen bjuda in som även är medlem i resurspartnerns organisation behöver inte acceptera en inbjudan för att logga in.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-123">Invitations that are sent to an additional user in an inviting organization that is also a member of the partner organization are not required to accept an invitation to sign in.</span></span>

<span data-ttu-id="aaf5e-124">Stöd för en innehavare program kan du aktivera den *kontakta mig* upplevelse, men om du vill aktivera den enkelklickning / kostnadsfria utvärderingsversionen som AppSource rekommenderar aktiverar multitenans för ditt program i stället.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-124">Single-tenant applications can enable the *Contact Me* experience, but if you want to enable the single-click/ free trial experience that AppSource recommends, enable multi-tenancy on your application instead.</span></span>


## <a name="appsource-trial-experiences"></a><span data-ttu-id="aaf5e-125">AppSource utvärderingsversion upplevelser</span><span class="sxs-lookup"><span data-stu-id="aaf5e-125">AppSource trial experiences</span></span>

### <a name="free-trial-customer-led-trial-experience"></a><span data-ttu-id="aaf5e-126">Kostnadsfri utvärderingsversion (kund-ledde utvärderingsversionen)</span><span class="sxs-lookup"><span data-stu-id="aaf5e-126">Free Trial (Customer-led trial experience)</span></span> 
<span data-ttu-id="aaf5e-127">Den *kunden ledde utvärderingsversion* är upplevelsen AppSource rekommenderar som den erbjuder ett enda musklick åtkomst till ditt program.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-127">The *customer-led trial* is the experience that AppSource recommends as it offers a single-click access to your application.</span></span> <span data-ttu-id="aaf5e-128">Under en illustration av hur det här upplevelsen ser ut som:</span><span class="sxs-lookup"><span data-stu-id="aaf5e-128">Below an illustration of how this experience looks like:</span></span><br/><br/>

<table >
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-129">Användaren hittar programmet i AppSource webbplats</span><span class="sxs-lookup"><span data-stu-id="aaf5e-129">User finds your application in AppSource Web Site</span></span></li><li><span data-ttu-id="aaf5e-130">'Kostnadsfri utvärderingsversion' alternativet väljs</span><span class="sxs-lookup"><span data-stu-id="aaf5e-130">Selects ‘Free trial’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step2.png" width="85%" /><ul><li><span data-ttu-id="aaf5e-131">AppSource dirigerar användaren till en URL i din webbplats</span><span class="sxs-lookup"><span data-stu-id="aaf5e-131">AppSource redirects user to a URL in your web site</span></span></li><li><span data-ttu-id="aaf5e-132">Din webbplats startar den <i>single-sign-on</i> automatiskt (på sidinläsning)</span><span class="sxs-lookup"><span data-stu-id="aaf5e-132">Your web site starts the <i>single-sign-on</i> process automatically (on page load)</span></span></li></ul></td>
    <td valign="top" width="33%">3.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-133">Användaren omdirigeras till Microsoft-inloggningssida</span><span class="sxs-lookup"><span data-stu-id="aaf5e-133">User is redirected to Microsoft Sign-in page</span></span></li><li><span data-ttu-id="aaf5e-134">Användaren anger autentiseringsuppgifter för inloggning</span><span class="sxs-lookup"><span data-stu-id="aaf5e-134">User provides credentials to sign in</span></span></li></ul></td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-135">Användaren lämnar samtycke för ditt program</span><span class="sxs-lookup"><span data-stu-id="aaf5e-135">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-136">Inloggningen har slutförts och användaren omdirigeras till webbplatsen</span><span class="sxs-lookup"><span data-stu-id="aaf5e-136">Sign-in completes and user is redirected back to your web site</span></span></li><li><span data-ttu-id="aaf5e-137">Användaren startar den kostnadsfria utvärderingsversionen</span><span class="sxs-lookup"><span data-stu-id="aaf5e-137">User starts the free trial</span></span></li></ul></td>
    <td></td>
</tr>
</table>

### <a name="contact-me-partner-led-trial-experience"></a><span data-ttu-id="aaf5e-138">Kontakta mig (Partner ledde utvärderingsversionen)</span><span class="sxs-lookup"><span data-stu-id="aaf5e-138">Contact Me (Partner-led trial experience)</span></span>
<span data-ttu-id="aaf5e-139">Den *partner utvärderingsversionen* kan användas när en manuell eller en långsiktig åtgärd som ska hända att etablera användaren / företaget: exempelvis programmet måste etablera virtuella datorer, databasinstanser eller åtgärder som tar lång tid att slutföra.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-139">The *partner trial experience* can be used when a manual or a long-term operation needs to happen to provision the user/ company: for example, your application needs to provision virtual machines, database instances, or operations that take much time to complete.</span></span> <span data-ttu-id="aaf5e-140">I det här fallet när användaren väljer den *begära utvärderingsversion* knappen och fyllning i ett formulär skickar AppSource användarens kontaktinformation.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-140">In this case, after user selects the *'Request Trial'* button and fills out a form, AppSource sends you the user's contact information.</span></span> <span data-ttu-id="aaf5e-141">Vid mottagning av den här informationen kan du sedan etablera miljön och skicka instruktionerna till användaren på hur du kommer åt utvärderingsversionen:</span><span class="sxs-lookup"><span data-stu-id="aaf5e-141">Upon receiving this information, you then provision the environment and send the instructions to the user on how to access the trial experience:</span></span><br/><br/>

<table valign="top">
<tr>
    <td valign="top" width="33%">1.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step1.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-142">Användaren hittar programmet i AppSource-webbplats</span><span class="sxs-lookup"><span data-stu-id="aaf5e-142">User finds your application in AppSource web site</span></span></li><li><span data-ttu-id="aaf5e-143">Kontakta mig alternativet automatiskt</span><span class="sxs-lookup"><span data-stu-id="aaf5e-143">Selects ‘Contact Me’ option</span></span></li></ul></td>
    <td valign="top" width="33%">2.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step2.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-144">Fyller i ett formulär med kontaktinformation</span><span class="sxs-lookup"><span data-stu-id="aaf5e-144">Fills out a form with contact information</span></span></li></ul></td>
     <td valign="top" width="33%">3.<br/><br/>
        <table bgcolor="#f7f7f7">
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/UserContact.png" width="55%"/></td>
            <td><span data-ttu-id="aaf5e-145">Du får användarinformation</span><span class="sxs-lookup"><span data-stu-id="aaf5e-145">You receive user information</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/SetupEnv.png" width="55%"/></td>
            <td><span data-ttu-id="aaf5e-146">Konfigurationsmiljö</span><span class="sxs-lookup"><span data-stu-id="aaf5e-146">Setup environment</span></span></td>
        </tr>
        <tr>
            <td><img src="media/active-directory-devhowto-appsource-certified/ContactCustomer.png" width="55%"/></td>
            <td><span data-ttu-id="aaf5e-147">Kontakta användaren med testversionen information</span><span class="sxs-lookup"><span data-stu-id="aaf5e-147">Contact user with trial info</span></span></td>
        </tr>
        </table><br/><br/>
        <ul><li><span data-ttu-id="aaf5e-148">Du får användaren information och utvärderingsversion instans av installationsprogrammet</span><span class="sxs-lookup"><span data-stu-id="aaf5e-148">You receive user's information and setup trial instance</span></span></li><li><span data-ttu-id="aaf5e-149">Du skickar hyperlänken ska få åtkomst till ditt program till användaren</span><span class="sxs-lookup"><span data-stu-id="aaf5e-149">You send the hyperlink to access your application to the user</span></span></li></ul>
    </td>
</tr>
<tr>
    <td valign="top" width="33%">4.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step3.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-150">Användaren har åtkomst till programmet och slutföra processen enkel inloggning</span><span class="sxs-lookup"><span data-stu-id="aaf5e-150">User accesses your application and complete the single-sign-on process</span></span></li></ul></td>
    <td valign="top" width="33%">5.<br/><img src="media/active-directory-devhowto-appsource-certified/partner-led-trial-step4.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-151">Användaren lämnar samtycke för ditt program</span><span class="sxs-lookup"><span data-stu-id="aaf5e-151">User gives consent for your application</span></span></li></ul></td>
    <td valign="top" width="33%">6.<br/><img src="media/active-directory-devhowto-appsource-certified/customer-led-trial-step5.png" width="85%"/><ul><li><span data-ttu-id="aaf5e-152">Inloggningen har slutförts och användaren omdirigeras till webbplatsen</span><span class="sxs-lookup"><span data-stu-id="aaf5e-152">Sign-in completes and user is redirected back to your web site</span></span></li><li><span data-ttu-id="aaf5e-153">Användaren startar den kostnadsfria utvärderingsversionen</span><span class="sxs-lookup"><span data-stu-id="aaf5e-153">User starts the free trial</span></span></li></ul></td>
   
</tr>
</table>

### <a name="more-information"></a><span data-ttu-id="aaf5e-154">Mer information</span><span class="sxs-lookup"><span data-stu-id="aaf5e-154">More information</span></span>
<span data-ttu-id="aaf5e-155">Läs mer om AppSource utvärderingsversionen [den här videon](https://aka.ms/trialexperienceforwebapps).</span><span class="sxs-lookup"><span data-stu-id="aaf5e-155">For more information about the AppSource trial experience, see [this video](https://aka.ms/trialexperienceforwebapps).</span></span> 
 
## <a name="next-steps"></a><span data-ttu-id="aaf5e-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aaf5e-156">Next Steps</span></span>

- <span data-ttu-id="aaf5e-157">Mer information om hur du skapar program som stöder Azure Active Directory inloggningar finns [Autentiseringsscenarier för Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span><span class="sxs-lookup"><span data-stu-id="aaf5e-157">For more information on building applications that support Azure Active Directory sign-ins, see [Authentication Scenarios for Azure AD](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-scenarios)</span></span> 

- <span data-ttu-id="aaf5e-158">Gå finns information om hur du listar SaaS-program i AppSource [AppSource partnerinformation](https://appsource.microsoft.com/partners)</span><span class="sxs-lookup"><span data-stu-id="aaf5e-158">For information on how to list your SaaS application in AppSource, go see [AppSource Partner Information](https://appsource.microsoft.com/partners)</span></span>


## <a name="get-support"></a><span data-ttu-id="aaf5e-159">Support</span><span class="sxs-lookup"><span data-stu-id="aaf5e-159">Get Support</span></span>
<span data-ttu-id="aaf5e-160">Vi använder för Azure Active Directory-integrering [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) med gemenskapen att tillhandahålla stöd för.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-160">For Azure Active Directory integration, we use [Stack Overflow](http://stackoverflow.com/questions/tagged/azure-active-directory) with the community to provide support.</span></span> 

<span data-ttu-id="aaf5e-161">Vi rekommenderar starkt att du be dina frågor på Stack Overflow först och bläddra befintliga problem för att se om någon har frågat din fråga innan.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-161">We highly recommend you ask your questions on Stack Overflow first and browse existing issues to see if someone has asked your question before.</span></span> <span data-ttu-id="aaf5e-162">Kontrollera att dina frågor eller kommentarer är märkta med `[azure-active-directory]`.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-162">Make sure that your questions or comments are tagged with `[azure-active-directory]`.</span></span>

<span data-ttu-id="aaf5e-163">Använd följande avsnitt för kommentarer för att ge feedback och hjälp oss att förfina och utforma innehållet.</span><span class="sxs-lookup"><span data-stu-id="aaf5e-163">Use the following comments section to provide feedback and help us refine and shape our content.</span></span>

<!--Reference style links -->
[AAD-Auth-Scenarios]: ./active-directory-authentication-scenarios.md
[AAD-Auth-Scenarios-Browser-To-WebApp]: ./active-directory-authentication-scenarios.md#web-browser-to-web-application
[AAD-Dev-Guide]: ./active-directory-developers-guide.md
[AAD-Howto-Multitenant-Overview]: ./active-directory-devhowto-multi-tenant-overview.md
[AAD-QuickStart-Web-Apps]: ./active-directory-developers-guide.md#get-started


<!--Image references-->