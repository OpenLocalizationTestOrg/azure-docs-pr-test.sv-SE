---
title: "aaaUsing en grupp toomanage åtkomst tooSaaS program | Microsoft Docs"
description: "Hur toouse grupper i Azure Active Directory Premium eller Basic tooassign åt tooSaaS program som är integrerade med Azure Active Directory."
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
ms.openlocfilehash: f45ea4472b3d88e8ea514af3db31b4cc9ea68d58
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-a-group-toomanage-access-toosaas-applications"></a><span data-ttu-id="dc29b-103">Med hjälp av en grupp toomanage åtkomst tooSaaS program</span><span class="sxs-lookup"><span data-stu-id="dc29b-103">Using a group toomanage access tooSaaS applications</span></span>
<span data-ttu-id="dc29b-104">Du kan använda grupper tooassign åtkomst tooa SaaS-program som är integrerad med Azure AD med hjälp av Azure Active Directory (AD Azure) med en Azure AD Premium eller Azure AD Basic-licens.</span><span class="sxs-lookup"><span data-stu-id="dc29b-104">Using Azure Active Directory (Azure AD) with an Azure AD Premium or Azure AD Basic license, you can use groups tooassign access tooa SaaS application that's integrated with Azure AD.</span></span> <span data-ttu-id="dc29b-105">Till exempel om du vill tooassign för hello marknadsföring avdelning toouse fem olika SaaS-program, kan du skapa en grupp som innehåller hello användare i marknadsföringsavdelningen hello och tilldela sedan de grupp toothese fem SaaS-program som är behövs av hello marknadsföringsavdelningen.</span><span class="sxs-lookup"><span data-stu-id="dc29b-105">For example, if you want tooassign access for hello marketing department toouse five different SaaS applications, you can create a group that contains hello users in hello marketing department, and then assign that group toothese five SaaS applications that are needed by hello marketing department.</span></span> <span data-ttu-id="dc29b-106">Det här sättet du kan spara tid genom att hantera hello medlemskap hello marknadsföringsavdelningen på ett ställe.</span><span class="sxs-lookup"><span data-stu-id="dc29b-106">This way you can save time by managing hello membership of hello marketing department in one place.</span></span> <span data-ttu-id="dc29b-107">Sedan tilldelas användare toohello program när de läggs till som medlemmar i gruppen för hello marknadsföring och har deras tilldelningar tas bort från hello program när de tas bort från hello marknadsföringsgruppen.</span><span class="sxs-lookup"><span data-stu-id="dc29b-107">Users then are assigned toohello application when they are added as members of hello marketing group, and have their assignments removed from hello application when they are removed from hello marketing group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="dc29b-108">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="dc29b-108">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 

<span data-ttu-id="dc29b-109">Den här funktionen kan användas med hundratals program som du kan lägga till från inom hello Azure AD Application Gallery.</span><span class="sxs-lookup"><span data-stu-id="dc29b-109">This capability can be used with hundreds of applications that you can add from within hello Azure AD Application Gallery.</span></span>

<span data-ttu-id="dc29b-110">**tooassign åtkomst för en grupp tooa SaaS-program**</span><span class="sxs-lookup"><span data-stu-id="dc29b-110">**tooassign access for a group tooa SaaS application**</span></span>

1. <span data-ttu-id="dc29b-111">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory** hello navigeringsfältet hello vänster.</span><span class="sxs-lookup"><span data-stu-id="dc29b-111">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory** on hello navigation bar on hello left hand side.</span></span>
2. <span data-ttu-id="dc29b-112">Välj hello **Directory** fliken och sedan öppna hello katalog där du vill att tooassign åtkomst för en grupp tooa SaaS-program.</span><span class="sxs-lookup"><span data-stu-id="dc29b-112">Select hello **Directory** tab, and then open hello directory in which you want tooassign access for a group tooa SaaS application.</span></span>
3. <span data-ttu-id="dc29b-113">Välj hello **program** fliken. Välj ett program som du har lagt till från hello Application Gallery och klicka sedan på hello **användare och grupper** fliken.</span><span class="sxs-lookup"><span data-stu-id="dc29b-113">Select hello **Applications** tab. Select an application that you added from hello Application Gallery, and then click  hello **Users and Groups** tab.</span></span>
4. <span data-ttu-id="dc29b-114">På hello **användare och grupper** på fliken hello **från och med** ange hello namn hello grupp toowhich som du vill tooassign åtkomst och sedan väljer hello hello övre högra är markerat.</span><span class="sxs-lookup"><span data-stu-id="dc29b-114">On hello **Users and Groups** tab, in hello **Starting with** field, enter hello name of hello group toowhich you want tooassign access, and then select hello check mark in hello upper right.</span></span> <span data-ttu-id="dc29b-115">Du behöver bara tootype hello första delen av gruppens namn.</span><span class="sxs-lookup"><span data-stu-id="dc29b-115">You only need tootype hello first part of a group's name.</span></span>
5. <span data-ttu-id="dc29b-116">Välj hello grupp och välj hello **tilldela åtkomst** knappen.</span><span class="sxs-lookup"><span data-stu-id="dc29b-116">Select hello group, then then select hello **Assign Access** button.</span></span> <span data-ttu-id="dc29b-117">Välj **Ja** när du ser hello bekräftelsemeddelande.</span><span class="sxs-lookup"><span data-stu-id="dc29b-117">Select **Yes** when you see hello confirmation message.</span></span> <span data-ttu-id="dc29b-118">Kapslade gruppmedlemskap stöds inte för gruppbaserad tilldelning tooapplications just nu.</span><span class="sxs-lookup"><span data-stu-id="dc29b-118">Nested group memberships are not supported for group-based assignment tooapplications at this time.</span></span>
6. <span data-ttu-id="dc29b-119">Du kan också se vilka användare som är tilldelade toohello program, antingen direkt eller genom medlemskap i en grupp.</span><span class="sxs-lookup"><span data-stu-id="dc29b-119">You can also see which users are assigned toohello application, either directly or by membership in a group.</span></span> <span data-ttu-id="dc29b-120">toodo detta, ändra hello **visa listrutan från ”grupper”** för**”alla användare”**.</span><span class="sxs-lookup"><span data-stu-id="dc29b-120">toodo this, change hello **Show dropdown from 'Groups'** too**'All Users'**.</span></span> <span data-ttu-id="dc29b-121">hello listan visar användare i hello katalog och huruvida varje användare tilldelas toohello program.</span><span class="sxs-lookup"><span data-stu-id="dc29b-121">hello list shows users in hello directory and whether or not each user is assigned toohello application.</span></span> <span data-ttu-id="dc29b-122">hello listan visas också om hello tilldelade användare har tilldelats toohello program direkt (Tilldelningstyp visas som ”Direct”) eller genom gruppmedlemskap (Tilldelningstyp visas som ärvs.)</span><span class="sxs-lookup"><span data-stu-id="dc29b-122">hello list also shows whether hello assigned users are assigned toohello application directly (assignment type shown as 'Direct'), or by virtue of group membership (assignment type shown as 'Inherited.')</span></span>

> [!NOTE]
> <span data-ttu-id="dc29b-123">Du kan se hello användare och grupper bara när du har aktiverat Azure AD Premium eller Azure AD Basic.</span><span class="sxs-lookup"><span data-stu-id="dc29b-123">You can see hello Users and Groups tab only after you have enabled Azure AD Premium or Azure AD Basic.</span></span>
>
>

### <a name="next-steps"></a><span data-ttu-id="dc29b-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dc29b-124">Next steps</span></span>
<span data-ttu-id="dc29b-125">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="dc29b-125">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="dc29b-126">Hantera åtkomst tooresources med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="dc29b-126">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="dc29b-127">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc29b-127">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="dc29b-128">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="dc29b-128">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="dc29b-129">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="dc29b-129">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="dc29b-130">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="dc29b-130">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
