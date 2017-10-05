---
title: "Säkerhetskopiera virtuella Windows Azure-datorer | Microsoft Docs"
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
ms.openlocfilehash: 8e58a2290e5034ef393f65cbcddb86e18cf4a6ec
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-windows-virtual-machines-in-azure"></a><span data-ttu-id="1b463-103">Säkerhetskopiera Windows-datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="1b463-103">Back up Windows virtual machines in Azure</span></span>

<span data-ttu-id="1b463-104">Du kan skydda dina data genom att säkerhetskopiera med jämna mellanrum.</span><span class="sxs-lookup"><span data-stu-id="1b463-104">You can protect your data by taking backups at regular intervals.</span></span> <span data-ttu-id="1b463-105">Azure-säkerhetskopiering skapar återställningspunkter som är lagrade i geo-redundant recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="1b463-105">Azure Backup creates recovery points that are stored in geo-redundant recovery vaults.</span></span> <span data-ttu-id="1b463-106">När du återställer från en återställningspunkt kan du återställa hela VM eller bara vissa filer.</span><span class="sxs-lookup"><span data-stu-id="1b463-106">When you restore from a recovery point, you can restore the whole VM or just specific files.</span></span> <span data-ttu-id="1b463-107">Den här artikeln förklarar hur du återställer en fil till en virtuell dator som kör Windows Server och IIS.</span><span class="sxs-lookup"><span data-stu-id="1b463-107">This article explains how to restore a single file to a VM running Windows Server and IIS.</span></span> <span data-ttu-id="1b463-108">Om du inte redan har en virtuell dator ska användas, kan du skapa en med hjälp av den [Windows quickstart](quick-create-portal.md).</span><span class="sxs-lookup"><span data-stu-id="1b463-108">If you don't already have a VM to use, you can create one using the [Windows quickstart](quick-create-portal.md).</span></span> <span data-ttu-id="1b463-109">I den här självstudiekursen får du lära du dig att:</span><span class="sxs-lookup"><span data-stu-id="1b463-109">In this tutorial you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b463-110">Skapa en säkerhetskopia av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1b463-110">Create a backup of a VM</span></span>
> * <span data-ttu-id="1b463-111">Schemalägga en daglig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1b463-111">Schedule a daily backup</span></span>
> * <span data-ttu-id="1b463-112">Återställa en fil från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="1b463-112">Restore a file from a backup</span></span>




## <a name="backup-overview"></a><span data-ttu-id="1b463-113">Översikt över Backup</span><span class="sxs-lookup"><span data-stu-id="1b463-113">Backup overview</span></span>

<span data-ttu-id="1b463-114">När tjänsten Azure Backup initierar ett säkerhetskopieringsjobb, utlöser reservanknytning för att ta en ögonblicksbild i tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="1b463-114">When the Azure Backup service initiates a backup job, it triggers the backup extension to take a point-in-time snapshot.</span></span> <span data-ttu-id="1b463-115">Azure Backup-tjänsten används den _VMSnapshot_ tillägg.</span><span class="sxs-lookup"><span data-stu-id="1b463-115">The Azure Backup service uses the _VMSnapshot_ extension.</span></span> <span data-ttu-id="1b463-116">Tillägget installeras under den första säkerhetskopieringen om den virtuella datorn körs.</span><span class="sxs-lookup"><span data-stu-id="1b463-116">The extension is installed during the first VM backup if the VM is running.</span></span> <span data-ttu-id="1b463-117">Om inte den virtuella datorn körs, tar en ögonblicksbild av det underliggande lagringsutrymmet i Backup-tjänsten (eftersom inga program skrivningar inträffar när den virtuella datorn stoppas).</span><span class="sxs-lookup"><span data-stu-id="1b463-117">If the VM is not running, the Backup service takes a snapshot of the underlying storage (since no application writes occur while the VM is stopped).</span></span>

<span data-ttu-id="1b463-118">När en ögonblicksbild av virtuella Windows-datorer, samordnar Backup-tjänsten med Volume Shadow Copy Service (VSS) att hämta en programkonsekvent ögonblicksbild av den virtuella datorns diskar.</span><span class="sxs-lookup"><span data-stu-id="1b463-118">When taking a snapshot of Windows VMs, the Backup service coordinates with the Volume Shadow Copy Service (VSS) to get a consistent snapshot of the virtual machine's disks.</span></span> <span data-ttu-id="1b463-119">När Azure Backup-tjänsten tar ögonblicksbilden kan överföra data till valvet.</span><span class="sxs-lookup"><span data-stu-id="1b463-119">Once the Azure Backup service takes the snapshot, the data is transferred to the vault.</span></span> <span data-ttu-id="1b463-120">För att maximera effektiviteten, tjänsten identifierar och överför endast block av data som har ändrats sedan den tidigare säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="1b463-120">To maximize efficiency, the service identifies and transfers only the blocks of data that have changed since the previous backup.</span></span>

<span data-ttu-id="1b463-121">När dataöverföringen har slutförts ögonblicksbilden tas bort och skapa en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="1b463-121">When the data transfer is complete, the snapshot is removed and a recovery point is created.</span></span>


## <a name="create-a-backup"></a><span data-ttu-id="1b463-122">Skapa en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="1b463-122">Create a backup</span></span>
<span data-ttu-id="1b463-123">Skapa en enkel schemalagd daglig säkerhetskopiering till ett Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="1b463-123">Create a simple scheduled daily backup to a Recovery Services Vault.</span></span> 

1. <span data-ttu-id="1b463-124">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="1b463-124">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="1b463-125">Välj **Virtuella datorer** på menyn till vänster.</span><span class="sxs-lookup"><span data-stu-id="1b463-125">In the menu on the left, select **Virtual machines**.</span></span> 
3. <span data-ttu-id="1b463-126">Välj en virtuell dator som du vill säkerhetskopiera i listan.</span><span class="sxs-lookup"><span data-stu-id="1b463-126">From the list, select a VM to back up.</span></span>
4. <span data-ttu-id="1b463-127">På VM-blad i den **inställningar** klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="1b463-127">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="1b463-128">Den **Aktivera säkerhetskopiering** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="1b463-128">The **Enable backup** blade opens.</span></span>
5. <span data-ttu-id="1b463-129">I **Recovery Services-valvet**, klickar du på **Skapa nytt** och ange namn för det nya valvet.</span><span class="sxs-lookup"><span data-stu-id="1b463-129">In **Recovery Services vault**, click **Create new** and provide the name for the new vault.</span></span> <span data-ttu-id="1b463-130">Ett nytt valv skapas i samma resursgrupp och plats som den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1b463-130">A new vault is created in the same Resource Group and location as the virtual machine.</span></span>
6. <span data-ttu-id="1b463-131">Klicka på **säkerhetskopiera princip**.</span><span class="sxs-lookup"><span data-stu-id="1b463-131">Click **Backup policy**.</span></span> <span data-ttu-id="1b463-132">I det här exemplet att behålla standardinställningarna och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1b463-132">For this example, keep the defaults and click **OK**.</span></span>
7. <span data-ttu-id="1b463-133">På den **Aktivera säkerhetskopiering** bladet, klickar du på **Aktivera säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="1b463-133">On the **Enable backup** blade, click **Enable Backup**.</span></span> <span data-ttu-id="1b463-134">Detta skapar en daglig säkerhetskopiering baserat på standardschemat.</span><span class="sxs-lookup"><span data-stu-id="1b463-134">This creates a daily backup based on the default schedule.</span></span>
10. <span data-ttu-id="1b463-135">Skapa en första återställningspunkten på den **säkerhetskopiering** bladet och klickar på **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="1b463-135">To create an initial recovery point, on the **Backup** blade click **Backup now**.</span></span>
11. <span data-ttu-id="1b463-136">På den **säkerhetskopiering nu** bladet klickar du på kalenderikonen, Använd kalender för att välja den sista dagen i den här återställningspunkten behålls Klicka på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="1b463-136">On the **Backup Now** blade, click the calendar icon, use the calendar control to select the last day this recovery point is retained, and click **Backup**.</span></span>
12. <span data-ttu-id="1b463-137">I den **säkerhetskopiering** bladet för den virtuella datorn, visas antalet återställningspunkter som är klar.</span><span class="sxs-lookup"><span data-stu-id="1b463-137">In the **Backup** blade for your VM, you will see the number of recovery points that are complete.</span></span>

    ![Återställningspunkter](./media/tutorial-backup-vms/backup-complete.png)
    
<span data-ttu-id="1b463-139">Den första säkerhetskopieringen tar ungefär 20 minuter.</span><span class="sxs-lookup"><span data-stu-id="1b463-139">The first backup takes about 20 minutes.</span></span> <span data-ttu-id="1b463-140">När säkerhetskopieringen är klar fortsätter du till nästa del av den här kursen.</span><span class="sxs-lookup"><span data-stu-id="1b463-140">Proceed to the next part of this tutorial after your backup is finished.</span></span>

## <a name="recover-a-file"></a><span data-ttu-id="1b463-141">Återställa en fil</span><span class="sxs-lookup"><span data-stu-id="1b463-141">Recover a file</span></span>

<span data-ttu-id="1b463-142">Om du tar bort av misstag eller göra ändringar i en fil, kan du använda filåterställning för att återställa filen från din säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="1b463-142">If you accidentally delete or make changes to a file, you can use File Recovery to recover the file from your backup vault.</span></span> <span data-ttu-id="1b463-143">Filåterställning använder ett skript som körs på den virtuella datorn att montera återställningspunkten som lokal enhet.</span><span class="sxs-lookup"><span data-stu-id="1b463-143">File Recovery uses a script that runs on the VM, to mount the recovery point as local drive.</span></span> <span data-ttu-id="1b463-144">Dessa enheter förblir monterade 12 timmar så att du kan kopiera filer från återställningspunkten och återställa dem till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1b463-144">These drives will remain mounted for 12 hours so that you can copy files from the recovery point and restore them to the VM.</span></span>  

<span data-ttu-id="1b463-145">I det här exemplet visar vi hur du återställer avbildningsfilen som används i standardwebbsidan för IIS.</span><span class="sxs-lookup"><span data-stu-id="1b463-145">In this example, we show how to recover the image file that is used in the default web page for IIS.</span></span> 

1. <span data-ttu-id="1b463-146">Öppna en webbläsare och ansluta till IP-adressen för den virtuella datorn för att visa sidan med IIS.</span><span class="sxs-lookup"><span data-stu-id="1b463-146">Open a browser and connect to the IP address of the VM to show the default IIS page.</span></span>

    ![Standardwebbsidan för IIS](./media/tutorial-backup-vms/iis-working.png)

2. <span data-ttu-id="1b463-148">Ansluta till den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="1b463-148">Connect to the VM.</span></span>
3. <span data-ttu-id="1b463-149">På den virtuella datorn, öppna **Utforskaren** och navigera till \inetpub\wwwroot och ta bort filen **iisstart.png**.</span><span class="sxs-lookup"><span data-stu-id="1b463-149">On the VM, open **File Explorer** and navigate to \inetpub\wwwroot and delete the file **iisstart.png**.</span></span>
4. <span data-ttu-id="1b463-150">Uppdatera webbläsaren att bilden på sidan IIS är borta på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="1b463-150">On your local computer, refresh the browser to see that the image on the default IIS page is gone.</span></span>

    ![Standardwebbsidan för IIS](./media/tutorial-backup-vms/iis-broken.png)

5. <span data-ttu-id="1b463-152">På den lokala datorn, öppna en ny flik och gå i den [Azure-portalen](https://portal.azure.com).</span><span class="sxs-lookup"><span data-stu-id="1b463-152">On your local computer, open a new tab and go the the [Azure portal](https://portal.azure.com).</span></span>
6. <span data-ttu-id="1b463-153">Välj på menyn till vänster **virtuella datorer** och välj formuläret VM i listan.</span><span class="sxs-lookup"><span data-stu-id="1b463-153">In the menu on the left, select **Virtual machines** and select the VM form the list.</span></span>
8. <span data-ttu-id="1b463-154">På VM-blad i den **inställningar** klickar du på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="1b463-154">On the VM blade, in the **Settings** section, click **Backup**.</span></span> <span data-ttu-id="1b463-155">Den **säkerhetskopiering** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="1b463-155">The **Backup** blade opens.</span></span> 
9. <span data-ttu-id="1b463-156">Välj på menyn längst upp på bladet **filåterställning**.</span><span class="sxs-lookup"><span data-stu-id="1b463-156">In the menu at the top of the blade, select **File Recovery**.</span></span> <span data-ttu-id="1b463-157">Den **filåterställning** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="1b463-157">The **File Recovery** blade opens.</span></span>
10. <span data-ttu-id="1b463-158">I **steg 1: Välj återställningspunkt**, Välj en återställningspunkt från listrutan.</span><span class="sxs-lookup"><span data-stu-id="1b463-158">In **Step 1: Select recovery point**, select a recovery point from the drop-down.</span></span>
11. <span data-ttu-id="1b463-159">I **steg 2: hämta skript för att bläddra och återställa filerna**, klicka på den **hämta körbara** knappen.</span><span class="sxs-lookup"><span data-stu-id="1b463-159">In **Step 2: Download script to browse and recover files**, click the **Download Executable** button.</span></span> <span data-ttu-id="1b463-160">Spara filen på din **hämtar** mapp.</span><span class="sxs-lookup"><span data-stu-id="1b463-160">Save the file to your **Downloads** folder.</span></span>
12. <span data-ttu-id="1b463-161">Öppna på den lokala datorn **Utforskaren** och navigera till din **hämtar** mappen och kopiera den nedladdade .exe-fil.</span><span class="sxs-lookup"><span data-stu-id="1b463-161">On your local computer, open **File Explorer** and navigate to your **Downloads** folder and copy the downloaded .exe file.</span></span> <span data-ttu-id="1b463-162">Filnamnet ska föregås av VM-namn.</span><span class="sxs-lookup"><span data-stu-id="1b463-162">The filename will be prefixed by your VM name.</span></span> 
13. <span data-ttu-id="1b463-163">Klistra in .exe-fil till skrivbordet på den virtuella datorn på den virtuella datorn (via RDP-anslutning).</span><span class="sxs-lookup"><span data-stu-id="1b463-163">On your VM (over the RDP connection) paste the .exe file to the Desktop of your VM.</span></span> 
14. <span data-ttu-id="1b463-164">Navigera till skrivbordet på den virtuella datorn och dubbelklicka på .exe.</span><span class="sxs-lookup"><span data-stu-id="1b463-164">Navigate to the desktop of your VM and double-click on the .exe.</span></span> <span data-ttu-id="1b463-165">Kommer att starta en kommandotolk och sedan montera återställningspunkten som en filresurs som du kan komma åt.</span><span class="sxs-lookup"><span data-stu-id="1b463-165">This will launch a command prompt and then mount the recovery point as a file share that you can access.</span></span> <span data-ttu-id="1b463-166">När du är klar att skapa resursen, Skriv **q** att stänga Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="1b463-166">When it is finished creating the share, type **q** to close the command prompt.</span></span>
15. <span data-ttu-id="1b463-167">Öppna på den virtuella datorn **Utforskaren** och navigera till enhetsbeteckningen som användes för filresursen.</span><span class="sxs-lookup"><span data-stu-id="1b463-167">On your VM, open **File Explorer** and navigate to the drive letter that was used for the file share.</span></span>
16. <span data-ttu-id="1b463-168">Navigera till \inetpub\wwwroot och kopiera **iisstart.png** från filen dela och klistra in den i \inetpub\wwwroot.</span><span class="sxs-lookup"><span data-stu-id="1b463-168">Navigate to \inetpub\wwwroot and copy **iisstart.png** from the file share and paste it into \inetpub\wwwroot.</span></span> <span data-ttu-id="1b463-169">Till exempel F:\inetpub\wwwroot\iisstart.png kopiera och klistra in den i c:\inetpub\wwwroot att återskapa filen.</span><span class="sxs-lookup"><span data-stu-id="1b463-169">For example, copy F:\inetpub\wwwroot\iisstart.png and paste it into c:\inetpub\wwwroot to recover the file.</span></span>
17. <span data-ttu-id="1b463-170">Öppna fliken webbläsare där du är ansluten till IP-adressen för den virtuella datorn som visar sidan IIS på den lokala datorn.</span><span class="sxs-lookup"><span data-stu-id="1b463-170">On your local computer, open the browser tab where you are connected to the IP address of the VM showing the IIS default page.</span></span> <span data-ttu-id="1b463-171">Tryck på CTRL + F5 för att uppdatera sidan webbläsare.</span><span class="sxs-lookup"><span data-stu-id="1b463-171">Press CTRL + F5 to refresh the browser page.</span></span> <span data-ttu-id="1b463-172">Du bör nu se att avbildningen har återställts.</span><span class="sxs-lookup"><span data-stu-id="1b463-172">You should now see that the image has been restored.</span></span>
18. <span data-ttu-id="1b463-173">På den lokala datorn, gå tillbaka till fliken för Azure-portalen och i **steg3: demontera diskarna efter återställningen** klickar du på den **demontera diskar** knappen.</span><span class="sxs-lookup"><span data-stu-id="1b463-173">On your local computer, go back to the browser tab for the Azure portal and in **Step 3: Unmount the disks after recovery** click the **Unmount Disks** button.</span></span> <span data-ttu-id="1b463-174">Om du glömmer att göra det här steget är anslutningen till monteringspunkt Stäng automatiskt efter 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="1b463-174">If you forget to do this step, the connection to the mountpoint is automatically close after 12 hours.</span></span> <span data-ttu-id="1b463-175">Du måste hämta ett nytt skript om du vill skapa en ny monteringspunkt efter dessa 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="1b463-175">After those 12 hours, you need to download a new script to create a new mountpoint.</span></span>


## <a name="next-steps"></a><span data-ttu-id="1b463-176">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1b463-176">Next steps</span></span>

<span data-ttu-id="1b463-177">I den här självstudiekursen lärde du dig att:</span><span class="sxs-lookup"><span data-stu-id="1b463-177">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="1b463-178">Skapa en säkerhetskopia av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="1b463-178">Create a backup of a VM</span></span>
> * <span data-ttu-id="1b463-179">Schemalägga en daglig säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="1b463-179">Schedule a daily backup</span></span>
> * <span data-ttu-id="1b463-180">Återställa en fil från en säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="1b463-180">Restore a file from a backup</span></span>

<span data-ttu-id="1b463-181">Gå vidare till nästa kurs mer information om övervakning av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="1b463-181">Advance to the next tutorial to learn about monitoring virtual machines.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="1b463-182">Övervaka virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="1b463-182">Monitor virtual machines</span></span>](tutorial-monitoring.md)









