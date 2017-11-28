---
title: aaaPowerShell exempel synkronisering mellan SQL-databas och SQL Server lokala | Microsoft Docs
description: Azure PowerShell exempel skriptet toosync mellan en Azure SQL Database och en lokal SQL Server-databas
services: sql-database
documentationcenter: sql-database
author: jognanay
manager: jhubbard
editor: 
tags: 
ms.assetid: 
ms.service: sql-database
ms.custom: load & move data
ms.devlang: PowerShell
ms.topic: sample
ms.tgt_pltfrm: sql-database
ms.workload: database
ms.date: 07/31/2017
ms.author: douglasl
ms.openlocfilehash: 0b2ddab3b715b42cf7b6a90fd31c0b4571ad322d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-toosync-between-an-azure-sql-database-and-a-sql-server-on-premises-database"></a><span data-ttu-id="22dfd-103">Använd PowerShell toosync mellan en Azure SQL Database och en lokal SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="22dfd-103">Use PowerShell toosync between an Azure SQL Database and a SQL Server on-premises database</span></span>

<span data-ttu-id="22dfd-104">Det här exemplet PowerShell konfigurerar datasynkronisering toosync mellan en Azure SQL Database och en lokal SQL Server-databas.</span><span class="sxs-lookup"><span data-stu-id="22dfd-104">This PowerShell example configures Data Sync toosync between an Azure SQL Database and a SQL Server on-premises database.</span></span> 

<span data-ttu-id="22dfd-105">Det här exemplet kräver hello Azure PowerShell Modulversion 4.2 eller senare.</span><span class="sxs-lookup"><span data-stu-id="22dfd-105">This sample requires hello Azure PowerShell module version 4.2 or later.</span></span> <span data-ttu-id="22dfd-106">Kör `Get-Module -ListAvailable AzureRM` toofind hello installerad version.</span><span class="sxs-lookup"><span data-stu-id="22dfd-106">Run `Get-Module -ListAvailable AzureRM` toofind hello installed version.</span></span> <span data-ttu-id="22dfd-107">Om du behöver tooinstall eller uppgradering, se [installera Azure PowerShell-modulen](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span><span class="sxs-lookup"><span data-stu-id="22dfd-107">If you need tooinstall or upgrade, see [Install Azure PowerShell module](https://docs.microsoft.com/en-us/powershell/azure/install-azurerm-ps).</span></span>
 
<span data-ttu-id="22dfd-108">Kör `Login-AzureRmAccount` toocreate en anslutning till Azure.</span><span class="sxs-lookup"><span data-stu-id="22dfd-108">Run `Login-AzureRmAccount` toocreate a connection with Azure.</span></span> 

## <a name="sample-script"></a><span data-ttu-id="22dfd-109">Exempelskript</span><span class="sxs-lookup"><span data-stu-id="22dfd-109">Sample script</span></span>

```powershell
# prerequisites: 
# 1. Create an Azure Database from AdventureWorksLT sample database as hub database
# 2. Create an Azure Database in hello same region as sync database
# 3. Create an on premises SQL Server Database as member database
# 4. Update hello parameters below before running hello sample
#
using namespace Microsoft.Azure.Commands.Sql.DataSync.Model
using namespace System.Collections.Generic

# Hub database info
# Subscription id for hub database
$SubscriptionId = "subscription_guid"
# Resrouce group name for hub database
$ResourceGroupName = "ResourceGroupName"
# Server name for hub database
$ServerName = "ServerName"
# Database name for hub database
$DatabaseName = "TestHubDatabase"

# Sync database info
# Resource group name for sync database
$SyncDatabaseResourceGroupName = "ResourceGroupName"
# Server name for sync database
$SyncDatabaseServerName = "ServerName"
# Sync database name
$SyncDatabaseName = "SyncDatabaseName"

# Sync group info
# Sync group name
$SyncGroupName = "TestSyncGroup"
# Conflict resolution Policy. Value can be HubWin or MemberWin
$ConflictResolutionPolicy = "HubWin"
# Sync interval in seconds. Value must be no less than 300
$IntervalInSeconds = 300

# Member database info
# Member name
$SyncMemberName = "member"
# Member server name
$MemberServerName = "OnPremiseServer"
# Member database name
$MemberDatabaseName = "MemberDatabaseTest"
# Member database type. Value can be AzureSqlDatabase or SqlServerDatabase
$MemberDatabaseType = "SqlServerDatabase"
# Sync direction. Value can be Bidirectional, Onewaymembertohub, Onewayhubtomember
$SyncDirection = "Bidirectional"

#Sync Agent Info
$SyncAgentName = "TestSyncAgent"
$SyncAgentResourceGroupName = "ResourceGroupName"
$SyncAgentServerName = "ServerName"

# Other info
# Temp file toosave hello sync schema
$TempFile = $env:TEMP+"\syncSchema.json"

# List of included columns and tables in quoted name
$IncludedColumnsAndTables =  "[SalesLT].[Address].[AddressID]",
                             "[SalesLT].[Address].[AddressLine2]",
                             "[SalesLT].[Address].[rowguid]",
                             "[SalesLT].[Address].[PostalCode]",
                             "[SalesLT].[ProductDescription]"
$MetadataList = [System.Collections.ArrayList]::new($IncludedColumnsAndTables)


add-azurermaccount 
select-azurermsubscription -SubscriptionId $SubscriptionId

# Use this section if it is safe tooshow password in hello script.
# Otherwise, use hello PromptForCredential
# $User = "username"
# $PWord = ConvertTo-SecureString -String "password" -AsPlainText -Force
# $Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $PWord

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$ServerName+".database.windows.net", 
              "", 
              "")

 #Create a new Sync Agent
 Write-Host "Creating new Sync Agent "
 New-AzureRmSqlSyncAgent -ResourceGroupName $ResourceGroupName  `
                               -ServerName  $ServerName  `
                               -SyncDatabaseName $SyncDatabaseName `
                              -SyncAgentName $SyncAgentName


#Generate Agent Key
Write-Host "Generating Agent Key"
$AgentKey = New-AzureRmSqlSyncAgentKey -ResourceGroupName $ResourceGroupName   `
                              -ServerName  $ServerName  `
                              -SyncAgentName $SyncAgentName
 Write-Host "Use your agent key tooconfigure hello sync agent. Do this before proceeding"
$agentkey
                  
#DO hello FOLLOWING BEFORE hello NEXT STEP
#Install hello on-premises sync agent on your machine and register hello sync agent using hello agent key generated above toobring hello sync agent online.
#Add hello SQL server database information including server name, database name, user name, password on hello configuration tool within hello sync agent.  


# Create a new sync group
Write-Host "Creating Sync Group"$SyncGroupName
New-AzureRmSqlSyncGroup   -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -SyncDatabaseName $SyncDatabaseName `
                            -SyncDatabaseServerName $SyncDatabaseServerName `
                            -SyncDatabaseResourceGroupName $SyncDatabaseResourceGroupName `
                            -ConflictResolutionPolicy $ConflictResolutionPolicy `
                            -DatabaseCredential $Credential

# Use this section if it is safe tooshow password in hello script.
#$User = "username"
#$Password = ConvertTo-SecureString -String "password" -AsPlainText -Force
#$Credential = New-Object -TypeName "System.Management.Automation.PSCredential" -ArgumentList $User, $Password

$Credential = $Host.ui.PromptForCredential("Need credential", 
              "Please enter your user name and password for server "+$MemberServerName, 
              "", 
              "")

#Get Information from sync Agent 
#Confirm that your SQL Server was configured
#Note hello Database ID, you will use this as SqlServerDatabaseID for hello next step.
$SyncAgentInfo = Get-AzureRmSqlSyncAgentLinkedDatabase -ResourceGroupName $ResourceGroupName   `
                              -ServerName  $ServerName  `
                              -SyncAgentName $SyncAgentName

# Add a new sync member
Write-Host "Adding member"$SyncMemberName" toohello sync group"



New-AzureRmSqlSyncMember -ResourceGroupName $ResourceGroupName -ServerName $ServerName -DatabaseName $DatabaseName -SyncGroupName $SyncGroupName -Name $SyncMemberName -MemberDatabaseType $MemberDatabaseType -SyncAgentResourceGroupName SyncAgentResourceGroupName -SyncAgentServerName $SyncAgentServerName -SyncAgentName $SyncAgentName  -SyncDirection $SyncDirection -SqlServerDatabaseID  $SyncAgentInfo.DatabaseId

# Refresh database schema from hub database
# Specify hello -SyncMemberName parameter if you want toorefresh schema from hello member database
Write-Host "Refreshing database schema from hub database"
$StartTime= Get-Date
Update-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                              -ServerName $ServerName `
                              -DatabaseName $DatabaseName `
                              -SyncGroupName $SyncGroupName


#Waiting for successful refresh

$StartTime=$StartTime.ToUniversalTime()
$timer=0
$timeout=90
# Check hello log and see if refresh has gone through
Write-Host "Check for successful refresh"
$IsSucceeded = "false"
While ($IsSucceeded -eq "False")
{
    Start-Sleep -s 10
    $timer=$timer+1
    $Details = Get-AzureRmSqlSyncSchema -SyncGroupName $SyncGroupName -ServerName $ServerName -DatabaseName $DatabaseName -ResourceGroupName $ResourceGroupName
    if ($Details.LastUpdateTime -gt $StartTime)
      {
        Write-Host "Refresh was successful"
        $IsSucceeded = $true
      }
    if ($timer -eq $timeout) 
      {
              Write-Host "Refresh timed out"
        break;
      }
}


# Get hello database schema 
Write-Host "Adding tables and columns toohello sync schema"
$databaseSchema = Get-AzureRmSqlSyncSchema   -ResourceGroupName $ResourceGroupName `
                                             -ServerName $ServerName `
                                             -DatabaseName $DatabaseName `
                                             -SyncGroupName $SyncGroupName `

$databaseSchema | ConvertTo-Json -depth 5 -Compress | Out-File "C:\Users\OnPremiseServer\AppData\Local\Temp\syncSchema.json"     
$newSchema = [AzureSqlSyncGroupSchemaModel]::new()
$newSchema.Tables = [List[AzureSqlSyncGroupSchemaTableModel]]::new();

# Add columns and tables toohello sync schema
foreach ($tableSchema in $databaseSchema.Tables)
{
    $newTableSchema = [AzureSqlSyncGroupSchemaTableModel]::new()
    $newTableSchema.QuotedName = $tableSchema.QuotedName
    $newTableSchema.Columns = [List[AzureSqlSyncGroupSchemaColumnModel]]::new();
    $addAllColumns = $false
    if ($MetadataList.Contains($tableSchema.QuotedName))
    {
        if ($tableSchema.HasError)
        {
            $fullTableName = $tableSchema.QuotedName
            Write-Host "Can't add table $fullTableName toohello sync schema" -foregroundcolor "Red"
            Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            continue;
        }
        else
        {
            $addAllColumns = $true
        }
    }
    foreach($columnSchema in $tableSchema.Columns)
    {
        $fullColumnName = $tableSchema.QuotedName + "." + $columnSchema.QuotedName
        if ($addAllColumns -or $MetadataList.Contains($fullColumnName))
        {
            if ((-not $addAllColumns) -and $tableSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName toohello sync schema" -foregroundcolor "Red"
                Write-Host $tableSchema.ErrorId -foregroundcolor "Red"
            }
            elseif ((-not $addAllColumns) -and $columnSchema.HasError)
            {
                Write-Host "Can't add column $fullColumnName toohello sync schema" -foregroundcolor "Red"
                Write-Host $columnSchema.ErrorId -foregroundcolor "Red"
            }
            else
            {
                Write-Host "Adding"$fullColumnName" toohello sync schema"
                $newColumnSchema = [AzureSqlSyncGroupSchemaColumnModel]::new()
                $newColumnSchema.QuotedName = $columnSchema.QuotedName
                $newColumnSchema.DataSize = $columnSchema.DataSize
                $newColumnSchema.DataType = $columnSchema.DataType
                $newTableSchema.Columns.Add($newColumnSchema)
            }
        }
    }
    if ($newTableSchema.Columns.Count -gt 0)
    {
        $newSchema.Tables.Add($newTableSchema)
    }
}

# Convert sync schema tooJson format
$schemaString = $newSchema | ConvertTo-Json -depth 5 -Compress

# workaround a powershell bug
$schemaString = $schemaString.Replace('"Tables"', '"tables"').Replace('"Columns"', '"columns"').Replace('"QuotedName"', '"quotedName"').Replace('"MasterSyncMemberName"','"masterSyncMemberName"')

# Save hello sync schema tooa temp file
$schemaString | Out-File $TempFile

# Update sync schema
Write-Host "Updating hello sync schema"
Update-AzureRmSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                            -ServerName $ServerName `
                            -DatabaseName $DatabaseName `
                            -Name $SyncGroupName `
                            -Schema $TempFile

$SyncLogStartTime = Get-Date

# Trigger sync manually
Write-Host "Trigger sync manually"
Start-AzureRmSqlSyncGroupSync  -ResourceGroupName $ResourceGroupName `
                               -ServerName $ServerName `
                               -DatabaseName $DatabaseName `
                               -SyncGroupName $SyncGroupName

# Check hello sync log and wait until hello first sync succeeded
Write-Host "Check hello sync log"
$IsSucceeded = $false
For ($i = 0; ($i -lt 300) -and (-not $IsSucceeded); $i = $i + 10)
{
    Start-Sleep -s 10
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzureRmSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            if ($SyncLog.Details.Contains("Sync completed successfully"))
            {
                Write-Host $SyncLog.TimeStamp : $SyncLog.Details
                $IsSucceeded = $true
            }
        }
    }
}

if ($IsSucceeded)
{
    # Enable scheduled sync
    Write-Host "Enable hello scheduled sync with 300 seconds interval"
    Update-AzureRmSqlSyncGroup  -ResourceGroupName $ResourceGroupName `
                                -ServerName $ServerName `
                                -DatabaseName $DatabaseName `
                                -Name $SyncGroupName `
                                -IntervalInSeconds $IntervalInSeconds
}
else
{
    # Output all log if sync doesn't succeed in 300 seconds
    $SyncLogEndTime = Get-Date
    $SyncLogList = Get-AzureRmSqlSyncGroupLog  -ResourceGroupName $ResourceGroupName `
                                           -ServerName $ServerName `
                                           -DatabaseName $DatabaseName `
                                           -SyncGroupName $SyncGroupName `
                                           -StartTime $SyncLogStartTime.ToUniversalTime() `
                                           -EndTime $SyncLogEndTime.ToUniversalTime()
    if ($SynclogList.Length -gt 0)
    {
        foreach ($SyncLog in $SyncLogList)
        {
            Write-Host $SyncLog.TimeStamp : $SyncLog.Details
        }
    }
}
```

## <a name="clean-up-deployment"></a><span data-ttu-id="22dfd-110">Rensa distribution</span><span class="sxs-lookup"><span data-stu-id="22dfd-110">Clean up deployment</span></span>

<span data-ttu-id="22dfd-111">När du har kört hello exempelskript du kan köra hello efter kommandot tooremove hello resursgruppen och alla resurser som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="22dfd-111">After you run hello sample script, you can run hello following command tooremove hello resource group and all resources associated with it.</span></span>

```powershell
Remove-AzureRmResourceGroup -ResourceGroupName "myResourceGroup"
```

## <a name="script-explanation"></a><span data-ttu-id="22dfd-112">Skriptet förklaring</span><span class="sxs-lookup"><span data-stu-id="22dfd-112">Script explanation</span></span>

<span data-ttu-id="22dfd-113">Det här skriptet använder hello följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="22dfd-113">This script uses hello following commands.</span></span> <span data-ttu-id="22dfd-114">Varje kommando i hello tabellen länkar toocommand specifika-dokumentationen.</span><span class="sxs-lookup"><span data-stu-id="22dfd-114">Each command in hello table links toocommand-specific documentation.</span></span>

| <span data-ttu-id="22dfd-115">Kommando</span><span class="sxs-lookup"><span data-stu-id="22dfd-115">Command</span></span> | <span data-ttu-id="22dfd-116">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="22dfd-116">Notes</span></span> |
|---|---|
| [<span data-ttu-id="22dfd-117">Ny AzureRmSqlSyncAgent</span><span class="sxs-lookup"><span data-stu-id="22dfd-117">New-AzureRmSqlSyncAgent</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgent) |  <span data-ttu-id="22dfd-118">Skapar en ny Agent för synkronisering</span><span class="sxs-lookup"><span data-stu-id="22dfd-118">Creates a new Sync Agent</span></span> |
| [<span data-ttu-id="22dfd-119">Ny AzureRmSqlSyncAgentKey</span><span class="sxs-lookup"><span data-stu-id="22dfd-119">New-AzureRmSqlSyncAgentKey</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncAgentKey) |  <span data-ttu-id="22dfd-120">Genererar hello agentnyckeln som associeras med hello Sync-agent</span><span class="sxs-lookup"><span data-stu-id="22dfd-120">Generates hello agent key associated with hello Sync agent</span></span> |
| [<span data-ttu-id="22dfd-121">Get-AzureRmSqlSyncAgentLinkedDatabase</span><span class="sxs-lookup"><span data-stu-id="22dfd-121">Get-AzureRmSqlSyncAgentLinkedDatabase</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncAgentLinkedDatabase) |  <span data-ttu-id="22dfd-122">Hämta alla hello-information för hello Sync-Agent</span><span class="sxs-lookup"><span data-stu-id="22dfd-122">Get all hello information for hello Sync Agent</span></span> |
| [<span data-ttu-id="22dfd-123">Ny AzureRmSqlSyncMember</span><span class="sxs-lookup"><span data-stu-id="22dfd-123">New-AzureRmSqlSyncMember</span></span>](/powershell/module/azurerm.sql/New-AzureRmSqlSyncMember) |  <span data-ttu-id="22dfd-124">Lägg till en ny medlem toohello Sync-grupp</span><span class="sxs-lookup"><span data-stu-id="22dfd-124">Add a new member toohello Sync Group</span></span> |
| [<span data-ttu-id="22dfd-125">Uppdatera AzureRmSqlSyncSchema</span><span class="sxs-lookup"><span data-stu-id="22dfd-125">Update-AzureRmSqlSyncSchema</span></span>](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncSchema) |  <span data-ttu-id="22dfd-126">Uppdaterar schemainformation för hello-databas</span><span class="sxs-lookup"><span data-stu-id="22dfd-126">Refreshes hello database schema information</span></span> |
| [<span data-ttu-id="22dfd-127">Get-AzureRmSqlSyncSchema</span><span class="sxs-lookup"><span data-stu-id="22dfd-127">Get-AzureRmSqlSyncSchema</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncSchem) |  <span data-ttu-id="22dfd-128">Hämta schemainformation för hello-databas</span><span class="sxs-lookup"><span data-stu-id="22dfd-128">Get hello database schema information</span></span> |
| [<span data-ttu-id="22dfd-129">Uppdatera AzureRmSqlSyncGroup</span><span class="sxs-lookup"><span data-stu-id="22dfd-129">Update-AzureRmSqlSyncGroup</span></span>](/powershell/module/azurerm.sql/Update-AzureRmSqlSyncGroup) |  <span data-ttu-id="22dfd-130">Uppdateringar hello Sync-grupp</span><span class="sxs-lookup"><span data-stu-id="22dfd-130">Updates hello Sync Group</span></span> |
| [<span data-ttu-id="22dfd-131">Start-AzureRmSqlSyncGroupSync</span><span class="sxs-lookup"><span data-stu-id="22dfd-131">Start-AzureRmSqlSyncGroupSync</span></span>](/powershell/module/azurerm.sql/Start-AzureRmSqlSyncGroupSync) | <span data-ttu-id="22dfd-132">Utlöser en synkronisering</span><span class="sxs-lookup"><span data-stu-id="22dfd-132">Triggers a Sync</span></span> |
| [<span data-ttu-id="22dfd-133">Get-AzureRmSqlSyncGroupLog</span><span class="sxs-lookup"><span data-stu-id="22dfd-133">Get-AzureRmSqlSyncGroupLog</span></span>](/powershell/module/azurerm.sql/Get-AzureRmSqlSyncGroupLog) |  <span data-ttu-id="22dfd-134">Kontrollerar hello Sync-logg</span><span class="sxs-lookup"><span data-stu-id="22dfd-134">Checks hello Sync Log</span></span> |
|||

## <a name="next-steps"></a><span data-ttu-id="22dfd-135">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="22dfd-135">Next steps</span></span>

<span data-ttu-id="22dfd-136">Läs mer om Azure PowerShell [Azure PowerShell dokumentationen](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="22dfd-136">For more information about Azure PowerShell, see [Azure PowerShell documentation](/powershell/azure/overview).</span></span>

<span data-ttu-id="22dfd-137">Ytterligare exempel för SQL Database PowerShell-skript finns i [Azure SQL Database PowerShell-skript](../sql-database-powershell-samples.md).</span><span class="sxs-lookup"><span data-stu-id="22dfd-137">Additional SQL Database PowerShell script samples can be found in [Azure SQL Database PowerShell scripts](../sql-database-powershell-samples.md).</span></span>
