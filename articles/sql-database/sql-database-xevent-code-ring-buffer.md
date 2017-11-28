---
title: "XEvent-ringbufferten koden för SQL-databas | Microsoft Docs"
description: "Ger en Transact-SQL-kodexempel som görs enkelt och snabbt med hjälp av ringbufferten mål, i Azure SQL Database."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: 2510fb3f-c8f2-437a-8f49-9d5f6c96e75b
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/03/2017
ms.author: genemi
ms.openlocfilehash: 6fbefe151901ac3b15d93712422878fc4d6206f1
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="7c509-103">Ring buffert mål koden för utökade händelser i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="7c509-103">Ring Buffer target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="7c509-104">Du vill använda en fullständig kodexempel för enklaste snabbt sätt att avbilda och rapportera information om en utökad händelse under ett test.</span><span class="sxs-lookup"><span data-stu-id="7c509-104">You want a complete code sample for the easiest quick way to capture and report information for an extended event during a test.</span></span> <span data-ttu-id="7c509-105">Det enklaste målet för utökade händelsedata är den [ringbufferten mål](http://msdn.microsoft.com/library/ff878182.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c509-105">The easiest target for extended event data is the [Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx).</span></span>

<span data-ttu-id="7c509-106">Det här avsnittet presenteras en Transact-SQL-kodexempel som:</span><span class="sxs-lookup"><span data-stu-id="7c509-106">This topic presents a Transact-SQL code sample that:</span></span>

1. <span data-ttu-id="7c509-107">Skapar en tabell med data för att visa med.</span><span class="sxs-lookup"><span data-stu-id="7c509-107">Creates a table with data to demonstrate with.</span></span>
2. <span data-ttu-id="7c509-108">Skapar en session för en befintlig utökad händelse, nämligen **sqlserver.sql_statement_starting**.</span><span class="sxs-lookup"><span data-stu-id="7c509-108">Creates a session for an existing extended event, namely **sqlserver.sql_statement_starting**.</span></span>
   
   * <span data-ttu-id="7c509-109">Händelsen är begränsad till SQL-uttryck som innehåller en viss uppdatering sträng: **instruktionen som '% uppdatering tabEmployee %'**.</span><span class="sxs-lookup"><span data-stu-id="7c509-109">The event is limited to SQL statements that contain a particular Update string: **statement LIKE '%UPDATE tabEmployee%'**.</span></span>
   * <span data-ttu-id="7c509-110">Väljer att skicka utdata för händelsen till ett mål av typen ringbufferten, nämligen **package0.ring_buffer**.</span><span class="sxs-lookup"><span data-stu-id="7c509-110">Chooses to send the output of the event to a target of type Ring Buffer, namely  **package0.ring_buffer**.</span></span>
3. <span data-ttu-id="7c509-111">Startar händelsesessionen.</span><span class="sxs-lookup"><span data-stu-id="7c509-111">Starts the event session.</span></span>
4. <span data-ttu-id="7c509-112">Utfärdar ett par enkla UPDATE SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="7c509-112">Issues a couple of simple SQL UPDATE statements.</span></span>
5. <span data-ttu-id="7c509-113">Utfärdar en SQL SELECT-instruktion för att hämta utdata för händelse från ringbufferten.</span><span class="sxs-lookup"><span data-stu-id="7c509-113">Issues a SQL SELECT statement to retrieve event output from the Ring Buffer.</span></span>
   
   * <span data-ttu-id="7c509-114">**sys.dm_xe_database_session_targets** och andra dynamiska hanteringsvyer (av DMV: er) som är anslutna.</span><span class="sxs-lookup"><span data-stu-id="7c509-114">**sys.dm_xe_database_session_targets** and other dynamic management views (DMVs) are joined.</span></span>
6. <span data-ttu-id="7c509-115">Stoppar händelsesessionen.</span><span class="sxs-lookup"><span data-stu-id="7c509-115">Stops the event session.</span></span>
7. <span data-ttu-id="7c509-116">Utelämnar ringbufferten mål, om du vill frigöra resurser.</span><span class="sxs-lookup"><span data-stu-id="7c509-116">Drops the Ring Buffer target, to release its resources.</span></span>
8. <span data-ttu-id="7c509-117">Utelämnar händelsesessionen och demo-tabellen.</span><span class="sxs-lookup"><span data-stu-id="7c509-117">Drops the event session and the demo table.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="7c509-118">Krav</span><span class="sxs-lookup"><span data-stu-id="7c509-118">Prerequisites</span></span>

* <span data-ttu-id="7c509-119">Ett Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7c509-119">An Azure account and subscription.</span></span> <span data-ttu-id="7c509-120">Registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="7c509-120">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="7c509-121">Alla databaser som du kan skapa en tabell i.</span><span class="sxs-lookup"><span data-stu-id="7c509-121">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="7c509-122">Alternativt kan du [skapa en **AdventureWorksLT** demonstrationsdatabas](sql-database-get-started.md) i minuter.</span><span class="sxs-lookup"><span data-stu-id="7c509-122">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="7c509-123">SQL Server Management Studio (ssms.exe), helst den senaste månatliga update-versionen.</span><span class="sxs-lookup"><span data-stu-id="7c509-123">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="7c509-124">Du kan hämta den senaste ssms.exe från:</span><span class="sxs-lookup"><span data-stu-id="7c509-124">You can download the latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="7c509-125">Avsnittet [Hämta SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="7c509-125">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="7c509-126">En direktlänk till nedladdningen.</span><span class="sxs-lookup"><span data-stu-id="7c509-126">A direct link to the download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a><span data-ttu-id="7c509-127">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="7c509-127">Code sample</span></span>

<span data-ttu-id="7c509-128">Med mycket mindre ändringar, kan du köra följande kodexempel i ringbufferten på Azure SQL Database eller Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c509-128">With very minor modification, the following Ring Buffer code sample can be run on either Azure SQL Database or Microsoft SQL Server.</span></span> <span data-ttu-id="7c509-129">Skillnaden är förekomsten av noden '_databas' i vissa dynamiska hanteringsvyer (av DMV: er), används i FROM-satsen i steg 5.</span><span class="sxs-lookup"><span data-stu-id="7c509-129">The difference is the presence of the node '_database' in the name of some dynamic management views (DMVs), used in the FROM clause in Step 5.</span></span> <span data-ttu-id="7c509-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="7c509-130">For example:</span></span>

* <span data-ttu-id="7c509-131">sys.dm_xe**_databas**_session_targets</span><span class="sxs-lookup"><span data-stu-id="7c509-131">sys.dm_xe**_database**_session_targets</span></span>
* <span data-ttu-id="7c509-132">sys.dm_xe_session_targets</span><span class="sxs-lookup"><span data-stu-id="7c509-132">sys.dm_xe_session_targets</span></span>

&nbsp;

```sql
GO
----  Transact-SQL.
---- Step set 1.

SET NOCOUNT ON;
GO


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'tabEmployee')
BEGIN
    DROP TABLE tabEmployee;
END
GO


CREATE TABLE tabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO tabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO

---- Step set 2.


IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'eventsession_gm_azuresqldb51')
BEGIN
    DROP EVENT SESSION eventsession_gm_azuresqldb51
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE '%UPDATE tabEmployee%'
            )
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
GO

---- Step set 3.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = START;
GO

---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
GO

---- Step set 5.


SELECT
    se.name                      AS [session-name],
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source,
    st.target_data,
    CAST(st.target_data AS XML)  AS [target_data_XML]
FROM
               sys.dm_xe_database_session_event_actions  AS ac

    INNER JOIN sys.dm_xe_database_session_events         AS ev  ON ev.event_name = ac.event_name
        AND CAST(ev.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_object_columns AS oc
         ON CAST(oc.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_session_targets        AS st
         ON CAST(st.event_session_address AS BINARY(8)) = CAST(ac.event_session_address AS BINARY(8))

    INNER JOIN sys.dm_xe_database_sessions               AS se
         ON CAST(ac.event_session_address AS BINARY(8)) = CAST(se.address AS BINARY(8))
WHERE
        oc.column_name = 'occurrence_number'
    AND
        se.name        = 'eventsession_gm_azuresqldb51'
    AND
        ac.action_name = 'sql_text'
ORDER BY
    se.name,
    ev.event_name,
    ac.action_name,
    st.target_name,
    se.session_source
;
GO

---- Step set 6.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    STATE = STOP;
GO

---- Step set 7.


ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO

---- Step set 8.


DROP EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE;
GO

DROP TABLE tabEmployee;
GO
```


&nbsp;

## <a name="ring-buffer-contents"></a><span data-ttu-id="7c509-133">Ring Buffer innehållet</span><span class="sxs-lookup"><span data-stu-id="7c509-133">Ring Buffer contents</span></span>

<span data-ttu-id="7c509-134">Vi använde ssms.exe för att köra kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="7c509-134">We used ssms.exe to run the code sample.</span></span>

<span data-ttu-id="7c509-135">Om du vill visa resultatet, vi har klickat på cellen under kolumnrubriken **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="7c509-135">To view the results, we clicked the cell under the column header **target_data_XML**.</span></span>

<span data-ttu-id="7c509-136">Sedan i resultatfönstret klickar vi på cellen under kolumnrubriken **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="7c509-136">Then in the results pane we clicked the cell under the column header **target_data_XML**.</span></span> <span data-ttu-id="7c509-137">Detta klickar du på Skapa en annan fil flik i ssms.exe där innehållet i resultatcellen visades som XML.</span><span class="sxs-lookup"><span data-stu-id="7c509-137">This click created another file tab in ssms.exe in which the content of the result cell was displayed, as XML.</span></span>

<span data-ttu-id="7c509-138">Utdata visas i följande blocket.</span><span class="sxs-lookup"><span data-stu-id="7c509-138">The output is shown in the following block.</span></span> <span data-ttu-id="7c509-139">Det ser ut långt, men det är bara två  **<event>**  element.</span><span class="sxs-lookup"><span data-stu-id="7c509-139">It looks long, but it is just two **<event>** elements.</span></span>

&nbsp;

```
<RingBufferTarget truncated="0" processingTime="0" totalEventsProcessed="2" eventCount="2" droppedCount="0" memoryUsed="1728">
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.317Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>7</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>184</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>328</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
  <event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T15:29:31.327Z">
    <data name="state">
      <type name="statement_starting_state" package="sqlserver" />
      <value>0</value>
      <text>Normal</text>
    </data>
    <data name="line_number">
      <type name="int32" package="package0" />
      <value>10</value>
    </data>
    <data name="offset">
      <type name="int32" package="package0" />
      <value>340</value>
    </data>
    <data name="offset_end">
      <type name="int32" package="package0" />
      <value>486</value>
    </data>
    <data name="statement">
      <type name="unicode_string" package="package0" />
      <value>UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015</value>
    </data>
    <action name="sql_text" package="sqlserver">
      <type name="unicode_string" package="package0" />
      <value>
---- Step set 4.


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM tabEmployee;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 102;

UPDATE tabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 1015;

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM tabEmployee;
</value>
    </action>
  </event>
</RingBufferTarget>
```


#### <a name="release-resources-held-by-your-ring-buffer"></a><span data-ttu-id="7c509-140">Frigöra resurser som innehas av Ring Buffer</span><span class="sxs-lookup"><span data-stu-id="7c509-140">Release resources held by your Ring Buffer</span></span>

<span data-ttu-id="7c509-141">När du är klar med Ring Buffer kan du ta bort den och släpper dess resurser som utfärdar en **ALTER** ut så här:</span><span class="sxs-lookup"><span data-stu-id="7c509-141">When you are done with your Ring Buffer, you can remove it and release its resources issuing an **ALTER** like the following:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


<span data-ttu-id="7c509-142">Definitionen för händelsesessionen uppdaterats, men inte ta bort.</span><span class="sxs-lookup"><span data-stu-id="7c509-142">The definition of your event session is updated, but not dropped.</span></span> <span data-ttu-id="7c509-143">Du kan senare lägga till en annan instans av ringbufferten händelsesessionen:</span><span class="sxs-lookup"><span data-stu-id="7c509-143">Later you can add another instance of the Ring Buffer to your event session:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a><span data-ttu-id="7c509-144">Mer information</span><span class="sxs-lookup"><span data-stu-id="7c509-144">More information</span></span>

<span data-ttu-id="7c509-145">Primär avsnittet för utökade händelser på Azure SQL Database är:</span><span class="sxs-lookup"><span data-stu-id="7c509-145">The primary topic for extended events on Azure SQL Database is:</span></span>

* <span data-ttu-id="7c509-146">[Utökad händelse överväganden i SQL-databas](sql-database-xevent-db-diff-from-svr.md), vilket står i kontrast vissa aspekter av utökade händelser som skiljer sig åt mellan Azure SQL Database jämfört med Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="7c509-146">[Extended event considerations in SQL Database](sql-database-xevent-db-diff-from-svr.md), which contrasts some aspects of extended events that differ between Azure SQL Database versus Microsoft SQL Server.</span></span>

<span data-ttu-id="7c509-147">Andra exempel avsnitt i koden för utökade händelser finns på följande länkar.</span><span class="sxs-lookup"><span data-stu-id="7c509-147">Other code sample topics for extended events are available at the following links.</span></span> <span data-ttu-id="7c509-148">Du måste regelbundet kontrollera varje prov för att se om exemplet riktar sig till Microsoft SQL Server jämfört med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="7c509-148">However, you must routinely check any sample to see whether the sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="7c509-149">Sedan kan du bestämma om mindre ändringar behövs för att köra exemplet.</span><span class="sxs-lookup"><span data-stu-id="7c509-149">Then you can decide whether minor changes are needed to run the sample.</span></span>

* <span data-ttu-id="7c509-150">Kodexempel för Azure SQL Database: [händelsefilen mål koden för utökade händelser i SQL-databas](sql-database-xevent-code-event-file.md)</span><span class="sxs-lookup"><span data-stu-id="7c509-150">Code sample for Azure SQL Database: [Event File target code for extended events in SQL Database](sql-database-xevent-code-event-file.md)</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find the Objects That Have the Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
