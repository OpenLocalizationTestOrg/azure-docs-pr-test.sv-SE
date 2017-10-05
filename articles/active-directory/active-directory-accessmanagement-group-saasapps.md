---
title: "Hantera åtkomst till SaaS-program med hjälp av en grupp | Microsoft Docs"
description: "Hur du använder grupper i Azure Active Directory Premium eller Basic du tilldelar åtkomst till SaaS-program som är integrerade med Azure Active Directory."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: ab8dee63-bedc-46ca-8852-234f5c16ae98
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/04/2017
ms.author: curtand
ms.reviewer: piotrci
ms.custom: it-pro;oldportal
ms.openlocfilehash: d350011ee9fc5ced9ddb16993f68d3c840a645a5
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="using-a-group-to-manage-access-to-saas-applications"></a><span data-ttu-id="929cb-103">Använd en grupp för att hantera åtkomst till SaaS-program</span><span class="sxs-lookup"><span data-stu-id="929cb-103">Using a group to manage access to SaaS applications</span></span>
<span data-ttu-id="929cb-104">Använder Azure Active Directory (AD Azure) med en Azure AD Premium eller Azure AD Basic-licens kan använda du grupper för att tilldela åtkomst till ett SaaS-program som är integrerad med Azure AD.</span><span class="sxs-lookup"><span data-stu-id="929cb-104">Using Azure Active Directory (Azure AD) with an Azure AD Premium or Azure AD Basic license, you can use groups to assign access to a SaaS application that's integrated with Azure AD.</span></span> <span data-ttu-id="929cb-105">Till exempel om du vill tilldela åtkomst för marknadsföringsavdelningen att använda fem olika SaaS-program kan du skapa en grupp som innehåller användarna i marknadsföringsavdelningen och sedan tilldela den gruppen för dessa fem SaaS-program som krävs av marknadsföringsavdelningen.</span><span class="sxs-lookup"><span data-stu-id="929cb-105">For example, if you want to assign access for the marketing department to use five different SaaS applications, you can create a group that contains the users in the marketing department, and then assign that group to these five SaaS applications that are needed by the marketing department.</span></span> <span data-ttu-id="929cb-106">Det här sättet kan du spara tid genom att hantera medlemskap i marknadsföringsavdelningen på ett ställe.</span><span class="sxs-lookup"><span data-stu-id="929cb-106">This way you can save time by managing the membership of the marketing department in one place.</span></span> <span data-ttu-id="929cb-107">Användare sedan tilldelas till programmet när de läggs till som medlemmar i gruppen marknadsföring och deras tilldelningar ta bort från programmet när de tas bort från gruppen marknadsföring.</span><span class="sxs-lookup"><span data-stu-id="929cb-107">Users then are assigned to the application when they are added as members of the marketing group, and have their assignments removed from the application when they are removed from the marketing group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="929cb-108">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="929cb-108">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 

<span data-ttu-id="929cb-109">Den här funktionen kan användas med hundratals program som du kan lägga till i Azure AD Application Gallery.</span><span class="sxs-lookup"><span data-stu-id="929cb-109">This capability can be used with hundreds of applications that you can add from within the Azure AD Application Gallery.</span></span>

<span data-ttu-id="929cb-110">**Du tilldelar åtkomst för en grupp till ett SaaS-program**</span><span class="sxs-lookup"><span data-stu-id="929cb-110">**To assign access for a group to a SaaS application**</span></span>

1. <span data-ttu-id="929cb-111">I den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory** på navigeringsfältet till vänster.</span><span class="sxs-lookup"><span data-stu-id="929cb-111">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory** on the navigation bar on the left hand side.</span></span>
2. <span data-ttu-id="929cb-112">Välj den **Directory** och öppna katalogen där du vill ge behörighet för en grupp till ett SaaS-program.</span><span class="sxs-lookup"><span data-stu-id="929cb-112">Select the **Directory** tab, and then open the directory in which you want to assign access for a group to a SaaS application.</span></span>
3. <span data-ttu-id="929cb-113">Välj den **program** fliken. Välj ett program som du har lagt till från galleriet program och klicka sedan på den **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="929cb-113">Select the **Applications** tab. Select an application that you added from the Application Gallery, and then click  the **Users and Groups** tab.</span></span>
4. <span data-ttu-id="929cb-114">På den **användare och grupper** fliken den **från och med** , ange namnet på gruppen som du vill bevilja åtkomst och välj sedan kryssmarkeringen i det övre högra hörnet.</span><span class="sxs-lookup"><span data-stu-id="929cb-114">On the **Users and Groups** tab, in the **Starting with** field, enter the name of the group to which you want to assign access, and then select the check mark in the upper right.</span></span> <span data-ttu-id="929cb-115">Du behöver bara ange den första delen av gruppens namn.</span><span class="sxs-lookup"><span data-stu-id="929cb-115">You only need to type the first part of a group's name.</span></span>
5. <span data-ttu-id="929cb-116">Markera gruppen och välj sedan den **tilldela åtkomst** knappen.</span><span class="sxs-lookup"><span data-stu-id="929cb-116">Select the group, then then select the **Assign Access** button.</span></span> <span data-ttu-id="929cb-117">Välj **Ja** när du ser i bekräftelsemeddelandet.</span><span class="sxs-lookup"><span data-stu-id="929cb-117">Select **Yes** when you see the confirmation message.</span></span> <span data-ttu-id="929cb-118">Kapslade gruppmedlemskap stöds inte för gruppbaserad tilldelning till program just nu.</span><span class="sxs-lookup"><span data-stu-id="929cb-118">Nested group memberships are not supported for group-based assignment to applications at this time.</span></span>
6. <span data-ttu-id="929cb-119">Du kan också se vilka användare som har tilldelats programmet, antingen direkt eller genom medlemskap i en grupp.</span><span class="sxs-lookup"><span data-stu-id="929cb-119">You can also see which users are assigned to the application, either directly or by membership in a group.</span></span> <span data-ttu-id="929cb-120">Det gör du genom att ändra den **visa listrutan från ”grupper”** till **”alla användare”**.</span><span class="sxs-lookup"><span data-stu-id="929cb-120">To do this, change the **Show dropdown from 'Groups'** to **'All Users'**.</span></span> <span data-ttu-id="929cb-121">I listan visas användare i katalogen och huruvida varje användare tilldelas till programmet.</span><span class="sxs-lookup"><span data-stu-id="929cb-121">The list shows users in the directory and whether or not each user is assigned to the application.</span></span> <span data-ttu-id="929cb-122">I listan visas också om tilldelade användare som är kopplade till programmet direkt (Tilldelningstyp visas som ”Direct”) eller genom gruppmedlemskap (Tilldelningstyp visas som ärvs.)</span><span class="sxs-lookup"><span data-stu-id="929cb-122">The list also shows whether the assigned users are assigned to the application directly (assignment type shown as 'Direct'), or by virtue of group membership (assignment type shown as 'Inherited.')</span></span>

> [!NOTE]
> <span data-ttu-id="929cb-123">Du kan se fliken användare och grupper bara när du har aktiverat Azure AD Premium eller Azure AD Basic.</span><span class="sxs-lookup"><span data-stu-id="929cb-123">You can see the Users and Groups tab only after you have enabled Azure AD Premium or Azure AD Basic.</span></span>
>
>

### <a name="next-steps"></a><span data-ttu-id="929cb-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="929cb-124">Next steps</span></span>
<span data-ttu-id="929cb-125">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="929cb-125">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="929cb-126">Hantera åtkomst till resurser med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="929cb-126">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="929cb-127">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="929cb-127">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="929cb-128">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="929cb-128">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="929cb-129">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="929cb-129">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="929cb-130">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="929cb-130">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
