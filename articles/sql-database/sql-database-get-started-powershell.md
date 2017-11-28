---
title: 'Azure PowerShell: Skapa en SQL-databas | Microsoft Docs'
description: "Lär dig hur toocreate en logisk SQL Database-server, brandväggsregel på servernivå och databaser i hello Azure-portalen."
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
ms.devlang: PowerShell
ms.topic: hero-article
ms.date: 04/17/2017
ms.author: carlrab
ms.openlocfilehash: e89f68b44083a3b64e61f95117dbbedfa6647ccb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-a-single-azure-sql-database-using-powershell"></a><span data-ttu-id="57269-104">Skapa en enskild Azure SQL-databas med PowerShell</span><span class="sxs-lookup"><span data-stu-id="57269-104">Create a single Azure SQL database using PowerShell</span></span>

<span data-ttu-id="57269-105">PowerShell är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="57269-105">PowerShell is used toocreate and manage Azure resources from hello command line or in scripts.</span></span> <span data-ttu-id="57269-106">Detta hjälper information med hjälp av PowerShell toodeploy en Azure SQL-databas i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i en [logisk Azure SQL Database-server](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="57269-106">This guide details using PowerShell toodeploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="57269-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="57269-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

<span data-ttu-id="57269-108">Den här kursen kräver hello Azure PowerShell Modulversion 4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="57269-108">This tutorial requires hello Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="57269-109">Kör ` Get-Module -ListAvailable AzureRM` toofind hello version.</span><span class="sxs-lookup"><span data-stu-id="57269-109">Run ` Get-Module -ListAvailable AzureRM` toofind hello version.</span></span> <span data-ttu-id="57269-110">Om du behöver tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="57269-110">If you need tooinstall or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

## <a name="log-in-tooazure"></a><span data-ttu-id="57269-111">Logga in tooAzure</span><span class="sxs-lookup"><span data-stu-id="57269-111">Log in tooAzure</span></span>

<span data-ttu-id="57269-112">Logga in tooyour Azure-prenumeration med hjälp av hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) kommando och följ hello på skärmen riktningar.</span><span class="sxs-lookup"><span data-stu-id="57269-112">Log in tooyour Azure subscription using hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command and follow hello on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a><span data-ttu-id="57269-113">Skapa variabler</span><span class="sxs-lookup"><span data-stu-id="57269-113">Create variables</span></span>

<span data-ttu-id="57269-114">Definiera variabler för användning i hello skript i denna Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="57269-114">Define variables for use in hello scripts in this quick start.</span></span>

```powershell
# hello data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# hello logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# hello login information for hello server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# hello ip address range that you want tooallow tooaccess your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# hello database name
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="57269-115">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="57269-115">Create a resource group</span></span>

<span data-ttu-id="57269-116">Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kommando.</span><span class="sxs-lookup"><span data-stu-id="57269-116">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="57269-117">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="57269-117">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="57269-118">hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats.</span><span class="sxs-lookup"><span data-stu-id="57269-118">hello following example creates a resource group named `myResourceGroup` in hello `westeurope` location.</span></span>

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="57269-119">Skapa en logisk server</span><span class="sxs-lookup"><span data-stu-id="57269-119">Create a logical server</span></span>

<span data-ttu-id="57269-120">Skapa en [logisk Azure SQL Database-server](sql-database-features.md) med hello [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) kommando.</span><span class="sxs-lookup"><span data-stu-id="57269-120">Create an [Azure SQL Database logical server](sql-database-features.md) using hello [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) command.</span></span> <span data-ttu-id="57269-121">En logisk server innehåller en uppsättning databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="57269-121">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="57269-122">hello följande exempel skapas en slumpmässigt namn i resursgruppen med en administratörsinloggning med namnet `ServerAdmin` och lösenordet `ChangeYourAdminPassword1`.</span><span class="sxs-lookup"><span data-stu-id="57269-122">hello following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="57269-123">Ersätt dessa fördefinierade värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="57269-123">Replace these pre-defined values as desired.</span></span>

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="57269-124">Konfigurera en serverbrandväggsregel</span><span class="sxs-lookup"><span data-stu-id="57269-124">Configure a server firewall rule</span></span>

<span data-ttu-id="57269-125">Skapa en [Azure SQL Database brandväggsregel på servernivå](sql-database-firewall-configure.md) med hello [ny AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) kommando.</span><span class="sxs-lookup"><span data-stu-id="57269-125">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using hello [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) command.</span></span> <span data-ttu-id="57269-126">En brandväggsregel på servernivå kan ett externt program, till exempel SQL Server Management Studio eller hello SQLCMD-verktyget tooconnect tooa SQL-databasen via hello SQL Database-tjänsten brandvägg.</span><span class="sxs-lookup"><span data-stu-id="57269-126">A server-level firewall rule allows an external application, such as SQL Server Management Studio or hello SQLCMD utility tooconnect tooa SQL database through hello SQL Database service firewall.</span></span> <span data-ttu-id="57269-127">I följande exempel hello, öppnas hello brandväggen bara för andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="57269-127">In hello following example, hello firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="57269-128">tooenable extern anslutning, ändra hello IP-adress tooan korrekt adress för din miljö.</span><span class="sxs-lookup"><span data-stu-id="57269-128">tooenable external connectivity, change hello IP address tooan appropriate address for your environment.</span></span> <span data-ttu-id="57269-129">tooopen alla IP-adresser, använda 0.0.0.0 som hello startar IP-adress och 255.255.255.255 som hello slutadress.</span><span class="sxs-lookup"><span data-stu-id="57269-129">tooopen all IP addresses, use 0.0.0.0 as hello starting IP address and 255.255.255.255 as hello ending address.</span></span>

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> <span data-ttu-id="57269-130">SQL Database kommunicerar via port 1433.</span><span class="sxs-lookup"><span data-stu-id="57269-130">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="57269-131">Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg.</span><span class="sxs-lookup"><span data-stu-id="57269-131">If you are trying tooconnect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="57269-132">I så fall, blir inte kan tooconnect tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.</span><span class="sxs-lookup"><span data-stu-id="57269-132">If so, you will not be able tooconnect tooyour Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a><span data-ttu-id="57269-133">Skapa en databas i hello server med exempeldata</span><span class="sxs-lookup"><span data-stu-id="57269-133">Create a database in hello server with sample data</span></span>

<span data-ttu-id="57269-134">Skapa en databas med en [prestandanivå S0](sql-database-service-tiers.md) i hello-servern med hjälp av hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) kommando.</span><span class="sxs-lookup"><span data-stu-id="57269-134">Create a database with an [S0 performance level](sql-database-service-tiers.md) in hello server using hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) command.</span></span> <span data-ttu-id="57269-135">hello följande exempel skapar en databas som heter `mySampleDatabase` och belastningar hello AdventureWorksLT exempeldata till den här databasen.</span><span class="sxs-lookup"><span data-stu-id="57269-135">hello following example creates a database called `mySampleDatabase` and loads hello AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="57269-136">Ersätt de fördefinierade värden efter behov (andra snabb startar i den här samlingen build vid hello värdena i den här snabbstartsguide).</span><span class="sxs-lookup"><span data-stu-id="57269-136">Replace these predefined values as desired (other quick starts in this collection build upon hello values in this quick start).</span></span>

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a><span data-ttu-id="57269-137">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="57269-137">Clean up resources</span></span>

<span data-ttu-id="57269-138">Andra snabbstarter i den här samlingen bygger på den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="57269-138">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="57269-139">Om du tänker toocontinue toowork med efterföljande snabbstarter, rensa hello resurser som skapas på den här quick starta inte.</span><span class="sxs-lookup"><span data-stu-id="57269-139">If you plan toocontinue on toowork with subsequent quick starts, do not clean up hello resources created in this quick start.</span></span> <span data-ttu-id="57269-140">Om du inte planerar toocontinue använda hello efter steg toodelete alla resurser skapas av den här Snabbstart i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="57269-140">If you do not plan toocontinue, use hello following steps toodelete all resources created by this quick start in hello Azure portal.</span></span>
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="57269-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="57269-141">Next steps</span></span>

<span data-ttu-id="57269-142">Nu när du har en databas kan du ansluta och söka med dina favoritverktyg.</span><span class="sxs-lookup"><span data-stu-id="57269-142">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="57269-143">Lär dig mer genom att välja verktyg nedan:</span><span class="sxs-lookup"><span data-stu-id="57269-143">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="57269-144">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="57269-144">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="57269-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="57269-145">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="57269-146">NET</span><span class="sxs-lookup"><span data-stu-id="57269-146">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="57269-147">PHP</span><span class="sxs-lookup"><span data-stu-id="57269-147">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="57269-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="57269-148">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="57269-149">Java</span><span class="sxs-lookup"><span data-stu-id="57269-149">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="57269-150">Python</span><span class="sxs-lookup"><span data-stu-id="57269-150">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="57269-151">Ruby</span><span class="sxs-lookup"><span data-stu-id="57269-151">Ruby</span></span>](sql-database-connect-query-ruby.md)

