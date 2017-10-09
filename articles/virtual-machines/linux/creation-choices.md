---
title: "aaaDifferent sätt toocreate en Linux VM i Azure | Microsoft Azure"
description: "Lär dig hello olika sätt toocreate en Linux-dator i Azure, inklusive länkar tootools och självstudier för varje metod."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.topic: get-started-article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 250e92c063c87a8c1279097dc2264777d95478d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="different-ways-toocreate-a-linux-vm"></a>Olika sätt toocreate en Linux VM
Har du hello flexibilitet i Azure toocreate en Linux-dator (VM) med hjälp av verktyg och arbetsflöden föredrar tooyou. Den här artikeln sammanfattar dessa skillnader och exempel för att skapa din virtuella Linux-datorer, inklusive hello Azure CLI 2.0. Du kan också visa skapa alternativ, inklusive hello [Azure CLI 1.0](creation-choices-nodejs.md).

Hej [Azure CLI 2.0](/cli/azure/install-az-cli2) är tillgänglig mellan plattformar via ett npm-paket, som tillhandahålls av distro paket eller dockerbehållare. Installera hello lämpligaste build för din miljö och logga in tooan Azure-konto med [az inloggning](/cli/azure/#login)

* [Skapa en Linux VM med hello Azure CLI 2.0](quick-create-cli.md)
  
  * Skapa en resursgrupp med [az group create](/cli/azure/group#create) med namnet *myResourceGroup*: 
   
    ```azurecli
    az group create --name myResourceGroup --location eastus
    ```
    
  * Skapa en virtuell dator med [az vm skapa](/cli/azure/vm#create) med namnet *myVM* med hello senaste *UbuntuLTS* avbildningen och generera SSH-nycklar, om de inte redan finns i *~/.ssh*:

    ```azurecli
    az vm create \
        --resource-group myResourceGroup \
        --name myVM \
        --image UbuntuLTS \
        --generate-ssh-keys
    ```

* [Skapa en virtuell Linux-dator med hjälp av en Azure-mall](create-ssh-secured-vm-from-template.md)
  
  * hello följande exempel används [az distribution skapa](/cli/azure/group/deployment#create) toocreate en virtuell dator från en mall som lagras på GitHub:
    
    ```azurecli
    az group deployment create --resource-group myResourceGroup \ 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
      --parameters @myparameters.json
    ```
* [Skapa en virtuell Linux-dator och anpassa med cloud-init](tutorial-automate-vm-deployment.md)

* [Skapa en belastningsutjämnat och mycket tillgängligt program på flera virtuella Linux-datorer](tutorial-load-balancer.md)


## <a name="azure-portal"></a>Azure Portal
Hej [Azure-portalen](https://portal.azure.com) kan du tooquickly skapa en virtuell dator eftersom det inte finns något tooinstall på datorn. Använd hello Azure portal toocreate hello VM:

* [Skapa en Linux VM som använder hello Azure-portalen](quick-create-portal.md) 


## <a name="operating-system-and-image-choices"></a>Alternativ för operativsystem och avbildning
När du skapar en virtuell dator kan välja du en avbildning baserat på operativsystem som du vill toorun hello. Azure och dess samarbetspartner erbjuder många avbildningar, varav några innehåller förinstallerade program och verktyg. Eller ladda upp en av dina egna avbildningar (se [hello följande avsnitt](#use-your-own-image)).

### <a name="azure-images"></a>Azure-avbildningar
Använd hello [az vm-avbildning](/cli/azure/vm/image) kommandon toosee vad som är tillgängligt genom utgivare, distro versionen och versioner.

Lista tillgängliga utgivare:

```azurecli
az vm image list-publishers --location eastus
```

Lista tillgängliga produkter (erbjudanden) för en viss utgivare:

```azurecli
az vm image list-offers --publisher Canonical --location eastus
```

Lista tillgängliga SKU:er (distributionsutgåvor) för ett givet erbjudande:

```azurecli
az vm image list-skus --publisher Canonical --offer UbuntuServer --location eastus
```

Lista alla tillgängliga bilder för en viss version:

```azurecli
az vm image list --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS --location eastus
```

Fler exempel på Bläddra och använder tillgängliga avbildningar finns [analysera och välja avbildningar för virtuella Azure-datorn med hello Azure CLI](cli-ps-findimage.md).

Hej [az vm skapa](/cli/azure/vm#create) kommandot har alias som du kan använda tooquickly access hello vanliga distributioner och deras senaste versioner. Alias är ofta snabbare än att ange hello utgivare, erbjudande, SKU och version varje gång du skapar en virtuell dator:

| Alias | Utgivare | Erbjudande | SKU | Version |
|:--- |:--- |:--- |:--- |:--- |
| CentOS |OpenLogic |Centos |7.2 |senaste |
| CoreOS |CoreOS |CoreOS |Stable |senaste |
| Debian |credativ |Debian |8 |senaste |
| openSUSE |SUSE |openSUSE |13.2 |senaste |
| RHEL |Redhat |RHEL |7.2 |senaste |
| SLES |SLES |SLES |12-SP1 |senaste |
| UbuntuLTS |Canonical |UbuntuServer |14.04.4-LTS |senaste |

### <a name="use-your-own-image"></a>Använda en egen avbildning
Om du behöver specifika anpassningar kan du använda en avbildning baserad på en befintlig virtuell Azure-dator genom att avbilda den virtuella datorn. Du kan också ladda upp en avbildning som skapats lokalt. Mer information om distributioner som stöds och hur toouse egna avbildningar finns hello följande artiklar:

* [Azure-godkända distributioner](endorsed-distros.md)
* [Information om icke-godkända distributioner](create-upload-generic.md)
* [Hur toocreate en bild från en befintlig Azure-VM](tutorial-custom-images.md).
  
  * Snabbkurs exempel kommandon toocreate en bild från en befintlig virtuell Azure-dator:
    
    ```azurecli
    az vm deallocate --resource-group myResourceGroup --name myVM
    az vm generalize --resource-group myResourceGroup --name myVM
    az vm image create --resource-group myResourceGroup --source myVM --name myImage
    ```

## <a name="next-steps"></a>Nästa steg
* Skapa en Linux VM med hello [CLI](quick-create-cli.md), från hello [portal](quick-create-portal.md), eller med hjälp av en [Azure Resource Manager-mall](../windows/cli-deploy-templates.md).
* När du har skapat en Linux VM ska du [lära dig mer om Azure-diskar och lagring](tutorial-manage-disks.md).
* Snabba steg för[återställa ett lösenord eller SSH-nycklar och hantera användare](using-vmaccess-extension.md).
