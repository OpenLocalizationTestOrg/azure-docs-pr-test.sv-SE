---
title: "aaaManage Resource Manager distribuerade virtuella datorsäkerhetskopieringar | Microsoft Docs"
description: "Lär dig hur toomanage och övervaka säkerhetskopiering för Resource Manager distribuerade virtuella datorer"
services: backup
documentationcenter: 
author: trinadhk
manager: shreeshd
editor: 
ms.assetid: f3050283-d60f-472d-b464-cb844e70d67e
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/28/2016
ms.author: trinadhk;markgal
ms.openlocfilehash: 241fc4b2a29c5cd8b8b0ee8efbf8ba4e96c6a7ea
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-virtual-machine-backups"></a><span data-ttu-id="b7185-103">Hantera säkerhetskopiering av virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="b7185-103">Manage Azure virtual machine backups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="b7185-104">Hantera Virtuella Azure-säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="b7185-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="b7185-105">Hantera klassiska VM-säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="b7185-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="b7185-106">Den här artikeln innehåller information om hur du hanterar VM-säkerhetskopieringar och förklarar hello aviseringar om säkerhetskopiering informationen i hello portalens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="b7185-106">This article provides guidance on managing VM backups, and explains hello backup alerts information available in hello portal dashboard.</span></span> <span data-ttu-id="b7185-107">hello anvisningarna i den här artikeln gäller toousing virtuella datorer med Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="b7185-107">hello guidance in this article applies toousing VMs with Recovery Services vaults.</span></span> <span data-ttu-id="b7185-108">Den här artikeln inte beskriver hello skapandet av virtuella datorer eller förklarar den hur tooprotect virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="b7185-108">This article does not cover hello creation of virtual machines, nor does it explain how tooprotect virtual machines.</span></span> <span data-ttu-id="b7185-109">En introduktion om hur du skyddar Azure Resource Manager distribuerade virtuella datorer i Azure med ett Recovery Services-valv finns [förhandstitt: Säkerhetskopiera virtuella datorer tooa Recovery Services-valvet](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b7185-109">For a primer on protecting Azure Resource Manager-deployed VMs in Azure with a Recovery Services vault, see [First look: Back up VMs tooa Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span>

## <a name="manage-vaults-and-protected-virtual-machines"></a><span data-ttu-id="b7185-110">Hantera valv och skyddade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b7185-110">Manage vaults and protected virtual machines</span></span>
<span data-ttu-id="b7185-111">I hello Azure-portalen ger hello Recovery Services-valvet instrumentpanelen åtkomst tooinformation om hello valvet, inklusive:</span><span class="sxs-lookup"><span data-stu-id="b7185-111">In hello Azure portal, hello Recovery Services vault dashboard provides access tooinformation about hello vault including:</span></span>

* <span data-ttu-id="b7185-112">hello senaste säkerhetskopian av ögonblicksbild, som också hello senaste återställningspunkt < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-112">hello most recent backup snapshot, which is also hello latest restore point <br\></span></span>
* <span data-ttu-id="b7185-113">Hej säkerhetskopieringsprincip < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-113">hello backup policy <br\></span></span>
* <span data-ttu-id="b7185-114">Total storlek på alla ögonblicksbilder av säkerhetskopior < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-114">total size of all backup snapshots <br\></span></span>
* <span data-ttu-id="b7185-115">antal virtuella datorer som är skyddade med hello valvet < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-115">number of virtual machines that are protected with hello vault <br\></span></span>

<span data-ttu-id="b7185-116">Många hanteringsuppgifter med en säkerhetskopiering av virtuella datorer som börjar med öppna hello valvet i hello instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7185-116">Many management tasks with a virtual machine backup begin with opening hello vault in hello dashboard.</span></span> <span data-ttu-id="b7185-117">Men eftersom valv kan vara används tooprotect flera objekt (eller flera virtuella datorer) tooview information om en viss virtuell dator, öppna hello valvet objektet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7185-117">However, because vaults can be used tooprotect multiple items (or multiple VMs), tooview details about a particular VM, open hello vault item dashboard.</span></span> <span data-ttu-id="b7185-118">hello följande procedur visar hur tooopen hello *valvet instrumentpanelen* och fortsätt sedan toohello *valvet objektet instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="b7185-118">hello following procedure shows you how tooopen hello *vault dashboard* and then continue toohello *vault item dashboard*.</span></span> <span data-ttu-id="b7185-119">Det finns ”tips” i båda procedurerna som pekar ut hur tooadd hello valvet och valvet objektet toohello Azure instrumentpanel med kommandot hello PIN-kod toodashboard.</span><span class="sxs-lookup"><span data-stu-id="b7185-119">There are "tips" in both procedures that point out how tooadd hello vault and vault item toohello Azure dashboard by using hello Pin toodashboard command.</span></span> <span data-ttu-id="b7185-120">PIN-kod toodashboard är ett sätt att skapa en genväg toohello valvet eller element.</span><span class="sxs-lookup"><span data-stu-id="b7185-120">Pin toodashboard is a way of creating a shortcut toohello vault or item.</span></span> <span data-ttu-id="b7185-121">Du kan också köra vanliga kommandon från hello genväg.</span><span class="sxs-lookup"><span data-stu-id="b7185-121">You can also execute common commands from hello shortcut.</span></span>

> [!TIP]
> <span data-ttu-id="b7185-122">Om du har flera instrumentpaneler och blad öppna Använd hello mörkt blå skjutreglaget längst hello hello fönstret tooslide hello Azure instrumentpanelen fram och tillbaka.</span><span class="sxs-lookup"><span data-stu-id="b7185-122">If you have multiple dashboards and blades open, use hello dark-blue slider at hello bottom of hello window tooslide hello Azure dashboard back and forth.</span></span>
>
>

![Fullständig vy med skjutreglaget](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-hello-dashboard"></a><span data-ttu-id="b7185-124">Öppna ett Recovery Services-valv i hello instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="b7185-124">Open a Recovery Services vault in hello dashboard:</span></span>
1. <span data-ttu-id="b7185-125">Logga in toohello [Azure-portalen](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="b7185-125">Sign in toohello [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="b7185-126">Hej hubbmenyn, klicka på **Bläddra** och Skriv i hello lista över resurser, **återställningstjänster**.</span><span class="sxs-lookup"><span data-stu-id="b7185-126">On hello Hub menu, click **Browse** and in hello list of resources, type **Recovery Services**.</span></span> <span data-ttu-id="b7185-127">När du börjar skriva in hello listan filtreras baserat på dina indata.</span><span class="sxs-lookup"><span data-stu-id="b7185-127">As you begin typing, hello list filters based on your input.</span></span> <span data-ttu-id="b7185-128">Klicka på **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="b7185-128">Click **Recovery Services vault**.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    <span data-ttu-id="b7185-130">hello lista över Recovery Services-valv visas.</span><span class="sxs-lookup"><span data-stu-id="b7185-130">hello list of Recovery Services vaults are displayed.</span></span>

    ![<span data-ttu-id="b7185-131">Lista över Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="b7185-131">List of Recovery Services vaults</span></span> ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > <span data-ttu-id="b7185-132">Om du fäster en valvet toohello Azure instrumentpanelen är som valvet omedelbart tillgängligt när du öppnar hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="b7185-132">If you pin a vault toohello Azure Dashboard, that vault is immediately accessible when you open hello Azure portal.</span></span> <span data-ttu-id="b7185-133">toopin en valvet toohello instrumentpanelen i hello valvet lista, högerklicka på hello valvet och välj **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="b7185-133">toopin a vault toohello dashboard, in hello vault list, right-click hello vault, and select **Pin toodashboard**.</span></span>
   >
   >
3. <span data-ttu-id="b7185-134">Hello listan över valv och välj hello valvet tooopen dess instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="b7185-134">From hello list of vaults, select hello vault tooopen its dashboard.</span></span> <span data-ttu-id="b7185-135">När du väljer hello valvet, hello valvet instrumentpanelen och hello **inställningar** bladet som är öppna.</span><span class="sxs-lookup"><span data-stu-id="b7185-135">When you select hello vault, hello vault dashboard and hello **Settings** blade open.</span></span> <span data-ttu-id="b7185-136">I följande bild hello, hello **Contoso valvet** instrumentpanelen är markerat.</span><span class="sxs-lookup"><span data-stu-id="b7185-136">In hello following image, hello **Contoso-vault** dashboard is highlighted.</span></span>

    ![Öppna instrumentpanelen i valvet och inställningsbladet](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a><span data-ttu-id="b7185-138">Öppna en valvet objektet instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="b7185-138">Open a vault item dashboard</span></span>
<span data-ttu-id="b7185-139">Hello föregående procedur du har öppnat hello valvet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7185-139">In hello previous procedure you opened hello vault dashboard.</span></span> <span data-ttu-id="b7185-140">instrumentpanel för tooopen hello valvet objektet:</span><span class="sxs-lookup"><span data-stu-id="b7185-140">tooopen hello vault item dashboard:</span></span>

1. <span data-ttu-id="b7185-141">Hello valvet instrumentpanelen på hello **säkerhetskopiering objekt** panelen, klickar du på **Azure Virtual Machines**.</span><span class="sxs-lookup"><span data-stu-id="b7185-141">In hello vault dashboard, on hello **Backup Items** tile, click **Azure Virtual Machines**.</span></span>

    ![Öppna Säkerhetskopiering poster sida vid sida](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    <span data-ttu-id="b7185-143">Hej **Säkerhetskopieringsobjekt** bladet visar hello senaste säkerhetskopieringsjobbet för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="b7185-143">hello **Backup Items** blade lists hello last backup job for each item.</span></span> <span data-ttu-id="b7185-144">I det här exemplet har en virtuell dator, demovm markgal skyddas av det här valvet.</span><span class="sxs-lookup"><span data-stu-id="b7185-144">In this example, there is one virtual machine, demovm-markgal, protected by this vault.</span></span>  

    ![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > <span data-ttu-id="b7185-146">Du kan fästa en valvet objektet toohello Azure instrumentpanelen för enkel åtkomst.</span><span class="sxs-lookup"><span data-stu-id="b7185-146">For ease of access, you can pin a vault item toohello Azure Dashboard.</span></span> <span data-ttu-id="b7185-147">toopin ett valv-objekt i listan över objekt för hello valvet, högerklicka hello objektet och välj **PIN-kod toodashboard**.</span><span class="sxs-lookup"><span data-stu-id="b7185-147">toopin a vault item, in hello vault item list, right-click hello item and select **Pin toodashboard**.</span></span>
   >
   >
2. <span data-ttu-id="b7185-148">I hello **säkerhetskopiering objekt** bladet, klickar du på hello objektet tooopen hello valvet objektet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7185-148">In hello **Backup Items** blade, click hello item tooopen hello vault item dashboard.</span></span>

    ![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    <span data-ttu-id="b7185-150">hello valvet objektet instrumentpanel och dess **inställningar** bladet som är öppna.</span><span class="sxs-lookup"><span data-stu-id="b7185-150">hello vault item dashboard and its **Settings** blade open.</span></span>

    ![Säkerhetskopiera objekt instrumentpanel med inställningsbladet](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    <span data-ttu-id="b7185-152">Hello valvet objektet instrumentpanel kan du utföra många viktiga hanteringsaktiviteter som:</span><span class="sxs-lookup"><span data-stu-id="b7185-152">From hello vault item dashboard, you can accomplish many key management tasks, such as:</span></span>

   * <span data-ttu-id="b7185-153">Ändra principer eller skapa en ny säkerhetskopieringsprincip < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-153">change policies or create a new backup policy<br\></span></span>
   * <span data-ttu-id="b7185-154">Visa återställningspunkter och se deras konsekvent tillstånd < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-154">view restore points, and see their consistency state <br\></span></span>
   * <span data-ttu-id="b7185-155">säkerhetskopiering på begäran för en virtuell dator < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-155">on-demand backup of a virtual machine <br\></span></span>
   * <span data-ttu-id="b7185-156">sluta skydda virtuella datorer < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-156">stop protecting virtual machines <br\></span></span>
   * <span data-ttu-id="b7185-157">återuppta skyddet av en virtuell dator < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-157">resume protection of a virtual machine <br\></span></span>
   * <span data-ttu-id="b7185-158">ta bort säkerhetskopierade data (eller återställningspunkt) < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-158">delete a backup data (or recovery point) <br\></span></span>
   * <span data-ttu-id="b7185-159">[återställa säkerhetskopior](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\></span><span class="sxs-lookup"><span data-stu-id="b7185-159">[restore backup disks](backup-azure-arm-restore-vms.md#restore-backed-up-disks)  <br\></span></span>

<span data-ttu-id="b7185-160">Hello följande procedurer, är hello startpunkt hello valvet objektet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7185-160">For hello following procedures, hello starting point is hello vault item dashboard.</span></span>

## <a name="manage-backup-policies"></a><span data-ttu-id="b7185-161">Hantera säkerhetskopieringsprinciper</span><span class="sxs-lookup"><span data-stu-id="b7185-161">Manage backup policies</span></span>
1. <span data-ttu-id="b7185-162">På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **alla inställningar** tooopen hello **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="b7185-162">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **All Settings** tooopen hello **Settings** blade.</span></span>

    ![Princip för säkerhetskopiering bladet](./media/backup-azure-manage-vms/all-settings-button.png)
2. <span data-ttu-id="b7185-164">På hello **inställningar** bladet, klickar du på **säkerhetskopiera princip** tooopen det bladet.</span><span class="sxs-lookup"><span data-stu-id="b7185-164">On hello **Settings** blade, click **Backup policy** tooopen that blade.</span></span>

    <span data-ttu-id="b7185-165">På bladet hello hello säkerhetskopiering frekvens och kvarhållning intervallet information visas.</span><span class="sxs-lookup"><span data-stu-id="b7185-165">On hello blade, hello backup frequency and retention range details are shown.</span></span>

    ![Princip för säkerhetskopiering bladet](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. <span data-ttu-id="b7185-167">Från hello **Välj säkerhetskopieringsprincip** menyn:</span><span class="sxs-lookup"><span data-stu-id="b7185-167">From hello **Choose backup policy** menu:</span></span>

   * <span data-ttu-id="b7185-168">toochange principer, Välj en annan princip och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="b7185-168">toochange policies, select a different policy and click **Save**.</span></span> <span data-ttu-id="b7185-169">hello nya principen är omedelbart tillämpade toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="b7185-169">hello new policy is immediately applied toohello vault.</span></span> <span data-ttu-id="b7185-170">< br\></span><span class="sxs-lookup"><span data-stu-id="b7185-170"><br\></span></span>
   * <span data-ttu-id="b7185-171">toocreate en princip, Välj **Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="b7185-171">toocreate a policy, select **Create New**.</span></span>

     ![Säkerhetskopiering av virtuell dator](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     <span data-ttu-id="b7185-173">Anvisningar om hur du skapar en princip för säkerhetskopiering finns [definierar en princip för säkerhetskopiering](backup-azure-manage-vms.md#defining-a-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="b7185-173">For instructions on creating a backup policy, see [Defining a backup policy](backup-azure-manage-vms.md#defining-a-backup-policy).</span></span>

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> <span data-ttu-id="b7185-174">När du hanterar principer för säkerhetskopiering, gör att toofollow hello [metodtips](backup-azure-vms-introduction.md#best-practices) för optimal prestanda vid säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="b7185-174">While managing backup policies, make sure toofollow hello [best practices](backup-azure-vms-introduction.md#best-practices) for optimal backup performance</span></span>
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="b7185-175">Säkerhetskopiering på begäran för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b7185-175">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="b7185-176">Du kan utföra en på-begäran säkerhetskopiering av en virtuell dator när den har konfigurerats för skydd.</span><span class="sxs-lookup"><span data-stu-id="b7185-176">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="b7185-177">Om hello första säkerhetskopian väntar skapas säkerhetskopiering på begäran en fullständig kopia av hello virtuell dator i hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="b7185-177">If hello initial backup is pending, on-demand backup creates a full copy of hello virtual machine in hello Recovery Services vault.</span></span> <span data-ttu-id="b7185-178">Om hello första säkerhetskopieringen har slutförts skickas en säkerhetskopiering på begäran endast ändringar från hello tidigare ögonblicksbild, toohello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="b7185-178">If hello initial backup is completed, an on-demand backup will only send changes from hello previous snapshot, toohello Recovery Services vault.</span></span> <span data-ttu-id="b7185-179">Efterföljande säkerhetskopieringar är som är alltid inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="b7185-179">That is, subsequent backups are always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="b7185-180">Hej kvarhållningsintervallet för en säkerhetskopiering på begäran är hello kvarhållningsvärde har angetts för hello daglig säkerhetskopieringspunkt i hello princip.</span><span class="sxs-lookup"><span data-stu-id="b7185-180">hello retention range for an on-demand backup is hello retention value specified for hello Daily backup point in hello policy.</span></span> <span data-ttu-id="b7185-181">Om ingen daglig säkerhetskopieringspunkt är markerad används hello veckovis säkerhetskopieringspunkt.</span><span class="sxs-lookup"><span data-stu-id="b7185-181">If no Daily backup point is selected, then hello weekly backup point is used.</span></span>
>
>

<span data-ttu-id="b7185-182">tootrigger en på-begäran-säkerhetskopia av en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="b7185-182">tootrigger an on-demand backup of a virtual machine:</span></span>

* <span data-ttu-id="b7185-183">På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="b7185-183">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Backup now**.</span></span>

    ![Säkerhetskopiering nu knappen](./media/backup-azure-manage-vms/backup-now-button.png)

    <span data-ttu-id="b7185-185">hello portal ser till att du vill att toostart en säkerhetskopiering på begäran.</span><span class="sxs-lookup"><span data-stu-id="b7185-185">hello portal makes sure that you want toostart an on-demand backup job.</span></span> <span data-ttu-id="b7185-186">Klicka på **Ja** toostart hello säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="b7185-186">Click **Yes** toostart hello backup job.</span></span>

    ![Säkerhetskopiering nu knappen](./media/backup-azure-manage-vms/backup-now-check.png)

    <span data-ttu-id="b7185-188">hello säkerhetskopiering skapar en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="b7185-188">hello backup job creates a recovery point.</span></span> <span data-ttu-id="b7185-189">Hej Kvarhållningsintervall hello återställningspunkt är hello samma som Kvarhållningsintervall som angetts i hello principen som är associerad med hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b7185-189">hello retention range of hello recovery point is hello same as retention range specified in hello policy associated with hello virtual machine.</span></span> <span data-ttu-id="b7185-190">tootrack hello förloppet för hello jobbet i hello valvet instrumentpanelen, klicka på hello **säkerhetskopieringsjobb** panelen.</span><span class="sxs-lookup"><span data-stu-id="b7185-190">tootrack hello progress for hello job, in hello vault dashboard, click hello **Backup Jobs** tile.</span></span>  

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="b7185-191">Sluta skydda virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="b7185-191">Stop protecting virtual machines</span></span>
<span data-ttu-id="b7185-192">Om du väljer toostop skyddar en virtuell dator, tillfrågas du om du vill tooretain hello Återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="b7185-192">If you choose toostop protecting a virtual machine, you are asked if you want tooretain hello recovery points.</span></span> <span data-ttu-id="b7185-193">Det finns två sätt toostop skyddar virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="b7185-193">There are two ways toostop protecting virtual machines:</span></span>

* <span data-ttu-id="b7185-194">Stoppa alla framtida säkerhetskopieringsjobb och ta bort alla återställningspunkter, eller</span><span class="sxs-lookup"><span data-stu-id="b7185-194">stop all future backup jobs and delete all recovery points, or</span></span>
* <span data-ttu-id="b7185-195">Stoppa alla framtida säkerhetskopieringsjobb men lämna hello Återställningspunkter</span><span class="sxs-lookup"><span data-stu-id="b7185-195">stop all future backup jobs but leave hello recovery points</span></span> <br/>

<span data-ttu-id="b7185-196">Det finns en kostnad som är associerade med lämnar hello Återställningspunkter i lagringen.</span><span class="sxs-lookup"><span data-stu-id="b7185-196">There is a cost associated with leaving hello recovery points in storage.</span></span> <span data-ttu-id="b7185-197">Hello fördelen att lämna hello Återställningspunkter är dock kan du återställa virtuella hello senare, om så önskas.</span><span class="sxs-lookup"><span data-stu-id="b7185-197">However, hello benefit of leaving hello recovery points is you can restore hello virtual machine later, if desired.</span></span> <span data-ttu-id="b7185-198">Information om hello kostnaden för att lämna hello Återställningspunkter finns hello [prisinformationen](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="b7185-198">For information about hello cost of leaving hello recovery points, see hello  [pricing details](https://azure.microsoft.com/pricing/details/backup/).</span></span> <span data-ttu-id="b7185-199">Om du väljer toodelete alla återställningspunkter kan återställa du inte hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="b7185-199">If you choose toodelete all recovery points, you cannot restore hello virtual machine.</span></span>

<span data-ttu-id="b7185-200">toostop skydd för en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="b7185-200">toostop protection for a virtual machine:</span></span>

1. <span data-ttu-id="b7185-201">På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **stoppa säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="b7185-201">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Stop backup**.</span></span>

    ![Stoppa säkerhetskopiering](./media/backup-azure-manage-vms/stop-backup-button.png)

    <span data-ttu-id="b7185-203">hello stoppa säkerhetskopiering blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="b7185-203">hello Stop Backup blade opens.</span></span>

    ![Stoppa säkerhetskopiering bladet](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. <span data-ttu-id="b7185-205">På hello **stoppa säkerhetskopiering** bladet Välj om tooretain eller ta bort hello säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="b7185-205">On hello **Stop Backup** blade, choose whether tooretain or delete hello backup data.</span></span> <span data-ttu-id="b7185-206">hello information om innehåller information om ditt val.</span><span class="sxs-lookup"><span data-stu-id="b7185-206">hello information box provides details about your choice.</span></span>

    ![Stoppa skydd](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. <span data-ttu-id="b7185-208">Hoppa över toostep 4 om du har valt tooretain hello säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="b7185-208">If you chose tooretain hello backup data, skip toostep 4.</span></span> <span data-ttu-id="b7185-209">Om du väljer toodelete säkerhetskopieringsdata bekräfta som du vill använda toostop hello säkerhetskopieringsjobb och ta bort återställningspunkter hello - hello-typnamn för hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="b7185-209">If you chose toodelete backup data, confirm that you want toostop hello backup jobs and delete hello recovery points - type hello name of hello item.</span></span>

    ![Stoppa verifiering](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="b7185-211">Om du är osäker hello objektnamnet hovra över hello utropstecken tooview hello namn.</span><span class="sxs-lookup"><span data-stu-id="b7185-211">If you aren't sure of hello item name, hover over hello exclamation mark tooview hello name.</span></span> <span data-ttu-id="b7185-212">Hello namnet på hello objekt är också **stoppa säkerhetskopiering** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="b7185-212">Also, hello name of hello item is under **Stop Backup** at hello top of hello blade.</span></span>
4. <span data-ttu-id="b7185-213">Du kan också ange en **orsak** eller **kommentar**.</span><span class="sxs-lookup"><span data-stu-id="b7185-213">Optionally provide a **Reason** or **Comment**.</span></span>
5. <span data-ttu-id="b7185-214">toostop hello säkerhetskopieringsjobbet för hello aktuella objekt, klickar du på ![säkerhetskopiering stoppknappen](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span><span class="sxs-lookup"><span data-stu-id="b7185-214">toostop hello backup job for hello current item, click  ![Stop backup button](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span></span>

    <span data-ttu-id="b7185-215">Ett meddelande kan du vet hello säkerhetskopieringsjobb har stoppats.</span><span class="sxs-lookup"><span data-stu-id="b7185-215">A notification message lets you know hello backup jobs have been stopped.</span></span>

    ![Bekräfta stoppskydd](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a><span data-ttu-id="b7185-217">Återuppta skyddet av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b7185-217">Resume protection of a virtual machine</span></span>
<span data-ttu-id="b7185-218">Om hello **behålla säkerhetskopierade Data** alternativet valdes när skyddet för hello virtuella datorn har stoppats, så är det möjligt tooresume skydd.</span><span class="sxs-lookup"><span data-stu-id="b7185-218">If hello **Retain Backup Data** option was chosen when protection for hello virtual machine was stopped, then it is possible tooresume protection.</span></span> <span data-ttu-id="b7185-219">Om hello **ta bort säkerhetskopierade Data** alternativet har valts och sedan skyddet för hello virtuella datorn kan inte återuppta.</span><span class="sxs-lookup"><span data-stu-id="b7185-219">If hello **Delete Backup Data** option was chosen, then protection for hello virtual machine cannot resume.</span></span>

<span data-ttu-id="b7185-220">tooresume skydd för hello virtuell dator</span><span class="sxs-lookup"><span data-stu-id="b7185-220">tooresume protection for hello virtual machine</span></span>

1. <span data-ttu-id="b7185-221">På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **återuppta säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="b7185-221">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Resume backup**.</span></span>

    ![Återuppta skydd](./media/backup-azure-manage-vms/resume-backup-button.png)

    <span data-ttu-id="b7185-223">hello princip för säkerhetskopiering blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="b7185-223">hello Backup Policy blade opens.</span></span>

   > [!NOTE]
   > <span data-ttu-id="b7185-224">Du kan välja en annan princip än hello princip som virtuella datorn ursprungligen skyddades när skydda hello virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="b7185-224">When re-protecting hello virtual machine, you can choose a different policy than hello policy with which virtual machine was protected initially.</span></span>
   >
   >
2. <span data-ttu-id="b7185-225">Gör så hello i [hantera principer för säkerhetskopiering](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello princip för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="b7185-225">Follow hello steps in [Manage backup policies](backup-azure-manage-vms.md#manage-backup-policies) tooassign hello policy for hello virtual machine.</span></span>

    <span data-ttu-id="b7185-226">När hello säkerhetskopieringsprincip tillämpade toohello virtuell dator kan se du hello efter meddelande.</span><span class="sxs-lookup"><span data-stu-id="b7185-226">Once hello backup policy is applied toohello virtual machine, you see hello following message.</span></span>

    ![Har skyddat VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a><span data-ttu-id="b7185-228">Ta bort säkerhetskopierade data</span><span class="sxs-lookup"><span data-stu-id="b7185-228">Delete Backup data</span></span>
<span data-ttu-id="b7185-229">Du kan ta bort hello säkerhetskopierade data som är kopplade till en virtuell dator under hello **stoppa säkerhetskopiering** jobb, eller när som helst efter hello jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="b7185-229">You can delete hello backup data associated with a virtual machine during hello **Stop backup** job, or anytime after hello backup job has completed.</span></span> <span data-ttu-id="b7185-230">Det kan även vara bra toowait dagar eller veckor innan du tar bort hello Återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="b7185-230">It may even be beneficial toowait days or weeks before deleting hello recovery points.</span></span> <span data-ttu-id="b7185-231">Till skillnad från återställa återställningspunkter, när du tar bort säkerhetskopierade data, kan du välja specifika punkter toodelete.</span><span class="sxs-lookup"><span data-stu-id="b7185-231">Unlike restoring recovery points, when deleting backup data, you cannot choose specific recovery points toodelete.</span></span> <span data-ttu-id="b7185-232">Om du väljer toodelete dina säkerhetskopierade data kan du ta bort alla återställningspunkter som är associerade med hello-objektet.</span><span class="sxs-lookup"><span data-stu-id="b7185-232">If you choose toodelete your backup data, you delete all recovery points associated with hello item.</span></span>

<span data-ttu-id="b7185-233">hello följande procedur förutsätter hello säkerhetskopieringsjobbet för hello virtuella datorn har stoppats eller inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="b7185-233">hello following procedure assumes hello Backup job for hello virtual machine has been stopped or disabled.</span></span> <span data-ttu-id="b7185-234">När hello säkerhetskopieringsjobbet är inaktiverat hello **återuppta säkerhetskopiering** och **ta bort säkerhetskopiering** alternativ är tillgängliga i hello valvet objektet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="b7185-234">Once hello Backup job is disabled, hello **Resume backup** and **Delete backup** options are available in hello vault item dashboard.</span></span>

![Återuppta och ta bort knappar](./media/backup-azure-manage-vms/resume-delete-buttons.png)

<span data-ttu-id="b7185-236">toodelete säkerhetskopierade data på en virtuell dator med hello *säkerhetskopiera inaktiverats*:</span><span class="sxs-lookup"><span data-stu-id="b7185-236">toodelete backup data on a virtual machine with hello *Backup disabled*:</span></span>

1. <span data-ttu-id="b7185-237">På hello [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **ta bort säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="b7185-237">On hello [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Delete backup**.</span></span>

    ![VM-typ](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    <span data-ttu-id="b7185-239">Hej **ta bort säkerhetskopierade Data** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="b7185-239">hello **Delete Backup Data** blade opens.</span></span>

    ![VM-typ](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. <span data-ttu-id="b7185-241">Namn på hello hello objektet tooconfirm som du vill toodelete hello Återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="b7185-241">Type hello name of hello item tooconfirm you want toodelete hello recovery points.</span></span>

    ![Stoppa verifiering](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="b7185-243">Om du är osäker hello objektnamnet hovra över hello utropstecken tooview hello namn.</span><span class="sxs-lookup"><span data-stu-id="b7185-243">If you aren't sure of hello item name, hover over hello exclamation mark tooview hello name.</span></span> <span data-ttu-id="b7185-244">Hello namnet på hello objekt är också **ta bort säkerhetskopierade Data** hello överst i hello-bladet.</span><span class="sxs-lookup"><span data-stu-id="b7185-244">Also, hello name of hello item is under **Delete Backup Data** at hello top of hello blade.</span></span>
3. <span data-ttu-id="b7185-245">Du kan också ange en **orsak** eller **kommentar**.</span><span class="sxs-lookup"><span data-stu-id="b7185-245">Optionally provide a **Reason** or **Comment**.</span></span>
4. <span data-ttu-id="b7185-246">toodelete hello säkerhetskopierade data för hello aktuella objekt, klickar du på ![säkerhetskopiering stoppknappen](./media/backup-azure-manage-vms/delete-button.png)</span><span class="sxs-lookup"><span data-stu-id="b7185-246">toodelete hello backup data for hello current item, click  ![Stop backup button](./media/backup-azure-manage-vms/delete-button.png)</span></span>

    <span data-ttu-id="b7185-247">Ett meddelande kan du vet hello säkerhetskopierade data har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="b7185-247">A notification message lets you know hello backup data has been deleted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b7185-248">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b7185-248">Next steps</span></span>
<span data-ttu-id="b7185-249">Information om hur du skapar en virtuell dator från en återställningspunkt igen kolla [återställa virtuella Azure-datorer](backup-azure-restore-vms.md).</span><span class="sxs-lookup"><span data-stu-id="b7185-249">For information on re-creating a virtual machine from a recovery point, check out [Restore Azure VMs](backup-azure-restore-vms.md).</span></span> <span data-ttu-id="b7185-250">Om du behöver information om hur du skyddar virtuella datorer, se [förhandstitt: Säkerhetskopiera virtuella datorer tooa Recovery Services-valvet](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="b7185-250">If you need information on protecting your virtual machines, see [First look: Back up VMs tooa Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span> <span data-ttu-id="b7185-251">Information om övervakning av händelser finns [övervakar aviseringar för virtuell dator i Azure-säkerhetskopieringar](backup-azure-monitor-vms.md).</span><span class="sxs-lookup"><span data-stu-id="b7185-251">For information on monitoring events, see [Monitor alerts for Azure virtual machine backups](backup-azure-monitor-vms.md).</span></span>
