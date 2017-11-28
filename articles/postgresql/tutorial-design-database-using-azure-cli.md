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
# <a name="design-your-first-azure-database-for-postgresql-using-azure-cli"></a><span data-ttu-id="329f5-103">Utforma din första Azure-databas för PostgreSQL med Azure CLI</span><span class="sxs-lookup"><span data-stu-id="329f5-103">Design your first Azure Database for PostgreSQL using Azure CLI</span></span> 
<span data-ttu-id="329f5-104">I kursen får du använder Azure CLI (command-line-interface) och andra verktyg toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="329f5-104">In this tutorial, you use Azure CLI (command-line interface) and other utilities toolearn how to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="329f5-105">Skapa en Azure Database för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="329f5-105">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="329f5-106">Konfigurera hello serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="329f5-106">Configure hello server firewall</span></span>
> * <span data-ttu-id="329f5-107">Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) toocreate verktyget en databas</span><span class="sxs-lookup"><span data-stu-id="329f5-107">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="329f5-108">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="329f5-108">Load sample data</span></span>
> * <span data-ttu-id="329f5-109">Frågedata</span><span class="sxs-lookup"><span data-stu-id="329f5-109">Query data</span></span>
> * <span data-ttu-id="329f5-110">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="329f5-110">Update data</span></span>
> * <span data-ttu-id="329f5-111">Återställa data</span><span class="sxs-lookup"><span data-stu-id="329f5-111">Restore data</span></span>

<span data-ttu-id="329f5-112">Du kan använda hello Azure Cloud Shell i hello webbläsare eller [installera Azure CLI 2.0]( /cli/azure/install-azure-cli) på din egen dator toorun hello kodblock i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="329f5-112">You may use hello Azure Cloud Shell in hello browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer toorun hello code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="329f5-113">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="329f5-113">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="329f5-114">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="329f5-114">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="329f5-115">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="329f5-115">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="329f5-116">Om du har flera prenumerationer, Välj hello lämpliga prenumeration där hello resursen finns eller faktureras för.</span><span class="sxs-lookup"><span data-stu-id="329f5-116">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="329f5-117">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="329f5-117">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="329f5-118">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="329f5-118">Create a resource group</span></span>
<span data-ttu-id="329f5-119">Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="329f5-119">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="329f5-120">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="329f5-120">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="329f5-121">hello följande exempel skapar en resursgrupp med namnet `myresourcegroup` i hello `westus` plats.</span><span class="sxs-lookup"><span data-stu-id="329f5-121">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="329f5-122">Skapa en Azure Database för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="329f5-122">Create an Azure Database for PostgreSQL server</span></span>
<span data-ttu-id="329f5-123">Skapa en [Azure-databas för PostgreSQL server](overview.md) med hello [az postgres servern skapa](/cli/azure/postgres/server#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="329f5-123">Create an [Azure Database for PostgreSQL server](overview.md) using hello [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="329f5-124">En server innehåller en grupp med databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="329f5-124">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="329f5-125">hello följande exempel skapas en server med namnet `mypgserver-20170401` i resursgruppen `myresourcegroup` med inloggning för serveradministratör `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="329f5-125">hello following example creates a server called `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="329f5-126">Namnet på en server mappar tooDNS namn och därför krävs toobe globalt unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="329f5-126">Name of a server maps tooDNS name and is thus required toobe globally unique in Azure.</span></span> <span data-ttu-id="329f5-127">Ersätt hello `<server_admin_password>` med ett eget värde.</span><span class="sxs-lookup"><span data-stu-id="329f5-127">Substitute hello `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401 --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="329f5-128">hello server admin inloggningsnamn och lösenord som du anger här är nödvändig toolog i toohello server och databaserna senare i den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="329f5-128">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="329f5-129">Kom ihåg eller skriv ned den här informationen så att du kan använda den senare.</span><span class="sxs-lookup"><span data-stu-id="329f5-129">Remember or record this information for later use.</span></span>

<span data-ttu-id="329f5-130">Som standard skapas **postgres**-databasen under din server.</span><span class="sxs-lookup"><span data-stu-id="329f5-130">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="329f5-131">Hej [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databasen är en standarddatabas som är avsedd för användning av användare, verktyg och program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="329f5-131">hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="329f5-132">Konfigurera en brandväggsregel på servernivå</span><span class="sxs-lookup"><span data-stu-id="329f5-132">Configure a server-level firewall rule</span></span>

<span data-ttu-id="329f5-133">Skapa en Azure PostgreSQL servernivå brandväggsregel med hello [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="329f5-133">Create an Azure PostgreSQL server-level firewall rule with hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="329f5-134">En brandväggsregel på servernivå som tillåter ett externt program [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) eller [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server via hello Azure PostgreSQL tjänsten brandvägg.</span><span class="sxs-lookup"><span data-stu-id="329f5-134">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server through hello Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="329f5-135">Du kan ange en brandväggsregel som omfattar en IP-intervallet toobe kan tooconnect från nätverket.</span><span class="sxs-lookup"><span data-stu-id="329f5-135">You can set a firewall rule that covers an IP range toobe able tooconnect from your network.</span></span> <span data-ttu-id="329f5-136">hello följande exempel används [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) toocreate en brandväggsregel `AllowAllIps` för en IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="329f5-136">hello following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) toocreate a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="329f5-137">tooopen alla IP-adresser, använda 0.0.0.0 som hello startar IP-adress och 255.255.255.255 som hello slutadress.</span><span class="sxs-lookup"><span data-stu-id="329f5-137">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="329f5-138">Azure PostgreSQL-servern kommunicerar via port 5432.</span><span class="sxs-lookup"><span data-stu-id="329f5-138">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="329f5-139">När du ansluter innifrån ett företagsnätverk är det möjligt att utgående trafik via port 5432 inte tillåts av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="329f5-139">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="329f5-140">Har din IT-avdelning öppna port 5432 tooconnect tooyour Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="329f5-140">Have your IT department open port 5432 tooconnect tooyour Azure SQL Database server.</span></span>
>

## <a name="get-hello-connection-information"></a><span data-ttu-id="329f5-141">Hämta hello anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="329f5-141">Get hello connection information</span></span>

<span data-ttu-id="329f5-142">tooconnect tooyour server behöver du tooprovide värden information och åtkomst-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="329f5-142">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="329f5-143">hello resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="329f5-143">hello result is in JSON format.</span></span> <span data-ttu-id="329f5-144">Anteckna hello **administratorLogin** och **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="329f5-144">Make a note of hello **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-tooazure-database-for-postgresql-database-using-psql"></a><span data-ttu-id="329f5-145">Ansluta tooAzure databas för PostgreSQL-databas med hjälp av psql</span><span class="sxs-lookup"><span data-stu-id="329f5-145">Connect tooAzure Database for PostgreSQL database using psql</span></span>
<span data-ttu-id="329f5-146">Om klientdatorn har PostgreSQL installerat, kan du använda en lokal instans av [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), eller hello Azure Cloud-konsolen tooconnect tooan Azure PostgreSQL server.</span><span class="sxs-lookup"><span data-stu-id="329f5-146">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html), or hello Azure Cloud Console tooconnect tooan Azure PostgreSQL server.</span></span> <span data-ttu-id="329f5-147">Vi använder nu hello psql kommandoradsverktyget tooconnect toohello Azure-databas för PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="329f5-147">Let's now use hello psql command-line utility tooconnect toohello Azure Database for PostgreSQL server.</span></span>

1. <span data-ttu-id="329f5-148">Kör hello följande psql kommandot tooconnect tooan Azure-databas för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="329f5-148">Run hello following psql command tooconnect tooan Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="329f5-149">Till exempel följande kommando hello ansluter toohello standarddatabasen kallas **postgres** på servern PostgreSQL **mypgserver 20170401.postgres.database.azure.com** hjälp av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="329f5-149">For example, hello following command connects toohello default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="329f5-150">Ange hello `<server_admin_password>` du valde när du uppmanas att ange lösenord.</span><span class="sxs-lookup"><span data-stu-id="329f5-150">Enter hello `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 ---dbname=postgres
```

2.  <span data-ttu-id="329f5-151">När du är ansluten toohello server kan du skapa en tom databas hello i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="329f5-151">Once you are connected toohello server, create a blank database at hello prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="329f5-152">I Kommandotolken hello köra hello efter kommandot tooswitch toohello nyskapad databas **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="329f5-152">At hello prompt, execute hello following command tooswitch connection toohello newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="329f5-153">Skapa tabeller i hello-databas</span><span class="sxs-lookup"><span data-stu-id="329f5-153">Create tables in hello database</span></span>
<span data-ttu-id="329f5-154">Nu när du vet hur tooconnect toohello Azure-databas för PostgreSQL vi kan gå igenom hur toocomplete vissa grundläggande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="329f5-154">Now that you know how tooconnect toohello Azure Database for PostgreSQL, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="329f5-155">Vi kan först skapa en tabell och läsa in den med vissa data.</span><span class="sxs-lookup"><span data-stu-id="329f5-155">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="329f5-156">Nu ska vi skapa en tabell som spårar inventeringsinformation.</span><span class="sxs-lookup"><span data-stu-id="329f5-156">Let's create a table that tracks inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

<span data-ttu-id="329f5-157">Du kan se hello nyligen skapade tabellen i hello listan över tabeller nu genom att skriva:</span><span class="sxs-lookup"><span data-stu-id="329f5-157">You can see hello newly created table in hello list of tables now by typing:</span></span>
```sql
\dt
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="329f5-158">Läs in data till hello tabeller</span><span class="sxs-lookup"><span data-stu-id="329f5-158">Load data into hello tables</span></span>
<span data-ttu-id="329f5-159">Nu när vi har en tabell kan vi infoga vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="329f5-159">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="329f5-160">Kör följande fråga tooinsert hello vissa rader med data vid hello öppna Kommandotolkens fönster</span><span class="sxs-lookup"><span data-stu-id="329f5-160">At hello open command prompt window, run hello following query tooinsert some rows of data</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="329f5-161">Du har nu två rader med exempeldata till hello-tabell som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="329f5-161">You have now two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="329f5-162">Fråga efter och uppdatera hello data i hello tabeller</span><span class="sxs-lookup"><span data-stu-id="329f5-162">Query and update hello data in hello tables</span></span>
<span data-ttu-id="329f5-163">Kör följande fråga tooretrieve information från hello databastabell hello.</span><span class="sxs-lookup"><span data-stu-id="329f5-163">Execute hello following query tooretrieve information from hello database table.</span></span> 
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="329f5-164">Du kan också uppdatera hello data i hello tabeller</span><span class="sxs-lookup"><span data-stu-id="329f5-164">You can also update hello data in hello tables</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="329f5-165">hello rad uppdateras i enlighet med detta när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="329f5-165">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="329f5-166">Återställa en databas tooa tidigare punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="329f5-166">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="329f5-167">Anta att du av misstag har tagit bort en tabell.</span><span class="sxs-lookup"><span data-stu-id="329f5-167">Imagine you have accidentally deleted a table.</span></span> <span data-ttu-id="329f5-168">Detta är något du lätt kan återställa från.</span><span class="sxs-lookup"><span data-stu-id="329f5-168">This is something you cannot easily recover from.</span></span> <span data-ttu-id="329f5-169">Azure PostgreSQL-databas kan du toogo tillbaka tooany i tidpunkt (i hello senast too7 dagar (grundläggande) och 35 dagar (Standard)) och återställa den här nya tooa point-in-time-servern.</span><span class="sxs-lookup"><span data-stu-id="329f5-169">Azure Database for PostgreSQL allows you toogo back tooany point-in-time (in hello last up too7 days (Basic) and 35 days (Standard)) and restore this point-in-time tooa new server.</span></span> <span data-ttu-id="329f5-170">Du kan använda den här nya servern toorecover dina data.</span><span class="sxs-lookup"><span data-stu-id="329f5-170">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="329f5-171">hello följande steg hello exempel server tooa återställningspunkt innan hello tabell har lagts till.</span><span class="sxs-lookup"><span data-stu-id="329f5-171">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

```azurecli-interactive
az postgres server restore --resource-group myResourceGroup --name mypgserver-restored --restore-point-in-time 2017-04-13T13:59:00Z --source-server mypgserver-20170401
```

<span data-ttu-id="329f5-172">Hej `az postgres server restore` kommandot måste hello följande parametrar:</span><span class="sxs-lookup"><span data-stu-id="329f5-172">hello `az postgres server restore` command needs hello following parameters:</span></span>
| <span data-ttu-id="329f5-173">Inställning</span><span class="sxs-lookup"><span data-stu-id="329f5-173">Setting</span></span> | <span data-ttu-id="329f5-174">Föreslaget värde</span><span class="sxs-lookup"><span data-stu-id="329f5-174">Suggested value</span></span> | <span data-ttu-id="329f5-175">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="329f5-175">Description</span></span>  |
| --- | --- | --- |
| <span data-ttu-id="329f5-176">--resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="329f5-176">--resource-group</span></span> |  <span data-ttu-id="329f5-177">myResourceGroup</span><span class="sxs-lookup"><span data-stu-id="329f5-177">myResourceGroup</span></span> |  <span data-ttu-id="329f5-178">Resursgruppens namn i vilka hello källservern finns.</span><span class="sxs-lookup"><span data-stu-id="329f5-178">The resource group in which hello source server exists.</span></span>  |
| <span data-ttu-id="329f5-179">--namn</span><span class="sxs-lookup"><span data-stu-id="329f5-179">--name</span></span> | <span data-ttu-id="329f5-180">mypgserver återställs</span><span class="sxs-lookup"><span data-stu-id="329f5-180">mypgserver-restored</span></span> | <span data-ttu-id="329f5-181">hello namnet på hello nya server som skapas av hello restore-kommandot.</span><span class="sxs-lookup"><span data-stu-id="329f5-181">hello name of hello new server that is created by hello restore command.</span></span> |
| <span data-ttu-id="329f5-182">Återställ punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="329f5-182">restore-point-in-time</span></span> | <span data-ttu-id="329f5-183">2017-04-13T13:59:00Z</span><span class="sxs-lookup"><span data-stu-id="329f5-183">2017-04-13T13:59:00Z</span></span> | <span data-ttu-id="329f5-184">Välj en tidpunkt i toorestore till.</span><span class="sxs-lookup"><span data-stu-id="329f5-184">Select a point-in-time toorestore to.</span></span> <span data-ttu-id="329f5-185">Datumet och tiden måste vara inom hello källserverns kvarhållningsperiod för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="329f5-185">This date and time must be within hello source server's backup retention period.</span></span> <span data-ttu-id="329f5-186">Använd ISO8601-formatet för datum och tid.</span><span class="sxs-lookup"><span data-stu-id="329f5-186">Use ISO8601 date and time format.</span></span> <span data-ttu-id="329f5-187">Exempelvis kan du använda din egen lokala tidszon som `2017-04-13T05:59:00-08:00`, eller använda UTC Zulu format `2017-04-13T13:59:00Z`.</span><span class="sxs-lookup"><span data-stu-id="329f5-187">For example, you may use your own local timezone, such as `2017-04-13T05:59:00-08:00`, or use UTC Zulu format `2017-04-13T13:59:00Z`.</span></span> |
| <span data-ttu-id="329f5-188">--käll-servern</span><span class="sxs-lookup"><span data-stu-id="329f5-188">--source-server</span></span> | <span data-ttu-id="329f5-189">mypgserver 20170401</span><span class="sxs-lookup"><span data-stu-id="329f5-189">mypgserver-20170401</span></span> | <span data-ttu-id="329f5-190">hello namnet eller ID för hello källa server toorestore från.</span><span class="sxs-lookup"><span data-stu-id="329f5-190">hello name or ID of hello source server toorestore from.</span></span> |

<span data-ttu-id="329f5-191">Återställa en server tooa i tidpunkt skapar en ny server och kopiera som hello ursprungliga server från och med hello punkt tidpunkt du anger.</span><span class="sxs-lookup"><span data-stu-id="329f5-191">Restoring a server tooa point-in-time creates a new server, copying as hello original server as of hello point in time you specify.</span></span> <span data-ttu-id="329f5-192">hello plats och prissättning nivåvärden för hello återställts server är hello samma som hello källservern.</span><span class="sxs-lookup"><span data-stu-id="329f5-192">hello location and pricing tier values for hello restored server are hello same as hello source server.</span></span>

<span data-ttu-id="329f5-193">hello kommandot synkront och returneras när hello server har återställts.</span><span class="sxs-lookup"><span data-stu-id="329f5-193">hello command is synchronous, and will return after hello server is restored.</span></span> <span data-ttu-id="329f5-194">Leta upp hello nya servern som skapades när hello återställning har slutförts.</span><span class="sxs-lookup"><span data-stu-id="329f5-194">Once hello restore finishes, locate hello new server that was created.</span></span> <span data-ttu-id="329f5-195">Kontrollera hello data har återställts som förväntat.</span><span class="sxs-lookup"><span data-stu-id="329f5-195">Verify hello data was restored as expected.</span></span>


## <a name="next-steps"></a><span data-ttu-id="329f5-196">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="329f5-196">Next steps</span></span>
<span data-ttu-id="329f5-197">I kursen får du lärt dig hur toouse Azure CLI (command-line-interface) och andra verktyg för att:</span><span class="sxs-lookup"><span data-stu-id="329f5-197">In this tutorial, you learned how toouse Azure CLI (command-line interface) and other utilities to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="329f5-198">Skapa en Azure Database för PostgreSQL</span><span class="sxs-lookup"><span data-stu-id="329f5-198">Create an Azure Database for PostgreSQL</span></span>
> * <span data-ttu-id="329f5-199">Konfigurera hello serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="329f5-199">Configure hello server firewall</span></span>
> * <span data-ttu-id="329f5-200">Använd [ **psql** ](https://www.postgresql.org/docs/9.6/static/app-psql.html) toocreate verktyget en databas</span><span class="sxs-lookup"><span data-stu-id="329f5-200">Use [**psql**](https://www.postgresql.org/docs/9.6/static/app-psql.html) utility toocreate a database</span></span>
> * <span data-ttu-id="329f5-201">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="329f5-201">Load sample data</span></span>
> * <span data-ttu-id="329f5-202">Frågedata</span><span class="sxs-lookup"><span data-stu-id="329f5-202">Query data</span></span>
> * <span data-ttu-id="329f5-203">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="329f5-203">Update data</span></span>
> * <span data-ttu-id="329f5-204">Återställa data</span><span class="sxs-lookup"><span data-stu-id="329f5-204">Restore data</span></span>

<span data-ttu-id="329f5-205">Därefter lär du dig hur toouse hello Azure portal toodo liknande uppgifter, granska den här självstudiekursen: [utforma din första Azure-databas för PostgreSQL med hello Azure-portalen](tutorial-design-database-using-azure-portal.md)</span><span class="sxs-lookup"><span data-stu-id="329f5-205">Next, learn how toouse hello Azure portal toodo similar tasks, review this tutorial: [Design your first Azure Database for PostgreSQL using hello Azure portal](tutorial-design-database-using-azure-portal.md)</span></span>
