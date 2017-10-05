---
title: Distribuera virtuella Linux-datorer i Azure | Microsoft Docs
description: "Hur du distribuerar virtuella Linux-datorer i Azure för att minimera SSH-anslutningsproblem."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
tags: azure-resource-manager,top-support-issue
ms.assetid: e9530dd6-f5b0-4160-b36b-d75151d99eb7
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: support-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 06/23/2017
ms.author: iainfou
ms.openlocfilehash: 7a8653a82775e718c38f65f246d997ba61f99d58
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="redeploy-linux-virtual-machine-to-new-azure-node"></a><span data-ttu-id="fa8a0-103">Distribuera virtuell Linux-dator till den nya noden i Azure</span><span class="sxs-lookup"><span data-stu-id="fa8a0-103">Redeploy Linux virtual machine to new Azure node</span></span>
<span data-ttu-id="fa8a0-104">Om du står inför svårigheter felsökning SSH eller kan bidra till att programmet åtkomst till en Linux-dator (VM) i Azure, omdistribuera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="fa8a0-104">If you face difficulties troubleshooting SSH or application access to a Linux virtual machine (VM) in Azure, redeploying the VM may help.</span></span> <span data-ttu-id="fa8a0-105">När du distribuerar en virtuell dator, flyttar den virtuella datorn till en ny nod i Azure-infrastrukturen och sedan aktiveras den tillbaka.</span><span class="sxs-lookup"><span data-stu-id="fa8a0-105">When you redeploy a VM, it moves the VM to a new node within the Azure infrastructure and then powers it back on.</span></span> <span data-ttu-id="fa8a0-106">Alla konfigurationsalternativ och associerade resurser bevaras.</span><span class="sxs-lookup"><span data-stu-id="fa8a0-106">All your configuration options and associated resources are retained.</span></span> <span data-ttu-id="fa8a0-107">Den här artikeln visar hur du distribuerar en virtuell dator med hjälp av Azure CLI eller Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fa8a0-107">This article shows you how to redeploy a VM using Azure CLI or the Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="fa8a0-108">När du distribuerar en virtuell dator tillfälliga disken går förlorad och dynamiska IP-adresser som är kopplade till virtuella nätverksgränssnittet har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="fa8a0-108">After you redeploy a VM, the temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 

<span data-ttu-id="fa8a0-109">Du kan distribuera en virtuell dator med någon av följande alternativ.</span><span class="sxs-lookup"><span data-stu-id="fa8a0-109">You can redeploy a VM using one of the following options.</span></span> <span data-ttu-id="fa8a0-110">Du behöver bara välja ett alternativ för att distribuera om den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="fa8a0-110">You only need to choose one option to redeploy your VM:</span></span>

- [<span data-ttu-id="fa8a0-111">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fa8a0-111">Azure CLI 2.0</span></span>](#azure-cli-20)
- [<span data-ttu-id="fa8a0-112">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fa8a0-112">Azure CLI 1.0</span></span>](#azure-cli-10)
- [<span data-ttu-id="fa8a0-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="fa8a0-113">Azure portal</span></span>](#using-azure-portal)

## <a name="use-the-azure-cli-20"></a><span data-ttu-id="fa8a0-114">Använda Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="fa8a0-114">Use the Azure CLI 2.0</span></span>
<span data-ttu-id="fa8a0-115">Installera senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in till en Azure med hjälp av [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="fa8a0-115">Install the latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in to an Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="fa8a0-116">Distribuera den virtuella datorn med [az vm Omdistributionen](/cli/azure/vm#redeploy).</span><span class="sxs-lookup"><span data-stu-id="fa8a0-116">Redeploy your VM with [az vm redeploy](/cli/azure/vm#redeploy).</span></span> <span data-ttu-id="fa8a0-117">I följande exempel återdistribuerar den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="fa8a0-117">The following example redeploys the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-the-azure-cli-10"></a><span data-ttu-id="fa8a0-118">Använda Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="fa8a0-118">Use the Azure CLI 1.0</span></span>
<span data-ttu-id="fa8a0-119">Installera den [senaste Azure CLI 1.0](../../cli-install-nodejs.md), logga in på ett Azure-konto och se till att du är i läget Resource Manager (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="fa8a0-119">Install the [latest Azure CLI 1.0](../../cli-install-nodejs.md), log in to an Azure account, and make sure that you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="fa8a0-120">I följande exempel återdistribuerar den virtuella datorn med namnet *myVM* i resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="fa8a0-120">The following example redeploys the VM named *myVM* in the resource group named *myResourceGroup*:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="fa8a0-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fa8a0-121">Next steps</span></span>
<span data-ttu-id="fa8a0-122">Om du har problem med anslutningen till den virtuella datorn kan du hittar detaljerad hjälp på [felsökning av SSH-anslutningar](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [detaljerad SSH felsökningssteg](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fa8a0-122">If you are having issues connecting to your VM, you can find specific help on [troubleshooting SSH connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [detailed SSH troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="fa8a0-123">Om du inte kommer åt ett program som körs på den virtuella datorn, du kan också läsa [felsökning av problem med programmet](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="fa8a0-123">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

