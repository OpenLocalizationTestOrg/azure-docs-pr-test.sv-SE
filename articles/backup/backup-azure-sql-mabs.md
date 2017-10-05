---
title: "Azure-säkerhetskopiering av SQL Server-arbetsbelastningar med Azure Backup Server | Microsoft Docs"
description: "En introduktion till att säkerhetskopiera SQL Server-databaser med Azure Backup Server"
services: backup
documentationcenter: 
author: pvrk
manager: Shivamg
editor: 
ms.assetid: c8b1f7ec-26b1-4ef0-a3f2-91aec959daea
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: pullabhk
ms.openlocfilehash: 2af9ebaa8f52690ed63406cbd85b77544d2d900d
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="back-up-sql-server-to-azure-with-azure-backup-server"></a><span data-ttu-id="96002-103">Säkerhetskopiera SQL Server till Azure med Azure Backup Server</span><span class="sxs-lookup"><span data-stu-id="96002-103">Back up SQL Server to Azure With Azure Backup Server</span></span>
<span data-ttu-id="96002-104">Den här artikeln leder dig igenom konfigurationsstegen för säkerhetskopiering av SQL Server-databaser med Microsoft Azure Backup Server (MABS).</span><span class="sxs-lookup"><span data-stu-id="96002-104">This article leads you through the configuration steps for backup of SQL Server databases using Microsoft Azure Backup Server (MABS).</span></span>

<span data-ttu-id="96002-105">Hantering av säkerhetskopiering av SQL Server-databas till Azure och återställning från Azure omfattar tre steg:</span><span class="sxs-lookup"><span data-stu-id="96002-105">The management of SQL Server database backup to Azure and recovery from Azure involves three steps:</span></span>

1. <span data-ttu-id="96002-106">Skapa en princip för säkerhetskopiering för att skydda SQL Server-databaser till Azure.</span><span class="sxs-lookup"><span data-stu-id="96002-106">Create a backup policy to protect SQL Server databases to Azure.</span></span>
2. <span data-ttu-id="96002-107">Skapa säkerhetskopior för på-begäran till Azure.</span><span class="sxs-lookup"><span data-stu-id="96002-107">Create on-demand backup copies to Azure.</span></span>
3. <span data-ttu-id="96002-108">Återställ databasen från Azure.</span><span class="sxs-lookup"><span data-stu-id="96002-108">Recover the database from Azure.</span></span>

## <a name="before-you-start"></a><span data-ttu-id="96002-109">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="96002-109">Before you start</span></span>
<span data-ttu-id="96002-110">Innan du börjar bör du kontrollera att du har [installerad och förberedda Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="96002-110">Before you begin, ensure that you have [installed and prepared the Azure Backup Server](backup-azure-microsoft-azure-backup.md).</span></span>

## <a name="create-a-backup-policy-to-protect-sql-server-databases-to-azure"></a><span data-ttu-id="96002-111">Skapa en princip för säkerhetskopiering för att skydda SQL Server-databaser till Azure</span><span class="sxs-lookup"><span data-stu-id="96002-111">Create a backup policy to protect SQL Server databases to Azure</span></span>
1. <span data-ttu-id="96002-112">Klicka på Azure Backup Server UI, den **skydd** arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="96002-112">On the Azure Backup Server UI, click the **Protection** workspace.</span></span>
2. <span data-ttu-id="96002-113">Klicka på verktygsfliken **ny** att skapa en ny skyddsgrupp.</span><span class="sxs-lookup"><span data-stu-id="96002-113">On the tool ribbon, click **New** to create a new protection group.</span></span>

    ![Skapa en Skyddsgrupp](./media/backup-azure-backup-sql/protection-group.png)
3. <span data-ttu-id="96002-115">MABS visar startskärmen med vägledning om hur du skapar en **Skyddsgrupp**.</span><span class="sxs-lookup"><span data-stu-id="96002-115">MABS shows the start screen with the guidance on creating a **Protection Group**.</span></span> <span data-ttu-id="96002-116">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-116">Click **Next**.</span></span>
4. <span data-ttu-id="96002-117">Välj **servrar**.</span><span class="sxs-lookup"><span data-stu-id="96002-117">Select **Servers**.</span></span>

    ![Välj typ av Skyddsgrupp - 'Servrar'](./media/backup-azure-backup-sql/pg-servers.png)
5. <span data-ttu-id="96002-119">Expandera den SQL Server-datorn där databaserna säkerhetskopieras finns.</span><span class="sxs-lookup"><span data-stu-id="96002-119">Expand the SQL Server machine where the databases to be backed up are present.</span></span> <span data-ttu-id="96002-120">MABS visas olika datakällor som kan säkerhetskopieras från servern.</span><span class="sxs-lookup"><span data-stu-id="96002-120">MABS shows various data sources that can be backed up from that server.</span></span> <span data-ttu-id="96002-121">Expandera den **alla SQL-resurser** och väljer du databaserna (i det här fallet vi valt ReportServer$ MSDPM2012 och ReportServer$ MSDPM2012TempDB) som ska säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="96002-121">Expand the **All SQL Shares** and select the databases (in this case we selected ReportServer$MSDPM2012 and ReportServer$MSDPM2012TempDB) to be backed up.</span></span> <span data-ttu-id="96002-122">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-122">Click **Next**.</span></span>

    ![Välj SQL-databas](./media/backup-azure-backup-sql/pg-databases.png)
6. <span data-ttu-id="96002-124">Ange ett namn för skyddsgruppen och välj den **jag vill Onlineskydd** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="96002-124">Provide a name for the protection group and select the **I want online Protection** checkbox.</span></span>

    ![Dataskyddsmetod - kortvarigt disk & Online Azure](./media/backup-azure-backup-sql/pg-name.png)
7. <span data-ttu-id="96002-126">I den **ange kortvariga mål** skärmen får innehålla nödvändiga indata för att skapa säkerhetskopiering pekar på disk.</span><span class="sxs-lookup"><span data-stu-id="96002-126">In the **Specify Short-Term Goals** screen, include the necessary inputs to create backup points to disk.</span></span>

    <span data-ttu-id="96002-127">Här visas som **Kvarhållningsintervall** är inställd på *5 dagar*, **synkroniseringsfrekvensen** anges en gång till varje *15 minuter* som är frekvensen för säkerhetskopia görs.</span><span class="sxs-lookup"><span data-stu-id="96002-127">Here we see that **Retention range** is set to *5 days*, **Synchronization frequency** is set to once every *15 minutes* which is the frequency at which backup is taken.</span></span> <span data-ttu-id="96002-128">**Snabb och fullständig säkerhetskopiering** är inställd på *8:00 P.M*.</span><span class="sxs-lookup"><span data-stu-id="96002-128">**Express Full Backup** is set to *8:00 P.M*.</span></span>

    ![Kortsiktiga mål](./media/backup-azure-backup-sql/pg-shortterm.png)

   > [!NOTE]
   > <span data-ttu-id="96002-130">Vid 20:00:00 (enligt skärmen indata) skapas en säkerhetskopiering varje dag genom att överföra data som har ändrats från föregående dag 8:00 PM säkerhetskopieringspunkt.</span><span class="sxs-lookup"><span data-stu-id="96002-130">At 8:00 PM (according to the screen input) a backup point is created every day by transferring the data that has been modified from the previous day’s 8:00 PM backup point.</span></span> <span data-ttu-id="96002-131">Den här processen kallas **snabb fullständig säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="96002-131">This process is called **Express Full Backup**.</span></span> <span data-ttu-id="96002-132">Om det är nödvändigt att återställa databasen vid 21:00:00 – punkten skapas genom att spela upp loggar från senaste när transaktionen loggar synkroniseras var 15: e minut snabba och fullständiga säkerhetskopieringspunkt (20: 00 i detta fall).</span><span class="sxs-lookup"><span data-stu-id="96002-132">While the transaction logs are synchronized every 15 minutes, if there is a need to recover the database at 9:00 PM – then the point is created by replaying the logs from the last express full backup point (8pm in this case).</span></span>
   >
   >

8. <span data-ttu-id="96002-133">Klicka på **Nästa**</span><span class="sxs-lookup"><span data-stu-id="96002-133">Click **Next**</span></span>

    <span data-ttu-id="96002-134">MABS visar det övergripande lagringsutrymmet och potentiella diskarna.</span><span class="sxs-lookup"><span data-stu-id="96002-134">MABS shows the overall storage space available and the potential disk space utilization.</span></span>

    ![Diskallokering](./media/backup-azure-backup-sql/pg-storage.png)

    <span data-ttu-id="96002-136">Som standard skapar MABS en volym per datakälla (SQL Server-databas) som används för den första säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="96002-136">By default, MABS creates one volume per data source (SQL Server database) which is used for the initial backup copy.</span></span> <span data-ttu-id="96002-137">Med den här metoden begränsar LDM Logical Disk Manager () MABS skydd till 300 datakällor (SQL Server-databaser).</span><span class="sxs-lookup"><span data-stu-id="96002-137">Using this approach, the Logical Disk Manager (LDM) limits MABS protection to 300 data sources (SQL Server databases).</span></span> <span data-ttu-id="96002-138">För att undvika denna begränsning, Välj den **samplacera data i DPM-lagringspoolen**, alternativet.</span><span class="sxs-lookup"><span data-stu-id="96002-138">To work around this limitation, select the **Co-locate data in DPM Storage Pool**, option.</span></span> <span data-ttu-id="96002-139">Om du använder det här alternativet använder MABS en enda volym för flera datakällor, vilket gör att MABS att skydda upp till 2000 SQL-databaser.</span><span class="sxs-lookup"><span data-stu-id="96002-139">If you use this option, MABS uses a single volume for multiple data sources, which allows MABS to protect up to 2000 SQL databases.</span></span>

    <span data-ttu-id="96002-140">Om **Utöka automatiskt volymerna** alternativet har valts, MABS kan kontot för ökad säkerhetskopieringsvolymen när produktionsdata växer.</span><span class="sxs-lookup"><span data-stu-id="96002-140">If **Automatically grow the volumes** option is selected, MABS can account for the increased backup volume as the production data grows.</span></span> <span data-ttu-id="96002-141">Om **Utöka automatiskt volymerna** alternativet inte är markerat, MABS begränsar säkerhetskopieringslagring som används för att datakällorna i skyddsgruppen.</span><span class="sxs-lookup"><span data-stu-id="96002-141">If **Automatically grow the volumes** option is not selected, MABS limits the backup storage used to the data sources in the protection group.</span></span>
9. <span data-ttu-id="96002-142">Administratörer ges valet att överföra den här första säkerhetskopieringen manuellt (av nätverk) för att undvika överbelastning av bandbredd eller via nätverket.</span><span class="sxs-lookup"><span data-stu-id="96002-142">Administrators are given the choice of transferring this initial backup manually (off network) to avoid bandwidth congestion or over the network.</span></span> <span data-ttu-id="96002-143">De kan också konfigurera den tid då den första synkroniseringen kan inträffa.</span><span class="sxs-lookup"><span data-stu-id="96002-143">They can also configure the time at which the initial transfer can happen.</span></span> <span data-ttu-id="96002-144">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-144">Click **Next**.</span></span>

    ![Metoden för inledande replikering](./media/backup-azure-backup-sql/pg-manual.png)

    <span data-ttu-id="96002-146">Den första säkerhetskopian kräver överföring av hela datakällan (SQL Server-databas) från produktionsservern (SQL Server-dator) till MABS.</span><span class="sxs-lookup"><span data-stu-id="96002-146">The initial backup copy requires transfer of the entire data source (SQL Server database) from production server (SQL Server machine) to MABS.</span></span> <span data-ttu-id="96002-147">Dessa data kan vara stora och överföra data över nätverket kan överstiga bandbredd.</span><span class="sxs-lookup"><span data-stu-id="96002-147">This data might be large, and transferring the data over the network could exceed bandwidth.</span></span> <span data-ttu-id="96002-148">Därför kan administratörer kan välja att överföra den första säkerhetskopian: **manuellt** (med flyttbara media) att undvika överbelastning av bandbredd, eller **automatiskt via nätverket** (på en angiven tid).</span><span class="sxs-lookup"><span data-stu-id="96002-148">For this reason, administrators can choose to transfer the initial backup: **Manually** (using removable media) to avoid bandwidth congestion, or **Automatically over the network** (at a specified time).</span></span>

    <span data-ttu-id="96002-149">När den första säkerhetskopieringen är klar, är resten av säkerhetskopior säkerhetskopior på den första säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="96002-149">Once the initial backup is complete, the rest of the backups are incremental backups on the initial backup copy.</span></span> <span data-ttu-id="96002-150">Inkrementella säkerhetskopieringar brukar vara små och överföras lätt via nätverket.</span><span class="sxs-lookup"><span data-stu-id="96002-150">Incremental backups tend to be small and are easily transferred across the network.</span></span>
10. <span data-ttu-id="96002-151">Välj när du vill att konsekvenskontrollen att köra och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-151">Choose when you want the consistency check to run and click **Next**.</span></span>

    ![Konsekvenskontroll](./media/backup-azure-backup-sql/pg-consistent.png)

    <span data-ttu-id="96002-153">MABS kan utföra en konsekvenskontroll att kontrollera integriteten hos säkerhetskopieringspunkt.</span><span class="sxs-lookup"><span data-stu-id="96002-153">MABS can perform a consistency check to check the integrity of the backup point.</span></span> <span data-ttu-id="96002-154">Kontrollsumman för säkerhetskopian på produktionsservern (SQL Server-dator i det här scenariot) och de säkerhetskopierade data för filen på MABS beräknas.</span><span class="sxs-lookup"><span data-stu-id="96002-154">It calculates the checksum of the backup file on the production server (SQL Server machine in this scenario) and the backed-up data for that file at MABS.</span></span> <span data-ttu-id="96002-155">Om en konflikt uppstår förutsätts det att den säkerhetskopierade filen på MABS är skadad.</span><span class="sxs-lookup"><span data-stu-id="96002-155">In the case of a conflict, it is assumed that the backed-up file at MABS is corrupt.</span></span> <span data-ttu-id="96002-156">MABS hejdar säkerhetskopierade data genom att skicka block som motsvarar den felaktig matchning av kontrollsumma.</span><span class="sxs-lookup"><span data-stu-id="96002-156">MABS rectifies the backed-up data by sending the blocks corresponding to the checksum mismatch.</span></span> <span data-ttu-id="96002-157">Konsekvenskontroll är en igen prestandakrävande, har administratörer möjlighet att schemalägga en konsekvenskontroll eller kör automatiskt.</span><span class="sxs-lookup"><span data-stu-id="96002-157">As the consistency check is a performance-intensive operation, administrators have the option of scheduling the consistency check or running it automatically.</span></span>
11. <span data-ttu-id="96002-158">Välj databaser att skydda till Azure och klicka på för att ange online-skydd av datakällor **nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-158">To specify online protection of the datasources, select the databases to be protected to Azure and click **Next**.</span></span>

    ![Välj datakällor](./media/backup-azure-backup-sql/pg-sqldatabases.png)
12. <span data-ttu-id="96002-160">Administratörer kan välja scheman för säkerhetskopiering och bevarandeprinciper som passar organisationens principer.</span><span class="sxs-lookup"><span data-stu-id="96002-160">Administrators can choose backup schedules and retention policies that suit their organization policies.</span></span>

    ![Schemat och lagring](./media/backup-azure-backup-sql/pg-schedule.png)

    <span data-ttu-id="96002-162">I det här exemplet tas säkerhetskopieringar en gång om dagen med 12:00 och 20: 00 (längst ned på skärmen)</span><span class="sxs-lookup"><span data-stu-id="96002-162">In this example, backups are taken once a day at 12:00 PM and 8 PM (bottom part of the screen)</span></span>

    > [!NOTE]
    > <span data-ttu-id="96002-163">Det är en bra idé att ha några återställningspunkter för kortvarigt skydd på disk för snabb återställning.</span><span class="sxs-lookup"><span data-stu-id="96002-163">It’s a good practice to have a few short-term recovery points on disk, for quick recovery.</span></span> <span data-ttu-id="96002-164">Dessa återställningspunkter används för ”operativa återställning”.</span><span class="sxs-lookup"><span data-stu-id="96002-164">These recovery points are used for “operational recovery".</span></span> <span data-ttu-id="96002-165">Azure fungerar som en bra extern plats med högre SLA och garanteras tillgänglighet.</span><span class="sxs-lookup"><span data-stu-id="96002-165">Azure serves as a good offsite location with higher SLAs and guaranteed availability.</span></span>
    >
    >

    <span data-ttu-id="96002-166">**Bästa praxis**: Kontrollera att Azure-säkerhetskopieringar är schemalagda efter slutförandet av lokal disk-säkerhetskopiering med DPM.</span><span class="sxs-lookup"><span data-stu-id="96002-166">**Best Practice**: Make sure that Azure Backups are scheduled after the completion of local disk backups using DPM.</span></span> <span data-ttu-id="96002-167">Detta gör att den senaste disksäkerhetskopian av som ska kopieras till Azure.</span><span class="sxs-lookup"><span data-stu-id="96002-167">This enables the latest disk backup to be copied to Azure.</span></span>

13. <span data-ttu-id="96002-168">Välj principen bevarandeschemat.</span><span class="sxs-lookup"><span data-stu-id="96002-168">Choose the retention policy schedule.</span></span> <span data-ttu-id="96002-169">Information om hur bevarandeprincipen fungerar tillhandahålls på [Använd Azure Backup för att ersätta band infrastruktur artikeln](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="96002-169">The details on how the retention policy works are provided at [Use Azure Backup to replace your tape infrastructure article](backup-azure-backup-cloud-as-tape.md).</span></span>

    ![Bevarandeprincip](./media/backup-azure-backup-sql/pg-retentionschedule.png)

    <span data-ttu-id="96002-171">I det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="96002-171">In this example:</span></span>

    * <span data-ttu-id="96002-172">Säkerhetskopieringar tas en gång om dagen med 12:00 och 20: 00 (längst ned på skärmen) och bevaras i 180 dagar.</span><span class="sxs-lookup"><span data-stu-id="96002-172">Backups are taken once a day at 12:00 PM and 8 PM (bottom part of the screen) and are retained for 180 days.</span></span>
    * <span data-ttu-id="96002-173">Säkerhetskopian på lördag kl. 12:00</span><span class="sxs-lookup"><span data-stu-id="96002-173">The backup on Saturday at 12:00 P.M.</span></span> <span data-ttu-id="96002-174">sparas i 104 veckor</span><span class="sxs-lookup"><span data-stu-id="96002-174">is retained for 104 weeks</span></span>
    * <span data-ttu-id="96002-175">Säkerhetskopian på sista lördag kl. 12:00</span><span class="sxs-lookup"><span data-stu-id="96002-175">The backup on Last Saturday at 12:00 P.M.</span></span> <span data-ttu-id="96002-176">sparas i 60 månader</span><span class="sxs-lookup"><span data-stu-id="96002-176">is retained for 60 months</span></span>
    * <span data-ttu-id="96002-177">Säkerhetskopian på sista lördag mars klockan 12:00.</span><span class="sxs-lookup"><span data-stu-id="96002-177">The backup on Last Saturday of March at 12:00 P.M.</span></span> <span data-ttu-id="96002-178">sparas för 10 år</span><span class="sxs-lookup"><span data-stu-id="96002-178">is retained for 10 years</span></span>
14. <span data-ttu-id="96002-179">Klicka på **nästa** och väljer lämpligt alternativ för att överföra den första säkerhetskopian till Azure.</span><span class="sxs-lookup"><span data-stu-id="96002-179">Click **Next** and select the appropriate option for transferring the initial backup copy to Azure.</span></span> <span data-ttu-id="96002-180">Du kan välja **automatiskt via nätverket** eller **Offline säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="96002-180">You can choose **Automatically over the network** or **Offline Backup**.</span></span>

    * <span data-ttu-id="96002-181">**Automatiskt via nätverket** överförs säkerhetskopierade data till Azure enligt det schema som valts för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="96002-181">**Automatically over the network** transfers the backup data to Azure as per the schedule chosen for backup.</span></span>
    * <span data-ttu-id="96002-182">Hur **Offline säkerhetskopiering** fungerar förklaras på [Offline säkerhetskopiering arbetsflöde i Azure Backup](backup-azure-backup-import-export.md).</span><span class="sxs-lookup"><span data-stu-id="96002-182">How **Offline Backup** works is explained at [Offline Backup workflow in Azure Backup](backup-azure-backup-import-export.md).</span></span>

    <span data-ttu-id="96002-183">Välja relevant överföring mekanism för att skicka den första säkerhetskopian till Azure och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-183">Choose the relevant transfer mechanism to send the initial backup copy to Azure and click **Next**.</span></span>
15. <span data-ttu-id="96002-184">När du granska information om principen i den **sammanfattning** skärmen, klicka på den **Skapa grupp** för att slutföra arbetsflödet.</span><span class="sxs-lookup"><span data-stu-id="96002-184">Once you review the policy details in the **Summary** screen, click on the **Create group** button to complete the workflow.</span></span> <span data-ttu-id="96002-185">Du kan klicka på den **Stäng** knappen och övervaka jobbförloppet på arbetsytan övervakning.</span><span class="sxs-lookup"><span data-stu-id="96002-185">You can click the **Close** button and monitor the job progress in Monitoring workspace.</span></span>

    ![Skapandet av Skyddsgruppen pågår](./media/backup-azure-backup-sql/pg-summary.png)

## <a name="on-demand-backup-of-a-sql-server-database"></a><span data-ttu-id="96002-187">Säkerhetskopiering på begäran av en SQL Server-databas</span><span class="sxs-lookup"><span data-stu-id="96002-187">On-demand backup of a SQL Server database</span></span>
<span data-ttu-id="96002-188">När de föregående stegen skapade en princip för säkerhetskopiering, skapas en ”återställningspunkt” endast när den första säkerhetskopieringen inträffar.</span><span class="sxs-lookup"><span data-stu-id="96002-188">While the previous steps created a backup policy, a “recovery point” is created only when the first backup occurs.</span></span> <span data-ttu-id="96002-189">I stället för väntar Schemaläggaren kan förbättra, peka stegen nedan utlösaren genereringen av en återställningspunkt manuellt.</span><span class="sxs-lookup"><span data-stu-id="96002-189">Rather than waiting for the scheduler to kick in, the steps below trigger the creation of a recovery point manually.</span></span>

1. <span data-ttu-id="96002-190">Vänta tills skyddsgruppens status visas **OK** för databasen innan du skapar återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="96002-190">Wait until the protection group status shows **OK** for the database before creating the recovery point.</span></span>

    ![Skyddsgruppmedlemmar](./media/backup-azure-backup-sql/sqlbackup-recoverypoint.png)
2. <span data-ttu-id="96002-192">Högerklicka på databasen och välj **Skapa återställningspunkt**.</span><span class="sxs-lookup"><span data-stu-id="96002-192">Right-click on the database and select **Create Recovery Point**.</span></span>

    ![Skapa Onlineåterställningspunkt](./media/backup-azure-backup-sql/sqlbackup-createrp.png)
3. <span data-ttu-id="96002-194">Välj **Onlineskydd** i den nedrullningsbara menyn och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="96002-194">Choose **Online Protection** in the drop-down menu and click **OK**.</span></span> <span data-ttu-id="96002-195">Detta startar skapandet av en återställningspunkt i Azure.</span><span class="sxs-lookup"><span data-stu-id="96002-195">This starts the creation of a recovery point in Azure.</span></span>

    ![Skapa återställningspunkt](./media/backup-azure-backup-sql/sqlbackup-azure.png)
4. <span data-ttu-id="96002-197">Du kan visa jobbförloppet på den **övervakning** arbetsyta där du hittar en pågående jobb som det beskrivs i nästa figur.</span><span class="sxs-lookup"><span data-stu-id="96002-197">You can view the job progress in the **Monitoring** workspace where you'll find an in progress job like the one depicted in the next figure.</span></span>

    ![Övervakningskonsolen](./media/backup-azure-backup-sql/sqlbackup-monitoring.png)

## <a name="recover-a-sql-server-database-from-azure"></a><span data-ttu-id="96002-199">Återställa en SQL Server-databas från Azure</span><span class="sxs-lookup"><span data-stu-id="96002-199">Recover a SQL Server database from Azure</span></span>
<span data-ttu-id="96002-200">Följande steg krävs för att återställa en skyddad entitet (SQL Server-databas) från Azure.</span><span class="sxs-lookup"><span data-stu-id="96002-200">The following steps are required to recover a protected entity (SQL Server database) from Azure.</span></span>

1. <span data-ttu-id="96002-201">Öppna hanteringskonsolen för DPM-servern.</span><span class="sxs-lookup"><span data-stu-id="96002-201">Open the DPM server Management Console.</span></span> <span data-ttu-id="96002-202">Gå till **Recovery** arbetsyta där du kan se servrarna som säkerhetskopieras av DPM.</span><span class="sxs-lookup"><span data-stu-id="96002-202">Navigate to **Recovery** workspace where you can see the servers backed up by DPM.</span></span> <span data-ttu-id="96002-203">Bläddra obligatorisk databas (i det här fallet ReportServer$ MSDPM2012).</span><span class="sxs-lookup"><span data-stu-id="96002-203">Browse the required database (in this case ReportServer$MSDPM2012).</span></span> <span data-ttu-id="96002-204">Välj en **återställning från** som slutar med **Online**.</span><span class="sxs-lookup"><span data-stu-id="96002-204">Select a **Recovery from** time which ends with **Online**.</span></span>

    ![Välj återställningspunkt](./media/backup-azure-backup-sql/sqlbackup-restorepoint.png)
2. <span data-ttu-id="96002-206">Högerklicka på namnet på databasen och på **återställa**.</span><span class="sxs-lookup"><span data-stu-id="96002-206">Right-click the database name and click **Recover**.</span></span>

    ![Återställa från Azure](./media/backup-azure-backup-sql/sqlbackup-recover.png)
3. <span data-ttu-id="96002-208">DPM visar information om återställningspunkten.</span><span class="sxs-lookup"><span data-stu-id="96002-208">DPM shows the details of the recovery point.</span></span> <span data-ttu-id="96002-209">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-209">Click **Next**.</span></span> <span data-ttu-id="96002-210">Om du vill skriva över databasen väljer du typ av återställning **Återställ till ursprunglig instans av SQL Server**.</span><span class="sxs-lookup"><span data-stu-id="96002-210">To overwrite the database, select the recovery type **Recover to original instance of SQL Server**.</span></span> <span data-ttu-id="96002-211">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-211">Click **Next**.</span></span>

    ![Återställa till ursprunglig plats](./media/backup-azure-backup-sql/sqlbackup-recoveroriginal.png)

    <span data-ttu-id="96002-213">I det här exemplet kan DPM återställning av databasen till en annan SQL Server-instans eller till en nätverksmapp för fristående.</span><span class="sxs-lookup"><span data-stu-id="96002-213">In this example, DPM allows recovery of the database to another SQL Server instance or to a standalone network folder.</span></span>
4. <span data-ttu-id="96002-214">I den **Ange återställningsalternativ** skärmen, som du kan välja återställningsalternativ som begränsning av nätverksbandbredd för att begränsa bandbredden som används av återställningen.</span><span class="sxs-lookup"><span data-stu-id="96002-214">In the **Specify Recovery options** screen, you can select the recovery options like Network bandwidth usage throttling to throttle the bandwidth used by recovery.</span></span> <span data-ttu-id="96002-215">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="96002-215">Click **Next**.</span></span>
5. <span data-ttu-id="96002-216">I den **sammanfattning** ser du alla recovery konfigurationer som hittills.</span><span class="sxs-lookup"><span data-stu-id="96002-216">In the **Summary** screen, you see all the recovery configurations provided so far.</span></span> <span data-ttu-id="96002-217">Klicka på **återställa**.</span><span class="sxs-lookup"><span data-stu-id="96002-217">Click **Recover**.</span></span>

    <span data-ttu-id="96002-218">Status på återställningen visar databas som återställs.</span><span class="sxs-lookup"><span data-stu-id="96002-218">The Recovery status shows the database being recovered.</span></span> <span data-ttu-id="96002-219">Du kan klicka på **Stäng** att stänga guiden och visa förloppet på den **övervakning** arbetsytan.</span><span class="sxs-lookup"><span data-stu-id="96002-219">You can click **Close** to close the wizard and view the progress in the **Monitoring** workspace.</span></span>

    ![Starta återställningsprocessen](./media/backup-azure-backup-sql/sqlbackup-recoverying.png)

    <span data-ttu-id="96002-221">När återställningen har slutförts är den återställda databasen program som är konsekventa.</span><span class="sxs-lookup"><span data-stu-id="96002-221">Once the recovery is completed, the restored database is application consistent.</span></span>

### <a name="next-steps"></a><span data-ttu-id="96002-222">Nästa steg:</span><span class="sxs-lookup"><span data-stu-id="96002-222">Next Steps:</span></span>
<span data-ttu-id="96002-223">• [Azure Backup vanliga frågor och svar](backup-azure-backup-faq.md)</span><span class="sxs-lookup"><span data-stu-id="96002-223">•    [Azure Backup FAQ](backup-azure-backup-faq.md)</span></span>
