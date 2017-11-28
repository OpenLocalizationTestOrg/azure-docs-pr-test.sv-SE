---
title: "aaaRestore data tooa Windows Server eller Windows-klienten från att använda Azure hello klassiska distributionsmodellen | Microsoft Docs"
description: "Lär dig hur toorestore från en Windows Server eller Windows-klient."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 85585dfc-c764-4e8c-8f0e-40b969640ac2
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/11/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 4d1458d5233c4f55004ecfa95cbf7b3b18a03dde
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-hello-classic-deployment-model"></a><span data-ttu-id="3e07f-103">Återställ filer tooa Windows server eller Windows-klientdatorn med hjälp av hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="3e07f-103">Restore files tooa Windows server or Windows client machine using hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3e07f-104">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="3e07f-104">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
> * [<span data-ttu-id="3e07f-105">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="3e07f-105">Azure portal</span></span>](backup-azure-restore-windows-server.md)
>
>

<span data-ttu-id="3e07f-106">Den här artikeln förklarar hur toorecover data från en säkerhetskopia valvet och återställer den tooa server eller dator.</span><span class="sxs-lookup"><span data-stu-id="3e07f-106">This article explains how toorecover data from a backup vault and restore it tooa server or computer.</span></span> <span data-ttu-id="3e07f-107">Med början i mars 2017 kan skapa du inte längre säkerhetskopieringsvalv i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-107">Starting in March 2017, you can no longer create backup vaults in hello classic portal.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="3e07f-108">Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="3e07f-108">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="3e07f-109">Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="3e07f-109">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="3e07f-110">Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="3e07f-110">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="3e07f-111">**15 oktober 2017**, kommer du inte längre att kunna toouse PowerShell toocreate säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="3e07f-111">**October 15, 2017**, you will no longer be able toouse PowerShell toocreate Backup vaults.</span></span> <br/> <span data-ttu-id="3e07f-112">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="3e07f-112">**Starting November 1, 2017**:</span></span>
>- <span data-ttu-id="3e07f-113">Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="3e07f-113">Any remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="3e07f-114">Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-114">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="3e07f-115">Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="3e07f-115">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

<span data-ttu-id="3e07f-116">toorestore data använder du guiden för hello återställa Data i hello Microsoft Azure Recovery Services MARS-agenten.</span><span class="sxs-lookup"><span data-stu-id="3e07f-116">toorestore data, you use hello Recover Data wizard in hello Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="3e07f-117">När du återställer data är det möjligt att:</span><span class="sxs-lookup"><span data-stu-id="3e07f-117">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="3e07f-118">Återställ data toohello samma datorn från vilken hello säkerhetskopieringar som utförts.</span><span class="sxs-lookup"><span data-stu-id="3e07f-118">Restore data toohello same machine from which hello backups were taken.</span></span>
* <span data-ttu-id="3e07f-119">Återställa data tooan alternativa dator.</span><span class="sxs-lookup"><span data-stu-id="3e07f-119">Restore data tooan alternate machine.</span></span>

<span data-ttu-id="3e07f-120">Microsoft släppt en Preview update toohello MARS-agenten i januari 2017.</span><span class="sxs-lookup"><span data-stu-id="3e07f-120">In January 2017, Microsoft released a Preview update toohello MARS agent.</span></span> <span data-ttu-id="3e07f-121">Tillsammans med felkorrigeringar, kan du använda snabbmeddelanden återställa, vilket gör att du toomount en skrivbar återställning punkt ögonblicksbild som en återställningspunkt-volym.</span><span class="sxs-lookup"><span data-stu-id="3e07f-121">Along with bug fixes, this update enables Instant Restore, which allows you toomount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="3e07f-122">Du kan sedan utforska hello recovery volym och kopiera filer tooa lokal dator vilket selektivt återställningen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-122">You can then explore hello recovery volume and copy files tooa local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="3e07f-123">Hej [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) krävs om du vill toouse omedelbar återställning toorestore av data.</span><span class="sxs-lookup"><span data-stu-id="3e07f-123">hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want toouse Instant Restore toorestore data.</span></span> <span data-ttu-id="3e07f-124">Hello säkerhetskopierade data måste också skyddas i valv i språk som anges i hello support-artikeln.</span><span class="sxs-lookup"><span data-stu-id="3e07f-124">Also hello backup data must be protected in vaults in locales listed in hello support article.</span></span> <span data-ttu-id="3e07f-125">Kontakta hello [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) hello senaste lista över språk som har stöd för omedelbar återställning.</span><span class="sxs-lookup"><span data-stu-id="3e07f-125">Consult hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for hello latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="3e07f-126">Omedelbar återställning **inte** är tillgänglig i alla språk.</span><span class="sxs-lookup"><span data-stu-id="3e07f-126">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="3e07f-127">Omedelbar återställning är tillgänglig för användning i Recovery Services-valv i hello Azure-portalen och säkerhetskopieringsvalv i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-127">Instant Restore is available for use in Recovery Services vaults in hello Azure portal and Backup vaults in hello classic portal.</span></span> <span data-ttu-id="3e07f-128">Om du vill toouse Instant återställa hämta hello MARS uppdateringen och följ hello procedurer som nämner omedelbar återställning.</span><span class="sxs-lookup"><span data-stu-id="3e07f-128">If you want toouse Instant Restore, download hello MARS update, and follow hello procedures that mention Instant Restore.</span></span>


## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a><span data-ttu-id="3e07f-129">Använda snabbmeddelanden återställa toorecover data toohello samma dator</span><span class="sxs-lookup"><span data-stu-id="3e07f-129">Use Instant Restore toorecover data toohello same machine</span></span>

<span data-ttu-id="3e07f-130">Om du av misstag tar bort en fil- och vill toorestore den toohello samma dator (från vilka hello säkerhetskopiering tas), hello följande steg hjälper dig att återställa hello data.</span><span class="sxs-lookup"><span data-stu-id="3e07f-130">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="3e07f-131">Öppna hello **Microsoft Azure Backup** fästa i.</span><span class="sxs-lookup"><span data-stu-id="3e07f-131">Open hello **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="3e07f-132">Om du inte vet där hello snapin-modulen installerades, söka hello dator eller server för **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-132">If you don't know where hello snap in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="3e07f-133">Hej skrivbordsapp ska visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="3e07f-133">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="3e07f-134">Klicka på **återställa Data** toostart hello guiden.</span><span class="sxs-lookup"><span data-stu-id="3e07f-134">Click **Recover Data** toostart hello wizard.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="3e07f-136">På hello **komma igång** rutan, toorestore hello data toohello samma server eller dator, Välj **servern (`<server name>`)** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-136">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Välj den här servern alternativet toorestore hello data toohello samma dator](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="3e07f-138">På hello **Välj återställningsläge** fönstret Välj **enskilda filer och mappar** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-138">On hello **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Bläddra bland filer](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="3e07f-140">På hello **Välj volym och datum** rutan, Välj hello volymen som innehåller hello filer och/eller mappar som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="3e07f-140">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="3e07f-141">Välj en återställningspunkt hello kalendern.</span><span class="sxs-lookup"><span data-stu-id="3e07f-141">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="3e07f-142">Du kan återställa från valfri återställningspunkt i tid.</span><span class="sxs-lookup"><span data-stu-id="3e07f-142">You can restore from any recovery point in time.</span></span> <span data-ttu-id="3e07f-143">Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="3e07f-143">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="3e07f-144">När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="3e07f-144">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Volym och datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="3e07f-146">När du har valt hello recovery punkt toorestore, klickar du på **montera**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-146">Once you have chosen hello recovery point toorestore, click **Mount**.</span></span>

    <span data-ttu-id="3e07f-147">Azure-säkerhetskopiering monterar hello lokal återställningspunkt och använder den som en återställningspunkt-volym.</span><span class="sxs-lookup"><span data-stu-id="3e07f-147">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="3e07f-148">På hello **Bläddra och återställa filer** rutan klickar du på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill.</span><span class="sxs-lookup"><span data-stu-id="3e07f-148">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Återställningsalternativ](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="3e07f-150">I Utforskaren, kopiera hello filer och mappar du vill toorestore och klistra in dem tooany lokala toohello servern eller datorn.</span><span class="sxs-lookup"><span data-stu-id="3e07f-150">In Windows Explorer, copy hello files and/or folders you want toorestore and paste them tooany location local toohello server or computer.</span></span> <span data-ttu-id="3e07f-151">Du kan öppna eller strömma hello filer direkt från hello recovery volym och verifiera hello rätt versioner återställs.</span><span class="sxs-lookup"><span data-stu-id="3e07f-151">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Kopiera och klistra in filer och mappar från monterad volym toolocal plats](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="3e07f-153">När du är klar återställer hello filer och/eller mappar på hello **Bläddra och återställningsfiler** rutan klickar du på **koppla från**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-153">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="3e07f-154">Klicka på **Ja** tooconfirm som du vill toounmount hello volym.</span><span class="sxs-lookup"><span data-stu-id="3e07f-154">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Demontera hello volym och bekräfta](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="3e07f-156">Om du inte klicka på Koppla från, förblir hello Recovery volym monterade under sex timmar från hello tid när den är monterad.</span><span class="sxs-lookup"><span data-stu-id="3e07f-156">If you do not click Unmount, hello Recovery Volume will remain mounted for six hours from hello time when it was mounted.</span></span> <span data-ttu-id="3e07f-157">Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad.</span><span class="sxs-lookup"><span data-stu-id="3e07f-157">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="3e07f-158">Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.</span><span class="sxs-lookup"><span data-stu-id="3e07f-158">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="recover-data-toohello-same-machine"></a><span data-ttu-id="3e07f-159">Återställa data toohello samma dator</span><span class="sxs-lookup"><span data-stu-id="3e07f-159">Recover data toohello same machine</span></span>
<span data-ttu-id="3e07f-160">Om du av misstag tar bort en fil- och vill toorestore den toohello samma dator (från vilka hello säkerhetskopiering tas), hello följande steg hjälper dig att återställa hello data.</span><span class="sxs-lookup"><span data-stu-id="3e07f-160">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="3e07f-161">Öppna hello **Microsoft Azure Backup** fästa i.</span><span class="sxs-lookup"><span data-stu-id="3e07f-161">Open hello **Microsoft Azure Backup** snap in.</span></span>
2. <span data-ttu-id="3e07f-162">Klicka på **återställa Data** tooinitiate hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="3e07f-162">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server-classic/recover.png)
3. <span data-ttu-id="3e07f-164">Välj hello  **servern (*yourmachinename*) ** alternativet toorestore hello säkerhetskopierade filen på hello samma dator.</span><span class="sxs-lookup"><span data-stu-id="3e07f-164">Select hello **This server (*yourmachinename*)** option toorestore hello backed up file on hello same machine.</span></span>

    ![Samma dator](./media/backup-azure-restore-windows-server-classic/samemachine.png)
4. <span data-ttu-id="3e07f-166">Välj för**Bläddra efter filer** eller **söka efter filer**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-166">Choose too**Browse for files** or **Search for files**.</span></span>

    <span data-ttu-id="3e07f-167">Lämna hello standardalternativet om du planerar toorestore en eller flera filer vars sökväg är känd.</span><span class="sxs-lookup"><span data-stu-id="3e07f-167">Leave hello default option if you plan toorestore one or more files whose path is known.</span></span> <span data-ttu-id="3e07f-168">Om du inte är säker hello mappstrukturen men vill toosearch för en fil, Välj hello **söka efter filer** alternativet.</span><span class="sxs-lookup"><span data-stu-id="3e07f-168">If you are not sure about hello folder structure but would like toosearch for a file, pick hello **Search for files** option.</span></span> <span data-ttu-id="3e07f-169">Vi kommer att fortsätta med hello standardalternativet för hello syftet med det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="3e07f-169">For hello purpose of this section, we will proceed with hello default option.</span></span>

    ![Bläddra bland filer](./media/backup-azure-restore-windows-server-classic/browseandsearch.png)
5. <span data-ttu-id="3e07f-171">Välj hello volym som du vill toorestore hello-filen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-171">Select hello volume from which you wish toorestore hello file.</span></span>

    <span data-ttu-id="3e07f-172">Du kan återställa från valfri punkt i tiden.</span><span class="sxs-lookup"><span data-stu-id="3e07f-172">You can restore from any point in time.</span></span> <span data-ttu-id="3e07f-173">Datum som visas i **fetstil** i hello kalenderkontroll indikera hello tillgängligheten för en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="3e07f-173">Dates which appear in **bold** in hello calendar control indicate hello availability of a restore point.</span></span> <span data-ttu-id="3e07f-174">När ett datum har valts baserat på schemat för säkerhetskopiering (och hello genomförandet av en säkerhetskopiering), kan du välja en punkt i tiden från hello **tid** listrutan.</span><span class="sxs-lookup"><span data-stu-id="3e07f-174">Once a date is selected, based on your backup schedule (and hello success of a backup operation), you can select a point in time from hello **Time** drop down.</span></span>

    ![Volym och datum](./media/backup-azure-restore-windows-server-classic/volanddate.png)
6. <span data-ttu-id="3e07f-176">Välj hello objekt toorecover.</span><span class="sxs-lookup"><span data-stu-id="3e07f-176">Select hello items toorecover.</span></span> <span data-ttu-id="3e07f-177">Du kan välja flera mappar/filer som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="3e07f-177">You can multi-select folders/files you wish toorestore.</span></span>

    ![Välj filer](./media/backup-azure-restore-windows-server-classic/selectfiles.png)
7. <span data-ttu-id="3e07f-179">Ange hello recovery parametrar.</span><span class="sxs-lookup"><span data-stu-id="3e07f-179">Specify hello recovery parameters.</span></span>

    ![Återställningsalternativ](./media/backup-azure-restore-windows-server-classic/recoveroptions.png)

   * <span data-ttu-id="3e07f-181">Har du möjlighet att återställa toohello ursprungliga plats (filen/mappen skulle skrivas i vilka hello) eller tooanother plats i hello samma dator.</span><span class="sxs-lookup"><span data-stu-id="3e07f-181">You have an option of restoring toohello original location (in which hello file/folder would be overwritten) or tooanother location in hello same machine.</span></span>
   * <span data-ttu-id="3e07f-182">Om hello filen/mappen du vill toorestore finns i hello målplats, kan du skapa kopior (två versioner av hello samma fil), skriva över hello filer i hello målplatsen eller hoppa över hello återställning av hello-filer som finns i hello mål.</span><span class="sxs-lookup"><span data-stu-id="3e07f-182">If hello file/folder you wish toorestore exists in hello target location, you can create copies (two versions of hello same file), overwrite hello files in hello target location, or skip hello recovery of hello files which exist in hello target.</span></span>
   * <span data-ttu-id="3e07f-183">Vi rekommenderar starkt att du lämnar hello standardalternativet återställa hello ACL: er på hello-filer som är som återställs.</span><span class="sxs-lookup"><span data-stu-id="3e07f-183">It is highly recommended that you leave hello default option of restoring hello ACLs on hello files which are being recovered.</span></span>
8. <span data-ttu-id="3e07f-184">När dessa indata är tillgängliga, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-184">Once these inputs are provided, click **Next**.</span></span> <span data-ttu-id="3e07f-185">hello recovery arbetsflöde, vilket återställer hello filer toothis datorn startas.</span><span class="sxs-lookup"><span data-stu-id="3e07f-185">hello recovery workflow, which restores hello files toothis machine, will begin.</span></span>

## <a name="recover-tooan-alternate-machine"></a><span data-ttu-id="3e07f-186">Återställa tooan alternativa dator</span><span class="sxs-lookup"><span data-stu-id="3e07f-186">Recover tooan alternate machine</span></span>
<span data-ttu-id="3e07f-187">Du kan fortfarande återställa data från Azure Backup tooa annan dator om hela servern går förlorad.</span><span class="sxs-lookup"><span data-stu-id="3e07f-187">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="3e07f-188">hello följande steg visar hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="3e07f-188">hello following steps illustrate hello workflow.</span></span>  

<span data-ttu-id="3e07f-189">hello-terminologi som används i de här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="3e07f-189">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="3e07f-190">*Källdatorn* – hello ursprungliga datorn från vilken hello säkerhetskopian skapades och som inte är tillgänglig för tillfället.</span><span class="sxs-lookup"><span data-stu-id="3e07f-190">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="3e07f-191">*Måldatorn* – hello datorn toowhich hello data återställs.</span><span class="sxs-lookup"><span data-stu-id="3e07f-191">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="3e07f-192">*Exempel valvet* – hello Backup-valvet toowhich hello *källdatorn* och *måldatorn* registreras.</span><span class="sxs-lookup"><span data-stu-id="3e07f-192">*Sample vault* – hello Backup vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="3e07f-193">Säkerhetskopieringar som utförts från en dator kan inte återställas på en dator som kör en tidigare version av operativsystemet för hello.</span><span class="sxs-lookup"><span data-stu-id="3e07f-193">Backups taken from a machine cannot be restored on a machine which is running an earlier version of hello operating system.</span></span> <span data-ttu-id="3e07f-194">Till exempel om säkerhetskopiering utförs från en Windows 7-dator, kan den återställas på en Windows 8 eller senare datorn.</span><span class="sxs-lookup"><span data-stu-id="3e07f-194">For example, if backups are taken from a Windows 7 machine, it can be restored on a Windows 8 or above machine.</span></span> <span data-ttu-id="3e07f-195">Dock har hello vice versa inte värdet true.</span><span class="sxs-lookup"><span data-stu-id="3e07f-195">However, hello vice-versa does not hold true.</span></span>
>
>

1. <span data-ttu-id="3e07f-196">Öppna hello **Microsoft Azure Backup** fästa i på hello *måldatorn*.</span><span class="sxs-lookup"><span data-stu-id="3e07f-196">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>
2. <span data-ttu-id="3e07f-197">Se till att hello *måldatorn* och hello *källdatorn* är registrerade toohello samma säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="3e07f-197">Ensure that hello *Target machine* and hello *Source machine* are registered toohello same backup vault.</span></span>
3. <span data-ttu-id="3e07f-198">Klicka på **återställa Data** tooinitiate hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="3e07f-198">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server-classic/recover.png)
4. <span data-ttu-id="3e07f-200">Välj **en annan server**</span><span class="sxs-lookup"><span data-stu-id="3e07f-200">Select **Another server**</span></span>

    ![En annan Server](./media/backup-azure-restore-windows-server-classic/anotherserver.png)
5. <span data-ttu-id="3e07f-202">Ange hello valvautentiseringsfilen som motsvarar toohello *exempel valvet*.</span><span class="sxs-lookup"><span data-stu-id="3e07f-202">Provide hello vault credential file that corresponds toohello *Sample vault*.</span></span> <span data-ttu-id="3e07f-203">Ladda ned en ny valvautentiseringsfil från hello om hello valvautentiseringsfilen är ogiltig (eller har upphört att gälla) *exempel valvet* i hello klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-203">If hello vault credential file is invalid (or expired) download a new vault credential file from hello *Sample vault* in hello Azure classic portal.</span></span> <span data-ttu-id="3e07f-204">När hello valvautentiseringsfilen tillhandahålls visas hello säkerhetskopieringsvalvet mot hello valvautentiseringsfilen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-204">Once hello vault credential file is provided, hello backup vault against hello vault credential file is displayed.</span></span>
6. <span data-ttu-id="3e07f-205">Välj hello *källdatorn* hello listan med visade datorer.</span><span class="sxs-lookup"><span data-stu-id="3e07f-205">Select hello *Source machine* from hello list of displayed machines.</span></span>

    ![Lista över datorer](./media/backup-azure-restore-windows-server-classic/machinelist.png)
7. <span data-ttu-id="3e07f-207">Välj antingen hello **söka efter filer** eller **Bläddra efter filer** alternativet.</span><span class="sxs-lookup"><span data-stu-id="3e07f-207">Select either hello **Search for files** or **Browse for files** option.</span></span> <span data-ttu-id="3e07f-208">För hello syftet med det här avsnittet, kommer vi att använda hello **söka efter filer** alternativet.</span><span class="sxs-lookup"><span data-stu-id="3e07f-208">For hello purpose of this section, we will use hello **Search for files** option.</span></span>

    ![Söka](./media/backup-azure-restore-windows-server-classic/search.png)
8. <span data-ttu-id="3e07f-210">Välj hello volym och datum hello nästa skärm.</span><span class="sxs-lookup"><span data-stu-id="3e07f-210">Select hello volume and date in hello next screen.</span></span> <span data-ttu-id="3e07f-211">Sök hello mappen/filen namn du vill ha toorestore.</span><span class="sxs-lookup"><span data-stu-id="3e07f-211">Search for hello folder/file name you want toorestore.</span></span>

    ![Sökobjekt](./media/backup-azure-restore-windows-server-classic/searchitems.png)
9. <span data-ttu-id="3e07f-213">Välj hello plats där hello filer måste toobe återställs.</span><span class="sxs-lookup"><span data-stu-id="3e07f-213">Select hello location where hello files need toobe restored.</span></span>

    ![Återställa en plats](./media/backup-azure-restore-windows-server-classic/restorelocation.png)
10. <span data-ttu-id="3e07f-215">Ange hello krypteringslösenfrasen som du angav under *källdatorns Målegenskaper* registrering för*exempel valvet*.</span><span class="sxs-lookup"><span data-stu-id="3e07f-215">Provide hello encryption passphrase that was provided during *Source machine’s* registration too*Sample vault*.</span></span>

    ![Kryptering](./media/backup-azure-restore-windows-server-classic/encryption.png)
11. <span data-ttu-id="3e07f-217">När hello indata anges, klickar du på **återställa**som utlösare hello återställning av hello säkerhetskopierade filer toohello mål som angavs.</span><span class="sxs-lookup"><span data-stu-id="3e07f-217">Once hello input is provided, click **Recover**, which triggers hello restore of hello backed up files toohello destination provided.</span></span>

## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a><span data-ttu-id="3e07f-218">Använda snabbmeddelanden återställa toorestore data tooan alternativa dator</span><span class="sxs-lookup"><span data-stu-id="3e07f-218">Use Instant Restore toorestore data tooan alternate machine</span></span>
<span data-ttu-id="3e07f-219">Du kan fortfarande återställa data från Azure Backup tooa annan dator om hela servern går förlorad.</span><span class="sxs-lookup"><span data-stu-id="3e07f-219">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="3e07f-220">hello följande steg visar hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="3e07f-220">hello following steps illustrate hello workflow.</span></span>

<span data-ttu-id="3e07f-221">hello-terminologi som används i de här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="3e07f-221">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="3e07f-222">*Källdatorn* – hello ursprungliga datorn från vilken hello säkerhetskopian skapades och som inte är tillgänglig för tillfället.</span><span class="sxs-lookup"><span data-stu-id="3e07f-222">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="3e07f-223">*Måldatorn* – hello datorn toowhich hello data återställs.</span><span class="sxs-lookup"><span data-stu-id="3e07f-223">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="3e07f-224">*Exempel valvet* – hello Recovery Services-valvet toowhich hello *källdatorn* och *måldatorn* registreras.</span><span class="sxs-lookup"><span data-stu-id="3e07f-224">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="3e07f-225">Säkerhetskopieringar kan inte vara återställda tooa måldatorn som kör en tidigare version av operativsystemet för hello.</span><span class="sxs-lookup"><span data-stu-id="3e07f-225">Backups can't be restored tooa target machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="3e07f-226">Till exempel återställts en säkerhetskopia som gjorts från en Windows 7-dator kan vara på en Windows 8 eller senare, dator.</span><span class="sxs-lookup"><span data-stu-id="3e07f-226">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="3e07f-227">En säkerhetskopia som gjorts från en Windows 8-dator kan inte vara återställda tooa Windows 7-dator.</span><span class="sxs-lookup"><span data-stu-id="3e07f-227">A backup taken from a Windows 8 computer cannot be restored tooa Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="3e07f-228">Öppna hello **Microsoft Azure Backup** fästa i på hello *måldatorn*.</span><span class="sxs-lookup"><span data-stu-id="3e07f-228">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>

2. <span data-ttu-id="3e07f-229">Se till att hello *måldatorn* och hello *källdatorn* är registrerade toohello samma Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="3e07f-229">Ensure hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>

3. <span data-ttu-id="3e07f-230">Klicka på **återställa Data** tooopen hello **guiden återställa Data**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-230">Click **Recover Data** tooopen hello **Recover Data wizard**.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="3e07f-232">På hello **komma igång** väljer **en annan server**</span><span class="sxs-lookup"><span data-stu-id="3e07f-232">On hello **Getting Started** pane, select **Another server**</span></span>

    ![En annan Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="3e07f-234">Ange hello valvautentiseringsfilen som motsvarar toohello *exempel valvet*, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-234">Provide hello vault credential file that corresponds toohello *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="3e07f-235">Om hello valvautentiseringsfilen är ogiltig (eller har upphört att gälla), ladda ned en ny valvautentiseringsfil från hello *exempel valvet* i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3e07f-235">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="3e07f-236">När du har angett en giltig valvautentiseringen visas hello hello motsvarande Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="3e07f-236">Once you provide a valid vault credential, hello name of hello corresponding Backup Vault appears.</span></span>

6. <span data-ttu-id="3e07f-237">På hello **Välj Backup Server** rutan, Välj hello *källdatorn* hello listan över datorer som visas och ange hello lösenfras.</span><span class="sxs-lookup"><span data-stu-id="3e07f-237">On hello **Select Backup Server** pane, select hello *Source machine* from hello list of displayed machines and provide hello passphrase.</span></span> <span data-ttu-id="3e07f-238">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-238">Then click **Next**.</span></span>

    ![Lista över datorer](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="3e07f-240">På hello **Välj återställningsläge** väljer **enskilda filer och mappar** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-240">On hello **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Söka](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="3e07f-242">På hello **Välj volym och datum** rutan, Välj hello volymen som innehåller hello filer och/eller mappar som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="3e07f-242">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="3e07f-243">Välj en återställningspunkt hello kalendern.</span><span class="sxs-lookup"><span data-stu-id="3e07f-243">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="3e07f-244">Du kan återställa från valfri återställningspunkt i tid.</span><span class="sxs-lookup"><span data-stu-id="3e07f-244">You can restore from any recovery point in time.</span></span> <span data-ttu-id="3e07f-245">Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="3e07f-245">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="3e07f-246">När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="3e07f-246">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Sökobjekt](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="3e07f-248">Klicka på **montera** toolocally hello recovery monteringspunkt som en återställning volym på din *måldatorn*.</span><span class="sxs-lookup"><span data-stu-id="3e07f-248">Click **Mount** toolocally mount hello recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="3e07f-249">På hello **Bläddra och återställa filer** rutan klickar du på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill.</span><span class="sxs-lookup"><span data-stu-id="3e07f-249">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="3e07f-251">I Utforskaren, kopiera hello filer och mappar från hello recovery volym och klistra in tooyour *måldatorn* plats.</span><span class="sxs-lookup"><span data-stu-id="3e07f-251">In Windows Explorer, copy hello files and/or folders from hello recovery volume and paste them tooyour *Target machine* location.</span></span> <span data-ttu-id="3e07f-252">Du kan öppna eller strömma hello filer direkt från hello recovery volym och verifiera hello rätt versioner återställs.</span><span class="sxs-lookup"><span data-stu-id="3e07f-252">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="3e07f-254">När du är klar återställer hello filer och/eller mappar på hello **Bläddra och återställningsfiler** rutan klickar du på **koppla från**.</span><span class="sxs-lookup"><span data-stu-id="3e07f-254">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="3e07f-255">Klicka på **Ja** tooconfirm som du vill toounmount hello volym.</span><span class="sxs-lookup"><span data-stu-id="3e07f-255">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="3e07f-257">Om du inte klicka på Koppla från, förblir hello Recovery volym monterade under sex timmar från hello tid när den är monterad.</span><span class="sxs-lookup"><span data-stu-id="3e07f-257">If you do not click Unmount, hello Recovery Volume will remain mounted for six hours from hello time when it was mounted.</span></span> <span data-ttu-id="3e07f-258">Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad.</span><span class="sxs-lookup"><span data-stu-id="3e07f-258">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="3e07f-259">Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.</span><span class="sxs-lookup"><span data-stu-id="3e07f-259">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="next-steps"></a><span data-ttu-id="3e07f-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3e07f-260">Next steps</span></span>
* [<span data-ttu-id="3e07f-261">Azure Backup vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="3e07f-261">Azure Backup FAQ</span></span>](backup-azure-backup-faq.md)
* <span data-ttu-id="3e07f-262">Besök hello [Azure Backup-forumet](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span><span class="sxs-lookup"><span data-stu-id="3e07f-262">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933).</span></span>

## <a name="learn-more"></a><span data-ttu-id="3e07f-263">Läs mer</span><span class="sxs-lookup"><span data-stu-id="3e07f-263">Learn more</span></span>
* [<span data-ttu-id="3e07f-264">Översikt över Azure Backup</span><span class="sxs-lookup"><span data-stu-id="3e07f-264">Azure Backup Overview</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=222425)
* [<span data-ttu-id="3e07f-265">Säkerhetskopiera Azure virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3e07f-265">Backup Azure virtual machines</span></span>](backup-azure-vms-introduction.md)
* [<span data-ttu-id="3e07f-266">Säkerhetskopiera Microsoft-arbetsbelastningar</span><span class="sxs-lookup"><span data-stu-id="3e07f-266">Backup up Microsoft workloads</span></span>](backup-azure-dpm-introduction.md)
