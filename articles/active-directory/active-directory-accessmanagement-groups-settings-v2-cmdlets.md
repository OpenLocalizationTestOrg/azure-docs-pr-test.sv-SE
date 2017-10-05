---
title: "PowerShell-exempel för att hantera grupper i Azure Active Directory | Microsoft Docs"
description: "Den här sidan innehåller PowerShell-exempel som hjälper dig att hantera grupper i Azure Active Directory"
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
ms.openlocfilehash: a81820bc778c26f6e8051e2817ebd2b9c24b697a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="azure-active-directory-version-2-cmdlets-for-group-management"></a><span data-ttu-id="a1f71-104">Azure Active Directory version 2-cmdlets för grupphantering</span><span class="sxs-lookup"><span data-stu-id="a1f71-104">Azure Active Directory version 2 cmdlets for group management</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="a1f71-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="a1f71-105">Azure portal</span></span>](active-directory-groups-create-azure-portal.md)
> * [<span data-ttu-id="a1f71-106">Klassisk Azure-portal</span><span class="sxs-lookup"><span data-stu-id="a1f71-106">Azure classic portal</span></span>](active-directory-accessmanagement-manage-groups.md)
> * [<span data-ttu-id="a1f71-107">PowerShell</span><span class="sxs-lookup"><span data-stu-id="a1f71-107">PowerShell</span></span>](active-directory-accessmanagement-groups-settings-v2-cmdlets.md)
>
>

<span data-ttu-id="a1f71-108">Den här artikeln innehåller exempel på hur du använder PowerShell för att hantera grupper i Azure Active Directory (AD Azure).</span><span class="sxs-lookup"><span data-stu-id="a1f71-108">This article contains examples of how to use PowerShell to manage your groups in Azure Active Directory (Azure AD).</span></span>  <span data-ttu-id="a1f71-109">Den också förklarar hur du att börja arbeta med Azure AD PowerShell-modulen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-109">It also tells you how to get set up with the Azure AD PowerShell module.</span></span> <span data-ttu-id="a1f71-110">Först måste du [ladda ned Azure AD PowerShell-modulen](https://www.powershellgallery.com/packages/AzureAD/).</span><span class="sxs-lookup"><span data-stu-id="a1f71-110">First, you must [download the Azure AD PowerShell module](https://www.powershellgallery.com/packages/AzureAD/).</span></span>

## <a name="installing-the-azure-ad-powershell-module"></a><span data-ttu-id="a1f71-111">Installera Azure AD PowerShell-modul</span><span class="sxs-lookup"><span data-stu-id="a1f71-111">Installing the Azure AD PowerShell module</span></span>
<span data-ttu-id="a1f71-112">Om du vill installera Azure AD PowerShell-modulen använder du följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="a1f71-112">To install the Azure AD PowerShell module, use the following commands:</span></span>

    PS C:\Windows\system32> install-module azuread

<span data-ttu-id="a1f71-113">Om du vill verifiera att modulen har installerats, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a1f71-113">To verify that the module was installed, use the following command:</span></span>

    PS C:\Windows\system32> get-module azuread

    ModuleType Version      Name                                ExportedCommands
    ---------- ---------    ----                                ----------------
    Binary     2.0.0.115    azuread                      {Add-AzureADAdministrati...}

<span data-ttu-id="a1f71-114">Nu kan du börja använda cmdlet: arna i modulen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-114">Now you can start using the cmdlets in the module.</span></span> <span data-ttu-id="a1f71-115">En fullständig beskrivning av cmdletar i Azure AD-modulen finns i dokumentationen för online-referens för [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="a1f71-115">For a full description of the cmdlets in the Azure AD module, please refer to the online reference documentation for [Azure Active Directory PowerShell Version 2](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

## <a name="connecting-to-the-directory"></a><span data-ttu-id="a1f71-116">Ansluter till katalogen</span><span class="sxs-lookup"><span data-stu-id="a1f71-116">Connecting to the directory</span></span>
<span data-ttu-id="a1f71-117">Innan du kan börja hantera grupper med hjälp av Azure AD PowerShell-cmdlets, måste du ansluta din PowerShell-session till den katalog som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="a1f71-117">Before you can start managing groups using Azure AD PowerShell cmdlets, you must connect your PowerShell session to the directory you want to manage.</span></span> <span data-ttu-id="a1f71-118">Ange följande kommando:</span><span class="sxs-lookup"><span data-stu-id="a1f71-118">Use the following command:</span></span>

    PS C:\Windows\system32> Connect-AzureAD

<span data-ttu-id="a1f71-119">Cmdlet efterfrågar autentiseringsuppgifter som du vill använda för åtkomst till din katalog.</span><span class="sxs-lookup"><span data-stu-id="a1f71-119">The cmdlet prompts you for the credentials you want to use to access your directory.</span></span> <span data-ttu-id="a1f71-120">I det här exemplet använder karen@drumkit.onmicrosoft.com åtkomst till katalogen demonstrationen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-120">In this example, we are using karen@drumkit.onmicrosoft.com to access the demonstration directory.</span></span> <span data-ttu-id="a1f71-121">Om du vill visa sessionen var ansluten till din katalog returnerar cmdleten:</span><span class="sxs-lookup"><span data-stu-id="a1f71-121">The cmdlet returns a confirmation to show the session was connected successfully to your directory:</span></span>

    Account                       Environment Tenant
    -------                       ----------- ------
    Karen@drumkit.onmicrosoft.com AzureCloud  85b5ff1e-0402-400c-9e3c-0f…

<span data-ttu-id="a1f71-122">Nu kan du börja använda AzureAD-cmdlets för att hantera grupper i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-122">Now you can start using the AzureAD cmdlets to manage groups in your directory.</span></span>

## <a name="retrieving-groups"></a><span data-ttu-id="a1f71-123">Hämtning av grupper</span><span class="sxs-lookup"><span data-stu-id="a1f71-123">Retrieving groups</span></span>
<span data-ttu-id="a1f71-124">Du kan använda cmdlet Get-AzureADGroups för att hämta befintliga grupper från din katalog.</span><span class="sxs-lookup"><span data-stu-id="a1f71-124">To retrieve existing groups from your directory you can use the Get-AzureADGroups cmdlet.</span></span> <span data-ttu-id="a1f71-125">Om du vill hämta alla grupper i katalogen, använder du cmdlet utan parametrar:</span><span class="sxs-lookup"><span data-stu-id="a1f71-125">To retrieve all groups in the directory, use the cmdlet without parameters:</span></span>

    PS C:\Windows\system32> get-azureadgroup

<span data-ttu-id="a1f71-126">Cmdleten returnerar alla grupper i katalogen anslutna.</span><span class="sxs-lookup"><span data-stu-id="a1f71-126">The cmdlet returns all groups in the connected directory.</span></span>

<span data-ttu-id="a1f71-127">Du kan använda parametern - objekt-ID för att hämta en specifik grupp som du anger gruppens objectID:</span><span class="sxs-lookup"><span data-stu-id="a1f71-127">You can use the -objectID parameter to retrieve a specific group for which you specify the group’s objectID:</span></span>

    PS C:\Windows\system32> get-azureadgroup -ObjectId e29bae11-4ac0-450c-bc37-6dae8f3da61b

<span data-ttu-id="a1f71-128">Cmdleten returnerar nu gruppen vars objectID överensstämmer med värdet för parametern du angav:</span><span class="sxs-lookup"><span data-stu-id="a1f71-128">The cmdlet now returns the group whose objectID matches the value of the parameter you entered:</span></span>

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

<span data-ttu-id="a1f71-129">Du kan söka efter en specifik grupp med hjälp av Filterparametern -.</span><span class="sxs-lookup"><span data-stu-id="a1f71-129">You can search for a specific group using the -filter parameter.</span></span> <span data-ttu-id="a1f71-130">Den här parametern tar ett filter ODATA-satsen och returnerar alla grupper som matchar filtret, som i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="a1f71-130">This parameter takes an ODATA filter clause and returns all groups that match the filter, as in the following example:</span></span>

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
> <span data-ttu-id="a1f71-131">AzureAD PowerShell-cmdletar följer OData-frågan.</span><span class="sxs-lookup"><span data-stu-id="a1f71-131">The AzureAD PowerShell cmdlets implement the OData query standard.</span></span> <span data-ttu-id="a1f71-132">Mer information finns i **$filter** i [frågealternativ för OData-system med OData-slutpunkt](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span><span class="sxs-lookup"><span data-stu-id="a1f71-132">For more information, see **$filter** in [OData system query options using the OData endpoint](https://msdn.microsoft.com/library/gg309461.aspx#BKMK_filter).</span></span>

## <a name="creating-groups"></a><span data-ttu-id="a1f71-133">Skapa grupper</span><span class="sxs-lookup"><span data-stu-id="a1f71-133">Creating groups</span></span>
<span data-ttu-id="a1f71-134">Använd cmdleten New-AzureADGroup om du vill skapa en ny grupp i katalogen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-134">To create a new group in your directory, use the New-AzureADGroup cmdlet.</span></span> <span data-ttu-id="a1f71-135">Denna cmdlet skapar en ny säkerhetsgrupp som heter ”Marknadsföring”:</span><span class="sxs-lookup"><span data-stu-id="a1f71-135">This cmdlet creates a new security group called “Marketing":</span></span>

    PS C:\Windows\system32> New-AzureADGroup -Description "Marketing" -DisplayName "Marketing" -MailEnabled $false -SecurityEnabled $true -MailNickName "Marketing"

## <a name="updating-groups"></a><span data-ttu-id="a1f71-136">Uppdatering av grupper</span><span class="sxs-lookup"><span data-stu-id="a1f71-136">Updating groups</span></span>
<span data-ttu-id="a1f71-137">Använd cmdlet Set-AzureADGroup om du vill uppdatera en befintlig grupp.</span><span class="sxs-lookup"><span data-stu-id="a1f71-137">To update an existing group, use the Set-AzureADGroup cmdlet.</span></span> <span data-ttu-id="a1f71-138">I det här exemplet ändrar vi DisplayName-egenskap i gruppen ”Intune-administratörer”.</span><span class="sxs-lookup"><span data-stu-id="a1f71-138">In this example, we’re changing the DisplayName property of the group “Intune Administrators.”</span></span> <span data-ttu-id="a1f71-139">Först måste vi är att hitta gruppen med hjälp av cmdleten Get-AzureADGroup och filter som använder attributet visningsnamn:</span><span class="sxs-lookup"><span data-stu-id="a1f71-139">First, we’re finding the group using the Get-AzureADGroup cmdlet and filter using the DisplayName attribute:</span></span>

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

<span data-ttu-id="a1f71-140">Nu ska ändrar vi beskrivningsegenskapen till det nya värdet ”Intune Enhetsadministratörer”:</span><span class="sxs-lookup"><span data-stu-id="a1f71-140">Next, we’re changing the Description property to the new value “Intune Device Administrators”:</span></span>

    PS C:\Windows\system32> Set-AzureADGroup -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -Description "Intune Device Administrators"

<span data-ttu-id="a1f71-141">Nu om vi upptäcker gruppen kan se beskrivningsegenskapen har uppdaterats för att återspegla det nya värdet:</span><span class="sxs-lookup"><span data-stu-id="a1f71-141">Now if we find the group again, we see the Description property is updated to reflect the new value:</span></span>

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

## <a name="deleting-groups"></a><span data-ttu-id="a1f71-142">Ta bort grupper</span><span class="sxs-lookup"><span data-stu-id="a1f71-142">Deleting groups</span></span>
<span data-ttu-id="a1f71-143">Om du vill ta bort grupper från katalogen, använder du cmdleten Remove-AzureADGroup enligt följande:</span><span class="sxs-lookup"><span data-stu-id="a1f71-143">To delete groups from your directory, use the Remove-AzureADGroup cmdlet as follows:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroup -ObjectId b11ca53e-07cc-455d-9a89-1fe3ab24566b

## <a name="managing-members-of-groups"></a><span data-ttu-id="a1f71-144">Hantera medlemmar i grupper</span><span class="sxs-lookup"><span data-stu-id="a1f71-144">Managing members of groups</span></span>
<span data-ttu-id="a1f71-145">Använd cmdleten Add-AzureADGroupMember om du behöver lägga till nya medlemmar i en grupp.</span><span class="sxs-lookup"><span data-stu-id="a1f71-145">If you need to add new members to a group, use the Add-AzureADGroupMember cmdlet.</span></span> <span data-ttu-id="a1f71-146">Detta kommando lägger till en medlem i gruppen Intune-administratörer vi använde i det förra exemplet:</span><span class="sxs-lookup"><span data-stu-id="a1f71-146">This command adds a member to the Intune Administrators group we used in the previous example:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="a1f71-147">Parametern - ObjectId är ObjectID i gruppen som vi vill lägga till en medlem och RefObjectId - är objekt-ID för den användare som vi vill lägga till som en medlem i gruppen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-147">The -ObjectId parameter is the ObjectID of the group to which we want to add a member, and the -RefObjectId is the ObjectID of the user we want to add as a member to the group.</span></span>

<span data-ttu-id="a1f71-148">Använd cmdleten Get-AzureADGroupMember som i följande exempel för att få de befintliga medlemmarna i en grupp:</span><span class="sxs-lookup"><span data-stu-id="a1f71-148">To get the existing members of a group, use the Get-AzureADGroupMember cmdlet, as in this example:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          72cd4bbd-2594-40a2-935c-016f3cfeeeea User
                          8120cc36-64b4-4080-a9e8-23aa98e8b34f User

<span data-ttu-id="a1f71-149">Ta bort medlemmen som vi tidigare har lagts till i gruppen med cmdleten Remove-AzureADGroupMember som visas här:</span><span class="sxs-lookup"><span data-stu-id="a1f71-149">To remove the member we previously added to the group, use the Remove-AzureADGroupMember cmdlet, as is shown here:</span></span>

    PS C:\Windows\system32> Remove-AzureADGroupMember -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -MemberId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="a1f71-150">Använd cmdleten väljer AzureADGroupIdsUserIsMemberOf för att verifiera grupp membership(s) för en användare.</span><span class="sxs-lookup"><span data-stu-id="a1f71-150">To verify the group membership(s) of a user, use the Select-AzureADGroupIdsUserIsMemberOf cmdlet.</span></span> <span data-ttu-id="a1f71-151">Denna cmdlet tar som dess parametrar ObjectId för den användare som du vill kontrollera gruppmedlemskap och en lista över grupper som du vill kontrollera medlemskap.</span><span class="sxs-lookup"><span data-stu-id="a1f71-151">This cmdlet takes as its parameters the ObjectId of the user for which to check the group memberships, and a list of groups for which to check the memberships.</span></span> <span data-ttu-id="a1f71-152">I listan över grupper måste vara angivna i form av en komplex variabel av typen ”Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, så vi först skapa en variabel med den typen:</span><span class="sxs-lookup"><span data-stu-id="a1f71-152">The list of groups must be provided in the form of a complex variable of type “Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck”, so we first must create a variable with that type:</span></span>

    PS C:\Windows\system32> $g = new-object Microsoft.Open.AzureAD.Model.GroupIdsForMembershipCheck

<span data-ttu-id="a1f71-153">Nu ska ange vi värden för groupIds att checka in attributet ”GroupIds” för den här variabeln för komplex:</span><span class="sxs-lookup"><span data-stu-id="a1f71-153">Next, we provide values for the groupIds to check in the attribute “GroupIds” of this complex variable:</span></span>

    PS C:\Windows\system32> $g.GroupIds = "b11ca53e-07cc-455d-9a89-1fe3ab24566b", "31f1ff6c-d48c-4f8a-b2e1-abca7fd399df"

<span data-ttu-id="a1f71-154">Nu, om vi vill kontrollera gruppmedlemskap för en användare med ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea mot grupper i $g ska användas:</span><span class="sxs-lookup"><span data-stu-id="a1f71-154">Now, if we want to check the group memberships of a user with ObjectID 72cd4bbd-2594-40a2-935c-016f3cfeeeea against the groups in $g, we should use:</span></span>

    PS C:\Windows\system32> Select-AzureADGroupIdsUserIsMemberOf -ObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea -GroupIdsForMembershipCheck $g

    OdataMetadata                                                                                                 Value
    -------------                                                                                                  -----
    https://graph.windows.net/85b5ff1e-0402-400c-9e3c-0f9e965325d1/$metadata#Collection(Edm.String)             {31f1ff6c-d48c-4f8a-b2e1-abca7fd399df}


<span data-ttu-id="a1f71-155">Värdet som returneras är en lista över grupper som användaren är medlem.</span><span class="sxs-lookup"><span data-stu-id="a1f71-155">The value returned is a list of groups of which this user is a member.</span></span> <span data-ttu-id="a1f71-156">Du kan också använda den här metoden om du vill kontrollera kontakter, grupper eller tjänstens huvudnamn medlemskap för en angiven lista med grupper, Välj AzureADGroupIdsContactIsMemberOf, Välj AzureADGroupIdsGroupIsMemberOf eller välj AzureADGroupIdsServicePrincipalIsMemberOf</span><span class="sxs-lookup"><span data-stu-id="a1f71-156">You can also apply this method to check Contacts, Groups or Service Principals membership for a given list of groups, using Select-AzureADGroupIdsContactIsMemberOf, Select-AzureADGroupIdsGroupIsMemberOf or Select-AzureADGroupIdsServicePrincipalIsMemberOf</span></span>

## <a name="managing-owners-of-groups"></a><span data-ttu-id="a1f71-157">Hantera ägare av grupper</span><span class="sxs-lookup"><span data-stu-id="a1f71-157">Managing owners of groups</span></span>
<span data-ttu-id="a1f71-158">Om du vill lägga till ägare till en grupp, använder du Lägg till AzureADGroupOwner-cmdlet:</span><span class="sxs-lookup"><span data-stu-id="a1f71-158">To add owners to a group, use the Add-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Add-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -RefObjectId 72cd4bbd-2594-40a2-935c-016f3cfeeeea

<span data-ttu-id="a1f71-159">Parametern - ObjectId är ObjectID i gruppen som vi vill lägga till en ägare och RefObjectId - är objekt-ID för den användare som vi vill lägga till som en ägare av gruppen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-159">The -ObjectId parameter is the ObjectID of the group to which we want to add an owner, and the -RefObjectId is the ObjectID of the user we want to add as an owner of the group.</span></span>

<span data-ttu-id="a1f71-160">Använd cmdleten Get-AzureADGroupOwner för att hämta ägare för en grupp:</span><span class="sxs-lookup"><span data-stu-id="a1f71-160">To retrieve the owners of a group, use the Get-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> Get-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df

<span data-ttu-id="a1f71-161">Cmdleten returnerar listan över ägare för den angivna gruppen:</span><span class="sxs-lookup"><span data-stu-id="a1f71-161">The cmdlet returns the list of owners for the specified group:</span></span>

    DeletionTimeStamp ObjectId                             ObjectType
    ----------------- --------                             ----------
                          e831b3fd-77c9-49c7-9fca-de43e109ef67 User

<span data-ttu-id="a1f71-162">Använd cmdleten Remove-AzureADGroupOwner om du vill ta bort en ägare från en grupp:</span><span class="sxs-lookup"><span data-stu-id="a1f71-162">If you want to remove an owner from a group, use the Remove-AzureADGroupOwner cmdlet:</span></span>

    PS C:\Windows\system32> remove-AzureADGroupOwner -ObjectId 31f1ff6c-d48c-4f8a-b2e1-abca7fd399df -OwnerId e831b3fd-77c9-49c7-9fca-de43e109ef67

## <a name="reserved-aliases"></a><span data-ttu-id="a1f71-163">Reserverade alias</span><span class="sxs-lookup"><span data-stu-id="a1f71-163">Reserved aliases</span></span> 
<span data-ttu-id="a1f71-164">När en grupp har skapats, vissa slutpunkter gör det möjligt för användaren att ange en mailNickname eller alias som ska användas som en del av e-postadress i gruppen.</span><span class="sxs-lookup"><span data-stu-id="a1f71-164">When a group is created, certain endpoints allow the end user to specify a mailNickname or alias to be used as part of the email address of the group.</span></span> <span data-ttu-id="a1f71-165">Grupper med följande mycket Privilegierade e-post-alias kan bara skapas av en global administratör för Azure AD.</span><span class="sxs-lookup"><span data-stu-id="a1f71-165">Groups with the following highly privileged email aliases can only be created by an Azure AD global administrator.</span></span> 
  
* <span data-ttu-id="a1f71-166">missbruk</span><span class="sxs-lookup"><span data-stu-id="a1f71-166">abuse</span></span> 
* <span data-ttu-id="a1f71-167">Admin</span><span class="sxs-lookup"><span data-stu-id="a1f71-167">admin</span></span> 
* <span data-ttu-id="a1f71-168">Administratören</span><span class="sxs-lookup"><span data-stu-id="a1f71-168">administrator</span></span> 
* <span data-ttu-id="a1f71-169">hostmaster</span><span class="sxs-lookup"><span data-stu-id="a1f71-169">hostmaster</span></span> 
* <span data-ttu-id="a1f71-170">majordomo</span><span class="sxs-lookup"><span data-stu-id="a1f71-170">majordomo</span></span> 
* <span data-ttu-id="a1f71-171">postmaster</span><span class="sxs-lookup"><span data-stu-id="a1f71-171">postmaster</span></span> 
* <span data-ttu-id="a1f71-172">rot</span><span class="sxs-lookup"><span data-stu-id="a1f71-172">root</span></span> 
* <span data-ttu-id="a1f71-173">Skydda</span><span class="sxs-lookup"><span data-stu-id="a1f71-173">secure</span></span> 
* <span data-ttu-id="a1f71-174">Säkerhet</span><span class="sxs-lookup"><span data-stu-id="a1f71-174">security</span></span> 
* <span data-ttu-id="a1f71-175">SSL-admin</span><span class="sxs-lookup"><span data-stu-id="a1f71-175">ssl-admin</span></span> 
* <span data-ttu-id="a1f71-176">webbadministratör</span><span class="sxs-lookup"><span data-stu-id="a1f71-176">webmaster</span></span> 

## <a name="next-steps"></a><span data-ttu-id="a1f71-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a1f71-177">Next steps</span></span>
<span data-ttu-id="a1f71-178">Du kan hitta mer Azure Active Directory PowerShell-dokumentationen på [Azure Active Directory-cmdletarna](/powershell/azure/install-adv2?view=azureadps-2.0).</span><span class="sxs-lookup"><span data-stu-id="a1f71-178">You can find more Azure Active Directory PowerShell documentation at [Azure Active Directory Cmdlets](/powershell/azure/install-adv2?view=azureadps-2.0).</span></span>

* [<span data-ttu-id="a1f71-179">Hantera åtkomst till resurser med Azure Active Directory-grupper</span><span class="sxs-lookup"><span data-stu-id="a1f71-179">Managing access to resources with Azure Active Directory groups</span></span>](active-directory-manage-groups.md)
* [<span data-ttu-id="a1f71-180">Integrera dina lokala identiteter med Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="a1f71-180">Integrating your on-premises identities with Azure Active Directory</span></span>](active-directory-aadconnect.md)
