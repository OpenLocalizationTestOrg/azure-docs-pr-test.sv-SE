---
title: "aaaPreview Office 365-grupper upphör att gälla i Azure Active Directory | Microsoft Docs"
description: "Hur tooset in förfallodatum för Office 365 grupper i Azure Active Directory (förhandsgranskning)"
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
ms.openlocfilehash: a8c99961cff3aad3f4d8b0cc1b2eb8e8a4c9ba95
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-office-365-groups-expiration-preview"></a><span data-ttu-id="3c27c-103">Konfigurera förfallodatum för Office 365-grupper (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="3c27c-103">Configure Office 365 groups expiration (preview)</span></span>

<span data-ttu-id="3c27c-104">Du kan nu hantera hello livscykeln för Office 365-grupper genom att ange upphör att gälla för alla Office 365-grupper som du väljer.</span><span class="sxs-lookup"><span data-stu-id="3c27c-104">You can now manage hello lifecycle of Office 365 groups by setting expiration for any Office 365 groups that you select.</span></span> <span data-ttu-id="3c27c-105">När den här giltighetstid har angetts är ägare till dessa grupper och toorenew deras grupper om de fortfarande behöver hello grupper.</span><span class="sxs-lookup"><span data-stu-id="3c27c-105">Once this expiration is set, owners of those groups are asked toorenew their groups if they still need hello groups.</span></span> <span data-ttu-id="3c27c-106">En Office 365-grupp som inte förnyas tas bort.</span><span class="sxs-lookup"><span data-stu-id="3c27c-106">Any Office 365 group that is not renewed will be deleted.</span></span> <span data-ttu-id="3c27c-107">En Office 365-grupp som har tagits bort kan återställas inom 30 dagar av hello gruppen ägare eller administratör hello.</span><span class="sxs-lookup"><span data-stu-id="3c27c-107">Any Office 365 group that was deleted can be restored within 30 days by hello group owners or hello administrator.</span></span>  


> [!NOTE]
> <span data-ttu-id="3c27c-108">Du kan ställa in ett utgångsdatum för Office 365-grupper.</span><span class="sxs-lookup"><span data-stu-id="3c27c-108">You can set expiration for only Office 365 groups.</span></span>
>
> <span data-ttu-id="3c27c-109">Ange förfallodatum för O365-grupper måste du ange att en Azure AD Premium-licens har tilldelats</span><span class="sxs-lookup"><span data-stu-id="3c27c-109">Setting expiration for O365 groups requires that an Azure AD Premium license is assigned to</span></span>
>   - <span data-ttu-id="3c27c-110">Hej administratör som konfigurerar inställningar för hello giltighetstid för hello-klient</span><span class="sxs-lookup"><span data-stu-id="3c27c-110">hello administrator who configures hello expiration settings for hello tenant</span></span>
>   - <span data-ttu-id="3c27c-111">Alla medlemmar i hello grupper som du valt för den här inställningen</span><span class="sxs-lookup"><span data-stu-id="3c27c-111">All members of hello groups selected for this setting</span></span>

## <a name="set-office-365-groups-expiration"></a><span data-ttu-id="3c27c-112">Ställa in ett utgångsdatum för Office 365-grupper</span><span class="sxs-lookup"><span data-stu-id="3c27c-112">Set Office 365 groups expiration</span></span>

1. <span data-ttu-id="3c27c-113">Öppna hello [administrationscentret för Azure AD](https://aad.portal.azure.com) med ett konto som är en global administratör i Azure AD-klienten.</span><span class="sxs-lookup"><span data-stu-id="3c27c-113">Open hello [Azure AD admin center](https://aad.portal.azure.com) with an account that is a global administrator in your Azure AD tenant.</span></span>

2. <span data-ttu-id="3c27c-114">Öppna Azure AD, Välj **användare och grupper**.</span><span class="sxs-lookup"><span data-stu-id="3c27c-114">Open Azure AD, select **Users and groups**.</span></span>

3. <span data-ttu-id="3c27c-115">Välj **gruppinställningar** och välj sedan **giltighetstid** inställningar för tooopen hello giltighetstid.</span><span class="sxs-lookup"><span data-stu-id="3c27c-115">Select **Group settings** and then select **Expiration** tooopen hello expiration settings.</span></span>
  
  ![Bladet upphör att gälla](./media/active-directory-groups-lifecycle-azure-portal/expiration-settings.png)

4. <span data-ttu-id="3c27c-117">På hello **giltighetstid** bladet kan du:</span><span class="sxs-lookup"><span data-stu-id="3c27c-117">On hello **Expiration** blade, you can:</span></span>

  * <span data-ttu-id="3c27c-118">Ange hello grupp livslängd i dagar.</span><span class="sxs-lookup"><span data-stu-id="3c27c-118">Set hello group lifetime in days.</span></span> <span data-ttu-id="3c27c-119">Du kan välja något av hello förinställda värden eller ett anpassat värde (ska vara 31 dagar eller mer).</span><span class="sxs-lookup"><span data-stu-id="3c27c-119">You could select one of hello preset values, or a custom value (should be 31 days or more).</span></span> 
  * <span data-ttu-id="3c27c-120">Ange en e-postadress där hello förnyelse och förfallodatum meddelanden ska skickas när en grupp har ingen ägare.</span><span class="sxs-lookup"><span data-stu-id="3c27c-120">Specify an email address where hello renewal and expiration notifications should be sent when a group has no owner.</span></span> 
  * <span data-ttu-id="3c27c-121">Välj vilka Office 365-grupper att gälla.</span><span class="sxs-lookup"><span data-stu-id="3c27c-121">Select which Office 365 groups expire.</span></span> <span data-ttu-id="3c27c-122">Du kan aktivera förfallodatum för **alla** Office 365-grupper som du kan välja bland hello Office 365-grupper eller du väljer **ingen** att inaktivera upphör att gälla för alla grupper.</span><span class="sxs-lookup"><span data-stu-id="3c27c-122">You can enable expiration for **All** Office 365 groups, you can select from among hello Office 365 groups, or you select **None** to disable expiration for all groups.</span></span>
  * <span data-ttu-id="3c27c-123">Spara dina inställningar när du är klar genom att välja **spara**.</span><span class="sxs-lookup"><span data-stu-id="3c27c-123">Save your settings when you're done by selecting **Save**.</span></span>

<span data-ttu-id="3c27c-124">Anvisningar för hur toodownload och installera hello Microsoft PowerShell-modulen tooconfigure giltighetstid för Office 365-grupper via PowerShell finns [Azure Active Directory V2 PowerShell-modul - offentliga förhandsversionen 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span><span class="sxs-lookup"><span data-stu-id="3c27c-124">For instructions on how toodownload and install hello Microsoft PowerShell module tooconfigure expiration for Office 365 groups via PowerShell, see [Azure Active Directory V2 PowerShell Module - Public Preview Release 2.0.0.137](https://www.powershellgallery.com/packages/AzureADPreview/2.0.0.137).</span></span>

<span data-ttu-id="3c27c-125">E-postmeddelanden som den här skickas toohello Office 365 gruppen ägare 30 dagar, 15 dagar och 1 dag tidigare tooexpiration hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="3c27c-125">Email notifications such as this one are sent toohello Office 365 group owners 30 days, 15 days, and 1 day prior tooexpiration of hello group.</span></span>

![Förfallodatum för e-postavisering](./media/active-directory-groups-lifecycle-azure-portal/expiration-notification.png)

<span data-ttu-id="3c27c-127">hello gruppägare kan sedan välja **förnya grupp** toorenew hello Office 365-grupp eller kan du välja **gå toogroup** toosee hello medlemmar och annan information om hello grupp.</span><span class="sxs-lookup"><span data-stu-id="3c27c-127">hello group owner can then select **Renew group** toorenew hello Office 365 group, or can select **Go toogroup** toosee hello members and other details about hello group.</span></span>

<span data-ttu-id="3c27c-128">När en grupp upphör att gälla hello gruppen har tagits bort en dag efter hello upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="3c27c-128">When a group expires, hello group is deleted one day after hello expiration date.</span></span> <span data-ttu-id="3c27c-129">Ett e-postmeddelande som den här skickas toohello Office 365 gruppen ägare informerar dem om hello förfallodatum och efterföljande borttagning av deras Office 365-grupp.</span><span class="sxs-lookup"><span data-stu-id="3c27c-129">An email notification such as this one is sent toohello Office 365 group owners informing them about hello expiration and subsequent deletion of their Office 365 group.</span></span>

![Gruppen borttagning e-postavisering](./media/active-directory-groups-lifecycle-azure-portal/deletion-notification.png)

<span data-ttu-id="3c27c-131">hello gruppen kan återställas genom att välja **återställning grupp** eller med hjälp av PowerShell-cmdlets som beskrivs i [återställa en borttagen Office 365-grupp i Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/ Active-Directory-groups-Restore-Azure-Portal).</span><span class="sxs-lookup"><span data-stu-id="3c27c-131">hello group can be restored by selecting **Restore group** or by using PowerShell cmdlets, as described in [Restore a deleted Office 365 group in Azure Active Directory] (https://docs.microsoft.com/azure/active-directory/active-directory-groups-restore-azure-portal).</span></span>
    
<span data-ttu-id="3c27c-132">Om hello-grupp som du återställa innehåller dokument, SharePoint-webbplatser eller andra beständiga objekt, kan det ta upp too24 timmar toofully återställning hello grupp och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="3c27c-132">If hello group you're restoring contains documents, SharePoint sites, or other persistent objects, it might take up too24 hours toofully restore hello group and its contents.</span></span>

> [!NOTE]
> * <span data-ttu-id="3c27c-133">När du distribuerar hello inställningar för giltighetstid kan uppstå vissa grupper som är äldre än hello giltighetstid fönster.</span><span class="sxs-lookup"><span data-stu-id="3c27c-133">When deploying hello expiration settings, there might be some groups that are older than hello expiration window.</span></span> <span data-ttu-id="3c27c-134">Dessa grupper är inte omedelbart bort, men är ställa in too30 dagar tills upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="3c27c-134">These groups are not be immediately deleted, but are set too30 days until expiration.</span></span> <span data-ttu-id="3c27c-135">hello första förnyelse e-postmeddelande skickas ut inom en dag.</span><span class="sxs-lookup"><span data-stu-id="3c27c-135">hello first renewal notification email is sent out within a day.</span></span> <span data-ttu-id="3c27c-136">Till exempel grupp A skapades 400 dagar sedan och hello giltighetstid intervallet anges too180 dagar.</span><span class="sxs-lookup"><span data-stu-id="3c27c-136">For example, Group A was created 400 days ago, and hello expiration interval is set too180 days.</span></span> <span data-ttu-id="3c27c-137">När du använder inställningar för giltighetstid har grupp A 30 dagar innan den tas bort, såvida inte hello ägare förnyar den.</span><span class="sxs-lookup"><span data-stu-id="3c27c-137">When you apply expiration settings, Group A has 30 days before it is deleted, unless hello owner renews it.</span></span>
> * <span data-ttu-id="3c27c-138">När en dynamisk grupp tas bort och återställs, visas den som en ny grupp och nytt ifyllda bl.a toohello regeln.</span><span class="sxs-lookup"><span data-stu-id="3c27c-138">When a dynamic group is deleted and restored, it is seen as a new group and re-populated according toohello rule.</span></span> <span data-ttu-id="3c27c-139">Den här processen kan ta upp too24 timmar.</span><span class="sxs-lookup"><span data-stu-id="3c27c-139">This process might take up too24 hours.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3c27c-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3c27c-140">Next steps</span></span>
<span data-ttu-id="3c27c-141">Dessa artiklar innehåller ytterligare information om Azure AD-grupper.</span><span class="sxs-lookup"><span data-stu-id="3c27c-141">These articles provide additional information on Azure AD groups.</span></span>

* [<span data-ttu-id="3c27c-142">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="3c27c-142">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="3c27c-143">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="3c27c-143">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="3c27c-144">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="3c27c-144">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="3c27c-145">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="3c27c-145">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="3c27c-146">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="3c27c-146">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
