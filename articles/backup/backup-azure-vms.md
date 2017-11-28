---
title: "aaaBack in klassiska distribuerade virtuella Azure-datorer tooa säkerhetskopieringsvalvet | Microsoft Docs"
description: "Identifiera, registrera och säkerhetskopiera virtuella datorer med de här procedurerna för virtuell dator i Azure-säkerhetskopiering."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopiering av virtuella datorer Säkerhetskopiera virtuella; säkerhetskopiering och katastrofåterställning återställning. säkerhetskopiering"
ms.assetid: c0ab5469-65fd-4af5-ae9b-f5d183f82ce8
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/02/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: 048e32d9b2bd5bdd7a125225a71a6d805bb4fbd4
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a><span data-ttu-id="85be5-104">Säkerhetskopiera virtuella Azure-datorer (klassiska portal)</span><span class="sxs-lookup"><span data-stu-id="85be5-104">Back up Azure virtual machines (classic portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="85be5-105">Säkerhetskopiera virtuella datorer tooRecovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="85be5-105">Back up VMs tooRecovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="85be5-106">Säkerhetskopiera virtuella datorer tooBackup valvet</span><span class="sxs-lookup"><span data-stu-id="85be5-106">Back up VMs tooBackup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="85be5-107">Den här artikeln innehåller hello procedurer för att säkerhetskopiera en distribuerade klassisk Azure-dator (VM) tooa Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="85be5-107">This article provides hello procedures for backing up a Classic-deployed Azure virtual machine (VM) tooa Backup vault.</span></span> <span data-ttu-id="85be5-108">Det finns några uppgifter som du måste tootake care av innan du kan säkerhetskopiera en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="85be5-108">There are a few tasks you need tootake care of before you can back up an Azure virtual machine.</span></span> <span data-ttu-id="85be5-109">Om du inte redan gjort, fullständig hello [krav](backup-azure-vms-prepare.md) tooprepare din miljö för att säkerhetskopiera dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="85be5-109">If you haven't already done so, complete hello [prerequisites](backup-azure-vms-prepare.md) tooprepare your environment for backing up your VMs.</span></span>

<span data-ttu-id="85be5-110">Mer information finns i hello artiklar på [planerar din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md) och [virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="85be5-110">For additional information, see hello articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

> [!NOTE]
> <span data-ttu-id="85be5-111">Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="85be5-111">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="85be5-112">Ett säkerhetskopieringsvalv kan bara skydda klassisk distribuerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="85be5-112">A Backup vault can only protect Classic-deployed VMs.</span></span> <span data-ttu-id="85be5-113">Du kan inte skydda Hanteraren för filserverresurser distribuerade virtuella datorer med en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="85be5-113">You cannot protect Resource Manager-deployed VMs with a Backup vault.</span></span> <span data-ttu-id="85be5-114">Se [säkerhetskopiera virtuella datorer tooRecovery Services-valvet](backup-azure-arm-vms.md) mer information om hur du arbetar med Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="85be5-114">See [Back up VMs tooRecovery Services vault](backup-azure-arm-vms.md) for details on working with Recovery Services vaults.</span></span>
>
>

<span data-ttu-id="85be5-115">Säkerhetskopiering av virtuella Azure-datorer omfattar tre viktiga steg:</span><span class="sxs-lookup"><span data-stu-id="85be5-115">Backing up Azure virtual machines involves three key steps:</span></span>

![Tre steg tooback upp en Azure IaaS-VM](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> <span data-ttu-id="85be5-117">Säkerhetskopieringen av virtuella datorer är en lokal process.</span><span class="sxs-lookup"><span data-stu-id="85be5-117">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="85be5-118">Du kan säkerhetskopiera virtuella datorer i en region tooa säkerhetskopieringsvalvet i en annan region.</span><span class="sxs-lookup"><span data-stu-id="85be5-118">You cannot back up virtual machines in one region tooa backup vault in another region.</span></span> <span data-ttu-id="85be5-119">Så du måste skapa ett säkerhetskopieringsvalv i varje Azure-region där det finns virtuella datorer som ska säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="85be5-119">So, you must create a backup vault in each Azure region, where there are VMs that will be backed up.</span></span>
>
> [!IMPORTANT]
> <span data-ttu-id="85be5-120">Från mars 2017 kan använda du inte längre hello klassiska portal toocreate säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="85be5-120">Starting March 2017, you can no longer use hello classic portal toocreate Backup vaults.</span></span>
> <span data-ttu-id="85be5-121">Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="85be5-121">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="85be5-122">Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="85be5-122">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="85be5-123">Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="85be5-123">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="85be5-124">Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017.</span><span class="sxs-lookup"><span data-stu-id="85be5-124">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="85be5-125">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="85be5-125">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="85be5-126">Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="85be5-126">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="85be5-127">Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="85be5-127">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="85be5-128">Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="85be5-128">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="step-1---discover-azure-virtual-machines"></a><span data-ttu-id="85be5-129">Steg 1 – identifiera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="85be5-129">Step 1 - Discover Azure virtual machines</span></span>
<span data-ttu-id="85be5-130">tooensure alla nya virtuella datorer (VM) tillagda toohello abonnemang identifieras innan du registrerar, kör hello identifieringsprocessen.</span><span class="sxs-lookup"><span data-stu-id="85be5-130">tooensure any new virtual machines (VMs) added toohello subscription are identified before registering, run hello discovery process.</span></span> <span data-ttu-id="85be5-131">hello begär Azure hello listan över virtuella datorer i hello-prenumeration, tillsammans med ytterligare information som hello molntjänstnamnet och hello region.</span><span class="sxs-lookup"><span data-stu-id="85be5-131">hello process queries Azure for hello list of virtual machines in hello subscription, along with additional information like hello cloud service name and hello region.</span></span>

1. <span data-ttu-id="85be5-132">Logga in toohello [klassisk portal](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="85be5-132">Sign in toohello [Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="85be5-133">Hello listan med Azure-tjänster, på **återställningstjänster** tooopen hello lista över säkerhetskopiering och Site Recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="85be5-133">In hello list of Azure services, click **Recovery Services** tooopen hello list of Backup and Site Recovery vaults.</span></span>
    <span data-ttu-id="85be5-134">![Öppna valvet lista](./media/backup-azure-vms/choose-vault-list.png)</span><span class="sxs-lookup"><span data-stu-id="85be5-134">![Open vault list](./media/backup-azure-vms/choose-vault-list.png)</span></span>
3. <span data-ttu-id="85be5-135">Välj hello valvet tooback upp en virtuell dator i hello lista av säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="85be5-135">In hello list of Backup vaults, select hello vault tooback up a VM.</span></span>

    <span data-ttu-id="85be5-136">Om det här är en ny valvet hello portal öppnar toohello **Snabbstart** sidan.</span><span class="sxs-lookup"><span data-stu-id="85be5-136">If this is a new vault hello portal opens toohello **Quick Start** page.</span></span>

    ![Öppna registrerade objekt-menyn](./media/backup-azure-vms/vault-quick-start.png)

    <span data-ttu-id="85be5-138">Om hello valvet tidigare har konfigurerats, öppnar hello portal toohello som senast har använt-menyn.</span><span class="sxs-lookup"><span data-stu-id="85be5-138">If hello vault has previously been configured, hello portal opens toohello most recently used menu.</span></span>
4. <span data-ttu-id="85be5-139">Hello valvet menyn (överst hello på hello sidan) och klicka på **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="85be5-139">From hello vault menu (at hello top of hello page), click **Registered Items**.</span></span>

    ![Öppna registrerade objekt-menyn](./media/backup-azure-vms/vault-menu.png)
5. <span data-ttu-id="85be5-141">Från hello **typen** väljer du **Azure virtuella**.</span><span class="sxs-lookup"><span data-stu-id="85be5-141">From hello **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="85be5-143">Klicka på **identifiera** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="85be5-143">Click **DISCOVER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="85be5-144">![Knappen Identifiera](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="85be5-144">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="85be5-145">hello identifieringen kan ta några minuter innan hello virtuella datorer visas som som en tabell.</span><span class="sxs-lookup"><span data-stu-id="85be5-145">hello discovery process may take a few minutes while hello virtual machines are being tabulated.</span></span> <span data-ttu-id="85be5-146">Det finns ett meddelande längst ned hello hello-skärmen där du vet att hello processen körs.</span><span class="sxs-lookup"><span data-stu-id="85be5-146">There is a notification at hello bottom of hello screen that lets you know that hello process is running.</span></span>

    ![Identifiera virtuella datorer](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="85be5-148">hello-meddelande ändras när hello processen har slutförts.</span><span class="sxs-lookup"><span data-stu-id="85be5-148">hello notification changes when hello process is complete.</span></span> <span data-ttu-id="85be5-149">Om hello identifieringsprocessen inte gick att hitta hello virtuella datorer, kontrollera först hello virtuella datorer finns.</span><span class="sxs-lookup"><span data-stu-id="85be5-149">If hello discovery process did not find hello virtual machines, first ensure hello VMs exist.</span></span> <span data-ttu-id="85be5-150">Om det finns virtuella datorer hello, kontrollera hello virtuella datorer är i hello samma region som hello säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="85be5-150">If hello VMs exist, ensure hello VMs are in hello same region as hello backup vault.</span></span> <span data-ttu-id="85be5-151">Om hello VMs finns och är i Hej samma region, kontrollera hello virtuella datorer inte är redan registrerad tooa säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="85be5-151">If hello VMs exist and are in hello same region, ensure hello VMs are not already registered tooa backup vault.</span></span> <span data-ttu-id="85be5-152">Om en virtuell dator är tilldelade tooa säkerhetskopieringsvalvet men det är inte tillgängliga toobe tilldelade tooother säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="85be5-152">If a VM is assigned tooa backup vault it is not available toobe assigned tooother backup vaults.</span></span>

    ![Identifieringen är klar](./media/backup-azure-vms/discovery-complete.png)

    <span data-ttu-id="85be5-154">När du har identifierat hello nya objekt, gå tooStep 2 och registrera dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="85be5-154">Once you have discovered hello new items, go tooStep 2 and register your VMs.</span></span>

## <a name="step-2---register-azure-virtual-machines"></a><span data-ttu-id="85be5-155">Steg 2 – registrera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="85be5-155">Step 2 - Register Azure virtual machines</span></span>
<span data-ttu-id="85be5-156">Registrerar du en virtuell dator i Azure-tooassociate med hello Azure Backup-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="85be5-156">You register an Azure virtual machine tooassociate it with hello Azure Backup service.</span></span> <span data-ttu-id="85be5-157">Detta inträffar vanligtvis en gång.</span><span class="sxs-lookup"><span data-stu-id="85be5-157">This is typically a one-time activity.</span></span>

1. <span data-ttu-id="85be5-158">Navigera toohello säkerhetskopieringsvalv under **återställningstjänster** i hello Azure-portalen och klicka sedan på **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="85be5-158">Navigate toohello backup vault under **Recovery Services** in hello Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="85be5-159">Välj **Azure virtuella** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="85be5-159">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
3. <span data-ttu-id="85be5-161">Klicka på **registrera** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="85be5-161">Click **REGISTER** at hello bottom of hello page.</span></span>
    <span data-ttu-id="85be5-162">![Knappen Registrera](./media/backup-azure-vms/register-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="85be5-162">![Register button](./media/backup-azure-vms/register-button-only.png)</span></span>
4. <span data-ttu-id="85be5-163">I hello **registrera objekt** snabbmenyn, Välj hello virtuella datorer som du vill tooregister.</span><span class="sxs-lookup"><span data-stu-id="85be5-163">In hello **Register Items** shortcut menu, select hello virtual machines that you want tooregister.</span></span> <span data-ttu-id="85be5-164">Om det finns två eller flera virtuella datorer med hello samma, Använd hello cloud service toodistinguish mellan dem.</span><span class="sxs-lookup"><span data-stu-id="85be5-164">If there are two or more virtual machines with hello same name, use hello cloud service toodistinguish between them.</span></span>

   > [!TIP]
   > <span data-ttu-id="85be5-165">Flera virtuella datorer kan registreras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="85be5-165">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="85be5-166">Ett jobb skapas för varje virtuell dator som du har valt.</span><span class="sxs-lookup"><span data-stu-id="85be5-166">A job is created for each virtual machine that you've selected.</span></span>
5. <span data-ttu-id="85be5-167">Klicka på **visa jobb** i hello meddelande toogo toohello **jobb** sidan.</span><span class="sxs-lookup"><span data-stu-id="85be5-167">Click **View Job** in hello notification toogo toohello **Jobs** page.</span></span>

    ![Registrera jobb](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="85be5-169">hello virtuell dator visas också i hello lista över registrerade artiklar, tillsammans med hello status för hello registrering åtgärd.</span><span class="sxs-lookup"><span data-stu-id="85be5-169">hello virtual machine also appears in hello list of registered items, along with hello status of hello registration operation.</span></span>

    ![Registreringsstatus 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="85be5-171">När hello har slutförts, hello status ändras tooreflect hello *registrerade* tillstånd.</span><span class="sxs-lookup"><span data-stu-id="85be5-171">When hello operation completes, hello status changes tooreflect hello *registered* state.</span></span>

    ![Registreringsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a><span data-ttu-id="85be5-173">Steg 3 – skydda virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="85be5-173">Step 3 - Protect Azure virtual machines</span></span>
<span data-ttu-id="85be5-174">Du kan nu konfigurera en princip för säkerhetskopiering och kvarhållning för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="85be5-174">Now you can set up a backup and retention policy for hello virtual machine.</span></span> <span data-ttu-id="85be5-175">Flera virtuella datorer kan skyddas med hjälp av en enda skydda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="85be5-175">Multiple virtual machines can be protected by using a single protect action.</span></span>

<span data-ttu-id="85be5-176">Azure-säkerhetskopieringsvalv skapas efter maj 2015 medföljer en standardprincip inbyggda hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="85be5-176">Azure Backup vaults created after May 2015 come with a default policy built into hello vault.</span></span> <span data-ttu-id="85be5-177">Den här standardprincipen innehåller en Standardkvarhållning av 30 dagar och ett schema för säkerhetskopiering av en gång dagligen.</span><span class="sxs-lookup"><span data-stu-id="85be5-177">This default policy comes with a default retention of 30 days and a once-daily backup schedule.</span></span>

1. <span data-ttu-id="85be5-178">Navigera toohello säkerhetskopieringsvalv under **återställningstjänster** i hello Azure-portalen och klicka sedan på **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="85be5-178">Navigate toohello backup vault under **Recovery Services** in hello Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="85be5-179">Välj **Azure virtuella** hello nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="85be5-179">Select **Azure Virtual Machine** from hello drop-down menu.</span></span>

    ![Välja arbetsbelastning på portalen](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="85be5-181">Klicka på **skydda** på hello hello sidans nederkant.</span><span class="sxs-lookup"><span data-stu-id="85be5-181">Click **PROTECT** at hello bottom of hello page.</span></span>

    <span data-ttu-id="85be5-182">Hej **skydda objekt guiden** visas.</span><span class="sxs-lookup"><span data-stu-id="85be5-182">hello **Protect Items wizard** appears.</span></span> <span data-ttu-id="85be5-183">hello guiden visar endast virtuella datorer som registrerats och inte skyddas.</span><span class="sxs-lookup"><span data-stu-id="85be5-183">hello wizard only lists virtual machines that are registered and not protected.</span></span> <span data-ttu-id="85be5-184">Välj hello virtuella datorer som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="85be5-184">Select hello virtual machines that you want tooprotect.</span></span>

    <span data-ttu-id="85be5-185">Om det finns två eller flera virtuella datorer med hello samma, Använd hello cloud service toodistinguish mellan hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="85be5-185">If there are two or more virtual machines with hello same name, use hello cloud service toodistinguish between hello virtual machines.</span></span>

   > [!TIP]
   > <span data-ttu-id="85be5-186">Du kan skydda flera virtuella datorer samtidigt.</span><span class="sxs-lookup"><span data-stu-id="85be5-186">You can protect multiple virtual machines at one time.</span></span>
   >
   >

    ![Konfigurera skydd i stor skala](./media/backup-azure-vms/protect-at-scale.png)

4. <span data-ttu-id="85be5-188">Välj en **Säkerhetskopieringsschemat** tooback in hello virtuella datorer som du har valt.</span><span class="sxs-lookup"><span data-stu-id="85be5-188">Choose a **backup schedule** tooback up hello virtual machines that you've selected.</span></span> <span data-ttu-id="85be5-189">Du kan välja från en befintlig uppsättning principer eller definiera en ny.</span><span class="sxs-lookup"><span data-stu-id="85be5-189">You can pick from an existing set of policies or define a new one.</span></span>

    <span data-ttu-id="85be5-190">Flera virtuella datorer kan vara associerade med varje säkerhetskopieringspolicy.</span><span class="sxs-lookup"><span data-stu-id="85be5-190">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="85be5-191">Dock kan hello virtuell dator bara vara kopplad till en princip vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="85be5-191">However, hello virtual machine can only be associated with one policy at any given point in time.</span></span>

    ![Skydda med ny princip](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="85be5-193">En princip för säkerhetskopiering finns ett kvarhållning system för hello schemalagda säkerhetskopieringar.</span><span class="sxs-lookup"><span data-stu-id="85be5-193">A backup policy includes a retention scheme for hello scheduled backups.</span></span> <span data-ttu-id="85be5-194">Om du väljer en säkerhetskopieringsprincip kan ändra du inte alternativ för kvarhållning av hello i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="85be5-194">If you select an existing backup policy, you cannot modify hello retention options in hello next step.</span></span>
   >
   >

5. <span data-ttu-id="85be5-195">Välj en **Kvarhållningsintervall** tooassociate med hello säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="85be5-195">Choose a **retention range** tooassociate with hello backups.</span></span>

    ![Skydda med flexibel kvarhållning](./media/backup-azure-vms/policy-retention.png)

    <span data-ttu-id="85be5-197">Bevarandeprincip anger hello lång tid för att lagra en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="85be5-197">Retention policy specifies hello length of time for storing a backup.</span></span> <span data-ttu-id="85be5-198">Du kan ange olika bevarandeprinciper baserat på när hello säkerhetskopia görs.</span><span class="sxs-lookup"><span data-stu-id="85be5-198">You can specify different retention policies based on when hello backup is taken.</span></span> <span data-ttu-id="85be5-199">Exempelvis kan en säkerhetskopieringspunkt tas dagligen (som fungerar som en operativa återställningspunkt) bevaras i 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="85be5-199">For example, a backup point taken daily (which serves as an operational recovery point) might be preserved for 90 days.</span></span> <span data-ttu-id="85be5-200">Jämförelse behöva en säkerhetskopieringspunkt tas hello slutet av varje kvartal (som audit) toobe bevaras för många månader eller år.</span><span class="sxs-lookup"><span data-stu-id="85be5-200">In comparison, a backup point taken at hello end of each quarter (for audit purposes) may need toobe preserved for many months or years.</span></span>

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="85be5-202">I det här exemplet avbildningen:</span><span class="sxs-lookup"><span data-stu-id="85be5-202">In this example image:</span></span>

   * <span data-ttu-id="85be5-203">**Dagliga bevarandeprincip**: säkerhetskopieringar tas dagligen sparas i 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="85be5-203">**Daily retention policy**: Backups taken daily are stored for 30 days.</span></span>
   * <span data-ttu-id="85be5-204">**Varje vecka bevarandeprincip**: säkerhetskopior som gjorts varje vecka på söndag bevaras i 104 veckor.</span><span class="sxs-lookup"><span data-stu-id="85be5-204">**Weekly retention policy**: Backups taken every week on Sunday are preserved for 104 weeks.</span></span>
   * <span data-ttu-id="85be5-205">**Månatliga bevarandeprincip**: säkerhetskopieringar som utförts hello sista söndagen i varje månad bevaras i 120 månader.</span><span class="sxs-lookup"><span data-stu-id="85be5-205">**Monthly retention policy**: Backups taken on hello last Sunday of each month are preserved for 120 months.</span></span>
   * <span data-ttu-id="85be5-206">**Årlig bevarandeprincip**: säkerhetskopieringar som utförts hello första söndagen i varje januari bevaras för 99 år.</span><span class="sxs-lookup"><span data-stu-id="85be5-206">**Yearly retention policy**: Backups taken on hello first Sunday of every January are preserved for 99 years.</span></span>

     <span data-ttu-id="85be5-207">Ett jobb skapas tooconfigure hello protection-principen och associera hello virtuella datorer toothat princip för varje virtuell dator som du har valt.</span><span class="sxs-lookup"><span data-stu-id="85be5-207">A job is created tooconfigure hello protection policy and associate hello virtual machines toothat policy for each virtual machine that you've selected.</span></span>
6. <span data-ttu-id="85be5-208">tooview hello lista över **Konfigurera skydd** jobb hello valv-menyn, klicka på **jobb** och välj **Konfigurera skydd** från hello **åtgärden**  filter.</span><span class="sxs-lookup"><span data-stu-id="85be5-208">tooview hello list of **Configure Protection** jobs, from hello vaults menu, click **Jobs** and select **Configure Protection** from hello **Operation** filter.</span></span>

    ![Jobbet Konfigurera skydd](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a><span data-ttu-id="85be5-210">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="85be5-210">Initial backup</span></span>
<span data-ttu-id="85be5-211">När hello virtuella datorn är skyddad med en princip som det visas under hello **skyddade objekt** flik med hello status för *skyddade - (väntar på inledande säkerhetskopia)*.</span><span class="sxs-lookup"><span data-stu-id="85be5-211">Once hello virtual machine is protected with a policy, it shows up under hello **Protected Items** tab with hello status of *Protected - (pending initial backup)*.</span></span> <span data-ttu-id="85be5-212">Som standard är hello första schemalagd säkerhetskopiering hello *första säkerhetskopian*.</span><span class="sxs-lookup"><span data-stu-id="85be5-212">By default, hello first scheduled backup is hello *initial backup*.</span></span>

<span data-ttu-id="85be5-213">tootrigger hello inledande säkerhetskopia omedelbart när du har konfigurerat skyddet:</span><span class="sxs-lookup"><span data-stu-id="85be5-213">tootrigger hello initial backup immediately after configuring protection:</span></span>

1. <span data-ttu-id="85be5-214">Längst ned hello hello **skyddade objekt** klickar du på **säkerhetskopiering nu**.</span><span class="sxs-lookup"><span data-stu-id="85be5-214">At hello bottom of hello **Protected Items** page, click **Backup Now**.</span></span>

    <span data-ttu-id="85be5-215">hello Azure Backup-tjänsten skapar ett säkerhetskopieringsjobb för hello första säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="85be5-215">hello Azure Backup service creates a backup job for hello initial backup operation.</span></span>
2. <span data-ttu-id="85be5-216">Klicka på hello **jobb** fliken tooview hello lista över jobb.</span><span class="sxs-lookup"><span data-stu-id="85be5-216">Click hello **Jobs** tab tooview hello list of jobs.</span></span>

    ![Säkerhetskopiering pågår](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> <span data-ttu-id="85be5-218">Vid säkerhetskopiering hello utfärdar hello Azure Backup service kommandot toohello säkerhetskopiering filnamnstillägget i varje virtuell dator tooflush alla skriva jobb och utför en programkonsekvent ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="85be5-218">During hello backup operation, hello Azure Backup service issues a command toohello backup extension in each virtual machine tooflush all write jobs and take a consistent snapshot.</span></span>
>
>

<span data-ttu-id="85be5-219">När hello första säkerhetskopieringen är klar, hello hello virtuella datorns status för i hello **skyddade objekt** är *skyddade*.</span><span class="sxs-lookup"><span data-stu-id="85be5-219">When hello initial backup finishes, hello status of hello virtual machine in hello **Protected Items** tab is *Protected*.</span></span>

![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a><span data-ttu-id="85be5-221">Visa information och status för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="85be5-221">Viewing backup status and details</span></span>
<span data-ttu-id="85be5-222">När skyddade, ökar hello för antal virtuella datorer också i hello **instrumentpanelen** sidan Sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="85be5-222">Once protected, hello virtual machine count also increases in hello **Dashboard** page summary.</span></span> <span data-ttu-id="85be5-223">Hej **instrumentpanelen** sidan visar också hello antalet jobb från hello senaste 24 timmarna som var *lyckade*, har *misslyckades*, och är *pågår*.</span><span class="sxs-lookup"><span data-stu-id="85be5-223">hello **Dashboard** page also shows hello number of jobs from hello last 24 hours that were *successful*, have *failed*, and are *in progress*.</span></span> <span data-ttu-id="85be5-224">På hello **jobb** använder hello **Status**, **åtgärden**, eller **från** och **till** menyer toofilter hello jobb.</span><span class="sxs-lookup"><span data-stu-id="85be5-224">On hello **Jobs** page, use hello **Status**, **Operation**, or **From** and **To** menus toofilter hello jobs.</span></span>

![Status för säkerhetskopiering i instrumentpanelens sida](./media/backup-azure-vms/dashboard-protectedvms.png)

<span data-ttu-id="85be5-226">Värdena i hello instrumentpanelen uppdateras var 24: e timme.</span><span class="sxs-lookup"><span data-stu-id="85be5-226">Values in hello dashboard are refreshed once every 24 hours.</span></span>

## <a name="troubleshooting-errors"></a><span data-ttu-id="85be5-227">Felsökning av fel</span><span class="sxs-lookup"><span data-stu-id="85be5-227">Troubleshooting errors</span></span>
<span data-ttu-id="85be5-228">Om du stöter på problem under säkerhetskopieringen av virtuell dator granskar du hello [VM felsökningsartikel](backup-azure-vms-troubleshoot.md) för hjälp.</span><span class="sxs-lookup"><span data-stu-id="85be5-228">If you run into issues while backing up your virtual machine, look at hello [VM     troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="85be5-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="85be5-229">Next steps</span></span>
* [<span data-ttu-id="85be5-230">Hantera och övervaka dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="85be5-230">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="85be5-231">Återställa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="85be5-231">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
