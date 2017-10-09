---
title: "aaaAzure CLI exempel tooscale en Azure-databas för MySQL-server | Microsoft Docs"
description: "Det här exempelskriptet CLI skalas Azure-databas för MySQL servernivå tooa olika prestanda när du frågar hello mått."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 05/31/2017
ms.openlocfilehash: 721ef9db35a5f3be7a38438c1abb724187b18b75
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-scale-an-azure-database-for-mysql-server-using-azure-cli"></a>Övervaka och skala en Azure-databas för MySQL-server med Azure CLI
Det här exempelskriptet CLI skalar en Azure-databas för MySQL servernivå tooa olika prestanda när du frågar hello mått.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript
I det här exempelskriptet ändra hello markerade rader toocustomize hello admin användarnamn och lösenord. Ersätt hello prenumerations-id som används i hello az övervakaren kommandon med din egen prenumerations-id.[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/scale-mysql-server.sh?highlight=15-16 "Create and scale Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Rensa distribution
Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/scale-mysql-server/delete-mysql.sh  "Delete hello resource group.")]

## <a name="script-explanation"></a>Skriptet förklaring
Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| **Kommandot** | **Anteckningar** |
|---|---|
| [Skapa AZ grupp](/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ mysql-server](/cli/azure/mysql/server#create) | Skapar en MySQL-server som är värd för hello databaser. |
| [AZ övervakaren mått lista](/cli/azure/monitor/metrics#list) | Lista över hello måttet för hello resurser. |
| [ta bort grupp AZ](/cli/azure/group#delete) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg
- Läs mer om hello Azure CLI: [Azure CLI dokumentationen](/cli/azure/overview).
- Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för MySQL](../sample-scripts-azure-cli.md)
- Läs mer om att skala [tjänstnivåer](../concepts-service-tiers.md) och [Compute enheter och lagringsenheter](../concepts-compute-unit-and-storage.md).
