---
title: "aaaDesign vägledning för replikerade tabeller - Azure SQL Data Warehouse | Microsoft Docs"
description: "Rekommendationer för hur man designar replikerade tabeller i Azure SQL Data Warehouse-schemat."
services: sql-data-warehouse
documentationcenter: NA
author: ronortloff
manager: jhubbard
editor: 
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: tables
ms.date: 07/14/2017
ms.author: rortloff;barbkess
ms.openlocfilehash: 5d405b8c404c65177b387ba959126839c1cf8799
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="design-guidance-for-using-replicated-tables-in-azure-sql-data-warehouse"></a>Utforma riktlinjer för att använda replikerade tabeller i Azure SQL Data Warehouse
Den här artikeln ger rekommendationer för att utforma replikerade tabeller i SQL Data Warehouse-schemat. Använd dessa rekommendationer tooimprove frågeprestanda genom att minska komplexiteten för data movement och fråga.

> [!NOTE]
> hello är replikerad tabell för närvarande i förhandsversion. Vissa beteenden är ämne toochange.
> 

## <a name="prerequisites"></a>Krav
Den här artikeln förutsätter att du är bekant med datadistribution och begrepp för flytt av data i SQL Data Warehouse.  Mer information finns i [Distributed data](sql-data-warehouse-distributed-data.md). 

Som en del av tabelldesign, Förstå så mycket som möjligt om dina data och hur data hello efterfrågas.  Till exempel tänka på följande:

- Hur stor är hello tabellen?   
- Hur ofta uppdateras hello tabellen?   
- Måste jag fakta-och dimensionstabeller i ett informationslager?   

## <a name="what-is-a-replicated-table"></a>Vad är en replikerad tabell?
En replikerad tabell har en fullständig kopia av hello-tabell som är tillgängliga på varje Compute-nod. Replikera en tabell tar bort hello måste tootransfer data mellan datornoderna innan ett join- eller aggregering. Eftersom hello tabellen har flera kopior, replikerade tabeller som fungerar bäst när hello tabellstorlek är mindre än 2 GB komprimerat.

hello visar följande diagram en replikerad tabell som är tillgängligt på varje Compute-nod. Hello replikerad tabell är fullständigt kopierade tooa distributionsdatabasen på varje beräkningsnod i SQL Data Warehouse. 

![Replikerade tabellen](media/guidance-for-using-replicated-tables/replicated-table.png "replikerade tabell")  

Replikerade tabeller fungerar bra för små dimensionstabeller i ett stjärnschema. Dimensionstabeller är vanligtvis av storlek som gör det möjligt toostore och underhålla flera kopior. Dimensioner lagra beskrivande data som ändras långsamt, till exempel kundens namn och adress och produktinformation. hello långsamt ändra uppbyggnad hello data leder toofewer ombyggnad av hello replikerad tabell. 

Överväg att använda en replikerad tabell när:

- hello tabellstorleken på disken är mindre än 2 GB, oavsett hello antalet rader. toofind hello storleken på en tabell kan du använda hello [DBCC PDW_SHOWSPACEUSED](https://docs.microsoft.com/en-us/sql/t-sql/database-console-commands/dbcc-pdw-showspaceused-transact-sql) kommando: `DBCC PDW_SHOWSPACEUSED('ReplTableCandidate')`. 
- hello tabellen används i kopplingar som annars skulle kräva dataflyttning. Exempelvis kräver en koppling för hash-distribuerade tabeller dataflyttning när hello anslutande kolumner inte är hello samma kolumn för distribution. Om någon av hello distribuerade hash-tabeller är liten du en replikerad tabell. En koppling för en tabell som resursallokering kräver dataflyttning. Vi rekommenderar att du använder replikerade tabeller i stället för resursallokering tabeller i de flesta fall. 


Överväg att konvertera en befintlig distribuerad tabell tooa replikeras när:

- Frågan planerar Använd dataflyttsåtgärderna som sänder hello data tooall hello Compute-noder. Hej BroadcastMoveOperation dyr och långsammare prestanda för frågor. tooview dataflyttsåtgärderna i frågeplaner, Använd [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql).
 
Replikerade tabeller kan inte ge hello bästa möjliga prestanda när:

- hello-tabellen har ofta infoga, uppdatera och ta bort. Dessa data manipulation language (DML) åtgärder kräver en återskapning av hello replikerad tabell. Återskapa kan ofta ge lägre prestanda.
- hello datalagret skalas ofta. Skala ett informationslager ändrar hello antal Compute-noder som har en återskapning.
- hello-tabellen har ett stort antal kolumner, men dataåtgärder normalt kommer åt litet antal kolumner. I det här scenariot, istället för att replikera hello hela tabellen, det kan vara effektivare toohash distribuera hello tabellen och skapa ett index på hello ofta kolumner. När en fråga kräver dataflyttning SQL Data Warehouse endast flyttar data i hello begärda kolumner. 



## <a name="use-replicated-tables-with-simple-query-predicates"></a>Använda replikerade tabeller med enkel fråga predikat
Innan du väljer toodistribute eller replikera en tabell tänka hello typer av frågor som du planerar toorun mot hello tabell. När det är möjligt

- Använd replikerade tabeller för frågor med enkel fråga predikat, till exempel likhet eller olikhet.
- Använd distribuerade tabeller för frågor med komplicerade frågan predikat, till exempel vill eller inte gillar.

Processorintensiva frågor gör bäst ifrån sig när hello arbete distribueras över alla hello Compute-noder. Till exempel bättre frågor som körs beräkningar på varje rad i en tabell för distribuerade tabeller än replikerade tabeller. Eftersom en replikerad tabell lagras i sin helhet på varje beräkningsnod körs en processorintensiva fråga mot en replikerad tabell mot hello hela tabellen på varje beräkningsnod. hello kan extra beräkning sakta frågeprestanda.

Den här frågan har till exempel ett komplext predikat.  Det körs snabbare när leverantören är en distribuerad tabell i stället för en replikerad tabell. Leverantören kan vara hash-distribuerade eller resursallokering distribueras i det här exemplet.

```sql

SELECT EnglishProductName 
FROM DimProduct 
WHERE EnglishDescription LIKE '%frame%comfortable%'

```

## <a name="convert-existing-round-robin-tables-tooreplicated-tables"></a>Konvertera befintliga resursallokering tabeller tooreplicated tabeller
Om du redan har resursallokering tabeller, rekommenderar vi konvertera dem tooreplicated tabellerna om de uppfyller med villkor som beskrivs i den här artikeln. Replikerade tabeller förbättra prestanda över resursallokering tabeller eftersom de undanröjer hello behovet av dataflyttning.  En tabell med resursallokering kräver alltid dataflyttning för kopplingar. 

Det här exemplet används [CTAS](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse) toochange hello DimSalesTerritory tabell tooa replikerad tabell. Det här exemplet fungerar oavsett om DimSalesTerritory är hash-distribuerade eller resursallokering.

```sql
CREATE TABLE [dbo].[DimSalesTerritory_REPLICATE]   
WITH   
  (   
    CLUSTERED COLUMNSTORE INDEX,  
    DISTRIBUTION = REPLICATE  
  )  
AS SELECT * FROM [dbo].[DimSalesTerritory]
OPTION  (LABEL  = 'CTAS : DimSalesTerritory_REPLICATE') 

--Create statistics on new table
CREATE STATISTICS [SalesTerritoryKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryKey]);
CREATE STATISTICS [SalesTerritoryAlternateKey] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryAlternateKey]);
CREATE STATISTICS [SalesTerritoryRegion] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryRegion]);
CREATE STATISTICS [SalesTerritoryCountry] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryCountry]);
CREATE STATISTICS [SalesTerritoryGroup] ON [DimSalesTerritory_REPLICATE] ([SalesTerritoryGroup]);

-- Switch table names
RENAME OBJECT [dbo].[DimSalesTerritory] too[DimSalesTerritory_old];
RENAME OBJECT [dbo].[DimSalesTerritory_REPLICATE] too[DimSalesTerritory];

DROP TABLE [dbo].[DimSalesTerritory_old];
```  

### <a name="query-performance-example-for-round-robin-versus-replicated"></a>Exempel på frågan resultat för resursallokering jämfört med replikeras 

En replikerad tabell kräver inte någon dataflyttning för kopplingar eftersom det redan finns på varje beräkningsnod hello hela tabellen. Om hello dimensionstabeller resursallokering distribueras, kopieras en koppling hälsningspaket dimensionstabellen i fullständig tooeach Compute-nod. toomove hello data hello frågeplan innehåller en åtgärd som kallas BroadcastMoveOperation. Den här typen av data movement åtgärden långsammare prestanda för frågor och elimineras med hjälp av replikerade tabeller. tooview för planen frågesteg använda hello [sys.dm_pdw_request_steps](https://docs.microsoft.com/sql/relational-databases/system-dynamic-management-views/sys-dm-pdw-request-steps-transact-sql) system katalogvyn. 

Till exempel i följande fråga mot hello AdventureWorks schema hello ` FactInternetSales` tabellen är hash-distribueras. Hej `DimDate` och `DimSalesTerritory` tabeller är mindre dimensionstabeller. Den här frågan returnerar hello total försäljning i Nordamerika för räkenskapsår 2004:
 
```sql
SELECT [TotalSalesAmount] = SUM(SalesAmount)
FROM dbo.FactInternetSales s
INNER JOIN dbo.DimDate d
  ON d.DateKey = s.OrderDateKey
INNER JOIN dbo.DimSalesTerritory t
  ON t.SalesTerritoryKey = s.SalesTerritoryKey
WHERE d.FiscalYear = 2004
  AND t.SalesTerritoryGroup = 'North America'
```
Vi återskapas `DimDate` och `DimSalesTerritory` som resursallokering tabeller. Därför hello frågan visade hello följande frågeplan som har flera broadcast flytta operationer: 
 
![Resursallokering frågeplan](media/design-guidance-for-replicated-tables/round-robin-tables-query-plan.jpg) 

Vi återskapas `DimDate` och `DimSalesTerritory` som replikerade tabeller och kördes hello frågan igen. hello resulterande frågeplan är mycket kortare och inte har någon sänder ut flyttar.

![Replikerade frågeplan](media/design-guidance-for-replicated-tables/replicated-tables-query-plan.jpg) 


## <a name="performance-considerations-for-modifying-replicated-tables"></a>Prestandaöverväganden för att ändra replikerade tabeller
SQL Data Warehouse implementerar en replikerad tabell genom att underhålla en huvudversionen av hello tabell. Den kopierar hello huvudversionen tooone distributionsdatabasen på varje beräkningsnod. När det finns en ändring, uppdaterar SQL Data Warehouse först hello huvudtabellen. Sedan kräver den att hello tabeller på varje Compute-nod. En återskapning av en replikerad tabell innehåller kopiera hello tabell tooeach Compute-nod och sedan återskapa hello index.

Ombyggnad krävs efter:
- Data läses in eller ändras
- hello data warehouse är skalade tooa olika DWU-inställningar
- Tabelldefinitionen uppdateras

Ombyggnad krävs inte efter:
- Åtgärden pausa
- Åtgärden återuppta

hello återskapa sker inte omedelbart när data har ändrats. I stället hello återskapa utlöses hello första gången en fråga markerar i hello tabellen.  Inom hello första select-instruktion från hello tabell finns steg toorebuild hello replikerad tabell.  Eftersom hello återskapa görs i hello-fråga, kan hello påverkan toohello inledande väljer instruktionen ha betydande beroende på hello storleken på hello tabellen.  Om flera replikerade tabeller ingår som behöver en återskapning, återskapas varje kopia följd som steg i hello-instruktion.  toomaintain datakonsekvens under hello återskapa hello replikerad tabell ett exklusivt lås hämtas för hello tabellen.  hello lås förhindrar alla åtkomst toohello tabellen för hello återskapa hello varaktighet. 

### <a name="use-indexes-conservatively"></a>Använda index hänsyn
Standard indexering praxis gäller tooreplicated tabeller. SQL Data Warehouse återskapar varje replikerad Tabellindex som en del av hello återskapa. Använd endast index om hello prestandafördelar uppväger hello kostnaden för att bygga om hello index.  
 
### <a name="batch-data-loads"></a>Batch databelastningar
När du läser in data i replikerade tabeller, kan du försöka toominimize bygger genom batchbearbetning belastningar tillsammans. Utför alla hello batchar belastningar innan du kör select-satser.

Det här mönstret belastningen läser in data från fyra källor och anropar fyra bygger. 

- Läsa in från datakällan 1.
- SELECT-instruktion utlösare återskapa 1.
- Läsa in från källan 2.
- SELECT-instruktion utlösare återskapa 2.
- Läsa in från källan 3.
- SELECT-instruktion utlösare återskapa 3.
- Läsa in från datakällan 4.
- SELECT-instruktion utlösare återskapa 4.

Det här mönstret belastningen läser in data från fyra källor men endast anropar en återskapning.

- Läsa in från datakällan 1.
- Läsa in från källan 2.
- Läsa in från källan 3.
- Läsa in från datakällan 4.
- SELECT-instruktion utlösare återskapa.


### <a name="rebuild-a-replicated-table-after-a-batch-load"></a>Återskapa en replikerad tabell efter en batch-inläsning
tooensure konsekvent körningstider, rekommenderar vi att tvinga en uppdatering av hello replikerade tabeller efter en batch inläsningen. Annars måste hello första frågan vänta hello tabeller toorefresh som innehåller återskapa hello index. Beroende på hello antalet och storleken på replikerade tabeller påverkas, vara hello prestandapåverkan betydande.  

Den här frågan använder hello [sys.pdw_replicated_table_cache_state](https://docs.microsoft.com/sql/relational-databases/system-catalog-views/sys-pdw-replicated-table-cache-state-transact-sql) DMV toolist hello replikerade tabeller som har ändrats, men inte återskapas.

```sql 
SELECT [ReplicatedTable] = t.[name]
  FROM sys.tables t  
  JOIN sys.pdw_replicated_table_cache_state c  
    ON c.object_id = t.object_id 
  JOIN sys.pdw_table_distribution_properties p 
    ON p.object_id = t.object_id 
  WHERE c.[state] = 'NotReady'
    AND p.[distribution_policy_desc] = 'REPLICATE'
```
 
tooforce kör hello efter uttryck för varje tabell i hello föregående utdata för en återskapning. 

```sql
SELECT TOP 1 * FROM [ReplicatedTable]
``` 
 
## <a name="next-steps"></a>Nästa steg 
toocreate en replikerad tabell med någon av dessa uttryck:

- [Skapa tabell (Azure SQL Data Warehouse)](https://docs.microsoft.com/sql/t-sql/statements/create-table-azure-sql-data-warehouse)
- [Skapa TABLE AS SELECT (Azure SQL Data Warehouse](https://docs.microsoft.com/sql/t-sql/statements/create-table-as-select-azure-sql-data-warehouse)

En översikt över distribuerade tabeller, se [distribuerade tabeller](sql-data-warehouse-tables-distribute.md).



