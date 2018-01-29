---
title: Ladda upp en anpassad Linux-disk med Azure CLI 2.0 | Microsoft Docs
description: "Skapa och ladda upp en virtuell hårddisk (VHD) till Azure med hjälp av Resource Manager-distributionsmodellen och Azure CLI 2.0"
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
ms.openlocfilehash: 9159960af396e89f373da711e0cc46fdd996ab83
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/11/2017
---
# <a name="upload-and-create-a-linux-vm-from-custom-disk-with-the-azure-cli-20"></a>Ladda upp och skapa en Linux VM från anpassade disken med Azure CLI 2.0
Den här artikeln visar hur du överför en virtuell hårddisk (VHD) till ett Azure storage-konto med Azure CLI 2.0 och skapar virtuella Linux-datorer från den här anpassade disken. Du kan också utföra dessa steg med [Azure CLI 1.0](upload-vhd-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json). Den här funktionen kan du installera och konfigurera en Linux-distro enligt dina behov och sedan använda den virtuella Hårddisken för att snabbt skapa virtuella Azure-datorer (VM).

Det här avsnittet använder storage-konton för de sista virtuella hårddiskarna, men du kan också göra följande med hjälp av [hanterade diskar](upload-vhd.md). 

## <a name="quick-commands"></a>Snabbkommandon
Om du behöver utföra aktiviteten följande avsnittet beskriver grundläggande kommandon för att överföra en virtuell Hårddisk till Azure. Mer detaljerad information och kontext för varje steg finns resten av dokumentet [startar här](#requirements).

Se till att du har senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).

Ersätt exempel parameternamn med egna värden i följande exempel. Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `mydisks`.

Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `WestUs` plats:

```azurecli
az group create --name myResourceGroup --location westus
```

Skapa ett lagringskonto för att lagra dina virtuella diskar med [az storage-konto skapar](/cli/azure/storage/account#create). I följande exempel skapas ett lagringskonto med namnet `mystorageaccount`:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

Visa en lista med åtkomstnycklarna för ditt lagringskonto med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list). Anteckna `key1`:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Skapa en behållare i ditt lagringskonto med hjälp av lagringsnyckeln du fick med [az lagringsbehållaren skapa](/cli/azure/storage/container#create). I följande exempel skapas en behållare med namnet `mydisks` med lagring nyckelvärdet från `key1`:

```azurecli
az storage container create --account-name mystorageaccount \
    --account-key key1 --name mydisks
```

Slutligen överför den virtuella Hårddisken till den behållare som du skapat med [az storage blob överför](/cli/azure/storage/blob#upload). Ange den lokala sökvägen till den virtuella Hårddisken under `/path/to/disk/mydisk.vhd`:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

Ange URI: N till hårddisken (`--image`) med [az vm skapa](/cli/azure/vm#create). I följande exempel skapas en virtuell dator med namnet `myVM` med hjälp av den virtuella disken överfört tidigare:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisk/myDisks.vhd \
    --use-unmanaged-disk
```

Mål-lagringskontot måste vara samma som om du har överfört din virtuell disk till. Du måste också ange eller svar efterfrågar alla ytterligare parametrar som krävs av den **az vm skapa** kommandot, till exempel virtuella nätverk, offentlig IP-adress, användarnamn och SSH-nycklar. Du kan läsa mer om den [tillgängliga CLI Resource Manager parametrar](../azure-cli-arm-commands.md#azure-vm-commands-to-manage-your-azure-virtual-machines).

## <a name="requirements"></a>Krav
Du behöver följande för att slutföra följande steg:

* **Linux-operativsystem i en VHD-fil** -installera en [Azure-godkända Linux-distribution](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) (eller se [information för icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)) till en virtuell disk i VHD-format. Det finns flera verktyg för att skapa en virtuell dator och virtuell Hårddisk:
  * Installera och konfigurera [QEMU](https://en.wikibooks.org/wiki/QEMU/Installing_QEMU) eller [KVM](http://www.linux-kvm.org/page/RunningKVM), och ser till att använda VHD som bildformat. Om det behövs kan du [konvertera en bild](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) med `qemu-img convert`.
  * Du kan också använda Hyper-V [på Windows 10](https://msdn.microsoft.com/virtualization/hyperv_on_windows/quick_start/walkthrough_install) eller [på Windows Server 2012/2012 R2](https://technet.microsoft.com/library/hh846766.aspx).

> [!NOTE]
> Nyare VHDX-format stöds inte i Azure. När du skapar en virtuell dator kan du ange VHD som formatet. Om det behövs, du kan konvertera VHDX-diskar på VHD med [ `qemu-img convert` ](https://en.wikibooks.org/wiki/QEMU/Images#Converting_image_formats) eller [ `Convert-VHD` ](https://technet.microsoft.com/library/hh848454.aspx) PowerShell-cmdlet. Dessutom stöder inte Azure överför dynamiska virtuella hårddiskar, så du måste konvertera dessa diskar till statiska virtuella hårddiskar innan du laddar upp. Du kan använda verktyg som [Azure VHD-verktyg för att gå](https://github.com/Microsoft/azure-vhd-utils-for-go) att konvertera dynamiska diskar under processen med att ladda upp till Azure.
> 
> 

* Virtuella datorer skapas från anpassade disken måste finnas i samma lagringskonto som själva disken
  * Skapa ett lagringskonto och en behållare för både dina anpassade disk och skapade virtuella datorer
  * När du har skapat din virtuella dator, kan du ta bort disken

Se till att du har senast [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och inloggad till en Azure-konto med hjälp av [az inloggningen](/cli/azure/#login).

Ersätt exempel parameternamn med egna värden i följande exempel. Exempel parameternamn ingår `myResourceGroup`, `mystorageaccount`, och `mydisks`.

<a id="prepimage"> </a>

## <a name="prepare-the-disk-to-be-uploaded"></a>Förbereda disken som ska överföras
Azure stöder olika Linux-distributioner (se [godkända distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)). I följande artiklar när du går igenom hur du förbereder de olika Linux-distributioner som stöds i Azure:

* **[CentOS-baserade distributioner](create-upload-centos.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Debian Linux](debian-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Oracle Linux](oracle-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Red Hat Enterprise Linux](redhat-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[SLES & openSUSE](suse-create-upload-vhd.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Ubuntu](create-upload-ubuntu.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**
* **[Andra - icke-godkända distributioner](create-upload-generic.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)**

Se även den  **[Linux installationsinformation](create-upload-generic.md#general-linux-installation-notes)**  mer allmänna tips om hur du förbereder Linux avbildningar för Azure.

> [!NOTE]
> Den [Azure-plattformen SLA](https://azure.microsoft.com/support/legal/sla/virtual-machines/) gäller för virtuella datorer som kör Linux endast när en av de påtecknade distributioner används med konfigurationsinformation som anges under stöds versioner i [Linux på Azure-Endorsed distributioner](endorsed-distros.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).
> 
> 

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
Resursgrupper samordnar logiskt alla Azure-resurser som stöder virtuella datorer, till exempel virtuella nätverk och lagring. Resursgrupper för mer information, se [resursgrupper översikt](../../azure-resource-manager/resource-group-overview.md). Innan du laddar upp din anpassade disk och skapar virtuella datorer, måste du först skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create).

I följande exempel skapas en resursgrupp med namnet `myResourceGroup` i den `westus` plats:

```azurecli
az group create --name myResourceGroup --location westus
```

## <a name="create-a-storage-account"></a>skapar ett lagringskonto

Skapa ett lagringskonto för anpassade disk- och virtuella datorer med [az storage-konto skapar](/cli/azure/storage/account#create). Virtuella datorer med ohanterad diskar som du skapar från din anpassade disk måste vara i samma lagringskonto som disken. 

I följande exempel skapas ett lagringskonto med namnet `mystorageaccount` i resursgruppen som du skapade tidigare:

```azurecli
az storage account create --resource-group myResourceGroup --location westus \
  --name mystorageaccount --kind Storage --sku Standard_LRS
```

## <a name="list-storage-account-keys"></a>Lista nycklar för lagringskonto
Två 512-bitars åtkomstnycklar för varje lagringskonto genererar Azure. Åtkomstnycklarna används vid autentisering till lagringskontot, såsom för att utföra skrivåtgärder. Läs mer om [hantera åtkomst till lagring här](../../storage/common/storage-create-storage-account.md#manage-your-storage-account). Du kan visa snabbtangenterna med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list).

Visa åtkomstnycklar för lagringskontot som du skapade:

```azurecli
az storage account keys list --resource-group myResourceGroup --account-name mystorageaccount
```

Utdatan liknar följande:

```azurecli
info:    Executing command storage account keys list
+ Getting storage account keys
data:    Name  Key                                                                                       Permissions
data:    ----  ----------------------------------------------------------------------------------------  -----------
data:    key1  d4XAvZzlGAgWdvhlWfkZ9q4k9bYZkXkuPCJ15NTsQOeDeowCDAdB80r9zA/tUINApdSGQ94H9zkszYyxpe8erw==  Full
data:    key2  Ww0T7g4UyYLaBnLYcxIOTVziGAAHvU+wpwuPvK4ZG0CDFwu/mAxS/YYvAQGHocq1w7/3HcalbnfxtFdqoXOw8g==  Full
info:    storage account keys list command OK
```
Anteckna `key1` eftersom du ska använda för att interagera med ditt lagringskonto i nästa steg.

## <a name="create-a-storage-container"></a>Skapa en lagringsbehållare
På samma sätt som du skapar olika kataloger för att organisera logiskt det lokala filsystemet, kan du skapa behållare i ett lagringskonto för att organisera dina diskar. Ett lagringskonto kan innehålla valfritt antal behållare. Skapa en behållare med [az lagringsbehållaren skapa](/cli/azure/storage/container#create).

I följande exempel skapas en behållare med namnet `mydisks`:

```azurecli
az storage container create \
    --account-name mystorageaccount \
    --name mydisks
```

## <a name="upload-vhd"></a>Överför VHD
Ladda upp din anpassade disk med [az storage blob överför](/cli/azure/storage/blob#upload). Du överför och lagra anpassade disken som en sidblobb.

Ange din snabbtangent, den behållare som du skapade i föregående steg och sökvägen till den anpassa disken på den lokala datorn:

```azurecli
az storage blob upload --account-name mystorageaccount \
    --account-key key1 --container-name mydisks --type page \
    --file /path/to/disk/mydisk.vhd --name myDisk.vhd
```

## <a name="create-the-vm"></a>Skapa den virtuella datorn
Ange URI: N på disken för att skapa en virtuell dator med ohanterad diskar (`--image`) med [az vm skapa](/cli/azure/vm#create). I följande exempel skapas en virtuell dator med namnet `myVM` med hjälp av den virtuella disken överfört tidigare:

Du anger den `--image` parameter med [az vm skapa](/cli/azure/vm#create) så att den pekar till din anpassade disk. Se till att `--storage-account` matchar det lagringskonto där anpassade disken lagras. Du behöver inte använda samma behållare som den anpassa disken för att lagra dina virtuella datorer. Se till att skapa några ytterligare behållare på samma sätt som de föregående stegen innan du laddar upp din anpassade disk.

I följande exempel skapas en virtuell dator med namnet `myVM` från anpassade disken:

```azurecli
az vm create --resource-group myResourceGroup --location westus \
    --name myVM --storage-account mystorageaccount --os-type linux \
    --admin-username azureuser --ssh-key-value ~/.ssh/id_rsa.pub \
    --image https://mystorageaccount.blob.core.windows.net/mydisks/myDisk.vhd \
    --use-unmanaged-disk
```

Du måste ange eller svar efterfrågar alla ytterligare parametrar som krävs av den **az vm skapa** kommandot, till exempel användarnamn och SSH-nycklar.


## <a name="resource-manager-template"></a>Resource Manager-mall
Azure Resource Manager-mallar är JavaScript Object Notation (JSON) filer som definierar den miljö du vill skapa. Mallarna är uppdelade att olika resursproviders, till exempel beräkning eller nätverket. Du kan använda befintliga mallar eller skriva egna. Läs mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md).

I den `Microsoft.Compute/virtualMachines` providern av mallen som du har en `storageProfile` nod som innehåller konfigurationsinformation för den virtuella datorn. Så här redigerar du två huvudsakliga parametrarna är den `image` och `vhd` URI: er som pekar på anpassade disken och din nya VM virtuell disk. Nedan visas ett exempel på JSON för att använda en anpassad disk:

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

Du kan använda [denna befintlig mall för att skapa en virtuell dator från en anpassad avbildning](https://github.com/Azure/azure-quickstart-templates/tree/master/101-vm-from-user-image) eller läser du om [skapa egna mallar för Azure Resource Manager](../../azure-resource-manager/resource-group-authoring-templates.md). 

När du har en mall kan du använda [az distribution skapa](/cli/azure/group/deployment#create) att skapa din virtuella dator. Ange URI för JSON-mall med det `--template-uri` parameter:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-uri https://uri.to.template/mytemplate.json
```

Om du har en JSON-fil som lagras lokalt på datorn, kan du använda den `--template-file` parameter i stället:

```azurecli
az group deployment create --resource-group myNewResourceGroup \
  --template-file /path/to/mytemplate.json
```


## <a name="next-steps"></a>Nästa steg
När du har förberett och överföra din egen virtuell disk, kan du läsa mer om [med hjälp av hanteraren för filserverresurser och mallar](../../azure-resource-manager/resource-group-overview.md). Du kan också [lägger till en datadisk](add-disk.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) till din nya virtuella datorer. Om du har program som körs på din virtuella dator som du behöver åtkomst till [öppna portar och slutpunkter](nsg-quickstart.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

