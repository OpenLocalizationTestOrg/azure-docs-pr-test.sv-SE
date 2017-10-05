---
title: Migrera Automation-konto och resurser | Microsoft Docs
description: "Den här artikeln beskriver hur du flyttar ett Automation-konto i Azure Automation och associerade resurser från en prenumeration till en annan."
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
ms.openlocfilehash: 687da15bdaf854254321b59350f47549781676f5
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="migrate-automation-account-and-resources"></a><span data-ttu-id="d4963-103">Migrera Automation-konto och resurser</span><span class="sxs-lookup"><span data-stu-id="d4963-103">Migrate Automation Account and resources</span></span>
<span data-ttu-id="d4963-104">För Automation-konton och dess associerade resurser (d.v.s. tillgångar, runbooks, moduler, etc.) som du har skapat i Azure-portalen och vill migrera från en resursgrupp till en annan eller från en prenumeration till en annan, du kan göra detta med den [flyttar resurser](../azure-resource-manager/resource-group-move-resources.md) funktion i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d4963-104">For Automation accounts and its associated resources (i.e. assets, runbooks, modules, etc.) that you have created in the Azure portal and want to migrate from one resource group to another or from one subscription to another, you can accomplish this easily with the [move resources](../azure-resource-manager/resource-group-move-resources.md) feature available in the Azure portal.</span></span> <span data-ttu-id="d4963-105">Men innan du fortsätter med den här åtgärden, bör du först granska följande [checklistan innan du flyttar resurser](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) och dessutom i listan nedan som är specifika för automatisering.</span><span class="sxs-lookup"><span data-stu-id="d4963-105">However, before proceeding with this action, you should first review the following [checklist before moving resources](../azure-resource-manager/resource-group-move-resources.md#checklist-before-moving-resources) and additionally, the list below specific to Automation.</span></span>   

1. <span data-ttu-id="d4963-106">Mål-prenumeration/resursgrupp måste vara i samma region som källa.</span><span class="sxs-lookup"><span data-stu-id="d4963-106">The destination subscription/resource group must be in same region as the source.</span></span>  <span data-ttu-id="d4963-107">D.v.s. kan Automation-konton inte flyttas över regioner.</span><span class="sxs-lookup"><span data-stu-id="d4963-107">Meaning, Automation accounts cannot be moved across regions.</span></span>
2. <span data-ttu-id="d4963-108">När du flyttar resurser (t.ex. runbooks, jobb, etc.), låst både gruppen och målgruppen under åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d4963-108">When moving resources (e.g. runbooks, jobs, etc.), both the source group and the target group are locked for the duration of the operation.</span></span> <span data-ttu-id="d4963-109">Skriva och ta bort blockeras på grupper tills flyttningen är klar.</span><span class="sxs-lookup"><span data-stu-id="d4963-109">Write and delete operations are blocked on the groups until the move completes.</span></span>  
3. <span data-ttu-id="d4963-110">Alla runbooks eller variabler som refererar till en resurs eller prenumerations-ID från den befintliga prenumerationen kommer att behöva uppdateras när migreringen är klar.</span><span class="sxs-lookup"><span data-stu-id="d4963-110">Any runbooks or variables which reference a resource or subscription ID from the existing subscription will need to be updated after migration is completed.</span></span>   

> [!NOTE]
> <span data-ttu-id="d4963-111">Den här funktionen stöder inte flytta klassisk automation resurser.</span><span class="sxs-lookup"><span data-stu-id="d4963-111">This feature does not support moving Classic automation resources.</span></span>
>
>

## <a name="to-move-the-automation-account-using-the-portal"></a><span data-ttu-id="d4963-112">Att flytta Automation-konto med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="d4963-112">To move the Automation Account using the portal</span></span>
1. <span data-ttu-id="d4963-113">Från ditt Automation-konto klickar du på **flytta** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="d4963-113">From your Automation account, click **Move** at the top of the blade.</span></span><br> <span data-ttu-id="d4963-114">![Flytta alternativet](media/automation-migrate-account-subscription/automation-menu-move.png)</span><span class="sxs-lookup"><span data-stu-id="d4963-114">![Move option](media/automation-migrate-account-subscription/automation-menu-move.png)</span></span><br>
2. <span data-ttu-id="d4963-115">På den **flyttar resurser** bladet, Observera att det innehåller resurser som rör både Automation-konto och resurs-grupper.</span><span class="sxs-lookup"><span data-stu-id="d4963-115">On the **Move resources** blade, note that it presents resources related to both your Automation account and your resource group(s).</span></span>  <span data-ttu-id="d4963-116">Välj den **prenumeration** och **resursgruppen** från listrutorna eller Välj alternativet **skapa en ny resursgrupp** och ange namn på en ny resursgrupp i fältet har angetts.</span><span class="sxs-lookup"><span data-stu-id="d4963-116">Select the **subscription** and **resource group** from the drop-down lists, or select the option **create a new resource group** and enter a new resource group name in the field provided.</span></span>  
3. <span data-ttu-id="d4963-117">Granska och markera kryssrutan för att bekräfta att du *förstå verktyg och skript kommer att behöva uppdateras för att använda ny resurs-ID när resurser har flyttats* och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d4963-117">Review and select the checkbox to acknowledge you *understand tools and scripts will need to be updated to use new resource IDs after resources have been moved* and then click **OK**.</span></span><br> <span data-ttu-id="d4963-118">![Flytta resurser bladet](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span><span class="sxs-lookup"><span data-stu-id="d4963-118">![Move Resources Blade](media/automation-migrate-account-subscription/automation-move-resources-blade.png)</span></span><br>   

<span data-ttu-id="d4963-119">Den här åtgärden kan ta flera minuter att slutföra.</span><span class="sxs-lookup"><span data-stu-id="d4963-119">This action will take several minutes to complete.</span></span>  <span data-ttu-id="d4963-120">I **meddelanden**, visas med status för alla åtgärder som sker - verifieringen, migrering, och sedan slutligen när den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="d4963-120">In **Notifications**, you will be presented with a status of each action that takes place - validation, migration, and then finally when it is completed.</span></span>     

## <a name="to-move-the-automation-account-using-powershell"></a><span data-ttu-id="d4963-121">Att flytta Automation-konto med hjälp av PowerShell</span><span class="sxs-lookup"><span data-stu-id="d4963-121">To move the Automation Account using PowerShell</span></span>
<span data-ttu-id="d4963-122">Flytta befintliga Automation resurser till en annan resursgrupp eller prenumeration genom att använda den **Get-AzureRmResource** för att hämta specifika Automation-kontot och sedan **flytta AzureRmResource** att Utför övergången.</span><span class="sxs-lookup"><span data-stu-id="d4963-122">To move existing Automation resources to another resource group or subscription, use the  **Get-AzureRmResource** cmdlet to get the specific Automation account and then **Move-AzureRmResource** cmdlet to perform the move.</span></span>

<span data-ttu-id="d4963-123">Det första exemplet visar hur du flyttar ett Automation-konto till en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="d4963-123">The first example shows how to move an Automation account to a new resource group.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup"
   ```

<span data-ttu-id="d4963-124">När du kör kodexemplet ovan, uppmanas du att verifiera att du vill utföra den här åtgärden.</span><span class="sxs-lookup"><span data-stu-id="d4963-124">After you execute the above code example, you will be prompted to verify you want to perform this action.</span></span>  <span data-ttu-id="d4963-125">När du klickar på **Ja** och skript kan fortsätta, du får inga meddelanden när den utför migreringen.</span><span class="sxs-lookup"><span data-stu-id="d4963-125">Once you click **Yes** and allow the script to proceed, you will not receive any notifications while it's performing the migration.</span></span>  

<span data-ttu-id="d4963-126">Om du vill flytta till en ny prenumeration, innehåller ett värde för den *DestinationSubscriptionId* parameter.</span><span class="sxs-lookup"><span data-stu-id="d4963-126">To move to a new subscription, include a value for the *DestinationSubscriptionId* parameter.</span></span>

   ```
    $resource = Get-AzureRmResource -ResourceName "TestAutomationAccount" -ResourceGroupName "ResourceGroup01"
    Move-AzureRmResource -ResourceId $resource.ResourceId -DestinationResourceGroupName "NewResourceGroup" -DestinationSubscriptionId "SubscriptionId"
   ```

<span data-ttu-id="d4963-127">Som i föregående exempel, uppmanas du att bekräfta flyttningen.</span><span class="sxs-lookup"><span data-stu-id="d4963-127">As with the previous example, you will be prompted to confirm the move.</span></span>  

## <a name="next-steps"></a><span data-ttu-id="d4963-128">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d4963-128">Next steps</span></span>
* <span data-ttu-id="d4963-129">Mer information om resurserna flyttas till en ny resursgrupp eller prenumeration finns [flytta resurser till en ny resursgrupp eller prenumeration](../azure-resource-manager/resource-group-move-resources.md)</span><span class="sxs-lookup"><span data-stu-id="d4963-129">For more information about moving resources to new resource group or subscription, see [Move  resources to new resource group or subscription](../azure-resource-manager/resource-group-move-resources.md)</span></span>
* <span data-ttu-id="d4963-130">Mer information om rollbaserad åtkomstkontroll i Azure Automation finns i [Rollbaserad åtkomstkontroll i Azure Automation](automation-role-based-access-control.md).</span><span class="sxs-lookup"><span data-stu-id="d4963-130">For more information about Role-based Access Control in Azure Automation, refer to [Role-based access control in Azure Automation](automation-role-based-access-control.md).</span></span>
* <span data-ttu-id="d4963-131">Mer information om PowerShell-cmdletar för att hantera din prenumeration, se [med hjälp av Azure PowerShell med Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span><span class="sxs-lookup"><span data-stu-id="d4963-131">To learn about PowerShell cmdlets for managing your subscription, see [Using Azure PowerShell with Resource Manager](../azure-resource-manager/powershell-azure-resource-manager.md)</span></span>
* <span data-ttu-id="d4963-132">Mer information om Företagsportalen funktioner för att hantera din prenumeration, se [hantera resurser med hjälp av Azure Portal](../azure-resource-manager/resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d4963-132">To learn about portal features for managing your subscription, see [Using the Azure Portal to manage resources](../azure-resource-manager/resource-group-portal.md).</span></span>
