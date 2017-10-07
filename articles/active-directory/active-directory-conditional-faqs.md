---
title: "aaaAzure villkorlig åtkomst för Active Directory och svar | Microsoft Docs"
description: "Få svar toofrequently frågor och svar om villkorlig åtkomst i Azure Active Directory."
services: active-directory
documentationcenter: 
author: MarkusVi
manager: femila
ms.assetid: 14f7fc83-f4bb-41bf-b6f1-a9bb97717c34
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/05/2017
ms.author: markvi
ms.reviewer: calebb
ms.openlocfilehash: d23acbb01217d7e9717d1a43de1b46a929404118
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-conditional-access-faqs"></a><span data-ttu-id="b4197-103">Azure Active Directory villkorlig åtkomst vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="b4197-103">Azure Active Directory conditional access FAQs</span></span>

## <a name="which-applications-work-with-conditional-access-policies"></a><span data-ttu-id="b4197-104">Vilka program fungerar med principer för villkorlig åtkomst?</span><span class="sxs-lookup"><span data-stu-id="b4197-104">Which applications work with conditional access policies?</span></span>

<span data-ttu-id="b4197-105">Information om program som fungerar med principer för villkorlig åtkomst finns [program och webbläsare som använder regler för villkorlig åtkomst i Azure Active Directory](active-directory-conditional-access-supported-apps.md).</span><span class="sxs-lookup"><span data-stu-id="b4197-105">For information about applications that work with conditional access policies, see [Applications and browsers that use conditional access rules in Azure Active Directory](active-directory-conditional-access-supported-apps.md).</span></span>

## <a name="are-conditional-access-policies-enforced-for-b2b-collaboration-and-guest-users"></a><span data-ttu-id="b4197-106">Tillämpas principer för villkorlig åtkomst för B2B-samarbete och gästanvändare?</span><span class="sxs-lookup"><span data-stu-id="b4197-106">Are conditional access policies enforced for B2B collaboration and guest users?</span></span>

<span data-ttu-id="b4197-107">Principerna tillämpas för business-to-business (B2B) samarbete användare.</span><span class="sxs-lookup"><span data-stu-id="b4197-107">Policies are enforced for business-to-business (B2B) collaboration users.</span></span> <span data-ttu-id="b4197-108">Men i vissa fall kan kanske en användare inte kan toosatisfy hello-kraven.</span><span class="sxs-lookup"><span data-stu-id="b4197-108">However, in some cases, a user might not be able toosatisfy hello policy requirements.</span></span> <span data-ttu-id="b4197-109">En gästanvändare organisation kan till exempel inte stöd för multifaktorautentisering.</span><span class="sxs-lookup"><span data-stu-id="b4197-109">For example, a guest user's organization might not support multi-factor authentication.</span></span> 



## <a name="does-a-sharepoint-online-policy-also-apply-tooonedrive-for-business"></a><span data-ttu-id="b4197-110">En SharePoint Online policy även gäller tooOneDrive för företag?</span><span class="sxs-lookup"><span data-stu-id="b4197-110">Does a SharePoint Online policy also apply tooOneDrive for Business?</span></span>

<span data-ttu-id="b4197-111">Ja.</span><span class="sxs-lookup"><span data-stu-id="b4197-111">Yes.</span></span> <span data-ttu-id="b4197-112">SharePoint Online-principen gäller även tooOneDrive för företag.</span><span class="sxs-lookup"><span data-stu-id="b4197-112">A SharePoint Online policy also applies tooOneDrive for Business.</span></span>


## <a name="why-cant-i-set-a-policy-on-client-apps-like-word-or-outlook"></a><span data-ttu-id="b4197-113">Varför kan inte ange en princip på klientprogram som Word och Outlook?</span><span class="sxs-lookup"><span data-stu-id="b4197-113">Why can’t I set a policy on client apps, like Word or Outlook?</span></span>

<span data-ttu-id="b4197-114">En villkorlig åtkomstprincip anger krav för åtkomst till en tjänst.</span><span class="sxs-lookup"><span data-stu-id="b4197-114">A conditional access policy sets requirements for accessing a service.</span></span> <span data-ttu-id="b4197-115">Den tillämpas när toothat Autentiseringstjänsten inträffar.</span><span class="sxs-lookup"><span data-stu-id="b4197-115">It's enforced when authentication toothat service occurs.</span></span> <span data-ttu-id="b4197-116">hello principen anges inte direkt på ett klientprogram.</span><span class="sxs-lookup"><span data-stu-id="b4197-116">hello policy is not set directly on a client application.</span></span> <span data-ttu-id="b4197-117">I stället tillämpas när en klient anropar en tjänst.</span><span class="sxs-lookup"><span data-stu-id="b4197-117">Instead, it is applied when a client calls a service.</span></span> <span data-ttu-id="b4197-118">Till exempel gäller en princip på SharePoint tooclients anropar SharePoint.</span><span class="sxs-lookup"><span data-stu-id="b4197-118">For example, a policy set on SharePoint applies tooclients calling SharePoint.</span></span> <span data-ttu-id="b4197-119">En princip på Exchange gäller tooOutlook.</span><span class="sxs-lookup"><span data-stu-id="b4197-119">A policy set on Exchange applies tooOutlook.</span></span>

## <a name="does-a-conditional-access-policy-apply-tooservice-accounts"></a><span data-ttu-id="b4197-120">Gäller en villkorlig åtkomstprincip tooservice konton?</span><span class="sxs-lookup"><span data-stu-id="b4197-120">Does a conditional access policy apply tooservice accounts?</span></span>

<span data-ttu-id="b4197-121">Principer för villkorlig åtkomst gäller tooall användarkonton.</span><span class="sxs-lookup"><span data-stu-id="b4197-121">Conditional access policies apply tooall user accounts.</span></span> <span data-ttu-id="b4197-122">Detta innefattar användarkonton som används som tjänstkonton.</span><span class="sxs-lookup"><span data-stu-id="b4197-122">This includes user accounts that are used as service accounts.</span></span> <span data-ttu-id="b4197-123">Ofta kan inte ett tjänstkonto som kör obevakad uppfylla hello av en princip för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b4197-123">Often, a service account that runs unattended can't satisfy hello requirements of a conditional access policy.</span></span> <span data-ttu-id="b4197-124">Multifaktorautentisering kan komma att krävas.</span><span class="sxs-lookup"><span data-stu-id="b4197-124">For example, multi-factor authentication might be required.</span></span> <span data-ttu-id="b4197-125">Tjänstkonton som kan uteslutas från en princip med hjälp av villkorlig åtkomst principinställningarna för hantering.</span><span class="sxs-lookup"><span data-stu-id="b4197-125">Service accounts can be excluded from a policy by using conditional access policy management settings.</span></span> 

## <a name="are-graph-apis-available-for-configuring-conditional-access-policies"></a><span data-ttu-id="b4197-126">Finns Graph API: er för att konfigurera principer för villkorlig åtkomst?</span><span class="sxs-lookup"><span data-stu-id="b4197-126">Are Graph APIs available for configuring conditional access policies?</span></span>

<span data-ttu-id="b4197-127">För närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="b4197-127">Currently, no.</span></span> 

## <a name="what-is-hello-default-exclusion-policy-for-unsupported-device-platforms"></a><span data-ttu-id="b4197-128">Vad är hello undantag standardprincipen för plattformar för enheter som inte stöds?</span><span class="sxs-lookup"><span data-stu-id="b4197-128">What is hello default exclusion policy for unsupported device platforms?</span></span>

<span data-ttu-id="b4197-129">För närvarande kan tillämpas selektivt principer för villkorlig åtkomst för användare av iOS och Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="b4197-129">Currently, conditional access policies are selectively enforced on users of iOS and Android devices.</span></span> <span data-ttu-id="b4197-130">Program för andra enhetsplattformar, som standard inte påverkas av hello principen för villkorlig åtkomst för iOS och Android-enheter.</span><span class="sxs-lookup"><span data-stu-id="b4197-130">Applications on other device platforms are, by default, not affected by hello conditional access policy for iOS and Android devices.</span></span> <span data-ttu-id="b4197-131">En klientadministratör kan välja toooverride hello global princip toodisallow åtkomst toousers på plattformar som inte stöds.</span><span class="sxs-lookup"><span data-stu-id="b4197-131">A tenant admin can choose toooverride hello global policy toodisallow access toousers on platforms that are not supported.</span></span>


## <a name="how-do-conditional-access-policies-work-for-microsoft-teams"></a><span data-ttu-id="b4197-132">Hur fungerar principer för villkorlig åtkomst för Microsoft Teams?</span><span class="sxs-lookup"><span data-stu-id="b4197-132">How do conditional access policies work for Microsoft Teams?</span></span>  

<span data-ttu-id="b4197-133">Microsoft-Teams bygger kraftigt på Exchange Online och SharePoint Online för huvudscenarier produktivitet som möten, kalendrar och fildelning.</span><span class="sxs-lookup"><span data-stu-id="b4197-133">Microsoft Teams relies heavily on Exchange Online and SharePoint Online for core productivity scenarios, like meetings, calendars, and file sharing.</span></span> <span data-ttu-id="b4197-134">Principer för villkorlig åtkomst som ställs för dessa molnappar gäller tooMicrosoft team när en användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="b4197-134">Conditional access policies that are set for these cloud apps apply tooMicrosoft Teams when a user signs in.</span></span>

<span data-ttu-id="b4197-135">Microsoft-Teams även stöds separat som en molnapp i Azure Active Directory-principer för villkorlig åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b4197-135">Microsoft Teams also is supported separately as a cloud app in Azure Active Directory conditional access policies.</span></span> <span data-ttu-id="b4197-136">Principer för certifikat som har angetts för en molnappen gäller tooMicrosoft team när en användare loggar in.</span><span class="sxs-lookup"><span data-stu-id="b4197-136">Certificate authority policies that are set for a cloud app apply tooMicrosoft Teams when a user signs in.</span></span>

<span data-ttu-id="b4197-137">Microsoft-Teams skrivbordsklienter för Windows och Mac stöder modern autentisering.</span><span class="sxs-lookup"><span data-stu-id="b4197-137">Microsoft Teams desktop clients for Windows and Mac support modern authentication.</span></span> <span data-ttu-id="b4197-138">Modern autentisering kan logga in baserat på hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office-program för olika plattformar.</span><span class="sxs-lookup"><span data-stu-id="b4197-138">Modern authentication brings sign-in based on hello Azure Active Directory Authentication Library (ADAL) tooMicrosoft Office client applications across platforms.</span></span> 
