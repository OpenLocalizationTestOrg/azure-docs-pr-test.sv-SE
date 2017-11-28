---
title: 'Azure CLI: Skapa en SQL-databas | Microsoft Docs'
description: "Lär dig hur toocreate en logisk SQL Database-server, brandväggsregel på servernivå och databaser med hjälp av hello Azure CLI."
keywords: "sql database-självstudier, skapa en sql-databas"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: mvc,DBs & servers
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: azurecli
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: 9b1ffb17eabeb70a000ff0c997128832b07aa4fd
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-hello-azure-cli"></a><span data-ttu-id="597cd-104">Skapa en enda Azure SQL-databas med hello Azure CLI</span><span class="sxs-lookup"><span data-stu-id="597cd-104">Create a single Azure SQL database using hello Azure CLI</span></span>

<span data-ttu-id="597cd-105">hello Azure CLI är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="597cd-105">hello Azure CLI is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="597cd-106">Detta hjälper information med hjälp av hello Azure CLI toodeploy en Azure SQL-databas i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i en [logisk Azure SQL Database-server](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="597cd-106">This guide details using hello Azure CLI toodeploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="597cd-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="597cd-107">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) before you begin.</span></span>

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

<span data-ttu-id="597cd-108">Om du väljer tooinstall och använda hello CLI lokalt i det här avsnittet kräver att du kör hello Azure CLI version 2.0.4 eller senare.</span><span class="sxs-lookup"><span data-stu-id="597cd-108">If you choose tooinstall and use hello CLI locally, this topic requires that you are running hello Azure CLI version 2.0.4 or later.</span></span> <span data-ttu-id="597cd-109">Kör `az --version` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="597cd-109">Run `az --version` toofind hello version.</span></span> <span data-ttu-id="597cd-110">Om du behöver tooinstall eller uppgradering, se [installera Azure CLI 2.0]( /cli/azure/install-azure-cli).</span><span class="sxs-lookup"><span data-stu-id="597cd-110">If you need tooinstall or upgrade, see [Install Azure CLI 2.0]( /cli/azure/install-azure-cli).</span></span> 

## <a name="define-variables"></a><span data-ttu-id="597cd-111">Definiera variabler</span><span class="sxs-lookup"><span data-stu-id="597cd-111">Define variables</span></span>

<span data-ttu-id="597cd-112">Definiera variabler för användning i hello skript i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="597cd-112">Define variables for use in hello scripts in this quick start.</span></span>

```azurecli-interactive
# hello data center and resource name for your resources
export resourcegroupname = myResourceGroup
export location = westeurope
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
export servername = server-$RANDOM
# Set an admin login and password for your database
export adminlogin = ServerAdmin
export password = ChangeYourAdminPassword1
# hello ip address range that you want tooallow tooaccess your DB
export startip = "0.0.0.0"
export endip = "0.0.0.0"
# hello database name
export databasename = mySampleDatabase
```

## <a name="create-a-resource-group"></a><span data-ttu-id="597cd-113">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="597cd-113">Create a resource group</span></span>

<span data-ttu-id="597cd-114">Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med hello [az gruppen skapa](/cli/azure/group#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="597cd-114">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [az group create](/cli/azure/group#create) command.</span></span> <span data-ttu-id="597cd-115">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="597cd-115">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="597cd-116">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats.</span><span class="sxs-lookup"><span data-stu-id="597cd-116">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location.</span></span>

```azurecli-interactive
az group create --name $resourcegroupname --location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="597cd-117">Skapa en logisk server</span><span class="sxs-lookup"><span data-stu-id="597cd-117">Create a logical server</span></span>

<span data-ttu-id="597cd-118">Skapa en [logisk Azure SQL Database-server](sql-database-features.md) med hello [az sql-servern skapa](/cli/azure/sql/server#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="597cd-118">Create an [Azure SQL Database logical server](sql-database-features.md) using hello [az sql server create](/cli/azure/sql/server#create) command.</span></span> <span data-ttu-id="597cd-119">En logisk server innehåller en uppsättning databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="597cd-119">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="597cd-120">hello följande exempel skapas en slumpmässigt namn i resursgruppen med en administratörsinloggning med namnet `ServerAdmin` och lösenordet `ChangeYourAdminPassword1`.</span><span class="sxs-lookup"><span data-stu-id="597cd-120">hello following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="597cd-121">Ersätt dessa fördefinierade värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="597cd-121">Replace these pre-defined values as desired.</span></span>

```azurecli-interactive
az sql server create --name $servername --resource-group $resourcegroupname --location $location \
    --admin-user $adminlogin --admin-password $password
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="597cd-122">Konfigurera en serverbrandväggsregel</span><span class="sxs-lookup"><span data-stu-id="597cd-122">Configure a server firewall rule</span></span>

<span data-ttu-id="597cd-123">Skapa en [Azure SQL Database brandväggsregel på servernivå](sql-database-firewall-configure.md) med hello [az sql server-brandvägg skapa](/cli/azure/sql/server/firewall-rule#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="597cd-123">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using hello [az sql server firewall create](/cli/azure/sql/server/firewall-rule#create) command.</span></span> <span data-ttu-id="597cd-124">En brandväggsregel på servernivå kan ett externt program, till exempel SQL Server Management Studio eller hello SQLCMD-verktyget tooconnect tooa SQL-databasen via hello SQL Database-tjänsten brandvägg.</span><span class="sxs-lookup"><span data-stu-id="597cd-124">A server-level firewall rule allows an external application, such as SQL Server Management Studio or hello SQLCMD utility tooconnect tooa SQL database through hello SQL Database service firewall.</span></span> <span data-ttu-id="597cd-125">I följande exempel hello, öppnas hello brandväggen bara för andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="597cd-125">In hello following example, hello firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="597cd-126">tooenable extern anslutning, ändra hello IP-adress tooan korrekt adress för din miljö.</span><span class="sxs-lookup"><span data-stu-id="597cd-126">tooenable external connectivity, change hello IP address tooan appropriate address for your environment.</span></span> <span data-ttu-id="597cd-127">tooopen alla IP-adresser, använda 0.0.0.0 som hello startar IP-adress och 255.255.255.255 som hello slutadress.</span><span class="sxs-lookup"><span data-stu-id="597cd-127">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>  

```azurecli-interactive
az sql server firewall-rule create --resource-group $resourcegroupname --server $servername \
    -n AllowYourIp --start-ip-address $startip --end-ip-address $endip
```

> [!NOTE]
> <span data-ttu-id="597cd-128">SQL Database kommunicerar via port 1433.</span><span class="sxs-lookup"><span data-stu-id="597cd-128">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="597cd-129">Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg.</span><span class="sxs-lookup"><span data-stu-id="597cd-129">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="597cd-130">I så fall, blir inte kan tooconnect tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.</span><span class="sxs-lookup"><span data-stu-id="597cd-130">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a><span data-ttu-id="597cd-131">Skapa en databas i hello server med exempeldata</span><span class="sxs-lookup"><span data-stu-id="597cd-131">Create a database in hello server with sample data</span></span>

<span data-ttu-id="597cd-132">Skapa en databas med en [prestandanivå S0](sql-database-service-tiers.md) i hello-servern med hjälp av hello [az sql db skapa](/cli/azure/sql/db#create) kommando.</span><span class="sxs-lookup"><span data-stu-id="597cd-132">Create a database with an [S0 performance level](sql-database-service-tiers.md) in hello server using hello [az sql db create](/cli/azure/sql/db#create) command.</span></span> <span data-ttu-id="597cd-133">hello följande exempel skapar en databas som heter `mySampleDatabase` och belastningar hello AdventureWorksLT exempeldata till den här databasen.</span><span class="sxs-lookup"><span data-stu-id="597cd-133">hello following example creates a database called `mySampleDatabase` and loads hello AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="597cd-134">Ersätt de fördefinierade värden efter behov (andra snabb startar i den här samlingen build vid hello värdena i den här snabbstartsguide).</span><span class="sxs-lookup"><span data-stu-id="597cd-134">Replace these predefined values as desired (other quick starts in this collection build upon hello values in this quick start).</span></span>

```azurecli-interactive
az sql db create --resource-group $resourcegroupname --server $servername \
    --name $databasename --sample-name AdventureWorksLT --service-objective S0
```

## <a name="clean-up-resources"></a><span data-ttu-id="597cd-135">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="597cd-135">Clean up resources</span></span>

<span data-ttu-id="597cd-136">Andra snabbstarter i den här samlingen bygger på den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="597cd-136">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="597cd-137">Om du tänker toocontinue toowork med efterföljande snabbstarter, rensa hello resurser som skapas på den här quick starta inte.</span><span class="sxs-lookup"><span data-stu-id="597cd-137">If you plan toocontinue on toowork with subsequent quick starts, do not clean up hello resources created in this quick start.</span></span> <span data-ttu-id="597cd-138">Om du inte planerar toocontinue använda hello efter steg toodelete alla resurser skapas av den här Snabbstart i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="597cd-138">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quick start in hello Azure portal.</span></span>
>

```azurecli-interactive
az group delete --name $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="597cd-139">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="597cd-139">Next steps</span></span>

<span data-ttu-id="597cd-140">Nu när du har en databas kan du ansluta och söka med dina favoritverktyg.</span><span class="sxs-lookup"><span data-stu-id="597cd-140">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="597cd-141">Lär dig mer genom att välja verktyg nedan:</span><span class="sxs-lookup"><span data-stu-id="597cd-141">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="597cd-142">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="597cd-142">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="597cd-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="597cd-143">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="597cd-144">NET</span><span class="sxs-lookup"><span data-stu-id="597cd-144">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="597cd-145">PHP</span><span class="sxs-lookup"><span data-stu-id="597cd-145">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="597cd-146">Node.js</span><span class="sxs-lookup"><span data-stu-id="597cd-146">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="597cd-147">Java</span><span class="sxs-lookup"><span data-stu-id="597cd-147">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="597cd-148">Python</span><span class="sxs-lookup"><span data-stu-id="597cd-148">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="597cd-149">Ruby</span><span class="sxs-lookup"><span data-stu-id="597cd-149">Ruby</span></span>](sql-database-connect-query-ruby.md)

