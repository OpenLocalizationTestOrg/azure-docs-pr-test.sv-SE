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
# <a name="configure-and-restore-from-azure-sql-database-long-term-backup-retention"></a><span data-ttu-id="7cb87-103">Konfigurera och återställa från Azure SQL Database långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="7cb87-103">Configure and restore from Azure SQL Database long-term backup retention</span></span>

<span data-ttu-id="7cb87-104">Du kan konfigurera hello Azure Recovery Services-valvet toostore Azure SQL databassäkerhetskopieringar och sedan återställa en databas med hjälp av säkerhetskopior lagras i hello valvet med hello Azure-portalen eller PowerShell.</span><span class="sxs-lookup"><span data-stu-id="7cb87-104">You can configure hello Azure Recovery Services vault toostore Azure SQL database backups and then recover a database using backups retained in hello vault using hello Azure portal or PowerShell.</span></span>

## <a name="azure-portal"></a><span data-ttu-id="7cb87-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7cb87-105">Azure portal</span></span>

<span data-ttu-id="7cb87-106">hello följande avsnitt visar du hur toouse hello Azure portal tooconfigure hello Azure Recovery Services valvet, visa säkerhetskopieringar i hello-valvet och återställa från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-106">hello following sections show you how toouse hello Azure portal tooconfigure hello Azure Recovery Services vault, view backups in hello vault, and restore from hello vault.</span></span>

### <a name="configure-hello-vault-register-hello-server-and-select-databases"></a><span data-ttu-id="7cb87-107">Konfigurera hello valvet, registrera hello-servern och välj databaser</span><span class="sxs-lookup"><span data-stu-id="7cb87-107">Configure hello vault, register hello server, and select databases</span></span>

<span data-ttu-id="7cb87-108">Du [säkerhetskopieringar ett Azure Recovery Services-valvet tooretain automatisk](sql-database-long-term-retention.md) under längre tid än hello kvarhållningsperiod för din tjänstnivån.</span><span class="sxs-lookup"><span data-stu-id="7cb87-108">You [configure an Azure Recovery Services vault tooretain automated backups](sql-database-long-term-retention.md) for a period longer than hello retention period for your service tier.</span></span> 

1. <span data-ttu-id="7cb87-109">Öppna hello **SQL Server** för servern.</span><span class="sxs-lookup"><span data-stu-id="7cb87-109">Open hello **SQL Server** page for your server.</span></span>

   ![SQL server-sida](./media/sql-database-get-started-portal/sql-server-blade.png)

2. <span data-ttu-id="7cb87-111">Klicka på **Long-term backup retention** (Långsiktig kvarhållning av säkerhetskopior).</span><span class="sxs-lookup"><span data-stu-id="7cb87-111">Click **Long-term backup retention**.</span></span>

   ![länk till långsiktig kvarhållning av säkerhetskopior](./media/sql-database-get-started-backup-recovery/long-term-backup-retention-link.png)

3. <span data-ttu-id="7cb87-113">På hello **långsiktig lagring av säkerhetskopior** för servern, granska och Godkänn hello preview villkor (om du redan har gjort det - eller den här funktionen är inte längre i preview).</span><span class="sxs-lookup"><span data-stu-id="7cb87-113">On hello **Long-term backup retention** page for your server, review and accept hello preview terms (unless you have already done so - or this feature is no longer in preview).</span></span>

   ![Acceptera hello preview](./media/sql-database-get-started-backup-recovery/accept-the-preview-terms.png)

4. <span data-ttu-id="7cb87-115">tooconfigure långsiktig säkerhetskopiering kvarhållning, väljer du den databasen i hello rutnätet och klicka sedan på **konfigurera** hello i verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-115">tooconfigure long-term backup retention, select that database in hello grid and then click **Configure** on hello toolbar.</span></span>

   ![välj databas för långsiktig kvarhållning av säkerhetskopior](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

5. <span data-ttu-id="7cb87-117">På hello **konfigurera** klickar du på **konfigurera nödvändiga inställningar** under **Recovery service-valvet**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-117">On hello **Configure** page, click **Configure required settings** under **Recovery service vault**.</span></span>

   ![länk för att konfigurera valv](./media/sql-database-get-started-backup-recovery/configure-vault-link.png)

6. <span data-ttu-id="7cb87-119">På hello **återställningstjänstvalvet** väljer en befintlig valvet, om sådana finns.</span><span class="sxs-lookup"><span data-stu-id="7cb87-119">On hello **Recovery services vault** page, select an existing vault, if any.</span></span> <span data-ttu-id="7cb87-120">Annars om inga recovery services-ventilen hittades för din prenumeration, klickar du på tooexit hello flödet och skapa ett recovery services-valv.</span><span class="sxs-lookup"><span data-stu-id="7cb87-120">Otherwise, if no recovery services vault found for your subscription, click tooexit hello flow and create a recovery services vault.</span></span>

   ![Skapa valv länk](./media/sql-database-get-started-backup-recovery/create-new-vault-link.png)

7. <span data-ttu-id="7cb87-122">På hello **Recovery Services-valv** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-122">On hello **Recovery Services vaults** page, click **Add**.</span></span>

   ![Lägg till valvet länk](./media/sql-database-get-started-backup-recovery/add-new-vault-link.png)
   
8. <span data-ttu-id="7cb87-124">På hello **Recovery Services-valvet** sidan, ange ett giltigt namn för hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-124">On hello **Recovery Services vault** page, provide a valid name for hello Recovery Services vault.</span></span>

   ![namn på nytt valv](./media/sql-database-get-started-backup-recovery/new-vault-name.png)

9. <span data-ttu-id="7cb87-126">Välj din prenumeration och resursgrupp och välj sedan hello plats för hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-126">Select your subscription and resource group, and then select hello location for hello vault.</span></span> <span data-ttu-id="7cb87-127">När du är klar klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-127">When done, click **Create**.</span></span>

   ![Skapa valv](./media/sql-database-get-started-backup-recovery/create-new-vault.png)

   > [!IMPORTANT]
   > <span data-ttu-id="7cb87-129">hello valvet måste finnas i hello samma region som logisk hello Azure SQL-server och måste använda hello samma resursgrupp som hello logisk server.</span><span class="sxs-lookup"><span data-stu-id="7cb87-129">hello vault must be located in hello same region as hello Azure SQL logical server, and must use hello same resource group as hello logical server.</span></span>
   >

10. <span data-ttu-id="7cb87-130">När du har skapat hello nytt valv köra hello nödvändiga åtgärder tooreturn toohello **återställningstjänstvalvet** sidan.</span><span class="sxs-lookup"><span data-stu-id="7cb87-130">After hello new vault is created, execute hello necessary steps tooreturn toohello **Recovery services vault** page.</span></span>

11. <span data-ttu-id="7cb87-131">På hello **återställningstjänstvalvet** klickar hello valvet och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-131">On hello **Recovery services vault** page, click hello vault and then click **Select**.</span></span>

   ![välj befintligt valv](./media/sql-database-get-started-backup-recovery/select-existing-vault.png)

12. <span data-ttu-id="7cb87-133">På hello **konfigurera** sidan, ange ett giltigt namn för hello ny bevarandeprincip, ändra hello standard bevarandeprincip efter behov och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-133">On hello **Configure** page, provide a valid name for hello new retention policy, modify hello default retention policy as appropriate, and then click **OK**.</span></span>

   ![definiera kvarhållningsprincip](./media/sql-database-get-started-backup-recovery/define-retention-policy.png)

13. <span data-ttu-id="7cb87-135">På hello **långsiktig lagring av säkerhetskopior** för din databas, klickar du på **spara** och klicka sedan på **OK** tooapply hello långsiktig lagring av säkerhetskopior principen tooall valt databaser.</span><span class="sxs-lookup"><span data-stu-id="7cb87-135">On hello **Long-term backup retention** page for your database, click **Save** and then click **OK** tooapply hello long-term backup retention policy tooall selected databases.</span></span>

   ![definiera kvarhållningsprincip](./media/sql-database-get-started-backup-recovery/save-retention-policy.png)

14. <span data-ttu-id="7cb87-137">Klicka på **spara** tooenable långsiktig lagring av säkerhetskopior med det här nya principen toohello Azure Recovery Services-valvet som du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="7cb87-137">Click **Save** tooenable long-term backup retention using this new policy toohello Azure Recovery Services vault that you configured.</span></span>

   ![definiera kvarhållningsprincip](./media/sql-database-get-started-backup-recovery/enable-long-term-retention.png)

> [!IMPORTANT]
> <span data-ttu-id="7cb87-139">När du konfigurerat visas säkerhetskopieringar i hello valvet inom sju dagar.</span><span class="sxs-lookup"><span data-stu-id="7cb87-139">Once configured, backups show up in hello vault within next seven days.</span></span> <span data-ttu-id="7cb87-140">Fortsätt inte den här kursen tills säkerhetskopieringar visas i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-140">Do not continue this tutorial until backups show up in hello vault.</span></span>
>

### <a name="view-backups-in-long-term-retention-using-azure-portal"></a><span data-ttu-id="7cb87-141">Visa säkerhetskopior i långsiktig kvarhållning med hjälp av Azure portal</span><span class="sxs-lookup"><span data-stu-id="7cb87-141">View backups in long-term retention using Azure portal</span></span>

<span data-ttu-id="7cb87-142">Visa information om din databas säkerhetskopior i [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="7cb87-142">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

1. <span data-ttu-id="7cb87-143">I hello Azure-portalen, öppna ditt Azure Recovery Services-valv för säkerhetskopiering av databaser (gå för**alla resurser** och välj den hello listan över resurser för din prenumeration) tooview hello mängden lagringsutrymme som används av databasen säkerhetskopiering i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-143">In hello Azure portal, open your Azure Recovery Services vault for your database backups (go too**All resources** and select it from hello list of resources for your subscription) tooview hello amount of storage used by your database backups in hello vault.</span></span>

   ![visa Recovery Services-valvet med säkerhetskopior](./media/sql-database-get-started-backup-recovery/view-recovery-services-vault-with-data.png)

2. <span data-ttu-id="7cb87-145">Öppna hello **SQL-databas** för din databas.</span><span class="sxs-lookup"><span data-stu-id="7cb87-145">Open hello **SQL database** page for your database.</span></span>

   ![nya exempelsida db](./media/sql-database-get-started-portal/new-sample-db-blade.png)

3. <span data-ttu-id="7cb87-147">På verktygsfältet hello **återställa**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-147">On hello toolbar, click **Restore**.</span></span>

   ![verktygsfältet Återställ](./media/sql-database-get-started-backup-recovery/restore-toolbar.png)

4. <span data-ttu-id="7cb87-149">På hello återställning klickar du på **långsiktiga**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-149">On hello Restore page, click **Long-term**.</span></span>

5. <span data-ttu-id="7cb87-150">Under Azure valvet säkerhetskopior, klickar du på **markerar du en säkerhetskopia** tooview hello tillgängliga databassäkerhetskopieringar i långsiktig säkerhetskopiering kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="7cb87-150">Under Azure vault backups, click **Select a backup** tooview hello available database backups in long-term backup retention.</span></span>

   ![säkerhetskopior i valv](./media/sql-database-get-started-backup-recovery/view-backups-in-vault.png)

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention-using-hello-azure-portal"></a><span data-ttu-id="7cb87-152">Återställa en databas från en säkerhetskopia i långsiktig lagring av säkerhetskopior med hello Azure-portalen</span><span class="sxs-lookup"><span data-stu-id="7cb87-152">Restore a database from a backup in long-term backup retention using hello Azure portal</span></span>

<span data-ttu-id="7cb87-153">Du kan återställa hello tooa nya databasen från en säkerhetskopia i hello Azure Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-153">You restore hello database tooa new database from a backup in hello Azure Recovery Services vault.</span></span>

1. <span data-ttu-id="7cb87-154">På hello **Azure valvet säkerhetskopieringar** klickar hello säkerhetskopiering toorestore och klicka sedan på **Välj**.</span><span class="sxs-lookup"><span data-stu-id="7cb87-154">On hello **Azure vault backups** page, click hello backup toorestore and then click **Select**.</span></span>

   ![välj säkerhetskopia i valv](./media/sql-database-get-started-backup-recovery/select-backup-in-vault.png)

2. <span data-ttu-id="7cb87-156">I hello **databasnamnet** text Ange hello namn hello återställa databasen.</span><span class="sxs-lookup"><span data-stu-id="7cb87-156">In hello **Database name** text box, provide hello name for hello restored database.</span></span>

   ![nytt databasnamn](./media/sql-database-get-started-backup-recovery/new-database-name.png)

3. <span data-ttu-id="7cb87-158">Klicka på **OK** toorestore databasen från en säkerhetskopia av hello i hello valvet toohello nya databasen.</span><span class="sxs-lookup"><span data-stu-id="7cb87-158">Click **OK** toorestore your database from hello backup in hello vault toohello new database.</span></span>

4. <span data-ttu-id="7cb87-159">Klicka på hello notification ikonen tooview hello status för återställningsjobbet hello hello verktygsfältet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-159">On hello toolbar, click hello notification icon tooview hello status of hello restore job.</span></span>

   ![förlopp över återställningsjobb från valvet](./media/sql-database-get-started-backup-recovery/restore-job-progress-long-term.png)

5. <span data-ttu-id="7cb87-161">När hello Återställningsjobbet har slutförts, öppna hello **SQL-databaser** sidan tooview hello nyligen återställa databasen.</span><span class="sxs-lookup"><span data-stu-id="7cb87-161">When hello restore job is completed, open hello **SQL databases** page tooview hello newly restored database.</span></span>

   ![återställd databas från valvet](./media/sql-database-get-started-backup-recovery/restored-database-from-vault.png)

> [!NOTE]
> <span data-ttu-id="7cb87-163">Härifrån kan du ansluta toohello återställs databasen med hjälp av SQL Server Management Studio tooperform behövs uppgifter som för[extrahera en bit data från hello återställs databasen toocopy till hello befintlig databas eller toodelete hello befintliga Databasnamn för databasen och Byt namn på hello återställs databasen toohello befintliga](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="7cb87-163">From here, you can connect toohello restored database using SQL Server Management Studio tooperform needed tasks, such as too[extract a bit of data from hello restored database toocopy into hello existing database or toodelete hello existing database and rename hello restored database toohello existing database name](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>
>

## <a name="powershell"></a><span data-ttu-id="7cb87-164">PowerShell</span><span class="sxs-lookup"><span data-stu-id="7cb87-164">PowerShell</span></span>

<span data-ttu-id="7cb87-165">hello följande avsnitt visar hur toouse PowerShell tooconfigure hello Azure Recovery Services-valvet visa säkerhetskopieringar i hello valvet och återställa från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-165">hello following sections show you how toouse PowerShell tooconfigure hello Azure Recovery Services vault, view backups in hello vault, and restore from hello vault.</span></span>

### <a name="create-a-recovery-services-vault"></a><span data-ttu-id="7cb87-166">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="7cb87-166">Create a recovery services vault</span></span>

<span data-ttu-id="7cb87-167">Använd hello [ny AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate en recovery services-valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-167">Use hello [New-AzureRmRecoveryServicesVault](/powershell/module/azurerm.recoveryservices/new-azurermrecoveryservicesvault) toocreate a recovery services vault.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="7cb87-168">hello valvet måste finnas i hello samma region som logisk hello Azure SQL-server och måste använda hello samma resursgrupp som hello logisk server.</span><span class="sxs-lookup"><span data-stu-id="7cb87-168">hello vault must be located in hello same region as hello Azure SQL logical server, and must use hello same resource group as hello logical server.</span></span>

```PowerShell
# Create a recovery services vault

#$resourceGroupName = "{resource-group-name}"
#$serverName = "{server-name}"
$serverLocation = (Get-AzureRmSqlServer -ServerName $serverName -ResourceGroupName $resourceGroupName).Location
$recoveryServiceVaultName = "{new-vault-name}"

$vault = New-AzureRmRecoveryServicesVault -Name $recoveryServiceVaultName -ResourceGroupName $ResourceGroupName -Location $serverLocation 
Set-AzureRmRecoveryServicesBackupProperties -BackupStorageRedundancy LocallyRedundant -Vault $vault
```

### <a name="set-your-server-toouse-hello-recovery-vault-for-its-long-term-retention-backups"></a><span data-ttu-id="7cb87-169">Ange din server toouse hello recovery-valvet för dess långsiktig kvarhållning säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="7cb87-169">Set your server toouse hello recovery vault for its long-term retention backups</span></span>

<span data-ttu-id="7cb87-170">Använd hello [Set AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet tooassociate tidigare skapade återställningstjänster valvet med en specifik Azure SQL-server.</span><span class="sxs-lookup"><span data-stu-id="7cb87-170">Use hello [Set-AzureRmSqlServerBackupLongTermRetentionVault](/powershell/module/azurerm.sql/set-azurermsqlserverbackuplongtermretentionvault) cmdlet tooassociate a previously created recovery services vault with a specific Azure SQL server.</span></span>

```PowerShell
# Set your server toouse hello vault toofor long-term backup retention 

Set-AzureRmSqlServerBackupLongTermRetentionVault -ResourceGroupName $resourceGroupName -ServerName $serverName -ResourceId $vault.Id
```

### <a name="create-a-retention-policy"></a><span data-ttu-id="7cb87-171">Skapa en kvarhållningsprincip</span><span class="sxs-lookup"><span data-stu-id="7cb87-171">Create a retention policy</span></span>

<span data-ttu-id="7cb87-172">En bevarandeprincip är där du ange hur länge tookeep en säkerhetskopia av databasen.</span><span class="sxs-lookup"><span data-stu-id="7cb87-172">A retention policy is where you set how long tookeep a database backup.</span></span> <span data-ttu-id="7cb87-173">Använd hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet tooget hello standard kvarhållning princip toouse som hello mall för att skapa principer.</span><span class="sxs-lookup"><span data-stu-id="7cb87-173">Use hello [Get-AzureRmRecoveryServicesBackupRetentionPolicyObject](https://docs.microsoft.com/powershell/resourcemanager/azurerm.recoveryservices.backup/v2.3.0/get-azurermrecoveryservicesbackupretentionpolicyobject) cmdlet tooget hello default retention policy toouse as hello template for creating policies.</span></span> <span data-ttu-id="7cb87-174">I den här mallen anges hello kvarhållningsperiod i två år.</span><span class="sxs-lookup"><span data-stu-id="7cb87-174">In this template, hello retention period is set for 2 years.</span></span> <span data-ttu-id="7cb87-175">Kör därefter hello [ny AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally skapa hello princip.</span><span class="sxs-lookup"><span data-stu-id="7cb87-175">Next, run hello [New-AzureRmRecoveryServicesBackupProtectionPolicy](/powershell/module/azurerm.recoveryservices.backup/new-azurermrecoveryservicesbackupprotectionpolicy) toofinally create hello policy.</span></span> 

> [!NOTE]
> <span data-ttu-id="7cb87-176">Vissa cmdletar kräver att du ställer in hello valvet kontexten innan du kör ([Set AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) så att du ser denna cmdlet i några relaterade kodavsnitt.</span><span class="sxs-lookup"><span data-stu-id="7cb87-176">Some cmdlets require that you set hello vault context before running ([Set-AzureRmRecoveryServicesVaultContext](/powershell/module/azurerm.recoveryservices/set-azurermrecoveryservicesvaultcontext)) so you see this cmdlet in a few related snippets.</span></span> <span data-ttu-id="7cb87-177">Du kan ange hello kontext eftersom hello principen är en del av hello valvet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-177">You set hello context because hello policy is part of hello vault.</span></span> <span data-ttu-id="7cb87-178">Du kan skapa flera bevarandeprinciper för varje valvet och sedan använda hello önskad princip toospecific databaser.</span><span class="sxs-lookup"><span data-stu-id="7cb87-178">You can create multiple retention policies for each vault and then apply hello desired policy toospecific databases.</span></span> 


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

### <a name="configure-a-database-toouse-hello-previously-defined-retention-policy"></a><span data-ttu-id="7cb87-179">Konfigurera en bevarandeprincip för databasen toouse hello som tidigare definierats</span><span class="sxs-lookup"><span data-stu-id="7cb87-179">Configure a database toouse hello previously defined retention policy</span></span>

<span data-ttu-id="7cb87-180">Använd hello [Set AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet tooapply hello ny princip tooa specifika databas.</span><span class="sxs-lookup"><span data-stu-id="7cb87-180">Use hello [Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy](/powershell/module/azurerm.sql/set-azurermsqldatabasebackuplongtermretentionpolicy) cmdlet tooapply hello new policy tooa specific database.</span></span>

```PowerShell
# Enable long-term retention for a specific SQL database
$policyState = "enabled"
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName $resourceGroupName -ServerName $serverName -DatabaseName $databaseName -State $policyState -ResourceId $policy.Id
```

### <a name="view-backup-info-and-backups-in-long-term-retention"></a><span data-ttu-id="7cb87-181">Visa information om säkerhetskopiering och säkerhetskopieringar i långsiktig kvarhållning</span><span class="sxs-lookup"><span data-stu-id="7cb87-181">View backup info, and backups in long-term retention</span></span>

<span data-ttu-id="7cb87-182">Visa information om din databas säkerhetskopior i [långsiktig lagring av säkerhetskopior](sql-database-long-term-retention.md).</span><span class="sxs-lookup"><span data-stu-id="7cb87-182">View information about your database backups in [long-term backup retention](sql-database-long-term-retention.md).</span></span> 

<span data-ttu-id="7cb87-183">Använd hello följande cmdlets tooview säkerhetskopierad information:</span><span class="sxs-lookup"><span data-stu-id="7cb87-183">Use hello following cmdlets tooview backup information:</span></span>

- [<span data-ttu-id="7cb87-184">Get-AzureRmRecoveryServicesBackupContainer</span><span class="sxs-lookup"><span data-stu-id="7cb87-184">Get-AzureRmRecoveryServicesBackupContainer</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupcontainer)
- [<span data-ttu-id="7cb87-185">Get-AzureRmRecoveryServicesBackupItem</span><span class="sxs-lookup"><span data-stu-id="7cb87-185">Get-AzureRmRecoveryServicesBackupItem</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackupitem)
- [<span data-ttu-id="7cb87-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span><span class="sxs-lookup"><span data-stu-id="7cb87-186">Get-AzureRmRecoveryServicesBackupRecoveryPoint</span></span>](/powershell/module/azurerm.recoveryservices.backup/get-azurermrecoveryservicesbackuprecoverypoint)

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

### <a name="restore-a-database-from-a-backup-in-long-term-backup-retention"></a><span data-ttu-id="7cb87-187">Återställa en databas från en säkerhetskopia i långsiktig kvarhållning av säkerhetskopior</span><span class="sxs-lookup"><span data-stu-id="7cb87-187">Restore a database from a backup in long-term backup retention</span></span>

<span data-ttu-id="7cb87-188">Återställa från långsiktig lagring av säkerhetskopior använder hello [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.</span><span class="sxs-lookup"><span data-stu-id="7cb87-188">Restoring from long-term backup retention uses hello [Restore-AzureRmSqlDatabase](/powershell/module/azurerm.sql/restore-azurermsqldatabase) cmdlet.</span></span>

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
> <span data-ttu-id="7cb87-189">Från den här kan du ansluta toohello återställs databasen med hjälp av SQL Server Management Studio tooperform behövs uppgifter, till exempel tooextract lite data från hello återställa databasen toocopy till hello befintlig databas eller toodelete hello befintlig databas och Byt namn hello återställda toohello befintligt databasnamn.</span><span class="sxs-lookup"><span data-stu-id="7cb87-189">From here, you can connect toohello restored database using SQL Server Management Studio tooperform needed tasks, such as tooextract a bit of data from hello restored database toocopy into hello existing database or toodelete hello existing database and rename hello restored database toohello existing database name.</span></span> <span data-ttu-id="7cb87-190">Se [punkt tidpunkt för återställning](sql-database-recovery-using-backups.md#point-in-time-restore).</span><span class="sxs-lookup"><span data-stu-id="7cb87-190">See [point in time restore](sql-database-recovery-using-backups.md#point-in-time-restore).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7cb87-191">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7cb87-191">Next steps</span></span>

- <span data-ttu-id="7cb87-192">toolearn om tjänst som skapade automatisk säkerhetskopiering finns [automatisk säkerhetskopiering](sql-database-automated-backups.md)</span><span class="sxs-lookup"><span data-stu-id="7cb87-192">toolearn about service-generated automatic backups, see [automatic backups](sql-database-automated-backups.md)</span></span>
- <span data-ttu-id="7cb87-193">toolearn om långsiktig säkerhetskopiering kvarhållning finns [långsiktig lagring av säkerhetskopior.](sql-database-long-term-retention.md)</span><span class="sxs-lookup"><span data-stu-id="7cb87-193">toolearn about long-term backup retention, see [long-term backup retention](sql-database-long-term-retention.md)</span></span>
- <span data-ttu-id="7cb87-194">toolearn om återställning från säkerhetskopior, se [återställa från säkerhetskopia](sql-database-recovery-using-backups.md)</span><span class="sxs-lookup"><span data-stu-id="7cb87-194">toolearn about restoring from backups, see [restore from backup](sql-database-recovery-using-backups.md)</span></span>
