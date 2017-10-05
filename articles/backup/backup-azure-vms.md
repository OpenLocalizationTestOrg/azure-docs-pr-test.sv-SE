---
title: "Säkerhetskopiera klassisk distribuerade virtuella Azure-datorer till ett säkerhetskopieringsvalv | Microsoft Docs"
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
ms.openlocfilehash: e1da8bce96078a43c656f84005cefc8bbe81c9e3
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="back-up-azure-virtual-machines-classic-portal"></a><span data-ttu-id="ff1c2-104">Säkerhetskopiera virtuella Azure-datorer (klassiska portal)</span><span class="sxs-lookup"><span data-stu-id="ff1c2-104">Back up Azure virtual machines (classic portal)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="ff1c2-105">Säkerhetskopiera virtuella datorer till Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="ff1c2-105">Back up VMs to Recovery Services vault</span></span>](backup-azure-arm-vms.md)
> * [<span data-ttu-id="ff1c2-106">Säkerhetskopiera virtuella datorer till Backup-valvet</span><span class="sxs-lookup"><span data-stu-id="ff1c2-106">Back up VMs to Backup vault</span></span>](backup-azure-vms.md)
>
>

<span data-ttu-id="ff1c2-107">Den här artikeln innehåller procedurerna för att säkerhetskopiera en klassisk distribuerade Azure virtuell dator (VM) till en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-107">This article provides the procedures for backing up a Classic-deployed Azure virtual machine (VM) to a Backup vault.</span></span> <span data-ttu-id="ff1c2-108">Det finns några uppgifter som du behöver ta hand om innan du kan säkerhetskopiera en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-108">There are a few tasks you need to take care of before you can back up an Azure virtual machine.</span></span> <span data-ttu-id="ff1c2-109">Om du inte redan har gjort Slutför den [krav](backup-azure-vms-prepare.md) att förbereda miljön för att säkerhetskopiera dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-109">If you haven't already done so, complete the [prerequisites](backup-azure-vms-prepare.md) to prepare your environment for backing up your VMs.</span></span>

<span data-ttu-id="ff1c2-110">Mer information finns i artiklar på [planerar din infrastruktur för VM-säkerhetskopiering i Azure](backup-azure-vms-introduction.md) och [virtuella Azure-datorer](https://azure.microsoft.com/documentation/services/virtual-machines/).</span><span class="sxs-lookup"><span data-stu-id="ff1c2-110">For additional information, see the articles on [planning your VM backup infrastructure in Azure](backup-azure-vms-introduction.md) and [Azure virtual machines](https://azure.microsoft.com/documentation/services/virtual-machines/).</span></span>

> [!NOTE]
> <span data-ttu-id="ff1c2-111">Azure har två distributionsmodeller som används för att skapa och arbeta med resurser: [Resource Manager och den klassiska distributionsmodellen](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="ff1c2-111">Azure has two deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="ff1c2-112">Ett säkerhetskopieringsvalv kan bara skydda klassisk distribuerade virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-112">A Backup vault can only protect Classic-deployed VMs.</span></span> <span data-ttu-id="ff1c2-113">Du kan inte skydda Hanteraren för filserverresurser distribuerade virtuella datorer med en Backup-valvet.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-113">You cannot protect Resource Manager-deployed VMs with a Backup vault.</span></span> <span data-ttu-id="ff1c2-114">Se [säkerhetskopiera virtuella datorer till Recovery Services-valvet](backup-azure-arm-vms.md) mer information om hur du arbetar med Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-114">See [Back up VMs to Recovery Services vault](backup-azure-arm-vms.md) for details on working with Recovery Services vaults.</span></span>
>
>

<span data-ttu-id="ff1c2-115">Säkerhetskopiering av virtuella Azure-datorer omfattar tre viktiga steg:</span><span class="sxs-lookup"><span data-stu-id="ff1c2-115">Backing up Azure virtual machines involves three key steps:</span></span>

![Tre steg för att säkerhetskopiera en Azure IaaS-VM](./media/backup-azure-vms/3-steps-for-backup.png)

> [!NOTE]
> <span data-ttu-id="ff1c2-117">Säkerhetskopieringen av virtuella datorer är en lokal process.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-117">Backing up virtual machines is a local process.</span></span> <span data-ttu-id="ff1c2-118">Du kan säkerhetskopiera virtuella datorer i en region för säkerhetskopieringsvalvet i en annan region.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-118">You cannot back up virtual machines in one region to a backup vault in another region.</span></span> <span data-ttu-id="ff1c2-119">Så du måste skapa ett säkerhetskopieringsvalv i varje Azure-region där det finns virtuella datorer som ska säkerhetskopieras.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-119">So, you must create a backup vault in each Azure region, where there are VMs that will be backed up.</span></span>
>
> [!IMPORTANT]
> <span data-ttu-id="ff1c2-120">Från och med mars 2017 kan du inte längre använda den klassiska portalen för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-120">Starting March 2017, you can no longer use the classic portal to create Backup vaults.</span></span>
> <span data-ttu-id="ff1c2-121">Nu kan du uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-121">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="ff1c2-122">Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv).</span><span class="sxs-lookup"><span data-stu-id="ff1c2-122">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="ff1c2-123">Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-123">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="ff1c2-124">Efter den 15 oktober 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-124">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="ff1c2-125">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="ff1c2-125">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="ff1c2-126">Alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-126">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="ff1c2-127">Du kan inte längre komma åt dina säkerhetskopierade data i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-127">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="ff1c2-128">Använd i stället Azure Portal till att få åtkomst till dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-128">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="step-1---discover-azure-virtual-machines"></a><span data-ttu-id="ff1c2-129">Steg 1 – identifiera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="ff1c2-129">Step 1 - Discover Azure virtual machines</span></span>
<span data-ttu-id="ff1c2-130">Om du vill se till att alla nya virtuella datorer (VM) lägga till prenumerationen identifieras innan du registrerar kör identifieringen.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-130">To ensure any new virtual machines (VMs) added to the subscription are identified before registering, run the discovery process.</span></span> <span data-ttu-id="ff1c2-131">Under processen uppmanas Azure att returnera listan med virtuella datorer i prenumerationen, tillsammans med ytterligare information som molntjänstens namn och regionen.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-131">The process queries Azure for the list of virtual machines in the subscription, along with additional information like the cloud service name and the region.</span></span>

1. <span data-ttu-id="ff1c2-132">Logga in på den [klassisk portal](http://manage.windowsazure.com/)</span><span class="sxs-lookup"><span data-stu-id="ff1c2-132">Sign in to the [Classic portal](http://manage.windowsazure.com/)</span></span>
2. <span data-ttu-id="ff1c2-133">I listan över Azure-tjänster, klickar du på **återställningstjänster** att öppna listan över säkerhetskopiering och Site Recovery-valv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-133">In the list of Azure services, click **Recovery Services** to open the list of Backup and Site Recovery vaults.</span></span>
    <span data-ttu-id="ff1c2-134">![Öppna valvet lista](./media/backup-azure-vms/choose-vault-list.png)</span><span class="sxs-lookup"><span data-stu-id="ff1c2-134">![Open vault list](./media/backup-azure-vms/choose-vault-list.png)</span></span>
3. <span data-ttu-id="ff1c2-135">Välj valvet för att säkerhetskopiera en virtuell dator i listan över säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-135">In the list of Backup vaults, select the vault to back up a VM.</span></span>

    <span data-ttu-id="ff1c2-136">Om det här är ett nytt valv portalen öppnas på den **Snabbstart** sidan.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-136">If this is a new vault the portal opens to the **Quick Start** page.</span></span>

    ![Öppna registrerade objekt-menyn](./media/backup-azure-vms/vault-quick-start.png)

    <span data-ttu-id="ff1c2-138">Om valvet tidigare har konfigurerats, öppnar portalen senast använda-menyn.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-138">If the vault has previously been configured, the portal opens to the most recently used menu.</span></span>
4. <span data-ttu-id="ff1c2-139">Klicka på menyn valvet (överst på sidan) **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-139">From the vault menu (at the top of the page), click **Registered Items**.</span></span>

    ![Öppna registrerade objekt-menyn](./media/backup-azure-vms/vault-menu.png)
5. <span data-ttu-id="ff1c2-141">Välj  **Virtuell Azure-dator** på **Typ**-menyn.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-141">From the **Type** menu, select **Azure Virtual Machine**.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
6. <span data-ttu-id="ff1c2-143">Klicka på **Identifiera** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-143">Click **DISCOVER** at the bottom of the page.</span></span>
    <span data-ttu-id="ff1c2-144">![Knappen Identifiera](./media/backup-azure-vms/discover-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="ff1c2-144">![Discover button](./media/backup-azure-vms/discover-button-only.png)</span></span>

    <span data-ttu-id="ff1c2-145">Identifieringen kan ta några minuter medan de virtuella datorerna visas i tabellformat.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-145">The discovery process may take a few minutes while the virtual machines are being tabulated.</span></span> <span data-ttu-id="ff1c2-146">Ett meddelande visas längst ned på skärmen som anger att processen körs.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-146">There is a notification at the bottom of the screen that lets you know that the process is running.</span></span>

    ![Identifiera virtuella datorer](./media/backup-azure-vms/discovering-vms.png)

    <span data-ttu-id="ff1c2-148">Meddelandet ändras när processen är klar.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-148">The notification changes when the process is complete.</span></span> <span data-ttu-id="ff1c2-149">Om identifieringen inte gick att hitta de virtuella datorerna, först se till att de virtuella datorerna finns.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-149">If the discovery process did not find the virtual machines, first ensure the VMs exist.</span></span> <span data-ttu-id="ff1c2-150">Om de virtuella datorerna finns, se till att de virtuella datorerna finns i samma region som säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-150">If the VMs exist, ensure the VMs are in the same region as the backup vault.</span></span> <span data-ttu-id="ff1c2-151">Om de virtuella datorerna finns och är i samma region, se till att de virtuella datorerna inte redan har registrerats ett säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-151">If the VMs exist and are in the same region, ensure the VMs are not already registered to a backup vault.</span></span> <span data-ttu-id="ff1c2-152">Om en virtuell dator har tilldelats ett säkerhetskopieringsvalv som den inte är tillgänglig som ska tilldelas andra säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-152">If a VM is assigned to a backup vault it is not available to be assigned to other backup vaults.</span></span>

    ![Identifieringen är klar](./media/backup-azure-vms/discovery-complete.png)

    <span data-ttu-id="ff1c2-154">När du har identifierat nya objekt, gå till steg 2 och registrera dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-154">Once you have discovered the new items, go to Step 2 and register your VMs.</span></span>

## <a name="step-2---register-azure-virtual-machines"></a><span data-ttu-id="ff1c2-155">Steg 2 – registrera virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="ff1c2-155">Step 2 - Register Azure virtual machines</span></span>
<span data-ttu-id="ff1c2-156">Registrerar du en virtuell Azure-dator för att associera den med Azure Backup-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-156">You register an Azure virtual machine to associate it with the Azure Backup service.</span></span> <span data-ttu-id="ff1c2-157">Detta inträffar vanligtvis en gång.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-157">This is typically a one-time activity.</span></span>

1. <span data-ttu-id="ff1c2-158">Navigera till säkerhetskopieringsvalvet under **återställningstjänster** i Azure-portalen och klicka sedan på **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-158">Navigate to the backup vault under **Recovery Services** in the Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="ff1c2-159">Välj **Virtuell Azure-dator** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-159">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Välja arbetsbelastning](./media/backup-azure-vms/discovery-select-workload.png)
3. <span data-ttu-id="ff1c2-161">Klicka på **Registrera** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-161">Click **REGISTER** at the bottom of the page.</span></span>
    <span data-ttu-id="ff1c2-162">![Knappen Registrera](./media/backup-azure-vms/register-button-only.png)</span><span class="sxs-lookup"><span data-stu-id="ff1c2-162">![Register button](./media/backup-azure-vms/register-button-only.png)</span></span>
4. <span data-ttu-id="ff1c2-163">På snabbmenyn **Registrera objekt** väljer du de virtuella datorer som du vill registrera.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-163">In the **Register Items** shortcut menu, select the virtual machines that you want to register.</span></span> <span data-ttu-id="ff1c2-164">Om det finns två eller flera virtuella datorer med samma namn, kan du använda Molntjänsten för att skilja dem åt.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-164">If there are two or more virtual machines with the same name, use the cloud service to distinguish between them.</span></span>

   > [!TIP]
   > <span data-ttu-id="ff1c2-165">Flera virtuella datorer kan registreras samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-165">Multiple virtual machines can be registered at one time.</span></span>
   >
   >

    <span data-ttu-id="ff1c2-166">Ett jobb skapas för varje virtuell dator som du har valt.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-166">A job is created for each virtual machine that you've selected.</span></span>
5. <span data-ttu-id="ff1c2-167">Klicka på **Visa jobb** i meddelandet för att gå till sidan **Jobb**.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-167">Click **View Job** in the notification to go to the **Jobs** page.</span></span>

    ![Registrera jobb](./media/backup-azure-vms/register-create-job.png)

    <span data-ttu-id="ff1c2-169">Den virtuella datorn visas också i listan över registrerade objekt, tillsammans med statusen för registreringsåtgärden.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-169">The virtual machine also appears in the list of registered items, along with the status of the registration operation.</span></span>

    ![Registreringsstatus 1](./media/backup-azure-vms/register-status01.png)

    <span data-ttu-id="ff1c2-171">När åtgärden har slutförts ändras statusen för att återspegla tillståndet för *registrerade objekt*.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-171">When the operation completes, the status changes to reflect the *registered* state.</span></span>

    ![Registreringsstatus 2](./media/backup-azure-vms/register-status02.png)

## <a name="step-3---protect-azure-virtual-machines"></a><span data-ttu-id="ff1c2-173">Steg 3 – skydda virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="ff1c2-173">Step 3 - Protect Azure virtual machines</span></span>
<span data-ttu-id="ff1c2-174">Du kan nu konfigurera en princip för säkerhetskopiering och kvarhållning för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-174">Now you can set up a backup and retention policy for the virtual machine.</span></span> <span data-ttu-id="ff1c2-175">Flera virtuella datorer kan skyddas med hjälp av en enda skydda åtgärd.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-175">Multiple virtual machines can be protected by using a single protect action.</span></span>

<span data-ttu-id="ff1c2-176">Azure-säkerhetskopieringsvalv skapas efter maj 2015 medföljer en standardprincip inbyggda i valvet.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-176">Azure Backup vaults created after May 2015 come with a default policy built into the vault.</span></span> <span data-ttu-id="ff1c2-177">Den här standardprincipen innehåller en Standardkvarhållning av 30 dagar och ett schema för säkerhetskopiering av en gång dagligen.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-177">This default policy comes with a default retention of 30 days and a once-daily backup schedule.</span></span>

1. <span data-ttu-id="ff1c2-178">Navigera till säkerhetskopieringsvalvet under **återställningstjänster** i Azure-portalen och klicka sedan på **registrerade objekt**.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-178">Navigate to the backup vault under **Recovery Services** in the Azure portal, and then click **Registered Items**.</span></span>
2. <span data-ttu-id="ff1c2-179">Välj **Virtuell Azure-dator** i listrutan.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-179">Select **Azure Virtual Machine** from the drop-down menu.</span></span>

    ![Välja arbetsbelastning på portalen](./media/backup-azure-vms/select-workload.png)
3. <span data-ttu-id="ff1c2-181">Klicka på **Skydda** längst ned på sidan.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-181">Click **PROTECT** at the bottom of the page.</span></span>

    <span data-ttu-id="ff1c2-182">Den **skydda objekt guiden** visas.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-182">The **Protect Items wizard** appears.</span></span> <span data-ttu-id="ff1c2-183">Guiden visar endast virtuella datorer som registrerats och inte skyddas.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-183">The wizard only lists virtual machines that are registered and not protected.</span></span> <span data-ttu-id="ff1c2-184">Välj de virtuella datorer som du vill skydda.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-184">Select the virtual machines that you want to protect.</span></span>

    <span data-ttu-id="ff1c2-185">Om det finns två eller flera virtuella datorer med samma namn, kan du använda Molntjänsten för att skilja mellan de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-185">If there are two or more virtual machines with the same name, use the cloud service to distinguish between the virtual machines.</span></span>

   > [!TIP]
   > <span data-ttu-id="ff1c2-186">Du kan skydda flera virtuella datorer samtidigt.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-186">You can protect multiple virtual machines at one time.</span></span>
   >
   >

    ![Konfigurera skydd i stor skala](./media/backup-azure-vms/protect-at-scale.png)

4. <span data-ttu-id="ff1c2-188">Välj en **Säkerhetskopieringsschemat** att säkerhetskopiera virtuella datorer som du har valt.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-188">Choose a **backup schedule** to back up the virtual machines that you've selected.</span></span> <span data-ttu-id="ff1c2-189">Du kan välja från en befintlig uppsättning principer eller definiera en ny.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-189">You can pick from an existing set of policies or define a new one.</span></span>

    <span data-ttu-id="ff1c2-190">Flera virtuella datorer kan vara associerade med varje säkerhetskopieringspolicy.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-190">Each backup policy can have multiple virtual machines associated with it.</span></span> <span data-ttu-id="ff1c2-191">Den virtuella datorn kan dock bara vara kopplad till en princip vid en viss tidpunkt.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-191">However, the virtual machine can only be associated with one policy at any given point in time.</span></span>

    ![Skydda med ny princip](./media/backup-azure-vms/policy-schedule.png)

   > [!NOTE]
   > <span data-ttu-id="ff1c2-193">En säkerhetskopieringspolicy innehåller ett kvarhållningsschema för de schemalagda säkerhetskopieringarna.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-193">A backup policy includes a retention scheme for the scheduled backups.</span></span> <span data-ttu-id="ff1c2-194">Om du väljer en säkerhetskopieringsprincip kan ändra du inte alternativ för kvarhållning i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-194">If you select an existing backup policy, you cannot modify the retention options in the next step.</span></span>
   >
   >

5. <span data-ttu-id="ff1c2-195">Välj en **Kvarhållningsintervall** ska associeras med säkerhetskopiorna.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-195">Choose a **retention range** to associate with the backups.</span></span>

    ![Skydda med flexibel kvarhållning](./media/backup-azure-vms/policy-retention.png)

    <span data-ttu-id="ff1c2-197">Bevarandeprincipen anger hur länge en säkerhetskopia ska lagras.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-197">Retention policy specifies the length of time for storing a backup.</span></span> <span data-ttu-id="ff1c2-198">Du kan ange andra bevarandeprinciper baserat på när säkerhetskopieringen görs.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-198">You can specify different retention policies based on when the backup is taken.</span></span> <span data-ttu-id="ff1c2-199">Exempelvis kan en säkerhetskopieringspunkt tas dagligen (som fungerar som en operativa återställningspunkt) bevaras i 90 dagar.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-199">For example, a backup point taken daily (which serves as an operational recovery point) might be preserved for 90 days.</span></span> <span data-ttu-id="ff1c2-200">Jämförelse behöva en säkerhetskopieringspunkt vidtas i slutet av varje kvartal (som audit) bevaras för många månader eller år.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-200">In comparison, a backup point taken at the end of each quarter (for audit purposes) may need to be preserved for many months or years.</span></span>

    ![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/long-term-retention.png)

    <span data-ttu-id="ff1c2-202">I det här exemplet avbildningen:</span><span class="sxs-lookup"><span data-stu-id="ff1c2-202">In this example image:</span></span>

   * <span data-ttu-id="ff1c2-203">**Dagliga bevarandeprincip**: säkerhetskopieringar tas dagligen sparas i 30 dagar.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-203">**Daily retention policy**: Backups taken daily are stored for 30 days.</span></span>
   * <span data-ttu-id="ff1c2-204">**Varje vecka bevarandeprincip**: säkerhetskopior som gjorts varje vecka på söndag bevaras i 104 veckor.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-204">**Weekly retention policy**: Backups taken every week on Sunday are preserved for 104 weeks.</span></span>
   * <span data-ttu-id="ff1c2-205">**Månatliga bevarandeprincip**: säkerhetskopieringar som utförts på den sista söndagen i varje månad bevaras i 120 månader.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-205">**Monthly retention policy**: Backups taken on the last Sunday of each month are preserved for 120 months.</span></span>
   * <span data-ttu-id="ff1c2-206">**Årlig bevarandeprincip**: säkerhetskopieringar som utförts på den första söndagen i varje januari bevaras för 99 år.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-206">**Yearly retention policy**: Backups taken on the first Sunday of every January are preserved for 99 years.</span></span>

     <span data-ttu-id="ff1c2-207">Ett jobb skapas för att konfigurera protection-principen och koppla de virtuella datorerna till principen för varje virtuell dator som du har valt.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-207">A job is created to configure the protection policy and associate the virtual machines to that policy for each virtual machine that you've selected.</span></span>
6. <span data-ttu-id="ff1c2-208">Visa lista med **Konfigurera skydd** jobb valv-menyn klickar du på **jobb** och välj **Konfigurera skydd** från den **åtgärden** filter.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-208">To view the list of **Configure Protection** jobs, from the vaults menu, click **Jobs** and select **Configure Protection** from the **Operation** filter.</span></span>

    ![Jobbet Konfigurera skydd](./media/backup-azure-vms/protect-configureprotection.png)

## <a name="initial-backup"></a><span data-ttu-id="ff1c2-210">Den första säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="ff1c2-210">Initial backup</span></span>
<span data-ttu-id="ff1c2-211">När den virtuella datorn är skyddad med en princip som det visas under den **skyddade objekt** flik med status för *skyddade - (väntar på inledande säkerhetskopia)*.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-211">Once the virtual machine is protected with a policy, it shows up under the **Protected Items** tab with the status of *Protected - (pending initial backup)*.</span></span> <span data-ttu-id="ff1c2-212">Som standard är den första säkerhetskopieringen den *inledande säkerhetskopieringen*.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-212">By default, the first scheduled backup is the *initial backup*.</span></span>

<span data-ttu-id="ff1c2-213">Utlöser den första säkerhetskopian omedelbart när du har konfigurerat skyddet:</span><span class="sxs-lookup"><span data-stu-id="ff1c2-213">To trigger the initial backup immediately after configuring protection:</span></span>

1. <span data-ttu-id="ff1c2-214">Längst ned i den **skyddade objekt** klickar du på **säkerhetskopiering nu**.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-214">At the bottom of the **Protected Items** page, click **Backup Now**.</span></span>

    <span data-ttu-id="ff1c2-215">Tjänsten Azure Backup skapar ett säkerhetskopieringsjobb för den första säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-215">The Azure Backup service creates a backup job for the initial backup operation.</span></span>
2. <span data-ttu-id="ff1c2-216">Klicka på fliken **Jobb** för att visa listan över jobb.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-216">Click the **Jobs** tab to view the list of jobs.</span></span>

    ![Säkerhetskopiering pågår](./media/backup-azure-vms/protect-inprogress.png)

> [!NOTE]
> <span data-ttu-id="ff1c2-218">Under säkerhetskopieringen utfärdar tjänsten Azure Backup ett kommando till reservanknytning i varje virtuell dator att tömma alla jobb för skrivning och dra en programkonsekvent ögonblicksbild.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-218">During the backup operation, the Azure Backup service issues a command to the backup extension in each virtual machine to flush all write jobs and take a consistent snapshot.</span></span>
>
>

<span data-ttu-id="ff1c2-219">När den första säkerhetskopian är klar, status för den virtuella datorn i den **skyddade objekt** är *skyddade*.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-219">When the initial backup finishes, the status of the virtual machine in the **Protected Items** tab is *Protected*.</span></span>

![Virtuella datorer säkerhetskopieras med återställningspunkter](./media/backup-azure-vms/protect-backedupvm.png)

## <a name="viewing-backup-status-and-details"></a><span data-ttu-id="ff1c2-221">Visa information och status för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="ff1c2-221">Viewing backup status and details</span></span>
<span data-ttu-id="ff1c2-222">När skyddade, ökar antalet virtuella datorer även i den **instrumentpanelen** sidan Sammanfattning.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-222">Once protected, the virtual machine count also increases in the **Dashboard** page summary.</span></span> <span data-ttu-id="ff1c2-223">Den **instrumentpanelen** också visar antalet jobb från de senaste 24 timmarna som var *lyckade*, har *misslyckades*, och är *pågår*.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-223">The **Dashboard** page also shows the number of jobs from the last 24 hours that were *successful*, have *failed*, and are *in progress*.</span></span> <span data-ttu-id="ff1c2-224">På den **jobb** använder den **Status**, **åtgärden**, eller **från** och **till** menyer att filtrera jobb.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-224">On the **Jobs** page, use the **Status**, **Operation**, or **From** and **To** menus to filter the jobs.</span></span>

![Status för säkerhetskopiering i instrumentpanelens sida](./media/backup-azure-vms/dashboard-protectedvms.png)

<span data-ttu-id="ff1c2-226">Värdena i instrumentpanelen uppdateras var 24: e timme.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-226">Values in the dashboard are refreshed once every 24 hours.</span></span>

## <a name="troubleshooting-errors"></a><span data-ttu-id="ff1c2-227">Felsökning av fel</span><span class="sxs-lookup"><span data-stu-id="ff1c2-227">Troubleshooting errors</span></span>
<span data-ttu-id="ff1c2-228">Om du stöter på problem under säkerhetskopieringen av virtuell dator granskar du den [VM felsökningsartikel](backup-azure-vms-troubleshoot.md) för hjälp.</span><span class="sxs-lookup"><span data-stu-id="ff1c2-228">If you run into issues while backing up your virtual machine, look at the [VM     troubleshooting article](backup-azure-vms-troubleshoot.md) for help.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ff1c2-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ff1c2-229">Next steps</span></span>
* [<span data-ttu-id="ff1c2-230">Hantera och övervaka dina virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ff1c2-230">Manage and monitor your virtual machines</span></span>](backup-azure-manage-vms.md)
* [<span data-ttu-id="ff1c2-231">Återställa virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="ff1c2-231">Restore virtual machines</span></span>](backup-azure-restore-vms.md)
