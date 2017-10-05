---
title: "Konfigurera källan och målet för Hyper-V-replikering till Azure (med System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hur du gör inställningar för källa och mål för replikering av Hyper-V virtuella datorer i VMM-moln till Azure-lagring med Azure Site Recovery"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: 5edb6d87-25a5-40fe-b6f1-ddf7b55a6b31
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 07/25/2017
ms.author: raynew
ms.openlocfilehash: c72f839d0a1288dccb7deb3e44fc2b20d64818f0
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="step-8-set-up-the-source-and-target-for-hyper-v-with-vmm-replication-to-azure"></a><span data-ttu-id="1ff04-103">Steg 8: Ställ in på källan och målet för Hyper-V (med VMM) replikering till Azure</span><span class="sxs-lookup"><span data-stu-id="1ff04-103">Step 8: Set up the source and target for Hyper-V (with VMM) replication to Azure</span></span>

<span data-ttu-id="1ff04-104">Efter [skapar ett valv](vmm-to-azure-walkthrough-create-vault.md) och att ange vad du vill replikera, Använd den här artikeln för att konfigurera inställningar för källan och målet vid replikering av lokala Hyper-V virtuella datorer i System Center Virtual Machine Manager (VMM)-moln till Azure med hjälp av den [Azure Site Recovery](site-recovery-overview.md) tjänsten i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-104">After [creating a vault](vmm-to-azure-walkthrough-create-vault.md) and specifying what you want to replicate, use this article to configure source and target settings when replicating on-premises Hyper-V virtual machines in System Center Virtual Machine Manager (VMM) clouds to Azure, using the [Azure Site Recovery](site-recovery-overview.md) service in the Azure portal.</span></span>

<span data-ttu-id="1ff04-105">Publicera kommentarer och frågor längst ned i den här artikeln eller i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1ff04-105">Post comments and questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-the-source-environment"></a><span data-ttu-id="1ff04-106">Konfigurera källmiljön</span><span class="sxs-lookup"><span data-stu-id="1ff04-106">Set up the source environment</span></span>

<span data-ttu-id="1ff04-107">S1.</span><span class="sxs-lookup"><span data-stu-id="1ff04-107">S1.</span></span> <span data-ttu-id="1ff04-108">Klicka på **Förbereda infrastrukturen** > **Källa**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-108">Click **Prepare Infrastructure** > **Source**.</span></span>

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. <span data-ttu-id="1ff04-109">I **Förbered källa** klickar du på **+ VMM** för att lägga till en VMM-server.</span><span class="sxs-lookup"><span data-stu-id="1ff04-109">In **Prepare source**, click **+ VMM** to add a VMM server.</span></span>

    ![Konfigurera källan](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="1ff04-111">På **Lägg till server** kontrollerar du att **System Center VMM-server** visas i **Servertyp** och att VMM-servern uppfyller [de allmänna kraven och URL-kraven](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="1ff04-111">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that the VMM server meets the [prerequisites and URL requirements](#prerequisites).</span></span>
4. <span data-ttu-id="1ff04-112">Ladda ned installationsfilen för Azure Site Recovery-providern.</span><span class="sxs-lookup"><span data-stu-id="1ff04-112">Download the Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="1ff04-113">Ladda ned registreringsnyckeln.</span><span class="sxs-lookup"><span data-stu-id="1ff04-113">Download the registration key.</span></span> <span data-ttu-id="1ff04-114">Du behöver den när du kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-114">You need this when you run setup.</span></span> <span data-ttu-id="1ff04-115">Nyckeln är giltig i fem dagar efter att du har genererat den.</span><span class="sxs-lookup"><span data-stu-id="1ff04-115">The key is valid for five days after you generate it.</span></span>

    ![Konfigurera källan](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-the-provider-on-the-vmm-server"></a><span data-ttu-id="1ff04-117">Installera providern på VMM-servern</span><span class="sxs-lookup"><span data-stu-id="1ff04-117">Install the Provider on the VMM server</span></span>

1. <span data-ttu-id="1ff04-118">Kör installationsfilen på VMM-servern för providern.</span><span class="sxs-lookup"><span data-stu-id="1ff04-118">Run the Provider setup file on the VMM server.</span></span>
2. <span data-ttu-id="1ff04-119">I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.</span><span class="sxs-lookup"><span data-stu-id="1ff04-119">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="1ff04-120">I **Installation** accepterar du eller ändrar standardinstallationsplatsen för providern och klickar på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-120">In **Installation**, accept or modify the default Provider installation location and click **Install**.</span></span>

    ![Installationsplats](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. <span data-ttu-id="1ff04-122">När installationen är klar klickar du på **Registrera** för att registrera VMM-servern i valvet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-122">When installation finishes, click **Register** to register the VMM server in the vault.</span></span>
5. <span data-ttu-id="1ff04-123">På sidan **Valvinställningar** klickar du på **Bläddra** och väljer valvnyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-123">In the **Vault Settings** page, click **Browse** to select the vault key file.</span></span> <span data-ttu-id="1ff04-124">Ange Azure Site Recovery-prenumerationen och valvnamnet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-124">Specify the Azure Site Recovery subscription and the vault name.</span></span>

    ![Serverregistrering](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. <span data-ttu-id="1ff04-126">I **Internetanslutning** anger du hur providern som körs på VMM-servern ska ansluta till Site Recovery via internet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-126">In **Internet Connection**, specify how the Provider running on the VMM server will connect to Site Recovery over the internet.</span></span>

   * <span data-ttu-id="1ff04-127">Om du vill att providern ska ansluta direkt väljer du **Anslut direkt till Azure Site Recovery utan proxyserver**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-127">If you want the Provider to connect directly, select **Connect directly to Azure Site Recovery without a proxy**.</span></span>
   * <span data-ttu-id="1ff04-128">Om din befintliga proxyserver kräver autentisering, eller om du vill använda en anpassad proxyserver, väljer du **Anslut till Azure Site Recovery med proxyserver**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-128">If your existing proxy requires authentication, or you want to use a custom proxy, select **Connect to Azure Site Recovery using a proxy server**.</span></span>
   * <span data-ttu-id="1ff04-129">Om du använder en anpassad proxyserver anger du adressen, porten och autentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="1ff04-129">If you use a custom proxy, specify the address, port, and credentials.</span></span>
   * <span data-ttu-id="1ff04-130">Om du använder en proxyserver bör du redan ha tillåtit URL:erna som beskrivs i [krav](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="1ff04-130">If you're using a proxy, you should have already allowed the URLs described in [prerequisites](#on-premises-prerequisites).</span></span>
   * <span data-ttu-id="1ff04-131">Om du använder en anpassad proxyserver skapas ett RunAs-konto (DRAProxyAccount) i VMM automatiskt med de angivna proxyautentiseringsuppgifterna.</span><span class="sxs-lookup"><span data-stu-id="1ff04-131">If you use a custom proxy, a VMM RunAs account (DRAProxyAccount) will be created automatically using the specified proxy credentials.</span></span> <span data-ttu-id="1ff04-132">Konfigurera proxyservern så att det här kontot kan autentiseras.</span><span class="sxs-lookup"><span data-stu-id="1ff04-132">Configure the proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="1ff04-133">Du kan ändra inställningarna för RunAs-kontot i VMM i VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-133">The VMM RunAs account settings can be modified in the VMM console.</span></span> <span data-ttu-id="1ff04-134">I **Inställningar** expanderar du **Säkerhet** > **Kör som-konton** och ändrar sedan lösenordet för DRAProxyAccount.</span><span class="sxs-lookup"><span data-stu-id="1ff04-134">In **Settings**, expand **Security** > **Run As Accounts**, and then modify the password for DRAProxyAccount.</span></span> <span data-ttu-id="1ff04-135">Du måste starta om VMM-tjänsten så att den här inställningen börjar gälla.</span><span class="sxs-lookup"><span data-stu-id="1ff04-135">You’ll need to restart the VMM service so that this setting takes effect.</span></span>

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. <span data-ttu-id="1ff04-137">Acceptera eller ändra platsen för ett SSL-certifikat som genereras automatiskt för datakryptering.</span><span class="sxs-lookup"><span data-stu-id="1ff04-137">Accept or modify the location of an SSL certificate that’s automatically generated for data encryption.</span></span> <span data-ttu-id="1ff04-138">Det här certifikatet används om du aktiverar datakryptering för ett moln som skyddas av Azure på Azure Site Recovery-portalen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-138">This certificate is used if you enable data encryption for a cloud protected by Azure in the Azure Site Recovery portal.</span></span> <span data-ttu-id="1ff04-139">Skydda det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-139">Keep this certificate safe.</span></span> <span data-ttu-id="1ff04-140">När du kör en redundansväxling till Azure måste den dekrypteras om datakryptering är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="1ff04-140">When you run a failover to Azure you’ll need it to decrypt, if data encryption is enabled.</span></span>
8. <span data-ttu-id="1ff04-141">I **Servernamn** anger du ett eget namn som identifierar VMM-servern i valvet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-141">In **Server name**, specify a friendly name to identify the VMM server in the vault.</span></span> <span data-ttu-id="1ff04-142">I en klusterkonfiguration anger du namnet på VMM-klusterrollen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-142">In a cluster configuration, specify the VMM cluster role name.</span></span>
9. <span data-ttu-id="1ff04-143">Aktivera **Synkronisera molnmetadata** om du vill synkronisera metadata för alla moln på VMM-servern med valvet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-143">Enable **Sync cloud metadata**, if you want to synchronize metadata for all clouds on the VMM server with the vault.</span></span> <span data-ttu-id="1ff04-144">Den här åtgärden behöver bara göras en gång på varje server.</span><span class="sxs-lookup"><span data-stu-id="1ff04-144">This action only needs to happen once on each server.</span></span> <span data-ttu-id="1ff04-145">Om du inte vill synkronisera alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i molnegenskaperna i VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-145">If you don't want to synchronize all clouds, you can leave this setting unchecked and synchronize each cloud individually in the cloud properties in the VMM console.</span></span> <span data-ttu-id="1ff04-146">Slutför processen genom att klicka på **Registrera**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-146">Click **Register** to complete the process.</span></span>

    ![Serverregistrering](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. <span data-ttu-id="1ff04-148">Registreringen startar.</span><span class="sxs-lookup"><span data-stu-id="1ff04-148">Registration starts.</span></span> <span data-ttu-id="1ff04-149">När registreringen är klar visas servern på bladet **Site Recovery-infrastruktur** > **VMM-servrar**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-149">After registration finishes, the server is displayed in **Site Recovery Infrastructure** > **VMM Servers**.</span></span>


## <a name="install-the-azure-recovery-services-agent-on-hyper-v-hosts"></a><span data-ttu-id="1ff04-150">Installera Azure Recovery Services-agenten på Hyper-V-värdar</span><span class="sxs-lookup"><span data-stu-id="1ff04-150">Install the Azure Recovery Services agent on Hyper-V hosts</span></span>

1. <span data-ttu-id="1ff04-151">När du har konfigurerat providern måste du hämta installationsfilen för Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="1ff04-151">After you've set up the Provider, you need to download the installation file for the Azure Recovery Services agent.</span></span> <span data-ttu-id="1ff04-152">Kör installationsprogrammet på varje Hyper-V-server i VMM-molnet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-152">Run setup on each Hyper-V server in the VMM cloud.</span></span>

    ![Hyper-V-platser](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. <span data-ttu-id="1ff04-154">I **Kravkontroll**, klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-154">In **Prerequisites Check**, click **Next**.</span></span> <span data-ttu-id="1ff04-155">Alla nödvändiga komponenter som saknas installeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="1ff04-155">Any missing prerequisites will be automatically installed.</span></span>

    ![Krav för Recovery Services-agenten](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. <span data-ttu-id="1ff04-157">Godkänn eller ändra installationsplatsen och cachelagringsplatsen på **Installationsinställningar**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-157">In **Installation Settings**, accept or modify the installation location, and the cache location.</span></span> <span data-ttu-id="1ff04-158">Du kan konfigurera cachen på en enhet som har minst 5 GB tillgängligt utrymme, men vi rekommenderar en cacheenhet med 600 GB eller mer ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="1ff04-158">You can configure the cache on a drive that has at least 5 GB of storage available but we recommend a cache drive with 600 GB or more of free space.</span></span> <span data-ttu-id="1ff04-159">Klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-159">Then click **Install**.</span></span>
4. <span data-ttu-id="1ff04-160">När installationen är klar klickar du på **Stäng** för att slutföra.</span><span class="sxs-lookup"><span data-stu-id="1ff04-160">After installation is complete, click **Close** to finish.</span></span>

    ![Registrera MARS-agenten](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a><span data-ttu-id="1ff04-162">Kommandoradsinstallation</span><span class="sxs-lookup"><span data-stu-id="1ff04-162">Command line installation</span></span>
<span data-ttu-id="1ff04-163">Du kan installera Microsoft Azure Recovery Services-agenten från kommandoraden med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="1ff04-163">You can install the Microsoft Azure Recovery Services Agent from command line using the following command:</span></span>

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-to-site-recovery-from-hyper-v-hosts"></a><span data-ttu-id="1ff04-164">Konfigurera Internetåtkomst via en proxyserver till Site Recovery från Hyper-V-värdar</span><span class="sxs-lookup"><span data-stu-id="1ff04-164">Set up internet proxy access to Site Recovery from Hyper-V hosts</span></span>

<span data-ttu-id="1ff04-165">Recovery Services-agenten som körs på Hyper-V-värdar behöver Internetåtkomst till Azure för VM-replikering.</span><span class="sxs-lookup"><span data-stu-id="1ff04-165">The Recovery Services agent running on Hyper-V hosts needs internet access to Azure for VM replication.</span></span> <span data-ttu-id="1ff04-166">Om du ansluter till Internet via en proxyserver konfigurerar du den så här:</span><span class="sxs-lookup"><span data-stu-id="1ff04-166">If you're accessing the internet through a proxy, set it up as follows:</span></span>

1. <span data-ttu-id="1ff04-167">Öppna snapin-modulen Microsoft Azure Backup MMC på Hyper-V-värden.</span><span class="sxs-lookup"><span data-stu-id="1ff04-167">Open the Microsoft Azure Backup MMC snap-in on the Hyper-V host.</span></span> <span data-ttu-id="1ff04-168">Som standard finns det en genväg till Microsoft Azure Backup på skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="1ff04-168">By default, a shortcut for Microsoft Azure Backup is available on the desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="1ff04-169">Klicka på **Ändra egenskaper** i snapin-modulen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-169">In the snap-in, click **Change Properties**.</span></span>
3. <span data-ttu-id="1ff04-170">Ange information om proxyservern på fliken **Proxykonfiguration**.</span><span class="sxs-lookup"><span data-stu-id="1ff04-170">On the **Proxy Configuration** tab, specify proxy server information.</span></span>

    ![Registrera MARS-agenten](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. <span data-ttu-id="1ff04-172">Kontrollera att agenten kan nå URL:erna som beskrivs i [kravavsnittet](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="1ff04-172">Check that the agent can reach the URLs described in the [prerequisites](#on-premises-prerequisites).</span></span>

## <a name="set-up-the-target-environment"></a><span data-ttu-id="1ff04-173">Konfigurera målmiljön</span><span class="sxs-lookup"><span data-stu-id="1ff04-173">Set up the target environment</span></span>
<span data-ttu-id="1ff04-174">Ange Azure-lagringskontot som ska användas för replikering och det Azure-nätverk som virtuella Azure-datorer ska ansluta till efter en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="1ff04-174">Specify the Azure storage account to be used for replication, and the Azure network to which Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="1ff04-175">Klicka på **Förbered infrastruktur** > **Mål** och välj den prenumeration och resursgrupp där du vill skapa de redundansväxlade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="1ff04-175">Click **Prepare infrastructure** > **Target**, select the subscription and the resource group where you want to create the failed over virtual machines.</span></span> <span data-ttu-id="1ff04-176">Välj den distributionsmodell som du vill använda i Azure (klassisk eller Resource Manager) för de redundansväxlade virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="1ff04-176">Choose the deployment model that you want to use in Azure (classic or resource management) for the failed over virtual machines.</span></span>

    ![Lagring](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. <span data-ttu-id="1ff04-178">Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="1ff04-178">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    ![Lagring](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. <span data-ttu-id="1ff04-180">Om du inte har skapat ett lagringskonto och vill skapa ett med hjälp av Resource Manager klickar du på **+Lagringskonto** för att göra det direkt.</span><span class="sxs-lookup"><span data-stu-id="1ff04-180">If you haven't created a storage account, and you want to create one using Resource Manager, click **+Storage account** to do that inline.</span></span>  <span data-ttu-id="1ff04-181">På bladet **Skapa lagringskonto** anger du kontonamn, typ, prenumeration och plats.</span><span class="sxs-lookup"><span data-stu-id="1ff04-181">On the **Create storage account** blade specify an account name, type, subscription, and location.</span></span> <span data-ttu-id="1ff04-182">Kontot måste finnas på samma plats som Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-182">The account should be in the same location as the Recovery Services vault.</span></span>

   ![Lagring](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * <span data-ttu-id="1ff04-184">Om du vill skapa ett lagringskonto med hjälp av den klassiska modellen gör du det på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-184">If you want to create a storage account using the classic model, do that in the Azure portal.</span></span> [<span data-ttu-id="1ff04-185">Läs mer</span><span class="sxs-lookup"><span data-stu-id="1ff04-185">Learn more</span></span>](../storage/common/storage-create-storage-account.md)
   * <span data-ttu-id="1ff04-186">Om du använder ett Premium Storage-konto för replikerade data konfigurerar du ytterligare ett standardlagringskonto för att lagra replikeringsloggar som samlar in löpande ändringar i lokala data.</span><span class="sxs-lookup"><span data-stu-id="1ff04-186">If you’re using a premium storage account for replicated data, set up an additional standard storage account, to store replication logs that capture ongoing changes to on-premises data.</span></span>
5. <span data-ttu-id="1ff04-187">Om du inte har skapat ett Azure-nätverk och vill skapa ett med hjälp av Resource Manager klickar du på **+Nätverk** för att göra det direkt.</span><span class="sxs-lookup"><span data-stu-id="1ff04-187">If you haven't created an Azure network, and you want to create one using Resource Manager, click **+Network** to do that inline.</span></span> <span data-ttu-id="1ff04-188">På bladet **Skapa virtuellt nätverk** anger du nätverksnamn, adressintervall, information om undernät, prenumeration och plats.</span><span class="sxs-lookup"><span data-stu-id="1ff04-188">On the **Create virtual network** blade specify a network name, address range, subnet details, subscription, and location.</span></span> <span data-ttu-id="1ff04-189">Nätverket måste finnas på samma plats som Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="1ff04-189">The network should be in the same location as the Recovery Services vault.</span></span>

   ![Nätverk](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   <span data-ttu-id="1ff04-191">Om du vill skapa ett nätverk med den klassiska modellen gör du det på Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="1ff04-191">If you want to create a network using the classic model, do that in the Azure portal.</span></span> <span data-ttu-id="1ff04-192">[Läs mer](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="1ff04-192">[Learn more](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span></span>





## <a name="next-steps"></a><span data-ttu-id="1ff04-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1ff04-193">Next steps</span></span>

<span data-ttu-id="1ff04-194">Gå till [steg 9: Konfigurera nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="1ff04-194">Go to [Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>
