---
title: aaaUpload eller kopiera en anpassad Linux VM med Azure CLI 2.0 | Microsoft Docs
description: "Ladda upp eller kopiera en anpassad virtuell dator med hjälp av hello Resource Manager-modellen och hello Azure CLI 2.0"
services: virtual-machines-linux
documentationcenter: 
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 07/06/2017
ms.author: cynthn
ms.openlocfilehash: 79af897120a6ba7f4a427ba6c7d0c31b7b870bd7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a>Skapa en Linux VM från anpassade disken med hello Azure CLI 2.0

<!-- rename toocreate-vm-specialized -->

Den här artikeln beskrivs hur du tooupload en anpassad virtuell hårddisk (VHD) eller kopiera en en befintlig virtuell Hårddisk i Azure och skapa nya virtuella Linux-datorer (VM) från hello anpassade disken. Du kan installera och konfigurera en Linux distro tooyour krav och sedan använda den virtuella Hårddisken tooquickly skapa en ny Azure virtuell dator.

Om du vill toocreate flera virtuella datorer från anpassade disken, bör du skapa en avbildning från VM- eller VHD. Mer information finns i [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI](tutorial-custom-images.md).

Du kan välja mellan två alternativ:
* [Överföra en virtuell hårddisk](#option-1-upload-a-specialized-vhd)
* [Kopiera en befintlig virtuell Azure-dator](#option-2-copy-an-existing-azure-vm)

## <a name="quick-commands"></a>Snabbkommandon

När du skapar en ny virtuell dator med hjälp av [az vm skapa](/cli/azure/vm#create) från en anpassad eller särskilda disk du **bifoga** hello disk (--bifoga-os-disk) istället för att ange en anpassad eller marketplace-avbildning (--bilden). hello följande exempel skapas en virtuell dator med namnet *myVM* med hello hanterade disk med namnet *myManagedDisk* skapas från den anpassade virtuella Hårddisken:

```azurecli
az vm create --resource-group myResourceGroup --location eastus --name myVM \
   --os-type linux --attach-os-disk myManagedDisk
```

## <a name="requirements"></a>Krav
toocomplete hello följande steg, behöver du:

* En virtuell Linux-dator som har förberetts för användning i Azure. Hej [Förbered hello VM](#prepare-the-vm) i den här artikeln beskriver hur toofind distro specifik information om hur du installerar hello Azure Linux-agenten (waagent) som krävs för hello VM toowork korrekt i Azure och du toobe kan tooconnect tooit via SSH.
* hello VHD-filen från en befintlig [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD-format. Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:
  * Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat. Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med **qemu img konvertera**.
  * Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello senare VHDX-format stöds inte i Azure. När du skapar en virtuell dator kan du ange VHD som hello-format. Om det behövs kan du konvertera VHDX diskar tooVHD med [qemu img konvertera](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [Convert-VHD](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp. Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.
> 
> 


* Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår *myResourceGroup*, *mittlagringskonto*, och *mydisks*.

<a id="prepimage"> </a>

## <a name="prepare-hello-vm"></a>Förbereda hello VM

Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). hello följande artiklar när du går igenom hur tooprepare hello olika Linux-distributioner som stöds i Azure:

* [CentOS-baserade distributioner](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)
* [Andra - icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

Se även hello [Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes) mer allmänna tips om hur du förbereder Linux avbildningar för Azure.

> [!NOTE]
> Hej [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller tooVMs som kör Linux bara när en hello-godkända distributioner används med hello konfigurationsinformation som anges under stöds versioner i [Linux på Azure-godkända Distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="option-1-upload-a-vhd"></a>Alternativ 1: Ladda upp en virtuell Hårddisk

Du kan överföra en anpassad virtuell Hårddisk som du har körs på en lokal dator eller som du exporterade från ett annat moln. toouse hello VHD toocreate en ny Azure VM, behöver du tooupload hello VHD tooa storage-konto och skapa en hanterad disk från hello VHD. 

### <a name="create-a-resource-group"></a>Skapa en resursgrupp

Innan du laddar upp din anpassade disk och skapar virtuella datorer, måste du först toocreate en resursgrupp med [az gruppen skapa](/cli/azure/group#create).

hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats: [översikt över Azure hanterade diskar](../windows/managed-disks-overview.md)
```azurecli
az group create \
    --name myResourceGroup \
    --location eastus
```

### <a name="create-a-storage-account"></a>skapar ett lagringskonto

Skapa ett lagringskonto för anpassade disk- och virtuella datorer med [az storage-konto skapar](/cli/azure/storage/account#create). 

hello följande exempel skapas ett lagringskonto med namnet *mittlagringskonto* i hello resursgruppen skapade tidigare:

```azurecli
az storage account create \
    --resource-group myResourceGroup \
    --location eastus \
    --name mystorageaccount \
    --kind Storage \
    --sku Standard_LRS
```

### <a name="list-storage-account-keys"></a>Lista nycklar för lagringskonto
Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure. Dessa snabbtangenter används vid autentisering toohello storage-konto, som utför skrivåtgärder. Läs mer om [hantera åtkomst till toostorage här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Du visar hello snabbtangenter med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).

Visa hello snabbtangenter för hello storage-konto som du skapade:

```azurecli
az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount
```

hello utdata liknar:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
Anteckna **key1** som du vill använda den toointeract med ditt lagringskonto i hello nästa steg.

### <a name="create-a-storage-container"></a>Skapa en lagringsbehållare
Samma sätt som du skapar olika kataloger toologically ordna det lokala filsystemet i hello skapar behållare i en storage-konto tooorganize diskarna. Ett lagringskonto kan innehålla valfritt antal behållare. Skapa en behållare med [az lagringsbehållaren skapa](/cli/azure/storage/container#create).

hello följande exempel skapas en behållare med namnet *mydisks*:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

### <a name="upload-hello-vhd"></a>Överför hello VHD
Ladda upp din anpassade disk med [az storage blob överför](/cli/azure/storage/blob#upload). Du överför och lagra anpassade disken som en sidblobb.

Ange din åtkomstnyckel, hello-behållare som du skapade i föregående steg i hello och hello sökvägen toohello anpassade disk på den lokala datorn:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 \
    --container-name mydisks \
    --type page \
    --file /path/to/disk/mydisk.vhd \
    --name myDisk.vhd
```
Överför hello VHD kan ta en stund.

### <a name="create-a-managed-disk"></a>Skapa en hanterad disk


Skapa en hanterad disk från hello VHD använder [az disk skapa](/cli/azure/disk#create). hello följande exempel skapas en hanterad disk med namnet *myManagedDisk* från hello VHD du överförde tooyour med namnet lagringskonto och en behållare:

```azurecli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
  --source https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd
```
## <a name="option-2-copy-an-existing-vm"></a>Alternativ 2: Kopiera en befintlig virtuell dator

Du kan också skapa hello anpassade VM i Azure och sedan kopiera hello OS-disk och koppla den tooa nya VM toocreate en annan kopia. Det här är bra för testning, men om du vill toouse en befintlig virtuell Azure-dator som hello modell för flera nya virtuella datorer, verkligen bör du skapa en **bild** i stället. Mer information om hur du skapar en avbildning från en befintlig Azure-VM finns [skapa en anpassad avbildning av en virtuell Azure-dator med hjälp av hello CLI](tutorial-custom-images.md)

### <a name="create-a-snapshot"></a>Skapa en ögonblicksbild

Det här exemplet skapas en ögonblicksbild av en virtuell dator med namnet *myVM* i resursgruppen *myResourceGroup* och skapar en ögonblicksbild med namnet *osDiskSnapshot*.

```azure-cli
osDiskId=$(az vm show -g myResourceGroup -n myVM --query "storageProfile.osDisk.managedDisk.id" -o tsv)
az snapshot create \
    -g myResourceGroup \
    --source "$osDiskId" \
    --name osDiskSnapshot
```
###  <a name="create-hello-managed-disk"></a>Skapa hello hanterade diskar

Skapa en ny hanterade disk från hello ögonblicksbild.

Hämta hello-ID för hello ögonblicksbild. I det här exemplet hello ögonblicksbild heter *osDiskSnapshot* och finns i hello *myResourceGroup* resursgruppen.

```azure-cli
snapshotId=$(az snapshot show --name osDiskSnapshot --resource-group myResourceGroup --query [id] -o tsv)
```

Skapa hello hanterade diskar. I det här exemplet skapar vi en hanterad disk med namnet *myManagedDisk* från våra ögonblicksbild som är 128 GB i storlek i standardlagring.

```azure-cli
az disk create \
    --resource-group myResourceGroup \
    --name myManagedDisk \
    --sku Standard_LRS \
    --size-gb 128 \
    --source $snapshotId
```

## <a name="create-hello-vm"></a>Skapa hello VM

Nu ska du skapa den virtuella datorn med [az vm skapa](/cli/azure/vm#create) och bifoga (--bifoga-os-disk) hello hanteras disk som hello OS-disk. hello följande exempel skapas en virtuell dator med namnet *myNewVM* med hello hanterade disken som skapas från den överförda virtuella Hårddisken:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --location eastus \
    --name myNewVM \
    --os-type linux \
    --attach-os-disk myManagedDisk
```

Du bör vara kan tooSSH till hello VM med hello referenser från Virtuella hello källdatorn. 

## <a name="next-steps"></a>Nästa steg
När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md). Du kan också för[lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nya virtuella datorer. Om du har program som körs på din virtuella dator som du behöver tooaccess för[öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

