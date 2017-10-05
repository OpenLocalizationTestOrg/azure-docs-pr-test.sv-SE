---
title: 'Azure PowerShell: Skapa en SQL-databas | Microsoft Docs'
description: "Lär dig att skapa en logisk SQL Database-server, brandväggsregel på servernivå och databaser i Azure Portal."
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
ms.openlocfilehash: 44ed4a603977617c898315c4fc0b2d3dd3a8f16a
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="create-a-single-azure-sql-database-using-powershell"></a><span data-ttu-id="dfd7a-104">Skapa en enskild Azure SQL-databas med PowerShell</span><span class="sxs-lookup"><span data-stu-id="dfd7a-104">Create a single Azure SQL database using PowerShell</span></span>

<span data-ttu-id="dfd7a-105">PowerShell används för att skapa och hantera Azure-resurser från kommandoraden eller i skript.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-105">PowerShell is used to create and manage Azure resources from the command line or in scripts.</span></span> <span data-ttu-id="dfd7a-106">I den här handboken får du information om hur du använder PowerShell för att distribuera en Azure SQL-databas i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i en [logisk Azure SQL Database-server](sql-database-features.md).</span><span class="sxs-lookup"><span data-stu-id="dfd7a-106">This guide details using PowerShell to deploy an Azure SQL database in an [Azure resource group](../azure-resource-manager/resource-group-overview.md) in an [Azure SQL Database logical server](sql-database-features.md).</span></span>

<span data-ttu-id="dfd7a-107">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-107">If you don't have an Azure subscription, create a [free](https://azure.microsoft.com/free/) account before you begin.</span></span>

<span data-ttu-id="dfd7a-108">Den här självstudien kräver Azure PowerShell-modul version 4.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-108">This tutorial requires the Azure PowerShell module version 4.0 or later.</span></span> <span data-ttu-id="dfd7a-109">Kör ` Get-Module -ListAvailable AzureRM` för att hitta versionen.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-109">Run ` Get-Module -ListAvailable AzureRM` to find the version.</span></span> <span data-ttu-id="dfd7a-110">Om du behöver installera eller uppgradera kan du läsa [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps) (Installera Azure PowerShell-modul).</span><span class="sxs-lookup"><span data-stu-id="dfd7a-110">If you need to install or upgrade, see [Install Azure PowerShell module](/powershell/azure/install-azurerm-ps).</span></span> 

## <a name="log-in-to-azure"></a><span data-ttu-id="dfd7a-111">Logga in på Azure</span><span class="sxs-lookup"><span data-stu-id="dfd7a-111">Log in to Azure</span></span>

<span data-ttu-id="dfd7a-112">Logga in på Azure-prenumerationen med kommandot [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) och följ anvisningarna på skärmen.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-112">Log in to your Azure subscription using the [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) command and follow the on-screen directions.</span></span>

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a><span data-ttu-id="dfd7a-113">Skapa variabler</span><span class="sxs-lookup"><span data-stu-id="dfd7a-113">Create variables</span></span>

<span data-ttu-id="dfd7a-114">Definiera variabler för användning i skripten i den här snabbstartsguiden.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-114">Define variables for use in the scripts in this quick start.</span></span>

```powershell
# The data center and resource name for your resources
$resourcegroupname = "myResourceGroup"
$location = "WestEurope"
# The logical server name: Use a random value or replace with your own value (do not capitalize)
$servername = "server-$(Get-Random)"
# Set an admin login and password for your database
# The login information for the server
$adminlogin = "ServerAdmin"
$password = "ChangeYourAdminPassword1"
# The ip address range that you want to allow to access your server - change as appropriate
$startip = "0.0.0.0"
$endip = "0.0.0.0"
# The database name
$databasename = "mySampleDatabase"
```

## <a name="create-a-resource-group"></a><span data-ttu-id="dfd7a-115">Skapa en resursgrupp</span><span class="sxs-lookup"><span data-stu-id="dfd7a-115">Create a resource group</span></span>

<span data-ttu-id="dfd7a-116">Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med kommandot [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup).</span><span class="sxs-lookup"><span data-stu-id="dfd7a-116">Create an [Azure resource group](../azure-resource-manager/resource-group-overview.md) using the [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) command.</span></span> <span data-ttu-id="dfd7a-117">En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-117">A resource group is a logical container into which Azure resources are deployed and managed as a group.</span></span> <span data-ttu-id="dfd7a-118">I följande exempel skapas en resursgrupp med namnet `myResourceGroup` på platsen `westeurope`.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-118">The following example creates a resource group named `myResourceGroup` in the `westeurope` location.</span></span>

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a><span data-ttu-id="dfd7a-119">Skapa en logisk server</span><span class="sxs-lookup"><span data-stu-id="dfd7a-119">Create a logical server</span></span>

<span data-ttu-id="dfd7a-120">Skapa en logisk [Azure SQL Database-server](sql-database-features.md) med kommandot [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver).</span><span class="sxs-lookup"><span data-stu-id="dfd7a-120">Create an [Azure SQL Database logical server](sql-database-features.md) using the [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) command.</span></span> <span data-ttu-id="dfd7a-121">En logisk server innehåller en uppsättning databaser som hanteras som en grupp.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-121">A logical server contains a group of databases managed as a group.</span></span> <span data-ttu-id="dfd7a-122">I följande exempel skapas en server med ett slumpmässigt namn i resursgruppen med en administratörsinloggning med namnet `ServerAdmin` och lösenordet `ChangeYourAdminPassword1`.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-122">The following example creates a randomly named server in your resource group with an admin login named `ServerAdmin` and a password of `ChangeYourAdminPassword1`.</span></span> <span data-ttu-id="dfd7a-123">Ersätt dessa fördefinierade värden efter behov.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-123">Replace these pre-defined values as desired.</span></span>

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a><span data-ttu-id="dfd7a-124">Konfigurera en serverbrandväggsregel</span><span class="sxs-lookup"><span data-stu-id="dfd7a-124">Configure a server firewall rule</span></span>

<span data-ttu-id="dfd7a-125">Skapa en ny brandväggsregel på [Azure SQL Database-servernivå](sql-database-firewall-configure.md) med kommandot [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule).</span><span class="sxs-lookup"><span data-stu-id="dfd7a-125">Create an [Azure SQL Database server-level firewall rule](sql-database-firewall-configure.md) using the [New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) command.</span></span> <span data-ttu-id="dfd7a-126">En brandväggsregel på servernivå tillåter att ett externt program, t.ex. SQL Server Management Studio eller SQLCMD-verktyget, ansluter till en SQL-databas visa SQL Database-tjänstens brandvägg.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-126">A server-level firewall rule allows an external application, such as SQL Server Management Studio or the SQLCMD utility to connect to a SQL database through the SQL Database service firewall.</span></span> <span data-ttu-id="dfd7a-127">I följande exempel öppnas brandväggen bara för andra Azure-resurser.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-127">In the following example, the firewall is only opened for other Azure resources.</span></span> <span data-ttu-id="dfd7a-128">Aktivera extern anslutning, ändra IP-adressen till en adress som är lämplig för din miljö.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-128">To enable external connectivity, change the IP address to an appropriate address for your environment.</span></span> <span data-ttu-id="dfd7a-129">Öppna alla IP-adresser genom att använda 0.0.0.0 som den första IP-adressen och 255.255.255.255 som slutadress.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-129">To open all IP addresses, use 0.0.0.0 as the starting IP address and 255.255.255.255 as the ending address.</span></span>

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> <span data-ttu-id="dfd7a-130">SQL Database kommunicerar via port 1433.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-130">SQL Database communicates over port 1433.</span></span> <span data-ttu-id="dfd7a-131">Om du försöker ansluta inifrån ett företagsnätverk, kan utgående trafik via port 1433 nekas av nätverkets brandvägg.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-131">If you are trying to connect from within a corporate network, outbound traffic over port 1433 may not be allowed by your network's firewall.</span></span> <span data-ttu-id="dfd7a-132">I så fall kan du inte ansluta till din Azure SQL Database-server om din IT-avdelning inte öppnar port 1433.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-132">If so, you will not be able to connect to your Azure SQL Database server unless your IT department opens port 1433.</span></span>
>

## <a name="create-a-database-in-the-server-with-sample-data"></a><span data-ttu-id="dfd7a-133">Skapa en databas på servern med exempeldata</span><span class="sxs-lookup"><span data-stu-id="dfd7a-133">Create a database in the server with sample data</span></span>

<span data-ttu-id="dfd7a-134">Skapa en databas med en [S0-prestandanivå](sql-database-service-tiers.md) i servern med kommandot [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase).</span><span class="sxs-lookup"><span data-stu-id="dfd7a-134">Create a database with an [S0 performance level](sql-database-service-tiers.md) in the server using the [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) command.</span></span> <span data-ttu-id="dfd7a-135">Följande exempel skapar en databas med namnet `mySampleDatabase` och läser in AdventureWorksLT-exempeldata till den här databasen.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-135">The following example creates a database called `mySampleDatabase` and loads the AdventureWorksLT sample data into this database.</span></span> <span data-ttu-id="dfd7a-136">Ersätt de fördefinierade värdena efter behov (andra snabbstarter i den här samlingen bygger på värdena i den här snabbstarten).</span><span class="sxs-lookup"><span data-stu-id="dfd7a-136">Replace these predefined values as desired (other quick starts in this collection build upon the values in this quick start).</span></span>

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a><span data-ttu-id="dfd7a-137">Rensa resurser</span><span class="sxs-lookup"><span data-stu-id="dfd7a-137">Clean up resources</span></span>

<span data-ttu-id="dfd7a-138">Andra snabbstarter i den här samlingen bygger på den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-138">Other quick starts in this collection build upon this quick start.</span></span> 

> [!TIP]
> <span data-ttu-id="dfd7a-139">Om du planerar att fortsätta att arbeta med efterföljande snabbstarter, ska du inte rensa resurserna som skapades i den här snabbstarten.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-139">If you plan to continue on to work with subsequent quick starts, do not clean up the resources created in this quick start.</span></span> <span data-ttu-id="dfd7a-140">Om du inte planerar att fortsätta kan du använda stegen nedan för att ta bort alla resurser som har skapats i den här snabbstarten i Azure Portal.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-140">If you do not plan to continue, use the following steps to delete all resources created by this quick start in the Azure portal.</span></span>
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a><span data-ttu-id="dfd7a-141">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dfd7a-141">Next steps</span></span>

<span data-ttu-id="dfd7a-142">Nu när du har en databas kan du ansluta och söka med dina favoritverktyg.</span><span class="sxs-lookup"><span data-stu-id="dfd7a-142">Now that you have a database, you can connect and query using your favorite tools.</span></span> <span data-ttu-id="dfd7a-143">Lär dig mer genom att välja verktyg nedan:</span><span class="sxs-lookup"><span data-stu-id="dfd7a-143">Learn more by choosing your tool below:</span></span>

- [<span data-ttu-id="dfd7a-144">SQL Server Management Studio</span><span class="sxs-lookup"><span data-stu-id="dfd7a-144">SQL Server Management Studio</span></span>](sql-database-connect-query-ssms.md)
- [<span data-ttu-id="dfd7a-145">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="dfd7a-145">Visual Studio Code</span></span>](sql-database-connect-query-vscode.md)
- [<span data-ttu-id="dfd7a-146">NET</span><span class="sxs-lookup"><span data-stu-id="dfd7a-146">.NET</span></span>](sql-database-connect-query-dotnet.md)
- [<span data-ttu-id="dfd7a-147">PHP</span><span class="sxs-lookup"><span data-stu-id="dfd7a-147">PHP</span></span>](sql-database-connect-query-php.md)
- [<span data-ttu-id="dfd7a-148">Node.js</span><span class="sxs-lookup"><span data-stu-id="dfd7a-148">Node.js</span></span>](sql-database-connect-query-nodejs.md)
- [<span data-ttu-id="dfd7a-149">Java</span><span class="sxs-lookup"><span data-stu-id="dfd7a-149">Java</span></span>](sql-database-connect-query-java.md)
- [<span data-ttu-id="dfd7a-150">Python</span><span class="sxs-lookup"><span data-stu-id="dfd7a-150">Python</span></span>](sql-database-connect-query-python.md)
- [<span data-ttu-id="dfd7a-151">Ruby</span><span class="sxs-lookup"><span data-stu-id="dfd7a-151">Ruby</span></span>](sql-database-connect-query-ruby.md)

