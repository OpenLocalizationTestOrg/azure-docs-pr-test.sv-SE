---
title: "aaaManage virtuella datorer i en virtuell dator Skaluppsättning | Microsoft Docs"
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
ms.openlocfilehash: 7d848729c0fc708bd596b61feb528cf4bf4bafd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-virtual-machines-in-a-virtual-machine-scale-set"></a><span data-ttu-id="0a6cb-103">Hantera virtuella datorer i en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="0a6cb-103">Manage virtual machines in a virtual machine scale set</span></span>
<span data-ttu-id="0a6cb-104">Använd hello aktiviteter i denna artikel toomanage virtuella datorer i din skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-104">Use hello tasks in this article toomanage virtual machines in your virtual machine scale set.</span></span>

<span data-ttu-id="0a6cb-105">De flesta av hello aktiviteter som rör hantering av en virtuell dator i en skaluppsättning kräver att du vet hello instans-ID för hello datorn som du vill toomanage.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-105">Most of hello tasks that involve managing a virtual machine in a scale set require that you know hello instance ID of hello machine that you want toomanage.</span></span> <span data-ttu-id="0a6cb-106">Du kan använda [resursutforskaren Azure](https://resources.azure.com) toofind hello instans-ID för en virtuell dator i en skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-106">You can use [Azure Resource Explorer](https://resources.azure.com) toofind hello instance ID of a virtual machine in a scale set.</span></span> <span data-ttu-id="0a6cb-107">Du kan också använda Resursläsaren tooverify hello status för hello uppgifter som du är klar.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-107">You also use Resource Explorer tooverify hello status of hello tasks that you finish.</span></span>

<span data-ttu-id="0a6cb-108">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview) information om installation hello senaste versionen av Azure PowerShell, välja din prenumeration och loggar in tooyour konto.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-108">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview) for information about installing hello latest version of Azure PowerShell, selecting your subscription, and signing in tooyour account.</span></span>

## <a name="display-information-about-a-scale-set"></a><span data-ttu-id="0a6cb-109">Visa information om en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="0a6cb-109">Display information about a scale set</span></span>
<span data-ttu-id="0a6cb-110">Du kan hämta allmän information om en skalningsuppsättning som också är refererad tooas hello-instansvyn.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-110">You can get general information about a scale set, which is also referred tooas hello instance view.</span></span> <span data-ttu-id="0a6cb-111">Eller så kan du få mer specifik information, till exempel information om hello resurser i hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-111">Or, you can get more specific information, such as information about hello resources in hello scale set.</span></span>

<span data-ttu-id="0a6cb-112">Ersätt hello citattecken värden med hello namn eller din resursgrupp och skala ange och kör hello-kommando:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-112">Replace hello quoted values with hello name or your resource group and scale set and then run hello command:</span></span>

    Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"

<span data-ttu-id="0a6cb-113">Den returnerar ungefär så här:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-113">It returns something like this:</span></span>

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

<span data-ttu-id="0a6cb-114">Ersätt hello citattecken värden med hello namnet på din resursuppsättningen för gruppen och skala.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-114">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="0a6cb-115">Ersätt  *#*  med hello instans-ID hello virtuella dator du vill ha tooget information om och sedan köra den:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-115">Replace *#* with hello instance identifier of hello virtual machine that you want tooget information about and then run it:</span></span>

    Get-AzureRmVmssVM -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="0a6cb-116">Den returnerar något som liknar det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-116">It returns something like this example:</span></span>

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

## <a name="start-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="0a6cb-117">Starta en virtuell dator i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="0a6cb-117">Start a virtual machine in a scale set</span></span>
<span data-ttu-id="0a6cb-118">Ersätt hello citattecken värden med hello namnet på din resursuppsättningen för gruppen och skala.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-118">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="0a6cb-119">Ersätt  *#*  med hello virtuella dator du vill toostart och sedan köra den hello-ID:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-119">Replace *#* with hello identifier of hello virtual machine that you want toostart and then run it:</span></span>

    Start-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="0a6cb-120">I Resursläsaren, kan vi se att hello status för hello-instansen är **kör**:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-120">In Resource Explorer, we can see that hello status of hello instance is **running**:</span></span>

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

<span data-ttu-id="0a6cb-121">Du kan starta alla hello virtuella datorer i hello skaluppsättningen genom att inte använda hello - InstanceId-parameter.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-121">You can start all hello virtual machines in hello scale set by not using hello -InstanceId parameter.</span></span>

## <a name="stop-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="0a6cb-122">Stoppa en virtuell dator i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="0a6cb-122">Stop a virtual machine in a scale set</span></span>
<span data-ttu-id="0a6cb-123">Ersätt hello citattecken värden med hello namnet på din resursuppsättningen för gruppen och skala.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-123">Replace hello quoted values with hello name of your resource group and scale set.</span></span> <span data-ttu-id="0a6cb-124">Ersätt  *#*  med hello virtuella dator du vill toostop och sedan köra den hello-ID:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-124">Replace *#* with hello identifier of hello virtual machine that you want toostop and then run it:</span></span>

    Stop-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="0a6cb-125">I Resursläsaren, kan vi se att hello status för hello-instansen är **frigjorts**:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-125">In Resource Explorer, we can see that hello status of hello instance is **deallocated**:</span></span>

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

<span data-ttu-id="0a6cb-126">toostop en virtuell dator och frigör den inte kan använda hello StayProvisioned - parametern.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-126">toostop a virtual machine and not deallocate it, use hello -StayProvisioned parameter.</span></span> <span data-ttu-id="0a6cb-127">Du kan stoppa alla hello virtuella datorer i hello som inte använder hello InstanceId - parameter.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-127">You can stop all hello virtual machines in hello set by not using hello -InstanceId parameter.</span></span>

## <a name="restart-a-virtual-machine-in-a-scale-set"></a><span data-ttu-id="0a6cb-128">Starta om en virtuell dator i en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="0a6cb-128">Restart a virtual machine in a scale set</span></span>
<span data-ttu-id="0a6cb-129">Ersätt hello citattecken värden med hello namnet på resursen grupp och hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-129">Replace hello quoted values with hello name of your resource group and hello scale set.</span></span> <span data-ttu-id="0a6cb-130">Ersätt  *#*  med hello virtuella dator du vill toorestart och sedan köra den hello-ID:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-130">Replace *#* with hello identifier of hello virtual machine that you want toorestart and then run it:</span></span>

    Restart-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="0a6cb-131">Du kan starta om alla hello virtuella datorer i hello som inte använder hello InstanceId - parameter.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-131">You can restart all hello virtual machines in hello set by not using hello -InstanceId parameter.</span></span>

## <a name="remove-a-virtual-machine-from-a-scale-set"></a><span data-ttu-id="0a6cb-132">Ta bort en virtuell dator från en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="0a6cb-132">Remove a virtual machine from a scale set</span></span>
<span data-ttu-id="0a6cb-133">Ersätt hello citattecken värden med hello namnet på resursen grupp och hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-133">Replace hello quoted values with hello name of your resource group and hello scale set.</span></span> <span data-ttu-id="0a6cb-134">Ersätt  *#*  med hello virtuella dator du vill tooremove och sedan köra den hello-ID:</span><span class="sxs-lookup"><span data-stu-id="0a6cb-134">Replace *#* with hello identifier of hello virtual machine that you want tooremove and then run it:</span></span>  

    Remove-AzureRmVmss -ResourceGroupName "resource group name" –VMScaleSetName "scale set name" -InstanceId #

<span data-ttu-id="0a6cb-135">Du kan ta bort hello virtuella datorns skaluppsättning samtidigt genom att inte använda hello - InstanceId-parameter.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-135">You can remove hello virtual machine scale set all at once by not using hello -InstanceId parameter.</span></span>

## <a name="change-hello-capacity-of-a-scale-set"></a><span data-ttu-id="0a6cb-136">Ändra hello kapacitet för en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="0a6cb-136">Change hello capacity of a scale set</span></span>
<span data-ttu-id="0a6cb-137">Du kan lägga till eller ta bort virtuella datorer genom att ändra hello kapacitet för hello mängd.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-137">You can add or remove virtual machines by changing hello capacity of hello set.</span></span> <span data-ttu-id="0a6cb-138">Hämta skaluppsättning för hello som du vill toochange set hello kapacitet toowhat du vill toobe och sedan uppdatera hello skaluppsättning med hello ny kapacitet.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-138">Get hello scale set that you want toochange, set hello capacity toowhat you want it toobe, and then update hello scale set with hello new capacity.</span></span> <span data-ttu-id="0a6cb-139">I de här kommandona Ersätt hello citattecken värden med hello namnet på resursen grupp och hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-139">In these commands, replace hello quoted values with hello name of your resource group and hello scale set.</span></span>

    $vmss = Get-AzureRmVmss -ResourceGroupName "resource group name" -VMScaleSetName "scale set name"
    $vmss.sku.capacity = 5
    Update-AzureRmVmss -ResourceGroupName "resource group name" -Name "scale set name" -VirtualMachineScaleSet $vmss 

<span data-ttu-id="0a6cb-140">Om du tar bort virtuella datorer från hello skaluppsättning tas hello virtuella datorer med hello högsta-ID: n bort först.</span><span class="sxs-lookup"><span data-stu-id="0a6cb-140">If you are removing virtual machines from hello scale set, hello virtual machines with hello highest ids are removed first.</span></span>

