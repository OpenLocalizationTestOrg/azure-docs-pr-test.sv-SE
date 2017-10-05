---
title: Azure CLI Script exempel - kryptera Windows VM | Microsoft Docs
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
ms.openlocfilehash: 9574d779982c939aa970cd4bdf7df6e66793e3b9
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="encrypt-a-windows-virtual-machine-in-azure"></a>Kryptera en Windows-dator i Azure

Det här skriptet skapar en säker Azure Key Vault, krypteringsnycklar, Azure Active Directory-tjänstens huvudnamn och ett Windows-dator (VM). Den virtuella datorn krypteras sedan med hjälp av krypteringsnyckeln från Nyckelvalvet och tjänstens huvudnamn autentiseringsuppgifter.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/virtual-machine/encrypt-disks/encrypt_windows_vm.sh "kryptera Virtuella diskar")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando för att ta bort resursgruppen, virtuell dator och alla relaterade resurser.

```azurecli
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa en resursgrupp, Azure Key Vault, tjänstens huvudnamn, virtuell dator och alla relaterade resurser. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ keyvault](https://docs.microsoft.com/cli/azure/keyvault#create) | Skapar ett Azure Key Vault för lagring av säkra data, till exempel krypteringsnycklar. |
| [Skapa AZ keyvault nyckel](https://docs.microsoft.com/cli/azure/keyvault/key#create) | Skapar en krypteringsnyckel i Nyckelvalvet. |
| [AZ ad sp skapa-för-rbac](https://docs.microsoft.com/cli/azure/ad/sp#create-for-rbac) | Skapar ett Azure Active Directory-tjänstens huvudnamn för att autentisera och kontrollera åtkomst till krypteringsnycklarna på ett säkert sätt. |
| [Ange en AZ keyvault-princip](https://docs.microsoft.com/cli/azure/keyvault#set-policy) | Anger behörigheter på Nyckelvalvet att ge service principal åtkomst till krypteringsnycklarna. |
| [Skapa AZ vm](https://docs.microsoft.com/cli/azure/vm#create) | Skapar den virtuella datorn och ansluter till nätverkskort, virtuella nätverk, undernät och NSG. Det här kommandot anger också avbildning av virtuell dator som ska användas och administrativa autentiseringsuppgifter.  |
| [AZ vm kryptering aktivera](https://docs.microsoft.com/cli/azure/vm/encryption#enable) | Aktiverar kryptering på en virtuell dator med hjälp av service principal autentiseringsuppgifter och krypteringsnyckeln. |
| [AZ vm kryptering visa](https://docs.microsoft.com/cli/azure/vm/encryption#show) | Visar status för krypteringsprocessen VM. |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare virtuella CLI skriptexempel finns i den [Virtuella för Windows Azure-dokumentationen](../windows/cli-samples.md?toc=%2fazure%2fvirtual-machines%windows%2ftoc.json).
