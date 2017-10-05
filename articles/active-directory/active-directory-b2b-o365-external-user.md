---
title: Extern delning av Office 365 och Azure Active Directory B2B-samarbete | Microsoft Docs
description: "anspråk mappning referens för Azure Active Directory B2B-samarbete"
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
ms.openlocfilehash: cad0ce8f745f3d6ca14436fd714b08c60de0e459
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="office-365-external-sharing-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="258ec-103">Extern delning av Office 365 och Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="258ec-103">Office 365 external sharing and Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="258ec-104">Extern delning i Office 365 (OneDrive, SharePoint Online, enhetlig grupper, etc.) och Azure Active Directory (AD Azure) B2B-samarbete är tekniskt sett samma sak.</span><span class="sxs-lookup"><span data-stu-id="258ec-104">External sharing in Office 365 (OneDrive, SharePoint Online, Unified Groups, etc.) and Azure Active Directory (Azure AD) B2B collaboration are technically the same thing.</span></span> <span data-ttu-id="258ec-105">Alla extern delning (utom OneDrive/SharePoint Online), inklusive gäster i Office 365-grupper, använder redan Azure AD B2B-samarbetsinbjudan API: er för att delas.</span><span class="sxs-lookup"><span data-stu-id="258ec-105">All external sharing (except OneDrive/SharePoint Online), including guests in Office 365 Groups, already uses the Azure AD B2B collaboration invitation APIs for sharing.</span></span>

<span data-ttu-id="258ec-106">OneDrive/SharePoint Online finns en separat inbjudan manager.</span><span class="sxs-lookup"><span data-stu-id="258ec-106">OneDrive/SharePoint Online has a separate invitation manager.</span></span> <span data-ttu-id="258ec-107">Stöd för extern delning i OneDrive/SharePoint Online igång innan Azure AD utvecklats stödet.</span><span class="sxs-lookup"><span data-stu-id="258ec-107">Support for external sharing in OneDrive/SharePoint Online started before Azure AD developed its support.</span></span> <span data-ttu-id="258ec-108">Över tiden, OneDrive/SharePoint Online extern delning har uppstått flera funktioner och många miljoner användare som använder produkten har inbyggda delning mönster.</span><span class="sxs-lookup"><span data-stu-id="258ec-108">Over time, OneDrive/SharePoint Online external sharing has accrued several features and many millions of users who use the product's in-built sharing pattern.</span></span> <span data-ttu-id="258ec-109">Det finns emellertid vissa skillnader mellan hur OneDrive/SharePoint Online extern delning fungerar och hur fungerar Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="258ec-109">However, there are some subtle differences between how OneDrive/SharePoint Online external sharing works and how Azure AD B2B collaboration works:</span></span>

- <span data-ttu-id="258ec-110">OneDrive/SharePoint Online lägger till användare i katalogen när användare har lösts in deras inbjudan.</span><span class="sxs-lookup"><span data-stu-id="258ec-110">OneDrive/SharePoint Online adds users to the directory after users have redeemed their invitations.</span></span> <span data-ttu-id="258ec-111">Så innan åtgärd visas användaren i Azure AD-portalen.</span><span class="sxs-lookup"><span data-stu-id="258ec-111">So, before redemption, you don't see the user in Azure AD portal.</span></span> <span data-ttu-id="258ec-112">Om en användare inbjuder under tiden i en annan plats, skapas en ny inbjudan.</span><span class="sxs-lookup"><span data-stu-id="258ec-112">If another site invites a user in the meantime, a new invitation is generated.</span></span> <span data-ttu-id="258ec-113">Men när du använder Azure AD B2B-samarbete läggs användare omedelbart på inbjudan så att de visas överallt.</span><span class="sxs-lookup"><span data-stu-id="258ec-113">However, when you use Azure AD B2B collaboration, users are added immediately on invitation so that they show up everywhere.</span></span>

- <span data-ttu-id="258ec-114">Inlösning upplevelse i OneDrive/SharePoint Online ser ut upplevelse i Azure AD B2B-samarbete.</span><span class="sxs-lookup"><span data-stu-id="258ec-114">The redemption experience in OneDrive/SharePoint Online looks different from the experience in Azure AD B2B collaboration.</span></span> <span data-ttu-id="258ec-115">När en användare redeems en inbjudan, upplevelser ser likadana ut.</span><span class="sxs-lookup"><span data-stu-id="258ec-115">After a user redeems an invitation, the experiences look alike.</span></span>

- <span data-ttu-id="258ec-116">Azure AD B2B collaboration uppmanas användare kan nå från OneDrive/SharePoint Online delning av dialogrutor.</span><span class="sxs-lookup"><span data-stu-id="258ec-116">Azure AD B2B collaboration invited users can be picked from OneDrive/SharePoint Online sharing dialog boxes.</span></span> <span data-ttu-id="258ec-117">OneDrive/SharePoint Online uppmanas användare visas också i Azure AD när de lösa deras inbjudan.</span><span class="sxs-lookup"><span data-stu-id="258ec-117">OneDrive/SharePoint Online invited users also show up in Azure AD after they redeem their invitations.</span></span>

- <span data-ttu-id="258ec-118">För att hantera extern delning i OneDrive/SharePoint Online med Azure AD B2B-samarbete, ställa in OneDrive/SharePoint Online extern delning inställningen för att **Tillåt endast att dela med externa användare redan i katalogen**.</span><span class="sxs-lookup"><span data-stu-id="258ec-118">To manage external sharing in OneDrive/SharePoint Online with Azure AD B2B collaboration, set the OneDrive/SharePoint Online external sharing setting to **Only allow sharing with external users already in the directory**.</span></span> <span data-ttu-id="258ec-119">Användare kan gå till externt delade webbplatser och välj från externa samarbetspartners som administratören har lagts till.</span><span class="sxs-lookup"><span data-stu-id="258ec-119">Users can go to externally shared sites and pick from external collaborators that the admin has added.</span></span> <span data-ttu-id="258ec-120">Administratören kan lägga till externa samarbetspartners via B2B-samarbetsinbjudan API: er.</span><span class="sxs-lookup"><span data-stu-id="258ec-120">The admin can add the external collaborators through the B2B collaboration invitation APIs.</span></span>

![OneDrive/SharePoint Online extern delning](media/active-directory-b2b-o365-external-user/odsp-sharing-setting.png)

## <a name="next-steps"></a><span data-ttu-id="258ec-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="258ec-122">Next steps</span></span>

<span data-ttu-id="258ec-123">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="258ec-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="258ec-124">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="258ec-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="258ec-125">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="258ec-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="258ec-126">Lägger till en B2B-samarbete användare till en roll</span><span class="sxs-lookup"><span data-stu-id="258ec-126">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="258ec-127">Delegera B2B-samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="258ec-127">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="258ec-128">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="258ec-128">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="258ec-129">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="258ec-129">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="258ec-130">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="258ec-130">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="258ec-131">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="258ec-131">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="258ec-132">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="258ec-132">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="258ec-133">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="258ec-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
