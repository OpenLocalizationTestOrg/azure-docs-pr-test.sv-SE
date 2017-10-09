---
title: aaaUse Azure portal toomanage Azure-resurser | Microsoft Docs
description: "Använd Azure-portalen och Azure Resource Manager toomanage dina resurser. Visar hur toowork med instrumentpaneler toomonitor resurser."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 0725bbf2-5913-4c07-af6e-24e11d957fbc
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 0c89a197a31c5b6dd03ba457cb4d1fdf9f6d00f6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-resources-through-portal"></a><span data-ttu-id="fb525-104">Hantera Azure-resurser via portalen</span><span class="sxs-lookup"><span data-stu-id="fb525-104">Manage Azure resources through portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="fb525-105">Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="fb525-105">Azure PowerShell</span></span>](powershell-azure-resource-manager.md)
> * [<span data-ttu-id="fb525-106">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="fb525-106">Azure CLI</span></span>](xplat-cli-azure-resource-manager.md)
> * [<span data-ttu-id="fb525-107">Portal</span><span class="sxs-lookup"><span data-stu-id="fb525-107">Portal</span></span>](resource-group-portal.md) 
> * [<span data-ttu-id="fb525-108">REST-API</span><span class="sxs-lookup"><span data-stu-id="fb525-108">REST API</span></span>](resource-manager-rest-api.md)
> 
> 

<span data-ttu-id="fb525-109">Det här avsnittet visar hur toouse hello [Azure-portalen](https://portal.azure.com) med [Azure Resource Manager](resource-group-overview.md) toomanage Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="fb525-109">This topic shows how toouse hello [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) toomanage your Azure resources.</span></span> <span data-ttu-id="fb525-110">toolearn om hur du distribuerar resurser via hello portal finns [distribuera resurser med Resource Manager-mallar och Azure-portalen](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-110">toolearn about deploying resources through hello portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>

<span data-ttu-id="fb525-111">Inte alla tjänsten stöder för närvarande hello-portalen eller Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="fb525-111">Currently, not every service supports hello portal or Resource Manager.</span></span> <span data-ttu-id="fb525-112">För dessa tjänster måste toouse hello [klassiska portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="fb525-112">For those services, you need toouse hello [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="fb525-113">Hello statusen för varje tjänst, se [Azure portal tillgänglighet diagram](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="fb525-113">For hello status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="manage-resource-groups"></a><span data-ttu-id="fb525-114">Hantera resursgrupper</span><span class="sxs-lookup"><span data-stu-id="fb525-114">Manage resource groups</span></span>

<span data-ttu-id="fb525-115">En resursgrupp är en behållare som innehåller relaterade resurser för en Azure-lösning.</span><span class="sxs-lookup"><span data-stu-id="fb525-115">A resource group is a container that holds related resources for an Azure solution.</span></span> <span data-ttu-id="fb525-116">hello resursgrupp kan innehålla alla hello resurser för hello lösning eller bara de resurser som du vill toomanage som en grupp.</span><span class="sxs-lookup"><span data-stu-id="fb525-116">hello resource group can include all hello resources for hello solution, or only those resources that you want toomanage as a group.</span></span> <span data-ttu-id="fb525-117">Du bestämma hur du vill att tooallocate resurser tooresource grupper baserat på vad som är hello bäst för din organisation.</span><span class="sxs-lookup"><span data-stu-id="fb525-117">You decide how you want tooallocate resources tooresource groups based on what makes hello most sense for your organization.</span></span> <span data-ttu-id="fb525-118">I allmänhet kan lägga till resurser som delar hello samma livscykel toohello samma resursgrupp, så att du enkelt kan distribuera, uppdatera och ta bort dem som en grupp.</span><span class="sxs-lookup"><span data-stu-id="fb525-118">Generally, add resources that share hello same lifecycle toohello same resource group so you can easily deploy, update, and delete them as a group.</span></span> 

<span data-ttu-id="fb525-119">hello resursgruppen lagras metadata om hello resurser.</span><span class="sxs-lookup"><span data-stu-id="fb525-119">hello resource group stores metadata about hello resources.</span></span> <span data-ttu-id="fb525-120">När du anger en plats för hello resursgrupp kan anger du därför där den metadata lagras.</span><span class="sxs-lookup"><span data-stu-id="fb525-120">Therefore, when you specify a location for hello resource group, you are specifying where that metadata is stored.</span></span> <span data-ttu-id="fb525-121">Av kompatibilitetsskäl, kanske du måste tooensure som dina data lagras i ett visst område.</span><span class="sxs-lookup"><span data-stu-id="fb525-121">For compliance reasons, you may need tooensure that your data is stored in a particular region.</span></span>

1. <span data-ttu-id="fb525-122">toosee alla hello resursgrupper i din prenumeration, Välj **resursgrupper**.</span><span class="sxs-lookup"><span data-stu-id="fb525-122">toosee all hello resource groups in your subscription, select **Resource groups**.</span></span>
   
    ![Bläddra resursgrupper](./media/resource-group-portal/browse-groups.png)
2. <span data-ttu-id="fb525-124">Välj toocreate en tom resursgrupp **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="fb525-124">toocreate an empty resource group, select **Add**.</span></span>
   
    ![Lägg till resursgrupp](./media/resource-group-portal/add-resource-group.png)
3. <span data-ttu-id="fb525-126">Ange ett namn och plats för hello ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fb525-126">Provide a name and location for hello new resource group.</span></span> <span data-ttu-id="fb525-127">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="fb525-127">Select **Create**.</span></span>
   
    ![Skapa resursgrupp](./media/resource-group-portal/create-empty-group.png)
4. <span data-ttu-id="fb525-129">Du kan behöva tooselect **uppdatera** toosee hello nyligen skapade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="fb525-129">You may need tooselect **Refresh** toosee hello recently created resource group.</span></span>
   
    ![Uppdatera resursgruppen.](./media/resource-group-portal/refresh-resource-groups.png)
5. <span data-ttu-id="fb525-131">Välj toocustomize hello information som visas för dina resursgrupper **kolumner**.</span><span class="sxs-lookup"><span data-stu-id="fb525-131">toocustomize hello information displayed for your resource groups, select **Columns**.</span></span>
   
    ![Anpassa kolumner](./media/resource-group-portal/select-columns.png)
6. <span data-ttu-id="fb525-133">Välj hello kolumner tooadd och välj sedan **uppdatering**.</span><span class="sxs-lookup"><span data-stu-id="fb525-133">Select hello columns tooadd, and then select **Update**.</span></span>
   
    ![lägga till kolumner](./media/resource-group-portal/add-columns.png)
7. <span data-ttu-id="fb525-135">toolearn om hur du distribuerar resurser tooyour ny resursgrupp finns [distribuera resurser med Resource Manager-mallar och Azure-portalen](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-135">toolearn about deploying resources tooyour new resource group, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
8. <span data-ttu-id="fb525-136">Du kan fästa hello bladet tooyour instrumentpanelen för Snabbåtkomst tooa resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="fb525-136">For quick access tooa resource group, you can pin hello blade tooyour dashboard.</span></span>
   
    ![resursgruppen för PIN-kod](./media/resource-group-portal/pin-group.png)
9. <span data-ttu-id="fb525-138">hello instrumentpanelen visar hello resursgrupp och dess resurser.</span><span class="sxs-lookup"><span data-stu-id="fb525-138">hello dashboard displays hello resource group and its resources.</span></span> <span data-ttu-id="fb525-139">Du kan välja hello resursgrupper eller någon av dess resurser toonavigate toohello objekt.</span><span class="sxs-lookup"><span data-stu-id="fb525-139">You can select either hello resource groups or any of its resources toonavigate toohello item.</span></span>
   
    ![resursgruppen för PIN-kod](./media/resource-group-portal/show-resource-group-dashboard.png)

## <a name="tag-resources"></a><span data-ttu-id="fb525-141">Taggen resurser</span><span class="sxs-lookup"><span data-stu-id="fb525-141">Tag resources</span></span>
<span data-ttu-id="fb525-142">Du kan använda taggar tooresource grupper och resurser toologically ordna dina tillgångar.</span><span class="sxs-lookup"><span data-stu-id="fb525-142">You can apply tags tooresource groups and resources toologically organize your assets.</span></span> <span data-ttu-id="fb525-143">Information om hur du arbetar med taggar finns [med hjälp av taggar tooorganize resurserna i Azure](resource-group-using-tags.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-143">For information about working with tags, see [Using tags tooorganize your Azure resources](resource-group-using-tags.md).</span></span>

[!INCLUDE [resource-manager-tag-resource](../../includes/resource-manager-tag-resources.md)]

## <a name="monitor-resources"></a><span data-ttu-id="fb525-144">Övervaka resurser</span><span class="sxs-lookup"><span data-stu-id="fb525-144">Monitor resources</span></span>
<span data-ttu-id="fb525-145">När du väljer en resurs, visar hello resursbladet standard diagram och tabeller för att övervaka den resurstypen.</span><span class="sxs-lookup"><span data-stu-id="fb525-145">When you select a resource, hello resource blade presents default graphs and tables for monitoring that resource type.</span></span>

1. <span data-ttu-id="fb525-146">Välj en resurs och Observera hello **övervakning** avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb525-146">Select a resource and notice hello **Monitoring** section.</span></span> <span data-ttu-id="fb525-147">Den omfattar diagram som är relevanta toohello resurstypen.</span><span class="sxs-lookup"><span data-stu-id="fb525-147">It includes graphs that are relevant toohello resource type.</span></span> <span data-ttu-id="fb525-148">hello visar följande bild hello standard övervakningsdata för ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="fb525-148">hello following image shows hello default monitoring data for a storage account.</span></span>
   
    ![Visa övervakning](./media/resource-group-portal/show-monitoring.png)
2. <span data-ttu-id="fb525-150">Du kan fästa en del av hello bladet tooyour instrumentpanelen genom att välja hello ellips (...) ovanför hello-avsnittet.</span><span class="sxs-lookup"><span data-stu-id="fb525-150">You can pin a section of hello blade tooyour dashboard by selecting hello ellipsis (...) above hello section.</span></span> <span data-ttu-id="fb525-151">Du kan också anpassa hello storlek hello avsnitt i hello bladet eller ta bort den helt.</span><span class="sxs-lookup"><span data-stu-id="fb525-151">You can also customize hello size hello section in hello blade or remove it completely.</span></span> <span data-ttu-id="fb525-152">hello följande bild visar hur toopin, anpassa eller ta bort hello CPU och minne avsnitt.</span><span class="sxs-lookup"><span data-stu-id="fb525-152">hello following image shows how toopin, customize, or remove hello CPU and Memory section.</span></span>
   
    ![avsnittet för PIN-kod](./media/resource-group-portal/pin-cpu-section.png)
3. <span data-ttu-id="fb525-154">Efter att fästa hello avsnittet toohello instrumentpanelen visas hello sammanfattning på hello instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="fb525-154">After pinning hello section toohello dashboard, you will see hello summary on hello dashboard.</span></span> <span data-ttu-id="fb525-155">Och markera den omedelbart tar toomore information om hello data.</span><span class="sxs-lookup"><span data-stu-id="fb525-155">And, selecting it immediately takes you toomore details about hello data.</span></span>
   
    ![visa instrumentpanelen](./media/resource-group-portal/view-startboard.png)
4. <span data-ttu-id="fb525-157">toocompletely anpassa hello data du övervaka hello-portalen, navigera tooyour standardinstrumentpanelen och välj **ny instrumentpanel**.</span><span class="sxs-lookup"><span data-stu-id="fb525-157">toocompletely customize hello data you monitor through hello portal, navigate tooyour default dashboard, and select **New dashboard**.</span></span>
   
    ![instrumentpanel](./media/resource-group-portal/dashboard.png)
5. <span data-ttu-id="fb525-159">Namnge din nya instrumentpanel och dra paneler till hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="fb525-159">Give your new dashboard a name and drag tiles onto hello dashboard.</span></span> <span data-ttu-id="fb525-160">hello paneler filtreras efter de olika alternativ.</span><span class="sxs-lookup"><span data-stu-id="fb525-160">hello tiles are filtered by different options.</span></span>
   
    ![instrumentpanel](./media/resource-group-portal/create-dashboard.png)
   
     <span data-ttu-id="fb525-162">toolearn om hur du arbetar med instrumentpaneler, se [skapa och dela instrumentpaneler i hello Azure-portalen](../azure-portal/azure-portal-dashboards.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-162">toolearn about working with dashboards, see [Creating and sharing dashboards in hello Azure portal](../azure-portal/azure-portal-dashboards.md).</span></span>

## <a name="manage-resources"></a><span data-ttu-id="fb525-163">Hantera resurser</span><span class="sxs-lookup"><span data-stu-id="fb525-163">Manage resources</span></span>
<span data-ttu-id="fb525-164">Hello alternativ för att hantera hello resurs visas hello bladet för en resurs som.</span><span class="sxs-lookup"><span data-stu-id="fb525-164">In hello blade for a resource, you see hello options for managing hello resource.</span></span> <span data-ttu-id="fb525-165">hello portal anger alternativ för den specifika resurstypen.</span><span class="sxs-lookup"><span data-stu-id="fb525-165">hello portal presents management options for that particular resource type.</span></span> <span data-ttu-id="fb525-166">Du ser hello management kommandon överst hello hello resursbladet och hello vänster.</span><span class="sxs-lookup"><span data-stu-id="fb525-166">You see hello management commands across hello top of hello resource blade and on hello left side.</span></span>

![Hantera resurser](./media/resource-group-portal/manage-resources.png)

<span data-ttu-id="fb525-168">Du kan utföra åtgärder som till exempel starta och stoppa en virtuell dator och konfigurera om hello egenskaper för hello virtuell dator från dessa alternativ.</span><span class="sxs-lookup"><span data-stu-id="fb525-168">From these options, you can perform operations such as starting and stopping a virtual machine, or reconfiguring hello properties of hello virtual machine.</span></span>

## <a name="move-resources"></a><span data-ttu-id="fb525-169">Flytta resurser</span><span class="sxs-lookup"><span data-stu-id="fb525-169">Move resources</span></span>
<span data-ttu-id="fb525-170">Om du behöver toomove resurser tooanother resursgruppens namn eller en annan prenumeration, se [flytta resurser toonew resursgrupp eller prenumeration](resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-170">If you need toomove resources tooanother resource group or another subscription, see [Move resources toonew resource group or subscription](resource-group-move-resources.md).</span></span>

## <a name="lock-resources"></a><span data-ttu-id="fb525-171">Lås resurser</span><span class="sxs-lookup"><span data-stu-id="fb525-171">Lock resources</span></span>
<span data-ttu-id="fb525-172">Du kan låsa en prenumeration, resursgrupp eller resurs tooprevent andra användare i din organisation av misstag tas bort eller ändra viktiga resurser.</span><span class="sxs-lookup"><span data-stu-id="fb525-172">You can lock a subscription, resource group, or resource tooprevent other users in your organization from accidentally deleting or modifying critical resources.</span></span> <span data-ttu-id="fb525-173">Mer information finns i [Låsa resurser med Azure Resource Manager](resource-group-lock-resources.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-173">For more information, see [Lock resources with Azure Resource Manager](resource-group-lock-resources.md).</span></span>

[!INCLUDE [resource-manager-lock-resources](../../includes/resource-manager-lock-resources.md)]

## <a name="view-your-subscription-and-costs"></a><span data-ttu-id="fb525-174">Visa din prenumeration och kostnader</span><span class="sxs-lookup"><span data-stu-id="fb525-174">View your subscription and costs</span></span>
<span data-ttu-id="fb525-175">Du kan visa information om din prenumeration och hello upplyfta kostnader för alla resurser.</span><span class="sxs-lookup"><span data-stu-id="fb525-175">You can view information about your subscription and hello rolled-up costs for all your resources.</span></span> <span data-ttu-id="fb525-176">Välj **prenumerationer** och hello-prenumeration som du vill toosee.</span><span class="sxs-lookup"><span data-stu-id="fb525-176">Select **Subscriptions** and hello subscription you want toosee.</span></span> <span data-ttu-id="fb525-177">Du kan bara ha en prenumeration tooselect.</span><span class="sxs-lookup"><span data-stu-id="fb525-177">You might only have one subscription tooselect.</span></span>

![prenumeration](./media/resource-group-portal/select-subscription.png)

<span data-ttu-id="fb525-179">I hello prenumerationsbladet se en bränna hastighet.</span><span class="sxs-lookup"><span data-stu-id="fb525-179">Within hello subscription blade, you see a burn rate.</span></span>

![bränna hastighet](./media/resource-group-portal/burn-rate.png)

<span data-ttu-id="fb525-181">Och en uppdelning av kostnader efter resurstyp.</span><span class="sxs-lookup"><span data-stu-id="fb525-181">And, a breakdown of costs by resource type.</span></span>

![resurskostnader](./media/resource-group-portal/cost-by-resource.png)

## <a name="export-template"></a><span data-ttu-id="fb525-183">Exportera mall</span><span class="sxs-lookup"><span data-stu-id="fb525-183">Export template</span></span>
<span data-ttu-id="fb525-184">När du har installerat din resursgrupp, vill du kanske tooview hello Resource Manager-mall för hello resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fb525-184">After setting up your resource group, you may want tooview hello Resource Manager template for hello resource group.</span></span> <span data-ttu-id="fb525-185">Exporterar hello mallen har två fördelar:</span><span class="sxs-lookup"><span data-stu-id="fb525-185">Exporting hello template offers two benefits:</span></span>

1. <span data-ttu-id="fb525-186">Enkelt kan du automatisera framtida distributioner av hello lösning eftersom hello mallen innehåller alla hello hela infrastrukturen.</span><span class="sxs-lookup"><span data-stu-id="fb525-186">You can easily automate future deployments of hello solution because hello template contains all hello complete infrastructure.</span></span>
2. <span data-ttu-id="fb525-187">Du kan bekanta dig med mallens syntax genom att titta på hello JavaScript Object Notation (JSON) som representerar din lösning.</span><span class="sxs-lookup"><span data-stu-id="fb525-187">You can become familiar with template syntax by looking at hello JavaScript Object Notation (JSON) that represents your solution.</span></span>

<span data-ttu-id="fb525-188">Detaljerade anvisningar finns [exportera Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-188">For step-by-step guidance, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>

## <a name="delete-resource-group-or-resources"></a><span data-ttu-id="fb525-189">Ta bort resursgruppen eller resurser</span><span class="sxs-lookup"><span data-stu-id="fb525-189">Delete resource group or resources</span></span>
<span data-ttu-id="fb525-190">Tar bort en resursgrupp alla hello-resurser som ingår i den.</span><span class="sxs-lookup"><span data-stu-id="fb525-190">Deleting a resource group deletes all hello resources contained within it.</span></span> <span data-ttu-id="fb525-191">Du kan också ta bort enskilda resurser inom en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="fb525-191">You can also delete individual resources within a resource group.</span></span> <span data-ttu-id="fb525-192">Du vill tooexercise försiktig när du tar bort en resursgrupp eftersom det kan finnas resurser i andra resursgrupper som är länkade tooit.</span><span class="sxs-lookup"><span data-stu-id="fb525-192">You want tooexercise caution when you delete a resource group because there might be resources in other resource groups that are linked tooit.</span></span> <span data-ttu-id="fb525-193">Resource Manager tas inte bort länkade resurser, men de kan inte fungera utan hello förväntades resurser.</span><span class="sxs-lookup"><span data-stu-id="fb525-193">Resource Manager does not delete linked resources, but they may not operate correctly without hello expected resources.</span></span>

![Ta bort grupp](./media/resource-group-portal/delete-group.png)

## <a name="next-steps"></a><span data-ttu-id="fb525-195">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb525-195">Next steps</span></span>
* <span data-ttu-id="fb525-196">tooview aktivitetsloggar finns [granskningsåtgärder med Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-196">tooview activity logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="fb525-197">tooview information om en distribution finns [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-197">tooview details about a deployment, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="fb525-198">toodeploy resurser via hello portal finns [distribuera resurser med Resource Manager-mallar och Azure-portalen](resource-group-template-deploy-portal.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-198">toodeploy resources through hello portal, see [Deploy resources with Resource Manager templates and Azure portal](resource-group-template-deploy-portal.md).</span></span>
* <span data-ttu-id="fb525-199">toomanage åtkomst tooresources finns [använda rollen tilldelningar toomanage åtkomst tooyour Azure-prenumerationsresurser](../active-directory/role-based-access-control-configure.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-199">toomanage access tooresources, see [Use role assignments toomanage access tooyour Azure subscription resources](../active-directory/role-based-access-control-configure.md).</span></span>
* <span data-ttu-id="fb525-200">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="fb525-200">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

