---
title: Dynamiska grupper och Azure Active Directory B2B-samarbete | Microsoft Docs
description: "Azure Active Directory B2B-samarbete kan användas med dynamiska grupper i Azure AD"
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
ms.date: 06/27/2017
ms.author: curtand
ms.reviewer: sasubram
ms.openlocfilehash: 5818c41610c8c5df89abcb0dcd058bcbe9579ce7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="7248d-103">Dynamiska grupper och Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="7248d-103">Dynamic groups and Azure Active Directory B2B collaboration</span></span>

## <a name="what-are-dynamic-groups"></a><span data-ttu-id="7248d-104">Vad är dynamiska grupper?</span><span class="sxs-lookup"><span data-stu-id="7248d-104">What are dynamic groups?</span></span>
<span data-ttu-id="7248d-105">Dynamisk konfiguration av medlemskap i säkerhetsgrupper för Azure Active Directory (Azure AD) finns i [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="7248d-105">Dynamic configuration of security group membership for Azure Active Directory (Azure AD) is available in [the Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="7248d-106">Administratörer kan ange regler för att fylla i grupper som har skapats i Azure Active Directory baserat på användarattribut (till exempel userType, avdelning eller land).</span><span class="sxs-lookup"><span data-stu-id="7248d-106">Administrators can set rules to populate groups that are created in Azure Active Directory based on user attributes (such as userType, department, or country).</span></span> <span data-ttu-id="7248d-107">Medlemmar kan automatiskt läggas till eller tas bort från en säkerhetsgrupp baserat på deras attribut.</span><span class="sxs-lookup"><span data-stu-id="7248d-107">Members can be automatically added to or removed from a security group based on their attributes.</span></span> <span data-ttu-id="7248d-108">Dessa grupper kan ge åtkomst till program eller molnresurser (SharePoint-webbplatser, dokument) och tilldela licenser till medlemmar.</span><span class="sxs-lookup"><span data-stu-id="7248d-108">These groups can provide access to applications or cloud resources (SharePoint sites, documents) and to assign licenses to members.</span></span> <span data-ttu-id="7248d-109">Läs mer om dynamiska grupper i [särskilda grupper i Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span><span class="sxs-lookup"><span data-stu-id="7248d-109">Read more about dynamic groups in [Dedicated groups in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span></span>

<span data-ttu-id="7248d-110">Rätt [licensiering Azure AD Premium P1 eller P2](https://azure.microsoft.com/pricing/details/active-directory/) krävs för att skapa och använda dynamiska grupper.</span><span class="sxs-lookup"><span data-stu-id="7248d-110">The appropriate [Azure AD Premium P1 or P2 licensing](https://azure.microsoft.com/pricing/details/active-directory/) is required to create and use dynamic groups.</span></span> <span data-ttu-id="7248d-111">Läs mer i artikeln [skapa attributbaserade regler för dynamiska gruppmedlemskap i Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7248d-111">Learn more in the article [Create attribute-based rules for dynamic group membership in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

## <a name="what-are-the-built-in-dynamic-groups"></a><span data-ttu-id="7248d-112">Vad är de inbyggda dynamiska grupperna?</span><span class="sxs-lookup"><span data-stu-id="7248d-112">What are the built-in dynamic groups?</span></span>
<span data-ttu-id="7248d-113">Den **alla användare** dynamisk grupp gör innehavaradministratörer att skapa en grupp som innehåller alla användare i klienten med en enda klickning.</span><span class="sxs-lookup"><span data-stu-id="7248d-113">The **All users** dynamic group enables tenant admins to create a group containing all users in the tenant with a single click.</span></span> <span data-ttu-id="7248d-114">Som standard den **alla användare** gruppen innehåller alla användare i katalogen, inklusive medlemmar och gäster.</span><span class="sxs-lookup"><span data-stu-id="7248d-114">By default, the **All users** group includes all users in the directory, including Members and Guests.</span></span>
<span data-ttu-id="7248d-115">I den nya Azure Active Directory-administratörsportalen kan du välja att aktivera den **alla användare** i vyn inställningar.</span><span class="sxs-lookup"><span data-stu-id="7248d-115">Within the new Azure Active Directory admin portal, you can choose to enable the **All users** group in the Group Settings view.</span></span>

![inbyggda grupper](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-the-all-users-dynamic-group"></a><span data-ttu-id="7248d-117">Härdning av den dynamiska gruppen som alla användare</span><span class="sxs-lookup"><span data-stu-id="7248d-117">Hardening the All users dynamic group</span></span>
<span data-ttu-id="7248d-118">Som standard den **alla användare** innehåller gruppen användare samt dina B2B-samarbete (Gäst).</span><span class="sxs-lookup"><span data-stu-id="7248d-118">By default, the **All users** group contains your B2B collaboration (guest) users as well.</span></span> <span data-ttu-id="7248d-119">Du kan ytterligare skydda din **alla användare** gruppen med hjälp av en regel för att ta bort gästanvändare.</span><span class="sxs-lookup"><span data-stu-id="7248d-119">You can further secure your **All users** group by using a rule to remove guest users.</span></span> <span data-ttu-id="7248d-120">Följande bild visar den **alla användare** gruppen ändras för att utesluta gäster.</span><span class="sxs-lookup"><span data-stu-id="7248d-120">The following illustration shows the **All users** group modified to exclude guests.</span></span>

![Aktivera gruppen för alla användare](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

<span data-ttu-id="7248d-122">Du kan också vara bra att skapa en ny dynamisk grupp som innehåller endast gästanvändare, så att du kan tillämpa principer (till exempel Azure AD villkorliga åtkomstprinciper) till dem.</span><span class="sxs-lookup"><span data-stu-id="7248d-122">You might also find it useful to create a new dynamic group that contains only guest users, so that you can apply policies (such as Azure AD Conditional Access policies) to them.</span></span>
<span data-ttu-id="7248d-123">Hur sådan grupp kan se ut:</span><span class="sxs-lookup"><span data-stu-id="7248d-123">What such a group might look like:</span></span>

![Exkludera gästanvändare](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a><span data-ttu-id="7248d-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7248d-125">Next steps</span></span>

<span data-ttu-id="7248d-126">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="7248d-126">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="7248d-127">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="7248d-127">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="7248d-128">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="7248d-128">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="7248d-129">Lägger till en B2B-samarbete användare till en roll</span><span class="sxs-lookup"><span data-stu-id="7248d-129">Adding a B2B collaboration user to a role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="7248d-130">Delegera B2B-samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="7248d-130">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="7248d-131">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="7248d-131">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="7248d-132">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="7248d-132">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="7248d-133">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="7248d-133">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="7248d-134">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="7248d-134">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="7248d-135">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="7248d-135">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="7248d-136">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="7248d-136">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
