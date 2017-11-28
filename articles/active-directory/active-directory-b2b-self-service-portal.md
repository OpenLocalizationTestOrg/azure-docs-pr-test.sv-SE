---
title: "registreringsportalen för aaaSelf-tjänsten för Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete stöder företagsomfattande relationer genom att aktivera business partners tooselectively åtkomst till företagets program"
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
ms.openlocfilehash: c78920ecf812f7efc06a8b54b1fff834c32904f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a><span data-ttu-id="bb664-103">Självbetjäningsportalen för Azure AD B2B-samarbete registrering</span><span class="sxs-lookup"><span data-stu-id="bb664-103">Self-service portal for Azure AD B2B collaboration sign-up</span></span>

<span data-ttu-id="bb664-104">Kunder kan göra mycket med hello inbyggda funktioner som är tillgängliga via vår IT-administratören [Azure-portalen](https://portal.azure.com) och vår [programmet åtkomstpanelen](https://myapps.microsoft.com) för slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="bb664-104">Customers can do a lot with hello built-in features that are exposed through our IT admin [Azure portal](https://portal.azure.com) and our [Application Access Panel](https://myapps.microsoft.com) for end users.</span></span> <span data-ttu-id="bb664-105">Men det finns också företag måste toocustomize hello onboarding arbetsflöde för B2B användare toofit organisationens behov.</span><span class="sxs-lookup"><span data-stu-id="bb664-105">But we are also aware that businesses need toocustomize hello onboarding workflow for B2B users toofit their organization’s needs.</span></span> <span data-ttu-id="bb664-106">De kan göra det med [vårt API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span><span class="sxs-lookup"><span data-stu-id="bb664-106">They can do that with [our API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span></span>

<span data-ttu-id="bb664-107">I diskussioner med våra kunder finns en gemensam måste öka upp över alla andra.</span><span class="sxs-lookup"><span data-stu-id="bb664-107">In discussions with our customers, we see one common need rise up above all others.</span></span> <span data-ttu-id="bb664-108">hello bjuda in organisation kanske inte vet i förväg som hello enskilda externa samarbetspartners är som behöver åtkomst till tootheir resurser.</span><span class="sxs-lookup"><span data-stu-id="bb664-108">hello inviting organization may not know ahead of time who hello individual external collaborators are who need access tootheir resources.</span></span> <span data-ttu-id="bb664-109">De vill ha ett sätt för användare från partnerföretag för registrera sig själva med en uppsättning principer som hello bjuda in organisation kontroller.</span><span class="sxs-lookup"><span data-stu-id="bb664-109">They wanted a way for users from partner companies too sign themselves up with a set of policies that hello inviting organization controls.</span></span> <span data-ttu-id="bb664-110">Det här scenariot är möjligt via våra API: er, så vi har publicerat ett projekt på Github som har just: [Github exempelprojektet](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="bb664-110">This scenario is possible through our APIs,  so we published a project on Github that did just that: [sample Github project](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

<span data-ttu-id="bb664-111">Vår Github-projektet visar hur organisationer kan använda våra API: er och funktioner för en principbaserad, självbetjäning registrering för deras betrodda partners med regler som bestämmer hello-appar som de kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="bb664-111">Our Github project demonstrates how organizations can use our APIs, and provide a policy-based, self-service sign-up capability for their trusted partners, with rules that determine hello apps they can access.</span></span> <span data-ttu-id="bb664-112">Partner användare får åtkomst till tooresources när de behöver dem, på ett säkert sätt, utan hello bjuda in organisation toomanually publicera dem.</span><span class="sxs-lookup"><span data-stu-id="bb664-112">Partner users can get access tooresources when they need them, securely, without requiring hello inviting organization toomanually onboard them.</span></span> <span data-ttu-id="bb664-113">Du kan enkelt distribuera hello projekt till en Azure-prenumeration önskat.</span><span class="sxs-lookup"><span data-stu-id="bb664-113">You can easily deploy hello project into an Azure subscription of your choice.</span></span>

## <a name="as-is-code"></a><span data-ttu-id="bb664-114">Som-kod</span><span class="sxs-lookup"><span data-stu-id="bb664-114">As-is code</span></span>

<span data-ttu-id="bb664-115">Kom ihåg att den här koden görs tillgänglig som ett exempel toodemonstrate hello Azure Active Directory B2B-inbjudan API.</span><span class="sxs-lookup"><span data-stu-id="bb664-115">Remember that this code is made available as a sample toodemonstrate usage of hello Azure Active Directory B2B invitation API.</span></span> <span data-ttu-id="bb664-116">Det bör anpassas efter dev-team eller en partner och bör granskas innan de kan distribueras i ett produktionsscenario.</span><span class="sxs-lookup"><span data-stu-id="bb664-116">It should be customized by your dev team or a partner, and should be reviewed before being deployed in a production scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="bb664-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bb664-117">Next steps</span></span>

<span data-ttu-id="bb664-118">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="bb664-118">Browse our other articles on Azure AD B2B collaboration:</span></span>
* [<span data-ttu-id="bb664-119">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="bb664-119">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="bb664-120">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="bb664-120">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="bb664-121">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="bb664-121">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="bb664-122">hello-element i hello e-postinbjudan för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bb664-122">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="bb664-123">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="bb664-123">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="bb664-124">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="bb664-124">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="bb664-125">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bb664-125">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="bb664-126">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bb664-126">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="bb664-127">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="bb664-127">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="bb664-128">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="bb664-128">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="bb664-129">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="bb664-129">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)