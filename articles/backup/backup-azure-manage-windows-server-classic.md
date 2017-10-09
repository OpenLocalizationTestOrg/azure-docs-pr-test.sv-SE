---
title: "aaaManage Azure Backup-valv och servrar Azure använder hello klassiska distributionsmodellen | Microsoft Docs"
description: "Använd den här självstudiekursen toolearn hur toomanage Azure Backup-valv och servrar."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: tysonn
ms.assetid: f175eb12-0905-437f-91fd-eaee03ab6e81
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/14/2017
ms.author: markgal;
ms.openlocfilehash: 6c38b04f4a76604bfd639a9b2d58237ce44e2386
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-hello-classic-deployment-model"></a><span data-ttu-id="bce8e-103">Hantera Azure-säkerhetskopieringsvalv och servrar med hjälp av hello klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="bce8e-103">Manage Azure Backup vaults and servers using hello classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="bce8e-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="bce8e-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="bce8e-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="bce8e-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="bce8e-106">I den här artikeln hittar du en översikt över hello säkerhetskopiering hanteringsuppgifter tillgängliga via hello klassiska Azure-portalen och hello Microsoft Azure Backup agent.</span><span class="sxs-lookup"><span data-stu-id="bce8e-106">In this article you'll find an overview of hello backup management tasks available through hello Azure classic portal and hello Microsoft Azure Backup agent.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bce8e-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="bce8e-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="bce8e-108">Den här artikeln täcker hello klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="bce8e-108">This article covers using hello Classic deployment model.</span></span> <span data-ttu-id="bce8e-109">Microsoft rekommenderar att de flesta nya distributioner använder hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="bce8e-109">Microsoft recommends that most new deployments use hello Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="bce8e-110">Nu kan du uppgradera ditt valv tooRecovery Services säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="bce8e-110">You can now upgrade your Backup vaults tooRecovery Services vaults.</span></span> <span data-ttu-id="bce8e-111">Mer information finns i artikeln hello [uppgradera en Backup-valvet tooa Recovery Services-valvet](backup-azure-upgrade-backup-to-recovery-services.md).</span><span class="sxs-lookup"><span data-stu-id="bce8e-111">For details, see hello article [Upgrade a Backup vault tooa Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="bce8e-112">Microsoft rekommenderar att du tooupgrade din säkerhetskopieringsvalv tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="bce8e-112">Microsoft encourages you tooupgrade your Backup vaults tooRecovery Services vaults.</span></span><br/> <span data-ttu-id="bce8e-113">Du kan inte använda PowerShell toocreate säkerhetskopieringsvalv efter 15 oktober 2017.</span><span class="sxs-lookup"><span data-stu-id="bce8e-113">After October 15, 2017, you can’t use PowerShell toocreate Backup vaults.</span></span> <span data-ttu-id="bce8e-114">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="bce8e-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="bce8e-115">Alla återstående säkerhetskopieringsvalv blir automatiskt uppgraderade tooRecovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="bce8e-115">All remaining Backup vaults will be automatically upgraded tooRecovery Services vaults.</span></span>
>- <span data-ttu-id="bce8e-116">Du kommer inte att kunna tooaccess dina säkerhetskopierade data i hello klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="bce8e-116">You won't be able tooaccess your backup data in hello classic portal.</span></span> <span data-ttu-id="bce8e-117">Använd i stället hello Azure portal tooaccess dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="bce8e-117">Instead, use hello Azure portal tooaccess your backup data in Recovery Services vaults.</span></span>
>

## <a name="management-portal-tasks"></a><span data-ttu-id="bce8e-118">Portalen hanteringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="bce8e-118">Management portal tasks</span></span>
1. <span data-ttu-id="bce8e-119">Logga in toohello [hanteringsportalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="bce8e-119">Sign in toohello [Management Portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="bce8e-120">Klicka på **återställningstjänster**, klickar sedan på säkerhetskopieringsvalvet tooview hello snabbstartsidan hello namn.</span><span class="sxs-lookup"><span data-stu-id="bce8e-120">Click **Recovery Services**, then click hello name of backup vault tooview hello Quick Start page.</span></span>

    ![Återställningstjänster](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

<span data-ttu-id="bce8e-122">Du kan se hello tillgängliga hanteringsuppgifter genom att välja hello alternativ hello överst i hello snabbstartsidan.</span><span class="sxs-lookup"><span data-stu-id="bce8e-122">By selecting hello options at hello top of hello Quick Start page, you can see hello available management tasks.</span></span>

![Hantera flikar](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a><span data-ttu-id="bce8e-124">Instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="bce8e-124">Dashboard</span></span>
<span data-ttu-id="bce8e-125">Välj **instrumentpanelen** toosee hello användningsöversikten för hello-servern.</span><span class="sxs-lookup"><span data-stu-id="bce8e-125">Select **Dashboard** toosee hello usage overview for hello server.</span></span> <span data-ttu-id="bce8e-126">Hej **användningsöversikten** omfattar:</span><span class="sxs-lookup"><span data-stu-id="bce8e-126">hello **usage overview** includes:</span></span>

* <span data-ttu-id="bce8e-127">hello antal Windows-servrar registrerade toocloud</span><span class="sxs-lookup"><span data-stu-id="bce8e-127">hello number of Windows Servers registered toocloud</span></span>
* <span data-ttu-id="bce8e-128">hello antalet virtuella datorer i Azure skyddad i molnet</span><span class="sxs-lookup"><span data-stu-id="bce8e-128">hello number of Azure virtual machines protected in cloud</span></span>
* <span data-ttu-id="bce8e-129">hello totalt lagringsutrymme som förbrukas i Azure</span><span class="sxs-lookup"><span data-stu-id="bce8e-129">hello total storage consumed in Azure</span></span>
* <span data-ttu-id="bce8e-130">hello status för senaste jobb</span><span class="sxs-lookup"><span data-stu-id="bce8e-130">hello status of recent jobs</span></span>

<span data-ttu-id="bce8e-131">Längst ned hello hello instrumentpanelen kan du utföra följande uppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="bce8e-131">At hello bottom of hello Dashboard you can perform hello following tasks:</span></span>

* <span data-ttu-id="bce8e-132">**Hantera certifikat** – om ett certifikat har använt tooregister hello server och sedan använda det här tooupdate hello certifikatet.</span><span class="sxs-lookup"><span data-stu-id="bce8e-132">**Manage certificate** - If a certificate was used tooregister hello server, then use this tooupdate hello certificate.</span></span> <span data-ttu-id="bce8e-133">Om du använder autentiseringsuppgifter för valv kan inte använda **hantera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-133">If you are using vault credentials, do not use **Manage certificate**.</span></span>
* <span data-ttu-id="bce8e-134">**Ta bort** -borttagningar hello aktuella säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="bce8e-134">**Delete** - Deletes hello current backup vault.</span></span> <span data-ttu-id="bce8e-135">Om ett säkerhetskopieringsvalv används inte längre kan radera du den toofree upp lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="bce8e-135">If a backup vault is no longer being used, you can delete it toofree up storage space.</span></span> <span data-ttu-id="bce8e-136">**Ta bort** aktiveras först när alla registrerade servrar har tagits bort från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="bce8e-136">**Delete** is only enabled after all registered servers have been deleted from hello vault.</span></span>

![Säkerhetskopiering instrumentpanelsuppgifter](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a><span data-ttu-id="bce8e-138">Registrerade artiklar</span><span class="sxs-lookup"><span data-stu-id="bce8e-138">Registered items</span></span>
<span data-ttu-id="bce8e-139">Välj **registrerade objekt** tooview hello namnen på hello-servrar som är registrerade toothis valvet.</span><span class="sxs-lookup"><span data-stu-id="bce8e-139">Select **Registered Items** tooview hello names of hello servers that are registered toothis vault.</span></span>

![Registrerade artiklar](./media/backup-azure-manage-windows-server-classic/registered-items.png)

<span data-ttu-id="bce8e-141">Hej **typen** filter som standard tooAzure virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="bce8e-141">hello **Type** filter defaults tooAzure Virtual Machine.</span></span> <span data-ttu-id="bce8e-142">tooview hello namnen på hello-servrar som är registrerade toothis valvet, Välj **Windows server** från hello nedrullningsbara meny.</span><span class="sxs-lookup"><span data-stu-id="bce8e-142">tooview hello names of hello servers that are registered toothis vault, select **Windows server** from hello drop down menu.</span></span>

<span data-ttu-id="bce8e-143">Härifrån kan du utföra följande uppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="bce8e-143">From here you can perform hello following tasks:</span></span>

* <span data-ttu-id="bce8e-144">**Tillåt omregistrering** – när det här alternativet väljs för en server som du kan använda hello **Registreringsguiden** i hello lokala Microsoft Azure Backup agent tooregister hello server med hello säkerhetskopieringsvalvet en gång .</span><span class="sxs-lookup"><span data-stu-id="bce8e-144">**Allow Re-registration** - When this option is selected for a server you can use hello **Registration Wizard** in hello on-premises Microsoft Azure Backup agent tooregister hello server with hello backup vault a second time.</span></span> <span data-ttu-id="bce8e-145">Du kan behöva toore-registret på grund av tooan fel i hello certifikat eller om en server som hade toobe återskapas.</span><span class="sxs-lookup"><span data-stu-id="bce8e-145">You might need toore-register due tooan error in hello certificate or if a server had toobe rebuilt.</span></span>
* <span data-ttu-id="bce8e-146">**Ta bort** -tar bort en server från hello säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="bce8e-146">**Delete** - Deletes a server from hello backup vault.</span></span> <span data-ttu-id="bce8e-147">Alla hello lagras data som associeras med hello servern tas bort omedelbart.</span><span class="sxs-lookup"><span data-stu-id="bce8e-147">All of hello stored data associated with hello server is deleted immediately.</span></span>

    ![Registrerade artiklar uppgifter](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a><span data-ttu-id="bce8e-149">Skyddade objekt</span><span class="sxs-lookup"><span data-stu-id="bce8e-149">Protected items</span></span>
<span data-ttu-id="bce8e-150">Välj **skyddade objekt** tooview hello objekt som har säkerhetskopierats från hello-servrar.</span><span class="sxs-lookup"><span data-stu-id="bce8e-150">Select **Protected Items** tooview hello items that have been backed up from hello servers.</span></span>

![Skyddade objekt](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a><span data-ttu-id="bce8e-152">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="bce8e-152">Configure</span></span>
<span data-ttu-id="bce8e-153">Från hello **konfigurera** fliken som du kan välja hello lämpliga redundans lagringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="bce8e-153">From hello **Configure** tab you can select hello appropriate storage redundancy option.</span></span> <span data-ttu-id="bce8e-154">hello visas bästa tooselect hello lagring redundans direkt när du har skapat ett valv och alla datorer som är registrerade tooit.</span><span class="sxs-lookup"><span data-stu-id="bce8e-154">hello best time tooselect hello storage redundancy option is right after creating a vault and before any machines are registered tooit.</span></span>

> [!WARNING]
> <span data-ttu-id="bce8e-155">När ett objekt har varit registrerade toohello valvet, hello lagringsalternativ för redundans är låst och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="bce8e-155">Once an item has been registered toohello vault, hello storage redundancy option is locked and cannot be modified.</span></span>
>
>

![Konfigurera](./media/backup-azure-manage-windows-server-classic/configure.png)

<span data-ttu-id="bce8e-157">Se den här artikeln för mer information om [lagring redundans](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="bce8e-157">See this article for more information about [storage redundancy](../storage/common/storage-redundancy.md).</span></span>

## <a name="microsoft-azure-backup-agent-tasks"></a><span data-ttu-id="bce8e-158">Microsoft Azure Backup agentuppgifter</span><span class="sxs-lookup"><span data-stu-id="bce8e-158">Microsoft Azure Backup agent tasks</span></span>
### <a name="console"></a><span data-ttu-id="bce8e-159">Konsolen</span><span class="sxs-lookup"><span data-stu-id="bce8e-159">Console</span></span>
<span data-ttu-id="bce8e-160">Öppna hello **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="bce8e-160">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Backup-agenten](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

<span data-ttu-id="bce8e-162">Från hello **åtgärder** på hello höger i hello säkerhetskopieringsagent konsolen kan du utföra följande hanteringsuppgifter hello:</span><span class="sxs-lookup"><span data-stu-id="bce8e-162">From hello **Actions** available at hello right of hello backup agent console you can perform hello following management tasks:</span></span>

* <span data-ttu-id="bce8e-163">Registrera Server</span><span class="sxs-lookup"><span data-stu-id="bce8e-163">Register Server</span></span>
* <span data-ttu-id="bce8e-164">Schema för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="bce8e-164">Schedule Backup</span></span>
* <span data-ttu-id="bce8e-165">Säkerhetskopiera nu</span><span class="sxs-lookup"><span data-stu-id="bce8e-165">Back Up now</span></span>
* <span data-ttu-id="bce8e-166">Ändra egenskaper för</span><span class="sxs-lookup"><span data-stu-id="bce8e-166">Change Properties</span></span>

![Konsolen agentåtgärder](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> <span data-ttu-id="bce8e-168">för**återställa Data**, se [återställa filer tooa Windows server eller Windows-klientdatorn](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="bce8e-168">too**Recover Data**, see [Restore files tooa Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

### <a name="modify-an-existing-backup"></a><span data-ttu-id="bce8e-169">Ändra en befintlig säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="bce8e-169">Modify an existing backup</span></span>
1. <span data-ttu-id="bce8e-170">Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-170">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. <span data-ttu-id="bce8e-172">I hello **schema Säkerhetskopieringsguiden** lämna hello **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-172">In hello **Schedule Backup Wizard** leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Ändra en schemalagd säkerhetskopiering](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="bce8e-174">Om du vill tooadd eller ändra objekt på hello **Välj objekt tooBackup** skärmen klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-174">If you want tooadd or change items, on hello **Select Items tooBackup** screen click **Add Items**.</span></span>

    <span data-ttu-id="bce8e-175">Du kan också ange **Undantagsinställningar** från den här sidan i guiden hello.</span><span class="sxs-lookup"><span data-stu-id="bce8e-175">You can also set **Exclusion Settings** from this page in hello wizard.</span></span> <span data-ttu-id="bce8e-176">Om du vill tooexclude filer eller filtyper läsa hello proceduren för att lägga till [undantagsinställningar](#exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="bce8e-176">If you want tooexclude files or file types read hello procedure for adding [exclusion settings](#exclusion-settings).</span></span>
4. <span data-ttu-id="bce8e-177">Välj hello filer och mappar som du vill att tooback och klicka på **okej**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-177">Select hello files and folders you want tooback up and click **Okay**.</span></span>

    ![Lägg till artiklar](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. <span data-ttu-id="bce8e-179">Ange hello **Säkerhetskopieringsschemat** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-179">Specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="bce8e-180">Du kan schemalägga varje dag (högst 3 gånger per dag) eller veckovisa säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="bce8e-180">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Ange schema för säkerhetskopiering](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="bce8e-182">Ange schemat för säkerhetskopiering av hello beskrivs i detalj i detta [artikel](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="bce8e-182">Specifying hello backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >
6. <span data-ttu-id="bce8e-183">Välj hello **bevarandeprincip** för hello säkerhetskopia och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-183">Select hello **Retention Policy** for hello backup copy and click **Next**.</span></span>

    ![Välj din bevarandeprincip](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. <span data-ttu-id="bce8e-185">På hello **bekräftelse** skärmen granska hello information och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-185">On hello **Confirmation** screen review hello information and click **Finish**.</span></span>
8. <span data-ttu-id="bce8e-186">När hello guiden är färdig med hello **Säkerhetskopieringsschemat**, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-186">Once hello wizard finishes creating hello **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="bce8e-187">Ändrar skydd måste du bekräfta att säkerhetskopieringar utlöser korrekt genom att gå toohello **jobb** fliken och bekräftar att ändringarna visas i hello säkerhetskopiera jobb.</span><span class="sxs-lookup"><span data-stu-id="bce8e-187">After modifying protection, you can confirm that backups are triggering correctly by going toohello **Jobs** tab and confirming that changes are reflected in hello backup jobs.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="bce8e-188">Aktivera begränsning</span><span class="sxs-lookup"><span data-stu-id="bce8e-188">Enable Network Throttling</span></span>
<span data-ttu-id="bce8e-189">hello Azure Backup-agenten ger en begränsning flik där du toocontrol hur nätverksbandbredden används när data överfördes.</span><span class="sxs-lookup"><span data-stu-id="bce8e-189">hello Azure Backup agent provides a Throttling tab which allows you toocontrol how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="bce8e-190">Den här kontrollen kan vara användbart om du behöver tooback upp data under arbetstid men inte vill att hello säkerhetskopieringsprocessen toointerfere med andra Internettrafik.</span><span class="sxs-lookup"><span data-stu-id="bce8e-190">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other internet traffic.</span></span> <span data-ttu-id="bce8e-191">Begränsning av data transfer gäller tooback upp och återställa aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="bce8e-191">Throttling of data transfer applies tooback up and restore activities.</span></span>  

<span data-ttu-id="bce8e-192">tooenable begränsning:</span><span class="sxs-lookup"><span data-stu-id="bce8e-192">tooenable throttling:</span></span>

1. <span data-ttu-id="bce8e-193">I hello **säkerhetskopieringsagenten**, klickar du på **ändra egenskaper för**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-193">In hello **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="bce8e-194">Välj hello **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="bce8e-194">Select hello **Enable internet bandwidth usage throttling for backup operations** checkbox.</span></span>

    ![Nätverksbegränsning](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. <span data-ttu-id="bce8e-196">När du har aktiverat begränsning, ange hello tillåtet bandbredd för överföring av säkerhetskopierade data under **arbetstid** och **icke-arbetstid**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-196">Once you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="bce8e-197">hello bandbredd värden börjar på 512 kilobit per sekund (Kbps) och gå upp too1023 megabyte per sekund (Mbps).</span><span class="sxs-lookup"><span data-stu-id="bce8e-197">hello bandwidth values begin at 512 kilobytes per second (Kbps) and can go up too1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="bce8e-198">Du kan också ange hello start och avslutas för **arbetstid**, och vilka dagar i veckan för hello betraktas arbete dagar.</span><span class="sxs-lookup"><span data-stu-id="bce8e-198">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered Work days.</span></span> <span data-ttu-id="bce8e-199">hello är utanför hello avses arbetstid övervägt toobe icke-arbetstid.</span><span class="sxs-lookup"><span data-stu-id="bce8e-199">hello time outside of hello designated Work hours is considered toobe non-work hours.</span></span>
4. <span data-ttu-id="bce8e-200">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-200">Click **OK**.</span></span>

## <a name="exclusion-settings"></a><span data-ttu-id="bce8e-201">Undantagsinställningar</span><span class="sxs-lookup"><span data-stu-id="bce8e-201">Exclusion settings</span></span>
1. <span data-ttu-id="bce8e-202">Öppna hello **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="bce8e-202">Open hello **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Öppna säkerhetskopieringsagent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. <span data-ttu-id="bce8e-204">Klicka på hello Microsoft Azure Backup agent **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-204">In hello Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. <span data-ttu-id="bce8e-206">Lämna hello i hello schema Säkerhetskopieringsguiden **göra ändringar toobackup objekt eller gånger** alternativ som valts och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-206">In hello Schedule Backup Wizard leave hello **Make changes toobackup items or times** option selected and click **Next**.</span></span>

    ![Ändra ett schema](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="bce8e-208">Klicka på **undantag inställningar**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-208">Click **Exclusions Settings**.</span></span>

    ![Välj objekt tooexclude](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. <span data-ttu-id="bce8e-210">Klicka på **Lägg till undantag**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-210">Click **Add Exclusion**.</span></span>

    ![Lägg till undantag](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. <span data-ttu-id="bce8e-212">Välj hello plats och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-212">Select hello location and then, click **OK**.</span></span>

    ![Välj en plats för undantag](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. <span data-ttu-id="bce8e-214">Lägga till filnamnstillägget hello i hello **filtyp** fältet.</span><span class="sxs-lookup"><span data-stu-id="bce8e-214">Add hello file extension in hello **File Type** field.</span></span>

    ![Exkludera efter filtyp](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    <span data-ttu-id="bce8e-216">Lägger till en .mp3-tillägg</span><span class="sxs-lookup"><span data-stu-id="bce8e-216">Adding an .mp3 extension</span></span>

    ![Exempel på en typ.](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    <span data-ttu-id="bce8e-218">tooadd ett annat filtillägg klickar du på **Lägg till undantag** och ange en annan filtyp (lägga till filnamnstillägget .jpeg).</span><span class="sxs-lookup"><span data-stu-id="bce8e-218">tooadd another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Exempel på en annan typ](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. <span data-ttu-id="bce8e-220">När du har lagt till alla hello-tillägg, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-220">When you've added all hello extensions, click **OK**.</span></span>
9. <span data-ttu-id="bce8e-221">Fortsätter med hello schema för Säkerhetskopieringsguiden genom att klicka på **nästa** tills hello **bekräftelsesidan**, klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="bce8e-221">Continue through hello Schedule Backup Wizard by clicking **Next** until hello **Confirmation page**, then click **Finish**.</span></span>

    ![Bekräftelse av undantag](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a><span data-ttu-id="bce8e-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="bce8e-223">Next steps</span></span>
* [<span data-ttu-id="bce8e-224">Återställa Windows Server eller Windows-klienten från Azure</span><span class="sxs-lookup"><span data-stu-id="bce8e-224">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="bce8e-225">toolearn mer om Azure Backup finns [översikt över Azure-säkerhetskopiering](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="bce8e-225">toolearn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="bce8e-226">Besök hello [Azure Backup-forumet](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="bce8e-226">Visit hello [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
