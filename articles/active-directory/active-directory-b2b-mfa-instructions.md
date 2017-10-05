---
title: "Villkorlig åtkomst för användare i Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete stöder multifaktorautentisering (MFA) för selektiv åtkomst till företagets program"
services: active-directory
documentationcenter: 
author: sasubram
manager: femila
editor: 
tags: 
ms.assetid: 
ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: identity
ms.date: 05/24/2017
ms.author: sasubram
ms.openlocfilehash: d85f711d6551a68d1248ae8ec61e2ecc1ddc8ecd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a><span data-ttu-id="488e8-103">Villkorlig åtkomst för användare för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="488e8-103">Conditional access for B2B collaboration users</span></span>

## <a name="multi-factor-authentication-for-b2b-users"></a><span data-ttu-id="488e8-104">Multi-Factor authentication för B2B-användare</span><span class="sxs-lookup"><span data-stu-id="488e8-104">Multi-factor authentication for B2B users</span></span>
<span data-ttu-id="488e8-105">Organisationer kan tillämpa principer för multifaktorautentisering (MFA) för B2B-användare med Azure AD B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="488e8-105">With Azure AD B2B collaboration, organizations can enforce multi-factor authentication (MFA) policies for B2B users.</span></span> <span data-ttu-id="488e8-106">Dessa principer kan tillämpas på klienten, app eller enskilda användarnivå, på samma sätt som de är aktiverade för heltidsanställda och medlemmar i organisationen.</span><span class="sxs-lookup"><span data-stu-id="488e8-106">These policies can be enforced at the tenant, app, or individual user level, the same way that they are enabled for full-time employees and members of the organization.</span></span> <span data-ttu-id="488e8-107">MFA-principer tillämpas på resursorganisationen.</span><span class="sxs-lookup"><span data-stu-id="488e8-107">MFA policies are enforced at the resource organization.</span></span>

<span data-ttu-id="488e8-108">Exempel:</span><span class="sxs-lookup"><span data-stu-id="488e8-108">Example:</span></span>
1. <span data-ttu-id="488e8-109">Administratören eller information worker i företag A uppmanar användare från företaget B till ett program *Foo* i företag A.</span><span class="sxs-lookup"><span data-stu-id="488e8-109">Admin or information worker in Company A invites user from company B to an application *Foo* in company A.</span></span>
2. <span data-ttu-id="488e8-110">Programmet *Foo* i företag A är konfigurerad för att kräva MFA på åtkomst.</span><span class="sxs-lookup"><span data-stu-id="488e8-110">Application *Foo* in company A is configured to require MFA on access.</span></span>
3. <span data-ttu-id="488e8-111">När användare från företaget B försöker komma åt appen *Foo* i företaget en klient villkoren uppmanas de att slutföra MFA-kontrollen.</span><span class="sxs-lookup"><span data-stu-id="488e8-111">When the user from company B attempts to access app *Foo* in the company A tenant, they are asked to complete an MFA challenge.</span></span>
4. <span data-ttu-id="488e8-112">Användaren kan ställa in sina MFA med företagets A och väljer alternativet för Multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="488e8-112">The user can set up their MFA with company A, and chooses their MFA option.</span></span>
5. <span data-ttu-id="488e8-113">Det här scenariot fungerar för alla identitet (Azure AD eller MSA om användare i företaget B autentisera med sociala ID exempelvis)</span><span class="sxs-lookup"><span data-stu-id="488e8-113">This scenario works for any identity (Azure AD or MSA, for example, if users in Company B authenticate using social ID)</span></span>
6. <span data-ttu-id="488e8-114">Företag A måste ha tillräckligt många licenser för Premium Azure AD som stöder MFA.</span><span class="sxs-lookup"><span data-stu-id="488e8-114">Company A must have sufficient Premium Azure AD licenses that support MFA.</span></span> <span data-ttu-id="488e8-115">Användare från företaget B använder denna licens från företag A.</span><span class="sxs-lookup"><span data-stu-id="488e8-115">The user from company B consumes this license from company A.</span></span>

<span data-ttu-id="488e8-116">Bjuda in innehavare är alltid ansvarig för MFA för användare från partnerorganisationen, även om partnerorganisationens har funktioner för MFA.</span><span class="sxs-lookup"><span data-stu-id="488e8-116">The inviting tenancy is always responsible for MFA for users from the partner organization, even if the partner organization has MFA capabilities.</span></span>

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a><span data-ttu-id="488e8-117">Konfigurera MFA för B2B-samarbete användare</span><span class="sxs-lookup"><span data-stu-id="488e8-117">Setting up MFA for B2B collaboration users</span></span>
<span data-ttu-id="488e8-118">För att identifiera hur lätt det är att konfigurera MFA för B2B-samarbete användare finns i följande video:</span><span class="sxs-lookup"><span data-stu-id="488e8-118">To discover how easy it is to set up MFA for B2B collaboration users, see how in the following video:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a><span data-ttu-id="488e8-119">B2B användare MFA upplevelse i erbjuder inlösning</span><span class="sxs-lookup"><span data-stu-id="488e8-119">B2B users MFA experience for offer redemption</span></span>
<span data-ttu-id="488e8-120">Kolla in följande animeringen ska se betalda upplevelse:</span><span class="sxs-lookup"><span data-stu-id="488e8-120">Check out the following animation to see the redemption experience:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a><span data-ttu-id="488e8-121">MFA återställa för B2B-samarbete användare</span><span class="sxs-lookup"><span data-stu-id="488e8-121">MFA reset for B2B collaboration users</span></span>
<span data-ttu-id="488e8-122">För närvarande kan kan administratören kräva B2B-samarbete användare bevis upp igen endast med hjälp av följande PowerShell-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="488e8-122">Currently, the admin can require B2B collaboration users to proof up again only by using the following PowerShell cmdlets:</span></span>

1. <span data-ttu-id="488e8-123">Anslut till Azure AD</span><span class="sxs-lookup"><span data-stu-id="488e8-123">Connect to Azure AD</span></span>

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. <span data-ttu-id="488e8-124">Hämta alla användare med bevis metoder</span><span class="sxs-lookup"><span data-stu-id="488e8-124">Get all users with proof up methods</span></span>

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  <span data-ttu-id="488e8-125">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="488e8-125">Here is an example:</span></span>

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. <span data-ttu-id="488e8-126">Återställ MFA-metoden för en viss användare B2B-samarbete användaren ange bevis upp igen.</span><span class="sxs-lookup"><span data-stu-id="488e8-126">Reset the MFA method for a specific user to require the B2B collaboration user to set proof-up methods again.</span></span> <span data-ttu-id="488e8-127">Exempel:</span><span class="sxs-lookup"><span data-stu-id="488e8-127">Example:</span></span>

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-the-resource-tenancy"></a><span data-ttu-id="488e8-128">Varför gör vi MFA på resursen innehavare?</span><span class="sxs-lookup"><span data-stu-id="488e8-128">Why do we perform MFA at the resource tenancy?</span></span>

<span data-ttu-id="488e8-129">I den aktuella versionen är MFA alltid i resurs-innehavare, på grund av förutsägbarhet.</span><span class="sxs-lookup"><span data-stu-id="488e8-129">In the current release, MFA is always in the resource tenancy, for reasons of predictability.</span></span> <span data-ttu-id="488e8-130">Anta att till exempel Fabrikam uppmanas användaren Contoso (obegränsad) och Fabrikam har aktiverat MFA för B2B-användare.</span><span class="sxs-lookup"><span data-stu-id="488e8-130">For example, let’s say a Contoso user (Sally) is invited to Fabrikam and Fabrikam has enabled MFA for B2B users.</span></span>

<span data-ttu-id="488e8-131">Om Contoso har aktiverats för App1 men inte App2 MFA-principen, kan sedan om vi tittar på Contoso MFA-anspråk i token, vi se följande problem:</span><span class="sxs-lookup"><span data-stu-id="488e8-131">If Contoso has MFA policy enabled for App1 but not App2, then if we look at the Contoso MFA claim in the token, we might see the following issue:</span></span>

* <span data-ttu-id="488e8-132">Dag 1: En användare har MFA i Contoso och har åtkomst till App1 och inga ytterligare MFA uppmaning visas i Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="488e8-132">Day 1: A user has MFA in Contoso and is accessing App1, then no additional MFA prompt is shown in Fabrikam.</span></span>

* <span data-ttu-id="488e8-133">Dag 2: Användaren har åtkomst till appen 2 i Contoso, så nu att få åtkomst till Fabrikam, de måste registrera sig för MFA det.</span><span class="sxs-lookup"><span data-stu-id="488e8-133">Day 2: The user has accessed App 2 in Contoso, so now when accessing Fabrikam, they must register for MFA there.</span></span>

<span data-ttu-id="488e8-134">Den här processen kan vara förvirrande och kan leda till att släppa inloggningen slutföranden.</span><span class="sxs-lookup"><span data-stu-id="488e8-134">This process can be confusing and could lead to drop in sign-in completions.</span></span>

<span data-ttu-id="488e8-135">Dessutom även om Contoso MFA funktionen, är det inte alltid fallet Fabrikam skulle förtroende Contoso MFA-principen.</span><span class="sxs-lookup"><span data-stu-id="488e8-135">Moreover, even if Contoso has MFA capability, it is not always the case the Fabrikam would trust the Contoso MFA policy.</span></span>

<span data-ttu-id="488e8-136">Slutligen fungerar resurs klient MFA även för MSA: er och sociala-ID: N och partner organisationer som behöver inte konfigurera MFA.</span><span class="sxs-lookup"><span data-stu-id="488e8-136">Finally, resource tenant MFA also works for MSAs and social IDs and for partner orgs that do not have MFA set up.</span></span>

<span data-ttu-id="488e8-137">Rekommendation för MFA för B2B-användare är därför att alltid kräva MFA i bjuda in klienten.</span><span class="sxs-lookup"><span data-stu-id="488e8-137">Therefore, the recommendation for MFA for B2B users is to always require MFA in the inviting tenant.</span></span> <span data-ttu-id="488e8-138">Detta krav kan leda till dubbla MFA i vissa fall, men när åtkomst till bjuda in innehavaren av slutanvändare är förutsägbar: Lisa måste registrera sig för MFA med bjuda in innehavaren.</span><span class="sxs-lookup"><span data-stu-id="488e8-138">This requirement could lead to double MFA in some cases, but whenever accessing the inviting tenant, the end-users experience is predictable: Sally must register for MFA with the inviting tenant.</span></span>

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a><span data-ttu-id="488e8-139">Enhetsbaserad platsbaserad och risker-baserad villkorlig åtkomst för B2B-användare</span><span class="sxs-lookup"><span data-stu-id="488e8-139">Device-based, location-based, and risk-based conditional access for B2B users</span></span>

<span data-ttu-id="488e8-140">När Contoso aktiverar enhetsbaserad villkorlig åtkomstvillkorspolicy till företagets data, förhindrade åtkomst från enheter som inte hanteras av Contoso och som inte är kompatibla med principer för Contoso-enheter.</span><span class="sxs-lookup"><span data-stu-id="488e8-140">When Contoso enables device-based conditional access policies for their corporate data, access is prevented from devices that are not managed by Contoso and not compliant with the Contoso device policies.</span></span>

<span data-ttu-id="488e8-141">Om enheten B2B inte hanteras av Contoso, blockeras åtkomsten B2B användare från resurspartner-organisationer i den kontexten principerna tillämpas.</span><span class="sxs-lookup"><span data-stu-id="488e8-141">If the B2B user’s device isn't managed by Contoso, access of B2B users from the partner organizations is blocked in whatever context these policies are enforced.</span></span> <span data-ttu-id="488e8-142">Contoso kan dock skapa undantagslistor som innehåller viss partner användare om du vill utelämna dem från principen för enhetsbaserad villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="488e8-142">However, Contoso can create exclusion lists containing specific partner users to exclude them from the device-based conditional access policy.</span></span>

#### <a name="location-based-conditional-access-for-b2b"></a><span data-ttu-id="488e8-143">Plats-baserad villkorlig åtkomst för B2B</span><span class="sxs-lookup"><span data-stu-id="488e8-143">Location-based conditional access for B2B</span></span>

<span data-ttu-id="488e8-144">Principer för plats-baserad villkorlig åtkomst tillämpas för B2B användare om bjuda in organisationen kan skapa en tillförlitlig IP-adressintervall som definierar deras partnerorganisationer.</span><span class="sxs-lookup"><span data-stu-id="488e8-144">Location-based conditional access policies can be enforced for B2B users if the inviting organization is able to create a trusted IP address range that defines their partner organizations.</span></span>

#### <a name="risk-based-conditional-access-for-b2b"></a><span data-ttu-id="488e8-145">Risk-baserad villkorlig åtkomst för B2B</span><span class="sxs-lookup"><span data-stu-id="488e8-145">Risk-based conditional access for B2B</span></span>

<span data-ttu-id="488e8-146">För närvarande kan inte använda risk-baserade inloggning principer på B2B användare eftersom riskbedömningen utförs på B2B användarens hemorganisation.</span><span class="sxs-lookup"><span data-stu-id="488e8-146">Currently, risk-based sign-in policies cannot be applied to B2B users because the risk evaluation is performed at the B2B user’s home organization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="488e8-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="488e8-147">Next steps</span></span>

<span data-ttu-id="488e8-148">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="488e8-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="488e8-149">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="488e8-149">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="488e8-150">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="488e8-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="488e8-151">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="488e8-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="488e8-152">Elementen i e-postinbjudan B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="488e8-152">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="488e8-153">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="488e8-153">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="488e8-154">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="488e8-154">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="488e8-155">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="488e8-155">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="488e8-156">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="488e8-156">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="488e8-157">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="488e8-157">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="488e8-158">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="488e8-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="488e8-159">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="488e8-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
