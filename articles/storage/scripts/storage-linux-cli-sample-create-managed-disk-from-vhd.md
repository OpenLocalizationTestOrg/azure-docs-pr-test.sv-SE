---
title: "aaaAzure CLI skriptexempel - skapa en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration | Microsoft Docs"
description: "Azure CLI-skript Sample - skapa en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration"
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
ms.openlocfilehash: 1e792fdbb7daea92bf6a6589a5d8aab5b9b5a670
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-managed-disk-from-a-vhd-file-in-a-storage-account-in-hello-same-subscription-with-cli"></a><span data-ttu-id="32426-103">Skapa en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration med CLI</span><span class="sxs-lookup"><span data-stu-id="32426-103">Create a managed disk from a VHD file in a storage account in hello same subscription with CLI</span></span>

<span data-ttu-id="32426-104">Det här skriptet skapar en hanterad disk från en VHD-fil i ett lagringskonto i hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="32426-104">This script creates a managed disk from a VHD file in a storage account in hello same subscription.</span></span> <span data-ttu-id="32426-105">Använd det här skriptet tooimport en särskild (inte generaliserad/Sysprep) VHD toomanaged OS disk toocreate en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="32426-105">Use this script tooimport a specialized (not generalized/sysprepped) VHD toomanaged OS disk toocreate a virtual machine.</span></span> <span data-ttu-id="32426-106">Eller använda den tooimport en VHD toomanaged data datadisk.</span><span class="sxs-lookup"><span data-stu-id="32426-106">Or, use it tooimport a data VHD toomanaged data disk.</span></span> 


[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="32426-107">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="32426-107">Sample script</span></span>

[!code-azurecli[main](../../../cli_scripts/storage/create-managed-data-disks-from-vhd/create-managed-data-disks-from-vhd.sh "Create managed disk from VHD")]


## <a name="script-explanation"></a><span data-ttu-id="32426-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="32426-108">Script explanation</span></span>

<span data-ttu-id="32426-109">Det här skriptet använder följande kommandon toocreate hanterade diskar från en virtuell Hårddisk.</span><span class="sxs-lookup"><span data-stu-id="32426-109">This script uses following commands toocreate a managed disk from a VHD.</span></span> <span data-ttu-id="32426-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="32426-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="32426-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="32426-111">Command</span></span> | <span data-ttu-id="32426-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="32426-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="32426-113">Skapa AZ disk</span><span class="sxs-lookup"><span data-stu-id="32426-113">az disk create</span></span>](https://docs.microsoft.com/cli/azure/disk#create) | <span data-ttu-id="32426-114">Skapar en hanterad disk med hjälp av en VHD-URI i ett lagringskonto i hello samma prenumeration</span><span class="sxs-lookup"><span data-stu-id="32426-114">Creates a managed disk using URI of a VHD in a storage account in hello same subscription</span></span> |

## <a name="next-steps"></a><span data-ttu-id="32426-115">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32426-115">Next steps</span></span>

[<span data-ttu-id="32426-116">Skapa en virtuell dator genom att koppla en hanterade diskar som OS-disk</span><span class="sxs-lookup"><span data-stu-id="32426-116">Create a virtual machine by attaching a managed disk as OS disk</span></span>](./../../virtual-machines/scripts/virtual-machines-linux-cli-sample-create-vm-from-managed-os-disks.md?toc=%2fcli%2fmodule%2ftoc.json)

<span data-ttu-id="32426-117">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="32426-117">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="32426-118">Ytterligare virtuella datorer och hanterade diskar CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="32426-118">Additional virtual machine and managed disks CLI script samples can be found in hello [Azure Linux VM documentation](../../virtual-machines/linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
