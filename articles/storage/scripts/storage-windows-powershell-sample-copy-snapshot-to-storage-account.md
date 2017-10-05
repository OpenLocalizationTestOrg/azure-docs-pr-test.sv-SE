---
title: "Azure PowerShell skriptexempel - Export/kopiera ögonblicksbild som VHD till ett lagringskonto i annan region | Microsoft Docs"
description: "Azure PowerShell skriptexempel - Export/kopiera ögonblicksbild som VHD till ett lagringskonto i samma annan region"
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
ms.openlocfilehash: 80f83f4b66165ddd2bdd0edca2be5d026cb11a9b
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="exportcopy-managed-snapshots-as-vhd-to-a-storage-account-in-different-region-with-powershell"></a><span data-ttu-id="30061-103">Exportera eller kopieras hanterad ögonblicksbilder som VHD ett lagringskonto i en annan region med PowerShell</span><span class="sxs-lookup"><span data-stu-id="30061-103">Export/Copy managed snapshots as VHD to a storage account in different region with PowerShell</span></span>

<span data-ttu-id="30061-104">Det här skriptet exporterar en hanterad ögonblicksbild till ett lagringskonto i en annan region.</span><span class="sxs-lookup"><span data-stu-id="30061-104">This script exports a managed snapshot to a storage account in different region.</span></span> <span data-ttu-id="30061-105">Den första genererar ögonblicksbilden SAS-URI och sedan används för att kopiera den till ett lagringskonto i en annan region.</span><span class="sxs-lookup"><span data-stu-id="30061-105">It first generates the SAS URI of the snapshot and then uses it to copy it to a storage account in different region.</span></span> <span data-ttu-id="30061-106">Använd det här skriptet för att underhålla säkerhetskopiering hanterade diskar i annan region för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="30061-106">Use this script to maintain backup of your managed disks in different region for disaster recovery.</span></span>  

[!INCLUDE [sample-powershell-install](../../../includes/sample-powershell-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="30061-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="30061-107">Sample script</span></span>

<span data-ttu-id="30061-108">[!code-powershell[huvudsakliga](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "kopiera ögonblicksbild")]</span><span class="sxs-lookup"><span data-stu-id="30061-108">[!code-powershell[main](../../../powershell_scripts/storage/copy-snapshot-to-storage-account/copy-snapshot-to-storage-account.ps1 "Copy snapshot")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="30061-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="30061-109">Script explanation</span></span>

<span data-ttu-id="30061-110">Det här skriptet använder följande kommandon för att generera SAS URI för en hanterad ögonblicksbild och kopierar ögonblicksbilden till ett lagringskonto med hjälp av SAS-URI.</span><span class="sxs-lookup"><span data-stu-id="30061-110">This script uses following commands to generate SAS URI for a managed snapshot and copies the snapshot to a storage account using SAS URI.</span></span> <span data-ttu-id="30061-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="30061-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="30061-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="30061-112">Command</span></span> | <span data-ttu-id="30061-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="30061-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="30061-114">Bevilja AzureRmSnapshotAccess</span><span class="sxs-lookup"><span data-stu-id="30061-114">Grant-AzureRmSnapshotAccess</span></span>](/powershell/module/azurerm.compute/New-AzureRmDisk) | <span data-ttu-id="30061-115">Genererar SAS-URI för en ögonblicksbild som används för att kopiera den till ett lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="30061-115">Generates SAS URI for a snapshot that is used to copy it to a storage account.</span></span> |
| [<span data-ttu-id="30061-116">Ny AzureStorageContext</span><span class="sxs-lookup"><span data-stu-id="30061-116">New-AzureStorageContext</span></span>](/powershell/module/azure.storage/New-AzureStorageContext) | <span data-ttu-id="30061-117">Skapar en kontexten för lagringskontot med kontonamnet och nyckeln.</span><span class="sxs-lookup"><span data-stu-id="30061-117">Creates a storage account context using the account name and key.</span></span> <span data-ttu-id="30061-118">Den här kontexten kan användas för att utföra åtgärder för läsning och skrivning på lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="30061-118">This context can be used to perform read/write operations on the storage account.</span></span> |
| [<span data-ttu-id="30061-119">Start-AzureStorageBlobCopy</span><span class="sxs-lookup"><span data-stu-id="30061-119">Start-AzureStorageBlobCopy</span></span>](/powershell/module/azure.storage/Start-AzureStorageBlobCopy) | <span data-ttu-id="30061-120">Kopierar underliggande VHD från en ögonblicksbild till ett lagringskonto</span><span class="sxs-lookup"><span data-stu-id="30061-120">Copies the underlying VHD of a snapshot to a storage account</span></span> |

## <a name="next-steps"></a><span data-ttu-id="30061-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="30061-121">Next steps</span></span>

[<span data-ttu-id="30061-122">Skapa en hanterad disk från en virtuell Hårddisk</span><span class="sxs-lookup"><span data-stu-id="30061-122">Create a managed disk from a VHD</span></span>](./../scripts/storage-windows-powershell-sample-create-managed-disk-from-vhd.md?toc=%2fpowershell%2fmodule%2ftoc.json)

[<span data-ttu-id="30061-123">Skapa en virtuell dator från en hanterad disk</span><span class="sxs-lookup"><span data-stu-id="30061-123">Create a virtual machine from a managed disk</span></span>](./../../virtual-machines/scripts/virtual-machines-windows-powershell-sample-create-vm-from-managed-os-disks.md?toc=%2fpowershell%2fmodule%2ftoc.json)

<span data-ttu-id="30061-124">Mer information om Azure PowerShell-modulen finns [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="30061-124">For more information on the Azure PowerShell module, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="30061-125">Ytterligare virtuella PowerShell-skript-exempel finns i den [Virtuella för Windows Azure-dokumentationen](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="30061-125">Additional virtual machine PowerShell script samples can be found in the [Azure Windows VM documentation](../../virtual-machines/windows/powershell-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>