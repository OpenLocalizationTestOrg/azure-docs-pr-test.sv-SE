---
title: Duplicera program (Azure Azure) | Microsoft Docs
description: "Den här artikeln beskriver hur du ställer in replikering av virtuella datorer som körs i en Azure-region till en annan region i Azure."
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
ms.openlocfilehash: f9f97cf840b722c8cfee169dd1640e0682f287ff
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-azure-virtual-machines-to-another-azure-region"></a><span data-ttu-id="16fbf-103">Replikera virtuella Azure-datorer till en annan Azure-region</span><span class="sxs-lookup"><span data-stu-id="16fbf-103">Replicate Azure virtual machines to another Azure region</span></span>



>[!NOTE]
>
> <span data-ttu-id="16fbf-104">Site Recovery replikering för virtuella Azure-datorer är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="16fbf-104">Site Recovery replication for Azure virtual machines is currently in preview.</span></span>

<span data-ttu-id="16fbf-105">Den här artikeln beskriver hur du ställer in replikering av virtuella datorer som körs i en Azure-region till en annan Azure-region.</span><span class="sxs-lookup"><span data-stu-id="16fbf-105">This article describes how to set up replication of virtual machines running in one Azure region to another Azure region.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="16fbf-106">Krav</span><span class="sxs-lookup"><span data-stu-id="16fbf-106">Prerequisites</span></span>

* <span data-ttu-id="16fbf-107">Artikeln förutsätter att du redan vet om Site Recovery och Recovery Services-valvet.</span><span class="sxs-lookup"><span data-stu-id="16fbf-107">The article assumes that you already know about Site Recovery and Recovery Services Vault.</span></span> <span data-ttu-id="16fbf-108">Du måste ha en Recovery services-ventilen pre skapas.</span><span class="sxs-lookup"><span data-stu-id="16fbf-108">You need to have a 'Recovery services vault' pre created.</span></span>

    >[!NOTE]
    >
    > <span data-ttu-id="16fbf-109">Det rekommenderas att du skapar den 'återställningstjänstvalvet' på den plats där du vill att dina virtuella datorer för replikering.</span><span class="sxs-lookup"><span data-stu-id="16fbf-109">It is recommended that you create the 'Recovery services vault' in the location where you want your VMs to replicate.</span></span> <span data-ttu-id="16fbf-110">Till exempel om din målplats 'centrala USA, skapa valv i 'Centrala USA'.</span><span class="sxs-lookup"><span data-stu-id="16fbf-110">For example, if your target location is 'Central US', create vault in 'Central US'.</span></span>

* <span data-ttu-id="16fbf-111">Om du använder Nätverkssäkerhetsgrupp grupper (NSG) regler eller brandväggen proxy för att styra åtkomsten till utgående Internetanslutning på Azure Virtual Machines, se till att du godkända obligatorisk URL: er eller IP-adresser.</span><span class="sxs-lookup"><span data-stu-id="16fbf-111">If you are using Network Security Groups (NSG) rules or firewall proxy to control access to outbound internet connectivity on the Azure VMs, ensure that you whitelist the required URLs or IPs.</span></span> <span data-ttu-id="16fbf-112">Referera till [nätverk riktlinjerna](./site-recovery-azure-to-azure-networking-guidance.md) för mer information.</span><span class="sxs-lookup"><span data-stu-id="16fbf-112">Refer to [Networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md) for more details.</span></span>

* <span data-ttu-id="16fbf-113">Om du har en ExpressRoute eller en VPN-anslutning mellan lokala och källplats i Azure följer [platsöverväganden för Azure till lokala ExpressRoute / VPN-konfiguration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) dokumentet.</span><span class="sxs-lookup"><span data-stu-id="16fbf-113">If you have an ExpressRoute or a VPN connection between on-premises and the source location in Azure, follow [Site Recovery Considerations for Azure to on-premises ExpressRoute / VPN configuration](site-recovery-azure-to-azure-networking-guidance.md#guidelines-for-existing-azure-to-on-premises-expressroutevpn-configuration) document.</span></span>

* <span data-ttu-id="16fbf-114">Ditt Azure-konto måste ha vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) att aktivera replikering för en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="16fbf-114">Your Azure user account needs to have certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) to enable replication of an Azure virtual machine.</span></span>

* <span data-ttu-id="16fbf-115">Din Azure-prenumeration måste vara aktiverat för att skapa virtuella datorer i den målplats som du vill använda som DR region.</span><span class="sxs-lookup"><span data-stu-id="16fbf-115">Your Azure subscription should be enabled to create VMs in the target location you want to use as DR region.</span></span> <span data-ttu-id="16fbf-116">Du kan kontakta support om du vill aktivera kvoten som krävs.</span><span class="sxs-lookup"><span data-stu-id="16fbf-116">You can contact support to enable the required quota.</span></span>

## <a name="enable-replication-from-azure-site-recovery-vault"></a><span data-ttu-id="16fbf-117">Aktivera replikering från Azure Site Recovery-valvet</span><span class="sxs-lookup"><span data-stu-id="16fbf-117">Enable replication from Azure Site Recovery vault</span></span>
<span data-ttu-id="16fbf-118">För den här bilden kommer vi replikera virtuella datorer som körs i östra Asien Azure installationsplatsen på Syd Östasien.</span><span class="sxs-lookup"><span data-stu-id="16fbf-118">For this illustration, we will replicate VMs running  in the ‘East Asia’ Azure location to the ‘South East Asia’ location.</span></span> <span data-ttu-id="16fbf-119">Stegen är följande:</span><span class="sxs-lookup"><span data-stu-id="16fbf-119">The steps are as follows:</span></span>

 <span data-ttu-id="16fbf-120">Klicka på **+ replikera** i valvet för att aktivera replikering för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="16fbf-120">Click **+Replicate** in the vault to enable replication for the virtual machines.</span></span>

1. <span data-ttu-id="16fbf-121">**Källa:** det refererar till referenspunkt för datorer i det här fallet **Azure**.</span><span class="sxs-lookup"><span data-stu-id="16fbf-121">**Source:** It refers to the point of origin of the machines which in this case is **Azure**.</span></span>

2. <span data-ttu-id="16fbf-122">**Datakällplats:** är det Azure-region där du vill skydda dina virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="16fbf-122">**Source location:** It is the Azure region from where you want to protect your virtual machines.</span></span> <span data-ttu-id="16fbf-123">Den här bilden ska källplatsen östra Asien</span><span class="sxs-lookup"><span data-stu-id="16fbf-123">For this illustration, the source location will be 'East Asia'</span></span>

3. <span data-ttu-id="16fbf-124">**Distributionsmodell:** refererar den till Azure distributionsmodell källdatorer.</span><span class="sxs-lookup"><span data-stu-id="16fbf-124">**Deployment model:** It refers to the Azure deployment model of the source machines.</span></span> <span data-ttu-id="16fbf-125">Du kan välja antingen klassiska eller resource manager och datorer som tillhör en viss modell visas för skydd i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="16fbf-125">You can select either classic or resource manager and machines belonging to the specific model will be listed for protection in the next step.</span></span>

      >[!NOTE]
      >
      > <span data-ttu-id="16fbf-126">Du kan endast replikera en klassisk virtuell dator och återställa den som en klassisk virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="16fbf-126">You can only replicate a classic virtual machine and recover it as a classic virtual machine.</span></span> <span data-ttu-id="16fbf-127">Du kan inte återställa den som en virtuell dator i Resource Manager.</span><span class="sxs-lookup"><span data-stu-id="16fbf-127">You cannot recover it as a Resource Manager virtual machine.</span></span>

4. <span data-ttu-id="16fbf-128">**Resursgrupp:** det är den resursgrupp som din virtuella källdatorer tillhör.</span><span class="sxs-lookup"><span data-stu-id="16fbf-128">**Resource Group:** It’s the resource group to which your source virtual machines belong.</span></span> <span data-ttu-id="16fbf-129">Alla virtuella datorer under den valda resursgruppen visas för skydd i nästa steg.</span><span class="sxs-lookup"><span data-stu-id="16fbf-129">All the VMs under the selected resource group will be listed for protection in the next step.</span></span>

    ![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/enabledrwizard1.png)

<span data-ttu-id="16fbf-131">I **virtuella datorer > Välj virtuella datorer**och på varje dator som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="16fbf-131">In **Virtual Machines > Select virtual machines**, click and select each machine you want to replicate.</span></span> <span data-ttu-id="16fbf-132">Du kan bara välja datorer som stöder replikering.</span><span class="sxs-lookup"><span data-stu-id="16fbf-132">You can only select machines for which replication can be enabled.</span></span> <span data-ttu-id="16fbf-133">Klicka sedan på OK.</span><span class="sxs-lookup"><span data-stu-id="16fbf-133">Then click OK.</span></span>
    <span data-ttu-id="16fbf-134">![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span><span class="sxs-lookup"><span data-stu-id="16fbf-134">![Enable replication](./media/site-recovery-replicate-azure-to-azure/virtualmachine_selection.png)</span></span>


<span data-ttu-id="16fbf-135">Under inställningar kan du konfigurera egenskaper för mål</span><span class="sxs-lookup"><span data-stu-id="16fbf-135">Under Settings section you can configure target site properties</span></span>

1. <span data-ttu-id="16fbf-136">**Målplatsen:** detta är den plats där källdata virtuella datorn kommer att replikeras.</span><span class="sxs-lookup"><span data-stu-id="16fbf-136">**Target Location:**  This is the location where your source virtual machine data will be replicated.</span></span> <span data-ttu-id="16fbf-137">Beroende på din plats för valda datorer, tillhandahåller Site Recovery listan över målregioner lämplig.</span><span class="sxs-lookup"><span data-stu-id="16fbf-137">Depending upon your selected machines location, Site Recovery will provide you the list of suitable target regions.</span></span>

    > [!TIP]
    > <span data-ttu-id="16fbf-138">Det rekommenderas att hålla målplats samma från och med din recovery services-valvet.</span><span class="sxs-lookup"><span data-stu-id="16fbf-138">It is recommended to keep target location same as of your recovery services vault.</span></span>

2. <span data-ttu-id="16fbf-139">**Målresursgruppen:** är det resursgruppen där alla de replikerade virtuella datorerna ska tillhöra. Som standard skapar ASR en ny resursgrupp i området mål med namn med suffixet ”asr”.</span><span class="sxs-lookup"><span data-stu-id="16fbf-139">**Target resource group :** It is the resource group to which all your replicated virtual machines will belong.By default ASR will create a new resource group in the target region with name having "asr" suffix.</span></span> <span data-ttu-id="16fbf-140">Om resursgruppen som skapats av ASR redan finns, den av återanvändas. Du kan också välja att anpassa den som visas i följande avsnitt.</span><span class="sxs-lookup"><span data-stu-id="16fbf-140">In case resource group created by ASR already exist, it will be reused.You can also choose to customize it as shown in the section below.</span></span>    
3. <span data-ttu-id="16fbf-141">**Mål för virtuella nätverk:** som standard ASR skapar ett nytt virtuellt nätverk i målregionen med namn med suffixet ”asr”.</span><span class="sxs-lookup"><span data-stu-id="16fbf-141">**Target Virtual Network:** By default, ASR will create a new virtual network in the target region with name having "asr" suffix.</span></span> <span data-ttu-id="16fbf-142">Detta kommer att mappas till nätverket källa och kommer att användas för alla framtida skydd.</span><span class="sxs-lookup"><span data-stu-id="16fbf-142">This will be mapped to your source network and will be used for any future protection.</span></span>

    > [!NOTE]
    > <span data-ttu-id="16fbf-143">[Nätverk klientkontrollen](site-recovery-network-mapping-azure-to-azure.md) vill veta mer om nätverksmappning.</span><span class="sxs-lookup"><span data-stu-id="16fbf-143">[Check networking details](site-recovery-network-mapping-azure-to-azure.md) to know more about network mapping.</span></span>

4. <span data-ttu-id="16fbf-144">**Rikta Storage-konton:** som standard ASR skapar nya mål-lagringskontot frihandsbilden lagringskonfigurationen källa VM.</span><span class="sxs-lookup"><span data-stu-id="16fbf-144">**Target Storage accounts:** By default, ASR will create the new target storage account mimicking your source VM storage configuration.</span></span> <span data-ttu-id="16fbf-145">Om lagringskonto som skapats av ASR redan finns, den av återanvändas.</span><span class="sxs-lookup"><span data-stu-id="16fbf-145">In case storage account created by ASR already exist, it will be reused.</span></span>

5. <span data-ttu-id="16fbf-146">**Cachelagra Storage-konton:** ASR måste extra lagring som kallas konto för cachelagring i området för källa.</span><span class="sxs-lookup"><span data-stu-id="16fbf-146">**Cache Storage accounts:** ASR needs extra storage account called cache storage in the source region.</span></span> <span data-ttu-id="16fbf-147">Alla ändringar som sker på virtuella källdatorer spåras och skickas till cachen storage-konto innan du replikerar de till målplatsen.</span><span class="sxs-lookup"><span data-stu-id="16fbf-147">All the changes happening on the source VMs are tracked and sent to cache storage account before replicating those to the target location.</span></span>

6. <span data-ttu-id="16fbf-148">**Tillgänglighetsuppsättningen:** som standard ASR skapar en ny tillgänglighetsuppsättning i området mål med namn med suffixet ”asr”.</span><span class="sxs-lookup"><span data-stu-id="16fbf-148">**Availability set :** By default, ASR will create a new availability set in the target region with name having "asr" suffix.</span></span> <span data-ttu-id="16fbf-149">Om tillgänglighet som skapats av ASR redan finns kommer den av återanvändas.</span><span class="sxs-lookup"><span data-stu-id="16fbf-149">In case availability set created by ASR already exist, it will be reused.</span></span>

7.  <span data-ttu-id="16fbf-150">**Replikeringsprincip:** definierar inställningar för återställning punkt kvarhållning historik och app programkonsekvent ögonblicksbild frekvens.</span><span class="sxs-lookup"><span data-stu-id="16fbf-150">**Replication Policy:** It defines the settings for recovery point retention history and app consistent snapshot frequency.</span></span> <span data-ttu-id="16fbf-151">Som standard skapas ASR en ny replikeringsprincip med standardinställningarna för ”24 timmars för kvarhållningstid för återställningspunkten och” 60 minuters för app programkonsekvent ögonblicksbild frekvens.</span><span class="sxs-lookup"><span data-stu-id="16fbf-151">By default, ASR will create a new replication policy with default settings of ‘24 hours’ for recovery point retention and ’60 minutes’ for app consistent snapshot frequency.</span></span>

    ![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/enabledrwizard3.PNG)

## <a name="customize-target-resources"></a><span data-ttu-id="16fbf-153">Anpassa target-resurser</span><span class="sxs-lookup"><span data-stu-id="16fbf-153">Customize target resources</span></span>

<span data-ttu-id="16fbf-154">Om du vill ändra standardinställningarna som används av ASR kan du ändra inställningarna utifrån dina behov.</span><span class="sxs-lookup"><span data-stu-id="16fbf-154">In case you want to change the defaults used by ASR, you can change the settings based on your needs.</span></span>

1. <span data-ttu-id="16fbf-155">**Anpassa:** Klicka om du vill ändra standardinställningarna som används av ASR.</span><span class="sxs-lookup"><span data-stu-id="16fbf-155">**Customize:** Click it to change the defaults used by ASR.</span></span>

2. <span data-ttu-id="16fbf-156">**Målresursgruppen:** du kan välja resursgruppen från listan över alla resursgrupper finns i målplatsen i prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="16fbf-156">**Target resource group :**  You can select the resource group from the list of all the resource groups existing in the target location within the subscription.</span></span>

3. <span data-ttu-id="16fbf-157">**Mål för virtuella nätverk:** du hittar listan över det virtuella nätverket på målplatsen.</span><span class="sxs-lookup"><span data-stu-id="16fbf-157">**Target Virtual Network:** You can find the list of all the virtual network in the target location.</span></span>

4. <span data-ttu-id="16fbf-158">**Tillgänglighetsuppsättningen:** anger tillgänglighetsinställningarna kan endast läggas till de virtuella datorerna som är en del av tillgänglighet i källan region.</span><span class="sxs-lookup"><span data-stu-id="16fbf-158">**Availability set :** You can only add availability sets settings to the virtual machines which are a part of availability in source region.</span></span>

5. <span data-ttu-id="16fbf-159">**Mål Storage-konton:**</span><span class="sxs-lookup"><span data-stu-id="16fbf-159">**Target Storage accounts:**</span></span>

<span data-ttu-id="16fbf-160">![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/customize.PNG) klickar du på **skapa målresurs** och aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="16fbf-160">![Enable replication](./media/site-recovery-replicate-azure-to-azure/customize.PNG) Click on **Create target resource** and Enable Replication</span></span>


<span data-ttu-id="16fbf-161">När virtuella datorer är skyddade du kan kontrollera status för virtuella datorer hälsa under **replikerade objekt**</span><span class="sxs-lookup"><span data-stu-id="16fbf-161">Once virtual machines are protected you can check the status of VMs health under **Replicated items**</span></span>

>[!NOTE]
><span data-ttu-id="16fbf-162">Under tid då det inledande replikering kan en risk för att det tar tid att uppdatera status och du inte kan se förloppet under en viss tid.</span><span class="sxs-lookup"><span data-stu-id="16fbf-162">During the time of initial replication there could a possibility that status takes time to refresh and you don't see progress for some time.</span></span> <span data-ttu-id="16fbf-163">Du kan klicka på uppdateringsknappen överst på bladet för att hämta senaste status.</span><span class="sxs-lookup"><span data-stu-id="16fbf-163">You can click the Refresh button on the top of the blade to get the latest status.</span></span>
>

![Aktivera replikering](./media/site-recovery-replicate-azure-to-azure/replicateditems.PNG)


## <a name="next-steps"></a><span data-ttu-id="16fbf-165">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="16fbf-165">Next steps</span></span>
- <span data-ttu-id="16fbf-166">[Lär dig mer](site-recovery-test-failover-to-azure.md) om att köra ett redundanstest.</span><span class="sxs-lookup"><span data-stu-id="16fbf-166">[Learn more](site-recovery-test-failover-to-azure.md) about running a test failover.</span></span>
- <span data-ttu-id="16fbf-167">[Lär dig mer](site-recovery-failover.md) om olika typer av redundansväxlingar och hur du kör dem.</span><span class="sxs-lookup"><span data-stu-id="16fbf-167">[Learn more](site-recovery-failover.md) about different types of failovers, and how to run them.</span></span>
- <span data-ttu-id="16fbf-168">Lär dig mer om [med återställningsplaner](site-recovery-create-recovery-plans.md) att minska Återställningstidsmål.</span><span class="sxs-lookup"><span data-stu-id="16fbf-168">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) to reduce RTO.</span></span>
- <span data-ttu-id="16fbf-169">Lär dig mer om [skydda virtuella datorer i Azure](site-recovery-how-to-reprotect.md) efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="16fbf-169">Learn more about [reprotecting Azure  VMs](site-recovery-how-to-reprotect.md) after failover.</span></span>
