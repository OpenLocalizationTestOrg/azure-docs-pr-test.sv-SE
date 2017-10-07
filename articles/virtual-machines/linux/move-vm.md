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
# <a name="move-a-linux-vm-tooanother-subscription-or-resource-group"></a><span data-ttu-id="c65be-103">Flytta en Linux VM tooanother prenumerationen eller resursen grupp</span><span class="sxs-lookup"><span data-stu-id="c65be-103">Move a Linux VM tooanother subscription or resource group</span></span>
<span data-ttu-id="c65be-104">Den här artikeln guidar dig igenom hur toomove en Linux VM mellan resursgrupper eller prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c65be-104">This article walks you through how toomove a Linux VM between resource groups or subscriptions.</span></span> <span data-ttu-id="c65be-105">Flytta en virtuell dator mellan prenumerationer kan vara användbar om du har skapat en virtuell dator i en personlig prenumeration och nu vill toomove den tooyour företagsprenumeration.</span><span class="sxs-lookup"><span data-stu-id="c65be-105">Moving a VM between subscriptions can be handy if you created a VM in a personal subscription and now want toomove it tooyour company's subscription.</span></span>

> [!IMPORTANT]
><span data-ttu-id="c65be-106">Du kan inte flytta hanterade diskar just nu.</span><span class="sxs-lookup"><span data-stu-id="c65be-106">You cannot move Managed Disks at this time.</span></span> 
>
><span data-ttu-id="c65be-107">Ny resurs-ID: N skapas som en del av hello move.</span><span class="sxs-lookup"><span data-stu-id="c65be-107">New resource IDs are created as part of hello move.</span></span> <span data-ttu-id="c65be-108">När hello VM har flyttats, måste du tooupdate din verktyg och skript toouse hello ny resurs-ID.</span><span class="sxs-lookup"><span data-stu-id="c65be-108">Once hello VM has been moved, you need tooupdate your tools and scripts toouse hello new resource IDs.</span></span> 
> 
> 

## <a name="use-hello-azure-cli-toomove-a-vm"></a><span data-ttu-id="c65be-109">Använd hello Azure CLI toomove en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="c65be-109">Use hello Azure CLI toomove a VM</span></span>
<span data-ttu-id="c65be-110">toosuccessfully flytta en virtuell dator måste du toomove hello VM och alla dess stödfiler resurser.</span><span class="sxs-lookup"><span data-stu-id="c65be-110">toosuccessfully move a VM, you need toomove hello VM and all its supporting resources.</span></span> <span data-ttu-id="c65be-111">Använd hello **azure-grupp visa** kommandot toolist alla hello resurser i en resursgrupp och deras ID.</span><span class="sxs-lookup"><span data-stu-id="c65be-111">Use hello **azure group show** command toolist all hello resources in a resource group and their IDs.</span></span> <span data-ttu-id="c65be-112">Det hjälper toopipe hello utdata från kommandot tooa filen så att du kan kopiera och klistra in hello ID: N i senare kommandon.</span><span class="sxs-lookup"><span data-stu-id="c65be-112">It helps toopipe hello output of this command tooa file so you can copy and paste hello IDs into later commands.</span></span>

    azure group show <resourceGroupName>

<span data-ttu-id="c65be-113">toomove en virtuell dator och dess resurser tooanother resursgrupp, använda hello **azure-resurs flytta** CLI-kommando.</span><span class="sxs-lookup"><span data-stu-id="c65be-113">toomove a VM and its resources tooanother resource group, use hello **azure resource move** CLI command.</span></span> <span data-ttu-id="c65be-114">hello som följande exempel visar hur toomove en virtuell dator och hello vanligaste resurser den kräver.</span><span class="sxs-lookup"><span data-stu-id="c65be-114">hello following example shows how toomove a VM and hello most common resources it requires.</span></span> <span data-ttu-id="c65be-115">Vi använder hello **-i** parameter- och pass i en kommaavgränsad lista (utan blanksteg) med ID: N för hello resurser toomove.</span><span class="sxs-lookup"><span data-stu-id="c65be-115">We use hello **-i** parameter and pass in a comma-separated list (without spaces) of IDs for hello resources toomove.</span></span>

    vm=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Compute/virtualMachines/<vmName>
    nic=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkInterfaces/<nicName>
    nsg=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/networkSecurityGroups/<nsgName>
    pip=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/publicIPAddresses/<publicIPName>
    vnet=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Network/virtualNetworks/<vnetName>
    diag=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<diagnosticStorageAccountName>
    storage=/subscriptions/<sourceSubscriptionID>/resourceGroups/<sourceResourceGroup>/providers/Microsoft.Storage/storageAccounts/<storageAcountName>      

    azure resource move --ids $vm,$nic,$nsg,$pip,$vnet,$storage,$diag -d "<destinationResourceGroup>"

<span data-ttu-id="c65be-116">Om du vill toomove hello virtuella datorn och dess resurser tooa annan prenumeration, lägga till hello **--mål subscriptionId &#60; destinationSubscriptionID &#62;** parametern toospecify hello målprenumerationen.</span><span class="sxs-lookup"><span data-stu-id="c65be-116">If you want toomove hello VM and its resources tooa different subscription, add hello **--destination-subscriptionId &#60;destinationSubscriptionID&#62;** parameter toospecify hello destination subscription.</span></span>

<span data-ttu-id="c65be-117">Om du arbetar från hello Kommandotolken på en Windows-dator, måste du tooadd en  **$**  framför hello variabelnamn när du deklarerar dem.</span><span class="sxs-lookup"><span data-stu-id="c65be-117">If you are working from hello Command Prompt on a Windows computer, you need tooadd a **$** in front of hello variable names when you declare them.</span></span> <span data-ttu-id="c65be-118">Detta är inte behövs i Linux.</span><span class="sxs-lookup"><span data-stu-id="c65be-118">This isn't needed in Linux.</span></span>

<span data-ttu-id="c65be-119">Du uppmanas tooconfirm som du vill toomove hello angivna resursen.</span><span class="sxs-lookup"><span data-stu-id="c65be-119">You are asked tooconfirm that you want toomove hello specified resource.</span></span> <span data-ttu-id="c65be-120">Typen **Y** tooconfirm som du vill toomove hello resurser.</span><span class="sxs-lookup"><span data-stu-id="c65be-120">Type **Y** tooconfirm that you want toomove hello resources.</span></span>

[!INCLUDE [virtual-machines-common-move-vm](../../../includes/virtual-machines-common-move-vm.md)]

## <a name="next-steps"></a><span data-ttu-id="c65be-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c65be-121">Next steps</span></span>
<span data-ttu-id="c65be-122">Du kan flytta många olika typer av resurser mellan resursgrupper och prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c65be-122">You can move many different types of resources between resource groups and subscriptions.</span></span> <span data-ttu-id="c65be-123">Mer information finns i [flytta resurser toonew resursgrupp eller prenumeration](../../resource-group-move-resources.md).</span><span class="sxs-lookup"><span data-stu-id="c65be-123">For more information, see [Move resources toonew resource group or subscription](../../resource-group-move-resources.md).</span></span>    

