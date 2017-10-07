---
title: "aaaAzure skriptexempel PowerShell - Export/kopiera ögonblicksbild som VHD tooa storage-konto i en annan region | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Export/kopiera ögonblicksbild som VHD tooa lagringskonto i samma annan region"
services: virtual-machines-windows
documentationcenter: storage
author: ramankumarlive
manager: kavithag
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/05/2017
ms.author: ramankum
ms.openlocfilehash: 3b3e38c6b06bfa1e117f4e913dfc09443a795196
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-tooa-storage-account-in-different-region-with-powershell"></a><span data-ttu-id="f40aa-103">Export/kopiera hanterad ögonblicksbilder som VHD tooa storage-konto i en annan region med PowerShell</span><span class="sxs-lookup"><span data-stu-id="f40aa-103">Export/Copy managed snapshots as VHD tooa storage account in different region with PowerShell</span></span>

<span data-ttu-id="f40aa-104">Det här skriptet exporterar ett hanterade ögonblicksbild tooa storage-konto i annan region.</span><span class="sxs-lookup"><span data-stu-id="f40aa-104">This script exports a managed snapshot tooa storage account in different region.</span></span> <span data-ttu-id="f40aa-105">Den första genererar hello hello ögonblicksbild SAS-URI och använder sedan toocopy den tooa storage-konto i en annan region.</span><span class="sxs-lookup"><span data-stu-id="f40aa-105">It first generates hello SAS URI of hello snapshot and then uses it toocopy it tooa storage account in different region.</span></span> <span data-ttu-id="f40aa-106">Använda skript toomaintain säkerhetskopian hanterade diskar i annan region för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="f40aa-106">Use this script toomaintain backup of your managed disks in different region for disaster recovery.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="f40aa-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="f40aa-107">Sample script</span></span>

[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]


## <a name="script-explanation"></a><span data-ttu-id="f40aa-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="f40aa-108">Script explanation</span></span>

<span data-ttu-id="f40aa-109">Det här skriptet använder följande kommandon toogenerate SAS-URI för en hanterad ögonblicksbilder och kopior och hello ögonblicksbild tooa lagringskonto med hjälp av SAS-URI.</span><span class="sxs-lookup"><span data-stu-id="f40aa-109">This script uses following commands toogenerate SAS URI for a managed snapshot and copies hello snapshot tooa storage account using SAS URI.</span></span> <span data-ttu-id="f40aa-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="f40aa-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="f40aa-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="f40aa-111">Command</span></span> | <span data-ttu-id="f40aa-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="f40aa-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="f40aa-113">Bevilja AzureRmSnapshotAccess</span><span class="sxs-lookup"><span data-stu-id="f40aa-113">Grant-AzureRmSnapshotAccess</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="f40aa-114">Genererar SAS-URI för en ögonblicksbild som är används toocopy den tooa storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f40aa-114">Generates SAS URI for a snapshot that is used toocopy it tooa storage account.</span></span> |
| [<span data-ttu-id="f40aa-115">Ny AzureStorageContext</span><span class="sxs-lookup"><span data-stu-id="f40aa-115">New-AzureStorageContext</span></span>](/powershell/module/azure.storage/New-AzureStorageContext) | <span data-ttu-id="f40aa-116">Skapar en kontexten för lagringskontot med hello kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="f40aa-116">Creates a storage account context using hello account name and key.</span></span> <span data-ttu-id="f40aa-117">Den här kontexten kan vara används tooperform läsning och skrivning åtgärder på hello storage-konto.</span><span class="sxs-lookup"><span data-stu-id="f40aa-117">This context can be used tooperform read/write operations on hello storage account.</span></span> |
| [<span data-ttu-id="f40aa-118">Start-AzureStorageBlobCopy</span><span class="sxs-lookup"><span data-stu-id="f40aa-118">Start-AzureStorageBlobCopy</span></span>](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | <span data-ttu-id="f40aa-119">Kopior hello underliggande VHD från en ögonblicksbild tooa storage-konto</span><span class="sxs-lookup"><span data-stu-id="f40aa-119">Copies hello underlying VHD of a snapshot tooa storage account</span></span> |

## <a name="next-steps"></a><span data-ttu-id="f40aa-120">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f40aa-120">Next steps</span></span>

[<span data-ttu-id="f40aa-121">Skapa en hanterad disk från en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="f40aa-121">Create a managed disk from a VHD</span></span>](./../scripts/storage-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[<span data-ttu-id="f40aa-122">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="f40aa-122">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="f40aa-123">Mer information om hello Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="f40aa-123">For more information on hello Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="f40aa-124">Ytterligare virtuella PowerShell-skript-exempel finns i hello [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="f40aa-124">Additional virtual machine PowerShell script samples can be found in hello [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
