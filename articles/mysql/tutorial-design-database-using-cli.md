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
# <a name="design-your-first-azure-database-for-mysql-database"></a><span data-ttu-id="a073e-103">Utforma din första Azure-databas för MySQL-databas</span><span class="sxs-lookup"><span data-stu-id="a073e-103">Design your first Azure Database for MySQL database</span></span>

<span data-ttu-id="a073e-104">Azure MySQL-databas är en relationsdatabastjänst i hello Microsoft cloud baserat på MySQL Community Edition databasmotorn.</span><span class="sxs-lookup"><span data-stu-id="a073e-104">Azure Database for MySQL is a relational database service in hello Microsoft cloud based on MySQL Community Edition database engine.</span></span> <span data-ttu-id="a073e-105">I kursen får du använder Azure CLI (command-line-interface) och andra verktyg toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="a073e-105">In this tutorial, you use Azure CLI (command-line interface) and other utilities toolearn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="a073e-106">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="a073e-106">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="a073e-107">Konfigurera hello serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="a073e-107">Configure hello server firewall</span></span>
> * <span data-ttu-id="a073e-108">Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate en databas</span><span class="sxs-lookup"><span data-stu-id="a073e-108">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate a database</span></span>
> * <span data-ttu-id="a073e-109">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="a073e-109">Load sample data</span></span>
> * <span data-ttu-id="a073e-110">Frågedata</span><span class="sxs-lookup"><span data-stu-id="a073e-110">Query data</span></span>
> * <span data-ttu-id="a073e-111">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="a073e-111">Update data</span></span>
> * <span data-ttu-id="a073e-112">Återställa data</span><span class="sxs-lookup"><span data-stu-id="a073e-112">Restore data</span></span>

<span data-ttu-id="a073e-113">Du kan använda hello Azure Cloud Shell i hello webbläsare eller [installera Azure CLI 2.0]( /cli/azure/install-azure-cli) på din egen dator toorun hello kodblock i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="a073e-113">You may use hello Azure Cloud Shell in hello browser, or [Install Azure CLI 2.0]( /cli/azure/install-azure-cli) on your own computer toorun hello code blocks in this tutorial.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="a073e-114">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="a073e-114">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="a073e-115">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="a073e-115">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="a073e-116">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="a073e-116">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="a073e-117">Om du har flera prenumerationer, Välj hello lämpliga prenumeration där hello resursen finns eller faktureras för.</span><span class="sxs-lookup"><span data-stu-id="a073e-117">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource exists or is billed for.</span></span> <span data-ttu-id="a073e-118">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="a073e-118">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="a073e-119">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="a073e-119">Create a resource group</span></span>
<span data-ttu-id="a073e-120">Skapa en [Azure-resursgrupp](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) med [az gruppen skapa](https://docs.microsoft.com/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="a073e-120">Create an [Azure resource group](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-overview) with [az group create](https://docs.microsoft.com/cli/azure/group#create) command.</span></span> <span data-ttu-id="a073e-121">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="a073e-121">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span>

<span data-ttu-id="a073e-122">hello följande exempel skapar en resursgrupp med namnet `mycliresource` i hello `westus` plats.</span><span class="sxs-lookup"><span data-stu-id="a073e-122">hello following example creates a resource group named `mycliresource` in hello `westus` location.</span></span>

```azurecli-interactive
az group create --name mycliresource --location westus
```

## <a name="create-an-azure-database-for-mysql-server"></a><span data-ttu-id="a073e-123">Skapa en Azure Database för MySQL-server</span><span class="sxs-lookup"><span data-stu-id="a073e-123">Create an Azure Database for MySQL server</span></span>
<span data-ttu-id="a073e-124">Skapa en Azure-databas för MySQL-server med hello az mysql-servern skapa kommando.</span><span class="sxs-lookup"><span data-stu-id="a073e-124">Create an Azure Database for MySQL server with hello az mysql server create command.</span></span> <span data-ttu-id="a073e-125">En server kan hantera flera databaser.</span><span class="sxs-lookup"><span data-stu-id="a073e-125">A server can manage multiple databases.</span></span> <span data-ttu-id="a073e-126">Normalt används en separat databas för varje projekt eller för varje användare.</span><span class="sxs-lookup"><span data-stu-id="a073e-126">Typically, a separate database is used for each project or for each user.</span></span>

<span data-ttu-id="a073e-127">hello följande exempel skapas en Azure-databas för MySQL-server finns i `westus` i hello resursgruppen `mycliresource` med namnet `mycliserver`.</span><span class="sxs-lookup"><span data-stu-id="a073e-127">hello following example creates an Azure Database for MySQL server located in `westus` in hello resource group `mycliresource` with name `mycliserver`.</span></span> <span data-ttu-id="a073e-128">hello-servern har en administratörsinloggning i namnet `myadmin` och lösenord `Password01!`.</span><span class="sxs-lookup"><span data-stu-id="a073e-128">hello server has an administrator log in named `myadmin` and password `Password01!`.</span></span> <span data-ttu-id="a073e-129">hello server skapas med **grundläggande** prestandanivån och **50** compute enheter som delas mellan alla hello databaser i hello-server.</span><span class="sxs-lookup"><span data-stu-id="a073e-129">hello server is created with **Basic** performance tier and **50** compute units shared between all hello databases in hello server.</span></span> <span data-ttu-id="a073e-130">Du kan skala beräkning och lagring uppåt eller nedåt beroende på hello programbehov.</span><span class="sxs-lookup"><span data-stu-id="a073e-130">You can scale compute and storage up or down depending on hello application needs.</span></span>

```azurecli-interactive
az mysql server create --resource-group mycliresource --name mycliserver
--location westus --user myadmin --password Password01!
--performance-tier Basic --compute-units 50
```

## <a name="configure-firewall-rule"></a><span data-ttu-id="a073e-131">Konfigurera brandväggsregeln</span><span class="sxs-lookup"><span data-stu-id="a073e-131">Configure firewall rule</span></span>
<span data-ttu-id="a073e-132">Skapa en Azure-databas för MySQL servernivå brandväggsregel med hello az mysql-brandväggsregel skapa kommando.</span><span class="sxs-lookup"><span data-stu-id="a073e-132">Create an Azure Database for MySQL server-level firewall rule with hello az mysql server firewall-rule create command.</span></span> <span data-ttu-id="a073e-133">En brandväggsregel på servernivå som tillåter ett externt program **mysql** kommandoradsverktyget eller MySQL arbetsstationen tooconnect tooyour server via hello Azure MySQL service brandväggen.</span><span class="sxs-lookup"><span data-stu-id="a073e-133">A server-level firewall rule allows an external application, such as **mysql** command-line tool or MySQL Workbench tooconnect tooyour server through hello Azure MySQL service firewall.</span></span> 

<span data-ttu-id="a073e-134">hello skapas följande exempel en brandväggsregel för en fördefinierad adressintervallet.</span><span class="sxs-lookup"><span data-stu-id="a073e-134">hello following example creates a firewall rule for a predefined address range.</span></span> <span data-ttu-id="a073e-135">Det här exemplet visar hello hela möjliga IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="a073e-135">This example shows hello entire possible range of IP addresses.</span></span>

```azurecli-interactive
az mysql server firewall-rule create --resource-group mycliresource --server mycliserver --name AllowYourIP --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

## <a name="get-hello-connection-information"></a><span data-ttu-id="a073e-136">Hämta hello anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="a073e-136">Get hello connection information</span></span>

<span data-ttu-id="a073e-137">tooconnect tooyour server behöver du tooprovide värden information och åtkomst-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="a073e-137">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az mysql server show --resource-group mycliresource --name mycliserver
```

<span data-ttu-id="a073e-138">hello resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="a073e-138">hello result is in JSON format.</span></span> <span data-ttu-id="a073e-139">Anteckna hello **fullyQualifiedDomainName** och **administratorLogin**.</span><span class="sxs-lookup"><span data-stu-id="a073e-139">Make a note of hello **fullyQualifiedDomainName** and **administratorLogin**.</span></span>
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

## <a name="connect-toohello-server-using-mysql"></a><span data-ttu-id="a073e-140">Ansluta toohello servern med hjälp av mysql</span><span class="sxs-lookup"><span data-stu-id="a073e-140">Connect toohello server using mysql</span></span>
<span data-ttu-id="a073e-141">Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish anslutning tooyour Azure-databas för MySQL-servern.</span><span class="sxs-lookup"><span data-stu-id="a073e-141">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) tooestablish a connection tooyour Azure Database for MySQL server.</span></span> <span data-ttu-id="a073e-142">I det här exemplet är hello-kommandot:</span><span class="sxs-lookup"><span data-stu-id="a073e-142">In this example, hello command is:</span></span>
```cmd
mysql -h mycliserver.database.windows.net -u myadmin@mycliserver -p
```

## <a name="create-a-blank-database"></a><span data-ttu-id="a073e-143">Skapa en tom databas</span><span class="sxs-lookup"><span data-stu-id="a073e-143">Create a blank database</span></span>
<span data-ttu-id="a073e-144">När du är ansluten toohello server kan du skapa en tom databas.</span><span class="sxs-lookup"><span data-stu-id="a073e-144">Once you’re connected toohello server, create a blank database.</span></span>
```sql
mysql> CREATE DATABASE mysampledb;
```

<span data-ttu-id="a073e-145">Kör hello efter kommandot tooswitch hello toothis nyskapad databas hello Kommandotolken:</span><span class="sxs-lookup"><span data-stu-id="a073e-145">At hello prompt, run hello following command tooswitch hello connection toothis newly created database:</span></span>
```sql
mysql> USE mysampledb;
```

## <a name="create-tables-in-hello-database"></a><span data-ttu-id="a073e-146">Skapa tabeller i hello-databas</span><span class="sxs-lookup"><span data-stu-id="a073e-146">Create tables in hello database</span></span>
<span data-ttu-id="a073e-147">Nu när du vet hur tooconnect toohello Azure-databas för MySQL-databas, det kan gå igenom hur toocomplete vissa grundläggande uppgifter.</span><span class="sxs-lookup"><span data-stu-id="a073e-147">Now that you know how tooconnect toohello Azure Database for MySQL database, we can go over how toocomplete some basic tasks.</span></span>

<span data-ttu-id="a073e-148">Vi kan först skapa en tabell och läsa in den med vissa data.</span><span class="sxs-lookup"><span data-stu-id="a073e-148">First, we can create a table and load it with some data.</span></span> <span data-ttu-id="a073e-149">Nu ska vi skapa en tabell som innehåller information om maskinvaruinventering.</span><span class="sxs-lookup"><span data-stu-id="a073e-149">Let's create a table that stores inventory information.</span></span>
```sql
CREATE TABLE inventory (
    id serial PRIMARY KEY, 
    name VARCHAR(50), 
    quantity INTEGER
);
```

## <a name="load-data-into-hello-tables"></a><span data-ttu-id="a073e-150">Läs in data till hello tabeller</span><span class="sxs-lookup"><span data-stu-id="a073e-150">Load data into hello tables</span></span>
<span data-ttu-id="a073e-151">Nu när vi har en tabell kan vi infoga vissa data i den.</span><span class="sxs-lookup"><span data-stu-id="a073e-151">Now that we have a table, we can insert some data into it.</span></span> <span data-ttu-id="a073e-152">Kör följande fråga tooinsert hello vissa rader med data vid hello öppna Kommandotolkens fönster.</span><span class="sxs-lookup"><span data-stu-id="a073e-152">At hello open command prompt window, run hello following query tooinsert some rows of data.</span></span>
```sql
INSERT INTO inventory (id, name, quantity) VALUES (1, 'banana', 150); 
INSERT INTO inventory (id, name, quantity) VALUES (2, 'orange', 154);
```

<span data-ttu-id="a073e-153">Nu har du två rader med exempeldata till hello-tabell som du skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="a073e-153">Now you have two rows of sample data into hello table you created earlier.</span></span>

## <a name="query-and-update-hello-data-in-hello-tables"></a><span data-ttu-id="a073e-154">Fråga efter och uppdatera hello data i hello tabeller</span><span class="sxs-lookup"><span data-stu-id="a073e-154">Query and update hello data in hello tables</span></span>
<span data-ttu-id="a073e-155">Kör följande fråga tooretrieve information från hello databastabell hello.</span><span class="sxs-lookup"><span data-stu-id="a073e-155">Execute hello following query tooretrieve information from hello database table.</span></span>
```sql
SELECT * FROM inventory;
```

<span data-ttu-id="a073e-156">Du kan också uppdatera hello data i hello tabeller.</span><span class="sxs-lookup"><span data-stu-id="a073e-156">You can also update hello data in hello tables.</span></span>
```sql
UPDATE inventory SET quantity = 200 WHERE name = 'banana';
```

<span data-ttu-id="a073e-157">hello rad uppdateras i enlighet med detta när du hämtar data.</span><span class="sxs-lookup"><span data-stu-id="a073e-157">hello row gets updated accordingly when you retrieve data.</span></span>
```sql
SELECT * FROM inventory;
```

## <a name="restore-a-database-tooa-previous-point-in-time"></a><span data-ttu-id="a073e-158">Återställa en databas tooa tidigare punkt i tiden</span><span class="sxs-lookup"><span data-stu-id="a073e-158">Restore a database tooa previous point in time</span></span>
<span data-ttu-id="a073e-159">Anta att du av misstag har tagit bort den här tabellen.</span><span class="sxs-lookup"><span data-stu-id="a073e-159">Imagine you have accidentally deleted this table.</span></span> <span data-ttu-id="a073e-160">Detta är något du lätt kan återställa från.</span><span class="sxs-lookup"><span data-stu-id="a073e-160">This is something you cannot easily recover from.</span></span> <span data-ttu-id="a073e-161">Azure MySQL-databas kan du toogo tillbaka tooany punkt tidpunkt i hello senast in too35 dagar och återställningspunkt i tid tooa ny server.</span><span class="sxs-lookup"><span data-stu-id="a073e-161">Azure Database for MySQL allows you toogo back tooany point in time in hello last up too35 days and restore this point in time tooa new server.</span></span> <span data-ttu-id="a073e-162">Du kan använda den här nya servern toorecover dina data.</span><span class="sxs-lookup"><span data-stu-id="a073e-162">You can use this new server toorecover your deleted data.</span></span> <span data-ttu-id="a073e-163">hello följande steg hello exempel server tooa återställningspunkt innan hello tabell har lagts till.</span><span class="sxs-lookup"><span data-stu-id="a073e-163">hello following steps restore hello sample server tooa point before hello table was added.</span></span>

<span data-ttu-id="a073e-164">För hello återställning måste hello följande information:</span><span class="sxs-lookup"><span data-stu-id="a073e-164">For hello Restore you need hello following information:</span></span>

- <span data-ttu-id="a073e-165">Återställningspunkt: Välj en i tidpunkt som inträffar innan hello-servern har ändrats.</span><span class="sxs-lookup"><span data-stu-id="a073e-165">Restore point: Select a point-in-time that occurs before hello server was changed.</span></span> <span data-ttu-id="a073e-166">Måste vara större än eller lika med toohello källplatsens databas äldsta säkerhetskopierade värde.</span><span class="sxs-lookup"><span data-stu-id="a073e-166">Must be greater than or equal toohello source database's Oldest backup value.</span></span>
- <span data-ttu-id="a073e-167">Målservern: Ange ett nytt servernamn som du vill toorestore till</span><span class="sxs-lookup"><span data-stu-id="a073e-167">Target server: Provide a new server name you want toorestore to</span></span>
- <span data-ttu-id="a073e-168">Källservern: Ange hello namn hello-server som du vill använda toorestore från</span><span class="sxs-lookup"><span data-stu-id="a073e-168">Source server: Provide hello name of hello server you want toorestore from</span></span>
- <span data-ttu-id="a073e-169">Plats: Du kan inte välja hello region, som standard är det samma som källservern hello</span><span class="sxs-lookup"><span data-stu-id="a073e-169">Location: You cannot select hello region, by default it is same as hello source server</span></span>

```azurecli-interactive
az mysql server restore --resource-group mycliresource --name mycliserver-restored --restore-point-in-time "2017-05-4 03:10" --source-server-name mycliserver
```

<span data-ttu-id="a073e-170">toorestore hello server och [återställa tooa i tidpunkt](./howto-restore-server-portal.md) innan hello tabellen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="a073e-170">toorestore hello server and [restore tooa point-in-time](./howto-restore-server-portal.md) before hello table was deleted.</span></span> <span data-ttu-id="a073e-171">Återställa en server tooa olika punkt i tiden skapar en ny server dubbla som hello originalservern av hello tidpunkt du anger, förutsatt att den är i hello kvarhållningsperiod för din [tjänstnivån](./concepts-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="a073e-171">Restoring a server tooa different point in time creates a duplicate new server as hello original server as of hello point in time you specify, provided that it is within hello retention period for your [service tier](./concepts-service-tiers.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="a073e-172">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a073e-172">Next Steps</span></span>
<span data-ttu-id="a073e-173">I den här kursen har du lärt dig att:</span><span class="sxs-lookup"><span data-stu-id="a073e-173">In this tutorial you learned to:</span></span>
> [!div class="checklist"]
> * <span data-ttu-id="a073e-174">Skapa en Azure-databas för MySQL</span><span class="sxs-lookup"><span data-stu-id="a073e-174">Create an Azure Database for MySQL</span></span>
> * <span data-ttu-id="a073e-175">Konfigurera hello serverbrandvägg</span><span class="sxs-lookup"><span data-stu-id="a073e-175">Configure hello server firewall</span></span>
> * <span data-ttu-id="a073e-176">Använd [mysql kommandoradsverktyget](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate en databas</span><span class="sxs-lookup"><span data-stu-id="a073e-176">Use [mysql command line tool](https://dev.mysql.com/doc/refman/5.6/en/mysql.html) toocreate a database</span></span>
> * <span data-ttu-id="a073e-177">Läs in exempeldata</span><span class="sxs-lookup"><span data-stu-id="a073e-177">Load sample data</span></span>
> * <span data-ttu-id="a073e-178">Frågedata</span><span class="sxs-lookup"><span data-stu-id="a073e-178">Query data</span></span>
> * <span data-ttu-id="a073e-179">Uppdatera data</span><span class="sxs-lookup"><span data-stu-id="a073e-179">Update data</span></span>
> * <span data-ttu-id="a073e-180">Återställa data</span><span class="sxs-lookup"><span data-stu-id="a073e-180">Restore data</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="a073e-181">Azure-databas för MySQL - Azure CLI-exempel</span><span class="sxs-lookup"><span data-stu-id="a073e-181">Azure Database for MySQL - Azure CLI samples</span></span>](./sample-scripts-azure-cli.md)
