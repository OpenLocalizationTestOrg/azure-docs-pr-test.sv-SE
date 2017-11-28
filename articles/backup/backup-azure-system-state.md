---
title: "aaaBack in Windows system tillstånd tooAzure | Microsoft Docs"
description: "Lär dig tooback in hello systemtillståndet för Windows Server och/eller tooAzure för Windows-datorer."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "hur toobackup; hur tooback. Säkerhetskopiera filer och mappar"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: be5d4be81af981c10de82add9fe962a730753cf5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a><span data-ttu-id="dfbfc-104">Säkerhetskopiera systemtillståndet för Windows i Resource Manager-distribution</span><span class="sxs-lookup"><span data-stu-id="dfbfc-104">Back up Windows system state in Resource Manager deployment</span></span>
<span data-ttu-id="dfbfc-105">Den här artikeln förklarar hur tooback in Windows Server-systemet tillstånd tooAzure.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-105">This article explains how tooback up your Windows Server system state tooAzure.</span></span> <span data-ttu-id="dfbfc-106">Det är en självstudiekurs avsedda toowalk du via hello grunderna.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-106">It's a tutorial intended toowalk you through hello basics.</span></span>

<span data-ttu-id="dfbfc-107">Om du vill tooknow mer om Azure Backup kan läsa det här [översikt](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-107">If you want tooknow more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="dfbfc-108">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) som ger dig åtkomst till Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="dfbfc-109">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="dfbfc-109">Create a recovery services vault</span></span>
<span data-ttu-id="dfbfc-110">tooback upp filer och mappar måste toocreate Recovery Services-valvet i hello region där du vill toostore hello data.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-110">tooback up your files and folders, you need toocreate a Recovery Services vault in hello region where you want toostore hello data.</span></span> <span data-ttu-id="dfbfc-111">Du måste också toodetermine du hur replikerade lagringen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-111">You also need toodetermine how you want your storage replicated.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="dfbfc-112">toocreate Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="dfbfc-112">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="dfbfc-113">Om du inte redan har gjort det loggar du in toohello [Azure Portal](https://portal.azure.com/) med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-113">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="dfbfc-114">Hej hubbmenyn, klicka på **fler tjänster** och Skriv i hello lista över resurser, **återställningstjänster** och på **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-114">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="dfbfc-116">Om det finns recovery services-valv i hello prenumeration, visas hello valv.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-116">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>
3. <span data-ttu-id="dfbfc-117">På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-117">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="dfbfc-119">hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-119">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Skapa Recovery Services-valv (steg 3)](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="dfbfc-121">För **namn**, ange ett eget namn tooidentify hello valv.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-121">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="dfbfc-122">hello namn måste toobe unika för hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-122">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="dfbfc-123">Skriv ett namn som innehåller mellan 2 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-123">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="dfbfc-124">Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-124">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="dfbfc-125">I hello **prenumeration** Använd hello nedrullningsbara menyn toochoose hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-125">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="dfbfc-126">Om du använder bara en prenumeration som prenumeration visas och du kan hoppa över toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-126">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="dfbfc-127">Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-127">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="dfbfc-128">Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-128">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="dfbfc-129">I hello **resursgruppen** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="dfbfc-129">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="dfbfc-130">Välj **Skapa nytt** om du vill toocreate en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-130">select **Create new** if you want toocreate a Resource group.</span></span>
    <span data-ttu-id="dfbfc-131">Eller</span><span class="sxs-lookup"><span data-stu-id="dfbfc-131">Or</span></span>
    * <span data-ttu-id="dfbfc-132">Välj **använda befintliga** och klicka på hello nedrullningsbara menyn toosee hello tillgängliga listan över resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-132">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="dfbfc-133">Mer information om resursgrupper finns i hello [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-133">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="dfbfc-134">Klicka på **plats** tooselect hello geografiskt område för hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-134">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="dfbfc-135">Det här alternativet anger hello geografiska region där dina säkerhetskopierade data skickas.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-135">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="dfbfc-136">Hello längst ned på bladet för hello Recovery Services-valvet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-136">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="dfbfc-137">Det kan ta flera minuter för hello Recovery Services-valvet toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-137">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="dfbfc-138">Övervaka hello statusmeddelanden i hello övre högra delen av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-138">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="dfbfc-139">När din valvet har skapats visas den i hello lista över Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-139">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="dfbfc-140">Om du inte ser ditt valv efter ett par minuter klickar du på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-140">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Klicka på Uppdatera](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="dfbfc-142">När du ser ditt valv i hello lista över Recovery Services-valv, är du redo tooset hello lagring redundans.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-142">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-hello-vault"></a><span data-ttu-id="dfbfc-143">Ange lagringsredundans för hello valvet</span><span class="sxs-lookup"><span data-stu-id="dfbfc-143">Set storage redundancy for hello vault</span></span>
<span data-ttu-id="dfbfc-144">När du skapar ett Recovery Services-valv, kontrollera att lagring redundans är konfigurerade hello som du vill.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-144">When you create a Recovery Services vault, make sure storage redundancy is configured hello way you want.</span></span>

1. <span data-ttu-id="dfbfc-145">Från hello **Recovery Services-valv** bladet Klicka hello nya valvet.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-145">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Välj hello nytt valv hello listan över Recovery Services-valvet](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="dfbfc-147">När du väljer hello valvet hello **Recovery Services-valvet** bladet begränsar och hello inställningsbladet (*som har hello namnet på valvet hello överst hello*) och hello valvet informationsbladet öppna.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-147">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Visa hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="dfbfc-149">Använda hello lodräta bilden tooscroll ned toohello hantera avsnitt i hello nytt valv inställningar-bladet och klickar på **säkerhetskopiering infrastruktur**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-149">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="dfbfc-150">hello säkerhetskopiering infrastruktur blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-150">hello Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="dfbfc-151">I hello säkerhetskopiering infrastruktur-bladet, klickar du på **konfigurering av säkerhetskopiering** tooopen hello **konfigurering av säkerhetskopiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-151">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

    ![Ange hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="dfbfc-153">Välj hello lämpliga replikering lagringsalternativ för ditt valv.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-153">Choose hello appropriate storage replication option for your vault.</span></span>

    ![alternativ för lagringskonfiguration](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="dfbfc-155">Valvet använder geo-redundant lagring som standard.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-155">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="dfbfc-156">Om du använder Azure som en primär säkerhetskopieringslagring slutpunkt fortsätta toouse **Geo-redundant**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-156">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="dfbfc-157">Om du inte använder Azure som en primär säkerhetskopieringslagring slutpunkt, Välj **lokalt redundant**, vilket minskar hello Azure lagringskostnader.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-157">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="dfbfc-158">Läs mer om alternativen för [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) i denna [översikt av lagringsredundans](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-158">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="dfbfc-159">Nu när du har skapat ett valv, kan du konfigurera den för att säkerhetskopiera systemtillståndet i Windows.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-159">Now that you've created a vault, configure it for backing up Windows System State.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="dfbfc-160">Konfigurera hello valvet</span><span class="sxs-lookup"><span data-stu-id="dfbfc-160">Configure hello vault</span></span>
1. <span data-ttu-id="dfbfc-161">Hej Recovery Services-valvet bladet (för hello valvet du just skapade), i hello komma igång-avsnittet, klickar du på **säkerhetskopiering**, sedan på hello **Kom igång med säkerhetskopiering** bladet väljer  **Säkerhetskopiering målet**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-161">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="dfbfc-163">Hej **säkerhetskopiering målet** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-163">hello **Backup Goal** blade opens.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="dfbfc-165">Från hello **var körs din arbetsbelastning?** nedrullningsbara menyn och väljer **lokalt**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-165">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="dfbfc-166">Du väljer **Lokalt** eftersom din Windows Server- eller Windows-dator är en fysisk dator som inte finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-166">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="dfbfc-167">Från hello **vad vill du vill toobackup?** väljer du **systemtillstånd**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-167">From hello **What do you want toobackup?** menu, select **System State**, and click **OK**.</span></span>

    ![Konfigurera filer och mappar](./media/backup-azure-system-state/backup-goal-system-state.png)

    <span data-ttu-id="dfbfc-169">När du klickar på OK, visas en markering nästa för**säkerhetskopiering målet**, och hello **Förbered infrastruktur** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-169">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

    ![När säkerhetskopieringsmålet har konfigurerats går du vidare och förbereder infrastrukturen](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="dfbfc-171">På hello **Förbered infrastruktur** bladet, klickar du på **ladda ned Agent för Windows Server och Windows-klient**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-171">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="dfbfc-173">Om du använder Windows Server viktigt, och välj sedan toodownload hello agent för Windows Server viktigt.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-173">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="dfbfc-174">Ett popup-menyn efterfrågar toorun eller spara MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-174">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

    ![Dialogrutan MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="dfbfc-176">I hello hämta popup-menyn, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-176">In hello download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="dfbfc-177">Som standard hello **MARSagentinstaller.exe** tooyour hämtningsmapp sparas i filen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-177">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="dfbfc-178">När hello installer är klar visas ett popup-fönster som frågar om du vill toorun hello installer eller öppna hello mapp.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-178">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="dfbfc-180">Du behöver inte tooinstall hello agenten ännu.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-180">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="dfbfc-181">Du kan installera hello agenten när du har hämtat hello valvautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-181">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="dfbfc-182">På hello **Förbered infrastruktur** bladet, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-182">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

    ![hämta autentiseringsuppgifter för valvet](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="dfbfc-184">Hej valvautentiseringsuppgifter hämta tooyour hämtningsmapp.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-184">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="dfbfc-185">När hello valvautentiseringsuppgifter nedladdning, visas ett popup-fönster som frågar om du vill tooopen eller spara hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-185">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="dfbfc-186">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-186">Click **Save**.</span></span> <span data-ttu-id="dfbfc-187">Om du råkar klicka på **öppna**, låta hello dialogruta som försöker tooopen hello valvautentiseringsuppgifter, misslyckas.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-187">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="dfbfc-188">Du kan inte öppna hello valvautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-188">You cannot open hello vault credentials.</span></span> <span data-ttu-id="dfbfc-189">Fortsätt toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-189">Proceed toohello next step.</span></span> <span data-ttu-id="dfbfc-190">Hej valvautentiseringsuppgifter finns i hello hämtningsmapp.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-190">hello vault credentials are in hello Downloads folder.</span></span>   

    ![valvautentiseringsuppgifterna har hämtats](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="dfbfc-192">Installera och registrera hello-agent</span><span class="sxs-lookup"><span data-stu-id="dfbfc-192">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="dfbfc-193">Aktivera säkerhetskopiering via hello Azure-portalen är inte tillgängligt ännu.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-193">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="dfbfc-194">Använd hello Microsoft Azure Recovery Services-agenten tooback status för Windows Server-System.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-194">Use hello Microsoft Azure Recovery Services Agent tooback up Windows Server System State.</span></span>
>

1. <span data-ttu-id="dfbfc-195">Leta upp och dubbelklicka på hello **MARSagentinstaller.exe** från hello hämtningar mappen (eller andra lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-195">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="dfbfc-196">hello installer innehåller ett antal meddelanden som extraheras, installerar och registrerar hello Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-196">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

    ![Autentiseringsuppgifter för att köra Recovery Services-agentinstallationsprogrammet](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="dfbfc-198">Slutför hello installationsguiden för Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-198">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="dfbfc-199">toocomplete hello guiden måste du:</span><span class="sxs-lookup"><span data-stu-id="dfbfc-199">toocomplete hello wizard, you need to:</span></span>

   * <span data-ttu-id="dfbfc-200">Välj en plats för hello installation och cache-mappen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-200">Choose a location for hello installation and cache folder.</span></span>
   * <span data-ttu-id="dfbfc-201">Ange proxyserveradressen serverinformation om du använder en proxy server tooconnect toohello internet.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-201">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
   * <span data-ttu-id="dfbfc-202">Ange ditt användarnamn och lösenord om du använder en autentiserad proxyserver.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-202">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="dfbfc-203">Ange hello ned valvautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="dfbfc-203">Provide hello downloaded vault credentials</span></span>
   * <span data-ttu-id="dfbfc-204">Spara hello krypteringslösenfrasen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-204">Save hello encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="dfbfc-205">Om du förlorar eller glömmer hello lösenfras går inte Microsoft att återställa hello säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-205">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="dfbfc-206">Spara hello-filen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-206">Save hello file in a secure location.</span></span> <span data-ttu-id="dfbfc-207">Det är obligatoriskt toorestore en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-207">It is required toorestore a backup.</span></span>
     >
     >

<span data-ttu-id="dfbfc-208">hello-agent har installerats och datorn är registrerade toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-208">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="dfbfc-209">Du är klar tooconfigure och schemalägger säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-209">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="back-up-windows-server-system-state-preview"></a><span data-ttu-id="dfbfc-210">Säkerhetskopiera systemtillståndet för Windows Server (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="dfbfc-210">Back up Windows Server System State (Preview)</span></span>
<span data-ttu-id="dfbfc-211">hello första säkerhetskopian innehåller tre uppgifter:</span><span class="sxs-lookup"><span data-stu-id="dfbfc-211">hello initial backup includes three tasks:</span></span>

* <span data-ttu-id="dfbfc-212">Aktivera säkerhetskopiering av systemtillstånd med hello Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="dfbfc-212">Enable System State Backup using hello Azure Backup agent</span></span>
* <span data-ttu-id="dfbfc-213">Schemalägga hello säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="dfbfc-213">Schedule hello backup</span></span>
* <span data-ttu-id="dfbfc-214">Säkerhetskopiera filer och mappar för hello första gången</span><span class="sxs-lookup"><span data-stu-id="dfbfc-214">Back up files and folders for hello first time</span></span>

<span data-ttu-id="dfbfc-215">toocomplete hello första säkerhetskopiering, Använd hello Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-215">toocomplete hello initial backup, use hello Microsoft Azure Recovery Services agent.</span></span>

### <a name="tooenable-system-state-backup-using-hello-azure-backup-agent"></a><span data-ttu-id="dfbfc-216">säkerhetskopian av systemtillstånd tooenable med hello Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="dfbfc-216">tooenable System State backup using hello Azure Backup agent</span></span>

1. <span data-ttu-id="dfbfc-217">Kör hello efter kommandot toostop hello Azure Backup-motorn i en PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-217">In a PowerShell session, run hello following command toostop hello Azure Backup engine.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="dfbfc-218">Öppna hello Windows-registret.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-218">Open hello Windows Registry.</span></span>

  ```
  PS C:\> regedit.exe
  ```

3. <span data-ttu-id="dfbfc-219">Lägg till följande registernyckel med hello hello angetts DWord-värde.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-219">Add hello following registry key with hello specified DWord Value.</span></span>

  | <span data-ttu-id="dfbfc-220">Sökväg i registret</span><span class="sxs-lookup"><span data-stu-id="dfbfc-220">Registry path</span></span> | <span data-ttu-id="dfbfc-221">Registernyckel</span><span class="sxs-lookup"><span data-stu-id="dfbfc-221">Registry key</span></span> | <span data-ttu-id="dfbfc-222">DWord-värde</span><span class="sxs-lookup"><span data-stu-id="dfbfc-222">DWord value</span></span> |
  |---------------|--------------|-------------|
  | <span data-ttu-id="dfbfc-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="dfbfc-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="dfbfc-224">TurnOffSSBFeature</span><span class="sxs-lookup"><span data-stu-id="dfbfc-224">TurnOffSSBFeature</span></span> | <span data-ttu-id="dfbfc-225">2</span><span class="sxs-lookup"><span data-stu-id="dfbfc-225">2</span></span> |

4. <span data-ttu-id="dfbfc-226">Starta om hello säkerhetskopiering motorn genom att köra följande kommando i en upphöjd kommandotolk hello.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-226">Restart hello Backup engine by executing hello following command in an elevated command prompt.</span></span>

  ```
  PS C:\> Net start obengine
  ```

### <a name="tooschedule-hello-backup-job"></a><span data-ttu-id="dfbfc-227">tooschedule hello säkerhetskopieringsjobb</span><span class="sxs-lookup"><span data-stu-id="dfbfc-227">tooschedule hello backup job</span></span>

1. <span data-ttu-id="dfbfc-228">Öppna hello Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-228">Open hello Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="dfbfc-229">Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-229">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Starta hello Azure Recovery Services-agenten](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. <span data-ttu-id="dfbfc-231">Klicka på hello Recovery Services-agenten **schemalägga säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-231">In hello Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. <span data-ttu-id="dfbfc-233">På hello komma igång-sidan hello guiden för Schemalägg säkerhetskopiering klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-233">On hello Getting started page of hello Schedule Backup Wizard, click **Next**.</span></span>

4. <span data-ttu-id="dfbfc-234">På hello Välj objekt tooBackup klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-234">On hello Select Items tooBackup page, click **Add Items**.</span></span>

5. <span data-ttu-id="dfbfc-235">Välj **systemtillstånd** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-235">Select **System State** and then click **OK**.</span></span>

6. <span data-ttu-id="dfbfc-236">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-236">Click **Next**.</span></span>

7. <span data-ttu-id="dfbfc-237">hello schema för säkerhetskopiering av systemtillstånd och förvaring automatiskt tooback in varje söndag vid 21:00:00 lokal tid, och hello kvarhållningsperiod anges too60 dagar.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-237">hello System State Backup and Retention schedule is automatically set tooback up every Sunday at 9:00 PM local time, and hello retention period is set too60 days.</span></span>

   > [!NOTE]
   > <span data-ttu-id="dfbfc-238">Princip för säkerhetskopiering och kvarhållning av systemtillståndet konfigureras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-238">System State backup and retention policy is automatically configured.</span></span> <span data-ttu-id="dfbfc-239">Om du säkerhetskopierar filer och mappar dessutom toohello systemtillstånd för Windows Server, ange bara hello-säkerhetskopiering och kvarhållning princip för säkerhetskopior av filer från hello guiden.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-239">If you back up Files and Folders in addition toohello Windows Server System State, specify only hello Backup and Retention policy for file backups from hello wizard.</span></span> 
   >

8. <span data-ttu-id="dfbfc-240">På sidan Bekräfta hello granska hello information och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-240">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>

9. <span data-ttu-id="dfbfc-241">När hello guiden är färdig med hello schemat för säkerhetskopiering, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-241">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="tooback-up-windows-server-system-state-for-hello-first-time"></a><span data-ttu-id="dfbfc-242">tooback Windows Server System tillstånd för hello första gången</span><span class="sxs-lookup"><span data-stu-id="dfbfc-242">tooback up Windows Server System State for hello first time</span></span>

1. <span data-ttu-id="dfbfc-243">Kontrollera att det finns inga väntande uppdateringar för Windows Server som kräver en omstart.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-243">Make sure there are no pending updates for Windows Server that require a reboot.</span></span>

2. <span data-ttu-id="dfbfc-244">Klicka på hello Recovery Services-agenten **Säkerhetskopiera nu** toocomplete hello inledande seeding hello nätverket.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-244">In hello Recovery Services agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Säkerhetskopiera Windows Server nu](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. <span data-ttu-id="dfbfc-246">På sidan Bekräfta hello använder hello granskningsinställningar som hello tillbaka nu guiden Installera tooback upp hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-246">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="dfbfc-247">Klicka på **Säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-247">Then click **Back Up**.</span></span>

4. <span data-ttu-id="dfbfc-248">Klicka på **Stäng** tooclose hello guiden.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-248">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="dfbfc-249">Om du stänger guiden hello innan hello säkerhetskopieringen är klar fortsätter hello guiden toorun i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-249">If you close hello wizard before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

5. <span data-ttu-id="dfbfc-250">Om du säkerhetskopierar filer och mappar på din server dessutom status för Windows-System för toohello, kommer hello säkerhetskopiering nu guiden bara säkerhetskopiera filer.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-250">If you back up Files and Folders on your server, in addition toohello Windows Server System State, hello Backup Now wizard will only back up files.</span></span> <span data-ttu-id="dfbfc-251">tooperform en ad hoc-systemtillstånd säkerhetskopiera använder hello följande PowerShell-kommando:</span><span class="sxs-lookup"><span data-stu-id="dfbfc-251">tooperform an ad hoc System State back up, use hello following PowerShell command:</span></span>

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  <span data-ttu-id="dfbfc-252">När hello första säkerhetskopieringen har slutförts hello **jobbet slutfört** status visas i konsolen för hello-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-252">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

  ![IR slutfört](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="dfbfc-254">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="dfbfc-254">Frequently asked questions</span></span>

<span data-ttu-id="dfbfc-255">hello följande frågor och svar ger ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-255">hello following questions and answers provide supplementary information.</span></span>

### <a name="what-is-hello-staging-volume"></a><span data-ttu-id="dfbfc-256">Vad är hello mellanlagring volymen?</span><span class="sxs-lookup"><span data-stu-id="dfbfc-256">What is hello Staging Volume?</span></span>

<span data-ttu-id="dfbfc-257">hello mellanlagring volym representerar hello mellanliggande plats där hello inbyggt, Windows Server Backup skapar etapper hello säkerhetskopia av systemtillståndet.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-257">hello Staging Volume represents hello intermediate location where hello natively available, Windows Server Backup stages hello System State Backup.</span></span> <span data-ttu-id="dfbfc-258">Azure Backup-agenten och komprimerar och krypterar mellanliggande säkerhetskopian och skickar den via säker HTTPS-protokollet toohello konfigurerad Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-258">Azure Backup agent then compresses and encrypts this intermediate backup and sends it via secure HTTPS Protocol toohello configured Recovery Services Vault.</span></span> <span data-ttu-id="dfbfc-259">**Vi rekommenderar starkt att du etablerar hello mellanlagring volymen i en Windows OS-volym. Om du upptäcker problem med säkerhetskopiering av systemtillståndet, kontrollerar hello platsen för mellanlagring volymen är hello första felsökning.**</span><span class="sxs-lookup"><span data-stu-id="dfbfc-259">**We strongly recommended you establish hello Staging Volume in a non-Windows-OS volume. If you observe problems with System State Backups, checking hello location of your Staging Volume is hello first troubleshooting step.**</span></span> 

### <a name="how-can-i-change-hello-staging-volume-path-specified-in-hello-azure-backup-agent"></a><span data-ttu-id="dfbfc-260">Hur kan jag ändra hello mellanlagring volymsökväg som anges i hello Azure Backup-agenten?</span><span class="sxs-lookup"><span data-stu-id="dfbfc-260">How can I change hello Staging Volume Path specified in hello Azure Backup agent?</span></span>

<span data-ttu-id="dfbfc-261">hello mellanlagring volymen finns som standard i hello cachemappen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-261">hello Staging Volume is located in hello cache folder by default.</span></span> 

1. <span data-ttu-id="dfbfc-262">toochange den här platsen, Använd hello följande kommando (i en upphöjd kommandotolk):</span><span class="sxs-lookup"><span data-stu-id="dfbfc-262">toochange this location, use hello following command (in an elevated command prompt):</span></span>
  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="dfbfc-263">Uppdatera hello följande registervärden med hello toohello nya mellanlagring volym sökväg.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-263">Then update hello following registry entries with hello path toohello new Staging Volume folder.</span></span>

  |<span data-ttu-id="dfbfc-264">Sökväg i registret</span><span class="sxs-lookup"><span data-stu-id="dfbfc-264">Registry path</span></span>|<span data-ttu-id="dfbfc-265">Registernyckel</span><span class="sxs-lookup"><span data-stu-id="dfbfc-265">Registry key</span></span>|<span data-ttu-id="dfbfc-266">Värde</span><span class="sxs-lookup"><span data-stu-id="dfbfc-266">Value</span></span>|
  |-------------|------------|-----|
  |<span data-ttu-id="dfbfc-267">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="dfbfc-267">HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="dfbfc-268">SSBStagingPath</span><span class="sxs-lookup"><span data-stu-id="dfbfc-268">SSBStagingPath</span></span> | <span data-ttu-id="dfbfc-269">nya mellanlagringsplatsen volym</span><span class="sxs-lookup"><span data-stu-id="dfbfc-269">new staging volume location</span></span> |

<span data-ttu-id="dfbfc-270">hello mellanlagring sökväg är skiftlägeskänsligt och måste vara exakt samma skiftläge hello som det som finns på hello-servern.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-270">hello Staging Path is case sensitive and must be hello exact same casing as what exists on hello server.</span></span> 

3. <span data-ttu-id="dfbfc-271">Starta om hello säkerhetskopiering motorn när du har ändrat hello mellanlagring Volymsökväg:</span><span class="sxs-lookup"><span data-stu-id="dfbfc-271">Once you change hello Staging volume path, restart hello Backup engine:</span></span>
  ```
  PS C:\> Net start obengine
  ```
4. <span data-ttu-id="dfbfc-272">toopick hello ändras sökvägen, öppna hello Microsoft Azure Recovery Services-agenten och utlösa en ad hoc-säkerhetskopiering av systemtillstånd.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-272">toopick up hello changed path, open hello Microsoft Azure Recovery Services agent and trigger an ad hoc backup of System State.</span></span>

### <a name="why-is-hello-system-state-default-retention-set-too60-days"></a><span data-ttu-id="dfbfc-273">Varför är hello systemtillstånd Standardkvarhållning ställa in too60 dagar?</span><span class="sxs-lookup"><span data-stu-id="dfbfc-273">Why is hello System State default retention set too60 days?</span></span>

<span data-ttu-id="dfbfc-274">en säkerhetskopia av systemtillståndet hello livslängd är hello samma som hello ”tombstone-livslängden” för hello Windows Server Active Directory-rollen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-274">hello useful life of a system state backup is hello same as hello "tombstone lifetime" setting for hello Windows Server Active Directory role.</span></span> <span data-ttu-id="dfbfc-275">hello standardvärdet för posten för hello tombstone-livslängden är 60 dagar.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-275">hello default value for hello tombstone lifetime entry is 60 days.</span></span> <span data-ttu-id="dfbfc-276">Det här värdet kan ställas in på hello konfigurationsobjekt för Directory Service (NTDS).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-276">This value can be set on hello Directory Service (NTDS) config object.</span></span>

### <a name="how-do-i-change-hello-default-backup-and-retention-policy-for-system-state"></a><span data-ttu-id="dfbfc-277">Hur ändrar jag hello standard säkerhetskopiering och bevarandeprincip för systemtillstånd?</span><span class="sxs-lookup"><span data-stu-id="dfbfc-277">How do I change hello default Backup and Retention Policy for System State?</span></span>

<span data-ttu-id="dfbfc-278">toochange hello standard säkerhetskopiering och bevarandeprincip för systemtillstånd:</span><span class="sxs-lookup"><span data-stu-id="dfbfc-278">toochange hello default Backup and Retention Policy for System State:</span></span>
1. <span data-ttu-id="dfbfc-279">Stoppa hello säkerhetskopiering motorn.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-279">Stop hello Backup engine.</span></span> <span data-ttu-id="dfbfc-280">Kör följande kommando från en upphöjd kommandotolk hello.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-280">Run hello following command from an elevated command prompt.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="dfbfc-281">Lägg till eller uppdatera hello följande poster för registernyckel i HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-281">Add or update hello following registry key entries in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span></span>

  |<span data-ttu-id="dfbfc-282">Namn i registret</span><span class="sxs-lookup"><span data-stu-id="dfbfc-282">Registry Name</span></span>|<span data-ttu-id="dfbfc-283">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="dfbfc-283">Description</span></span>|<span data-ttu-id="dfbfc-284">Värde</span><span class="sxs-lookup"><span data-stu-id="dfbfc-284">Value</span></span>|
  |-------------|-----------|-----|
  |<span data-ttu-id="dfbfc-285">SSBScheduleTime</span><span class="sxs-lookup"><span data-stu-id="dfbfc-285">SSBScheduleTime</span></span>|<span data-ttu-id="dfbfc-286">Använda tooconfigure hello tid för hello säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-286">Used tooconfigure hello time of hello backup.</span></span> <span data-ttu-id="dfbfc-287">Standardvärdet är 21: 00 lokaltid.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-287">Default is 9PM local time.</span></span>|<span data-ttu-id="dfbfc-288">DWord: Formatet HHMM (decimalt) till exempel 2130 för 9:30:00 lokal tid</span><span class="sxs-lookup"><span data-stu-id="dfbfc-288">DWord: Format HHMM (Decimal) for example 2130 for 9:30PM local time</span></span>|
  |<span data-ttu-id="dfbfc-289">SSBScheduleDays</span><span class="sxs-lookup"><span data-stu-id="dfbfc-289">SSBScheduleDays</span></span>|<span data-ttu-id="dfbfc-290">Använda tooconfigure hello dagar när systemtillstånd måste utföras på hello angetts tid.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-290">Used tooconfigure hello days when System State Backup must be performed at hello specified time.</span></span> <span data-ttu-id="dfbfc-291">Enskilda siffror ange hello veckodagar.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-291">Individual digits specify days of hello week.</span></span> <span data-ttu-id="dfbfc-292">0 representerar söndag, 1 är måndag och så vidare.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-292">0 represents Sunday, 1 is Monday, and so on.</span></span> <span data-ttu-id="dfbfc-293">Standard dag för säkerhetskopiering är söndag.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-293">Default day for backup is Sunday.</span></span>|<span data-ttu-id="dfbfc-294">DWord: dagarnas hello vecka toorun säkerhetskopiering (decimalt), till exempel 1230 schemalägger säkerhetskopieringar på måndag, tisdag, onsdag och söndag.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-294">DWord: days of hello week toorun backup (decimal) for example 1230 schedules backups on Monday, Tuesday, Wednesday, and Sunday.</span></span>|
  |<span data-ttu-id="dfbfc-295">SSBRetentionDays</span><span class="sxs-lookup"><span data-stu-id="dfbfc-295">SSBRetentionDays</span></span>|<span data-ttu-id="dfbfc-296">Använda tooconfigure hello dagar tooretain säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-296">Used tooconfigure hello days tooretain backup.</span></span> <span data-ttu-id="dfbfc-297">Standardvärdet är 60.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-297">Default value is 60.</span></span> <span data-ttu-id="dfbfc-298">Högsta tillåtna värdet är 180.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-298">Maximum allowed value is 180.</span></span>|<span data-ttu-id="dfbfc-299">DWord: Dagar tooretain säkerhetskopiering (decimalt).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-299">DWord: Days tooretain backup (decimal).</span></span>|

3. <span data-ttu-id="dfbfc-300">Använd hello följande kommando toorestart hello säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-300">Use hello following command toorestart hello backup engine.</span></span>
    ```
    PS C:\> Net start obengine
    ```

4. <span data-ttu-id="dfbfc-301">Öppna hello Microsoft Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-301">Open hello Microsoft Recovery Services agent.</span></span>

5. <span data-ttu-id="dfbfc-302">Klicka på **schemalägga säkerhetskopiering** och klicka sedan på **nästa** tills du ser hello ändringarna återges.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-302">Click **Schedule Backup** and then click **Next** until you see hello changes reflected.</span></span>

6. <span data-ttu-id="dfbfc-303">Klicka på **Slutför** tooapply hello ändringar.</span><span class="sxs-lookup"><span data-stu-id="dfbfc-303">Click **Finish** tooapply hello changes.</span></span>


## <a name="questions"></a><span data-ttu-id="dfbfc-304">Frågor?</span><span class="sxs-lookup"><span data-stu-id="dfbfc-304">Questions?</span></span>
<span data-ttu-id="dfbfc-305">Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-305">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="dfbfc-306">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="dfbfc-306">Next steps</span></span>
* <span data-ttu-id="dfbfc-307">Få mer information om hur du [säkerhetskopierar Windows-datorer](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-307">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="dfbfc-308">Nu när du har säkerhetskopierat dina filer och mappar kan du [hantera dina valv och servrar](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-308">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="dfbfc-309">Om du behöver toorestore en säkerhetskopia kan använda den här artikeln för[återställa filer tooa Windows datorn](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="dfbfc-309">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
