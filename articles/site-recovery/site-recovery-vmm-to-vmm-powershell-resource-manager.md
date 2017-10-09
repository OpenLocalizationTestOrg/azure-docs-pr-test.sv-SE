---
title: "aaaReplicate Hyper-V virtuella datorer i VMM tooa sekundär plats med PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Beskriver hur toodeploy Azure Site Recovery tooorchestrate replikering, redundans och återställning av virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats med hjälp av PowerShell (Resource Manager)"
services: site-recovery
documentationcenter: 
author: sujaytalasila
manager: rochakm
editor: raynew
ms.assetid: 9d38e9c3-217c-4e44-830c-575e9a4141f2
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/05/2017
ms.author: sutalasi
ms.openlocfilehash: a769dcc68d66c18b9dc47539071f4d0e0f1db70f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-tooa-secondary-vmm-site-using-powershell-resource-manager"></a><span data-ttu-id="4d2fb-103">Replikera virtuella Hyper-V-datorer i VMM-moln tooa sekundär VMM-plats med hjälp av PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="4d2fb-103">Replicate Hyper-V virtual machines in VMM clouds tooa secondary VMM site using PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="4d2fb-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="4d2fb-104">Azure Portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="4d2fb-105">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="4d2fb-105">Classic Portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="4d2fb-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="4d2fb-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="4d2fb-107">Välkommen tooAzure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="4d2fb-107">Welcome tooAzure Site Recovery!</span></span> <span data-ttu-id="4d2fb-108">Använd den här artikeln om du vill tooreplicate lokala Hyper-V virtuella datorer som hanteras i System Center Virtual Machine Manager (VMM) moln tooa sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-108">Use this article if you want tooreplicate on-premises Hyper-V  virtual machines managed in System Center Virtual Machine Manager (VMM) clouds tooa secondary site.</span></span>

<span data-ttu-id="4d2fb-109">Den här artikeln visar hur toouse PowerShell tooautomate vanliga uppgifter du behöver tooperform när du ställer in Azure Site Recovery tooreplicate Hyper-V-datorer i System Center VMM-moln tooSystem Center VMM-moln på sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-109">This article shows you how toouse PowerShell tooautomate common tasks you need tooperform when you set up Azure Site Recovery tooreplicate Hyper-V virtual machines in System Center VMM clouds tooSystem Center VMM clouds in secondary site.</span></span>

<span data-ttu-id="4d2fb-110">hello artikeln handlar om förutsättningar för hello scenariot och visar</span><span class="sxs-lookup"><span data-stu-id="4d2fb-110">hello article includes prerequisites for hello scenario, and shows you</span></span>

* <span data-ttu-id="4d2fb-111">Hur tooset upp ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="4d2fb-111">How tooset up a Recovery Services Vault</span></span>
* <span data-ttu-id="4d2fb-112">Installera hello Azure Site Recovery-providern på VMM-källservern hello och hello VMM-målservern</span><span class="sxs-lookup"><span data-stu-id="4d2fb-112">Install hello Azure Site Recovery Provider on hello source VMM server and hello target VMM server</span></span>
* <span data-ttu-id="4d2fb-113">Registrera hello VMM-servrarna i hello-valv</span><span class="sxs-lookup"><span data-stu-id="4d2fb-113">Register hello VMM server(s) in hello vault</span></span>
* <span data-ttu-id="4d2fb-114">Konfigurera replikeringsprincip för hello VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-114">Configure replication policy for hello VMM Cloud.</span></span> <span data-ttu-id="4d2fb-115">hello replikeringsinställningarna i hello princip kommer att tillämpas tooall skyddade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4d2fb-115">hello replication settings in hello policy will be applied tooall protected virtual machines</span></span>
* <span data-ttu-id="4d2fb-116">Aktivera skydd för hello virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-116">Enable protection for hello virtual machines.</span></span>
* <span data-ttu-id="4d2fb-117">Testa hello redundans för virtuella datorer separat eller som en del av en återställning planera toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-117">Test hello failover of VMs individually or as part of a recovery plan toomake sure everything is working as expected.</span></span>
* <span data-ttu-id="4d2fb-118">Utför en planerad eller oplanerad växling för virtuella datorer separat eller som en del av en återställning plan toomake att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-118">Perform a planned or an unplanned failover of VMs individually or as part of a recovery plan toomake sure everything is working as expected.</span></span>

<span data-ttu-id="4d2fb-119">Om du stöter på problem med att installera det här scenariot, publicera dina frågor på hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-119">If you run into problems setting up this scenario, post your questions on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="4d2fb-120">Azure har två olika [distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) för att skapa och arbeta med resurser: Azure Resource Manager och den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-120">Azure has two different [deployment models](../azure-resource-manager/resource-manager-deployment-model.md) for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="4d2fb-121">Azure har också två portaler – hello klassiska Azure-portalen som stöder hello klassiska distributionsmodellen och hello Azure-portalen med stöd för båda distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-121">Azure also has two portals – hello Azure classic portal that supports hello classic deployment model, and hello Azure portal with support for both deployment models.</span></span> <span data-ttu-id="4d2fb-122">Den här artikeln beskriver hello Resource Manager-modellen.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-122">This article covers hello Resource Manager deployment model.</span></span>
>
>

## <a name="on-premises-prerequisites"></a><span data-ttu-id="4d2fb-123">Krav för det lokala systemet</span><span class="sxs-lookup"><span data-stu-id="4d2fb-123">On-premises prerequisites</span></span>
<span data-ttu-id="4d2fb-124">Här är vad du behöver i hello primära och sekundära lokala platser toodeploy det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-124">Here's what you'll need in hello primary and secondary on-premises sites toodeploy this scenario:</span></span>

| <span data-ttu-id="4d2fb-125">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="4d2fb-125">**Prerequisites**</span></span> | <span data-ttu-id="4d2fb-126">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="4d2fb-126">**Details**</span></span> |
| --- | --- |
| <span data-ttu-id="4d2fb-127">**VMM**</span><span class="sxs-lookup"><span data-stu-id="4d2fb-127">**VMM**</span></span> |<span data-ttu-id="4d2fb-128">Vi rekommenderar att du distribuerar en VMM-servern i hello primär plats och en VMM-servern i hello sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-128">We recommend you deploy a VMM server in hello primary site and a VMM server in hello secondary site.</span></span><br/><br/> <span data-ttu-id="4d2fb-129">Du kan också [replikera mellan moln på en VMM-server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-129">You can also [replicate between clouds on a single VMM server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span></span> <span data-ttu-id="4d2fb-130">toodo detta behöver du minst två moln som konfigurerats på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-130">toodo this you'll need at least two clouds configured on hello VMM server.</span></span><br/><br/> <span data-ttu-id="4d2fb-131">VMM-servrar måste köra minst System Center 2012 SP1 med hello senaste uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-131">VMM servers should be running at least System Center 2012 SP1 with hello latest updates.</span></span><br/><br/> <span data-ttu-id="4d2fb-132">Varje VMM-servern måste ha en eller flera moln har konfigurerats och alla moln måste ha hello Hyper-V kapacitetsprofilen anges.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-132">Each VMM server must have at one or more clouds configured and all clouds must have hello Hyper-V Capacity profile set.</span></span> <br/><br/><span data-ttu-id="4d2fb-133">Molnen måste innehålla en eller flera VMM-värdgrupper.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-133">Clouds must contain one or more VMM host groups.</span></span><br/><br/><span data-ttu-id="4d2fb-134">Lär dig mer om hur du konfigurerar VMM-moln i [konfigurera hello VMM molnet fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), och [genomgång: skapa privata moln med System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-134">Learn more about setting up VMM clouds in [Configuring hello VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), and [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span></span><br/><br/> <span data-ttu-id="4d2fb-135">VMM-servrar ska ha tillgång till internet.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-135">VMM servers should have internet access.</span></span> |
| <span data-ttu-id="4d2fb-136">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="4d2fb-136">**Hyper-V**</span></span> |<span data-ttu-id="4d2fb-137">Hyper-V-servrar måste köra minst Windows Server 2012 med hello Hyper-V-rollen och har hello senaste uppdateringarna installerade.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-137">Hyper-V servers must be running at least Windows Server 2012 with hello Hyper-V role and have hello latest updates installed.</span></span><br/><br/> <span data-ttu-id="4d2fb-138">En Hyper-V-server måste innehålla en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-138">A Hyper-V server should contain one or more VMs.</span></span><br/><br/>  <span data-ttu-id="4d2fb-139">Hyper-V-värdservrar ska finnas i värdgrupper i hello primära och sekundära VMM-moln.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-139">Hyper-V host servers should be located in host groups in hello primary and secondary VMM clouds.</span></span><br/><br/> <span data-ttu-id="4d2fb-140">Om du kör Hyper-V i ett kluster i Windows Server 2012 R2 bör du installera [uppdatera 2961977](https://support.microsoft.com/kb/2961977)</span><span class="sxs-lookup"><span data-stu-id="4d2fb-140">If you're running Hyper-V in a cluster on Windows Server 2012 R2 you should install [update 2961977](https://support.microsoft.com/kb/2961977)</span></span><br/><br/> <span data-ttu-id="4d2fb-141">Om du kör Hyper-V i ett kluster på Windows Server 2012 Observera att klusterutjämning skapas inte automatiskt om du har en statisk IP-adress-kluster.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-141">If you're running Hyper-V in a cluster on Windows Server 2012 note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="4d2fb-142">Du behöver tooconfigure hello klusterutjämning manuellt.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-142">You'll need tooconfigure hello cluster broker manually.</span></span> <span data-ttu-id="4d2fb-143">[Läs mer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-143">[Read more](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span></span> |
| <span data-ttu-id="4d2fb-144">**Leverantör**</span><span class="sxs-lookup"><span data-stu-id="4d2fb-144">**Provider**</span></span> |<span data-ttu-id="4d2fb-145">Under distributionen av Site Recovery installerar du hello Azure Site Recovery-providern på VMM-servrar.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-145">During Site Recovery deployment you install hello Azure Site Recovery Provider on VMM servers.</span></span> <span data-ttu-id="4d2fb-146">hello providern kommunicerar med Site Recovery via HTTPS 443 tooorchestrate replikering.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-146">hello Provider communicates with Site Recovery over HTTPS 443 tooorchestrate replication.</span></span> <span data-ttu-id="4d2fb-147">Replikering sker mellan hello primära och sekundära Hyper-V-servrar över hello LAN eller VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-147">Data replication occurs between hello primary and secondary Hyper-V servers over hello LAN or a VPN connection.</span></span><br/><br/> <span data-ttu-id="4d2fb-148">hello providern som körs på hello VMM-servern måste komma åt toothese URL: er: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-148">hello Provider running on hello VMM server needs access toothese URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.</span></span><br/><br/> <span data-ttu-id="4d2fb-149">Dessutom tillåter brandväggen kommunikation från hello VMM-servrar toohello [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) och tillåta hello HTTPS (443)-protokollet.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-149">In addition allow firewall communication from hello VMM servers toohello [Azure datacenter IP ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) and allow hello HTTPS (443) protocol.</span></span> |

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="4d2fb-150">Krav för nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="4d2fb-150">Network mapping prerequisites</span></span>
<span data-ttu-id="4d2fb-151">Mappar nätverksmappningen mellan VMM VM-nätverk på hello primära och sekundära VMM-servrar till:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-151">Network mapping maps between VMM VM networks on hello primary and secondary VMM servers to:</span></span>

* <span data-ttu-id="4d2fb-152">Placera optimalt Replikdatorerna på sekundära Hyper-V-värdar efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-152">Optimally place replica VMs on secondary Hyper-V hosts after failover.</span></span>
* <span data-ttu-id="4d2fb-153">Ansluta replikering VMs tooappropriate Virtuella datornätverk.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-153">Connect replica VMs tooappropriate VM networks.</span></span>
* <span data-ttu-id="4d2fb-154">Om du inte konfigurerar nätverket mappning replik virtuella datorer inte anslutna tooany nätverket efter redundans.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-154">If you don't configure network mapping replica VMs won't be connected tooany network after failover.</span></span>
* <span data-ttu-id="4d2fb-155">Om du vill tooset nätverk är mappning under Site Recovery distribution här vad du behöver:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-155">If you want tooset up network mapping during Site Recovery deployment here's what you'll need:</span></span>

  * <span data-ttu-id="4d2fb-156">Se till att virtuella datorer på hello källan Hyper-V-värdservern är anslutna tooa VMM VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-156">Make sure that VMs on hello source Hyper-V host server are connected tooa VMM VM network.</span></span> <span data-ttu-id="4d2fb-157">Nätverket bör vara länkade tooa logiska nätverk som är kopplad till hello moln.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-157">That network should be linked tooa logical network that is associated with hello cloud.</span></span>
  * <span data-ttu-id="4d2fb-158">Kontrollera att hello sekundära moln som du vill använda för återställning har ett motsvarande Virtuellt datornätverk.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-158">Verify that hello secondary cloud that you'll use for recovery has a corresponding VM network configured.</span></span> <span data-ttu-id="4d2fb-159">Det Virtuella datornätverket ska vara länkade tooa logiskt nätverk som är kopplad till hello sekundära molnet.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-159">That VM network should be linked tooa logical network that's associated with hello secondary cloud.</span></span>

<span data-ttu-id="4d2fb-160">Lär dig mer om hur du konfigurerar nätverk i VMM i hello nedan artiklar</span><span class="sxs-lookup"><span data-stu-id="4d2fb-160">Learn more about configuring VMM networks in hello below articles</span></span>

* [<span data-ttu-id="4d2fb-161">Hur tooconfigure logiska nätverk i VMM</span><span class="sxs-lookup"><span data-stu-id="4d2fb-161">How tooconfigure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="4d2fb-162">Hur tooconfigure Virtuella nätverk och gateways i VMM</span><span class="sxs-lookup"><span data-stu-id="4d2fb-162">How tooconfigure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)

<span data-ttu-id="4d2fb-163">[Lär dig mer](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) om hur nätverksmappning fungerar.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-163">[Learn more](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) about how network mapping works.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="4d2fb-164">PowerShell-krav</span><span class="sxs-lookup"><span data-stu-id="4d2fb-164">PowerShell prerequisites</span></span>
<span data-ttu-id="4d2fb-165">Kontrollera att du har Azure PowerShell redo toogo.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-165">Make sure you have Azure PowerShell ready toogo.</span></span> <span data-ttu-id="4d2fb-166">Om du redan använder PowerShell, behöver du tooupgrade tooversion 0.8.10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-166">If you are already using PowerShell, you'll need tooupgrade tooversion 0.8.10 or later.</span></span> <span data-ttu-id="4d2fb-167">Information om hur du konfigurerar PowerShell finns i hello [Guide tooinstall och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-167">For information about setting up PowerShell, see hello [Guide tooinstall and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="4d2fb-168">När du har skapat och konfigurerat PowerShell, kan du visa alla hello tillgängliga cmdlets för hello tjänsten [här](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-168">Once you have set up and configured PowerShell, you can view all of hello available cmdlets for hello service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="4d2fb-169">toolearn om tips som kan hjälpa dig att använda hello-cmdlets, till exempel hur parametervärden indata och utdata hanteras vanligtvis av Azure PowerShell finns hello [Guide tooget igång med Azure-Cmdlets](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-169">toolearn about tips that can help you use hello cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see hello [Guide tooget Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-hello-subscription"></a><span data-ttu-id="4d2fb-170">Steg 1: Ange hello prenumeration</span><span class="sxs-lookup"><span data-stu-id="4d2fb-170">Step 1: Set hello subscription</span></span>
1. <span data-ttu-id="4d2fb-171">Från Azure powershell, inloggning tooyour Azure-konto: med hello följande cmdlet: ar</span><span class="sxs-lookup"><span data-stu-id="4d2fb-171">From Azure powershell, login tooyour Azure account: using hello following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="4d2fb-172">Hämta en lista över dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-172">Get a list of your subscriptions.</span></span> <span data-ttu-id="4d2fb-173">Detta visar också en lista över hello subscriptionIDs för varje hello prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-173">This will also list hello subscriptionIDs for each of hello subscriptions.</span></span> <span data-ttu-id="4d2fb-174">Notera hello prenumerations-ID för hello prenumeration där du vill toocreate hello recovery services-valvet</span><span class="sxs-lookup"><span data-stu-id="4d2fb-174">Note down hello subscriptionID of hello subscription in which you wish toocreate hello recovery services vault</span></span>    

        Get-AzureRmSubscription
3. <span data-ttu-id="4d2fb-175">Ange hello prenumeration i vilka hello recovery services-ventilen är toobe som skapats av nämna hello prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="4d2fb-175">Set hello subscription in which hello recovery services vault is toobe created by mentioning hello subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="4d2fb-176">Steg 2: Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="4d2fb-176">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="4d2fb-177">Skapa en resursgrupp i Azure Resource Manager om du inte redan har en</span><span class="sxs-lookup"><span data-stu-id="4d2fb-177">Create an Azure Resource Manager resource group if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="4d2fb-178">Skapa ett nytt Recovery Services-valv och spara hello skapade ASR valvet objekt i en variabel (kommer att användas senare).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-178">Create a new Recovery Services vault and save hello created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="4d2fb-179">Du kan också hämta hello ASR valvet post skapa objektet med hello Get-AzureRMRecoveryServicesVault cmdlet:-</span><span class="sxs-lookup"><span data-stu-id="4d2fb-179">You can also retrieve hello ASR vault object post creation using hello Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-hello-recovery-services-vault-context"></a><span data-ttu-id="4d2fb-180">Steg 3: Ange hello Recovery Services-ventilen kontext</span><span class="sxs-lookup"><span data-stu-id="4d2fb-180">Step 3: Set hello Recovery Services Vault context</span></span>
1. <span data-ttu-id="4d2fb-181">Om du har ett valv som redan har skapats, kör du hello nedan kommandot tooget hello valvet.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-181">If you have a vault already created, run hello below command tooget hello vault.</span></span>

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. <span data-ttu-id="4d2fb-182">Ange hello valvet kontexten genom att köra hello nedan kommando.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-182">Set hello vault context by running hello below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-hello-azure-site-recovery-provider"></a><span data-ttu-id="4d2fb-183">Steg 4: Installera hello Azure Site Recovery Provider</span><span class="sxs-lookup"><span data-stu-id="4d2fb-183">Step 4: Install hello Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="4d2fb-184">Skapa en katalog på hello VMM-datorn genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-184">On hello VMM machine, create a directory by running hello following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="4d2fb-185">Extrahera hello filer med hjälp av hello ned providern genom att köra följande kommando hello</span><span class="sxs-lookup"><span data-stu-id="4d2fb-185">Extract hello files using hello downloaded provider by running hello following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="4d2fb-186">Installera hello-providern med hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-186">Install hello provider using hello following commands:</span></span>

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

   <span data-ttu-id="4d2fb-187">Vänta tills hello installation toofinish.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-187">Wait for hello installation toofinish.</span></span>
4. <span data-ttu-id="4d2fb-188">Registrera hello servern i hello valvet med hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-188">Register hello server in hello vault using hello following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a><span data-ttu-id="4d2fb-189">Steg 5: Skapa och koppla en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="4d2fb-189">Step 5: Create and associate a replication policy</span></span>
1. <span data-ttu-id="4d2fb-190">Skapa en replikeringsprincip för Hyper-V 2012 R2 genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-190">Create a Hyper-V 2012 R2 replication policy by running hello following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify hello number of hours tooretain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify hello frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify hello port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > <span data-ttu-id="4d2fb-191">hello VMM-moln kan innehålla Hyper-V-värdar som kör olika versioner av Windows Server (som anges i hello Hyper-V-krav), men hello replikeringsprincip är OS-version som är specifika.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-191">hello VMM cloud can contain Hyper-V hosts running different versions of Windows Server (as mentioned in hello Hyper-V prerequisites), but hello replication policy is OS version specific.</span></span> <span data-ttu-id="4d2fb-192">Om du har olika värdar som körs på olika operativsystemversioner skapa separata replikeringsprinciper för varje typ av OS-version.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-192">If you have different hosts running on different operating system versions, then create separate replication policies for each type of OS version.</span></span> <span data-ttu-id="4d2fb-193">För t.ex: Om du har fem värdar som kör Windows 2012 för servrar och tre på Windows Server 2012 R2 kan du skapa två principer för replikering – en för varje typ av versioner av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-193">For eg: If you have five hosts running on Windows Servers 2012 and three on Windows Server 2012 R2, create two replication polices – one for each type of operating system versions.</span></span>

1. <span data-ttu-id="4d2fb-194">Hämta hello primära skyddsbehållaren (primära VMM-moln) och återställning skyddsbehållaren (återställning VMM-moln) genom att köra följande kommandon hello:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-194">Get hello primary protection container (primary VMM Cloud) and recovery protection container (recovery VMM Cloud) by running hello following commands:</span></span>

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. <span data-ttu-id="4d2fb-195">Hämta hello-principen som du skapade i steg 1 med hjälp av hello eget namn för hello-princip</span><span class="sxs-lookup"><span data-stu-id="4d2fb-195">Retrieve hello policy you created in step 1 using hello friendly name of hello policy</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="4d2fb-196">Starta hello associering av hello skyddsbehållaren (VMM-moln) med hello replikeringsprincip:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-196">Start hello association of hello protection container (VMM Cloud) with hello replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. <span data-ttu-id="4d2fb-197">Vänta tills hello princip association jobbet toocomplete.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-197">Wait for hello policy association job toocomplete.</span></span> <span data-ttu-id="4d2fb-198">Du kan kontrollera om hello jobbet har slutförts med hello följande PowerShell-fragment.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-198">You can check if hello job has completed using hello following PowerShell snippet.</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   <span data-ttu-id="4d2fb-199">När hello jobbet har bearbetat köra hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-199">After hello job has finished processing, run hello following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="4d2fb-200">toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-200">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-6-configure-network-mapping"></a><span data-ttu-id="4d2fb-201">Steg 6: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="4d2fb-201">Step 6: Configure network mapping</span></span>
1. <span data-ttu-id="4d2fb-202">hello första kommandot hämtar servrar för hello aktuella Azure Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-202">hello first command gets servers for hello current Azure Site Recovery vault.</span></span> <span data-ttu-id="4d2fb-203">hello kommandot lagrar hello Microsoft Azure Site Recovery-servrar i hello $Servers matrisvariabel.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-203">hello command stores hello Microsoft Azure Site Recovery servers in hello $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="4d2fb-204">hello nedan kommandon hämta hello site recovery nätverk för VMM-källservern hello och hello VMM-målservern.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-204">hello below commands get hello site recovery network for hello source VMM server and hello target VMM server.</span></span>

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > <span data-ttu-id="4d2fb-205">hello VMM-källservern kan vara hello första eller hello andra en i hello servrar matris.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-205">hello source VMM server can be hello first one or hello second one in hello servers array.</span></span> <span data-ttu-id="4d2fb-206">Kontrollera hello namnen på hello VMM-servrar och hämta hello nätverk på rätt sätt</span><span class="sxs-lookup"><span data-stu-id="4d2fb-206">Check hello names of hello VMM servers and get hello networks appropriately</span></span>


1. <span data-ttu-id="4d2fb-207">hello slutliga cmdlet skapar en mappning mellan hello primära nätverk och hello recovery nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-207">hello final cmdlet creates a mapping between hello primary network and hello recovery network.</span></span> <span data-ttu-id="4d2fb-208">hello cmdlet anger hello primära nätverket som hello första elementet i $PrimaryNetworks och hello recovery nätverk som hello första elementet i $RecoveryNetworks.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-208">hello cmdlet specifies hello primary network as hello first element of $PrimaryNetworks and hello recovery network as hello first element of $RecoveryNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a><span data-ttu-id="4d2fb-209">Steg 7: Konfigurera lagring-mappning</span><span class="sxs-lookup"><span data-stu-id="4d2fb-209">Step 7: Configure storage mapping</span></span>
1. <span data-ttu-id="4d2fb-210">hello nedan kommando hämtar hello lista med lagringsklassificeringar i $storageclassifications variabel.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-210">hello below command gets hello list of storage classifications into $storageclassifications variable.</span></span>

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. <span data-ttu-id="4d2fb-211">hello nedan kommandon hämta hello källa klassificeringen i $SourceClassificaion variabeln och mål klassificering i $TargetClassification variabel.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-211">hello below commands get hello source classification into $SourceClassificaion variable and target classification into $TargetClassification variable.</span></span>

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > <span data-ttu-id="4d2fb-212">hello klassificeringar för källa och mål kan vara ett element i matrisen hello.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-212">hello source and target classifications can be any element in hello array.</span></span> <span data-ttu-id="4d2fb-213">Läs toohello utdata från hello nedan kommandot toofigure hello index för käll- och klassificeringar på $storageclassifications matris.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-213">Refer toohello output of hello below command toofigure hello index of source and target classifications in $storageclassifications array.</span></span>

    > <span data-ttu-id="4d2fb-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object - egenskapen FriendlyName, Id | Formatera tabell</span><span class="sxs-lookup"><span data-stu-id="4d2fb-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object -Property FriendlyName, Id | Format-Table</span></span>


1. <span data-ttu-id="4d2fb-215">hello nedan cmdlet skapar en mappning mellan hello källa klassificering och hello mål klassificering.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-215">hello below cmdlet creates a mapping between hello source classification and hello target classification.</span></span>

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a><span data-ttu-id="4d2fb-216">Steg 8: Aktivera skydd för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="4d2fb-216">Step 8: Enable protection for virtual machines</span></span>
<span data-ttu-id="4d2fb-217">Du kan aktivera skydd för virtuella datorer i molnet hello när hello servrar, moln och nätverk har konfigurerats korrekt.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-217">After hello servers, clouds and networks are configured correctly, you can enable protection for virtual machines in hello cloud.</span></span>

1. <span data-ttu-id="4d2fb-218">tooenable skydd, kör följande kommando tooget hello skyddsbehållaren hello:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-218">tooenable protection, run hello following command tooget hello protection container:</span></span>

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. <span data-ttu-id="4d2fb-219">Hämta hello skyddad entitet (VM) genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-219">Get hello protection entity (VM) by running hello following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. <span data-ttu-id="4d2fb-220">Aktivera replikering för hello VM genom att köra följande kommando hello:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-220">Enable replication for hello VM by running hello following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a><span data-ttu-id="4d2fb-221">Testa distributionen</span><span class="sxs-lookup"><span data-stu-id="4d2fb-221">Test your deployment</span></span>
<span data-ttu-id="4d2fb-222">tootest planera distributionen av du kan köra ett redundanstest för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och köra ett redundanstest för hello.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-222">tootest your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for hello plan.</span></span> <span data-ttu-id="4d2fb-223">Ett redundanstest simulerar redundans- och återställningsmekanismen i ett isolerat nätverk.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-223">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span>

> [!NOTE]
> <span data-ttu-id="4d2fb-224">Du kan skapa en återställningsplan för ditt program i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-224">You can create a recovery plan for your application in Azure portal.</span></span>
>
>

<span data-ttu-id="4d2fb-225">toocheck hello slutföra hello åtgärden gör hello i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="4d2fb-225">toocheck hello completion of hello operation, follow hello steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="4d2fb-226">Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="4d2fb-226">Run a test failover</span></span>
1. <span data-ttu-id="4d2fb-227">Kör hello nedan cmdlets tooget hello Virtuella nätverk toowhich du tootest redundans dina virtuella datorer till.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-227">Run hello below cmdlets tooget hello VM network toowhich you want tootest failover your VMs to.</span></span>

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. <span data-ttu-id="4d2fb-228">Utför ett redundanstest av en virtuell dator genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-228">Perform a test failover of a VM by doing hello following:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. <span data-ttu-id="4d2fb-229">Utför ett redundanstest av en återställningsplan hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-229">Perform a test failover of a recovery plan by doing hello following:</span></span>

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a><span data-ttu-id="4d2fb-230">Kör en planerad redundans</span><span class="sxs-lookup"><span data-stu-id="4d2fb-230">Run a planned failover</span></span>
1. <span data-ttu-id="4d2fb-231">Utför en planerad redundansväxling av en virtuell dator genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-231">Perform a planned failover of a VM by doing hello following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. <span data-ttu-id="4d2fb-232">Utför en planerad redundansväxling av en återställningsplan hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-232">Perform a planned failover of a recovery plan by doing hello following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="4d2fb-233">Kör en oplanerad redundans</span><span class="sxs-lookup"><span data-stu-id="4d2fb-233">Run an unplanned failover</span></span>
1. <span data-ttu-id="4d2fb-234">Utföra en oplanerad redundans för en virtuell dator genom att göra hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-234">Perform an unplanned failover of a VM by doing hello following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

<span data-ttu-id="4d2fb-235">2. utföra en oplanerad redundans för en återställningsplan hello följande:</span><span class="sxs-lookup"><span data-stu-id="4d2fb-235">2.Perform an unplanned failover of a recovery plan by doing hello following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <span data-ttu-id="4d2fb-236"><a name=monitor></a>Övervakaraktivitet</span><span class="sxs-lookup"><span data-stu-id="4d2fb-236"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="4d2fb-237">Använd följande kommandon toomonitor hello aktivitet hello.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-237">Use hello following commands toomonitor hello activity.</span></span> <span data-ttu-id="4d2fb-238">Observera att du har toowait mellan jobb för hello bearbetning toofinish.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-238">Note that you have toowait in between jobs for hello processing toofinish.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="4d2fb-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="4d2fb-239">Next steps</span></span>
<span data-ttu-id="4d2fb-240">[Läs mer](/powershell/module/azurerm.recoveryservices.backup/#recovery) om Azure Site Recovery med Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="4d2fb-240">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
