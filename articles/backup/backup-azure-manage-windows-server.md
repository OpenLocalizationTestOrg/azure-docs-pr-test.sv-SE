---
title: aaaManage Azure recovery services-valv och servrar | Microsoft Docs
description: "Använd den här självstudiekursen toolearn hur toomanage Azure recovery services-valv och servrar."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: 4eea984b-7ed6-4600-ac60-99d2e9cb6d8a
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: markgal
ms.openlocfilehash: b4c35c86faa0828b3c63a13b85c095c0cbaba50e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="monitor-and-manage-azure-recovery-services-vaults-and-servers-for-windows-machines"></a><span data-ttu-id="59d14-103">Övervaka och hantera Azure Recovery Services-valv och servrar för Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="59d14-103">Monitor and manage Azure recovery services vaults and servers for Windows machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="59d14-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="59d14-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="59d14-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="59d14-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="59d14-106">I den här artikeln hittar du en översikt över hello säkerhetskopiering Övervakare och hanteringsuppgifter som är tillgängliga via hello Azure-portalen och hello Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="59d14-106">In this article you'll find an overview of hello backup monitor and management tasks available through hello Azure portal and hello Microsoft Azure Backup agent.</span></span> <span data-ttu-id="59d14-107">Den här artikeln förutsätter att du redan har en Azure-prenumeration och har skapat minst ett Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="59d14-107">This article assumes you already have an Azure subscription and have created at least one Recovery Services vault.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/learn-about-deployment-models-rm-include.md)]


## <a name="open-a-recovery-services-vault"></a><span data-ttu-id="59d14-108">Öppna ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="59d14-108">Open a Recovery Services vault</span></span>

<span data-ttu-id="59d14-109">hello Recovery Services-valvet instrumentpanelen visar hello information eller attribut för Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="59d14-109">hello Recovery Services vault dashboard shows you hello details or attributes of a Recovery Services vault.</span></span>

1. <span data-ttu-id="59d14-110">Logga in toohello [Azure Portal](https://portal.azure.com/) med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="59d14-110">Sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="59d14-111">Hej hubbmenyn, klicka på **fler tjänster**.</span><span class="sxs-lookup"><span data-stu-id="59d14-111">On hello Hub menu, click **More Services**.</span></span>

    ![Öppna en lista över Recovery Services-valv steg 1](./media/backup-azure-manage-windows-server/open-rs-vault-list.png) <br/>

3. <span data-ttu-id="59d14-113">Vill du tooopen Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="59d14-113">You want tooopen a Recovery Services vault.</span></span> <span data-ttu-id="59d14-114">I dialogrutan för hello börja skriva **återställningstjänster**.</span><span class="sxs-lookup"><span data-stu-id="59d14-114">In hello dialog box, start typing **Recovery Services**.</span></span> <span data-ttu-id="59d14-115">När du börjar skriva in hello listan filtreras baserat på dina indata.</span><span class="sxs-lookup"><span data-stu-id="59d14-115">As you begin typing, hello list will filter based on your input.</span></span> <span data-ttu-id="59d14-116">Klicka på **Recovery Services-valv** toodisplay hello lista över Recovery Services-valv i din prenumeration.</span><span class="sxs-lookup"><span data-stu-id="59d14-116">Click **Recovery Services vaults** toodisplay hello list of Recovery Services vaults in your subscription.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-manage-windows-server/browse-to-rs-vaults-2.png) <br/>

    <span data-ttu-id="59d14-118">hello lista över Recovery Services-valv öppnas.</span><span class="sxs-lookup"><span data-stu-id="59d14-118">hello list of Recovery Services vaults opens.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-manage-windows-server/list-of-rs-vaults.png) <br/>

4. <span data-ttu-id="59d14-120">Hello lista över valv, Välj hello namnet på hello Recovery Services-valv som du vill tooopen.</span><span class="sxs-lookup"><span data-stu-id="59d14-120">From hello list of vaults, select hello name of hello Recovery Services vault you want tooopen.</span></span> <span data-ttu-id="59d14-121">hello Recovery Services-valvet instrumentpanelen blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="59d14-121">hello Recovery Services vault dashboard blade opens.</span></span>

    ![Recovery services-valvet instrumentpanelen](./media/backup-azure-manage-windows-server/rs-vault-blade.png) <br/>

    <span data-ttu-id="59d14-123">Nu när du har öppnat hello Recovery Services-valvet, försöker du med hello övervakning eller hantering av uppgifter.</span><span class="sxs-lookup"><span data-stu-id="59d14-123">Now that you have opened hello Recovery Services vault, try any of hello monitoring or management tasks.</span></span>

## <a name="monitor-backup-jobs-and-alerts"></a><span data-ttu-id="59d14-124">Övervakaren säkerhetskopieringsjobb och -varningar</span><span class="sxs-lookup"><span data-stu-id="59d14-124">Monitor backup jobs and alerts</span></span>

<span data-ttu-id="59d14-125">Du övervaka jobb och aviseringar från hello återställningstjänster valvet instrumentpanel, där du ser:</span><span class="sxs-lookup"><span data-stu-id="59d14-125">You monitor jobs and alerts from hello Recovery Services vault dashboard, where you see:</span></span>

* <span data-ttu-id="59d14-126">Information om aviseringar om säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="59d14-126">Backup alerts details</span></span>
* <span data-ttu-id="59d14-127">Filer och mappar samt Azure virtuella datorer som skyddas i hello moln</span><span class="sxs-lookup"><span data-stu-id="59d14-127">Files and folders, as well as Azure virtual machines protected in hello cloud</span></span>
* <span data-ttu-id="59d14-128">Totalt lagringsutrymme som förbrukas i Azure</span><span class="sxs-lookup"><span data-stu-id="59d14-128">Total storage consumed in Azure</span></span>
* <span data-ttu-id="59d14-129">Status för säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="59d14-129">Backup job status</span></span>

![Säkerhetskopiering instrumentpanelsuppgifter](./media/backup-azure-manage-windows-server/dashboard-tiles.png)

<span data-ttu-id="59d14-131">Om du klickar på hello informationen i dessa brickor öppnas hello associerade bladet där du hanterar relaterade aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="59d14-131">Clicking hello information in each of these tiles will open hello associated blade where you manage related tasks.</span></span>

<span data-ttu-id="59d14-132">Från hello överkant hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="59d14-132">From hello top of hello Dashboard:</span></span>

* <span data-ttu-id="59d14-133">Inställningar som ger åtkomst tillgängliga aktiviteter för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="59d14-133">Settings provides access available backup tasks.</span></span>
* <span data-ttu-id="59d14-134">Backup - hjälper till att du säkerhetskopierar nya filer och mappar (eller virtuella Azure-datorer) toohello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="59d14-134">Backup - helps you back up new files and folders (or Azure VMs) toohello Recovery Services vault.</span></span>
* <span data-ttu-id="59d14-135">Ta bort – om en recovery services-valvet används inte längre kan du kan ta bort den toofree upp lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="59d14-135">Delete - If a recovery services vault is no longer being used, you can delete it toofree up storage space.</span></span> <span data-ttu-id="59d14-136">Ta bort aktiveras först när alla skyddade servrar har tagits bort från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="59d14-136">Delete is only enabled after all protected servers have been deleted from hello vault.</span></span>

![Säkerhetskopiering instrumentpanelsuppgifter](./media/backup-azure-manage-windows-server/dashboard-tasks.png)

## <a name="alerts-for-backups-using-azure-backup-agent"></a><span data-ttu-id="59d14-138">Aviseringar för säkerhetskopiering med Azure backup-agenten:</span><span class="sxs-lookup"><span data-stu-id="59d14-138">Alerts for backups using Azure backup agent:</span></span>
| <span data-ttu-id="59d14-139">Aviseringsnivå</span><span class="sxs-lookup"><span data-stu-id="59d14-139">Alert Level</span></span> | <span data-ttu-id="59d14-140">Aviseringar skickas</span><span class="sxs-lookup"><span data-stu-id="59d14-140">Alerts sent</span></span> |
| --- | --- |
| <span data-ttu-id="59d14-141">kritiska</span><span class="sxs-lookup"><span data-stu-id="59d14-141">Critical</span></span> |<span data-ttu-id="59d14-142">Säkerhetskopieringen har misslyckats, Återställningsfel</span><span class="sxs-lookup"><span data-stu-id="59d14-142">Backup failure, recovery failure</span></span> |
| <span data-ttu-id="59d14-143">Varning</span><span class="sxs-lookup"><span data-stu-id="59d14-143">Warning</span></span> |<span data-ttu-id="59d14-144">Slutfördes med varningar (när färre än 100 filerna säkerhetskopieras inte på grund av problem med toocorruption och mer än en miljon filer har säkerhetskopierats)</span><span class="sxs-lookup"><span data-stu-id="59d14-144">Backup completed with warnings (when fewer than one hundred files are not backed up due toocorruption issues, and more than one million files are successfully backed up)</span></span> |
| <span data-ttu-id="59d14-145">Information</span><span class="sxs-lookup"><span data-stu-id="59d14-145">Informational</span></span> |<span data-ttu-id="59d14-146">Ingen</span><span class="sxs-lookup"><span data-stu-id="59d14-146">None</span></span> |

## <a name="manage-backup-alerts"></a><span data-ttu-id="59d14-147">Hantera aviseringar för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="59d14-147">Manage Backup alerts</span></span>
<span data-ttu-id="59d14-148">Klicka på hello **säkerhetskopiering aviseringar** panelen tooopen hello **säkerhetskopiering aviseringar** bladet och hantera aviseringar.</span><span class="sxs-lookup"><span data-stu-id="59d14-148">Click hello **Backup Alerts** tile tooopen hello **Backup Alerts** blade and manage alerts.</span></span>

![Aviseringar om säkerhetskopiering](./media/backup-azure-manage-windows-server/manage-backup-alerts.png)

<span data-ttu-id="59d14-150">hello säkerhetskopiering aviseringar innehåller hello antalet:</span><span class="sxs-lookup"><span data-stu-id="59d14-150">hello Backup Alerts tile shows you hello number of:</span></span>

* <span data-ttu-id="59d14-151">kritiska aviseringar som inte matchas med de senaste 24 timmarna</span><span class="sxs-lookup"><span data-stu-id="59d14-151">critical alerts unresolved in last 24 hours</span></span>
* <span data-ttu-id="59d14-152">varningsaviseringar som inte matchas med de senaste 24 timmarna</span><span class="sxs-lookup"><span data-stu-id="59d14-152">warning alerts unresolved in last 24 hours</span></span>

<span data-ttu-id="59d14-153">Klicka på var och en av dessa länkar tar toohello **säkerhetskopiering aviseringar** bladet med en filtrerad vy av aviseringarna (kritiskt eller varning).</span><span class="sxs-lookup"><span data-stu-id="59d14-153">Clicking on each of these links takes you toohello **Backup Alerts** blade with a filtered view of these alerts (critical or warning).</span></span>

<span data-ttu-id="59d14-154">Hello säkerhetskopiering aviseringar bladet du:</span><span class="sxs-lookup"><span data-stu-id="59d14-154">From hello Backup Alerts blade, you:</span></span>

* <span data-ttu-id="59d14-155">Välj hello lämplig information tooinclude med aviseringarna.</span><span class="sxs-lookup"><span data-stu-id="59d14-155">Choose hello appropriate information tooinclude with your alerts.</span></span>

    ![Välj kolumner](./media/backup-azure-manage-windows-server/choose-alerts-colunms.png)
* <span data-ttu-id="59d14-157">Filtrera aviseringar efter allvarlighetsgrad, status och börja/sluta tider.</span><span class="sxs-lookup"><span data-stu-id="59d14-157">Filter alerts for severity, status and start/end times.</span></span>

    ![Filtrera aviseringar](./media/backup-azure-manage-windows-server/filter-alerts.png)
* <span data-ttu-id="59d14-159">Konfigurera aviseringar för allvarlighetsgrad, frekvens och mottagare, samt aktivera och inaktivera aviseringar.</span><span class="sxs-lookup"><span data-stu-id="59d14-159">Configure notifications for severity, frequency and recipients, as well as turn alerts on or off.</span></span>

    ![Filtrera aviseringar](./media/backup-azure-manage-windows-server/configure-notifications.png)

<span data-ttu-id="59d14-161">Om **Per avisering** är markerad som hello **Avisera** frekvens ingen gruppering eller minskning av e-post inträffar.</span><span class="sxs-lookup"><span data-stu-id="59d14-161">If **Per Alert** is selected as hello **Notify** frequency no grouping or reduction in emails occurs.</span></span> <span data-ttu-id="59d14-162">Alla aviseringar som resulterar i 1-meddelande.</span><span class="sxs-lookup"><span data-stu-id="59d14-162">Every alert results in 1 notification.</span></span> <span data-ttu-id="59d14-163">Detta är standardinställningen för hello och hello upplösning e-postmeddelande skickas också omedelbart.</span><span class="sxs-lookup"><span data-stu-id="59d14-163">This is hello default setting and hello resolution email is also sent out immediately.</span></span>

<span data-ttu-id="59d14-164">Om **timvis sammanfattad** är markerad som hello **Avisera** frekvens som en e-postmeddelande skickas toohello användare som talar om att det inte finns olöst nya aviseringar som genererats i hello senaste timmen.</span><span class="sxs-lookup"><span data-stu-id="59d14-164">If **Hourly Digest** is selected as hello **Notify** frequency one email is sent toohello user telling them that there are unresolved new alerts generated in hello last hour.</span></span> <span data-ttu-id="59d14-165">En lösning i e-postmeddelande skickas slutet hello hello timme.</span><span class="sxs-lookup"><span data-stu-id="59d14-165">A resolution email is sent out at hello end of hello hour.</span></span>

<span data-ttu-id="59d14-166">Aviseringar kan skickas för hello följande allvarlighetsgrader:</span><span class="sxs-lookup"><span data-stu-id="59d14-166">Alerts can be sent for hello following severity levels:</span></span>

* <span data-ttu-id="59d14-167">kritiska</span><span class="sxs-lookup"><span data-stu-id="59d14-167">critical</span></span>
* <span data-ttu-id="59d14-168">Varning</span><span class="sxs-lookup"><span data-stu-id="59d14-168">warning</span></span>
* <span data-ttu-id="59d14-169">Information</span><span class="sxs-lookup"><span data-stu-id="59d14-169">information</span></span>

<span data-ttu-id="59d14-170">Du inaktiverar hello avisering med hello **inaktivera** knapp i hello informationsbladet för jobbet.</span><span class="sxs-lookup"><span data-stu-id="59d14-170">You inactivate hello alert with hello **inactivate** button in hello job details blade.</span></span> <span data-ttu-id="59d14-171">När du klickar på Inaktivera, kan du ange upplösning anteckningar.</span><span class="sxs-lookup"><span data-stu-id="59d14-171">When you click inactivate, you can provide resolution notes.</span></span>

<span data-ttu-id="59d14-172">Du väljer hello kolumner du vill tooappear som en del av hello avisering med hello **Välj kolumner** knappen.</span><span class="sxs-lookup"><span data-stu-id="59d14-172">You choose hello columns you want tooappear as part of hello alert with hello **Choose columns** button.</span></span>

> [!NOTE]
> <span data-ttu-id="59d14-173">Från hello **inställningar** bladet du hanterar aviseringar om säkerhetskopiering genom att välja **övervakning och rapporter > aviseringar och händelser > Säkerhetskopieringsaviseringar** och sedan klicka på **Filter** eller  **Konfigurera meddelanden**.</span><span class="sxs-lookup"><span data-stu-id="59d14-173">From hello **Settings** blade, you manage backup alerts by selecting **Monitoring and Reports > Alerts and Events > Backup Alerts** and then clicking **Filter** or **Configure Notifications**.</span></span>
>
>

## <a name="manage-backup-items"></a><span data-ttu-id="59d14-174">Hantera säkerhetskopiering objekt</span><span class="sxs-lookup"><span data-stu-id="59d14-174">Manage Backup items</span></span>
<span data-ttu-id="59d14-175">Hantera lokala säkerhetskopiorna är nu tillgänglig i hello-hanteringsportalen.</span><span class="sxs-lookup"><span data-stu-id="59d14-175">Managing on-premises backups is now available in hello management portal.</span></span> <span data-ttu-id="59d14-176">Under hello säkerhetskopiering av hello instrumentpanelen hello **Säkerhetskopieringsobjekt** panelen visar hello antalet Säkerhetskopiera objekt skyddade toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="59d14-176">In hello Backup section of hello dashboard, hello **Backup Items** tile shows hello number of backup items protected toohello vault.</span></span>

<span data-ttu-id="59d14-177">Klicka på **-mappar** i hello panelen Backup-objekt.</span><span class="sxs-lookup"><span data-stu-id="59d14-177">Click **File-Folders** in hello Backup Items tile.</span></span>

![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-windows-server/backup-items-tile.png)

<span data-ttu-id="59d14-179">hello Säkerhetskopieringsobjekt öppnas i blad med hello filtrera uppsättningen tooFile-mappen där du vill se varje säkerhetskopieringen post i listan.</span><span class="sxs-lookup"><span data-stu-id="59d14-179">hello Backup Items blade opens with hello filter set tooFile-Folder where you see each specific backup item listed.</span></span>

![Säkerhetskopiera objekt](./media/backup-azure-manage-windows-server/backup-item-list.png)

<span data-ttu-id="59d14-181">Om du väljer ett specifikt objekt för säkerhetskopiering hello listan finns hello viktig information för objektet.</span><span class="sxs-lookup"><span data-stu-id="59d14-181">If you select a specific backup item from hello list, you see hello essential details for that item.</span></span>

> [!NOTE]
> <span data-ttu-id="59d14-182">Från hello **inställningar** bladet du hanterar filer och mappar genom att välja **skyddade objekt > säkerhetskopiering objekt** och sedan välja **-mappar** från hello nedrullningsbara meny.</span><span class="sxs-lookup"><span data-stu-id="59d14-182">From hello **Settings** blade, you manage files and folders by selecting **Protected Items > Backup Items** and then selecting **File-Folders** from hello drop down menu.</span></span>
>
>

![Säkerhetskopiera objekt från inställningar](./media/backup-azure-manage-windows-server/backup-files-and-folders.png)

## <a name="manage-backup-jobs"></a><span data-ttu-id="59d14-184">Hantera säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="59d14-184">Manage Backup jobs</span></span>
<span data-ttu-id="59d14-185">Säkerhetskopieringsjobb för både lokala (när hello lokal server säkerhetskopierar tooAzure) och Azure-säkerhetskopieringar visas i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="59d14-185">Backup jobs for both on-premises (when hello on-premises server is backing up tooAzure) and Azure backups are visible in hello dashboard.</span></span>

<span data-ttu-id="59d14-186">I hello säkerhetskopiering avsnittet hello instrumentpanelen visar hello säkerhetskopiering jobbet panelen hello antalet jobb:</span><span class="sxs-lookup"><span data-stu-id="59d14-186">In hello Backup section of hello dashboard, hello Backup job tile shows hello number of jobs:</span></span>

* <span data-ttu-id="59d14-187">pågår</span><span class="sxs-lookup"><span data-stu-id="59d14-187">in progress</span></span>
* <span data-ttu-id="59d14-188">kunde inte hello senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="59d14-188">failed in hello last 24 hours.</span></span>

<span data-ttu-id="59d14-189">toomanage säkerhetskopieringsjobb, klicka på hello **säkerhetskopieringsjobb** panelen, vilket öppnar bladet för hello säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="59d14-189">toomanage your backup jobs, click hello **Backup Jobs** tile, which opens hello Backup Jobs blade.</span></span>

![Säkerhetskopiera objekt från inställningar](./media/backup-azure-manage-windows-server/backup-jobs.png)

<span data-ttu-id="59d14-191">Du ändrar hello informationen i hello säkerhetskopieringsjobb bladet med hello **Välj kolumner** knappen hello överst på hello sidan.</span><span class="sxs-lookup"><span data-stu-id="59d14-191">You modify hello information available in hello Backup Jobs blade with hello **Choose columns** button at hello top of hello page.</span></span>

<span data-ttu-id="59d14-192">Använd hello **Filter** knappen tooselect mellan filer och mappar och säkerhetskopiering av Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="59d14-192">Use hello **Filter** button tooselect between Files and folders and Azure virtual machine backup.</span></span>

<span data-ttu-id="59d14-193">Om du inte ser den säkerhetskopierade filer och mappar, klickar du på **Filter** knappen hello överst i hello sida och välj **filer och mappar** hello objekttypen-menyn.</span><span class="sxs-lookup"><span data-stu-id="59d14-193">If you don't see your backed up files and folders, click **Filter** button at hello top of hello page and select **Files and folders** from hello Item Type menu.</span></span>

> [!NOTE]
> <span data-ttu-id="59d14-194">Från hello **inställningar** bladet du hantera säkerhetskopieringsjobb genom att välja **övervakning och rapporter > jobb > säkerhetskopieringsjobb** och sedan välja **-mappar** hello listrutan meny.</span><span class="sxs-lookup"><span data-stu-id="59d14-194">From hello **Settings** blade, you manage backup jobs by selecting **Monitoring and Reports > Jobs > Backup Jobs** and then selecting **File-Folders** from hello drop down menu.</span></span>
>
>

## <a name="monitor-backup-usage"></a><span data-ttu-id="59d14-195">Övervaka användningen av säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="59d14-195">Monitor Backup usage</span></span>
<span data-ttu-id="59d14-196">I hello säkerhetskopiering avsnittet hello instrumentpanelen visar ikonen för användning i säkerhetskopieringen av hello hello lagringsutrymme som förbrukas i Azure.</span><span class="sxs-lookup"><span data-stu-id="59d14-196">In hello Backup section of hello dashboard, hello Backup Usage tile shows hello storage consumed in Azure.</span></span> <span data-ttu-id="59d14-197">Lagringskvoten tillhandahålls för:</span><span class="sxs-lookup"><span data-stu-id="59d14-197">Storage usage is provided for:</span></span>

* <span data-ttu-id="59d14-198">Lagringskvoten på molnet LRS som är associerade med hello-valvet</span><span class="sxs-lookup"><span data-stu-id="59d14-198">Cloud LRS storage usage associated with hello vault</span></span>
* <span data-ttu-id="59d14-199">Lagringskvoten på molnet GRS som är associerade med hello-valvet</span><span class="sxs-lookup"><span data-stu-id="59d14-199">Cloud GRS storage usage associated with hello vault</span></span>

## <a name="manage-your-production-servers"></a><span data-ttu-id="59d14-200">Hantera produktionsservrarna</span><span class="sxs-lookup"><span data-stu-id="59d14-200">Manage your production servers</span></span>
<span data-ttu-id="59d14-201">toomanage produktionsservrarna, klicka på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="59d14-201">toomanage your production servers, click **Settings**.</span></span>

<span data-ttu-id="59d14-202">Klicka på under hantera **säkerhetskopiering infrastruktur > produktionsservrar**.</span><span class="sxs-lookup"><span data-stu-id="59d14-202">Under Manage click **Backup infrastructure > Production Servers**.</span></span>

<span data-ttu-id="59d14-203">hello produktionsservrar bladet en lista över alla tillgängliga produktionsservrar.</span><span class="sxs-lookup"><span data-stu-id="59d14-203">hello Production Servers blade lists of all your available production servers.</span></span> <span data-ttu-id="59d14-204">Klicka på en server i hello listan tooopen hello serverinformation.</span><span class="sxs-lookup"><span data-stu-id="59d14-204">Click on a server in hello list tooopen hello server details.</span></span>

![Skyddade objekt](./media/backup-azure-manage-windows-server/production-server-list.png)


## <a name="open-hello-azure-backup-agent"></a><span data-ttu-id="59d14-206">Öppna hello Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="59d14-206">Open hello Azure Backup agent</span></span>
<span data-ttu-id="59d14-207">Öppna hello **Microsoft Azure Backup-agenten** (du hittar det genom att söka igenom din dator för *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="59d14-207">Open hello **Microsoft Azure Backup agent** (you find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/snap-in-search.png)

<span data-ttu-id="59d14-209">Från hello **åtgärder** på hello höger i hello säkerhetskopieringsagent konsolen du utföra följande hanteringsuppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="59d14-209">From hello **Actions** available at hello right of hello backup agent console you perform hello following management tasks:</span></span>

* <span data-ttu-id="59d14-210">Registrera Server</span><span class="sxs-lookup"><span data-stu-id="59d14-210">Register Server</span></span>
* <span data-ttu-id="59d14-211">Schema för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="59d14-211">Schedule Backup</span></span>
* <span data-ttu-id="59d14-212">Säkerhetskopiera nu</span><span class="sxs-lookup"><span data-stu-id="59d14-212">Back Up now</span></span>
* <span data-ttu-id="59d14-213">Ändra egenskaper för</span><span class="sxs-lookup"><span data-stu-id="59d14-213">Change Properties</span></span>

![Microsoft Azure Backup agent konsolen åtgärder](./media/backup-azure-manage-windows-server/console-actions.png)

> [!NOTE]
> <span data-ttu-id="59d14-215">för**återställa Data**, se [återställa filer tooa Windows server eller Windows-klientdatorn](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="59d14-215">too**Recover Data**, see [Restore files tooa Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

## <a name="modify-hello-backup-schedule"></a><span data-ttu-id="59d14-216">Ändra schemat för säkerhetskopiering av hello</span><span class="sxs-lookup"><span data-stu-id="59d14-216">Modify hello backup schedule</span></span>
1. <span data-ttu-id="59d14-217">Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="59d14-217">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/schedule-backup.png)
2. <span data-ttu-id="59d14-219">I hello **schema Säkerhetskopieringsguiden** lämna hello **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="59d14-219">In hello **Schedule Backup Wizard** leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="59d14-221">Om du vill tooadd eller ändra objekt på hello **Välj objekt tooBackup** skärmen klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="59d14-221">If you want tooadd or change items, on hello **Select Items tooBackup** screen click **Add Items**.</span></span>

    <span data-ttu-id="59d14-222">Du kan också ange **Undantagsinställningar** från den här sidan i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="59d14-222">You can also set **Exclusion Settings** from this page in hello wizard.</span></span> <span data-ttu-id="59d14-223">Om du vill tooexclude filer eller filtyper läsa hello proceduren för att lägga till [undantagsinställningar](#manage-exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="59d14-223">If you want tooexclude files or file types read hello procedure for adding [exclusion settings](#manage-exclusion-settings).</span></span>
4. <span data-ttu-id="59d14-224">Välj hello filer och mappar som du vill att tooback och klicka på **okej**.</span><span class="sxs-lookup"><span data-stu-id="59d14-224">Select hello files and folders you want tooback up and click **Okay**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/add-items-modify.png)
5. <span data-ttu-id="59d14-226">Ange hello **Säkerhetskopieringsschemat** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="59d14-226">Specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="59d14-227">Du kan schemalägga varje dag (högst 3 gånger per dag) eller veckovisa säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="59d14-227">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Windows Server Backup-objekt](./media/backup-azure-manage-windows-server/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="59d14-229">Ange schemat för säkerhetskopiering av hello beskrivs i detalj i detta [artikel](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="59d14-229">Specifying hello backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

6. <span data-ttu-id="59d14-230">Välj hello **bevarandeprincip** för hello säkerhetskopia och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="59d14-230">Select hello **Retention Policy** for hello backup copy and click **Next**.</span></span>

    ![Windows Server Backup-objekt](./media/backup-azure-manage-windows-server/select-retention-policy-modify.png)
7. <span data-ttu-id="59d14-232">På hello **bekräftelse** skärmen granska hello information och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="59d14-232">On hello **Confirmation** screen review hello information and click **Finish**.</span></span>
8. <span data-ttu-id="59d14-233">När hello guiden är färdig med hello **Säkerhetskopieringsschemat**, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="59d14-233">Once hello wizard finishes creating hello **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="59d14-234">Ändrar skydd måste du bekräfta att säkerhetskopieringar utlöser korrekt genom att gå toohello **jobb** fliken och bekräftar att ändringarna visas i hello säkerhetskopiera jobb.</span><span class="sxs-lookup"><span data-stu-id="59d14-234">After modifying protection, you can confirm that backups are triggering correctly by going toohello **Jobs** tab and confirming that changes are reflected in hello backup jobs.</span></span>

## <a name="enable-network-throttling"></a><span data-ttu-id="59d14-235">Aktivera begränsning</span><span class="sxs-lookup"><span data-stu-id="59d14-235">Enable Network Throttling</span></span>

<span data-ttu-id="59d14-236">hello Azure Backup-agenten ger en begränsning flik där du toocontrol hur nätverksbandbredden används när data överfördes.</span><span class="sxs-lookup"><span data-stu-id="59d14-236">hello Azure Backup agent provides a Throttling tab which allows you toocontrol how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="59d14-237">Den här kontrollen kan vara användbart om du behöver tooback upp data under arbetstid men inte vill att hello säkerhetskopieringsprocessen toointerfere med andra Internettrafik.</span><span class="sxs-lookup"><span data-stu-id="59d14-237">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other internet traffic.</span></span> <span data-ttu-id="59d14-238">Begränsning av data transfer gäller tooback upp och återställa aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="59d14-238">Throttling of data transfer applies tooback up and restore activities.</span></span>  

<span data-ttu-id="59d14-239">tooenable begränsning:</span><span class="sxs-lookup"><span data-stu-id="59d14-239">tooenable throttling:</span></span>

1. <span data-ttu-id="59d14-240">I hello **säkerhetskopieringsagenten**, klickar du på **ändra egenskaper för**.</span><span class="sxs-lookup"><span data-stu-id="59d14-240">In hello **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="59d14-241">På hello ** begränsning fliken Välj **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder**.</span><span class="sxs-lookup"><span data-stu-id="59d14-241">On hello **Throttling tab, select **Enable internet bandwidth usage throttling for backup operations**.</span></span>

    ![Nätverksbegränsning](./media/backup-azure-manage-windows-server/throttling-dialog.png)

    <span data-ttu-id="59d14-243">När du har aktiverat begränsning, ange hello tillåtet bandbredd för överföring av säkerhetskopierade data under **arbetstid** och **icke-arbetstid**.</span><span class="sxs-lookup"><span data-stu-id="59d14-243">Once you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="59d14-244">hello bandbredd värden börjar på 512 kilobit per sekund (Kbps) och gå upp too1023 megabyte per sekund (Mbps).</span><span class="sxs-lookup"><span data-stu-id="59d14-244">hello bandwidth values begin at 512 kilobytes per second (Kbps) and can go up too1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="59d14-245">Du kan också ange hello start och avslutas för **arbetstid**, och vilka dagar i veckan för hello betraktas arbete dagar.</span><span class="sxs-lookup"><span data-stu-id="59d14-245">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered Work days.</span></span> <span data-ttu-id="59d14-246">hello är utanför hello avses arbetstid övervägt toobe icke-arbetstid.</span><span class="sxs-lookup"><span data-stu-id="59d14-246">hello time outside of hello designated Work hours is considered toobe non-work hours.</span></span>
3. <span data-ttu-id="59d14-247">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="59d14-247">Click **OK**.</span></span>

## <a name="manage-exclusion-settings"></a><span data-ttu-id="59d14-248">Hantera undantagsinställningar</span><span class="sxs-lookup"><span data-stu-id="59d14-248">Manage exclusion settings</span></span>
1. <span data-ttu-id="59d14-249">Öppna hello **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="59d14-249">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/snap-in-search.png)
2. <span data-ttu-id="59d14-251">Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="59d14-251">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/schedule-backup.png)
3. <span data-ttu-id="59d14-253">Lämna hello i hello schema Säkerhetskopieringsguiden **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="59d14-253">In hello Schedule Backup Wizard leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="59d14-255">Klicka på **undantag inställningar**.</span><span class="sxs-lookup"><span data-stu-id="59d14-255">Click **Exclusions Settings**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclusion-settings.png)
5. <span data-ttu-id="59d14-257">Klicka på **Lägg till undantag**.</span><span class="sxs-lookup"><span data-stu-id="59d14-257">Click **Add Exclusion**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/add-exclusion.png)
6. <span data-ttu-id="59d14-259">Välj hello plats och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="59d14-259">Select hello location and then, click **OK**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclusion-location.png)
7. <span data-ttu-id="59d14-261">Lägga till filnamnstillägget hello i hello **filtyp** fältet.</span><span class="sxs-lookup"><span data-stu-id="59d14-261">Add hello file extension in hello **File Type** field.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclude-file-type.png)

    <span data-ttu-id="59d14-263">Lägger till en .mp3-tillägg</span><span class="sxs-lookup"><span data-stu-id="59d14-263">Adding an .mp3 extension</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclude-mp3.png)

    <span data-ttu-id="59d14-265">tooadd ett annat filtillägg klickar du på **Lägg till undantag** och ange en annan filtyp (lägga till filnamnstillägget .jpeg).</span><span class="sxs-lookup"><span data-stu-id="59d14-265">tooadd another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/exclude-jpg.png)
8. <span data-ttu-id="59d14-267">När du har lagt till alla hello-tillägg, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="59d14-267">When you've added all hello extensions, click **OK**.</span></span>
9. <span data-ttu-id="59d14-268">Fortsätter med hello schema för Säkerhetskopieringsguiden genom att klicka på **nästa** tills hello **bekräftelsesidan**, klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="59d14-268">Continue through hello Schedule Backup Wizard by clicking **Next** until hello **Confirmation page**, then click **Finish**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server/finish-exclusions.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="59d14-270">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="59d14-270">Frequently asked questions</span></span>
<span data-ttu-id="59d14-271">**F1. hello säkerhetskopieringsjobbet status visas som slutförda i hello Azure backup-agenten, varför inte den hämta visas omedelbart i portal?**</span><span class="sxs-lookup"><span data-stu-id="59d14-271">**Q1. hello backup job status shows as completed in hello Azure backup agent, why doesn't it get reflected immediately in portal?**</span></span>

<span data-ttu-id="59d14-272">S1.</span><span class="sxs-lookup"><span data-stu-id="59d14-272">A1.</span></span> <span data-ttu-id="59d14-273">Det visas på Maximal fördröjning på 15 minuter mellan hello säkerhetskopieringsjobbet status i hello Azure backup-agenten och hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="59d14-273">There is at maximum delay of 15 mins between hello backup job status reflected in hello Azure backup agent and hello Azure portal.</span></span>

<span data-ttu-id="59d14-274">**Q.2 när ett säkerhetskopieringsjobb misslyckas, hur lång tid tar det tooraise en avisering?**</span><span class="sxs-lookup"><span data-stu-id="59d14-274">**Q.2 When a backup job fails, how long does it take tooraise an alert?**</span></span>

<span data-ttu-id="59d14-275">A.2 en avisering utlöses inom 20 minuter för hello Azure säkerhetskopieringen har misslyckats.</span><span class="sxs-lookup"><span data-stu-id="59d14-275">A.2 An alert is raised within 20 mins of hello Azure backup failure.</span></span>

<span data-ttu-id="59d14-276">**KV. 3. Finns det fall där inte ett e-postmeddelande skickas om meddelanden har konfigurerats?**</span><span class="sxs-lookup"><span data-stu-id="59d14-276">**Q3. Is there a case where an email won’t be sent if notifications are configured?**</span></span>

<span data-ttu-id="59d14-277">S3.</span><span class="sxs-lookup"><span data-stu-id="59d14-277">A3.</span></span> <span data-ttu-id="59d14-278">Nedan visas hello fall när hello-meddelande inte skickas i ordning tooreduce hello avisering brus:</span><span class="sxs-lookup"><span data-stu-id="59d14-278">Below are hello cases when hello notification will not be sent in order tooreduce hello alert noise:</span></span>

* <span data-ttu-id="59d14-279">Om meddelanden konfigureras varje timme och en avisering aktiveras som lösts inom hello timme</span><span class="sxs-lookup"><span data-stu-id="59d14-279">If notifications are configured hourly and an alert is raised and resolved within hello hour</span></span>
* <span data-ttu-id="59d14-280">Jobbet har avbrutits.</span><span class="sxs-lookup"><span data-stu-id="59d14-280">Job is canceled.</span></span>
* <span data-ttu-id="59d14-281">Andra säkerhetskopieringsjobbet misslyckades eftersom ursprungliga säkerhetskopiering pågår.</span><span class="sxs-lookup"><span data-stu-id="59d14-281">Second backup job failed because original backup job is in progress.</span></span>

## <a name="troubleshooting-monitoring-issues"></a><span data-ttu-id="59d14-282">Felsökning av problem med övervakning</span><span class="sxs-lookup"><span data-stu-id="59d14-282">Troubleshooting Monitoring Issues</span></span>
<span data-ttu-id="59d14-283">**Problem:** jobb och/eller varningar från hello Azure Backup-agenten visas inte i hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="59d14-283">**Issue:** Jobs and/or alerts from hello Azure Backup agent do not appear in hello portal.</span></span>

<span data-ttu-id="59d14-284">**Felsökningssteg:** hello processen ```OBRecoveryServicesManagementAgent```, skickar hello jobbet och avisering data toohello Azure Backup-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="59d14-284">**Troubleshooting steps:** hello process, ```OBRecoveryServicesManagementAgent```, sends hello job and alert data toohello Azure Backup service.</span></span> <span data-ttu-id="59d14-285">Ibland den här processen kan fastna eller avstängning.</span><span class="sxs-lookup"><span data-stu-id="59d14-285">Occasionally this process can become stuck or shutdown.</span></span>

1. <span data-ttu-id="59d14-286">tooverify hello processen körs inte, öppna **Aktivitetshanteraren** och kontrollera om hello ```OBRecoveryServicesManagementAgent``` processen körs.</span><span class="sxs-lookup"><span data-stu-id="59d14-286">tooverify hello process is not running, open **Task Manager** and check if hello ```OBRecoveryServicesManagementAgent``` process is running.</span></span>
2. <span data-ttu-id="59d14-287">Förutsatt att hello processen inte körs, öppna **Kontrollpanelen** och bläddra hello lista över tjänster.</span><span class="sxs-lookup"><span data-stu-id="59d14-287">Assuming that hello process is not running, open **Control Panel** and browse hello list of services.</span></span> <span data-ttu-id="59d14-288">Starta eller starta om **Microsoft Azure Recovery Services-hanteringsagenten**.</span><span class="sxs-lookup"><span data-stu-id="59d14-288">Start or restart **Microsoft Azure Recovery Services Management Agent**.</span></span>

    <span data-ttu-id="59d14-289">Bläddra hello loggar vid ytterligare information:</span><span class="sxs-lookup"><span data-stu-id="59d14-289">For further information, browse hello logs at:</span></span><br/><span data-ttu-id="59d14-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*`Exempel:</span><span class="sxs-lookup"><span data-stu-id="59d14-290">
   `<AzureBackup_agent_install_folder>\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider*` For example:</span></span><br/>
   `C:\Program Files\Microsoft Azure Recovery Services Agent\Temp\GatewayProvider0.errlog`

## <a name="next-steps"></a><span data-ttu-id="59d14-291">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="59d14-291">Next steps</span></span>
* [<span data-ttu-id="59d14-292">Återställa Windows Server eller Windows-klienten från Azure</span><span class="sxs-lookup"><span data-stu-id="59d14-292">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="59d14-293">toolearn mer om Azure Backup finns [översikt över Azure-säkerhetskopiering](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="59d14-293">toolearn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="59d14-294">Besök hello [Azure Backup-forumet](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="59d14-294">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
