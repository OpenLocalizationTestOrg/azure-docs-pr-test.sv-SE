---
title: aaaMove Windows VM resurs i Azure | Microsoft Docs
description: Flytta en virtuell Windows-dator tooanother Azure-prenumeration eller resursgrupp i hello Resource Manager-distributionsmodellen.
services: virtual-machines-windows
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 4e383427-4aff-4bf3-a0f4-dbff5c6f0c81
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 859e78dce9acf1168780d4ee8e9f6dac0e3c11cf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-windows-vm-tooanother-azure-subscription-or-resource-group"></a><span data-ttu-id="2c347-103">Flytta en virtuell Windows-dator tooanother Azure-prenumeration eller resursgrupp</span><span class="sxs-lookup"><span data-stu-id="2c347-103">Move a Windows VM tooanother Azure subscription or resource group</span></span>
<span data-ttu-id="2c347-104">Den här artikeln guidar dig igenom hur toomove en Windows-VM mellan resursgrupper eller prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2c347-104">This article walks you through how toomove a Windows VM between resource groups or subscriptions.</span></span> <span data-ttu-id="2c347-105">Flytta mellan prenumerationer kan vara användbar om du ursprungligen har skapat en virtuell dator i en personlig prenumeration och nu vill toomove den tooyour företags prenumeration toocontinue ditt arbete.</span><span class="sxs-lookup"><span data-stu-id="2c347-105">Moving between subscriptions can be handy if you originally created a VM in a personal subscription and now want toomove it tooyour company's subscription toocontinue your work.</span></span>

> [!IMPORTANT]
><span data-ttu-id="2c347-106">Du kan inte flytta hanterade diskar just nu.</span><span class="sxs-lookup"><span data-stu-id="2c347-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="2c347-107">Ny resurs-ID: N skapas som en del av hello move.</span><span class="sxs-lookup"><span data-stu-id="2c347-107">New resource IDs are created as part of hello move.</span></span> <span data-ttu-id="2c347-108">När hello VM har flyttats, måste du tooupdate din verktyg och skript toouse hello ny resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="2c347-108">Once hello VM has been moved, you need tooupdate your tools and scripts toouse hello new resource IDs.</span></span> 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-toomove-a-vm"></a><span data-ttu-id="2c347-109">Använd Powershell toomove en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="2c347-109">Use Powershell toomove a VM</span></span>
<span data-ttu-id="2c347-110">toomove en virtuell dator tooanother resursgrupp måste toomake att du även flytta alla hello beroende resurser.</span><span class="sxs-lookup"><span data-stu-id="2c347-110">toomove a virtual machine tooanother resource group, you need toomake sure that you also move all of hello dependent resources.</span></span> <span data-ttu-id="2c347-111">toouse hello flytta AzureRMResource cmdlet, behöver du hello resursnamnet och hello typ av resurs.</span><span class="sxs-lookup"><span data-stu-id="2c347-111">toouse hello Move-AzureRMResource cmdlet, you need hello resource name and hello type of resource.</span></span> <span data-ttu-id="2c347-112">Du kan få båda från hello hitta AzureRMResource cmdlet.</span><span class="sxs-lookup"><span data-stu-id="2c347-112">You can get both from hello Find-AzureRMResource cmdlet.</span></span>

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


<span data-ttu-id="2c347-113">toomove en virtuell dator måste toomove flera resurser.</span><span class="sxs-lookup"><span data-stu-id="2c347-113">toomove a VM we need toomove multiple resources.</span></span> <span data-ttu-id="2c347-114">Vi kan bara skapa separat för varje resurs och visa dem.</span><span class="sxs-lookup"><span data-stu-id="2c347-114">We can just create separate variables for each resource and then list them.</span></span> <span data-ttu-id="2c347-115">Det här exemplet innehåller de flesta av grundläggande hello-resurserna för en virtuell dator, men du kan lägga till fler efter behov.</span><span class="sxs-lookup"><span data-stu-id="2c347-115">This example includes most of hello basic resources for a VM, but you can add more as needed.</span></span>

    $sourceRG = "<sourceResourceGroupName>"
    $destinationRG = "<destinationResourceGroupName>"

    $vm = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Compute/virtualMachines" -ResourceName "<vmName>"
    $storageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<storageAccountName>"
    $diagStorageAccount = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Storage/storageAccounts" -ResourceName "<diagnosticStorageAccountName>"
    $vNet = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/virtualNetworks" -ResourceName "<vNetName>"
    $nic = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkInterfaces" -ResourceName "<nicName>"
    $ip = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/publicIPAddresses" -ResourceName "<ipName>"
    $nsg = Get-AzureRmResource -ResourceGroupName $sourceRG -ResourceType "Microsoft.Network/networkSecurityGroups" -ResourceName "<nsgName>"

    Move-AzureRmResource -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId

<span data-ttu-id="2c347-116">toomove Hej resurser toodifferent prenumeration, innehåller hello **- DestinationSubscriptionId** parameter.</span><span class="sxs-lookup"><span data-stu-id="2c347-116">toomove hello resources toodifferent subscription, include hello **-DestinationSubscriptionId** parameter.</span></span> 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



<span data-ttu-id="2c347-117">Du blir ombedd tooconfirm som du vill toomove hello anges resurser.</span><span class="sxs-lookup"><span data-stu-id="2c347-117">You will be asked tooconfirm that you want toomove hello specified resources.</span></span> <span data-ttu-id="2c347-118">Typen **Y** tooconfirm som du vill toomove hello resurser.</span><span class="sxs-lookup"><span data-stu-id="2c347-118">Type **Y** tooconfirm that you want toomove hello resources.</span></span>

## <a name="next-steps"></a><span data-ttu-id="2c347-119">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2c347-119">Next steps</span></span>
<span data-ttu-id="2c347-120">Du kan flytta många olika typer av resurser mellan resursgrupper och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="2c347-120">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="2c347-121">Mer information finns i [flytta resurser toonew resursgrupp eller prenumeration](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="2c347-121">For more information, see [Move resources toonew resource group or subscription](../../resource-group-move-resources.md).</span></span>    

