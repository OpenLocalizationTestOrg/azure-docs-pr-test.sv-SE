---
title: "Lägg till en Azure Active Directory B2B-samarbete användare till en roll | Microsoft Docs"
description: "Lägg till en gästanvändare till en roll i Azure Active Directory"
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
ms.openlocfilehash: e816349ea971c997f655b4d51672dba666bc3e89
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="grant-permissions-to-users-from-partner-organizations-in-your-azure-active-directory-tenant"></a><span data-ttu-id="98664-103">Bevilja behörigheter till användare från partnerorganisationer i Azure Active Directory-klient</span><span class="sxs-lookup"><span data-stu-id="98664-103">Grant permissions to users from partner organizations in your Azure Active Directory tenant</span></span>

<span data-ttu-id="98664-104">Azure Active Directory (AD Azure) B2B-samarbete användare läggs till som gästanvändare i katalogen och gästbehörigheter i katalogen begränsas som standard.</span><span class="sxs-lookup"><span data-stu-id="98664-104">Azure Active Directory (Azure AD) B2B collaboration users are added as guest users to the directory, and guest permissions in the directory are restricted by default.</span></span> <span data-ttu-id="98664-105">Ditt företag kanske måste vissa gästanvändare att fylla högre behörighet roller i din organisation.</span><span class="sxs-lookup"><span data-stu-id="98664-105">Your business may need some guest users to fill higher-privilege roles in your organization.</span></span> <span data-ttu-id="98664-106">Gästanvändare kan läggas till några roller som du önskar, baserat på din organisations behov för att stödja definiera roller för högre behörighet.</span><span class="sxs-lookup"><span data-stu-id="98664-106">To support defining higher-privilege roles, guest users can be added to any roles you desire, based on your organization's needs.</span></span>

## <a name="default-role"></a><span data-ttu-id="98664-107">standardroll</span><span class="sxs-lookup"><span data-stu-id="98664-107">Default role</span></span>

![standardroll](./media/active-directory-b2b-add-guest-to-role/default-role.png)

## <a name="global-administrator-role"></a><span data-ttu-id="98664-109">Rollen Global administratör</span><span class="sxs-lookup"><span data-stu-id="98664-109">Global Administrator Role</span></span>

![rollen Global administratör](./media/active-directory-b2b-add-guest-to-role/global-admin-role.png)

## <a name="limited-administrator-role"></a><span data-ttu-id="98664-111">Begränsad administratörsroll</span><span class="sxs-lookup"><span data-stu-id="98664-111">Limited Administrator Role</span></span>

![begränsad administratörsroll](./media/active-directory-b2b-add-guest-to-role/limited-admin-role.png)

## <a name="next-steps"></a><span data-ttu-id="98664-113">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="98664-113">Next steps</span></span>

<span data-ttu-id="98664-114">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="98664-114">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="98664-115">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="98664-115">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="98664-116">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="98664-116">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="98664-117">Delegera B2bB samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="98664-117">Delegate B2bB collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="98664-118">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="98664-118">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="98664-119">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="98664-119">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="98664-120">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="98664-120">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="98664-121">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="98664-121">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="98664-122">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="98664-122">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="98664-123">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="98664-123">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="98664-124">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="98664-124">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
