---
title: "Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur du skapar och hanterar Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI-kommandoraden."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: 9a03722e9f71be307bdbf0b846a4cbf7b34cd7ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a>Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI
Brandväggsregler på servernivå kan administratörer hantera åtkomst till en Azure-databas för MySQL-Server från en specifik IP-adress eller intervall av IP-adresser. Med hjälp av lämplig Azure CLI-kommandona, kan du skapa, uppdatera, ta bort, lista, och visa brandväggsregler för att hantera servern. En översikt över Azure-databas för MySQL brandväggar, se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Krav
* [Installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Installera Azure Python SDK för PostgreSQL och MySQL-tjänster
* Installera Azure CLI-komponenten för PostgreSQL och MySQL-tjänster
* Skapa en Azure Database för MySQL-server

## <a name="firewall-rule-commands"></a>Brandväggsregel kommandon:
Den **az mysql-brandväggsregel** kommandot används från Azure CLI för att skapa, ta bort, lista, visa och uppdatera brandväggsreglerna.

Kommandon:
- **Skapa**: skapa en brandväggsregel för Azure MySQL-server.
- **ta bort**: ta bort en brandväggsregel för Azure MySQL-server.
- **listan** : visa en lista med brandväggsregler för Azure MySQL-server.
- **Visa** : Visa detaljer för en server i Azure MySQL brandväggsregel.
- **Uppdatera**: uppdatera en Azure MySQL brandväggsregel.

## <a name="login-to-azure-and-list-your-azure-database-for-mysql-servers"></a>Logga in på Azure listan och Azure-databas för MySQL-servrar
På ett säkert sätt ansluta Azure CLI med Azure-konto. Använd den **az inloggningen** kommando för att göra detta.

1. Kör följande kommando från kommandoraden.
```azurecli
az login
```
Det här kommandot genererar en kod som ska användas i nästa steg.

2. Använd en webbläsare för att öppna sidan [https://aka.ms/devicelogin](https://aka.ms/devicelogin) och ange koden.

3. Logga in med dina autentiseringsuppgifter för Azure när du ombeds göra det.

4. När inloggningen har behörighet, skrivs en lista över prenumerationer i konsolen. Kopiera id för att ange den aktuella prenumerationen för att flytta framåt önskade prenumerationen.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Visa en lista med Azure-databaser för MySQL-servrar för din prenumeration och resurs-grupp om du är osäker på namnen.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Observera namnattributet i listan som ska användas för att ange vilken MySQL-server för att arbeta med. Bekräfta informationen för servern för att använda attributet name och bekräfta att namnet är rätt om det behövs:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lista brandväggsregler på Azure-databas för MySQL-Server 
Med hjälp av namnet på servern och resursgruppens namn, lista över befintliga serverbrandväggsreglerna på servern. Observera att server name-attribut har angetts i den **--server** växla och inte den **--namnet** växla.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
Utdata visar en lista över regler om någon, som standard i JSON-format. Du kan använda växeln **--utdatatabell** för ett mer lättläst tabellformat som utdata.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a>Skapa brandväggsregeln på Azure-databas för MySQL-Server
Skapa en ny brandväggsregel på servern med namnet på Azure MySQL-servern och resursgruppens namn. Ange ett namn för regeln, start-IP och sista IP-regeln för att täcka ett intervall med IP-adresser att tillåta åtkomst.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
Ange samma adress som den första IP- och slut-IP som i det här exemplet för en enda IP-adress ska tillåtas åtkomst.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Kommandoutdata när lyckades, visar en lista över information om brandväggsregeln som du har skapat, som standard i JSON-format. Om det finns ett fel, visar utdata felmeddelandetext i stället.

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a>Uppdatera brandväggsregel på Azure-databas för MySQL-server 
Med Azure MySQL-servernamnet och resursgruppens namn kan uppdatera en befintlig brandväggsregel på servern. Ange namnet på befintlig brandväggsregel som indata och starta IP- och IP-attribut som ska uppdateras.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Kommandoutdata när lyckades, visar en lista över information om brandväggsregeln som du har uppdaterat som standard i JSON-format. Om det finns ett fel, visar utdata felmeddelandetext i stället.

> [!NOTE]
> Om brandväggsregeln inte finns, kommer att skapas med kommandot Uppdatera.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Visa regeldetaljer brandvägg på Azure-databas för MySQL-servern
Använda Azure MySQL-servernamnet och resursgruppens namn, visa befintliga brandväggen Regelinformation från servern. Ange namnet på befintlig brandväggsregel som indata.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Kommandoutdata när lyckades, visar en lista över information om brandväggsregeln som du har angett som standard i JSON-format. Om det finns ett fel, visar utdata felmeddelandetext i stället.

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a>Tar bort brandväggsregeln på Azure-databas för MySQL-Server
Använda Azure MySQL-servernamnet och resursgruppens namn, ta bort en befintlig brandväggsregel från servern. Ange namnet på befintlig brandväggsregel.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Det är klart, inga utdata. Felmeddelandetext vid fel, kommer att returneras.

## <a name="next-steps"></a>Nästa steg
- Mer information om [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)
- [Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure portal](./howto-manage-firewall-using-portal.md)
