---
title: aaaCreate en kopia av din Linux VM med hello Azure CLI 1.0 | Microsoft Docs
description: "Lär dig hur toocreate en kopia av den virtuella datorn i Azure Linux med hello Azure CLI 1.0 i hello Resource Manager-distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
tags: azure-resource-manager
ms.assetid: 770569d2-23c1-4a5b-801e-cddcd1375164
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: cynthn
ms.openlocfilehash: 997a2c8109e7083ececd76fd1013e9ed4d3e6afd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-hello-azure-cli-10"></a><span data-ttu-id="cf9e2-103">Skapa en kopia av en Linux-dator som körs på Azure med hello Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="cf9e2-103">Create a copy of a Linux virtual machine running on Azure with hello Azure CLI 1.0</span></span>
<span data-ttu-id="cf9e2-104">Den här artikeln visar hur toocreate en kopia av din Azure-dator (VM) kör Linux med hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-104">This article shows you how toocreate a copy of your Azure virtual machine (VM) running Linux using hello Resource Manager deployment model.</span></span> <span data-ttu-id="cf9e2-105">Först du kopiera över hello operativsystemet och data diskar tooa ny behållare, och sedan konfigurera hello nätverksresurser och skapa hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-105">First you copy over hello operating system and data disks tooa new container, then set up hello network resources and create hello new virtual machine.</span></span>

<span data-ttu-id="cf9e2-106">Du kan också [överför och skapa en virtuell dator från anpassade diskavbildning](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="cf9e2-106">You can also [upload and create a VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="cf9e2-107">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="cf9e2-107">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="cf9e2-108">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="cf9e2-108">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="cf9e2-109">Azure CLI 1.0 – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="cf9e2-109">Azure CLI 1.0 – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="cf9e2-110">[Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="cf9e2-110">[Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="cf9e2-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="cf9e2-111">Before you begin</span></span>
<span data-ttu-id="cf9e2-112">Se till att du uppfyller följande krav innan du börjar hello åtgärderna hello:</span><span class="sxs-lookup"><span data-stu-id="cf9e2-112">Ensure that you meet hello following prerequisites before you start hello steps:</span></span>

* <span data-ttu-id="cf9e2-113">Du har hello [Azure CLI](../../cli-install-nodejs.md) hämtas och installeras på din dator.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-113">You have hello [Azure CLI](../../cli-install-nodejs.md) downloaded and installed on your machine.</span></span> 
* <span data-ttu-id="cf9e2-114">Du måste också information om dina befintliga virtuella Azure Linux-datorn:</span><span class="sxs-lookup"><span data-stu-id="cf9e2-114">You also need some information about your existing Azure Linux VM:</span></span>

| <span data-ttu-id="cf9e2-115">Information om källdatorn VM</span><span class="sxs-lookup"><span data-stu-id="cf9e2-115">Source VM information</span></span> | <span data-ttu-id="cf9e2-116">Där tooget den</span><span class="sxs-lookup"><span data-stu-id="cf9e2-116">Where tooget it</span></span> |
| --- | --- |
| <span data-ttu-id="cf9e2-117">VM-namn</span><span class="sxs-lookup"><span data-stu-id="cf9e2-117">VM name</span></span> |`azure vm list` |
| <span data-ttu-id="cf9e2-118">Resursgruppens namn</span><span class="sxs-lookup"><span data-stu-id="cf9e2-118">Resource Group name</span></span> |`azure vm list` |
| <span data-ttu-id="cf9e2-119">Plats</span><span class="sxs-lookup"><span data-stu-id="cf9e2-119">Location</span></span> |`azure vm list` |
| <span data-ttu-id="cf9e2-120">Lagringskontonamnet</span><span class="sxs-lookup"><span data-stu-id="cf9e2-120">Storage Account name</span></span> |`azure storage account list -g <resourceGroup>` |
| <span data-ttu-id="cf9e2-121">Behållarens namn</span><span class="sxs-lookup"><span data-stu-id="cf9e2-121">Container name</span></span> |`azure storage container list -a <sourcestorageaccountname>` |
| <span data-ttu-id="cf9e2-122">Namn på datakälla VM VHD-filen</span><span class="sxs-lookup"><span data-stu-id="cf9e2-122">Source VM VHD file name</span></span> |`azure storage blob list --container <containerName>` |

* <span data-ttu-id="cf9e2-123">Du behöver toomake vissa alternativ om din nya VM:   </span><span class="sxs-lookup"><span data-stu-id="cf9e2-123">You will need toomake some choices about your new VM:    </span></span><br> <span data-ttu-id="cf9e2-124">-Behållarnamn   </span><span class="sxs-lookup"><span data-stu-id="cf9e2-124">-Container name    </span></span><br> <span data-ttu-id="cf9e2-125">Namn på virtuell dator-   </span><span class="sxs-lookup"><span data-stu-id="cf9e2-125">-VM name    </span></span><br> <span data-ttu-id="cf9e2-126">VM - storlek   </span><span class="sxs-lookup"><span data-stu-id="cf9e2-126">-VM size    </span></span><br> <span data-ttu-id="cf9e2-127">vNet - namn   </span><span class="sxs-lookup"><span data-stu-id="cf9e2-127">-vNet name    </span></span><br> <span data-ttu-id="cf9e2-128">-Undernätsnamn   </span><span class="sxs-lookup"><span data-stu-id="cf9e2-128">-SubNet name    </span></span><br> <span data-ttu-id="cf9e2-129">-IP-namn   </span><span class="sxs-lookup"><span data-stu-id="cf9e2-129">-IP Name    </span></span><br> <span data-ttu-id="cf9e2-130">NIC - namn</span><span class="sxs-lookup"><span data-stu-id="cf9e2-130">-NIC name</span></span>

## <a name="login-and-set-your-subscription"></a><span data-ttu-id="cf9e2-131">Logga in och ange din prenumeration</span><span class="sxs-lookup"><span data-stu-id="cf9e2-131">Login and set your subscription</span></span>
1. <span data-ttu-id="cf9e2-132">Inloggningen toohello CLI.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-132">Login toohello CLI.</span></span>

    ```azurecli
    azure login
    ```
2. <span data-ttu-id="cf9e2-133">Kontrollera att du är i läget Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-133">Make sure you are in Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="cf9e2-134">Ange hello korrekt prenumeration.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-134">Set hello correct subscription.</span></span> <span data-ttu-id="cf9e2-135">Du kan använda 'list azure-konto' toosee alla dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-135">You can use 'azure account list' toosee all of your subscriptions.</span></span>

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-hello-vm"></a><span data-ttu-id="cf9e2-136">Stoppa hello VM</span><span class="sxs-lookup"><span data-stu-id="cf9e2-136">Stop hello VM</span></span>
<span data-ttu-id="cf9e2-137">Stoppa och frigöra Virtuella hello källdatorn.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-137">Stop and deallocate hello source VM.</span></span> <span data-ttu-id="cf9e2-138">Du kan använda 'azure vm list' tooget en lista över alla hello virtuella datorer i din prenumeration och deras resursgruppnamn.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-138">You can use 'azure vm list' tooget a list of all of hello VMs in your subscription and their resource group names.</span></span>

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-hello-vhd"></a><span data-ttu-id="cf9e2-139">Kopiera hello VHD</span><span class="sxs-lookup"><span data-stu-id="cf9e2-139">Copy hello VHD</span></span>
<span data-ttu-id="cf9e2-140">Du kan kopiera hello VHD från hello källa toohello lagringsplats med hello `azure storage blob copy start`.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-140">You can copy hello VHD from hello source storage toohello destination using hello `azure storage blob copy start`.</span></span> <span data-ttu-id="cf9e2-141">I det här exemplet ska vi toocopy hello VHD toohello samma lagringskonto, men en annan behållare.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-141">In this example, we are going toocopy hello VHD toohello same storage account, but a different container.</span></span>

<span data-ttu-id="cf9e2-142">toocopy hello VHD tooanother behållare i hello samma lagringskonto, typ:</span><span class="sxs-lookup"><span data-stu-id="cf9e2-142">toocopy hello VHD tooanother container in hello same storage account, type:</span></span>

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-hello-virtual-network-for-your-new-vm"></a><span data-ttu-id="cf9e2-143">Konfigurera hello virtuellt nätverk för den nya virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="cf9e2-143">Set up hello virtual network for your new VM</span></span>
<span data-ttu-id="cf9e2-144">Ange ett virtuellt nätverk och nätverkskort för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="cf9e2-144">Set up a virtual network and NIC for your new VM.</span></span> 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-hello-new-vm"></a><span data-ttu-id="cf9e2-145">Skapa hello ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="cf9e2-145">Create hello new VM</span></span>
<span data-ttu-id="cf9e2-146">Nu kan du skapa en virtuell dator från den överförda virtuella hårddisken [med hjälp av en resource manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) eller via hello kopieras CLI genom att ange hello URI tooyour disk genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="cf9e2-146">You can now create a VM from your uploaded virtual disk [using a resource manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) or through hello CLI by specifying hello URI tooyour copied disk by typing:</span></span>

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a><span data-ttu-id="cf9e2-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cf9e2-147">Next steps</span></span>
<span data-ttu-id="cf9e2-148">toolearn hur toouse Azure CLI toomanage din nya virtuella datorn finns [Azure CLI-kommandona för hello Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="cf9e2-148">toolearn how toouse Azure CLI toomanage your new virtual machine, see [Azure CLI commands for hello Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>

