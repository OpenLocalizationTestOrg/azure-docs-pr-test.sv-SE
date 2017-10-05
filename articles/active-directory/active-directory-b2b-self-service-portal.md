---
title: "Registrering självbetjäningsportalen för Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete ger stöd för dina företagsomfattande relationer genom att tilldela affärspartner selektiv åtkomst till dina affärsprogram"
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
ms.openlocfilehash: 307373c75bbb87cec683f7a3097f8f159c9d5e61
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="self-service-portal-for-azure-ad-b2b-collaboration-sign-up"></a><span data-ttu-id="dc018-103">Självbetjäningsportalen för Azure AD B2B-samarbete registrering</span><span class="sxs-lookup"><span data-stu-id="dc018-103">Self-service portal for Azure AD B2B collaboration sign-up</span></span>

<span data-ttu-id="dc018-104">Kunder kan göra mycket med inbyggda funktioner som är tillgängliga via vår IT-administratören [Azure-portalen](https://portal.azure.com) och vår [programmet åtkomstpanelen](https://myapps.microsoft.com) för slutanvändare.</span><span class="sxs-lookup"><span data-stu-id="dc018-104">Customers can do a lot with the built-in features that are exposed through our IT admin [Azure portal](https://portal.azure.com) and our [Application Access Panel](https://myapps.microsoft.com) for end users.</span></span> <span data-ttu-id="dc018-105">Men det finns också företag måste anpassa onboarding arbetsflödet för B2B-användare efter organisationens behov.</span><span class="sxs-lookup"><span data-stu-id="dc018-105">But we are also aware that businesses need to customize the onboarding workflow for B2B users to fit their organization’s needs.</span></span> <span data-ttu-id="dc018-106">De kan göra det med [vårt API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span><span class="sxs-lookup"><span data-stu-id="dc018-106">They can do that with [our API](https://developer.microsoft.com/graph/docs/api-reference/v1.0/resources/invitation).</span></span>

<span data-ttu-id="dc018-107">I diskussioner med våra kunder finns en gemensam måste öka upp över alla andra.</span><span class="sxs-lookup"><span data-stu-id="dc018-107">In discussions with our customers, we see one common need rise up above all others.</span></span> <span data-ttu-id="dc018-108">Bjuda in organisationen kanske inte vet i förväg som enskilda externa samarbetspartners som behöver åtkomst till sina resurser.</span><span class="sxs-lookup"><span data-stu-id="dc018-108">The inviting organization may not know ahead of time who the individual external collaborators are who need access to their resources.</span></span> <span data-ttu-id="dc018-109">De vill ha ett sätt för användare från partnerföretag registrera sig själva med en uppsättning principer som styr bjuda in organisationen.</span><span class="sxs-lookup"><span data-stu-id="dc018-109">They wanted a way for users from partner companies to  sign themselves up with a set of policies that the inviting organization controls.</span></span> <span data-ttu-id="dc018-110">Det här scenariot är möjligt via våra API: er, så vi har publicerat ett projekt på Github som har just: [Github exempelprojektet](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span><span class="sxs-lookup"><span data-stu-id="dc018-110">This scenario is possible through our APIs,  so we published a project on Github that did just that: [sample Github project](https://github.com/Azure/active-directory-dotnet-graphapi-b2bportal-web).</span></span>

<span data-ttu-id="dc018-111">Vår Github-projektet visar hur organisationer kan använda våra API: er och funktioner för en principbaserad, självbetjäning registrering för deras betrodda partners med regler som styr appar som de kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="dc018-111">Our Github project demonstrates how organizations can use our APIs, and provide a policy-based, self-service sign-up capability for their trusted partners, with rules that determine the apps they can access.</span></span> <span data-ttu-id="dc018-112">Partneranvändarna kan få åtkomst till resurser när de behöver dem, på ett säkert sätt, utan att bjuda in organisationen för att manuellt publicera de.</span><span class="sxs-lookup"><span data-stu-id="dc018-112">Partner users can get access to resources when they need them, securely, without requiring the inviting organization to manually onboard them.</span></span> <span data-ttu-id="dc018-113">Du kan enkelt distribuera projektet till en Azure-prenumeration önskat.</span><span class="sxs-lookup"><span data-stu-id="dc018-113">You can easily deploy the project into an Azure subscription of your choice.</span></span>

## <a name="as-is-code"></a><span data-ttu-id="dc018-114">Som-kod</span><span class="sxs-lookup"><span data-stu-id="dc018-114">As-is code</span></span>

<span data-ttu-id="dc018-115">Kom ihåg att den här koden görs tillgänglig som ett exempel för att demonstrera användning av Azure Active Directory B2B-inbjudan API.</span><span class="sxs-lookup"><span data-stu-id="dc018-115">Remember that this code is made available as a sample to demonstrate usage of the Azure Active Directory B2B invitation API.</span></span> <span data-ttu-id="dc018-116">Det bör anpassas efter dev-team eller en partner och bör granskas innan de kan distribueras i ett produktionsscenario.</span><span class="sxs-lookup"><span data-stu-id="dc018-116">It should be customized by your dev team or a partner, and should be reviewed before being deployed in a production scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="dc018-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc018-117">Next steps</span></span>

<span data-ttu-id="dc018-118">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="dc018-118">Browse our other articles on Azure AD B2B collaboration:</span></span>
* [<span data-ttu-id="dc018-119">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="dc018-119">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="dc018-120">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="dc018-120">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="dc018-121">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="dc018-121">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="dc018-122">Elementen i e-postinbjudan B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="dc018-122">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="dc018-123">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="dc018-123">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="dc018-124">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="dc018-124">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="dc018-125">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="dc018-125">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="dc018-126">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="dc018-126">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="dc018-127">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="dc018-127">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="dc018-128">Lägg till B2B-samarbete användare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="dc018-128">Add B2B collaboration users without an invitation</span></span>](active-directory-b2b-add-user-without-invite.md)
* [<span data-ttu-id="dc018-129">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc018-129">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)