---
title: "Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB | Microsoft Docs"
description: "Hur man monterar Azure File storage på virtuella Linux-datorer med hjälp av SMB med Azure CLI 2.0"
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
ms.openlocfilehash: 9eae17b304f8a987b44ebed8906dabd8ff3a36a8
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-using-smb"></a><span data-ttu-id="66231-103">Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB</span><span class="sxs-lookup"><span data-stu-id="66231-103">Mount Azure File storage on Linux VMs using SMB</span></span>

<span data-ttu-id="66231-104">Den här artikeln visar hur du använder tjänsten Azure File storage på en Linux-VM i en SMB-montering med Azure CLI 2.0.</span><span class="sxs-lookup"><span data-stu-id="66231-104">This article shows you how to utilize the Azure File storage service on a Linux VM using an SMB mount with the Azure CLI 2.0.</span></span> <span data-ttu-id="66231-105">Azure File storage erbjuder filresurser i molnet genom att använda SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="66231-105">Azure File storage offers file shares in the cloud using the standard SMB protocol.</span></span> <span data-ttu-id="66231-106">Du kan också utföra dessa steg med [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="66231-106">You can also perform these steps with the [Azure CLI 1.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).</span></span> <span data-ttu-id="66231-107">Kraven är:</span><span class="sxs-lookup"><span data-stu-id="66231-107">The requirements are:</span></span>

- [<span data-ttu-id="66231-108">ett Azure-konto</span><span class="sxs-lookup"><span data-stu-id="66231-108">an Azure account</span></span>](https://azure.microsoft.com/pricing/free-trial/)
- [<span data-ttu-id="66231-109">offentliga och privata SSH-nyckelfiler</span><span class="sxs-lookup"><span data-stu-id="66231-109">SSH public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="quick-commands"></a><span data-ttu-id="66231-110">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="66231-110">Quick Commands</span></span>

* <span data-ttu-id="66231-111">En resursgrupp</span><span class="sxs-lookup"><span data-stu-id="66231-111">A resource group</span></span>
* <span data-ttu-id="66231-112">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="66231-112">An Azure virtual network</span></span>
* <span data-ttu-id="66231-113">En nätverkssäkerhetsgrupp med en SSH inkommande</span><span class="sxs-lookup"><span data-stu-id="66231-113">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="66231-114">Ett undernät</span><span class="sxs-lookup"><span data-stu-id="66231-114">A subnet</span></span>
* <span data-ttu-id="66231-115">Ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="66231-115">An Azure storage account</span></span>
* <span data-ttu-id="66231-116">Azure lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="66231-116">Azure storage account keys</span></span>
* <span data-ttu-id="66231-117">Ett Azure File storage-resurs</span><span class="sxs-lookup"><span data-stu-id="66231-117">An Azure File storage share</span></span>
* <span data-ttu-id="66231-118">En virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="66231-118">A Linux VM</span></span>

<span data-ttu-id="66231-119">Ersätt alla exempel med dina egna inställningar.</span><span class="sxs-lookup"><span data-stu-id="66231-119">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-the-local-mount"></a><span data-ttu-id="66231-120">Skapa en katalog för lokal montering</span><span class="sxs-lookup"><span data-stu-id="66231-120">Create a directory for the local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-the-file-storage-smb-share-to-the-mount-point"></a><span data-ttu-id="66231-121">Montera fillagring SMB-resurs till monteringspunkten</span><span class="sxs-lookup"><span data-stu-id="66231-121">Mount the File storage SMB share to the mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-the-mount-after-a-reboot"></a><span data-ttu-id="66231-122">Monteringen är kvar efter en omstart</span><span class="sxs-lookup"><span data-stu-id="66231-122">Persist the mount after a reboot</span></span>
<span data-ttu-id="66231-123">Om du vill göra det lägger du till följande rad i den `/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="66231-123">To do so, add the following line to the `/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="66231-124">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="66231-124">Detailed walkthrough</span></span>

<span data-ttu-id="66231-125">File storage erbjuder filresurser i molnet som använder SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="66231-125">File storage offers file shares in the cloud that use the standard SMB protocol.</span></span> <span data-ttu-id="66231-126">Du kan också montera en filresurs från alla operativsystem som stöder SMB 3.0 med den senaste versionen av File storage.</span><span class="sxs-lookup"><span data-stu-id="66231-126">With the latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="66231-127">När du använder en SMB-monteringspunkter på Linux hämta enkelt säkerhetskopieringar till en robust, permanenta arkivering lagringsplats som stöds av ett serviceavtal.</span><span class="sxs-lookup"><span data-stu-id="66231-127">When you use an SMB mount on Linux, you get easy backups to a robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="66231-128">Flytta filer från en virtuell dator till en SMB-monteringspunkter som är värd för fillagring är ett bra sätt att debug-loggar.</span><span class="sxs-lookup"><span data-stu-id="66231-128">Moving files from a VM to an SMB mount that's hosted on File storage is a great way to debug logs.</span></span> <span data-ttu-id="66231-129">Det beror på att samma SMB-resursen kan monteras lokalt till din Mac, Linux eller Windows-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="66231-129">That's because the same SMB share can be mounted locally to your Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="66231-130">SMB är den bästa lösningen för direktuppspelning av Linux eller programloggar i realtid, eftersom SMB-protokollet inte har byggts för att hantera sådana uppgifter tunga loggning.</span><span class="sxs-lookup"><span data-stu-id="66231-130">SMB isn't the best solution for streaming Linux or application logs in real time, because the SMB protocol is not built to handle such heavy logging duties.</span></span> <span data-ttu-id="66231-131">En dedikerad, enhetlig loggning layer verktyg som till exempel Fluentd är ett bättre alternativ än SMB för att samla in Linux- och programmet loggning utdata.</span><span class="sxs-lookup"><span data-stu-id="66231-131">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="66231-132">För den här detaljerade genomgången ska vi skapa de förutsättningar som krävs för att först skapa fillagringsresursen och montera den via SMB på en Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="66231-132">For this detailed walkthrough, we create the prerequisites needed to first create the File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="66231-133">Skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create) för filresursen.</span><span class="sxs-lookup"><span data-stu-id="66231-133">Create a resource group with [az group create](/cli/azure/group#create) to hold the file share.</span></span>

    <span data-ttu-id="66231-134">Så här skapar du en resursgrupp med namnet `myResourceGroup` på plats ”USA, västra”, Använd följande exempel:</span><span class="sxs-lookup"><span data-stu-id="66231-134">To create a resource group named `myResourceGroup` in the "West US" location, use the following example:</span></span>

    ```azurecli
    az group create --name myResourceGroup --location westus
    ```

2. <span data-ttu-id="66231-135">Skapa ett Azure storage-konto med [az storage-konto skapar](/cli/azure/storage/account#create) de faktiska filerna.</span><span class="sxs-lookup"><span data-stu-id="66231-135">Create an Azure storage account with [az storage account create](/cli/azure/storage/account#create) to store the actual files.</span></span>

    <span data-ttu-id="66231-136">Använd följande exempel för att skapa ett lagringskonto med namnet mittlagringskonto med hjälp av Standard_LRS lagring SKU:</span><span class="sxs-lookup"><span data-stu-id="66231-136">To create a storage account named mystorageaccount by using the Standard_LRS storage SKU, use the following example:</span></span>

    ```azurecli
    az storage account create --resource-group myResourceGroup \
        --name mystorageaccount \
        --location westus \
        --sku Standard_LRS
    ```

3. <span data-ttu-id="66231-137">Visa lagringskontonycklarna.</span><span class="sxs-lookup"><span data-stu-id="66231-137">Show the storage account keys.</span></span>

    <span data-ttu-id="66231-138">När du skapar ett lagringskonto skapas nycklar för kontot i par så att de kan roteras utan några avbrott i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="66231-138">When you create a storage account, the account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="66231-139">När du växlar till den andra nyckeln i paret, skapar du en ny nyckel.</span><span class="sxs-lookup"><span data-stu-id="66231-139">When you switch to the second key in the pair, you create a new key pair.</span></span> <span data-ttu-id="66231-140">Ny lagringskontonycklar skapas alltid parvis, se till att du alltid har minst en oanvända lagringskontonyckel redo att växla till.</span><span class="sxs-lookup"><span data-stu-id="66231-140">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage account key ready to switch to.</span></span>

    <span data-ttu-id="66231-141">Visa lagringskontonycklar med den [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).</span><span class="sxs-lookup"><span data-stu-id="66231-141">View the storage account keys with the [az storage account keys list](/cli/azure/storage/account/keys#list).</span></span> <span data-ttu-id="66231-142">Lagringskontot nycklar för den namngivna `mystorageaccount` visas i följande exempel:</span><span class="sxs-lookup"><span data-stu-id="66231-142">The storage account keys for the named `mystorageaccount` are listed in the following example:</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount
    ```

    <span data-ttu-id="66231-143">Om du vill extrahera en enskild nyckel måste använda den `--query` flaggan.</span><span class="sxs-lookup"><span data-stu-id="66231-143">To extract a single key, use the `--query` flag.</span></span> <span data-ttu-id="66231-144">I följande exempel hämtar den första nyckeln (`[0]`):</span><span class="sxs-lookup"><span data-stu-id="66231-144">The following example extracts the first key (`[0]`):</span></span>

    ```azurecli
    az storage account keys list --resource-group myResourceGroup \
        --account-name mystorageaccount \
        --query '[0].{Key:value}' --output tsv
    ```

4. <span data-ttu-id="66231-145">Skapa File storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="66231-145">Create the File storage share.</span></span>

    <span data-ttu-id="66231-146">Fillagringsresursen innehåller SMB-resursen med [az lagringsresurs skapa](/cli/azure/storage/share#create).</span><span class="sxs-lookup"><span data-stu-id="66231-146">The File storage share contains the SMB share with [az storage share create](/cli/azure/storage/share#create).</span></span> <span data-ttu-id="66231-147">Kvoten uttrycks alltid i gigabyte (GB).</span><span class="sxs-lookup"><span data-stu-id="66231-147">The quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="66231-148">Pass i en av nycklarna från den föregående `az storage account keys list` kommando.</span><span class="sxs-lookup"><span data-stu-id="66231-148">Pass in one of the keys from the preceding `az storage account keys list` command.</span></span> <span data-ttu-id="66231-149">Skapa en resurs med namnet mystorageshare med en 10 GB kvot genom att använda följande exempel:</span><span class="sxs-lookup"><span data-stu-id="66231-149">Create a share named mystorageshare with a 10-GB quota by using the following example:</span></span>

    ```azurecli
    az storage share create --name mystorageshare \
        --quota 10 \
        --account-name mystorageaccount \
        --account-key nPOgPR<--snip-->4Q==
    ```

5. <span data-ttu-id="66231-150">Skapa en monteringspunkt katalog.</span><span class="sxs-lookup"><span data-stu-id="66231-150">Create a mount-point directory.</span></span>

    <span data-ttu-id="66231-151">Skapa en lokal katalog i filsystemet Linux för att montera SMB-resursen.</span><span class="sxs-lookup"><span data-stu-id="66231-151">Create a local directory in the Linux file system to mount the SMB share to.</span></span> <span data-ttu-id="66231-152">Något skrivs eller läsa från katalogen för lokal montering vidarebefordras till SMB-resurs som är värd för lagring av filer.</span><span class="sxs-lookup"><span data-stu-id="66231-152">Anything written or read from the local mount directory is forwarded to the SMB share that's hosted on File storage.</span></span> <span data-ttu-id="66231-153">Om du vill skapa en lokal katalog i /mnt/mymountdirectory kan du använda följande exempel:</span><span class="sxs-lookup"><span data-stu-id="66231-153">To create a local directory at /mnt/mymountdirectory, use the following example:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

6. <span data-ttu-id="66231-154">Montera SMB-resursen till den lokala katalogen.</span><span class="sxs-lookup"><span data-stu-id="66231-154">Mount the SMB share to the local directory.</span></span>

    <span data-ttu-id="66231-155">Ange dina egna lagring Kontoanvändarnamn och lagringskontonyckel för monteringspunkter på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="66231-155">Provide your own storage account username and storage account key for the mount credentials as follows:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=mystorageaccount,password=mystorageaccountkey,dir_mode=0777,file_mode=0777
    ```

7. <span data-ttu-id="66231-156">Spara SMB montera via omstarter.</span><span class="sxs-lookup"><span data-stu-id="66231-156">Persist the SMB mount through reboots.</span></span>

    <span data-ttu-id="66231-157">När du startar om Linux VM är den monterade SMB-resursen omonterade vid avstängningen.</span><span class="sxs-lookup"><span data-stu-id="66231-157">When you reboot the Linux VM, the mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="66231-158">Lägga till en rad Linux /etc/fstab om du vill återansluta till SMB-resursen på Start.</span><span class="sxs-lookup"><span data-stu-id="66231-158">To remount the SMB share on boot, add a line to the Linux /etc/fstab.</span></span> <span data-ttu-id="66231-159">Linux använder filen fstab för att lista filsystem som krävs för att montera under startprocessen.</span><span class="sxs-lookup"><span data-stu-id="66231-159">Linux uses the fstab file to list the file systems that it needs to mount during the boot process.</span></span> <span data-ttu-id="66231-160">Lägger till SMB-resursen garanterar att File storage-resurs är en permanent anslutet filsystem för Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="66231-160">Adding the SMB share ensures that the File storage share is a permanently mounted file system for the Linux VM.</span></span> <span data-ttu-id="66231-161">Det är möjligt att lägga till File storage SMB-resurs i en ny virtuell dator när du använder molntjänster initiering.</span><span class="sxs-lookup"><span data-stu-id="66231-161">Adding the File storage SMB share to a new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="66231-162">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="66231-162">Next steps</span></span>

- [<span data-ttu-id="66231-163">Med hjälp av molnet init för att anpassa en Linux VM under skapandet</span><span class="sxs-lookup"><span data-stu-id="66231-163">Using cloud-init to customize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="66231-164">Lägg till en disk till en virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="66231-164">Add a disk to a Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="66231-165">Kryptera diskar på en Linux-VM med hjälp av Azure CLI</span><span class="sxs-lookup"><span data-stu-id="66231-165">Encrypt disks on a Linux VM by using the Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
