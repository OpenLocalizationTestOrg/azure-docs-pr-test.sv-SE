---
title: "Konvertera en virtuell Azure-dator till en skalningsuppsättning | Microsoft Docs"
description: "Skapa och distribuera en Linux Azure skaluppsättningen för virtuell dator med Azure CLI."
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
ms.openlocfilehash: 8d3376d2791b1349298db618d475ce5573083702
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="convert-an-existing-azure-virtual-machine-to-a-scale-set"></a><span data-ttu-id="bdb30-103">Konvertera en befintlig Azure virtuell dator till en skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="bdb30-103">Convert an existing Azure virtual machine to a scale set</span></span>

<span data-ttu-id="bdb30-104">Den här kursen visar hur du använder Azure CLI 2.0 för att konvertera en virtuell dator till en skaluppsättning för virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="bdb30-104">This tutorial shows you how to use Azure CLI 2.0 to convert a virtual machine to a virtual machine scale set.</span></span> <span data-ttu-id="bdb30-105">Du också lära dig hur du kan automatisera konfigurationen av de virtuella datorerna i skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="bdb30-105">You also learn how to automate the configuration of the virtual machines in the scale set.</span></span> <span data-ttu-id="bdb30-106">Läs mer om hur du installerar Azure CLI 2.0 [komma igång med Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span><span class="sxs-lookup"><span data-stu-id="bdb30-106">For more information on how to install Azure CLI 2.0, see [Getting Started with Azure CLI 2.0](/cli/azure/get-started-with-azure-cli.md).</span></span> <span data-ttu-id="bdb30-107">Läs mer om skaluppsättningar [Skalningsuppsättningarna för virtuella](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span><span class="sxs-lookup"><span data-stu-id="bdb30-107">For more information about scale sets, see [Virtual Machine Scale Sets](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md).</span></span>

## <a name="step-1---deprovision-the-vm"></a><span data-ttu-id="bdb30-108">Steg 1 – ta bort etableringen av den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="bdb30-108">Step 1 - Deprovision the VM</span></span>

<span data-ttu-id="bdb30-109">Använda SSH för att ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bdb30-109">Use SSH to connect to the VM.</span></span>

<span data-ttu-id="bdb30-110">Ta bort etableringen av den virtuella datorn med Virtuella Azure-agenten att ta bort filer och data.</span><span class="sxs-lookup"><span data-stu-id="bdb30-110">Deprovision the VM using the Azure VM agent to delete files and data.</span></span> <span data-ttu-id="bdb30-111">En detaljerad översikt över avetablering finns [avbilda en virtuell Linux-dator](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="bdb30-111">For a detailed overview of deprovisioning, see [Capture a Linux virtual machine](capture-image.md).</span></span>

```bash
sudo waagent -deprovision+user -force
exit
```

## <a name="step-2---capture-an-image-of-the-vm"></a><span data-ttu-id="bdb30-112">Steg 2 – Skapa en avbildning av den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="bdb30-112">Step 2 - Capture an image of the VM</span></span>

<span data-ttu-id="bdb30-113">En detaljerad översikt över avbildning finns [avbilda en virtuell Linux-dator](capture-image.md).</span><span class="sxs-lookup"><span data-stu-id="bdb30-113">For a detailed overview of capturing, see [Capture a Linux virtual machine](capture-image.md).</span></span>

<span data-ttu-id="bdb30-114">Frigör den virtuella datorn med [az vm frigöra](/cli/azure/vm#deallocate):</span><span class="sxs-lookup"><span data-stu-id="bdb30-114">Deallocate the VM with [az vm deallocate](/cli/azure/vm#deallocate):</span></span>

```azurecli
az vm deallocate --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="bdb30-115">Generalisera den virtuella datorn med [az vm generalisera](/cli/azure/vm#generalize):</span><span class="sxs-lookup"><span data-stu-id="bdb30-115">Generalize the VM with [az vm generalize](/cli/azure/vm#generalize):</span></span>

```azurecli
az vm generalize --resource-group myResourceGroup --name myVM
```

<span data-ttu-id="bdb30-116">Skapa en avbildning från den Virtuella datorresursen med [az bild skapa](/cli/azure/image#create):</span><span class="sxs-lookup"><span data-stu-id="bdb30-116">Create an image from the VM resource with [az image create](/cli/azure/image#create):</span></span>

```azurecli
az image create --resource-group myResourceGroup --name myImage --source myVM
```

## <a name="step-3---create-the-scale-set"></a><span data-ttu-id="bdb30-117">Steg 3 – skapa skaluppsättning</span><span class="sxs-lookup"><span data-stu-id="bdb30-117">Step 3 - Create the scale set</span></span>

<span data-ttu-id="bdb30-118">Hämta den **id** för bilden.</span><span class="sxs-lookup"><span data-stu-id="bdb30-118">Get the **id** of the image.</span></span>

```azurecli
az image show --resource-group myResourceGroup --name myImage --query id
```

```json
"/subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage"
```

<span data-ttu-id="bdb30-119">Skapa en virtuell dator från din bildresurs med [az vmss skapa](/cli/azure/vmss#create):</span><span class="sxs-lookup"><span data-stu-id="bdb30-119">Create a VM from your image resource with [az vmss create](/cli/azure/vmss#create):</span></span>

```azurecli
az vmss create --resource-group myResourceGroup --name myScaleSet --image /subscriptions/afbdaf8b-9188-4651-bce1-9115dd57c98b/resourceGroups/vmtest/providers/Microsoft.Compute/images/myImage --upgrade-policy-mode automatic --vm-sku Standard_DS1_v2 --data-disk-sizes-gb 10 --admin-username azureuser --generate-ssh-keys
```

<span data-ttu-id="bdb30-120">Det här kommandot också koppla en disk på 10gb data.</span><span class="sxs-lookup"><span data-stu-id="bdb30-120">This command also attached a 10gb data disk.</span></span> <span data-ttu-id="bdb30-121">Tänk på att beroende på den virtuella datorn storlek vald (Vi använde **Standard_DS1_v2**), antalet datadiskar som tillåts är olika.</span><span class="sxs-lookup"><span data-stu-id="bdb30-121">Keep in mind that depending on the VM size chosen (we used **Standard_DS1_v2**), the number of data disks allowed is different.</span></span> <span data-ttu-id="bdb30-122">Mer information finns i [storlekar för virtuella datorer](sizes.md).</span><span class="sxs-lookup"><span data-stu-id="bdb30-122">For more information, review the [virtual machine sizes](sizes.md).</span></span>

<span data-ttu-id="bdb30-123">När skaluppsättning har avslutats, ansluta till den.</span><span class="sxs-lookup"><span data-stu-id="bdb30-123">Once the scale set finishes, connect to it.</span></span> <span data-ttu-id="bdb30-124">Hämta en lista över IP-adresser för instanserna för SSH med [az vmss lista-instans--anslutningsinformation](/cli/azure/vmss#list-instance-connection-info):</span><span class="sxs-lookup"><span data-stu-id="bdb30-124">Get a list of IP addresses for the instances for SSH with [az vmss list-instance-connection-info](/cli/azure/vmss#list-instance-connection-info):</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

```json
[
  "52.183.00.000:50000",
  "52.183.00.000:50003"
]
```

<span data-ttu-id="bdb30-125">Nu kan du ansluta till den virtuella dator instansen att initiera datadisk</span><span class="sxs-lookup"><span data-stu-id="bdb30-125">Now you can connect to the virtual machine instance to initialize the data disk</span></span>

```bash
ssh -i ~/.ssh/id_rsa.pub -p 50000 azureuser@52.183.00.000
```

## <a name="step-4---initialize-the-data-disk"></a><span data-ttu-id="bdb30-126">Steg 4 – initiera datadisken</span><span class="sxs-lookup"><span data-stu-id="bdb30-126">Step 4 - Initialize the data disk</span></span>

<span data-ttu-id="bdb30-127">När du är ansluten till den virtuella datorn, partitionera hårddisken med `fdisk`:</span><span class="sxs-lookup"><span data-stu-id="bdb30-127">While connected to the virtual machine, partition the disk with `fdisk`:</span></span>

```bash
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | sudo fdisk /dev/sdc
```

<span data-ttu-id="bdb30-128">Skriva ett filsystem till partitionen med det `mkfs` kommando:</span><span class="sxs-lookup"><span data-stu-id="bdb30-128">Write a file system to the partition with the `mkfs` command:</span></span>

```bash
sudo mkfs -t ext4 /dev/sdc1
```

<span data-ttu-id="bdb30-129">Montera den nya disken så att den är tillgänglig i operativsystemet:</span><span class="sxs-lookup"><span data-stu-id="bdb30-129">Mount the new disk so that it is accessible in the operating system:</span></span>

```bash
sudo mkdir /datadrive ; sudo mount /dev/sdc1 /datadrive
```

<span data-ttu-id="bdb30-130">Disken kan nu vara åtkomst via datadrive monteringspunkt, som kan verifieras med `ls /datadrive/`.</span><span class="sxs-lookup"><span data-stu-id="bdb30-130">The disk can now be accesses through the datadrive mountpoint, which can be verified with `ls /datadrive/`.</span></span>

<span data-ttu-id="bdb30-131">Avsluta SSH-session.</span><span class="sxs-lookup"><span data-stu-id="bdb30-131">End the SSH session.</span></span>


## <a name="step-5---configure-firewall"></a><span data-ttu-id="bdb30-132">Steg 5 – konfigurera brandväggen</span><span class="sxs-lookup"><span data-stu-id="bdb30-132">Step 5 - Configure firewall</span></span>

<span data-ttu-id="bdb30-133">Hålslag brandväggen för att den webbserver som värd av skaluppsättningen hål.</span><span class="sxs-lookup"><span data-stu-id="bdb30-133">Punch a hole through the firewall to the webserver hosted by the scale set.</span></span> <span data-ttu-id="bdb30-134">När skaluppsättning skapades, skapades även en belastningsutjämnare och använt det **SSH** till enskilda virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="bdb30-134">When the scale set was created, a load balancer was also created, and you used it **SSH** to the individual virtual machines.</span></span> <span data-ttu-id="bdb30-135">Om du vill öppna en port som du behöver två typer av information som du kan hämta med hjälp av Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bdb30-135">To open a port, you need two pieces of information, which you can get using Azure CLI.</span></span>

* <span data-ttu-id="bdb30-136">**Frontend-IP-adresspool**</span><span class="sxs-lookup"><span data-stu-id="bdb30-136">**Frontend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query frontendIpConfigurations[0].name`

* <span data-ttu-id="bdb30-137">**Backend-IP-adresspool**</span><span class="sxs-lookup"><span data-stu-id="bdb30-137">**Backend IP address pool**</span></span>  
`az network lb show --resource-group myResourceGroup --name myScaleSetLB --output table --query backendAddressPools[0].name`

<span data-ttu-id="bdb30-138">Med dessa två namn, kan du öppna port **80**.</span><span class="sxs-lookup"><span data-stu-id="bdb30-138">With those two names, you can open port **80**.</span></span>

```azurecli
az network lb rule create --backend-pool-name myScaleSetLBBEPool --backend-port 80 --frontend-ip-name loadBalancerFrontEnd --frontend-port 80 --name webserver --protocol tcp --resource-group myResourceGroup --lb-name myScaleSetLB
```


## <a name="step-6---automate-configuration"></a><span data-ttu-id="bdb30-139">Steg 6 – automatisera konfigurationen</span><span class="sxs-lookup"><span data-stu-id="bdb30-139">Step 6 - Automate configuration</span></span>

<span data-ttu-id="bdb30-140">Datadisken måste konfigureras på varje virtuell dator-instans.</span><span class="sxs-lookup"><span data-stu-id="bdb30-140">The data disk needs to be configured on each virtual machine instance.</span></span> <span data-ttu-id="bdb30-141">Vi kan automatisera konfigurationen av den virtuella datorn med den **CustomScript** tillägg.</span><span class="sxs-lookup"><span data-stu-id="bdb30-141">We can automate the configuration of the virtual machine with the **CustomScript** extension.</span></span>

<span data-ttu-id="bdb30-142">Skapa först en *.sh* skript som innehåller kommandon för disk-format.</span><span class="sxs-lookup"><span data-stu-id="bdb30-142">First, create a *.sh* script that includes the disk format commands.</span></span>

```sh
#!/bin/bash

# Setup the data disk
(echo n; echo p; echo 1; echo  ; echo  ; echo w) | fdisk /dev/sdc
fdisk /dev/sdc
mkfs -t ext4 /dev/sdc1
mkdir /datadrive
mount /dev/sdc1 /datadrive

exit 0
```

<span data-ttu-id="bdb30-143">Därefter ladda upp den skriptfilen till platsen där den **CustomScript** tillägg kan komma åt den.</span><span class="sxs-lookup"><span data-stu-id="bdb30-143">Next, upload that script file to where the **CustomScript** extension can access it.</span></span> <span data-ttu-id="bdb30-144">En kopia finns [här](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span><span class="sxs-lookup"><span data-stu-id="bdb30-144">A copy is available [here](https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4).</span></span>

<span data-ttu-id="bdb30-145">Skapa en lokal fil med namnet **settings.json** och placera följande JSON-blocket i den.</span><span class="sxs-lookup"><span data-stu-id="bdb30-145">Create a local file named **settings.json** and put the following JSON block in it.</span></span> <span data-ttu-id="bdb30-146">Den `flieUris` egenskapen ska anges till där skriptfilen överfördes till.</span><span class="sxs-lookup"><span data-stu-id="bdb30-146">The `flieUris` property should be set to where your script file was uploaded to.</span></span>

```json
{
  "fileUris": ["https://gist.githubusercontent.com/Thraka/ab1d8b78ac4b23722f3d3c1c03ac5df4/raw/3ac6e385010ac675e23ce583ce27b1a752f1b482/prep-vmss.sh"],
  "commandToExecute": "bash prep-vmss.sh" 
}
```

<span data-ttu-id="bdb30-147">Distribuera det här kommandot till nivå med den **CustomScript** tillägg, refererar till den **settings.json** fil som vi just skapade.</span><span class="sxs-lookup"><span data-stu-id="bdb30-147">Deploy this command to your scale set with the **CustomScript** extension, referencing the **settings.json** file we just created.</span></span>

```azurecli
az vmss extension set --publisher Microsoft.Azure.Extensions --version 2.0 --name CustomScript --resource-group myResourceGroup --vmss-name myScaleSet --settings @settings.json
```

<span data-ttu-id="bdb30-148">Det här tillägget körs automatiskt på alla aktuella instanser och alla instanser som skapats senare genom att skala.</span><span class="sxs-lookup"><span data-stu-id="bdb30-148">This extension automatically runs on all current instances, and any instances later created by scaling.</span></span>

## <a name="step-7---configure-autoscale-rules"></a><span data-ttu-id="bdb30-149">Steg 7 – konfigurera autoskalning regler</span><span class="sxs-lookup"><span data-stu-id="bdb30-149">Step 7 - Configure autoscale rules</span></span>

<span data-ttu-id="bdb30-150">Autoskala regler kan för närvarande inte anges i Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="bdb30-150">Currently, autoscale rules cannot be set in Azure CLI.</span></span> <span data-ttu-id="bdb30-151">Använd den [Azure-portalen](https://portal.azure.com) så här konfigurerar du Autoskala.</span><span class="sxs-lookup"><span data-stu-id="bdb30-151">Use the [Azure portal](https://portal.azure.com) to configure autoscale.</span></span>

## <a name="step-8---management-tasks"></a><span data-ttu-id="bdb30-152">Steg 8 - administrativa uppgifter</span><span class="sxs-lookup"><span data-stu-id="bdb30-152">Step 8 - Management tasks</span></span>

<span data-ttu-id="bdb30-153">Du kan behöva köra en eller flera administrativa uppgifter i hela livscykeln för skaluppsättning.</span><span class="sxs-lookup"><span data-stu-id="bdb30-153">Throughout the lifecycle of the scale set, you may need to run one or more management tasks.</span></span> <span data-ttu-id="bdb30-154">Dessutom kanske du vill skapa skript som automatiserar olika livscykel-uppgifter och Azure CLI tillhandahåller ett snabbt sätt att utföra dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bdb30-154">Additionally, you may want to create scripts that automate various lifecycle-tasks, and the Azure CLI provides a quick way to do those tasks.</span></span> <span data-ttu-id="bdb30-155">Här följer några vanliga uppgifter.</span><span class="sxs-lookup"><span data-stu-id="bdb30-155">Here are a few common tasks.</span></span>

### <a name="get-connection-info"></a><span data-ttu-id="bdb30-156">Hämta anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="bdb30-156">Get connection info</span></span>

```azurecli
az vmss list-instance-connection-info --resource-group myResourceGroup --name myScaleSet
```

### <a name="set-instance-count-manual-scale"></a><span data-ttu-id="bdb30-157">Ange instansantal (manuell skala)</span><span class="sxs-lookup"><span data-stu-id="bdb30-157">Set instance count (manual scale)</span></span>

```azurecli
az vmss scale --resource-group myResourceGroup --name myScaleSet --new-capacity 4
```

### <a name="delete-resource-group"></a><span data-ttu-id="bdb30-158">Ta bort resursgruppen</span><span class="sxs-lookup"><span data-stu-id="bdb30-158">Delete resource group</span></span>

<span data-ttu-id="bdb30-159">En resursgrupp också tar du bort alla resurser som ingår i.</span><span class="sxs-lookup"><span data-stu-id="bdb30-159">Deleting a resource group also deletes all resources contained within.</span></span>

```azurecli
az group delete --name myResourceGroup
```

## <a name="next-steps"></a><span data-ttu-id="bdb30-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bdb30-160">Next steps</span></span>
<span data-ttu-id="bdb30-161">Mer information om några av de virtuella skala set-funktionerna som introducerades i den här kursen finns följande information:</span><span class="sxs-lookup"><span data-stu-id="bdb30-161">To learn more about some of the virtual machine scale set features introduced in this tutorial, see the following information:</span></span>

- [<span data-ttu-id="bdb30-162">Översikt över Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="bdb30-162">Azure Virtual Machine Scale Sets overview</span></span>](../../virtual-machine-scale-sets/virtual-machine-scale-sets-overview.md)
- [<span data-ttu-id="bdb30-163">Översikt över Azure Load Balancer</span><span class="sxs-lookup"><span data-stu-id="bdb30-163">Azure Load Balancer overview</span></span>](../../load-balancer/load-balancer-overview.md)
- [<span data-ttu-id="bdb30-164">Styra flödet i nätverkstrafiken med nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="bdb30-164">Control network traffic flow with network security groups</span></span>](../../virtual-network/virtual-networks-nsg.md)