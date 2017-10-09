---
title: "aaaCreate och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI | Microsoft Docs"
description: "Den här artikeln beskriver hur toocreate och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI-kommandoraden."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
editor: jasonwhowell
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.openlocfilehash: b2f0b4ccf34ce88e3a5e72a64d3f8c78a5bc2a54
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-database-for-mysql-firewall-rules-using-azure-cli"></a>Skapa och hantera Azure-databas för MySQL brandväggsregler med hjälp av Azure CLI
Brandväggsregler på servernivå aktivera administratörer toomanage åtkomst tooan Azure-databas för MySQL-Server från en specifik IP-adress eller intervall av IP-adresser. Med lämplig Azure CLI-kommandona kan du skapa, uppdatera, ta bort, lista och visa brandväggen regler toomanage servern. En översikt över Azure-databas för MySQL brandväggar, se [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)

## <a name="prerequisites"></a>Krav
* [Installera Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli)
* Installera Azure Python SDK för PostgreSQL och MySQL-tjänster
* Installera hello Azure CLI-komponenten för PostgreSQL och MySQL-tjänster
* Skapa en Azure Database för MySQL-server

## <a name="firewall-rule-commands"></a>Brandväggsregel kommandon:
Hej **az mysql-brandväggsregel** kommandot används från Azure CLI toocreate, ta bort, lista, visa och uppdatera brandväggsreglerna.

Kommandon:
- **Skapa**: skapa en brandväggsregel för Azure MySQL-server.
- **ta bort**: ta bort en brandväggsregel för Azure MySQL-server.
- **listan** : visa en lista med brandväggsregler för hello Azure MySQL-server.
- **Visa** : Visa hello information på en server i Azure MySQL brandväggsregel.
- **Uppdatera**: uppdatera en Azure MySQL brandväggsregel.

## <a name="login-tooazure-and-list-your-azure-database-for-mysql-servers"></a>Inloggningen tooAzure listan och din Azure-databas för MySQL-servrar
På ett säkert sätt ansluta Azure CLI med Azure-konto. Använd hello **az inloggningen** kommandot toodo detta.

1. Kör följande kommando från kommandoraden hello hello.
```azurecli
az login
```
Det här kommandot kommer att bli en kod toouse i hello nästa steg.

2. Använda en webbläsare tooopen hello webbsida [https://aka.ms/devicelogin](https://aka.ms/devicelogin) och ange hello kod.

3. Logga in med dina autentiseringsuppgifter för Azure i Kommandotolken hello.

4. När inloggningen har behörighet, skrivs en lista över prenumerationer ut i hello-konsolen. Kopiera hello-id för hello önskade prenumeration tooset hello aktuell prenumeration toobe används går vidare.
   ```azurecli-interactive
   az account set --subscription {your subscription id}
   ```

5. Lista över hello Azure-databaser för MySQL-servrar för din prenumeration och resurs-grupp om du är osäker på hello namn.

   ```azurecli-interactive
   az mysql server list --resource-group myResourceGroup
   ```

   Observera hello name-attribut i hello lista, som kommer att använda toospecify vilka MySQL server toowork på. Om det behövs, bekräfta hello information för att toousing hello namn attributet tooconfirm servernamnet är korrekt:

   ```azurecli-interactive
   az mysql server show --resource-group myResourceGroup --name mysqlserver4demo
   ```

## <a name="list-firewall-rules-on-azure-database-for-mysql-server"></a>Lista brandväggsregler på Azure-databas för MySQL-Server 
Med hjälp av hello servernamn och hello resursgruppens namn, lista hello befintliga serverbrandväggsreglerna på hello-servern. Observera att hello server name-attribut har angetts i hello **--server** växlar och inte hello **--namnet** växla.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo
```
hello utdata visar en lista över hello regler om någon, som standard i JSON-format. Du kan använda hello växel **--utdatatabell** för ett mer lättläst tabellformat som hello utdata.
```azurecli-interactive
az mysql server firewall-rule list --resource-group myResourceGroup --server mysqlserver4demo --output table
```
## <a name="create-firewall-rule-on-azure-database-for-mysql-server"></a>Skapa brandväggsregeln på Azure-databas för MySQL-Server
Skapa en ny brandväggsregel på hello-servern med hello Azure MySQL-servernamnet och hello resursgruppens namn. Ange ett namn för hello regeln, hello start-IP och sista IP för hello regeln toocover tooallow åtkomst till ett intervall med IP-adresser.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.15
```
För en enda IP-adress toobe åtkomst, ange hello samma adress som hello första IP- och slut-IP, som i följande exempel.
```azurecli-interactive
az mysql server firewall-rule create --resource-group myResourceGroup  
--server mysql --name "Firewall Rule with a Single Address" --start-ip-address 1.1.1.1 --end-ip-address 1.1.1.1
```
Hello kommandoutdata vid lyckades, visar en lista över hello information om hello brandväggsregel som du har skapat, som standard i JSON-format. Om det finns ett fel, visar hello utdata text för felmeddelande i stället.

## <a name="update-firewall-rule-on-azure-database-for-mysql-server"></a>Uppdatera brandväggsregel på Azure-databas för MySQL-server 
Med hello Azure MySQL-servernamnet och hello resursgruppens namn kan uppdatera en befintlig brandväggsregel på hello-servern. Ange hello namnet för hello befintlig brandväggsregel som indata och hello start IP- och IP-attribut tooupdate.
```azurecli-interactive
az mysql server firewall-rule update --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1" --start-ip-address 13.83.152.0 --end-ip-address 13.83.152.1
```
Hello kommandoutdata vid lyckades, visar en lista över hello information om hello brandväggsregel som du har uppdaterat som standard i JSON-format. Om det finns ett fel, visar hello utdata text för felmeddelande i stället.

> [!NOTE]
> Om hello brandväggsregel inte finns, kommer att skapas av hello update-kommandot.

## <a name="show-firewall-rule-details-on-azure-database-for-mysql-server"></a>Visa regeldetaljer brandvägg på Azure-databas för MySQL-servern
Använda hello Azure MySQL-servernamn och hello resursgruppens namn, visa hello befintliga brandväggen Regelinformation från hello-servern. Ange hello namnet för hello befintlig brandväggsregel som indata.
```azurecli-interactive
az mysql server firewall-rule show --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Hello kommandoutdata vid lyckades, visar en lista över hello information om hello brandväggsregel som du har angett som standard i JSON-format. Om det finns ett fel, visar hello utdata text för felmeddelande i stället.

## <a name="delete-firewall-rule-on-azure-database-for-mysql-server"></a>Tar bort brandväggsregeln på Azure-databas för MySQL-Server
Använda hello Azure MySQL-servernamn och hello resursgruppens namn, ta bort en befintlig brandväggsregel från hello-server. Ange hello namnet för hello befintlig brandväggsregel.
```azurecli-interactive
az mysql server firewall-rule delete --resource-group myResourceGroup --server mysqlserver4demo --name "Firewall Rule 1"
```
Det är klart, inga utdata. Vid fel returneras hello felmeddelandetext.

## <a name="next-steps"></a>Nästa steg
- Mer information om [Azure-databas för MySQL serverbrandväggsreglerna](./concepts-firewall-rules.md)
- [Skapa och hantera Azure-databas för MySQL brandväggsregler med hello Azure-portalen](./howto-manage-firewall-using-portal.md)
