---
title: Skriptexempel Azure CLI - montera operativsystemdisk | Microsoft Docs
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
ms.openlocfilehash: b54a94d833644ebfae4c1fac59e4753ab723ced4
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="troubleshoot-a-vms-operating-system-disk"></a><span data-ttu-id="e9479-103">Felsöka en operativsystemdisk för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e9479-103">Troubleshoot a VMs operating system disk</span></span>

<span data-ttu-id="e9479-104">Det här skriptet monterar operativsystemdisken för en misslyckad eller problematiska virtuell dator som en datadisk till en andra virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9479-104">This script mounts the operating system disk of a failed or problematic virtual machine as a data disk to a second virtual machine.</span></span> <span data-ttu-id="e9479-105">Detta kan vara användbar vid felsökning av disk problem eller återställa data.</span><span class="sxs-lookup"><span data-stu-id="e9479-105">This can be useful when troubleshooting disk issues or recovering data.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="e9479-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="e9479-106">Sample script</span></span>

<span data-ttu-id="e9479-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "snabbt skapa virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="e9479-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/mount-os-disk/mount-os-disk.sh "Quick Create VM")]</span></span>

## <a name="script-explanation"></a><span data-ttu-id="e9479-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="e9479-108">Script explanation</span></span>

<span data-ttu-id="e9479-109">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="e9479-109">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="e9479-110">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="e9479-110">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="e9479-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="e9479-111">Command</span></span> | <span data-ttu-id="e9479-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="e9479-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="e9479-113">AZ vm visa</span><span class="sxs-lookup"><span data-stu-id="e9479-113">az vm show</span></span>](https://docs.microsoft.com/cli/azure/vm#show) | <span data-ttu-id="e9479-114">Returnera en lista över virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e9479-114">Return a list of virtual machines.</span></span> <span data-ttu-id="e9479-115">I det här fallet används frågealternativet att returnera den virtuella disken för operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="e9479-115">In this case, the query option is used to return the virtual machine operating system disk.</span></span> <span data-ttu-id="e9479-116">Det här värdet läggs sedan till ett variabelnamn 'uri'.</span><span class="sxs-lookup"><span data-stu-id="e9479-116">This value is then added to a variable name ‘uri’.</span></span> |
| [<span data-ttu-id="e9479-117">AZ virtuella ta bort</span><span class="sxs-lookup"><span data-stu-id="e9479-117">az vm delete</span></span>](https://docs.microsoft.com/cli/azure/vm#delete) | <span data-ttu-id="e9479-118">Tar bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9479-118">Deletes a virtual machine.</span></span> |
| [<span data-ttu-id="e9479-119">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="e9479-119">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="e9479-120">Skapar en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9479-120">Creates a virtual machine.</span></span>  |
| [<span data-ttu-id="e9479-121">Koppla AZ virtuell disk</span><span class="sxs-lookup"><span data-stu-id="e9479-121">az vm disk attach</span></span>](https://docs.microsoft.com/cli/azure/vm/disk#attach) | <span data-ttu-id="e9479-122">Bifogar en disk till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9479-122">Attaches a disk to a virtual machine.</span></span> |
| [<span data-ttu-id="e9479-123">AZ vm lista-ip-adresser</span><span class="sxs-lookup"><span data-stu-id="e9479-123">az vm list-ip-addresses</span></span>](https://docs.microsoft.com/cli/azure/vm#list-ip-addresses) | <span data-ttu-id="e9479-124">Returnerar IP-adresser för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e9479-124">Returns the IP addresses of a virtual machine.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="e9479-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9479-125">Next steps</span></span>

<span data-ttu-id="e9479-126">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="e9479-126">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="e9479-127">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="e9479-127">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
