---
title: "Skapa en Azure Database för PostgreSQL med hjälp av Azure-CLI:n | Microsoft Docs"
description: "Snabbstartsguide för att skapa och hantera Azure Database för PostgreSQL-server med Azure-CLI (kommandoradsgränssnitt)."
services: postgresql
author: sanagama
ms.author: sanagama
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.devlang: azure-cli
ms.topic: quickstart
ms.date: 06/13/2017
ms.openlocfilehash: d78243abc140c7b3f0b99bdf56821b7920568550
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="create-an-azure-database-for-postgresql-using-the-azure-cli"></a><span data-ttu-id="cfd80-103">Skapa en Azure Database för PostgreSQL med hjälp av Azure-CLI:n</span><span class="sxs-lookup"><span data-stu-id="cfd80-103">Create an Azure Database for PostgreSQL using the Azure CLI</span></span>
<span data-ttu-id="cfd80-104">Azure Database för PostgreSQL är en hanterad tjänst som låter dig köra, hantera och skala högtillgängliga PostgreSQL-databaser i molnet.</span><span class="sxs-lookup"><span data-stu-id="cfd80-104">Azure Database for PostgreSQL is a managed service that enables you to run, manage, and scale highly available PostgreSQL databases in the cloud.</span></span> <span data-ttu-id="cfd80-105">Azure CLI används för att skapa och hantera Azure-resurser från kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="cfd80-105">The Azure CLI is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="cfd80-106">Den här snabbstarten visar hur du skapar en Azure Database för PostgreSQL-server i en [Azure-resursgrupp](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) med hjälp av Azure-CLI:n.</span><span class="sxs-lookup"><span data-stu-id="cfd80-106">This quickstart shows you how to create an Azure Database for PostgreSQL server in an [Azure resource group](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) using the Azure CLI.</span></span>

<span data-ttu-id="cfd80-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="cfd80-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="cfd80-108">Om du väljer att installera och använda CLI lokalt måste du köra Azure CLI version 2.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="cfd80-108">If you choose to install and use the CLI locally, this topic requires that you are running the Azure CLI version 2.0 or later.</span></span> <span data-ttu-id="cfd80-109">Kör `az --version` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="cfd80-109">Run `az --version` to find the version.</span></span> <span data-ttu-id="cfd80-110">Om du behöver installera eller uppgradera kan du läsa [Installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="cfd80-110">If you need to install or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

<span data-ttu-id="cfd80-111">Om du har flera prenumerationer, välj lämplig prenumeration som resursen ska debiteras till.</span><span class="sxs-lookup"><span data-stu-id="cfd80-111">If you have multiple subscriptions, choose the appropriate subscription in which the resource will be billed.</span></span> <span data-ttu-id="cfd80-112">Välj en specifik prenumerations-ID under ditt konto med hjälp av kommandot [az account set](/cli/azure/account#set).</span><span class="sxs-lookup"><span data-stu-id="cfd80-112">Select a specific subscription ID under your account using [az account set](/cli/azure/account#set) command.</span></span>
```azurecli-interactive
az account set --subscription 00000000-0000-0000-0000-000000000000
```

## <a name="create-a-resource-group"></a><span data-ttu-id="cfd80-113">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="cfd80-113">Create a resource group</span></span>

<span data-ttu-id="cfd80-114">Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med kommandot [az group create](/cli/azure/group#create).</span><span class="sxs-lookup"><span data-stu-id="cfd80-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using the [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="cfd80-115">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="cfd80-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="cfd80-116">I följande exempel skapas en resursgrupp med namnet `myresourcegroup` på platsen `westus`.</span><span class="sxs-lookup"><span data-stu-id="cfd80-116">The following example creates a resource group named `myresourcegroup` in the `westus` location.</span></span>
```azurecli-interactive
az group create --name myresourcegroup --location westus
```

## <a name="create-an-azure-database-for-postgresql-server"></a><span data-ttu-id="cfd80-117">Skapa en Azure Database för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="cfd80-117">Create an Azure Database for PostgreSQL server</span></span>

<span data-ttu-id="cfd80-118">Skapa en [Azure Database för PostgreSQL-server](overview.md) med kommandot [az postgres server create](/cli/azure/postgres/server#create).</span><span class="sxs-lookup"><span data-stu-id="cfd80-118">Create an [Azure Database for PostgreSQL server](overview.md) using the [az postgres server create](/cli/azure/postgres/server#create) command.</span></span> <span data-ttu-id="cfd80-119">En server innehåller en grupp med databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="cfd80-119">A server contains a group of databases managed as a group.</span></span> 

<span data-ttu-id="cfd80-120">Följande exempel skapar en server som heter `mypgserver-20170401` i din resursgrupp `myresourcegroup` med serveradmin-inloggningen `mylogin`.</span><span class="sxs-lookup"><span data-stu-id="cfd80-120">The following example creates a server named `mypgserver-20170401` in your resource group `myresourcegroup` with server admin login `mylogin`.</span></span> <span data-ttu-id="cfd80-121">Namnet på en server mappar till DNS-namnet och måste därför vara globalt unikt i Azure.</span><span class="sxs-lookup"><span data-stu-id="cfd80-121">The name of a server maps to DNS name and is thus required to be globally unique in Azure.</span></span> <span data-ttu-id="cfd80-122">Ersätt `<server_admin_password>` med ditt eget värde.</span><span class="sxs-lookup"><span data-stu-id="cfd80-122">Substitute the `<server_admin_password>` with your own value.</span></span>
```azurecli-interactive
az postgres server create --resource-group myresourcegroup --name mypgserver-20170401  --location westus --admin-user mylogin --admin-password <server_admin_password> --performance-tier Basic --compute-units 50 --version 9.6
```

> [!IMPORTANT]
> <span data-ttu-id="cfd80-123">Det användarnamn och lösenord för serveradministration du anger här krävs för inloggning på servern och databaserna senare i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="cfd80-123">The server admin login and password that you specify here are required to log in to the server and its databases later in this quick start.</span></span> <span data-ttu-id="cfd80-124">Kom ihåg eller skriv ned den här informationen så att du kan använda den senare.</span><span class="sxs-lookup"><span data-stu-id="cfd80-124">Remember or record this information for later use.</span></span>

<span data-ttu-id="cfd80-125">Som standard skapas **postgres**-databasen under din server.</span><span class="sxs-lookup"><span data-stu-id="cfd80-125">By default, **postgres** database gets created under your server.</span></span> <span data-ttu-id="cfd80-126">[Postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html)-databasen är en standarddatabas som är avsedd för användare, verktyg och tredje parts program.</span><span class="sxs-lookup"><span data-stu-id="cfd80-126">The [postgres](https://www.postgresql.org/docs/9.6/static/app-initdb.html) database is a default database meant for use by users, utilities, and third-party applications.</span></span> 


## <a name="configure-a-server-level-firewall-rule"></a><span data-ttu-id="cfd80-127">Konfigurera en brandväggsregel på servernivå</span><span class="sxs-lookup"><span data-stu-id="cfd80-127">Configure a server-level firewall rule</span></span>

<span data-ttu-id="cfd80-128">Skapa en brandväggsregel på Azure PostgreSQL-servernivå med kommandot [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create).</span><span class="sxs-lookup"><span data-stu-id="cfd80-128">Create an Azure PostgreSQL server-level firewall rule with the [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) command.</span></span> <span data-ttu-id="cfd80-129">En brandväggsregel på servernivå låter ett externt program som en [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) eller en [PgAdmin](https://www.pgadmin.org/) ansluta till din server via Azure PostgreSQL-tjänstens brandvägg.</span><span class="sxs-lookup"><span data-stu-id="cfd80-129">A server-level firewall rule allows an external application, such as [psql](https://www.postgresql.org/docs/9.2/static/app-psql.html) or [PgAdmin](https://www.pgadmin.org/) to connect to your server through the Azure PostgreSQL service firewall.</span></span> 

<span data-ttu-id="cfd80-130">Du kan ställa in en brandväggsregel som omfattar ett IP-intervall för att kunna ansluta från ditt nätverk.</span><span class="sxs-lookup"><span data-stu-id="cfd80-130">You can set a firewall rule that covers an IP range to be able to connect from your network.</span></span> <span data-ttu-id="cfd80-131">Följande exempel använder [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) för att skapa en brandväggsregel `AllowAllIps` för ett IP-adressintervall.</span><span class="sxs-lookup"><span data-stu-id="cfd80-131">The following example uses [az postgres server firewall-rule create](/cli/azure/postgres/server/firewall-rule#create) to create a firewall rule `AllowAllIps` for an IP address range.</span></span> <span data-ttu-id="cfd80-132">Öppna alla IP-adresser genom att använda 0.0.0.0 som den första IP-adressen och 255.255.255.255 som slutadress.</span><span class="sxs-lookup"><span data-stu-id="cfd80-132">To open all IP addresses, use 0.0.0.0 as the starting IP address and 255.255.255.255 as the ending address.</span></span>
```azurecli-interactive
az postgres server firewall-rule create --resource-group myresourcegroup --server mypgserver-20170401 --name AllowAllIps --start-ip-address 0.0.0.0 --end-ip-address 255.255.255.255
```

> [!NOTE]
> <span data-ttu-id="cfd80-133">Azure PostgreSQL-servern kommunicerar via port 5432.</span><span class="sxs-lookup"><span data-stu-id="cfd80-133">Azure PostgreSQL server communicates over port 5432.</span></span> <span data-ttu-id="cfd80-134">När du ansluter innifrån ett företagsnätverk är det möjligt att utgående trafik via port 5432 inte tillåts av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="cfd80-134">When connecting from within a corporate network, outbound traffic over port 5432 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="cfd80-135">Be din IT-avdelning öppna port 5432 för att ansluta till din Azure SQL-databasserver.</span><span class="sxs-lookup"><span data-stu-id="cfd80-135">Have your IT department open port 5432 to connect to your Azure SQL Database server.</span></span>

## <a name="get-the-connection-information"></a><span data-ttu-id="cfd80-136">Hämta anslutningsinformationen</span><span class="sxs-lookup"><span data-stu-id="cfd80-136">Get the connection information</span></span>

<span data-ttu-id="cfd80-137">För att ansluta till servern måste du ange värddatorinformationen och autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="cfd80-137">To connect to your server, you need to provide host information and access credentials.</span></span>
```azurecli-interactive
az postgres server show --resource-group myresourcegroup --name mypgserver-20170401
```

<span data-ttu-id="cfd80-138">Resultatet är i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="cfd80-138">The result is in JSON format.</span></span> <span data-ttu-id="cfd80-139">Anteckna **administratorLogin** och **fullyQualifiedDomainName**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-139">Make a note of the **administratorLogin** and **fullyQualifiedDomainName**.</span></span>
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

## <a name="connect-to-postgresql-database-using-psql"></a><span data-ttu-id="cfd80-140">Anslut till PostgreSQL-databasen med psql</span><span class="sxs-lookup"><span data-stu-id="cfd80-140">Connect to PostgreSQL database using psql</span></span>

<span data-ttu-id="cfd80-141">Om din klientdator har PostgreSQL installerat, kan du använda en lokal instans av [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) för att ansluta till en Azure PostgreSQL-server.</span><span class="sxs-lookup"><span data-stu-id="cfd80-141">If your client computer has PostgreSQL installed, you can use a local instance of [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) to connect to an Azure PostgreSQL server.</span></span> <span data-ttu-id="cfd80-142">Nu använder vi psql-kommandoradsverktyget för att ansluta till Azure PostgreSQL-servern.</span><span class="sxs-lookup"><span data-stu-id="cfd80-142">Let's now use the psql command-line utility to connect to the Azure PostgreSQL server.</span></span>

1. <span data-ttu-id="cfd80-143">Kör följande psql-kommando för att ansluta till en Azure Database för PostgreSQL-server</span><span class="sxs-lookup"><span data-stu-id="cfd80-143">Run the following psql command to connect to an Azure Database for PostgreSQL server</span></span>
```azurecli-interactive
psql --host=<servername> --port=<port> --username=<user@servername> --dbname=<dbname>
```

  <span data-ttu-id="cfd80-144">Följande kommando till exempel, ansluter till standarddatabasen som heter **postgres** på din PostgreSQL-server **mypgserver-20170401.postgres.database.azure.com** med hjälp av autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="cfd80-144">For example, the following command connects to the default database called **postgres** on your PostgreSQL server **mypgserver-20170401.postgres.database.azure.com** using access credentials.</span></span> <span data-ttu-id="cfd80-145">Ange den `<server_admin_password>` som du valde när du uppmanades att ange lösenordet.</span><span class="sxs-lookup"><span data-stu-id="cfd80-145">Enter the `<server_admin_password>` you chose when prompted for password.</span></span>
  
  ```azurecli-interactive
psql --host=mypgserver-20170401.postgres.database.azure.com --port=5432 --username=mylogin@mypgserver-20170401 --dbname=postgres
```

2.  <span data-ttu-id="cfd80-146">När du är ansluten till servern, skapar du en blank databas i prompten.</span><span class="sxs-lookup"><span data-stu-id="cfd80-146">Once you are connected to the server, create a blank database at the prompt.</span></span>
```sql
CREATE DATABASE mypgsqldb;
```

3.  <span data-ttu-id="cfd80-147">I prompten, kör du följande kommando för att växla anslutning till den nyligen skapade databasen **mypgsqldb**:</span><span class="sxs-lookup"><span data-stu-id="cfd80-147">At the prompt, execute the following command to switch connection to the newly created database **mypgsqldb**:</span></span>
```sql
\c mypgsqldb
```

## <a name="connect-to-postgresql-database-using-pgadmin"></a><span data-ttu-id="cfd80-148">Anslut till PostgreSQL-databasen med pgAdmin</span><span class="sxs-lookup"><span data-stu-id="cfd80-148">Connect to PostgreSQL database using pgAdmin</span></span>

<span data-ttu-id="cfd80-149">Om du vill ansluta till Azure PostgreSQL-servern med GUI-verktyget _pgAdmin_</span><span class="sxs-lookup"><span data-stu-id="cfd80-149">To connect to Azure PostgreSQL server using the GUI tool _pgAdmin_</span></span>
1.  <span data-ttu-id="cfd80-150">Starta _pgAdmin_-programmet på din klientdator.</span><span class="sxs-lookup"><span data-stu-id="cfd80-150">Launch the _pgAdmin_ application on your client computer.</span></span> <span data-ttu-id="cfd80-151">Du kan installera _pgAdmin_ från http://www.pgadmin.org/.</span><span class="sxs-lookup"><span data-stu-id="cfd80-151">You can install _pgAdmin_ from http://www.pgadmin.org/.</span></span>
2.  <span data-ttu-id="cfd80-152">Välj **Lägg till ny Server** från menyn **snabblänkar**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-152">Choose **Add New Server** from the **Quick Links** menu.</span></span>
3.  <span data-ttu-id="cfd80-153">I dialogrutan **skapa – server** i fliken **allmänt**, anger du ett unikt eget namn för servern.</span><span class="sxs-lookup"><span data-stu-id="cfd80-153">In the **Create - Server** dialog box **General** tab, enter a unique friendly Name for the server.</span></span> <span data-ttu-id="cfd80-154">Låt säga **Azure PostgreSQL Server**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-154">Say **Azure PostgreSQL Server**.</span></span>
 <span data-ttu-id="cfd80-155">![pgAdmin-verktyget – skapa – server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span><span class="sxs-lookup"><span data-stu-id="cfd80-155">![pgAdmin tool - create - server](./media/quickstart-create-server-database-azure-cli/1-pgadmin-create-server.png)</span></span>
4.  <span data-ttu-id="cfd80-156">I dialogrutan **skapa – server**, i fliken **anslutning**:</span><span class="sxs-lookup"><span data-stu-id="cfd80-156">In the **Create - Server** dialog box, **Connection** tab:</span></span>
    - <span data-ttu-id="cfd80-157">Ange det fullständigt kvalificerade servernamnet (till exempel **mypgserver-20170401.postgres.database.azure.com**) i rutan **värddatornamn / adress**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-157">Enter the fully qualified server name (for example, **mypgserver-20170401.postgres.database.azure.com**) in the **Host Name/ Address** box.</span></span> 
    - <span data-ttu-id="cfd80-158">Ange port 5432 i **Port**-rutan.</span><span class="sxs-lookup"><span data-stu-id="cfd80-158">Enter port 5432 into the **Port** box.</span></span> 
    - <span data-ttu-id="cfd80-159">Ange den **serveradmin-inloggning (user@mypgserver)** som du fick tidigare i den här snabbstarten och lösenordet du angav när du skapade servern i **användarnamn**- respektive **lösenords**-rutorna.</span><span class="sxs-lookup"><span data-stu-id="cfd80-159">Enter the **Server admin login (user@mypgserver)** obtained earlier in this quickstart and password you entered when you created the server into the **Username** and **Password** boxes, respectively.</span></span>
    - <span data-ttu-id="cfd80-160">Välj **SSL-läge** som **kräv**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-160">Select **SSL Mode** as **Require**.</span></span> <span data-ttu-id="cfd80-161">Som standard skapas alla Azure PostgreSQL-servrar med SSL tvingande aktiverat.</span><span class="sxs-lookup"><span data-stu-id="cfd80-161">By default, all Azure PostgreSQL servers are created with SSL enforcing turned ON.</span></span> <span data-ttu-id="cfd80-162">Om du vill inaktivera SSL tvingande, se informationen i [tvingande SSL](./concepts-ssl-connection-security.md).</span><span class="sxs-lookup"><span data-stu-id="cfd80-162">To turn OFF SSL enforcing, see details in [Enforcing SSL](./concepts-ssl-connection-security.md).</span></span>

    ![pgAdmin – skapa – server](./media/quickstart-create-server-database-azure-cli/2-pgadmin-create-server.png)
5.  <span data-ttu-id="cfd80-164">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-164">Click **Save**.</span></span>
6.  <span data-ttu-id="cfd80-165">I webbläsarens vänstra fönster, expanderar du **servergrupper**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-165">In the Browser left pane, expand the **Server Groups**.</span></span> <span data-ttu-id="cfd80-166">Välj din server **Azure PostgreSQL-server**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-166">Choose your server **Azure PostgreSQL Server**.</span></span>
7.  <span data-ttu-id="cfd80-167">Välj den **server** du anslöt till och välj sedan **databaser** under den.</span><span class="sxs-lookup"><span data-stu-id="cfd80-167">Choose the **Server** you connected to, and then choose **Databases** under it.</span></span> 
8.  <span data-ttu-id="cfd80-168">Högerklicka på **databaser** för att skapa en databas.</span><span class="sxs-lookup"><span data-stu-id="cfd80-168">Right-click on **Databases** to Create a Database.</span></span>
9.  <span data-ttu-id="cfd80-169">Välj ett databasnamn **mypgsqldb** och ägaren för den som inloggning för serveradministratören **mylogin**.</span><span class="sxs-lookup"><span data-stu-id="cfd80-169">Choose a database name **mypgsqldb** and the owner for it as server admin login **mylogin**.</span></span>
10. <span data-ttu-id="cfd80-170">Klicka på **spara** för att skapa en tom databas.</span><span class="sxs-lookup"><span data-stu-id="cfd80-170">Click **Save** to create a blank database.</span></span>
11. <span data-ttu-id="cfd80-171">I **webbläsaren**, expanderar du **servrar**-gruppen.</span><span class="sxs-lookup"><span data-stu-id="cfd80-171">In the **Browser**, expand the **Servers** group.</span></span> <span data-ttu-id="cfd80-172">Expandera servern som du skapade och se databasen **mypgsqldb** under den.</span><span class="sxs-lookup"><span data-stu-id="cfd80-172">Expand the server you created, and see the database **mypgsqldb** under it.</span></span>
 <span data-ttu-id="cfd80-173">![pgAdmin – skapa – databas](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span><span class="sxs-lookup"><span data-stu-id="cfd80-173">![pgAdmin - Create - Database](./media/quickstart-create-server-database-azure-cli/3-pgadmin-database.png)</span></span>


## <a name="clean-up-resources"></a><span data-ttu-id="cfd80-174">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="cfd80-174">Clean up resources</span></span>

<span data-ttu-id="cfd80-175">Rensa upp alla resurser du skapade i snabbstarten genom att ta bort [Azure-resursgruppen](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="cfd80-175">Clean up all resources you created in the quickstart by deleting the [Azure resource group](../azure-resource-manager/resource-group-overview.md).</span></span>

> [!TIP]
> <span data-ttu-id="cfd80-176">De andra snabbstarterna i den här samlingen bygger på den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="cfd80-176">Other quickstarts in this collection build upon this quick start.</span></span> <span data-ttu-id="cfd80-177">Om du planerar att fortsätta att arbeta med efterföljande snabbstarter, ska du inte rensa upp resurserna som du skapade i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="cfd80-177">If you plan to continue on to work with subsequent quickstarts, do not clean up the resources created in this quickstart.</span></span> <span data-ttu-id="cfd80-178">Om du inte planerar att fortsätta, kan du använda följande steg för att ta bort alla resurser som skapades av den här snabbstarten i Azure-CLI:n.</span><span class="sxs-lookup"><span data-stu-id="cfd80-178">If you do not plan to continue, use the following steps to delete all resources created by this quickstart in the Azure CLI.</span></span>

```azurecli-interactive
az group delete --name myresourcegroup
```

<span data-ttu-id="cfd80-179">Om du endast vill radera den nyskapade servern kan du köra kommandot [az postgres server delete](/cli/azure/postgres/server#delete).</span><span class="sxs-lookup"><span data-stu-id="cfd80-179">If you just would like to delete the one newly created server, you can run [az postgres server delete](/cli/azure/postgres/server#delete) command.</span></span>
```azurecli-interactive
az postgres server delete --resource-group myresourcegroup --name mypgserver-20170401
```

## <a name="next-steps"></a><span data-ttu-id="cfd80-180">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="cfd80-180">Next steps</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="cfd80-181">Migrera din databas med Exportera och importera</span><span class="sxs-lookup"><span data-stu-id="cfd80-181">Migrate your database using Export and Import</span></span>](./howto-migrate-using-export-and-import.md)
