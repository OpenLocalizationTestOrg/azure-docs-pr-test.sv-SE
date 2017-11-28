---
title: "Skriptexempel Azure CLI - kopia (flytta) ögonblicksbild av en hanterad disk till samma eller en annan prenumeration med CLI | Microsoft Docs"
description: "Skriptexempel Azure CLI - kopia (flytta) ögonblicksbild av en hanterad disk till samma eller en annan prenumeration med CLI"
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
ms.openlocfilehash: 0127e342bd0c3afbe9de775399f5510814bff499
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="copy-snapshot-of-a-managed-disk-to-same-or-different-subscription-with-cli"></a><span data-ttu-id="c3093-103">Kopiera ögonblicksbild av en hanterad disk till samma eller en annan prenumeration med CLI</span><span class="sxs-lookup"><span data-stu-id="c3093-103">Copy snapshot of a managed disk to same or different subscription with CLI</span></span>

<span data-ttu-id="c3093-104">Det här skriptet kopierar en ögonblicksbild av en hanterad disk till samma eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c3093-104">This script copies a snapshot of a managed disk to same or different subscription.</span></span> <span data-ttu-id="c3093-105">Använd det här skriptet för att flytta en ögonblicksbild till annan prenumeration i samma region som den överordnade ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="c3093-105">Use this script to move a snapshot to different subscription in the same region as the parent snapshot.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="c3093-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="c3093-106">Sample script</span></span>

<span data-ttu-id="c3093-107">[!code-azurecli[huvudsakliga](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "kopiera ögonblicksbild")]</span><span class="sxs-lookup"><span data-stu-id="c3093-107">[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="c3093-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="c3093-108">Script explanation</span></span>

<span data-ttu-id="c3093-109">Det här skriptet använder följande kommandon för att skapa en ögonblicksbild i målprenumerationen använder ögonblicksbilden käll-Id.</span><span class="sxs-lookup"><span data-stu-id="c3093-109">This script uses following commands to create a snapshot in the target subscription using the Id of the source snapshot.</span></span> <span data-ttu-id="c3093-110">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="c3093-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="c3093-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="c3093-111">Command</span></span> | <span data-ttu-id="c3093-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="c3093-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="c3093-113">AZ ögonblicksbild visa</span><span class="sxs-lookup"><span data-stu-id="c3093-113">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="c3093-114">Hämtar alla egenskaper för en ögonblicksbild med hjälp av namn och egenskaper för resursgrupp av ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="c3093-114">Gets all the properties of a snapshot using the name and resource group properties of the snapshot.</span></span> <span data-ttu-id="c3093-115">ID-egenskapen används för att kopiera ögonblicksbilden till en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="c3093-115">Id property is used to copy the snapshot to different subscription.</span></span>  |
| [<span data-ttu-id="c3093-116">Skapa AZ ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="c3093-116">az snapshot create</span></span>](https://docs.microsoft.com/cli/azure/snapshot#create) | <span data-ttu-id="c3093-117">Kopierar en ögonblicksbild genom att skapa en ögonblicksbild av en annan prenumeration med Id och namnet på den överordnade ögonblicksbilden.</span><span class="sxs-lookup"><span data-stu-id="c3093-117">Copies a snapshot by creating a snapshot in different subscription using the Id and name of the parent snapshot.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="c3093-118">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c3093-118">Next steps</span></span>

[<span data-ttu-id="c3093-119">Skapa en virtuell dator från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="c3093-119">Create a virtual machine from a snapshot</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="c3093-120">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c3093-120">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="c3093-121">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="c3093-121">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
