---
title: "aaaAzure SQL-databas elastisk frågan översikt | Microsoft Docs"
description: "Översikt över hello elastisk fråga funktionen"
services: sql-database
documentationcenter: 
manager: jhubbard
author: MladjoA
ms.assetid: a8bf0e2c-bc74-44d0-9b1e-bcc9a6aa2e33
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/27/2016
ms.author: mlandzic
ms.openlocfilehash: db17f551882cfcae0da67fdda12708baeb6db81c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="azure-sql-database-elastic-query-overview-preview"></a>SQL-databas elastisk frågan översikt över Azure (förhandsversion)
hello elastisk fråga funktionen (under förhandsgranskning) kan du toorun en Transact-SQL-fråga som sträcker sig över flera databaser i Azure SQL Database. Kan du tooperform mellan databasfrågor tooaccess fjärrtabeller och tooconnect från Microsoft och tredje parter verktyg (Excel, PowerBI, Tableau, etc.) tooquery över datanivåer med flera databaser. Med den här funktionen kan du skala upp frågor toolarge datanivåer i SQL-databas och visualisera hello resulterar i rapporter för business intelligence (BI).


## <a name="why-use-elastic-queries"></a>Varför använda elastisk frågor?

**Azure SQL Database**

Fråga på Azure SQL-databaser helt i T-SQL. Detta ger skrivskyddad frågor till fjärranslutna databaser. Detta ger ett alternativ för aktuell lokal SQL Server-kunder toomigrate program som använder tre och fyra deltid namn eller länkad server tooSQL DB.

**Tillgängligt på standardnivån**

Elastisk frågan stöds på hello Standard prestandanivån i tillägg toohello premiumnivån prestanda. Avsnittet hello på begränsningar i förhandsversionen nedan på prestanda begränsningar för lägre prestandanivåer.

**Push tooremote databaser**

Elastisk frågor kan nu push SQL parametrar toohello fjärr-databaser för körning.

**Lagrade proceduren körning**

Köra lagrade RPC-anrop eller remote funktioner med hjälp av [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714).

**Flexibilitet**

Externa tabeller med elastisk fråga kan nu referera tooremote tabeller med ett annat schema eller tabellnamn.

## <a name="elastic-query-scenarios"></a>Elastisk frågan scenarier

hello målet är toofacilitate frågar scenarier där rader bidrar till ett övergripande resultat i flera databaser. hello fråga kan antingen bestå av hello användare eller program direkt eller indirekt via verktyg som är anslutna toohello databas. Detta är särskilt användbar när du skapar rapporter med hjälp av kommersiella verktyg för integrering av BI eller data, eller alla program som inte kan ändras. Du kan fråga över flera databaser med hello välbekanta SQL Server connectivity upplevelse i verktyg som Excel, PowerBI, Tableau eller Cognos med en elastisk fråga.
En elastisk fråga kan lätt att komma åt tooan hela uppsättningen av databaser via frågor som utfärdats av SQL Server Management Studio eller Visual Studio och underlättar flera databaser frågar från Entity Framework eller andra ORM-miljöer. Bild 1 visar ett scenario där en befintlig molnet program (som använder hello [klientbibliotek för elastisk databas](sql-database-elastic-database-client-library.md)) nivån bygger på en uppskalad data och en elastisk fråga används för rapportering av flera databaser.

**Bild 1** elastisk fråga som används på utskalat datanivå

![Elastisk fråga som används på utskalat datanivå][1]

Kundscenarier för elastiska frågan kännetecknas av hello följande topologier:

* **Vertikal partitionering - mellan databasfrågor** (topologi 1): hello data är partitionerad lodrätt mellan ett antal databaser i en datanivå. Vanligtvis finns olika uppsättningar med tabeller på olika databaser. Det innebär att hello schema skiljer sig på olika databaser. Till exempel finns alla tabeller för lager på en databas när alla redovisning-relaterade tabeller är på en andra databas. Vanliga användningsområden med den här topologin kräver en tooquery över eller toocompile rapporter över tabeller i flera databaser.
* **Horisontell partitionering - horisontell partitionering** (topologi 2): Data är partitionerad vågrätt toodistribute rader i en skaländras ut data tjänstnivån. Hello-schemat är identiska för alla deltagande databaser med den här metoden. Den här metoden kallas även ”delning”. Horisontell partitionering kan utföras och hanteras med (1) hello elastisk databas verktyg bibliotek eller (2) self-delning. En elastisk frågan är tooquery eller kompilera rapporter över många delar.

> [!NOTE]
> Elastisk frågan fungerar bäst för tillfällig reporting scenarier där de flesta av hello bearbetning kan utföras på hello datanivå. För tunga reporting arbetsbelastningar eller informationslager-scenarier med mer komplexa frågor kan också överväga att använda [Azure SQL Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/).
>  

## <a name="vertical-partitioning---cross-database-queries"></a>Vertikal partitionering - frågor med flera databaser

toobegin kodning, se [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).

En elastisk fråga kan vara används toomake data i en SQL-databas tillgänglig tooother SQL-databaser. På så sätt kan frågor från en databas toorefer tootables i alla andra fjärranslutna SQL-databasen. hello första steget är toodefine en extern datakälla för varje fjärrdatabasen. hello extern datakälla har definierats i hello lokal databas som du vill toogain åtkomst tootables finns på hello fjärrdatabasen. Det krävs inga ändringar på hello fjärrdatabasen. Vanliga vertikal partitionering scenarier där olika databaser har olika scheman, kan elastisk frågor använda tooimplement vanliga användningsområden, till exempel komma åt tooreference data och frågar flera databaser.

> [!IMPORTANT]
> Du måste ha behörigheten ALTER ANY extern DATAKÄLLA. Den här behörigheten ingår i hello ALTER DATABASE-behörighet. ALTER ANY extern DATAKÄLLA behörigheter är nödvändiga toorefer toohello underliggande datakällan.
>

**Referensdata**: hello topologi används för hantering av referens. Två tabeller (T1 och T2) med referensdata hålls hello figuren nedan visas på en dedikerad databas. Med en elastisk fråga, du kan nu komma åt tabeller T1 och T2 via fjärranslutning från andra databaser som visas i hello bild. Använd topologi 1 om referenstabellerna är liten eller remote frågor i referenstabellen har selektiv predikat.

**Bild 2** vertikal partitionering – med elastisk frågan tooquery referensdata

![Vertikal partitionering – med elastisk frågan tooquery referensdata][3]

**Flera databaser frågar**: elastiska frågar aktivera användningsområden som kräver fråga över flera SQL-databaser. Bild 3 visar fyra olika databaser: CRM, inventering, HR och produkter. Frågor som utförs i en av databaserna hello också behöver komma åt tooone eller alla hello andra databaser. Du kan med en elastisk fråga för att konfigurera databasen för det här fallet genom att köra några enkla DDL-instruktionerna på varje hello fyra databaser. Efter den här engångskonfiguration är åtkomst tooa fjärrtabell lika enkelt som hänvisar tooa lokal tabell från T-SQL-frågor eller BI-verktyg. Den här metoden rekommenderas om hello fjärrfrågor inte returnerar stora resultat.

**Bild 3** vertikal partitionering – med hjälp av elastisk frågan tooquery mellan olika databaser

![Vertikal partitionering – med hjälp av elastisk frågan tooquery mellan olika databaser][4]

hello följande konfigurera elastisk databasfrågor för vertikal partitionering scenarier som kräver åtkomst tooa tabellen finns på fjärranslutna SQL-databaser med hello samma schema:

* [Skapa HUVUDNYCKEL](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* [Skapa och släpp extern DATAKÄLLA](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource av typen **RDBMS**
* [Skapa och släpp extern tabell](https://msdn.microsoft.com/library/dn935021.aspx) mytable prefix

Du kan komma åt hello fjärrtabell ”mytable” som prefix som om det vore en lokal tabell när du har kört hello DDL-instruktioner. Azure SQL Database automatiskt öppnar en anslutning toohello fjärrdatabas, bearbetar din förfrågan på hello fjärrdatabasen och returnerar hello resultat.

## <a name="horizontal-partitioning---sharding"></a>Horisontell partitionering - delning
Elastisk frågan tooperform rapporteringsuppgifter över ett delat, d.v.s., vågrätt partitionerad datanivå kräver en [elastisk databas Fragmentera kartan](sql-database-elastic-scale-shard-map-management.md) toorepresent hello databaser hello datanivå. Normalt bara en enda Fragmentera karta används i det här scenariot och en särskild databas med elastisk frågefunktioner (huvudnod) fungerar som hello startpunkten för rapportering frågor. Endast den här databasen måste åtkomst toohello Fragmentera kartan. Bild 4 illustrerar den här topologin och dess konfiguration med hello elastisk fråga databasen och Fragmentera karta. hello databaser i hello datanivå kan vara av en Azure SQL Database version eller utgåva. Läs mer om hello klientbibliotek för elastisk databas och skapa Fragmentera maps [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md).

**Bild 4** vågräta partitionering – med elastisk fråga för rapportering över shardade data kärnnivåerna

![Vågrät partitionering – med elastisk fråga för rapportering över shardade data kärnnivåerna][5]

> [!NOTE]
> Elastisk fråga databas (huvudnod) kan vara separat databas eller så kan vara hello samma databas som värdar hello Fragmentera mappar. Den konfiguration som du väljer, kontrollera att tjänsten och prestanda nivån databasen är tillräckligt högt toohandle hello förväntades mängden inloggning/frågebegäranden.

hello konfigurera följande elastisk databasfrågor för horisontell partitionering scenarier som kräver åtkomst tooa uppsättning tabell som finns på (vanligtvis) flera fjärranslutna SQL-databaser:

* [Skapa HUVUDNYCKEL](https://msdn.microsoft.com/library/ms174382.aspx) mymasterkey
* [CREATE DATABASE SCOPED CREDENTIAL](https://msdn.microsoft.com/library/mt270260.aspx) mycredential
* Skapa en [Fragmentera kartan](sql-database-elastic-scale-shard-map-management.md) som representerar din datanivån med hello klientbibliotek för elastisk databas.   
* [Skapa och släpp extern DATAKÄLLA](https://msdn.microsoft.com/library/dn935022.aspx) mydatasource av typen **SHARD_MAP_MANAGER**
* [Skapa och släpp extern tabell](https://msdn.microsoft.com/library/dn935021.aspx) mytable prefix

När du har utfört dessa steg kan du komma åt hello vågrätt partitionerad tabell ”mytable” som prefix som om det vore en lokal tabell. Azure SQL Database automatiskt öppnar flera parallella anslutningar toohello remote databaser där hello tabeller lagras fysiskt, bearbetar hello begäranden på hello remote databaser och returnerar hello resultat.
Mer information om hello steg som krävs för hello horisontell partitionering scenario kan hittas i [elastisk frågan för horisontell partitionering](sql-database-elastic-query-horizontal-partitioning.md).

toobegin kodning, se [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).

## <a name="t-sql-querying"></a>T-SQL-fråga
När du har definierat din externa datakällor och externa tabeller kan använda du reguljära SQL Server-anslutning strängar tooconnect toohello databaser där du har definierat externa tabeller. Du kan sedan köra T-SQL-uttryck via externa tabeller för anslutningen med hello begränsningar som beskrivs nedan. Du hittar mer information och exempel för T-SQL-frågor i hello dokumentationen avsnitt [horisontell partitionering](sql-database-elastic-query-horizontal-partitioning.md) och [vertikal partitionering](sql-database-elastic-query-vertical-partitioning.md).

## <a name="connectivity-for-tools"></a>Anslutning för verktyg
Du kan använda reguljära strängar tooconnect för SQL Server-anslutning, program och BI eller data integration toodatabases för verktyg som har externa tabeller. Kontrollera att SQL Server stöds som en datakälla för verktyget du behöver. När du är ansluten, se toohello elastisk fråga databasen och hello externa tabeller i databasen precis som du gör med andra SQL Server-databasen du ansluter toowith verktyget du behöver.

> [!IMPORTANT]
> Autentisering med hjälp av Azure Active Directory med elastisk frågor stöds inte för närvarande.
> 
> 

## <a name="cost"></a>Kostnad
Elastisk frågan ingår i hello kostnaden för Azure SQL Database-databaser. Observera att där remote databaserna är i ett annat Datacenter än hello elastisk frågan endpoint-topologier stöds, men datatrafik från fjärranslutna databaser debiteras regular [Azure priser](https://azure.microsoft.com/pricing/details/data-transfers/).

## <a name="preview-limitations"></a>Begränsningar i förhandsversionen
* Kör ditt första elastisk frågan kan ta upp tooa några minuter på hello Standard prestandanivå. Nu är nödvändiga tooload hello elastisk frågefunktioner; läser in prestanda förbättras med högre prestandanivåer.
* Skript för externa datakällor eller externa tabeller från SSMS eller SSDT stöds inte ännu.
* Import/Export för SQL DB stöder ännu inte externa datakällor och externa tabeller. Om du behöver toouse Import/Export kan ta bort dessa objekt innan du exporterar och sedan återskapa dem när du har importerat.
* Elastisk fråga stöder för närvarande bara läsbehörighet tooexternal tabeller. Du kan dock använda alla T-SQL-funktioner på hello databasen där hello extern tabell definieras. Detta kan vara praktiskt att t.ex. spara tillfälliga resultat, t.ex. Markera med < column_list > till < local_table > eller toodefine lagrade procedurer på hello elastisk fråga databas används tooexternal tabeller.
* Förutom nvarchar(max) stöds inte LOB-typerna i extern tabelldefinitioner. Som en tillfällig lösning kan du skapa en vy på hello fjärrdatabasen som kastar hello LOB-typ i nvarchar(max), definiera din extern tabell över hello vyn i stället för hello bastabellen och konvertera den tillbaka till hello ursprungliga LOB-typen i dina frågor.
* Kolumnstatistik via externa tabeller stöds inte för närvarande. Tabeller statistik stöds, men måste toobe som skapats manuellt.

## <a name="feedback"></a>Feedback
Kontrollera dela feedback om din upplevelse med elastisk frågor med oss på Disqus nedan hello MSDN-forum eller Stackoverflow. Vi är intresserade av alla typer av feedback om hello service (fel, grov kanter, funktionen luckor).

## <a name="next-steps"></a>Nästa steg

* Vertikal partitionering, finns [komma igång med flera databaser fråga (vertikal partitionering)](sql-database-elastic-query-getting-started-vertical.md).
* Syntax och exempel frågor för lodrätt partitionerade finns [frågar lodrätt partitionerad data)](sql-database-elastic-query-vertical-partitioning.md)
* Horisontell partitionering (delning), finns [komma igång med elastisk frågan för horisontell partitionering (delning)](sql-database-elastic-query-getting-started.md).
* Syntax och exempel frågor för vågrätt partitionerade finns [frågar vågrätt partitionerad data)](sql-database-elastic-query-horizontal-partitioning.md)
* Se [sp\_köra \_remote](https://msdn.microsoft.com/library/mt703714) för en lagrad procedur som kör en Transact-SQL-instruktion på en enda remote Azure SQL Database eller en uppsättning databaser som fungerar som delar i en vågrät partitioneringsschema.

<!--Image references-->
[1]: ./media/sql-database-elastic-query-overview/overview.png
[2]: ./media/sql-database-elastic-query-overview/topology1.png
[3]: ./media/sql-database-elastic-query-overview/vertpartrrefdata.png
[4]: ./media/sql-database-elastic-query-overview/verticalpartitioning.png
[5]: ./media/sql-database-elastic-query-overview/horizontalpartitioning.png

<!--anchors-->
