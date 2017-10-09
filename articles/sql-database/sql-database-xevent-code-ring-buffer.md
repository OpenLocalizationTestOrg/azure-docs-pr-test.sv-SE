---
title: "aaaXEvent ringbufferten koden för SQL-databas | Microsoft Docs"
description: "Ger en Transact-SQL-kodexempel som görs enkelt och snabbt med hjälp av hello ringbufferten mål i Azure SQL Database."
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
ms.openlocfilehash: 21df748d9999d6837d2b5bbe4a3f47fb351b4633
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="ring-buffer-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="64d9a-103">Ring buffert mål koden för utökade händelser i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="64d9a-103">Ring Buffer target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="64d9a-104">Du vill använda en fullständig kodexempel för hello enklaste snabbt toocapture och rapportera information om en utökad händelse under ett test.</span><span class="sxs-lookup"><span data-stu-id="64d9a-104">You want a complete code sample for hello easiest quick way toocapture and report information for an extended event during a test.</span></span> <span data-ttu-id="64d9a-105">hello enklaste målet för utökade händelsedata är hello [ringbufferten mål](http://msdn.microsoft.com/library/ff878182.aspx).</span><span class="sxs-lookup"><span data-stu-id="64d9a-105">hello easiest target for extended event data is hello [Ring Buffer target](http://msdn.microsoft.com/library/ff878182.aspx).</span></span>

<span data-ttu-id="64d9a-106">Det här avsnittet presenteras en Transact-SQL-kodexempel som:</span><span class="sxs-lookup"><span data-stu-id="64d9a-106">This topic presents a Transact-SQL code sample that:</span></span>

1. <span data-ttu-id="64d9a-107">Skapar en tabell med data toodemonstrate med.</span><span class="sxs-lookup"><span data-stu-id="64d9a-107">Creates a table with data toodemonstrate with.</span></span>
2. <span data-ttu-id="64d9a-108">Skapar en session för en befintlig utökad händelse, nämligen **sqlserver.sql_statement_starting**.</span><span class="sxs-lookup"><span data-stu-id="64d9a-108">Creates a session for an existing extended event, namely **sqlserver.sql_statement_starting**.</span></span>
   
   * <span data-ttu-id="64d9a-109">hello händelsen är begränsad tooSQL instruktioner som innehåller en viss uppdatering sträng: **instruktionen som '% uppdatering tabEmployee %'**.</span><span class="sxs-lookup"><span data-stu-id="64d9a-109">hello event is limited tooSQL statements that contain a particular Update string: **statement LIKE '%UPDATE tabEmployee%'**.</span></span>
   * <span data-ttu-id="64d9a-110">Väljer toosend hello utdata från hello händelse tooa mål av typen ringbufferten, nämligen **package0.ring_buffer**.</span><span class="sxs-lookup"><span data-stu-id="64d9a-110">Chooses toosend hello output of hello event tooa target of type Ring Buffer, namely  **package0.ring_buffer**.</span></span>
3. <span data-ttu-id="64d9a-111">Startar hello händelsesessionen.</span><span class="sxs-lookup"><span data-stu-id="64d9a-111">Starts hello event session.</span></span>
4. <span data-ttu-id="64d9a-112">Utfärdar ett par enkla UPDATE SQL-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="64d9a-112">Issues a couple of simple SQL UPDATE statements.</span></span>
5. <span data-ttu-id="64d9a-113">Utfärdar en SQL SELECT-instruktionen tooretrieve händelse utdata från hello ringbufferten.</span><span class="sxs-lookup"><span data-stu-id="64d9a-113">Issues a SQL SELECT statement tooretrieve event output from hello Ring Buffer.</span></span>
   
   * <span data-ttu-id="64d9a-114">**sys.dm_xe_database_session_targets** och andra dynamiska hanteringsvyer (av DMV: er) som är anslutna.</span><span class="sxs-lookup"><span data-stu-id="64d9a-114">**sys.dm_xe_database_session_targets** and other dynamic management views (DMVs) are joined.</span></span>
6. <span data-ttu-id="64d9a-115">Stoppar hello händelsesessionen.</span><span class="sxs-lookup"><span data-stu-id="64d9a-115">Stops hello event session.</span></span>
7. <span data-ttu-id="64d9a-116">Således hello ringbufferten mål, toorelease dess resurser.</span><span class="sxs-lookup"><span data-stu-id="64d9a-116">Drops hello Ring Buffer target, toorelease its resources.</span></span>
8. <span data-ttu-id="64d9a-117">Utelämnar hello händelsesessionen och hello demo tabell.</span><span class="sxs-lookup"><span data-stu-id="64d9a-117">Drops hello event session and hello demo table.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="64d9a-118">Krav</span><span class="sxs-lookup"><span data-stu-id="64d9a-118">Prerequisites</span></span>

* <span data-ttu-id="64d9a-119">Ett Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="64d9a-119">An Azure account and subscription.</span></span> <span data-ttu-id="64d9a-120">Registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="64d9a-120">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="64d9a-121">Alla databaser som du kan skapa en tabell i.</span><span class="sxs-lookup"><span data-stu-id="64d9a-121">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="64d9a-122">Alternativt kan du [skapa en **AdventureWorksLT** demonstrationsdatabas](sql-database-get-started.md) i minuter.</span><span class="sxs-lookup"><span data-stu-id="64d9a-122">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="64d9a-123">SQL Server Management Studio (ssms.exe), helst den senaste månatliga update-versionen.</span><span class="sxs-lookup"><span data-stu-id="64d9a-123">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="64d9a-124">Du kan hämta hello senaste ssms.exe från:</span><span class="sxs-lookup"><span data-stu-id="64d9a-124">You can download hello latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="64d9a-125">Avsnittet [Hämta SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="64d9a-125">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="64d9a-126">En direktlänk toohello hämtning.</span><span class="sxs-lookup"><span data-stu-id="64d9a-126">A direct link toohello download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)

## <a name="code-sample"></a><span data-ttu-id="64d9a-127">Kodexempel</span><span class="sxs-lookup"><span data-stu-id="64d9a-127">Code sample</span></span>

<span data-ttu-id="64d9a-128">Med mycket mindre ändringar, kan hello följande kodexempel i ringbufferten köras på Azure SQL Database eller Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64d9a-128">With very minor modification, hello following Ring Buffer code sample can be run on either Azure SQL Database or Microsoft SQL Server.</span></span> <span data-ttu-id="64d9a-129">hello skillnaden är hello förekomst av hello noden '_databas' i hello namnet på vissa dynamiska hanteringsvyer (av DMV: er), används i hello FROM-satsen i steg 5.</span><span class="sxs-lookup"><span data-stu-id="64d9a-129">hello difference is hello presence of hello node '_database' in hello name of some dynamic management views (DMVs), used in hello FROM clause in Step 5.</span></span> <span data-ttu-id="64d9a-130">Exempel:</span><span class="sxs-lookup"><span data-stu-id="64d9a-130">For example:</span></span>

* <span data-ttu-id="64d9a-131">sys.dm_xe**_databas**_session_targets</span><span class="sxs-lookup"><span data-stu-id="64d9a-131">sys.dm_xe**_database**_session_targets</span></span>
* <span data-ttu-id="64d9a-132">sys.dm_xe_session_targets</span><span class="sxs-lookup"><span data-stu-id="64d9a-132">sys.dm_xe_session_targets</span></span>

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

## <a name="ring-buffer-contents"></a><span data-ttu-id="64d9a-133">Ring Buffer innehållet</span><span class="sxs-lookup"><span data-stu-id="64d9a-133">Ring Buffer contents</span></span>

<span data-ttu-id="64d9a-134">Vi använde ssms.exe toorun hello kodexempel.</span><span class="sxs-lookup"><span data-stu-id="64d9a-134">We used ssms.exe toorun hello code sample.</span></span>

<span data-ttu-id="64d9a-135">tooview hello resultat vi klickat på hello cell under hello kolumnrubriken **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="64d9a-135">tooview hello results, we clicked hello cell under hello column header **target_data_XML**.</span></span>

<span data-ttu-id="64d9a-136">Sedan i resultatfönstret hello vi klickade hello cell under hello kolumnrubriken **target_data_XML**.</span><span class="sxs-lookup"><span data-stu-id="64d9a-136">Then in hello results pane we clicked hello cell under hello column header **target_data_XML**.</span></span> <span data-ttu-id="64d9a-137">Detta klickar du på Skapa en annan fil flik i ssms.exe i vilka hello innehållet i hello resultatcellen visades som XML.</span><span class="sxs-lookup"><span data-stu-id="64d9a-137">This click created another file tab in ssms.exe in which hello content of hello result cell was displayed, as XML.</span></span>

<span data-ttu-id="64d9a-138">hello utdata visas i hello följande block.</span><span class="sxs-lookup"><span data-stu-id="64d9a-138">hello output is shown in hello following block.</span></span> <span data-ttu-id="64d9a-139">Det ser ut långt, men det är bara två  **<event>**  element.</span><span class="sxs-lookup"><span data-stu-id="64d9a-139">It looks long, but it is just two **<event>** elements.</span></span>

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


#### <a name="release-resources-held-by-your-ring-buffer"></a><span data-ttu-id="64d9a-140">Frigöra resurser som innehas av Ring Buffer</span><span class="sxs-lookup"><span data-stu-id="64d9a-140">Release resources held by your Ring Buffer</span></span>

<span data-ttu-id="64d9a-141">När du är klar med Ring Buffer kan du ta bort den och släpper dess resurser som utfärdar en **ALTER** som hello följande:</span><span class="sxs-lookup"><span data-stu-id="64d9a-141">When you are done with your Ring Buffer, you can remove it and release its resources issuing an **ALTER** like hello following:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    DROP TARGET package0.ring_buffer;
GO
```


<span data-ttu-id="64d9a-142">hello definitionen för händelsesessionen uppdaterats, men inte ta bort.</span><span class="sxs-lookup"><span data-stu-id="64d9a-142">hello definition of your event session is updated, but not dropped.</span></span> <span data-ttu-id="64d9a-143">Du kan senare lägga till en annan instans av hello ringbufferten tooyour händelsesessionen:</span><span class="sxs-lookup"><span data-stu-id="64d9a-143">Later you can add another instance of hello Ring Buffer tooyour event session:</span></span>

```sql
ALTER EVENT SESSION eventsession_gm_azuresqldb51
    ON DATABASE
    ADD TARGET
        package0.ring_buffer
            (SET
                max_memory = 500   -- Units of KB.
            );
```


## <a name="more-information"></a><span data-ttu-id="64d9a-144">Mer information</span><span class="sxs-lookup"><span data-stu-id="64d9a-144">More information</span></span>

<span data-ttu-id="64d9a-145">hello primära avsnittet för utökade händelser på Azure SQL Database är:</span><span class="sxs-lookup"><span data-stu-id="64d9a-145">hello primary topic for extended events on Azure SQL Database is:</span></span>

* <span data-ttu-id="64d9a-146">[Utökad händelse överväganden i SQL-databas](sql-database-xevent-db-diff-from-svr.md), vilket står i kontrast vissa aspekter av utökade händelser som skiljer sig åt mellan Azure SQL Database jämfört med Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="64d9a-146">[Extended event considerations in SQL Database](sql-database-xevent-db-diff-from-svr.md), which contrasts some aspects of extended events that differ between Azure SQL Database versus Microsoft SQL Server.</span></span>

<span data-ttu-id="64d9a-147">Andra exempel avsnitt i koden för utökade händelser finns på följande länkar hello.</span><span class="sxs-lookup"><span data-stu-id="64d9a-147">Other code sample topics for extended events are available at hello following links.</span></span> <span data-ttu-id="64d9a-148">Du måste dock regelbundet kontrollera alla exempel toosee om hello exempel riktar sig till Microsoft SQL Server jämfört med Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="64d9a-148">However, you must routinely check any sample toosee whether hello sample targets Microsoft SQL Server versus Azure SQL Database.</span></span> <span data-ttu-id="64d9a-149">Sedan kan du bestämma om mindre ändringar är nödvändiga toorun hello exempel.</span><span class="sxs-lookup"><span data-stu-id="64d9a-149">Then you can decide whether minor changes are needed toorun hello sample.</span></span>

* <span data-ttu-id="64d9a-150">Kodexempel för Azure SQL Database: [händelsefilen mål koden för utökade händelser i SQL-databas](sql-database-xevent-code-event-file.md)</span><span class="sxs-lookup"><span data-stu-id="64d9a-150">Code sample for Azure SQL Database: [Event File target code for extended events in SQL Database](sql-database-xevent-code-event-file.md)</span></span>

<!--
('lock_acquired' event.)

- Code sample for SQL Server: [Determine Which Queries Are Holding Locks](http://msdn.microsoft.com/library/bb677357.aspx)
- Code sample for SQL Server: [Find hello Objects That Have hello Most Locks Taken on Them](http://msdn.microsoft.com/library/bb630355.aspx)
-->
