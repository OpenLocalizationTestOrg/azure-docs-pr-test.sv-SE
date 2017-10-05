---
title: "Azure CLI Script exempel - distribuera LYKTA stacken i en Skaluppsättning för Utjämning av nätverksbelastning virtuellt Machin | Microsoft Docs"
description: "Använda tillägget för anpassat skript för att distribuera LYKTA stacken i en = belastningsutjämnade virtuella skaluppsättningen på Azure."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: allclark
manager: douge
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 04/05/2017
ms.author: allclark
ms.custom: mvc
ms.openlocfilehash: 4007e8c85c0ff24bf5030881eac666582714eae3
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="deploy-the-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a><span data-ttu-id="5bdf7-103">Distribuera LYKTA stacken i en belastningsutjämnad virtuella datorns skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="5bdf7-103">Deploy the LAMP stack in a load-balanced virtual machine scale set</span></span>

<span data-ttu-id="5bdf7-104">Det här exemplet skapar en skaluppsättning för virtuell dator och tillämpar ett tillägg som kör ett anpassat skript för att distribuera LYKTA stacken på varje virtuell dator i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-104">This example creates a virtual machine scale set and applies an extension that runs a custom script to deploy the LAMP stack on each virtual machine in the scale set.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="5bdf7-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="5bdf7-105">Sample script</span></span>

<span data-ttu-id="5bdf7-106">[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "skapa virtuella skala med LYKTA stack")]</span><span class="sxs-lookup"><span data-stu-id="5bdf7-106">[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]</span></span>

## <a name="connect"></a><span data-ttu-id="5bdf7-107">Anslut</span><span class="sxs-lookup"><span data-stu-id="5bdf7-107">Connect</span></span>

<span data-ttu-id="5bdf7-108">Använd den här koden för att se hur du ansluter till din virtuella dator och din skala anges.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-108">Use this code to see how to connect to your VMs and your scale set.</span></span>

<span data-ttu-id="5bdf7-109">[!code-azurecli[huvudsakliga](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "åtkomst till virtuella datorns skaluppsättning")]</span><span class="sxs-lookup"><span data-stu-id="5bdf7-109">[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access the virtual machine scale set")]</span></span>

## <a name="clean-up-deployment"></a><span data-ttu-id="5bdf7-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="5bdf7-110">Clean up deployment</span></span> 

<span data-ttu-id="5bdf7-111">Kör följande kommando för att ta bort resursgruppen och skaluppsättning för virtuella datorer och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-111">Run the following command to remove the resource group, the scale set and VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="5bdf7-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="5bdf7-112">Script explanation</span></span>

<span data-ttu-id="5bdf7-113">Det här skriptet använder följande kommandon för att skapa en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-113">This script uses the following commands to create a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="5bdf7-114">Varje kommando i tabellen länkar till kommandot viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-114">Each command in the table links to command specific documentation.</span></span>

| <span data-ttu-id="5bdf7-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="5bdf7-115">Command</span></span> | <span data-ttu-id="5bdf7-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="5bdf7-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="5bdf7-117">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="5bdf7-117">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="5bdf7-118">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-118">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="5bdf7-119">Skapa AZ vmss</span><span class="sxs-lookup"><span data-stu-id="5bdf7-119">az vmss create</span></span>](https://docs.microsoft.com/cli/azure/vmss#create) | <span data-ttu-id="5bdf7-120">Skapar en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5bdf7-120">Creates a virtual machine scale set</span></span> |
| [<span data-ttu-id="5bdf7-121">Skapa AZ nätverket lb regel</span><span class="sxs-lookup"><span data-stu-id="5bdf7-121">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="5bdf7-122">Lägg till en slutpunkt för Utjämning av nätverksbelastning</span><span class="sxs-lookup"><span data-stu-id="5bdf7-122">Add a load-balanced endpoint</span></span> |
| [<span data-ttu-id="5bdf7-123">AZ vmss tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="5bdf7-123">az vmss extension set</span></span>](https://docs.microsoft.com/cli/azure/vmss/extension#set) | <span data-ttu-id="5bdf7-124">Skapa tillägg som körs skriptet på distribution av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5bdf7-124">Create the extension that runs the custom script on deployment of a VM</span></span> |
| [<span data-ttu-id="5bdf7-125">AZ vmss update-instanser</span><span class="sxs-lookup"><span data-stu-id="5bdf7-125">az vmss update-instances</span></span>](https://docs.microsoft.com/cli/azure/vmss#update-instances) | <span data-ttu-id="5bdf7-126">Kör skriptet på VM-instanser som har distribuerats innan tillägget har tillämpats på skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-126">Run the custom script on the VM instances that were deployed before the extension was applied to the scale set.</span></span> |
| [<span data-ttu-id="5bdf7-127">AZ vmss skala</span><span class="sxs-lookup"><span data-stu-id="5bdf7-127">az vmss scale</span></span>](https://docs.microsoft.com/cli/azure/vmss#scale) | <span data-ttu-id="5bdf7-128">Skala upp skalan genom att lägga till flera VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-128">Scale up the scale set by adding more VM instances.</span></span> <span data-ttu-id="5bdf7-129">Anpassat skript körs på dessa när de distribueras.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-129">The custom script is run on these when they are deployed.</span></span> |
| [<span data-ttu-id="5bdf7-130">AZ nätverket offentliga ip-lista</span><span class="sxs-lookup"><span data-stu-id="5bdf7-130">az network public-ip list</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#list) | <span data-ttu-id="5bdf7-131">Hämta IP-adresserna för de virtuella datorerna som skapats av exemplet.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-131">Get the IP addresses of the VMs created by the sample.</span></span> |
| [<span data-ttu-id="5bdf7-132">AZ nätverket lb visa</span><span class="sxs-lookup"><span data-stu-id="5bdf7-132">az network lb show</span></span>](https://docs.microsoft.com/cli/azure/network/lb#show) | <span data-ttu-id="5bdf7-133">Hämta klient- och servergränssnitten portar som används av belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="5bdf7-133">Get the frontend and backend ports used by the load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="5bdf7-134">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5bdf7-134">Next steps</span></span>

<span data-ttu-id="5bdf7-135">Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="5bdf7-135">For more information on the Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="5bdf7-136">Ytterligare virtuella CLI skriptexempel finns i den [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="5bdf7-136">Additional virtual machine CLI script samples can be found in the [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
