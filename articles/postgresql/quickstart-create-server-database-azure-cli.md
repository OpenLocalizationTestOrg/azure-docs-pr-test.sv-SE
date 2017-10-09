---
title: "Skapa en Azure-databas för PostgreSQL med hello Azure CLI | Microsoft Docs"
description: "Snabb start guiden toocreate och hantera Azure-databas för PostgreSQL-servern med hjälp av Azure CLI (kommandoradsgränssnittet)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: 946aa3cbf5ff9f5ac4e51248412d3da5d718141e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-an-azure-database-for-postgresql-using-hello-azure-cli"></a><span data-ttu-id="e3205-103">Skapa en Azure-databas för PostgreSQL med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="e3205-103">Create an Azure Database for PostgreSQL using hello Azure CLI</span></span>
<span data-ttu-id="e3205-104">Azure PostgreSQL-databas är en hanterad tjänst som gör att du toorun, hantera och skala högtillgänglig PostgreSQL-databaser i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="e3205-104">Azure Database for PostgreSQL is a managed service that enables you toorun, manage, and scale highly available PostgreSQL databases in hello cloud.</span></span> <span data-ttu-id="e3205-105">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="e3205-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="e3205-106">Den här snabbstarten visar hur toocreate en Azure-databas för PostgreSQL-server i en [Azure-resursgrupp](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) med hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e3205-106">This quickstart shows you how toocreate an Azure Database for PostgreSQL server in an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) using hello Azure CLI.</span></span>

<span data-ttu-id="e3205-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="e3205-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="e3205-108">Om du väljer tooinstall och använda hello CLI lokalt kräver i det här avsnittet att du kör hello Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="e3205-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="e3205-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="e3205-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="e3205-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="e3205-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="e3205-111">Om du har flera prenumerationer kan välja hello lämpliga prenumeration hello resursen kommer att debiteras.</span><span class="sxs-lookup"><span data-stu-id="e3205-111">If you have multiple subscriptions, choose hello appropriate subscription in which hello resource will be billed.</span></span> <span data-ttu-id="e3205-112">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="e3205-112">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="e3205-113">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="e3205-113">Create a resource group</span></span>

<span data-ttu-id="e3205-114">Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e3205-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="e3205-115">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="e3205-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="e3205-116">hello följande exempel skapar en resursgrupp med namnet `myresourcegroup` i hello `westus` plats.</span><span class="sxs-lookup"><span data-stu-id="e3205-116">hello following example creates a resource group named `myresourcegroup` in hello `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="e3205-117">Skapa en Azure Database för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="e3205-117">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="e3205-118">Skapa en [Azure-databas för PostgreSQL server](overview.md) med hello [az postgres servern skapa](/cli/azure/postgres/server#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e3205-118">Create an [Azure Database for PostgreSQL server](overview.md) using hello [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="e3205-119">En server innehåller en grupp med databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="e3205-119">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="e3205-120">hello följande exempel skapas en server med namnet `mypgserver-20170401` i resursgruppen `myresourcegroup` med inloggning för serveradministratör `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="e3205-120">hello following example creates a server named `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="e3205-121">hello namnet på en server mappar tooDNS namn och därför krävs toobe globalt unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="e3205-121">hello name of a server maps tooDNS name and is thus required toobe globally unique in Azure.</span></span> <span data-ttu-id="e3205-122">Ersätt hello `<server_admin_password>` med ett eget värde.</span><span class="sxs-lookup"><span data-stu-id="e3205-122">Substitute hello `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="e3205-123">hello server admin inloggningsnamn och lösenord som du anger här är nödvändig toolog i toohello server och databaserna senare i den här snabbstartsguide.</span><span class="sxs-lookup"><span data-stu-id="e3205-123">hello server admin login and password that you specify here are required toolog in toohello server and its databases later in this quick start.</span></span> <span data-ttu-id="e3205-124">Kom ihåg eller skriv ned den här informationen så att du kan använda den senare.</span><span class="sxs-lookup"><span data-stu-id="e3205-124">Remember or record this information for later use.</span></span>

<span data-ttu-id="e3205-125">Som standard skapas **postgres**-databasen under din server.</span><span class="sxs-lookup"><span data-stu-id="e3205-125">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="e3205-126">Hej [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) databasen är en standarddatabas som är avsedd för användning av användare, verktyg och program från tredje part.</span><span class="sxs-lookup"><span data-stu-id="e3205-126">hello [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="e3205-127">Konfigurera en brandväggsregel på servernivå</span><span class="sxs-lookup"><span data-stu-id="e3205-127">Configure a server-level firewall rule</span></span>

<span data-ttu-id="e3205-128">Skapa en Azure PostgreSQL servernivå brandväggsregel med hello [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="e3205-128">Create an Azure PostgreSQL server-level firewall rule with hello [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="e3205-129">En brandväggsregel på servernivå som tillåter ett externt program [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) eller [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server via hello Azure PostgreSQL tjänsten brandvägg.</span><span class="sxs-lookup"><span data-stu-id="e3205-129">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) tooconnect tooyour server through hello Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="e3205-130">Du kan ange en brandväggsregel som omfattar en IP-intervallet toobe kan tooconnect från nätverket.</span><span class="sxs-lookup"><span data-stu-id="e3205-130">You can set a firewall rule that covers an IP range toobe able tooconnect from your network.</span></span> <span data-ttu-id="e3205-131">hello följande exempel används [az postgres-brandväggsregel skapa](/cli/azure/postgres/server/firewall-rule#create) toocreate en brandväggsregel `AllowAllIps` för en IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="e3205-131">hello following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) toocreate a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="e3205-132">tooopen alla IP-adresser, använda 0.0.0.0 som hello startar IP-adress och 255.255.255.255 som hello slutadress.</span><span class="sxs-lookup"><span data-stu-id="e3205-132">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="e3205-133">Azure PostgreSQL-servern kommunicerar via port 5432.</span><span class="sxs-lookup"><span data-stu-id="e3205-133">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="e3205-134">När du ansluter innifrån ett företagsnätverk är det möjligt att utgående trafik via port 5432 inte tillåts av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="e3205-134">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="e3205-135">Har din IT-avdelning öppna port 5432 tooconnect tooyour Azure SQL Database-server.</span><span class="sxs-lookup"><span data-stu-id="e3205-135">Have your IT department open port 5432 tooconnect tooyour Azure SQL Database server.</span></span>

## <a name="get-hello-connection-information"></a><span data-ttu-id="e3205-136">Hämta hello anslutningsinformation</span><span class="sxs-lookup"><span data-stu-id="e3205-136">Get hello connection information</span></span>

<span data-ttu-id="e3205-137">tooconnect tooyour server behöver du tooprovide värden information och åtkomst-autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e3205-137">tooconnect tooyour server, you need tooprovide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="e3205-138">hello resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="e3205-138">hello result is in JSON format.</span></span> <span data-ttu-id="e3205-139">Anteckna hello **administratorLogin** och **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="e3205-139">Make a note of hello **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-toopostgresql-database-using-psql"></a><span data-ttu-id="e3205-140">Ansluta tooPostgreSQL databasen med hjälp av psql</span><span class="sxs-lookup"><span data-stu-id="e3205-140">Connect tooPostgreSQL database using psql</span></span>

<span data-ttu-id="e3205-141">Om klientdatorn har PostgreSQL installerat, kan du använda en lokal instans av [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="e3205-141">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) tooconnect tooan Azure PostgreSQL server.</span></span> <span data-ttu-id="e3205-142">Låt oss nu använda hello psql kommandoradsverktyget tooconnect toohello Azure PostgreSQL server.</span><span class="sxs-lookup"><span data-stu-id="e3205-142">Let's now use hello psql command-line utility tooconnect toohello Azure PostgreSQL server.</span></span>

1. <span data-ttu-id="e3205-143">Kör hello följande psql kommandot tooconnect tooan Azure-databas för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="e3205-143">Run hello following psql command tooconnect tooan Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="e3205-144">Till exempel följande kommando hello ansluter toohello standarddatabasen kallas **postgres** på servern PostgreSQL **mypgserver 20170401.postgres.database.azure.com** hjälp av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="e3205-144">For example, hello following command connects toohello default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="e3205-145">Ange hello `<server_admin_password>` du valde när du uppmanas att ange lösenord.</span><span class="sxs-lookup"><span data-stu-id="e3205-145">Enter hello `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  <span data-ttu-id="e3205-146">När du är ansluten toohello server kan du skapa en tom databas hello i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="e3205-146">Once you are connected toohello server, create a blank database at hello prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="e3205-147">I Kommandotolken hello köra hello efter kommandot tooswitch toohello nyskapad databas **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="e3205-147">At hello prompt, execute hello following command tooswitch connection toohello newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="connect-toopostgresql-database-using-pgadmin"></a><span data-ttu-id="e3205-148">Ansluta tooPostgreSQL databasen med hjälp av pgAdmin</span><span class="sxs-lookup"><span data-stu-id="e3205-148">Connect tooPostgreSQL database using pgAdmin</span></span>

<span data-ttu-id="e3205-149">tooconnect tooAzure PostgreSQL servern med hjälp av verktyget hello GUI _pgAdmin_</span><span class="sxs-lookup"><span data-stu-id="e3205-149">tooconnect tooAzure PostgreSQL server using hello GUI tool _pgAdmin_</span></span>
1.  <span data-ttu-id="e3205-150">Starta hello _pgAdmin_ på klientdatorn.</span><span class="sxs-lookup"><span data-stu-id="e3205-150">Launch hello _pgAdmin_ application on your client computer.</span></span> <span data-ttu-id="e3205-151">Du kan installera _pgAdmin_ från http://www.pgadmin.org/.</span><span class="sxs-lookup"><span data-stu-id="e3205-151">You can install _pgAdmin_ from http://www.pgadmin.org/.</span></span>
2.  <span data-ttu-id="e3205-152">Välj **Lägg till ny Server** från hello **snabblänkar** menyn.</span><span class="sxs-lookup"><span data-stu-id="e3205-152">Choose **Add New Server** from hello **Quick Links** menu.</span></span>
3.  <span data-ttu-id="e3205-153">I hello **skapa - Server** dialogrutan **allmänna** ange ett unikt namn för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="e3205-153">In hello **Create - Server** dialog box **General** tab, enter a unique friendly Name for hello server.</span></span> <span data-ttu-id="e3205-154">Låt säga **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="e3205-154">Say **Azure PostgreSQL Server**.</span></span>
 <span data-ttu-id="e3205-155">![pgAdmin-verktyget – skapa – server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span><span class="sxs-lookup"><span data-stu-id="e3205-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span></span>
4.  <span data-ttu-id="e3205-156">I hello **skapa - Server** dialogrutan **anslutning** fliken:</span><span class="sxs-lookup"><span data-stu-id="e3205-156">In hello **Create - Server** dialog box, **Connection** tab:</span></span>
    - <span data-ttu-id="e3205-157">Ange hello fullständigt kvalificerade servernamnet (till exempel **mypgserver 20170401.postgres.database.azure.com**) i hello **värddatorns namn / adress** rutan.</span><span class="sxs-lookup"><span data-stu-id="e3205-157">Enter hello fully qualified server name (for example, **mypgserver-20170401.postgres.database.azure.com**) in hello **Host Name/ Address** box.</span></span> 
    - <span data-ttu-id="e3205-158">Ange port 5432 i hello **Port** rutan.</span><span class="sxs-lookup"><span data-stu-id="e3205-158">Enter port 5432 into hello **Port** box.</span></span> 
    - <span data-ttu-id="e3205-159">Ange hello **inloggning för serveradministratör (user@mypgserver)** fick tidigare i det här quickstart och lösenordet du angav när du skapade hello server i hello **användarnamn** och **lösenord** respektive rutorna.</span><span class="sxs-lookup"><span data-stu-id="e3205-159">Enter hello **Server admin login (user@mypgserver)** obtained earlier in this quickstart and password you entered when you created hello server into hello **Username** and **Password** boxes, respectively.</span></span>
    - <span data-ttu-id="e3205-160">Välj **SSL-läge** som **kräv**.</span><span class="sxs-lookup"><span data-stu-id="e3205-160">Select **SSL Mode** as **Require**.</span></span> <span data-ttu-id="e3205-161">Som standard skapas alla Azure PostgreSQL-servrar med SSL tvingande aktiverat.</span><span class="sxs-lookup"><span data-stu-id="e3205-161">By default, all Azure PostgreSQL servers are created with SSL enforcing turned ON.</span></span> <span data-ttu-id="e3205-162">tooturn av SSL genomdriva mer information finns i [framtvinga SSL](./concepts-ssl-connection-security.md).</span><span class="sxs-lookup"><span data-stu-id="e3205-162">tooturn OFF SSL enforcing, see details in [Enforcing SSL](./concepts-ssl-connection-security.md).</span></span>

    ![pgAdmin – skapa – server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  <span data-ttu-id="e3205-164">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="e3205-164">Click **Save**.</span></span>
6.  <span data-ttu-id="e3205-165">I hello webbläsaren till vänster och expanderar hello **servergrupper**.</span><span class="sxs-lookup"><span data-stu-id="e3205-165">In hello Browser left pane, expand hello **Server Groups**.</span></span> <span data-ttu-id="e3205-166">Välj din server **Azure PostgreSQL-server**.</span><span class="sxs-lookup"><span data-stu-id="e3205-166">Choose your server **Azure PostgreSQL Server**.</span></span>
7.  <span data-ttu-id="e3205-167">Välj hello **Server** du ansluten till och välj sedan **databaser** under den.</span><span class="sxs-lookup"><span data-stu-id="e3205-167">Choose hello **Server** you connected to, and then choose **Databases** under it.</span></span> 
8.  <span data-ttu-id="e3205-168">Högerklicka på **databaser** tooCreate en databas.</span><span class="sxs-lookup"><span data-stu-id="e3205-168">Right-click on **Databases** tooCreate a Database.</span></span>
9.  <span data-ttu-id="e3205-169">Välj ett databasnamn **mypgsqldb** och hello ägare för den som inloggning för serveradministratör **mylogin**.</span><span class="sxs-lookup"><span data-stu-id="e3205-169">Choose a database name **mypgsqldb** and hello owner for it as server admin login **mylogin**.</span></span>
10. <span data-ttu-id="e3205-170">Klicka på **spara** toocreate en tom databas.</span><span class="sxs-lookup"><span data-stu-id="e3205-170">Click **Save** toocreate a blank database.</span></span>
11. <span data-ttu-id="e3205-171">I hello **webbläsare**, expandera hello **servrar** grupp.</span><span class="sxs-lookup"><span data-stu-id="e3205-171">In hello **Browser**, expand hello **Servers** group.</span></span> <span data-ttu-id="e3205-172">Expandera hello-server som du skapade och se hello databasen **mypgsqldb** under den.</span><span class="sxs-lookup"><span data-stu-id="e3205-172">Expand hello server you created, and see hello database **mypgsqldb** under it.</span></span>
 <span data-ttu-id="e3205-173">![pgAdmin – skapa – databas](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span><span class="sxs-lookup"><span data-stu-id="e3205-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span></span>


## <a name="clean-up-resources"></a><span data-ttu-id="e3205-174">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="e3205-174">Clean up resources</span></span>

<span data-ttu-id="e3205-175">Rensa alla resurser som du skapade i hello quickstart genom att ta bort hello [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="e3205-175">Clean up all resources you created in hello quickstart by deleting hello [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!TIP]
> <span data-ttu-id="e3205-176">De andra snabbstarterna i den här samlingen bygger på den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="e3205-176">Other quickstarts in this collection build upon this quick start.</span></span> <span data-ttu-id="e3205-177">Om du tänker toocontinue toowork med efterföljande hello Snabbstart, vill inte rensa resurser som skapats i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="e3205-177">If you plan toocontinue on toowork with subsequent quickstarts, do not clean up hello resources created in this quickstart.</span></span> <span data-ttu-id="e3205-178">Om du inte planerar toocontinue använder du följande steg toodelete hello alla resurser som skapats av denna Snabbstart i hello Azure CLI.</span><span class="sxs-lookup"><span data-stu-id="e3205-178">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quickstart in hello Azure CLI.</span></span>

```azurecli-interactive
az group delete --name myresourcegroup
```

<span data-ttu-id="e3205-179">Om du bara vill toodelete hello en nyligen skapade server kan du köra [az postgres server delete](/cli/azure/postgres/server#delete) kommando.</span><span class="sxs-lookup"><span data-stu-id="e3205-179">If you just would like toodelete hello one newly created server, you can run [az postgres server delete](/cli/azure/postgres/server#delete) command.</span></span>
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a><span data-ttu-id="e3205-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3205-180">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="e3205-181">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="e3205-181">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
