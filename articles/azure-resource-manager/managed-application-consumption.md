---
title: "Använda en Azure hanterat program | Microsoft Docs"
description: "Beskriver hur en kund skapar en Azure hanterade program från publicerade filer."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 08/23/2017
ms.author: gauravbh; tomfitz
ms.openlocfilehash: ed8fbaf2a4546c8e31eeced11cd0b5627fd62c0c
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="consume-an-internal-managed-application"></a><span data-ttu-id="e0da2-103">Använda en intern hanterad App</span><span class="sxs-lookup"><span data-stu-id="e0da2-103">Consume an internal managed application</span></span>

<span data-ttu-id="e0da2-104">Du kan använda Azure [hanterade program](managed-application-overview.md) som är avsedda för medlemmar i din organisation.</span><span class="sxs-lookup"><span data-stu-id="e0da2-104">You can consume Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="e0da2-105">Du kan till exempel välja hanterade program från IT-avdelningen som säkerställa efterlevnad med organisationens normer.</span><span class="sxs-lookup"><span data-stu-id="e0da2-105">For example, you can select managed applications from your IT department that ensure compliance with organizational standards.</span></span> <span data-ttu-id="e0da2-106">Dessa hanterade program är tillgängliga via Tjänstkatalogen inte Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="e0da2-106">These managed applications are available through the Service Catalog, not the Azure Marketplace.</span></span>

<span data-ttu-id="e0da2-107">Innan du fortsätter med den här artikeln, måste du ha ett hanterat program som är tillgängliga i tjänstkatalogen för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="e0da2-107">Before proceeding with this article, you must have a managed application available in the service catalog for your subscription.</span></span> <span data-ttu-id="e0da2-108">Om någon i din organisation redan inte har skapat ett hanterat program, se [publicera ett hanterat program för internt bruk](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="e0da2-108">If someone in your organization has not already created a managed application, see [Publish a managed application for internal consumption](managed-application-publishing.md).</span></span>

<span data-ttu-id="e0da2-109">För närvarande kan kan du använda Azure CLI eller Azure-portalen för att använda ett hanterat program.</span><span class="sxs-lookup"><span data-stu-id="e0da2-109">Currently, you can use either Azure CLI or the Azure portal to consume a managed application.</span></span>

## <a name="create-the-managed-application-by-using-the-portal"></a><span data-ttu-id="e0da2-110">Skapa det hanterade programmet med hjälp av portalen</span><span class="sxs-lookup"><span data-stu-id="e0da2-110">Create the managed application by using the portal</span></span>

<span data-ttu-id="e0da2-111">Följ anvisningarna nedan om du vill distribuera ett hanterat program via portalen</span><span class="sxs-lookup"><span data-stu-id="e0da2-111">To deploy a managed application through the portal, follow these steps:</span></span>

1. <span data-ttu-id="e0da2-112">Gå till Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-112">Go to the Azure portal.</span></span> <span data-ttu-id="e0da2-113">Sök efter **Tjänstkatalogen hanterat program**.</span><span class="sxs-lookup"><span data-stu-id="e0da2-113">Search for **Service Catalog Managed Application**.</span></span>

   ![Tjänstkatalogen hanterade program](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. <span data-ttu-id="e0da2-115">Välj det hanterade programmet som du vill skapa i listan över tillgängliga lösningar.</span><span class="sxs-lookup"><span data-stu-id="e0da2-115">Select the managed application you want to create from the list of available solutions.</span></span> <span data-ttu-id="e0da2-116">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="e0da2-116">Select **Create**.</span></span>

   ![Val av hanterade program](./media/managed-application-consumption/select-offer.png)

1. <span data-ttu-id="e0da2-118">Ange värden för parametrarna som krävs för att etablera resurserna.</span><span class="sxs-lookup"><span data-stu-id="e0da2-118">Provide values for the parameters that are required to provision the resources.</span></span> <span data-ttu-id="e0da2-119">Välj **Väst centrala oss** för platsen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-119">Select **West Central US** for location.</span></span> <span data-ttu-id="e0da2-120">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="e0da2-120">Select **OK**.</span></span>

   ![Parametrar för hanterade program](./media/managed-application-consumption/input-parameters.png)

1. <span data-ttu-id="e0da2-122">Mallen verifierar de värden du angav.</span><span class="sxs-lookup"><span data-stu-id="e0da2-122">The template validates the values you provided.</span></span> <span data-ttu-id="e0da2-123">Om verifieringen lyckas, väljer **OK** att starta distributionen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-123">If validation succeeds, select **OK** to start the deployment.</span></span>

   ![Validering av hanterade program](./media/managed-application-consumption/validation.png)

<span data-ttu-id="e0da2-125">När distributionen är klar etableras lämpliga resurser som definierats i mallen i den hantera resursgruppen som du angav.</span><span class="sxs-lookup"><span data-stu-id="e0da2-125">After the deployment finishes, the appropriate resources defined in the template are provisioned in the managed resource group you provided.</span></span>

## <a name="create-the-managed-application-by-using-azure-cli"></a><span data-ttu-id="e0da2-126">Skapa det hanterade programmet med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e0da2-126">Create the managed application by using Azure CLI</span></span>

<span data-ttu-id="e0da2-127">Det finns två sätt att skapa ett hanterat program med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="e0da2-127">There are two ways to create a managed application by using Azure CLI:</span></span>

* <span data-ttu-id="e0da2-128">Använd kommandot för att skapa hanterade program.</span><span class="sxs-lookup"><span data-stu-id="e0da2-128">Use the command for creating managed applications.</span></span>
* <span data-ttu-id="e0da2-129">Använd kommandot reguljära mallen distribution.</span><span class="sxs-lookup"><span data-stu-id="e0da2-129">Use the regular template deployment command.</span></span>

### <a name="use-the-template-deployment-command"></a><span data-ttu-id="e0da2-130">Använd mallkommandot för distribution</span><span class="sxs-lookup"><span data-stu-id="e0da2-130">Use the template deployment command</span></span>

<span data-ttu-id="e0da2-131">Distribuera filen applianceMainTemplate.json som skapats av leverantören.</span><span class="sxs-lookup"><span data-stu-id="e0da2-131">Deploy the applianceMainTemplate.json file that the vendor created.</span></span>

<span data-ttu-id="e0da2-132">Skapa sedan två resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="e0da2-132">Then create two resource groups.</span></span> <span data-ttu-id="e0da2-133">Den första resursgruppen finns där programresursen hanterade har skapats: Microsoft.Solutions/appliances.</span><span class="sxs-lookup"><span data-stu-id="e0da2-133">The first resource group is where the managed application resource is created: Microsoft.Solutions/appliances.</span></span> <span data-ttu-id="e0da2-134">Andra resursgruppen innehåller alla resurser som definierats i mainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="e0da2-134">The second resource group contains all the resources defined in mainTemplate.json.</span></span> <span data-ttu-id="e0da2-135">Den här resursgruppen hanteras av ISV: er.</span><span class="sxs-lookup"><span data-stu-id="e0da2-135">This resource group is managed by the ISV.</span></span>

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="e0da2-136">Använd `westcentralus` som plats för resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-136">Use `westcentralus` as the location of the resource group.</span></span>
>

<span data-ttu-id="e0da2-137">Om du vill distribuera applianceMainTemplate.json i mainResourceGroup, använder du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="e0da2-137">To deploy applianceMainTemplate.json in mainResourceGroup, use the following command:</span></span>

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

<span data-ttu-id="e0da2-138">När mallen föregående körs, ombeds du ange värden för parametrar som definieras i mallen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-138">After the preceding template runs, it prompts you for the values of the parameters that are defined in the template.</span></span> <span data-ttu-id="e0da2-139">Utöver de parametrar som behövs för att etablera resurser i en mall, behöver du två viktiga parametervärden:</span><span class="sxs-lookup"><span data-stu-id="e0da2-139">In addition to the parameters that are needed to provision resources in a template, you need two key parameter values:</span></span>

- <span data-ttu-id="e0da2-140">**managedResourceGroupId**: ID för resursgruppen som innehåller de resurser som definierats i applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="e0da2-140">**managedResourceGroupId**: The ID of the resource group containing the resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="e0da2-141">ID som är i formatet `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span><span class="sxs-lookup"><span data-stu-id="e0da2-141">The ID is of the form `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span></span> <span data-ttu-id="e0da2-142">I föregående exempel är det ID för `managedResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="e0da2-142">In the preceding example, it's the ID of `managedResourceGroup`.</span></span>
- <span data-ttu-id="e0da2-143">**applianceDefinitionId**: ID för den hanterade programtjänsten definition resursen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-143">**applianceDefinitionId**: The ID of the managed application definition resource.</span></span> <span data-ttu-id="e0da2-144">Det här värdet anges av ISV: er.</span><span class="sxs-lookup"><span data-stu-id="e0da2-144">This value is provided by the ISV.</span></span>

> [!NOTE]
> <span data-ttu-id="e0da2-145">Utgivaren måste ge åtkomst till resursgruppen som innehåller programdefinitionen av hanterade.</span><span class="sxs-lookup"><span data-stu-id="e0da2-145">The publisher must grant access to the resource group that contains the managed application definition.</span></span> <span data-ttu-id="e0da2-146">Definition av resursen har skapats i publisher prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-146">The definition resource is created in the publisher subscription.</span></span> <span data-ttu-id="e0da2-147">Därför måste en användare, grupp eller ett program i innehavaren för customer läsbehörighet till den här resursen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-147">Therefore, a user, user group, or application in the customer tenant needs read access to this resource.</span></span>

<span data-ttu-id="e0da2-148">När distributionen har slutförts, visas det hanterade programmet skapas i mainResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="e0da2-148">After the deployment finishes successfully, you see the managed application is created in mainResourceGroup.</span></span> <span data-ttu-id="e0da2-149">StorageAccount resursen har skapats i managedResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="e0da2-149">The storageAccount resource is created in managedResourceGroup.</span></span>

### <a name="use-the-create-command"></a><span data-ttu-id="e0da2-150">Använd kommandot create</span><span class="sxs-lookup"><span data-stu-id="e0da2-150">Use the create command</span></span>

<span data-ttu-id="e0da2-151">Du kan använda den `az managedapp create` kommando för att skapa ett hanterat program från definition för hanterade program.</span><span class="sxs-lookup"><span data-stu-id="e0da2-151">You can use the `az managedapp create` command to create a managed application from the managed application definition.</span></span>

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* <span data-ttu-id="e0da2-152">**installation-definition-Id**: resurs-ID för hanterade programdefinitionen skapades i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e0da2-152">**appliance-definition-Id**: The resource ID of the managed application definition created in the preceding step.</span></span> <span data-ttu-id="e0da2-153">Kör följande kommando för att få detta ID:</span><span class="sxs-lookup"><span data-stu-id="e0da2-153">To obtain this ID, run the following command:</span></span>

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  <span data-ttu-id="e0da2-154">Det här kommandot returnerar definition för hanterade program.</span><span class="sxs-lookup"><span data-stu-id="e0da2-154">This command returns the managed application definition.</span></span> <span data-ttu-id="e0da2-155">Du måste värdet för egenskapen ID.</span><span class="sxs-lookup"><span data-stu-id="e0da2-155">You need the value of the ID property.</span></span>

* <span data-ttu-id="e0da2-156">**hanterade-rg-id**: namnet på resursgruppen som innehåller de resurser som definierats i applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="e0da2-156">**managed-rg-id**: The name of the resource group containing the resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="e0da2-157">Den här resursgruppen tillhör hanterade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-157">This resource group is the managed resource group.</span></span> <span data-ttu-id="e0da2-158">Den hanteras av utgivaren.</span><span class="sxs-lookup"><span data-stu-id="e0da2-158">It's managed by the publisher.</span></span> <span data-ttu-id="e0da2-159">Om det inte finns, skapas den åt dig.</span><span class="sxs-lookup"><span data-stu-id="e0da2-159">If it doesn't exist, it's created for you.</span></span>
* <span data-ttu-id="e0da2-160">**resursgruppens namn**: resursgruppen där programresursen hanterade har skapats.</span><span class="sxs-lookup"><span data-stu-id="e0da2-160">**resource-group**: The resource group where the managed application resource is created.</span></span> <span data-ttu-id="e0da2-161">Resursen Microsoft.Solutions/appliance bor i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-161">The Microsoft.Solutions/appliance resource lives in this resource group.</span></span>
* <span data-ttu-id="e0da2-162">**parametrarna**: de parametrar som krävs för de resurser som definierats i applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="e0da2-162">**parameters**: The parameters that are needed for the resources defined in applianceMainTemplate.json.</span></span>

## <a name="known-issues"></a><span data-ttu-id="e0da2-163">Kända problem</span><span class="sxs-lookup"><span data-stu-id="e0da2-163">Known issues</span></span>

<span data-ttu-id="e0da2-164">Den här förhandsversionen omfattar följande:</span><span class="sxs-lookup"><span data-stu-id="e0da2-164">This preview release includes the following issues:</span></span>

* <span data-ttu-id="e0da2-165">En 500 Internt serverfel visas under genereringen av det hanterade programmet.</span><span class="sxs-lookup"><span data-stu-id="e0da2-165">A 500 internal server error appears during the creation of the managed application.</span></span> <span data-ttu-id="e0da2-166">Om du stöter på problemet, är det troligt att tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="e0da2-166">If you run into this issue, it's likely to be intermittent.</span></span> <span data-ttu-id="e0da2-167">Försök igen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-167">Retry the operation.</span></span>
* <span data-ttu-id="e0da2-168">En ny resursgrupp krävs för den hantera resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-168">A new resource group is needed for the managed resource group.</span></span> <span data-ttu-id="e0da2-169">Om du använder en befintlig resursgrupp, misslyckas distributionen.</span><span class="sxs-lookup"><span data-stu-id="e0da2-169">If you use an existing resource group, the deployment fails.</span></span>
* <span data-ttu-id="e0da2-170">Resursgruppen som innehåller Microsoft.Solutions/appliances resursen måste skapas i den **westcentralus** plats.</span><span class="sxs-lookup"><span data-stu-id="e0da2-170">The resource group that contains the Microsoft.Solutions/appliances resource must be created in the **westcentralus** location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e0da2-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e0da2-171">Next steps</span></span>

* <span data-ttu-id="e0da2-172">En introduktion till hanterade program, se [hanteras Programöversikt](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e0da2-172">For an introduction to managed applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="e0da2-173">Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="e0da2-173">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="e0da2-174">Information om hur du publicerar hanterade program på Azure Marketplace finns [Azure hanterade program i Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="e0da2-174">For information about publishing managed applications to the Azure Marketplace, see [Azure managed applications in the Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="e0da2-175">Information om hur du använder ett hanterat program från Marketplace finns [använda Azure hanterade program i Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="e0da2-175">For information about consuming a managed application from the Marketplace, see [Consume Azure managed applications in the Marketplace](managed-application-consume-marketplace.md).</span></span>
