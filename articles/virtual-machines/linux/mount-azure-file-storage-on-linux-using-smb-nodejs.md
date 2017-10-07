---
title: "aaaMount Azure File storage i virtuella Linux-datorer med hjälp av SMB med Azure CLI 1.0 | Microsoft Docs"
description: "Hur toomount Azure File storage i virtuella Linux-datorer med hjälp av SMB"
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
ms.openlocfilehash: 14a4224228cadb0ae2f05e8e5c8022ee84f138a4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="mount-azure-file-storage-on-linux-vms-by-using-smb-with-azure-cli-10"></a><span data-ttu-id="b28db-103">Montera Azure File storage i virtuella Linux-datorer med hjälp av SMB med Azure CLI 1.0</span><span class="sxs-lookup"><span data-stu-id="b28db-103">Mount Azure File storage on Linux VMs by using SMB with Azure CLI 1.0</span></span>

<span data-ttu-id="b28db-104">Den här artikeln visar hur toomount Azure File storage på en Linux-VM med hjälp av hello Server Message Block (SMB)-protokollet.</span><span class="sxs-lookup"><span data-stu-id="b28db-104">This article shows how toomount Azure File storage on a Linux VM by using hello Server Message Block (SMB) protocol.</span></span> <span data-ttu-id="b28db-105">File storage erbjuder filresurser i hello molnet via hello SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="b28db-105">File storage offers file shares in hello cloud via hello standard SMB protocol.</span></span> <span data-ttu-id="b28db-106">hello kraven är:</span><span class="sxs-lookup"><span data-stu-id="b28db-106">hello requirements are:</span></span>

* <span data-ttu-id="b28db-107">En [Azure-konto](https://azure.microsoft.com/pricing/free-trial/)</span><span class="sxs-lookup"><span data-stu-id="b28db-107">An [Azure account](https://azure.microsoft.com/pricing/free-trial/)</span></span>
* [<span data-ttu-id="b28db-108">Secure Shell (SSH) offentliga och privata nyckelfilerna</span><span class="sxs-lookup"><span data-stu-id="b28db-108">Secure Shell (SSH) public and private key files</span></span>](mac-create-ssh-keys.md)

## <a name="cli-versions-toouse"></a><span data-ttu-id="b28db-109">CLI versioner toouse</span><span class="sxs-lookup"><span data-stu-id="b28db-109">CLI versions toouse</span></span>
<span data-ttu-id="b28db-110">Du kan göra hello med något av följande kommandoradsgränssnittet (CLI) versioner hello:</span><span class="sxs-lookup"><span data-stu-id="b28db-110">You can complete hello task by using one of hello following command-line interface (CLI) versions:</span></span>

- <span data-ttu-id="b28db-111">[Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)</span><span class="sxs-lookup"><span data-stu-id="b28db-111">[Azure CLI 1.0](#quick-commands) – our CLI for hello classic and resource management deployment models (this article)</span></span>
- <span data-ttu-id="b28db-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)-vår nästa generations CLI för hello resursdistributionsmodell för hantering</span><span class="sxs-lookup"><span data-stu-id="b28db-112">[Azure CLI 2.0](mount-azure-file-storage-on-linux-using-smb-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)- our next generation CLI for hello resource management deployment model</span></span>


## <a name="quick-commands"></a><span data-ttu-id="b28db-113">Snabbkommandon</span><span class="sxs-lookup"><span data-stu-id="b28db-113">Quick commands</span></span>
<span data-ttu-id="b28db-114">tooaccomplish hello aktivitet snabbt åtgärderna hello i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="b28db-114">tooaccomplish hello task quickly, follow hello steps in this section.</span></span> <span data-ttu-id="b28db-115">Mer detaljerad information och kontext som börjar vid hello [”detaljerad genomgång”](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) avsnitt.</span><span class="sxs-lookup"><span data-stu-id="b28db-115">For more detailed information and context, begin at hello ["Detailed walkthrough"](mount-azure-file-storage-on-linux-using-smb.md#detailed-walkthrough) section.</span></span>

### <a name="prerequisites"></a><span data-ttu-id="b28db-116">Krav</span><span class="sxs-lookup"><span data-stu-id="b28db-116">Prerequisites</span></span>
* <span data-ttu-id="b28db-117">En resursgrupp</span><span class="sxs-lookup"><span data-stu-id="b28db-117">A resource group</span></span>
* <span data-ttu-id="b28db-118">Azure-nätverk</span><span class="sxs-lookup"><span data-stu-id="b28db-118">An Azure virtual network</span></span>
* <span data-ttu-id="b28db-119">En nätverkssäkerhetsgrupp med en SSH inkommande</span><span class="sxs-lookup"><span data-stu-id="b28db-119">A network security group with an SSH inbound</span></span>
* <span data-ttu-id="b28db-120">Ett undernät</span><span class="sxs-lookup"><span data-stu-id="b28db-120">A subnet</span></span>
* <span data-ttu-id="b28db-121">Ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="b28db-121">An Azure storage account</span></span>
* <span data-ttu-id="b28db-122">Azure lagringskontonycklar</span><span class="sxs-lookup"><span data-stu-id="b28db-122">Azure storage account keys</span></span>
* <span data-ttu-id="b28db-123">Ett Azure File storage-resurs</span><span class="sxs-lookup"><span data-stu-id="b28db-123">An Azure File storage share</span></span>
* <span data-ttu-id="b28db-124">En virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="b28db-124">A Linux VM</span></span>

<span data-ttu-id="b28db-125">Ersätt alla exempel med dina egna inställningar.</span><span class="sxs-lookup"><span data-stu-id="b28db-125">Replace any examples with your own settings.</span></span>

### <a name="create-a-directory-for-hello-local-mount"></a><span data-ttu-id="b28db-126">Skapa en katalog för hello lokal montering</span><span class="sxs-lookup"><span data-stu-id="b28db-126">Create a directory for hello local mount</span></span>

```bash
mkdir -p /mnt/mymountpoint
```

### <a name="mount-hello-file-storage-smb-share-toohello-mount-point"></a><span data-ttu-id="b28db-127">Montera hello filen lagring SMB-resursen toohello monteringspunkt</span><span class="sxs-lookup"><span data-stu-id="b28db-127">Mount hello File storage SMB share toohello mount point</span></span>

```bash
sudo mount -t cifs //myaccountname.file.core.windows.net/mysharename /mymountpoint -o vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

### <a name="persist-hello-mount-after-a-reboot"></a><span data-ttu-id="b28db-128">Spara hello monteringspunkter efter en omstart</span><span class="sxs-lookup"><span data-stu-id="b28db-128">Persist hello mount after a reboot</span></span>
<span data-ttu-id="b28db-129">Lägg till följande rad för hello`/etc/fstab`:</span><span class="sxs-lookup"><span data-stu-id="b28db-129">Add hello following line too`/etc/fstab`:</span></span>

```bash
//myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
```

## <a name="detailed-walkthrough"></a><span data-ttu-id="b28db-130">Detaljerad genomgång</span><span class="sxs-lookup"><span data-stu-id="b28db-130">Detailed walkthrough</span></span>

<span data-ttu-id="b28db-131">File storage erbjuder filresurser i hello moln som använder hello SMB-standardprotokollet.</span><span class="sxs-lookup"><span data-stu-id="b28db-131">File storage offers file shares in hello cloud that use hello standard SMB protocol.</span></span> <span data-ttu-id="b28db-132">Du kan också montera en filresurs från alla operativsystem som stöder SMB 3.0 hello senaste versionen för File storage.</span><span class="sxs-lookup"><span data-stu-id="b28db-132">With hello latest release of File storage, you can also mount a file share from any OS that supports SMB 3.0.</span></span> <span data-ttu-id="b28db-133">När du använder en SMB-monteringspunkter på Linux hämta enkelt säkerhetskopieringar tooa robust, permanenta arkivering lagringsplats som stöds av ett serviceavtal.</span><span class="sxs-lookup"><span data-stu-id="b28db-133">When you use an SMB mount on Linux, you get easy backups tooa robust, permanent archiving storage location that is supported by an SLA.</span></span>

<span data-ttu-id="b28db-134">Flytta filer från en VM tooan SMB montering som finns på File storage är en bra sätt toodebug loggar.</span><span class="sxs-lookup"><span data-stu-id="b28db-134">Moving files from a VM tooan SMB mount that's hosted on File storage is a great way toodebug logs.</span></span> <span data-ttu-id="b28db-135">Det beror på att hello samma SMB-resurs kan monteras lokalt tooyour Mac, Linux eller Windows-arbetsstation.</span><span class="sxs-lookup"><span data-stu-id="b28db-135">That's because hello same SMB share can be mounted locally tooyour Mac, Linux, or Windows workstation.</span></span> <span data-ttu-id="b28db-136">SMB är hello bästa lösningen för direktuppspelning av Linux eller programloggar i realtid, eftersom hello SMB-protokollet inte är inbyggda toohandle sådana uppgifter tunga loggning.</span><span class="sxs-lookup"><span data-stu-id="b28db-136">SMB isn't hello best solution for streaming Linux or application logs in real time, because hello SMB protocol is not built toohandle such heavy logging duties.</span></span> <span data-ttu-id="b28db-137">En dedikerad, enhetlig loggning layer verktyg som till exempel Fluentd är ett bättre alternativ än SMB för att samla in Linux- och programmet loggning utdata.</span><span class="sxs-lookup"><span data-stu-id="b28db-137">A dedicated, unified logging layer tool such as Fluentd would be a better choice than SMB for collecting Linux and application logging output.</span></span>

<span data-ttu-id="b28db-138">Den här detaljerade genomgången skapar vi hello krav behövs toofirst skapa hello fillagringsresursen och montera den via SMB på en Linux-VM.</span><span class="sxs-lookup"><span data-stu-id="b28db-138">For this detailed walkthrough, we create hello prerequisites needed toofirst create hello File storage share, and then mount it via SMB on a Linux VM.</span></span>

1. <span data-ttu-id="b28db-139">Skapa ett Azure storage-konto med hjälp av hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b28db-139">Create an Azure storage account by using hello following code:</span></span>

    ```azurecli
    azure storage account create myStorageAccount \
    --sku-name lrs \
    --kind storage \
    -l westus \
    -g myResourceGroup
    ```

2. <span data-ttu-id="b28db-140">Visa hello lagringskontonycklar.</span><span class="sxs-lookup"><span data-stu-id="b28db-140">Show hello storage account keys.</span></span>

    <span data-ttu-id="b28db-141">När du skapar ett lagringskonto skapas hello nycklar i par så att de kan roteras utan några avbrott i tjänsten.</span><span class="sxs-lookup"><span data-stu-id="b28db-141">When you create a storage account, hello account keys are created in pairs so that they can be rotated without any service interruption.</span></span> <span data-ttu-id="b28db-142">När du växlar toohello andra nyckeln i hello par kan skapa du ett nytt nyckelpar.</span><span class="sxs-lookup"><span data-stu-id="b28db-142">When you switch toohello second key in hello pair, you create a new key pair.</span></span> <span data-ttu-id="b28db-143">Ny lagringskontonycklar skapas alltid parvis, se till att du alltid har minst en oanvända lagring viktiga redo tooswitch till.</span><span class="sxs-lookup"><span data-stu-id="b28db-143">New storage account keys are always created in pairs, ensuring that you always have at least one unused storage key ready tooswitch to.</span></span> <span data-ttu-id="b28db-144">tooshow hello lagringskontonycklar, Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b28db-144">tooshow hello storage account keys, use hello following code:</span></span>

    ```azurecli
    azure storage account keys list myStorageAccount \
    --resource-group myResourceGroup
    ```
3. <span data-ttu-id="b28db-145">Skapa hello File storage-resurs.</span><span class="sxs-lookup"><span data-stu-id="b28db-145">Create hello File storage share.</span></span>

    <span data-ttu-id="b28db-146">Hej fillagringsresursen innehåller hello SMB-resurs.</span><span class="sxs-lookup"><span data-stu-id="b28db-146">hello File storage share contains hello SMB share.</span></span> <span data-ttu-id="b28db-147">hello kvot uttrycks alltid i gigabyte (GB).</span><span class="sxs-lookup"><span data-stu-id="b28db-147">hello quota is always expressed in gigabytes (GB).</span></span> <span data-ttu-id="b28db-148">toocreate hello File storage-resurs, använder hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b28db-148">toocreate hello File storage share, use hello following code:</span></span>

    ```azurecli
    azure storage share create mystorageshare \
    --quota 10 \
    --account-name myStorageAccount \
    --account-key nPOgPR<--snip-->4Q==
    ```

4. <span data-ttu-id="b28db-149">Skapa hello monteringspunkt katalog.</span><span class="sxs-lookup"><span data-stu-id="b28db-149">Create hello mount-point directory.</span></span>

    <span data-ttu-id="b28db-150">Du måste skapa en lokal katalog i hello Linux filen system toomount hello SMB-resurs till.</span><span class="sxs-lookup"><span data-stu-id="b28db-150">You must create a local directory in hello Linux file system toomount hello SMB share to.</span></span> <span data-ttu-id="b28db-151">Något skrivs eller läsa från hello lokal montering directory vidarebefordras toohello SMB-resurs som är värd för lagring av filer.</span><span class="sxs-lookup"><span data-stu-id="b28db-151">Anything written or read from hello local mount directory is forwarded toohello SMB share that's hosted on File storage.</span></span> <span data-ttu-id="b28db-152">toocreate hello directory, Använd hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b28db-152">toocreate hello directory, use hello following code:</span></span>

    ```bash
    sudo mkdir -p /mnt/mymountdirectory
    ```

5. <span data-ttu-id="b28db-153">Montera hello SMB-resurs med hjälp av hello följande kod:</span><span class="sxs-lookup"><span data-stu-id="b28db-153">Mount hello SMB share by using hello following code:</span></span>

    ```azurecli
    sudo mount -t cifs //myStorageAccount.file.core.windows.net/mystorageshare /mnt/mymountdirectory -o vers=3.0,username=myStorageAccount,password=myStorageAccountkey,dir_mode=0777,file_mode=0777
    ```

6. <span data-ttu-id="b28db-154">Spara hello SMB montera via omstarter.</span><span class="sxs-lookup"><span data-stu-id="b28db-154">Persist hello SMB mount through reboots.</span></span>

    <span data-ttu-id="b28db-155">När du startar om hello Linux VM är hello monterade SMB-resurs frånkopplade vid avstängningen.</span><span class="sxs-lookup"><span data-stu-id="b28db-155">When you reboot hello Linux VM, hello mounted SMB share is unmounted during shutdown.</span></span> <span data-ttu-id="b28db-156">tooremount hello SMB-resursen på Start, måste du lägga till en rad toohello Linux /etc/fstab.</span><span class="sxs-lookup"><span data-stu-id="b28db-156">tooremount hello SMB share on boot, you must add a line toohello Linux /etc/fstab.</span></span> <span data-ttu-id="b28db-157">Linux använder hello fstab filen toolist hello filsystem måste toomount under hello startprocessen.</span><span class="sxs-lookup"><span data-stu-id="b28db-157">Linux uses hello fstab file toolist hello file systems that it needs toomount during hello boot process.</span></span> <span data-ttu-id="b28db-158">Att lägga till hello SMB-resurs säkerställer att hello File storage-resurs är en permanent monterade filsystem för hello Linux VM.</span><span class="sxs-lookup"><span data-stu-id="b28db-158">Adding hello SMB share ensures that hello File storage share is a permanently mounted file system for hello Linux VM.</span></span> <span data-ttu-id="b28db-159">Lägger till hello File storage SMB-resursen tooa är ny virtuell dator möjligt när du använder molntjänster initiering.</span><span class="sxs-lookup"><span data-stu-id="b28db-159">Adding hello File storage SMB share tooa new VM is possible when you use cloud-init.</span></span>

    ```bash
    //myaccountname.file.core.windows.net/mysharename /mymountpoint cifs vers=3.0,username=myaccountname,password=StorageAccountKeyEndingIn==,dir_mode=0777,file_mode=0777
    ```

## <a name="next-steps"></a><span data-ttu-id="b28db-160">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b28db-160">Next steps</span></span>

- [<span data-ttu-id="b28db-161">Med hjälp av molnet init toocustomize en Linux VM under skapande av</span><span class="sxs-lookup"><span data-stu-id="b28db-161">Using cloud-init toocustomize a Linux VM during creation</span></span>](using-cloud-init.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="b28db-162">Lägg till en disk tooa Linux VM</span><span class="sxs-lookup"><span data-stu-id="b28db-162">Add a disk tooa Linux VM</span></span>](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
- [<span data-ttu-id="b28db-163">Kryptera diskar på en Linux-VM med hjälp av hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="b28db-163">Encrypt disks on a Linux VM by using hello Azure CLI</span></span>](encrypt-disks.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
