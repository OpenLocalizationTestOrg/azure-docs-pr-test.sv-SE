---
title: "Skriptexempel Azure CLI - rotera åtkomst för lagringskontonycklar | Microsoft Docs"
description: "Skapa ett Azure Storage-konto och sedan hämta och rotera åtkomstnycklar sitt konto."
services: storage
documentationcenter: na
author: tamram
manager: timlt
editor: tysonn
ms.assetid: 
ms.custom: mvc
ms.service: storage
ms.workload: storage
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: sample
ms.date: 06/22/2017
ms.author: tamram
ms.openlocfilehash: c8c99dd8663eab5b5c1c47d34f110f9dceaace6b
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/01/2017
---
# <a name="create-a-storage-account-and-rotate-its-account-access-keys"></a>Skapa ett lagringskonto och rotera åtkomstnycklar sitt konto

Det här skriptet skapar ett Azure Storage-konto, visar åtkomstnycklarna för det nya lagringskontot och sedan förnyar (roterar) nycklarna.

[!INCLUDE [sample-cli-install](../../../includes/sample-cli-install.md)]

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/storage/rotate-storage-account-keys/rotate-storage-account-keys.sh "Rotate storage account keys")]

## <a name="clean-up-deployment"></a>Rensa distribution 

Kör följande kommando för att ta bort resursgruppen, storage-konto och alla relaterade resurser.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa storage-konto och hämta och rotera dess åtkomstnycklar. Varje objekt i tabellen länkar till kommando-specifik dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ storage-konto](/cli/azure/storage/account#create) | Skapar ett Azure Storage-konto i den angivna resursgruppen. |
| [AZ nycklar lagringskontolistan](/cli/azure/storage/account/keys#list) | Visar åtkomstnycklar för lagring konto för det angivna kontot. |
| [förnya AZ lagringskontonycklar](/cli/azure/storage/account/keys#renew) | Återskapar åtkomstnyckeln för kontot primär eller sekundär lagring. |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](/cli/azure/overview).

Ytterligare lagringsutrymme CLI skriptexempel finns i den [Azure CLI prover för Azure Blob storage](../blobs/storage-samples-blobs-cli.md).
