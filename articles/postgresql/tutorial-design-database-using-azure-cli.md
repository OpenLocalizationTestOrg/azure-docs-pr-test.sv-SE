---
title: "Utforma din första Azure-databas för PostgreSQL med Azure CLI | Microsoft Docs"
description: "Den här kursen visar hur du utformar din första Azure-databas för PostgreSQL med Azure CLI."
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
ms.openlocfilehash: cf536fce8925f9173b541b845af25a8d8c38eabd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a><span data-ttu-id="24174-103">Utforma din första Azure-databas för PostgreSQL med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="24174-103">Design your first Azure Database for PostgreSQL using Azure CLI</span></span> 
<span data-ttu-id="24174-104">I kursen får du använder Azure CLI (command-line-interface) och andra verktyg att lära dig hur du:</span><span class="sxs-lookup"><span data-stu-id="24174-104">In this tutorial, you use Azure CLI (command-line interface) and other utilities to learn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="24174-105">Skapa en Azure Database för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="24174-105">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="24174-106">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="24174-106">Configure the server firewall</span></span>
> * <span data-ttu-id="24174-107">Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) verktyg för att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="24174-107">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility to create a database</span></span>
> * <span data-ttu-id="24174-108">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="24174-108">Load sample data</span></span>
> * <span data-ttu-id="24174-109">Frågedata</span><span class="sxs-lookup"><span data-stu-id="24174-109">Query data</span></span>
> * <span data-ttu-id="24174-110">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="24174-110">Update data</span></span>
> * <span data-ttu-id="24174-111">Återställa data</span><span class="sxs-lookup"><span data-stu-id="24174-111">Restore data</span></span>

<span data-ttu-id="24174-112">Du kan använda Azure Cloud Shell i webbläsaren eller [installera Azure CLI 2.0]( /cli/azure/install-azure-cli) på din dator för att köra kodblock i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="24174-112">You may use the Azure Cloud Shell in the browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer to run the code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="24174-113">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="24174-113">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="24174-114">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="24174-114">Run `az --version` to find the version.</span></span> <span data-ttu-id="24174-115">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="24174-115">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="24174-116">Om du har flera prenumerationer väljer du en lämplig prenumerationen där resursen ligger eller faktureras.</span><span class="sxs-lookup"><span data-stu-id="24174-116">If you have multiple subscriptions, choose the appropriate subscription in which the resource exists or is billed for.</span></span> <span data-ttu-id="24174-117">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="24174-117">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="24174-118">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="24174-118">Create a resource group</span></span>
<span data-ttu-id="24174-119">Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="24174-119">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="24174-120">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="24174-120">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="24174-121">I följande exempel skapas en resursgrupp med namnet `myresourcegroup` på platsen `westus`.</span><span class="sxs-lookup"><span data-stu-id="24174-121">The following example creates a resource group named `myresourcegroup` in the `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="24174-122">Skapa en Azure Database för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="24174-122">Create an Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="24174-123">Skapa en [Azure Database för PostgreSQL-server](overview.md) med kommandot [az postgres server create](/cli/azure/postgres/server#create).</span><span class="sxs-lookup"><span data-stu-id="24174-123">Create an [Azure Database for PostgreSQL server](overview.md) using the [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="24174-124">En server innehåller en grupp med databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="24174-124">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="24174-125">I följande exempel skapas en server med namnet `mypgserver-20170401` i resursgruppen `myresourcegroup` med inloggning för serveradministratör `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="24174-125">The following example creates a server called `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="24174-126">Namnet på en server mappar till DNS-namn och därför måste vara globalt unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="24174-126">Name of a server maps to DNS name and is thus required to be globally unique in Azure.</span></span> <span data-ttu-id="24174-127">Ersätt `<server_admin_password>` med ditt eget värde.</span><span class="sxs-lookup"><span data-stu-id="24174-127">Substitute the `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="24174-128">Det användarnamn och lösenord för serveradministration du anger här krävs för inloggning på servern och databaserna senare i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="24174-128">The server admin login and password that you specify here are required to log in to the server and its databases later in this quick start.</span></span> <span data-ttu-id="24174-129">Kom ihåg eller skriv ned den här informationen så att du kan använda den senare.</span><span class="sxs-lookup"><span data-stu-id="24174-129">Remember or record this information for later use.</span></span>

<span data-ttu-id="24174-130">Som standard skapas **postgres**-databasen under din server.</span><span class="sxs-lookup"><span data-stu-id="24174-130">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="24174-131">[Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-databasen är en standarddatabas som är avsedd för användare, verktyg och tredje parts program.</span><span class="sxs-lookup"><span data-stu-id="24174-131">The [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="24174-132">Konfigurera en brandväggsregel på servernivå</span><span class="sxs-lookup"><span data-stu-id="24174-132">Configure a server-level firewall rule</span></span>

<span data-ttu-id="24174-133">Skapa en brandväggsregel på Azure PostgreSQL-servernivå med kommandot [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create).</span><span class="sxs-lookup"><span data-stu-id="24174-133">Create an Azure PostgreSQL server-level firewall rule with the [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="24174-134">En brandväggsregel på servernivå låter ett externt program som en [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) eller en [PgAdmin](https://www.pgadmin.org/) ansluta till din server via Azure PostgreSQL-tjänstens brandvägg.</span><span class="sxs-lookup"><span data-stu-id="24174-134">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) to connect to your server through the Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="24174-135">Du kan ställa in en brandväggsregel som omfattar ett IP-intervall för att kunna ansluta från ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="24174-135">You can set a firewall rule that covers an IP range to be able to connect from your network.</span></span> <span data-ttu-id="24174-136">Följande exempel använder [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) för att skapa en brandväggsregel `AllowAllIps` för ett IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="24174-136">The following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) to create a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="24174-137">Öppna alla IP-adresser genom att använda 0.0.0.0 som den första IP-adressen och 255.255.255.255 som slutadress.</span><span class="sxs-lookup"><span data-stu-id="24174-137">To open all IP addresses, use 0.0.0.0 as the starting IP address and 255.255.255.255 as the ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="24174-138">Azure PostgreSQL-servern kommunicerar via port 5432.</span><span class="sxs-lookup"><span data-stu-id="24174-138">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="24174-139">När du ansluter innifrån ett företagsnätverk är det möjligt att utgående trafik via port 5432 inte tillåts av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="24174-139">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="24174-140">Be din IT-avdelning öppna port 5432 för att ansluta till din Azure SQL-databasserver.</span><span class="sxs-lookup"><span data-stu-id="24174-140">Have your IT department open port 5432 to connect to your Azure SQL Database server.</span></span>
>

## <a name="get-the-connection-information"></a><span data-ttu-id="24174-141">Hämta anslutningsinformationen</span><span class="sxs-lookup"><span data-stu-id="24174-141">Get the connection information</span></span>

<span data-ttu-id="24174-142">För att ansluta till servern måste du ange värddatorinformationen och autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="24174-142">To connect to your server, you need to provide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="24174-143">Resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="24174-143">The result is in JSON format.</span></span> <span data-ttu-id="24174-144">Anteckna **administratorLogin** och **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="24174-144">Make a note of the **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-to-azure-database-for-postgresql-database-using-psql"></a><span data-ttu-id="24174-145">Ansluta till Azure-databas för PostgreSQL-databas med hjälp av psql</span><span class="sxs-lookup"><span data-stu-id="24174-145">Connect to Azure Database for PostgreSQL database using psql</span></span>
<span data-ttu-id="24174-146">Om klientdatorn har PostgreSQL installerat, kan du använda en lokal instans av [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), eller Azure Cloud-konsolen att ansluta till en Azure PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="24174-146">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), or the Azure Cloud Console to connect to an Azure PostgreSQL server.</span></span> <span data-ttu-id="24174-147">Nu använder vi psql-kommandoradsverktyget för att ansluta till Azure Database för PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="24174-147">Let's now use the psql command-line utility to connect to the Azure Database for PostgreSQL server.</span></span>

1. <span data-ttu-id="24174-148">Kör följande psql-kommando för att ansluta till en Azure Database för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="24174-148">Run the following psql command to connect to an Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="24174-149">Följande kommando till exempel, ansluter till standarddatabasen som heter **postgres** på din PostgreSQL-server **mypgserver-20170401.postgres.database.azure.com** med hjälp av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="24174-149">For example, the following command connects to the default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="24174-150">Ange den `<server_admin_password>` som du valde när du uppmanades att ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="24174-150">Enter the `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  <span data-ttu-id="24174-151">När du är ansluten till servern, skapar du en blank databas i prompten.</span><span class="sxs-lookup"><span data-stu-id="24174-151">Once you are connected to the server, create a blank database at the prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="24174-152">I prompten, kör du följande kommando för att växla anslutning till den nyligen skapade databasen **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="24174-152">At the prompt, execute the following command to switch connection to the newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="create-tables-in-the-database"></a><span data-ttu-id="24174-153">Skapa tabeller i databasen</span><span class="sxs-lookup"><span data-stu-id="24174-153">Create tables in the database</span></span>
<span data-ttu-id="24174-154">Nu när du vet hur du ansluter till Azure-databasen för PostgreSQL gå vi igenom hur du utför några grundläggande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="24174-154">Now that you know how to connect to the Azure Database for PostgreSQL, we can go over how to complete some basic tasks.</span></span>

<span data-ttu-id="24174-155">Vi kan först skapa en tabell och läsa in den med vissa data.</span><span class="sxs-lookup"><span data-stu-id="24174-155">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="24174-156">Nu ska vi skapa en tabell som spårar inventeringsinformation.</span><span class="sxs-lookup"><span data-stu-id="24174-156">Let's create a table that tracks inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

<span data-ttu-id="24174-157">Du kan se den nyligen skapade tabellen i listan över tabeller nu genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="24174-157">You can see the newly created table in the list of tables now by typing:</span></span>
```sql
\dt
```

## <a name="load-data-into-the-tables"></a><span data-ttu-id="24174-158">Läser in data i tabeller</span><span class="sxs-lookup"><span data-stu-id="24174-158">Load data into the tables</span></span>
<span data-ttu-id="24174-159">Nu när vi har en tabell kan vi infoga vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="24174-159">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="24174-160">Kör följande fråga för att infoga vissa rader med data i öppna en kommandotolk-fönster</span><span class="sxs-lookup"><span data-stu-id="24174-160">At the open command prompt window, run the following query to insert some rows of data</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="24174-161">Du har nu två rader med exempeldata i tabellen som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="24174-161">You have now two rows of sample data into the table you created earlier.</span></span>

## <a name="query-and-update-the-data-in-the-tables"></a><span data-ttu-id="24174-162">Fråga efter och uppdatera data i tabeller</span><span class="sxs-lookup"><span data-stu-id="24174-162">Query and update the data in the tables</span></span>
<span data-ttu-id="24174-163">Kör följande fråga för att hämta information från databastabellen.</span><span class="sxs-lookup"><span data-stu-id="24174-163">Execute the following query to retrieve information from the database table.</span></span> 
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="24174-164">Du kan också uppdatera data i tabeller</span><span class="sxs-lookup"><span data-stu-id="24174-164">You can also update the data in the tables</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="24174-165">Raden uppdateras i enlighet med detta när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="24174-165">The row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-to-a-previous-point-in-time"></a><span data-ttu-id="24174-166">Återställa en databas till en tidigare tidpunkt</span><span class="sxs-lookup"><span data-stu-id="24174-166">Restore a database to a previous point in time</span></span>
<span data-ttu-id="24174-167">Anta att du av misstag har tagit bort en tabell.</span><span class="sxs-lookup"><span data-stu-id="24174-167">Imagine you have accidentally deleted a table.</span></span> <span data-ttu-id="24174-168">Detta är något du lätt kan återställa från.</span><span class="sxs-lookup"><span data-stu-id="24174-168">This is something you cannot easily recover from.</span></span> <span data-ttu-id="24174-169">Azure-databas för PostgreSQL kan du gå tillbaka till någon punkt i tid (i det senaste upp till 7 dagar (grundläggande) och 35 dagar (Standard)) och återställa den här i tidpunkt till en ny server.</span><span class="sxs-lookup"><span data-stu-id="24174-169">Azure Database for PostgreSQL allows you to go back to any point-in-time (in the last up to 7 days (Basic) and 35 days (Standard)) and restore this point-in-time to a new server.</span></span> <span data-ttu-id="24174-170">Du kan använda den här nya servern för att återställa dina data.</span><span class="sxs-lookup"><span data-stu-id="24174-170">You can use this new server to recover your deleted data.</span></span> <span data-ttu-id="24174-171">Följande steg återställa exempelserver till en innan tabellen har lagts till.</span><span class="sxs-lookup"><span data-stu-id="24174-171">The following steps restore the sample server to a point before the table was added.</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="24174-172">Den `az postgres server restore` kommandot måste följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="24174-172">The `az postgres server restore` command needs the following parameters:</span></span>
| <span data-ttu-id="24174-173">Inställning</span><span class="sxs-lookup"><span data-stu-id="24174-173">Setting</span></span> | <span data-ttu-id="24174-174">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="24174-174">Suggested value</span></span> | <span data-ttu-id="24174-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="24174-175">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="24174-176">--resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="24174-176">--resource-group</span></span> |  <span data-ttu-id="24174-177">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="24174-177">myResourceGroup</span></span> |  <span data-ttu-id="24174-178">Resursgruppens namn som källservern finns.</span><span class="sxs-lookup"><span data-stu-id="24174-178">The resource group in which the source server exists.</span></span>  |
| <span data-ttu-id="24174-179">--namn</span><span class="sxs-lookup"><span data-stu-id="24174-179">--name</span></span> | <span data-ttu-id="24174-180">mypgserver återställs</span><span class="sxs-lookup"><span data-stu-id="24174-180">mypgserver-restored</span></span> | <span data-ttu-id="24174-181">Namnet på den nya servern som skapas med kommandot restore.</span><span class="sxs-lookup"><span data-stu-id="24174-181">The name of the new server that is created by the restore command.</span></span> |
| <span data-ttu-id="24174-182">Återställ punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="24174-182">restore-point-in-time</span></span> | <span data-ttu-id="24174-183">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="24174-183">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="24174-184">Välj en punkt i tid att återställa till.</span><span class="sxs-lookup"><span data-stu-id="24174-184">Select a point-in-time to restore to.</span></span> <span data-ttu-id="24174-185">Datumet och tiden måste vara inom källserverns kvarhållningsperiod för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="24174-185">This date and time must be within the source server's backup retention period.</span></span> <span data-ttu-id="24174-186">Använd ISO8601-formatet för datum och tid.</span><span class="sxs-lookup"><span data-stu-id="24174-186">Use ISO8601 date and time format.</span></span> <span data-ttu-id="24174-187">Exempelvis kan du använda din egen lokala tidszon som `2017-04-13T05:59:00-08:00`, eller använda UTC Zulu format `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="24174-187">For example, you may use your own local timezone, such as `2017-04-13T05:59:00-08:00`, or use UTC Zulu format `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="24174-188">--käll-servern</span><span class="sxs-lookup"><span data-stu-id="24174-188">--source-server</span></span> | <span data-ttu-id="24174-189">mypgserver 20170401</span><span class="sxs-lookup"><span data-stu-id="24174-189">mypgserver-20170401</span></span> | <span data-ttu-id="24174-190">Namnet eller ID på källservern för att återställa från.</span><span class="sxs-lookup"><span data-stu-id="24174-190">The name or ID of the source server to restore from.</span></span> |

<span data-ttu-id="24174-191">Återställa en server till en i tidpunkt skapar en ny server och kopiera som den ursprungliga servern från och med punkten tidpunkt du anger.</span><span class="sxs-lookup"><span data-stu-id="24174-191">Restoring a server to a point-in-time creates a new server, copying as the original server as of the point in time you specify.</span></span> <span data-ttu-id="24174-192">Plats och prisnivå nivåvärden för den återställda servern är samma som källservern.</span><span class="sxs-lookup"><span data-stu-id="24174-192">The location and pricing tier values for the restored server are the same as the source server.</span></span>

<span data-ttu-id="24174-193">Kommandot är synkron och kommer tillbaka när servern har återställts.</span><span class="sxs-lookup"><span data-stu-id="24174-193">The command is synchronous, and will return after the server is restored.</span></span> <span data-ttu-id="24174-194">Leta upp den nya servern som skapades när återställningen är klar.</span><span class="sxs-lookup"><span data-stu-id="24174-194">Once the restore finishes, locate the new server that was created.</span></span> <span data-ttu-id="24174-195">Kontrollera att data har återställts som förväntat.</span><span class="sxs-lookup"><span data-stu-id="24174-195">Verify the data was restored as expected.</span></span>


## <a name="next-steps"></a><span data-ttu-id="24174-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="24174-196">Next steps</span></span>
<span data-ttu-id="24174-197">I kursen får du har lärt dig hur du använder Azure CLI (command-line-interface) och andra verktyg för att:</span><span class="sxs-lookup"><span data-stu-id="24174-197">In this tutorial, you learned how to use Azure CLI (command-line interface) and other utilities to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="24174-198">Skapa en Azure Database för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="24174-198">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="24174-199">Konfigurera server-brandväggen</span><span class="sxs-lookup"><span data-stu-id="24174-199">Configure the server firewall</span></span>
> * <span data-ttu-id="24174-200">Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) verktyg för att skapa en databas</span><span class="sxs-lookup"><span data-stu-id="24174-200">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility to create a database</span></span>
> * <span data-ttu-id="24174-201">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="24174-201">Load sample data</span></span>
> * <span data-ttu-id="24174-202">Frågedata</span><span class="sxs-lookup"><span data-stu-id="24174-202">Query data</span></span>
> * <span data-ttu-id="24174-203">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="24174-203">Update data</span></span>
> * <span data-ttu-id="24174-204">Återställa data</span><span class="sxs-lookup"><span data-stu-id="24174-204">Restore data</span></span>

<span data-ttu-id="24174-205">Därefter beskrivs hur du använder Azure portal gör liknande uppgifter genom att granska den här självstudiekursen: [utforma din första Azure-databas för PostgreSQL med Azure-portalen](tutorial-design-database-using-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="24174-205">Next, learn how to use the Azure portal to do similar tasks, review this tutorial: [Design your first Azure Database for PostgreSQL using the Azure portal](tutorial-design-database-using-azure-portal.md)</span></span>
