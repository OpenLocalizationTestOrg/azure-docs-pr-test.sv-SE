---
title: "aaaXEvent händelsefilen koden för SQL-databas | Microsoft Docs"
description: "Tillhandahåller PowerShell och Transact-SQL för en tvåfasmigrering kodexempel som visar hello händelsefilen målet i en utökad händelse på Azure SQL Database. Azure Storage är en obligatorisk del av det här scenariot."
services: sql-database
documentationcenter: 
author: MightyPen
manager: jhubbard
editor: 
tags: 
ms.assetid: bbb10ecc-739f-4159-b844-12b4be161231
ms.service: sql-database
ms.custom: monitor & tune
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/06/2017
ms.author: genemi
ms.openlocfilehash: 4457bd3250f4644b54da2f7daddb9da12070e93a
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="ad74b-104">Händelsekod filen mål för utökade händelser i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="ad74b-104">Event File target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="ad74b-105">Du en fullständig kodexemplet en robust sätt toocapture och rapportera information för en utökad händelse.</span><span class="sxs-lookup"><span data-stu-id="ad74b-105">You want a complete code sample for a robust way toocapture and report information for an extended event.</span></span>

<span data-ttu-id="ad74b-106">I Microsoft SQL Server hello [händelse målfil](http://msdn.microsoft.com/library/ff878115.aspx) är används toostore händelse utdata till en fil på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="ad74b-106">In Microsoft SQL Server, hello [Event File target](http://msdn.microsoft.com/library/ff878115.aspx) is used toostore event outputs into a local hard drive file.</span></span> <span data-ttu-id="ad74b-107">Men dessa filer är inte tillgängliga tooAzure SQL-databas.</span><span class="sxs-lookup"><span data-stu-id="ad74b-107">But such files are not available tooAzure SQL Database.</span></span> <span data-ttu-id="ad74b-108">I stället använder vi hello Azure Storage service toosupport hello händelsefilen mål.</span><span class="sxs-lookup"><span data-stu-id="ad74b-108">Instead we use hello Azure Storage service toosupport hello Event File target.</span></span>

<span data-ttu-id="ad74b-109">Det här avsnittet presenteras ett kodexempel som två faser:</span><span class="sxs-lookup"><span data-stu-id="ad74b-109">This topic presents a two-phase code sample:</span></span>

* <span data-ttu-id="ad74b-110">PowerShell toocreate en Azure Storage-behållare i hello molnet.</span><span class="sxs-lookup"><span data-stu-id="ad74b-110">PowerShell, toocreate an Azure Storage container in hello cloud.</span></span>
* <span data-ttu-id="ad74b-111">Transact-SQL:</span><span class="sxs-lookup"><span data-stu-id="ad74b-111">Transact-SQL:</span></span>
  
  * <span data-ttu-id="ad74b-112">tooassign hello Azure Storage-behållare tooan händelsefilen mål.</span><span class="sxs-lookup"><span data-stu-id="ad74b-112">tooassign hello Azure Storage container tooan Event File target.</span></span>
  * <span data-ttu-id="ad74b-113">toocreate och starta hello händelsesessionen och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ad74b-113">toocreate and start hello event session, and so on.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="ad74b-114">Krav</span><span class="sxs-lookup"><span data-stu-id="ad74b-114">Prerequisites</span></span>

* <span data-ttu-id="ad74b-115">Ett Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ad74b-115">An Azure account and subscription.</span></span> <span data-ttu-id="ad74b-116">Registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="ad74b-116">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="ad74b-117">Alla databaser som du kan skapa en tabell i.</span><span class="sxs-lookup"><span data-stu-id="ad74b-117">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="ad74b-118">Alternativt kan du [skapa en **AdventureWorksLT** demonstrationsdatabas](sql-database-get-started.md) i minuter.</span><span class="sxs-lookup"><span data-stu-id="ad74b-118">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="ad74b-119">SQL Server Management Studio (ssms.exe), helst den senaste månatliga update-versionen.</span><span class="sxs-lookup"><span data-stu-id="ad74b-119">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="ad74b-120">Du kan hämta hello senaste ssms.exe från:</span><span class="sxs-lookup"><span data-stu-id="ad74b-120">You can download hello latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="ad74b-121">Avsnittet [Hämta SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="ad74b-121">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="ad74b-122">En direktlänk toohello hämtning.</span><span class="sxs-lookup"><span data-stu-id="ad74b-122">A direct link toohello download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)
* <span data-ttu-id="ad74b-123">Du måste ha hello [Azure PowerShell-moduler](http://go.microsoft.com/?linkid=9811175) installerad.</span><span class="sxs-lookup"><span data-stu-id="ad74b-123">You must have hello [Azure PowerShell modules](http://go.microsoft.com/?linkid=9811175) installed.</span></span>
  
  * <span data-ttu-id="ad74b-124">hello moduler ger kommandon som - **ny AzureStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="ad74b-124">hello modules provide commands such as - **New-AzureStorageAccount**.</span></span>

## <a name="phase-1-powershell-code-for-azure-storage-container"></a><span data-ttu-id="ad74b-125">Fas 1: PowerShell-koden för Azure Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="ad74b-125">Phase 1: PowerShell code for Azure Storage container</span></span>

<span data-ttu-id="ad74b-126">Det här PowerShell är fas 1 av hello 2PC kodexempel.</span><span class="sxs-lookup"><span data-stu-id="ad74b-126">This PowerShell is phase 1 of hello two-phase code sample.</span></span>

<span data-ttu-id="ad74b-127">hello skriptet startas med kommandon tooclean efter en eventuell tidigare kör och är rerunnable.</span><span class="sxs-lookup"><span data-stu-id="ad74b-127">hello script starts with commands tooclean up after a possible previous run, and is rerunnable.</span></span>

1. <span data-ttu-id="ad74b-128">Klistra in hello PowerShell-skript i en enkel textredigerare, till exempel Notepad.exe och spara hello skript som en fil med hello **.ps1**.</span><span class="sxs-lookup"><span data-stu-id="ad74b-128">Paste hello PowerShell script into a simple text editor such as Notepad.exe, and save hello script as a file with hello extension **.ps1**.</span></span>
2. <span data-ttu-id="ad74b-129">Starta PowerShell ISE som administratör.</span><span class="sxs-lookup"><span data-stu-id="ad74b-129">Start PowerShell ISE as an Administrator.</span></span>
3. <span data-ttu-id="ad74b-130">Hello Kommandotolken skriver du:</span><span class="sxs-lookup"><span data-stu-id="ad74b-130">At hello prompt, type</span></span><br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/><span data-ttu-id="ad74b-131">och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="ad74b-131">and then press Enter.</span></span>
4. <span data-ttu-id="ad74b-132">I PowerShell ISE öppnar din **.ps1** fil.</span><span class="sxs-lookup"><span data-stu-id="ad74b-132">In PowerShell ISE, open your **.ps1** file.</span></span> <span data-ttu-id="ad74b-133">Kör skriptet hello.</span><span class="sxs-lookup"><span data-stu-id="ad74b-133">Run hello script.</span></span>
5. <span data-ttu-id="ad74b-134">hello skript startar ett nytt fönster där du loggar in tooAzure först.</span><span class="sxs-lookup"><span data-stu-id="ad74b-134">hello script first starts a new window in which you log in tooAzure.</span></span>
   
   * <span data-ttu-id="ad74b-135">Om du kör skriptet hello utan att avbryta din session har hello praktiskt möjlighet att kommentera bort hello **Add-AzureAccount** kommando.</span><span class="sxs-lookup"><span data-stu-id="ad74b-135">If you rerun hello script without disrupting your session, you have hello convenient option of commenting out hello **Add-AzureAccount** command.</span></span>

![PowerShell ISE med Azure-modulen installerad, redo toorun skript.][30_powershell_ise]


### <a name="powershell-code"></a><span data-ttu-id="ad74b-137">PowerShell-kod</span><span class="sxs-lookup"><span data-stu-id="ad74b-137">PowerShell code</span></span>

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after hello first run.
# Current PowerShell environment retains hello successful outcome.

'Expect a pop-up window in which you log in tooAzure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit hello values assigned toothese variables, especially hello first few!
'

# Ensure hello current date is between
# hello Expiry and Start time values that you edit here.

$subscriptionName    = 'YOUR_SUBSCRIPTION_NAME'
$policySasExpiryTime = '2016-01-28T23:44:56Z'
$policySasStartTime  = '2015-08-01'


$storageAccountName     = 'gmstorageaccountxevent'
$storageAccountLocation = 'West US'
$contextName            = 'gmcontext'
$containerName          = 'gmcontainerxevent'
$policySasToken         = 'gmpolicysastoken'


# Leave this value alone, as 'rwl'.
$policySasPermission = 'rwl'

#--------------- 3 -----------------------


# hello ending display lists your Azure subscriptions.
# One should match hello $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for hello current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up hello old Azure Storage Account after any previous run, 
before continuing this new run.'


If ($storageAccountName)
{
    Remove-AzureStorageAccount -StorageAccountName $storageAccountName
}

#--------------- 5 -----------------------

[System.DateTime]::Now.ToString()

'
Create a storage account. 
This might take several minutes, will beep when ready.
  ...PLEASE WAIT...'

New-AzureStorageAccount `
    -StorageAccountName $storageAccountName `
    -Location           $storageAccountLocation

[System.DateTime]::Now.ToString()

[System.Media.SystemSounds]::Beep.Play()


'
Get hello primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# hello context will be needed toocreate a container within hello storage account.

'Create a context object from hello storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within hello storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy toobe applied toohello SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for hello container.
'
Try
{
    $sasTokenWithPolicy = New-AzureStorageContainerSASToken `
        -Name    $containerName `
        -Context $context `
        -Policy  $policySasToken
}
Catch 
{
    $Error[0].Exception.ToString()
}

#-------------- 7 ------------------------


'Display hello values that YOU must edit into hello Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here toodelete your Azure Storage account. See hello preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift toohello Transact-SQL portion of hello two-part code sample!'

# EOFile
```


<span data-ttu-id="ad74b-138">Anteckna hello några namngivna värden som hello PowerShell-skript ut när det avslutas.</span><span class="sxs-lookup"><span data-stu-id="ad74b-138">Take note of hello few named values that hello PowerShell script prints when it ends.</span></span> <span data-ttu-id="ad74b-139">Du måste redigera dessa värden till hello Transact-SQL-skript som följer som fas 2.</span><span class="sxs-lookup"><span data-stu-id="ad74b-139">You must edit those values into hello Transact-SQL script that follows as phase 2.</span></span>

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a><span data-ttu-id="ad74b-140">Fas 2: Transact-SQL-kod som använder Azure Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="ad74b-140">Phase 2: Transact-SQL code that uses Azure Storage container</span></span>

* <span data-ttu-id="ad74b-141">Fas 1 av det här kodexemplet körde du ett PowerShell-skript toocreate en Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="ad74b-141">In phase 1 of this code sample, you ran a PowerShell script toocreate an Azure Storage container.</span></span>
* <span data-ttu-id="ad74b-142">Nästa fas 2 måste hello följande Transact-SQL-skript använda hello behållare.</span><span class="sxs-lookup"><span data-stu-id="ad74b-142">Next in phase 2, hello following Transact-SQL script must use hello container.</span></span>

<span data-ttu-id="ad74b-143">hello skriptet startas med kommandon tooclean efter en eventuell tidigare kör och är rerunnable.</span><span class="sxs-lookup"><span data-stu-id="ad74b-143">hello script starts with commands tooclean up after a possible previous run, and is rerunnable.</span></span>

<span data-ttu-id="ad74b-144">hello PowerShell-skript ut några namngivna värden när det avslutades.</span><span class="sxs-lookup"><span data-stu-id="ad74b-144">hello PowerShell script printed a few named values when it ended.</span></span> <span data-ttu-id="ad74b-145">Du måste redigera hello Transact-SQL-skript toouse dessa värden.</span><span class="sxs-lookup"><span data-stu-id="ad74b-145">You must edit hello Transact-SQL script toouse those values.</span></span> <span data-ttu-id="ad74b-146">Hitta **TODO** hello Transact-SQL-skript toolocate hello Redigera punkter.</span><span class="sxs-lookup"><span data-stu-id="ad74b-146">Find **TODO** in hello Transact-SQL script toolocate hello edit points.</span></span>

1. <span data-ttu-id="ad74b-147">Öppna SQL Server Management Studio (ssms.exe).</span><span class="sxs-lookup"><span data-stu-id="ad74b-147">Open SQL Server Management Studio (ssms.exe).</span></span>
2. <span data-ttu-id="ad74b-148">Ansluta tooyour på Azure SQL-databasen.</span><span class="sxs-lookup"><span data-stu-id="ad74b-148">Connect tooyour Azure SQL Database database.</span></span>
3. <span data-ttu-id="ad74b-149">Klicka på tooopen nya frågefönstret.</span><span class="sxs-lookup"><span data-stu-id="ad74b-149">Click tooopen a new query pane.</span></span>
4. <span data-ttu-id="ad74b-150">Klistra in följande Transact-SQL-skript i frågefönstret hello hello.</span><span class="sxs-lookup"><span data-stu-id="ad74b-150">Paste hello following Transact-SQL script into hello query pane.</span></span>
5. <span data-ttu-id="ad74b-151">Hitta alla **TODO** i hello skript och göra hello lämpliga ändringar.</span><span class="sxs-lookup"><span data-stu-id="ad74b-151">Find every **TODO** in hello script and make hello appropriate edits.</span></span>
6. <span data-ttu-id="ad74b-152">Spara och kör sedan hello skript.</span><span class="sxs-lookup"><span data-stu-id="ad74b-152">Save, and then run hello script.</span></span>


> [!WARNING]
> <span data-ttu-id="ad74b-153">hello SAS-nyckelvärdet som genererats av hello föregående PowerShell-skript kan börja med en '?' (frågetecken).</span><span class="sxs-lookup"><span data-stu-id="ad74b-153">hello SAS key value generated by hello preceding PowerShell script might begin with a '?' (question mark).</span></span> <span data-ttu-id="ad74b-154">När du använder hello SAS-nyckeln i hello följande T-SQL-skript, måste du *ta bort hello inledande '?'* .</span><span class="sxs-lookup"><span data-stu-id="ad74b-154">When you use hello SAS key in hello following T-SQL script, you must *remove hello leading '?'*.</span></span> <span data-ttu-id="ad74b-155">Annars kan ditt arbete blockeras av säkerhet.</span><span class="sxs-lookup"><span data-stu-id="ad74b-155">Otherwise your efforts might be blocked by security.</span></span>


### <a name="transact-sql-code"></a><span data-ttu-id="ad74b-156">Transact-SQL-kod</span><span class="sxs-lookup"><span data-stu-id="ad74b-156">Transact-SQL code</span></span>

```sql
---- TODO: First, run hello PowerShell portion of this two-part code sample.
---- TODO: Second, find every 'TODO' in this Transact-SQL file, and edit each.

---- Transact-SQL code for Event File target on Azure SQL Database.


SET NOCOUNT ON;
GO


----  Step 1.  Establish one little table, and  ---------
----  insert one row of data.


IF EXISTS
    (SELECT * FROM sys.objects
        WHERE type = 'U' and name = 'gmTabEmployee')
BEGIN
    DROP TABLE gmTabEmployee;
END
GO


CREATE TABLE gmTabEmployee
(
    EmployeeGuid         uniqueIdentifier   not null  default newid()  primary key,
    EmployeeId           int                not null  identity(1,1),
    EmployeeKudosCount   int                not null  default 0,
    EmployeeDescr        nvarchar(256)          null
);
GO


INSERT INTO gmTabEmployee ( EmployeeDescr )
    VALUES ( 'Jane Doe' );
GO


------  Step 2.  Create key, and  ------------
------  Create credential (your Azure Storage container must already exist).


IF NOT EXISTS
    (SELECT * FROM sys.symmetric_keys
        WHERE symmetric_key_id = 101)
BEGIN
    CREATE MASTER KEY ENCRYPTION BY PASSWORD = '0C34C960-6621-4682-A123-C7EA08E3FC46' -- Or any newid().
END
GO


IF EXISTS
    (SELECT * FROM sys.database_scoped_credentials
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in hello long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  hello event session has an event with an action,
------  and a has a target.

IF EXISTS
    (SELECT * from sys.database_event_sessions
        WHERE name = 'gmeventsessionname240b')
BEGIN
    DROP
        EVENT SESSION
            gmeventsessionname240b
        ON DATABASE;
END
GO


CREATE
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE

    ADD EVENT
        sqlserver.sql_statement_starting
            (
            ACTION (sqlserver.sql_text)
            WHERE statement LIKE 'UPDATE gmTabEmployee%'
            )
    ADD TARGET
        package0.event_file
            (
            -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
            -- Also, tweak hello .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start hello event session.  ----------------
------  Issue hello SQL Update statements that will be traced.
------  Then stop hello session.

------  Note: If hello target fails tooattach,
------  hello session must be stopped and restarted.

ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = START;
GO


SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
GO


ALTER
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE
    STATE = STOP;
GO


-------------- Step 5.  Select hello results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and hello associated Container name.
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b',
                null, null, null
            );
GO


-------------- Step 6.  Clean up. ----------

DROP
    EVENT SESSION
        gmeventsessionname240b
    ON DATABASE;
GO

DROP DATABASE SCOPED CREDENTIAL
    -- TODO: Assign AzureStorageAccount name, and hello associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount toodelete your Azure Storage account!';
GO
```


<span data-ttu-id="ad74b-157">Om hello mål misslyckas tooattach när du kör måste du stoppa och starta om sessionen för hello händelser:</span><span class="sxs-lookup"><span data-stu-id="ad74b-157">If hello target fails tooattach when you run, you must stop and restart hello event session:</span></span>

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a><span data-ttu-id="ad74b-158">Resultat</span><span class="sxs-lookup"><span data-stu-id="ad74b-158">Output</span></span>

<span data-ttu-id="ad74b-159">När hello Transact-SQL-skriptet är klar klickar du på en cell under hello **event_data_XML** kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="ad74b-159">When hello Transact-SQL script completes, click a cell under hello **event_data_XML** column header.</span></span> <span data-ttu-id="ad74b-160">En  **<event>**  element visas som visar en UPDATE-instruktion.</span><span class="sxs-lookup"><span data-stu-id="ad74b-160">One **<event>** element is displayed which shows one UPDATE statement.</span></span>

<span data-ttu-id="ad74b-161">Här är en  **<event>**  element som genererades under testning:</span><span class="sxs-lookup"><span data-stu-id="ad74b-161">Here is one **<event>** element that was generated during testing:</span></span>


```xml
<event name="sql_statement_starting" package="sqlserver" timestamp="2015-09-22T19:18:45.420Z">
  <data name="state">
    <value>0</value>
    <text>Normal</text>
  </data>
  <data name="line_number">
    <value>5</value>
  </data>
  <data name="offset">
    <value>148</value>
  </data>
  <data name="offset_end">
    <value>368</value>
  </data>
  <data name="statement">
    <value>UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe'</value>
  </data>
  <action name="sql_text" package="sqlserver">
    <value>

SELECT 'BEFORE_Updates', EmployeeKudosCount, * FROM gmTabEmployee;

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 2
    WHERE EmployeeDescr = 'Jane Doe';

UPDATE gmTabEmployee
    SET EmployeeKudosCount = EmployeeKudosCount + 13
    WHERE EmployeeDescr = 'Jane Doe';

SELECT 'AFTER__Updates', EmployeeKudosCount, * FROM gmTabEmployee;
</value>
  </action>
</event>
```


<span data-ttu-id="ad74b-162">Hej föregående Transact-SQL-skriptet som används hello efter system funktionen tooread hello event_file:</span><span class="sxs-lookup"><span data-stu-id="ad74b-162">hello preceding Transact-SQL script used hello following system function tooread hello event_file:</span></span>

* [<span data-ttu-id="ad74b-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="ad74b-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/cc280743.aspx)

<span data-ttu-id="ad74b-164">En förklaring av avancerade alternativ för hello visning av data från utökade händelser finns på:</span><span class="sxs-lookup"><span data-stu-id="ad74b-164">An explanation of advanced options for hello viewing of data from extended events is available at:</span></span>

* [<span data-ttu-id="ad74b-165">Avancerade visning av måldata från utökade händelser</span><span class="sxs-lookup"><span data-stu-id="ad74b-165">Advanced Viewing of Target Data from Extended Events</span></span>](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-hello-code-sample-toorun-on-sql-server"></a><span data-ttu-id="ad74b-166">Konvertera hello kod exempel toorun på SQL Server</span><span class="sxs-lookup"><span data-stu-id="ad74b-166">Converting hello code sample toorun on SQL Server</span></span>

<span data-ttu-id="ad74b-167">Anta att du vill ha toorun hello föregående Transact-SQL-exemplet på Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ad74b-167">Suppose you wanted toorun hello preceding Transact-SQL sample on Microsoft SQL Server.</span></span>

* <span data-ttu-id="ad74b-168">För enkelhetens skull du kan toocompletely Ersätt användning av hello Azure Storage-behållare med en enkel fil som **C:\myeventdata.xel**.</span><span class="sxs-lookup"><span data-stu-id="ad74b-168">For simplicity, you would want toocompletely replace use of hello Azure Storage container with a simple file such as **C:\myeventdata.xel**.</span></span> <span data-ttu-id="ad74b-169">hello-filen skulle skrivas toohello hårddisken på hello-dator som är värd för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="ad74b-169">hello file would be written toohello local hard drive of hello computer that hosts SQL Server.</span></span>
* <span data-ttu-id="ad74b-170">Du behöver inte någon form av Transact-SQL-uttryck för **CREATE MASTER KEY** och **skapa AUTENTISERINGSUPPGIFTER**.</span><span class="sxs-lookup"><span data-stu-id="ad74b-170">You would not need any kind of Transact-SQL statements for **CREATE MASTER KEY** and **CREATE CREDENTIAL**.</span></span>
* <span data-ttu-id="ad74b-171">I hello **Skapa händelse SESSION** instruktionen i dess **lägga till MÅLET** -sats, ska du ersätta hello http-värdet som tilldelas gjorts för**filename =** med en fullständig sökvägssträng som  **C:\myfile.xel**.</span><span class="sxs-lookup"><span data-stu-id="ad74b-171">In hello **CREATE EVENT SESSION** statement, in its **ADD TARGET** clause, you would replace hello Http value assigned made too**filename=** with a full path string like **C:\myfile.xel**.</span></span>
  
  * <span data-ttu-id="ad74b-172">Inga Azure Storage-konto behöver involveras.</span><span class="sxs-lookup"><span data-stu-id="ad74b-172">No Azure Storage account need be involved.</span></span>

## <a name="more-information"></a><span data-ttu-id="ad74b-173">Mer information</span><span class="sxs-lookup"><span data-stu-id="ad74b-173">More information</span></span>

<span data-ttu-id="ad74b-174">Mer information om konton och behållare i hello Azure Storage-tjänst finns:</span><span class="sxs-lookup"><span data-stu-id="ad74b-174">For more info about accounts and containers in hello Azure Storage service, see:</span></span>

* [<span data-ttu-id="ad74b-175">Hur toouse Blob storage från .NET</span><span class="sxs-lookup"><span data-stu-id="ad74b-175">How toouse Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="ad74b-176">Namnge och referera till behållare, Blobbar och Metadata</span><span class="sxs-lookup"><span data-stu-id="ad74b-176">Naming and Referencing Containers, Blobs, and Metadata</span></span>](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [<span data-ttu-id="ad74b-177">Arbeta med hello Rotbehållare</span><span class="sxs-lookup"><span data-stu-id="ad74b-177">Working with hello Root Container</span></span>](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [<span data-ttu-id="ad74b-178">Lektionen 1: Skapa en princip för lagrade åtkomst och en signatur för delad åtkomst på en Azure-behållare</span><span class="sxs-lookup"><span data-stu-id="ad74b-178">Lesson 1: Create a stored access policy and a shared access signature on an Azure container</span></span>](http://msdn.microsoft.com/library/dn466430.aspx)
  * [<span data-ttu-id="ad74b-179">Lektionen 2: Skapa en SQL Server-autentiseringsuppgifter med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="ad74b-179">Lesson 2: Create a SQL Server credential using a shared access signature</span></span>](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

