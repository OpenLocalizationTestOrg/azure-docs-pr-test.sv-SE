---
title: aaaMigrate Automation-konto och resurser | Microsoft Docs
description: "Den här artikeln beskriver hur toomove ett Automation-konto i Azure Automation och associerade resurser från en prenumeration tooanother."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
ms.assetid: 9c2db4a2-f324-48dc-8ce7-3343bf7230d5
ms.service: automation
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: magoedte
ms.openlocfilehash: 201c9091cd2d78d7ea407c1e5fb27f366bb4fa8c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="migrate-automation-account-and-resources"></a><span data-ttu-id="7f0fd-103">Migrera Automation-konto och resurser</span><span class="sxs-lookup"><span data-stu-id="7f0fd-103">Migrate Automation Account and resources</span></span>
<span data-ttu-id="7f0fd-104">För Automation-konton och dess associerade resurser (d.v.s. tillgångar, runbooks, moduler, etc.) som du har skapat i hello Azure-portalen och toomigrate från en resurs grupp tooanother eller från en prenumeration tooanother kan du göra det enkelt med Hej [flyttar resurser](../azure-resource-manager/resource-group-move-resources.md) funktion i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-104">For Automation accounts and its associated resources (i.e. assets, runbooks, modules, etc.) that you have created in hello Azure portal and want toomigrate from one resource group tooanother or from one subscription tooanother, you can accomplish this easily with hello [move resources](../azure-resource-manager/resource-group-move-resources.md) feature available in hello Azure portal.</span></span> <span data-ttu-id="7f0fd-105">Men innan du fortsätter med den här åtgärden, bör du först granska hello följande [checklistan innan du flyttar resurser](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) och dessutom hello listan nedan specifika tooAutomation.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-105">However, before proceeding with this action, you should first review hello following [checklist before moving resources](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) and additionally, hello list below specific tooAutomation.</span></span>   

1. <span data-ttu-id="7f0fd-106">hello mål prenumeration/resursgrupp måste vara i samma region som hello källa.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-106">hello destination subscription/resource group must be in same region as hello source.</span></span>  <span data-ttu-id="7f0fd-107">D.v.s. kan Automation-konton inte flyttas över regioner.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-107">Meaning, Automation accounts cannot be moved across regions.</span></span>
2. <span data-ttu-id="7f0fd-108">När du flyttar resurser (t.ex. runbooks, jobb, etc.), är såväl hello källgrupp hello målgruppen låsta för hello drifttiden hello.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-108">When moving resources (e.g. runbooks, jobs, etc.), both hello source group and hello target group are locked for hello duration of hello operation.</span></span> <span data-ttu-id="7f0fd-109">Skriva och ta bort blockeras på hello grupper tills hello flytta har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-109">Write and delete operations are blocked on hello groups until hello move completes.</span></span>  
3. <span data-ttu-id="7f0fd-110">Alla runbooks eller variabler som refererar till en resurs eller prenumerations-ID från hello befintliga prenumeration behöver toobe uppdateras när migreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-110">Any runbooks or variables which reference a resource or subscription ID from hello existing subscription will need toobe updated after migration is completed.</span></span>   

> [!NOTE]
> <span data-ttu-id="7f0fd-111">Den här funktionen stöder inte flytta klassisk automation resurser.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-111">This feature does not support moving Classic automation resources.</span></span>
>
>

## <a name="toomove-hello-automation-account-using-hello-portal"></a><span data-ttu-id="7f0fd-112">toomove hello Automation-konto med hjälp av hello portal</span><span class="sxs-lookup"><span data-stu-id="7f0fd-112">toomove hello Automation Account using hello portal</span></span>
1. <span data-ttu-id="7f0fd-113">Från ditt Automation-konto klickar du på **flytta** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-113">From your Automation account, click **Move** at hello top of hello blade.</span></span><br> <span data-ttu-id="7f0fd-114">![Flytta alternativet](media/automation-migrate-account-subscription/automation-menu-move.png)</span><span class="sxs-lookup"><span data-stu-id="7f0fd-114">![Move option](media/automation-migrate-account-subscription/automation-menu-move.png)</span></span><br>
2. <span data-ttu-id="7f0fd-115">På hello **flyttar resurser** bladet, Observera att det innehåller resurser relaterade tooboth ditt Automation-konto och resurs-grupper.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-115">On hello **Move resources** blade, note that it presents resources related tooboth your Automation account and your resource group(s).</span></span>  <span data-ttu-id="7f0fd-116">Välj hello **prenumeration** och **resursgruppen** från hello listrutorna eller välj hello alternativet **skapa en ny resursgrupp** och ange ett namn på ny resursgrupp i hello fält har angetts.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-116">Select hello **subscription** and **resource group** from hello drop-down lists, or select hello option **create a new resource group** and enter a new resource group name in hello field provided.</span></span>  
3. <span data-ttu-id="7f0fd-117">Granska och välj hello kryssrutan tooacknowledge du *förstå verktyg och skript kommer måste toobe uppdateras toouse ny resurs-ID när resurser har flyttats* och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-117">Review and select hello checkbox tooacknowledge you *understand tools and scripts will need toobe updated toouse new resource IDs after resources have been moved* and then click **OK**.</span></span><br> <span data-ttu-id="7f0fd-118">![Flytta resurser bladet](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span><span class="sxs-lookup"><span data-stu-id="7f0fd-118">![Move Resources Blade](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span></span><br>   

<span data-ttu-id="7f0fd-119">Den här åtgärden tar flera minuter toocomplete.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-119">This action will take several minutes toocomplete.</span></span>  <span data-ttu-id="7f0fd-120">I **meddelanden**, visas med status för alla åtgärder som sker - verifieringen, migrering, och sedan slutligen när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-120">In **Notifications**, you will be presented with a status of each action that takes place - validation, migration, and then finally when it is completed.</span></span>     

## <a name="toomove-hello-automation-account-using-powershell"></a><span data-ttu-id="7f0fd-121">toomove hello Automation-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="7f0fd-121">toomove hello Automation Account using PowerShell</span></span>
<span data-ttu-id="7f0fd-122">toomove befintlig Automation resurser tooanother resursgrupp eller prenumeration, använda hello **Get-AzureRmResource** cmdlet-tooget hello specifika Automation-konto och sedan **flytta AzureRmResource** cmdlet tooperform hello flytta.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-122">toomove existing Automation resources tooanother resource group or subscription, use hello  **Get-AzureRmResource** cmdlet tooget hello specific Automation account and then **Move-AzureRmResource** cmdlet tooperform hello move.</span></span>

<span data-ttu-id="7f0fd-123">hello första exemplet visar hur toomove ett Automation-kontot tooa ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-123">hello first example shows how toomove an Automation account tooa new resource group.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

<span data-ttu-id="7f0fd-124">När du kör hello ovan kodexempel, kommer du att ange tooverify som du vill tooperform den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-124">After you execute hello above code example, you will be prompted tooverify you want tooperform this action.</span></span>  <span data-ttu-id="7f0fd-125">När du klickar på **Ja** och tillåta Hej skriptet tooproceed, du får inga meddelanden när den utför hello migrering.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-125">Once you click **Yes** and allow hello script tooproceed, you will not receive any notifications while it's performing hello migration.</span></span>  

<span data-ttu-id="7f0fd-126">toomove tooa ny prenumeration, innehåller ett värde för hello *DestinationSubscriptionId* parameter.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-126">toomove tooa new subscription, include a value for hello *DestinationSubscriptionId* parameter.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

<span data-ttu-id="7f0fd-127">Du kommer att tillfrågas tooconfirm hello flytta som i föregående exempel hello.</span><span class="sxs-lookup"><span data-stu-id="7f0fd-127">As with hello previous example, you will be prompted tooconfirm hello move.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="7f0fd-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f0fd-128">Next steps</span></span>
* <span data-ttu-id="7f0fd-129">Mer information om att flytta resurser toonew resursgrupp eller prenumeration finns [flytta resurser toonew resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="7f0fd-129">For more information about moving resources toonew resource group or subscription, see [Move  resources toonew resource group or subscription](../azure-resource-manager/resource-group-move-resources.md)</span></span>
* <span data-ttu-id="7f0fd-130">Mer information om rollbaserad åtkomstkontroll i Azure Automation finns för[rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="7f0fd-130">For more information about Role-based Access Control in Azure Automation, refer too[Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="7f0fd-131">toolearn om PowerShell-cmdletar för att hantera din prenumeration finns [med hjälp av Azure PowerShell med Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="7f0fd-131">toolearn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="7f0fd-132">toolearn om portalen funktioner för att hantera din prenumeration, se [använder hello Azure Portal toomanage resurser](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="7f0fd-132">toolearn about portal features for managing your subscription, see [Using hello Azure Portal toomanage resources](../azure-resource-manager/resource-group-portal.md).</span></span>
