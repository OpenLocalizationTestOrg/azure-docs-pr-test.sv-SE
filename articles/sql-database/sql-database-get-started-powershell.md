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
# <a name="create-a-single-azure-sql-database-using-powershell"></a>Skapa en enskild Azure SQL-databas med PowerShell

PowerShell är används toocreate och hantera Azure-resurser från hello kommandoraden eller i skript. Detta hjälper information med hjälp av PowerShell toodeploy en Azure SQL-databas i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) i en [logisk Azure SQL Database-server](sql-database-features.md).

Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt](https://azure.microsoft.com/free/) konto innan du börjar.

Den här kursen kräver hello Azure PowerShell Modulversion 4.0 eller senare. Kör ` Get-Module -ListAvailable AzureRM` toofind hello version. Om du behöver tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps). 

## <a name="log-in-tooazure"></a>Logga in tooAzure

Logga in tooyour Azure-prenumeration med hjälp av hello [Add-AzureRmAccount](/powershell/module/azurerm.profile/add-azurermaccount) kommando och följ hello på skärmen riktningar.

```powershell
Add-AzureRmAccount
```

## <a name="create-variables"></a>Skapa variabler

Definiera variabler för användning i hello skript i denna Snabbstart.

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

## <a name="create-a-resource-group"></a>Skapa en resursgrupp

Skapa en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med hello [New-AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup) kommando. En resursgrupp är en logisk behållare där Azure-resurser distribueras och hanteras som en grupp. hello följande exempel skapar en resursgrupp med namnet `myResourceGroup` i hello `westeurope` plats.

```powershell
New-AzureRmResourceGroup -Name $resourcegroupname -Location $location
```
## <a name="create-a-logical-server"></a>Skapa en logisk server

Skapa en [logisk Azure SQL Database-server](sql-database-features.md) med hello [New-AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver) kommando. En logisk server innehåller en uppsättning databaser som hanteras som en grupp. hello följande exempel skapas en slumpmässigt namn i resursgruppen med en administratörsinloggning med namnet `ServerAdmin` och lösenordet `ChangeYourAdminPassword1`. Ersätt dessa fördefinierade värden efter behov.

```powershell
New-AzureRmSqlServer -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -Location $location `
    -SqlAdministratorCredentials $(New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $adminlogin, $(ConvertTo-SecureString -String $password -AsPlainText -Force))
```

## <a name="configure-a-server-firewall-rule"></a>Konfigurera en serverbrandväggsregel

Skapa en [Azure SQL Database brandväggsregel på servernivå](sql-database-firewall-configure.md) med hello [ny AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule) kommando. En brandväggsregel på servernivå kan ett externt program, till exempel SQL Server Management Studio eller hello SQLCMD-verktyget tooconnect tooa SQL-databasen via hello SQL Database-tjänsten brandvägg. I följande exempel hello, öppnas hello brandväggen bara för andra Azure-resurser. tooenable extern anslutning, ändra hello IP-adress tooan korrekt adress för din miljö. tooopen alla IP-adresser, använda 0.0.0.0 som hello startar IP-adress och 255.255.255.255 som hello slutadress.

```powershell
New-AzureRmSqlServerFirewallRule -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -FirewallRuleName "AllowSome" -StartIpAddress $startip -EndIpAddress $endip
```

> [!NOTE]
> SQL Database kommunicerar via port 1433. Om du försöker tooconnect från ett företagsnätverk, tillåtas utgående trafik via port 1433 inte av ditt nätverks brandvägg. I så fall, blir inte kan tooconnect tooyour Azure SQL Database-server om din IT-avdelning öppnar port 1433.
>

## <a name="create-a-database-in-hello-server-with-sample-data"></a>Skapa en databas i hello server med exempeldata

Skapa en databas med en [prestandanivå S0](sql-database-service-tiers.md) i hello-servern med hjälp av hello [New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase) kommando. hello följande exempel skapar en databas som heter `mySampleDatabase` och belastningar hello AdventureWorksLT exempeldata till den här databasen. Ersätt de fördefinierade värden efter behov (andra snabb startar i den här samlingen build vid hello värdena i den här snabbstartsguide).

```powershell
New-AzureRmSqlDatabase  -ResourceGroupName $resourcegroupname `
    -ServerName $servername `
    -DatabaseName $databasename `
    -SampleName "AdventureWorksLT" `
    -RequestedServiceObjectiveName "S0"
```

## <a name="clean-up-resources"></a>Rensa resurser

Andra snabbstarter i den här samlingen bygger på den här snabbstarten. 

> [!TIP]
> Om du tänker toocontinue toowork med efterföljande snabbstarter, rensa hello resurser som skapas på den här quick starta inte. Om du inte planerar toocontinue använda hello efter steg toodelete alla resurser skapas av den här Snabbstart i hello Azure-portalen.
>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName $resourcegroupname
```

## <a name="next-steps"></a>Nästa steg

Nu när du har en databas kan du ansluta och söka med dina favoritverktyg. Lär dig mer genom att välja verktyg nedan:

- [SQL Server Management Studio](sql-database-connect-query-ssms.md)
- [Visual Studio Code](sql-database-connect-query-vscode.md)
- [NET](sql-database-connect-query-dotnet.md)
- [PHP](sql-database-connect-query-php.md)
- [Node.js](sql-database-connect-query-nodejs.md)
- [Java](sql-database-connect-query-java.md)
- [Python](sql-database-connect-query-python.md)
- [Ruby](sql-database-connect-query-ruby.md)

