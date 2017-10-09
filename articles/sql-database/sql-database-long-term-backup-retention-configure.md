---
title: "Konfigurera långsiktig lagring av säkerhetskopior. - Azure SQL database | Microsoft Docs"
description: "Lär dig hur toostore automatisk säkerhetskopiering i hello Azure Recovery Services-valvet och toorestore från hello Azure Recovery Services-valvet"
services: sql-database
documentationcenter: 
author: CarlRabeler
manager: jhubbard
editor: 
ms.assetid: aeb8c4c3-6ae2-45f7-b2c3-fa13e3752eed
ms.service: sql-database
ms.custom: business continuity
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/10/2017
ms.author: carlrab
ms.openlocfilehash: 603f4dd21cee4407d46f749655aba8f9ef3322c0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a>Konfigurera och återställa från Azure SQL Database långsiktig lagring av säkerhetskopior.

Du kan konfigurera hello Azure Recovery Services-valvet toostore Azure SQL databassäkerhetskopieringar och sedan återställa en databas med hjälp av säkerhetskopior lagras i hello valvet med hello Azure-portalen eller PowerShell.

## <a name="azure-portal"></a>Azure Portal

hello följande avsnitt visar du hur toouse hello Azure portal tooconfigure hello Azure Recovery Services valvet, visa säkerhetskopieringar i hello-valvet och återställa från hello-valvet.

### <a name="configure-hello-vault-register-hello-server-and-select-databases"></a>Konfigurera hello valvet, registrera hello-servern och välj databaser

Du [säkerhetskopieringar ett Azure Recovery Services-valvet tooretain automatisk](sql-database-long-term-retention.md) under längre tid än hello kvarhållningsperiod för din tjänstnivån. 

1. Öppna hello **SQL Server** för servern.

   ![SQL server-sida](./media/sql-database-get-started-portal/sql-server-blade.png)

2. Klicka på **Long-term backup retention** (Långsiktig kvarhållning av säkerhetskopior).

   ![länk till långsiktig kvarhållning av säkerhetskopior](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. På hello **långsiktig lagring av säkerhetskopior** för servern, granska och Godkänn hello preview villkor (om du redan har gjort det - eller den här funktionen är inte längre i preview).

   ![Acceptera hello preview](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. tooconfigure långsiktig säkerhetskopiering kvarhållning, väljer du den databasen i hello rutnätet och klicka sedan på **konfigurera** hello i verktygsfältet.

   ![välj databas för långsiktig kvarhållning av säkerhetskopior](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. På hello **konfigurera** klickar du på **konfigurera nödvändiga inställningar** under **Recovery service-valvet**.

   ![länk för att konfigurera valv](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. På hello **återställningstjänstvalvet** väljer en befintlig valvet, om sådana finns. Annars om inga recovery services-ventilen hittades för din prenumeration, klickar du på tooexit hello flödet och skapa ett recovery services-valv.

   ![Skapa valv länk](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. På hello **Recovery Services-valv** klickar du på **Lägg till**.

   ![Lägg till valvet länk](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. På hello **Recovery Services-valvet** sidan, ange ett giltigt namn för hello Recovery Services-valvet.

   ![namn på nytt valv](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. Välj din prenumeration och resursgrupp och välj sedan hello plats för hello-valvet. När du är klar klickar du på **Skapa**.

   ![Skapa valv](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > hello valvet måste finnas i hello samma region som logisk hello Azure SQL-server och måste använda hello samma resursgrupp som hello logisk server.
   >

10. När du har skapat hello nytt valv köra hello nödvändiga åtgärder tooreturn toohello **återställningstjänstvalvet** sidan.

11. På hello **återställningstjänstvalvet** klickar hello valvet och klicka sedan på **Välj**.

   ![välj befintligt valv](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. På hello **konfigurera** sidan, ange ett giltigt namn för hello ny bevarandeprincip, ändra hello standard bevarandeprincip efter behov och klicka sedan på **OK**.

   ![definiera kvarhållningsprincip](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. På hello **långsiktig lagring av säkerhetskopior** för din databas, klickar du på **spara** och klicka sedan på **OK** tooapply hello långsiktig lagring av säkerhetskopior principen tooall valt databaser.

   ![definiera kvarhållningsprincip](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. Klicka på **spara** tooenable långsiktig lagring av säkerhetskopior med det här nya principen toohello Azure Recovery Services-valvet som du har konfigurerat.

   ![definiera kvarhållningsprincip](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> När du konfigurerat visas säkerhetskopieringar i hello valvet inom sju dagar. Fortsätt inte den här kursen tills säkerhetskopieringar visas i hello-valvet.
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a>Visa säkerhetskopior i långsiktig kvarhållning med hjälp av Azure portal

Visa information om din databas säkerhetskopior i [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md). 

1. I hello Azure-portalen, öppna ditt Azure Recovery Services-valv för säkerhetskopiering av databaser (gå för**alla resurser** och välj den hello listan över resurser för din prenumeration) tooview hello mängden lagringsutrymme som används av databasen säkerhetskopiering i hello-valvet.

   ![visa Recovery Services-valvet med säkerhetskopior](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. Öppna hello **SQL-databas** för din databas.

   ![nya exempelsida db](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. På verktygsfältet hello **återställa**.

   ![verktygsfältet Återställ](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. På hello återställning klickar du på **långsiktiga**.

5. Under Azure valvet säkerhetskopior, klickar du på **markerar du en säkerhetskopia** tooview hello tillgängliga databassäkerhetskopieringar i långsiktig säkerhetskopiering kvarhållning.

   ![säkerhetskopior i valv](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-hello-azure-portal"></a>Återställa en databas från en säkerhetskopia i långsiktig lagring av säkerhetskopior med hello Azure-portalen

Du kan återställa hello tooa nya databasen från en säkerhetskopia i hello Azure Recovery Services-valvet.

1. På hello **Azure valvet säkerhetskopieringar** klickar hello säkerhetskopiering toorestore och klicka sedan på **Välj**.

   ![välj säkerhetskopia i valv](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. I hello **databasnamnet** text Ange hello namn hello återställa databasen.

   ![nytt databasnamn](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. Klicka på **OK** toorestore databasen från en säkerhetskopia av hello i hello valvet toohello nya databasen.

4. Klicka på hello notification ikonen tooview hello status för återställningsjobbet hello hello verktygsfältet.

   ![förlopp över återställningsjobb från valvet](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. När hello Återställningsjobbet har slutförts, öppna hello **SQL-databaser** sidan tooview hello nyligen återställa databasen.

   ![återställd databas från valvet](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> Härifrån kan du ansluta toohello återställs databasen med hjälp av SQL Server Management Studio tooperform behövs uppgifter som för[extrahera en bit data från hello återställs databasen toocopy till hello befintlig databas eller toodelete hello befintliga Databasnamn för databasen och Byt namn på hello återställs databasen toohello befintliga](sql-database-recovery-using-backups.md#point-in-time-restore).
>

## <a name="powershell"></a>PowerShell

hello följande avsnitt visar hur toouse PowerShell tooconfigure hello Azure Recovery Services-valvet visa säkerhetskopieringar i hello valvet och återställa från hello-valvet.

### <a name="create-a-recovery-services-vault"></a>Skapa ett Recovery Services-valv

Använd hello [ny AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate en recovery services-valvet.

> [!IMPORTANT]
> hello valvet måste finnas i hello samma region som logisk hello Azure SQL-server och måste använda hello samma resursgrupp som hello logisk server.

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-toouse-hello-recovery-vault-for-its-long-term-retention-backups"></a>Ange din server toouse hello recovery-valvet för dess långsiktig kvarhållning säkerhetskopieringar

Använd hello [Set AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet tooassociate tidigare skapade återställningstjänster valvet med en specifik Azure SQL-server.

```PowerShell
# Set your server toouse hello vault toofor long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a>Skapa en kvarhållningsprincip

En bevarandeprincip är där du ange hur länge tookeep en säkerhetskopia av databasen. Använd hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet tooget hello standard kvarhållning princip toouse som hello mall för att skapa principer. I den här mallen anges hello kvarhållningsperiod i två år. Kör därefter hello [ny AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally skapa hello princip. 

> [!NOTE]
> Vissa cmdletar kräver att du ställer in hello valvet kontexten innan du kör ([Set AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) så att du ser denna cmdlet i några relaterade kodavsnitt. Du kan ange hello kontext eftersom hello principen är en del av hello valvet. Du kan skapa flera bevarandeprinciper för varje valvet och sedan använda hello önskad princip toospecific databaser. 


```PowerShell
# Retrieve hello default retention policy for hello AzureSQLDatabase workload type
$retentionPolicy = Get-AzureRmRecoveryServicesBackupRetentionPolicyObject -WorkloadType AzureSQLDatabase

# Set hello retention value tootwo years (you can set tooany time between 1 week and 10 years)
$retentionPolicy.RetentionDurationType = "Years"
$retentionPolicy.RetentionCount = 2
$retentionPolicyName = "my2YearRetentionPolicy"

# Set hello vault context toohello vault you are creating hello policy for
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# Create hello new policy
$policy = New-AzureRmRecoveryServicesBackupProtectionPolicy -name $retentionPolicyName -WorkloadType AzureSQLDatabase -retentionPolicy $retentionPolicy
$policy
```

### <a name="configure-a-database-toouse-hello-previously-defined-retention-policy"></a>Konfigurera en bevarandeprincip för databasen toouse hello som tidigare definierats

Använd hello [Set AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet tooapply hello ny princip tooa specifika databas.

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a>Visa information om säkerhetskopiering och säkerhetskopieringar i långsiktig kvarhållning

Visa information om din databas säkerhetskopior i [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md). 

Använd hello följande cmdlets tooview säkerhetskopierad information:

- [Get-AzureRmRecoveryServicesBackupContainer](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [Get-AzureRmRecoveryServicesBackupItem](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [Get-AzureRmRecoveryServicesBackupRecoveryPoint](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

```PowerShell
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$databaseNeedingRestore = $databaseName

# Set hello vault context toohello vault we want toorestore from
#$vault = Get-AzureRmRecoveryServicesVault -ResourceGroupName $resourceGroupName
Set-AzureRmRecoveryServicesVaultContext -Vault $vault

# hello following commands find hello container associated with hello server 'myserver' under resource group 'myresourcegroup'
$container = Get-AzureRmRecoveryServicesBackupContainer -ContainerType AzureSQL -FriendlyName $vault.Name

# Get hello long-term retention metadata associated with a specific database
$item = Get-AzureRmRecoveryServicesBackupItem -Container $container -WorkloadType AzureSQLDatabase -Name $databaseNeedingRestore

# Get all available backups for hello previously indicated database
# Optionally, set hello -StartDate and -EndDate parameters tooreturn backups within a specific time period
$availableBackups = Get-AzureRmRecoveryServicesBackupRecoveryPoint -Item $item
$availableBackups
```

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a>Återställa en databas från en säkerhetskopia i långsiktig kvarhållning av säkerhetskopior

Återställa från långsiktig lagring av säkerhetskopior använder hello [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.

```PowerShell
# Restore hello most recent backup: $availableBackups[0]
#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$restoredDatabaseName = "{new-database-name}"
$edition = "Basic"
$performanceLevel = "Basic"

$restoredDb = Restore-AzureRmSqlDatabase -FromLongTermRetentionBackup -ResourceId $availableBackups[0].Id -ResourceGroupName $resourceGroupName `
 -ServerName $serverName -TargetDatabaseName $restoredDatabaseName -Edition $edition -ServiceObjectiveName $performanceLevel
$restoredDb
```


> [!NOTE]
> Från den här kan du ansluta toohello återställs databasen med hjälp av SQL Server Management Studio tooperform behövs uppgifter, till exempel tooextract lite data från hello återställa databasen toocopy till hello befintlig databas eller toodelete hello befintlig databas och Byt namn hello återställda toohello befintligt databasnamn. Se [punkt tidpunkt för återställning](sql-database-recovery-using-backups.md#point-in-time-restore).

## <a name="next-steps"></a>Nästa steg

- toolearn om tjänst som skapade automatisk säkerhetskopiering finns [automatisk säkerhetskopiering](sql-database-automated-backups.md)
- toolearn om långsiktig säkerhetskopiering kvarhållning finns [långsiktig lagring av säkerhetskopior.](sql-database-long-term-retention.md)
- toolearn om återställning från säkerhetskopior, se [återställa från säkerhetskopia](sql-database-recovery-using-backups.md)
