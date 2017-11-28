---
title: aaaBack in Windows filer och mappar tooAzure (Resource Manager) | Microsoft Docs
description: "Lär dig tooback in Windows-filer och mappar tooAzure i en Resource Manager distribution."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "hur toobackup; hur tooback. Säkerhetskopiera filer och mappar"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 8/15/2017
ms.author: markgal;
ms.openlocfilehash: 07d6580a84d5092ed2c61bf86ff5fcb148423ef2
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="first-look-back-up-files-and-folders-in-resource-manager-deployment"></a><span data-ttu-id="766b4-104">En första titt: Säkerhetskopiera filer och mappar med Resource Manager-distributions</span><span class="sxs-lookup"><span data-stu-id="766b4-104">First look: back up files and folders in Resource Manager deployment</span></span>
<span data-ttu-id="766b4-105">Den här artikeln förklarar hur tooback in Windows Server (eller Windows-dator) filer och mappar tooAzure med hjälp av en Resource Manager distribution.</span><span class="sxs-lookup"><span data-stu-id="766b4-105">This article explains how tooback up your Windows Server (or Windows computer) files and folders tooAzure using a Resource Manager deployment.</span></span> <span data-ttu-id="766b4-106">Det är en självstudiekurs avsedda toowalk du via hello grunderna.</span><span class="sxs-lookup"><span data-stu-id="766b4-106">It's a tutorial intended toowalk you through hello basics.</span></span> <span data-ttu-id="766b4-107">Om du vill tooget igång med Azure Backup är hello rätt plats.</span><span class="sxs-lookup"><span data-stu-id="766b4-107">If you want tooget started using Azure Backup, you're in hello right place.</span></span>

<span data-ttu-id="766b4-108">Om du vill tooknow mer om Azure Backup kan läsa det här [översikt](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="766b4-108">If you want tooknow more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="766b4-109">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) som ger dig åtkomst till Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="766b4-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="766b4-110">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="766b4-110">Create a recovery services vault</span></span>
<span data-ttu-id="766b4-111">tooback upp filer och mappar måste toocreate Recovery Services-valvet i hello region där du vill toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="766b4-111">tooback up your files and folders, you need toocreate a Recovery Services vault in hello region where you want toostore hello data.</span></span> <span data-ttu-id="766b4-112">Du måste också toodetermine du hur replikerade lagringen.</span><span class="sxs-lookup"><span data-stu-id="766b4-112">You also need toodetermine how you want your storage replicated.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="766b4-113">toocreate Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="766b4-113">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="766b4-114">Om du inte redan har gjort det loggar du in toohello [Azure Portal](https://portal.azure.com/) med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="766b4-114">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="766b4-115">Hej hubbmenyn, klicka på **fler tjänster** och Skriv i hello lista över resurser, **återställningstjänster** och på **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="766b4-115">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="766b4-117">Om det finns recovery services-valv i hello prenumeration, visas hello valv.</span><span class="sxs-lookup"><span data-stu-id="766b4-117">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>
3. <span data-ttu-id="766b4-118">På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="766b4-118">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="766b4-120">hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.</span><span class="sxs-lookup"><span data-stu-id="766b4-120">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Skapa Recovery Services-valv (steg 3)](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="766b4-122">För **namn**, ange ett eget namn tooidentify hello valv.</span><span class="sxs-lookup"><span data-stu-id="766b4-122">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="766b4-123">hello namn måste toobe unika för hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="766b4-123">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="766b4-124">Skriv ett namn som innehåller mellan 2 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="766b4-124">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="766b4-125">Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="766b4-125">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="766b4-126">I hello **prenumeration** Använd hello nedrullningsbara menyn toochoose hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="766b4-126">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="766b4-127">Om du använder bara en prenumeration som prenumeration visas och du kan hoppa över toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="766b4-127">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="766b4-128">Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="766b4-128">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="766b4-129">Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="766b4-129">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="766b4-130">I hello **resursgruppen** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="766b4-130">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="766b4-131">Välj **Skapa nytt** om du vill toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="766b4-131">select **Create new** if you want toocreate a new Resource group.</span></span>
    <span data-ttu-id="766b4-132">Eller</span><span class="sxs-lookup"><span data-stu-id="766b4-132">Or</span></span>
    * <span data-ttu-id="766b4-133">Välj **använda befintliga** och klicka på hello nedrullningsbara menyn toosee hello tillgängliga listan över resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="766b4-133">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="766b4-134">Mer information om resursgrupper finns i hello [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="766b4-134">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="766b4-135">Klicka på **plats** tooselect hello geografiskt område för hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="766b4-135">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="766b4-136">Det här alternativet anger hello geografiska region där dina säkerhetskopierade data skickas.</span><span class="sxs-lookup"><span data-stu-id="766b4-136">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="766b4-137">Hello längst ned på bladet för hello Recovery Services-valvet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="766b4-137">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="766b4-138">Det kan ta flera minuter för hello Recovery Services-valvet toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="766b4-138">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="766b4-139">Övervaka hello statusmeddelanden i hello övre högra delen av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="766b4-139">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="766b4-140">När din valvet har skapats visas den i hello lista över Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="766b4-140">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="766b4-141">Om du inte ser ditt valv efter ett par minuter klickar du på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="766b4-141">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Klicka på Uppdatera](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="766b4-143">När du ser ditt valv i hello lista över Recovery Services-valv, är du redo tooset hello lagring redundans.</span><span class="sxs-lookup"><span data-stu-id="766b4-143">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-hello-vault"></a><span data-ttu-id="766b4-144">Ange lagringsredundans för hello valvet</span><span class="sxs-lookup"><span data-stu-id="766b4-144">Set storage redundancy for hello vault</span></span>
<span data-ttu-id="766b4-145">När du skapar ett Recovery Services-valv, kontrollera att lagring redundans är konfigurerade hello som du vill.</span><span class="sxs-lookup"><span data-stu-id="766b4-145">When you create a Recovery Services vault, make sure storage redundancy is configured hello way you want.</span></span>

1. <span data-ttu-id="766b4-146">Från hello **Recovery Services-valv** bladet Klicka hello nya valvet.</span><span class="sxs-lookup"><span data-stu-id="766b4-146">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Välj hello nytt valv hello listan över Recovery Services-valvet](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="766b4-148">När du väljer hello valvet hello **Recovery Services-valvet** bladet begränsar och hello inställningsbladet (*som har hello namnet på valvet hello överst hello*) och hello valvet informationsbladet öppna.</span><span class="sxs-lookup"><span data-stu-id="766b4-148">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Visa hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="766b4-150">Använda hello lodräta bilden tooscroll ned toohello hantera avsnitt i hello nytt valv inställningar-bladet och klickar på **säkerhetskopiering infrastruktur**.</span><span class="sxs-lookup"><span data-stu-id="766b4-150">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="766b4-151">hello säkerhetskopiering infrastruktur blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="766b4-151">hello Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="766b4-152">I hello säkerhetskopiering infrastruktur-bladet, klickar du på **konfigurering av säkerhetskopiering** tooopen hello **konfigurering av säkerhetskopiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="766b4-152">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

    ![Ange hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="766b4-154">Välj hello lämpliga replikering lagringsalternativ för ditt valv.</span><span class="sxs-lookup"><span data-stu-id="766b4-154">Choose hello appropriate storage replication option for your vault.</span></span>

    ![alternativ för lagringskonfiguration](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="766b4-156">Valvet använder geo-redundant lagring som standard.</span><span class="sxs-lookup"><span data-stu-id="766b4-156">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="766b4-157">Om du använder Azure som en primär säkerhetskopieringslagring slutpunkt fortsätta toouse **Geo-redundant**.</span><span class="sxs-lookup"><span data-stu-id="766b4-157">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="766b4-158">Om du inte använder Azure som en primär säkerhetskopieringslagring slutpunkt, Välj **lokalt redundant**, vilket minskar hello Azure lagringskostnader.</span><span class="sxs-lookup"><span data-stu-id="766b4-158">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="766b4-159">Läs mer om alternativen för [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) i denna [översikt av lagringsredundans](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="766b4-159">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="766b4-160">Nu när du har skapat ett valv konfigurerar du det för säkerhetskopiering av filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="766b4-160">Now that you've created a vault, configure it for backing up files and folders.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="766b4-161">Konfigurera hello valvet</span><span class="sxs-lookup"><span data-stu-id="766b4-161">Configure hello vault</span></span>
1. <span data-ttu-id="766b4-162">Hej Recovery Services-valvet bladet (för hello valvet du just skapade), i hello komma igång-avsnittet, klickar du på **säkerhetskopiering**, sedan på hello **Kom igång med säkerhetskopiering** bladet väljer  **Säkerhetskopiering målet**.</span><span class="sxs-lookup"><span data-stu-id="766b4-162">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="766b4-164">Hej **säkerhetskopiering målet** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="766b4-164">hello **Backup Goal** blade opens.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="766b4-166">Från hello **var körs din arbetsbelastning?** nedrullningsbara menyn och väljer **lokalt**.</span><span class="sxs-lookup"><span data-stu-id="766b4-166">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="766b4-167">Du väljer **Lokalt** eftersom din Windows Server- eller Windows-dator är en fysisk dator som inte finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="766b4-167">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="766b4-168">Från hello **vad vill du vill toobackup?** väljer du **filer och mappar**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="766b4-168">From hello **What do you want toobackup?** menu, select **Files and folders**, and click **OK**.</span></span>

    ![Konfigurera filer och mappar](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

    <span data-ttu-id="766b4-170">När du klickar på OK, visas en markering nästa för**säkerhetskopiering målet**, och hello **Förbered infrastruktur** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="766b4-170">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

    ![När säkerhetskopieringsmålet har konfigurerats går du vidare och förbereder infrastrukturen](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="766b4-172">På hello **Förbered infrastruktur** bladet, klickar du på **ladda ned Agent för Windows Server och Windows-klient**.</span><span class="sxs-lookup"><span data-stu-id="766b4-172">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="766b4-174">Om du använder Windows Server viktigt, och välj sedan toodownload hello agent för Windows Server viktigt.</span><span class="sxs-lookup"><span data-stu-id="766b4-174">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="766b4-175">Ett popup-menyn efterfrågar toorun eller spara MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="766b4-175">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

    ![Dialogrutan MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="766b4-177">I hello hämta popup-menyn, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="766b4-177">In hello download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="766b4-178">Som standard hello **MARSagentinstaller.exe** tooyour hämtningsmapp sparas i filen.</span><span class="sxs-lookup"><span data-stu-id="766b4-178">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="766b4-179">När hello installer är klar visas ett popup-fönster som frågar om du vill toorun hello installer eller öppna hello mapp.</span><span class="sxs-lookup"><span data-stu-id="766b4-179">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="766b4-181">Du behöver inte tooinstall hello agenten ännu.</span><span class="sxs-lookup"><span data-stu-id="766b4-181">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="766b4-182">Du kan installera hello agenten när du har hämtat hello valvautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="766b4-182">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="766b4-183">På hello **Förbered infrastruktur** bladet, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="766b4-183">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

    ![hämta autentiseringsuppgifter för valvet](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="766b4-185">Hej valvautentiseringsuppgifter hämta tooyour hämtningsmapp.</span><span class="sxs-lookup"><span data-stu-id="766b4-185">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="766b4-186">När hello valvautentiseringsuppgifter nedladdning, visas ett popup-fönster som frågar om du vill tooopen eller spara hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="766b4-186">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="766b4-187">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="766b4-187">Click **Save**.</span></span> <span data-ttu-id="766b4-188">Om du råkar klicka på **öppna**, låta hello dialogruta som försöker tooopen hello valvautentiseringsuppgifter, misslyckas.</span><span class="sxs-lookup"><span data-stu-id="766b4-188">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="766b4-189">Du kan inte öppna hello valvautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="766b4-189">You cannot open hello vault credentials.</span></span> <span data-ttu-id="766b4-190">Fortsätt toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="766b4-190">Proceed toohello next step.</span></span> <span data-ttu-id="766b4-191">Hej valvautentiseringsuppgifter finns i hello hämtningsmapp.</span><span class="sxs-lookup"><span data-stu-id="766b4-191">hello vault credentials are in hello Downloads folder.</span></span>   

    ![valvautentiseringsuppgifterna har hämtats](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="766b4-193">Installera och registrera hello-agent</span><span class="sxs-lookup"><span data-stu-id="766b4-193">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="766b4-194">Aktivera säkerhetskopiering via hello Azure-portalen är inte tillgängligt ännu.</span><span class="sxs-lookup"><span data-stu-id="766b4-194">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="766b4-195">Använd hello Microsoft Azure Recovery Services-agenten tooback upp filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="766b4-195">Use hello Microsoft Azure Recovery Services Agent tooback up your files and folders.</span></span>
>

1. <span data-ttu-id="766b4-196">Leta upp och dubbelklicka på hello **MARSagentinstaller.exe** från hello hämtningar mappen (eller andra lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="766b4-196">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="766b4-197">hello installer innehåller ett antal meddelanden som extraheras, installerar och registrerar hello Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="766b4-197">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

    ![Autentiseringsuppgifter för att köra Recovery Services-agentinstallationsprogrammet](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="766b4-199">Slutför hello installationsguiden för Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="766b4-199">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="766b4-200">toocomplete hello guiden måste du:</span><span class="sxs-lookup"><span data-stu-id="766b4-200">toocomplete hello wizard, you need to:</span></span>

   * <span data-ttu-id="766b4-201">Välj en plats för hello installation och cache-mappen.</span><span class="sxs-lookup"><span data-stu-id="766b4-201">Choose a location for hello installation and cache folder.</span></span>
   * <span data-ttu-id="766b4-202">Ange proxyserveradressen serverinformation om du använder en proxy server tooconnect toohello internet.</span><span class="sxs-lookup"><span data-stu-id="766b4-202">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
   * <span data-ttu-id="766b4-203">Ange ditt användarnamn och lösenord om du använder en autentiserad proxyserver.</span><span class="sxs-lookup"><span data-stu-id="766b4-203">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="766b4-204">Ange hello ned valvautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="766b4-204">Provide hello downloaded vault credentials</span></span>
   * <span data-ttu-id="766b4-205">Spara hello krypteringslösenfrasen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="766b4-205">Save hello encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="766b4-206">Om du förlorar eller glömmer hello lösenfras går inte Microsoft att återställa hello säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="766b4-206">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="766b4-207">Spara hello-filen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="766b4-207">Save hello file in a secure location.</span></span> <span data-ttu-id="766b4-208">Det är obligatoriskt toorestore en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="766b4-208">It is required toorestore a backup.</span></span>
     >
     >

<span data-ttu-id="766b4-209">hello-agent har installerats och datorn är registrerade toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="766b4-209">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="766b4-210">Du är klar tooconfigure och schemalägger säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="766b4-210">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="766b4-211">Nätverks- och anslutningskrav</span><span class="sxs-lookup"><span data-stu-id="766b4-211">Network and Connectivity Requirements</span></span>

<span data-ttu-id="766b4-212">Om din dator/proxy har begränsad tillgång till internet, kan du kontrollera att brandväggen på hello mcahine/proxy är konfigurerade tooallow hello följande URL: er:</span><span class="sxs-lookup"><span data-stu-id="766b4-212">If your machine/proxy has limited internet access, ensure that firewall settings on hello mcahine/proxy are configured tooallow hello following URLs:</span></span> <br>
    1. <span data-ttu-id="766b4-213">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="766b4-213">www.msftncsi.com</span></span>
    2. <span data-ttu-id="766b4-214">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="766b4-214">*.Microsoft.com</span></span>
    3. <span data-ttu-id="766b4-215">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="766b4-215">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="766b4-216">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="766b4-216">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="766b4-217">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="766b4-217">*.windows.ne</span></span>

## <a name="back-up-your-files-and-folders"></a><span data-ttu-id="766b4-218">Säkerhetskopiera dina filer och mappar</span><span class="sxs-lookup"><span data-stu-id="766b4-218">Back up your files and folders</span></span>
<span data-ttu-id="766b4-219">hello första säkerhetskopian innehåller två viktiga uppgifter:</span><span class="sxs-lookup"><span data-stu-id="766b4-219">hello initial backup includes two key tasks:</span></span>

* <span data-ttu-id="766b4-220">Schemalägga hello säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="766b4-220">Schedule hello backup</span></span>
* <span data-ttu-id="766b4-221">Säkerhetskopiera filer och mappar för hello första gången</span><span class="sxs-lookup"><span data-stu-id="766b4-221">Back up files and folders for hello first time</span></span>

<span data-ttu-id="766b4-222">toocomplete hello första säkerhetskopiering, Använd hello Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="766b4-222">toocomplete hello initial backup, use hello Microsoft Azure Recovery Services agent.</span></span>

### <a name="tooschedule-hello-backup-job"></a><span data-ttu-id="766b4-223">tooschedule hello säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="766b4-223">tooschedule hello backup job</span></span>
1. <span data-ttu-id="766b4-224">Öppna hello Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="766b4-224">Open hello Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="766b4-225">Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.</span><span class="sxs-lookup"><span data-stu-id="766b4-225">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Starta hello Azure Recovery Services-agenten](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. <span data-ttu-id="766b4-227">Klicka på hello Recovery Services-agenten **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="766b4-227">In hello Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. <span data-ttu-id="766b4-229">På hello komma igång-sidan hello guiden för Schemalägg säkerhetskopiering klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="766b4-229">On hello Getting started page of hello Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="766b4-230">På hello Välj objekt tooBackup klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="766b4-230">On hello Select Items tooBackup page, click **Add Items**.</span></span>
5. <span data-ttu-id="766b4-231">Välj hello filer och mappar som du vill att tooback och klicka sedan på **okej**.</span><span class="sxs-lookup"><span data-stu-id="766b4-231">Select hello files and folders that you want tooback up, and then click **Okay**.</span></span>
6. <span data-ttu-id="766b4-232">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="766b4-232">Click **Next**.</span></span>
7. <span data-ttu-id="766b4-233">På hello **ange schemat för säkerhetskopiering** anger hello **Säkerhetskopieringsschemat** och på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="766b4-233">On hello **Specify Backup Schedule** page, specify hello **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="766b4-234">Du kan schemalägga säkerhetskopieringar varje dag (högst tre gånger om dagen) eller varje vecka.</span><span class="sxs-lookup"><span data-stu-id="766b4-234">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Objekt för Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="766b4-236">Mer information om hur toospecify hello schemat för säkerhetskopiering finns hello artikel [Använd Azure Backup tooreplace infrastrukturen band](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="766b4-236">For more information about how toospecify hello backup schedule, see hello article [Use Azure Backup tooreplace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

8. <span data-ttu-id="766b4-237">På hello **Välj bevarandeprincip** sidan, Välj hello **bevarandeprincip** hello säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="766b4-237">On hello **Select Retention Policy** page, select hello **Retention Policy** for hello backup copy.</span></span>

    <span data-ttu-id="766b4-238">hello bevarandeprincip anger hur länge hello säkerhetskopierade data lagras.</span><span class="sxs-lookup"><span data-stu-id="766b4-238">hello retention policy specifies how long hello backup data is stored.</span></span> <span data-ttu-id="766b4-239">Du kan ange olika bevarandeprinciper baserat på när hello säkerhetskopia i stället för att ange en ”platt policy” för alla säkerhetskopiering punkterna.</span><span class="sxs-lookup"><span data-stu-id="766b4-239">Rather than specifying a “flat policy” for all backup points, you can specify different retention policies based on when hello backup occurs.</span></span> <span data-ttu-id="766b4-240">Du kan ändra hello dagliga, veckovisa, månatliga och årliga Kvarhållningsintervall principer toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="766b4-240">You can modify hello daily, weekly, monthly, and yearly retention policies toomeet your needs.</span></span>
9. <span data-ttu-id="766b4-241">Välj hello inledande säkerhetskopieringstyp hello på sidan Välj typ av inledande säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="766b4-241">On hello Choose Initial Backup Type page, choose hello initial backup type.</span></span> <span data-ttu-id="766b4-242">Lämna alternativet hello **automatiskt över hello nätverk** markerad och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="766b4-242">Leave hello option **Automatically over hello network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="766b4-243">Du kan säkerhetskopiera automatiskt över hello nätverk eller du kan säkerhetskopiera offline.</span><span class="sxs-lookup"><span data-stu-id="766b4-243">You can back up automatically over hello network, or you can back up offline.</span></span> <span data-ttu-id="766b4-244">hello resten av den här artikeln beskrivs hello säkerhetskopiera automatiskt.</span><span class="sxs-lookup"><span data-stu-id="766b4-244">hello remainder of this article describes hello process for backing up automatically.</span></span> <span data-ttu-id="766b4-245">Om du föredrar toodo en offlinesäkerhetskopiering granska hello artikel [Offline säkerhetskopiering arbetsflödet i Azure Backup](backup-azure-backup-import-export.md) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="766b4-245">If you prefer toodo an offline backup, review hello article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="766b4-246">På sidan Bekräfta hello granska hello information och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="766b4-246">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>
11. <span data-ttu-id="766b4-247">När hello guiden är färdig med hello schemat för säkerhetskopiering, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="766b4-247">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a><span data-ttu-id="766b4-248">tooback av filer och mappar för hello första gången</span><span class="sxs-lookup"><span data-stu-id="766b4-248">tooback up files and folders for hello first time</span></span>
1. <span data-ttu-id="766b4-249">Klicka på hello Recovery Services-agenten **Säkerhetskopiera nu** toocomplete hello inledande seeding hello nätverket.</span><span class="sxs-lookup"><span data-stu-id="766b4-249">In hello Recovery Services agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Säkerhetskopiera Windows Server nu](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. <span data-ttu-id="766b4-251">På sidan Bekräfta hello använder hello granskningsinställningar som hello tillbaka nu guiden Installera tooback upp hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="766b4-251">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="766b4-252">Klicka på **Säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="766b4-252">Then click **Back Up**.</span></span>
3. <span data-ttu-id="766b4-253">Klicka på **Stäng** tooclose hello guiden.</span><span class="sxs-lookup"><span data-stu-id="766b4-253">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="766b4-254">Om du stänger guiden hello innan hello säkerhetskopieringen är klar fortsätter hello guiden toorun i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="766b4-254">If you close hello wizard before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

<span data-ttu-id="766b4-255">När hello första säkerhetskopieringen har slutförts hello **jobbet slutfört** status visas i konsolen för hello-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="766b4-255">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

![IR slutfört](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="766b4-257">Frågor?</span><span class="sxs-lookup"><span data-stu-id="766b4-257">Questions?</span></span>
<span data-ttu-id="766b4-258">Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="766b4-258">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="766b4-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="766b4-259">Next steps</span></span>
* <span data-ttu-id="766b4-260">Få mer information om hur du [säkerhetskopierar Windows-datorer](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="766b4-260">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="766b4-261">Nu när du har säkerhetskopierat dina filer och mappar kan du [hantera dina valv och servrar](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="766b4-261">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="766b4-262">Om du behöver toorestore en säkerhetskopia kan använda den här artikeln för[återställa filer tooa Windows datorn](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="766b4-262">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
