---
title: "aaaUsing klientbibliotek för elastisk databas med Entity Framework | Microsoft Docs"
description: "Använd klientbibliotek för elastisk databas och Entity Framework för att koda databaser"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: b9c3065b-cb92-41be-aa7f-deba23e7e159
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/06/2017
ms.author: torsteng
ms.openlocfilehash: 917f6d28d9855c0b42afe2c008613a9bbb3ec6b6
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="elastic-database-client-library-with-entity-framework"></a>Klientbibliotek för elastisk databas med Entity Framework
Det här dokumentet visar hello ändringar i ett Entity Framework-program som är nödvändiga toointegrate med hello [elastisk Databasverktyg](sql-database-elastic-scale-introduction.md). hello fokus ligger på fastställdes [Fragmentera kartan management](sql-database-elastic-scale-shard-map-management.md) och [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) med hello Entity Framework **Code First** metod. Hej [Code först - ny databas](http://msdn.microsoft.com/data/jj193542.aspx) självstudier för EF fungerar som exemplet körs i hela dokumentet. hello exempelkod som åtföljer det här dokumentet är en del av verktygen för elastisk databas uppsättning exempel i hello Visual Studio-kodexempel.

## <a name="downloading-and-running-hello-sample-code"></a>Hämta och kör hello exempelkod
toodownload hello koden för den här artikeln:

* Visual Studio 2012 eller senare krävs. 
* Hämta hello [elastisk DB-verktyg för Azure SQL - Entity Framework-integrering exempel](https://code.msdn.microsoft.com/windowsapps/Elastic-Scale-with-Azure-bae904ba) på MSDN. Packa upp hello exempel tooa plats.
* Starta Visual Studio. 
* Välj Arkiv -> Öppna projekt/lösning i Visual Studio. 
* I hello **öppna projekt** dialogrutan navigera toohello exempel som du hämtade och markera **EntityFrameworkCodeFirst.sln** tooopen hello exempel. 

toorun hello exemplet behöver du toocreate tre tomma databaser i Azure SQL Database:

* Fragmentera kartan Manager-databasen
* Fragmentera 1-databasen
* Fragmentera 2-databas

När du har skapat dessa databaser, Fyll i hello platshållare i **Program.cs** med din Azure SQL DB-servernamn, databasnamn hello och dina autentiseringsuppgifter tooconnect toohello databaser. Skapa hello lösning i Visual Studio. Visual Studio hämtar hello krävs NuGet-paket för hello klientbibliotek för elastisk databas, Entity Framework och hantering som en del av hello build-tillfälligt fel. Se till att återställa NuGet-paket har aktiverats för din lösning. Du kan aktivera den här inställningen genom att högerklicka på hello lösningsfilen i hello Visual Studio Solution Explorer. 

## <a name="entity-framework-workflows"></a>Entity Framework-arbetsflöden
Entity Framework utvecklare förlitar sig på något av följande fyra arbetsflöden toobuild program och tooensure persistence för programobjekt hello: 

* **Code första (ny databas)**: hello EF utvecklare skapar hello modellen i hello programkod och sedan EF genererar hello databasen från den. 
* **Code första (befintlig databas)**: hello utvecklare kan EF generera hello programkod för hello modellen från en befintlig databas.
* **Modellen första**: hello utvecklare skapar hello modellen i hello EF designer och EF skapar sedan hello databasen från hello modell.
* **Databasen första**: hello utvecklare använder EF tooling tooinfer hello modellen från en befintlig databas. 

Alla dessa metoder är beroende av hello DbContext-klassen tootransparently hantera anslutningar och databasschemat för ett program. Som diskuteras i detalj senare i hello dokumentet olika konstruktorer i hello DbContext-basklassen Tillåt för olika nivåer av kontroll över anslutningen Skapa databas startprogram och schemat skapas. Utmaningar uppkommer främst hello fakta som tillhandahålls av EF hello-anslutning databashantering hello anslutning hanteringsfunktioner hello beroende routning av gränssnitt för data som anges av hello klientbibliotek för elastisk databas. 

## <a name="elastic-database-tools-assumptions"></a>Förutsättningar för verktyg för elastisk databas
Termdefinitioner finns [elastisk databas verktyg ordlista](sql-database-elastic-scale-glossary.md).

Med klientbibliotek för elastisk databas, kan du definiera partitioner för dina programdata som kallas shardlets. Shardlets identifieras med en nyckel för horisontell partitionering och mappade toospecific databaser. Ett program kan ha valfritt antal databaser efter behov och distribuera hello shardlets tooprovide tillräckligt med kapacitet eller prestanda angivna aktuella affärsbehov. hello-mappningen för horisontell partitionering nyckelvärden toohello databaser lagras av en Fragmentera karta som tillhandahålls av hello elastisk databas-klientens API: er. Den här funktionen kallas **Fragmentera kartan Management**, eller SMM för kort. hello Fragmentera kartan fungerar också som hello broker till databaser för begäranden som innehåller en nyckel för horisontell partitionering. Vi refererar toothis kapaciteten som **data beroende routning**. 

hello Fragmentera kartan manager skyddar användare från inkonsekvent vyer i shardlet data som kan uppstå när samtidiga shardlet hanteringsåtgärder (till exempel omlokalisering data från en Fragmentera tooanother) sker. toodo hello så Fragmentera maps hanteras av hello biblioteket broker hello databasen klientanslutningar för ett program. Detta tillåter hello Fragmentera kartan funktioner tooautomatically kill en databasanslutning när Fragmentera hanteringsåtgärder kan påverka hello shardlet som hello anslutningen har skapats för. Den här metoden måste toointegrate med några av EFS funktioner, till exempel skapa nya anslutningar från en befintlig en toocheck för databasen. I allmänhet våra observationer har att hello standard DbContext-konstruktorer bara fungerar på ett tillförlitligt sätt för stängd databasanslutningar som kan klonas på ett säkert sätt för EF arbete. hello design principen för elastisk databas är i stället tooonly broker öppna anslutningar. En tror att stänga en anslutning som asynkrona av hello klientbiblioteket innan överlämnande toohello EF DbContext kan lösa problemet. Genom att stänga hello anslutning och förlita dig på EF toore öppna det, kan en dock foregoes hello validering och konsekvens kontroller som utförs av hello-biblioteket. hello migreringar funktioner i EF, men använder dessa anslutningar toomanage hello underliggande databasschemat på ett sätt som är transparent toohello program. Vi rekommenderar vi skulle som tooretain och kombinera alla dessa funktioner från både hello klientbibliotek för elastisk databas och EF i hello samma program. hello beskrivs följande avsnitt de här egenskaperna och kraven i detalj. 

## <a name="requirements"></a>Krav
När du arbetar med både hello klientbibliotek för elastisk databas och Entity Framework-API: er kan vill vi tooretain hello följande egenskaper: 

* **Skalbar**: tooadd eller ta bort databaser från hello datanivå hello delat tillämpas efter behov för hello kapacitetskraven hello program. Det innebär att kontroll över hello hello skapas eller tas bort av databaser och använder hello elastisk databas Fragmentera kartan manager API: er toomanage databaser och mappningar av shardlets. 
* **Konsekvenskontroll**: hello program använder horisontell partitionering och använder hello data beroende funktioner för hello klientbiblioteket. tooavoid skadas eller fel frågeresultat anslutningar är asynkrona via hello Fragmentera kartan manager. Detta bevarar validering och konsekvens.
* **Code första**: tooretain hello lättare EFS kod första paradigmet. I den första koden mappas klasser i programmet hello transparent toohello underliggande databasstrukturer. hello programkod samverkar med DbSets som maskerar de flesta aspekter hello underliggande databasen bearbetas.
* **Schemat**: Entity Framework hanterar inledande databasen schemat skapas och efterföljande schemat utvecklingen via migreringar. Behålla dessa funktioner är det enkelt som hello data utvecklas att anpassa din app. 

hello följande riktlinjer anger hur toosatisfy kraven för Code First program med elastiska Databasverktyg. 

## <a name="data-dependent-routing-using-ef-dbcontext"></a>Data beroende routning med EF DbContext
Databasanslutningar med Entity Framework vanligtvis hanteras via underklasser för **DbContext**. Skapa dessa underklasser som härleds från **DbContext**. Är där du definierar din **DbSets** som implementerar hello databasen har säkerhetskopierats samlingar av CLR-objekt för tillämpningsprogrammet. Vi kan identifiera flera användbara egenskaper som inte nödvändigtvis har för andra EF code första Programscenarier hello gäller data beroende routning är: 

* hello databas redan finns och har registrerats i hello elastisk databas Fragmentera kartan. 
* hello schemat för programmet hello redan distribuerade toohello databasen (beskrivs nedan). 
* Data dependent routning anslutningar toohello databasen asynkrona hello Fragmentera Map. 

toointegrate **DbContexts** med data beroende routning för skalbar:

1. Skapa fysiska databasanslutningar via hello elastisk Databasgränssnitt hello Fragmentera kartan manager 
2. Omsluta hello anslutning med hello **DbContext** underklass
3. Skicka hello anslutningen till hello **DbContext** basera klasser tooensure all hello bearbetning på hello EF sida samt händer. 

hello följande kodexempel illustrerar den här metoden. (Den här koden är också i hello tillhörande Visual Studio-projekt)

    public class ElasticScaleContext<T> : DbContext
    {
    public DbSet<Blog> Blogs { get; set; }
    …

        // C'tor for data dependent routing. This call will open a validated connection 
        // routed toohello proper shard by hello shard map manager. 
        // Note that hello base class c'tor call will fail for an open connection
        // if migrations need toobe done and SQL credentials are used. This is hello reason for hello 
        // separation of c'tors into hello data-dependent routing case (this c'tor) and hello internal c'tor for new shards.
        public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
            : base(CreateDDRConnection(shardMap, shardingKey, connectionStr), 
            true /* contextOwnsConnection */)
        {
        }

        // Only static methods are allowed in calls into base class c'tors.
        private static DbConnection CreateDDRConnection(
        ShardMap shardMap, 
        T shardingKey, 
        string connectionStr)
        {
            // No initialization
            Database.SetInitializer<ElasticScaleContext<T>>(null);

            // Ask shard map toobroker a validated connection for hello given key
            SqlConnection conn = shardMap.OpenConnectionForKey<T>
                                (shardingKey, connectionStr, ConnectionOptions.Validate);
            return conn;
        }    

## <a name="main-points"></a>Huvudpunkter
* En ny konstruktor ersätter hello standardkonstruktor i hello DbContext underklass 
* nya hello-konstruktorn har hello-argument som krävs för data beroende Routning via klientbibliotek för elastisk databas:
  
  * hello Fragmentera kartan tooaccess hello data beroende routning gränssnitt,
  * hello horisontell partitionering viktiga tooidentify hello shardlet
  * en anslutningssträng med hello autentiseringsuppgifter för hello data beroende routning anslutning toohello Fragmentera. 
* hello anropet toohello basklass konstruktorn tar en detour i en statisk metod som utför alla hello steg krävs för data-beroende routning. 
  
  * Den använder hello OpenConnectionForKey anrop av hello elastisk Databasgränssnitt på hello Fragmentera kartan tooestablish en öppen anslutning.
  * hello Fragmentera kartan skapar hello öppen anslutning toohello Fragmentera som innehåller hello shardlet för hello angivna horisontell partitionering nyckel.
  * Öppna anslutningen skickas tillbaka toohello basklass konstruktorn för DbContext tooindicate att den här anslutningen är toobe som används av EF i stället för att låta EF automatiskt skapa en ny anslutning. Det här sättet hello-anslutning har taggats av hello elastisk databas klienten API så att den kan garantera konsekvens under Fragmentera kartan hanteringsåtgärder.

Använd hello nya konstruktor för DbContext-underklass i stället för hello standardkonstruktor i koden. Här är ett exempel: 

    // Create and save a new blog.

    Console.Write("Enter a name for a new blog: "); 
    var name = Console.ReadLine(); 

    using (var db = new ElasticScaleContext<int>( 
                            sharding.ShardMap,  
                            tenantId1,  
                            connStrBldr.ConnectionString)) 
    { 
        var blog = new Blog { Name = name }; 
        db.Blogs.Add(blog); 
        db.SaveChanges(); 

        // Display all Blogs for tenant 1 
        var query = from b in db.Blogs 
                    orderby b.Name 
                    select b; 
     … 
    }

hello nya konstruktorn öppnar hello anslutning toohello Fragmentera som lagrar hello data för hello shardlet identifieras av hello värdet för **tenantid1**. Hej koden i hello **med** block förblir oförändrad tooaccess hello **DbSet** för bloggar med EF på hello Fragmentera för **tenantid1**. Detta ändrar semantik för hello koden i hello med block så att alla databasåtgärder nu omfång toohello en Fragmentera där **tenantid1** sparas. Till exempel en LINQ-fråga över hello bloggar **DbSet** skulle bara returnera bloggar som lagras på hello aktuella Fragmentera, men inte hello de som lagras på andra delar.  

#### <a name="transient-faults-handling"></a>Tillfälliga problem hantering
hello Microsoft Patterns & praxis gruppmedlemmarna publicerade hello [hello tillfälliga fel hanterar programmet Block](https://msdn.microsoft.com/library/dn440719.aspx). hello biblioteket används med elastisk skalbarhet klientbiblioteket i kombination med EF. Se dock till att alla tillfälligt undantag returnerar tooa plats där vi kan se till att nya hello-konstruktor som används efter ett tillfälligt fel så att alla nya anslutningsförsök görs med hjälp av hello-konstruktorer som vi har tweaked. En anslutning toohello korrekt Fragmentera inte är säkert och det finns inga garantier hello anslutning underhålls annars som ändringarna toohello Fragmentera karta visas. 

hello följande kodexempel illustrerar hur en SQL-återförsöksprincip kan användas runt hello nya **DbContext** underklasskonstruktorer: 

    SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
    { 
        using (var db = new ElasticScaleContext<int>( 
                                sharding.ShardMap,  
                                tenantId1,  
                                connStrBldr.ConnectionString)) 
            { 
                    var blog = new Blog { Name = name }; 
                    db.Blogs.Add(blog); 
                    db.SaveChanges(); 
            … 
            } 
        }); 

**SqlDatabaseUtils.SqlRetryPolicy** i hello koden ovan har definierats som en **SqlDatabaseTransientErrorDetectionStrategy** med ett återförsöksvärde på 10 och 5 sekunder väntetid mellan återförsök. Den här metoden är liknande toohello vägledning för EF och användarinitierad transaktioner (se [begränsningar med du försöker köra strategier (och senare EF6)](http://msdn.microsoft.com/data/dn307226). Båda situationer kräver att programmet hello styr hello scope toowhich hello tillfälligt undantag returnerar: tooeither öppna hello transaktion eller återskapa hello kontext från hello rätt konstruktor som använder hello elastisk databas (som visas) klientbiblioteket.

hello måste toocontrol där tillfälligt undantag ta oss tillbaka i omfånget utesluter också hello användning av hello inbyggda **SqlAzureExecutionStrategy** som medföljer EF. **SqlAzureExecutionStrategy** skulle öppna en anslutning utan att använda **OpenConnectionForKey** och därför kringgå alla hello-verifieringen som utförs som en del av hello **OpenConnectionForKey** anropa. Hello kodexempel används i stället hello inbyggda **DefaultExecutionStrategy** som medföljer också EF. I motsats för**SqlAzureExecutionStrategy**, den fungerar korrekt i kombination med hello återförsöksprincip tillfälliga fel hanterar. hello körningsprincipen är inställd i hello **ElasticScaleDbConfiguration** klass. Observera att vi valt inte toouse **DefaultSqlExecutionStrategy** eftersom föreslås toouse **SqlAzureExecutionStrategy** om tillfälligt undantag inträffar - som skulle leda toowrong beteende enligt beskrivningen. Mer information om hello annan retry principer och EF finns [Connection Resiliency i EF](http://msdn.microsoft.com/data/dn456835.aspx).     

#### <a name="constructor-rewrites"></a>Konstruktorn omskrivningar
hello kodexemplen ovan visar hello standard konstruktorn skriver krävs för ditt program i ordning toouse data beroende routning med hello Entity Framework. den här metoden tooother konstruktorer är en generalisering hello i den följande tabellen. 

| Aktuella konstruktor | Omskrivet konstruktor för data | Grundläggande konstruktor | Anteckningar |
| --- | --- | --- | --- |
| MyContext() |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection bool) |hello anslutningen behöver toobe en funktion av hello Fragmentera karta och hello data beroende routning nyckel. Du behöver tooby pass automatisk anslutning skapande av EF och i stället använda hello Fragmentera kartan toobroker hello anslutning. |
| MyContext(string) |ElasticScaleContext (ShardMap, TKey) |DbContext (DbConnection bool) |hello anslutningen är en funktion av hello Fragmentera karta och hello data beroende routning nyckel. Fasta databasrollen namn eller anslutningssträngen fungerar inte som de hoppa över verifieringen av hello Fragmentera kartan. |
| MyContext(DbCompiledModel) |ElasticScaleContext (ShardMap TKey, DbCompiledModel) |DbContext (DbConnection DbCompiledModel, bool) |hello anslutning skapas för hello anges Fragmentera karta och horisontell partitionering nyckeln med hello mallen. hello kompilerade modellen skickas toohello grundläggande c'tor. |
| MyContext (DbConnection bool) |ElasticScaleContext (ShardMap TKey, bool) |DbContext (DbConnection bool) |hello anslutning måste härledas från hello Fragmentera karta och hello nyckeln toobe. Det går inte att ange indata (såvida inte dessa indata har redan använder hello Fragmentera karta och hello nyckel). hello booleskt värde skickas. |
| MyContext (sträng, DbCompiledModel) |ElasticScaleContext (ShardMap TKey, DbCompiledModel) |DbContext (DbConnection DbCompiledModel, bool) |hello anslutning måste härledas från hello Fragmentera karta och hello nyckeln toobe. Det går inte att ange indata (såvida inte dessa indata använde hello Fragmentera karta och hello nyckel). hello kompilerade modellen skickas. |
| MyContext (ObjectContext bool) |ElasticScaleContext (ShardMap TKey ObjectContext, bool) |DbContext (ObjectContext bool) |hello nya konstruktorn måste tooensure eventuella i hello ObjectContext skickas som indata är ändrats tooa anslutning hanteras av elastisk skalbarhet. En detaljerad beskrivning av ObjectContexts ligger utanför hello i det här dokumentet. |
| MyContext (DbConnection DbCompiledModel, bool) |ElasticScaleContext (ShardMap TKey DbCompiledModel, bool) |DbContext (DbConnection DbCompiledModel, bool;) |hello anslutning måste härledas från hello Fragmentera karta och hello nyckeln toobe. hello-anslutningen kan inte anges som indata (såvida inte dessa indata har redan använder hello Fragmentera karta och hello nyckel). Modellen och booleskt värde skickas toohello basklasskonstruktor. |

## <a name="shard-schema-deployment-through-ef-migrations"></a>Fragmentera schemat distribution via EF migrering
Schemahantering av automatiska är i syfte att underlätta tillhandahålls av hello Entity Framework. Hello gäller program med elastiska Databasverktyg är vill vi tooretain den här funktionen tooautomatically etablera hello schemat toonewly skapade shards när databaserna läggs toohello delat program. Det är tooincrease kapaciteten på hello datanivå för delat program som använder EF hello primära användningsfall. Förlita dig på EFS funktioner för schemahantering av minskar hello databasen administration arbete med ett delat program som bygger på EF. 

Schemat distribution via EF migreringar fungerar bäst med **inte öppnats anslutningar**. Detta är däremot toohello scenario för data beroende routning som förlitar sig på hello öppnas anslutning som tillhandahålls av hello elastisk databas klienten API. En annan skillnad hello konsekvenskontroll krävs: när önskvärt tooensure konsekvenskontroll för alla data beroende routning anslutningar tooprotect mot samtidiga Fragmentera kartan manipulering, det är inte ett problem med inledande schema distribution tooa ny databas som har ännu inte har registrerats i hello Fragmentera kartan och har inte allokerats toohold shardlets. Vi kan därför förlitar sig på vanlig databasanslutningar för den här scenarier som skillnad från toodata-beroende routning.  

Detta leder tooan metod där schemat distribution via EF migreringar är direkt kopplade till hello registrering av nya hello-databasen som en Fragmentera hello programmet Fragmentera kartan. Detta är beroende av hello följande krav: 

* hello-databasen har redan skapats. 
* hello-databasen är tom, det innehåller inget användarschema och inga användardata.
* hello databasen tillgängliga inte ännu via hello elastisk databas-klientens API: er för data-beroende routning. 

När dessa krav är uppfyllda, kan vi skapa en vanlig icke öppnade **SqlConnection** tookick av EF migrering för distribution av schema. hello följande kodexempel illustrerar den här metoden. 

        // Enter a new shard - i.e. an empty database - toohello shard map, allocate a first tenant tooit  
        // and kick off EF intialization of hello database toodeploy schema 

        public void RegisterNewShard(string server, string database, string connStr, int key) 
        { 

            Shard shard = this.ShardMap.CreateShard(new ShardLocation(server, database)); 

            SqlConnectionStringBuilder connStrBldr = new SqlConnectionStringBuilder(connStr); 
            connStrBldr.DataSource = server; 
            connStrBldr.InitialCatalog = database; 

            // Go into a DbContext tootrigger migrations and schema deployment for hello new shard. 
            // This requires an un-opened connection. 
            using (var db = new ElasticScaleContext<int>(connStrBldr.ConnectionString)) 
            { 
                // Run a query tooengage EF migrations 
                (from b in db.Blogs 
                    select b).Count(); 
            } 

            // Register hello mapping of hello tenant toohello shard in hello shard map. 
            // After this step, data-dependent routing on hello shard map can be used 

            this.ShardMap.CreatePointMapping(key, shard); 
        } 


Det här exemplet visar hello metoden **RegisterNewShard** att registren hello Fragmentera hello Fragmentera kartan distribuerar hello schemat via EF migreringar och lagrar en mappning av en nyckel toohello Fragmentera för horisontell partitionering. Det är beroende av en konstruktor med hello **DbContext** underklass (**ElasticScaleContext** i hello exemplet) som tar en SQL-anslutningssträng som indata. hello-koden för den här konstruktorn är enkel, som följande exempel visar hello: 

        // C'tor toodeploy schema and migrations tooa new shard 
        protected internal ElasticScaleContext(string connectionString) 
            : base(SetInitializerForConnection(connectionString)) 
        { 
        } 

        // Only static methods are allowed in calls into base class c'tors 
        private static string SetInitializerForConnection(string connnectionString) 
        { 
            // We want existence checks so that hello schema can get deployed 
            Database.SetInitializer<ElasticScaleContext<T>>( 
        new CreateDatabaseIfNotExists<ElasticScaleContext<T>>()); 

            return connnectionString; 
        } 

En kan ha använt hello version av hello-konstruktor som ärvts från hello basklass. Men hello kod måste tooensure som standard initieraren för EF hello används vid anslutning. Därför hello kort detour till hello statisk metod innan anrop till hello basklasskonstruktor med hello anslutningssträngen. Observera att hello registrering av shards ska köras i en annan app domän eller processen tooensure som hello initieraren inställningar för EF inte står i konflikt. 

## <a name="limitations"></a>Begränsningar
hello-metoder som beskrivs i det här dokumentet medför några begränsningar: 

* EF-program som använder **LocalDb** toomigrate tooa vanlig SQL Server-databas måste först innan du använder klientbibliotek för elastisk databas. Skala ut ett program via delning med elastisk skalbarhet är inte möjligt med **LocalDb**. Observera att utveckling kan fortfarande använda **LocalDb**. 
* Alla ändringar toohello program som implicerar databasen schemaändringar måste toogo via EF migreringar på alla delar. hello exempelkod för det här dokumentet visar inte hur toodo detta. Överväg att använda Update-databasen med en ConnectionString parametern tooiterate över alla delar; eller extrahera hello T-SQL-skript för hello väntande migrering med hjälp av Update-databas med hello - skript och tillämpa hello T-SQL-skriptet tooyour delar.  
* Få en begäran, förutsätts att alla dess databasbearbetning ingår i en enda Fragmentera som identifieras av hello horisontell partitionering nyckel som tillhandahålls av hello-begäran. Men har detta antagande inte alltid värdet true. Till exempel när det är inte möjligt toomake en nyckel för horisontell partitionering som är tillgängliga. tooaddress detta, hello klientbiblioteket tillhandahåller hello **MultiShardQuery** klassen som implementerar en anslutning abstraktion för frågor över flera delar. Lär dig toouse hello **MultiShardQuery** i kombination med EF ligger utanför hello i det här dokumentet

## <a name="conclusion"></a>Slutsats
Via hello steg som beskrivs i det här dokumentet, EF program kan använda hello elastisk databas klienten bibliotekets kapaciteten för data beroende routning av eftersom konstruktorerna i hello **DbContext** underklasser som används i hello EF programmet. Den här gränsen hello ändringar krävs toothose platser där **DbContext** klasser finns redan. EF program kan dessutom fortsätta toobenefit från schemat för automatisk distribution genom att kombinera hello steg som anropar hello nödvändiga EF migrering med hello registrering av nya delar och avbildningar i hello Fragmentera kartan. 

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

<!--Image references-->
[1]: ./media/sql-database-elastic-scale-use-entity-framework-applications-visual-studio/sample.png
