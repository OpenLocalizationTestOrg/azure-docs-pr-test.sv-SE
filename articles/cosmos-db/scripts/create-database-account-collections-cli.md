---
title: aaaAzure CLI skript-skapa en Azure Cosmos DB DocumentDB API-kontot, databas och samling | Microsoft Docs
description: "Azure CLI-skript exempel – skapa ett Azure Cosmos DB DocumentDB API-kontot, databas och samling"
services: cosmos-db
documentationcenter: cosmosdb
author: mimig1
manager: jhubbard
editor: 
tags: azure-service-management
ms.assetid: 
ms.service: cosmos-db
ms.custom: mvc
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: cosmosdb
ms.workload: database
ms.date: 06/06/2017
ms.author: mimig
ms.openlocfilehash: 53919a849e04fa69680219e51c0289b9f2affe07
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-cosmos-db-create-an-documentdb-api-account-using-cli"></a>Azure Cosmos DB: Skapa ett DocumentDB-API-konto med hjälp av CLI

Det här exempelskriptet CLI skapar ett Azure Cosmos DB DocumentDB API-konto, databas och samling.  

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/cosmosdb/create-cosmosdb-account-database/create-cosmosdb-account-database.sh?highlight=15-35 "Create an Azure Cosmos DB DocumentDB API account, database, and collection")]

## <a name="clean-up-deployment"></a>Rensa distribution

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ cosmosdb](/cli/azure/cosmosdb#create) | Skapar ett Azure DB som Cosmos-konto. |
| [ta bort grupp AZ](/cli/azure/resource#delete) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare Azure Cosmos DB CLI skriptexempel finns i hello [Azure Cosmos DB CLI dokumentationen](../cli-samples.md).
