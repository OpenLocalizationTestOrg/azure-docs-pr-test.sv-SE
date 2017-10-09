---
title: aaaUse Azure portal toodeploy Azure-resurser | Microsoft Docs
description: "Använd Azure-portalen och Azure Resource Manager toodeploy dina resurser."
services: azure-resource-manager,azure-portal
documentationcenter: 
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 2c98a4aa-8d9f-4a0a-b764-214dbe8ed009
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: tomfitz
ms.openlocfilehash: 5a5217f94c8dfc0c1ebd613903ea3dcbe1197bfc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a><span data-ttu-id="345a7-103">Distribuera resurser med Resource Manager-mallar och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="345a7-103">Deploy resources with Resource Manager templates and Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="345a7-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="345a7-104">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="345a7-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="345a7-105">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="345a7-106">Portal</span><span class="sxs-lookup"><span data-stu-id="345a7-106">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="345a7-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="345a7-107">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="345a7-108">Det här avsnittet visar hur toouse hello [Azure-portalen](https://portal.azure.com) med [Azure Resource Manager](resource-group-overview.md) toodeploy Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="345a7-108">This topic shows how toouse hello [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) toodeploy your Azure resources.</span></span> <span data-ttu-id="345a7-109">toolearn om hur du hanterar dina resurser, se [hantera Azure-resurser via portalen](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-109">toolearn about managing your resources, see [Manage Azure resources through portal](resource-group-portal.md).</span></span>

<span data-ttu-id="345a7-110">Inte alla tjänsten stöder för närvarande hello-portalen eller Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="345a7-110">Currently, not every service supports hello portal or Resource Manager.</span></span> <span data-ttu-id="345a7-111">För dessa tjänster måste toouse hello [klassiska portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="345a7-111">For those services, you need toouse hello [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="345a7-112">Hello statusen för varje tjänst, se [Azure portal tillgänglighet diagram](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="345a7-112">For hello status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="345a7-113">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="345a7-113">Create resource group</span></span>
1. <span data-ttu-id="345a7-114">Välj toocreate en tom resursgrupp **ny** > **Management** > **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="345a7-114">toocreate an empty resource group, select **New** > **Management** > **Resource Group**.</span></span>
   
    ![Skapa tom resursgrupp](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. <span data-ttu-id="345a7-116">Ge det ett namn och en plats och, om det behövs väljer du en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="345a7-116">Give it a name and location, and, if necessary, select a subscription.</span></span> <span data-ttu-id="345a7-117">Du behöver tooprovide en plats för resursgruppen hello eftersom hello resursgruppen lagras metadata om hello resurser.</span><span class="sxs-lookup"><span data-stu-id="345a7-117">You need tooprovide a location for hello resource group because hello resource group stores metadata about hello resources.</span></span> <span data-ttu-id="345a7-118">Av kompatibilitetsskäl vill du kanske toospecify där den metadata lagras.</span><span class="sxs-lookup"><span data-stu-id="345a7-118">For compliance reasons, you may want toospecify where that metadata is stored.</span></span> <span data-ttu-id="345a7-119">I allmänhet rekommenderar vi att du anger en plats där de flesta av dina resurser ska placeras.</span><span class="sxs-lookup"><span data-stu-id="345a7-119">In general, we recommend that you specify a location where most of your resources will reside.</span></span> <span data-ttu-id="345a7-120">Med hjälp av hello samma plats kan förenkla din mall.</span><span class="sxs-lookup"><span data-stu-id="345a7-120">Using hello same location can simplify your template.</span></span>
   
    ![gruppvärden](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a><span data-ttu-id="345a7-122">Distribuera resurser från Marketplace</span><span class="sxs-lookup"><span data-stu-id="345a7-122">Deploy resources from Marketplace</span></span>
<span data-ttu-id="345a7-123">När du har skapat en resursgrupp kan du distribuera resurser tooit från hello Marketplace.</span><span class="sxs-lookup"><span data-stu-id="345a7-123">After you create a resource group, you can deploy resources tooit from hello Marketplace.</span></span> <span data-ttu-id="345a7-124">hello Marketplace innehåller fördefinierade lösningar för vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="345a7-124">hello Marketplace provides pre-defined solutions for common scenarios.</span></span>

1. <span data-ttu-id="345a7-125">Välj toostart en distribution **ny** och hello typ av resurs som toodeploy.</span><span class="sxs-lookup"><span data-stu-id="345a7-125">toostart a deployment, select **New** and hello type of resource you would like toodeploy.</span></span> <span data-ttu-id="345a7-126">Leta sedan för hello viss version av hello resurs som toodeploy.</span><span class="sxs-lookup"><span data-stu-id="345a7-126">Then, look for hello particular version of hello resource you would like toodeploy.</span></span>
   
    ![distribuera resurs](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. <span data-ttu-id="345a7-128">Om du inte ser hello som toodeploy lösning kan du söka hello Marketplace för den.</span><span class="sxs-lookup"><span data-stu-id="345a7-128">If you do not see hello particular solution you would like toodeploy, you can search hello Marketplace for it.</span></span>
   
    ![Sök på marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. <span data-ttu-id="345a7-130">Beroende på hello typ av valda resurs har du en samling med relevanta egenskaper tooset före distributionen.</span><span class="sxs-lookup"><span data-stu-id="345a7-130">Depending on hello type of selected resource, you have a collection of relevant properties tooset before deployment.</span></span> <span data-ttu-id="345a7-131">Dessa alternativ visas inte här, eftersom de kan variera, beroende på resurstypen.</span><span class="sxs-lookup"><span data-stu-id="345a7-131">Those options are not shown here, as they vary based on resource type.</span></span> <span data-ttu-id="345a7-132">Du måste välja en resursgrupp för målet för alla typer.</span><span class="sxs-lookup"><span data-stu-id="345a7-132">For all types, you must select a destination resource group.</span></span> <span data-ttu-id="345a7-133">hello följande bild visar hur toocreate en webbapp och distribuera den toohello resursgrupp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="345a7-133">hello following image shows how toocreate a web app and deploy it toohello resource group you created.</span></span>
   
    ![Skapa resursgrupp](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    <span data-ttu-id="345a7-135">Alternativt kan du bestämma toocreate en resursgrupp när du distribuerar dina resurser.</span><span class="sxs-lookup"><span data-stu-id="345a7-135">Alternatively, you can decide toocreate a resource group when deploying your resources.</span></span> <span data-ttu-id="345a7-136">Välj **Skapa nytt** och namnge hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="345a7-136">Select **Create new** and give hello resource group a name.</span></span>
   
    ![Skapa ny resursgrupp](./media/resource-group-template-deploy-portal/select-new-group.png)
4. <span data-ttu-id="345a7-138">Börjar din distribution.</span><span class="sxs-lookup"><span data-stu-id="345a7-138">Your deployment begins.</span></span> <span data-ttu-id="345a7-139">hello distributionen kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="345a7-139">hello deployment could take a few minutes.</span></span> <span data-ttu-id="345a7-140">När hello distributionen är klar visas ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="345a7-140">When hello deployment has finished, you see a notification.</span></span>
   
    ![Visa meddelande](./media/resource-group-template-deploy-portal/view-notification.png)
5. <span data-ttu-id="345a7-142">När du har distribuerat dina resurser du kan lägga till flera resurser toohello resursgrupp med hjälp av hello **Lägg till** på hello blad för resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="345a7-142">After deploying your resources, you can add more resources toohello resource group by using hello **Add** command on hello resource group blade.</span></span>
   
    ![lägga till en resurs](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a><span data-ttu-id="345a7-144">Distribuera resurser från anpassad mall</span><span class="sxs-lookup"><span data-stu-id="345a7-144">Deploy resources from custom template</span></span>
<span data-ttu-id="345a7-145">Om du vill tooexecute en distribution, men inte använda någon av hello mallar i hello Marketplace, kan du skapa en anpassad mall som definierar hello infrastrukturen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="345a7-145">If you want tooexecute a deployment but not use any of hello templates in hello Marketplace, you can create a customized template that defines hello infrastructure for your solution.</span></span> <span data-ttu-id="345a7-146">toolearn om hur du skapar mallar finns [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-146">toolearn about creating templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

1. <span data-ttu-id="345a7-147">toodeploy en anpassad mall hello-portalen, Välj **ny**, och starta söker efter **malldistribution** tills du kan välja bland hello alternativ.</span><span class="sxs-lookup"><span data-stu-id="345a7-147">toodeploy a customized template through hello portal, select **New**, and start searching for **Template Deployment** until you can select it from hello options.</span></span>
   
    ![Sök malldistribution](./media/resource-group-template-deploy-portal/search-template.png)
2. <span data-ttu-id="345a7-149">Välj **malldistribution** från hello tillgängliga resurser.</span><span class="sxs-lookup"><span data-stu-id="345a7-149">Select **Template Deployment** from hello available resources.</span></span>
   
    ![Välj för malldistribution](./media/resource-group-template-deploy-portal/select-template.png)
3. <span data-ttu-id="345a7-151">Öppna hello tom mall som är tillgänglig för att anpassa när du startar hello malldistribution.</span><span class="sxs-lookup"><span data-stu-id="345a7-151">After launching hello template deployment, open hello blank template that is available for customizing.</span></span>
   
    ![Skapa mall](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    <span data-ttu-id="345a7-153">Lägg till hello JSON-syntax som definierar hello-resurser som du vill att toodeploy i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="345a7-153">In hello editor, add hello JSON syntax that defines hello resources you want toodeploy.</span></span> <span data-ttu-id="345a7-154">Välj **spara** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="345a7-154">Select **Save** when done.</span></span> <span data-ttu-id="345a7-155">Anvisningar om hur du skriver hello JSON syntax finns [genomgång av Resource Manager-mall](resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-155">For guidance on writing hello JSON syntax, see [Resource Manager template walkthrough](resource-manager-template-walkthrough.md).</span></span>
   
    ![redigera mall](./media/resource-group-template-deploy-portal/edit-template.png)
4. <span data-ttu-id="345a7-157">Du kan också välja en befintlig mall från hello [Azure snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="345a7-157">Or, you can select a pre-existing template from hello [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="345a7-158">Dessa mallar tillhandahålls av hello community.</span><span class="sxs-lookup"><span data-stu-id="345a7-158">These templates are contributed by hello community.</span></span> <span data-ttu-id="345a7-159">De täcker många vanliga scenarier och någon kan ha lagt till en mall som är liknande toowhat som du försöker toodeploy.</span><span class="sxs-lookup"><span data-stu-id="345a7-159">They cover many common scenarios, and someone may have added a template that is similar toowhat you are trying toodeploy.</span></span> <span data-ttu-id="345a7-160">Du kan söka hello mallar toofind något som matchar ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="345a7-160">You can search hello templates toofind something that matches your scenario.</span></span>
   
    ![Välj mall för Snabbstart](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    <span data-ttu-id="345a7-162">Du kan visa hello valda mallen i hello-redigeraren.</span><span class="sxs-lookup"><span data-stu-id="345a7-162">You can view hello selected template in hello editor.</span></span>
5. <span data-ttu-id="345a7-163">När du har angett alla hello andra värden, Välj **skapa** toodeploy hello mallen.</span><span class="sxs-lookup"><span data-stu-id="345a7-163">After providing all hello other values, select **Create** toodeploy hello template.</span></span> 
   
    ![distribuera mallen](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-tooyour-account"></a><span data-ttu-id="345a7-165">Distribuera resurser från en mall som sparats tooyour konto</span><span class="sxs-lookup"><span data-stu-id="345a7-165">Deploy resources from a template saved tooyour account</span></span>
<span data-ttu-id="345a7-166">hello portal gör toosave en mall tooyour Azure-konto och distribuera den senare.</span><span class="sxs-lookup"><span data-stu-id="345a7-166">hello portal enables you toosave a template tooyour Azure account, and redeploy it later.</span></span> <span data-ttu-id="345a7-167">Mer information om hur du arbetar med dessa sparade mallar, [Kom igång med privata mallar på hello Azure-portalen](../marketplace-consumer/mytemplates-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-167">For more information about working with these saved templates, [Get started with private templates on hello Azure portal](../marketplace-consumer/mytemplates-getstarted.md).</span></span>

1. <span data-ttu-id="345a7-168">toofind dina sparade mallar, Välj **Bläddra** > **mallar**.</span><span class="sxs-lookup"><span data-stu-id="345a7-168">toofind your saved templates, select **Browse** > **Templates**.</span></span>
   
    ![Bläddra efter mallar](./media/resource-group-template-deploy-portal/browse-templates.png)
2. <span data-ttu-id="345a7-170">Hello lista över mallar som har sparats tooyour konto, Välj hello som du vill toowork på.</span><span class="sxs-lookup"><span data-stu-id="345a7-170">From hello list of templates saved tooyour account, select hello one you wish toowork on.</span></span>
   
    ![sparade mallar](./media/resource-group-template-deploy-portal/saved-templates.png)
3. <span data-ttu-id="345a7-172">Välj **distribuera** tooredeploy detta spara mallen.</span><span class="sxs-lookup"><span data-stu-id="345a7-172">Select **Deploy** tooredeploy this saved template.</span></span>
   
    ![distribuera sparade mallen](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a><span data-ttu-id="345a7-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="345a7-174">Next steps</span></span>
* <span data-ttu-id="345a7-175">tooview granskningsloggarna finns [granskningsåtgärder med Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-175">tooview audit logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="345a7-176">tootroubleshoot distributionsfel finns [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-176">tootroubleshoot deployment errors, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="345a7-177">tooretrieve en mall från en distribution eller resursgrupp, se [exportera Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-177">tooretrieve a template from a deployment or resource group, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="345a7-178">Anvisningar om hur företag kan använda Resource Manager tooeffectively hantera prenumerationer, se [kodskelett Azure enterprise - normativ prenumeration styrning](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="345a7-178">For guidance on how enterprises can use Resource Manager tooeffectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

