---
title: Dedikerade grupper i Azure Active Directory | Microsoft Docs
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
ms.openlocfilehash: d9decd5de6a5bafc525edc5b04c82701185088ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="dedicated-groups-in-azure-active-directory"></a><span data-ttu-id="7d021-103">Dedikerade grupper i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d021-103">Dedicated groups in Azure Active Directory</span></span>
<span data-ttu-id="7d021-104">I Azure Active Directory (Azure AD), funktionen dedikerade grupper skapas automatiskt och fyller medlemskap för Azure AD fördefinierade grupper.</span><span class="sxs-lookup"><span data-stu-id="7d021-104">In Azure Active Directory (Azure AD), the dedicated groups feature automatically creates and populates membership for Azure AD predefined groups.</span></span> <span data-ttu-id="7d021-105">Medlemmar i dedikerade grupper kan inte läggas till eller tas bort med hjälp av den klassiska Azure-portalen, Windows PowerShell-cmdlets eller programmässigt.</span><span class="sxs-lookup"><span data-stu-id="7d021-105">Members of dedicated groups cannot be added or removed using the Azure classic portal, Windows PowerShell cmdlets, or programmatically.</span></span>

> [!NOTE]
> <span data-ttu-id="7d021-106">Dedikerade grupper kräver att en Azure AD Premium-licens har tilldelats</span><span class="sxs-lookup"><span data-stu-id="7d021-106">Dedicated groups require that an Azure AD Premium license is assigned to</span></span>
>
> * <span data-ttu-id="7d021-107">Administratören som hanterar regeln för en grupp</span><span class="sxs-lookup"><span data-stu-id="7d021-107">the administrator who manages the rule on a group</span></span>
> * <span data-ttu-id="7d021-108">alla användare som är markerade som regeln ska vara medlem i gruppen</span><span class="sxs-lookup"><span data-stu-id="7d021-108">all users who are selected by the rule to be a member of the group</span></span>
>
>

<span data-ttu-id="7d021-109">**Så här aktiverar du dedikerade grupper**</span><span class="sxs-lookup"><span data-stu-id="7d021-109">**To enable dedicated groups**</span></span>

1. <span data-ttu-id="7d021-110">I den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="7d021-110">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="7d021-111">Välj den **grupper** och sedan öppna den grupp du vill redigera.</span><span class="sxs-lookup"><span data-stu-id="7d021-111">Select the **Groups** tab, and then open the group you want to edit.</span></span>
3. <span data-ttu-id="7d021-112">Välj den **konfigurera** fliken och ange sedan **aktivera särskilda grupper** till **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7d021-112">Select the **Configure** tab, and then set **Enable Dedicated Groups** to **Yes**.</span></span>

<span data-ttu-id="7d021-113">När du aktiverar dedikerade grupper växeln har angetts till **Ja**, ytterligare kan du aktivera katalogen för att automatiskt skapa gruppen Alla användare dedikerade genom att ange den **aktiverar ”alla användare” grupp** växla till  **Ja**.</span><span class="sxs-lookup"><span data-stu-id="7d021-113">Once the Enable Dedicated Groups switch is set to **Yes**, you can further enable the directory to automatically create the All Users dedicated group by setting the **Enable “All Users” Group** switch to **Yes**.</span></span> <span data-ttu-id="7d021-114">Du kan sedan också redigera namnet på den här dedikerade gruppen genom att skriva det i den **visningsnamn för ”alla användare” grupp** fältet.</span><span class="sxs-lookup"><span data-stu-id="7d021-114">You can then also edit the name of this dedicated group by typing it in the **Display Name for “All Users” Group** field.</span></span>

<span data-ttu-id="7d021-115">Gruppen Alla användare kan användas för att tilldela samma behörigheter för alla användare i din katalog.</span><span class="sxs-lookup"><span data-stu-id="7d021-115">The All Users group can be used to assign the same permissions to all the users in your directory.</span></span> <span data-ttu-id="7d021-116">Du kan till exempel ge alla användare i din katalogåtkomst till ett SaaS-program genom att tilldela behörighet för gruppen Alla användare dedikerade till det här programmet.</span><span class="sxs-lookup"><span data-stu-id="7d021-116">For example, you can grant all users in your directory access to a SaaS application by assigning access for the All Users dedicated group to this application.</span></span>

<span data-ttu-id="7d021-117">Dedikerad alla användare gruppen innehåller alla användare i katalogen, inklusive gäster och externa användare.</span><span class="sxs-lookup"><span data-stu-id="7d021-117">The dedicated All Users group includes all users in the directory, including guests and external users.</span></span> <span data-ttu-id="7d021-118">Om du behöver en grupp som omfattar inte externa användare, så du kan göra detta genom att skapa en grupp med en attributbaserad dynamisk regel till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="7d021-118">If you need a group that excludes external users, then you can accomplish this by creating a group with an attribute-based dynamic rule such as the following:</span></span>

                (user.userPrincipalName -notContains "#EXT#@")

<span data-ttu-id="7d021-119">För en grupp som inte omfattar alla gäster, använder du en regel till exempel följande:</span><span class="sxs-lookup"><span data-stu-id="7d021-119">For a group that excludes all Guests, use a rule such as the following:</span></span>

                (user.userType -ne "Guest")

<span data-ttu-id="7d021-120">Mer information om hur du skapar *avancerade* regler (regler som kan innehålla flera jämförelser) för dynamiska gruppmedlemskap finns i [Använda attribut för att skapa avancerade regler](active-directory-accessmanagement-groups-with-advanced-rules.md).</span><span class="sxs-lookup"><span data-stu-id="7d021-120">To learn about how to create *advanced* rules (rules that can contain multiple comparisons) for dynamic group membership, see [Using attributes to create advanced rules](active-directory-accessmanagement-groups-with-advanced-rules.md).</span></span>

### <a name="next-steps"></a><span data-ttu-id="7d021-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7d021-121">Next steps</span></span>
<span data-ttu-id="7d021-122">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7d021-122">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="7d021-123">Hantera åtkomst till resurser med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="7d021-123">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="7d021-124">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d021-124">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="7d021-125">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7d021-125">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="7d021-126">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7d021-126">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
