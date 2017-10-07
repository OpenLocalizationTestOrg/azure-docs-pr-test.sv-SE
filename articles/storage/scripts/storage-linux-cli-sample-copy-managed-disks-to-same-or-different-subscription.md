---
title: aaaAzure CLI skriptexempel - kopia (flytta) hanteras diskar toosame eller annan prenumeration | Microsoft Docs
description: Azure CLI skriptexempel - kopia (flytta) hanteras diskar toosame eller en annan prenumeration
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
ms.openlocfilehash: 8581169baa0fd0e0eec1c72eab77b657f48b1cfa
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="copy-managed-disks-toosame-or-different-subscription-with-cli"></a><span data-ttu-id="94793-103">Kopiera toosame för hanterade diskar eller annan prenumeration med CLI</span><span class="sxs-lookup"><span data-stu-id="94793-103">Copy managed disks toosame or different subscription with CLI</span></span>

<span data-ttu-id="94793-104">Det här skriptet kopierar en toosame för hanterade diskar eller en annan prenumeration men hello i samma region.</span><span class="sxs-lookup"><span data-stu-id="94793-104">This script copies a managed disk toosame or different subscription but in hello same region.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="94793-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="94793-105">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/copy-managed-disks-to-same-or-different-subscription/copy-managed-disks-to-same-or-different-subscription.sh "Copy managed disk")]


## <a name="script-explanation"></a><span data-ttu-id="94793-106">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="94793-106">Script explanation</span></span>

<span data-ttu-id="94793-107">Det här skriptet använder följande kommandon toocreate en ny hanterade disk i hello mål en prenumeration med hjälp av hello-Id för hello källa hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="94793-107">This script uses following commands toocreate a new managed disk in hello target subscription using hello Id of hello source managed disk.</span></span> <span data-ttu-id="94793-108">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="94793-108">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="94793-109">Kommando</span><span class="sxs-lookup"><span data-stu-id="94793-109">Command</span></span> | <span data-ttu-id="94793-110">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="94793-110">Notes</span></span> |
|---|---|
| [<span data-ttu-id="94793-111">AZ disk visa</span><span class="sxs-lookup"><span data-stu-id="94793-111">az disk show</span></span>](https://docs.microsoft.com/cli/azure/disk#show) | <span data-ttu-id="94793-112">Hämtar alla hello egenskaper för en hanterad disk med hjälp av egenskaper för hello-namn och en resurs grupp av hello hanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="94793-112">Gets all hello properties of a managed disk using hello name and resource group properties of hello managed disk.</span></span> <span data-ttu-id="94793-113">ID-egenskap är används toocopy hello hanteras disk toodifferent prenumeration.</span><span class="sxs-lookup"><span data-stu-id="94793-113">Id property is used toocopy hello managed disk toodifferent subscription.</span></span>  |
| [<span data-ttu-id="94793-114">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="94793-114">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="94793-115">Kopierar hanterade diskar genom att skapa en ny hanterad disk i en annan prenumeration med Id och namn hello överordnade hanterade disken.</span><span class="sxs-lookup"><span data-stu-id="94793-115">Copies a managed disk by creating a new managed disk in different subscription using Id and name hello parent managed disk.</span></span>  |

## <a name="next-steps"></a><span data-ttu-id="94793-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="94793-116">Next steps</span></span>

[<span data-ttu-id="94793-117">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="94793-117">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="94793-118">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="94793-118">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="94793-119">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="94793-119">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
