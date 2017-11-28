---
title: "Azure-säkerhetskopiering: Återställa systemtillståndet tooa Windows Server | Microsoft Docs"
description: "Steg för steg förklaring för återställning av systemtillståndet för Windows Server från en säkerhetskopia i Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: shivamg
editor: 
ms.assetid: 
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/18/2017
ms.author: saurse;trinadhk;markgal;
ms.openlocfilehash: a45506507f53e2744350d3b6b2e52f1db415de4f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="restore-system-state-toowindows-server"></a><span data-ttu-id="fb21a-103">Återställa Systemtillstånd tooWindows Server</span><span class="sxs-lookup"><span data-stu-id="fb21a-103">Restore System State tooWindows Server</span></span>

<span data-ttu-id="fb21a-104">Den här artikeln förklarar hur toorestore Windows Server systemtillståndet från en Azure Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="fb21a-104">This article explains how toorestore Windows Server System State backups from an Azure Recovery Services vault.</span></span> <span data-ttu-id="fb21a-105">toorestore systemtillstånd, måste du ha en säkerhetskopia av systemtillståndet (skapas med hello instruktionerna i [säkerhetskopiera systemtillståndet](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), och kontrollera att du har installerat hello [senaste versionen av hello Microsoft Azure Recovery Services (MARS)-agenten](http://aka.ms/azurebackup_agent).</span><span class="sxs-lookup"><span data-stu-id="fb21a-105">toorestore System State, you must have a System State backup (created using hello instructions in [Back up System State](backup-azure-system-state.md#back-up-windows-server-system-state-preview)), and make sure you have installed hello [latest version of hello Microsoft Azure Recovery Services (MARS) agent](http://aka.ms/azurebackup_agent).</span></span> <span data-ttu-id="fb21a-106">Återställa Systemtillstånd för Windows Server-data från ett Azure Recovery Services-valv är en tvåstegsprocess:</span><span class="sxs-lookup"><span data-stu-id="fb21a-106">Recovering Windows Server System State data from an Azure Recovery Services vault is a two-step process:</span></span>

1. <span data-ttu-id="fb21a-107">Återställa Systemtillstånd som filer från Azure Backup.</span><span class="sxs-lookup"><span data-stu-id="fb21a-107">Restore System State as files from Azure Backup.</span></span> <span data-ttu-id="fb21a-108">När du återställer systemtillståndet som filer från Azure Backup kan du antingen:</span><span class="sxs-lookup"><span data-stu-id="fb21a-108">When restoring System State as files from Azure Backup, you can either:</span></span>
  * <span data-ttu-id="fb21a-109">Återställa Systemtillstånd toohello samma server där hello säkerhetskopieringar som utförts, eller</span><span class="sxs-lookup"><span data-stu-id="fb21a-109">Restore System State toohello same server where hello backups were taken, or</span></span>
  * <span data-ttu-id="fb21a-110">Återställa Systemtillstånd tooan alternativ filserver.</span><span class="sxs-lookup"><span data-stu-id="fb21a-110">Restore System State file tooan alternate server.</span></span>

2. <span data-ttu-id="fb21a-111">Tillämpa hello återställa systemtillståndet filer tooa Windows Server.</span><span class="sxs-lookup"><span data-stu-id="fb21a-111">Apply hello restored System State files tooa Windows Server.</span></span>


## <a name="recover-system-state-files-toohello-same-server"></a><span data-ttu-id="fb21a-112">Återställer du systemtillståndet filer toohello samma server</span><span class="sxs-lookup"><span data-stu-id="fb21a-112">Recover System State files toohello same server</span></span>
<span data-ttu-id="fb21a-113">hello följande steg förklarar hur tooroll tillbaka din Windows Server configuration tooa tidigare tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb21a-113">hello following steps explain how tooroll back your Windows Server configuration tooa previous state.</span></span> <span data-ttu-id="fb21a-114">Kan vara bra att återställa servern configuration tillbaka tooa kända stabilt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb21a-114">Rolling your server configuration back tooa known, stable state, can be extremely valuable.</span></span> <span data-ttu-id="fb21a-115">hello följande steg för återställning hello serverns systemtillstånd från en Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="fb21a-115">hello following steps restore hello server's System State from a Recovery Services vault.</span></span> 

1. <span data-ttu-id="fb21a-116">Öppna hello **Microsoft Azure Backup** snapin-modulen.</span><span class="sxs-lookup"><span data-stu-id="fb21a-116">Open hello **Microsoft Azure Backup** snap-in.</span></span> <span data-ttu-id="fb21a-117">Om du inte vet där hello snapin-modulen installerades, söka hello dator eller server för **Microsoft Azure Backup**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-117">If you don't know where hello snap-in was installed, search hello computer or server for **Microsoft Azure Backup**.</span></span>

    <span data-ttu-id="fb21a-118">Hej skrivbordsapp ska visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="fb21a-118">hello desktop app should appear in hello search results.</span></span>

2. <span data-ttu-id="fb21a-119">Klicka på **återställa Data** toostart hello guiden.</span><span class="sxs-lookup"><span data-stu-id="fb21a-119">Click **Recover Data** toostart hello wizard.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server/recover.png)

3. <span data-ttu-id="fb21a-121">På hello **komma igång** rutan, toorestore hello data toohello samma server eller dator, Välj **servern (`<server name>`)** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-121">On hello **Getting Started** pane, toorestore hello data toohello same server or computer, select **This server (`<server name>`)** and click **Next**.</span></span>

    ![Välj den här servern alternativet toorestore hello data toohello samma dator](./media/backup-azure-restore-system-state/samemachine.png)

4. <span data-ttu-id="fb21a-123">På hello **Välj återställningsläge** fönstret Välj **systemtillstånd** och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-123">On hello **Select Recovery Mode** pane, choose **System State** and then click **Next**.</span></span>

    ![Bläddra bland filer](./media/backup-azure-restore-system-state/recover-type-selection.png)

5. <span data-ttu-id="fb21a-125">Hello kalendern i **Välj volym och datum** pekar du väljer en återställning.</span><span class="sxs-lookup"><span data-stu-id="fb21a-125">On hello calendar in **Select Volume and Date** pane, select a recovery point.</span></span> 

    <span data-ttu-id="fb21a-126">Du kan återställa från valfri återställningspunkt i tid.</span><span class="sxs-lookup"><span data-stu-id="fb21a-126">You can restore from any recovery point in time.</span></span> <span data-ttu-id="fb21a-127">Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="fb21a-127">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="fb21a-128">När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="fb21a-128">Once you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span>

    ![Volym och datum](./media/backup-azure-restore-system-state/select-date.png)

6. <span data-ttu-id="fb21a-130">När du har valt hello recovery punkt toorestore, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-130">Once you have chosen hello recovery point toorestore, click **Next**.</span></span>

    <span data-ttu-id="fb21a-131">Azure-säkerhetskopiering monterar hello lokal återställningspunkt och använder den som en återställningspunkt-volym.</span><span class="sxs-lookup"><span data-stu-id="fb21a-131">Azure Backup mounts hello local recovery point, and uses it as a recovery volume.</span></span>

7. <span data-ttu-id="fb21a-132">Ange hello mål för hello återställda filer för systemtillstånd på hello nästa ruta, och klicka på **Bläddra** tooopen Windows Explorer och hitta hello filer och mappar som du vill.</span><span class="sxs-lookup"><span data-stu-id="fb21a-132">On hello next pane, specify hello destination for hello recovered System State files and click **Browse** tooopen Windows Explorer and find hello files and folders you want.</span></span> <span data-ttu-id="fb21a-133">Hej alternativet **skapa kopior så att du har båda versionerna**, skapar kopior av enskilda filer i en befintlig systemtillstånd filarkivet istället för att skapa hello kopia av hello hela systemtillstånd Arkiv.</span><span class="sxs-lookup"><span data-stu-id="fb21a-133">hello option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating hello copy of hello entire System State archive.</span></span>

    ![Återställningsalternativ](./media/backup-azure-restore-system-state/recover-as-files.png)

8. <span data-ttu-id="fb21a-135">Kontrollera hello information om återställning på hello **bekräftelse** och klickar på **återställa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-135">Verify hello details of recovery on hello **Confirmation** pane and click **Recover**.</span></span>

   ![Klicka på Återställ tooacknowledge hello Återställ åtgärd](./media/backup-azure-restore-system-state/confirm-recovery.png)

9. <span data-ttu-id="fb21a-137">Kopiera hello *WindowsImageBackup* katalogen i hello Recovery tooa icke-kritiska målvolymen hello-Server.</span><span class="sxs-lookup"><span data-stu-id="fb21a-137">Copy hello *WindowsImageBackup* directory in hello Recovery destination tooa non-critical volume of hello server.</span></span> <span data-ttu-id="fb21a-138">Vanligtvis är hello systemvolymen Windows hello kritisk volym.</span><span class="sxs-lookup"><span data-stu-id="fb21a-138">Usually, hello Windows OS volume is hello critical volume.</span></span>

10. <span data-ttu-id="fb21a-139">När hello återställningen är klar gör hello hello under [tillämpa återställa systemtillståndet filer toohello Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello systemtillstånd återställningsprocessen.</span><span class="sxs-lookup"><span data-stu-id="fb21a-139">Once hello recovery is successful, follow hello steps in hello section, [Apply restored System State files toohello Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-files-to-the-windows-server), toocomplete hello System State recovery process.</span></span>

## <a name="recover-system-state-files-tooan-alternate-server"></a><span data-ttu-id="fb21a-140">Återställer du systemtillståndet filer tooan alternativ server</span><span class="sxs-lookup"><span data-stu-id="fb21a-140">Recover System State files tooan alternate server</span></span>

<span data-ttu-id="fb21a-141">Om din Windows-Server är skadad eller inte tillgänglig och du vill toorestore den tooa stabilt läge genom att återställa Hej systemtillstånd för Windows Server, kan du återställa hello skadat serverns systemtillstånd från en annan server.</span><span class="sxs-lookup"><span data-stu-id="fb21a-141">If your Windows Server is corrupted or inaccessible, and you want toorestore it tooa stable state by recovering hello Windows Server System State, you can restore hello corrupted server's System State from another server.</span></span> <span data-ttu-id="fb21a-142">Använd hello följa steg toohello återställa systemtillståndet på en separat server.</span><span class="sxs-lookup"><span data-stu-id="fb21a-142">Use hello following steps toohello restore System State on a separate server.</span></span>  

<span data-ttu-id="fb21a-143">hello-terminologi som används i de här stegen innefattar:</span><span class="sxs-lookup"><span data-stu-id="fb21a-143">hello terminology used in these steps includes:</span></span>

- <span data-ttu-id="fb21a-144">*Källdatorn* – hello ursprungliga datorn från vilken hello säkerhetskopian skapades och som inte är tillgänglig för tillfället.</span><span class="sxs-lookup"><span data-stu-id="fb21a-144">*Source machine* – hello original machine from which hello backup was taken and which is currently unavailable.</span></span>
- <span data-ttu-id="fb21a-145">*Måldatorn* – hello datorn toowhich hello data återställs.</span><span class="sxs-lookup"><span data-stu-id="fb21a-145">*Target machine* – hello machine toowhich hello data is being recovered.</span></span>
- <span data-ttu-id="fb21a-146">*Exempel valvet* – hello Recovery Services-valvet toowhich hello *källdatorn* och *måldatorn* registreras.</span><span class="sxs-lookup"><span data-stu-id="fb21a-146">*Sample vault* – hello Recovery Services vault toowhich hello *Source machine* and *Target machine* are registered.</span></span> <br/>

> [!NOTE]
> <span data-ttu-id="fb21a-147">Säkerhetskopieringar som utförts från en dator får inte vara återställda tooa dator som kör en tidigare version av operativsystemet för hello.</span><span class="sxs-lookup"><span data-stu-id="fb21a-147">Backups taken from one machine cannot be restored tooa machine running an earlier version of hello operating system.</span></span> <span data-ttu-id="fb21a-148">Till exempel återställa säkerhetskopior som hämtas från en Windows Server 2016-datorn inte kan vara tooWindows Server 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="fb21a-148">For example, backups taken from a Windows Server 2016 machine can't be restored tooWindows Server 2012 R2.</span></span> <span data-ttu-id="fb21a-149">Hello inversen är dock möjligt.</span><span class="sxs-lookup"><span data-stu-id="fb21a-149">However, hello inverse is possible.</span></span> <span data-ttu-id="fb21a-150">Du kan använda säkerhetskopior från Windows Server 2012 R2 toorestore Windows Server 2016.</span><span class="sxs-lookup"><span data-stu-id="fb21a-150">You can use backups from Windows Server 2012 R2 toorestore Windows Server 2016.</span></span>
>

1. <span data-ttu-id="fb21a-151">Öppna hello **Microsoft Azure Backup** snapin-modulen på hello *måldatorn*.</span><span class="sxs-lookup"><span data-stu-id="fb21a-151">Open hello **Microsoft Azure Backup** snap-in on hello *Target machine*.</span></span>
2. <span data-ttu-id="fb21a-152">Se till att hello *måldatorn* och hello *källdatorn* är registrerade toohello samma Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="fb21a-152">Ensure that hello *Target machine* and hello *Source machine* are registered toohello same Recovery Services vault.</span></span>
3. <span data-ttu-id="fb21a-153">Klicka på **återställa Data** tooinitiate hello arbetsflöde.</span><span class="sxs-lookup"><span data-stu-id="fb21a-153">Click **Recover Data** tooinitiate hello workflow.</span></span>

    ![Återställa Data](./media/backup-azure-restore-windows-server-classic/recover.png)

4. <span data-ttu-id="fb21a-155">Välj **en annan server**</span><span class="sxs-lookup"><span data-stu-id="fb21a-155">Select **Another server**</span></span>

    ![En annan Server](./media/backup-azure-restore-system-state/anotherserver.png)

5. <span data-ttu-id="fb21a-157">Ange hello valvautentiseringsfilen som motsvarar toohello *exempel valvet*.</span><span class="sxs-lookup"><span data-stu-id="fb21a-157">Provide hello vault credential file that corresponds toohello *Sample vault*.</span></span> <span data-ttu-id="fb21a-158">Om hello valvautentiseringsfilen är ogiltig (eller har upphört att gälla), ladda ned en ny valvautentiseringsfil från hello *exempel valvet* i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="fb21a-158">If hello vault credential file is invalid (or expired), download a new vault credential file from hello *Sample vault* in hello Azure portal.</span></span> <span data-ttu-id="fb21a-159">Hello Recovery Services-valvet som är associerade med hello valvautentiseringsfilen visas när hello valvautentiseringsfilen har angetts.</span><span class="sxs-lookup"><span data-stu-id="fb21a-159">Once hello vault credential file is provided, hello Recovery Services vault associated with hello vault credential file appears.</span></span>

6. <span data-ttu-id="fb21a-160">I fönstret Välj Backup Server hello Välj hello *källdatorn* hello listan med visade datorer.</span><span class="sxs-lookup"><span data-stu-id="fb21a-160">On hello Select Backup Server pane, select hello *Source machine* from hello list of displayed machines.</span></span>

    ![Lista över datorer](./media/backup-azure-restore-windows-server-classic/machinelist.png)

7. <span data-ttu-id="fb21a-162">I fönstret Välj återställningsläge hello väljer **systemtillstånd** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-162">On hello Select Recovery Mode pane, choose **System State** and click **Next**.</span></span> 

    ![Söka](./media/backup-azure-restore-system-state/recover-type-selection.png)

8. <span data-ttu-id="fb21a-164">På hello kalendern i hello **Välj volym och datum** pekar du väljer en återställning.</span><span class="sxs-lookup"><span data-stu-id="fb21a-164">On hello Calendar in hello **Select Volume and Date** pane, select a recovery point.</span></span> <span data-ttu-id="fb21a-165">Du kan återställa från valfri återställningspunkt i tid.</span><span class="sxs-lookup"><span data-stu-id="fb21a-165">You can restore from any recovery point in time.</span></span> <span data-ttu-id="fb21a-166">Datum i **fetstil** ange hello tillgängligheten för minst en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="fb21a-166">Dates in **bold** indicate hello availability of at least one recovery point.</span></span> <span data-ttu-id="fb21a-167">När du har valt ett datum, om det finns flera återställningspunkter, Välj hello specifik återställningspunkt hello **tid** nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="fb21a-167">Once  you select a date, if multiple recovery points are available, choose hello specific recovery point from hello **Time** drop-down menu.</span></span> 

    ![Sökobjekt](./media/backup-azure-restore-system-state/select-date.png)

9. <span data-ttu-id="fb21a-169">När du har valt hello recovery punkt toorestore, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-169">Once you have chosen hello recovery point toorestore, click **Next**.</span></span>

10. <span data-ttu-id="fb21a-170">På hello **Välj återställningsläge för systemet tillstånd** rutan Ange hello mål där du vill att systemtillståndet filer toobe återställas och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-170">On hello **Select System State Recovery Mode** pane, specify hello destination where you want System State files toobe recovered, then click **Next**.</span></span>

    ![Kryptering](./media/backup-azure-restore-system-state/recover-as-files.png)

    <span data-ttu-id="fb21a-172">Hej alternativet **skapa kopior så att du har båda versionerna**, skapar kopior av enskilda filer i en befintlig systemtillstånd filarkivet istället för att skapa hello kopia av hello hela systemtillstånd Arkiv.</span><span class="sxs-lookup"><span data-stu-id="fb21a-172">hello option, **Create copies so that you have both versions**, creates copies of individual files in an existing System State file archive instead of creating hello copy of hello entire System State archive.</span></span>

11. <span data-ttu-id="fb21a-173">Kontrollera informationen om hello av återställningspunkter i hello bekräftelse rutan och klicka på **återställa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-173">Verify hello details of recovery on hello Confirmation pane, and click **Recover**.</span></span> 

    ![Klicka på hello Återställ knappen tooconfirm hello återställningsprocessen](./media/backup-azure-restore-system-state/confirm-recovery.png)

12. <span data-ttu-id="fb21a-175">Kopiera hello *WindowsImageBackup* directory tooa icke-kritisk volym hello Server (till exempel D:\).</span><span class="sxs-lookup"><span data-stu-id="fb21a-175">Copy hello *WindowsImageBackup* directory tooa non-critical volume of hello server (for example D:\).</span></span> <span data-ttu-id="fb21a-176">Vanligtvis är hello systemvolymen Windows hello kritisk volym.</span><span class="sxs-lookup"><span data-stu-id="fb21a-176">Usually hello Windows OS volume is hello critical volume.</span></span>

13. <span data-ttu-id="fb21a-177">toocomplete hello återställningen Använd hello följande avsnitt för[gäller hello återställa systemtillståndet filer på en Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span><span class="sxs-lookup"><span data-stu-id="fb21a-177">toocomplete hello recovery process, use hello following section too[apply hello restored System State files on a Windows Server](backup-azure-restore-system-state.md#apply-restored-system-state-on-a-windows-server).</span></span>




## <a name="apply-restored-system-state-on-a-windows-server"></a><span data-ttu-id="fb21a-178">Tillämpa återställda systemtillståndet på en Windows Server</span><span class="sxs-lookup"><span data-stu-id="fb21a-178">Apply restored System State on a Windows Server</span></span>

<span data-ttu-id="fb21a-179">När du har återställt systemtillstånd som filer med Azure Recovery Services-agenten, återställa Använd hello Windows Server Backup-verktyget tooapply hello systemtillståndet tooWindows Server.</span><span class="sxs-lookup"><span data-stu-id="fb21a-179">Once you have recovered System State as files using Azure Recovery Services Agent, use hello Windows Server Backup utility tooapply hello recovered System State tooWindows Server.</span></span> <span data-ttu-id="fb21a-180">hello verktyg för Windows Server Backup finns redan på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="fb21a-180">hello Windows Server Backup utility is already available on hello server.</span></span> <span data-ttu-id="fb21a-181">hello följande steg förklarar hur tooapply hello återställa Systemtillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb21a-181">hello following steps explain how tooapply hello recovered System State.</span></span>

1. <span data-ttu-id="fb21a-182">Använd hello följande kommandon tooreboot servern i *reparationsläge för katalogtjänster*.</span><span class="sxs-lookup"><span data-stu-id="fb21a-182">Use hello following commands tooreboot your server in *Directory Services Repair Mode*.</span></span> <span data-ttu-id="fb21a-183">I en upphöjd kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="fb21a-183">In an elevated command prompt:</span></span>

    ```
    PS C:\> Bcdedit /set safeboot dsrepair
    PS C:\> Shutdown /r /t 0
    ```

2. <span data-ttu-id="fb21a-184">Efter omstart hello öppna hello Windows Server Backup snapin-modulen.</span><span class="sxs-lookup"><span data-stu-id="fb21a-184">After hello reboot, open hello Windows Server Backup snap-in.</span></span> <span data-ttu-id="fb21a-185">Om du inte vet där hello snapin-modulen installerades, söka hello dator eller server för **Windows Server Backup**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-185">If you don't know where hello snap-in was installed, search hello computer or server for **Windows Server Backup**.</span></span>

    <span data-ttu-id="fb21a-186">Hej skrivbordsapp visas i sökresultaten hello.</span><span class="sxs-lookup"><span data-stu-id="fb21a-186">hello desktop app appears in hello search results.</span></span>

3. <span data-ttu-id="fb21a-187">Hello snapin-modulen, Välj **lokal säkerhetskopia**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-187">In hello snap-in, select **Local Backup**.</span></span>

    ![Välj lokal säkerhetskopia toorestore därifrån](./media/backup-azure-restore-system-state/win-server-backup-local-backup.png)

4. <span data-ttu-id="fb21a-189">På hello lokal säkerhetskopia i konsolen hello **åtgärdsfönstret**, klickar du på **återställa** tooopen hello Återställningsguiden.</span><span class="sxs-lookup"><span data-stu-id="fb21a-189">On hello Local Backup console, in hello **Actions Pane**, click **Recover** tooopen hello Recovery Wizard.</span></span>

5. <span data-ttu-id="fb21a-190">Välj alternativet för hello **en säkerhetskopia lagrad på en annan plats**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-190">Select hello option, **A backup stored in another location**, and click **Next**.</span></span>

   ![Välj toorecover tooa annan server](./media/backup-azure-restore-system-state/backup-stored-in-diff-location.png)

6. <span data-ttu-id="fb21a-192">När du anger hello platstyp, Välj **Remote delade mappen** om din säkerhetskopia av systemtillståndet återställda tooanother server.</span><span class="sxs-lookup"><span data-stu-id="fb21a-192">When specifying hello location type, select **Remote shared folder** if your System State backup was recovered tooanother server.</span></span> <span data-ttu-id="fb21a-193">Om systemets tillstånd återställts lokalt, välj sedan **lokala enheter**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-193">If your System State was recovered locally, then select **Local drives**.</span></span> 

    ![Välj om toorecovery från lokal server eller en annan](./media/backup-azure-restore-system-state/ss-recovery-remote-shared-folder.png)

7. <span data-ttu-id="fb21a-195">Ange hello sökvägen toohello *WindowsImageBackup* directory, eller välj hello lokala enheten som innehåller den här katalogen (till exempel D:\WindowsImageBackup) återställas i samband med hello systemtillstånd filer återställning med hjälp av Azure Recovery Agent och klicka på Services **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-195">Enter hello path toohello *WindowsImageBackup* directory, or choose hello local drive containing this directory (for example, D:\WindowsImageBackup), recovered as part of hello System State files recovery using Azure Recovery Services Agent and click **Next**.</span></span>

    ![sökvägen toohello delad fil](./media/backup-azure-restore-system-state/ss-recovery-remote-folder.png)

8. <span data-ttu-id="fb21a-197">Välj hello systemtillstånd version som du vill toorestore och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-197">Select hello System State version that you want toorestore, and click **Next**.</span></span>

9. <span data-ttu-id="fb21a-198">Välj i rutan Välj typ av återställning hello **systemtillstånd** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-198">In hello Select Recovery Type pane, select **System State** and click **Next**.</span></span>

10. <span data-ttu-id="fb21a-199">Hello platsen för hello återställning av systemtillstånd, Välj **ursprungsplatsen**, och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="fb21a-199">For hello location of hello System State Recovery, select **Original Location**, and click **Next**.</span></span>

11. <span data-ttu-id="fb21a-200">Granska hello-händelsebekräftelse-information, verifiera hello omstart inställningarna och klicka på **återställa** tooapplly hello återställa systemtillståndet filer.</span><span class="sxs-lookup"><span data-stu-id="fb21a-200">Review hello confirmation details, verify hello reboot settings, and click **Recover** tooapplly hello restored System State files.</span></span>

    ![Starta hello återställa systemtillståndet filer](./media/backup-azure-restore-system-state/launch-ss-recovery.png)

## <a name="special-considerations-for-system-state-recovery-on-active-directory-server"></a><span data-ttu-id="fb21a-202">Speciella överväganden för återställning av systemtillstånd på Active Directory-server</span><span class="sxs-lookup"><span data-stu-id="fb21a-202">Special considerations for System State recovery on Active Directory server</span></span>

<span data-ttu-id="fb21a-203">Säkerhetskopian av systemtillstånd innehåller Active Directory-data.</span><span class="sxs-lookup"><span data-stu-id="fb21a-203">System State backup includes Active Directory data.</span></span> <span data-ttu-id="fb21a-204">Använd hello följande steg toorestore Active Directory Domain Service (AD DS) från dess aktuella tillstånd tooa tidigare tillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb21a-204">Use hello following steps toorestore Active Directory Domain Service (AD DS) from its current state tooa previous state.</span></span>

1. <span data-ttu-id="fb21a-205">Starta om hello-domänkontrollanten i Directory återställningsläge för katalogtjänster (DSRM).</span><span class="sxs-lookup"><span data-stu-id="fb21a-205">Restart hello domain controller in Directory Services Restore Mode (DSRM).</span></span>
2. <span data-ttu-id="fb21a-206">Gör hello [här](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse Windows Server Backup-cmdletar toorecover AD DS.</span><span class="sxs-lookup"><span data-stu-id="fb21a-206">Follow hello steps [here](https://technet.microsoft.com/en-us/library/cc794755(v=ws.10).aspx) toouse Windows Server Backup cmdlets toorecover AD DS.</span></span>


## <a name="troubleshoot-failed-system-state-restore"></a><span data-ttu-id="fb21a-207">Felsökning av misslyckade återställning av systemtillståndet</span><span class="sxs-lookup"><span data-stu-id="fb21a-207">Troubleshoot failed System State restore</span></span>

<span data-ttu-id="fb21a-208">Om hello föregående process för tillämpning av systemtillståndet inte slutförs korrekt, använder du hello Windows Recovery Environment (Win RE) toorecover Windows Server.</span><span class="sxs-lookup"><span data-stu-id="fb21a-208">If hello previous process of applying System State does not complete successfully, use hello Windows Recovery Environment (Win RE) toorecover your Windows Server.</span></span> <span data-ttu-id="fb21a-209">hello följande steg förklarar hur toorecover med Win RE.</span><span class="sxs-lookup"><span data-stu-id="fb21a-209">hello following steps explain how toorecover using Win RE.</span></span> <span data-ttu-id="fb21a-210">Använd det här alternativet endast om Windows Server inte startar normalt efter en återställning av systemtillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb21a-210">Use This option only if Windows Server does not boot normally after a System State restore.</span></span> <span data-ttu-id="fb21a-211">följande process hello raderar-system data, var försiktig.</span><span class="sxs-lookup"><span data-stu-id="fb21a-211">hello following process erases non-system data, use caution.</span></span> 

1. <span data-ttu-id="fb21a-212">Starta Windows-Server i hello Windows Recovery Environment (Win RE).</span><span class="sxs-lookup"><span data-stu-id="fb21a-212">Boot your Windows Server into hello Windows Recovery Environment (Win RE).</span></span>

2. <span data-ttu-id="fb21a-213">Välj Felsök hello tre alternativ.</span><span class="sxs-lookup"><span data-stu-id="fb21a-213">Select Troubleshoot from hello three available options.</span></span>

    ![Öppna menyn](./media/backup-azure-restore-system-state/winre-1.png)

3. <span data-ttu-id="fb21a-215">Från hello **avancerade alternativ** väljer **kommandotolk** och ange hello server administratörsanvändarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="fb21a-215">From hello **Advanced Options** screen, select **Command Prompt** and provide hello server administrator username and password.</span></span>

   ![Öppna menyn](./media/backup-azure-restore-system-state/winre-2.png)

4. <span data-ttu-id="fb21a-217">Ange hello server administratörsanvändarnamn och lösenord.</span><span class="sxs-lookup"><span data-stu-id="fb21a-217">Provide hello server administrator username and password.</span></span>

    ![Öppna menyn](./media/backup-azure-restore-system-state/winre-3.png)

5. <span data-ttu-id="fb21a-219">När du öppnar hello kommandotolk i administratörsläge, kör du följande kommando tooget hello systemtillstånd säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="fb21a-219">When you open hello command prompt in administrator mode, run following command tooget hello System State backup versions.</span></span>

    ```
    Wbadmin get versions -backuptarget:<Volume where WindowsImageBackup folder is copied>:
    ```
    ![Hämta systemtillstånd säkerhetskopior](./media/backup-azure-restore-system-state/winre-4.png)

6. <span data-ttu-id="fb21a-221">Kör följande kommando tooget hello alla volymer som är tillgängliga i hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="fb21a-221">Run hello following command tooget all volumes available in hello backup.</span></span>

    ```
    Wbadmin get items -version:<copy version from above step> -backuptarget:<Backup volume>
    ```

    ![Hämta systemtillstånd säkerhetskopior](./media/backup-azure-restore-system-state/winre-5.png)

7. <span data-ttu-id="fb21a-223">hello återställer följande kommando alla volymer som ingår i hello säkerhetskopia av systemtillståndet.</span><span class="sxs-lookup"><span data-stu-id="fb21a-223">hello following command recovers all volumes that are part of hello System State Backup.</span></span> <span data-ttu-id="fb21a-224">Observera att det här steget återställer endast kritiska volymer för hello som ingår i hello systemtillstånd.</span><span class="sxs-lookup"><span data-stu-id="fb21a-224">Note that this step recovers only hello critical volumes that are part of hello System State.</span></span> <span data-ttu-id="fb21a-225">Alla andra data raderas.</span><span class="sxs-lookup"><span data-stu-id="fb21a-225">All non-System data is erased.</span></span>

    ```
    Wbadmin start recovery -items:C: -itemtype:Volume -version:<Backupversion> -backuptarget:<backup target volume>
    ```
     ![Hämta systemtillstånd säkerhetskopior](./media/backup-azure-restore-system-state/winre-6.png)



## <a name="next-steps"></a><span data-ttu-id="fb21a-227">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fb21a-227">Next steps</span></span>
* <span data-ttu-id="fb21a-228">Nu när du har återställt dina filer och mappar, kan du [hantera dina säkerhetskopieringar](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="fb21a-228">Now that you've recovered your files and folders, you can [manage your backups](backup-azure-manage-windows-server.md).</span></span>
