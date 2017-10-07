---
title: aaaMove en Linux VM i Azure | Microsoft Docs
description: Flytta en Linux VM tooanother Azure-prenumeration eller resursgrupp i hello Resource Manager-distributionsmodellen.
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: d635f0a5-4458-4b95-a5f8-eed4f41eb4d4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 938d04234059111912f03e72d14dabd338bc0678
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="move-a-linux-vm-tooanother-subscription-or-resource-group"></a>Flytta en Linux VM tooanother prenumerationen eller resursen grupp
Den här artikeln guidar dig igenom hur toomove en Linux VM mellan resursgrupper eller prenumerationer. Flytta en virtuell dator mellan prenumerationer kan vara användbar om du har skapat en virtuell dator i en personlig prenumeration och nu vill toomove den tooyour företagsprenumeration.

> [!IMPORTANT]
>Du kan inte flytta hanterade diskar just nu. 
>
>Ny resurs-ID: N skapas som en del av hello move. När hello VM har flyttats, måste du tooupdate din verktyg och skript toouse hello ny resurs-ID. 
> 
> 

## <a name="use-hello-azure-cli-toomove-a-vm"></a>Använd hello Azure CLI toomove en virtuell dator
toosuccessfully flytta en virtuell dator måste du toomove hello VM och alla dess stödfiler resurser. Använd hello **azure-grupp visa** kommandot toolist alla hello resurser i en resursgrupp och deras ID. Det hjälper toopipe hello utdata från kommandot tooa filen så att du kan kopiera och klistra in hello ID: N i senare kommandon.

    azure group show <resourceGroupName>

toomove en virtuell dator och dess resurser tooanother resursgrupp, använda hello **azure-resurs flytta** CLI-kommando. hello som följande exempel visar hur toomove en virtuell dator och hello vanligaste resurser den kräver. Vi använder hello **-i** parameter- och pass i en kommaavgränsad lista (utan blanksteg) med ID: N för hello resurser toomove.

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

Om du vill toomove hello virtuella datorn och dess resurser tooa annan prenumeration, lägga till hello **--mål subscriptionId &#60; destinationSubscriptionID &#62;** parametern toospecify hello målprenumerationen.

Om du arbetar från hello Kommandotolken på en Windows-dator, måste du tooadd en  **$**  framför hello variabelnamn när du deklarerar dem. Detta är inte behövs i Linux.

Du uppmanas tooconfirm som du vill toomove hello angivna resursen. Typen **Y** tooconfirm som du vill toomove hello resurser.

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a>Nästa steg
Du kan flytta många olika typer av resurser mellan resursgrupper och prenumerationer. Mer information finns i [flytta resurser toonew resursgrupp eller prenumeration](../../resource-group-move-resources.md).    

