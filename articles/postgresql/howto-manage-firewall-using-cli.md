---
title: "aaaCreate och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI-kommandoraden."
services: postgresql
author: jasonwhowell
ms.author: jasonh
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 06e34c9e3996c2ec3df63191d794bc34d0ca0cf2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-postgresql-firewall-rules-using-azure-cli"></a>Skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hjälp av Azure CLI
Brandväggsregler på servernivå aktivera administratörer toomanage åtkomst tooan Azure-databas för PostgreSQL-Server från en specifik IP-adress eller intervall av IP-adresser. Med lämplig Azure CLI-kommandona kan du skapa, uppdatera, ta bort, lista och visa brandväggen regler toomanage servern. En översikt över Azure-databas för PostgreSQL brandväggar, se [Azure-databas för PostgreSQL serverbrandväggsreglerna](concepts-firewall-rules.md)

## <a name="prerequisites"></a>Krav
toostep via den här hur tooguide behöver du:
- En [Azure-databas för PostgreSQL-server och databas](quickstart-create-server-database-azure-cli.md)
- Installera [Azure CLI 2.0](/cli/azure/install-azure-cli) kommandoraden verktyg eller Använd hello Azure Cloud Shell i hello webbläsare.

## <a name="configure-firewall-rules-for-azure-database-for-postgresql"></a>Konfigurera brandväggsregler för Azure-databas för PostgreSQL
Hej [az postgres-brandväggsregel](/cli/azure/postgres/server/firewall-rule) kommandon är används tooconfigure brandväggsregler.

## <a name="list-firewall-rules"></a>Lista brandväggsregler 
toolist hello befintliga server brandväggsregler på hello-servern kör hello [az postgres brandväggsregel serverlista](/cli/azure/postgres/server/firewall-rule#list) kommando.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401
```
hello utdata visar hello regler om någon, som standard i JSON-format. Du kan använda hello växel `--output table` för ett mer lättläst tabellformat som hello utdata.
```azurecli-interactive
az postgres server firewall-rule list --resource-group myresourcegroup --server mypgserver-20170401 --output table
```
## <a name="create-firewall-rule"></a>Skapa brandväggsregel
toocreate en ny brandväggsregel på hello-servern kör hello [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) kommando. 

Det här exemplet tillåter en mängd alla IP-adresser tooaccess hello server **mypgserver 20170401.postgres.database.azure.com**
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```
Ange tooallow en enda IP-adress tooaccess hello samma adress som hello första IP- och slut-IP, som i följande exempel.
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup  
--server mypgserver-20170401 --name "AllowSingleIpAddress" --start-ip-address 13.83.152.1 --end-ip-address 13.83.152.1
```
Vid lyckades visar hello kommandoutdata hello information om hello brandväggsregel som du har skapat, som standard i JSON-format. Om det finns ett fel, utdata hello showserror meddelandetexten i stället.

## <a name="update-firewall-rule"></a>Uppdatera brandväggsregel 
Uppdatera en befintlig brandväggsregel på hello-servern med [az postgres server-brandväggsregel uppdateringen](/cli/azure/postgres/server/firewall-rule#update) kommando. Ange hello namnet för hello befintlig brandväggsregel som indata och hello start IP- och IP-attribut tooupdate.
```azurecli-interactive
az postgres server firewall-rule update --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.255
```
Vid lyckades visar hello kommandoutdata hello information om hello brandväggsregel som du har uppdaterat som standard i JSON-format. Om det finns ett fel, utdata hello showserror meddelandetexten i stället.
> [!NOTE]
> Om hello brandväggsregel inte finns, hämtar den skapats av hello update-kommandot.

## <a name="show-firewall-rule-details"></a>Visa brandväggen regeldetaljer
Du kan också visa Regelinformation om för en server för befintliga hello-brandväggen genom att köra [az postgres server-brandväggsregel visa](/cli/azure/postgres/server/firewall-rule#show) kommando.
```azurecli-interactive
az postgres server firewall-rule show --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Vid lyckades visar hello kommandoutdata hello information om hello brandväggsregel som du har angett som standard i JSON-format. Om det finns ett fel, utdata hello showserror meddelandetexten i stället.

## <a name="delete-firewall-rule"></a>Ta bort brandväggsregel
toorevoke åtkomst för ett IP-adressintervall från hello-servern, ta bort en befintlig brandväggsregel genom att köra hello [az postgres-brandväggsregel ta bort](/cli/azure/postgres/server/firewall-rule#delete) kommando. Ange hello namnet för hello befintlig brandväggsregel.
```azurecli-interactive
az postgres server firewall-rule delete --resource-group myresourcegroup --server mypgserver-20170401 --name "AllowIpRange"
```
Det är klart, inga utdata. Vid fel returneras hello felmeddelandetext.

## <a name="next-steps"></a>Nästa steg
- På liknande sätt kan du använda en webbläsare för[skapa och hantera Azure-databas för PostgreSQL brandväggsregler med hello Azure-portalen](howto-manage-firewall-using-portal.md)
- Mer information om [Azure-databas för PostgreSQL serverbrandväggsreglerna](concepts-firewall-rules.md)
- Hjälp med att ansluta tooan Azure-databas för PostgreSQL-servern finns [anslutningsbibliotek för Azure-databas för PostgreSQL](concepts-connection-libraries.md)
