---
title: aaaBackup Azure virtuella Windows-datorer | Microsoft Docs
description: "Skydda Windows-datorer genom att säkerhetskopiera dem med Azure Backup."
services: virtual-machines-windows
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 1cd3e1940a83aacd160cba3c8613b63b6f3c11d9
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a><span data-ttu-id="d84bc-103">Säkerhetskopiera Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="d84bc-103">Back up Windows virtual machines in Azure</span></span>

<span data-ttu-id="d84bc-104">Du kan skydda dina data genom att säkerhetskopiera med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="d84bc-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="d84bc-105">Azure-säkerhetskopiering skapar återställningspunkter som är lagrade i geo-redundant recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="d84bc-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="d84bc-106">När du återställer från en återställningspunkt kan du återställa hello hela VM eller bara vissa filer.</span><span class="sxs-lookup"><span data-stu-id="d84bc-106">When you restore from a recovery point, you can restore hello whole VM or just specific files.</span></span> <span data-ttu-id="d84bc-107">Den här artikeln förklarar hur toorestore en enda fil tooa VM kör Windows Server och IIS.</span><span class="sxs-lookup"><span data-stu-id="d84bc-107">This article explains how toorestore a single file tooa VM running Windows Server and IIS.</span></span> <span data-ttu-id="d84bc-108">Om du inte redan har en VM-toouse kan du skapa en med hjälp av hello [Windows quickstart](quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="d84bc-108">If you don't already have a VM toouse, you can create one using hello [Windows quickstart](quick-create-portal.md).</span></span> <span data-ttu-id="d84bc-109">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="d84bc-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d84bc-110">Skapa en säkerhetskopia av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d84bc-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="d84bc-111">Schemalägga en daglig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="d84bc-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="d84bc-112">Återställa en fil från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="d84bc-112">Restore a file from a backup</span></span>




## <a name="backup-overview"></a><span data-ttu-id="d84bc-113">Översikt över Backup</span><span class="sxs-lookup"><span data-stu-id="d84bc-113">Backup overview</span></span>

<span data-ttu-id="d84bc-114">När hello Azure Backup-tjänsten startar ett säkerhetskopieringsjobb, utlöser hello reservanknytning tootake en tidpunkt i ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="d84bc-114">When hello Azure Backup service initiates a backup job, it triggers hello backup extension tootake a point-in-time snapshot.</span></span> <span data-ttu-id="d84bc-115">hello Azure Backup-tjänsten använder hello _VMSnapshot_ tillägg.</span><span class="sxs-lookup"><span data-stu-id="d84bc-115">hello Azure Backup service uses hello _VMSnapshot_ extension.</span></span> <span data-ttu-id="d84bc-116">hello tillägget installeras under hello första säkerhetskopiering om hello VM körs.</span><span class="sxs-lookup"><span data-stu-id="d84bc-116">hello extension is installed during hello first VM backup if hello VM is running.</span></span> <span data-ttu-id="d84bc-117">Om hello inte körs, tar en ögonblicksbild av hello underliggande lagring (eftersom inga program skrivningar sker när hello VM stoppas) hello Backup-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d84bc-117">If hello VM is not running, hello Backup service takes a snapshot of hello underlying storage (since no application writes occur while hello VM is stopped).</span></span>

<span data-ttu-id="d84bc-118">När en ögonblicksbild av virtuella Windows-datorer, samordnar hello Backup-tjänsten med hello Volume Shadow Copy Service (VSS) tooget en programkonsekvent ögonblicksbild av hello virtuella datorns diskar.</span><span class="sxs-lookup"><span data-stu-id="d84bc-118">When taking a snapshot of Windows VMs, hello Backup service coordinates with hello Volume Shadow Copy Service (VSS) tooget a consistent snapshot of hello virtual machine's disks.</span></span> <span data-ttu-id="d84bc-119">När hello Azure Backup-tjänsten tar hello ögonblicksbild, är hello data överförda toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="d84bc-119">Once hello Azure Backup service takes hello snapshot, hello data is transferred toohello vault.</span></span> <span data-ttu-id="d84bc-120">toomaximize effektivitet hello-tjänsten identifierar och överför bara hello datablock som har ändrats sedan tidigare hello-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="d84bc-120">toomaximize efficiency, hello service identifies and transfers only hello blocks of data that have changed since hello previous backup.</span></span>

<span data-ttu-id="d84bc-121">När hello dataöverföring är klar hello ögonblicksbild tas bort och skapa en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="d84bc-121">When hello data transfer is complete, hello snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="d84bc-122">Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="d84bc-122">Create a backup</span></span>
<span data-ttu-id="d84bc-123">Skapa en enkel schemalagd daglig säkerhetskopiering tooa Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="d84bc-123">Create a simple scheduled daily backup tooa Recovery Services Vault.</span></span> 

1. <span data-ttu-id="d84bc-124">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="d84bc-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="d84bc-125">Välj hello menyn hello vänster **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-125">In hello menu on hello left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="d84bc-126">Välj en VM-tooback in hello listan.</span><span class="sxs-lookup"><span data-stu-id="d84bc-126">From hello list, select a VM tooback up.</span></span>
4. <span data-ttu-id="d84bc-127">Hello VM bladet i hello **inställningar** klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-127">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="d84bc-128">Hej **Aktivera säkerhetskopiering** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d84bc-128">hello **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="d84bc-129">I **Recovery Services-valvet**, klickar du på **Skapa nytt** och ange hello namn för hello nytt valv.</span><span class="sxs-lookup"><span data-stu-id="d84bc-129">In **Recovery Services vault**, click **Create new** and provide hello name for hello new vault.</span></span> <span data-ttu-id="d84bc-130">Ett nytt valv skapas i hello samma resursgrupp och plats som hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d84bc-130">A new vault is created in hello same Resource Group and location as hello virtual machine.</span></span>
6. <span data-ttu-id="d84bc-131">Klicka på **säkerhetskopiera princip**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-131">Click **Backup policy**.</span></span> <span data-ttu-id="d84bc-132">I det här exemplet hålla hello standardvärden och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-132">For this example, keep hello defaults and click **OK**.</span></span>
7. <span data-ttu-id="d84bc-133">På hello **Aktivera säkerhetskopiering** bladet, klickar du på **Aktivera säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-133">On hello **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="d84bc-134">Detta skapar en daglig säkerhetskopiering baserat på hello standardschemat.</span><span class="sxs-lookup"><span data-stu-id="d84bc-134">This creates a daily backup based on hello default schedule.</span></span>
10. <span data-ttu-id="d84bc-135">toocreate en första återställningspunkten på hello **säkerhetskopiering** bladet och klickar på **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-135">toocreate an initial recovery point, on hello **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="d84bc-136">På hello **säkerhetskopiering nu** bladet Klicka hello kalendern använder hello kalender kontrollen tooselect hello sista dagen återställningspunkten behålls Klicka på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-136">On hello **Backup Now** blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="d84bc-137">I hello **säkerhetskopiering** bladet för den virtuella datorn visas hello antal återställningspunkter som är klar.</span><span class="sxs-lookup"><span data-stu-id="d84bc-137">In hello **Backup** blade for your VM, you will see hello number of recovery points that are complete.</span></span>

    ![Återställningspunkter](./media/tutorial-backup-vms/backup-complete.png)
    
<span data-ttu-id="d84bc-139">hello första säkerhetskopieringen tar ungefär 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="d84bc-139">hello first backup takes about 20 minutes.</span></span> <span data-ttu-id="d84bc-140">När säkerhetskopieringen är klar går du vidare toohello nästa del av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="d84bc-140">Proceed toohello next part of this tutorial after your backup is finished.</span></span>

## <a name="recover-a-file"></a><span data-ttu-id="d84bc-141">Återställa en fil</span><span class="sxs-lookup"><span data-stu-id="d84bc-141">Recover a file</span></span>

<span data-ttu-id="d84bc-142">Om du tar bort av misstag eller göra ändringar tooa fil, kan du använda filåterställning toorecover hello-fil från din backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="d84bc-142">If you accidentally delete or make changes tooa file, you can use File Recovery toorecover hello file from your backup vault.</span></span> <span data-ttu-id="d84bc-143">Filåterställning använder ett skript som körs på hello VM, toomount hello återställningspunkt som lokal enhet.</span><span class="sxs-lookup"><span data-stu-id="d84bc-143">File Recovery uses a script that runs on hello VM, toomount hello recovery point as local drive.</span></span> <span data-ttu-id="d84bc-144">Dessa enheter förblir monterade 12 timmar så att du kan kopiera filer från hello återställningspunkt och återställa dem toohello VM.</span><span class="sxs-lookup"><span data-stu-id="d84bc-144">These drives will remain mounted for 12 hours so that you can copy files from hello recovery point and restore them toohello VM.</span></span>  

<span data-ttu-id="d84bc-145">I det här exemplet visar vi hur toorecover hello image-filen som används i hello standardwebbsidan för IIS.</span><span class="sxs-lookup"><span data-stu-id="d84bc-145">In this example, we show how toorecover hello image file that is used in hello default web page for IIS.</span></span> 

1. <span data-ttu-id="d84bc-146">Öppna en webbläsare och Anslut toohello IP-adressen för hello VM tooshow hello IIS standardsida.</span><span class="sxs-lookup"><span data-stu-id="d84bc-146">Open a browser and connect toohello IP address of hello VM tooshow hello default IIS page.</span></span>

    ![Standardwebbsidan för IIS](./media/tutorial-backup-vms/iis-working.png)

2. <span data-ttu-id="d84bc-148">Ansluta toohello VM.</span><span class="sxs-lookup"><span data-stu-id="d84bc-148">Connect toohello VM.</span></span>
3. <span data-ttu-id="d84bc-149">Öppna på hello VM, **Utforskaren** och navigera too\inetpub\wwwroot och ta bort filen hello **iisstart.png**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-149">On hello VM, open **File Explorer** and navigate too\inetpub\wwwroot and delete hello file **iisstart.png**.</span></span>
4. <span data-ttu-id="d84bc-150">Uppdatera hello webbläsare toosee som hello bilden på hello standardsida för IIS är borta på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="d84bc-150">On your local computer, refresh hello browser toosee that hello image on hello default IIS page is gone.</span></span>

    ![Standardwebbsidan för IIS](./media/tutorial-backup-vms/iis-broken.png)

5. <span data-ttu-id="d84bc-152">På den lokala datorn, öppna en ny flik och gå hello hello [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="d84bc-152">On your local computer, open a new tab and go hello hello [Azure portal](https://portal.azure.com).</span></span>
6. <span data-ttu-id="d84bc-153">Välj hello menyn hello vänster **virtuella datorer** och välj hello VM formuläret hello lista.</span><span class="sxs-lookup"><span data-stu-id="d84bc-153">In hello menu on hello left, select **Virtual machines** and select hello VM form hello list.</span></span>
8. <span data-ttu-id="d84bc-154">Hello VM bladet i hello **inställningar** klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-154">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="d84bc-155">Hej **säkerhetskopiering** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d84bc-155">hello **Backup** blade opens.</span></span> 
9. <span data-ttu-id="d84bc-156">Välj hello menyn hello överst på bladet hello **filåterställning**.</span><span class="sxs-lookup"><span data-stu-id="d84bc-156">In hello menu at hello top of hello blade, select **File Recovery**.</span></span> <span data-ttu-id="d84bc-157">Hej **filåterställning** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="d84bc-157">hello **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="d84bc-158">I **steg 1: Välj återställningspunkt**, Välj en återställningspunkt hello i listrutan.</span><span class="sxs-lookup"><span data-stu-id="d84bc-158">In **Step 1: Select recovery point**, select a recovery point from hello drop-down.</span></span>
11. <span data-ttu-id="d84bc-159">I **steg 2: hämta skriptet toobrowse och återställa filerna**, klicka på hello **hämta körbara** knappen.</span><span class="sxs-lookup"><span data-stu-id="d84bc-159">In **Step 2: Download script toobrowse and recover files**, click hello **Download Executable** button.</span></span> <span data-ttu-id="d84bc-160">Spara hello filen tooyour **hämtar** mapp.</span><span class="sxs-lookup"><span data-stu-id="d84bc-160">Save hello file tooyour **Downloads** folder.</span></span>
12. <span data-ttu-id="d84bc-161">Öppna på den lokala datorn **Utforskaren** och navigera tooyour **hämtar** mappen och kopiera hello hämtas .exe-fil.</span><span class="sxs-lookup"><span data-stu-id="d84bc-161">On your local computer, open **File Explorer** and navigate tooyour **Downloads** folder and copy hello downloaded .exe file.</span></span> <span data-ttu-id="d84bc-162">hello-filnamn ska föregås av VM-namn.</span><span class="sxs-lookup"><span data-stu-id="d84bc-162">hello filename will be prefixed by your VM name.</span></span> 
13. <span data-ttu-id="d84bc-163">Klistra in hello .exe-fil toohello skrivbordet för den virtuella datorn på den virtuella datorn (via hello RDP-anslutning).</span><span class="sxs-lookup"><span data-stu-id="d84bc-163">On your VM (over hello RDP connection) paste hello .exe file toohello Desktop of your VM.</span></span> 
14. <span data-ttu-id="d84bc-164">Navigera toohello skrivbordet för den virtuella datorn och dubbelklicka på hello .exe.</span><span class="sxs-lookup"><span data-stu-id="d84bc-164">Navigate toohello desktop of your VM and double-click on hello .exe.</span></span> <span data-ttu-id="d84bc-165">Kommer att starta en kommandotolk och sedan montera hello återställningspunkt som en filresurs som du kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="d84bc-165">This will launch a command prompt and then mount hello recovery point as a file share that you can access.</span></span> <span data-ttu-id="d84bc-166">När du är klar skapar hello resursen, Skriv **q** tooclose hello-kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="d84bc-166">When it is finished creating hello share, type **q** tooclose hello command prompt.</span></span>
15. <span data-ttu-id="d84bc-167">Öppna på den virtuella datorn **Utforskaren** och navigera toohello enhetsbeteckning som användes för hello filresurs.</span><span class="sxs-lookup"><span data-stu-id="d84bc-167">On your VM, open **File Explorer** and navigate toohello drive letter that was used for hello file share.</span></span>
16. <span data-ttu-id="d84bc-168">Navigera too\inetpub\wwwroot och kopiera **iisstart.png** från hello filen dela och klistra in den i \inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="d84bc-168">Navigate too\inetpub\wwwroot and copy **iisstart.png** from hello file share and paste it into \inetpub\wwwroot.</span></span> <span data-ttu-id="d84bc-169">Till exempel kopiera F:\inetpub\wwwroot\iisstart.png och klistra in den i c:\inetpub\wwwroot toorecover hello-filen.</span><span class="sxs-lookup"><span data-stu-id="d84bc-169">For example, copy F:\inetpub\wwwroot\iisstart.png and paste it into c:\inetpub\wwwroot toorecover hello file.</span></span>
17. <span data-ttu-id="d84bc-170">På den lokala datorn öppnar du hello webbläsarflik när du är ansluten toohello IP-adressen för hello VM som visar hello IIS standardsida.</span><span class="sxs-lookup"><span data-stu-id="d84bc-170">On your local computer, open hello browser tab where you are connected toohello IP address of hello VM showing hello IIS default page.</span></span> <span data-ttu-id="d84bc-171">Tryck på CTRL + F5 toorefresh hello Webbläsarsida.</span><span class="sxs-lookup"><span data-stu-id="d84bc-171">Press CTRL + F5 toorefresh hello browser page.</span></span> <span data-ttu-id="d84bc-172">Du bör nu se att hello avbildningen har återställts.</span><span class="sxs-lookup"><span data-stu-id="d84bc-172">You should now see that hello image has been restored.</span></span>
18. <span data-ttu-id="d84bc-173">På den lokala datorn, gå tillbaka toohello webbläsarflik hello Azure-portalen och på **steg3: demontera hello diskar efter återställningen** klickar du på hello **demontera diskar** knappen.</span><span class="sxs-lookup"><span data-stu-id="d84bc-173">On your local computer, go back toohello browser tab for hello Azure portal and in **Step 3: Unmount hello disks after recovery** click hello **Unmount Disks** button.</span></span> <span data-ttu-id="d84bc-174">Om du glömmer bort toodo det här steget är hello anslutning toohello monteringspunkt Stäng automatiskt efter 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="d84bc-174">If you forget toodo this step, hello connection toohello mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="d84bc-175">När dessa 12 timmar måste toodownload ett nytt skript toocreate nya monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="d84bc-175">After those 12 hours, you need toodownload a new script toocreate a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="d84bc-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d84bc-176">Next steps</span></span>

<span data-ttu-id="d84bc-177">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="d84bc-177">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d84bc-178">Skapa en säkerhetskopia av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="d84bc-178">Create a backup of a VM</span></span>
> * <span data-ttu-id="d84bc-179">Schemalägga en daglig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="d84bc-179">Schedule a daily backup</span></span>
> * <span data-ttu-id="d84bc-180">Återställa en fil från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="d84bc-180">Restore a file from a backup</span></span>

<span data-ttu-id="d84bc-181">Avancera toohello nästa självstudiekurs toolearn om övervakning av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d84bc-181">Advance toohello next tutorial toolearn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="d84bc-182">Övervaka virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d84bc-182">Monitor virtual machines</span></span>](tutorial-monitoring.md)









