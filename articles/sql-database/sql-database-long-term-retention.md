---
title: "aaaStore Azure SQL Database säkerhetskopieringar för too10 år | Microsoft Docs"
description: "Lär dig hur Azure SQL Database stöder lagra säkerhetskopior för too10 år."
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
ms.openlocfilehash: 5825ebd4e3bd66b59b13aea603d377ef814a1df3
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="store-azure-sql-database-backups-for-up-too10-years"></a><span data-ttu-id="070df-103">Säkerhetskopieringar Azure SQL Database för too10 år</span><span class="sxs-lookup"><span data-stu-id="070df-103">Store Azure SQL Database backups for up too10 years</span></span>
<span data-ttu-id="070df-104">Många program har regelverk, kompatibilitet eller andra företag syfte som kräver att du tooretain databassäkerhetskopieringar utöver hello 7-35 dagar som tillhandahålls av Azure SQL Database [automatiska säkerhetskopieringar](sql-database-automated-backups.md).</span><span class="sxs-lookup"><span data-stu-id="070df-104">Many applications have regulatory, compliance, or other business purposes that require you tooretain database backups beyond hello 7-35 days provided by Azure SQL Database [automatic backups](sql-database-automated-backups.md).</span></span> <span data-ttu-id="070df-105">Med funktionen hello långsiktig lagring av säkerhetskopior kan lagra du säkerhetskopiorna SQL-databasen i ett Azure Recovery Services-valv för too10 år.</span><span class="sxs-lookup"><span data-stu-id="070df-105">By using hello long-term backup retention feature, you can store your SQL database backups in an Azure Recovery Services vault for up too10 years.</span></span> <span data-ttu-id="070df-106">Du kan lagra upp too1 000 databaser per valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-106">You can store up too1,000 databases per vault.</span></span> <span data-ttu-id="070df-107">Du kan sedan välja någon säkerhetskopia i hello valvet toorestore den som en ny databas.</span><span class="sxs-lookup"><span data-stu-id="070df-107">You then can select any backup in hello vault toorestore it as a new database.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="070df-108">Långsiktig lagring av säkerhetskopior är för närvarande under förhandsgranskning och finns tillgänglig i hello följande områden: Östra Australien, sydost, södra, centrala USA, östra Asien, östra USA, östra USA 2, centrala Indien, södra Indien, östra, västra Japan, norra centrala USA, Nord Europa, södra centrala USA, Sydostasien, västra Europa och västra USA.</span><span class="sxs-lookup"><span data-stu-id="070df-108">Long-term backup retention is currently in preview and available in hello following regions: Australia East, Australia Southeast, Brazil South, Central US, East Asia, East US, East US 2, India Central, India South, Japan East, Japan West, North Central US, North Europe, South Central US, Southeast Asia, West Europe, and West US.</span></span>
>

> [!NOTE]
> <span data-ttu-id="070df-109">Du kan aktivera in too200 databaser per valvet under en 24-timmarsperiod.</span><span class="sxs-lookup"><span data-stu-id="070df-109">You can enable up too200 databases per vault during a 24-hour period.</span></span> <span data-ttu-id="070df-110">Vi rekommenderar att du använder ett separat valv för varje server toominimize hello effekten av den här gränsen.</span><span class="sxs-lookup"><span data-stu-id="070df-110">We recommend that you use a separate vault for each server toominimize hello impact of this limit.</span></span> 
> 

## <a name="how-sql-database-long-term-backup-retention-works"></a><span data-ttu-id="070df-111">Så här fungerar SQL Database långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="070df-111">How SQL Database long-term backup retention works</span></span>

<span data-ttu-id="070df-112">Du kan associera en SQL-databasserver med långsiktig säkerhetskopiering kvarhållning med Azure Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-112">With long-term backup retention, you can associate a SQL database server with an Azure Recovery Services vault.</span></span> 

* <span data-ttu-id="070df-113">Måste du skapa hello valvet i hello samma Azure-prenumeration som skapade hello SQLServer och hello i samma geografiska region och resurs-grupp.</span><span class="sxs-lookup"><span data-stu-id="070df-113">You must create hello vault in hello same Azure subscription that created hello SQL server and in hello same geographic region and resource group.</span></span> 
* <span data-ttu-id="070df-114">Sedan kan du konfigurera en bevarandeprincip för alla databaser.</span><span class="sxs-lookup"><span data-stu-id="070df-114">You then configure a retention policy for any database.</span></span> <span data-ttu-id="070df-115">hello princip orsaker hello veckovisa fullständiga databasen säkerhetskopieringar toobe kopieras toohello Recovery Services-valvet och bevaras under hello angivna kvarhållningsperiod (upp too10 år).</span><span class="sxs-lookup"><span data-stu-id="070df-115">hello policy causes hello weekly full database backups toobe copied toohello Recovery Services vault and retained for hello specified retention period (up too10 years).</span></span> 
* <span data-ttu-id="070df-116">Du kan sedan återställa hello databasen från någon av dessa säkerhetskopieringar tooa ny databas i någon server i hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="070df-116">You can then restore hello database from any of these backups tooa new database in any server in hello subscription.</span></span> <span data-ttu-id="070df-117">Azure storage skapar en kopia av befintliga säkerhetskopior och hello kopiera påverkar inte prestanda hello befintlig databas.</span><span class="sxs-lookup"><span data-stu-id="070df-117">Azure storage creates a copy from existing backups, and hello copy has no performance impact on hello existing database.</span></span>

> [!TIP]
> <span data-ttu-id="070df-118">En hur tooguide finns [konfigurera och återställning från Azure SQL Database långsiktig lagring av säkerhetskopior](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="070df-118">For a how-tooguide, see [Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="enable-long-term-backup-retention"></a><span data-ttu-id="070df-119">Aktivera långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="070df-119">Enable long-term backup retention</span></span>

<span data-ttu-id="070df-120">tooconfigure långsiktig lagring av säkerhetskopior för en databas:</span><span class="sxs-lookup"><span data-stu-id="070df-120">tooconfigure long-term backup retention for a database:</span></span>

1. <span data-ttu-id="070df-121">Skapa ett Azure Recovery Services-valv i hello samma region, prenumeration och resurs grupp som SQL-databasservern.</span><span class="sxs-lookup"><span data-stu-id="070df-121">Create an Azure Recovery Services vault in hello same region, subscription, and resource group as your SQL database server.</span></span> 
2. <span data-ttu-id="070df-122">Registrera hello server toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-122">Register hello server toohello vault.</span></span>
3. <span data-ttu-id="070df-123">Skapa en princip för Azure Recovery Services-skydd.</span><span class="sxs-lookup"><span data-stu-id="070df-123">Create an Azure Recovery Services protection policy.</span></span>
4. <span data-ttu-id="070df-124">Tillämpa hello skydd princip toohello databaser som kräver långsiktig säkerhetskopiering kvarhållning.</span><span class="sxs-lookup"><span data-stu-id="070df-124">Apply hello protection policy toohello databases that require long-term backup retention.</span></span>

<span data-ttu-id="070df-125">tooconfigure, hantera, och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="070df-125">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of hello following:</span></span>

* <span data-ttu-id="070df-126">Med hjälp av hello Azure-portalen: Klicka på **långsiktig lagring av säkerhetskopior**, väljer du en databas och klicka sedan på **konfigurera**.</span><span class="sxs-lookup"><span data-stu-id="070df-126">Using hello Azure portal: Click **Long-term backup retention**, select a database, and then click **Configure**.</span></span> 

   ![Välj en databas för långsiktig lagring av säkerhetskopior.](./media/sql-database-get-started-backup-recovery/select-database-for-long-term-backup-retention.png)

* <span data-ttu-id="070df-128">Med hjälp av PowerShell: Gå för[konfigurera och återställning från Azure SQL Database långsiktig lagring av säkerhetskopior](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="070df-128">Using PowerShell: Go too[Configure and restore from Azure SQL Database long-term backup retention](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="restore-a-database-thats-stored-with-hello-long-term-backup-retention-feature"></a><span data-ttu-id="070df-129">Återställa en databas som lagras med hello funktionen för långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="070df-129">Restore a database that's stored with hello long-term backup retention feature</span></span>

<span data-ttu-id="070df-130">toorecover från en säkerhetskopia för långsiktig lagring av säkerhetskopior.:</span><span class="sxs-lookup"><span data-stu-id="070df-130">toorecover from a long-term backup retention backup:</span></span>

1. <span data-ttu-id="070df-131">Lista hello valvet där hello säkerhetskopian lagras.</span><span class="sxs-lookup"><span data-stu-id="070df-131">List hello vault where hello backup is stored.</span></span>
2. <span data-ttu-id="070df-132">Lista hello behållare som är mappade tooyour logiska server.</span><span class="sxs-lookup"><span data-stu-id="070df-132">List hello container that is mapped tooyour logical server.</span></span>
3. <span data-ttu-id="070df-133">Lista hello datakälla inom hello valvet som är mappade tooyour databas.</span><span class="sxs-lookup"><span data-stu-id="070df-133">List hello data source within hello vault that is mapped tooyour database.</span></span>
4. <span data-ttu-id="070df-134">Lista hello Återställningspunkter som är tillgängliga toorestore.</span><span class="sxs-lookup"><span data-stu-id="070df-134">List hello recovery points that are available toorestore.</span></span>
5. <span data-ttu-id="070df-135">Återställa hello databas från återställning hello punkt toohello målservern i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="070df-135">Restore hello database from hello recovery point toohello target server within your subscription.</span></span>

<span data-ttu-id="070df-136">tooconfigure, hantera, och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatiska säkerhetskopieringar i ett Azure Recovery Services-valv, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="070df-136">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault, do either of hello following:</span></span>

* <span data-ttu-id="070df-137">Med hjälp av hello Azure-portalen: gå för[hantera långsiktig lagring av säkerhetskopior med hello Azure-portalen](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="070df-137">Using hello Azure portal: Go too[Manage long-term backup retention using hello Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="070df-138">Med hjälp av PowerShell: Gå för[hantera långsiktig lagring av säkerhetskopior med hjälp av PowerShell](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="070df-138">Using PowerShell: Go too[Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="get-pricing-for-long-term-backup-retention"></a><span data-ttu-id="070df-139">Hämta priser för långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="070df-139">Get pricing for long-term backup retention</span></span>

<span data-ttu-id="070df-140">Långsiktig säkerhetskopiering kvarhållning av en SQL-databas debiteras enligt toohello [Azure-säkerhetskopiering tjänster priser priser](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="070df-140">Long-term backup retention of a SQL database is charged according toohello [Azure backup services pricing rates](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="070df-141">När hello SQL database-server är registrerad toohello valvet, debiteras du för hello totalt lagringsutrymme som används av hello veckovisa säkerhetskopior lagras i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-141">After hello SQL database server is registered toohello vault, you are charged for hello total storage that's used by hello weekly backups stored in hello vault.</span></span>

## <a name="view-available-backups-that-are-stored-in-long-term-backup-retention"></a><span data-ttu-id="070df-142">Visa tillgängliga säkerhetskopior som lagras i långsiktig lagring av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="070df-142">View available backups that are stored in long-term backup retention</span></span>

<span data-ttu-id="070df-143">tooconfigure, hantera, och återställa en databas från långsiktig säkerhetskopiering kvarhållning av automatisk säkerhetskopiering i Azure Recovery Services-valvet via hello Azure-portalen, gör du något av följande hello:</span><span class="sxs-lookup"><span data-stu-id="070df-143">tooconfigure, manage, and restore a database from long-term backup retention of automated backups in an Azure Recovery Services vault by using hello Azure portal, do either of hello following:</span></span>

* <span data-ttu-id="070df-144">Med hjälp av hello Azure-portalen: gå för[hantera långsiktig lagring av säkerhetskopior med hello Azure-portalen](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="070df-144">Using hello Azure portal: Go too[Manage long-term backup retention using hello Azure portal](sql-database-long-term-backup-retention-configure.md).</span></span> 

* <span data-ttu-id="070df-145">Med hjälp av PowerShell: Gå för[hantera långsiktig lagring av säkerhetskopior med hjälp av PowerShell](sql-database-long-term-backup-retention-configure.md).</span><span class="sxs-lookup"><span data-stu-id="070df-145">Using PowerShell: Go too[Manage long-term backup retention using PowerShell](sql-database-long-term-backup-retention-configure.md).</span></span>

## <a name="disable-long-term-retention"></a><span data-ttu-id="070df-146">Inaktivera långsiktig kvarhållning</span><span class="sxs-lookup"><span data-stu-id="070df-146">Disable long-term retention</span></span>

<span data-ttu-id="070df-147">hello återställningstjänsten hanterar automatiskt hello rensning av säkerhetskopieringar utifrån hello tillhandahålls bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="070df-147">hello recovery service automatically handles hello cleanup of backups based on hello provided retention policy.</span></span> 

<span data-ttu-id="070df-148">toostop skicka hello säkerhetskopieringar för en viss databas toohello valvet, ta bort hello bevarandeprincip för den här databasen.</span><span class="sxs-lookup"><span data-stu-id="070df-148">toostop sending hello backups for a specific database toohello vault, remove hello retention policy for that database.</span></span>
  
```
Set-AzureRmSqlDatabaseBackupLongTermRetentionPolicy -ResourceGroupName 'RG1' -ServerName 'Server1' -DatabaseName 'DB1' -State 'Disabled' -ResourceId $policy.Id
```

> [!NOTE]
> <span data-ttu-id="070df-149">hello säkerhetskopieringar som redan finns i hello valvet påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="070df-149">hello backups that are already in hello vault are unaffected.</span></span> <span data-ttu-id="070df-150">De tas bort automatiskt av hello recovery-tjänsten när deras kvarhållningsperiod upphör att gälla.</span><span class="sxs-lookup"><span data-stu-id="070df-150">They are automatically deleted by hello recovery service when their retention period expires.</span></span>

## <a name="long-term-backup-retention-faq"></a><span data-ttu-id="070df-151">Långsiktig lagring av säkerhetskopior vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="070df-151">Long-term backup retention FAQ</span></span>

<span data-ttu-id="070df-152">**Kan jag manuellt ta bort specifika säkerhetskopior i hello valvet?**</span><span class="sxs-lookup"><span data-stu-id="070df-152">**Can I manually delete specific backups in hello vault?**</span></span>

<span data-ttu-id="070df-153">För närvarande inte.</span><span class="sxs-lookup"><span data-stu-id="070df-153">Not currently.</span></span> <span data-ttu-id="070df-154">hello valvet rensas automatiskt säkerhetskopior när hello kvarhållningsperiod har upphört att gälla.</span><span class="sxs-lookup"><span data-stu-id="070df-154">hello vault automatically cleans up backups when hello retention period has expired.</span></span>

<span data-ttu-id="070df-155">**Kan jag registrera Min server toostore säkerhetskopieringar toomore än en valvet?**</span><span class="sxs-lookup"><span data-stu-id="070df-155">**Can I register my server toostore backups toomore than one vault?**</span></span>

<span data-ttu-id="070df-156">Nej, kan du för närvarande lagra säkerhetskopior tooonly ett valv i taget.</span><span class="sxs-lookup"><span data-stu-id="070df-156">No, you can currently store backups tooonly one vault at a time.</span></span>

<span data-ttu-id="070df-157">**Kan jag ha ett valv och server för olika prenumerationer?**</span><span class="sxs-lookup"><span data-stu-id="070df-157">**Can I have a vault and server in different subscriptions?**</span></span>

<span data-ttu-id="070df-158">Nej, för närvarande hello valvet och servern måste inneha hello samma prenumeration och resurs-grupp.</span><span class="sxs-lookup"><span data-stu-id="070df-158">No, currently hello vault and server must be in hello same subscription and resource group.</span></span>

<span data-ttu-id="070df-159">**Kan jag använda ett valv som skapats i en region som skiljer sig från Min server region?**</span><span class="sxs-lookup"><span data-stu-id="070df-159">**Can I use a vault that I created in a region that's different from my server’s region?**</span></span>

<span data-ttu-id="070df-160">Nej, hello valvet och servern måste vara i hello samma region toominimize kopiera tid och undvika trafik debiteringar.</span><span class="sxs-lookup"><span data-stu-id="070df-160">No, hello vault and server must be in hello same region toominimize copy time and avoid traffic charges.</span></span>

<span data-ttu-id="070df-161">**Hur många databaser kan lagra i ett valv?**</span><span class="sxs-lookup"><span data-stu-id="070df-161">**How many databases can I store in one vault?**</span></span>

<span data-ttu-id="070df-162">Vi stöder för närvarande in too1 000 databaser per valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-162">Currently, we support up too1,000 databases per vault.</span></span> 

<span data-ttu-id="070df-163">**Hur många valv kan skapa per prenumeration?**</span><span class="sxs-lookup"><span data-stu-id="070df-163">**How many vaults can I create per subscription?**</span></span>

<span data-ttu-id="070df-164">Du kan skapa upp too25 valv per prenumeration.</span><span class="sxs-lookup"><span data-stu-id="070df-164">You can create up too25 vaults per subscription.</span></span>

<span data-ttu-id="070df-165">**Hur många databaser kan konfigurera per dag per valvet?**</span><span class="sxs-lookup"><span data-stu-id="070df-165">**How many databases can I configure per day per vault?**</span></span>

<span data-ttu-id="070df-166">Du kan ställa in 200 databaser per dag per valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-166">You can set up 200 databases per day per vault.</span></span>

<span data-ttu-id="070df-167">**Fungerar långsiktig lagring av säkerhetskopior med elastiska pooler?**</span><span class="sxs-lookup"><span data-stu-id="070df-167">**Does long-term backup retention work with elastic pools?**</span></span>

<span data-ttu-id="070df-168">Ja.</span><span class="sxs-lookup"><span data-stu-id="070df-168">Yes.</span></span> <span data-ttu-id="070df-169">Alla databaser i poolen hello kan konfigureras med hello bevarandeprincip.</span><span class="sxs-lookup"><span data-stu-id="070df-169">Any database in hello pool can be configured with hello retention policy.</span></span>

<span data-ttu-id="070df-170">**Kan jag välja hello tid då hello säkerhetskopian?**</span><span class="sxs-lookup"><span data-stu-id="070df-170">**Can I choose hello time at which hello backup is created?**</span></span>

<span data-ttu-id="070df-171">Nej, SQL-databas kontrollerar hello Säkerhetskopieringsschemat toominimize hello inverkan på prestanda på dina databaser.</span><span class="sxs-lookup"><span data-stu-id="070df-171">No, SQL Database controls hello backup schedule toominimize hello performance impact on your databases.</span></span>

<span data-ttu-id="070df-172">**Jag har transparent datakryptering är aktiverat för databasen. Kan jag använda den med hello valvet?**</span><span class="sxs-lookup"><span data-stu-id="070df-172">**I have transparent data encryption enabled for my database. Can I use it with hello vault?**</span></span> 

<span data-ttu-id="070df-173">Ja, transparent datakryptering stöds.</span><span class="sxs-lookup"><span data-stu-id="070df-173">Yes, transparent data encryption is supported.</span></span> <span data-ttu-id="070df-174">Du kan återställa hello databasen från hello valvet även om hello ursprungliga databasen finns inte längre.</span><span class="sxs-lookup"><span data-stu-id="070df-174">You can restore hello database from hello vault even if hello original database no longer exists.</span></span>

<span data-ttu-id="070df-175">**Vad händer med hello säkerhetskopieringar i hello valvet om min prenumeration har inaktiverats?**</span><span class="sxs-lookup"><span data-stu-id="070df-175">**What happens with hello backups in hello vault if my subscription is suspended?**</span></span> 

<span data-ttu-id="070df-176">Om din prenumeration har inaktiverats behålla vi hello befintliga databaser och säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="070df-176">If your subscription is suspended, we retain hello existing databases and backups.</span></span> <span data-ttu-id="070df-177">Nya säkerhetskopior är inte kopierade toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-177">New backups are not copied toohello vault.</span></span> <span data-ttu-id="070df-178">När du återaktivera hello prenumeration återupptas hello service kopiera säkerhetskopieringar toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="070df-178">After you reactivate hello subscription, hello service resumes copying backups toohello vault.</span></span> <span data-ttu-id="070df-179">Ditt valv blir tillgänglig toohello återställningsåtgärder med hjälp av hello säkerhetskopior som har kopierats det innan hello prenumeration pausades.</span><span class="sxs-lookup"><span data-stu-id="070df-179">Your vault becomes accessible toohello restore operations by using hello backups that were copied there before hello subscription was suspended.</span></span> 

<span data-ttu-id="070df-180">**Kan jag få åtkomst till toohello SQL-databasens säkerhetskopierade filer så att jag kan hämta eller återställa dem toohello SQLServer?**</span><span class="sxs-lookup"><span data-stu-id="070df-180">**Can I get access toohello SQL database backup files so I can download or restore them toohello SQL server?**</span></span>

<span data-ttu-id="070df-181">Nej, inte för närvarande.</span><span class="sxs-lookup"><span data-stu-id="070df-181">No, not currently.</span></span>

<span data-ttu-id="070df-182">**Är det möjligt toohave flera schemalägger (varje dag, varje vecka, månad, varje år) i en SQL-bevarandeprincip.**</span><span class="sxs-lookup"><span data-stu-id="070df-182">**Is it possible toohave multiple schedules (daily, weekly, monthly, yearly) within a SQL retention policy.**</span></span>

<span data-ttu-id="070df-183">Ingen, flera scheman är endast tillgänglig för säkerhetskopiering för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="070df-183">No, multiple schedules are currently available only for virtual machine backups.</span></span>

<span data-ttu-id="070df-184">**Vad händer om vi konfigurerar långsiktig lagring av säkerhetskopior för en databas som är en databasdagar för sekundär aktiv geo-replikering?**</span><span class="sxs-lookup"><span data-stu-id="070df-184">**What if we set up long-term backup retention on a database that is an active geo-replication secondary database?**</span></span>

<span data-ttu-id="070df-185">Eftersom vi inte göra säkerhetskopior på repliker, finns det inget alternativ för långsiktig lagring av säkerhetskopior på sekundära databaser.</span><span class="sxs-lookup"><span data-stu-id="070df-185">Because we don't take backups on replicas, there is currently no option for long-term backup retention on secondary databases.</span></span> <span data-ttu-id="070df-186">Men är det viktigt för användare tooset upp långsiktig lagring av säkerhetskopior på en databasdagar för sekundär aktiv geo-replikering för följande:</span><span class="sxs-lookup"><span data-stu-id="070df-186">However, it is important for users tooset up long-term backup retention on an active geo-replication secondary database for these reasons:</span></span>
* <span data-ttu-id="070df-187">När en växling sker och hello databasen blir en primär databas, tar vi en fullständig säkerhetskopiering, vilket är överförda toovault.</span><span class="sxs-lookup"><span data-stu-id="070df-187">When a failover happens and hello database becomes a primary database, we take a full backup, which is uploaded toovault.</span></span>
* <span data-ttu-id="070df-188">Det finns inga extra kostnad toohello kund för att ställa in långsiktig lagring av säkerhetskopior på en sekundär databas.</span><span class="sxs-lookup"><span data-stu-id="070df-188">There is no extra cost toohello customer for setting up long-term backup retention on a secondary database.</span></span>

## <a name="next-steps"></a><span data-ttu-id="070df-189">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="070df-189">Next steps</span></span>
<span data-ttu-id="070df-190">Eftersom databassäkerhetskopieringar skydda data från oavsiktliga skador eller tas bort, är de en viktig del av alla affärskontinuitet och haveriberedskap.</span><span class="sxs-lookup"><span data-stu-id="070df-190">Because database backups protect data from accidental corruption or deletion, they're an essential part of any business continuity and disaster recovery strategy.</span></span> <span data-ttu-id="070df-191">toolearn om Hej andra SQL-databas kontinuitet för företag-lösningar, se [översikt över verksamhetskontinuitet](sql-database-business-continuity.md).</span><span class="sxs-lookup"><span data-stu-id="070df-191">toolearn about hello other SQL Database business-continuity solutions, see [Business continuity overview](sql-database-business-continuity.md).</span></span>
