---
title: "En första titt: Säkerhetskopiera virtuella datorer i Azure med ett säkerhetskopieringsvalv | Microsoft Docs"
description: "Använd hello klassiska portal tooback upp Azure Virtual Machines tooa Backup-valvet. Den här självstudiekursen beskriver alla faser, inklusive skapa hello Backup-valvet, registrera hello virtuella datorer, skapa princip för säkerhetskopiering och kör hello inledande säkerhetskopieringsjobb."
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
ms.openlocfilehash: 77700e69eab9faccbc7ef923e1eb4e1f97be75d7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-backing-up-azure-virtual-machines"></a><span data-ttu-id="c628e-104">En första titt: Säkerhetskopiera virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="c628e-104">First look: Backing up Azure virtual machines</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c628e-105">Skydda virtuella datorer med ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="c628e-105">Protect VMs with a recovery services vault</span></span>](backup-azure-vms-first-look-arm.md)
> * [<span data-ttu-id="c628e-106">Skydda virtuella datorer i Azure med ett Backup-valv</span><span class="sxs-lookup"><span data-stu-id="c628e-106">Protect Azure VMs with a backup vault</span></span>](backup-azure-vms-first-look.md)
>
>

<span data-ttu-id="c628e-107">Den här kursen tar dig igenom hello steg för att säkerhetskopiera en virtuell Azure-dator (VM) tooa säkerhetskopieringsvalvet i Azure.</span><span class="sxs-lookup"><span data-stu-id="c628e-107">This tutorial takes you through hello steps for backing up an Azure virtual machine (VM) tooa backup vault in Azure.</span></span> <span data-ttu-id="c628e-108">Den här artikeln beskriver hello klassiska modellen eller Service Manager-distributionsmodellen, för att säkerhetskopiera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c628e-108">This article describes hello classic model or Service Manager deployment model, for backing up VMs.</span></span> <span data-ttu-id="c628e-109">hello gäller följande steg endast tooBackup valv som skapats i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="c628e-109">hello following steps apply only tooBackup vaults created in hello classic portal.</span></span> <span data-ttu-id="c628e-110">Microsoft rekommenderar att du använder hello Resource Manager-modellen för nya distributioner.</span><span class="sxs-lookup"><span data-stu-id="c628e-110">Microsoft recommends using hello Resource Manager model for new deployments.</span></span>

<span data-ttu-id="c628e-111">Om du vill säkerhetskopiera en VM tooa Recovery Services-valv som tillhör tooa resursgruppen finns [förhandstitt: skydda virtuella datorer med en återställningstjänstvalvet](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="c628e-111">If you are interested in backing up a VM tooa Recovery Services vault that belongs tooa Resource Group, see [First look: Protect VMs with a recovery services vault](backup-azure-vms-first-look-arm.md).</span></span>

<span data-ttu-id="c628e-112">toosuccessfully slutföra hello följande självstudier, dessa förutsättningar måste finnas:</span><span class="sxs-lookup"><span data-stu-id="c628e-112">toosuccessfully complete hello following tutorial, these prerequisites must exist:</span></span>

* <span data-ttu-id="c628e-113">Du har skapat en virtuell dator i Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="c628e-113">You have created a VM in your Azure subscription.</span></span>
* <span data-ttu-id="c628e-114">hello VM har anslutning tooAzure offentliga IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="c628e-114">hello VM has connectivity tooAzure public IP addresses.</span></span> <span data-ttu-id="c628e-115">Mer information finns i [Nätverksanslutningar](backup-azure-vms-prepare.md#network-connectivity).</span><span class="sxs-lookup"><span data-stu-id="c628e-115">For additional information, see [Network connectivity](backup-azure-vms-prepare.md#network-connectivity).</span></span>


> [!NOTE]
> <span data-ttu-id="c628e-116">Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="c628e-116">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="c628e-117">Den här kursen ska användas med virtuella datorer som skapats i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="c628e-117">This tutorial is for use with virtual machines created in hello classic portal.</span></span>
>
>

## <a name="create-a-backup-vault"></a><span data-ttu-id="c628e-118">Skapa ett säkerhetskopieringsvalv</span><span class="sxs-lookup"><span data-stu-id="c628e-118">Create a backup vault</span></span>
<span data-ttu-id="c628e-119">Ett säkerhetskopieringsvalv är en entitet som lagrar alla hello säkerhetskopieringar och återställningspunkter som har skapats med tiden.</span><span class="sxs-lookup"><span data-stu-id="c628e-119">A backup vault is an entity that stores all hello backups and recovery points that have been created over time.</span></span> <span data-ttu-id="c628e-120">Hej säkerhetskopieringsvalvet innehåller också hello säkerhetskopieringsprinciper som är kopplade toohello virtuella datorer säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="c628e-120">hello backup vault also contains hello backup policies that are applied toohello virtual machines being backed up.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="c628e-121">Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="c628e-121">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
> <span data-ttu-id="c628e-122">Du kan uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="c628e-122">You can upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="c628e-123">Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="c628e-123">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="c628e-124">Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="c628e-124">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="c628e-125">Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017.</span><span class="sxs-lookup"><span data-stu-id="c628e-125">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="c628e-126">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="c628e-126">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="c628e-127">Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="c628e-127">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="c628e-128">Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="c628e-128">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="c628e-129">Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="c628e-129">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="discover-and-register-azure-virtual-machines"></a><span data-ttu-id="c628e-130">Identifiera och registrera virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="c628e-130">Discover and Register Azure virtual machines</span></span>
<span data-ttu-id="c628e-131">Innan du registrerar hello VM med ett valv, kör du hello identifiering processen tooidentify eventuella nya virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c628e-131">Before registering hello VM with a vault, run hello discovery process tooidentify any new VMs.</span></span> <span data-ttu-id="c628e-132">Detta returnerar en lista över virtuella datorer i hello-prenumeration, tillsammans med ytterligare information som hello molntjänstnamnet och hello region.</span><span class="sxs-lookup"><span data-stu-id="c628e-132">This returns a list of virtual machines in hello subscription, along with additional information like hello cloud service name and hello region.</span></span>

1. <span data-ttu-id="c628e-133">Logga in toohello [klassiska Azure-portalen](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="c628e-133">Sign in toohello [Azure Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="c628e-134">I hello klassiska Azure-portalen klickar du på **återställningstjänster** tooopen hello lista över Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="c628e-134">In hello Azure classic portal, click **Recovery Services** tooopen hello list of Recovery Services vaults.</span></span>
    <span data-ttu-id="c628e-135">![Välja arbetsbelastning](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span><span class="sxs-lookup"><span data-stu-id="c628e-135">![Select workload](./media/backup-azure-vms-first-look/recovery-services-icon.png)</span></span>
3. <span data-ttu-id="c628e-136">Hello lista över valv, Välj hello valvet tooback upp en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c628e-136">From hello list of vaults, select hello vault tooback up a VM.</span></span>

    <span data-ttu-id="c628e-137">När du väljer ditt valv öppnas i hello **Snabbstart** sidan</span><span class="sxs-lookup"><span data-stu-id="c628e-137">When you select your vault, it opens in hello **Quick Start** page</span></span>
4. <span data-ttu-id="c628e-138">Hello valvet-menyn, klickar du på **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="c628e-138">From hello vault menu, click **Registered Items**.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms-first-look/configure-registered-items.png)
5. <span data-ttu-id="c628e-140">Från hello **typen** väljer du **Azure virtuella**.</span><span class="sxs-lookup"><span data-stu-id="c628e-140">From hello **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="c628e-142">Klicka på **identifiera** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="c628e-142">Click **DISCOVER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="c628e-143">![Knappen Identifiera](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="c628e-143">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="c628e-144">hello identifieringen kan ta några minuter innan hello virtuella datorer visas som som en tabell.</span><span class="sxs-lookup"><span data-stu-id="c628e-144">hello discovery process may take a few minutes while hello virtual machines are being tabulated.</span></span> <span data-ttu-id="c628e-145">Det finns ett meddelande längst ned hello hello-skärmen där du vet att hello processen körs.</span><span class="sxs-lookup"><span data-stu-id="c628e-145">There is a notification at hello bottom of hello screen that lets you know that hello process is running.</span></span>

    ![Identifiera virtuella datorer](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="c628e-147">hello-meddelande ändras när hello processen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="c628e-147">hello notification changes when hello process is complete.</span></span>

    ![Identifieringen är klar](./media/backup-azure-vms-first-look/discovery-complete.png)
7. <span data-ttu-id="c628e-149">Klicka på **registrera** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="c628e-149">Click **REGISTER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="c628e-150">![Knappen Registrera](./media/backup-azure-vms-first-look/register-icon.png)</span><span class="sxs-lookup"><span data-stu-id="c628e-150">![Register button](./media/backup-azure-vms-first-look/register-icon.png)</span></span>
8. <span data-ttu-id="c628e-151">I hello **registrera objekt** snabbmenyn, Välj hello virtuella datorer som du vill tooregister.</span><span class="sxs-lookup"><span data-stu-id="c628e-151">In hello **Register Items** shortcut menu, select hello virtual machines that you want tooregister.</span></span>

   > [!TIP]
   > <span data-ttu-id="c628e-152">Flera virtuella datorer kan registreras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="c628e-152">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="c628e-153">Ett jobb skapas för varje virtuell dator som du har valt.</span><span class="sxs-lookup"><span data-stu-id="c628e-153">A job is created for each virtual machine that you've selected.</span></span>
9. <span data-ttu-id="c628e-154">Klicka på **visa jobb** i hello meddelande toogo toohello **jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="c628e-154">Click **View Job** in hello notification toogo toohello **Jobs** page.</span></span>

    ![Registrera jobb](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="c628e-156">hello virtuell dator visas också i hello lista över registrerade artiklar, tillsammans med hello status för hello registrering åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c628e-156">hello virtual machine also appears in hello list of registered items, along with hello status of hello registration operation.</span></span>

    ![Registreringsstatus 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="c628e-158">När hello har slutförts, hello status ändras tooreflect hello *registrerade* tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c628e-158">When hello operation completes, hello status changes tooreflect hello *registered* state.</span></span>

    ![Registreringsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="install-hello-vm-agent-on-hello-virtual-machine"></a><span data-ttu-id="c628e-160">Installera hello VM-agenten på hello virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="c628e-160">Install hello VM Agent on hello virtual machine</span></span>
<span data-ttu-id="c628e-161">hello Azure VM-agenten måste installeras på hello Azure virtuell dator för hello säkerhetskopiering tillägget toowork.</span><span class="sxs-lookup"><span data-stu-id="c628e-161">hello Azure VM Agent must be installed on hello Azure virtual machine for hello Backup extension toowork.</span></span> <span data-ttu-id="c628e-162">Om den virtuella datorn skapades från hello Azure-galleriet finns hello VM-agenten redan på hello VM; Du kan hoppa över för[skyddar dina virtuella datorer](backup-azure-vms-first-look.md#create-the-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="c628e-162">If your VM was created from hello Azure gallery, hello VM Agent is already present on hello VM; you can skip too[protecting your VMs](backup-azure-vms-first-look.md#create-the-backup-policy).</span></span>

<span data-ttu-id="c628e-163">Om din virtuella dator migreras från ett lokalt datacenter, hello VM förmodligen inte hello VM-agenten installerad.</span><span class="sxs-lookup"><span data-stu-id="c628e-163">If your VM migrated from an on-premises datacenter, hello VM probably does not have hello VM Agent installed.</span></span> <span data-ttu-id="c628e-164">Du måste installera hello VM-agenten på hello virtuella datorn innan du fortsätter tooprotect hello VM.</span><span class="sxs-lookup"><span data-stu-id="c628e-164">You must install hello VM Agent on hello virtual machine before proceeding tooprotect hello VM.</span></span> <span data-ttu-id="c628e-165">Detaljerade anvisningar om hur du installerar hello VM-Agent, finns hello [VM-agenten avsnitt i hello säkerhetskopiering VMs artikel](backup-azure-vms-prepare.md#vm-agent).</span><span class="sxs-lookup"><span data-stu-id="c628e-165">For detailed steps on installing hello VM Agent, see hello [VM Agent section of hello Backup VMs article](backup-azure-vms-prepare.md#vm-agent).</span></span>

## <a name="create-hello-backup-policy"></a><span data-ttu-id="c628e-166">Skapa hello säkerhetskopieringsprincip</span><span class="sxs-lookup"><span data-stu-id="c628e-166">Create hello backup policy</span></span>
<span data-ttu-id="c628e-167">Innan du utlösa hello inledande säkerhetskopieringsjobbet Schemalägg hello när ögonblicksbilder av säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="c628e-167">Before you trigger hello initial backup job, set hello schedule when backup snapshots are taken.</span></span> <span data-ttu-id="c628e-168">hello schemalägga när ögonblicksbilder av säkerhetskopior tas och hello tid dessa ögonblicksbilder är är behålls hello säkerhetskopieringsprincip.</span><span class="sxs-lookup"><span data-stu-id="c628e-168">hello schedule when backup snapshots are taken, and hello length of time those snapshots are retained, is hello backup policy.</span></span> <span data-ttu-id="c628e-169">hello kvarhållning informationen baseras på farfar-pappa-son säkerhetskopiering rotation schemat.</span><span class="sxs-lookup"><span data-stu-id="c628e-169">hello retention information is based on Grandfather-father-son backup rotation scheme.</span></span>

1. <span data-ttu-id="c628e-170">Navigera toohello säkerhetskopieringsvalv under **återställningstjänster** i hello Azure klassiska portal och klicka på **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="c628e-170">Navigate toohello backup vault under **Recovery Services** in hello Azure Classic portal, and  click **Registered Items**.</span></span>
2. <span data-ttu-id="c628e-171">Välj **Azure virtuella** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="c628e-171">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Välja arbetsbelastning på portalen](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="c628e-173">Klicka på **skydda** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="c628e-173">Click **PROTECT** at hello bottom of hello page.</span></span>
    <span data-ttu-id="c628e-174">![Klicka på Skydda](./media/backup-azure-vms-first-look/protect-icon.png)</span><span class="sxs-lookup"><span data-stu-id="c628e-174">![Click Protect](./media/backup-azure-vms-first-look/protect-icon.png)</span></span>

    <span data-ttu-id="c628e-175">Hej **skydda objekt guiden** visas och anger *endast* virtuella datorer som registrerats och inte skyddas.</span><span class="sxs-lookup"><span data-stu-id="c628e-175">hello **Protect Items wizard** appears and lists *only* virtual machines that are registered and not protected.</span></span>

    ![Konfigurera skydd i stor skala](./media/backup-azure-vms/protect-at-scale.png)
4. <span data-ttu-id="c628e-177">Välj hello virtuella datorer som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="c628e-177">Select hello virtual machines that you want tooprotect.</span></span>

    <span data-ttu-id="c628e-178">Om det finns två eller flera virtuella datorer med hello namn samma, Använd hello Molntjänsten toodistinguish mellan hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c628e-178">If there are two or more virtual machines with hello same name, use hello Cloud Service toodistinguish between hello virtual machines.</span></span>
5. <span data-ttu-id="c628e-179">På hello **Konfigurera skydd** -menyn väljer du en befintlig princip eller skapa en ny princip tooprotect hello virtuella datorer som du identifierade.</span><span class="sxs-lookup"><span data-stu-id="c628e-179">On hello **Configure protection** menu select an existing policy or create a new policy tooprotect hello virtual machines that you identified.</span></span>

    <span data-ttu-id="c628e-180">Nytt säkerhetskopieringsvalv har en standardprincip som är associerade med hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="c628e-180">New Backup vaults have a default policy associated with hello vault.</span></span> <span data-ttu-id="c628e-181">Den här principen tar en ögonblicksbild av varje kväll daglig och hello ögonblicksbild finns kvar i 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="c628e-181">This policy takes a daily snapshot each evening, and hello daily snapshot is retained for 30 days.</span></span> <span data-ttu-id="c628e-182">Flera virtuella datorer kan vara associerade med varje säkerhetskopieringspolicy.</span><span class="sxs-lookup"><span data-stu-id="c628e-182">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="c628e-183">Dock kan hello virtuell dator bara vara kopplad till en princip i taget.</span><span class="sxs-lookup"><span data-stu-id="c628e-183">However, hello virtual machine can only be associated with one policy at a time.</span></span>

    ![Skydda med ny princip](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="c628e-185">En princip för säkerhetskopiering finns ett kvarhållning system för hello schemalagda säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="c628e-185">A backup policy includes a retention scheme for hello scheduled backups.</span></span> <span data-ttu-id="c628e-186">Om du väljer en säkerhetskopieringsprincip blir toomodify hello kvarhållning alternativ i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="c628e-186">If you select an existing backup policy, you will be unable toomodify hello retention options in hello next step.</span></span>
   >
   >
6. <span data-ttu-id="c628e-187">På **Kvarhållningsintervall** definiera hello dagliga, veckovisa, månatliga och årliga omfång för hello specifika säkerhetskopiering punkter.</span><span class="sxs-lookup"><span data-stu-id="c628e-187">On **Retention Range** define hello daily, weekly, monthly, and yearly scope for hello specific backup points.</span></span>

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="c628e-189">Bevarandeprincip anger hello lång tid för att lagra en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="c628e-189">Retention policy specifies hello length of time for storing a backup.</span></span> <span data-ttu-id="c628e-190">Du kan ange olika bevarandeprinciper baserat på när hello säkerhetskopia görs.</span><span class="sxs-lookup"><span data-stu-id="c628e-190">You can specify different retention policies based on when hello backup is taken.</span></span>
7. <span data-ttu-id="c628e-191">Klicka på **jobb** tooview hello lista över **Konfigurera skydd** jobb.</span><span class="sxs-lookup"><span data-stu-id="c628e-191">Click **Jobs** tooview hello list of **Configure Protection** jobs.</span></span>

    ![Jobbet Konfigurera skydd](./media/backup-azure-vms/protect-configureprotection.png)

    <span data-ttu-id="c628e-193">Nu när du har skapat hello princip gå toohello nästa steg och kör hello första säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="c628e-193">Now that you've established hello policy, go toohello next step and run hello initial backup.</span></span>

## <a name="initial-backup"></a><span data-ttu-id="c628e-194">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="c628e-194">Initial backup</span></span>
<span data-ttu-id="c628e-195">När en virtuell dator har skyddats med en princip kan du visa relationen på hello **skyddade objekt** fliken. Tills hello första säkerhetskopian inträffar hello **skyddsstatus** visas som **skyddade - (väntar på inledande säkerhetskopia)**.</span><span class="sxs-lookup"><span data-stu-id="c628e-195">Once a virtual machine has been protected with a policy, you can view that relationship on hello **Protected Items** tab. Until hello initial backup occurs, hello **Protection Status** shows as **Protected - (pending initial backup)**.</span></span> <span data-ttu-id="c628e-196">Som standard är hello första schemalagd säkerhetskopiering hello *första säkerhetskopian*.</span><span class="sxs-lookup"><span data-stu-id="c628e-196">By default, hello first scheduled backup is hello *initial backup*.</span></span>

![Säkerhetskopiering väntar](./media/backup-azure-vms-first-look/protection-pending-border.png)

<span data-ttu-id="c628e-198">toostart hello första säkerhetskopian nu:</span><span class="sxs-lookup"><span data-stu-id="c628e-198">toostart hello initial backup now:</span></span>

1. <span data-ttu-id="c628e-199">På hello **skyddade objekt** klickar du på **säkerhetskopiering nu** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="c628e-199">On hello **Protected Items** page, click **Backup Now** at hello bottom of hello page.</span></span>
    <span data-ttu-id="c628e-200">![Ikonen Säkerhetskopiera nu](./media/backup-azure-vms-first-look/backup-now-icon.png)</span><span class="sxs-lookup"><span data-stu-id="c628e-200">![Backup Now icon](./media/backup-azure-vms-first-look/backup-now-icon.png)</span></span>

    <span data-ttu-id="c628e-201">hello Azure Backup-tjänsten skapar ett säkerhetskopieringsjobb för hello första säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="c628e-201">hello Azure Backup service creates a backup job for hello initial backup operation.</span></span>
2. <span data-ttu-id="c628e-202">Klicka på hello **jobb** fliken tooview hello lista över jobb.</span><span class="sxs-lookup"><span data-stu-id="c628e-202">Click hello **Jobs** tab tooview hello list of jobs.</span></span>

    ![Säkerhetskopiering pågår](./media/backup-azure-vms-first-look/protect-inprogress.png)

    <span data-ttu-id="c628e-204">När den första säkerhetskopieringen är klar, hello status för hello virtuell dator i hello **skyddade objekt** är *skyddade*.</span><span class="sxs-lookup"><span data-stu-id="c628e-204">When initial backup is complete, hello status of hello virtual machine in hello **Protected Items** tab is *Protected*.</span></span>

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/protect-backedupvm.png)

   > [!NOTE]
   > <span data-ttu-id="c628e-206">Säkerhetskopieringen av virtuella datorer är en lokal process.</span><span class="sxs-lookup"><span data-stu-id="c628e-206">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="c628e-207">Du kan säkerhetskopiera virtuella datorer från en region tooa säkerhetskopieringsvalv i en annan region.</span><span class="sxs-lookup"><span data-stu-id="c628e-207">You cannot back up virtual machines from one region tooa backup vault in another region.</span></span> <span data-ttu-id="c628e-208">För varje Azure-region som har virtuella datorer som behöver säkerhetskopieras toobe, måste därför minst ett säkerhetskopieringsvalv skapas i regionen.</span><span class="sxs-lookup"><span data-stu-id="c628e-208">So, for every Azure region that has VMs that need toobe backed up, at least one backup vault must be created in that region.</span></span>
   >
   >

## <a name="next-steps"></a><span data-ttu-id="c628e-209">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c628e-209">Next steps</span></span>
<span data-ttu-id="c628e-210">Nu när du har säkerhetskopierat en virtuell dator kan du fortsätta med flera andra intressanta steg.</span><span class="sxs-lookup"><span data-stu-id="c628e-210">Now that you have successfully backed up a VM, there are several next steps that could be of interest.</span></span> <span data-ttu-id="c628e-211">hello mest logiska steg är toofamiliarize dig att återställa data tooa VM.</span><span class="sxs-lookup"><span data-stu-id="c628e-211">hello most logical step is toofamiliarize yourself with restoring data tooa VM.</span></span> <span data-ttu-id="c628e-212">Det finns dock hanteringsaktiviteter som hjälper dig att förstå hur tookeep data säkert och minska kostnaderna.</span><span class="sxs-lookup"><span data-stu-id="c628e-212">However, there are management tasks that will help you understand how tookeep your data safe and minimize costs.</span></span>

* [<span data-ttu-id="c628e-213">Hantera och övervaka dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c628e-213">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="c628e-214">Återställa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c628e-214">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
* [<span data-ttu-id="c628e-215">Felsökningsanvisningar</span><span class="sxs-lookup"><span data-stu-id="c628e-215">Troubleshooting guidance</span></span>](backup-azure-vms-troubleshoot.md)

## <a name="questions"></a><span data-ttu-id="c628e-216">Frågor?</span><span class="sxs-lookup"><span data-stu-id="c628e-216">Questions?</span></span>
<span data-ttu-id="c628e-217">Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="c628e-217">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>
