---
title: Data beroende routning med Azure SQL Database | Microsoft Docs
description: "Hur du använder klassen ShardMapManager i .NET-appar för data-beroende routning, en funktion i delat databaser i Azure SQL Database"
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
ms.openlocfilehash: 6b68bbb0133afd1493acdb58f79f3eeaf6a8d7cd
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="data-dependent-routing"></a><span data-ttu-id="03a2b-103">Databeroende routning</span><span class="sxs-lookup"><span data-stu-id="03a2b-103">Data dependent routing</span></span>
<span data-ttu-id="03a2b-104">**Data beroende routning** är möjligheten att använda data i en fråga för att vidarebefordra begäran till en lämplig databas.</span><span class="sxs-lookup"><span data-stu-id="03a2b-104">**Data dependent routing** is the ability to use the data in a query to route the request to an appropriate database.</span></span> <span data-ttu-id="03a2b-105">Detta är ett grundläggande mönster när du arbetar med delat databaser.</span><span class="sxs-lookup"><span data-stu-id="03a2b-105">This is a fundamental pattern when working with sharded databases.</span></span> <span data-ttu-id="03a2b-106">Kontexten för begäran kan även användas för routning av begäran, särskilt om nyckeln för horisontell partitionering inte är en del av frågan.</span><span class="sxs-lookup"><span data-stu-id="03a2b-106">The request context may also be used to route the request, especially if the sharding key is not part of the query.</span></span> <span data-ttu-id="03a2b-107">Varje specifik fråga eller en transaktion i ett program som använder data beroende routning är begränsad åtkomst till en enskild databas per begäran.</span><span class="sxs-lookup"><span data-stu-id="03a2b-107">Each specific query or transaction in an application using data dependent routing is restricted to accessing a single database per request.</span></span> <span data-ttu-id="03a2b-108">För Azure SQL Database elastisk verktyg för den här routning sker med hjälp av  **[ShardMapManager klassen](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)**  i ADO.NET-program.</span><span class="sxs-lookup"><span data-stu-id="03a2b-108">For the Azure SQL Database Elastic tools, this routing is accomplished with the **[ShardMapManager class](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanager.aspx)** in ADO.NET applications.</span></span>

<span data-ttu-id="03a2b-109">Programmet behöver inte följa olika anslutningssträngar eller DB platser som är associerade med olika datasegment i delat miljö.</span><span class="sxs-lookup"><span data-stu-id="03a2b-109">The application does not need to track various connection strings or DB locations associated with different slices of data in the sharded environment.</span></span> <span data-ttu-id="03a2b-110">I stället den [Fragmentera kartan Manager](sql-database-elastic-scale-shard-map-management.md) öppnas anslutningar till rätt databaser vid behov, baserat på data i kartan Fragmentera och värdet för nyckeln för horisontell partitionering som är mål för programmets begäran.</span><span class="sxs-lookup"><span data-stu-id="03a2b-110">Instead, the [Shard Map Manager](sql-database-elastic-scale-shard-map-management.md) opens connections to the correct databases when needed, based on the data in the shard map and the value of the sharding key that is the target of the application’s request.</span></span> <span data-ttu-id="03a2b-111">Nyckeln är vanligtvis den *customer_id*, *tenant_id*, *date_key*, eller vissa specifika identifierare som är en grundläggande parameter för databasbegäran).</span><span class="sxs-lookup"><span data-stu-id="03a2b-111">The key is typically the *customer_id*, *tenant_id*, *date_key*, or some other specific identifier that is a fundamental parameter of the database request).</span></span> 

<span data-ttu-id="03a2b-112">Mer information finns i [skala ut SQL Server med Data beroende routning](https://technet.microsoft.com/library/cc966448.aspx).</span><span class="sxs-lookup"><span data-stu-id="03a2b-112">For more information, see [Scaling Out SQL Server with Data Dependent Routing](https://technet.microsoft.com/library/cc966448.aspx).</span></span>

## <a name="download-the-client-library"></a><span data-ttu-id="03a2b-113">Hämta klientbiblioteket</span><span class="sxs-lookup"><span data-stu-id="03a2b-113">Download the client library</span></span>
<span data-ttu-id="03a2b-114">För att få klassen, installera den [klientbibliotek för elastisk databas](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span><span class="sxs-lookup"><span data-stu-id="03a2b-114">To get the class, install the [Elastic Database Client Library](http://www.nuget.org/packages/Microsoft.Azure.SqlDatabase.ElasticScale.Client/).</span></span> 

## <a name="using-a-shardmapmanager-in-a-data-dependent-routing-application"></a><span data-ttu-id="03a2b-115">Med hjälp av en ShardMapManager i ett beroende routning program</span><span class="sxs-lookup"><span data-stu-id="03a2b-115">Using a ShardMapManager in a data dependent routing application</span></span>
<span data-ttu-id="03a2b-116">Program ska skapa en instans av den **ShardMapManager** under initieringen med factory anropet  **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span><span class="sxs-lookup"><span data-stu-id="03a2b-116">Applications should instantiate the **ShardMapManager** during initialization, using the factory call **[GetSQLShardMapManager](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmapmanagerfactory.getsqlshardmapmanager.aspx)**.</span></span> <span data-ttu-id="03a2b-117">I detta exempel både en **ShardMapManager** och en specifik **ShardMap** som den innehåller har initierats.</span><span class="sxs-lookup"><span data-stu-id="03a2b-117">In this example, both a **ShardMapManager** and a specific **ShardMap** that it contains are initialized.</span></span> <span data-ttu-id="03a2b-118">Det här exemplet illustrerar GetSqlShardMapManager och [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) metoder.</span><span class="sxs-lookup"><span data-stu-id="03a2b-118">This example shows the GetSqlShardMapManager and [GetRangeShardMap](https://msdn.microsoft.com/library/azure/dn824173.aspx) methods.</span></span>

    ShardMapManager smm = ShardMapManagerFactory.GetSqlShardMapManager(smmConnnectionString, 
                      ShardMapManagerLoadPolicy.Lazy);
    RangeShardMap<int> customerShardMap = smm.GetRangeShardMap<int>("customerMap"); 

### <a name="use-lowest-privilege-credentials-possible-for-getting-the-shard-map"></a><span data-ttu-id="03a2b-119">Använda lägsta behörighet autentiseringsuppgifter som är möjliga för att hämta Fragmentera kartan</span><span class="sxs-lookup"><span data-stu-id="03a2b-119">Use lowest privilege credentials possible for getting the shard map</span></span>
<span data-ttu-id="03a2b-120">Om ett program inte manipulering Fragmentera kartan sig själv, de autentiseringsuppgifter som används i standardmetoden har bara läsbehörighet på den **globala Fragmentera kartan** databas.</span><span class="sxs-lookup"><span data-stu-id="03a2b-120">If an application is not manipulating the shard map itself, the credentials used in the factory method should have just read-only permissions on the **Global Shard Map** database.</span></span> <span data-ttu-id="03a2b-121">Autentiseringsuppgifterna är vanligtvis olika från autentiseringsuppgifter som används för att öppna anslutningar till hanteraren Fragmentera kartan.</span><span class="sxs-lookup"><span data-stu-id="03a2b-121">These credentials are typically different from credentials used to open connections to the shard map manager.</span></span> <span data-ttu-id="03a2b-122">Se även [autentiseringsuppgifter används för att komma åt klientbibliotek för elastisk databas](sql-database-elastic-scale-manage-credentials.md).</span><span class="sxs-lookup"><span data-stu-id="03a2b-122">See also [Credentials used to access the Elastic Database client library](sql-database-elastic-scale-manage-credentials.md).</span></span> 

## <a name="call-the-openconnectionforkey-method"></a><span data-ttu-id="03a2b-123">Anropa metoden OpenConnectionForKey</span><span class="sxs-lookup"><span data-stu-id="03a2b-123">Call the OpenConnectionForKey method</span></span>
<span data-ttu-id="03a2b-124">Den  **[ShardMap.OpenConnectionForKey metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)**  returnerar en ADO.Net-anslutning som är redo för att utfärda kommandon till rätt databas baserat på värdet för den **nyckeln** parameter.</span><span class="sxs-lookup"><span data-stu-id="03a2b-124">The **[ShardMap.OpenConnectionForKey method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkey.aspx)** returns an ADO.Net connection ready for issuing commands to the appropriate database based on the value of the **key** parameter.</span></span> <span data-ttu-id="03a2b-125">Fragmentera informationen cachelagras i program genom den **ShardMapManager**, så dessa begäranden inte normalt innefattar en databassökning mot den **globala Fragmentera kartan** databas.</span><span class="sxs-lookup"><span data-stu-id="03a2b-125">Shard information is cached in the application by the **ShardMapManager**, so these requests do not typically involve a database lookup against the **Global Shard Map** database.</span></span> 

    // Syntax: 
    public SqlConnection OpenConnectionForKey<TKey>(
        TKey key,
        string connectionString,
        ConnectionOptions options
    )


* <span data-ttu-id="03a2b-126">Den **nyckeln** parametern används som en sökning-nyckel till Fragmentera mappningen för att fastställa en lämplig databas för begäran.</span><span class="sxs-lookup"><span data-stu-id="03a2b-126">The **key** parameter is used as a lookup key into the shard map to determine the appropriate database for the request.</span></span> 
* <span data-ttu-id="03a2b-127">Den **connectionString** används för att skicka användarens autentiseringsuppgifter för önskad anslutning.</span><span class="sxs-lookup"><span data-stu-id="03a2b-127">The **connectionString** is used to pass only the user credentials for the desired connection.</span></span> <span data-ttu-id="03a2b-128">Inget databasnamn eller servernamn ingår i detta *connectionString* eftersom metoden avgör databasen och server använder den **ShardMap**.</span><span class="sxs-lookup"><span data-stu-id="03a2b-128">No database name or server name are included in this *connectionString* since the method will determine the database and server using the **ShardMap**.</span></span> 
* <span data-ttu-id="03a2b-129">Den  **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)**  ska anges till **ConnectionOptions.Validate** om en miljö där Fragmentera mappar kan ändras och rader kan flyttas till andra databaser på grund av delade eller merge-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="03a2b-129">The **[connectionOptions](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.connectionoptions.aspx)** should be set to **ConnectionOptions.Validate** if an environment where shard maps may change and rows may move to other databases as a result of split or merge operations.</span></span> <span data-ttu-id="03a2b-130">Detta innebär en kortfattad fråga till lokal Fragmentera kartan på målet databasen (inte till globala Fragmentera kartan) innan anslutningen levereras till programmet.</span><span class="sxs-lookup"><span data-stu-id="03a2b-130">This involves a brief query to the local shard map on the target database (not to the global shard map) before the connection is delivered to the application.</span></span> 

<span data-ttu-id="03a2b-131">Om verifiering mot den lokala Fragmentera mappningen misslyckas (som anger att cacheminnet är felaktig), kommer Fragmentera kartan Manager fråga globala Fragmentera kartan för att hämta nya rätt värde för sökningen, uppdatera cachen och hämta och returnera lämpliga databasanslutningen.</span><span class="sxs-lookup"><span data-stu-id="03a2b-131">If the validation against the local shard map fails (indicating that the cache is incorrect), the Shard Map Manager will query the global shard map to obtain the new correct value for the lookup, update the cache, and obtain and return the appropriate database connection.</span></span> 

<span data-ttu-id="03a2b-132">Använd **ConnectionOptions.None** endast när Fragmentera mappningsändringar inte förväntas när ett program är online.</span><span class="sxs-lookup"><span data-stu-id="03a2b-132">Use **ConnectionOptions.None** only when shard mapping changes are not expected while an application is online.</span></span> <span data-ttu-id="03a2b-133">I så fall cachelagrade värden kan antas att alltid för att vara korrekt och extra fram och åter validering anropet till måldatabasen kan hoppas på ett säkert sätt.</span><span class="sxs-lookup"><span data-stu-id="03a2b-133">In that case, the cached values can be assumed to always be correct, and the extra round-trip validation call to the target database can be safely skipped.</span></span> <span data-ttu-id="03a2b-134">Som minskar databastrafik.</span><span class="sxs-lookup"><span data-stu-id="03a2b-134">That reduces database traffic.</span></span> <span data-ttu-id="03a2b-135">Den **connectionOptions** kan också anges via ett värde i en konfigurationsfil för att indikera om ändringar för horisontell partitionering är förväntat eller inte under en viss tidsperiod.</span><span class="sxs-lookup"><span data-stu-id="03a2b-135">The **connectionOptions** may also be set via a value in a configuration file to indicate whether sharding changes are expected or not during a period of time.</span></span>  

<span data-ttu-id="03a2b-136">Det här exemplet används värdet för ett integer-nyckeln **CustomerID**med hjälp av en **ShardMap** objekt med namnet **customerShardMap**.</span><span class="sxs-lookup"><span data-stu-id="03a2b-136">This example uses the value of an integer key **CustomerID**, using a **ShardMap** object named **customerShardMap**.</span></span>  

    int customerId = 12345; 
    int newPersonId = 4321; 

    // Connect to the shard for that customer ID. No need to call a SqlConnection 
    // constructor followed by the Open method.
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

<span data-ttu-id="03a2b-137">Den **OpenConnectionForKey** metoden returnerar en ny redan öppen anslutning till rätt databas.</span><span class="sxs-lookup"><span data-stu-id="03a2b-137">The **OpenConnectionForKey** method returns a new already-open connection to the correct database.</span></span> <span data-ttu-id="03a2b-138">Anslutningar som används i det här sättet fortfarande dra full nytta av ADO.Net anslutningspooler.</span><span class="sxs-lookup"><span data-stu-id="03a2b-138">Connections utilized in this way still take full advantage of ADO.Net connection pooling.</span></span> <span data-ttu-id="03a2b-139">Så länge transaktioner och begäranden kan betjänas av en Fragmentera samtidigt, bör detta vara den endast ändringen som krävs i ett program som redan använder ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="03a2b-139">As long as transactions and requests can be satisfied by one shard at a time, this should be the only modification necessary in an application already using ADO.Net.</span></span> 

<span data-ttu-id="03a2b-140">Den  **[OpenConnectionForKeyAsync metoden](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)**  finns även om programmet gör Använd asynkron programmering med ADO.Net.</span><span class="sxs-lookup"><span data-stu-id="03a2b-140">The **[OpenConnectionForKeyAsync method](https://msdn.microsoft.com/library/azure/microsoft.azure.sqldatabase.elasticscale.shardmanagement.shardmap.openconnectionforkeyasync.aspx)** is also available if your application makes use asynchronous programming with ADO.Net.</span></span> <span data-ttu-id="03a2b-141">Sitt beteende är data beroende routning motsvarar ADO. Net's  **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)**  metod.</span><span class="sxs-lookup"><span data-stu-id="03a2b-141">Its behavior is the data dependent routing equivalent of ADO.Net's **[Connection.OpenAsync](https://msdn.microsoft.com/library/hh223688\(v=vs.110\).aspx)** method.</span></span>

## <a name="integrating-with-transient-fault-handling"></a><span data-ttu-id="03a2b-142">Integrera med tillfälligt fel hantering</span><span class="sxs-lookup"><span data-stu-id="03a2b-142">Integrating with transient fault handling</span></span>
<span data-ttu-id="03a2b-143">Bästa praxis för att utveckla program för åtkomst av data i molnet är att se till att tillfälliga fel som fångas av appen och att åtgärderna är ett nytt försök flera gånger innan ge ett fel.</span><span class="sxs-lookup"><span data-stu-id="03a2b-143">A best practice in developing data access applications in the cloud is to ensure that transient faults are caught by the app, and that the operations are retried several times before throwing an error.</span></span> <span data-ttu-id="03a2b-144">Tillfälligt fel hantering för molnprogram beskrivs på [hantering av tillfälliga fel](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span><span class="sxs-lookup"><span data-stu-id="03a2b-144">Transient fault handling for cloud applications is discussed at [Transient Fault Handling](https://msdn.microsoft.com/library/dn440719\(v=pandp.60\).aspx).</span></span> 

<span data-ttu-id="03a2b-145">Tillfälligt fel hantering kan samexistera naturligt med mönstret Data beroende routning.</span><span class="sxs-lookup"><span data-stu-id="03a2b-145">Transient fault handling can coexist naturally with the Data Dependent Routing pattern.</span></span> <span data-ttu-id="03a2b-146">Ett krav är att försöka hela data access begäran inklusive den **med** block som hämtade data beroende routning anslutningen.</span><span class="sxs-lookup"><span data-stu-id="03a2b-146">The key requirement is to retry the entire data access request including the **using** block that obtained the data-dependent routing connection.</span></span> <span data-ttu-id="03a2b-147">Exemplet ovan kan skrivas enligt följande (anteckning markeras ändra).</span><span class="sxs-lookup"><span data-stu-id="03a2b-147">The example above could be rewritten as follows (note highlighted change).</span></span> 

### <a name="example---data-dependent-routing-with-transient-fault-handling"></a><span data-ttu-id="03a2b-148">Exempel – data beroende routning med tillfälligt fel hantering</span><span class="sxs-lookup"><span data-stu-id="03a2b-148">Example - data dependent routing with transient fault handling</span></span>
<pre><code>int customerId = 12345; 
int newPersonId = 4321; 

<span style="background-color:  #FFFF00">Configuration.SqlRetryPolicy.ExecuteAction(() =&gt; </span> 
<span style="background-color:  #FFFF00">    { </span>
        // Connect to the shard for a customer ID. 
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


<span data-ttu-id="03a2b-149">Paket som är nödvändiga för att implementera tillfälligt fel hantering laddas ned automatiskt när du skapar exempelprogrammet elastisk databas.</span><span class="sxs-lookup"><span data-stu-id="03a2b-149">Packages necessary to implement transient fault handling are downloaded automatically when you build the elastic database sample application.</span></span> <span data-ttu-id="03a2b-150">Paket finns också separat på [Enterprise Library - tillfälliga fel hanterar programmet Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span><span class="sxs-lookup"><span data-stu-id="03a2b-150">Packages are also available separately at [Enterprise Library - Transient Fault Handling Application Block](http://www.nuget.org/packages/EnterpriseLibrary.TransientFaultHandling/).</span></span> <span data-ttu-id="03a2b-151">Använd version 6.0 eller senare.</span><span class="sxs-lookup"><span data-stu-id="03a2b-151">Use version 6.0 or later.</span></span> 

## <a name="transactional-consistency"></a><span data-ttu-id="03a2b-152">Transaktionskonsekvens</span><span class="sxs-lookup"><span data-stu-id="03a2b-152">Transactional consistency</span></span>
<span data-ttu-id="03a2b-153">Transaktionell egenskaper är garanterat för alla åtgärder lokalt på en Fragmentera.</span><span class="sxs-lookup"><span data-stu-id="03a2b-153">Transactional properties are guaranteed for all operations local to a shard.</span></span> <span data-ttu-id="03a2b-154">Till exempel köra transaktioner som har skickats via data beroende routning inom omfånget för mål-Fragmentera för anslutningen.</span><span class="sxs-lookup"><span data-stu-id="03a2b-154">For example, transactions submitted through data-dependent routing execute within the scope of the target shard for the connection.</span></span> <span data-ttu-id="03a2b-155">För tillfället finns inga funktioner för att ta med flera anslutningar i en transaktion och det finns därför inga transaktionella garantier för åtgärder som utförs i shards.</span><span class="sxs-lookup"><span data-stu-id="03a2b-155">At this time, there are no capabilities provided for enlisting multiple connections into a transaction, and therefore there are no transactional guarantees for operations performed across shards.</span></span>

## <a name="next-steps"></a><span data-ttu-id="03a2b-156">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="03a2b-156">Next steps</span></span>
<span data-ttu-id="03a2b-157">Koppla från en Fragmentera eller återansluta ett Fragmentera finns [med hjälp av klassen RecoveryManager för att åtgärda problem med Fragmentera karta](sql-database-elastic-database-recovery-manager.md)</span><span class="sxs-lookup"><span data-stu-id="03a2b-157">To detach a shard, or to reattach a shard, see [Using the RecoveryManager class to fix shard map problems](sql-database-elastic-database-recovery-manager.md)</span></span>

[!INCLUDE [elastic-scale-include](../../includes/elastic-scale-include.md)]

