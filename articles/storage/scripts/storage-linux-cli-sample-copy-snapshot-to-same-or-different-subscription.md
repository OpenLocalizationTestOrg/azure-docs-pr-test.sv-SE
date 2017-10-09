---
title: "aaaAzure skriptexempel CLI - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration med CLI | Microsoft Docs"
description: "Skriptexempel Azure CLI - kopia (flytta) ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration med CLI"
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
ms.openlocfilehash: 4a21fd2435181a033b563100888aba0c5834496d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-snapshot-of-a-managed-disk-toosame-or-different-subscription-with-cli"></a><span data-ttu-id="37dc6-103">Kopiera ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration med CLI</span><span class="sxs-lookup"><span data-stu-id="37dc6-103">Copy snapshot of a managed disk toosame or different subscription with CLI</span></span>

<span data-ttu-id="37dc6-104">Det här skriptet kopierar en ögonblicksbild av en toosame för hanterade diskar eller en annan prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37dc6-104">This script copies a snapshot of a managed disk toosame or different subscription.</span></span> <span data-ttu-id="37dc6-105">Använd det här skriptet toomove en ögonblicksbild toodifferent prenumeration i hello samma region som hello överordnade ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="37dc6-105">Use this script toomove a snapshot toodifferent subscription in hello same region as hello parent snapshot.</span></span>


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="37dc6-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="37dc6-106">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshot-to-same-or-different-subscription/copy-snapshot-to-same-or-different-subscription.sh "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="37dc6-107">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="37dc6-107">Script explanation</span></span>

<span data-ttu-id="37dc6-108">Det här skriptet använder följande kommandon toocreate en ögonblicksbild i hello mål en prenumeration med hjälp av hello-Id för hello källa ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="37dc6-108">This script uses following commands toocreate a snapshot in hello target subscription using hello Id of hello source snapshot.</span></span> <span data-ttu-id="37dc6-109">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="37dc6-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="37dc6-110">Kommando</span><span class="sxs-lookup"><span data-stu-id="37dc6-110">Command</span></span> | <span data-ttu-id="37dc6-111">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="37dc6-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="37dc6-112">AZ ögonblicksbild visa</span><span class="sxs-lookup"><span data-stu-id="37dc6-112">az snapshot show</span></span>](https://docs.microsoft.com/cli/azure/snapshot#show) | <span data-ttu-id="37dc6-113">Hämtar alla hello egenskaperna för en ögonblicksbild med hello namn och egenskaper för resursgrupp hello ögonblicksbilder.</span><span class="sxs-lookup"><span data-stu-id="37dc6-113">Gets all hello properties of a snapshot using hello name and resource group properties of hello snapshot.</span></span> <span data-ttu-id="37dc6-114">ID-egenskap är används toocopy hello ögonblicksbild toodifferent prenumeration.</span><span class="sxs-lookup"><span data-stu-id="37dc6-114">Id property is used toocopy hello snapshot toodifferent subscription.</span></span>  |
| [<span data-ttu-id="37dc6-115">Skapa AZ ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="37dc6-115">az snapshot create</span></span>](https://docs.microsoft.com/cli/azure/snapshot#create) | <span data-ttu-id="37dc6-116">Kopierar en ögonblicksbild genom att skapa en ögonblicksbild av en annan prenumeration med hello-Id och namnet på hello överordnade ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="37dc6-116">Copies a snapshot by creating a snapshot in different subscription using hello Id and name of hello parent snapshot.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="37dc6-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="37dc6-117">Next steps</span></span>

[<span data-ttu-id="37dc6-118">Skapa en virtuell dator från en ögonblicksbild</span><span class="sxs-lookup"><span data-stu-id="37dc6-118">Create a virtual machine from a snapshot</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-snapshot.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="37dc6-119">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="37dc6-119">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="37dc6-120">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="37dc6-120">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
