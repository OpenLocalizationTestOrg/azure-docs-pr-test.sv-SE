---
title: "Hantera Resource Manager distribuerade virtuella datorsäkerhetskopieringar | Microsoft Docs"
description: "Lär dig att hantera och övervaka säkerhetskopiering för Resource Manager distribuerade virtuella datorer"
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
ms.openlocfilehash: 35a21cb99ca4bad124a9f764cef9da453e1fe47f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="manage-azure-virtual-machine-backups"></a><span data-ttu-id="e9cf8-103">Hantera säkerhetskopiering av virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="e9cf8-103">Manage Azure virtual machine backups</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="e9cf8-104">Hantera Virtuella Azure-säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e9cf8-104">Manage Azure VM backups</span></span>](backup-azure-manage-vms.md)
> * [<span data-ttu-id="e9cf8-105">Hantera klassiska VM-säkerhetskopieringar</span><span class="sxs-lookup"><span data-stu-id="e9cf8-105">Manage Classic VM backups</span></span>](backup-azure-manage-vms-classic.md)
>
>

<span data-ttu-id="e9cf8-106">Den här artikeln innehåller information om hur du hanterar Virtuella säkerhetskopieringar och förklarar aviseringar om säkerhetskopiering informationen i portalens instrumentpanel.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-106">This article provides guidance on managing VM backups, and explains the backup alerts information available in the portal dashboard.</span></span> <span data-ttu-id="e9cf8-107">Riktlinjerna i den här artikeln gäller för virtuella datorer med Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-107">The guidance in this article applies to using VMs with Recovery Services vaults.</span></span> <span data-ttu-id="e9cf8-108">Den här artikeln täcker inte skapandet av virtuella datorer och inte heller har förklarar hur du skyddar virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-108">This article does not cover the creation of virtual machines, nor does it explain how to protect virtual machines.</span></span> <span data-ttu-id="e9cf8-109">En introduktion om hur du skyddar Azure Resource Manager distribuerade virtuella datorer i Azure med ett Recovery Services-valv finns [förhandstitt: Säkerhetskopiera virtuella datorer till ett Recovery Services-valv](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="e9cf8-109">For a primer on protecting Azure Resource Manager-deployed VMs in Azure with a Recovery Services vault, see [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span>

## <a name="manage-vaults-and-protected-virtual-machines"></a><span data-ttu-id="e9cf8-110">Hantera valv och skyddade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e9cf8-110">Manage vaults and protected virtual machines</span></span>
<span data-ttu-id="e9cf8-111">I Azure-portalen ger Recovery Services-valvet instrumentpanelen tillgång till information om valvet, inklusive:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-111">In the Azure portal, the Recovery Services vault dashboard provides access to information about the vault including:</span></span>

* <span data-ttu-id="e9cf8-112">den senaste ögonblicksbild, som också är den senaste återställningspunkten < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-112">the most recent backup snapshot, which is also the latest restore point <br\></span></span>
* <span data-ttu-id="e9cf8-113">säkerhetskopieringsprincipen < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-113">the backup policy <br\></span></span>
* <span data-ttu-id="e9cf8-114">Total storlek på alla ögonblicksbilder av säkerhetskopior < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-114">total size of all backup snapshots <br\></span></span>
* <span data-ttu-id="e9cf8-115">antal virtuella datorer som är skyddade med valvet < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-115">number of virtual machines that are protected with the vault <br\></span></span>

<span data-ttu-id="e9cf8-116">Många hanteringsuppgifter med en säkerhetskopiering av virtuella datorer som börjar med öppna valvet i instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-116">Many management tasks with a virtual machine backup begin with opening the vault in the dashboard.</span></span> <span data-ttu-id="e9cf8-117">Men eftersom valv kan användas för att skydda flera objekt (eller flera virtuella datorer), om du vill visa information om en viss virtuell dator, öppna instrumentpanelen valvet objektet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-117">However, because vaults can be used to protect multiple items (or multiple VMs), to view details about a particular VM, open the vault item dashboard.</span></span> <span data-ttu-id="e9cf8-118">Följande procedur visar hur du öppnar den *valvet instrumentpanelen* och fortsätt sedan till den *valvet objektet instrumentpanelen*.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-118">The following procedure shows you how to open the *vault dashboard* and then continue to the *vault item dashboard*.</span></span> <span data-ttu-id="e9cf8-119">Det finns ”tips” i båda procedurerna som pekar reda på hur du lägger till valvet och valvet objektet i Azure instrumentpanel med hjälp av PIN-kod för instrumentpanelen kommando.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-119">There are "tips" in both procedures that point out how to add the vault and vault item to the Azure dashboard by using the Pin to dashboard command.</span></span> <span data-ttu-id="e9cf8-120">Fäst på instrumentpanelen är ett sätt att skapa en genväg till valvet eller objekt.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-120">Pin to dashboard is a way of creating a shortcut to the vault or item.</span></span> <span data-ttu-id="e9cf8-121">Du kan också köra vanliga kommandon från genvägen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-121">You can also execute common commands from the shortcut.</span></span>

> [!TIP]
> <span data-ttu-id="e9cf8-122">Om du har flera instrumentpaneler och öppnar blad, Använd mörkt blå skjutreglaget längst ned i fönstret till bild Azure instrumentpanelen fram och tillbaka.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-122">If you have multiple dashboards and blades open, use the dark-blue slider at the bottom of the window to slide the Azure dashboard back and forth.</span></span>
>
>

![Fullständig vy med skjutreglaget](./media/backup-azure-manage-vms/bottom-slider.png)

### <a name="open-a-recovery-services-vault-in-the-dashboard"></a><span data-ttu-id="e9cf8-124">Öppna ett Recovery Services-valv i instrumentpanelen:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-124">Open a Recovery Services vault in the dashboard:</span></span>
1. <span data-ttu-id="e9cf8-125">Logga in på [Azure Portal](https://portal.azure.com/).</span><span class="sxs-lookup"><span data-stu-id="e9cf8-125">Sign in to the [Azure portal](https://portal.azure.com/).</span></span>
2. <span data-ttu-id="e9cf8-126">På navmenyn klickar du på **Bläddra** och i listan över resurser skriver du **Recovery Services**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-126">On the Hub menu, click **Browse** and in the list of resources, type **Recovery Services**.</span></span> <span data-ttu-id="e9cf8-127">När du börjar skriva filtreras listan baserat på det du skriver.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-127">As you begin typing, the list filters based on your input.</span></span> <span data-ttu-id="e9cf8-128">Klicka på **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-128">Click **Recovery Services vault**.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-azure-manage-vms/browse-to-rs-vaults.png) <br/>

    <span data-ttu-id="e9cf8-130">Listan över Recovery Services-valv visas.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-130">The list of Recovery Services vaults are displayed.</span></span>

    ![<span data-ttu-id="e9cf8-131">Lista över Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="e9cf8-131">List of Recovery Services vaults</span></span> ](./media/backup-azure-manage-vms/list-o-vaults.png) <br/>

   > [!TIP]
   > <span data-ttu-id="e9cf8-132">Om du fäster ett valv på instrumentpanelen i Azure är som valvet omedelbart tillgängligt när du öppnar den Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-132">If you pin a vault to the Azure Dashboard, that vault is immediately accessible when you open the Azure portal.</span></span> <span data-ttu-id="e9cf8-133">Högerklicka på valvet för att fästa ett valv på instrumentpanelen i listan med valvet, och välj **fäst på instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-133">To pin a vault to the dashboard, in the vault list, right-click the vault, and select **Pin to dashboard**.</span></span>
   >
   >
3. <span data-ttu-id="e9cf8-134">Välj i listan över valv valvet för att öppna dess instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-134">From the list of vaults, select the vault to open its dashboard.</span></span> <span data-ttu-id="e9cf8-135">När du väljer valvet, valvet instrumentpanelen och **inställningar** bladet som är öppna.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-135">When you select the vault, the vault dashboard and the **Settings** blade open.</span></span> <span data-ttu-id="e9cf8-136">I följande bild, den **Contoso valvet** instrumentpanelen är markerat.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-136">In the following image, the **Contoso-vault** dashboard is highlighted.</span></span>

    ![Öppna instrumentpanelen i valvet och inställningsbladet](./media/backup-azure-manage-vms/full-view-rs-vault.png)

### <a name="open-a-vault-item-dashboard"></a><span data-ttu-id="e9cf8-138">Öppna en valvet objektet instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="e9cf8-138">Open a vault item dashboard</span></span>
<span data-ttu-id="e9cf8-139">Du har öppnat valvet instrumentpanelen i föregående procedur.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-139">In the previous procedure you opened the vault dashboard.</span></span> <span data-ttu-id="e9cf8-140">Öppna instrumentpanelen valvet objektet:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-140">To open the vault item dashboard:</span></span>

1. <span data-ttu-id="e9cf8-141">I instrumentpanelen för valvet på den **säkerhetskopiering objekt** panelen, klickar du på **Azure Virtual Machines**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-141">In the vault dashboard, on the **Backup Items** tile, click **Azure Virtual Machines**.</span></span>

    ![Öppna Säkerhetskopiering poster sida vid sida](./media/backup-azure-manage-vms/contoso-vault-1606.png)

    <span data-ttu-id="e9cf8-143">Den **Säkerhetskopieringsobjekt** bladet visar en lista över senaste säkerhetskopieringsjobbet för varje objekt.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-143">The **Backup Items** blade lists the last backup job for each item.</span></span> <span data-ttu-id="e9cf8-144">I det här exemplet har en virtuell dator, demovm markgal skyddas av det här valvet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-144">In this example, there is one virtual machine, demovm-markgal, protected by this vault.</span></span>  

    ![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-vms/backup-items-blade.png)

   > [!TIP]
   > <span data-ttu-id="e9cf8-146">Du kan fästa ett valv objekt på instrumentpanelen i Azure för enkel åtkomst.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-146">For ease of access, you can pin a vault item to the Azure Dashboard.</span></span> <span data-ttu-id="e9cf8-147">Om du vill fästa ett valv objekt i listan över objekt valvet, högerklicka på objektet och välj **fäst på instrumentpanelen**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-147">To pin a vault item, in the vault item list, right-click the item and select **Pin to dashboard**.</span></span>
   >
   >
2. <span data-ttu-id="e9cf8-148">I den **säkerhetskopiering objekt** bladet, klickar du på objektet för att öppna instrumentpanelen valvet objektet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-148">In the **Backup Items** blade, click the item to open the vault item dashboard.</span></span>

    ![Säkerhetskopiera objekt sida vid sida](./media/backup-azure-manage-vms/backup-items-blade-select-item.png)

    <span data-ttu-id="e9cf8-150">Valvet objektet instrumentpanel och dess **inställningar** bladet som är öppna.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-150">The vault item dashboard and its **Settings** blade open.</span></span>

    ![Säkerhetskopiera objekt instrumentpanel med inställningsbladet](./media/backup-azure-manage-vms/item-dashboard-settings.png)

    <span data-ttu-id="e9cf8-152">Från instrumentpanelen valvet objekt, kan du utföra många viktiga hanteringsaktiviteter som:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-152">From the vault item dashboard, you can accomplish many key management tasks, such as:</span></span>

   * <span data-ttu-id="e9cf8-153">Ändra principer eller skapa en ny säkerhetskopieringsprincip < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-153">change policies or create a new backup policy<br\></span></span>
   * <span data-ttu-id="e9cf8-154">Visa återställningspunkter och se deras konsekvent tillstånd < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-154">view restore points, and see their consistency state <br\></span></span>
   * <span data-ttu-id="e9cf8-155">säkerhetskopiering på begäran för en virtuell dator < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-155">on-demand backup of a virtual machine <br\></span></span>
   * <span data-ttu-id="e9cf8-156">sluta skydda virtuella datorer < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-156">stop protecting virtual machines <br\></span></span>
   * <span data-ttu-id="e9cf8-157">återuppta skyddet av en virtuell dator < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-157">resume protection of a virtual machine <br\></span></span>
   * <span data-ttu-id="e9cf8-158">ta bort säkerhetskopierade data (eller återställningspunkt) < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-158">delete a backup data (or recovery point) <br\></span></span>
   * <span data-ttu-id="e9cf8-159">[återställa säkerhetskopior](backup-azure-arm-restore-vms.md#restore-backed-up-disks) < br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-159">[restore backup disks](backup-azure-arm-restore-vms.md#restore-backed-up-disks)  <br\></span></span>

<span data-ttu-id="e9cf8-160">Följande procedurer är startpunkten valvet objektet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-160">For the following procedures, the starting point is the vault item dashboard.</span></span>

## <a name="manage-backup-policies"></a><span data-ttu-id="e9cf8-161">Hantera säkerhetskopieringsprinciper</span><span class="sxs-lookup"><span data-stu-id="e9cf8-161">Manage backup policies</span></span>
1. <span data-ttu-id="e9cf8-162">På den [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **alla inställningar** att öppna den **inställningar** bladet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-162">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **All Settings** to open the **Settings** blade.</span></span>

    ![Princip för säkerhetskopiering bladet](./media/backup-azure-manage-vms/all-settings-button.png)
2. <span data-ttu-id="e9cf8-164">På den **inställningar** bladet, klickar du på **säkerhetskopiera princip** att öppna det bladet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-164">On the **Settings** blade, click **Backup policy** to open that blade.</span></span>

    <span data-ttu-id="e9cf8-165">På bladet visas säkerhetskopiering frekvens och kvarhållning intervallet information.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-165">On the blade, the backup frequency and retention range details are shown.</span></span>

    ![Princip för säkerhetskopiering bladet](./media/backup-azure-manage-vms/backup-policy-blade.png)
3. <span data-ttu-id="e9cf8-167">Från den **Välj säkerhetskopieringsprincip** menyn:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-167">From the **Choose backup policy** menu:</span></span>

   * <span data-ttu-id="e9cf8-168">Om du vill ändra principer, Välj en annan princip och klicka på **spara**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-168">To change policies, select a different policy and click **Save**.</span></span> <span data-ttu-id="e9cf8-169">Den nya principen tillämpas omedelbart på valvet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-169">The new policy is immediately applied to the vault.</span></span> <span data-ttu-id="e9cf8-170">< br\></span><span class="sxs-lookup"><span data-stu-id="e9cf8-170"><br\></span></span>
   * <span data-ttu-id="e9cf8-171">Om du vill skapa en princip, Välj **Skapa nytt**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-171">To create a policy, select **Create New**.</span></span>

     ![Säkerhetskopiering av virtuell dator](./media/backup-azure-manage-vms/backup-policy-create-new.png)

     <span data-ttu-id="e9cf8-173">Anvisningar om hur du skapar en princip för säkerhetskopiering finns [definierar en princip för säkerhetskopiering](backup-azure-manage-vms.md#defining-a-backup-policy).</span><span class="sxs-lookup"><span data-stu-id="e9cf8-173">For instructions on creating a backup policy, see [Defining a backup policy](backup-azure-manage-vms.md#defining-a-backup-policy).</span></span>

[!INCLUDE [backup-create-backup-policy-for-vm](../../includes/backup-create-backup-policy-for-vm.md)]

> [!NOTE]
> <span data-ttu-id="e9cf8-174">När du hanterar principer för säkerhetskopiering, se till att följa den [metodtips](backup-azure-vms-introduction.md#best-practices) för optimal prestanda vid säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="e9cf8-174">While managing backup policies, make sure to follow the [best practices](backup-azure-vms-introduction.md#best-practices) for optimal backup performance</span></span>
>
>

## <a name="on-demand-backup-of-a-virtual-machine"></a><span data-ttu-id="e9cf8-175">Säkerhetskopiering på begäran för en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e9cf8-175">On-demand backup of a virtual machine</span></span>
<span data-ttu-id="e9cf8-176">Du kan utföra en på-begäran säkerhetskopiering av en virtuell dator när den har konfigurerats för skydd.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-176">You can take an on-demand backup of a virtual machine once it is configured for protection.</span></span> <span data-ttu-id="e9cf8-177">Om den första säkerhetskopian väntar skapas säkerhetskopiering på begäran en fullständig kopia av den virtuella datorn i Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-177">If the initial backup is pending, on-demand backup creates a full copy of the virtual machine in the Recovery Services vault.</span></span> <span data-ttu-id="e9cf8-178">Om den första säkerhetskopieringen har slutförts, skickas en säkerhetskopiering på begäran endast ändringar från tidigare ögonblicksbild till Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-178">If the initial backup is completed, an on-demand backup will only send changes from the previous snapshot, to the Recovery Services vault.</span></span> <span data-ttu-id="e9cf8-179">Efterföljande säkerhetskopieringar är som är alltid inkrementellt.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-179">That is, subsequent backups are always incremental.</span></span>

> [!NOTE]
> <span data-ttu-id="e9cf8-180">Kvarhållningsintervall för en säkerhetskopiering på begäran är kvarhållning det angivna värdet för daglig säkerhetskopieringspunkt i principen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-180">The retention range for an on-demand backup is the retention value specified for the Daily backup point in the policy.</span></span> <span data-ttu-id="e9cf8-181">Om ingen daglig säkerhetskopieringspunkt är markerad används veckovis säkerhetskopieringspunkt.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-181">If no Daily backup point is selected, then the weekly backup point is used.</span></span>
>
>

<span data-ttu-id="e9cf8-182">Starta en säkerhetskopiering på begäran för en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-182">To trigger an on-demand backup of a virtual machine:</span></span>

* <span data-ttu-id="e9cf8-183">På den [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **Säkerhetskopiera nu**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-183">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Backup now**.</span></span>

    ![Säkerhetskopiering nu knappen](./media/backup-azure-manage-vms/backup-now-button.png)

    <span data-ttu-id="e9cf8-185">Portalen ser till att du vill starta en säkerhetskopiering på begäran.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-185">The portal makes sure that you want to start an on-demand backup job.</span></span> <span data-ttu-id="e9cf8-186">Klicka på **Ja** att starta jobbet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-186">Click **Yes** to start the backup job.</span></span>

    ![Säkerhetskopiering nu knappen](./media/backup-azure-manage-vms/backup-now-check.png)

    <span data-ttu-id="e9cf8-188">Säkerhetskopieringsjobbet skapar en återställningspunkt.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-188">The backup job creates a recovery point.</span></span> <span data-ttu-id="e9cf8-189">Kvarhållningsintervall för återställningspunkten är detsamma som Kvarhållningsintervall som anges i principen som är associerad med den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-189">The retention range of the recovery point is the same as retention range specified in the policy associated with the virtual machine.</span></span> <span data-ttu-id="e9cf8-190">Om du vill spåra förloppet för jobbet på valvet instrumentpanelen klickar du på den **säkerhetskopieringsjobb** panelen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-190">To track the progress for the job, in the vault dashboard, click the **Backup Jobs** tile.</span></span>  

## <a name="stop-protecting-virtual-machines"></a><span data-ttu-id="e9cf8-191">Sluta skydda virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="e9cf8-191">Stop protecting virtual machines</span></span>
<span data-ttu-id="e9cf8-192">Om du vill sluta skydda en virtuell dator, tillfrågas du om du vill behålla återställningspunkterna.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-192">If you choose to stop protecting a virtual machine, you are asked if you want to retain the recovery points.</span></span> <span data-ttu-id="e9cf8-193">Det finns två sätt att sluta skydda virtuella datorer:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-193">There are two ways to stop protecting virtual machines:</span></span>

* <span data-ttu-id="e9cf8-194">Stoppa alla framtida säkerhetskopieringsjobb och ta bort alla återställningspunkter, eller</span><span class="sxs-lookup"><span data-stu-id="e9cf8-194">stop all future backup jobs and delete all recovery points, or</span></span>
* <span data-ttu-id="e9cf8-195">Stoppa alla framtida säkerhetskopieringsjobb men lämna kvar återställningspunkter</span><span class="sxs-lookup"><span data-stu-id="e9cf8-195">stop all future backup jobs but leave the recovery points</span></span> <br/>

<span data-ttu-id="e9cf8-196">Det finns en kostnad som är associerade med lämnar återställningspunkter i lagringen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-196">There is a cost associated with leaving the recovery points in storage.</span></span> <span data-ttu-id="e9cf8-197">Fördelen med att lämna återställningspunkterna är dock kan du återställa den virtuella datorn senare, om så önskas.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-197">However, the benefit of leaving the recovery points is you can restore the virtual machine later, if desired.</span></span> <span data-ttu-id="e9cf8-198">Information om kostnaden för att lämna återställningspunkterna finns på [prisinformationen](https://azure.microsoft.com/pricing/details/backup/).</span><span class="sxs-lookup"><span data-stu-id="e9cf8-198">For information about the cost of leaving the recovery points, see the  [pricing details](https://azure.microsoft.com/pricing/details/backup/).</span></span> <span data-ttu-id="e9cf8-199">Om du vill ta bort alla återställningspunkter kan återställa du inte den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-199">If you choose to delete all recovery points, you cannot restore the virtual machine.</span></span>

<span data-ttu-id="e9cf8-200">Ta bort skyddet för en virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-200">To stop protection for a virtual machine:</span></span>

1. <span data-ttu-id="e9cf8-201">På den [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **stoppa säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-201">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Stop backup**.</span></span>

    ![Stoppa säkerhetskopiering](./media/backup-azure-manage-vms/stop-backup-button.png)

    <span data-ttu-id="e9cf8-203">Stoppa säkerhetskopiering blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-203">The Stop Backup blade opens.</span></span>

    ![Stoppa säkerhetskopiering bladet](./media/backup-azure-manage-vms/stop-backup-blade.png)
2. <span data-ttu-id="e9cf8-205">På den **stoppa säkerhetskopiering** bladet Välj om du vill behålla eller ta bort säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-205">On the **Stop Backup** blade, choose whether to retain or delete the backup data.</span></span> <span data-ttu-id="e9cf8-206">Informationsrutan innehåller information om ditt val.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-206">The information box provides details about your choice.</span></span>

    ![Stoppa skydd](./media/backup-azure-manage-vms/retain-or-delete-option.png)
3. <span data-ttu-id="e9cf8-208">Om du valde att behålla säkerhetskopierade data vidare till steg 4.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-208">If you chose to retain the backup data, skip to step 4.</span></span> <span data-ttu-id="e9cf8-209">Om du har valt att ta bort säkerhetskopieringsdata, bekräfta att du vill stoppa alla säkerhetskopieringsjobb och ta bort återställningspunkter - skriver du namnet på objektet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-209">If you chose to delete backup data, confirm that you want to stop the backup jobs and delete the recovery points - type the name of the item.</span></span>

    ![Stoppa verifiering](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="e9cf8-211">Om du är osäker objektnamnet hovra över utropstecken att visa namnet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-211">If you aren't sure of the item name, hover over the exclamation mark to view the name.</span></span> <span data-ttu-id="e9cf8-212">Namnet på objektet är också **stoppa säkerhetskopiering** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-212">Also, the name of the item is under **Stop Backup** at the top of the blade.</span></span>
4. <span data-ttu-id="e9cf8-213">Du kan också ange en **orsak** eller **kommentar**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-213">Optionally provide a **Reason** or **Comment**.</span></span>
5. <span data-ttu-id="e9cf8-214">Säkerhetskopieringsjobbet för det aktuella objektet klickar du på ![säkerhetskopiering stoppknappen](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span><span class="sxs-lookup"><span data-stu-id="e9cf8-214">To stop the backup job for the current item, click  ![Stop backup button](./media/backup-azure-manage-vms/stop-backup-button-blue.png)</span></span>

    <span data-ttu-id="e9cf8-215">Ett meddelande kan du vet att säkerhetskopiera jobb har stoppats.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-215">A notification message lets you know the backup jobs have been stopped.</span></span>

    ![Bekräfta stoppskydd](./media/backup-azure-manage-vms/stop-message.png)

## <a name="resume-protection-of-a-virtual-machine"></a><span data-ttu-id="e9cf8-217">Återuppta skyddet av en virtuell dator</span><span class="sxs-lookup"><span data-stu-id="e9cf8-217">Resume protection of a virtual machine</span></span>
<span data-ttu-id="e9cf8-218">Om den **behålla säkerhetskopierade Data** alternativet valdes när skyddet för den virtuella datorn har stoppats, så är det möjligt att skyddet ska återupptas.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-218">If the **Retain Backup Data** option was chosen when protection for the virtual machine was stopped, then it is possible to resume protection.</span></span> <span data-ttu-id="e9cf8-219">Om den **ta bort säkerhetskopierade Data** alternativet har valts och sedan skyddet för den virtuella datorn kan inte återuppta.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-219">If the **Delete Backup Data** option was chosen, then protection for the virtual machine cannot resume.</span></span>

<span data-ttu-id="e9cf8-220">Att återuppta skydd för den virtuella datorn</span><span class="sxs-lookup"><span data-stu-id="e9cf8-220">To resume protection for the virtual machine</span></span>

1. <span data-ttu-id="e9cf8-221">På den [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **återuppta säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-221">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Resume backup**.</span></span>

    ![Återuppta skydd](./media/backup-azure-manage-vms/resume-backup-button.png)

    <span data-ttu-id="e9cf8-223">Princip för säkerhetskopiering blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-223">The Backup Policy blade opens.</span></span>

   > [!NOTE]
   > <span data-ttu-id="e9cf8-224">Du kan välja en annan princip än principen som virtuella datorn ursprungligen skyddades när skydda den virtuella datorn igen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-224">When re-protecting the virtual machine, you can choose a different policy than the policy with which virtual machine was protected initially.</span></span>
   >
   >
2. <span data-ttu-id="e9cf8-225">Följ stegen i [hantera principer för säkerhetskopiering](backup-azure-manage-vms.md#manage-backup-policies) tilldela principen för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-225">Follow the steps in [Manage backup policies](backup-azure-manage-vms.md#manage-backup-policies) to assign the policy for the virtual machine.</span></span>

    <span data-ttu-id="e9cf8-226">När säkerhetskopieringsprincipen som används för den virtuella datorn, visas följande meddelande.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-226">Once the backup policy is applied to the virtual machine, you see the following message.</span></span>

    ![Har skyddat VM](./media/backup-azure-manage-vms/success-message.png)

## <a name="delete-backup-data"></a><span data-ttu-id="e9cf8-228">Ta bort säkerhetskopierade data</span><span class="sxs-lookup"><span data-stu-id="e9cf8-228">Delete Backup data</span></span>
<span data-ttu-id="e9cf8-229">Du kan ta bort den säkerhetskopiera informationen som är kopplad till en virtuell dator under den **stoppa säkerhetskopiering** jobb, eller när som helst efter säkerhetskopieringen jobbet har slutförts.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-229">You can delete the backup data associated with a virtual machine during the **Stop backup** job, or anytime after the backup job has completed.</span></span> <span data-ttu-id="e9cf8-230">Det kan även vara bra att vänta dagar eller veckor innan du tar bort återställningspunkterna.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-230">It may even be beneficial to wait days or weeks before deleting the recovery points.</span></span> <span data-ttu-id="e9cf8-231">Till skillnad från återställa återställningspunkter, när du tar bort säkerhetskopierade data, kan du välja specifika återställningspunkterna för att ta bort.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-231">Unlike restoring recovery points, when deleting backup data, you cannot choose specific recovery points to delete.</span></span> <span data-ttu-id="e9cf8-232">Om du väljer att ta bort dina säkerhetskopierade data kan du ta bort alla återställningspunkter som är associerad med objektet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-232">If you choose to delete your backup data, you delete all recovery points associated with the item.</span></span>

<span data-ttu-id="e9cf8-233">Följande procedur förutsätter att jobbet för den virtuella datorn har stoppats eller inaktiverats.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-233">The following procedure assumes the Backup job for the virtual machine has been stopped or disabled.</span></span> <span data-ttu-id="e9cf8-234">När jobbet som är inaktiverat på **återuppta säkerhetskopiering** och **ta bort säkerhetskopiering** alternativ är tillgängliga i valvet objektet instrumentpanelen.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-234">Once the Backup job is disabled, the **Resume backup** and **Delete backup** options are available in the vault item dashboard.</span></span>

![Återuppta och ta bort knappar](./media/backup-azure-manage-vms/resume-delete-buttons.png)

<span data-ttu-id="e9cf8-236">Ta bort säkerhetskopieringsdata på en virtuell dator med den *säkerhetskopiera inaktiverats*:</span><span class="sxs-lookup"><span data-stu-id="e9cf8-236">To delete backup data on a virtual machine with the *Backup disabled*:</span></span>

1. <span data-ttu-id="e9cf8-237">På den [valvet objektet instrumentpanelen](backup-azure-manage-vms.md#open-a-vault-item-dashboard), klickar du på **ta bort säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-237">On the [vault item dashboard](backup-azure-manage-vms.md#open-a-vault-item-dashboard), click **Delete backup**.</span></span>

    ![VM-typ](./media/backup-azure-manage-vms/delete-backup-buttom.png)

    <span data-ttu-id="e9cf8-239">Den **ta bort säkerhetskopierade Data** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-239">The **Delete Backup Data** blade opens.</span></span>

    ![VM-typ](./media/backup-azure-manage-vms/delete-backup-blade.png)
2. <span data-ttu-id="e9cf8-241">Skriv namnet på objektet du vill bekräfta att du vill ta bort återställningspunkter.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-241">Type the name of the item to confirm you want to delete the recovery points.</span></span>

    ![Stoppa verifiering](./media/backup-azure-manage-vms/item-verification-box.png)

    <span data-ttu-id="e9cf8-243">Om du är osäker objektnamnet hovra över utropstecken att visa namnet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-243">If you aren't sure of the item name, hover over the exclamation mark to view the name.</span></span> <span data-ttu-id="e9cf8-244">Namnet på objektet är också **ta bort säkerhetskopierade Data** längst upp på bladet.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-244">Also, the name of the item is under **Delete Backup Data** at the top of the blade.</span></span>
3. <span data-ttu-id="e9cf8-245">Du kan också ange en **orsak** eller **kommentar**.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-245">Optionally provide a **Reason** or **Comment**.</span></span>
4. <span data-ttu-id="e9cf8-246">Ta bort säkerhetskopierade data för det aktuella objektet, klicka på ![säkerhetskopiering stoppknappen](./media/backup-azure-manage-vms/delete-button.png)</span><span class="sxs-lookup"><span data-stu-id="e9cf8-246">To delete the backup data for the current item, click  ![Stop backup button](./media/backup-azure-manage-vms/delete-button.png)</span></span>

    <span data-ttu-id="e9cf8-247">Ett meddelande får du reda på den säkerhetskopiera informationen har tagits bort.</span><span class="sxs-lookup"><span data-stu-id="e9cf8-247">A notification message lets you know the backup data has been deleted.</span></span>

## <a name="next-steps"></a><span data-ttu-id="e9cf8-248">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e9cf8-248">Next steps</span></span>
<span data-ttu-id="e9cf8-249">Information om hur du skapar en virtuell dator från en återställningspunkt igen kolla [återställa virtuella Azure-datorer](backup-azure-restore-vms.md).</span><span class="sxs-lookup"><span data-stu-id="e9cf8-249">For information on re-creating a virtual machine from a recovery point, check out [Restore Azure VMs](backup-azure-restore-vms.md).</span></span> <span data-ttu-id="e9cf8-250">Om du behöver information om hur du skyddar virtuella datorer, se [förhandstitt: Säkerhetskopiera virtuella datorer till ett Recovery Services-valv](backup-azure-vms-first-look-arm.md).</span><span class="sxs-lookup"><span data-stu-id="e9cf8-250">If you need information on protecting your virtual machines, see [First look: Back up VMs to a Recovery Services vault](backup-azure-vms-first-look-arm.md).</span></span> <span data-ttu-id="e9cf8-251">Information om övervakning av händelser finns [övervakar aviseringar för virtuell dator i Azure-säkerhetskopieringar](backup-azure-monitor-vms.md).</span><span class="sxs-lookup"><span data-stu-id="e9cf8-251">For information on monitoring events, see [Monitor alerts for Azure virtual machine backups](backup-azure-monitor-vms.md).</span></span>
