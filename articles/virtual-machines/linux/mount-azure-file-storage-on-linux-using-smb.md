---
title: "aaaMount Azure File storage i virtuella Linux-datorer med hjälp av SMB | Microsoft Docs"
description: "Hur toomount Azure File storage i virtuella Linux-datorer med hjälp av SMB med hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: virtual-machines-linux
author: vlivech
manager: timlt
editor: 
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 02/13/2017
ms.author: v-livech
ms.openlocfilehash: 7b34c81e602748b78c924f44a54b876744c28f56
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a><span data-ttu-id="1563b-103">Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB</span><span class="sxs-lookup"><span data-stu-id="1563b-103">Mount Azure File storage on Linux VMs using SMB</span></span>

<span data-ttu-id="1563b-104">Den här artikeln visar hur tooutilize hello Azure File storage-tjänst på en Linux VM som använder SMB montera med hello Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="1563b-104">This article shows you how tooutilize hello Azure File storage service on a Linux VM using an SMB mount with hello Azure CLI 2.0.</span></span> <span data-ttu-id="1563b-105">Azure File storage erbjuder filresurser i hello molnet med hello SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="1563b-105">Azure File storage offers file shares in hello cloud using hello standard SMB protocol.</span></span> <span data-ttu-id="1563b-106">Du kan också utföra dessa steg med hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="1563b-106">You can also perform these steps with hello [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="1563b-107">hello kraven är:</span><span class="sxs-lookup"><span data-stu-id="1563b-107">hello requirements are:</span></span>

- [<span data-ttu-id="1563b-108">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="1563b-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="1563b-109">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="1563b-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="quick-commands"></a><span data-ttu-id="1563b-110">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="1563b-110">Quick Commands</span></span>

* <span data-ttu-id="1563b-111">En resursgrupp</span><span class="sxs-lookup"><span data-stu-id="1563b-111">A resource group</span></span>
* <span data-ttu-id="1563b-112">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="1563b-112">An Azure virtual network</span></span>
* <span data-ttu-id="1563b-113">En nätverkssäkerhetsgrupp med en SSH inkommande</span><span class="sxs-lookup"><span data-stu-id="1563b-113">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="1563b-114">Ett undernät</span><span class="sxs-lookup"><span data-stu-id="1563b-114">A subnet</span></span>
* <span data-ttu-id="1563b-115">Ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="1563b-115">An Azure storage account</span></span>
* <span data-ttu-id="1563b-116">Azure lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="1563b-116">Azure storage account keys</span></span>
* <span data-ttu-id="1563b-117">Ett Azure File storage-resurs</span><span class="sxs-lookup"><span data-stu-id="1563b-117">An Azure File storage share</span></span>
* <span data-ttu-id="1563b-118">En virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="1563b-118">A Linux VM</span></span>

<span data-ttu-id="1563b-119">Ersätt alla exempel med dina egna inställningar.</span><span class="sxs-lookup"><span data-stu-id="1563b-119">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-hello-local-mount"></a><span data-ttu-id="1563b-120">Skapa en katalog för hello lokal montering</span><span class="sxs-lookup"><span data-stu-id="1563b-120">Create a directory for hello local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a><span data-ttu-id="1563b-121">Montera hello filen lagring SMB-resursen toohello monteringspunkt</span><span class="sxs-lookup"><span data-stu-id="1563b-121">Mount hello File storage SMB share toohello mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a><span data-ttu-id="1563b-122">Spara hello monteringspunkter efter en omstart</span><span class="sxs-lookup"><span data-stu-id="1563b-122">Persist hello mount after a reboot</span></span>
<span data-ttu-id="1563b-123">toodo Lägg därför till följande rad toohello hello `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="1563b-123">toodo so, add hello following line toohello `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="1563b-124">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="1563b-124">Detailed walkthrough</span></span>

<span data-ttu-id="1563b-125">File storage erbjuder filresurser i hello moln som använder hello SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="1563b-125">File storage offers file shares in hello cloud that use hello standard SMB protocol.</span></span> <span data-ttu-id="1563b-126">Du kan också montera en filresurs från alla operativsystem som stöder SMB 3.0 hello senaste versionen för File storage.</span><span class="sxs-lookup"><span data-stu-id="1563b-126">With hello latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="1563b-127">När du använder en SMB-monteringspunkter på Linux hämta enkelt säkerhetskopieringar tooa robust, permanenta arkivering lagringsplats som stöds av ett serviceavtal.</span><span class="sxs-lookup"><span data-stu-id="1563b-127">When you use an SMB mount on Linux, you get easy backups tooa robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="1563b-128">Flytta filer från en VM tooan SMB montering som finns på File storage är en bra sätt toodebug loggar.</span><span class="sxs-lookup"><span data-stu-id="1563b-128">Moving files from a VM tooan SMB mount that's hosted on File storage is a great way toodebug logs.</span></span> <span data-ttu-id="1563b-129">Det beror på att hello samma SMB-resurs kan monteras lokalt tooyour Mac, Linux eller Windows-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="1563b-129">That's because hello same SMB share can be mounted locally tooyour Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="1563b-130">SMB är hello bästa lösningen för direktuppspelning av Linux eller programloggar i realtid, eftersom hello SMB-protokollet inte är inbyggda toohandle sådana uppgifter tunga loggning.</span><span class="sxs-lookup"><span data-stu-id="1563b-130">SMB isn't hello best solution for streaming Linux or application logs in real time, because hello SMB protocol is not built toohandle such heavy logging duties.</span></span> <span data-ttu-id="1563b-131">En dedikerad, enhetlig loggning layer verktyg som till exempel Fluentd är ett bättre alternativ än SMB för att samla in Linux- och programmet loggning utdata.</span><span class="sxs-lookup"><span data-stu-id="1563b-131">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="1563b-132">Den här detaljerade genomgången skapar vi hello krav behövs toofirst skapa hello fillagringsresursen och montera den via SMB på en Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="1563b-132">For this detailed walkthrough, we create hello prerequisites needed toofirst create hello File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="1563b-133">Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create) toohold hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="1563b-133">Create a resource group with [az group create](/cli/azure/group#create) toohold hello file share.</span></span>

    <span data-ttu-id="1563b-134">en resursgrupp med namnet toocreate `myResourceGroup` i hello ”USA, västra” plats, använder du hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1563b-134">toocreate a resource group named `myResourceGroup` in hello "West US" location, use hello following example:</span></span>

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. <span data-ttu-id="1563b-135">Skapa ett Azure storage-konto med [az storage-konto skapar](/cli/azure/storage/account#create) toostore hello faktiska filer.</span><span class="sxs-lookup"><span data-stu-id="1563b-135">Create an Azure storage account with [az storage account create](/cli/azure/storage/account#create) toostore hello actual files.</span></span>

    <span data-ttu-id="1563b-136">toocreate ett lagringskonto med namnet mittlagringskonto med hjälp av hello Standard_LRS lagring SKU, Använd hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1563b-136">toocreate a storage account named mystorageaccount by using hello Standard_LRS storage SKU, use hello following example:</span></span>

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. <span data-ttu-id="1563b-137">Visa hello lagringskontonycklar.</span><span class="sxs-lookup"><span data-stu-id="1563b-137">Show hello storage account keys.</span></span>

    <span data-ttu-id="1563b-138">När du skapar ett lagringskonto skapas hello nycklar i par så att de kan roteras utan några avbrott i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1563b-138">When you create a storage account, hello account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="1563b-139">När du växlar toohello andra nyckeln i hello par kan skapa du ett nytt nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="1563b-139">When you switch toohello second key in hello pair, you create a new key pair.</span></span> <span data-ttu-id="1563b-140">Ny lagringskontonycklar skapas alltid parvis, se till att du alltid har minst en oanvända storage-konto viktiga redo tooswitch till.</span><span class="sxs-lookup"><span data-stu-id="1563b-140">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage account key ready tooswitch to.</span></span>

    <span data-ttu-id="1563b-141">Visa hello lagringskontonycklar med hello [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="1563b-141">View hello storage account keys with hello [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="1563b-142">Hej lagringskontonycklar för hello med namnet `mystorageaccount` visas i följande exempel hello:</span><span class="sxs-lookup"><span data-stu-id="1563b-142">hello storage account keys for hello named `mystorageaccount` are listed in hello following example:</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    <span data-ttu-id="1563b-143">tooextract en enskild nyckel använder hello `--query` flaggan.</span><span class="sxs-lookup"><span data-stu-id="1563b-143">tooextract a single key, use hello `--query` flag.</span></span> <span data-ttu-id="1563b-144">hello följande exempel extraherar hello första nyckeln (`[0]`):</span><span class="sxs-lookup"><span data-stu-id="1563b-144">hello following example extracts hello first key (`[0]`):</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. <span data-ttu-id="1563b-145">Skapa hello File storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="1563b-145">Create hello File storage share.</span></span>

    <span data-ttu-id="1563b-146">Hej fillagringsresursen innehåller hello SMB-resurs med [az lagringsresurs skapa](/cli/azure/storage/share#create).</span><span class="sxs-lookup"><span data-stu-id="1563b-146">hello File storage share contains hello SMB share with [az storage share create](/cli/azure/storage/share#create).</span></span> <span data-ttu-id="1563b-147">hello kvot uttrycks alltid i gigabyte (GB).</span><span class="sxs-lookup"><span data-stu-id="1563b-147">hello quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="1563b-148">Skicka in en av hello nycklar från föregående hello `az storage account keys list` kommando.</span><span class="sxs-lookup"><span data-stu-id="1563b-148">Pass in one of hello keys from hello preceding `az storage account keys list` command.</span></span> <span data-ttu-id="1563b-149">Skapa en resurs med namnet mystorageshare med en 10 GB kvot genom att använda hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1563b-149">Create a share named mystorageshare with a 10-GB quota by using hello following example:</span></span>

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. <span data-ttu-id="1563b-150">Skapa en monteringspunkt katalog.</span><span class="sxs-lookup"><span data-stu-id="1563b-150">Create a mount-point directory.</span></span>

    <span data-ttu-id="1563b-151">Skapa en lokal katalog i hello Linux filen system toomount hello SMB-resurs till.</span><span class="sxs-lookup"><span data-stu-id="1563b-151">Create a local directory in hello Linux file system toomount hello SMB share to.</span></span> <span data-ttu-id="1563b-152">Något skrivs eller läsa från hello lokal montering directory vidarebefordras toohello SMB-resurs som är värd för lagring av filer.</span><span class="sxs-lookup"><span data-stu-id="1563b-152">Anything written or read from hello local mount directory is forwarded toohello SMB share that's hosted on File storage.</span></span> <span data-ttu-id="1563b-153">toocreate en lokal katalog på/mnt/mymountdirectory, Använd hello följande exempel:</span><span class="sxs-lookup"><span data-stu-id="1563b-153">toocreate a local directory at /mnt/mymountdirectory, use hello following example:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. <span data-ttu-id="1563b-154">Montera hello SMB-resursen toohello lokal katalog.</span><span class="sxs-lookup"><span data-stu-id="1563b-154">Mount hello SMB share toohello local directory.</span></span>

    <span data-ttu-id="1563b-155">Ange en egen lagring Kontoanvändarnamn och lagringskontonyckel hello montera autentiseringsuppgifter på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1563b-155">Provide your own storage account username and storage account key for hello mount credentials as follows:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. <span data-ttu-id="1563b-156">Spara hello SMB montera via omstarter.</span><span class="sxs-lookup"><span data-stu-id="1563b-156">Persist hello SMB mount through reboots.</span></span>

    <span data-ttu-id="1563b-157">När du startar om hello Linux VM är hello monterade SMB-resurs frånkopplade vid avstängningen.</span><span class="sxs-lookup"><span data-stu-id="1563b-157">When you reboot hello Linux VM, hello mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="1563b-158">tooremount hello SMB-resurs på Start, lägga till en rad toohello Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="1563b-158">tooremount hello SMB share on boot, add a line toohello Linux /etc/fstab.</span></span> <span data-ttu-id="1563b-159">Linux använder hello fstab filen toolist hello filsystem måste toomount under hello startprocessen.</span><span class="sxs-lookup"><span data-stu-id="1563b-159">Linux uses hello fstab file toolist hello file systems that it needs toomount during hello boot process.</span></span> <span data-ttu-id="1563b-160">Att lägga till hello SMB-resurs säkerställer att hello File storage-resurs är en permanent monterade filsystem för hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="1563b-160">Adding hello SMB share ensures that hello File storage share is a permanently mounted file system for hello Linux VM.</span></span> <span data-ttu-id="1563b-161">Lägger till hello File storage SMB-resursen tooa är ny virtuell dator möjligt när du använder molntjänster initiering.</span><span class="sxs-lookup"><span data-stu-id="1563b-161">Adding hello File storage SMB share tooa new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="1563b-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1563b-162">Next steps</span></span>

- [<span data-ttu-id="1563b-163">Med hjälp av molnet init toocustomize en Linux VM under skapande av</span><span class="sxs-lookup"><span data-stu-id="1563b-163">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="1563b-164">Lägg till en disk tooa Linux VM</span><span class="sxs-lookup"><span data-stu-id="1563b-164">Add a disk tooa Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="1563b-165">Kryptera diskar på en Linux-VM med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="1563b-165">Encrypt disks on a Linux VM by using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
