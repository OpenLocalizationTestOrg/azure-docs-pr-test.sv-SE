---
title: "Säkerhetskopiera Windows systemtillståndet till Azure | Microsoft Docs"
description: "Lär dig hur du säkerhetskopierar systemtillståndet för Windows Server och/eller Windows datorer till Azure."
services: backup
documentationcenter: 
author: saurabhsensharma
manager: carmonm
editor: 
keywords: "hur du säkerhetskopierar; säkerhetskopiera; säkerhetskopiera filer och mappar"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/31/2017
ms.author: saurse;markgal
ms.openlocfilehash: 6fbd96935f444d8b0c6d068ebd0d28e612f19816
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="back-up-windows-system-state-in-resource-manager-deployment"></a><span data-ttu-id="ef22e-104">Säkerhetskopiera systemtillståndet för Windows i Resource Manager-distribution</span><span class="sxs-lookup"><span data-stu-id="ef22e-104">Back up Windows system state in Resource Manager deployment</span></span>
<span data-ttu-id="ef22e-105">Den här artikeln förklarar hur du säkerhetskopierar systemtillståndet Windows Server till Azure.</span><span class="sxs-lookup"><span data-stu-id="ef22e-105">This article explains how to back up your Windows Server system state to Azure.</span></span> <span data-ttu-id="ef22e-106">I den här självstudiekursen går vi igenom grunderna.</span><span class="sxs-lookup"><span data-stu-id="ef22e-106">It's a tutorial intended to walk you through the basics.</span></span>

<span data-ttu-id="ef22e-107">Om du vill veta mer om Azure Backup läser du den här [översikten](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="ef22e-107">If you want to know more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="ef22e-108">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) som ger dig åtkomst till Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="ef22e-108">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="ef22e-109">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="ef22e-109">Create a recovery services vault</span></span>
<span data-ttu-id="ef22e-110">Innan du kan säkerhetskopiera filer och mappar måste du skapa ett Recovery Services-valv i den region som du vill lagra informationen i.</span><span class="sxs-lookup"><span data-stu-id="ef22e-110">To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data.</span></span> <span data-ttu-id="ef22e-111">Du måste också bestämma hur du vill att lagringen ska replikeras.</span><span class="sxs-lookup"><span data-stu-id="ef22e-111">You also need to determine how you want your storage replicated.</span></span>

### <a name="to-create-a-recovery-services-vault"></a><span data-ttu-id="ef22e-112">Så här skapar du ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="ef22e-112">To create a Recovery Services vault</span></span>
1. <span data-ttu-id="ef22e-113">Om du inte redan gjort det loggar du in på [Azure-portalen](https://portal.azure.com/) med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="ef22e-113">If you haven't already done so, sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="ef22e-114">På navigeringsmenyn klickar du på **Fler tjänster** och skriver **Recovery Services** i listan över resurser och klickar sedan på **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-114">On the Hub menu, click **More services** and in the list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="ef22e-116">Om det finns Recovery Services-valv i prenumerationen visas valven.</span><span class="sxs-lookup"><span data-stu-id="ef22e-116">If there are recovery services vaults in the subscription, the vaults are listed.</span></span>
3. <span data-ttu-id="ef22e-117">På menyn **Recovery Services-valv** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-117">On the **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="ef22e-119">Bladet Recovery Services-valv öppnas och du uppmanas att ange **namn**, **prenumeration**, **resursgrupp** och **plats**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-119">The Recovery Services vault blade opens, prompting you to provide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Skapa Recovery Services-valv (steg 3)](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="ef22e-121">I **Namn** anger du ett eget namn som identifierar valvet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-121">For **Name**, enter a friendly name to identify the vault.</span></span> <span data-ttu-id="ef22e-122">Namnet måste vara unikt för Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-122">The name needs to be unique for the Azure subscription.</span></span> <span data-ttu-id="ef22e-123">Skriv ett namn som innehåller mellan 2 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="ef22e-123">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="ef22e-124">Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="ef22e-124">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="ef22e-125">I avsnittet **Prenumeration** använder du listrutan för att välja Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-125">In the **Subscription** section, use the drop-down menu to choose the Azure subscription.</span></span> <span data-ttu-id="ef22e-126">Om du bara använder en prenumeration visas den och du kan gå vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="ef22e-126">If you use only one subscription, that subscription appears and you can skip to the next step.</span></span> <span data-ttu-id="ef22e-127">Om du inte är säker på vilken prenumeration du ska använda använder du standardprenumerationen (eller den föreslagna).</span><span class="sxs-lookup"><span data-stu-id="ef22e-127">If you are not sure which subscription to use, use the default (or suggested) subscription.</span></span> <span data-ttu-id="ef22e-128">Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="ef22e-128">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="ef22e-129">Gör följande i avsnittet **Resursgrupp**:</span><span class="sxs-lookup"><span data-stu-id="ef22e-129">In the **Resource group** section:</span></span>

    * <span data-ttu-id="ef22e-130">Välj **Skapa ny** om du vill skapa en resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="ef22e-130">select **Create new** if you want to create a Resource group.</span></span>
    <span data-ttu-id="ef22e-131">Eller</span><span class="sxs-lookup"><span data-stu-id="ef22e-131">Or</span></span>
    * <span data-ttu-id="ef22e-132">Välj **Använd befintlig** och klicka på listrutan om du vill se listan över tillgängliga resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="ef22e-132">select **Use existing** and click the drop-down menu to see the available list of Resource groups.</span></span>

  <span data-ttu-id="ef22e-133">Fullständig information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="ef22e-133">For complete information on Resource groups, see the [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="ef22e-134">Klicka på **Plats** för att välja en geografisk region för valvet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-134">Click **Location** to select the geographic region for the vault.</span></span> <span data-ttu-id="ef22e-135">Det här alternativet anger den geografiska region som dina säkerhetskopierade data skickas till.</span><span class="sxs-lookup"><span data-stu-id="ef22e-135">This choice determines the geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="ef22e-136">Längst ned på bladet för Recovery Services-valvet klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-136">At the bottom of the Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="ef22e-137">Det kan ta flera minuter innan Recovery Services-valvet har skapats.</span><span class="sxs-lookup"><span data-stu-id="ef22e-137">It can take several minutes for the Recovery Services vault to be created.</span></span> <span data-ttu-id="ef22e-138">Övervaka statusmeddelandena uppe till höger i portalen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-138">Monitor the status notifications in the upper right-hand area of the portal.</span></span> <span data-ttu-id="ef22e-139">När valvet har skapats visas det i listan över Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="ef22e-139">Once your vault is created, it appears in the list of Recovery Services vaults.</span></span> <span data-ttu-id="ef22e-140">Om du inte ser ditt valv efter ett par minuter klickar du på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-140">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Klicka på Uppdatera](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="ef22e-142">När du ser valvet i listan över Recovery Services-valv kan du ange lagringsredundansen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-142">Once you see your vault in the list of Recovery Services vaults, you are ready to set the storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-the-vault"></a><span data-ttu-id="ef22e-143">Ange lagringsredundans för valvet</span><span class="sxs-lookup"><span data-stu-id="ef22e-143">Set storage redundancy for the vault</span></span>
<span data-ttu-id="ef22e-144">När du skapar ett Recovery Services-valv ska du alltid kontrollera att lagringsredundansen är konfigurerad på det sätt som du vill.</span><span class="sxs-lookup"><span data-stu-id="ef22e-144">When you create a Recovery Services vault, make sure storage redundancy is configured the way you want.</span></span>

1. <span data-ttu-id="ef22e-145">På bladet **Recovery Services-valv** klickar du på det nya valvet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-145">From the **Recovery Services vaults** blade, click the new vault.</span></span>

    ![Välj det nya valvet i listan över Recovery Services-valv](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="ef22e-147">Om du väljer valvet minimeras bladet **Recovery Services-valv** och bladet Inställningar (*som har namnet på valvet överst*) och bladet med valvinformation öppnas.</span><span class="sxs-lookup"><span data-stu-id="ef22e-147">When you select the vault, the **Recovery Services vault** blade narrows, and the Settings blade (*which has the name of the vault at the top*) and the vault details blade open.</span></span>

    ![Visa lagringskonfigurationen för det nya valvet](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="ef22e-149">På det nya valvets inställningsblad använder du det lodräta reglaget och bläddrar ned till avsnittet Hantera. Där klickar du på **Infrastruktur för säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-149">In the new vault's Settings blade, use the vertical slide to scroll down to the Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="ef22e-150">Bladet Infrastruktur för säkerhetskopiering öppnas.</span><span class="sxs-lookup"><span data-stu-id="ef22e-150">The Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="ef22e-151">På bladet Infrastruktur för säkerhetskopiering klickar du på **Konfiguration av säkerhetskopiering** för att öppna bladet **Konfiguration av säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-151">In the Backup Infrastructure blade, click **Backup Configuration** to open the **Backup Configuration** blade.</span></span>

    ![Ange lagringskonfigurationen för det nya valvet](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="ef22e-153">Välj lämpligt alternativ för lagringsreplikering för valvet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-153">Choose the appropriate storage replication option for your vault.</span></span>

    ![alternativ för lagringskonfiguration](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="ef22e-155">Valvet använder geo-redundant lagring som standard.</span><span class="sxs-lookup"><span data-stu-id="ef22e-155">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="ef22e-156">Om du använder Azure som primär slutpunkt för lagring av säkerhetskopior fortsätter du att använda **geo-redundant** lagring.</span><span class="sxs-lookup"><span data-stu-id="ef22e-156">If you use Azure as a primary backup storage endpoint, continue to use **Geo-redundant**.</span></span> <span data-ttu-id="ef22e-157">Om du inte använder Azure som en slutpunkt för primär lagring av säkerhetskopior väljer du **Lokalt redundant**, vilket minskar kostnaderna för Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="ef22e-157">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces the Azure storage costs.</span></span> <span data-ttu-id="ef22e-158">Läs mer om alternativen för [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) i denna [översikt av lagringsredundans](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="ef22e-158">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="ef22e-159">Nu när du har skapat ett valv, kan du konfigurera den för att säkerhetskopiera systemtillståndet i Windows.</span><span class="sxs-lookup"><span data-stu-id="ef22e-159">Now that you've created a vault, configure it for backing up Windows System State.</span></span>

## <a name="configure-the-vault"></a><span data-ttu-id="ef22e-160">Konfigurera valvet</span><span class="sxs-lookup"><span data-stu-id="ef22e-160">Configure the vault</span></span>
1. <span data-ttu-id="ef22e-161">På bladet Recovery Services-valv (för valvet som du precis har skapat) klickar du på **Säkerhetskopiering** i avsnittet Komma igång och väljer sedan **Säkerhetskopieringsmål** på bladet **Kom igång med säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-161">On the Recovery Services vault blade (for the vault you just created), in the Getting Started section, click **Backup**, then on the **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="ef22e-163">Bladet **Säkerhetskopieringsmål** öppnas.</span><span class="sxs-lookup"><span data-stu-id="ef22e-163">The **Backup Goal** blade opens.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="ef22e-165">Från listrutan **Var körs din arbetsbelastning?** väljer du **Lokalt**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-165">From the **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="ef22e-166">Du väljer **Lokalt** eftersom din Windows Server- eller Windows-dator är en fysisk dator som inte finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="ef22e-166">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="ef22e-167">Från den **vad vill du säkerhetskopiera?** väljer du **systemtillstånd**, och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-167">From the **What do you want to backup?** menu, select **System State**, and click **OK**.</span></span>

    ![Konfigurera filer och mappar](./media/backup-azure-system-state/backup-goal-system-state.png)

    <span data-ttu-id="ef22e-169">När du klickar på OK visas en markering bredvid **Säkerhetskopieringsmål** och bladet **Förbered infrastruktur** öppnas.</span><span class="sxs-lookup"><span data-stu-id="ef22e-169">After clicking OK, a checkmark appears next to **Backup goal**, and the **Prepare infrastructure** blade opens.</span></span>

    ![När säkerhetskopieringsmålet har konfigurerats går du vidare och förbereder infrastrukturen](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="ef22e-171">På bladet **Förbered infrastruktur** klickar du på **Ladda ned agent för Windows Server eller Windows Client**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-171">On the **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="ef22e-173">Om du använder Windows Server Essential väljer du att hämta agenten för Windows Server Essential.</span><span class="sxs-lookup"><span data-stu-id="ef22e-173">If you are using Windows Server Essential, then choose to download the agent for Windows Server Essential.</span></span> <span data-ttu-id="ef22e-174">En popup-meny visas och du uppmanas att köra eller spara MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="ef22e-174">A pop-up menu prompts you to run or save MARSAgentInstaller.exe.</span></span>

    ![Dialogrutan MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="ef22e-176">På snabbmenyn för hämtningen klickar du på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-176">In the download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="ef22e-177">Som standard sparas filen **MARSagentinstaller.exe** i mappen för nedladdningar.</span><span class="sxs-lookup"><span data-stu-id="ef22e-177">By default, the **MARSagentinstaller.exe** file is saved to your Downloads folder.</span></span> <span data-ttu-id="ef22e-178">När installationsprogrammet har slutförts visas ett popup-fönster och du tillfrågas om du vill köra installationsprogrammet eller öppna mappen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-178">When the installer completes, you will see a pop-up asking if you want to run the installer, or open the folder.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="ef22e-180">Du behöver inte installera agenten än.</span><span class="sxs-lookup"><span data-stu-id="ef22e-180">You don't need to install the agent yet.</span></span> <span data-ttu-id="ef22e-181">Du kan installera agenten när du har hämtat valvautentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="ef22e-181">You can install the agent after you have downloaded the vault credentials.</span></span>

6. <span data-ttu-id="ef22e-182">Klicka på **Ladda ned** på bladet **Förbered infrastruktur**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-182">On the **Prepare infrastructure** blade, click **Download**.</span></span>

    ![hämta autentiseringsuppgifter för valvet](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="ef22e-184">Autentiseringsuppgifterna för valvet hämtas till mappen Hämtningsbara filer.</span><span class="sxs-lookup"><span data-stu-id="ef22e-184">The vault credentials download to your Downloads folder.</span></span> <span data-ttu-id="ef22e-185">När autentiseringsuppgifterna för valvet har hämtats visas ett popup-fönster och du tillfrågas om du vill öppna eller spara autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="ef22e-185">After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials.</span></span> <span data-ttu-id="ef22e-186">Klicka på **Save** (Spara).</span><span class="sxs-lookup"><span data-stu-id="ef22e-186">Click **Save**.</span></span> <span data-ttu-id="ef22e-187">Om du råkar klicka på **Öppna** av misstag väntar du tills dialogrutan som försöker öppna autentiseringsuppgifterna för valvet misslyckas.</span><span class="sxs-lookup"><span data-stu-id="ef22e-187">If you accidentally click **Open**, let the dialog that attempts to open the vault credentials, fail.</span></span> <span data-ttu-id="ef22e-188">Du kan inte öppna valvautentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="ef22e-188">You cannot open the vault credentials.</span></span> <span data-ttu-id="ef22e-189">Gå vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="ef22e-189">Proceed to the next step.</span></span> <span data-ttu-id="ef22e-190">Valvautentiseringsuppgifterna finns i mappen Hämtade filer.</span><span class="sxs-lookup"><span data-stu-id="ef22e-190">The vault credentials are in the Downloads folder.</span></span>   

    ![valvautentiseringsuppgifterna har hämtats](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a><span data-ttu-id="ef22e-192">Installera och registrera agenten</span><span class="sxs-lookup"><span data-stu-id="ef22e-192">Install and register the agent</span></span>

> [!NOTE]
> <span data-ttu-id="ef22e-193">Aktivering av säkerhetskopiering via Azure Portal är inte tillgängligt ännu.</span><span class="sxs-lookup"><span data-stu-id="ef22e-193">Enabling backup through the Azure portal is not available, yet.</span></span> <span data-ttu-id="ef22e-194">Använda Microsoft Azure Recovery Services-agenten för säkerhetskopiering av systemtillstånd för Windows Server.</span><span class="sxs-lookup"><span data-stu-id="ef22e-194">Use the Microsoft Azure Recovery Services Agent to back up Windows Server System State.</span></span>
>

1. <span data-ttu-id="ef22e-195">Leta upp och dubbelklicka på **MARSagentinstaller.exe** från mappen för nedladdade filer (eller en annan lagringsplats).</span><span class="sxs-lookup"><span data-stu-id="ef22e-195">Locate and double-click the **MARSagentinstaller.exe** from the Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="ef22e-196">Installationsprogrammet visar ett antal meddelanden när Recovery Services-agenten extraheras, installeras och registreras.</span><span class="sxs-lookup"><span data-stu-id="ef22e-196">The installer provides a series of messages as it extracts, installs, and registers the Recovery Services agent.</span></span>

    ![Autentiseringsuppgifter för att köra Recovery Services-agentinstallationsprogrammet](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="ef22e-198">Slutför installationsguiden för Microsoft Azure Recovery Services Agent.</span><span class="sxs-lookup"><span data-stu-id="ef22e-198">Complete the Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="ef22e-199">För att slutföra guiden måste du:</span><span class="sxs-lookup"><span data-stu-id="ef22e-199">To complete the wizard, you need to:</span></span>

   * <span data-ttu-id="ef22e-200">Välja en plats för installationen och cachelagringsmappen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-200">Choose a location for the installation and cache folder.</span></span>
   * <span data-ttu-id="ef22e-201">Ange information om proxyservern om du använder en proxyserver för att ansluta till Internet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-201">Provide your proxy server info if you use a proxy server to connect to the internet.</span></span>
   * <span data-ttu-id="ef22e-202">Ange ditt användarnamn och lösenord om du använder en autentiserad proxyserver.</span><span class="sxs-lookup"><span data-stu-id="ef22e-202">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="ef22e-203">Ange autentiseringsuppgifterna som du hämtat för valvet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-203">Provide the downloaded vault credentials</span></span>
   * <span data-ttu-id="ef22e-204">Spara krypteringslösenfrasen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="ef22e-204">Save the encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="ef22e-205">Om du tappar bort eller glömmer lösenfrasen kan Microsoft inte hjälpa dig att återställa dina säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="ef22e-205">If you lose or forget the passphrase, Microsoft cannot help recover the backup data.</span></span> <span data-ttu-id="ef22e-206">Spara filen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="ef22e-206">Save the file in a secure location.</span></span> <span data-ttu-id="ef22e-207">Det krävs för att återställa en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="ef22e-207">It is required to restore a backup.</span></span>
     >
     >

<span data-ttu-id="ef22e-208">Nu installeras agenten och datorn registreras i valvet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-208">The agent is now installed and your machine is registered to the vault.</span></span> <span data-ttu-id="ef22e-209">Nu kan du konfigurera och schemalägga säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-209">You're ready to configure and schedule your backup.</span></span>

## <a name="back-up-windows-server-system-state-preview"></a><span data-ttu-id="ef22e-210">Säkerhetskopiera systemtillståndet för Windows Server (förhandsgranskning)</span><span class="sxs-lookup"><span data-stu-id="ef22e-210">Back up Windows Server System State (Preview)</span></span>
<span data-ttu-id="ef22e-211">Den första säkerhetskopian innehåller tre uppgifter:</span><span class="sxs-lookup"><span data-stu-id="ef22e-211">The initial backup includes three tasks:</span></span>

* <span data-ttu-id="ef22e-212">Aktivera säkerhetskopiering av systemtillstånd med Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="ef22e-212">Enable System State Backup using the Azure Backup agent</span></span>
* <span data-ttu-id="ef22e-213">Schemalägg säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="ef22e-213">Schedule the backup</span></span>
* <span data-ttu-id="ef22e-214">Säkerhetskopiera filer och mappar för första gången</span><span class="sxs-lookup"><span data-stu-id="ef22e-214">Back up files and folders for the first time</span></span>

<span data-ttu-id="ef22e-215">För att slutföra den första säkerhetskopieringen använder du Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="ef22e-215">To complete the initial backup, use the Microsoft Azure Recovery Services agent.</span></span>

### <a name="to-enable-system-state-backup-using-the-azure-backup-agent"></a><span data-ttu-id="ef22e-216">Så här aktiverar du säkerhetskopian av systemtillstånd med Azure Backup-agenten</span><span class="sxs-lookup"><span data-stu-id="ef22e-216">To enable System State backup using the Azure Backup agent</span></span>

1. <span data-ttu-id="ef22e-217">Kör följande kommando för att stoppa Azure Backup-motorn i en PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="ef22e-217">In a PowerShell session, run the following command to stop the Azure Backup engine.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="ef22e-218">Öppna Windows-registret.</span><span class="sxs-lookup"><span data-stu-id="ef22e-218">Open the Windows Registry.</span></span>

  ```
  PS C:\> regedit.exe
  ```

3. <span data-ttu-id="ef22e-219">Lägg till följande registernyckel med det angivna DWord-värdet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-219">Add the following registry key with the specified DWord Value.</span></span>

  | <span data-ttu-id="ef22e-220">Sökväg i registret</span><span class="sxs-lookup"><span data-stu-id="ef22e-220">Registry path</span></span> | <span data-ttu-id="ef22e-221">Registernyckel</span><span class="sxs-lookup"><span data-stu-id="ef22e-221">Registry key</span></span> | <span data-ttu-id="ef22e-222">DWord-värde</span><span class="sxs-lookup"><span data-stu-id="ef22e-222">DWord value</span></span> |
  |---------------|--------------|-------------|
  | <span data-ttu-id="ef22e-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="ef22e-223">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="ef22e-224">TurnOffSSBFeature</span><span class="sxs-lookup"><span data-stu-id="ef22e-224">TurnOffSSBFeature</span></span> | <span data-ttu-id="ef22e-225">2</span><span class="sxs-lookup"><span data-stu-id="ef22e-225">2</span></span> |

4. <span data-ttu-id="ef22e-226">Starta om motorn för säkerhetskopiering genom att köra följande kommando i en kommandotolk med förhöjd behörighet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-226">Restart the Backup engine by executing the following command in an elevated command prompt.</span></span>

  ```
  PS C:\> Net start obengine
  ```

### <a name="to-schedule-the-backup-job"></a><span data-ttu-id="ef22e-227">Så här schemalägger du säkerhetskopieringsjobbet</span><span class="sxs-lookup"><span data-stu-id="ef22e-227">To schedule the backup job</span></span>

1. <span data-ttu-id="ef22e-228">Öppna Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="ef22e-228">Open the Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="ef22e-229">Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.</span><span class="sxs-lookup"><span data-stu-id="ef22e-229">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Starta Azure Recovery Services-agenten](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)

2. <span data-ttu-id="ef22e-231">Klicka på Recovery Services-agenten och sedan på **Schemalägg säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-231">In the Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)

3. <span data-ttu-id="ef22e-233">Klicka på **Nästa** på sidan Komma igång i guiden Schemalägg säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="ef22e-233">On the Getting started page of the Schedule Backup Wizard, click **Next**.</span></span>

4. <span data-ttu-id="ef22e-234">På sidan Välj objekt som ska säkerhetskopieras klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-234">On the Select Items to Backup page, click **Add Items**.</span></span>

5. <span data-ttu-id="ef22e-235">Välj **systemtillstånd** och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-235">Select **System State** and then click **OK**.</span></span>

6. <span data-ttu-id="ef22e-236">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-236">Click **Next**.</span></span>

7. <span data-ttu-id="ef22e-237">Schemat för säkerhetskopiering av systemtillstånd och kvarhållning anges automatiskt att säkerhetskopiera varje söndag vid 21:00:00 lokal tid och kvarhållningsperioden anges till 60 dagar.</span><span class="sxs-lookup"><span data-stu-id="ef22e-237">The System State Backup and Retention schedule is automatically set to back up every Sunday at 9:00 PM local time, and the retention period is set to 60 days.</span></span>

   > [!NOTE]
   > <span data-ttu-id="ef22e-238">Princip för säkerhetskopiering och kvarhållning av systemtillståndet konfigureras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="ef22e-238">System State backup and retention policy is automatically configured.</span></span> <span data-ttu-id="ef22e-239">Ange endast säkerhetskopiering och kvarhållning principen för säkerhetskopior av filer från guiden om du säkerhetskopierar filer och mappar samt status för Windows Server-System.</span><span class="sxs-lookup"><span data-stu-id="ef22e-239">If you back up Files and Folders in addition to the Windows Server System State, specify only the Backup and Retention policy for file backups from the wizard.</span></span> 
   >

8. <span data-ttu-id="ef22e-240">Läs informationen på sidan Bekräftelse och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-240">On the Confirmation page, review the information, and then click **Finish**.</span></span>

9. <span data-ttu-id="ef22e-241">När guiden har skapat säkerhetskopieringsschemat klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-241">After the wizard finishes creating the backup schedule, click **Close**.</span></span>

### <a name="to-back-up-windows-server-system-state-for-the-first-time"></a><span data-ttu-id="ef22e-242">Säkerhetskopiera Windows Server System tillstånd för första gången</span><span class="sxs-lookup"><span data-stu-id="ef22e-242">To back up Windows Server System State for the first time</span></span>

1. <span data-ttu-id="ef22e-243">Kontrollera att det finns inga väntande uppdateringar för Windows Server som kräver en omstart.</span><span class="sxs-lookup"><span data-stu-id="ef22e-243">Make sure there are no pending updates for Windows Server that require a reboot.</span></span>

2. <span data-ttu-id="ef22e-244">I Recovery Services-agenten klickar du på **Säkerhetskopiera nu** för att slutföra en inledande seeding över nätverket.</span><span class="sxs-lookup"><span data-stu-id="ef22e-244">In the Recovery Services agent, click **Back Up Now** to complete the initial seeding over the network.</span></span>

    ![Säkerhetskopiera Windows Server nu](./media/backup-try-azure-backup-in-10-mins/backup-now.png)

3. <span data-ttu-id="ef22e-246">Gå igenom inställningarna på sidan Bekräftelse som guiden Säkerhetskopiera nu ska använda för att säkerhetskopiera datorn.</span><span class="sxs-lookup"><span data-stu-id="ef22e-246">On the Confirmation page, review the settings that the Back Up Now Wizard will use to back up the machine.</span></span> <span data-ttu-id="ef22e-247">Klicka på **Säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-247">Then click **Back Up**.</span></span>

4. <span data-ttu-id="ef22e-248">Stäng guiden genom att klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="ef22e-248">Click **Close** to close the wizard.</span></span> <span data-ttu-id="ef22e-249">Om du stänger guiden innan säkerhetskopieringen är klar fortsätter guiden att köras i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="ef22e-249">If you close the wizard before the backup process finishes, the wizard continues to run in the background.</span></span>

5. <span data-ttu-id="ef22e-250">Om du säkerhetskopierar filer och mappar på din server, förutom systemtillstånd för Windows Server, kommer guiden Säkerhetskopiering nu bara säkerhetskopiera filer.</span><span class="sxs-lookup"><span data-stu-id="ef22e-250">If you back up Files and Folders on your server, in addition to the Windows Server System State, the Backup Now wizard will only back up files.</span></span> <span data-ttu-id="ef22e-251">Använd följande PowerShell-kommando för att utföra en ad hoc-systemtillstånd säkerhetskopiera:</span><span class="sxs-lookup"><span data-stu-id="ef22e-251">To perform an ad hoc System State back up, use the following PowerShell command:</span></span>

    ```
    PS C:\> Start-OBSystemStateBackup
    ```

  <span data-ttu-id="ef22e-252">När den första säkerhetskopieringen har slutförts visas statusen **Jobbet har slutförts** i säkerhetskopieringskonsolen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-252">After the initial backup is completed, the **Job completed** status appears in the Backup console.</span></span>

  ![IR slutfört](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="frequently-asked-questions"></a><span data-ttu-id="ef22e-254">Vanliga frågor och svar</span><span class="sxs-lookup"><span data-stu-id="ef22e-254">Frequently asked questions</span></span>

<span data-ttu-id="ef22e-255">Följande frågor och svar kan du ange ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="ef22e-255">The following questions and answers provide supplementary information.</span></span>

### <a name="what-is-the-staging-volume"></a><span data-ttu-id="ef22e-256">Vad är mellanlagring volymen?</span><span class="sxs-lookup"><span data-stu-id="ef22e-256">What is the Staging Volume?</span></span>

<span data-ttu-id="ef22e-257">Mellanlagring volymen representerar den mellanliggande platsen där den inbyggt, Windows Server Backup skapar säkerhetskopian av systemtillståndet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-257">The Staging Volume represents the intermediate location where the natively available, Windows Server Backup stages the System State Backup.</span></span> <span data-ttu-id="ef22e-258">Azure Backup-agenten och sedan komprimeras och krypterar mellanliggande säkerhetskopian och skickar den via säker HTTPS-protokoll till konfigurerade Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="ef22e-258">Azure Backup agent then compresses and encrypts this intermediate backup and sends it via secure HTTPS Protocol to the configured Recovery Services Vault.</span></span> <span data-ttu-id="ef22e-259">**Vi rekommenderar starkt att du etablerar mellanlagring volymen i en Windows OS-volym. Om du upptäcker problem med säkerhetskopiering av systemtillståndet, Kontrollera platsen för mellanlagring volymen är det första steget för felsökning.**</span><span class="sxs-lookup"><span data-stu-id="ef22e-259">**We strongly recommended you establish the Staging Volume in a non-Windows-OS volume. If you observe problems with System State Backups, checking the location of your Staging Volume is the first troubleshooting step.**</span></span> 

### <a name="how-can-i-change-the-staging-volume-path-specified-in-the-azure-backup-agent"></a><span data-ttu-id="ef22e-260">Hur kan jag ändra mellanlagring volym sökvägen som anges i Azure Backup-agenten?</span><span class="sxs-lookup"><span data-stu-id="ef22e-260">How can I change the Staging Volume Path specified in the Azure Backup agent?</span></span>

<span data-ttu-id="ef22e-261">Mellanlagring volymen finns som standard i cachemappen.</span><span class="sxs-lookup"><span data-stu-id="ef22e-261">The Staging Volume is located in the cache folder by default.</span></span> 

1. <span data-ttu-id="ef22e-262">Om du vill ändra den här platsen, använder du följande kommando (i en upphöjd kommandotolk):</span><span class="sxs-lookup"><span data-stu-id="ef22e-262">To change this location, use the following command (in an elevated command prompt):</span></span>
  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="ef22e-263">Uppdatera följande registerposter med sökvägen till mappen mellanlagring volym.</span><span class="sxs-lookup"><span data-stu-id="ef22e-263">Then update the following registry entries with the path to the new Staging Volume folder.</span></span>

  |<span data-ttu-id="ef22e-264">Sökväg i registret</span><span class="sxs-lookup"><span data-stu-id="ef22e-264">Registry path</span></span>|<span data-ttu-id="ef22e-265">Registernyckel</span><span class="sxs-lookup"><span data-stu-id="ef22e-265">Registry key</span></span>|<span data-ttu-id="ef22e-266">Värde</span><span class="sxs-lookup"><span data-stu-id="ef22e-266">Value</span></span>|
  |-------------|------------|-----|
  |<span data-ttu-id="ef22e-267">HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span><span class="sxs-lookup"><span data-stu-id="ef22e-267">HKEY_LOCAL_MACHINE\Software\Microsoft\Windows Azure Backup\Config\CloudBackupProvider</span></span> | <span data-ttu-id="ef22e-268">SSBStagingPath</span><span class="sxs-lookup"><span data-stu-id="ef22e-268">SSBStagingPath</span></span> | <span data-ttu-id="ef22e-269">nya mellanlagringsplatsen volym</span><span class="sxs-lookup"><span data-stu-id="ef22e-269">new staging volume location</span></span> |

<span data-ttu-id="ef22e-270">Sökvägen till mellanlagring är skiftlägeskänsligt och måste vara exakt samma skiftläge som det som finns på servern.</span><span class="sxs-lookup"><span data-stu-id="ef22e-270">The Staging Path is case sensitive and must be the exact same casing as what exists on the server.</span></span> 

3. <span data-ttu-id="ef22e-271">Starta om motorn för säkerhetskopiering när du har ändrat volymsökväg mellanlagring:</span><span class="sxs-lookup"><span data-stu-id="ef22e-271">Once you change the Staging volume path, restart the Backup engine:</span></span>
  ```
  PS C:\> Net start obengine
  ```
4. <span data-ttu-id="ef22e-272">Öppna Microsoft Azure Recovery Services-agenten för att hämta upp den ändrade sökvägen och Utlös en ad hoc-säkerhetskopiering av systemtillstånd.</span><span class="sxs-lookup"><span data-stu-id="ef22e-272">To pick up the changed path, open the Microsoft Azure Recovery Services agent and trigger an ad hoc backup of System State.</span></span>

### <a name="why-is-the-system-state-default-retention-set-to-60-days"></a><span data-ttu-id="ef22e-273">Varför anges systemtillstånd Standardkvarhållning till 60 dagar?</span><span class="sxs-lookup"><span data-stu-id="ef22e-273">Why is the System State default retention set to 60 days?</span></span>

<span data-ttu-id="ef22e-274">En säkerhetskopia av systemtillståndet livslängd är samma som ”tombstone-livslängden” för rollen Windows Server Active Directory.</span><span class="sxs-lookup"><span data-stu-id="ef22e-274">The useful life of a system state backup is the same as the "tombstone lifetime" setting for the Windows Server Active Directory role.</span></span> <span data-ttu-id="ef22e-275">Standardvärdet för posten tombstone-livslängden är 60 dagar.</span><span class="sxs-lookup"><span data-stu-id="ef22e-275">The default value for the tombstone lifetime entry is 60 days.</span></span> <span data-ttu-id="ef22e-276">Det här värdet kan ställas in på konfigurationsobjekt för Directory Service (NTDS).</span><span class="sxs-lookup"><span data-stu-id="ef22e-276">This value can be set on the Directory Service (NTDS) config object.</span></span>

### <a name="how-do-i-change-the-default-backup-and-retention-policy-for-system-state"></a><span data-ttu-id="ef22e-277">Hur kan jag ändra standard säkerhetskopiering och bevarandeprincip för systemtillstånd?</span><span class="sxs-lookup"><span data-stu-id="ef22e-277">How do I change the default Backup and Retention Policy for System State?</span></span>

<span data-ttu-id="ef22e-278">Ändra standard säkerhetskopiering och bevarandeprincip för systemtillstånd:</span><span class="sxs-lookup"><span data-stu-id="ef22e-278">To change the default Backup and Retention Policy for System State:</span></span>
1. <span data-ttu-id="ef22e-279">Stäng av motorn för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="ef22e-279">Stop the Backup engine.</span></span> <span data-ttu-id="ef22e-280">Kör följande kommando från en upphöjd kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ef22e-280">Run the following command from an elevated command prompt.</span></span>

  ```
  PS C:\> Net stop obengine
  ```

2. <span data-ttu-id="ef22e-281">Lägg till eller uppdatera följande viktiga registerposter i HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span><span class="sxs-lookup"><span data-stu-id="ef22e-281">Add or update the following registry key entries in HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Azure Backup\Config\CloudBackupProvider.</span></span>

  |<span data-ttu-id="ef22e-282">Namn i registret</span><span class="sxs-lookup"><span data-stu-id="ef22e-282">Registry Name</span></span>|<span data-ttu-id="ef22e-283">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="ef22e-283">Description</span></span>|<span data-ttu-id="ef22e-284">Värde</span><span class="sxs-lookup"><span data-stu-id="ef22e-284">Value</span></span>|
  |-------------|-----------|-----|
  |<span data-ttu-id="ef22e-285">SSBScheduleTime</span><span class="sxs-lookup"><span data-stu-id="ef22e-285">SSBScheduleTime</span></span>|<span data-ttu-id="ef22e-286">Används för att konfigurera tidpunkten för säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="ef22e-286">Used to configure the time of the backup.</span></span> <span data-ttu-id="ef22e-287">Standardvärdet är 21: 00 lokaltid.</span><span class="sxs-lookup"><span data-stu-id="ef22e-287">Default is 9PM local time.</span></span>|<span data-ttu-id="ef22e-288">DWord: Formatet HHMM (decimalt) till exempel 2130 för 9:30:00 lokal tid</span><span class="sxs-lookup"><span data-stu-id="ef22e-288">DWord: Format HHMM (Decimal) for example 2130 for 9:30PM local time</span></span>|
  |<span data-ttu-id="ef22e-289">SSBScheduleDays</span><span class="sxs-lookup"><span data-stu-id="ef22e-289">SSBScheduleDays</span></span>|<span data-ttu-id="ef22e-290">Används för att konfigurera de dagar då systemtillstånd måste utföras på den angivna tiden.</span><span class="sxs-lookup"><span data-stu-id="ef22e-290">Used to configure the days when System State Backup must be performed at the specified time.</span></span> <span data-ttu-id="ef22e-291">Enskilda siffror Ange dagar i veckan.</span><span class="sxs-lookup"><span data-stu-id="ef22e-291">Individual digits specify days of the week.</span></span> <span data-ttu-id="ef22e-292">0 representerar söndag, 1 är måndag och så vidare.</span><span class="sxs-lookup"><span data-stu-id="ef22e-292">0 represents Sunday, 1 is Monday, and so on.</span></span> <span data-ttu-id="ef22e-293">Standard dag för säkerhetskopiering är söndag.</span><span class="sxs-lookup"><span data-stu-id="ef22e-293">Default day for backup is Sunday.</span></span>|<span data-ttu-id="ef22e-294">DWord: dagar i veckan för att köra säkerhetskopiering (decimalt), till exempel 1230 schemalägger säkerhetskopieringar på måndag, tisdag, onsdag och söndag.</span><span class="sxs-lookup"><span data-stu-id="ef22e-294">DWord: days of the week to run backup (decimal) for example 1230 schedules backups on Monday, Tuesday, Wednesday, and Sunday.</span></span>|
  |<span data-ttu-id="ef22e-295">SSBRetentionDays</span><span class="sxs-lookup"><span data-stu-id="ef22e-295">SSBRetentionDays</span></span>|<span data-ttu-id="ef22e-296">Används för att konfigurera dagar för att bevara säkerhetskopior.</span><span class="sxs-lookup"><span data-stu-id="ef22e-296">Used to configure the days to retain backup.</span></span> <span data-ttu-id="ef22e-297">Standardvärdet är 60.</span><span class="sxs-lookup"><span data-stu-id="ef22e-297">Default value is 60.</span></span> <span data-ttu-id="ef22e-298">Högsta tillåtna värdet är 180.</span><span class="sxs-lookup"><span data-stu-id="ef22e-298">Maximum allowed value is 180.</span></span>|<span data-ttu-id="ef22e-299">DWord: Dagar att behålla säkerhetskopiering (decimalt).</span><span class="sxs-lookup"><span data-stu-id="ef22e-299">DWord: Days to retain backup (decimal).</span></span>|

3. <span data-ttu-id="ef22e-300">Använd följande kommando för att starta om säkerhetskopieringen motorn.</span><span class="sxs-lookup"><span data-stu-id="ef22e-300">Use the following command to restart the backup engine.</span></span>
    ```
    PS C:\> Net start obengine
    ```

4. <span data-ttu-id="ef22e-301">Öppna Microsoft Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="ef22e-301">Open the Microsoft Recovery Services agent.</span></span>

5. <span data-ttu-id="ef22e-302">Klicka på **schemalägga säkerhetskopiering** och klicka sedan på **nästa** tills du ser ändringarna återges.</span><span class="sxs-lookup"><span data-stu-id="ef22e-302">Click **Schedule Backup** and then click **Next** until you see the changes reflected.</span></span>

6. <span data-ttu-id="ef22e-303">Klicka på **Slutför** att tillämpa ändringarna.</span><span class="sxs-lookup"><span data-stu-id="ef22e-303">Click **Finish** to apply the changes.</span></span>


## <a name="questions"></a><span data-ttu-id="ef22e-304">Frågor?</span><span class="sxs-lookup"><span data-stu-id="ef22e-304">Questions?</span></span>
<span data-ttu-id="ef22e-305">Om du har frågor eller om du saknar en funktion är du välkommen att [lämna feedback](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="ef22e-305">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="ef22e-306">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ef22e-306">Next steps</span></span>
* <span data-ttu-id="ef22e-307">Få mer information om hur du [säkerhetskopierar Windows-datorer](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="ef22e-307">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="ef22e-308">Nu när du har säkerhetskopierat dina filer och mappar kan du [hantera dina valv och servrar](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="ef22e-308">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="ef22e-309">Om du behöver återställa en säkerhetskopia använder du den här artikeln för att [återställa filer till en Windows-dator](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="ef22e-309">If you need to restore a backup, use this article to [restore files to a Windows machine](backup-azure-restore-windows-server.md).</span></span>
