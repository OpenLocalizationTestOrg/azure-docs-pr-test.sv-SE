---
title: "Förhandsgranska förfallodatum för Office 365-grupper i Azure Active Directory | Microsoft Docs"
description: "Hur du ställer in förfallodatum för Office 365-grupper i Azure Active Directory (förhandsgranskning)"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: kairaz.contractor
ms.custom: it-pro
ms.openlocfilehash: 8a43df84fd050d7b4bd8d937b8c55e744cb805d3
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a><span data-ttu-id="46904-103">Konfigurera förfallodatum för Office 365-grupper (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="46904-103">Configure Office 365 groups expiration (preview)</span></span>

<span data-ttu-id="46904-104">Du kan nu hantera livscykeln för Office 365-grupper genom att ange upphör att gälla för alla Office 365-grupper som du väljer.</span><span class="sxs-lookup"><span data-stu-id="46904-104">You can now manage the lifecycle of Office 365 groups by setting expiration for any Office 365 groups that you select.</span></span> <span data-ttu-id="46904-105">När den här giltighetstid har angetts uppmanas ägare i dessa grupper att förnya sina grupper om de fortfarande behöver grupperna.</span><span class="sxs-lookup"><span data-stu-id="46904-105">Once this expiration is set, owners of those groups are asked to renew their groups if they still need the groups.</span></span> <span data-ttu-id="46904-106">En Office 365-grupp som inte förnyas tas bort.</span><span class="sxs-lookup"><span data-stu-id="46904-106">Any Office 365 group that is not renewed will be deleted.</span></span> <span data-ttu-id="46904-107">Du kan återställa en Office 365-grupp som har tagits bort inom 30 dagar av gruppen ägare eller administratören.</span><span class="sxs-lookup"><span data-stu-id="46904-107">Any Office 365 group that was deleted can be restored within 30 days by the group owners or the administrator.</span></span>  


> [!NOTE]
> <span data-ttu-id="46904-108">Du kan ställa in ett utgångsdatum för Office 365-grupper.</span><span class="sxs-lookup"><span data-stu-id="46904-108">You can set expiration for only Office 365 groups.</span></span>
>
> <span data-ttu-id="46904-109">Ange förfallodatum för O365-grupper måste du ange att en Azure AD Premium-licens har tilldelats</span><span class="sxs-lookup"><span data-stu-id="46904-109">Setting expiration for O365 groups requires that an Azure AD Premium license is assigned to</span></span>
>   - <span data-ttu-id="46904-110">Administratören som konfigurerar inställningar för giltighetstid för klienten</span><span class="sxs-lookup"><span data-stu-id="46904-110">The administrator who configures the expiration settings for the tenant</span></span>
>   - <span data-ttu-id="46904-111">Alla medlemmar i grupper som valts för den här inställningen</span><span class="sxs-lookup"><span data-stu-id="46904-111">All members of the groups selected for this setting</span></span>

## <a name="set-office-365-groups-expiration"></a><span data-ttu-id="46904-112">Ställa in ett utgångsdatum för Office 365-grupper</span><span class="sxs-lookup"><span data-stu-id="46904-112">Set Office 365 groups expiration</span></span>

1. <span data-ttu-id="46904-113">Öppna den [administrationscentret för Azure AD](https://aad.portal.azure.com) med ett konto som är en global administratör i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="46904-113">Open the [Azure AD admin center](https://aad.portal.azure.com) with an account that is a global administrator in your Azure AD tenant.</span></span>

2. <span data-ttu-id="46904-114">Öppna Azure AD, Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="46904-114">Open Azure AD, select **Users and groups**.</span></span>

3. <span data-ttu-id="46904-115">Välj **gruppinställningarna** och välj sedan **giltighetstid** att öppna inställningarna för förfallodatum.</span><span class="sxs-lookup"><span data-stu-id="46904-115">Select **Group settings** and then select **Expiration** to open the expiration settings.</span></span>
  
  ![Bladet upphör att gälla](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. <span data-ttu-id="46904-117">På den **giltighetstid** bladet kan du:</span><span class="sxs-lookup"><span data-stu-id="46904-117">On the **Expiration** blade, you can:</span></span>

  * <span data-ttu-id="46904-118">Ange grupp-livstid i dagar.</span><span class="sxs-lookup"><span data-stu-id="46904-118">Set the group lifetime in days.</span></span> <span data-ttu-id="46904-119">Du kan välja något av de fördefinierade värdena eller ett anpassat värde (ska vara 31 dagar eller mer).</span><span class="sxs-lookup"><span data-stu-id="46904-119">You could select one of the preset values, or a custom value (should be 31 days or more).</span></span> 
  * <span data-ttu-id="46904-120">Ange en e-postadress där förnyelse och förfallodatum meddelanden ska skickas när en grupp har ingen ägare.</span><span class="sxs-lookup"><span data-stu-id="46904-120">Specify an email address where the renewal and expiration notifications should be sent when a group has no owner.</span></span> 
  * <span data-ttu-id="46904-121">Välj vilka Office 365-grupper att gälla.</span><span class="sxs-lookup"><span data-stu-id="46904-121">Select which Office 365 groups expire.</span></span> <span data-ttu-id="46904-122">Du kan aktivera förfallodatum för **alla** Office 365-grupper du kan välja bland Office 365-grupper eller du väljer **ingen** att inaktivera upphör att gälla för alla grupper.</span><span class="sxs-lookup"><span data-stu-id="46904-122">You can enable expiration for **All** Office 365 groups, you can select from among the Office 365 groups, or you select **None** to disable expiration for all groups.</span></span>
  * <span data-ttu-id="46904-123">Spara dina inställningar när du är klar genom att välja **spara**.</span><span class="sxs-lookup"><span data-stu-id="46904-123">Save your settings when you're done by selecting **Save**.</span></span>

<span data-ttu-id="46904-124">Instruktioner om hur du hämtar och installerar Microsoft PowerShell-modulen konfigurera giltighetstid för Office 365-grupper via PowerShell finns i [Azure Active Directory V2 PowerShell-modul - offentliga förhandsversionen 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span><span class="sxs-lookup"><span data-stu-id="46904-124">For instructions on how to download and install the Microsoft PowerShell module to configure expiration for Office 365 groups via PowerShell, see [Azure Active Directory V2 PowerShell Module - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span></span>

<span data-ttu-id="46904-125">E-postmeddelanden som den här skickas till Office 365 gruppen ägare 30 dagar, 15 dagar och 1 dag innan upphör att gälla i gruppen.</span><span class="sxs-lookup"><span data-stu-id="46904-125">Email notifications such as this one are sent to the Office 365 group owners 30 days, 15 days, and 1 day prior to expiration of the group.</span></span>

![Förfallodatum för e-postavisering](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

<span data-ttu-id="46904-127">Gruppägare kan sedan välja **förnya grupp** att förnya Office 365 grupp eller välja **går du till gruppen** att se medlemmarna och annan information om gruppen.</span><span class="sxs-lookup"><span data-stu-id="46904-127">The group owner can then select **Renew group** to renew the Office 365 group, or can select **Go to group** to see the members and other details about the group.</span></span>

<span data-ttu-id="46904-128">När en grupp upphör att gälla bort gruppen en dag efter förfallodatumet.</span><span class="sxs-lookup"><span data-stu-id="46904-128">When a group expires, the group is deleted one day after the expiration date.</span></span> <span data-ttu-id="46904-129">Ett e-postmeddelande som den här skickas till Office 365 gruppen ägare informerar dem om förfallodatum och efterföljande borttagning av deras Office 365-grupp.</span><span class="sxs-lookup"><span data-stu-id="46904-129">An email notification such as this one is sent to the Office 365 group owners informing them about the expiration and subsequent deletion of their Office 365 group.</span></span>

![Gruppen borttagning e-postavisering](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

<span data-ttu-id="46904-131">Gruppen kan återställas genom att välja **återställning grupp** eller med hjälp av PowerShell-cmdlets som beskrivs i [återställa en borttagen Office 365-grupp i Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-groups-Restore-Azure-Portal).</span><span class="sxs-lookup"><span data-stu-id="46904-131">The group can be restored by selecting **Restore group** or by using PowerShell cmdlets, as described in [Restore a deleted Office 365 group in Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).</span></span>
    
<span data-ttu-id="46904-132">Om gruppen du återställa innehåller dokument, SharePoint-webbplatser eller andra beständiga objekt, kan det ta upp till 24 timmar att fullständigt återställa gruppen och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="46904-132">If the group you're restoring contains documents, SharePoint sites, or other persistent objects, it might take up to 24 hours to fully restore the group and its contents.</span></span>

> [!NOTE]
> * <span data-ttu-id="46904-133">När du distribuerar inställningar för giltighetstid kan finnas det vissa grupper som är äldre än fönstret upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="46904-133">When deploying the expiration settings, there might be some groups that are older than the expiration window.</span></span> <span data-ttu-id="46904-134">Dessa grupper inte är raderas omedelbart, men är inställda på 30 dagar tills upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="46904-134">These groups are not be immediately deleted, but are set to 30 days until expiration.</span></span> <span data-ttu-id="46904-135">Första förnyelse-e-postmeddelandet skickas ut inom en dag.</span><span class="sxs-lookup"><span data-stu-id="46904-135">The first renewal notification email is sent out within a day.</span></span> <span data-ttu-id="46904-136">Till exempel grupp A skapades 400 dagar sedan och förfallodatum intervallet anges till 180 dagar.</span><span class="sxs-lookup"><span data-stu-id="46904-136">For example, Group A was created 400 days ago, and the expiration interval is set to 180 days.</span></span> <span data-ttu-id="46904-137">När du använder inställningar för giltighetstid har grupp A 30 dagar innan den tas bort, såvida inte ägaren förnyar den.</span><span class="sxs-lookup"><span data-stu-id="46904-137">When you apply expiration settings, Group A has 30 days before it is deleted, unless the owner renews it.</span></span>
> * <span data-ttu-id="46904-138">När en dynamisk grupp tas bort och återställs, är det ses som en ny grupp och nytt fylls enligt regeln.</span><span class="sxs-lookup"><span data-stu-id="46904-138">When a dynamic group is deleted and restored, it is seen as a new group and re-populated according to the rule.</span></span> <span data-ttu-id="46904-139">Den här processen kan ta upp till 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="46904-139">This process might take up to 24 hours.</span></span>

## <a name="next-steps"></a><span data-ttu-id="46904-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="46904-140">Next steps</span></span>
<span data-ttu-id="46904-141">Dessa artiklar innehåller ytterligare information om Azure AD-grupper.</span><span class="sxs-lookup"><span data-stu-id="46904-141">These articles provide additional information on Azure AD groups.</span></span>

* [<span data-ttu-id="46904-142">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="46904-142">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="46904-143">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="46904-143">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="46904-144">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="46904-144">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="46904-145">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="46904-145">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="46904-146">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="46904-146">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
