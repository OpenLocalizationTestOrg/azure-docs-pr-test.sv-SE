---
title: "aaaAzure CLI skriptexempel - distribuera hello LYKTA Stack i en Load-Balanced virtuellt Machin Skaluppsättning | Microsoft Docs"
description: "Använd ett anpassat skript för tillägget toodeploy hello LYKTA Stack i en = belastningsutjämnade virtuella skaluppsättningen på Azure."
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
ms.openlocfilehash: d5278db809faaa0997a08b00a53387d754fce3d3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="deploy-hello-lamp-stack-in-a-load-balanced-virtual-machine-scale-set"></a><span data-ttu-id="a9c9c-103">Distribuera hello LYKTA stacken i en belastningsutjämnad virtuella datorns skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="a9c9c-103">Deploy hello LAMP stack in a load-balanced virtual machine scale set</span></span>

<span data-ttu-id="a9c9c-104">Det här exemplet skapar en skaluppsättning för virtuell dator och tillämpar ett tillägg som kör ett anpassat skript toodeploy hello LYKTA stacken på varje virtuell dator i skaluppsättning för hello.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-104">This example creates a virtual machine scale set and applies an extension that runs a custom script toodeploy hello LAMP stack on each virtual machine in hello scale set.</span></span>

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

## <a name="sample-script"></a><span data-ttu-id="a9c9c-105">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="a9c9c-105">Sample script</span></span>

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/build-stack.sh "Create virtual machine scale set with LAMP stack")]

## <a name="connect"></a><span data-ttu-id="a9c9c-106">Anslut</span><span class="sxs-lookup"><span data-stu-id="a9c9c-106">Connect</span></span>

<span data-ttu-id="a9c9c-107">Använd den här koden toosee hur tooconnect tooyour virtuella datorer och din skala in.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-107">Use this code toosee how tooconnect tooyour VMs and your scale set.</span></span>

[!code-azurecli[main](../../../cli_scripts/virtual-machine/create-scaleset-php-ansible/how-to-access.sh "Access hello virtual machine scale set")]

## <a name="clean-up-deployment"></a><span data-ttu-id="a9c9c-108">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="a9c9c-108">Clean up deployment</span></span> 

<span data-ttu-id="a9c9c-109">Kör följande kommando tooremove hello resursgrupp, hello skaluppsättning och virtuella datorer och alla relaterade resurser hello.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-109">Run hello following command tooremove hello resource group, hello scale set and VMs, and all related resources.</span></span>

```azurecli-interactive 
az group delete -n myResourceGroup
```

## <a name="script-explanation"></a><span data-ttu-id="a9c9c-110">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="a9c9c-110">Script explanation</span></span>

<span data-ttu-id="a9c9c-111">Det här skriptet använder hello följande kommandon toocreate en resursgrupp, virtuella, tillgänglighetsuppsättning, belastningsutjämnare och alla relaterade resurser.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-111">This script uses hello following commands toocreate a resource group, virtual machine, availability set, load balancer, and all related resources.</span></span> <span data-ttu-id="a9c9c-112">Varje kommando i hello tabellen länkar toocommand viss dokumentation.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-112">Each command in hello table links toocommand specific documentation.</span></span>

| <span data-ttu-id="a9c9c-113">Kommando</span><span class="sxs-lookup"><span data-stu-id="a9c9c-113">Command</span></span> | <span data-ttu-id="a9c9c-114">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="a9c9c-114">Notes</span></span> |
|---|---|
| [<span data-ttu-id="a9c9c-115">Skapa AZ grupp</span><span class="sxs-lookup"><span data-stu-id="a9c9c-115">az group create</span></span>](https://docs.microsoft.com/cli/azure/group#create) | <span data-ttu-id="a9c9c-116">Skapar en resursgrupp som är lagrade i alla resurser.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-116">Creates a resource group in which all resources are stored.</span></span> |
| [<span data-ttu-id="a9c9c-117">Skapa AZ vmss</span><span class="sxs-lookup"><span data-stu-id="a9c9c-117">az vmss create</span></span>](https://docs.microsoft.com/cli/azure/vmss#create) | <span data-ttu-id="a9c9c-118">Skapar en skaluppsättning för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a9c9c-118">Creates a virtual machine scale set</span></span> |
| [<span data-ttu-id="a9c9c-119">Skapa AZ nätverket lb regel</span><span class="sxs-lookup"><span data-stu-id="a9c9c-119">az network lb rule create</span></span>](https://docs.microsoft.com/cli/azure/network/lb/rule#create) | <span data-ttu-id="a9c9c-120">Lägg till en slutpunkt för Utjämning av nätverksbelastning</span><span class="sxs-lookup"><span data-stu-id="a9c9c-120">Add a load-balanced endpoint</span></span> |
| [<span data-ttu-id="a9c9c-121">AZ vmss tillägget uppsättningen</span><span class="sxs-lookup"><span data-stu-id="a9c9c-121">az vmss extension set</span></span>](https://docs.microsoft.com/cli/azure/vmss/extension#set) | <span data-ttu-id="a9c9c-122">Skapa hello-tillägg som kör hello anpassat skript för distribution av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="a9c9c-122">Create hello extension that runs hello custom script on deployment of a VM</span></span> |
| [<span data-ttu-id="a9c9c-123">AZ vmss update-instanser</span><span class="sxs-lookup"><span data-stu-id="a9c9c-123">az vmss update-instances</span></span>](https://docs.microsoft.com/cli/azure/vmss#update-instances) | <span data-ttu-id="a9c9c-124">Kör hello anpassat skript på hello VM-instanser som har distribuerats innan hello tillägget installerades toohello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-124">Run hello custom script on hello VM instances that were deployed before hello extension was applied toohello scale set.</span></span> |
| [<span data-ttu-id="a9c9c-125">AZ vmss skala</span><span class="sxs-lookup"><span data-stu-id="a9c9c-125">az vmss scale</span></span>](https://docs.microsoft.com/cli/azure/vmss#scale) | <span data-ttu-id="a9c9c-126">Skala upp hello skala genom att lägga till flera VM-instanser.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-126">Scale up hello scale set by adding more VM instances.</span></span> <span data-ttu-id="a9c9c-127">hello anpassade skript körs om dem när de har distribuerats.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-127">hello custom script is run on these when they are deployed.</span></span> |
| [<span data-ttu-id="a9c9c-128">AZ nätverket offentliga ip-lista</span><span class="sxs-lookup"><span data-stu-id="a9c9c-128">az network public-ip list</span></span>](https://docs.microsoft.com/cli/azure/network/public-ip#list) | <span data-ttu-id="a9c9c-129">Hämta hello IP-adresser för hello virtuella datorer som skapats av hello exempel.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-129">Get hello IP addresses of hello VMs created by hello sample.</span></span> |
| [<span data-ttu-id="a9c9c-130">AZ nätverket lb visa</span><span class="sxs-lookup"><span data-stu-id="a9c9c-130">az network lb show</span></span>](https://docs.microsoft.com/cli/azure/network/lb#show) | <span data-ttu-id="a9c9c-131">Hämta hello frontend och backend portar som används av hello belastningsutjämnaren.</span><span class="sxs-lookup"><span data-stu-id="a9c9c-131">Get hello frontend and backend ports used by hello load balancer.</span></span> |

## <a name="next-steps"></a><span data-ttu-id="a9c9c-132">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a9c9c-132">Next steps</span></span>

<span data-ttu-id="a9c9c-133">Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="a9c9c-133">For more information on hello Azure CLI, see [Azure CLI documentation](https://docs.microsoft.com/cli/azure/overview).</span></span>

<span data-ttu-id="a9c9c-134">Ytterligare virtuella CLI skriptexempel finns i hello [virtuella Azure Linux-datorn dokumentationen](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="a9c9c-134">Additional virtual machine CLI script samples can be found in hello [Azure Linux VM documentation](../linux/cli-samples.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>
