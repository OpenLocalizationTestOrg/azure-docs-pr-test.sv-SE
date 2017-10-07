---
title: "aaaConfigure och åtkomst serverloggen PostgreSQL med Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur tooconfigure och åtkomst hello loggas i Azure-databas för PostgreSQL med hjälp av Azure CLI-kommandoraden."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 924d4e7634cc48405bcb0f813cf61d8b72ad8aa0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-access-server-logs-using-azure-cli"></a>Konfigurera och komma åt server-loggar med hjälp av Azure CLI
Du kan hämta hello PostgreSQL server-felloggarna med hello kommandoradsgränssnittet (Azure CLI). Tootransaction åtkomstloggar stöds dock inte. 

## <a name="prerequisites"></a>Krav
toostep via den här hur tooguide behöver du:
- En [Azure-databas för PostgreSQL-server](quickstart-create-server-database-azure-cli.md)
- Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) kommandoradsverktyget verktyg eller Använd hello Azure Cloud Shell i hello webbläsare.

## <a name="configure-logging-for-azure-database-for-postgresql"></a>Konfigurera loggning för Azure-databas för PostgreSQL
Du kan konfigurera hello serverloggen tooaccess frågan och felloggarna. Felloggarna kan innehålla information om automatisk vakuum, anslutning och kontrollpunkter.
1. Aktivera loggning
2. Update-loggen\_-instruktionen och logga\_min\_varaktighet\_instruktionen tooenable loggning av frågor
3. Uppdatera kvarhållningsperioden

Mer information finns i [anpassa server konfigurationsparametrar](howto-configure-server-parameters-using-cli.md).

## <a name="list-logs-for-azure-database-for-postgresql-server"></a>Lista loggar för Azure-databas för PostgreSQL-server
loggfiler för toolist hello tillgänglig för din server kör hello [az postgres serverloggen listan](/cli/azure/postgres/server-logs#list) kommando.

Du kan ange hello loggfiler för server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup**, och ange tooa text fil med namnet **loggen\_filer\_list.txt.**
```azurecli-interactive
az postgres server-logs list --resource-group myresourcegroup --server mypgserver-20170401 > log_files_list.txt
```
## <a name="download-logs-locally-from-hello-server"></a>Hämta loggarna lokalt från hello-server
Hej [az postgres serverloggen hämta](/cli/azure/postgres/server-logs#download) med kommandot kan du toodownload individuella loggfiler för servern. 

Det här exemplet hämtningar hello specifika loggfilen för hello server **mypgserver 20170401.postgres.database.azure.com** under resursgrupp **myresourcegroup** tooyour lokal miljö.
```azurecli-interactive
az postgres server-logs download --name 20170414-mypgserver-20170401-postgresql.log --resource-group myresourcegroup --server mypgserver-20170401
```
## <a name="next-steps"></a>Nästa steg
- toolearn mer information om server-loggar finns [Server-loggar i Azure-databas för PostgreSQL](concepts-server-logs.md)
- Mer information om serverparametrar finns [anpassa konfigurationsparametrar för servern med hjälp av Azure CLI](howto-configure-server-parameters-using-cli.md)
