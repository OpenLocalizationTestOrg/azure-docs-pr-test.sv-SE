---
title: "Azure CLI skript exempel - lägga till ett program i Batch | Microsoft Docs"
description: "Azure CLI skript exempel - lägga till ett program i Batch"
services: batch
documentationcenter: 
author: annatisch
manager: daryls
editor: tysonn
ms.assetid: 
ms.service: batch
ms.devlang: azurecli
ms.topic: article
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 05/02/2017
ms.author: antisch
ms.openlocfilehash: 5d057eaf32867aedc95d58c5185e2be1f9385ec0
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="adding-applications-to-azure-batch-with-azure-cli"></a>Lägga till program till Azure Batch med Azure CLI

Det här skriptet visar hur du konfigurerar ett program för användning med en Azure Batch-pool eller en uppgift. Paketera din körbar fil, tillsammans med eventuella beroenden till en ZIP-fil om du vill konfigurera ett program. I det här exemplet kallas den körbara zip-filen ' min-program-exe.zip'.

## <a name="prerequisites"></a>Krav

- Installera Azure CLI med hjälp av instruktionerna i den [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.
- Skapa ett Batch-konto om du inte redan har ett. Se [skapa ett batchkonto med Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[huvudsakliga](../../../cli_scripts/batch/add-application/add-application.sh "Lägg till program")]

## <a name="clean-up-application"></a>Rensa program

När du har kört ovan exempelskript kör du följande kommandon för att ta bort programmet och alla dess överförda programpaket.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon för att skapa ett program och ladda upp ett programpaket.
Varje kommando i tabellen länkar till kommando-specifik dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ batch-program](https://docs.microsoft.com/cli/azure/batch/application#create) | Skapar ett program.  |
| [AZ batch programmet uppsättningen](https://docs.microsoft.com/cli/azure/batch/application#set) | Uppdaterar egenskaperna för ett program.  |
| [Skapa AZ batch-programpaket](https://docs.microsoft.com/cli/azure/batch/application/package#create) | Lägger till ett programpaket till det angivna programmet.  |

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Batch CLI skriptexempel finns i den [Azure Batch CLI dokumentationen](../batch-cli-samples.md).
