---
title: "Azure CLI-skript Sample - skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma prenumeration | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma prenumeration"
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
ms.openlocfilehash: 5022ca23ac2c2e515a9b80d44b1221f3c05fecb1
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-the-same-subscription-with-cli"></a><span data-ttu-id="1a820-103">Skapa en hanterad disk från en VHD-fil i ett lagringskonto i samma prenumeration med CLI</span><span class="sxs-lookup"><span data-stu-id="1a820-103">Create a managed disk from a VHD file in a storage account in the same subscription with CLI</span></span>

<span data-ttu-id="1a820-104">Det här skriptet skapar en hanterad disk från en VHD-fil i ett lagringskonto i samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1a820-104">This script creates a managed disk from a VHD file in a storage account in the same subscription.</span></span> <span data-ttu-id="1a820-105">Använd det här skriptet för att importera ett speciellt (inte generaliserad/Sysprep) VHD till hanterade OS-disken för att skapa en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="1a820-105">Use this script to import a specialized (not generalized/sysprepped) VHD to managed OS disk to create a virtual machine.</span></span> <span data-ttu-id="1a820-106">Eller använda den för att importera en VHD-data till hanterade datadisk.</span><span class="sxs-lookup"><span data-stu-id="1a820-106">Or, use it to import a data VHD to managed data disk.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="1a820-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="1a820-107">Sample script</span></span>

<span data-ttu-id="1a820-108">[!code-azurecli[huvudsakliga](../../../cli_scripts/storage/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "skapa hanterade disk från virtuell Hårddisk")]</span><span class="sxs-lookup"><span data-stu-id="1a820-108">[!code-azurecli[main](../../../cli_scripts/storage/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]</span></span>


## <a name="script-explanation"></a><span data-ttu-id="1a820-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="1a820-109">Script explanation</span></span>

<span data-ttu-id="1a820-110">Det här skriptet använder följande kommandon för att skapa en hanterad disk från en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="1a820-110">This script uses following commands to create a managed disk from a VHD.</span></span> <span data-ttu-id="1a820-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="1a820-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="1a820-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="1a820-112">Command</span></span> | <span data-ttu-id="1a820-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="1a820-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="1a820-114">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="1a820-114">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="1a820-115">Skapar en hanterad disk med hjälp av en VHD-URI i ett lagringskonto i samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="1a820-115">Creates a managed disk using URI of a VHD in a storage account in the same subscription</span></span> |

## <a name="next-steps"></a><span data-ttu-id="1a820-116">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a820-116">Next steps</span></span>

[<span data-ttu-id="1a820-117">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="1a820-117">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="1a820-118">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="1a820-118">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="1a820-119">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1a820-119">Additional virtual machine and managed disks CLI script samples can be found in the [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
