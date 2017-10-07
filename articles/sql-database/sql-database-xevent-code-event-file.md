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
# <a name="event-file-target-code-for-extended-events-in-sql-database"></a>Händelsekod filen mål för utökade händelser i SQL-databas

[!INCLUDE [sql-database-xevents-selectors-1-include](../../includes/sql-database-xevents-selectors-1-include.md)]

Du en fullständig kodexemplet en robust sätt toocapture och rapportera information för en utökad händelse.

I Microsoft SQL Server hello [händelse målfil](http://msdn.microsoft.com/library/ff878115.aspx) är används toostore händelse utdata till en fil på hårddisken. Men dessa filer är inte tillgängliga tooAzure SQL-databas. I stället använder vi hello Azure Storage service toosupport hello händelsefilen mål.

Det här avsnittet presenteras ett kodexempel som två faser:

* PowerShell toocreate en Azure Storage-behållare i hello molnet.
* Transact-SQL:
  
  * tooassign hello Azure Storage-behållare tooan händelsefilen mål.
  * toocreate och starta hello händelsesessionen och så vidare.

## <a name="prerequisites"></a>Krav

* Ett Azure-konto och prenumeration. Registrera dig för en [kostnadsfri utvärderingsversion](https://azure.microsoft.com/pricing/free-trial/).
* Alla databaser som du kan skapa en tabell i.
  
  * Alternativt kan du [skapa en **AdventureWorksLT** demonstrationsdatabas](sql-database-get-started.md) i minuter.
* SQL Server Management Studio (ssms.exe), helst den senaste månatliga update-versionen. 
  Du kan hämta hello senaste ssms.exe från:
  
  * Avsnittet [Hämta SQL Server Management Studio](http://msdn.microsoft.com/library/mt238290.aspx).
  * [En direktlänk toohello hämtning.](http://go.microsoft.com/fwlink/?linkid=616025)
* Du måste ha hello [Azure PowerShell-moduler](http://go.microsoft.com/?linkid=9811175) installerad.
  
  * hello moduler ger kommandon som - **ny AzureStorageAccount**.

## <a name="phase-1-powershell-code-for-azure-storage-container"></a>Fas 1: PowerShell-koden för Azure Storage-behållare

Det här PowerShell är fas 1 av hello 2PC kodexempel.

hello skriptet startas med kommandon tooclean efter en eventuell tidigare kör och är rerunnable.

1. Klistra in hello PowerShell-skript i en enkel textredigerare, till exempel Notepad.exe och spara hello skript som en fil med hello **.ps1**.
2. Starta PowerShell ISE som administratör.
3. Hello Kommandotolken skriver du:<br/>`Set-ExecutionPolicy -ExecutionPolicy Unrestricted -Scope CurrentUser`<br/>och tryck sedan på RETUR.
4. I PowerShell ISE öppnar din **.ps1** fil. Kör skriptet hello.
5. hello skript startar ett nytt fönster där du loggar in tooAzure först.
   
   * Om du kör skriptet hello utan att avbryta din session har hello praktiskt möjlighet att kommentera bort hello **Add-AzureAccount** kommando.

![PowerShell ISE med Azure-modulen installerad, redo toorun skript.][30_powershell_ise]


### <a name="powershell-code"></a>PowerShell-kod

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


Anteckna hello några namngivna värden som hello PowerShell-skript ut när det avslutas. Du måste redigera dessa värden till hello Transact-SQL-skript som följer som fas 2.

## <a name="phase-2-transact-sql-code-that-uses-azure-storage-container"></a>Fas 2: Transact-SQL-kod som använder Azure Storage-behållare

* Fas 1 av det här kodexemplet körde du ett PowerShell-skript toocreate en Azure Storage-behållare.
* Nästa fas 2 måste hello följande Transact-SQL-skript använda hello behållare.

hello skriptet startas med kommandon tooclean efter en eventuell tidigare kör och är rerunnable.

hello PowerShell-skript ut några namngivna värden när det avslutades. Du måste redigera hello Transact-SQL-skript toouse dessa värden. Hitta **TODO** hello Transact-SQL-skript toolocate hello Redigera punkter.

1. Öppna SQL Server Management Studio (ssms.exe).
2. Ansluta tooyour på Azure SQL-databasen.
3. Klicka på tooopen nya frågefönstret.
4. Klistra in följande Transact-SQL-skript i frågefönstret hello hello.
5. Hitta alla **TODO** i hello skript och göra hello lämpliga ändringar.
6. Spara och kör sedan hello skript.


> [!WARNING]
> hello SAS-nyckelvärdet som genererats av hello föregående PowerShell-skript kan börja med en '?' (frågetecken). När du använder hello SAS-nyckeln i hello följande T-SQL-skript, måste du *ta bort hello inledande '?'* . Annars kan ditt arbete blockeras av säkerhet.


### <a name="transact-sql-code"></a>Transact-SQL-kod

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


Om hello mål misslyckas tooattach när du kör måste du stoppa och starta om sessionen för hello händelser:

```sql
ALTER EVENT SESSION ... STATE = STOP;
GO
ALTER EVENT SESSION ... STATE = START;
GO
```


## <a name="output"></a>Resultat

När hello Transact-SQL-skriptet är klar klickar du på en cell under hello **event_data_XML** kolumnrubriken. En  **<event>**  element visas som visar en UPDATE-instruktion.

Här är en  **<event>**  element som genererades under testning:


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


Hej föregående Transact-SQL-skriptet som används hello efter system funktionen tooread hello event_file:

* [sys.fn_xe_file_target_read_file (Transact-SQL)](http://msdn.microsoft.com/library/cc280743.aspx)

En förklaring av avancerade alternativ för hello visning av data från utökade händelser finns på:

* [Avancerade visning av måldata från utökade händelser](http://msdn.microsoft.com/library/mt752502.aspx)


## <a name="converting-hello-code-sample-toorun-on-sql-server"></a>Konvertera hello kod exempel toorun på SQL Server

Anta att du vill ha toorun hello föregående Transact-SQL-exemplet på Microsoft SQL Server.

* För enkelhetens skull du kan toocompletely Ersätt användning av hello Azure Storage-behållare med en enkel fil som **C:\myeventdata.xel**. hello-filen skulle skrivas toohello hårddisken på hello-dator som är värd för SQL Server.
* Du behöver inte någon form av Transact-SQL-uttryck för **CREATE MASTER KEY** och **skapa AUTENTISERINGSUPPGIFTER**.
* I hello **Skapa händelse SESSION** instruktionen i dess **lägga till MÅLET** -sats, ska du ersätta hello http-värdet som tilldelas gjorts för**filename =** med en fullständig sökvägssträng som  **C:\myfile.xel**.
  
  * Inga Azure Storage-konto behöver involveras.

## <a name="more-information"></a>Mer information

Mer information om konton och behållare i hello Azure Storage-tjänst finns:

* [Hur toouse Blob storage från .NET](../storage/blobs/storage-dotnet-how-to-use-blobs.md)
* [Namnge och referera till behållare, Blobbar och Metadata](http://msdn.microsoft.com/library/azure/dd135715.aspx)
* [Arbeta med hello Rotbehållare](http://msdn.microsoft.com/library/azure/ee395424.aspx)
* [Lektionen 1: Skapa en princip för lagrade åtkomst och en signatur för delad åtkomst på en Azure-behållare](http://msdn.microsoft.com/library/dn466430.aspx)
  * [Lektionen 2: Skapa en SQL Server-autentiseringsuppgifter med hjälp av en signatur för delad åtkomst](http://msdn.microsoft.com/library/dn466435.aspx)

<!--
Image references.
-->

[30_powershell_ise]: ./media/sql-database-xevent-code-event-file/event-file-powershell-ise-b30.png

