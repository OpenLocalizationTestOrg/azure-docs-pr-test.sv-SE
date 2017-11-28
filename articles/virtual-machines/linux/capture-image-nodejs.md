---
title: "Avbilda en virtuell Azure Linux-dator ska användas som en mall | Microsoft Docs"
description: "Lär dig mer om att fånga och generalisera en avbildning av en Linux-baserade Azure virtuell dator (VM) skapats med Azure Resource Manager-distributionsmodellen."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: e608116f-f478-41be-b787-c2ad91b5a802
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/09/2017
ms.author: iainfou
ms.openlocfilehash: b1164fbd816eea5189786850f096438e32f8f802
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a><span data-ttu-id="83b7f-103">Avbilda en Linux-dator som körs på Azure</span><span class="sxs-lookup"><span data-stu-id="83b7f-103">Capture a Linux virtual machine running on Azure</span></span>
<span data-ttu-id="83b7f-104">Följ stegen i den här artikeln för att generalisera och avbilda dina Azure Linux-dator (VM) i Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="83b7f-104">Follow the steps in this article to generalize and capture your Azure Linux virtual machine (VM) in the Resource Manager deployment model.</span></span> <span data-ttu-id="83b7f-105">När du generaliserar den virtuella datorn, ta bort personlig information och förbereda den virtuella datorn som ska användas som en bild.</span><span class="sxs-lookup"><span data-stu-id="83b7f-105">When you generalize the VM, you remove personal account information and prepare the VM to be used as an image.</span></span> <span data-ttu-id="83b7f-106">Du sedan avbilda en generaliserad virtuell hårddisk (VHD) avbildas för Operativsystemet, virtuella hårddiskar för bifogade datadiskar och en [Resource Manager-mall](../../azure-resource-manager/resource-group-overview.md) för nya VM-distributioner.</span><span class="sxs-lookup"><span data-stu-id="83b7f-106">You then capture a generalized virtual hard disk (VHD) image for the OS, VHDs for attached data disks, and a [Resource Manager template](../../azure-resource-manager/resource-group-overview.md) for new VM deployments.</span></span> <span data-ttu-id="83b7f-107">Den här artikeln beskriver hur du hämta en datoravbildning av virtuell med Azure CLI 1.0 för en virtuell dator med hjälp av ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="83b7f-107">This article details how to capture a VM image with the Azure CLI 1.0 for a VM using unmanaged disks.</span></span> <span data-ttu-id="83b7f-108">Du kan också [avbilda en virtuell dator i Azure hanterade diskar med Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="83b7f-108">You can also [capture a VM using Azure Managed Disks with the Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="83b7f-109">Hanterade diskar hanteras av Azure-plattformen och behöver inte alla förberedelse eller plats att lagra dem.</span><span class="sxs-lookup"><span data-stu-id="83b7f-109">Managed disks are handled by the Azure platform and do not require any preparation or location to store them.</span></span> <span data-ttu-id="83b7f-110">Mer information finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="83b7f-110">For more information, see [Azure Managed Disks overview](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="83b7f-111">Ställ in nätverksresurser för varje ny virtuell dator för att skapa virtuella datorer med hjälp av avbildningen, och Använd mall (en JavaScript Object Notation eller JSON,-fil) för att distribuera från de VHD-avbildningarna.</span><span class="sxs-lookup"><span data-stu-id="83b7f-111">To create VMs using the image, set up network resources for each new VM, and use the template (a JavaScript Object Notation, or JSON, file) to deploy it from the captured VHD images.</span></span> <span data-ttu-id="83b7f-112">På så sätt kan replikera du en virtuell dator med dess aktuella programvarukonfiguration, ungefär samma sätt som du använder avbildningar i Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="83b7f-112">In this way, you can replicate a VM with its current software configuration, similar to the way you use images in the Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="83b7f-113">Om du vill skapa en kopia av din befintliga Linux VM med det särskilda tillståndet för säkerhetskopiering eller felsökning, se [skapar en kopia av en Linux-dator som körs på Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="83b7f-113">If you want to create a copy of your existing Linux VM with its specialized state for backup or debugging, see [Create a copy of a Linux virtual machine running on Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="83b7f-114">Och om du vill överföra en Linux VHD från en lokal virtuell dator finns [överför och skapa en Linux VM från anpassade diskavbildning](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="83b7f-114">And if you want to upload a Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>  

## <a name="cli-versions-to-complete-the-task"></a><span data-ttu-id="83b7f-115">CLI-versioner för att slutföra uppgiften</span><span class="sxs-lookup"><span data-stu-id="83b7f-115">CLI versions to complete the task</span></span>
<span data-ttu-id="83b7f-116">Du kan slutföra uppgiften med någon av följande CLI-versioner:</span><span class="sxs-lookup"><span data-stu-id="83b7f-116">You can complete the task using one of the following CLI versions:</span></span>

- <span data-ttu-id="83b7f-117">[Azure CLI 1.0](#before-you-begin) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="83b7f-117">[Azure CLI 1.0](#before-you-begin) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="83b7f-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) – vår nästa generations CLI för distributionsmodellen resurshantering</span><span class="sxs-lookup"><span data-stu-id="83b7f-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for the resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="83b7f-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="83b7f-119">Before you begin</span></span>
<span data-ttu-id="83b7f-120">Se till att du uppfyller följande krav:</span><span class="sxs-lookup"><span data-stu-id="83b7f-120">Ensure that you meet the following prerequisites:</span></span>

* <span data-ttu-id="83b7f-121">**Azure VM som skapats i Resource Manager-distributionsmodellen** -om du inte har skapat en Linux VM, kan du använda den [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), eller [Resource Manager-mallar ](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="83b7f-121">**Azure VM created in the Resource Manager deployment model** - If you haven't created a Linux VM, you can use the [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), the [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> 
  
    <span data-ttu-id="83b7f-122">Konfigurera den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-122">Configure the VM as needed.</span></span> <span data-ttu-id="83b7f-123">Till exempel [lägga till datadiskar](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), tillämpa uppdateringar och installera program.</span><span class="sxs-lookup"><span data-stu-id="83b7f-123">For example, [add data disks](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), apply updates, and install applications.</span></span> 
* <span data-ttu-id="83b7f-124">**Azure CLI** -installera den [Azure CLI](../../cli-install-nodejs.md) på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="83b7f-124">**Azure CLI** - Install the [Azure CLI](../../cli-install-nodejs.md) on a local computer.</span></span>

## <a name="step-1-remove-the-azure-linux-agent"></a><span data-ttu-id="83b7f-125">Steg 1: Ta bort Azure Linux-agent</span><span class="sxs-lookup"><span data-stu-id="83b7f-125">Step 1: Remove the Azure Linux agent</span></span>
<span data-ttu-id="83b7f-126">Kör först den **waagent** kommandot med de **avetablering** parameter på Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="83b7f-126">First, run the **waagent** command with the **deprovision** parameter on the Linux VM.</span></span> <span data-ttu-id="83b7f-127">Det här kommandot tar bort filer och data så att den virtuella datorn redo för att generalisera.</span><span class="sxs-lookup"><span data-stu-id="83b7f-127">This command deletes files and data to make the VM ready for generalizing.</span></span> <span data-ttu-id="83b7f-128">Mer information finns i [Azure Linux-agenten användarhandboken](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="83b7f-128">For details, see the [Azure Linux Agent user guide](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

1. <span data-ttu-id="83b7f-129">Ansluta till din Linux VM som använder en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="83b7f-129">Connect to your Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="83b7f-130">I fönstret SSH skriver du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="83b7f-130">In the SSH window, type the following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > <span data-ttu-id="83b7f-131">Endast köra kommandot på en virtuell dator som du vill samla in som en bild.</span><span class="sxs-lookup"><span data-stu-id="83b7f-131">Only run this command on a VM that you intend to capture as an image.</span></span> <span data-ttu-id="83b7f-132">Är det inte säkert att avbildningen är avmarkerad av all känslig information eller lämpar sig för omfördelning.</span><span class="sxs-lookup"><span data-stu-id="83b7f-132">It does not guarantee that the image is cleared of all sensitive information or is suitable for redistribution.</span></span>
 
3. <span data-ttu-id="83b7f-133">Typen **y** att fortsätta.</span><span class="sxs-lookup"><span data-stu-id="83b7f-133">Type **y** to continue.</span></span> <span data-ttu-id="83b7f-134">Du kan lägga till den **-tvinga** parametern för att undvika det här steget bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="83b7f-134">You can add the **-force** parameter to avoid this confirmation step.</span></span>
4. <span data-ttu-id="83b7f-135">När kommandot har slutförts genom att skriva **avsluta**.</span><span class="sxs-lookup"><span data-stu-id="83b7f-135">After the command completes, type **exit**.</span></span> <span data-ttu-id="83b7f-136">Det här steget stänger SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="83b7f-136">This step closes the SSH client.</span></span>

## <a name="step-2-capture-the-vm"></a><span data-ttu-id="83b7f-137">Steg 2: Skapa den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="83b7f-137">Step 2: Capture the VM</span></span>
<span data-ttu-id="83b7f-138">Använda Azure CLI för att generalisera och avbilda den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-138">Use the Azure CLI to generalize and capture the VM.</span></span> <span data-ttu-id="83b7f-139">Ersätt exempel parameternamn med egna värden i följande exempel.</span><span class="sxs-lookup"><span data-stu-id="83b7f-139">In the following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="83b7f-140">Exempel parameternamn inkluderar **myResourceGroup**, **myVnet**, och **myVM**.</span><span class="sxs-lookup"><span data-stu-id="83b7f-140">Example parameter names include **myResourceGroup**, **myVnet**, and **myVM**.</span></span>

1. <span data-ttu-id="83b7f-141">Öppna Azure CLI från den lokala datorn och [logga in på Azure-prenumerationen](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="83b7f-141">From your local computer, open the Azure CLI and [login to your Azure subscription](../../xplat-cli-connect.md).</span></span> 
2. <span data-ttu-id="83b7f-142">Kontrollera att du är i läget Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="83b7f-142">Make sure you are in Resource Manager mode.</span></span>
   
    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="83b7f-143">Stänga av den virtuella datorn som du redan avetableras med hjälp av följande kommando:</span><span class="sxs-lookup"><span data-stu-id="83b7f-143">Shut down the VM that you already deprovisioned by using the following command:</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. <span data-ttu-id="83b7f-144">Generalisera den virtuella datorn med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="83b7f-144">Generalize the VM with the following command:</span></span>
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. <span data-ttu-id="83b7f-145">Nu köra den **azure vm-avbildning** kommandot, som samlar in den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-145">Now run the **azure vm capture** command, which captures the VM.</span></span> <span data-ttu-id="83b7f-146">I följande exempel avbildningen virtuella hårddiskar till med namn som börjar med **MyVHDNamePrefix**, och **-t** alternativet anger en sökväg till mallen **MyTemplate.json**.</span><span class="sxs-lookup"><span data-stu-id="83b7f-146">In the following example, the image VHDs are captured with names beginning with **MyVHDNamePrefix**, and the **-t** option specifies a path to the template **MyTemplate.json**.</span></span> 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="83b7f-147">VHD-avbildningsfiler skapas som standard i samma lagringskonto som används för den ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-147">The image VHD files get created by default in the same storage account that the original VM used.</span></span> <span data-ttu-id="83b7f-148">Använd den *samma lagringskonto* att lagra de virtuella hårddiskarna för alla nya virtuella datorer som du skapar från avbildningen.</span><span class="sxs-lookup"><span data-stu-id="83b7f-148">Use the *same storage account* to store the VHDs for any new VMs you create from the image.</span></span> 

6. <span data-ttu-id="83b7f-149">Öppna JSON-mall för att hitta platsen för en avbildning i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="83b7f-149">To find the location of a captured image, open the JSON template in a text editor.</span></span> <span data-ttu-id="83b7f-150">I den **storageProfile**, hitta den **uri** av den **bild** finns i den **system** behållare.</span><span class="sxs-lookup"><span data-stu-id="83b7f-150">In the **storageProfile**, find the **uri** of the **image** located in the **system** container.</span></span> <span data-ttu-id="83b7f-151">Till exempel liknar URI för diskavbildning OS`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="83b7f-151">For example, the URI of the OS disk image is similar to `https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>

## <a name="step-3-create-a-vm-from-the-captured-image"></a><span data-ttu-id="83b7f-152">Steg 3: Skapa en virtuell dator från avbildningen</span><span class="sxs-lookup"><span data-stu-id="83b7f-152">Step 3: Create a VM from the captured image</span></span>
<span data-ttu-id="83b7f-153">Nu använda avbildningen med en mall för att skapa en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="83b7f-153">Now use the image with a template to create a Linux VM.</span></span> <span data-ttu-id="83b7f-154">Dessa steg visar hur du använder Azure CLI och JSON-fil-mall som du hämtat för att skapa den virtuella datorn i ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="83b7f-154">These steps show you how to use the Azure CLI and the JSON file template you captured to create the VM in a new virtual network.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="83b7f-155">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="83b7f-155">Create network resources</span></span>
<span data-ttu-id="83b7f-156">Om du vill använda en mall måste du först konfigurera ett virtuellt nätverk och nätverkskort för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-156">To use the template, you first need to set up a virtual network and NIC for your new VM.</span></span> <span data-ttu-id="83b7f-157">Vi rekommenderar att du skapar en resursgrupp för dessa resurser i den plats där VM-avbildning lagrad.</span><span class="sxs-lookup"><span data-stu-id="83b7f-157">We recommend you create a resource group for these resources in the location where your VM image is stored.</span></span> <span data-ttu-id="83b7f-158">Kör kommandon som liknar följande, ersätta namnen för dina resurser och en lämplig Azure plats (”centralus” i de här kommandona):</span><span class="sxs-lookup"><span data-stu-id="83b7f-158">Run commands similar to the following, substituting names for your resources and an appropriate Azure location ("centralus" in these commands):</span></span>

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-the-id-of-the-nic"></a><span data-ttu-id="83b7f-159">Hämta Id för nätverkskortet</span><span class="sxs-lookup"><span data-stu-id="83b7f-159">Get the Id of the NIC</span></span>
<span data-ttu-id="83b7f-160">Om du vill distribuera en virtuell dator från avbildningen med hjälp av JSON som du sparade under hämtning måste Id för nätverkskortet.</span><span class="sxs-lookup"><span data-stu-id="83b7f-160">To deploy a VM from the image by using the JSON you saved during capture, you need the Id of the NIC.</span></span> <span data-ttu-id="83b7f-161">Skaffa det genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="83b7f-161">Obtain it by running the following command:</span></span>

```azurecli
azure network nic show myResourceGroup1 myNIC
```

<span data-ttu-id="83b7f-162">Den **Id** i utdata liknar`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span><span class="sxs-lookup"><span data-stu-id="83b7f-162">The **Id** in the output is similar to `/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span></span>

### <a name="create-a-vm"></a><span data-ttu-id="83b7f-163">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="83b7f-163">Create a VM</span></span>
<span data-ttu-id="83b7f-164">Nu ska du köra följande kommando för att skapa den virtuella datorn från den VM-avbildningen.</span><span class="sxs-lookup"><span data-stu-id="83b7f-164">Now run the following command to create your VM from the captured VM image.</span></span> <span data-ttu-id="83b7f-165">Använd den **-f** parametern för att ange sökvägen till mallen JSON-fil som du sparade.</span><span class="sxs-lookup"><span data-stu-id="83b7f-165">Use the **-f** parameter to specify the path to the template JSON file you saved.</span></span>

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

<span data-ttu-id="83b7f-166">Kommandots utdata uppmanas du att ange ett nytt VM-namn, admin-användarnamn och lösenord och Id för det nätverkskort som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="83b7f-166">In the command output, you are prompted to supply a new VM name, the admin user name and password, and the Id of the NIC you created previously.</span></span>

```bash
info:    Executing command group deployment create
info:    Supply values for the following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

<span data-ttu-id="83b7f-167">I följande exempel visar vad som visas för en lyckad distribution:</span><span class="sxs-lookup"><span data-stu-id="83b7f-167">The following sample shows what you see for a successful deployment:</span></span>

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment to complete
data:    DeploymentName     : MyDeployment
data:    ResourceGroupName  : MyResourceGroup1
data:    ProvisioningState  : Succeeded
data:    Timestamp          : xxxxxxx
data:    Mode               : Incremental
data:    Name                Type          Value

data:    ------------------  ------------  -------------------------------------

data:    vmName              String        myNewVM

data:    vmSize              String        Standard_D1

data:    adminUserName       String        myAdminuser

data:    adminPassword       SecureString  undefined

data:    networkInterfaceId  String        /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
info:    group deployment create command OK
```

### <a name="verify-the-deployment"></a><span data-ttu-id="83b7f-168">Kontrollera distributionen</span><span class="sxs-lookup"><span data-stu-id="83b7f-168">Verify the deployment</span></span>
<span data-ttu-id="83b7f-169">Nu SSH till den virtuella datorn som du skapade för att kontrollera distributionen och börja använda den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-169">Now SSH to the virtual machine you created to verify the deployment and start using the new VM.</span></span> <span data-ttu-id="83b7f-170">Hitta IP-adressen för den virtuella datorn som du skapade genom att köra följande kommando för att ansluta via SSH:</span><span class="sxs-lookup"><span data-stu-id="83b7f-170">To connect via SSH, find the IP address of the VM you created by running the following command:</span></span>

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

<span data-ttu-id="83b7f-171">Den offentliga IP-adressen visas i kommandot.</span><span class="sxs-lookup"><span data-stu-id="83b7f-171">The public IP address is listed in the command output.</span></span> <span data-ttu-id="83b7f-172">Som standard ansluter till Linux-VM av SSH på port 22.</span><span class="sxs-lookup"><span data-stu-id="83b7f-172">By default you connect to the Linux VM by SSH on port 22.</span></span>

## <a name="create-additional-vms"></a><span data-ttu-id="83b7f-173">Skapa ytterligare virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="83b7f-173">Create additional VMs</span></span>
<span data-ttu-id="83b7f-174">Använd den fångade avbildningen och mallen för att distribuera ytterligare virtuella datorer med hjälp av stegen i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="83b7f-174">Use the captured image and template to deploy additional VMs using the steps in the preceding section.</span></span> <span data-ttu-id="83b7f-175">Andra alternativ för att skapa virtuella datorer från avbildningen är en mall i Snabbstart eller körs den **azure vm skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="83b7f-175">Other options to create VMs from the image include using a quickstart template or running the **azure vm create** command.</span></span>

### <a name="use-the-captured-template"></a><span data-ttu-id="83b7f-176">Använda mallen avbildade</span><span class="sxs-lookup"><span data-stu-id="83b7f-176">Use the captured template</span></span>
<span data-ttu-id="83b7f-177">Så här (beskrivs i föregående avsnitt) för att använda den fångade avbildningen och mallen:</span><span class="sxs-lookup"><span data-stu-id="83b7f-177">To use the captured image and template, follow these steps (detailed in the preceding section):</span></span>

* <span data-ttu-id="83b7f-178">Se till att VM-avbildning finns i samma lagringskonto som är värd för den Virtuella datorns virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="83b7f-178">Ensure that your VM image is in the same storage account that hosts your VM's VHD.</span></span>
* <span data-ttu-id="83b7f-179">Kopiera mallen JSON-fil och ange ett unikt namn för OS-disken på den nya VM VHD (eller virtuella hårddiskar).</span><span class="sxs-lookup"><span data-stu-id="83b7f-179">Copy the template JSON file and specify a unique name for the OS disk of the new VM's VHD (or VHDs).</span></span> <span data-ttu-id="83b7f-180">Till exempel i den **storageProfile**under **vhd**i **uri**, ange ett unikt namn för den **osDisk** VHD som liknar`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="83b7f-180">For example, in the **storageProfile**, under **vhd**, in **uri**, specify a unique name for the **osDisk** VHD, similar to `https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>
* <span data-ttu-id="83b7f-181">Skapa ett nätverkskort i samma eller ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="83b7f-181">Create a NIC in either the same or a different virtual network.</span></span>
* <span data-ttu-id="83b7f-182">Skapa en distribution i resursgruppen som du ställer in det virtuella nätverket med ändrade mallen JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="83b7f-182">Using the modified template JSON file, create a deployment in the resource group in which you set up the virtual network.</span></span>

### <a name="use-a-quickstart-template"></a><span data-ttu-id="83b7f-183">Använd en mall för Snabbstart</span><span class="sxs-lookup"><span data-stu-id="83b7f-183">Use a quickstart template</span></span>
<span data-ttu-id="83b7f-184">Om du vill att nätverket ställa in automatiskt när du skapar en virtuell dator från avbildningen kan du ange resurserna i en mall.</span><span class="sxs-lookup"><span data-stu-id="83b7f-184">If you want the network set up automatically when you create a VM from the image, you can specify those resources in a template.</span></span> <span data-ttu-id="83b7f-185">Se exempelvis den [101-vm-från--användaravbildning mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) från GitHub.</span><span class="sxs-lookup"><span data-stu-id="83b7f-185">For example, see the [101-vm-from-user-image template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) from GitHub.</span></span> <span data-ttu-id="83b7f-186">Den här mallen skapar en virtuell dator från den anpassade avbildningen och nödvändiga virtuella nätverk, offentlig IP-adress och NIC-resurser.</span><span class="sxs-lookup"><span data-stu-id="83b7f-186">This template creates a VM from your custom image and the necessary virtual network, public IP address, and NIC resources.</span></span> <span data-ttu-id="83b7f-187">En genomgång av mallen i Azure portal finns [hur du skapar en virtuell dator från en anpassad avbildning med hjälp av en Resource Manager-mall](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span><span class="sxs-lookup"><span data-stu-id="83b7f-187">For a walkthrough of using the template in the Azure portal, see [How to create a virtual machine from a custom image using a Resource Manager template](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span></span>

### <a name="use-the-azure-vm-create-command"></a><span data-ttu-id="83b7f-188">Använd azure vm skapa kommando</span><span class="sxs-lookup"><span data-stu-id="83b7f-188">Use the azure vm create command</span></span>
<span data-ttu-id="83b7f-189">Vanligtvis är det enklast att använda en Resource Manager-mall för att skapa en virtuell dator från avbildningen.</span><span class="sxs-lookup"><span data-stu-id="83b7f-189">Usually it's easiest to use a Resource Manager template to create a VM from the image.</span></span> <span data-ttu-id="83b7f-190">Du kan dock skapa den virtuella datorn *imperatively* med hjälp av den **azure vm skapa** kommandot med den **-Q** (**--bild urn**) parametern.</span><span class="sxs-lookup"><span data-stu-id="83b7f-190">However, you can create the VM *imperatively* by using the **azure vm create** command with the **-Q** (**--image-urn**) parameter.</span></span> <span data-ttu-id="83b7f-191">Om du använder den här metoden kan du också ange den **-d** (**--os-disk-vhd**) parameter för att ange platsen för OS VHD-filen för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-191">If you use this method, you also pass the **-d** (**--os-disk-vhd**) parameter to specify the location of the OS .vhd file for the new VM.</span></span> <span data-ttu-id="83b7f-192">Den här filen måste vara i behållaren virtuella hårddiskar för det lagringskonto där VHD-avbildningsfilen lagras.</span><span class="sxs-lookup"><span data-stu-id="83b7f-192">This file must be in the vhds container of the storage account where the image VHD file is stored.</span></span> <span data-ttu-id="83b7f-193">Kommandot kopierar VHD för den nya virtuella datorn automatiskt till den **virtuella hårddiskar** behållare.</span><span class="sxs-lookup"><span data-stu-id="83b7f-193">The command copies the VHD for the new VM automatically to the **vhds** container.</span></span>

<span data-ttu-id="83b7f-194">Innan du kör **azure vm skapa** med avbildningen, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="83b7f-194">Before running **azure vm create** with the image, complete the following steps:</span></span>

1. <span data-ttu-id="83b7f-195">Skapa en resursgrupp eller identifiera en befintlig resursgrupp för distributionen.</span><span class="sxs-lookup"><span data-stu-id="83b7f-195">Create a resource group, or identify an existing resource group for the deployment.</span></span>
2. <span data-ttu-id="83b7f-196">Skapa en offentlig IP-adressresurs och en resurs för nätverkskort för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="83b7f-196">Create a public IP address resource and a NIC resource for the new VM.</span></span> <span data-ttu-id="83b7f-197">Steg för att skapa ett virtuellt nätverk, offentlig IP-adress och NIC med hjälp av CLI, finns i tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="83b7f-197">For steps to create a virtual network, public IP address, and NIC by using the CLI, see earlier in this article.</span></span> <span data-ttu-id="83b7f-198">(**azure vm skapa** kan också skapa ett nätverkskort, men du måste lägga till extraparametrar för ett virtuellt nätverk och undernät.)</span><span class="sxs-lookup"><span data-stu-id="83b7f-198">(**azure vm create** can also create a NIC, but you need to pass additional parameters for a virtual network and subnet.)</span></span>

<span data-ttu-id="83b7f-199">Kör ett kommando som klarar URI: er för både den nya OS VHD-filen och den befintliga avbildningen.</span><span class="sxs-lookup"><span data-stu-id="83b7f-199">Then run a command that passes URIs to both the new OS VHD file and the existing image.</span></span> <span data-ttu-id="83b7f-200">I det här exemplet en storlek Standard_A1 VM har skapats i östra USA.</span><span class="sxs-lookup"><span data-stu-id="83b7f-200">In this example, a size Standard_A1 VM is created in the East US region.</span></span>

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

<span data-ttu-id="83b7f-201">Ytterligare alternativ för, kör `azure help vm create`.</span><span class="sxs-lookup"><span data-stu-id="83b7f-201">For additional command options, run `azure help vm create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="83b7f-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="83b7f-202">Next steps</span></span>
<span data-ttu-id="83b7f-203">För att hantera dina virtuella datorer med CLI, se uppgifterna i [distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och Azure CLI](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="83b7f-203">To manage your VMs with the CLI, see the tasks in [Deploy and manage virtual machines by using Azure Resource Manager templates and the Azure CLI](create-ssh-secured-vm-from-template.md).</span></span>

