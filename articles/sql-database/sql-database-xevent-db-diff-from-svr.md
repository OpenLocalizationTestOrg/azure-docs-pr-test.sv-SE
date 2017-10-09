---
title: "aaaExtended händelser i SQL-databas | Microsoft Docs"
description: "Beskriver utökade händelser (XEvents) i Azure SQL-databas och hur händelsesessioner skiljer sig från händelsesessioner i Microsoft SQL Server."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 3b28cf15-f820-4b3c-8310-908d6d5b9d0c
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 8c966a84412aa561c92b16e5c6902102483eb1bf
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="extended-events-in-sql-database"></a><span data-ttu-id="db713-103">Utökade händelser i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="db713-103">Extended events in SQL Database</span></span>
[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="db713-104">Det här avsnittet beskrivs hur hello implementering av utökade händelser i Azure SQL Database är något annorlunda jämfört med tooextended händelser i Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db713-104">This topic explains how hello implementation of extended events in Azure SQL Database is slightly different compared tooextended events in Microsoft SQL Server.</span></span>

- <span data-ttu-id="db713-105">SQL Database V12 fått hello utökade händelser funktion i hello andra hälften av kalender 2015.</span><span class="sxs-lookup"><span data-stu-id="db713-105">SQL Database V12 gained hello extended events feature in hello second half of calendar 2015.</span></span>
- <span data-ttu-id="db713-106">SQL Server har haft utökade händelser sedan 2008.</span><span class="sxs-lookup"><span data-stu-id="db713-106">SQL Server has had extended events since 2008.</span></span>
- <span data-ttu-id="db713-107">hello funktionsuppsättningen för utökade händelser på SQL-databas är en robust delmängd av hello funktioner på SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db713-107">hello feature set of extended events on SQL Database is a robust subset of hello features on SQL Server.</span></span>

<span data-ttu-id="db713-108">*XEvents* är en informell smeknamn som används ibland för ”utökade händelser” i bloggar och andra informell platser.</span><span class="sxs-lookup"><span data-stu-id="db713-108">*XEvents* is an informal nickname that is sometimes used for 'extended events' in blogs and other informal locations.</span></span>

<span data-ttu-id="db713-109">Mer information om utökade händelser för Azure SQL Database och Microsoft SQL Server finns på:</span><span class="sxs-lookup"><span data-stu-id="db713-109">Additional information about extended events, for Azure SQL Database and Microsoft SQL Server, is available at:</span></span>

- [<span data-ttu-id="db713-110">Snabbstart: Utökade händelser i SQL Server</span><span class="sxs-lookup"><span data-stu-id="db713-110">Quick Start: Extended events in SQL Server</span></span>](http://msdn.microsoft.com/library/mt733217.aspx)
- [<span data-ttu-id="db713-111">Utökade händelser</span><span class="sxs-lookup"><span data-stu-id="db713-111">Extended Events</span></span>](http://msdn.microsoft.com/library/bb630282.aspx)

## <a name="prerequisites"></a><span data-ttu-id="db713-112">Krav</span><span class="sxs-lookup"><span data-stu-id="db713-112">Prerequisites</span></span>

<span data-ttu-id="db713-113">Det här avsnittet förutsätter att du redan har viss erfarenhet av:</span><span class="sxs-lookup"><span data-stu-id="db713-113">This topic assumes you already have some knowledge of:</span></span>

- <span data-ttu-id="db713-114">[Azure SQL Database-tjänsten](https://azure.microsoft.com/services/sql-database/).</span><span class="sxs-lookup"><span data-stu-id="db713-114">[Azure SQL Database service](https://azure.microsoft.com/services/sql-database/).</span></span>
- <span data-ttu-id="db713-115">[Utökade händelser](http://msdn.microsoft.com/library/bb630282.aspx) i Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="db713-115">[Extended events](http://msdn.microsoft.com/library/bb630282.aspx) in Microsoft SQL Server.</span></span>

- <span data-ttu-id="db713-116">hello huvuddelen av vår dokumentation om utökade händelser gäller tooboth SQL Server och SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="db713-116">hello bulk of our documentation about extended events applies tooboth SQL Server and SQL Database.</span></span>

<span data-ttu-id="db713-117">Tidigare exponering toohello följande objekt är användbart när du väljer hello händelsefilen som hello [mål](#AzureXEventsTargets):</span><span class="sxs-lookup"><span data-stu-id="db713-117">Prior exposure toohello following items is helpful when choosing hello Event File as hello [target](#AzureXEventsTargets):</span></span>

- [<span data-ttu-id="db713-118">Azure Storage-tjänst</span><span class="sxs-lookup"><span data-stu-id="db713-118">Azure Storage service</span></span>](https://azure.microsoft.com/services/storage/)


- <span data-ttu-id="db713-119">PowerShell</span><span class="sxs-lookup"><span data-stu-id="db713-119">PowerShell</span></span>
    - <span data-ttu-id="db713-120">[Använda Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md) -innehåller omfattande information om PowerShell och hello Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="db713-120">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and hello Azure Storage service.</span></span>

## <a name="code-samples"></a><span data-ttu-id="db713-121">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="db713-121">Code samples</span></span>

<span data-ttu-id="db713-122">Relaterade avsnitt innehåller två kodexempel:</span><span class="sxs-lookup"><span data-stu-id="db713-122">Related topics provide two code samples:</span></span>


- [<span data-ttu-id="db713-123">Ring buffert mål koden för utökade händelser i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="db713-123">Ring Buffer target code for extended events in SQL Database</span></span>](sql-database-xevent-code-ring-buffer.md)
    - <span data-ttu-id="db713-124">Kort enkelt Transact-SQL-skript.</span><span class="sxs-lookup"><span data-stu-id="db713-124">Short simple Transact-SQL script.</span></span>
    - <span data-ttu-id="db713-125">Vi betona i hello kod exempel avsnittet att du ska släpper dess resurser genom att köra alter släpp när du är klar med ett ringbufferten `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` instruktionen.</span><span class="sxs-lookup"><span data-stu-id="db713-125">We emphasize in hello code sample topic that, when you are done with a Ring Buffer target, you should release its resources by executing an alter-drop `ALTER EVENT SESSION ... ON DATABASE DROP TARGET ...;` statement.</span></span> <span data-ttu-id="db713-126">Du kan senare lägga till en annan instans av ringbufferten av `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span><span class="sxs-lookup"><span data-stu-id="db713-126">Later you can add another instance of Ring Buffer by `ALTER EVENT SESSION ... ON DATABASE ADD TARGET ...`.</span></span>


- [<span data-ttu-id="db713-127">Händelsekod filen mål för utökade händelser i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="db713-127">Event File target code for extended events in SQL Database</span></span>](sql-database-xevent-code-event-file.md)
    - <span data-ttu-id="db713-128">Fas 1 är PowerShell toocreate en Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="db713-128">Phase 1 is PowerShell toocreate an Azure Storage container.</span></span>
    - <span data-ttu-id="db713-129">Fas 2 är Transact-SQL som använder hello Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="db713-129">Phase 2 is Transact-SQL that uses hello Azure Storage container.</span></span>

## <a name="transact-sql-differences"></a><span data-ttu-id="db713-130">Transact-SQL-skillnader</span><span class="sxs-lookup"><span data-stu-id="db713-130">Transact-SQL differences</span></span>


- <span data-ttu-id="db713-131">När du kör hello [Skapa händelse SESSION](http://msdn.microsoft.com/library/bb677289.aspx) kommandot på SQL Server du använder hello **på servern** satsen.</span><span class="sxs-lookup"><span data-stu-id="db713-131">When you execute hello [CREATE EVENT SESSION](http://msdn.microsoft.com/library/bb677289.aspx) command on SQL Server, you use hello **ON SERVER** clause.</span></span> <span data-ttu-id="db713-132">Men på SQL-databas som du använder hello **på databasen** satsen i stället.</span><span class="sxs-lookup"><span data-stu-id="db713-132">But on SQL Database you use hello **ON DATABASE** clause instead.</span></span>


- <span data-ttu-id="db713-133">hello **på databasen** satsen gäller även toohello [ALTER händelse SESSION](http://msdn.microsoft.com/library/bb630368.aspx) och [släppa händelse SESSION](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL-kommandon.</span><span class="sxs-lookup"><span data-stu-id="db713-133">hello **ON DATABASE** clause also applies toohello [ALTER EVENT SESSION](http://msdn.microsoft.com/library/bb630368.aspx) and [DROP EVENT SESSION](http://msdn.microsoft.com/library/bb630257.aspx) Transact-SQL commands.</span></span>


- <span data-ttu-id="db713-134">Ett bra tips är tooinclude hello händelse session möjlighet att **STARTUP_STATE = ON** i din **Skapa händelse SESSION** eller **ALTER händelse SESSION** instruktioner.</span><span class="sxs-lookup"><span data-stu-id="db713-134">A best practice is tooinclude hello event session option of **STARTUP_STATE = ON** in your **CREATE EVENT SESSION**  or **ALTER EVENT SESSION** statements.</span></span>
    - <span data-ttu-id="db713-135">Hej **= ON** värdet stöder en automatisk omstart efter en Omkonfigurering av hello logiska databasen på grund av tooa växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="db713-135">hello **= ON** value supports an automatic restart after a reconfiguration of hello logical database due tooa failover.</span></span>

## <a name="new-catalog-views"></a><span data-ttu-id="db713-136">Nya katalogvyerna</span><span class="sxs-lookup"><span data-stu-id="db713-136">New catalog views</span></span>

<span data-ttu-id="db713-137">hello utökade händelser stöds av flera [katalogen vyer](http://msdn.microsoft.com/library/ms174365.aspx).</span><span class="sxs-lookup"><span data-stu-id="db713-137">hello extended events feature is supported by several [catalog views](http://msdn.microsoft.com/library/ms174365.aspx).</span></span> <span data-ttu-id="db713-138">Katalogvyer berättar om *metadata eller definitioner* användarskapade händelse sessioner i hello aktuella databasen.</span><span class="sxs-lookup"><span data-stu-id="db713-138">Catalog views tell you about *metadata or definitions* of user-created event sessions in hello current database.</span></span> <span data-ttu-id="db713-139">hello vyer returnerar inte information om instanser av aktiva händelsesessioner.</span><span class="sxs-lookup"><span data-stu-id="db713-139">hello views do not return information about instances of active event sessions.</span></span>

| <span data-ttu-id="db713-140">Namnet på</span><span class="sxs-lookup"><span data-stu-id="db713-140">Name of</span></span><br/><span data-ttu-id="db713-141">katalogvyn</span><span class="sxs-lookup"><span data-stu-id="db713-141">catalog view</span></span> | <span data-ttu-id="db713-142">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="db713-142">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="db713-143">**sys.database_event_session_actions**</span><span class="sxs-lookup"><span data-stu-id="db713-143">**sys.database_event_session_actions**</span></span> |<span data-ttu-id="db713-144">Returnerar en rad för varje åtgärd för varje händelse en händelse-session.</span><span class="sxs-lookup"><span data-stu-id="db713-144">Returns a row for each action on each event of an event session.</span></span> |
| <span data-ttu-id="db713-145">**sys.database_event_session_events**</span><span class="sxs-lookup"><span data-stu-id="db713-145">**sys.database_event_session_events**</span></span> |<span data-ttu-id="db713-146">Returnerar en rad för varje händelse i en händelsesession.</span><span class="sxs-lookup"><span data-stu-id="db713-146">Returns a row for each event in an event session.</span></span> |
| <span data-ttu-id="db713-147">**sys.database_event_session_fields**</span><span class="sxs-lookup"><span data-stu-id="db713-147">**sys.database_event_session_fields**</span></span> |<span data-ttu-id="db713-148">Returnerar en rad för varje anpassa kan kolumn som uttryckligen har ställts in på händelser och mål.</span><span class="sxs-lookup"><span data-stu-id="db713-148">Returns a row for each customize-able column that was explicitly set on events and targets.</span></span> |
| <span data-ttu-id="db713-149">**sys.database_event_session_targets**</span><span class="sxs-lookup"><span data-stu-id="db713-149">**sys.database_event_session_targets**</span></span> |<span data-ttu-id="db713-150">Returnerar en rad för varje händelsemål för en händelsesessionen.</span><span class="sxs-lookup"><span data-stu-id="db713-150">Returns a row for each event target for an event session.</span></span> |
| <span data-ttu-id="db713-151">**sys.database_event_sessions**</span><span class="sxs-lookup"><span data-stu-id="db713-151">**sys.database_event_sessions**</span></span> |<span data-ttu-id="db713-152">Returnerar en rad för varje händelse session i hello SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="db713-152">Returns a row for each event session in hello SQL Database database.</span></span> |

<span data-ttu-id="db713-153">I Microsoft SQL Server har liknande katalogvyer namn som innehåller *.server\_*  i stället för *.database\_*.</span><span class="sxs-lookup"><span data-stu-id="db713-153">In Microsoft SQL Server, similar catalog views have names that include *.server\_* instead of *.database\_*.</span></span> <span data-ttu-id="db713-154">hello namnmönstret är som **sys.server_event_%**.</span><span class="sxs-lookup"><span data-stu-id="db713-154">hello name pattern is like **sys.server_event_%**.</span></span>

## <a name="new-dynamic-management-views-dmvshttpmsdnmicrosoftcomlibraryms188754aspx"></a><span data-ttu-id="db713-155">Nya dynamiska hanteringsvyer [(av DMV: er)](http://msdn.microsoft.com/library/ms188754.aspx)</span><span class="sxs-lookup"><span data-stu-id="db713-155">New dynamic management views [(DMVs)](http://msdn.microsoft.com/library/ms188754.aspx)</span></span>

<span data-ttu-id="db713-156">Azure SQL Database har [dynamiska hanteringsvyer (av DMV: er)](http://msdn.microsoft.com/library/bb677293.aspx) som stöd för utökade händelser.</span><span class="sxs-lookup"><span data-stu-id="db713-156">Azure SQL Database has [dynamic management views (DMVs)](http://msdn.microsoft.com/library/bb677293.aspx) that support extended events.</span></span> <span data-ttu-id="db713-157">Av DMV: er berättar om *active* händelsesessioner.</span><span class="sxs-lookup"><span data-stu-id="db713-157">DMVs tell you about *active* event sessions.</span></span>

| <span data-ttu-id="db713-158">Namnet på DMV</span><span class="sxs-lookup"><span data-stu-id="db713-158">Name of DMV</span></span> | <span data-ttu-id="db713-159">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="db713-159">Description</span></span> |
|:--- |:--- |
| <span data-ttu-id="db713-160">**sys.dm_xe_database_session_event_actions**</span><span class="sxs-lookup"><span data-stu-id="db713-160">**sys.dm_xe_database_session_event_actions**</span></span> |<span data-ttu-id="db713-161">Returnerar information om händelsen session åtgärder.</span><span class="sxs-lookup"><span data-stu-id="db713-161">Returns information about event session actions.</span></span> |
| <span data-ttu-id="db713-162">**sys.dm_xe_database_session_events**</span><span class="sxs-lookup"><span data-stu-id="db713-162">**sys.dm_xe_database_session_events**</span></span> |<span data-ttu-id="db713-163">Returnerar information om Sessionshändelser.</span><span class="sxs-lookup"><span data-stu-id="db713-163">Returns information about session events.</span></span> |
| <span data-ttu-id="db713-164">**sys.dm_xe_database_session_object_columns**</span><span class="sxs-lookup"><span data-stu-id="db713-164">**sys.dm_xe_database_session_object_columns**</span></span> |<span data-ttu-id="db713-165">Visar hello konfigurationsvärden för objekt som är bundna tooa session.</span><span class="sxs-lookup"><span data-stu-id="db713-165">Shows hello configuration values for objects that are bound tooa session.</span></span> |
| <span data-ttu-id="db713-166">**sys.dm_xe_database_session_targets**</span><span class="sxs-lookup"><span data-stu-id="db713-166">**sys.dm_xe_database_session_targets**</span></span> |<span data-ttu-id="db713-167">Returnerar information om sessionen mål.</span><span class="sxs-lookup"><span data-stu-id="db713-167">Returns information about session targets.</span></span> |
| <span data-ttu-id="db713-168">**sys.dm_xe_database_sessions**</span><span class="sxs-lookup"><span data-stu-id="db713-168">**sys.dm_xe_database_sessions**</span></span> |<span data-ttu-id="db713-169">Returnerar en rad för varje händelsesession som är begränsade toohello aktuella databasen.</span><span class="sxs-lookup"><span data-stu-id="db713-169">Returns a row for each event session that is scoped toohello current database.</span></span> |

<span data-ttu-id="db713-170">I Microsoft SQL Server, kallas liknande katalogvyer utan hello  *\_databasen* som del av hello namn:</span><span class="sxs-lookup"><span data-stu-id="db713-170">In Microsoft SQL Server, similar catalog views are named without hello *\_database* portion of hello name, such as:</span></span>

- <span data-ttu-id="db713-171">**sys.dm_xe_sessions**, i stället för namn</span><span class="sxs-lookup"><span data-stu-id="db713-171">**sys.dm_xe_sessions**, instead of name</span></span><br/><span data-ttu-id="db713-172">**sys.dm_xe_database_sessions**.</span><span class="sxs-lookup"><span data-stu-id="db713-172">**sys.dm_xe_database_sessions**.</span></span>

### <a name="dmvs-common-tooboth"></a><span data-ttu-id="db713-173">Av DMV: er vanliga tooboth</span><span class="sxs-lookup"><span data-stu-id="db713-173">DMVs common tooboth</span></span>
<span data-ttu-id="db713-174">För utökade händelser finns ytterligare av DMV: er som är vanliga tooboth Azure SQL Database och Microsoft SQL Server:</span><span class="sxs-lookup"><span data-stu-id="db713-174">For extended events there are additional DMVs that are common tooboth Azure SQL Database and Microsoft SQL Server:</span></span>

- <span data-ttu-id="db713-175">**sys.dm_xe_map_values**</span><span class="sxs-lookup"><span data-stu-id="db713-175">**sys.dm_xe_map_values**</span></span>
- <span data-ttu-id="db713-176">**sys.dm_xe_object_columns**</span><span class="sxs-lookup"><span data-stu-id="db713-176">**sys.dm_xe_object_columns**</span></span>
- <span data-ttu-id="db713-177">**sys.dm_xe_objects**</span><span class="sxs-lookup"><span data-stu-id="db713-177">**sys.dm_xe_objects**</span></span>
- <span data-ttu-id="db713-178">**sys.dm_xe_packages**</span><span class="sxs-lookup"><span data-stu-id="db713-178">**sys.dm_xe_packages**</span></span>

 <a name="sqlfindseventsactionstargets" id="sqlfindseventsactionstargets"></a>

## <a name="find-hello-available-extended-events-actions-and-targets"></a><span data-ttu-id="db713-179">Hitta hello tillgängliga utökade händelser och åtgärder som mål</span><span class="sxs-lookup"><span data-stu-id="db713-179">Find hello available extended events, actions, and targets</span></span>

<span data-ttu-id="db713-180">Du kan köra en enkel SQL **Välj** tooobtain en lista över tillgängliga hello-händelser och åtgärder som mål.</span><span class="sxs-lookup"><span data-stu-id="db713-180">You can run a simple SQL **SELECT** tooobtain a list of hello available events, actions, and target.</span></span>

```sql
SELECT
        o.object_type,
        p.name         AS [package_name],
        o.name         AS [db_object_name],
        o.description  AS [db_obj_description]
    FROM
                   sys.dm_xe_objects  AS o
        INNER JOIN sys.dm_xe_packages AS p  ON p.guid = o.package_guid
    WHERE
        o.object_type in
            (
            'action',  'event',  'target'
            )
    ORDER BY
        o.object_type,
        p.name,
        o.name;
```


<span data-ttu-id="db713-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span><span class="sxs-lookup"><span data-stu-id="db713-181"><a name="AzureXEventsTargets" id="AzureXEventsTargets"></a> &nbsp;</span></span>

## <a name="targets-for-your-sql-database-event-sessions"></a><span data-ttu-id="db713-182">Mål för händelsesessioner din SQL-databas</span><span class="sxs-lookup"><span data-stu-id="db713-182">Targets for your SQL Database event sessions</span></span>

<span data-ttu-id="db713-183">Här följer mål som kan hämta resultat från event-sessioner på SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="db713-183">Here are targets that can capture results from your event sessions on SQL Database:</span></span>

- <span data-ttu-id="db713-184">[Ring Buffer mål](http://msdn.microsoft.com/library/ff878182.aspx) -kort innehåller händelsedata i minnet.</span><span class="sxs-lookup"><span data-stu-id="db713-184">[Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx) - Briefly holds event data in memory.</span></span>
- <span data-ttu-id="db713-185">[Händelsen räknaren mål](http://msdn.microsoft.com/library/ff878025.aspx) -räknar alla händelser som inträffar under en session för utökade händelser.</span><span class="sxs-lookup"><span data-stu-id="db713-185">[Event Counter target](http://msdn.microsoft.com/library/ff878025.aspx) - Counts all events that occur during an extended events session.</span></span>
- <span data-ttu-id="db713-186">[Målfil för händelsen](http://msdn.microsoft.com/library/ff878115.aspx) -skrivningar fullständig buffertar tooan Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="db713-186">[Event File target](http://msdn.microsoft.com/library/ff878115.aspx) - Writes complete buffers tooan Azure Storage container.</span></span>

<span data-ttu-id="db713-187">Hej [ETW Event Tracing for Windows ()](http://msdn.microsoft.com/library/ms751538.aspx) API är inte tillgängligt för utökade händelser på SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="db713-187">hello [Event Tracing for Windows (ETW)](http://msdn.microsoft.com/library/ms751538.aspx) API is not available for extended events on SQL Database.</span></span>

## <a name="restrictions"></a><span data-ttu-id="db713-188">Begränsningar</span><span class="sxs-lookup"><span data-stu-id="db713-188">Restrictions</span></span>

<span data-ttu-id="db713-189">Det finns ett par av säkerhetsrelaterade skillnaderna befitting hello molnmiljö av SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="db713-189">There are a couple of security-related differences befitting hello cloud environment of SQL Database:</span></span>

- <span data-ttu-id="db713-190">Utökade händelser baseras på hello enskild klientisolering modell.</span><span class="sxs-lookup"><span data-stu-id="db713-190">Extended events are founded on hello single-tenant isolation model.</span></span> <span data-ttu-id="db713-191">En händelsesession i en databas kan inte komma åt data eller händelser från en annan databas.</span><span class="sxs-lookup"><span data-stu-id="db713-191">An event session in one database cannot access data or events from another database.</span></span>
- <span data-ttu-id="db713-192">Du kan inte utfärda en **Skapa händelse SESSION** instruktionen i hello kontext för hello **master** databas.</span><span class="sxs-lookup"><span data-stu-id="db713-192">You cannot issue a **CREATE EVENT SESSION** statement in hello context of hello **master** database.</span></span>

## <a name="permission-model"></a><span data-ttu-id="db713-193">Behörighetsmodellen</span><span class="sxs-lookup"><span data-stu-id="db713-193">Permission model</span></span>

<span data-ttu-id="db713-194">Du måste ha **kontrollen** behörighet på hello databasen tooissue en **Skapa händelse SESSION** instruktionen.</span><span class="sxs-lookup"><span data-stu-id="db713-194">You must have **Control** permission on hello database tooissue a **CREATE EVENT SESSION** statement.</span></span> <span data-ttu-id="db713-195">hello databasägaren (dbo) har **kontrollen** behörighet.</span><span class="sxs-lookup"><span data-stu-id="db713-195">hello database owner (dbo) has **Control** permission.</span></span>

### <a name="storage-container-authorizations"></a><span data-ttu-id="db713-196">Tillstånd för Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="db713-196">Storage container authorizations</span></span>

<span data-ttu-id="db713-197">hello SAS-token som du skapar för Azure Storage-behållare måste ange **rwl** hello behörigheter.</span><span class="sxs-lookup"><span data-stu-id="db713-197">hello SAS token you generate for your Azure Storage container must specify **rwl** for hello permissions.</span></span> <span data-ttu-id="db713-198">Hej **rwl** värdet ger hello följande behörigheter:</span><span class="sxs-lookup"><span data-stu-id="db713-198">hello **rwl** value provides hello following permissions:</span></span>

- <span data-ttu-id="db713-199">Läsa</span><span class="sxs-lookup"><span data-stu-id="db713-199">Read</span></span>
- <span data-ttu-id="db713-200">Skriva</span><span class="sxs-lookup"><span data-stu-id="db713-200">Write</span></span>
- <span data-ttu-id="db713-201">Visa lista</span><span class="sxs-lookup"><span data-stu-id="db713-201">List</span></span>

## <a name="performance-considerations"></a><span data-ttu-id="db713-202">Saker att tänka på gällande prestanda</span><span class="sxs-lookup"><span data-stu-id="db713-202">Performance considerations</span></span>

<span data-ttu-id="db713-203">Det finns scenarier där intensiva användningen av utökade händelser kan samlas mer aktiva minne än vad som är felfri för hello övergripande system.</span><span class="sxs-lookup"><span data-stu-id="db713-203">There are scenarios where intensive use of extended events can accumulate more active memory than is healthy for hello overall system.</span></span> <span data-ttu-id="db713-204">Därför hello Azure SQL Database system dynamiskt anger och justerar gränserna för hello minnesmängden active som kan visas med en händelsesessionen.</span><span class="sxs-lookup"><span data-stu-id="db713-204">Therefore hello Azure SQL Database system dynamically sets and adjusts limits on hello amount of active memory that can be accumulated by an event session.</span></span> <span data-ttu-id="db713-205">Många faktorer gå in hello dynamisk beräkning.</span><span class="sxs-lookup"><span data-stu-id="db713-205">Many factors go into hello dynamic calculation.</span></span>

<span data-ttu-id="db713-206">Om du får ett felmeddelande om ett maximalt minne har tvingats fram är vissa åtgärder du kan vidta:</span><span class="sxs-lookup"><span data-stu-id="db713-206">If you receive an error message that says a memory maximum was enforced, some corrective actions you can take are:</span></span>

- <span data-ttu-id="db713-207">Köra färre samtidiga händelsesessioner.</span><span class="sxs-lookup"><span data-stu-id="db713-207">Run fewer concurrent event sessions.</span></span>
- <span data-ttu-id="db713-208">Via din **skapa** och **ALTER** uttryck för händelsesessioner, minska hello mängden minne som du anger på hello **MAX\_minne** satsen.</span><span class="sxs-lookup"><span data-stu-id="db713-208">Through your **CREATE** and **ALTER** statements for event sessions, reduce hello amount of memory you specify on hello **MAX\_MEMORY** clause.</span></span>

### <a name="network-latency"></a><span data-ttu-id="db713-209">Svarstid för nätverk</span><span class="sxs-lookup"><span data-stu-id="db713-209">Network latency</span></span>

<span data-ttu-id="db713-210">Hej **händelsefilen** mål kan uppleva Nätverksfördröjningen eller fel vid spara data tooAzure Storage-blobbar.</span><span class="sxs-lookup"><span data-stu-id="db713-210">hello **Event File** target might experience network latency or failures while persisting data tooAzure Storage blobs.</span></span> <span data-ttu-id="db713-211">Andra händelser i SQL-databas kan fördröjas väntan på hello nätverket kommunikation toocomplete.</span><span class="sxs-lookup"><span data-stu-id="db713-211">Other events in SQL Database might be delayed while they wait for hello network communication toocomplete.</span></span> <span data-ttu-id="db713-212">Den här fördröjningen kan sakta din arbetsbelastning.</span><span class="sxs-lookup"><span data-stu-id="db713-212">This delay can slow your workload.</span></span>

- <span data-ttu-id="db713-213">toomitigate prestanda risk, Undvik att ange hello **EVENT_RETENTION_MODE** alternativet för**NO_EVENT_LOSS** i din session Händelsedefinitioner.</span><span class="sxs-lookup"><span data-stu-id="db713-213">toomitigate this performance risk, avoid setting hello **EVENT_RETENTION_MODE** option too**NO_EVENT_LOSS** in your event session definitions.</span></span>

## <a name="related-links"></a><span data-ttu-id="db713-214">Relaterade länkar</span><span class="sxs-lookup"><span data-stu-id="db713-214">Related links</span></span>

- <span data-ttu-id="db713-215">[Använda Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md).</span><span class="sxs-lookup"><span data-stu-id="db713-215">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md).</span></span>
- [<span data-ttu-id="db713-216">Azure Storage-Cmdlets</span><span class="sxs-lookup"><span data-stu-id="db713-216">Azure Storage Cmdlets</span></span>](http://msdn.microsoft.com/library/dn806401.aspx)
- <span data-ttu-id="db713-217">[Använda Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md) -innehåller omfattande information om PowerShell och hello Azure Storage-tjänst.</span><span class="sxs-lookup"><span data-stu-id="db713-217">[Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) - Provides comprehensive information about PowerShell and hello Azure Storage service.</span></span>
- [<span data-ttu-id="db713-218">Hur toouse Blob storage från .NET</span><span class="sxs-lookup"><span data-stu-id="db713-218">How toouse Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
- [<span data-ttu-id="db713-219">Skapa autentiseringsuppgift (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="db713-219">CREATE CREDENTIAL (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/ms189522.aspx)
- [<span data-ttu-id="db713-220">Skapa HÄNDELSESESSIONEN (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="db713-220">CREATE EVENT SESSION (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/bb677289.aspx)
- [<span data-ttu-id="db713-221">Jonathan Kehayias blogginlägg om utökade händelser i Microsoft SQL Server</span><span class="sxs-lookup"><span data-stu-id="db713-221">Jonathan Kehayias' blog posts about extended events in Microsoft SQL Server</span></span>](http://www.sqlskills.com/blogs/jonathan/category/extended-events/)


- <span data-ttu-id="db713-222">hello Azure *tjänstuppdateringar* webbsidan, minskas med parametern tooAzure SQL-databas:</span><span class="sxs-lookup"><span data-stu-id="db713-222">hello Azure *Service Updates* webpage, narrowed by parameter tooAzure SQL Database:</span></span>
    - [<span data-ttu-id="db713-223">https://Azure.microsoft.com/Updates/?Service=SQL-Database</span><span class="sxs-lookup"><span data-stu-id="db713-223">https://azure.microsoft.com/updates/?service=sql-database</span></span>](https://azure.microsoft.com/updates/?service=sql-database)


<span data-ttu-id="db713-224">Andra exempel avsnitt i koden för utökade händelser finns på följande länkar hello.</span><span class="sxs-lookup"><span data-stu-id="db713-224">Other code sample topics for extended events are available at hello following links.</span></span> <span data-ttu-id="db713-225">Du måste dock regelbundet kontrollera alla exempel toosee om hello exempel riktar sig till Microsoft SQL Server jämfört med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="db713-225">However, you must routinely check any sample toosee whether hello sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="db713-226">Sedan kan du bestämma om mindre ändringar är nödvändiga toorun hello exempel.</span><span class="sxs-lookup"><span data-stu-id="db713-226">Then you can decide whether minor changes are needed toorun hello sample.</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
