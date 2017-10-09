---
title: "aaaConditional åtkomst för användare för Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete stöder multifaktorautentisering (MFA) för selektiv åtkomst tooyour företagsprogram"
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
ms.openlocfilehash: 3a05be4393f74ff8e87f32432a222a5fbac9af62
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="conditional-access-for-b2b-collaboration-users"></a><span data-ttu-id="feb34-103">Villkorlig åtkomst för användare för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="feb34-103">Conditional access for B2B collaboration users</span></span>

## <a name="multi-factor-authentication-for-b2b-users"></a><span data-ttu-id="feb34-104">Multi-Factor authentication för B2B-användare</span><span class="sxs-lookup"><span data-stu-id="feb34-104">Multi-factor authentication for B2B users</span></span>
<span data-ttu-id="feb34-105">Organisationer kan tillämpa principer för multifaktorautentisering (MFA) för B2B-användare med Azure AD B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="feb34-105">With Azure AD B2B collaboration, organizations can enforce multi-factor authentication (MFA) policies for B2B users.</span></span> <span data-ttu-id="feb34-106">Dessa principer kan tillämpas på hello klienten, app eller enskilda användarnivå hello samma sätt som de är aktiverade för heltidsanställda och medlemmar i hello organisation.</span><span class="sxs-lookup"><span data-stu-id="feb34-106">These policies can be enforced at hello tenant, app, or individual user level, hello same way that they are enabled for full-time employees and members of hello organization.</span></span> <span data-ttu-id="feb34-107">MFA-principer tillämpas på hello resursorganisationen.</span><span class="sxs-lookup"><span data-stu-id="feb34-107">MFA policies are enforced at hello resource organization.</span></span>

<span data-ttu-id="feb34-108">Exempel:</span><span class="sxs-lookup"><span data-stu-id="feb34-108">Example:</span></span>
1. <span data-ttu-id="feb34-109">Administratören eller information worker i företag A uppmanar användare från företaget B tooan program *Foo* i företag A.</span><span class="sxs-lookup"><span data-stu-id="feb34-109">Admin or information worker in Company A invites user from company B tooan application *Foo* in company A.</span></span>
2. <span data-ttu-id="feb34-110">Programmet *Foo* i företag A är konfigurerade toorequire MFA på åtkomst.</span><span class="sxs-lookup"><span data-stu-id="feb34-110">Application *Foo* in company A is configured toorequire MFA on access.</span></span>
3. <span data-ttu-id="feb34-111">När hello användare från företaget B försöker tooaccess app *Foo* hello företag en klientorganisation, de är och toocomplete MFA-kontrollen.</span><span class="sxs-lookup"><span data-stu-id="feb34-111">When hello user from company B attempts tooaccess app *Foo* in hello company A tenant, they are asked toocomplete an MFA challenge.</span></span>
4. <span data-ttu-id="feb34-112">hello användaren kan ställa in sina MFA med företagets A och väljer alternativet sina MFA.</span><span class="sxs-lookup"><span data-stu-id="feb34-112">hello user can set up their MFA with company A, and chooses their MFA option.</span></span>
5. <span data-ttu-id="feb34-113">Det här scenariot fungerar för alla identitet (Azure AD eller MSA om användare i företaget B autentisera med sociala ID exempelvis)</span><span class="sxs-lookup"><span data-stu-id="feb34-113">This scenario works for any identity (Azure AD or MSA, for example, if users in Company B authenticate using social ID)</span></span>
6. <span data-ttu-id="feb34-114">Företag A måste ha tillräckligt många licenser för Premium Azure AD som stöder MFA.</span><span class="sxs-lookup"><span data-stu-id="feb34-114">Company A must have sufficient Premium Azure AD licenses that support MFA.</span></span> <span data-ttu-id="feb34-115">hello användare från företaget B använder denna licens från företag A.</span><span class="sxs-lookup"><span data-stu-id="feb34-115">hello user from company B consumes this license from company A.</span></span>

<span data-ttu-id="feb34-116">hello bjuda in innehavare är alltid ansvarig för MFA för användare från partnerorganisationen hello, även om hello partnerorganisation har funktioner för MFA.</span><span class="sxs-lookup"><span data-stu-id="feb34-116">hello inviting tenancy is always responsible for MFA for users from hello partner organization, even if hello partner organization has MFA capabilities.</span></span>

### <a name="setting-up-mfa-for-b2b-collaboration-users"></a><span data-ttu-id="feb34-117">Konfigurera MFA för B2B-samarbete användare</span><span class="sxs-lookup"><span data-stu-id="feb34-117">Setting up MFA for B2B collaboration users</span></span>
<span data-ttu-id="feb34-118">toodiscover hur lätt det är tooset in MFA för B2B-samarbete användare kan se hur i hello följande video:</span><span class="sxs-lookup"><span data-stu-id="feb34-118">toodiscover how easy it is tooset up MFA for B2B collaboration users, see how in hello following video:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/b2b-conditional-access-setup/Player]

### <a name="b2b-users-mfa-experience-for-offer-redemption"></a><span data-ttu-id="feb34-119">B2B användare MFA upplevelse i erbjuder inlösning</span><span class="sxs-lookup"><span data-stu-id="feb34-119">B2B users MFA experience for offer redemption</span></span>
<span data-ttu-id="feb34-120">Kolla hello efter animering toosee hello inlösning upplevelse:</span><span class="sxs-lookup"><span data-stu-id="feb34-120">Check out hello following animation toosee hello redemption experience:</span></span>

>[!VIDEO https://channel9.msdn.com/Blogs/Azure/MFA-redemption/Player]

### <a name="mfa-reset-for-b2b-collaboration-users"></a><span data-ttu-id="feb34-121">MFA återställa för B2B-samarbete användare</span><span class="sxs-lookup"><span data-stu-id="feb34-121">MFA reset for B2B collaboration users</span></span>
<span data-ttu-id="feb34-122">För närvarande Hej administratör kan kräva B2B-samarbete användare tooproof in igen med hello följande PowerShell-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="feb34-122">Currently, hello admin can require B2B collaboration users tooproof up again only by using hello following PowerShell cmdlets:</span></span>

1. <span data-ttu-id="feb34-123">Ansluta tooAzure AD</span><span class="sxs-lookup"><span data-stu-id="feb34-123">Connect tooAzure AD</span></span>

  ```
  $cred = Get-Credential
  Connect-MsolService -Credential $cred
  ```
2. <span data-ttu-id="feb34-124">Hämta alla användare med bevis metoder</span><span class="sxs-lookup"><span data-stu-id="feb34-124">Get all users with proof up methods</span></span>

  ```
  Get-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```
  <span data-ttu-id="feb34-125">Här är ett exempel:</span><span class="sxs-lookup"><span data-stu-id="feb34-125">Here is an example:</span></span>

  ```
  PS C:\Users\tjwasserGet-MsolUser | where { $_.StrongAuthenticationMethods} | select UserPrincipalName, @{n="Methods";e={($_.StrongAuthenticationMethods).MethodType}}
  ```

3. <span data-ttu-id="feb34-126">Återställa hello MFA-metoden för en specifik användare toorequire hello B2B-samarbete användaren tooset bevis upp metoder igen.</span><span class="sxs-lookup"><span data-stu-id="feb34-126">Reset hello MFA method for a specific user toorequire hello B2B collaboration user tooset proof-up methods again.</span></span> <span data-ttu-id="feb34-127">Exempel:</span><span class="sxs-lookup"><span data-stu-id="feb34-127">Example:</span></span>

  ```
  Reset-MsolStrongAuthenticationMethodByUpn -UserPrincipalName gsamoogle_gmail.com#EXT#@ WoodGroveAzureAD.onmicrosoft.com
  ```

### <a name="why-do-we-perform-mfa-at-hello-resource-tenancy"></a><span data-ttu-id="feb34-128">Varför gör vi MFA vid hello resurs innehavare?</span><span class="sxs-lookup"><span data-stu-id="feb34-128">Why do we perform MFA at hello resource tenancy?</span></span>

<span data-ttu-id="feb34-129">I hello aktuella versionen är MFA alltid i hello resurs innehavare av förutsägbarhet.</span><span class="sxs-lookup"><span data-stu-id="feb34-129">In hello current release, MFA is always in hello resource tenancy, for reasons of predictability.</span></span> <span data-ttu-id="feb34-130">Anta att till exempel en Contoso-användare (obegränsad) är inbjudna tooFabrikam och Fabrikam har aktiverat MFA för B2B-användare.</span><span class="sxs-lookup"><span data-stu-id="feb34-130">For example, let’s say a Contoso user (Sally) is invited tooFabrikam and Fabrikam has enabled MFA for B2B users.</span></span>

<span data-ttu-id="feb34-131">Om Contoso har aktiverats för App1 men inte App2 MFA-principen, kan sedan om vi tittar på hello Contoso MFA anspråk i hello token vi se hello följande problem:</span><span class="sxs-lookup"><span data-stu-id="feb34-131">If Contoso has MFA policy enabled for App1 but not App2, then if we look at hello Contoso MFA claim in hello token, we might see hello following issue:</span></span>

* <span data-ttu-id="feb34-132">Dag 1: En användare har MFA i Contoso och har åtkomst till App1 och inga ytterligare MFA uppmaning visas i Fabrikam.</span><span class="sxs-lookup"><span data-stu-id="feb34-132">Day 1: A user has MFA in Contoso and is accessing App1, then no additional MFA prompt is shown in Fabrikam.</span></span>

* <span data-ttu-id="feb34-133">Dag 2: hello användare har åtkomst till appen 2 i Contoso, nu att få åtkomst till Fabrikam, de måste registrera sig för MFA det.</span><span class="sxs-lookup"><span data-stu-id="feb34-133">Day 2: hello user has accessed App 2 in Contoso, so now when accessing Fabrikam, they must register for MFA there.</span></span>

<span data-ttu-id="feb34-134">Den här processen kan vara förvirrande och leda toodrop i inloggning slutföranden.</span><span class="sxs-lookup"><span data-stu-id="feb34-134">This process can be confusing and could lead toodrop in sign-in completions.</span></span>

<span data-ttu-id="feb34-135">Dessutom även om Contoso MFA funktionen, är det inte alltid hello case hello Fabrikam skulle förtroende hello Contoso MFA-principen.</span><span class="sxs-lookup"><span data-stu-id="feb34-135">Moreover, even if Contoso has MFA capability, it is not always hello case hello Fabrikam would trust hello Contoso MFA policy.</span></span>

<span data-ttu-id="feb34-136">Slutligen fungerar resurs klient MFA även för MSA: er och sociala-ID: N och partner organisationer som behöver inte konfigurera MFA.</span><span class="sxs-lookup"><span data-stu-id="feb34-136">Finally, resource tenant MFA also works for MSAs and social IDs and for partner orgs that do not have MFA set up.</span></span>

<span data-ttu-id="feb34-137">Därför är hello rekommendation för MFA för användare B2B tooalways kräva MFA i hello bjuda in klient.</span><span class="sxs-lookup"><span data-stu-id="feb34-137">Therefore, hello recommendation for MFA for B2B users is tooalways require MFA in hello inviting tenant.</span></span> <span data-ttu-id="feb34-138">Det här kravet medför toodouble MFA i vissa fall, men när åtkomst till hello bjuda in klient hello slutanvändare upplevelse är förutsägbar: Lisa måste registrera sig för MFA med hello bjuda in innehavaren.</span><span class="sxs-lookup"><span data-stu-id="feb34-138">This requirement could lead toodouble MFA in some cases, but whenever accessing hello inviting tenant, hello end-users experience is predictable: Sally must register for MFA with hello inviting tenant.</span></span>

### <a name="device-based-location-based-and-risk-based-conditional-access-for-b2b-users"></a><span data-ttu-id="feb34-139">Enhetsbaserad platsbaserad och risker-baserad villkorlig åtkomst för B2B-användare</span><span class="sxs-lookup"><span data-stu-id="feb34-139">Device-based, location-based, and risk-based conditional access for B2B users</span></span>

<span data-ttu-id="feb34-140">När Contoso aktiverar enhetsbaserad villkorlig åtkomstvillkorspolicy till företagets data, förhindrade åtkomst från enheter som inte hanteras av Contoso och inte är kompatibla med principer för hello Contoso-enheter.</span><span class="sxs-lookup"><span data-stu-id="feb34-140">When Contoso enables device-based conditional access policies for their corporate data, access is prevented from devices that are not managed by Contoso and not compliant with hello Contoso device policies.</span></span>

<span data-ttu-id="feb34-141">Om hello B2B användarens enhet inte hanteras av Contoso, blockeras åtkomsten av B2B användare från hello partnerorganisationer i oavsett kontexten principerna tillämpas.</span><span class="sxs-lookup"><span data-stu-id="feb34-141">If hello B2B user’s device isn't managed by Contoso, access of B2B users from hello partner organizations is blocked in whatever context these policies are enforced.</span></span> <span data-ttu-id="feb34-142">Contoso kan dock skapa undantag listor som innehåller viss partner användare tooexclude dem från hello enhetsbaserad villkorlig åtkomstprincip.</span><span class="sxs-lookup"><span data-stu-id="feb34-142">However, Contoso can create exclusion lists containing specific partner users tooexclude them from hello device-based conditional access policy.</span></span>

#### <a name="location-based-conditional-access-for-b2b"></a><span data-ttu-id="feb34-143">Plats-baserad villkorlig åtkomst för B2B</span><span class="sxs-lookup"><span data-stu-id="feb34-143">Location-based conditional access for B2B</span></span>

<span data-ttu-id="feb34-144">Principer för plats-baserad villkorlig åtkomst tillämpas för B2B användare om hello bjuda in organisation kan toocreate ett tillförlitliga IP-adressintervall som definierar deras partnerorganisationer.</span><span class="sxs-lookup"><span data-stu-id="feb34-144">Location-based conditional access policies can be enforced for B2B users if hello inviting organization is able toocreate a trusted IP address range that defines their partner organizations.</span></span>

#### <a name="risk-based-conditional-access-for-b2b"></a><span data-ttu-id="feb34-145">Risk-baserad villkorlig åtkomst för B2B</span><span class="sxs-lookup"><span data-stu-id="feb34-145">Risk-based conditional access for B2B</span></span>

<span data-ttu-id="feb34-146">Risk-baserade inloggning principer kan för närvarande inte tillämpade tooB2B användare eftersom hello riskbedömning utförs på hello B2B användarens hemorganisation.</span><span class="sxs-lookup"><span data-stu-id="feb34-146">Currently, risk-based sign-in policies cannot be applied tooB2B users because hello risk evaluation is performed at hello B2B user’s home organization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="feb34-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="feb34-147">Next steps</span></span>

<span data-ttu-id="feb34-148">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="feb34-148">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="feb34-149">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="feb34-149">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="feb34-150">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="feb34-150">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="feb34-151">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="feb34-151">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="feb34-152">hello-element i hello e-postinbjudan för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="feb34-152">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="feb34-153">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="feb34-153">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="feb34-154">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="feb34-154">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="feb34-155">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="feb34-155">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="feb34-156">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="feb34-156">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="feb34-157">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="feb34-157">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="feb34-158">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="feb34-158">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="feb34-159">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="feb34-159">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
