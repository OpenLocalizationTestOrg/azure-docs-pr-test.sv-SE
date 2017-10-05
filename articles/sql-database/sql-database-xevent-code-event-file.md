---
title: "XEvent-händelsefilen koden för SQL-databas | Microsoft Docs"
description: "Tillhandahåller PowerShell och Transact-SQL för en tvåfasmigrering kodexempel som visar händelsefilen målet i en utökad händelse på Azure SQL Database. Azure Storage är en obligatorisk del av det här scenariot."
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
ms.openlocfilehash: e8c7a9af11ac4c22be00426337ab7c8b8ff0860f
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a><span data-ttu-id="8b4f2-104">Händelsekod filen mål för utökade händelser i SQL-databas</span><span class="sxs-lookup"><span data-stu-id="8b4f2-104">Event File target code for extended events in SQL Database</span></span>

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

<span data-ttu-id="8b4f2-105">Du vill använda en fullständig kodexempel för ett stabilt sätt att avbilda och rapportera information om en utökad händelse.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-105">You want a complete code sample for a robust way to capture and report information for an extended event.</span></span>

<span data-ttu-id="8b4f2-106">I Microsoft SQL Server på [händelse målfil](http://msdn.microsoft.com/library/ff878115.aspx) används för att lagra händelsen utdata till en fil på hårddisken.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-106">In Microsoft SQL Server, the [Event File target](http://msdn.microsoft.com/library/ff878115.aspx) is used to store event outputs into a local hard drive file.</span></span> <span data-ttu-id="8b4f2-107">Men dessa filer är inte tillgänglig för Azure SQL Database.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-107">But such files are not available to Azure SQL Database.</span></span> <span data-ttu-id="8b4f2-108">I stället använder vi Azure Storage-tjänsten för att stödja målfil för händelsen.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-108">Instead we use the Azure Storage service to support the Event File target.</span></span>

<span data-ttu-id="8b4f2-109">Det här avsnittet presenteras ett kodexempel som två faser:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-109">This topic presents a two-phase code sample:</span></span>

* <span data-ttu-id="8b4f2-110">PowerShell för att skapa en Azure Storage-behållare i molnet.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-110">PowerShell, to create an Azure Storage container in the cloud.</span></span>
* <span data-ttu-id="8b4f2-111">Transact-SQL:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-111">Transact-SQL:</span></span>
  
  * <span data-ttu-id="8b4f2-112">Tilldela Azure Storage-behållare till en målfil för händelsen.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-112">To assign the Azure Storage container to an Event File target.</span></span>
  * <span data-ttu-id="8b4f2-113">Att skapa och starta händelsesessionen och så vidare.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-113">To create and start the event session, and so on.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="8b4f2-114">Krav</span><span class="sxs-lookup"><span data-stu-id="8b4f2-114">Prerequisites</span></span>

* <span data-ttu-id="8b4f2-115">Ett Azure-konto och prenumeration.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-115">An Azure account and subscription.</span></span> <span data-ttu-id="8b4f2-116">Registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="8b4f2-116">You can sign up for a [free trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>
* <span data-ttu-id="8b4f2-117">Alla databaser som du kan skapa en tabell i.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-117">Any database you can create a table in.</span></span>
  
  * <span data-ttu-id="8b4f2-118">Alternativt kan du [skapa en **AdventureWorksLT** demonstrationsdatabas](sql-database-get-started.md) i minuter.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-118">Optionally you can [create an **AdventureWorksLT** demonstration database](sql-database-get-started.md) in minutes.</span></span>
* <span data-ttu-id="8b4f2-119">SQL Server Management Studio (ssms.exe), helst den senaste månatliga update-versionen.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-119">SQL Server Management Studio (ssms.exe), ideally its latest monthly update version.</span></span> 
  <span data-ttu-id="8b4f2-120">Du kan hämta den senaste ssms.exe från:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-120">You can download the latest ssms.exe from:</span></span>
  
  * <span data-ttu-id="8b4f2-121">Avsnittet [Hämta SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span><span class="sxs-lookup"><span data-stu-id="8b4f2-121">Topic titled [Download SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).</span></span>
  * [<span data-ttu-id="8b4f2-122">En direktlänk till nedladdningen.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-122">A direct link to the download.</span></span>](http://go.microsoft.com/fwlink/?linkid=616025)
* <span data-ttu-id="8b4f2-123">Du måste ha den [Azure PowerShell-moduler](http://go.microsoft.com/?linkid=9811175) installerad.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-123">You must have the [Azure PowerShell modules](http://go.microsoft.com/?linkid=9811175) installed.</span></span>
  
  * <span data-ttu-id="8b4f2-124">Moduler med kommandon som - **ny AzureStorageAccount**.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-124">The modules provide commands such as - **New-AzureStorageAccount**.</span></span>

## <a name="phase-1-powershell-code-for-azure-storage-container"></a><span data-ttu-id="8b4f2-125">Fas 1: PowerShell-koden för Azure Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="8b4f2-125">Phase 1: PowerShell code for Azure Storage container</span></span>

<span data-ttu-id="8b4f2-126">Det här PowerShell är fas 1 av två faser kodexemplet.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-126">This PowerShell is phase 1 of the two-phase code sample.</span></span>

<span data-ttu-id="8b4f2-127">Skriptet börjar med kommandon för att rensa efter en eventuell tidigare kör och är rerunnable.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-127">The script starts with commands to clean up after a possible previous run, and is rerunnable.</span></span>

1. <span data-ttu-id="8b4f2-128">Klistra in PowerShell-skriptet i en enkel textredigerare, till exempel Notepad.exe och spara skriptet som en fil med tillägget **.ps1**.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-128">Paste the PowerShell script into a simple text editor such as Notepad.exe, and save the script as a file with the extension **.ps1**.</span></span>
2. <span data-ttu-id="8b4f2-129">Starta PowerShell ISE som administratör.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-129">Start PowerShell ISE as an Administrator.</span></span>
3. <span data-ttu-id="8b4f2-130">I Kommandotolken skriver du:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-130">At the prompt, type</span></span><br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/><span data-ttu-id="8b4f2-131">och tryck sedan på RETUR.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-131">and then press Enter.</span></span>
4. <span data-ttu-id="8b4f2-132">I PowerShell ISE öppnar din **.ps1** fil.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-132">In PowerShell ISE, open your **.ps1** file.</span></span> <span data-ttu-id="8b4f2-133">Kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-133">Run the script.</span></span>
5. <span data-ttu-id="8b4f2-134">Skriptet först startar ett nytt fönster där du loggar in på Azure.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-134">The script first starts a new window in which you log in to Azure.</span></span>
   
   * <span data-ttu-id="8b4f2-135">Om du kör skriptet utan att störa sessionen du välja lämplig kommentera ut den **Add-AzureAccount** kommando.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-135">If you rerun the script without disrupting your session, you have the convenient option of commenting out the **Add-AzureAccount** command.</span></span>

![PowerShell ISE med Azure-modulen installerad, redo att köra skriptet.][30_powershell_ise]


### <a name="powershell-code"></a><span data-ttu-id="8b4f2-137">PowerShell-kod</span><span class="sxs-lookup"><span data-stu-id="8b4f2-137">PowerShell code</span></span>

```powershell
## TODO: Before running, find all 'TODO' and make each edit!!

#--------------- 1 -----------------------


# You can comment out or skip this Add-AzureAccount
# command after the first run.
# Current PowerShell environment retains the successful outcome.

'Expect a pop-up window in which you log in to Azure.'


Add-AzureAccount

#-------------- 2 ------------------------


'
TODO: Edit the values assigned to these variables, especially the first few!
'

# Ensure the current date is between
# the Expiry and Start time values that you edit here.

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


# The ending display lists your Azure subscriptions.
# One should match the $subscriptionName value you assigned
#   earlier in this PowerShell script. 

'Choose an existing subscription for the current PowerShell environment.'


Select-AzureSubscription -SubscriptionName $subscriptionName


#-------------- 4 ------------------------


'
Clean up the old Azure Storage Account after any previous run, 
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
Get the primary access key for your storage account.
'


$primaryAccessKey_ForStorageAccount = `
    (Get-AzureStorageKey `
        -StorageAccountName $storageAccountName).Primary

"`$primaryAccessKey_ForStorageAccount = $primaryAccessKey_ForStorageAccount"

'Azure Storage Account cmdlet completed.
Remainder of PowerShell .ps1 script continues.
'

#--------------- 6 -----------------------


# The context will be needed to create a container within the storage account.

'Create a context object from the storage account and its primary access key.
'

$context = New-AzureStorageContext `
    -StorageAccountName $storageAccountName `
    -StorageAccountKey  $primaryAccessKey_ForStorageAccount


'Create a container within the storage account.
'


$containerObjectInStorageAccount = New-AzureStorageContainer `
    -Name    $containerName `
    -Context $context


'Create a security policy to be applied to the SAS token.
'

New-AzureStorageContainerStoredAccessPolicy `
    -Container  $containerName `
    -Context    $context `
    -Policy     $policySasToken `
    -Permission $policySasPermission `
    -ExpiryTime $policySasExpiryTime `
    -StartTime  $policySasStartTime 

'
Generate a SAS token for the container.
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


'Display the values that YOU must edit into the Transact-SQL script next!:
'

"storageAccountName: $storageAccountName"
"containerName:      $containerName"
"sasTokenWithPolicy: $sasTokenWithPolicy"

'
REMINDER: sasTokenWithPolicy here might start with "?" character, which you must exclude from Transact-SQL.
'

'
(Later, return here to delete your Azure Storage account. See the preceding - Remove-AzureStorageAccount -StorageAccountName $storageAccountName)'

'
Now shift to the Transact-SQL portion of the two-part code sample!'

# EOFile
```


<span data-ttu-id="8b4f2-138">Anteckna några namngivna värden som PowerShell-skriptet ut när det avslutas.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-138">Take note of the few named values that the PowerShell script prints when it ends.</span></span> <span data-ttu-id="8b4f2-139">Du måste redigera dessa värden i Transact-SQL-skript som följer som fas 2.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-139">You must edit those values into the Transact-SQL script that follows as phase 2.</span></span>

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a><span data-ttu-id="8b4f2-140">Fas 2: Transact-SQL-kod som använder Azure Storage-behållare</span><span class="sxs-lookup"><span data-stu-id="8b4f2-140">Phase 2: Transact-SQL code that uses Azure Storage container</span></span>

* <span data-ttu-id="8b4f2-141">Fas 1 av det här kodexemplet körde du ett PowerShell-skript för att skapa en Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-141">In phase 1 of this code sample, you ran a PowerShell script to create an Azure Storage container.</span></span>
* <span data-ttu-id="8b4f2-142">Därefter måste följande Transact-SQL-skript i fas 2 använda behållaren.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-142">Next in phase 2, the following Transact-SQL script must use the container.</span></span>

<span data-ttu-id="8b4f2-143">Skriptet börjar med kommandon för att rensa efter en eventuell tidigare kör och är rerunnable.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-143">The script starts with commands to clean up after a possible previous run, and is rerunnable.</span></span>

<span data-ttu-id="8b4f2-144">PowerShell-skriptet ut några namngivna värden när det avslutades.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-144">The PowerShell script printed a few named values when it ended.</span></span> <span data-ttu-id="8b4f2-145">Du måste redigera Transact-SQL-skript för att använda dessa värden.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-145">You must edit the Transact-SQL script to use those values.</span></span> <span data-ttu-id="8b4f2-146">Hitta **TODO** i Transact-SQL-skript för att hitta redigeringspunkter.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-146">Find **TODO** in the Transact-SQL script to locate the edit points.</span></span>

1. <span data-ttu-id="8b4f2-147">Öppna SQL Server Management Studio (ssms.exe).</span><span class="sxs-lookup"><span data-stu-id="8b4f2-147">Open SQL Server Management Studio (ssms.exe).</span></span>
2. <span data-ttu-id="8b4f2-148">Ansluta till din Azure SQL Database-databas.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-148">Connect to your Azure SQL Database database.</span></span>
3. <span data-ttu-id="8b4f2-149">Klicka för att öppna en ny fråga.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-149">Click to open a new query pane.</span></span>
4. <span data-ttu-id="8b4f2-150">Klistra in följande Transact-SQL-skript i frågefönstret.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-150">Paste the following Transact-SQL script into the query pane.</span></span>
5. <span data-ttu-id="8b4f2-151">Hitta alla **TODO** i skriptet och göra lämpliga ändringar.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-151">Find every **TODO** in the script and make the appropriate edits.</span></span>
6. <span data-ttu-id="8b4f2-152">Spara och kör skriptet.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-152">Save, and then run the script.</span></span>


> [!WARNING]
> <span data-ttu-id="8b4f2-153">SAS-nyckelvärdet som genereras av föregående PowerShell-skriptet kan börja med en '?' (frågetecken).</span><span class="sxs-lookup"><span data-stu-id="8b4f2-153">The SAS key value generated by the preceding PowerShell script might begin with a '?' (question mark).</span></span> <span data-ttu-id="8b4f2-154">När du använder SAS-nyckel i följande T-SQL-skript, måste du *ta bort inledande '?'* .</span><span class="sxs-lookup"><span data-stu-id="8b4f2-154">When you use the SAS key in the following T-SQL script, you must *remove the leading '?'*.</span></span> <span data-ttu-id="8b4f2-155">Annars kan ditt arbete blockeras av säkerhet.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-155">Otherwise your efforts might be blocked by security.</span></span>


### <a name="transact-sql-code"></a><span data-ttu-id="8b4f2-156">Transact-SQL-kod</span><span class="sxs-lookup"><span data-stu-id="8b4f2-156">Transact-SQL code</span></span>

```sql
---- TODO: First, run the PowerShell portion of this two-part code sample.
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
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        WHERE name = 'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent')
BEGIN
    DROP DATABASE SCOPED CREDENTIAL
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent] ;
END
GO


CREATE
    DATABASE SCOPED
    CREDENTIAL
        -- use '.blob.',   and not '.queue.' or '.table.' etc.
        -- TODO: Assign AzureStorageAccount name, and the associated Container name.
        [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    WITH
        IDENTITY = 'SHARED ACCESS SIGNATURE',  -- "SAS" token.
        -- TODO: Paste in the long SasToken string here for Secret, but exclude any leading '?'.
        SECRET = 'sv=2014-02-14&sr=c&si=gmpolicysastoken&sig=EjAqjo6Nu5xMLEZEkMkLbeF7TD9v1J8DNB2t8gOKTts%3D'
    ;
GO


------  Step 3.  Create (define) an event session.  --------
------  The event session has an event with an action,
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
            -- TODO: Assign AzureStorageAccount name, and the associated Container name.
            -- Also, tweak the .xel file name at end, if you like.
            SET filename =
                'https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent/anyfilenamexel242b.xel'
            )
    WITH
        (MAX_MEMORY = 10 MB,
        MAX_DISPATCH_LATENCY = 3 SECONDS)
    ;
GO


------  Step 4.  Start the event session.  ----------------
------  Issue the SQL Update statements that will be traced.
------  Then stop the session.

------  Note: If the target fails to attach,
------  the session must be stopped and restarted.

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


-------------- Step 5.  Select the results. ----------

SELECT
        *, 'CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS!' as [CLICK_NEXT_CELL_TO_BROWSE_ITS_RESULTS],
        CAST(event_data AS XML) AS [event_data_XML]  -- TODO: In ssms.exe results grid, double-click this cell!
    FROM
        sys.fn_xe_file_target_read_file
            (
                -- TODO: Fill in Storage Account name, and the associated Container name.
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
    -- TODO: Assign AzureStorageAccount name, and the associated Container name.
    [https://gmstorageaccountxevent.blob.core.windows.net/gmcontainerxevent]
    ;
GO

DROP TABLE gmTabEmployee;
GO

PRINT 'Use PowerShell Remove-AzureStorageAccount to delete your Azure Storage account!';
GO
```


<span data-ttu-id="8b4f2-157">Om det inte går att koppla när du kör mål, måste du stoppa och starta om händelsesessionen:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-157">If the target fails to attach when you run, you must stop and restart the event session:</span></span>

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a><span data-ttu-id="8b4f2-158">Resultat</span><span class="sxs-lookup"><span data-stu-id="8b4f2-158">Output</span></span>

<span data-ttu-id="8b4f2-159">När Transact-SQL-skriptet har slutförts klickar du på en cell under den **event_data_XML** kolumnrubriken.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-159">When the Transact-SQL script completes, click a cell under the **event_data_XML** column header.</span></span> <span data-ttu-id="8b4f2-160">En  **<event>**  element visas som visar en UPDATE-instruktion.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-160">One **<event>** element is displayed which shows one UPDATE statement.</span></span>

<span data-ttu-id="8b4f2-161">Här är en  **<event>**  element som genererades under testning:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-161">Here is one **<event>** element that was generated during testing:</span></span>


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


<span data-ttu-id="8b4f2-162">Föregående Transact-SQL-skript används följande funktion i operativsystemet för att läsa event_file:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-162">The preceding Transact-SQL script used the following system function to read the event_file:</span></span>

* [<span data-ttu-id="8b4f2-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span><span class="sxs-lookup"><span data-stu-id="8b4f2-163">sys.fn_xe_file_target_read_file (Transact-SQL)</span></span>](http://msdn.microsoft.com/library/cc280743.aspx)

<span data-ttu-id="8b4f2-164">En förklaring av avancerade alternativ för visning av data från utökade händelser finns på:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-164">An explanation of advanced options for the viewing of data from extended events is available at:</span></span>

* [<span data-ttu-id="8b4f2-165">Avancerade visning av måldata från utökade händelser</span><span class="sxs-lookup"><span data-stu-id="8b4f2-165">Advanced Viewing of Target Data from Extended Events</span></span>](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-the-code-sample-to-run-on-sql-server"></a><span data-ttu-id="8b4f2-166">Konvertera kodexemplet körs på SQL Server</span><span class="sxs-lookup"><span data-stu-id="8b4f2-166">Converting the code sample to run on SQL Server</span></span>

<span data-ttu-id="8b4f2-167">Anta att du vill köra föregående Transact-SQL-exemplet på Microsoft SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-167">Suppose you wanted to run the preceding Transact-SQL sample on Microsoft SQL Server.</span></span>

* <span data-ttu-id="8b4f2-168">För enkelhetens skull du vill ersätta helt användning av Azure Storage-behållare i med en enkel fil **C:\myeventdata.xel**.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-168">For simplicity, you would want to completely replace use of the Azure Storage container with a simple file such as **C:\myeventdata.xel**.</span></span> <span data-ttu-id="8b4f2-169">Filen skulle skrivas till den lokala hårddisken på den dator som är värd för SQL Server.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-169">The file would be written to the local hard drive of the computer that hosts SQL Server.</span></span>
* <span data-ttu-id="8b4f2-170">Du behöver inte någon form av Transact-SQL-uttryck för **CREATE MASTER KEY** och **skapa AUTENTISERINGSUPPGIFTER**.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-170">You would not need any kind of Transact-SQL statements for **CREATE MASTER KEY** and **CREATE CREDENTIAL**.</span></span>
* <span data-ttu-id="8b4f2-171">I den **Skapa händelse SESSION** instruktionen i dess **lägga till MÅLET** -sats, ska du ersätta Http-värdet som tilldelas gjort **filename =** med en fullständig sökvägssträng som **C:\myfile.xel**.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-171">In the **CREATE EVENT SESSION** statement, in its **ADD TARGET** clause, you would replace the Http value assigned made to **filename=** with a full path string like **C:\myfile.xel**.</span></span>
  
  * <span data-ttu-id="8b4f2-172">Inga Azure Storage-konto behöver involveras.</span><span class="sxs-lookup"><span data-stu-id="8b4f2-172">No Azure Storage account need be involved.</span></span>

## <a name="more-information"></a><span data-ttu-id="8b4f2-173">Mer information</span><span class="sxs-lookup"><span data-stu-id="8b4f2-173">More information</span></span>

<span data-ttu-id="8b4f2-174">Mer information om konton och behållare i Azure Storage-tjänsten finns:</span><span class="sxs-lookup"><span data-stu-id="8b4f2-174">For more info about accounts and containers in the Azure Storage service, see:</span></span>

* [<span data-ttu-id="8b4f2-175">Använda Blob storage från .NET</span><span class="sxs-lookup"><span data-stu-id="8b4f2-175">How to use Blob storage from .NET</span></span>](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [<span data-ttu-id="8b4f2-176">Namnge och referera till behållare, Blobbar och Metadata</span><span class="sxs-lookup"><span data-stu-id="8b4f2-176">Naming and Referencing Containers, Blobs, and Metadata</span></span>](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [<span data-ttu-id="8b4f2-177">Arbeta med Root-behållaren</span><span class="sxs-lookup"><span data-stu-id="8b4f2-177">Working with the Root Container</span></span>](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [<span data-ttu-id="8b4f2-178">Lektionen 1: Skapa en princip för lagrade åtkomst och en signatur för delad åtkomst på en Azure-behållare</span><span class="sxs-lookup"><span data-stu-id="8b4f2-178">Lesson 1: Create a stored access policy and a shared access signature on an Azure container</span></span>](http://msdn.microsoft.com/library/dn466430.aspx)
  * [<span data-ttu-id="8b4f2-179">Lektionen 2: Skapa en SQL Server-autentiseringsuppgifter med hjälp av en signatur för delad åtkomst</span><span class="sxs-lookup"><span data-stu-id="8b4f2-179">Lesson 2: Create a SQL Server credential using a shared access signature</span></span>](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

