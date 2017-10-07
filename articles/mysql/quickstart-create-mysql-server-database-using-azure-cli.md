---
title: "Snabbstart: Skapa en Azure Database för MySQL-server – Azure CLI | Microsoft Docs"
description: "Denna Snabbstart beskriver hur hello toouse Azure CLI toocreate en Azure-databas för MySQL-server i en Azure-resursgrupp."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: hero-article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 708d0cce12e812cb464adcf7e83e6f85c196bafe
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-mysql-server-using-azure-cli"></a>Skapa en Azure Database för MySQL-server med Azure CLI
Denna Snabbstart beskriver hur toouse hello Azure CLI toocreate en Azure-databas för MySQL-server i en Azure-resursgrupp om fem minuter. hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare. Kör `az --version` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli). 

Om du har flera prenumerationer, Välj hello lämpliga prenumeration där hello resursen finns eller faktureras för. Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a>Skapa en resursgrupp
Skapa en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) med hello [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando. En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.

hello följande exempel skapar en resursgrupp med namnet `myresourcegroup` i hello `westus` plats.

```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a>Skapa en Azure Database för MySQL-server
Skapa en Azure-databas för MySQL-server med hello **az mysql-servern skapa** kommando. En server kan hantera flera databaser. Normalt används en separat databas för varje projekt eller för varje användare.

hello följande exempel skapas en Azure-databas för MySQL-server finns i `westus` i hello resursgruppen `myresourcegroup` med namnet `myserver4demo`. hello-servern har en administratörsinloggning i namnet `myadmin` och lösenord `Password01!`. hello server skapas med **grundläggande** prestandanivån och **50** compute enheter som delas mellan alla hello databaser i hello-server. Du kan skala beräkning och lagring uppåt eller nedåt beroende på hello programbehov.

```azurecli-interactive
az mysql server create --resource-group myresourcegroup --name myserver4demo --location westus --admin-user myadmin --admin-password Password01! --performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a>Konfigurera brandväggsregeln
Skapa en Azure-databas för MySQL servernivå brandväggsregel med hello **az mysql-brandväggsregel skapa** kommando. En brandväggsregel på servernivå kan ett externt program, till exempel hello **mysql.exe** kommandoradsverktyget eller MySQL arbetsstationen tooconnect tooyour server via hello Azure MySQL service brandväggen. 

hello skapas följande exempel en brandväggsregel för en fördefinierad-adressintervall som i det här exemplet är hello hela möjliga IP-adressintervall.

```azurecli-interactive
az mysql server firewall-rule create --resource-group myresourcegroup --server myserver4demo --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
## <a name="configure-ssl-settings"></a>Konfigurera SSL-inställningar
Som standard verkställs SSL-anslutningar mellan servern och dina klientprogram.  Detta garanterar säkerheten för ”i rörelse” data genom att kryptera hello dataströmmen över hello internet.  toomake som den här snabbstartsguide lättare vi att inaktivera SSL-anslutningar för servern.  Detta rekommenderas inte för produktionsservrar.  Mer information finns i [Konfigurera SSL-anslutning i ditt program toosecurely ansluta tooAzure databas för MySQL](./howto-configure-ssl.md).

hello inaktiverar följande exempel att framtvinga SSL på MySQL-servern.
 
 ```azurecli-interactive
 az mysql server update --resource-group myresourcegroup --name myserver4demo -g -n --ssl-enforcement Disabled
 ```

## <a name="get-hello-connection-information"></a>Hämta hello anslutningsinformation

tooconnect tooyour server behöver du tooprovide värden information och åtkomst-autentiseringsuppgifter.

```azurecli-interactive
az mysql server show --resource-group myresourcegroup --name myserver4demo
```

hello resultatet är i JSON-format. Anteckna hello **fullyQualifiedDomainName** och **administratorLogin**.
```json
{
  "administratorLogin": "myadmin",
  "administratorLoginPassword": null,
  "fullyQualifiedDomainName": "myserver4demo.mysql.database.azure.com",
  "id": "/subscriptions/00000000-0000-0000-0000-000000000000/resourceGroups/myresourcegroup/providers/Microsoft.DBforMySQL/servers/myserver4demo",
  "location": "westus",
  "name": "myserver4demo",
  "resourceGroup": "myresourcegroup",
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

## <a name="connect-toohello-server-using-hello-mysqlexe-command-line-tool"></a>Ansluta toohello servern med hjälp av kommandoradsverktyget för hello mysql.exe
Ansluta tooyour servern med hjälp av hello **mysql.exe** kommandoradsverktyget. Du kan hämta MySQL [här](https://dev.mysql.com/downloads/) och installera programmet på din dator. I stället kan du också klicka hello **prova** i kodexempel eller hello `>_` knappen hello övre högra verktygsfältet i hello Azure-portalen och starta hello **Azure Cloud Shell**.

Skriv hello nästa kommandon: 

1. Ansluta toohello server med **mysql** kommandoradsverktyget:
```azurecli-interactive
 mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
```

2. Visa status för servern:
```sql
 mysql> status
```
Om allt går bra ska hello kommandoradsverktyget utdata hello följande text:

```dos
C:\Users\>mysql -h myserver4demo.mysql.database.azure.com -u myadmin@myserver4demo -p
Enter password: ***********
Welcome toohello MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 65512
Server version: 5.6.26.0 MySQL Community Server (GPL)

Copyright (c) 2000, 2016, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' tooclear hello current input statement.

mysql> status
--------------
mysql  Ver 14.14 Distrib 5.6.35, for Win64 (x86_64)

Connection id:          65512
Current database:
Current user:           myadmin@116.230.243.143
SSL:                    Not in use
Using delimiter:        ;
Server version:         5.6.26.0 MySQL Community Server (GPL)
Protocol version:       10
Connection:             myserver4demo.mysql.database.azure.com via TCP/IP
Server characterset:    latin1
Db     characterset:    latin1
Client characterset:    gbk
Conn.  characterset:    gbk
TCP port:               3306
Uptime:                 2 days 9 hours 47 min 20 sec

Threads: 4  Questions: 34833  Slow queries: 2  Opens: 84  Flush tables: 4  Open tables: 1  Queries per second avg: 0.167
--------------

mysql>
```

> [!TIP]
> Fler kommandon finns i [referenshandboken för MySQL 5.7 – kapitel 4.5.1](https://dev.mysql.com/doc/refman/5.7/en/mysql.html).

## <a name="connect-toohello-server-using-hello-mysql-workbench-gui-tool"></a>Ansluta toohello servern med hjälp av hello MySQL arbetsstationen GUI-verktyg
1.  Starta hello MySQL arbetsstationen programmet på klientdatorn. Du kan ladda ned och installera MySQL Workbench [här](https://dev.mysql.com/downloads/workbench/).

2.  I hello **installera ny anslutning** dialogrutan Ange följande information hello **parametrar** fliken:

   ![konfigurera ny anslutning](./media/quickstart-create-mysql-server-database-using-azure-cli/setup-new-connection.png)

| **Inställning** | **Föreslaget värde** | **Beskrivning** |
|---|---|---|
|   Anslutningsnamn | Min anslutning | Ange ett namn på anslutningen (det kan vara vad som helst) |
| Anslutningsmetod | välj Standard (TCP/IP) | Använda TCP/IP-protokollet tooconnect tooAzure databas för MySQL > |
| Värdnamn | myserver4demo.mysql.database.azure.com | Servernamn som du antecknade tidigare. |
| Port | 3306 | hello-standardporten för MySQL används. |
| Användarnamn | myadmin@myserver4demo | hello inloggning för serveradministratör du antecknade tidigare. |
| Lösenord | **** | Använd hello admin kontolösenord som du tidigare har konfigurerat. |

Klicka på **Testanslutningen** tootest om alla parametrar är korrekt konfigurerade.
Nu kan du klicka på hello anslutning toosuccessfully ansluta toohello server.

## <a name="clean-up-resources"></a>Rensa resurser
Om du inte behöver dessa resurser för en annan Snabbstartsguide, kan du ta bort dem genom att göra hello följande kommando: 

```azurecli-interactive
az group delete --name myresourcegroup
```

## <a name="next-steps"></a>Nästa steg

> [!div class="nextstepaction"]
> [Utforma en MySQL-databas med Azure CLI](./tutorial-design-database-using-cli.md)
