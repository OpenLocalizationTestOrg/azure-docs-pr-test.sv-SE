---
title: "Återställa en borttagen Office 365-grupp i Azure Active Directory | Microsoft Docs"
description: "Återställa en borttagen grupp, visa återställningsbara grupper och permamnently ta bort en grupp i Azure Active Directory"
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: cc5f232a-1e77-45c2-b28b-1fcb4621725c
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: curtand
ms.openlocfilehash: 32d36ae96797a59bb47bf782ab038f21f231f046
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a><span data-ttu-id="e2d60-103">Återställa en borttagen Office 365-grupp i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="e2d60-103">Restore a deleted Office 365 group in Azure Active Directory</span></span>

<span data-ttu-id="e2d60-104">När du tar bort en Office 365-grupp i Azure Active Directory (AD Azure) är den borttagna gruppen kvarhållna men är inte synliga för 30 dagar från det datum då tas bort.</span><span class="sxs-lookup"><span data-stu-id="e2d60-104">When you delete an Office 365 group in the Azure Active Directory (Azure AD), the deleted group is retained but not visible for 30 days from the deletion date.</span></span> <span data-ttu-id="e2d60-105">Detta är så att gruppen och dess innehåll kan återställas om det behövs.</span><span class="sxs-lookup"><span data-stu-id="e2d60-105">This is so that the group and its contents can be restored if needed.</span></span> <span data-ttu-id="e2d60-106">Den här funktionen är begränsad till Office 365-grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="e2d60-106">This functionality is restricted exclusively to Office 365 groups in Azure AD.</span></span> <span data-ttu-id="e2d60-107">Det är inte tillgänglig för säkerhetsgrupper och distributionsgrupper.</span><span class="sxs-lookup"><span data-stu-id="e2d60-107">It is not available for security groups and distribution groups.</span></span>

> [!NOTE] 
> <span data-ttu-id="e2d60-108">Använd inte `Remove-MsolGroup` eftersom det tar bort gruppen permanent.</span><span class="sxs-lookup"><span data-stu-id="e2d60-108">Don't use `Remove-MsolGroup` because it purges the group permanently.</span></span> <span data-ttu-id="e2d60-109">Använd alltid `Remove-AzureADMSGroup` att ta bort en grupp för O365.</span><span class="sxs-lookup"><span data-stu-id="e2d60-109">Always use `Remove-AzureADMSGroup` to delete an O365 group.</span></span> 

<span data-ttu-id="e2d60-110">De behörigheter som krävs för att återställa en grupp kan vara något av följande:</span><span class="sxs-lookup"><span data-stu-id="e2d60-110">The permissions required to restore a group can be any of the following:</span></span>

<span data-ttu-id="e2d60-111">Roll</span><span class="sxs-lookup"><span data-stu-id="e2d60-111">Role</span></span>  | <span data-ttu-id="e2d60-112">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="e2d60-112">Permissions</span></span> 
--------- | ---------
<span data-ttu-id="e2d60-113">Företagsadministratör, Partner Tier2 stöd och InTune-tjänsten-administratörer</span><span class="sxs-lookup"><span data-stu-id="e2d60-113">Company Administrator, Partner Tier2 support, and InTune Service Admins</span></span> | <span data-ttu-id="e2d60-114">Återställa en borttagen grupp för Office 365</span><span class="sxs-lookup"><span data-stu-id="e2d60-114">Can restore any deleted Office 365 group</span></span> 
<span data-ttu-id="e2d60-115">Stöd för användaren kontoadministratör och Partner Tier1</span><span class="sxs-lookup"><span data-stu-id="e2d60-115">User Account Administrator and Partner Tier1 support</span></span> | <span data-ttu-id="e2d60-116">Återställa alla borttagna Office 365-gruppen utom de som tilldelats till rollen företagsadministratör</span><span class="sxs-lookup"><span data-stu-id="e2d60-116">Can restore any deleted Office 365 group except those assigned to the Company Administrator role</span></span> 
<span data-ttu-id="e2d60-117">Användare</span><span class="sxs-lookup"><span data-stu-id="e2d60-117">User</span></span> | <span data-ttu-id="e2d60-118">Återställa en borttagen Office 365-grupp som de ägs</span><span class="sxs-lookup"><span data-stu-id="e2d60-118">Can restore any deleted Office 365 group that they owned</span></span> 


## <a name="view-the-deleted-office-365-groups-that-are-available-to-restore"></a><span data-ttu-id="e2d60-119">Visa de borttagna Office 365-grupper som är tillgängliga för återställning</span><span class="sxs-lookup"><span data-stu-id="e2d60-119">View the deleted Office 365 groups that are available to restore</span></span>
<span data-ttu-id="e2d60-120">Följande cmdlets kan användas för att visa de borttagna grupperna för att verifiera att den eller de som du är intresserad inte har ännu har permanent bort.</span><span class="sxs-lookup"><span data-stu-id="e2d60-120">The following cmdlets can be used to view the deleted groups to verify that the one or ones you're interested in have not yet been permanently purged.</span></span> <span data-ttu-id="e2d60-121">Dessa cmdletar är en del av den [Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="e2d60-121">These cmdlets are part of the [Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span> <span data-ttu-id="e2d60-122">Mer information om den här modulen kan hittas i den [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) artikel.</span><span class="sxs-lookup"><span data-stu-id="e2d60-122">More information about this module can be found in the [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) article.</span></span>

1.  <span data-ttu-id="e2d60-123">Kör följande cmdlet bort om du vill visa alla Office 365-grupper i din klient som är fortfarande tillgängliga för återställning.</span><span class="sxs-lookup"><span data-stu-id="e2d60-123">Run the following cmdlet to display all deleted Office 365 groups in your tenant that are still available to restore.</span></span>
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  <span data-ttu-id="e2d60-124">Alternativt, om du vet objectID för en specifik grupp (och du får från cmdlet i steg 1) kör du följande cmdlet för att verifiera att den specifika borttagna gruppen inte har ännu permanent rensats.</span><span class="sxs-lookup"><span data-stu-id="e2d60-124">Alternately, if you know the objectID of a specific group (and you can get it from the cmdlet in step 1), run the following cmdlet to verify that the specific deleted group has not yet been permanently purged.</span></span>
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-to-restore-your-deleted-office-365-group"></a><span data-ttu-id="e2d60-125">Så här återställer du din Office 365-grupp som tagits bort</span><span class="sxs-lookup"><span data-stu-id="e2d60-125">How to restore your deleted Office 365 group</span></span>
<span data-ttu-id="e2d60-126">När du har kontrollerat att gruppen är fortfarande tillgänglig för återställning kan du återställa den borttagna gruppen med något av följande steg.</span><span class="sxs-lookup"><span data-stu-id="e2d60-126">Once you have verified that the group is still available to restore, restore the deleted group with one of the following steps.</span></span> <span data-ttu-id="e2d60-127">Om gruppen innehåller dokument, SP platser eller andra beständiga objekt, kan det ta upp till 24 timmar för att återställa en grupp och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="e2d60-127">If the group contains documents, SP sites, or other persistent objects, it might take up to 24 hours to fully restore a group and its contents.</span></span>

1.  <span data-ttu-id="e2d60-128">Kör följande cmdlet om du vill återställa gruppen och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="e2d60-128">Run the following cmdlet to restore the group and its contents.</span></span>
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

<span data-ttu-id="e2d60-129">Alternativt kan du köra följande cmdlet om du vill ta bort den borttagna gruppen.</span><span class="sxs-lookup"><span data-stu-id="e2d60-129">Alternatively, the following cmdlet can be run to permanently remove the deleted group.</span></span>
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a><span data-ttu-id="e2d60-130">Hur vet du detta arbetat?</span><span class="sxs-lookup"><span data-stu-id="e2d60-130">How do you know this worked?</span></span>
<span data-ttu-id="e2d60-131">För att verifiera att du har har återställt en Office 365-grupp, kör de `Get-AzureADGroup –ObjectId <objectId>` för att visa information om gruppen.</span><span class="sxs-lookup"><span data-stu-id="e2d60-131">To verify that you’ve successfully restored an Office 365 group, run the `Get-AzureADGroup –ObjectId <objectId>` cmdlet to display information about the group.</span></span> <span data-ttu-id="e2d60-132">Efter återställningen har begäran slutförts:</span><span class="sxs-lookup"><span data-stu-id="e2d60-132">After the restore request is completed:</span></span>
- <span data-ttu-id="e2d60-133">Gruppen visas i det vänstra navigeringsfältet i Exchange</span><span class="sxs-lookup"><span data-stu-id="e2d60-133">The group will appear in the Left nav bar on Exchange</span></span>
- <span data-ttu-id="e2d60-134">Planera för gruppen visas i Planner</span><span class="sxs-lookup"><span data-stu-id="e2d60-134">The plan for the group will appear in Planner</span></span>
- <span data-ttu-id="e2d60-135">Sharepoint-webbplatser och allt innehåll ska vara tillgängliga</span><span class="sxs-lookup"><span data-stu-id="e2d60-135">Any Sharepoint sites and all of their contents will be available</span></span>
- <span data-ttu-id="e2d60-136">Gruppen kan nås från någon av Exchange-slutpunkter och andra Office 365-arbetsbelastningar som har stöd för Office 365-grupper</span><span class="sxs-lookup"><span data-stu-id="e2d60-136">The group can be accessed from any of the Exchange endpoints and other Office 365 workloads that support Office 365 groups</span></span>


## <a name="next-steps"></a><span data-ttu-id="e2d60-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e2d60-137">Next steps</span></span>
<span data-ttu-id="e2d60-138">Dessa artiklar innehåller ytterligare information om Azure Active Directory-grupper.</span><span class="sxs-lookup"><span data-stu-id="e2d60-138">These articles provide additional information on Azure Active Directory groups.</span></span>

* [<span data-ttu-id="e2d60-139">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="e2d60-139">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="e2d60-140">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="e2d60-140">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="e2d60-141">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="e2d60-141">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="e2d60-142">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="e2d60-142">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="e2d60-143">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="e2d60-143">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
