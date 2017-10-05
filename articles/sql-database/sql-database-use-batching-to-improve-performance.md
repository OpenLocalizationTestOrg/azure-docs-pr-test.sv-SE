---
title: "Hur du använder batchbearbetning för att förbättra prestanda för Azure SQL Database-program"
description: "Avsnittet ger bevis som batching databasåtgärder avsevärt imroves hastighet och skalbarheten för dina Azure SQL Database-program. Även om dessa batching metoder fungera för SQL Server-databas, fokuserar av artikeln på Azure."
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
ms.openlocfilehash: 22cff47444306e599325ba3035d83a0266d69c72
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="how-to-use-batching-to-improve-sql-database-application-performance"></a><span data-ttu-id="81da8-104">Hur du använder batchbearbetning för att förbättra programmens prestanda för SQL-databas</span><span class="sxs-lookup"><span data-stu-id="81da8-104">How to use batching to improve SQL Database application performance</span></span>
<span data-ttu-id="81da8-105">Batchbearbetning operations till Azure SQL Database avsevärt bättre prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="81da8-105">Batching operations to Azure SQL Database significantly improves the performance and scalability of your applications.</span></span> <span data-ttu-id="81da8-106">Den första delen av den här artikeln innehåller några exempel på testresultat som jämför sekventiella och batch-förfrågningar till en SQL-databas för att förstå fördelarna.</span><span class="sxs-lookup"><span data-stu-id="81da8-106">In order to understand the benefits, the first part of this article covers some sample test results that compare sequential and batched requests to a SQL Database.</span></span> <span data-ttu-id="81da8-107">Resten av artikeln visar tekniker, scenarier och överväganden som hjälper dig att använda batchbearbetning har i din Azure-program.</span><span class="sxs-lookup"><span data-stu-id="81da8-107">The remainder of the article shows the techniques, scenarios, and considerations to help you to use batching successfully in your Azure applications.</span></span>

## <a name="why-is-batching-important-for-sql-database"></a><span data-ttu-id="81da8-108">Varför är batchbearbetning viktigt för SQL-databas?</span><span class="sxs-lookup"><span data-stu-id="81da8-108">Why is batching important for SQL Database?</span></span>
<span data-ttu-id="81da8-109">Batchbearbetning anrop till en fjärrtjänsten är en välkänd strategi för att öka prestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="81da8-109">Batching calls to a remote service is a well-known strategy for increasing performance and scalability.</span></span> <span data-ttu-id="81da8-110">Det är fasta bearbetningskostnaderna för eventuella interaktioner med en fjärransluten tjänst, till exempel serialisering och nätverksöverföring deserialisering.</span><span class="sxs-lookup"><span data-stu-id="81da8-110">There are fixed processing costs to any interactions with a remote service, such as serialization, network transfer, and deserialization.</span></span> <span data-ttu-id="81da8-111">Paketera många separata transaktioner i en enskild batch minimerar dessa kostnader.</span><span class="sxs-lookup"><span data-stu-id="81da8-111">Packaging many separate transactions into a single batch minimizes these costs.</span></span>

<span data-ttu-id="81da8-112">I det här dokumentet som vi vill undersöka olika SQL-databas batchbearbetning strategier och scenarier.</span><span class="sxs-lookup"><span data-stu-id="81da8-112">In this paper, we want to examine various SQL Database batching strategies and scenarios.</span></span> <span data-ttu-id="81da8-113">Men strategierna är också viktigt för lokala program som använder SQL Server, finns det flera orsaker till syntaxmarkering användningen av batchbearbetning för SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="81da8-113">Although these strategies are also important for on-premises applications that use SQL Server, there are several reasons for highlighting the use of batching for SQL Database:</span></span>

* <span data-ttu-id="81da8-114">Det finns potentiellt större Nätverksfördröjningen för åtkomst till SQL-databas, särskilt om du ansluter till SQL-databas från utanför samma Microsoft Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="81da8-114">There is potentially greater network latency in accessing SQL Database, especially if you are accessing SQL Database from outside the same Microsoft Azure datacenter.</span></span>
* <span data-ttu-id="81da8-115">Flera egenskaper för SQL-databasen innebär att effektiviteten för de data access layer motsvarar övergripande skalbarhet i databasen.</span><span class="sxs-lookup"><span data-stu-id="81da8-115">The multitenant characteristics of SQL Database means that the efficiency of the data access layer correlates to the overall scalability of the database.</span></span> <span data-ttu-id="81da8-116">SQL-databas måste förhindra att en enskild klient/användare alltid använder databasresurser till skada för andra klienter.</span><span class="sxs-lookup"><span data-stu-id="81da8-116">SQL Database must prevent any single tenant/user from monopolizing database resources to the detriment of other tenants.</span></span> <span data-ttu-id="81da8-117">Som svar på användning utöver fördefinierade kvoter SQL Database minska eller svara med begränsning undantag.</span><span class="sxs-lookup"><span data-stu-id="81da8-117">In response to usage in excess of predefined quotas, SQL Database can reduce throughput or respond with throttling exceptions.</span></span> <span data-ttu-id="81da8-118">Effektivitet, till exempel batchbearbetning, kan du fortsätta att arbeta med SQL-databasen innan det nådde dessa begränsningar.</span><span class="sxs-lookup"><span data-stu-id="81da8-118">Efficiencies, such as batching, enable you to do more work on SQL Database before reaching these limits.</span></span> 
* <span data-ttu-id="81da8-119">Batchbearbetning är också gälla för konfigurationer med flera databaser (delning).</span><span class="sxs-lookup"><span data-stu-id="81da8-119">Batching is also effective for architectures that use multiple databases (sharding).</span></span> <span data-ttu-id="81da8-120">Effektivitet för din interaktion med varje enhet i databasen är fortfarande en avgörande faktor för din övergripande skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="81da8-120">The efficiency of your interaction with each database unit is still a key factor in your overall scalability.</span></span> 

<span data-ttu-id="81da8-121">En av fördelarna med att använda SQL-databas är att du inte behöver hantera servrar som värd för databasen.</span><span class="sxs-lookup"><span data-stu-id="81da8-121">One of the benefits of using SQL Database is that you don’t have to manage the servers that host the database.</span></span> <span data-ttu-id="81da8-122">Hanterade infrastrukturen innebär dock också att du behöver tänka på olika sätt på databasoptimering.</span><span class="sxs-lookup"><span data-stu-id="81da8-122">However, this managed infrastructure also means that you have to think differently about database optimizations.</span></span> <span data-ttu-id="81da8-123">Du kan inte längre leta för att förbättra databasinfrastruktur datormaskinvaran eller nätverksanslutningen.</span><span class="sxs-lookup"><span data-stu-id="81da8-123">You can no longer look to improve the database hardware or network infrastructure.</span></span> <span data-ttu-id="81da8-124">Microsoft Azure styr dessa miljöer.</span><span class="sxs-lookup"><span data-stu-id="81da8-124">Microsoft Azure controls those environments.</span></span> <span data-ttu-id="81da8-125">Området du kan styra är hur programmet samverkar med SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="81da8-125">The main area that you can control is how your application interacts with SQL Database.</span></span> <span data-ttu-id="81da8-126">Batchbearbetning är en av dessa optimeringar.</span><span class="sxs-lookup"><span data-stu-id="81da8-126">Batching is one of these optimizations.</span></span> 

<span data-ttu-id="81da8-127">Den första delen av dokumentet undersöker olika batching tekniker för .NET-program som använder SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="81da8-127">The first part of the paper examines various batching techniques for .NET applications that use SQL Database.</span></span> <span data-ttu-id="81da8-128">De sista två avsnitt beskriver batchbearbetning riktlinjer och scenarier.</span><span class="sxs-lookup"><span data-stu-id="81da8-128">The last two sections cover batching guidelines and scenarios.</span></span>

## <a name="batching-strategies"></a><span data-ttu-id="81da8-129">Batchbearbetning strategier</span><span class="sxs-lookup"><span data-stu-id="81da8-129">Batching strategies</span></span>
### <a name="note-about-timing-results-in-this-topic"></a><span data-ttu-id="81da8-130">Observera om tidsinställning resultat i det här avsnittet</span><span class="sxs-lookup"><span data-stu-id="81da8-130">Note about timing results in this topic</span></span>
> [!NOTE]
> <span data-ttu-id="81da8-131">Resultatet är inte prestandamått men är avsedda att visa **relativa prestandan**.</span><span class="sxs-lookup"><span data-stu-id="81da8-131">Results are not benchmarks but are meant to show **relative performance**.</span></span> <span data-ttu-id="81da8-132">Tidsinställningar baseras på ett genomsnitt av minst 10 testkörningar.</span><span class="sxs-lookup"><span data-stu-id="81da8-132">Timings are based on an average of at least 10 test runs.</span></span> <span data-ttu-id="81da8-133">Åtgärder som infogar i en tom tabell.</span><span class="sxs-lookup"><span data-stu-id="81da8-133">Operations are inserts into an empty table.</span></span> <span data-ttu-id="81da8-134">Dessa tester har uppmätta pre-V12 och de inte nödvändigtvis motsvarar dataflödet som kan uppstå i en V12-databas med den nya [tjänstnivåer](sql-database-service-tiers.md).</span><span class="sxs-lookup"><span data-stu-id="81da8-134">These tests were measured pre-V12, and they do not necessarily correspond to throughput that you might experience in a V12 database using the new [service tiers](sql-database-service-tiers.md).</span></span> <span data-ttu-id="81da8-135">Relativa fördelen att batching tekniken bör vara densamma.</span><span class="sxs-lookup"><span data-stu-id="81da8-135">The relative benefit of the batching technique should be similar.</span></span>
> 
> 

### <a name="transactions"></a><span data-ttu-id="81da8-136">Transaktioner</span><span class="sxs-lookup"><span data-stu-id="81da8-136">Transactions</span></span>
<span data-ttu-id="81da8-137">Det verkar konstigt att påbörja en granskning av batchbearbetning av diskutera transaktioner.</span><span class="sxs-lookup"><span data-stu-id="81da8-137">It seems strange to begin a review of batching by discussing transactions.</span></span> <span data-ttu-id="81da8-138">Men användning av transaktioner på klientsidan har en diskret serversidan batching effekt som förbättrar prestanda.</span><span class="sxs-lookup"><span data-stu-id="81da8-138">But the use of client-side transactions has a subtle server-side batching effect that improves performance.</span></span> <span data-ttu-id="81da8-139">Och transaktioner kan läggas till med bara några få rader med kod, så att de är ett snabbt sätt att förbättra prestanda för sekventiella operationer.</span><span class="sxs-lookup"><span data-stu-id="81da8-139">And transactions can be added with only a few lines of code, so they provide a fast way to improve performance of sequential operations.</span></span>

<span data-ttu-id="81da8-140">Överväg följande C#-koden som innehåller en sekvens med insert och uppdateringsåtgärder på en enkel tabell.</span><span class="sxs-lookup"><span data-stu-id="81da8-140">Consider the following C# code that contains a sequence of insert and update operations on a simple table.</span></span>

    List<string> dbOperations = new List<string>();
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 1");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 2");
    dbOperations.Add("update MyTable set mytext = 'updated text' where id = 3");
    dbOperations.Add("insert MyTable values ('new value',1)");
    dbOperations.Add("insert MyTable values ('new value',2)");
    dbOperations.Add("insert MyTable values ('new value',3)");

<span data-ttu-id="81da8-141">Följande kod ADO.NET utför sekventiellt dessa åtgärder.</span><span class="sxs-lookup"><span data-stu-id="81da8-141">The following ADO.NET code sequentially performs these operations.</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        conn.Open();

        foreach(string commandString in dbOperations)
        {
            SqlCommand cmd = new SqlCommand(commandString, conn);
            cmd.ExecuteNonQuery();                   
        }
    }

<span data-ttu-id="81da8-142">Det bästa sättet att optimera koden är att genomföra någon form av klientsidan massbearbetning av dessa anrop.</span><span class="sxs-lookup"><span data-stu-id="81da8-142">The best way to optimize this code is to implement some form of client-side batching of these calls.</span></span> <span data-ttu-id="81da8-143">Men det finns ett enkelt sätt att öka prestanda för den här koden genom att helt enkelt omsluta sekvens med anrop i en transaktion.</span><span class="sxs-lookup"><span data-stu-id="81da8-143">But there is a simple way to increase the performance of this code by simply wrapping the sequence of calls in a transaction.</span></span> <span data-ttu-id="81da8-144">Här är samma kod som använder en transaktion.</span><span class="sxs-lookup"><span data-stu-id="81da8-144">Here is the same code that uses a transaction.</span></span>

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

<span data-ttu-id="81da8-145">Transaktioner används faktiskt i båda de här exemplen.</span><span class="sxs-lookup"><span data-stu-id="81da8-145">Transactions are actually being used in both of these examples.</span></span> <span data-ttu-id="81da8-146">I det första exemplet är varje enskild anropet en implicit transaktion.</span><span class="sxs-lookup"><span data-stu-id="81da8-146">In the first example, each individual call is an implicit transaction.</span></span> <span data-ttu-id="81da8-147">I det andra exemplet radbryts alla anropen med en explicit transaktion.</span><span class="sxs-lookup"><span data-stu-id="81da8-147">In the second example, an explicit transaction wraps all of the calls.</span></span> <span data-ttu-id="81da8-148">Per i dokumentationen för den [write-ahead transaktionsloggen](https://msdn.microsoft.com/library/ms186259.aspx), loggposter är replikatorn tömmer till disken när transaktionen genomförts.</span><span class="sxs-lookup"><span data-stu-id="81da8-148">Per the documentation for the [write-ahead transaction log](https://msdn.microsoft.com/library/ms186259.aspx), log records are flushed to the disk when the transaction commits.</span></span> <span data-ttu-id="81da8-149">Så genom att lägga till fler anrop i en transaktion kan skrivningen till transaktionsloggen fördröja tills genomförs transaktionen.</span><span class="sxs-lookup"><span data-stu-id="81da8-149">So by including more calls in a transaction, the write to the transaction log can delay until the transaction is committed.</span></span> <span data-ttu-id="81da8-150">Du aktiverar i praktiken batchbearbetning för skrivningar till serverns transaktionsloggen.</span><span class="sxs-lookup"><span data-stu-id="81da8-150">In effect, you are enabling batching for the writes to the server’s transaction log.</span></span>

<span data-ttu-id="81da8-151">I följande tabell visas några ad hoc-testresultaten.</span><span class="sxs-lookup"><span data-stu-id="81da8-151">The following table shows some ad-hoc testing results.</span></span> <span data-ttu-id="81da8-152">Samma sekventiella infogningarna med och utan transaktioner tester.</span><span class="sxs-lookup"><span data-stu-id="81da8-152">The tests performed the same sequential inserts with and without transactions.</span></span> <span data-ttu-id="81da8-153">För mer perspektiv körde den första uppsättningen tester via fjärranslutning från en bärbar dator till databasen i Microsoft Azure.</span><span class="sxs-lookup"><span data-stu-id="81da8-153">For more perspective, the first set of tests ran remotely from a laptop to the database in Microsoft Azure.</span></span> <span data-ttu-id="81da8-154">Den första uppsättningen tester körde från en tjänst i molnet och båda finns i samma datacenter (USA, västra) för Microsoft Azure-databas.</span><span class="sxs-lookup"><span data-stu-id="81da8-154">The second set of tests ran from a cloud service and database that both resided within the same Microsoft Azure datacenter (West US).</span></span> <span data-ttu-id="81da8-155">Följande tabell visar varaktigheten i millisekunder för sekventiella infogningar med och utan transaktioner.</span><span class="sxs-lookup"><span data-stu-id="81da8-155">The following table shows the duration in milliseconds of sequential inserts with and without transactions.</span></span>

<span data-ttu-id="81da8-156">**Lokalt till Azure**:</span><span class="sxs-lookup"><span data-stu-id="81da8-156">**On-Premises to Azure**:</span></span>

| <span data-ttu-id="81da8-157">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="81da8-157">Operations</span></span> | <span data-ttu-id="81da8-158">Ingen transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-158">No Transaction (ms)</span></span> | <span data-ttu-id="81da8-159">Transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-159">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81da8-160">1</span><span class="sxs-lookup"><span data-stu-id="81da8-160">1</span></span> |<span data-ttu-id="81da8-161">130</span><span class="sxs-lookup"><span data-stu-id="81da8-161">130</span></span> |<span data-ttu-id="81da8-162">402</span><span class="sxs-lookup"><span data-stu-id="81da8-162">402</span></span> |
| <span data-ttu-id="81da8-163">10</span><span class="sxs-lookup"><span data-stu-id="81da8-163">10</span></span> |<span data-ttu-id="81da8-164">1208</span><span class="sxs-lookup"><span data-stu-id="81da8-164">1208</span></span> |<span data-ttu-id="81da8-165">1226</span><span class="sxs-lookup"><span data-stu-id="81da8-165">1226</span></span> |
| <span data-ttu-id="81da8-166">100</span><span class="sxs-lookup"><span data-stu-id="81da8-166">100</span></span> |<span data-ttu-id="81da8-167">12662</span><span class="sxs-lookup"><span data-stu-id="81da8-167">12662</span></span> |<span data-ttu-id="81da8-168">10395</span><span class="sxs-lookup"><span data-stu-id="81da8-168">10395</span></span> |
| <span data-ttu-id="81da8-169">1000</span><span class="sxs-lookup"><span data-stu-id="81da8-169">1000</span></span> |<span data-ttu-id="81da8-170">128852</span><span class="sxs-lookup"><span data-stu-id="81da8-170">128852</span></span> |<span data-ttu-id="81da8-171">102917</span><span class="sxs-lookup"><span data-stu-id="81da8-171">102917</span></span> |

<span data-ttu-id="81da8-172">**Azure till Azure (samma datacenter)**:</span><span class="sxs-lookup"><span data-stu-id="81da8-172">**Azure to Azure (same datacenter)**:</span></span>

| <span data-ttu-id="81da8-173">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="81da8-173">Operations</span></span> | <span data-ttu-id="81da8-174">Ingen transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-174">No Transaction (ms)</span></span> | <span data-ttu-id="81da8-175">Transaktion (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-175">Transaction (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81da8-176">1</span><span class="sxs-lookup"><span data-stu-id="81da8-176">1</span></span> |<span data-ttu-id="81da8-177">21</span><span class="sxs-lookup"><span data-stu-id="81da8-177">21</span></span> |<span data-ttu-id="81da8-178">26</span><span class="sxs-lookup"><span data-stu-id="81da8-178">26</span></span> |
| <span data-ttu-id="81da8-179">10</span><span class="sxs-lookup"><span data-stu-id="81da8-179">10</span></span> |<span data-ttu-id="81da8-180">220</span><span class="sxs-lookup"><span data-stu-id="81da8-180">220</span></span> |<span data-ttu-id="81da8-181">56</span><span class="sxs-lookup"><span data-stu-id="81da8-181">56</span></span> |
| <span data-ttu-id="81da8-182">100</span><span class="sxs-lookup"><span data-stu-id="81da8-182">100</span></span> |<span data-ttu-id="81da8-183">2145</span><span class="sxs-lookup"><span data-stu-id="81da8-183">2145</span></span> |<span data-ttu-id="81da8-184">341</span><span class="sxs-lookup"><span data-stu-id="81da8-184">341</span></span> |
| <span data-ttu-id="81da8-185">1000</span><span class="sxs-lookup"><span data-stu-id="81da8-185">1000</span></span> |<span data-ttu-id="81da8-186">21479</span><span class="sxs-lookup"><span data-stu-id="81da8-186">21479</span></span> |<span data-ttu-id="81da8-187">2756</span><span class="sxs-lookup"><span data-stu-id="81da8-187">2756</span></span> |

> [!NOTE]
> <span data-ttu-id="81da8-188">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="81da8-188">Results are not benchmarks.</span></span> <span data-ttu-id="81da8-189">Finns det [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="81da8-189">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="81da8-190">Baserat på föregående testresultaten, minskar byta en enda åtgärd i en transaktion faktiskt prestanda.</span><span class="sxs-lookup"><span data-stu-id="81da8-190">Based on the previous test results, wrapping a single operation in a transaction actually decreases performance.</span></span> <span data-ttu-id="81da8-191">Men om du ökar antalet åtgärder i en enda transaktion förbättring av prestanda blir större.</span><span class="sxs-lookup"><span data-stu-id="81da8-191">But as you increase the number of operations within a single transaction, the performance improvement becomes more marked.</span></span> <span data-ttu-id="81da8-192">Prestandaskillnaden är också mer märkbart när alla åtgärder som vidtas i Microsoft Azure-datacenter.</span><span class="sxs-lookup"><span data-stu-id="81da8-192">The performance difference is also more noticeable when all operations occur within the Microsoft Azure datacenter.</span></span> <span data-ttu-id="81da8-193">Ökad latens med SQL-databas från utanför Microsoft Azure-datacenter överskuggar prestandafördelar med transaktioner.</span><span class="sxs-lookup"><span data-stu-id="81da8-193">The increased latency of using SQL Database from outside the Microsoft Azure datacenter overshadows the performance gain of using transactions.</span></span>

<span data-ttu-id="81da8-194">Även om användning av transaktioner kan öka prestanda, fortsätter att [Se metodtips för transaktioner och anslutningar](https://msdn.microsoft.com/library/ms187484.aspx).</span><span class="sxs-lookup"><span data-stu-id="81da8-194">Although the use of transactions can increase performance, continue to [observe best practices for transactions and connections](https://msdn.microsoft.com/library/ms187484.aspx).</span></span> <span data-ttu-id="81da8-195">Behåll transaktioner så kort som möjligt och Stäng databasanslutningen när arbetet är klar.</span><span class="sxs-lookup"><span data-stu-id="81da8-195">Keep the transaction as short as possible, and close the database connection after the work completes.</span></span> <span data-ttu-id="81da8-196">Den med hjälp av instruktionen i det förra exemplet säkerställer att anslutningen är stängd när efterföljande kodblocket har slutförts.</span><span class="sxs-lookup"><span data-stu-id="81da8-196">The using statement in the previous example assures that the connection is closed when the subsequent code block completes.</span></span>

<span data-ttu-id="81da8-197">Föregående exempel visar att du kan lägga till en lokal transaktion ADO.NET koden med två rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-197">The previous example demonstrates that you can add a local transaction to any ADO.NET code with two lines.</span></span> <span data-ttu-id="81da8-198">Transaktioner erbjuder ett snabbt sätt att förbättra prestanda för kod som gör sekventiella infoga, uppdatera och ta bort.</span><span class="sxs-lookup"><span data-stu-id="81da8-198">Transactions offer a quick way to improve the performance of code that makes sequential insert, update, and delete operations.</span></span> <span data-ttu-id="81da8-199">Men för bästa prestanda, Överväg att ändra koden efter dra nytta av klientsidan batchbearbetning, till exempel tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="81da8-199">However, for the fastest performance, consider changing the code further to take advantage of client-side batching, such as table-valued parameters.</span></span>

<span data-ttu-id="81da8-200">Mer information om transaktioner i ADO.NET finns [lokala transaktioner i ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span><span class="sxs-lookup"><span data-stu-id="81da8-200">For more information about transactions in ADO.NET, see [Local Transactions in ADO.NET](https://docs.microsoft.com/dotnet/framework/data/adonet/local-transactions).</span></span>

### <a name="table-valued-parameters"></a><span data-ttu-id="81da8-201">tabellvärdeparametrar</span><span class="sxs-lookup"><span data-stu-id="81da8-201">Table-valued parameters</span></span>
<span data-ttu-id="81da8-202">Tabellvärdeparametrar stöd för användardefinierade tabelltyper som parametrar i Transact-SQL-uttryck, lagrade procedurer och funktioner.</span><span class="sxs-lookup"><span data-stu-id="81da8-202">Table-valued parameters support user-defined table types as parameters in Transact-SQL statements, stored procedures, and functions.</span></span> <span data-ttu-id="81da8-203">Den här batching tekniken på klientsidan kan du skicka flera rader med data i parametern-tabellvärdesfunktion.</span><span class="sxs-lookup"><span data-stu-id="81da8-203">This client-side batching technique allows you to send multiple rows of data within the table-valued parameter.</span></span> <span data-ttu-id="81da8-204">Om du vill använda tabellvärdeparametrar måste du först definiera en tabelltyp.</span><span class="sxs-lookup"><span data-stu-id="81da8-204">To use table-valued parameters, first define a table type.</span></span> <span data-ttu-id="81da8-205">Följande Transact-SQL-instruktion skapas en tabelltyp som heter **MyTableType**.</span><span class="sxs-lookup"><span data-stu-id="81da8-205">The following Transact-SQL statement creates a table type named **MyTableType**.</span></span>

    CREATE TYPE MyTableType AS TABLE 
    ( mytext TEXT,
      num INT );


<span data-ttu-id="81da8-206">I koden, skapar du en **DataTable** med exakt samma namn och typer av tabelltypen.</span><span class="sxs-lookup"><span data-stu-id="81da8-206">In code, you create a **DataTable** with the exact same names and types of the table type.</span></span> <span data-ttu-id="81da8-207">Skicka detta **DataTable** i en parameter i en textfråga eller en lagrad procedur anropa.</span><span class="sxs-lookup"><span data-stu-id="81da8-207">Pass this **DataTable** in a parameter in a text query or stored procedure call.</span></span> <span data-ttu-id="81da8-208">I följande exempel visas den här tekniken:</span><span class="sxs-lookup"><span data-stu-id="81da8-208">The following example shows this technique:</span></span>

    using (SqlConnection connection = new SqlConnection(CloudConfigurationManager.GetSetting("Sql.ConnectionString")))
    {
        connection.Open();

        DataTable table = new DataTable();
        // Add columns and rows. The following is a simple example.
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

<span data-ttu-id="81da8-209">I föregående exempel är den **SqlCommand** objekt infogar rader från en tabellvärdesparameter  **@TestTvp** .</span><span class="sxs-lookup"><span data-stu-id="81da8-209">In the previous example, the **SqlCommand** object inserts rows from a table-valued parameter, **@TestTvp**.</span></span> <span data-ttu-id="81da8-210">Den tidigare skapade **DataTable** objekt som har tilldelats den här parametern med det **SqlCommand.Parameters.Add** metod.</span><span class="sxs-lookup"><span data-stu-id="81da8-210">The previously created **DataTable** object is assigned to this parameter with the **SqlCommand.Parameters.Add** method.</span></span> <span data-ttu-id="81da8-211">Batchbearbetning infogningar i ett anrop avsevärt ökar prestanda över sekventiella infogningar.</span><span class="sxs-lookup"><span data-stu-id="81da8-211">Batching the inserts in one call significantly increases the performance over sequential inserts.</span></span>

<span data-ttu-id="81da8-212">Använda en lagrad procedur i stället för ett textbaserat kommando för att förbättra det tidigare exemplet ytterligare.</span><span class="sxs-lookup"><span data-stu-id="81da8-212">To improve the previous example further, use a stored procedure instead of a text-based command.</span></span> <span data-ttu-id="81da8-213">Följande Transact-SQL-kommando skapar en lagrad procedur som tar den **SimpleTestTableType** tabellvärdesparametern.</span><span class="sxs-lookup"><span data-stu-id="81da8-213">The following Transact-SQL command creates a stored procedure that takes the **SimpleTestTableType** table-valued parameter.</span></span>

    CREATE PROCEDURE [dbo].[sp_InsertRows] 
    @TestTvp as MyTableType READONLY
    AS
    BEGIN
    INSERT INTO MyTable(mytext, num) 
    SELECT mytext, num FROM @TestTvp
    END
    GO

<span data-ttu-id="81da8-214">Ändra den **SqlCommand** deklarationen i det förra kodexemplet till följande objekt.</span><span class="sxs-lookup"><span data-stu-id="81da8-214">Then change the **SqlCommand** object declaration in the previous code example to the following.</span></span>

    SqlCommand cmd = new SqlCommand("sp_InsertRows", connection);
    cmd.CommandType = CommandType.StoredProcedure;

<span data-ttu-id="81da8-215">I de flesta fall har tabellvärdeparametrar motsvarande eller bättre prestanda än andra batching metoder.</span><span class="sxs-lookup"><span data-stu-id="81da8-215">In most cases, table-valued parameters have equivalent or better performance than other batching techniques.</span></span> <span data-ttu-id="81da8-216">Tabellvärdeparametrar är ofta bättre, eftersom de är mer flexibelt än andra alternativ.</span><span class="sxs-lookup"><span data-stu-id="81da8-216">Table-valued parameters are often preferable, because they are more flexible than other options.</span></span> <span data-ttu-id="81da8-217">Till exempel tillåter andra tekniker, till exempel SQL-masskopiering, bara nya rader infogas.</span><span class="sxs-lookup"><span data-stu-id="81da8-217">For example, other techniques, such as SQL bulk copy, only permit the insertion of new rows.</span></span> <span data-ttu-id="81da8-218">Men med tabellvärdeparametrar, du kan använda logiken i den lagrade proceduren för att avgöra vilka rader som uppdateras och som kan infogas.</span><span class="sxs-lookup"><span data-stu-id="81da8-218">But with table-valued parameters, you can use logic in the stored procedure to determine which rows are updates and which are inserts.</span></span> <span data-ttu-id="81da8-219">Tabelltypen kan också ändras så att den innehåller en ”åtgärden”-kolumn som anger om den angivna raden ska infogas, uppdateras eller tas bort.</span><span class="sxs-lookup"><span data-stu-id="81da8-219">The table type can also be modified to contain an “Operation” column that indicates whether the specified row should be inserted, updated, or deleted.</span></span>

<span data-ttu-id="81da8-220">Följande tabell visar ad hoc-testresultaten för användning av tabellvärdeparametrar i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="81da8-220">The following table shows ad-hoc test results for the use of table-valued parameters in milliseconds.</span></span>

| <span data-ttu-id="81da8-221">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="81da8-221">Operations</span></span> | <span data-ttu-id="81da8-222">Lokalt till Azure (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-222">On-Premises to Azure (ms)</span></span> | <span data-ttu-id="81da8-223">Samma Azure-datacenter (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-223">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81da8-224">1</span><span class="sxs-lookup"><span data-stu-id="81da8-224">1</span></span> |<span data-ttu-id="81da8-225">124</span><span class="sxs-lookup"><span data-stu-id="81da8-225">124</span></span> |<span data-ttu-id="81da8-226">32</span><span class="sxs-lookup"><span data-stu-id="81da8-226">32</span></span> |
| <span data-ttu-id="81da8-227">10</span><span class="sxs-lookup"><span data-stu-id="81da8-227">10</span></span> |<span data-ttu-id="81da8-228">131</span><span class="sxs-lookup"><span data-stu-id="81da8-228">131</span></span> |<span data-ttu-id="81da8-229">25</span><span class="sxs-lookup"><span data-stu-id="81da8-229">25</span></span> |
| <span data-ttu-id="81da8-230">100</span><span class="sxs-lookup"><span data-stu-id="81da8-230">100</span></span> |<span data-ttu-id="81da8-231">338</span><span class="sxs-lookup"><span data-stu-id="81da8-231">338</span></span> |<span data-ttu-id="81da8-232">51</span><span class="sxs-lookup"><span data-stu-id="81da8-232">51</span></span> |
| <span data-ttu-id="81da8-233">1000</span><span class="sxs-lookup"><span data-stu-id="81da8-233">1000</span></span> |<span data-ttu-id="81da8-234">2615</span><span class="sxs-lookup"><span data-stu-id="81da8-234">2615</span></span> |<span data-ttu-id="81da8-235">382</span><span class="sxs-lookup"><span data-stu-id="81da8-235">382</span></span> |
| <span data-ttu-id="81da8-236">10000</span><span class="sxs-lookup"><span data-stu-id="81da8-236">10000</span></span> |<span data-ttu-id="81da8-237">23830</span><span class="sxs-lookup"><span data-stu-id="81da8-237">23830</span></span> |<span data-ttu-id="81da8-238">3586</span><span class="sxs-lookup"><span data-stu-id="81da8-238">3586</span></span> |

> [!NOTE]
> <span data-ttu-id="81da8-239">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="81da8-239">Results are not benchmarks.</span></span> <span data-ttu-id="81da8-240">Finns det [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="81da8-240">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="81da8-241">Prestandafördelar från batchbearbetning är uppenbar.</span><span class="sxs-lookup"><span data-stu-id="81da8-241">The performance gain from batching is immediately apparent.</span></span> <span data-ttu-id="81da8-242">I det föregående sekventiella testet tog 1000 åtgärderna 129 sekunder utanför datacentret och 21 sekunder från inom datacentret.</span><span class="sxs-lookup"><span data-stu-id="81da8-242">In the previous sequential test, 1000 operations took 129 seconds outside the datacenter and 21 seconds from within the datacenter.</span></span> <span data-ttu-id="81da8-243">Men med tabellvärdeparametrar, 1000 operations Ta endast 2.6 sekunder utanför datacentret och 0,4 sekunder inom datacentret.</span><span class="sxs-lookup"><span data-stu-id="81da8-243">But with table-valued parameters, 1000 operations take only 2.6 seconds outside the datacenter and 0.4 seconds within the datacenter.</span></span>

<span data-ttu-id="81da8-244">Mer information om tabellvärdeparametrar finns [Table-Valued parametrar](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="81da8-244">For more information on table-valued parameters, see [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

### <a name="sql-bulk-copy"></a><span data-ttu-id="81da8-245">SQL-masskopiering</span><span class="sxs-lookup"><span data-stu-id="81da8-245">SQL bulk copy</span></span>
<span data-ttu-id="81da8-246">SQL-masskopiering är ett annat sätt att infoga stora mängder data i en måldatabasen.</span><span class="sxs-lookup"><span data-stu-id="81da8-246">SQL bulk copy is another way to insert large amounts of data into a target database.</span></span> <span data-ttu-id="81da8-247">.NET-program kan använda den **SqlBulkCopy körs** klassen för att utföra bulk insert åtgärder.</span><span class="sxs-lookup"><span data-stu-id="81da8-247">.NET applications can use the **SqlBulkCopy** class to perform bulk insert operations.</span></span> <span data-ttu-id="81da8-248">**SqlBulkCopy körs** liknar i funktionen kommandoradsverktyget **Bcp.exe**, eller Transact-SQL-instruktion **BULK INSERT**.</span><span class="sxs-lookup"><span data-stu-id="81da8-248">**SqlBulkCopy** is similar in function to the command-line tool, **Bcp.exe**, or the Transact-SQL statement, **BULK INSERT**.</span></span> <span data-ttu-id="81da8-249">Följande kodexempel visar hur du masskopiera raderna i källan **DataTable**, tabell mytable prefix till måltabellen i SQL Server.</span><span class="sxs-lookup"><span data-stu-id="81da8-249">The following code example shows how to bulk copy the rows in the source **DataTable**, table, to the destination table in SQL Server, MyTable.</span></span>

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

<span data-ttu-id="81da8-250">Det finns tillfällen där masskopiering är att föredra över tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="81da8-250">There are some cases where bulk copy is preferred over table-valued parameters.</span></span> <span data-ttu-id="81da8-251">Se jämförelsetabellen av tabellvärdeparametrar jämfört med BULK INSERT åtgärder i avsnittet [Table-Valued parametrar](https://msdn.microsoft.com/library/bb510489.aspx).</span><span class="sxs-lookup"><span data-stu-id="81da8-251">See the comparison table of Table-Valued parameters versus BULK INSERT operations in the topic [Table-Valued Parameters](https://msdn.microsoft.com/library/bb510489.aspx).</span></span>

<span data-ttu-id="81da8-252">Följande ad hoc-testresultaten visar prestanda för batchbearbetning med **SqlBulkCopy körs** i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="81da8-252">The following ad-hoc test results show the performance of batching with **SqlBulkCopy** in milliseconds.</span></span>

| <span data-ttu-id="81da8-253">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="81da8-253">Operations</span></span> | <span data-ttu-id="81da8-254">Lokalt till Azure (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-254">On-Premises to Azure (ms)</span></span> | <span data-ttu-id="81da8-255">Samma Azure-datacenter (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-255">Azure same datacenter (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81da8-256">1</span><span class="sxs-lookup"><span data-stu-id="81da8-256">1</span></span> |<span data-ttu-id="81da8-257">433</span><span class="sxs-lookup"><span data-stu-id="81da8-257">433</span></span> |<span data-ttu-id="81da8-258">57</span><span class="sxs-lookup"><span data-stu-id="81da8-258">57</span></span> |
| <span data-ttu-id="81da8-259">10</span><span class="sxs-lookup"><span data-stu-id="81da8-259">10</span></span> |<span data-ttu-id="81da8-260">441</span><span class="sxs-lookup"><span data-stu-id="81da8-260">441</span></span> |<span data-ttu-id="81da8-261">32</span><span class="sxs-lookup"><span data-stu-id="81da8-261">32</span></span> |
| <span data-ttu-id="81da8-262">100</span><span class="sxs-lookup"><span data-stu-id="81da8-262">100</span></span> |<span data-ttu-id="81da8-263">636</span><span class="sxs-lookup"><span data-stu-id="81da8-263">636</span></span> |<span data-ttu-id="81da8-264">53</span><span class="sxs-lookup"><span data-stu-id="81da8-264">53</span></span> |
| <span data-ttu-id="81da8-265">1000</span><span class="sxs-lookup"><span data-stu-id="81da8-265">1000</span></span> |<span data-ttu-id="81da8-266">2535</span><span class="sxs-lookup"><span data-stu-id="81da8-266">2535</span></span> |<span data-ttu-id="81da8-267">341</span><span class="sxs-lookup"><span data-stu-id="81da8-267">341</span></span> |
| <span data-ttu-id="81da8-268">10000</span><span class="sxs-lookup"><span data-stu-id="81da8-268">10000</span></span> |<span data-ttu-id="81da8-269">21605</span><span class="sxs-lookup"><span data-stu-id="81da8-269">21605</span></span> |<span data-ttu-id="81da8-270">2737</span><span class="sxs-lookup"><span data-stu-id="81da8-270">2737</span></span> |

> [!NOTE]
> <span data-ttu-id="81da8-271">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="81da8-271">Results are not benchmarks.</span></span> <span data-ttu-id="81da8-272">Finns det [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="81da8-272">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="81da8-273">I batch är mindre, Använd tabellvärdeparametrar gick bättre än förväntat den **SqlBulkCopy körs** klass.</span><span class="sxs-lookup"><span data-stu-id="81da8-273">In smaller batch sizes, the use table-valued parameters outperformed the **SqlBulkCopy** class.</span></span> <span data-ttu-id="81da8-274">Dock **SqlBulkCopy körs** utföras 12-31% snabbare än tabellvärdeparametrar för testerna av 1 000 och 10 000 rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-274">However, **SqlBulkCopy** performed 12-31% faster than table-valued parameters for the tests of 1,000 and 10,000 rows.</span></span> <span data-ttu-id="81da8-275">Som tabellvärdeparametrar, **SqlBulkCopy körs** är ett bra alternativ för gruppbaserad infogningar särskilt jämfört med utförandet av ett annat operationer.</span><span class="sxs-lookup"><span data-stu-id="81da8-275">Like table-valued parameters, **SqlBulkCopy** is a good option for batched inserts, especially when compared to the performance of non-batched operations.</span></span>

<span data-ttu-id="81da8-276">Mer information om masskopiering i ADO.NET finns [Masskopieringsåtgärder i SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span><span class="sxs-lookup"><span data-stu-id="81da8-276">For more information on bulk copy in ADO.NET, see [Bulk Copy Operations in SQL Server](https://msdn.microsoft.com/library/7ek5da1a.aspx).</span></span>

### <a name="multiple-row-parameterized-insert-statements"></a><span data-ttu-id="81da8-277">Flera rader som innehåller parametrar Infoga instruktioner</span><span class="sxs-lookup"><span data-stu-id="81da8-277">Multiple-row Parameterized INSERT statements</span></span>
<span data-ttu-id="81da8-278">Ett alternativ för små batchar är att skapa en stor parametriserade INSERT-sats som infogar flera rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-278">One alternative for small batches is to construct a large parameterized INSERT statement that inserts multiple rows.</span></span> <span data-ttu-id="81da8-279">Följande kodexempel visar den här tekniken.</span><span class="sxs-lookup"><span data-stu-id="81da8-279">The following code example demonstrates this technique.</span></span>

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


<span data-ttu-id="81da8-280">Det här exemplet är avsedd att visa det grundläggande konceptet.</span><span class="sxs-lookup"><span data-stu-id="81da8-280">This example is meant to show the basic concept.</span></span> <span data-ttu-id="81da8-281">En mer realistisk scenariot skulle gå igenom entiteterna krävs för att konstruera frågesträngen och parametrarna samtidigt.</span><span class="sxs-lookup"><span data-stu-id="81da8-281">A more realistic scenario would loop through the required entities to construct the query string and the command parameters simultaneously.</span></span> <span data-ttu-id="81da8-282">Du är begränsad till totalt 2100 frågeparametrar, så detta begränsar det totala antalet rader som kan bearbetas i det här sättet.</span><span class="sxs-lookup"><span data-stu-id="81da8-282">You are limited to a total of 2100 query parameters, so this limits the total number of rows that can be processed in this manner.</span></span>

<span data-ttu-id="81da8-283">Följande ad hoc-testresultaten visar prestanda för den här typen av insert-instruktionen i millisekunder.</span><span class="sxs-lookup"><span data-stu-id="81da8-283">The following ad-hoc test results show the performance of this type of insert statement in milliseconds.</span></span>

| <span data-ttu-id="81da8-284">Åtgärder</span><span class="sxs-lookup"><span data-stu-id="81da8-284">Operations</span></span> | <span data-ttu-id="81da8-285">Tabellvärdeparametrar (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-285">Table-valued parameters (ms)</span></span> | <span data-ttu-id="81da8-286">Infoga enstaka uttryck (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-286">Single-statement INSERT (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81da8-287">1</span><span class="sxs-lookup"><span data-stu-id="81da8-287">1</span></span> |<span data-ttu-id="81da8-288">32</span><span class="sxs-lookup"><span data-stu-id="81da8-288">32</span></span> |<span data-ttu-id="81da8-289">20</span><span class="sxs-lookup"><span data-stu-id="81da8-289">20</span></span> |
| <span data-ttu-id="81da8-290">10</span><span class="sxs-lookup"><span data-stu-id="81da8-290">10</span></span> |<span data-ttu-id="81da8-291">30</span><span class="sxs-lookup"><span data-stu-id="81da8-291">30</span></span> |<span data-ttu-id="81da8-292">25</span><span class="sxs-lookup"><span data-stu-id="81da8-292">25</span></span> |
| <span data-ttu-id="81da8-293">100</span><span class="sxs-lookup"><span data-stu-id="81da8-293">100</span></span> |<span data-ttu-id="81da8-294">33</span><span class="sxs-lookup"><span data-stu-id="81da8-294">33</span></span> |<span data-ttu-id="81da8-295">51</span><span class="sxs-lookup"><span data-stu-id="81da8-295">51</span></span> |

> [!NOTE]
> <span data-ttu-id="81da8-296">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="81da8-296">Results are not benchmarks.</span></span> <span data-ttu-id="81da8-297">Finns det [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="81da8-297">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="81da8-298">Den här metoden kan vara något snabbare för batchar som är mindre än 100 rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-298">This approach can be slightly faster for batches that are less than 100 rows.</span></span> <span data-ttu-id="81da8-299">Även om förbättring är liten är tekniken ett annat alternativ som fungerar bra i ditt specifika program scenario.</span><span class="sxs-lookup"><span data-stu-id="81da8-299">Although the improvement is small, this technique is another option that might work well in your specific application scenario.</span></span>

### <a name="dataadapter"></a><span data-ttu-id="81da8-300">DataAdapter</span><span class="sxs-lookup"><span data-stu-id="81da8-300">DataAdapter</span></span>
<span data-ttu-id="81da8-301">Den **DataAdapter** klassen kan du ändra en **DataSet** objektet och sedan skicka ändringarna som INSERT, UPDATE och DELETE-åtgärder.</span><span class="sxs-lookup"><span data-stu-id="81da8-301">The **DataAdapter** class allows you to modify a **DataSet** object and then submit the changes as INSERT, UPDATE, and DELETE operations.</span></span> <span data-ttu-id="81da8-302">Om du använder den **DataAdapter** på detta sätt är det viktigt att notera att anrop görs för varje distinkta åtgärd.</span><span class="sxs-lookup"><span data-stu-id="81da8-302">If you are using the **DataAdapter** in this manner, it is important to note that separate calls are made for each distinct operation.</span></span> <span data-ttu-id="81da8-303">Du kan förbättra prestanda i **UpdateBatchSize** egenskapen antal åtgärder som ska grupperas i taget.</span><span class="sxs-lookup"><span data-stu-id="81da8-303">To improve performance, use the **UpdateBatchSize** property to the number of operations that should be batched at a time.</span></span> <span data-ttu-id="81da8-304">Mer information finns i [utför Batch åtgärder med hjälp av DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span><span class="sxs-lookup"><span data-stu-id="81da8-304">For more information, see [Performing Batch Operations Using DataAdapters](https://msdn.microsoft.com/library/aadf8fk2.aspx).</span></span>

### <a name="entity-framework"></a><span data-ttu-id="81da8-305">Entity framework</span><span class="sxs-lookup"><span data-stu-id="81da8-305">Entity framework</span></span>
<span data-ttu-id="81da8-306">Entity Framework stöder för närvarande inte batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="81da8-306">Entity Framework does not currently support batching.</span></span> <span data-ttu-id="81da8-307">Olika utvecklare i gemenskapen har försökt att visa lösningar, till exempel åsidosättning av **SaveChanges** metod.</span><span class="sxs-lookup"><span data-stu-id="81da8-307">Different developers in the community have attempted to demonstrate workarounds, such as override the **SaveChanges** method.</span></span> <span data-ttu-id="81da8-308">Men lösningarna som vanligtvis är komplicerade och anpassade program och datamodellen.</span><span class="sxs-lookup"><span data-stu-id="81da8-308">But the solutions are typically complex and customized to the application and data model.</span></span> <span data-ttu-id="81da8-309">Entity Framework codeplex projektet har för närvarande en diskussionssida på den här funktionsbegäran.</span><span class="sxs-lookup"><span data-stu-id="81da8-309">The Entity Framework codeplex project currently has a discussion page on this feature request.</span></span> <span data-ttu-id="81da8-310">Om du vill visa den här diskussionen [Design mötesanteckningar - 2 augusti 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span><span class="sxs-lookup"><span data-stu-id="81da8-310">To view this discussion, see [Design Meeting Notes - August 2, 2012](http://entityframework.codeplex.com/wikipage?title=Design%20Meeting%20Notes%20-%20August%202%2c%202012).</span></span>

### <a name="xml"></a><span data-ttu-id="81da8-311">XML</span><span class="sxs-lookup"><span data-stu-id="81da8-311">XML</span></span>
<span data-ttu-id="81da8-312">För fullständighetens skull bör du att det är viktigt att tala om XML-Datatypen som en batching strategi.</span><span class="sxs-lookup"><span data-stu-id="81da8-312">For completeness, we feel that it is important to talk about XML as a batching strategy.</span></span> <span data-ttu-id="81da8-313">Användning av XML har dock inga fördelar över andra metoder och flera nackdelar.</span><span class="sxs-lookup"><span data-stu-id="81da8-313">However, the use of XML has no advantages over other methods and several disadvantages.</span></span> <span data-ttu-id="81da8-314">Metoden som liknar tabellvärdeparametrar, men en XML-fil eller sträng har överförts till en lagrad procedur i stället för en användardefinierad tabell.</span><span class="sxs-lookup"><span data-stu-id="81da8-314">The approach is similar to table-valued parameters, but an XML file or string is passed to a stored procedure instead of a user-defined table.</span></span> <span data-ttu-id="81da8-315">Den lagrade proceduren Parsar kommandon i den lagrade proceduren.</span><span class="sxs-lookup"><span data-stu-id="81da8-315">The stored procedure parses the commands in the stored procedure.</span></span>

<span data-ttu-id="81da8-316">Det finns flera nackdelar med att den här metoden:</span><span class="sxs-lookup"><span data-stu-id="81da8-316">There are several disadvantages to this approach:</span></span>

* <span data-ttu-id="81da8-317">Arbeta med XML kan vara krånglig och tillförlitligt.</span><span class="sxs-lookup"><span data-stu-id="81da8-317">Working with XML can be cumbersome and error prone.</span></span>
* <span data-ttu-id="81da8-318">Parsning av XML-filen på databasen kan vara Processorn märkbart.</span><span class="sxs-lookup"><span data-stu-id="81da8-318">Parsing the XML on the database can be CPU-intensive.</span></span>
* <span data-ttu-id="81da8-319">I de flesta fall är den här metoden går långsammare än om tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="81da8-319">In most cases, this method is slower than table-valued parameters.</span></span>

<span data-ttu-id="81da8-320">Därför rekommenderas inte användning av XML för batch-frågor.</span><span class="sxs-lookup"><span data-stu-id="81da8-320">For these reasons, the use of XML for batch queries is not recommended.</span></span>

## <a name="batching-considerations"></a><span data-ttu-id="81da8-321">Batchbearbetning överväganden</span><span class="sxs-lookup"><span data-stu-id="81da8-321">Batching considerations</span></span>
<span data-ttu-id="81da8-322">Följande avsnitt innehåller mer hjälp för användning av batchbearbetning i SQL Database-program.</span><span class="sxs-lookup"><span data-stu-id="81da8-322">The following sections provide more guidance for the use of batching in SQL Database applications.</span></span>

### <a name="tradeoffs"></a><span data-ttu-id="81da8-323">Nackdelar</span><span class="sxs-lookup"><span data-stu-id="81da8-323">Tradeoffs</span></span>
<span data-ttu-id="81da8-324">Beroende på din arkitektur involverar batchbearbetning en kompromiss mellan prestanda och återhämtning.</span><span class="sxs-lookup"><span data-stu-id="81da8-324">Depending on your architecture, batching can involve a tradeoff between performance and resiliency.</span></span> <span data-ttu-id="81da8-325">Tänk dig ett scenario där din roll oväntat stängs av.</span><span class="sxs-lookup"><span data-stu-id="81da8-325">For example, consider the scenario where your role unexpectedly goes down.</span></span> <span data-ttu-id="81da8-326">Om du tappar bort en rad med data är effekten mindre än effekten av att förlora en stor grupp med rader som inte har skickats.</span><span class="sxs-lookup"><span data-stu-id="81da8-326">If you lose one row of data, the impact is smaller than the impact of losing a large batch of unsubmitted rows.</span></span> <span data-ttu-id="81da8-327">Det finns en större risk när du buffert rader innan de skickas till databasen i ett angivet tidsintervall.</span><span class="sxs-lookup"><span data-stu-id="81da8-327">There is a greater risk when you buffer rows before sending them to the database in a specified time window.</span></span>

<span data-ttu-id="81da8-328">På grund av det här förhållandet utvärdera vilken typ av åtgärder som du batch.</span><span class="sxs-lookup"><span data-stu-id="81da8-328">Because of this tradeoff, evaluate the type of operations that you batch.</span></span> <span data-ttu-id="81da8-329">Batch mer aggressivt (större batchar och längre tidsfönster) med data som är mindre viktigt.</span><span class="sxs-lookup"><span data-stu-id="81da8-329">Batch more aggressively (larger batches and longer time windows) with data that is less critical.</span></span>

### <a name="batch-size"></a><span data-ttu-id="81da8-330">Batchstorlek</span><span class="sxs-lookup"><span data-stu-id="81da8-330">Batch size</span></span>
<span data-ttu-id="81da8-331">I våra tester fanns det vanligtvis ingen fördel med att dela upp stora batchar i mindre segment.</span><span class="sxs-lookup"><span data-stu-id="81da8-331">In our tests, there was typically no advantage to breaking large batches into smaller chunks.</span></span> <span data-ttu-id="81da8-332">Faktum är resulterade ofta den här delfältet i långsammare än att skicka en enda stor grupp.</span><span class="sxs-lookup"><span data-stu-id="81da8-332">In fact, this subdivision often resulted in slower performance than submitting a single large batch.</span></span> <span data-ttu-id="81da8-333">Tänk dig ett scenario där du vill infoga 1000 översta raderna.</span><span class="sxs-lookup"><span data-stu-id="81da8-333">For example, consider a scenario where you want to insert 1000 rows.</span></span> <span data-ttu-id="81da8-334">Följande tabell visar hur lång tid det tar för att använda tabellvärdeparametrar för att infoga 1000 översta raderna när indelat i mindre batchar.</span><span class="sxs-lookup"><span data-stu-id="81da8-334">The following table shows how long it takes to use table-valued parameters to insert 1000 rows when divided into smaller batches.</span></span>

| <span data-ttu-id="81da8-335">Batchstorlek</span><span class="sxs-lookup"><span data-stu-id="81da8-335">Batch size</span></span> | <span data-ttu-id="81da8-336">Upprepningar</span><span class="sxs-lookup"><span data-stu-id="81da8-336">Iterations</span></span> | <span data-ttu-id="81da8-337">Tabellvärdeparametrar (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-337">Table-valued parameters (ms)</span></span> |
| --- | --- | --- |
| <span data-ttu-id="81da8-338">1000</span><span class="sxs-lookup"><span data-stu-id="81da8-338">1000</span></span> |<span data-ttu-id="81da8-339">1</span><span class="sxs-lookup"><span data-stu-id="81da8-339">1</span></span> |<span data-ttu-id="81da8-340">347</span><span class="sxs-lookup"><span data-stu-id="81da8-340">347</span></span> |
| <span data-ttu-id="81da8-341">500</span><span class="sxs-lookup"><span data-stu-id="81da8-341">500</span></span> |<span data-ttu-id="81da8-342">2</span><span class="sxs-lookup"><span data-stu-id="81da8-342">2</span></span> |<span data-ttu-id="81da8-343">355</span><span class="sxs-lookup"><span data-stu-id="81da8-343">355</span></span> |
| <span data-ttu-id="81da8-344">100</span><span class="sxs-lookup"><span data-stu-id="81da8-344">100</span></span> |<span data-ttu-id="81da8-345">10</span><span class="sxs-lookup"><span data-stu-id="81da8-345">10</span></span> |<span data-ttu-id="81da8-346">465</span><span class="sxs-lookup"><span data-stu-id="81da8-346">465</span></span> |
| <span data-ttu-id="81da8-347">50</span><span class="sxs-lookup"><span data-stu-id="81da8-347">50</span></span> |<span data-ttu-id="81da8-348">20</span><span class="sxs-lookup"><span data-stu-id="81da8-348">20</span></span> |<span data-ttu-id="81da8-349">630</span><span class="sxs-lookup"><span data-stu-id="81da8-349">630</span></span> |

> [!NOTE]
> <span data-ttu-id="81da8-350">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="81da8-350">Results are not benchmarks.</span></span> <span data-ttu-id="81da8-351">Finns det [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="81da8-351">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="81da8-352">Du kan se att bästa prestanda för 1000 översta raderna är att skicka dem på samma gång.</span><span class="sxs-lookup"><span data-stu-id="81da8-352">You can see that the best performance for 1000 rows is to submit them all at once.</span></span> <span data-ttu-id="81da8-353">Det uppstod små prestandafördelar att dela en 10000 raden batch i två grupper med 5000 i andra tester (visas inte här).</span><span class="sxs-lookup"><span data-stu-id="81da8-353">In other tests (not shown here) there was a small performance gain to break a 10000 row batch into two batches of 5000.</span></span> <span data-ttu-id="81da8-354">Men tabellschemat för dessa tester är relativt enkla, så du bör utföra tester på specifika data och batch-storlekar för att verifiera dessa resultat.</span><span class="sxs-lookup"><span data-stu-id="81da8-354">But the table schema for these tests is relatively simple, so you should perform tests on your specific data and batch sizes to verify these findings.</span></span>

<span data-ttu-id="81da8-355">En annan faktor är att om den totala batchen blir för stor, SQL-databas kan begränsa vägrar att bekräfta batchen.</span><span class="sxs-lookup"><span data-stu-id="81da8-355">Another factor to consider is that if the total batch becomes too large, SQL Database might throttle and refuse to commit the batch.</span></span> <span data-ttu-id="81da8-356">Testa din situation för att fastställa om det finns en perfekt batchstorlek för bästa resultat.</span><span class="sxs-lookup"><span data-stu-id="81da8-356">For the best results, test your specific scenario to determine if there is an ideal batch size.</span></span> <span data-ttu-id="81da8-357">Se batchstorleken konfigureras vid körningen för snabba justeringar utifrån prestanda eller fel.</span><span class="sxs-lookup"><span data-stu-id="81da8-357">Make the batch size configurable at runtime to enable quick adjustments based on performance or errors.</span></span>

<span data-ttu-id="81da8-358">Slutligen balansera storlek på batch med risker med batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="81da8-358">Finally, balance the size of the batch with the risks associated with batching.</span></span> <span data-ttu-id="81da8-359">Om det är tillfälliga fel eller rollen misslyckas, bör du konsekvenserna av du försöker igen eller förlust av data i batchen.</span><span class="sxs-lookup"><span data-stu-id="81da8-359">If there are transient errors or the role fails, consider the consequences of retrying the operation or of losing the data in the batch.</span></span>

### <a name="parallel-processing"></a><span data-ttu-id="81da8-360">Parallell bearbetning</span><span class="sxs-lookup"><span data-stu-id="81da8-360">Parallel processing</span></span>
<span data-ttu-id="81da8-361">Vad händer om du tog metod för att minska batchstorleken men använda flera trådar för att utföra arbetet?</span><span class="sxs-lookup"><span data-stu-id="81da8-361">What if you took the approach of reducing the batch size but used multiple threads to execute the work?</span></span> <span data-ttu-id="81da8-362">Våra tester visade igen att flera mindre flertrådade batchar vanligtvis utförs värre än en enskild större batch.</span><span class="sxs-lookup"><span data-stu-id="81da8-362">Again, our tests showed that several smaller multithreaded batches typically performed worse than a single larger batch.</span></span> <span data-ttu-id="81da8-363">Följande test försöker infoga 1000 rader i en eller flera parallella batchar.</span><span class="sxs-lookup"><span data-stu-id="81da8-363">The following test attempts to insert 1000 rows in one or more parallel batches.</span></span> <span data-ttu-id="81da8-364">Det här testet visar hur flera samtidiga batchar faktiskt minskade prestanda.</span><span class="sxs-lookup"><span data-stu-id="81da8-364">This test shows how more simultaneous batches actually decreased performance.</span></span>

| <span data-ttu-id="81da8-365">Batchstorlek [iterationer]</span><span class="sxs-lookup"><span data-stu-id="81da8-365">Batch size [Iterations]</span></span> | <span data-ttu-id="81da8-366">Två trådar (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-366">Two threads (ms)</span></span> | <span data-ttu-id="81da8-367">Fyra trådar (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-367">Four threads (ms)</span></span> | <span data-ttu-id="81da8-368">Sex trådar (ms)</span><span class="sxs-lookup"><span data-stu-id="81da8-368">Six threads (ms)</span></span> |
| --- | --- | --- | --- |
| <span data-ttu-id="81da8-369">1000 [1]</span><span class="sxs-lookup"><span data-stu-id="81da8-369">1000 [1]</span></span> |<span data-ttu-id="81da8-370">277</span><span class="sxs-lookup"><span data-stu-id="81da8-370">277</span></span> |<span data-ttu-id="81da8-371">315</span><span class="sxs-lookup"><span data-stu-id="81da8-371">315</span></span> |<span data-ttu-id="81da8-372">266</span><span class="sxs-lookup"><span data-stu-id="81da8-372">266</span></span> |
| <span data-ttu-id="81da8-373">500 [2]</span><span class="sxs-lookup"><span data-stu-id="81da8-373">500 [2]</span></span> |<span data-ttu-id="81da8-374">548</span><span class="sxs-lookup"><span data-stu-id="81da8-374">548</span></span> |<span data-ttu-id="81da8-375">278</span><span class="sxs-lookup"><span data-stu-id="81da8-375">278</span></span> |<span data-ttu-id="81da8-376">256</span><span class="sxs-lookup"><span data-stu-id="81da8-376">256</span></span> |
| <span data-ttu-id="81da8-377">250 [4]</span><span class="sxs-lookup"><span data-stu-id="81da8-377">250 [4]</span></span> |<span data-ttu-id="81da8-378">405</span><span class="sxs-lookup"><span data-stu-id="81da8-378">405</span></span> |<span data-ttu-id="81da8-379">329</span><span class="sxs-lookup"><span data-stu-id="81da8-379">329</span></span> |<span data-ttu-id="81da8-380">265</span><span class="sxs-lookup"><span data-stu-id="81da8-380">265</span></span> |
| <span data-ttu-id="81da8-381">100 [10]</span><span class="sxs-lookup"><span data-stu-id="81da8-381">100 [10]</span></span> |<span data-ttu-id="81da8-382">488</span><span class="sxs-lookup"><span data-stu-id="81da8-382">488</span></span> |<span data-ttu-id="81da8-383">439</span><span class="sxs-lookup"><span data-stu-id="81da8-383">439</span></span> |<span data-ttu-id="81da8-384">391</span><span class="sxs-lookup"><span data-stu-id="81da8-384">391</span></span> |

> [!NOTE]
> <span data-ttu-id="81da8-385">Resultatet är inte prestandamått.</span><span class="sxs-lookup"><span data-stu-id="81da8-385">Results are not benchmarks.</span></span> <span data-ttu-id="81da8-386">Finns det [anteckning om tidsinställning resultat i det här avsnittet](#note-about-timing-results-in-this-topic).</span><span class="sxs-lookup"><span data-stu-id="81da8-386">See the [note about timing results in this topic](#note-about-timing-results-in-this-topic).</span></span>
> 
> 

<span data-ttu-id="81da8-387">Det finns flera möjliga orsaker till sämre prestanda på grund av parallellitet:</span><span class="sxs-lookup"><span data-stu-id="81da8-387">There are several potential reasons for the degradation in performance due to parallelism:</span></span>

* <span data-ttu-id="81da8-388">Det finns flera samtidiga nätverket anrop i stället för en.</span><span class="sxs-lookup"><span data-stu-id="81da8-388">There are multiple simultaneous network calls instead of one.</span></span>
* <span data-ttu-id="81da8-389">Flera åtgärder mot en tabell kan resultera i konkurrens och blockerar.</span><span class="sxs-lookup"><span data-stu-id="81da8-389">Multiple operations against a single table can result in contention and blocking.</span></span>
* <span data-ttu-id="81da8-390">Det finns kostnader som flertrådsteknik.</span><span class="sxs-lookup"><span data-stu-id="81da8-390">There are overheads associated with multithreading.</span></span>
* <span data-ttu-id="81da8-391">Kostnaden för öppna flera anslutningar uppväger fördelen med parallell bearbetning.</span><span class="sxs-lookup"><span data-stu-id="81da8-391">The expense of opening multiple connections outweighs the benefit of parallel processing.</span></span>

<span data-ttu-id="81da8-392">Om du anpassar olika tabeller eller databaser, är det möjligt att se vissa prestanda få med den här strategin.</span><span class="sxs-lookup"><span data-stu-id="81da8-392">If you target different tables or databases, it is possible to see some performance gain with this strategy.</span></span> <span data-ttu-id="81da8-393">Horisontell partitionering databasen eller federationer är ett scenario för den här metoden.</span><span class="sxs-lookup"><span data-stu-id="81da8-393">Database sharding or federations would be a scenario for this approach.</span></span> <span data-ttu-id="81da8-394">Horisontell partitionering använder flera databaser och dirigerar olika data för varje databas.</span><span class="sxs-lookup"><span data-stu-id="81da8-394">Sharding uses multiple databases and routes different data to each database.</span></span> <span data-ttu-id="81da8-395">Om en annan databas ska varje liten grupp, kan utför operationerna parallellt vara mer effektivt.</span><span class="sxs-lookup"><span data-stu-id="81da8-395">If each small batch is going to a different database, then performing the operations in parallel can be more efficient.</span></span> <span data-ttu-id="81da8-396">Prestanda är inte tillräckligt stor för ska användas som grund för ett beslut för att använda databasen delning i din lösning.</span><span class="sxs-lookup"><span data-stu-id="81da8-396">However, the performance gain is not significant enough to use as the basis for a decision to use database sharding in your solution.</span></span>

<span data-ttu-id="81da8-397">I vissa Designer leda parallell körning av mindre batchar till bättre genomflöde begäranden i ett system under belastning.</span><span class="sxs-lookup"><span data-stu-id="81da8-397">In some designs, parallel execution of smaller batches can result in improved throughput of requests in a system under load.</span></span> <span data-ttu-id="81da8-398">I det här fallet trots att det går snabbare att bearbeta en enskild större batch kan bearbeta flera batchar parallellt vara mer effektivt.</span><span class="sxs-lookup"><span data-stu-id="81da8-398">In this case, even though it is quicker to process a single larger batch, processing multiple batches in parallel might be more efficient.</span></span>

<span data-ttu-id="81da8-399">Om du använder parallell körning, bör du kontrollera det maximala antalet trådar.</span><span class="sxs-lookup"><span data-stu-id="81da8-399">If you do use parallel execution, consider controlling the maximum number of worker threads.</span></span> <span data-ttu-id="81da8-400">Ett mindre antal leda till mindre konkurrens och snabbare körningstid.</span><span class="sxs-lookup"><span data-stu-id="81da8-400">A smaller number might result in less contention and a faster execution time.</span></span> <span data-ttu-id="81da8-401">Du kan också den ytterligare belastningen som placeras på måldatabasen både anslutningar och transaktioner.</span><span class="sxs-lookup"><span data-stu-id="81da8-401">Also, consider the additional load that this places on the target database both in connections and transactions.</span></span>

### <a name="related-performance-factors"></a><span data-ttu-id="81da8-402">Prestandadata relaterade faktorer</span><span class="sxs-lookup"><span data-stu-id="81da8-402">Related performance factors</span></span>
<span data-ttu-id="81da8-403">Vanliga vägledning om databasprestanda påverkar också batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="81da8-403">Typical guidance on database performance also affects batching.</span></span> <span data-ttu-id="81da8-404">Till exempel infoga minskar prestanda för tabeller som har en primärnyckel för stora eller många icke-grupperat index.</span><span class="sxs-lookup"><span data-stu-id="81da8-404">For example, insert performance is reduced for tables that have a large primary key or many nonclustered indexes.</span></span>

<span data-ttu-id="81da8-405">Om tabellvärdeparametrar använder en lagrad procedur kan du använda kommandot **SET NOCOUNT ON** i början av proceduren.</span><span class="sxs-lookup"><span data-stu-id="81da8-405">If table-valued parameters use a stored procedure, you can use the command **SET NOCOUNT ON** at the beginning of the procedure.</span></span> <span data-ttu-id="81da8-406">Den här instruktionen Undertrycker avkastning för antalet berörda rader i proceduren.</span><span class="sxs-lookup"><span data-stu-id="81da8-406">This statement suppresses the return of the count of the affected rows in the procedure.</span></span> <span data-ttu-id="81da8-407">Men i våra tester kan användningen av **SET NOCOUNT ON** inte påverkar eller minskade prestanda.</span><span class="sxs-lookup"><span data-stu-id="81da8-407">However, in our tests, the use of **SET NOCOUNT ON** either had no effect or decreased performance.</span></span> <span data-ttu-id="81da8-408">Testa lagrade proceduren har enkelt med en enda **infoga** från parametern-tabellvärdesfunktion.</span><span class="sxs-lookup"><span data-stu-id="81da8-408">The test stored procedure was simple with a single **INSERT** command from the table-valued parameter.</span></span> <span data-ttu-id="81da8-409">Det är möjligt att mer komplexa lagrade procedurer skulle dra nytta av den här instruktionen.</span><span class="sxs-lookup"><span data-stu-id="81da8-409">It is possible that more complex stored procedures would benefit from this statement.</span></span> <span data-ttu-id="81da8-410">Men förutsätt inte att lägga till **SET NOCOUNT ON** till den lagrade proceduren automatiskt förbättrar prestanda.</span><span class="sxs-lookup"><span data-stu-id="81da8-410">But don’t assume that adding **SET NOCOUNT ON** to your stored procedure automatically improves performance.</span></span> <span data-ttu-id="81da8-411">Testa den lagrade proceduren med och utan för att förstå effekten av **SET NOCOUNT ON** instruktionen.</span><span class="sxs-lookup"><span data-stu-id="81da8-411">To understand the effect, test your stored procedure with and without the **SET NOCOUNT ON** statement.</span></span>

## <a name="batching-scenarios"></a><span data-ttu-id="81da8-412">Batchbearbetning scenarier</span><span class="sxs-lookup"><span data-stu-id="81da8-412">Batching scenarios</span></span>
<span data-ttu-id="81da8-413">I följande avsnitt beskrivs hur du använder tabellvärdeparametrar i tre scenarier för programmet.</span><span class="sxs-lookup"><span data-stu-id="81da8-413">The following sections describe how to use table-valued parameters in three application scenarios.</span></span> <span data-ttu-id="81da8-414">Det första scenariot visar hur buffring och batchbearbetning kan fungera tillsammans.</span><span class="sxs-lookup"><span data-stu-id="81da8-414">The first scenario shows how buffering and batching can work together.</span></span> <span data-ttu-id="81da8-415">Det andra scenariot förbättrar prestanda genom att utföra åtgärder för master-detaljer i en enda lagrade proceduranropet.</span><span class="sxs-lookup"><span data-stu-id="81da8-415">The second scenario improves performance by performing master-detail operations in a single stored procedure call.</span></span> <span data-ttu-id="81da8-416">Det sista scenariot visar hur du använder tabellvärdeparametrar i en ”UPSERT”-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="81da8-416">The final scenario shows how to use table-valued parameters in an “UPSERT” operation.</span></span>

### <a name="buffering"></a><span data-ttu-id="81da8-417">Buffring</span><span class="sxs-lookup"><span data-stu-id="81da8-417">Buffering</span></span>
<span data-ttu-id="81da8-418">Men det är några scenarier som är uppenbara kandidat för batchbearbetning, finns det många scenarier som kan dra nytta av batchbearbetning av fördröjd bearbetning.</span><span class="sxs-lookup"><span data-stu-id="81da8-418">Although there are some scenarios that are obvious candidate for batching, there are many scenarios that could take advantage of batching by delayed processing.</span></span> <span data-ttu-id="81da8-419">Fördröjd bearbetningen har dock en större risk att data förloras vid ett oväntat fel.</span><span class="sxs-lookup"><span data-stu-id="81da8-419">However, delayed processing also carries a greater risk that the data is lost in the event of an unexpected failure.</span></span> <span data-ttu-id="81da8-420">Det är viktigt att förstå den här risken och Överväg konsekvenserna.</span><span class="sxs-lookup"><span data-stu-id="81da8-420">It is important to understand this risk and consider the consequences.</span></span>

<span data-ttu-id="81da8-421">Tänk dig ett webbprogram som spårar navigeringshistoriken för varje användare.</span><span class="sxs-lookup"><span data-stu-id="81da8-421">For example, consider a web application that tracks the navigation history of each user.</span></span> <span data-ttu-id="81da8-422">Programmet kunde göra en databas-anrop för att registrera vyn sida på varje sida i begäran.</span><span class="sxs-lookup"><span data-stu-id="81da8-422">On each page request, the application could make a database call to record the user’s page view.</span></span> <span data-ttu-id="81da8-423">Men högre prestanda och skalbarhet kan uppnås genom buffring användarnas navigering aktiviteter och informationen skickas till databasen i batchar.</span><span class="sxs-lookup"><span data-stu-id="81da8-423">But higher performance and scalability can be achieved by buffering the users’ navigation activities and then sending this data to the database in batches.</span></span> <span data-ttu-id="81da8-424">Du kan utlösa database-uppdateringen av förfluten tid och/eller buffertstorlek.</span><span class="sxs-lookup"><span data-stu-id="81da8-424">You can trigger the database update by elapsed time and/or buffer size.</span></span> <span data-ttu-id="81da8-425">En regel kan till exempel ange att gruppen ska bearbetas efter 20 sekunder eller när bufferten når 1 000 objekt.</span><span class="sxs-lookup"><span data-stu-id="81da8-425">For example, a rule could specify that the batch should be processed after 20 seconds or when the buffer reaches 1000 items.</span></span>

<span data-ttu-id="81da8-426">Följande kodexempel används [reaktiv tillägg - Rx](https://msdn.microsoft.com/data/gg577609) bearbeta buffertlagrade händelser som skapats av en övervakningsklass.</span><span class="sxs-lookup"><span data-stu-id="81da8-426">The following code example uses [Reactive Extensions - Rx](https://msdn.microsoft.com/data/gg577609) to process buffered events raised by a monitoring class.</span></span> <span data-ttu-id="81da8-427">När bufferten fyller eller en tidsgräns uppnås, i gruppen med användardata skickas till databasen med en tabellvärdesparameter.</span><span class="sxs-lookup"><span data-stu-id="81da8-427">When the buffer fills or a timeout is reached, the batch of user data is sent to the database with a table-valued parameter.</span></span>

<span data-ttu-id="81da8-428">Följande NavHistoryData klass modeller navigering användarinformation.</span><span class="sxs-lookup"><span data-stu-id="81da8-428">The following NavHistoryData class models the user navigation details.</span></span> <span data-ttu-id="81da8-429">Den innehåller grundläggande information, till exempel användar-ID, URL: en som öppnas och åtkomst-tid.</span><span class="sxs-lookup"><span data-stu-id="81da8-429">It contains basic information such as the user identifier, the URL accessed, and the access time.</span></span>

    public class NavHistoryData
    {
        public NavHistoryData(int userId, string url, DateTime accessTime)
        { UserId = userId; URL = url; AccessTime = accessTime; }
        public int UserId { get; set; }
        public string URL { get; set; }
        public DateTime AccessTime { get; set; }
    }

<span data-ttu-id="81da8-430">Klassen NavHistoryDataMonitor ansvarar för buffring navigering användardata till databasen.</span><span class="sxs-lookup"><span data-stu-id="81da8-430">The NavHistoryDataMonitor class is responsible for buffering the user navigation data to the database.</span></span> <span data-ttu-id="81da8-431">Den innehåller en metod, RecordUserNavigationEntry som svarar genom att en **OnAdded** händelse.</span><span class="sxs-lookup"><span data-stu-id="81da8-431">It contains a method, RecordUserNavigationEntry, which responds by raising an **OnAdded** event.</span></span> <span data-ttu-id="81da8-432">Följande kod visar konstruktorn logiken som använder Rx för att skapa en synliga samling baserat på händelsen.</span><span class="sxs-lookup"><span data-stu-id="81da8-432">The following code shows the constructor logic that uses Rx to create an observable collection based on the event.</span></span> <span data-ttu-id="81da8-433">Den sedan prenumererar synliga samlingen med metoden buffert.</span><span class="sxs-lookup"><span data-stu-id="81da8-433">It then subscribes to this observable collection with the Buffer method.</span></span> <span data-ttu-id="81da8-434">Överlagring anger att bufferten ska skickas varje 20 sekunder eller 1000 poster.</span><span class="sxs-lookup"><span data-stu-id="81da8-434">The overload specifies that the buffer should be sent every 20 seconds or 1000 entries.</span></span>

    public NavHistoryDataMonitor()
    {
        var observableData =
            Observable.FromEventPattern<NavHistoryDataEventArgs>(this, "OnAdded");

        observableData.Buffer(TimeSpan.FromSeconds(20), 1000).Subscribe(Handler);           
    }

<span data-ttu-id="81da8-435">Hanteraren konverterar alla buffrade objekt till en tabellvärdesfunktion typ och skickar sedan denna typ till en lagrad procedur som bearbetar batchen.</span><span class="sxs-lookup"><span data-stu-id="81da8-435">The handler converts all of the buffered items into a table-valued type and then passes this type to a stored procedure that processes the batch.</span></span> <span data-ttu-id="81da8-436">Följande kod visar slutförd definitionen för både NavHistoryDataEventArgs och NavHistoryDataMonitor-klasser.</span><span class="sxs-lookup"><span data-stu-id="81da8-436">The following code shows the complete definition for both the NavHistoryDataEventArgs and the NavHistoryDataMonitor classes.</span></span>

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

<span data-ttu-id="81da8-437">Om du vill använda den här buffring klassen skapar programmet ett statiskt NavHistoryDataMonitor-objekt.</span><span class="sxs-lookup"><span data-stu-id="81da8-437">To use this buffering class, the application creates a static NavHistoryDataMonitor object.</span></span> <span data-ttu-id="81da8-438">Varje gång en användare ansluter till en sida anropar programmet metoden NavHistoryDataMonitor.RecordUserNavigationEntry.</span><span class="sxs-lookup"><span data-stu-id="81da8-438">Each time a user accesses a page, the application calls the NavHistoryDataMonitor.RecordUserNavigationEntry method.</span></span> <span data-ttu-id="81da8-439">Buffring logiken fortsätter att vara noga med att skicka dessa poster till databasen i batchar.</span><span class="sxs-lookup"><span data-stu-id="81da8-439">The buffering logic proceeds to take care of sending these entries to the database in batches.</span></span>

### <a name="master-detail"></a><span data-ttu-id="81da8-440">Master detaljer</span><span class="sxs-lookup"><span data-stu-id="81da8-440">Master detail</span></span>
<span data-ttu-id="81da8-441">Tabellvärdeparametrar är användbara för enkla INSERT-scenarier.</span><span class="sxs-lookup"><span data-stu-id="81da8-441">Table-valued parameters are useful for simple INSERT scenarios.</span></span> <span data-ttu-id="81da8-442">Det kan vara svårare att batch infogningar som omfattar mer än en tabell.</span><span class="sxs-lookup"><span data-stu-id="81da8-442">However, it can be more challenging to batch inserts that involve more than one table.</span></span> <span data-ttu-id="81da8-443">”Översikt/detaljer” scenario är ett bra exempel.</span><span class="sxs-lookup"><span data-stu-id="81da8-443">The “master/detail” scenario is a good example.</span></span> <span data-ttu-id="81da8-444">Huvudtabellen identifierar den primära entiteten.</span><span class="sxs-lookup"><span data-stu-id="81da8-444">The master table identifies the primary entity.</span></span> <span data-ttu-id="81da8-445">En eller flera tabeller i detalj lagra mer data om enheten.</span><span class="sxs-lookup"><span data-stu-id="81da8-445">One or more detail tables store more data about the entity.</span></span> <span data-ttu-id="81da8-446">I det här scenariot framtvinga sekundärnyckelrelationer relationen mellan information till en unik master entitet.</span><span class="sxs-lookup"><span data-stu-id="81da8-446">In this scenario, foreign key relationships enforce the relationship of details to a unique master entity.</span></span> <span data-ttu-id="81da8-447">Överväg att en förenklad version av PurchaseOrder tabellerna och dess associerade OrderDetail.</span><span class="sxs-lookup"><span data-stu-id="81da8-447">Consider a simplified version of a PurchaseOrder table and its associated OrderDetail table.</span></span> <span data-ttu-id="81da8-448">Följande Transact-SQL skapar PurchaseOrder-tabell med fyra kolumner: OrderID, OrderDate, CustomerID och Status.</span><span class="sxs-lookup"><span data-stu-id="81da8-448">The following Transact-SQL creates the PurchaseOrder table with four columns: OrderID, OrderDate, CustomerID, and Status.</span></span>

    CREATE TABLE [dbo].[PurchaseOrder](
    [OrderID] [int] IDENTITY(1,1) NOT NULL,
    [OrderDate] [datetime] NOT NULL,
    [CustomerID] [int] NOT NULL,
    [Status] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrder] 
    PRIMARY KEY CLUSTERED ( [OrderID] ASC ))

<span data-ttu-id="81da8-449">Varje innehåller en eller flera produktinköp.</span><span class="sxs-lookup"><span data-stu-id="81da8-449">Each order contains one or more product purchases.</span></span> <span data-ttu-id="81da8-450">Den här informationen samlas i tabellen PurchaseOrderDetail.</span><span class="sxs-lookup"><span data-stu-id="81da8-450">This information is captured in the PurchaseOrderDetail table.</span></span> <span data-ttu-id="81da8-451">Följande Transact-SQL skapar PurchaseOrderDetail-tabell med fem kolumner: OrderID, OrderDetailID, ProductID, Enhetspris och OrderQty.</span><span class="sxs-lookup"><span data-stu-id="81da8-451">The following Transact-SQL creates the PurchaseOrderDetail table with five columns: OrderID, OrderDetailID, ProductID, UnitPrice, and OrderQty.</span></span>

    CREATE TABLE [dbo].[PurchaseOrderDetail](
    [OrderID] [int] NOT NULL,
    [OrderDetailID] [int] IDENTITY(1,1) NOT NULL,
    [ProductID] [int] NOT NULL,
    [UnitPrice] [money] NULL,
    [OrderQty] [smallint] NULL,
     CONSTRAINT [PrimaryKey_PurchaseOrderDetail] PRIMARY KEY CLUSTERED 
    ( [OrderID] ASC, [OrderDetailID] ASC ))

<span data-ttu-id="81da8-452">En order måste referera till kolumnen OrderID i tabellen PurchaseOrderDetail från tabellen PurchaseOrder.</span><span class="sxs-lookup"><span data-stu-id="81da8-452">The OrderID column in the PurchaseOrderDetail table must reference an order from the PurchaseOrder table.</span></span> <span data-ttu-id="81da8-453">Följande definition av en sekundärnyckel tillämpar den här begränsningen.</span><span class="sxs-lookup"><span data-stu-id="81da8-453">The following definition of a foreign key enforces this constraint.</span></span>

    ALTER TABLE [dbo].[PurchaseOrderDetail]  WITH CHECK ADD 
    CONSTRAINT [FK_OrderID_PurchaseOrder] FOREIGN KEY([OrderID])
    REFERENCES [dbo].[PurchaseOrder] ([OrderID])

<span data-ttu-id="81da8-454">Du måste ha en användardefinierad tabelltyp för varje måltabellen för att kunna använda tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="81da8-454">In order to use table-valued parameters, you must have one user-defined table type for each target table.</span></span>

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

<span data-ttu-id="81da8-455">Definiera en lagrad procedur som accepterar tabeller för dessa typer.</span><span class="sxs-lookup"><span data-stu-id="81da8-455">Then define a stored procedure that accepts tables of these types.</span></span> <span data-ttu-id="81da8-456">Den här proceduren kan ett program till en uppsättning order och orderinformationen i ett enda anrop batch-lokalt.</span><span class="sxs-lookup"><span data-stu-id="81da8-456">This procedure allows an application to locally batch a set of orders and order details in a single call.</span></span> <span data-ttu-id="81da8-457">Följande Transact-SQL ger fullständig lagrade procedur-deklaration för det här köpet ordning exemplet.</span><span class="sxs-lookup"><span data-stu-id="81da8-457">The following Transact-SQL provides the complete stored procedure declaration for this purchase order example.</span></span>

    CREATE PROCEDURE sp_InsertOrdersBatch (
    @orders as PurchaseOrderTableType READONLY,
    @details as PurchaseOrderDetailTableType READONLY )
    AS
    SET NOCOUNT ON;

    -- Table that connects the order identifiers in the @orders
    -- table with the actual order identifiers in the PurchaseOrder table
    DECLARE @IdentityLink AS TABLE ( 
    SubmittedKey int, 
    ActualKey int, 
    RowNumber int identity(1,1)
    );

          -- Add new orders to the PurchaseOrder table, storing the actual
    -- order identifiers in the @IdentityLink table   
    INSERT INTO PurchaseOrder ([OrderDate], [CustomerID], [Status])
    OUTPUT inserted.OrderID INTO @IdentityLink (ActualKey)
    SELECT [OrderDate], [CustomerID], [Status] FROM @orders ORDER BY OrderID;

    -- Match the passed-in order identifiers with the actual identifiers
    -- and complete the @IdentityLink table for use with inserting the details
    WITH OrderedRows As (
    SELECT OrderID, ROW_NUMBER () OVER (ORDER BY OrderID) As RowNumber 
    FROM @orders
    )
    UPDATE @IdentityLink SET SubmittedKey = M.OrderID
    FROM @IdentityLink L JOIN OrderedRows M ON L.RowNumber = M.RowNumber;

    -- Insert the order details into the PurchaseOrderDetail table, 
          -- using the actual order identifiers of the master table, PurchaseOrder
    INSERT INTO PurchaseOrderDetail (
    [OrderID],
    [ProductID],
    [UnitPrice],
    [OrderQty] )
    SELECT L.ActualKey, D.ProductID, D.UnitPrice, D.OrderQty
    FROM @details D
    JOIN @IdentityLink L ON L.SubmittedKey = D.OrderID;
    GO

<span data-ttu-id="81da8-458">I det här exemplet är det lokalt definierade @IdentityLink tabellen lagras de faktiska värdena OrderID från nyligen infogade rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-458">In this example, the locally defined @IdentityLink table stores the actual OrderID values from the newly inserted rows.</span></span> <span data-ttu-id="81da8-459">Dessa identifierare ordning skiljer sig från tillfälliga OrderID värdena i den @orders och @details tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="81da8-459">These order identifiers are different from the temporary OrderID values in the @orders and @details table-valued parameters.</span></span> <span data-ttu-id="81da8-460">Därför den @IdentityLink tabell ansluter sedan OrderID värdena från den @orders parameter till verkliga OrderID värdena för nya rader i tabellen PurchaseOrder.</span><span class="sxs-lookup"><span data-stu-id="81da8-460">For this reason, the @IdentityLink table then connects the OrderID values from the @orders parameter to the real OrderID values for the new rows in the PurchaseOrder table.</span></span> <span data-ttu-id="81da8-461">Efter det här steget i @IdentityLink tabell kan underlätta Infoga orderinformationen med faktiska OrderID som uppfyller foreign key-begränsningen.</span><span class="sxs-lookup"><span data-stu-id="81da8-461">After this step, the @IdentityLink table can facilitate inserting the order details with the actual OrderID that satisfies the foreign key constraint.</span></span>

<span data-ttu-id="81da8-462">Den här lagrade proceduren kan användas från kod eller andra Transact-SQL-anrop.</span><span class="sxs-lookup"><span data-stu-id="81da8-462">This stored procedure can be used from code or from other Transact-SQL calls.</span></span> <span data-ttu-id="81da8-463">Se avsnittet tabellvärdeparametrar för det här dokumentet som ett exempel.</span><span class="sxs-lookup"><span data-stu-id="81da8-463">See the table-valued parameters section of this paper for a code example.</span></span> <span data-ttu-id="81da8-464">Följande Transact-SQL visar hur du anropar sp_InsertOrdersBatch.</span><span class="sxs-lookup"><span data-stu-id="81da8-464">The following Transact-SQL shows how to call the sp_InsertOrdersBatch.</span></span>

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

<span data-ttu-id="81da8-465">Den här lösningen kan varje batch för att använda en uppsättning OrderID värden som börjar vid 1.</span><span class="sxs-lookup"><span data-stu-id="81da8-465">This solution allows each batch to use a set of OrderID values that begin at 1.</span></span> <span data-ttu-id="81da8-466">Värdena för tillfälliga OrderID beskriva relationer i batchen, men de faktiska värdena OrderID bestäms vid tidpunkten för insert-åtgärden.</span><span class="sxs-lookup"><span data-stu-id="81da8-466">These temporary OrderID values describe the relationships in the batch, but the actual OrderID values are determined at the time of the insert operation.</span></span> <span data-ttu-id="81da8-467">Du kan köra samma instruktioner i föregående exempel upprepade gånger och generera unika order i databasen.</span><span class="sxs-lookup"><span data-stu-id="81da8-467">You can run the same statements in the previous example repeatedly and generate unique orders in the database.</span></span> <span data-ttu-id="81da8-468">Överväg att lägga till mer kod eller databasen logik som förhindrar att duplicerade order när du använder detta batchbearbetning tekniken därför.</span><span class="sxs-lookup"><span data-stu-id="81da8-468">For this reason, consider adding more code or database logic that prevents duplicate orders when using this batching technique.</span></span>

<span data-ttu-id="81da8-469">Det här exemplet visar att ännu mer komplexa databasåtgärder, till exempel översikt detaljer operations kan grupperas med tabellvärdeparametrar.</span><span class="sxs-lookup"><span data-stu-id="81da8-469">This example demonstrates that even more complex database operations, such as master-detail operations, can be batched using table-valued parameters.</span></span>

### <a name="upsert"></a><span data-ttu-id="81da8-470">UPSERT</span><span class="sxs-lookup"><span data-stu-id="81da8-470">UPSERT</span></span>
<span data-ttu-id="81da8-471">En annan batching scenariet inbegriper samtidigt uppdaterar befintliga rader och infoga nya rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-471">Another batching scenario involves simultaneously updating existing rows and inserting new rows.</span></span> <span data-ttu-id="81da8-472">Den här åtgärden kallas ibland för en ”UPSERT” (update + insert)-åtgärd.</span><span class="sxs-lookup"><span data-stu-id="81da8-472">This operation is sometimes referred to as an “UPSERT” (update + insert) operation.</span></span> <span data-ttu-id="81da8-473">I stället för separata anropar infoga och uppdatera MERGE-instruktion som är bäst för den här uppgiften.</span><span class="sxs-lookup"><span data-stu-id="81da8-473">Rather than making separate calls to INSERT and UPDATE, the MERGE statement is best suited to this task.</span></span> <span data-ttu-id="81da8-474">MERGE-instruktion kan utföra både insert och uppdatera åtgärder i ett enda anrop.</span><span class="sxs-lookup"><span data-stu-id="81da8-474">The MERGE statement can perform both insert and update operations in a single call.</span></span>

<span data-ttu-id="81da8-475">Tabellvärdeparametrar kan användas med MERGE-instruktion för att utföra uppdateringar och infogningar.</span><span class="sxs-lookup"><span data-stu-id="81da8-475">Table-valued parameters can be used with the MERGE statement to perform updates and inserts.</span></span> <span data-ttu-id="81da8-476">Anta till exempel att en förenklad medarbetare tabell som innehåller följande kolumner: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span><span class="sxs-lookup"><span data-stu-id="81da8-476">For example, consider a simplified Employee table that contains the following columns: EmployeeID, FirstName, LastName, SocialSecurityNumber:</span></span>

    CREATE TABLE [dbo].[Employee](
    [EmployeeID] [int] IDENTITY(1,1) NOT NULL,
    [FirstName] [nvarchar](50) NOT NULL,
    [LastName] [nvarchar](50) NOT NULL,
    [SocialSecurityNumber] [nvarchar](50) NOT NULL,
     CONSTRAINT [PrimaryKey_Employee] PRIMARY KEY CLUSTERED 
    ([EmployeeID] ASC ))

<span data-ttu-id="81da8-477">Du kan använda det faktum att SocialSecurityNumber är unikt för dokument för flera anställda i det här exemplet.</span><span class="sxs-lookup"><span data-stu-id="81da8-477">In this example, you can use the fact that the SocialSecurityNumber is unique to perform a MERGE of multiple employees.</span></span> <span data-ttu-id="81da8-478">Först skapar du användardefinierade tabelltypen:</span><span class="sxs-lookup"><span data-stu-id="81da8-478">First, create the user-defined table type:</span></span>

    CREATE TYPE EmployeeTableType AS TABLE 
    ( Employee_ID INT,
      FirstName NVARCHAR(50),
      LastName NVARCHAR(50),
      SocialSecurityNumber NVARCHAR(50) );
    GO

<span data-ttu-id="81da8-479">Sedan skapar en lagrad procedur eller skriva kod som använder MERGE-instruktion för att utföra uppdateringen och infoga.</span><span class="sxs-lookup"><span data-stu-id="81da8-479">Next, create a stored procedure or write code that uses the MERGE statement to perform the update and insert.</span></span> <span data-ttu-id="81da8-480">I följande exempel används MERGE-instruktion på en tabellvärdesparameter @employees, av typen EmployeeTableType.</span><span class="sxs-lookup"><span data-stu-id="81da8-480">The following example uses the MERGE statement on a table-valued parameter, @employees, of type EmployeeTableType.</span></span> <span data-ttu-id="81da8-481">Innehållet i den @employees tabellen visas inte här.</span><span class="sxs-lookup"><span data-stu-id="81da8-481">The contents of the @employees table are not shown here.</span></span>

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

<span data-ttu-id="81da8-482">Mer information finns i dokumentation och exempel för MERGE-instruktion.</span><span class="sxs-lookup"><span data-stu-id="81da8-482">For more information, see the documentation and examples for the MERGE statement.</span></span> <span data-ttu-id="81da8-483">Även om samma arbets kunde utföras i en flera steg lagras proceduranropet med separata INSERT och UPDATE-åtgärder, MERGE-instruktionen är effektivare.</span><span class="sxs-lookup"><span data-stu-id="81da8-483">Although the same work could be performed in a multiple-step stored procedure call with separate INSERT and UPDATE operations, the MERGE statement is more efficient.</span></span> <span data-ttu-id="81da8-484">Databaskod kan också skapa Transact-SQL-anrop som MERGE-instruktion direkt utan att två databasanrop för INSERT och UPDATE.</span><span class="sxs-lookup"><span data-stu-id="81da8-484">Database code can also construct Transact-SQL calls that use the MERGE statement directly without requiring two database calls for INSERT and UPDATE.</span></span>

## <a name="recommendation-summary"></a><span data-ttu-id="81da8-485">Rekommendation sammanfattning</span><span class="sxs-lookup"><span data-stu-id="81da8-485">Recommendation summary</span></span>
<span data-ttu-id="81da8-486">Följande lista innehåller en sammanfattning av batching rekommendationerna som beskrivs i det här avsnittet:</span><span class="sxs-lookup"><span data-stu-id="81da8-486">The following list provides a summary of the batching recommendations discussed in this topic:</span></span>

* <span data-ttu-id="81da8-487">Använd buffring och batchbearbetning för att öka prestanda och skalbarhet SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="81da8-487">Use buffering and batching to increase the performance and scalability of SQL Database applications.</span></span>
* <span data-ttu-id="81da8-488">Förstå kompromisser mellan batchbearbetning/buffring och återhämtning.</span><span class="sxs-lookup"><span data-stu-id="81da8-488">Understand the tradeoffs between batching/buffering and resiliency.</span></span> <span data-ttu-id="81da8-489">Vid fel, roll, kan risken för att förlora en obearbetat batch affärskritiska data uppväger prestandafördelarna batchbearbetning.</span><span class="sxs-lookup"><span data-stu-id="81da8-489">During a role failure, the risk of losing an unprocessed batch of business-critical data might outweigh the performance benefit of batching.</span></span>
* <span data-ttu-id="81da8-490">Försök att hålla alla anrop till databasen i ett enda datacenter att minska svarstiden.</span><span class="sxs-lookup"><span data-stu-id="81da8-490">Attempt to keep all calls to the database within a single datacenter to reduce latency.</span></span>
* <span data-ttu-id="81da8-491">Om du väljer en enda batching teknik erbjuder tabellvärdeparametrar bästa prestanda och flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="81da8-491">If you choose a single batching technique, table-valued parameters offer the best performance and flexibility.</span></span>
* <span data-ttu-id="81da8-492">Följer dessa allmänna riktlinjer för bästa prestanda för insert, men testa ditt scenario:</span><span class="sxs-lookup"><span data-stu-id="81da8-492">For the fastest insert performance, follow these general guidelines but test your scenario:</span></span>
  * <span data-ttu-id="81da8-493">Använd ett enda parametrar INSERT-kommando för < 100 rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-493">For < 100 rows, use a single parameterized INSERT command.</span></span>
  * <span data-ttu-id="81da8-494">Använda tabellvärdeparametrar för < 1000 rader.</span><span class="sxs-lookup"><span data-stu-id="81da8-494">For < 1000 rows, use table-valued parameters.</span></span>
  * <span data-ttu-id="81da8-495">För > = 1000 rader, Använd SqlBulkCopy körs.</span><span class="sxs-lookup"><span data-stu-id="81da8-495">For >= 1000 rows, use SqlBulkCopy.</span></span>
* <span data-ttu-id="81da8-496">För update och delete-åtgärder kan använda tabellvärdeparametrar med lagrade proceduren logik som anger rätt igen på varje rad i tabellen-parametern.</span><span class="sxs-lookup"><span data-stu-id="81da8-496">For update and delete operations, use table-valued parameters with stored procedure logic that determines the correct operation on each row in the table parameter.</span></span>
* <span data-ttu-id="81da8-497">Riktlinjer för batch-storlek:</span><span class="sxs-lookup"><span data-stu-id="81da8-497">Batch size guidelines:</span></span>
  * <span data-ttu-id="81da8-498">Använd de största batch-storlekar som passar ditt program och affärskrav.</span><span class="sxs-lookup"><span data-stu-id="81da8-498">Use the largest batch sizes that make sense for your application and business requirements.</span></span>
  * <span data-ttu-id="81da8-499">Balansera prestandafördelar med stora batchar med risk för tillfälliga eller katastrofalt fel.</span><span class="sxs-lookup"><span data-stu-id="81da8-499">Balance the performance gain of large batches with the risks of temporary or catastrophic failures.</span></span> <span data-ttu-id="81da8-500">Vad är en följd av återförsök eller förlust av data i gruppen?</span><span class="sxs-lookup"><span data-stu-id="81da8-500">What is the consequence of retries or loss of the data in the batch?</span></span> 
  * <span data-ttu-id="81da8-501">Testa den största batchstorleken för att verifiera att SQL-databasen inte avvisa den.</span><span class="sxs-lookup"><span data-stu-id="81da8-501">Test the largest batch size to verify that SQL Database does not reject it.</span></span>
  * <span data-ttu-id="81da8-502">Skapa inställningar som kontrollen batchbearbetning, till exempel batchstorleken eller tidsfönstret buffring.</span><span class="sxs-lookup"><span data-stu-id="81da8-502">Create configuration settings that control batching, such as the batch size or the buffering time window.</span></span> <span data-ttu-id="81da8-503">Dessa inställningar ger flexibilitet.</span><span class="sxs-lookup"><span data-stu-id="81da8-503">These settings provide flexibility.</span></span> <span data-ttu-id="81da8-504">Du kan ändra batching beteendet i produktionsmiljö utan att omdistribuera Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="81da8-504">You can change the batching behavior in production without redeploying the cloud service.</span></span>
* <span data-ttu-id="81da8-505">Undvik parallell körning av batchar som fungerar på en enda tabell i en databas.</span><span class="sxs-lookup"><span data-stu-id="81da8-505">Avoid parallel execution of batches that operate on a single table in one database.</span></span> <span data-ttu-id="81da8-506">Om du väljer att dela upp en enskild batch över flera trådar, kör du testerna för att fastställa bästa antal trådar.</span><span class="sxs-lookup"><span data-stu-id="81da8-506">If you do choose to divide a single batch across multiple worker threads, run tests to determine the ideal number of threads.</span></span> <span data-ttu-id="81da8-507">När du har ett okänt tröskelvärde fler trådar kommer försämra prestanda i stället öka den.</span><span class="sxs-lookup"><span data-stu-id="81da8-507">After an unspecified threshold, more threads will decrease performance rather than increase it.</span></span>
* <span data-ttu-id="81da8-508">Överväg att buffring på storleken och tid som ett sätt att implementera batchbearbetning för flera scenarier.</span><span class="sxs-lookup"><span data-stu-id="81da8-508">Consider buffering on size and time as a way of implementing batching for more scenarios.</span></span>

## <a name="next-steps"></a><span data-ttu-id="81da8-509">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="81da8-509">Next steps</span></span>
<span data-ttu-id="81da8-510">Den här artikeln fokuserar på hur databasdesign och kodning tekniker relaterade till batchbearbetning kan förbättra ditt programprestanda och skalbarhet.</span><span class="sxs-lookup"><span data-stu-id="81da8-510">This article focused on how database design and coding techniques related to batching can improve your application performance and scalability.</span></span> <span data-ttu-id="81da8-511">Men det är bara en faktor i din övergripande säkerhetsstrategi.</span><span class="sxs-lookup"><span data-stu-id="81da8-511">But this is just one factor in your overall strategy.</span></span> <span data-ttu-id="81da8-512">Fler sätt att förbättra prestanda och skalbarhet finns [Azure SQL Database-prestandaråd för enskilda databaser](sql-database-performance-guidance.md) och [pris- och prestandaöverväganden för en elastisk pool](sql-database-elastic-pool-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="81da8-512">For more ways to improve performance and scalability, see [Azure SQL Database performance guidance for single databases](sql-database-performance-guidance.md) and [Price and performance considerations for an elastic pool](sql-database-elastic-pool-guidance.md).</span></span>

