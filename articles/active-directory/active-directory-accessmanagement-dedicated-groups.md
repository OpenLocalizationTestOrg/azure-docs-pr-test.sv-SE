---
title: aaaDedicated grupper i Azure Active Directory | Microsoft Docs
description: "Översikt över hur dedikerade grupper resurser i Azure Active Directory och hur de skapas."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 86158909-083a-41fe-8090-955e96ad1865
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/08/2017
ms.author: curtand
ms.openlocfilehash: 8feec6e1a4e6b384470392d3043caeeec2b03dd2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a><span data-ttu-id="734da-103">Dedikerade grupper i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="734da-103">Dedicated groups in Azure Active Directory</span></span>
<span data-ttu-id="734da-104">I Azure Active Directory (Azure AD), hello dedikerade grupper funktionen skapas automatiskt och fyller medlemskap för Azure AD fördefinierade grupper.</span><span class="sxs-lookup"><span data-stu-id="734da-104">In Azure Active Directory (Azure AD), hello dedicated groups feature automatically creates and populates membership for Azure AD predefined groups.</span></span> <span data-ttu-id="734da-105">Medlemmar i dedikerade grupper kan inte läggas till eller tas bort med hjälp av hello Azure klassiska portal, Windows PowerShell-cmdletar eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="734da-105">Members of dedicated groups cannot be added or removed using hello Azure classic portal, Windows PowerShell cmdlets, or programmatically.</span></span>

> [!NOTE]
> <span data-ttu-id="734da-106">Dedikerade grupper kräver att en Azure AD Premium-licens har tilldelats</span><span class="sxs-lookup"><span data-stu-id="734da-106">Dedicated groups require that an Azure AD Premium license is assigned to</span></span>
>
> * <span data-ttu-id="734da-107">Hej administratör som hanterar hello regeln för en grupp</span><span class="sxs-lookup"><span data-stu-id="734da-107">hello administrator who manages hello rule on a group</span></span>
> * <span data-ttu-id="734da-108">alla användare som är markerade som hello regel toobe medlem i gruppen hello</span><span class="sxs-lookup"><span data-stu-id="734da-108">all users who are selected by hello rule toobe a member of hello group</span></span>
>
>

<span data-ttu-id="734da-109">**tooenable dedikerade grupper**</span><span class="sxs-lookup"><span data-stu-id="734da-109">**tooenable dedicated groups**</span></span>

1. <span data-ttu-id="734da-110">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="734da-110">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="734da-111">Välj hello **grupper** fliken och sedan öppna hello-grupp som du vill tooedit.</span><span class="sxs-lookup"><span data-stu-id="734da-111">Select hello **Groups** tab, and then open hello group you want tooedit.</span></span>
3. <span data-ttu-id="734da-112">Välj hello **konfigurera** fliken och ange sedan **aktivera särskilda grupper** för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="734da-112">Select hello **Configure** tab, and then set **Enable Dedicated Groups** too**Yes**.</span></span>

<span data-ttu-id="734da-113">När hello aktivera dedikerade grupper växeln har angetts för**Ja**, du kan aktivera ytterligare hello directory tooautomatically skapar hello alla användare dedikerad grupp genom att ange hello **aktiverar ”alla användare” grupp** Växla för**Ja**.</span><span class="sxs-lookup"><span data-stu-id="734da-113">Once hello Enable Dedicated Groups switch is set too**Yes**, you can further enable hello directory tooautomatically create hello All Users dedicated group by setting hello **Enable “All Users” Group** switch too**Yes**.</span></span> <span data-ttu-id="734da-114">Du kan också redigera hello namnet på den här dedikerade gruppen genom att skriva det i hello **visningsnamn för ”alla användare” grupp** fältet.</span><span class="sxs-lookup"><span data-stu-id="734da-114">You can then also edit hello name of this dedicated group by typing it in hello **Display Name for “All Users” Group** field.</span></span>

<span data-ttu-id="734da-115">hello gruppen för alla användare kan användas för tooassign hello samma behörigheter tooall hello användare i katalogen.</span><span class="sxs-lookup"><span data-stu-id="734da-115">hello All Users group can be used tooassign hello same permissions tooall hello users in your directory.</span></span> <span data-ttu-id="734da-116">Du kan till exempel ge alla användare i din katalog åtkomst tooa SaaS-program genom att tilldela åtkomst för hello alla användare dedikerad grupp toothis program.</span><span class="sxs-lookup"><span data-stu-id="734da-116">For example, you can grant all users in your directory access tooa SaaS application by assigning access for hello All Users dedicated group toothis application.</span></span>

<span data-ttu-id="734da-117">hello dedikerad alla användare gruppen innehåller alla användare i hello directory, inklusive gäster och externa användare.</span><span class="sxs-lookup"><span data-stu-id="734da-117">hello dedicated All Users group includes all users in hello directory, including guests and external users.</span></span> <span data-ttu-id="734da-118">Om du behöver en grupp som omfattar inte externa användare, så du åstadkommer detta genom att skapa en grupp med en attributbaserad dynamisk regel till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="734da-118">If you need a group that excludes external users, then you can accomplish this by creating a group with an attribute-based dynamic rule such as hello following:</span></span>

                (user.userPrincipalName -notContains "#EXT#@")

<span data-ttu-id="734da-119">För en grupp som inte omfattar alla gäster, använder du en regel till exempel hello följande:</span><span class="sxs-lookup"><span data-stu-id="734da-119">For a group that excludes all Guests, use a rule such as hello following:</span></span>

                (user.userType -ne "Guest")

<span data-ttu-id="734da-120">toolearn om hur toocreate *avancerade* regler (regler som kan innehålla flera jämförelser) för dynamiska gruppmedlemskap finns [genom att använda attribut toocreate avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="734da-120">toolearn about how toocreate *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes toocreate advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

### <a name="next-steps"></a><span data-ttu-id="734da-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="734da-121">Next steps</span></span>
<span data-ttu-id="734da-122">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="734da-122">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="734da-123">Hantera åtkomst tooresources med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="734da-123">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="734da-124">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="734da-124">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="734da-125">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="734da-125">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="734da-126">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="734da-126">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
