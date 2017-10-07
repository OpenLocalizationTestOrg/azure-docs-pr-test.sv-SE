---
title: "aaaDesign ditt första Azure-databas för MySQL-databas - Azure CLI | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur toocreate och hantera Azure-databas för MySQL-servern och databasen med Azure CLI 2.0 hello-kommandoraden."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 6339913c2af58e0e4c4eabb69097a5c9c245781c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a>Utforma din första Azure-databas för MySQL-databas

Azure MySQL-databas är en relationsdatabastjänst i hello Microsoft cloud baserat på MySQL Community Edition databasmotorn. I kursen får du använder Azure CLI (command-line-interface) och andra verktyg toolearn hur till:

> [!div class="checklist"]
> * Skapa en Azure-databas för MySQL
> * Konfigurera hello serverbrandvägg
> * Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate en databas
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
Skapa en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) med [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando. En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.

hello följande exempel skapar en resursgrupp med namnet `mycliresource` i hello `westus` plats.

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Skapa en Azure Database för MySQL-server
Skapa en Azure-databas för MySQL-server med hello az mysql-servern skapa kommando. En server kan hantera flera databaser. Normalt används en separat databas för varje projekt eller för varje användare.

hello följande exempel skapas en Azure-databas för MySQL-server finns i `westus` i hello resursgruppen `mycliresource` med namnet `mycliserver`. hello-servern har en administratörsinloggning i namnet `myadmin` och lösenord `Password01!`. hello server skapas med **grundläggande** prestandanivån och **50** compute enheter som delas mellan alla hello databaser i hello-server. Du kan skala beräkning och lagring uppåt eller nedåt beroende på hello programbehov.

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurera brandväggsregeln
Skapa en Azure-databas för MySQL servernivå brandväggsregel med hello az mysql-brandväggsregel skapa kommando. En brandväggsregel på servernivå som tillåter ett externt program **mysql** kommandoradsverktyget eller MySQL arbetsstationen tooconnect tooyour server via hello Azure MySQL service brandväggen. 

hello skapas följande exempel en brandväggsregel för en fördefinierad adressintervallet. Det här exemplet visar hello hela möjliga IP-adressintervall.

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-hello-connection-information"></a>Hämta hello anslutningsinformation

tooconnect tooyour server behöver du tooprovide värden information och åtkomst-autentiseringsuppgifter.
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

hello resultatet är i JSON-format. Anteckna hello **fullyQualifiedDomainName** och **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "mycliserver.database.windows.net",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/mycliresource/providers/Microsoft.DBforMySQL/servers/mycliserver",
  "location": "westus",
  "name": "mycliserver",
  "resourceGroup": "mycliresource",
  "sku": {
    "capacity": 50,
    "family": null,
    "name": "MYSQLS2M50",
    "size": null,
    "tier": "Basic"
  },
  "storageMb": 2048,
  "tags": null,
  "type": "Microsoft.DBforMySQL/servers",
  "userVisibleState": "Ready",
  "version": null
}
```

## <a name="connect-toohello-server-using-mysql"></a>Ansluta toohello servern med hjälp av mysql
Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish anslutning tooyour Azure-databas för MySQL-servern. I det här exemplet är hello-kommandot:
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a>Skapa en tom databas
När du är ansluten toohello server kan du skapa en tom databas.
```sql
mysql> CREATE DATABASE mysampledb;
```

Kör hello efter kommandot tooswitch hello toothis nyskapad databas hello Kommandotolken:
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a>Skapa tabeller i hello-databas
Nu när du vet hur tooconnect toohello Azure-databas för MySQL-databas, det kan gå igenom hur toocomplete vissa grundläggande uppgifter.

Vi kan först skapa en tabell och läsa in den med vissa data. Nu ska vi skapa en tabell som innehåller information om maskinvaruinventering.
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a>Läs in data till hello tabeller
Nu när vi har en tabell kan vi infoga vissa data i den. Kör följande fråga tooinsert hello vissa rader med data vid hello öppna Kommandotolkens fönster.
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

Nu har du två rader med exempeldata till hello-tabell som du skapade tidigare.

## <a name="query-and-update-hello-data-in-hello-tables"></a>Fråga efter och uppdatera hello data i hello tabeller
Kör följande fråga tooretrieve information från hello databastabell hello.
```sql
SELECT * FROM inventory;
```

Du kan också uppdatera hello data i hello tabeller.
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

hello rad uppdateras i enlighet med detta när du hämtar data.
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a>Återställa en databas tooa tidigare punkt i tiden
Anta att du av misstag har tagit bort den här tabellen. Detta är något du lätt kan återställa från. Azure MySQL-databas kan du toogo tillbaka tooany punkt tidpunkt i hello senast in too35 dagar och återställningspunkt i tid tooa ny server. Du kan använda den här nya servern toorecover dina data. hello följande steg hello exempel server tooa återställningspunkt innan hello tabell har lagts till.

För hello återställning måste hello följande information:

- Återställningspunkt: Välj en i tidpunkt som inträffar innan hello-servern har ändrats. Måste vara större än eller lika med toohello källplatsens databas äldsta säkerhetskopierade värde.
- Målservern: Ange ett nytt servernamn som du vill toorestore till
- Källservern: Ange hello namn hello-server som du vill använda toorestore från
- Plats: Du kan inte välja hello region, som standard är det samma som källservern hello

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

toorestore hello server och [återställa tooa i tidpunkt](./howto-restore-server-portal.md) innan hello tabellen har tagits bort. Återställa en server tooa olika punkt i tiden skapar en ny server dubbla som hello originalservern av hello tidpunkt du anger, förutsatt att den är i hello kvarhållningsperiod för din [tjänstnivån](./concepts-service-tiers.md).

## <a name="next-steps"></a>Nästa steg
I den här kursen har du lärt dig att:
> [!div class="checklist"]
> * Skapa en Azure-databas för MySQL
> * Konfigurera hello serverbrandvägg
> * Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate en databas
> * Läs in exempeldata
> * Frågedata
> * Uppdatera data
> * Återställa data

> [!div class="nextstepaction"]
> [Azure-databas för MySQL - Azure CLI-exempel](./sample-scripts-azure-cli.md)
