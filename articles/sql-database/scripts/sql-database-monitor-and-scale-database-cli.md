---
title: "CLI exempel-övervakaren-skala-enda Azure SQL database | Microsoft Docs"
description: "Azure CLI-exempelskript för att övervaka och skala en enskild Azure SQL-databas"
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
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 01911b85268244a8fddb32aa726f8a870abbaf77
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="use-cli-to-monitor-and-scale-a-single-sql-database"></a>Använd CLI för att övervaka och skala en enskild SQL-databas

Detta exempel på Azure CLI-skript skalar en enskild Azure SQL-databas till en annan prestandanivå efter hämtar Storleksinformation i databasen. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[huvudsakliga](../../../cli_scripts/sql-database/monitor-and-scale-database/monitor-and-scale-database.sh "Övervakare och skala en SQL-databas")]

## <a name="clean-up-deployment"></a>Rensa distribution

Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>Skriptet förklaring

Det här skriptet använder följande kommandon. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| Kommando | Anteckningar |
|---|---|
| [Skapa AZ grupp](https://docs.microsoft.com/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ SQLServer](https://docs.microsoft.com/cli/azure/sql/server#create) | Skapar en logisk server som värd för en databas. |
| [AZ sql db visa-användning](https://docs.microsoft.com/cli/azure/sql/db#show-usage) | Visar användningsinformation storlek för en databas. |
| [AZ sql db-uppdateringen](https://docs.microsoft.com/cli/azure/sql/db#update) | Uppdaterar Databasegenskaper (till exempel nivå eller prestanda servicenivån) eller flyttar en databas i, slut på eller mellan elastiska pooler. |
| [ta bort grupp AZ](https://docs.microsoft.com/cli/azure/vm/extension#set) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |
|||

## <a name="next-steps"></a>Nästa steg

Mer information om Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare SQL-databas CLI skriptexempel finns i den [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).
