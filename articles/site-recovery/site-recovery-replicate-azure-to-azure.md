---
title: aaaReplicate program (Azure tooAzure) | Microsoft Docs
description: "Den här artikeln beskriver hur tooset duplicering av virtuella datorer som körs i en Azure-region för en annan region i Azure."
services: site-recovery
documentationcenter: 
author: asgang
manager: rochakm
editor: 
ms.assetid: 
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 5/22/2017
ms.author: asgang
ms.openlocfilehash: fb190dac14419f892a1c6b45a3d991d8005e4bd0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-virtual-machines-tooanother-azure-region"></a><span data-ttu-id="5016f-103">Replikera virtuella datorer i Azure tooanother Azure-region</span><span class="sxs-lookup"><span data-stu-id="5016f-103">Replicate Azure virtual machines tooanother Azure region</span></span>



>[!NOTE]
>
> <span data-ttu-id="5016f-104">Site Recovery replikering för virtuella Azure-datorer är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="5016f-104">Site Recovery replication for Azure virtual machines is currently in preview.</span></span>

<span data-ttu-id="5016f-105">Den här artikeln beskriver hur tooset duplicering av virtuella datorer som körs i en Azure-region tooanother Azure-region.</span><span class="sxs-lookup"><span data-stu-id="5016f-105">This article describes how tooset up replication of virtual machines running in one Azure region tooanother Azure region.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5016f-106">Krav</span><span class="sxs-lookup"><span data-stu-id="5016f-106">Prerequisites</span></span>

* <span data-ttu-id="5016f-107">hello förutsätter att du redan vet om Site Recovery och Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="5016f-107">hello article assumes that you already know about Site Recovery and Recovery Services Vault.</span></span> <span data-ttu-id="5016f-108">Du måste toohave en Recovery services-ventilen pre skapas.</span><span class="sxs-lookup"><span data-stu-id="5016f-108">You need toohave a 'Recovery services vault' pre created.</span></span>

    >[!NOTE]
    >
    > <span data-ttu-id="5016f-109">Det rekommenderas att du skapar hello Recovery services-ventilen i hello plats där du vill att din tooreplicate för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5016f-109">It is recommended that you create hello 'Recovery services vault' in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="5016f-110">Till exempel om din målplats 'centrala USA, skapa valv i 'Centrala USA'.</span><span class="sxs-lookup"><span data-stu-id="5016f-110">For example, if your target location is 'Central US', create vault in 'Central US'.</span></span>

* <span data-ttu-id="5016f-111">Se till att du godkända hello begärda URL: er eller IP-adresser om du använder regler för Nätverkssäkerhetsgrupp grupper (NSG) eller brandväggen proxy toocontrol åtkomst toooutbound internet-anslutning på hello Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5016f-111">If you are using Network Security Groups (NSG) rules or firewall proxy toocontrol access toooutbound internet connectivity on hello Azure VMs, ensure that you whitelist hello required URLs or IPs.</span></span> <span data-ttu-id="5016f-112">Se för[nätverk riktlinjerna](./site-recovery-azure-to-azure-networking-guidance.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="5016f-112">Refer too[Networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md) for more details.</span></span>

* <span data-ttu-id="5016f-113">Om du har en ExpressRoute eller en VPN-anslutning mellan lokala och hello datakällplats i Azure, Följ [platsöverväganden för Azure tooon lokala ExpressRoute / VPN-konfiguration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="5016f-113">If you have an ExpressRoute or a VPN connection between on-premises and hello source location in Azure, follow [Site Recovery Considerations for Azure tooon-premises ExpressRoute / VPN configuration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) document.</span></span>

* <span data-ttu-id="5016f-114">Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="5016f-114">Your Azure user account needs toohave certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of an Azure virtual machine.</span></span>

* <span data-ttu-id="5016f-115">Din Azure-prenumeration ska vara aktiverade toocreate virtuella datorer i hello målplats du toouse som DR region.</span><span class="sxs-lookup"><span data-stu-id="5016f-115">Your Azure subscription should be enabled toocreate VMs in hello target location you want toouse as DR region.</span></span> <span data-ttu-id="5016f-116">Du kan kontakta support tooenable hello krävs kvoten.</span><span class="sxs-lookup"><span data-stu-id="5016f-116">You can contact support tooenable hello required quota.</span></span>

## <a name="enable-replication-from-azure-site-recovery-vault"></a><span data-ttu-id="5016f-117">Aktivera replikering från Azure Site Recovery-valvet</span><span class="sxs-lookup"><span data-stu-id="5016f-117">Enable replication from Azure Site Recovery vault</span></span>
<span data-ttu-id="5016f-118">Vi kommer replikera virtuella datorer som körs i Azure-plats toohello för hello östra Asien, Sydostasien, plats för den här bilden.</span><span class="sxs-lookup"><span data-stu-id="5016f-118">For this illustration, we will replicate VMs running  in hello ‘East Asia’ Azure location toohello ‘South East Asia’ location.</span></span> <span data-ttu-id="5016f-119">hello stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="5016f-119">hello steps are as follows:</span></span>

 <span data-ttu-id="5016f-120">Klicka på **+ replikera** i hello valvet tooenable replikering för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5016f-120">Click **+Replicate** in hello vault tooenable replication for hello virtual machines.</span></span>

1. <span data-ttu-id="5016f-121">**Källa:** refererar den ursprungliga hello datorer i det här fallet toohello punkt **Azure**.</span><span class="sxs-lookup"><span data-stu-id="5016f-121">**Source:** It refers toohello point of origin of hello machines which in this case is **Azure**.</span></span>

2. <span data-ttu-id="5016f-122">**Datakällplats:** är det hello Azure-region där du vill att tooprotect dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5016f-122">**Source location:** It is hello Azure region from where you want tooprotect your virtual machines.</span></span> <span data-ttu-id="5016f-123">Den här bilden ska hello källplats östra Asien</span><span class="sxs-lookup"><span data-stu-id="5016f-123">For this illustration, hello source location will be 'East Asia'</span></span>

3. <span data-ttu-id="5016f-124">**Distributionsmodell:** refererar toohello Azure distributionsmodell för hello källdatorer.</span><span class="sxs-lookup"><span data-stu-id="5016f-124">**Deployment model:** It refers toohello Azure deployment model of hello source machines.</span></span> <span data-ttu-id="5016f-125">Du kan välja antingen klassiska eller resource manager och datorer som tillhör toohello viss modell visas för skydd i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="5016f-125">You can select either classic or resource manager and machines belonging toohello specific model will be listed for protection in hello next step.</span></span>

      >[!NOTE]
      >
      > <span data-ttu-id="5016f-126">Du kan endast replikera en klassisk virtuell dator och återställa den som en klassisk virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5016f-126">You can only replicate a classic virtual machine and recover it as a classic virtual machine.</span></span> <span data-ttu-id="5016f-127">Du kan inte återställa den som en virtuell dator i Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="5016f-127">You cannot recover it as a Resource Manager virtual machine.</span></span>

4. <span data-ttu-id="5016f-128">**Resursgrupp:** det är hello resurs grupp toowhich dina virtuella källdatorer tillhör.</span><span class="sxs-lookup"><span data-stu-id="5016f-128">**Resource Group:** It’s hello resource group toowhich your source virtual machines belong.</span></span> <span data-ttu-id="5016f-129">Alla hello virtuella datorer under hello valda resursgruppen visas för skydd i hello nästa steg.</span><span class="sxs-lookup"><span data-stu-id="5016f-129">All hello VMs under hello selected resource group will be listed for protection in hello next step.</span></span>

    ![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

<span data-ttu-id="5016f-131">I **virtuella datorer > Välj virtuella datorer**och på varje dator som du vill tooreplicate.</span><span class="sxs-lookup"><span data-stu-id="5016f-131">In **Virtual Machines > Select virtual machines**, click and select each machine you want tooreplicate.</span></span> <span data-ttu-id="5016f-132">Du kan bara välja datorer som stöder replikering.</span><span class="sxs-lookup"><span data-stu-id="5016f-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="5016f-133">Klicka sedan på OK.</span><span class="sxs-lookup"><span data-stu-id="5016f-133">Then click OK.</span></span>
    <span data-ttu-id="5016f-134">![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span><span class="sxs-lookup"><span data-stu-id="5016f-134">![Enable replication](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span></span>


<span data-ttu-id="5016f-135">Under inställningar kan du konfigurera egenskaper för mål</span><span class="sxs-lookup"><span data-stu-id="5016f-135">Under Settings section you can configure target site properties</span></span>

1. <span data-ttu-id="5016f-136">**Målplatsen:** det här är hello plats där källdata virtuella datorn kommer att replikeras.</span><span class="sxs-lookup"><span data-stu-id="5016f-136">**Target Location:**  This is hello location where your source virtual machine data will be replicated.</span></span> <span data-ttu-id="5016f-137">Beroende på din plats för valda datorer, tillhandahåller Site Recovery du hello lista över lämpliga målregioner.</span><span class="sxs-lookup"><span data-stu-id="5016f-137">Depending upon your selected machines location, Site Recovery will provide you hello list of suitable target regions.</span></span>

    > [!TIP]
    > <span data-ttu-id="5016f-138">Det rekommenderas tookeep målplats samma från och med din recovery services-valvet.</span><span class="sxs-lookup"><span data-stu-id="5016f-138">It is recommended tookeep target location same as of your recovery services vault.</span></span>

2. <span data-ttu-id="5016f-139">**Målresursgruppen:** är det hello resurs grupp toowhich alla replikerade virtuella datorer ska tillhöra. Som standard skapar ASR en ny resursgrupp i hello målregionen med namn med suffixet ”asr”.</span><span class="sxs-lookup"><span data-stu-id="5016f-139">**Target resource group :** It is hello resource group toowhich all your replicated virtual machines will belong.By default ASR will create a new resource group in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="5016f-140">Om resursgruppen som skapats av ASR redan finns, den av återanvändas. Du kan också välja toocustomize den enligt hello nedan.</span><span class="sxs-lookup"><span data-stu-id="5016f-140">In case resource group created by ASR already exist, it will be reused.You can also choose toocustomize it as shown in hello section below.</span></span>    
3. <span data-ttu-id="5016f-141">**Mål för virtuella nätverk:** som standard ASR skapas ett nytt virtuellt nätverk i hello målregionen med namn med suffixet ”asr”.</span><span class="sxs-lookup"><span data-stu-id="5016f-141">**Target Virtual Network:** By default, ASR will create a new virtual network in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="5016f-142">Detta kommer att vara mappade tooyour källnätverket och kommer att användas för alla framtida skydd.</span><span class="sxs-lookup"><span data-stu-id="5016f-142">This will be mapped tooyour source network and will be used for any future protection.</span></span>

    > [!NOTE]
    > <span data-ttu-id="5016f-143">[Nätverk klientkontrollen](site-recovery-network-mapping-azure-to-azure.md) tooknow mer om nätverksmappning.</span><span class="sxs-lookup"><span data-stu-id="5016f-143">[Check networking details](site-recovery-network-mapping-azure-to-azure.md) tooknow more about network mapping.</span></span>

4. <span data-ttu-id="5016f-144">**Rikta Storage-konton:** skapar som standard ASR hello nya mål-lagringskontot frihandsbilden lagringskonfigurationen källa VM.</span><span class="sxs-lookup"><span data-stu-id="5016f-144">**Target Storage accounts:** By default, ASR will create hello new target storage account mimicking your source VM storage configuration.</span></span> <span data-ttu-id="5016f-145">Om lagringskonto som skapats av ASR redan finns, den av återanvändas.</span><span class="sxs-lookup"><span data-stu-id="5016f-145">In case storage account created by ASR already exist, it will be reused.</span></span>

5. <span data-ttu-id="5016f-146">**Cachelagra Storage-konton:** ASR måste extra lagring som kallas konto för cachelagring i hello källa region.</span><span class="sxs-lookup"><span data-stu-id="5016f-146">**Cache Storage accounts:** ASR needs extra storage account called cache storage in hello source region.</span></span> <span data-ttu-id="5016f-147">Alla hello ändringar som sker på hello virtuella källdatorer spåras och skickas toocache storage-konto innan du replikerar de toohello målplatsen.</span><span class="sxs-lookup"><span data-stu-id="5016f-147">All hello changes happening on hello source VMs are tracked and sent toocache storage account before replicating those toohello target location.</span></span>

6. <span data-ttu-id="5016f-148">**Tillgänglighetsuppsättningen:** som standard ASR skapar en ny tillgänglighetsuppsättning i hello målregionen med namn med suffixet ”asr”.</span><span class="sxs-lookup"><span data-stu-id="5016f-148">**Availability set :** By default, ASR will create a new availability set in hello target region with name having "asr" suffix.</span></span> <span data-ttu-id="5016f-149">Om tillgänglighet som skapats av ASR redan finns kommer den av återanvändas.</span><span class="sxs-lookup"><span data-stu-id="5016f-149">In case availability set created by ASR already exist, it will be reused.</span></span>

7.  <span data-ttu-id="5016f-150">**Replikeringsprincip:** definierar hello inställningar för återställning punkt kvarhållning historik och app programkonsekvent ögonblicksbild frekvens.</span><span class="sxs-lookup"><span data-stu-id="5016f-150">**Replication Policy:** It defines hello settings for recovery point retention history and app consistent snapshot frequency.</span></span> <span data-ttu-id="5016f-151">Som standard skapas ASR en ny replikeringsprincip med standardinställningarna för ”24 timmars för kvarhållningstid för återställningspunkten och” 60 minuters för app programkonsekvent ögonblicksbild frekvens.</span><span class="sxs-lookup"><span data-stu-id="5016f-151">By default, ASR will create a new replication policy with default settings of ‘24 hours’ for recovery point retention and ’60 minutes’ for app consistent snapshot frequency.</span></span>

    ![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a><span data-ttu-id="5016f-153">Anpassa target-resurser</span><span class="sxs-lookup"><span data-stu-id="5016f-153">Customize target resources</span></span>

<span data-ttu-id="5016f-154">Om du vill toochange hello standardvärden som används av ASR kan du ändra hello inställningar baserat på dina behov.</span><span class="sxs-lookup"><span data-stu-id="5016f-154">In case you want toochange hello defaults used by ASR, you can change hello settings based on your needs.</span></span>

1. <span data-ttu-id="5016f-155">**Anpassa:** klickar du på den toochange hello standardvärden som används av ASR.</span><span class="sxs-lookup"><span data-stu-id="5016f-155">**Customize:** Click it toochange hello defaults used by ASR.</span></span>

2. <span data-ttu-id="5016f-156">**Målresursgruppen:** du kan välja hello resursgrupp hello listan över alla resursgrupper för hello befintliga hello målplatsen inom hello prenumeration.</span><span class="sxs-lookup"><span data-stu-id="5016f-156">**Target resource group :**  You can select hello resource group from hello list of all hello resource groups existing in hello target location within hello subscription.</span></span>

3. <span data-ttu-id="5016f-157">**Mål för virtuella nätverk:** du hittar hello lista över alla hello virtuellt nätverk i hello målplats.</span><span class="sxs-lookup"><span data-stu-id="5016f-157">**Target Virtual Network:** You can find hello list of all hello virtual network in hello target location.</span></span>

4. <span data-ttu-id="5016f-158">**Tillgänglighetsuppsättningen:** du kan bara lägga till tillgänglighet anger inställningar toohello virtuella datorer som ingår i tillgänglighet i källan region.</span><span class="sxs-lookup"><span data-stu-id="5016f-158">**Availability set :** You can only add availability sets settings toohello virtual machines which are a part of availability in source region.</span></span>

5. <span data-ttu-id="5016f-159">**Mål Storage-konton:**</span><span class="sxs-lookup"><span data-stu-id="5016f-159">**Target Storage accounts:**</span></span>

<span data-ttu-id="5016f-160">![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klickar du på **skapa målresurs** och aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="5016f-160">![Enable replication](./media/site-recovery-replicate-azure-to-azure/customize.PNG) Click on **Create target resource** and Enable Replication</span></span>


<span data-ttu-id="5016f-161">När virtuella datorer är skyddade kan du hello status för virtuella datorer hälsa under **replikerade objekt**</span><span class="sxs-lookup"><span data-stu-id="5016f-161">Once virtual machines are protected you can check hello status of VMs health under **Replicated items**</span></span>

>[!NOTE]
><span data-ttu-id="5016f-162">Under hello kunde tid för inledande replikering det en risk för att status tar tid toorefresh och du inte kan se förloppet under en viss tid.</span><span class="sxs-lookup"><span data-stu-id="5016f-162">During hello time of initial replication there could a possibility that status takes time toorefresh and you don't see progress for some time.</span></span> <span data-ttu-id="5016f-163">Du kan klicka hello uppdatera ovanpå hello hello bladet tooget hello senaste status.</span><span class="sxs-lookup"><span data-stu-id="5016f-163">You can click hello Refresh button on hello top of hello blade tooget hello latest status.</span></span>
>

![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a><span data-ttu-id="5016f-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="5016f-165">Next steps</span></span>
- <span data-ttu-id="5016f-166">[Lär dig mer](site-recovery-test-failover-to-azure.md) om att köra ett redundanstest.</span><span class="sxs-lookup"><span data-stu-id="5016f-166">[Learn more](site-recovery-test-failover-to-azure.md) about running a test failover.</span></span>
- <span data-ttu-id="5016f-167">[Lär dig mer](site-recovery-failover.md) om olika typer av växling vid fel, och hur toorun dem.</span><span class="sxs-lookup"><span data-stu-id="5016f-167">[Learn more](site-recovery-failover.md) about different types of failovers, and how toorun them.</span></span>
- <span data-ttu-id="5016f-168">Lär dig mer om [med återställningsplaner](site-recovery-create-recovery-plans.md) tooreduce Återställningstidsmål.</span><span class="sxs-lookup"><span data-stu-id="5016f-168">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) tooreduce RTO.</span></span>
- <span data-ttu-id="5016f-169">Lär dig mer om [skydda virtuella datorer i Azure](site-recovery-how-to-reprotect.md) efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="5016f-169">Learn more about [reprotecting Azure  VMs](site-recovery-how-to-reprotect.md) after failover.</span></span>
