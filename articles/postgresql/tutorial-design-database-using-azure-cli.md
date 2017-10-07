---
title: "Utforma din första Azure-databas för PostgreSQL med Azure CLI | Microsoft Docs"
description: "Den här kursen visar hur tooDesign ditt första Azure-databas för PostgreSQL med Azure CLI."
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.custom: mvc
ms.devlang: azure-cli
ms.topic: tutorial
ms.date: 06/13/2017
ms.openlocfilehash: 7914925c090e0b6f3e7c8a999eedb0b2baf83d7d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a>Utforma din första Azure-databas för PostgreSQL med Azure CLI 
I kursen får du använder Azure CLI (command-line-interface) och andra verktyg toolearn hur till:
> [!div class="checklist"]
> * Skapa en Azure Database för PostgreSQL
> * Konfigurera hello serverbrandvägg
> * Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) toocreate verktyget en databas
> * Läs in exempeldata
> * Frågedata
> * Uppdatera data
> * Återställa data

Du kan använda hello Azure Cloud Shell i hello webbläsare eller [installera Azure CLI 2.0]( /cli/azure/install-azure-cli) på din egen dator toorun hello kodblock i den här självstudiekursen.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Om du har flera prenumerationer, Välj hello lämpliga prenumeration där hello resursen finns eller faktureras för. Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).
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

hello följande exempel skapas en server med namnet `mypgserver-20170401` i resursgruppen `myresourcegroup` med inloggning för serveradministratör `mylogin`. Namnet på en server mappar tooDNS namn och därför krävs toobe globalt unikt i Azure. Ersätt hello `<server_admin_password>` med ett eget värde.
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
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
>

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

## <a name="connect-tooazure-database-for-postgresql-database-using-psql"></a>Ansluta tooAzure databas för PostgreSQL-databas med hjälp av psql
Om klientdatorn har PostgreSQL installerat, kan du använda en lokal instans av [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), eller hello Azure Cloud-konsolen tooconnect tooan Azure PostgreSQL server. Vi använder nu hello psql kommandoradsverktyget tooconnect toohello Azure-databas för PostgreSQL-server.

1. Kör hello följande psql kommandot tooconnect tooan Azure-databas för PostgreSQL-server
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  Till exempel följande kommando hello ansluter toohello standarddatabasen kallas **postgres** på servern PostgreSQL **mypgserver 20170401.postgres.database.azure.com** hjälp av autentiseringsuppgifter. Ange hello `<server_admin_password>` du valde när du uppmanas att ange lösenord.
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  När du är ansluten toohello server kan du skapa en tom databas hello i Kommandotolken.
```sql
CREATE DATABASE mypgsqldb;
```

3.  I Kommandotolken hello köra hello efter kommandot tooswitch toohello nyskapad databas **mypgsqldb**:
```sql
\c mypgsqldb
```

## <a name="create-tables-in-hello-database"></a>Skapa tabeller i hello-databas
Nu när du vet hur tooconnect toohello Azure-databas för PostgreSQL vi kan gå igenom hur toocomplete vissa grundläggande uppgifter.

Vi kan först skapa en tabell och läsa in den med vissa data. Nu ska vi skapa en tabell som spårar inventeringsinformation.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

Du kan se hello nyligen skapade tabellen i hello listan över tabeller nu genom att skriva:
```sql
\dt
```

## <a name="load-data-into-hello-tables"></a>Läs in data till hello tabeller
Nu när vi har en tabell kan vi infoga vissa data i den. Kör följande fråga tooinsert hello vissa rader med data vid hello öppna Kommandotolkens fönster
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Du har nu två rader med exempeldata till hello-tabell som du skapade tidigare.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Fråga efter och uppdatera hello data i hello tabeller
Kör följande fråga tooretrieve information från hello databastabell hello. 
```sql
SELECT * FROM inventory;
```

Du kan också uppdatera hello data i hello tabeller
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

hello rad uppdateras i enlighet med detta när du hämtar data.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Återställa en databas tooa tidigare punkt i tiden
Anta att du av misstag har tagit bort en tabell. Detta är något du lätt kan återställa från. Azure PostgreSQL-databas kan du toogo tillbaka tooany i tidpunkt (i hello senast too7 dagar (grundläggande) och 35 dagar (Standard)) och återställa den här nya tooa point-in-time-servern. Du kan använda den här nya servern toorecover dina data. hello följande steg hello exempel server tooa återställningspunkt innan hello tabell har lagts till.

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

Hej `az postgres server restore` kommandot måste hello följande parametrar:
| Inställning | Föreslaget värde | Beskrivning  |
| --- | --- | --- |
| --resursgruppen. |  myResourceGroup |  Resursgruppens namn i vilka hello källservern finns.  |
| --namn | mypgserver återställs | hello namnet på hello nya server som skapas av hello restore-kommandot. |
| Återställ punkt i tiden | 2017-04-13T13:59:00Z | Välj en tidpunkt i toorestore till. Datumet och tiden måste vara inom hello källserverns kvarhållningsperiod för säkerhetskopiering. Använd ISO8601-formatet för datum och tid. Exempelvis kan du använda din egen lokala tidszon som `2017-04-13T05:59:00-08:00`, eller använda UTC Zulu format `2017-04-13T13:59:00Z`. |
| --käll-servern | mypgserver 20170401 | hello namnet eller ID för hello källa server toorestore från. |

Återställa en server tooa i tidpunkt skapar en ny server och kopiera som hello ursprungliga server från och med hello punkt tidpunkt du anger. hello plats och prissättning nivåvärden för hello återställts server är hello samma som hello källservern.

hello kommandot synkront och returneras när hello server har återställts. Leta upp hello nya servern som skapades när hello återställning har slutförts. Kontrollera hello data har återställts som förväntat.


## <a name="next-steps"></a>Nästa steg
I kursen får du lärt dig hur toouse Azure CLI (command-line-interface) och andra verktyg för att:
> [!div class="checklist"]
> * Skapa en Azure Database för PostgreSQL
> * Konfigurera hello serverbrandvägg
> * Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) toocreate verktyget en databas
> * Läs in exempeldata
> * Frågedata
> * Uppdatera data
> * Återställa data

Därefter lär du dig hur toouse hello Azure portal toodo liknande uppgifter, granska den här självstudiekursen: [utforma din första Azure-databas för PostgreSQL med hello Azure-portalen](tutorial-design-database-using-azure-portal.md)
