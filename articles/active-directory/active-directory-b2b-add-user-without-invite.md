---
title: "Lägga till B2B-samarbete användare i Azure Active Directory utan en inbjudan | Microsoft Docs"
description: "Du kan låta en gästanvändare lägga till andra gästanvändare i din Azure AD utan löser en inbjudan i Azure Active Directory B2B-samarbete."
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
ms.date: 03/15/2017
ms.author: sasubram
ms.openlocfilehash: 91b9477cdb679851e7d8d2942c06999a05f64e46
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a><span data-ttu-id="c5012-103">Lägg till B2B-samarbete gästanvändare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="c5012-103">Add B2B collaboration guest users without an invitation</span></span>

<span data-ttu-id="c5012-104">Du kan tillåta en användare som ett ombud för partner, lägga till användare från partnern i din organisation utan att behöva inbjudningar till lösas.</span><span class="sxs-lookup"><span data-stu-id="c5012-104">You can allow a user, such as a partner representative, to add users from the partner to your organization without needing invitations to be redeemed.</span></span> <span data-ttu-id="c5012-105">Alla måste du göra är att bevilja användaren uppräkningen i katalogen som du använder för organisationen partner.</span><span class="sxs-lookup"><span data-stu-id="c5012-105">All you must do is grant that user enumeration privileges in the directory you're using for the partner org.</span></span> 

<span data-ttu-id="c5012-106">Bevilja dessa behörighet när:</span><span class="sxs-lookup"><span data-stu-id="c5012-106">Grant these privileges when:</span></span>

1. <span data-ttu-id="c5012-107">En användare i organisationen värden (till exempel WoodGrove) inbjuder en användare från partnerorganisationen (till exempel Sam@litware.com) som gäst.</span><span class="sxs-lookup"><span data-stu-id="c5012-107">A user in the host organization (for example, WoodGrove) invites one user from the partner organization (for example, Sam@litware.com) as Guest.</span></span>
2. <span data-ttu-id="c5012-108">Administratören i organisationen värden konfigurerar principer som gör att Sam att identifiera och lägga till andra användare från partnerorganisationen (Litware).</span><span class="sxs-lookup"><span data-stu-id="c5012-108">The admin in the host organization sets up policies that allow Sam to identify and add other users from the partner organization (Litware).</span></span>
3. <span data-ttu-id="c5012-109">Nu Sam kan lägga till andra användare från Litware WoodGrove directory, grupper eller program utan att behöva inbjudningar till lösas.</span><span class="sxs-lookup"><span data-stu-id="c5012-109">Now Sam can add other users from Litware to the WoodGrove directory, groups, or applications without needing invitations to be redeemed.</span></span> <span data-ttu-id="c5012-110">Om Sam har lämplig uppräkningen privilegier i Litware, det sker automatiskt.</span><span class="sxs-lookup"><span data-stu-id="c5012-110">If Sam has the appropriate enumeration privileges in Litware, it happens automatically.</span></span>

### <a name="next-steps"></a><span data-ttu-id="c5012-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c5012-111">Next steps</span></span>

<span data-ttu-id="c5012-112">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="c5012-112">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="c5012-113">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="c5012-113">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="c5012-114">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="c5012-114">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="c5012-115">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="c5012-115">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="c5012-116">Elementen i e-postinbjudan B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="c5012-116">The elements of the B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="c5012-117">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="c5012-117">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="c5012-118">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="c5012-118">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="c5012-119">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="c5012-119">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="c5012-120">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="c5012-120">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="c5012-121">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="c5012-121">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="c5012-122">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="c5012-122">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="c5012-123">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="c5012-123">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)