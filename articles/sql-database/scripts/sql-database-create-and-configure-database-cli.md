---
title: aaaCLI exempel-skapa en Azure SQL database | Microsoft Docs
description: Azure CLI exempel skriptet toocreate en SQL-databas
services: sql-database
documentationcenter: sql-database
author: janeng
manager: jstrauss
editor: carlrab
tags: azure-service-management
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 06/23/2017
ms.author: janeng
ms.openlocfilehash: 0d54e284e19f16387813e24d7beb7ab048a39263
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-cli-toocreate-a-single-azure-sql-database-and-configure-a-firewall-rule"></a>Använd CLI toocreate en Azure SQL-databas och konfigurera en brandväggsregel

Detta exempel på Azure CLI-skript skapar en Azure SQL database och konfigurera en brandväggsregel på servernivå. När hello skriptet har körts utan problem, konfigurerade hello SQL-databas kan nås från alla Azure-tjänster och hello IP-adress. 

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript

[!code-azurecli-interactive[main](../../../cli_scripts/sql-database/create-and-configure-database/create-and-configure-database.sh?highlight=9-10 "Create SQL Database")]

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
| [Skapa AZ SQLServer](/cli/azure/sql/server#create) | Skapar en logisk server att värdar hello SQL-databas. |
| [Skapa AZ sql server-brandvägg](/cli/azure/sql/server/firewall-rule#create) | Skapar en brandvägg regeln tooallow åtkomst tooall SQL-databaser på hello-servern från hello angivna IP-adressintervall. |
| [Skapa AZ sql-databas](/cli/azure/sql/db#create) | Skapar hello SQL-databas i hello logiska server. |
| [ta bort grupp AZ](/cli/azure/resource#delete) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg

Mer information om hello Azure CLI finns [Azure CLI dokumentationen](https://docs.microsoft.com/cli/azure/overview).

Ytterligare SQL-databas CLI skriptexempel finns i hello [dokumentation för Azure SQL Database](../sql-database-cli-samples.md).

