---
title: "aaaNext steg för hantering med hjälp av grupper | Microsoft Docs"
description: "Avancerade hur-för hantering av säkerhetsgrupper och hur toouse dessa grupper toomanage åtkomst tooa resurs."
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
ms.openlocfilehash: 4fd55f893290fac3551a130f29bd12709cf551e6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="managing-owners-for-a-group"></a><span data-ttu-id="e042d-103">Hantera ägare för en grupp</span><span class="sxs-lookup"><span data-stu-id="e042d-103">Managing owners for a group</span></span>
<span data-ttu-id="e042d-104">När en resursägare har tilldelat åtkomst tooa resurs tooan Azure AD-grupp, hanteras hello medlemskap i gruppen hello av hello gruppägare.</span><span class="sxs-lookup"><span data-stu-id="e042d-104">Once a resource owner has assigned access tooa resource tooan Azure AD group, hello membership of hello group is managed by hello group owner.</span></span> <span data-ttu-id="e042d-105">hello resursägaren effektivt hello behörighet tooassign användare toohello resurs toohello ägare hello gruppen.</span><span class="sxs-lookup"><span data-stu-id="e042d-105">hello resource owner effectively delegates hello permission tooassign users toohello resource toohello owner of hello group.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="e042d-106">Microsoft rekommenderar att du hanterar Azure AD med hjälp av hello [administrationscentret för Azure AD](https://aad.portal.azure.com) i hello Azure-portalen istället för att använda hello klassiska Azure-portalen som hänvisas till i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="e042d-106">Microsoft recommends that you manage Azure AD using hello [Azure AD admin center](https://aad.portal.azure.com) in hello Azure portal instead of using hello Azure classic portal referenced in this article.</span></span> 

## <a name="assigning-group-ownership"></a><span data-ttu-id="e042d-107">Tilldela ägarskap för grupp</span><span class="sxs-lookup"><span data-stu-id="e042d-107">Assigning group ownership</span></span>
<span data-ttu-id="e042d-108">**tooadd en ägare tooa grupp**</span><span class="sxs-lookup"><span data-stu-id="e042d-108">**tooadd an owner tooa group**</span></span>

1. <span data-ttu-id="e042d-109">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="e042d-109">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="e042d-110">Välj hello **grupper** fliken och sedan öppna hello-grupp som du vill tooadd ägare till.</span><span class="sxs-lookup"><span data-stu-id="e042d-110">Select hello **Groups** tab, and then open hello group that you want tooadd owners to.</span></span>
3. <span data-ttu-id="e042d-111">Välj **lägga till ägare**.</span><span class="sxs-lookup"><span data-stu-id="e042d-111">Select **Add Owners**.</span></span>
4. <span data-ttu-id="e042d-112">På hello **lägga till ägare** sidan, Välj hello användare du vill tooadd hello ägare av den här gruppen och kontrollera att namnet läggs toohello **valda** fönstret.</span><span class="sxs-lookup"><span data-stu-id="e042d-112">On hello **Add owners** page, select hello user that you want tooadd as hello owner of this group, and make sure this name is added toohello **Selected** pane.</span></span>

<span data-ttu-id="e042d-113">**tooremove ägare från en grupp**</span><span class="sxs-lookup"><span data-stu-id="e042d-113">**tooremove an owner from a group**</span></span>

1. <span data-ttu-id="e042d-114">I hello [klassiska Azure-portalen](https://manage.windowsazure.com)väljer **Active Directory**, och sedan öppna din organisations katalog.</span><span class="sxs-lookup"><span data-stu-id="e042d-114">In hello [Azure classic portal](https://manage.windowsazure.com), select **Active Directory**, and then open your organization’s directory.</span></span>
2. <span data-ttu-id="e042d-115">Välj hello **grupper** fliken och sedan öppna hello grupp som du vill tooremove ägare från.</span><span class="sxs-lookup"><span data-stu-id="e042d-115">Select hello **Groups** tab, and then open hello group that you want tooremove an owner from.</span></span>
3. <span data-ttu-id="e042d-116">Välj hello **ägare** fliken.</span><span class="sxs-lookup"><span data-stu-id="e042d-116">Select hello **Owners** tab.</span></span>
4. <span data-ttu-id="e042d-117">Välj hello ägare att du vill tooremove från den här gruppen och välj sedan **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="e042d-117">Select hello owner that you want tooremove from this group, and then select **Remove**.</span></span>

## <a name="additional-information"></a><span data-ttu-id="e042d-118">Ytterligare information</span><span class="sxs-lookup"><span data-stu-id="e042d-118">Additional information</span></span>
<span data-ttu-id="e042d-119">Dessa artiklar innehåller ytterligare information om Azure Active Directory.</span><span class="sxs-lookup"><span data-stu-id="e042d-119">These articles provide additional information on Azure Active Directory.</span></span>

* [<span data-ttu-id="e042d-120">Hantera åtkomst tooresources med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="e042d-120">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="e042d-121">Azure Active Directory-cmdletar för att konfigurera gruppinställningar</span><span class="sxs-lookup"><span data-stu-id="e042d-121">Azure Active Directory cmdlets for configuring group settings</span></span>](active-directory-accessmanagement-groups-settings-cmdlets.md)
* [<span data-ttu-id="e042d-122">Artikelindex för programhantering i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e042d-122">Article Index for Application Management in Azure Active Directory</span></span>](active-directory-apps-index.md)
* [<span data-ttu-id="e042d-123">Vad är Azure Active Directory?</span><span class="sxs-lookup"><span data-stu-id="e042d-123">What is Azure Active Directory?</span></span>](active-directory-whatis.md)
* [<span data-ttu-id="e042d-124">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e042d-124">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
