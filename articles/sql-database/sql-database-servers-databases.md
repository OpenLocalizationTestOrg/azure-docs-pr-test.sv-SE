---
title: aaaCreate & Hantera Azure SQL-servrar och databaser | Microsoft Docs
description: "Lär dig mer om Azure SQL Database-server och databasbegrepp och skapa och hantera servrar och databaser med hjälp av hello Azure-portalen, PowerShell, hello Azure CLI, Transact-SQL och hello REST API."
services: sql-database
documentationcenter: na
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/05/2017
ms.author: carlrab
ms.openlocfilehash: 0f526e388a5a620349f5a14e8d57a8355ac451ee
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="create-and-manage-azure-sql-database-servers-and-databases"></a>Skapa och hantera Azure SQL Database-servrar och databaser

En Azure SQL database är en hanterad databas i Microsoft Azure som skapas i en [Azure-resursgrupp](../azure-resource-manager/resource-group-overview.md) med en definierad uppsättning [beräkning och lagring resurser för olika arbetsbelastningar](sql-database-service-tiers.md). En Azure SQL database är associerad med en logisk server från Azure SQL Database som skapas i en viss Azure-region. 

## <a name="an-azure-sql-database-can-be-a-single-pooled-or-partitioned-database"></a>En Azure SQL database kan vara en enskild, grupperade eller partitionerade databas

En Azure SQL database kan vara:

- En enkel databas med dess [uppsättning resurser](sql-database-what-is-a-dtu.md#what-are-database-transaction-units-dtus) (DTU:er)
- En del av en [elastiska SQL-poolen](sql-database-elastic-pool.md) som [delar en uppsättning resurser](sql-database-what-is-a-dtu.md#what-are-elastic-database-transaction-units-edtus) (edtu: er)
- En del av en [utskalad uppsättning delade databaser](sql-database-elastic-scale-introduction.md#horizontal-and-vertical-scaling), som kan vara enskilda databaser eller databaser i en pool
- En del av en uppsättning databaser som ingår i ett [SaaS-designmönster för flera klienter](sql-database-design-patterns-multi-tenancy-saas-applications.md), vars databaser kan vara enskilda databaser eller databaser i en pool (eller båda) 

> [!TIP]
> För giltiga databasnamn, se [databasidentifierare](https://docs.microsoft.com/en-us/sql/relational-databases/databases/database-identifiers). 
>
 
- hello databasen Standardsortering används av Microsoft Azure SQL Database är **SQL_LATIN1_GENERAL_CP1_CI_AS**, där **LATIN1_GENERAL** är engelska (USA) **CP1** är teckentabellen 1252, **CI** är skiftlägeskänslig, och **AS** är accentkänsliga. Mer information om hur tooset hello sortering finns [COLLATE (Transact-SQL)](https://msdn.microsoft.com/library/ms184391.aspx).
- Microsoft Azure SQL Database stöder tabelldata dataström (TDS) protokollet klientversionen 7.3 eller senare.
- TCP/IP-anslutningar tillåts.

## <a name="what-is-an-azure-sql-logical-server"></a>Vad är en logisk Azure SQL-server?

En logisk server som fungerar som en central administrativ plats för flera databaser, inklusive [SQL elastiska pooler](sql-database-elastic-pool.md) [inloggningar](sql-database-manage-logins.md), [regler i brandväggen](sql-database-firewall-configure.md), [granskning regler](sql-database-auditing.md), [hot principer](sql-database-threat-detection.md), och [redundans grupper](sql-database-geo-replication-overview.md). En logisk server kan vara i en annan region än dess resursgruppen. hello logisk server måste finnas innan du kan skapa hello Azure SQL-databas. Alla databaser på en server som skapas i hello samma region som hello logisk server. 


> [!IMPORTANT]
> En server är en logisk konstruktion som skiljer sig från en SQL Server-instans som du kanske känner till lokala hälsningsmeddelande i SQL-databas. Mer specifikt hello SQL Database-tjänsten ger inga garantier om platsen för hello databaser i relationen tootheir logiska servrar och visar inga instansnivå åtkomst eller funktioner.
> 

När du skapar en logisk server kan ange du en server inloggningskonto och lösenord som har administrativa rättigheter toohello master-databasen på den servern och alla databaser som skapas på servern. Det här första kontot är ett konto för SQL-inloggning. Azure SQL Database stöder SQL-autentisering och Azure Active Directory-autentisering för autentisering. Information om inloggning och autentisering finns [hantera databaser och inloggningar i Azure SQL Database](sql-database-manage-logins.md). Windows-autentisering stöds inte. 

> [!TIP]
> Giltig resurs grupp- och servernamnen finns [namngivning av regler och begränsningar](https://docs.microsoft.com/azure/architecture/best-practices/naming-conventions).
>

En logisk Azure Database-server:

- Skapas i en Azure-prenumeration, men kan flyttas med befintliga resurser tooanother prenumerationen
- Är hello överordnade resurs för databaser och elastiska pooler datalager
- Ger ett namnområde för databaser och elastiska pooler datalager
- En logisk behållare med starka livstid semantik - ta bort en server och tar bort är hello inneslutna databaser och elastiska pooler datalager
- Deltar i [Azure rollbaserad åtkomstkontroll (RBAC)](/active-directory/role-based-access-control-what-is) -databaser och elastiska pooler datalager på en server ärver behörigheter från hello-server
- Är ett högsta-element för hello identiteten för databaser och elastiska pooler datalager för Azure-resurs hanteringsändamål (se hello URL-schema för databaser och pooler)
- Samlar resurser i en region.
- Tillhandahåller en anslutningsslutpunkt för databasåtkomst (<serverName>.database.windows.net)
- Tillhandahåller åtkomst toometadata om befintliga resurser via av DMV: er av anslutande tooa master-databasen 
- Ger hello omfång för principer för hantering som gäller tooits databaser - inloggningar, brandvägg, granska, hot identifiering osv. 
- Begränsas av en kvot i hello överordnade prenumerationen (sex servrar per prenumeration som standard - [finns prenumeration begränsar här](../azure-subscription-service-limits.md))
- Ger hello Omfattningen för databaskvoten och DTU-kvot för hello resurser som den innehåller (till exempel 45 000 DTU)
- Är hello versionshantering omfattning för funktioner på befintliga resurser 
- Huvudkontoinloggningar på servernivå kan hantera alla databaser på en server.
- Kan innehålla inloggningar liknande toothose i instanser av SQL Server på din lokala som beviljas åtkomst tooone eller flera databaser med hello server och kan vara begränsad beviljas administratörsbehörighet. Mer information finns i avsnittet om [inloggningar](sql-database-manage-logins.md).

## <a name="azure-sql-databases-protected-by-sql-database-firewall"></a>Azure SQL-databaser som skyddas av Brandvägg för SQL-databas

toohelp skydda dina data, en [SQL Database-brandvägg](sql-database-firewall-configure.md) förhindrar alla åtkomst tooyour database-server eller någon av dess databaser från utanför din anslutning toohello server direkt via Azure-prenumeration anslutningen. tooenable ytterligare anslutningar, måste du [skapa brandväggsregler för en eller flera](sql-database-firewall-configure.md#creating-and-managing-firewall-rules). Skapa och hantera SQL elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-hello-azure-portal"></a>Hantera Azure SQL-servrar, databaser och brandväggar med hello Azure-portalen

Du kan skapa hello Azure SQL database resursgrupp i förväg eller när du skapar hello-servern. Det finns flera metoder för att hämta tooa nya SQL server formuläret, antingen genom att skapa en ny SQLServer eller som en del av att skapa en ny databas. 

### <a name="create-a-blank-sql-server-logical-server"></a>Skapa en tom SQLServer (logisk server)

en Azure SQL Database-server (utan en databas) med hjälp av toocreate hello [Azure-portalen](https://portal.azure.com), navigera tooa tomt SQL server (logisk server) formulär. hello följande skärmbild visar en metod för att öppna ett formulär toocreate en tom logisk SQL-server. 

   ![Skapa logisk server slutförts formulär](./media/sql-database-migrate-your-sql-server-database/logical-server-create-completed.png)

Om du får toothis formuläret med hjälp av en annan metod är hello information på formuläret hello identiska.

### <a name="create-a-blank-or-sample-sql-database"></a>Skapa en tom eller exempel SQL-databas

en Azure SQL database med toocreate hello [Azure-portalen](https://portal.azure.com), navigera tooa tomt formulär för SQL-databas och tillhandahålla hello information som efterfrågas. Du kan skapa hello Azure SQL database resursgruppen och logiska server i förväg eller när du skapar själva hello-databasen. Du kan skapa en tom databas eller skapa en exempeldatabas baserat på Adventure Works med 

  ![skapa databas-1](./media/sql-database-get-started-portal/create-database-1.png)

> [VIKTIGA] Information om hur du väljer hello prisnivån för din databas finns [tjänstnivåer](sql-database-service-tiers.md).
>

### <a name="manage-an-existing-sql-server"></a>Hantera en befintlig SQLServer

toomanage en befintlig server navigera toohello servern med hjälp av ett antal metoder - exempel från och med den specifika SQL-databas, hello **SQL-servrar** sida eller hello **alla resurser** sidan. följande skärmbild visar hur hello toobegin inställning en brandvägg på servernivå från hello **översikt** sida för en server. 

   ![Översikt över logisk server](./media/sql-database-migrate-your-sql-server-database/logical-server-overview.png)

toomanage en befintlig databas går toohello **SQL-databaser** och klickar på hello-databasen som du vill toomanage. följande skärmbild visar hur hello toobegin inställning servernivå Brandvägg för en databas från hello **översikt** sida för en databas. 

   ![brandväggsregler för server](./media/sql-database-get-started-portal/server-firewall-rule.png) 

> [!IMPORTANT]
> tooconfigure prestanda egenskaper för en databas finns [tjänstnivåer](sql-database-service-tiers.md).
>

> [!TIP]
> En självstudiekurs i Azure portal Snabbstart finns [skapa en Azure SQL database i hello Azure-portalen](sql-database-get-started-portal.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-powershell"></a>Hantera Azure SQL-servrar, databaser och brandväggar med PowerShell

toocreate och hantera Azure SQL server, databaser och brandväggar med Azure PowerShell, Använd hello följande PowerShell-cmdlets. Om du behöver tooinstall eller uppgradera PowerShell Se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps). Skapa och hantera SQL elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).

| Cmdlet | Beskrivning |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Skapar en databas |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Hämtar en eller flera databaser|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Anger egenskaperna för en databas eller flyttar en befintlig databas till en elastisk pool|
|[Ta bort-AzureRmSqlDatabase](/powershell/module/azurerm.sql/remove-azurermsqldatabase)|Tar bort en databas|
|[Ny AzureRmResourceGroup](/powershell/module/azurerm.resources/new-azurermresourcegroup)|Skapar en resursgrupp]
|[Ny AzureRmSqlServer](/powershell/module/azurerm.sql/new-azurermsqlserver)|Skapar en server|
|[Get-AzureRmSqlServer](/powershell/module/azurerm.sql/get-azurermsqlserver)|Returnerar information om servrar|
|[Set-AzureRmSqlServer](https://docs.microsoft.com/en-us/powershell/module/azurerm.sql/set-azurermsqlserver)|Ändrar egenskaperna för en server|
|[Ta bort AzureRmSqlServer](/powershell/module/azurerm.sql/remove-azurermsqlserver)|Tar bort en server|
|[New-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/new-azurermsqlserverfirewallrule)|Skapar en brandväggsregel på servernivå |
|[Get-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/get-azurermsqlserverfirewallrule)|Hämtar brandväggsregler för en server|
|[Set-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/set-azurermsqlserverfirewallrule)|Ändrar en brandväggsregel på en server|
|[Remove-AzureRmSqlServerFirewallRule](/powershell/module/azurerm.sql/remove-azurermsqlserverfirewallrule)|Tar bort en brandväggsregel från en server.|

> [!TIP]
> En PowerShell Snabbstartsguide finns [skapa en enda Azure SQL-databas med hjälp av PowerShell](sql-database-get-started-portal.md). PowerShell-exempelskript, finns [toocreate Använd PowerShell en enda Azure SQL-databas och konfigurera en brandväggsregel](scripts/sql-database-create-and-configure-database-powershell.md) och [Övervakare och skala en enskild SQL-databas med hjälp av PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-hello-azure-cli"></a>Hantera Azure SQL-servrar, databaser och brandväggar med hello Azure CLI

toocreate och hantera Azure SQL server-databaser och brandväggar med hello [Azure CLI](/cli/azure/overview), använder hello följande [Azure CLI SQL Database](/cli/azure/sql/db) kommandon. Använd hello [moln Shell](/azure/cloud-shell/overview) toorun hello CLI i webbläsaren eller [installera](/cli/azure/install-azure-cli) på macOS, Linux och Windows. Skapa och hantera SQL elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).

| Cmdlet | Beskrivning |
| --- | --- |
|[Skapa AZ sql-databas](/cli/azure/sql/db#create) |Skapar en databas|
|[AZ sql db-lista](/cli/azure/sql/db#list)|Visar en lista över alla databaser och datalager i en server eller alla databaser i en elastisk pool|
|[AZ sql db lista-versioner](/cli/azure/sql/db#list-editions)|Visar tillgängliga mål och Lagringsgränser|
|[AZ sql db lista-användningsområden](/cli/azure/sql/db#list-usages)|Returnerar databasen användningsområden|
|[AZ sql db visa](/cli/azure/sql/db#show)|Hämtar ett databasen eller data warehouse|
|[AZ sql db-uppdateringen](/cli/azure/sql/db#update)|Uppdaterar en-databas|
|[AZ sql db bort](/cli/azure/sql/db#delete)|Tar bort en databas|
|[Skapa AZ grupp](/cli/azure/group#create)|Skapar en resursgrupp|
|[Skapa AZ SQLServer](/cli/azure/sql/server#create)|Skapar en server|
|[AZ sql server-lista](/cli/azure/sql/server#list)|Visar servrar|
|[AZ sql server lista-användningsområden](/cli/azure/sql/server#list-usages)|Returnerar servern användningsområden|
|[Visa för AZ sql server](/cli/azure/sql/server#show)|Hämtar en server|
|[AZ sql server-uppdatering](/cli/azure/sql/server#update)|Uppdaterar en server|
|[ta bort AZ sql-server](/cli/azure/sql/server#delete)|Tar bort en server|
|[Skapa AZ sql server-brandväggsregel](/cli/azure/sql/server/firewall-rule#create)|Skapar en brandväggsregel|
|[AZ sql server-brandväggsregel lista](/cli/azure/sql/server/firewall-rule#list)|Visar hello brandväggsregler på en server|
|[AZ sql server-brandväggsregel visa](/cli/azure/sql/server/firewall-rule#show)|Visar hello detaljer för en brandväggsregel|
|[uppdatering av AZ sql server-brandväggsregel](/cli/azure/sql/server/firewall-rule#update)|Uppdaterar en brandväggsregel|
|[ta bort AZ sql server-brandväggsregel](/cli/azure/sql/server/firewall-rule#delete)|Tar bort en brandväggsregel|

> [!TIP]
> Läs en Azure CLI Snabbstartsguide [skapar en enda Azure SQL-databas med hello Azure CLI](sql-database-get-started-cli.md). Azure CLI exempelskript finns [Använd CLI toocreate en enda Azure SQL-databas och konfigurera en brandväggsregel](scripts/sql-database-create-and-configure-database-cli.md) och [Använd CLI toomonitor och skala en enskild SQL-databas](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-transact-sql"></a>Hantera Azure SQL-servrar, databaser och brandväggar med Transact-SQL

toocreate och hantera Azure SQL server, databaser och brandväggar med Transact-SQL, använda hello följande T-SQL-kommandon. Du kan skicka dessa kommandon använder hello Azure-portalen [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), eller andra program som kan ansluta tooan Azure SQL Database-server och skicka Transact-SQL kommandon. För att hantera SQL elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).

> [!IMPORTANT]
> Du kan inte skapa eller ta bort en server med hjälp av Transact-SQL.
>

| Kommando | Beskrivning |
| --- | --- |
|[Skapa databas (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Skapar en ny databas. Du måste vara anslutna toohello huvuddatabasen toocreate en ny databas.|
| [ALTER DATABASE (Azure SQL-databas)](/sql/t-sql/statements/alter-database-azure-sql-database) |Ändrar en Azure SQL database. |
|[ALTER DATABASE (Azure SQL Data Warehouse)](/sql/t-sql/statements/alter-database-azure-sql-data-warehouse)|Ändrar ett Azure SQL Data Warehouse.|
|[Ta bort databasen (Transact-SQL)](/sql/t-sql/statements/drop-database-transact-sql)|Tar bort en databas.|
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Returnerar hello edition (tjänstnivån), tjänstmålet (prisnivån) och namn på elastisk pool, för en Azure SQL-databas eller ett Azure SQL Data Warehouse. Returnerar information om alla databaser om inloggad toohello master-databasen i en Azure SQL Database-server. Du måste vara anslutna toohello huvuddatabasen för Azure SQL Data Warehouse.|
|[sys.dm_db_resource_stats (Azure SQL Database)](/sql/relational-databases/system-dynamic-management-views/sys-dm-db-resource-stats-azure-sql-database)| Returnerar förbrukning av CPU, i/o och minne för en Azure SQL Database-databas. Det finns en rad för var 15: e sekund, även om det inte sker i hello-databasen.|
|[sys.resource_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-resource-stats-azure-sql-database)|Returnerar CPU-användning och lagring data för en Azure SQL Database. hello data som samlas in och sammanställs inom fem-minuters mellanrum.|
|[sys.database_connection_stats (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-connection-stats-azure-sql-database)|Innehåller statistik för SQL Database connectivity databashändelser, ger en översikt av databasen anslutning framgångar och misslyckanden. |
|[sys.event_log (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-event-log-azure-sql-database)|Returnerar lyckade Azure SQL Database-databasanslutningar anslutningsfel och deadlocks. Du kan använda denna information tootrack eller felsöka din Databasaktivitet med SQL-databas.|
|[sp_set_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-firewall-rule-azure-sql-database)|Skapar eller uppdaterar hello servernivå brandväggsinställningar för SQL Database-server. Den här lagrade proceduren finns bara i huvudsaklig inloggning på servernivå hello huvuddatabasen toohello. En brandväggsregel på servernivå kan bara skapas med hjälp av Transact-SQL efter hello första servernivå brandväggsregel har skapats av en användare med Azure-behörighet|
|[sys.firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-firewall-rules-azure-sql-database)|Returnerar information om hello servernivå brandväggsinställningar som är associerade med Microsoft Azure SQL Database.|
|[sp_delete_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-firewall-rule-azure-sql-database)|Tar bort servernivå brandväggsinställningar från din SQL Database-server. Den här lagrade proceduren finns bara i huvudsaklig inloggning på servernivå hello huvuddatabasen toohello.|
|[sp_set_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-set-database-firewall-rule-azure-sql-database)|Skapar eller uppdaterar hello databasnivå brandväggsregler för din Azure SQL Database eller SQL Data Warehouse. Databasens brandväggsregler kan konfigureras för hello master-databasen och databaserna på SQL-databas. Databasens brandväggsregler är användbara när du använder finns databasanvändare. |
|[sys.database_firewall_rules (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-firewall-rules-azure-sql-database)|Returnerar information om hello databasnivå brandväggsinställningar som är associerade med Microsoft Azure SQL Database. |
|[sp_delete_database_firewall_rule (Azure SQL Database)](/sql/relational-databases/system-stored-procedures/sp-delete-database-firewall-rule-azure-sql-database)|Tar bort databasnivå brandväggsinställningen från din Azure SQL Database eller SQL Data Warehouse. |


> [!TIP]
> Snabbstartsguide med SQL Server Management Studio på Microsoft Windows, se [Azure SQL Database: Använd SQL Server Management Studio tooconnect och fråga data](sql-database-connect-query-ssms.md). En självstudiekurs med Visual Studio-koden på hello macOS, Linux eller Windows, se [Azure SQL Database: Använd Visual Studio Code tooconnect och fråga data](sql-database-connect-query-vscode.md).

## <a name="manage-azure-sql-servers-databases-and-firewalls-using-hello-rest-api"></a>Hantera Azure SQL-servrar, databaser och brandväggar med hello REST API

toocreate och hantera Azure SQL server, databaser och brandväggar med hello REST-API, se [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Nästa steg

- toolearn om poolning databaser med SQL elastiska pooler, se [elastiska pooler](sql-database-elastic-pool.md).
- Information om hello Azure SQL Database-tjänsten finns [vad är SQL Database?](sql-database-technical-overview.md).
- toolearn om att migrera en SQL Server-databasen tooAzure finns [migrera tooAzure SQL-databas](sql-database-cloud-migrate.md).
- Information om vilka funktioner som stöds finns i avsnittet [Funktioner](sql-database-features.md).
