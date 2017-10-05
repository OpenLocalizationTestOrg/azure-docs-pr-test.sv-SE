---
title: "Säkerhetskopiera virtuella datorer i Azure | Microsoft Docs"
description: "Identifiera, registrera och säkerhetskopiera virtuella Azure-datorer till ett recovery services-valv."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopiering av virtuella datorer Säkerhetskopiera virtuella; säkerhetskopiering och katastrofåterställning återställning. arm säkerhetskopiering"
ms.assetid: 5c68481d-7be3-4e68-b87c-0961c267053e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: trinadhk;jimpark;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 40983a3de104238d09b976b5fcf2419da42c1bba
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="back-up-azure-virtual-machines-to-a-recovery-services-vault"></a><span data-ttu-id="3cd7a-104">Säkerhetskopiera virtuella Azure-datorer till ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="3cd7a-104">Back up Azure virtual machines to a Recovery Services vault</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="3cd7a-105">Säkerhetskopiera virtuella datorer till Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="3cd7a-105">Back up VMs to Recovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="3cd7a-106">Säkerhetskopiera virtuella datorer till Backup-valvet</span><span class="sxs-lookup"><span data-stu-id="3cd7a-106">Back up VMs to Backup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="3cd7a-107">Den här artikeln beskrivs hur du säkerhetskopierar virtuella Azure-datorer (både distribuerade Resource Manager och klassisk distribuerade) till Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-107">This article details how to back up Azure VMs (both Resource Manager-deployed and Classic-deployed) to a Recovery Services vault.</span></span> <span data-ttu-id="3cd7a-108">Det mesta av arbetet för att säkerhetskopiera virtuella datorer är förberedelserna.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-108">Most of the work for backing up VMs is the preparation.</span></span> <span data-ttu-id="3cd7a-109">Innan du kan säkerhetskopiera eller mellan en virtuell dator, måste du slutföra de [krav](backup-azure-arm-vms-prepare.md) att förbereda miljön för att skydda dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-109">Before you can back up or protect a VM, you must complete the [prerequisites](backup-azure-arm-vms-prepare.md) to prepare your environment for protecting your VMs.</span></span> <span data-ttu-id="3cd7a-110">När du har slutfört förutsättningarna kan du starta säkerhetskopiering för att ta ögonblicksbilder av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-110">Once you have completed the prerequisites, then you can initiate the backup operation to take snapshots of your VM.</span></span>


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

<span data-ttu-id="3cd7a-111">Mer information finns i artiklar på [planerar din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md) och [virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="3cd7a-111">For more information, see the articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

## <a name="triggering-the-backup-job"></a><span data-ttu-id="3cd7a-112">Utlöser säkerhetskopieringsjobbet</span><span class="sxs-lookup"><span data-stu-id="3cd7a-112">Triggering the backup job</span></span>
<span data-ttu-id="3cd7a-113">Den säkerhetskopieringsprincip som är kopplade till Recovery Services-valvet definierar hur ofta och när Säkerhetskopieringsåtgärden körs.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-113">The backup policy associated with the Recovery Services vault defines how often and when the backup operation runs.</span></span> <span data-ttu-id="3cd7a-114">Som standard är den första säkerhetskopian i den första schemalagda säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-114">By default, the first scheduled backup is the initial backup.</span></span> <span data-ttu-id="3cd7a-115">Innan den första säkerhetskopieringen har körts visas Status för senaste säkerhetskopiering på bladet **Säkerhetskopieringsjobb** som **Varning (första säkerhetskopiering väntar)**.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-115">Until the initial backup occurs, the Last Backup Status on the **Backup Jobs** blade shows as **Warning(initial backup pending)**.</span></span>

![Säkerhetskopiering väntar](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

<span data-ttu-id="3cd7a-117">Såvida inte den första säkerhetskopieringen kommer att börja snart, rekommenderar vi att du kör **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-117">Unless your initial backup is due to begin soon, it is recommended that you run **Back up Now**.</span></span> <span data-ttu-id="3cd7a-118">Följande procedur börjar från instrumentpanelen valvet.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-118">The following procedure starts from the vault dashboard.</span></span> <span data-ttu-id="3cd7a-119">Den här proceduren används för att köra det inledande säkerhetskopieringsjobbet när du har slutfört alla krav.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-119">This procedure serves for running the initial backup job after you have completed all prerequisites.</span></span> <span data-ttu-id="3cd7a-120">Den här proceduren är inte tillgängligt om inledande säkerhetskopieringsjobbet redan har körts.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-120">If the initial backup job has already been run, this procedure is not available.</span></span> <span data-ttu-id="3cd7a-121">Den associera säkerhetskopieringsprincipen anger nästa säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-121">The associated backup policy determines the next backup job.</span></span>  

<span data-ttu-id="3cd7a-122">Så här kör du det första säkerhetskopieringsjobbet:</span><span class="sxs-lookup"><span data-stu-id="3cd7a-122">To run the initial backup job:</span></span>

1. <span data-ttu-id="3cd7a-123">På instrumentpanelen för valvet klickar du på numret under **Säkerhetskopieringsobjekt**, eller på panelen **Säkerhetskopieringsobjekt**.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-123">On the vault dashboard, click the number under **Backup Items**, or click the **Backup Items** tile.</span></span> <br/><span data-ttu-id="3cd7a-124">
  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span><span class="sxs-lookup"><span data-stu-id="3cd7a-124">
![Settings icon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span></span>

  <span data-ttu-id="3cd7a-125">Bladet **Säkerhetskopieringsobjekt** öppnas.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-125">The **Backup Items** blade opens.</span></span>

  ![Säkerhetskopieringsobjekt](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. <span data-ttu-id="3cd7a-127">Välj objektet på bladet **Säkerhetskopieringsobjekt**.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-127">On the **Backup Items** blade, select the item.</span></span>

  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  <span data-ttu-id="3cd7a-129">Listan **Säkerhetskopieringsobjekt** öppnas.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-129">The **Backup Items** list opens.</span></span> <br/>

  ![Säkerhetskopieringsjobbet utlöses](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. <span data-ttu-id="3cd7a-131">Öppna snabbmenyn genom att klicka på ellipserna **...** i listan **Säkerhetskopieringsobjekt**.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-131">On the **Backup Items** list, click the ellipses **...** to open the Context menu.</span></span>

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu.png)

  <span data-ttu-id="3cd7a-133">Snabbmenyn visas.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-133">The Context menu appears.</span></span>

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. <span data-ttu-id="3cd7a-135">Klicka på **Säkerhetskopiera nu** på snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-135">On the Context menu, click **Backup now**.</span></span>

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  <span data-ttu-id="3cd7a-137">Bladet Säkerhetskopiera nu öppnas.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-137">The Backup Now blade opens.</span></span>

  ![Visar bladet Säkerhetskopiera nu](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. <span data-ttu-id="3cd7a-139">På bladet Säkerhetskopiera nu klickar du på kalenderikonen, använder kalenderkontrollen för att välja den sista dagen som den här återställningspunkten ska behållas och klickar sedan på **Säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-139">On the Backup Now blade, click the calendar icon, use the calendar control to select the last day this recovery point is retained, and click **Backup**.</span></span>

  ![Ange den sista dagen som återställningspunkten som skapas med Säkerhetskopiera nu ska behållas](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  <span data-ttu-id="3cd7a-141">Distributionsmeddelanden visas som anger att säkerhetskopieringsjobbet har initierats, och du kan övervaka förloppet för jobbet på sidan Säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-141">Deployment notifications let you know the backup job has been triggered, and that you can monitor the progress of the job on the Backup jobs page.</span></span> <span data-ttu-id="3cd7a-142">Beroende på den virtuella datorns storlek kan det ta en stund att skapa den första säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-142">Depending on the size of your VM, creating the initial backup may take a while.</span></span>

6. <span data-ttu-id="3cd7a-143">Du kan visa eller spåra statusen för den första säkerhetskopieringen genom att klicka på **Pågår** på panelen **Säkerhetskopieringsjobb** på instrumentpanelen för valvet.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-143">To view or track the status of the initial backup, on the vault dashboard, on the **Backup Jobs** tile click **In progress**.</span></span>

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  <span data-ttu-id="3cd7a-145">Bladet Säkerhetskopieringsjobb öppnas.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-145">The Backup Jobs blade opens.</span></span>

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  <span data-ttu-id="3cd7a-147">Du kan se statusen för alla jobb på bladet **Säkerhetskopieringsjobb**.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-147">In the **Backup jobs** blade, you can see the status of all jobs.</span></span> <span data-ttu-id="3cd7a-148">Kontrollera om säkerhetskopieringsjobbet för den virtuella datorn fortfarande körs, eller om det har slutförts.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-148">Check if the backup job for your VM is still in progress, or if it has finished.</span></span> <span data-ttu-id="3cd7a-149">När säkerhetskopieringsjobbet är klart visas statusen *Slutfört*.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-149">When a backup job is finished, the status is *Completed*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="3cd7a-150">Som en del av säkerhetskopieringen skickar tjänsten Azure Backup ett kommando till säkerhetskopieringstillägget på varje virtuell dator som instruerar det att tömma alla skrivningar och använda en konsekvent ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-150">As a part of the backup operation, the Azure Backup service issues a command to the backup extension in each VM to flush all writes and take a consistent snapshot.</span></span>
  >
  >

## <a name="troubleshooting-errors"></a><span data-ttu-id="3cd7a-151">Felsökning av fel</span><span class="sxs-lookup"><span data-stu-id="3cd7a-151">Troubleshooting errors</span></span>
<span data-ttu-id="3cd7a-152">Om du stöter på problem när säkerhetskopiera den virtuella datorn finns på [VM felsökningsartikel](backup-azure-vms-troubleshoot.md) för hjälp.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-152">If you run into issues while backing up your virtual machine, see the [VM troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3cd7a-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3cd7a-153">Next steps</span></span>
<span data-ttu-id="3cd7a-154">Nu när du har skyddat den virtuella datorn finns i följande artiklar om VM hanteringsuppgifter och återställa virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3cd7a-154">Now that you have protected your VM, see the following articles to learn about VM management tasks, and how to restore VMs.</span></span>

* [<span data-ttu-id="3cd7a-155">Hantera och övervaka dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3cd7a-155">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="3cd7a-156">Återställa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3cd7a-156">Restore virtual machines</span></span>](backup-azure-arm-restore-vms.md)
