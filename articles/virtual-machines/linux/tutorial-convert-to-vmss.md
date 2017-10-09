---
title: "aaaConvert ett Azure tooa skaluppsättningen | Microsoft Docs"
description: "Skapa och distribuera en Linux Azure skaluppsättningen för virtuell dator med hello Azure CLI."
services: virtual-machine-scale-sets
documentationcenter: 
author: Thraka
manager: timlt
editor: 
tags: 
ms.assetid: 
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: article
ms.date: 04/05/2017
ms.author: adegeo
ms.openlocfilehash: e228282ac4855cef589b8500e74e9d461f9aed84
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="convert-an-existing-azure-virtual-machine-tooa-scale-set"></a><span data-ttu-id="281af-103">Konvertera en befintlig skaluppsättning för virtuell dator i Azure tooa</span><span class="sxs-lookup"><span data-stu-id="281af-103">Convert an existing Azure virtual machine tooa scale set</span></span>

<span data-ttu-id="281af-104">Den här kursen visar hur toouse Azure CLI 2.0 tooconvert en virtuell dator tooa virtuella skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="281af-104">This tutorial shows you how toouse Azure CLI 2.0 tooconvert a virtual machine tooa virtual machine scale set.</span></span> <span data-ttu-id="281af-105">Du lär dig också hur tooautomate hello konfigurationen av hello virtuella datorer i hello skala in.</span><span class="sxs-lookup"><span data-stu-id="281af-105">You also learn how tooautomate hello configuration of hello virtual machines in hello scale set.</span></span> <span data-ttu-id="281af-106">Mer information om hur tooinstall Azure CLI 2.0, se [komma igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="281af-106">For more information on how tooinstall Azure CLI 2.0, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span> <span data-ttu-id="281af-107">Läs mer om skaluppsättningar [Skalningsuppsättningarna för virtuella](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="281af-107">For more information about scale sets, see [Virtual Machine Scale Sets](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span></span>

## <a name="step-1---deprovision-hello-vm"></a><span data-ttu-id="281af-108">Steg 1 – ta bort etableringen hello VM</span><span class="sxs-lookup"><span data-stu-id="281af-108">Step 1 - Deprovision hello VM</span></span>

<span data-ttu-id="281af-109">Använda SSH tooconnect toohello VM.</span><span class="sxs-lookup"><span data-stu-id="281af-109">Use SSH tooconnect toohello VM.</span></span>

<span data-ttu-id="281af-110">Avetablering hello VM använder hello Azure VM-agenten toodelete filer och data.</span><span class="sxs-lookup"><span data-stu-id="281af-110">Deprovision hello VM using hello Azure VM agent toodelete files and data.</span></span> <span data-ttu-id="281af-111">En detaljerad översikt över avetablering finns [avbilda en virtuell Linux-dator](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="281af-111">For a detailed overview of deprovisioning, see [Capture a Linux virtual machine](capture-image.md).</span></span>

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-hello-vm"></a><span data-ttu-id="281af-112">Steg 2 – Skapa en avbildning av hello VM</span><span class="sxs-lookup"><span data-stu-id="281af-112">Step 2 - Capture an image of hello VM</span></span>

<span data-ttu-id="281af-113">En detaljerad översikt över avbildning finns [avbilda en virtuell Linux-dator](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="281af-113">For a detailed overview of capturing, see [Capture a Linux virtual machine](capture-image.md).</span></span>

<span data-ttu-id="281af-114">Frigöra hello virtuell dator med [az vm frigöra](/cli/azure/vm#deallocate):</span><span class="sxs-lookup"><span data-stu-id="281af-114">Deallocate hello VM with [az vm deallocate](/cli/azure/vm#deallocate):</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="281af-115">Generalisera hello virtuell dator med [az vm generalisera](/cli/azure/vm#generalize):</span><span class="sxs-lookup"><span data-stu-id="281af-115">Generalize hello VM with [az vm generalize](/cli/azure/vm#generalize):</span></span>

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="281af-116">Skapa en avbildning från hello Virtuella datorresursen med [az bild skapa](/cli/azure/image#create):</span><span class="sxs-lookup"><span data-stu-id="281af-116">Create an image from hello VM resource with [az image create](/cli/azure/image#create):</span></span>

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-hello-scale-set"></a><span data-ttu-id="281af-117">Steg 3 – skapa hello skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="281af-117">Step 3 - Create hello scale set</span></span>

<span data-ttu-id="281af-118">Hämta hello **id** till hello bild.</span><span class="sxs-lookup"><span data-stu-id="281af-118">Get hello **id** of hello image.</span></span>

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

<span data-ttu-id="281af-119">Skapa en virtuell dator från din bildresurs med [az vmss skapa](/cli/azure/vmss#create):</span><span class="sxs-lookup"><span data-stu-id="281af-119">Create a VM from your image resource with [az vmss create](/cli/azure/vmss#create):</span></span>

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

<span data-ttu-id="281af-120">Det här kommandot också koppla en disk på 10gb data.</span><span class="sxs-lookup"><span data-stu-id="281af-120">This command also attached a 10gb data disk.</span></span> <span data-ttu-id="281af-121">Tänk på att beroende på hello VM storlek vald (Vi använde **Standard_DS1_v2**), hello antalet tillåtna datadiskar är olika.</span><span class="sxs-lookup"><span data-stu-id="281af-121">Keep in mind that depending on hello VM size chosen (we used **Standard_DS1_v2**), hello number of data disks allowed is different.</span></span> <span data-ttu-id="281af-122">Mer information finns i hello [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="281af-122">For more information, review hello [virtual machine sizes](sizes.md).</span></span>

<span data-ttu-id="281af-123">När hello skaluppsättning har avslutats, ansluta tooit.</span><span class="sxs-lookup"><span data-stu-id="281af-123">Once hello scale set finishes, connect tooit.</span></span> <span data-ttu-id="281af-124">Hämta en lista över IP-adresser för hello instanser för SSH med [az vmss lista-instans--anslutningsinformation](/cli/azure/vmss#list-instance-connection-info):</span><span class="sxs-lookup"><span data-stu-id="281af-124">Get a list of IP addresses for hello instances for SSH with [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info):</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

<span data-ttu-id="281af-125">Nu kan du ansluta toohello virtuella instansen tooinitialize hello datadisk</span><span class="sxs-lookup"><span data-stu-id="281af-125">Now you can connect toohello virtual machine instance tooinitialize hello data disk</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-hello-data-disk"></a><span data-ttu-id="281af-126">Steg 4 – initiera hello datadisk</span><span class="sxs-lookup"><span data-stu-id="281af-126">Step 4 - Initialize hello data disk</span></span>

<span data-ttu-id="281af-127">När anslutna toohello virtuella partitionsdisk hello med `fdisk`:</span><span class="sxs-lookup"><span data-stu-id="281af-127">While connected toohello virtual machine, partition hello disk with `fdisk`:</span></span>

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="281af-128">Skriv en toohello filsystempartition med hello `mkfs` kommando:</span><span class="sxs-lookup"><span data-stu-id="281af-128">Write a file system toohello partition with hello `mkfs` command:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="281af-129">Montera hello ny disk så att den är tillgänglig i hello operativsystemet:</span><span class="sxs-lookup"><span data-stu-id="281af-129">Mount hello new disk so that it is accessible in hello operating system:</span></span>

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="281af-130">hello disk kan nu vara åtkomst via hello datadrive monteringspunkt, som kan verifieras med `ls /datadrive/`.</span><span class="sxs-lookup"><span data-stu-id="281af-130">hello disk can now be accesses through hello datadrive mountpoint, which can be verified with `ls /datadrive/`.</span></span>

<span data-ttu-id="281af-131">Avsluta hello SSH-session.</span><span class="sxs-lookup"><span data-stu-id="281af-131">End hello SSH session.</span></span>


## <a name="step-5---configure-firewall"></a><span data-ttu-id="281af-132">Steg 5 – konfigurera brandväggen</span><span class="sxs-lookup"><span data-stu-id="281af-132">Step 5 - Configure firewall</span></span>

<span data-ttu-id="281af-133">Hålslag hål hello brandväggen toohello webbserver hos hello skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="281af-133">Punch a hole through hello firewall toohello webserver hosted by hello scale set.</span></span> <span data-ttu-id="281af-134">När hello skaluppsättning skapades, skapades även belastningsutjämning och använt det **SSH** toohello enskilda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="281af-134">When hello scale set was created, a load balancer was also created, and you used it **SSH** toohello individual virtual machines.</span></span> <span data-ttu-id="281af-135">tooopen en port som du behöver två typer av information som du kan hämta med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="281af-135">tooopen a port, you need two pieces of information, which you can get using Azure CLI.</span></span>

* <span data-ttu-id="281af-136">**Frontend-IP-adresspool**</span><span class="sxs-lookup"><span data-stu-id="281af-136">**Frontend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* <span data-ttu-id="281af-137">**Backend-IP-adresspool**</span><span class="sxs-lookup"><span data-stu-id="281af-137">**Backend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

<span data-ttu-id="281af-138">Med dessa två namn, kan du öppna port **80**.</span><span class="sxs-lookup"><span data-stu-id="281af-138">With those two names, you can open port **80**.</span></span>

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a><span data-ttu-id="281af-139">Steg 6 – automatisera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="281af-139">Step 6 - Automate configuration</span></span>

<span data-ttu-id="281af-140">Hej datadisk måste toobe som konfigurerats på varje virtuell dator-instans.</span><span class="sxs-lookup"><span data-stu-id="281af-140">hello data disk needs toobe configured on each virtual machine instance.</span></span> <span data-ttu-id="281af-141">Vi kan automatisera hello konfigurationen av hello virtuell dator med hello **CustomScript** tillägg.</span><span class="sxs-lookup"><span data-stu-id="281af-141">We can automate hello configuration of hello virtual machine with hello **CustomScript** extension.</span></span>

<span data-ttu-id="281af-142">Skapa först en *.sh* skript som innehåller hello disk format kommandon.</span><span class="sxs-lookup"><span data-stu-id="281af-142">First, create a *.sh* script that includes hello disk format commands.</span></span>

```sh
#!/bin/bash

# Setup hello data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

<span data-ttu-id="281af-143">Därefter ladda upp den skriptet filen toowhere hello **CustomScript** tillägg kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="281af-143">Next, upload that script file toowhere hello **CustomScript** extension can access it.</span></span> <span data-ttu-id="281af-144">En kopia finns [här](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span><span class="sxs-lookup"><span data-stu-id="281af-144">A copy is available [here](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span></span>

<span data-ttu-id="281af-145">Skapa en lokal fil med namnet **settings.json** och placera hello följande JSON-block i den.</span><span class="sxs-lookup"><span data-stu-id="281af-145">Create a local file named **settings.json** and put hello following JSON block in it.</span></span> <span data-ttu-id="281af-146">Hej `flieUris` egenskapen ska anges toowhere skriptfilen överfördes till.</span><span class="sxs-lookup"><span data-stu-id="281af-146">hello `flieUris` property should be set toowhere your script file was uploaded to.</span></span>

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

<span data-ttu-id="281af-147">Distribuera det här kommandot tooyour skala med hello **CustomScript** tillägg, refererar till hello **settings.json** fil som vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="281af-147">Deploy this command tooyour scale set with hello **CustomScript** extension, referencing hello **settings.json** file we just created.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

<span data-ttu-id="281af-148">Det här tillägget körs automatiskt på alla aktuella instanser och alla instanser som skapats senare genom att skala.</span><span class="sxs-lookup"><span data-stu-id="281af-148">This extension automatically runs on all current instances, and any instances later created by scaling.</span></span>

## <a name="step-7---configure-autoscale-rules"></a><span data-ttu-id="281af-149">Steg 7 – konfigurera autoskalning regler</span><span class="sxs-lookup"><span data-stu-id="281af-149">Step 7 - Configure autoscale rules</span></span>

<span data-ttu-id="281af-150">Autoskala regler kan för närvarande inte anges i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="281af-150">Currently, autoscale rules cannot be set in Azure CLI.</span></span> <span data-ttu-id="281af-151">Använd hello [Azure-portalen](https://portal.azure.com) tooconfigure Autoskala.</span><span class="sxs-lookup"><span data-stu-id="281af-151">Use hello [Azure portal](https://portal.azure.com) tooconfigure autoscale.</span></span>

## <a name="step-8---management-tasks"></a><span data-ttu-id="281af-152">Steg 8 - administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="281af-152">Step 8 - Management tasks</span></span>

<span data-ttu-id="281af-153">Du kan behöva toorun under hello livscykel för skaluppsättning för hello, en eller flera hanteringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="281af-153">Throughout hello lifecycle of hello scale set, you may need toorun one or more management tasks.</span></span> <span data-ttu-id="281af-154">Dessutom du toocreate skript som automatiserar olika livscykel-uppgifter och hello Azure CLI tillhandahåller ett snabbt sätt toodo uppgifterna.</span><span class="sxs-lookup"><span data-stu-id="281af-154">Additionally, you may want toocreate scripts that automate various lifecycle-tasks, and hello Azure CLI provides a quick way toodo those tasks.</span></span> <span data-ttu-id="281af-155">Här följer några vanliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="281af-155">Here are a few common tasks.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="281af-156">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="281af-156">Get connection info</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a><span data-ttu-id="281af-157">Ange instansantal (manuell skala)</span><span class="sxs-lookup"><span data-stu-id="281af-157">Set instance count (manual scale)</span></span>

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a><span data-ttu-id="281af-158">Ta bort resursgruppen</span><span class="sxs-lookup"><span data-stu-id="281af-158">Delete resource group</span></span>

<span data-ttu-id="281af-159">En resursgrupp också tar du bort alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="281af-159">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="281af-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="281af-160">Next steps</span></span>
<span data-ttu-id="281af-161">toolearn mer om några av hello virtuella skala ange funktionerna i den här kursen finns hello följande information:</span><span class="sxs-lookup"><span data-stu-id="281af-161">toolearn more about some of hello virtual machine scale set features introduced in this tutorial, see hello following information:</span></span>

- [<span data-ttu-id="281af-162">Översikt över Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="281af-162">Azure Virtual Machine Scale Sets overview</span></span>](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [<span data-ttu-id="281af-163">Översikt över Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="281af-163">Azure Load Balancer overview</span></span>](../../load-balancer/load-balancer-overview.md)
- [<span data-ttu-id="281af-164">Styra flödet i nätverkstrafiken med nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="281af-164">Control network traffic flow with network security groups</span></span>](../../virtual-network/virtual-networks-nsg.md)