---
title: "Vad är Azure Active Directory B2B-samarbete? | Microsoft Docs"
description: "Företagsomfattande relationer genom att tilldela affärspartner selektiv åtkomst till företagets program har stöd för Azure Active Directory B2B-samarbete."
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
ms.openlocfilehash: fbc12a52555b190d43b5e953fd4d19923a25b0ed
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="what-is-azure-ad-b2b-collaboration"></a><span data-ttu-id="d067b-104">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="d067b-104">What is Azure AD B2B collaboration?</span></span>

<iframe width="560" height="315" src="https://www.youtube.com/embed/AhwrweCBdsc" frameborder="0" allowfullscreen></iframe>

<span data-ttu-id="d067b-105">Azure AD business-to-business (B2B) funktioner kan en organisation som använder Azure AD för att fungera på ett säkert sätt och på ett säkert sätt med användare från någon annan organisation små eller stora.</span><span class="sxs-lookup"><span data-stu-id="d067b-105">Azure AD business-to-business (B2B) collaboration capabilities enable any organization using Azure AD to work safely and securely with users from any other organization, small or large.</span></span> <span data-ttu-id="d067b-106">Dessa organisationer kan vara med Azure AD eller utan, eller med en IT-organisation eller utan.</span><span class="sxs-lookup"><span data-stu-id="d067b-106">Those organizations can be with Azure AD or without, or even with an IT organization or without.</span></span> 

<span data-ttu-id="d067b-107">Organisationer som använder Azure AD kan ge åtkomst till dokument, resurser och program till sina partner samtidigt fullständig kontroll över företagets data.</span><span class="sxs-lookup"><span data-stu-id="d067b-107">Organizations using Azure AD can provide access to documents, resources, and applications to their partners, while maintaining complete control over their own corporate data.</span></span> <span data-ttu-id="d067b-108">Utvecklare kan använda Azure AD business-to-business API: er för att skriva program som samordnar två organisationer i mer på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="d067b-108">Developers can use the Azure AD business-to-business APIs to write applications that bring two organizations together in more securely.</span></span> <span data-ttu-id="d067b-109">Det är också praktiskt för slutanvändare att navigera.</span><span class="sxs-lookup"><span data-stu-id="d067b-109">Also, it's pretty easy for end users to navigate.</span></span>

<span data-ttu-id="d067b-110">97% av våra kunder bett oss om Azure AD B2B-samarbete är mycket viktigt att dem.</span><span class="sxs-lookup"><span data-stu-id="d067b-110">97% of our customers have told us Azure AD B2B collaboration is very important to them.</span></span>

![cirkeldiagram](media/active-directory-b2b-what-is-azure-ad-b2b/97-percent-support.png)

<span data-ttu-id="d067b-112">Vi hade cirka 3 miljoner användare använder redan Azure AD B2B-samarbetesfunktioner från och med tidig April 2017.</span><span class="sxs-lookup"><span data-stu-id="d067b-112">As of early April 2017, we had about 3 million users already using Azure AD B2B collaboration capabilities.</span></span> <span data-ttu-id="d067b-113">Och mer än 23% av Azure AD-organisationer som har fler än 10 användare drar nytta redan av dessa funktioner.</span><span class="sxs-lookup"><span data-stu-id="d067b-113">And more than 23% of Azure AD organizations that have more than 10 users are already benefiting from these capabilities.</span></span>

## <a name="the-key-benefits-of-azure-ad-b2b-collaboration-to-your-organization"></a><span data-ttu-id="d067b-114">De främsta fördelarna med Azure AD B2B-samarbete för din organisation</span><span class="sxs-lookup"><span data-stu-id="d067b-114">The key benefits of Azure AD B2B collaboration to your organization</span></span>

### <a name="work-with-any-user-from-any-partner"></a><span data-ttu-id="d067b-115">Arbeta med alla användare från någon part</span><span class="sxs-lookup"><span data-stu-id="d067b-115">Work with any user from any partner</span></span>

* <span data-ttu-id="d067b-116">Partners använda sina egna autentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="d067b-116">Partners use their own credentials</span></span>

* <span data-ttu-id="d067b-117">Inga krav att använda Azure AD-partner</span><span class="sxs-lookup"><span data-stu-id="d067b-117">No requirement for partners to use Azure AD</span></span>

* <span data-ttu-id="d067b-118">Inga externa kataloger eller komplexa installation krävs</span><span class="sxs-lookup"><span data-stu-id="d067b-118">No external directories or complex set-up required</span></span>

### <a name="simple-and-secure-collaboration"></a><span data-ttu-id="d067b-119">Enkelt och säkert samarbete</span><span class="sxs-lookup"><span data-stu-id="d067b-119">Simple and secure collaboration</span></span>

* <span data-ttu-id="d067b-120">Ge åtkomst till företagets app eller data, samtidigt som du använder avancerade, Azure AD-påslagen auktoriseringsprinciper</span><span class="sxs-lookup"><span data-stu-id="d067b-120">Provide access to any corporate app or data, while applying sophisticated, Azure AD-powered authorization policies</span></span>

* <span data-ttu-id="d067b-121">Enkelt för användare</span><span class="sxs-lookup"><span data-stu-id="d067b-121">Easy for users</span></span>

* <span data-ttu-id="d067b-122">Säkerhet i företagsklass för appar och data</span><span class="sxs-lookup"><span data-stu-id="d067b-122">Enterprise-grade security for apps and data</span></span>

### <a name="no-management-overhead"></a><span data-ttu-id="d067b-123">Inga hanteringskostnader</span><span class="sxs-lookup"><span data-stu-id="d067b-123">No management overhead</span></span>

* <span data-ttu-id="d067b-124">Ingen extern hantering av konto eller lösenord</span><span class="sxs-lookup"><span data-stu-id="d067b-124">No external account or password management</span></span>

* <span data-ttu-id="d067b-125">Ingen synkronisering eller manuell livscykel kontohantering</span><span class="sxs-lookup"><span data-stu-id="d067b-125">No sync or manual account lifecycle management</span></span>

* <span data-ttu-id="d067b-126">Inga externa administrativa kostnader</span><span class="sxs-lookup"><span data-stu-id="d067b-126">No external administrative overhead</span></span>

## <a name="you-can-easily-add-b2b-collaboration-users-to-your-organization"></a><span data-ttu-id="d067b-127">Du kan enkelt lägga till B2B-samarbete användare för din organisation</span><span class="sxs-lookup"><span data-stu-id="d067b-127">You can easily add B2B collaboration users to your organization</span></span>

<span data-ttu-id="d067b-128">Administratörer kan lägga till B2B-samarbete (Gäst) användare i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d067b-128">Admins can add B2B collaboration (guest) users in the [Azure portal](https://portal.azure.com).</span></span>

![Lägg till gästanvändare](media/active-directory-b2b-what-is-azure-ad-b2b/adding-b2b-users-admin.png)

### <a name="enable-your-collaborators-to-bring-their-own-identity"></a><span data-ttu-id="d067b-130">Aktivera din medarbetare att ta sina egna identitet</span><span class="sxs-lookup"><span data-stu-id="d067b-130">Enable your collaborators to bring their own identity</span></span>

<span data-ttu-id="d067b-131">B2B medarbetare kan logga in med en identitet efter eget val.</span><span class="sxs-lookup"><span data-stu-id="d067b-131">B2B collaborators can sign in with an identity of their choice.</span></span> <span data-ttu-id="d067b-132">Om användaren inte har ett microsoftkonto eller ett Azure AD-katalogen skapas för dem sömlöst vid tidpunkten för erbjudande inlösning.</span><span class="sxs-lookup"><span data-stu-id="d067b-132">If the user doesn’t have a Microsoft account or an Azure AD account – one is created for them seamlessly at the time for offer redemption.</span></span>

![logga in identitet val](media/active-directory-b2b-what-is-azure-ad-b2b/sign-in-identity-choice.png)

### <a name="delegate-to-application-and-group-owners"></a><span data-ttu-id="d067b-134">Delegera till programmet och gruppen ägare</span><span class="sxs-lookup"><span data-stu-id="d067b-134">Delegate to application and group owners</span></span> 
<span data-ttu-id="d067b-135">Program- och gruppen ägare kan lägga till B2B användare direkt till alla program som intresserar dig, om det är ett Microsoft-program eller inte.</span><span class="sxs-lookup"><span data-stu-id="d067b-135">Application and group owners can add B2B users directly to any application that you care about, whether it is a Microsoft application or not.</span></span> <span data-ttu-id="d067b-136">Administratörer kan delegera behörighet att lägga till icke-administratörer B2B-användare.</span><span class="sxs-lookup"><span data-stu-id="d067b-136">Admins can delegate permission to add B2B users to non-admins.</span></span> <span data-ttu-id="d067b-137">Icke-administratörer kan använda den [åtkomstpanelen för Azure AD-program](https://myapps.microsoft.com) att lägga till program eller grupper B2B-samarbete användare.</span><span class="sxs-lookup"><span data-stu-id="d067b-137">Non-admins can use the [Azure AD Application Access Panel](https://myapps.microsoft.com) to add B2B collaboration users to applications or groups.</span></span>

![åtkomstpanelen](media/active-directory-b2b-what-is-azure-ad-b2b/access-panel.png)

![lägga till medlem](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="authorization-policies-protect-your-corporate-content"></a><span data-ttu-id="d067b-140">Auktoriseringsprinciper skyddar företagets innehållet</span><span class="sxs-lookup"><span data-stu-id="d067b-140">Authorization policies protect your corporate content</span></span>

<span data-ttu-id="d067b-141">Principer för villkorlig åtkomst, till exempel multifaktorautentisering, kan tillämpas:</span><span class="sxs-lookup"><span data-stu-id="d067b-141">Conditional access policies, such as multi-factor authentication, can be enforced:</span></span>
- <span data-ttu-id="d067b-142">På nivån för klient</span><span class="sxs-lookup"><span data-stu-id="d067b-142">At the tenant level</span></span>
- <span data-ttu-id="d067b-143">På programnivå</span><span class="sxs-lookup"><span data-stu-id="d067b-143">At the application level</span></span>
- <span data-ttu-id="d067b-144">För vissa användare att skydda företagets appar och data</span><span class="sxs-lookup"><span data-stu-id="d067b-144">For specific users to protect corporate apps and data</span></span>

![lägga till medlem](media/active-directory-b2b-what-is-azure-ad-b2b/add-member.png)

### <a name="use-our-apis-and-sample-code-to-easily-build-applications-to-onboard"></a><span data-ttu-id="d067b-146">Använd våra API: er och exempelkod enkelt kan skapa program ska publiceras</span><span class="sxs-lookup"><span data-stu-id="d067b-146">Use our APIs and sample code to easily build applications to onboard</span></span>
<span data-ttu-id="d067b-147">Ta din externa partners på sätt som är anpassade till din organisations behov.</span><span class="sxs-lookup"><span data-stu-id="d067b-147">Bring your external partners on board in ways customized to your organization’s needs.</span></span>

<span data-ttu-id="d067b-148">Med hjälp av den [B2B-samarbetsinbjudan API: er](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), du kan anpassa dina onboarding-upplevelser, inklusive att skapa registrering självbetjäningsportaler.</span><span class="sxs-lookup"><span data-stu-id="d067b-148">Using the [B2B collaboration invitation APIs](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation), you can customize your onboarding experiences, including creating self-service sign-up portals.</span></span> <span data-ttu-id="d067b-149">Vi tillhandahåller exempelkod för en självbetjäningsportal [på Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="d067b-149">We provide sample code for a self-service portal [on Github](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

![registreringsportalen](media/active-directory-b2b-what-is-azure-ad-b2b/sign-up-portal.png)

<span data-ttu-id="d067b-151">Du kan hämta alla fördelar med Azure AD att skydda din partnerrelationer på ett sätt som slutanvändarna hitta enkelt och intuitivt med Azure AD B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="d067b-151">With Azure AD B2B collaboration, you can get the full power of Azure AD to protect your partner relationships in a way that end users find easy and intuitive.</span></span> <span data-ttu-id="d067b-152">Alltså måste ansluta till tusentals organisationer som redan använder Azure AD B2B för sina externt samarbete!</span><span class="sxs-lookup"><span data-stu-id="d067b-152">So go ahead, join the thousands of organizations that are already using Azure AD B2B for their external collaboration!</span></span>

## <a name="next-steps"></a><span data-ttu-id="d067b-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d067b-153">Next steps</span></span>

* <span data-ttu-id="d067b-154">Administratören upplevelser finns i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d067b-154">Administrator experiences are found in the [Azure portal](https://portal.azure.com).</span></span>

* <span data-ttu-id="d067b-155">Information worker upplevelser är tillgängliga i den [åtkomstpanelen](https://myapps.microsoft.com).</span><span class="sxs-lookup"><span data-stu-id="d067b-155">Information worker experiences are available in the [Access Panel](https://myapps.microsoft.com).</span></span>

* <span data-ttu-id="d067b-156">Och som alltid ansluta med Produktteamet för feedback, diskussioner och förslag till våra [Microsoft teknisk Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span><span class="sxs-lookup"><span data-stu-id="d067b-156">And, as always, connect with the product team for any feedback, discussions, and suggestions through our [Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Azure-Active-Directory-B2B/bd-p/AzureAD_B2b).</span></span>

<span data-ttu-id="d067b-157">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="d067b-157">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="d067b-158">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="d067b-158">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="d067b-159">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="d067b-159">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="d067b-160">Elementen i e-postinbjudan B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="d067b-160">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="d067b-161">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="d067b-161">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="d067b-162">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="d067b-162">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="d067b-163">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="d067b-163">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="d067b-164">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="d067b-164">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="d067b-165">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="d067b-165">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="d067b-166">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="d067b-166">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="d067b-167">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="d067b-167">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="d067b-168">B2B-samarbete användaren granskning och rapportering</span><span class="sxs-lookup"><span data-stu-id="d067b-168">B2B collaboration user auditing and reporting</span></span>](active-directory-b2b-auditing-and-reporting.md)
* [<span data-ttu-id="d067b-169">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="d067b-169">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
