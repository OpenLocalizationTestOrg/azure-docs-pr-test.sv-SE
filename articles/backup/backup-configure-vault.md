---
title: aaaUse Azure Backup agent tooback av filer och mappar | Microsoft Docs
description: "Använd hello Microsoft Azure Backup agent tooback in Windows tooAzure för filer och mappar. Skapa ett Recovery Services-valv, installera säkerhetskopieringsagenten hello, definiera hello säkerhetskopieringsprincip och köra hello första säkerhetskopian på hello filer och mappar."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "säkerhetskopieringsvalvet; Säkerhetskopiera en Windows-server. Säkerhetskopiera windows;"
ms.assetid: 7f5b1943-b3c1-4ddb-8fb7-3560533c68d5
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/15/2017
ms.author: markgal;trinadhk;
ms.openlocfilehash: be203c24841971872b5c6e7cf260a2fa5c58ac47
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="back-up-a-windows-server-or-client-tooazure-using-hello-resource-manager-deployment-model"></a><span data-ttu-id="272cf-105">Säkerhetskopiera en Windows Server eller klient tooAzure med hello Resource Manager-distributionsmodellen</span><span class="sxs-lookup"><span data-stu-id="272cf-105">Back up a Windows Server or client tooAzure using hello Resource Manager deployment model</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="272cf-106">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="272cf-106">Azure portal</span></span>](backup-configure-vault.md)
> * [<span data-ttu-id="272cf-107">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="272cf-107">Classic portal</span></span>](backup-configure-vault-classic.md)
>
>

<span data-ttu-id="272cf-108">Den här artikeln förklarar hur tooback in Windows Server (eller Windows-klient) filer och mappar tooAzure med Azure Backup med hello Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="272cf-108">This article explains how tooback up your Windows Server (or Windows client) files and folders tooAzure with Azure Backup using hello Resource Manager deployment model.</span></span>

[!INCLUDE [learn-about-deployment-models](../../includes/backup-deployment-models.md)]

![Säkerhetskopieringsprocessen steg](./media/backup-configure-vault/initial-backup-process.png)

## <a name="before-you-start"></a><span data-ttu-id="272cf-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="272cf-110">Before you start</span></span>
<span data-ttu-id="272cf-111">tooback konfigurerar en server eller klient tooAzure du behöver ett Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="272cf-111">tooback up a server or client tooAzure, you need an Azure account.</span></span> <span data-ttu-id="272cf-112">Om du inte har någon, kan du skapa en [kostnadsfritt konto](https://azure.microsoft.com/free/) på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="272cf-112">If you don't have one, you can create a [free account](https://azure.microsoft.com/free/) in just a couple of minutes.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="272cf-113">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="272cf-113">Create a Recovery Services vault</span></span>
<span data-ttu-id="272cf-114">Recovery Services-valvet är en entitet som lagrar alla hello säkerhetskopieringar och återställningspunkter som du skapar över tid.</span><span class="sxs-lookup"><span data-stu-id="272cf-114">A Recovery Services vault is an entity that stores all hello backups and recovery points you create over time.</span></span> <span data-ttu-id="272cf-115">hello Recovery Services-valvet innehåller också hello säkerhetskopieringsprincip tillämpas toohello skyddade filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="272cf-115">hello Recovery Services vault also contains hello backup policy applied toohello protected files and folders.</span></span> <span data-ttu-id="272cf-116">När du skapar ett Recovery Services-valv, bör du också välja hello lämpliga redundans lagringsalternativ.</span><span class="sxs-lookup"><span data-stu-id="272cf-116">When you create a Recovery Services vault, you should also select hello appropriate storage redundancy option.</span></span>

### <a name="toocreate-a-recovery-services-vault"></a><span data-ttu-id="272cf-117">toocreate Recovery Services-valvet</span><span class="sxs-lookup"><span data-stu-id="272cf-117">toocreate a Recovery Services vault</span></span>
1. <span data-ttu-id="272cf-118">Om du inte redan har gjort det loggar du in toohello [Azure Portal](https://portal.azure.com/) med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="272cf-118">If you haven't already done so, sign in toohello [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="272cf-119">Hej hubbmenyn, klicka på **fler tjänster** och Skriv i hello lista över resurser, **återställningstjänster** och på **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="272cf-119">On hello Hub menu, click **More services** and in hello list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="272cf-121">Om det finns recovery services-valv i hello prenumeration, visas hello valv.</span><span class="sxs-lookup"><span data-stu-id="272cf-121">If there are recovery services vaults in hello subscription, hello vaults are listed.</span></span>

3. <span data-ttu-id="272cf-122">På hello **Recovery Services-valv** -menyn klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="272cf-122">On hello **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="272cf-124">hello återställningstjänster valvet blad öppnas, där du uppmanas tooprovide en **namn**, **prenumeration**, **resursgruppen**, och **plats**.</span><span class="sxs-lookup"><span data-stu-id="272cf-124">hello Recovery Services vault blade opens, prompting you tooprovide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Skapa Recovery Services-valv (steg 3)](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="272cf-126">För **namn**, ange ett eget namn tooidentify hello valv.</span><span class="sxs-lookup"><span data-stu-id="272cf-126">For **Name**, enter a friendly name tooidentify hello vault.</span></span> <span data-ttu-id="272cf-127">hello namn måste toobe unika för hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="272cf-127">hello name needs toobe unique for hello Azure subscription.</span></span> <span data-ttu-id="272cf-128">Skriv ett namn som innehåller mellan 2 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="272cf-128">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="272cf-129">Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="272cf-129">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="272cf-130">I hello **prenumeration** Använd hello nedrullningsbara menyn toochoose hello Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="272cf-130">In hello **Subscription** section, use hello drop-down menu toochoose hello Azure subscription.</span></span> <span data-ttu-id="272cf-131">Om du använder bara en prenumeration som prenumeration visas och du kan hoppa över toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="272cf-131">If you use only one subscription, that subscription appears and you can skip toohello next step.</span></span> <span data-ttu-id="272cf-132">Om du inte är säker på vilken prenumeration toouse använder standard-hello (eller förslag) prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="272cf-132">If you are not sure which subscription toouse, use hello default (or suggested) subscription.</span></span> <span data-ttu-id="272cf-133">Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="272cf-133">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="272cf-134">I hello **resursgruppen** avsnitt:</span><span class="sxs-lookup"><span data-stu-id="272cf-134">In hello **Resource group** section:</span></span>

    * <span data-ttu-id="272cf-135">Välj **Skapa nytt** om du vill toocreate en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="272cf-135">select **Create new** if you want toocreate a new Resource group.</span></span>
    <span data-ttu-id="272cf-136">Eller</span><span class="sxs-lookup"><span data-stu-id="272cf-136">Or</span></span>
    * <span data-ttu-id="272cf-137">Välj **använda befintliga** och klicka på hello nedrullningsbara menyn toosee hello tillgängliga listan över resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="272cf-137">select **Use existing** and click hello drop-down menu toosee hello available list of Resource groups.</span></span>

  <span data-ttu-id="272cf-138">Mer information om resursgrupper finns i hello [översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="272cf-138">For complete information on Resource groups, see hello [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="272cf-139">Klicka på **plats** tooselect hello geografiskt område för hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="272cf-139">Click **Location** tooselect hello geographic region for hello vault.</span></span> <span data-ttu-id="272cf-140">Det här alternativet anger hello geografiska region där dina säkerhetskopierade data skickas.</span><span class="sxs-lookup"><span data-stu-id="272cf-140">This choice determines hello geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="272cf-141">Hello längst ned på bladet för hello Recovery Services-valvet, klickar du på **skapa**.</span><span class="sxs-lookup"><span data-stu-id="272cf-141">At hello bottom of hello Recovery Services vault blade, click **Create**.</span></span>

  <span data-ttu-id="272cf-142">Det kan ta flera minuter för hello Recovery Services-valvet toobe skapas.</span><span class="sxs-lookup"><span data-stu-id="272cf-142">It can take several minutes for hello Recovery Services vault toobe created.</span></span> <span data-ttu-id="272cf-143">Övervaka hello statusmeddelanden i hello övre högra delen av hello-portalen.</span><span class="sxs-lookup"><span data-stu-id="272cf-143">Monitor hello status notifications in hello upper right-hand area of hello portal.</span></span> <span data-ttu-id="272cf-144">När din valvet har skapats visas den i hello lista över Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="272cf-144">Once your vault is created, it appears in hello list of Recovery Services vaults.</span></span> <span data-ttu-id="272cf-145">Om du inte ser ditt valv efter ett par minuter klickar du på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="272cf-145">If after several minutes you don't see your vault, click **Refresh**.</span></span>

  ![Klicka på Uppdatera](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

  <span data-ttu-id="272cf-147">När du ser ditt valv i hello lista över Recovery Services-valv, är du redo tooset hello lagring redundans.</span><span class="sxs-lookup"><span data-stu-id="272cf-147">Once you see your vault in hello list of Recovery Services vaults, you are ready tooset hello storage redundancy.</span></span>


### <a name="set-storage-redundancy"></a><span data-ttu-id="272cf-148">Ange lagring redundans</span><span class="sxs-lookup"><span data-stu-id="272cf-148">Set storage redundancy</span></span>
<span data-ttu-id="272cf-149">Första gången du skapar ett Recovery Services-valv bestämmer du hur lagringen ska replikeras.</span><span class="sxs-lookup"><span data-stu-id="272cf-149">When you first create a Recovery Services vault you determine how storage is replicated.</span></span>

1. <span data-ttu-id="272cf-150">Från hello **Recovery Services-valv** bladet Klicka hello nya valvet.</span><span class="sxs-lookup"><span data-stu-id="272cf-150">From hello **Recovery Services vaults** blade, click hello new vault.</span></span>

    ![Välj hello nytt valv hello listan över Recovery Services-valvet](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="272cf-152">När du väljer hello valvet hello **Recovery Services-valvet** bladet begränsar och hello inställningsbladet (*som har hello namnet på valvet hello överst hello*) och hello valvet informationsbladet öppna.</span><span class="sxs-lookup"><span data-stu-id="272cf-152">When you select hello vault, hello **Recovery Services vault** blade narrows, and hello Settings blade (*which has hello name of hello vault at hello top*) and hello vault details blade open.</span></span>

    ![Visa hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)

2. <span data-ttu-id="272cf-154">Använda hello lodräta bilden tooscroll ned toohello hantera avsnitt i hello nytt valv inställningar-bladet och klickar på **säkerhetskopiering infrastruktur**.</span><span class="sxs-lookup"><span data-stu-id="272cf-154">In hello new vault's Settings blade, use hello vertical slide tooscroll down toohello Manage section, and click **Backup Infrastructure**.</span></span>

  <span data-ttu-id="272cf-155">hello säkerhetskopiering infrastruktur blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="272cf-155">hello Backup Infrastructure blade opens.</span></span>

3. <span data-ttu-id="272cf-156">I hello säkerhetskopiering infrastruktur-bladet, klickar du på **konfigurering av säkerhetskopiering** tooopen hello **konfigurering av säkerhetskopiering** bladet.</span><span class="sxs-lookup"><span data-stu-id="272cf-156">In hello Backup Infrastructure blade, click **Backup Configuration** tooopen hello **Backup Configuration** blade.</span></span>

  ![Ange hello lagringskonfigurationen för nytt valv](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)

4. <span data-ttu-id="272cf-158">Välj hello lämpliga replikering lagringsalternativ för ditt valv.</span><span class="sxs-lookup"><span data-stu-id="272cf-158">Choose hello appropriate storage replication option for your vault.</span></span>

  ![alternativ för lagringskonfiguration](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

  <span data-ttu-id="272cf-160">Valvet använder geo-redundant lagring som standard.</span><span class="sxs-lookup"><span data-stu-id="272cf-160">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="272cf-161">Om du använder Azure som en primär säkerhetskopieringslagring slutpunkt fortsätta toouse **Geo-redundant**.</span><span class="sxs-lookup"><span data-stu-id="272cf-161">If you use Azure as a primary backup storage endpoint, continue toouse **Geo-redundant**.</span></span> <span data-ttu-id="272cf-162">Om du inte använder Azure som en primär säkerhetskopieringslagring slutpunkt, Välj **lokalt redundant**, vilket minskar hello Azure lagringskostnader.</span><span class="sxs-lookup"><span data-stu-id="272cf-162">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces hello Azure storage costs.</span></span> <span data-ttu-id="272cf-163">Läs mer om alternativen för [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) i denna [översikt av lagringsredundans](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="272cf-163">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="272cf-164">Nu när du har skapat ett valv, förbereda din infrastruktur tooback av filer och mappar genom att ladda ned och installera hello Microsoft Azure Recovery Services-agenten, ladda ned valvautentiseringsuppgifter och sedan använda dessa autentiseringsuppgifter tooregister hello agent med hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="272cf-164">Now that you've created a vault, prepare your infrastructure tooback up files and folders by downloading and installing hello Microsoft Azure Recovery Services agent, downloading vault credentials, and then using those credentials tooregister hello agent with hello vault.</span></span>

## <a name="configure-hello-vault"></a><span data-ttu-id="272cf-165">Konfigurera hello valvet</span><span class="sxs-lookup"><span data-stu-id="272cf-165">Configure hello vault</span></span>

1. <span data-ttu-id="272cf-166">Hej Recovery Services-valvet bladet (för hello valvet du just skapade), i hello komma igång-avsnittet, klickar du på **säkerhetskopiering**, sedan på hello **Kom igång med säkerhetskopiering** bladet väljer  **Säkerhetskopiering målet**.</span><span class="sxs-lookup"><span data-stu-id="272cf-166">On hello Recovery Services vault blade (for hello vault you just created), in hello Getting Started section, click **Backup**, then on hello **Getting Started with Backup** blade, select **Backup goal**.</span></span>

  ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

  <span data-ttu-id="272cf-168">Hej **säkerhetskopiering målet** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="272cf-168">hello **Backup Goal** blade opens.</span></span> <span data-ttu-id="272cf-169">Om hello Recovery Services-valvet har konfigurerats tidigare, sedan hello **säkerhetskopiering målet** blad öppnas när du klickar på **säkerhetskopiering** valvet bladet på hello Recovery Services.</span><span class="sxs-lookup"><span data-stu-id="272cf-169">If hello Recovery Services vault has been previously configured, then hello **Backup Goal** blades opens when you click **Backup** on hello Recovery Services vault blade.</span></span>

  ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="272cf-171">Från hello **var körs din arbetsbelastning?** nedrullningsbara menyn och väljer **lokalt**.</span><span class="sxs-lookup"><span data-stu-id="272cf-171">From hello **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

  <span data-ttu-id="272cf-172">Du väljer **Lokalt** eftersom din Windows Server- eller Windows-dator är en fysisk dator som inte finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="272cf-172">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="272cf-173">Från hello **vad vill du vill toobackup?** väljer du **filer och mappar**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="272cf-173">From hello **What do you want toobackup?** menu, select **Files and folders**, and click **OK**.</span></span>

  ![Konfigurera filer och mappar](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

  <span data-ttu-id="272cf-175">När du klickar på OK, visas en markering nästa för**säkerhetskopiering målet**, och hello **Förbered infrastruktur** blad öppnas.</span><span class="sxs-lookup"><span data-stu-id="272cf-175">After clicking OK, a checkmark appears next too**Backup goal**, and hello **Prepare infrastructure** blade opens.</span></span>

  ![När säkerhetskopieringsmålet har konfigurerats går du vidare och förbereder infrastrukturen](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="272cf-177">På hello **Förbered infrastruktur** bladet, klickar du på **ladda ned Agent för Windows Server och Windows-klient**.</span><span class="sxs-lookup"><span data-stu-id="272cf-177">On hello **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

  ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

  <span data-ttu-id="272cf-179">Om du använder Windows Server viktigt, och välj sedan toodownload hello agent för Windows Server viktigt.</span><span class="sxs-lookup"><span data-stu-id="272cf-179">If you are using Windows Server Essential, then choose toodownload hello agent for Windows Server Essential.</span></span> <span data-ttu-id="272cf-180">Ett popup-menyn efterfrågar toorun eller spara MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="272cf-180">A pop-up menu prompts you toorun or save MARSAgentInstaller.exe.</span></span>

  ![Dialogrutan MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="272cf-182">I hello hämta popup-menyn, klickar du på **spara**.</span><span class="sxs-lookup"><span data-stu-id="272cf-182">In hello download pop-up menu, click **Save**.</span></span>

  <span data-ttu-id="272cf-183">Som standard hello **MARSagentinstaller.exe** tooyour hämtningsmapp sparas i filen.</span><span class="sxs-lookup"><span data-stu-id="272cf-183">By default, hello **MARSagentinstaller.exe** file is saved tooyour Downloads folder.</span></span> <span data-ttu-id="272cf-184">När hello installer är klar visas ett popup-fönster som frågar om du vill toorun hello installer eller öppna hello mapp.</span><span class="sxs-lookup"><span data-stu-id="272cf-184">When hello installer completes, you will see a pop-up asking if you want toorun hello installer, or open hello folder.</span></span>

  ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

  <span data-ttu-id="272cf-186">Du behöver inte tooinstall hello agenten ännu.</span><span class="sxs-lookup"><span data-stu-id="272cf-186">You don't need tooinstall hello agent yet.</span></span> <span data-ttu-id="272cf-187">Du kan installera hello agenten när du har hämtat hello valvautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="272cf-187">You can install hello agent after you have downloaded hello vault credentials.</span></span>

6. <span data-ttu-id="272cf-188">På hello **Förbered infrastruktur** bladet, klickar du på **hämta**.</span><span class="sxs-lookup"><span data-stu-id="272cf-188">On hello **Prepare infrastructure** blade, click **Download**.</span></span>

  ![hämta autentiseringsuppgifter för valvet](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

  <span data-ttu-id="272cf-190">Hej valvautentiseringsuppgifter hämta tooyour hämtningsmapp.</span><span class="sxs-lookup"><span data-stu-id="272cf-190">hello vault credentials download tooyour Downloads folder.</span></span> <span data-ttu-id="272cf-191">När hello valvautentiseringsuppgifter nedladdning, visas ett popup-fönster som frågar om du vill tooopen eller spara hello autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="272cf-191">After hello vault credentials finish downloading, you see a pop-up asking if you want tooopen or save hello credentials.</span></span> <span data-ttu-id="272cf-192">Klicka på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="272cf-192">Click **Save**.</span></span> <span data-ttu-id="272cf-193">Om du råkar klicka på **öppna**, låta hello dialogruta som försöker tooopen hello valvautentiseringsuppgifter, misslyckas.</span><span class="sxs-lookup"><span data-stu-id="272cf-193">If you accidentally click **Open**, let hello dialog that attempts tooopen hello vault credentials, fail.</span></span> <span data-ttu-id="272cf-194">Du kan inte öppna hello valvautentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="272cf-194">You cannot open hello vault credentials.</span></span> <span data-ttu-id="272cf-195">Fortsätt toohello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="272cf-195">Proceed toohello next step.</span></span> <span data-ttu-id="272cf-196">Hej valvautentiseringsuppgifter finns i hello hämtningsmapp.</span><span class="sxs-lookup"><span data-stu-id="272cf-196">hello vault credentials are in hello Downloads folder.</span></span>   

  ![valvautentiseringsuppgifterna har hämtats](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-hello-agent"></a><span data-ttu-id="272cf-198">Installera och registrera hello-agent</span><span class="sxs-lookup"><span data-stu-id="272cf-198">Install and register hello agent</span></span>

> [!NOTE]
> <span data-ttu-id="272cf-199">Aktivera säkerhetskopiering via hello Azure-portalen är inte tillgängligt ännu.</span><span class="sxs-lookup"><span data-stu-id="272cf-199">Enabling backup through hello Azure portal is not available, yet.</span></span> <span data-ttu-id="272cf-200">Använd hello Microsoft Azure Recovery Services-agenten tooback upp filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="272cf-200">Use hello Microsoft Azure Recovery Services Agent tooback up your files and folders.</span></span>
>

1. <span data-ttu-id="272cf-201">Leta upp och dubbelklicka på hello **MARSagentinstaller.exe** från hello hämtningar mappen (eller andra lagringsplatsen).</span><span class="sxs-lookup"><span data-stu-id="272cf-201">Locate and double-click hello **MARSagentinstaller.exe** from hello Downloads folder (or other saved location).</span></span>

  <span data-ttu-id="272cf-202">hello installer innehåller ett antal meddelanden som extraheras, installerar och registrerar hello Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="272cf-202">hello installer provides a series of messages as it extracts, installs, and registers hello Recovery Services agent.</span></span>

  ![Autentiseringsuppgifter för att köra Recovery Services-agentinstallationsprogrammet](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="272cf-204">Slutför hello installationsguiden för Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="272cf-204">Complete hello Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="272cf-205">toocomplete hello guiden måste du:</span><span class="sxs-lookup"><span data-stu-id="272cf-205">toocomplete hello wizard, you need to:</span></span>

  * <span data-ttu-id="272cf-206">Välj en plats för hello installation och cache-mappen.</span><span class="sxs-lookup"><span data-stu-id="272cf-206">Choose a location for hello installation and cache folder.</span></span>
  * <span data-ttu-id="272cf-207">Ange proxyserveradressen serverinformation om du använder en proxy server tooconnect toohello internet.</span><span class="sxs-lookup"><span data-stu-id="272cf-207">Provide your proxy server info if you use a proxy server tooconnect toohello internet.</span></span>
  * <span data-ttu-id="272cf-208">Ange ditt användarnamn och lösenord om du använder en autentiserad proxyserver.</span><span class="sxs-lookup"><span data-stu-id="272cf-208">Provide your user name and password details if you use an authenticated proxy.</span></span>
  * <span data-ttu-id="272cf-209">Ange hello ned valvautentiseringsuppgifter</span><span class="sxs-lookup"><span data-stu-id="272cf-209">Provide hello downloaded vault credentials</span></span>
  * <span data-ttu-id="272cf-210">Spara hello krypteringslösenfrasen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="272cf-210">Save hello encryption passphrase in a secure location.</span></span>

  > [!NOTE]
  > <span data-ttu-id="272cf-211">Om du förlorar eller glömmer hello lösenfras går inte Microsoft att återställa hello säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="272cf-211">If you lose or forget hello passphrase, Microsoft cannot help recover hello backup data.</span></span> <span data-ttu-id="272cf-212">Spara hello-filen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="272cf-212">Save hello file in a secure location.</span></span> <span data-ttu-id="272cf-213">Det är obligatoriskt toorestore en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="272cf-213">It is required toorestore a backup.</span></span>
  >
  >

<span data-ttu-id="272cf-214">hello-agent har installerats och datorn är registrerade toohello valvet.</span><span class="sxs-lookup"><span data-stu-id="272cf-214">hello agent is now installed and your machine is registered toohello vault.</span></span> <span data-ttu-id="272cf-215">Du är klar tooconfigure och schemalägger säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="272cf-215">You're ready tooconfigure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="272cf-216">Nätverks- och anslutningskrav</span><span class="sxs-lookup"><span data-stu-id="272cf-216">Network and Connectivity Requirements</span></span>

<span data-ttu-id="272cf-217">Om din dator/proxy har begränsad tillgång till internet, kan du kontrollera att brandväggen på datorn/proxy – hello är konfigurerade tooallow hello följande URL: er:</span><span class="sxs-lookup"><span data-stu-id="272cf-217">If your machine/proxy has limited internet access, ensure that firewall settings on hello machine/proxy are configured tooallow hello following URLs:</span></span> <br>
    1. <span data-ttu-id="272cf-218">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="272cf-218">www.msftncsi.com</span></span>
    2. <span data-ttu-id="272cf-219">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="272cf-219">*.Microsoft.com</span></span>
    3. <span data-ttu-id="272cf-220">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="272cf-220">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="272cf-221">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="272cf-221">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="272cf-222">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="272cf-222">*.windows.ne</span></span>


## <a name="create-hello-backup-policy"></a><span data-ttu-id="272cf-223">Skapa hello säkerhetskopieringsprincip</span><span class="sxs-lookup"><span data-stu-id="272cf-223">Create hello backup policy</span></span>
<span data-ttu-id="272cf-224">hello säkerhetskopieringsprincip är hello schema när återställningspunkter tas och hello tid hello återställningspunkterna bevaras.</span><span class="sxs-lookup"><span data-stu-id="272cf-224">hello backup policy is hello schedule when recovery points are taken, and hello length of time hello recovery points are retained.</span></span> <span data-ttu-id="272cf-225">Använd hello Microsoft Azure Backup agent toocreate hello säkerhetskopieringsprincip för filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="272cf-225">Use hello Microsoft Azure Backup agent toocreate hello backup policy for files and folders.</span></span>

### <a name="toocreate-a-backup-schedule"></a><span data-ttu-id="272cf-226">toocreate ett schema för säkerhetskopiering</span><span class="sxs-lookup"><span data-stu-id="272cf-226">toocreate a backup schedule</span></span>
1. <span data-ttu-id="272cf-227">Öppna hello Microsoft Azure Backup-agenten.</span><span class="sxs-lookup"><span data-stu-id="272cf-227">Open hello Microsoft Azure Backup agent.</span></span> <span data-ttu-id="272cf-228">Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.</span><span class="sxs-lookup"><span data-stu-id="272cf-228">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Starta hello Azure Backup-agenten](./media/backup-configure-vault/snap-in-search.png)
2. <span data-ttu-id="272cf-230">I hello Backup agent **åtgärder** rutan klickar du på **schemalägga säkerhetskopiering** toolaunch hello guiden för schemat för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="272cf-230">In hello Backup agent's **Actions** pane, click **Schedule Backup** toolaunch hello Schedule Backup Wizard.</span></span>

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-configure-vault/schedule-first-backup.png)

3. <span data-ttu-id="272cf-232">På hello **komma igång** av hello guiden Schemalägg säkerhetskopiering, klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="272cf-232">On hello **Getting started** page of hello Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="272cf-233">På hello **Välj objekt tooBackup** klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="272cf-233">On hello **Select Items tooBackup** page, click **Add Items**.</span></span>

  <span data-ttu-id="272cf-234">hello öppnas Välj objekt.</span><span class="sxs-lookup"><span data-stu-id="272cf-234">hello Select Items dialog opens.</span></span>

5. <span data-ttu-id="272cf-235">Välj hello filer och mappar som du vill tooprotect och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="272cf-235">Select hello files and folders that you want tooprotect, and then click **OK**.</span></span>
6. <span data-ttu-id="272cf-236">I hello **Välj objekt tooBackup** klickar du på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="272cf-236">In hello **Select Items tooBackup** page, click **Next**.</span></span>
7. <span data-ttu-id="272cf-237">På hello **ange schemat för säkerhetskopiering** anger hello schemat för säkerhetskopiering och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="272cf-237">On hello **Specify Backup Schedule** page, specify hello backup schedule and click **Next**.</span></span>

    <span data-ttu-id="272cf-238">Du kan schemalägga säkerhetskopieringar varje dag (högst tre gånger om dagen) eller varje vecka.</span><span class="sxs-lookup"><span data-stu-id="272cf-238">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Windows Server Backup-objekt](./media/backup-configure-vault/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="272cf-240">Mer information om hur toospecify hello schemat för säkerhetskopiering finns hello artikel [Använd Azure Backup tooreplace infrastrukturen band](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="272cf-240">For more information about how toospecify hello backup schedule, see hello article [Use Azure Backup tooreplace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >
   >

8. <span data-ttu-id="272cf-241">På hello **Välj bevarandeprincip** sidan, Välj hello specifika kvarhållning principer hello hello säkerhetskopian och klicka på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="272cf-241">On hello **Select Retention Policy** page, choose hello specific retention policies hello for hello backup copy and click **Next**.</span></span>

    <span data-ttu-id="272cf-242">hello bevarandeprincip anger hello varaktigheten vilka hello-säkerhetskopian lagras.</span><span class="sxs-lookup"><span data-stu-id="272cf-242">hello retention policy specifies hello duration which hello backup is stored.</span></span> <span data-ttu-id="272cf-243">I stället för att bara ange en ”platt policy” för alla återställningspunkter för säkerhetskopiering, kan du ange olika bevarandeprinciper baserat på när hello säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="272cf-243">Rather than just specifying a “flat policy” for all backup points, you can specify different retention policies based on when hello backup occurs.</span></span> <span data-ttu-id="272cf-244">Du kan ändra hello dagliga, veckovisa, månatliga och årliga Kvarhållningsintervall principer toomeet dina behov.</span><span class="sxs-lookup"><span data-stu-id="272cf-244">You can modify hello daily, weekly, monthly, and yearly retention policies toomeet your needs.</span></span>
9. <span data-ttu-id="272cf-245">Välj hello inledande säkerhetskopieringstyp hello på sidan Välj typ av inledande säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="272cf-245">On hello Choose Initial Backup Type page, choose hello initial backup type.</span></span> <span data-ttu-id="272cf-246">Lämna alternativet hello **automatiskt över hello nätverk** markerad och klicka sedan på **nästa**.</span><span class="sxs-lookup"><span data-stu-id="272cf-246">Leave hello option **Automatically over hello network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="272cf-247">Du kan säkerhetskopiera automatiskt över hello nätverk eller du kan säkerhetskopiera offline.</span><span class="sxs-lookup"><span data-stu-id="272cf-247">You can back up automatically over hello network, or you can back up offline.</span></span> <span data-ttu-id="272cf-248">hello resten av den här artikeln beskrivs hello säkerhetskopiera automatiskt.</span><span class="sxs-lookup"><span data-stu-id="272cf-248">hello remainder of this article describes hello process for backing up automatically.</span></span> <span data-ttu-id="272cf-249">Om du föredrar toodo en offlinesäkerhetskopiering granska hello artikel [Offline säkerhetskopiering arbetsflödet i Azure Backup](backup-azure-backup-import-export.md) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="272cf-249">If you prefer toodo an offline backup, review hello article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="272cf-250">På sidan Bekräfta hello granska hello information och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="272cf-250">On hello Confirmation page, review hello information, and then click **Finish**.</span></span>
11. <span data-ttu-id="272cf-251">När hello guiden är färdig med hello schemat för säkerhetskopiering, klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="272cf-251">After hello wizard finishes creating hello backup schedule, click **Close**.</span></span>

### <a name="enable-network-throttling"></a><span data-ttu-id="272cf-252">Aktivera begränsning</span><span class="sxs-lookup"><span data-stu-id="272cf-252">Enable network throttling</span></span>
<span data-ttu-id="272cf-253">hello Microsoft Azure Backup agent innehåller nätverksbegränsning.</span><span class="sxs-lookup"><span data-stu-id="272cf-253">hello Microsoft Azure Backup agent provides network throttling.</span></span> <span data-ttu-id="272cf-254">Begränsningskontroller hur nätverksbandbredden används när data överfördes.</span><span class="sxs-lookup"><span data-stu-id="272cf-254">Throttling controls how network bandwidth is used during data transfer.</span></span> <span data-ttu-id="272cf-255">Den här kontrollen kan vara användbart om du behöver tooback upp data under arbetstid men inte vill att hello säkerhetskopieringsprocessen toointerfere med andra Internettrafik.</span><span class="sxs-lookup"><span data-stu-id="272cf-255">This control can be helpful if you need tooback up data during work hours but do not want hello backup process toointerfere with other Internet traffic.</span></span> <span data-ttu-id="272cf-256">Begränsning gäller tooback upp och återställa aktiviteter.</span><span class="sxs-lookup"><span data-stu-id="272cf-256">Throttling applies tooback up and restore activities.</span></span>

> [!NOTE]
> <span data-ttu-id="272cf-257">Nätverksbegränsning är inte tillgänglig på Windows Server 2008 R2 SP1, Windows Server 2008 SP2 eller Windows 7 (utan servicepack).</span><span class="sxs-lookup"><span data-stu-id="272cf-257">Network throttling is not available on Windows Server 2008 R2 SP1, Windows Server 2008 SP2, or Windows 7 (with service packs).</span></span> <span data-ttu-id="272cf-258">hello Azure Backup nätverket begränsning funktionen aktiverar tjänstkvalitet (QoS) på hello lokala operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="272cf-258">hello Azure Backup network throttling feature engages Quality of Service (QoS) on hello local operating system.</span></span> <span data-ttu-id="272cf-259">Även om Azure Backup kan skydda dessa operativsystem, fungerar inte hello version av QoS finns på dessa plattformar med Azure Backup nätverksbegränsning.</span><span class="sxs-lookup"><span data-stu-id="272cf-259">Though Azure Backup can protect these operating systems, hello version of QoS available on these platforms doesn't work with Azure Backup network throttling.</span></span> <span data-ttu-id="272cf-260">Nätverksbegränsning kan användas på alla andra [operativsystem](backup-azure-backup-faq.md).</span><span class="sxs-lookup"><span data-stu-id="272cf-260">Network throttling can be used on all other [supported operating systems](backup-azure-backup-faq.md).</span></span>
>
>

<span data-ttu-id="272cf-261">**tooenable nätverksbegränsning**</span><span class="sxs-lookup"><span data-stu-id="272cf-261">**tooenable network throttling**</span></span>

1. <span data-ttu-id="272cf-262">Klicka på hello Microsoft Azure Backup agent **ändra egenskaper för**.</span><span class="sxs-lookup"><span data-stu-id="272cf-262">In hello Microsoft Azure Backup agent, click **Change Properties**.</span></span>

    ![Ändra egenskaper för](./media/backup-configure-vault/change-properties.png)
2. <span data-ttu-id="272cf-264">På hello **begränsning** fliken, väljer hello **aktivera Användningsbegränsning för internetbandbredd för säkerhetskopieringsåtgärder** kryssrutan.</span><span class="sxs-lookup"><span data-stu-id="272cf-264">On hello **Throttling** tab, select hello **Enable internet bandwidth usage throttling for backup operations** check box.</span></span>

    ![Nätverksbegränsning](./media/backup-configure-vault/throttling-dialog.png)
3. <span data-ttu-id="272cf-266">När du har aktiverat begränsning, ange hello tillåtet bandbredd för överföring av säkerhetskopierade data under **arbetstid** och **icke-arbetstid**.</span><span class="sxs-lookup"><span data-stu-id="272cf-266">After you have enabled throttling, specify hello allowed bandwidth for backup data transfer during **Work hours** and **Non-work hours**.</span></span>

    <span data-ttu-id="272cf-267">hello bandbredd värden börjar på 512 kilobit per sekund (Kbps) och gå upp too1 023 megabyte per sekund (MBps).</span><span class="sxs-lookup"><span data-stu-id="272cf-267">hello bandwidth values begin at 512 kilobits per second (Kbps) and can go up too1,023 megabytes per second (MBps).</span></span> <span data-ttu-id="272cf-268">Du kan också ange hello start och avslutas för **arbetstid**, och vilka hello veckodagar som anses vara arbetsdagar.</span><span class="sxs-lookup"><span data-stu-id="272cf-268">You can also designate hello start and finish for **Work hours**, and which days of hello week are considered work days.</span></span> <span data-ttu-id="272cf-269">Timmar utanför avsedda arbete timmar anses fungerar timmar.</span><span class="sxs-lookup"><span data-stu-id="272cf-269">Hours outside of designated work hours are considered non-work hours.</span></span>
4. <span data-ttu-id="272cf-270">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="272cf-270">Click **OK**.</span></span>

### <a name="tooback-up-files-and-folders-for-hello-first-time"></a><span data-ttu-id="272cf-271">tooback av filer och mappar för hello första gången</span><span class="sxs-lookup"><span data-stu-id="272cf-271">tooback up files and folders for hello first time</span></span>
1. <span data-ttu-id="272cf-272">I hello backup-agenten, klickar du på **Säkerhetskopiera nu** toocomplete hello inledande seeding hello nätverket.</span><span class="sxs-lookup"><span data-stu-id="272cf-272">In hello backup agent, click **Back Up Now** toocomplete hello initial seeding over hello network.</span></span>

    ![Säkerhetskopiera Windows Server nu](./media/backup-configure-vault/backup-now.png)
2. <span data-ttu-id="272cf-274">På sidan Bekräfta hello använder hello granskningsinställningar som hello tillbaka nu guiden Installera tooback upp hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="272cf-274">On hello Confirmation page, review hello settings that hello Back Up Now Wizard will use tooback up hello machine.</span></span> <span data-ttu-id="272cf-275">Klicka på **Säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="272cf-275">Then click **Back Up**.</span></span>
3. <span data-ttu-id="272cf-276">Klicka på **Stäng** tooclose hello guiden.</span><span class="sxs-lookup"><span data-stu-id="272cf-276">Click **Close** tooclose hello wizard.</span></span> <span data-ttu-id="272cf-277">Om du gör detta innan hello säkerhetskopieringen är klar fortsätter hello guiden toorun i hello bakgrund.</span><span class="sxs-lookup"><span data-stu-id="272cf-277">If you do this before hello backup process finishes, hello wizard continues toorun in hello background.</span></span>

<span data-ttu-id="272cf-278">När hello första säkerhetskopieringen har slutförts hello **jobbet slutfört** status visas i konsolen för hello-säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="272cf-278">After hello initial backup is completed, hello **Job completed** status appears in hello Backup console.</span></span>

![IR slutfört](./media/backup-configure-vault/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="272cf-280">Frågor?</span><span class="sxs-lookup"><span data-stu-id="272cf-280">Questions?</span></span>
<span data-ttu-id="272cf-281">Om du har frågor eller om det inte finns någon funktion som du vill att toosee ingår, [skicka feedback](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="272cf-281">If you have questions, or if there is any feature that you would like toosee included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="272cf-282">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="272cf-282">Next steps</span></span>
<span data-ttu-id="272cf-283">För ytterligare information om hur du säkerhetskopierar virtuella datorer eller andra arbetsbelastningar, se:</span><span class="sxs-lookup"><span data-stu-id="272cf-283">For additional information about backing up VMs or other workloads, see:</span></span>

* <span data-ttu-id="272cf-284">Nu när du har säkerhetskopierat dina filer och mappar kan du [hantera dina valv och servrar](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="272cf-284">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="272cf-285">Om du behöver toorestore en säkerhetskopia kan använda den här artikeln för[återställa filer tooa Windows datorn](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="272cf-285">If you need toorestore a backup, use this article too[restore files tooa Windows machine](backup-azure-restore-windows-server.md).</span></span>
