---
title: "Azure SQL Database säkerhetskopieringar för upp till 10 år | Microsoft Docs"
description: "Lär dig hur Azure SQL Database stöder lagra säkerhetskopior för upp till 10 år."
keywords: 
services: sql-database
documentationcenter: 
author: anosov1960
manager: jhubbard
editor: 
ms.assetid: 66fdb8b8-5903-4d3a-802e-af08d204566e
ms.service: sql-database
ms.custom: business continuity
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: NA
ms.date: 12/22/2016
ms.author: sashan
ms.openlocfilehash: 25e651203f804fbf32d632b5f83145a3f3f72a7f
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="store-azure-sql-database-backups-for-up-to-10-years"></a><span data-ttu-id="1dff4-103">Azure SQL Database säkerhetskopieringar för upp till 10 år</span><span class="sxs-lookup"><span data-stu-id="1dff4-103">Store Azure SQL Database backups for up to 10 years</span></span>
<span data-ttu-id="1dff4-104">Många program har regelverk, kompatibilitet eller andra företag syfte som kräver att du behåller databassäkerhetskopieringar utöver de 7-35 dagar som tillhandahålls av Azure SQL Database [automatiska säkerhetskopieringar](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-104">Many applications have regulatory, compliance, or other business purposes that require you to retain database backups beyond the 7-35 days provided by Azure SQL Database [automatic backups](sql-database-automated-backups.md).</span></span> <span data-ttu-id="1dff4-105">Med hjälp av funktionen för långsiktig lagring av säkerhetskopior kan du lagra säkerhetskopiorna SQL-databasen i ett Azure Recovery Services-valv i upp till 10 år.</span><span class="sxs-lookup"><span data-stu-id="1dff4-105">By using the long-term backup retention feature, you can store your SQL database backups in an Azure Recovery Services vault for up to 10 years.</span></span> <span data-ttu-id="1dff4-106">Du kan lagra upp till 1 000 databaser per valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-106">You can store up to 1,000 databases per vault.</span></span> <span data-ttu-id="1dff4-107">Du kan sedan välja någon säkerhetskopia i valvet för att återställa den som en ny databas.</span><span class="sxs-lookup"><span data-stu-id="1dff4-107">You then can select any backup in the vault to restore it as a new database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="1dff4-108">Långsiktig lagring av säkerhetskopior är för närvarande under förhandsgranskning och tillgänglig inom följande områden: östra, Australien, sydost, södra, centrala USA, östra Asien, östra USA, östra USA 2, centrala Indien, södra Indien, östra, västra Japan, norra centrala USA, Nordeuropa, södra centrala USA, Sydostasien, västra Europa, och västra USA.</span><span class="sxs-lookup"><span data-stu-id="1dff4-108">Long-term backup retention is currently in preview and available in the following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.</span></span>
>

> [!NOTE]
> <span data-ttu-id="1dff4-109">Du kan aktivera upp till 200 databaser per valvet under en 24-timmarsperiod.</span><span class="sxs-lookup"><span data-stu-id="1dff4-109">You can enable up to 200 databases per vault during a 24-hour period.</span></span> <span data-ttu-id="1dff4-110">Vi rekommenderar att du använder ett separat valv för varje server för att minimera effekten av den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="1dff4-110">We recommend that you use a separate vault for each server to minimize the impact of this limit.</span></span> 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a><span data-ttu-id="1dff4-111">Så här fungerar SQL Database långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="1dff4-111">How SQL Database long-term backup retention works</span></span>

<span data-ttu-id="1dff4-112">Du kan associera en SQL-databasserver med långsiktig säkerhetskopiering kvarhållning med Azure Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-112">With long-term backup retention, you can associate a SQL database server with an Azure Recovery Services vault.</span></span> 

* <span data-ttu-id="1dff4-113">Du måste skapa valvet i samma Azure-prenumerationen som skapats av SQLServer och i samma geografiska region och resursgruppen.</span><span class="sxs-lookup"><span data-stu-id="1dff4-113">You must create the vault in the same Azure subscription that created the SQL server and in the same geographic region and resource group.</span></span> 
* <span data-ttu-id="1dff4-114">Sedan kan du konfigurera en bevarandeprincip för alla databaser.</span><span class="sxs-lookup"><span data-stu-id="1dff4-114">You then configure a retention policy for any database.</span></span> <span data-ttu-id="1dff4-115">Principen gör veckovisa fullständiga databassäkerhetskopieringar som ska kopieras till Recovery Services-valvet och bevaras under den angivna bevarandeperioden (upp till 10 år).</span><span class="sxs-lookup"><span data-stu-id="1dff4-115">The policy causes the weekly full database backups to be copied to the Recovery Services vault and retained for the specified retention period (up to 10 years).</span></span> 
* <span data-ttu-id="1dff4-116">Du kan sedan återställa databasen från någon av dessa säkerhetskopieringar till en ny databas i någon server i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="1dff4-116">You can then restore the database from any of these backups to a new database in any server in the subscription.</span></span> <span data-ttu-id="1dff4-117">Azure storage skapar en kopia av befintliga säkerhetskopior och kopian påverkar inte prestanda på den befintliga databasen.</span><span class="sxs-lookup"><span data-stu-id="1dff4-117">Azure storage creates a copy from existing backups, and the copy has no performance impact on the existing database.</span></span>

> [!TIP]
> <span data-ttu-id="1dff4-118">Instruktioner, se [konfigurera och återställning från Azure SQL Database långsiktig lagring av säkerhetskopior](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-118">For a how-to guide, see [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="enable-long-term-backup-retention"></a><span data-ttu-id="1dff4-119">Aktivera långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="1dff4-119">Enable long-term backup retention</span></span>

<span data-ttu-id="1dff4-120">Konfigurera långsiktig lagring av säkerhetskopior för en databas:</span><span class="sxs-lookup"><span data-stu-id="1dff4-120">To configure long-term backup retention for a database:</span></span>

1. <span data-ttu-id="1dff4-121">Skapa ett Azure Recovery Services-valv som din SQL database-server i samma region, prenumeration och resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1dff4-121">Create an Azure Recovery Services vault in the same region, subscription, and resource group as your SQL database server.</span></span> 
2. <span data-ttu-id="1dff4-122">Registrera servern i valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-122">Register the server to the vault.</span></span>
3. <span data-ttu-id="1dff4-123">Skapa en princip för Azure Recovery Services-skydd.</span><span class="sxs-lookup"><span data-stu-id="1dff4-123">Create an Azure Recovery Services protection policy.</span></span>
4. <span data-ttu-id="1dff4-124">Gäller protection-principen för de databaser som kräver långsiktig säkerhetskopiering kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="1dff4-124">Apply the protection policy to the databases that require long-term backup retention.</span></span>

<span data-ttu-id="1dff4-125">Om du vill konfigurera, hantera och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv, gör du något av följande:</span><span class="sxs-lookup"><span data-stu-id="1dff4-125">To configure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of the following:</span></span>

* <span data-ttu-id="1dff4-126">Med hjälp av Azure portal: Klicka på **långsiktig lagring av säkerhetskopior**, väljer du en databas och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="1dff4-126">Using the Azure portal: Click **Long-term backup retention**, select a database, and then click **Configure**.</span></span> 

   ![Välj en databas för långsiktig lagring av säkerhetskopior.](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* <span data-ttu-id="1dff4-128">Med hjälp av PowerShell: Gå till [konfigurera och återställning från Azure SQL Database långsiktig lagring av säkerhetskopior](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-128">Using PowerShell: Go to [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="restore-a-database-thats-stored-with-the-long-term-backup-retention-feature"></a><span data-ttu-id="1dff4-129">Återställa en databas som lagras med funktionen för långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="1dff4-129">Restore a database that's stored with the long-term backup retention feature</span></span>

<span data-ttu-id="1dff4-130">För att återställa från en säkerhetskopia för långsiktig lagring av säkerhetskopior.:</span><span class="sxs-lookup"><span data-stu-id="1dff4-130">To recover from a long-term backup retention backup:</span></span>

1. <span data-ttu-id="1dff4-131">Lista över valvet där säkerhetskopian lagras.</span><span class="sxs-lookup"><span data-stu-id="1dff4-131">List the vault where the backup is stored.</span></span>
2. <span data-ttu-id="1dff4-132">Lista över behållare som är mappad till din logiska server.</span><span class="sxs-lookup"><span data-stu-id="1dff4-132">List the container that is mapped to your logical server.</span></span>
3. <span data-ttu-id="1dff4-133">Lista över datakälla i valvet som är mappad till din databas.</span><span class="sxs-lookup"><span data-stu-id="1dff4-133">List the data source within the vault that is mapped to your database.</span></span>
4. <span data-ttu-id="1dff4-134">Lista över de återställningspunkter som är tillgängliga för återställning.</span><span class="sxs-lookup"><span data-stu-id="1dff4-134">List the recovery points that are available to restore.</span></span>
5. <span data-ttu-id="1dff4-135">Återställ databasen från återställningspunkten till servern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1dff4-135">Restore the database from the recovery point to the target server within your subscription.</span></span>

<span data-ttu-id="1dff4-136">Om du vill konfigurera, hantera och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv, gör du något av följande:</span><span class="sxs-lookup"><span data-stu-id="1dff4-136">To configure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of the following:</span></span>

* <span data-ttu-id="1dff4-137">Med hjälp av Azure portal: Gå till [hantera långsiktig lagring av säkerhetskopior med Azure-portalen](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-137">Using the Azure portal: Go to [Manage long-term backup retention using the Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="1dff4-138">Med hjälp av PowerShell: Gå till [hantera långsiktig lagring av säkerhetskopior med hjälp av PowerShell](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-138">Using PowerShell: Go to [Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="get-pricing-for-long-term-backup-retention"></a><span data-ttu-id="1dff4-139">Hämta priser för långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="1dff4-139">Get pricing for long-term backup retention</span></span>

<span data-ttu-id="1dff4-140">Långsiktig säkerhetskopiering kvarhållning av en SQL-databas debiteras enligt den [Azure-säkerhetskopiering tjänster priser priser](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="1dff4-140">Long-term backup retention of a SQL database is charged according to the [Azure backup services pricing rates](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="1dff4-141">När SQL-databasservern är registrerad för valvet, debiteras du för det totala lagringsutrymmet som används av de veckovisa säkerhetskopiorna som lagras i valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-141">After the SQL database server is registered to the vault, you are charged for the total storage that's used by the weekly backups stored in the vault.</span></span>

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a><span data-ttu-id="1dff4-142">Visa tillgängliga säkerhetskopior som lagras i långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="1dff4-142">View available backups that are stored in long-term backup retention</span></span>

<span data-ttu-id="1dff4-143">Om du vill konfigurera, hantera och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv med hjälp av Azure portal, gör du något av följande:</span><span class="sxs-lookup"><span data-stu-id="1dff4-143">To configure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault by using the Azure portal, do either of the following:</span></span>

* <span data-ttu-id="1dff4-144">Med hjälp av Azure portal: Gå till [hantera långsiktig lagring av säkerhetskopior med Azure-portalen](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-144">Using the Azure portal: Go to [Manage long-term backup retention using the Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="1dff4-145">Med hjälp av PowerShell: Gå till [hantera långsiktig lagring av säkerhetskopior med hjälp av PowerShell](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-145">Using PowerShell: Go to [Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="disable-long-term-retention"></a><span data-ttu-id="1dff4-146">Inaktivera långsiktig kvarhållning</span><span class="sxs-lookup"><span data-stu-id="1dff4-146">Disable long-term retention</span></span>

<span data-ttu-id="1dff4-147">Tjänsten recovery hanterar automatiskt rensning av säkerhetskopior baserat på den angivna bevarandeprincipen.</span><span class="sxs-lookup"><span data-stu-id="1dff4-147">The recovery service automatically handles the cleanup of backups based on the provided retention policy.</span></span> 

<span data-ttu-id="1dff4-148">Ta bort bevarandeprincip för den här databasen om du vill sluta skicka säkerhetskopieringar för en viss databas till valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-148">To stop sending the backups for a specific database to the vault, remove the retention policy for that database.</span></span>
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> <span data-ttu-id="1dff4-149">Säkerhetskopieringar som redan finns i valvet påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="1dff4-149">The backups that are already in the vault are unaffected.</span></span> <span data-ttu-id="1dff4-150">De tas bort automatiskt av återställningstjänsten när deras kvarhållningsperiod upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="1dff4-150">They are automatically deleted by the recovery service when their retention period expires.</span></span>

## <a name="long-term-backup-retention-faq"></a><span data-ttu-id="1dff4-151">Långsiktig lagring av säkerhetskopior vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="1dff4-151">Long-term backup retention FAQ</span></span>

<span data-ttu-id="1dff4-152">**Kan jag manuellt ta bort specifika säkerhetskopior i valvet?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-152">**Can I manually delete specific backups in the vault?**</span></span>

<span data-ttu-id="1dff4-153">För närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="1dff4-153">Not currently.</span></span> <span data-ttu-id="1dff4-154">Valvet rensas automatiskt säkerhetskopior när kvarhållningsperioden har gått ut.</span><span class="sxs-lookup"><span data-stu-id="1dff4-154">The vault automatically cleans up backups when the retention period has expired.</span></span>

<span data-ttu-id="1dff4-155">**Kan jag registrera Min server för att lagra säkerhetskopior till flera valv?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-155">**Can I register my server to store backups to more than one vault?**</span></span>

<span data-ttu-id="1dff4-156">Nej, du kan för närvarande säkerhetskopieringar till endast en valvet i taget.</span><span class="sxs-lookup"><span data-stu-id="1dff4-156">No, you can currently store backups to only one vault at a time.</span></span>

<span data-ttu-id="1dff4-157">**Kan jag ha ett valv och server för olika prenumerationer?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-157">**Can I have a vault and server in different subscriptions?**</span></span>

<span data-ttu-id="1dff4-158">Nej, för närvarande på valvet och servern måste vara i samma prenumeration och resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="1dff4-158">No, currently the vault and server must be in the same subscription and resource group.</span></span>

<span data-ttu-id="1dff4-159">**Kan jag använda ett valv som skapats i en region som skiljer sig från Min server region?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-159">**Can I use a vault that I created in a region that's different from my server’s region?**</span></span>

<span data-ttu-id="1dff4-160">Nej, valvet och servern måste vara i samma region för att minimera kopiera tid och undvika avgifterna för trafiken.</span><span class="sxs-lookup"><span data-stu-id="1dff4-160">No, the vault and server must be in the same region to minimize copy time and avoid traffic charges.</span></span>

<span data-ttu-id="1dff4-161">**Hur många databaser kan lagra i ett valv?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-161">**How many databases can I store in one vault?**</span></span>

<span data-ttu-id="1dff4-162">Vi stöder för närvarande upp till 1 000 databaser per valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-162">Currently, we support up to 1,000 databases per vault.</span></span> 

<span data-ttu-id="1dff4-163">**Hur många valv kan skapa per prenumeration?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-163">**How many vaults can I create per subscription?**</span></span>

<span data-ttu-id="1dff4-164">Du kan skapa upp till 25 valv per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="1dff4-164">You can create up to 25 vaults per subscription.</span></span>

<span data-ttu-id="1dff4-165">**Hur många databaser kan konfigurera per dag per valvet?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-165">**How many databases can I configure per day per vault?**</span></span>

<span data-ttu-id="1dff4-166">Du kan ställa in 200 databaser per dag per valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-166">You can set up 200 databases per day per vault.</span></span>

<span data-ttu-id="1dff4-167">**Fungerar långsiktig lagring av säkerhetskopior med elastiska pooler?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-167">**Does long-term backup retention work with elastic pools?**</span></span>

<span data-ttu-id="1dff4-168">Ja.</span><span class="sxs-lookup"><span data-stu-id="1dff4-168">Yes.</span></span> <span data-ttu-id="1dff4-169">Alla databaser i poolen kan konfigureras med bevarandeprincipen.</span><span class="sxs-lookup"><span data-stu-id="1dff4-169">Any database in the pool can be configured with the retention policy.</span></span>

<span data-ttu-id="1dff4-170">**Kan jag välja den tid då säkerhetskopian skapades?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-170">**Can I choose the time at which the backup is created?**</span></span>

<span data-ttu-id="1dff4-171">Nej, SQL-databas kontrollerar schemat för säkerhetskopiering för att minimera påverkan på prestandan för dina databaser.</span><span class="sxs-lookup"><span data-stu-id="1dff4-171">No, SQL Database controls the backup schedule to minimize the performance impact on your databases.</span></span>

<span data-ttu-id="1dff4-172">**Jag har transparent datakryptering är aktiverat för databasen. Kan jag använda den med valvet?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-172">**I have transparent data encryption enabled for my database. Can I use it with the vault?**</span></span> 

<span data-ttu-id="1dff4-173">Ja, transparent datakryptering stöds.</span><span class="sxs-lookup"><span data-stu-id="1dff4-173">Yes, transparent data encryption is supported.</span></span> <span data-ttu-id="1dff4-174">Du kan återställa databasen från valvet, även om den ursprungliga databasen finns inte längre.</span><span class="sxs-lookup"><span data-stu-id="1dff4-174">You can restore the database from the vault even if the original database no longer exists.</span></span>

<span data-ttu-id="1dff4-175">**Vad händer med säkerhetskopieringar i valvet om min prenumeration har inaktiverats?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-175">**What happens with the backups in the vault if my subscription is suspended?**</span></span> 

<span data-ttu-id="1dff4-176">Om din prenumeration har inaktiverats behålla det befintliga databaser och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="1dff4-176">If your subscription is suspended, we retain the existing databases and backups.</span></span> <span data-ttu-id="1dff4-177">Nya säkerhetskopior kopieras inte till valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-177">New backups are not copied to the vault.</span></span> <span data-ttu-id="1dff4-178">När du återaktivera prenumerationen återupptas tjänsten kopiera säkerhetskopiering till valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-178">After you reactivate the subscription, the service resumes copying backups to the vault.</span></span> <span data-ttu-id="1dff4-179">Ditt valv blir tillgänglig för återställningsåtgärder med hjälp av säkerhetskopior som har kopierats det innan prenumerationen pausades.</span><span class="sxs-lookup"><span data-stu-id="1dff4-179">Your vault becomes accessible to the restore operations by using the backups that were copied there before the subscription was suspended.</span></span> 

<span data-ttu-id="1dff4-180">**Kan jag få åtkomst till SQL-databasfilerna säkerhetskopiering så att jag kan hämta eller återställa dem till SQLServer?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-180">**Can I get access to the SQL database backup files so I can download or restore them to the SQL server?**</span></span>

<span data-ttu-id="1dff4-181">Nej, inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="1dff4-181">No, not currently.</span></span>

<span data-ttu-id="1dff4-182">**Är det möjligt att ha flera scheman (varje dag, vecka, månad, varje år) i en SQL-bevarandeprincip.**</span><span class="sxs-lookup"><span data-stu-id="1dff4-182">**Is it possible to have multiple schedules (daily, weekly, monthly, yearly) within a SQL retention policy.**</span></span>

<span data-ttu-id="1dff4-183">Ingen, flera scheman är endast tillgänglig för säkerhetskopiering för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1dff4-183">No, multiple schedules are currently available only for virtual machine backups.</span></span>

<span data-ttu-id="1dff4-184">**Vad händer om vi konfigurerar långsiktig lagring av säkerhetskopior för en databas som är en databasdagar för sekundär aktiv geo-replikering?**</span><span class="sxs-lookup"><span data-stu-id="1dff4-184">**What if we set up long-term backup retention on a database that is an active geo-replication secondary database?**</span></span>

<span data-ttu-id="1dff4-185">Eftersom vi inte göra säkerhetskopior på repliker, finns det inget alternativ för långsiktig lagring av säkerhetskopior på sekundära databaser.</span><span class="sxs-lookup"><span data-stu-id="1dff4-185">Because we don't take backups on replicas, there is currently no option for long-term backup retention on secondary databases.</span></span> <span data-ttu-id="1dff4-186">Det är dock viktigt för användare att ställa in långsiktig lagring av säkerhetskopior på en databasdagar för sekundär aktiv geo-replikering av följande skäl:</span><span class="sxs-lookup"><span data-stu-id="1dff4-186">However, it is important for users to set up long-term backup retention on an active geo-replication secondary database for these reasons:</span></span>
* <span data-ttu-id="1dff4-187">När en växling sker och databasen blir en primär databas, tar vi en fullständig säkerhetskopiering, som har överförts till valvet.</span><span class="sxs-lookup"><span data-stu-id="1dff4-187">When a failover happens and the database becomes a primary database, we take a full backup, which is uploaded to vault.</span></span>
* <span data-ttu-id="1dff4-188">Det kostar ingenting extra till kunden för att ställa in långsiktig lagring av säkerhetskopior på en sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="1dff4-188">There is no extra cost to the customer for setting up long-term backup retention on a secondary database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="1dff4-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1dff4-189">Next steps</span></span>
<span data-ttu-id="1dff4-190">Eftersom databassäkerhetskopieringar skydda data från oavsiktliga skador eller tas bort, är de en viktig del av alla affärskontinuitet och haveriberedskap.</span><span class="sxs-lookup"><span data-stu-id="1dff4-190">Because database backups protect data from accidental corruption or deletion, they're an essential part of any business continuity and disaster recovery strategy.</span></span> <span data-ttu-id="1dff4-191">Mer information om de andra SQL-databas kontinuitet för företag-lösningarna, se [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="1dff4-191">To learn about the other SQL Database business-continuity solutions, see [Business continuity overview](sql-database-business-continuity.md).</span></span>
