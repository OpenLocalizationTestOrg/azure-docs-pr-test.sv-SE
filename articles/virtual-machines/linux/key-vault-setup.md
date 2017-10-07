---
title: "aaaSet in Azure Key Vault för virtuella Linux-datorer | Microsoft Docs"
description: "Hur hello tooset in Key Vault för användning med en virtuell dator i Azure Resource Manager med CLI 2.0."
services: virtual-machines-linux
documentationcenter: 
author: singhkays
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: bccdd5ab-5ccf-4760-9039-92c6eafb15bd
ms.service: virtual-machines-linux
ms.devlang: azurecli
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.topic: article
ms.date: 02/24/2017
ms.author: singhkay
ms.openlocfilehash: a5dc1fbe59a71b4456ba5b9bbacdb90440064757
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-tooset-up-key-vault-for-virtual-machines-with-hello-azure-cli-20"></a>Hur tooset in Key Vault för virtuella datorer med hello Azure CLI 2.0

I hello Azure Resource Manager-stacken modelleras hemligheter/certifikat som resurser som tillhandahålls av Key Vault. toolearn mer om Azure Key Vault finns [vad är Azure Key Vault?](../../key-vault/key-vault-whatis.md) För Key Vault toobe används med Azure Resource Manager VMs hello *EnabledForDeployment* egenskapen i Nyckelvalvet måste anges tootrue. Den här artikeln lär du dig hur tooset in Key Vault för användning med virtuella Azure-datorer (VM) med hjälp av hello Azure CLI 2.0. Du kan också utföra dessa steg med hello [Azure CLI 1.0](key-vault-setup-cli-nodejs.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json).

tooperform dessa steg, behöver du hello senaste [Azure CLI 2.0](/cli/azure/install-az-cli2) installerad och logga in tooan Azure-konto med [az inloggningen](/cli/azure/#login).

## <a name="create-a-key-vault"></a>Skapa en Key Vault-lösning
Skapa en nyckelvalvet och tilldela hello princip för programdistribution med [az keyvault skapa](/cli/azure/keyvault#create). hello följande exempel skapas ett nyckelvalv med namnet `myKeyVault` i hello `myResourceGroup` resursgrupp:

```azurecli
az keyvault create -l westus -n myKeyVault -g myResourceGroup --enabled-for-deployment true
```

## <a name="update-a-key-vault-for-use-with-vms"></a>Uppdatera ett Nyckelvalv för användning med virtuella datorer
Ange princip för hello-distribution på en befintlig nyckelvalv med [az keyvault uppdatering](/cli/azure/keyvault#update). hello följande uppdateringar hello nyckelvalv med namnet `myKeyVault` i hello `myResourceGroup` resursgrupp:

```azurecli
az keyvault update -n myKeyVault -g myResourceGroup --set properties.enabledForDeployment=true
```

## <a name="use-templates-tooset-up-key-vault"></a>Använda mallar tooset in Key Vault
När du använder en mall måste tooset hello `enabledForDeployment` egenskapen för`true` för hello Key Vault-resursen på följande sätt:

```json
{
    "type": "Microsoft.KeyVault/vaults",
    "name": "ContosoKeyVault",
    "apiVersion": "2015-06-01",
    "location": "<location-of-key-vault>",
    "properties": {
    "enabledForDeployment": "true",
    ....
    ....
    }
}
```

## <a name="next-steps"></a>Nästa steg
Andra alternativ som du kan konfigurera när du skapar ett Nyckelvalv med hjälp av mallar, se [skapa ett nyckelvalv](https://azure.microsoft.com/documentation/templates/101-key-vault-create/).
