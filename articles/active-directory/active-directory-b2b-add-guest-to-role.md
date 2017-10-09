---
title: "aaaAdd ett Azure Active Directory B2B-samarbete användarrollen tooa | Microsoft Docs"
description: "Lägg till en roll tooa gäst i Azure Active Directory"
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
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ccc58a0c8ecc73f8e79a8d827efdc0ff93846a96
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="grant-permissions-toousers-from-partner-organizations-in-your-azure-active-directory-tenant"></a><span data-ttu-id="ff1fc-103">Bevilja behörigheter toousers från partnerorganisationer i Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="ff1fc-103">Grant permissions toousers from partner organizations in your Azure Active Directory tenant</span></span>

<span data-ttu-id="ff1fc-104">Azure Active Directory (AD Azure) B2B-samarbete användare läggs till som gäst användare toohello directory och gästbehörigheter i hello directory begränsas som standard.</span><span class="sxs-lookup"><span data-stu-id="ff1fc-104">Azure Active Directory (Azure AD) B2B collaboration users are added as guest users toohello directory, and guest permissions in hello directory are restricted by default.</span></span> <span data-ttu-id="ff1fc-105">Ditt företag kanske måste vissa gäst användare toofill högre behörighet roller i din organisation.</span><span class="sxs-lookup"><span data-stu-id="ff1fc-105">Your business may need some guest users toofill higher-privilege roles in your organization.</span></span> <span data-ttu-id="ff1fc-106">toosupport definiera högre behörighet roller gästanvändare kan vara tillagda tooany roller som du önskar, baserat på organisationens behov.</span><span class="sxs-lookup"><span data-stu-id="ff1fc-106">toosupport defining higher-privilege roles, guest users can be added tooany roles you desire, based on your organization's needs.</span></span>

## <a name="default-role"></a><span data-ttu-id="ff1fc-107">standardroll</span><span class="sxs-lookup"><span data-stu-id="ff1fc-107">Default role</span></span>

![standardroll](./media/active-directory-b2b-add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a><span data-ttu-id="ff1fc-109">Rollen Global administratör</span><span class="sxs-lookup"><span data-stu-id="ff1fc-109">Global Administrator Role</span></span>

![rollen Global administratör](./media/active-directory-b2b-add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a><span data-ttu-id="ff1fc-111">Begränsad administratörsroll</span><span class="sxs-lookup"><span data-stu-id="ff1fc-111">Limited Administrator Role</span></span>

![begränsad administratörsroll](./media/active-directory-b2b-add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a><span data-ttu-id="ff1fc-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff1fc-113">Next steps</span></span>

<span data-ttu-id="ff1fc-114">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="ff1fc-114">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="ff1fc-115">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="ff1fc-115">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="ff1fc-116">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="ff1fc-116">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="ff1fc-117">Delegera B2bB samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="ff1fc-117">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="ff1fc-118">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="ff1fc-118">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="ff1fc-119">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="ff1fc-119">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="ff1fc-120">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="ff1fc-120">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="ff1fc-121">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="ff1fc-121">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="ff1fc-122">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="ff1fc-122">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="ff1fc-123">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="ff1fc-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="ff1fc-124">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="ff1fc-124">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
