---
title: "aaaConfigure hello tjänstparametrar i Azure-databas för PostgreSQL | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure hello tjänstparametrar i Azure-databas för att använda PostgreSQL hello Azure CLI-kommandoraden."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 84a11de24ba87fc0eb6744aaa4b53f65a183903d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="customize-server-configuration-parameters-using-azure-cli"></a>Anpassa konfigurationsparametrar för servern med hjälp av Azure CLI
Du kan visa, visa och uppdatera konfigurationsparametrar för ett Azure PostgreSQL-server med hello kommandoradsgränssnittet (Azure CLI). Endast en delmängd av motorkonfigurationer är tillgängliga på servernivå och kan ändras. 

## <a name="prerequisites"></a>Krav
toostep via den här hur tooguide behöver du:
- En server och databas [skapa en Azure-databas för PostgreSQL](quickstart-create-server-database-azure-cli.md)
- Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) kommandoraden verktyg eller Använd hello Azure Cloud Shell i hello webbläsare.

## <a name="list-server-configuration-parameters-for-azure-database-for-postgresql-server"></a>Lista server konfigurationsparametrar för Azure-databas för PostgreSQL-server
toolist alla ändringsbar parametrar i en server och deras värden kör hello [az postgres server konfigurationslistan](/cli/azure/postgres/server/configuration#list) kommando.

Du kan ange hello server konfigurationsparametrar för hello server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup**.
```azurecli-interactive
az postgres server configuration list --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="show-server-configuration-parameter-details"></a>Visa serverkonfiguration parameterinformation
tooshow information om en specifik konfigurationsparameter för en server som kör hello [az postgres server configuration visa](/cli/azure/postgres/server/configuration#show) kommando.

Det här exemplet visas information om hello **loggen\_min\_meddelanden** server konfigurationsparameter för server **mypgserver 20170401.postgres.database.azure.com** under resursgruppen **myresourcegroup.**
```azurecli-interactive
az postgres server configuration show --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="modify-server-configuration-parameter-value"></a>Ändra parametervärdet för server-konfiguration
Du kan också ändra hello-värdet för en viss server konfigurationsparameter och hello underliggande Konfigurationsvärdet för hello PostgreSQL server engine uppdateras. tooupdate hello configuration Använd hello [az postgres server configuration set](/cli/azure/postgres/server/configuration#set) kommando. 

tooupdate hello **loggen\_min\_meddelanden** server konfigurationsparameter Server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup.**
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401 --value INFO
```
Om du vill tooreset hello värdet för en konfigurationsparameter du bara välja tooleave ut hello valfria `--value` parameter och hello tjänsten gäller hello standardvärdet. I ovanstående exempel ser det ut som:
```azurecli-interactive
az postgres server configuration set --name log_min_messages --resource-group myresourcegroup --server mypgserver-20170401
```
Det här återställer hello **loggen\_min\_meddelanden** configuration toohello standardvärdet **varning**. Mer information om konfiguration och tillåtna värden finns i dokumentationen för PostgreSQL på [serverkonfiguration](https://www.postgresql.org/docs/9.6/static/runtime-config.html).

## <a name="next-steps"></a>Nästa steg
- tooconfigure och åtkomst server-loggar finns [Server-loggar i Azure-databas för PostgreSQL](concepts-server-logs.md)
