---
title: aaaCreate och ladda upp en Linux VHD-tooAzure | Microsoft Docs
description: "Skapa och ladda upp en Azure virtuell hårddisk (VHD) som innehåller hello Linux-operativsystem med hjälp av hello klassiska distributionsmodellen"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: 8058ff98-db03-4309-9bf4-69842bd64dd4
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: iainfou
ms.openlocfilehash: 77b01316386c4a6eb68c129fa68d42f0a8996edc
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="creating-and-uploading-a-virtual-hard-disk-that-contains-hello-linux-operating-system"></a>Skapa och ladda upp en virtuell hårddisk som innehåller hello Linux-operativsystem
> [!IMPORTANT] 
> Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../../../resource-manager-deployment-model.md). Den här artikeln täcker hello klassiska distributionsmodellen. Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen. Du kan också [ladda upp en anpassad avbildning med Azure Resource Manager](../upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

Den här artikeln visar hur toocreate och ladda upp en virtuell hårddisk (VHD) så att du kan använda den som din egen avbildning toocreate virtuella datorer i Azure. Lär dig hur tooprepare hello-operativsystemet så att du kan använda den toocreate flera virtuella datorer baserat på avbildningen. 


## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har hello följande objekt:

* **Linux-operativsystem i en VHD-fil** -du har installerat en [Azure-godkända Linux-distribution](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD-format. Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:
  * Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat. Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.
  * Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello senare VHDX-format stöds inte i Azure. När du skapar en virtuell dator kan du ange VHD som hello-format. Om det behövs kan du konvertera VHDX diskar tooVHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp. Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.

* **Azure-kommandoradsgränssnittet** -installera hello senaste [Azure-kommandoradsgränssnittet](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2) tooupload hello VHD.

<a id="prepimage"> </a>

## <a name="step-1-prepare-hello-image-toobe-uploaded"></a>Steg 1: Förbered hello avbildningen toobe har överförts
Azure stöder olika Linux-distributioner (se [godkända distributioner](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). hello följande artiklar leder dig igenom hur tooprepare hello olika Linux-distributioner som stöds på Azure. När du har slutfört hello stegen i följande guider hello du återvända hit när du har en VHD-fil som är redo tooupload tooAzure:

* **[CentOS-baserade distributioner](../create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](../debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](../oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](../redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](../suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](../create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Andra - icke-godkända distributioner](../create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

> [!NOTE]
> hello Azure-plattformen SLA gäller toovirtual datorer som kör Linux OS endast när en hello-godkända distributioner används med hello konfigurationsinformation som anges under stöds versioner hello [Linux på Azure-Endorsed distributioner ](../endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Alla Linux-distributioner i hello Azure-avbildning galleriet är påtecknade distributioner med hello nödvändig konfiguration.
> 
> 

Se även hello  **[Linux installationsinformation](../create-upload-generic.md#general-linux-installation-notes)**  mer allmänna tips om hur du förbereder Linux avbildningar för Azure.

<a id="connect"> </a>

## <a name="step-2-prepare-hello-connection-tooazure"></a>Steg 2: Förbered hello anslutning tooAzure
Kontrollera att du använder hello Azure CLI i hello klassiska distributionsmodellen (`azure config mode asm`), logga sedan in tooyour konto:

```azurecli
azure login
```


<a id="upload"> </a>

## <a name="step-3-upload-hello-image-tooazure"></a>Steg 3: Överför hello bilden tooAzure
Du behöver en storage-konto tooupload VHD-filen till. Du kan antingen välja ett befintligt lagringskonto eller [skapa en ny](../../../storage/common/storage-create-storage-account.md).

Använd hello Azure CLI tooupload hello avbildningen med hjälp av hello följande kommando:

```azurecli
azure vm image create <ImageName> `
    --blob-url <BlobStorageURL>/<YourImagesFolder>/<VHDName> `
    --os Linux <PathToVHDFile>
```

I hello förra exemplet:

* **BlobStorageURL** är hello URL för hello storage-konto som du planerar toouse
* **YourImagesFolder** är hello behållare i blobblagring dit toostore bilderna
* **VHDName** är hello etiketten som visas i portalen tooidentify hello virtuell hårddisk.
* **PathToVHDFile** hello sökvägen och namnet på hello VHD-filen på din dator.

hello följande kommando visar en komplett exempel:

```azurecli
azure vm image create myImage `
    --blob-url https://mystorage.blob.core.windows.net/vhds/myimage.vhd `
    --os Linux /home/ahmet/myimage.vhd
```

## <a name="step-4-create-a-vm-from-hello-image"></a>Steg 4: Skapa en virtuell dator från hello avbildning
Du skapar en virtuell dator med hjälp av `azure vm create` i hello samma sätt som vanliga virtuell dator. Ange hello namn du gav avbildningen i hello föregående steg. I följande exempel hello, vi använda hello **myImage** avbildningsnamn i hello föregående steg:

```azurecli
azure vm create --userName ops --password P@ssw0rd! --vm-size Small --ssh `
    --location "West US" "myDeployedVM" myImage
```

toocreate egna virtuella datorer, ange egna användarnamn + lösenord, plats, DNS-namn och namn.

## <a name="next-steps"></a>Nästa steg
Mer information finns i [Azure CLI-referens för hello Azure klassiska distributionsmodellen](https://docs.microsoft.com/cli/azure/get-started-with-az-cli2).

[Step 1: Prepare hello image toobe uploaded]:#prepimage
[Step 2: Prepare hello connection tooAzure]:#connect
[Step 3: Upload hello image tooAzure]:#upload
