---
title: "aaaReplicate en flera nivåer Citrix XenDesktop XenApp distribution och använda Azure Site Recovery | Microsoft Docs"
description: "Den här artikeln beskriver hur tooprotect och Återställ Citrix XenDesktop och XenApp distributioner med hjälp av Azure Site Recovery."
services: site-recovery
documentationcenter: 
author: ponatara
manager: abhemraj
editor: 
ms.assetid: 9126f5e8-e9ed-4c31-b6b4-bf969c12c184
ms.service: site-recovery
ms.workload: storage-backup-recovery
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/18/2017
ms.author: ponatara
ms.openlocfilehash: c4ea9f95f91c585cdcf9d776b02c0967f4c16ab0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="replicate-a-multi-tier-citrix-xenapp-and-xendesktop-deployment-using-azure-site-recovery"></a><span data-ttu-id="d6eab-103">Replikera en flera nivåer Citrix XenApp XenDesktop distribution och använda Azure Site Recovery</span><span class="sxs-lookup"><span data-stu-id="d6eab-103">Replicate a multi-tier Citrix XenApp and XenDesktop deployment using Azure Site Recovery</span></span>

## <a name="overview"></a><span data-ttu-id="d6eab-104">Översikt</span><span class="sxs-lookup"><span data-stu-id="d6eab-104">Overview</span></span>

<span data-ttu-id="d6eab-105">Citrix XenDesktop är en lösning för virtualisering av Fjärrskrivbordstjänster som levererar skrivbord och program som en ondemand-tjänsten tooany användare var som helst.</span><span class="sxs-lookup"><span data-stu-id="d6eab-105">Citrix XenDesktop is a desktop virtualization solution that delivers desktops and applications as an ondemand service tooany user, anywhere.</span></span> <span data-ttu-id="d6eab-106">Med FlexCast leverans teknik kan XenDesktop snabbt och säkert leverera program och skrivbord toousers.</span><span class="sxs-lookup"><span data-stu-id="d6eab-106">With FlexCast delivery technology, XenDesktop can quickly and securely deliver applications and desktops toousers.</span></span>
<span data-ttu-id="d6eab-107">Idag, tillhandahåller Citrix XenApp inte en katastrof återställningsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="d6eab-107">Today, Citrix XenApp does not provide any disaster recovery capabilities.</span></span>

<span data-ttu-id="d6eab-108">En bra lösning för haveriberedskap, ska tillåta modellering av återställningsplaner runt hello ovan programarkitekturer för komplexa och har även hello möjlighet tooadd anpassade steg toohandle mappning mellan olika nivåer därför att tillhandahålla en enkelklickning att som lösning för en katastrofåterställning inledande tooa händelsen hello lägre Återställningstidsmål.</span><span class="sxs-lookup"><span data-stu-id="d6eab-108">A good disaster recovery solution, should allow modeling of recovery plans around hello above complex application architectures and also have hello ability tooadd customized steps toohandle application mappings between various tiers hence providing a single-click sure shot solution in hello event of a disaster leading tooa lower RTO.</span></span>

<span data-ttu-id="d6eab-109">Det här dokumentet innehåller stegvisa anvisningar för att skapa en lösning för katastrofåterställning för dina lokala Citrix XenApp distributioner på Hyper-V och VMware vSphere-plattformar.</span><span class="sxs-lookup"><span data-stu-id="d6eab-109">This document provides a step-by-step guidance for building a disaster recovery solution for your on-premises Citrix XenApp deployments on Hyper-V and VMware vSphere platforms.</span></span> <span data-ttu-id="d6eab-110">Det här dokumentet beskriver också hur tooperform ett redundanstest (disaster recovery-test) och oplanerad redundans tooAzure med återställningsplaner, hello stöds konfigurationer och förutsättningar.</span><span class="sxs-lookup"><span data-stu-id="d6eab-110">This document also describes how tooperform a test failover(disaster recovery drill) and unplanned failover tooAzure using recovery plans, hello supported configurations and prerequisites.</span></span>


## <a name="prerequisites"></a><span data-ttu-id="d6eab-111">Krav</span><span class="sxs-lookup"><span data-stu-id="d6eab-111">Prerequisites</span></span>

<span data-ttu-id="d6eab-112">Innan du börjar, kontrollera att du förstår hello följande:</span><span class="sxs-lookup"><span data-stu-id="d6eab-112">Before you start, make sure you understand hello following:</span></span>

1. [<span data-ttu-id="d6eab-113">Replikera en virtuell dator tooAzure</span><span class="sxs-lookup"><span data-stu-id="d6eab-113">Replicating a virtual machine tooAzure</span></span>](site-recovery-vmware-to-azure.md)
1. <span data-ttu-id="d6eab-114">Hur för[utforma ett nätverk för återställning](site-recovery-network-design.md)</span><span class="sxs-lookup"><span data-stu-id="d6eab-114">How too[design a recovery network](site-recovery-network-design.md)</span></span>
1. [<span data-ttu-id="d6eab-115">Gör en testa redundans tooAzure</span><span class="sxs-lookup"><span data-stu-id="d6eab-115">Doing a test failover tooAzure</span></span>](site-recovery-test-failover-to-azure.md)
1. [<span data-ttu-id="d6eab-116">Gör en tooAzure för växling vid fel</span><span class="sxs-lookup"><span data-stu-id="d6eab-116">Doing a failover tooAzure</span></span>](site-recovery-failover.md)
1. <span data-ttu-id="d6eab-117">Hur för[replikera en domänkontrollant](site-recovery-active-directory.md)</span><span class="sxs-lookup"><span data-stu-id="d6eab-117">How too[replicate a domain controller](site-recovery-active-directory.md)</span></span>
1. <span data-ttu-id="d6eab-118">Hur för[replikera SQL Server](site-recovery-sql.md)</span><span class="sxs-lookup"><span data-stu-id="d6eab-118">How too[replicate SQL Server](site-recovery-sql.md)</span></span>

## <a name="deployment-patterns"></a><span data-ttu-id="d6eab-119">Mönster för distribution</span><span class="sxs-lookup"><span data-stu-id="d6eab-119">Deployment patterns</span></span>

<span data-ttu-id="d6eab-120">En Citrix XenApp och XenDesktop servergruppen har vanligtvis hello efter distributionen mönster:</span><span class="sxs-lookup"><span data-stu-id="d6eab-120">A Citrix XenApp and XenDesktop farm typically has hello following deployment pattern:</span></span>

<span data-ttu-id="d6eab-121">**Mönster för distribution**</span><span class="sxs-lookup"><span data-stu-id="d6eab-121">**Deployment pattern**</span></span>

<span data-ttu-id="d6eab-122">Citrix XenApp och XenDesktop distribution med AD DNS-server, SQL databasserver server, Citrix leverans styrenhet, StoreFront, XenApp Master (VDA), Citrix XenApp licensserver</span><span class="sxs-lookup"><span data-stu-id="d6eab-122">Citrix XenApp and XenDesktop deployment with AD DNS server, SQL database server, Citrix Delivery Controller, StoreFront server, XenApp Master (VDA), Citrix XenApp License Server</span></span>

![Distribution av mönstret 1](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-deployment.png)


## <a name="site-recovery-support"></a><span data-ttu-id="d6eab-124">Site Recovery-stöd</span><span class="sxs-lookup"><span data-stu-id="d6eab-124">Site Recovery support</span></span>

<span data-ttu-id="d6eab-125">För hello syftet med den här artikeln, Citrix-distributioner på virtuella VMware-datorer som hanteras av vSphere 6.0 eller System Center VMM 2012 R2 har använt toosetup DR.</span><span class="sxs-lookup"><span data-stu-id="d6eab-125">For hello purpose of this article, Citrix deployments on VMware virtual machines managed by vSphere 6.0 / System Center VMM 2012 R2 were used toosetup DR.</span></span>

### <a name="source-and-target"></a><span data-ttu-id="d6eab-126">Källa och mål</span><span class="sxs-lookup"><span data-stu-id="d6eab-126">Source and target</span></span>

<span data-ttu-id="d6eab-127">**Scenario**</span><span class="sxs-lookup"><span data-stu-id="d6eab-127">**Scenario**</span></span> | <span data-ttu-id="d6eab-128">**tooa sekundär plats**</span><span class="sxs-lookup"><span data-stu-id="d6eab-128">**tooa secondary site**</span></span> | <span data-ttu-id="d6eab-129">**tooAzure**</span><span class="sxs-lookup"><span data-stu-id="d6eab-129">**tooAzure**</span></span>
--- | --- | ---
<span data-ttu-id="d6eab-130">**Hyper-V**</span><span class="sxs-lookup"><span data-stu-id="d6eab-130">**Hyper-V**</span></span> | <span data-ttu-id="d6eab-131">Inte i omfånget</span><span class="sxs-lookup"><span data-stu-id="d6eab-131">Not in scope</span></span> | <span data-ttu-id="d6eab-132">Ja</span><span class="sxs-lookup"><span data-stu-id="d6eab-132">Yes</span></span>
<span data-ttu-id="d6eab-133">**VMware**</span><span class="sxs-lookup"><span data-stu-id="d6eab-133">**VMware**</span></span> | <span data-ttu-id="d6eab-134">Inte i omfånget</span><span class="sxs-lookup"><span data-stu-id="d6eab-134">Not in scope</span></span> | <span data-ttu-id="d6eab-135">Ja</span><span class="sxs-lookup"><span data-stu-id="d6eab-135">Yes</span></span>
<span data-ttu-id="d6eab-136">**Fysisk server**</span><span class="sxs-lookup"><span data-stu-id="d6eab-136">**Physical server**</span></span> | <span data-ttu-id="d6eab-137">Inte i omfånget</span><span class="sxs-lookup"><span data-stu-id="d6eab-137">Not in scope</span></span> | <span data-ttu-id="d6eab-138">Ja</span><span class="sxs-lookup"><span data-stu-id="d6eab-138">Yes</span></span>

### <a name="versions"></a><span data-ttu-id="d6eab-139">Versioner</span><span class="sxs-lookup"><span data-stu-id="d6eab-139">Versions</span></span>
<span data-ttu-id="d6eab-140">Kunderna kan distribuera XenApp komponenter som virtuella datorer på Hyper-V- eller VMware eller fysiska servrar.</span><span class="sxs-lookup"><span data-stu-id="d6eab-140">Customers can deploy XenApp components as Virtual Machines running on Hyper-V or VMware or as Physical Servers.</span></span> <span data-ttu-id="d6eab-141">Azure Site Recovery kan skydda både fysiska och virtuella distributioner tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-141">Azure Site Recovery can protect both physical and virtual deployments tooAzure.</span></span>
<span data-ttu-id="d6eab-142">Eftersom XenApp 7,7 eller senare stöds i Azure, kan endast installationer med dessa versioner flyttas över tooAzure för katastrofåterställning eller migrering.</span><span class="sxs-lookup"><span data-stu-id="d6eab-142">Since XenApp 7.7 or later is supported in Azure, only deployments with these versions can be failed over tooAzure for Disaster Recovery or migration.</span></span>

### <a name="things-tookeep-in-mind"></a><span data-ttu-id="d6eab-143">Saker tookeep i åtanke</span><span class="sxs-lookup"><span data-stu-id="d6eab-143">Things tookeep in mind</span></span>

1. <span data-ttu-id="d6eab-144">Skydd och återställning av lokala distributioner med hjälp av Server-OS datorer toodeliver XenApp publicerade appar och XenApp publiceras skrivbord stöds.</span><span class="sxs-lookup"><span data-stu-id="d6eab-144">Protection and recovery of on-premises deployments using Server OS machines toodeliver XenApp published apps and XenApp published desktops is supported.</span></span>

2. <span data-ttu-id="d6eab-145">Skydd och återställning av lokala distributioner med hjälp av fjärrskrivbord OS datorer toodeliver Desktop VDI för klientens virtuella skrivbord, inklusive Windows 10 stöds inte.</span><span class="sxs-lookup"><span data-stu-id="d6eab-145">Protection and recovery of on-premises deployments using desktop OS machines toodeliver Desktop VDI for client virtual desktops, including Windows 10, is not supported.</span></span> <span data-ttu-id="d6eab-146">Det beror på att ASR inte stöder hello återställning av datorer med desktop OS'es.</span><span class="sxs-lookup"><span data-stu-id="d6eab-146">This is because ASR does not support hello recovery of machines with desktop OS’es.</span></span>  <span data-ttu-id="d6eab-147">Dessutom vissa klienten virtuellt skrivbord smaksättningar (t.ex.</span><span class="sxs-lookup"><span data-stu-id="d6eab-147">Also, some client virtual desktop flavours (eg.</span></span> <span data-ttu-id="d6eab-148">Windows 7) ännu stöds inte för licensiering i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-148">Windows 7) are not yet supported for licensing in Azure.</span></span> <span data-ttu-id="d6eab-149">[Lär dig mer](https://azure.microsoft.com/pricing/licensing-faq/) om licensiering för klient/server-datorer i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-149">[Learn More](https://azure.microsoft.com/pricing/licensing-faq/) about licensing for client/server desktops in Azure.</span></span>

3.  <span data-ttu-id="d6eab-150">Azure Site Recovery kan replikera och skydda befintliga lokala MCS eller PVS kloningar.</span><span class="sxs-lookup"><span data-stu-id="d6eab-150">Azure Site Recovery cannot replicate and protect existing on-premises MCS or PVS clones.</span></span>
<span data-ttu-id="d6eab-151">Du måste toorecreate dessa kloner med hjälp av Azure RM etablering från leverans domänkontrollant.</span><span class="sxs-lookup"><span data-stu-id="d6eab-151">You need toorecreate these clones using Azure RM provisioning from Delivery controller.</span></span>

4. <span data-ttu-id="d6eab-152">NetScaler kan inte skyddas med Azure Site Recovery som NetScaler baseras på FreeBSD och Azure Site Recovery stöder inte skydd av FreeBSD-OS.</span><span class="sxs-lookup"><span data-stu-id="d6eab-152">NetScaler cannot be protected using Azure Site Recovery as NetScaler is based on FreeBSD and Azure Site Recovery does not support protection of FreeBSD OS.</span></span> <span data-ttu-id="d6eab-153">Du behöver toodeploy och konfigurera en ny installation NetScaler från Azure-marknadsplatsen efter växling vid fel tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-153">You would need toodeploy and configure a new NetScaler appliance from Azure Market place after failover tooAzure.</span></span>


## <a name="replicating-virtual-machines"></a><span data-ttu-id="d6eab-154">Replikering av virtuella datorer</span><span class="sxs-lookup"><span data-stu-id="d6eab-154">Replicating virtual machines</span></span>

<span data-ttu-id="d6eab-155">följande komponenter i hello Citrix XenApp distribution hello måste toobe skyddade tooenable replikering och återställning.</span><span class="sxs-lookup"><span data-stu-id="d6eab-155">hello following components of hello Citrix XenApp deployment need toobe protected tooenable replication and recovery.</span></span>

* <span data-ttu-id="d6eab-156">Skydd av AD-DNS-server</span><span class="sxs-lookup"><span data-stu-id="d6eab-156">Protection of AD DNS server</span></span>
* <span data-ttu-id="d6eab-157">Skydd av SQL-databasserver</span><span class="sxs-lookup"><span data-stu-id="d6eab-157">Protection of SQL database server</span></span>
* <span data-ttu-id="d6eab-158">Skydd av Citrix leverans av domänkontrollant</span><span class="sxs-lookup"><span data-stu-id="d6eab-158">Protection of Citrix Delivery Controller</span></span>
* <span data-ttu-id="d6eab-159">Skydd av StoreFront server.</span><span class="sxs-lookup"><span data-stu-id="d6eab-159">Protection of StoreFront server.</span></span>
* <span data-ttu-id="d6eab-160">Skydd av XenApp Master (VDA)</span><span class="sxs-lookup"><span data-stu-id="d6eab-160">Protection of XenApp Master (VDA)</span></span>
* <span data-ttu-id="d6eab-161">Skydd av Citrix XenApp licensserver</span><span class="sxs-lookup"><span data-stu-id="d6eab-161">Protection of Citrix XenApp License Server</span></span>


<span data-ttu-id="d6eab-162">**AD DNS-server-replikering**</span><span class="sxs-lookup"><span data-stu-id="d6eab-162">**AD DNS server replication**</span></span>

<span data-ttu-id="d6eab-163">Se för[skydda Active Directory och DNS med Azure Site Recovery](site-recovery-active-directory.md) på anvisningar för att replikera och konfigurerar en domänkontrollant i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-163">Please refer too[Protect Active Directory and DNS with Azure Site Recovery](site-recovery-active-directory.md) on guidance for replicating and configuring a domain controller in Azure.</span></span>

<span data-ttu-id="d6eab-164">**SQL Server databasreplikering**</span><span class="sxs-lookup"><span data-stu-id="d6eab-164">**SQL database Server replication**</span></span>

<span data-ttu-id="d6eab-165">Se för[skydda SQL Server med SQL Server-katastrofåterställning och Azure Site Recovery](site-recovery-sql.md) detaljerad teknisk information om hello rekommenderade alternativ för att skydda SQL-servrar.</span><span class="sxs-lookup"><span data-stu-id="d6eab-165">Please refer too[Protect SQL Server with SQL Server disaster recovery and Azure Site Recovery](site-recovery-sql.md) for detailed technical guidance on hello recommended options for protecting SQL servers.</span></span>

<span data-ttu-id="d6eab-166">Följ [vägledningen](site-recovery-vmware-to-azure.md) toostart replikerar hello andra virtuella datorer tooAzure för komponenten.</span><span class="sxs-lookup"><span data-stu-id="d6eab-166">Follow [this guidance](site-recovery-vmware-to-azure.md) toostart replicating hello other component virtual machines tooAzure.</span></span>

![Skydd av XenApp komponenter](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-enablereplication.png)

<span data-ttu-id="d6eab-168">**Beräknings- och nätverksinställningar**</span><span class="sxs-lookup"><span data-stu-id="d6eab-168">**Compute and Network Settings**</span></span>

<span data-ttu-id="d6eab-169">När hello datorer är skyddade (status visas som ”skyddad” under replikerade objekt), hello beräkning och nätverksinställningar måste toobe konfigurerats.</span><span class="sxs-lookup"><span data-stu-id="d6eab-169">After hello machines are protected (status shows as “Protected” under Replicated Items), hello Compute and Network settings need toobe configured.</span></span>
<span data-ttu-id="d6eab-170">I beräknings- och nätverksinställningar > beräkna egenskaper, kan du ange hello Azure VM-namn och mål för storlek.</span><span class="sxs-lookup"><span data-stu-id="d6eab-170">In Compute and Network > Compute properties, you can specify hello Azure VM name and target size.</span></span>
<span data-ttu-id="d6eab-171">Ändra hello namnet toocomply med krav för Azure om du behöver.</span><span class="sxs-lookup"><span data-stu-id="d6eab-171">Modify hello name toocomply with Azure requirements if you need to.</span></span> <span data-ttu-id="d6eab-172">Du kan också visa och lägga till information om hello målnätverket, undernätet och IP-adress som ska tilldelas toohello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="d6eab-172">You can also view and add information about hello target network, subnet, and IP address that will be assigned toohello Azure VM.</span></span>

<span data-ttu-id="d6eab-173">Observera följande hello:</span><span class="sxs-lookup"><span data-stu-id="d6eab-173">Note hello following:</span></span>

* <span data-ttu-id="d6eab-174">Du kan ange IP-adressen för hello mål.</span><span class="sxs-lookup"><span data-stu-id="d6eab-174">You can set hello target IP address.</span></span> <span data-ttu-id="d6eab-175">Om du inte anger någon adress använder hello redundansväxlade datorn DHCP.</span><span class="sxs-lookup"><span data-stu-id="d6eab-175">If you don't provide an address, hello failed over machine will use DHCP.</span></span> <span data-ttu-id="d6eab-176">Om du anger en adress som inte är tillgänglig under växling vid fel, fungerar inte hello växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="d6eab-176">If you set an address that isn't available at failover, hello failover won't work.</span></span> <span data-ttu-id="d6eab-177">hello samma mål-IP-adress kan användas för att testa redundans om adressen hello är tillgänglig i nätverket hello.</span><span class="sxs-lookup"><span data-stu-id="d6eab-177">hello same target IP address can be used for test failover if hello address is available in hello test failover network.</span></span>

* <span data-ttu-id="d6eab-178">För hello AD/DNS-server, behåller hello lokal adress kan du ange samma adress hello som hello DNS-server för hello Azure Virtual network.</span><span class="sxs-lookup"><span data-stu-id="d6eab-178">For hello AD/DNS server, retaining hello on-premises address lets you specify hello same address as hello DNS server for hello Azure Virtual network.</span></span>

<span data-ttu-id="d6eab-179">hello antalet nätverkskort beror hello storleken du anger för hello för den virtuella måldatorn enligt följande:</span><span class="sxs-lookup"><span data-stu-id="d6eab-179">hello number of network adapters is dictated by hello size you specify for hello target virtual machine, as follows:</span></span>

*   <span data-ttu-id="d6eab-180">Om hello antalet nätverkskort på hello källdatorn är mindre än eller lika med toohello antal nätverkskort som tillåts för hello måldatorn och sedan hello målet att ha hello samma antal kort som hello källa.</span><span class="sxs-lookup"><span data-stu-id="d6eab-180">If hello number of network adapters on hello source machine is less than or equal toohello number of adapters allowed for hello target machine size, then hello target will have hello same number of adapters as hello source.</span></span>
*   <span data-ttu-id="d6eab-181">Om hello antalet nätverkskort för hello virtuella källdatorn överskrider hello tillåtna antal för hello målstorleken så hello högsta kommer att användas.</span><span class="sxs-lookup"><span data-stu-id="d6eab-181">If hello number of adapters for hello source virtual machine exceeds hello number allowed for hello target size then hello target size maximum will be used.</span></span>
* <span data-ttu-id="d6eab-182">Om en källdator har två nätverkskort och hello måldatorn stöder fyra, kommer hello måldatorn ha två nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="d6eab-182">For example, if a source machine has two network adapters and hello target machine size supports four, hello target machine will have two adapters.</span></span> <span data-ttu-id="d6eab-183">Om hello källdatorn har två nätverkskort men hello målstorleken endast stöder ett att hello måldatorn ha en enda nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="d6eab-183">If hello source machine has two adapters but hello supported target size only supports one then hello target machine will have only one adapter.</span></span>
*   <span data-ttu-id="d6eab-184">Om hello virtuella datorn har flera nätverkskort ansluts alla toohello samma nätverk.</span><span class="sxs-lookup"><span data-stu-id="d6eab-184">If hello virtual machine has multiple network adapters they will all connect toohello same network.</span></span>
*   <span data-ttu-id="d6eab-185">Om hello virtuella datorn har flera nätverkskort, blir hello första som visas i listan hello hello standardnätverkskort i hello virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="d6eab-185">If hello virtual machine has multiple network adapters, then hello first one shown in hello list becomes hello Default network adapter in hello Azure virtual machine.</span></span>


## <a name="creating-a-recovery-plan"></a><span data-ttu-id="d6eab-186">Skapa en återställningsplan</span><span class="sxs-lookup"><span data-stu-id="d6eab-186">Creating a recovery plan</span></span>

<span data-ttu-id="d6eab-187">Efter att replikering har aktiverats för hello XenApp komponenten VMs är hello nästa steg toocreate en återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="d6eab-187">After replication is enabled for hello XenApp component VMs, hello next step is toocreate a recovery plan.</span></span>
<span data-ttu-id="d6eab-188">En återställning planera grupper tillsammans virtuella datorer med samma krav för redundans och återställning.</span><span class="sxs-lookup"><span data-stu-id="d6eab-188">A recovery plan groups together virtual machines with similar requirements for failover and recovery.</span></span>  

<span data-ttu-id="d6eab-189">**Steg toocreate en återställningsplan**</span><span class="sxs-lookup"><span data-stu-id="d6eab-189">**Steps toocreate a recovery plan**</span></span>

1. <span data-ttu-id="d6eab-190">Lägg till hello XenApp komponenten virtuella datorer i hello planera för återställning.</span><span class="sxs-lookup"><span data-stu-id="d6eab-190">Add hello XenApp component virtual machines in hello Recovery Plan.</span></span>
2. <span data-ttu-id="d6eab-191">Klicka på Återställningsplaner -> + Planera för återställning.</span><span class="sxs-lookup"><span data-stu-id="d6eab-191">Click Recovery Plans -> + Recovery Plan.</span></span> <span data-ttu-id="d6eab-192">Ange ett intuitivt namn för hello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="d6eab-192">Provide an intuitive name for hello recovery plan.</span></span>
3. <span data-ttu-id="d6eab-193">För virtuella VMware-datorer: Välj källa som VMware processervern, mål som Microsoft Azure och distributionsmodell som Resource Manager och klicka på Välj objekt.</span><span class="sxs-lookup"><span data-stu-id="d6eab-193">For VMware virtual machines: Select source as VMware process server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items.</span></span>
4. <span data-ttu-id="d6eab-194">För Hyper-V virtuella datorer: Välj källa som VMM-servern, mål som Microsoft Azure och distributionsmodell som Resource Manager och klicka på Välj objekt och välj sedan hello XenApp distribution av virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="d6eab-194">For Hyper-V virtual machines: Select source as VMM server, target as Microsoft Azure, and deployment model as Resource Manager and click on Select items and then select hello XenApp deployment VMs.</span></span>

### <a name="adding-virtual-machines-toofailover-groups"></a><span data-ttu-id="d6eab-195">Lägga till virtuella datorer toofailover grupper</span><span class="sxs-lookup"><span data-stu-id="d6eab-195">Adding virtual machines toofailover groups</span></span>

<span data-ttu-id="d6eab-196">Återställningsplaner kan vara anpassade tooadd redundans grupper för specifika start order, skript eller manuella åtgärder.</span><span class="sxs-lookup"><span data-stu-id="d6eab-196">Recovery plans can be customized tooadd failover groups for specific startup order, scripts or manual actions.</span></span> <span data-ttu-id="d6eab-197">följande grupper hello måste toobe tillagda toohello återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="d6eab-197">hello following groups need toobe added toohello recovery plan.</span></span>

1. <span data-ttu-id="d6eab-198">Redundans Group1: AD DNS</span><span class="sxs-lookup"><span data-stu-id="d6eab-198">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="d6eab-199">Redundans Grupp2: SQL Server-datorer</span><span class="sxs-lookup"><span data-stu-id="d6eab-199">Failover Group2: SQL Server VMs</span></span>
2. <span data-ttu-id="d6eab-200">Redundans Group3: VDA Master avbildningen VM</span><span class="sxs-lookup"><span data-stu-id="d6eab-200">Failover Group3: VDA Master Image VM</span></span>
3. <span data-ttu-id="d6eab-201">Failover Group4: Leverans styrenhet och StoreFront virtuella server-datorer</span><span class="sxs-lookup"><span data-stu-id="d6eab-201">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>


### <a name="adding-scripts-toohello-recovery-plan"></a><span data-ttu-id="d6eab-202">Lägga till skript toohello återställningsplan</span><span class="sxs-lookup"><span data-stu-id="d6eab-202">Adding scripts toohello recovery plan</span></span>

<span data-ttu-id="d6eab-203">Skript kan köras före eller efter en viss grupp i en återställningsplan.</span><span class="sxs-lookup"><span data-stu-id="d6eab-203">Scripts can be run before or after a specific group in a recovery plan.</span></span> <span data-ttu-id="d6eab-204">Manuella åtgärder kan också ingå och utförs under växling vid fel.</span><span class="sxs-lookup"><span data-stu-id="d6eab-204">Manual actions can be also be included and performed during failover.</span></span>

<span data-ttu-id="d6eab-205">hello anpassade återställningsplan ser ut så hello nedan:</span><span class="sxs-lookup"><span data-stu-id="d6eab-205">hello customized recovery plan looks like hello below:</span></span>

1. <span data-ttu-id="d6eab-206">Redundans Group1: AD DNS</span><span class="sxs-lookup"><span data-stu-id="d6eab-206">Failover Group1: AD DNS</span></span>
2. <span data-ttu-id="d6eab-207">Redundans Grupp2: SQL Server-datorer</span><span class="sxs-lookup"><span data-stu-id="d6eab-207">Failover Group2: SQL Server VMs</span></span>
3. <span data-ttu-id="d6eab-208">Redundans Group3: VDA Master avbildningen VM</span><span class="sxs-lookup"><span data-stu-id="d6eab-208">Failover Group3: VDA Master Image VM</span></span>

   >[!NOTE]     
   ><span data-ttu-id="d6eab-209">Steg 4, 6 och 7 som innehåller instruktioner för manuell eller skript som är tillämpliga tooonly ett lokalt XenApp > miljö med MCS/PVS kataloger.</span><span class="sxs-lookup"><span data-stu-id="d6eab-209">Steps 4, 6 and 7 containing manual or script actions are applicable tooonly an on-premises XenApp >environment with MCS/PVS catalogs.</span></span>

4. <span data-ttu-id="d6eab-210">3 manuell eller skript åtgärd: avstängning master VDA VM hello Master VDA VM när redundansväxlats tooAzure blir körs.</span><span class="sxs-lookup"><span data-stu-id="d6eab-210">Group 3 Manual or script action: Shutdown master VDA VM hello Master VDA VM when failed over tooAzure will be in a running state.</span></span> <span data-ttu-id="d6eab-211">toocreate nya MCS kataloger med Azure ARM-värd, hello master VDA VM är obligatoriska toobe i Stoppad (de allokerade) tillstånd.</span><span class="sxs-lookup"><span data-stu-id="d6eab-211">toocreate new MCS catalogs using Azure ARM hosting, hello master VDA VM is required toobe in Stopped (de allocated) state.</span></span> <span data-ttu-id="d6eab-212">Avstängning hello virtuell dator från Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="d6eab-212">Shutdown hello VM from Azure Portal.</span></span>

5. <span data-ttu-id="d6eab-213">Failover Group4: Leverans styrenhet och StoreFront virtuella server-datorer</span><span class="sxs-lookup"><span data-stu-id="d6eab-213">Failover Group4: Delivery Controller and StoreFront server VMs</span></span>
6. <span data-ttu-id="d6eab-214">Group3 manuell eller skript åtgärd 1:</span><span class="sxs-lookup"><span data-stu-id="d6eab-214">Group3 manual or script action 1:</span></span>

    <span data-ttu-id="d6eab-215">***Lägg till Azure RM värdanslutning***</span><span class="sxs-lookup"><span data-stu-id="d6eab-215">***Add Azure RM host connection***</span></span>

    <span data-ttu-id="d6eab-216">Skapa Azure ARM-värdanslutning i leverans Controller datorn tooprovision nya MCS kataloger i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-216">Create Azure ARM host connection in Delivery Controller machine tooprovision new MCS catalogs in Azure.</span></span> <span data-ttu-id="d6eab-217">Följ hello stegen som beskrivs i det här [artikel](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span><span class="sxs-lookup"><span data-stu-id="d6eab-217">Follow hello steps as explained in this [article](https://www.citrix.com/blogs/2016/07/21/connecting-to-azure-resource-manager-in-xenapp-xendesktop/).</span></span>

7. <span data-ttu-id="d6eab-218">Group3 manuell eller skript åtgärd 2:</span><span class="sxs-lookup"><span data-stu-id="d6eab-218">Group3 manual or script action 2:</span></span>

    <span data-ttu-id="d6eab-219">***Skapa nytt MCS kataloger i Azure***</span><span class="sxs-lookup"><span data-stu-id="d6eab-219">***Re-create MCS Catalogs in Azure***</span></span>

    <span data-ttu-id="d6eab-220">hello befintliga MCS eller PVS kloner på hello primära platsen inte replikerade tooAzure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-220">hello existing MCS or PVS clones on hello primary site will not be replicated tooAzure.</span></span> <span data-ttu-id="d6eab-221">Du måste toorecreate dessa kloner med hello replikerade master VDA och Azure ARM-etablering från leverans domänkontrollant. Följ hello stegen som beskrivs i det här [artikel](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate MCS kataloger i Azure.</span><span class="sxs-lookup"><span data-stu-id="d6eab-221">You need toorecreate these clones using hello replicated master VDA and Azure ARM provisioning from Delivery controller.Follow hello steps as explained in this [article](https://www.citrix.com/blogs/2016/09/12/using-xenapp-xendesktop-in-azure-resource-manager/) toocreate MCS catalogs in Azure.</span></span>

![Återställningsplan för XenApp komponenter](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-recoveryplan.png)


   >[!NOTE]
   ><span data-ttu-id="d6eab-223">Du kan använda skript på [plats](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS med hello nya IP-adresser för hello redundansväxlats > virtuella datorer eller tooattach belastningsutjämning på hello redundansväxlats virtuell dator, om det behövs.</span><span class="sxs-lookup"><span data-stu-id="d6eab-223">You can use scripts at [location](https://github.com/Azure/azure-quickstart-templates/blob/>master/asr-automation-recovery/scripts) tooupdate hello DNS with hello new IPs of hello failed over >virtual machines or tooattach a load balancer on hello failed over virtual machine, if needed.</span></span>


## <a name="doing-a-test-failover"></a><span data-ttu-id="d6eab-224">Gör ett redundanstest</span><span class="sxs-lookup"><span data-stu-id="d6eab-224">Doing a test failover</span></span>

<span data-ttu-id="d6eab-225">Följ [vägledningen](site-recovery-test-failover-to-azure.md) toodo testa redundans.</span><span class="sxs-lookup"><span data-stu-id="d6eab-225">Follow [this guidance](site-recovery-test-failover-to-azure.md) toodo a test failover.</span></span>

![Återställningsplan](./media/site-recovery-citrix-xenapp-and-xendesktop/citrix-tfo.png)


## <a name="doing-a-failover"></a><span data-ttu-id="d6eab-227">Genomför en redundansväxling enligt</span><span class="sxs-lookup"><span data-stu-id="d6eab-227">Doing a failover</span></span>

<span data-ttu-id="d6eab-228">Följ [vägledningen](site-recovery-failover.md) när du gör en redundansväxling.</span><span class="sxs-lookup"><span data-stu-id="d6eab-228">Follow [this guidance](site-recovery-failover.md) when you are doing a failover.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d6eab-229">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d6eab-229">Next steps</span></span>

<span data-ttu-id="d6eab-230">Du kan [mer](https://aka.ms/citrix-xenapp-xendesktop-with-asr) om att replikera Citrix XenApp och XenDesktop distributioner i detta dokument.</span><span class="sxs-lookup"><span data-stu-id="d6eab-230">You can [learn more](https://aka.ms/citrix-xenapp-xendesktop-with-asr) about replicating Citrix XenApp and XenDesktop deployments  in this white paper.</span></span> <span data-ttu-id="d6eab-231">Titta på hello vägledning för[replikera andra program](site-recovery-workload.md) med Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="d6eab-231">Look at hello guidance too[replicate other applications](site-recovery-workload.md) using Site Recovery.</span></span>
