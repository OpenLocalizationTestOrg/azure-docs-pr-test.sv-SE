---
title: "aaaSet hello källa och mål för Hyper-V-replikering tooAzure (med System Center VMM) med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello steg tooset inställningar för källa och mål för replikering av Hyper-V virtuella datorer i VMM-moln tooAzure lagring med Azure Site Recovery"
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
ms.openlocfilehash: 3f8c5386cb64527c775aef636980bac098ee9905
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-8-set-up-hello-source-and-target-for-hyper-v-with-vmm-replication-tooazure"></a><span data-ttu-id="644d9-103">Steg 8: Ställ in hello källa och mål för tooAzure för replikering av Hyper-V (med VMM)</span><span class="sxs-lookup"><span data-stu-id="644d9-103">Step 8: Set up hello source and target for Hyper-V (with VMM) replication tooAzure</span></span>

<span data-ttu-id="644d9-104">Efter [skapar ett valv](vmm-to-azure-walkthrough-create-vault.md) och ange vad du vill tooreplicate, använder den här artikeln tooconfigure källa och mål inställningar vid replikering av lokala Hyper-V virtuella datorer i System Center Virtual Machine Manager (VMM) moln tooAzure, med hjälp av hello [Azure Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="644d9-104">After [creating a vault](vmm-to-azure-walkthrough-create-vault.md) and specifying what you want tooreplicate, use this article tooconfigure source and target settings when replicating on-premises Hyper-V virtual machines in System Center Virtual Machine Manager (VMM) clouds tooAzure, using hello [Azure Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="644d9-105">Publicera kommentarer och frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="644d9-105">Post comments and questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>


## <a name="set-up-hello-source-environment"></a><span data-ttu-id="644d9-106">Ställ in hello källmiljön</span><span class="sxs-lookup"><span data-stu-id="644d9-106">Set up hello source environment</span></span>

<span data-ttu-id="644d9-107">S1.</span><span class="sxs-lookup"><span data-stu-id="644d9-107">S1.</span></span> <span data-ttu-id="644d9-108">Klicka på **Förbereda infrastrukturen** > **Källa**.</span><span class="sxs-lookup"><span data-stu-id="644d9-108">Click **Prepare Infrastructure** > **Source**.</span></span>

    ![Set up source](./media/vmm-to-azure-walkthrough-source-target/set-source1.png)

2. <span data-ttu-id="644d9-109">I **Förbered källa**, klickar du på **+ VMM** tooadd en VMM-server.</span><span class="sxs-lookup"><span data-stu-id="644d9-109">In **Prepare source**, click **+ VMM** tooadd a VMM server.</span></span>

    ![Konfigurera källan](./media/vmm-to-azure-walkthrough-source-target/set-source2.png)

3. <span data-ttu-id="644d9-111">I **Lägg till Server**, kontrollera att **System Center VMM-servern** visas i **servertyp** och den hello VMM-servern uppfyller hello [krav och URL: en krav för](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="644d9-111">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that hello VMM server meets hello [prerequisites and URL requirements](#prerequisites).</span></span>
4. <span data-ttu-id="644d9-112">Hämta installationsfilen för hello Azure Site Recovery-providern.</span><span class="sxs-lookup"><span data-stu-id="644d9-112">Download hello Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="644d9-113">Hämta hello registreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="644d9-113">Download hello registration key.</span></span> <span data-ttu-id="644d9-114">Du behöver den när du kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="644d9-114">You need this when you run setup.</span></span> <span data-ttu-id="644d9-115">hello nyckeln är giltig i fem dagar efter genereringen.</span><span class="sxs-lookup"><span data-stu-id="644d9-115">hello key is valid for five days after you generate it.</span></span>

    ![Konfigurera källan](./media/vmm-to-azure-walkthrough-source-target/set-source3.png)

## <a name="install-hello-provider-on-hello-vmm-server"></a><span data-ttu-id="644d9-117">Installera hello providern på hello VMM-servern</span><span class="sxs-lookup"><span data-stu-id="644d9-117">Install hello Provider on hello VMM server</span></span>

1. <span data-ttu-id="644d9-118">Kör installationsfilen för hello-providern på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="644d9-118">Run hello Provider setup file on hello VMM server.</span></span>
2. <span data-ttu-id="644d9-119">I **Microsoft Update** kan du välja uppdateringar så att provideruppdateringarna installeras i enlighet med din Microsoft Update-princip.</span><span class="sxs-lookup"><span data-stu-id="644d9-119">In **Microsoft Update**, you can opt in for updates so that Provider updates are installed in accordance with your Microsoft Update policy.</span></span>
3. <span data-ttu-id="644d9-120">I **Installation**, acceptera eller ändra hello standardinstallationsplatsen för providern och klickar på **installera**.</span><span class="sxs-lookup"><span data-stu-id="644d9-120">In **Installation**, accept or modify hello default Provider installation location and click **Install**.</span></span>

    ![Installationsplats](./media/vmm-to-azure-walkthrough-source-target/provider2.png)
4. <span data-ttu-id="644d9-122">När installationen är klar klickar du på **registrera** tooregister hello VMM-servern i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="644d9-122">When installation finishes, click **Register** tooregister hello VMM server in hello vault.</span></span>
5. <span data-ttu-id="644d9-123">I hello **Valvinställningar** klickar du på **Bläddra** tooselect hello valvnyckelfilen.</span><span class="sxs-lookup"><span data-stu-id="644d9-123">In hello **Vault Settings** page, click **Browse** tooselect hello vault key file.</span></span> <span data-ttu-id="644d9-124">Ange hello Azure Site Recovery-prenumerationen och valvnamnet hello.</span><span class="sxs-lookup"><span data-stu-id="644d9-124">Specify hello Azure Site Recovery subscription and hello vault name.</span></span>

    ![Serverregistrering](./media/vmm-to-azure-walkthrough-source-target/provider10.png)
6. <span data-ttu-id="644d9-126">I **Internetanslutning**, ange hur providern som körs på hello VMM-servern ansluter tooSite Recovery via hello hello internet.</span><span class="sxs-lookup"><span data-stu-id="644d9-126">In **Internet Connection**, specify how hello Provider running on hello VMM server will connect tooSite Recovery over hello internet.</span></span>

   * <span data-ttu-id="644d9-127">Om du vill hello providern tooconnect direkt markerar **ansluter direkt tooAzure Site Recovery utan en proxy**.</span><span class="sxs-lookup"><span data-stu-id="644d9-127">If you want hello Provider tooconnect directly, select **Connect directly tooAzure Site Recovery without a proxy**.</span></span>
   * <span data-ttu-id="644d9-128">Om den befintliga proxyservern kräver autentisering eller om du vill toouse en anpassad proxyserver, Välj **ansluta tooAzure Platsåterställningen genom att använda en proxyserver**.</span><span class="sxs-lookup"><span data-stu-id="644d9-128">If your existing proxy requires authentication, or you want toouse a custom proxy, select **Connect tooAzure Site Recovery using a proxy server**.</span></span>
   * <span data-ttu-id="644d9-129">Om du använder en anpassad proxyserver, ange hello-adress, port och autentiseringsuppgifter.</span><span class="sxs-lookup"><span data-stu-id="644d9-129">If you use a custom proxy, specify hello address, port, and credentials.</span></span>
   * <span data-ttu-id="644d9-130">Om du använder en proxyserver du bör redan ha tillåtit hello webbadresser som beskrivs i [krav](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="644d9-130">If you're using a proxy, you should have already allowed hello URLs described in [prerequisites](#on-premises-prerequisites).</span></span>
   * <span data-ttu-id="644d9-131">Om du använder en anpassad proxyserver VMM RunAs-konto (DRAProxyAccount) skapas automatiskt med hello angivna autentiseringsuppgifterna för proxyn.</span><span class="sxs-lookup"><span data-stu-id="644d9-131">If you use a custom proxy, a VMM RunAs account (DRAProxyAccount) will be created automatically using hello specified proxy credentials.</span></span> <span data-ttu-id="644d9-132">Konfigurera hello proxyserver så att det här kontot kan autentiseras.</span><span class="sxs-lookup"><span data-stu-id="644d9-132">Configure hello proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="644d9-133">inställningarna för hello VMM RunAs-konto kan ändras i hello VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="644d9-133">hello VMM RunAs account settings can be modified in hello VMM console.</span></span> <span data-ttu-id="644d9-134">I **inställningar**, expandera **säkerhet** > **kör som-konton**, och sedan ändra hello lösenordet för DRAProxyAccount.</span><span class="sxs-lookup"><span data-stu-id="644d9-134">In **Settings**, expand **Security** > **Run As Accounts**, and then modify hello password for DRAProxyAccount.</span></span> <span data-ttu-id="644d9-135">Du behöver toorestart hello VMM-tjänsten så att den här inställningen träder i kraft.</span><span class="sxs-lookup"><span data-stu-id="644d9-135">You’ll need toorestart hello VMM service so that this setting takes effect.</span></span>

     ![Internet](./media/vmm-to-azure-walkthrough-source-target/provider13.png)
7. <span data-ttu-id="644d9-137">Acceptera eller ändra hello platsen för ett SSL-certifikat som genereras automatiskt för datakryptering.</span><span class="sxs-lookup"><span data-stu-id="644d9-137">Accept or modify hello location of an SSL certificate that’s automatically generated for data encryption.</span></span> <span data-ttu-id="644d9-138">Det här certifikatet används om du aktiverar datakryptering för ett moln som skyddas av Azure hello Azure Site Recovery-portalen.</span><span class="sxs-lookup"><span data-stu-id="644d9-138">This certificate is used if you enable data encryption for a cloud protected by Azure in hello Azure Site Recovery portal.</span></span> <span data-ttu-id="644d9-139">Skydda det här certifikatet.</span><span class="sxs-lookup"><span data-stu-id="644d9-139">Keep this certificate safe.</span></span> <span data-ttu-id="644d9-140">När du kör en växling vid fel tooAzure måste den toodecrypt, om datakryptering är aktiverat.</span><span class="sxs-lookup"><span data-stu-id="644d9-140">When you run a failover tooAzure you’ll need it toodecrypt, if data encryption is enabled.</span></span>
8. <span data-ttu-id="644d9-141">I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="644d9-141">In **Server name**, specify a friendly name tooidentify hello VMM server in hello vault.</span></span> <span data-ttu-id="644d9-142">Ange hello VMM klusternamnet roll i en klusterkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="644d9-142">In a cluster configuration, specify hello VMM cluster role name.</span></span>
9. <span data-ttu-id="644d9-143">Aktivera **synkronisera molnmetadata**om du vill toosynchronize metadata för alla moln på hello VMM-servern med hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="644d9-143">Enable **Sync cloud metadata**, if you want toosynchronize metadata for all clouds on hello VMM server with hello vault.</span></span> <span data-ttu-id="644d9-144">Den här åtgärden behöver bara toohappen en gång på varje server.</span><span class="sxs-lookup"><span data-stu-id="644d9-144">This action only needs toohappen once on each server.</span></span> <span data-ttu-id="644d9-145">Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="644d9-145">If you don't want toosynchronize all clouds, you can leave this setting unchecked and synchronize each cloud individually in hello cloud properties in hello VMM console.</span></span> <span data-ttu-id="644d9-146">Klicka på **registrera** toocomplete hello processen.</span><span class="sxs-lookup"><span data-stu-id="644d9-146">Click **Register** toocomplete hello process.</span></span>

    ![Serverregistrering](./media/vmm-to-azure-walkthrough-source-target/provider16.png)
10. <span data-ttu-id="644d9-148">Registreringen startar.</span><span class="sxs-lookup"><span data-stu-id="644d9-148">Registration starts.</span></span> <span data-ttu-id="644d9-149">När registreringen är klar hello server visas i **Site Recovery-infrastruktur** > **VMM-servrar**.</span><span class="sxs-lookup"><span data-stu-id="644d9-149">After registration finishes, hello server is displayed in **Site Recovery Infrastructure** > **VMM Servers**.</span></span>


## <a name="install-hello-azure-recovery-services-agent-on-hyper-v-hosts"></a><span data-ttu-id="644d9-150">Installera hello Azure Recovery Services-agenten på Hyper-V-värdar</span><span class="sxs-lookup"><span data-stu-id="644d9-150">Install hello Azure Recovery Services agent on Hyper-V hosts</span></span>

1. <span data-ttu-id="644d9-151">När du har konfigurerat hello providern behöver du toodownload hello installationsfilen för hello Azure Recovery Services-agenten.</span><span class="sxs-lookup"><span data-stu-id="644d9-151">After you've set up hello Provider, you need toodownload hello installation file for hello Azure Recovery Services agent.</span></span> <span data-ttu-id="644d9-152">Kör installationsprogrammet på varje Hyper-V-server i hello VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="644d9-152">Run setup on each Hyper-V server in hello VMM cloud.</span></span>

    ![Hyper-V-platser](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent1.png)
2. <span data-ttu-id="644d9-154">I **Kravkontroll**, klicka på **Nästa**.</span><span class="sxs-lookup"><span data-stu-id="644d9-154">In **Prerequisites Check**, click **Next**.</span></span> <span data-ttu-id="644d9-155">Alla nödvändiga komponenter som saknas installeras automatiskt.</span><span class="sxs-lookup"><span data-stu-id="644d9-155">Any missing prerequisites will be automatically installed.</span></span>

    ![Krav för Recovery Services-agenten](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent2.png)
3. <span data-ttu-id="644d9-157">I **installationsinställningar**Godkänn eller ändra hello installationsplatsen och hello cacheplatsen.</span><span class="sxs-lookup"><span data-stu-id="644d9-157">In **Installation Settings**, accept or modify hello installation location, and hello cache location.</span></span> <span data-ttu-id="644d9-158">Du kan konfigurera hello cachen på en enhet som har minst 5 GB tillgängligt utrymme, men vi rekommenderar en cacheenhet med 600 GB eller mer ledigt utrymme.</span><span class="sxs-lookup"><span data-stu-id="644d9-158">You can configure hello cache on a drive that has at least 5 GB of storage available but we recommend a cache drive with 600 GB or more of free space.</span></span> <span data-ttu-id="644d9-159">Klicka på **Installera**.</span><span class="sxs-lookup"><span data-stu-id="644d9-159">Then click **Install**.</span></span>
4. <span data-ttu-id="644d9-160">När installationen är klar klickar du på **Stäng** toofinish.</span><span class="sxs-lookup"><span data-stu-id="644d9-160">After installation is complete, click **Close** toofinish.</span></span>

    ![Registrera MARS-agenten](./media/vmm-to-azure-walkthrough-source-target/hyperv-agent3.png)

### <a name="command-line-installation"></a><span data-ttu-id="644d9-162">Kommandoradsinstallation</span><span class="sxs-lookup"><span data-stu-id="644d9-162">Command line installation</span></span>
<span data-ttu-id="644d9-163">Du kan installera hello Microsoft Azure Recovery Services-agenten från kommandoraden med hjälp av hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="644d9-163">You can install hello Microsoft Azure Recovery Services Agent from command line using hello following command:</span></span>

     marsagentinstaller.exe /q /nu

### <a name="set-up-internet-proxy-access-toosite-recovery-from-hyper-v-hosts"></a><span data-ttu-id="644d9-164">Konfigurera internet proxy åtkomst tooSite återställning från Hyper-V-värdar</span><span class="sxs-lookup"><span data-stu-id="644d9-164">Set up internet proxy access tooSite Recovery from Hyper-V hosts</span></span>

<span data-ttu-id="644d9-165">hello Recovery Services-agenten körs på Hyper-V-värdar måste tooAzure för internet-åtkomst för VM-replikering.</span><span class="sxs-lookup"><span data-stu-id="644d9-165">hello Recovery Services agent running on Hyper-V hosts needs internet access tooAzure for VM replication.</span></span> <span data-ttu-id="644d9-166">Om du försöker komma åt hello internet via en proxyserver, ställa in den på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="644d9-166">If you're accessing hello internet through a proxy, set it up as follows:</span></span>

1. <span data-ttu-id="644d9-167">Öppna hello Microsoft Azure Backup MMC snapin-modulen på hello Hyper-V-värd.</span><span class="sxs-lookup"><span data-stu-id="644d9-167">Open hello Microsoft Azure Backup MMC snap-in on hello Hyper-V host.</span></span> <span data-ttu-id="644d9-168">Som standard finns en genväg till Microsoft Azure Backup på hello skrivbordet eller i C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span><span class="sxs-lookup"><span data-stu-id="644d9-168">By default, a shortcut for Microsoft Azure Backup is available on hello desktop or in C:\Program Files\Microsoft Azure Recovery Services Agent\bin\wabadmin.</span></span>
2. <span data-ttu-id="644d9-169">I snapin-modulen hello, klickar du på **ändra egenskaper för**.</span><span class="sxs-lookup"><span data-stu-id="644d9-169">In hello snap-in, click **Change Properties**.</span></span>
3. <span data-ttu-id="644d9-170">På hello **proxykonfiguration** anger du information om proxyservern.</span><span class="sxs-lookup"><span data-stu-id="644d9-170">On hello **Proxy Configuration** tab, specify proxy server information.</span></span>

    ![Registrera MARS-agenten](./media/vmm-to-azure-walkthrough-source-target/mars-proxy.png)
4. <span data-ttu-id="644d9-172">Kontrollera att hello-agenten kan nå hello webbadresser som beskrivs i hello [krav](#on-premises-prerequisites).</span><span class="sxs-lookup"><span data-stu-id="644d9-172">Check that hello agent can reach hello URLs described in hello [prerequisites](#on-premises-prerequisites).</span></span>

## <a name="set-up-hello-target-environment"></a><span data-ttu-id="644d9-173">Ställ in hello målmiljön</span><span class="sxs-lookup"><span data-stu-id="644d9-173">Set up hello target environment</span></span>
<span data-ttu-id="644d9-174">Ange hello Azure storage-konto toobe används för replikering och ansluter hello Azure-nätverk toowhich virtuella Azure-datorer efter redundans.</span><span class="sxs-lookup"><span data-stu-id="644d9-174">Specify hello Azure storage account toobe used for replication, and hello Azure network toowhich Azure VMs will connect after failover.</span></span>

1. <span data-ttu-id="644d9-175">Klicka på **Förbered infrastruktur** > **mål**hello prenumerationen och välj hello resursgruppen där du vill att toocreate hello misslyckades för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="644d9-175">Click **Prepare infrastructure** > **Target**, select hello subscription and hello resource group where you want toocreate hello failed over virtual machines.</span></span> <span data-ttu-id="644d9-176">Välj hello distributionsmodell som du vill toouse i Azure (klassiska eller resource management) för hello misslyckades för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="644d9-176">Choose hello deployment model that you want toouse in Azure (classic or resource management) for hello failed over virtual machines.</span></span>

    ![Lagring](./media/vmm-to-azure-walkthrough-source-target/enablerep3.png)

2. <span data-ttu-id="644d9-178">Site Recovery kontrollerar att du har ett eller flera kompatibla Azure-lagringskonton och Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="644d9-178">Site Recovery checks that you have one or more compatible Azure storage accounts and networks.</span></span>

    ![Lagring](./media/vmm-to-azure-walkthrough-source-target/compatible-storage.png)

3. <span data-ttu-id="644d9-180">Om du inte har skapat ett lagringskonto och du vill toocreate en med Resource Manager klickar du på **+ lagringskonto** toodo det internt.</span><span class="sxs-lookup"><span data-stu-id="644d9-180">If you haven't created a storage account, and you want toocreate one using Resource Manager, click **+Storage account** toodo that inline.</span></span>  <span data-ttu-id="644d9-181">På hello **skapa lagringskonto** anger ett kontonamn, typ, prenumeration och plats.</span><span class="sxs-lookup"><span data-stu-id="644d9-181">On hello **Create storage account** blade specify an account name, type, subscription, and location.</span></span> <span data-ttu-id="644d9-182">hello-konto måste finnas i hello samma plats som hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="644d9-182">hello account should be in hello same location as hello Recovery Services vault.</span></span>

   ![Lagring](./media/vmm-to-azure-walkthrough-source-target/gs-createstorage.png)


   * <span data-ttu-id="644d9-184">Om du vill toocreate ett lagringskonto med hjälp av hello klassiska modellen gör det hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="644d9-184">If you want toocreate a storage account using hello classic model, do that in hello Azure portal.</span></span> [<span data-ttu-id="644d9-185">Läs mer</span><span class="sxs-lookup"><span data-stu-id="644d9-185">Learn more</span></span>](../storage/common/storage-create-storage-account.md)
   * <span data-ttu-id="644d9-186">Om du använder ett premium-lagringskonto för replikerade data, skapa en ytterligare standardlagringskonto toostore replikeringsloggar som samlar in löpande ändringar tooon lokala data.</span><span class="sxs-lookup"><span data-stu-id="644d9-186">If you’re using a premium storage account for replicated data, set up an additional standard storage account, toostore replication logs that capture ongoing changes tooon-premises data.</span></span>
5. <span data-ttu-id="644d9-187">Om du inte har skapat ett Azure-nätverk och du vill toocreate en med Resource Manager klickar du på **+ nätverk** toodo det internt.</span><span class="sxs-lookup"><span data-stu-id="644d9-187">If you haven't created an Azure network, and you want toocreate one using Resource Manager, click **+Network** toodo that inline.</span></span> <span data-ttu-id="644d9-188">På hello **skapa virtuellt nätverk** anger ett nätverksnamn, adressintervall, information om undernät, prenumeration och plats.</span><span class="sxs-lookup"><span data-stu-id="644d9-188">On hello **Create virtual network** blade specify a network name, address range, subnet details, subscription, and location.</span></span> <span data-ttu-id="644d9-189">hello nätverket bör finnas i hello samma plats som hello Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="644d9-189">hello network should be in hello same location as hello Recovery Services vault.</span></span>

   ![Nätverk](./media/vmm-to-azure-walkthrough-source-target/gs-createnetwork.png)

   <span data-ttu-id="644d9-191">Om du vill toocreate ett nätverk med hello klassiska modellen gör det hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="644d9-191">If you want toocreate a network using hello classic model, do that in hello Azure portal.</span></span> <span data-ttu-id="644d9-192">[Läs mer](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span><span class="sxs-lookup"><span data-stu-id="644d9-192">[Learn more](../virtual-network/virtual-networks-create-vnet-classic-pportal.md).</span></span>





## <a name="next-steps"></a><span data-ttu-id="644d9-193">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="644d9-193">Next steps</span></span>

<span data-ttu-id="644d9-194">Gå för[steg 9: Konfigurera nätverksmappning](vmm-to-azure-walkthrough-network-mapping.md)</span><span class="sxs-lookup"><span data-stu-id="644d9-194">Go too[Step 9: Configure network mapping](vmm-to-azure-walkthrough-network-mapping.md)</span></span>
