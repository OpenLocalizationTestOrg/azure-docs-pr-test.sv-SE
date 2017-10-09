---
title: "aaaCreate en Linux VM i Azure från en mall | Microsoft Docs"
description: "Hur hello toouse Azure CLI 2.0 toocreate en Linux VM från en Resource Manager-mall"
services: virtual-machines-linux
documentationcenter: 
author: iainfoulds
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 721b8378-9e47-411e-842c-ec3276d3256a
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: azurecli
ms.topic: article
ms.date: 05/12/2017
ms.author: iainfou
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: f4b8c4103cccbae13c679ff2a2cac928a0e8e809
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-virtual-machine-with-azure-resource-manager-templates"></a>Hur toocreate en Linux-dator med Azure Resource Manager-mallar
Den här artikeln visar hur tooquickly distribuera en Linux-dator (VM) med Azure Resource Manager-mallar och hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](create-ssh-secured-vm-from-template-nodejs.md).


## <a name="templates-overview"></a>Översikt över mallar
Azure Resource Manager-mallarna är JSON-filer som definierar hello-infrastrukturen och konfigurationen av din Azure-lösning. Genom att använda en mall kan du distribuera lösningen flera gånger under dess livscykel och vara säker på att dina resurser distribueras konsekvent. toolearn mer om hello format hello mall och hur du skapar det, se [skapa din första Azure Resource Manager-mallen](../../azure-resource-manager/resource-manager-create-first-template.md). tooview hello JSON-syntax för typer av resurser, se [definiera resurser i Azure Resource Manager-mallar](/azure/templates/).


## <a name="create-resource-group"></a>Skapa resursgrupp
En Azure-resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras. En resursgrupp måste skapas innan en virtuell dator. hello följande exempel skapar en resursgrupp med namnet *myResourceGroupVM* i hello *eastus* region:

```azurecli
az group create --name myResourceGroup --location eastus
```

## <a name="create-virtual-machine"></a>Skapa en virtuell dator
hello följande exempel skapas en virtuell dator från [Azure Resource Manager-mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json) med [az distribution skapa](/cli/azure/group/deployment#create). Ange hello värde egna SSH offentlig nyckel, till exempel hello innehållet i *~/.ssh/id_rsa.pub*. Om du behöver toocreate ett SSH-nyckelpar finns [hur toocreate och använder en SSH nyckelpar för Linux virtuella datorer i Azure](mac-create-ssh-keys.md).

```azurecli
az group deployment create --resource-group myResourceGroup \
  --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json \
  --parameters '{"sshKeyData": {"value": "ssh-rsa AAAAB3N{snip}B9eIgoZ"}}'
```

I det här exemplet anges en mall som lagras i GitHub. Du kan också hämta eller skapa en mall och ange hello lokal sökväg med hello samma `--template-file` parameter.

tooSSH tooyour VM, hämta hello offentliga IP-adress med [az nätverket offentliga ip-visa](/cli/azure/network/public-ip#show):

```azurecli
az network public-ip show \
    --resource-group myResourceGroup \
    --name sshPublicIP \
    --query [ipAddress] \
    --output tsv
```

Du kan sedan SSH tooyour VM som vanligt:

```bash
ssh azureuser@<ipAddress>
```

## <a name="next-steps"></a>Nästa steg
I det här exemplet skapas en grundläggande Linux VM. För mer Resource Manager-mallar som inkluderar ramverk för programmet eller skapa mer komplexa miljöer, bläddra hello [Azure quickstart mallgalleriet](https://azure.microsoft.com/documentation/templates/).
