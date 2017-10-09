---
title: "aaaSet hello källa och mål för Hyper-V-replikering tooa sekundär plats med Azure Site Recovery | Microsoft Docs"
description: "Beskriver hur tooset upp hello källa och mål när replikera virtuella Hyper-V-datorer toosecondary VMM webbplats med Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: fa7809f1-7633-425f-b25d-d10d004e8d0b
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/30/2017
ms.author: raynew
ms.openlocfilehash: 451cb4413ca5c09777a7faf512b1c8ea43e695f7
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-6-set-up-hello-replication-source-and-target"></a><span data-ttu-id="1059f-103">Steg 6: Konfigurera hello replikeringskälla och mål</span><span class="sxs-lookup"><span data-stu-id="1059f-103">Step 6: Set up hello replication source and target</span></span>


<span data-ttu-id="1059f-104">När du har skapat en återställningstjänster valvet för Hyper-V-replikering tooa sekundär VMM-plats med [Azure Site Recovery](site-recovery-overview.md), Använd den här artikeln tooset hello källa och mål replikering platser.</span><span class="sxs-lookup"><span data-stu-id="1059f-104">After creating a Recovery Services vault for Hyper-V replication tooa secondary VMM site with [Azure Site Recovery](site-recovery-overview.md), use this article tooset up hello source and target replication locations.</span></span> 

<span data-ttu-id="1059f-105">När du har läst den här artikeln efter eventuella kommentarer längst ned hello, eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="1059f-105">After reading this article, post any comments at hello bottom, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>




## <a name="set-up-hello-source-environment"></a><span data-ttu-id="1059f-106">Ställ in hello källmiljön</span><span class="sxs-lookup"><span data-stu-id="1059f-106">Set up hello source environment</span></span>

<span data-ttu-id="1059f-107">Installera hello Azure Site Recovery-providern på VMM-servrar och identifiera och registrera servrarna i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="1059f-107">Install hello Azure Site Recovery Provider on VMM servers, and discover and register servers in hello vault.</span></span>

1. <span data-ttu-id="1059f-108">Klicka på **steg 1: förbereda infrastrukturen** > **källa**.</span><span class="sxs-lookup"><span data-stu-id="1059f-108">Click **Step 1: Prepare Infrastructure** > **Source**.</span></span>

    ![Konfigurera källan](./media/vmm-to-vmm-walkthrough-source-target/goals-source.png)
2. <span data-ttu-id="1059f-110">I **Förbered källa**, klickar du på **+ VMM** tooadd en VMM-server.</span><span class="sxs-lookup"><span data-stu-id="1059f-110">In **Prepare source**, click **+ VMM** tooadd a VMM server.</span></span>

    ![Konfigurera källan](./media/vmm-to-vmm-walkthrough-source-target/set-source1.png)
3. <span data-ttu-id="1059f-112">I **Lägg till Server**, kontrollera att **System Center VMM-servern** visas i **servertyp** och den hello VMM-servern uppfyller hello [krav](#prerequisites).</span><span class="sxs-lookup"><span data-stu-id="1059f-112">In **Add Server**, check that **System Center VMM server** appears in **Server type** and that hello VMM server meets hello [prerequisites](#prerequisites).</span></span>
4. <span data-ttu-id="1059f-113">Hämta installationsfilen för hello Azure Site Recovery-providern.</span><span class="sxs-lookup"><span data-stu-id="1059f-113">Download hello Azure Site Recovery Provider installation file.</span></span>
5. <span data-ttu-id="1059f-114">Hämta hello registreringsnyckel.</span><span class="sxs-lookup"><span data-stu-id="1059f-114">Download hello registration key.</span></span> <span data-ttu-id="1059f-115">Du behöver den när du kör installationsprogrammet.</span><span class="sxs-lookup"><span data-stu-id="1059f-115">You need this when you run setup.</span></span> <span data-ttu-id="1059f-116">hello nyckeln är giltig i fem dagar efter genereringen.</span><span class="sxs-lookup"><span data-stu-id="1059f-116">hello key is valid for five days after you generate it.</span></span>

    ![Konfigurera källan](./media/vmm-to-vmm-walkthrough-source-target/set-source3.png)
6. <span data-ttu-id="1059f-118">Installera hello Azure Site Recovery Provider på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="1059f-118">Install hello Azure Site Recovery Provider on hello VMM server.</span></span> <span data-ttu-id="1059f-119">Du behöver inte installera tooexplicitly något på Hyper-V-värdservrar.</span><span class="sxs-lookup"><span data-stu-id="1059f-119">You don't need tooexplicitly install anything on Hyper-V host servers.</span></span>


## <a name="install-hello-azure-site-recovery-provider"></a><span data-ttu-id="1059f-120">Installera hello Azure Site Recovery Provider</span><span class="sxs-lookup"><span data-stu-id="1059f-120">Install hello Azure Site Recovery Provider</span></span>

1. <span data-ttu-id="1059f-121">Kör installationsfilen för hello-providern på varje VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="1059f-121">Run hello Provider setup file on each VMM server.</span></span> <span data-ttu-id="1059f-122">Om VMM distribueras i ett kluster, följande hello hello första gången du installerar:</span><span class="sxs-lookup"><span data-stu-id="1059f-122">If VMM is deployed in a cluster, do hello following hello first time you install:</span></span>
    -  <span data-ttu-id="1059f-123">Installera hello-providern på en aktiv nod och slutför hello installation tooregister hello VMM-servern i valvet hello.</span><span class="sxs-lookup"><span data-stu-id="1059f-123">Install hello provider on an active node, and finish hello installation tooregister hello VMM server in hello vault.</span></span>
    - <span data-ttu-id="1059f-124">Installera sedan hello providern på hello andra noder.</span><span class="sxs-lookup"><span data-stu-id="1059f-124">Then, install hello Provider on hello other nodes.</span></span> <span data-ttu-id="1059f-125">Klusternoder bör köra hello samma version av hello providern.</span><span class="sxs-lookup"><span data-stu-id="1059f-125">Cluster nodes should all run hello same version of hello Provider.</span></span>
2. <span data-ttu-id="1059f-126">Installationsprogrammet körs några kontroller av förutsättningar och begär behörighet toostop hello VMM-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="1059f-126">Setup runs a few prerequisite checks, and requests permission toostop hello VMM service.</span></span> <span data-ttu-id="1059f-127">hello VMM-tjänsten kommer att startas om automatiskt när installationen är klar.</span><span class="sxs-lookup"><span data-stu-id="1059f-127">hello VMM service will be restarted automatically when setup finishes.</span></span> <span data-ttu-id="1059f-128">Om du installerar på en VMM-klustret är begärd toostop hello klustrad roll.</span><span class="sxs-lookup"><span data-stu-id="1059f-128">If you install on a VMM cluster, you're prompted toostop hello Cluster role.</span></span>
3. <span data-ttu-id="1059f-129">I **Microsoft Update**, kan du välja toospecify att provideruppdateringarna installeras i enlighet med Microsoft Update-princip.</span><span class="sxs-lookup"><span data-stu-id="1059f-129">In **Microsoft Update**, you can opt in toospecify that provider updates are installed in accordance with your Microsoft Update policy.</span></span>
4. <span data-ttu-id="1059f-130">I **Installation**acceptera eller ändra hello standardplatsen för installation och klicka på **installera**.</span><span class="sxs-lookup"><span data-stu-id="1059f-130">In **Installation**, accept or modify hello default installation location, and click **Install**.</span></span>

    ![Installationsplats](./media/vmm-to-vmm-walkthrough-source-target/provider-location.png)
5. <span data-ttu-id="1059f-132">När installationen är klar klickar du på **registrera** tooregister hello server i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="1059f-132">After installation is complete, click **Register** tooregister hello server in hello vault.</span></span>

    ![Installationsplats](./media/vmm-to-vmm-walkthrough-source-target/provider-register.png)
6. <span data-ttu-id="1059f-134">I **valvnamnet**, verifierar hello valvet som servern ska registreras i vilka hello hello-namn.</span><span class="sxs-lookup"><span data-stu-id="1059f-134">In **Vault name**, verify hello name of hello vault in which hello server will be registered.</span></span> <span data-ttu-id="1059f-135">Klicka på *Nästa*.</span><span class="sxs-lookup"><span data-stu-id="1059f-135">Click *Next*.</span></span>

    ![Serverregistrering](./media/vmm-to-vmm-walkthrough-source-target/vaultcred.png)
7. <span data-ttu-id="1059f-137">I **Internetanslutning**, ange hur hello-providern som körs på hello VMM-servern ansluter tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1059f-137">In **Internet Connection**, specify how hello provider running on hello VMM server connects tooAzure.</span></span>

    ![Internetinställningar](./media/vmm-to-vmm-walkthrough-source-target/proxydetails.png)

   - <span data-ttu-id="1059f-139">Du kan ange hello providern ska ansluta direkt toohello internet, eller via en proxyserver.</span><span class="sxs-lookup"><span data-stu-id="1059f-139">You can specify that hello provider should connect directly toohello internet, or via a proxy.</span></span>
   - <span data-ttu-id="1059f-140">Ange proxy-inställningar om det behövs.</span><span class="sxs-lookup"><span data-stu-id="1059f-140">Specify proxy settings if needed.</span></span>
   - <span data-ttu-id="1059f-141">Om du använder en proxyserver VMM RunAs-konto (DRAProxyAccount) skapas automatiskt med hjälp av hello angivna autentiseringsuppgifterna för proxyn.</span><span class="sxs-lookup"><span data-stu-id="1059f-141">If you use a proxy, a VMM RunAs account (DRAProxyAccount) is created automatically using hello specified proxy credentials.</span></span> <span data-ttu-id="1059f-142">Konfigurera hello proxyserver så att det här kontot kan autentiseras.</span><span class="sxs-lookup"><span data-stu-id="1059f-142">Configure hello proxy server so that this account can authenticate successfully.</span></span> <span data-ttu-id="1059f-143">hello inställningarna för RunAs-konto kan ändras i hello VMM-konsolen > **inställningar** > **säkerhet** > **kör som-konton**.</span><span class="sxs-lookup"><span data-stu-id="1059f-143">hello RunAs account settings can be modified in hello VMM console > **Settings** > **Security** > **Run As Accounts**.</span></span> <span data-ttu-id="1059f-144">Starta om hello VMM-tjänsten tooupdate ändras.</span><span class="sxs-lookup"><span data-stu-id="1059f-144">Restart hello VMM service tooupdate changes.</span></span>
8. <span data-ttu-id="1059f-145">I **registreringsnyckel**väljer hello nyckel som du hämtade från Azure Site Recovery och kopierat toohello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="1059f-145">In **Registration Key**, select hello key that you downloaded from Azure Site Recovery and copied toohello VMM server.</span></span>
9. <span data-ttu-id="1059f-146">hello krypteringsinställning används bara när du replikerar virtuella Hyper-V-datorer i VMM-moln tooAzure.</span><span class="sxs-lookup"><span data-stu-id="1059f-146">hello encryption setting is only used when you're replicating Hyper-V VMs in VMM clouds tooAzure.</span></span> <span data-ttu-id="1059f-147">Om du replikerar tooa sekundär plats används den inte.</span><span class="sxs-lookup"><span data-stu-id="1059f-147">If you're replicating tooa secondary site it's not used.</span></span>
10. <span data-ttu-id="1059f-148">I **servernamn**, ange ett eget namn tooidentify hello VMM-servern i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="1059f-148">In **Server name**, specify a friendly name tooidentify hello VMM server in hello vault.</span></span> <span data-ttu-id="1059f-149">Ange hello VMM klusternamnet roll i en klusterkonfiguration.</span><span class="sxs-lookup"><span data-stu-id="1059f-149">In a cluster configuration specify hello VMM cluster role name.</span></span>
11. <span data-ttu-id="1059f-150">I **synkronisera molnmetadata**väljer du om du vill använda toosynchronize metadata för alla moln på hello VMM-servern med hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="1059f-150">In **Synchronize cloud metadata**, select whether you want toosynchronize metadata for all clouds on hello VMM server with hello vault.</span></span> <span data-ttu-id="1059f-151">Den här åtgärden behöver bara toohappen en gång på varje server.</span><span class="sxs-lookup"><span data-stu-id="1059f-151">This action only needs toohappen once on each server.</span></span> <span data-ttu-id="1059f-152">Om du inte vill toosynchronize alla moln kan du lämna den här inställningen avmarkerad och synkronisera varje moln individuellt i hello molnegenskaperna hello VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="1059f-152">If you don't want toosynchronize all clouds, you can leave this setting unchecked, and synchronize each cloud individually in hello cloud properties in hello VMM console.</span></span>
12. <span data-ttu-id="1059f-153">Klicka på **nästa** toocomplete hello processen.</span><span class="sxs-lookup"><span data-stu-id="1059f-153">Click **Next** toocomplete hello process.</span></span> <span data-ttu-id="1059f-154">Efter registreringen hämtas metadata från hello VMM-servern av Azure Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="1059f-154">After registration, metadata from hello VMM server is retrieved by Azure Site Recovery.</span></span> <span data-ttu-id="1059f-155">hello servern visas på hello **VMM-servrar** fliken på hello **servrar** sida i hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="1059f-155">hello server is displayed on hello **VMM Servers** tab on hello **Servers** page in hello vault.</span></span>

    ![Server](./media/vmm-to-vmm-walkthrough-source-target/provider13.png)
13. <span data-ttu-id="1059f-157">När hello-servern är tillgänglig i hello Site Recovery-konsolen i **källa** > **Förbered källa** Välj hello VMM-servern och välj hello moln i vilka hello Hyper-V-värd finns.</span><span class="sxs-lookup"><span data-stu-id="1059f-157">After hello server is available in hello Site Recovery console, in **Source** > **Prepare source** select hello VMM server, and select hello cloud in which hello Hyper-V host is located.</span></span> <span data-ttu-id="1059f-158">Klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="1059f-158">Then click **OK**.</span></span>

<span data-ttu-id="1059f-159">Du kan också installera hello-provider från hello kommandorad:</span><span class="sxs-lookup"><span data-stu-id="1059f-159">You can also install hello provider from hello command line:</span></span>

[!INCLUDE [site-recovery-rw-provider-command-line](../../includes/site-recovery-rw-provider-command-line.md)]


## <a name="set-up-hello-target-environment"></a><span data-ttu-id="1059f-160">Ställ in hello målmiljön</span><span class="sxs-lookup"><span data-stu-id="1059f-160">Set up hello target environment</span></span>

<span data-ttu-id="1059f-161">Välj VMM-målservern hello och molnet:</span><span class="sxs-lookup"><span data-stu-id="1059f-161">Select hello target VMM server and cloud:</span></span>

1. <span data-ttu-id="1059f-162">Klicka på **Förbered infrastruktur** > **mål**, och välj hello VMM-målservern du vill toouse.</span><span class="sxs-lookup"><span data-stu-id="1059f-162">Click **Prepare infrastructure** > **Target**, and select hello target VMM server you want toouse.</span></span>
2. <span data-ttu-id="1059f-163">Moln på hello-servern som är synkroniserade med Site Recovery visas.</span><span class="sxs-lookup"><span data-stu-id="1059f-163">Clouds on hello server that are synchronized with Site Recovery will be displayed.</span></span> <span data-ttu-id="1059f-164">Välj hello målmoln.</span><span class="sxs-lookup"><span data-stu-id="1059f-164">Select hello target cloud.</span></span>

   ![mål](./media/vmm-to-vmm-walkthrough-source-target/target-vmm.png)



## <a name="next-steps"></a><span data-ttu-id="1059f-166">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1059f-166">Next steps</span></span>

<span data-ttu-id="1059f-167">Gå för[steg 7: Konfigurera nätverksmappning](vmm-to-vmm-walkthrough-network-mapping.md).</span><span class="sxs-lookup"><span data-stu-id="1059f-167">Go too[Step 7: Configure network mapping](vmm-to-vmm-walkthrough-network-mapping.md).</span></span>
