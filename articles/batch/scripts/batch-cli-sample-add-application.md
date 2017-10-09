---
title: "aaaAzure CLI skriptexempel - lägga till ett program i Batch | Microsoft Docs"
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
ms.openlocfilehash: cb33b3a7b30610011b19954a987995cc5f0257c4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="adding-applications-tooazure-batch-with-azure-cli"></a>Lägga till program tooAzure Batch med Azure CLI

Det här skriptet visar hur tooset skapat ett program för användning med en Azure Batch-pool eller en uppgift. tooset upp ett program, paketera din körbar fil, tillsammans med eventuella beroenden till en ZIP-fil. I det här exemplet hello körbara zip-fil som kallas ”min-program-exe.zip'.

## <a name="prerequisites"></a>Krav

- Installera hello Azure CLI med hjälp av anvisningarna i hello hello [Azure CLI installationsguiden](https://docs.microsoft.com/cli/azure/install-azure-cli), om du inte redan har gjort.
- Skapa ett Batch-konto om du inte redan har ett. Se [skapa ett batchkonto med hello Azure CLI](https://docs.microsoft.com/azure/batch/scripts/batch-cli-sample-create-account) för ett exempelskript som skapar ett konto.

## <a name="sample-script"></a>Exempelskript

[!code-azurecli[main](../../../cli_scripts/batch/add-application/add-application.sh "Add Application")]

## <a name="clean-up-application"></a>Rensa program

När du har kört hello ovan exempelskript kör du följande kommandon tooremove hello programmet och alla dess överförda programpaket.

```azurecli
az batch application package delete -g myresourcegroup -n mybatchaccount --application-id myapp --version 1.0 --yes
az batch application delete -g myresourcegroup -n mybatchaccount --application-id myapp --yes
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon toocreate hello ett program och ladda upp ett programpaket.
Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ batch-program](https://docs.microsoft.com/cli/azure/batch/application#create) | Skapar ett program.  |
| [AZ batch programmet uppsättningen](https://docs.microsoft.com/cli/azure/batch/application#set) | Uppdaterar egenskaperna för ett program.  |
| [Skapa AZ batch-programpaket](https://docs.microsoft.com/cli/azure/batch/application/package#create) | Lägger till ett program paketet toohello angivna program.  |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Batch CLI skriptexempel finns i hello [Azure Batch CLI dokumentationen](../batch-cli-samples.md).
