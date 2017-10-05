---
title: "Använda PowerShell för att säkerhetskopiera och återställa Apptjänst-appar"
description: "Lär dig hur du använder PowerShell för att säkerhetskopiera och återställa en app i Azure App Service"
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
ms.openlocfilehash: 34a7e1d025c301ca056753d964bb3c5f4f1a62d8
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a><span data-ttu-id="f1961-103">Använda PowerShell för att säkerhetskopiera och återställa Apptjänst-appar</span><span class="sxs-lookup"><span data-stu-id="f1961-103">Use PowerShell to back up and restore App Service apps</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="f1961-104">PowerShell</span><span class="sxs-lookup"><span data-stu-id="f1961-104">PowerShell</span></span>](app-service-powershell-backup.md)
> * [<span data-ttu-id="f1961-105">REST-API</span><span class="sxs-lookup"><span data-stu-id="f1961-105">REST API</span></span>](../app-service-web/websites-csm-backup.md)
> 
> 

<span data-ttu-id="f1961-106">Lär dig hur du använder Azure PowerShell för att säkerhetskopiera och återställa [Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/).</span><span class="sxs-lookup"><span data-stu-id="f1961-106">Learn how to use Azure PowerShell to back up and restore [App Service apps](https://azure.microsoft.com/services/app-service/web/).</span></span> <span data-ttu-id="f1961-107">Läs mer om web app säkerhetskopior, inklusive krav och begränsningar, [säkerhetskopiera en webbapp i Azure App Service](../app-service-web/web-sites-backup.md).</span><span class="sxs-lookup"><span data-stu-id="f1961-107">For more information about web app backups, including requirements and restrictions, see [Back up a web app in Azure App Service](../app-service-web/web-sites-backup.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f1961-108">Krav</span><span class="sxs-lookup"><span data-stu-id="f1961-108">Prerequisites</span></span>
<span data-ttu-id="f1961-109">Om du vill använda PowerShell för att hantera säkerhetskopiorna app, behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="f1961-109">To use PowerShell to manage your app backups, you need the following:</span></span>

* <span data-ttu-id="f1961-110">**En SAS-URL** som tillåter Läs- och skrivbehörighet till en Azure Storage-behållare.</span><span class="sxs-lookup"><span data-stu-id="f1961-110">**A SAS URL** that allows read and write access to an Azure Storage container.</span></span> <span data-ttu-id="f1961-111">Se [förstå SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md) för en förklaring av SAS-URL: er.</span><span class="sxs-lookup"><span data-stu-id="f1961-111">See [Understanding the SAS model](../storage/common/storage-dotnet-shared-access-signature-part-1.md) for an explanation of SAS URLs.</span></span> <span data-ttu-id="f1961-112">Se [med hjälp av Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md) exempel för att hantera Azure Storage med hjälp av PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f1961-112">See [Using Azure PowerShell with Azure Storage](../storage/common/storage-powershell-guide-full.md) for examples of managing Azure Storage using PowerShell.</span></span>
* <span data-ttu-id="f1961-113">**En databasanslutningssträng** om du vill säkerhetskopiera en databas tillsammans med ditt webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f1961-113">**A database connection string** if you want to back up a database along with your web app.</span></span>

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a><span data-ttu-id="f1961-114">Hur du skapar en SAS-URL som ska användas med web app backup-cmdletar</span><span class="sxs-lookup"><span data-stu-id="f1961-114">How to generate a SAS URL to use with the web app backup cmdlets</span></span>
<span data-ttu-id="f1961-115">En SAS-URL kan genereras med PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f1961-115">A SAS URL can be generated with PowerShell.</span></span> <span data-ttu-id="f1961-116">Här är ett exempel på hur du skapar en som kan användas med de cmdlets som beskrivs i den här artikeln.</span><span class="sxs-lookup"><span data-stu-id="f1961-116">Here is an example of how to generate one that can be used with the cmdlets discussed in this article.</span></span>

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a><span data-ttu-id="f1961-117">Installera Azure PowerShell 1.3.2 eller större</span><span class="sxs-lookup"><span data-stu-id="f1961-117">Install Azure PowerShell 1.3.2 or greater</span></span>
<span data-ttu-id="f1961-118">Se [med hjälp av Azure PowerShell med Azure Resource Manager](/powershell/azure/overview) anvisningar för att installera och använda Azure PowerShell.</span><span class="sxs-lookup"><span data-stu-id="f1961-118">See [Using Azure PowerShell with Azure Resource Manager](/powershell/azure/overview) for instructions on installing and using Azure PowerShell.</span></span>

## <a name="create-a-backup"></a><span data-ttu-id="f1961-119">Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f1961-119">Create a backup</span></span>
<span data-ttu-id="f1961-120">Använd cmdlet New-AzureRmWebAppBackup för att skapa en säkerhetskopia av ett webbprogram.</span><span class="sxs-lookup"><span data-stu-id="f1961-120">Use the New-AzureRmWebAppBackup cmdlet to create a backup of a web app.</span></span>

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

<span data-ttu-id="f1961-121">Detta skapar en säkerhetskopia med ett automatiskt genererat namn.</span><span class="sxs-lookup"><span data-stu-id="f1961-121">This creates a backup with an automatically generated name.</span></span> <span data-ttu-id="f1961-122">Använd parametern säkerhetskopia om du vill ange ett namn för säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="f1961-122">If you would like to provide a name for your backup, use the BackupName optional parameter.</span></span>

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

<span data-ttu-id="f1961-123">För att inkludera en databas i säkerhetskopieringen, först skapa en säkerhetskopiering inställning för databasen med hjälp av cmdlet New-AzureRmWebAppDatabaseBackupSetting, och sedan ange den här inställningen i parametern databaser för cmdleten New-AzureRmWebAppBackup.</span><span class="sxs-lookup"><span data-stu-id="f1961-123">To include a database in the backup, first create a database backup setting using the New-AzureRmWebAppDatabaseBackupSetting cmdlet, then supply that setting in the Databases parameter of the New-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="f1961-124">Parametern databaser accepterar en matris med databasinställningarna, så att du kan säkerhetskopiera mer än en databas.</span><span class="sxs-lookup"><span data-stu-id="f1961-124">The Databases parameter accepts an array of database settings, allowing you to back up more than one database.</span></span>

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a><span data-ttu-id="f1961-125">Hämta säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="f1961-125">Get backups</span></span>
<span data-ttu-id="f1961-126">Get-AzureRmWebAppBackupList cmdlet returnerar en matris med alla säkerhetskopior för en webbapp.</span><span class="sxs-lookup"><span data-stu-id="f1961-126">The Get-AzureRmWebAppBackupList cmdlet returns an array of all backups for a web app.</span></span> <span data-ttu-id="f1961-127">Du måste ange namnet på webbprogrammet och dess resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="f1961-127">You must supply the name of the web app and its resource group.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

<span data-ttu-id="f1961-128">Använd cmdleten Get-AzureRmWebAppBackup för att få en säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="f1961-128">To get a specific backup, use the Get-AzureRmWebAppBackup cmdlet.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

<span data-ttu-id="f1961-129">Också kan du skicka ett web app-objekt till säkerhetskopiering management-cmdlets i informationssyfte.</span><span class="sxs-lookup"><span data-stu-id="f1961-129">You can also pipe a web app object into any of the backup management cmdlets for convenience.</span></span>

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a><span data-ttu-id="f1961-130">Schemalägga automatisk säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="f1961-130">Schedule automatic backups</span></span>
<span data-ttu-id="f1961-131">Du kan också schemalägga säkerhetskopieringar sker automatiskt vid ett angivet intervall.</span><span class="sxs-lookup"><span data-stu-id="f1961-131">You can schedule backups to happen automatically at a specified interval.</span></span> <span data-ttu-id="f1961-132">Använd cmdleten redigera AzureRmWebAppBackupConfiguration om du vill konfigurera ett schema för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f1961-132">To configure a backup schedule, use the Edit-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="f1961-133">Denna cmdlet har flera parametrar:</span><span class="sxs-lookup"><span data-stu-id="f1961-133">This cmdlet takes several parameters:</span></span>

* <span data-ttu-id="f1961-134">**Namnet** -namnet på webbprogrammet.</span><span class="sxs-lookup"><span data-stu-id="f1961-134">**Name** - The name of the web app.</span></span>
* <span data-ttu-id="f1961-135">**ResourceGroupName** -namnet på resursgruppen som innehåller webbappen.</span><span class="sxs-lookup"><span data-stu-id="f1961-135">**ResourceGroupName** - The name of the resource group containing the web app.</span></span>
* <span data-ttu-id="f1961-136">**Fack** – valfritt.</span><span class="sxs-lookup"><span data-stu-id="f1961-136">**Slot** - Optional.</span></span> <span data-ttu-id="f1961-137">Namnet på webbprogramplats.</span><span class="sxs-lookup"><span data-stu-id="f1961-137">The name of the web app slot.</span></span>
* <span data-ttu-id="f1961-138">**StorageAccountUrl** -SAS URL: en för Azure Storage-behållare som används för att lagra säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="f1961-138">**StorageAccountUrl** - The SAS URL for the Azure Storage container used to store the backups.</span></span>
* <span data-ttu-id="f1961-139">**FrequencyInterval** -numeriskt värde för hur ofta säkerhetskopieringar ska göras.</span><span class="sxs-lookup"><span data-stu-id="f1961-139">**FrequencyInterval** - Numeric value for how often the backups should be made.</span></span> <span data-ttu-id="f1961-140">Måste vara ett positivt heltal.</span><span class="sxs-lookup"><span data-stu-id="f1961-140">Must be a positive integer.</span></span>
* <span data-ttu-id="f1961-141">**FrequencyUnit** -tidsenhet för hur ofta säkerhetskopieringar ska göras.</span><span class="sxs-lookup"><span data-stu-id="f1961-141">**FrequencyUnit** - Unit of time for how often the backups should be made.</span></span> <span data-ttu-id="f1961-142">Alternativen är timme och dag.</span><span class="sxs-lookup"><span data-stu-id="f1961-142">Options are Hour and Day.</span></span>
* <span data-ttu-id="f1961-143">**RetentionPeriodInDays** - hur många dagar som automatisk säkerhetskopiering ska sparas innan de tas bort automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f1961-143">**RetentionPeriodInDays** - How many days the automatic backups should be saved before being automatically deleted.</span></span>
* <span data-ttu-id="f1961-144">**StartTime** – valfritt.</span><span class="sxs-lookup"><span data-stu-id="f1961-144">**StartTime** - Optional.</span></span> <span data-ttu-id="f1961-145">Den tid när de automatiska säkerhetskopieringarna ska starta.</span><span class="sxs-lookup"><span data-stu-id="f1961-145">The time when the automatic backups should begin.</span></span> <span data-ttu-id="f1961-146">Säkerhetskopieringar börjar omedelbart om detta är null.</span><span class="sxs-lookup"><span data-stu-id="f1961-146">Backups begin immediately if this is null.</span></span> <span data-ttu-id="f1961-147">Måste vara ett datetime-värde.</span><span class="sxs-lookup"><span data-stu-id="f1961-147">Must be a DateTime.</span></span>
* <span data-ttu-id="f1961-148">**Databaser** – valfritt.</span><span class="sxs-lookup"><span data-stu-id="f1961-148">**Databases** - Optional.</span></span> <span data-ttu-id="f1961-149">En matris med DatabaseBackupSettings för databaser för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f1961-149">An array of DatabaseBackupSettings for the databases to backup.</span></span>
* <span data-ttu-id="f1961-150">**KeepAtLeastOneBackup** – valfritt växlade parametern.</span><span class="sxs-lookup"><span data-stu-id="f1961-150">**KeepAtLeastOneBackup** - Optional switched parameter.</span></span> <span data-ttu-id="f1961-151">Ange om en säkerhetskopia alltid ska behållas i lagringskontot, oavsett hur gammal den är.</span><span class="sxs-lookup"><span data-stu-id="f1961-151">Supply this if one backup should always be kept in the storage account, regardless of how old it is.</span></span>

<span data-ttu-id="f1961-152">Följande är ett exempel på hur du använder denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f1961-152">Following is an example of how to use this cmdlet.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

<span data-ttu-id="f1961-153">Använd cmdleten Get-AzureRmWebAppBackupConfiguration för att få det aktuella schemat för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="f1961-153">To get the current backup schedule, use the Get-AzureRmWebAppBackupConfiguration cmdlet.</span></span> <span data-ttu-id="f1961-154">Detta kan vara användbart för att ändra ett schema som redan har konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="f1961-154">This can be useful for modifying a schedule that has already been configured.</span></span>

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a><span data-ttu-id="f1961-155">Återställa en webbapp från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f1961-155">Restore a web app from a backup</span></span>
<span data-ttu-id="f1961-156">Använd cmdleten återställning AzureRmWebAppBackup om du vill återställa en webbapp från en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f1961-156">To restore a web app from a backup, use the Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="f1961-157">Det enklaste sättet att använda denna cmdlet är att pipe i en säkerhetskopieobjektet hämtas från cmdlet Get-AzureRmWebAppBackup eller Get-AzureRmWebAppBackupList cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f1961-157">The easiest way to use this cmdlet is to pipe in a backup object retrieved from the Get-AzureRmWebAppBackup cmdlet or Get-AzureRmWebAppBackupList cmdlet.</span></span>

<span data-ttu-id="f1961-158">När du har en säkerhetskopieobjektet kan skicka du det till återställning AzureRmWebAppBackup cmdlet.</span><span class="sxs-lookup"><span data-stu-id="f1961-158">Once you have a backup object, you can pipe it into the Restore-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="f1961-159">Ange parametern överskrivning växel för att ange att du vill skriva över innehållet i ditt webbprogram med innehållet i säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="f1961-159">Specify the Overwrite switch parameter to indicate that you intend to overwrite the contents of your web app with the contents of the backup.</span></span> <span data-ttu-id="f1961-160">Om säkerhetskopian innehåller databaser, återställs även dessa databaser.</span><span class="sxs-lookup"><span data-stu-id="f1961-160">If the backup contains databases, those databases are restored as well.</span></span>

        $backup | Restore-AzureRmWebAppBackup -Overwrite

<span data-ttu-id="f1961-161">Följande är ett exempel på hur du använder återställning-AzureRmWebAppBackup genom att ange alla parametrar.</span><span class="sxs-lookup"><span data-stu-id="f1961-161">Following is an example of how to use the Restore-AzureRmWebAppBackup by specifying all the parameters.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a><span data-ttu-id="f1961-162">Ta bort en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="f1961-162">Delete a backup</span></span>
<span data-ttu-id="f1961-163">Använd cmdleten Remove-AzureRmWebAppBackup om du vill ta bort en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="f1961-163">To delete a backup, use the Remove-AzureRmWebAppBackup cmdlet.</span></span> <span data-ttu-id="f1961-164">Detta tar bort säkerhetskopian från ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="f1961-164">This removes the backup from your storage account.</span></span> <span data-ttu-id="f1961-165">Ange appens namn och dess resursgruppen ID på den säkerhetskopia som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f1961-165">Specify your app name, its resource group, and the ID of the backup you want to delete.</span></span>

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

<span data-ttu-id="f1961-166">Dessutom kan du skicka en säkerhetskopieobjektet till cmdleten Remove-AzureRmWebAppBackup att ta bort den.</span><span class="sxs-lookup"><span data-stu-id="f1961-166">You can also pipe a backup object into the Remove-AzureRmWebAppBackup cmdlet to delete it.</span></span>

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
