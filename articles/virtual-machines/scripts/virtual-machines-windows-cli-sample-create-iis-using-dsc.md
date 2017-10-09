---
title: aaaAzure CLI skriptexempel - skapa en virtuell Windows Server 2016-dator med IIS med DSC | Microsoft Docs
description: "Azure CLI-skript exempel – skapa en virtuell Windows Server 2016-dator med IIS använder DSC"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: rickstercdn
manager: timlt
editor: tysonn
tags: 
ms.assetid: 
ms.service: virtual-machines-Windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 02/23/2017
ms.author: rclaus
ms.custom: mvc
ms.openlocfilehash: 9883b5925dddca3dd53d083679a0e76d0fb982e5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-vm-with-iis-using-dsc"></a><span data-ttu-id="28c42-103">Skapa en virtuell dator med IIS använder DSC</span><span class="sxs-lookup"><span data-stu-id="28c42-103">Create a VM with IIS using DSC</span></span>

<span data-ttu-id="28c42-104">Det här skriptet skapar en virtuell dator och använder hello Azure virtuella DSC anpassat skript tillägget tooinstall och konfigurera IIS.</span><span class="sxs-lookup"><span data-stu-id="28c42-104">This script creates a virtual machine, and uses hello Azure Virtual Machine DSC custom script extension tooinstall and configure IIS.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="28c42-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="28c42-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]

## <a name="clean-up-deployment"></a><span data-ttu-id="28c42-106">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="28c42-106">Clean up deployment</span></span> 

<span data-ttu-id="28c42-107">Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="28c42-107">Run hello following command tooremove hello resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="28c42-108">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="28c42-108">Script explanation</span></span>

<span data-ttu-id="28c42-109">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, en virtuell dator, och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="28c42-109">This script uses hello following commands toocreate a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="28c42-110">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="28c42-110">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="28c42-111">Kommando</span><span class="sxs-lookup"><span data-stu-id="28c42-111">Command</span></span> | <span data-ttu-id="28c42-112">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="28c42-112">Notes</span></span> |
|---|---|
| [<span data-ttu-id="28c42-113">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="28c42-113">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="28c42-114">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="28c42-114">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="28c42-115">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="28c42-115">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="28c42-116">Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="28c42-116">Creates hello virtual machine and connects it toohello network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="28c42-117">Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="28c42-117">This command also specifies hello virtual machine image toobe used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="28c42-118">AZ vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="28c42-118">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="28c42-119">Lägg till hello tillägget för anpassat skript toohello virtuella datorn som anropar en skriptet tooinstall IIS.</span><span class="sxs-lookup"><span data-stu-id="28c42-119">Add hello Custom Script Extension toohello virtual machine which invokes a script tooinstall IIS.</span></span> |
| [<span data-ttu-id="28c42-120">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="28c42-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="28c42-121">Skapar en network security group regeln tooallow inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="28c42-121">Creates a network security group rule tooallow inbound traffic.</span></span> <span data-ttu-id="28c42-122">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="28c42-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="28c42-123">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="28c42-123">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="28c42-124">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="28c42-124">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="28c42-125">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="28c42-125">Next steps</span></span>

<span data-ttu-id="28c42-126">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="28c42-126">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="28c42-127">Ytterligare virtuella CLI skriptexempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="28c42-127">Additional virtual machine CLI script samples can be found in hello [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
