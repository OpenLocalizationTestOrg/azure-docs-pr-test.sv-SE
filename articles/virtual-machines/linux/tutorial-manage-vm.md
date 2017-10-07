---
title: aaaCreate och hantera virtuella Linux-datorer med hello Azure CLI | Microsoft Docs
description: "Självstudiekurs – skapa och hantera virtuella Linux-datorer med hello Azure CLI"
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
ms.openlocfilehash: 05f7c1cf860f809bc13f110778d3bddd619ac6f1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-linux-vms-with-hello-azure-cli"></a><span data-ttu-id="5aa27-103">Skapa och hantera virtuella Linux-datorer med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="5aa27-103">Create and Manage Linux VMs with hello Azure CLI</span></span>

<span data-ttu-id="5aa27-104">Virtuella datorer i Azure ger en fullständigt konfigurerbara och flexibel datormiljö.</span><span class="sxs-lookup"><span data-stu-id="5aa27-104">Azure virtual machines provide a fully configurable and flexible computing environment.</span></span> <span data-ttu-id="5aa27-105">Den här kursen ingår grundläggande virtuella Azure-datorn distribution objekt, till exempel välja en VM-storlek, välja en VM-avbildning och distribuera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5aa27-105">This tutorial covers basic Azure virtual machine deployment items such as selecting a VM size, selecting a VM image, and deploying a VM.</span></span> <span data-ttu-id="5aa27-106">Lär dig att:</span><span class="sxs-lookup"><span data-stu-id="5aa27-106">You learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5aa27-107">Skapa och ansluta tooa VM</span><span class="sxs-lookup"><span data-stu-id="5aa27-107">Create and connect tooa VM</span></span>
> * <span data-ttu-id="5aa27-108">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="5aa27-108">Select and use VM images</span></span>
> * <span data-ttu-id="5aa27-109">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="5aa27-109">View and use specific VM sizes</span></span>
> * <span data-ttu-id="5aa27-110">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5aa27-110">Resize a VM</span></span>
> * <span data-ttu-id="5aa27-111">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5aa27-111">View and understand VM state</span></span>


[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="5aa27-112">Om du väljer tooinstall och använda hello CLI lokalt kursen krävs att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="5aa27-112">If you choose tooinstall and use hello CLI locally, this tutorial requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="5aa27-113">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="5aa27-113">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="5aa27-114">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="5aa27-114">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="create-resource-group"></a><span data-ttu-id="5aa27-115">Skapa resursgrupp</span><span class="sxs-lookup"><span data-stu-id="5aa27-115">Create resource group</span></span>

<span data-ttu-id="5aa27-116">Skapa en resursgrupp med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="5aa27-116">Create a resource group with hello [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> 

<span data-ttu-id="5aa27-117">En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras.</span><span class="sxs-lookup"><span data-stu-id="5aa27-117">An Azure resource group is a logical container into which Azure resources are deployed and managed.</span></span> <span data-ttu-id="5aa27-118">En resursgrupp måste skapas innan en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5aa27-118">A resource group must be created before a virtual machine.</span></span> <span data-ttu-id="5aa27-119">I det här exemplet en resursgrupp med namnet *myResourceGroupVM* skapas i hello *eastus* region.</span><span class="sxs-lookup"><span data-stu-id="5aa27-119">In this example, a resource group named *myResourceGroupVM* is created in hello *eastus* region.</span></span> 

```azurecli-interactive 
az group create --name myResourceGroupVM --location eastus
```

<span data-ttu-id="5aa27-120">hello resursgrupp anges när du skapar eller ändrar en VM som kan ses i hela den här kursen.</span><span class="sxs-lookup"><span data-stu-id="5aa27-120">hello resource group is specified when creating or modifying a VM, which can be seen throughout this tutorial.</span></span>

## <a name="create-virtual-machine"></a><span data-ttu-id="5aa27-121">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5aa27-121">Create virtual machine</span></span>

<span data-ttu-id="5aa27-122">Skapa en virtuell dator med hello [az vm skapa](https://docs.microsoft.com/cli/azure/vm#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="5aa27-122">Create a virtual machine with hello [az vm create](https://docs.microsoft.com/cli/azure/vm#create) command.</span></span> 

<span data-ttu-id="5aa27-123">När du skapar en virtuell dator, är flera alternativ tillgängliga, till exempel operativsystemavbildningen, storlek och administrativa autentiseringsuppgifter för disken.</span><span class="sxs-lookup"><span data-stu-id="5aa27-123">When creating a virtual machine, several options are available such as operating system image, disk sizing, and administrative credentials.</span></span> <span data-ttu-id="5aa27-124">I det här exemplet skapas en virtuell dator med namnet *myVM* kör Ubuntu Server.</span><span class="sxs-lookup"><span data-stu-id="5aa27-124">In this example, a virtual machine is created with a name of *myVM* running Ubuntu Server.</span></span> 

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM --image UbuntuLTS --generate-ssh-keys
```

<span data-ttu-id="5aa27-125">En gång hello VM har skapats, hello Azure CLI matar ut information om hello VM.</span><span class="sxs-lookup"><span data-stu-id="5aa27-125">Once hello VM has been created, hello Azure CLI outputs information about hello VM.</span></span> <span data-ttu-id="5aa27-126">Anteckna hello `publicIpAddress`, den här adressen kan vara används tooaccess hello virtuell dator...</span><span class="sxs-lookup"><span data-stu-id="5aa27-126">Take note of hello `publicIpAddress`, this address can be used tooaccess hello virtual machine..</span></span> 

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

## <a name="connect-toovm"></a><span data-ttu-id="5aa27-127">Ansluta tooVM</span><span class="sxs-lookup"><span data-stu-id="5aa27-127">Connect tooVM</span></span>

<span data-ttu-id="5aa27-128">Nu kan du ansluta toohello VM via SSH.</span><span class="sxs-lookup"><span data-stu-id="5aa27-128">You can now connect toohello VM using SSH.</span></span> <span data-ttu-id="5aa27-129">Ersätt hello IP-adressen med hello `publicIpAddress` anges i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="5aa27-129">Replace hello example IP address with hello `publicIpAddress` noted in hello previous step.</span></span>

```bash
ssh 52.174.34.95
```

<span data-ttu-id="5aa27-130">När du är klar med hello VM Stäng hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="5aa27-130">Once finished with hello VM, close hello SSH session.</span></span> 

```bash
exit
```

## <a name="understand-vm-images"></a><span data-ttu-id="5aa27-131">Förstå VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="5aa27-131">Understand VM images</span></span>

<span data-ttu-id="5aa27-132">hello Azure marketplace innehåller många avbildningar som kan använda toocreate virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5aa27-132">hello Azure marketplace includes many images that can be used toocreate VMs.</span></span> <span data-ttu-id="5aa27-133">I föregående steg hello skapades en virtuell dator med en Ubuntu-bild.</span><span class="sxs-lookup"><span data-stu-id="5aa27-133">In hello previous steps, a virtual machine was created using an Ubuntu image.</span></span> <span data-ttu-id="5aa27-134">I det här steget används hello Azure CLI är används toosearch hello marketplace för en CentOS avbildningen, som sedan toodeploy en andra virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5aa27-134">In this step, hello Azure CLI is used toosearch hello marketplace for a CentOS image, which is then used toodeploy a second virtual machine.</span></span>  

<span data-ttu-id="5aa27-135">toosee en lista över hello vanligaste bilder använder hello [az vm bildlista](/cli/azure/vm/image#list) kommando.</span><span class="sxs-lookup"><span data-stu-id="5aa27-135">toosee a list of hello most commonly used images, use hello [az vm image list](/cli/azure/vm/image#list) command.</span></span>

```azurecli-interactive 
az vm image list --output table
```

<span data-ttu-id="5aa27-136">hello kommandoutdata returnerar hello populäraste VM-avbildningar i Azure.</span><span class="sxs-lookup"><span data-stu-id="5aa27-136">hello command output returns hello most popular VM images on Azure.</span></span>

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

<span data-ttu-id="5aa27-137">En fullständig lista över kan ses genom att lägga till hello `--all` argumentet.</span><span class="sxs-lookup"><span data-stu-id="5aa27-137">A full list can be seen by adding hello `--all` argument.</span></span> <span data-ttu-id="5aa27-138">hello bildlista kan också filtreras efter `--publisher` eller `–-offer`.</span><span class="sxs-lookup"><span data-stu-id="5aa27-138">hello image list can also be filtered by `--publisher` or `–-offer`.</span></span> <span data-ttu-id="5aa27-139">I det här exemplet hello listan filtreras för alla bilder med ett erbjudande som matchar *CentOS*.</span><span class="sxs-lookup"><span data-stu-id="5aa27-139">In this example, hello list is filtered for all images with an offer that matches *CentOS*.</span></span> 

```azurecli-interactive 
az vm image list --offer CentOS --all --output table
```

<span data-ttu-id="5aa27-140">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="5aa27-140">Partial output:</span></span>

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

<span data-ttu-id="5aa27-141">toodeploy en virtuell dator med hjälp av en viss bild anteckna hello värdet i hello *Urn* kolumn.</span><span class="sxs-lookup"><span data-stu-id="5aa27-141">toodeploy a VM using a specific image, take note of hello value in hello *Urn* column.</span></span> <span data-ttu-id="5aa27-142">När du anger hello avbildningen kan versionsnumret för hello avbildningen ersättas med ”senaste”, som väljer hello senaste versionen av hello distribution.</span><span class="sxs-lookup"><span data-stu-id="5aa27-142">When specifying hello image, hello image version number can be replaced with “latest”, which selects hello latest version of hello distribution.</span></span> <span data-ttu-id="5aa27-143">I det här exemplet hello `--image` argumentet är används toospecify hello senaste versionen av en CentOS 6.5-bild.</span><span class="sxs-lookup"><span data-stu-id="5aa27-143">In this example, hello `--image` argument is used toospecify hello latest version of a CentOS 6.5 image.</span></span>  

```azurecli-interactive 
az vm create --resource-group myResourceGroupVM --name myVM2 --image OpenLogic:CentOS:6.5:latest --generate-ssh-keys
```

## <a name="understand-vm-sizes"></a><span data-ttu-id="5aa27-144">Förstå VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="5aa27-144">Understand VM sizes</span></span>

<span data-ttu-id="5aa27-145">Storlek på en virtuell dator bestäms hello beräkningsresurser som Processorn och GPU-minne som görs tillgängliga toohello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5aa27-145">A virtual machine size determines hello amount of compute resources such as CPU, GPU, and memory that are made available toohello virtual machine.</span></span> <span data-ttu-id="5aa27-146">Virtuella datorer måste toobe storlek på lämpligt sätt för hello förväntades arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="5aa27-146">Virtual machines need toobe sized appropriately for hello expected work load.</span></span> <span data-ttu-id="5aa27-147">Om belastningen ökar, kan en befintlig virtuell dator ändras.</span><span class="sxs-lookup"><span data-stu-id="5aa27-147">If workload increases, an existing virtual machine can be resized.</span></span>

### <a name="vm-sizes"></a><span data-ttu-id="5aa27-148">VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="5aa27-148">VM Sizes</span></span>

<span data-ttu-id="5aa27-149">i den följande tabellen hello kategoriserar storlekar i användningsfall.</span><span class="sxs-lookup"><span data-stu-id="5aa27-149">hello following table categorizes sizes into use cases.</span></span>  

| <span data-ttu-id="5aa27-150">Typ</span><span class="sxs-lookup"><span data-stu-id="5aa27-150">Type</span></span>                     | <span data-ttu-id="5aa27-151">Storlekar</span><span class="sxs-lookup"><span data-stu-id="5aa27-151">Sizes</span></span>           |    <span data-ttu-id="5aa27-152">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5aa27-152">Description</span></span>       |
|--------------------------|-------------------|------------------------------------------------------------------------------------------------------------------------------------|
| [<span data-ttu-id="5aa27-153">Generellt syfte</span><span class="sxs-lookup"><span data-stu-id="5aa27-153">General purpose</span></span>](sizes-general.md)         |<span data-ttu-id="5aa27-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0 7</span><span class="sxs-lookup"><span data-stu-id="5aa27-154">Dsv3, Dv3, DSv2, Dv2, DS, D, Av2, A0-7</span></span>| <span data-ttu-id="5aa27-155">Belastningsutjämnade CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="5aa27-155">Balanced CPU-to-memory.</span></span> <span data-ttu-id="5aa27-156">Idealiskt för dev / test och lösningar för små toomedium program och data.</span><span class="sxs-lookup"><span data-stu-id="5aa27-156">Ideal for dev / test and small toomedium applications and data solutions.</span></span>  |
| [<span data-ttu-id="5aa27-157">Beräkningsoptimerad</span><span class="sxs-lookup"><span data-stu-id="5aa27-157">Compute optimized</span></span>](sizes-compute.md)   | <span data-ttu-id="5aa27-158">FS, F</span><span class="sxs-lookup"><span data-stu-id="5aa27-158">Fs, F</span></span>             | <span data-ttu-id="5aa27-159">Hög CPU-till-minne.</span><span class="sxs-lookup"><span data-stu-id="5aa27-159">High CPU-to-memory.</span></span> <span data-ttu-id="5aa27-160">Bra för medelhög trafik program, nätverksinstallationer och batchprocesser.</span><span class="sxs-lookup"><span data-stu-id="5aa27-160">Good for medium traffic applications, network appliances, and batch processes.</span></span>        |
| [<span data-ttu-id="5aa27-161">Minnesoptimerad</span><span class="sxs-lookup"><span data-stu-id="5aa27-161">Memory optimized</span></span>](../virtual-machines-windows-sizes-memory.md)    | <span data-ttu-id="5aa27-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span><span class="sxs-lookup"><span data-stu-id="5aa27-162">Esv3, Ev3, M, GS, G, DSv2, DS, Dv2, D</span></span>   | <span data-ttu-id="5aa27-163">Hög minne-till-core.</span><span class="sxs-lookup"><span data-stu-id="5aa27-163">High memory-to-core.</span></span> <span data-ttu-id="5aa27-164">Perfekt för relationsdatabaser, medelhög toolarge cacheminnen och analyser i minnet.</span><span class="sxs-lookup"><span data-stu-id="5aa27-164">Great for relational databases, medium toolarge caches, and in-memory analytics.</span></span>                 |
| [<span data-ttu-id="5aa27-165">Lagringsoptimerad</span><span class="sxs-lookup"><span data-stu-id="5aa27-165">Storage optimized</span></span>](../virtual-machines-windows-sizes-storage.md)      | <span data-ttu-id="5aa27-166">Ls</span><span class="sxs-lookup"><span data-stu-id="5aa27-166">Ls</span></span>                | <span data-ttu-id="5aa27-167">Högt diskgenomflöde och I/O.</span><span class="sxs-lookup"><span data-stu-id="5aa27-167">High disk throughput and IO.</span></span> <span data-ttu-id="5aa27-168">Perfekt för stordata, SQL- och NoSQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="5aa27-168">Ideal for Big Data, SQL, and NoSQL databases.</span></span>                                                         |
| [<span data-ttu-id="5aa27-169">GPU</span><span class="sxs-lookup"><span data-stu-id="5aa27-169">GPU</span></span>](sizes-gpu.md)          | <span data-ttu-id="5aa27-170">NV NC</span><span class="sxs-lookup"><span data-stu-id="5aa27-170">NV, NC</span></span>            | <span data-ttu-id="5aa27-171">Särskilda virtuella datorer som mål för tunga grafisk återgivning och redigering av video.</span><span class="sxs-lookup"><span data-stu-id="5aa27-171">Specialized VMs targeted for heavy graphic rendering and video editing.</span></span>       |
| [<span data-ttu-id="5aa27-172">Hög prestanda</span><span class="sxs-lookup"><span data-stu-id="5aa27-172">High performance</span></span>](sizes-hpc.md) | <span data-ttu-id="5aa27-173">H, A8-11</span><span class="sxs-lookup"><span data-stu-id="5aa27-173">H, A8-11</span></span>          | <span data-ttu-id="5aa27-174">Våra mest kraftfulla CPU virtuella datorer med valfritt hög genomströmning nätverksgränssnitt (RDMA).</span><span class="sxs-lookup"><span data-stu-id="5aa27-174">Our most powerful CPU VMs with optional high-throughput network interfaces (RDMA).</span></span> 


### <a name="find-available-vm-sizes"></a><span data-ttu-id="5aa27-175">Hitta tillgängliga storlekar på VM</span><span class="sxs-lookup"><span data-stu-id="5aa27-175">Find available VM sizes</span></span>

<span data-ttu-id="5aa27-176">toosee en lista över Virtuella storlekar tillgängliga i en viss region, Använd hello [az lista-storlekar på vm](/cli/azure/vm#list-sizes) kommando.</span><span class="sxs-lookup"><span data-stu-id="5aa27-176">toosee a list of VM sizes available in a particular region, use hello [az vm list-sizes](/cli/azure/vm#list-sizes) command.</span></span> 

```azurecli-interactive 
az vm list-sizes --location eastus --output table
```

<span data-ttu-id="5aa27-177">Partiella utdata:</span><span class="sxs-lookup"><span data-stu-id="5aa27-177">Partial output:</span></span>

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

### <a name="create-vm-with-specific-size"></a><span data-ttu-id="5aa27-178">Skapa virtuell dator med specifika storlek</span><span class="sxs-lookup"><span data-stu-id="5aa27-178">Create VM with specific size</span></span>

<span data-ttu-id="5aa27-179">I hello tidigare VM skapa exempelvis har en storlek inte angetts, vilket resulterar i en standardstorlek.</span><span class="sxs-lookup"><span data-stu-id="5aa27-179">In hello previous VM creation example, a size was not provided, which results in a default size.</span></span> <span data-ttu-id="5aa27-180">En VM-storlek kan väljas i Skapa en gång med hjälp av [az vm skapa](/cli/azure/vm#create) och hello `--size` argumentet.</span><span class="sxs-lookup"><span data-stu-id="5aa27-180">A VM size can be selected at creation time using [az vm create](/cli/azure/vm#create) and hello `--size` argument.</span></span> 

```azurecli-interactive 
az vm create \
    --resource-group myResourceGroupVM \
    --name myVM3 \
    --image UbuntuLTS \
    --size Standard_F4s \
    --generate-ssh-keys
```

### <a name="resize-a-vm"></a><span data-ttu-id="5aa27-181">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5aa27-181">Resize a VM</span></span>

<span data-ttu-id="5aa27-182">När en virtuell dator har distribuerats kan storleksändrade tooincrease eller minska resursallokering.</span><span class="sxs-lookup"><span data-stu-id="5aa27-182">After a VM has been deployed, it can be resized tooincrease or decrease resource allocation.</span></span>

<span data-ttu-id="5aa27-183">Innan du ändrar storlek på en virtuell dator, kontrollera om hello önskad storlek är tillgänglig på hello aktuella Azure-klustret.</span><span class="sxs-lookup"><span data-stu-id="5aa27-183">Before resizing a VM, check if hello desired size is available on hello current Azure cluster.</span></span> <span data-ttu-id="5aa27-184">Hej [az vm-vm-storlek-alternativ för](/cli/azure/vm#list-vm-resize-options) kommandot returnerar hello lista över storlekar.</span><span class="sxs-lookup"><span data-stu-id="5aa27-184">hello [az vm list-vm-resize-options](/cli/azure/vm#list-vm-resize-options) command returns hello list of sizes.</span></span> 

```azurecli-interactive 
az vm list-vm-resize-options --resource-group myResourceGroupVM --name myVM --query [].name
```
<span data-ttu-id="5aa27-185">Eventuellt hello storleken är tillgänglig hello VM storlek kan ändras från ett slås på tillstånd, men den startas under hello-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="5aa27-185">If hello desired size is available, hello VM can be resized from a powered-on state, however it is rebooted during hello operation.</span></span> <span data-ttu-id="5aa27-186">Använd hello [az vm ändra storlek på]( /cli/azure/vm#resize) kommandot tooperform hello storlek.</span><span class="sxs-lookup"><span data-stu-id="5aa27-186">Use hello [az vm resize]( /cli/azure/vm#resize) command tooperform hello resize.</span></span>

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_DS4_v2
```

<span data-ttu-id="5aa27-187">Om hello önskad storlek är inte på hello aktuella klustret hello VM behov toobe frigjorts innan hello att ändra storlek på kan uppstå.</span><span class="sxs-lookup"><span data-stu-id="5aa27-187">If hello desired size is not on hello current cluster, hello VM needs toobe deallocated before hello resize operation can occur.</span></span> <span data-ttu-id="5aa27-188">Använd hello [az vm frigöra]( /cli/azure/vm#deallocate) kommando toostop och frigöra hello VM.</span><span class="sxs-lookup"><span data-stu-id="5aa27-188">Use hello [az vm deallocate]( /cli/azure/vm#deallocate) command toostop and deallocate hello VM.</span></span> <span data-ttu-id="5aa27-189">Observera att när hello VM är påslagen tillbaka, några data på hello temporär disk kan tas bort.</span><span class="sxs-lookup"><span data-stu-id="5aa27-189">Note, when hello VM is powered back on, any data on hello temp disk may be removed.</span></span> <span data-ttu-id="5aa27-190">hello offentliga IP-adressen ändras även om inte en statisk IP-adress används.</span><span class="sxs-lookup"><span data-stu-id="5aa27-190">hello public IP address also changes unless a static IP address is being used.</span></span> 

```azurecli-interactive 
az vm deallocate --resource-group myResourceGroupVM --name myVM
```

<span data-ttu-id="5aa27-191">När frigjorts, kan det uppstå hello storlek.</span><span class="sxs-lookup"><span data-stu-id="5aa27-191">Once deallocated, hello resize can occur.</span></span> 

```azurecli-interactive 
az vm resize --resource-group myResourceGroupVM --name myVM --size Standard_GS1
```

<span data-ttu-id="5aa27-192">När hello ändra storlek på kan hello VM startas.</span><span class="sxs-lookup"><span data-stu-id="5aa27-192">After hello resize, hello VM can be started.</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

## <a name="vm-power-states"></a><span data-ttu-id="5aa27-193">Energisparfunktioner för VM</span><span class="sxs-lookup"><span data-stu-id="5aa27-193">VM power states</span></span>

<span data-ttu-id="5aa27-194">En virtuell Azure-dator kan ha en av många energisparfunktioner.</span><span class="sxs-lookup"><span data-stu-id="5aa27-194">An Azure VM can have one of many power states.</span></span> <span data-ttu-id="5aa27-195">Det här tillståndet representerar hello aktuell status för hello VM från hello synvinkel av hello hypervisor.</span><span class="sxs-lookup"><span data-stu-id="5aa27-195">This state represents hello current state of hello VM from hello standpoint of hello hypervisor.</span></span> 

### <a name="power-states"></a><span data-ttu-id="5aa27-196">Energisparfunktioner</span><span class="sxs-lookup"><span data-stu-id="5aa27-196">Power states</span></span>

| <span data-ttu-id="5aa27-197">Energisparläge</span><span class="sxs-lookup"><span data-stu-id="5aa27-197">Power State</span></span> | <span data-ttu-id="5aa27-198">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="5aa27-198">Description</span></span>
|----|----|
| <span data-ttu-id="5aa27-199">Startar</span><span class="sxs-lookup"><span data-stu-id="5aa27-199">Starting</span></span> | <span data-ttu-id="5aa27-200">Anger hello virtuella datorn startas.</span><span class="sxs-lookup"><span data-stu-id="5aa27-200">Indicates hello virtual machine is being started.</span></span> |
| <span data-ttu-id="5aa27-201">Körs</span><span class="sxs-lookup"><span data-stu-id="5aa27-201">Running</span></span> | <span data-ttu-id="5aa27-202">Anger att hello den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="5aa27-202">Indicates that hello virtual machine is running.</span></span> |
| <span data-ttu-id="5aa27-203">Stoppas</span><span class="sxs-lookup"><span data-stu-id="5aa27-203">Stopping</span></span> | <span data-ttu-id="5aa27-204">Anger att hello den virtuella datorn stoppas.</span><span class="sxs-lookup"><span data-stu-id="5aa27-204">Indicates that hello virtual machine is being stopped.</span></span> | 
| <span data-ttu-id="5aa27-205">Stoppad</span><span class="sxs-lookup"><span data-stu-id="5aa27-205">Stopped</span></span> | <span data-ttu-id="5aa27-206">Anger att hello den virtuella datorn har stoppats.</span><span class="sxs-lookup"><span data-stu-id="5aa27-206">Indicates that hello virtual machine is stopped.</span></span> <span data-ttu-id="5aa27-207">Virtuella datorer i hello stoppats tillstånd fortfarande avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="5aa27-207">Virtual machines in hello stopped state still incur compute charges.</span></span>  |
| <span data-ttu-id="5aa27-208">Det har frigjorts</span><span class="sxs-lookup"><span data-stu-id="5aa27-208">Deallocating</span></span> | <span data-ttu-id="5aa27-209">Anger att hello den virtuella datorn har flyttats.</span><span class="sxs-lookup"><span data-stu-id="5aa27-209">Indicates that hello virtual machine is being deallocated.</span></span> |
| <span data-ttu-id="5aa27-210">Frigöra</span><span class="sxs-lookup"><span data-stu-id="5aa27-210">Deallocated</span></span> | <span data-ttu-id="5aa27-211">Anger att hello den virtuella datorn tas bort från hello hypervisor men fortfarande tillgängliga i hello kontrollplan.</span><span class="sxs-lookup"><span data-stu-id="5aa27-211">Indicates that hello virtual machine is removed from hello hypervisor but still available in hello control plane.</span></span> <span data-ttu-id="5aa27-212">Virtuella datorer i hello Deallocated tillstånd inte avgifter beräkning.</span><span class="sxs-lookup"><span data-stu-id="5aa27-212">Virtual machines in hello Deallocated state do not incur compute charges.</span></span> |
| - | <span data-ttu-id="5aa27-213">Anger att hello energisparläge för hello virtuell dator är okänt.</span><span class="sxs-lookup"><span data-stu-id="5aa27-213">Indicates that hello power state of hello virtual machine is unknown.</span></span> |

### <a name="find-power-state"></a><span data-ttu-id="5aa27-214">Hitta energiläge</span><span class="sxs-lookup"><span data-stu-id="5aa27-214">Find power state</span></span>

<span data-ttu-id="5aa27-215">tooretrieve hello tillståndet för en viss virtuell dator, Använd hello [az vm hämta Instansvy](/cli/azure/vm#get-instance-view) kommando.</span><span class="sxs-lookup"><span data-stu-id="5aa27-215">tooretrieve hello state of a particular VM, use hello [az vm get instance-view](/cli/azure/vm#get-instance-view) command.</span></span> <span data-ttu-id="5aa27-216">Vara säker på att toospecify ett giltigt namn för en virtuell dator och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="5aa27-216">Be sure toospecify a valid name for a virtual machine and resource group.</span></span> 

```azurecli-interactive 
az vm get-instance-view \
    --name myVM \
    --resource-group myResourceGroupVM \
    --query instanceView.statuses[1] --output table
```

<span data-ttu-id="5aa27-217">Resultat:</span><span class="sxs-lookup"><span data-stu-id="5aa27-217">Output:</span></span>

```azurecli-interactive 
ode                DisplayStatus    Level
------------------  ---------------  -------
PowerState/running  VM running       Info
```

## <a name="management-tasks"></a><span data-ttu-id="5aa27-218">Administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="5aa27-218">Management tasks</span></span>

<span data-ttu-id="5aa27-219">Under hello livscykeln för en virtuell dator, kan du toorun hanteringsuppgifter, till exempel starta, stoppa eller ta bort en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5aa27-219">During hello life-cycle of a virtual machine, you may want toorun management tasks such as starting, stopping, or deleting a virtual machine.</span></span> <span data-ttu-id="5aa27-220">Dessutom kan du toocreate skript tooautomate repetitiva och komplicerade uppgifter.</span><span class="sxs-lookup"><span data-stu-id="5aa27-220">Additionally, you may want toocreate scripts tooautomate repetitive or complex tasks.</span></span> <span data-ttu-id="5aa27-221">Använder hello Azure CLI, kan många vanliga administrativa uppgifter köras från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="5aa27-221">Using hello Azure CLI, many common management tasks can be run from hello command line or in scripts.</span></span> 

### <a name="get-ip-address"></a><span data-ttu-id="5aa27-222">Hämta IP-adress</span><span class="sxs-lookup"><span data-stu-id="5aa27-222">Get IP address</span></span>

<span data-ttu-id="5aa27-223">Det här kommandot returnerar hello privata och offentliga IP-adresser för en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5aa27-223">This command returns hello private and public IP addresses of a virtual machine.</span></span>  

```azurecli-interactive 
az vm list-ip-addresses --resource-group myResourceGroupVM --name myVM --output table
```

### <a name="stop-virtual-machine"></a><span data-ttu-id="5aa27-224">Stoppa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5aa27-224">Stop virtual machine</span></span>

```azurecli-interactive 
az vm stop --resource-group myResourceGroupVM --name myVM
```

### <a name="start-virtual-machine"></a><span data-ttu-id="5aa27-225">Starta den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="5aa27-225">Start virtual machine</span></span>

```azurecli-interactive 
az vm start --resource-group myResourceGroupVM --name myVM
```

### <a name="delete-resource-group"></a><span data-ttu-id="5aa27-226">Ta bort resursgruppen</span><span class="sxs-lookup"><span data-stu-id="5aa27-226">Delete resource group</span></span>

<span data-ttu-id="5aa27-227">En resursgrupp också tar du bort alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="5aa27-227">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli-interactive 
az group delete --name myResourceGroupVM --no-wait --yes
```

## <a name="next-steps"></a><span data-ttu-id="5aa27-228">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5aa27-228">Next steps</span></span>

<span data-ttu-id="5aa27-229">I kursen får du lärt dig om grundläggande VM skapande och hantering, till exempel hur du:</span><span class="sxs-lookup"><span data-stu-id="5aa27-229">In this tutorial, you learned about basic VM creation and management such as how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="5aa27-230">Skapa och ansluta tooa VM</span><span class="sxs-lookup"><span data-stu-id="5aa27-230">Create and connect tooa VM</span></span>
> * <span data-ttu-id="5aa27-231">Välj och Använd VM-avbildningar</span><span class="sxs-lookup"><span data-stu-id="5aa27-231">Select and use VM images</span></span>
> * <span data-ttu-id="5aa27-232">Visa och använda specifika VM-storlekar</span><span class="sxs-lookup"><span data-stu-id="5aa27-232">View and use specific VM sizes</span></span>
> * <span data-ttu-id="5aa27-233">Ändra storlek på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5aa27-233">Resize a VM</span></span>
> * <span data-ttu-id="5aa27-234">Visa och förstå tillstånd för virtuell dator</span><span class="sxs-lookup"><span data-stu-id="5aa27-234">View and understand VM state</span></span>

<span data-ttu-id="5aa27-235">Avancera toohello nästa självstudiekurs toolearn om Virtuella diskar.</span><span class="sxs-lookup"><span data-stu-id="5aa27-235">Advance toohello next tutorial toolearn about VM disks.</span></span>  

> [!div class="nextstepaction"]
> [<span data-ttu-id="5aa27-236">Skapa och hantera Virtuella diskar</span><span class="sxs-lookup"><span data-stu-id="5aa27-236">Create and Manage VM disks</span></span>](./tutorial-manage-disks.md)
