---
title: aaaConsume en Azure hanterat program | Microsoft Docs
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
ms.openlocfilehash: b8510086eb05304c0e351a391b7e0cf34a467568
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="consume-an-internal-managed-application"></a><span data-ttu-id="eaa6f-103">Använda en intern hanterad App</span><span class="sxs-lookup"><span data-stu-id="eaa6f-103">Consume an internal managed application</span></span>

<span data-ttu-id="eaa6f-104">Du kan använda Azure [hanterade program](managed-application-overview.md) som är avsedda för medlemmar i din organisation.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-104">You can consume Azure [managed applications](managed-application-overview.md) that are intended for members of your organization.</span></span> <span data-ttu-id="eaa6f-105">Du kan till exempel välja hanterade program från IT-avdelningen som säkerställa efterlevnad med organisationens normer.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-105">For example, you can select managed applications from your IT department that ensure compliance with organizational standards.</span></span> <span data-ttu-id="eaa6f-106">Dessa hanterade program är tillgängliga via hello Tjänstkatalog, inte hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-106">These managed applications are available through hello Service Catalog, not hello Azure Marketplace.</span></span>

<span data-ttu-id="eaa6f-107">Innan du fortsätter med den här artikeln, måste du ha ett hanterat program som är tillgängliga i hello tjänstkatalogen för din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-107">Before proceeding with this article, you must have a managed application available in hello service catalog for your subscription.</span></span> <span data-ttu-id="eaa6f-108">Om någon i din organisation redan inte har skapat ett hanterat program, se [publicera ett hanterat program för internt bruk](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="eaa6f-108">If someone in your organization has not already created a managed application, see [Publish a managed application for internal consumption](managed-application-publishing.md).</span></span>

<span data-ttu-id="eaa6f-109">För närvarande kan du använda Azure CLI eller hello Azure portal tooconsume hanterade program.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-109">Currently, you can use either Azure CLI or hello Azure portal tooconsume a managed application.</span></span>

## <a name="create-hello-managed-application-by-using-hello-portal"></a><span data-ttu-id="eaa6f-110">Skapa hello hanterade program med hjälp av hello-portalen</span><span class="sxs-lookup"><span data-stu-id="eaa6f-110">Create hello managed application by using hello portal</span></span>

<span data-ttu-id="eaa6f-111">toodeploy hanterade programmet hello-portalen så här:</span><span class="sxs-lookup"><span data-stu-id="eaa6f-111">toodeploy a managed application through hello portal, follow these steps:</span></span>

1. <span data-ttu-id="eaa6f-112">Gå toohello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-112">Go toohello Azure portal.</span></span> <span data-ttu-id="eaa6f-113">Sök efter **Tjänstkatalogen hanterat program**.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-113">Search for **Service Catalog Managed Application**.</span></span>

   ![Tjänstkatalogen hanterade program](./media/managed-application-consumption/create-service-catalog-managed-application.png)

1. <span data-ttu-id="eaa6f-115">Välj hello hanterade program som du vill toocreate hello listan över tillgängliga lösningar.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-115">Select hello managed application you want toocreate from hello list of available solutions.</span></span> <span data-ttu-id="eaa6f-116">Välj **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-116">Select **Create**.</span></span>

   ![Val av hanterade program](./media/managed-application-consumption/select-offer.png)

1. <span data-ttu-id="eaa6f-118">Ange värden för hello-parametrar som är nödvändiga tooprovision hello resurser.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-118">Provide values for hello parameters that are required tooprovision hello resources.</span></span> <span data-ttu-id="eaa6f-119">Välj **Väst centrala oss** för platsen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-119">Select **West Central US** for location.</span></span> <span data-ttu-id="eaa6f-120">Välj **OK**.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-120">Select **OK**.</span></span>

   ![Parametrar för hanterade program](./media/managed-application-consumption/input-parameters.png)

1. <span data-ttu-id="eaa6f-122">hello mallen verifierar hello-värden som du angav.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-122">hello template validates hello values you provided.</span></span> <span data-ttu-id="eaa6f-123">Om verifieringen lyckas, väljer **OK** toostart hello distribution.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-123">If validation succeeds, select **OK** toostart hello deployment.</span></span>

   ![Validering av hanterade program](./media/managed-application-consumption/validation.png)

<span data-ttu-id="eaa6f-125">När hello distributionen är klar etableras hello lämpliga resurser som definierats i mallen för hello i hanterade hello resursgrupp som du angav.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-125">After hello deployment finishes, hello appropriate resources defined in hello template are provisioned in hello managed resource group you provided.</span></span>

## <a name="create-hello-managed-application-by-using-azure-cli"></a><span data-ttu-id="eaa6f-126">Skapa hello hanterade program med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="eaa6f-126">Create hello managed application by using Azure CLI</span></span>

<span data-ttu-id="eaa6f-127">Det finns två sätt toocreate ett hanterat program med hjälp av Azure CLI:</span><span class="sxs-lookup"><span data-stu-id="eaa6f-127">There are two ways toocreate a managed application by using Azure CLI:</span></span>

* <span data-ttu-id="eaa6f-128">Hello kommandot för att skapa hanterade program.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-128">Use hello command for creating managed applications.</span></span>
* <span data-ttu-id="eaa6f-129">Kommandot hello reguljära mallen distribution.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-129">Use hello regular template deployment command.</span></span>

### <a name="use-hello-template-deployment-command"></a><span data-ttu-id="eaa6f-130">Kommandot hello mall för distribution</span><span class="sxs-lookup"><span data-stu-id="eaa6f-130">Use hello template deployment command</span></span>

<span data-ttu-id="eaa6f-131">Distribuera hello applianceMainTemplate.json fil som hello leverantör skapas.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-131">Deploy hello applianceMainTemplate.json file that hello vendor created.</span></span>

<span data-ttu-id="eaa6f-132">Skapa sedan två resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-132">Then create two resource groups.</span></span> <span data-ttu-id="eaa6f-133">hello första resursgruppen är där hello hanterade programresursen har skapats: Microsoft.Solutions/appliances.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-133">hello first resource group is where hello managed application resource is created: Microsoft.Solutions/appliances.</span></span> <span data-ttu-id="eaa6f-134">hello andra resursgruppen innehåller alla hello-resurser som definierats i mainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-134">hello second resource group contains all hello resources defined in mainTemplate.json.</span></span> <span data-ttu-id="eaa6f-135">Den här resursgruppen hanteras av hello ISV.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-135">This resource group is managed by hello ISV.</span></span>

```azurecli
az group create --name mainResourceGroup --location westcentralus
az group create --name managedResourceGroup --location westcentralus
```

> [!NOTE]
> <span data-ttu-id="eaa6f-136">Använd `westcentralus` som hello plats hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-136">Use `westcentralus` as hello location of hello resource group.</span></span>
>

<span data-ttu-id="eaa6f-137">toodeploy applianceMainTemplate.json i mainResourceGroup, Använd hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eaa6f-137">toodeploy applianceMainTemplate.json in mainResourceGroup, use hello following command:</span></span>

```azurecli
az group deployment create --name managedAppDeployment --resourceGroup mainResourceGroup --templateUri
```

<span data-ttu-id="eaa6f-138">Efter hello föregående mall körs, ombeds du ange hello värdena för hello-parametrar som definierats i mallen för hello.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-138">After hello preceding template runs, it prompts you for hello values of hello parameters that are defined in hello template.</span></span> <span data-ttu-id="eaa6f-139">Dessutom toohello parametrar som behövs tooprovision resurser i en mall måste du två viktiga parametervärden:</span><span class="sxs-lookup"><span data-stu-id="eaa6f-139">In addition toohello parameters that are needed tooprovision resources in a template, you need two key parameter values:</span></span>

- <span data-ttu-id="eaa6f-140">**managedResourceGroupId**: hello-ID för hello resurs som innehåller hello gruppresurser definieras i applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-140">**managedResourceGroupId**: hello ID of hello resource group containing hello resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="eaa6f-141">hello-ID är hello formatet `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-141">hello ID is of hello form `/subscriptions/{subscriptionId}/resourceGroups/{resoureGroupName}`.</span></span> <span data-ttu-id="eaa6f-142">I föregående exempel hello, det är hello-ID för `managedResourceGroup`.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-142">In hello preceding example, it's hello ID of `managedResourceGroup`.</span></span>
- <span data-ttu-id="eaa6f-143">**applianceDefinitionId**: hello-ID för hello hanterade definition programresursen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-143">**applianceDefinitionId**: hello ID of hello managed application definition resource.</span></span> <span data-ttu-id="eaa6f-144">Det här värdet anges av hello ISV.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-144">This value is provided by hello ISV.</span></span>

> [!NOTE]
> <span data-ttu-id="eaa6f-145">hello publisher måste ge åtkomst toohello resursgruppen som innehåller definitionen av hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-145">hello publisher must grant access toohello resource group that contains hello managed application definition.</span></span> <span data-ttu-id="eaa6f-146">hello definition resursen skapas i hello publisher prenumeration.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-146">hello definition resource is created in hello publisher subscription.</span></span> <span data-ttu-id="eaa6f-147">Därför måste en användare, grupp eller programmet hello kunden klient läsbehörighet toothis resurs.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-147">Therefore, a user, user group, or application in hello customer tenant needs read access toothis resource.</span></span>

<span data-ttu-id="eaa6f-148">När hello distributionen är klar har du se hello hanterade program skapas i mainResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-148">After hello deployment finishes successfully, you see hello managed application is created in mainResourceGroup.</span></span> <span data-ttu-id="eaa6f-149">Hej storageAccount resursen skapas i managedResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-149">hello storageAccount resource is created in managedResourceGroup.</span></span>

### <a name="use-hello-create-command"></a><span data-ttu-id="eaa6f-150">Använd hello skapa kommando</span><span class="sxs-lookup"><span data-stu-id="eaa6f-150">Use hello create command</span></span>

<span data-ttu-id="eaa6f-151">Du kan använda hello `az managedapp create` kommandot toocreate ett hanterat program från hello hanteras programmets definition.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-151">You can use hello `az managedapp create` command toocreate a managed application from hello managed application definition.</span></span>

```azurecli
az managedapp create --name ravtestappliance401 --location "westcentralus"
    --kind "Servicecatalog" --resource-group "ravApplianceCustRG401"
    --managedapp-definition-id "/subscriptions/{guid}/resourceGroups/ravApplianceDefRG401/providers/Microsoft.Solutions/applianceDefinitions/ravtestAppDef401"
    --managed-rg-id "/subscriptions/{guid}/resourceGroups/ravApplianceCustManagedRG401"
    --parameters "{\"storageAccountName\": {\"value\": \"ravappliancedemostore1\"}}"
    --debug
```

* <span data-ttu-id="eaa6f-152">**installation-definition-Id**: hello resurs-ID för hello hanterade definition för program som skapats i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-152">**appliance-definition-Id**: hello resource ID of hello managed application definition created in hello preceding step.</span></span> <span data-ttu-id="eaa6f-153">tooobtain-ID, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="eaa6f-153">tooobtain this ID, run hello following command:</span></span>

  ```azurecli
  az appliance definition show -n ravtestAppDef1 -g ravApplianceRG2
  ```

  <span data-ttu-id="eaa6f-154">Det här kommandot returnerar definition för hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-154">This command returns hello managed application definition.</span></span> <span data-ttu-id="eaa6f-155">Du måste hello värdet för hello ID-egenskap.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-155">You need hello value of hello ID property.</span></span>

* <span data-ttu-id="eaa6f-156">**hanterade-rg-id**: hello namnet på hello resurs som innehåller hello gruppresurser definieras i applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-156">**managed-rg-id**: hello name of hello resource group containing hello resources defined in applianceMainTemplate.json.</span></span> <span data-ttu-id="eaa6f-157">Den här resursgruppen tillhör hello hanterade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-157">This resource group is hello managed resource group.</span></span> <span data-ttu-id="eaa6f-158">Den hanteras av hello utgivare.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-158">It's managed by hello publisher.</span></span> <span data-ttu-id="eaa6f-159">Om det inte finns, skapas den åt dig.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-159">If it doesn't exist, it's created for you.</span></span>
* <span data-ttu-id="eaa6f-160">**resursgruppens namn**: hello resursgruppen där hello hanteras programresursen har skapats.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-160">**resource-group**: hello resource group where hello managed application resource is created.</span></span> <span data-ttu-id="eaa6f-161">Hej Microsoft.Solutions/appliance resursen finns i den här resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-161">hello Microsoft.Solutions/appliance resource lives in this resource group.</span></span>
* <span data-ttu-id="eaa6f-162">**parametrarna**: hello parametrar som behövs för hello resurser som definierats i applianceMainTemplate.json.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-162">**parameters**: hello parameters that are needed for hello resources defined in applianceMainTemplate.json.</span></span>

## <a name="known-issues"></a><span data-ttu-id="eaa6f-163">Kända problem</span><span class="sxs-lookup"><span data-stu-id="eaa6f-163">Known issues</span></span>

<span data-ttu-id="eaa6f-164">Den här förhandsversionen innehåller hello följande problem:</span><span class="sxs-lookup"><span data-stu-id="eaa6f-164">This preview release includes hello following issues:</span></span>

* <span data-ttu-id="eaa6f-165">En 500 Internt serverfel visas under hello skapandet av hello hanterade program.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-165">A 500 internal server error appears during hello creation of hello managed application.</span></span> <span data-ttu-id="eaa6f-166">Om du stöter på problemet är sannolikt toobe tillfälligt.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-166">If you run into this issue, it's likely toobe intermittent.</span></span> <span data-ttu-id="eaa6f-167">Försök igen om hello.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-167">Retry hello operation.</span></span>
* <span data-ttu-id="eaa6f-168">Du behöver en ny resursgrupp hello hanterade resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-168">A new resource group is needed for hello managed resource group.</span></span> <span data-ttu-id="eaa6f-169">Om du använder en befintlig resursgrupp misslyckas hello distributionen.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-169">If you use an existing resource group, hello deployment fails.</span></span>
* <span data-ttu-id="eaa6f-170">hello resursgruppen som innehåller hello Microsoft.Solutions/appliances resursen måste skapas i hello **westcentralus** plats.</span><span class="sxs-lookup"><span data-stu-id="eaa6f-170">hello resource group that contains hello Microsoft.Solutions/appliances resource must be created in hello **westcentralus** location.</span></span>

## <a name="next-steps"></a><span data-ttu-id="eaa6f-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="eaa6f-171">Next steps</span></span>

* <span data-ttu-id="eaa6f-172">En introduktion toomanaged program, se [hanteras Programöversikt](managed-application-overview.md).</span><span class="sxs-lookup"><span data-stu-id="eaa6f-172">For an introduction toomanaged applications, see [Managed application overview](managed-application-overview.md).</span></span>
* <span data-ttu-id="eaa6f-173">Information om hur du publicerar ett program för Tjänstkatalog hanteras finns [skapa och publicera en applikation för Tjänstkatalog hanteras](managed-application-publishing.md).</span><span class="sxs-lookup"><span data-stu-id="eaa6f-173">For information about publishing a Service Catalog managed application, see [Create and publish a Service Catalog managed application](managed-application-publishing.md).</span></span>
* <span data-ttu-id="eaa6f-174">Information om publishing hanterade program toohello Azure Marketplace finns [Azure hanterade program i hello Marketplace](managed-application-author-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="eaa6f-174">For information about publishing managed applications toohello Azure Marketplace, see [Azure managed applications in hello Marketplace](managed-application-author-marketplace.md).</span></span>
* <span data-ttu-id="eaa6f-175">Information om hur du använder ett hanterat program från hello Marketplace finns [använda Azure hanterade program i hello Marketplace](managed-application-consume-marketplace.md).</span><span class="sxs-lookup"><span data-stu-id="eaa6f-175">For information about consuming a managed application from hello Marketplace, see [Consume Azure managed applications in hello Marketplace](managed-application-consume-marketplace.md).</span></span>
