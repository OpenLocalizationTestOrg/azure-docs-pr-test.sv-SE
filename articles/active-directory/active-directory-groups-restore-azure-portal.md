---
title: aaaRestore en borttagen Office 365-grupp i Azure Active Directory | Microsoft Docs
description: "Hur toorestore en borttagen grupp, visa återställningsbara grupper och permamnently tar bort en grupp i Azure Active Directory"
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
ms.openlocfilehash: 4e6650d64aa19f0c909f1c511f48681de8032f63
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-a-deleted-office-365-group-in-azure-active-directory"></a><span data-ttu-id="be5ec-103">Återställa en borttagen Office 365-grupp i Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="be5ec-103">Restore a deleted Office 365 group in Azure Active Directory</span></span>

<span data-ttu-id="be5ec-104">När du tar bort en Office 365-grupp i hello Azure Active Directory (AD Azure) hello bort grupp är kvarhållna men är inte synliga i 30 dagar från hello borttagning datum.</span><span class="sxs-lookup"><span data-stu-id="be5ec-104">When you delete an Office 365 group in hello Azure Active Directory (Azure AD), hello deleted group is retained but not visible for 30 days from hello deletion date.</span></span> <span data-ttu-id="be5ec-105">Detta är så att hello grupp och dess innehåll kan återställas om det behövs.</span><span class="sxs-lookup"><span data-stu-id="be5ec-105">This is so that hello group and its contents can be restored if needed.</span></span> <span data-ttu-id="be5ec-106">Den här funktionen är begränsat enbart tooOffice 365 grupper i Azure AD.</span><span class="sxs-lookup"><span data-stu-id="be5ec-106">This functionality is restricted exclusively tooOffice 365 groups in Azure AD.</span></span> <span data-ttu-id="be5ec-107">Det är inte tillgänglig för säkerhetsgrupper och distributionsgrupper.</span><span class="sxs-lookup"><span data-stu-id="be5ec-107">It is not available for security groups and distribution groups.</span></span>

> [!NOTE] 
> <span data-ttu-id="be5ec-108">Använd inte `Remove-MsolGroup` eftersom det tar bort hello grupp permanent.</span><span class="sxs-lookup"><span data-stu-id="be5ec-108">Don't use `Remove-MsolGroup` because it purges hello group permanently.</span></span> <span data-ttu-id="be5ec-109">Använd alltid `Remove-AzureADMSGroup` toodelete en O365-gruppen.</span><span class="sxs-lookup"><span data-stu-id="be5ec-109">Always use `Remove-AzureADMSGroup` toodelete an O365 group.</span></span> 

<span data-ttu-id="be5ec-110">hello behörigheter krävs toorestore en grupp kan vara något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="be5ec-110">hello permissions required toorestore a group can be any of hello following:</span></span>

<span data-ttu-id="be5ec-111">Roll</span><span class="sxs-lookup"><span data-stu-id="be5ec-111">Role</span></span>  | <span data-ttu-id="be5ec-112">Behörigheter</span><span class="sxs-lookup"><span data-stu-id="be5ec-112">Permissions</span></span> 
--------- | ---------
<span data-ttu-id="be5ec-113">Företagsadministratör, Partner Tier2 stöd och InTune-tjänsten-administratörer</span><span class="sxs-lookup"><span data-stu-id="be5ec-113">Company Administrator, Partner Tier2 support, and InTune Service Admins</span></span> | <span data-ttu-id="be5ec-114">Återställa en borttagen grupp för Office 365</span><span class="sxs-lookup"><span data-stu-id="be5ec-114">Can restore any deleted Office 365 group</span></span> 
<span data-ttu-id="be5ec-115">Stöd för användaren kontoadministratör och Partner Tier1</span><span class="sxs-lookup"><span data-stu-id="be5ec-115">User Account Administrator and Partner Tier1 support</span></span> | <span data-ttu-id="be5ec-116">Återställa alla borttagna Office 365-gruppen utom de tilldelade toohello företagsadministratör roll</span><span class="sxs-lookup"><span data-stu-id="be5ec-116">Can restore any deleted Office 365 group except those assigned toohello Company Administrator role</span></span> 
<span data-ttu-id="be5ec-117">Användare</span><span class="sxs-lookup"><span data-stu-id="be5ec-117">User</span></span> | <span data-ttu-id="be5ec-118">Återställa en borttagen Office 365-grupp som de ägs</span><span class="sxs-lookup"><span data-stu-id="be5ec-118">Can restore any deleted Office 365 group that they owned</span></span> 


## <a name="view-hello-deleted-office-365-groups-that-are-available-toorestore"></a><span data-ttu-id="be5ec-119">Visa hello bort Office 365-grupper som är tillgängliga toorestore</span><span class="sxs-lookup"><span data-stu-id="be5ec-119">View hello deleted Office 365 groups that are available toorestore</span></span>
<span data-ttu-id="be5ec-120">hello följande cmdlets kan vara används tooview hello bort grupper tooverify som hello något eller de som du är intresserad har inte ännu har permanent bort.</span><span class="sxs-lookup"><span data-stu-id="be5ec-120">hello following cmdlets can be used tooview hello deleted groups tooverify that hello one or ones you're interested in have not yet been permanently purged.</span></span> <span data-ttu-id="be5ec-121">Dessa cmdletar är en del av hello [Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="be5ec-121">These cmdlets are part of hello [Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span> <span data-ttu-id="be5ec-122">Mer information om den här modulen kan hittas i hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) artikel.</span><span class="sxs-lookup"><span data-stu-id="be5ec-122">More information about this module can be found in hello [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0) article.</span></span>

1.  <span data-ttu-id="be5ec-123">Kör följande cmdlet toodisplay alla bort Office 365-grupper i din klient som är fortfarande tillgängliga toorestore hello.</span><span class="sxs-lookup"><span data-stu-id="be5ec-123">Run hello following cmdlet toodisplay all deleted Office 365 groups in your tenant that are still available toorestore.</span></span>
  ```
  Get-AzureADMSDeletedGroup
  ```

2.  <span data-ttu-id="be5ec-124">Alternativt, om du vet hello objectID för en specifik grupp (och du får från hello cmdlet i steg 1) kör hello följande cmdlet tooverify som hello specifika borttagna gruppen har inte ännu permanent rensats.</span><span class="sxs-lookup"><span data-stu-id="be5ec-124">Alternately, if you know hello objectID of a specific group (and you can get it from hello cmdlet in step 1), run hello following cmdlet tooverify that hello specific deleted group has not yet been permanently purged.</span></span>
  ```
  Get-AzureADMSDeletedGroup –Id <objectId>
  ```



## <a name="how-toorestore-your-deleted-office-365-group"></a><span data-ttu-id="be5ec-125">Hur toorestore din borttagna Office 365-grupp</span><span class="sxs-lookup"><span data-stu-id="be5ec-125">How toorestore your deleted Office 365 group</span></span>
<span data-ttu-id="be5ec-126">När du har kontrollerat att hello är fortfarande tillgängliga toorestore, återställa hello bort grupp med något av följande hello.</span><span class="sxs-lookup"><span data-stu-id="be5ec-126">Once you have verified that hello group is still available toorestore, restore hello deleted group with one of hello following steps.</span></span> <span data-ttu-id="be5ec-127">Om hello grupp innehåller dokument, SP platser eller andra beständiga objekt, kan det ta upp too24 timmar toofully Återställ en grupp och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="be5ec-127">If hello group contains documents, SP sites, or other persistent objects, it might take up too24 hours toofully restore a group and its contents.</span></span>

1.  <span data-ttu-id="be5ec-128">Kör hello följande cmdlet toorestore hello grupp och dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="be5ec-128">Run hello following cmdlet toorestore hello group and its contents.</span></span>
  
  ```
  Restore-AzureADMSDeletedDirectoryObject –Id <objectId>
  ``` 

<span data-ttu-id="be5ec-129">Du kan också kan hello följande cmdlet köras toopermanently ta bort hello bort grupp.</span><span class="sxs-lookup"><span data-stu-id="be5ec-129">Alternatively, hello following cmdlet can be run toopermanently remove hello deleted group.</span></span>
  ```
  Remove-AzureADMSDeletedDirectoryObject –Id <objectId>
  ```

## <a name="how-do-you-know-this-worked"></a><span data-ttu-id="be5ec-130">Hur vet du detta arbetat?</span><span class="sxs-lookup"><span data-stu-id="be5ec-130">How do you know this worked?</span></span>
<span data-ttu-id="be5ec-131">tooverify som du har har återställt en Office 365-grupp, kör hello `Get-AzureADGroup –ObjectId <objectId>` cmdlet toodisplay information om hello-gruppen.</span><span class="sxs-lookup"><span data-stu-id="be5ec-131">tooverify that you’ve successfully restored an Office 365 group, run hello `Get-AzureADGroup –ObjectId <objectId>` cmdlet toodisplay information about hello group.</span></span> <span data-ttu-id="be5ec-132">Begäran har slutförts efter hello återställning:</span><span class="sxs-lookup"><span data-stu-id="be5ec-132">After hello restore request is completed:</span></span>
- <span data-ttu-id="be5ec-133">hello gruppen visas i hello vänstra navigeringsfältet i Exchange</span><span class="sxs-lookup"><span data-stu-id="be5ec-133">hello group will appear in hello Left nav bar on Exchange</span></span>
- <span data-ttu-id="be5ec-134">hello plan för hello gruppen visas i Planner</span><span class="sxs-lookup"><span data-stu-id="be5ec-134">hello plan for hello group will appear in Planner</span></span>
- <span data-ttu-id="be5ec-135">Sharepoint-webbplatser och allt innehåll ska vara tillgängliga</span><span class="sxs-lookup"><span data-stu-id="be5ec-135">Any Sharepoint sites and all of their contents will be available</span></span>
- <span data-ttu-id="be5ec-136">hello gruppen kan nås från alla hello Exchange slutpunkter och andra Office 365-arbetsbelastningar som har stöd för Office 365-grupper</span><span class="sxs-lookup"><span data-stu-id="be5ec-136">hello group can be accessed from any of hello Exchange endpoints and other Office 365 workloads that support Office 365 groups</span></span>


## <a name="next-steps"></a><span data-ttu-id="be5ec-137">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="be5ec-137">Next steps</span></span>
<span data-ttu-id="be5ec-138">Dessa artiklar innehåller ytterligare information om Azure Active Directory-grupper.</span><span class="sxs-lookup"><span data-stu-id="be5ec-138">These articles provide additional information on Azure Active Directory groups.</span></span>

* [<span data-ttu-id="be5ec-139">Se befintliga grupper</span><span class="sxs-lookup"><span data-stu-id="be5ec-139">See existing groups</span></span>](active-directory-groups-view-azure-portal.md)
* [<span data-ttu-id="be5ec-140">Hantera inställningar för en grupp</span><span class="sxs-lookup"><span data-stu-id="be5ec-140">Manage settings of a group</span></span>](active-directory-groups-settings-azure-portal.md)
* [<span data-ttu-id="be5ec-141">Hantera medlemmar i en grupp</span><span class="sxs-lookup"><span data-stu-id="be5ec-141">Manage members of a group</span></span>](active-directory-groups-members-azure-portal.md)
* [<span data-ttu-id="be5ec-142">Hantera medlemskap i en grupp</span><span class="sxs-lookup"><span data-stu-id="be5ec-142">Manage memberships of a group</span></span>](active-directory-groups-membership-azure-portal.md)
* [<span data-ttu-id="be5ec-143">Hantera dynamiska regler för användare i en grupp</span><span class="sxs-lookup"><span data-stu-id="be5ec-143">Manage dynamic rules for users in a group</span></span>](active-directory-groups-dynamic-membership-azure-portal.md)
