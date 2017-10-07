---
title: "aaaWhat är Azure SQL Data Warehouse? | Microsoft Docs"
description: "En distribuerad databas i företagsklass som kan bearbeta petabytevolymer med relationella och icke-relationella data. Det är hello branschens första informationslager i molnet växa, krympa och pausa på sekunden."
services: sql-data-warehouse
documentationcenter: NA
author: jrowlandjones
manager: bjhubbard
editor: 
ms.assetid: 4006c201-ec71-4982-b8ba-24bba879d7bb
ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: hero-article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.custom: overview
ms.date: 2/28/2017
ms.author: jrj;barbkess
ms.openlocfilehash: 5fefe40879230f123c2e4a90b9c20a35779cf711
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="what-is-azure-sql-data-warehouse"></a>Vad är Azure SQL Data Warehouse?
Azure SQL Data Warehouse är en molnbaserad, skalbar, relationell databas med Massivt Parallell Bearbetning (MPP) som kan bearbeta massiva mängder data. 

SQL Data Warehouse:

* Kombinerar relationella hello SQL Server-databas med Azure skalbara molnfunktioner. 
* Frikopplar lagring från beräkning.
* Aktiverar ökning, minskning, pausande eller återupptagande av beräkning. 
* Integrerar över hello Azure-plattformen.
* Använder SQL Server Transact-SQL (T-SQL) och verktyg.
* Uppfyller olika juridiska och affärssäkerhetskrav som SOC och ISO.

Den här artikeln beskriver hello viktiga funktioner i SQL Data Warehouse.

## <a name="massively-parallel-processing-architecture"></a>MPP-arkitektur (Massively Parallel Processing)
SQL Data Warehouse är ett distribuerat MPP-databassystem. Sprider ut dina data i många shared-nothing-lagrings- och bearbetningsenheter hello bakgrunden SQL Data Warehouse. hello data lagras i ett Premium lokalt redundant lagring lager ovanpå som dynamiskt länkade datornoderna köra frågor. SQL Data Warehouse tar en ”dela och Erövra” metoden toorunning läser in och komplexa frågor. Begäranden tas emot av en Control-noden, optimerad för distribution och därefter passerar tooCompute noder toodo sitt arbete för parallellt.

Med fristående lagringsutrymme och databearbetning kan SQL Data Warehouse:

* Öka eller minska lagringsstorlek oberoende av beräkning.
* Öka eller minska beräkningskapacitet utan att flytta data.
* Pausa beräkningskapaciteten, lämna data intakta och betala endast för lagring.
* Återuppta beräkningskapacitet under driftstimmar.

hello följande diagram visar hello arkitektur i detalj.

![SQL Data Warehouse-arkitektur][1]

**Control-noden:** hello Control-noden hanterar och optimerar frågor. Det är hello klientdelen som interagerar med alla program och anslutningar. Hello Control-noden drivs av SQL-databas i SQL Data Warehouse och ansluter tooit ser ut och känns hello samma. Under hello ytan samordnar Control-noden hello alla hello dataförflyttning och beräkningar som krävs för toorun parallella frågor på dina distribuerade data. När du skickar en T-SQL-fråga tooSQL Data Warehouse, omvandlar hello Control-noden den till separata frågor som körs på varje beräkningsnod parallellt.

**Compute-noder:** hello Compute-noderna utgör som hello kraften bakom SQL Data Warehouse. De är SQL-databaser som lagrar dina data och bearbetar din fråga. När du lägger till data, distribuerar SQL Data Warehouse hello rader tooyour Compute-noder. hello Compute-noderna är hello anställda som kör hello parallella frågor på dina data. Efter bearbetning skickar hello resultaten tillbaka toohello Control-noden. toofinish hello frågan, aggregerar hello Control-noden hello resultaten och returnerar hello slutresultatet.

**Lagring:** Dina data lagras i Azure Blob Storage. När Compute-noder interagerar med dina data, skriva och läsa direkt tooand från blob storage. Eftersom Azure-lagring expanderar helt transparent och frågeprestanda avsevärt, SQL Data Warehouse kan göra hello samma. Eftersom beräkning och lagring sker oberoende av varandra, kan SQL Data Warehouse automatiskt skala lagring separat från beräkning och vice versa. Azure Blob storage är också fullständigt feltolerant och förenklar hello säkerhetskopiering och återställning.

**Data Movement Service:** Data Movement Service (DMS) flyttar data mellan hello noder. DMS ger hello Compute-noder åtkomst toodata de behöver för kopplingar och aggregeringar. DMS är inte en Azure-tjänst. Det är en windowstjänst som körs samtidigt som SQL-databasen på alla hello-noder. DMS är en bakgrundsprocess som inte går att interagera med direkt. Men du kan titta på frågan planer toosee när DMS-åtgärder sker, eftersom dataflyttning är nödvändiga toorun varje fråga parallellt.

## <a name="optimized-for-data-warehouse-workloads"></a>Optimerad för arbetsbelastningar i informationslager
hello MPP-metoden hjälp av flera datalagring specifika prestandaoptimeringarna, inklusive:

* En distribuerad frågeoptimerare och en uppsättning komplex statistik för alla data. Med information om datastorlek och distribution, är hello-tjänsten kan toooptimize frågor genom att bedöma hello kostnaden för specifika distribuerade frågeåtgärder.
* Avancerade algoritmer och tekniker integreras hello data movement processen tooefficiently flytta data mellan bearbetningsresurser efter behov tooperform hello frågan. De här dataflyttsåtgärderna är inbyggda i och alla optimeringar toohello Data Movement Service sker automatiskt.
* Grupperade **columnstore**-index (kolumnlagringsindex) som standard. Med hjälp av kolumnbaserad lagring får SQL Data Warehouse i genomsnitt 5 x komprimering vinster över traditionell, radbaserad lagring och upp too10x eller mer frågeprestanda. Analytics-frågor som behöver tooscan ett stort antal rader fungerar bättre med columnstore-index.


## <a name="predictable-and-scalable-performance-with-data-warehouse-units"></a>Förutsägbara och skalbara prestanda med informationslagerenheter
SQL Data Warehouse är byggt med liknande teknik som SQL Database, vilket innebär att experter kan förvänta sig konsekventa och förutsägbara prestanda för analytiska frågor. Användare kan förvänta toosee prestanda skala linjärt som de lägger till eller ta bort Compute-noder. Allokering av resurser tooyour SQL Data Warehouse mäts i Informationslagerenheter (dwu: er). Dwu: er är ett mått på underliggande resurser, t.ex. CPU, minne, IOPS som är allokerade tooyour SQL Data Warehouse. Öka hello antalet dwu: er ökar resurser och prestanda. Mer specifikt ser DWU-enheterna till att:

* Du kommer kunna tooscale datalager utan att oroa hello underliggande maskinvara eller programvara.
* Du kan förutsäga förbättring av prestanda för en DWU-nivå innan du ändrar hello beräkning för ditt informationslager.
* hello kan underliggande maskinvara och programvara för din instans ändras eller flyttas utan att påverka arbetsbelastningens prestanda.
* Microsoft kan förbättra hello underliggande arkitekturen för hello tjänsten utan att påverka hello prestandan för din arbetsbelastning.
* Microsoft snabbt kan förbättra prestanda i SQL Data Warehouse på ett sätt som är skalbart och jämnt effekter hello system.

DWU-enheter ger ett mått på tre mätvärden med stark korrelation till informationslagrets arbetsbelastningsprestanda. hello följande nyckel arbetsbelastning mått skala linjärt med hello dwu: er.

**Genomsökning/aggregering:** En standardinformationslagerfråga som genomsöker ett stort antal rader och sedan genomför en komplex aggregering. Det här är en I/O- och processorintensiv åtgärd.

**Läs in:** hello möjlighet tooingest data i hello-tjänsten. Inläsningar utförs bäst med PolyBase från Azure Storage Blob eller Azure Data Lake. Detta mått är utformad toostress nätverks- och CPU-aspekter av hello-tjänsten.

**Skapa tabell som Select (CTAS):** CTAS mäter hello möjlighet toocopy en tabell. Det innebär att läsa data från lagring, distribuera den över hello noder hello-enhetens och skriva toostorage igen. Det här är en processor-, I/O- och nätverksintensiv åtgärd.

## <a name="built-on-sql-server"></a>Bygger på SQL Server
SQL Data Warehouse är baserat på hello SQL Servers relationella databasmotor och innehåller många hello-funktioner som du förväntar dig från ett informationslager i företagsklass. Om du redan känner till T-SQL, är det enkelt tootransfer din knowledge tooSQL Data Warehouse. Om du har avancerade kunskaper eller bara precis kommit igång, hjälper hello exemplen i dokumentationen för hello börjar. Generellt sett kan du tänka hello sätt som vi byggt hello språkliga elementen i SQL Data Warehouse på följande sätt:

* SQL Data Warehouse använder T-SQL-syntaxen för många åtgärder. Det finns också stöd för en omfattande uppsättning traditionella SQL-konstruktioner som lagrade procedurer, användardefinierade funktioner, tabellpartitionering, index och sortering.
* SQL Data Warehouse innehåller även ett antal nyare SQL Server-funktioner som grupperade **columnstore**-index, PolyBase-integrering och datagranskning (med hotbedömning).
* Vissa T-SQL-språkelement som är mindre vanliga för arbetsbelastningar i informationslager, eller som är nyare tooSQL-servern är kanske inte tillgänglig för närvarande. Mer information finns i hello [Migreringsdokumentation][Migration documentation].

Med hello Transact-SQL och funktionen funktionslikheterna mellan SQL Server, SQL Data Warehouse, SQL Database och Analytics Platform System, kan du utveckla en lösning som passar dina databehov. Du kan välja där tookeep dina data baserat på prestanda, säkerhet, och skalningskrav och sedan överföra data vid behov mellan olika system.

## <a name="data-protection"></a>Dataskydd
SQL Data Warehouse lagrar alla data i lokalt redundant Azure Premium-lagring. Flera synkrona kopior av hello data bevaras i hello lokala data center tooguarantee transparent dataskydd mot lokaliserade fel. Dessutom säkerhetskopierar SQL Data Warehouse automatiskt dina aktiva (inte pausade) databaser med jämna mellanrum med hjälp av Azure Storage-ögonblicksbilder. toolearn mer om hur säkerhetskopierar och återställer fungerar, se hello [översikt över säkerhetskopiering och återställning][Backup and restore overview].

## <a name="integrated-with-microsoft-tools"></a>Integrerat med Microsoft-verktyg
SQL Data Warehouse integrerat många av hello verktyg som SQL Server-användare är bekanta med. Dessa verktyg innefattar:

**Traditionella SQL Server-verktyg:** SQL Data Warehouse är helt integrerat med SQL Server Analysis Services, Integration Services och Reporting Services.

**Molnbaserade verktyg:** SQL Data Warehouse kan integreras tillsammans med ett antal tjänster i Azure, inklusive Data Factory, Stream Analytics, Machine Learning och Power BI. En mer komplett lista finns i [Översikt över integrerade verktyg][Integrated tools overview].

**Verktyg från tredje part:** Många leverantörer av tredjepartsverktyg har certifierad integrering av sina verktyg med SQL Data Warehouse. En fullständig lista finns i [SQL Data Warehouse-samarbetspartner][SQL Data Warehouse solution partners].

## <a name="hybrid-data-sources-scenarios"></a>Scenarier för hybriddatakällor
Polybase låter dig tooleverage dina data från olika källor med bekanta T-SQL-kommandon. Polybase låter dig tooquery icke-relationella data som lagras i Azure Blob storage som om det är en vanlig tabell. Använd Polybase tooquery icke-relationella data eller tooimport icke-relationella data till SQL Data Warehouse.

* PolyBase använder sig av externa tabeller tooaccess icke-relationella data. Hej tabelldefinitionerna lagras i SQL Data Warehouse och du kommer åt dem med hjälp av SQL och verktyg som du skulle använda för normala, relationella data.
* Polybase är integreringsneutralt. Den visar hello samma funktioner och funktionalitet tooall hello källor som det stöder. hello data läses av Polybase kan vara i olika format, inklusive avgränsade eller ORC-filer.
* PolyBase kan vara används tooaccess blobblagring som även används som lagring för ett HDInsight-kluster. Detta ger dig åtkomst toohello samma data med relationella och icke-relationella verktyg.

## <a name="sla"></a>SLA
SQL Data Warehouse erbjuder ett servicenivåavtal (SLA) på produktnivå som en del av serviceavtalet för Microsoft Online Services. Mer information finns på sidan om [SLA för SQL Data Warehouse][SLA for SQL Data Warehouse]. SLA-information om andra produkter kan du besöka hello [serviceavtal] Azure sidan eller hämta dem på hello [volymlicensiering] [ Volume Licensing] sidan. 

## <a name="next-steps"></a>Nästa steg
Nu när du vet lite om SQL Data Warehouse, lär du dig hur tooquickly [skapa ett SQL Data Warehouse] [ create a SQL Data Warehouse] och [läsa in exempeldata][load sample data]. Om du är ny tooAzure kan du hitta hello [Azure ordlista] [ Azure glossary] användbara när du får nya terminologi. Eller så kan du se över några av de övriga SQL Data Warehouse-resurserna.  

* [Kundernas framgångsberättelser]
* [Bloggar]
* [Funktionsbegäranden]
* [Videoklipp]
* [Customer Advisory Team-bloggar]
* [Skapa ett supportärende]
* [MSDN-forum]
* [Stack Overflow-forum]
* [Twitter]

<!--Image references-->
[1]: ./media/sql-data-warehouse-overview-what-is/dwarchitecture.png

<!--Article references-->
[Skapa ett supportärende]: ./sql-data-warehouse-get-started-create-support-ticket.md
[load sample data]: ./sql-data-warehouse-load-sample-databases.md
[create a SQL Data Warehouse]: ./sql-data-warehouse-get-started-provision.md
[Migration documentation]: ./sql-data-warehouse-overview-migrate.md
[SQL Data Warehouse solution partners]: ./sql-data-warehouse-partner-business-intelligence.md
[Integrated tools overview]: ./sql-data-warehouse-overview-integrate.md
[Backup and restore overview]: ./sql-data-warehouse-restore-database-overview.md
[Azure glossary]: ../azure-glossary-cloud-terminology.md

<!--MSDN references-->

<!--Other Web references-->
[Kundernas framgångsberättelser]: https://azure.microsoft.com/case-studies/?service=sql-data-warehouse
[Bloggar]: https://azure.microsoft.com/blog/tag/azure-sql-data-warehouse/
[Customer Advisory Team-bloggar]: https://blogs.msdn.microsoft.com/sqlcat/tag/sql-dw/
[Funktionsbegäranden]: https://feedback.azure.com/forums/307516-sql-data-warehouse
[MSDN-forum]: https://social.msdn.microsoft.com/Forums/azure/en-US/home?forum=AzureSQLDataWarehouse
[Stack Overflow-forum]: http://stackoverflow.com/questions/tagged/azure-sqldw
[Twitter]: https://twitter.com/hashtag/SQLDW
[Videoklipp]: https://azure.microsoft.com/documentation/videos/index/?services=sql-data-warehouse
[SLA for SQL Data Warehouse]: https://azure.microsoft.com/en-us/support/legal/sla/sql-data-warehouse/v1_0/
[Volume Licensing]: http://www.microsoftvolumelicensing.com/DocumentSearch.aspx?Mode=3&DocumentTypeId=37
[serviceavtal]: https://azure.microsoft.com/en-us/support/legal/sla/
