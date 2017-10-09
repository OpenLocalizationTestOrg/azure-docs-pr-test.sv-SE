---
title: aaaUpload en anpassad Linux-disk med Azure CLI 2.0 | Microsoft Docs
description: "Skapa och ladda upp en virtuell hårddisk (VHD) tooAzure med hello Resource Manager-modellen och hello Azure CLI 2.0"
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
ms.date: 07/10/2017
ms.author: cynthn
ms.openlocfilehash: cf8556ab4dfe6c4e5eff4e99fe1ddc44393f774c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-hello-azure-cli-20"></a>Ladda upp och skapa en Linux VM från anpassade disken med hello Azure CLI 2.0
Den här artikeln visar hur tooupload en virtuell hårddisk (VHD) tooan Azure storage-konto med hello Azure CLI 2.0 och skapa virtuella Linux-datorer från den här anpassade disken. Du kan också utföra dessa steg med hello [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Den här funktionen kan du tooinstall och konfigurera en Linux distro tooyour krav och sedan använda den virtuella Hårddisken tooquickly skapa virtuella Azure-datorer (VM).

Det här avsnittet använder storage-konton för hello sista virtuella hårddiskar, men du kan också göra följande med hjälp av [hanterade diskar](upload-vhd.md). 

## <a name="quick-commands"></a>Snabbkommandon
Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello bas kommandon tooupload en VHD-tooAzure. Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#requirements).

Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `mydisks`.

Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUs` plats:

```azurecli
az group create --name myResourceGroup --location westus
```

Skapa en storage-konto toohold din virtuella diskar med [az storage-konto skapar](/cli/azure/storage/account#create). hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Visa en lista över hello åtkomstnycklarna för ditt lagringskonto med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list). Anteckna `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Skapa en behållare i ditt lagringskonto med hjälp av hello lagringsnyckel du fick med [az lagringsbehållaren skapa](/cli/azure/storage/container#create). hello följande exempel skapas en behållare med namnet `mydisks` med hello lagring nyckelvärde från `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Slutligen överför din VHD toohello-behållare som du skapat med [az storage blob överför](/cli/azure/storage/blob#upload). Ange hello lokal sökväg tooyour VHD under `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Ange hello URI tooyour disk (`--image`) med [az vm skapa](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet `myVM` med hello virtuella disken överfört tidigare:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Hej destinationslagringskontot har toobe hello samma som om du har överfört din virtuell disk till. Du också behöva toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello **az vm skapa** kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar. Du kan läsa mer om hello [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Krav
toocomplete hello följande steg, behöver du:

* **Linux-operativsystem i en VHD-fil** -installera en [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD format. Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:
  * Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat. Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.
  * Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello senare VHDX-format stöds inte i Azure. När du skapar en virtuell dator kan du ange VHD som hello-format. Om det behövs kan du konvertera VHDX diskar tooVHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp. Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.
> 
> 

* Virtuella datorer skapas från anpassade disken måste finnas i hello samma lagringskonto som själva hello disken
  * Skapa en storage-konto och en behållare toohold både dina anpassade disk och skapade virtuella datorer
  * När du har skapat din virtuella dator, kan du ta bort disken

Se till att du har hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-hello-disk-toobe-uploaded"></a>Förbereda hello disk toobe har överförts
Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). hello följande artiklar när du går igenom hur tooprepare hello olika Linux-distributioner som stöds i Azure:

* **[CentOS-baserade distributioner](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Andra - icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Se även hello  **[Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes)**  mer allmänna tips om hur du förbereder Linux avbildningar för Azure.

> [!NOTE]
> Hej [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller tooVMs som kör Linux bara när en hello-godkända distributioner används med hello konfigurationsinformation som anges under stöds versioner i [Linux på Azure-godkända Distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
Resursgrupper sätta logiskt ihop alla hello Azure-resurser toosupport virtuella datorer, till exempel hello virtuella nätverk och lagring. Resursgrupper för mer information, se [resursgrupper översikt](../../azure-resource-manager/resource-group-overview.md). Innan du laddar upp din anpassade disk och skapar virtuella datorer, måste du först toocreate en resursgrupp med [az gruppen skapa](/cli/azure/group#create).

hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westus` plats:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

Skapa ett lagringskonto för anpassade disk- och virtuella datorer med [az storage-konto skapar](/cli/azure/storage/account#create). Virtuella datorer med ohanterad diskar som du skapar från din anpassade disken måste toobe i hello samma lagringskonto som disken. 

hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount` i hello resursgruppen skapade tidigare:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Lista nycklar för lagringskonto
Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure. Dessa snabbtangenter används vid autentisering toohello storage-konto, till exempel toocarry ut skrivåtgärder. Läs mer om [hantera åtkomst till toostorage här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Du visar hello snabbtangenter med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).

Visa hello snabbtangenter för hello storage-konto som du skapade:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
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
Anteckna `key1` som du vill använda den toointeract med ditt lagringskonto i hello nästa steg.

## <a name="create-a-storage-container"></a>Skapa en lagringsbehållare
Samma sätt som du skapar olika kataloger toologically ordna det lokala filsystemet i hello skapar behållare i en storage-konto tooorganize diskarna. Ett lagringskonto kan innehålla valfritt antal behållare. Skapa en behållare med [az lagringsbehållaren skapa](/cli/azure/storage/container#create).

hello följande exempel skapas en behållare med namnet `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>Överför VHD
Ladda upp din anpassade disk med [az storage blob överför](/cli/azure/storage/blob#upload). Du överför och lagra anpassade disken som en sidblobb.

Ange din åtkomstnyckel, hello-behållare som du skapade i föregående steg i hello och hello sökvägen toohello anpassade disk på den lokala datorn:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-hello-vm"></a>Skapa hello VM
toocreate en virtuell dator med ohanterad diskar, ange hello URI tooyour disk (`--image`) med [az vm skapa](/cli/azure/vm#create). hello följande exempel skapas en virtuell dator med namnet `myVM` med hello virtuella disken överfört tidigare:

Du anger hello `--image` parameter med [az vm skapa](/cli/azure/vm#create) toopoint tooyour anpassade disk. Se till att `--storage-account` matchar hello lagringskonto där anpassade disken ska lagras. Du har inte toouse hello samma behållare som hello anpassade disk toostore dina virtuella datorer. Se till att toocreate eventuella ytterligare behållare i hello samma sätt som hello tidigare steg innan du laddar upp din anpassade disk.

hello följande exempel skapas en virtuell dator med namnet `myVM` från anpassade disken:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Du fortfarande behöver toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello **az vm skapa** kommandot, till exempel användarnamn och SSH-nycklar.


## <a name="resource-manager-template"></a>Resource Manager-mall
Azure Resource Manager-mallar är JavaScript Object Notation (JSON) filer som definierar hello-miljö som du vill toobuild. hello mallar är uppdelade i toodifferent resursproviders, till exempel beräkning eller nätverket. Du kan använda befintliga mallar eller skriva egna. Läs mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).

Inom hello `Microsoft.Compute/virtualMachines` providern av mallen som du har en `storageProfile` nod som innehåller hello konfigurationsinformation för den virtuella datorn. hello två huvudsakliga parametrar tooedit är hello `image` och `vhd` URI: er som pekar tooyour anpassade disk och den nya Virtuella datorns virtuella disk. hello nedan visar ett exempel på hello JSON för att använda en anpassad disk:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Du kan använda [detta befintliga mallen toocreate en virtuell dator från en anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) eller läser du om [skapa egna mallar för Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md). 

När du har en mall kan du använda [az distribution skapa](/cli/azure/group/deployment#create) toocreate dina virtuella datorer. Ange hello URI för JSON-mall med hello `--template-uri` parameter:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Om du har en JSON-fil som lagras lokalt på datorn, kan du använda hello `--template-file` parameter i stället:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Nästa steg
När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md). Du kan också för[lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nya virtuella datorer. Om du har program som körs på din virtuella dator som du behöver tooaccess för[öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

