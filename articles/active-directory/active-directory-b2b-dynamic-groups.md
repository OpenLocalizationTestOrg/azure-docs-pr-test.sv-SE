---
title: aaaDynamic grupper och Azure Active Directory B2B-samarbete | Microsoft Docs
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
ms.openlocfilehash: b011298de5fd2c851c6d9caaf5c2b257807ef0a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dynamic-groups-and-azure-active-directory-b2b-collaboration"></a><span data-ttu-id="33e16-103">Dynamiska grupper och Azure Active Directory B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="33e16-103">Dynamic groups and Azure Active Directory B2B collaboration</span></span>

## <a name="what-are-dynamic-groups"></a><span data-ttu-id="33e16-104">Vad är dynamiska grupper?</span><span class="sxs-lookup"><span data-stu-id="33e16-104">What are dynamic groups?</span></span>
<span data-ttu-id="33e16-105">Dynamisk konfiguration av medlemskap i säkerhetsgrupper för Azure Active Directory (Azure AD) finns i [hello Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="33e16-105">Dynamic configuration of security group membership for Azure Active Directory (Azure AD) is available in [hello Azure portal](https://portal.azure.com).</span></span> <span data-ttu-id="33e16-106">Administratörer kan ange regler toopopulate grupper som har skapats i Azure Active Directory baserat på användarattribut (till exempel userType, avdelning eller land).</span><span class="sxs-lookup"><span data-stu-id="33e16-106">Administrators can set rules toopopulate groups that are created in Azure Active Directory based on user attributes (such as userType, department, or country).</span></span> <span data-ttu-id="33e16-107">Medlemmar kan läggas till automatiskt tooor tas bort från en säkerhetsgrupp baserat på deras attribut.</span><span class="sxs-lookup"><span data-stu-id="33e16-107">Members can be automatically added tooor removed from a security group based on their attributes.</span></span> <span data-ttu-id="33e16-108">Dessa grupper kan ge åtkomst tooapplications eller molnbaserade resurser (SharePoint-webbplatser, dokument) och tooassign licenser toomembers.</span><span class="sxs-lookup"><span data-stu-id="33e16-108">These groups can provide access tooapplications or cloud resources (SharePoint sites, documents) and tooassign licenses toomembers.</span></span> <span data-ttu-id="33e16-109">Läs mer om dynamiska grupper i [särskilda grupper i Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span><span class="sxs-lookup"><span data-stu-id="33e16-109">Read more about dynamic groups in [Dedicated groups in Azure Active Directory](active-directory-accessmanagement-dedicated-groups.md).</span></span>

<span data-ttu-id="33e16-110">hello lämpliga [licensiering Azure AD Premium P1 eller P2](https://azure.microsoft.com/pricing/details/active-directory/) är nödvändiga toocreate och Använd dynamiska grupper.</span><span class="sxs-lookup"><span data-stu-id="33e16-110">hello appropriate [Azure AD Premium P1 or P2 licensing](https://azure.microsoft.com/pricing/details/active-directory/) is required toocreate and use dynamic groups.</span></span> <span data-ttu-id="33e16-111">Läs mer i hello artikel [skapa attributbaserade regler för dynamiska gruppmedlemskap i Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span><span class="sxs-lookup"><span data-stu-id="33e16-111">Learn more in hello article [Create attribute-based rules for dynamic group membership in Azure Active Directory](active-directory-groups-dynamic-membership-azure-portal.md).</span></span>

## <a name="what-are-hello-built-in-dynamic-groups"></a><span data-ttu-id="33e16-112">Vad är hello inbyggda dynamiska grupper?</span><span class="sxs-lookup"><span data-stu-id="33e16-112">What are hello built-in dynamic groups?</span></span>
<span data-ttu-id="33e16-113">Hej **alla användare** dynamisk grupp gör det möjligt för innehavare administratörer toocreate klickar du på en grupp som innehåller alla användare i hello-klient med en enda.</span><span class="sxs-lookup"><span data-stu-id="33e16-113">hello **All users** dynamic group enables tenant admins toocreate a group containing all users in hello tenant with a single click.</span></span> <span data-ttu-id="33e16-114">Som standard hello **alla användare** gruppen innehåller alla användare i hello-katalogen, inklusive medlemmar och gäster.</span><span class="sxs-lookup"><span data-stu-id="33e16-114">By default, hello **All users** group includes all users in hello directory, including Members and Guests.</span></span>
<span data-ttu-id="33e16-115">Hello nya Azure Active Directory-administratörsportalen, kan du välja tooenable hello **alla användare** i hello gruppinställningar visa.</span><span class="sxs-lookup"><span data-stu-id="33e16-115">Within hello new Azure Active Directory admin portal, you can choose tooenable hello **All users** group in hello Group Settings view.</span></span>

![inbyggda grupper](media/active-directory-b2b-dynamic-groups/built-in-groups.png)

## <a name="hardening-hello-all-users-dynamic-group"></a><span data-ttu-id="33e16-117">Härdning hello dynamiska gruppen för alla användare</span><span class="sxs-lookup"><span data-stu-id="33e16-117">Hardening hello All users dynamic group</span></span>
<span data-ttu-id="33e16-118">Som standard hello **alla användare** innehåller gruppen användare samt dina B2B-samarbete (Gäst).</span><span class="sxs-lookup"><span data-stu-id="33e16-118">By default, hello **All users** group contains your B2B collaboration (guest) users as well.</span></span> <span data-ttu-id="33e16-119">Du kan ytterligare skydda din **alla användare** gruppen med hjälp av en regel tooremove gästanvändare.</span><span class="sxs-lookup"><span data-stu-id="33e16-119">You can further secure your **All users** group by using a rule tooremove guest users.</span></span> <span data-ttu-id="33e16-120">hello följande bild visar hello **alla användare** gruppen ändras tooexclude gäster.</span><span class="sxs-lookup"><span data-stu-id="33e16-120">hello following illustration shows hello **All users** group modified tooexclude guests.</span></span>

![Aktivera gruppen för alla användare](media/active-directory-b2b-dynamic-groups/enable-all-users-group.png)

<span data-ttu-id="33e16-122">Det kan också vara användbara toocreate en dynamisk grupp som innehåller endast gästanvändare, så att du kan tillämpa principer (till exempel Azure AD villkorliga åtkomstprinciper) toothem.</span><span class="sxs-lookup"><span data-stu-id="33e16-122">You might also find it useful toocreate a new dynamic group that contains only guest users, so that you can apply policies (such as Azure AD Conditional Access policies) toothem.</span></span>
<span data-ttu-id="33e16-123">Hur sådan grupp kan se ut:</span><span class="sxs-lookup"><span data-stu-id="33e16-123">What such a group might look like:</span></span>

![Exkludera gästanvändare](media/active-directory-b2b-dynamic-groups/exclude-guest-users.png)

## <a name="next-steps"></a><span data-ttu-id="33e16-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="33e16-125">Next steps</span></span>

<span data-ttu-id="33e16-126">Läs andra artiklar om Azure AD B2B-samarbete:</span><span class="sxs-lookup"><span data-stu-id="33e16-126">Browse our other articles on Azure AD B2B collaboration:</span></span>

* [<span data-ttu-id="33e16-127">Vad är Azure AD B2B-samarbete?</span><span class="sxs-lookup"><span data-stu-id="33e16-127">What is Azure AD B2B collaboration?</span></span>](active-directory-b2b-what-is-azure-ad-b2b.md)
* [<span data-ttu-id="33e16-128">Egenskaper för användare av B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="33e16-128">B2B collaboration user properties</span></span>](active-directory-b2b-user-properties.md)
* [<span data-ttu-id="33e16-129">Lägga till en B2B-samarbete tooa användarroll</span><span class="sxs-lookup"><span data-stu-id="33e16-129">Adding a B2B collaboration user tooa role</span></span>](active-directory-b2b-add-guest-to-role.md)
* [<span data-ttu-id="33e16-130">Delegera B2B-samarbete inbjudningar</span><span class="sxs-lookup"><span data-stu-id="33e16-130">Delegate B2B collaboration invitations</span></span>](active-directory-b2b-delegate-invitations.md)
* [<span data-ttu-id="33e16-131">B2B-samarbete kod och PowerShell-exempel</span><span class="sxs-lookup"><span data-stu-id="33e16-131">B2B collaboration code and PowerShell samples</span></span>](active-directory-b2b-code-samples.md)
* [<span data-ttu-id="33e16-132">Konfigurera SaaS-appar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="33e16-132">Configure SaaS apps for B2B collaboration</span></span>](active-directory-b2b-configure-saas-apps.md)
* [<span data-ttu-id="33e16-133">Användartoken för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="33e16-133">B2B collaboration user tokens</span></span>](active-directory-b2b-user-token.md)
* [<span data-ttu-id="33e16-134">B2B-samarbete användaranspråk mappning</span><span class="sxs-lookup"><span data-stu-id="33e16-134">B2B collaboration user claims mapping</span></span>](active-directory-b2b-claims-mapping.md)
* [<span data-ttu-id="33e16-135">Extern delning av Office 365</span><span class="sxs-lookup"><span data-stu-id="33e16-135">Office 365 external sharing</span></span>](active-directory-b2b-o365-external-user.md)
* [<span data-ttu-id="33e16-136">Aktuella begränsningar för B2B-samarbete</span><span class="sxs-lookup"><span data-stu-id="33e16-136">B2B collaboration current limitations</span></span>](active-directory-b2b-current-limitations.md)
