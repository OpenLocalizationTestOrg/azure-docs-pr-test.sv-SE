---
title: "aaaEnable replikering för virtuella datorer i Azure tooanother Azure-region med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattar hello stegen tooenable replikering tooanother Azure-region för virtuella Azure-datorer med hjälp av hello Azure Site Recovery-tjänsten"
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmonm
editor: 
ms.assetid: a309644f-d36b-4188-bba7-ad45a2d9bede
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 8/01/2017
ms.author: raynew
ms.openlocfilehash: 2fa03db45a18ccb8b9f31ed05589be0dd6d5f031
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a><span data-ttu-id="118fd-103">Steg 5: Aktivera replikering för virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="118fd-103">Step 5: Enable replication for Azure VMs</span></span>


<span data-ttu-id="118fd-104">När du har installerat en [Recovery Services-valvet](azure-to-azure-walkthrough-vault.md), använda den här artikeln tooenable replikering av virtuella datorer (VM), tooanother Azure-region med [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="118fd-104">After setting up a [Recovery Services vault](azure-to-azure-walkthrough-vault.md), use this article tooenable replication of virtual machines (VMs), tooanother Azure region, with [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="118fd-105">tooenable replikering, konfigurera inställningar för källa och mål, kontrollera hello replikeringsprinciper och välj virtuella datorer du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="118fd-105">tooenable replication, you set up source and target settings, verify hello replication policy, and select VMs you want tooreplicate.</span></span>

- <span data-ttu-id="118fd-106">När du har virtuella datorerna i Azure-hello artikeln bör replikerar toohello sekundära Azure-region.</span><span class="sxs-lookup"><span data-stu-id="118fd-106">When you finish hello article, your Azure VMs should be replicating toohello secondary Azure region.</span></span>
- <span data-ttu-id="118fd-107">Skicka kommentarer längst ned hello i den här artikeln eller ställa frågor i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="118fd-107">Post any comments at hello bottom of this article, or ask questions in hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

>[!NOTE]
>
> <span data-ttu-id="118fd-108">Azure VM-replikering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="118fd-108">Azure VM replication is currently in preview.</span></span>


## <a name="select-hello-source"></a><span data-ttu-id="118fd-109">Välj hello källa</span><span class="sxs-lookup"><span data-stu-id="118fd-109">Select hello source</span></span> 

1. <span data-ttu-id="118fd-110">Klicka på valvnamnet för hello i Recovery Services-valv > **+ replikera**.</span><span class="sxs-lookup"><span data-stu-id="118fd-110">In Recovery Services vaults, click hello vault name > **+Replicate**.</span></span>
2. <span data-ttu-id="118fd-111">I **källa**väljer **Azure - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="118fd-111">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="118fd-112">I **datakällplats**väljer hello källa Azure-region där din virtuella dator körs för närvarande.</span><span class="sxs-lookup"><span data-stu-id="118fd-112">In **Source location**, select hello source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="118fd-113">Välj hello **virtuella Azure-datorn distributionsmodell** för virtuella datorer: **Resource Manager** eller **klassiska**.</span><span class="sxs-lookup"><span data-stu-id="118fd-113">Select hello **Azure virtual machine deployment model** for VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="118fd-114">Välj hello **källresursgruppen** för hanteraren för virtuella datorer, eller **Molntjänsten** för klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="118fd-114">Select hello **Source resource group** for Resource Manager VMs, or **cloud service** for classic VMs.</span></span>
5. <span data-ttu-id="118fd-115">Klicka på **OK** toosave hello inställningar.</span><span class="sxs-lookup"><span data-stu-id="118fd-115">Click **OK** toosave hello settings.</span></span>

    ![Konfigurera hello källa](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-hello-vms"></a><span data-ttu-id="118fd-117">Välj hello virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="118fd-117">Select hello VMs</span></span>

<span data-ttu-id="118fd-118">Site Recovery hämtar en lista över hello virtuella datorer som är associerade med hello prenumeration och resurs grupp eller ett moln-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="118fd-118">Site Recovery retrieves a list of hello VMs associated with hello subscription and resource group/cloud service.</span></span>

1. <span data-ttu-id="118fd-119">I **virtuella datorer**, Välj hello virtuella datorer du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="118fd-119">In **Virtual Machines**, select hello VMs you want tooreplicate.</span></span>
2. <span data-ttu-id="118fd-120">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="118fd-120">Click **OK**.</span></span>

    ![Välj virtuella datorer](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a><span data-ttu-id="118fd-122">Konfigurera inställningar</span><span class="sxs-lookup"><span data-stu-id="118fd-122">Configure settings</span></span>

<span data-ttu-id="118fd-123">Site Recovery tillhandahåller standardinställningarna för hello målregionen (baserat på inställningarna för hello datakälla region), och hello replikeringsprincip:</span><span class="sxs-lookup"><span data-stu-id="118fd-123">Site Recovery provisions default settings for hello target region (based on hello source region settings), and hello replication policy:</span></span>

   - <span data-ttu-id="118fd-124">**Målplatsen**: hello målregionen som du vill använda toouse för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="118fd-124">**Target location**: hello target region you want toouse for disaster recovery.</span></span> <span data-ttu-id="118fd-125">Vi rekommenderar att hello målplats matchar hello platsen för hello Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="118fd-125">We recommend that hello target location matches hello location of hello Site Recovery vault.</span></span>
   - <span data-ttu-id="118fd-126">**Målresursgruppen**: resurs grupp toowhich Azure virtuella datorer i hello målregionen hör efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="118fd-126">**Target resource group**: Resource group toowhich Azure VMs in hello target region will belong after failover.</span></span> <span data-ttu-id="118fd-127">Som standard skapar Site Recovery en ny resursgrupp i hello målregionen med ett ”asr” suffix.</span><span class="sxs-lookup"><span data-stu-id="118fd-127">By default, Site Recovery creates a new resource group in hello target region with an "asr" suffix.</span></span> 
   - <span data-ttu-id="118fd-128">**Virtuella målnätverket**: hello nätverk i vilka Azure virtuella datorer i hello målregionen kommer att finnas efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="118fd-128">**Target virtual network**: hello network in which Azure VMs in hello target region will be located after failover.</span></span> <span data-ttu-id="118fd-129">Som standard skapar Site Recovery ett nytt virtuellt nätverk (och undernät) i hello målregionen med ett ”asr” suffix.</span><span class="sxs-lookup"><span data-stu-id="118fd-129">By default, Site Recovery creates a new virtual network (and subnets) in hello target region with an "asr" suffix.</span></span> <span data-ttu-id="118fd-130">Det här nätverket är mappade tooyour Källnätverk.</span><span class="sxs-lookup"><span data-stu-id="118fd-130">This network is mapped tooyour source network.</span></span> <span data-ttu-id="118fd-131">Observera att du kan tilldela en specifik IP-adress efter växling vid fel på en virtuell dator, om du behöver tooretain hello samma IP-adressen i hello käll- och målplatserna.</span><span class="sxs-lookup"><span data-stu-id="118fd-131">Note that you can assign a specific IP address after failover of a VM, if you need tooretain hello same IP address in hello source and target locations.</span></span> 
   - <span data-ttu-id="118fd-132">**Cachelagra lagringskonton**: Site Recovery använder ett lagringskonto i hello källa region.</span><span class="sxs-lookup"><span data-stu-id="118fd-132">**Cache storage accounts**: Site Recovery uses a storage account in hello source region.</span></span> <span data-ttu-id="118fd-133">Ändringar på virtuella källdatorer skickas toothis konto innan replikeringen toohello målplats.</span><span class="sxs-lookup"><span data-stu-id="118fd-133">Changes on source VMs are sent toothis account, before replication toohello target location.</span></span> 
   - <span data-ttu-id="118fd-134">**Rikta lagringskonton**: som standard skapar Site Recovery ett nytt lagringskonto i hello målregionen, toomirror hello källa VM storage-konto.</span><span class="sxs-lookup"><span data-stu-id="118fd-134">**Target storage accounts**: By default, Site Recovery creates a new storage account in hello target region, toomirror hello source VM storage account.</span></span>
   -  <span data-ttu-id="118fd-135">**Rikta tillgänglighetsuppsättningar**: som standard skapar en ny tillgänglighetsuppsättning i hello målregionen med hello ”asr” suffix Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="118fd-135">**Target availability sets**: By default, Site Recovery creates a new availability set in hello target region, with hello "asr" suffix.</span></span> 
   - <span data-ttu-id="118fd-136">**Namn på**: principnamn.</span><span class="sxs-lookup"><span data-stu-id="118fd-136">**Replication policy name**: Policy name.</span></span>
   - <span data-ttu-id="118fd-137">**Kvarhållningstid för återställningspunkten**: som standard sparas Site Recovery återställningspunkter under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="118fd-137">**Recovery point retention**: By default Site Recovery keeps recovery points for 24 hours.</span></span> <span data-ttu-id="118fd-138">Du kan konfigurera ett värde mellan 1 och 72 timmar.</span><span class="sxs-lookup"><span data-stu-id="118fd-138">You can configure a value between 1 and 72 hours.</span></span>
   - <span data-ttu-id="118fd-139">**Frekvens av programkonsekventa ögonblicksbilder**: som standard Site Recovery tar en ögonblicksbild av programkonsekventa var fjärde timme.</span><span class="sxs-lookup"><span data-stu-id="118fd-139">**App-consistent snapshot frequency**: By default Site Recovery takes an app-consistent snapshot every 4 hours.</span></span> <span data-ttu-id="118fd-140">Du kan konfigurera ett värde mellan 1 och 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="118fd-140">You can configure any value between 1 and 12 hours.</span></span> <span data-ttu-id="118fd-141">Data replikeras kontinuerligt:</span><span class="sxs-lookup"><span data-stu-id="118fd-141">Data is replicated continuously:</span></span>
    - <span data-ttu-id="118fd-142">Kraschkonsekvent återställningspunkter upprätthålla konsekventa data write-ordning när de skapas.</span><span class="sxs-lookup"><span data-stu-id="118fd-142">Crash-consistent recovery points maintain consistent data write-order when created.</span></span> <span data-ttu-id="118fd-143">Den här typen av återställningspunkt räcker vanligtvis om din app är utformad toorecover från en krasch utan inkonsekventa data</span><span class="sxs-lookup"><span data-stu-id="118fd-143">This type of recovery point is usually sufficient if your app is designed toorecover from a crash without data inconsistencies</span></span>
    - <span data-ttu-id="118fd-144">Kraschkonsekvent återställningspunkter skapas med några minuters mellanrum.</span><span class="sxs-lookup"><span data-stu-id="118fd-144">Crash-consistent recovery points are generated every few minutes.</span></span> <span data-ttu-id="118fd-145">Med dessa recovery punkter toofail över och Återställ din virtuella dator innehåller hello efter minuter en återställning punkt mål (RPO).</span><span class="sxs-lookup"><span data-stu-id="118fd-145">Using these recovery points toofail over and recover your VMs provides a Recovery Point Objective (RPO) in hello order of minutes.</span></span>
    - <span data-ttu-id="118fd-146">Programkonsekventa återställningspunkter (i tillägget toowrite ordning konsekvenskontroll) kontrollera att appar körs slutföra alla åtgärder och rensa buffertar toodisk (programmet offline stegvis).</span><span class="sxs-lookup"><span data-stu-id="118fd-146">App-consistent recovery points (in addition toowrite-order consistency) ensure that running apps complete all operations and flush buffers toodisk (application quiescing).</span></span> <span data-ttu-id="118fd-147">Vi rekommenderar att du använder dessa återställningspunkter för databas-appar som SQL Server, Oracle och Exchange.</span><span class="sxs-lookup"><span data-stu-id="118fd-147">We recommend you use these recovery points for database apps such as SQL Server, Oracle, and Exchange.</span></span>
        
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a><span data-ttu-id="118fd-149">Ändra inställningar</span><span class="sxs-lookup"><span data-stu-id="118fd-149">Modify settings</span></span>

<span data-ttu-id="118fd-150">Om du vill toomodify mål och replikering principinställningar hello följande:</span><span class="sxs-lookup"><span data-stu-id="118fd-150">If you want toomodify target and replication policy settings, do hello following:</span></span>

1. <span data-ttu-id="118fd-151">tooview eller ändra inställningarna för målet, klickar på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="118fd-151">tooview or modify target settings, click **Settings**.</span></span>
2. <span data-ttu-id="118fd-152">toooverride hello mål standardinställningarna klickar du på **anpassa**.</span><span class="sxs-lookup"><span data-stu-id="118fd-152">toooverride hello default target settings, click **Customize**.</span></span> <span data-ttu-id="118fd-153">Du kan ange en target-resursgruppen, virtuella nätverk, tillgänglighetsuppsättning och mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="118fd-153">You can specify a target resource group, virtual network, availability set, and target storage account.</span></span> <span data-ttu-id="118fd-154">Du kan bara lägga till tillgänglighetsuppsättningar om virtuella datorer som är en del av en uppsättning i hello källa region.</span><span class="sxs-lookup"><span data-stu-id="118fd-154">You can only add availability sets if VMs are part of a set in hello source region.</span></span>

    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. <span data-ttu-id="118fd-156">toooverride replikeringsinställningar för återställningspunkter och programkonsekventa ögonblicksbilder klickar du på **anpassa** nästa för**Replikeringsprincipen**.</span><span class="sxs-lookup"><span data-stu-id="118fd-156">toooverride replication settings for recovery points and app-consistent snapshots, click **Customize** next too**Replication Policy**.</span></span>
 
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. <span data-ttu-id="118fd-158">toostart etablering hello target-resurser, klickar du på **skapa målresurser**.</span><span class="sxs-lookup"><span data-stu-id="118fd-158">toostart provisioning hello target resources, click **Create target resources**.</span></span> <span data-ttu-id="118fd-159">Etablering tar en minut.</span><span class="sxs-lookup"><span data-stu-id="118fd-159">Provisioning takes a minute or so.</span></span> <span data-ttu-id="118fd-160">Stäng inte hello bladet under etableringen eller du har toostart över.</span><span class="sxs-lookup"><span data-stu-id="118fd-160">Don't close hello blade during provisioning, or you'll have toostart over.</span></span>




## <a name="enable-replication"></a><span data-ttu-id="118fd-161">Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="118fd-161">Enable replication</span></span>

1. <span data-ttu-id="118fd-162">I **inställningar**, klickar du på **Aktivera replikering**.</span><span class="sxs-lookup"><span data-stu-id="118fd-162">In **Settings**, click **Enable replication**.</span></span> <span data-ttu-id="118fd-163">Detta gör det möjligt för inledande replikering av hello virtuella datorer som du har valt.</span><span class="sxs-lookup"><span data-stu-id="118fd-163">This enables initial replication of hello VMs you selected.</span></span> <span data-ttu-id="118fd-164">Status för den inledande replikeringen kan ta viss tid toorefresh.</span><span class="sxs-lookup"><span data-stu-id="118fd-164">Initial replication status might take some time toorefresh.</span></span> <span data-ttu-id="118fd-165">Klicka på **uppdatera** tooget hello senaste status.</span><span class="sxs-lookup"><span data-stu-id="118fd-165">Click **Refresh** tooget hello latest status.</span></span>

2. <span data-ttu-id="118fd-166">Du kan följa förloppet för hello **Skyddsaktivering** jobb **inställningar** > **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="118fd-166">You can track progress of hello **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

3. <span data-ttu-id="118fd-167">I **inställningar** > **replikerade objekt**, kan du visa hello status för virtuella datorer och hello inledande replikering pågår.</span><span class="sxs-lookup"><span data-stu-id="118fd-167">In **Settings** > **Replicated Items**, you can view hello status of VMs and hello initial replication progress.</span></span> <span data-ttu-id="118fd-168">Klicka på hello VM toodrill ned till dess inställningar.</span><span class="sxs-lookup"><span data-stu-id="118fd-168">Click hello VM toodrill down into its settings.</span></span>



## <a name="next-steps"></a><span data-ttu-id="118fd-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="118fd-169">Next steps</span></span>

<span data-ttu-id="118fd-170">Gå för[steg 6: köra ett redundanstest](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="118fd-170">Go too[Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>
