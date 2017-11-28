---
title: aaaBack virtuella Azure-datorer | Microsoft Docs
description: "Identifiera, registrera och säkerhetskopiera virtuella datorer i Azure tooa recovery services-valvet."
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
ms.openlocfilehash: a204a42726450a7fd89b5563a786b5070578b113
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-tooa-recovery-services-vault"></a><span data-ttu-id="10340-104">Säkerhetskopiera virtuella datorer i Azure tooa Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="10340-104">Back up Azure virtual machines tooa Recovery Services vault</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="10340-105">Säkerhetskopiera virtuella datorer tooRecovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="10340-105">Back up VMs tooRecovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="10340-106">Säkerhetskopiera virtuella datorer tooBackup valvet</span><span class="sxs-lookup"><span data-stu-id="10340-106">Back up VMs tooBackup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="10340-107">Den här artikeln beskrivs hur tooback in virtuella Azure-datorer (både distribuerade Resource Manager och klassisk distribuerade) tooa Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="10340-107">This article details how tooback up Azure VMs (both Resource Manager-deployed and Classic-deployed) tooa Recovery Services vault.</span></span> <span data-ttu-id="10340-108">De flesta hello arbete för att säkerhetskopiera virtuella datorer är hello förberedelser.</span><span class="sxs-lookup"><span data-stu-id="10340-108">Most of hello work for backing up VMs is hello preparation.</span></span> <span data-ttu-id="10340-109">Innan du kan säkerhetskopiera eller mellan en virtuell dator, måste du slutföra hello [krav](backup-azure-arm-vms-prepare.md) tooprepare din miljö för att skydda dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="10340-109">Before you can back up or protect a VM, you must complete hello [prerequisites](backup-azure-arm-vms-prepare.md) tooprepare your environment for protecting your VMs.</span></span> <span data-ttu-id="10340-110">När du har slutfört hello krav, kan du initiera hello Säkerhetskopieringsåtgärden tootake ögonblicksbilder av den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="10340-110">Once you have completed hello prerequisites, then you can initiate hello backup operation tootake snapshots of your VM.</span></span>


[!INCLUDE [learn about backup deployment models](../../includes/backup-deployment-models.md)]

<span data-ttu-id="10340-111">Mer information finns i hello artiklar på [planerar din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md) och [virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="10340-111">For more information, see hello articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

## <a name="triggering-hello-backup-job"></a><span data-ttu-id="10340-112">Utlösande hello säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="10340-112">Triggering hello backup job</span></span>
<span data-ttu-id="10340-113">hello princip för säkerhetskopiering som är associerade med hello Recovery Services-valvet definierar när och hur ofta hello Säkerhetskopieringsåtgärden körs.</span><span class="sxs-lookup"><span data-stu-id="10340-113">hello backup policy associated with hello Recovery Services vault defines how often and when hello backup operation runs.</span></span> <span data-ttu-id="10340-114">Som standard är hello första schemalagd säkerhetskopiering hello första säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="10340-114">By default, hello first scheduled backup is hello initial backup.</span></span> <span data-ttu-id="10340-115">Tills hello första säkerhetskopian inträffar hello senaste Status för säkerhetskopiering på hello **säkerhetskopieringsjobb** bladet visas som **varning (första säkerhetskopian väntande)**.</span><span class="sxs-lookup"><span data-stu-id="10340-115">Until hello initial backup occurs, hello Last Backup Status on hello **Backup Jobs** blade shows as **Warning(initial backup pending)**.</span></span>

![Säkerhetskopiering väntar](./media/backup-azure-vms-first-look-arm/initial-backup-not-run.png)

<span data-ttu-id="10340-117">Om din första säkerhetskopian förfaller toobegin snart, rekommenderas att du kör **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="10340-117">Unless your initial backup is due toobegin soon, it is recommended that you run **Back up Now**.</span></span> <span data-ttu-id="10340-118">hello börjar följande procedur från instrumentpanelen för hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="10340-118">hello following procedure starts from hello vault dashboard.</span></span> <span data-ttu-id="10340-119">Den här proceduren används för att köra hello inledande säkerhetskopieringsjobbet när du har slutfört alla krav.</span><span class="sxs-lookup"><span data-stu-id="10340-119">This procedure serves for running hello initial backup job after you have completed all prerequisites.</span></span> <span data-ttu-id="10340-120">Den här proceduren är inte tillgängligt om hello inledande säkerhetskopieringsjobbet redan har körts.</span><span class="sxs-lookup"><span data-stu-id="10340-120">If hello initial backup job has already been run, this procedure is not available.</span></span> <span data-ttu-id="10340-121">hello anger associerade säkerhetskopieringsprincip hello nästa säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="10340-121">hello associated backup policy determines hello next backup job.</span></span>  

<span data-ttu-id="10340-122">toorun hello inledande säkerhetskopieringsjobbet:</span><span class="sxs-lookup"><span data-stu-id="10340-122">toorun hello initial backup job:</span></span>

1. <span data-ttu-id="10340-123">Hello valvet instrumentpanelen, klicka på hello antalet under **säkerhetskopiering objekt**, eller klicka på hello **säkerhetskopiering objekt** panelen.</span><span class="sxs-lookup"><span data-stu-id="10340-123">On hello vault dashboard, click hello number under **Backup Items**, or click hello **Backup Items** tile.</span></span> <br/><span data-ttu-id="10340-124">
  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span><span class="sxs-lookup"><span data-stu-id="10340-124">
![Settings icon](./media/backup-azure-vms-first-look-arm/rs-vault-config-vm-back-up-now-1.png)</span></span>

  <span data-ttu-id="10340-125">Hej **säkerhetskopiering objekt** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="10340-125">hello **Backup Items** blade opens.</span></span>

  ![Säkerhetskopieringsobjekt](./media/backup-azure-vms-first-look-arm/back-up-items-list.png)

2. <span data-ttu-id="10340-127">På hello **säkerhetskopiering objekt** bladet, Välj hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="10340-127">On hello **Backup Items** blade, select hello item.</span></span>

  ![Ikonen Inställningar](./media/backup-azure-vms-first-look-arm/back-up-items-list-selected.png)

  <span data-ttu-id="10340-129">Hej **säkerhetskopiering objekt** lista öppnas.</span><span class="sxs-lookup"><span data-stu-id="10340-129">hello **Backup Items** list opens.</span></span> <br/>

  ![Säkerhetskopieringsjobbet utlöses](./media/backup-azure-vms-first-look-arm/backup-items-not-run.png)

3. <span data-ttu-id="10340-131">På hello **säkerhetskopiering objekt** klickar du på ellipserna hello **...**  tooopen hello snabbmenyn.</span><span class="sxs-lookup"><span data-stu-id="10340-131">On hello **Backup Items** list, click hello ellipses **...** tooopen hello Context menu.</span></span>

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu.png)

  <span data-ttu-id="10340-133">hello snabbmenyn visas.</span><span class="sxs-lookup"><span data-stu-id="10340-133">hello Context menu appears.</span></span>

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small.png)

4. <span data-ttu-id="10340-135">Klicka på hello snabbmenyn **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="10340-135">On hello Context menu, click **Backup now**.</span></span>

  ![Snabbmeny](./media/backup-azure-vms-first-look-arm/context-menu-small-backup-now.png)

  <span data-ttu-id="10340-137">hello säkerhetskopiering nu blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="10340-137">hello Backup Now blade opens.</span></span>

  ![Visar hello säkerhetskopiering nu bladet](./media/backup-azure-vms-first-look-arm/backup-now-blade-short.png)

5. <span data-ttu-id="10340-139">På hello säkerhetskopiering nu bladet Klicka hello kalendern använder hello kalender kontrollen tooselect hello sista dagen återställningspunkten behålls Klicka på **säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="10340-139">On hello Backup Now blade, click hello calendar icon, use hello calendar control tooselect hello last day this recovery point is retained, and click **Backup**.</span></span>

  ![Ange hello sista dag hello säkerhetskopiering nu återställningspunkten behålls](./media/backup-azure-vms-first-look-arm/backup-now-blade-calendar.png)

  <span data-ttu-id="10340-141">Distribution meddelanden att du vet hello jobbet har utlösts och att du kan övervaka hello hello jobb på sidan för hello säkerhetskopiering jobb.</span><span class="sxs-lookup"><span data-stu-id="10340-141">Deployment notifications let you know hello backup job has been triggered, and that you can monitor hello progress of hello job on hello Backup jobs page.</span></span> <span data-ttu-id="10340-142">Beroende på hello storlek på den virtuella datorn, kan det ta en stund att skapa hello första säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="10340-142">Depending on hello size of your VM, creating hello initial backup may take a while.</span></span>

6. <span data-ttu-id="10340-143">tooview och spåra hello status för hello första säkerhetskopian på hello valvet instrumentpanelen på hello **säkerhetskopieringsjobb** Klicka på panelen **pågår**.</span><span class="sxs-lookup"><span data-stu-id="10340-143">tooview or track hello status of hello initial backup, on hello vault dashboard, on hello **Backup Jobs** tile click **In progress**.</span></span>

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/open-backup-jobs-1.png)

  <span data-ttu-id="10340-145">hello säkerhetskopieringsjobb blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="10340-145">hello Backup Jobs blade opens.</span></span>

  ![Panelen Säkerhetskopieringsjobb](./media/backup-azure-vms-first-look-arm/backup-jobs-in-jobs-view-1.png)

  <span data-ttu-id="10340-147">I hello **säkerhetskopiera jobb** bladet hittar du hello status för alla jobb.</span><span class="sxs-lookup"><span data-stu-id="10340-147">In hello **Backup jobs** blade, you can see hello status of all jobs.</span></span> <span data-ttu-id="10340-148">Kontrollera om hello säkerhetskopieringsjobbet för den virtuella datorn pågår fortfarande eller om den har slutförts.</span><span class="sxs-lookup"><span data-stu-id="10340-148">Check if hello backup job for your VM is still in progress, or if it has finished.</span></span> <span data-ttu-id="10340-149">När ett säkerhetskopieringsjobb är klar är hello status *slutförd*.</span><span class="sxs-lookup"><span data-stu-id="10340-149">When a backup job is finished, hello status is *Completed*.</span></span>

  > [!NOTE]
  > <span data-ttu-id="10340-150">Som en del av hello säkerhetskopieringsåtgärd utfärdar hello Azure Backup service kommandot toohello säkerhetskopiering filnamnstillägget i varje VM-tooflush alla skriver och utför en programkonsekvent ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="10340-150">As a part of hello backup operation, hello Azure Backup service issues a command toohello backup extension in each VM tooflush all writes and take a consistent snapshot.</span></span>
  >
  >

## <a name="troubleshooting-errors"></a><span data-ttu-id="10340-151">Felsökning av fel</span><span class="sxs-lookup"><span data-stu-id="10340-151">Troubleshooting errors</span></span>
<span data-ttu-id="10340-152">Om du stöter på problem under säkerhetskopieringen för den virtuella datorn, se hello [VM felsökningsartikel](backup-azure-vms-troubleshoot.md) för hjälp.</span><span class="sxs-lookup"><span data-stu-id="10340-152">If you run into issues while backing up your virtual machine, see hello [VM troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="10340-153">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="10340-153">Next steps</span></span>
<span data-ttu-id="10340-154">Nu när du har skyddat den virtuella datorn finns hello följande artiklar toolearn om VM hanteringsuppgifter och hur toorestore virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="10340-154">Now that you have protected your VM, see hello following articles toolearn about VM management tasks, and how toorestore VMs.</span></span>

* [<span data-ttu-id="10340-155">Hantera och övervaka dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="10340-155">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="10340-156">Återställa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="10340-156">Restore virtual machines</span></span>](backup-azure-arm-restore-vms.md)
