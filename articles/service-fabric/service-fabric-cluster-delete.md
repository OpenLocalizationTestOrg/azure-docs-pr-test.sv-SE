---
title: aaaDelete en Azure-kluster och dess resurser | Microsoft Docs
description: "Lär dig hur toocompletely ta bort ett Service Fabric-kluster antingen ta bort hello resursgruppen som innehåller hello klustret eller selektivt ta bort resurser."
services: service-fabric
documentationcenter: .net
author: ChackDan
manager: timlt
editor: 
ms.assetid: de422950-2d22-4ddb-ac47-dd663a946a7e
ms.service: service-fabric
ms.devlang: dotnet
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/24/2017
ms.author: chackdan
ms.openlocfilehash: 5c15a4184644da715cd69397f2150de86ab433ba
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="delete-a-service-fabric-cluster-on-azure-and-hello-resources-it-uses"></a><span data-ttu-id="79909-103">Ta bort ett Service Fabric-kluster på Azure och hello resurser som används</span><span class="sxs-lookup"><span data-stu-id="79909-103">Delete a Service Fabric cluster on Azure and hello resources it uses</span></span>
<span data-ttu-id="79909-104">Service Fabric-klustret består av många andra Azure-resurser förutom toohello klusterresurs sig själv.</span><span class="sxs-lookup"><span data-stu-id="79909-104">A Service Fabric cluster is made up of many other Azure resources in addition toohello cluster resource itself.</span></span> <span data-ttu-id="79909-105">Så toocompletely ta bort Service Fabric-kluster behöver du också toodelete alla hello av resurser.</span><span class="sxs-lookup"><span data-stu-id="79909-105">So toocompletely delete a Service Fabric cluster you also need toodelete all hello resources it is made of.</span></span>
<span data-ttu-id="79909-106">Har du två alternativ: antingen ta bort hello resursgruppen som hello klustret finns i (som tar bort hello klusterresurs och andra resurser i resursgruppen hello) eller uttryckligen ta bort hello klusterresurs och dess tillhörande resurser (men inte andra resurser i resursgruppen hello).</span><span class="sxs-lookup"><span data-stu-id="79909-106">You have two options: Either delete hello resource group that hello cluster is in (which deletes hello cluster resource and any other resources in hello resource group) or specifically delete hello cluster resource and it's associated resources (but not other resources in hello resource group).</span></span>

> [!NOTE]
> <span data-ttu-id="79909-107">Om du tar bort hello klusterresurs **inte** ta bort alla hello andra resurser som består av Service Fabric-klustret.</span><span class="sxs-lookup"><span data-stu-id="79909-107">Deleting hello cluster resource **does not** delete all hello other resources that your Service Fabric cluster is composed of.</span></span>
> 
> 

## <a name="delete-hello-entire-resource-group-rg-that-hello-service-fabric-cluster-is-in"></a><span data-ttu-id="79909-108">Ta bort hello hela resursgruppen (RG) som hello Service Fabric-klustret är i</span><span class="sxs-lookup"><span data-stu-id="79909-108">Delete hello entire resource group (RG) that hello Service Fabric cluster is in</span></span>
<span data-ttu-id="79909-109">Detta är hello enklaste sättet tooensure du ta bort alla hello resurser som är associerade med klustret, inklusive hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="79909-109">This is hello easiest way tooensure that you delete all hello resources associated with your cluster, including hello resource group.</span></span> <span data-ttu-id="79909-110">Du kan ta bort hello resursgrupp med hjälp av PowerShell eller via hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="79909-110">You can delete hello resource group using PowerShell or through hello Azure portal.</span></span> <span data-ttu-id="79909-111">Om resursgruppen har resurser som inte är relaterade tooService fabric-kluster, kan du ta bort specifika resurser.</span><span class="sxs-lookup"><span data-stu-id="79909-111">If your resource group has resources that are not related tooService fabric cluster, then you can delete specific resources.</span></span>

### <a name="delete-hello-resource-group-using-azure-powershell"></a><span data-ttu-id="79909-112">Ta bort hello resursgruppen med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="79909-112">Delete hello resource group using Azure PowerShell</span></span>
<span data-ttu-id="79909-113">Du kan också ta bort hello resursgruppen genom att köra hello följande Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="79909-113">You can also delete hello resource group by running hello following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="79909-114">Kontrollera att Azure PowerShell 1.0 eller högre är installerad på datorn.</span><span class="sxs-lookup"><span data-stu-id="79909-114">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="79909-115">Om du inte har gjort det innan du följer hello steg som beskrivs i [hur tooinstall och konfigurera Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="79909-115">If you have not done this before, follow hello steps outlined in [How tooinstall and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="79909-116">Öppna ett PowerShell-fönster och kör hello följande PS-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="79909-116">Open a PowerShell window and run hello following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount

Remove-AzureRmResourceGroup -Name <name of ResouceGroup> -Force
```

<span data-ttu-id="79909-117">Du får en fråga tooconfirm hello borttagning om du inte använde hello *-Force* alternativet.</span><span class="sxs-lookup"><span data-stu-id="79909-117">You will get a prompt tooconfirm hello deletion if you did not use hello *-Force* option.</span></span> <span data-ttu-id="79909-118">Hej RG vid bekräftelse och alla hello-resurser som den innehåller tas bort.</span><span class="sxs-lookup"><span data-stu-id="79909-118">On confirmation hello RG and all hello resources it contains are deleted.</span></span>

### <a name="delete-a-resource-group-in-hello-azure-portal"></a><span data-ttu-id="79909-119">Ta bort en resursgrupp i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="79909-119">Delete a resource group in hello Azure portal</span></span>
1. <span data-ttu-id="79909-120">Inloggningen toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79909-120">Login toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="79909-121">Navigera toohello Service Fabric-kluster som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="79909-121">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
3. <span data-ttu-id="79909-122">Klicka på hello resursgruppens namn på hello klustret essentials sida.</span><span class="sxs-lookup"><span data-stu-id="79909-122">Click on hello Resource Group name on hello cluster essentials page.</span></span>
4. <span data-ttu-id="79909-123">Detta öppnar hello **resurs grupp Essentials** sidan.</span><span class="sxs-lookup"><span data-stu-id="79909-123">This brings up hello **Resource Group Essentials** page.</span></span>
5. <span data-ttu-id="79909-124">Klicka på **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="79909-124">Click **Delete**.</span></span>
6. <span data-ttu-id="79909-125">Följ instruktionerna för hello på sidan toocomplete hello borttagningen av hello resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="79909-125">Follow hello instructions on that page toocomplete hello deletion of hello resource group.</span></span>

![Ta bort resursgruppen.][ResourceGroupDelete]

## <a name="delete-hello-cluster-resource-and-hello-resources-it-uses-but-not-other-resources-in-hello-resource-group"></a><span data-ttu-id="79909-127">Ta bort hello klusterresurs och hello resurser används, men inte andra resurser i hello resursgrupp</span><span class="sxs-lookup"><span data-stu-id="79909-127">Delete hello cluster resource and hello resources it uses, but not other resources in hello resource group</span></span>
<span data-ttu-id="79909-128">Om resursgruppen har resurser som är relaterade toohello Service Fabric-kluster du vill toodelete och det är enklare toodelete hello hela resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="79909-128">If your resource group has only resources that are related toohello Service Fabric cluster you want toodelete, then it is easier toodelete hello entire resource group.</span></span> <span data-ttu-id="79909-129">Följ dessa steg om du vill tooselectively delete hello resurser i taget i resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="79909-129">If you want tooselectively delete hello resources one-by-one in your resource group, then follow these steps.</span></span>

<span data-ttu-id="79909-130">Om du har distribuerat ditt kluster med hjälp av hello-portalen eller något av hello Service Fabric Resource Manager-mallar från hello mallgalleriet märks alla hello-resurser som hello klustret använder med hello följande två taggar.</span><span class="sxs-lookup"><span data-stu-id="79909-130">If you deployed your cluster using hello portal or using one of hello Service Fabric Resource Manager templates from hello template gallery, then all hello resources that hello cluster uses are tagged with hello following two tags.</span></span> <span data-ttu-id="79909-131">Du kan använda dem toodecide vilka resurser du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="79909-131">You can use them toodecide which resources you want toodelete.</span></span>

<span data-ttu-id="79909-132">***Taggen nr 1:*** nyckel = clusterName, värde = 'namn på hello klustret'</span><span class="sxs-lookup"><span data-stu-id="79909-132">***Tag#1:*** Key = clusterName, Value = 'name of hello cluster'</span></span>

<span data-ttu-id="79909-133">***Taggen nr 2:*** nyckel = resourceName, värde = ServiceFabric</span><span class="sxs-lookup"><span data-stu-id="79909-133">***Tag#2:*** Key = resourceName, Value = ServiceFabric</span></span>

### <a name="delete-specific-resources-in-hello-azure-portal"></a><span data-ttu-id="79909-134">Ta bort specifika resurser i hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="79909-134">Delete specific resources in hello Azure portal</span></span>
1. <span data-ttu-id="79909-135">Inloggningen toohello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="79909-135">Login toohello [Azure portal](https://portal.azure.com).</span></span>
2. <span data-ttu-id="79909-136">Navigera toohello Service Fabric-kluster som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="79909-136">Navigate toohello Service Fabric cluster you want toodelete.</span></span>
3. <span data-ttu-id="79909-137">Gå för**alla inställningar** på hello essentials bladet.</span><span class="sxs-lookup"><span data-stu-id="79909-137">Go too**All settings** on hello essentials blade.</span></span>
4. <span data-ttu-id="79909-138">Klicka på **taggar** under **resurshantering** hello inställningar-bladet.</span><span class="sxs-lookup"><span data-stu-id="79909-138">Click on **Tags** under **Resource Management** in hello settings blade.</span></span>
5. <span data-ttu-id="79909-139">Klicka på någon av hello **taggar** i hello taggar bladet tooget en lista över alla hello resurser med taggen.</span><span class="sxs-lookup"><span data-stu-id="79909-139">Click on one of hello **Tags** in hello tags blade tooget a list of all hello resources with that tag.</span></span>
   
    ![Resurstaggar][ResourceTags]
6. <span data-ttu-id="79909-141">När du har hello lista med märkta resurser klickar du på var och en av hello resurser och ta bort dem.</span><span class="sxs-lookup"><span data-stu-id="79909-141">Once you have hello list of tagged resources, click on each of hello resources and delete them.</span></span>
   
    ![Taggade resurser][TaggedResources]

### <a name="delete-hello-resources-using-azure-powershell"></a><span data-ttu-id="79909-143">Ta bort hello resurser med hjälp av Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="79909-143">Delete hello resources using Azure PowerShell</span></span>
<span data-ttu-id="79909-144">Du kan ta bort hello resurser i taget genom att köra hello följande Azure PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="79909-144">You can delete hello resources one-by-one by running hello following Azure PowerShell cmdlets.</span></span> <span data-ttu-id="79909-145">Kontrollera att Azure PowerShell 1.0 eller högre är installerad på datorn.</span><span class="sxs-lookup"><span data-stu-id="79909-145">Make sure Azure PowerShell 1.0 or greater is installed on your computer.</span></span> <span data-ttu-id="79909-146">Om du inte har gjort det innan du följer hello steg som beskrivs i [hur tooinstall och konfigurera Azure PowerShell.](/powershell/azure/overview)</span><span class="sxs-lookup"><span data-stu-id="79909-146">If you have not done this before, follow hello steps outlined in [How tooinstall and Configure Azure PowerShell.](/powershell/azure/overview)</span></span>

<span data-ttu-id="79909-147">Öppna ett PowerShell-fönster och kör hello följande PS-cmdlets:</span><span class="sxs-lookup"><span data-stu-id="79909-147">Open a PowerShell window and run hello following PS cmdlets:</span></span>

```powershell
Login-AzureRmAccount
```
<span data-ttu-id="79909-148">För varje hello resurser du vill toodelete, kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="79909-148">For each of hello resources you want toodelete, run hello following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "<Resource Type>" -ResourceGroupName "<name of hello resource group>" -Force
```

<span data-ttu-id="79909-149">toodelete hello-klusterresursen kör hello följande:</span><span class="sxs-lookup"><span data-stu-id="79909-149">toodelete hello cluster resource, run hello following:</span></span>

```powershell
Remove-AzureRmResource -ResourceName "<name of hello Resource>" -ResourceType "Microsoft.ServiceFabric/clusters" -ResourceGroupName "<name of hello resource group>" -Force
```

## <a name="next-steps"></a><span data-ttu-id="79909-150">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79909-150">Next steps</span></span>
<span data-ttu-id="79909-151">Läs hello följande tooalso Lär dig om att uppgradera ett kluster och partitionering tjänster:</span><span class="sxs-lookup"><span data-stu-id="79909-151">Read hello following tooalso learn about upgrading a cluster and partitioning services:</span></span>

* [<span data-ttu-id="79909-152">Lär dig mer om klusteruppgradering</span><span class="sxs-lookup"><span data-stu-id="79909-152">Learn about cluster upgrades</span></span>](service-fabric-cluster-upgrade.md)
* [<span data-ttu-id="79909-153">Lär dig mer om partitionering tillståndskänsliga tjänster för maximal skala</span><span class="sxs-lookup"><span data-stu-id="79909-153">Learn about partitioning stateful services for maximum scale</span></span>](service-fabric-concepts-partitioning.md)

<!--Image references-->
[ResourceGroupDelete]: ./media/service-fabric-cluster-delete/ResourceGroupDelete.PNG

[ResourceTags]: ./media/service-fabric-cluster-delete/ResourceTags.png

[TaggedResources]: ./media/service-fabric-cluster-delete/TaggedResources.PNG
