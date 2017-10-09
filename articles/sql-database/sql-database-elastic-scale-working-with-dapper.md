---
title: "aaaUsing klientbibliotek för elastisk databas med Dapper | Microsoft Docs"
description: "Med Dapper klientbibliotek för elastisk databas."
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
ms.assetid: 463d2676-3b19-47c2-83df-f8c50492c9d2
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: torsteng
ms.openlocfilehash: c22ece2a977265e93850f0ad3f3ca48f0a8733ac
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="using-elastic-database-client-library-with-dapper"></a>Med Dapper klientbibliotek för elastisk databas
Det här dokumentet är för utvecklare som förlitar sig på Dapper toobuild program, men också tooembrace [elastisk databas-tooling](sql-database-elastic-scale-introduction.md) toocreate program som implementerar horisontell partitionering tooscale ut sina datanivå.  Det här dokumentet visar hello ändringar i Dapper-baserade program som är nödvändiga toointegrate med elastiska Databasverktyg. Vår fokus på fastställdes hello elastisk databas Fragmentera hanterings- och beroende routning med Dapper. 

**Exempelkoden**: [elastisk Databasverktyg för Azure SQL Database - Dapper integrering](https://code.msdn.microsoft.com/Elastic-Scale-with-Azure-e19fc77f).

Integrera **Dapper** och **DapperExtensions** med hello klientbibliotek för elastisk databas för Azure SQL Database är enkelt. Dina program kan använda data beroende routning genom att ändra hello skapa och öppna i nytt [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekt toouse hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) anropa från hello [klienten biblioteket](http://msdn.microsoft.com/library/azure/dn765902.aspx). Detta begränsar ändringar i dina program tooonly där nya anslutningar skapas och öppnas. 

## <a name="dapper-overview"></a>Dapper översikt
**Dapper** är en Objektrelationer mappning. .NET-objekt från ditt program tooa relationsdatabas (och vice versa) mappas. hello första delen av hello exempelkod visar hur du kan integrera hello klientbibliotek för elastisk databas med Dapper-baserade program. hello andra delen av hello exempelkod visar hur toointegrate när du använder både Dapper och DapperExtensions.  

hello mapper funktioner i Dapper innehåller tilläggsmetoder om databasanslutningar som förenklar skickar T-SQL-uttryck för körning eller frågor hello-databasen. Till exempel Dapper gör det enkelt toomap mellan .NET-objekt och hello parametrar för SQL-uttryck för **kör** samtal eller tooconsume hello resultatet av dina SQL-frågor till .NET-objekt med hjälp av **frågan**anrop från Dapper. 

När du använder DapperExtensions, behöver du inte längre tooprovide hello SQL-instruktioner. Tillägg metoder som **GetList** eller **infoga** över hello databasanslutning skapa hello SQL-instruktioner hello bakgrunden.

En annan fördel med Dapper och DapperExtensions är att hello programkontroller hello skapandet av hello databasanslutning. På så sätt kan interagera med klientbiblioteket för hello elastisk databas som mäklare databasanslutningar baserat på hello mappningen av shardlets toodatabases.

tooget hello Dapper sammansättningar, se [Dapper punkt net](http://www.nuget.org/packages/Dapper/). Hej Dapper tillägg, se [DapperExtensions](http://www.nuget.org/packages/DapperExtensions).

## <a name="a-quick-look-at-hello-elastic-database-client-library"></a>En titt på hello klientbibliotek för elastisk databas
Du kan definiera partitioner för dina programdata som kallas med hello klientbibliotek för elastisk databas, *shardlets* , mappa dem toodatabases och identifiera dem genom *horisontell partitionering nycklar*. Du kan ha valfritt antal databaser du behöver och distribuera din shardlets mellan dessa databaser. hello-mappningen för horisontell partitionering nyckelvärden toohello databaser lagras av en Fragmentera karta som tillhandahålls av hello biblioteket API: er. Den här funktionen kallas **Fragmentera kartan management**. hello Fragmentera kartan fungerar också som hello broker till databaser för begäranden som innehåller en nyckel för horisontell partitionering. Den här funktionen är refererad tooas **data beroende routning**.

![Fragmentera mappar och data beroende Routning][1]

hello Fragmentera kartan manager skyddar användare från inkonsekvent vyer i shardlet data som kan uppstå när samtidiga shardlet hanteringsåtgärder sker på hello-databaser. toodo hello så Fragmentera maps broker hello anslutningar för ett program som skapats med hello-biblioteket. Fragmentera hanteringsåtgärder kan påverka hello shardlet, kan hello Fragmentera kartan funktioner tooautomatically kill en databasanslutning. 

Istället för att använda hello traditionella sätt toocreate anslutningar för Dapper måste toouse hello [OpenConnectionForKey metoden](http://msdn.microsoft.com/library/azure/dn824099.aspx). Detta säkerställer att alla hello validering sker och anslutningar hanteras korrekt när data flyttas mellan shards.

### <a name="requirements-for-dapper-integration"></a>Krav för Dapper integrering
När du arbetar med både hello klientbibliotek för elastisk databas och hello Dapper API: er kan vill vi tooretain hello följande egenskaper:

* **Scaleout**: vi vill tooadd eller ta bort databaser från hello datanivå hello delat tillämpas efter behov för hello kapacitetskraven hello program. 
* **Konsekvenskontroll**: eftersom vårt program utskalad med horisontell partitionering, behöver vi tooperform data beroende routning. Vi vill toouse hello Data beroende funktioner för hello biblioteket toodo så. I synnerhet vi vill tooretain hello validering och konsekvens garanterar tillhandahålls av anslutningar som asynkrona via hello Fragmentera kartan manager i ordning tooavoid skadas eller fel frågeresultat. Detta säkerställer att anslutningar tooa angivna shardlet avvisade eller stoppas om (till exempel) hello shardlet är för närvarande har flyttats tooa olika Fragmentera med delade/Merge API: er.
* **Objektmappningen**: vi vill tooretain hello bekvämlighet hello mappningar som tillhandahålls av Dapper tootranslate mellan klasser i hello program och hello underliggande databasstrukturer. 

hello följande avsnitt innehåller information om kraven för programmen baserat på **Dapper** och **DapperExtensions**.

## <a name="technical-guidance"></a>Teknisk information
### <a name="data-dependent-routing-with-dapper"></a>Data beroende routning med Dapper
Med Dapper ansvarar hello-programmet vanligtvis för att skapa och öppna hello anslutningar toohello underliggande databasen. Typ T som angetts av hello program, returnerar Dapper frågeresultat som .NET samlingar av typen T. Dapper utför hello mappning från hello T-SQL resultatet rader toohello objekt av typen T. På liknande sätt mappar Dapper .NET-objekt till SQL-värden eller parametrar för uttryck data manipulation language (DML). Dapper erbjuder den här funktionen via tilläggsmetoder på hello regelbundna [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekt från hello ADO .NET SQL klientbibliotek. hello SQL-anslutning som returneras av hello Elastic Scale API: er för DDR är också regelbundna [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekt. Detta gör att vi toodirectly Använd Dapper tillägg över hello-typen som returnerades av hello-klientbiblioteket DDR-API, eftersom det är en enkel anslutning för SQL-klienten.

Dessa observationer gör det enkelt toouse anslutningar asynkrona av hello klientbibliotek för elastisk databas för Dapper.

Det här kodexemplet (från hello tillhörande exempel) illustrerar hello metod där hello horisontell partitionering nyckeln tillhandahålls av hello programmet toohello biblioteket toobroker hello anslutning toohello rätt Fragmentera.   

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                     key: tenantId1, 
                     connectionString: connStrBldr.ConnectionString, 
                     options: ConnectionOptions.Validate))
    {
        var blog = new Blog { Name = name };
        sqlconn.Execute(@"
                      INSERT INTO
                            Blog (Name)
                            VALUES (@name)", new { name = blog.Name }
                        );
    }

hello anropet toohello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) API ersätter hello standard skapas eller öppnas av en SQL-klientanslutning. Hej [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) anrop tar hello-argument som krävs för data beroende routning: 

* hello Fragmentera kartan tooaccess hello beroende routning gränssnitt för data
* hello horisontell partitionering viktiga tooidentify hello shardlet
* hello autentiseringsuppgifter (användarnamn och lösenord) tooconnect toohello Fragmentera

hello Fragmentera kartan objektet skapar en anslutning toohello Fragmentera som innehåller hello shardlet för hello angivna horisontell partitionering nyckel. hello elastisk databas-klientens API: er tagga också hello anslutning tooimplement dess konsekvens garanterar. Eftersom hello anropa för[OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) returnerar en vanlig SQL objekt för klientanslutning, hello efterföljande anrop toohello **Execute** metod från Dapper sätt hello standard Dapper praxis.

Frågor arbete mycket hello samma sätt – du först öppna hello anslutning med [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) från hello klient API. Du kan använda hello regelbundna Dapper tillägget metoder toomap hello resultaten av SQL-frågan i .NET-objekt:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId1, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate ))
    {    
           // Display all Blogs for tenant 1
           IEnumerable<Blog> result = sqlconn.Query<Blog>(@"
                                SELECT * 
                                FROM Blog
                                ORDER BY Name");

           Console.WriteLine("All blogs for tenant id {0}:", tenantId1);
           foreach (var item in result)
           {
                Console.WriteLine(item.Name);
            }
    }

Observera att hello **med** med hello DDR anslutning scope blockera alla databasåtgärder i hello block toohello en Fragmentera där tenantId1 sparas. hello frågan returnerar endast bloggar som lagras på hello aktuella Fragmentera, men inte hello som finns på andra delar. 

## <a name="data-dependent-routing-with-dapper-and-dapperextensions"></a>Data beroende routning med Dapper och DapperExtensions
Dapper levereras med en ekosystem med ytterligare filnamnstillägg som kan ge ytterligare underlätta för dig och abstraction från hello databas när du utvecklar databasprogram. DapperExtensions är ett exempel. 

Med hjälp av DapperExtensions i ditt program ändrar inte hur databasanslutningar skapas och hanteras. Det är fortfarande hello programmet ansvar tooopen anslutningar och regelbundna SQL Client anslutningsobjekt förväntas av hello tilläggsmetoder. Vi kan utgå ifrån hello [OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) som beskrivs ovan. Som hello visar följande kodexempel hello enda förändringen är att det inte längre finns toowrite hello T-SQL-uttryck:

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           var blog = new Blog { Name = name2 };
           sqlconn.Insert(blog);
    }

Och här är hello kodexempel för hello-frågan: 

    using (SqlConnection sqlconn = shardingLayer.ShardMap.OpenConnectionForKey(
                    key: tenantId2, 
                    connectionString: connStrBldr.ConnectionString, 
                    options: ConnectionOptions.Validate))
    {
           // Display all Blogs for tenant 2
           IEnumerable<Blog> result = sqlconn.GetList<Blog>();
           Console.WriteLine("All blogs for tenant id {0}:", tenantId2);
           foreach (var item in result)
           {
               Console.WriteLine(item.Name);
           }
    }

### <a name="handling-transient-faults"></a>Hantering av tillfälliga problem
hello Microsoft Patterns & praxis gruppmedlemmarna publicerade hello [tillfälliga fel hanterar programmet Block](http://msdn.microsoft.com/library/hh680934.aspx) toohelp programutvecklare minimera vanliga tillfälligt fel villkor uppstod när körs i hello moln. Mer information finns i [Perseverance, hemligheten för alla framgångar: med hello tillfälliga fel hanterar programmet Block](http://msdn.microsoft.com/library/dn440719.aspx).

hello kodexempel är beroende av hello tillfälligt fel biblioteket tooprotect mot tillfälliga problem. 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
    {
       using (SqlConnection sqlconn = 
          shardingLayer.ShardMap.OpenConnectionForKey(tenantId2, connStrBldr.ConnectionString, ConnectionOptions.Validate))
          {
              var blog = new Blog { Name = name2 };
              sqlconn.Insert(blog);
          }
    });

**SqlDatabaseUtils.SqlRetryPolicy** i hello koden ovan har definierats som en **SqlDatabaseTransientErrorDetectionStrategy** med ett återförsöksvärde på 10 och 5 sekunder väntetid mellan återförsök. Om du använder transaktioner, se till att ditt försök omfång återgår toohello i början av hello transaktion i hello fall av ett tillfälligt fel.

## <a name="limitations"></a>Begränsningar
hello-metoder som beskrivs i det här dokumentet medför några begränsningar:

* hello exempelkod för det här dokumentet visar inte hur toomanage schema över shards.
* Får en förfrågan antar vi att dess databasbearbetning ingår i en enda Fragmentera som identifieras av hello horisontell partitionering nyckel som tillhandahålls av hello-begäran. Men innehåller detta antagande inte alltid, till exempel när det inte är möjligt toomake en nyckel för horisontell partitionering som är tillgängliga. tooaddress detta, hello klientbibliotek för elastisk databas innehåller hello [MultiShardQuery klassen](http://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.query.multishardexception.aspx). hello-klassen implementerar en anslutning abstraktion för frågor över flera delar. Använda MultiShardQuery i kombination med Dapper ligger utanför hello i det här dokumentet.

## <a name="conclusion"></a>Slutsats
Program med hjälp av Dapper och DapperExtensions kan enkelt utnyttja elastisk Databasverktyg för Azure SQL Database. Via hello steg som beskrivs i det här dokumentet, programmen kan använda hello verktyget kapaciteten för data beroende routning genom att ändra hello skapa och öppna i nytt [SqlConnection](http://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx) objekt toouse hello [ OpenConnectionForKey](http://msdn.microsoft.com/library/azure/dn807226.aspx) anrop av hello klientbibliotek för elastisk databas. Detta begränsar hello programmet ändringar krävs toothose platser där nya anslutningar skapas och öppnas. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-working-with-dapper/dapperimage1.png
