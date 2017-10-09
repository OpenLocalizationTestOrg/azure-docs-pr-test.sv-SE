---
title: "Azure SQL Database-tjänsten och prestanda nivåer | Microsoft Docs"
description: "Jämför SQL Database servicenivåer och prestandanivåer för enskilda databaser och införa SQL elastiska pooler"
keywords: databasalternativ, databasprestanda
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: f5c5c596-cd1e-451f-92a7-b70d4916e974
ms.service: sql-database
ms.custom: DBs & servers
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 06/30/2017
ms.author: carlrab
ms.openlocfilehash: b27b78788614d32e6c0c4267fe0ce504ebd2f444
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-performance-options-are-available-for-an-azure-sql-database"></a>Vilka alternativ för prestanda är tillgängliga för en Azure SQL Database

[Azure SQL Database](sql-database-technical-overview.md) erbjuder fyra servicenivåer för både enkel och [grupperade](sql-database-elastic-pool.md) databaser. Dessa tjänstnivåer är: **grundläggande**, **Standard**, **Premium**, och **Premium RS**. Har flera prestandanivåer inom varje tjänstnivå ([dtu: er](sql-database-what-is-a-dtu.md)) och lagring toohandle olika arbetsbelastningar och data storlek. Högre prestandanivåer ger ytterligare beräkningsresurser och lagringsresurser utformad toodeliver allt högre genomflöde och kapacitet. Du kan ändra servicenivåer och prestandanivåer lagring dynamiskt utan driftavbrott. 
- **Grundläggande**, **Standard** och **Premium** tjänstnivåer som alla har en upptids-SLA på 99.99%, flexibla alternativ för verksamhetskontinuitet, säkerhetsfunktioner och timvis debitering. 
- Hej **Premium RS** nivå tillhandahåller hello samma prestandanivåer hello Premium-nivån med en minskad SLA eftersom den körs med ett lägre antal redundanta kopior än en databas i hello andra servicenivåer. Så i hello händelse för misslyckade tjänster, kanske du måste toorecover databasen från en säkerhetskopia med upp tooa 5 minuter fördröjning.

> [!IMPORTANT]
> En Azure SQL database får en garanterad uppsättning resurser och hello förväntade prestandaegenskaperna för din databas inte påverkas av en databas i Azure. 

## <a name="choosing-a-service-tier"></a>Välja tjänstnivå
hello innehåller följande tabell exempel på hello nivåer som bäst passar för olika programbelasningar.

| Tjänstenivå | Målbelastningar |
| :--- | --- |
| **Basic** | Bäst lämpad för små databaser som vanligtvis stöder en aktiv åtgärd åt gången. Exempelvis databaser som används för utveckling och testning, eller småskaliga program som inte används ofta. |
| **Standard** |Hej gå toooption för molnprogram med låg toomedium IO prestandakrav som stöder flera samtidiga frågor. Exempelvis arbetsgrupps- eller webbappar. |
| **Premium** | Utformad för höga transaktionsvolymer med höga IO-prestandakrav. Den här nivån har stöd för många samtidiga användare. Exempelvis databaser som stöder verksamhetskritiska program. |
| **Premium-RS** | Utformad för i/o-intensiva arbetsbelastningar som inte kräver hello högsta tillgänglighet garantier. Exempel innefattar testning högpresterande arbetsbelastningar eller en analytiska arbetsbelastningar där hello databasen inte är hello system för posten. |
|||

Du kan skapa enskilda databaser med dedicerade resurser i en tjänstnivå på en specifik [prestandanivå](sql-database-service-tiers.md#single-database-service-tiers-and-performance-levels) eller du kan också skapa databaser i en [elastiska SQL-poolen](sql-database-service-tiers.md#elastic-pool-service-tiers-and-performance-in-edtus). I en elastisk SQL-poolen delas hello beräkning och lagring resurser över flera databaser i en enskild logisk server. 

Resurser som är tillgängliga för enskilda databaser uttrycks i Database Transaction Units (Dtu) och resurser för en SQL-elastiska pooler uttrycks i elastiska Datatransaktionsenheter (edtu: er). Mer information om dtu: er och edtu: er Se [vad är dtu: er och edtu: er?](sql-database-what-is-a-dtu.md).

toodecide starta på en tjänstnivå genom att fastställa hello minsta databasfunktioner som du behöver:

| **Tjänstens nivå funktioner** | **Basic** | **Standard** | **Premium** | **Premium-RS**|
| :-- | --: | --: | --: | --: |
| Enskild databas som högsta storlek | 2 GB | 250 GB | 4 TB *  | 500 GB  |
| Storlek på maximalt elastisk pool | 156 GB | 2,9 TB | 4 TB * | 750 GB |
| Maximala databasstorleken i en elastisk pool | 2 GB | 250 GB | 500 GB | 500 GB |
| Maximalt antal databaser per pool | 500  | 500 | 100 | 100 |
| Maximal enskild databas dtu: er | 5 | 100 | 4000 | 1000 |
| Maximal dtu: er per databas i en elastisk pool | 5 | 3000 | 4000 | 1000 |
| Databasen period för lagring av säkerhetskopior. | 7 dagar | 35 dagar | 35 dagar | 35 dagar |
||||||

> [!IMPORTANT]
> Storage in too4 TB är tillgängliga i hello följande områden: oss East2, västra USA, oss Gov Virginia, Västeuropa, Tyskland Central, söder Östasien, östra, Östra Australien, Kanada Central och Kanada Öst. Se [aktuella 4 TB begränsningar](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize)
>

När du har bestämt hello lämpliga tjänstnivån, är du redo toodetermine hello prestandanivå (hello antalet dtu: er) och hello lagringsutrymmet för hello-databasen. 

- Hej S2 och S3 prestandanivåer i hello **Standard** nivå är ofta en bra utgångspunkt. 
- För databaser med hög CPU- eller -i/o-krav hello prestandanivåer i hello **Premium** nivå är hello rätt startpunkt. 
- Hej **Premium** nivå erbjuder mer Processorkraft och börjar på 10 x flera i/o jämfört med toohello högsta prestanda nivån i hello **Standard** nivå.
- Hej **PremiumRS** nivå erbjuder hello prestanda för hello **Premium** nivå till en lägre kostnad, men med ett reducerat SERVICENIVÅAVTAL.

> [!IMPORTANT]
> Granska hello [SQL elastiska pooler](sql-database-elastic-pool.md) avsnittet hello information om gruppera databaser i SQL-elastiska pooler tooshare beräkning och lagring resurser. hello resten av det här avsnittet fokuserar på servicenivåer och prestandanivåer för enskilda databaser.
>

## <a name="single-database-service-tiers-and-performance-levels"></a>Servicenivåer och prestandanivåer för enskilda databaser
För enskilda databaser finns flera prestandanivåer och lagring belopp inom varje tjänstnivå. 

[!INCLUDE [SQL DB service tiers table](../../includes/sql-database-service-tiers-table.md)]

## <a name="scaling-up-or-scaling-down-a-single-database"></a>Skala upp eller skala ned en enkel databas

När du har valt en tjänstnivå och en prestandanivå kan du skala upp eller ned en enkel databas dynamiskt utifrån det faktiska resultatet.  

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-dynamically-scale-up-or-scale-down/player]
>

Ändra hello nivå eller prestanda tjänstenivå för en databas skapar en replik av hello originaldatabasen på hello nya prestandanivå och sedan växlar anslutningar toohello repliken. Inga data går förlorade under den här processen men under hello kort stund när vi växlar över toohello repliken, anslutningar toohello databasen är inaktiverade, så vissa transaktioner som rör sig kan återställas. hello tidslängd för hello växla varierar, men är vanligtvis under 4 sekunder är mindre än 30 sekunder 99% hello tid. Om det finns stora mängder transaktioner som rör sig vid hello anslutningar för tillfället är inaktiverade, hello tidslängd för hello växla kanske inte längre.  

hello hela skala upp processen hello varaktighet beror på båda hello storlek och tjänstnivån för hello databasen före och efter hello ändra. Till exempel bör en 250 GB-databas som ändras till, från eller inom en standard-tjänstnivå slutföras inom 6 timmar. För en databas med hello samma storlek som ändras prestandanivåer inom hello premiumnivån bör slutföras inom tre timmar.

> [!TIP]
> toocheck på hello status för pågående SQL-databas skalning åtgärden som du kan använda följande fråga hello: ```select * from sys.dm_operation_status```.
>

* Om du uppgraderar tooa högre nivå eller prestanda servicenivå ökar hello maximala databasstorleken inte om du inte uttryckligen anger en större maximal storlek.
* toodowngrade en databas hello databasen måste vara mindre än hello högsta tillåtna storleken hello mål tjänstnivån. 
* När du uppgraderar en databas med [georeplikering](sql-database-geo-replication-portal.md) aktiverad, uppgradera den sekundära databaser toohello önskade prestandanivån innan du uppgraderar hello primära databasen (allmän vägledning). När du uppgraderar tooa olika krävs edition ugrading hello sekundära databasen först. 
* När nedgradera en databas med [georeplikering](sql-database-geo-replication-portal.md) aktiverad, nedgradera dess primära databaser toohello önskade prestandanivå innan du nedgraderar hello sekundär databas (allmän vägledning). När nedgradera tooa olika krävs edition lägre hello primära databasen först. 

* hello återställning Tjänsterbjudanden är olika för olika tjänstnivåer hello. Om du nedgradera toohello **grundläggande** nivån du har en lägre säkerhetskopiering kvarhållningsperiod - finns [Azure SQL Database-säkerhetskopiering](sql-database-automated-backups.md).
* hello nya egenskaper för hello databasen verkställs inte förrän hello ändringarna är klara.


## <a name="current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize"></a>Aktuella begränsningar för P11 och P15 databaser med 4 TB maxsize

En maximal storlek på 4 TB för P11 och P15 databasen stöds i vissa regioner (som diskuterats tidigare). hello gäller följande överväganden och begränsningar tooP11 och P15 databaser med 4 TB maxsize:

- Om du väljer hello 4 TB maxsize alternativet när du skapar en databas (med ett värde på 4 TB eller 4096 GB), skapa hello kommandot misslyckas med ett fel om hello databasen har etablerats i en region som stöds inte.
- Du kan öka hello maxsize lagring too4 TB för befintliga P11 och P15 databaser finns i någon av hello stöds regioner. Detta kan kontrolleras med hjälp av hello [Välj DATABASEPROPERTYEX](https://msdn.microsoft.com/library/ms186823.aspx) eller genom att kontrollera hello databasstorleken i hello Azure-portalen. Uppgradera en befintlig P11 eller P15 kan databasen endast utföras av en huvudsaklig inloggning på servernivå eller av medlemmar i hello dbmanager-databasroll. 
- Om en uppgraderingsåtgärd utförs i uppdateras en region som stöds hello konfiguration omedelbart. hello databasen förblir online under hello uppgraderingsprocessen. Du kan dock använda hello fullständig 4 TB lagringsutrymme innan hello faktiska databasfilerna har uppgraderats toohello nya maxsize. hello lång tid som krävs beror på hello storleken på hello-databasen uppgraderas.  
- När du skapar eller uppdaterar en P11 eller P15, kan du endast välja mellan 1 TB och 4 TB maxsize. Storlekar för mellanlagring stöds inte för närvarande. När du skapar en P11/P15 är hello standard lagringsalternativ på 1 TB förvald. Du kan öka hello lagring maximala too4TB för en ny eller befintlig enskild databas för databaser som finns på någon av hello stöds regioner. Max storlek kan inte ökas ovan 1 TB för alla andra regioner. hello pris ändras inte när du väljer 4 TB ingår lagringsutrymme.
- hello 4 TB databasen maxsize får inte vara ändrade too1 TB även om hello lagringsutrymme som faktiskt används understiger 1 TB. Du kan därför nedgradera P11 - 4 TB/P15 - 4 TB-tooa P11 - 1 TB/P15 - 1 TB eller en lägre prestandanivå exempelvis tooP1 P6) tills ger ytterligare lagringsalternativ för hello resten av hello prestandanivåer. Den här begränsningen gäller även toohello återställning och kopiera scenarier inklusive i tidpunkt, geo-återställning, lång-sikt-säkerhetskopia-kvarhållning och databaskopian. När en databas har konfigurerats med alternativet för hello 4 TB kan måste alla återställningsåtgärder för den här databasen köras i en P11/P15 med 4 TB maxsize.
- När skapar eller uppgraderar en P11/P15 databas i en region som stöds inte hello skapar eller uppgradera misslyckas med följande felmeddelande hello: **P11 och P15-databas med too4TB lagring är tillgängliga i oss East2, västra USA, oss Gov Virginia Västeuropa, Tyskland Central, Sydostasien, östra, Östra Australien, Kanada Central och Kanada Öst.**
- Scenarier med aktiv geo-replikering:
   - Skapa en relation för geo-replikering: om hello primära databasen är P11 eller P15, hello secondary(ies) måste också vara P11 eller P15; lägre prestandanivåer avvisas som sekundärservrar eftersom de inte kan stödja 4 TB.
   - Uppgradera hello primära databasen i en relation för geo-replikering: ändra hello maxsize too4 TB på en primär databas utlöser hello samma ändras på hello sekundär databas. Båda uppgraderingarna måste genomföras för hello ändringen på hello primära tootake effekt. Region begränsningar för hello 4TB alternativet (se ovan). Om hello sekundära är i en region som inte stöder 4 TB, uppgraderas inte hello primära.
- Hello Import/Export-tjänsten för att läsa in P11 4TB/P15 - 4TB - databaser stöds inte. Använd SqlPackage.exe för[importera](sql-database-import.md) och [exportera](sql-database-export.md) data.

## <a name="manage-single-database-service-tiers-and-performance-levels-using-hello-azure-portal"></a>Hantera enskilda databaser servicenivåer och prestandanivåer med hello Azure-portalen

tooset eller ändra hello tjänstnivån, prestanda eller lagringsutrymmet för en ny eller befintlig Azure SQL-databas med hello Azure-portalen, öppna hello **konfigurera prestanda** fönster för databasen genom att klicka på  **Prisnivå (skala dtu: er)** – som visas i följande skärmbild hello. 

- Ange eller ändra hello tjänstnivå genom att välja hello tjänstnivån för din arbetsbelastning. 
- Ange eller ändra hello prestandanivå (**dtu: er**) i en tjänstnivå med hello **DTU** skjutreglaget.
- Ange eller ändra hello lagringsutrymmet för hello prestandanivå med hello **lagring** skjutreglaget. 

  ![Konfigurera tjänstnivå och prestandanivå servicenivå](./media/sql-database-service-tiers/service-tier-performance-level.png)

> [!IMPORTANT]
> Granska [aktuella begränsningar för P11 och P15 databaser med 4 TB maxsize](sql-database-service-tiers.md#current-limitations-of-p11-and-p15-databases-with-4-tb-maxsize) när du väljer en P11 eller P15 tjänstnivå.
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-powershell"></a>Hantera enskilda databaser servicenivåer och prestandanivåer med hjälp av PowerShell

tooset eller ändra tjänstnivåer för Azure SQL-databaser, prestandanivåer och lagringsutrymmet med hjälp av PowerShell, Använd hello följande PowerShell-cmdlets. Om du behöver tooinstall eller uppgradera PowerShell Se [installera Azure PowerShell-modulen](/powershell/azure/install-azurerm-ps). 

| Cmdlet | Beskrivning |
| --- | --- |
|[New-AzureRmSqlDatabase](/powershell/module/azurerm.sql/new-azurermsqldatabase)|Skapar en databas |
|[Get-AzureRmSqlDatabase](/powershell/module/azurerm.sql/get-azurermsqldatabase)|Hämtar en eller flera databaser|
|[Set-AzureRmSqlDatabase](/powershell/module/azurerm.sql/set-azurermsqldatabase)|Anger egenskaperna för en databas eller flyttar en befintlig databas till en elastisk pool|


> [!TIP]
> Ett exempel PowerShell-skript som övervakar hello prestandamått för en databas skalas den tooa högre prestandanivå och skapar en aviseringsregel på en av hello prestandamått finns [Övervakare och skala en enskild SQL-databas med hjälp av PowerShell](scripts/sql-database-monitor-and-scale-database-powershell.md).

## <a name="manage-single-database-service-tiers-and-performance-levels-using-hello-azure-cli"></a>Hantera enskilda databaser servicenivåer och prestandanivåer med hello Azure CLI

tooset eller ändra tjänstnivåer för Azure SQL-databaser, prestandanivåer och lagringsutrymmet med hjälp av hello Azure CLI, använder hello följande [Azure CLI SQL Database](/cli/azure/sql/db) kommandon. Använd hello [moln Shell](/azure/cloud-shell/overview) toorun hello CLI i webbläsaren eller [installera](/cli/azure/install-azure-cli) på macOS, Linux och Windows. Skapa och hantera SQL elastiska pooler finns [elastiska pooler](sql-database-elastic-pool.md).

| Cmdlet | Beskrivning |
| --- | --- |
|[Skapa AZ sql-databas](/cli/azure/sql/db#create) |Skapar en databas|
|[AZ sql db-lista](/cli/azure/sql/db#list)|Visar en lista över alla databaser och datalager i en server eller alla databaser i en elastisk pool|
|[AZ sql db lista-versioner](/cli/azure/sql/db#list-editions)|Visar tillgängliga mål och Lagringsgränser|
|[AZ sql db lista-användningsområden](/cli/azure/sql/db#list-usages)|Returnerar databasen användningsområden|
|[AZ sql db visa](/cli/azure/sql/db#show)|Hämtar ett databasen eller data warehouse|
|[AZ sql db-uppdateringen](/cli/azure/sql/db#update)|Uppdaterar en-databas|

> [!TIP]
> Ett Azure CLI exempelskript som kan skalas enda Azure SQL database tooa olika prestandanivå efter hämtar information om hello storleken på hello databasen finns [Använd CLI toomonitor och skala en enskild SQL-databas](scripts/sql-database-monitor-and-scale-database-cli.md).
>

## <a name="manage-single-database-service-tiers-and-performance-levels-using-transact-sql"></a>Hantera enskilda databaser servicenivåer och prestandanivåer med Transact-SQL

tooset eller ändra tjänstnivåer för Azure SQL-databaser, prestandanivåer och lagringsutrymmet med Transact-SQL, använda hello följande T-SQL-kommandon. Du kan skicka dessa kommandon använder hello Azure-portalen [SQL Server Management Studio](/sql/ssms/use-sql-server-management-studio), [Visual Studio Code](https://code.visualstudio.com/docs), eller andra program som kan ansluta tooan Azure SQL Database-server och skicka Transact-SQL kommandon. 

| Kommando | Beskrivning |
| --- | --- |
|[Skapa databas (Azure SQL Database)](/sql/t-sql/statements/create-database-azure-sql-database)|Skapar en ny databas. Du måste vara anslutna toohello huvuddatabasen toocreate en ny databas.|
| [ALTER DATABASE (Azure SQL-databas)](/sql/t-sql/statements/alter-database-azure-sql-database) |Ändrar en Azure SQL database. |
|[sys.database_service_objectives (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-service-objectives-azure-sql-database)|Returnerar hello edition (tjänstnivån), tjänstmålet (prisnivån) och namn på elastisk pool, för en Azure SQL-databas eller ett Azure SQL Data Warehouse. Returnerar information om alla databaser om inloggad toohello master-databasen i en Azure SQL Database-server. Du måste vara anslutna toohello huvuddatabasen för Azure SQL Data Warehouse.|
|[sys.database_usage (Azure SQL Database)](/sql/relational-databases/system-catalog-views/sys-database-usage-azure-sql-database)|Visar hello antalet, typen och varaktighet för databaser på en Azure SQL Database-server.|

hello följande exempel visar hello maxsize ändras med hjälp av hello ALTER DATABASE-kommandot:

 ```sql
ALTER DATABASE <myDatabaseName> 
   MODIFY (MAXSIZE = 4096 GB);
```

## <a name="manage-single-databases-using-hello-rest-api"></a>Hantera enskilda databaser med hello REST API

tooset eller ändra tjänstnivåer för Azure SQL-databaser, prestandanivåer och lagringsutrymmet med hjälp av hello REST-API finns [Azure SQL Database REST API](/rest/api/sql/).

## <a name="next-steps"></a>Nästa steg

* Lär dig mer om [dtu: er](sql-database-what-is-a-dtu.md).
* toolearn om övervakning av DTU-användningen finns [övervakning och prestandajustering](sql-database-troubleshoot-performance.md).

