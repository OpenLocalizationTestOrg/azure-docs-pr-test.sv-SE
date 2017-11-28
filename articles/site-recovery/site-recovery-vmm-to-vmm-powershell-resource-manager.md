---
title: "Replikera virtuella Hyper-V-datorer i VMM till en sekundär plats med PowerShell (Azure Resource Manager) | Microsoft Docs"
description: "Beskriver hur du distribuerar Azure Site Recovery för att samordna replikering, redundans och återställning av Hyper-V virtuella datorer i VMM-moln till en sekundär VMM-plats med hjälp av PowerShell (Resource Manager)"
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
ms.openlocfilehash: 5a6e00877b0a2b139d5322f610c1901ad76a710f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="replicate-hyper-v-virtual-machines-in-vmm-clouds-to-a-secondary-vmm-site-using-powershell-resource-manager"></a><span data-ttu-id="c03f5-103">Replikera virtuella Hyper-V-datorer i VMM-moln till en sekundär VMM-plats med hjälp av PowerShell (Resource Manager)</span><span class="sxs-lookup"><span data-stu-id="c03f5-103">Replicate Hyper-V virtual machines in VMM clouds to a secondary VMM site using PowerShell (Resource Manager)</span></span>
> [!div class="op_single_selector"]
> * [<span data-ttu-id="c03f5-104">Azure Portal</span><span class="sxs-lookup"><span data-stu-id="c03f5-104">Azure Portal</span></span>](site-recovery-vmm-to-vmm.md)
> * [<span data-ttu-id="c03f5-105">Klassisk portal</span><span class="sxs-lookup"><span data-stu-id="c03f5-105">Classic Portal</span></span>](site-recovery-vmm-to-vmm-classic.md)
> * [<span data-ttu-id="c03f5-106">PowerShell – Resource Manager</span><span class="sxs-lookup"><span data-stu-id="c03f5-106">PowerShell - Resource Manager</span></span>](site-recovery-vmm-to-vmm-powershell-resource-manager.md)
>
>

<span data-ttu-id="c03f5-107">Välkommen till Azure Site Recovery!</span><span class="sxs-lookup"><span data-stu-id="c03f5-107">Welcome to Azure Site Recovery!</span></span> <span data-ttu-id="c03f5-108">Använd den här artikeln om du vill replikera lokala virtuella Hyper-V-datorer hanteras i System Center Virtual Machine Manager (VMM)-moln till en sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="c03f5-108">Use this article if you want to replicate on-premises Hyper-V  virtual machines managed in System Center Virtual Machine Manager (VMM) clouds to a secondary site.</span></span>

<span data-ttu-id="c03f5-109">Den här artikeln visar hur du använder PowerShell för att automatisera vanliga uppgifter som du behöver utföra när du ställer in Azure Site Recovery replikera Hyper-V virtuella datorer i System Center VMM-moln till System Center VMM-moln på sekundär plats.</span><span class="sxs-lookup"><span data-stu-id="c03f5-109">This article shows you how to use PowerShell to automate common tasks you need to perform when you set up Azure Site Recovery to replicate Hyper-V virtual machines in System Center VMM clouds to System Center VMM clouds in secondary site.</span></span>

<span data-ttu-id="c03f5-110">Artikeln innehåller kraven för scenariot och visar</span><span class="sxs-lookup"><span data-stu-id="c03f5-110">The article includes prerequisites for the scenario, and shows you</span></span>

* <span data-ttu-id="c03f5-111">Hur du ställer in ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="c03f5-111">How to set up a Recovery Services Vault</span></span>
* <span data-ttu-id="c03f5-112">Installera Azure Site Recovery-providern på VMM-källservern och målservern för VMM</span><span class="sxs-lookup"><span data-stu-id="c03f5-112">Install the Azure Site Recovery Provider on the source VMM server and the target VMM server</span></span>
* <span data-ttu-id="c03f5-113">Registrera VMM-servrar i valvet</span><span class="sxs-lookup"><span data-stu-id="c03f5-113">Register the VMM server(s) in the vault</span></span>
* <span data-ttu-id="c03f5-114">Konfigurera princip för lösenordsreplikering för VMM-molnet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-114">Configure replication policy for the VMM Cloud.</span></span> <span data-ttu-id="c03f5-115">Replikeringsinställningarna i principen tillämpas på alla skyddade virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c03f5-115">The replication settings in the policy will be applied to all protected virtual machines</span></span>
* <span data-ttu-id="c03f5-116">Aktivera skydd för virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c03f5-116">Enable protection for the virtual machines.</span></span>
* <span data-ttu-id="c03f5-117">Testa redundans för virtuella datorer separat eller som en del av en återställningsplan för att kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="c03f5-117">Test the failover of VMs individually or as part of a recovery plan to make sure everything is working as expected.</span></span>
* <span data-ttu-id="c03f5-118">Utför en planerad eller oplanerad växling för virtuella datorer separat eller som en del av en återställningsplan för att kontrollera att allt fungerar som förväntat.</span><span class="sxs-lookup"><span data-stu-id="c03f5-118">Perform a planned or an unplanned failover of VMs individually or as part of a recovery plan to make sure everything is working as expected.</span></span>

<span data-ttu-id="c03f5-119">Om du stöter på problem med att installera det här scenariot, publicera dina frågor på den [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="c03f5-119">If you run into problems setting up this scenario, post your questions on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

> [!NOTE]
> <span data-ttu-id="c03f5-120">Azure har två olika [distributionsmodeller](../azure-resource-manager/resource-manager-deployment-model.md) för att skapa och arbeta med resurser: Azure Resource Manager och den klassiska distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-120">Azure has two different [deployment models](../azure-resource-manager/resource-manager-deployment-model.md) for creating and working with resources: Azure Resource Manager and classic.</span></span> <span data-ttu-id="c03f5-121">Azure har också två portaler – den klassiska Azure-portalen som stöder den klassiska distributionsmodellen och Azure-portalen som stöder båda distributionsmodellerna.</span><span class="sxs-lookup"><span data-stu-id="c03f5-121">Azure also has two portals – the Azure classic portal that supports the classic deployment model, and the Azure portal with support for both deployment models.</span></span> <span data-ttu-id="c03f5-122">Den här artikeln beskriver Resource Manager-distributionsmodellen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-122">This article covers the Resource Manager deployment model.</span></span>
>
>

## <a name="on-premises-prerequisites"></a><span data-ttu-id="c03f5-123">Krav för det lokala systemet</span><span class="sxs-lookup"><span data-stu-id="c03f5-123">On-premises prerequisites</span></span>
<span data-ttu-id="c03f5-124">Här är vad du behöver i de primära och sekundära lokala platserna distribuera det här scenariot:</span><span class="sxs-lookup"><span data-stu-id="c03f5-124">Here's what you'll need in the primary and secondary on-premises sites to deploy this scenario:</span></span>

| <span data-ttu-id="c03f5-125">**Förutsättningar**</span><span class="sxs-lookup"><span data-stu-id="c03f5-125">**Prerequisites**</span></span> | <span data-ttu-id="c03f5-126">**Detaljer**</span><span class="sxs-lookup"><span data-stu-id="c03f5-126">**Details**</span></span> |
| --- | --- |
| <span data-ttu-id="c03f5-127">**VMM**</span><span class="sxs-lookup"><span data-stu-id="c03f5-127">**VMM**</span></span> |<span data-ttu-id="c03f5-128">Vi rekommenderar att du distribuerar en VMM-server på den primära platsen och en VMM-servern på den sekundära platsen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-128">We recommend you deploy a VMM server in the primary site and a VMM server in the secondary site.</span></span><br/><br/> <span data-ttu-id="c03f5-129">Du kan också [replikera mellan moln på en VMM-server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span><span class="sxs-lookup"><span data-stu-id="c03f5-129">You can also [replicate between clouds on a single VMM server](site-recovery-vmm-to-vmm.md#prepare-for-single-server-deployment).</span></span> <span data-ttu-id="c03f5-130">Om du vill göra detta behöver du minst två moln som konfigurerats på VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="c03f5-130">To do this you'll need at least two clouds configured on the VMM server.</span></span><br/><br/> <span data-ttu-id="c03f5-131">VMM-servrar måste köra minst System Center 2012 SP1 med de senaste uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="c03f5-131">VMM servers should be running at least System Center 2012 SP1 with the latest updates.</span></span><br/><br/> <span data-ttu-id="c03f5-132">Varje VMM-server måste ha en eller flera moln har konfigurerats och alla moln måste ha Hyper-V kapacitetsprofilen ange.</span><span class="sxs-lookup"><span data-stu-id="c03f5-132">Each VMM server must have at one or more clouds configured and all clouds must have the Hyper-V Capacity profile set.</span></span> <br/><br/><span data-ttu-id="c03f5-133">Molnen måste innehålla en eller flera VMM-värdgrupper.</span><span class="sxs-lookup"><span data-stu-id="c03f5-133">Clouds must contain one or more VMM host groups.</span></span><br/><br/><span data-ttu-id="c03f5-134">Lär dig mer om hur du konfigurerar VMM-moln i [konfigurera VMM molnet fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), och [genomgång: skapa privata moln med System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span><span class="sxs-lookup"><span data-stu-id="c03f5-134">Learn more about setting up VMM clouds in [Configuring the VMM cloud fabric](https://msdn.microsoft.com/library/azure/dn469075.aspx#BKMK_Fabric), and [Walkthrough: Creating private clouds with System Center 2012 SP1 VMM](http://blogs.technet.com/b/keithmayer/archive/2013/04/18/walkthrough-creating-private-clouds-with-system-center-2012-sp1-virtual-machine-manager-build-your-private-cloud-in-a-month.aspx).</span></span><br/><br/> <span data-ttu-id="c03f5-135">VMM-servrar ska ha tillgång till internet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-135">VMM servers should have internet access.</span></span> |
| <span data-ttu-id="c03f5-136">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="c03f5-136">**Hyper-V**</span></span> |<span data-ttu-id="c03f5-137">Hyper-V-servrar måste köra minst Windows Server 2012 med Hyper-V-rollen och har installerat de senaste uppdateringarna.</span><span class="sxs-lookup"><span data-stu-id="c03f5-137">Hyper-V servers must be running at least Windows Server 2012 with the Hyper-V role and have the latest updates installed.</span></span><br/><br/> <span data-ttu-id="c03f5-138">En Hyper-V-server måste innehålla en eller flera virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c03f5-138">A Hyper-V server should contain one or more VMs.</span></span><br/><br/>  <span data-ttu-id="c03f5-139">Hyper-V-värdservrar ska finnas i värdgrupper i de primära och sekundära VMM-molnen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-139">Hyper-V host servers should be located in host groups in the primary and secondary VMM clouds.</span></span><br/><br/> <span data-ttu-id="c03f5-140">Om du kör Hyper-V i ett kluster i Windows Server 2012 R2 bör du installera [uppdatera 2961977](https://support.microsoft.com/kb/2961977)</span><span class="sxs-lookup"><span data-stu-id="c03f5-140">If you're running Hyper-V in a cluster on Windows Server 2012 R2 you should install [update 2961977](https://support.microsoft.com/kb/2961977)</span></span><br/><br/> <span data-ttu-id="c03f5-141">Om du kör Hyper-V i ett kluster på Windows Server 2012 Observera att klusterutjämning skapas inte automatiskt om du har en statisk IP-adress-kluster.</span><span class="sxs-lookup"><span data-stu-id="c03f5-141">If you're running Hyper-V in a cluster on Windows Server 2012 note that cluster broker isn't created automatically if you have a static IP address-based cluster.</span></span> <span data-ttu-id="c03f5-142">Du måste konfigurera klusterutjämningen manuellt.</span><span class="sxs-lookup"><span data-stu-id="c03f5-142">You'll need to configure the cluster broker manually.</span></span> <span data-ttu-id="c03f5-143">[Läs mer](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span><span class="sxs-lookup"><span data-stu-id="c03f5-143">[Read more](http://social.technet.microsoft.com/wiki/contents/articles/18792.configure-replica-broker-role-cluster-to-cluster-replication.aspx).</span></span> |
| <span data-ttu-id="c03f5-144">**Leverantör**</span><span class="sxs-lookup"><span data-stu-id="c03f5-144">**Provider**</span></span> |<span data-ttu-id="c03f5-145">Under distributionen av Site Recovery installerar du Azure Site Recovery-providern på VMM-servrar.</span><span class="sxs-lookup"><span data-stu-id="c03f5-145">During Site Recovery deployment you install the Azure Site Recovery Provider on VMM servers.</span></span> <span data-ttu-id="c03f5-146">Providern kommunicerar med Site Recovery via HTTPS 443 för att samordna replikeringen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-146">The Provider communicates with Site Recovery over HTTPS 443 to orchestrate replication.</span></span> <span data-ttu-id="c03f5-147">Datareplikering sker mellan primära och sekundära Hyper-V-servrar över LAN- eller en VPN-anslutning.</span><span class="sxs-lookup"><span data-stu-id="c03f5-147">Data replication occurs between the primary and secondary Hyper-V servers over the LAN or a VPN connection.</span></span><br/><br/> <span data-ttu-id="c03f5-148">Providern som körs på VMM-servern behöver åtkomst till dessa webbadresser: *. hypervrecoverymanager.windowsazure.com; *. accesscontrol.windows.net; *. backup.windowsazure.com; *. blob.core.windows.net; *. store.core.windows.net.</span><span class="sxs-lookup"><span data-stu-id="c03f5-148">The Provider running on the VMM server needs access to these URLs: *.hypervrecoverymanager.windowsazure.com; *.accesscontrol.windows.net; *.backup.windowsazure.com; *.blob.core.windows.net; *.store.core.windows.net.</span></span><br/><br/> <span data-ttu-id="c03f5-149">Dessutom tillåter brandväggen kommunikation från VMM-servrar för att den [IP-adressintervall för Azure-datacenter](https://www.microsoft.com/download/confirmation.aspx?id=41653) och tillåta protokollet HTTPS (443).</span><span class="sxs-lookup"><span data-stu-id="c03f5-149">In addition allow firewall communication from the VMM servers to the [Azure datacenter IP ranges](https://www.microsoft.com/download/confirmation.aspx?id=41653) and allow the HTTPS (443) protocol.</span></span> |

### <a name="network-mapping-prerequisites"></a><span data-ttu-id="c03f5-150">Krav för nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="c03f5-150">Network mapping prerequisites</span></span>
<span data-ttu-id="c03f5-151">Mappar nätverksmappningen mellan VMM VM-nätverk på de primära och sekundära VMM-servrarna till:</span><span class="sxs-lookup"><span data-stu-id="c03f5-151">Network mapping maps between VMM VM networks on the primary and secondary VMM servers to:</span></span>

* <span data-ttu-id="c03f5-152">Placera optimalt Replikdatorerna på sekundära Hyper-V-värdar efter växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="c03f5-152">Optimally place replica VMs on secondary Hyper-V hosts after failover.</span></span>
* <span data-ttu-id="c03f5-153">Ansluta Replikdatorerna till lämpliga VM-nätverk.</span><span class="sxs-lookup"><span data-stu-id="c03f5-153">Connect replica VMs to appropriate VM networks.</span></span>
* <span data-ttu-id="c03f5-154">Om du inte konfigurerar nätverket mappning replik virtuella datorer inte ansluts till något nätverk efter redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="c03f5-154">If you don't configure network mapping replica VMs won't be connected to any network after failover.</span></span>
* <span data-ttu-id="c03f5-155">Om du vill konfigurera nätverk är mappning under Site Recovery distributionen här vad du behöver:</span><span class="sxs-lookup"><span data-stu-id="c03f5-155">If you want to set up network mapping during Site Recovery deployment here's what you'll need:</span></span>

  * <span data-ttu-id="c03f5-156">Se till att de virtuella datorerna på Hyper-V-källvärdservern är anslutna till ett VM-nätverk i VMM.</span><span class="sxs-lookup"><span data-stu-id="c03f5-156">Make sure that VMs on the source Hyper-V host server are connected to a VMM VM network.</span></span> <span data-ttu-id="c03f5-157">Nätverket ska kopplas till ett logiskt nätverk som är associerat med molnet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-157">That network should be linked to a logical network that is associated with the cloud.</span></span>
  * <span data-ttu-id="c03f5-158">Kontrollera att det sekundära moln som du vill använda för återställning har ett motsvarande Virtuellt datornätverk.</span><span class="sxs-lookup"><span data-stu-id="c03f5-158">Verify that the secondary cloud that you'll use for recovery has a corresponding VM network configured.</span></span> <span data-ttu-id="c03f5-159">Det Virtuella datornätverket ska kopplas till ett logiskt nätverk som är associerad med det sekundära molnet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-159">That VM network should be linked to a logical network that's associated with the secondary cloud.</span></span>

<span data-ttu-id="c03f5-160">Lär dig mer om hur du konfigurerar VMM-nätverk i den nedan artiklar</span><span class="sxs-lookup"><span data-stu-id="c03f5-160">Learn more about configuring VMM networks in the below articles</span></span>

* [<span data-ttu-id="c03f5-161">Konfigurera logiska nätverk i VMM</span><span class="sxs-lookup"><span data-stu-id="c03f5-161">How to configure logical networks in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386307)
* [<span data-ttu-id="c03f5-162">Konfigurera Virtuella datornätverk och gateways i VMM</span><span class="sxs-lookup"><span data-stu-id="c03f5-162">How to configure VM networks and gateways in VMM</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=386308)

<span data-ttu-id="c03f5-163">[Lär dig mer](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) om hur nätverksmappning fungerar.</span><span class="sxs-lookup"><span data-stu-id="c03f5-163">[Learn more](site-recovery-vmm-to-vmm.md#prepare-for-network-mapping) about how network mapping works.</span></span>

### <a name="powershell-prerequisites"></a><span data-ttu-id="c03f5-164">PowerShell-krav</span><span class="sxs-lookup"><span data-stu-id="c03f5-164">PowerShell prerequisites</span></span>
<span data-ttu-id="c03f5-165">Kontrollera att du har Azure PowerShell gå vidare.</span><span class="sxs-lookup"><span data-stu-id="c03f5-165">Make sure you have Azure PowerShell ready to go.</span></span> <span data-ttu-id="c03f5-166">Om du redan använder PowerShell, måste du uppgradera till version 0.8.10 eller senare.</span><span class="sxs-lookup"><span data-stu-id="c03f5-166">If you are already using PowerShell, you'll need to upgrade to version 0.8.10 or later.</span></span> <span data-ttu-id="c03f5-167">Information om hur du konfigurerar PowerShell finns i [Guide för att installera och konfigurera Azure PowerShell](/powershell/azureps-cmdlets-docs).</span><span class="sxs-lookup"><span data-stu-id="c03f5-167">For information about setting up PowerShell, see the [Guide to install and configure Azure PowerShell](/powershell/azureps-cmdlets-docs).</span></span> <span data-ttu-id="c03f5-168">När du har skapat och konfigurerat PowerShell, kan du visa alla tillgängliga cmdlets för tjänsten [här](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="c03f5-168">Once you have set up and configured PowerShell, you can view all of the available cmdlets for the service [here](/powershell/azure/overview).</span></span>

<span data-ttu-id="c03f5-169">Mer information om tips som kan du använda cmdlet: ar, till exempel hur parametervärden indata och utdata hanteras vanligtvis av Azure PowerShell finns i [Guide för att komma igång med Azure-Cmdlets](/powershell/azure/get-started-azureps).</span><span class="sxs-lookup"><span data-stu-id="c03f5-169">To learn about tips that can help you use the cmdlets, such as how parameter values, inputs, and outputs are typically handled in Azure PowerShell, see the [Guide to get Started with Azure Cmdlets](/powershell/azure/get-started-azureps).</span></span>

## <a name="step-1-set-the-subscription"></a><span data-ttu-id="c03f5-170">Steg 1: Ställ in prenumerationen</span><span class="sxs-lookup"><span data-stu-id="c03f5-170">Step 1: Set the subscription</span></span>
1. <span data-ttu-id="c03f5-171">Från Azure powershell, logga in på ditt Azure-konto: använda följande cmdlets</span><span class="sxs-lookup"><span data-stu-id="c03f5-171">From Azure powershell, login to your Azure account: using the following cmdlets</span></span>

        $UserName = "<user@live.com>"
        $Password = "<password>"
        $SecurePassword = ConvertTo-SecureString -AsPlainText $Password -Force
        $Cred = New-Object System.Management.Automation.PSCredential -ArgumentList $UserName, $SecurePassword
        Login-AzureRmAccount #-Credential $Cred
2. <span data-ttu-id="c03f5-172">Hämta en lista över dina prenumerationer.</span><span class="sxs-lookup"><span data-stu-id="c03f5-172">Get a list of your subscriptions.</span></span> <span data-ttu-id="c03f5-173">Detta visar också en lista över subscriptionIDs för var och en av prenumerationerna.</span><span class="sxs-lookup"><span data-stu-id="c03f5-173">This will also list the subscriptionIDs for each of the subscriptions.</span></span> <span data-ttu-id="c03f5-174">Anteckna prenumerations-ID för prenumerationen som du vill skapa recovery services-valvet</span><span class="sxs-lookup"><span data-stu-id="c03f5-174">Note down the subscriptionID of the subscription in which you wish to create the recovery services vault</span></span>    

        Get-AzureRmSubscription
3. <span data-ttu-id="c03f5-175">Ange den prenumeration som recovery services-ventilen är skapas av nämna prenumerations-ID</span><span class="sxs-lookup"><span data-stu-id="c03f5-175">Set the subscription in which the recovery services vault is to be created by mentioning the subscription ID</span></span>

        Set-AzureRmContext –SubscriptionID <subscriptionId>

## <a name="step-2-create-a-recovery-services-vault"></a><span data-ttu-id="c03f5-176">Steg 2: Skapa ett Recovery Services-valv</span><span class="sxs-lookup"><span data-stu-id="c03f5-176">Step 2: Create a Recovery Services vault</span></span>
1. <span data-ttu-id="c03f5-177">Skapa en resursgrupp i Azure Resource Manager om du inte redan har en</span><span class="sxs-lookup"><span data-stu-id="c03f5-177">Create an Azure Resource Manager resource group if you don't have one already</span></span>

        New-AzureRmResourceGroup -Name #ResourceGroupName -Location #location
2. <span data-ttu-id="c03f5-178">Skapa ett nytt Recovery Services-valv och spara det ASR valv objektet i variabeln (kommer att användas senare).</span><span class="sxs-lookup"><span data-stu-id="c03f5-178">Create a new Recovery Services vault and save the created ASR vault object in a variable (will be used later).</span></span> <span data-ttu-id="c03f5-179">Du kan också hämta ASR valvet objektet post skapas med hjälp av cmdleten Get-AzureRMRecoveryServicesVault:-</span><span class="sxs-lookup"><span data-stu-id="c03f5-179">You can also retrieve the ASR vault object post creation using the Get-AzureRMRecoveryServicesVault cmdlet:-</span></span>

        $vault = New-AzureRmRecoveryServicesVault -Name #vaultname -ResouceGroupName #ResourceGroupName -Location #location

## <a name="step-3-set-the-recovery-services-vault-context"></a><span data-ttu-id="c03f5-180">Steg 3: Ange Återställningstjänstvalvet-kontext</span><span class="sxs-lookup"><span data-stu-id="c03f5-180">Step 3: Set the Recovery Services Vault context</span></span>
1. <span data-ttu-id="c03f5-181">Om du har ett valv som redan har skapats kan du köra den nedan att hämta valvet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-181">If you have a vault already created, run the below command to get the vault.</span></span>

       $vault = Get-AzureRmRecoveryServicesVault -Name #vaultname
2. <span data-ttu-id="c03f5-182">Ange valvet-kontexten genom att köra den nedan kommando.</span><span class="sxs-lookup"><span data-stu-id="c03f5-182">Set the vault context by running the below command.</span></span>

       Set-AzureRmSiteRecoveryVaultSettings -ARSVault $vault

## <a name="step-4-install-the-azure-site-recovery-provider"></a><span data-ttu-id="c03f5-183">Steg 4: Installera Azure Site Recovery-providern</span><span class="sxs-lookup"><span data-stu-id="c03f5-183">Step 4: Install the Azure Site Recovery Provider</span></span>
1. <span data-ttu-id="c03f5-184">Skapa en katalog på VMM-datorn genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c03f5-184">On the VMM machine, create a directory by running the following command:</span></span>

       New-Item c:\ASR -type directory
2. <span data-ttu-id="c03f5-185">Extrahera filerna med hjälp av hämtade providern genom att köra följande kommando</span><span class="sxs-lookup"><span data-stu-id="c03f5-185">Extract the files using the downloaded provider by running the following command</span></span>

       pushd C:\ASR\
       .\AzureSiteRecoveryProvider.exe /x:. /q
3. <span data-ttu-id="c03f5-186">Installera providern med hjälp av följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c03f5-186">Install the provider using the following commands:</span></span>

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

   <span data-ttu-id="c03f5-187">Vänta på att installationen ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="c03f5-187">Wait for the installation to finish.</span></span>
4. <span data-ttu-id="c03f5-188">Registrera servern i valvet med följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c03f5-188">Register the server in the vault using the following command:</span></span>

       $BinPath = $env:SystemDrive+"\Program Files\Microsoft System Center 2012 R2\Virtual Machine Manager\bin"
       pushd $BinPath
       $encryptionFilePath = "C:\temp\".\DRConfigurator.exe /r /Credentials $VaultSettingFilePath /vmmfriendlyname $env:COMPUTERNAME /dataencryptionenabled $encryptionFilePath /startvmmservice

## <a name="step-5-create-and-associate-a-replication-policy"></a><span data-ttu-id="c03f5-189">Steg 5: Skapa och koppla en replikeringsprincip</span><span class="sxs-lookup"><span data-stu-id="c03f5-189">Step 5: Create and associate a replication policy</span></span>
1. <span data-ttu-id="c03f5-190">Skapa en replikeringsprincip för Hyper-V 2012 R2 genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c03f5-190">Create a Hyper-V 2012 R2 replication policy by running the following command:</span></span>

        $ReplicationFrequencyInSeconds = "300";        #options are 30,300,900
        $PolicyName = “replicapolicy”
        $RepProvider = HyperVReplica2012R2
        $Recoverypoints = 24                    #specify the number of hours to retain recovery pints
        $AppConsistentSnapshotFrequency = 4 #specify the frequency (in hours) at which app consistent snapshots are taken
        $AuthMode = "Kerberos"  #options are "Kerberos" or "Certificate"
        $AuthPort = "8083"  #specify the port number that will be used for replication traffic on Hyper-V hosts
        $InitialRepMethod = "Online" #options are "Online" or "Offline"

        $policyresult = New-AzureRmSiteRecoveryPolicy -Name $policyname -ReplicationProvider $RepProvider -ReplicationFrequencyInSeconds $Replicationfrequencyinseconds -RecoveryPoints $recoverypoints -ApplicationConsistentSnapshotFrequencyInHours $AppConsistentSnapshotFrequency -Authentication $AuthMode -ReplicationPort $AuthPort -ReplicationMethod $InitialRepMethod

    > [!NOTE]
    > <span data-ttu-id="c03f5-191">VMM-molnet kan innehålla Hyper-V-värdar som kör olika versioner av Windows Server (som anges i Hyper-V-krav), men är OS-version som är specifika för replikeringsprincipen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-191">The VMM cloud can contain Hyper-V hosts running different versions of Windows Server (as mentioned in the Hyper-V prerequisites), but the replication policy is OS version specific.</span></span> <span data-ttu-id="c03f5-192">Om du har olika värdar som körs på olika operativsystemversioner skapa separata replikeringsprinciper för varje typ av OS-version.</span><span class="sxs-lookup"><span data-stu-id="c03f5-192">If you have different hosts running on different operating system versions, then create separate replication policies for each type of OS version.</span></span> <span data-ttu-id="c03f5-193">För t.ex: Om du har fem värdar som kör Windows 2012 för servrar och tre på Windows Server 2012 R2 kan du skapa två principer för replikering – en för varje typ av versioner av operativsystemet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-193">For eg: If you have five hosts running on Windows Servers 2012 and three on Windows Server 2012 R2, create two replication polices – one for each type of operating system versions.</span></span>

1. <span data-ttu-id="c03f5-194">Hämta primära skyddsbehållaren (primära VMM-moln) och återställning skyddsbehållaren (återställning VMM-moln) genom att köra följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="c03f5-194">Get the primary protection container (primary VMM Cloud) and recovery protection container (recovery VMM Cloud) by running the following commands:</span></span>

       $PrimaryCloud = "testprimarycloud"
       $primaryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloud;  

       $RecoveryCloud = "testrecoverycloud"
       $recoveryprotectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $RecoveryCloud;  
2. <span data-ttu-id="c03f5-195">Hämta en princip som du skapade i steg 1 med eget namn för principen</span><span class="sxs-lookup"><span data-stu-id="c03f5-195">Retrieve the policy you created in step 1 using the friendly name of the policy</span></span>

       $policy = Get-AzureRmSiteRecoveryPolicy -FriendlyName $policyname
3. <span data-ttu-id="c03f5-196">Starta associationen för skyddsbehållaren (VMM-moln) med replikeringsprincipen:</span><span class="sxs-lookup"><span data-stu-id="c03f5-196">Start the association of the protection container (VMM Cloud) with the replication policy:</span></span>

       $associationJob  = Start-AzureRmSiteRecoveryPolicyAssociationJob -Policy     $Policy -PrimaryProtectionContainer $primaryprotectionContainer -RecoveryProtectionContainer $recoveryprotectionContainer
4. <span data-ttu-id="c03f5-197">Vänta tills princip association jobbet ska slutföras.</span><span class="sxs-lookup"><span data-stu-id="c03f5-197">Wait for the policy association job to complete.</span></span> <span data-ttu-id="c03f5-198">Du kan kontrollera om jobbet har slutförts med följande kodavsnitt i PowerShell.</span><span class="sxs-lookup"><span data-stu-id="c03f5-198">You can check if the job has completed using the following PowerShell snippet.</span></span>

       $job = Get-AzureRmSiteRecoveryJob -Job $associationJob

       if($job -eq $null -or $job.StateDescription -ne "Completed")
       {
         $isJobLeftForProcessing = $true;
       }

   <span data-ttu-id="c03f5-199">När jobbet har slutförts bearbetning, kör du följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c03f5-199">After the job has finished processing, run the following command:</span></span>

       if($isJobLeftForProcessing)
       {
         Start-Sleep -Seconds 60
       }
       }While($isJobLeftForProcessing)

<span data-ttu-id="c03f5-200">Om du vill kontrollera åtgärden slutfördes, följer du stegen i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="c03f5-200">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

## <a name="step-6-configure-network-mapping"></a><span data-ttu-id="c03f5-201">Steg 6: Konfigurera nätverksmappning</span><span class="sxs-lookup"><span data-stu-id="c03f5-201">Step 6: Configure network mapping</span></span>
1. <span data-ttu-id="c03f5-202">Det första kommandot hämtar servrar för det aktuella Azure Site Recovery-valvet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-202">The first command gets servers for the current Azure Site Recovery vault.</span></span> <span data-ttu-id="c03f5-203">Kommandot lagrar Microsoft Azure Site Recovery-servrar i variabeln $Servers matris.</span><span class="sxs-lookup"><span data-stu-id="c03f5-203">The command stores the Microsoft Azure Site Recovery servers in the $Servers array variable.</span></span>

        $Servers = Get-AzureRmSiteRecoveryServer
2. <span data-ttu-id="c03f5-204">Den nedan kommandon hämta site recovery nätverk för VMM-källservern och målservern för VMM.</span><span class="sxs-lookup"><span data-stu-id="c03f5-204">The below commands get the site recovery network for the source VMM server and the target VMM server.</span></span>

        $PrimaryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[0]        

        $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]

    > [!NOTE]
    > <span data-ttu-id="c03f5-205">VMM-källservern kan vara det första och det andra i matrisen servrar.</span><span class="sxs-lookup"><span data-stu-id="c03f5-205">The source VMM server can be the first one or the second one in the servers array.</span></span> <span data-ttu-id="c03f5-206">Kontrollera namnen på VMM-servrar och hämta nätverken på rätt sätt</span><span class="sxs-lookup"><span data-stu-id="c03f5-206">Check the names of the VMM servers and get the networks appropriately</span></span>


1. <span data-ttu-id="c03f5-207">Den slutliga cmdleten skapar en mappning mellan det primära nätverket och återställningsnätverket.</span><span class="sxs-lookup"><span data-stu-id="c03f5-207">The final cmdlet creates a mapping between the primary network and the recovery network.</span></span> <span data-ttu-id="c03f5-208">Cmdlet anger det primära nätverket som det första elementet i $PrimaryNetworks och återställning nätverk som det första elementet i $RecoveryNetworks.</span><span class="sxs-lookup"><span data-stu-id="c03f5-208">The cmdlet specifies the primary network as the first element of $PrimaryNetworks and the recovery network as the first element of $RecoveryNetworks.</span></span>

        New-AzureRmSiteRecoveryNetworkMapping -PrimaryNetwork $PrimaryNetworks[0] -RecoveryNetwork $RecoveryNetworks[0]

## <a name="step-7-configure-storage-mapping"></a><span data-ttu-id="c03f5-209">Steg 7: Konfigurera lagring-mappning</span><span class="sxs-lookup"><span data-stu-id="c03f5-209">Step 7: Configure storage mapping</span></span>
1. <span data-ttu-id="c03f5-210">Den nedan kommando hämtar listan över lagringsklassificeringar i $storageclassifications variabel.</span><span class="sxs-lookup"><span data-stu-id="c03f5-210">The below command gets the list of storage classifications into $storageclassifications variable.</span></span>

        $storageclassifications = Get-AzureRmSiteRecoveryStorageClassification
2. <span data-ttu-id="c03f5-211">Den nedan kommandon få klassificeringen källan till $SourceClassificaion variabeln och mål för klassificeringen i $TargetClassification variabeln.</span><span class="sxs-lookup"><span data-stu-id="c03f5-211">The below commands get the source classification into $SourceClassificaion variable and target classification into $TargetClassification variable.</span></span>

        $SourceClassificaion = $storageclassifications[0]

        $TargetClassification = $storageclassifications[1]

    > [!NOTE]
    > <span data-ttu-id="c03f5-212">Klassificeringarna källa och mål kan vara alla element i matrisen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-212">The source and target classifications can be any element in the array.</span></span> <span data-ttu-id="c03f5-213">Referera till utdata från den nedan kommando för att räkna index för käll- och klassificeringar på $storageclassifications matris.</span><span class="sxs-lookup"><span data-stu-id="c03f5-213">Refer to the output of the below command to figure the index of source and target classifications in $storageclassifications array.</span></span>

    > <span data-ttu-id="c03f5-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object - egenskapen FriendlyName, Id | Formatera tabell</span><span class="sxs-lookup"><span data-stu-id="c03f5-214">Get-AzureRmSiteRecoveryStorageClassification | Select-Object -Property FriendlyName, Id | Format-Table</span></span>


1. <span data-ttu-id="c03f5-215">Den nedan cmdlet skapar en mappning mellan klassificeringen källa och mål-klassificering.</span><span class="sxs-lookup"><span data-stu-id="c03f5-215">The below cmdlet creates a mapping between the source classification and the target classification.</span></span>

        New-AzureRmSiteRecoveryStorageClassificationMapping -PrimaryStorageClassification $SourceClassificaion -RecoveryStorageClassification $TargetClassification

## <a name="step-8-enable-protection-for-virtual-machines"></a><span data-ttu-id="c03f5-216">Steg 8: Aktivera skydd för virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="c03f5-216">Step 8: Enable protection for virtual machines</span></span>
<span data-ttu-id="c03f5-217">När servrar, moln och nätverk har konfigurerats korrekt och kan du aktivera skydd för virtuella datorer i molnet.</span><span class="sxs-lookup"><span data-stu-id="c03f5-217">After the servers, clouds and networks are configured correctly, you can enable protection for virtual machines in the cloud.</span></span>

1. <span data-ttu-id="c03f5-218">Kör följande kommando för att hämta skyddsbehållaren för att aktivera skydd:</span><span class="sxs-lookup"><span data-stu-id="c03f5-218">To enable protection, run the following command to get the protection container:</span></span>

          $PrimaryProtectionContainer = Get-AzureRmSiteRecoveryProtectionContainer -friendlyName $PrimaryCloudName
2. <span data-ttu-id="c03f5-219">Hämta skyddad entitet (VM) genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c03f5-219">Get the protection entity (VM) by running the following command:</span></span>

           $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -friendlyName $VMName -ProtectionContainer $PrimaryProtectionContainer
3. <span data-ttu-id="c03f5-220">Aktivera replikering för den virtuella datorn genom att köra följande kommando:</span><span class="sxs-lookup"><span data-stu-id="c03f5-220">Enable replication for the VM by running the following command:</span></span>

          $jobResult = Set-AzureRmSiteRecoveryProtectionEntity -ProtectionEntity $protectionentity -Protection Enable -Policy $policy

## <a name="test-your-deployment"></a><span data-ttu-id="c03f5-221">Testa distributionen</span><span class="sxs-lookup"><span data-stu-id="c03f5-221">Test your deployment</span></span>
<span data-ttu-id="c03f5-222">Om du vill testa distributionen kan du köra ett redundanstest för en enskild virtuell dator eller skapa en återställningsplan som består av flera virtuella datorer och köra ett redundanstest för planen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-222">To test your deployment you can run a test failover for a single virtual machine, or create a recovery plan consisting of multiple virtual machines and run a test failover for the plan.</span></span> <span data-ttu-id="c03f5-223">Ett redundanstest simulerar redundans- och återställningsmekanismen i ett isolerat nätverk.</span><span class="sxs-lookup"><span data-stu-id="c03f5-223">Test failover simulates your failover and recovery mechanism in an isolated network.</span></span>

> [!NOTE]
> <span data-ttu-id="c03f5-224">Du kan skapa en återställningsplan för ditt program i Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="c03f5-224">You can create a recovery plan for your application in Azure portal.</span></span>
>
>

<span data-ttu-id="c03f5-225">Om du vill kontrollera åtgärden slutfördes, följer du stegen i [Övervakaraktiviteten](#monitor).</span><span class="sxs-lookup"><span data-stu-id="c03f5-225">To check the completion of the operation, follow the steps in [Monitor Activity](#monitor).</span></span>

### <a name="run-a-test-failover"></a><span data-ttu-id="c03f5-226">Köra ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="c03f5-226">Run a test failover</span></span>
1. <span data-ttu-id="c03f5-227">Kör den nedan cmdlets för att hämta dina virtuella datorer till för det Virtuella datornätverket som du vill testa redundans.</span><span class="sxs-lookup"><span data-stu-id="c03f5-227">Run the below cmdlets to get the VM network to which you want to test failover your VMs to.</span></span>

       $Servers = Get-AzureRmSiteRecoveryServer
       $RecoveryNetworks = Get-AzureRmSiteRecoveryNetwork -Server $Servers[1]
2. <span data-ttu-id="c03f5-228">Utför ett redundanstest av en virtuell dator genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c03f5-228">Perform a test failover of a VM by doing the following:</span></span>

       $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -FriendlyName $VMName -ProtectionContainer $PrimaryprotectionContainer

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity -VMNetwork $RecoveryNetworks[1]
3. <span data-ttu-id="c03f5-229">Utför ett redundanstest av en återställningsplan genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c03f5-229">Perform a test failover of a recovery plan by doing the following:</span></span>

       $recoveryplanname = "test-recovery-plan"

       $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

       $jobIDResult =  Start-AzureRmSiteRecoveryTestFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan -VMNetwork $RecoveryNetworks[1]

### <a name="run-a-planned-failover"></a><span data-ttu-id="c03f5-230">Kör en planerad redundans</span><span class="sxs-lookup"><span data-stu-id="c03f5-230">Run a planned failover</span></span>
1. <span data-ttu-id="c03f5-231">Utför en planerad redundansväxling av en virtuell dator genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c03f5-231">Perform a planned failover of a VM by doing the following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity
2. <span data-ttu-id="c03f5-232">Utför en planerad redundansväxling av en återställningsplan genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c03f5-232">Perform a planned failover of a recovery plan by doing the following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryPlannedFailoverJob -Direction PrimaryToRecovery -Recoveryplan $recoveryplan

### <a name="run-an-unplanned-failover"></a><span data-ttu-id="c03f5-233">Kör en oplanerad redundans</span><span class="sxs-lookup"><span data-stu-id="c03f5-233">Run an unplanned failover</span></span>
1. <span data-ttu-id="c03f5-234">Utföra en oplanerad redundans för en virtuell dator genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c03f5-234">Perform an unplanned failover of a VM by doing the following:</span></span>

        $protectionEntity = Get-AzureRmSiteRecoveryProtectionEntity -Name $VMName -ProtectionContainer $PrimaryprotectionContainer

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

<span data-ttu-id="c03f5-235">2. utföra en oplanerad redundans för en återställningsplan genom att göra följande:</span><span class="sxs-lookup"><span data-stu-id="c03f5-235">2.Perform an unplanned failover of a recovery plan by doing the following:</span></span>

        $recoveryplanname = "test-recovery-plan"

        $recoveryplan = Get-AzureRmSiteRecoveryRecoveryPlan -FriendlyName $recoveryplanname

        $jobIDResult =  Start-AzureRmSiteRecoveryUnPlannedFailoverJob -Direction PrimaryToRecovery -ProtectionEntity $protectionEntity

## <span data-ttu-id="c03f5-236"><a name=monitor></a>Övervakaraktivitet</span><span class="sxs-lookup"><span data-stu-id="c03f5-236"><a name=monitor></a> Monitor Activity</span></span>
<span data-ttu-id="c03f5-237">Du kan använda följande kommandon för att övervaka aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="c03f5-237">Use the following commands to monitor the activity.</span></span> <span data-ttu-id="c03f5-238">Observera att du har ska vänta mellan jobb för bearbetning till slut.</span><span class="sxs-lookup"><span data-stu-id="c03f5-238">Note that you have to wait in between jobs for the processing to finish.</span></span>

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



## <a name="next-steps"></a><span data-ttu-id="c03f5-239">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c03f5-239">Next steps</span></span>
<span data-ttu-id="c03f5-240">[Läs mer](/powershell/module/azurerm.recoveryservices.backup/#recovery) om Azure Site Recovery med Azure Resource Manager PowerShell-cmdlets.</span><span class="sxs-lookup"><span data-stu-id="c03f5-240">[Read more](/powershell/module/azurerm.recoveryservices.backup/#recovery) about Azure Site Recovery with Azure Resource Manager PowerShell cmdlets.</span></span>
