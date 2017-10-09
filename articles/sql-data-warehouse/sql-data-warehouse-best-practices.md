---
title: "aaaBest praxis för Azure SQL Data Warehouse | Microsoft Docs"
description: "Rekommendationer och metodtips som du bör känna till när du utvecklar lösningar för Azure SQL Data Warehouse. De hjälper dig att lyckas!"
services: sql-data-warehouse
documentationcenter: NA
author: shivaniguptamsft
manager: jhubbard
editor: 
ms.assetid: 7b698cad-b152-4d33-97f5-5155dfa60f79
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: performance
ms.date: 10/31/2016
ms.author: shigu;barbkess
ms.openlocfilehash: 1f42908fc03e1ca52278a6853d1afe9543d648b7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="best-practices-for-azure-sql-data-warehouse"></a>Metodtips för Azure SQL Data Warehouse
Den här artikeln är en samling av många metodtips som hjälper dig tooachieve optimal prestanda från din Azure SQL Data Warehouse.  Vissa av hello begrepp i den här artikeln är enkla och enkelt tooexplain, andra begrepp är mer avancerade och vi scratch bara hello yta i den här artikeln.  hello syftet med den här artikeln är toogive du vissa grundläggande vägledning och tooraise medvetenhet om viktiga områden toofocus som du skapar ditt data warehouse.  Varje avsnitt presenteras tooa begrepp och peka sedan toomore detaljerad artiklar vilket omfattar hello begrepp i djupare.

Om du precis har satt igång med Azure SQL Data Warehouse kan artikeln kännas överväldigande, men oroa dig inte.  hello följd av hello avsnitt är främst i hello prioritetsordning.  Om du startar genom att fokusera på hello först några begrepp som du kommer att i god form.  När du är mer van vid att använda SQL Data Warehouse kan du komma tillbaka och studera fler begrepp.  Det tar inte lång för allt toomake mening.

## <a name="reduce-cost-with-pause-and-scale"></a>Minska kostnaderna genom att pausa och skala
En nyckelfunktion i SQL Data Warehouse är hello möjlighet toopause när du inte använder den, vilket avbryter hello faktureringen av beräkningsresurser.  En annan nyckelegenskap är hello möjlighet tooscale resurser.  Pausa och skalning kan göras via hello Azure-portalen eller via PowerShell-kommandon.  Bekanta dig med funktionerna som de kan avsevärt minska hello kostnaden för ditt data warehouse när den inte används.  Om du vill att ditt data warehouse tillgänglig kan kanske du vill tooconsider skala ned toohello minsta storlek, en DW100 i stället för att pausa.

Se även [Pause compute resources][Pause compute resources] (Pausa beräkningsresurser), [Resume compute resources][Resume compute resources] (Återuppta beräkningsresurser) och [Scale compute resources] (Skala beräkningsresurser).

## <a name="drain-transactions-before-pausing-or-scaling"></a>Tömma transaktioner före pausning eller skalning
När du pausar eller skala ditt SQL Data Warehouse bakgrunden hello dina frågor har avbrutits när du initierar hello pausa eller skala begäran.  Om du avbryter en enkel urvalsfråga är en snabb åtgärd och har nästan ingen påverkan toohello tid det tar toopause eller skala din instans.  Dock kanske transaktionella frågor som ändra dina data eller hello datastrukturen hello inte kan toostop snabbt.  **Transaktionsfrågor måste per definition slutföras i sin helhet eller så måste ändringarna återställas.**  Återställer hello utfört transaktionella frågeresultatet kan ta lång eller ännu längre tid än hello ursprungliga ändra hello frågan installera.  Till exempel om du avbryter en fråga som tog bort rader och redan har körts under en timme, kan det ta hello system en timme tooinsert tillbaka hello rader som har tagits bort.  Om du kör paus eller skalning medan transaktioner är aktiverade, din paus eller skalning kan verka tootake har lång tid eftersom du pausar och skalning toowait för hello återställning toocomplete innan den kan fortsätta.

Se även [Understanding transactions][Understanding transactions] (Förstå transaktioner) och [Optimizing transactions][Optimizing transactions] (Optimera transaktioner)

## <a name="maintain-statistics"></a>Underhålla statistik
Till skillnad från SQL Server, som automatiskt identifierar och skapar eller uppdaterar statistik i kolumner, kräver SQL Data Warehouse manuellt underhåll av statistik.  Medan vi planerar toochange optimerad detta i hello framtida för nu vill du toomaintain din statistik tooensure som hello SQL Data Warehouse planer.  hello planer som skapats av hello optimering är bättre hello tillgänglig statistik.  **Provade statistik skapas för varje kolumn är ett enkelt sätt tooget igång med statistik.**  Det är lika viktigt tooupdate statistik som betydande ändringar sker tooyour data.  En försiktig metod kanske tooupdate statistiken varje dag eller efter varje inläsningen.  Det finns alltid avvägningarna mellan prestanda och hello kostnaden toocreate och uppdatera statistik. Om du hittar det tar för lång toomaintain alla din statistik kan kanske du vill tootry toobe mer noga med vilka kolumner har statistik eller vilka kolumner som behöver uppdateras regelbundet.  Exempelvis kanske du vill tooupdate datumkolumnerna där nya värden kan vara lagts till, varje dag. **Du kommer att få hello utnyttja genom att låta statistik för kolumner som ingår i kopplingar, kolumner som används i hello där satsen och kolumner finns i GROUP BY.**

Se även [Manage table statistics][Manage table statistics] (Hantera tabellstatistik), [CREATE STATISTICS][CREATE STATISTICS] och [UPDATE STATISTICS][UPDATE STATISTICS]

## <a name="group-insert-statements-into-batches"></a>Gruppera INSERT-satser i batchar
En enstaka belastningen tooa liten tabell med en INSERT-instruktion eller även en periodiska inläsning av en sökningen kan utföra fungerar bra för dina behov med en instruktion som `INSERT INTO MyLookup VALUES (1, 'Type 1')`.  Men om du behöver tooload tusentals eller miljoner rader i hela hello dag kan kanske du upptäcker att singleton INFOGNINGAR bara inte kan hålla.  Utveckla dina processer i stället så att de skriver tooa fil och en annan process som regelbundet kommer och läser in den här filen.

Se även [INSERT][INSERT]

## <a name="use-polybase-tooload-and-export-data-quickly"></a>Använd PolyBase tooload och exportera data snabbt
SQL Data Warehouse stöder inläsning och export av data via flera verktyg, bland annat Azure Data Factory, PolyBase och BCP.  För små datamängder där prestanda inte är viktigt räcker alla verktygen för dina behov.  När du läser in eller export av stora mängder data eller snabb prestanda krävs, är PolyBase hello bästa valet.  PolyBase kommer är utformad tooleverage hello (massivt parallell bearbetning) MPP-arkitektur för SQL Data Warehouse och därför att läsa in och exportera data omfattningen snabbare än med andra verktyg.  PolyBase-inläsningar kan utföras med hjälp av CTAS eller INSERT INTO.  **Med hjälp av CTAS minimeras transaktionsloggning och hello snabbaste sättet tooload dina data.**  Azure Data Factory stöder också PolyBase-inläsningar.  PolyBase stöder olika filformat, inklusive Gzip-filer.  **toomaximize dataflöde när du använder gzip textfiler, break till 60 eller flera filer toomaximize parallellitet din belastningen.**  För snabbare totalt genomflöde bör du överväga att använda samtidig inläsning av data.

Se även [Load data][Load data] (Läsa in data), [Guide for using PolyBase][Guide for using PolyBase] (Guide för att använda PolyBase), [Azure SQL Data Warehouse loading patterns and strategies][Azure SQL Data Warehouse loading patterns and strategies] (Azure SQL Data Warehouse: inläsningsmönster och strategier), [Load Data with Azure Data Factory][Load Data with Azure Data Factory] (Läsa in data med Azure Data Factory), [Move data with Azure Data Factory][Move data with Azure Data Factory] (Flytta data med Azure Data Factory), [CREATE EXTERNAL FILE FORMAT][CREATE EXTERNAL FILE FORMAT], [Create Table As Select (CTAS)][Create table as select (CTAS)]

## <a name="load-then-query-external-tables"></a>Läsa in och sedan fråga externa tabeller
Polybase, även kallat externa tabeller kan vara hello snabbaste sättet tooload data, är det inte optimalt för frågor. Polybase-tabeller i SQL Data Warehouse har för närvarande endast stöd för Azure-blobbfiler. Dessa filer har inte några beräkningsresurser som backar upp dem.  Därför kan avlasta detta verk SQL Data Warehouse och därför måste läsa hello hela filen genom att läsa in den tootempdb i ordning tooread hello data.  Därför om du har flera frågor som kommer att fråga data det är bättre tooload dessa data en gång och har frågor hello lokal tabell.

Se även [Guide for using PolyBase][Guide for using PolyBase] (Guide för att använda PolyBase)

## <a name="hash-distribute-large-tables"></a>Hash-distribuera stora tabeller
Tabeller distribueras som standard med resursallokering (Round Robin).  Det gör det lättare för användare tooget igång med att skapa tabeller utan toodecide hur tabellerna ska distribueras.  Resursallokeringstabeller kan prestera bra för vissa arbetsbelastningar, men i de flesta fall blir prestanda bättre om du väljer en distributionskolumn.  hello vanligaste exempel på när en tabell som distribueras av en kolumn långt överträffar en resursallokering tabell är när två stora faktatabeller är anslutna.  Till exempel om du har en ordertabell, som distribueras av order_id, och en tabell för transaktioner som distribueras också av order_id, när du ansluter din order tabell tooyour transaktioner tabell på order_id, blir den här frågan en direktfråga, vilket innebär att Vi eliminera dataflyttsåtgärderna.  Färre steg innebär en snabbare fråga.  Mindre dataflyttning gör också att frågor körs snabbare.  Denna förklaring bara repor hello yta. Glöm inte att inkommande data inte sorteras på hello distribution tangent som det tar lång tid att din belastning vid inläsning av en distribuerad tabell.  Se hello nedan länkar för mycket mer information om hur att markera en kolumn för distribution kan förbättra prestanda och hur toodefine en tabell som är distribuerade i hello WITH-satsen i instruktionen skapa tabeller.

Se även [Table overview][Table overview] (Tabellöversikt), [Table distribution][Table distribution] (Tabelldistribution), [Selecting table distribution][Selecting table distribution] (Välja tabelldistribution), [CREATE TABLE][CREATE TABLE], [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="do-not-over-partition"></a>Överpartitionera inte
Partitionering av data kan vara mycket effektivt för att hantera data genom partitionsväxlingar eller optimeringsgenomsökningar med partitionseliminering, men för många partitioner kan göra att dina frågor körs långsammare.  En partitioneringsstrategi med hög granularitet som fungerar bra med SQL Server fungerar ofta inte lika bra med SQL Data Warehouse.  Med för många partitioner kan också minska hello effektivitet med grupperade columnstore-index om varje partition har färre än 1 miljon rader.  Tänk på att hello bakgrunden SQL Data Warehouse partitionerar data du i 60 databaser, så om du skapar en tabell med 100 partitioner det faktiskt leder 6000 partitioner.  Varje arbetsbelastning är olika så hello rekommendationer är tooexperiment med partitionering toosee vad som fungerar bäst för din arbetsbelastning.  Överväg att använda lägre granularitet än vad som har fungerat i SQL Server.  Överväg exempelvis att använda veckovisa eller månadsvisa partitioneringar i stället för dagliga.

Se även [Table partitioning][Table partitioning] (Tabellpartitionering)

## <a name="minimize-transaction-sizes"></a>Minimera transaktionsstorlekar
INSERT-, UPDATE- och DELETE-instruktioner körs i en transaktion och när de misslyckas måste de återställas.  toominimize hello potential för en lång återställning enklare transaktion storlekar när det är möjligt.  Du kan göra det genom att dela upp INSERT-, UPDATE- och DELETE-instruktioner i flera delar.  Till exempel om du har en INSERT som du förväntar dig tootake 1 timme, om möjligt dela hello INSERT upp i 4 delar som körs varje i 15 minuter.  Utnyttja Minimal loggning specialfall, som CTAS, TRUNCATE, DROP TABLE eller infoga tooempty tabeller, tooreduce återställning risk.  Ett annat sätt tooeliminate återställningar är toouse Metadata endast åtgärder som partitionsväxling för datahantering.  Till exempel snarare än köra en DELETE-instruktion toodelete alla rader i en tabell där hello order_date var i oktober 2001 du kunde partitionerar data varje månad och sedan växla ut hello partitionen med data för en tom partition från en annan tabell (se ALTER TABELLEN exempel).  Bör du använda en CTAS toowrite hello-data som du vill ha tookeep i en tabell i stället för att använda DELETE för opartitionerat tabeller.  Om en CTAS tar Hej lika lång tid, är det en säkrare toorun åtgärden eftersom den har minimala transaktionsloggning och kan avbrytas snabbt vid behov.

Se även [Understanding transactions][Understanding transactions] (Förstå transaktioner), [Optimizing transactions][Optimizing transactions] (Optimera transaktioner), [Table partitioning][Table partitioning] (Tabellpartitionering), [TRUNCATE TABLE][TRUNCATE TABLE], [ALTER TABLE][ALTER TABLE], [Create Table As Select (CTAS)][Create table as select (CTAS)]

## <a name="use-hello-smallest-possible-column-size"></a>Använd hello minsta möjliga kolumnstorlek
När du definierar din DDL förbättras med hello minsta datatypen som stöder data frågeprestanda.  Detta är särskilt viktigt för CHAR- och VARCHAR-kolumner.  Om hello längsta värde i en kolumn är 25 tecken, definierar du kolumnen som VARCHAR(25).  Undvik att definiera alla kolumner tooa stora standard tecken.  Definiera också kolumner som VARCHAR när det är allt som krävs i stället för att använda NVARCHAR.

Se även [Table overview][Table overview] (Tabellöversikt), [Table data types][Table data types] (Datatyper för tabeller) och [CREATE TABLE][CREATE TABLE]

## <a name="use-temporary-heap-tables-for-transient-data"></a>Använda tillfälliga heap-tabeller för tillfälliga data
När du tillfälligt hamnar data i SQL Data Warehouse, kan du använda en heap-tabell kommer att hello övergripande processen snabbare.  Om du läser in klustrad data endast toostage innan du kör flera transformationer som läser in hello tabellen tooheap blir mycket snabbare än inläsning hello data tooa columnstore-tabellen.  Dessutom laddar ladda data tooa temporära tabellen också mycket snabbare än en tabell toopermanent lagringen lästes in.  Temporära tabeller börjar med ”#” och kan bara kommas åt av hello-session som har skapat den, så att de fungerar bara i begränsade fall.   Heap-tabeller har definierats i hello WITH-satsen i en CREATE TABLE.  Kom ihåg toocreate statistik för den temporära tabellen för om du använder en temporär tabell.

Se även [Temporary tables][Temporary tables] (Temporära tabeller), [CREATE TABLE][CREATE TABLE] och [CREATE TABLE AS SELECT][CREATE TABLE AS SELECT]

## <a name="optimize-clustered-columnstore-tables"></a>Optimera grupperade columnstore-tabeller
Grupperade columnstore-index är en av hello mest effektiva sätten som du kan lagra data i Azure SQL Data Warehouse.  Som standard skapas tabeller i SQL Data Warehouse som grupperade ColumnStore-tabeller.  tooget hello bästa prestanda för frågor på columnstore-tabeller har bra segment kvalitet är viktigt.  När rader skrivs toocolumnstore tabeller minnesbelastning, drabbas columnstore segment kvalitet.  Segmentkvaliteten kan mätas utifrån antalet rader i en komprimerad radgrupp.  Se hello [orsaker till dåliga columnstore-index kvalitet] [ Causes of poor columnstore index quality] i hello [tabell index] [ Table indexes] artikel steg för steg instruktioner om identifiera och förbättra kvaliteten segment för grupperade columnstore-tabeller.  Eftersom hög kvalitet columnstore segment är viktigt, är det en bra idé toouse användare-ID: n som är i hello medelstora eller stora resursklassen för inläsning av data.  Hej färre Dwu du använder hello större hello resursklassen vill tooassign tooyour inläsning av användaren.

Eftersom columnstore-tabeller vanligtvis inte skicka data till en komprimerad columnstore segment tills det finns mer än 1 miljoner rader per tabell och varje SQL Data Warehouse-tabellen är partitionerad i 60 tabeller som en tumregel dra columnstore-tabeller inte en fråga Om inte tabellen hello har mer än 60 miljoner rader.  Tabell med mindre än 60 miljoner rader, kan det inte vara en mening toohave ett columnstore-index.  Men det skadar inte heller.  Dessutom, om du partitionera data sedan vill tooconsider att varje partition måste toohave 1 miljon rader toobenefit från ett grupperat columnstore-index.  Om en tabell har 100 partitioner, så måste den toohave minst 6 miljarder rader toobenefit från en butik med grupperade kolumner (60 distributioner * 100 partitioner * 1 miljoner rader).  Om din tabell inte har 6 miljarder rader i det här exemplet, minska hello antalet partitioner eller Överväg att använda en heap-tabellen i stället.  Det kan också vara värt att experimentera toosee om kan vinna bättre prestanda med en heap-tabell med sekundärindex i stället för en columnstore-tabell.  Columnstore-tabeller stöder inte sekundära index än.

När du frågar ett columnstore-tabellen, köra frågor snabbare om du väljer endast hello kolumner som du behöver.  

Se även [Table indexes][Table indexes] (Tabellindex), [Columnstore indexes guide][Columnstore indexes guide] (Guide för columnstore-index), [Rebuilding columnstore indexes][Rebuilding columnstore indexes] (Återskapa columnstore-index)

## <a name="use-larger-resource-class-tooimprove-query-performance"></a>Använda större resurs klassen tooimprove frågeprestanda
SQL Data Warehouse använder resursgrupper som ett sätt tooallocate minne tooqueries.  Out of box hello tilldelas alla användare toohello små resursklass som ger 100 MB minne per distribution.  Eftersom det finns alltid ges minst 100 MB, wide hello totala systemminnet allokeringen är 6 000 MB eller bara under 6 GB i 60 distributioner och varje distribution.  Vissa frågor som stora kopplingar eller belastningar tooclustered columnstore-tabeller, drar nytta av större minnesallokering.  Vissa frågor, t.ex. rena genomsökningar, har ingen nytta av detta.  På hello Vänd sida påverkar använder större resursklasser samtidighet, så ska tootake detta i åtanke innan du flyttar alla dina användare tooa stora resursklassen.

Se även [Concurrency and workload management][Concurrency and workload management] (Hantering av samtidiga körningar och arbetsbelastningar)

## <a name="use-smaller-resource-class-tooincrease-concurrency"></a>Använda mindre resursklassen tooIncrease samtidighet
Om du granska resultatet att användarfrågor verka toohave lång tid, det kan bero på användarna körs i större resursklasser och tar upp mycket samtidighet fack orsakar andra frågor tooqueue upp.  toosee användare frågor köas kör `SELECT * FROM sys.dm_pdw_waits` toosee om några rader returneras.

Se även [Concurrency and workload management][Concurrency and workload management] (Hantering av samtidiga körningar och arbetsbelastningar) och [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="use-dmvs-toomonitor-and-optimize-your-queries"></a>Använd av DMV: er toomonitor och optimera dina frågor
SQL Data Warehouse har flera av DMV: er som kan vara används toomonitor Frågekörningen.  hello övervakning artikel nedan går igenom stegvisa instruktioner för hur toolook hello information av en pågående fråga.  använda hello etikett och dina frågor kan hjälpa tooquickly hitta frågor i dessa av DMV: er.

Se även [Monitor your workload using DMVs][Monitor your workload using DMVs] (Övervaka arbetsbelastningen med datahanteringsvyer), [LABEL][LABEL], [OPTION][OPTION], [sys.dm_exec_sessions][sys.dm_exec_sessions], [sys.dm_pdw_exec_requests][sys.dm_pdw_exec_requests], [sys.dm_pdw_request_steps][sys.dm_pdw_request_steps], [sys.dm_pdw_sql_requests][sys.dm_pdw_sql_requests], [sys.dm_pdw_dms_workers], [DBCC PDW_SHOWEXECUTIONPLAN][DBCC PDW_SHOWEXECUTIONPLAN], [sys.dm_pdw_waits][sys.dm_pdw_waits]

## <a name="other-resources"></a>Andra resurser
Information om vanliga problem och lösningar finns i vår [felsökningsartikel][Troubleshooting].

Om du inte hittar det du söker efter i den här artikeln kan du prova att använda hello ”Sök efter dokument” hello vänster för den här sidan toosearch alla hello Azure SQL Data Warehouse-dokument.  Hej [Azure SQL Data Warehouse MSDN-Forum] [ Azure SQL Data Warehouse MSDN Forum] var att skapa som en plats om du tooask frågor tooother användare och toohello produktgruppen för SQL Data Warehouse.  Vi övervaka aktivt den här forumet tooensure dina frågor besvaras antingen av en annan användare eller en av oss.  Om du vill tooask dina frågor på Stack Overflow kan vi har också en [Azure SQL Data Warehouse Stack Overflow-Forum][Azure SQL Data Warehouse Stack Overflow Forum].

Slutligen, Använd hello [Azure SQL Data Warehouse Feedback] [ Azure SQL Data Warehouse Feedback] sidan toomake funktioner som efterfrågas.  Genom att skicka in dina egna önskemål eller rösta fram andras förfrågningar hjälper du oss att prioritera funktioner.

<!--Image references-->

<!--Article references-->
[Create a support ticket]: ./sql-data-warehouse-get-started-create-support-ticket.md
[Concurrency and workload management]: ./sql-data-warehouse-develop-concurrency.md
[Create table as select (CTAS)]: ./sql-data-warehouse-develop-ctas.md
[Table overview]: ./sql-data-warehouse-tables-overview.md
[Table data types]: ./sql-data-warehouse-tables-data-types.md
[Table distribution]: ./sql-data-warehouse-tables-distribute.md
[Table indexes]: ./sql-data-warehouse-tables-index.md
[Causes of poor columnstore index quality]: ./sql-data-warehouse-tables-index.md#causes-of-poor-columnstore-index-quality
[Rebuilding columnstore indexes]: ./sql-data-warehouse-tables-index.md#rebuilding-indexes-to-improve-segment-quality
[Table partitioning]: ./sql-data-warehouse-tables-partition.md
[Manage table statistics]: ./sql-data-warehouse-tables-statistics.md
[Temporary tables]: ./sql-data-warehouse-tables-temporary.md
[Guide for using PolyBase]: ./sql-data-warehouse-load-polybase-guide.md
[Load data]: ./sql-data-warehouse-overview-load.md
[Move data with Azure Data Factory]: ../data-factory/data-factory-azure-sql-data-warehouse-connector.md
[Load data with Azure Data Factory]: ./sql-data-warehouse-get-started-load-with-azure-data-factory.md
[Load data with bcp]: ./sql-data-warehouse-load-with-bcp.md
[Load data with PolyBase]: ./sql-data-warehouse-get-started-load-with-polybase.md
[Monitor your workload using DMVs]: ./sql-data-warehouse-manage-monitor.md
[Pause compute resources]: ./sql-data-warehouse-manage-compute-overview.md#pause-compute-bk
[Resume compute resources]: ./sql-data-warehouse-manage-compute-overview.md#resume-compute-bk
[Scale compute resources]: ./sql-data-warehouse-manage-compute-overview.md#scale-compute (Skala beräkningsresurser)
[Understanding transactions]: ./sql-data-warehouse-develop-transactions.md
[Optimizing transactions]: ./sql-data-warehouse-develop-best-practices-transactions.md
[Troubleshooting]: ./sql-data-warehouse-troubleshoot.md
[LABEL]: ./sql-data-warehouse-develop-label.md

<!--MSDN references-->
[ALTER TABLE]: https://msdn.microsoft.com/library/ms190273.aspx
[CREATE EXTERNAL FILE FORMAT]: https://msdn.microsoft.com/library/dn935026.aspx
[CREATE STATISTICS]: https://msdn.microsoft.com/library/ms188038.aspx
[CREATE TABLE]: https://msdn.microsoft.com/library/mt203953.aspx
[CREATE TABLE AS SELECT]: https://msdn.microsoft.com/library/mt204041.aspx
[DBCC PDW_SHOWEXECUTIONPLAN]: https://msdn.microsoft.com/library/mt204017.aspx
[INSERT]: https://msdn.microsoft.com/library/ms174335.aspx
[OPTION]: https://msdn.microsoft.com/library/ms190322.aspx
[TRUNCATE TABLE]: https://msdn.microsoft.com/library/ms177570.aspx
[UPDATE STATISTICS]: https://msdn.microsoft.com/library/ms187348.aspx
[sys.dm_exec_sessions]: https://msdn.microsoft.com/library/ms176013.aspx
[sys.dm_pdw_exec_requests]: https://msdn.microsoft.com/library/mt203887.aspx
[sys.dm_pdw_request_steps]: https://msdn.microsoft.com/library/mt203913.aspx
[sys.dm_pdw_sql_requests]: https://msdn.microsoft.com/library/mt203889.aspx
[sys.dm_pdw_dms_workers]: https://msdn.microsoft.com/library/mt203878.aspx
[sys.dm_pdw_waits]: https://msdn.microsoft.com/library/mt203893.aspx
[Columnstore indexes guide]: https://msdn.microsoft.com/library/gg492088.aspx

<!--Other Web references-->
[Selecting table distribution]: https://blogs.msdn.microsoft.com/sqlcat/2015/08/11/choosing-hash-distributed-table-vs-round-robin-distributed-table-in-azure-sql-dw-service/
[Azure SQL Data Warehouse Feedback]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[Azure SQL Data Warehouse MSDN Forum]: https://social.msdn.microsoft.com/Forums/sqlserver/home?forum=AzureSQLDataWarehouse
[Azure SQL Data Warehouse Stack Overflow Forum]:  http://stackoverflow.com/questions/tagged/azure-sqldw
[Azure SQL Data Warehouse loading patterns and strategies]: https://blogs.msdn.microsoft.com/sqlcat/2016/02/06/azure-sql-data-warehouse-loading-patterns-and-strategies
