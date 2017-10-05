---
title: "Delegera inbjudningar för Azure Active Directory B2B-samarbete | Microsoft Docs"
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
ms.openlocfilehash: 78613cc978b585a98d235245194c02371f7f3849
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="delegate-invitations-for-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="89b01-103">Delegera inbjudningar för Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="89b01-103">Delegate invitations for Azure Active Directory B2B collaboration</span></span>

<span data-ttu-id="89b01-104">Du behöver inte vara en global administratör för att skicka inbjudningar med Azure Active Directory (AD Azure) business-to-business (B2B) samarbete.</span><span class="sxs-lookup"><span data-stu-id="89b01-104">With Azure Active Directory (Azure AD) business-to-business (B2B) collaboration, you do not have to be a global admin to send invitations.</span></span> <span data-ttu-id="89b01-105">Du kan i stället använda principer och delegera inbjudningar till användare vars roller tillåta dem att skicka inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="89b01-105">Instead, you can use policies and delegate invitations to users whose roles allow them to send invitations.</span></span> <span data-ttu-id="89b01-106">Det är ett viktigt nytt sätt att delegera gästen användaren inbjudningar via rollen gäst bjuder in.</span><span class="sxs-lookup"><span data-stu-id="89b01-106">An important new way to delegate guest user invitations is through the Guest Inviter role.</span></span>

## <a name="guest-inviter-role"></a><span data-ttu-id="89b01-107">Gästen bjuder in roll</span><span class="sxs-lookup"><span data-stu-id="89b01-107">Guest Inviter role</span></span>
<span data-ttu-id="89b01-108">Vi kan tilldela användaren rollen gäst bjuder in skicka inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="89b01-108">We can assign the user to Guest Inviter role to send invitations.</span></span> <span data-ttu-id="89b01-109">Du behöver inte vara medlem i rollen global administratör för att skicka inbjudningar.</span><span class="sxs-lookup"><span data-stu-id="89b01-109">You don't have to be member of the global admin role to send invitations.</span></span> <span data-ttu-id="89b01-110">Som standard anropa vanliga användare API för inbjudan en global administratör inaktiverat inbjudningar för vanliga användare.</span><span class="sxs-lookup"><span data-stu-id="89b01-110">By default, regular users can also invoke the invite API unless a global admin disabled invitations for regular users.</span></span> <span data-ttu-id="89b01-111">En användare kan även anropa API: et med Azure-portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="89b01-111">A user can also invoke the API using the Azure portal or PowerShell.</span></span>

<span data-ttu-id="89b01-112">Här är ett exempel som visar hur du använder PowerShell för att lägga till en användare i rollen gäst bjuder in:</span><span class="sxs-lookup"><span data-stu-id="89b01-112">Here's an example that shows how to use PowerShell to add a user to the Guest Inviter role:</span></span>

```
Add-MsolRoleMember -RoleObjectId 95e79109-95c0-4d8e-aee3-d01accf2d47b -RoleMemberEmailAddress <RoleMemberEmailAddress>
```

## <a name="control-who-can-invite"></a><span data-ttu-id="89b01-113">Kontrollera vem som kan bjuda in</span><span class="sxs-lookup"><span data-stu-id="89b01-113">Control who can invite</span></span>

![Styra hur att bjuda in](media/active-directory-b2b-delegate-invitations/control-who-to-invite.png)

<span data-ttu-id="89b01-115">En klientadministratör kan ange följande inbjudan principer med Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="89b01-115">With Azure AD B2B collaboration, a tenant admin can set the following invitation policies:</span></span>

- <span data-ttu-id="89b01-116">Inaktivera inbjudningar</span><span class="sxs-lookup"><span data-stu-id="89b01-116">Turn off invitations</span></span>
- <span data-ttu-id="89b01-117">Bara administratörer och användare med rollen gäst bjuder in bjuda in</span><span class="sxs-lookup"><span data-stu-id="89b01-117">Only admins and users in the Guest Inviter role can invite</span></span>
- <span data-ttu-id="89b01-118">Administratörer, Gäst bjuder in roll och medlemmar kan bjuda in</span><span class="sxs-lookup"><span data-stu-id="89b01-118">Admins, the Guest Inviter role, and members can invite</span></span>
- <span data-ttu-id="89b01-119">Alla användare, inklusive gäster, kan bjuda in</span><span class="sxs-lookup"><span data-stu-id="89b01-119">All users, including guests, can invite</span></span>

<span data-ttu-id="89b01-120">Klienter är som standard #4.</span><span class="sxs-lookup"><span data-stu-id="89b01-120">By default, tenants are set to #4.</span></span> <span data-ttu-id="89b01-121">(Alla användare, inklusive gäster, erbjuda B2B-användare.)</span><span class="sxs-lookup"><span data-stu-id="89b01-121">(All users, including guests, can invite B2B users.)</span></span>

## <a name="next-steps"></a><span data-ttu-id="89b01-122">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89b01-122">Next steps</span></span>

<span data-ttu-id="89b01-123">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="89b01-123">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="89b01-124">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="89b01-124">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="89b01-125">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="89b01-125">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="89b01-126">Lägger till en B2B-samarbete användare till en roll</span><span class="sxs-lookup"><span data-stu-id="89b01-126">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="89b01-127">Dynamiska grupper och B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="89b01-127">Dynamic groups and B2B collaboration</span></span>](active-directory-b2b-dynamic-groups.md)
* [<span data-ttu-id="89b01-128">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="89b01-128">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="89b01-129">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="89b01-129">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="89b01-130">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="89b01-130">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="89b01-131">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="89b01-131">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="89b01-132">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="89b01-132">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="89b01-133">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="89b01-133">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
