---
title: "aaaWhat är Azure Active Directory B2B-samarbete? | Microsoft Docs"
description: "Företagsomfattande relationer genom att aktivera business partners tooselectively åtkomst till företagets program har stöd för Azure Active Directory B2B-samarbete."
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 1464387b-ee8b-4c7c-94b3-2c5567224c27
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 06/27/2017
ms.author: curtand
ms.custom: aaddev
ms.reviewer: sasubram
ms.openlocfilehash: 359989b66f3d012c306e8748a607662fffacb919
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a><span data-ttu-id="4fdb1-104">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="4fdb1-104">What is Azure AD B2B collaboration?</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

<span data-ttu-id="4fdb1-105">Azure AD business-to-business (B2B) funktioner kan en organisation med Azure AD toowork enkelt och säkert användare från någon annan organisation små eller stora.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-105">Azure AD business-to-business (B2B) collaboration capabilities enable any organization using Azure AD toowork safely and securely with users from any other organization, small or large.</span></span> <span data-ttu-id="4fdb1-106">Dessa organisationer kan vara med Azure AD eller utan, eller med en IT-organisation eller utan.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-106">Those organizations can be with Azure AD or without, or even with an IT organization or without.</span></span> 

<span data-ttu-id="4fdb1-107">Organisationer som använder Azure AD kan ge åtkomst toodocuments, resurser och program tootheir partners, samtidigt som fullständig kontroll över företagets data.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-107">Organizations using Azure AD can provide access toodocuments, resources, and applications tootheir partners, while maintaining complete control over their own corporate data.</span></span> <span data-ttu-id="4fdb1-108">Utvecklare kan använda hello Azure AD business-to-business-API: er toowrite program som samordnar två organisationer i mer på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-108">Developers can use hello Azure AD business-to-business APIs toowrite applications that bring two organizations together in more securely.</span></span> <span data-ttu-id="4fdb1-109">Det är också praktiskt för slutanvändare toonavigate.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-109">Also, it's pretty easy for end users toonavigate.</span></span>

<span data-ttu-id="4fdb1-110">97% av våra kunder bett oss om Azure AD B2B-samarbete är mycket viktigt toothem.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-110">97% of our customers have told us Azure AD B2B collaboration is very important toothem.</span></span>

![cirkeldiagram](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

<span data-ttu-id="4fdb1-112">Vi hade cirka 3 miljoner användare använder redan Azure AD B2B-samarbetesfunktioner från och med tidig April 2017.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-112">As of early April 2017, we had about 3 million users already using Azure AD B2B collaboration capabilities.</span></span> <span data-ttu-id="4fdb1-113">Och mer än 23% av Azure AD-organisationer som har fler än 10 användare drar nytta redan av dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-113">And more than 23% of Azure AD organizations that have more than 10 users are already benefiting from these capabilities.</span></span>

## <a name="hello-key-benefits-of-azure-ad-b2b-collaboration-tooyour-organization"></a><span data-ttu-id="4fdb1-114">hello viktiga fördelar med Azure AD B2B-samarbete tooyour organisation</span><span class="sxs-lookup"><span data-stu-id="4fdb1-114">hello key benefits of Azure AD B2B collaboration tooyour organization</span></span>

### <a name="work-with-any-user-from-any-partner"></a><span data-ttu-id="4fdb1-115">Arbeta med alla användare från någon part</span><span class="sxs-lookup"><span data-stu-id="4fdb1-115">Work with any user from any partner</span></span>

* <span data-ttu-id="4fdb1-116">Partners använda sina egna autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="4fdb1-116">Partners use their own credentials</span></span>

* <span data-ttu-id="4fdb1-117">Inga krav för partners toouse Azure AD</span><span class="sxs-lookup"><span data-stu-id="4fdb1-117">No requirement for partners toouse Azure AD</span></span>

* <span data-ttu-id="4fdb1-118">Inga externa kataloger eller komplexa installation krävs</span><span class="sxs-lookup"><span data-stu-id="4fdb1-118">No external directories or complex set-up required</span></span>

### <a name="simple-and-secure-collaboration"></a><span data-ttu-id="4fdb1-119">Enkelt och säkert samarbete</span><span class="sxs-lookup"><span data-stu-id="4fdb1-119">Simple and secure collaboration</span></span>

* <span data-ttu-id="4fdb1-120">Ge åtkomst tooany företagets app eller data, samtidigt som du använder avancerade, Azure AD-påslagen auktoriseringsprinciper</span><span class="sxs-lookup"><span data-stu-id="4fdb1-120">Provide access tooany corporate app or data, while applying sophisticated, Azure AD-powered authorization policies</span></span>

* <span data-ttu-id="4fdb1-121">Enkelt för användare</span><span class="sxs-lookup"><span data-stu-id="4fdb1-121">Easy for users</span></span>

* <span data-ttu-id="4fdb1-122">Säkerhet i företagsklass för appar och data</span><span class="sxs-lookup"><span data-stu-id="4fdb1-122">Enterprise-grade security for apps and data</span></span>

### <a name="no-management-overhead"></a><span data-ttu-id="4fdb1-123">Inga hanteringskostnader</span><span class="sxs-lookup"><span data-stu-id="4fdb1-123">No management overhead</span></span>

* <span data-ttu-id="4fdb1-124">Ingen extern hantering av konto eller lösenord</span><span class="sxs-lookup"><span data-stu-id="4fdb1-124">No external account or password management</span></span>

* <span data-ttu-id="4fdb1-125">Ingen synkronisering eller manuell livscykel kontohantering</span><span class="sxs-lookup"><span data-stu-id="4fdb1-125">No sync or manual account lifecycle management</span></span>

* <span data-ttu-id="4fdb1-126">Inga externa administrativa kostnader</span><span class="sxs-lookup"><span data-stu-id="4fdb1-126">No external administrative overhead</span></span>

## <a name="you-can-easily-add-b2b-collaboration-users-tooyour-organization"></a><span data-ttu-id="4fdb1-127">Du kan enkelt lägga till B2B-samarbete användare tooyour organisation</span><span class="sxs-lookup"><span data-stu-id="4fdb1-127">You can easily add B2B collaboration users tooyour organization</span></span>

<span data-ttu-id="4fdb1-128">Administratörer kan lägga till B2B-samarbete (Gäst) användare i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4fdb1-128">Admins can add B2B collaboration (guest) users in hello [Azure portal](https://portal.azure.com).</span></span>

![Lägg till gästanvändare](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-toobring-their-own-identity"></a><span data-ttu-id="4fdb1-130">Aktivera din medarbetare toobring sin egen identitet</span><span class="sxs-lookup"><span data-stu-id="4fdb1-130">Enable your collaborators toobring their own identity</span></span>

<span data-ttu-id="4fdb1-131">B2B medarbetare kan logga in med en identitet efter eget val.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-131">B2B collaborators can sign in with an identity of their choice.</span></span> <span data-ttu-id="4fdb1-132">Om hello användaren har inte ett Microsoft-konto eller ett Azure AD-konto – en skapas för dem sömlöst vid hello för erbjudande åtgärd.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-132">If hello user doesn’t have a Microsoft account or an Azure AD account – one is created for them seamlessly at hello time for offer redemption.</span></span>

![logga in identitet val](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-tooapplication-and-group-owners"></a><span data-ttu-id="4fdb1-134">Delegera tooapplication och gruppen ägare</span><span class="sxs-lookup"><span data-stu-id="4fdb1-134">Delegate tooapplication and group owners</span></span> 
<span data-ttu-id="4fdb1-135">Program- och gruppen ägare kan lägga till B2B-användare direkt tooany program som intresserar dig, om det är ett Microsoft-program eller inte.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-135">Application and group owners can add B2B users directly tooany application that you care about, whether it is a Microsoft application or not.</span></span> <span data-ttu-id="4fdb1-136">Administratörer kan delegera behörighet tooadd B2B användare toonon-administratörer.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-136">Admins can delegate permission tooadd B2B users toonon-admins.</span></span> <span data-ttu-id="4fdb1-137">Icke-administratörer kan använda hello [åtkomstpanelen för Azure AD-programmet](https://myapps.microsoft.com) tooadd B2B collaboration tooapplications för användare eller grupper.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-137">Non-admins can use hello [Azure AD Application Access Panel](https://myapps.microsoft.com) tooadd B2B collaboration users tooapplications or groups.</span></span>

![åtkomstpanelen](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![lägga till medlem](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a><span data-ttu-id="4fdb1-140">Auktoriseringsprinciper skyddar företagets innehållet</span><span class="sxs-lookup"><span data-stu-id="4fdb1-140">Authorization policies protect your corporate content</span></span>

<span data-ttu-id="4fdb1-141">Principer för villkorlig åtkomst, till exempel multifaktorautentisering, kan tillämpas:</span><span class="sxs-lookup"><span data-stu-id="4fdb1-141">Conditional access policies, such as multi-factor authentication, can be enforced:</span></span>
- <span data-ttu-id="4fdb1-142">På nivån för hello-klient</span><span class="sxs-lookup"><span data-stu-id="4fdb1-142">At hello tenant level</span></span>
- <span data-ttu-id="4fdb1-143">På programnivå hello</span><span class="sxs-lookup"><span data-stu-id="4fdb1-143">At hello application level</span></span>
- <span data-ttu-id="4fdb1-144">För specifika användare tooprotect företagets appar och data</span><span class="sxs-lookup"><span data-stu-id="4fdb1-144">For specific users tooprotect corporate apps and data</span></span>

![lägga till medlem](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-tooeasily-build-applications-tooonboard"></a><span data-ttu-id="4fdb1-146">Använd våra API: er och exempel kod tooeasily bygga program tooonboard</span><span class="sxs-lookup"><span data-stu-id="4fdb1-146">Use our APIs and sample code tooeasily build applications tooonboard</span></span>
<span data-ttu-id="4fdb1-147">Ta din externa partners på tåget i sätt anpassade tooyour organisations behov.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-147">Bring your external partners on board in ways customized tooyour organization’s needs.</span></span>

<span data-ttu-id="4fdb1-148">Med hjälp av hello [B2B-samarbetsinbjudan API: er](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), du kan anpassa dina onboarding-upplevelser, inklusive att skapa registrering självbetjäningsportaler.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-148">Using hello [B2B collaboration invitation APIs](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), you can customize your onboarding experiences, including creating self-service sign-up portals.</span></span> <span data-ttu-id="4fdb1-149">Vi tillhandahåller exempelkod för en självbetjäningsportal [på Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="4fdb1-149">We provide sample code for a self-service portal [on Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

![registreringsportalen](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

<span data-ttu-id="4fdb1-151">Med Azure AD B2B-samarbete du hello alla fördelar med Azure AD tooprotect din partnerrelationer på ett sätt som slutanvändarna hitta enkelt och intuitivt.</span><span class="sxs-lookup"><span data-stu-id="4fdb1-151">With Azure AD B2B collaboration, you can get hello full power of Azure AD tooprotect your partner relationships in a way that end users find easy and intuitive.</span></span> <span data-ttu-id="4fdb1-152">Därför gå vidare, koppling hello tusentalsavgränsare för organisationer som redan använder Azure AD B2B för sina externt samarbete!</span><span class="sxs-lookup"><span data-stu-id="4fdb1-152">So go ahead, join hello thousands of organizations that are already using Azure AD B2B for their external collaboration!</span></span>

## <a name="next-steps"></a><span data-ttu-id="4fdb1-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4fdb1-153">Next steps</span></span>

* <span data-ttu-id="4fdb1-154">Administratören upplevelser finns i hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="4fdb1-154">Administrator experiences are found in hello [Azure portal](https://portal.azure.com).</span></span>

* <span data-ttu-id="4fdb1-155">Information worker upplevelser är tillgängliga i hello [åtkomstpanelen](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="4fdb1-155">Information worker experiences are available in hello [Access Panel](https://myapps.microsoft.com).</span></span>

* <span data-ttu-id="4fdb1-156">Och som alltid ansluta med hello Produktteamet för feedback, diskussioner och förslag till våra [Microsoft teknisk Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span><span class="sxs-lookup"><span data-stu-id="4fdb1-156">And, as always, connect with hello product team for any feedback, discussions, and suggestions through our [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span></span>

<span data-ttu-id="4fdb1-157">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="4fdb1-157">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="4fdb1-158">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="4fdb1-158">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="4fdb1-159">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="4fdb1-159">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="4fdb1-160">hello-element i hello e-postinbjudan för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4fdb1-160">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="4fdb1-161">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="4fdb1-161">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="4fdb1-162">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="4fdb1-162">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="4fdb1-163">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4fdb1-163">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="4fdb1-164">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4fdb1-164">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="4fdb1-165">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="4fdb1-165">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="4fdb1-166">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="4fdb1-166">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="4fdb1-167">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="4fdb1-167">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="4fdb1-168">B2B-samarbete användaren granskning och rapportering</span><span class="sxs-lookup"><span data-stu-id="4fdb1-168">B2B collaboration user auditing and reporting</span></span>](active-directory-b2b-auditing-and-reporting.md)
* [<span data-ttu-id="4fdb1-169">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="4fdb1-169">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
