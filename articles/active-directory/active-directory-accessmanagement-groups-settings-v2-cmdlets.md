---
title: "aaaPowerShell exempel för att hantera grupper i Azure Active Directory | Microsoft Docs"
description: "Den här sidan innehåller PowerShell exempel toohelp du hantera dina grupper i Azure Active Directory"
keywords: Azure AD, Azure Active Directory PowerShell grupper, grupphantering
services: active-directory
documentationcenter: 
author: curtand
manager: femila
editor: 
ms.assetid: 7a5023dc-2727-4c25-8254-b531fc3244ac
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/09/2017
ms.author: curtand
ms.reviewer: rodejo
ms.openlocfilehash: ba049babc436e99a290f20899b3a87bcfa811d9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a><span data-ttu-id="12cce-104">Azure Active Directory version 2-cmdlets för grupphantering</span><span class="sxs-lookup"><span data-stu-id="12cce-104">Azure Active Directory version 2 cmdlets for group management</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="12cce-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="12cce-105">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="12cce-106">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="12cce-106">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="12cce-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="12cce-107">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="12cce-108">Den här artikeln innehåller exempel på hur toouse PowerShell toomanage grupper i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="12cce-108">This article contains examples of how toouse PowerShell toomanage your groups in Azure Active Directory (Azure AD).</span></span>  <span data-ttu-id="12cce-109">Dessutom talar du hur tooget som konfigurerats med hello Azure AD PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="12cce-109">It also tells you how tooget set up with hello Azure AD PowerShell module.</span></span> <span data-ttu-id="12cce-110">Först måste du [hämta hello Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="12cce-110">First, you must [download hello Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="installing-hello-azure-ad-powershell-module"></a><span data-ttu-id="12cce-111">Installera hello Azure AD PowerShell-modul</span><span class="sxs-lookup"><span data-stu-id="12cce-111">Installing hello Azure AD PowerShell module</span></span>
<span data-ttu-id="12cce-112">tooinstall hello Azure AD PowerShell-modulen använder hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="12cce-112">tooinstall hello Azure AD PowerShell module, use hello following commands:</span></span>

    PS C:\Windows\system32> install-module azuread

<span data-ttu-id="12cce-113">tooverify som hello modulen installerades, använder hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="12cce-113">tooverify that hello module was installed, use hello following command:</span></span>

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

<span data-ttu-id="12cce-114">Nu kan du börja använda hello-cmdlets i hello modul.</span><span class="sxs-lookup"><span data-stu-id="12cce-114">Now you can start using hello cmdlets in hello module.</span></span> <span data-ttu-id="12cce-115">En fullständig beskrivning av hello-cmdletar i hello Azure AD-modulen finns toohello online referensdokumentationen för [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="12cce-115">For a full description of hello cmdlets in hello Azure AD module, please refer toohello online reference documentation for [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="connecting-toohello-directory"></a><span data-ttu-id="12cce-116">Ansluta toohello directory</span><span class="sxs-lookup"><span data-stu-id="12cce-116">Connecting toohello directory</span></span>
<span data-ttu-id="12cce-117">Innan du kan börja hantera grupper med hjälp av Azure AD PowerShell-cmdlets, måste du ansluta din PowerShell-session toohello katalog som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="12cce-117">Before you can start managing groups using Azure AD PowerShell cmdlets, you must connect your PowerShell session toohello directory you want toomanage.</span></span> <span data-ttu-id="12cce-118">Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="12cce-118">Use hello following command:</span></span>

    PS C:\Windows\system32> Connect-AzureAD

<span data-ttu-id="12cce-119">hello cmdlet uppmanar dig för hello autentiseringsuppgifter du vill använda toouse tooaccess din katalog.</span><span class="sxs-lookup"><span data-stu-id="12cce-119">hello cmdlet prompts you for hello credentials you want toouse tooaccess your directory.</span></span> <span data-ttu-id="12cce-120">I det här exemplet använder vi karen@drumkit.onmicrosoft.com tooaccess hello demonstration directory.</span><span class="sxs-lookup"><span data-stu-id="12cce-120">In this example, we are using karen@drumkit.onmicrosoft.com tooaccess hello demonstration directory.</span></span> <span data-ttu-id="12cce-121">hello cmdlet returnerar en bekräftelse tooshow hello session ansluten tooyour directory:</span><span class="sxs-lookup"><span data-stu-id="12cce-121">hello cmdlet returns a confirmation tooshow hello session was connected successfully tooyour directory:</span></span>

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

<span data-ttu-id="12cce-122">Nu kan du börja använda hello AzureAD cmdlets toomanage grupper i katalogen.</span><span class="sxs-lookup"><span data-stu-id="12cce-122">Now you can start using hello AzureAD cmdlets toomanage groups in your directory.</span></span>

## <a name="retrieving-groups"></a><span data-ttu-id="12cce-123">Hämtning av grupper</span><span class="sxs-lookup"><span data-stu-id="12cce-123">Retrieving groups</span></span>
<span data-ttu-id="12cce-124">tooretrieve befintliga grupper från din katalog som du kan använda hello Get-AzureADGroups cmdlet.</span><span class="sxs-lookup"><span data-stu-id="12cce-124">tooretrieve existing groups from your directory you can use hello Get-AzureADGroups cmdlet.</span></span> <span data-ttu-id="12cce-125">tooretrieve alla grupper i hello directory, Använd hello cmdlet utan parametrar:</span><span class="sxs-lookup"><span data-stu-id="12cce-125">tooretrieve all groups in hello directory, use hello cmdlet without parameters:</span></span>

    PS C:\Windows\system32> get-azureadgroup

<span data-ttu-id="12cce-126">hello-cmdlet returnerar alla grupper i hello ansluten katalog.</span><span class="sxs-lookup"><span data-stu-id="12cce-126">hello cmdlet returns all groups in hello connected directory.</span></span>

<span data-ttu-id="12cce-127">Du kan använda hello - objectID parametern tooretrieve en specifik grupp som du anger hello grupp objekt-ID:</span><span class="sxs-lookup"><span data-stu-id="12cce-127">You can use hello -objectID parameter tooretrieve a specific group for which you specify hello group’s objectID:</span></span>

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

<span data-ttu-id="12cce-128">hello-cmdlet returnerar nu hello grupp vars objectID matchar hello värdet för hello-parametern som du angav:</span><span class="sxs-lookup"><span data-stu-id="12cce-128">hello cmdlet now returns hello group whose objectID matches hello value of hello parameter you entered:</span></span>

    DeletionTimeStamp            :
    ObjectId                     : e29bae11-4ac0-450c-bc37-6dae8f3da61b
    ObjectType                   : Group
    Description                  :
    DirSyncEnabled               :
    DisplayName                  : Pacific NW Support
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 9bb4139b-60a1-434a-8c0d-7c1f8eee2df9
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

<span data-ttu-id="12cce-129">Du kan söka efter en specifik grupp med hjälp av hello - Filterparametern.</span><span class="sxs-lookup"><span data-stu-id="12cce-129">You can search for a specific group using hello -filter parameter.</span></span> <span data-ttu-id="12cce-130">Den här parametern tar ett filter ODATA-satsen och returnerar alla grupper som matchar hello filter, som i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="12cce-130">This parameter takes an ODATA filter clause and returns all groups that match hello filter, as in hello following example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

> [!NOTE] 
> <span data-ttu-id="12cce-131">Hej AzureAD PowerShell-cmdlets implementera hello OData-frågan som standard.</span><span class="sxs-lookup"><span data-stu-id="12cce-131">hello AzureAD PowerShell cmdlets implement hello OData query standard.</span></span> <span data-ttu-id="12cce-132">Mer information finns i **$filter** i [frågealternativ för OData-system med hello OData-slutpunkt](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span><span class="sxs-lookup"><span data-stu-id="12cce-132">For more information, see **$filter** in [OData system query options using hello OData endpoint](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span></span>

## <a name="creating-groups"></a><span data-ttu-id="12cce-133">Skapa grupper</span><span class="sxs-lookup"><span data-stu-id="12cce-133">Creating groups</span></span>
<span data-ttu-id="12cce-134">toocreate en ny grupp i din katalog, Använd hello ny AzureADGroup cmdlet.</span><span class="sxs-lookup"><span data-stu-id="12cce-134">toocreate a new group in your directory, use hello New-AzureADGroup cmdlet.</span></span> <span data-ttu-id="12cce-135">Denna cmdlet skapar en ny säkerhetsgrupp som heter ”Marknadsföring”:</span><span class="sxs-lookup"><span data-stu-id="12cce-135">This cmdlet creates a new security group called “Marketing":</span></span>

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a><span data-ttu-id="12cce-136">Uppdatering av grupper</span><span class="sxs-lookup"><span data-stu-id="12cce-136">Updating groups</span></span>
<span data-ttu-id="12cce-137">tooupdate en befintlig grupp, Använd hello Set-AzureADGroup cmdlet.</span><span class="sxs-lookup"><span data-stu-id="12cce-137">tooupdate an existing group, use hello Set-AzureADGroup cmdlet.</span></span> <span data-ttu-id="12cce-138">I det här exemplet ändrar vi hello DisplayName-egenskap i hello gruppen ”Intune-administratörer”.</span><span class="sxs-lookup"><span data-stu-id="12cce-138">In this example, we’re changing hello DisplayName property of hello group “Intune Administrators.”</span></span> <span data-ttu-id="12cce-139">Först vi söka efter hello gruppen med hello Get-AzureADGroup cmdlet och filter som använder hello DisplayName attributet:</span><span class="sxs-lookup"><span data-stu-id="12cce-139">First, we’re finding hello group using hello Get-AzureADGroup cmdlet and filter using hello DisplayName attribute:</span></span>

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

<span data-ttu-id="12cce-140">Nu ska ändrar vi hello beskrivning toohello nya egenskapsvärdet ”Intune Enhetsadministratörer”:</span><span class="sxs-lookup"><span data-stu-id="12cce-140">Next, we’re changing hello Description property toohello new value “Intune Device Administrators”:</span></span>

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

<span data-ttu-id="12cce-141">Nu om vi upptäcker hello grupp kan vi se hello beskrivningsegenskapen är uppdaterade tooreflect hello nytt värde:</span><span class="sxs-lookup"><span data-stu-id="12cce-141">Now if we find hello group again, we see hello Description property is updated tooreflect hello new value:</span></span>

    PS C:\Windows\system32> Get-AzureADGroup -Filter "DisplayName eq 'Intune Administrators'"


    DeletionTimeStamp            :
    ObjectId                     : 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df
    ObjectType                   : Group
    Description                  : Intune Device Administrators
    DirSyncEnabled               :
    DisplayName                  : Intune Administrators
    LastDirSyncTime              :
    Mail                         :
    MailEnabled                  : False
    MailNickName                 : 4dd067a0-6515-4f23-968a-cc2ffc2eff5c
    OnPremisesSecurityIdentifier :
    ProvisioningErrors           : {}
    ProxyAddresses               : {}
    SecurityEnabled              : True

## <a name="deleting-groups"></a><span data-ttu-id="12cce-142">Ta bort grupper</span><span class="sxs-lookup"><span data-stu-id="12cce-142">Deleting groups</span></span>
<span data-ttu-id="12cce-143">toodelete grupper från din katalog använder hello Remove-AzureADGroup cmdlet enligt följande:</span><span class="sxs-lookup"><span data-stu-id="12cce-143">toodelete groups from your directory, use hello Remove-AzureADGroup cmdlet as follows:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a><span data-ttu-id="12cce-144">Hantera medlemmar i grupper</span><span class="sxs-lookup"><span data-stu-id="12cce-144">Managing members of groups</span></span>
<span data-ttu-id="12cce-145">Om du behöver tooadd nya medlemmar tooa grupp använda hello Lägg till AzureADGroupMember cmdlet.</span><span class="sxs-lookup"><span data-stu-id="12cce-145">If you need tooadd new members tooa group, use hello Add-AzureADGroupMember cmdlet.</span></span> <span data-ttu-id="12cce-146">Detta kommando lägger till en medlem toohello Intune administratörsgruppen vi använde i hello föregående exempel:</span><span class="sxs-lookup"><span data-stu-id="12cce-146">This command adds a member toohello Intune Administrators group we used in hello previous example:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="12cce-147">hello - parametern objektId är hello ObjectID hello grupp toowhich vi vill tooadd medlem och hello - RefObjectId är hello ObjectID för hello användare vi vill tooadd som en medlem toohello grupp.</span><span class="sxs-lookup"><span data-stu-id="12cce-147">hello -ObjectId parameter is hello ObjectID of hello group toowhich we want tooadd a member, and hello -RefObjectId is hello ObjectID of hello user we want tooadd as a member toohello group.</span></span>

<span data-ttu-id="12cce-148">tooget hello befintliga medlemmar i en grupp, använda hello Get-AzureADGroupMember cmdlet, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="12cce-148">tooget hello existing members of a group, use hello Get-AzureADGroupMember cmdlet, as in this example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

<span data-ttu-id="12cce-149">tooremove hello medlem vi tidigare lagt till toohello grupp, Använd hello Remove-AzureADGroupMember cmdlet, som visas här:</span><span class="sxs-lookup"><span data-stu-id="12cce-149">tooremove hello member we previously added toohello group, use hello Remove-AzureADGroupMember cmdlet, as is shown here:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="12cce-150">tooverify hello grupp membership(s) för en användare, Använd hello väljer AzureADGroupIdsUserIsMemberOf cmdlet.</span><span class="sxs-lookup"><span data-stu-id="12cce-150">tooverify hello group membership(s) of a user, use hello Select-AzureADGroupIdsUserIsMemberOf cmdlet.</span></span> <span data-ttu-id="12cce-151">Den här cmdleten tar eftersom dess parametrar hello ObjectId för hello användare för vilka toocheck hello gruppmedlemskap, och en lista över grupper för vilka toocheck hello medlemskap.</span><span class="sxs-lookup"><span data-stu-id="12cce-151">This cmdlet takes as its parameters hello ObjectId of hello user for which toocheck hello group memberships, and a list of groups for which toocheck hello memberships.</span></span> <span data-ttu-id="12cce-152">hello listan över grupper måste anges i hello form av en komplex variabel av typen ”Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, så vi först skapa en variabel med den typen:</span><span class="sxs-lookup"><span data-stu-id="12cce-152">hello list of groups must be provided in hello form of a complex variable of type “Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, so we first must create a variable with that type:</span></span>

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

<span data-ttu-id="12cce-153">Nu ska ange vi värden för hello groupIds toocheck i hello attributet ”GroupIds” för den här variabeln för komplex:</span><span class="sxs-lookup"><span data-stu-id="12cce-153">Next, we provide values for hello groupIds toocheck in hello attribute “GroupIds” of this complex variable:</span></span>

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

<span data-ttu-id="12cce-154">Nu, om vi vill toocheck hello gruppmedlemskap för en användare med ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea mot hello grupper i $g ska användas:</span><span class="sxs-lookup"><span data-stu-id="12cce-154">Now, if we want toocheck hello group memberships of a user with ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea against hello groups in $g, we should use:</span></span>

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


<span data-ttu-id="12cce-155">hello-värde som returneras är en lista över grupper som användaren är medlem.</span><span class="sxs-lookup"><span data-stu-id="12cce-155">hello value returned is a list of groups of which this user is a member.</span></span> <span data-ttu-id="12cce-156">Du kan också använda den här metoden toocheck kontakter, grupper eller tjänstens huvudnamn medlemskap för en angiven lista med grupper, med hjälp av Select-AzureADGroupIdsContactIsMemberOf, Välj AzureADGroupIdsGroupIsMemberOf eller Välj AzureADGroupIdsServicePrincipalIsMemberOf</span><span class="sxs-lookup"><span data-stu-id="12cce-156">You can also apply this method toocheck Contacts, Groups or Service Principals membership for a given list of groups, using Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf or Select-AzureADGroupIdsServicePrincipalIsMemberOf</span></span>

## <a name="managing-owners-of-groups"></a><span data-ttu-id="12cce-157">Hantera ägare av grupper</span><span class="sxs-lookup"><span data-stu-id="12cce-157">Managing owners of groups</span></span>
<span data-ttu-id="12cce-158">tooadd ägare tooa grupp, Använd hello Lägg till AzureADGroupOwner cmdlet:</span><span class="sxs-lookup"><span data-stu-id="12cce-158">tooadd owners tooa group, use hello Add-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="12cce-159">hello - parametern objektId är hello ObjectID hello grupp toowhich vi vill tooadd en ägare och hello - RefObjectId är hello ObjectID för hello användare vi vill tooadd som ägare av hello grupp.</span><span class="sxs-lookup"><span data-stu-id="12cce-159">hello -ObjectId parameter is hello ObjectID of hello group toowhich we want tooadd an owner, and hello -RefObjectId is hello ObjectID of hello user we want tooadd as an owner of hello group.</span></span>

<span data-ttu-id="12cce-160">tooretrieve hello ägare för en grupp använder hello Get-AzureADGroupOwner cmdlet:</span><span class="sxs-lookup"><span data-stu-id="12cce-160">tooretrieve hello owners of a group, use hello Get-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

<span data-ttu-id="12cce-161">hello-cmdlet returnerar hello listan över ägare för hello angiven grupp:</span><span class="sxs-lookup"><span data-stu-id="12cce-161">hello cmdlet returns hello list of owners for hello specified group:</span></span>

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

<span data-ttu-id="12cce-162">Om du vill tooremove ägare från en grupp, använder du hello Remove-AzureADGroupOwner cmdlet:</span><span class="sxs-lookup"><span data-stu-id="12cce-162">If you want tooremove an owner from a group, use hello Remove-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a><span data-ttu-id="12cce-163">Reserverade alias</span><span class="sxs-lookup"><span data-stu-id="12cce-163">Reserved aliases</span></span> 
<span data-ttu-id="12cce-164">När en grupp har skapats, kan vissa slutpunkter hello slutanvändaren toospecify en mailNickname eller alias toobe som används som en del av hello e-postadress hello grupp.</span><span class="sxs-lookup"><span data-stu-id="12cce-164">When a group is created, certain endpoints allow hello end user toospecify a mailNickname or alias toobe used as part of hello email address of hello group.</span></span> <span data-ttu-id="12cce-165">Grupper med hello följande mycket Privilegierade e-alias kan bara skapas av en global administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="12cce-165">Groups with hello following highly privileged email aliases can only be created by an Azure AD global administrator.</span></span> 
  
* <span data-ttu-id="12cce-166">missbruk</span><span class="sxs-lookup"><span data-stu-id="12cce-166">abuse</span></span> 
* <span data-ttu-id="12cce-167">Admin</span><span class="sxs-lookup"><span data-stu-id="12cce-167">admin</span></span> 
* <span data-ttu-id="12cce-168">Administratören</span><span class="sxs-lookup"><span data-stu-id="12cce-168">administrator</span></span> 
* <span data-ttu-id="12cce-169">hostmaster</span><span class="sxs-lookup"><span data-stu-id="12cce-169">hostmaster</span></span> 
* <span data-ttu-id="12cce-170">majordomo</span><span class="sxs-lookup"><span data-stu-id="12cce-170">majordomo</span></span> 
* <span data-ttu-id="12cce-171">postmaster</span><span class="sxs-lookup"><span data-stu-id="12cce-171">postmaster</span></span> 
* <span data-ttu-id="12cce-172">rot</span><span class="sxs-lookup"><span data-stu-id="12cce-172">root</span></span> 
* <span data-ttu-id="12cce-173">Skydda</span><span class="sxs-lookup"><span data-stu-id="12cce-173">secure</span></span> 
* <span data-ttu-id="12cce-174">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="12cce-174">security</span></span> 
* <span data-ttu-id="12cce-175">SSL-admin</span><span class="sxs-lookup"><span data-stu-id="12cce-175">ssl-admin</span></span> 
* <span data-ttu-id="12cce-176">webbadministratör</span><span class="sxs-lookup"><span data-stu-id="12cce-176">webmaster</span></span> 

## <a name="next-steps"></a><span data-ttu-id="12cce-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="12cce-177">Next steps</span></span>
<span data-ttu-id="12cce-178">Du kan hitta mer Azure Active Directory PowerShell-dokumentationen på [Azure Active Directory-cmdletarna](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="12cce-178">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

* [<span data-ttu-id="12cce-179">Hantera åtkomst tooresources med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="12cce-179">Managing access tooresources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="12cce-180">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="12cce-180">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
