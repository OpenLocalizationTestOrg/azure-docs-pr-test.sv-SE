---
title: Ta bort servrar och inaktivera skydd | Microsoft Docs
description: "Den här artikeln beskrivs hur du avregistrera servrar från en Site Recovery-valvet och att inaktivera skydd för virtuella datorer och fysiska servrar."
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
ms.openlocfilehash: 43f92a35dc9b04584badd1c9f1152470246b5012
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="remove-servers-and-disable-protection"></a><span data-ttu-id="f7837-103">Ta bort servrar och inaktivera skydd</span><span class="sxs-lookup"><span data-stu-id="f7837-103">Remove servers and disable protection</span></span>

<span data-ttu-id="f7837-104">Azure Site Recovery-tjänsten bidrar till din affärskontinuitet och haveriberedskap (BCDR) strategi.</span><span class="sxs-lookup"><span data-stu-id="f7837-104">The Azure Site Recovery service contributes to your business continuity and disaster recovery (BCDR) strategy.</span></span> <span data-ttu-id="f7837-105">Tjänsten samordnar replikering, redundans och återställning av virtuella datorer och fysiska servrar.</span><span class="sxs-lookup"><span data-stu-id="f7837-105">The service orchestrates replication, failover and recovery of virtual machines and physical servers.</span></span> <span data-ttu-id="f7837-106">Datorer kan replikeras till Azure eller till ett sekundärt lokalt datacenter.</span><span class="sxs-lookup"><span data-stu-id="f7837-106">Machines can be replicated to Azure, or to a secondary on-premises data center.</span></span> <span data-ttu-id="f7837-107">En snabb översikt finns i [Vad är Azure Site Recovery?](site-recovery-overview.md)</span><span class="sxs-lookup"><span data-stu-id="f7837-107">For a quick overview read [What is Azure Site Recovery?](site-recovery-overview.md)</span></span>

<span data-ttu-id="f7837-108">Den här artikeln beskriver hur du avregistrera servrar från en Recovery Services-valvet i Azure-portalen och hur du inaktiverar skydd för datorer som skyddas av Site Recovery.</span><span class="sxs-lookup"><span data-stu-id="f7837-108">This article describes how to unregister servers from a Recovery Services vault in the Azure portal, and how to disable protection for machines protected by Site Recovery.</span></span>

<span data-ttu-id="f7837-109">Skriv dina kommentarer eller frågor längst ned i den här artikeln eller i [Azure Recovery Services-forumet](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span><span class="sxs-lookup"><span data-stu-id="f7837-109">Post any comments or questions at the bottom of this article, or on the [Azure Recovery Services Forum](https://social.msdn.microsoft.com/forums/azure/home?forum=hypervrecovmgr).</span></span>

## <a name="unregister-a-connected-configuration-server"></a><span data-ttu-id="f7837-110">Avregistrera en ansluten konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="f7837-110">Unregister a connected configuration server</span></span>

<span data-ttu-id="f7837-111">Om du replikerar virtuella VMware-datorer eller Windows-/ Linux fysiska servrar till Azure kan du avregistrera en ansluten konfigurationsservern från ett valv på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f7837-111">If you replicate VMware VMs or Windows/Linux physical servers to Azure, you can unregister a connected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="f7837-112">Inaktivera skydd för datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-112">Disable machine protection.</span></span> <span data-ttu-id="f7837-113">I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-113">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="f7837-114">Kopplingen mellan alla principer.</span><span class="sxs-lookup"><span data-stu-id="f7837-114">Disassociate any policies.</span></span> <span data-ttu-id="f7837-115">I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **replikeringsprinciper**, dubbelklicka på den associerade princip.</span><span class="sxs-lookup"><span data-stu-id="f7837-115">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="f7837-116">Högerklicka på konfigurationsservern > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="f7837-116">Right-click the configuration server > **Disassociate**.</span></span>
3. <span data-ttu-id="f7837-117">Ta bort alla ytterligare lokala process eller huvudmålservern servrar.</span><span class="sxs-lookup"><span data-stu-id="f7837-117">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="f7837-118">I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på servern > **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-118">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click the server > **Delete**.</span></span>
4. <span data-ttu-id="f7837-119">Ta bort konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="f7837-119">Delete the configuration server.</span></span>
5. <span data-ttu-id="f7837-120">Avinstallera mobilitetstjänsten på huvudmålservern manuellt (det här är antingen en separat server eller körs på konfigurationsservern).</span><span class="sxs-lookup"><span data-stu-id="f7837-120">Manually uninstall the Mobility service running on the master target server (this will be either a separate server, or running on the configuration server).</span></span>
6. <span data-ttu-id="f7837-121">Avinstallera alla ytterligare servrar.</span><span class="sxs-lookup"><span data-stu-id="f7837-121">Uninstall any additional process servers.</span></span>
7. <span data-ttu-id="f7837-122">Avinstallera konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="f7837-122">Uninstall the configuration server.</span></span>
8. <span data-ttu-id="f7837-123">Avinstallera instansen av MySQL som har installerats med Site Recovery på konfigurationsservern och.</span><span class="sxs-lookup"><span data-stu-id="f7837-123">On the configuration server, uninstall the instance of MySQL that was installed by Site Recovery.</span></span>
9. <span data-ttu-id="f7837-124">Ta bort nyckeln i registret på konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="f7837-124">In the registry of the configuration server delete the key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-unconnected-configuration-server"></a><span data-ttu-id="f7837-125">Avregistrera en ansluten konfigurationsservern</span><span class="sxs-lookup"><span data-stu-id="f7837-125">Unregister a unconnected configuration server</span></span>

<span data-ttu-id="f7837-126">Om du replikerar virtuella VMware-datorer eller Windows-/ Linux fysiska servrar till Azure kan du avregistrera en ansluten konfigurationsservern från ett valv på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f7837-126">If you replicate VMware VMs or Windows/Linux physical servers to Azure, you can unregister an unconnected configuration server from a vault as follows:</span></span>

1. <span data-ttu-id="f7837-127">Inaktivera skydd för datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-127">Disable machine protection.</span></span> <span data-ttu-id="f7837-128">I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-128">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span> <span data-ttu-id="f7837-129">Välj **sluta hantera datorn**.</span><span class="sxs-lookup"><span data-stu-id="f7837-129">Select **Stop managing the machine**.</span></span>
2. <span data-ttu-id="f7837-130">Ta bort alla ytterligare lokala process eller huvudmålservern servrar.</span><span class="sxs-lookup"><span data-stu-id="f7837-130">Remove any additional on-premises process or master target servers.</span></span> <span data-ttu-id="f7837-131">I **Site Recovery-infrastruktur** > **för VMWare och fysiska datorer** > **Konfigurationsservrar**, högerklicka på servern > **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-131">In **Site Recovery Infrastructure** > **For VMWare & Physical Machines** > **Configuration Servers**, right-click the server > **Delete**.</span></span>
3. <span data-ttu-id="f7837-132">Ta bort konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="f7837-132">Delete the configuration server.</span></span>
4. <span data-ttu-id="f7837-133">Avinstallera mobilitetstjänsten på huvudmålservern manuellt (det här är antingen en separat server eller körs på konfigurationsservern).</span><span class="sxs-lookup"><span data-stu-id="f7837-133">Manually uninstall the Mobility service running on the master target server (this will be either a separate server, or running on the configuration server).</span></span>
5. <span data-ttu-id="f7837-134">Avinstallera alla ytterligare servrar.</span><span class="sxs-lookup"><span data-stu-id="f7837-134">Uninstall any additional process servers.</span></span>
6. <span data-ttu-id="f7837-135">Avinstallera konfigurationsservern.</span><span class="sxs-lookup"><span data-stu-id="f7837-135">Uninstall the configuration server.</span></span>
7. <span data-ttu-id="f7837-136">Avinstallera instansen av MySQL som har installerats med Site Recovery på konfigurationsservern och.</span><span class="sxs-lookup"><span data-stu-id="f7837-136">On the configuration server, uninstall the instance of MySQL that was installed by Site Recovery.</span></span>
8. <span data-ttu-id="f7837-137">Ta bort nyckeln i registret på konfigurationsservern ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span><span class="sxs-lookup"><span data-stu-id="f7837-137">In the registry of the configuration server delete the key ``HKEY_LOCAL_MACHINE\Software\Microsoft\Azure Site Recovery``.</span></span>

## <a name="unregister-a-connected-vmm-server"></a><span data-ttu-id="f7837-138">Avregistrera en ansluten VMM-server</span><span class="sxs-lookup"><span data-stu-id="f7837-138">Unregister a connected VMM server</span></span>

<span data-ttu-id="f7837-139">Som bästa praxis rekommenderar vi att du avregistrera VMM-servern när den är ansluten till Azure.</span><span class="sxs-lookup"><span data-stu-id="f7837-139">As a best practice, we recommend that you unregister the VMM server when it's connected to Azure.</span></span> <span data-ttu-id="f7837-140">Detta säkerställer att inställningar på VMM-servrar (och på andra VMM-servrar med parad moln) rensas korrekt.</span><span class="sxs-lookup"><span data-stu-id="f7837-140">This ensures that settings on the VMM servers (and on other VMM servers with paired clouds) are cleaned up properly.</span></span> <span data-ttu-id="f7837-141">Du bör bara ta bort en ansluten server om det är ett permanent problem med anslutningen.</span><span class="sxs-lookup"><span data-stu-id="f7837-141">You should only remove an unconnected server if there's a permanent issue with connectivity.</span></span> <span data-ttu-id="f7837-142">Om VMM-servern inte är ansluten, behöver du köra ett skript för att rensa inställningarna manuellt.</span><span class="sxs-lookup"><span data-stu-id="f7837-142">If the VMM server isn’t connected, you will need to manually run a script to clean up settings.</span></span>

1. <span data-ttu-id="f7837-143">Stoppa replikering av virtuella datorer i moln på VMM-servern som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-143">Stop replicating VMs in clouds on the VMM server you want to remove.</span></span>
2. <span data-ttu-id="f7837-144">Ta bort alla nätverksmappningar som används av moln på VMM-servern som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-144">Delete any network mappings used by clouds on the VMM server you want to delete.</span></span> <span data-ttu-id="f7837-145">I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på nätverksmappningen > **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-145">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click the network mapping > **Delete**.</span></span>
3. <span data-ttu-id="f7837-146">Kopplingen mellan replikeringsprinciper från moln på VMM-servern som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-146">Disassociate replication policies from clouds on the VMM server you want to remove.</span></span>  <span data-ttu-id="f7837-147">I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, dubbelklicka på den associera principen.</span><span class="sxs-lookup"><span data-stu-id="f7837-147">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="f7837-148">Högerklicka på molnet > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="f7837-148">Right-click the cloud > **Disassociate**.</span></span>
4. <span data-ttu-id="f7837-149">Ta bort VMM-servern eller aktiv nod i VMM.</span><span class="sxs-lookup"><span data-stu-id="f7837-149">Delete the VMM server or active VMM node.</span></span> <span data-ttu-id="f7837-150">I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på servern > **ta bort** .</span><span class="sxs-lookup"><span data-stu-id="f7837-150">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click the server > **Delete**.</span></span>
5. <span data-ttu-id="f7837-151">Avinstallera providern på VMM-servern manuellt.</span><span class="sxs-lookup"><span data-stu-id="f7837-151">Uninstall the Provider manually on the VMM server.</span></span> <span data-ttu-id="f7837-152">Om du har ett kluster kan du ta bort från alla noder.</span><span class="sxs-lookup"><span data-stu-id="f7837-152">If you have a cluster, remove from all nodes.</span></span>
6. <span data-ttu-id="f7837-153">Om du replikerar till Azure du manuellt ta bort Microsoft Recovery Services-agenten från Hyper-V-värdar i de borttagna moln.</span><span class="sxs-lookup"><span data-stu-id="f7837-153">If you're replicating to Azure, manually remove the Microsoft Recovery Services agent from Hyper-V hosts in the deleted clouds.</span></span>



### <a name="unregister-an-unconnected-vmm-server"></a><span data-ttu-id="f7837-154">Avregistrera en ansluten VMM-servern</span><span class="sxs-lookup"><span data-stu-id="f7837-154">Unregister an unconnected VMM server</span></span>

1. <span data-ttu-id="f7837-155">Stoppa replikering av virtuella datorer i moln på VMM-servern som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-155">Stop replicating VMs in clouds on the VMM server you want to remove.</span></span>
2. <span data-ttu-id="f7837-156">Ta bort alla nätverksmappningar som används av moln på VMM-servern som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-156">Delete any network mappings used by clouds on the VMM server that you want to delete.</span></span> <span data-ttu-id="f7837-157">I **Site Recovery-infrastruktur** > **för System Center VMM** > **nätverksmappning**, högerklicka på nätverksmappningen > **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-157">In **Site Recovery Infrastructure** > **For System Center VMM** > **Network Mapping**, right-click the network mapping > **Delete**.</span></span>
3. <span data-ttu-id="f7837-158">Observera ID för VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="f7837-158">Note the ID of the VMM server.</span></span>
4. <span data-ttu-id="f7837-159">Kopplingen mellan replikeringsprinciper från moln på VMM-servern som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-159">Disassociate replication policies from clouds on the VMM server you want to remove.</span></span>  <span data-ttu-id="f7837-160">I **Site Recovery-infrastruktur** > **för System Center VMM** >  **replikeringsprinciper**, dubbelklicka på den associera principen.</span><span class="sxs-lookup"><span data-stu-id="f7837-160">In **Site Recovery Infrastructure** > **For System Center VMM** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="f7837-161">Högerklicka på molnet > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="f7837-161">Right-click the cloud > **Disassociate**.</span></span>
5. <span data-ttu-id="f7837-162">Ta bort VMM-servern eller aktiva noden.</span><span class="sxs-lookup"><span data-stu-id="f7837-162">Delete the VMM server or active node.</span></span> <span data-ttu-id="f7837-163">I **Site Recovery-infrastruktur** > **för System Center VMM** > **VMM-servrar**, högerklicka på servern > **ta bort** .</span><span class="sxs-lookup"><span data-stu-id="f7837-163">In **Site Recovery Infrastructure** > **For System Center VMM** > **VMM Servers**, right-click the server > **Delete**.</span></span>
6. <span data-ttu-id="f7837-164">Hämta och kör den [rensningsskript](http://aka.ms/asr-cleanup-script-vmm) på VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="f7837-164">Download and run the [cleanup script](http://aka.ms/asr-cleanup-script-vmm) on the VMM server.</span></span> <span data-ttu-id="f7837-165">Öppna PowerShell med den **kör som administratör** alternativet för att ändra körningsprincipen för standardomfånget som (LocalMachine).</span><span class="sxs-lookup"><span data-stu-id="f7837-165">Open PowerShell with the **Run as Administrator** option, to change the execution policy for the default (LocalMachine) scope.</span></span> <span data-ttu-id="f7837-166">Ange ID för VMM-servern som du vill ta bort i skriptet.</span><span class="sxs-lookup"><span data-stu-id="f7837-166">In the script, specify the ID of the VMM server you want to remove.</span></span> <span data-ttu-id="f7837-167">Skriptet tar bort registrering och molnet länkning av information från servern.</span><span class="sxs-lookup"><span data-stu-id="f7837-167">The script removes registration and cloud pairing information from the server.</span></span>
5. <span data-ttu-id="f7837-168">Kör skriptet för rensning på andra VMM-servrar som innehåller moln som har parats ihop med moln på VMM-servern som du vill ta bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-168">Run the cleanup script on any other VMM servers that contain clouds that are paired with clouds on the VMM server you want to remove.</span></span>
6. <span data-ttu-id="f7837-169">Kör skriptet för rensning på alla andra passiva VMM-klusternoder som har installerad.</span><span class="sxs-lookup"><span data-stu-id="f7837-169">Run the  cleanup script on any other passive VMM cluster nodes that have the Provider installed.</span></span>
7. <span data-ttu-id="f7837-170">Avinstallera providern på VMM-servern manuellt.</span><span class="sxs-lookup"><span data-stu-id="f7837-170">Uninstall the Provider manually on the VMM server.</span></span> <span data-ttu-id="f7837-171">Om du har ett kluster kan du ta bort från alla noder.</span><span class="sxs-lookup"><span data-stu-id="f7837-171">If you have a cluster, remove from all nodes.</span></span>
8. <span data-ttu-id="f7837-172">Om du replikerar till Azure kan du ta bort Microsoft Recovery Services-agenten Hyper-V-värdar i de borttagna moln.</span><span class="sxs-lookup"><span data-stu-id="f7837-172">If you replicating to Azure, you can remove the Microsoft Recovery Services agent from Hyper-V hosts in the deleted clouds.</span></span>

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a><span data-ttu-id="f7837-173">Avregistrera en Hyper-V-värd i en Hyper-V-plats</span><span class="sxs-lookup"><span data-stu-id="f7837-173">Unregister a Hyper-V host in a Hyper-V Site</span></span>

<span data-ttu-id="f7837-174">Hyper-V-värdar som inte hanteras av VMM samlas till en Hyper-V-plats.</span><span class="sxs-lookup"><span data-stu-id="f7837-174">Hyper-V hosts that aren't managed by VMM are gathered into a Hyper-V site.</span></span> <span data-ttu-id="f7837-175">Ta bort en värd i en Hyper-V-plats på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="f7837-175">Remove a host in a Hyper-V site as follows:</span></span>

1. <span data-ttu-id="f7837-176">Inaktivera replikering för Hyper-V virtuella datorer finns på värden.</span><span class="sxs-lookup"><span data-stu-id="f7837-176">Disable replication for Hyper-V VMs located on the host.</span></span>
2. <span data-ttu-id="f7837-177">Kopplingen mellan principer för Hyper-V-platsen.</span><span class="sxs-lookup"><span data-stu-id="f7837-177">Disassociate policies for the Hyper-V site.</span></span> <span data-ttu-id="f7837-178">I **Site Recovery-infrastruktur** > **för Hyper-V-platser** >  **replikeringsprinciper**, dubbelklicka på den associera principen.</span><span class="sxs-lookup"><span data-stu-id="f7837-178">In **Site Recovery Infrastructure** > **For Hyper-V Sites** >  **Replication Policies**, double-click the associated policy.</span></span> <span data-ttu-id="f7837-179">Högerklicka på platsen > **ta bort association med**.</span><span class="sxs-lookup"><span data-stu-id="f7837-179">Right-click the site > **Disassociate**.</span></span>
3. <span data-ttu-id="f7837-180">Ta bort Hyper-V-värdar.</span><span class="sxs-lookup"><span data-stu-id="f7837-180">Delete Hyper-V hosts.</span></span> <span data-ttu-id="f7837-181">I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-värdar**, högerklicka på servern >  **Ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-181">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Hosts**, right-click the server > **Delete**.</span></span>
4. <span data-ttu-id="f7837-182">Ta bort Hyper-V-platsen när alla värdar har tagits bort från den.</span><span class="sxs-lookup"><span data-stu-id="f7837-182">Delete the Hyper-V site after all hosts have been removed from it.</span></span> <span data-ttu-id="f7837-183">I **Site Recovery-infrastruktur** > **för System Center VMM** > **Hyper-V-platser**, högerklicka på platsen > **ta bort** .</span><span class="sxs-lookup"><span data-stu-id="f7837-183">In **Site Recovery Infrastructure** > **For System Center VMM** > **Hyper-V Sites**, right-click the site > **Delete**.</span></span>
5. <span data-ttu-id="f7837-184">Kör följande skript på varje Hyper-V-värd som tagits bort.</span><span class="sxs-lookup"><span data-stu-id="f7837-184">Run the following script on each Hyper-V host that you removed.</span></span> <span data-ttu-id="f7837-185">Skriptet rensar inställningarna på servern och Avregistrerar från valvet.</span><span class="sxs-lookup"><span data-stu-id="f7837-185">The script cleans up settings on the server, and unregisters it from the vault.</span></span>


        `` pushd .
        try
        {
             $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
             $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
             $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
             $isAdmin=$principal.IsInRole($administrators)
             if (!$isAdmin)
             {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;       
             }

            $error.Clear()    
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
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
                "Stopping the Azure Site Recovery service..."
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

            # First retrive all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
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



## <a name="disable-protection-for-a-vmware-vm-or-physical-server"></a><span data-ttu-id="f7837-186">Inaktivera skyddet för en VMware-VM eller fysiska servrar</span><span class="sxs-lookup"><span data-stu-id="f7837-186">Disable protection for a VMware VM or physical server</span></span>

1. <span data-ttu-id="f7837-187">I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-187">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="f7837-188">I **ta bort datorn**, väljer du något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="f7837-188">In **Remove Machine**, select one of these options:</span></span>
    - <span data-ttu-id="f7837-189">**Inaktivera skyddet för datorn (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="f7837-189">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="f7837-190">Använd det här alternativet för att stoppa replikering av datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-190">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="f7837-191">Site Recovery-inställningarna kommer att rensas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f7837-191">Site Recovery settings will be cleaned up automatically.</span></span> <span data-ttu-id="f7837-192">Endast visas det här alternativet under följande omständigheter:</span><span class="sxs-lookup"><span data-stu-id="f7837-192">You will only see this option in the following circumstances:</span></span>
        - <span data-ttu-id="f7837-193">**Du har ändrat storlek VM volymen**– när du ändrar storlek på en volym som den virtuella datorn försätts i ett kritiskt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="f7837-193">**You've resized the VM volume**—When you resize a volume the virtual machine goes into a critical state.</span></span> <span data-ttu-id="f7837-194">Välj det här alternativet inaktiverar skyddet medan behåller återställningspunkter i Azure.</span><span class="sxs-lookup"><span data-stu-id="f7837-194">Select this option to disables protection while retaining recovery points in Azure.</span></span> <span data-ttu-id="f7837-195">När du aktiverar skyddet igen för datorn överförs data för den nya storleken volymen till Azure.</span><span class="sxs-lookup"><span data-stu-id="f7837-195">When you enable protection for the machine again, the data for the resized volume will be transferred to Azure.</span></span>
        - <span data-ttu-id="f7837-196">**Du har nyligen kör redundans**– när du har kört en växling vid fel för att testa din miljö, Välj det här alternativet för att börja skydda lokala datorer igen.</span><span class="sxs-lookup"><span data-stu-id="f7837-196">**You've recently run a failover**—After you've run a failover to test your environment, select this option to start protecting on-premises machines again.</span></span> <span data-ttu-id="f7837-197">Varje virtuell dator inaktiveras och måste du aktivera skyddet för dem igen.</span><span class="sxs-lookup"><span data-stu-id="f7837-197">It disables each virtual machine, and then you need to enable protection for them again.</span></span> <span data-ttu-id="f7837-198">Om du inaktiverar datorn med den här inställningen påverkar inte den replikerade virtuella datorn i Azure.</span><span class="sxs-lookup"><span data-stu-id="f7837-198">Disabling the machine with this setting doesn't affect the replica virtual machine in Azure.</span></span> <span data-ttu-id="f7837-199">Inte avinstallera mobilitetstjänsten på datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-199">Don't uninstall the Mobility service from the machine.</span></span>
    - <span data-ttu-id="f7837-200">**Sluta hantera datorn**.</span><span class="sxs-lookup"><span data-stu-id="f7837-200">**Stop managing the machine**.</span></span> <span data-ttu-id="f7837-201">Om du väljer det här alternativet måste tas datorn endast bort från valvet.</span><span class="sxs-lookup"><span data-stu-id="f7837-201">If you select this option, the machine will only be removed from the vault.</span></span> <span data-ttu-id="f7837-202">Lokala skyddsinställningar för datorn påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="f7837-202">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="f7837-203">Du måste rensa inställningarna genom att avinstallera mobilitetstjänsten att ta bort inställningarna på datorn och ta bort datorn från Azure-prenumerationen.</span><span class="sxs-lookup"><span data-stu-id="f7837-203">To remove settings on the machine, and to remove the machine from the Azure subscription, you need to clean the settings by uninstalling the Mobility service.</span></span>

## <a name="disable-protection-for-a-hyper-v-vm-in-a-vmm-cloud"></a><span data-ttu-id="f7837-204">Inaktivera skyddet för en Hyper-V virtuell dator i VMM-moln</span><span class="sxs-lookup"><span data-stu-id="f7837-204">Disable protection for a Hyper-V VM in a VMM cloud</span></span>

1. <span data-ttu-id="f7837-205">I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-205">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="f7837-206">I **ta bort datorn**, väljer du något av följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="f7837-206">In **Remove Machine**, select one of these options:</span></span>

    - <span data-ttu-id="f7837-207">**Inaktivera skyddet för datorn (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="f7837-207">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="f7837-208">Använd det här alternativet för att stoppa replikering av datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-208">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="f7837-209">Site Recovery-inställningarna kommer att rensas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f7837-209">Site Recovery settings will be cleaned up automatically.</span></span>
    - <span data-ttu-id="f7837-210">**Sluta hantera datorn**.</span><span class="sxs-lookup"><span data-stu-id="f7837-210">**Stop managing the machine**.</span></span> <span data-ttu-id="f7837-211">Om du väljer det här alternativet måste tas datorn endast bort från valvet.</span><span class="sxs-lookup"><span data-stu-id="f7837-211">If you select this option, the machine will only be removed from the vault.</span></span> <span data-ttu-id="f7837-212">Lokala skyddsinställningar för datorn påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="f7837-212">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="f7837-213">Ta bort inställningar på datorn och ta bort datorn från Azure-prenumerationen, måste du rensa inställningarna manuellt med hjälp av anvisningarna nedan.</span><span class="sxs-lookup"><span data-stu-id="f7837-213">To remove settings on the machine, and to remove the machine from the Azure subscription, you need to clean the settings up manually, using the instructions below.</span></span> <span data-ttu-id="f7837-214">Observera att om du väljer att ta bort den virtuella datorn och dess hårddiskar de ska tas bort från målplatsen.</span><span class="sxs-lookup"><span data-stu-id="f7837-214">Note that if you select to delete the virtual machine and its hard disks, they'll be removed from the target location.</span></span>

### <a name="clean-up-protection-settings---replication-to-a-secondary-vmm-site"></a><span data-ttu-id="f7837-215">Rensa skyddsinställningarna - replikering till en sekundär plats för VMM</span><span class="sxs-lookup"><span data-stu-id="f7837-215">Clean up protection settings - replication to a secondary VMM site</span></span>

<span data-ttu-id="f7837-216">Om du har valt **sluta hantera datorn** och du replikerar till en sekundär plats, köra detta skript på den primära servern att rensa inställningarna för den primära virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-216">If you selected **Stop managing the machine** and you replicating to a secondary site, run this script on the primary server to clean up the settings for the primary virtual machine.</span></span> <span data-ttu-id="f7837-217">Klicka på knappen PowerShell om du vill öppna VMM PowerShell-konsolen i VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="f7837-217">In the VMM console click the PowerShell button to open the VMM PowerShell console.</span></span> <span data-ttu-id="f7837-218">Ersätt SQLVM1 med namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-218">Replace SQLVM1 with the name of your virtual machine.</span></span>

         ``$vm = get-scvirtualmachine -Name "SQLVM1"
         Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="f7837-219">Kör skriptet för att rensa inställningarna för den sekundära virtuella datorn på den sekundära VMM-servern:</span><span class="sxs-lookup"><span data-stu-id="f7837-219">On the secondary VMM server run this script to clean up the settings for the secondary virtual machine:</span></span>

        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force``
3. <span data-ttu-id="f7837-220">Uppdatera de virtuella datorerna på Hyper-V-värdservern på den sekundära VMM-servern så att den sekundära virtuella datorn hämtar identifieras igen i VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="f7837-220">On the secondary VMM server, refresh the virtual machines on the Hyper-V host server, so that the secondary VM gets detected again in the VMM console.</span></span>
4. <span data-ttu-id="f7837-221">Stegen ovan rensa bort replikeringsinställningarna på VMM-servern.</span><span class="sxs-lookup"><span data-stu-id="f7837-221">The above steps clear up the replication settings on the VMM server.</span></span> <span data-ttu-id="f7837-222">Om du vill stoppa replikering för den virtuella datorn kör du följande skript OJ de primära och sekundära virtuella datorerna.</span><span class="sxs-lookup"><span data-stu-id="f7837-222">If you want to stop replication for the virtual machine, run the following script oh the primary and secondary VMs.</span></span> <span data-ttu-id="f7837-223">Ersätt SQLVM1 med namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-223">Replace SQLVM1 with the name of your virtual machine.</span></span>

        ``Remove-VMReplication –VMName “SQLVM1”``

### <a name="clean-up-protection-settings---replication-to-azure"></a><span data-ttu-id="f7837-224">Rensa skyddsinställningarna - replikering till Azure</span><span class="sxs-lookup"><span data-stu-id="f7837-224">Clean up protection settings - replication to Azure</span></span>

1. <span data-ttu-id="f7837-225">Om du har valt **sluta hantera datorn** och du replikerar till Azure, köra skriptet på VMM-källservern med hjälp av PowerShell från VMM-konsolen.</span><span class="sxs-lookup"><span data-stu-id="f7837-225">If you selected **Stop managing the machine** and you replicate to Azure, run this script on the source VMM server, using PowerShell from the VMM console.</span></span>
        ``$vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection``
2. <span data-ttu-id="f7837-226">Replikeringsinställningarna på VMM-servern avmarkerar du stegen ovan.</span><span class="sxs-lookup"><span data-stu-id="f7837-226">The above steps clear the replication settings on the VMM server.</span></span> <span data-ttu-id="f7837-227">Kör skriptet för att stoppa replikering för den virtuella datorn körs på Hyper-V-värdservern.</span><span class="sxs-lookup"><span data-stu-id="f7837-227">To stop replication for the virtual machine running on the Hyper-V host server, run this script.</span></span> <span data-ttu-id="f7837-228">Ersätt SQLVM1 med namnet på den virtuella datorn och host01.contoso.com med namnet på Hyper-V-värdservern.</span><span class="sxs-lookup"><span data-stu-id="f7837-228">Replace SQLVM1 with the name of your virtual machine, and host01.contoso.com with the name of the Hyper-V host server.</span></span>

        ``$vmName = "SQLVM1"
        $hostName  = "host01.contoso.com"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
        $replicationService.RemoveReplicationRelationship($vm.__PATH)``


## <a name="disable-protection-for-a-hyper-v-vm-in-a-hyper-v-site"></a><span data-ttu-id="f7837-229">Inaktivera skyddet för en Hyper-V virtuell dator på en Hyper-V-plats</span><span class="sxs-lookup"><span data-stu-id="f7837-229">Disable protection for a Hyper-V VM in a Hyper-V Site</span></span>

<span data-ttu-id="f7837-230">Använd den här proceduren om du replikerar virtuella Hyper-V-datorer till Azure utan en VMM-server.</span><span class="sxs-lookup"><span data-stu-id="f7837-230">Use this procedure if you're replicating Hyper-V VMs to Azure without a VMM server.</span></span>

1. <span data-ttu-id="f7837-231">I **skyddade objekt** > **replikerade objekt**, högerklicka på datorn > **ta bort**.</span><span class="sxs-lookup"><span data-stu-id="f7837-231">In **Protected Items** > **Replicated Items**, right-click the machine > **Delete**.</span></span>
2. <span data-ttu-id="f7837-232">I **ta bort datorn**, du kan välja följande alternativ:</span><span class="sxs-lookup"><span data-stu-id="f7837-232">In **Remove Machine**, you can select the following options:</span></span>

   - <span data-ttu-id="f7837-233">**Inaktivera skyddet för datorn (rekommenderas)**.</span><span class="sxs-lookup"><span data-stu-id="f7837-233">**Disable protection for the machine (recommended)**.</span></span> <span data-ttu-id="f7837-234">Använd det här alternativet för att stoppa replikering av datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-234">Use this option to stop replicating the machine.</span></span> <span data-ttu-id="f7837-235">Site Recovery-inställningarna kommer att rensas automatiskt.</span><span class="sxs-lookup"><span data-stu-id="f7837-235">Site Recovery settings will be cleaned up automatically.</span></span>
   - <span data-ttu-id="f7837-236">**Sluta hantera datorn**.</span><span class="sxs-lookup"><span data-stu-id="f7837-236">**Stop managing the machine**.</span></span> <span data-ttu-id="f7837-237">Om du väljer det här alternativet kommer bara datorn tas bort från valvet.</span><span class="sxs-lookup"><span data-stu-id="f7837-237">If you select this option the machine will only be removed from the vault.</span></span> <span data-ttu-id="f7837-238">Lokala skyddsinställningar för datorn påverkas inte.</span><span class="sxs-lookup"><span data-stu-id="f7837-238">On-premises protection settings for the machine won’t be affected.</span></span> <span data-ttu-id="f7837-239">Ta bort inställningar på datorn och ta bort den virtuella datorn från Azure-prenumerationen, måste du rensa inställningarna manuellt.</span><span class="sxs-lookup"><span data-stu-id="f7837-239">To remove settings on the machine, and to remove the virtual machine from the Azure subscription, you need to clean the settings up manually.</span></span> <span data-ttu-id="f7837-240">Om du väljer att ta bort den virtuella datorn och dess hårddiskar kommer de att tas bort från målplatsen.</span><span class="sxs-lookup"><span data-stu-id="f7837-240">If you select to delete the virtual machine and its hard disks they will be removed from the target location.</span></span>
3. <span data-ttu-id="f7837-241">Om du har valt **sluta hantera datorn**, köra skriptet på Hyper-V-värd källservern att ta bort replikering för den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-241">If you selected **Stop managing the machine**, run this script on the source Hyper-V host server, to remove replication for the virtual machine.</span></span> <span data-ttu-id="f7837-242">Ersätt SQLVM1 med namnet på den virtuella datorn.</span><span class="sxs-lookup"><span data-stu-id="f7837-242">Replace SQLVM1 with the name of your virtual machine.</span></span>

        $vmName = "SQLVM1"
        $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
        $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
        $replicationService.RemoveReplicationRelationship($vm.__PATH)
