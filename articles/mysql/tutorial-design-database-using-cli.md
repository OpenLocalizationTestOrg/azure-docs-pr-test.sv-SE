---
title: "Utforma din första Azure-databas för MySQL - databas i Azure CLI | Microsoft Docs"
description: "Den här självstudiekursen beskrivs hur du skapar och hanterar Azure-databas för MySQL-server och databas med hjälp av Azure CLI 2.0 från kommandoraden."
services: mysql
author: v-chenyh
ms.author: v-chenyh
manager: jhubbard
ms.service: mysql-database
ms.devlang: azure-cli
ms.topic: article
ms.date: 06/13/2017
ms.custom: mvc
ms.openlocfilehash: 590cba6cb58d0c0eaedb9f122ac048c33988004d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="82d4a-103">Utforma din första Azure-databas för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="82d4a-103">Design your first Azure Database for MySQL database</span></span>

<span data-ttu-id="82d4a-104">Azure MySQL-databas är en relationsdatabastjänst i molnet Microsoft utifrån MySQL Community Edition databasmotorn.</span><span class="sxs-lookup"><span data-stu-id="82d4a-104">Azure Database for MySQL is a relational database service in the Microsoft cloud based on MySQL Community Edition database engine.</span></span> <span data-ttu-id="82d4a-105">I kursen får du använder Azure CLI (command-line-interface) och andra verktyg att lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="82d4a-105">In this tutorial, you use Azure CLI (command-line interface) and other utilities to learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="82d4a-106">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="82d4a-106">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="82d4a-107">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="82d4a-107">Configure the server firewall</span></span>
> * <span data-ttu-id="82d4a-108">Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="82d4a-108">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) to create a database</span></span>
> * <span data-ttu-id="82d4a-109">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="82d4a-109">Load sample data</span></span>
> * <span data-ttu-id="82d4a-110">Frågedata</span><span class="sxs-lookup"><span data-stu-id="82d4a-110">Query data</span></span>
> * <span data-ttu-id="82d4a-111">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="82d4a-111">Update data</span></span>
> * <span data-ttu-id="82d4a-112">Återställa data</span><span class="sxs-lookup"><span data-stu-id="82d4a-112">Restore data</span></span>

<span data-ttu-id="82d4a-113">Du kan använda Azure Cloud Shell i webbläsaren eller [installera Azure CLI 2.0]( /cli/azure/install-azure-cli) på din dator för att köra kodblock i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="82d4a-113">You may use the Azure Cloud Shell in the browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer to run the code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="82d4a-114">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="82d4a-114">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="82d4a-115">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="82d4a-115">Run `az --version` to find the version.</span></span> <span data-ttu-id="82d4a-116">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="82d4a-116">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="82d4a-117">Om du har flera prenumerationer väljer du en lämplig prenumerationen där resursen ligger eller faktureras.</span><span class="sxs-lookup"><span data-stu-id="82d4a-117">If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span> <span data-ttu-id="82d4a-118">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="82d4a-118">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="82d4a-119">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="82d4a-119">Create a resource group</span></span>
<span data-ttu-id="82d4a-120">Skapa en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) med [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="82d4a-120">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) with [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="82d4a-121">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="82d4a-121">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="82d4a-122">I följande exempel skapas en resursgrupp med namnet `mycliresource` på platsen `westus`.</span><span class="sxs-lookup"><span data-stu-id="82d4a-122">The following example creates a resource group named `mycliresource` in the `westus` location.</span></span>

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="82d4a-123">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="82d4a-123">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="82d4a-124">Skapa en Azure-databas för MySQL-server med az mysql-servern skapa kommando.</span><span class="sxs-lookup"><span data-stu-id="82d4a-124">Create an Azure Database for MySQL server with the az mysql server create command.</span></span> <span data-ttu-id="82d4a-125">En server kan hantera flera databaser.</span><span class="sxs-lookup"><span data-stu-id="82d4a-125">A server can manage multiple databases.</span></span> <span data-ttu-id="82d4a-126">Normalt används en separat databas för varje projekt eller för varje användare.</span><span class="sxs-lookup"><span data-stu-id="82d4a-126">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="82d4a-127">I följande exempel skapas en Azure Database för MySQL-server i `westus` i resursgruppen `mycliresource` med namnet `mycliserver`.</span><span class="sxs-lookup"><span data-stu-id="82d4a-127">The following example creates an Azure Database for MySQL server located in `westus` in the resource group `mycliresource` with name `mycliserver`.</span></span> <span data-ttu-id="82d4a-128">Servern har en administratörsinloggning med namnet `myadmin` och lösenordet `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="82d4a-128">The server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="82d4a-129">Servern skapas med prestandanivån **Basic** och **50** beräkningsenheter som delas mellan alla databaser på servern.</span><span class="sxs-lookup"><span data-stu-id="82d4a-129">The server is created with **Basic** performance tier and **50** compute units shared between all the databases in the server.</span></span> <span data-ttu-id="82d4a-130">Du kan skala beräkning och lagring uppåt eller nedåt beroende på behoven i dina appar.</span><span class="sxs-lookup"><span data-stu-id="82d4a-130">You can scale compute and storage up or down depending on the application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="82d4a-131">Konfigurera brandväggsregeln</span><span class="sxs-lookup"><span data-stu-id="82d4a-131">Configure firewall rule</span></span>
<span data-ttu-id="82d4a-132">Skapa en Azure-databas för MySQL servernivå brandväggsregel med az mysql server-brandväggsregeln skapa kommando.</span><span class="sxs-lookup"><span data-stu-id="82d4a-132">Create an Azure Database for MySQL server-level firewall rule with the az mysql server firewall-rule create command.</span></span> <span data-ttu-id="82d4a-133">En brandväggsregel på servernivå som tillåter ett externt program **mysql** kommandoradsverktyget eller MySQL-arbetsstationen för att ansluta till servern via brandväggen MySQL på Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="82d4a-133">A server-level firewall rule allows an external application, such as **mysql** command-line tool or MySQL Workbench to connect to your server through the Azure MySQL service firewall.</span></span> 

<span data-ttu-id="82d4a-134">I följande exempel skapas en brandväggsregel för en fördefinierad adressintervallet.</span><span class="sxs-lookup"><span data-stu-id="82d4a-134">The following example creates a firewall rule for a predefined address range.</span></span> <span data-ttu-id="82d4a-135">Det här exemplet visar hela möjliga intervallet av IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="82d4a-135">This example shows the entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-the-connection-information"></a><span data-ttu-id="82d4a-136">Hämta anslutningsinformationen</span><span class="sxs-lookup"><span data-stu-id="82d4a-136">Get the connection information</span></span>

<span data-ttu-id="82d4a-137">För att ansluta till servern måste du ange värddatorinformationen och autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="82d4a-137">To connect to your server, you need to provide host information and access credentials.</span></span>
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

<span data-ttu-id="82d4a-138">Resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="82d4a-138">The result is in JSON format.</span></span> <span data-ttu-id="82d4a-139">Anteckna **fullyQualifiedDomainName** och **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="82d4a-139">Make a note of the **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-to-the-server-using-mysql"></a><span data-ttu-id="82d4a-140">Ansluta till servern med mysql</span><span class="sxs-lookup"><span data-stu-id="82d4a-140">Connect to the server using mysql</span></span>
<span data-ttu-id="82d4a-141">Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) att upprätta en anslutning till din Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="82d4a-141">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) to establish a connection to your Azure Database for MySQL server.</span></span> <span data-ttu-id="82d4a-142">I det här exemplet är kommandot:</span><span class="sxs-lookup"><span data-stu-id="82d4a-142">In this example, the command is:</span></span>
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="82d4a-143">Skapa en tom databas</span><span class="sxs-lookup"><span data-stu-id="82d4a-143">Create a blank database</span></span>
<span data-ttu-id="82d4a-144">När du är ansluten till servern kan du skapa en tom databas.</span><span class="sxs-lookup"><span data-stu-id="82d4a-144">Once you’re connected to the server, create a blank database.</span></span>
```sql
mysql> CREATE DATABASE mysampledb;
```

<span data-ttu-id="82d4a-145">Kör följande kommando för att växla anslutningen till den nya databasen i Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="82d4a-145">At the prompt, run the following command to switch the connection to this newly created database:</span></span>
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-the-database"></a><span data-ttu-id="82d4a-146">Skapa tabeller i databasen</span><span class="sxs-lookup"><span data-stu-id="82d4a-146">Create tables in the database</span></span>
<span data-ttu-id="82d4a-147">Nu när du vet hur du ansluter till Azure-databasen för MySQL-databas, gå vi igenom hur du utför några grundläggande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="82d4a-147">Now that you know how to connect to the Azure Database for MySQL database, we can go over how to complete some basic tasks.</span></span>

<span data-ttu-id="82d4a-148">Vi kan först skapa en tabell och läsa in den med vissa data.</span><span class="sxs-lookup"><span data-stu-id="82d4a-148">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="82d4a-149">Nu ska vi skapa en tabell som innehåller information om maskinvaruinventering.</span><span class="sxs-lookup"><span data-stu-id="82d4a-149">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-the-tables"></a><span data-ttu-id="82d4a-150">Läser in data i tabeller</span><span class="sxs-lookup"><span data-stu-id="82d4a-150">Load data into the tables</span></span>
<span data-ttu-id="82d4a-151">Nu när vi har en tabell kan vi infoga vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="82d4a-151">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="82d4a-152">Kör följande fråga för att infoga vissa rader med data i öppna en kommandotolk-fönster.</span><span class="sxs-lookup"><span data-stu-id="82d4a-152">At the open command prompt window, run the following query to insert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="82d4a-153">Nu har du två rader med exempeldata i tabellen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="82d4a-153">Now you have two rows of sample data into the table you created earlier.</span></span>

## <a name="query-and-update-the-data-in-the-tables"></a><span data-ttu-id="82d4a-154">Fråga efter och uppdatera data i tabeller</span><span class="sxs-lookup"><span data-stu-id="82d4a-154">Query and update the data in the tables</span></span>
<span data-ttu-id="82d4a-155">Kör följande fråga för att hämta information från databastabellen.</span><span class="sxs-lookup"><span data-stu-id="82d4a-155">Execute the following query to retrieve information from the database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="82d4a-156">Du kan också uppdatera data i tabeller.</span><span class="sxs-lookup"><span data-stu-id="82d4a-156">You can also update the data in the tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="82d4a-157">Raden uppdateras i enlighet med detta när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="82d4a-157">The row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a><span data-ttu-id="82d4a-158">Återställa en databas till en tidigare tidpunkt</span><span class="sxs-lookup"><span data-stu-id="82d4a-158">Restore a database to a previous point in time</span></span>
<span data-ttu-id="82d4a-159">Anta att du av misstag har tagit bort den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="82d4a-159">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="82d4a-160">Detta är något du lätt kan återställa från.</span><span class="sxs-lookup"><span data-stu-id="82d4a-160">This is something you cannot easily recover from.</span></span> <span data-ttu-id="82d4a-161">Azure MySQL-databas kan du gå tillbaka till valfri punkt i tiden i den senaste upp till 35 dagar och en återställningspunkt i tid till en ny server.</span><span class="sxs-lookup"><span data-stu-id="82d4a-161">Azure Database for MySQL allows you to go back to any point in time in the last up to 35 days and restore this point in time to a new server.</span></span> <span data-ttu-id="82d4a-162">Du kan använda den här nya servern för att återställa dina data.</span><span class="sxs-lookup"><span data-stu-id="82d4a-162">You can use this new server to recover your deleted data.</span></span> <span data-ttu-id="82d4a-163">Följande steg återställa exempelserver till en innan tabellen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="82d4a-163">The following steps restore the sample server to a point before the table was added.</span></span>

<span data-ttu-id="82d4a-164">För återställningen måste du följande information:</span><span class="sxs-lookup"><span data-stu-id="82d4a-164">For the Restore you need the following information:</span></span>

- <span data-ttu-id="82d4a-165">Återställningspunkt: Välj en i tidpunkt som inträffar innan servern har ändrats.</span><span class="sxs-lookup"><span data-stu-id="82d4a-165">Restore point: Select a point-in-time that occurs before the server was changed.</span></span> <span data-ttu-id="82d4a-166">Måste vara större än eller lika med värdet i källdatabasen äldsta säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="82d4a-166">Must be greater than or equal to the source database's Oldest backup value.</span></span>
- <span data-ttu-id="82d4a-167">Målservern: Ange ett nytt servernamn som du vill återställa till</span><span class="sxs-lookup"><span data-stu-id="82d4a-167">Target server: Provide a new server name you want to restore to</span></span>
- <span data-ttu-id="82d4a-168">Källservern: Ange namnet på den server som du vill återställa från</span><span class="sxs-lookup"><span data-stu-id="82d4a-168">Source server: Provide the name of the server you want to restore from</span></span>
- <span data-ttu-id="82d4a-169">Plats: Du kan inte välja regionen, som standard är det samma som källservern</span><span class="sxs-lookup"><span data-stu-id="82d4a-169">Location: You cannot select the region, by default it is same as the source server</span></span>

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

<span data-ttu-id="82d4a-170">Att återställa servern och [återställa till point-in-time](./howto-restore-server-portal.md) innan tabellen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="82d4a-170">To restore the server and [restore to a point-in-time](./howto-restore-server-portal.md) before the table was deleted.</span></span> <span data-ttu-id="82d4a-171">Återställa en server till en annan tidpunkt skapar en dubblett ny server som den ursprungliga servern från och med punkten tidpunkt du anger under förutsättning att det är inom kvarhållningsperioden för din [tjänstnivån](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="82d4a-171">Restoring a server to a different point in time creates a duplicate new server as the original server as of the point in time you specify, provided that it is within the retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="82d4a-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="82d4a-172">Next Steps</span></span>
<span data-ttu-id="82d4a-173">I den här kursen har du lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="82d4a-173">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="82d4a-174">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="82d4a-174">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="82d4a-175">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="82d4a-175">Configure the server firewall</span></span>
> * <span data-ttu-id="82d4a-176">Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="82d4a-176">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) to create a database</span></span>
> * <span data-ttu-id="82d4a-177">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="82d4a-177">Load sample data</span></span>
> * <span data-ttu-id="82d4a-178">Frågedata</span><span class="sxs-lookup"><span data-stu-id="82d4a-178">Query data</span></span>
> * <span data-ttu-id="82d4a-179">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="82d4a-179">Update data</span></span>
> * <span data-ttu-id="82d4a-180">Återställa data</span><span class="sxs-lookup"><span data-stu-id="82d4a-180">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="82d4a-181">Azure-databas för MySQL - Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="82d4a-181">Azure Database for MySQL - Azure CLI samples</span></span>](./sample-scripts-azure-cli.md)
