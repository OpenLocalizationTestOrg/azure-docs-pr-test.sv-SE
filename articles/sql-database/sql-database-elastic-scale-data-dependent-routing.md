---
title: aaaData beroende routning med Azure SQL Database | Microsoft Docs
description: "Hur toouse hello ShardMapManager klassen i .NET-appar för data-beroende routning, en funktion i delat databaser i Azure SQL Database"
services: sql-database
documentationcenter: 
manager: jhubbard
author: torsteng
editor: 
ms.assetid: cad09e15-5561-4448-aa18-b38f54cda004
ms.service: sql-database
ms.custom: scale out apps
ms.workload: sql-database
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/27/2017
ms.author: ddove
ms.openlocfilehash: 34014508ae01905686791fe096bb275cb84f53b4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="data-dependent-routing"></a><span data-ttu-id="35cfe-103">Databeroende routning</span><span class="sxs-lookup"><span data-stu-id="35cfe-103">Data dependent routing</span></span>
<span data-ttu-id="35cfe-104">**Data beroende routning** är hello möjlighet toouse hello data i en fråga tooroute hello begäran tooan lämplig databas.</span><span class="sxs-lookup"><span data-stu-id="35cfe-104">**Data dependent routing** is hello ability toouse hello data in a query tooroute hello request tooan appropriate database.</span></span> <span data-ttu-id="35cfe-105">Detta är ett grundläggande mönster när du arbetar med delat databaser.</span><span class="sxs-lookup"><span data-stu-id="35cfe-105">This is a fundamental pattern when working with sharded databases.</span></span> <span data-ttu-id="35cfe-106">hello begärandekontexten kan också vara används tooroute hello begäran, särskilt om hello horisontell partitionering nyckeln inte är en del av hello fråga.</span><span class="sxs-lookup"><span data-stu-id="35cfe-106">hello request context may also be used tooroute hello request, especially if hello sharding key is not part of hello query.</span></span> <span data-ttu-id="35cfe-107">Varje specifik fråga eller en transaktion i ett program som använder data beroende routning är begränsad tooaccessing en enskild databas per begäran.</span><span class="sxs-lookup"><span data-stu-id="35cfe-107">Each specific query or transaction in an application using data dependent routing is restricted tooaccessing a single database per request.</span></span> <span data-ttu-id="35cfe-108">För hello Azure SQL Database elastisk verktyg, den här routning sker med hello  **[ShardMapManager klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  i ADO.NET-program.</span><span class="sxs-lookup"><span data-stu-id="35cfe-108">For hello Azure SQL Database Elastic tools, this routing is accomplished with hello **[ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)** in ADO.NET applications.</span></span>

<span data-ttu-id="35cfe-109">hello program behöver inte tootrack olika anslutningssträngar eller DB platser som är associerade med olika datasegment i hello delat miljö.</span><span class="sxs-lookup"><span data-stu-id="35cfe-109">hello application does not need tootrack various connection strings or DB locations associated with different slices of data in hello sharded environment.</span></span> <span data-ttu-id="35cfe-110">I stället hello [Fragmentera kartan Manager](sql-database-elastic-scale-shard-map-management.md) öppnas anslutningar toohello rätt databaser vid behov, baserat på hello data i hello Fragmentera kartan och hello värdet för hello horisontell partitionering nyckel som är hello mål för hello programbegäran.</span><span class="sxs-lookup"><span data-stu-id="35cfe-110">Instead, hello [Shard Map Manager](sql-database-elastic-scale-shard-map-management.md) opens connections toohello correct databases when needed, based on hello data in hello shard map and hello value of hello sharding key that is hello target of hello application’s request.</span></span> <span data-ttu-id="35cfe-111">hello nyckeln är vanligtvis hello *customer_id*, *tenant_id*, *date_key*, eller vissa specifika identifierare som är en grundläggande parameter för begäran om hello databas).</span><span class="sxs-lookup"><span data-stu-id="35cfe-111">hello key is typically hello *customer_id*, *tenant_id*, *date_key*, or some other specific identifier that is a fundamental parameter of hello database request).</span></span> 

<span data-ttu-id="35cfe-112">Mer information finns i [skala ut SQL Server med Data beroende routning](https://technet.microsoft.com/library/cc966448.aspx).</span><span class="sxs-lookup"><span data-stu-id="35cfe-112">For more information, see [Scaling Out SQL Server with Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx).</span></span>

## <a name="download-hello-client-library"></a><span data-ttu-id="35cfe-113">Hämta hello-klientbibliotek</span><span class="sxs-lookup"><span data-stu-id="35cfe-113">Download hello client library</span></span>
<span data-ttu-id="35cfe-114">tooget hello klass, installera hello [klientbibliotek för elastisk databas](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="35cfe-114">tooget hello class, install hello [Elastic Database Client Library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span> 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a><span data-ttu-id="35cfe-115">Med hjälp av en ShardMapManager i ett beroende routning program</span><span class="sxs-lookup"><span data-stu-id="35cfe-115">Using a ShardMapManager in a data dependent routing application</span></span>
<span data-ttu-id="35cfe-116">Program ska initiera hello **ShardMapManager** under initieringen med hello factory anropet  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="35cfe-116">Applications should instantiate hello **ShardMapManager** during initialization, using hello factory call **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span></span> <span data-ttu-id="35cfe-117">I detta exempel både en **ShardMapManager** och en specifik **ShardMap** som den innehåller har initierats.</span><span class="sxs-lookup"><span data-stu-id="35cfe-117">In this example, both a **ShardMapManager** and a specific **ShardMap** that it contains are initialized.</span></span> <span data-ttu-id="35cfe-118">Det här exemplet visar hello GetSqlShardMapManager och [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metoder.</span><span class="sxs-lookup"><span data-stu-id="35cfe-118">This example shows hello GetSqlShardMapManager and [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) methods.</span></span>

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-hello-shard-map"></a><span data-ttu-id="35cfe-119">Använd lägsta privilegium autentiseringsuppgifter som är möjliga för att hämta hello Fragmentera karta</span><span class="sxs-lookup"><span data-stu-id="35cfe-119">Use lowest privilege credentials possible for getting hello shard map</span></span>
<span data-ttu-id="35cfe-120">Om ett program inte manipulering hello Fragmentera kartan själva, hello autentiseringsuppgifter i hello fabriksmetoden bör ha bara läsbehörighet på hello **globala Fragmentera kartan** databas.</span><span class="sxs-lookup"><span data-stu-id="35cfe-120">If an application is not manipulating hello shard map itself, hello credentials used in hello factory method should have just read-only permissions on hello **Global Shard Map** database.</span></span> <span data-ttu-id="35cfe-121">Autentiseringsuppgifterna är vanligtvis skiljer sig från autentiseringsuppgifterna tooopen anslutningar toohello Fragmentera kartan manager.</span><span class="sxs-lookup"><span data-stu-id="35cfe-121">These credentials are typically different from credentials used tooopen connections toohello shard map manager.</span></span> <span data-ttu-id="35cfe-122">Se även [autentiseringsuppgifter används tooaccess hello-klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="35cfe-122">See also [Credentials used tooaccess hello Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span> 

## <a name="call-hello-openconnectionforkey-method"></a><span data-ttu-id="35cfe-123">Anropa hello OpenConnectionForKey metod</span><span class="sxs-lookup"><span data-stu-id="35cfe-123">Call hello OpenConnectionForKey method</span></span>
<span data-ttu-id="35cfe-124">Hej  **[ShardMap.OpenConnectionForKey metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  returnerar en ADO.Net-anslutning som är redo för att utfärda kommandon toohello lämplig databas baserat på hello värdet för hello **nyckeln**parameter.</span><span class="sxs-lookup"><span data-stu-id="35cfe-124">hello **[ShardMap.OpenConnectionForKey method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)** returns an ADO.Net connection ready for issuing commands toohello appropriate database based on hello value of hello **key** parameter.</span></span> <span data-ttu-id="35cfe-125">Fragmentera informationen cachelagras i hello programmet hello **ShardMapManager**, så dessa begäranden inte normalt innefattar en databassökning mot hello **globala Fragmentera kartan** databas.</span><span class="sxs-lookup"><span data-stu-id="35cfe-125">Shard information is cached in hello application by hello **ShardMapManager**, so these requests do not typically involve a database lookup against hello **Global Shard Map** database.</span></span> 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* <span data-ttu-id="35cfe-126">Hej **nyckeln** parametern används som en nyckel för sökning i hello Fragmentera kartan toodetermine hello lämplig databas för hello-begäran.</span><span class="sxs-lookup"><span data-stu-id="35cfe-126">hello **key** parameter is used as a lookup key into hello shard map toodetermine hello appropriate database for hello request.</span></span> 
* <span data-ttu-id="35cfe-127">Hej **connectionString** är används toopass endast hello användarautentiseringsuppgifter för hello önskad anslutning.</span><span class="sxs-lookup"><span data-stu-id="35cfe-127">hello **connectionString** is used toopass only hello user credentials for hello desired connection.</span></span> <span data-ttu-id="35cfe-128">Inget databasnamn eller servernamn ingår i detta *connectionString* eftersom hello metoden avgör hello databasen och servern med hjälp av hello **ShardMap**.</span><span class="sxs-lookup"><span data-stu-id="35cfe-128">No database name or server name are included in this *connectionString* since hello method will determine hello database and server using hello **ShardMap**.</span></span> 
* <span data-ttu-id="35cfe-129">Hej  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  ska anges för**ConnectionOptions.Validate** om en miljö där Fragmentera mappar kan ändras och rader kan flytta tooother databaser som en resultatet av delade eller merge-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="35cfe-129">hello **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)** should be set too**ConnectionOptions.Validate** if an environment where shard maps may change and rows may move tooother databases as a result of split or merge operations.</span></span> <span data-ttu-id="35cfe-130">Detta innebär en kortfattad fråga toohello lokala Fragmentera karta på hello måldatabasen (inte toohello globala Fragmentera mappar) innan hello anslutning levereras toohello program.</span><span class="sxs-lookup"><span data-stu-id="35cfe-130">This involves a brief query toohello local shard map on hello target database (not toohello global shard map) before hello connection is delivered toohello application.</span></span> 

<span data-ttu-id="35cfe-131">Om hello validering mot hello lokala Fragmentera mappningen misslyckas (som anger att hello cache är felaktig) hello Fragmentera kartan Manager att söka hello globala Fragmentera kartan tooobtain hello nya rätt värde för hello-sökning, hello cachen uppdateras och hämta och returnera hello lämplig databasanslutning.</span><span class="sxs-lookup"><span data-stu-id="35cfe-131">If hello validation against hello local shard map fails (indicating that hello cache is incorrect), hello Shard Map Manager will query hello global shard map tooobtain hello new correct value for hello lookup, update hello cache, and obtain and return hello appropriate database connection.</span></span> 

<span data-ttu-id="35cfe-132">Använd **ConnectionOptions.None** endast när Fragmentera mappningsändringar inte förväntas när ett program är online.</span><span class="sxs-lookup"><span data-stu-id="35cfe-132">Use **ConnectionOptions.None** only when shard mapping changes are not expected while an application is online.</span></span> <span data-ttu-id="35cfe-133">I så fall kan hello cachelagrade värden antas tooalways vara korrekt, och hello extra fram och åter validering anropet toohello måldatabasen kan hoppas på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="35cfe-133">In that case, hello cached values can be assumed tooalways be correct, and hello extra round-trip validation call toohello target database can be safely skipped.</span></span> <span data-ttu-id="35cfe-134">Som minskar databastrafik.</span><span class="sxs-lookup"><span data-stu-id="35cfe-134">That reduces database traffic.</span></span> <span data-ttu-id="35cfe-135">Hej **connectionOptions** kan också anges via ett värde i en konfiguration filen tooindicate om ändringar för horisontell partitionering är förväntat eller inte under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="35cfe-135">hello **connectionOptions** may also be set via a value in a configuration file tooindicate whether sharding changes are expected or not during a period of time.</span></span>  

<span data-ttu-id="35cfe-136">Det här exemplet används hello-värdet för ett integer-nyckeln **CustomerID**med hjälp av en **ShardMap** objekt med namnet **customerShardMap**.</span><span class="sxs-lookup"><span data-stu-id="35cfe-136">This example uses hello value of an integer key **CustomerID**, using a **ShardMap** object named **customerShardMap**.</span></span>  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect toohello shard for that customer ID. No need toocall a SqlConnection 
    // constructor followed by hello Open method.
    using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId, 
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
    { 
        // Execute a simple command. 
        SqlCommand cmd = conn.CreateCommand(); 
        cmd.CommandText = @"UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID"; 

        cmd.Parameters.AddWithValue("@customerID", customerId); 
        cmd.Parameters.AddWithValue("@newPersonID", newPersonId); 
        cmd.ExecuteNonQuery(); 
    }  

<span data-ttu-id="35cfe-137">Hej **OpenConnectionForKey** metoden returnerar en ny redan öppen anslutning toohello rätt databas.</span><span class="sxs-lookup"><span data-stu-id="35cfe-137">hello **OpenConnectionForKey** method returns a new already-open connection toohello correct database.</span></span> <span data-ttu-id="35cfe-138">Anslutningar som används i det här sättet fortfarande dra full nytta av ADO.Net anslutningspooler.</span><span class="sxs-lookup"><span data-stu-id="35cfe-138">Connections utilized in this way still take full advantage of ADO.Net connection pooling.</span></span> <span data-ttu-id="35cfe-139">Så länge transaktioner och begäranden kan betjänas av en Fragmentera samtidigt, bör detta vara hello endast ändringar som krävs i ett program som redan använder ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="35cfe-139">As long as transactions and requests can be satisfied by one shard at a time, this should be hello only modification necessary in an application already using ADO.Net.</span></span> 

<span data-ttu-id="35cfe-140">Hej  **[OpenConnectionForKeyAsync metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  finns även om programmet gör Använd asynkron programmering med ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="35cfe-140">hello **[OpenConnectionForKeyAsync method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)** is also available if your application makes use asynchronous programming with ADO.Net.</span></span> <span data-ttu-id="35cfe-141">Sitt beteende är hello data beroende routning motsvarar ADO. Net's  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metod.</span><span class="sxs-lookup"><span data-stu-id="35cfe-141">Its behavior is hello data dependent routing equivalent of ADO.Net's **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)** method.</span></span>

## <a name="integrating-with-transient-fault-handling"></a><span data-ttu-id="35cfe-142">Integrera med tillfälligt fel hantering</span><span class="sxs-lookup"><span data-stu-id="35cfe-142">Integrating with transient fault handling</span></span>
<span data-ttu-id="35cfe-143">Bästa praxis för att utveckla program i molnet hello åtkomst är tooensure som fångas av hello app tillfälliga problem och som hello operations har gjorts flera gånger innan ge ett fel.</span><span class="sxs-lookup"><span data-stu-id="35cfe-143">A best practice in developing data access applications in hello cloud is tooensure that transient faults are caught by hello app, and that hello operations are retried several times before throwing an error.</span></span> <span data-ttu-id="35cfe-144">Tillfälligt fel hantering för molnprogram beskrivs på [hantering av tillfälliga fel](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span><span class="sxs-lookup"><span data-stu-id="35cfe-144">Transient fault handling for cloud applications is discussed at [Transient Fault Handling](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span></span> 

<span data-ttu-id="35cfe-145">Tillfälligt fel hantering kan samexistera naturligt med hello Data beroende routning mönster.</span><span class="sxs-lookup"><span data-stu-id="35cfe-145">Transient fault handling can coexist naturally with hello Data Dependent Routing pattern.</span></span> <span data-ttu-id="35cfe-146">hello krav är tooretry hello hela data åtkomstbegäran inklusive hello **med** block som erhålls hello data beroende routning anslutning.</span><span class="sxs-lookup"><span data-stu-id="35cfe-146">hello key requirement is tooretry hello entire data access request including hello **using** block that obtained hello data-dependent routing connection.</span></span> <span data-ttu-id="35cfe-147">hello-exemplet ovan kan skrivas på följande sätt (Observera markerade ändring).</span><span class="sxs-lookup"><span data-stu-id="35cfe-147">hello example above could be rewritten as follows (note highlighted change).</span></span> 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a><span data-ttu-id="35cfe-148">Exempel – data beroende routning med tillfälligt fel hantering</span><span class="sxs-lookup"><span data-stu-id="35cfe-148">Example - data dependent routing with transient fault handling</span></span>
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect toohello shard for a customer ID. 
        using (SqlConnection conn = customerShardMap.OpenConnectionForKey(customerId,  
        Configuration.GetCredentialsConnectionString(), ConnectionOptions.Validate)) 
        { 
            // Execute a simple command 
            SqlCommand cmd = conn.CreateCommand(); 

            cmd.CommandText = @&quot;UPDATE Sales.Customer 
                            SET PersonID = @newPersonID 
                            WHERE CustomerID = @customerID&quot;; 

            cmd.Parameters.AddWithValue(&quot;@customerID&quot;, customerId); 
            cmd.Parameters.AddWithValue(&quot;@newPersonID&quot;, newPersonId); 
            cmd.ExecuteNonQuery(); 

            Console.WriteLine(&quot;Update completed&quot;); 
        } 
<span style="background-color:  #FFFF00">    }); </span> 
</code></pre>


<span data-ttu-id="35cfe-149">Paket som är nödvändiga tooimplement tillfälligt fel hantering laddas ned automatiskt när du skapar hello elastisk databas exempelprogrammet.</span><span class="sxs-lookup"><span data-stu-id="35cfe-149">Packages necessary tooimplement transient fault handling are downloaded automatically when you build hello elastic database sample application.</span></span> <span data-ttu-id="35cfe-150">Paket finns också separat på [Enterprise Library - tillfälliga fel hanterar programmet Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span><span class="sxs-lookup"><span data-stu-id="35cfe-150">Packages are also available separately at [Enterprise Library - Transient Fault Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span></span> <span data-ttu-id="35cfe-151">Använd version 6.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="35cfe-151">Use version 6.0 or later.</span></span> 

## <a name="transactional-consistency"></a><span data-ttu-id="35cfe-152">Transaktionskonsekvens</span><span class="sxs-lookup"><span data-stu-id="35cfe-152">Transactional consistency</span></span>
<span data-ttu-id="35cfe-153">Transaktionell egenskaper är garanterat för alla åtgärder lokala tooa Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="35cfe-153">Transactional properties are guaranteed for all operations local tooa shard.</span></span> <span data-ttu-id="35cfe-154">Till exempel köra transaktioner som har skickats via data beroende routning inom hello omfång hello mål Fragmentera för hello-anslutning.</span><span class="sxs-lookup"><span data-stu-id="35cfe-154">For example, transactions submitted through data-dependent routing execute within hello scope of hello target shard for hello connection.</span></span> <span data-ttu-id="35cfe-155">För tillfället finns inga funktioner för att ta med flera anslutningar i en transaktion och det finns därför inga transaktionella garantier för åtgärder som utförs i shards.</span><span class="sxs-lookup"><span data-stu-id="35cfe-155">At this time, there are no capabilities provided for enlisting multiple connections into a transaction, and therefore there are no transactional guarantees for operations performed across shards.</span></span>

## <a name="next-steps"></a><span data-ttu-id="35cfe-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="35cfe-156">Next steps</span></span>
<span data-ttu-id="35cfe-157">toodetach en Fragmentera eller tooreattach Fragmentera, se [med hello RecoveryManager klassen toofix Fragmentera kartan problem](sql-database-elastic-database-recovery-manager.md)</span><span class="sxs-lookup"><span data-stu-id="35cfe-157">toodetach a shard, or tooreattach a shard, see [Using hello RecoveryManager class toofix shard map problems](sql-database-elastic-database-recovery-manager.md)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

