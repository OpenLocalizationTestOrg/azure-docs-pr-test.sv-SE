---
title: "aaaGrant behörigheter toospecific lab användarprinciper | Microsoft Docs"
description: "Lär dig hur toogrant behörigheter toospecific lab användarprinciper i DevTest Labs baserat på varje användares behov"
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
ms.openlocfilehash: 35647ab837243188f06566cdf365b67fe33a3865
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="grant-user-permissions-toospecific-lab-policies"></a><span data-ttu-id="e3888-103">Bevilja användarbehörighet toospecific principer för labbet</span><span class="sxs-lookup"><span data-stu-id="e3888-103">Grant user permissions toospecific lab policies</span></span>
## <a name="overview"></a><span data-ttu-id="e3888-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="e3888-104">Overview</span></span>
<span data-ttu-id="e3888-105">Den här artikeln beskrivs hur toouse PowerShell toogrant användare behörighet tooa viss lab princip.</span><span class="sxs-lookup"><span data-stu-id="e3888-105">This article illustrates how toouse PowerShell toogrant users permissions tooa particular lab policy.</span></span> <span data-ttu-id="e3888-106">På så sätt kan kan behörigheter användas beroende på varje användares behov.</span><span class="sxs-lookup"><span data-stu-id="e3888-106">That way, permissions can be applied based on each user's needs.</span></span> <span data-ttu-id="e3888-107">Till exempel du kanske vill toogrant en viss hello möjlighet toochange hello VM principinställningar för användaren, men inte hello kostnad principer.</span><span class="sxs-lookup"><span data-stu-id="e3888-107">For example, you might want toogrant a particular user hello ability toochange hello VM policy settings, but not hello cost policies.</span></span>

## <a name="policies-as-resources"></a><span data-ttu-id="e3888-108">Principer som resurser</span><span class="sxs-lookup"><span data-stu-id="e3888-108">Policies as resources</span></span>
<span data-ttu-id="e3888-109">Enligt beskrivningen i hello [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md) artikeln RBAC Aktivera detaljerad åtkomsthantering av resurser för Azure.</span><span class="sxs-lookup"><span data-stu-id="e3888-109">As discussed in hello [Azure Role-based Access Control](../active-directory/role-based-access-control-configure.md) article, RBAC enables fine-grained access management of resources for Azure.</span></span> <span data-ttu-id="e3888-110">Med RBAC kan du särskilja uppgifter inom DevOps-teamet och ge endast hello mängden åtkomst toousers de behöver tooperform sitt arbete.</span><span class="sxs-lookup"><span data-stu-id="e3888-110">Using RBAC, you can segregate duties within your DevOps team and grant only hello amount of access toousers that they need tooperform their jobs.</span></span>

<span data-ttu-id="e3888-111">I DevTest Labs är en princip för en resurstyp som möjliggör hello RBAC åtgärd **Microsoft.DevTestLab/labs/policySets/policies/**.</span><span class="sxs-lookup"><span data-stu-id="e3888-111">In DevTest Labs, a policy is a resource type that enables hello RBAC action **Microsoft.DevTestLab/labs/policySets/policies/**.</span></span> <span data-ttu-id="e3888-112">Varje princip för labbet är en resurs i hello princip resurstyp och kan tilldelas som en omfattning tooan RBAC roll.</span><span class="sxs-lookup"><span data-stu-id="e3888-112">Each lab policy is a resource in hello Policy resource type, and can be assigned as a scope tooan RBAC role.</span></span>

<span data-ttu-id="e3888-113">Till exempel i ordning toogrant användare läsning och skrivning behörighet toohello **tillåtna storlekar på VM** princip, skulle du skapa en anpassad roll som fungerar med hello **Microsoft.DevTestLab/labs/policySets/policies/** * åtgärd och tilldela hello lämpliga användare toothis anpassad roll i hello omfattning **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span><span class="sxs-lookup"><span data-stu-id="e3888-113">For example, in order toogrant users read/write permission toohello **Allowed VM Sizes** policy, you would create a custom role that works with hello **Microsoft.DevTestLab/labs/policySets/policies/*** action, and then assign hello appropriate users toothis custom role in hello scope of **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.</span></span>

<span data-ttu-id="e3888-114">toolearn mer om anpassade roller i RBAC, se hello [anpassade roller åtkomstkontroll](../active-directory/role-based-access-control-custom-roles.md).</span><span class="sxs-lookup"><span data-stu-id="e3888-114">toolearn more about custom roles in RBAC, see hello [Custom roles access control](../active-directory/role-based-access-control-custom-roles.md).</span></span>

## <a name="creating-a-lab-custom-role-using-powershell"></a><span data-ttu-id="e3888-115">Skapa en anpassad labb-roll med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="e3888-115">Creating a lab custom role using PowerShell</span></span>
<span data-ttu-id="e3888-116">I ordning tooget igång, behöver du tooread hello följande artikel som förklarar hur tooinstall och konfigurera hello Azure PowerShell-cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span><span class="sxs-lookup"><span data-stu-id="e3888-116">In order tooget started, you’ll need tooread hello following article, which will explain how tooinstall and configure hello Azure PowerShell cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).</span></span>

<span data-ttu-id="e3888-117">När du har skapat hello Azure PowerShell-cmdlets kan du utföra följande uppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="e3888-117">Once you’ve set up hello Azure PowerShell cmdlets, you can perform hello following tasks:</span></span>

* <span data-ttu-id="e3888-118">Visa en lista med alla hello operations åtgärder för en resursprovider</span><span class="sxs-lookup"><span data-stu-id="e3888-118">List all hello operations/actions for a resource provider</span></span>
* <span data-ttu-id="e3888-119">Lista över åtgärder i en viss roll:</span><span class="sxs-lookup"><span data-stu-id="e3888-119">List actions in a particular role:</span></span>
* <span data-ttu-id="e3888-120">Skapa en anpassad roll</span><span class="sxs-lookup"><span data-stu-id="e3888-120">Create a custom role</span></span>

<span data-ttu-id="e3888-121">hello följande PowerShell-skriptet visar exempel på hur tooperform dessa uppgifter:</span><span class="sxs-lookup"><span data-stu-id="e3888-121">hello following PowerShell script illustrates examples of how tooperform these tasks:</span></span>

    ‘List all hello operations/actions for a resource provider.
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

## <a name="assigning-permissions-tooa-user-for-a-specific-policy-using-custom-roles"></a><span data-ttu-id="e3888-122">Tilldela behörigheter tooa användare för en specifik princip som använder anpassade roller</span><span class="sxs-lookup"><span data-stu-id="e3888-122">Assigning permissions tooa user for a specific policy using custom roles</span></span>
<span data-ttu-id="e3888-123">När du har definierat din anpassade roller, kan du tilldela dem toousers.</span><span class="sxs-lookup"><span data-stu-id="e3888-123">Once you’ve defined your custom roles, you can assign them toousers.</span></span> <span data-ttu-id="e3888-124">I ordning tooassign en anpassad roll tooa användare, måste du först skaffa hello **ObjectId** som representerar den aktuella användaren.</span><span class="sxs-lookup"><span data-stu-id="e3888-124">In order tooassign a custom role tooa user, you must first obtain hello **ObjectId** representing that user.</span></span> <span data-ttu-id="e3888-125">toodo som använder hello **Get-AzureRmADUser** cmdlet.</span><span class="sxs-lookup"><span data-stu-id="e3888-125">toodo that, use hello **Get-AzureRmADUser** cmdlet.</span></span>

<span data-ttu-id="e3888-126">I följande exempel hello, hello **ObjectId** av hello *SomeUser* användaren är 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span><span class="sxs-lookup"><span data-stu-id="e3888-126">In hello following example, hello **ObjectId** of hello *SomeUser* user is 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.</span></span>

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

<span data-ttu-id="e3888-127">När du har hello **ObjectId** för hello användare och en anpassad rollnamn, du kan tilldela användaren rollen toohello med hello **ny AzureRmRoleAssignment** cmdlet:</span><span class="sxs-lookup"><span data-stu-id="e3888-127">Once you have hello **ObjectId** for hello user and a custom role name, you can assign that role toohello user with hello **New-AzureRmRoleAssignment** cmdlet:</span></span>

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

<span data-ttu-id="e3888-128">I föregående exempel hello hello **AllowedVmSizesInLab** principen används.</span><span class="sxs-lookup"><span data-stu-id="e3888-128">In hello previous example, hello **AllowedVmSizesInLab** policy is used.</span></span> <span data-ttu-id="e3888-129">Du kan använda någon av följande principer hello:</span><span class="sxs-lookup"><span data-stu-id="e3888-129">You can use any of hello following polices:</span></span>

* <span data-ttu-id="e3888-130">MaxVmsAllowedPerUser</span><span class="sxs-lookup"><span data-stu-id="e3888-130">MaxVmsAllowedPerUser</span></span>
* <span data-ttu-id="e3888-131">MaxVmsAllowedPerLab</span><span class="sxs-lookup"><span data-stu-id="e3888-131">MaxVmsAllowedPerLab</span></span>
* <span data-ttu-id="e3888-132">AllowedVmSizesInLab</span><span class="sxs-lookup"><span data-stu-id="e3888-132">AllowedVmSizesInLab</span></span>
* <span data-ttu-id="e3888-133">LabVmsShutdown</span><span class="sxs-lookup"><span data-stu-id="e3888-133">LabVmsShutdown</span></span>

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a><span data-ttu-id="e3888-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3888-134">Next steps</span></span>
<span data-ttu-id="e3888-135">När du beviljat behörigheter toospecific lab användarprinciper, här är några tooconsider för nästa steg:</span><span class="sxs-lookup"><span data-stu-id="e3888-135">Once you've granted user permissions toospecific lab policies, here are some next steps tooconsider:</span></span>

* <span data-ttu-id="e3888-136">[Säker åtkomst tooa lab](devtest-lab-add-devtest-user.md).</span><span class="sxs-lookup"><span data-stu-id="e3888-136">[Secure access tooa lab](devtest-lab-add-devtest-user.md).</span></span>
* <span data-ttu-id="e3888-137">[Ange principer för labbet](devtest-lab-set-lab-policy.md).</span><span class="sxs-lookup"><span data-stu-id="e3888-137">[Set lab policies](devtest-lab-set-lab-policy.md).</span></span>
* <span data-ttu-id="e3888-138">[Skapa en labbmall](devtest-lab-create-template.md).</span><span class="sxs-lookup"><span data-stu-id="e3888-138">[Create a lab template](devtest-lab-create-template.md).</span></span>
* <span data-ttu-id="e3888-139">[Skapa anpassade artefakter för dina virtuella datorer](devtest-lab-artifact-author.md).</span><span class="sxs-lookup"><span data-stu-id="e3888-139">[Create custom artifacts for your VMs](devtest-lab-artifact-author.md).</span></span>
* <span data-ttu-id="e3888-140">[Lägg till en virtuell dator med artefakter tooa lab](devtest-lab-add-vm-with-artifacts.md).</span><span class="sxs-lookup"><span data-stu-id="e3888-140">[Add a VM with artifacts tooa lab](devtest-lab-add-vm-with-artifacts.md).</span></span>

