---
title: "Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB med Azure CLI 1.0 | Microsoft Docs"
description: "Hur man monterar Azure File storage på Linux virtuella datorer med hjälp av SMB"
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
ms.date: 12/07/2016
ms.author: v-livech
ms.openlocfilehash: 4951860630f0aad107d0846d52ebe4423ee0b91c
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a><span data-ttu-id="239b1-103">Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="239b1-103">Mount Azure File storage on Linux VMs by using SMB with Azure CLI 1.0</span></span>

<span data-ttu-id="239b1-104">Den här artikeln visar hur du monterar Azure File storage på en Linux-VM med hjälp av Server Message Block (SMB)-protokollet.</span><span class="sxs-lookup"><span data-stu-id="239b1-104">This article shows how to mount Azure File storage on a Linux VM by using the Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="239b1-105">File storage erbjuder filresurser i molnet via SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="239b1-105">File storage offers file shares in the cloud via the standard SMB protocol.</span></span> <span data-ttu-id="239b1-106">Kraven är:</span><span class="sxs-lookup"><span data-stu-id="239b1-106">The requirements are:</span></span>

* <span data-ttu-id="239b1-107">En [Azure-konto](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="239b1-107">An [Azure account](https://azure.microsoft.com/pricing/free-trial/)</span></span>
* [<span data-ttu-id="239b1-108">Secure Shell (SSH) offentliga och privata nyckelfilerna</span><span class="sxs-lookup"><span data-stu-id="239b1-108">Secure Shell (SSH) public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="cli-versions-to-use"></a><span data-ttu-id="239b1-109">CLI-versioner som du kan använda</span><span class="sxs-lookup"><span data-stu-id="239b1-109">CLI versions to use</span></span>
<span data-ttu-id="239b1-110">Du kan slutföra uppgiften med någon av följande versioner kommandoradsgränssnittet (CLI):</span><span class="sxs-lookup"><span data-stu-id="239b1-110">You can complete the task by using one of the following command-line interface (CLI) versions:</span></span>

- <span data-ttu-id="239b1-111">[Azure CLI 1.0](#quick-commands) – våra CLI för klassisk och resurs management på distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="239b1-111">[Azure CLI 1.0](#quick-commands) – our CLI for the classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="239b1-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-vår nästa generations CLI för resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="239b1-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)- our next generation CLI for the resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="239b1-113">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="239b1-113">Quick commands</span></span>
<span data-ttu-id="239b1-114">Följ stegen i det här avsnittet om du vill slutföra uppgiften snabbt.</span><span class="sxs-lookup"><span data-stu-id="239b1-114">To accomplish the task quickly, follow the steps in this section.</span></span> <span data-ttu-id="239b1-115">Mer detaljerad information och kontext som börjar vid den [”detaljerad genomgång”](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="239b1-115">For more detailed information and context, begin at the ["Detailed walkthrough"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) section.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="239b1-116">Krav</span><span class="sxs-lookup"><span data-stu-id="239b1-116">Prerequisites</span></span>
* <span data-ttu-id="239b1-117">En resursgrupp</span><span class="sxs-lookup"><span data-stu-id="239b1-117">A resource group</span></span>
* <span data-ttu-id="239b1-118">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="239b1-118">An Azure virtual network</span></span>
* <span data-ttu-id="239b1-119">En nätverkssäkerhetsgrupp med en SSH inkommande</span><span class="sxs-lookup"><span data-stu-id="239b1-119">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="239b1-120">Ett undernät</span><span class="sxs-lookup"><span data-stu-id="239b1-120">A subnet</span></span>
* <span data-ttu-id="239b1-121">Ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="239b1-121">An Azure storage account</span></span>
* <span data-ttu-id="239b1-122">Azure lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="239b1-122">Azure storage account keys</span></span>
* <span data-ttu-id="239b1-123">Ett Azure File storage-resurs</span><span class="sxs-lookup"><span data-stu-id="239b1-123">An Azure File storage share</span></span>
* <span data-ttu-id="239b1-124">En virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="239b1-124">A Linux VM</span></span>

<span data-ttu-id="239b1-125">Ersätt alla exempel med dina egna inställningar.</span><span class="sxs-lookup"><span data-stu-id="239b1-125">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-the-local-mount"></a><span data-ttu-id="239b1-126">Skapa en katalog för lokal montering</span><span class="sxs-lookup"><span data-stu-id="239b1-126">Create a directory for the local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a><span data-ttu-id="239b1-127">Montera fillagring SMB-resurs till monteringspunkten</span><span class="sxs-lookup"><span data-stu-id="239b1-127">Mount the File storage SMB share to the mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a><span data-ttu-id="239b1-128">Monteringen är kvar efter en omstart</span><span class="sxs-lookup"><span data-stu-id="239b1-128">Persist the mount after a reboot</span></span>
<span data-ttu-id="239b1-129">Lägg till följande rad i `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="239b1-129">Add the following line to `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="239b1-130">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="239b1-130">Detailed walkthrough</span></span>

<span data-ttu-id="239b1-131">File storage erbjuder filresurser i molnet som använder SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="239b1-131">File storage offers file shares in the cloud that use the standard SMB protocol.</span></span> <span data-ttu-id="239b1-132">Du kan också montera en filresurs från alla operativsystem som stöder SMB 3.0 med den senaste versionen av File storage.</span><span class="sxs-lookup"><span data-stu-id="239b1-132">With the latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="239b1-133">När du använder en SMB-monteringspunkter på Linux hämta enkelt säkerhetskopieringar till en robust, permanenta arkivering lagringsplats som stöds av ett serviceavtal.</span><span class="sxs-lookup"><span data-stu-id="239b1-133">When you use an SMB mount on Linux, you get easy backups to a robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="239b1-134">Flytta filer från en virtuell dator till en SMB-monteringspunkter som är värd för fillagring är ett bra sätt att debug-loggar.</span><span class="sxs-lookup"><span data-stu-id="239b1-134">Moving files from a VM to an SMB mount that's hosted on File storage is a great way to debug logs.</span></span> <span data-ttu-id="239b1-135">Det beror på att samma SMB-resursen kan monteras lokalt till din Mac, Linux eller Windows-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="239b1-135">That's because the same SMB share can be mounted locally to your Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="239b1-136">SMB är den bästa lösningen för direktuppspelning av Linux eller programloggar i realtid, eftersom SMB-protokollet inte har byggts för att hantera sådana uppgifter tunga loggning.</span><span class="sxs-lookup"><span data-stu-id="239b1-136">SMB isn't the best solution for streaming Linux or application logs in real time, because the SMB protocol is not built to handle such heavy logging duties.</span></span> <span data-ttu-id="239b1-137">En dedikerad, enhetlig loggning layer verktyg som till exempel Fluentd är ett bättre alternativ än SMB för att samla in Linux- och programmet loggning utdata.</span><span class="sxs-lookup"><span data-stu-id="239b1-137">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="239b1-138">För den här detaljerade genomgången ska vi skapa de förutsättningar som krävs för att först skapa fillagringsresursen och montera den via SMB på en Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="239b1-138">For this detailed walkthrough, we create the prerequisites needed to first create the File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="239b1-139">Skapa ett Azure storage-konto med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="239b1-139">Create an Azure storage account by using the following code:</span></span>

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. <span data-ttu-id="239b1-140">Visa lagringskontonycklarna.</span><span class="sxs-lookup"><span data-stu-id="239b1-140">Show the storage account keys.</span></span>

    <span data-ttu-id="239b1-141">När du skapar ett lagringskonto skapas nycklar för kontot i par så att de kan roteras utan några avbrott i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="239b1-141">When you create a storage account, the account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="239b1-142">När du växlar till den andra nyckeln i paret, skapar du en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="239b1-142">When you switch to the second key in the pair, you create a new key pair.</span></span> <span data-ttu-id="239b1-143">Ny lagringskontonycklar skapas alltid parvis, se till att du alltid har minst en oanvända-lagringsnyckel är redo att växla till.</span><span class="sxs-lookup"><span data-stu-id="239b1-143">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage key ready to switch to.</span></span> <span data-ttu-id="239b1-144">Om du vill visa lagringskontonycklarna, använder du följande kod:</span><span class="sxs-lookup"><span data-stu-id="239b1-144">To show the storage account keys, use the following code:</span></span>

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. <span data-ttu-id="239b1-145">Skapa File storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="239b1-145">Create the File storage share.</span></span>

    <span data-ttu-id="239b1-146">Fillagringsresursen innehåller SMB-resursen.</span><span class="sxs-lookup"><span data-stu-id="239b1-146">The File storage share contains the SMB share.</span></span> <span data-ttu-id="239b1-147">Kvoten uttrycks alltid i gigabyte (GB).</span><span class="sxs-lookup"><span data-stu-id="239b1-147">The quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="239b1-148">Använd följande kod för att skapa File storage-resurs:</span><span class="sxs-lookup"><span data-stu-id="239b1-148">To create the File storage share, use the following code:</span></span>

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. <span data-ttu-id="239b1-149">Skapa katalogen monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="239b1-149">Create the mount-point directory.</span></span>

    <span data-ttu-id="239b1-150">Du måste skapa en lokal katalog i filsystemet Linux för att montera SMB-resursen.</span><span class="sxs-lookup"><span data-stu-id="239b1-150">You must create a local directory in the Linux file system to mount the SMB share to.</span></span> <span data-ttu-id="239b1-151">Något skrivs eller läsa från katalogen för lokal montering vidarebefordras till SMB-resurs som är värd för lagring av filer.</span><span class="sxs-lookup"><span data-stu-id="239b1-151">Anything written or read from the local mount directory is forwarded to the SMB share that's hosted on File storage.</span></span> <span data-ttu-id="239b1-152">Använd följande kod för att skapa katalogen:</span><span class="sxs-lookup"><span data-stu-id="239b1-152">To create the directory, use the following code:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. <span data-ttu-id="239b1-153">Montera SMB-resursen med hjälp av följande kod:</span><span class="sxs-lookup"><span data-stu-id="239b1-153">Mount the SMB share by using the following code:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. <span data-ttu-id="239b1-154">Spara SMB montera via omstarter.</span><span class="sxs-lookup"><span data-stu-id="239b1-154">Persist the SMB mount through reboots.</span></span>

    <span data-ttu-id="239b1-155">När du startar om Linux VM är den monterade SMB-resursen omonterade vid avstängningen.</span><span class="sxs-lookup"><span data-stu-id="239b1-155">When you reboot the Linux VM, the mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="239b1-156">Om du vill återansluta till SMB-resursen på Start, måste du lägga till en rad Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="239b1-156">To remount the SMB share on boot, you must add a line to the Linux /etc/fstab.</span></span> <span data-ttu-id="239b1-157">Linux använder filen fstab för att lista filsystem som krävs för att montera under startprocessen.</span><span class="sxs-lookup"><span data-stu-id="239b1-157">Linux uses the fstab file to list the file systems that it needs to mount during the boot process.</span></span> <span data-ttu-id="239b1-158">Lägger till SMB-resursen garanterar att File storage-resurs är en permanent anslutet filsystem för Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="239b1-158">Adding the SMB share ensures that the File storage share is a permanently mounted file system for the Linux VM.</span></span> <span data-ttu-id="239b1-159">Det är möjligt att lägga till File storage SMB-resurs i en ny virtuell dator när du använder molntjänster initiering.</span><span class="sxs-lookup"><span data-stu-id="239b1-159">Adding the File storage SMB share to a new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="239b1-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="239b1-160">Next steps</span></span>

- [<span data-ttu-id="239b1-161">Med hjälp av molnet init för att anpassa en Linux VM under skapandet</span><span class="sxs-lookup"><span data-stu-id="239b1-161">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="239b1-162">Lägg till en disk till en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="239b1-162">Add a disk to a Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="239b1-163">Kryptera diskar på en Linux-VM med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="239b1-163">Encrypt disks on a Linux VM by using the Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
