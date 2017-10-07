---
title: 'Azure CLI: Skapa en SQL-databas | Microsoft Docs'
description: "Lär dig hur toocreate en logisk SQL Database-server, brandväggsregel på servernivå och databaser med hjälp av hello Azure CLI."
keywords: "sql database-självstudier, skapa en sql-databas"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: 9b1ffb17eabeb70a000ff0c997128832b07aa4fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-hello-azure-cli"></a>Skapa en enda Azure SQL-databas med hello Azure CLI

hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript. Detta hjälper information med hjälp av hello Azure CLI toodeploy en Azure SQL-databas i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i en [logisk Azure SQL Database-server](sql-database-features.md).

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt i det här avsnittet kräver att du kör hello Azure CLI version 2.0.4 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

## <a name="define-variables"></a>Definiera variabler

Definiera variabler för användning i hello skript i denna Snabbstart.

```azurecli-interactive
# hello data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = westeurope
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# hello ip address range that you want tooallow tooaccess your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# hello database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create) kommando. En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp. hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats.

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a>Skapa en logisk server

Skapa en [logisk Azure SQL Database-server](sql-database-features.md) med hello [az sql-servern skapa](/cli/azure/sql/server#create) kommando. En logisk server innehåller en uppsättning databaser som hanteras som en grupp. hello följande exempel skapas en slumpmässigt namn i resursgruppen med en administratörsinloggning med namnet `ServerAdmin` och lösenordet `ChangeYourAdminPassword1`. Ersätt dessa fördefinierade värden efter behov.

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurera en serverbrandväggsregel

Skapa en [Azure SQL Database brandväggsregel på servernivå](sql-database-firewall-configure.md) med hello [az sql server-brandvägg skapa](/cli/azure/sql/server/firewall-rule#create) kommando. En brandväggsregel på servernivå kan ett externt program, till exempel SQL Server Management Studio eller hello SQLCMD-verktyget tooconnect tooa SQL-databasen via hello SQL Database-tjänsten brandvägg. I följande exempel hello, öppnas hello brandväggen bara för andra Azure-resurser. tooenable extern anslutning, ändra hello IP-adress tooan korrekt adress för din miljö. tooopen alla IP-adresser, använda 0.0.0.0 som hello startar IP-adress och 255.255.255.255 som hello slutadress.  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> SQL Database kommunicerar via port 1433. Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg. I så fall, blir inte kan tooconnect tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a>Skapa en databas i hello server med exempeldata

Skapa en databas med en [prestandanivå S0](sql-database-service-tiers.md) i hello-servern med hjälp av hello [az sql db skapa](/cli/azure/sql/db#create) kommando. hello följande exempel skapar en databas som heter `mySampleDatabase` och belastningar hello AdventureWorksLT exempeldata till den här databasen. Ersätt de fördefinierade värden efter behov (andra snabb startar i den här samlingen build vid hello värdena i den här snabbstartsguide).

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a>Rensa resurser

Andra snabbstarter i den här samlingen bygger på den här snabbstarten. 

> [!TIP]
> Om du tänker toocontinue toowork med efterföljande snabbstarter, rensa hello resurser som skapas på den här quick starta inte. Om du inte planerar toocontinue använda hello efter steg toodelete alla resurser skapas av den här Snabbstart i hello Azure-portalen.
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a>Nästa steg

Nu när du har en databas kan du ansluta och söka med dina favoritverktyg. Lär dig mer genom att välja verktyg nedan:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

