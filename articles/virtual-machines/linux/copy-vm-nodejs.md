---
title: Skapa en kopia av Linux-VM med Azure CLI 1.0 | Microsoft Docs
description: "Lär dig hur du skapar en kopia av den virtuella Azure Linux-datorn med Azure CLI 1.0 i Resource Manager-distributionsmodellen"
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
ms.openlocfilehash: 62ae54f3596c9383cbf3b401fcfdb42ecfdee63c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-copy-of-a-linux-virtual-machine-running-on-azure-with-the-azure-cli-10"></a><span data-ttu-id="b2cc6-103">Skapa en kopia av en Linux-dator som körs på Azure med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b2cc6-103">Create a copy of a Linux virtual machine running on Azure with the Azure CLI 1.0</span></span>
<span data-ttu-id="b2cc6-104">Den här artikeln visar hur du skapar en kopia av din Azure virtuell dator (VM kör Linux med hjälp av Resource Manager-distributionsmodellen).</span><span class="sxs-lookup"><span data-stu-id="b2cc6-104">This article shows you how to create a copy of your Azure virtual machine (VM) running Linux using the Resource Manager deployment model.</span></span> <span data-ttu-id="b2cc6-105">Först du kopiera över operativsystemet och datadiskar till en ny behållare och sedan konfigurera nätverksresurserna och skapa den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-105">First you copy over the operating system and data disks to a new container, then set up the network resources and create the new virtual machine.</span></span>

<span data-ttu-id="b2cc6-106">Du kan också [överför och skapa en virtuell dator från anpassade diskavbildning](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="b2cc6-106">You can also [upload and create a VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="b2cc6-107">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="b2cc6-107">CLI versions to complete the task</span></span>
<span data-ttu-id="b2cc6-108">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="b2cc6-108">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="b2cc6-109">Azure CLI 1.0 – vårt CLI för den klassiska distributionsmodellen och Resource Manager-distributionsmodellen (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="b2cc6-109">Azure CLI 1.0 – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b2cc6-110">[Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="b2cc6-110">[Azure CLI 2.0](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="b2cc6-111">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="b2cc6-111">Before you begin</span></span>
<span data-ttu-id="b2cc6-112">Se till att du uppfyller följande krav innan du startar stegen:</span><span class="sxs-lookup"><span data-stu-id="b2cc6-112">Ensure that you meet the following prerequisites before you start the steps:</span></span>

* <span data-ttu-id="b2cc6-113">Du har den [Azure CLI](../../cli-install-nodejs.md) hämtas och installeras på din dator.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-113">You have the [Azure CLI](../../cli-install-nodejs.md) downloaded and installed on your machine.</span></span> 
* <span data-ttu-id="b2cc6-114">Du måste också information om dina befintliga virtuella Azure Linux-datorn:</span><span class="sxs-lookup"><span data-stu-id="b2cc6-114">You also need some information about your existing Azure Linux VM:</span></span>

| <span data-ttu-id="b2cc6-115">Information om källdatorn VM</span><span class="sxs-lookup"><span data-stu-id="b2cc6-115">Source VM information</span></span> | <span data-ttu-id="b2cc6-116">Hämta den</span><span class="sxs-lookup"><span data-stu-id="b2cc6-116">Where to get it</span></span> |
| --- | --- |
| <span data-ttu-id="b2cc6-117">VM-namn</span><span class="sxs-lookup"><span data-stu-id="b2cc6-117">VM name</span></span> |`azure vm list` |
| <span data-ttu-id="b2cc6-118">Resursgruppens namn</span><span class="sxs-lookup"><span data-stu-id="b2cc6-118">Resource Group name</span></span> |`azure vm list` |
| <span data-ttu-id="b2cc6-119">Plats</span><span class="sxs-lookup"><span data-stu-id="b2cc6-119">Location</span></span> |`azure vm list` |
| <span data-ttu-id="b2cc6-120">Lagringskontonamnet</span><span class="sxs-lookup"><span data-stu-id="b2cc6-120">Storage Account name</span></span> |`azure storage account list -g <resourceGroup>` |
| <span data-ttu-id="b2cc6-121">Behållarens namn</span><span class="sxs-lookup"><span data-stu-id="b2cc6-121">Container name</span></span> |`azure storage container list -a <sourcestorageaccountname>` |
| <span data-ttu-id="b2cc6-122">Namn på datakälla VM VHD-filen</span><span class="sxs-lookup"><span data-stu-id="b2cc6-122">Source VM VHD file name</span></span> |`azure storage blob list --container <containerName>` |

* <span data-ttu-id="b2cc6-123">Du behöver göra vissa val om den nya virtuella datorn:   </span><span class="sxs-lookup"><span data-stu-id="b2cc6-123">You will need to make some choices about your new VM:    </span></span><br> <span data-ttu-id="b2cc6-124">-Behållarnamn   </span><span class="sxs-lookup"><span data-stu-id="b2cc6-124">-Container name    </span></span><br> <span data-ttu-id="b2cc6-125">Namn på virtuell dator-   </span><span class="sxs-lookup"><span data-stu-id="b2cc6-125">-VM name    </span></span><br> <span data-ttu-id="b2cc6-126">VM - storlek   </span><span class="sxs-lookup"><span data-stu-id="b2cc6-126">-VM size    </span></span><br> <span data-ttu-id="b2cc6-127">vNet - namn   </span><span class="sxs-lookup"><span data-stu-id="b2cc6-127">-vNet name    </span></span><br> <span data-ttu-id="b2cc6-128">-Undernätsnamn   </span><span class="sxs-lookup"><span data-stu-id="b2cc6-128">-SubNet name    </span></span><br> <span data-ttu-id="b2cc6-129">-IP-namn   </span><span class="sxs-lookup"><span data-stu-id="b2cc6-129">-IP Name    </span></span><br> <span data-ttu-id="b2cc6-130">NIC - namn</span><span class="sxs-lookup"><span data-stu-id="b2cc6-130">-NIC name</span></span>

## <a name="login-and-set-your-subscription"></a><span data-ttu-id="b2cc6-131">Logga in och ange din prenumeration</span><span class="sxs-lookup"><span data-stu-id="b2cc6-131">Login and set your subscription</span></span>
1. <span data-ttu-id="b2cc6-132">Logga in CLI.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-132">Login to the CLI.</span></span>

    ```azurecli
    azure login
    ```
2. <span data-ttu-id="b2cc6-133">Kontrollera att du är i läget Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-133">Make sure you are in Resource Manager mode.</span></span>

    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="b2cc6-134">Ange korrekt prenumeration.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-134">Set the correct subscription.</span></span> <span data-ttu-id="b2cc6-135">Du kan använda ”azure-konto ' att se alla dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-135">You can use 'azure account list' to see all of your subscriptions.</span></span>

    ```azurecli
    azure account set mySubscriptionID
    ```

## <a name="stop-the-vm"></a><span data-ttu-id="b2cc6-136">Stoppa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b2cc6-136">Stop the VM</span></span>
<span data-ttu-id="b2cc6-137">Stoppa och ta bort den Virtuella källdatorn.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-137">Stop and deallocate the source VM.</span></span> <span data-ttu-id="b2cc6-138">Du kan använda 'azure vm list' och hämta en lista över alla de virtuella datorerna i din prenumeration och resursen gruppnamn.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-138">You can use 'azure vm list' to get a list of all of the VMs in your subscription and their resource group names.</span></span>

```azurecli
azure vm stop myResourceGroup myVM
azure vm deallocate myResourceGroup MyVM
```


## <a name="copy-the-vhd"></a><span data-ttu-id="b2cc6-139">Kopiera den virtuella Hårddisken</span><span class="sxs-lookup"><span data-stu-id="b2cc6-139">Copy the VHD</span></span>
<span data-ttu-id="b2cc6-140">Du kan kopiera den virtuella Hårddisken från källservern. lagring till målet med den `azure storage blob copy start`.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-140">You can copy the VHD from the source storage to the destination using the `azure storage blob copy start`.</span></span> <span data-ttu-id="b2cc6-141">I det här exemplet det dags att kopiera den virtuella Hårddisken till samma lagringskonto, men en annan behållare.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-141">In this example, we are going to copy the VHD to the same storage account, but a different container.</span></span>

<span data-ttu-id="b2cc6-142">Om du vill kopiera den virtuella Hårddisken till en annan behållare i samma lagringskonto, skriver du:</span><span class="sxs-lookup"><span data-stu-id="b2cc6-142">To copy the VHD to another container in the same storage account, type:</span></span>

```azurecli
azure storage blob copy start \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd \
        myNewContainerName
```

## <a name="set-up-the-virtual-network-for-your-new-vm"></a><span data-ttu-id="b2cc6-143">Konfigurera det virtuella nätverket för den nya virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="b2cc6-143">Set up the virtual network for your new VM</span></span>
<span data-ttu-id="b2cc6-144">Ange ett virtuellt nätverk och nätverkskort för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b2cc6-144">Set up a virtual network and NIC for your new VM.</span></span> 

```azurecli
azure network vnet create myResourceGroup myVnet -l myLocation

azure network vnet subnet create -a <address.prefix.in.CIDR/format> myResourceGroup myVnet mySubnet

azure network public-ip create myResourceGroup myPublicIP -l myLocation

azure network nic create myResourceGroup myNic -k mySubnet -m myVnet -p myPublicIP -l myLocation
```


## <a name="create-the-new-vm"></a><span data-ttu-id="b2cc6-145">Skapa ny virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b2cc6-145">Create the new VM</span></span>
<span data-ttu-id="b2cc6-146">Nu kan du skapa en virtuell dator från den överförda virtuella hårddisken [med hjälp av en resource manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) eller via CLI genom att ange URI: N till den kopierade disken genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="b2cc6-146">You can now create a VM from your uploaded virtual disk [using a resource manager template](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd) or through the CLI by specifying the URI to your copied disk by typing:</span></span>

```azurecli
azure vm create -n myVM -l myLocation -g myResourceGroup -f myNic \
        -z Standard_DS1_v2 -y Linux \
        https://mystorageaccountname.blob.core.windows.net:8080/mycontainername/myVHD.vhd 
```



## <a name="next-steps"></a><span data-ttu-id="b2cc6-147">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b2cc6-147">Next steps</span></span>
<span data-ttu-id="b2cc6-148">Information om hur du använder Azure CLI för att hantera den nya virtuella datorn finns [Azure CLI-kommandona för Azure Resource Manager](../azure-cli-arm-commands.md).</span><span class="sxs-lookup"><span data-stu-id="b2cc6-148">To learn how to use Azure CLI to manage your new virtual machine, see [Azure CLI commands for the Azure Resource Manager](../azure-cli-arm-commands.md).</span></span>

