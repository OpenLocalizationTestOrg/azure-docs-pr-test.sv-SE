---
title: aaaReplicate Hyper-V virtuella datorer i VMM-moln med Azure Site Recovery och PowerShell (Resource Manager) | Microsoft Docs
description: Replikera virtuella Hyper-V-datorer i VMM-moln med Azure Site Recovery och PowerShell
services: site-recovery
documentationcenter: 
author: Rajani-Janaki-Ram
manager: rochakm
editor: raynew
ms.assetid: 6ac509ad-5024-43d8-b621-d8fec019b9a9
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: rajanaki
ms.openlocfilehash: b0f641de4e10a600ead415ceb9bd488fb4d1659d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooazure-using-powershell-and-azure-resource-manager"></a><span data-ttu-id="7f73e-103">Replikera virtuella Hyper-V-datorer i VMM-moln tooAzure med PowerShell och Azure Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7f73e-103">Replicate Hyper-V virtual machines in VMM clouds tooAzure using PowerShell and Azure Resource Manager</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="7f73e-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="7f73e-104">Azure Portal</span></span>](site-recovery-vmm-to-azure.md)
> * [<span data-ttu-id="7f73e-105">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="7f73e-105">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-azure-powershell-resource-manager.md)
> * [<span data-ttu-id="7f73e-106">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="7f73e-106">Classic Portal</span></span>](site-recovery-vmm-to-azure-classic.md)
> * [<span data-ttu-id="7f73e-107">PowerShell – Klassisk</span><span class="sxs-lookup"><span data-stu-id="7f73e-107">PowerShell - Classic</span></span>](site-recovery-deploy-with-powershell.md)
>
>

## <a name="overview"></a><span data-ttu-id="7f73e-108">Översikt</span><span class="sxs-lookup"><span data-stu-id="7f73e-108">Overview</span></span>
<span data-ttu-id="7f73e-109">Azure Site Recovery bidrar tooyour disaster recovery (BCDR) strategi för affärskontinuitet och genom att samordna replikering, redundans och återställning av virtuella datorer i ett antal distributionsscenarier.</span><span class="sxs-lookup"><span data-stu-id="7f73e-109">Azure Site Recovery contributes tooyour business continuity and disaster recovery (BCDR) strategy by orchestrating replication, failover and recovery of virtual machines in a number of deployment scenarios.</span></span> <span data-ttu-id="7f73e-110">En fullständig lista över distributionen scenarier finns hello [översikt över Azure Site Recovery](site-recovery-overview.md).</span><span class="sxs-lookup"><span data-stu-id="7f73e-110">For a full list of deployment scenarios see hello [Azure Site Recovery overview](site-recovery-overview.md).</span></span>

<span data-ttu-id="7f73e-111">Den här artikeln visar hur toouse PowerShell tooautomate vanliga uppgifter du behöver tooperform när du ställer in Azure Site Recovery tooreplicate Hyper-V virtuella datorer i System Center VMM-moln tooAzure lagring.</span><span class="sxs-lookup"><span data-stu-id="7f73e-111">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooAzure storage.</span></span>

<span data-ttu-id="7f73e-112">hello artikeln handlar om förutsättningar för hello scenariot och visar</span><span class="sxs-lookup"><span data-stu-id="7f73e-112">hello article includes prerequisites for hello scenario, and shows you</span></span>

* <span data-ttu-id="7f73e-113">Hur tooset upp ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="7f73e-113">How tooset up a Recovery Services Vault</span></span>
* <span data-ttu-id="7f73e-114">Installera hello Azure Site Recovery-providern på VMM-källservern hello</span><span class="sxs-lookup"><span data-stu-id="7f73e-114">Install hello Azure Site Recovery Provider on hello source VMM server</span></span>
* <span data-ttu-id="7f73e-115">Registrera hello servern i hello valv, lägga till ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="7f73e-115">Register hello server in hello vault, add an Azure storage account</span></span>
* <span data-ttu-id="7f73e-116">Installera hello Azure Recovery Services-agenten på Hyper-V-värdservrar</span><span class="sxs-lookup"><span data-stu-id="7f73e-116">Install hello Azure Recovery Services agent on Hyper-V host servers</span></span>
* <span data-ttu-id="7f73e-117">Konfigurera skyddsinställningar för VMM-moln som ska tillämpas tooall skyddade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="7f73e-117">Configure protection settings for VMM clouds, that will be applied tooall protected virtual machines</span></span>
* <span data-ttu-id="7f73e-118">Aktivera skyddet för de virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="7f73e-118">Enable protection for those virtual machines.</span></span>
* <span data-ttu-id="7f73e-119">Testa hello failover toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="7f73e-119">Test hello fail-over toomake sure everything's working as expected.</span></span>

<span data-ttu-id="7f73e-120">Om du stöter på problem med att installera det här scenariot, publicera dina frågor på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="7f73e-120">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="7f73e-121">Azure har två olika distributionsmodeller för att skapa och arbeta med resurser: [Resource Manager och klassisk](../azure-resource-manager/resource-manager-deployment-model.md).</span><span class="sxs-lookup"><span data-stu-id="7f73e-121">Azure has two different deployment models for creating and working with resources: [Resource Manager and Classic](../azure-resource-manager/resource-manager-deployment-model.md).</span></span> <span data-ttu-id="7f73e-122">Den här artikeln täcker hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="7f73e-122">This article covers using hello Resource Manager deployment model.</span></span>
>
>

## <a name="before-you-start"></a><span data-ttu-id="7f73e-123">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="7f73e-123">Before you start</span></span>
<span data-ttu-id="7f73e-124">Kontrollera att du har dessa krav är uppfyllda:</span><span class="sxs-lookup"><span data-stu-id="7f73e-124">Make sure you have these prerequisites in place:</span></span>

### <a name="azure-prerequisites"></a><span data-ttu-id="7f73e-125">Krav för Azure</span><span class="sxs-lookup"><span data-stu-id="7f73e-125">Azure prerequisites</span></span>
* <span data-ttu-id="7f73e-126">Du behöver ett [Microsoft Azure](https://azure.microsoft.com/)-konto.</span><span class="sxs-lookup"><span data-stu-id="7f73e-126">You'll need a [Microsoft Azure](https://azure.microsoft.com/) account.</span></span> <span data-ttu-id="7f73e-127">Om du inte har någon, börja med en [kostnadsfritt konto](https://azure.microsoft.com/free).</span><span class="sxs-lookup"><span data-stu-id="7f73e-127">If you don't have one, start with a [free account](https://azure.microsoft.com/free).</span></span> <span data-ttu-id="7f73e-128">Dessutom kan du läsa om hello [priser för Azure Site Recovery Manager](https://azure.microsoft.com/pricing/details/site-recovery/).</span><span class="sxs-lookup"><span data-stu-id="7f73e-128">In addition, you can read about hello [Azure Site Recovery Manager pricing](https://azure.microsoft.com/pricing/details/site-recovery/).</span></span>
* <span data-ttu-id="7f73e-129">Du behöver en CSP-prenumeration om du provar ut hello replikering tooa CSP prenumeration scenario.</span><span class="sxs-lookup"><span data-stu-id="7f73e-129">You'll need a CSP subscription if you are trying out hello replication tooa CSP subscription scenario.</span></span> <span data-ttu-id="7f73e-130">Mer information om hello CSP-programmet i [hur tooenroll i hello CSP program](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f73e-130">Learn more about hello CSP program in [how tooenroll in hello CSP program](https://msdn.microsoft.com/library/partnercenter/mt156995.aspx).</span></span>
* <span data-ttu-id="7f73e-131">Du behöver ett Azure v2 lagring (Resource Manager) konto toostore data som replikeras tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7f73e-131">You'll need an Azure v2 storage (Resource Manager) account toostore data replicated tooAzure.</span></span> <span data-ttu-id="7f73e-132">hello-kontot måste geo-replikering aktiverat.</span><span class="sxs-lookup"><span data-stu-id="7f73e-132">hello account needs geo-replication enabled.</span></span> <span data-ttu-id="7f73e-133">Det bör vara i samma region som hello Azure Site Recovery-tjänsten hello och associeras med hello samma prenumeration eller hello CSP-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7f73e-133">It should be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription or hello CSP subscription.</span></span> <span data-ttu-id="7f73e-134">toolearn mer om hur du konfigurerar Azure storage finns hello [introduktion tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) referens.</span><span class="sxs-lookup"><span data-stu-id="7f73e-134">toolearn more about setting up Azure storage, see hello [Introduction tooMicrosoft Azure Storage](../storage/common/storage-introduction.md) for reference.</span></span>
* <span data-ttu-id="7f73e-135">Du behöver toomake till att virtuella datorer som du vill tooprotect följer hello [förutsättningar för Azure virtuell dator](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span><span class="sxs-lookup"><span data-stu-id="7f73e-135">You'll need toomake sure that virtual machines you want tooprotect comply with hello [Azure virtual machine prerequisites](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements).</span></span>

> [!NOTE]
> <span data-ttu-id="7f73e-136">För närvarande är endast VM nivån åtgärder möjlig via Powershell.</span><span class="sxs-lookup"><span data-stu-id="7f73e-136">Currently, only VM level operations are possible through Powershell.</span></span> <span data-ttu-id="7f73e-137">Stöd för nivån återställningsåtgärder för planen kommer att göras snart.</span><span class="sxs-lookup"><span data-stu-id="7f73e-137">Support for recovery plan level operations will be made soon.</span></span>  <span data-ttu-id="7f73e-138">Nu är du begränsad tooperforming misslyckas-FELNIVÅ endast på en ”skyddad VM' kornighet och inte på en återställning planera nivå.</span><span class="sxs-lookup"><span data-stu-id="7f73e-138">For now, you are limited tooperforming fail-overs only at a ‘protected VM’ granularity and not at a Recovery Plan level.</span></span>
>
>

### <a name="vmm-prerequisites"></a><span data-ttu-id="7f73e-139">VMM – krav</span><span class="sxs-lookup"><span data-stu-id="7f73e-139">VMM prerequisites</span></span>
* <span data-ttu-id="7f73e-140">Du måste VMM-servern som körs på System Center 2012 R2.</span><span class="sxs-lookup"><span data-stu-id="7f73e-140">You'll need VMM server running on System Center 2012 R2.</span></span>
* <span data-ttu-id="7f73e-141">Någon VMM-server som innehåller virtuella datorer vill du tooprotect måste köra hello Azure Site Recovery-providern.</span><span class="sxs-lookup"><span data-stu-id="7f73e-141">Any VMM server containing virtual machines you want tooprotect must be running hello Azure Site Recovery Provider.</span></span> <span data-ttu-id="7f73e-142">Detta installeras under hello Azure Site Recovery-distribution.</span><span class="sxs-lookup"><span data-stu-id="7f73e-142">This is installed during hello Azure Site Recovery deployment.</span></span>
* <span data-ttu-id="7f73e-143">Du behöver minst ett moln på hello VMM-servern som du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="7f73e-143">You'll need at least one cloud on hello VMM server you want tooprotect.</span></span> <span data-ttu-id="7f73e-144">hello moln bör innehålla:</span><span class="sxs-lookup"><span data-stu-id="7f73e-144">hello cloud should contain:</span></span>
  * <span data-ttu-id="7f73e-145">En eller flera VMM-värdgrupper.</span><span class="sxs-lookup"><span data-stu-id="7f73e-145">One or more VMM host groups.</span></span>
  * <span data-ttu-id="7f73e-146">En eller flera Hyper-V-värdservrar eller Hyper-V-kluster i varje värdgrupp.</span><span class="sxs-lookup"><span data-stu-id="7f73e-146">One or more Hyper-V host servers or clusters in each host group.</span></span>
  * <span data-ttu-id="7f73e-147">En eller flera virtuella datorer på källservern för hello Hyper-V.</span><span class="sxs-lookup"><span data-stu-id="7f73e-147">One or more virtual machines on hello source Hyper-V server.</span></span>
* <span data-ttu-id="7f73e-148">Lär dig mer om hur du konfigurerar VMM-moln:</span><span class="sxs-lookup"><span data-stu-id="7f73e-148">Learn more about setting up VMM clouds:</span></span>
  * <span data-ttu-id="7f73e-149">Läs mer om privata VMM-moln i [vad är nytt i privata moln med VMM för System Center 2012 R2](http://go.microsoft.com/fwlink/?LinkId=324952) och i [VMM 2012 och hello moln](http://go.microsoft.com/fwlink/?LinkId=324956).</span><span class="sxs-lookup"><span data-stu-id="7f73e-149">Read more about private VMM clouds in [What’s New in Private Cloud with System Center 2012 R2 VMM](http://go.microsoft.com/fwlink/?LinkId=324952) and in [VMM 2012 and hello clouds](http://go.microsoft.com/fwlink/?LinkId=324956).</span></span>
  * <span data-ttu-id="7f73e-150">Lär dig mer om [konfigurera hello VMM molnet fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)</span><span class="sxs-lookup"><span data-stu-id="7f73e-150">Learn about [Configuring hello VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric)</span></span>
  * <span data-ttu-id="7f73e-151">När ditt moln fabric-element är på plats Lär dig mer om att skapa privata moln i [skapa ett privat moln i VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) och i [genomgång: skapa privata moln med System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).</span><span class="sxs-lookup"><span data-stu-id="7f73e-151">After your cloud fabric elements are in place learn about creating private clouds in [Creating a private cloud in VMM](http://go.microsoft.com/fwlink/p/?LinkId=324953) and in [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://go.microsoft.com/fwlink/p/?LinkId=324954).</span></span>

### <a name="hyper-v-prerequisites"></a><span data-ttu-id="7f73e-152">Hyper-V-krav</span><span class="sxs-lookup"><span data-stu-id="7f73e-152">Hyper-V prerequisites</span></span>
* <span data-ttu-id="7f73e-153">hello Hyper-V-värdservrar måste köra minst **Windows Server 2012** med Hyper-V-rollen eller **Microsoft Hyper-V Server 2012** och har hello senaste uppdateringarna installerade.</span><span class="sxs-lookup"><span data-stu-id="7f73e-153">hello host Hyper-V servers must be running at least **Windows Server 2012** with Hyper-V role or **Microsoft Hyper-V Server 2012** and have hello latest updates installed.</span></span>
* <span data-ttu-id="7f73e-154">Om du kör Hyper-V i ett kluster bör du vara medveten om att klusterutjämning inte skapas automatiskt om du har ett statiskt IP-adressbaserat kluster.</span><span class="sxs-lookup"><span data-stu-id="7f73e-154">If you're running Hyper-V in a cluster note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="7f73e-155">Du behöver tooconfigure hello klusterutjämning manuellt.</span><span class="sxs-lookup"><span data-stu-id="7f73e-155">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="7f73e-156">för</span><span class="sxs-lookup"><span data-stu-id="7f73e-156">For</span></span>
* <span data-ttu-id="7f73e-157">Instruktioner finns i [hur tooConfigure Hyper-V Replica Broker](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).</span><span class="sxs-lookup"><span data-stu-id="7f73e-157">For instructions see [How tooConfigure Hyper-V Replica Broker](http://blogs.technet.com/b/haroldwong/archive/2013/03/27/server-virtualization-series-hyper-v-replica-broker-explained-part-15-of-20-by-yung-chou.aspx).</span></span>
* <span data-ttu-id="7f73e-158">Alla Hyper-V-värdserver eller kluster som du vill toomanage skydd måste ingå i ett VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="7f73e-158">Any Hyper-V host server or cluster for which you want toomanage protection must be included in a VMM cloud.</span></span>

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="7f73e-159">Krav för nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="7f73e-159">Network mapping prerequisites</span></span>
<span data-ttu-id="7f73e-160">När du skyddar virtuella datorer i Azure mappar nätverksmappningen hello hello virtuella datornätverk på VMM-källservern hello och mål Azure nätverk tooenable hello följande:</span><span class="sxs-lookup"><span data-stu-id="7f73e-160">When you protect virtual machines in Azure, hello network mapping  maps hello virtual machine networks on hello source VMM server and target Azure networks tooenable hello following:</span></span>

* <span data-ttu-id="7f73e-161">Alla datorer som redundansväxlas hello samma nätverk kan ansluta tooeach andra, oavsett vilken återställningsplan de finns i.</span><span class="sxs-lookup"><span data-stu-id="7f73e-161">All machines which fail over on hello same network can connect tooeach other, irrespective of which recovery plan they are in.</span></span>
* <span data-ttu-id="7f73e-162">Om en nätverksgateway har konfigurerats på hello Azure-målnätverket kan kan virtuella datorer ansluta tooother lokala virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="7f73e-162">If a network gateway is setup on hello target Azure network, virtual machines can connect tooother on-premises virtual machines.</span></span>
* <span data-ttu-id="7f73e-163">Om du inte konfigurerar nätverksmappning kan endast hello virtuella datorer som växlar över i hello samma återställningsplan kommer att kunna tooconnect tooeach andra efter failover tooAzure.</span><span class="sxs-lookup"><span data-stu-id="7f73e-163">If you don’t configure network mapping, only hello virtual machines that fail over in hello same recovery plan will be able tooconnect tooeach other after fail-over tooAzure.</span></span>

<span data-ttu-id="7f73e-164">Om du vill toodeploy nätverksmappning behöver hello följande:</span><span class="sxs-lookup"><span data-stu-id="7f73e-164">If you want toodeploy network mapping you'll need hello following:</span></span>

* <span data-ttu-id="7f73e-165">hello virtuella datorer du vill tooprotect på VMM-källservern hello ska vara anslutna tooa VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f73e-165">hello virtual machines you want tooprotect on hello source VMM server should be connected tooa VM network.</span></span> <span data-ttu-id="7f73e-166">Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.</span><span class="sxs-lookup"><span data-stu-id="7f73e-166">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
* <span data-ttu-id="7f73e-167">En Azure-nätverk toowhich replikerade virtuella datorer kan ansluta efter redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="7f73e-167">An Azure network toowhich replicated virtual machines can connect after fail-over.</span></span> <span data-ttu-id="7f73e-168">Du väljer det här nätverket när hello failover.</span><span class="sxs-lookup"><span data-stu-id="7f73e-168">You'll select this network at hello time of fail-over.</span></span> <span data-ttu-id="7f73e-169">hello nätverket bör finnas i hello samma region som din Azure Site Recovery-prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7f73e-169">hello network should be in hello same region as your Azure Site Recovery subscription.</span></span>

<span data-ttu-id="7f73e-170">Mer information om nätverksmappning i</span><span class="sxs-lookup"><span data-stu-id="7f73e-170">Learn more about network mapping in</span></span>

* [<span data-ttu-id="7f73e-171">Hur tooconfigure logiska nätverk i VMM</span><span class="sxs-lookup"><span data-stu-id="7f73e-171">How tooconfigure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="7f73e-172">Hur tooconfigure Virtuella nätverk och gateways i VMM</span><span class="sxs-lookup"><span data-stu-id="7f73e-172">How tooconfigure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)
* [<span data-ttu-id="7f73e-173">Hur tooconfigure och övervaka virtuella nätverk i Azure</span><span class="sxs-lookup"><span data-stu-id="7f73e-173">How tooconfigure and monitor virtual networks in Azure</span></span>](https://azure.microsoft.com/documentation/services/virtual-network/)

### <a name="powershell-prerequisites"></a><span data-ttu-id="7f73e-174">PowerShell-krav</span><span class="sxs-lookup"><span data-stu-id="7f73e-174">PowerShell prerequisites</span></span>
<span data-ttu-id="7f73e-175">Kontrollera att du har Azure PowerShell redo toogo.</span><span class="sxs-lookup"><span data-stu-id="7f73e-175">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="7f73e-176">Om du redan använder PowerShell, behöver du tooupgrade tooversion 0.8.10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="7f73e-176">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="7f73e-177">Information om hur du konfigurerar PowerShell finns i hello [Guide tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="7f73e-177">For information about setting up PowerShell, see hello [Guide tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="7f73e-178">När du har skapat och konfigurerat PowerShell, kan du visa alla hello tillgängliga cmdlets för hello tjänsten [här](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="7f73e-178">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="7f73e-179">toolearn om tips som kan hjälpa dig att använda hello-cmdlets, till exempel hur parametervärden indata och utdata hanteras vanligtvis av Azure PowerShell finns hello [Guide tooget igång med Azure-Cmdlets](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="7f73e-179">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see hello [Guide tooget Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="7f73e-180">Steg 1: Ange hello prenumeration</span><span class="sxs-lookup"><span data-stu-id="7f73e-180">Step 1: Set hello subscription</span></span>
1. <span data-ttu-id="7f73e-181">Från Azure powershell, inloggning tooyour Azure-konto: med hello följande cmdlet: ar</span><span class="sxs-lookup"><span data-stu-id="7f73e-181">From Azure powershell, login tooyour Azure account: using hello following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="7f73e-182">Hämta en lista över dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7f73e-182">Get a list of your subscriptions.</span></span> <span data-ttu-id="7f73e-183">Detta visar också en lista över hello subscriptionIDs för varje hello prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="7f73e-183">This will also list hello subscriptionIDs for each of hello subscriptions.</span></span> <span data-ttu-id="7f73e-184">Notera hello prenumerations-ID för hello prenumeration där du vill toocreate hello recovery services-valvet</span><span class="sxs-lookup"><span data-stu-id="7f73e-184">Note down hello subscriptionID of hello subscription in which you wish toocreate hello recovery services vault</span></span>

        Get-AzureRmSubscription
3. <span data-ttu-id="7f73e-185">Ange hello prenumeration i vilka hello recovery services-ventilen är toobe som skapats av nämna hello prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="7f73e-185">Set hello subscription in which hello recovery services vault is toobe created by mentioning hello subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="7f73e-186">Steg 2: Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="7f73e-186">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="7f73e-187">Skapa en resursgrupp i Azure Resource Manager om du inte redan har en</span><span class="sxs-lookup"><span data-stu-id="7f73e-187">Create a resource group  in Azure Resource Manager if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="7f73e-188">Skapa ett nytt Recovery Services-valv och spara hello skapade ASR valvet objekt i en variabel (kommer att användas senare).</span><span class="sxs-lookup"><span data-stu-id="7f73e-188">Create a new Recovery Services vault and save hello created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="7f73e-189">Du kan också hämta hello ASR valvet post skapa objektet med hello Get-AzureRMRecoveryServicesVault cmdlet:-</span><span class="sxs-lookup"><span data-stu-id="7f73e-189">You can also retrieve hello ASR vault object post creation using hello Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="7f73e-190">Steg 3: Ange hello Recovery Services-ventilen kontext</span><span class="sxs-lookup"><span data-stu-id="7f73e-190">Step 3: Set hello Recovery Services Vault context</span></span>

<span data-ttu-id="7f73e-191">Ange hello valvet kontexten genom att köra hello nedan kommando.</span><span class="sxs-lookup"><span data-stu-id="7f73e-191">Set hello vault context by running hello below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="7f73e-192">Steg 4: Installera hello Azure Site Recovery Provider</span><span class="sxs-lookup"><span data-stu-id="7f73e-192">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="7f73e-193">Skapa en katalog på hello VMM-datorn genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-193">On hello VMM machine, create a directory by running hello following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="7f73e-194">Extrahera hello filer med hjälp av hello ned providern genom att köra följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="7f73e-194">Extract hello files using hello downloaded provider by running hello following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="7f73e-195">Installera hello-providern med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="7f73e-195">Install hello provider using hello following commands:</span></span>

       .\SetupDr.exe /i
       $installationRegPath = "hklm:\software\Microsoft\Microsoft System Center Virtual Machine Manager Server\DRAdapter"
       do
       {
         $isNotInstalled = $true;
         if(Test-Path $installationRegPath)
         {
           $isNotInstalled = $false;
         }
       }While($isNotInstalled)

   <span data-ttu-id="7f73e-196">Vänta tills hello installation toofinish.</span><span class="sxs-lookup"><span data-stu-id="7f73e-196">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="7f73e-197">Registrera hello servern i hello valvet med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7f73e-197">Register hello server in hello vault using hello following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-an-azure-storage-account"></a><span data-ttu-id="7f73e-198">Steg 5: Skapa ett Azure storage-konto</span><span class="sxs-lookup"><span data-stu-id="7f73e-198">Step 5: Create an Azure storage account</span></span>

<span data-ttu-id="7f73e-199">Om du inte har ett Azure storage-konto, skapa ett konto med geo-replikering är aktiverat i hello samma geo som hello valvet genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-199">If you don't have an Azure storage account, create a geo-replication enabled account in hello same geo as hello vault by running hello following command:</span></span>

        $StorageAccountName = "teststorageacc1"    #StorageAccountname
        $StorageAccountGeo  = "Southeast Asia"     
        $ResourceGroupName =  “myRG”             #ResourceGroupName
        $RecoveryStorageAccount = New-AzureRmStorageAccount -ResourceGroupName $ResourceGroupName -Name $StorageAccountName -Type “StandardGRS” -Location $StorageAccountGeo

<span data-ttu-id="7f73e-200">Observera att hello storage-konto måste vara i samma region som hello Azure Site Recovery-tjänsten hello och associeras med hello samma prenumeration.</span><span class="sxs-lookup"><span data-stu-id="7f73e-200">Note that hello storage account must be in hello same region as hello Azure Site Recovery service, and be associated with hello same subscription.</span></span>

## <a name="step-6-install-hello-azure-recovery-services-agent"></a><span data-ttu-id="7f73e-201">Steg 6: Installera hello Azure Recovery Services-agenten</span><span class="sxs-lookup"><span data-stu-id="7f73e-201">Step 6: Install hello Azure Recovery Services Agent</span></span>
1. <span data-ttu-id="7f73e-202">Hämta hello Azure Recovery Services-agenten på [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) och installera det på varje Hyper-V-värdserver i hello VMM moln du vill tooprotect.</span><span class="sxs-lookup"><span data-stu-id="7f73e-202">Download hello Azure Recovery Services agent at [http://aka.ms/latestmarsagent](http://aka.ms/latestmarsagent) and install it on each Hyper-V host server located in hello VMM clouds you want tooprotect.</span></span>
2. <span data-ttu-id="7f73e-203">Kör följande kommando på alla värdar i VMM hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-203">Run hello following command on all VMM hosts:</span></span>

       marsagentinstaller.exe /q /nu

## <a name="step-7-configure-cloud-protection-settings"></a><span data-ttu-id="7f73e-204">Steg 7: Konfigurera moln skyddsinställningarna</span><span class="sxs-lookup"><span data-stu-id="7f73e-204">Step 7: Configure cloud protection settings</span></span>
1. <span data-ttu-id="7f73e-205">Skapa en replikering princip tooAzure genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-205">Create a replication policy tooAzure by running hello following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $Recoverypoints = 6                    #specify hello number of recovery points

        $policryresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider HyperVReplicaAzure -ReplicationFrequencyInSeconds $replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours 1 -RecoveryAzureStorageAccountId "/subscriptions/q1345667/resourceGroups/test/providers/Microsoft.Storage/storageAccounts/teststorageacc1"

1. <span data-ttu-id="7f73e-206">Hämta en skyddsbehållaren genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-206">Get a protection container by running hello following commands:</span></span>

       $PrimaryCloud = "testcloud"
       $protectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  
2. <span data-ttu-id="7f73e-207">Hämta hello princip information tooa variabeln med hello jobb som har skapats och nämna hello eget namn:</span><span class="sxs-lookup"><span data-stu-id="7f73e-207">Get hello policy details tooa variable using hello job that was created and mentioning hello friendly policy name:</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="7f73e-208">Starta hello associering av hello skyddsbehållaren med hello replikeringsprincip:</span><span class="sxs-lookup"><span data-stu-id="7f73e-208">Start hello association of hello protection container with hello replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $protectionContainer  
4. <span data-ttu-id="7f73e-209">När hello jobbet har slutförts, kör du hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7f73e-209">After hello job has finished, run hello following command:</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }
5. <span data-ttu-id="7f73e-210">När hello jobbet har bearbetat köra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="7f73e-210">After hello job has finished processing, run hello following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="7f73e-211">toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="7f73e-211">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-8-configure-network-mapping"></a><span data-ttu-id="7f73e-212">Steg 8: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="7f73e-212">Step 8: Configure network mapping</span></span>
<span data-ttu-id="7f73e-213">Innan du börjar nätverksmappning Kontrollera att virtuella datorer på VMM-källservern hello anslutna tooa VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f73e-213">Before you begin network mapping verify that virtual machines on hello source VMM server are connected tooa VM network.</span></span> <span data-ttu-id="7f73e-214">Dessutom kan skapa en eller flera virtuella Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f73e-214">In addition, create one or more Azure virtual networks.</span></span>

<span data-ttu-id="7f73e-215">Mer information om hur toocreate ett virtuellt nätverk med Azure Resource Manager och PowerShell, i [skapa ett virtuellt nätverk med en plats-till-plats VPN-anslutning med Azure Resource Manager och PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)</span><span class="sxs-lookup"><span data-stu-id="7f73e-215">Learn more about how toocreate a virtual network using Azure Resource Manager and PowerShell, in [Create a virtual network with a site-to-site VPN connection using Azure Resource Manager and PowerShell](../vpn-gateway/vpn-gateway-create-site-to-site-rm-powershell.md)</span></span>

<span data-ttu-id="7f73e-216">Observera att flera virtuella nätverk kan vara mappade tooa enda Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f73e-216">Note that multiple Virtual Machine networks can be mapped tooa single Azure network.</span></span> <span data-ttu-id="7f73e-217">Om hello målnätverket har flera undernät och ett av dessa undernät har hello samma namn som undernätet på vilka hello virtuella källdatorn finns så hello replikerade virtuella datorn kommer att vara anslutna toothat målundernätverket efter redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="7f73e-217">If hello target network has multiple subnets and one of those subnets has hello same name as subnet on which hello source virtual machine is located, then hello replica virtual machine will be connected toothat target subnet after fail-over.</span></span> <span data-ttu-id="7f73e-218">Om det inte finns något målundernät med ett matchande namn, att hello virtuella datorn anslutna toohello första undernätet i hello nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f73e-218">If there is no target subnet with a matching name, hello virtual machine will be connected toohello first subnet in hello network.</span></span>

1. <span data-ttu-id="7f73e-219">hello första kommandot hämtar servrar för hello aktuella Azure Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="7f73e-219">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="7f73e-220">hello kommandot lagrar hello Microsoft Azure Site Recovery-servrar i hello $Servers matrisvariabel.</span><span class="sxs-lookup"><span data-stu-id="7f73e-220">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="7f73e-221">hello andra kommandot hämtar hello site recovery nätverk för hello första servern i hello $Servers matris.</span><span class="sxs-lookup"><span data-stu-id="7f73e-221">hello second command gets hello site recovery network for hello first server in hello $Servers array.</span></span> <span data-ttu-id="7f73e-222">hello kommandot lagrar hello nätverk i hello $Networks variabeln.</span><span class="sxs-lookup"><span data-stu-id="7f73e-222">hello command stores hello networks in hello $Networks variable.</span></span>

        $Networks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]

1. <span data-ttu-id="7f73e-223">hello tredje kommandot hämtar virtuella Azure-nätverk och värdet i hello $AzureVmNetworks variabel.</span><span class="sxs-lookup"><span data-stu-id="7f73e-223">hello third command gets Azure virtual networks, and then that value in hello $AzureVmNetworks variable.</span></span>

        $AzureVmNetworks =  Get-AzureRmVirtualNetwork
2. <span data-ttu-id="7f73e-224">hello slutliga cmdlet skapar en mappning mellan hello primära nätverk och hello Azure virtuella nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f73e-224">hello final cmdlet creates a mapping between hello primary network and hello Azure virtual machine network.</span></span> <span data-ttu-id="7f73e-225">hello cmdlet anger hello primära nätverket som hello första elementet i $Networks.</span><span class="sxs-lookup"><span data-stu-id="7f73e-225">hello cmdlet specifies hello primary network as hello first element of $Networks.</span></span> <span data-ttu-id="7f73e-226">hello cmdlet anger ett virtuellt datornätverk som hello första elementet i $AzureVmNetworks.</span><span class="sxs-lookup"><span data-stu-id="7f73e-226">hello cmdlet specifies a virtual machine network as hello first element of $AzureVmNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $Networks[0] -AzureVMNetworkId $AzureVmNetworks[0]

## <a name="step-9-enable-protection-for-virtual-machines"></a><span data-ttu-id="7f73e-227">Steg 9: Aktivera skydd för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="7f73e-227">Step 9: Enable protection for virtual machines</span></span>
<span data-ttu-id="7f73e-228">Du kan aktivera skydd för virtuella datorer i molnet hello när hello servrar, moln och nätverk har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="7f73e-228">After hello servers, clouds and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span>

 <span data-ttu-id="7f73e-229">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-229">Note hello following:</span></span>

* <span data-ttu-id="7f73e-230">Virtuella datorer måste uppfylla kraven för Azure.</span><span class="sxs-lookup"><span data-stu-id="7f73e-230">Virtual machines must meet Azure requirements.</span></span> <span data-ttu-id="7f73e-231">Kontrollera dessa i [krav och Support](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) i Planeringsguiden för hello.</span><span class="sxs-lookup"><span data-stu-id="7f73e-231">Check these in [Prerequisites and Support](site-recovery-support-matrix-to-azure.md#failed-over-azure-vm-requirements) in hello planning guide.</span></span>
* <span data-ttu-id="7f73e-232">tooenable skydd, hello operativsystem och operativsystem diskegenskaper måste anges för hello virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7f73e-232">tooenable protection, hello operating system and operating system disk properties must be set for hello virtual machine.</span></span> <span data-ttu-id="7f73e-233">När du skapar en virtuell dator i VMM med en mall för virtuell dator kan du ange hello-egenskapen.</span><span class="sxs-lookup"><span data-stu-id="7f73e-233">When you create a virtual machine in VMM using a virtual machine template you can set hello property.</span></span> <span data-ttu-id="7f73e-234">Du kan också ange egenskaperna för befintliga virtuella datorer på hello **allmänna** och **maskinvarukonfiguration** flikar hello egenskaperna för virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="7f73e-234">You can also set these properties for existing virtual machines on hello **General** and **Hardware Configuration** tabs of hello virtual machine properties.</span></span> <span data-ttu-id="7f73e-235">Om du inte anger dessa egenskaper i VMM kommer du att kunna tooconfigure dem i hello Azure Site Recovery-portalen.</span><span class="sxs-lookup"><span data-stu-id="7f73e-235">If you don't set these properties in VMM you'll be able tooconfigure them in hello Azure Site Recovery portal.</span></span>

1. <span data-ttu-id="7f73e-236">tooenable skydd, kör följande kommando tooget hello skyddsbehållaren hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-236">tooenable protection, run hello following command tooget hello protection container:</span></span>

          $ProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $CloudName
2. <span data-ttu-id="7f73e-237">Hämta hello skyddad entitet (VM) genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-237">Get hello protection entity (VM) by running hello following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $protectionContainer
3. <span data-ttu-id="7f73e-238">Aktivera hello DR för hello VM genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-238">Enable hello DR for hello VM by running hello following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable –Force -Policy $policy -RecoveryAzureStorageAccountId  $storageID "/subscriptions/217653172865hcvkchgvd/resourceGroups/rajanirgps/providers/Microsoft.Storage/storageAccounts/teststorageacc1

## <a name="test-your-deployment"></a><span data-ttu-id="7f73e-239">Testa distributionen</span><span class="sxs-lookup"><span data-stu-id="7f73e-239">Test your deployment</span></span>
<span data-ttu-id="7f73e-240">tootest din distribution som du kan köra ett test failover för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och kör ett test failover för hello-plan.</span><span class="sxs-lookup"><span data-stu-id="7f73e-240">tootest your deployment you can run a test fail-over for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test fail-over for hello plan.</span></span> <span data-ttu-id="7f73e-241">Test av redundansväxling simulerar din failover- och återställningsmekanismen i ett isolerat nätverk.</span><span class="sxs-lookup"><span data-stu-id="7f73e-241">Test fail-over simulates your fail-over and recovery mechanism in an isolated network.</span></span> <span data-ttu-id="7f73e-242">Tänk på följande:</span><span class="sxs-lookup"><span data-stu-id="7f73e-242">Note that:</span></span>

* <span data-ttu-id="7f73e-243">Om du vill tooconnect toohello virtuell dator i Azure med hjälp av fjärrskrivbord efter hello failover, aktivera anslutning till fjärrskrivbord på hello virtuell dator innan du kör hello testa redundans.</span><span class="sxs-lookup"><span data-stu-id="7f73e-243">If you want tooconnect toohello virtual machine in Azure using Remote Desktop after hello fail-over, enable Remote Desktop Connection on hello virtual machine before you run hello test failover.</span></span>
* <span data-ttu-id="7f73e-244">Efter redundansväxling, ska du använda en offentlig IP-adress tooconnect toohello virtuell-dator i Azure med hjälp av fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="7f73e-244">After fail-over, you'll use a public IP address tooconnect toohello virtual machine in Azure using Remote Desktop.</span></span> <span data-ttu-id="7f73e-245">Om du vill toodo detta, se till att du inte har några domänprinciper som hindrar dig från anslutande tooa virtuell dator med en offentlig adress.</span><span class="sxs-lookup"><span data-stu-id="7f73e-245">If you want toodo this, ensure you don't have any domain policies that prevent you from connecting tooa virtual machine using a public address.</span></span>

<span data-ttu-id="7f73e-246">toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="7f73e-246">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="7f73e-247">Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="7f73e-247">Run a test failover</span></span>
- <span data-ttu-id="7f73e-248">Starta hello testa redundans genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-248">Start hello test failover by running hello following command:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-a-planned-failover"></a><span data-ttu-id="7f73e-249">Kör en planerad redundans</span><span class="sxs-lookup"><span data-stu-id="7f73e-249">Run a planned failover</span></span>
- <span data-ttu-id="7f73e-250">Starta hello planerad redundans genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-250">Start hello planned failover by running hello following command:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="7f73e-251">Kör en oplanerad redundans</span><span class="sxs-lookup"><span data-stu-id="7f73e-251">Run an unplanned failover</span></span>
- <span data-ttu-id="7f73e-252">Starta hello oplanerad redundans genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="7f73e-252">Start hello unplanned failover by running hello following command:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $protectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -AzureVMNetworkId <string>  

## <span data-ttu-id="7f73e-253"><a name=monitor></a>Övervakaraktivitet</span><span class="sxs-lookup"><span data-stu-id="7f73e-253"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="7f73e-254">Använd följande kommandon toomonitor hello aktivitet hello.</span><span class="sxs-lookup"><span data-stu-id="7f73e-254">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="7f73e-255">Observera att du har toowait mellan jobb för hello bearbetning toofinish.</span><span class="sxs-lookup"><span data-stu-id="7f73e-255">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

    Do
    {
        $job = Get-AzureSiteRecoveryJob -Id $associationJob.JobId;
        Write-Host "Job State:{0}, StateDescription:{1}" -f Job.State, $job.StateDescription;
        if($job -eq $null -or $job.StateDescription -ne "Completed")
        {
            $isJobLeftForProcessing = $true;
        }

    if($isJobLeftForProcessing)
        {
            Start-Sleep -Seconds 60
        }
    }While($isJobLeftForProcessing)



## <a name="next-steps"></a><span data-ttu-id="7f73e-256">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f73e-256">Next steps</span></span>
<span data-ttu-id="7f73e-257">[Läs mer](/powershell/module/azurerm.recoveryservices.backup/#recovery) om Azure Site Recovery med Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="7f73e-257">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
