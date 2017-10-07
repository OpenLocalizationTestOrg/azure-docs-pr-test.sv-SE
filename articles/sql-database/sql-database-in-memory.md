---
title: aaaAzure SQL databas i minnet tekniker | Microsoft Docs
description: "Azure SQL Database i minnet tekniker förbättra hello prestanda för transaktionell och analytics arbetsbelastningar. Lär dig hur tootake nytta av dessa tekniker."
services: sql-database
documentationCenter: 
author: jodebrui
manager: jhubbard
editor: 
ms.assetid: 250ef341-90e5-492f-b075-b4750d237c05
ms.service: sql-database
ms.custom: develop databases
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/24/2017
ms.author: jodebrui
ms.openlocfilehash: 1bacd7297b2f9b018853088eabf2a2ee66a9cb43
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="optimize-performance-by-using-in-memory-technologies-in-sql-database"></a>Optimera prestanda genom att använda InMemory-tekniker i SQL-databas

Du kan åstadkomma prestandaförbättringar med olika arbetsbelastningar med hjälp av InMemory-tekniker i Azure SQL Database: transaktionell (online transaktionsbearbetning (OLTP)), analytics (online analytical processing (OLAP)), och blandade (hybrid transaktion analytical processing (HTAP)). Eftersom Hej effektivare fråga och transaktionsbearbetning, InMemory-tekniker hjälper dig också tooreduce kostnaden. Vanligtvis behöver du inte tooupgrade hello prisnivån för hello databasen tooachieve prestandavinster. I vissa fall kan du även eventuellt minska hello prisnivån när ändå prestandaförbättringar med InMemory-teknik.

Här är två exempel på hur Minnesintern OLTP hjälpt toosignificantly förbättra prestanda:

- Med hjälp av Minnesintern OLTP [kvorum affärslösningar var kan toodouble arbetsbelastningen samtidigt förbättra dtu: er med 70%](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database).
    - DTU innebär *databasen genomflödesenhet*, och innehåller en mesurement resursförbrukning.
- hello följande videoklipp visar viktig förbättringar i resursanvändningen med ett exempel på arbetsbelastning: [Minnesintern OLTP i Azure SQL Database Video](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB).
    - Mer information finns i blogginlägget hello: [Minnesintern OLTP i Azure SQL Database-blogginlägg](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

InMemory-teknik finns i alla databaser i hello Premium-nivån, inklusive databaser i Premium elastiska pooler.

hello förklarar följande videon potentiella prestandafördelar med InMemory-tekniker i Azure SQL Database. Kom ihåg att hello prestanda får du alltid ser beror på många faktorer, inklusive hello uppbyggnad hello arbetsbelastning och data, mönster för dataåtkomst på hello-databasen, och så vidare.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Azure-SQL-Database-In-Memory-Technologies/player]
>
>

Azure SQL Database har hello följande InMemory-tekniker:

- *Minnesintern OLTP* ökar dataflödet och minskar svarstiden för transaktionsbearbetning. Scenarier som har nytta i minnet OLTP är: hög genomströmning transaktionsbearbetning, till exempel handel och spel, datapåfyllning från händelser eller IoT-enheter, cachelagring datainläsning, och tillfällig tabell och tabellen variabeln scenarier.
- *Grupperade columnstore-index* minska din lagring storleken (upp too10 gånger) och förbättra prestanda för frågor för rapportering och analys. Du kan använda den med faktatabeller i dina data dataarkiv toofit mer data i databasen och förbättra prestanda. Du kan också använda den med historiska data i din databas tooarchive och vara kan tooquery in too10 gånger mer data.
- *Icke-grupperat columnstore-index* HTAP hjälp du toogain realtid insikter om ditt företag genom att fråga hello operativa databasen direkt, utan hello måste toorun en dyr extrahering, transformering och laddning (ETL) processen och vänta för hello data warehouse toobe fylls i. Icke-grupperat columnstore-index fjärrkörning mycket snabbt analytics frågor på hello OLTP-databasen samtidigt minska hello inverkan på hello operativa arbetsbelastning.
- Du kan också ha hello kombination av en minnesoptimerad tabell med ett columnstore-index. Den här kombinationen kan tooperform mycket snabbt transaktionsbearbetning, och för*samtidigt* kör analytics frågar snabbt på hello samma data.

Både columnstore-index och Minnesintern OLTP har varit en del av SQL Server-produkt för hello sedan 2012 och 2014, respektive. Azure SQL Database och SQL Server dela hello samma implementering av InMemory-tekniker. Framöver, släpps nya funktioner för dessa tekniker i Azure SQL Database först innan de blir tillgängliga i SQL Server.

Det här avsnittet beskrivs aspekter av Minnesintern OLTP och columnstore-index som är specifika tooAzure SQL-databas och även exempel:
- Hello effekten av dessa tekniker visas på storleksbegränsningar för lagring och data.
- Du ser hur toomanage hello flytt av databaser som använder dessa tekniker mellan hello olika prisnivåer.
- Ser du två exempel som visar hello användning av Minnesintern OLTP, samt columnstore-index i Azure SQL Database.

Se hello följande resurser för mer information.

Detaljerad information om hello tekniker:

- [Minnesintern OLTP översikt och Användningsscenarier](https://msdn.microsoft.com/library/mt774593.aspx) (omfattar referenser toocustomer fallstudier och information tooget igång)
- [Dokumentation för OLTP i minnet](http://msdn.microsoft.com/library/dn133186.aspx)
- [Guide för Columnstore-index](https://msdn.microsoft.com/library/gg492088.aspx)
- Hybrid transaktionella/analytisk behandling (HTAP), även kallat [operativa analys i realtid](https://msdn.microsoft.com/library/dn817827.aspx)

En kort introduktion på Minnesintern OLTP: [snabb Start 1: I minnet OLTP-teknik för snabbare prestanda för T-SQL](http://msdn.microsoft.com/library/mt694156.aspx) (en annan artikel toohelp dig att komma igång)

Djupgående video om hello tekniker:

- [Minnesintern OLTP i Azure SQL Database](https://channel9.msdn.com/Shows/Data-Exposed/In-Memory-OTLP-in-Azure-SQL-DB) (som innehåller en demonstration av prestanda fördelar och steg tooreproduce resultaten och själv)
- [Minnesintern OLTP video: Vad det är och när/hur toouse den](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/03/in-memory-oltp-video-what-it-is-and-whenhow-to-use-it/)
- [Kolumnlagringsindexet: I minnet Analytics videor från Ignite 2016](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2016/10/04/columnstore-index-in-memory-analytics-i-e-columnstore-index-videos-from-ignite-2016/)

## <a name="storage-and-data-size"></a>Lagrings- och storlek

### <a name="data-size-and-storage-cap-for-in-memory-oltp"></a>Storlek och lagring cap för OLTP i minnet

Minnesintern OLTP innehåller minnesoptimerade tabeller som används för lagring av användardata. Dessa tabeller är obligatoriska toofit i minnet. Eftersom du hantera minne direkt i hello SQL Database-tjänsten har vi hello begreppet en kvot för användardata. Den här idén är refererad tooas *Minnesintern OLTP lagring*.

Varje stöds fristående databas prisnivå och varje elastisk pool prisnivån innehåller mängden lagringsutrymme som OLTP i minnet. När hello skrivning får du ett GB lagringsutrymme för varje 125 datatransaktionsenheter (Dtu) eller en elastisk databas transaktionsenheter (edtu: er).

Hej [SQL Database servicenivåer](sql-database-service-tiers.md) artikeln har hello officiella lista över hello Minnesintern OLTP lagring som är tillgängliga för varje fristående databasen och stöds elastisk pool prisnivån.

följande objekt räknas in din Minnesintern OLTP lagring cap hello:

- Aktiva användare datarader i minnesoptimerade tabeller och tabellvariabler. Observera att den gamla raden versioner inte räknas in hello fjärrskrivbordsanslutning.
- Index för minnesoptimerade tabeller.
- Operativ tillsyn ALTER TABLE-åtgärder.

Om du träffar hello cap felmeddelandet out av kvot och du inte längre kan tooinsert eller uppdatera data. toomitigate felet, ta bort data eller öka hello prisnivån hello databas eller poolen.

Mer information om övervakning i minnet OLTP lagringsanvändningen och konfigurera aviseringar när du nästan träffar hello cap finns [övervakaren InMemory-lagringen](sql-database-in-memory-oltp-monitoring.md).

#### <a name="about-elastic-pools"></a>Om elastiska pooler

Hello OLTP InMemory-lagringen delas mellan alla databaser i poolen hello med elastiska pooler. Därför kan hello användning i en databas potentiellt påverka andra databaser. Det finns två åtgärder för den här:

- Konfigurera en Max-eDTU för databaser som är lägre än hello eDTU-antal för hello poolen som helhet. Den här högsta värdet caps hello lagringsanvändningen i minnet OLTP, i en databas i hello pool, toohello storlek som motsvarar toohello eDTU antal.
- Konfigurera en minsta eDTU som är större än 0. Detta minsta garanterar att varje databas i hello poolen har hello mängden tillgängligt lagringsutrymme för OLTP i minnet som motsvarar toohello konfigurerad Min eDTU.

### <a name="data-size-and-storage-for-columnstore-indexes"></a>Datastorlek och lagring för columnstore-index

Columnstore-index är inte obligatoriska toofit i minnet. Därför hello endast fästpunkten på hello storlek hello index är hello största övergripande databasens storlek som dokumenteras i hello [SQL Database servicenivåer](sql-database-service-tiers.md) artikel.

När du använder grupperade columnstore-index används kolumner komprimering för hello bastabellen lagring. Den här komprimeringen kan avsevärt minska hello lagring storleken på användardata, vilket innebär att du får plats mer data i hello-databasen. Och hello komprimering kan förlängas med [kolumner arkivering komprimering](https://msdn.microsoft.com/library/cc280449.aspx#Using Columnstore and Columnstore Archive Compression). hello mängden komprimering som du kan åstadkomma beror på hello uppbyggnad hello data, men 10 gånger hello komprimering är inte ovanligt.

Om du har en databas med en maximal storlek på 1 terabyte (TB) och du uppnå 10 gånger hello komprimering med hjälp av columnstore-index kan anpassa du totalt 10 TB användardata i hello-databasen.

När du använder icke-grupperat columnstore-index, lagras fortfarande hello bastabellen hello traditionella rowstore format. Hej lagringsbesparingar är därför inte lika stor som med grupperade columnstore-index. Om du ersätter ett antal traditionella icke-grupperat index med en enda columnstore-index, se du fortfarande en övergripande besparingar i hello lagring storleken för hello tabell.

## <a name="moving-databases-that-use-in-memory-technologies-between-pricing-tiers"></a>Flytta databaser som använder minnesinterna tekniker mellan prisnivåer

Det finns aldrig alla inkompatibiliteter eller andra problem när du uppgraderar tooa högre prisnivå, såsom från Standard tooPremium. hello tillgängliga funktioner och resurser bara öka.

Men lägre hello prisnivån kan påverka din databas. hello påverkan är särskilt tydligt när du Nedgradera från tooStandard Premium eller Basic när databasen innehåller Minnesintern OLTP-objekt. Minnesoptimerade tabeller och columnstore-index är inte tillgänglig efter hello nedgradering (även om de är synligt). hello samma förutsättningar gäller när du sänker hello prisnivån för en elastisk pool eller flytta en databas med InMemory-tekniker, till en Standard eller grundläggande elastisk pool.

### <a name="in-memory-oltp"></a>Minnesintern OLTP

*Nedgradera tooBasic/Standard*: Minnesintern OLTP stöds inte i databaser i hello Standard- eller Basic-nivån. Det är dessutom inte möjligt toomove en databas som har alla Minnesintern OLTP objekt toohello Standard- eller Basic-nivån.

Ta bort alla minnesoptimerade tabeller och tabelltyper samt alla internt kompilerade moduler för T-SQL innan du nedgradera hello databasen tooStandard/enkel.

Det finns en programmässiga sätt toounderstand om en viss databas stöder Minnesintern OLTP. Du kan köra hello följande Transact-SQL-fråga:

```
SELECT DatabasePropertyEx(DB_NAME(), 'IsXTPSupported');
```

Om hello frågan returnerar **1**, i minnet OLTP stöds i den här databasen.


*Nedgradera tooa lägre premiumnivån*: Data i minnesoptimerade tabeller måste rymmas inom hello Minnesintern OLTP lagring som är associerad med hello prisnivån hello databasen eller som är tillgängliga i hello elastisk pool. Om du försöker toolower hello prisnivån eller flytta hello-databas till en pool som inte har tillräckligt med lagringsutrymme för OLTP i minnet, hello åtgärden misslyckas.

### <a name="columnstore-indexes"></a>Columnstore-index

*Nedgradera tooBasic eller Standard*: Columnstore-index stöds endast på hello Premium-prisnivån och inte på hello Standard eller Basic nivåer. När du nedgradera din databas tooStandard eller Basic otillgänglig columnstore-index. hello system behålls columnstore-index, men den använder aldrig hello index. Om du senare uppgradera tillbaka tooPremium är columnstore-index klar toobe utnyttjas igen.

Om du har en **klustrade** columnstore-index hello hela tabellen blir tillgänglig efter nivå nedgradering. Därför rekommenderar vi att du ta bort alla *klustrade* columnstore-index innan du nedgradera databasen under hello Premium-nivån.

*Nedgradera tooa lägre premiumnivån*: den här nedgradering lyckas om hello hela databasen ryms inom hello maximala databasstorleken för hello mål prisnivån eller inom hello tillgängligt lagringsutrymme i hello elastisk pool. Det finns ingen specifik inverkan från hello columnstore-index.


<a id="install_oltp_manuallink" name="install_oltp_manuallink"></a>

&nbsp;

## <a name="1-install-hello-in-memory-oltp-sample"></a>1. Installera hello Minnesintern OLTP-exempel

Du kan skapa hello AdventureWorksLT exempeldatabasen med ett par klick i hello [Azure-portalen](https://portal.azure.com/). Sedan beskrivs hello stegen i det här avsnittet hur du kan utöka AdventureWorksLT databasen med InMemory-OLTP-objekt och visa prestandafördelarna.

En mer simplistic, men mer tilltalande prestanda demo för InMemory-OLTP finns:

- Utgåvan: [i-minne-oltp-demo-v1.0](https://github.com/Microsoft/sql-server-samples/releases/tag/in-memory-oltp-demo-v1.0)
- Källkod: [in-memory-oltp-demo-source-code](https://github.com/Microsoft/sql-server-samples/tree/master/samples/features/in-memory/ticket-reservations)

#### <a name="installation-steps"></a>Installationssteg

1. I hello [Azure-portalen](https://portal.azure.com/), skapar en Premium-databas på en server. Ange hello **källa** toohello AdventureWorksLT exempeldatabasen. Detaljerade instruktioner finns [skapa din första Azure SQL-databas](sql-database-get-started-portal.md).

2. Anslut toohello databasen med SQL Server Management Studio [(SSMS.exe)](http://msdn.microsoft.com/library/mt238290.aspx).

3. Kopiera hello [Minnesintern OLTP Transact-SQL-skript](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_oltp_sample.sql) tooyour Urklipp. Hej T-SQL-skript skapar hello nödvändiga InMemory-objekt i hello AdventureWorksLT exempeldatabasen som du skapade i steg 1.

4. Klistra in hello T-SQL-skript i SSMS och sedan köra hello skript. Hej `MEMORY_OPTIMIZED = ON` instruktionen CREATE TABLE-satser är avgörande. Exempel:


```
CREATE TABLE [SalesLT].[SalesOrderHeader_inmem](
    [SalesOrderID] int IDENTITY NOT NULL PRIMARY KEY NONCLUSTERED ...,
    ...
) WITH (MEMORY_OPTIMIZED = ON);
```


#### <a name="error-40536"></a>Fel 40536


Om det uppstår fel 40536 när du kör hello T-SQL-skript kör följande T-SQL-skript tooverify om hello databasen stöder i minnet hello:


```
SELECT DatabasePropertyEx(DB_Name(), 'IsXTPSupported');
```


Ett resultat av **0** innebär att i minnet inte stöds och **1** innebär att stöds. toodiagnose hello problemet, se till att hello-databasen i hello premiumnivån.


#### <a name="about-hello-created-memory-optimized-items"></a>Om hello skapade minnesoptimerade objekt

**Tabeller**: hello exemplet innehåller hello följande minnesoptimerade tabeller:

- SalesLT.Product_inmem
- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem
- Demo.DemoSalesOrderHeaderSeed
- Demo.DemoSalesOrderDetailSeed


Du kan inspektera minnesoptimerade tabeller via hello **Object Explorer** i SSMS. Högerklicka på **tabeller** > **Filter** > **filtrera inställningar** > **är Minnesoptimerad**. hello-värdet är lika med 1.


Eller fråga hello katalogvyer, exempelvis:


```
SELECT is_memory_optimized, name, type_desc, durability_desc
    FROM sys.tables
    WHERE is_memory_optimized = 1;
```


**Internt kompilerade lagrade proceduren**: du kan inspektera SalesLT.usp_InsertSalesOrder_inmem via en katalog Visa fråga:


```
SELECT uses_native_compilation, OBJECT_NAME(object_id), definition
    FROM sys.sql_modules
    WHERE uses_native_compilation = 1;
```


&nbsp;

### <a name="run-hello-sample-oltp-workload"></a>Kör hello exempel OLTP-arbetsbelastning

Hej endast skillnaden mellan hello följande två *lagrade procedurer* är att hello första proceduren använder minnesoptimerade tabeller hello-versioner, när hello använder den andra proceduren hello vanliga på disken tabeller:

- SalesLT**.** usp_InsertSalesOrder**_inmem**
- SalesLT**.** usp_InsertSalesOrder**_ondisk**


I det här avsnittet visas hur toouse hello praktiska **ostress.exe** verktyget tooexecute hello två lagrade procedurer på stress nivåer. Du kan jämföra hur lång tid det tar för hello två stress körs toofinish.


När du kör ostress.exe, rekommenderar vi att du skickar parametervärden som utformats för både hello följande:

- Kör ett stort antal samtidiga anslutningar med hjälp av - n100.
- Har varje anslutning slinga hundratals gånger, med hjälp av - r500.


Däremot kanske du vill toostart med mycket mindre värdena så - n10 och -r50 tooensure att allt fungerar.


### <a name="script-for-ostressexe"></a>Skriptet för ostress.exe


Det här avsnittet visar hello T-SQL-skript som är inbäddad i vår ostress.exe-kommandoraden. hello-skript som använder objekt som har skapats av hello T-SQL-skript som du tidigare har installerat.


hello följande skript infogar ett exempel ordern fem artiklar i hello följande minnesoptimerade *tabeller*:

- SalesLT.SalesOrderHeader_inmem
- SalesLT.SalesOrderDetail_inmem


```
DECLARE
    @i int = 0,
    @od SalesLT.SalesOrderDetailType_inmem,
    @SalesOrderID int,
    @DueDate datetime2 = sysdatetime(),
    @CustomerID int = rand() * 8000,
    @BillToAddressID int = rand() * 10000,
    @ShipToAddressID int = rand() * 10000;

INSERT INTO @od
    SELECT OrderQty, ProductID
    FROM Demo.DemoSalesOrderDetailSeed
    WHERE OrderID= cast((rand()*60) as int);

WHILE (@i < 20)
begin;
    EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT,
        @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od;
    SET @i = @i + 1;
end
```


toomake hello *_ondisk* version av Hej föregående T-SQL-skript för ostress.exe, ska du ersätta båda förekomster av hello *_inmem* substring med *_ondisk*. Dessa ersättningar påverkar hello namnen på tabeller och lagrade procedurer.


### <a name="install-rml-utilities-and-ostress"></a>Installera RML verktyg och ostress


Du skulle du bör planera toorun ostress.exe på en Azure-dator (VM). Du skapar en [Azure VM](https://azure.microsoft.com/documentation/services/virtual-machines/) i hello samma Azure geografiska region där AdventureWorksLT databasen finns. Men du kan köra ostress.exe på din bärbara dator i stället.


På hello VM, eller oavsett värd du väljer, installera verktyg för hello Replay Markup Language (RML). hello verktyg inkluderar ostress.exe.

Mer information finns i:
- Hej ostress.exe diskussion i [exempeldatabasen för InMemory-OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
- [Exempel på databasen för InMemory-OLTP](http://msdn.microsoft.com/library/mt465764.aspx).
- Hej [bloggen för att installera ostress.exe](http://blogs.msdn.com/b/psssql/archive/2013/10/29/cumulative-update-2-to-the-rml-utilities-for-microsoft-sql-server-released.aspx).



<!--
dn511655.aspx is for SQL 2014,
[Extensions tooAdventureWorks tooDemonstrate In-Memory OLTP]
(http://msdn.microsoft.com/library/dn511655&#x28;v=sql.120&#x29;.aspx)

whereas for SQL 2016+
[Sample Database for In-Memory OLTP]
(http://msdn.microsoft.com/library/mt465764.aspx)
-->



### <a name="run-hello-inmem-stress-workload-first"></a>Kör hello *_inmem* betonar arbetsbelastning först


Du kan använda en *RML Cmd-Prompt* fönstret toorun våra ostress.exe-kommandoraden. hello kommandoradsparametrar direkt ostress till:

- Kör 100 anslutningar samtidigt (-n100).
- Varje anslutning och kör hello T-SQL-skript 50 gånger (-r50).


```
ostress.exe -n100 -r50 -S<servername>.database.windows.net -U<login> -P<password> -d<database> -q -Q"DECLARE @i int = 0, @od SalesLT.SalesOrderDetailType_inmem, @SalesOrderID int, @DueDate datetime2 = sysdatetime(), @CustomerID int = rand() * 8000, @BillToAddressID int = rand() * 10000, @ShipToAddressID int = rand()* 10000; INSERT INTO @od SELECT OrderQty, ProductID FROM Demo.DemoSalesOrderDetailSeed WHERE OrderID= cast((rand()*60) as int); WHILE (@i < 20) begin; EXECUTE SalesLT.usp_InsertSalesOrder_inmem @SalesOrderID OUTPUT, @DueDate, @CustomerID, @BillToAddressID, @ShipToAddressID, @od; set @i += 1; end"
```


toorun hello föregående ostress.exe kommandoraden:


1. Återställ hello databasen datainnehåll genom att köra följande kommando i SSMS toodelete hello alla hello-data som infogats av alla tidigare körs:

    ``` tsql
    EXECUTE Demo.usp_DemoReset;
    ```

2. Kopiera hello texten i hello föregående ostress.exe kommandoraden tooyour Urklipp.

3. Ersätt hello `<placeholders>` för hello parametrar -S - U -P -d med hello korrigera verkliga värden.

4. Kör redigerade kommandoraden i ett RML Cmd-fönster.


#### <a name="result-is-a-duration"></a>Resultatet är en varaktighet


När ostress.exe är klar skriver hello kör varaktighet som den sista raden i utdata i hello RML kommandofönstret. Till exempel varade en kortare testkörning ungefär 1,5 minuter:

`11/12/15 00:35:00.873 [0x000030A8] OSTRESS exiting normally, elapsed time: 00:01:31.867`


#### <a name="reset-edit-for-ondisk-then-rerun"></a>Återställ, redigera för *_ondisk*, kör sedan


När du har hello resultatet från hello *_inmem* kör, utför följande steg för hello hello *_ondisk* kör:


1. Återställ hello databasen genom att köra följande kommando i SSMS toodelete hello alla hello-data som infogats av hello tidigare kör:
```
EXECUTE Demo.usp_DemoReset;
```

2. Redigera hello ostress.exe kommandoraden tooreplace alla *_inmem* med *_ondisk*.

3. Kör ostress.exe för hello gång och avbilda hello varaktighet resultat.

4. Igen och återställa hello-databasen (för ansvarsfullt om du tar bort kan vara en stor mängd testdata).


#### <a name="expected-comparison-results"></a>Förväntade Jämförelseresultat

Våra tester i minnet har visat att prestanda förbättras av **nio gånger** för den här simplistic arbetsbelastning med ostress körs på en Azure VM i hello samma Azure-region som hello-databas.

<a id="install_analytics_manuallink" name="install_analytics_manuallink"></a>

&nbsp;

## <a name="2-install-hello-in-memory-analytics-sample"></a>2. Installera hello Analytics InMemory-exempel


I det här avsnittet kan jämföra du hello i/o och resultat när du använder ett columnstore-index jämfört med traditionell b-trädindex.


För analys i realtid på en OLTP-arbetsbelastning är det ofta bäst toouse ett icke-grupperat columnstore-index. Mer information finns i [Columnstore-index beskrivs](http://msdn.microsoft.com/library/gg492088.aspx).



### <a name="prepare-hello-columnstore-analytics-test"></a>Förbereda hello columnstore analytics test


1. Använd hello Azure portal toocreate en ny AdventureWorksLT databas från hello exemplet.
 - Använda exakt samma namn.
 - Välj alla premiumnivån.

2. Kopiera hello [sql_in memory_analytics_sample](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/sql_in-memory_analytics_sample.sql) tooyour Urklipp.
 - Hej T-SQL-skript skapar hello nödvändiga InMemory-objekt i hello AdventureWorksLT exempeldatabasen som du skapade i steg 1.
 - hello skriptet skapar hälsningspaket dimensionstabellen och två faktatabeller. hello faktatabeller fylls i med 3.5 miljoner rader.
 - hello skript kan ta 15 minuter toocomplete.

3. Klistra in hello T-SQL-skript i SSMS och sedan köra hello skript. Hej **COLUMNSTORE** nyckelord i hello **CREATE INDEX** -instruktionen är mycket viktigt, som:<br/>`CREATE NONCLUSTERED COLUMNSTORE INDEX ...;`

4. Ange AdventureWorksLT toocompatibility nivå 130:<br/>`ALTER DATABASE AdventureworksLT SET compatibility_level = 130;`

    Nivå 130 är inte direkt relaterat tooIn minnet funktioner. Men nivå 130 erbjuder bättre frågeprestanda än 120.


#### <a name="key-tables-and-columnstore-indexes"></a>Viktiga tabeller och columnstore-index


- dbo. FactResellerSalesXL_CCI är en tabell som har ett grupperat columnstore-index som har avancerade komprimering vid hello *data* nivå.

- dbo. FactResellerSalesXL_PageCompressed är en tabell som har en motsvarande reguljära grupperat index, som komprimeras bara på hello *sidan* nivå.


#### <a name="key-queries-toocompare-hello-columnstore-index"></a>Viktiga frågor toocompare hello columnstore-index


Det finns [flera T-SQL-fråga typer som du kan köra](https://raw.githubusercontent.com/Microsoft/sql-server-samples/master/samples/features/in-memory/t-sql-scripts/clustered_columnstore_sample_queries.sql) toosee prestandaförbättringar. Betala uppmärksamhet toothis par frågor i steg 2 i hello T-SQL-skript. De skiljer sig bara på en rad:


- `FROM FactResellerSalesXL_PageCompressed a`
- `FROM FactResellerSalesXL_CCI a`


Ett grupperat columnstore-index är i hello FactResellerSalesXL\_CCI tabell.

hello följande T-SQL-skript utdrag skrivs ut statistik för i/o och tid för hello frågan för varje tabell.


```
/*********************************************************************
Step 2 -- Overview
-- Page Compressed BTree table v/s Columnstore table performance differences
-- Enable actual Query Plan in order toosee Plan differences when Executing
*/
-- Ensure Database is in 130 compatibility mode
ALTER DATABASE AdventureworksLT SET compatibility_level = 130
GO

-- Execute a typical query that joins hello Fact Table with dimension tables
-- Note this query will run on hello Page Compressed table, Note down hello time
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO

SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_PageCompressed a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO
SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO


-- This is hello same Prior query on a table with a clustered columnstore index CCI
-- hello comparison numbers are even more dramatic hello larger hello table is (this is an 11 million row table only)
SET STATISTICS IO ON
SET STATISTICS TIME ON
GO
SELECT c.Year
    ,e.ProductCategoryKey
    ,FirstName + ' ' + LastName AS FullName
    ,count(SalesOrderNumber) AS NumSales
    ,sum(SalesAmount) AS TotalSalesAmt
    ,Avg(SalesAmount) AS AvgSalesAmt
    ,count(DISTINCT SalesOrderNumber) AS NumOrders
    ,count(DISTINCT a.CustomerKey) AS CountCustomers
FROM FactResellerSalesXL_CCI a
INNER JOIN DimProduct b ON b.ProductKey = a.ProductKey
INNER JOIN DimCustomer d ON d.CustomerKey = a.CustomerKey
Inner JOIN DimProductSubCategory e on e.ProductSubcategoryKey = b.ProductSubcategoryKey
INNER JOIN DimDate c ON c.DateKey = a.OrderDateKey
GROUP BY e.ProductCategoryKey,c.Year,d.CustomerKey,d.FirstName,d.LastName
GO

SET STATISTICS IO OFF
SET STATISTICS TIME OFF
GO
```

Du kan förvänta dig om nio gånger hello prestandafördelar för den här frågan med hjälp av hello grupperade columnstore-indexet jämfört med traditionell hello-index i en databas med hello P2 prisnivå. Du kan förvänta dig ungefär 57 gånger hello prestandafördelar med P15, med hjälp av hello columnstore-index.



## <a name="next-steps"></a>Nästa steg

- [Snabbstart 1: Minnesintern OLTP-teknik för snabbare T-SQL-prestanda](http://msdn.microsoft.com/library/mt694156.aspx)

- [Använd InMemory-OLTP i ett befintligt Azure SQL-program](sql-database-in-memory-oltp-migration.md)

- [Övervakaren i minnet OLTP lagring](sql-database-in-memory-oltp-monitoring.md) för OLTP i minnet


## <a name="additional-resources"></a>Ytterligare resurser

#### <a name="deeper-information"></a>Mer detaljerad information

- [Lär dig hur kvorum fördubblar viktiga databasen arbetsbelastning och sänka DTU med 70% med InMemory-OLTP i SQL-databas](https://customers.microsoft.com/story/quorum-doubles-key-databases-workload-while-lowering-dtu-with-sql-database)

- [Minnesintern OLTP i Azure SQL Database blogginlägget](https://azure.microsoft.com/blog/in-memory-oltp-in-azure-sql-database/)

- [Lär dig mer om OLTP i minnet](http://msdn.microsoft.com/library/dn133186.aspx)

- [Lär dig mer om columnstore-index](https://msdn.microsoft.com/library/gg492088.aspx)

- [Lär dig mer om drift analys i realtid](http://msdn.microsoft.com/library/dn817827.aspx)

- Se [vanliga arbetsbelastning mönster och överväganden vid migrering](http://msdn.microsoft.com/library/dn673538.aspx) (som beskriver arbetsbelastning mönster där Minnesintern OLTP ofta ger betydande prestandavinster)

#### <a name="application-design"></a>Programmet design

- [OLTP (i minnet optimering) i minnet](http://msdn.microsoft.com/library/dn133186.aspx)

- [Använd InMemory-OLTP i ett befintligt Azure SQL-program](sql-database-in-memory-oltp-migration.md)

#### <a name="tools"></a>Verktyg

- [Azure Portal](https://portal.azure.com/)

- [SQL Server Management Studio (SSMS)](https://msdn.microsoft.com/library/mt238290.aspx)

- [SQL Server Data Tools (SSDT)](http://msdn.microsoft.com/library/mt204009.aspx)
