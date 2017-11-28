---
title: "Använda Azure-portalen för att distribuera Azure-resurser | Microsoft Docs"
description: "Använd Azure-portalen och Azure Resource Manager för att distribuera dina resurser."
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
ms.openlocfilehash: a4cac5834c667976b0a2d1f2748e4309a166d16a
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="deploy-resources-with-resource-manager-templates-and-azure-portal"></a><span data-ttu-id="ff589-103">Distribuera resurser med Resource Manager-mallar och Azure Portal</span><span class="sxs-lookup"><span data-stu-id="ff589-103">Deploy resources with Resource Manager templates and Azure portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ff589-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="ff589-104">PowerShell</span></span>](resource-group-template-deploy.md)
> * [<span data-ttu-id="ff589-105">Azure CLI</span><span class="sxs-lookup"><span data-stu-id="ff589-105">Azure CLI</span></span>](resource-group-template-deploy-cli.md)
> * [<span data-ttu-id="ff589-106">Portal</span><span class="sxs-lookup"><span data-stu-id="ff589-106">Portal</span></span>](resource-group-template-deploy-portal.md)
> * [<span data-ttu-id="ff589-107">REST-API</span><span class="sxs-lookup"><span data-stu-id="ff589-107">REST API</span></span>](resource-group-template-deploy-rest.md)
> 
> 

<span data-ttu-id="ff589-108">Det här avsnittet visar hur du använder den [Azure-portalen](https://portal.azure.com) med [Azure Resource Manager](resource-group-overview.md) att distribuera Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="ff589-108">This topic shows how to use the [Azure portal](https://portal.azure.com) with [Azure Resource Manager](resource-group-overview.md) to deploy your Azure resources.</span></span> <span data-ttu-id="ff589-109">Läs om hur du hanterar dina resurser i [hantera Azure-resurser via portalen](resource-group-portal.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-109">To learn about managing your resources, see [Manage Azure resources through portal](resource-group-portal.md).</span></span>

<span data-ttu-id="ff589-110">Inte alla tjänsten stöder för närvarande portalen eller Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="ff589-110">Currently, not every service supports the portal or Resource Manager.</span></span> <span data-ttu-id="ff589-111">För dessa tjänster måste du använda den [klassiska portalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="ff589-111">For those services, you need to use the [classic portal](https://manage.windowsazure.com).</span></span> <span data-ttu-id="ff589-112">Statusen för varje tjänst, se [Azure portal tillgänglighet diagram](https://azure.microsoft.com/features/azure-portal/availability/).</span><span class="sxs-lookup"><span data-stu-id="ff589-112">For the status of each service, see [Azure portal availability chart](https://azure.microsoft.com/features/azure-portal/availability/).</span></span>

## <a name="create-resource-group"></a><span data-ttu-id="ff589-113">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="ff589-113">Create resource group</span></span>
1. <span data-ttu-id="ff589-114">Om du vill skapa en tom resursgrupp **ny** > **Management** > **resursgruppen**.</span><span class="sxs-lookup"><span data-stu-id="ff589-114">To create an empty resource group, select **New** > **Management** > **Resource Group**.</span></span>
   
    ![Skapa tom resursgrupp](./media/resource-group-template-deploy-portal/create-empty-group.png)
2. <span data-ttu-id="ff589-116">Ge det ett namn och en plats och, om det behövs väljer du en prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ff589-116">Give it a name and location, and, if necessary, select a subscription.</span></span> <span data-ttu-id="ff589-117">Du måste ange en plats för resursgruppen eftersom resursgruppen lagras metadata om resurserna.</span><span class="sxs-lookup"><span data-stu-id="ff589-117">You need to provide a location for the resource group because the resource group stores metadata about the resources.</span></span> <span data-ttu-id="ff589-118">Av kompatibilitetsskäl, kanske du vill ange var den metadata lagras.</span><span class="sxs-lookup"><span data-stu-id="ff589-118">For compliance reasons, you may want to specify where that metadata is stored.</span></span> <span data-ttu-id="ff589-119">I allmänhet rekommenderar vi att du anger en plats där de flesta av dina resurser ska placeras.</span><span class="sxs-lookup"><span data-stu-id="ff589-119">In general, we recommend that you specify a location where most of your resources will reside.</span></span> <span data-ttu-id="ff589-120">Med hjälp av samma plats kan förenkla din mall.</span><span class="sxs-lookup"><span data-stu-id="ff589-120">Using the same location can simplify your template.</span></span>
   
    ![gruppvärden](./media/resource-group-template-deploy-portal/set-group-properties.png)

## <a name="deploy-resources-from-marketplace"></a><span data-ttu-id="ff589-122">Distribuera resurser från Marketplace</span><span class="sxs-lookup"><span data-stu-id="ff589-122">Deploy resources from Marketplace</span></span>
<span data-ttu-id="ff589-123">När du har skapat en resursgrupp kan du distribuera resurser till den från Marketplace.</span><span class="sxs-lookup"><span data-stu-id="ff589-123">After you create a resource group, you can deploy resources to it from the Marketplace.</span></span> <span data-ttu-id="ff589-124">Marketplace innehåller fördefinierade lösningar för vanliga scenarier.</span><span class="sxs-lookup"><span data-stu-id="ff589-124">The Marketplace provides pre-defined solutions for common scenarios.</span></span>

1. <span data-ttu-id="ff589-125">Om du vill starta en distribution väljer **ny** och typ av resurs som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="ff589-125">To start a deployment, select **New** and the type of resource you would like to deploy.</span></span> <span data-ttu-id="ff589-126">Leta efter en viss version av resursen som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="ff589-126">Then, look for the particular version of the resource you would like to deploy.</span></span>
   
    ![distribuera resurs](./media/resource-group-template-deploy-portal/deploy-resource.png)
2. <span data-ttu-id="ff589-128">Om du inte ser den lösning som du vill distribuera, kan du söka Marketplace för den.</span><span class="sxs-lookup"><span data-stu-id="ff589-128">If you do not see the particular solution you would like to deploy, you can search the Marketplace for it.</span></span>
   
    ![Sök på marketplace](./media/resource-group-template-deploy-portal/search-resource.png)
3. <span data-ttu-id="ff589-130">Beroende på vilken typ av markerade resursen har en samling relevanta egenskaper för att ange före distributionen.</span><span class="sxs-lookup"><span data-stu-id="ff589-130">Depending on the type of selected resource, you have a collection of relevant properties to set before deployment.</span></span> <span data-ttu-id="ff589-131">Dessa alternativ visas inte här, eftersom de kan variera, beroende på resurstypen.</span><span class="sxs-lookup"><span data-stu-id="ff589-131">Those options are not shown here, as they vary based on resource type.</span></span> <span data-ttu-id="ff589-132">Du måste välja en resursgrupp för målet för alla typer.</span><span class="sxs-lookup"><span data-stu-id="ff589-132">For all types, you must select a destination resource group.</span></span> <span data-ttu-id="ff589-133">Följande bild visar hur du skapar en webbapp och distribuera den till den resursgrupp som du skapade.</span><span class="sxs-lookup"><span data-stu-id="ff589-133">The following image shows how to create a web app and deploy it to the resource group you created.</span></span>
   
    ![Skapa resursgrupp](./media/resource-group-template-deploy-portal/select-existing-group.png)
   
    <span data-ttu-id="ff589-135">Du kan också välja att skapa en resursgrupp när du distribuerar dina resurser.</span><span class="sxs-lookup"><span data-stu-id="ff589-135">Alternatively, you can decide to create a resource group when deploying your resources.</span></span> <span data-ttu-id="ff589-136">Välj **Skapa nytt** och namnge resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ff589-136">Select **Create new** and give the resource group a name.</span></span>
   
    ![Skapa ny resursgrupp](./media/resource-group-template-deploy-portal/select-new-group.png)
4. <span data-ttu-id="ff589-138">Börjar din distribution.</span><span class="sxs-lookup"><span data-stu-id="ff589-138">Your deployment begins.</span></span> <span data-ttu-id="ff589-139">Distributionen kan ta några minuter.</span><span class="sxs-lookup"><span data-stu-id="ff589-139">The deployment could take a few minutes.</span></span> <span data-ttu-id="ff589-140">När distributionen är klar visas ett meddelande.</span><span class="sxs-lookup"><span data-stu-id="ff589-140">When the deployment has finished, you see a notification.</span></span>
   
    ![Visa meddelande](./media/resource-group-template-deploy-portal/view-notification.png)
5. <span data-ttu-id="ff589-142">När du har distribuerat dina resurser, du kan lägga till fler resurser i resursgruppen med hjälp av den **Lägg till** på bladet för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="ff589-142">After deploying your resources, you can add more resources to the resource group by using the **Add** command on the resource group blade.</span></span>
   
    ![lägga till en resurs](./media/resource-group-template-deploy-portal/add-resource.png)

## <a name="deploy-resources-from-custom-template"></a><span data-ttu-id="ff589-144">Distribuera resurser från anpassad mall</span><span class="sxs-lookup"><span data-stu-id="ff589-144">Deploy resources from custom template</span></span>
<span data-ttu-id="ff589-145">Om du vill köra en distribution utan att använda någon av mallar i Marketplace, kan du skapa en anpassad mall som definierar infrastrukturen för lösningen.</span><span class="sxs-lookup"><span data-stu-id="ff589-145">If you want to execute a deployment but not use any of the templates in the Marketplace, you can create a customized template that defines the infrastructure for your solution.</span></span> <span data-ttu-id="ff589-146">Läs om hur du skapar mallar i [redigera Azure Resource Manager-mallar](resource-group-authoring-templates.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-146">To learn about creating templates, see [Authoring Azure Resource Manager templates](resource-group-authoring-templates.md).</span></span>

1. <span data-ttu-id="ff589-147">Om du vill distribuera en anpassad mall via portalen, Välj **ny**, och starta söker efter **malldistribution** tills du kan välja bland alternativ.</span><span class="sxs-lookup"><span data-stu-id="ff589-147">To deploy a customized template through the portal, select **New**, and start searching for **Template Deployment** until you can select it from the options.</span></span>
   
    ![Sök malldistribution](./media/resource-group-template-deploy-portal/search-template.png)
2. <span data-ttu-id="ff589-149">Välj **malldistribution** från de tillgängliga resurserna.</span><span class="sxs-lookup"><span data-stu-id="ff589-149">Select **Template Deployment** from the available resources.</span></span>
   
    ![Välj för malldistribution](./media/resource-group-template-deploy-portal/select-template.png)
3. <span data-ttu-id="ff589-151">Öppna den tomma mallen som är tillgänglig för att anpassa när du startar malldistributionen av.</span><span class="sxs-lookup"><span data-stu-id="ff589-151">After launching the template deployment, open the blank template that is available for customizing.</span></span>
   
    ![Skapa mall](./media/resource-group-template-deploy-portal/show-custom-template.png)
   
    <span data-ttu-id="ff589-153">I redigeraren, lägger du till JSON-syntax som definierar de resurser som du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="ff589-153">In the editor, add the JSON syntax that defines the resources you want to deploy.</span></span> <span data-ttu-id="ff589-154">Välj **spara** när du är klar.</span><span class="sxs-lookup"><span data-stu-id="ff589-154">Select **Save** when done.</span></span> <span data-ttu-id="ff589-155">Anvisningar om hur du skriver JSON-syntax finns [genomgång av Resource Manager-mall](resource-manager-template-walkthrough.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-155">For guidance on writing the JSON syntax, see [Resource Manager template walkthrough](resource-manager-template-walkthrough.md).</span></span>
   
    ![redigera mall](./media/resource-group-template-deploy-portal/edit-template.png)
4. <span data-ttu-id="ff589-157">Du kan också välja en befintlig mall från den [Azure snabbstartsmallar](https://azure.microsoft.com/documentation/templates/).</span><span class="sxs-lookup"><span data-stu-id="ff589-157">Or, you can select a pre-existing template from the [Azure quickstart templates](https://azure.microsoft.com/documentation/templates/).</span></span> <span data-ttu-id="ff589-158">Dessa mallar tillhandahålls av gruppen.</span><span class="sxs-lookup"><span data-stu-id="ff589-158">These templates are contributed by the community.</span></span> <span data-ttu-id="ff589-159">De täcker många vanliga scenarier och någon kan ha lagt till en mall som liknar vad du vill distribuera.</span><span class="sxs-lookup"><span data-stu-id="ff589-159">They cover many common scenarios, and someone may have added a template that is similar to what you are trying to deploy.</span></span> <span data-ttu-id="ff589-160">Du kan söka mallar för att hitta något som passar ditt scenario.</span><span class="sxs-lookup"><span data-stu-id="ff589-160">You can search the templates to find something that matches your scenario.</span></span>
   
    ![Välj mall för Snabbstart](./media/resource-group-template-deploy-portal/select-quickstart-template.png)
   
    <span data-ttu-id="ff589-162">Du kan visa den valda mallen i redigeraren.</span><span class="sxs-lookup"><span data-stu-id="ff589-162">You can view the selected template in the editor.</span></span>
5. <span data-ttu-id="ff589-163">När du har angett alla andra värden, Välj **skapa** att distribuera mallen.</span><span class="sxs-lookup"><span data-stu-id="ff589-163">After providing all the other values, select **Create** to deploy the template.</span></span> 
   
    ![distribuera mallen](./media/resource-group-template-deploy-portal/create-custom-deploy.png)

## <a name="deploy-resources-from-a-template-saved-to-your-account"></a><span data-ttu-id="ff589-165">Distribuera resurser från en mall som sparats i ditt konto</span><span class="sxs-lookup"><span data-stu-id="ff589-165">Deploy resources from a template saved to your account</span></span>
<span data-ttu-id="ff589-166">Portalen kan du spara en mall i Azure-konto och distribuera den senare.</span><span class="sxs-lookup"><span data-stu-id="ff589-166">The portal enables you to save a template to your Azure account, and redeploy it later.</span></span> <span data-ttu-id="ff589-167">Mer information om hur du arbetar med dessa sparade mallar, [Kom igång med privata mallar på Azure portal](../marketplace-consumer/mytemplates-getstarted.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-167">For more information about working with these saved templates, [Get started with private templates on the Azure portal](../marketplace-consumer/mytemplates-getstarted.md).</span></span>

1. <span data-ttu-id="ff589-168">Om du vill hitta dina sparade mallar, Välj **Bläddra** > **mallar**.</span><span class="sxs-lookup"><span data-stu-id="ff589-168">To find your saved templates, select **Browse** > **Templates**.</span></span>
   
    ![Bläddra efter mallar](./media/resource-group-template-deploy-portal/browse-templates.png)
2. <span data-ttu-id="ff589-170">Välj det du vill arbeta med i listan över mallar som sparas i ditt konto.</span><span class="sxs-lookup"><span data-stu-id="ff589-170">From the list of templates saved to your account, select the one you wish to work on.</span></span>
   
    ![sparade mallar](./media/resource-group-template-deploy-portal/saved-templates.png)
3. <span data-ttu-id="ff589-172">Välj **distribuera** distribuera sparade mallen.</span><span class="sxs-lookup"><span data-stu-id="ff589-172">Select **Deploy** to redeploy this saved template.</span></span>
   
    ![distribuera sparade mallen](./media/resource-group-template-deploy-portal/deploy-saved-template.png)

## <a name="next-steps"></a><span data-ttu-id="ff589-174">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff589-174">Next steps</span></span>
* <span data-ttu-id="ff589-175">Om du vill visa granskningsloggarna finns [granskningsåtgärder med Resource Manager](resource-group-audit.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-175">To view audit logs, see [Audit operations with Resource Manager](resource-group-audit.md).</span></span>
* <span data-ttu-id="ff589-176">Om du vill felsöka distribution, se [visa distributionsåtgärder](resource-manager-deployment-operations.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-176">To troubleshoot deployment errors, see [View deployment operations](resource-manager-deployment-operations.md).</span></span>
* <span data-ttu-id="ff589-177">Om du vill hämta en mall från en distribution eller resursgruppen finns [exportera Azure Resource Manager-mall från befintliga resurser](resource-manager-export-template.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-177">To retrieve a template from a deployment or resource group, see [Export Azure Resource Manager template from existing resources](resource-manager-export-template.md).</span></span>
* <span data-ttu-id="ff589-178">Vägledning för hur företag kan använda resurshanteraren för att effektivt hantera prenumerationer finns i [Azure enterprise scaffold - förebyggande prenumerationsåtgärder](resource-manager-subscription-governance.md).</span><span class="sxs-lookup"><span data-stu-id="ff589-178">For guidance on how enterprises can use Resource Manager to effectively manage subscriptions, see [Azure enterprise scaffold - prescriptive subscription governance](resource-manager-subscription-governance.md).</span></span>

