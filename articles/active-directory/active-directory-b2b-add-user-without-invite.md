---
title: "aaaAdd B2B-samarbete användare tooAzure Active Directory utan inbjudan | Microsoft Docs"
description: "Du kan låta en gästanvändare lägga till andra gäst användare tooyour Azure AD utan löser en inbjudan i Azure Active Directory B2B-samarbete."
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
ms.openlocfilehash: 459d99b9f856a35973d1b2cbfabdc23fe40c8f44
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="add-b2b-collaboration-guest-users-without-an-invitation"></a><span data-ttu-id="1208e-103">Lägg till B2B-samarbete gästanvändare utan inbjudan</span><span class="sxs-lookup"><span data-stu-id="1208e-103">Add B2B collaboration guest users without an invitation</span></span>

<span data-ttu-id="1208e-104">Du kan tillåta en användare, till exempel en partner representativt, tooadd användare från hello tooyour partnerorganisation utan att behöva inbjudningar toobe lösts in.</span><span class="sxs-lookup"><span data-stu-id="1208e-104">You can allow a user, such as a partner representative, tooadd users from hello partner tooyour organization without needing invitations toobe redeemed.</span></span> <span data-ttu-id="1208e-105">Alla måste du göra är att bevilja användaren uppräkningen i hello-katalog som du använder för hello partner organisationen.</span><span class="sxs-lookup"><span data-stu-id="1208e-105">All you must do is grant that user enumeration privileges in hello directory you're using for hello partner org.</span></span> 

<span data-ttu-id="1208e-106">Bevilja dessa behörighet när:</span><span class="sxs-lookup"><span data-stu-id="1208e-106">Grant these privileges when:</span></span>

1. <span data-ttu-id="1208e-107">En användare i hello värden organisation (till exempel WoodGrove) inbjuder en användare från partnerorganisationen hello (till exempel Sam@litware.com) som gäst.</span><span class="sxs-lookup"><span data-stu-id="1208e-107">A user in hello host organization (for example, WoodGrove) invites one user from hello partner organization (for example, Sam@litware.com) as Guest.</span></span>
2. <span data-ttu-id="1208e-108">Hej administratör i hello värden organisation konfigurerar principer som gör att Sam tooidentify och lägga till andra användare från partnerorganisationen hello (Litware).</span><span class="sxs-lookup"><span data-stu-id="1208e-108">hello admin in hello host organization sets up policies that allow Sam tooidentify and add other users from hello partner organization (Litware).</span></span>
3. <span data-ttu-id="1208e-109">Nu Sam kan lägga till andra användare från Litware toohello WoodGrove katalog, grupper eller program utan att behöva inbjudningar toobe lösts in.</span><span class="sxs-lookup"><span data-stu-id="1208e-109">Now Sam can add other users from Litware toohello WoodGrove directory, groups, or applications without needing invitations toobe redeemed.</span></span> <span data-ttu-id="1208e-110">Om Sam har hello lämpliga uppräkningen i Litware, sker det automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1208e-110">If Sam has hello appropriate enumeration privileges in Litware, it happens automatically.</span></span>

### <a name="next-steps"></a><span data-ttu-id="1208e-111">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1208e-111">Next steps</span></span>

<span data-ttu-id="1208e-112">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="1208e-112">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="1208e-113">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="1208e-113">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="1208e-114">Hur lägger Azure Active Directory-administratörer till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="1208e-114">How do Azure Active Directory admins add B2B collaboration users?</span></span>](active-directory-b2b-admin-add-users.md)
* [<span data-ttu-id="1208e-115">Hur lägger informationsarbetare till B2B-samarbete användare?</span><span class="sxs-lookup"><span data-stu-id="1208e-115">How do information workers add B2B collaboration users?</span></span>](active-directory-b2b-iw-add-users.md)
* [<span data-ttu-id="1208e-116">hello-element i hello e-postinbjudan för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="1208e-116">hello elements of hello B2B collaboration invitation email</span></span>](active-directory-b2b-invitation-email.md)
* [<span data-ttu-id="1208e-117">B2B-samarbete inbjudan inlösning</span><span class="sxs-lookup"><span data-stu-id="1208e-117">B2B collaboration invitation redemption</span></span>](active-directory-b2b-redemption-experience.md)
* [<span data-ttu-id="1208e-118">Azure AD B2B-samarbete och licensiering</span><span class="sxs-lookup"><span data-stu-id="1208e-118">Azure AD B2B collaboration licensing</span></span>](active-directory-b2b-licensing.md)
* [<span data-ttu-id="1208e-119">Felsökning av Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="1208e-119">Troubleshooting Azure Active Directory B2B collaboration</span></span>](active-directory-b2b-troubleshooting.md)
* [<span data-ttu-id="1208e-120">Vanliga och frågor svar om Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="1208e-120">Azure Active Directory B2B collaboration frequently asked questions (FAQ)</span></span>](active-directory-b2b-faq.md)
* [<span data-ttu-id="1208e-121">Azure Active Directory B2B-samarbete API och anpassning</span><span class="sxs-lookup"><span data-stu-id="1208e-121">Azure Active Directory B2B collaboration API and customization</span></span>](active-directory-b2b-api.md)
* [<span data-ttu-id="1208e-122">Multi-Factor Authentication för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="1208e-122">Multi-factor authentication for B2B collaboration users</span></span>](active-directory-b2b-mfa-instructions.md)
* [<span data-ttu-id="1208e-123">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="1208e-123">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)