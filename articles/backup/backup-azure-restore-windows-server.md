---
title: aaaRestore data i Azure tooa Windows Server eller Windows-dator | Microsoft Docs
description: "Lär dig hur toorestore data lagras i Azure tooa Windows Server eller Windows-dator."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 742f4b9e-c0ab-4eeb-8e22-ee29b83c22c4
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/16/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: 11335495e448986a74f1733406f87e04331641d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-files-tooa-windows-server-or-windows-client-machine-using-resource-manager-deployment-model"></a><span data-ttu-id="32ac3-103">Återställ filer tooa Windows server eller Windows-klientdatorn med hjälp av Resource Manager-distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="32ac3-103">Restore files tooa Windows server or Windows client machine using Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="32ac3-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="32ac3-104">Azure portal</span></span>](backup-azure-restore-windows-server.md)
> * [<span data-ttu-id="32ac3-105">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="32ac3-105">Classic portal</span></span>](backup-azure-restore-windows-server-classic.md)
>
>

<span data-ttu-id="32ac3-106">Den här artikeln förklarar hur toorestore data från ett säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="32ac3-106">This article explains how toorestore data from a backup vault.</span></span> <span data-ttu-id="32ac3-107">toorestore data använder du guiden för hello återställa Data i hello Microsoft Azure Recovery Services MARS-agenten.</span><span class="sxs-lookup"><span data-stu-id="32ac3-107">toorestore data, you use hello Recover Data wizard in hello Microsoft Azure Recovery Services (MARS) agent.</span></span> <span data-ttu-id="32ac3-108">När du återställer data är det möjligt att:</span><span class="sxs-lookup"><span data-stu-id="32ac3-108">When you restore data, it is possible to:</span></span>

* <span data-ttu-id="32ac3-109">Återställ data toohello samma datorn från vilken hello säkerhetskopieringar som utförts.</span><span class="sxs-lookup"><span data-stu-id="32ac3-109">Restore data toohello same machine from which hello backups were taken.</span></span>
* <span data-ttu-id="32ac3-110">Återställa data tooan alternativa dator.</span><span class="sxs-lookup"><span data-stu-id="32ac3-110">Restore data tooan alternate machine.</span></span>

<span data-ttu-id="32ac3-111">Microsoft släppt en Preview update toohello MARS-agenten i januari 2017.</span><span class="sxs-lookup"><span data-stu-id="32ac3-111">In January 2017, Microsoft released a Preview update toohello MARS agent.</span></span> <span data-ttu-id="32ac3-112">Tillsammans med felkorrigeringar, kan du använda snabbmeddelanden återställa, vilket gör att du toomount en skrivbar återställning punkt ögonblicksbild som en återställningspunkt-volym.</span><span class="sxs-lookup"><span data-stu-id="32ac3-112">Along with bug fixes, this update enables Instant Restore, which allows you toomount a writeable recovery point snapshot as a recovery volume.</span></span> <span data-ttu-id="32ac3-113">Du kan sedan utforska hello recovery volym och kopiera filer tooa lokal dator vilket selektivt återställningen.</span><span class="sxs-lookup"><span data-stu-id="32ac3-113">You can then explore hello recovery volume and copy files tooa local computer thereby selectively restoring files.</span></span>

> [!NOTE]
> <span data-ttu-id="32ac3-114">Hej [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) krävs om du vill toouse omedelbar återställning toorestore av data.</span><span class="sxs-lookup"><span data-stu-id="32ac3-114">hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) is required if you want toouse Instant Restore toorestore data.</span></span> <span data-ttu-id="32ac3-115">Hello säkerhetskopierade data måste också skyddas i valv i språk som anges i hello support-artikeln.</span><span class="sxs-lookup"><span data-stu-id="32ac3-115">Also hello backup data must be protected in vaults in locales listed in hello support article.</span></span> <span data-ttu-id="32ac3-116">Kontakta hello [januari 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) hello senaste lista över språk som har stöd för omedelbar återställning.</span><span class="sxs-lookup"><span data-stu-id="32ac3-116">Consult hello [January 2017 Azure Backup update](https://support.microsoft.com/en-us/help/3216528?preview) for hello latest list of locales that support Instant Restore.</span></span> <span data-ttu-id="32ac3-117">Omedelbar återställning **inte** är tillgänglig i alla språk.</span><span class="sxs-lookup"><span data-stu-id="32ac3-117">Instant Restore is **not** currently available in all locales.</span></span>
>

<span data-ttu-id="32ac3-118">Omedelbar återställning är tillgänglig för användning i Recovery Services-valv i hello Azure-portalen och säkerhetskopieringsvalv i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="32ac3-118">Instant Restore is available for use in Recovery Services vaults in hello Azure portal and Backup vaults in hello classic portal.</span></span> <span data-ttu-id="32ac3-119">Om du vill toouse Instant återställa hämta hello MARS uppdateringen och följ hello procedurer som nämner omedelbar återställning.</span><span class="sxs-lookup"><span data-stu-id="32ac3-119">If you want toouse Instant Restore, download hello MARS update, and follow hello procedures that mention Instant Restore.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]

## <a name="use-instant-restore-toorecover-data-toohello-same-machine"></a><span data-ttu-id="32ac3-120">Använda snabbmeddelanden återställa toorecover data toohello samma dator</span><span class="sxs-lookup"><span data-stu-id="32ac3-120">Use Instant Restore toorecover data toohello same machine</span></span>

<span data-ttu-id="32ac3-121">Om du av misstag tar bort en fil- och vill toorestore den toohello samma dator (från vilka hello säkerhetskopiering tas), hello följande steg hjälper dig att återställa hello data.</span><span class="sxs-lookup"><span data-stu-id="32ac3-121">If you accidentally deleted a file and wish toorestore it toohello same machine (from which hello backup is taken), hello following steps will help you recover hello data.</span></span>

1. <span data-ttu-id="32ac3-122">Öppna hello **Microsoft Azure Backup** fästa i.</span><span class="sxs-lookup"><span data-stu-id="32ac3-122">Open hello **Microsoft Azure Backup** snap in.</span></span> <span data-ttu-id="32ac3-123">Om du inte vet där hello snapin-modulen installerades, söka hello dator eller server för **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-123">If you don't know where hello snap in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="32ac3-124">Hej skrivbordsapp ska visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="32ac3-124">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="32ac3-125">Klicka på **återställa Data** toostart hello guiden.</span><span class="sxs-lookup"><span data-stu-id="32ac3-125">Click **Recover Data** toostart hello wizard.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="32ac3-127">På hello **komma igång** rutan, toorestore hello data toohello samma server eller dator, Välj **servern (`<server name>`)** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-127">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Välj den här servern alternativet toorestore hello data toohello samma dator](./media/backup-azure-restore-windows-server/samemachine_gettingstarted_instantrestore.png)

4. <span data-ttu-id="32ac3-129">På hello **Välj återställningsläge** fönstret Välj **enskilda filer och mappar** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-129">On hello **Select Recovery Mode** pane, choose **Individual files and folders** and then click **Next**.</span></span>

    ![Bläddra bland filer](./media/backup-azure-restore-windows-server/samemachine_selectrecoverymode_instantrestore.png)

5. <span data-ttu-id="32ac3-131">På hello **Välj volym och datum** rutan, Välj hello volymen som innehåller hello filer och/eller mappar som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="32ac3-131">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="32ac3-132">Välj en återställningspunkt hello kalendern.</span><span class="sxs-lookup"><span data-stu-id="32ac3-132">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="32ac3-133">Du kan återställa från valfri återställningspunkt i tid.</span><span class="sxs-lookup"><span data-stu-id="32ac3-133">You can restore from any recovery point in time.</span></span> <span data-ttu-id="32ac3-134">Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="32ac3-134">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="32ac3-135">När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="32ac3-135">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Volym och datum](./media/backup-azure-restore-windows-server/samemachine_selectvolumedate_instantrestore.png)

6. <span data-ttu-id="32ac3-137">När du har valt hello recovery punkt toorestore, klickar du på **montera**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-137">Once you have chosen hello recovery point toorestore, click **Mount**.</span></span>

    <span data-ttu-id="32ac3-138">Azure-säkerhetskopiering monterar hello lokal återställningspunkt och använder den som en återställningspunkt-volym.</span><span class="sxs-lookup"><span data-stu-id="32ac3-138">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="32ac3-139">På hello **Bläddra och återställa filer** rutan klickar du på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill.</span><span class="sxs-lookup"><span data-stu-id="32ac3-139">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Återställningsalternativ](./media/backup-azure-restore-windows-server/samemachine_browserecover_instantrestore.png)


8. <span data-ttu-id="32ac3-141">I Utforskaren, kopiera hello filer och mappar du vill toorestore och klistra in dem tooany lokala toohello servern eller datorn.</span><span class="sxs-lookup"><span data-stu-id="32ac3-141">In Windows Explorer, copy hello files and/or folders you want toorestore and paste them tooany location local toohello server or computer.</span></span> <span data-ttu-id="32ac3-142">Du kan öppna eller strömma hello filer direkt från hello recovery volym och verifiera hello rätt versioner återställs.</span><span class="sxs-lookup"><span data-stu-id="32ac3-142">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Kopiera och klistra in filer och mappar från monterad volym toolocal plats](./media/backup-azure-restore-windows-server/samemachine_copy_instantrestore.png)

9. <span data-ttu-id="32ac3-144">När du är klar återställer hello filer och/eller mappar på hello **Bläddra och återställningsfiler** rutan klickar du på **koppla från**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-144">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="32ac3-145">Klicka på **Ja** tooconfirm som du vill toounmount hello volym.</span><span class="sxs-lookup"><span data-stu-id="32ac3-145">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Demontera hello volym och bekräfta](./media/backup-azure-restore-windows-server/samemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="32ac3-147">Om du inte klicka på Koppla från, förblir hello Recovery volym monterade i 6 timmar från hello tid när den är monterad.</span><span class="sxs-lookup"><span data-stu-id="32ac3-147">If you do not click Unmount, hello Recovery Volume will remain mounted for 6 hours from hello time when it was mounted.</span></span> <span data-ttu-id="32ac3-148">Hello monteringstid är dock utökade upp till 24 timmar vid en pågående filkopiering maximalt.</span><span class="sxs-lookup"><span data-stu-id="32ac3-148">However, hello mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="32ac3-149">Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad.</span><span class="sxs-lookup"><span data-stu-id="32ac3-149">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="32ac3-150">Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.</span><span class="sxs-lookup"><span data-stu-id="32ac3-150">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >


## <a name="use-instant-restore-toorestore-data-tooan-alternate-machine"></a><span data-ttu-id="32ac3-151">Använda snabbmeddelanden återställa toorestore data tooan alternativa dator</span><span class="sxs-lookup"><span data-stu-id="32ac3-151">Use Instant Restore toorestore data tooan alternate machine</span></span>
<span data-ttu-id="32ac3-152">Du kan fortfarande återställa data från Azure Backup tooa annan dator om hela servern går förlorad.</span><span class="sxs-lookup"><span data-stu-id="32ac3-152">If your entire server is lost, you can still recover data from Azure Backup tooa different machine.</span></span> <span data-ttu-id="32ac3-153">hello följande steg visar hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="32ac3-153">hello following steps illustrate hello workflow.</span></span>


<span data-ttu-id="32ac3-154">hello-terminologi som används i de här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="32ac3-154">hello terminology used in these steps includes:</span></span>

* <span data-ttu-id="32ac3-155">*Källdatorn* – hello ursprungliga datorn från vilken hello säkerhetskopian skapades och som inte är tillgänglig för tillfället.</span><span class="sxs-lookup"><span data-stu-id="32ac3-155">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
* <span data-ttu-id="32ac3-156">*Måldatorn* – hello datorn toowhich hello data återställs.</span><span class="sxs-lookup"><span data-stu-id="32ac3-156">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
* <span data-ttu-id="32ac3-157">*Exempel valvet* – hello Recovery Services-valvet toowhich hello *källdatorn* och *måldatorn* registreras.</span><span class="sxs-lookup"><span data-stu-id="32ac3-157">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="32ac3-158">Säkerhetskopieringar kan inte vara återställda tooa måldatorn som kör en tidigare version av operativsystemet för hello.</span><span class="sxs-lookup"><span data-stu-id="32ac3-158">Backups can't be restored tooa target machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="32ac3-159">Till exempel återställts en säkerhetskopia som gjorts från en Windows 7-dator kan vara på en Windows 8 eller senare, dator.</span><span class="sxs-lookup"><span data-stu-id="32ac3-159">For example, a backup taken from a Windows 7 computer can be restored on a Windows 8, or later, computer.</span></span> <span data-ttu-id="32ac3-160">En säkerhetskopia som gjorts från en Windows 8-dator kan inte vara återställda tooa Windows 7-dator.</span><span class="sxs-lookup"><span data-stu-id="32ac3-160">A backup taken from a Windows 8 computer cannot be restored tooa Windows 7 computer.</span></span>
>
>

1. <span data-ttu-id="32ac3-161">Öppna hello **Microsoft Azure Backup** fästa i på hello *måldatorn*.</span><span class="sxs-lookup"><span data-stu-id="32ac3-161">Open hello **Microsoft Azure Backup** snap in on hello *Target machine*.</span></span>

2. <span data-ttu-id="32ac3-162">Se till att hello *måldatorn* och hello *källdatorn* är registrerade toohello samma Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="32ac3-162">Ensure hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>

3. <span data-ttu-id="32ac3-163">Klicka på **återställa Data** tooopen hello **guiden återställa Data**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-163">Click **Recover Data** tooopen hello **Recover Data wizard**.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

4. <span data-ttu-id="32ac3-165">På hello **komma igång** väljer **en annan server**</span><span class="sxs-lookup"><span data-stu-id="32ac3-165">On hello **Getting Started** pane, select **Another server**</span></span>

    ![En annan Server](./media/backup-azure-restore-windows-server/alternatemachine_gettingstarted_instantrestore.png)

5. <span data-ttu-id="32ac3-167">Ange hello valvautentiseringsfilen som motsvarar toohello *exempel valvet*, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-167">Provide hello vault credential file that corresponds toohello *Sample vault*, and click **Next**.</span></span>

    <span data-ttu-id="32ac3-168">Om hello valvautentiseringsfilen är ogiltig (eller har upphört att gälla), ladda ned en ny valvautentiseringsfil från hello *exempel valvet* i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="32ac3-168">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="32ac3-169">När du har angett en giltig valvautentiseringen visas hello hello motsvarande Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="32ac3-169">Once you provide a valid vault credential, hello name of hello corresponding Backup Vault appears.</span></span>


6. <span data-ttu-id="32ac3-170">På hello **Välj Backup Server** rutan, Välj hello *källdatorn* hello listan över datorer som visas och ange hello lösenfras.</span><span class="sxs-lookup"><span data-stu-id="32ac3-170">On hello **Select Backup Server** pane, select hello *Source machine* from hello list of displayed machines and provide hello passphrase.</span></span> <span data-ttu-id="32ac3-171">Klicka sedan på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-171">Then click **Next**.</span></span>

    ![Lista över datorer](./media/backup-azure-restore-windows-server/alternatemachine_selectmachine_instantrestore.png)

7. <span data-ttu-id="32ac3-173">På hello **Välj återställningsläge** väljer **enskilda filer och mappar** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-173">On hello **Select Recovery Mode** pane, select **Individual files and folders** and click **Next**.</span></span>

    ![Söka](./media/backup-azure-restore-windows-server/alternatemachine_selectrecoverymode_instantrestore.png)

8. <span data-ttu-id="32ac3-175">På hello **Välj volym och datum** rutan, Välj hello volymen som innehåller hello filer och/eller mappar som du vill toorestore.</span><span class="sxs-lookup"><span data-stu-id="32ac3-175">On hello **Select Volume and Date** pane, select hello volume that contains hello files and/or folders you want toorestore.</span></span>

    <span data-ttu-id="32ac3-176">Välj en återställningspunkt hello kalendern.</span><span class="sxs-lookup"><span data-stu-id="32ac3-176">On hello calendar, select a recovery point.</span></span> <span data-ttu-id="32ac3-177">Du kan återställa från valfri återställningspunkt i tid.</span><span class="sxs-lookup"><span data-stu-id="32ac3-177">You can restore from any recovery point in time.</span></span> <span data-ttu-id="32ac3-178">Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="32ac3-178">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="32ac3-179">När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="32ac3-179">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Sökobjekt](./media/backup-azure-restore-windows-server/alternatemachine_selectvolumedate_instantrestore.png)

9. <span data-ttu-id="32ac3-181">Klicka på **montera** toolocally hello recovery monteringspunkt som en återställning volym på din *måldatorn*.</span><span class="sxs-lookup"><span data-stu-id="32ac3-181">Click **Mount** toolocally mount hello recovery point as a recovery volume on your *Target machine*.</span></span>

10. <span data-ttu-id="32ac3-182">På hello **Bläddra och återställa filer** rutan klickar du på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill.</span><span class="sxs-lookup"><span data-stu-id="32ac3-182">On hello **Browse and Recover Files** pane, click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span>

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_browserecover_instantrestore.png)

11. <span data-ttu-id="32ac3-184">I Utforskaren, kopiera hello filer och mappar från hello recovery volym och klistra in tooyour *måldatorn* plats.</span><span class="sxs-lookup"><span data-stu-id="32ac3-184">In Windows Explorer, copy hello files and/or folders from hello recovery volume and paste them tooyour *Target machine* location.</span></span> <span data-ttu-id="32ac3-185">Du kan öppna eller strömma hello filer direkt från hello recovery volym och verifiera hello rätt versioner återställs.</span><span class="sxs-lookup"><span data-stu-id="32ac3-185">You can open or stream hello files directly from hello recovery volume and verify hello correct versions are recovered.</span></span>

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_copy_instantrestore.png)

12. <span data-ttu-id="32ac3-187">När du är klar återställer hello filer och/eller mappar på hello **Bläddra och återställningsfiler** rutan klickar du på **koppla från**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-187">When you are finished restoring hello files and/or folders, on hello **Browse and Recovery Files** pane, click **Unmount**.</span></span> <span data-ttu-id="32ac3-188">Klicka på **Ja** tooconfirm som du vill toounmount hello volym.</span><span class="sxs-lookup"><span data-stu-id="32ac3-188">Then click **Yes** tooconfirm that you want toounmount hello volume.</span></span>

    ![Kryptering](./media/backup-azure-restore-windows-server/alternatemachine_unmount_instantrestore.png)

    > [!Important]
    > <span data-ttu-id="32ac3-190">Om du inte klicka på Koppla från, förblir hello Recovery volym monterade i 6 timmar från hello tid när den är monterad.</span><span class="sxs-lookup"><span data-stu-id="32ac3-190">If you do not click Unmount, hello Recovery Volume will remain mounted for 6 hours from hello time when it was mounted.</span></span> <span data-ttu-id="32ac3-191">Hello monteringstid är dock utökade upp till 24 timmar vid en pågående filkopiering maximalt.</span><span class="sxs-lookup"><span data-stu-id="32ac3-191">However, hello mount time is extended upto a maximum of 24 hours in case of an ongoing file-copy.</span></span> <span data-ttu-id="32ac3-192">Inga säkerhetskopieringsåtgärder kommer att köras när hello volym är monterad.</span><span class="sxs-lookup"><span data-stu-id="32ac3-192">No backup operations will run while hello volume is mounted.</span></span> <span data-ttu-id="32ac3-193">Alla Säkerhetskopieringsåtgärden schemalagda toorun under hello tiden när hello volym är monterad, körs när hello recovery volymen är frånkopplade.</span><span class="sxs-lookup"><span data-stu-id="32ac3-193">Any backup operation scheduled toorun during hello time when hello volume is mounted, will run after hello recovery volume is unmounted.</span></span>
    >

## <a name="troubleshooting"></a><span data-ttu-id="32ac3-194">Felsökning</span><span class="sxs-lookup"><span data-stu-id="32ac3-194">Troubleshooting</span></span>
<span data-ttu-id="32ac3-195">Om Azure Backup har inte monterar hello recovery volymen även efter flera minuter om du klickar på **montera** eller misslyckas toomount hello recovery volym med ett eller flera fel gör hello nedan toobegin återställer normalt.</span><span class="sxs-lookup"><span data-stu-id="32ac3-195">If Azure Backup does not successfully mount hello recovery volume even after several minutes of clicking **Mount** or fails toomount hello recovery volume with one or more errors, follow hello steps below toobegin recovering normally.</span></span>

1.  <span data-ttu-id="32ac3-196">Avbryt hello pågående montera processen om det körs under flera minuter.</span><span class="sxs-lookup"><span data-stu-id="32ac3-196">Cancel hello ongoing mount process in case it has been running for several minutes.</span></span>

2.  <span data-ttu-id="32ac3-197">Se till att du är på hello senaste versionen av hello Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="32ac3-197">Ensure that you are on hello latest version of hello Azure Backup agent.</span></span> <span data-ttu-id="32ac3-198">toofind hello version information för Azure Backup-agenten, klicka på **om Microsoft Azure Recovery Services-agenten** på hello **åtgärder** fönstret Microsoft Azure Backup konsolen och se till att hello  **Version** antalet är lika tooor som är högre än hello-version som nämns i [i den här artikeln](https://go.microsoft.com/fwlink/?linkid=229525).</span><span class="sxs-lookup"><span data-stu-id="32ac3-198">toofind out hello version information of Azure Backup agent, click on **About Microsoft Azure Recovery Services Agent** on hello **Actions** pane of Microsoft Azure Backup console and ensure that hello **Version** number is equal tooor higher than hello version mentioned in [this article](https://go.microsoft.com/fwlink/?linkid=229525).</span></span> <span data-ttu-id="32ac3-199">Du kan hämta hello senaste versionen från [här](https://go.microsoft.com/fwLink/?LinkID=288905)</span><span class="sxs-lookup"><span data-stu-id="32ac3-199">You can download hello latest version from [here](https://go.microsoft.com/fwLink/?LinkID=288905)</span></span>

3.  <span data-ttu-id="32ac3-200">Gå för**Enhetshanteraren** -> **lagringsstyrenheter** och se till att du kan hitta **Microsoft iSCSI Initiator**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-200">Go too**Device Manager** -> **Storage Controllers** and ensure that you can locate **Microsoft iSCSI Initiator**.</span></span> <span data-ttu-id="32ac3-201">Om du kan hitta kan gå direkt toostep 7 nedan.</span><span class="sxs-lookup"><span data-stu-id="32ac3-201">If you can locate it, directly go toostep 7 below.</span></span> 

4.  <span data-ttu-id="32ac3-202">Om du inte hittar Microsoft iSCSI Initiator service som anges i steg 3, kontrollera toosee om du kan hitta en post under **Enhetshanteraren** -> **lagringsstyrenheter** kallas  **Okänd enhet** med maskinvaru-ID **ROOT\ISCSIPRT**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-202">If you cannot locate Microsoft iSCSI Initiator service as mentioned in step 3, check toosee if you can find an entry under **Device Manager** -> **Storage Controllers** called **Unknown Device** with Hardware ID **ROOT\ISCSIPRT**.</span></span>

5.  <span data-ttu-id="32ac3-203">Högerklicka på **okänd enhet** och välj **Uppdatera drivrutin**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-203">Right click on **Unknown Device** and select **Update Driver Software**.</span></span>

6.  <span data-ttu-id="32ac3-204">Uppdatera hello drivrutin genom att välja hello alternativ för **Sök automatiskt efter uppdaterade drivrutiner**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-204">Update hello driver by selecting hello option too **Search automatically for updated driver software**.</span></span> <span data-ttu-id="32ac3-205">Slutförande av hello uppdatering bör ändra **okänd enhet** för**Microsoft iSCSI Initiator** enligt nedan.</span><span class="sxs-lookup"><span data-stu-id="32ac3-205">Completion of hello update should change **Unknown Device** too**Microsoft iSCSI Initiator** as shown below.</span></span> 

    ![Kryptering](./media/backup-azure-restore-windows-server/UnknowniSCSIDevice.png)

7.  <span data-ttu-id="32ac3-207">Gå för**Aktivitetshanteraren** -> **tjänster (lokala)** -> **Microsoft iSCSI Initiator Service**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-207">Go too**Task Manager** -> **Services (Local)** -> **Microsoft iSCSI Initiator Service**.</span></span> 

    ![Kryptering](./media/backup-azure-restore-windows-server/MicrosoftInitiatorServiceRunning.png)
    
8.  <span data-ttu-id="32ac3-209">Starta om hello Microsoft iSCSI Initiator service genom att högerklicka på hello-tjänsten, klicka på **stoppa** och ytterligare Högerklicka igen och klicka på **starta**.</span><span class="sxs-lookup"><span data-stu-id="32ac3-209">Restart hello Microsoft iSCSI Initiator service by right-clicking on hello service, clicking on **Stop** and further right clicking again and clicking on **Start**.</span></span>

9.  <span data-ttu-id="32ac3-210">Försök återställa med hjälp av omedelbar återställning.</span><span class="sxs-lookup"><span data-stu-id="32ac3-210">Retry recovering using Instant Restore.</span></span> 

<span data-ttu-id="32ac3-211">Starta om din serverklienten om hello återställningen fortfarande misslyckas.</span><span class="sxs-lookup"><span data-stu-id="32ac3-211">If hello recovery still fails, reboot your server/client.</span></span> <span data-ttu-id="32ac3-212">Om en omstart är inte önskvärt eller hello recovery fortfarande misslyckas efter hello servern startas om, försök att återställa från en annan dator och kontakta Azure stöder genom att gå för[Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) och skicka en supportförfrågan.</span><span class="sxs-lookup"><span data-stu-id="32ac3-212">If a reboot is not desirable or hello recovery still fails even after rebooting hello server, try recovering from an Alternate Machine, and contact Azure Support by going too[Azure Portal](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) and submitting a support request.</span></span>

## <a name="next-steps"></a><span data-ttu-id="32ac3-213">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="32ac3-213">Next steps</span></span>
* <span data-ttu-id="32ac3-214">Nu när du har återställt dina filer och mappar, kan du [hantera dina säkerhetskopieringar](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="32ac3-214">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
