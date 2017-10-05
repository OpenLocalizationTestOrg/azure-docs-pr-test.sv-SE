---
title: Skapa och hantera virtuella Linux-datorer med Azure CLI | Microsoft Docs
description: "Självstudiekurs – skapa och hantera virtuella Linux-datorer med Azure CLI"
services: virtual-machines-linux
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 05/02/2017
ms.author: nepeters
ms.custom: mvc
ms.openlocfilehash: c163c715eb1438a0d6b0ab53cbb43816ca8dbbb4
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="create-and-manage-linux-vms-with-the-azure-cli"></a><span data-ttu-id="e5a7f-103">Skapa och hantera virtuella Linux-datorer med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e5a7f-103">Create and Manage Linux VMs with the Azure CLI</span></span>

<span data-ttu-id="e5a7f-104">Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="e5a7f-105">Den här kursen ingår grundläggande virtuella Azure-datorn distribution objekt, till exempel välja en VM-storlek, välja en VM-avbildning och distribuera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="e5a7f-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="e5a7f-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e5a7f-107">Skapa och ansluta till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-107">Create and connect to a VM</span></span>
> * <span data-ttu-id="e5a7f-108">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-108">Select and use VM images</span></span>
> * <span data-ttu-id="e5a7f-109">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="e5a7f-110">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-110">Resize a VM</span></span>
> * <span data-ttu-id="e5a7f-111">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-111">View and understand VM state</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e5a7f-112">Om du väljer att installera och använda CLI lokalt kursen krävs att du använder Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-112">If you choose to install and use the CLI locally, this tutorial requires that you are running the Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="e5a7f-113">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-113">Run `az --version` to find the version.</span></span> <span data-ttu-id="e5a7f-114">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e5a7f-114">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-resource-group"></a><span data-ttu-id="e5a7f-115">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e5a7f-115">Create resource group</span></span>

<span data-ttu-id="e5a7f-116">Skapa en resursgrupp med kommandot [az group create](https://docs.microsoft.com/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="e5a7f-116">Create a resource group with the [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="e5a7f-117">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="e5a7f-118">En resursgrupp måste skapas innan en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="e5a7f-119">I det här exemplet en resursgrupp med namnet *myResourceGroupVM* skapas i den *eastus* region.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-119">In this example, a resource group named *myResourceGroupVM* is created in the *eastus* region.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

<span data-ttu-id="e5a7f-120">Resursgruppen har angetts när du skapar eller ändrar en VM som kan ses i hela den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-120">The resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="e5a7f-121">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-121">Create virtual machine</span></span>

<span data-ttu-id="e5a7f-122">Skapa en virtuell dator med den [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-122">Create a virtual machine with the [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="e5a7f-123">När du skapar en virtuell dator, är flera alternativ tillgängliga, till exempel operativsystemavbildningen, storlek och administrativa autentiseringsuppgifter för disken.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-123">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="e5a7f-124">I det här exemplet skapas en virtuell dator med namnet *myVM* kör Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-124">In this example, a virtual machine is created with a name of *myVM* running Ubuntu Server.</span></span> 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="e5a7f-125">När den virtuella datorn har skapats, Azure CLI matar ut information om den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-125">Once the VM has been created, the Azure CLI outputs information about the VM.</span></span> <span data-ttu-id="e5a7f-126">Anteckna den `publicIpAddress`, den här adressen kan användas för åtkomst till den virtuella datorn...</span><span class="sxs-lookup"><span data-stu-id="e5a7f-126">Take note of the `publicIpAddress`, this address can be used to access the virtual machine..</span></span> 

```azurecli-interactive 
{
  "fqdns": "",
  "id": "/subscriptions/d5b9d4b7-6fc1-0000-0000-000000000000/resourceGroups/myResourceGroupVM/providers/Microsoft.Compute/virtualMachines/myVM",
  "location": "eastus",
  "macAddress": "00-0D-3A-23-9A-49",
  "powerState": "VM running",
  "privateIpAddress": "10.0.0.4",
  "publicIpAddress": "52.174.34.95",
  "resourceGroup": "myResourceGroupVM"
}
```

## <a name="connect-to-vm"></a><span data-ttu-id="e5a7f-127">Ansluta till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-127">Connect to VM</span></span>

<span data-ttu-id="e5a7f-128">Nu kan du ansluta till den virtuella datorn via SSH.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-128">You can now connect to the VM using SSH.</span></span> <span data-ttu-id="e5a7f-129">Ersätt den IP-adressen med den `publicIpAddress` anges i föregående steg.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-129">Replace the example IP address with the `publicIpAddress` noted in the previous step.</span></span>

```bash
ssh 52.174.34.95
```

<span data-ttu-id="e5a7f-130">Stäng SSH-session när du är klar med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-130">Once finished with the VM, close the SSH session.</span></span> 

```bash
exit
```

## <a name="understand-vm-images"></a><span data-ttu-id="e5a7f-131">Förstå VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-131">Understand VM images</span></span>

<span data-ttu-id="e5a7f-132">Azure marketplace innehåller många avbildningar som kan användas för att skapa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-132">The Azure marketplace includes many images that can be used to create VMs.</span></span> <span data-ttu-id="e5a7f-133">I de föregående stegen skapades en virtuell dator med en Ubuntu-bild.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-133">In the previous steps, a virtual machine was created using an Ubuntu image.</span></span> <span data-ttu-id="e5a7f-134">I det här steget används Azure CLI för att söka marketplace för en avbildning av CentOS, som sedan används för att distribuera en andra virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-134">In this step, the Azure CLI is used to search the marketplace for a CentOS image, which is then used to deploy a second virtual machine.</span></span>  

<span data-ttu-id="e5a7f-135">Om du vill se en lista över de vanligaste bilder, den [az vm bildlista](/cli/azure/vm/image#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-135">To see a list of the most commonly used images, use the [az vm image list](/cli/azure/vm/image#list) command.</span></span>

```azurecli-interactive 
az vm image list --output table
```

<span data-ttu-id="e5a7f-136">Kommandoutdata returnerar de mest populära VM-avbildningarna i Azure.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-136">The command output returns the most popular VM images on Azure.</span></span>

```bash
Offer          Publisher               Sku                 Urn                                                             UrnAlias             Version
-------------  ----------------------  ------------------  --------------------------------------------------------------  -------------------  ---------
WindowsServer  MicrosoftWindowsServer  2016-Datacenter     MicrosoftWindowsServer:WindowsServer:2016-Datacenter:latest     Win2016Datacenter    latest
WindowsServer  MicrosoftWindowsServer  2012-R2-Datacenter  MicrosoftWindowsServer:WindowsServer:2012-R2-Datacenter:latest  Win2012R2Datacenter  latest
WindowsServer  MicrosoftWindowsServer  2008-R2-SP1         MicrosoftWindowsServer:WindowsServer:2008-R2-SP1:latest         Win2008R2SP1         latest
WindowsServer  MicrosoftWindowsServer  2012-Datacenter     MicrosoftWindowsServer:WindowsServer:2012-Datacenter:latest     Win2012Datacenter    latest
UbuntuServer   Canonical               16.04-LTS           Canonical:UbuntuServer:16.04-LTS:latest                         UbuntuLTS            latest
CentOS         OpenLogic               7.3                 OpenLogic:CentOS:7.3:latest                                     CentOS               latest
openSUSE-Leap  SUSE                    42.2                SUSE:openSUSE-Leap:42.2:latest                                  openSUSE-Leap        latest
RHEL           RedHat                  7.3                 RedHat:RHEL:7.3:latest                                          RHEL                 latest
SLES           SUSE                    12-SP2              SUSE:SLES:12-SP2:latest                                         SLES                 latest
Debian         credativ                8                   credativ:Debian:8:latest                                        Debian               latest
CoreOS         CoreOS                  Stable              CoreOS:CoreOS:Stable:latest                                     CoreOS               latest
```

<span data-ttu-id="e5a7f-137">En fullständig lista kan visas genom att lägga till den `--all` argumentet.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-137">A full list can be seen by adding the `--all` argument.</span></span> <span data-ttu-id="e5a7f-138">Bildlistan kan också filtreras efter `--publisher` eller `–-offer`.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-138">The image list can also be filtered by `--publisher` or `–-offer`.</span></span> <span data-ttu-id="e5a7f-139">I det här exemplet listan filtreras för alla bilder med ett erbjudande som matchar *CentOS*.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-139">In this example, the list is filtered for all images with an offer that matches *CentOS*.</span></span> 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

<span data-ttu-id="e5a7f-140">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="e5a7f-140">Partial output:</span></span>

```azurecli-interactive 
Offer             Publisher         Sku   Urn                                     Version
----------------  ----------------  ----  --------------------------------------  -----------
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201501         6.5.201501
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201503         6.5.201503
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.201506         6.5.201506
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20150904       6.5.20150904
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20160309       6.5.20160309
CentOS            OpenLogic         6.5   OpenLogic:CentOS:6.5:6.5.20170207       6.5.20170207
```

<span data-ttu-id="e5a7f-141">Om du vill distribuera en virtuell dator med hjälp av en viss bild, anteckna värdet i den *Urn* kolumn.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-141">To deploy a VM using a specific image, take note of the value in the *Urn* column.</span></span> <span data-ttu-id="e5a7f-142">När du anger bilden, kan det bild versionsnumret ersättas med ”senaste”, som väljs den senaste versionen av distributionen.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-142">When specifying the image, the image version number can be replaced with “latest”, which selects the latest version of the distribution.</span></span> <span data-ttu-id="e5a7f-143">I detta exempel på `--image` argument används för att ange den senaste versionen av en CentOS 6.5-bild.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-143">In this example, the `--image` argument is used to specify the latest version of a CentOS 6.5 image.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="e5a7f-144">Förstå VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-144">Understand VM sizes</span></span>

<span data-ttu-id="e5a7f-145">Storlek på en virtuell dator bestämmer hur mycket av beräkningsresurser som Processorn och GPU-minne som är tillgängliga för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-145">A virtual machine size determines the amount of compute resources such as CPU, GPU, and memory that are made available to the virtual machine.</span></span> <span data-ttu-id="e5a7f-146">Virtuella datorer måste vara korrekt storlek för den förväntade belastningen.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-146">Virtual machines need to be sized appropriately for the expected work load.</span></span> <span data-ttu-id="e5a7f-147">Om belastningen ökar, kan en befintlig virtuell dator ändras.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-147">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="e5a7f-148">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-148">VM Sizes</span></span>

<span data-ttu-id="e5a7f-149">I följande tabell kategoriserar storlekar i användningsfall.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-149">The following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="e5a7f-150">Typ</span><span class="sxs-lookup"><span data-stu-id="e5a7f-150">Type</span></span>                     | <span data-ttu-id="e5a7f-151">Storlekar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-151">Sizes</span></span>           |    <span data-ttu-id="e5a7f-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5a7f-152">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="e5a7f-153">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="e5a7f-153">General purpose</span></span>](sizes-general.md)         |<span data-ttu-id="e5a7f-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="e5a7f-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="e5a7f-155">Belastningsutjämnade CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-155">Balanced CPU-to-memory.</span></span> <span data-ttu-id="e5a7f-156">Idealiskt för dev / test och små till medelstora lösningar för program och data.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-156">Ideal for dev / test and small to medium applications and data solutions.</span></span>  |
| [<span data-ttu-id="e5a7f-157">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="e5a7f-157">Compute optimized</span></span>](sizes-compute.md)   | <span data-ttu-id="e5a7f-158">FS, F</span><span class="sxs-lookup"><span data-stu-id="e5a7f-158">Fs, F</span></span>             | <span data-ttu-id="e5a7f-159">Hög CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-159">High CPU-to-memory.</span></span> <span data-ttu-id="e5a7f-160">Bra för medelhög trafik program, nätverksinstallationer och batchprocesser.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-160">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| [<span data-ttu-id="e5a7f-161">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="e5a7f-161">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)    | <span data-ttu-id="e5a7f-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="e5a7f-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="e5a7f-163">Hög minne-till-core.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-163">High memory-to-core.</span></span> <span data-ttu-id="e5a7f-164">Perfekt för relationsdatabaser, medelstora till stora cacheminnen och analyser i minnet.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-164">Great for relational databases, medium to large caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="e5a7f-165">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="e5a7f-165">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)      | <span data-ttu-id="e5a7f-166">Ls</span><span class="sxs-lookup"><span data-stu-id="e5a7f-166">Ls</span></span>                | <span data-ttu-id="e5a7f-167">Högt diskgenomflöde och I/O.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-167">High disk throughput and IO.</span></span> <span data-ttu-id="e5a7f-168">Perfekt för stordata, SQL- och NoSQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-168">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="e5a7f-169">GPU</span><span class="sxs-lookup"><span data-stu-id="e5a7f-169">GPU</span></span>](sizes-gpu.md)          | <span data-ttu-id="e5a7f-170">NV NC</span><span class="sxs-lookup"><span data-stu-id="e5a7f-170">NV, NC</span></span>            | <span data-ttu-id="e5a7f-171">Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-171">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| [<span data-ttu-id="e5a7f-172">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="e5a7f-172">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="e5a7f-173">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="e5a7f-173">H, A8-11</span></span>          | <span data-ttu-id="e5a7f-174">Våra mest kraftfulla CPU virtuella datorer med valfritt hög genomströmning nätverksgränssnitt (RDMA).</span><span class="sxs-lookup"><span data-stu-id="e5a7f-174">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="e5a7f-175">Hitta tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="e5a7f-175">Find available VM sizes</span></span>

<span data-ttu-id="e5a7f-176">Om du vill se en lista över storlekar på VM tillgängliga i en viss region, Använd den [az lista-storlekar på vm](/cli/azure/vm#list-sizes) kommando.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-176">To see a list of VM sizes available in a particular region, use the [az vm list-sizes](/cli/azure/vm#list-sizes) command.</span></span> 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

<span data-ttu-id="e5a7f-177">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="e5a7f-177">Partial output:</span></span>

```azurecli-interactive 
  MaxDataDiskCount    MemoryInMb  Name                      NumberOfCores    OsDiskSizeInMb    ResourceDiskSizeInMb
------------------  ------------  ----------------------  ---------------  ----------------  ----------------------
                 2          3584  Standard_DS1                          1           1047552                    7168
                 4          7168  Standard_DS2                          2           1047552                   14336
                 8         14336  Standard_DS3                          4           1047552                   28672
                16         28672  Standard_DS4                          8           1047552                   57344
                 4         14336  Standard_DS11                         2           1047552                   28672
                 8         28672  Standard_DS12                         4           1047552                   57344
                16         57344  Standard_DS13                         8           1047552                  114688
                32        114688  Standard_DS14                        16           1047552                  229376
                 1           768  Standard_A0                           1           1047552                   20480
                 2          1792  Standard_A1                           1           1047552                   71680
                 4          3584  Standard_A2                           2           1047552                  138240
                 8          7168  Standard_A3                           4           1047552                  291840
                 4         14336  Standard_A5                           2           1047552                  138240
                16         14336  Standard_A4                           8           1047552                  619520
                 8         28672  Standard_A6                           4           1047552                  291840
                16         57344  Standard_A7                           8           1047552                  619520
```

### <a name="create-vm-with-specific-size"></a><span data-ttu-id="e5a7f-178">Skapa virtuell dator med specifika storlek</span><span class="sxs-lookup"><span data-stu-id="e5a7f-178">Create VM with specific size</span></span>

<span data-ttu-id="e5a7f-179">I föregående VM skapa exempel har en storlek inte angetts, vilket resulterar i en standardstorlek.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-179">In the previous VM creation example, a size was not provided, which results in a default size.</span></span> <span data-ttu-id="e5a7f-180">En VM-storlek kan väljas i Skapa en gång med hjälp av [az vm skapa](/cli/azure/vm#create) och `--size` argumentet.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-180">A VM size can be selected at creation time using [az vm create](/cli/azure/vm#create) and the `--size` argument.</span></span> 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a><span data-ttu-id="e5a7f-181">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-181">Resize a VM</span></span>

<span data-ttu-id="e5a7f-182">När en virtuell dator har distribuerats, kan den ändras för att öka eller minska resursallokering.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-182">After a VM has been deployed, it can be resized to increase or decrease resource allocation.</span></span>

<span data-ttu-id="e5a7f-183">Kontrollera om önskad storlek är tillgängligt på den aktuella Azure klustret innan du ändrar storlek på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-183">Before resizing a VM, check if the desired size is available on the current Azure cluster.</span></span> <span data-ttu-id="e5a7f-184">Den [az vm-vm-storlek-alternativ för](/cli/azure/vm#list-vm-resize-options) kommando returnerar listan över storlekar.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-184">The [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options) command returns the list of sizes.</span></span> 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
<span data-ttu-id="e5a7f-185">Om önskad storlek är tillgänglig, kan den virtuella datorn ändras från ett slås på tillstånd, men den startas under åtgärden.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-185">If the desired size is available, the VM can be resized from a powered-on state, however it is rebooted during the operation.</span></span> <span data-ttu-id="e5a7f-186">Använd den [az vm ändra storlek på]( /cli/azure/vm#resize) kommando för att utföra storlek.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-186">Use the [az vm resize]( /cli/azure/vm#resize) command to perform the resize.</span></span>

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

<span data-ttu-id="e5a7f-187">Om önskad storlek inte är i det aktuella klustret måste den virtuella datorn frigörs innan åtgärden Ändra storlek kan ske.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-187">If the desired size is not on the current cluster, the VM needs to be deallocated before the resize operation can occur.</span></span> <span data-ttu-id="e5a7f-188">Använd den [az vm frigöra]( /cli/azure/vm#deallocate) kommando för att stoppa och ta bort den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-188">Use the [az vm deallocate]( /cli/azure/vm#deallocate) command to stop and deallocate the VM.</span></span> <span data-ttu-id="e5a7f-189">Observera, när den virtuella datorn är påslagen tillbaka, några data på disken för temporär kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-189">Note, when the VM is powered back on, any data on the temp disk may be removed.</span></span> <span data-ttu-id="e5a7f-190">Den offentliga IP-adressen ändras även om inte en statisk IP-adress används.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-190">The public IP address also changes unless a static IP address is being used.</span></span> 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

<span data-ttu-id="e5a7f-191">När frigjorts, kan det uppstå storlek.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-191">Once deallocated, the resize can occur.</span></span> 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

<span data-ttu-id="e5a7f-192">Efter storleksändringen kan du starta den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-192">After the resize, the VM can be started.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a><span data-ttu-id="e5a7f-193">Energisparfunktioner för VM</span><span class="sxs-lookup"><span data-stu-id="e5a7f-193">VM power states</span></span>

<span data-ttu-id="e5a7f-194">En virtuell Azure-dator kan ha en av många energisparfunktioner.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-194">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="e5a7f-195">Det här tillståndet representerar det aktuella tillståndet för den virtuella datorn för hypervisor-programmet.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-195">This state represents the current state of the VM from the standpoint of the hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="e5a7f-196">Energisparfunktioner</span><span class="sxs-lookup"><span data-stu-id="e5a7f-196">Power states</span></span>

| <span data-ttu-id="e5a7f-197">Energisparläge</span><span class="sxs-lookup"><span data-stu-id="e5a7f-197">Power State</span></span> | <span data-ttu-id="e5a7f-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="e5a7f-198">Description</span></span>
|----|----|
| <span data-ttu-id="e5a7f-199">Startar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-199">Starting</span></span> | <span data-ttu-id="e5a7f-200">Anger den virtuella datorn startas.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-200">Indicates the virtual machine is being started.</span></span> |
| <span data-ttu-id="e5a7f-201">Körs</span><span class="sxs-lookup"><span data-stu-id="e5a7f-201">Running</span></span> | <span data-ttu-id="e5a7f-202">Anger att den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-202">Indicates that the virtual machine is running.</span></span> |
| <span data-ttu-id="e5a7f-203">Stoppas</span><span class="sxs-lookup"><span data-stu-id="e5a7f-203">Stopping</span></span> | <span data-ttu-id="e5a7f-204">Anger att den virtuella datorn har stoppats.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-204">Indicates that the virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="e5a7f-205">Stoppad</span><span class="sxs-lookup"><span data-stu-id="e5a7f-205">Stopped</span></span> | <span data-ttu-id="e5a7f-206">Anger att den virtuella datorn har stoppats.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-206">Indicates that the virtual machine is stopped.</span></span> <span data-ttu-id="e5a7f-207">Virtuella datorer i ett stoppat tillstånd fortfarande avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-207">Virtual machines in the stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="e5a7f-208">Det har frigjorts</span><span class="sxs-lookup"><span data-stu-id="e5a7f-208">Deallocating</span></span> | <span data-ttu-id="e5a7f-209">Anger att den virtuella datorn har flyttats.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-209">Indicates that the virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="e5a7f-210">Frigöra</span><span class="sxs-lookup"><span data-stu-id="e5a7f-210">Deallocated</span></span> | <span data-ttu-id="e5a7f-211">Anger att den virtuella datorn tas bort från hypervisor-programmet men fortfarande tillgängliga i kontrollplan.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-211">Indicates that the virtual machine is removed from the hypervisor but still available in the control plane.</span></span> <span data-ttu-id="e5a7f-212">Virtuella datorer med tillståndet Deallocated inte avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-212">Virtual machines in the Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="e5a7f-213">Anger att energisparläge för den virtuella datorn är okänt.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-213">Indicates that the power state of the virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="e5a7f-214">Hitta energiläge</span><span class="sxs-lookup"><span data-stu-id="e5a7f-214">Find power state</span></span>

<span data-ttu-id="e5a7f-215">Använd för att hämta tillståndet för en viss virtuell dator i [az vm hämta Instansvy](/cli/azure/vm#get-instance-view) kommando.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-215">To retrieve the state of a particular VM, use the [az vm get instance-view](/cli/azure/vm#get-instance-view) command.</span></span> <span data-ttu-id="e5a7f-216">Se till att ange ett giltigt namn för en virtuell dator och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-216">Be sure to specify a valid name for a virtual machine and resource group.</span></span> 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

<span data-ttu-id="e5a7f-217">Resultat:</span><span class="sxs-lookup"><span data-stu-id="e5a7f-217">Output:</span></span>

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a><span data-ttu-id="e5a7f-218">Administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="e5a7f-218">Management tasks</span></span>

<span data-ttu-id="e5a7f-219">Under livscykeln för en virtuell dator, kan du vill köra hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-219">During the life-cycle of a virtual machine, you may want to run management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="e5a7f-220">Dessutom kanske du vill skapa skript för att automatisera repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-220">Additionally, you may want to create scripts to automate repetitive or complex tasks.</span></span> <span data-ttu-id="e5a7f-221">Använda Azure CLI kan många vanliga administrativa uppgifter köras från kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-221">Using the Azure CLI, many common management tasks can be run from the command line or in scripts.</span></span> 

### <a name="get-ip-address"></a><span data-ttu-id="e5a7f-222">Hämta IP-adress</span><span class="sxs-lookup"><span data-stu-id="e5a7f-222">Get IP address</span></span>

<span data-ttu-id="e5a7f-223">Det här kommandot returnerar privata och offentliga IP-adresser för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-223">This command returns the private and public IP addresses of a virtual machine.</span></span>  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a><span data-ttu-id="e5a7f-224">Stoppa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="e5a7f-224">Stop virtual machine</span></span>

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a><span data-ttu-id="e5a7f-225">Starta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="e5a7f-225">Start virtual machine</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="e5a7f-226">Ta bort resursgruppen</span><span class="sxs-lookup"><span data-stu-id="e5a7f-226">Delete resource group</span></span>

<span data-ttu-id="e5a7f-227">En resursgrupp också tar du bort alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-227">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a><span data-ttu-id="e5a7f-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e5a7f-228">Next steps</span></span>

<span data-ttu-id="e5a7f-229">I kursen får du lärt dig om grundläggande VM skapande och hantering, till exempel hur du:</span><span class="sxs-lookup"><span data-stu-id="e5a7f-229">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e5a7f-230">Skapa och ansluta till en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-230">Create and connect to a VM</span></span>
> * <span data-ttu-id="e5a7f-231">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-231">Select and use VM images</span></span>
> * <span data-ttu-id="e5a7f-232">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-232">View and use specific VM sizes</span></span>
> * <span data-ttu-id="e5a7f-233">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-233">Resize a VM</span></span>
> * <span data-ttu-id="e5a7f-234">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e5a7f-234">View and understand VM state</span></span>

<span data-ttu-id="e5a7f-235">Gå vidare till nästa kurs vill veta mer om Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="e5a7f-235">Advance to the next tutorial to learn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="e5a7f-236">Skapa och hantera Virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="e5a7f-236">Create and Manage VM disks</span></span>](./tutorial-manage-disks.md)
