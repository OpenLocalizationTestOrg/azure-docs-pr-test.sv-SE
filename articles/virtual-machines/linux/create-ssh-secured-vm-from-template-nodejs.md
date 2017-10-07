---
title: "aaaCreate en Linux VM som använder en Azure-mall med Azure CLI 1.0 | Microsoft Docs"
description: "Skapa en Linux-VM på Azure med hjälp av en Azure Resource Manager-mall och hello Azure CLI 1.0."
services: virtual-machines-linux
documentationcenter: 
author: vlivech
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 05/12/2017
ms.author: v-livech
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: b694cc8247a8431b7ef4b24cc7dc2b4cdb9660ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toocreate-a-linux-vm-using-hello-azure-cli-10-an-azure-resource-manager-template"></a>Hur toocreate en Linux VM som använder hello Azure CLI 1.0 en Azure Resource Manager-mall
Den här artikeln visar hur tooquickly distribuera en Linux-dator med hjälp av hello Azure CLI 1.0 och en Azure Resource Manager-mall. hello artikel kräver:

* ett Azure-konto ([hämta en kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/)).
* Hej [Azure CLI 1.0](../../cli-install-nodejs.md) loggat in med `azure login`.
* hello Azure CLI *måste vara i* läget Azure Resource Manager `azure config mode arm`.

Du kan också snabbt distribuera en Linux VM-mall med hjälp av hello [Azure-portalen](quick-create-portal.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

## <a name="cli-versions-toocomplete-hello-task"></a>CLI versioner toocomplete hello aktivitet
Du kan göra hello med hjälp av något av följande versioner av CLI hello:

- [Azure CLI 1.0](#quick-command-summary) – våra CLI för hello klassisk och resurs management distributionsmodeller (den här artikeln)
- [Azure CLI 2.0](create-ssh-secured-vm-from-template.md) -vår nästa generations CLI för hello resursdistributionsmodell för hantering

## <a name="quick-command-summary"></a>Snabb kommandosammanfattning
```azurecli
azure group create \
    -n myResourceGroup \
    -l westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

## <a name="detailed-walkthrough"></a>Detaljerad genomgång
Mallar kan du toocreate virtuella datorer i Azure med inställningar som du vill toocustomize under hello starten, inställningar för användarnamn och värdnamn. För den här artikeln startar vi en Azure-mall med hjälp av en virtuell Ubuntu-dator tillsammans med en nätverkssäkerhetsgrupp med port 22 öppen för SSH.

Azure Resource Manager-mallar är JSON-filer som kan användas för enkla engångsuppgifter som generering av en virtuell Ubuntu-dator, så som det görs i den här artikeln.  Azure-mallar kan också vara används tooconstruct komplexa Azure-konfigurationer i hela miljöer som en grupp för testning, utveckling eller produktion distribution.

## <a name="create-hello-linux-vm"></a>Skapa hello Linux VM
Hej följande exempel visas hur toocall `azure group create` toocreate en resurs gruppen och distribuera en SSH-skyddad Linux VM på hello samma gång med hjälp av [Azure Resource Manager-mallen](https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json). Tänk på att i ditt exempel måste toouse namn som är unikt tooyour miljö. Det här exemplet används *myResourceGroup* som hello resursgruppens namn, och *myVM* som hello VM-namn.

```azurecli
azure group create \
    --name myResourceGroup \
    --location westus \
    --template-uri https://raw.githubusercontent.com/azure/azure-quickstart-templates/master/101-vm-sshkey/azuredeploy.json
```

hello utdata bör se ut som följande utdatablock hello:

```azurecli
info:    Executing command group create
+ Getting resource group myResourceGroup
+ Creating resource group myResourceGroup
info:    Created resource group myResourceGroup
info:    Supply values for hello following parameters
sshKeyData: ssh-rsa AAAAB3Nza<..ssh public key text..>VQgwjNjQ== myAdminUser@myVM
+ Initializing template configurations and parameters
+ Creating a deployment
info:    Created template deployment "azuredeploy"
data:    Id:                  /subscriptions/<..subid text..>/resourceGroups/myResourceGroup
data:    Name:                myResourceGroup
data:    Location:            westus
data:    Provisioning State:  Succeeded
data:    Tags: null
data:
info:    group create command OK
```

Som exempel distribuera en virtuell dator med hjälp av hello `--template-uri` parameter.  Du kan också hämta eller skapa en mall lokalt och skicka hello mallen med hjälp av hello `--template-file` parameter med en sökväg toohello mallfilen som argument. hello Azure CLI uppmanar dig att hello-parametrar som krävs av hello mallen.

## <a name="next-steps"></a>Nästa steg
Sök hello [mallgalleriet](https://azure.microsoft.com/documentation/templates/) toodiscover vilka appen ramverk toodeploy nästa.

