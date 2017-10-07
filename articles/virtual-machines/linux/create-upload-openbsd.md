---
title: aaaCreate och ladda upp en OpenBSD VM bild tooAzure | Microsoft Docs
description: "Lär dig hur toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello OpenBSD operativsystemet toocreate en virtuell Azure-dator via Azure CLI"
services: virtual-machines-linux
documentationcenter: 
author: KylieLiang
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 1ef30f32-61c1-4ba8-9542-801d7b18e9bf
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/24/2017
ms.author: kyliel
ms.openlocfilehash: 0524f45ea1ecec37e55cbe9d54438a571a831de7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-upload-an-openbsd-disk-image-tooazure"></a>Skapa och ladda upp en OpenBSD disk image-tooAzure
Den här artikeln visar hur toocreate och ladda upp en virtuell hårddisk (VHD) som innehåller hello OpenBSD operativsystem. När du har överfört kan du använda den som din egen avbildning toocreate en virtuell dator (VM) i Azure med Azure CLI.


## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du har hello följande objekt:

* **En Azure-prenumeration** -om du inte har ett konto kan du skapa en på bara några minuter. Om du har en MSDN-prenumeration, se [månatliga Azure-kredit för Visual Studio-prenumeranter](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/). Annars kan du lära dig hur för[skapa ett kostnadsfritt utvärderingskonto](https://azure.microsoft.com/pricing/free-trial/).  
* **Azure CLI 2.0** -Kontrollera att du har hello senaste [Azure CLI 2.0](/cli/azure/install-azure-cli) installerad och inloggad tooyour Azure-konto med [az inloggningen](/cli/azure/#login).
* **OpenBSD operativsystem i en VHD-fil** -en OpenBSD operativsystem som stöds (version 6.1) måste vara installerade tooa virtuell hårddisk. Flera verktyg finns toocreate VHD-filer. Du kan till exempel använda en virtualiseringslösning till exempel Hyper-V toocreate hello VHD-filen och installera hello-operativsystemet. Anvisningar om hur tooinstall och använda Hyper-V, se [installera Hyper-V och skapa en virtuell dator](http://technet.microsoft.com/library/hh846766.aspx).


## <a name="prepare-openbsd-image-for-azure"></a>Förbereda OpenBSD bild för Azure
Slutför hello följande procedurer på hello VM där du installerade hello OpenBSD operativsystemet 6.1, som läggs till Hyper-V-stöd:

1. Om DHCP inte är aktiverad under installationen, aktivera hello-tjänsten på följande sätt:

    ```sh    
    echo dhcp > /etc/hostname.hvn0
    ```

2. Ställ in en seriekonsolen enligt följande:

    ```sh
    echo "stty com0 115200" >> /etc/boot.conf
    echo "set tty com0" >> /etc/boot.conf
    ```

3. Konfigurera installationen enligt följande:

    ```sh
    echo "https://ftp.openbsd.org/pub/OpenBSD" > /etc/installurl
    ```
   
4. Som standard hello `root` användaren är inaktiverad på virtuella datorer i Azure. Användare kan köra kommandon med utökade privilegier med hjälp av hello `doas` på OpenBSD VM. Doas är aktiverad som standard. Mer information finns i [doas.conf](http://man.openbsd.org/doas.conf.5). 

5. Installera och konfigurera krav för hello Azure-agenten på följande sätt:

    ```sh
    pkg_add py-setuptools openssl git
    ln -sf /usr/local/bin/python2.7 /usr/local/bin/python
    ln -sf /usr/local/bin/python2.7-2to3 /usr/local/bin/2to3
    ln -sf /usr/local/bin/python2.7-config /usr/local/bin/python-config
    ln -sf /usr/local/bin/pydoc2.7  /usr/local/bin/pydoc
    ```

6. hello senaste versionen av hello Azure-agenten alltid finns på [Github](https://github.com/Azure/WALinuxAgent/releases). Installera hello-agenten på följande sätt:

    ```sh
    git clone https://github.com/Azure/WALinuxAgent 
    cd WALinuxAgent
    python setup.py install
    waagent -register-service
    ```

    > [!IMPORTANT]
    > När du har installerat Azure-agenten är det en bra idé tooverify körs på följande sätt:
    >
    > ```bash
    > ps auxw | grep waagent
    > root     79309  0.0  1.5  9184 15356 p1  S      4:11PM    0:00.46 python /usr/local/sbin/waagent -daemon (python2.7)
    > cat /var/log/waagent.log
    > ```

7. Avetablering hello system tooclean den och gör den lämplig för reprovisioning. hello följande kommando tas även bort hello senaste etablerade användarkontot och hello associerade data:

    ```sh
    waagent -deprovision+user -force
    ```

Nu kan du stänga av den virtuella datorn.


## <a name="prepare-hello-vhd"></a>Förbereda hello VHD
Hej VHDX-format stöds inte i Azure, endast **fast virtuell Hårddisk**. Du kan konvertera hello disk toofixed VHD-format med hjälp av Hyper-V Manager eller hello Powershell [convert-vhd](https://technet.microsoft.com/itpro/powershell/windows/hyper-v/convert-vhd) cmdlet. Ett exempel är följande.

```powershell
Convert-VHD OpenBSD61.vhdx OpenBSD61.vhd -VHDType Fixed
```

## <a name="create-storage-resources-and-upload"></a>Skapa storage-resurser och ladda upp
Börja med att skapa en resursgrupp med [az gruppen skapa](/cli/azure/group#create). hello följande exempel skapar en resursgrupp med namnet *myResourceGroup* i hello *eastus* plats:

```azurecli
az group create --name myResourceGroup --location eastus
```

tooupload den virtuella Hårddisken, skapa ett lagringskonto med [az storage-konto skapar](/cli/azure/storage/account#create). Lagringskontonamn måste vara unikt, så ange dina egna namn. hello följande exempel skapas ett lagringskonto med namnet *mittlagringskonto*:

```azurecli
az storage account create --resource-group myResourceGroup \
    --name mystorageaccount \
    --location eastus \
    --sku Premium_LRS
```

toocontrol åtkomst toohello lagringskontot kan du få hello lagringsnyckel med [az nycklar lagringskontolistan](/cli/azure/storage/account/keys#list) på följande sätt:

```azurecli
STORAGE_KEY=$(az storage account keys list \
    --resource-group myResourceGroup \
    --account-name mystorageaccount \
    --query "[?keyName=='key1']  | [0].value" -o tsv)
```

toologically separat hello virtuella hårddiskar som du överför, skapa en behållare i hello lagringskonto med [az lagringsbehållaren skapa](/cli/azure/storage/container#create):

```azurecli
az storage container create \
    --name vhds \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```

Slutligen överför den virtuella Hårddisken med [az storage blob överför](/cli/azure/storage/blob#upload) på följande sätt:

```azurecli
az storage blob upload \
    --container-name vhds \
    --file ./OpenBSD61.vhd \
    --name OpenBSD61.vhd \
    --account-name mystorageaccount \
    --account-key ${STORAGE_KEY}
```


## <a name="create-vm-from-your-vhd"></a>Skapa virtuell dator från den virtuella Hårddisken
Du kan skapa en virtuell dator med en [exempel på skript](../scripts/virtual-machines-linux-cli-sample-create-vm-vhd.md) eller direkt med [az vm skapa](/cli/azure/vm#create). toospecify hello OpenBSD VHD du överförde Använd hello `--image` parameter på följande sätt:

```azurecli
az vm create \
    --resource-group myResourceGroup \
    --name myOpenBSD61 \
    --image "https://mystorageaccount.blob.core.windows.net/vhds/OpenBSD61.vhd" \
    --os-type linux \
    --admin-username azureuser \
    --ssh-key-value ~/.ssh/id_rsa.pub
```

Hämta hello IP-adress för din OpenBSD VM med [az vm lista-ip-adresser](/cli/azure/vm#list-ip-addresses) på följande sätt:

```azurecli
az vm list-ip-addresses --resource-group myResourceGroup --name myOpenBSD61
```

Du kan nu SSH tooyour OpenBSD VM som vanligt:
        
```bash
ssh azureuser@<ip address>
```


## <a name="next-steps"></a>Nästa steg
Om du vill tooknow mer om Hyper-V stöd på OpenBSD6.1 läsa [OpenBSD 6.1](https://www.openbsd.org/61.html) och [hyperv.4](http://man.openbsd.org/hyperv.4).

Om du vill toocreate en virtuell dator från hanterade diskar kan läsa [az disk](/cli/azure/disk). 
