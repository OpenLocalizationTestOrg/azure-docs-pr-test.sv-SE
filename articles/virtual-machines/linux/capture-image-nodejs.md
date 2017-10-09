---
title: aaaCapture en toouse virtuella Azure Linux-datorn som en mall | Microsoft Docs
description: "Lär dig hur toocapture och generalisera en avbildning av en Linux-baserade Azure virtuell dator (VM) skapas med hello Azure Resource Manager-distributionsmodellen."
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
ms.openlocfilehash: 877eee5c842bebe80e755c2240cdaaef4ade6ff5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="capture-a-linux-virtual-machine-running-on-azure"></a><span data-ttu-id="708d5-103">Avbilda en Linux-dator som körs på Azure</span><span class="sxs-lookup"><span data-stu-id="708d5-103">Capture a Linux virtual machine running on Azure</span></span>
<span data-ttu-id="708d5-104">Följ hello stegen i den här artikeln toogeneralize och avbilda dina Azure Linux-dator (VM) i hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="708d5-104">Follow hello steps in this article toogeneralize and capture your Azure Linux virtual machine (VM) in hello Resource Manager deployment model.</span></span> <span data-ttu-id="708d5-105">När du generaliserar hello VM ta bort personlig information och förbereda hello VM toobe används som en bild.</span><span class="sxs-lookup"><span data-stu-id="708d5-105">When you generalize hello VM, you remove personal account information and prepare hello VM toobe used as an image.</span></span> <span data-ttu-id="708d5-106">Du sedan avbilda en generaliserad virtuell hårddisk (VHD) bild för hello OS, virtuella hårddiskar för bifogade datadiskar och en [Resource Manager-mall](../../azure-resource-manager/resource-group-overview.md) för nya VM-distributioner.</span><span class="sxs-lookup"><span data-stu-id="708d5-106">You then capture a generalized virtual hard disk (VHD) image for hello OS, VHDs for attached data disks, and a [Resource Manager template](../../azure-resource-manager/resource-group-overview.md) for new VM deployments.</span></span> <span data-ttu-id="708d5-107">Den här artikeln beskrivs hur toocapture en virtuell dator av avbildning med hello Azure CLI 1.0 för en virtuell dator med hjälp av ohanterade diskar.</span><span class="sxs-lookup"><span data-stu-id="708d5-107">This article details how toocapture a VM image with hello Azure CLI 1.0 for a VM using unmanaged disks.</span></span> <span data-ttu-id="708d5-108">Du kan också [avbilda en virtuell dator med hjälp av Azure hanterade diskar med hello Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="708d5-108">You can also [capture a VM using Azure Managed Disks with hello Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="708d5-109">Hanterade diskar som hanteras av hello Azure-plattformen och kräver inte någon förberedelse eller plats toostore dem.</span><span class="sxs-lookup"><span data-stu-id="708d5-109">Managed disks are handled by hello Azure platform and do not require any preparation or location toostore them.</span></span> <span data-ttu-id="708d5-110">Mer information finns i [Översikt över Azure Managed Disks](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="708d5-110">For more information, see [Azure Managed Disks overview](../windows/managed-disks-overview.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> 

<span data-ttu-id="708d5-111">toocreate virtuella datorer med hjälp av hello avbildning konfigurera nätverksresurser för varje ny virtuell dator och använda hello mall (en JavaScript Object Notation eller JSON,-fil) toodeploy från hello avbildas VHD-avbildningar.</span><span class="sxs-lookup"><span data-stu-id="708d5-111">toocreate VMs using hello image, set up network resources for each new VM, and use hello template (a JavaScript Object Notation, or JSON, file) toodeploy it from hello captured VHD images.</span></span> <span data-ttu-id="708d5-112">På så sätt kan replikera du en virtuell dator med dess aktuella programvarukonfiguration, liknande toohello sätt du använda bilder i hello Azure Marketplace.</span><span class="sxs-lookup"><span data-stu-id="708d5-112">In this way, you can replicate a VM with its current software configuration, similar toohello way you use images in hello Azure Marketplace.</span></span>

> [!TIP]
> <span data-ttu-id="708d5-113">Om du vill toocreate en kopia av din befintliga Linux VM med det särskilda tillståndet för säkerhetskopiering eller felsökning, se [skapar en kopia av en Linux-dator som körs på Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="708d5-113">If you want toocreate a copy of your existing Linux VM with its specialized state for backup or debugging, see [Create a copy of a Linux virtual machine running on Azure](copy-vm.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="708d5-114">Och om du vill tooupload en Linux VHD från en lokal virtuell dator, se [överför och skapa en Linux VM från anpassade diskavbildning](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="708d5-114">And if you want tooupload a Linux VHD from an on-premises VM, see [Upload and create a Linux VM from custom disk image](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>  

## <a name="cli-versions-toocomplete-hello-task"></a><span data-ttu-id="708d5-115">CLI versioner toocomplete hello aktivitet</span><span class="sxs-lookup"><span data-stu-id="708d5-115">CLI versions toocomplete hello task</span></span>
<span data-ttu-id="708d5-116">Du kan göra hello med hjälp av något av följande versioner av CLI hello:</span><span class="sxs-lookup"><span data-stu-id="708d5-116">You can complete hello task using one of hello following CLI versions:</span></span>

- <span data-ttu-id="708d5-117">[Azure CLI 1.0](#before-you-begin) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="708d5-117">[Azure CLI 1.0](#before-you-begin) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="708d5-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="708d5-118">[Azure CLI 2.0](capture-image.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) - our next generation CLI for hello resource management deployment model</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="708d5-119">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="708d5-119">Before you begin</span></span>
<span data-ttu-id="708d5-120">Se till att du uppfyller hello följande krav:</span><span class="sxs-lookup"><span data-stu-id="708d5-120">Ensure that you meet hello following prerequisites:</span></span>

* <span data-ttu-id="708d5-121">**Azure VM som skapats i hello Resource Manager-distributionsmodellen** -om du inte har skapat en Linux VM, kan du använda hello [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), eller [Resource Manager mallar](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="708d5-121">**Azure VM created in hello Resource Manager deployment model** - If you haven't created a Linux VM, you can use hello [portal](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), hello [Azure CLI](quick-create-cli.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), or [Resource Manager templates](create-ssh-secured-vm-from-template.md).</span></span> 
  
    <span data-ttu-id="708d5-122">Konfigurera hello VM efter behov.</span><span class="sxs-lookup"><span data-stu-id="708d5-122">Configure hello VM as needed.</span></span> <span data-ttu-id="708d5-123">Till exempel [lägga till datadiskar](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), tillämpa uppdateringar och installera program.</span><span class="sxs-lookup"><span data-stu-id="708d5-123">For example, [add data disks](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json), apply updates, and install applications.</span></span> 
* <span data-ttu-id="708d5-124">**Azure CLI** -installera hello [Azure CLI](../../cli-install-nodejs.md) på en lokal dator.</span><span class="sxs-lookup"><span data-stu-id="708d5-124">**Azure CLI** - Install hello [Azure CLI](../../cli-install-nodejs.md) on a local computer.</span></span>

## <a name="step-1-remove-hello-azure-linux-agent"></a><span data-ttu-id="708d5-125">Steg 1: Ta bort hello Azure Linux-agent</span><span class="sxs-lookup"><span data-stu-id="708d5-125">Step 1: Remove hello Azure Linux agent</span></span>
<span data-ttu-id="708d5-126">Kör först hello **waagent** med hello **avetablering** parameter på hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="708d5-126">First, run hello **waagent** command with hello **deprovision** parameter on hello Linux VM.</span></span> <span data-ttu-id="708d5-127">Det här kommandot tar bort filer och data toomake hello VM redo för att generalisera.</span><span class="sxs-lookup"><span data-stu-id="708d5-127">This command deletes files and data toomake hello VM ready for generalizing.</span></span> <span data-ttu-id="708d5-128">Mer information finns i hello [Azure Linux-agenten användarhandboken](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="708d5-128">For details, see hello [Azure Linux Agent user guide](../windows/agent-user-guide.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span>

1. <span data-ttu-id="708d5-129">Ansluta tooyour Linux VM som använder en SSH-klient.</span><span class="sxs-lookup"><span data-stu-id="708d5-129">Connect tooyour Linux VM using an SSH client.</span></span>
2. <span data-ttu-id="708d5-130">Ange följande kommando hello i hello SSH fönstret:</span><span class="sxs-lookup"><span data-stu-id="708d5-130">In hello SSH window, type hello following command:</span></span>
   
    ```bash
    sudo waagent -deprovision+user
    ```
   > [!NOTE]
   > <span data-ttu-id="708d5-131">Endast köra kommandot på en virtuell dator som du avser toocapture som en bild.</span><span class="sxs-lookup"><span data-stu-id="708d5-131">Only run this command on a VM that you intend toocapture as an image.</span></span> <span data-ttu-id="708d5-132">Det garanterar inte hello avbildningen rensas av all känslig information eller lämpar sig för omfördelning.</span><span class="sxs-lookup"><span data-stu-id="708d5-132">It does not guarantee that hello image is cleared of all sensitive information or is suitable for redistribution.</span></span>
 
3. <span data-ttu-id="708d5-133">Typen **y** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="708d5-133">Type **y** toocontinue.</span></span> <span data-ttu-id="708d5-134">Du kan lägga till hello **-tvinga** parametern tooavoid åtgärden bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="708d5-134">You can add hello **-force** parameter tooavoid this confirmation step.</span></span>
4. <span data-ttu-id="708d5-135">När hello-kommandot har slutförts genom att skriva **avsluta**.</span><span class="sxs-lookup"><span data-stu-id="708d5-135">After hello command completes, type **exit**.</span></span> <span data-ttu-id="708d5-136">Det här steget stänger hello SSH-klienten.</span><span class="sxs-lookup"><span data-stu-id="708d5-136">This step closes hello SSH client.</span></span>

## <a name="step-2-capture-hello-vm"></a><span data-ttu-id="708d5-137">Steg 2: Samla in hello VM</span><span class="sxs-lookup"><span data-stu-id="708d5-137">Step 2: Capture hello VM</span></span>
<span data-ttu-id="708d5-138">Använd hello Azure CLI toogeneralize och avbilda hello VM.</span><span class="sxs-lookup"><span data-stu-id="708d5-138">Use hello Azure CLI toogeneralize and capture hello VM.</span></span> <span data-ttu-id="708d5-139">Följande exempel Ersätt exempel parameternamn i hello med egna värden.</span><span class="sxs-lookup"><span data-stu-id="708d5-139">In hello following examples, replace example parameter names with your own values.</span></span> <span data-ttu-id="708d5-140">Exempel parameternamn inkluderar **myResourceGroup**, **myVnet**, och **myVM**.</span><span class="sxs-lookup"><span data-stu-id="708d5-140">Example parameter names include **myResourceGroup**, **myVnet**, and **myVM**.</span></span>

1. <span data-ttu-id="708d5-141">Öppna hello Azure CLI från den lokala datorn och [inloggning tooyour Azure-prenumeration](../../xplat-cli-connect.md).</span><span class="sxs-lookup"><span data-stu-id="708d5-141">From your local computer, open hello Azure CLI and [login tooyour Azure subscription](../../xplat-cli-connect.md).</span></span> 
2. <span data-ttu-id="708d5-142">Kontrollera att du är i läget Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="708d5-142">Make sure you are in Resource Manager mode.</span></span>
   
    ```azurecli
    azure config mode arm
    ```
3. <span data-ttu-id="708d5-143">Stänga av hello virtuell dator som du redan avetableras med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="708d5-143">Shut down hello VM that you already deprovisioned by using hello following command:</span></span>
   
    ```azurecli
    azure vm deallocate -g myResourceGroup -n myVM
    ```
4. <span data-ttu-id="708d5-144">Generalisera hello VM med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="708d5-144">Generalize hello VM with hello following command:</span></span>
   
    ```azurecli
    azure vm generalize -g myResourceGroup -n myVM
    ```
5. <span data-ttu-id="708d5-145">Kör nu hello **azure vm-avbildning** kommando, som registrerar hello VM.</span><span class="sxs-lookup"><span data-stu-id="708d5-145">Now run hello **azure vm capture** command, which captures hello VM.</span></span> <span data-ttu-id="708d5-146">I följande exempel hello, hello avbildningen virtuella hårddiskar till med namn som börjar med **MyVHDNamePrefix**, och hello **-t** alternativet anger en sökväg toohello mall **MyTemplate.json**.</span><span class="sxs-lookup"><span data-stu-id="708d5-146">In hello following example, hello image VHDs are captured with names beginning with **MyVHDNamePrefix**, and hello **-t** option specifies a path toohello template **MyTemplate.json**.</span></span> 
   
    ```azurecli
    azure vm capture -g myResourceGroup -n myVM -p myVHDNamePrefix -t myTemplate.json
    ```
   
   > [!IMPORTANT]
   > <span data-ttu-id="708d5-147">hello avbildningen VHD-filer skapas som standard i hello används samma lagringskonto som hello ursprungliga virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="708d5-147">hello image VHD files get created by default in hello same storage account that hello original VM used.</span></span> <span data-ttu-id="708d5-148">Använd hello *samma lagringskonto* toostore hello virtuella hårddiskar för alla nya virtuella datorer som du skapar från hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="708d5-148">Use hello *same storage account* toostore hello VHDs for any new VMs you create from hello image.</span></span> 

6. <span data-ttu-id="708d5-149">toofind hello platsen för avbildning öppna hello JSON-mall i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="708d5-149">toofind hello location of a captured image, open hello JSON template in a text editor.</span></span> <span data-ttu-id="708d5-150">I hello **storageProfile**, hitta hello **uri** av hello **bild** finns i hello **system** behållare.</span><span class="sxs-lookup"><span data-stu-id="708d5-150">In hello **storageProfile**, find hello **uri** of hello **image** located in hello **system** container.</span></span> <span data-ttu-id="708d5-151">Till exempel är hello diskavbildning hello OS-URI liknande för`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="708d5-151">For example, hello URI of hello OS disk image is similar too`https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>

## <a name="step-3-create-a-vm-from-hello-captured-image"></a><span data-ttu-id="708d5-152">Steg 3: Skapa en virtuell dator från hello avbildas avbildning</span><span class="sxs-lookup"><span data-stu-id="708d5-152">Step 3: Create a VM from hello captured image</span></span>
<span data-ttu-id="708d5-153">Nu använda hello avbildning med en mall toocreate en Linux VM.</span><span class="sxs-lookup"><span data-stu-id="708d5-153">Now use hello image with a template toocreate a Linux VM.</span></span> <span data-ttu-id="708d5-154">Dessa steg visar hur toouse hello Azure CLI och hello JSON mall du hämtat toocreate hello VM i ett nytt virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="708d5-154">These steps show you how toouse hello Azure CLI and hello JSON file template you captured toocreate hello VM in a new virtual network.</span></span>

### <a name="create-network-resources"></a><span data-ttu-id="708d5-155">Skapa nätverksresurser</span><span class="sxs-lookup"><span data-stu-id="708d5-155">Create network resources</span></span>
<span data-ttu-id="708d5-156">toouse hello mall måste du först tooset ett virtuellt nätverk och nätverkskort för den nya virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="708d5-156">toouse hello template, you first need tooset up a virtual network and NIC for your new VM.</span></span> <span data-ttu-id="708d5-157">Vi rekommenderar att du skapar en resursgrupp för dessa resurser i hello plats där VM-avbildning lagras.</span><span class="sxs-lookup"><span data-stu-id="708d5-157">We recommend you create a resource group for these resources in hello location where your VM image is stored.</span></span> <span data-ttu-id="708d5-158">Kör kommandon liknande toohello efter, ersätter namn för dina resurser och en lämplig Azure plats (”centralus” i de här kommandona):</span><span class="sxs-lookup"><span data-stu-id="708d5-158">Run commands similar toohello following, substituting names for your resources and an appropriate Azure location ("centralus" in these commands):</span></span>

```azurecli
azure group create myResourceGroup1 -l "centralus"

azure network vnet create myResourceGroup1 myVnet -l "centralus"

azure network vnet subnet create myResourceGroup1 myVnet mySubnet

azure network public-ip create myResourceGroup1 myPublicIP -l "centralus"

azure network nic create myResourceGroup1 myNIC -k mySubnet -m myVnet -p myPublicIP -l "centralus"
```

### <a name="get-hello-id-of-hello-nic"></a><span data-ttu-id="708d5-159">Hämta hello-Id för hello NIC</span><span class="sxs-lookup"><span data-stu-id="708d5-159">Get hello Id of hello NIC</span></span>
<span data-ttu-id="708d5-160">toodeploy en virtuell dator från hello avbildningen med hjälp av hello JSON som du sparade under hämtning, behöver du hello-Id för hello NIC.</span><span class="sxs-lookup"><span data-stu-id="708d5-160">toodeploy a VM from hello image by using hello JSON you saved during capture, you need hello Id of hello NIC.</span></span> <span data-ttu-id="708d5-161">Du kan skaffa det genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="708d5-161">Obtain it by running hello following command:</span></span>

```azurecli
azure network nic show myResourceGroup1 myNIC
```

<span data-ttu-id="708d5-162">Hej **Id** i hello utdata är liknande för`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span><span class="sxs-lookup"><span data-stu-id="708d5-162">hello **Id** in hello output is similar too`/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/MyResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic`</span></span>

### <a name="create-a-vm"></a><span data-ttu-id="708d5-163">Skapa en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="708d5-163">Create a VM</span></span>
<span data-ttu-id="708d5-164">Nu kör hello följande kommando toocreate avbildas den virtuella datorn från hello VM-avbildning.</span><span class="sxs-lookup"><span data-stu-id="708d5-164">Now run hello following command toocreate your VM from hello captured VM image.</span></span> <span data-ttu-id="708d5-165">Använd hello **-f** parametern toospecify hello sökvägen toohello mallens JSON-filen som du sparade.</span><span class="sxs-lookup"><span data-stu-id="708d5-165">Use hello **-f** parameter toospecify hello path toohello template JSON file you saved.</span></span>

```azurecli
azure group deployment create myResourceGroup1 MyDeployment -f MyTemplate.json
```

<span data-ttu-id="708d5-166">I hello kommandoutdata du tillfrågas toosupply ett nytt namn på virtuell dator, hello admin-användarnamn och lösenord och hello-Id för hello NIC som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="708d5-166">In hello command output, you are prompted toosupply a new VM name, hello admin user name and password, and hello Id of hello NIC you created previously.</span></span>

```bash
info:    Executing command group deployment create
info:    Supply values for hello following parameters
vmName: myNewVM
adminUserName: myAdminuser
adminPassword: ********
networkInterfaceId: /subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resource Groups/myResourceGroup1/providers/Microsoft.Network/networkInterfaces/myNic
```

<span data-ttu-id="708d5-167">hello som följande exempel visar vad som visas för en lyckad distribution:</span><span class="sxs-lookup"><span data-stu-id="708d5-167">hello following sample shows what you see for a successful deployment:</span></span>

```bash
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment xxxxxxx
+ Waiting for deployment toocomplete
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

### <a name="verify-hello-deployment"></a><span data-ttu-id="708d5-168">Kontrollera distributionen av hello</span><span class="sxs-lookup"><span data-stu-id="708d5-168">Verify hello deployment</span></span>
<span data-ttu-id="708d5-169">Nu SSH toohello virtuella datorn du har skapat tooverify hello distribution och börjar använda hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="708d5-169">Now SSH toohello virtual machine you created tooverify hello deployment and start using hello new VM.</span></span> <span data-ttu-id="708d5-170">tooconnect via SSH, hitta hello IP-adressen för hello VM som du skapade genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="708d5-170">tooconnect via SSH, find hello IP address of hello VM you created by running hello following command:</span></span>

```azurecli
azure network public-ip show myResourceGroup1 myPublicIP
```

<span data-ttu-id="708d5-171">hello offentliga IP-adressen listas i hello kommandoutdata.</span><span class="sxs-lookup"><span data-stu-id="708d5-171">hello public IP address is listed in hello command output.</span></span> <span data-ttu-id="708d5-172">Som standard ansluter toohello Linux VM av SSH på port 22.</span><span class="sxs-lookup"><span data-stu-id="708d5-172">By default you connect toohello Linux VM by SSH on port 22.</span></span>

## <a name="create-additional-vms"></a><span data-ttu-id="708d5-173">Skapa ytterligare virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="708d5-173">Create additional VMs</span></span>
<span data-ttu-id="708d5-174">Använd hello avbildas avbildningen och mallen toodeploy ytterligare virtuella datorer med hello stegen i föregående avsnitt hello.</span><span class="sxs-lookup"><span data-stu-id="708d5-174">Use hello captured image and template toodeploy additional VMs using hello steps in hello preceding section.</span></span> <span data-ttu-id="708d5-175">Andra alternativ toocreate virtuella datorer från hello bilden innehåller en mall i Snabbstart eller kör hello **azure vm skapa** kommando.</span><span class="sxs-lookup"><span data-stu-id="708d5-175">Other options toocreate VMs from hello image include using a quickstart template or running hello **azure vm create** command.</span></span>

### <a name="use-hello-captured-template"></a><span data-ttu-id="708d5-176">Använd hello avbildade mall</span><span class="sxs-lookup"><span data-stu-id="708d5-176">Use hello captured template</span></span>
<span data-ttu-id="708d5-177">toouse hello avbildas avbildningen och mallen, Följ dessa steg (beskrivs i föregående avsnitt hello):</span><span class="sxs-lookup"><span data-stu-id="708d5-177">toouse hello captured image and template, follow these steps (detailed in hello preceding section):</span></span>

* <span data-ttu-id="708d5-178">Se till att VM-avbildning hello samma lagringskonto som är värd för den Virtuella datorns virtuella Hårddisken.</span><span class="sxs-lookup"><span data-stu-id="708d5-178">Ensure that your VM image is in hello same storage account that hosts your VM's VHD.</span></span>
* <span data-ttu-id="708d5-179">Kopiera hello mallen JSON-fil och ange ett unikt namn för hello OS-disken hello nya VM VHD (eller virtuella hårddiskar).</span><span class="sxs-lookup"><span data-stu-id="708d5-179">Copy hello template JSON file and specify a unique name for hello OS disk of hello new VM's VHD (or VHDs).</span></span> <span data-ttu-id="708d5-180">Till exempel i hello **storageProfile**under **vhd**i **uri**, ange ett unikt namn för hello **osDisk** virtuell Hårddisk, liknande för`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span><span class="sxs-lookup"><span data-stu-id="708d5-180">For example, in hello **storageProfile**, under **vhd**, in **uri**, specify a unique name for hello **osDisk** VHD, similar too`https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix-osDisk.xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx.vhd`</span></span>
* <span data-ttu-id="708d5-181">Skapa ett nätverkskort i antingen hello samma eller ett annat virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="708d5-181">Create a NIC in either hello same or a different virtual network.</span></span>
* <span data-ttu-id="708d5-182">Skapa en distribution i hello resursgrupp som du ställer in hello virtuellt nätverk med hello ändrade mallen JSON-fil.</span><span class="sxs-lookup"><span data-stu-id="708d5-182">Using hello modified template JSON file, create a deployment in hello resource group in which you set up hello virtual network.</span></span>

### <a name="use-a-quickstart-template"></a><span data-ttu-id="708d5-183">Använd en mall för Snabbstart</span><span class="sxs-lookup"><span data-stu-id="708d5-183">Use a quickstart template</span></span>
<span data-ttu-id="708d5-184">Om du vill hello nätverksinstallation automatiskt när du skapar en virtuell dator från hello avbildning kan ange du resurserna i en mall.</span><span class="sxs-lookup"><span data-stu-id="708d5-184">If you want hello network set up automatically when you create a VM from hello image, you can specify those resources in a template.</span></span> <span data-ttu-id="708d5-185">Se exempelvis hello [101-vm-från--användaravbildning mallen](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) från GitHub.</span><span class="sxs-lookup"><span data-stu-id="708d5-185">For example, see hello [101-vm-from-user-image template](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) from GitHub.</span></span> <span data-ttu-id="708d5-186">Den här mallen skapar en virtuell dator från din anpassade avbildningen och hello nödvändiga virtuella nätverk, offentlig IP-adress och NIC-resurser.</span><span class="sxs-lookup"><span data-stu-id="708d5-186">This template creates a VM from your custom image and hello necessary virtual network, public IP address, and NIC resources.</span></span> <span data-ttu-id="708d5-187">En genomgång av hello mallen i hello Azure-portalen finns [hur toocreate en virtuell dator från en anpassad avbildning med hjälp av en Resource Manager-mall](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span><span class="sxs-lookup"><span data-stu-id="708d5-187">For a walkthrough of using hello template in hello Azure portal, see [How toocreate a virtual machine from a custom image using a Resource Manager template](http://codeisahighway.com/how-to-create-a-virtual-machine-from-a-custom-image-using-an-arm-template/).</span></span>

### <a name="use-hello-azure-vm-create-command"></a><span data-ttu-id="708d5-188">Använd hello azure vm skapa kommando</span><span class="sxs-lookup"><span data-stu-id="708d5-188">Use hello azure vm create command</span></span>
<span data-ttu-id="708d5-189">Vanligtvis är det enklaste toouse toocreate en Resource Manager-mallen en virtuell dator från hello avbildning.</span><span class="sxs-lookup"><span data-stu-id="708d5-189">Usually it's easiest toouse a Resource Manager template toocreate a VM from hello image.</span></span> <span data-ttu-id="708d5-190">Du kan dock skapa hello VM *imperatively* med hjälp av hello **azure vm skapa** med hello **-Q** (**--bild urn**) parameter .</span><span class="sxs-lookup"><span data-stu-id="708d5-190">However, you can create hello VM *imperatively* by using hello **azure vm create** command with hello **-Q** (**--image-urn**) parameter.</span></span> <span data-ttu-id="708d5-191">Om du använder den här metoden kan du också ange hello **-d** (**--os-disk-vhd**) parametern toospecify hello platsen för hello OS VHD-filen för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="708d5-191">If you use this method, you also pass hello **-d** (**--os-disk-vhd**) parameter toospecify hello location of hello OS .vhd file for hello new VM.</span></span> <span data-ttu-id="708d5-192">Den här filen måste vara i hello VHD-behållare för hello storage-konto där hello avbildningen VHD-filen lagras.</span><span class="sxs-lookup"><span data-stu-id="708d5-192">This file must be in hello vhds container of hello storage account where hello image VHD file is stored.</span></span> <span data-ttu-id="708d5-193">Hej kommandot kopior hello VHD för hello nya virtuella datorn automatiskt toohello **virtuella hårddiskar** behållare.</span><span class="sxs-lookup"><span data-stu-id="708d5-193">hello command copies hello VHD for hello new VM automatically toohello **vhds** container.</span></span>

<span data-ttu-id="708d5-194">Innan du kör **azure vm skapa** med hello avbildningen slutföra hello följande steg:</span><span class="sxs-lookup"><span data-stu-id="708d5-194">Before running **azure vm create** with hello image, complete hello following steps:</span></span>

1. <span data-ttu-id="708d5-195">Skapa en resursgrupp eller identifiera en befintlig resursgrupp för hello-distribution.</span><span class="sxs-lookup"><span data-stu-id="708d5-195">Create a resource group, or identify an existing resource group for hello deployment.</span></span>
2. <span data-ttu-id="708d5-196">Skapa en offentlig IP-adressresurs och en NIC-resurs för hello ny virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="708d5-196">Create a public IP address resource and a NIC resource for hello new VM.</span></span> <span data-ttu-id="708d5-197">För steg toocreate finns ett virtuellt nätverk, offentliga IP-adress och NIC med hjälp av hello CLI, tidigare i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="708d5-197">For steps toocreate a virtual network, public IP address, and NIC by using hello CLI, see earlier in this article.</span></span> <span data-ttu-id="708d5-198">(**azure vm skapa** kan också skapa ett nätverkskort, men du måste toopass ytterligare parametrar för ett virtuellt nätverk och undernät.)</span><span class="sxs-lookup"><span data-stu-id="708d5-198">(**azure vm create** can also create a NIC, but you need toopass additional parameters for a virtual network and subnet.)</span></span>

<span data-ttu-id="708d5-199">Kör ett kommando som klarar URI: er tooboth hello nya OS VHD-filen och hello befintlig avbildning.</span><span class="sxs-lookup"><span data-stu-id="708d5-199">Then run a command that passes URIs tooboth hello new OS VHD file and hello existing image.</span></span> <span data-ttu-id="708d5-200">I det här exemplet en storlek Standard_A1 VM har skapats i hello östra USA region.</span><span class="sxs-lookup"><span data-stu-id="708d5-200">In this example, a size Standard_A1 VM is created in hello East US region.</span></span>

```azurecli
azure vm create -g myResourceGroup1 -n myNewVM -l eastus -y Linux \
-z Standard_A1 -u myAdminname -p myPassword -f myNIC \
-d "https://xxxxxxxxxxxxxx.blob.core.windows.net/vhds/MyNewVHDNamePrefix.vhd" \
-Q "https://xxxxxxxxxxxxxx.blob.core.windows.net/system/Microsoft.Compute/Images/vhds/MyVHDNamePrefix-osDisk.vhd"
```

<span data-ttu-id="708d5-201">Ytterligare alternativ för, kör `azure help vm create`.</span><span class="sxs-lookup"><span data-stu-id="708d5-201">For additional command options, run `azure help vm create`.</span></span>

## <a name="next-steps"></a><span data-ttu-id="708d5-202">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="708d5-202">Next steps</span></span>
<span data-ttu-id="708d5-203">toomanage dina virtuella datorer med hello CLI, se hello uppgifter i [distribuera och hantera virtuella datorer med hjälp av Azure Resource Manager-mallar och hello Azure CLI](create-ssh-secured-vm-from-template.md).</span><span class="sxs-lookup"><span data-stu-id="708d5-203">toomanage your VMs with hello CLI, see hello tasks in [Deploy and manage virtual machines by using Azure Resource Manager templates and hello Azure CLI](create-ssh-secured-vm-from-template.md).</span></span>

