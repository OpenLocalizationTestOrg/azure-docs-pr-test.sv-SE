---
title: aaaRemove servrar och inaktivera skydd | Microsoft Docs
description: "Den här artikeln beskriver hur toounregister servrar från en Site Recovery-valvet och toodisable skydd för virtuella datorer och fysiska servrar."
services: site-recovery
documentationcenter: 
author: rayne-wiselman
manager: cfreeman
editor: 
ms.assetid: ef1f31d5-285b-4a0f-89b5-0123cd422d80
ms.service: site-recovery
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: storage-backup-recovery
ms.date: 03/27/2017
ms.author: raynew
ms.openlocfilehash: 95f20433f782c93685ad4bae93c6bc0e2d2f2356
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="remove-servers-and-disable-protection"></a><span data-ttu-id="5ba98-103">Ta bort servrar och inaktivera skydd</span><span class="sxs-lookup"><span data-stu-id="5ba98-103">Remove servers and disable protection</span></span>

<span data-ttu-id="5ba98-104">hello Azure Site Recovery-tjänsten bidrar tooyour affärskontinuitet och haveriberedskap (BCDR).</span><span class="sxs-lookup"><span data-stu-id="5ba98-104">hello Azure Site Recovery service contributes tooyour business continuity and disaster recovery (BCDR) strategy.</span></span> <span data-ttu-id="5ba98-105">hello service samordnar replikering, redundans och återställning av virtuella datorer och fysiska servrar.</span><span class="sxs-lookup"><span data-stu-id="5ba98-105">hello service orchestrates replication, failover and recovery of virtual machines and physical servers.</span></span> <span data-ttu-id="5ba98-106">Datorer kan vara replikerade tooAzure eller tooa sekundärt lokalt datacenter.</span><span class="sxs-lookup"><span data-stu-id="5ba98-106">Machines can be replicated tooAzure, or tooa secondary on-premises data center.</span></span> <span data-ttu-id="5ba98-107">En snabb översikt finns i [Vad är Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="5ba98-107">For a quick overview read [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>

<span data-ttu-id="5ba98-108">Den här artikeln beskriver hur toounregister servrar från en återställningstjänster valvet i hello Azure-portalen och hur toodisable skydd för datorer som skyddas av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="5ba98-108">This article describes how toounregister servers from a Recovery Services vault in hello Azure portal, and how toodisable protection for machines protected by Site Recovery.</span></span>

<span data-ttu-id="5ba98-109">Skriv dina kommentarer eller frågor längst ned hello i den här artikeln eller i hello [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="5ba98-109">Post any comments or questions at hello bottom of this article, or on hello [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="unregister-a-connected-configuration-server"></a><span data-ttu-id="5ba98-110">Avregistrera en ansluten konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="5ba98-110">Unregister a connected configuration server</span></span>

<span data-ttu-id="5ba98-111">Om du replikera virtuella VMware-datorer eller Windows-/ Linux fysiska servrar tooAzure avregistrera du en ansluten konfigurationsservern från ett valv på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5ba98-111">If you replicate VMware VMs or Windows/Linux physical servers tooAzure, you can unregister a connected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="5ba98-112">Inaktivera skydd för datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-112">Disable machine protection.</span></span> <span data-ttu-id="5ba98-113">I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-113">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="5ba98-114">Kopplingen mellan alla principer.</span><span class="sxs-lookup"><span data-stu-id="5ba98-114">Disassociate any policies.</span></span> <span data-ttu-id="5ba98-115">I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **replikeringsprinciper**, dubbelklicka på hello associerade principer.</span><span class="sxs-lookup"><span data-stu-id="5ba98-115">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="5ba98-116">Högerklicka på hello konfigurationsservern > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-116">Right-click hello configuration server > **Disassociate**.</span></span>
3. <span data-ttu-id="5ba98-117">Ta bort alla ytterligare lokala process eller huvudmålservern servrar.</span><span class="sxs-lookup"><span data-stu-id="5ba98-117">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="5ba98-118">I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på hello server > **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-118">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click hello server > **Delete**.</span></span>
4. <span data-ttu-id="5ba98-119">Ta bort hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-119">Delete hello configuration server.</span></span>
5. <span data-ttu-id="5ba98-120">Avinstallera manuellt hello mobilitetstjänsten körs på hello huvudmålservern (det här är antingen en separat server eller körs på konfigurationsservern hello).</span><span class="sxs-lookup"><span data-stu-id="5ba98-120">Manually uninstall hello Mobility service running on hello master target server (this will be either a separate server, or running on hello configuration server).</span></span>
6. <span data-ttu-id="5ba98-121">Avinstallera alla ytterligare servrar.</span><span class="sxs-lookup"><span data-stu-id="5ba98-121">Uninstall any additional process servers.</span></span>
7. <span data-ttu-id="5ba98-122">Avinstallera hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-122">Uninstall hello configuration server.</span></span>
8. <span data-ttu-id="5ba98-123">Avinstallera hello instans av MySQL som har installerats med Site Recovery på hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-123">On hello configuration server, uninstall hello instance of MySQL that was installed by Site Recovery.</span></span>
9. <span data-ttu-id="5ba98-124">Ta bort hello nyckeln i hello register hello konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="5ba98-124">In hello registry of hello configuration server delete hello key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-unconnected-configuration-server"></a><span data-ttu-id="5ba98-125">Avregistrera en ansluten konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="5ba98-125">Unregister a unconnected configuration server</span></span>

<span data-ttu-id="5ba98-126">Om du replikera virtuella VMware-datorer eller Windows-/ Linux fysiska servrar tooAzure avregistrera du en ansluten konfigurationsservern från ett valv på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5ba98-126">If you replicate VMware VMs or Windows/Linux physical servers tooAzure, you can unregister an unconnected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="5ba98-127">Inaktivera skydd för datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-127">Disable machine protection.</span></span> <span data-ttu-id="5ba98-128">I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-128">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span> <span data-ttu-id="5ba98-129">Välj **sluta hantera hello datorn**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-129">Select **Stop managing hello machine**.</span></span>
2. <span data-ttu-id="5ba98-130">Ta bort alla ytterligare lokala process eller huvudmålservern servrar.</span><span class="sxs-lookup"><span data-stu-id="5ba98-130">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="5ba98-131">I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på hello server > **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-131">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click hello server > **Delete**.</span></span>
3. <span data-ttu-id="5ba98-132">Ta bort hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-132">Delete hello configuration server.</span></span>
4. <span data-ttu-id="5ba98-133">Avinstallera manuellt hello mobilitetstjänsten körs på hello huvudmålservern (det här är antingen en separat server eller körs på konfigurationsservern hello).</span><span class="sxs-lookup"><span data-stu-id="5ba98-133">Manually uninstall hello Mobility service running on hello master target server (this will be either a separate server, or running on hello configuration server).</span></span>
5. <span data-ttu-id="5ba98-134">Avinstallera alla ytterligare servrar.</span><span class="sxs-lookup"><span data-stu-id="5ba98-134">Uninstall any additional process servers.</span></span>
6. <span data-ttu-id="5ba98-135">Avinstallera hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-135">Uninstall hello configuration server.</span></span>
7. <span data-ttu-id="5ba98-136">Avinstallera hello instans av MySQL som har installerats med Site Recovery på hello konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-136">On hello configuration server, uninstall hello instance of MySQL that was installed by Site Recovery.</span></span>
8. <span data-ttu-id="5ba98-137">Ta bort hello nyckeln i hello register hello konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="5ba98-137">In hello registry of hello configuration server delete hello key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-connected-vmm-server"></a><span data-ttu-id="5ba98-138">Avregistrera en ansluten VMM-server</span><span class="sxs-lookup"><span data-stu-id="5ba98-138">Unregister a connected VMM server</span></span>

<span data-ttu-id="5ba98-139">Som bästa praxis rekommenderar vi att du avregistrera hello VMM-servern när den är ansluten tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5ba98-139">As a best practice, we recommend that you unregister hello VMM server when it's connected tooAzure.</span></span> <span data-ttu-id="5ba98-140">Detta säkerställer att inställningar på hello VMM-servrar (och på andra VMM-servrar med parad moln) rensas korrekt.</span><span class="sxs-lookup"><span data-stu-id="5ba98-140">This ensures that settings on hello VMM servers (and on other VMM servers with paired clouds) are cleaned up properly.</span></span> <span data-ttu-id="5ba98-141">Du bör bara ta bort en ansluten server om det är ett permanent problem med anslutningen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-141">You should only remove an unconnected server if there's a permanent issue with connectivity.</span></span> <span data-ttu-id="5ba98-142">Om hello VMM-servern inte är ansluten måste toomanually kör ett skript tooclean inställningarna.</span><span class="sxs-lookup"><span data-stu-id="5ba98-142">If hello VMM server isn’t connected, you will need toomanually run a script tooclean up settings.</span></span>

1. <span data-ttu-id="5ba98-143">Stoppa replikering av virtuella datorer i moln på hello VMM-servern som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="5ba98-143">Stop replicating VMs in clouds on hello VMM server you want tooremove.</span></span>
2. <span data-ttu-id="5ba98-144">Ta bort alla nätverksmappningar som används av moln på hello VMM-servern som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="5ba98-144">Delete any network mappings used by clouds on hello VMM server you want toodelete.</span></span> <span data-ttu-id="5ba98-145">I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på hello nätverksmappning >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-145">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click hello network mapping > **Delete**.</span></span>
3. <span data-ttu-id="5ba98-146">Kopplingen mellan replikeringsprinciper från moln på hello VMM-servern som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="5ba98-146">Disassociate replication policies from clouds on hello VMM server you want tooremove.</span></span>  <span data-ttu-id="5ba98-147">I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, genom att dubbelklicka på hello associerade.</span><span class="sxs-lookup"><span data-stu-id="5ba98-147">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="5ba98-148">Högerklicka på hello molntjänster > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-148">Right-click hello cloud > **Disassociate**.</span></span>
4. <span data-ttu-id="5ba98-149">Ta bort hello VMM-servern eller aktiv nod i VMM.</span><span class="sxs-lookup"><span data-stu-id="5ba98-149">Delete hello VMM server or active VMM node.</span></span> <span data-ttu-id="5ba98-150">I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på hello server >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-150">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click hello server > **Delete**.</span></span>
5. <span data-ttu-id="5ba98-151">Avinstallera hello providern manuellt på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-151">Uninstall hello Provider manually on hello VMM server.</span></span> <span data-ttu-id="5ba98-152">Om du har ett kluster kan du ta bort från alla noder.</span><span class="sxs-lookup"><span data-stu-id="5ba98-152">If you have a cluster, remove from all nodes.</span></span>
6. <span data-ttu-id="5ba98-153">Om du replikerar tooAzure du manuellt ta bort hello Microsoft Recovery Services-agenten från Hyper-V-värdar i hello bort moln.</span><span class="sxs-lookup"><span data-stu-id="5ba98-153">If you're replicating tooAzure, manually remove hello Microsoft Recovery Services agent from Hyper-V hosts in hello deleted clouds.</span></span>



### <a name="unregister-an-unconnected-vmm-server"></a><span data-ttu-id="5ba98-154">Avregistrera en ansluten VMM-servern</span><span class="sxs-lookup"><span data-stu-id="5ba98-154">Unregister an unconnected VMM server</span></span>

1. <span data-ttu-id="5ba98-155">Stoppa replikering av virtuella datorer i moln på hello VMM-servern som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="5ba98-155">Stop replicating VMs in clouds on hello VMM server you want tooremove.</span></span>
2. <span data-ttu-id="5ba98-156">Ta bort alla nätverksmappningar som används av moln på hello VMM-servern som du vill toodelete.</span><span class="sxs-lookup"><span data-stu-id="5ba98-156">Delete any network mappings used by clouds on hello VMM server that you want toodelete.</span></span> <span data-ttu-id="5ba98-157">I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på hello nätverksmappning >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-157">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click hello network mapping > **Delete**.</span></span>
3. <span data-ttu-id="5ba98-158">Observera hello-ID för hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-158">Note hello ID of hello VMM server.</span></span>
4. <span data-ttu-id="5ba98-159">Kopplingen mellan replikeringsprinciper från moln på hello VMM-servern som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="5ba98-159">Disassociate replication policies from clouds on hello VMM server you want tooremove.</span></span>  <span data-ttu-id="5ba98-160">I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, genom att dubbelklicka på hello associerade.</span><span class="sxs-lookup"><span data-stu-id="5ba98-160">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="5ba98-161">Högerklicka på hello molntjänster > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-161">Right-click hello cloud > **Disassociate**.</span></span>
5. <span data-ttu-id="5ba98-162">Ta bort hello VMM-servern eller aktiva noden.</span><span class="sxs-lookup"><span data-stu-id="5ba98-162">Delete hello VMM server or active node.</span></span> <span data-ttu-id="5ba98-163">I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på hello server >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-163">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click hello server > **Delete**.</span></span>
6. <span data-ttu-id="5ba98-164">Hämta och köra hello [rensningsskript](http://aka.ms/asr-cleanup-script-vmm) på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-164">Download and run hello [cleanup script](http://aka.ms/asr-cleanup-script-vmm) on hello VMM server.</span></span> <span data-ttu-id="5ba98-165">Öppna PowerShell med hello **kör som administratör** alternativ, toochange hello körningsprincipen för hello standard (LocalMachine) omfång.</span><span class="sxs-lookup"><span data-stu-id="5ba98-165">Open PowerShell with hello **Run as Administrator** option, toochange hello execution policy for hello default (LocalMachine) scope.</span></span> <span data-ttu-id="5ba98-166">Ange hello-ID för hello VMM-servern som du vill tooremove i hello skript.</span><span class="sxs-lookup"><span data-stu-id="5ba98-166">In hello script, specify hello ID of hello VMM server you want tooremove.</span></span> <span data-ttu-id="5ba98-167">hello skriptet tar bort registrering och molnet länkning av information från hello-server.</span><span class="sxs-lookup"><span data-stu-id="5ba98-167">hello script removes registration and cloud pairing information from hello server.</span></span>
5. <span data-ttu-id="5ba98-168">Kör hello rensningsskript på andra VMM-servrar som innehåller moln som har parats ihop med moln på hello VMM-servern som du vill tooremove.</span><span class="sxs-lookup"><span data-stu-id="5ba98-168">Run hello cleanup script on any other VMM servers that contain clouds that are paired with clouds on hello VMM server you want tooremove.</span></span>
6. <span data-ttu-id="5ba98-169">Kör hello Rensa skriptet på alla andra passiva VMM-klusternoder som har hello-Provider installerad.</span><span class="sxs-lookup"><span data-stu-id="5ba98-169">Run hello  cleanup script on any other passive VMM cluster nodes that have hello Provider installed.</span></span>
7. <span data-ttu-id="5ba98-170">Avinstallera hello providern manuellt på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-170">Uninstall hello Provider manually on hello VMM server.</span></span> <span data-ttu-id="5ba98-171">Om du har ett kluster kan du ta bort från alla noder.</span><span class="sxs-lookup"><span data-stu-id="5ba98-171">If you have a cluster, remove from all nodes.</span></span>
8. <span data-ttu-id="5ba98-172">Om du replikerar tooAzure, du kan ta bort hello Microsoft Recovery Services-agenten från Hyper-V-värdar i hello bort moln.</span><span class="sxs-lookup"><span data-stu-id="5ba98-172">If you replicating tooAzure, you can remove hello Microsoft Recovery Services agent from Hyper-V hosts in hello deleted clouds.</span></span>

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a><span data-ttu-id="5ba98-173">Avregistrera en Hyper-V-värd i en Hyper-V-plats</span><span class="sxs-lookup"><span data-stu-id="5ba98-173">Unregister a Hyper-V host in a Hyper-V Site</span></span>

<span data-ttu-id="5ba98-174">Hyper-V-värdar som inte hanteras av VMM samlas till en Hyper-V-plats.</span><span class="sxs-lookup"><span data-stu-id="5ba98-174">Hyper-V hosts that aren't managed by VMM are gathered into a Hyper-V site.</span></span> <span data-ttu-id="5ba98-175">Ta bort en värd i en Hyper-V-plats på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="5ba98-175">Remove a host in a Hyper-V site as follows:</span></span>

1. <span data-ttu-id="5ba98-176">Inaktivera replikering för Hyper-V virtuella datorer finns på hello värd.</span><span class="sxs-lookup"><span data-stu-id="5ba98-176">Disable replication for Hyper-V VMs located on hello host.</span></span>
2. <span data-ttu-id="5ba98-177">Kopplingen mellan principer för hello Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-177">Disassociate policies for hello Hyper-V site.</span></span> <span data-ttu-id="5ba98-178">I **Site Recovery-infrastruktur** > **för Hyper-V-platser** >  **replikeringsprinciper**, genom att dubbelklicka på hello associerade.</span><span class="sxs-lookup"><span data-stu-id="5ba98-178">In **Site Recovery Infrastructure** > **For Hyper-V Sites** >  **Replication Policies**, double-click hello associated policy.</span></span> <span data-ttu-id="5ba98-179">Högerklicka på hello plats > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-179">Right-click hello site > **Disassociate**.</span></span>
3. <span data-ttu-id="5ba98-180">Ta bort Hyper-V-värdar.</span><span class="sxs-lookup"><span data-stu-id="5ba98-180">Delete Hyper-V hosts.</span></span> <span data-ttu-id="5ba98-181">I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-värdar**, högerklicka på hello server >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-181">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Hosts**, right-click hello server > **Delete**.</span></span>
4. <span data-ttu-id="5ba98-182">Ta bort hello Hyper-V-platsen när alla värdar har tagits bort från den.</span><span class="sxs-lookup"><span data-stu-id="5ba98-182">Delete hello Hyper-V site after all hosts have been removed from it.</span></span> <span data-ttu-id="5ba98-183">I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-platser**, högerklicka på hello plats >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-183">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Sites**, right-click hello site > **Delete**.</span></span>
5. <span data-ttu-id="5ba98-184">Kör följande skript på varje Hyper-V-värd som du har tagit bort hello.</span><span class="sxs-lookup"><span data-stu-id="5ba98-184">Run hello following script on each Hyper-V host that you removed.</span></span> <span data-ttu-id="5ba98-185">hello skriptet rensar inställningarna på hello-servern och Avregistrerar från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="5ba98-185">hello script cleans up settings on hello server, and unregisters it from hello vault.</span></span>


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run hello script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove hello old Azure Site Recovery Provider related properties. Do you want toocontinue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping hello Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'

            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."    
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
            {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {            
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                "Registry keys removed."
            }

            # First retrive all hello certificates toobe deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete hello certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {    
            [system.exception]
            Write-Host "Error occured" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd``



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a><span data-ttu-id="5ba98-186">Inaktivera skyddet för en VMware-VM eller fysiska servrar</span><span class="sxs-lookup"><span data-stu-id="5ba98-186">Disable protection for a VMware VM or physical server</span></span>

1. <span data-ttu-id="5ba98-187">I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-187">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="5ba98-188">I **ta bort datorn**, väljer du något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="5ba98-188">In **Remove Machine**, select one of these options:</span></span>
    - <span data-ttu-id="5ba98-189">**Inaktivera skyddet för hello datorn (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-189">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="5ba98-190">Använd det här alternativet toostop replikerar hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-190">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="5ba98-191">Site Recovery-inställningarna kommer att rensas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="5ba98-191">Site Recovery settings will be cleaned up automatically.</span></span> <span data-ttu-id="5ba98-192">Endast visas det här alternativet i hello följande omständigheter:</span><span class="sxs-lookup"><span data-stu-id="5ba98-192">You will only see this option in hello following circumstances:</span></span>
        - <span data-ttu-id="5ba98-193">**Du har ändrat storlek hello VM volym**– när du ändrar storlek på en volym hello virtuella datorn försätts i ett kritiskt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="5ba98-193">**You've resized hello VM volume**—When you resize a volume hello virtual machine goes into a critical state.</span></span> <span data-ttu-id="5ba98-194">Välj det här alternativet toodisables skyddet samtidigt som företaget behåller återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="5ba98-194">Select this option toodisables protection while retaining recovery points in Azure.</span></span> <span data-ttu-id="5ba98-195">När du aktiverar skyddet för hello datorn igen kommer hello data för hello storlek volymen att överförda tooAzure.</span><span class="sxs-lookup"><span data-stu-id="5ba98-195">When you enable protection for hello machine again, hello data for hello resized volume will be transferred tooAzure.</span></span>
        - <span data-ttu-id="5ba98-196">**Du har nyligen kör redundans**– när du har kört en växling vid fel tootest din miljö, Välj det här alternativet toostart skydda lokala datorer igen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-196">**You've recently run a failover**—After you've run a failover tootest your environment, select this option toostart protecting on-premises machines again.</span></span> <span data-ttu-id="5ba98-197">Varje virtuell dator inaktiveras och måste tooenable skyddet för dem igen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-197">It disables each virtual machine, and then you need tooenable protection for them again.</span></span> <span data-ttu-id="5ba98-198">Inaktivera hello-datorn med den här inställningen påverkar inte hello replikerade virtuella datorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="5ba98-198">Disabling hello machine with this setting doesn't affect hello replica virtual machine in Azure.</span></span> <span data-ttu-id="5ba98-199">Inte avinstallera mobilitetstjänsten för hello från hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-199">Don't uninstall hello Mobility service from hello machine.</span></span>
    - <span data-ttu-id="5ba98-200">**Sluta hantera hello datorn**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-200">**Stop managing hello machine**.</span></span> <span data-ttu-id="5ba98-201">Om du väljer det här alternativet tas hello datorn endast bort från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="5ba98-201">If you select this option, hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="5ba98-202">Lokala skyddsinställningarna för hello datorn påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="5ba98-202">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="5ba98-203">tooremove inställningarna på hello datorn och tooremove hello dator från hello Azure-prenumeration, du måste tooclean hello inställningar genom att avinstallera mobilitetstjänsten för hello.</span><span class="sxs-lookup"><span data-stu-id="5ba98-203">tooremove settings on hello machine, and tooremove hello machine from hello Azure subscription, you need tooclean hello settings by uninstalling hello Mobility service.</span></span>

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a><span data-ttu-id="5ba98-204">Inaktivera skyddet för en Hyper-V virtuell dator i VMM-moln</span><span class="sxs-lookup"><span data-stu-id="5ba98-204">Disable protection for a Hyper-V VM in a VMM cloud</span></span>

1. <span data-ttu-id="5ba98-205">I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-205">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="5ba98-206">I **ta bort datorn**, väljer du något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="5ba98-206">In **Remove Machine**, select one of these options:</span></span>

    - <span data-ttu-id="5ba98-207">**Inaktivera skyddet för hello datorn (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-207">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="5ba98-208">Använd det här alternativet toostop replikerar hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-208">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="5ba98-209">Site Recovery-inställningarna kommer att rensas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="5ba98-209">Site Recovery settings will be cleaned up automatically.</span></span>
    - <span data-ttu-id="5ba98-210">**Sluta hantera hello datorn**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-210">**Stop managing hello machine**.</span></span> <span data-ttu-id="5ba98-211">Om du väljer det här alternativet tas hello datorn endast bort från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="5ba98-211">If you select this option, hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="5ba98-212">Lokala skyddsinställningarna för hello datorn påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="5ba98-212">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="5ba98-213">tooremove inställningarna på hello datorn och tooremove hello dator från hello Azure-prenumeration, måste tooclean hello inställningar in manuellt med hjälp av hello anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="5ba98-213">tooremove settings on hello machine, and tooremove hello machine from hello Azure subscription, you need tooclean hello settings up manually, using hello instructions below.</span></span> <span data-ttu-id="5ba98-214">Observera att om du väljer toodelete hello virtuell dator och datorns hårddiskar måste de ska tas bort från hello målplats.</span><span class="sxs-lookup"><span data-stu-id="5ba98-214">Note that if you select toodelete hello virtual machine and its hard disks, they'll be removed from hello target location.</span></span>

### <a name="clean-up-protection-settings---replication-tooa-secondary-vmm-site"></a><span data-ttu-id="5ba98-215">Rensa skyddsinställningarna - replikering tooa sekundär VMM-plats</span><span class="sxs-lookup"><span data-stu-id="5ba98-215">Clean up protection settings - replication tooa secondary VMM site</span></span>

<span data-ttu-id="5ba98-216">Om du har valt **sluta hantera hello datorn** och replikerar tooa sekundär plats, kör skriptet på hello primärservern tooclean hello inställningarna för hello primära virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-216">If you selected **Stop managing hello machine** and you replicating tooa secondary site, run this script on hello primary server tooclean up hello settings for hello primary virtual machine.</span></span> <span data-ttu-id="5ba98-217">Klicka på hello PowerShell knappen tooopen hello VMM PowerShell-konsolen i hello VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-217">In hello VMM console click hello PowerShell button tooopen hello VMM PowerShell console.</span></span> <span data-ttu-id="5ba98-218">Ersätt SQLVM1 med hello namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-218">Replace SQLVM1 with hello name of your virtual machine.</span></span>

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="5ba98-219">Kör det här skriptet tooclean hello inställningarna för hello sekundära virtuella datorn på hello sekundär VMM-servern:</span><span class="sxs-lookup"><span data-stu-id="5ba98-219">On hello secondary VMM server run this script tooclean up hello settings for hello secondary virtual machine:</span></span>

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. <span data-ttu-id="5ba98-220">Uppdatera hello virtuella datorer på hello Hyper-V-värdservern på hello sekundär VMM-servern så att hello sekundära VM hämtar identifieras igen hello VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-220">On hello secondary VMM server, refresh hello virtual machines on hello Hyper-V host server, so that hello secondary VM gets detected again in hello VMM console.</span></span>
4. <span data-ttu-id="5ba98-221">hello senare steg Rensa hello replikeringsinställningarna på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-221">hello above steps clear up hello replication settings on hello VMM server.</span></span> <span data-ttu-id="5ba98-222">Om du vill toostop replikering för hello virtuell dator, kör följande skript OJ hello hello primära och sekundära virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="5ba98-222">If you want toostop replication for hello virtual machine, run hello following script oh hello primary and secondary VMs.</span></span> <span data-ttu-id="5ba98-223">Ersätt SQLVM1 med hello namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-223">Replace SQLVM1 with hello name of your virtual machine.</span></span>

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-tooazure"></a><span data-ttu-id="5ba98-224">Rensa skyddsinställningarna - replikering tooAzure</span><span class="sxs-lookup"><span data-stu-id="5ba98-224">Clean up protection settings - replication tooAzure</span></span>

1. <span data-ttu-id="5ba98-225">Om du har valt **sluta hantera hello datorn** och replikera tooAzure kan du köra skriptet på hello VMM-källservern med hjälp av PowerShell från hello VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-225">If you selected **Stop managing hello machine** and you replicate tooAzure, run this script on hello source VMM server, using PowerShell from hello VMM console.</span></span>
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="5ba98-226">hello senare steg avmarkera hello replikeringsinställningarna på hello VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-226">hello above steps clear hello replication settings on hello VMM server.</span></span> <span data-ttu-id="5ba98-227">toostop replikering för hello virtuell dator som kör på hello Hyper-V-värdservern kör det här skriptet.</span><span class="sxs-lookup"><span data-stu-id="5ba98-227">toostop replication for hello virtual machine running on hello Hyper-V host server, run this script.</span></span> <span data-ttu-id="5ba98-228">Ersätt SQLVM1 med hello namnet på den virtuella datorn och host01.contoso.com med hello namnet på hello Hyper-V-värdservern.</span><span class="sxs-lookup"><span data-stu-id="5ba98-228">Replace SQLVM1 with hello name of your virtual machine, and host01.contoso.com with hello name of hello Hyper-V host server.</span></span>

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a><span data-ttu-id="5ba98-229">Inaktivera skyddet för en Hyper-V virtuell dator på en Hyper-V-plats</span><span class="sxs-lookup"><span data-stu-id="5ba98-229">Disable protection for a Hyper-V VM in a Hyper-V Site</span></span>

<span data-ttu-id="5ba98-230">Använd följande procedur om du replikerar virtuella Hyper-V-datorer tooAzure utan en VMM-server.</span><span class="sxs-lookup"><span data-stu-id="5ba98-230">Use this procedure if you're replicating Hyper-V VMs tooAzure without a VMM server.</span></span>

1. <span data-ttu-id="5ba98-231">I **skyddade objekt** > **replikerade objekt**, högerklicka på hello datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-231">In **Protected Items** > **Replicated Items**, right-click hello machine > **Delete**.</span></span>
2. <span data-ttu-id="5ba98-232">I **ta bort datorn**, kan du välja hello följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="5ba98-232">In **Remove Machine**, you can select hello following options:</span></span>

   - <span data-ttu-id="5ba98-233">**Inaktivera skyddet för hello datorn (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-233">**Disable protection for hello machine (recommended)**.</span></span> <span data-ttu-id="5ba98-234">Använd det här alternativet toostop replikerar hello-datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-234">Use this option toostop replicating hello machine.</span></span> <span data-ttu-id="5ba98-235">Site Recovery-inställningarna kommer att rensas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="5ba98-235">Site Recovery settings will be cleaned up automatically.</span></span>
   - <span data-ttu-id="5ba98-236">**Sluta hantera hello datorn**.</span><span class="sxs-lookup"><span data-stu-id="5ba98-236">**Stop managing hello machine**.</span></span> <span data-ttu-id="5ba98-237">Om du väljer det här alternativet hello datorn endast tas bort från hello-valvet.</span><span class="sxs-lookup"><span data-stu-id="5ba98-237">If you select this option hello machine will only be removed from hello vault.</span></span> <span data-ttu-id="5ba98-238">Lokala skyddsinställningarna för hello datorn påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="5ba98-238">On-premises protection settings for hello machine won’t be affected.</span></span> <span data-ttu-id="5ba98-239">tooremove inställningarna på hello datorn och tooremove hello virtuell dator från hello Azure-prenumeration, måste tooclean hello inställningar in manuellt.</span><span class="sxs-lookup"><span data-stu-id="5ba98-239">tooremove settings on hello machine, and tooremove hello virtual machine from hello Azure subscription, you need tooclean hello settings up manually.</span></span> <span data-ttu-id="5ba98-240">Om du väljer toodelete hello virtuella datorn och dess hårddiskar tas bort från hello målplatsen.</span><span class="sxs-lookup"><span data-stu-id="5ba98-240">If you select toodelete hello virtual machine and its hard disks they will be removed from hello target location.</span></span>
3. <span data-ttu-id="5ba98-241">Om du har valt **sluta hantera hello datorn**, kör det här skriptet på hello källa Hyper-V-värdservern, tooremove replikering för hello virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="5ba98-241">If you selected **Stop managing hello machine**, run this script on hello source Hyper-V host server, tooremove replication for hello virtual machine.</span></span> <span data-ttu-id="5ba98-242">Ersätt SQLVM1 med hello namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="5ba98-242">Replace SQLVM1 with hello name of your virtual machine.</span></span>

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
