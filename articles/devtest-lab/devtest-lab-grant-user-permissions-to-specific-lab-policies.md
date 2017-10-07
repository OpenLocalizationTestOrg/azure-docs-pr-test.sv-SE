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
# <a name="grant-user-permissions-toospecific-lab-policies"></a>Bevilja användarbehörighet toospecific principer för labbet
## <a name="overview"></a>Översikt
Den här artikeln beskrivs hur toouse PowerShell toogrant användare behörighet tooa viss lab princip. På så sätt kan kan behörigheter användas beroende på varje användares behov. Till exempel du kanske vill toogrant en viss hello möjlighet toochange hello VM principinställningar för användaren, men inte hello kostnad principer.

## <a name="policies-as-resources"></a>Principer som resurser
Enligt beskrivningen i hello [Azure rollbaserad åtkomstkontroll](../active-directory/role-based-access-control-configure.md) artikeln RBAC Aktivera detaljerad åtkomsthantering av resurser för Azure. Med RBAC kan du särskilja uppgifter inom DevOps-teamet och ge endast hello mängden åtkomst toousers de behöver tooperform sitt arbete.

I DevTest Labs är en princip för en resurstyp som möjliggör hello RBAC åtgärd **Microsoft.DevTestLab/labs/policySets/policies/**. Varje princip för labbet är en resurs i hello princip resurstyp och kan tilldelas som en omfattning tooan RBAC roll.

Till exempel i ordning toogrant användare läsning och skrivning behörighet toohello **tillåtna storlekar på VM** princip, skulle du skapa en anpassad roll som fungerar med hello **Microsoft.DevTestLab/labs/policySets/policies/** * åtgärd och tilldela hello lämpliga användare toothis anpassad roll i hello omfattning **Microsoft.DevTestLab/labs/policySets/policies/AllowedVmSizesInLab**.

toolearn mer om anpassade roller i RBAC, se hello [anpassade roller åtkomstkontroll](../active-directory/role-based-access-control-custom-roles.md).

## <a name="creating-a-lab-custom-role-using-powershell"></a>Skapa en anpassad labb-roll med hjälp av PowerShell
I ordning tooget igång, behöver du tooread hello följande artikel som förklarar hur tooinstall och konfigurera hello Azure PowerShell-cmdlets: [https://azure.microsoft.com/blog/azps-1-0-pre](https://azure.microsoft.com/blog/azps-1-0-pre).

När du har skapat hello Azure PowerShell-cmdlets kan du utföra följande uppgifter hello:

* Visa en lista med alla hello operations åtgärder för en resursprovider
* Lista över åtgärder i en viss roll:
* Skapa en anpassad roll

hello följande PowerShell-skriptet visar exempel på hur tooperform dessa uppgifter:

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

## <a name="assigning-permissions-tooa-user-for-a-specific-policy-using-custom-roles"></a>Tilldela behörigheter tooa användare för en specifik princip som använder anpassade roller
När du har definierat din anpassade roller, kan du tilldela dem toousers. I ordning tooassign en anpassad roll tooa användare, måste du först skaffa hello **ObjectId** som representerar den aktuella användaren. toodo som använder hello **Get-AzureRmADUser** cmdlet.

I följande exempel hello, hello **ObjectId** av hello *SomeUser* användaren är 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3.

    PS C:\>Get-AzureRmADUser -SearchString "SomeUser"

    DisplayName                    Type                           ObjectId
    -----------                    ----                           --------
    someuser@hotmail.com                                          05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3

När du har hello **ObjectId** för hello användare och en anpassad rollnamn, du kan tilldela användaren rollen toohello med hello **ny AzureRmRoleAssignment** cmdlet:

    PS C:\>New-AzureRmRoleAssignment -ObjectId 05DEFF7B-0AC3-4ABF-B74D-6A72CD5BF3F3 -RoleDefinitionName "Policy Contributor" -Scope /subscriptions/<SubscriptionID>/resourceGroups/<ResourceGroupName>/providers/Microsoft.DevTestLab/labs/<LabName>/policySets/policies/AllowedVmSizesInLab

I föregående exempel hello hello **AllowedVmSizesInLab** principen används. Du kan använda någon av följande principer hello:

* MaxVmsAllowedPerUser
* MaxVmsAllowedPerLab
* AllowedVmSizesInLab
* LabVmsShutdown

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Nästa steg
När du beviljat behörigheter toospecific lab användarprinciper, här är några tooconsider för nästa steg:

* [Säker åtkomst tooa lab](devtest-lab-add-devtest-user.md).
* [Ange principer för labbet](devtest-lab-set-lab-policy.md).
* [Skapa en labbmall](devtest-lab-create-template.md).
* [Skapa anpassade artefakter för dina virtuella datorer](devtest-lab-artifact-author.md).
* [Lägg till en virtuell dator med artefakter tooa lab](devtest-lab-add-vm-with-artifacts.md).

