---
title: "En första titt: Säkerhetskopiera virtuella datorer i Azure med ett säkerhetskopieringsvalv | Microsoft Docs"
description: "Använd den klassiska portalen för att säkerhetskopiera virtuella datorer i Azure till ett Backup-valv. I den här kursen beskrivs alla skeden, inklusive att skapa säkerhetskopieringsvalvet, registrera de virtuella datorerna, skapa säkerhetskopieringsprincipen och köra det första säkerhetskopieringsjobbet."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
ms.assetid: 722820dc-b65f-425c-a9e5-c1946e896a87
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: markgal;
ms.openlocfilehash: fc31d7654e455ec5b4e4bb9af4cf1a166f1661ee
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a><span data-ttu-id="d5025-104">En första titt: Säkerhetskopiera virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="d5025-104">First look: Backing up Azure virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="d5025-105">Skydda virtuella datorer med ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="d5025-105">Protect VMs with a recovery services vault</span></span>](backup-azure-vms-first-look-arm.md)
> * [<span data-ttu-id="d5025-106">Skydda virtuella datorer i Azure med ett Backup-valv</span><span class="sxs-lookup"><span data-stu-id="d5025-106">Protect Azure VMs with a backup vault</span></span>](backup-azure-vms-first-look.md)
>
>

<span data-ttu-id="d5025-107">de här självstudierna beskriver steg för steg hur du säkerhetskopierar en virtuell Azure-dator till Azure.</span><span class="sxs-lookup"><span data-stu-id="d5025-107">This tutorial takes you through the steps for backing up an Azure virtual machine (VM) to a backup vault in Azure.</span></span> <span data-ttu-id="d5025-108">Den här artikeln beskriver den klassiska modellen eller Service Manager-distributionsmodellen för att säkerhetskopiera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d5025-108">This article describes the classic model or Service Manager deployment model, for backing up VMs.</span></span> <span data-ttu-id="d5025-109">Följande steg gäller endast säkerhetskopieringsvalv som har skapats i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="d5025-109">The following steps apply only to Backup vaults created in the classic portal.</span></span> <span data-ttu-id="d5025-110">Microsoft rekommenderar att Resource Manager-modellen används för nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="d5025-110">Microsoft recommends using the Resource Manager model for new deployments.</span></span>

<span data-ttu-id="d5025-111">Om du vill säkerhetskopiera en virtuell dator till ett Recovery Services-valv som hör till en resursgrupp, hittar du mer information i [En första titt: Skydda virtuella datorer i Azure med ett Recovery Services-valv](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="d5025-111">If you are interested in backing up a VM to a Recovery Services vault that belongs to a Resource Group, see [First look: Protect VMs with a recovery services vault](backup-azure-vms-first-look-arm.md).</span></span>

<span data-ttu-id="d5025-112">Följande krav måste vara uppfyllda för att du ska kunna genomföra den här självstudien:</span><span class="sxs-lookup"><span data-stu-id="d5025-112">To successfully complete the following tutorial, these prerequisites must exist:</span></span>

* <span data-ttu-id="d5025-113">Du har skapat en virtuell dator i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="d5025-113">You have created a VM in your Azure subscription.</span></span>
* <span data-ttu-id="d5025-114">Den virtuella datorn kan ansluta till Azures offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="d5025-114">The VM has connectivity to Azure public IP addresses.</span></span> <span data-ttu-id="d5025-115">Mer information finns i [Nätverksanslutningar](backup-azure-vms-prepare.md#network-connectivity).</span><span class="sxs-lookup"><span data-stu-id="d5025-115">For additional information, see [Network connectivity](backup-azure-vms-prepare.md#network-connectivity).</span></span>


> [!NOTE]
> <span data-ttu-id="d5025-116">Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="d5025-116">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="d5025-117">Den här självstudiekursen handlar om virtuella datorer som har skapats i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="d5025-117">This tutorial is for use with virtual machines created in the classic portal.</span></span>
>
>

## <a name="create-a-backup-vault"></a><span data-ttu-id="d5025-118">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="d5025-118">Create a backup vault</span></span>
<span data-ttu-id="d5025-119">Ett säkerhetskopieringsvalv är en entitet som lagrar alla säkerhetskopior och återställningspunkter som har skapats med tiden.</span><span class="sxs-lookup"><span data-stu-id="d5025-119">A backup vault is an entity that stores all the backups and recovery points that have been created over time.</span></span> <span data-ttu-id="d5025-120">Säkerhetskopieringsvalvet innehåller även säkerhetskopieringspolicyerna som tillämpas på de virtuella datorer som säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="d5025-120">The backup vault also contains the backup policies that are applied to the virtual machines being backed up.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="d5025-121">Från och med mars 2017 kan du inte längre använda den klassiska portalen för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="d5025-121">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
> <span data-ttu-id="d5025-122">Du kan uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="d5025-122">You can upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="d5025-123">Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv).</span><span class="sxs-lookup"><span data-stu-id="d5025-123">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="d5025-124">Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="d5025-124">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="d5025-125">Efter den 15 oktober 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="d5025-125">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="d5025-126">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="d5025-126">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="d5025-127">Alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="d5025-127">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="d5025-128">Du kan inte längre komma åt dina säkerhetskopierade data i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="d5025-128">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="d5025-129">Använd i stället Azure Portal till att få åtkomst till dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="d5025-129">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="discover-and-register-azure-virtual-machines"></a><span data-ttu-id="d5025-130">Identifiera och registrera virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="d5025-130">Discover and Register Azure virtual machines</span></span>
<span data-ttu-id="d5025-131">Innan du registrerar den virtuella datorn med ett valv kör du identifieringsprocessen för att identifiera nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d5025-131">Before registering the VM with a vault, run the discovery process to identify any new VMs.</span></span> <span data-ttu-id="d5025-132">När du gör det returneras en lista över virtuella datorer i prenumerationen, jämte ytterligare information som molntjänstens namn och regionen.</span><span class="sxs-lookup"><span data-stu-id="d5025-132">This returns a list of virtual machines in the subscription, along with additional information like the cloud service name and the region.</span></span>

1. <span data-ttu-id="d5025-133">Logga in på den [klassiska Azure-portalen](http://manage.windowsazure.com/).</span><span class="sxs-lookup"><span data-stu-id="d5025-133">Sign in to the [Azure Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="d5025-134">Öppna listan över Recovery Services-valv genom att klicka på **Recovery Services** på den klassiska Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d5025-134">In the Azure classic portal, click **Recovery Services** to open the list of Recovery Services vaults.</span></span>
    <span data-ttu-id="d5025-135">![Välja arbetsbelastning](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span><span class="sxs-lookup"><span data-stu-id="d5025-135">![Select workload](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span></span>
3. <span data-ttu-id="d5025-136">I listan med valv väljer du valvet för att säkerhetskopiera en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d5025-136">From the list of vaults, select the vault to back up a VM.</span></span>

    <span data-ttu-id="d5025-137">När du väljer valvet öppnas det på sidan **Snabbstart**.</span><span class="sxs-lookup"><span data-stu-id="d5025-137">When you select your vault, it opens in the **Quick Start** page</span></span>
4. <span data-ttu-id="d5025-138">Klicka på **Registrerade objekt** på valvmenyn.</span><span class="sxs-lookup"><span data-stu-id="d5025-138">From the vault menu, click **Registered Items**.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. <span data-ttu-id="d5025-140">Välj  **Virtuell Azure-dator** på **Typ**-menyn.</span><span class="sxs-lookup"><span data-stu-id="d5025-140">From the **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="d5025-142">Klicka på **Identifiera** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="d5025-142">Click **DISCOVER** at the bottom of the page.</span></span>
    <span data-ttu-id="d5025-143">![Knappen Identifiera](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="d5025-143">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="d5025-144">Identifieringen kan ta några minuter medan de virtuella datorerna visas i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="d5025-144">The discovery process may take a few minutes while the virtual machines are being tabulated.</span></span> <span data-ttu-id="d5025-145">Ett meddelande visas längst ned på skärmen som anger att processen körs.</span><span class="sxs-lookup"><span data-stu-id="d5025-145">There is a notification at the bottom of the screen that lets you know that the process is running.</span></span>

    ![Identifiera virtuella datorer](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="d5025-147">Meddelandet ändras när processen är klar.</span><span class="sxs-lookup"><span data-stu-id="d5025-147">The notification changes when the process is complete.</span></span>

    ![Identifieringen är klar](./media/backup-azure-vms-first-look/discovery-complete.png)
7. <span data-ttu-id="d5025-149">Klicka på **Registrera** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="d5025-149">Click **REGISTER** at the bottom of the page.</span></span>
    <span data-ttu-id="d5025-150">![Knappen Registrera](./media/backup-azure-vms-first-look/register-icon.png)</span><span class="sxs-lookup"><span data-stu-id="d5025-150">![Register button](./media/backup-azure-vms-first-look/register-icon.png)</span></span>
8. <span data-ttu-id="d5025-151">På snabbmenyn **Registrera objekt** väljer du de virtuella datorer som du vill registrera.</span><span class="sxs-lookup"><span data-stu-id="d5025-151">In the **Register Items** shortcut menu, select the virtual machines that you want to register.</span></span>

   > [!TIP]
   > <span data-ttu-id="d5025-152">Flera virtuella datorer kan registreras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="d5025-152">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="d5025-153">Ett jobb skapas för varje virtuell dator som du har valt.</span><span class="sxs-lookup"><span data-stu-id="d5025-153">A job is created for each virtual machine that you've selected.</span></span>
9. <span data-ttu-id="d5025-154">Klicka på **Visa jobb** i meddelandet för att gå till sidan **Jobb**.</span><span class="sxs-lookup"><span data-stu-id="d5025-154">Click **View Job** in the notification to go to the **Jobs** page.</span></span>

    ![Registrera jobb](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="d5025-156">Den virtuella datorn visas också i listan över registrerade objekt, tillsammans med statusen för registreringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="d5025-156">The virtual machine also appears in the list of registered items, along with the status of the registration operation.</span></span>

    ![Registreringsstatus 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="d5025-158">När åtgärden har slutförts ändras statusen för att återspegla tillståndet för *registrerade objekt*.</span><span class="sxs-lookup"><span data-stu-id="d5025-158">When the operation completes, the status changes to reflect the *registered* state.</span></span>

    ![Registreringsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-the-vm-agent-on-the-virtual-machine"></a><span data-ttu-id="d5025-160">Installera VM-agenten på den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="d5025-160">Install the VM Agent on the virtual machine</span></span>
<span data-ttu-id="d5025-161">VM-agenten i Azure måste installeras på den virtuella Azure-datorn för att säkerhetskopieringstillägget ska fungera.</span><span class="sxs-lookup"><span data-stu-id="d5025-161">The Azure VM Agent must be installed on the Azure virtual machine for the Backup extension to work.</span></span> <span data-ttu-id="d5025-162">Om den virtuella datorn skapades från Azure-galleriet finns VM-agenten redan på den virtuella datorn; du kan gå direkt till att [skydda dina virtuella datorer](backup-azure-vms-first-look.md#create-the-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="d5025-162">If your VM was created from the Azure gallery, the VM Agent is already present on the VM; you can skip to [protecting your VMs](backup-azure-vms-first-look.md#create-the-backup-policy).</span></span>

<span data-ttu-id="d5025-163">Om en virtuell dator migreras från ett lokalt datacenter är VM-agenten antagligen inte installerad på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d5025-163">If your VM migrated from an on-premises datacenter, the VM probably does not have the VM Agent installed.</span></span> <span data-ttu-id="d5025-164">Du måste installera VM-agenten på den virtuella datorn innan du fortsätter att skydda den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="d5025-164">You must install the VM Agent on the virtual machine before proceeding to protect the VM.</span></span> <span data-ttu-id="d5025-165">Detaljerade anvisningar om hur du installerar VM-agenten finns i [avsnittet om VM-agenten i artikeln om säkerhetskopiering av virtuella datorer](backup-azure-vms-prepare.md#vm-agent).</span><span class="sxs-lookup"><span data-stu-id="d5025-165">For detailed steps on installing the VM Agent, see the [VM Agent section of the Backup VMs article](backup-azure-vms-prepare.md#vm-agent).</span></span>

## <a name="create-the-backup-policy"></a><span data-ttu-id="d5025-166">Skapa säkerhetskopieringsprincipen</span><span class="sxs-lookup"><span data-stu-id="d5025-166">Create the backup policy</span></span>
<span data-ttu-id="d5025-167">Innan du initierar det första säkerhetskopieringsjobbet definierar du schemat som anger när ögonblicksbilder av säkerhetskopior ska tas.</span><span class="sxs-lookup"><span data-stu-id="d5025-167">Before you trigger the initial backup job, set the schedule when backup snapshots are taken.</span></span> <span data-ttu-id="d5025-168">Schemat som definierar när ögonblicksbilder av säkerhetskopior tas och hur länge dessa ögonblicksbilder bevaras utgör säkerhetskopieringspolicyn.</span><span class="sxs-lookup"><span data-stu-id="d5025-168">The schedule when backup snapshots are taken, and the length of time those snapshots are retained, is the backup policy.</span></span> <span data-ttu-id="d5025-169">Bevarandeinformationen baseras på ett säkerhetsrotationsschema av typen ”farfar-far-son”.</span><span class="sxs-lookup"><span data-stu-id="d5025-169">The retention information is based on Grandfather-father-son backup rotation scheme.</span></span>

1. <span data-ttu-id="d5025-170">Gå till säkerhetskopieringsvalvet under **Recovery Services** på den klassiska Azure-portalen och klicka på **Registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="d5025-170">Navigate to the backup vault under **Recovery Services** in the Azure Classic portal, and  click **Registered Items**.</span></span>
2. <span data-ttu-id="d5025-171">Välj **Virtuell Azure-dator** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="d5025-171">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Välja arbetsbelastning på portalen](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="d5025-173">Klicka på **Skydda** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="d5025-173">Click **PROTECT** at the bottom of the page.</span></span>
    <span data-ttu-id="d5025-174">![Klicka på Skydda](./media/backup-azure-vms-first-look/protect-icon.png)</span><span class="sxs-lookup"><span data-stu-id="d5025-174">![Click Protect](./media/backup-azure-vms-first-look/protect-icon.png)</span></span>

    <span data-ttu-id="d5025-175">Guiden **Skydda objekt** visas och visar *endast* virtuella datorer som är registrerade och som inte skyddas.</span><span class="sxs-lookup"><span data-stu-id="d5025-175">The **Protect Items wizard** appears and lists *only* virtual machines that are registered and not protected.</span></span>

    ![Konfigurera skydd i stor skala](./media/backup-azure-vms/protect-at-scale.png)
4. <span data-ttu-id="d5025-177">Välj de virtuella datorer som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="d5025-177">Select the virtual machines that you want to protect.</span></span>

    <span data-ttu-id="d5025-178">Om det finns två eller flera virtuella datorer med samma namn använder du Cloud Services för att skilja mellan virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d5025-178">If there are two or more virtual machines with the same name, use the Cloud Service to distinguish between the virtual machines.</span></span>
5. <span data-ttu-id="d5025-179">På menyn **Konfigurera skydd** väljer du en befintlig princip eller skapar en ny princip för att skydda de virtuella datorer som du har identifierat.</span><span class="sxs-lookup"><span data-stu-id="d5025-179">On the **Configure protection** menu select an existing policy or create a new policy to protect the virtual machines that you identified.</span></span>

    <span data-ttu-id="d5025-180">Nya säkerhetskopieringsvalv har en standardprincip som är associerad med valvet.</span><span class="sxs-lookup"><span data-stu-id="d5025-180">New Backup vaults have a default policy associated with the vault.</span></span> <span data-ttu-id="d5025-181">Den här principen tar en daglig ögonblicksbild varje kväll som bevaras i 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="d5025-181">This policy takes a daily snapshot each evening, and the daily snapshot is retained for 30 days.</span></span> <span data-ttu-id="d5025-182">Flera virtuella datorer kan vara associerade med varje säkerhetskopieringspolicy.</span><span class="sxs-lookup"><span data-stu-id="d5025-182">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="d5025-183">Men den virtuella datorn kan bara associeras med en princip i taget.</span><span class="sxs-lookup"><span data-stu-id="d5025-183">However, the virtual machine can only be associated with one policy at a time.</span></span>

    ![Skydda med ny princip](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="d5025-185">En säkerhetskopieringspolicy innehåller ett kvarhållningsschema för de schemalagda säkerhetskopieringarna.</span><span class="sxs-lookup"><span data-stu-id="d5025-185">A backup policy includes a retention scheme for the scheduled backups.</span></span> <span data-ttu-id="d5025-186">Om du väljer en befintlig säkerhetskopieringspolicy kan du inte ändra kvarhållningsalternativen i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="d5025-186">If you select an existing backup policy, you will be unable to modify the retention options in the next step.</span></span>
   >
   >
6. <span data-ttu-id="d5025-187">I **Kvarhållningsintervall** definierar du det dagliga, veckovisa, månatliga och årliga omfånget för de specifika säkerhetskopieringspunkterna.</span><span class="sxs-lookup"><span data-stu-id="d5025-187">On **Retention Range** define the daily, weekly, monthly, and yearly scope for the specific backup points.</span></span>

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="d5025-189">Bevarandeprincipen anger hur länge en säkerhetskopia ska lagras.</span><span class="sxs-lookup"><span data-stu-id="d5025-189">Retention policy specifies the length of time for storing a backup.</span></span> <span data-ttu-id="d5025-190">Du kan ange andra bevarandeprinciper baserat på när säkerhetskopieringen görs.</span><span class="sxs-lookup"><span data-stu-id="d5025-190">You can specify different retention policies based on when the backup is taken.</span></span>
7. <span data-ttu-id="d5025-191">Klicka på **Jobb** för att visa en lista över jobb av typen **Konfigurera skydd**.</span><span class="sxs-lookup"><span data-stu-id="d5025-191">Click **Jobs** to view the list of **Configure Protection** jobs.</span></span>

    ![Jobbet Konfigurera skydd](./media/backup-azure-vms/protect-configureprotection.png)

    <span data-ttu-id="d5025-193">Nu när du har skapat principen går du till nästa steg och kör den första säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="d5025-193">Now that you've established the policy, go to the next step and run the initial backup.</span></span>

## <a name="initial-backup"></a><span data-ttu-id="d5025-194">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="d5025-194">Initial backup</span></span>
<span data-ttu-id="d5025-195">När en virtuell dator har skyddats med en princip kan du visa den relationen på fliken **Skyddade objekt**. Innan den första säkerhetskopieringen har körts visas **Skyddsstatus** som **Skyddade – (första säkerhetskopiering väntar)**.</span><span class="sxs-lookup"><span data-stu-id="d5025-195">Once a virtual machine has been protected with a policy, you can view that relationship on the **Protected Items** tab. Until the initial backup occurs, the **Protection Status** shows as **Protected - (pending initial backup)**.</span></span> <span data-ttu-id="d5025-196">Som standard är den första säkerhetskopieringen den *inledande säkerhetskopieringen*.</span><span class="sxs-lookup"><span data-stu-id="d5025-196">By default, the first scheduled backup is the *initial backup*.</span></span>

![Säkerhetskopiering väntar](./media/backup-azure-vms-first-look/protection-pending-border.png)

<span data-ttu-id="d5025-198">Så här startar du den första säkerhetskopieringen nu:</span><span class="sxs-lookup"><span data-stu-id="d5025-198">To start the initial backup now:</span></span>

1. <span data-ttu-id="d5025-199">På sidan **Skyddade objekt** klickar du på **Säkerhetskopiera nu** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="d5025-199">On the **Protected Items** page, click **Backup Now** at the bottom of the page.</span></span>
    <span data-ttu-id="d5025-200">![Ikonen Säkerhetskopiera nu](./media/backup-azure-vms-first-look/backup-now-icon.png)</span><span class="sxs-lookup"><span data-stu-id="d5025-200">![Backup Now icon](./media/backup-azure-vms-first-look/backup-now-icon.png)</span></span>

    <span data-ttu-id="d5025-201">Tjänsten Azure Backup skapar ett säkerhetskopieringsjobb för den första säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="d5025-201">The Azure Backup service creates a backup job for the initial backup operation.</span></span>
2. <span data-ttu-id="d5025-202">Klicka på fliken **Jobb** för att visa listan över jobb.</span><span class="sxs-lookup"><span data-stu-id="d5025-202">Click the **Jobs** tab to view the list of jobs.</span></span>

    ![Säkerhetskopiering pågår](./media/backup-azure-vms-first-look/protect-inprogress.png)

    <span data-ttu-id="d5025-204">När den första säkerhetskopieringen är klar ändras statusen för den virtuella datorn på fliken **Skyddade objekt** till *Skyddade*.</span><span class="sxs-lookup"><span data-stu-id="d5025-204">When initial backup is complete, the status of the virtual machine in the **Protected Items** tab is *Protected*.</span></span>

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > <span data-ttu-id="d5025-206">Säkerhetskopieringen av virtuella datorer är en lokal process.</span><span class="sxs-lookup"><span data-stu-id="d5025-206">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="d5025-207">Du kan inte säkerhetskopiera virtuella datorer från en region till ett säkerhetskopieringsvalv i en annan region.</span><span class="sxs-lookup"><span data-stu-id="d5025-207">You cannot back up virtual machines from one region to a backup vault in another region.</span></span> <span data-ttu-id="d5025-208">Så, för varje Azure-region som innehåller virtuella datorer som behöver säkerhetskopieras måste minst ett säkerhetskopieringsvalv skapas i den regionen.</span><span class="sxs-lookup"><span data-stu-id="d5025-208">So, for every Azure region that has VMs that need to be backed up, at least one backup vault must be created in that region.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="d5025-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d5025-209">Next steps</span></span>
<span data-ttu-id="d5025-210">Nu när du har säkerhetskopierat en virtuell dator kan du fortsätta med flera andra intressanta steg.</span><span class="sxs-lookup"><span data-stu-id="d5025-210">Now that you have successfully backed up a VM, there are several next steps that could be of interest.</span></span> <span data-ttu-id="d5025-211">Det mest logiska steget är att lära dig hur du återställer data till en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="d5025-211">The most logical step is to familiarize yourself with restoring data to a VM.</span></span> <span data-ttu-id="d5025-212">Det finns dock hanteringsaktiviteter som hjälper dig att förstå hur du skyddar dina data och minimerar kostnaderna.</span><span class="sxs-lookup"><span data-stu-id="d5025-212">However, there are management tasks that will help you understand how to keep your data safe and minimize costs.</span></span>

* [<span data-ttu-id="d5025-213">Hantera och övervaka dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d5025-213">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="d5025-214">Återställa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d5025-214">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
* [<span data-ttu-id="d5025-215">Felsökningsanvisningar</span><span class="sxs-lookup"><span data-stu-id="d5025-215">Troubleshooting guidance</span></span>](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a><span data-ttu-id="d5025-216">Frågor?</span><span class="sxs-lookup"><span data-stu-id="d5025-216">Questions?</span></span>
<span data-ttu-id="d5025-217">Om du har frågor eller om du saknar en funktion är du välkommen att [lämna feedback](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="d5025-217">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>
