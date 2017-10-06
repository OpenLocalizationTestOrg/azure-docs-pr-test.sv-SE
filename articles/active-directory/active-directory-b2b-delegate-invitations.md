---
title: "aaaDelegate inbjudningar för Azure Active Directory B2B-samarbete | Microsoft Docs"
description: "Azure Active Directory B2B-samarbete användaregenskaper kan konfigureras"
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
ms.date: 05/23/2017
ms.author: sasubram
ms.openlocfilehash: c0122d6f60d494c6e251c41d947dc254ea887620
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="9e394-103">Delegera inbjudningar för Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="9e394-103">Delegate invitations for Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="9e394-104">Med Azure Active Directory (AD Azure) business-to-business (B2B) samarbete har du inte toobe en global administratör toosend inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="9e394-104">With Azure Active Directory (Azure AD) business-to-business (B2B) collaboration, you do not have toobe a global admin toosend invitations.</span></span> <span data-ttu-id="9e394-105">Du kan i stället använda principer och delegera inbjudningar toousers vars roller att de toosend inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="9e394-105">Instead, you can use policies and delegate invitations toousers whose roles allow them toosend invitations.</span></span> <span data-ttu-id="9e394-106">Ett viktigt nytt sätt toodelegate gästen användaren inbjudningar är via hello gäst bjuder in roll.</span><span class="sxs-lookup"><span data-stu-id="9e394-106">An important new way toodelegate guest user invitations is through hello Guest Inviter role.</span></span>

## <a name="guest-inviter-role"></a><span data-ttu-id="9e394-107">Gästen bjuder in roll</span><span class="sxs-lookup"><span data-stu-id="9e394-107">Guest Inviter role</span></span>
<span data-ttu-id="9e394-108">Vi kan tilldela hello användaren tooGuest bjuder in rollen toosend inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="9e394-108">We can assign hello user tooGuest Inviter role toosend invitations.</span></span> <span data-ttu-id="9e394-109">Du har inte toobe tillhör hello global administratör rollen toosend inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="9e394-109">You don't have toobe member of hello global admin role toosend invitations.</span></span> <span data-ttu-id="9e394-110">Som standard anropa vanliga användare hello inbjudan API en global administratör inaktiverat inbjudningar för vanliga användare.</span><span class="sxs-lookup"><span data-stu-id="9e394-110">By default, regular users can also invoke hello invite API unless a global admin disabled invitations for regular users.</span></span> <span data-ttu-id="9e394-111">En användare kan även anropa hello API: et med hello Azure-portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="9e394-111">A user can also invoke hello API using hello Azure portal or PowerShell.</span></span>

<span data-ttu-id="9e394-112">Här är ett exempel som visar hur toouse PowerShell tooadd en användarroll toohello gäst bjuder in:</span><span class="sxs-lookup"><span data-stu-id="9e394-112">Here's an example that shows how toouse PowerShell tooadd a user toohello Guest Inviter role:</span></span>

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a><span data-ttu-id="9e394-113">Kontrollera vem som kan bjuda in</span><span class="sxs-lookup"><span data-stu-id="9e394-113">Control who can invite</span></span>

![Kontrollera hur tooinvite](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

<span data-ttu-id="9e394-115">En klientadministratör kan ange hello efter inbjudan principer med Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="9e394-115">With Azure AD B2B collaboration, a tenant admin can set hello following invitation policies:</span></span>

- <span data-ttu-id="9e394-116">Inaktivera inbjudningar</span><span class="sxs-lookup"><span data-stu-id="9e394-116">Turn off invitations</span></span>
- <span data-ttu-id="9e394-117">Bara administratörer och användare i hello gäst bjuder in roll kan bjuda in</span><span class="sxs-lookup"><span data-stu-id="9e394-117">Only admins and users in hello Guest Inviter role can invite</span></span>
- <span data-ttu-id="9e394-118">Administratörer, hello gäst bjuder in roll och medlemmar kan bjuda in</span><span class="sxs-lookup"><span data-stu-id="9e394-118">Admins, hello Guest Inviter role, and members can invite</span></span>
- <span data-ttu-id="9e394-119">Alla användare, inklusive gäster, kan bjuda in</span><span class="sxs-lookup"><span data-stu-id="9e394-119">All users, including guests, can invite</span></span>

<span data-ttu-id="9e394-120">Klienter är som standard för #4.</span><span class="sxs-lookup"><span data-stu-id="9e394-120">By default, tenants are set too#4.</span></span> <span data-ttu-id="9e394-121">(Alla användare, inklusive gäster, erbjuda B2B-användare.)</span><span class="sxs-lookup"><span data-stu-id="9e394-121">(All users, including guests, can invite B2B users.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="9e394-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="9e394-122">Next steps</span></span>

<span data-ttu-id="9e394-123">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="9e394-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="9e394-124">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="9e394-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="9e394-125">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="9e394-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="9e394-126">Lägga till en B2B-samarbete tooa användarroll</span><span class="sxs-lookup"><span data-stu-id="9e394-126">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="9e394-127">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="9e394-127">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="9e394-128">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="9e394-128">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="9e394-129">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="9e394-129">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="9e394-130">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="9e394-130">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="9e394-131">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="9e394-131">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="9e394-132">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="9e394-132">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="9e394-133">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="9e394-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
