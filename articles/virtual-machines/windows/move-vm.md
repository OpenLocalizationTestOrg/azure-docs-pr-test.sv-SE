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
# <a name="move-a-windows-vm-tooanother-azure-subscription-or-resource-group"></a>Flytta en virtuell Windows-dator tooanother Azure-prenumeration eller resursgrupp
Den här artikeln guidar dig igenom hur toomove en Windows-VM mellan resursgrupper eller prenumerationer. Flytta mellan prenumerationer kan vara användbar om du ursprungligen har skapat en virtuell dator i en personlig prenumeration och nu vill toomove den tooyour företags prenumeration toocontinue ditt arbete.

> [!IMPORTANT]
>Du kan inte flytta hanterade diskar just nu. 
>
>Ny resurs-ID: N skapas som en del av hello move. När hello VM har flyttats, måste du tooupdate din verktyg och skript toouse hello ny resurs-ID. 
> 
> 

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="use-powershell-toomove-a-vm"></a>Använd Powershell toomove en virtuell dator
toomove en virtuell dator tooanother resursgrupp måste toomake att du även flytta alla hello beroende resurser. toouse hello flytta AzureRMResource cmdlet, behöver du hello resursnamnet och hello typ av resurs. Du kan få båda från hello hitta AzureRMResource cmdlet.

    Find-AzureRMResource -ResourceGroupNameContains "<sourceResourceGroupName>"


toomove en virtuell dator måste toomove flera resurser. Vi kan bara skapa separat för varje resurs och visa dem. Det här exemplet innehåller de flesta av grundläggande hello-resurserna för en virtuell dator, men du kan lägga till fler efter behov.

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

toomove Hej resurser toodifferent prenumeration, innehåller hello **- DestinationSubscriptionId** parameter. 

    Move-AzureRmResource -DestinationSubscriptionId "<destinationSubscriptionID>" -DestinationResourceGroupName $destinationRG -ResourceId $vm.ResourceId, $storageAccount.ResourceId, $diagStorageAccount.ResourceId, $vNet.ResourceId, $nic.ResourceId, $ip.ResourceId, $nsg.ResourceId



Du blir ombedd tooconfirm som du vill toomove hello anges resurser. Typen **Y** tooconfirm som du vill toomove hello resurser.

## <a name="next-steps"></a>Nästa steg
Du kan flytta många olika typer av resurser mellan resursgrupper och prenumerationer. Mer information finns i [flytta resurser toonew resursgrupp eller prenumeration](../../resource-group-move-resources.md).    

