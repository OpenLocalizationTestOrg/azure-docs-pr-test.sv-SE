---
title: "aaaUse PowerShell tooback upp och återställa Apptjänst-appar"
description: "Lär dig hur toouse PowerShell tooback upp och återställa en app i Azure App Service"
services: app-service
documentationcenter: 
author: NKing92
manager: erikre
editor: 
ms.assetid: 7ea8661e-aefb-4823-9626-6bff980cdebf
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2016
ms.author: nicking
ms.openlocfilehash: 4042166f6c650841926f010056d6c80ab2de57e3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-powershell-tooback-up-and-restore-app-service-apps"></a><span data-ttu-id="6e5fc-103">Använd PowerShell tooback och återställa Apptjänst-appar</span><span class="sxs-lookup"><span data-stu-id="6e5fc-103">Use PowerShell tooback up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="6e5fc-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="6e5fc-104">PowerShell</span></span>](app-service-powershell-backup.md)
> * [<span data-ttu-id="6e5fc-105">REST-API</span><span class="sxs-lookup"><span data-stu-id="6e5fc-105">REST API</span></span>](../app-service-web/websites-csm-backup.md)
> 
> 

<span data-ttu-id="6e5fc-106">Lär dig hur toouse Azure PowerShell tooback upp och återställa [Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="6e5fc-106">Learn how toouse Azure PowerShell tooback up and restore [App Service apps](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="6e5fc-107">Läs mer om web app säkerhetskopior, inklusive krav och begränsningar, [säkerhetskopiera en webbapp i Azure App Service](../app-service-web/web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="6e5fc-107">For more information about web app backups, including requirements and restrictions, see [Back up a web app in Azure App Service](../app-service-web/web-sites-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="6e5fc-108">Krav</span><span class="sxs-lookup"><span data-stu-id="6e5fc-108">Prerequisites</span></span>
<span data-ttu-id="6e5fc-109">toouse PowerShell toomanage säkerhetskopiorna app behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="6e5fc-109">toouse PowerShell toomanage your app backups, you need hello following:</span></span>

* <span data-ttu-id="6e5fc-110">**En SAS-URL** som tillåter Läs- och skrivbehörighet tooan Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-110">**A SAS URL** that allows read and write access tooan Azure Storage container.</span></span> <span data-ttu-id="6e5fc-111">Se [förstå hello SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md) för en förklaring av SAS-URL: er.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-111">See [Understanding hello SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for an explanation of SAS URLs.</span></span> <span data-ttu-id="6e5fc-112">Se [med hjälp av Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md) exempel för att hantera Azure Storage med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-112">See [Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) for examples of managing Azure Storage using PowerShell.</span></span>
* <span data-ttu-id="6e5fc-113">**En databasanslutningssträng** om du vill tooback upp en databas tillsammans med ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-113">**A database connection string** if you want tooback up a database along with your web app.</span></span>

### <a name="how-toogenerate-a-sas-url-toouse-with-hello-web-app-backup-cmdlets"></a><span data-ttu-id="6e5fc-114">Hur toogenerate en SAS-URL toouse med hello webbprogrammet säkerhetskopiera cmdlets</span><span class="sxs-lookup"><span data-stu-id="6e5fc-114">How toogenerate a SAS URL toouse with hello web app backup cmdlets</span></span>
<span data-ttu-id="6e5fc-115">En SAS-URL kan genereras med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-115">A SAS URL can be generated with PowerShell.</span></span> <span data-ttu-id="6e5fc-116">Här är ett exempel på hur toogenerate som kan användas med hello cmdlets beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-116">Here is an example of how toogenerate one that can be used with hello cmdlets discussed in this article.</span></span>

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure tooselect hello appropriate key. Here we select hello first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a><span data-ttu-id="6e5fc-117">Installera Azure PowerShell 1.3.2 eller större</span><span class="sxs-lookup"><span data-stu-id="6e5fc-117">Install Azure PowerShell 1.3.2 or greater</span></span>
<span data-ttu-id="6e5fc-118">Se [med hjälp av Azure PowerShell med Azure Resource Manager](/powershell/azure/overview) anvisningar för att installera och använda Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-118">See [Using Azure PowerShell with Azure Resource Manager](/powershell/azure/overview) for instructions on installing and using Azure PowerShell.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="6e5fc-119">Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6e5fc-119">Create a backup</span></span>
<span data-ttu-id="6e5fc-120">Använd hello ny AzureRmWebAppBackup cmdlet toocreate en säkerhetskopia av ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-120">Use hello New-AzureRmWebAppBackup cmdlet toocreate a backup of a web app.</span></span>

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

<span data-ttu-id="6e5fc-121">Detta skapar en säkerhetskopia med ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-121">This creates a backup with an automatically generated name.</span></span> <span data-ttu-id="6e5fc-122">Om du vill tooprovide ett namn för säkerhetskopian kan du använda valfri parameter för hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-122">If you would like tooprovide a name for your backup, use hello BackupName optional parameter.</span></span>

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

<span data-ttu-id="6e5fc-123">tooinclude en databas i hello säkerhetskopiering, först skapa en databas inställningen för säkerhetskopiering med hello ny AzureRmWebAppDatabaseBackupSetting cmdlet och sedan ange att det i hello databaser parametern för hello-cmdlet New-AzureRmWebAppBackup.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-123">tooinclude a database in hello backup, first create a database backup setting using hello New-AzureRmWebAppDatabaseBackupSetting cmdlet, then supply that setting in hello Databases parameter of hello New-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6e5fc-124">hello databaser parametern accepterar en matris med databasinställningarna, så att du tooback in mer än en databas.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-124">hello Databases parameter accepts an array of database settings, allowing you tooback up more than one database.</span></span>

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a><span data-ttu-id="6e5fc-125">Hämta säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="6e5fc-125">Get backups</span></span>
<span data-ttu-id="6e5fc-126">hello Get-AzureRmWebAppBackupList cmdlet returnerar en matris med alla säkerhetskopior för en webbapp.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-126">hello Get-AzureRmWebAppBackupList cmdlet returns an array of all backups for a web app.</span></span> <span data-ttu-id="6e5fc-127">Du måste ange hello namnet på webbprogrammet hello och dess resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-127">You must supply hello name of hello web app and its resource group.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

<span data-ttu-id="6e5fc-128">tooget en specifik säkerhetskopiering, Använd hello Get-AzureRmWebAppBackup cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-128">tooget a specific backup, use hello Get-AzureRmWebAppBackup cmdlet.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

<span data-ttu-id="6e5fc-129">Också kan du skicka ett web app-objekt till hello säkerhetskopiering management-cmdlets i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-129">You can also pipe a web app object into any of hello backup management cmdlets for convenience.</span></span>

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a><span data-ttu-id="6e5fc-130">Schemalägga automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="6e5fc-130">Schedule automatic backups</span></span>
<span data-ttu-id="6e5fc-131">Du kan schemalägga säkerhetskopieringar toohappen automatiskt vid ett angivet intervall.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-131">You can schedule backups toohappen automatically at a specified interval.</span></span> <span data-ttu-id="6e5fc-132">tooconfigure ett schema för säkerhetskopiering, Använd hello redigera AzureRmWebAppBackupConfiguration cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-132">tooconfigure a backup schedule, use hello Edit-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="6e5fc-133">Denna cmdlet har flera parametrar:</span><span class="sxs-lookup"><span data-stu-id="6e5fc-133">This cmdlet takes several parameters:</span></span>

* <span data-ttu-id="6e5fc-134">**Namnet** - hello namn på hello webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-134">**Name** - hello name of hello web app.</span></span>
* <span data-ttu-id="6e5fc-135">**ResourceGroupName** - hello namn på hello resurs grupp som innehåller hello webb-app.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-135">**ResourceGroupName** - hello name of hello resource group containing hello web app.</span></span>
* <span data-ttu-id="6e5fc-136">**Fack** – valfritt.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-136">**Slot** - Optional.</span></span> <span data-ttu-id="6e5fc-137">hello namnet på hello webbprogramplats.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-137">hello name of hello web app slot.</span></span>
* <span data-ttu-id="6e5fc-138">**StorageAccountUrl** -hello SAS-URL för hello Azure Storage-behållare används toostore hello säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-138">**StorageAccountUrl** - hello SAS URL for hello Azure Storage container used toostore hello backups.</span></span>
* <span data-ttu-id="6e5fc-139">**FrequencyInterval** -numeriskt värde för hur ofta hello säkerhetskopieringar ska göras.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-139">**FrequencyInterval** - Numeric value for how often hello backups should be made.</span></span> <span data-ttu-id="6e5fc-140">Måste vara ett positivt heltal.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-140">Must be a positive integer.</span></span>
* <span data-ttu-id="6e5fc-141">**FrequencyUnit** -tidsenhet för hur ofta hello säkerhetskopieringar ska göras.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-141">**FrequencyUnit** - Unit of time for how often hello backups should be made.</span></span> <span data-ttu-id="6e5fc-142">Alternativen är timme och dag.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-142">Options are Hour and Day.</span></span>
* <span data-ttu-id="6e5fc-143">**RetentionPeriodInDays** - hur många dagar hello automatisk säkerhetskopiering ska sparas innan de tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-143">**RetentionPeriodInDays** - How many days hello automatic backups should be saved before being automatically deleted.</span></span>
* <span data-ttu-id="6e5fc-144">**StartTime** – valfritt.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-144">**StartTime** - Optional.</span></span> <span data-ttu-id="6e5fc-145">hello tid när hello automatisk säkerhetskopiering ska starta.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-145">hello time when hello automatic backups should begin.</span></span> <span data-ttu-id="6e5fc-146">Säkerhetskopieringar börjar omedelbart om detta är null.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-146">Backups begin immediately if this is null.</span></span> <span data-ttu-id="6e5fc-147">Måste vara ett datetime-värde.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-147">Must be a DateTime.</span></span>
* <span data-ttu-id="6e5fc-148">**Databaser** – valfritt.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-148">**Databases** - Optional.</span></span> <span data-ttu-id="6e5fc-149">En matris med DatabaseBackupSettings för hello databaser toobackup.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-149">An array of DatabaseBackupSettings for hello databases toobackup.</span></span>
* <span data-ttu-id="6e5fc-150">**KeepAtLeastOneBackup** – valfritt växlade parametern.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-150">**KeepAtLeastOneBackup** - Optional switched parameter.</span></span> <span data-ttu-id="6e5fc-151">Ange detta om en säkerhetskopia alltid ska behållas i hello lagringskonto, oavsett hur gammal den är.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-151">Supply this if one backup should always be kept in hello storage account, regardless of how old it is.</span></span>

<span data-ttu-id="6e5fc-152">Följande är ett exempel på hur toouse denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-152">Following is an example of how toouse this cmdlet.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

<span data-ttu-id="6e5fc-153">tooget hello aktuella schemat för säkerhetskopiering, Använd hello Get-AzureRmWebAppBackupConfiguration cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-153">tooget hello current backup schedule, use hello Get-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="6e5fc-154">Detta kan vara användbart för att ändra ett schema som redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-154">This can be useful for modifying a schedule that has already been configured.</span></span>

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify hello configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply hello new configuration by piping it into hello Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a><span data-ttu-id="6e5fc-155">Återställa en webbapp från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6e5fc-155">Restore a web app from a backup</span></span>
<span data-ttu-id="6e5fc-156">toorestore en webbapp från en säkerhetskopia, Använd hello återställning AzureRmWebAppBackup cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-156">toorestore a web app from a backup, use hello Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6e5fc-157">hello enklaste sättet toouse denna cmdlet är toopipe i en säkerhetskopiering objekt som har hämtats från hello Get-AzureRmWebAppBackup cmdlet eller Get-AzureRmWebAppBackupList cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-157">hello easiest way toouse this cmdlet is toopipe in a backup object retrieved from hello Get-AzureRmWebAppBackup cmdlet or Get-AzureRmWebAppBackupList cmdlet.</span></span>

<span data-ttu-id="6e5fc-158">När du har en säkerhetskopieobjektet kan skicka du det till hello återställning AzureRmWebAppBackup cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-158">Once you have a backup object, you can pipe it into hello Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6e5fc-159">Ange hello Skriv växeln parametern tooindicate du avser toooverwrite hello innehållet i ditt webbprogram med hello innehållet i hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-159">Specify hello Overwrite switch parameter tooindicate that you intend toooverwrite hello contents of your web app with hello contents of hello backup.</span></span> <span data-ttu-id="6e5fc-160">Om hello säkerhetskopian innehåller databaser, återställs även dessa databaser.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-160">If hello backup contains databases, those databases are restored as well.</span></span>

        $backup | Restore-AzureRmWebAppBackup -Overwrite

<span data-ttu-id="6e5fc-161">Följande är ett exempel på hur toouse hello återställning AzureRmWebAppBackup genom att ange alla hello-parametrar.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-161">Following is an example of how toouse hello Restore-AzureRmWebAppBackup by specifying all hello parameters.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a><span data-ttu-id="6e5fc-162">Ta bort en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="6e5fc-162">Delete a backup</span></span>
<span data-ttu-id="6e5fc-163">toodelete en säkerhetskopia, Använd hello Remove-AzureRmWebAppBackup cmdlet.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-163">toodelete a backup, use hello Remove-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="6e5fc-164">Detta tar bort hello säkerhetskopiering från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-164">This removes hello backup from your storage account.</span></span> <span data-ttu-id="6e5fc-165">Ange ditt appnamn, dess resursgruppen och hello-ID för hello säkerhetskopiering önskade toodelete.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-165">Specify your app name, its resource group, and hello ID of hello backup you want toodelete.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

<span data-ttu-id="6e5fc-166">Också kan du skicka en säkerhetskopieobjektet till hello Remove-AzureRmWebAppBackup cmdlet toodelete den.</span><span class="sxs-lookup"><span data-stu-id="6e5fc-166">You can also pipe a backup object into hello Remove-AzureRmWebAppBackup cmdlet toodelete it.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
