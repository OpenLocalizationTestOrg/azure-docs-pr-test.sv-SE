---
title: "aaaAzure skriptexempel CLI - Export/kopiera ögonblicksbild som VHD tooa storage-konto i en annan region | Microsoft Docs"
description: "Skriptexempel Azure CLI - Export/kopiera ögonblicksbild som VHD tooa lagringskonto i samma eller olika prenumeration"
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
ms.openlocfilehash: 027c5e588c4f10d64d125c17f4c78a7d8e1ef060
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-cli"></a><span data-ttu-id="99f09-103">Export/kopiera hanterad ögonblicksbilder som VHD tooa storage-konto i en annan region med CLI</span><span class="sxs-lookup"><span data-stu-id="99f09-103">Export/Copy managed snapshots as VHD tooa storage account in different region with CLI</span></span>

<span data-ttu-id="99f09-104">Det här skriptet exporterar ett hanterade ögonblicksbild tooa storage-konto i annan region.</span><span class="sxs-lookup"><span data-stu-id="99f09-104">This script exports a managed snapshot tooa storage account in different region.</span></span> <span data-ttu-id="99f09-105">Den första genererar hello hello ögonblicksbild SAS-URI och använder sedan toocopy den tooa storage-konto i en annan region.</span><span class="sxs-lookup"><span data-stu-id="99f09-105">It first generates hello SAS URI of hello snapshot and then uses it toocopy it tooa storage account in different region.</span></span> <span data-ttu-id="99f09-106">Använda skript toomaintain säkerhetskopian hanterade diskar i annan region för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="99f09-106">Use this script toomaintain backup of your managed disks in different region for disaster recovery.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="99f09-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="99f09-107">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/copy-snapshots-to-storage-account/copy-snapshots-to-storage-account.sh "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="99f09-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="99f09-108">Script explanation</span></span>

<span data-ttu-id="99f09-109">Det här skriptet använder följande kommandon toogenerate SAS-URI för en hanterad ögonblicksbilder och kopior och hello ögonblicksbild tooa lagringskonto med hjälp av SAS-URI.</span><span class="sxs-lookup"><span data-stu-id="99f09-109">This script uses following commands toogenerate SAS URI for a managed snapshot and copies hello snapshot tooa storage account using SAS URI.</span></span> <span data-ttu-id="99f09-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="99f09-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="99f09-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="99f09-111">Command</span></span> | <span data-ttu-id="99f09-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="99f09-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="99f09-113">AZ ögonblicksbild bevilja åtkomst</span><span class="sxs-lookup"><span data-stu-id="99f09-113">az snapshot grant-access</span></span>](https://docs.microsoft.com/cli/azure/snapshot#grant-access) | <span data-ttu-id="99f09-114">Genererar skrivskyddad SAS som används toocopy underliggande VHD-filen tooa lagringskonto eller ladda ned det lokala tooon</span><span class="sxs-lookup"><span data-stu-id="99f09-114">Generates read-only SAS that is used toocopy underlying VHD file tooa storage account or download it tooon-premises</span></span>  |
| [<span data-ttu-id="99f09-115">Starta AZ storage blob-kopia</span><span class="sxs-lookup"><span data-stu-id="99f09-115">az storage blob copy start</span></span>](https://docs.microsoft.com/en-us/cli/azure/storage/blob/copy#start) | <span data-ttu-id="99f09-116">Kopierar en blobb asynkront från en tooanother för storage-konto</span><span class="sxs-lookup"><span data-stu-id="99f09-116">Copies a blob asynchronously from one storage account tooanother</span></span> |

## <a name="next-steps"></a><span data-ttu-id="99f09-117">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="99f09-117">Next steps</span></span>

[<span data-ttu-id="99f09-118">Skapa en hanterad disk från en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="99f09-118">Create a managed disk from a VHD</span></span>](./../scripts/storage-linux-cli-sample-create-managed-disk-from-vhd.md?toc=%2fcli%2fmodule%2ftoc.json)

[<span data-ttu-id="99f09-119">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="99f09-119">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="99f09-120">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="99f09-120">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="99f09-121">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="99f09-121">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
