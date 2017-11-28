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
# <a name="how-toouse-batching-tooimprove-sql-database-application-performance"></a><span data-ttu-id="91852-104">Hur toouse batchbearbetning tooimprove programprestanda för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="91852-104">How toouse batching tooimprove SQL Database application performance</span></span>
<span data-ttu-id="91852-105">Batchbearbetning operations tooAzure SQL-databas avsevärt förbättrar hello prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="91852-105">Batching operations tooAzure SQL Database significantly improves hello performance and scalability of your applications.</span></span> <span data-ttu-id="91852-106">I ordning toounderstand hello fördelar innehåller hello första delen av den här artikeln några exempel på testresultat som jämför sekventiella och gruppbaserad begäranden tooa SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="91852-106">In order toounderstand hello benefits, hello first part of this article covers some sample test results that compare sequential and batched requests tooa SQL Database.</span></span> <span data-ttu-id="91852-107">hello resten av hello artikeln visar hello tekniker, scenarier och överväganden toohelp toouse batchbearbetning har i din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="91852-107">hello remainder of hello article shows hello techniques, scenarios, and considerations toohelp you toouse batching successfully in your Azure applications.</span></span>

## <a name="why-is-batching-important-for-sql-database"></a><span data-ttu-id="91852-108">Varför är batchbearbetning viktigt för SQL-databas?</span><span class="sxs-lookup"><span data-stu-id="91852-108">Why is batching important for SQL Database?</span></span>
<span data-ttu-id="91852-109">Batchbearbetning anrop tooa fjärrtjänsten är en välkänd strategi för att öka prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="91852-109">Batching calls tooa remote service is a well-known strategy for increasing performance and scalability.</span></span> <span data-ttu-id="91852-110">Det fasta är bearbetning kostnader tooany interaktioner med en fjärransluten tjänst, till exempel serialisering och nätverksöverföring deserialisering.</span><span class="sxs-lookup"><span data-stu-id="91852-110">There are fixed processing costs tooany interactions with a remote service, such as serialization, network transfer, and deserialization.</span></span> <span data-ttu-id="91852-111">Paketera många separata transaktioner i en enskild batch minimerar dessa kostnader.</span><span class="sxs-lookup"><span data-stu-id="91852-111">Packaging many separate transactions into a single batch minimizes these costs.</span></span>

<span data-ttu-id="91852-112">I det här dokumentet vill vi tooexamine olika scenarier och batchbearbetning strategier för SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="91852-112">In this paper, we want tooexamine various SQL Database batching strategies and scenarios.</span></span> <span data-ttu-id="91852-113">Men strategierna är också viktigt för lokala program som använder SQL Server, finns det flera orsaker till syntaxmarkering hello användning av batchbearbetning för SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="91852-113">Although these strategies are also important for on-premises applications that use SQL Server, there are several reasons for highlighting hello use of batching for SQL Database:</span></span>

* <span data-ttu-id="91852-114">Det finns potentiellt större Nätverksfördröjningen för åtkomst till SQL-databas, särskilt om du ansluter till SQL-databas från utanför hello samma Microsoft Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="91852-114">There is potentially greater network latency in accessing SQL Database, especially if you are accessing SQL Database from outside hello same Microsoft Azure datacenter.</span></span>
* <span data-ttu-id="91852-115">hello flera egenskaper för SQL-databasen innebär att hello effektiviteten för hello data åt lagret korrelerar toohello övergripande skalbarhet av hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="91852-115">hello multitenant characteristics of SQL Database means that hello efficiency of hello data access layer correlates toohello overall scalability of hello database.</span></span> <span data-ttu-id="91852-116">SQL-databas måste förhindra att en enskild klient/användare alltid använder databasen resurser toohello skada för andra klienter.</span><span class="sxs-lookup"><span data-stu-id="91852-116">SQL Database must prevent any single tenant/user from monopolizing database resources toohello detriment of other tenants.</span></span> <span data-ttu-id="91852-117">I svaret toousage överskrider fördefinierade kvoter, SQL Database minska eller svara med begränsning undantag.</span><span class="sxs-lookup"><span data-stu-id="91852-117">In response toousage in excess of predefined quotas, SQL Database can reduce throughput or respond with throttling exceptions.</span></span> <span data-ttu-id="91852-118">Effektivitet, till exempel batchbearbetning, aktiverar du toodo mer arbete i SQL-databasen innan det nådde dessa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="91852-118">Efficiencies, such as batching, enable you toodo more work on SQL Database before reaching these limits.</span></span> 
* <span data-ttu-id="91852-119">Batchbearbetning är också gälla för konfigurationer med flera databaser (delning).</span><span class="sxs-lookup"><span data-stu-id="91852-119">Batching is also effective for architectures that use multiple databases (sharding).</span></span> <span data-ttu-id="91852-120">hello effektiviteten för interaktionen med varje enhet i databasen är fortfarande en avgörande faktor för din övergripande skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="91852-120">hello efficiency of your interaction with each database unit is still a key factor in your overall scalability.</span></span> 

<span data-ttu-id="91852-121">En av hello fördelarna med att använda SQL-databas är att du inte har toomanage hello servrar att värden hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="91852-121">One of hello benefits of using SQL Database is that you don’t have toomanage hello servers that host hello database.</span></span> <span data-ttu-id="91852-122">Hanterade infrastrukturen innebär dock också att du har toothink annorlunda om databasoptimering.</span><span class="sxs-lookup"><span data-stu-id="91852-122">However, this managed infrastructure also means that you have toothink differently about database optimizations.</span></span> <span data-ttu-id="91852-123">Du kan inte längre se tooimprove hello datormaskinvaran eller nätverksanslutningen databasinfrastruktur.</span><span class="sxs-lookup"><span data-stu-id="91852-123">You can no longer look tooimprove hello database hardware or network infrastructure.</span></span> <span data-ttu-id="91852-124">Microsoft Azure styr dessa miljöer.</span><span class="sxs-lookup"><span data-stu-id="91852-124">Microsoft Azure controls those environments.</span></span> <span data-ttu-id="91852-125">hello huvudsakliga område som du kan styra är hur programmet samverkar med SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="91852-125">hello main area that you can control is how your application interacts with SQL Database.</span></span> <span data-ttu-id="91852-126">Batchbearbetning är en av dessa optimeringar.</span><span class="sxs-lookup"><span data-stu-id="91852-126">Batching is one of these optimizations.</span></span> 

<span data-ttu-id="91852-127">hello första delen av hello papper undersöker olika batching tekniker för .NET-program som använder SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="91852-127">hello first part of hello paper examines various batching techniques for .NET applications that use SQL Database.</span></span> <span data-ttu-id="91852-128">hello sista två avsnitt beskriver batchbearbetning riktlinjer och scenarier.</span><span class="sxs-lookup"><span data-stu-id="91852-128">hello last two sections cover batching guidelines and scenarios.</span></span>

## <a name="batching-strategies"></a><span data-ttu-id="91852-129">Batchbearbetning strategier</span><span class="sxs-lookup"><span data-stu-id="91852-129">Batching strategies</span></span>
### <a name="note-about-timing-results-in-this-topic"></a><span data-ttu-id="91852-130">Observera om tidsinställning resultat i det här avsnittet</span><span class="sxs-lookup"><span data-stu-id="91852-130">Note about timing results in this topic</span></span>
> [!NOTE]
> <span data-ttu-id="91852-131">Resultatet är inte prestandamått men är avsedda tooshow **relativa prestandan**.</span><span class="sxs-lookup"><span data-stu-id="91852-131">Results are not benchmarks but are meant tooshow **relative performance**.</span></span> <span data-ttu-id="91852-132">Tidsinställningar baseras på ett genomsnitt av minst 10 testkörningar.</span><span class="sxs-lookup"><span data-stu-id="91852-132">Timings are based on an average of at least 10 test runs.</span></span> <span data-ttu-id="91852-133">Åtgärder som infogar i en tom tabell.</span><span class="sxs-lookup"><span data-stu-id="91852-133">Operations are inserts into an empty table.</span></span> <span data-ttu-id="91852-134">Dessa tester har uppmätta pre-V12 och de inte nödvändigtvis toothroughput som kan uppstå i en V12-databas med hjälp av hello ny [tjänstnivåer](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="91852-134">These tests were measured pre-V12, and they do not necessarily correspond toothroughput that you might experience in a V12 database using hello new [service tiers](sql-database-service-tiers.md).</span></span> <span data-ttu-id="91852-135">hello relativa fördelen hello batchbearbetning tekniken bör vara densamma.</span><span class="sxs-lookup"><span data-stu-id="91852-135">hello relative benefit of hello batching technique should be similar.</span></span>
> 
> 

### <a name="transactions"></a><span data-ttu-id="91852-136">Transaktioner</span><span class="sxs-lookup"><span data-stu-id="91852-136">Transactions</span></span>
<span data-ttu-id="91852-137">Det verkar konstigt toobegin en granskning av batchbearbetning av diskutera transaktioner.</span><span class="sxs-lookup"><span data-stu-id="91852-137">It seems strange toobegin a review of batching by discussing transactions.</span></span> <span data-ttu-id="91852-138">Men hello användning av transaktioner på klientsidan har en diskret serversidan batching effekt som förbättrar prestanda.</span><span class="sxs-lookup"><span data-stu-id="91852-138">But hello use of client-side transactions has a subtle server-side batching effect that improves performance.</span></span> <span data-ttu-id="91852-139">Och transaktioner kan läggas till med bara några få rader med kod, så att de får ett snabbt sätt tooimprove prestanda för sekventiella operationer.</span><span class="sxs-lookup"><span data-stu-id="91852-139">And transactions can be added with only a few lines of code, so they provide a fast way tooimprove performance of sequential operations.</span></span>

<span data-ttu-id="91852-140">Överväg att hello följande C#-kod som innehåller en sekvens med insert och uppdateringsåtgärder på en enkel tabell.</span><span class="sxs-lookup"><span data-stu-id="91852-140">Consider hello following C# code that contains a sequence of insert and update operations on a simple table.</span></span>

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

<span data-ttu-id="91852-141">hello följande ADO.NET kod sekventiellt utför dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="91852-141">hello following ADO.NET code sequentially performs these operations.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

<span data-ttu-id="91852-142">Hej bästa sätt toooptimize koden är tooimplement någon form av klientsidan massbearbetning av dessa anrop.</span><span class="sxs-lookup"><span data-stu-id="91852-142">hello best way toooptimize this code is tooimplement some form of client-side batching of these calls.</span></span> <span data-ttu-id="91852-143">Men det finns ett enkelt sätt tooincrease hello prestanda för den här koden genom att helt enkelt omsluta hello sekvens med anrop i en transaktion.</span><span class="sxs-lookup"><span data-stu-id="91852-143">But there is a simple way tooincrease hello performance of this code by simply wrapping hello sequence of calls in a transaction.</span></span> <span data-ttu-id="91852-144">Här är hello samma kod som använder en transaktion.</span><span class="sxs-lookup"><span data-stu-id="91852-144">Here is hello same code that uses a transaction.</span></span>

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

<span data-ttu-id="91852-145">Transaktioner används faktiskt i båda de här exemplen.</span><span class="sxs-lookup"><span data-stu-id="91852-145">Transactions are actually being used in both of these examples.</span></span> <span data-ttu-id="91852-146">Varje enskilda anropet är en implicit transaktion i hello första exemplet.</span><span class="sxs-lookup"><span data-stu-id="91852-146">In hello first example, each individual call is an implicit transaction.</span></span> <span data-ttu-id="91852-147">I andra exemplet hello radbryts en explicit transaktion alla hello-anrop.</span><span class="sxs-lookup"><span data-stu-id="91852-147">In hello second example, an explicit transaction wraps all of hello calls.</span></span> <span data-ttu-id="91852-148">Per hello dokumentationen för hello [write-ahead transaktionsloggen](https://msdn.microsoft.com/library/ms186259.aspx), loggposter är en toohello disk när hello transaktion har genomförts.</span><span class="sxs-lookup"><span data-stu-id="91852-148">Per hello documentation for hello [write-ahead transaction log](https://msdn.microsoft.com/library/ms186259.aspx), log records are flushed toohello disk when hello transaction commits.</span></span> <span data-ttu-id="91852-149">Så genom att lägga till fler anrop i en transaktion kan hello skrivåtgärder toohello transaktionsloggen fördröja tills genomförs hello transaktionen.</span><span class="sxs-lookup"><span data-stu-id="91852-149">So by including more calls in a transaction, hello write toohello transaction log can delay until hello transaction is committed.</span></span> <span data-ttu-id="91852-150">Du aktiverar i praktiken batchbearbetning för hello skrivningar toohello serverns transaktionsloggen.</span><span class="sxs-lookup"><span data-stu-id="91852-150">In effect, you are enabling batching for hello writes toohello server’s transaction log.</span></span>

<span data-ttu-id="91852-151">hello följande tabell visar några ad hoc-testresultaten.</span><span class="sxs-lookup"><span data-stu-id="91852-151">hello following table shows some ad-hoc testing results.</span></span> <span data-ttu-id="91852-152">hello tester hello samma sekventiella infogar med och utan transaktioner.</span><span class="sxs-lookup"><span data-stu-id="91852-152">hello tests performed hello same sequential inserts with and without transactions.</span></span> <span data-ttu-id="91852-153">Mer perspektiv kördes hello första uppsättningen tester för fjärråtkomst från en bärbar dator toohello databas i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="91852-153">For more perspective, hello first set of tests ran remotely from a laptop toohello database in Microsoft Azure.</span></span> <span data-ttu-id="91852-154">hello första uppsättningen tester körde från en Molntjänsten och databasen att båda finns inom hello samma Microsoft Azure-datacenter (USA, västra).</span><span class="sxs-lookup"><span data-stu-id="91852-154">hello second set of tests ran from a cloud service and database that both resided within hello same Microsoft Azure datacenter (West US).</span></span> <span data-ttu-id="91852-155">hello följande tabell visar hello tid i millisekunder för sekventiella infogningar med och utan transaktioner.</span><span class="sxs-lookup"><span data-stu-id="91852-155">hello following table shows hello duration in milliseconds of sequential inserts with and without transactions.</span></span>

<span data-ttu-id="91852-156">**Lokala tooAzure**:</span><span class="sxs-lookup"><span data-stu-id="91852-156">**On-Premises tooAzure**:</span></span>

| <span data-ttu-id="91852-157">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="91852-157">Operations</span></span> | <span data-ttu-id="91852-158">Ingen transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-158">No Transaction (ms)</span></span> | <span data-ttu-id="91852-159">Transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-159">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91852-160">1</span><span class="sxs-lookup"><span data-stu-id="91852-160">1</span></span> |<span data-ttu-id="91852-161">130</span><span class="sxs-lookup"><span data-stu-id="91852-161">130</span></span> |<span data-ttu-id="91852-162">402</span><span class="sxs-lookup"><span data-stu-id="91852-162">402</span></span> |
| <span data-ttu-id="91852-163">10</span><span class="sxs-lookup"><span data-stu-id="91852-163">10</span></span> |<span data-ttu-id="91852-164">1208</span><span class="sxs-lookup"><span data-stu-id="91852-164">1208</span></span> |<span data-ttu-id="91852-165">1226</span><span class="sxs-lookup"><span data-stu-id="91852-165">1226</span></span> |
| <span data-ttu-id="91852-166">100</span><span class="sxs-lookup"><span data-stu-id="91852-166">100</span></span> |<span data-ttu-id="91852-167">12662</span><span class="sxs-lookup"><span data-stu-id="91852-167">12662</span></span> |<span data-ttu-id="91852-168">10395</span><span class="sxs-lookup"><span data-stu-id="91852-168">10395</span></span> |
| <span data-ttu-id="91852-169">1000</span><span class="sxs-lookup"><span data-stu-id="91852-169">1000</span></span> |<span data-ttu-id="91852-170">128852</span><span class="sxs-lookup"><span data-stu-id="91852-170">128852</span></span> |<span data-ttu-id="91852-171">102917</span><span class="sxs-lookup"><span data-stu-id="91852-171">102917</span></span> |

<span data-ttu-id="91852-172">**Azure tooAzure (samma datacenter)**:</span><span class="sxs-lookup"><span data-stu-id="91852-172">**Azure tooAzure (same datacenter)**:</span></span>

| <span data-ttu-id="91852-173">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="91852-173">Operations</span></span> | <span data-ttu-id="91852-174">Ingen transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-174">No Transaction (ms)</span></span> | <span data-ttu-id="91852-175">Transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-175">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91852-176">1</span><span class="sxs-lookup"><span data-stu-id="91852-176">1</span></span> |<span data-ttu-id="91852-177">21</span><span class="sxs-lookup"><span data-stu-id="91852-177">21</span></span> |<span data-ttu-id="91852-178">26</span><span class="sxs-lookup"><span data-stu-id="91852-178">26</span></span> |
| <span data-ttu-id="91852-179">10</span><span class="sxs-lookup"><span data-stu-id="91852-179">10</span></span> |<span data-ttu-id="91852-180">220</span><span class="sxs-lookup"><span data-stu-id="91852-180">220</span></span> |<span data-ttu-id="91852-181">56</span><span class="sxs-lookup"><span data-stu-id="91852-181">56</span></span> |
| <span data-ttu-id="91852-182">100</span><span class="sxs-lookup"><span data-stu-id="91852-182">100</span></span> |<span data-ttu-id="91852-183">2145</span><span class="sxs-lookup"><span data-stu-id="91852-183">2145</span></span> |<span data-ttu-id="91852-184">341</span><span class="sxs-lookup"><span data-stu-id="91852-184">341</span></span> |
| <span data-ttu-id="91852-185">1000</span><span class="sxs-lookup"><span data-stu-id="91852-185">1000</span></span> |<span data-ttu-id="91852-186">21479</span><span class="sxs-lookup"><span data-stu-id="91852-186">21479</span></span> |<span data-ttu-id="91852-187">2756</span><span class="sxs-lookup"><span data-stu-id="91852-187">2756</span></span> |

> [!NOTE]
> <span data-ttu-id="91852-188">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="91852-188">Results are not benchmarks.</span></span> <span data-ttu-id="91852-189">Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="91852-189">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="91852-190">Baserat på hello tidigare testresultaten, minskar byta en enda åtgärd i en transaktion faktiskt prestanda.</span><span class="sxs-lookup"><span data-stu-id="91852-190">Based on hello previous test results, wrapping a single operation in a transaction actually decreases performance.</span></span> <span data-ttu-id="91852-191">Men om du ökar hello antal åtgärder i en enda transaktion hello prestandaförbättring blir större.</span><span class="sxs-lookup"><span data-stu-id="91852-191">But as you increase hello number of operations within a single transaction, hello performance improvement becomes more marked.</span></span> <span data-ttu-id="91852-192">Hej prestandaskillnad är också mer märkbart när alla åtgärder inom hello Microsoft Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="91852-192">hello performance difference is also more noticeable when all operations occur within hello Microsoft Azure datacenter.</span></span> <span data-ttu-id="91852-193">hello överskuggar ökad latens med SQL-databas från utanför hello Microsoft Azure-datacenter hello prestandafördelar med transaktioner.</span><span class="sxs-lookup"><span data-stu-id="91852-193">hello increased latency of using SQL Database from outside hello Microsoft Azure datacenter overshadows hello performance gain of using transactions.</span></span>

<span data-ttu-id="91852-194">Även om hello användning av transaktioner kan öka prestanda kan fortsätta för[Se metodtips för transaktioner och anslutningar](https://msdn.microsoft.com/library/ms187484.aspx).</span><span class="sxs-lookup"><span data-stu-id="91852-194">Although hello use of transactions can increase performance, continue too[observe best practices for transactions and connections](https://msdn.microsoft.com/library/ms187484.aspx).</span></span> <span data-ttu-id="91852-195">Behåll hello transaktioner så kort som möjligt och Stäng hello databasanslutning när hello arbete har slutförts.</span><span class="sxs-lookup"><span data-stu-id="91852-195">Keep hello transaction as short as possible, and close hello database connection after hello work completes.</span></span> <span data-ttu-id="91852-196">hello med instruktionen i hello föregående exempel säkerställer att hello anslutningen är stängd när hello efterföljande kodblock har slutförts.</span><span class="sxs-lookup"><span data-stu-id="91852-196">hello using statement in hello previous example assures that hello connection is closed when hello subsequent code block completes.</span></span>

<span data-ttu-id="91852-197">hello föregående exempel visar att du kan lägga till en lokal transaktion tooany ADO.NET kod med två rader.</span><span class="sxs-lookup"><span data-stu-id="91852-197">hello previous example demonstrates that you can add a local transaction tooany ADO.NET code with two lines.</span></span> <span data-ttu-id="91852-198">Transaktioner erbjuder ett snabbt sätt tooimprove hello prestanda av kod som gör sekventiella infoga, uppdatera och ta bort.</span><span class="sxs-lookup"><span data-stu-id="91852-198">Transactions offer a quick way tooimprove hello performance of code that makes sequential insert, update, and delete operations.</span></span> <span data-ttu-id="91852-199">Men för hello bästa prestanda, Överväg att ändra hello kod ytterligare tootake nytta av klientsidan batchbearbetning, till exempel tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="91852-199">However, for hello fastest performance, consider changing hello code further tootake advantage of client-side batching, such as table-valued parameters.</span></span>

<span data-ttu-id="91852-200">Mer information om transaktioner i ADO.NET finns [lokala transaktioner i ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span><span class="sxs-lookup"><span data-stu-id="91852-200">For more information about transactions in ADO.NET, see [Local Transactions in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span></span>

### <a name="table-valued-parameters"></a><span data-ttu-id="91852-201">tabellvärdeparametrar</span><span class="sxs-lookup"><span data-stu-id="91852-201">Table-valued parameters</span></span>
<span data-ttu-id="91852-202">Tabellvärdeparametrar stöd för användardefinierade tabelltyper som parametrar i Transact-SQL-uttryck, lagrade procedurer och funktioner.</span><span class="sxs-lookup"><span data-stu-id="91852-202">Table-valued parameters support user-defined table types as parameters in Transact-SQL statements, stored procedures, and functions.</span></span> <span data-ttu-id="91852-203">Denna batching teknik för klientsidan kan du toosend flera rader med data i hello tabellvärdesparametern.</span><span class="sxs-lookup"><span data-stu-id="91852-203">This client-side batching technique allows you toosend multiple rows of data within hello table-valued parameter.</span></span> <span data-ttu-id="91852-204">toouse tabellvärdeparametrar, först definiera en tabelltyp.</span><span class="sxs-lookup"><span data-stu-id="91852-204">toouse table-valued parameters, first define a table type.</span></span> <span data-ttu-id="91852-205">hello följande Transact-SQL-instruktion skapas en tabelltyp som heter **MyTableType**.</span><span class="sxs-lookup"><span data-stu-id="91852-205">hello following Transact-SQL statement creates a table type named **MyTableType**.</span></span>

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


<span data-ttu-id="91852-206">I koden, skapar du en **DataTable** med hello exakt samma namn och typer för hello tabelltyp.</span><span class="sxs-lookup"><span data-stu-id="91852-206">In code, you create a **DataTable** with hello exact same names and types of hello table type.</span></span> <span data-ttu-id="91852-207">Skicka detta **DataTable** i en parameter i en textfråga eller en lagrad procedur anropa.</span><span class="sxs-lookup"><span data-stu-id="91852-207">Pass this **DataTable** in a parameter in a text query or stored procedure call.</span></span> <span data-ttu-id="91852-208">hello visas följande exempel den här tekniken:</span><span class="sxs-lookup"><span data-stu-id="91852-208">hello following example shows this technique:</span></span>

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

<span data-ttu-id="91852-209">I föregående exempel hello hello **SqlCommand** objekt infogar rader från en tabellvärdesparameter  **@TestTvp** .</span><span class="sxs-lookup"><span data-stu-id="91852-209">In hello previous example, hello **SqlCommand** object inserts rows from a table-valued parameter, **@TestTvp**.</span></span> <span data-ttu-id="91852-210">hello skapat **DataTable** objekt tilldelas toothis parameter med hello **SqlCommand.Parameters.Add** metod.</span><span class="sxs-lookup"><span data-stu-id="91852-210">hello previously created **DataTable** object is assigned toothis parameter with hello **SqlCommand.Parameters.Add** method.</span></span> <span data-ttu-id="91852-211">Batchbearbetning hello infogningar i ett anropa avsevärt ökar hello prestanda över sekventiella infogningar.</span><span class="sxs-lookup"><span data-stu-id="91852-211">Batching hello inserts in one call significantly increases hello performance over sequential inserts.</span></span>

<span data-ttu-id="91852-212">tooimprove hello föregående exempel dessutom använda en lagrad procedur i stället för ett textbaserat kommando.</span><span class="sxs-lookup"><span data-stu-id="91852-212">tooimprove hello previous example further, use a stored procedure instead of a text-based command.</span></span> <span data-ttu-id="91852-213">hello följande Transact-SQL-kommando skapar en lagrad procedur som tar hello **SimpleTestTableType** tabellvärdesparametern.</span><span class="sxs-lookup"><span data-stu-id="91852-213">hello following Transact-SQL command creates a stored procedure that takes hello **SimpleTestTableType** table-valued parameter.</span></span>

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

<span data-ttu-id="91852-214">Ändra hello **SqlCommand** objekt deklarationen i hello föregående kod exemplet toohello nedan.</span><span class="sxs-lookup"><span data-stu-id="91852-214">Then change hello **SqlCommand** object declaration in hello previous code example toohello following.</span></span>

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

<span data-ttu-id="91852-215">I de flesta fall har tabellvärdeparametrar motsvarande eller bättre prestanda än andra batching metoder.</span><span class="sxs-lookup"><span data-stu-id="91852-215">In most cases, table-valued parameters have equivalent or better performance than other batching techniques.</span></span> <span data-ttu-id="91852-216">Tabellvärdeparametrar är ofta bättre, eftersom de är mer flexibelt än andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="91852-216">Table-valued parameters are often preferable, because they are more flexible than other options.</span></span> <span data-ttu-id="91852-217">Till exempel tillåter andra tekniker, till exempel SQL-masskopiering, bara hello nya rader infogas.</span><span class="sxs-lookup"><span data-stu-id="91852-217">For example, other techniques, such as SQL bulk copy, only permit hello insertion of new rows.</span></span> <span data-ttu-id="91852-218">Men med tabellvärdeparametrar, kan du använda logik i hello lagrade proceduren toodetermine vilka rader som uppdateras och som kan infogas.</span><span class="sxs-lookup"><span data-stu-id="91852-218">But with table-valued parameters, you can use logic in hello stored procedure toodetermine which rows are updates and which are inserts.</span></span> <span data-ttu-id="91852-219">hello tabelltyp kan också vara ändrade toocontain en ”åtgärden”-kolumn som anger om hello angivna raden ska infogas, uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="91852-219">hello table type can also be modified toocontain an “Operation” column that indicates whether hello specified row should be inserted, updated, or deleted.</span></span>

<span data-ttu-id="91852-220">hello följande tabell visar ad hoc-testresultaten för hello använda tabellvärdeparametrar i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="91852-220">hello following table shows ad-hoc test results for hello use of table-valued parameters in milliseconds.</span></span>

| <span data-ttu-id="91852-221">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="91852-221">Operations</span></span> | <span data-ttu-id="91852-222">Lokala tooAzure (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-222">On-Premises tooAzure (ms)</span></span> | <span data-ttu-id="91852-223">Samma Azure-datacenter (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-223">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91852-224">1</span><span class="sxs-lookup"><span data-stu-id="91852-224">1</span></span> |<span data-ttu-id="91852-225">124</span><span class="sxs-lookup"><span data-stu-id="91852-225">124</span></span> |<span data-ttu-id="91852-226">32</span><span class="sxs-lookup"><span data-stu-id="91852-226">32</span></span> |
| <span data-ttu-id="91852-227">10</span><span class="sxs-lookup"><span data-stu-id="91852-227">10</span></span> |<span data-ttu-id="91852-228">131</span><span class="sxs-lookup"><span data-stu-id="91852-228">131</span></span> |<span data-ttu-id="91852-229">25</span><span class="sxs-lookup"><span data-stu-id="91852-229">25</span></span> |
| <span data-ttu-id="91852-230">100</span><span class="sxs-lookup"><span data-stu-id="91852-230">100</span></span> |<span data-ttu-id="91852-231">338</span><span class="sxs-lookup"><span data-stu-id="91852-231">338</span></span> |<span data-ttu-id="91852-232">51</span><span class="sxs-lookup"><span data-stu-id="91852-232">51</span></span> |
| <span data-ttu-id="91852-233">1000</span><span class="sxs-lookup"><span data-stu-id="91852-233">1000</span></span> |<span data-ttu-id="91852-234">2615</span><span class="sxs-lookup"><span data-stu-id="91852-234">2615</span></span> |<span data-ttu-id="91852-235">382</span><span class="sxs-lookup"><span data-stu-id="91852-235">382</span></span> |
| <span data-ttu-id="91852-236">10000</span><span class="sxs-lookup"><span data-stu-id="91852-236">10000</span></span> |<span data-ttu-id="91852-237">23830</span><span class="sxs-lookup"><span data-stu-id="91852-237">23830</span></span> |<span data-ttu-id="91852-238">3586</span><span class="sxs-lookup"><span data-stu-id="91852-238">3586</span></span> |

> [!NOTE]
> <span data-ttu-id="91852-239">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="91852-239">Results are not benchmarks.</span></span> <span data-ttu-id="91852-240">Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="91852-240">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="91852-241">hello prestandafördelar från batchbearbetning är uppenbar.</span><span class="sxs-lookup"><span data-stu-id="91852-241">hello performance gain from batching is immediately apparent.</span></span> <span data-ttu-id="91852-242">I hello föregående sekventiella testet tog 1000 åtgärderna 129 sekunder utanför hello datacenter och 21 sekunder från inom hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="91852-242">In hello previous sequential test, 1000 operations took 129 seconds outside hello datacenter and 21 seconds from within hello datacenter.</span></span> <span data-ttu-id="91852-243">Men med tabellvärdeparametrar, 1000 operations Ta endast 2.6 sekunder utanför hello datacenter och 0,4 sekunder inom hello datacenter.</span><span class="sxs-lookup"><span data-stu-id="91852-243">But with table-valued parameters, 1000 operations take only 2.6 seconds outside hello datacenter and 0.4 seconds within hello datacenter.</span></span>

<span data-ttu-id="91852-244">Mer information om tabellvärdeparametrar finns [Table-Valued parametrar](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="91852-244">For more information on table-valued parameters, see [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

### <a name="sql-bulk-copy"></a><span data-ttu-id="91852-245">SQL-masskopiering</span><span class="sxs-lookup"><span data-stu-id="91852-245">SQL bulk copy</span></span>
<span data-ttu-id="91852-246">SQL-masskopiering är ett annat sätt tooinsert stora mängder data till en måldatabas.</span><span class="sxs-lookup"><span data-stu-id="91852-246">SQL bulk copy is another way tooinsert large amounts of data into a target database.</span></span> <span data-ttu-id="91852-247">.NET-program kan använda hello **SqlBulkCopy körs** klassen tooperform massinfogning åtgärder.</span><span class="sxs-lookup"><span data-stu-id="91852-247">.NET applications can use hello **SqlBulkCopy** class tooperform bulk insert operations.</span></span> <span data-ttu-id="91852-248">**SqlBulkCopy körs** liknar funktionen toohello kommandoradsverktyget **Bcp.exe**, eller hello Transact-SQL-instruktionen **BULK INSERT**.</span><span class="sxs-lookup"><span data-stu-id="91852-248">**SqlBulkCopy** is similar in function toohello command-line tool, **Bcp.exe**, or hello Transact-SQL statement, **BULK INSERT**.</span></span> <span data-ttu-id="91852-249">hello följande kodexempel visar hur toobulk kopiera hello rader i hello källan **DataTable**, tabell, toohello måltabellen i SQL Server, mytable prefix.</span><span class="sxs-lookup"><span data-stu-id="91852-249">hello following code example shows how toobulk copy hello rows in hello source **DataTable**, table, toohello destination table in SQL Server, MyTable.</span></span>

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

<span data-ttu-id="91852-250">Det finns tillfällen där masskopiering är att föredra över tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="91852-250">There are some cases where bulk copy is preferred over table-valued parameters.</span></span> <span data-ttu-id="91852-251">Se hello jämförelsetabell för tabellvärdeparametrar jämfört med BULK INSERT åtgärder i hello avsnittet [Table-Valued parametrar](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="91852-251">See hello comparison table of Table-Valued parameters versus BULK INSERT operations in hello topic [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

<span data-ttu-id="91852-252">hello följande ad hoc-test visar hello prestanda för batchbearbetning med **SqlBulkCopy körs** i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="91852-252">hello following ad-hoc test results show hello performance of batching with **SqlBulkCopy** in milliseconds.</span></span>

| <span data-ttu-id="91852-253">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="91852-253">Operations</span></span> | <span data-ttu-id="91852-254">Lokala tooAzure (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-254">On-Premises tooAzure (ms)</span></span> | <span data-ttu-id="91852-255">Samma Azure-datacenter (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-255">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91852-256">1</span><span class="sxs-lookup"><span data-stu-id="91852-256">1</span></span> |<span data-ttu-id="91852-257">433</span><span class="sxs-lookup"><span data-stu-id="91852-257">433</span></span> |<span data-ttu-id="91852-258">57</span><span class="sxs-lookup"><span data-stu-id="91852-258">57</span></span> |
| <span data-ttu-id="91852-259">10</span><span class="sxs-lookup"><span data-stu-id="91852-259">10</span></span> |<span data-ttu-id="91852-260">441</span><span class="sxs-lookup"><span data-stu-id="91852-260">441</span></span> |<span data-ttu-id="91852-261">32</span><span class="sxs-lookup"><span data-stu-id="91852-261">32</span></span> |
| <span data-ttu-id="91852-262">100</span><span class="sxs-lookup"><span data-stu-id="91852-262">100</span></span> |<span data-ttu-id="91852-263">636</span><span class="sxs-lookup"><span data-stu-id="91852-263">636</span></span> |<span data-ttu-id="91852-264">53</span><span class="sxs-lookup"><span data-stu-id="91852-264">53</span></span> |
| <span data-ttu-id="91852-265">1000</span><span class="sxs-lookup"><span data-stu-id="91852-265">1000</span></span> |<span data-ttu-id="91852-266">2535</span><span class="sxs-lookup"><span data-stu-id="91852-266">2535</span></span> |<span data-ttu-id="91852-267">341</span><span class="sxs-lookup"><span data-stu-id="91852-267">341</span></span> |
| <span data-ttu-id="91852-268">10000</span><span class="sxs-lookup"><span data-stu-id="91852-268">10000</span></span> |<span data-ttu-id="91852-269">21605</span><span class="sxs-lookup"><span data-stu-id="91852-269">21605</span></span> |<span data-ttu-id="91852-270">2737</span><span class="sxs-lookup"><span data-stu-id="91852-270">2737</span></span> |

> [!NOTE]
> <span data-ttu-id="91852-271">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="91852-271">Results are not benchmarks.</span></span> <span data-ttu-id="91852-272">Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="91852-272">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="91852-273">I mindre batch storlekar hello använda tabellvärdeparametrar gick bättre än förväntat hello **SqlBulkCopy körs** klass.</span><span class="sxs-lookup"><span data-stu-id="91852-273">In smaller batch sizes, hello use table-valued parameters outperformed hello **SqlBulkCopy** class.</span></span> <span data-ttu-id="91852-274">Dock **SqlBulkCopy körs** utförs 12-31% snabbare än tabellvärdeparametrar för hello test 1 000 och 10 000 rader.</span><span class="sxs-lookup"><span data-stu-id="91852-274">However, **SqlBulkCopy** performed 12-31% faster than table-valued parameters for hello tests of 1,000 and 10,000 rows.</span></span> <span data-ttu-id="91852-275">Som tabellvärdeparametrar, **SqlBulkCopy körs** är ett bra alternativ för gruppbaserad infogningar särskilt jämfört toohello prestanda för ett annat åtgärder.</span><span class="sxs-lookup"><span data-stu-id="91852-275">Like table-valued parameters, **SqlBulkCopy** is a good option for batched inserts, especially when compared toohello performance of non-batched operations.</span></span>

<span data-ttu-id="91852-276">Mer information om masskopiering i ADO.NET finns [Masskopieringsåtgärder i SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span><span class="sxs-lookup"><span data-stu-id="91852-276">For more information on bulk copy in ADO.NET, see [Bulk Copy Operations in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span></span>

### <a name="multiple-row-parameterized-insert-statements"></a><span data-ttu-id="91852-277">Flera rader som innehåller parametrar Infoga instruktioner</span><span class="sxs-lookup"><span data-stu-id="91852-277">Multiple-row Parameterized INSERT statements</span></span>
<span data-ttu-id="91852-278">Ett alternativ för små batchar är en stor tooconstruct parametriserade INSERT-instruktionen som infogar flera rader.</span><span class="sxs-lookup"><span data-stu-id="91852-278">One alternative for small batches is tooconstruct a large parameterized INSERT statement that inserts multiple rows.</span></span> <span data-ttu-id="91852-279">hello följande kodexempel visar den här tekniken.</span><span class="sxs-lookup"><span data-stu-id="91852-279">hello following code example demonstrates this technique.</span></span>

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


<span data-ttu-id="91852-280">Det här exemplet är avsedd tooshow hello grundläggande begrepp.</span><span class="sxs-lookup"><span data-stu-id="91852-280">This example is meant tooshow hello basic concept.</span></span> <span data-ttu-id="91852-281">En mer realistisk scenariot skulle gå igenom hello krävs entiteter tooconstruct hello frågesträngen och hello kommandoparametrarna samtidigt.</span><span class="sxs-lookup"><span data-stu-id="91852-281">A more realistic scenario would loop through hello required entities tooconstruct hello query string and hello command parameters simultaneously.</span></span> <span data-ttu-id="91852-282">Är du begränsad tooa totalt 2100 parametrar, så detta begränsar hello Totalt antal rader som kan bearbetas i det här sättet.</span><span class="sxs-lookup"><span data-stu-id="91852-282">You are limited tooa total of 2100 query parameters, so this limits hello total number of rows that can be processed in this manner.</span></span>

<span data-ttu-id="91852-283">hello följande ad hoc-Testa resultaten visar hello prestanda för den här typen av insert-instruktionen i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="91852-283">hello following ad-hoc test results show hello performance of this type of insert statement in milliseconds.</span></span>

| <span data-ttu-id="91852-284">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="91852-284">Operations</span></span> | <span data-ttu-id="91852-285">Tabellvärdeparametrar (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-285">Table-valued parameters (ms)</span></span> | <span data-ttu-id="91852-286">Infoga enstaka uttryck (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-286">Single-statement INSERT (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91852-287">1</span><span class="sxs-lookup"><span data-stu-id="91852-287">1</span></span> |<span data-ttu-id="91852-288">32</span><span class="sxs-lookup"><span data-stu-id="91852-288">32</span></span> |<span data-ttu-id="91852-289">20</span><span class="sxs-lookup"><span data-stu-id="91852-289">20</span></span> |
| <span data-ttu-id="91852-290">10</span><span class="sxs-lookup"><span data-stu-id="91852-290">10</span></span> |<span data-ttu-id="91852-291">30</span><span class="sxs-lookup"><span data-stu-id="91852-291">30</span></span> |<span data-ttu-id="91852-292">25</span><span class="sxs-lookup"><span data-stu-id="91852-292">25</span></span> |
| <span data-ttu-id="91852-293">100</span><span class="sxs-lookup"><span data-stu-id="91852-293">100</span></span> |<span data-ttu-id="91852-294">33</span><span class="sxs-lookup"><span data-stu-id="91852-294">33</span></span> |<span data-ttu-id="91852-295">51</span><span class="sxs-lookup"><span data-stu-id="91852-295">51</span></span> |

> [!NOTE]
> <span data-ttu-id="91852-296">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="91852-296">Results are not benchmarks.</span></span> <span data-ttu-id="91852-297">Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="91852-297">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="91852-298">Den här metoden kan vara något snabbare för batchar som är mindre än 100 rader.</span><span class="sxs-lookup"><span data-stu-id="91852-298">This approach can be slightly faster for batches that are less than 100 rows.</span></span> <span data-ttu-id="91852-299">Även om hello improvement är liten är tekniken ett annat alternativ som fungerar bra i ditt specifika program scenario.</span><span class="sxs-lookup"><span data-stu-id="91852-299">Although hello improvement is small, this technique is another option that might work well in your specific application scenario.</span></span>

### <a name="dataadapter"></a><span data-ttu-id="91852-300">DataAdapter</span><span class="sxs-lookup"><span data-stu-id="91852-300">DataAdapter</span></span>
<span data-ttu-id="91852-301">Hej **DataAdapter** klassen kan du toomodify en **DataSet** objektet och sedan skicka hello ändringar som INSERT, UPDATE och DELETE-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="91852-301">hello **DataAdapter** class allows you toomodify a **DataSet** object and then submit hello changes as INSERT, UPDATE, and DELETE operations.</span></span> <span data-ttu-id="91852-302">Om du använder hello **DataAdapter** i det här sättet är det viktigt toonote som avgränsar anrop görs för varje distinkta åtgärd.</span><span class="sxs-lookup"><span data-stu-id="91852-302">If you are using hello **DataAdapter** in this manner, it is important toonote that separate calls are made for each distinct operation.</span></span> <span data-ttu-id="91852-303">tooimprove prestanda, Använd hello **UpdateBatchSize** egenskapen toohello antal åtgärder som ska grupperas i taget.</span><span class="sxs-lookup"><span data-stu-id="91852-303">tooimprove performance, use hello **UpdateBatchSize** property toohello number of operations that should be batched at a time.</span></span> <span data-ttu-id="91852-304">Mer information finns i [utför Batch åtgärder med hjälp av DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span><span class="sxs-lookup"><span data-stu-id="91852-304">For more information, see [Performing Batch Operations Using DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span></span>

### <a name="entity-framework"></a><span data-ttu-id="91852-305">Entity framework</span><span class="sxs-lookup"><span data-stu-id="91852-305">Entity framework</span></span>
<span data-ttu-id="91852-306">Entity Framework stöder för närvarande inte batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="91852-306">Entity Framework does not currently support batching.</span></span> <span data-ttu-id="91852-307">Olika utvecklare i hello community har försökt toodemonstrate lösningar, till exempel åsidosättning hello **SaveChanges** metod.</span><span class="sxs-lookup"><span data-stu-id="91852-307">Different developers in hello community have attempted toodemonstrate workarounds, such as override hello **SaveChanges** method.</span></span> <span data-ttu-id="91852-308">Men hello lösningar är vanligtvis toohello komplexa och anpassade program och datamodellen.</span><span class="sxs-lookup"><span data-stu-id="91852-308">But hello solutions are typically complex and customized toohello application and data model.</span></span> <span data-ttu-id="91852-309">hello Entity Framework codeplex projekt har för närvarande en diskussionssida på den här funktionsbegäran.</span><span class="sxs-lookup"><span data-stu-id="91852-309">hello Entity Framework codeplex project currently has a discussion page on this feature request.</span></span> <span data-ttu-id="91852-310">tooview den här diskussionen finns [Design mötesanteckningar - 2 augusti 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span><span class="sxs-lookup"><span data-stu-id="91852-310">tooview this discussion, see [Design Meeting Notes - August 2, 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span></span>

### <a name="xml"></a><span data-ttu-id="91852-311">XML</span><span class="sxs-lookup"><span data-stu-id="91852-311">XML</span></span>
<span data-ttu-id="91852-312">För fullständighetens skull bör du att det är viktigt tootalk om XML-Datatypen som en batching strategi.</span><span class="sxs-lookup"><span data-stu-id="91852-312">For completeness, we feel that it is important tootalk about XML as a batching strategy.</span></span> <span data-ttu-id="91852-313">Hello användning av XML-har dock inga fördelar över andra metoder och flera nackdelar.</span><span class="sxs-lookup"><span data-stu-id="91852-313">However, hello use of XML has no advantages over other methods and several disadvantages.</span></span> <span data-ttu-id="91852-314">hello-metoden är liknande tootable enkelvärdesattribut parametrar, men ett XML-fil eller sträng har överförts tooa lagrade procedur i stället för en användardefinierad tabell.</span><span class="sxs-lookup"><span data-stu-id="91852-314">hello approach is similar tootable-valued parameters, but an XML file or string is passed tooa stored procedure instead of a user-defined table.</span></span> <span data-ttu-id="91852-315">hello lagrade proceduren Parsar hello kommandon i hello lagrad procedur.</span><span class="sxs-lookup"><span data-stu-id="91852-315">hello stored procedure parses hello commands in hello stored procedure.</span></span>

<span data-ttu-id="91852-316">Det finns flera nackdelar toothis metod:</span><span class="sxs-lookup"><span data-stu-id="91852-316">There are several disadvantages toothis approach:</span></span>

* <span data-ttu-id="91852-317">Arbeta med XML kan vara krånglig och tillförlitligt.</span><span class="sxs-lookup"><span data-stu-id="91852-317">Working with XML can be cumbersome and error prone.</span></span>
* <span data-ttu-id="91852-318">Tolkning hello XML för hello databasen kan vara Processorn märkbart.</span><span class="sxs-lookup"><span data-stu-id="91852-318">Parsing hello XML on hello database can be CPU-intensive.</span></span>
* <span data-ttu-id="91852-319">I de flesta fall är den här metoden går långsammare än om tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="91852-319">In most cases, this method is slower than table-valued parameters.</span></span>

<span data-ttu-id="91852-320">Därför rekommenderas inte hello användning av XML för batch-frågor.</span><span class="sxs-lookup"><span data-stu-id="91852-320">For these reasons, hello use of XML for batch queries is not recommended.</span></span>

## <a name="batching-considerations"></a><span data-ttu-id="91852-321">Batchbearbetning överväganden</span><span class="sxs-lookup"><span data-stu-id="91852-321">Batching considerations</span></span>
<span data-ttu-id="91852-322">hello följande avsnitt ge mer vägledning för hello användning av batchbearbetning i SQL Database-program.</span><span class="sxs-lookup"><span data-stu-id="91852-322">hello following sections provide more guidance for hello use of batching in SQL Database applications.</span></span>

### <a name="tradeoffs"></a><span data-ttu-id="91852-323">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="91852-323">Tradeoffs</span></span>
<span data-ttu-id="91852-324">Beroende på din arkitektur involverar batchbearbetning en kompromiss mellan prestanda och återhämtning.</span><span class="sxs-lookup"><span data-stu-id="91852-324">Depending on your architecture, batching can involve a tradeoff between performance and resiliency.</span></span> <span data-ttu-id="91852-325">Anta exempelvis att hello scenario där din roll oväntat stängs av.</span><span class="sxs-lookup"><span data-stu-id="91852-325">For example, consider hello scenario where your role unexpectedly goes down.</span></span> <span data-ttu-id="91852-326">Om du tappar bort en rad med data är hello påverkan mindre än hello effekten av att förlora en stor grupp med rader som inte har skickats.</span><span class="sxs-lookup"><span data-stu-id="91852-326">If you lose one row of data, hello impact is smaller than hello impact of losing a large batch of unsubmitted rows.</span></span> <span data-ttu-id="91852-327">Det finns en större risk när du buffert rader innan de skickas toohello databasen i ett angivet tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="91852-327">There is a greater risk when you buffer rows before sending them toohello database in a specified time window.</span></span>

<span data-ttu-id="91852-328">På grund av det här förhållandet utvärdera hello typ av åtgärder som du batch.</span><span class="sxs-lookup"><span data-stu-id="91852-328">Because of this tradeoff, evaluate hello type of operations that you batch.</span></span> <span data-ttu-id="91852-329">Batch mer aggressivt (större batchar och längre tidsfönster) med data som är mindre viktigt.</span><span class="sxs-lookup"><span data-stu-id="91852-329">Batch more aggressively (larger batches and longer time windows) with data that is less critical.</span></span>

### <a name="batch-size"></a><span data-ttu-id="91852-330">Batchstorlek</span><span class="sxs-lookup"><span data-stu-id="91852-330">Batch size</span></span>
<span data-ttu-id="91852-331">I våra tester fanns det vanligtvis ingen fördel toobreaking stora batchar i mindre delar.</span><span class="sxs-lookup"><span data-stu-id="91852-331">In our tests, there was typically no advantage toobreaking large batches into smaller chunks.</span></span> <span data-ttu-id="91852-332">Faktum är resulterade ofta den här delfältet i långsammare än att skicka en enda stor grupp.</span><span class="sxs-lookup"><span data-stu-id="91852-332">In fact, this subdivision often resulted in slower performance than submitting a single large batch.</span></span> <span data-ttu-id="91852-333">Tänk dig ett scenario där du vill att tooinsert 1000 översta raderna.</span><span class="sxs-lookup"><span data-stu-id="91852-333">For example, consider a scenario where you want tooinsert 1000 rows.</span></span> <span data-ttu-id="91852-334">hello följande tabell visar hur lång tid det tar toouse tabellvärdeparametrar tooinsert 1000 rader när indelat i mindre batchar.</span><span class="sxs-lookup"><span data-stu-id="91852-334">hello following table shows how long it takes toouse table-valued parameters tooinsert 1000 rows when divided into smaller batches.</span></span>

| <span data-ttu-id="91852-335">Batchstorlek</span><span class="sxs-lookup"><span data-stu-id="91852-335">Batch size</span></span> | <span data-ttu-id="91852-336">Upprepningar</span><span class="sxs-lookup"><span data-stu-id="91852-336">Iterations</span></span> | <span data-ttu-id="91852-337">Tabellvärdeparametrar (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-337">Table-valued parameters (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="91852-338">1000</span><span class="sxs-lookup"><span data-stu-id="91852-338">1000</span></span> |<span data-ttu-id="91852-339">1</span><span class="sxs-lookup"><span data-stu-id="91852-339">1</span></span> |<span data-ttu-id="91852-340">347</span><span class="sxs-lookup"><span data-stu-id="91852-340">347</span></span> |
| <span data-ttu-id="91852-341">500</span><span class="sxs-lookup"><span data-stu-id="91852-341">500</span></span> |<span data-ttu-id="91852-342">2</span><span class="sxs-lookup"><span data-stu-id="91852-342">2</span></span> |<span data-ttu-id="91852-343">355</span><span class="sxs-lookup"><span data-stu-id="91852-343">355</span></span> |
| <span data-ttu-id="91852-344">100</span><span class="sxs-lookup"><span data-stu-id="91852-344">100</span></span> |<span data-ttu-id="91852-345">10</span><span class="sxs-lookup"><span data-stu-id="91852-345">10</span></span> |<span data-ttu-id="91852-346">465</span><span class="sxs-lookup"><span data-stu-id="91852-346">465</span></span> |
| <span data-ttu-id="91852-347">50</span><span class="sxs-lookup"><span data-stu-id="91852-347">50</span></span> |<span data-ttu-id="91852-348">20</span><span class="sxs-lookup"><span data-stu-id="91852-348">20</span></span> |<span data-ttu-id="91852-349">630</span><span class="sxs-lookup"><span data-stu-id="91852-349">630</span></span> |

> [!NOTE]
> <span data-ttu-id="91852-350">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="91852-350">Results are not benchmarks.</span></span> <span data-ttu-id="91852-351">Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="91852-351">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="91852-352">Du kan se att hello bästa prestanda för 1000 översta raderna är toosubmit alla samtidigt.</span><span class="sxs-lookup"><span data-stu-id="91852-352">You can see that hello best performance for 1000 rows is toosubmit them all at once.</span></span> <span data-ttu-id="91852-353">I andra tester (visas inte här) uppstod en liten prestanda vinst toobreak en 10000 raden batch i två grupper om 5000.</span><span class="sxs-lookup"><span data-stu-id="91852-353">In other tests (not shown here) there was a small performance gain toobreak a 10000 row batch into two batches of 5000.</span></span> <span data-ttu-id="91852-354">Men hello tabellschemat för dessa tester är relativt enkla, så du bör utföra tester på specifika data och batch storlekar tooverify dessa resultat.</span><span class="sxs-lookup"><span data-stu-id="91852-354">But hello table schema for these tests is relatively simple, so you should perform tests on your specific data and batch sizes tooverify these findings.</span></span>

<span data-ttu-id="91852-355">En annan faktor tooconsider är att om hello totala batch blir för stor, SQL-databas kan begränsa refusera toocommit hello batch.</span><span class="sxs-lookup"><span data-stu-id="91852-355">Another factor tooconsider is that if hello total batch becomes too large, SQL Database might throttle and refuse toocommit hello batch.</span></span> <span data-ttu-id="91852-356">Testa din situation toodetermine hello resultatet blir bäst om det finns en perfekt batchstorlek.</span><span class="sxs-lookup"><span data-stu-id="91852-356">For hello best results, test your specific scenario toodetermine if there is an ideal batch size.</span></span> <span data-ttu-id="91852-357">Gör hello batchstorlek konfigureras vid körning tooenable snabba justeringar utifrån prestanda eller fel.</span><span class="sxs-lookup"><span data-stu-id="91852-357">Make hello batch size configurable at runtime tooenable quick adjustments based on performance or errors.</span></span>

<span data-ttu-id="91852-358">Slutligen balansera hello storleken på hello batch med hello risker associerade med batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="91852-358">Finally, balance hello size of hello batch with hello risks associated with batching.</span></span> <span data-ttu-id="91852-359">Om det finns tillfälliga fel eller hello rollen misslyckas, Överväg hello konsekvenserna av du försöker hello åtgärden eller hello dataförlust i hello batch.</span><span class="sxs-lookup"><span data-stu-id="91852-359">If there are transient errors or hello role fails, consider hello consequences of retrying hello operation or of losing hello data in hello batch.</span></span>

### <a name="parallel-processing"></a><span data-ttu-id="91852-360">Parallell bearbetning</span><span class="sxs-lookup"><span data-stu-id="91852-360">Parallel processing</span></span>
<span data-ttu-id="91852-361">Vad händer om du tog hello-metoden för att minska hello batchstorlek men används för flera trådar tooexecute hello arbete?</span><span class="sxs-lookup"><span data-stu-id="91852-361">What if you took hello approach of reducing hello batch size but used multiple threads tooexecute hello work?</span></span> <span data-ttu-id="91852-362">Våra tester visade igen att flera mindre flertrådade batchar vanligtvis utförs värre än en enskild större batch.</span><span class="sxs-lookup"><span data-stu-id="91852-362">Again, our tests showed that several smaller multithreaded batches typically performed worse than a single larger batch.</span></span> <span data-ttu-id="91852-363">hello försöker följande test tooinsert 1000 rader i en eller flera parallella batchar.</span><span class="sxs-lookup"><span data-stu-id="91852-363">hello following test attempts tooinsert 1000 rows in one or more parallel batches.</span></span> <span data-ttu-id="91852-364">Det här testet visar hur flera samtidiga batchar faktiskt minskade prestanda.</span><span class="sxs-lookup"><span data-stu-id="91852-364">This test shows how more simultaneous batches actually decreased performance.</span></span>

| <span data-ttu-id="91852-365">Batchstorlek [iterationer]</span><span class="sxs-lookup"><span data-stu-id="91852-365">Batch size [Iterations]</span></span> | <span data-ttu-id="91852-366">Två trådar (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-366">Two threads (ms)</span></span> | <span data-ttu-id="91852-367">Fyra trådar (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-367">Four threads (ms)</span></span> | <span data-ttu-id="91852-368">Sex trådar (ms)</span><span class="sxs-lookup"><span data-stu-id="91852-368">Six threads (ms)</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="91852-369">1000 [1]</span><span class="sxs-lookup"><span data-stu-id="91852-369">1000 [1]</span></span> |<span data-ttu-id="91852-370">277</span><span class="sxs-lookup"><span data-stu-id="91852-370">277</span></span> |<span data-ttu-id="91852-371">315</span><span class="sxs-lookup"><span data-stu-id="91852-371">315</span></span> |<span data-ttu-id="91852-372">266</span><span class="sxs-lookup"><span data-stu-id="91852-372">266</span></span> |
| <span data-ttu-id="91852-373">500 [2]</span><span class="sxs-lookup"><span data-stu-id="91852-373">500 [2]</span></span> |<span data-ttu-id="91852-374">548</span><span class="sxs-lookup"><span data-stu-id="91852-374">548</span></span> |<span data-ttu-id="91852-375">278</span><span class="sxs-lookup"><span data-stu-id="91852-375">278</span></span> |<span data-ttu-id="91852-376">256</span><span class="sxs-lookup"><span data-stu-id="91852-376">256</span></span> |
| <span data-ttu-id="91852-377">250 [4]</span><span class="sxs-lookup"><span data-stu-id="91852-377">250 [4]</span></span> |<span data-ttu-id="91852-378">405</span><span class="sxs-lookup"><span data-stu-id="91852-378">405</span></span> |<span data-ttu-id="91852-379">329</span><span class="sxs-lookup"><span data-stu-id="91852-379">329</span></span> |<span data-ttu-id="91852-380">265</span><span class="sxs-lookup"><span data-stu-id="91852-380">265</span></span> |
| <span data-ttu-id="91852-381">100 [10]</span><span class="sxs-lookup"><span data-stu-id="91852-381">100 [10]</span></span> |<span data-ttu-id="91852-382">488</span><span class="sxs-lookup"><span data-stu-id="91852-382">488</span></span> |<span data-ttu-id="91852-383">439</span><span class="sxs-lookup"><span data-stu-id="91852-383">439</span></span> |<span data-ttu-id="91852-384">391</span><span class="sxs-lookup"><span data-stu-id="91852-384">391</span></span> |

> [!NOTE]
> <span data-ttu-id="91852-385">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="91852-385">Results are not benchmarks.</span></span> <span data-ttu-id="91852-386">Se hello [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="91852-386">See hello [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="91852-387">Det finns flera möjliga orsaker till hello sämre prestanda på grund av tooparallelism:</span><span class="sxs-lookup"><span data-stu-id="91852-387">There are several potential reasons for hello degradation in performance due tooparallelism:</span></span>

* <span data-ttu-id="91852-388">Det finns flera samtidiga nätverket anrop i stället för en.</span><span class="sxs-lookup"><span data-stu-id="91852-388">There are multiple simultaneous network calls instead of one.</span></span>
* <span data-ttu-id="91852-389">Flera åtgärder mot en tabell kan resultera i konkurrens och blockerar.</span><span class="sxs-lookup"><span data-stu-id="91852-389">Multiple operations against a single table can result in contention and blocking.</span></span>
* <span data-ttu-id="91852-390">Det finns kostnader som flertrådsteknik.</span><span class="sxs-lookup"><span data-stu-id="91852-390">There are overheads associated with multithreading.</span></span>
* <span data-ttu-id="91852-391">hello kostnaden för att öppna flera anslutningar uppväger hello fördelen parallell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="91852-391">hello expense of opening multiple connections outweighs hello benefit of parallel processing.</span></span>

<span data-ttu-id="91852-392">Om du anpassar olika tabeller eller databaser, är det möjligt toosee vissa prestandaförbättring med den här strategin.</span><span class="sxs-lookup"><span data-stu-id="91852-392">If you target different tables or databases, it is possible toosee some performance gain with this strategy.</span></span> <span data-ttu-id="91852-393">Horisontell partitionering databasen eller federationer är ett scenario för den här metoden.</span><span class="sxs-lookup"><span data-stu-id="91852-393">Database sharding or federations would be a scenario for this approach.</span></span> <span data-ttu-id="91852-394">Horisontell partitionering använder flera databaser och vägar olika tooeach databas.</span><span class="sxs-lookup"><span data-stu-id="91852-394">Sharding uses multiple databases and routes different data tooeach database.</span></span> <span data-ttu-id="91852-395">Om varje liten grupp ska tooa annan databas, kan utför sedan hello åtgärder parallellt vara mer effektivt.</span><span class="sxs-lookup"><span data-stu-id="91852-395">If each small batch is going tooa different database, then performing hello operations in parallel can be more efficient.</span></span> <span data-ttu-id="91852-396">Hello prestandafördelar är dock inte tillräckligt betydande toouse hello utgångspunkt för beslut toouse databasen delning i din lösning.</span><span class="sxs-lookup"><span data-stu-id="91852-396">However, hello performance gain is not significant enough toouse as hello basis for a decision toouse database sharding in your solution.</span></span>

<span data-ttu-id="91852-397">I vissa Designer leda parallell körning av mindre batchar till bättre genomflöde begäranden i ett system under belastning.</span><span class="sxs-lookup"><span data-stu-id="91852-397">In some designs, parallel execution of smaller batches can result in improved throughput of requests in a system under load.</span></span> <span data-ttu-id="91852-398">I det här fallet trots att det är snabbare tooprocess en enskild större batch kan bearbeta flera batchar parallellt vara mer effektivt.</span><span class="sxs-lookup"><span data-stu-id="91852-398">In this case, even though it is quicker tooprocess a single larger batch, processing multiple batches in parallel might be more efficient.</span></span>

<span data-ttu-id="91852-399">Om du använder parallell körning kan du styra hello maximalt antal arbetstrådar.</span><span class="sxs-lookup"><span data-stu-id="91852-399">If you do use parallel execution, consider controlling hello maximum number of worker threads.</span></span> <span data-ttu-id="91852-400">Ett mindre antal leda till mindre konkurrens och snabbare körningstid.</span><span class="sxs-lookup"><span data-stu-id="91852-400">A smaller number might result in less contention and a faster execution time.</span></span> <span data-ttu-id="91852-401">Du kan också hello belastning som placeras på hello måldatabasen både anslutningar och transaktioner.</span><span class="sxs-lookup"><span data-stu-id="91852-401">Also, consider hello additional load that this places on hello target database both in connections and transactions.</span></span>

### <a name="related-performance-factors"></a><span data-ttu-id="91852-402">Prestandadata relaterade faktorer</span><span class="sxs-lookup"><span data-stu-id="91852-402">Related performance factors</span></span>
<span data-ttu-id="91852-403">Vanliga vägledning om databasprestanda påverkar också batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="91852-403">Typical guidance on database performance also affects batching.</span></span> <span data-ttu-id="91852-404">Till exempel infoga minskar prestanda för tabeller som har en primärnyckel för stora eller många icke-grupperat index.</span><span class="sxs-lookup"><span data-stu-id="91852-404">For example, insert performance is reduced for tables that have a large primary key or many nonclustered indexes.</span></span>

<span data-ttu-id="91852-405">Om tabellvärdeparametrar använder en lagrad procedur, kan du använda kommandot hello **SET NOCOUNT ON** hello början av hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="91852-405">If table-valued parameters use a stored procedure, you can use hello command **SET NOCOUNT ON** at hello beginning of hello procedure.</span></span> <span data-ttu-id="91852-406">Den här instruktionen Undertrycker hello returnera hello antal hello påverkas rader i hello proceduren.</span><span class="sxs-lookup"><span data-stu-id="91852-406">This statement suppresses hello return of hello count of hello affected rows in hello procedure.</span></span> <span data-ttu-id="91852-407">Men i våra tester hello användning av **SET NOCOUNT ON** inte påverkar eller minskade prestanda.</span><span class="sxs-lookup"><span data-stu-id="91852-407">However, in our tests, hello use of **SET NOCOUNT ON** either had no effect or decreased performance.</span></span> <span data-ttu-id="91852-408">hello test lagrade proceduren har enkelt med en enda **infoga** från hello tabellvärdesparametern.</span><span class="sxs-lookup"><span data-stu-id="91852-408">hello test stored procedure was simple with a single **INSERT** command from hello table-valued parameter.</span></span> <span data-ttu-id="91852-409">Det är möjligt att mer komplexa lagrade procedurer skulle dra nytta av den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="91852-409">It is possible that more complex stored procedures would benefit from this statement.</span></span> <span data-ttu-id="91852-410">Men förutsätt inte att lägga till **SET NOCOUNT ON** tooyour lagrade proceduren automatiskt förbättrar prestanda.</span><span class="sxs-lookup"><span data-stu-id="91852-410">But don’t assume that adding **SET NOCOUNT ON** tooyour stored procedure automatically improves performance.</span></span> <span data-ttu-id="91852-411">toounderstand hello effekt, testa den lagrade proceduren med och utan hello **SET NOCOUNT ON** instruktionen.</span><span class="sxs-lookup"><span data-stu-id="91852-411">toounderstand hello effect, test your stored procedure with and without hello **SET NOCOUNT ON** statement.</span></span>

## <a name="batching-scenarios"></a><span data-ttu-id="91852-412">Batchbearbetning scenarier</span><span class="sxs-lookup"><span data-stu-id="91852-412">Batching scenarios</span></span>
<span data-ttu-id="91852-413">hello följande avsnitt beskrivs hur toouse tabellvärdeparametrar i tre scenarier för programmet.</span><span class="sxs-lookup"><span data-stu-id="91852-413">hello following sections describe how toouse table-valued parameters in three application scenarios.</span></span> <span data-ttu-id="91852-414">hello första scenariot visar hur buffring och batchbearbetning kan fungera tillsammans.</span><span class="sxs-lookup"><span data-stu-id="91852-414">hello first scenario shows how buffering and batching can work together.</span></span> <span data-ttu-id="91852-415">hello andra scenariot förbättrar prestanda genom att utföra åtgärder för master-detaljer i en enda lagrade proceduranropet.</span><span class="sxs-lookup"><span data-stu-id="91852-415">hello second scenario improves performance by performing master-detail operations in a single stored procedure call.</span></span> <span data-ttu-id="91852-416">Hej sista scenariot visar hur toouse tabellvärdeparametrar i en ”UPSERT”-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="91852-416">hello final scenario shows how toouse table-valued parameters in an “UPSERT” operation.</span></span>

### <a name="buffering"></a><span data-ttu-id="91852-417">Buffring</span><span class="sxs-lookup"><span data-stu-id="91852-417">Buffering</span></span>
<span data-ttu-id="91852-418">Men det är några scenarier som är uppenbara kandidat för batchbearbetning, finns det många scenarier som kan dra nytta av batchbearbetning av fördröjd bearbetning.</span><span class="sxs-lookup"><span data-stu-id="91852-418">Although there are some scenarios that are obvious candidate for batching, there are many scenarios that could take advantage of batching by delayed processing.</span></span> <span data-ttu-id="91852-419">Dock innebär också fördröjd bearbetning en större risk hello data förloras i hello händelse av ett oväntat fel.</span><span class="sxs-lookup"><span data-stu-id="91852-419">However, delayed processing also carries a greater risk that hello data is lost in hello event of an unexpected failure.</span></span> <span data-ttu-id="91852-420">Det är viktigt toounderstand risken och Överväg hello konsekvenser.</span><span class="sxs-lookup"><span data-stu-id="91852-420">It is important toounderstand this risk and consider hello consequences.</span></span>

<span data-ttu-id="91852-421">Tänk dig ett webbprogram som spårar hello historik för varje användare.</span><span class="sxs-lookup"><span data-stu-id="91852-421">For example, consider a web application that tracks hello navigation history of each user.</span></span> <span data-ttu-id="91852-422">Vid varje sidbegäran gör hello programmet databasen anropet toorecord hello användarens sidvisningen.</span><span class="sxs-lookup"><span data-stu-id="91852-422">On each page request, hello application could make a database call toorecord hello user’s page view.</span></span> <span data-ttu-id="91852-423">Men högre prestanda och skalbarhet kan uppnås genom buffring hello användarnas navigering aktiviteter och sedan skickar data toohello databasen i batchar.</span><span class="sxs-lookup"><span data-stu-id="91852-423">But higher performance and scalability can be achieved by buffering hello users’ navigation activities and then sending this data toohello database in batches.</span></span> <span data-ttu-id="91852-424">Du kan utlösa hello database-uppdateringen av förfluten tid och/eller buffertstorlek.</span><span class="sxs-lookup"><span data-stu-id="91852-424">You can trigger hello database update by elapsed time and/or buffer size.</span></span> <span data-ttu-id="91852-425">En regel kan till exempel ange att hello batch ska bearbetas efter 20 sekunder eller när hello buffert når 1 000 objekt.</span><span class="sxs-lookup"><span data-stu-id="91852-425">For example, a rule could specify that hello batch should be processed after 20 seconds or when hello buffer reaches 1000 items.</span></span>

<span data-ttu-id="91852-426">hello följande kodexempel används [reaktiv tillägg - Rx](https://msdn.microsoft.com/data/gg577609) tooprocess buffras händelser som skapats av en övervakningsklass.</span><span class="sxs-lookup"><span data-stu-id="91852-426">hello following code example uses [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) tooprocess buffered events raised by a monitoring class.</span></span> <span data-ttu-id="91852-427">Hello när buffert fyllning eller en tidsgräns uppnås, hello batch användardata skickas toohello databasen med en tabellvärdesparameter.</span><span class="sxs-lookup"><span data-stu-id="91852-427">When hello buffer fills or a timeout is reached, hello batch of user data is sent toohello database with a table-valued parameter.</span></span>

<span data-ttu-id="91852-428">hello följande NavHistoryData klassen modeller hello navigering användarinformation.</span><span class="sxs-lookup"><span data-stu-id="91852-428">hello following NavHistoryData class models hello user navigation details.</span></span> <span data-ttu-id="91852-429">Det innehåller grundläggande information, till exempel hello användar-ID, hello URL: en används och hello åtkomsttid.</span><span class="sxs-lookup"><span data-stu-id="91852-429">It contains basic information such as hello user identifier, hello URL accessed, and hello access time.</span></span>

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

<span data-ttu-id="91852-430">Hej NavHistoryDataMonitor klass är ansvarig för buffring hello navigering data toohello användardatabas.</span><span class="sxs-lookup"><span data-stu-id="91852-430">hello NavHistoryDataMonitor class is responsible for buffering hello user navigation data toohello database.</span></span> <span data-ttu-id="91852-431">Den innehåller en metod, RecordUserNavigationEntry som svarar genom att en **OnAdded** händelse.</span><span class="sxs-lookup"><span data-stu-id="91852-431">It contains a method, RecordUserNavigationEntry, which responds by raising an **OnAdded** event.</span></span> <span data-ttu-id="91852-432">hello följande kod visar hello konstruktorn logik som använder Rx toocreate en synliga samling baserat på hello-händelse.</span><span class="sxs-lookup"><span data-stu-id="91852-432">hello following code shows hello constructor logic that uses Rx toocreate an observable collection based on hello event.</span></span> <span data-ttu-id="91852-433">Den prenumererar sedan toothis synliga samling med hello buffert metod.</span><span class="sxs-lookup"><span data-stu-id="91852-433">It then subscribes toothis observable collection with hello Buffer method.</span></span> <span data-ttu-id="91852-434">hello överlagring anger att hello bufferten ska skickas varje 20 sekunder eller 1000 poster.</span><span class="sxs-lookup"><span data-stu-id="91852-434">hello overload specifies that hello buffer should be sent every 20 seconds or 1000 entries.</span></span>

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

<span data-ttu-id="91852-435">hello handler konverterar alla hello buffras objekt till en tabellvärdesfunktion typ och skickar sedan den här typen tooa lagrade proceduren som processer hello batch.</span><span class="sxs-lookup"><span data-stu-id="91852-435">hello handler converts all of hello buffered items into a table-valued type and then passes this type tooa stored procedure that processes hello batch.</span></span> <span data-ttu-id="91852-436">hello visar följande kod hello klar definition för både hello NavHistoryDataEventArgs och hello NavHistoryDataMonitor klasser.</span><span class="sxs-lookup"><span data-stu-id="91852-436">hello following code shows hello complete definition for both hello NavHistoryDataEventArgs and hello NavHistoryDataMonitor classes.</span></span>

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

<span data-ttu-id="91852-437">toouse denna buffring klass hello program skapar en statisk NavHistoryDataMonitor-objekt.</span><span class="sxs-lookup"><span data-stu-id="91852-437">toouse this buffering class, hello application creates a static NavHistoryDataMonitor object.</span></span> <span data-ttu-id="91852-438">Varje gång en användare ansluter till en sida hello programmet anropar hello NavHistoryDataMonitor.RecordUserNavigationEntry metod.</span><span class="sxs-lookup"><span data-stu-id="91852-438">Each time a user accesses a page, hello application calls hello NavHistoryDataMonitor.RecordUserNavigationEntry method.</span></span> <span data-ttu-id="91852-439">hello buffring logik fortsätter tootake vård skicka dessa poster toohello databasen i batchar.</span><span class="sxs-lookup"><span data-stu-id="91852-439">hello buffering logic proceeds tootake care of sending these entries toohello database in batches.</span></span>

### <a name="master-detail"></a><span data-ttu-id="91852-440">Master detaljer</span><span class="sxs-lookup"><span data-stu-id="91852-440">Master detail</span></span>
<span data-ttu-id="91852-441">Tabellvärdeparametrar är användbara för enkla INSERT-scenarier.</span><span class="sxs-lookup"><span data-stu-id="91852-441">Table-valued parameters are useful for simple INSERT scenarios.</span></span> <span data-ttu-id="91852-442">Det kan dock vara mer utmanande toobatch infogningar som rör fler än en tabell.</span><span class="sxs-lookup"><span data-stu-id="91852-442">However, it can be more challenging toobatch inserts that involve more than one table.</span></span> <span data-ttu-id="91852-443">Hej ”översikt/detaljer” scenario är ett bra exempel.</span><span class="sxs-lookup"><span data-stu-id="91852-443">hello “master/detail” scenario is a good example.</span></span> <span data-ttu-id="91852-444">hello huvudtabellen identifierar hello primära enhet.</span><span class="sxs-lookup"><span data-stu-id="91852-444">hello master table identifies hello primary entity.</span></span> <span data-ttu-id="91852-445">En eller flera tabeller i detalj lagra mer data om hello entitet.</span><span class="sxs-lookup"><span data-stu-id="91852-445">One or more detail tables store more data about hello entity.</span></span> <span data-ttu-id="91852-446">I det här scenariot framtvinga sekundärnyckelrelationer hello relationen mellan information tooa unika master entitet.</span><span class="sxs-lookup"><span data-stu-id="91852-446">In this scenario, foreign key relationships enforce hello relationship of details tooa unique master entity.</span></span> <span data-ttu-id="91852-447">Överväg att en förenklad version av PurchaseOrder tabellerna och dess associerade OrderDetail.</span><span class="sxs-lookup"><span data-stu-id="91852-447">Consider a simplified version of a PurchaseOrder table and its associated OrderDetail table.</span></span> <span data-ttu-id="91852-448">hello följande Transact-SQL skapar hello PurchaseOrder tabell med fyra kolumner: OrderID, OrderDate, CustomerID och Status.</span><span class="sxs-lookup"><span data-stu-id="91852-448">hello following Transact-SQL creates hello PurchaseOrder table with four columns: OrderID, OrderDate, CustomerID, and Status.</span></span>

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

<span data-ttu-id="91852-449">Varje innehåller en eller flera produktinköp.</span><span class="sxs-lookup"><span data-stu-id="91852-449">Each order contains one or more product purchases.</span></span> <span data-ttu-id="91852-450">Den här informationen samlas i hello PurchaseOrderDetail tabell.</span><span class="sxs-lookup"><span data-stu-id="91852-450">This information is captured in hello PurchaseOrderDetail table.</span></span> <span data-ttu-id="91852-451">hello följande Transact-SQL skapar hello PurchaseOrderDetail tabell med fem kolumner: OrderID, OrderDetailID, ProductID, Enhetspris och OrderQty.</span><span class="sxs-lookup"><span data-stu-id="91852-451">hello following Transact-SQL creates hello PurchaseOrderDetail table with five columns: OrderID, OrderDetailID, ProductID, UnitPrice, and OrderQty.</span></span>

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

<span data-ttu-id="91852-452">hello OrderID kolumn i hello PurchaseOrderDetail tabell måste referera till en order från hello PurchaseOrder tabell.</span><span class="sxs-lookup"><span data-stu-id="91852-452">hello OrderID column in hello PurchaseOrderDetail table must reference an order from hello PurchaseOrder table.</span></span> <span data-ttu-id="91852-453">hello definitionen av en sekundärnyckel tillämpar den här begränsningen.</span><span class="sxs-lookup"><span data-stu-id="91852-453">hello following definition of a foreign key enforces this constraint.</span></span>

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

<span data-ttu-id="91852-454">I ordning toouse tabellvärdeparametrar, måste du ha en användardefinierad tabelltyp för varje måltabellen.</span><span class="sxs-lookup"><span data-stu-id="91852-454">In order toouse table-valued parameters, you must have one user-defined table type for each target table.</span></span>

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

<span data-ttu-id="91852-455">Definiera en lagrad procedur som accepterar tabeller för dessa typer.</span><span class="sxs-lookup"><span data-stu-id="91852-455">Then define a stored procedure that accepts tables of these types.</span></span> <span data-ttu-id="91852-456">Den här proceduren kan ett program toolocally batch en uppsättning order och orderinformationen i ett enda anrop.</span><span class="sxs-lookup"><span data-stu-id="91852-456">This procedure allows an application toolocally batch a set of orders and order details in a single call.</span></span> <span data-ttu-id="91852-457">hello ger följande Transact-SQL hello fullständig lagrade procedursdeklaration för det här köpet ordning exemplet.</span><span class="sxs-lookup"><span data-stu-id="91852-457">hello following Transact-SQL provides hello complete stored procedure declaration for this purchase order example.</span></span>

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

<span data-ttu-id="91852-458">I det här exemplet hello lokalt definierade @IdentityLink tabellen lagras hello faktiska OrderID värden från hello nyligen infogade rader.</span><span class="sxs-lookup"><span data-stu-id="91852-458">In this example, hello locally defined @IdentityLink table stores hello actual OrderID values from hello newly inserted rows.</span></span> <span data-ttu-id="91852-459">Dessa identifierare ordning skiljer sig från hello tillfälliga OrderID värden i hello @orders och @details tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="91852-459">These order identifiers are different from hello temporary OrderID values in hello @orders and @details table-valued parameters.</span></span> <span data-ttu-id="91852-460">Därför hello @IdentityLink tabell ansluter sedan hello OrderID värden från hello @orders toohello verkliga OrderID parametervärden för hello nya rader i hello PurchaseOrder tabell.</span><span class="sxs-lookup"><span data-stu-id="91852-460">For this reason, hello @IdentityLink table then connects hello OrderID values from hello @orders parameter toohello real OrderID values for hello new rows in hello PurchaseOrder table.</span></span> <span data-ttu-id="91852-461">Efter det här steget hello @IdentityLink tabellen kan underlätta Infoga hello orderinformationen med hello faktiska OrderID som uppfyller hello foreign key-begränsningen.</span><span class="sxs-lookup"><span data-stu-id="91852-461">After this step, hello @IdentityLink table can facilitate inserting hello order details with hello actual OrderID that satisfies hello foreign key constraint.</span></span>

<span data-ttu-id="91852-462">Den här lagrade proceduren kan användas från kod eller andra Transact-SQL-anrop.</span><span class="sxs-lookup"><span data-stu-id="91852-462">This stored procedure can be used from code or from other Transact-SQL calls.</span></span> <span data-ttu-id="91852-463">Avsnittet hello tabellvärdeparametrar i det här dokumentet som ett exempel.</span><span class="sxs-lookup"><span data-stu-id="91852-463">See hello table-valued parameters section of this paper for a code example.</span></span> <span data-ttu-id="91852-464">hello följande Transact-SQL visar hur toocall hello sp_InsertOrdersBatch.</span><span class="sxs-lookup"><span data-stu-id="91852-464">hello following Transact-SQL shows how toocall hello sp_InsertOrdersBatch.</span></span>

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

<span data-ttu-id="91852-465">Den här lösningen kan varje batch toouse en uppsättning OrderID värden som börjar vid 1.</span><span class="sxs-lookup"><span data-stu-id="91852-465">This solution allows each batch toouse a set of OrderID values that begin at 1.</span></span> <span data-ttu-id="91852-466">Värdena för tillfälliga OrderID beskriver hello relationer i hello batch men hello faktiska OrderID värdena bestäms när hello hello insert-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="91852-466">These temporary OrderID values describe hello relationships in hello batch, but hello actual OrderID values are determined at hello time of hello insert operation.</span></span> <span data-ttu-id="91852-467">Du kan köra hello samma instruktioner i hello föregående exempel upprepade gånger och skapa unika order i hello-databasen.</span><span class="sxs-lookup"><span data-stu-id="91852-467">You can run hello same statements in hello previous example repeatedly and generate unique orders in hello database.</span></span> <span data-ttu-id="91852-468">Överväg att lägga till mer kod eller databasen logik som förhindrar att duplicerade order när du använder detta batchbearbetning tekniken därför.</span><span class="sxs-lookup"><span data-stu-id="91852-468">For this reason, consider adding more code or database logic that prevents duplicate orders when using this batching technique.</span></span>

<span data-ttu-id="91852-469">Det här exemplet visar att ännu mer komplexa databasåtgärder, till exempel översikt detaljer operations kan grupperas med tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="91852-469">This example demonstrates that even more complex database operations, such as master-detail operations, can be batched using table-valued parameters.</span></span>

### <a name="upsert"></a><span data-ttu-id="91852-470">UPSERT</span><span class="sxs-lookup"><span data-stu-id="91852-470">UPSERT</span></span>
<span data-ttu-id="91852-471">En annan batching scenariet inbegriper samtidigt uppdaterar befintliga rader och infoga nya rader.</span><span class="sxs-lookup"><span data-stu-id="91852-471">Another batching scenario involves simultaneously updating existing rows and inserting new rows.</span></span> <span data-ttu-id="91852-472">Denna åtgärd är ibland hänvisade tooas en ”UPSERT” (update + insert)-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="91852-472">This operation is sometimes referred tooas an “UPSERT” (update + insert) operation.</span></span> <span data-ttu-id="91852-473">I stället för att göra anrop tooINSERT och uppdatera är hello MERGE-instruktion bäst lämpade toothis uppgift.</span><span class="sxs-lookup"><span data-stu-id="91852-473">Rather than making separate calls tooINSERT and UPDATE, hello MERGE statement is best suited toothis task.</span></span> <span data-ttu-id="91852-474">hello MERGE-instruktion kan utföra både insert och uppdatera åtgärder i ett enda anrop.</span><span class="sxs-lookup"><span data-stu-id="91852-474">hello MERGE statement can perform both insert and update operations in a single call.</span></span>

<span data-ttu-id="91852-475">Tabellvärdeparametrar kan användas med hello MERGE-instruktion tooperform uppdateringar och infogningar.</span><span class="sxs-lookup"><span data-stu-id="91852-475">Table-valued parameters can be used with hello MERGE statement tooperform updates and inserts.</span></span> <span data-ttu-id="91852-476">Anta till exempel att en förenklad medarbetare tabell som innehåller hello följande kolumner: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span><span class="sxs-lookup"><span data-stu-id="91852-476">For example, consider a simplified Employee table that contains hello following columns: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span></span>

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

<span data-ttu-id="91852-477">Du kan använda hello faktum att hello SocialSecurityNumber unika tooperform en SAMMANFOGNINGEN av flera medarbetare i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="91852-477">In this example, you can use hello fact that hello SocialSecurityNumber is unique tooperform a MERGE of multiple employees.</span></span> <span data-ttu-id="91852-478">Skapa först hello användardefinierade tabelltyp:</span><span class="sxs-lookup"><span data-stu-id="91852-478">First, create hello user-defined table type:</span></span>

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

<span data-ttu-id="91852-479">Sedan skapar en lagrad procedur eller skriva kod som använder hello MERGE-instruktion tooperform hello update och infoga.</span><span class="sxs-lookup"><span data-stu-id="91852-479">Next, create a stored procedure or write code that uses hello MERGE statement tooperform hello update and insert.</span></span> <span data-ttu-id="91852-480">hello följande exempel används hello MERGE-instruktion på en tabellvärdesparameter @employees, av typen EmployeeTableType.</span><span class="sxs-lookup"><span data-stu-id="91852-480">hello following example uses hello MERGE statement on a table-valued parameter, @employees, of type EmployeeTableType.</span></span> <span data-ttu-id="91852-481">Hej innehållet i hello @employees tabellen visas inte här.</span><span class="sxs-lookup"><span data-stu-id="91852-481">hello contents of hello @employees table are not shown here.</span></span>

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

<span data-ttu-id="91852-482">Mer information finns i hello dokumentation och exempel för hello MERGE-instruktion.</span><span class="sxs-lookup"><span data-stu-id="91852-482">For more information, see hello documentation and examples for hello MERGE statement.</span></span> <span data-ttu-id="91852-483">Även om hello samma arbets kunde utföras i flera steg lagrade proceduranrop med separata INSERT och UPDATE-åtgärder, är hello MERGE-instruktion effektivare.</span><span class="sxs-lookup"><span data-stu-id="91852-483">Although hello same work could be performed in a multiple-step stored procedure call with separate INSERT and UPDATE operations, hello MERGE statement is more efficient.</span></span> <span data-ttu-id="91852-484">Databaskod kan också skapa Transact-SQL-anrop som hello MERGE-instruktion direkt utan att två databasanrop för INSERT och UPDATE.</span><span class="sxs-lookup"><span data-stu-id="91852-484">Database code can also construct Transact-SQL calls that use hello MERGE statement directly without requiring two database calls for INSERT and UPDATE.</span></span>

## <a name="recommendation-summary"></a><span data-ttu-id="91852-485">Rekommendation sammanfattning</span><span class="sxs-lookup"><span data-stu-id="91852-485">Recommendation summary</span></span>
<span data-ttu-id="91852-486">hello innehåller följande lista en sammanfattning av hello batchbearbetning rekommendationerna som beskrivs i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="91852-486">hello following list provides a summary of hello batching recommendations discussed in this topic:</span></span>

* <span data-ttu-id="91852-487">Använd buffring och batchbearbetning tooincrease hello prestanda och skalbarhet SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="91852-487">Use buffering and batching tooincrease hello performance and scalability of SQL Database applications.</span></span>
* <span data-ttu-id="91852-488">Förstå hello kompromisser mellan batchbearbetning/buffring och återhämtning.</span><span class="sxs-lookup"><span data-stu-id="91852-488">Understand hello tradeoffs between batching/buffering and resiliency.</span></span> <span data-ttu-id="91852-489">Vid fel, roll, kan hello risken för att förlora en obearbetat batch affärskritiska data uppväger hello prestandafördelarna batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="91852-489">During a role failure, hello risk of losing an unprocessed batch of business-critical data might outweigh hello performance benefit of batching.</span></span>
* <span data-ttu-id="91852-490">Försök tookeep alla anrop toohello databas inom en enskild datacenter tooreduce fördröjning.</span><span class="sxs-lookup"><span data-stu-id="91852-490">Attempt tookeep all calls toohello database within a single datacenter tooreduce latency.</span></span>
* <span data-ttu-id="91852-491">Om du väljer en enda batching teknik erbjuder tabellvärdeparametrar hello bästa prestanda och flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="91852-491">If you choose a single batching technique, table-valued parameters offer hello best performance and flexibility.</span></span>
* <span data-ttu-id="91852-492">Infoga prestanda för hello snabbaste, följer dessa allmänna riktlinjer men testa ditt scenario:</span><span class="sxs-lookup"><span data-stu-id="91852-492">For hello fastest insert performance, follow these general guidelines but test your scenario:</span></span>
  * <span data-ttu-id="91852-493">Använd ett enda parametrar INSERT-kommando för < 100 rader.</span><span class="sxs-lookup"><span data-stu-id="91852-493">For < 100 rows, use a single parameterized INSERT command.</span></span>
  * <span data-ttu-id="91852-494">Använda tabellvärdeparametrar för < 1000 rader.</span><span class="sxs-lookup"><span data-stu-id="91852-494">For < 1000 rows, use table-valued parameters.</span></span>
  * <span data-ttu-id="91852-495">För > = 1000 rader, Använd SqlBulkCopy körs.</span><span class="sxs-lookup"><span data-stu-id="91852-495">For >= 1000 rows, use SqlBulkCopy.</span></span>
* <span data-ttu-id="91852-496">För update och delete-åtgärder kan använda tabellvärdeparametrar med lagrade proceduren logik som bestämmer hello rätt åtgärden på varje rad i hello tabell parametern.</span><span class="sxs-lookup"><span data-stu-id="91852-496">For update and delete operations, use table-valued parameters with stored procedure logic that determines hello correct operation on each row in hello table parameter.</span></span>
* <span data-ttu-id="91852-497">Riktlinjer för batch-storlek:</span><span class="sxs-lookup"><span data-stu-id="91852-497">Batch size guidelines:</span></span>
  * <span data-ttu-id="91852-498">Använda hello största batch som passar ditt program och affärskrav.</span><span class="sxs-lookup"><span data-stu-id="91852-498">Use hello largest batch sizes that make sense for your application and business requirements.</span></span>
  * <span data-ttu-id="91852-499">Belastningsutjämna hello prestandaförbättring av stora batchar med hello risk för tillfälliga eller katastrofalt fel.</span><span class="sxs-lookup"><span data-stu-id="91852-499">Balance hello performance gain of large batches with hello risks of temporary or catastrophic failures.</span></span> <span data-ttu-id="91852-500">Vad är hello följd av återförsök eller dataförlust hello i hello batch?</span><span class="sxs-lookup"><span data-stu-id="91852-500">What is hello consequence of retries or loss of hello data in hello batch?</span></span> 
  * <span data-ttu-id="91852-501">Testa hello största batch storlek tooverify att SQL-databasen inte avvisa den.</span><span class="sxs-lookup"><span data-stu-id="91852-501">Test hello largest batch size tooverify that SQL Database does not reject it.</span></span>
  * <span data-ttu-id="91852-502">Skapa inställningar som kontrollen batchbearbetning, till exempel hello batchstorlek eller hello buffring tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="91852-502">Create configuration settings that control batching, such as hello batch size or hello buffering time window.</span></span> <span data-ttu-id="91852-503">Dessa inställningar ger flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="91852-503">These settings provide flexibility.</span></span> <span data-ttu-id="91852-504">Du kan ändra hello batchbearbetning beteende i produktionsmiljö utan att omdistribuera hello-Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="91852-504">You can change hello batching behavior in production without redeploying hello cloud service.</span></span>
* <span data-ttu-id="91852-505">Undvik parallell körning av batchar som fungerar på en enda tabell i en databas.</span><span class="sxs-lookup"><span data-stu-id="91852-505">Avoid parallel execution of batches that operate on a single table in one database.</span></span> <span data-ttu-id="91852-506">Om du väljer toodivide en enskild batch över flera trådar kan köra testerna toodetermine hello perfekt antal trådar.</span><span class="sxs-lookup"><span data-stu-id="91852-506">If you do choose toodivide a single batch across multiple worker threads, run tests toodetermine hello ideal number of threads.</span></span> <span data-ttu-id="91852-507">När du har ett okänt tröskelvärde fler trådar kommer försämra prestanda i stället öka den.</span><span class="sxs-lookup"><span data-stu-id="91852-507">After an unspecified threshold, more threads will decrease performance rather than increase it.</span></span>
* <span data-ttu-id="91852-508">Överväg att buffring på storleken och tid som ett sätt att implementera batchbearbetning för flera scenarier.</span><span class="sxs-lookup"><span data-stu-id="91852-508">Consider buffering on size and time as a way of implementing batching for more scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="91852-509">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="91852-509">Next steps</span></span>
<span data-ttu-id="91852-510">Den här artikeln fokuserar på hur databasdesign och kodning tekniker relaterade toobatching kan förbättra ditt programprestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="91852-510">This article focused on how database design and coding techniques related toobatching can improve your application performance and scalability.</span></span> <span data-ttu-id="91852-511">Men det är bara en faktor i din övergripande säkerhetsstrategi.</span><span class="sxs-lookup"><span data-stu-id="91852-511">But this is just one factor in your overall strategy.</span></span> <span data-ttu-id="91852-512">Mer sätt tooimprove prestanda och skalbarhet finns [Azure SQL Database-prestandaråd för enskilda databaser](sql-database-performance-guidance.md) och [pris- och prestandaöverväganden för en elastisk pool](sql-database-elastic-pool-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="91852-512">For more ways tooimprove performance and scalability, see [Azure SQL Database performance guidance for single databases](sql-database-performance-guidance.md) and [Price and performance considerations for an elastic pool](sql-database-elastic-pool-guidance.md).</span></span>

