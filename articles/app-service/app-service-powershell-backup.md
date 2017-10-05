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
# <a name="use-powershell-to-back-up-and-restore-app-service-apps"></a>Använda PowerShell för att säkerhetskopiera och återställa Apptjänst-appar
> [!div class="op_single_selector"]
> * [PowerShell](app-service-powershell-backup.md)
> * [REST-API](../app-service-web/websites-csm-backup.md)
> 
> 

Lär dig hur du använder Azure PowerShell för att säkerhetskopiera och återställa [Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/). Läs mer om web app säkerhetskopior, inklusive krav och begränsningar, [säkerhetskopiera en webbapp i Azure App Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Krav
Om du vill använda PowerShell för att hantera säkerhetskopiorna app, behöver du följande:

* **En SAS-URL** som tillåter Läs- och skrivbehörighet till en Azure Storage-behållare. Se [förstå SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md) för en förklaring av SAS-URL: er. Se [med hjälp av Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md) exempel för att hantera Azure Storage med hjälp av PowerShell.
* **En databasanslutningssträng** om du vill säkerhetskopiera en databas tillsammans med ditt webbprogram.

### <a name="how-to-generate-a-sas-url-to-use-with-the-web-app-backup-cmdlets"></a>Hur du skapar en SAS-URL som ska användas med web app backup-cmdletar
En SAS-URL kan genereras med PowerShell. Här är ett exempel på hur du skapar en som kan användas med de cmdlets som beskrivs i den här artikeln.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure to select the appropriate key. Here we select the first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Installera Azure PowerShell 1.3.2 eller större
Se [med hjälp av Azure PowerShell med Azure Resource Manager](/powershell/azure/overview) anvisningar för att installera och använda Azure PowerShell.

## <a name="create-a-backup"></a>Skapa en säkerhetskopia
Använd cmdlet New-AzureRmWebAppBackup för att skapa en säkerhetskopia av ett webbprogram.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Detta skapar en säkerhetskopia med ett automatiskt genererat namn. Använd parametern säkerhetskopia om du vill ange ett namn för säkerhetskopian.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

För att inkludera en databas i säkerhetskopieringen, först skapa en säkerhetskopiering inställning för databasen med hjälp av cmdlet New-AzureRmWebAppDatabaseBackupSetting, och sedan ange den här inställningen i parametern databaser för cmdleten New-AzureRmWebAppBackup. Parametern databaser accepterar en matris med databasinställningarna, så att du kan säkerhetskopiera mer än en databas.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Hämta säkerhetskopieringar
Get-AzureRmWebAppBackupList cmdlet returnerar en matris med alla säkerhetskopior för en webbapp. Du måste ange namnet på webbprogrammet och dess resursgruppen.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

Använd cmdleten Get-AzureRmWebAppBackup för att få en säkerhetskopieringen.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Också kan du skicka ett web app-objekt till säkerhetskopiering management-cmdlets i informationssyfte.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Schemalägga automatisk säkerhetskopiering
Du kan också schemalägga säkerhetskopieringar sker automatiskt vid ett angivet intervall. Använd cmdleten redigera AzureRmWebAppBackupConfiguration om du vill konfigurera ett schema för säkerhetskopiering. Denna cmdlet har flera parametrar:

* **Namnet** -namnet på webbprogrammet.
* **ResourceGroupName** -namnet på resursgruppen som innehåller webbappen.
* **Fack** – valfritt. Namnet på webbprogramplats.
* **StorageAccountUrl** -SAS URL: en för Azure Storage-behållare som används för att lagra säkerhetskopior.
* **FrequencyInterval** -numeriskt värde för hur ofta säkerhetskopieringar ska göras. Måste vara ett positivt heltal.
* **FrequencyUnit** -tidsenhet för hur ofta säkerhetskopieringar ska göras. Alternativen är timme och dag.
* **RetentionPeriodInDays** - hur många dagar som automatisk säkerhetskopiering ska sparas innan de tas bort automatiskt.
* **StartTime** – valfritt. Den tid när de automatiska säkerhetskopieringarna ska starta. Säkerhetskopieringar börjar omedelbart om detta är null. Måste vara ett datetime-värde.
* **Databaser** – valfritt. En matris med DatabaseBackupSettings för databaser för säkerhetskopiering.
* **KeepAtLeastOneBackup** – valfritt växlade parametern. Ange om en säkerhetskopia alltid ska behållas i lagringskontot, oavsett hur gammal den är.

Följande är ett exempel på hur du använder denna cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

Använd cmdleten Get-AzureRmWebAppBackupConfiguration för att få det aktuella schemat för säkerhetskopiering. Detta kan vara användbart för att ändra ett schema som redan har konfigurerats.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify the configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply the new configuration by piping it into the Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Återställa en webbapp från en säkerhetskopia
Använd cmdleten återställning AzureRmWebAppBackup om du vill återställa en webbapp från en säkerhetskopia. Det enklaste sättet att använda denna cmdlet är att pipe i en säkerhetskopieobjektet hämtas från cmdlet Get-AzureRmWebAppBackup eller Get-AzureRmWebAppBackupList cmdlet.

När du har en säkerhetskopieobjektet kan skicka du det till återställning AzureRmWebAppBackup cmdlet. Ange parametern överskrivning växel för att ange att du vill skriva över innehållet i ditt webbprogram med innehållet i säkerhetskopian. Om säkerhetskopian innehåller databaser, återställs även dessa databaser.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Följande är ett exempel på hur du använder återställning-AzureRmWebAppBackup genom att ange alla parametrar.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Ta bort en säkerhetskopia
Använd cmdleten Remove-AzureRmWebAppBackup om du vill ta bort en säkerhetskopia. Detta tar bort säkerhetskopian från ditt lagringskonto. Ange appens namn och dess resursgruppen ID på den säkerhetskopia som du vill ta bort.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Dessutom kan du skicka en säkerhetskopieobjektet till cmdleten Remove-AzureRmWebAppBackup att ta bort den.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
