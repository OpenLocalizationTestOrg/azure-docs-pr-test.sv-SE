---
title: "Bevilja användarbehörighet att principer för specifika labbet | Microsoft Docs"
description: "Lär dig att ge användarbehörigheter principer för specifika labb i DevTest Labs baserat på varje användares behov"
services: devtest-lab,virtual-machines,visual-studio-online
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 5ca829f0-eb69-40a1-ae26-03a629db1d7e
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/25/2016
ms.author: tarcher
ms.openlocfilehash: 0bd9f83257834d9681479ba9117c48ffd6d6e166
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="grant-user-permissions-to-specific-lab-policies"></a><span data-ttu-id="48b0d-103">Bevilja användarbehörighet att principer för specifika labbet</span><span class="sxs-lookup"><span data-stu-id="48b0d-103">Grant user permissions to specific lab policies</span></span>
## <a name="overview"></a><span data-ttu-id="48b0d-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="48b0d-104">Overview</span></span>
<span data-ttu-id="48b0d-105">Den här artikeln visar hur du använder PowerShell för att ge användare behörighet till en viss labb-princip.</span><span class="sxs-lookup"><span data-stu-id="48b0d-105">This article illustrates how to use PowerShell to grant users permissions to a particular lab policy.</span></span> <span data-ttu-id="48b0d-106">På så sätt kan kan behörigheter användas beroende på varje användares behov.</span><span class="sxs-lookup"><span data-stu-id="48b0d-106">That way, permissions can be applied based on each user's needs.</span></span> <span data-ttu-id="48b0d-107">Du kanske vill ge en viss användare möjlighet att ändra principinställningarna VM, men inte kostnaden principer.</span><span class="sxs-lookup"><span data-stu-id="48b0d-107">For example, you might want to grant a particular user the ability to change the VM policy settings, but not the cost policies.</span></span>

## <a name="policies-as-resources"></a><span data-ttu-id="48b0d-108">Principer som resurser</span><span class="sxs-lookup"><span data-stu-id="48b0d-108">Policies as resources</span></span>
<span data-ttu-id="48b0d-109">Enligt beskrivningen i den [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md) artikeln RBAC Aktivera detaljerad åtkomsthantering av resurser för Azure.</span><span class="sxs-lookup"><span data-stu-id="48b0d-109">As discussed in the [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md) article, RBAC enables fine-grained access management of resources for Azure.</span></span> <span data-ttu-id="48b0d-110">Med RBAC kan du särskilja uppgifter inom DevOps-teamet och ge bara mängden åtkomst till användare som de behöver för att utföra sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="48b0d-110">Using RBAC, you can segregate duties within your DevOps team and grant only the amount of access to users that they need to perform their jobs.</span></span>

<span data-ttu-id="48b0d-111">I DevTest Labs är en princip för en resurstyp som aktiverar åtgärden RBAC **Microsoft.DevTestLab/labs/policySets/policies/**.</span><span class="sxs-lookup"><span data-stu-id="48b0d-111">In DevTest Labs, a policy is a resource type that enables the RBAC action **Microsoft.DevTestLab/labs/policySets/policies/**.</span></span> <span data-ttu-id="48b0d-112">Varje princip för labbet är en resurs i princip resurstypen och kan tilldelas som ett scope till ett RBAC-roll.</span><span class="sxs-lookup"><span data-stu-id="48b0d-112">Each lab policy is a resource in the Policy resource type, and can be assigned as a scope to an RBAC role.</span></span>

<span data-ttu-id="48b0d-113">Till exempel för att ge användare läs/skrivbehörighet till den **tillåtna storlekar på VM** princip, skulle du skapa en anpassad roll som fungerar med den **Microsoft.DevTestLab/labs/policySets/policies/*** åtgärd, och sedan tilldela rätt användare till den här anpassade rollen i omfånget för **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span><span class="sxs-lookup"><span data-stu-id="48b0d-113">For example, in order to grant users read/write permission to the **Allowed VM Sizes** policy, you would create a custom role that works with the **Microsoft.DevTestLab/labs/policySets/policies/*** action, and then assign the appropriate users to this custom role in the scope of **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span></span>

<span data-ttu-id="48b0d-114">Mer information om anpassade roller i RBAC finns det [anpassade roller åtkomstkontroll](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="48b0d-114">To learn more about custom roles in RBAC, see the [Custom roles access control](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="creating-a-lab-custom-role-using-powershell"></a><span data-ttu-id="48b0d-115">Skapa en anpassad labb-roll med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="48b0d-115">Creating a lab custom role using PowerShell</span></span>
<span data-ttu-id="48b0d-116">Du behöver läsa följande artikel som förklarar hur du installerar och konfigurerar du Azure PowerShell-cmdlets för att komma igång: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span><span class="sxs-lookup"><span data-stu-id="48b0d-116">In order to get started, you’ll need to read the following article, which will explain how to install and configure the Azure PowerShell cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span></span>

<span data-ttu-id="48b0d-117">När du har konfigurerat Azure PowerShell-cmdlets kan du utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="48b0d-117">Once you’ve set up the Azure PowerShell cmdlets, you can perform the following tasks:</span></span>

* <span data-ttu-id="48b0d-118">Visa en lista med alla operations/åtgärder för en resursprovider</span><span class="sxs-lookup"><span data-stu-id="48b0d-118">List all the operations/actions for a resource provider</span></span>
* <span data-ttu-id="48b0d-119">Lista över åtgärder i en viss roll:</span><span class="sxs-lookup"><span data-stu-id="48b0d-119">List actions in a particular role:</span></span>
* <span data-ttu-id="48b0d-120">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="48b0d-120">Create a custom role</span></span>

<span data-ttu-id="48b0d-121">Följande PowerShell-skript visar exempel på hur du utför dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="48b0d-121">The following PowerShell script illustrates examples of how to perform these tasks:</span></span>

    ‘List all the operations/actions for a resource provider.
    Get-AzureRmProviderOperation -OperationSearchString "Microsoft.DevTestLab/*"

    ‘List actions in a particular role.
    (Get-AzureRmRoleDefinition "DevTest Labs User").Actions

    ‘Create custom role.
    $policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User")
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "Policy Contributor"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("/subscriptions/<SubscriptionID> ")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/policySets/policies/*")
    $policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)

## <a name="assigning-permissions-to-a-user-for-a-specific-policy-using-custom-roles"></a><span data-ttu-id="48b0d-122">Tilldela behörigheter till en användare för en specifik princip som använder anpassade roller</span><span class="sxs-lookup"><span data-stu-id="48b0d-122">Assigning permissions to a user for a specific policy using custom roles</span></span>
<span data-ttu-id="48b0d-123">När du har definierat din anpassade roller, kan du tilldela dem till användare.</span><span class="sxs-lookup"><span data-stu-id="48b0d-123">Once you’ve defined your custom roles, you can assign them to users.</span></span> <span data-ttu-id="48b0d-124">För att tilldela en anpassad roll till en användare, måste du först skaffa den **ObjectId** som representerar den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="48b0d-124">In order to assign a custom role to a user, you must first obtain the **ObjectId** representing that user.</span></span> <span data-ttu-id="48b0d-125">Göra genom att använda den **Get-AzureRmADUser** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="48b0d-125">To do that, use the **Get-AzureRmADUser** cmdlet.</span></span>

<span data-ttu-id="48b0d-126">I följande exempel visas den **ObjectId** av den *SomeUser* användaren är 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span><span class="sxs-lookup"><span data-stu-id="48b0d-126">In the following example, the **ObjectId** of the *SomeUser* user is 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span></span>

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

<span data-ttu-id="48b0d-127">När du har den **ObjectId** för användaren och ett namn på anpassad roll kan du tilldela rollen användare med den **ny AzureRmRoleAssignment** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="48b0d-127">Once you have the **ObjectId** for the user and a custom role name, you can assign that role to the user with the **New-AzureRmRoleAssignment** cmdlet:</span></span>

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

<span data-ttu-id="48b0d-128">I föregående exempel är den **AllowedVmSizesInLab** principen används.</span><span class="sxs-lookup"><span data-stu-id="48b0d-128">In the previous example, the **AllowedVmSizesInLab** policy is used.</span></span> <span data-ttu-id="48b0d-129">Du kan använda någon av följande principer:</span><span class="sxs-lookup"><span data-stu-id="48b0d-129">You can use any of the following polices:</span></span>

* <span data-ttu-id="48b0d-130">MaxVmsAllowedPerUser</span><span class="sxs-lookup"><span data-stu-id="48b0d-130">MaxVmsAllowedPerUser</span></span>
* <span data-ttu-id="48b0d-131">MaxVmsAllowedPerLab</span><span class="sxs-lookup"><span data-stu-id="48b0d-131">MaxVmsAllowedPerLab</span></span>
* <span data-ttu-id="48b0d-132">AllowedVmSizesInLab</span><span class="sxs-lookup"><span data-stu-id="48b0d-132">AllowedVmSizesInLab</span></span>
* <span data-ttu-id="48b0d-133">LabVmsShutdown</span><span class="sxs-lookup"><span data-stu-id="48b0d-133">LabVmsShutdown</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="48b0d-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="48b0d-134">Next steps</span></span>
<span data-ttu-id="48b0d-135">När du har behörighet användaren att principer för specifika labbet här är att tänka på följande steg:</span><span class="sxs-lookup"><span data-stu-id="48b0d-135">Once you've granted user permissions to specific lab policies, here are some next steps to consider:</span></span>

* <span data-ttu-id="48b0d-136">[Säker åtkomst till ett labb](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="48b0d-136">[Secure access to a lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="48b0d-137">[Ange principer för labbet](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="48b0d-137">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="48b0d-138">[Skapa en labbmall](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="48b0d-138">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="48b0d-139">[Skapa anpassade artefakter för dina virtuella datorer](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="48b0d-139">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="48b0d-140">[Lägga till en virtuell dator med artefakter i ett labb](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="48b0d-140">[Add a VM with artifacts to a lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

