---
title: "Replikera virtuella Azure-datorer mellan Azure-regioner för katastrofåterställningskrav: Azure tooAzure | Microsoft Docs"
description: "Sammanfattar hello stegen tooreplicate Azure virtuella datorer mellan Azure-regioner (Azure till Azure) med hello Azure Site Recovery-tjänsten för disaster recovery behov."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: carmon
editor: 
ms.assetid: dab98aa5-9c41-4475-b7dc-2e07ab1cfd18
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/10/2017
ms.author: raynew
ms.openlocfilehash: c4c425af260a9bcc3dd4dcc13da26109e20f03bb
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-azure-vms-between-regions-with-azure-site-recovery"></a><span data-ttu-id="3d796-103">Replikera virtuella Azure-datorer mellan regioner med Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="3d796-103">Replicate Azure VMs between regions with Azure Site Recovery</span></span>

>[!NOTE]
>
> <span data-ttu-id="3d796-104">Azure Site Recovery replikering för virtuella Azure-datorer (VM) är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="3d796-104">Azure Site Recovery replication for Azure virtual machines (VMs) is currently in preview.</span></span>

<span data-ttu-id="3d796-105">Den här artikeln beskriver hur tooreplicate Azure virtuella datorer mellan Azure-regioner med hjälp av hello [Site Recovery](site-recovery-overview.md) tjänsten i hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="3d796-105">This article describes how tooreplicate Azure VMs between Azure regions by using hello [Site Recovery](site-recovery-overview.md) service in hello Azure portal.</span></span>

<span data-ttu-id="3d796-106">Publicera kommentarer och frågor längst ned hello av den här artikeln eller på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="3d796-106">Post comments and questions at hello bottom of this article or on hello [Azure Recovery Services forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="disaster-recovery-in-azure"></a><span data-ttu-id="3d796-107">Katastrofåterställning i Azure</span><span class="sxs-lookup"><span data-stu-id="3d796-107">Disaster recovery in Azure</span></span>

<span data-ttu-id="3d796-108">Inbyggda Azure-infrastrukturen-funktioner bidrar tooa robust och flexibel tillgängligheten för arbetsbelastningar som körs på virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="3d796-108">Built-in Azure infrastructure capabilities and features contribute tooa robust and resilient availability strategy for workloads that run on Azure VMs.</span></span> <span data-ttu-id="3d796-109">Det finns många orsaker till varför du behöver tooplan för katastrofåterställning mellan Azure-regioner själv:</span><span class="sxs-lookup"><span data-stu-id="3d796-109">However, there are many reasons why you need tooplan for disaster recovery between Azure regions yourself:</span></span>

- <span data-ttu-id="3d796-110">Du behöver toomeet efterlevnadsriktlinjer för specifika appar och arbetsbelastningar som kräver en affärskontinuitet och haveriberedskap (BCDR).</span><span class="sxs-lookup"><span data-stu-id="3d796-110">You need toomeet compliance guidelines for specific apps and workloads that require a business continuity and disaster recovery (BCDR) strategy.</span></span>
- <span data-ttu-id="3d796-111">Du vill hello möjlighet tooprotect och återställa virtuella Azure-datorer baserat på din affärsbeslut och inte bara baserat på inbyggda funktioner för Azure.</span><span class="sxs-lookup"><span data-stu-id="3d796-111">You want hello ability tooprotect and recover Azure VMs based on your business decisions, and not only based on inbuilt Azure functionality.</span></span>
- <span data-ttu-id="3d796-112">Du behöver tootest redundans och återställning enligt dina behov som företag och efterlevnad, utan inverkan på produktion.</span><span class="sxs-lookup"><span data-stu-id="3d796-112">You need tootest failover and recovery in accordance with your business and compliance needs, with no impact on production.</span></span>
- <span data-ttu-id="3d796-113">Du behöver toofail över toohello recovery region i en katastrof hello-händelse och växla tillbaka toohello ursprungliga källan region sömlöst.</span><span class="sxs-lookup"><span data-stu-id="3d796-113">You need toofail over toohello recovery region in hello event of a disaster and fail back toohello original source region seamlessly.</span></span>

<span data-ttu-id="3d796-114">Använda Site Recovery för Virtuella Azure-Azure replikering toohelp du gör dessa uppgifter.</span><span class="sxs-lookup"><span data-stu-id="3d796-114">Use Site Recovery for Azure-to-Azure VM replication toohelp you do all these tasks.</span></span>


## <a name="why-use-site-recovery"></a><span data-ttu-id="3d796-115">Varför ska jag använda Site Recovery?</span><span class="sxs-lookup"><span data-stu-id="3d796-115">Why use Site Recovery?</span></span>      

<span data-ttu-id="3d796-116">Site Recovery tillhandahåller ett enkelt sätt tooreplicate Azure virtuella datorer mellan regioner:</span><span class="sxs-lookup"><span data-stu-id="3d796-116">Site Recovery provides a simple way tooreplicate Azure VMs between regions:</span></span>

- <span data-ttu-id="3d796-117">**Automatisk distribution**.</span><span class="sxs-lookup"><span data-stu-id="3d796-117">**Automatic deployment**.</span></span> <span data-ttu-id="3d796-118">Till skillnad från en aktiv-aktiv replikeringsmodell behövs det ingen för ett dyrt och komplexa infrastrukturen i hello sekundär region.</span><span class="sxs-lookup"><span data-stu-id="3d796-118">Unlike an active-active replication model, there's no need for an expensive and complex infrastructure in hello secondary region.</span></span> <span data-ttu-id="3d796-119">När du aktiverar replikering skapar Site Recovery automatiskt hello krävs resurser i hello målregionen baserat på inställningarna för datakälla region.</span><span class="sxs-lookup"><span data-stu-id="3d796-119">When you enable replication, Site Recovery automatically creates hello required resources in hello target region, based on source region settings.</span></span>
- <span data-ttu-id="3d796-120">**Kontrollera regioner**.</span><span class="sxs-lookup"><span data-stu-id="3d796-120">**Control regions**.</span></span> <span data-ttu-id="3d796-121">Du kan replikera från en region tooany region inom en kontinent med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3d796-121">With Site Recovery, you can replicate from any region tooany region within a continent.</span></span> <span data-ttu-id="3d796-122">Jämför detta med läsbehörighet geo-redundant lagring, som replikerar asynkront mellan standard [länkas regioner](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) endast.</span><span class="sxs-lookup"><span data-stu-id="3d796-122">Compare this with read-access geo-redundant storage, which replicates asynchronously between standard [paired regions](https://docs.microsoft.com/azure/best-practices-availability-paired-regions) only.</span></span> <span data-ttu-id="3d796-123">Geo-redundant lagring med läsbehörighet tillhandahåller skrivskyddad åtkomst toohello data i hello målregionen.</span><span class="sxs-lookup"><span data-stu-id="3d796-123">Read-access geo-redundant storage provides read-only access toohello data in hello target region.</span></span>
- <span data-ttu-id="3d796-124">**Automatisk replikering**.</span><span class="sxs-lookup"><span data-stu-id="3d796-124">**Automated replication**.</span></span> <span data-ttu-id="3d796-125">Site Recovery tillhandahåller automatiserad kontinuerlig replikering.</span><span class="sxs-lookup"><span data-stu-id="3d796-125">Site Recovery provides automated continuous replication.</span></span> <span data-ttu-id="3d796-126">Redundans och återställning kan aktiveras med en enda klickning.</span><span class="sxs-lookup"><span data-stu-id="3d796-126">Failover and failback can be triggered with a single click.</span></span>
- <span data-ttu-id="3d796-127">**Återställningstidsmålet och Återställningspunktmål**.</span><span class="sxs-lookup"><span data-stu-id="3d796-127">**RTO and RPO**.</span></span> <span data-ttu-id="3d796-128">Site Recovery drar nytta av hello Azure nätverksinfrastrukturen som kopplar samman regioner tookeep RTO och Återställningspunktmål mycket låg.</span><span class="sxs-lookup"><span data-stu-id="3d796-128">Site Recovery takes advantage of hello Azure network infrastructure that connects regions tookeep RTO and RPO very low.</span></span>
- <span data-ttu-id="3d796-129">**Testa**.</span><span class="sxs-lookup"><span data-stu-id="3d796-129">**Testing**.</span></span> <span data-ttu-id="3d796-130">Du kan köra katastrofåterställning flyttar med på begäran redundanstestningar, vid behov utan att påverka dina produktionsarbetsbelastningar eller pågående replikering.</span><span class="sxs-lookup"><span data-stu-id="3d796-130">You can run disaster-recovery drills with on-demand test failovers, as and when needed, without affecting your production workloads or ongoing replication.</span></span>
- <span data-ttu-id="3d796-131">**Återställningsplaner**.</span><span class="sxs-lookup"><span data-stu-id="3d796-131">**Recovery plans**.</span></span> <span data-ttu-id="3d796-132">Du kan använda återställning planer tooorchestrate redundans och återställning efter fel för hello hela program som körs på flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3d796-132">You can use recovery plans tooorchestrate failover and failback of hello entire application running on multiple VMs.</span></span> <span data-ttu-id="3d796-133">hello återställning plan för har omfattande förstklassigt integrering med Azure automation-runbooks.</span><span class="sxs-lookup"><span data-stu-id="3d796-133">hello recovery plan feature has rich first-class integration with Azure automation runbooks.</span></span>


## <a name="deployment-summary"></a><span data-ttu-id="3d796-134">Översikt över distribution</span><span class="sxs-lookup"><span data-stu-id="3d796-134">Deployment summary</span></span>

<span data-ttu-id="3d796-135">Här följer en sammanfattning av vad du behöver toodo tooset duplicering av virtuella datorer mellan Azure-regioner:</span><span class="sxs-lookup"><span data-stu-id="3d796-135">Here's a summary of what you need toodo tooset up replication of VMs between Azure regions:</span></span>

1. <span data-ttu-id="3d796-136">Skapa ett Recovery Services-valv.</span><span class="sxs-lookup"><span data-stu-id="3d796-136">Create a Recovery Services vault.</span></span> <span data-ttu-id="3d796-137">hello valvet innehåller konfigurationsinställningar och styr replikeringen.</span><span class="sxs-lookup"><span data-stu-id="3d796-137">hello vault contains configuration settings and orchestrates replication.</span></span>
2. <span data-ttu-id="3d796-138">Aktivera replikering för hello Azure virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3d796-138">Enable replication for hello Azure VMs.</span></span>
3. <span data-ttu-id="3d796-139">Kör ett test redundans toomake till att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="3d796-139">Run a test failover toomake sure that everything's working as expected.</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="3d796-140">Du kan kontrollera hello [stöd matrix för replikering av Azure Virtuella](./site-recovery-support-matrix-azure-to-azure.md).</span><span class="sxs-lookup"><span data-stu-id="3d796-140">You can check hello [support matrix for Azure VM replication](./site-recovery-support-matrix-azure-to-azure.md).</span></span>

>[!IMPORTANT]
>
> <span data-ttu-id="3d796-141">Information om hur tooconfigure hello krävs utgående nätverksanslutningen för virtuella datorer i Azure för Site Recovery replikering finns hello [nätverk riktlinjerna](./site-recovery-azure-to-azure-networking-guidance.md).</span><span class="sxs-lookup"><span data-stu-id="3d796-141">For information on how tooconfigure hello required network outbound connectivity for Azure VMs for Site Recovery replication, see hello [networking guidance document](./site-recovery-azure-to-azure-networking-guidance.md).</span></span>

### <a name="before-you-start"></a><span data-ttu-id="3d796-142">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="3d796-142">Before you start</span></span>

* <span data-ttu-id="3d796-143">Ditt Azure-konto måste toohave vissa [behörigheter](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replikering av en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="3d796-143">Your Azure user account needs toohave certain [permissions](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) tooenable replication of an Azure virtual machine.</span></span>
* <span data-ttu-id="3d796-144">Din Azure-prenumeration ska vara aktiverade toocreate virtuella datorer i hello målplats som du vill toouse som hello disaster recovery region.</span><span class="sxs-lookup"><span data-stu-id="3d796-144">Your Azure subscription should be enabled toocreate VMs in hello target location that you want toouse as hello disaster recovery region.</span></span> <span data-ttu-id="3d796-145">Kontakta supporten tooenable hello krävs kvoten.</span><span class="sxs-lookup"><span data-stu-id="3d796-145">Contact support tooenable hello required quota.</span></span>

## <a name="create-a-recovery-services-vault"></a><span data-ttu-id="3d796-146">Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="3d796-146">Create a Recovery Services vault</span></span>

[!INCLUDE [site-recovery-create-vault](../../includes/site-recovery-create-vault.md)]

>[!NOTE]
>
> <span data-ttu-id="3d796-147">Vi rekommenderar att du skapar hello Recovery Services-valvet i hello plats där du vill att din tooreplicate för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3d796-147">We recommend that you create hello Recovery Services vault in hello location where you want your VMs tooreplicate.</span></span> <span data-ttu-id="3d796-148">Till exempel om din målplatsen är hello central oss, skapa hello valv i **centrala USA**.</span><span class="sxs-lookup"><span data-stu-id="3d796-148">For example, if your target location is hello central US, create hello vault in **Central US**.</span></span>

## <a name="enable-replication"></a><span data-ttu-id="3d796-149">Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="3d796-149">Enable replication</span></span>

<span data-ttu-id="3d796-150">I **Recovery Services-valv**, klicka på valvnamnet för hello.</span><span class="sxs-lookup"><span data-stu-id="3d796-150">In **Recovery Services vaults**, click hello vault name.</span></span> <span data-ttu-id="3d796-151">Klicka på hello i hello valvet, **+ replikera** knappen.</span><span class="sxs-lookup"><span data-stu-id="3d796-151">In hello vault, click hello **+Replicate** button.</span></span>

### <a name="step-1-configure-hello-source"></a><span data-ttu-id="3d796-152">Steg 1.</span><span class="sxs-lookup"><span data-stu-id="3d796-152">Step 1.</span></span> <span data-ttu-id="3d796-153">Konfigurera hello källa</span><span class="sxs-lookup"><span data-stu-id="3d796-153">Configure hello source</span></span>
1. <span data-ttu-id="3d796-154">I **källa**väljer **Azure - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="3d796-154">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="3d796-155">I **datakällplats**väljer hello källa Azure-region där din virtuella dator körs för närvarande.</span><span class="sxs-lookup"><span data-stu-id="3d796-155">In **Source location**, select hello source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="3d796-156">Välj hello distributionsmodell för din virtuella datorer: **Resource Manager** eller **klassiska**.</span><span class="sxs-lookup"><span data-stu-id="3d796-156">Select hello deployment model of your VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="3d796-157">Välj hello **källresursgruppen** för hanteraren för virtuella datorer eller **Molntjänsten** för klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3d796-157">Select hello **Source resource group** for Resource Manager VMs or **cloud service** for classic VMs.</span></span>

    ![Konfigurera hello källa](./media/site-recovery-azure-to-azure/source.png)

### <a name="step-2-select-virtual-machines"></a><span data-ttu-id="3d796-159">Steg 2.</span><span class="sxs-lookup"><span data-stu-id="3d796-159">Step 2.</span></span> <span data-ttu-id="3d796-160">Välj virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="3d796-160">Select virtual machines</span></span>

* <span data-ttu-id="3d796-161">Välj hello virtuella datorer du vill tooreplicate och klicka sedan på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d796-161">Select hello VMs you want tooreplicate, and then click **OK**.</span></span>

    ![Välj virtuella datorer](./media/site-recovery-azure-to-azure/vms.png)

### <a name="step-3-configure-settings"></a><span data-ttu-id="3d796-163">Steg 3.</span><span class="sxs-lookup"><span data-stu-id="3d796-163">Step 3.</span></span> <span data-ttu-id="3d796-164">Konfigurera inställningar</span><span class="sxs-lookup"><span data-stu-id="3d796-164">Configure settings</span></span>

1. <span data-ttu-id="3d796-165">toooverride hello standard målet inställningar och ange hello inställningar du väljer, klickar du på **anpassa**.</span><span class="sxs-lookup"><span data-stu-id="3d796-165">toooverride hello default target settings and specify hello settings of your choice, click **Customize**.</span></span> <span data-ttu-id="3d796-166">Mer information finns i [anpassa målresurser](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span><span class="sxs-lookup"><span data-stu-id="3d796-166">For more information, see [Customize target resources](site-recovery-replicate-azure-to-azure.md##customize-target-resources).</span></span>

    ![Konfigurera inställningar](./media/site-recovery-azure-to-azure/settings.png)


2. <span data-ttu-id="3d796-168">Som standard skapar Site Recovery en replikeringsprincip som tar programkonsekventa ögonblicksbilder var fjärde timme och behåller återställningspunkter under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="3d796-168">By default, Site Recovery creates a replication policy that takes app-consistent snapshots every 4 hours and retains recovery points for 24 hours.</span></span> <span data-ttu-id="3d796-169">toocreate en princip med olika inställningar, klicka på **anpassa** nästa för**Replikeringsprincipen**.</span><span class="sxs-lookup"><span data-stu-id="3d796-169">toocreate a policy with different settings, click **Customize** next too**Replication Policy**.</span></span>

    ![Anpassa principen](./media/site-recovery-azure-to-azure/customize-policy.png)

3. <span data-ttu-id="3d796-171">toostart etablering hello target-resurser, klickar du på **skapa målresurser**.</span><span class="sxs-lookup"><span data-stu-id="3d796-171">toostart provisioning hello target resources, click **Create target resources**.</span></span> <span data-ttu-id="3d796-172">Etablering tar en minut.</span><span class="sxs-lookup"><span data-stu-id="3d796-172">Provisioning takes a minute or so.</span></span> <span data-ttu-id="3d796-173">Stäng inte hello bladet under etableringen, eller så behöver toostart över.</span><span class="sxs-lookup"><span data-stu-id="3d796-173">Don't close hello blade during provisioning, or you need toostart over.</span></span>

4. <span data-ttu-id="3d796-174">tootrigger replikering av hello valt VM, klicka på **Aktivera replikering**.</span><span class="sxs-lookup"><span data-stu-id="3d796-174">tootrigger replication of hello selected VM, click **Enable replication**.</span></span>

5. <span data-ttu-id="3d796-175">Du kan följa förloppet för hello **Skyddsaktivering** jobb **inställningar** > **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="3d796-175">You can track progress of hello **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

6. <span data-ttu-id="3d796-176">I **inställningar** > **replikerade objekt**, kan du visa hello status för virtuella datorer och hello inledande replikering pågår.</span><span class="sxs-lookup"><span data-stu-id="3d796-176">In **Settings** > **Replicated Items**, you can view hello status of VMs and hello initial replication progress.</span></span> <span data-ttu-id="3d796-177">Klicka på hello VM toodrill ned till dess inställningar.</span><span class="sxs-lookup"><span data-stu-id="3d796-177">Click hello VM toodrill down into its settings.</span></span>

## <a name="run-a-test-failover"></a><span data-ttu-id="3d796-178">Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="3d796-178">Run a test failover</span></span>

<span data-ttu-id="3d796-179">När du har skapat allt kör ett test redundans toomake att allt fungerar som förväntat:</span><span class="sxs-lookup"><span data-stu-id="3d796-179">After you set up everything, run a test failover toomake sure everything's working as expected:</span></span>

1. <span data-ttu-id="3d796-180">toofail över en enskild dator i **inställningar** > **replikerade objekt**, klicka på hello VM **+ testa redundans** ikon.</span><span class="sxs-lookup"><span data-stu-id="3d796-180">toofail over a single machine, in **Settings** > **Replicated Items**, click hello VM **+Test Failover** icon.</span></span>

2. <span data-ttu-id="3d796-181">toofail via en återställning planera i **inställningar** > **Återställningsplaner**, högerklicka på hello plan **Redundanstestningen**.</span><span class="sxs-lookup"><span data-stu-id="3d796-181">toofail over a recovery plan, in **Settings** > **Recovery Plans**, right-click hello plan **Test Failover**.</span></span> <span data-ttu-id="3d796-182">toocreate en återställningsplan [följer du dessa instruktioner](site-recovery-create-recovery-plans.md).</span><span class="sxs-lookup"><span data-stu-id="3d796-182">toocreate a recovery plan, [follow these instructions](site-recovery-create-recovery-plans.md).</span></span> 

3. <span data-ttu-id="3d796-183">I **Redundanstest**väljer hello mål virtuella Azure-nätverket toowhich virtuella Azure-datorer är anslutna när hello växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3d796-183">In **Test Failover**, select hello target Azure virtual network toowhich Azure VMs are connected after hello failover occurs.</span></span>

4. <span data-ttu-id="3d796-184">toostart Hej växling vid fel, klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3d796-184">toostart hello failover, click **OK**.</span></span> <span data-ttu-id="3d796-185">tootrack utvecklas, klicka på hello VM tooopen dess egenskaper.</span><span class="sxs-lookup"><span data-stu-id="3d796-185">tootrack progress, click hello VM tooopen its properties.</span></span> <span data-ttu-id="3d796-186">Du kan också klicka på hello **Redundanstest** jobb i hello valvnamnet > **inställningar** > **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="3d796-186">Or you can click hello **Test Failover** job in hello vault name > **Settings** > **Jobs** > **Site Recovery jobs**.</span></span>

5. <span data-ttu-id="3d796-187">Efter hello växling vid fel är klar hello replik Azure datorn visas i hello Azure-portalen > **virtuella datorer**.</span><span class="sxs-lookup"><span data-stu-id="3d796-187">After hello failover finishes, hello replica Azure machine appears in hello Azure portal > **Virtual Machines**.</span></span> <span data-ttu-id="3d796-188">Kontrollera att hello VM hello rätt storlek, att den är ansluten toohello lämpligt nätverk och att den körs.</span><span class="sxs-lookup"><span data-stu-id="3d796-188">Make sure that hello VM is hello appropriate size, that it's connected toohello appropriate network, and that it's running.</span></span>

6. <span data-ttu-id="3d796-189">toodelete hello virtuella datorer som har skapats under hello redundanstestningen klickar du på **Rensa redundanstestet** på hello replikerade objekt eller hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="3d796-189">toodelete hello VMs that were created during hello test failover, click **Cleanup test failover** on hello replicated item or hello recovery plan.</span></span> <span data-ttu-id="3d796-190">I **anteckningar**, registrera och spara observationer från redundanstestningen hello.</span><span class="sxs-lookup"><span data-stu-id="3d796-190">In **Notes**, record and save any observations associated with hello test failover.</span></span> 

<span data-ttu-id="3d796-191">[Lär dig mer](site-recovery-test-failover-to-azure.md) om redundanstestning.</span><span class="sxs-lookup"><span data-stu-id="3d796-191">[Learn more](site-recovery-test-failover-to-azure.md) about test failovers.</span></span>


## <a name="next-steps"></a><span data-ttu-id="3d796-192">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3d796-192">Next steps</span></span>

<span data-ttu-id="3d796-193">När du testa hello distribution:</span><span class="sxs-lookup"><span data-stu-id="3d796-193">After you test hello deployment:</span></span>

- <span data-ttu-id="3d796-194">[Lär dig mer](site-recovery-failover.md) om olika typer av redundansväxlingar och hur toorun dem.</span><span class="sxs-lookup"><span data-stu-id="3d796-194">[Learn more](site-recovery-failover.md) about different types of failovers and how toorun them.</span></span>
- <span data-ttu-id="3d796-195">Lär dig mer om [med återställningsplaner](site-recovery-create-recovery-plans.md) tooreduce Återställningstidsmål.</span><span class="sxs-lookup"><span data-stu-id="3d796-195">Learn more about [using recovery plans](site-recovery-create-recovery-plans.md) tooreduce RTO.</span></span>
