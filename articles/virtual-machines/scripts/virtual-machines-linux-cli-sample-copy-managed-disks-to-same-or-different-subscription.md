---
title: Azure CLI skriptexempel - kopia (flytta) hanterade diskar till samma eller olika prenumeration | Microsoft Docs
description: Azure CLI skriptexempel - kopia (flytta) hanterade diskar till samma eller en annan prenumeration
services: virtual-machines-linux
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/19/2017
ms.author: ramankum
ms.openlocfilehash: dcf92babf84872ffbbba81127952f8422104c723
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="copy-managed-disks-to-same-or-different-subscription-with-cli"></a><span data-ttu-id="ddc1c-103">Kopiera hanterade diskar till samma eller en annan prenumeration med CLI</span><span class="sxs-lookup"><span data-stu-id="ddc1c-103">Copy managed disks to same or different subscription with CLI</span></span>

<span data-ttu-id="ddc1c-104">Det här skriptet kopierar hanterade diskar till samma eller olika prenumeration men i samma region.</span><span class="sxs-lookup"><span data-stu-id="ddc1c-104">This script copies a managed disk to same or different subscription but in the same region.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="ddc1c-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="ddc1c-105">Sample script</span></span>

<span data-ttu-id="ddc1c-106">[!code-azurecli[huvudsakliga](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "kopiera hanteras disk")]</span><span class="sxs-lookup"><span data-stu-id="ddc1c-106">[!code-azurecli[main](../../../cli_scripts/virtual-machine/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="ddc1c-107">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="ddc1c-107">Script explanation</span></span>

<span data-ttu-id="ddc1c-108">Det här skriptet använder följande kommandon för att skapa en ny hanterade disk i målprenumerationen med ID: T för den hanterade källdisken.</span><span class="sxs-lookup"><span data-stu-id="ddc1c-108">This script uses following commands to create a new managed disk in the target subscription using the Id of the source managed disk.</span></span> <span data-ttu-id="ddc1c-109">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="ddc1c-109">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="ddc1c-110">Kommando</span><span class="sxs-lookup"><span data-stu-id="ddc1c-110">Command</span></span> | <span data-ttu-id="ddc1c-111">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="ddc1c-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="ddc1c-112">AZ disk visa</span><span class="sxs-lookup"><span data-stu-id="ddc1c-112">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="ddc1c-113">Hämtar alla egenskaper för en hanterad disk med hjälp av namn och egenskaper för resursgrupp för hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="ddc1c-113">Gets all the properties of a managed disk using the name and resource group properties of the managed disk.</span></span> <span data-ttu-id="ddc1c-114">ID-egenskapen används för att kopiera hanterade diskar till en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ddc1c-114">Id property is used to copy the managed disk to different subscription.</span></span>  |
| [<span data-ttu-id="ddc1c-115">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="ddc1c-115">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="ddc1c-116">Kopior av hanterade diskar genom att skapa en ny hanteras disk i en annan prenumeration med Id och namn överordnade hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="ddc1c-116">Copies a managed disk by creating a new managed disk in different subscription using Id and name the parent managed disk.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="ddc1c-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ddc1c-117">Next steps</span></span>

[<span data-ttu-id="ddc1c-118">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="ddc1c-118">Create a virtual machine from a managed disk</span></span>](./virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="ddc1c-119">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="ddc1c-119">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="ddc1c-120">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="ddc1c-120">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../app-service-web/app-service-cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
