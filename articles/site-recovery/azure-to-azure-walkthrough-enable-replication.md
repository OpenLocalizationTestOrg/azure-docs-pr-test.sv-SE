---
title: "Aktivera replikering för virtuella Azure-datorer till en annan Azure-region med Azure Site Recovery | Microsoft Docs"
description: "Sammanfattas de steg som du måste aktivera replikering till en annan Azure-region för virtuella Azure-datorer med hjälp av Azure Site Recovery-tjänsten"
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
ms.openlocfilehash: f426ba4341e61d3c7da820d7d5097b217e94ca0e
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="step-5-enable-replication-for-azure-vms"></a><span data-ttu-id="3173a-103">Steg 5: Aktivera replikering för virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="3173a-103">Step 5: Enable replication for Azure VMs</span></span>


<span data-ttu-id="3173a-104">När du har installerat en [Recovery Services-valvet](azure-to-azure-walkthrough-vault.md), Använd den här artikeln för att aktivera replikering av virtuella datorer (VM) till en annan Azure-regionen med [Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="3173a-104">After setting up a [Recovery Services vault](azure-to-azure-walkthrough-vault.md), use this article to enable replication of virtual machines (VMs), to another Azure region, with [Azure Site Recovery](site-recovery-overview.md).</span></span> <span data-ttu-id="3173a-105">Om du vill aktivera replikering, konfigurera inställningar för källa och mål, kontrollera replikeringsprinciper och välj virtuella datorer du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="3173a-105">To enable replication, you set up source and target settings, verify the replication policy, and select VMs you want to replicate.</span></span>

- <span data-ttu-id="3173a-106">När du är klar i artikeln ditt virtuella Azure-datorer ska replikeras till den sekundära regionen för Azure.</span><span class="sxs-lookup"><span data-stu-id="3173a-106">When you finish the article, your Azure VMs should be replicating to the secondary Azure region.</span></span>
- <span data-ttu-id="3173a-107">Skicka kommentarer längst ned i den här artikeln eller ställa frågor i den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span><span class="sxs-lookup"><span data-stu-id="3173a-107">Post any comments at the bottom of this article, or ask questions in the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr)</span></span>

>[!NOTE]
>
> <span data-ttu-id="3173a-108">Azure VM-replikering är för närvarande under förhandsgranskning.</span><span class="sxs-lookup"><span data-stu-id="3173a-108">Azure VM replication is currently in preview.</span></span>


## <a name="select-the-source"></a><span data-ttu-id="3173a-109">Välj källa</span><span class="sxs-lookup"><span data-stu-id="3173a-109">Select the source</span></span> 

1. <span data-ttu-id="3173a-110">Klicka på valvnamnet i Recovery Services-valv > **+ replikera**.</span><span class="sxs-lookup"><span data-stu-id="3173a-110">In Recovery Services vaults, click the vault name > **+Replicate**.</span></span>
2. <span data-ttu-id="3173a-111">I **källa**väljer **Azure - PREVIEW**.</span><span class="sxs-lookup"><span data-stu-id="3173a-111">In **Source**, select **Azure - PREVIEW**.</span></span>
2. <span data-ttu-id="3173a-112">I **datakällplats**, väljer du den Azure-region där din virtuella dator körs för närvarande.</span><span class="sxs-lookup"><span data-stu-id="3173a-112">In **Source location**, select the source Azure region where your VMs are currently running.</span></span>
3. <span data-ttu-id="3173a-113">Välj den **virtuella Azure-datorn distributionsmodell** för virtuella datorer: **Resource Manager** eller **klassiska**.</span><span class="sxs-lookup"><span data-stu-id="3173a-113">Select the **Azure virtual machine deployment model** for VMs: **Resource Manager** or **Classic**.</span></span>
4. <span data-ttu-id="3173a-114">Välj den **källresursgruppen** för hanteraren för virtuella datorer, eller **Molntjänsten** för klassiska virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="3173a-114">Select the **Source resource group** for Resource Manager VMs, or **cloud service** for classic VMs.</span></span>
5. <span data-ttu-id="3173a-115">Spara inställningarna genom att klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3173a-115">Click **OK** to save the settings.</span></span>

    ![Konfigurera källan](./media/azure-to-azure-walkthrough-enable-replication/source.png)

## <a name="select-the-vms"></a><span data-ttu-id="3173a-117">Välj de virtuella datorerna</span><span class="sxs-lookup"><span data-stu-id="3173a-117">Select the VMs</span></span>

<span data-ttu-id="3173a-118">Site Recovery hämtar en lista över de virtuella datorerna som är associerad med tjänsten prenumeration och resurs grupp eller ett moln.</span><span class="sxs-lookup"><span data-stu-id="3173a-118">Site Recovery retrieves a list of the VMs associated with the subscription and resource group/cloud service.</span></span>

1. <span data-ttu-id="3173a-119">I **virtuella datorer**, Välj de virtuella datorerna som du vill replikera.</span><span class="sxs-lookup"><span data-stu-id="3173a-119">In **Virtual Machines**, select the VMs you want to replicate.</span></span>
2. <span data-ttu-id="3173a-120">Klicka på **OK**.</span><span class="sxs-lookup"><span data-stu-id="3173a-120">Click **OK**.</span></span>

    ![Välj virtuella datorer](./media/azure-to-azure-walkthrough-enable-replication/vms.png)


## <a name="configure-settings"></a><span data-ttu-id="3173a-122">Konfigurera inställningar</span><span class="sxs-lookup"><span data-stu-id="3173a-122">Configure settings</span></span>

<span data-ttu-id="3173a-123">Site Recovery tillhandahåller standardinställningar för målregionen (baserat på inställningarna för datakälla region) och replikeringsprinciper:</span><span class="sxs-lookup"><span data-stu-id="3173a-123">Site Recovery provisions default settings for the target region (based on the source region settings), and the replication policy:</span></span>

   - <span data-ttu-id="3173a-124">**Målplatsen**: målregionen som du vill använda för katastrofåterställning.</span><span class="sxs-lookup"><span data-stu-id="3173a-124">**Target location**: The target region you want to use for disaster recovery.</span></span> <span data-ttu-id="3173a-125">Vi rekommenderar att målplatsen matchar platsen för Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="3173a-125">We recommend that the target location matches the location of the Site Recovery vault.</span></span>
   - <span data-ttu-id="3173a-126">**Målresursgruppen**: resursgruppen som virtuella Azure-datorer i målet region ska tillhöra efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3173a-126">**Target resource group**: Resource group to which Azure VMs in the target region will belong after failover.</span></span> <span data-ttu-id="3173a-127">Som standard skapar Site Recovery en ny resursgrupp i målregionen med ett ”asr” suffix.</span><span class="sxs-lookup"><span data-stu-id="3173a-127">By default, Site Recovery creates a new resource group in the target region with an "asr" suffix.</span></span> 
   - <span data-ttu-id="3173a-128">**Virtuella målnätverket**: nätverk i vilka Azure virtuella datorer i målet region kommer att finnas efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="3173a-128">**Target virtual network**: The network in which Azure VMs in the target region will be located after failover.</span></span> <span data-ttu-id="3173a-129">Som standard skapar Site Recovery ett nytt virtuellt nätverk (och undernät) i området mål med en ”asr” suffix.</span><span class="sxs-lookup"><span data-stu-id="3173a-129">By default, Site Recovery creates a new virtual network (and subnets) in the target region with an "asr" suffix.</span></span> <span data-ttu-id="3173a-130">Det här nätverket är mappad till nätverket källa.</span><span class="sxs-lookup"><span data-stu-id="3173a-130">This network is mapped to your source network.</span></span> <span data-ttu-id="3173a-131">Observera att du kan tilldela en specifik IP-adress efter växling vid fel på en virtuell dator om du vill behålla samma IP-adress i käll- och målplatserna.</span><span class="sxs-lookup"><span data-stu-id="3173a-131">Note that you can assign a specific IP address after failover of a VM, if you need to retain the same IP address in the source and target locations.</span></span> 
   - <span data-ttu-id="3173a-132">**Cachelagra lagringskonton**: Site Recovery använder ett lagringskonto i regionen källa.</span><span class="sxs-lookup"><span data-stu-id="3173a-132">**Cache storage accounts**: Site Recovery uses a storage account in the source region.</span></span> <span data-ttu-id="3173a-133">Ändringar på virtuella källdatorer skickas till det här kontot innan replikeringen till målplatsen.</span><span class="sxs-lookup"><span data-stu-id="3173a-133">Changes on source VMs are sent to this account, before replication to the target location.</span></span> 
   - <span data-ttu-id="3173a-134">**Rikta lagringskonton**: som standard skapar Site Recovery ett nytt lagringskonto i målregionen, för spegling av källan VM storage-konto.</span><span class="sxs-lookup"><span data-stu-id="3173a-134">**Target storage accounts**: By default, Site Recovery creates a new storage account in the target region, to mirror the source VM storage account.</span></span>
   -  <span data-ttu-id="3173a-135">**Rikta tillgänglighetsuppsättningar**: som standard skapar en ny tillgänglighetsuppsättning i målregionen med suffixet ”asr” Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="3173a-135">**Target availability sets**: By default, Site Recovery creates a new availability set in the target region, with the "asr" suffix.</span></span> 
   - <span data-ttu-id="3173a-136">**Namn på**: principnamn.</span><span class="sxs-lookup"><span data-stu-id="3173a-136">**Replication policy name**: Policy name.</span></span>
   - <span data-ttu-id="3173a-137">**Kvarhållningstid för återställningspunkten**: som standard sparas Site Recovery återställningspunkter under 24 timmar.</span><span class="sxs-lookup"><span data-stu-id="3173a-137">**Recovery point retention**: By default Site Recovery keeps recovery points for 24 hours.</span></span> <span data-ttu-id="3173a-138">Du kan konfigurera ett värde mellan 1 och 72 timmar.</span><span class="sxs-lookup"><span data-stu-id="3173a-138">You can configure a value between 1 and 72 hours.</span></span>
   - <span data-ttu-id="3173a-139">**Frekvens av programkonsekventa ögonblicksbilder**: som standard Site Recovery tar en ögonblicksbild av programkonsekventa var fjärde timme.</span><span class="sxs-lookup"><span data-stu-id="3173a-139">**App-consistent snapshot frequency**: By default Site Recovery takes an app-consistent snapshot every 4 hours.</span></span> <span data-ttu-id="3173a-140">Du kan konfigurera ett värde mellan 1 och 12 timmar.</span><span class="sxs-lookup"><span data-stu-id="3173a-140">You can configure any value between 1 and 12 hours.</span></span> <span data-ttu-id="3173a-141">Data replikeras kontinuerligt:</span><span class="sxs-lookup"><span data-stu-id="3173a-141">Data is replicated continuously:</span></span>
    - <span data-ttu-id="3173a-142">Kraschkonsekvent återställningspunkter upprätthålla konsekventa data write-ordning när de skapas.</span><span class="sxs-lookup"><span data-stu-id="3173a-142">Crash-consistent recovery points maintain consistent data write-order when created.</span></span> <span data-ttu-id="3173a-143">Den här typen av återställningspunkt räcker vanligtvis om din app är utformat för att återställa från en krasch utan inkonsekventa data</span><span class="sxs-lookup"><span data-stu-id="3173a-143">This type of recovery point is usually sufficient if your app is designed to recover from a crash without data inconsistencies</span></span>
    - <span data-ttu-id="3173a-144">Kraschkonsekvent återställningspunkter skapas med några minuters mellanrum.</span><span class="sxs-lookup"><span data-stu-id="3173a-144">Crash-consistent recovery points are generated every few minutes.</span></span> <span data-ttu-id="3173a-145">Med dessa återställningspunkter att växla över och återställa din virtuella dator innehåller en återställning punkt mål (RPO) i minuter.</span><span class="sxs-lookup"><span data-stu-id="3173a-145">Using these recovery points to fail over and recover your VMs provides a Recovery Point Objective (RPO) in the order of minutes.</span></span>
    - <span data-ttu-id="3173a-146">Programkonsekventa återställningspunkter (förutom skrivåtgärder ordning konsekvenskontroll) kontrollera att appar körs slutföra alla åtgärder och rensa buffertar till disk (programmet offline stegvis).</span><span class="sxs-lookup"><span data-stu-id="3173a-146">App-consistent recovery points (in addition to write-order consistency) ensure that running apps complete all operations and flush buffers to disk (application quiescing).</span></span> <span data-ttu-id="3173a-147">Vi rekommenderar att du använder dessa återställningspunkter för databas-appar som SQL Server, Oracle och Exchange.</span><span class="sxs-lookup"><span data-stu-id="3173a-147">We recommend you use these recovery points for database apps such as SQL Server, Oracle, and Exchange.</span></span>
        
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/settings.png)


### <a name="modify-settings"></a><span data-ttu-id="3173a-149">Ändra inställningar</span><span class="sxs-lookup"><span data-stu-id="3173a-149">Modify settings</span></span>

<span data-ttu-id="3173a-150">Om du vill ändra inställningar för mål- och replikering, gör du följande:</span><span class="sxs-lookup"><span data-stu-id="3173a-150">If you want to modify target and replication policy settings, do the following:</span></span>

1. <span data-ttu-id="3173a-151">Om du vill visa eller ändra Målinställningar, klickar du på **inställningar**.</span><span class="sxs-lookup"><span data-stu-id="3173a-151">To view or modify target settings, click **Settings**.</span></span>
2. <span data-ttu-id="3173a-152">Om du vill åsidosätta standardinställningarna för mål klickar du på **anpassa**.</span><span class="sxs-lookup"><span data-stu-id="3173a-152">To override the default target settings, click **Customize**.</span></span> <span data-ttu-id="3173a-153">Du kan ange en target-resursgruppen, virtuella nätverk, tillgänglighetsuppsättning och mål-lagringskontot.</span><span class="sxs-lookup"><span data-stu-id="3173a-153">You can specify a target resource group, virtual network, availability set, and target storage account.</span></span> <span data-ttu-id="3173a-154">Du kan bara lägga till tillgänglighetsuppsättningar om virtuella datorer som är en del av en uppsättning i käll-region.</span><span class="sxs-lookup"><span data-stu-id="3173a-154">You can only add availability sets if VMs are part of a set in the source region.</span></span>

    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-target.png)

3. <span data-ttu-id="3173a-156">Om du vill åsidosätta replikeringsinställningar för återställningspunkter och programkonsekventa ögonblicksbilder klickar du på **anpassa** bredvid **Replikeringsprincipen**.</span><span class="sxs-lookup"><span data-stu-id="3173a-156">To override replication settings for recovery points and app-consistent snapshots, click **Customize** next to **Replication Policy**.</span></span>
 
    ![Konfigurera inställningar](./media/azure-to-azure-walkthrough-enable-replication/customize-policy.png)

4. <span data-ttu-id="3173a-158">Starta etablering target-resurser, klicka på **skapa målresurser**.</span><span class="sxs-lookup"><span data-stu-id="3173a-158">To start provisioning the target resources, click **Create target resources**.</span></span> <span data-ttu-id="3173a-159">Etablering tar en minut.</span><span class="sxs-lookup"><span data-stu-id="3173a-159">Provisioning takes a minute or so.</span></span> <span data-ttu-id="3173a-160">Stäng inte bladet under etableringen eller måste du börja om från början.</span><span class="sxs-lookup"><span data-stu-id="3173a-160">Don't close the blade during provisioning, or you'll have to start over.</span></span>




## <a name="enable-replication"></a><span data-ttu-id="3173a-161">Aktivera replikering</span><span class="sxs-lookup"><span data-stu-id="3173a-161">Enable replication</span></span>

1. <span data-ttu-id="3173a-162">I **inställningar**, klickar du på **Aktivera replikering**.</span><span class="sxs-lookup"><span data-stu-id="3173a-162">In **Settings**, click **Enable replication**.</span></span> <span data-ttu-id="3173a-163">Detta gör det möjligt för inledande replikering av de virtuella datorerna som du har valt.</span><span class="sxs-lookup"><span data-stu-id="3173a-163">This enables initial replication of the VMs you selected.</span></span> <span data-ttu-id="3173a-164">Status för den inledande replikeringen kan ta lite tid att uppdatera.</span><span class="sxs-lookup"><span data-stu-id="3173a-164">Initial replication status might take some time to refresh.</span></span> <span data-ttu-id="3173a-165">Klicka på **uppdatera** att hämta senaste status.</span><span class="sxs-lookup"><span data-stu-id="3173a-165">Click **Refresh** to get the latest status.</span></span>

2. <span data-ttu-id="3173a-166">Du kan följa förloppet för den **Aktivera skydd** jobb **inställningar** > **jobb** > **Site Recovery-jobb**.</span><span class="sxs-lookup"><span data-stu-id="3173a-166">You can track progress of the **Enable protection** job in **Settings** > **Jobs** > **Site Recovery Jobs**.</span></span>

3. <span data-ttu-id="3173a-167">I **inställningar** > **replikerade objekt**, du kan visa status för virtuella datorer och den inledande replikeringen pågår.</span><span class="sxs-lookup"><span data-stu-id="3173a-167">In **Settings** > **Replicated Items**, you can view the status of VMs and the initial replication progress.</span></span> <span data-ttu-id="3173a-168">Klicka på den virtuella datorn och öka detaljnivån till dess inställningar.</span><span class="sxs-lookup"><span data-stu-id="3173a-168">Click the VM to drill down into its settings.</span></span>



## <a name="next-steps"></a><span data-ttu-id="3173a-169">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3173a-169">Next steps</span></span>

<span data-ttu-id="3173a-170">Gå till [steg 6: köra ett redundanstest](azure-to-azure-walkthrough-test-failover.md)</span><span class="sxs-lookup"><span data-stu-id="3173a-170">Go to [Step 6: Run a test failover](azure-to-azure-walkthrough-test-failover.md)</span></span>
