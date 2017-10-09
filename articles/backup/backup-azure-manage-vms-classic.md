---
title: "aaaManage och övervaka virtuella Azure-datorsäkerhetskopieringar | Microsoft Docs"
description: "Lär dig hur toomanage och övervaka en virtuell Azure-datorn säkerhetskopieringar"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: 4372944e-d33a-4f6a-bd5f-d04af2842713
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: trinadhk;markgal;
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: cb95e0b3760c96f7fd563c42cb4c635553f48957
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-common-azure-backup-jobs-and-trigger-alerts-in-hello-classic-portal"></a><span data-ttu-id="29c8b-103">Hantera vanliga Azure Backup-jobb och aviseringar för utlösaren i hello klassiska portalen</span><span class="sxs-lookup"><span data-stu-id="29c8b-103">Manage common Azure Backup jobs and trigger alerts in hello classic portal</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="29c8b-104">Hantera Virtuella Azure-säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="29c8b-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="29c8b-105">Hantera klassiska VM-säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="29c8b-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="29c8b-106">Den här artikeln innehåller information om vanliga hantering och övervakning uppgifter för klassiska modellen virtuella datorer som skyddas i Azure.</span><span class="sxs-lookup"><span data-stu-id="29c8b-106">This article provides information about common management and monitoring tasks for Classic-model virtual machines protected in Azure.</span></span>  

> [!NOTE]
> <span data-ttu-id="29c8b-107">Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="29c8b-107">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="29c8b-108">Se [förbereda din miljö tooback av Azure virtuella datorer](backup-azure-vms-prepare.md) mer information om hur du arbetar med klassisk distribution modellen virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="29c8b-108">See [Prepare your environment tooback up Azure virtual machines](backup-azure-vms-prepare.md) for details on working with Classic deployment model VMs.</span></span>
>
> [!IMPORTANT]
><span data-ttu-id="29c8b-109">Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="29c8b-109">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
>
> <span data-ttu-id="29c8b-110">Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="29c8b-110">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="29c8b-111">Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="29c8b-111">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="29c8b-112">Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="29c8b-112">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="29c8b-113">Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017.</span><span class="sxs-lookup"><span data-stu-id="29c8b-113">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="29c8b-114">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="29c8b-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="29c8b-115">Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="29c8b-115">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="29c8b-116">Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-116">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="29c8b-117">Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="29c8b-117">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>

## <a name="manage-protected-virtual-machines"></a><span data-ttu-id="29c8b-118">Hantera skyddade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="29c8b-118">Manage protected virtual machines</span></span>
<span data-ttu-id="29c8b-119">toomanage skyddade virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="29c8b-119">toomanage protected virtual machines:</span></span>

1. <span data-ttu-id="29c8b-120">tooview och hantera inställningar för säkerhetskopiering för en virtuell dator klickar du på hello **skyddade objekt** fliken.</span><span class="sxs-lookup"><span data-stu-id="29c8b-120">tooview and manage backup settings for a virtual machine click hello **Protected Items** tab.</span></span>
2. <span data-ttu-id="29c8b-121">Klicka på hello namnet på ett skyddat objekt toosee hello **säkerhetskopiering information** fliken som innehåller information om hello senaste säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-121">Click on hello name of a protected item toosee hello **Backup Details** tab, which shows you information about hello last backup.</span></span>

    ![Säkerhetskopiering av virtuell dator](./media/backup-azure-manage-vms/backup-vmdetails.png)
3. <span data-ttu-id="29c8b-123">tooview och hantera inställningarna för säkerhetskopieringsprincip för en virtuell dator klickar du på hello **principer** fliken.</span><span class="sxs-lookup"><span data-stu-id="29c8b-123">tooview and manage backup policy settings for a virtual machine click hello **Policies** tab.</span></span>

    ![Princip för virtuell dator](./media/backup-azure-manage-vms/manage-policy-settings.png)

    <span data-ttu-id="29c8b-125">Hej **Säkerhetskopieringsprinciper** fliken visar du hello befintlig princip.</span><span class="sxs-lookup"><span data-stu-id="29c8b-125">hello **Backup Policies** tab shows you hello existing policy.</span></span> <span data-ttu-id="29c8b-126">Du kan ändra vid behov.</span><span class="sxs-lookup"><span data-stu-id="29c8b-126">You can modify as needed.</span></span> <span data-ttu-id="29c8b-127">Om du behöver toocreate en ny princip klickar du på **skapa** på hello **principer** sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-127">If you need toocreate a new policy click **Create** on hello **Policies** page.</span></span> <span data-ttu-id="29c8b-128">Observera att om du vill tooremove en princip för det inte får ha alla virtuella datorer som är kopplade till den.</span><span class="sxs-lookup"><span data-stu-id="29c8b-128">Note that if you want tooremove a policy it shouldn't have any virtual machines associated with it.</span></span>

    ![Princip för virtuell dator](./media/backup-azure-manage-vms/backup-vmpolicy.png)
4. <span data-ttu-id="29c8b-130">Du kan få mer information om åtgärder eller status för en virtuell dator på hello **jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-130">You can get more information about actions or status for a virtual machine on hello **Jobs** page.</span></span> <span data-ttu-id="29c8b-131">Klicka på ett jobb i hello listan tooget mer information eller filtrera jobb för en specifik virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="29c8b-131">Click a job in hello list tooget more details, or filter jobs for a specific virtual machine.</span></span>

    ![Jobb](./media/backup-azure-manage-vms/backup-job.png)

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="29c8b-133">Säkerhetskopiering på begäran för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="29c8b-133">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="29c8b-134">Du kan utföra en på-begäran säkerhetskopiering av en virtuell dator när den har konfigurerats för skydd.</span><span class="sxs-lookup"><span data-stu-id="29c8b-134">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="29c8b-135">Om hello första säkerhetskopian väntar för hello virtuell dator, säkerhetskopiering på begäran skapar en fullständig kopia av hello virtuell dator i Azure backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="29c8b-135">If hello initial backup is pending for hello virtual machine, on-demand backup will create a full copy of hello virtual machine in Azure backup vault.</span></span> <span data-ttu-id="29c8b-136">Om första säkerhetskopieringen har slutförts är på begäran säkerhetskopiering kommer bara skicka ändringar från tidigare säkerhetskopiering tooAzure säkerhetskopiering valvet dvs. det alltid inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="29c8b-136">If first backup is completed, on-demand backup will only send changes from previous backup tooAzure backup vault i.e. it is always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="29c8b-137">Kvarhållningsintervallet för en säkerhetskopiering på begäran anges tooretention värde har angetts för dagliga kvarhållning i säkerhetskopieringsprincip motsvarande toohello VM.</span><span class="sxs-lookup"><span data-stu-id="29c8b-137">Retention range of an on-demand backup is set tooretention value specified for Daily retention in backup policy corresponding toohello VM.</span></span>  
>
>

<span data-ttu-id="29c8b-138">tootake en på-begäran-säkerhetskopia av en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="29c8b-138">tootake an on-demand backup of a virtual machine:</span></span>

1. <span data-ttu-id="29c8b-139">Navigera toohello **skyddade objekt** och välja **Azure virtuella** som **typen** (om det inte redan är vald) och klicka på **Välj**knappen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-139">Navigate toohello **Protected Items** page and select **Azure Virtual Machine** as **Type** (if not already selected) and click on **Select** button.</span></span>

    ![VM-typ](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="29c8b-141">Välj hello virtuella dator som du vill tootake en på-begäran säkerhetskopiering och klicka på **Säkerhetskopiera nu** knappen på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="29c8b-141">Select hello virtual machine on which you want tootake an on-demand backup and click on **Backup Now** button at hello bottom of hello page.</span></span>

    ![Säkerhetskopiera nu](./media/backup-azure-manage-vms/backup-now.png)

    <span data-ttu-id="29c8b-143">Detta skapar en säkerhetskopiering på hello valda virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="29c8b-143">This will create a backup job on hello selected virtual machine.</span></span> <span data-ttu-id="29c8b-144">Kvarhållningsintervallet för återställningspunkt som skapats via det här jobbet ska vara detsamma som som anges i hello principen som är associerad med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="29c8b-144">Retention range of recovery point created through this job will be same as that specified in hello policy associated with hello virtual machine.</span></span>

    ![Skapa säkerhetskopieringsjobb](./media/backup-azure-manage-vms/creating-job.png)

   > [!NOTE]
   > <span data-ttu-id="29c8b-146">tooview hello principen som är associerad med en virtuell dator, gå nedåt till virtuell dator i hello **skyddade objekt** sida och gå toobackup principflik.</span><span class="sxs-lookup"><span data-stu-id="29c8b-146">tooview hello policy associated with a virtual machine, drill down into virtual machine in hello **Protected Items** page and go toobackup policy tab.</span></span>
   >
   >
3. <span data-ttu-id="29c8b-147">När hello jobbet har skapats kan du klicka på **visa jobb** knapp i hello popup liggande toosee hello motsvarande jobb hello jobb på sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-147">Once hello job is created, you can click on **View job** button in hello toast bar toosee hello corresponding job in hello jobs page.</span></span>

    ![Säkerhetskopieringsjobbet har skapats](./media/backup-azure-manage-vms/created-job.png)
4. <span data-ttu-id="29c8b-149">Efter slutförd hello jobbet en återställningspunkt skapas som du kan använda toorestore hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="29c8b-149">After successful completion of hello job, a recovery point will be created which you can use toorestore hello virtual machine.</span></span> <span data-ttu-id="29c8b-150">Detta ökar också kolumnvärde för hello recovery punkt 1 i **skyddade objekt** sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-150">This will also increment hello recovery point column value by 1 in **Protected Items** page.</span></span>

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="29c8b-151">Sluta skydda virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="29c8b-151">Stop protecting virtual machines</span></span>
<span data-ttu-id="29c8b-152">Du kan välja toostop hello framtida säkerhetskopieringar av en virtuell dator med hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="29c8b-152">You can choose toostop hello future backups of a virtual machine with hello following options:</span></span>

* <span data-ttu-id="29c8b-153">Behålla säkerhetskopierade data som är associerade med den virtuella datorn i Azure Backup-valvet</span><span class="sxs-lookup"><span data-stu-id="29c8b-153">Retain backup data associated with virtual machine in Azure Backup vault</span></span>
* <span data-ttu-id="29c8b-154">Ta bort säkerhetskopierade data som är associerade med den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="29c8b-154">Delete backup data associated with virtual machine</span></span>

<span data-ttu-id="29c8b-155">Om du har valt tooretain säkerhetskopierade data som är associerade med den virtuella datorn kan använda du hello säkerhetskopieringsdata toorestore hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="29c8b-155">If you have selected tooretain backup data associated with virtual machine, you can use hello backup data toorestore hello virtual machine.</span></span> <span data-ttu-id="29c8b-156">Prisinformation för sådana virtuella datorer, klickar du på [här](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="29c8b-156">For pricing details for such virtual machines, click [here](https://azure.microsoft.com/pricing/details/backup/).</span></span>

<span data-ttu-id="29c8b-157">tooStop skydd för en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="29c8b-157">tooStop protection for a virtual machine:</span></span>

1. <span data-ttu-id="29c8b-158">Navigera för**skyddade objekt** och välja **virtuella Azure-datorn** som hello filtertyp (om det inte redan är vald) och klicka på **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-158">Navigate too**Protected Items** page and select **Azure virtual machine** as hello filter type (if not already selected) and click on **Select** button.</span></span>

    ![VM-typ](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="29c8b-160">Välj hello virtuella datorn och klicka på **stoppa skydd** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="29c8b-160">Select hello virtual machine and click on **Stop Protection** at hello bottom of hello page.</span></span>

    ![Stoppa skydd](./media/backup-azure-manage-vms/stop-protection.png)
3. <span data-ttu-id="29c8b-162">Standard Azure Backup tas inte bort hello säkerhetskopierade data som är associerade med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="29c8b-162">By default, Azure Backup doesn’t delete hello backup data associated with hello virtual machine.</span></span>

    ![Bekräfta stoppskydd](./media/backup-azure-manage-vms/confirm-stop-protection.png)

    <span data-ttu-id="29c8b-164">Om du vill säkerhetskopiera data för toodelete, markera kryssrutan för hello.</span><span class="sxs-lookup"><span data-stu-id="29c8b-164">If you want toodelete backup data, select hello check box.</span></span>

    ![Kryssruta](./media/backup-azure-manage-vms/checkbox.png)

    <span data-ttu-id="29c8b-166">Välj en orsak till stoppar hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="29c8b-166">Please select a reason for stopping hello backup.</span></span> <span data-ttu-id="29c8b-167">När det här är valfritt kommer att ange en orsak hjälpa Azure Backup toowork på hello feedback och prioritera hello kundscenarier.</span><span class="sxs-lookup"><span data-stu-id="29c8b-167">While this is optional, providing a reason will help Azure Backup toowork on hello feedback and prioritize hello customer scenarios.</span></span>
4. <span data-ttu-id="29c8b-168">Klicka på **skicka** knappen toosubmit hello **sluta skydda** jobb.</span><span class="sxs-lookup"><span data-stu-id="29c8b-168">Click on **Submit** button toosubmit hello **Stop protection** job.</span></span> <span data-ttu-id="29c8b-169">Klicka på **visa jobb** toosee hello motsvarande hello jobb i **jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-169">Click on **View Job** toosee hello corresponding hello job in **Jobs** page.</span></span>

    ![Stoppa skydd](./media/backup-azure-manage-vms/stop-protect-success.png)

    <span data-ttu-id="29c8b-171">Om du inte har valt **ta bort associerade säkerhetskopieringsdata** alternativ under **stoppa skydd** guiden och sedan efter jobbet har slutförts, skydd ändras status för**skyddet avbrutits**.</span><span class="sxs-lookup"><span data-stu-id="29c8b-171">If you have not selected **Delete associated backup data** option during **Stop Protection** wizard, then post job completion, protection status changes too**Protection Stopped**.</span></span> <span data-ttu-id="29c8b-172">hello data förblir med Azure Backup tills den raderas explicit.</span><span class="sxs-lookup"><span data-stu-id="29c8b-172">hello data remains with Azure Backup until it is explicitly deleted.</span></span> <span data-ttu-id="29c8b-173">Du kan alltid ta bort hello data genom att välja hello virtuell dator i hello **skyddade objekt** sidan och klicka på **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="29c8b-173">You can always delete hello data by selecting hello virtual machine in hello **Protected Items** page and clicking **Delete**.</span></span>

    ![Stoppad skydd](./media/backup-azure-manage-vms/protection-stopped-status.png)

    <span data-ttu-id="29c8b-175">Om du har valt hello **ta bort associerade säkerhetskopieringsdata** alternativet och hello virtuella datorn kommer inte att en del av hello **skyddade objekt** sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-175">If you have selected hello **Delete associated backup data** option, hello virtual machine won’t be part of hello **Protected Items** page.</span></span>

## <a name="re-protect-virtual-machine"></a><span data-ttu-id="29c8b-176">Skydda virtuella datorn på nytt</span><span class="sxs-lookup"><span data-stu-id="29c8b-176">Re-protect Virtual machine</span></span>
<span data-ttu-id="29c8b-177">Om du inte har valt hello **ta bort associera säkerhetskopieringsdata** alternativet i **stoppa skydd**, du kan skydda hello virtuella datorn igen genom att följa hello steg liknande toobacking in registrerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="29c8b-177">If you have not selected hello **Delete associate backup data** option in **Stop Protection**, you can re-protect hello virtual machine by following hello steps similar toobacking up registered virtual machines.</span></span> <span data-ttu-id="29c8b-178">När skyddade, kommer den här virtuella datorn har säkerhetskopieringsdata behålls tidigare toostop skydd och återställningspunkter som skapats efter att skydda.</span><span class="sxs-lookup"><span data-stu-id="29c8b-178">Once protected, this virtual machine will have backup data retained prior toostop protection and recovery points created after re-protect.</span></span>

<span data-ttu-id="29c8b-179">Efter att skydda skyddsstatus hello virtuella datorn kommer att ändras för**skyddade** om det finns återställningspunkter tidigare för**stoppa skydd**.</span><span class="sxs-lookup"><span data-stu-id="29c8b-179">After re-protect, hello virtual machine’s protection status will be changed too**Protected** if there are recovery points prior too**Stop Protection**.</span></span>

  ![Redundansväxlade virtuella datorn](./media/backup-azure-manage-vms/reprotected-status.png)

> [!NOTE]
> <span data-ttu-id="29c8b-181">Du kan välja en annan princip än hello princip som virtuella datorn ursprungligen skyddades när skydda hello virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-181">When re-protecting hello virtual machine, you can choose a different policy than hello policy with which virtual machine was protected initially.</span></span>
>
>

## <a name="unregister-virtual-machines"></a><span data-ttu-id="29c8b-182">Avregistrera virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="29c8b-182">Unregister virtual machines</span></span>
<span data-ttu-id="29c8b-183">Om du vill tooremove hello virtuell dator från hello säkerhetskopieringsvalv:</span><span class="sxs-lookup"><span data-stu-id="29c8b-183">If you want tooremove hello virtual machine from hello backup vault:</span></span>

1. <span data-ttu-id="29c8b-184">Klicka på hello **AVREGISTRERA** knappen på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="29c8b-184">Click on hello **UNREGISTER** button at hello bottom of hello page.</span></span>

    ![Inaktivera skydd](./media/backup-azure-manage-vms/unregister-button.png)

    <span data-ttu-id="29c8b-186">Ett popup-meddelande visas längst ned hello hello-skärmen uppmanar dig att bekräfta.</span><span class="sxs-lookup"><span data-stu-id="29c8b-186">A toast notification will appear at hello bottom of hello screen requesting confirmation.</span></span> <span data-ttu-id="29c8b-187">Klicka på **Ja** toocontinue.</span><span class="sxs-lookup"><span data-stu-id="29c8b-187">Click **YES** toocontinue.</span></span>

    ![Inaktivera skydd](./media/backup-azure-manage-vms/confirm-unregister.png)

## <a name="delete-backup-data"></a><span data-ttu-id="29c8b-189">Ta bort säkerhetskopierade data</span><span class="sxs-lookup"><span data-stu-id="29c8b-189">Delete Backup data</span></span>
<span data-ttu-id="29c8b-190">Du kan ta bort hello säkerhetskopierade data som är kopplade till en virtuell dator antingen:</span><span class="sxs-lookup"><span data-stu-id="29c8b-190">You can delete hello backup data associated with a virtual machine, either:</span></span>

* <span data-ttu-id="29c8b-191">Under stoppa Skyddsjobbet</span><span class="sxs-lookup"><span data-stu-id="29c8b-191">During Stop Protection Job</span></span>
* <span data-ttu-id="29c8b-192">När en stoppskydd har jobbet slutförts på en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="29c8b-192">After a stop protection job is completed on a virtual machine</span></span>

<span data-ttu-id="29c8b-193">toodelete säkerhetskopierade data på en virtuell dator som är i hello *skyddet avbrutits* tillstånd efter slutförande av en **stoppa säkerhetskopiering** jobb:</span><span class="sxs-lookup"><span data-stu-id="29c8b-193">toodelete backup data on a virtual machine, which is in hello *Protection Stopped* state post successful completion of a **Stop Backup** job:</span></span>

1. <span data-ttu-id="29c8b-194">Navigera toohello **skyddade objekt** och välja **Azure virtuella** som *typen* och klicka på hello **Välj** knappen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-194">Navigate toohello **Protected Items** page and select **Azure Virtual Machine** as *type* and click hello **Select** button.</span></span>

    ![VM-typ](./media/backup-azure-manage-vms/vm-type.png)
2. <span data-ttu-id="29c8b-196">Välj hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="29c8b-196">Select hello virtual machine.</span></span> <span data-ttu-id="29c8b-197">hello virtuella datorn kommer att vara i **skyddet avbrutits** tillstånd.</span><span class="sxs-lookup"><span data-stu-id="29c8b-197">hello virtual machine will be in **Protection Stopped** state.</span></span>

    ![Skyddet har stoppats](./media/backup-azure-manage-vms/protection-stopped-b.png)
3. <span data-ttu-id="29c8b-199">Klicka på hello **ta bort** knappen på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="29c8b-199">Click hello **DELETE** button at hello bottom of hello page.</span></span>

    ![Ta bort säkerhetskopia](./media/backup-azure-manage-vms/delete-backup.png)
4. <span data-ttu-id="29c8b-201">I hello **ta bort säkerhetskopieringsdata** guiden, Välj en orsak för att ta bort säkerhetskopieringsdata (rekommenderas) och klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="29c8b-201">In hello **Delete backup data** wizard, select a reason for deleting backup data (highly recommended) and click **Submit**.</span></span>

    ![Ta bort säkerhetskopierade data](./media/backup-azure-manage-vms/delete-backup-data.png)
5. <span data-ttu-id="29c8b-203">Detta skapar ett jobb toodelete säkerhetskopierade data för den valda virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="29c8b-203">This will create a job toodelete backup data of selected virtual machine.</span></span> <span data-ttu-id="29c8b-204">Klicka på **visa jobb** toosee motsvarande jobb i jobb-sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-204">Click **View job** toosee corresponding job in Jobs page.</span></span>

    ![Data har tagits bort](./media/backup-azure-manage-vms/delete-data-success.png)

    <span data-ttu-id="29c8b-206">När hello jobbet är slutfört, hello post motsvarande toohello virtuell dator tas bort från **skyddade objekt** sidan.</span><span class="sxs-lookup"><span data-stu-id="29c8b-206">Once hello job is completed, hello entry corresponding toohello virtual machine will be removed from **Protected items** page.</span></span>

## <a name="dashboard"></a><span data-ttu-id="29c8b-207">Instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="29c8b-207">Dashboard</span></span>
<span data-ttu-id="29c8b-208">På hello **instrumentpanelen** sidan som du kan granska information om virtuella Azure-datorer, lagring och jobb som är kopplade till dem i hello senaste 24 timmarna.</span><span class="sxs-lookup"><span data-stu-id="29c8b-208">On hello **Dashboard** page you can review information about Azure virtual machines, their storage, and jobs associated with them in hello last 24 hours.</span></span> <span data-ttu-id="29c8b-209">Du kan visa status för säkerhetskopiering och eventuella tillhörande säkerhetskopiering fel.</span><span class="sxs-lookup"><span data-stu-id="29c8b-209">You can view backup status and any associated backup errors.</span></span>

![Instrumentpanel](./media/backup-azure-manage-vms/dashboard-protectedvms.png)

> [!NOTE]
> <span data-ttu-id="29c8b-211">Värdena i hello instrumentpanelen uppdateras var 24: e timme.</span><span class="sxs-lookup"><span data-stu-id="29c8b-211">Values in hello dashboard are refreshed once every 24 hours.</span></span>
>
>

## <a name="auditing-operations"></a><span data-ttu-id="29c8b-212">Granska Operations</span><span class="sxs-lookup"><span data-stu-id="29c8b-212">Auditing Operations</span></span>
<span data-ttu-id="29c8b-213">Azure backup är granskning av hello ”åtgärdsloggar” av säkerhetskopieringsåtgärder utlöses av hello kunden, vilket gör det enkelt toosee exakt vilka hanteringsåtgärder som utförts på hello säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="29c8b-213">Azure backup provides review of hello "operation logs" of backup operations triggered by hello customer making it easy toosee exactly what management operations were performed on hello backup vault.</span></span> <span data-ttu-id="29c8b-214">Operations loggar aktivera bra post före och gransknings-och stöd för hello säkerhetskopieringsåtgärder.</span><span class="sxs-lookup"><span data-stu-id="29c8b-214">Operations logs enable great post-mortem and audit support for hello backup operations.</span></span>

<span data-ttu-id="29c8b-215">följande åtgärder hello loggas i åtgärdsloggar:</span><span class="sxs-lookup"><span data-stu-id="29c8b-215">hello following operations are logged in Operation logs:</span></span>

* <span data-ttu-id="29c8b-216">Registrera dig</span><span class="sxs-lookup"><span data-stu-id="29c8b-216">Register</span></span>
* <span data-ttu-id="29c8b-217">Avregistrera</span><span class="sxs-lookup"><span data-stu-id="29c8b-217">Unregister</span></span>
* <span data-ttu-id="29c8b-218">Konfigurera skydd</span><span class="sxs-lookup"><span data-stu-id="29c8b-218">Configure protection</span></span>
* <span data-ttu-id="29c8b-219">Säkerhetskopiering (både schemalagd samt säkerhetskopiering på begäran via BackupNow)</span><span class="sxs-lookup"><span data-stu-id="29c8b-219">Backup ( Both scheduled as well as on-demand backup through BackupNow)</span></span>
* <span data-ttu-id="29c8b-220">Återställ</span><span class="sxs-lookup"><span data-stu-id="29c8b-220">Restore</span></span>
* <span data-ttu-id="29c8b-221">Stoppa skydd</span><span class="sxs-lookup"><span data-stu-id="29c8b-221">Stop protection</span></span>
* <span data-ttu-id="29c8b-222">Ta bort säkerhetskopierade data</span><span class="sxs-lookup"><span data-stu-id="29c8b-222">Delete backup data</span></span>
* <span data-ttu-id="29c8b-223">Lägg till princip</span><span class="sxs-lookup"><span data-stu-id="29c8b-223">Add policy</span></span>
* <span data-ttu-id="29c8b-224">Ta bort princip</span><span class="sxs-lookup"><span data-stu-id="29c8b-224">Delete policy</span></span>
* <span data-ttu-id="29c8b-225">Uppdatera principen</span><span class="sxs-lookup"><span data-stu-id="29c8b-225">Update policy</span></span>
* <span data-ttu-id="29c8b-226">Avbryta jobb</span><span class="sxs-lookup"><span data-stu-id="29c8b-226">Cancel job</span></span>

<span data-ttu-id="29c8b-227">tooview åtgärden loggar motsvarande tooa säkerhetskopieringsvalv:</span><span class="sxs-lookup"><span data-stu-id="29c8b-227">tooview operation logs corresponding tooa backup vault:</span></span>

1. <span data-ttu-id="29c8b-228">Navigera för**hanteringstjänster** i Azure-portalen och klicka sedan på hello **Åtgärdsloggar** fliken.</span><span class="sxs-lookup"><span data-stu-id="29c8b-228">Navigate too**Management services** in Azure portal, and then click hello **Operation Logs** tab.</span></span>

    ![Åtgärdsloggar](./media/backup-azure-manage-vms/ops-logs.png)
2. <span data-ttu-id="29c8b-230">Markera i hello filter **säkerhetskopiering** som *typ* och ange hello säkerhetskopieringsvalvet namn i *tjänstnamn* och klicka på **skicka**.</span><span class="sxs-lookup"><span data-stu-id="29c8b-230">In hello filters, select **Backup** as *Type* and specify hello backup vault name in *service name* and click on **Submit**.</span></span>

    ![Åtgärden loggar Filter](./media/backup-azure-manage-vms/ops-logs-filter.png)
3. <span data-ttu-id="29c8b-232">Markera alla åtgärder i hello operations loggar och klicka **information** toosee information motsvarande tooan igen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-232">In hello operations logs, select any operation and click  **Details** toosee details corresponding tooan operation.</span></span>

    ![Information om hämtning av loggar](./media/backup-azure-manage-vms/ops-logs-details.png)

    <span data-ttu-id="29c8b-234">Hej **information guiden** innehåller information om hello åtgärden utlöses, jobb-Id som den här åtgärden utlöses och starttid för hello åtgärden.</span><span class="sxs-lookup"><span data-stu-id="29c8b-234">hello **Details wizard** contains information about hello operation triggered, job Id, resource on which this operation is triggered, and start time of hello operation.</span></span>

    ![Åtgärdsinformation](./media/backup-azure-manage-vms/ops-logs-details-window.png)

## <a name="alert-notifications"></a><span data-ttu-id="29c8b-236">Varningsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="29c8b-236">Alert notifications</span></span>
<span data-ttu-id="29c8b-237">Du kan hämta anpassade aviseringar för hello jobb i portalen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-237">You can get custom alert notifications for hello jobs in portal.</span></span> <span data-ttu-id="29c8b-238">Detta uppnås genom att definiera PowerShell-baserade Varningsregler operativa loggar händelser.</span><span class="sxs-lookup"><span data-stu-id="29c8b-238">This is achieved by defining PowerShell-based alert rules on operational logs events.</span></span> <span data-ttu-id="29c8b-239">Vi rekommenderar att du använder *PowerShell version 1.3.0 eller senare*.</span><span class="sxs-lookup"><span data-stu-id="29c8b-239">We recommend using *PowerShell version 1.3.0 or above*.</span></span>

<span data-ttu-id="29c8b-240">toodefine ett anpassat meddelande tooalert för Säkerhetskopieringsfel ett Exempelkommando ser ut:</span><span class="sxs-lookup"><span data-stu-id="29c8b-240">toodefine a custom notification tooalert for backup failures, a sample command will look like:</span></span>

```
PS C:\> $actionEmail = New-AzureRmAlertRuleEmail -CustomEmail contoso@microsoft.com
PS C:\> Add-AzureRmLogAlertRule -Name backupFailedAlert -Location "East US" -ResourceGroup RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US -OperationName Microsoft.Backup/backupVault/Backup -Status Failed -TargetResourceId /subscriptions/86eeac34-eth9a-4de3-84db-7a27d121967e/resourceGroups/RecoveryServices-DP2RCXUGWS3MLJF4LKPI3A3OMJ2DI4SRJK6HIJH22HFIHZVVELRQ-East-US/providers/microsoft.backupbvtd2/BackupVault/trinadhVault -Actions $actionEmail
```

<span data-ttu-id="29c8b-241">**ResourceId**: du kan hämta det från Operations Logs popup enligt ovan i avsnittet.</span><span class="sxs-lookup"><span data-stu-id="29c8b-241">**ResourceId**: You can get this from Operations Logs popup as described in above section.</span></span> <span data-ttu-id="29c8b-242">ResourceUri i information popup-fönster för en åtgärd är hello ResourceId toobe som angetts för denna cmdlet.</span><span class="sxs-lookup"><span data-stu-id="29c8b-242">ResourceUri in details popup window of an operation is hello ResourceId toobe supplied for this cmdlet.</span></span>

<span data-ttu-id="29c8b-243">**OperationName**: Detta blir hello format ”Microsoft.Backup/backupvault/<EventName>” där EventName är en av registrera, avregistrera, ConfigureProtection, säkerhetskopiering, återställning, StopProtection, DeleteBackupData, CreateProtectionPolicy DeleteProtectionPolicy, UpdateProtectionPolicy</span><span class="sxs-lookup"><span data-stu-id="29c8b-243">**OperationName**: This will be of hello format "Microsoft.Backup/backupvault/<EventName>" where EventName is one of Register,Unregister,ConfigureProtection,Backup,Restore,StopProtection,DeleteBackupData,CreateProtectionPolicy,DeleteProtectionPolicy,UpdateProtectionPolicy</span></span>

<span data-ttu-id="29c8b-244">**Status för**: värden är igång, som stöds har lyckats och misslyckats.</span><span class="sxs-lookup"><span data-stu-id="29c8b-244">**Status**: Supported values are- Started, Succeeded and Failed.</span></span>

<span data-ttu-id="29c8b-245">**ResourceGroup**: ResourceGroup hello resurs som åtgärden utlöses.</span><span class="sxs-lookup"><span data-stu-id="29c8b-245">**ResourceGroup**:ResourceGroup of hello resource on which operation is triggered.</span></span> <span data-ttu-id="29c8b-246">Du kan hämta den från ResourceId värde.</span><span class="sxs-lookup"><span data-stu-id="29c8b-246">You can obtain this from ResourceId value.</span></span> <span data-ttu-id="29c8b-247">Värdet mellan fält */resourceGroups/* och */providers/* i ResourceId hello värde anges för ResourceGroup.</span><span class="sxs-lookup"><span data-stu-id="29c8b-247">Value between fields */resourceGroups/* and */providers/* in ResourceId value is hello value for ResourceGroup.</span></span>

<span data-ttu-id="29c8b-248">**Namnet**: namnet på hello varningsregel.</span><span class="sxs-lookup"><span data-stu-id="29c8b-248">**Name**: Name of hello Alert Rule.</span></span>

<span data-ttu-id="29c8b-249">**CustomEmail**: Ange hello anpassade e-postadress toowhich du vill toosend aviseringsmeddelanden</span><span class="sxs-lookup"><span data-stu-id="29c8b-249">**CustomEmail**: Specify hello custom email address toowhich you want toosend alert notification</span></span>

<span data-ttu-id="29c8b-250">**SendToServiceOwners**: det här alternativet skickas aviseringsmeddelanden tooall administratörer och medadministratörer för hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="29c8b-250">**SendToServiceOwners**: This option sends alert notification tooall administrators and co-administrators of hello subscription.</span></span> <span data-ttu-id="29c8b-251">Den kan användas i **ny AzureRmAlertRuleEmail** cmdlet</span><span class="sxs-lookup"><span data-stu-id="29c8b-251">It can be used in **New-AzureRmAlertRuleEmail** cmdlet</span></span>

### <a name="limitations-on-alerts"></a><span data-ttu-id="29c8b-252">Begränsningar för aviseringar</span><span class="sxs-lookup"><span data-stu-id="29c8b-252">Limitations on Alerts</span></span>
<span data-ttu-id="29c8b-253">Händelsebaserat aviseringar har utsatts toohello följande begränsningar:</span><span class="sxs-lookup"><span data-stu-id="29c8b-253">Event-based alerts are subjected toohello following limitations:</span></span>

1. <span data-ttu-id="29c8b-254">Aviseringar aktiveras på alla virtuella datorer i hello säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="29c8b-254">Alerts are triggered on all virtual machines in hello backup vault.</span></span> <span data-ttu-id="29c8b-255">Du kan anpassa den tooget aviseringar för en specifik uppsättning virtuella datorer i ett säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="29c8b-255">You cannot customize it tooget alerts for specific set of virtual machines in a backup vault.</span></span>
2. <span data-ttu-id="29c8b-256">Den här funktionen är i förhandsgranskningen.</span><span class="sxs-lookup"><span data-stu-id="29c8b-256">This feature is in Preview.</span></span> [<span data-ttu-id="29c8b-257">Läs mer</span><span class="sxs-lookup"><span data-stu-id="29c8b-257">Learn more</span></span>](../monitoring-and-diagnostics/insights-powershell-samples.md#create-metric-alerts)
3. <span data-ttu-id="29c8b-258">Du får aviseringar från ”alerts-noreply@mail.windowsazure.com”.</span><span class="sxs-lookup"><span data-stu-id="29c8b-258">You will receive alerts from "alerts-noreply@mail.windowsazure.com".</span></span> <span data-ttu-id="29c8b-259">För närvarande kan du ändra hello e-avsändaren.</span><span class="sxs-lookup"><span data-stu-id="29c8b-259">Currently you can't modify hello email sender.</span></span>

## <a name="next-steps"></a><span data-ttu-id="29c8b-260">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="29c8b-260">Next steps</span></span>
* [<span data-ttu-id="29c8b-261">Återställa virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="29c8b-261">Restore Azure VMs</span></span>](backup-azure-restore-vms.md)
