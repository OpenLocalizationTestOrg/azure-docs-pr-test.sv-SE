---
title: aaaAzure CLI skriptexempel - kryptera en virtuell Windows-dator | Microsoft Docs
description: Azure CLI Script exempel - kryptera Windows VM
services: virtual-machines-windows
documentationcenter: virtual-machines
author: iainfoulds
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 06/02/2017
ms.author: iainfou
ms.openlocfilehash: 7a9928467f818cd5358d3d19bff5129d8fa21313
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Kryptera en Windows-dator i Azure

Det här skriptet skapar en säker Azure Key Vault, krypteringsnycklar, Azure Active Directory-tjänstens huvudnamn och ett Windows-dator (VM). hello VM krypteras sedan med hello krypteringsnyckeln från Nyckelvalvet och tjänstens huvudnamn autentiseringsuppgifter.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "Encrypt VM disks")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando tooremove hello resursgrupp, VM och alla relaterade resurser hello.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hello en resurs grupp, Azure Key Vault service principal, virtuell dator, och alla relaterade resurser. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ keyvault](https://docs.microsoft.com/cli/azure/keyvault#create) | Skapar ett Azure Key Vault toostore säkra data, till exempel krypteringsnycklar. |
| [Skapa AZ keyvault nyckel](https://docs.microsoft.com/cli/azure/keyvault/key#create) | Skapar en krypteringsnyckel i Nyckelvalvet. |
| [AZ ad sp skapa-för-rbac](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | Skapar ett Azure Active Directory service principal toosecurely autentisera och styra tooencryption åtkomstnycklar. |
| [Ange en AZ keyvault-princip](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Anger behörigheter på hello Key Vault toogrant hello service principal tooencryption åtkomstnycklar. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm#create) | Skapar hello virtuella datorn och ansluter den toohello nätverkskort, virtuella nätverk, undernät och NSG. Det här kommandot anger också hello virtuella avbildningen toobe används och administrativa autentiseringsuppgifter.  |
| [AZ vm kryptering aktivera](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | Aktiverar kryptering på en virtuell dator med hjälp av hello service principal autentiseringsuppgifter och krypteringsnyckeln. |
| [AZ vm kryptering visa](https://docs.microsoft.com/cli/azure/vm/encryption#show) | Visar hello status hello VM krypteringsprocessen. |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i hello [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
