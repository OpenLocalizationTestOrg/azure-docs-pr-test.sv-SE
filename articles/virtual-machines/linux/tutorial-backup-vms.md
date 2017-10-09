---
title: "Säkerhetskopiera virtuella Azure Linux-datorer | Microsoft Docs"
description: "Skydda din virtuella Linux-datorer genom att säkerhetskopiera dem med Azure Backup."
services: virtual-machines-linux
documentationcenter: virtual-machines
author: cynthn
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 
ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 07/27/2017
ms.author: cynthn
ms.custom: mvc
ms.openlocfilehash: 7c00392d5185a2f067f2ee2717529dcbde1e71f5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-linux--virtual-machines-in-azure"></a><span data-ttu-id="e3204-103">Säkerhetskopiera virtuella Linux-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="e3204-103">Back up Linux  virtual machines in Azure</span></span>

<span data-ttu-id="e3204-104">Du kan skydda dina data genom att säkerhetskopiera med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="e3204-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="e3204-105">Azure-säkerhetskopiering skapar återställningspunkter som är lagrade i geo-redundant recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="e3204-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="e3204-106">När du återställer från en återställningspunkt kan du återställa hello hela VM eller bara vissa filer.</span><span class="sxs-lookup"><span data-stu-id="e3204-106">When you restore from a recovery point, you can restore hello whole VM or just specific files.</span></span> <span data-ttu-id="e3204-107">Den här artikeln förklarar hur toorestore en enda fil tooa Linux VM körs nginx.</span><span class="sxs-lookup"><span data-stu-id="e3204-107">This article explains how toorestore a single file tooa Linux VM running nginx.</span></span> <span data-ttu-id="e3204-108">Om du inte redan har en VM-toouse kan du skapa en med hjälp av hello [Linux quickstart](quick-create-cli.md).</span><span class="sxs-lookup"><span data-stu-id="e3204-108">If you don't already have a VM toouse, you can create one using hello [Linux quickstart](quick-create-cli.md).</span></span> <span data-ttu-id="e3204-109">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="e3204-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3204-110">Skapa en säkerhetskopia av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e3204-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="e3204-111">Schemalägga en daglig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e3204-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="e3204-112">Återställa en fil från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="e3204-112">Restore a file from a backup</span></span>



## <a name="backup-overview"></a><span data-ttu-id="e3204-113">Översikt över Backup</span><span class="sxs-lookup"><span data-stu-id="e3204-113">Backup overview</span></span>

<span data-ttu-id="e3204-114">När hello Azure Backup service initierar en säkerhetskopia, utlöser hello reservanknytning tootake en tidpunkt i ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="e3204-114">When hello Azure Backup service initiates a backup, it triggers hello backup extension tootake a point-in-time snapshot.</span></span> <span data-ttu-id="e3204-115">hello Azure Backup-tjänsten använder hello _VMSnapshotLinux_ tillägget Linux.</span><span class="sxs-lookup"><span data-stu-id="e3204-115">hello Azure Backup service uses hello _VMSnapshotLinux_ extension in Linux.</span></span> <span data-ttu-id="e3204-116">hello tillägget installeras under hello första säkerhetskopiering om hello VM körs.</span><span class="sxs-lookup"><span data-stu-id="e3204-116">hello extension is installed during hello first VM backup if hello VM is running.</span></span> <span data-ttu-id="e3204-117">Om hello inte körs, tar en ögonblicksbild av hello underliggande lagring (eftersom inga program skrivningar sker när hello VM stoppas) hello Backup-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="e3204-117">If hello VM is not running, hello Backup service takes a snapshot of hello underlying storage (since no application writes occur while hello VM is stopped).</span></span>

<span data-ttu-id="e3204-118">Standard Azure Backup tar en konsekvent filsystemsäkerhetskopia för Linux VM men det kan vara konfigurerade tootake [konsekvent säkerhetskopiering av program med skript före och efter skript framework](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span><span class="sxs-lookup"><span data-stu-id="e3204-118">By default, Azure Backup takes a file system consistent backup for Linux VM but it can be configured tootake [application consistent backup using pre-script and post-script framework](https://docs.microsoft.com/azure/backup/backup-azure-linux-app-consistent).</span></span> <span data-ttu-id="e3204-119">När hello Azure Backup-tjänsten tar hello ögonblicksbild, är hello data överförda toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="e3204-119">Once hello Azure Backup service takes hello snapshot, hello data is transferred toohello vault.</span></span> <span data-ttu-id="e3204-120">toomaximize effektivitet hello-tjänsten identifierar och överför bara hello datablock som har ändrats sedan tidigare hello-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="e3204-120">toomaximize efficiency, hello service identifies and transfers only hello blocks of data that have changed since hello previous backup.</span></span>

<span data-ttu-id="e3204-121">När hello dataöverföring är klar hello ögonblicksbild tas bort och skapa en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="e3204-121">When hello data transfer is complete, hello snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="e3204-122">Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="e3204-122">Create a backup</span></span>
<span data-ttu-id="e3204-123">Skapa en enkel schemalagd daglig säkerhetskopiering tooa Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="e3204-123">Create a simple scheduled daily backup tooa Recovery Services Vault.</span></span> 

1. <span data-ttu-id="e3204-124">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e3204-124">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e3204-125">Välj hello menyn hello vänster **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e3204-125">In hello menu on hello left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="e3204-126">Välj en VM-tooback in hello listan.</span><span class="sxs-lookup"><span data-stu-id="e3204-126">From hello list, select a VM tooback up.</span></span>
4. <span data-ttu-id="e3204-127">Hello VM bladet i hello **inställningar** klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="e3204-127">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="e3204-128">Hej **Aktivera säkerhetskopiering** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="e3204-128">hello **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="e3204-129">I **Recovery Services-valvet**, klickar du på **Skapa nytt** och ange hello namn för hello nytt valv.</span><span class="sxs-lookup"><span data-stu-id="e3204-129">In **Recovery Services vault**, click **Create new** and provide hello name for hello new vault.</span></span> <span data-ttu-id="e3204-130">Ett nytt valv skapas i hello samma resursgrupp och plats som hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="e3204-130">A new vault is created in hello same Resource Group and location as hello virtual machine.</span></span>
6. <span data-ttu-id="e3204-131">Klicka på **säkerhetskopiera princip**.</span><span class="sxs-lookup"><span data-stu-id="e3204-131">Click **Backup policy**.</span></span> <span data-ttu-id="e3204-132">I det här exemplet hålla hello standardvärden och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="e3204-132">For this example, keep hello defaults and click **OK**.</span></span>
7. <span data-ttu-id="e3204-133">På hello **Aktivera säkerhetskopiering** bladet, klickar du på **Aktivera säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="e3204-133">On hello **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="e3204-134">Detta skapar en daglig säkerhetskopiering baserat på hello standardschemat.</span><span class="sxs-lookup"><span data-stu-id="e3204-134">This creates a daily backup based on hello default schedule.</span></span>
10. <span data-ttu-id="e3204-135">toocreate en första återställningspunkten på hello **säkerhetskopiering** bladet och klickar på **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="e3204-135">toocreate an initial recovery point, on hello **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="e3204-136">På hello **säkerhetskopiering nu** bladet Klicka hello kalendern använder hello kalender kontrollen tooselect hello sista dagen återställningspunkten behålls Klicka på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="e3204-136">On hello **Backup Now** blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="e3204-137">I hello **säkerhetskopiering** bladet för den virtuella datorn visas hello antal återställningspunkter som är klar.</span><span class="sxs-lookup"><span data-stu-id="e3204-137">In hello **Backup** blade for your VM, you will see hello number of recovery points that are complete.</span></span>

    ![Återställningspunkter](./media/tutorial-backup-vms/backup-complete.png)

<span data-ttu-id="e3204-139">hello första säkerhetskopieringen tar ungefär 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="e3204-139">hello first backup takes about 20 minutes.</span></span> <span data-ttu-id="e3204-140">När säkerhetskopieringen är klar går du vidare toohello nästa del av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="e3204-140">Proceed toohello next part of this tutorial after your backup is finished.</span></span>

## <a name="restore-a-file"></a><span data-ttu-id="e3204-141">Återställa en fil</span><span class="sxs-lookup"><span data-stu-id="e3204-141">Restore a file</span></span>

<span data-ttu-id="e3204-142">Om du tar bort av misstag eller göra ändringar tooa fil, kan du använda filåterställning toorecover hello-fil från din backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="e3204-142">If you accidentally delete or make changes tooa file, you can use File Recovery toorecover hello file from your backup vault.</span></span> <span data-ttu-id="e3204-143">Filåterställning använder ett skript som körs på hello VM, toomount hello återställningspunkt som lokal enhet.</span><span class="sxs-lookup"><span data-stu-id="e3204-143">File Recovery uses a script that runs on hello VM, toomount hello recovery point as local drive.</span></span> <span data-ttu-id="e3204-144">Dessa enheter förblir monterade 12 timmar så att du kan kopiera filer från hello återställningspunkt och återställa dem toohello VM.</span><span class="sxs-lookup"><span data-stu-id="e3204-144">These drives will remain mounted for 12 hours so that you can copy files from hello recovery point and restore them toohello VM.</span></span>  

<span data-ttu-id="e3204-145">I det här exemplet visar vi hur toorecover hello standard nginx webbsida /var/www/html/index.nginx-debian.html.</span><span class="sxs-lookup"><span data-stu-id="e3204-145">In this example, we show how toorecover hello default nginx web page /var/www/html/index.nginx-debian.html.</span></span> <span data-ttu-id="e3204-146">hello offentliga IP-adressen för våra VM i det här exemplet är *13.69.75.209*.</span><span class="sxs-lookup"><span data-stu-id="e3204-146">hello public IP address of our VM in this example is *13.69.75.209*.</span></span> <span data-ttu-id="e3204-147">Du kan hitta hello IP-adress för virtuell dator med hjälp av:</span><span class="sxs-lookup"><span data-stu-id="e3204-147">You can find hello IP address of your vm using:</span></span>

 ```bash 
 az vm show --resource-group myResourceGroup --name myVM -d --query [publicIps] --o tsv
 ```

 
1. <span data-ttu-id="e3204-148">Öppna en webbläsare och Skriv i hello offentliga IP-adressen för din Virtuella toosee hello nginx standardwebbsidan på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="e3204-148">On your local computer, open a browser and type in hello public IP address of your VM toosee hello default nginx web page.</span></span>

    ![Standard nginx-webbsida](./media/tutorial-backup-vms/nginx-working.png)

1. <span data-ttu-id="e3204-150">SSH till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e3204-150">SSH into your VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
2. <span data-ttu-id="e3204-151">Ta bort /var/www/html/index.nginx-debian.html.</span><span class="sxs-lookup"><span data-stu-id="e3204-151">Delete /var/www/html/index.nginx-debian.html.</span></span>

    ```bash
    sudo rm /var/www/html/index.nginx-debian.html
    ```
    
4. <span data-ttu-id="e3204-152">Uppdatera hello webbläsaren på den lokala datorn genom att träffa CTRL + F5 toosee som standardsida nginx är borta.</span><span class="sxs-lookup"><span data-stu-id="e3204-152">On your local computer, refresh hello browser by hitting CTRL + F5 toosee that default nginx page is gone.</span></span>

    ![Standard nginx-webbsida](./media/tutorial-backup-vms/nginx-broken.png)
    
1. <span data-ttu-id="e3204-154">På den lokala datorn loggar du in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e3204-154">On your local computer, sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
6. <span data-ttu-id="e3204-155">Välj hello menyn hello vänster **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="e3204-155">In hello menu on hello left, select **Virtual machines**.</span></span> 
7. <span data-ttu-id="e3204-156">Välj hello VM hello listan.</span><span class="sxs-lookup"><span data-stu-id="e3204-156">From hello list, select hello VM.</span></span>
8. <span data-ttu-id="e3204-157">Hello VM bladet i hello **inställningar** klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="e3204-157">On hello VM blade, in hello **Settings** section, click **Backup**.</span></span> <span data-ttu-id="e3204-158">Hej **säkerhetskopiering** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="e3204-158">hello **Backup** blade opens.</span></span> 
9. <span data-ttu-id="e3204-159">Välj hello menyn hello överst på bladet hello **filåterställning**.</span><span class="sxs-lookup"><span data-stu-id="e3204-159">In hello menu at hello top of hello blade, select **File Recovery**.</span></span> <span data-ttu-id="e3204-160">Hej **filåterställning** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="e3204-160">hello **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="e3204-161">I **steg 1: Välj återställningspunkt**, Välj en återställningspunkt hello i listrutan.</span><span class="sxs-lookup"><span data-stu-id="e3204-161">In **Step 1: Select recovery point**, select a recovery point from hello drop-down.</span></span>
11. <span data-ttu-id="e3204-162">I **steg 2: hämta skriptet toobrowse och återställa filerna**, klicka på hello **hämta körbara** knappen.</span><span class="sxs-lookup"><span data-stu-id="e3204-162">In **Step 2: Download script toobrowse and recover files**, click hello **Download Executable** button.</span></span> <span data-ttu-id="e3204-163">Spara hello nedladdade filen tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="e3204-163">Save hello downloaded file tooyour local computer.</span></span>
7. <span data-ttu-id="e3204-164">Klicka på **hämta skriptet** toodownload hello skriptfilen lokalt.</span><span class="sxs-lookup"><span data-stu-id="e3204-164">Click **Download script** toodownload hello script file locally.</span></span>
8. <span data-ttu-id="e3204-165">Öppna ett Bash-Kommandotolken och Skriv hello efter, ersätter *Linux_myVM_05-05-2017.sh* med hello rätt sökväg och filnamn för hello-skript som du hämtade *azureuser* med hello användarnamn för hello VM och *13.69.75.209* med hello offentliga IP-adressen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e3204-165">Open a Bash prompt and type hello following, replacing *Linux_myVM_05-05-2017.sh* with hello correct path and filename for hello script that you downloaded, *azureuser* with hello username for hello VM and *13.69.75.209* with hello public IP address for your VM.</span></span>
    
    ```bash
    scp Linux_myVM_05-05-2017.sh azureuser@13.69.75.209:
    ```
    
9. <span data-ttu-id="e3204-166">Öppna en SSH-anslutning toohello VM på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="e3204-166">On your local computer, open an SSH connection toohello VM.</span></span>

    ```bash
    ssh 13.69.75.209
    ```
    
10. <span data-ttu-id="e3204-167">Lägg till på den virtuella datorn köra behörigheter toohello skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="e3204-167">On your VM, add execute permissions toohello script file.</span></span>

    ```bash
    chmod +x Linux_myVM_05-05-2017.sh
    ```
    
11. <span data-ttu-id="e3204-168">På den virtuella datorn kör hello skriptet toomount hello återställningspunkt som ett filsystem.</span><span class="sxs-lookup"><span data-stu-id="e3204-168">On your VM, run hello script toomount hello recovery point as a filesystem.</span></span>

    ```bash
    ./Linux_myVM_05-05-2017.sh
    ```
    
12. <span data-ttu-id="e3204-169">hello utdata från hello skriptet ger du hello sökvägen för hello monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="e3204-169">hello output from hello script gives you hello path for hello mount point.</span></span> <span data-ttu-id="e3204-170">hello utdata ser ut ungefär toothis:</span><span class="sxs-lookup"><span data-stu-id="e3204-170">hello output looks similar toothis:</span></span>

    ```bash
    Microsoft Azure VM Backup - File Recovery
    ______________________________________________
                          
    Connecting toorecovery point using ISCSI service...
    
    Connection succeeded!
    
    Please wait while we attach volumes of hello recovery point toothis machine...
                         
    ************ Volumes of hello recovery point and their mount paths on this machine ************

    Sr.No.  |  Disk  |  Volume  |  MountPath 

    1)  | /dev/sdc  |  /dev/sdc1  |  /home/azureuser/myVM-20170505191055/Volume1

    ************ Open File Explorer toobrowse for files. ************

    After recovery, tooremove hello disks and close hello connection toohello recovery point, please click 'Unmount Disks' in step 3 of hello portal.

    Please enter 'q/Q' tooexit...
    ```

12. <span data-ttu-id="e3204-171">Kopiera hello nginx standardwebbsidan på den virtuella datorn från hello montera punkt tillbaka toowhere hello filen tas bort.</span><span class="sxs-lookup"><span data-stu-id="e3204-171">On your VM, copy hello nginx default web page from hello mount point back toowhere you deleted hello file.</span></span>

    ```bash
    sudo cp ~/myVM-20170505191055/Volume1/var/www/html/index.nginx-debian.html /var/www/html/
    ```
    
17. <span data-ttu-id="e3204-172">På den lokala datorn öppnar du hello webbläsarflik när du är ansluten toohello IP-adressen för hello VM som visar hello nginx standardsida.</span><span class="sxs-lookup"><span data-stu-id="e3204-172">On your local computer, open hello browser tab where you are connected toohello IP address of hello VM showing hello nginx default page.</span></span> <span data-ttu-id="e3204-173">Tryck på CTRL + F5 toorefresh hello Webbläsarsida.</span><span class="sxs-lookup"><span data-stu-id="e3204-173">Press CTRL + F5 toorefresh hello browser page.</span></span> <span data-ttu-id="e3204-174">Du bör nu se att hello standardsida fungerar igen.</span><span class="sxs-lookup"><span data-stu-id="e3204-174">You should now see that hello default page is working again.</span></span>

    ![Standard nginx-webbsida](./media/tutorial-backup-vms/nginx-working.png)

18. <span data-ttu-id="e3204-176">På den lokala datorn, gå tillbaka toohello webbläsarflik hello Azure-portalen och på **steg3: demontera hello diskar efter återställningen** klickar du på hello **demontera diskar** knappen.</span><span class="sxs-lookup"><span data-stu-id="e3204-176">On your local computer, go back toohello browser tab for hello Azure portal and in **Step 3: Unmount hello disks after recovery** click hello **Unmount Disks** button.</span></span> <span data-ttu-id="e3204-177">Om du glömmer bort toodo det här steget är hello anslutning toohello monteringspunkt Stäng automatiskt efter 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="e3204-177">If you forget toodo this step, hello connection toohello mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="e3204-178">När dessa 12 timmar måste toodownload ett nytt skript toocreate nya monteringspunkt.</span><span class="sxs-lookup"><span data-stu-id="e3204-178">After those 12 hours, you need toodownload a new script toocreate a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="e3204-179">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e3204-179">Next steps</span></span>

<span data-ttu-id="e3204-180">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="e3204-180">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e3204-181">Skapa en säkerhetskopia av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e3204-181">Create a backup of a VM</span></span>
> * <span data-ttu-id="e3204-182">Schemalägga en daglig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e3204-182">Schedule a daily backup</span></span>
> * <span data-ttu-id="e3204-183">Återställa en fil från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="e3204-183">Restore a file from a backup</span></span>

<span data-ttu-id="e3204-184">Avancera toohello nästa självstudiekurs toolearn om övervakning av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e3204-184">Advance toohello next tutorial toolearn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="e3204-185">Övervaka virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e3204-185">Monitor virtual machines</span></span>](tutorial-monitoring.md)

