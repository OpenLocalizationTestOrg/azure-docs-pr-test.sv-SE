---
title: "Azure CLI-skript - ändra serverkonfigurationer | Microsoft Docs"
description: "Det här exempelskriptet CLI visar en lista över alla tillgängliga serverkonfigurationer och uppdaterar värdet för innodb_lock_wait_timeout."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: sample
ms.custom: mvc
ms.date: 11/03/2017
ms.openlocfilehash: 9a94f257e5cd3534127e8594ddee3c5f837876df
ms.sourcegitcommit: 5a6e943718a8d2bc5babea3cd624c0557ab67bd5
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/01/2017
---
# <a name="list-and-update-configurations-of-an-azure-database-for-mysql-server-using-azure-cli"></a>Visa och uppdatera konfigurationer av en Azure-databas för MySQL-server med Azure CLI
Det här exempelskriptet CLI listar alla tillgängliga konfigurationsparametrar samt de tillåtna värdena för Azure-databas för MySQL-server och anger den *innodb_lock_wait_timeout* till ett värde som är än standardvärdet.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare. Kör `az --version` för att hitta versionen. Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript
I det här exempelskriptet ändrar du markerade rader om du vill anpassa admin användarnamn och lösenord.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/change-server-configurations.sh?highlight=15-16 "List and update configurations of Azure Database for MySQL.")]

## <a name="clean-up-deployment"></a>Rensa distribution
Följande kommando kan användas för att ta bort resursgruppen och alla resurser som är associerade med den efter skriptexempel har körts.
[!code-azurecli-interactive[main](../../../cli_scripts/mysql/change-server-configurations/delete-mysql.sh  "Delete the resource group.")]

## <a name="script-explanation"></a>Skriptet förklaring
Det här skriptet använder följande kommandon. Varje kommando i tabellen länkar till kommandot viss dokumentation.

| **Kommandot** | **Anteckningar** |
|---|---|
| [Skapa AZ grupp](/cli/azure/group#az_group_create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ mysql-server](/cli/azure/mysql/server#az_msql_server_create) | Skapar en MySQL-server som är värd för databaserna. |
| [AZ mysql configuration serverlista](/cli/azure/mysql/server/configuration#az_msql_server_configuration_list) | Lista över konfigurationer av en Azure-databas för MySQL-servern. |
| [AZ mysql server configuration set](/cli/azure/mysql/server/configuration#az_msql_server_configuration_set) | Uppdatera konfigurationen av en Azure-databas för MySQL-servern. |
| [AZ mysql server configuration visa](/cli/azure/mysql/server/configuration#az_msql_server_configuration_show) | Visa konfigurationen av en Azure-databas för MySQL-servern. |
| [ta bort grupp AZ](/cli/azure/group#az_group_delete) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg
- Läs mer om Azure CLI: [Azure CLI dokumentationen](/cli/azure/overview).
- Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för MySQL](../sample-scripts-azure-cli.md)
- Mer information om serverparametrar finns [hur att konfigurera parametrar för Server i Azure-databas för MySQL](../howto-server-parameters.md).
