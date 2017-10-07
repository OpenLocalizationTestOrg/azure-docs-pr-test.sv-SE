---
title: aaaHow toouse batchbearbetning tooimprove Azure SQL Database prestanda
description: "hello avsnittet ger bevis att batchbearbetning databasen operations avsevärt imroves hello hastighet och skalbarheten för dina Azure SQL Database-program. Även om dessa batching metoder fungera för SQL Server-databas, fokuserar hello hello artikeln på på Azure."
services: sql-database
documentationcenter: na
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 563862ca-c65a-46f6-975d-10df7ff6aa9c
ms.service: sql-database
ms.custom: develop apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: data-management
ms.date: 07/12/2016
ms.author: sstein
ms.openlocfilehash: 124b203ee69c595f0813852ff09ef9ec6841233a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toouse-batching-tooimprove-sql-database-application-performance"></a>Hur toouse batchbearbetning tooimprove programprestanda för SQL-databas
Batchbearbetning operations tooAzure SQL-databas avsevärt förbättrar hello prestanda och skalbarhet. I ordning toounderstand hello fördelar innehåller hello första delen av den här artikeln några exempel på testresultat som jämför sekventiella och gruppbaserad begäranden tooa SQL-databas. hello resten av hello artikeln visar hello tekniker, scenarier och överväganden toohelp toouse batchbearbetning har i din Azure-program.

## <a name="why-is-batching-important-for-sql-database"></a>Varför är batchbearbetning viktigt för SQL-databas?
Batchbearbetning anrop tooa fjärrtjänsten är en välkänd strategi för att öka prestanda och skalbarhet. Det fasta är bearbetning kostnader tooany interaktioner med en fjärransluten tjänst, till exempel serialisering och nätverksöverföring deserialisering. Paketera många separata transaktioner i en enskild batch minimerar dessa kostnader.

I det här dokumentet vill vi tooexamine olika scenarier och batchbearbetning strategier för SQL-databas. Men strategierna är också viktigt för lokala program som använder SQL Server, finns det flera orsaker till syntaxmarkering hello användning av batchbearbetning för SQL-databas:

* Det finns potentiellt större Nätverksfördröjningen för åtkomst till SQL-databas, särskilt om du ansluter till SQL-databas från utanför hello samma Microsoft Azure-datacenter.
* hello flera egenskaper för SQL-databasen innebär att hello effektiviteten för hello data åt lagret korrelerar toohello övergripande skalbarhet av hello-databasen. SQL-databas måste förhindra att en enskild klient/användare alltid använder databasen resurser toohello skada för andra klienter. I svaret toousage överskrider fördefinierade kvoter, SQL Database minska eller svara med begränsning undantag. Effektivitet, till exempel batchbearbetning, aktiverar du toodo mer arbete i SQL-databasen innan det nådde dessa begränsningar. 
* Batchbearbetning är också gälla för konfigurationer med flera databaser (delning). hello effektiviteten för interaktionen med varje enhet i databasen är fortfarande en avgörande faktor för din övergripande skalbarhet. 

En av hello fördelarna med att använda SQL-databas är att du inte har toomanage hello servrar att värden hello-databasen. Hanterade infrastrukturen innebär dock också att du har toothink annorlunda om databasoptimering. Du kan inte längre se tooimprove hello datormaskinvaran eller nätverksanslutningen databasinfrastruktur. Microsoft Azure styr dessa miljöer. hello huvudsakliga område som du kan styra är hur programmet samverkar med SQL-databas. Batchbearbetning är en av dessa optimeringar. 

hello första delen av hello papper undersöker olika batching tekniker för .NET-program som använder SQL-databas. hello sista två avsnitt beskriver batchbearbetning riktlinjer och scenarier.

## <a name="batching-strategies"></a>Batchbearbetning strategier
### <a name="note-about-timing-results-in-this-topic"></a>Observera om tidsinställning resultat i det här avsnittet
> [!NOTE]
> Resultatet är inte prestandamått men är avsedda tooshow **relativa prestandan**. Tidsinställningar baseras på ett genomsnitt av minst 10 testkörningar. Åtgärder som infogar i en tom tabell. Dessa tester har uppmätta pre-V12 och de inte nödvändigtvis toothroughput som kan uppstå i en V12-databas med hjälp av hello ny [tjänstnivåer](sql-database-service-tiers.md). hello relativa fördelen hello batchbearbetning tekniken bör vara densamma.
> 
> 

### <a name="transactions"></a>Transaktioner
Det verkar konstigt toobegin en granskning av batchbearbetning av diskutera transaktioner. Men hello användning av transaktioner på klientsidan har en diskret serversidan batching effekt som förbättrar prestanda. Och transaktioner kan läggas till med bara några få rader med kod, så att de får ett snabbt sätt tooimprove prestanda för sekventiella operationer.

Överväg att hello följande C#-kod som innehåller en sekvens med insert och uppdateringsåtgärder på en enkel tabell.

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

hello följande ADO.NET kod sekventiellt utför dessa åtgärder.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

Hej bästa sätt toooptimize koden är tooimplement någon form av klientsidan massbearbetning av dessa anrop. Men det finns ett enkelt sätt tooincrease hello prestanda för den här koden genom att helt enkelt omsluta hello sekvens med anrop i en transaktion. Här är hello samma kod som använder en transaktion.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();
        SqlTransaction transaction = conn.BeginTransaction();

        foreach (string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn, transaction);
            cmd.ExecuteNonQuery();
        }

        transaction.Commit();
    }

Transaktioner används faktiskt i båda de här exemplen. Varje enskilda anropet är en implicit transaktion i hello första exemplet. I andra exemplet hello radbryts en explicit transaktion alla hello-anrop. Per hello dokumentationen för hello [write-ahead transaktionsloggen](https://msdn.microsoft.com/library/ms186259.aspx), loggposter är en toohello disk när hello transaktion har genomförts. Så genom att lägga till fler anrop i en transaktion kan hello skrivåtgärder toohello transaktionsloggen fördröja tills genomförs hello transaktionen. Du aktiverar i praktiken batchbearbetning för hello skrivningar toohello serverns transaktionsloggen.

hello följande tabell visar några ad hoc-testresultaten. hello tester hello samma sekventiella infogar med och utan transaktioner. Mer perspektiv kördes hello första uppsättningen tester för fjärråtkomst från en bärbar dator toohello databas i Microsoft Azure. hello första uppsättningen tester körde från en Molntjänsten och databasen att båda finns inom hello samma Microsoft Azure-datacenter (USA, västra). hello följande tabell visar hello tid i millisekunder för sekventiella infogningar med och utan transaktioner.

**Lokala tooAzure**:

| Åtgärder | Ingen transaktion (ms) | Transaktion (ms) |
| --- | --- | --- |
| 1 |130 |402 |
| 10 |1208 |1226 |
| 100 |12662 |10395 |
| 1000 |128852 |102917 |

**Azure tooAzure (samma datacenter)**:

| Åtgärder | Ingen transaktion (ms) | Transaktion (ms) |
| --- | --- | --- |
| 1 |21 |26 |
| 10 |220 |56 |
| 100 |2145 |341 |
| 1000 |21479 |2756 |

> [!NOTE]
> Resultatet är inte prestandamått. Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).
> 
> 

Baserat på hello tidigare testresultaten, minskar byta en enda åtgärd i en transaktion faktiskt prestanda. Men om du ökar hello antal åtgärder i en enda transaktion hello prestandaförbättring blir större. Hej prestandaskillnad är också mer märkbart när alla åtgärder inom hello Microsoft Azure-datacenter. hello överskuggar ökad latens med SQL-databas från utanför hello Microsoft Azure-datacenter hello prestandafördelar med transaktioner.

Även om hello användning av transaktioner kan öka prestanda kan fortsätta för[Se metodtips för transaktioner och anslutningar](https://msdn.microsoft.com/library/ms187484.aspx). Behåll hello transaktioner så kort som möjligt och Stäng hello databasanslutning när hello arbete har slutförts. hello med instruktionen i hello föregående exempel säkerställer att hello anslutningen är stängd när hello efterföljande kodblock har slutförts.

hello föregående exempel visar att du kan lägga till en lokal transaktion tooany ADO.NET kod med två rader. Transaktioner erbjuder ett snabbt sätt tooimprove hello prestanda av kod som gör sekventiella infoga, uppdatera och ta bort. Men för hello bästa prestanda, Överväg att ändra hello kod ytterligare tootake nytta av klientsidan batchbearbetning, till exempel tabellvärdeparametrar.

Mer information om transaktioner i ADO.NET finns [lokala transaktioner i ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).

### <a name="table-valued-parameters"></a>tabellvärdeparametrar
Tabellvärdeparametrar stöd för användardefinierade tabelltyper som parametrar i Transact-SQL-uttryck, lagrade procedurer och funktioner. Denna batching teknik för klientsidan kan du toosend flera rader med data i hello tabellvärdesparametern. toouse tabellvärdeparametrar, först definiera en tabelltyp. hello följande Transact-SQL-instruktion skapas en tabelltyp som heter **MyTableType**.

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


I koden, skapar du en **DataTable** med hello exakt samma namn och typer för hello tabelltyp. Skicka detta **DataTable** i en parameter i en textfråga eller en lagrad procedur anropa. hello visas följande exempel den här tekniken:

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. hello following is a simple example.
        table.Columns.Add("mytext", typeof(string));
        table.Columns.Add("num", typeof(int));    
        for (var i = 0; i < 10; i++)
        {
            table.Rows.Add(DateTime.Now.ToString(), DateTime.Now.Millisecond);
        }

        SqlCommand cmd = new SqlCommand(
            "INSERT INTO MyTable(mytext, num) SELECT mytext, num FROM @TestTvp",
            connection);

        cmd.Parameters.Add(
            new SqlParameter()
            {
                ParameterName = "@TestTvp",
                SqlDbType = SqlDbType.Structured,
                TypeName = "MyTableType",
                Value = table,
            });

        cmd.ExecuteNonQuery();
    }

I föregående exempel hello hello **SqlCommand** objekt infogar rader från en tabellvärdesparameter  **@TestTvp** . hello skapat **DataTable** objekt tilldelas toothis parameter med hello **SqlCommand.Parameters.Add** metod. Batchbearbetning hello infogningar i ett anropa avsevärt ökar hello prestanda över sekventiella infogningar.

tooimprove hello föregående exempel dessutom använda en lagrad procedur i stället för ett textbaserat kommando. hello följande Transact-SQL-kommando skapar en lagrad procedur som tar hello **SimpleTestTableType** tabellvärdesparametern.

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

Ändra hello **SqlCommand** objekt deklarationen i hello föregående kod exemplet toohello nedan.

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

I de flesta fall har tabellvärdeparametrar motsvarande eller bättre prestanda än andra batching metoder. Tabellvärdeparametrar är ofta bättre, eftersom de är mer flexibelt än andra alternativ. Till exempel tillåter andra tekniker, till exempel SQL-masskopiering, bara hello nya rader infogas. Men med tabellvärdeparametrar, kan du använda logik i hello lagrade proceduren toodetermine vilka rader som uppdateras och som kan infogas. hello tabelltyp kan också vara ändrade toocontain en ”åtgärden”-kolumn som anger om hello angivna raden ska infogas, uppdateras eller tas bort.

hello följande tabell visar ad hoc-testresultaten för hello använda tabellvärdeparametrar i millisekunder.

| Åtgärder | Lokala tooAzure (ms) | Samma Azure-datacenter (ms) |
| --- | --- | --- |
| 1 |124 |32 |
| 10 |131 |25 |
| 100 |338 |51 |
| 1000 |2615 |382 |
| 10000 |23830 |3586 |

> [!NOTE]
> Resultatet är inte prestandamått. Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).
> 
> 

hello prestandafördelar från batchbearbetning är uppenbar. I hello föregående sekventiella testet tog 1000 åtgärderna 129 sekunder utanför hello datacenter och 21 sekunder från inom hello datacenter. Men med tabellvärdeparametrar, 1000 operations Ta endast 2.6 sekunder utanför hello datacenter och 0,4 sekunder inom hello datacenter.

Mer information om tabellvärdeparametrar finns [Table-Valued parametrar](https://msdn.microsoft.com/library/bb510489.aspx).

### <a name="sql-bulk-copy"></a>SQL-masskopiering
SQL-masskopiering är ett annat sätt tooinsert stora mängder data till en måldatabas. .NET-program kan använda hello **SqlBulkCopy körs** klassen tooperform massinfogning åtgärder. **SqlBulkCopy körs** liknar funktionen toohello kommandoradsverktyget **Bcp.exe**, eller hello Transact-SQL-instruktionen **BULK INSERT**. hello följande kodexempel visar hur toobulk kopiera hello rader i hello källan **DataTable**, tabell, toohello måltabellen i SQL Server, mytable prefix.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        using (SqlBulkCopy bulkCopy = new SqlBulkCopy(connection))
        {
            bulkCopy.DestinationTableName = "MyTable";
            bulkCopy.ColumnMappings.Add("mytext", "mytext");
            bulkCopy.ColumnMappings.Add("num", "num");
            bulkCopy.WriteToServer(table);
        }
    }

Det finns tillfällen där masskopiering är att föredra över tabellvärdeparametrar. Se hello jämförelsetabell för tabellvärdeparametrar jämfört med BULK INSERT åtgärder i hello avsnittet [Table-Valued parametrar](https://msdn.microsoft.com/library/bb510489.aspx).

hello följande ad hoc-test visar hello prestanda för batchbearbetning med **SqlBulkCopy körs** i millisekunder.

| Åtgärder | Lokala tooAzure (ms) | Samma Azure-datacenter (ms) |
| --- | --- | --- |
| 1 |433 |57 |
| 10 |441 |32 |
| 100 |636 |53 |
| 1000 |2535 |341 |
| 10000 |21605 |2737 |

> [!NOTE]
> Resultatet är inte prestandamått. Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).
> 
> 

I mindre batch storlekar hello använda tabellvärdeparametrar gick bättre än förväntat hello **SqlBulkCopy körs** klass. Dock **SqlBulkCopy körs** utförs 12-31% snabbare än tabellvärdeparametrar för hello test 1 000 och 10 000 rader. Som tabellvärdeparametrar, **SqlBulkCopy körs** är ett bra alternativ för gruppbaserad infogningar särskilt jämfört toohello prestanda för ett annat åtgärder.

Mer information om masskopiering i ADO.NET finns [Masskopieringsåtgärder i SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).

### <a name="multiple-row-parameterized-insert-statements"></a>Flera rader som innehåller parametrar Infoga instruktioner
Ett alternativ för små batchar är en stor tooconstruct parametriserade INSERT-instruktionen som infogar flera rader. hello följande kodexempel visar den här tekniken.

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        string insertCommand = "INSERT INTO [MyTable] ( mytext, num ) " +
            "VALUES (@p1, @p2), (@p3, @p4), (@p5, @p6), (@p7, @p8), (@p9, @p10)";

        SqlCommand cmd = new SqlCommand(insertCommand, connection);

        for (int i = 1; i <= 10; i += 2)
        {
            cmd.Parameters.Add(new SqlParameter("@p" + i.ToString(), "test"));
            cmd.Parameters.Add(new SqlParameter("@p" + (i+1).ToString(), i));
        }

        cmd.ExecuteNonQuery();
    }


Det här exemplet är avsedd tooshow hello grundläggande begrepp. En mer realistisk scenariot skulle gå igenom hello krävs entiteter tooconstruct hello frågesträngen och hello kommandoparametrarna samtidigt. Är du begränsad tooa totalt 2100 parametrar, så detta begränsar hello Totalt antal rader som kan bearbetas i det här sättet.

hello följande ad hoc-Testa resultaten visar hello prestanda för den här typen av insert-instruktionen i millisekunder.

| Åtgärder | Tabellvärdeparametrar (ms) | Infoga enstaka uttryck (ms) |
| --- | --- | --- |
| 1 |32 |20 |
| 10 |30 |25 |
| 100 |33 |51 |

> [!NOTE]
> Resultatet är inte prestandamått. Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).
> 
> 

Den här metoden kan vara något snabbare för batchar som är mindre än 100 rader. Även om hello improvement är liten är tekniken ett annat alternativ som fungerar bra i ditt specifika program scenario.

### <a name="dataadapter"></a>DataAdapter
Hej **DataAdapter** klassen kan du toomodify en **DataSet** objektet och sedan skicka hello ändringar som INSERT, UPDATE och DELETE-åtgärder. Om du använder hello **DataAdapter** i det här sättet är det viktigt toonote som avgränsar anrop görs för varje distinkta åtgärd. tooimprove prestanda, Använd hello **UpdateBatchSize** egenskapen toohello antal åtgärder som ska grupperas i taget. Mer information finns i [utför Batch åtgärder med hjälp av DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).

### <a name="entity-framework"></a>Entity framework
Entity Framework stöder för närvarande inte batchbearbetning. Olika utvecklare i hello community har försökt toodemonstrate lösningar, till exempel åsidosättning hello **SaveChanges** metod. Men hello lösningar är vanligtvis toohello komplexa och anpassade program och datamodellen. hello Entity Framework codeplex projekt har för närvarande en diskussionssida på den här funktionsbegäran. tooview den här diskussionen finns [Design mötesanteckningar - 2 augusti 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).

### <a name="xml"></a>XML
För fullständighetens skull bör du att det är viktigt tootalk om XML-Datatypen som en batching strategi. Hello användning av XML-har dock inga fördelar över andra metoder och flera nackdelar. hello-metoden är liknande tootable enkelvärdesattribut parametrar, men ett XML-fil eller sträng har överförts tooa lagrade procedur i stället för en användardefinierad tabell. hello lagrade proceduren Parsar hello kommandon i hello lagrad procedur.

Det finns flera nackdelar toothis metod:

* Arbeta med XML kan vara krånglig och tillförlitligt.
* Tolkning hello XML för hello databasen kan vara Processorn märkbart.
* I de flesta fall är den här metoden går långsammare än om tabellvärdeparametrar.

Därför rekommenderas inte hello användning av XML för batch-frågor.

## <a name="batching-considerations"></a>Batchbearbetning överväganden
hello följande avsnitt ge mer vägledning för hello användning av batchbearbetning i SQL Database-program.

### <a name="tradeoffs"></a>Nackdelar
Beroende på din arkitektur involverar batchbearbetning en kompromiss mellan prestanda och återhämtning. Anta exempelvis att hello scenario där din roll oväntat stängs av. Om du tappar bort en rad med data är hello påverkan mindre än hello effekten av att förlora en stor grupp med rader som inte har skickats. Det finns en större risk när du buffert rader innan de skickas toohello databasen i ett angivet tidsintervall.

På grund av det här förhållandet utvärdera hello typ av åtgärder som du batch. Batch mer aggressivt (större batchar och längre tidsfönster) med data som är mindre viktigt.

### <a name="batch-size"></a>Batchstorlek
I våra tester fanns det vanligtvis ingen fördel toobreaking stora batchar i mindre delar. Faktum är resulterade ofta den här delfältet i långsammare än att skicka en enda stor grupp. Tänk dig ett scenario där du vill att tooinsert 1000 översta raderna. hello följande tabell visar hur lång tid det tar toouse tabellvärdeparametrar tooinsert 1000 rader när indelat i mindre batchar.

| Batchstorlek | Upprepningar | Tabellvärdeparametrar (ms) |
| --- | --- | --- |
| 1000 |1 |347 |
| 500 |2 |355 |
| 100 |10 |465 |
| 50 |20 |630 |

> [!NOTE]
> Resultatet är inte prestandamått. Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).
> 
> 

Du kan se att hello bästa prestanda för 1000 översta raderna är toosubmit alla samtidigt. I andra tester (visas inte här) uppstod en liten prestanda vinst toobreak en 10000 raden batch i två grupper om 5000. Men hello tabellschemat för dessa tester är relativt enkla, så du bör utföra tester på specifika data och batch storlekar tooverify dessa resultat.

En annan faktor tooconsider är att om hello totala batch blir för stor, SQL-databas kan begränsa refusera toocommit hello batch. Testa din situation toodetermine hello resultatet blir bäst om det finns en perfekt batchstorlek. Gör hello batchstorlek konfigureras vid körning tooenable snabba justeringar utifrån prestanda eller fel.

Slutligen balansera hello storleken på hello batch med hello risker associerade med batchbearbetning. Om det finns tillfälliga fel eller hello rollen misslyckas, Överväg hello konsekvenserna av du försöker hello åtgärden eller hello dataförlust i hello batch.

### <a name="parallel-processing"></a>Parallell bearbetning
Vad händer om du tog hello-metoden för att minska hello batchstorlek men används för flera trådar tooexecute hello arbete? Våra tester visade igen att flera mindre flertrådade batchar vanligtvis utförs värre än en enskild större batch. hello försöker följande test tooinsert 1000 rader i en eller flera parallella batchar. Det här testet visar hur flera samtidiga batchar faktiskt minskade prestanda.

| Batchstorlek [iterationer] | Två trådar (ms) | Fyra trådar (ms) | Sex trådar (ms) |
| --- | --- | --- | --- |
| 1000 [1] |277 |315 |266 |
| 500 [2] |548 |278 |256 |
| 250 [4] |405 |329 |265 |
| 100 [10] |488 |439 |391 |

> [!NOTE]
> Resultatet är inte prestandamått. Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).
> 
> 

Det finns flera möjliga orsaker till hello sämre prestanda på grund av tooparallelism:

* Det finns flera samtidiga nätverket anrop i stället för en.
* Flera åtgärder mot en tabell kan resultera i konkurrens och blockerar.
* Det finns kostnader som flertrådsteknik.
* hello kostnaden för att öppna flera anslutningar uppväger hello fördelen parallell bearbetning.

Om du anpassar olika tabeller eller databaser, är det möjligt toosee vissa prestandaförbättring med den här strategin. Horisontell partitionering databasen eller federationer är ett scenario för den här metoden. Horisontell partitionering använder flera databaser och vägar olika tooeach databas. Om varje liten grupp ska tooa annan databas, kan utför sedan hello åtgärder parallellt vara mer effektivt. Hello prestandafördelar är dock inte tillräckligt betydande toouse hello utgångspunkt för beslut toouse databasen delning i din lösning.

I vissa Designer leda parallell körning av mindre batchar till bättre genomflöde begäranden i ett system under belastning. I det här fallet trots att det är snabbare tooprocess en enskild större batch kan bearbeta flera batchar parallellt vara mer effektivt.

Om du använder parallell körning kan du styra hello maximalt antal arbetstrådar. Ett mindre antal leda till mindre konkurrens och snabbare körningstid. Du kan också hello belastning som placeras på hello måldatabasen både anslutningar och transaktioner.

### <a name="related-performance-factors"></a>Prestandadata relaterade faktorer
Vanliga vägledning om databasprestanda påverkar också batchbearbetning. Till exempel infoga minskar prestanda för tabeller som har en primärnyckel för stora eller många icke-grupperat index.

Om tabellvärdeparametrar använder en lagrad procedur, kan du använda kommandot hello **SET NOCOUNT ON** hello början av hello proceduren. Den här instruktionen Undertrycker hello returnera hello antal hello påverkas rader i hello proceduren. Men i våra tester hello användning av **SET NOCOUNT ON** inte påverkar eller minskade prestanda. hello test lagrade proceduren har enkelt med en enda **infoga** från hello tabellvärdesparametern. Det är möjligt att mer komplexa lagrade procedurer skulle dra nytta av den här instruktionen. Men förutsätt inte att lägga till **SET NOCOUNT ON** tooyour lagrade proceduren automatiskt förbättrar prestanda. toounderstand hello effekt, testa den lagrade proceduren med och utan hello **SET NOCOUNT ON** instruktionen.

## <a name="batching-scenarios"></a>Batchbearbetning scenarier
hello följande avsnitt beskrivs hur toouse tabellvärdeparametrar i tre scenarier för programmet. hello första scenariot visar hur buffring och batchbearbetning kan fungera tillsammans. hello andra scenariot förbättrar prestanda genom att utföra åtgärder för master-detaljer i en enda lagrade proceduranropet. Hej sista scenariot visar hur toouse tabellvärdeparametrar i en ”UPSERT”-åtgärd.

### <a name="buffering"></a>Buffring
Men det är några scenarier som är uppenbara kandidat för batchbearbetning, finns det många scenarier som kan dra nytta av batchbearbetning av fördröjd bearbetning. Dock innebär också fördröjd bearbetning en större risk hello data förloras i hello händelse av ett oväntat fel. Det är viktigt toounderstand risken och Överväg hello konsekvenser.

Tänk dig ett webbprogram som spårar hello historik för varje användare. Vid varje sidbegäran gör hello programmet databasen anropet toorecord hello användarens sidvisningen. Men högre prestanda och skalbarhet kan uppnås genom buffring hello användarnas navigering aktiviteter och sedan skickar data toohello databasen i batchar. Du kan utlösa hello database-uppdateringen av förfluten tid och/eller buffertstorlek. En regel kan till exempel ange att hello batch ska bearbetas efter 20 sekunder eller när hello buffert når 1 000 objekt.

hello följande kodexempel används [reaktiv tillägg - Rx](https://msdn.microsoft.com/data/gg577609) tooprocess buffras händelser som skapats av en övervakningsklass. Hello när buffert fyllning eller en tidsgräns uppnås, hello batch användardata skickas toohello databasen med en tabellvärdesparameter.

hello följande NavHistoryData klassen modeller hello navigering användarinformation. Det innehåller grundläggande information, till exempel hello användar-ID, hello URL: en används och hello åtkomsttid.

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

Hej NavHistoryDataMonitor klass är ansvarig för buffring hello navigering data toohello användardatabas. Den innehåller en metod, RecordUserNavigationEntry som svarar genom att en **OnAdded** händelse. hello följande kod visar hello konstruktorn logik som använder Rx toocreate en synliga samling baserat på hello-händelse. Den prenumererar sedan toothis synliga samling med hello buffert metod. hello överlagring anger att hello bufferten ska skickas varje 20 sekunder eller 1000 poster.

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

hello handler konverterar alla hello buffras objekt till en tabellvärdesfunktion typ och skickar sedan den här typen tooa lagrade proceduren som processer hello batch. hello visar följande kod hello klar definition för både hello NavHistoryDataEventArgs och hello NavHistoryDataMonitor klasser.

    public class NavHistoryDataEventArgs : System.EventArgs
    {
        public NavHistoryDataEventArgs(NavHistoryData data) { Data = data; }
        public NavHistoryData Data { get; set; }
    }

    public class NavHistoryDataMonitor
    {
        public event EventHandler<NavHistoryDataEventArgs> OnAdded;

        public NavHistoryDataMonitor()
        {
            var observableData =
                Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

            observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
        }

        public void RecordUserNavigationEntry(NavHistoryData data)
        {    
            if (OnAdded != null)
                OnAdded(this, new NavHistoryDataEventArgs(data));
        }

        protected void Handler(IList<EventPattern<NavHistoryDataEventArgs>> items)
        {
            DataTable navHistoryBatch = new DataTable("NavigationHistoryBatch");
            navHistoryBatch.Columns.Add("UserId", typeof(int));
            navHistoryBatch.Columns.Add("URL", typeof(string));
            navHistoryBatch.Columns.Add("AccessTime", typeof(DateTime));
            foreach (EventPattern<NavHistoryDataEventArgs> item in items)
            {
                NavHistoryData data = item.EventArgs.Data;
                navHistoryBatch.Rows.Add(data.UserId, data.URL, data.AccessTime);
            }

            using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
            {
                connection.Open();

                SqlCommand cmd = new SqlCommand("sp_RecordUserNavigation", connection);
                cmd.CommandType = CommandType.StoredProcedure;

                cmd.Parameters.Add(
                    new SqlParameter()
                    {
                        ParameterName = "@NavHistoryBatch",
                        SqlDbType = SqlDbType.Structured,
                        TypeName = "NavigationHistoryTableType",
                        Value = navHistoryBatch,
                    });

                cmd.ExecuteNonQuery();
            }
        }
    }

toouse denna buffring klass hello program skapar en statisk NavHistoryDataMonitor-objekt. Varje gång en användare ansluter till en sida hello programmet anropar hello NavHistoryDataMonitor.RecordUserNavigationEntry metod. hello buffring logik fortsätter tootake vård skicka dessa poster toohello databasen i batchar.

### <a name="master-detail"></a>Master detaljer
Tabellvärdeparametrar är användbara för enkla INSERT-scenarier. Det kan dock vara mer utmanande toobatch infogningar som rör fler än en tabell. Hej ”översikt/detaljer” scenario är ett bra exempel. hello huvudtabellen identifierar hello primära enhet. En eller flera tabeller i detalj lagra mer data om hello entitet. I det här scenariot framtvinga sekundärnyckelrelationer hello relationen mellan information tooa unika master entitet. Överväg att en förenklad version av PurchaseOrder tabellerna och dess associerade OrderDetail. hello följande Transact-SQL skapar hello PurchaseOrder tabell med fyra kolumner: OrderID, OrderDate, CustomerID och Status.

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

Varje innehåller en eller flera produktinköp. Den här informationen samlas i hello PurchaseOrderDetail tabell. hello följande Transact-SQL skapar hello PurchaseOrderDetail tabell med fem kolumner: OrderID, OrderDetailID, ProductID, Enhetspris och OrderQty.

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

hello OrderID kolumn i hello PurchaseOrderDetail tabell måste referera till en order från hello PurchaseOrder tabell. hello definitionen av en sekundärnyckel tillämpar den här begränsningen.

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

I ordning toouse tabellvärdeparametrar, måste du ha en användardefinierad tabelltyp för varje måltabellen.

    CREATE TYPE PurchaseOrderTableType AS TABLE 
    ( OrderID INT,
      OrderDate DATETIME,
      CustomerID INT,
      Status NVARCHAR(50) );
    GO

    CREATE TYPE PurchaseOrderDetailTableType AS TABLE 
    ( OrderID INT,
      ProductID INT,
      UnitPrice MONEY,
      OrderQty SMALLINT );
    GO

Definiera en lagrad procedur som accepterar tabeller för dessa typer. Den här proceduren kan ett program toolocally batch en uppsättning order och orderinformationen i ett enda anrop. hello ger följande Transact-SQL hello fullständig lagrade procedursdeklaration för det här köpet ordning exemplet.

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects hello order identifiers in hello @orders
    -- table with hello actual order identifiers in hello PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders toohello PurchaseOrder table, storing hello actual
    -- order identifiers in hello @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match hello passed-in order identifiers with hello actual identifiers
    -- and complete hello @IdentityLink table for use with inserting hello details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert hello order details into hello PurchaseOrderDetail table, 
          -- using hello actual order identifiers of hello master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

I det här exemplet hello lokalt definierade @IdentityLink tabellen lagras hello faktiska OrderID värden från hello nyligen infogade rader. Dessa identifierare ordning skiljer sig från hello tillfälliga OrderID värden i hello @orders och @details tabellvärdeparametrar. Därför hello @IdentityLink tabell ansluter sedan hello OrderID värden från hello @orders toohello verkliga OrderID parametervärden för hello nya rader i hello PurchaseOrder tabell. Efter det här steget hello @IdentityLink tabellen kan underlätta Infoga hello orderinformationen med hello faktiska OrderID som uppfyller hello foreign key-begränsningen.

Den här lagrade proceduren kan användas från kod eller andra Transact-SQL-anrop. Avsnittet hello tabellvärdeparametrar i det här dokumentet som ett exempel. hello följande Transact-SQL visar hur toocall hello sp_InsertOrdersBatch.

    declare @orders as PurchaseOrderTableType
    declare @details as PurchaseOrderDetailTableType

    INSERT @orders 
    ([OrderID], [OrderDate], [CustomerID], [Status])
    VALUES(1, '1/1/2013', 1125, 'Complete'),
    (2, '1/13/2013', 348, 'Processing'),
    (3, '1/12/2013', 2504, 'Shipped')

    INSERT @details
    ([OrderID], [ProductID], [UnitPrice], [OrderQty])
    VALUES(1, 10, $11.50, 1),
    (1, 12, $1.58, 1),
    (2, 23, $2.57, 2),
    (3, 4, $10.00, 1)

    exec sp_InsertOrdersBatch @orders, @details

Den här lösningen kan varje batch toouse en uppsättning OrderID värden som börjar vid 1. Värdena för tillfälliga OrderID beskriver hello relationer i hello batch men hello faktiska OrderID värdena bestäms när hello hello insert-åtgärden. Du kan köra hello samma instruktioner i hello föregående exempel upprepade gånger och skapa unika order i hello-databasen. Överväg att lägga till mer kod eller databasen logik som förhindrar att duplicerade order när du använder detta batchbearbetning tekniken därför.

Det här exemplet visar att ännu mer komplexa databasåtgärder, till exempel översikt detaljer operations kan grupperas med tabellvärdeparametrar.

### <a name="upsert"></a>UPSERT
En annan batching scenariet inbegriper samtidigt uppdaterar befintliga rader och infoga nya rader. Denna åtgärd är ibland hänvisade tooas en ”UPSERT” (update + insert)-åtgärd. I stället för att göra anrop tooINSERT och uppdatera är hello MERGE-instruktion bäst lämpade toothis uppgift. hello MERGE-instruktion kan utföra både insert och uppdatera åtgärder i ett enda anrop.

Tabellvärdeparametrar kan användas med hello MERGE-instruktion tooperform uppdateringar och infogningar. Anta till exempel att en förenklad medarbetare tabell som innehåller hello följande kolumner: EmployeeID, FirstName, LastName, SocialSecurityNumber:

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

Du kan använda hello faktum att hello SocialSecurityNumber unika tooperform en SAMMANFOGNINGEN av flera medarbetare i det här exemplet. Skapa först hello användardefinierade tabelltyp:

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

Sedan skapar en lagrad procedur eller skriva kod som använder hello MERGE-instruktion tooperform hello update och infoga. hello följande exempel används hello MERGE-instruktion på en tabellvärdesparameter @employees, av typen EmployeeTableType. Hej innehållet i hello @employees tabellen visas inte här.

    MERGE Employee AS target
    USING (SELECT [FirstName], [LastName], [SocialSecurityNumber] FROM @employees) 
    AS source ([FirstName], [LastName], [SocialSecurityNumber])
    ON (target.[SocialSecurityNumber] = source.[SocialSecurityNumber])
    WHEN MATCHED THEN 
    UPDATE SET
    target.FirstName = source.FirstName, 
    target.LastName = source.LastName
    WHEN NOT MATCHED THEN
       INSERT ([FirstName], [LastName], [SocialSecurityNumber])
       VALUES (source.[FirstName], source.[LastName], source.[SocialSecurityNumber]);

Mer information finns i hello dokumentation och exempel för hello MERGE-instruktion. Även om hello samma arbets kunde utföras i flera steg lagrade proceduranrop med separata INSERT och UPDATE-åtgärder, är hello MERGE-instruktion effektivare. Databaskod kan också skapa Transact-SQL-anrop som hello MERGE-instruktion direkt utan att två databasanrop för INSERT och UPDATE.

## <a name="recommendation-summary"></a>Rekommendation sammanfattning
hello innehåller följande lista en sammanfattning av hello batchbearbetning rekommendationerna som beskrivs i det här avsnittet:

* Använd buffring och batchbearbetning tooincrease hello prestanda och skalbarhet SQL-databas.
* Förstå hello kompromisser mellan batchbearbetning/buffring och återhämtning. Vid fel, roll, kan hello risken för att förlora en obearbetat batch affärskritiska data uppväger hello prestandafördelarna batchbearbetning.
* Försök tookeep alla anrop toohello databas inom en enskild datacenter tooreduce fördröjning.
* Om du väljer en enda batching teknik erbjuder tabellvärdeparametrar hello bästa prestanda och flexibilitet.
* Infoga prestanda för hello snabbaste, följer dessa allmänna riktlinjer men testa ditt scenario:
  * Använd ett enda parametrar INSERT-kommando för < 100 rader.
  * Använda tabellvärdeparametrar för < 1000 rader.
  * För > = 1000 rader, Använd SqlBulkCopy körs.
* För update och delete-åtgärder kan använda tabellvärdeparametrar med lagrade proceduren logik som bestämmer hello rätt åtgärden på varje rad i hello tabell parametern.
* Riktlinjer för batch-storlek:
  * Använda hello största batch som passar ditt program och affärskrav.
  * Belastningsutjämna hello prestandaförbättring av stora batchar med hello risk för tillfälliga eller katastrofalt fel. Vad är hello följd av återförsök eller dataförlust hello i hello batch? 
  * Testa hello största batch storlek tooverify att SQL-databasen inte avvisa den.
  * Skapa inställningar som kontrollen batchbearbetning, till exempel hello batchstorlek eller hello buffring tidsperioden. Dessa inställningar ger flexibilitet. Du kan ändra hello batchbearbetning beteende i produktionsmiljö utan att omdistribuera hello-Molntjänsten.
* Undvik parallell körning av batchar som fungerar på en enda tabell i en databas. Om du väljer toodivide en enskild batch över flera trådar kan köra testerna toodetermine hello perfekt antal trådar. När du har ett okänt tröskelvärde fler trådar kommer försämra prestanda i stället öka den.
* Överväg att buffring på storleken och tid som ett sätt att implementera batchbearbetning för flera scenarier.

## <a name="next-steps"></a>Nästa steg
Den här artikeln fokuserar på hur databasdesign och kodning tekniker relaterade toobatching kan förbättra ditt programprestanda och skalbarhet. Men det är bara en faktor i din övergripande säkerhetsstrategi. Mer sätt tooimprove prestanda och skalbarhet finns [Azure SQL Database-prestandaråd för enskilda databaser](sql-database-performance-guidance.md) och [pris- och prestandaöverväganden för en elastisk pool](sql-database-elastic-pool-guidance.md).

