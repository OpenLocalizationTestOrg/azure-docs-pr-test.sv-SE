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
# <a name="multi-tenant-applications-with-elastic-database-tools-and-row-level-security"></a><span data-ttu-id="3217a-103">Program med flera klienter med elastiska Databasverktyg och säkerhet på radnivå</span><span class="sxs-lookup"><span data-stu-id="3217a-103">Multi-tenant applications with elastic database tools and row-level security</span></span>
<span data-ttu-id="3217a-104">[Elastisk Databasverktyg](sql-database-elastic-scale-get-started.md) och [på radnivå (RLS) för säkerhet](https://msdn.microsoft.com/library/dn765131) erbjuder en kraftfull uppsättning funktioner för att effektivt och flexibelt skala hello datanivå i ett program för flera innehavare med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="3217a-104">[Elastic database tools](sql-database-elastic-scale-get-started.md) and [row-level security (RLS)](https://msdn.microsoft.com/library/dn765131) offer a powerful set of capabilities for flexibly and efficiently scaling hello data tier of a multi-tenant application with Azure SQL Database.</span></span> <span data-ttu-id="3217a-105">Se [designmönster för flera innehavare SaaS-program med Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="3217a-105">See [Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database](sql-database-design-patterns-multi-tenancy-saas-applications.md) for more information.</span></span> 

<span data-ttu-id="3217a-106">Den här artikeln beskrivs hur toouse dessa tekniker tillsammans toobuild för ett program med en mycket skalbar datanivå som har stöd för flera innehavare shards, med hjälp av **ADO.NET SqlClient** och/eller **Entity Framework**.</span><span class="sxs-lookup"><span data-stu-id="3217a-106">This article illustrates how toouse these technologies together toobuild an application with a highly scalable data tier that supports multi-tenant shards, using **ADO.NET SqlClient** and/or **Entity Framework**.</span></span>  

* <span data-ttu-id="3217a-107">**Elastisk Databasverktyg** kan utvecklare tooscale ut hello datanivå för ett program via branschstandard horisontell partitionering metoder med en uppsättning .NET-bibliotek och mallar för Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="3217a-107">**Elastic database tools** enables developers tooscale out hello data tier of an application via industry-standard sharding practices using a set of .NET libraries and Azure service templates.</span></span> <span data-ttu-id="3217a-108">Hantera shards med hello klientbibliotek för elastisk databas kan automatisera och förenkla många hello infrastrukturella uppgifter som vanligtvis är kopplad till horisontell partitionering.</span><span class="sxs-lookup"><span data-stu-id="3217a-108">Managing shards with using hello Elastic Database Client Library helps automate and streamline many of hello infrastructural tasks typically associated with sharding.</span></span> 
* <span data-ttu-id="3217a-109">**Säkerhet för radnivå** gör att utvecklare toostore data för flera klienter i hello samma databasen med säkerhet principer toofilter ut rader som inte tillhör toohello klient kör en fråga.</span><span class="sxs-lookup"><span data-stu-id="3217a-109">**Row-level security** enables developers toostore data for multiple tenants in hello same database using security policies toofilter out rows that do not belong toohello tenant executing a query.</span></span> <span data-ttu-id="3217a-110">Centralisera åtkomst logik med RLS inuti hello databas, snarare växer än i hello program förenklar du Underhåll och minskar hello risken för fel som ett program-kodbas.</span><span class="sxs-lookup"><span data-stu-id="3217a-110">Centralizing access logic with RLS inside hello database, rather than in hello application, simplifies maintenance and reduces hello risk of error as an application’s codebase grows.</span></span> 

<span data-ttu-id="3217a-111">Med hjälp av dessa funktioner tillsammans, ett program kan dra nytta av kostnaden besparingar och effektivitet vinster genom att lagra data för flera innehavare i hello samma fragment-databasen.</span><span class="sxs-lookup"><span data-stu-id="3217a-111">Using these features together, an application can benefit from cost savings and efficiency gains by storing data for multiple tenants in hello same shard database.</span></span> <span data-ttu-id="3217a-112">På hello har samma gång, ett program som fortfarande hello flexibilitet toooffer isolerad enskild klient shards för ”premium-klienter som behöver garantier för strängare prestanda eftersom flera innehavare delar inte kan garantera lika resurs distributionen till klienter.</span><span class="sxs-lookup"><span data-stu-id="3217a-112">At hello same time, an application still has hello flexibility toooffer isolated, single-tenant shards for “premium” tenants who require stricter performance guarantees since multi-tenant shards do not guarantee equal resource distribution among tenants.</span></span>  

<span data-ttu-id="3217a-113">Kort sagt: hello elastisk databas-klientbiblioteket [data beroende routning](sql-database-elastic-scale-data-dependent-routing.md) API: er automatiskt ska ansluta klienter toohello rätt Fragmentera databas som innehåller sin horisontell partitionering nyckel (vanligtvis en ”TenantId”).</span><span class="sxs-lookup"><span data-stu-id="3217a-113">In short, hello elastic database client library’s [data dependent routing](sql-database-elastic-scale-data-dependent-routing.md) APIs automatically connect tenants toohello correct shard database containing their sharding key (generally a “TenantId”).</span></span> <span data-ttu-id="3217a-114">När du är ansluten, säkerställer en RLS säkerhetsprincip hello databasen att innehavare endast har åtkomst till rader som innehåller sina TenantId.</span><span class="sxs-lookup"><span data-stu-id="3217a-114">Once connected, an RLS security policy within hello database ensures that tenants can only access rows that contain their TenantId.</span></span> <span data-ttu-id="3217a-115">Det förutsätts att alla tabeller innehåller en TenantId kolumnen tooindicate vilka rader som tillhör tooeach klient.</span><span class="sxs-lookup"><span data-stu-id="3217a-115">It is assumed that all tables contain a TenantId column tooindicate which rows belong tooeach tenant.</span></span> 

![Bloggar app-arkitektur][1]

## <a name="download-hello-sample-project"></a><span data-ttu-id="3217a-117">Hämta hello exempelprojektet</span><span class="sxs-lookup"><span data-stu-id="3217a-117">Download hello sample project</span></span>
### <a name="prerequisites"></a><span data-ttu-id="3217a-118">Krav</span><span class="sxs-lookup"><span data-stu-id="3217a-118">Prerequisites</span></span>
* <span data-ttu-id="3217a-119">Använda Visual Studio (2012 eller senare)</span><span class="sxs-lookup"><span data-stu-id="3217a-119">Use Visual Studio (2012 or higher)</span></span> 
* <span data-ttu-id="3217a-120">Skapa tre Azure SQL-databaser</span><span class="sxs-lookup"><span data-stu-id="3217a-120">Create three Azure SQL Databases</span></span> 
* <span data-ttu-id="3217a-121">Hämta exempelprojektet: [elastisk DB-verktyg för Azure SQL - Shards för flera innehavare](http://go.microsoft.com/?linkid=9888163)</span><span class="sxs-lookup"><span data-stu-id="3217a-121">Download sample project: [Elastic DB Tools for Azure SQL - Multi-Tenant Shards](http://go.microsoft.com/?linkid=9888163)</span></span>
  * <span data-ttu-id="3217a-122">Fyll i hello information för dina databaser hello början av **Program.cs**</span><span class="sxs-lookup"><span data-stu-id="3217a-122">Fill in hello information for your databases at hello beginning of **Program.cs**</span></span> 

<span data-ttu-id="3217a-123">Det här projektet utökar hello en beskrivs i [elastisk DB-verktyg för Azure SQL - Entity Framework-integrering](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) genom att lägga till stöd för flera innehavare Fragmentera databaser.</span><span class="sxs-lookup"><span data-stu-id="3217a-123">This project extends hello one described in [Elastic DB Tools for Azure SQL - Entity Framework Integration](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md) by adding support for multi-tenant shard databases.</span></span> <span data-ttu-id="3217a-124">Den skapar ett enkelt konsolprogram för att skapa bloggar och inlägg, med fyra klienter och två databaser för flera innehavare Fragmentera enligt beskrivningen i hello ovan diagram.</span><span class="sxs-lookup"><span data-stu-id="3217a-124">It builds a simple console application for creating blogs and posts, with four tenants and two multi-tenant shard databases as illustrated in hello above diagram.</span></span> 

<span data-ttu-id="3217a-125">Skapa och köra programmet hello.</span><span class="sxs-lookup"><span data-stu-id="3217a-125">Build and run hello application.</span></span> <span data-ttu-id="3217a-126">Detta kommer bootstrap hello elastisk Databasverktyg Fragmentera kartan manager och kör hello följande test:</span><span class="sxs-lookup"><span data-stu-id="3217a-126">This will bootstrap hello elastic database tools’ shard map manager and run hello following tests:</span></span> 

1. <span data-ttu-id="3217a-127">Använder Entity Framework och LINQ, skapa en ny blogg och sedan visa alla bloggar för varje klient</span><span class="sxs-lookup"><span data-stu-id="3217a-127">Using Entity Framework and LINQ, create a new blog and then display all blogs for each tenant</span></span>
2. <span data-ttu-id="3217a-128">Med ADO.NET SqlClient kan visa alla bloggar för en klient</span><span class="sxs-lookup"><span data-stu-id="3217a-128">Using ADO.NET SqlClient, display all blogs for a tenant</span></span>
3. <span data-ttu-id="3217a-129">Försök tooinsert en blogg för hello fel klient tooverify som ett felmeddelande</span><span class="sxs-lookup"><span data-stu-id="3217a-129">Try tooinsert a blog for hello wrong tenant tooverify that an error is thrown</span></span>  

<span data-ttu-id="3217a-130">Observera att eftersom RLS inte ännu har aktiverats i hello Fragmentera databaser, var och en av de här testerna visar att ett problem: klienter är kan toosee bloggar som inte tillhör toothem och hello program inte hindras från att lägga till en blogg för hello fel klient.</span><span class="sxs-lookup"><span data-stu-id="3217a-130">Notice that because RLS has not yet been enabled in hello shard databases, each of these tests reveals a problem: tenants are able toosee blogs that do not belong toothem, and hello application is not prevented from inserting a blog for hello wrong tenant.</span></span> <span data-ttu-id="3217a-131">hello resten av den här artikeln beskriver hur tooresolve dessa problem genom att tillämpa klient isolering med RLS.</span><span class="sxs-lookup"><span data-stu-id="3217a-131">hello remainder of this article describes how tooresolve these problems by enforcing tenant isolation with RLS.</span></span> <span data-ttu-id="3217a-132">Det finns två steg:</span><span class="sxs-lookup"><span data-stu-id="3217a-132">There are two steps:</span></span> 

1. <span data-ttu-id="3217a-133">**Programmet nivå**: ändra hello programkod tooalways set hello aktuella TenantId i hello SESSION_CONTEXT när du har öppnat en anslutning.</span><span class="sxs-lookup"><span data-stu-id="3217a-133">**Application tier**: Modify hello application code tooalways set hello current TenantId in hello SESSION_CONTEXT after opening a connection.</span></span> <span data-ttu-id="3217a-134">hello exempelprojektet har gjort det redan.</span><span class="sxs-lookup"><span data-stu-id="3217a-134">hello sample project has already done this.</span></span> 
2. <span data-ttu-id="3217a-135">**Datanivå**: skapa en säkerhetsprincip för RLS i varje Fragmentera databasen toofilter rader med hello TenantId lagras i SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="3217a-135">**Data tier**: Create an RLS security policy in each shard database toofilter rows based on hello TenantId stored in SESSION_CONTEXT.</span></span> <span data-ttu-id="3217a-136">Du behöver toodo detta för varje Fragmentera databaserna, annars rader i flera delar filtreras inte.</span><span class="sxs-lookup"><span data-stu-id="3217a-136">You will need toodo this for each of your shard databases, otherwise rows in multi-tenant shards will not be filtered.</span></span> 

## <a name="step-1-application-tier-set-tenantid-in-hello-sessioncontext"></a><span data-ttu-id="3217a-137">Steg 1) programmet nivå: Ange TenantId i hello SESSION_CONTEXT</span><span class="sxs-lookup"><span data-stu-id="3217a-137">Step 1) Application tier: Set TenantId in hello SESSION_CONTEXT</span></span>
<span data-ttu-id="3217a-138">När du ansluter tooa Fragmentera databasen med hjälp av hello elastisk databas klienten bibliotekets data beroende routning API: er, hello programmet fortfarande måste tootell hello databasen använder vilka TenantId anslutningen så att en RLS säkerhetsprincip kan filtrera ut rader tillhör tooother innehavarna.</span><span class="sxs-lookup"><span data-stu-id="3217a-138">After connecting tooa shard database using hello elastic database client library’s data dependent routing APIs, hello application still needs tootell hello database which TenantId is using that connection so that an RLS security policy can filter out rows belonging tooother tenants.</span></span> <span data-ttu-id="3217a-139">Hej rekommenderade sättet toopass informationen är toostore hello aktuella TenantId för anslutningen i hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span><span class="sxs-lookup"><span data-stu-id="3217a-139">hello recommended way toopass this information is toostore hello current TenantId for that connection in hello [SESSION_CONTEXT](https://msdn.microsoft.com/library/mt590806.aspx).</span></span> <span data-ttu-id="3217a-140">(Obs: du kan också använda [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), men SESSION_CONTEXT är ett bättre alternativ eftersom det är enklare toouse returnerar NULL som standard och har stöd för nyckel-värdepar.)</span><span class="sxs-lookup"><span data-stu-id="3217a-140">(Note: You could alternatively use [CONTEXT_INFO](https://msdn.microsoft.com/library/ms180125.aspx), but SESSION_CONTEXT is a better option because it is easier toouse, returns NULL by default, and supports key-value pairs.)</span></span>

### <a name="entity-framework"></a><span data-ttu-id="3217a-141">Entity Framework</span><span class="sxs-lookup"><span data-stu-id="3217a-141">Entity Framework</span></span>
<span data-ttu-id="3217a-142">För program som använder Entity Framework, hello enklaste metoden är tooset hello SESSION_CONTEXT inom hello ElasticScaleContext åsidosätta beskrivs i [Data beroende routning med EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span><span class="sxs-lookup"><span data-stu-id="3217a-142">For applications using Entity Framework, hello easiest approach is tooset hello SESSION_CONTEXT within hello ElasticScaleContext override described in [Data Dependent Routing using EF DbContext](sql-database-elastic-scale-use-entity-framework-applications-visual-studio.md#data-dependent-routing-using-ef-dbcontext).</span></span> <span data-ttu-id="3217a-143">Innan det returneras hello anslutning asynkrona via data beroende routning, skapa och köra en SqlCommand som anger 'TenantId' i hello SESSION_CONTEXT toohello shardingKey som angetts för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="3217a-143">Before returning hello connection brokered through data dependent routing, simply create and execute a SqlCommand that sets 'TenantId' in hello SESSION_CONTEXT toohello shardingKey specified for that connection.</span></span> <span data-ttu-id="3217a-144">Det här sättet behöver du bara toowrite kod när tooset hello SESSION_CONTEXT.</span><span class="sxs-lookup"><span data-stu-id="3217a-144">This way, you only need toowrite code once tooset hello SESSION_CONTEXT.</span></span> 

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

<span data-ttu-id="3217a-145">Nu hello SESSION_CONTEXT anges automatiskt med angiven hello TenantId när ElasticScaleContext anropas:</span><span class="sxs-lookup"><span data-stu-id="3217a-145">Now hello SESSION_CONTEXT is automatically set with hello specified TenantId whenever ElasticScaleContext is invoked:</span></span> 

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

### <a name="adonet-sqlclient"></a><span data-ttu-id="3217a-146">ADO.NET SqlClient</span><span class="sxs-lookup"><span data-stu-id="3217a-146">ADO.NET SqlClient</span></span>
<span data-ttu-id="3217a-147">För program med hjälp av ADO.NET SqlClient hello rekommenderad metod är toocreate funktionen wrapper runt ShardMap.OpenConnectionForKey() som anger automatiskt 'TenantId' hello SESSION_CONTEXT toohello korrigera TenantId innan det returneras en anslutning.</span><span class="sxs-lookup"><span data-stu-id="3217a-147">For applications using ADO.NET SqlClient, hello recommended approach is toocreate a wrapper function around ShardMap.OpenConnectionForKey() that automatically sets 'TenantId' in hello SESSION_CONTEXT toohello correct TenantId before returning a connection.</span></span> <span data-ttu-id="3217a-148">tooensure som SESSION_CONTEXT alltid som du bör endast öppna anslutningar som använder den här wrapper-funktionen.</span><span class="sxs-lookup"><span data-stu-id="3217a-148">tooensure that SESSION_CONTEXT is always set, you should only open connections using this wrapper function.</span></span>

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

## <a name="step-2-data-tier-create-row-level-security-policy"></a><span data-ttu-id="3217a-149">Steg 2) Data tjänstnivån: skapa radnivå säkerhetsprincip</span><span class="sxs-lookup"><span data-stu-id="3217a-149">Step 2) Data tier: Create row-level security policy</span></span>
### <a name="create-a-security-policy-toofilter-hello-rows-each-tenant-can-access"></a><span data-ttu-id="3217a-150">Skapa en säkerhet princip toofilter hello rader som har åtkomst till varje klient</span><span class="sxs-lookup"><span data-stu-id="3217a-150">Create a security policy toofilter hello rows each tenant can access</span></span>
<span data-ttu-id="3217a-151">Nu när programmet hello anger SESSION_CONTEXT med hello aktuella TenantId innan du hämtar, en säkerhetsprincip för RLS filtrera frågor och utelämna rader som har en annan TenantId.</span><span class="sxs-lookup"><span data-stu-id="3217a-151">Now that hello application is setting SESSION_CONTEXT with hello current TenantId before querying, an RLS security policy can filter queries and exclude rows that have a different TenantId.</span></span>  

<span data-ttu-id="3217a-152">RLS har implementerats i T-SQL: en användardefinierad funktion definierar hello åtkomst logik och en säkerhetsprincip Binder den här funktionen tooany antal tabeller.</span><span class="sxs-lookup"><span data-stu-id="3217a-152">RLS is implemented in T-SQL: a user-defined function defines hello access logic, and a security policy binds this function tooany number of tables.</span></span> <span data-ttu-id="3217a-153">För det här projektet verifierar hello funktionen bara att hello program (i stället för en annan SQL-användare) är anslutet toohello databas och att hello 'TenantId' lagras i hello SESSION_CONTEXT matchar hello klient-ID för en viss rad.</span><span class="sxs-lookup"><span data-stu-id="3217a-153">For this project, hello function will simply verify that hello application (rather than some other SQL user) is connected toohello database, and that hello 'TenantId' stored in hello SESSION_CONTEXT matches hello TenantId of a given row.</span></span> <span data-ttu-id="3217a-154">Ett filterpredikat tillåter rader som uppfyller dessa villkor toopass genom hello filter för SELECT-, UPDATE- och DELETE-frågor. och ett block-predikat förhindras rader som strider mot dessa villkor från att infogad eller uppdaterad.</span><span class="sxs-lookup"><span data-stu-id="3217a-154">A filter predicate will allow rows that meet these conditions toopass through hello filter for SELECT, UPDATE, and DELETE queries; and a block predicate will prevent rows that violate these conditions from being INSERTed or UPDATEd.</span></span> <span data-ttu-id="3217a-155">Om SESSION_CONTEXT inte har angetts, returneras NULL och inga rader kommer att vara synlig eller kan toobe infogas.</span><span class="sxs-lookup"><span data-stu-id="3217a-155">If SESSION_CONTEXT has not been set, it will return NULL and no rows will be visible or able toobe inserted.</span></span> 

<span data-ttu-id="3217a-156">tooenable RLS, kör följande T-SQL på alla delar med hjälp av antingen Visual Studio (SSDT), SSMS, hello eller hello PowerShell-skript som ingår i projektet hello (eller om du använder [elastiska databasen jobb](sql-database-elastic-jobs-overview.md), kan du använda det tooautomate körning i den här T-SQL på alla shards):</span><span class="sxs-lookup"><span data-stu-id="3217a-156">tooenable RLS, execute hello following T-SQL on all shards using either Visual Studio (SSDT), SSMS, or hello PowerShell script included in hello project (or if you are using [Elastic Database Jobs](sql-database-elastic-jobs-overview.md), you can use it tooautomate execution of this T-SQL on all shards):</span></span> 

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
> <span data-ttu-id="3217a-157">Du kan använda en helper lagrade procedur som automatiskt genererar en säkerhetsprincip för att lägga till ett predikat för alla tabeller i ett schema för mer komplexa projekt som kräver en tooadd hello predikat på hundratals olika tabeller.</span><span class="sxs-lookup"><span data-stu-id="3217a-157">For more complex projects that need tooadd hello predicate on hundreds of tables, you can use a helper stored procedure that automatically generates a security policy adding a predicate on all tables in a schema.</span></span> <span data-ttu-id="3217a-158">Se [gäller säkerhet på radnivå tooall tabeller - helper-skript (blogg)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span><span class="sxs-lookup"><span data-stu-id="3217a-158">See [Apply Row-Level Security tooall tables - helper script (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/03/31/apply-row-level-security-to-all-tables-helper-script).</span></span>  
> 
> 

<span data-ttu-id="3217a-159">Nu om du kör hello exempelprogrammet igen ska klienter kunna toosee endast rader som tillhör toothem.</span><span class="sxs-lookup"><span data-stu-id="3217a-159">Now if you run hello sample application again, tenants will able toosee only rows that belong toothem.</span></span> <span data-ttu-id="3217a-160">Dessutom hello programmet går inte att infoga rader som tillhör tootenants än hello en anslutna toohello Fragmentera databas och det går inte att uppdatera synliga rader toohave olika TenantId.</span><span class="sxs-lookup"><span data-stu-id="3217a-160">In addition, hello application cannot insert rows that belong tootenants other than hello one currently connected toohello shard database, and it cannot update visible rows toohave a different TenantId.</span></span> <span data-ttu-id="3217a-161">Om toodo försöker antingen hello program, aktiveras en DbUpdateException.</span><span class="sxs-lookup"><span data-stu-id="3217a-161">If hello application attempts toodo either, a DbUpdateException will be raised.</span></span>

<span data-ttu-id="3217a-162">Om du lägger till en ny tabell senare bara ALTER hello säkerhetsprincip och Lägg till filter och block-predikat på hello ny tabell:</span><span class="sxs-lookup"><span data-stu-id="3217a-162">If you add a new table later on, simply ALTER hello security policy and add filter and block predicates on hello new table:</span></span> 

```
ALTER SECURITY POLICY rls.tenantAccessPolicy     
    ADD FILTER PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable,
    ADD BLOCK PREDICATE rls.fn_tenantAccessPredicate(TenantId) ON dbo.MyNewTable
GO 
```

### <a name="add-default-constraints-tooautomatically-populate-tenantid-for-inserts"></a><span data-ttu-id="3217a-163">Lägg till standard begränsningar tooautomatically fylla TenantId för infogningar</span><span class="sxs-lookup"><span data-stu-id="3217a-163">Add default constraints tooautomatically populate TenantId for INSERTs</span></span>
<span data-ttu-id="3217a-164">Du kan placera en standard-begränsning för varje tabell tooautomatically fylla hello TenantId med hello värdet som för närvarande lagras i SESSION_CONTEXT vid infogning av rader.</span><span class="sxs-lookup"><span data-stu-id="3217a-164">You can put a default constraint on each table tooautomatically populate hello TenantId with hello value currently stored in SESSION_CONTEXT when inserting rows.</span></span> <span data-ttu-id="3217a-165">Exempel:</span><span class="sxs-lookup"><span data-stu-id="3217a-165">For example:</span></span> 

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

<span data-ttu-id="3217a-166">Hello program behöver nu inte toospecify en TenantId när du infogar rader:</span><span class="sxs-lookup"><span data-stu-id="3217a-166">Now hello application does not need toospecify a TenantId when inserting rows:</span></span> 

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
> <span data-ttu-id="3217a-167">Om du använder standardbegränsningar för en Entity Framework-projekt, rekommenderas att du inte inkluderar hello TenantId kolumnen i datamodellen EF.</span><span class="sxs-lookup"><span data-stu-id="3217a-167">If you use default constraints for an Entity Framework project, it is recommended that you do NOT include hello TenantId column in your EF data model.</span></span> <span data-ttu-id="3217a-168">Det beror på att automatiskt ange standardvärden som åsidosätter hello standardbegränsningar skapas i T-SQL som använder SESSION_CONTEXT i Entity Framework-frågor.</span><span class="sxs-lookup"><span data-stu-id="3217a-168">This is because Entity Framework queries automatically supply default values which will override hello default constraints created in T-SQL that use SESSION_CONTEXT.</span></span> <span data-ttu-id="3217a-169">toouse standardbegränsningar i hello exempel projekt, till exempel bör du ta bort TenantId från DataClasses.cs (och kör Add-migrering i hello Package Manager-konsolen) och Använd T-SQL-tooensure som hello fältet finns bara i hello databastabeller.</span><span class="sxs-lookup"><span data-stu-id="3217a-169">toouse default constraints in hello sample project, for instance, you should remove TenantId from DataClasses.cs (and run Add-Migration in hello Package Manager Console) and use T-SQL tooensure that hello field only exists in hello database tables.</span></span> <span data-ttu-id="3217a-170">Det här sättet EF kommer inte automatiskt anger felaktiga standardvärden vid infogning av data.</span><span class="sxs-lookup"><span data-stu-id="3217a-170">This way, EF will not automatically supply incorrect default values when inserting data.</span></span> 
> 
> 

### <a name="optional-enable-a-superuser-tooaccess-all-rows"></a><span data-ttu-id="3217a-171">(Valfritt) Aktivera en ”superanvändare” tooaccess alla rader</span><span class="sxs-lookup"><span data-stu-id="3217a-171">(Optional) Enable a "superuser" tooaccess all rows</span></span>
<span data-ttu-id="3217a-172">Vissa program kanske toocreate ”superanvändare” som har åtkomst till alla rader, till exempel i ordning tooenable rapportering över alla klienter i alla delar eller tooperform dela/Merge-åtgärder på shards som rör glidande klient rader mellan databaser.</span><span class="sxs-lookup"><span data-stu-id="3217a-172">Some applications may want toocreate a "superuser" who can access all rows, for instance, in order tooenable reporting across all tenants on all shards, or tooperform Split/Merge operations on shards that involve moving tenant rows between databases.</span></span> <span data-ttu-id="3217a-173">tooenable kan du bör skapa en ny SQL-användare (”superanvändare” i det här exemplet) i varje Fragmentera-databas.</span><span class="sxs-lookup"><span data-stu-id="3217a-173">tooenable this, you should create a new SQL user ("superuser" in this example) in each shard database.</span></span> <span data-ttu-id="3217a-174">Sedan ändra hello säkerhetsprincipen med en ny predikat funktion som gör att den här användaren tooaccess alla rader:</span><span class="sxs-lookup"><span data-stu-id="3217a-174">Then alter hello security policy with a new predicate function that allows this user tooaccess all rows:</span></span>

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


### <a name="maintenance"></a><span data-ttu-id="3217a-175">Underhåll</span><span class="sxs-lookup"><span data-stu-id="3217a-175">Maintenance</span></span>
* <span data-ttu-id="3217a-176">**Lägga till nya shards**: du måste köra hello T-SQL-skript tooenable RLS på alla nya shards, annars frågor om dessa delar filtreras inte.</span><span class="sxs-lookup"><span data-stu-id="3217a-176">**Adding new shards**: You must execute hello T-SQL script tooenable RLS on any new shards, otherwise queries on these shards will not be filtered.</span></span>
* <span data-ttu-id="3217a-177">**Lägga till nya tabeller**: du måste lägga till ett filter och blockera predikat toohello säkerhetsprincipen på alla delar när en ny tabell skapas, annars frågor med hello nya tabellen kommer inte att filtreras.</span><span class="sxs-lookup"><span data-stu-id="3217a-177">**Adding new tables**: You must add a filter and block predicate toohello security policy on all shards whenever a new table is created, otherwise queries on hello new table will not be filtered.</span></span> <span data-ttu-id="3217a-178">Detta kan automatiseras med hjälp av en DDL-utlösare, enligt beskrivningen i [gäller säkerhet på radnivå toonewly skapas automatiskt tabeller (blogg)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span><span class="sxs-lookup"><span data-stu-id="3217a-178">This can be automated using a DDL trigger, as described in [Apply Row-Level Security automatically toonewly created tables (blog)](http://blogs.msdn.com/b/sqlsecurity/archive/2015/05/22/apply-row-level-security-automatically-to-newly-created-tables.aspx).</span></span>

## <a name="summary"></a><span data-ttu-id="3217a-179">Sammanfattning</span><span class="sxs-lookup"><span data-stu-id="3217a-179">Summary</span></span>
<span data-ttu-id="3217a-180">Elastisk Databasverktyg och säkerhet på radnivå kan vara används tillsammans tooscale ut datanivå för ett program med stöd för både flera innehavare och stöd för en innehavare delar.</span><span class="sxs-lookup"><span data-stu-id="3217a-180">Elastic database tools and row-level security can be used together tooscale out an application’s data tier with support for both multi-tenant and single-tenant shards.</span></span> <span data-ttu-id="3217a-181">Flera innehavare shards kan vara används toostore data mer effektivt (särskilt i fall där ett stort antal klienter har bara ett fåtal rader med data) när en klient shards kan vara används toosupport premium klienter med striktare prestanda och isolering krav.</span><span class="sxs-lookup"><span data-stu-id="3217a-181">Multi-tenant shards can be used toostore data more efficiently (particularly in cases where a large number of tenants have only a few rows of data), while single-tenant shards can be used toosupport premium tenants with stricter performance and isolation requirements.</span></span>  <span data-ttu-id="3217a-182">Mer information finns i [säkerhet på radnivå referens](https://msdn.microsoft.com/library/dn765131).</span><span class="sxs-lookup"><span data-stu-id="3217a-182">For more information, see [Row-Level Security reference](https://msdn.microsoft.com/library/dn765131).</span></span> 

## <a name="additional-resources"></a><span data-ttu-id="3217a-183">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="3217a-183">Additional resources</span></span>
* [<span data-ttu-id="3217a-184">Vad är en Azure elastisk pool?</span><span class="sxs-lookup"><span data-stu-id="3217a-184">What is an Azure elastic pool?</span></span>](sql-database-elastic-pool.md)
* [<span data-ttu-id="3217a-185">Skala ut med Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3217a-185">Scaling out with Azure SQL Database</span></span>](sql-database-elastic-scale-introduction.md)
* [<span data-ttu-id="3217a-186">Designmönster för SaaS-program med flera klientorganisationer med Azure SQL Database</span><span class="sxs-lookup"><span data-stu-id="3217a-186">Design Patterns for Multi-tenant SaaS Applications with Azure SQL Database</span></span>](sql-database-design-patterns-multi-tenancy-saas-applications.md)
* [<span data-ttu-id="3217a-187">Autentisering i appar med flera klienter, med hjälp av Azure AD och OpenID Connect</span><span class="sxs-lookup"><span data-stu-id="3217a-187">Authentication in multitenant apps, using Azure AD and OpenID Connect</span></span>](../guidance/guidance-multitenant-identity-authenticate.md)
* [<span data-ttu-id="3217a-188">Tailspin Surveys-programmet</span><span class="sxs-lookup"><span data-stu-id="3217a-188">Tailspin Surveys application</span></span>](../guidance/guidance-multitenant-identity-tailspin.md)

## <a name="questions-and-feature-requests"></a><span data-ttu-id="3217a-189">Frågor och Funktionsbegäranden</span><span class="sxs-lookup"><span data-stu-id="3217a-189">Questions and Feature Requests</span></span>
<span data-ttu-id="3217a-190">För frågor kan du nå toous på hello [SQL Database-forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) och för funktionsbegäranden, Lägg till dem toohello [SQL-databas Feedbackforum](https://feedback.azure.com/forums/217321-sql-database/).</span><span class="sxs-lookup"><span data-stu-id="3217a-190">For questions, please reach out toous on hello [SQL Database forum](http://social.msdn.microsoft.com/forums/azure/home?forum=ssdsgetstarted) and for feature requests, please add them toohello [SQL Database feedback forum](https://feedback.azure.com/forums/217321-sql-database/).</span></span>

<!--Image references-->
[1]: ./media/sql-database-elastic-tools-multi-tenant-row-level-security/blogging-app.png
<!--anchors-->


