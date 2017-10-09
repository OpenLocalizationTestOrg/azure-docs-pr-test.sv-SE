---
title: aaaAzure skriptexempel CLI - montera operativsystemdisk | Microsoft Docs
description: Skriptexempel Azure CLI - montera operativsystemdisk
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/27/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: 5c614d09a64780575b70424d29052f1a6affec59
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a><span data-ttu-id="bde89-103">Felsöka en operativsystemdisk för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="bde89-103">Troubleshoot a VMs operating system disk</span></span>

<span data-ttu-id="bde89-104">Det här skriptet monterar hello operativsystemdisken för en misslyckad eller problematiska virtuell dator som data disk tooa andra virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bde89-104">This script mounts hello operating system disk of a failed or problematic virtual machine as a data disk tooa second virtual machine.</span></span> <span data-ttu-id="bde89-105">Detta kan vara användbar vid felsökning av disk problem eller återställa data.</span><span class="sxs-lookup"><span data-stu-id="bde89-105">This can be useful when troubleshooting disk issues or recovering data.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="bde89-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="bde89-106">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]

## <a name="script-explanation"></a><span data-ttu-id="bde89-107">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="bde89-107">Script explanation</span></span>

<span data-ttu-id="bde89-108">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="bde89-108">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="bde89-109">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="bde89-109">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="bde89-110">Kommando</span><span class="sxs-lookup"><span data-stu-id="bde89-110">Command</span></span> | <span data-ttu-id="bde89-111">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="bde89-111">Notes</span></span> |
|---|---|
| [<span data-ttu-id="bde89-112">AZ vm visa</span><span class="sxs-lookup"><span data-stu-id="bde89-112">az vm show</span></span>](https://docs.microsoft.com/cli/azure/vm#show) | <span data-ttu-id="bde89-113">Returnera en lista över virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bde89-113">Return a list of virtual machines.</span></span> <span data-ttu-id="bde89-114">I det här fallet är hello frågealternativet används tooreturn hello virtuella operativsystemdisken.</span><span class="sxs-lookup"><span data-stu-id="bde89-114">In this case, hello query option is used tooreturn hello virtual machine operating system disk.</span></span> <span data-ttu-id="bde89-115">Det här värdet läggs sedan tooa variabelnamnet 'uri'.</span><span class="sxs-lookup"><span data-stu-id="bde89-115">This value is then added tooa variable name ‘uri’.</span></span> |
| [<span data-ttu-id="bde89-116">AZ virtuella ta bort</span><span class="sxs-lookup"><span data-stu-id="bde89-116">az vm delete</span></span>](https://docs.microsoft.com/cli/azure/vm#delete) | <span data-ttu-id="bde89-117">Tar bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bde89-117">Deletes a virtual machine.</span></span> |
| [<span data-ttu-id="bde89-118">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="bde89-118">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="bde89-119">Skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bde89-119">Creates a virtual machine.</span></span>  |
| [<span data-ttu-id="bde89-120">Koppla AZ virtuell disk</span><span class="sxs-lookup"><span data-stu-id="bde89-120">az vm disk attach</span></span>](https://docs.microsoft.com/cli/azure/vm/disk#attach) | <span data-ttu-id="bde89-121">Bifogar en disk tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bde89-121">Attaches a disk tooa virtual machine.</span></span> |
| [<span data-ttu-id="bde89-122">AZ vm lista-ip-adresser</span><span class="sxs-lookup"><span data-stu-id="bde89-122">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="bde89-123">Returnerar hello IP-adresser för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bde89-123">Returns hello IP addresses of a virtual machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="bde89-124">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bde89-124">Next steps</span></span>

<span data-ttu-id="bde89-125">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="bde89-125">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="bde89-126">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="bde89-126">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
