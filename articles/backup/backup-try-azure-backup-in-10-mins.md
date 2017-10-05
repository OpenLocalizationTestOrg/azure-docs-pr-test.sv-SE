---
title: "Säkerhetskopiera Windows-filer och -mappar till Azure (Resource Manager) | Microsoft Docs"
description: "Lär dig att säkerhetskopiera Windows-filer och -mappar till Azure i en distribution av resurshanteraren."
services: backup
documentationcenter: 
author: markgalioto
manager: carmonm
editor: 
keywords: "hur du säkerhetskopierar; säkerhetskopiera; säkerhetskopiera filer och mappar"
ms.assetid: 5b15ebf1-2214-4722-b937-96e2be8872bb
ms.service: backup
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 8/15/2017
ms.author: markgal;
ms.openlocfilehash: b21edb70eca3ec9552dc157ee3bb658d243b8fcd
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="first-look-back-up-files-and-folders-in-resource-manager-deployment"></a><span data-ttu-id="4899e-104">En första titt: Säkerhetskopiera filer och mappar med Resource Manager-distributions</span><span class="sxs-lookup"><span data-stu-id="4899e-104">First look: back up files and folders in Resource Manager deployment</span></span>
<span data-ttu-id="4899e-105">Den här artikeln förklarar hur du säkerhetskopierar filer och mappar i Windows Server (eller en Windows-dator) till Azure med hjälp av Resource Manager-distribution.</span><span class="sxs-lookup"><span data-stu-id="4899e-105">This article explains how to back up your Windows Server (or Windows computer) files and folders to Azure using a Resource Manager deployment.</span></span> <span data-ttu-id="4899e-106">I den här självstudiekursen går vi igenom grunderna.</span><span class="sxs-lookup"><span data-stu-id="4899e-106">It's a tutorial intended to walk you through the basics.</span></span> <span data-ttu-id="4899e-107">Om du vill komma igång med Azure Backup är du på rätt ställe.</span><span class="sxs-lookup"><span data-stu-id="4899e-107">If you want to get started using Azure Backup, you're in the right place.</span></span>

<span data-ttu-id="4899e-108">Om du vill veta mer om Azure Backup läser du den här [översikten](backup-introduction-to-azure-backup.md).</span><span class="sxs-lookup"><span data-stu-id="4899e-108">If you want to know more about Azure Backup, read this [overview](backup-introduction-to-azure-backup.md).</span></span>

<span data-ttu-id="4899e-109">Om du inte har en Azure-prenumeration kan du skapa ett [kostnadsfritt konto](https://azure.microsoft.com/free/) som ger dig åtkomst till Azure-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="4899e-109">If you don't have an Azure subscription, create a [free account](https://azure.microsoft.com/free/) that lets you access any Azure service.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="4899e-110">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="4899e-110">Create a recovery services vault</span></span>
<span data-ttu-id="4899e-111">Innan du kan säkerhetskopiera filer och mappar måste du skapa ett Recovery Services-valv i den region som du vill lagra informationen i.</span><span class="sxs-lookup"><span data-stu-id="4899e-111">To back up your files and folders, you need to create a Recovery Services vault in the region where you want to store the data.</span></span> <span data-ttu-id="4899e-112">Du måste också bestämma hur du vill att lagringen ska replikeras.</span><span class="sxs-lookup"><span data-stu-id="4899e-112">You also need to determine how you want your storage replicated.</span></span>

### <a name="to-create-a-recovery-services-vault"></a><span data-ttu-id="4899e-113">Så här skapar du ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="4899e-113">To create a Recovery Services vault</span></span>
1. <span data-ttu-id="4899e-114">Om du inte redan gjort det loggar du in på [Azure-portalen](https://portal.azure.com/) med din Azure-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="4899e-114">If you haven't already done so, sign in to the [Azure Portal](https://portal.azure.com/) using your Azure subscription.</span></span>
2. <span data-ttu-id="4899e-115">På navigeringsmenyn klickar du på **Fler tjänster** och skriver **Recovery Services** i listan över resurser och klickar sedan på **Recovery Services-valv**.</span><span class="sxs-lookup"><span data-stu-id="4899e-115">On the Hub menu, click **More services** and in the list of resources, type **Recovery Services** and click **Recovery Services vaults**.</span></span>

    ![Skapa Recovery Services-valv (steg 1)](./media/backup-try-azure-backup-in-10-mins/open-rs-vault-list.png) <br/>

    <span data-ttu-id="4899e-117">Om det finns Recovery Services-valv i prenumerationen visas valven.</span><span class="sxs-lookup"><span data-stu-id="4899e-117">If there are recovery services vaults in the subscription, the vaults are listed.</span></span>
3. <span data-ttu-id="4899e-118">På menyn **Recovery Services-valv** klickar du på **Lägg till**.</span><span class="sxs-lookup"><span data-stu-id="4899e-118">On the **Recovery Services vaults** menu, click **Add**.</span></span>

    ![Skapa Recovery Services-valv (steg 2)](./media/backup-try-azure-backup-in-10-mins/rs-vault-menu.png)

    <span data-ttu-id="4899e-120">Bladet Recovery Services-valv öppnas och du uppmanas att ange **namn**, **prenumeration**, **resursgrupp** och **plats**.</span><span class="sxs-lookup"><span data-stu-id="4899e-120">The Recovery Services vault blade opens, prompting you to provide a **Name**, **Subscription**, **Resource group**, and **Location**.</span></span>

    ![Skapa Recovery Services-valv (steg 3)](./media/backup-try-azure-backup-in-10-mins/rs-vault-step-3.png)

4. <span data-ttu-id="4899e-122">I **Namn** anger du ett eget namn som identifierar valvet.</span><span class="sxs-lookup"><span data-stu-id="4899e-122">For **Name**, enter a friendly name to identify the vault.</span></span> <span data-ttu-id="4899e-123">Namnet måste vara unikt för Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="4899e-123">The name needs to be unique for the Azure subscription.</span></span> <span data-ttu-id="4899e-124">Skriv ett namn som innehåller mellan 2 och 50 tecken.</span><span class="sxs-lookup"><span data-stu-id="4899e-124">Type a name that contains between 2 and 50 characters.</span></span> <span data-ttu-id="4899e-125">Det måste börja med en bokstav och får endast innehålla bokstäver, siffror och bindestreck.</span><span class="sxs-lookup"><span data-stu-id="4899e-125">It must start with a letter, and can contain only letters, numbers, and hyphens.</span></span>

5. <span data-ttu-id="4899e-126">I avsnittet **Prenumeration** använder du listrutan för att välja Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="4899e-126">In the **Subscription** section, use the drop-down menu to choose the Azure subscription.</span></span> <span data-ttu-id="4899e-127">Om du bara använder en prenumeration visas den och du kan gå vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="4899e-127">If you use only one subscription, that subscription appears and you can skip to the next step.</span></span> <span data-ttu-id="4899e-128">Om du inte är säker på vilken prenumeration du ska använda använder du standardprenumerationen (eller den föreslagna).</span><span class="sxs-lookup"><span data-stu-id="4899e-128">If you are not sure which subscription to use, use the default (or suggested) subscription.</span></span> <span data-ttu-id="4899e-129">Du kan bara välja mellan flera alternativ om ditt organisationskonto är associerat med flera Azure-prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="4899e-129">There are multiple choices only if your organizational account is associated with multiple Azure subscriptions.</span></span>

6. <span data-ttu-id="4899e-130">Gör följande i avsnittet **Resursgrupp**:</span><span class="sxs-lookup"><span data-stu-id="4899e-130">In the **Resource group** section:</span></span>

    * <span data-ttu-id="4899e-131">Välj **Skapa nytt** om du vill skapa en ny resursgrupp.</span><span class="sxs-lookup"><span data-stu-id="4899e-131">select **Create new** if you want to create a new Resource group.</span></span>
    <span data-ttu-id="4899e-132">Eller</span><span class="sxs-lookup"><span data-stu-id="4899e-132">Or</span></span>
    * <span data-ttu-id="4899e-133">Välj **Använd befintlig** och klicka på listrutan om du vill se listan över tillgängliga resursgrupper.</span><span class="sxs-lookup"><span data-stu-id="4899e-133">select **Use existing** and click the drop-down menu to see the available list of Resource groups.</span></span>

  <span data-ttu-id="4899e-134">Fullständig information om resursgrupper finns i [Översikt över Azure Resource Manager](../azure-resource-manager/resource-group-overview.md).</span><span class="sxs-lookup"><span data-stu-id="4899e-134">For complete information on Resource groups, see the [Azure Resource Manager overview](../azure-resource-manager/resource-group-overview.md).</span></span>

7. <span data-ttu-id="4899e-135">Klicka på **Plats** för att välja en geografisk region för valvet.</span><span class="sxs-lookup"><span data-stu-id="4899e-135">Click **Location** to select the geographic region for the vault.</span></span> <span data-ttu-id="4899e-136">Det här alternativet anger den geografiska region som dina säkerhetskopierade data skickas till.</span><span class="sxs-lookup"><span data-stu-id="4899e-136">This choice determines the geographic region where your backup data is sent.</span></span>

8. <span data-ttu-id="4899e-137">Längst ned på bladet för Recovery Services-valvet klickar du på **Skapa**.</span><span class="sxs-lookup"><span data-stu-id="4899e-137">At the bottom of the Recovery Services vault blade, click **Create**.</span></span>

    <span data-ttu-id="4899e-138">Det kan ta flera minuter innan Recovery Services-valvet har skapats.</span><span class="sxs-lookup"><span data-stu-id="4899e-138">It can take several minutes for the Recovery Services vault to be created.</span></span> <span data-ttu-id="4899e-139">Övervaka statusmeddelandena uppe till höger i portalen.</span><span class="sxs-lookup"><span data-stu-id="4899e-139">Monitor the status notifications in the upper right-hand area of the portal.</span></span> <span data-ttu-id="4899e-140">När valvet har skapats visas det i listan över Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="4899e-140">Once your vault is created, it appears in the list of Recovery Services vaults.</span></span> <span data-ttu-id="4899e-141">Om du inte ser ditt valv efter ett par minuter klickar du på **Uppdatera**.</span><span class="sxs-lookup"><span data-stu-id="4899e-141">If after several minutes you don't see your vault, click **Refresh**.</span></span>

    ![Klicka på Uppdatera](./media/backup-try-azure-backup-in-10-mins/refresh-button.png)</br>

    <span data-ttu-id="4899e-143">När du ser valvet i listan över Recovery Services-valv kan du ange lagringsredundansen.</span><span class="sxs-lookup"><span data-stu-id="4899e-143">Once you see your vault in the list of Recovery Services vaults, you are ready to set the storage redundancy.</span></span>

### <a name="set-storage-redundancy-for-the-vault"></a><span data-ttu-id="4899e-144">Ange lagringsredundans för valvet</span><span class="sxs-lookup"><span data-stu-id="4899e-144">Set storage redundancy for the vault</span></span>
<span data-ttu-id="4899e-145">När du skapar ett Recovery Services-valv ska du alltid kontrollera att lagringsredundansen är konfigurerad på det sätt som du vill.</span><span class="sxs-lookup"><span data-stu-id="4899e-145">When you create a Recovery Services vault, make sure storage redundancy is configured the way you want.</span></span>

1. <span data-ttu-id="4899e-146">På bladet **Recovery Services-valv** klickar du på det nya valvet.</span><span class="sxs-lookup"><span data-stu-id="4899e-146">From the **Recovery Services vaults** blade, click the new vault.</span></span>

    ![Välj det nya valvet i listan över Recovery Services-valv](./media/backup-try-azure-backup-in-10-mins/rs-vault-list.png)

    <span data-ttu-id="4899e-148">Om du väljer valvet minimeras bladet **Recovery Services-valv** och bladet Inställningar (*som har namnet på valvet överst*) och bladet med valvinformation öppnas.</span><span class="sxs-lookup"><span data-stu-id="4899e-148">When you select the vault, the **Recovery Services vault** blade narrows, and the Settings blade (*which has the name of the vault at the top*) and the vault details blade open.</span></span>

    ![Visa lagringskonfigurationen för det nya valvet](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration-2.png)
2. <span data-ttu-id="4899e-150">På det nya valvets inställningsblad använder du det lodräta reglaget och bläddrar ned till avsnittet Hantera. Där klickar du på **Infrastruktur för säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="4899e-150">In the new vault's Settings blade, use the vertical slide to scroll down to the Manage section, and click **Backup Infrastructure**.</span></span>
    <span data-ttu-id="4899e-151">Bladet Infrastruktur för säkerhetskopiering öppnas.</span><span class="sxs-lookup"><span data-stu-id="4899e-151">The Backup Infrastructure blade opens.</span></span>
3. <span data-ttu-id="4899e-152">På bladet Infrastruktur för säkerhetskopiering klickar du på **Konfiguration av säkerhetskopiering** för att öppna bladet **Konfiguration av säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="4899e-152">In the Backup Infrastructure blade, click **Backup Configuration** to open the **Backup Configuration** blade.</span></span>

    ![Ange lagringskonfigurationen för det nya valvet](./media/backup-try-azure-backup-in-10-mins/set-storage-configuration.png)
4. <span data-ttu-id="4899e-154">Välj lämpligt alternativ för lagringsreplikering för valvet.</span><span class="sxs-lookup"><span data-stu-id="4899e-154">Choose the appropriate storage replication option for your vault.</span></span>

    ![alternativ för lagringskonfiguration](./media/backup-try-azure-backup-in-10-mins/choose-storage-configuration.png)

    <span data-ttu-id="4899e-156">Valvet använder geo-redundant lagring som standard.</span><span class="sxs-lookup"><span data-stu-id="4899e-156">By default, your vault has geo-redundant storage.</span></span> <span data-ttu-id="4899e-157">Om du använder Azure som primär slutpunkt för lagring av säkerhetskopior fortsätter du att använda **geo-redundant** lagring.</span><span class="sxs-lookup"><span data-stu-id="4899e-157">If you use Azure as a primary backup storage endpoint, continue to use **Geo-redundant**.</span></span> <span data-ttu-id="4899e-158">Om du inte använder Azure som en slutpunkt för primär lagring av säkerhetskopior väljer du **Lokalt redundant**, vilket minskar kostnaderna för Azure-lagring.</span><span class="sxs-lookup"><span data-stu-id="4899e-158">If you don't use Azure as a primary backup storage endpoint, then choose **Locally-redundant**, which reduces the Azure storage costs.</span></span> <span data-ttu-id="4899e-159">Läs mer om alternativen för [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) och [lokalt redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) i denna [översikt av lagringsredundans](../storage/common/storage-redundancy.md).</span><span class="sxs-lookup"><span data-stu-id="4899e-159">Read more about [geo-redundant](../storage/common/storage-redundancy.md#geo-redundant-storage) and [locally redundant](../storage/common/storage-redundancy.md#locally-redundant-storage) storage options in this [Storage redundancy overview](../storage/common/storage-redundancy.md).</span></span>

<span data-ttu-id="4899e-160">Nu när du har skapat ett valv konfigurerar du det för säkerhetskopiering av filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="4899e-160">Now that you've created a vault, configure it for backing up files and folders.</span></span>

## <a name="configure-the-vault"></a><span data-ttu-id="4899e-161">Konfigurera valvet</span><span class="sxs-lookup"><span data-stu-id="4899e-161">Configure the vault</span></span>
1. <span data-ttu-id="4899e-162">På bladet Recovery Services-valv (för valvet som du precis har skapat) klickar du på **Säkerhetskopiering** i avsnittet Komma igång och väljer sedan **Säkerhetskopieringsmål** på bladet **Kom igång med säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="4899e-162">On the Recovery Services vault blade (for the vault you just created), in the Getting Started section, click **Backup**, then on the **Getting Started with Backup** blade, select **Backup goal**.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/open-backup-settings.png)

    <span data-ttu-id="4899e-164">Bladet **Säkerhetskopieringsmål** öppnas.</span><span class="sxs-lookup"><span data-stu-id="4899e-164">The **Backup Goal** blade opens.</span></span>

    ![Öppna bladet för säkerhetskopieringsmål](./media/backup-try-azure-backup-in-10-mins/backup-goal-blade.png)

2. <span data-ttu-id="4899e-166">Från listrutan **Var körs din arbetsbelastning?** väljer du **Lokalt**.</span><span class="sxs-lookup"><span data-stu-id="4899e-166">From the **Where is your workload running?** drop-down menu, select **On-premises**.</span></span>

    <span data-ttu-id="4899e-167">Du väljer **Lokalt** eftersom din Windows Server- eller Windows-dator är en fysisk dator som inte finns i Azure.</span><span class="sxs-lookup"><span data-stu-id="4899e-167">You choose **On-premises** because your Windows Server or Windows computer is a physical machine that is not in Azure.</span></span>

3. <span data-ttu-id="4899e-168">Från menyn **Vad vill du säkerhetskopiera?** väljer du **Filer och mappar** och klickar på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4899e-168">From the **What do you want to backup?** menu, select **Files and folders**, and click **OK**.</span></span>

    ![Konfigurera filer och mappar](./media/backup-try-azure-backup-in-10-mins/set-file-folder.png)

    <span data-ttu-id="4899e-170">När du klickar på OK visas en markering bredvid **Säkerhetskopieringsmål** och bladet **Förbered infrastruktur** öppnas.</span><span class="sxs-lookup"><span data-stu-id="4899e-170">After clicking OK, a checkmark appears next to **Backup goal**, and the **Prepare infrastructure** blade opens.</span></span>

    ![När säkerhetskopieringsmålet har konfigurerats går du vidare och förbereder infrastrukturen](./media/backup-try-azure-backup-in-10-mins/backup-goal-configed.png)

4. <span data-ttu-id="4899e-172">På bladet **Förbered infrastruktur** klickar du på **Ladda ned agent för Windows Server eller Windows Client**.</span><span class="sxs-lookup"><span data-stu-id="4899e-172">On the **Prepare infrastructure** blade, click **Download Agent for Windows Server or Windows Client**.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/choose-agent-for-server-client.png)

    <span data-ttu-id="4899e-174">Om du använder Windows Server Essential väljer du att hämta agenten för Windows Server Essential.</span><span class="sxs-lookup"><span data-stu-id="4899e-174">If you are using Windows Server Essential, then choose to download the agent for Windows Server Essential.</span></span> <span data-ttu-id="4899e-175">En popup-meny visas och du uppmanas att köra eller spara MARSAgentInstaller.exe.</span><span class="sxs-lookup"><span data-stu-id="4899e-175">A pop-up menu prompts you to run or save MARSAgentInstaller.exe.</span></span>

    ![Dialogrutan MARSAgentInstaller](./media/backup-try-azure-backup-in-10-mins/mars-installer-run-save.png)

5. <span data-ttu-id="4899e-177">På snabbmenyn för hämtningen klickar du på **Spara**.</span><span class="sxs-lookup"><span data-stu-id="4899e-177">In the download pop-up menu, click **Save**.</span></span>

    <span data-ttu-id="4899e-178">Som standard sparas filen **MARSagentinstaller.exe** i mappen för nedladdningar.</span><span class="sxs-lookup"><span data-stu-id="4899e-178">By default, the **MARSagentinstaller.exe** file is saved to your Downloads folder.</span></span> <span data-ttu-id="4899e-179">När installationsprogrammet har slutförts visas ett popup-fönster och du tillfrågas om du vill köra installationsprogrammet eller öppna mappen.</span><span class="sxs-lookup"><span data-stu-id="4899e-179">When the installer completes, you will see a pop-up asking if you want to run the installer, or open the folder.</span></span>

    ![förbereda infrastrukturen](./media/backup-try-azure-backup-in-10-mins/mars-installer-complete.png)

    <span data-ttu-id="4899e-181">Du behöver inte installera agenten än.</span><span class="sxs-lookup"><span data-stu-id="4899e-181">You don't need to install the agent yet.</span></span> <span data-ttu-id="4899e-182">Du kan installera agenten när du har hämtat valvautentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="4899e-182">You can install the agent after you have downloaded the vault credentials.</span></span>

6. <span data-ttu-id="4899e-183">Klicka på **Ladda ned** på bladet **Förbered infrastruktur**.</span><span class="sxs-lookup"><span data-stu-id="4899e-183">On the **Prepare infrastructure** blade, click **Download**.</span></span>

    ![hämta autentiseringsuppgifter för valvet](./media/backup-try-azure-backup-in-10-mins/download-vault-credentials.png)

    <span data-ttu-id="4899e-185">Autentiseringsuppgifterna för valvet hämtas till mappen Hämtningsbara filer.</span><span class="sxs-lookup"><span data-stu-id="4899e-185">The vault credentials download to your Downloads folder.</span></span> <span data-ttu-id="4899e-186">När autentiseringsuppgifterna för valvet har hämtats visas ett popup-fönster och du tillfrågas om du vill öppna eller spara autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="4899e-186">After the vault credentials finish downloading, you see a pop-up asking if you want to open or save the credentials.</span></span> <span data-ttu-id="4899e-187">Klicka på **Save** (Spara).</span><span class="sxs-lookup"><span data-stu-id="4899e-187">Click **Save**.</span></span> <span data-ttu-id="4899e-188">Om du råkar klicka på **Öppna** av misstag väntar du tills dialogrutan som försöker öppna autentiseringsuppgifterna för valvet misslyckas.</span><span class="sxs-lookup"><span data-stu-id="4899e-188">If you accidentally click **Open**, let the dialog that attempts to open the vault credentials, fail.</span></span> <span data-ttu-id="4899e-189">Du kan inte öppna valvautentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="4899e-189">You cannot open the vault credentials.</span></span> <span data-ttu-id="4899e-190">Gå vidare till nästa steg.</span><span class="sxs-lookup"><span data-stu-id="4899e-190">Proceed to the next step.</span></span> <span data-ttu-id="4899e-191">Valvautentiseringsuppgifterna finns i mappen Hämtade filer.</span><span class="sxs-lookup"><span data-stu-id="4899e-191">The vault credentials are in the Downloads folder.</span></span>   

    ![valvautentiseringsuppgifterna har hämtats](./media/backup-try-azure-backup-in-10-mins/vault-credentials-downloaded.png)

## <a name="install-and-register-the-agent"></a><span data-ttu-id="4899e-193">Installera och registrera agenten</span><span class="sxs-lookup"><span data-stu-id="4899e-193">Install and register the agent</span></span>

> [!NOTE]
> <span data-ttu-id="4899e-194">Aktivering av säkerhetskopiering via Azure Portal är inte tillgängligt ännu.</span><span class="sxs-lookup"><span data-stu-id="4899e-194">Enabling backup through the Azure portal is not available, yet.</span></span> <span data-ttu-id="4899e-195">Använd Microsoft Azure Recovery Services-agenten för att säkerhetskopiera filer och mappar.</span><span class="sxs-lookup"><span data-stu-id="4899e-195">Use the Microsoft Azure Recovery Services Agent to back up your files and folders.</span></span>
>

1. <span data-ttu-id="4899e-196">Leta upp och dubbelklicka på **MARSagentinstaller.exe** från mappen för nedladdade filer (eller en annan lagringsplats).</span><span class="sxs-lookup"><span data-stu-id="4899e-196">Locate and double-click the **MARSagentinstaller.exe** from the Downloads folder (or other saved location).</span></span>

    <span data-ttu-id="4899e-197">Installationsprogrammet visar ett antal meddelanden när Recovery Services-agenten extraheras, installeras och registreras.</span><span class="sxs-lookup"><span data-stu-id="4899e-197">The installer provides a series of messages as it extracts, installs, and registers the Recovery Services agent.</span></span>

    ![Autentiseringsuppgifter för att köra Recovery Services-agentinstallationsprogrammet](./media/backup-try-azure-backup-in-10-mins/mars-installer-registration.png)

2. <span data-ttu-id="4899e-199">Slutför installationsguiden för Microsoft Azure Recovery Services Agent.</span><span class="sxs-lookup"><span data-stu-id="4899e-199">Complete the Microsoft Azure Recovery Services Agent Setup Wizard.</span></span> <span data-ttu-id="4899e-200">För att slutföra guiden måste du:</span><span class="sxs-lookup"><span data-stu-id="4899e-200">To complete the wizard, you need to:</span></span>

   * <span data-ttu-id="4899e-201">Välja en plats för installationen och cachelagringsmappen.</span><span class="sxs-lookup"><span data-stu-id="4899e-201">Choose a location for the installation and cache folder.</span></span>
   * <span data-ttu-id="4899e-202">Ange information om proxyservern om du använder en proxyserver för att ansluta till Internet.</span><span class="sxs-lookup"><span data-stu-id="4899e-202">Provide your proxy server info if you use a proxy server to connect to the internet.</span></span>
   * <span data-ttu-id="4899e-203">Ange ditt användarnamn och lösenord om du använder en autentiserad proxyserver.</span><span class="sxs-lookup"><span data-stu-id="4899e-203">Provide your user name and password details if you use an authenticated proxy.</span></span>
   * <span data-ttu-id="4899e-204">Ange autentiseringsuppgifterna som du hämtat för valvet.</span><span class="sxs-lookup"><span data-stu-id="4899e-204">Provide the downloaded vault credentials</span></span>
   * <span data-ttu-id="4899e-205">Spara krypteringslösenfrasen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="4899e-205">Save the encryption passphrase in a secure location.</span></span>

     > [!NOTE]
     > <span data-ttu-id="4899e-206">Om du tappar bort eller glömmer lösenfrasen kan Microsoft inte hjälpa dig att återställa dina säkerhetskopierade data.</span><span class="sxs-lookup"><span data-stu-id="4899e-206">If you lose or forget the passphrase, Microsoft cannot help recover the backup data.</span></span> <span data-ttu-id="4899e-207">Spara filen på en säker plats.</span><span class="sxs-lookup"><span data-stu-id="4899e-207">Save the file in a secure location.</span></span> <span data-ttu-id="4899e-208">Det krävs för att återställa en säkerhetskopia.</span><span class="sxs-lookup"><span data-stu-id="4899e-208">It is required to restore a backup.</span></span>
     >
     >

<span data-ttu-id="4899e-209">Nu installeras agenten och datorn registreras i valvet.</span><span class="sxs-lookup"><span data-stu-id="4899e-209">The agent is now installed and your machine is registered to the vault.</span></span> <span data-ttu-id="4899e-210">Nu kan du konfigurera och schemalägga säkerhetskopieringen.</span><span class="sxs-lookup"><span data-stu-id="4899e-210">You're ready to configure and schedule your backup.</span></span>

## <a name="network-and-connectivity-requirements"></a><span data-ttu-id="4899e-211">Nätverks- och anslutningskrav</span><span class="sxs-lookup"><span data-stu-id="4899e-211">Network and Connectivity Requirements</span></span>

<span data-ttu-id="4899e-212">Om din dator/proxy har begränsad tillgång till Internet måste du se till att brandväggsinställningarna på mcahine/proxy har konfigurerats för att tillåta följande URL:er:</span><span class="sxs-lookup"><span data-stu-id="4899e-212">If your machine/proxy has limited internet access, ensure that firewall settings on the mcahine/proxy are configured to allow the following URLs:</span></span> <br>
    1. <span data-ttu-id="4899e-213">www.msftncsi.com</span><span class="sxs-lookup"><span data-stu-id="4899e-213">www.msftncsi.com</span></span>
    2. <span data-ttu-id="4899e-214">*.Microsoft.com</span><span class="sxs-lookup"><span data-stu-id="4899e-214">*.Microsoft.com</span></span>
    3. <span data-ttu-id="4899e-215">*.WindowsAzure.com</span><span class="sxs-lookup"><span data-stu-id="4899e-215">*.WindowsAzure.com</span></span>
    4. <span data-ttu-id="4899e-216">*.microsoftonline.com</span><span class="sxs-lookup"><span data-stu-id="4899e-216">*.microsoftonline.com</span></span>
    5. <span data-ttu-id="4899e-217">*.windows.ne</span><span class="sxs-lookup"><span data-stu-id="4899e-217">*.windows.ne</span></span>

## <a name="back-up-your-files-and-folders"></a><span data-ttu-id="4899e-218">Säkerhetskopiera dina filer och mappar</span><span class="sxs-lookup"><span data-stu-id="4899e-218">Back up your files and folders</span></span>
<span data-ttu-id="4899e-219">Den första säkerhetskopieringen omfattar två viktiga uppgifter:</span><span class="sxs-lookup"><span data-stu-id="4899e-219">The initial backup includes two key tasks:</span></span>

* <span data-ttu-id="4899e-220">Schemalägg säkerhetskopieringen</span><span class="sxs-lookup"><span data-stu-id="4899e-220">Schedule the backup</span></span>
* <span data-ttu-id="4899e-221">Säkerhetskopiera filer och mappar för första gången</span><span class="sxs-lookup"><span data-stu-id="4899e-221">Back up files and folders for the first time</span></span>

<span data-ttu-id="4899e-222">För att slutföra den första säkerhetskopieringen använder du Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="4899e-222">To complete the initial backup, use the Microsoft Azure Recovery Services agent.</span></span>

### <a name="to-schedule-the-backup-job"></a><span data-ttu-id="4899e-223">Så här schemalägger du säkerhetskopieringsjobbet</span><span class="sxs-lookup"><span data-stu-id="4899e-223">To schedule the backup job</span></span>
1. <span data-ttu-id="4899e-224">Öppna Microsoft Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="4899e-224">Open the Microsoft Azure Recovery Services agent.</span></span> <span data-ttu-id="4899e-225">Du hittar den genom att söka efter **Microsoft Azure Backup** på datorn.</span><span class="sxs-lookup"><span data-stu-id="4899e-225">You can find it by searching your machine for **Microsoft Azure Backup**.</span></span>

    ![Starta Azure Recovery Services-agenten](./media/backup-try-azure-backup-in-10-mins/snap-in-search.png)
2. <span data-ttu-id="4899e-227">Klicka på Recovery Services-agenten och sedan på **Schemalägg säkerhetskopiering**.</span><span class="sxs-lookup"><span data-stu-id="4899e-227">In the Recovery Services agent, click **Schedule Backup**.</span></span>

    ![Schemalägga en Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/schedule-first-backup.png)
3. <span data-ttu-id="4899e-229">Klicka på **Nästa** på sidan Komma igång i guiden Schemalägg säkerhetskopiering.</span><span class="sxs-lookup"><span data-stu-id="4899e-229">On the Getting started page of the Schedule Backup Wizard, click **Next**.</span></span>
4. <span data-ttu-id="4899e-230">På sidan Välj objekt som ska säkerhetskopieras klickar du på **Lägg till objekt**.</span><span class="sxs-lookup"><span data-stu-id="4899e-230">On the Select Items to Backup page, click **Add Items**.</span></span>
5. <span data-ttu-id="4899e-231">Markera de filer och mappar som du vill säkerhetskopiera och klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="4899e-231">Select the files and folders that you want to back up, and then click **Okay**.</span></span>
6. <span data-ttu-id="4899e-232">Klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="4899e-232">Click **Next**.</span></span>
7. <span data-ttu-id="4899e-233">På sidan **Ange schema för säkerhetskopiering** anger du **säkerhetskopieringsschemat** och klickar på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="4899e-233">On the **Specify Backup Schedule** page, specify the **backup schedule** and click **Next**.</span></span>

    <span data-ttu-id="4899e-234">Du kan schemalägga säkerhetskopieringar varje dag (högst tre gånger om dagen) eller varje vecka.</span><span class="sxs-lookup"><span data-stu-id="4899e-234">You can schedule daily (at a maximum rate of three times per day) or weekly backups.</span></span>

    ![Objekt för Windows Server-säkerhetskopiering](./media/backup-try-azure-backup-in-10-mins/specify-backup-schedule-close.png)

   > [!NOTE]
   > <span data-ttu-id="4899e-236">Mer information om hur du anger säkerhetskopieringsschemat finns i artikeln [Använda Azure Backup för att ersätta en bandbaserad infrastruktur](backup-azure-backup-cloud-as-tape.md).</span><span class="sxs-lookup"><span data-stu-id="4899e-236">For more information about how to specify the backup schedule, see the article [Use Azure Backup to replace your tape infrastructure](backup-azure-backup-cloud-as-tape.md).</span></span>
   >

8. <span data-ttu-id="4899e-237">På sidan **Välj bevarandeprincip** väljer du **bevarandeprincipen** för säkerhetskopian.</span><span class="sxs-lookup"><span data-stu-id="4899e-237">On the **Select Retention Policy** page, select the **Retention Policy** for the backup copy.</span></span>

    <span data-ttu-id="4899e-238">Bevarandeprincipen anger hur länge säkerhetskopierade data lagras.</span><span class="sxs-lookup"><span data-stu-id="4899e-238">The retention policy specifies how long the backup data is stored.</span></span> <span data-ttu-id="4899e-239">I stället för att bara ange en ”platt princip” för alla säkerhetskopieringspunkter kan du ange olika bevarandeprinciper baserat på när säkerhetskopieringen utförs.</span><span class="sxs-lookup"><span data-stu-id="4899e-239">Rather than specifying a “flat policy” for all backup points, you can specify different retention policies based on when the backup occurs.</span></span> <span data-ttu-id="4899e-240">Du kan ändra de dagliga, veckovisa, månatliga och årliga bevarandeprinciperna för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="4899e-240">You can modify the daily, weekly, monthly, and yearly retention policies to meet your needs.</span></span>
9. <span data-ttu-id="4899e-241">Välj den första säkerhetskopieringstypen på sidan Välj inledande säkerhetskopieringstyp.</span><span class="sxs-lookup"><span data-stu-id="4899e-241">On the Choose Initial Backup Type page, choose the initial backup type.</span></span> <span data-ttu-id="4899e-242">Lämna alternativet **Automatiskt över nätverket** markerat och klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="4899e-242">Leave the option **Automatically over the network** selected, and then click **Next**.</span></span>

    <span data-ttu-id="4899e-243">Du kan säkerhetskopiera automatiskt över nätverket, eller så kan du säkerhetskopiera offline.</span><span class="sxs-lookup"><span data-stu-id="4899e-243">You can back up automatically over the network, or you can back up offline.</span></span> <span data-ttu-id="4899e-244">Resten av den här artikeln beskriver hur du säkerhetskopierar automatiskt.</span><span class="sxs-lookup"><span data-stu-id="4899e-244">The remainder of this article describes the process for backing up automatically.</span></span> <span data-ttu-id="4899e-245">Om du föredrar att säkerhetskopiera offline läser du artikeln [Arbetsflöde för säkerhetskopiering offline i Azure Backup](backup-azure-backup-import-export.md) för ytterligare information.</span><span class="sxs-lookup"><span data-stu-id="4899e-245">If you prefer to do an offline backup, review the article [Offline backup workflow in Azure Backup](backup-azure-backup-import-export.md) for additional information.</span></span>
10. <span data-ttu-id="4899e-246">Läs informationen på sidan Bekräftelse och klicka sedan på **Slutför**.</span><span class="sxs-lookup"><span data-stu-id="4899e-246">On the Confirmation page, review the information, and then click **Finish**.</span></span>
11. <span data-ttu-id="4899e-247">När guiden har skapat säkerhetskopieringsschemat klickar du på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="4899e-247">After the wizard finishes creating the backup schedule, click **Close**.</span></span>

### <a name="to-back-up-files-and-folders-for-the-first-time"></a><span data-ttu-id="4899e-248">Så här säkerhetskopierar du filer och mappar för första gången</span><span class="sxs-lookup"><span data-stu-id="4899e-248">To back up files and folders for the first time</span></span>
1. <span data-ttu-id="4899e-249">I Recovery Services-agenten klickar du på **Säkerhetskopiera nu** för att slutföra en inledande seeding över nätverket.</span><span class="sxs-lookup"><span data-stu-id="4899e-249">In the Recovery Services agent, click **Back Up Now** to complete the initial seeding over the network.</span></span>

    ![Säkerhetskopiera Windows Server nu](./media/backup-try-azure-backup-in-10-mins/backup-now.png)
2. <span data-ttu-id="4899e-251">Gå igenom inställningarna på sidan Bekräftelse som guiden Säkerhetskopiera nu ska använda för att säkerhetskopiera datorn.</span><span class="sxs-lookup"><span data-stu-id="4899e-251">On the Confirmation page, review the settings that the Back Up Now Wizard will use to back up the machine.</span></span> <span data-ttu-id="4899e-252">Klicka på **Säkerhetskopiera**.</span><span class="sxs-lookup"><span data-stu-id="4899e-252">Then click **Back Up**.</span></span>
3. <span data-ttu-id="4899e-253">Stäng guiden genom att klicka på **Stäng**.</span><span class="sxs-lookup"><span data-stu-id="4899e-253">Click **Close** to close the wizard.</span></span> <span data-ttu-id="4899e-254">Om du stänger guiden innan säkerhetskopieringen är klar fortsätter guiden att köras i bakgrunden.</span><span class="sxs-lookup"><span data-stu-id="4899e-254">If you close the wizard before the backup process finishes, the wizard continues to run in the background.</span></span>

<span data-ttu-id="4899e-255">När den första säkerhetskopieringen har slutförts visas statusen **Jobbet har slutförts** i säkerhetskopieringskonsolen.</span><span class="sxs-lookup"><span data-stu-id="4899e-255">After the initial backup is completed, the **Job completed** status appears in the Backup console.</span></span>

![IR slutfört](./media/backup-try-azure-backup-in-10-mins/ircomplete.png)

## <a name="questions"></a><span data-ttu-id="4899e-257">Har du några frågor?</span><span class="sxs-lookup"><span data-stu-id="4899e-257">Questions?</span></span>
<span data-ttu-id="4899e-258">Om du har frågor eller om du saknar en funktion är du välkommen att [lämna feedback](http://aka.ms/azurebackup_feedback).</span><span class="sxs-lookup"><span data-stu-id="4899e-258">If you have questions, or if there is any feature that you would like to see included, [send us feedback](http://aka.ms/azurebackup_feedback).</span></span>

## <a name="next-steps"></a><span data-ttu-id="4899e-259">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4899e-259">Next steps</span></span>
* <span data-ttu-id="4899e-260">Få mer information om hur du [säkerhetskopierar Windows-datorer](backup-configure-vault.md).</span><span class="sxs-lookup"><span data-stu-id="4899e-260">Get more details about [backing up Windows machines](backup-configure-vault.md).</span></span>
* <span data-ttu-id="4899e-261">Nu när du har säkerhetskopierat dina filer och mappar kan du [hantera dina valv och servrar](backup-azure-manage-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="4899e-261">Now that you've backed up your files and folders, you can [manage your vaults and servers](backup-azure-manage-windows-server.md).</span></span>
* <span data-ttu-id="4899e-262">Om du behöver återställa en säkerhetskopia använder du den här artikeln för att [återställa filer till en Windows-dator](backup-azure-restore-windows-server.md).</span><span class="sxs-lookup"><span data-stu-id="4899e-262">If you need to restore a backup, use this article to [restore files to a Windows machine](backup-azure-restore-windows-server.md).</span></span>
