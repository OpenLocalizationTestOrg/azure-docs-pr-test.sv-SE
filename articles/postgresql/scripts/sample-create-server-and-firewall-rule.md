---
title: "aaa ”Azure CLI Script - skapa en Azure-databas för PostgreSQL | Microsoft Docs ”"
description: "Azure CLI skriptexempel - skapar en Azure-databas för PostgreSQL-server och konfigurerar en brandväggsregel på servernivå."
services: postgresql
author: salonisonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: sample
ms.date: 05/31/2017
ms.openlocfilehash: bbe31f77283aa4a3bb36d1fd9171c280594a8267
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-server-and-configure-a-firewall-rule-using-hello-azure-cli"></a>Skapa en Azure-databas för PostgreSQL-servern och konfigurera en brandväggsregel som använder hello Azure CLI
Det här exempelskriptet CLI skapar en Azure-databas för PostgreSQL-server och konfigurerar en brandväggsregel på servernivå. När hello skriptet har körts utan problem, hello PostgreSQL-servern kan nås från alla Azure-tjänster och hello konfigurerade IP-adress.

[!INCLUDE [cloud-shell-try-it](../../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="sample-script"></a>Exempelskript
I det här exempelskriptet redigera hello markerade rader toocustomize hello admin användarnamn och lösenord.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/create-postgresql-server-and-firewall-rule.sh?highlight=15-16 "Create an Azure Database for PostgreSQL, and server-level firewall rule.")]

## <a name="clean-up-deployment"></a>Rensa distribution
Efter hello skriptexempel har körts, kan det vara hello följande kommando används tooremove hello resursgruppen och alla resurser som är kopplade till den.
[!code-azurecli-interactive[main](../../../cli_scripts/postgresql/create-postgresql-server-and-firewall-rule/delete-postgresql.sh "Delete hello resource group.")]

## <a name="script-explanation"></a>Skriptet förklaring
Det här skriptet använder hello följande kommandon. Varje kommando i hello tabellen länkar toocommand viss dokumentation.

| **Kommandot** | **Anteckningar** |
|---|---|
| [Skapa AZ grupp](/cli/azure/group#create) | Skapar en resursgrupp som är lagrade i alla resurser. |
| [Skapa AZ postgres server](/cli/azure/postgres/server#create) | Skapar en PostgreSQL-server som är värd för hello databaser. |
| [Skapa AZ postgres serverbrandvägg](/cli/azure/postgres/server/firewall-rule#create) | Skapar en regel tooallow åtkomst toohello brandväggsserver och databaserna under den från hello angivna IP-adressintervall. |
| [ta bort grupp AZ](/cli/azure/group#delete) | Tar bort en resursgrupp, inklusive alla kapslade resurser. |

## <a name="next-steps"></a>Nästa steg
- Läs mer om hello Azure CLI: [Azure CLI-dokumentation](/cli/azure/overview)
- Försök ytterligare skript: [Azure CLI-exempel för Azure-databas för PostgreSQL](../sample-scripts-azure-cli.md)
