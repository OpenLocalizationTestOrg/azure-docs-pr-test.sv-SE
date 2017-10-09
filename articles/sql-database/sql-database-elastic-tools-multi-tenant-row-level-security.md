---
title: "aaaMulti klient program med elastiska Databasverktyg och säkerhet på radnivå"
description: "Lär dig hur toouse elastisk Databasverktyg tillsammans med radnivå säkerhet toobuild ett program med en mycket skalbar nivå på Azure SQL-databas som har stöd för flera innehavare delar."
metakeywords: azure sql database elastic tools multi tenant row level security rls
services: sql-database
documentationcenter: 
manager: jhubbard
author: tmullaney
ms.assetid: e72d3cfe-e9be-4326-b776-9c6d96c0a18e
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/27/2016
ms.author: thmullan;torsteng
ms.openlocfilehash: e00076a8db4a295374993aedd49f2318bd4d701d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a>Program med flera klienter med elastiska Databasverktyg och säkerhet på radnivå
[Elastisk Databasverktyg](sql-database-elastic-scale-get-started.md) och [på radnivå (RLS) för säkerhet](https://msdn.microsoft.com/library/dn765131) erbjuder en kraftfull uppsättning funktioner för att effektivt och flexibelt skala hello datanivå i ett program för flera innehavare med Azure SQL Database. Se [designmönster för flera innehavare SaaS-program med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) för mer information. 

Den här artikeln beskrivs hur toouse dessa tekniker tillsammans toobuild för ett program med en mycket skalbar datanivå som har stöd för flera innehavare shards, med hjälp av **ADO.NET SqlClient** och/eller **Entity Framework**.  

* **Elastisk Databasverktyg** kan utvecklare tooscale ut hello datanivå för ett program via branschstandard horisontell partitionering metoder med en uppsättning .NET-bibliotek och mallar för Azure-tjänsten. Hantera shards med hello klientbibliotek för elastisk databas kan automatisera och förenkla många hello infrastrukturella uppgifter som vanligtvis är kopplad till horisontell partitionering. 
* **Säkerhet för radnivå** gör att utvecklare toostore data för flera klienter i hello samma databasen med säkerhet principer toofilter ut rader som inte tillhör toohello klient kör en fråga. Centralisera åtkomst logik med RLS inuti hello databas, snarare växer än i hello program förenklar du Underhåll och minskar hello risken för fel som ett program-kodbas. 

Med hjälp av dessa funktioner tillsammans, ett program kan dra nytta av kostnaden besparingar och effektivitet vinster genom att lagra data för flera innehavare i hello samma fragment-databasen. På hello har samma gång, ett program som fortfarande hello flexibilitet toooffer isolerad enskild klient shards för ”premium-klienter som behöver garantier för strängare prestanda eftersom flera innehavare delar inte kan garantera lika resurs distributionen till klienter.  

Kort sagt: hello elastisk databas-klientbiblioteket [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) API: er automatiskt ska ansluta klienter toohello rätt Fragmentera databas som innehåller sin horisontell partitionering nyckel (vanligtvis en ”TenantId”). När du är ansluten, säkerställer en RLS säkerhetsprincip hello databasen att innehavare endast har åtkomst till rader som innehåller sina TenantId. Det förutsätts att alla tabeller innehåller en TenantId kolumnen tooindicate vilka rader som tillhör tooeach klient. 

![Bloggar app-arkitektur][1]

## <a name="download-hello-sample-project"></a>Hämta hello exempelprojektet
### <a name="prerequisites"></a>Krav
* Använda Visual Studio (2012 eller senare) 
* Skapa tre Azure SQL-databaser 
* Hämta exempelprojektet: [elastisk DB-verktyg för Azure SQL - Shards för flera innehavare](http://go.microsoft.com/?linkid=9888163)
  * Fyll i hello information för dina databaser hello början av **Program.cs** 

Det här projektet utökar hello en beskrivs i [elastisk DB-verktyg för Azure SQL - Entity Framework-integrering](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) genom att lägga till stöd för flera innehavare Fragmentera databaser. Den skapar ett enkelt konsolprogram för att skapa bloggar och inlägg, med fyra klienter och två databaser för flera innehavare Fragmentera enligt beskrivningen i hello ovan diagram. 

Skapa och köra programmet hello. Detta kommer bootstrap hello elastisk Databasverktyg Fragmentera kartan manager och kör hello följande test: 

1. Använder Entity Framework och LINQ, skapa en ny blogg och sedan visa alla bloggar för varje klient
2. Med ADO.NET SqlClient kan visa alla bloggar för en klient
3. Försök tooinsert en blogg för hello fel klient tooverify som ett felmeddelande  

Observera att eftersom RLS inte ännu har aktiverats i hello Fragmentera databaser, var och en av de här testerna visar att ett problem: klienter är kan toosee bloggar som inte tillhör toothem och hello program inte hindras från att lägga till en blogg för hello fel klient. hello resten av den här artikeln beskriver hur tooresolve dessa problem genom att tillämpa klient isolering med RLS. Det finns två steg: 

1. **Programmet nivå**: ändra hello programkod tooalways set hello aktuella TenantId i hello SESSION_CONTEXT när du har öppnat en anslutning. hello exempelprojektet har gjort det redan. 
2. **Datanivå**: skapa en säkerhetsprincip för RLS i varje Fragmentera databasen toofilter rader med hello TenantId lagras i SESSION_CONTEXT. Du behöver toodo detta för varje Fragmentera databaserna, annars rader i flera delar filtreras inte. 

## <a name="step-1-application-tier-set-tenantid-in-hello-sessioncontext"></a>Steg 1) programmet nivå: Ange TenantId i hello SESSION_CONTEXT
När du ansluter tooa Fragmentera databasen med hjälp av hello elastisk databas klienten bibliotekets data beroende routning API: er, hello programmet fortfarande måste tootell hello databasen använder vilka TenantId anslutningen så att en RLS säkerhetsprincip kan filtrera ut rader tillhör tooother innehavarna. Hej rekommenderade sättet toopass informationen är toostore hello aktuella TenantId för anslutningen i hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx). (Obs: du kan också använda [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), men SESSION_CONTEXT är ett bättre alternativ eftersom det är enklare toouse returnerar NULL som standard och har stöd för nyckel-värdepar.)

### <a name="entity-framework"></a>Entity Framework
För program som använder Entity Framework, hello enklaste metoden är tooset hello SESSION_CONTEXT inom hello ElasticScaleContext åsidosätta beskrivs i [Data beroende routning med EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext). Innan det returneras hello anslutning asynkrona via data beroende routning, skapa och köra en SqlCommand som anger 'TenantId' i hello SESSION_CONTEXT toohello shardingKey som angetts för anslutningen. Det här sättet behöver du bara toowrite kod när tooset hello SESSION_CONTEXT. 

```
// ElasticScaleContext.cs 
// ... 
// C'tor for data dependent routing. This call will open a validated connection routed toohello proper 
// shard by hello shard map manager. Note that hello base class c'tor call will fail for an open connection 
// if migrations need toobe done and SQL credentials are used. This is hello reason for hello  
// separation of c'tors into hello DDR case (this c'tor) and hello internal c'tor for new shards. 
public ElasticScaleContext(ShardMap shardMap, T shardingKey, string connectionStr)
    : base(OpenDDRConnection(shardMap, shardingKey, connectionStr), true /* contextOwnsConnection */)
{
}

public static SqlConnection OpenDDRConnection(ShardMap shardMap, T shardingKey, string connectionStr)
{
    // No initialization
    Database.SetInitializer<ElasticScaleContext<T>>(null);

    // Ask shard map toobroker a validated connection for hello given key
    SqlConnection conn = null;
    try
    {
        conn = shardMap.OpenConnectionForKey(shardingKey, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT tooshardingKey tooenable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", shardingKey);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
} 
// ... 
```

Nu hello SESSION_CONTEXT anges automatiskt med angiven hello TenantId när ElasticScaleContext anropas: 

```
// Program.cs 
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))   
    {     
        var query = from b in db.Blogs
                    orderby b.Name
                    select b;

        Console.WriteLine("All blogs for TenantId {0}:", tenantId);     
        foreach (var item in query)     
        {       
            Console.WriteLine(item.Name);     
        }   
    } 
}); 
```

### <a name="adonet-sqlclient"></a>ADO.NET SqlClient
För program med hjälp av ADO.NET SqlClient hello rekommenderad metod är toocreate funktionen wrapper runt ShardMap.OpenConnectionForKey() som anger automatiskt 'TenantId' hello SESSION_CONTEXT toohello korrigera TenantId innan det returneras en anslutning. tooensure som SESSION_CONTEXT alltid som du bör endast öppna anslutningar som använder den här wrapper-funktionen.

```
// Program.cs
// ...

// Wrapper function for ShardMap.OpenConnectionForKey() that automatically sets SESSION_CONTEXT with hello correct
// tenantId before returning a connection. As a best practice, you should only open connections using this 
// method tooensure that SESSION_CONTEXT is always set before executing a query.
public static SqlConnection OpenConnectionForTenant(ShardMap shardMap, int tenantId, string connectionStr)
{
    SqlConnection conn = null;
    try
    {
        // Ask shard map toobroker a validated connection for hello given key
        conn = shardMap.OpenConnectionForKey(tenantId, connectionStr, ConnectionOptions.Validate);

        // Set TenantId in SESSION_CONTEXT tooshardingKey tooenable Row-Level Security filtering
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"exec sp_set_session_context @key=N'TenantId', @value=@shardingKey";
        cmd.Parameters.AddWithValue("@shardingKey", tenantId);
        cmd.ExecuteNonQuery();

        return conn;
    }
    catch (Exception)
    {
        if (conn != null)
        {
            conn.Dispose();
        }

        throw;
    }
}

// ...

// Example query via ADO.NET SqlClient
// If row-level security is enabled, only Tenant 4's blogs will be listed
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() =>
{
    using (SqlConnection conn = OpenConnectionForTenant(sharding.ShardMap, tenantId4, connStrBldr.ConnectionString))
    {
        SqlCommand cmd = conn.CreateCommand();
        cmd.CommandText = @"SELECT * FROM Blogs";

        Console.WriteLine("--\nAll blogs for TenantId {0} (using ADO.NET SqlClient):", tenantId4);
        SqlDataReader reader = cmd.ExecuteReader();
        while (reader.Read())
        {
            Console.WriteLine("{0}", reader["Name"]);
        }
    }
});

```

## <a name="step-2-data-tier-create-row-level-security-policy"></a>Steg 2) Data tjänstnivån: skapa radnivå säkerhetsprincip
### <a name="create-a-security-policy-toofilter-hello-rows-each-tenant-can-access"></a>Skapa en säkerhet princip toofilter hello rader som har åtkomst till varje klient
Nu när programmet hello anger SESSION_CONTEXT med hello aktuella TenantId innan du hämtar, en säkerhetsprincip för RLS filtrera frågor och utelämna rader som har en annan TenantId.  

RLS har implementerats i T-SQL: en användardefinierad funktion definierar hello åtkomst logik och en säkerhetsprincip Binder den här funktionen tooany antal tabeller. För det här projektet verifierar hello funktionen bara att hello program (i stället för en annan SQL-användare) är anslutet toohello databas och att hello 'TenantId' lagras i hello SESSION_CONTEXT matchar hello klient-ID för en viss rad. Ett filterpredikat tillåter rader som uppfyller dessa villkor toopass genom hello filter för SELECT-, UPDATE- och DELETE-frågor. och ett block-predikat förhindras rader som strider mot dessa villkor från att infogad eller uppdaterad. Om SESSION_CONTEXT inte har angetts, returneras NULL och inga rader kommer att vara synlig eller kan toobe infogas. 

tooenable RLS, kör följande T-SQL på alla delar med hjälp av antingen Visual Studio (SSDT), SSMS, hello eller hello PowerShell-skript som ingår i projektet hello (eller om du använder [elastiska databasen jobb](sql-database-elastic-jobs-overview.md), kan du använda det tooautomate körning i den här T-SQL på alla shards): 

```
CREATE SCHEMA rls -- separate schema tooorganize RLS objects 
GO

CREATE FUNCTION rls.fn_tenantAccessPredicate(@TenantId int)     
    RETURNS TABLE     
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult          
        WHERE DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- hello user in your application’s connection string (dbo is only for demo purposes!)         
        AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
GO

CREATE SECURITY POLICY rls.tenantAccessPolicy
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Blogs,
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.Posts
GO 
```

> [!TIP]
> Du kan använda en helper lagrade procedur som automatiskt genererar en säkerhetsprincip för att lägga till ett predikat för alla tabeller i ett schema för mer komplexa projekt som kräver en tooadd hello predikat på hundratals olika tabeller. Se [gäller säkerhet på radnivå tooall tabeller - helper-skript (blogg)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).  
> 
> 

Nu om du kör hello exempelprogrammet igen ska klienter kunna toosee endast rader som tillhör toothem. Dessutom hello programmet går inte att infoga rader som tillhör tootenants än hello en anslutna toohello Fragmentera databas och det går inte att uppdatera synliga rader toohave olika TenantId. Om toodo försöker antingen hello program, aktiveras en DbUpdateException.

Om du lägger till en ny tabell senare bara ALTER hello säkerhetsprincip och Lägg till filter och block-predikat på hello ny tabell: 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-tooautomatically-populate-tenantid-for-inserts"></a>Lägg till standard begränsningar tooautomatically fylla TenantId för infogningar
Du kan placera en standard-begränsning för varje tabell tooautomatically fylla hello TenantId med hello värdet som för närvarande lagras i SESSION_CONTEXT vid infogning av rader. Exempel: 

```
-- Create default constraints tooauto-populate TenantId with hello value of SESSION_CONTEXT for inserts 
ALTER TABLE Blogs     
    ADD CONSTRAINT df_TenantId_Blogs      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO

ALTER TABLE Posts     
    ADD CONSTRAINT df_TenantId_Posts      
    DEFAULT CAST(SESSION_CONTEXT(N'TenantId') AS int) FOR TenantId 
GO 
```

Hello program behöver nu inte toospecify en TenantId när du infogar rader: 

```
SqlDatabaseUtils.SqlRetryPolicy.ExecuteAction(() => 
{   
    using (var db = new ElasticScaleContext<int>(sharding.ShardMap, tenantId, connStrBldr.ConnectionString))
    {
        var blog = new Blog { Name = name }; // default constraint sets TenantId automatically     
        db.Blogs.Add(blog);     
        db.SaveChanges();   
    } 
}); 
```

> [!NOTE]
> Om du använder standardbegränsningar för en Entity Framework-projekt, rekommenderas att du inte inkluderar hello TenantId kolumnen i datamodellen EF. Det beror på att automatiskt ange standardvärden som åsidosätter hello standardbegränsningar skapas i T-SQL som använder SESSION_CONTEXT i Entity Framework-frågor. toouse standardbegränsningar i hello exempel projekt, till exempel bör du ta bort TenantId från DataClasses.cs (och kör Add-migrering i hello Package Manager-konsolen) och Använd T-SQL-tooensure som hello fältet finns bara i hello databastabeller. Det här sättet EF kommer inte automatiskt anger felaktiga standardvärden vid infogning av data. 
> 
> 

### <a name="optional-enable-a-superuser-tooaccess-all-rows"></a>(Valfritt) Aktivera en ”superanvändare” tooaccess alla rader
Vissa program kanske toocreate ”superanvändare” som har åtkomst till alla rader, till exempel i ordning tooenable rapportering över alla klienter i alla delar eller tooperform dela/Merge-åtgärder på shards som rör glidande klient rader mellan databaser. tooenable kan du bör skapa en ny SQL-användare (”superanvändare” i det här exemplet) i varje Fragmentera-databas. Sedan ändra hello säkerhetsprincipen med en ny predikat funktion som gör att den här användaren tooaccess alla rader:

```
-- New predicate function that adds superuser logic
CREATE FUNCTION rls.fn_tenantAccessPredicateWithSuperUser(@TenantId int)
    RETURNS TABLE
    WITH SCHEMABINDING
AS
    RETURN SELECT 1 AS fn_accessResult 
        WHERE 
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('dbo') -- note, should not be dbo!
            AND CAST(SESSION_CONTEXT(N'TenantId') AS int) = @TenantId
        ) 
        OR
        (
            DATABASE_PRINCIPAL_ID() = DATABASE_PRINCIPAL_ID('superuser')
        )
GO

-- Atomically swap in hello new predicate function on each table
ALTER SECURITY POLICY rls.tenantAccessPolicy
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Blogs,
    ALTER FILTER PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts,
    ALTER BLOCK PREDICATE rls.fn_tenantAccessPredicateWithSuperUser(TenantId) ON dbo.Posts
GO
```


### <a name="maintenance"></a>Underhåll
* **Lägga till nya shards**: du måste köra hello T-SQL-skript tooenable RLS på alla nya shards, annars frågor om dessa delar filtreras inte.
* **Lägga till nya tabeller**: du måste lägga till ett filter och blockera predikat toohello säkerhetsprincipen på alla delar när en ny tabell skapas, annars frågor med hello nya tabellen kommer inte att filtreras. Detta kan automatiseras med hjälp av en DDL-utlösare, enligt beskrivningen i [gäller säkerhet på radnivå toonewly skapas automatiskt tabeller (blogg)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).

## <a name="summary"></a>Sammanfattning
Elastisk Databasverktyg och säkerhet på radnivå kan vara används tillsammans tooscale ut datanivå för ett program med stöd för både flera innehavare och stöd för en innehavare delar. Flera innehavare shards kan vara används toostore data mer effektivt (särskilt i fall där ett stort antal klienter har bara ett fåtal rader med data) när en klient shards kan vara används toosupport premium klienter med striktare prestanda och isolering krav.  Mer information finns i [säkerhet på radnivå referens](https://msdn.microsoft.com/library/dn765131). 

## <a name="additional-resources"></a>Ytterligare resurser
* [Vad är en Azure elastisk pool?](sql-database-elastic-pool.md)
* [Skala ut med Azure SQL Database](sql-database-elastic-scale-introduction.md)
* [Designmönster för SaaS-program med flera klientorganisationer med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [Autentisering i appar med flera klienter, med hjälp av Azure AD och OpenID Connect](../guidance/guidance-multitenant-identity-authenticate.md)
* [Tailspin Surveys-programmet](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a>Frågor och Funktionsbegäranden
För frågor kan du nå toous på hello [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) och för funktionsbegäranden, Lägg till dem toohello [SQL-databas Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/).

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


