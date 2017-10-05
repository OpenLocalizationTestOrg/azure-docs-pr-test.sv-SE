---
title: Azure CLI Script exempel - skapa en virtuell Linux-dator med WordPress | Microsoft Docs
description: Azure CLI Script exempel - skapa en virtuell Linux-dator med WordPress
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
ms.openlocfilehash: cc95a190b58cb208ac0b642fc9dc2253e993ca23
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-vm-with-wordpress"></a><span data-ttu-id="8a53e-103">Skapa en virtuell dator med WordPress</span><span class="sxs-lookup"><span data-stu-id="8a53e-103">Create a VM with WordPress</span></span>

<span data-ttu-id="8a53e-104">Det här skriptet skapar en virtuell dator och använder sedan tillägget för anpassat skript för Azure virtuell dator för att installera WordPress.</span><span class="sxs-lookup"><span data-stu-id="8a53e-104">This script creates a virtual machine, and then uses the Azure Virtual Machine custom script extension to install WordPress.</span></span> <span data-ttu-id="8a53e-105">När skriptet har körts, du kan komma åt webbplatsen WordPress konfiguration på `http://<public IP of VM>/wordpress`.</span><span class="sxs-lookup"><span data-stu-id="8a53e-105">After running the script, you can access the WordPress configuration site at  `http://<public IP of VM>/wordpress`.</span></span> 

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a><span data-ttu-id="8a53e-106">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="8a53e-106">Sample script</span></span>

<span data-ttu-id="8a53e-107">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "snabbt skapa virtuell dator")]</span><span class="sxs-lookup"><span data-stu-id="8a53e-107">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-wordpress-mysql/create-wordpress-mysql.sh "Quick Create VM")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="8a53e-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="8a53e-108">Clean up deployment</span></span> 

<span data-ttu-id="8a53e-109">Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8a53e-109">Run the following command to remove the resource group, VM, and all related resources.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="8a53e-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="8a53e-110">Script explanation</span></span>

<span data-ttu-id="8a53e-111">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuell dator och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="8a53e-111">This script uses the following commands to create a resource group, virtual machine, and all related resources.</span></span> <span data-ttu-id="8a53e-112">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="8a53e-112">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="8a53e-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="8a53e-113">Command</span></span> | <span data-ttu-id="8a53e-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="8a53e-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="8a53e-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="8a53e-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="8a53e-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="8a53e-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="8a53e-117">Skapa AZ vm</span><span class="sxs-lookup"><span data-stu-id="8a53e-117">az vm create</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="8a53e-118">Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG.</span><span class="sxs-lookup"><span data-stu-id="8a53e-118">Creates the virtual machine and connects it to the network card, virtual network, subnet, and NSG.</span></span> <span data-ttu-id="8a53e-119">Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="8a53e-119">This command also specifies the virtual machine image to be used, and administrative credentials.</span></span>  |
| [<span data-ttu-id="8a53e-120">AZ vm öppna-port</span><span class="sxs-lookup"><span data-stu-id="8a53e-120">az vm open-port</span></span>](https://docs.microsoft.com/cli/azure/vm#open-port) | <span data-ttu-id="8a53e-121">Skapar en grupp nätverkssäkerhetsregeln för att tillåta inkommande trafik.</span><span class="sxs-lookup"><span data-stu-id="8a53e-121">Creates a network security group rule to allow inbound traffic.</span></span> <span data-ttu-id="8a53e-122">I det här exemplet används port 80 för HTTP-trafik.</span><span class="sxs-lookup"><span data-stu-id="8a53e-122">In this sample, port 80 is opened for HTTP traffic.</span></span> |
| [<span data-ttu-id="8a53e-123">AZ vm-tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="8a53e-123">az vm extension set</span></span>](https://docs.microsoft.com/cli/azure/vm#create) | <span data-ttu-id="8a53e-124">Lägg till tillägget för anpassat skript till den virtuella datorn som anropar ett skript för att installera WordPress.</span><span class="sxs-lookup"><span data-stu-id="8a53e-124">Add the Custom Script Extension to the virtual machine, which invokes a script to install WordPress.</span></span> |
| [<span data-ttu-id="8a53e-125">ta bort grupp AZ</span><span class="sxs-lookup"><span data-stu-id="8a53e-125">az group delete</span></span>](https://docs.microsoft.com/cli/azure/vm/extension#set) | <span data-ttu-id="8a53e-126">Tar bort en resursgrupp, inklusive alla kapslade resurser.</span><span class="sxs-lookup"><span data-stu-id="8a53e-126">Deletes a resource group including all nested resources.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="8a53e-127">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8a53e-127">Next steps</span></span>

<span data-ttu-id="8a53e-128">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="8a53e-128">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="8a53e-129">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="8a53e-129">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
