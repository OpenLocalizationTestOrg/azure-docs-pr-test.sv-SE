---
title: "Skapa en Azure-databas för PostgreSQL med hello Azure CLI | Microsoft Docs"
description: "Snabb start guiden toocreate och hantera Azure-databas för PostgreSQL-servern med hjälp av Azure CLI (kommandoradsgränssnittet)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: 946aa3cbf5ff9f5ac4e51248412d3da5d718141e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-using-hello-azure-cli"></a>Skapa en Azure-databas för PostgreSQL med hello Azure CLI
Azure PostgreSQL-databas är en hanterad tjänst som gör att du toorun, hantera och skala högtillgänglig PostgreSQL-databaser i hello molnet. hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript. Den här snabbstarten visar hur toocreate en Azure-databas för PostgreSQL-server i en [Azure-resursgrupp](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) med hello Azure CLI.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Om du har flera prenumerationer kan välja hello lämpliga prenumeration hello resursen kommer att debiteras. Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create) kommando. En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp. hello följande exempel skapar en resursgrupp med namnet `myresourcegroup` i hello `westus` plats.
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a>Skapa en Azure Database för PostgreSQL-server

Skapa en [Azure-databas för PostgreSQL server](overview.md) med hello [az postgres servern skapa](/cli/azure/postgres/server#create) kommando. En server innehåller en grupp med databaser som hanteras som en grupp. 

hello följande exempel skapas en server med namnet `mypgserver-20170401` i resursgruppen `myresourcegroup` med inloggning för serveradministratör `mylogin`. hello namnet på en server mappar tooDNS namn och därför krävs toobe globalt unikt i Azure. Ersätt hello `<server_admin_password>` med ett eget värde.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> hello server admin inloggningsnamn och lösenord som du anger här är nödvändig toolog i toohello server och databaserna senare i den här snabbstartsguide. Kom ihåg eller skriv ned den här informationen så att du kan använda den senare.

Som standard skapas **postgres**-databasen under din server. Hej [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databasen är en standarddatabas som är avsedd för användning av användare, verktyg och program från tredje part. 


## <a name="configure-a-server-level-firewall-rule"></a>Konfigurera en brandväggsregel på servernivå

Skapa en Azure PostgreSQL servernivå brandväggsregel med hello [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) kommando. En brandväggsregel på servernivå som tillåter ett externt program [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) eller [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server via hello Azure PostgreSQL tjänsten brandvägg. 

Du kan ange en brandväggsregel som omfattar en IP-intervallet toobe kan tooconnect från nätverket. hello följande exempel används [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) toocreate en brandväggsregel `AllowAllIps` för en IP-adressintervall. tooopen alla IP-adresser, använda 0.0.0.0 som hello startar IP-adress och 255.255.255.255 som hello slutadress.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> Azure PostgreSQL-servern kommunicerar via port 5432. När du ansluter innifrån ett företagsnätverk är det möjligt att utgående trafik via port 5432 inte tillåts av nätverkets brandvägg. Har din IT-avdelning öppna port 5432 tooconnect tooyour Azure SQL Database-server.

## <a name="get-hello-connection-information"></a>Hämta hello anslutningsinformation

tooconnect tooyour server behöver du tooprovide värden information och åtkomst-autentiseringsuppgifter.
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

hello resultatet är i JSON-format. Anteckna hello **administratorLogin** och **fullyQualifiedDomainName**.
```json
{
  "administratorLogin": "mylogin",
  "fullyQualifiedDomainName": "mypgserver-20170401.postgres.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforPostgreSQL/servers/mypgserver-20170401",
  "location": "westus",
  "name": "mypgserver-20170401",
  "resourceGroup": "myresourcegroup",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "PGSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "sslEnforcement": null,
  "storageMb": 51200,
  "tags": null,
  "type": "Microsoft.DBforPostgreSQL/servers",
  "userVisibleState": "Ready",
  "version": "9.6"
}
```

## <a name="connect-toopostgresql-database-using-psql"></a>Ansluta tooPostgreSQL databasen med hjälp av psql

Om klientdatorn har PostgreSQL installerat, kan du använda en lokal instans av [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL-servern. Låt oss nu använda hello psql kommandoradsverktyget tooconnect toohello Azure PostgreSQL server.

1. Kör hello följande psql kommandot tooconnect tooan Azure-databas för PostgreSQL-server
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Till exempel följande kommando hello ansluter toohello standarddatabasen kallas **postgres** på servern PostgreSQL **mypgserver 20170401.postgres.database.azure.com** hjälp av autentiseringsuppgifter. Ange hello `<server_admin_password>` du valde när du uppmanas att ange lösenord.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  När du är ansluten toohello server kan du skapa en tom databas hello i Kommandotolken.
```sql
CREATE DATABASE mypgsqldb;
```

3.  I Kommandotolken hello köra hello efter kommandot tooswitch toohello nyskapad databas **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="connect-toopostgresql-database-using-pgadmin"></a>Ansluta tooPostgreSQL databasen med hjälp av pgAdmin

tooconnect tooAzure PostgreSQL servern med hjälp av verktyget hello GUI _pgAdmin_
1.  Starta hello _pgAdmin_ på klientdatorn. Du kan installera _pgAdmin_ från http://www.pgadmin.org/.
2.  Välj **Lägg till ny Server** från hello **snabblänkar** menyn.
3.  I hello **skapa - Server** dialogrutan **allmänna** ange ett unikt namn för hello-servern. Låt säga **Azure PostgreSQL Server**.
 ![pgAdmin-verktyget – skapa – server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)
4.  I hello **skapa - Server** dialogrutan **anslutning** fliken:
    - Ange hello fullständigt kvalificerade servernamnet (till exempel **mypgserver 20170401.postgres.database.azure.com**) i hello **värddatorns namn / adress** rutan. 
    - Ange port 5432 i hello **Port** rutan. 
    - Ange hello **inloggning för serveradministratör (user@mypgserver)** fick tidigare i det här quickstart och lösenordet du angav när du skapade hello server i hello **användarnamn** och **lösenord** respektive rutorna.
    - Välj **SSL-läge** som **kräv**. Som standard skapas alla Azure PostgreSQL-servrar med SSL tvingande aktiverat. tooturn av SSL genomdriva mer information finns i [framtvinga SSL](./concepts-ssl-connection-security.md).

    ![pgAdmin – skapa – server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  Klicka på **Spara**.
6.  I hello webbläsaren till vänster och expanderar hello **servergrupper**. Välj din server **Azure PostgreSQL-server**.
7.  Välj hello **Server** du ansluten till och välj sedan **databaser** under den. 
8.  Högerklicka på **databaser** tooCreate en databas.
9.  Välj ett databasnamn **mypgsqldb** och hello ägare för den som inloggning för serveradministratör **mylogin**.
10. Klicka på **spara** toocreate en tom databas.
11. I hello **webbläsare**, expandera hello **servrar** grupp. Expandera hello-server som du skapade och se hello databasen **mypgsqldb** under den.
 ![pgAdmin – skapa – databas](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)


## <a name="clean-up-resources"></a>Rensa resurser

Rensa alla resurser som du skapade i hello quickstart genom att ta bort hello [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md).

> [!TIP]
> De andra snabbstarterna i den här samlingen bygger på den här snabbstarten. Om du tänker toocontinue toowork med efterföljande hello Snabbstart, vill inte rensa resurser som skapats i denna Snabbstart. Om du inte planerar toocontinue använder du följande steg toodelete hello alla resurser som skapats av denna Snabbstart i hello Azure CLI.

```azurecli-interactive
az group delete --name myresourcegroup
```

Om du bara vill toodelete hello en nyligen skapade server kan du köra [az postgres server delete](/cli/azure/postgres/server#delete) kommando.
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a>Nästa steg
> [!div class="nextstepaction"]
> [Migrera din databas med Exportera och importera](./howto-migrate-using-export-and-import.md)
