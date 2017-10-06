---
title: "Olika sätt att skapa en Linux-VM med Azure CLI 1.0 | Microsoft Docs"
description: "Lär dig mer om vilka alternativ som du kan välja mellan när du skapar en virtuell Linux-dator i Azure, inklusive länkar till verktyg och självstudier för varje metod."
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f38f8a44-6c88-4490-a84a-46388212d24c
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: 
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 05/11/2017
ms.author: iainfou
ms.openlocfilehash: 1eb90d44797d66f3e09811918ce5a7f4ad4287c6
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="different-ways-to-create-a-linux-virtual-machine-in-azure"></a>Olika sätt att skapa en virtuell Linux-dator i Azure
I Azure har du tillräcklig flexibilitet för att kunna skapa en virtuell Linux-dator (VM) med hjälp av de verktyg och arbetsflöden som du känner dig mest bekväm med. I den här artikeln sammanfattas skillnader och exempel när du ska skapa dina virtuella Linux-datorer.

## <a name="azure-cli"></a>Azure CLI
Du kan skapa virtuella datorer i Azure med någon av följande CLI-versioner:

- Azure CLI 1.0 – vårt CLI för den klassiska distributionsmodellen och Resource Manager-distributionsmodellen (den här artikeln)
- [Azure CLI 2.0](../windows/creation-choices.md) – vår nästa generations CLI för distributionsmodellen resurshantering

Azure CLI 1.0 är tillgängligt på flera plattformar via ett npm-paket, distro-paket eller Docker-behållare. Du kan läsa mer om [hur du installerar och konfigurerar Azure CLI](../../cli-install-nodejs.md). Följande självstudier innehåller exempel på hur du använder Azure CLI 1.0. Läs alla artiklarna för mer information om de CLI-snabbstartkommandon som visas:

* [Skapa en virtuell Linux-dator från Azure CLI för utveckling och testning](quick-create-cli-nodejs.md)
  
  * I följande exempel skapas en CoreOS VM som använder en offentlig nyckel som heter *azure_id_rsa.pub*:
    
    ```azurecli
    azure vm quick-create -ssh-publickey-file ~/.ssh/azure_id_rsa.pub \
      --image-urn CoreOS
    ```
* [Skapa en säker virtuell Linux-dator med hjälp av en Azure-mall](create-ssh-secured-vm-from-template.md)
  
  * Följande exempel skapar en VM med en mall som lagrats i GitHub:
    
    ```azurecli
    azure group create --name myResourceGroup --location eastus 
      --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
    ```
* [Skapa en fullständig Linux-miljö med Azure CLI](create-cli-complete-nodejs.md)
  
  * Innefattar att skapa en belastningsutjämnare och flera virtuella datorer i en tillgänglighetsuppsättning.
* [Lägg till en disk till en virtuell Linux-dator](add-disk.md)
  
  * I följande exempel läggs en *5* Gb disk till en befintlig virtuell dator med namnet *myVM*:
    
    ```azurecli
    azure vm disk attach-new \
        --resource-group myResourceGroup \
        --vm-name myVM \
        --size-in-GB 5
    ```

## <a name="azure-portal"></a>Azure Portal
I [Azure Portal](https://portal.azure.com) kan du snabbt skapa en virtuell dator eftersom det inte finns något att installera i ditt system. Använd Azure Portal när du skapar den virtuella datorn:

* [Skapa en virtuell Linux-dator med hjälp av Azure Portal](quick-create-portal.md) 

## <a name="operating-system-and-image-choices"></a>Alternativ för operativsystem och avbildning
När du skapar en virtuell dator, väljer du en avbildning baserat på vilket operativsystem du vill köra. Azure och dess samarbetspartner erbjuder många avbildningar, varav några innehåller förinstallerade program och verktyg. Eller ladda upp en av dina egna avbildningar (se [följande avsnitt](#use-your-own-image)).

### <a name="azure-images"></a>Azure-avbildningar
Använd `azure vm image` CLI-kommandon för att se vad som finns tillgängligt via utgivare, distributionsutgåva och version.

Lista tillgängliga utgivare enligt följande:

```azurecli
azure vm image list-publishers --location eastus
```

Lista tillgängliga produkter (erbjudanden) för en viss utgivare enligt följande:

```azurecli
azure vm image list-offers --location eastus --publisher Canonical
```

Lista tillgängliga SKU:er (distributionsutgåvor) för ett givet erbjudande enligt följande:

```azurecli
azure vm image list-skus --location eastus --publisher Canonical --offer UbuntuServer
```

Lista alla tillgängliga avbildningar för en viss version enligt följande:

```azurecli
azure vm image list --location eastus --publisher Canonical --offer UbuntuServer --sku 16.04.0-LTS
```

Kommandona `azure vm quick-create` och `azure vm create` har alias som du kan använda för att snabbt komma åt vanliga distributioner och deras senaste versioner. Det går snabbare att använda alias än ange utgivare, erbjudande, SKU och version varje gång du skapar en virtuell dator:

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
Om du behöver specifika anpassningar kan du använda en avbildning baserad på en befintlig virtuell Azure-dator genom att *avbilda* den virtuella datorn. Du kan också ladda upp en avbildning som skapats lokalt. Mer information om distributioner som stöds och hur du använder egna avbildningar finns i följande artiklar:

* [Azure-godkända distributioner](endorsed-distros.md)
* [Information om icke-godkända distributioner](create-upload-generic.md)
* [Överföra och skapa en Linux VM från anpassad diskavbildning](upload-vhd.md)
* [Avbilda en virtuell Linux-dator som en Resource Manager-mall](capture-image.md).
  
  * Snabbstart med exempelkommandon för att avbilda en befintlig virtuell dator:
    
    ```azurecli
    azure vm deallocate --resource-group myResourceGroup --vm-name myVM
    azure vm generalize --resource-group myResourceGroup --vm-name myVM
    azure vm capture --resource-group myResourceGroup --vm-name myVM --vhd-name-prefix myCapturedVM
    ```

## <a name="next-steps"></a>Nästa steg
* Skapa en virtuell Linux-dator från [portalen](quick-create-portal.md), med [CLI](quick-create-cli.md) eller genom att använda en [Azure Resource Manager-mall](../windows/cli-deploy-templates.md).
* När du har skapat en virtuell Linux-dator kan du [lägga till en datadisk](add-disk.md).
* Snabba steg för att [återställa ett lösenord eller SSH-nycklar och hantera användare](using-vmaccess-extension.md)

