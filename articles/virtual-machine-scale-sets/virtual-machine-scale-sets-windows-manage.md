---
title: "Hantera virtuella datorer i en Skaluppsättning för virtuell dator | Microsoft Docs"
description: Hantera virtuella datorer i en virtuell dator-skala med Azure PowerShell.
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d35fa77a-de96-4ccd-a332-eb181d1f4273
ms.service: virtual-machine-scale-sets
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/27/2016
ms.author: adegeo
ms.openlocfilehash: d09a020b903e5f43afe03b86c675bcc1eb536cbc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="ba21b-103">Hantera virtuella datorer i en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="ba21b-103">Manage virtual machines in a virtual machine scale set</span></span>
<span data-ttu-id="ba21b-104">Använd uppgifterna i den här artikeln för att hantera virtuella datorer i din skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="ba21b-104">Use the tasks in this article to manage virtual machines in your virtual machine scale set.</span></span>

<span data-ttu-id="ba21b-105">De flesta av de uppgifter som rör hantering av en virtuell dator i en skaluppsättning kräver att du vet instans-ID för den dator som du vill hantera.</span><span class="sxs-lookup"><span data-stu-id="ba21b-105">Most of the tasks that involve managing a virtual machine in a scale set require that you know the instance ID of the machine that you want to manage.</span></span> <span data-ttu-id="ba21b-106">Du kan använda [resursutforskaren Azure](https://resources.azure.com) att hitta instans-ID för en virtuell dator i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ba21b-106">You can use [Azure Resource Explorer](https://resources.azure.com) to find the instance ID of a virtual machine in a scale set.</span></span> <span data-ttu-id="ba21b-107">Du kan också använda Resursläsaren för att kontrollera statusen för de aktiviteter som du är klar.</span><span class="sxs-lookup"><span data-stu-id="ba21b-107">You also use Resource Explorer to verify the status of the tasks that you finish.</span></span>

<span data-ttu-id="ba21b-108">Se [Installera och konfigurera Azure PowerShell](/powershell/azure/overview) för information om hur du installerar den senaste versionen av Azure PowerShell, väljer din prenumeration och loggar in på ditt konto.</span><span class="sxs-lookup"><span data-stu-id="ba21b-108">See [How to install and configure Azure PowerShell](/powershell/azure/overview) for information about installing the latest version of Azure PowerShell, selecting your subscription, and signing in to your account.</span></span>

## <a name="display-information-about-a-scale-set"></a><span data-ttu-id="ba21b-109">Visa information om en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="ba21b-109">Display information about a scale set</span></span>
<span data-ttu-id="ba21b-110">Du kan hämta allmän information om en skalningsuppsättning som också kallas instansvyn.</span><span class="sxs-lookup"><span data-stu-id="ba21b-110">You can get general information about a scale set, which is also referred to as the instance view.</span></span> <span data-ttu-id="ba21b-111">Eller så kan du få mer specifik information, till exempel information om resurser i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ba21b-111">Or, you can get more specific information, such as information about the resources in the scale set.</span></span>

<span data-ttu-id="ba21b-112">Ersätt angiven värdena med namnet eller resursgrupp och skala ange och sedan köra kommandot:</span><span class="sxs-lookup"><span data-stu-id="ba21b-112">Replace the quoted values with the name or your resource group and scale set and then run the command:</span></span>

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

<span data-ttu-id="ba21b-113">Den returnerar ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="ba21b-113">It returns something like this:</span></span>

    Id                                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/myvmss1
    Name                                        : myvmss1
    Type                                        : Microsoft.Compute/virtualMachineScaleSets
    Location                                    : centralus
    Sku                                         :
      Name                                      : Standard_A0
      Tier                                      : Standard
      Capacity                                  : 3
    UpgradePolicy                               :
      Mode                                      : Manual
    VirtualMachineProfile                       :
      OsProfile                                 :
        ComputerNamePrefix                      : vmss1
        AdminUsername                           : admin1
        WindowsConfiguration                    :
          ProvisionVMAgent                      : True
          EnableAutomaticUpdates                : True
    StorageProfile                              :
      ImageReference                            :
        Publisher                               : MicrosoftWindowsServer
        Offer                                   : WindowsServer
        Sku                                     : 2012-R2-Datacenter
        Version                                 : latest
      OsDisk                                    :
        Name                                    : vmssosdisk
        Caching                                 : ReadOnly
        CreateOption                            : FromImage
        VhdContainers[0]                        : https://astore.blob.core.windows.net/vmss
        VhdContainers[1]                        : https://gstore.blob.core.windows.net/vmss
        VhdContainers[2]                        : https://mstore.blob.core.windows.net/vmss
        VhdContainers[3]                        : https://sstore.blob.core.windows.net/vmss
        VhdContainers[4]                        : https://ystore.blob.core.windows.net/vmss
    NetworkProfile                              :
      NetworkInterfaceConfigurations[0]         :
        Name                                    : mync1
        Primary                                 : True
        IpConfigurations[0]                     :
          Name                                  : ip1
          Subnet                                :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/virtualNetworks/myvn1/subnets/mysn1
          LoadBalancerBackendAddressPools[0]    :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/backendAddressPools/bepool1
        LoadBalancerInboundNatPools[0]          :
            Id                                  : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Network/loadBalancers/mylb1/inboundNatPools/natpool1
    ExtensionProfile                            :
      Extensions[0]                             :
        Name                                    : Microsoft.Insights.VMDiagnosticsSettings
        Publisher                               : Microsoft.Azure.Diagnostics
        Type                                    : IaaSDiagnostics
        TypeHandlerVersion                      : 1.5
        AutoUpgradeMinorVersion                 : True
        Settings                                : {"xmlCfg":"...","storageAccount":"astore"}
    ProvisioningState                           : Succeeded

<span data-ttu-id="ba21b-114">Ersätt angiven värdena med namnet på din resursuppsättningen för gruppen och skala.</span><span class="sxs-lookup"><span data-stu-id="ba21b-114">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="ba21b-115">Ersätt  *#*  med den virtuella dator som du vill ha information om och sedan köra den instans-ID:</span><span class="sxs-lookup"><span data-stu-id="ba21b-115">Replace *#* with the instance identifier of the virtual machine that you want to get information about and then run it:</span></span>

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="ba21b-116">Den returnerar något som liknar det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="ba21b-116">It returns something like this example:</span></span>

    Id                            : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/
                                    virtualMachineScaleSets/myvmss1/virtualMachines/0
    Name                          : myvmss1_0
    Type                          : Microsoft.Compute/virtualMachineScaleSets/virtualMachines
    Location                      : centralus
    InstanceId                    : 0
    Sku                           :
      Name                        : Standard_A0
      Tier                        : Standard
    LatestModelApplied            : True
    StorageProfile                :
      ImageReference              :
        Publisher                 : MicrosoftWindowsServer
        Offer                     : WindowsServer
        Sku                       : 2012-R2-Datacenter
        Version                   : 4.0.20160617
      OsDisk                      :
        OsType                    : Windows
        Name                      : vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8
        Vhd                       :
          Uri                     : https://astore.blob.core.windows.net/vmss/vmssosdisk-os-0-e11cad52959b4b76a8d9f26c5190c4f8.vhd
        Caching                   : ReadOnly
        CreateOption              : FromImage
    OsProfile                     :
      ComputerName                : myvmss1-0
      AdminUsername               : admin1
      WindowsConfiguration        :
        ProvisionVMAgent          : True
        EnableAutomaticUpdates    : True
    NetworkProfile                :
      NetworkInterfaces[0]        :
        Id                        : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachineScaleSets/
                                    myvmss1/virtualMachines/0/networkInterfaces/mync1
    ProvisioningState             : Succeeded
    Resources[0]                  :
      Id                          : /subscriptions/{sub-id}/resourceGroups/myrg1/providers/Microsoft.Compute/virtualMachines/
                                    myvmss1_0/extensions/Microsoft.Insights.VMDiagnosticsSettings
      Name                        : Microsoft.Insights.VMDiagnosticsSettings
      Type                        : Microsoft.Compute/virtualMachines/extensions
      Location                    : centralus
      Publisher                   : Microsoft.Azure.Diagnostics
      VirtualMachineExtensionType : IaaSDiagnostics
      TypeHandlerVersion          : 1.5
      AutoUpgradeMinorVersion     : True
      Settings                    : {"xmlCfg":"...","storageAccount":"astore"}
      ProvisioningState           : Succeeded

## <a name="start-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="ba21b-117">Starta en virtuell dator i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="ba21b-117">Start a virtual machine in a scale set</span></span>
<span data-ttu-id="ba21b-118">Ersätt angiven värdena med namnet på din resursuppsättningen för gruppen och skala.</span><span class="sxs-lookup"><span data-stu-id="ba21b-118">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="ba21b-119">Ersätt  *#*  med identifieraren för den virtuella datorn som du vill starta och köra den:</span><span class="sxs-lookup"><span data-stu-id="ba21b-119">Replace *#* with the identifier of the virtual machine that you want to start and then run it:</span></span>

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="ba21b-120">I Resursläsaren, kan vi se att status för instansen är **kör**:</span><span class="sxs-lookup"><span data-stu-id="ba21b-120">In Resource Explorer, we can see that the status of the instance is **running**:</span></span>

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T02:10:08.0730839+00:00"
      },
      {
        "code": "PowerState/running",
        "level": "Info",
        "displayStatus": "VM running"
      }
    ]

<span data-ttu-id="ba21b-121">Du kan starta alla virtuella datorer i skaluppsättningen genom att inte använda - InstanceId-parameter.</span><span class="sxs-lookup"><span data-stu-id="ba21b-121">You can start all the virtual machines in the scale set by not using the -InstanceId parameter.</span></span>

## <a name="stop-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="ba21b-122">Stoppa en virtuell dator i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="ba21b-122">Stop a virtual machine in a scale set</span></span>
<span data-ttu-id="ba21b-123">Ersätt angiven värdena med namnet på din resursuppsättningen för gruppen och skala.</span><span class="sxs-lookup"><span data-stu-id="ba21b-123">Replace the quoted values with the name of your resource group and scale set.</span></span> <span data-ttu-id="ba21b-124">Ersätt  *#*  med identifieraren för den virtuella datorn som du vill stoppa och sedan köra den:</span><span class="sxs-lookup"><span data-stu-id="ba21b-124">Replace *#* with the identifier of the virtual machine that you want to stop and then run it:</span></span>

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="ba21b-125">I Resursläsaren, kan vi se att status för instansen är **frigjorts**:</span><span class="sxs-lookup"><span data-stu-id="ba21b-125">In Resource Explorer, we can see that the status of the instance is **deallocated**:</span></span>

    "statuses": [
      {
        "code": "ProvisioningState/succeeded",
        "level": "Info",
        "displayStatus": "Provisioning succeeded",
        "time": "2016-03-15T01:25:17.8792929+00:00"
      },
      {
        "code": "PowerState/deallocated",
        "level": "Info",
        "displayStatus": "VM deallocated"
      }
    ]

<span data-ttu-id="ba21b-126">Använd parametern - StayProvisioned om du vill stoppa en virtuell dator och inte frigör den.</span><span class="sxs-lookup"><span data-stu-id="ba21b-126">To stop a virtual machine and not deallocate it, use the -StayProvisioned parameter.</span></span> <span data-ttu-id="ba21b-127">Du kan stoppa alla virtuella datorer i uppsättningen med hjälp av parametern - InstanceId inte.</span><span class="sxs-lookup"><span data-stu-id="ba21b-127">You can stop all the virtual machines in the set by not using the -InstanceId parameter.</span></span>

## <a name="restart-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="ba21b-128">Starta om en virtuell dator i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="ba21b-128">Restart a virtual machine in a scale set</span></span>
<span data-ttu-id="ba21b-129">Ersätt värdet inom citattecken med namnet på resursgruppen och skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ba21b-129">Replace the quoted values with the name of your resource group and the scale set.</span></span> <span data-ttu-id="ba21b-130">Ersätt  *#*  med identifieraren för den virtuella datorn som du vill starta om och kör det:</span><span class="sxs-lookup"><span data-stu-id="ba21b-130">Replace *#* with the identifier of the virtual machine that you want to restart and then run it:</span></span>

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="ba21b-131">Du kan starta om alla virtuella datorer i uppsättningen genom att inte använda - InstanceId-parameter.</span><span class="sxs-lookup"><span data-stu-id="ba21b-131">You can restart all the virtual machines in the set by not using the -InstanceId parameter.</span></span>

## <a name="remove-a-virtual-machine-from-a-scale-set"></a><span data-ttu-id="ba21b-132">Ta bort en virtuell dator från en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="ba21b-132">Remove a virtual machine from a scale set</span></span>
<span data-ttu-id="ba21b-133">Ersätt värdet inom citattecken med namnet på resursgruppen och skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="ba21b-133">Replace the quoted values with the name of your resource group and the scale set.</span></span> <span data-ttu-id="ba21b-134">Ersätt  *#*  med identifieraren för den virtuella datorn som du vill ta bort och sedan köra den:</span><span class="sxs-lookup"><span data-stu-id="ba21b-134">Replace *#* with the identifier of the virtual machine that you want to remove and then run it:</span></span>  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="ba21b-135">Du kan ta bort virtuella datorns skaluppsättning samtidigt genom att inte använda - InstanceId-parameter.</span><span class="sxs-lookup"><span data-stu-id="ba21b-135">You can remove the virtual machine scale set all at once by not using the -InstanceId parameter.</span></span>

## <a name="change-the-capacity-of-a-scale-set"></a><span data-ttu-id="ba21b-136">Ändra kapaciteten för en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="ba21b-136">Change the capacity of a scale set</span></span>
<span data-ttu-id="ba21b-137">Du kan lägga till eller ta bort virtuella datorer genom att ändra uppsättningen kapacitet.</span><span class="sxs-lookup"><span data-stu-id="ba21b-137">You can add or remove virtual machines by changing the capacity of the set.</span></span> <span data-ttu-id="ba21b-138">Hämta skaluppsättning som du vill ändra, ange kapaciteten som du vill att den ska vara och uppdatera skaluppsättningen med den nya kapaciteten.</span><span class="sxs-lookup"><span data-stu-id="ba21b-138">Get the scale set that you want to change, set the capacity to what you want it to be, and then update the scale set with the new capacity.</span></span> <span data-ttu-id="ba21b-139">I de här kommandona ersätter du noterade värden med namnet på resursgruppen och dess skala.</span><span class="sxs-lookup"><span data-stu-id="ba21b-139">In these commands, replace the quoted values with the name of your resource group and the scale set.</span></span>

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

<span data-ttu-id="ba21b-140">Om du tar bort virtuella datorer från skaluppsättning tas de virtuella datorerna med de högsta ID bort först.</span><span class="sxs-lookup"><span data-stu-id="ba21b-140">If you are removing virtual machines from the scale set, the virtual machines with the highest ids are removed first.</span></span>

