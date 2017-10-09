---
title: aaaResize en virtuell Windows-dator i hello klassiska distributionsmodellen - Azure | Microsoft Docs
description: "Ändra storlek på en Windows-dator som skapats i hello klassiska distributionsmodellen med hjälp av Azure Powershell."
services: virtual-machines-windows
documentationcenter: 
author: Drewm3
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: e3038215-001c-406e-904d-e0f4e326a4c7
ms.service: virtual-machines-windows
ms.workload: na
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: article
ms.date: 10/19/2016
ms.author: drewm
ms.openlocfilehash: 39fab14431e2351c9515b0611e850eccfec7a798
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="resize-a-windows-vm-created-in-hello-classic-deployment-model"></a><span data-ttu-id="163ad-103">Ändra storlek på en Windows-VM som skapats i hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="163ad-103">Resize a Windows VM created in hello classic deployment model</span></span>
<span data-ttu-id="163ad-104">Den här artikeln visar hur tooresize en Windows-VM skapas i hello klassiska distributionsmodellen med hjälp av Azure Powershell.</span><span class="sxs-lookup"><span data-stu-id="163ad-104">This article shows you how tooresize a Windows VM, created in hello classic deployment model using Azure Powershell.</span></span>

<span data-ttu-id="163ad-105">När du överväger hello möjlighet tooresize en virtuell dator, finns det två principer som styr hello mängd storlekar tillgängliga tooresize hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="163ad-105">When considering hello ability tooresize a VM, there are two concepts which control hello range of sizes available tooresize hello virtual machine.</span></span> <span data-ttu-id="163ad-106">hello första konceptet är hello region där den virtuella datorn distribueras.</span><span class="sxs-lookup"><span data-stu-id="163ad-106">hello first concept is hello region in which your VM is deployed.</span></span> <span data-ttu-id="163ad-107">hello listan över VM-storlekar tillgängligt i region finns under fliken för hello-tjänster på webbsidan för hello Azure-regioner.</span><span class="sxs-lookup"><span data-stu-id="163ad-107">hello list of VM sizes available in region is under hello Services tab of hello Azure Regions web page.</span></span> <span data-ttu-id="163ad-108">hello andra begrepp är hello fysisk maskinvara som för närvarande är värd för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="163ad-108">hello second concept is hello physical hardware currently hosting your VM.</span></span> <span data-ttu-id="163ad-109">hello fysiska servrar som är värd för virtuella datorer grupperas i kluster vanliga fysisk maskinvara.</span><span class="sxs-lookup"><span data-stu-id="163ad-109">hello physical servers hosting VMs are grouped together in clusters of common physical hardware.</span></span> <span data-ttu-id="163ad-110">hello-metoden för att ändra en VM-storlek skiljer sig åt beroende på om hello önskad nya VM-storlek stöds av hello maskinvara klustret för närvarande värd hello VM.</span><span class="sxs-lookup"><span data-stu-id="163ad-110">hello method of changing a VM size differs depending on if hello desired new VM size is supported by hello hardware cluster currently hosting hello VM.</span></span>

> [!IMPORTANT] 
> <span data-ttu-id="163ad-111">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="163ad-111">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../../../resource-manager-deployment-model.md).</span></span> <span data-ttu-id="163ad-112">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="163ad-112">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="163ad-113">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="163ad-113">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span> <span data-ttu-id="163ad-114">Du kan också [ändra storlek på en virtuell dator som skapats i hello Resource Manager-distributionsmodellen](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="163ad-114">You can also [resize a VM created in hello Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

## <a name="add-your-account"></a><span data-ttu-id="163ad-115">Lägg till ditt konto</span><span class="sxs-lookup"><span data-stu-id="163ad-115">Add your account</span></span>
<span data-ttu-id="163ad-116">Du måste konfigurera Azure PowerShell toowork med klassiska Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="163ad-116">You must configure Azure PowerShell toowork with classic Azure resources.</span></span> <span data-ttu-id="163ad-117">Följ hello steg nedan tooconfigure Azure PowerShell toomanage klassiska resurser.</span><span class="sxs-lookup"><span data-stu-id="163ad-117">Follow hello steps below tooconfigure Azure PowerShell toomanage classic resources.</span></span>

1. <span data-ttu-id="163ad-118">I hello PowerShell-Kommandotolken, Skriv `Add-AzureAccount` och på **RETUR**.</span><span class="sxs-lookup"><span data-stu-id="163ad-118">At hello PowerShell prompt, type `Add-AzureAccount` and click **Enter**.</span></span> 
2. <span data-ttu-id="163ad-119">Ange hello e-postadress som är associerad med din Azure-prenumeration och klicka på **Fortsätt**.</span><span class="sxs-lookup"><span data-stu-id="163ad-119">Type in hello email address associated with your Azure subscription and click **Continue**.</span></span> 
3. <span data-ttu-id="163ad-120">Ange hello lösenord för ditt konto.</span><span class="sxs-lookup"><span data-stu-id="163ad-120">Type in hello password for your account.</span></span> 
4. <span data-ttu-id="163ad-121">Klicka på **logga in**.</span><span class="sxs-lookup"><span data-stu-id="163ad-121">Click **Sign in**.</span></span> 

## <a name="resize-in-hello-same-hardware-cluster"></a><span data-ttu-id="163ad-122">Ändra storlek på i hello samma maskinvara kluster</span><span class="sxs-lookup"><span data-stu-id="163ad-122">Resize in hello same hardware cluster</span></span>
<span data-ttu-id="163ad-123">tooresize en VM tooa storlek i hello maskinvara kluster som värd för hello VM, utföra hello följande steg.</span><span class="sxs-lookup"><span data-stu-id="163ad-123">tooresize a VM tooa size available in hello hardware cluster hosting hello VM, perform hello following steps.</span></span>

1. <span data-ttu-id="163ad-124">Kör följande PowerShell-kommando toolist hello VM storlekar som finns tillgängliga i hello maskinvara kluster värdtjänsten hello molnet som innehåller hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="163ad-124">Run hello following PowerShell command toolist hello VM sizes available in hello hardware cluster hosting hello cloud service which contains hello VM.</span></span>
   
    ```powershell
    Get-AzureService | where {$_.ServiceName -eq "<cloudServiceName>"}
    ```
2. <span data-ttu-id="163ad-125">Kör följande kommandon tooresize hello VM hello.</span><span class="sxs-lookup"><span data-stu-id="163ad-125">Run hello following commands tooresize hello VM.</span></span>
   
    ```powershell
    Get-AzureVM -ServiceName <cloudServiceName> -Name <vmName> | Set-AzureVMSize -InstanceSize <newVMSize> | Update-AzureVM
    ```

## <a name="resize-on-a-new-hardware-cluster"></a><span data-ttu-id="163ad-126">Ändra storlek på ett nytt kluster för maskinvara</span><span class="sxs-lookup"><span data-stu-id="163ad-126">Resize on a new hardware cluster</span></span>
<span data-ttu-id="163ad-127">tooresize en VM tooa storleken är inte tillgänglig i hello maskinvara klustret värd Hej VM, hello Molntjänsten och alla virtuella datorer i hello Molntjänsten måste återskapas.</span><span class="sxs-lookup"><span data-stu-id="163ad-127">tooresize a VM tooa size not available in hello hardware cluster hosting hello VM, hello cloud service and all VMs in hello cloud service must be recreated.</span></span> <span data-ttu-id="163ad-128">Varje tjänst i molnet finns på ett kluster med samma maskinvara så att alla virtuella datorer i hello Molntjänsten måste ha en storlek som stöds på ett kluster för maskinvara.</span><span class="sxs-lookup"><span data-stu-id="163ad-128">Each cloud service is hosted on a single hardware cluster so all VMs in hello cloud service must be a size that is supported on a hardware cluster.</span></span> <span data-ttu-id="163ad-129">hello följande steg beskriver hur tooresize en virtuell dator genom att ta bort och sedan återskapa hello cloud service.</span><span class="sxs-lookup"><span data-stu-id="163ad-129">hello following steps will describe how tooresize a VM by deleting and then recreating hello cloud service.</span></span>

1. <span data-ttu-id="163ad-130">Kör följande PowerShell-kommandot toolist hello VM storlekar som finns tillgängliga i hello region hello.</span><span class="sxs-lookup"><span data-stu-id="163ad-130">Run hello following PowerShell command toolist hello VM sizes available in hello region.</span></span> 
   
    ```powershell
    Get-AzureLocation | where {$_.Name -eq "<locationName>"}
    ```
2. <span data-ttu-id="163ad-131">Anteckna alla konfigurationsinställningar för varje virtuell dator i hello molntjänst som innehåller hello VM toobe storlek.</span><span class="sxs-lookup"><span data-stu-id="163ad-131">Make note of all configuration settings for each VM in hello cloud service which contains hello VM toobe resized.</span></span> 
3. <span data-ttu-id="163ad-132">Ta bort alla virtuella datorer i hello Molntjänsten välja hello alternativet tooretain hello diskar för varje virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="163ad-132">Delete all VMs in hello cloud service selecting hello option tooretain hello disks for each VM.</span></span>
4. <span data-ttu-id="163ad-133">Återskapa hello VM toobe storlek med hello önskade VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="163ad-133">Recreate hello VM toobe resized using hello desired VM size.</span></span>
5. <span data-ttu-id="163ad-134">Återskapa alla andra virtuella datorer som fanns i hello Molntjänsten med hjälp av en VM-storlek som är tillgängliga i hello maskinvara kluster är nu värd hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="163ad-134">Recreate all other VMs which were in hello cloud service using a VM size available in hello hardware cluster now hosting hello cloud service.</span></span>

<span data-ttu-id="163ad-135">Ett exempelskript för att ta bort och återskapa en tjänst i molnet med hjälp av en ny VM-storlek kan hittas [här](https://github.com/Azure/azure-vm-scripts).</span><span class="sxs-lookup"><span data-stu-id="163ad-135">A sample script for deleting and recreating a cloud service using a new VM size can be found [here](https://github.com/Azure/azure-vm-scripts).</span></span> 

## <a name="next-steps"></a><span data-ttu-id="163ad-136">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="163ad-136">Next steps</span></span>
* <span data-ttu-id="163ad-137">[Ändra storlek på en virtuell dator som skapats i hello Resource Manager-distributionsmodellen](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="163ad-137">[Resize a VM created in hello Resource Manager deployment model](../resize-vm.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>

