---
title: aaaCLI exempel flytta Azure SQL database-elastisk SQL-pool | Microsoft Docs
description: Azure CLI exempel skriptet toomove en SQL-databas i en elastisk SQL-pool
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: monitor & tune
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/05/2017
ms.author: janeng
ms.openlocfilehash: 841eb57d2d49612c3fadd3a6424a2b0309c69719
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toomove-an-azure-sql-database-in-a-sql-elastic-pool"></a>Använd CLI toomove Azure SQL-databas i en elastisk SQL-pool

Detta exempel på Azure CLI-skript skapar två elastiska pooler flyttar en Azure SQL-databas från en elastiska SQL-poolen till en annan elastisk SQL-pool och sedan flyttas hello databasen utanför elastisk pool tooa enda Azure databasens prestandanivå. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/move-database-between-pools/move-database-between-pools.sh "Move database between pools")]

## <a name="clean-up-deployment"></a>Rensa distribution

Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ SQLServer](https://docs.microsoft.com/cli/azure/sql/server#create) | Skapar en logisk server som är värd för en databas eller elastisk pool. |
| [Skapa AZ sql elastisk-pooler](https://docs.microsoft.com/cli/azure/sql/elastic-pool#create) | Skapar en elastisk pool i hello logiska server. |
| [Skapa AZ sql-databas](https://docs.microsoft.com/cli/azure/sql/db#create) | Skapar en databas i en logisk server som en enda eller en delad databas. |
| [AZ sql db-uppdateringen](https://docs.microsoft.com/cli/azure/sql/db#update) | Uppdaterar Databasegenskaper eller flyttar en databas i, slut på eller mellan elastiska pooler. |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare SQL-databas CLI skriptexempel finns i hello [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).


