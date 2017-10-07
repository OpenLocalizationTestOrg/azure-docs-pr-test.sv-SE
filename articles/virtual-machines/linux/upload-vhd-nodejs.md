---
title: aaaUpload en anpassad avbildning Linux med Azure CLI 1.0 | Microsoft Docs
description: "Skapa och ladda upp en virtuell hårddisk (VHD) tooAzure med en anpassad avbildning för Linux med hjälp av hello Resource Manager-modellen och hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: a8c7818f-eb65-409e-aa91-ce5ae975c564
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 10/10/2016
ms.author: iainfou
ms.openlocfilehash: 0bbd232debee1e632233d1c010e388dbc1548bf3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-image-by-using-hello-azure-cli-10"></a>Ladda upp och skapa en Linux VM av anpassade diskavbildning med hello Azure CLI 1.0
Den här artikeln visar hur tooupload som en virtuell hårddisk (VHD) tooAzure med hello Resource Manager-distributionsmodellen och skapar virtuella Linux-datorer från den här anpassade avbildningen. Den här funktionen kan du tooinstall och konfigurera en Linux distro tooyour krav och sedan använda den virtuella Hårddisken tooquickly skapa virtuella Azure-datorer (VM).


## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-commands) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) -vår nästa generations CLI för hello resursdistributionsmodell för hantering


## <a name="quick-commands"></a>Snabbkommandon
Om du behöver tooquickly utföra hello uppgiften, hello följande avsnitt information hello bas kommandon tooupload tooAzure en VM. Mer detaljerad information och kontext för varje steg finns hello resten av dokumentet hello [startar här](#requirements).

Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `myimages`.

Först skapa en resursgrupp. hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUs` plats:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

Skapa en storage-konto toohold din virtuella diskar. hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

Lista över hello åtkomstnycklarna för ditt lagringskonto. Anteckna `key1`:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
```

Skapa en behållare i ditt lagringskonto med hjälp av hello lagringsnyckel som du fick. hello följande exempel skapas en behållare med namnet `myimages` med hello lagring nyckelvärde från `key1`:

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

Slutligen överför din VHD toohello-behållare som du skapade. Ange hello lokal sökväg tooyour VHD under `/path/to/disk/mydisk.vhd`:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

Nu kan du skapa en virtuell dator från den överförda virtuella hårddisken [med hjälp av en Resource Manager-mall](https://github.com/Azure/azure-quickstart-templates/tree/master/201-vm-from-specialized-vhd). Du kan också använda hello CLI genom att ange hello URI tooyour disk (`--image-urn`). hello följande exempel skapas en virtuell dator med namnet `myVM` med hello virtuella disken överfört tidigare:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
```

Hej destinationslagringskontot har toobe hello samma som om du har överfört din virtuell disk till. Du också behöva toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello `azure vm create` kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar. Du kan läsa mer om hello [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Krav
toocomplete hello följande steg, behöver du:

* **Linux-operativsystem i en VHD-fil** -installera en [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) tooa virtuell disk i hello VHD format. Flera verktyg finns toocreate en virtuell dator och virtuell Hårddisk:
  * Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), sköter toouse VHD som bildformat. Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.
  * Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> hello senare VHDX-format stöds inte i Azure. När du skapar en virtuell dator kan du ange VHD som hello-format. Om det behövs kan du konvertera VHDX diskar tooVHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller hello [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Dessutom stöder Azure inte uppladdning dynamiska virtuella hårddiskar, så du måste tooconvert sådana diskar toostatic virtuella hårddiskar innan du laddar upp. Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) tooconvert dynamiska diskar under hello processen överför tooAzure.
> 
> 

* Virtuella datorer skapas från den anpassade avbildningen måste finnas i hello samma lagringskonto som själva hello-bilden
  * Skapa en storage-konto och en behållare toohold både dina anpassade avbildningen och skapade virtuella datorer
  * När du har skapat din virtuella dator, kan du ta bort bilden

Se till att du har [hello Azure CLI 1.0](../../cli-install-nodejs.md) loggas och med hjälp av Resource Manager-läge:

```azurecli
azure config mode arm
```

Följande exempel Ersätt exempel parameternamn i hello med egna värden. Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `myimages`.

<a id="prepimage"> </a>

## <a name="prepare-hello-image-toobe-uploaded"></a>Förbereda hello avbildningen toobe har överförts
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


## <a name="create-a-resource-group"></a>Skapa en resursgrupp
Resursgrupper sätta logiskt ihop alla hello Azure-resurser toosupport virtuella datorer, till exempel hello virtuella nätverk och lagring. Läs mer om [Azure resursgrupper här](../../azure-resource-manager/resource-group-overview.md). Innan du överför din anpassade diskavbildning och skapa virtuella datorer, måste du först toocreate en resursgrupp. 

hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `WestUS` plats:

```azurecli
azure group create myResourceGroup --location "WestUS"
```

## <a name="create-a-storage-account"></a>skapar ett lagringskonto
Virtuella datorer lagras som sidblobbar i ett lagringskonto. Läs mer om [Azure-blobblagring här](../../storage/common/storage-introduction.md#blob-storage). Du skapar ett lagringskonto för dina anpassade diskavbildning och virtuella datorer. Virtuella datorer som du skapar anpassade disk image måste toobe i hello samma lagringskonto som den bilden.

hello följande exempel skapas ett lagringskonto med namnet `mystorageaccount` i hello resursgruppen skapade tidigare:

```azurecli
azure storage account create mystorageaccount --resource-group myResourceGroup \
    --location "WestUS" --kind Storage --sku-name PLRS
```

## <a name="list-storage-account-keys"></a>Lista nycklar för lagringskonto
Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure. Dessa snabbtangenter används vid autentisering toohello storage-konto, till exempel toocarry ut skrivåtgärder. Läs mer om [hantera åtkomst till toostorage här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Du kan visa snabbtangenter med hello `azure storage account keys list` kommando.

Visa hello snabbtangenter för hello storage-konto som du skapade:

```azurecli
azure storage account keys list mystorageaccount --resource-group myResourceGroup
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
Samma sätt som du skapar olika kataloger toologically ordna det lokala filsystemet i hello skapar behållare i en storage-konto tooorganize dina virtuella diskar och bilder. Ett lagringskonto kan innehålla valfritt antal behållare. 

hello följande exempel skapas en behållare med namnet `myimages`, att ange hello åtkomstnyckel hämtades i föregående steg i hello (`key1`):

```azurecli
azure storage container create --account-name mystorageaccount \
    --account-key key1 --container myimages
```

## <a name="upload-vhd"></a>Överför VHD
Nu kan du faktiskt överföra din egen avbildning. Som med alla virtuella diskar som används av virtuella datorer kan du ladda upp och lagra din egen avbildning som en sidblobb.

Ange din åtkomstnyckel, hello-behållare som du skapade i föregående steg i hello och hello sökvägen toohello anpassade diskavbildning på den lokala datorn:

```azurecli
azure storage blob upload --blobtype page --account-name mystorageaccount \
    --account-key key1 --container myimages /path/to/disk/mydisk.vhd
```

## <a name="create-vm-from-custom-image"></a>Skapa virtuell dator från en anpassad avbildning
När du skapar virtuella datorer från din anpassade diskavbildning ange hello URI toohello diskavbildning. Se till att hello mål storage-konto matchar där din anpassade diskavbildning lagras. Du kan skapa den virtuella datorn med hjälp av hello Azure CLI eller Resource Manager JSON-mall.

### <a name="create-a-vm-using-hello-azure-cli"></a>Skapa en virtuell dator med hjälp av hello Azure CLI
Du anger hello `--image-urn` parameter med hello `azure vm create` kommandot toopoint tooyour anpassad avbildning. Se till att `--storage-account-name` matchar hello lagringskonto där din anpassade diskavbildning ska lagras. Du har inte toouse hello samma behållare som hello anpassade disk image toostore dina virtuella datorer. Se till att toocreate eventuella ytterligare behållare i hello samma sätt som hello tidigare steg innan du laddar upp din anpassade diskavbildningar.

hello följande exempel skapas en virtuell dator med namnet `myVM` från din egen avbildning:

```azurecli
azure vm create myVM -l "WestUS" --resource-group myResourceGroup \
    --image-urn https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd
    --storage-account-name mystorageaccount
```

Du fortfarande behöver toospecify eller besvara efterfrågas, alla hello ytterligare parametrar som krävs av hello `azure vm create` kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar. Läs mer om hello [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

### <a name="create-a-vm-using-a-json-template"></a>Skapa en virtuell dator med en JSON-mall
Azure Resource Manager-mallar är JavaScript Object Notation (JSON) filer som definierar hello-miljö som du vill toobuild. hello mallar är uppdelade i toodifferent resursproviders, till exempel beräkning eller nätverket. Du kan använda befintliga mallar eller skriva egna. Läs mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).

Inom hello `Microsoft.Compute/virtualMachines` providern av mallen som du har en `storageProfile` nod som innehåller hello konfigurationsinformation för den virtuella datorn. hello två huvudsakliga parametrar tooedit är hello `image` och `vhd` URI: er som tooyour anpassade diskavbildning och den nya Virtuella datorns virtuella disken. hello nedan visar ett exempel på hello JSON för att använda en anpassad avbildning:

```json
"storageProfile": {
          "osDisk": {
            "name": "myVM",
            "osType": "Linux",
            "caching": "ReadWrite",
            "createOption": "FromImage",
            "image": {
              "uri": "https://mystorageaccount.blob.core.windows.net/myimages/mydisk.vhd"
            },
            "vhd": {
              "uri": "https://mystorageaccount.blob.core.windows.net/vhds/newvmname.vhd"
            }
          }
```

Du kan använda [detta befintliga mallen toocreate en virtuell dator från en anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) eller läser du om [skapa egna mallar för Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md). 

När du har en mall kan du skapa dina virtuella datorer med hjälp av hello `azure group deployment create` kommando. Ange hello URI för JSON-mall med hello `--template-uri` parameter:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-uri https://uri.to.template/mytemplate.json
```

Om du har en JSON-fil som lagras lokalt på datorn, kan du använda hello `--template-file` parameter i stället:

```azurecli
azure group deployment create --resource-group myResourceGroup
    --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Nästa steg
När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md). Du kan också för[lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) tooyour nya virtuella datorer. Om du har program som körs på din virtuella dator som du behöver tooaccess för[öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

