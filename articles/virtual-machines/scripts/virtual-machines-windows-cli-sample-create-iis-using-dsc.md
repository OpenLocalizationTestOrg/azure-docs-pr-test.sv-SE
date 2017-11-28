---
title: "Azure CLI-skript exempel – skapa en virtuell Windows Server 2016-dator med IIS med DSC | Microsoft Docs"
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
ms.openlocfilehash: 1f605c0260fcaea29851d76c90ba0b287bec5ae7
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-iis-using-dsc"></a><span data-ttu-id="3315c-103">Skapa en virtuell dator med IIS använder DSC</span><span class="sxs-lookup"><span data-stu-id="3315c-103">Create a VM with IIS using DSC</span></span>

<span data-ttu-id="3315c-104">Det här skriptet skapar en virtuell dator och använder tillägget för Azure Virtual Machine DSC-anpassat skript för att installera och konfigurera IIS.</span><span class="sxs-lookup"><span data-stu-id="3315c-104">This script creates a virtual machine, and uses the Azure Virtual Machine DSC custom script extension to install and configure IIS.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="3315c-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="3315c-105">Sample script</span></span>

<span data-ttu-id="3315c-106">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "snabbt skapa virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="3315c-106">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-windows-iis-using-dsc/create-windows-iis-using-dsc.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="3315c-107">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="3315c-107">Clean up deployment</span></span> 

<span data-ttu-id="3315c-108">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="3315c-108">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup --yes
```

## <a name="script-explanation"></a><span data-ttu-id="3315c-109">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="3315c-109">Script explanation</span></span>

<span data-ttu-id="3315c-110">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="3315c-110">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="3315c-111">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="3315c-111">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="3315c-112">Kommando</span><span class="sxs-lookup"><span data-stu-id="3315c-112">Command</span></span> | <span data-ttu-id="3315c-113">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="3315c-113">Notes</span></span> |
|---|---|
| [<span data-ttu-id="3315c-114">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="3315c-114">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="3315c-115">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="3315c-115">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="3315c-116">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="3315c-116">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="3315c-117">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="3315c-117">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="3315c-118">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="3315c-118">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="3315c-119">AZ vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="3315c-119">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="3315c-120">Lägg till tillägget för anpassat skript till den virtuella datorn som anropar ett skript för att installera IIS.</span><span class="sxs-lookup"><span data-stu-id="3315c-120">Add the Custom Script Extension to the virtual machine which invokes a script to install IIS.</span></span> |
| [<span data-ttu-id="3315c-121">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="3315c-121">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="3315c-122">Skapar en grupp nätverkssäkerhetsregeln för att tillåta inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="3315c-122">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="3315c-123">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="3315c-123">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="3315c-124">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="3315c-124">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="3315c-125">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="3315c-125">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="3315c-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3315c-126">Next steps</span></span>

<span data-ttu-id="3315c-127">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="3315c-127">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="3315c-128">Ytterligare virtuella CLI skriptexempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="3315c-128">Additional virtual machine CLI script samples can be found in the [Azure Windows VM documentation](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).</span></span>
