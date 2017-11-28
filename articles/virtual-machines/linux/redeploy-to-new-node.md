---
title: aaaRedeploy Linux virtuella datorer i Azure | Microsoft Docs
description: "Hur tooredeploy Linux-datorer i Azure toomitigate SSH-anslutningen utfärdar."
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
ms.openlocfilehash: 9adfd1b11f262d362133366b2bba5e69c70c9b82
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="redeploy-linux-virtual-machine-toonew-azure-node"></a><span data-ttu-id="89e97-103">Distribuera Linux virtuella toonew Azure nod</span><span class="sxs-lookup"><span data-stu-id="89e97-103">Redeploy Linux virtual machine toonew Azure node</span></span>
<span data-ttu-id="89e97-104">Om du står inför svårigheter felsökning SSH eller program använder tooa Linux virtuella dator (VM) i Azure kan kan omdistribuera hello VM hjälpa.</span><span class="sxs-lookup"><span data-stu-id="89e97-104">If you face difficulties troubleshooting SSH or application access tooa Linux virtual machine (VM) in Azure, redeploying hello VM may help.</span></span> <span data-ttu-id="89e97-105">När du distribuerar en virtuell dator flyttas hello VM tooa ny nod i hello Azure-infrastrukturen och sedan aktiveras den tillbaka.</span><span class="sxs-lookup"><span data-stu-id="89e97-105">When you redeploy a VM, it moves hello VM tooa new node within hello Azure infrastructure and then powers it back on.</span></span> <span data-ttu-id="89e97-106">Alla konfigurationsalternativ och associerade resurser bevaras.</span><span class="sxs-lookup"><span data-stu-id="89e97-106">All your configuration options and associated resources are retained.</span></span> <span data-ttu-id="89e97-107">Den här artikeln beskrivs hur du tooredeploy en virtuell dator med hjälp av Azure CLI eller hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="89e97-107">This article shows you how tooredeploy a VM using Azure CLI or hello Azure portal.</span></span>

> [!NOTE]
> <span data-ttu-id="89e97-108">När du distribuerar en virtuell dator hello diskutrymme går förlorad och dynamiska IP-adresser som är kopplade till virtuella nätverksgränssnittet har uppdaterats.</span><span class="sxs-lookup"><span data-stu-id="89e97-108">After you redeploy a VM, hello temporary disk is lost and dynamic IP addresses associated with virtual network interface are updated.</span></span> 

<span data-ttu-id="89e97-109">Du kan distribuera en virtuell dator med någon av följande alternativ för hello.</span><span class="sxs-lookup"><span data-stu-id="89e97-109">You can redeploy a VM using one of hello following options.</span></span> <span data-ttu-id="89e97-110">Du behöver bara toochoose en alternativet tooredeploy den virtuella datorn:</span><span class="sxs-lookup"><span data-stu-id="89e97-110">You only need toochoose one option tooredeploy your VM:</span></span>

- [<span data-ttu-id="89e97-111">Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="89e97-111">Azure CLI 2.0</span></span>](#azure-cli-20)
- [<span data-ttu-id="89e97-112">Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="89e97-112">Azure CLI 1.0</span></span>](#azure-cli-10)
- [<span data-ttu-id="89e97-113">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="89e97-113">Azure portal</span></span>](#using-azure-portal)

## <a name="use-hello-azure-cli-20"></a><span data-ttu-id="89e97-114">Använd hello Azure CLI 2.0</span><span class="sxs-lookup"><span data-stu-id="89e97-114">Use hello Azure CLI 2.0</span></span>
<span data-ttu-id="89e97-115">Installera hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).</span><span class="sxs-lookup"><span data-stu-id="89e97-115">Install hello latest [Azure CLI 2.0](/cli/azure/install-az-cli2) and log in tooan Azure account using [az login](/cli/azure/#login).</span></span>

<span data-ttu-id="89e97-116">Distribuera den virtuella datorn med [az vm Omdistributionen](/cli/azure/vm#redeploy).</span><span class="sxs-lookup"><span data-stu-id="89e97-116">Redeploy your VM with [az vm redeploy](/cli/azure/vm#redeploy).</span></span> <span data-ttu-id="89e97-117">följande exempel återdistribuerar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="89e97-117">hello following example redeploys hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

```azurecli
az vm redeploy --resource-group myResourceGroup --name myVM 
```

## <a name="use-hello-azure-cli-10"></a><span data-ttu-id="89e97-118">Använd hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="89e97-118">Use hello Azure CLI 1.0</span></span>
<span data-ttu-id="89e97-119">Installera hello [senaste Azure CLI 1.0](../../cli-install-nodejs.md), logga in tooan Azure-konto och se till att du är i läget Resource Manager (`azure config mode arm`).</span><span class="sxs-lookup"><span data-stu-id="89e97-119">Install hello [latest Azure CLI 1.0](../../cli-install-nodejs.md), log in tooan Azure account, and make sure that you are in Resource Manager mode (`azure config mode arm`).</span></span>

<span data-ttu-id="89e97-120">följande exempel återdistribuerar hello hello virtuella datorn med namnet *myVM* i hello resursgrupp med namnet *myResourceGroup*:</span><span class="sxs-lookup"><span data-stu-id="89e97-120">hello following example redeploys hello VM named *myVM* in hello resource group named *myResourceGroup*:</span></span>

```azurecli
azure vm redeploy --resource-group myResourceGroup --vm-name myVM 
```

[!INCLUDE [virtual-machines-common-redeploy-to-new-node](../../../includes/virtual-machines-common-redeploy-to-new-node.md)]

## <a name="next-steps"></a><span data-ttu-id="89e97-121">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="89e97-121">Next steps</span></span>
<span data-ttu-id="89e97-122">Om du har problem med anslutning tooyour VM du hittar detaljerad hjälp på [felsökning av SSH-anslutningar](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) eller [detaljerad SSH felsökningssteg](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="89e97-122">If you are having issues connecting tooyour VM, you can find specific help on [troubleshooting SSH connections](troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) or [detailed SSH troubleshooting steps](detailed-troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="89e97-123">Om du inte kommer åt ett program som körs på den virtuella datorn, du kan också läsa [felsökning av problem med programmet](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="89e97-123">If you cannot access an application running on your VM, you can also read [application troubleshooting issues](troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

