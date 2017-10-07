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
# <a name="use-powershell-tooback-up-and-restore-app-service-apps"></a>Använd PowerShell tooback och återställa Apptjänst-appar
> [!div class="op_single_selector"]
> * [PowerShell](app-service-powershell-backup.md)
> * [REST-API](../app-service-web/websites-csm-backup.md)
> 
> 

Lär dig hur toouse Azure PowerShell tooback upp och återställa [Apptjänst-appar](https://azure.microsoft.com/services/app-service/web/). Läs mer om web app säkerhetskopior, inklusive krav och begränsningar, [säkerhetskopiera en webbapp i Azure App Service](../app-service-web/web-sites-backup.md).

## <a name="prerequisites"></a>Krav
toouse PowerShell toomanage säkerhetskopiorna app behöver du hello följande:

* **En SAS-URL** som tillåter Läs- och skrivbehörighet tooan Azure Storage-behållare. Se [förstå hello SAS-modellen](../storage/common/storage-dotnet-shared-access-signature-part-1.md) för en förklaring av SAS-URL: er. Se [med hjälp av Azure PowerShell med Azure Storage](../storage/common/storage-powershell-guide-full.md) exempel för att hantera Azure Storage med hjälp av PowerShell.
* **En databasanslutningssträng** om du vill tooback upp en databas tillsammans med ditt webbprogram.

### <a name="how-toogenerate-a-sas-url-toouse-with-hello-web-app-backup-cmdlets"></a>Hur toogenerate en SAS-URL toouse med hello webbprogrammet säkerhetskopiera cmdlets
En SAS-URL kan genereras med PowerShell. Här är ett exempel på hur toogenerate som kan användas med hello cmdlets beskrivs i den här artikeln.

        $storageAccountName = "<your storage account's name>"
        $storageAccountRg = "<your storage account's resource group>"

        # This returns an array of keys for your storage account. Be sure tooselect hello appropriate key. Here we select hello first key as a default.
        $storageAccountKey = Get-AzureRmStorageAccountKey -ResourceGroupName $storageAccountRg -Name $storageAccountName
        $context = New-AzureStorageContext -StorageAccountName $storageAccountName -StorageAccountKey $storageAccountKey[0].Value

        $blobContainerName = "<name of blob container for app backups>"
        $sasUrl = New-AzureStorageContainerSASToken -Name $blobContainerName -Permission rwdl -Context $context -ExpiryTime (Get-Date).AddMonths(1) -FullUri

## <a name="install-azure-powershell-132-or-greater"></a>Installera Azure PowerShell 1.3.2 eller större
Se [med hjälp av Azure PowerShell med Azure Resource Manager](/powershell/azure/overview) anvisningar för att installera och använda Azure PowerShell.

## <a name="create-a-backup"></a>Skapa en säkerhetskopia
Använd hello ny AzureRmWebAppBackup cmdlet toocreate en säkerhetskopia av ett webbprogram.

        $sasUrl = "<your SAS URL>"
        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl

Detta skapar en säkerhetskopia med ett automatiskt genererat namn. Om du vill tooprovide ett namn för säkerhetskopian kan du använda valfri parameter för hello säkerhetskopia.

        $backup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -StorageAccountUrl $sasUrl -BackupName MyBackup

tooinclude en databas i hello säkerhetskopiering, först skapa en databas inställningen för säkerhetskopiering med hello ny AzureRmWebAppDatabaseBackupSetting cmdlet och sedan ange att det i hello databaser parametern för hello-cmdlet New-AzureRmWebAppBackup. hello databaser parametern accepterar en matris med databasinställningarna, så att du tooback in mer än en databas.

        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbBackup = New-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupName MyBackup -StorageAccountUrl $sasUrl -Databases $dbSetting1,$dbSetting2

## <a name="get-backups"></a>Hämta säkerhetskopieringar
hello Get-AzureRmWebAppBackupList cmdlet returnerar en matris med alla säkerhetskopior för en webbapp. Du måste ange hello namnet på webbprogrammet hello och dess resursgruppen.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $backups = Get-AzureRmWebAppBackupList -Name $appName -ResourceGroupName $resourceGroupName

tooget en specifik säkerhetskopiering, Använd hello Get-AzureRmWebAppBackup cmdlet.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102

Också kan du skicka ett web app-objekt till hello säkerhetskopiering management-cmdlets i informationssyfte.

        $app = Get-AzureRmWebApp -Name ContosoApp -ResourceGroupName Default-Web-WestUS
        $backupList = $app | Get-AzureRmWebAppBackupList
        $backup = $app | Get-AzureRmWebAppBackup -BackupId 10102

## <a name="schedule-automatic-backups"></a>Schemalägga automatisk säkerhetskopiering
Du kan schemalägga säkerhetskopieringar toohappen automatiskt vid ett angivet intervall. tooconfigure ett schema för säkerhetskopiering, Använd hello redigera AzureRmWebAppBackupConfiguration cmdlet. Denna cmdlet har flera parametrar:

* **Namnet** - hello namn på hello webbprogrammet.
* **ResourceGroupName** - hello namn på hello resurs grupp som innehåller hello webb-app.
* **Fack** – valfritt. hello namnet på hello webbprogramplats.
* **StorageAccountUrl** -hello SAS-URL för hello Azure Storage-behållare används toostore hello säkerhetskopieringar.
* **FrequencyInterval** -numeriskt värde för hur ofta hello säkerhetskopieringar ska göras. Måste vara ett positivt heltal.
* **FrequencyUnit** -tidsenhet för hur ofta hello säkerhetskopieringar ska göras. Alternativen är timme och dag.
* **RetentionPeriodInDays** - hur många dagar hello automatisk säkerhetskopiering ska sparas innan de tas bort automatiskt.
* **StartTime** – valfritt. hello tid när hello automatisk säkerhetskopiering ska starta. Säkerhetskopieringar börjar omedelbart om detta är null. Måste vara ett datetime-värde.
* **Databaser** – valfritt. En matris med DatabaseBackupSettings för hello databaser toobackup.
* **KeepAtLeastOneBackup** – valfritt växlade parametern. Ange detta om en säkerhetskopia alltid ska behållas i hello lagringskonto, oavsett hur gammal den är.

Följande är ett exempel på hur toouse denna cmdlet.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Edit-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName -Slot $slotName `
          -StorageAccountUrl "<your SAS URL>" -FrequencyInterval 6 -FrequencyUnit Hour -Databases $dbSetting1,$dbSetting2 `
          -KeepAtLeastOneBackup -StartTime (Get-Date).AddHours(1)

tooget hello aktuella schemat för säkerhetskopiering, Använd hello Get-AzureRmWebAppBackupConfiguration cmdlet. Detta kan vara användbart för att ändra ett schema som redan har konfigurerats.

        $configuration = Get-AzureRmWebAppBackupConfiguration -Name $appName -ResourceGroupName $resourceGroupName

        # Modify hello configuration slightly
        $configuration.FrequencyInterval = 2
        $configuration.FrequencyUnit = "Day"

        # Apply hello new configuration by piping it into hello Edit-AzureRmWebAppBackupConfiguration cmdlet
        $configuration | Edit-AzureRmWebAppBackupConfiguration

## <a name="restore-a-web-app-from-a-backup"></a>Återställa en webbapp från en säkerhetskopia
toorestore en webbapp från en säkerhetskopia, Använd hello återställning AzureRmWebAppBackup cmdlet. hello enklaste sättet toouse denna cmdlet är toopipe i en säkerhetskopiering objekt som har hämtats från hello Get-AzureRmWebAppBackup cmdlet eller Get-AzureRmWebAppBackupList cmdlet.

När du har en säkerhetskopieobjektet kan skicka du det till hello återställning AzureRmWebAppBackup cmdlet. Ange hello Skriv växeln parametern tooindicate du avser toooverwrite hello innehållet i ditt webbprogram med hello innehållet i hello säkerhetskopiering. Om hello säkerhetskopian innehåller databaser, återställs även dessa databaser.

        $backup | Restore-AzureRmWebAppBackup -Overwrite

Följande är ett exempel på hur toouse hello återställning AzureRmWebAppBackup genom att ange alla hello-parametrar.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        $slotName = "StagingSlot"
        $blobName = "ContosoBackup.zip"
        $dbSetting1 = New-AzureRmWebAppDatabaseBackupSetting -Name DB1 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        $dbSetting2 = New-AzureRmWebAppDatabaseBackupSetting -Name DB2 -DatabaseType SqlAzure -ConnectionString "<connection_string>"
        Restore-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -Slot $slotName -StorageAccountUrl "<your SAS URL>" -BlobName $blobName -Databases $dbSetting1,$dbSetting2 -Overwrite

## <a name="delete-a-backup"></a>Ta bort en säkerhetskopia
toodelete en säkerhetskopia, Använd hello Remove-AzureRmWebAppBackup cmdlet. Detta tar bort hello säkerhetskopiering från ditt lagringskonto. Ange ditt appnamn, dess resursgruppen och hello-ID för hello säkerhetskopiering önskade toodelete.

        $resourceGroupName = "Default-Web-WestUS"
        $appName = "ContosoApp"
        Remove-AzureRmWebAppBackup -ResourceGroupName $resourceGroupName -Name $appName -BackupId 10102

Också kan du skicka en säkerhetskopieobjektet till hello Remove-AzureRmWebAppBackup cmdlet toodelete den.

        $backup = Get-AzureRmWebAppBackup -Name $appName -ResourceGroupName $resourceGroupName -BackupId 10102
        $backup | Remove-AzureRmWebAppBackup -Overwrite
