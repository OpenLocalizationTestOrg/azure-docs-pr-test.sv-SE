---
title: "Nästa steg för hantering med hjälp av grupper | Microsoft Docs"
description: "Avancerade hur-för hantering av säkerhetsgrupper och hur du använder dessa grupper för att hantera åtkomst till en resurs."
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 44350a3c-8ea1-4da1-aaac-7fc53933dd21
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2017
ms.author: curtand
ms.custom: oldportal;it-pro;
ms.openlocfilehash: 82fbeb379e90add09f7c569111053f6e9b1bc9c5
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="managing-owners-for-a-group"></a><span data-ttu-id="7bad9-103">Hantera ägare för en grupp</span><span class="sxs-lookup"><span data-stu-id="7bad9-103">Managing owners for a group</span></span>
<span data-ttu-id="7bad9-104">När en resursägare har tilldelats till en resurs till en Azure AD-grupp, hanteras medlemskap i gruppen av gruppägare.</span><span class="sxs-lookup"><span data-stu-id="7bad9-104">Once a resource owner has assigned access to a resource to an Azure AD group, the membership of the group is managed by the group owner.</span></span> <span data-ttu-id="7bad9-105">Effektivt resursägaren behörighet att tilldela användare till resursen till ägare av gruppen.</span><span class="sxs-lookup"><span data-stu-id="7bad9-105">The resource owner effectively delegates the permission to assign users to the resource to the owner of the group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7bad9-106">Microsoft rekommenderar att du hanterar Azure AD via [Azure AD administratörscenter](https://aad.portal.azure.com) på Azure Portal istället för via den klassiska Azure-portalen som nämns i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="7bad9-106">Microsoft recommends that you manage Azure AD using the [Azure AD admin center](https://aad.portal.azure.com) in the Azure portal instead of using the Azure classic portal referenced in this article.</span></span> 

## <a name="assigning-group-ownership"></a><span data-ttu-id="7bad9-107">Tilldela ägarskap för grupp</span><span class="sxs-lookup"><span data-stu-id="7bad9-107">Assigning group ownership</span></span>
<span data-ttu-id="7bad9-108">**Att lägga till en ägare till en grupp**</span><span class="sxs-lookup"><span data-stu-id="7bad9-108">**To add an owner to a group**</span></span>

1. <span data-ttu-id="7bad9-109">I den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="7bad9-109">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="7bad9-110">Välj den **grupper** och sedan öppna den grupp som du vill lägga till ägare till.</span><span class="sxs-lookup"><span data-stu-id="7bad9-110">Select the **Groups** tab, and then open the group that you want to add owners to.</span></span>
3. <span data-ttu-id="7bad9-111">Välj **lägga till ägare**.</span><span class="sxs-lookup"><span data-stu-id="7bad9-111">Select **Add Owners**.</span></span>
4. <span data-ttu-id="7bad9-112">På den **lägga till ägare** markerar du den användare som du vill lägga till som ägare av den här gruppen och kontrollera att namnet läggs till i **valda** fönstret.</span><span class="sxs-lookup"><span data-stu-id="7bad9-112">On the **Add owners** page, select the user that you want to add as the owner of this group, and make sure this name is added to the **Selected** pane.</span></span>

<span data-ttu-id="7bad9-113">**Ta bort en ägare från en grupp**</span><span class="sxs-lookup"><span data-stu-id="7bad9-113">**To remove an owner from a group**</span></span>

1. <span data-ttu-id="7bad9-114">I den [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="7bad9-114">In the [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="7bad9-115">Välj den **grupper** och sedan öppna den grupp som du vill ta bort en ägare från.</span><span class="sxs-lookup"><span data-stu-id="7bad9-115">Select the **Groups** tab, and then open the group that you want to remove an owner from.</span></span>
3. <span data-ttu-id="7bad9-116">Välj den **ägare** fliken.</span><span class="sxs-lookup"><span data-stu-id="7bad9-116">Select the **Owners** tab.</span></span>
4. <span data-ttu-id="7bad9-117">Välj en ägare som du vill ta bort från den här gruppen och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="7bad9-117">Select the owner that you want to remove from this group, and then select **Remove**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="7bad9-118">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="7bad9-118">Additional information</span></span>
<span data-ttu-id="7bad9-119">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="7bad9-119">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="7bad9-120">Hantera åtkomst till resurser med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="7bad9-120">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="7bad9-121">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="7bad9-121">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="7bad9-122">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7bad9-122">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="7bad9-123">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="7bad9-123">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="7bad9-124">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="7bad9-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
