---
title: "Hantera Azure-säkerhetskopieringsvalv och servrar Azure med hjälp av den klassiska distributionsmodellen | Microsoft Docs"
description: "Använd den här självstudiekursen för att lära dig att hantera Azure-säkerhetskopieringsvalv och servrar."
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
ms.openlocfilehash: 91451b2cdc42ed05ef7c1ba9c66ad5b4b45dd788
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="manage-azure-backup-vaults-and-servers-using-the-classic-deployment-model"></a><span data-ttu-id="61e35-103">Hantera Azure Backup Vault och servrar med hjälp av den klassiska distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="61e35-103">Manage Azure Backup vaults and servers using the classic deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="61e35-104">Resource Manager</span><span class="sxs-lookup"><span data-stu-id="61e35-104">Resource Manager</span></span>](backup-azure-manage-windows-server.md)
> * [<span data-ttu-id="61e35-105">Klassisk</span><span class="sxs-lookup"><span data-stu-id="61e35-105">Classic</span></span>](backup-azure-manage-windows-server-classic.md)
>
>

<span data-ttu-id="61e35-106">I den här artikeln hittar du en översikt över säkerhetskopiering hanteringsuppgifter som är tillgängliga via den klassiska Azure-portalen och Microsoft Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="61e35-106">In this article you'll find an overview of the backup management tasks available through the Azure classic portal and the Microsoft Azure Backup agent.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61e35-107">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="61e35-107">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="61e35-108">Den här artikeln täcker den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="61e35-108">This article covers using the Classic deployment model.</span></span> <span data-ttu-id="61e35-109">Microsoft rekommenderar att de flesta nya distributioner använder Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="61e35-109">Microsoft recommends that most new deployments use the Resource Manager model.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="61e35-110">Nu kan du uppgradera dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="61e35-110">You can now upgrade your Backup vaults to Recovery Services vaults.</span></span> <span data-ttu-id="61e35-111">Mer information finns i artikeln [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md) (Uppgradera ett säkerhetskopieringsvalv till ett Recovery Services-valv).</span><span class="sxs-lookup"><span data-stu-id="61e35-111">For details, see the article [Upgrade a Backup vault to a Recovery Services vault](backup-azure-upgrade-backup-to-recovery-services.md).</span></span> <span data-ttu-id="61e35-112">Microsoft rekommenderar att du uppgraderar dina säkerhetskopieringsvalv till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="61e35-112">Microsoft encourages you to upgrade your Backup vaults to Recovery Services vaults.</span></span><br/> <span data-ttu-id="61e35-113">Efter den 15 oktober 2017 kan du inte längre använda PowerShell för att skapa säkerhetskopieringsvalv.</span><span class="sxs-lookup"><span data-stu-id="61e35-113">After October 15, 2017, you can’t use PowerShell to create Backup vaults.</span></span> <span data-ttu-id="61e35-114">**Från den 1 november 2017**:</span><span class="sxs-lookup"><span data-stu-id="61e35-114">**By November 1, 2017**:</span></span>
>- <span data-ttu-id="61e35-115">Alla återstående säkerhetskopieringsvalv uppgraderas automatiskt till Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="61e35-115">All remaining Backup vaults will be automatically upgraded to Recovery Services vaults.</span></span>
>- <span data-ttu-id="61e35-116">Du kan inte längre komma åt dina säkerhetskopierade data i den klassiska portalen.</span><span class="sxs-lookup"><span data-stu-id="61e35-116">You won't be able to access your backup data in the classic portal.</span></span> <span data-ttu-id="61e35-117">Använd i stället Azure Portal till att få åtkomst till dina säkerhetskopierade data i Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="61e35-117">Instead, use the Azure portal to access your backup data in Recovery Services vaults.</span></span>
>

## <a name="management-portal-tasks"></a><span data-ttu-id="61e35-118">Portalen hanteringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="61e35-118">Management portal tasks</span></span>
1. <span data-ttu-id="61e35-119">Logga in på den [hanteringsportalen](https://manage.windowsazure.com).</span><span class="sxs-lookup"><span data-stu-id="61e35-119">Sign in to the [Management Portal](https://manage.windowsazure.com).</span></span>
2. <span data-ttu-id="61e35-120">Klicka på **återställningstjänster**, klicka på namnet på valvet för att visa sidan Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="61e35-120">Click **Recovery Services**, then click the name of backup vault to view the Quick Start page.</span></span>

    ![Återställningstjänster](./media/backup-azure-manage-windows-server-classic/rs-left-nav.png)

<span data-ttu-id="61e35-122">Du kan se de tillgängliga hanteringsuppgifterna genom att välja alternativen överst på sidan Snabbstart.</span><span class="sxs-lookup"><span data-stu-id="61e35-122">By selecting the options at the top of the Quick Start page, you can see the available management tasks.</span></span>

![Hantera flikar](./media/backup-azure-manage-windows-server-classic/qs-page.png)

### <a name="dashboard"></a><span data-ttu-id="61e35-124">Instrumentpanel</span><span class="sxs-lookup"><span data-stu-id="61e35-124">Dashboard</span></span>
<span data-ttu-id="61e35-125">Välj **instrumentpanelen** att se en översikt för användning för servern.</span><span class="sxs-lookup"><span data-stu-id="61e35-125">Select **Dashboard** to see the usage overview for the server.</span></span> <span data-ttu-id="61e35-126">Den **användningsöversikten** omfattar:</span><span class="sxs-lookup"><span data-stu-id="61e35-126">The **usage overview** includes:</span></span>

* <span data-ttu-id="61e35-127">Antal Windows-servrar som är registrerade i molnet</span><span class="sxs-lookup"><span data-stu-id="61e35-127">The number of Windows Servers registered to cloud</span></span>
* <span data-ttu-id="61e35-128">Antalet virtuella datorer i Azure skyddad i molnet</span><span class="sxs-lookup"><span data-stu-id="61e35-128">The number of Azure virtual machines protected in cloud</span></span>
* <span data-ttu-id="61e35-129">Det totala lagringsutrymmet som används i Azure</span><span class="sxs-lookup"><span data-stu-id="61e35-129">The total storage consumed in Azure</span></span>
* <span data-ttu-id="61e35-130">Status för senaste jobb</span><span class="sxs-lookup"><span data-stu-id="61e35-130">The status of recent jobs</span></span>

<span data-ttu-id="61e35-131">Längst ned i instrumentpanelen kan du utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="61e35-131">At the bottom of the Dashboard you can perform the following tasks:</span></span>

* <span data-ttu-id="61e35-132">**Hantera certifikat** – om ett certifikat som användes för att registrera servern och sedan använda detta för att uppdatera certifikatet.</span><span class="sxs-lookup"><span data-stu-id="61e35-132">**Manage certificate** - If a certificate was used to register the server, then use this to update the certificate.</span></span> <span data-ttu-id="61e35-133">Om du använder autentiseringsuppgifter för valv kan inte använda **hantera certifikat**.</span><span class="sxs-lookup"><span data-stu-id="61e35-133">If you are using vault credentials, do not use **Manage certificate**.</span></span>
* <span data-ttu-id="61e35-134">**Ta bort** -tar bort det aktuella säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="61e35-134">**Delete** - Deletes the current backup vault.</span></span> <span data-ttu-id="61e35-135">Om ett säkerhetskopieringsvalv används inte längre kan du ta bort den att frigöra lagringsutrymme.</span><span class="sxs-lookup"><span data-stu-id="61e35-135">If a backup vault is no longer being used, you can delete it to free up storage space.</span></span> <span data-ttu-id="61e35-136">**Ta bort** aktiveras först när alla registrerade servrar har tagits bort från valvet.</span><span class="sxs-lookup"><span data-stu-id="61e35-136">**Delete** is only enabled after all registered servers have been deleted from the vault.</span></span>

![Säkerhetskopiering instrumentpanelsuppgifter](./media/backup-azure-manage-windows-server-classic/dashboard-tasks.png)

## <a name="registered-items"></a><span data-ttu-id="61e35-138">Registrerade artiklar</span><span class="sxs-lookup"><span data-stu-id="61e35-138">Registered items</span></span>
<span data-ttu-id="61e35-139">Välj **registrerade objekt** att visa namnen på de servrar som är registrerade för det här valvet.</span><span class="sxs-lookup"><span data-stu-id="61e35-139">Select **Registered Items** to view the names of the servers that are registered to this vault.</span></span>

![Registrerade artiklar](./media/backup-azure-manage-windows-server-classic/registered-items.png)

<span data-ttu-id="61e35-141">Den **typen** filtrera standardvärden för virtuell dator i Azure.</span><span class="sxs-lookup"><span data-stu-id="61e35-141">The **Type** filter defaults to Azure Virtual Machine.</span></span> <span data-ttu-id="61e35-142">Om du vill visa namnen på de servrar som är registrerade för valvet, Välj **Windows server** från nedrullningsbara menyn.</span><span class="sxs-lookup"><span data-stu-id="61e35-142">To view the names of the servers that are registered to this vault, select **Windows server** from the drop down menu.</span></span>

<span data-ttu-id="61e35-143">Härifrån kan du utföra följande uppgifter:</span><span class="sxs-lookup"><span data-stu-id="61e35-143">From here you can perform the following tasks:</span></span>

* <span data-ttu-id="61e35-144">**Tillåt omregistrering** – när det här alternativet väljs för en server som du kan använda den **Registreringsguiden** i Microsoft Azure Backup agent lokalt för att registrera servern med backup valvet en andra gång.</span><span class="sxs-lookup"><span data-stu-id="61e35-144">**Allow Re-registration** - When this option is selected for a server you can use the **Registration Wizard** in the on-premises Microsoft Azure Backup agent to register the server with the backup vault a second time.</span></span> <span data-ttu-id="61e35-145">Du kan behöva registrera igen på grund av ett fel i certifikatet eller om en server som var tvungen att återskapas.</span><span class="sxs-lookup"><span data-stu-id="61e35-145">You might need to re-register due to an error in the certificate or if a server had to be rebuilt.</span></span>
* <span data-ttu-id="61e35-146">**Ta bort** -tar bort en server från säkerhetskopieringsvalvet.</span><span class="sxs-lookup"><span data-stu-id="61e35-146">**Delete** - Deletes a server from the backup vault.</span></span> <span data-ttu-id="61e35-147">Alla lagrade data som är associerade med servern tas bort omedelbart.</span><span class="sxs-lookup"><span data-stu-id="61e35-147">All of the stored data associated with the server is deleted immediately.</span></span>

    ![Registrerade artiklar uppgifter](./media/backup-azure-manage-windows-server-classic/registered-items-tasks.png)

## <a name="protected-items"></a><span data-ttu-id="61e35-149">Skyddade objekt</span><span class="sxs-lookup"><span data-stu-id="61e35-149">Protected items</span></span>
<span data-ttu-id="61e35-150">Välj **skyddade objekt** att visa objekt som har säkerhetskopierats från servrar.</span><span class="sxs-lookup"><span data-stu-id="61e35-150">Select **Protected Items** to view the items that have been backed up from the servers.</span></span>

![Skyddade objekt](./media/backup-azure-manage-windows-server-classic/protected-items.png)

## <a name="configure"></a><span data-ttu-id="61e35-152">Konfigurera</span><span class="sxs-lookup"><span data-stu-id="61e35-152">Configure</span></span>
<span data-ttu-id="61e35-153">Från den **konfigurera** fliken som du kan välja det lämpliga lagringsalternativet för redundans.</span><span class="sxs-lookup"><span data-stu-id="61e35-153">From the **Configure** tab you can select the appropriate storage redundancy option.</span></span> <span data-ttu-id="61e35-154">Den bästa tidpunkten att välja lagringsalternativ för redundans är rätt när du har skapat ett valv och alla datorer som är registrerade på den.</span><span class="sxs-lookup"><span data-stu-id="61e35-154">The best time to select the storage redundancy option is right after creating a vault and before any machines are registered to it.</span></span>

> [!WARNING]
> <span data-ttu-id="61e35-155">När ett objekt har registrerats i valvet, redundans lagringsalternativ är låst och kan inte ändras.</span><span class="sxs-lookup"><span data-stu-id="61e35-155">Once an item has been registered to the vault, the storage redundancy option is locked and cannot be modified.</span></span>
>
>

![Konfigurera](./media/backup-azure-manage-windows-server-classic/configure.png)

<span data-ttu-id="61e35-157">Se den här artikeln för mer information om [lagring redundans](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="61e35-157">See this article for more information about [storage redundancy](../storage/common/storage-redundancy.md).</span></span>

## <a name="microsoft-azure-backup-agent-tasks"></a><span data-ttu-id="61e35-158">Microsoft Azure Backup agentuppgifter</span><span class="sxs-lookup"><span data-stu-id="61e35-158">Microsoft Azure Backup agent tasks</span></span>
### <a name="console"></a><span data-ttu-id="61e35-159">Konsolen</span><span class="sxs-lookup"><span data-stu-id="61e35-159">Console</span></span>
<span data-ttu-id="61e35-160">Öppna den **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="61e35-160">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

![Backup-agenten](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)

<span data-ttu-id="61e35-162">Från den **åtgärder** tillgängliga längst till höger i konsolen backup-agenten kan du utföra följande hanteringsaktiviteter:</span><span class="sxs-lookup"><span data-stu-id="61e35-162">From the **Actions** available at the right of the backup agent console you can perform the following management tasks:</span></span>

* <span data-ttu-id="61e35-163">Registrera Server</span><span class="sxs-lookup"><span data-stu-id="61e35-163">Register Server</span></span>
* <span data-ttu-id="61e35-164">Schema för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="61e35-164">Schedule Backup</span></span>
* <span data-ttu-id="61e35-165">Säkerhetskopiera nu</span><span class="sxs-lookup"><span data-stu-id="61e35-165">Back Up now</span></span>
* <span data-ttu-id="61e35-166">Ändra egenskaper för</span><span class="sxs-lookup"><span data-stu-id="61e35-166">Change Properties</span></span>

![Konsolen agentåtgärder](./media/backup-azure-manage-windows-server-classic/console-actions.png)

> [!NOTE]
> <span data-ttu-id="61e35-168">Att **återställa Data**, se [återställer filer till en Windows server eller Windows-klientdatorn](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="61e35-168">To **Recover Data**, see [Restore files to a Windows server or Windows client machine](backup-azure-restore-windows-server.md).</span></span>
>
>

### <a name="modify-an-existing-backup"></a><span data-ttu-id="61e35-169">Ändra en befintlig säkerhetskopia</span><span class="sxs-lookup"><span data-stu-id="61e35-169">Modify an existing backup</span></span>
1. <span data-ttu-id="61e35-170">Klicka i Microsoft Azure Backup-agenten **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="61e35-170">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
2. <span data-ttu-id="61e35-172">I den **guiden schemalägga säkerhetskopiering** lämna den **göra ändringar i säkerhetskopiering objekt eller gånger** alternativ som valts och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="61e35-172">In the **Schedule Backup Wizard** leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Ändra en schemalagd säkerhetskopiering](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
3. <span data-ttu-id="61e35-174">Om du vill lägga till eller ändra objekt på den **markerar objekt att säkerhetskopiera** skärmen klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="61e35-174">If you want to add or change items, on the **Select Items to Backup** screen click **Add Items**.</span></span>

    <span data-ttu-id="61e35-175">Du kan också ange **Undantagsinställningar** från den här sidan i guiden.</span><span class="sxs-lookup"><span data-stu-id="61e35-175">You can also set **Exclusion Settings** from this page in the wizard.</span></span> <span data-ttu-id="61e35-176">Om du vill undanta filer eller filtyper Läs proceduren för att lägga till [undantagsinställningar](#exclusion-settings).</span><span class="sxs-lookup"><span data-stu-id="61e35-176">If you want to exclude files or file types read the procedure for adding [exclusion settings](#exclusion-settings).</span></span>
4. <span data-ttu-id="61e35-177">Välj de filer och mappar som du vill säkerhetskopiera och klicka på **okej**.</span><span class="sxs-lookup"><span data-stu-id="61e35-177">Select the files and folders you want to back up and click **Okay**.</span></span>

    ![Lägg till artiklar](./media/backup-azure-manage-windows-server-classic/add-items-modify.png)
5. <span data-ttu-id="61e35-179">Ange den **Säkerhetskopieringsschemat** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="61e35-179">Specify the **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="61e35-180">Du kan schemalägga varje dag (högst 3 gånger per dag) eller veckovisa säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="61e35-180">You can schedule daily (at a maximum of 3 times per day) or weekly backups.</span></span>

    ![Ange schema för säkerhetskopiering](./media/backup-azure-manage-windows-server-classic/specify-backup-schedule-modify-close.png)

   > [!NOTE]
   > <span data-ttu-id="61e35-182">Ange schemat för säkerhetskopiering är beskrivs i detalj i detta [artikel](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="61e35-182">Specifying the backup schedule is explained in detail in this [article](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >
6. <span data-ttu-id="61e35-183">Välj den **bevarandeprincip** för säkerhetskopia och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="61e35-183">Select the **Retention Policy** for the backup copy and click **Next**.</span></span>

    ![Välj din bevarandeprincip](./media/backup-azure-manage-windows-server-classic/select-retention-policy-modify.png)
7. <span data-ttu-id="61e35-185">På den **bekräftelse** granska informationen och klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="61e35-185">On the **Confirmation** screen review the information and click **Finish**.</span></span>
8. <span data-ttu-id="61e35-186">När guiden är klar att skapa den **Säkerhetskopieringsschemat**, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="61e35-186">Once the wizard finishes creating the **backup schedule**, click **Close**.</span></span>

    <span data-ttu-id="61e35-187">Ändrar skydd måste du bekräfta att säkerhetskopieringar utlöser korrekt genom att gå till den **jobb** fliken och bekräftar att ändringarna visas i alla säkerhetskopieringsjobb.</span><span class="sxs-lookup"><span data-stu-id="61e35-187">After modifying protection, you can confirm that backups are triggering correctly by going to the **Jobs** tab and confirming that changes are reflected in the backup jobs.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="61e35-188">Aktivera begränsning</span><span class="sxs-lookup"><span data-stu-id="61e35-188">Enable Network Throttling</span></span>
<span data-ttu-id="61e35-189">Azure Backup-agenten ger en begränsning flik där du kan styra hur nätverksbandbredden används när data överfördes.</span><span class="sxs-lookup"><span data-stu-id="61e35-189">The Azure Backup agent provides a Throttling tab which allows you to control how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="61e35-190">Den här kontrollen kan vara användbart om du behöver säkerhetskopiera data under arbetstid, men inte vill säkerhetskopieringsprocessen störa andra internet-trafik.</span><span class="sxs-lookup"><span data-stu-id="61e35-190">This control can be helpful if you need to back up data during work hours but do not want the backup process to interfere with other internet traffic.</span></span> <span data-ttu-id="61e35-191">Begränsning av data gäller transfer för att säkerhetskopiera och återställa aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="61e35-191">Throttling of data transfer applies to back up and restore activities.</span></span>  

<span data-ttu-id="61e35-192">Aktivera begränsning:</span><span class="sxs-lookup"><span data-stu-id="61e35-192">To enable throttling:</span></span>

1. <span data-ttu-id="61e35-193">I den **säkerhetskopieringsagenten**, klickar du på **ändra egenskaper för**.</span><span class="sxs-lookup"><span data-stu-id="61e35-193">In the **Backup agent**, click **Change Properties**.</span></span>
2. <span data-ttu-id="61e35-194">Välj den **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="61e35-194">Select the **Enable internet bandwidth usage throttling for backup operations** checkbox.</span></span>

    ![Nätverksbegränsning](./media/backup-azure-manage-windows-server-classic/throttling-dialog.png)
3. <span data-ttu-id="61e35-196">När du har aktiverat begränsning, ange tillåtna bandbredd för överföring av säkerhetskopierade data under **arbetstid** och **icke-arbetstid**.</span><span class="sxs-lookup"><span data-stu-id="61e35-196">Once you have enabled throttling, specify the allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="61e35-197">Värdena bandbredd börja på 512 kilobit per sekund (Kbps) och gå upp till 1 023 megabyte per sekund (Mbps).</span><span class="sxs-lookup"><span data-stu-id="61e35-197">The bandwidth values begin at 512 kilobytes per second (Kbps) and can go up to 1023 megabytes per second (Mbps).</span></span> <span data-ttu-id="61e35-198">Du kan också ange början och avslutas för **arbetstid**, och vilka dagar i veckan betraktas arbete dagar.</span><span class="sxs-lookup"><span data-stu-id="61e35-198">You can also designate the start and finish for **Work hours**, and which days of the week are considered Work days.</span></span> <span data-ttu-id="61e35-199">Tid utanför arbetstid avsedda betraktas som icke-arbetstid.</span><span class="sxs-lookup"><span data-stu-id="61e35-199">The time outside of the designated Work hours is considered to be non-work hours.</span></span>
4. <span data-ttu-id="61e35-200">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="61e35-200">Click **OK**.</span></span>

## <a name="exclusion-settings"></a><span data-ttu-id="61e35-201">Undantagsinställningar</span><span class="sxs-lookup"><span data-stu-id="61e35-201">Exclusion settings</span></span>
1. <span data-ttu-id="61e35-202">Öppna den **Microsoft Azure Backup-agenten** (du kan hitta den genom att söka igenom din dator för *Microsoft Azure Backup*).</span><span class="sxs-lookup"><span data-stu-id="61e35-202">Open the **Microsoft Azure Backup agent** (you can find it by searching your machine for *Microsoft Azure Backup*).</span></span>

    ![Öppna säkerhetskopieringsagent](./media/backup-azure-manage-windows-server-classic/snap-in-search.png)
2. <span data-ttu-id="61e35-204">Klicka i Microsoft Azure Backup-agenten **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="61e35-204">In the Microsoft Azure Backup agent click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Serverbackup](./media/backup-azure-manage-windows-server-classic/schedule-backup.png)
3. <span data-ttu-id="61e35-206">I guiden schemalägga säkerhetskopiering lämna den **göra ändringar i säkerhetskopiering objekt eller gånger** alternativ som valts och klickar på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="61e35-206">In the Schedule Backup Wizard leave the **Make changes to backup items or times** option selected and click **Next**.</span></span>

    ![Ändra ett schema](./media/backup-azure-manage-windows-server-classic/modify-or-stop-a-scheduled-backup.png)
4. <span data-ttu-id="61e35-208">Klicka på **undantag inställningar**.</span><span class="sxs-lookup"><span data-stu-id="61e35-208">Click **Exclusions Settings**.</span></span>

    ![Välj objekt som ska undantas](./media/backup-azure-manage-windows-server-classic/exclusion-settings.png)
5. <span data-ttu-id="61e35-210">Klicka på **Lägg till undantag**.</span><span class="sxs-lookup"><span data-stu-id="61e35-210">Click **Add Exclusion**.</span></span>

    ![Lägg till undantag](./media/backup-azure-manage-windows-server-classic/add-exclusion.png)
6. <span data-ttu-id="61e35-212">Välj platsen och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="61e35-212">Select the location and then, click **OK**.</span></span>

    ![Välj en plats för undantag](./media/backup-azure-manage-windows-server-classic/exclusion-location.png)
7. <span data-ttu-id="61e35-214">Lägga till filnamnstillägget i den **filtyp** fältet.</span><span class="sxs-lookup"><span data-stu-id="61e35-214">Add the file extension in the **File Type** field.</span></span>

    ![Exkludera efter filtyp](./media/backup-azure-manage-windows-server-classic/exclude-file-type.png)

    <span data-ttu-id="61e35-216">Lägger till en .mp3-tillägg</span><span class="sxs-lookup"><span data-stu-id="61e35-216">Adding an .mp3 extension</span></span>

    ![Exempel på en typ.](./media/backup-azure-manage-windows-server-classic/exclude-mp3.png)

    <span data-ttu-id="61e35-218">Lägg till ett annat filtillägg, klicka på **Lägg till undantag** och ange en annan filtyp (lägga till filnamnstillägget .jpeg).</span><span class="sxs-lookup"><span data-stu-id="61e35-218">To add another extension, click **Add Exclusion** and enter another file type extension (adding a .jpeg extension).</span></span>

    ![Exempel på en annan typ](./media/backup-azure-manage-windows-server-classic/exclude-jpg.png)
8. <span data-ttu-id="61e35-220">När du har lagt till alla tillägg, klickar du på **OK**.</span><span class="sxs-lookup"><span data-stu-id="61e35-220">When you've added all the extensions, click **OK**.</span></span>
9. <span data-ttu-id="61e35-221">Fortsätta med guiden Schemalägg säkerhetskopiering genom att klicka på **nästa** tills den **bekräftelsesidan**, klicka på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="61e35-221">Continue through the Schedule Backup Wizard by clicking **Next** until the **Confirmation page**, then click **Finish**.</span></span>

    ![Bekräftelse av undantag](./media/backup-azure-manage-windows-server-classic/finish-exclusions.png)

## <a name="next-steps"></a><span data-ttu-id="61e35-223">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="61e35-223">Next steps</span></span>
* [<span data-ttu-id="61e35-224">Återställa Windows Server eller Windows-klienten från Azure</span><span class="sxs-lookup"><span data-stu-id="61e35-224">Restore Windows Server or Windows Client from Azure</span></span>](backup-azure-restore-windows-server.md)
* <span data-ttu-id="61e35-225">Mer information om Azure Backup finns [översikt över Azure-säkerhetskopiering](backup-introduction-to-azure-backup.md)</span><span class="sxs-lookup"><span data-stu-id="61e35-225">To learn more about Azure Backup, see [Azure Backup Overview](backup-introduction-to-azure-backup.md)</span></span>
* <span data-ttu-id="61e35-226">Besök den [Azure Backup-Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span><span class="sxs-lookup"><span data-stu-id="61e35-226">Visit the [Azure Backup Forum](http://go.microsoft.com/fwlink/p/?LinkId=290933)</span></span>
