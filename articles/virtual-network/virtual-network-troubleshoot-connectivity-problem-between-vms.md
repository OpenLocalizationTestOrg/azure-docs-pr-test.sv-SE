---
title: aaaTroubleshooting anslutningsproblem mellan virtuella datorer i Azure | Microsoft Docs
description: "Lär dig hur tooTroubleshoot hello problem med nätverksanslutningen mellan virtuella Azure-datorer."
services: virtual-network
documentationcenter: na
author: chadmath
manager: cshepard
editor: 
tags: azure-resource-manager
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 08/25/2017
ms.author: genli
ms.openlocfilehash: ee0819178dcbee2bf529a495758e6c33f49e085e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="troubleshooting-connectivity-problems-between-azure-vms"></a><span data-ttu-id="8f39e-103">Felsökning av problem med nätverksanslutningen mellan virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="8f39e-103">Troubleshooting connectivity problems between Azure VMs</span></span>

<span data-ttu-id="8f39e-104">Kan det uppstå problem med nätverksanslutningen mellan virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="8f39e-104">You might experience connectivity problems between Azure VMs.</span></span> <span data-ttu-id="8f39e-105">Den här artikeln innehåller felsökning steg toohelp du lösa problemet.</span><span class="sxs-lookup"><span data-stu-id="8f39e-105">This article provides troubleshooting steps toohelp you resolve this problem.</span></span> 

[!INCLUDE [support-disclaimer](../../includes/support-disclaimer.md)]

## <a name="symptom"></a><span data-ttu-id="8f39e-106">Symtom</span><span class="sxs-lookup"><span data-stu-id="8f39e-106">Symptom</span></span>

<span data-ttu-id="8f39e-107">En virtuell dator i Azure kan inte ansluta tooanother Azure VM.</span><span class="sxs-lookup"><span data-stu-id="8f39e-107">An Azure VM cannot connect tooanother Azure VM.</span></span>

## <a name="troubleshooting-guidance"></a><span data-ttu-id="8f39e-108">Felsökningsanvisningar</span><span class="sxs-lookup"><span data-stu-id="8f39e-108">Troubleshooting guidance</span></span> 

1. [<span data-ttu-id="8f39e-109">Kontrollera om nätverkskortet är felkonfigurerat</span><span class="sxs-lookup"><span data-stu-id="8f39e-109">Check if NIC is misconfigured</span></span>](#step-1-check-if-nic-is-misconfigured)
2. [<span data-ttu-id="8f39e-110">Kontrollera om nätverkstrafik blockeras av NSG eller UDR</span><span class="sxs-lookup"><span data-stu-id="8f39e-110">Check if network traffic is blocked by NSG or UDR</span></span>](#step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr)
3. [<span data-ttu-id="8f39e-111">Kontrollera om nätverkstrafik blockeras av VM-brandvägg</span><span class="sxs-lookup"><span data-stu-id="8f39e-111">Check if network traffic is blocked by VM firewall</span></span>](#step-3-check-if-network-traffic-is-blocked-by-vm-firewall)
4. [<span data-ttu-id="8f39e-112">Kontrollera om VM app eller tjänst lyssnar på port hello</span><span class="sxs-lookup"><span data-stu-id="8f39e-112">Check whether VM app or service is listening on hello port</span></span>](#step-4-check-whether-vm-app-or-service-is-listening-on-the-port)
5. [<span data-ttu-id="8f39e-113">Kontrollera om hello problemet orsakas av SNAT</span><span class="sxs-lookup"><span data-stu-id="8f39e-113">Check whether hello problem is caused by SNAT</span></span>](#step-5-check-whether-the-problem-is-caused-by-snat)
6. [<span data-ttu-id="8f39e-114">Kontrollera om trafik blockeras av ACL: er för hello klassiska VM</span><span class="sxs-lookup"><span data-stu-id="8f39e-114">Check whether traffic is blocked by ACLs for hello classic VM</span></span>](#step-6-check-whether-traffic-is-blocked-by-acls-for-the-classic-vm)
7. [<span data-ttu-id="8f39e-115">Kontrollera om hello slutpunkten har skapats för hello klassiska VM</span><span class="sxs-lookup"><span data-stu-id="8f39e-115">Check whether hello endpoint is created for hello classic VM</span></span>](#step-7-check-whether-the-endpoint-is-created-for-the-classic-vm)
8. [<span data-ttu-id="8f39e-116">Det går inte tooconnect tooa VM-nätverksresurs</span><span class="sxs-lookup"><span data-stu-id="8f39e-116">Unable tooconnect tooa VM network share</span></span>](#step-8-unable-to-connect-to-a-vm-network-share)
9. [<span data-ttu-id="8f39e-117">Bland Vnet-anslutningar</span><span class="sxs-lookup"><span data-stu-id="8f39e-117">Inter-Vnet connectivity</span></span>](#step-9-inter-vnet-connectivity)

## <a name="troubleshooting-steps"></a><span data-ttu-id="8f39e-118">Felsökningssteg</span><span class="sxs-lookup"><span data-stu-id="8f39e-118">Troubleshooting steps</span></span>

<span data-ttu-id="8f39e-119">Följ dessa steg tootroubleshoot hello problem.</span><span class="sxs-lookup"><span data-stu-id="8f39e-119">Follow these steps tootroubleshoot hello problem.</span></span> <span data-ttu-id="8f39e-120">Kontrollera om hello problemet är löst efter varje steg.</span><span class="sxs-lookup"><span data-stu-id="8f39e-120">Check whether hello problem is resolved after each step.</span></span> 

### <a name="step-1-check-if-nic-is-misconfigured"></a><span data-ttu-id="8f39e-121">Steg 1: Kontrollera om nätverkskortet är felkonfigurerat</span><span class="sxs-lookup"><span data-stu-id="8f39e-121">Step 1: Check if NIC is misconfigured</span></span>

<span data-ttu-id="8f39e-122">Följ [hur tooreset nätverksgränssnittet för Azure Windows VM](../virtual-machines/windows/reset-network-interface.md).</span><span class="sxs-lookup"><span data-stu-id="8f39e-122">Follow [How tooreset network interface for Azure Windows VM](../virtual-machines/windows/reset-network-interface.md).</span></span> 

<span data-ttu-id="8f39e-123">Om hello problem uppstår när du har ändrat hello nätverksgränssnitt (NIC), gör du följande:</span><span class="sxs-lookup"><span data-stu-id="8f39e-123">If hello problem occurs after you modify hello network interface (NIC), follow these steps:</span></span>

<span data-ttu-id="8f39e-124">**Mulit NIC virtuella datorer**</span><span class="sxs-lookup"><span data-stu-id="8f39e-124">**Mulit-NIC VMs**</span></span>

1. <span data-ttu-id="8f39e-125">Lägg till ett nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="8f39e-125">Add a NIC.</span></span>
2. <span data-ttu-id="8f39e-126">Lösa problem hello i hello felaktiga NIC eller ta bort felaktiga nätverkskort.</span><span class="sxs-lookup"><span data-stu-id="8f39e-126">Fix hello problems in hello bad NIC or remove bad NIC.</span></span>  <span data-ttu-id="8f39e-127">Readd hello NIC.</span><span class="sxs-lookup"><span data-stu-id="8f39e-127">Then readd hello NIC.</span></span>

<span data-ttu-id="8f39e-128">Mer information finns i [Lägg till nätverket gränssnitt tooor ta bort från virtuella datorer](virtual-network-network-interface-vm.md).</span><span class="sxs-lookup"><span data-stu-id="8f39e-128">For more information, see [Add network interfaces tooor remove from virtual machines](virtual-network-network-interface-vm.md).</span></span>

<span data-ttu-id="8f39e-129">**Enskild NIC VM**</span><span class="sxs-lookup"><span data-stu-id="8f39e-129">**Single-NIC VM**</span></span> 

- [<span data-ttu-id="8f39e-130">Distribuera Windows VM</span><span class="sxs-lookup"><span data-stu-id="8f39e-130">Redeploy Windows VM</span></span>](../virtual-machines/windows/redeploy-to-new-node.md)
- [<span data-ttu-id="8f39e-131">Distribuera virtuell Linux-dator</span><span class="sxs-lookup"><span data-stu-id="8f39e-131">Redeploy Linux VM</span></span>](../virtual-machines/linux/redeploy-to-new-node.md)

### <a name="step-2-check-if-network-traffic-is-blocked-by-nsg-or-udr"></a><span data-ttu-id="8f39e-132">Steg 2: Kontrollera om nätverkstrafik blockeras av NSG eller UDR</span><span class="sxs-lookup"><span data-stu-id="8f39e-132">Step 2: Check if network traffic is blocked by NSG or UDR</span></span>

<span data-ttu-id="8f39e-133">Använda [nätverk Watcher IP-flöde Kontrollera](../network-watcher/network-watcher-ip-flow-verify-overview.md) och [NSG flödet loggning](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify om det är en Nätverkssäkerhetsgrupp eller User-defined väg kan störa trafikflödet.</span><span class="sxs-lookup"><span data-stu-id="8f39e-133">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span>

### <a name="step-3-check-if-network-traffic-is-blocked-by-vm-firewall"></a><span data-ttu-id="8f39e-134">Steg 3: Kontrollera om nätverkstrafik blockeras av VM-brandvägg</span><span class="sxs-lookup"><span data-stu-id="8f39e-134">Step 3: Check if network traffic is blocked by VM firewall</span></span>

<span data-ttu-id="8f39e-135">Inaktivera hello-brandväggen och sedan testa hello resultatet.</span><span class="sxs-lookup"><span data-stu-id="8f39e-135">Disable hello firewall, and then test hello result.</span></span> <span data-ttu-id="8f39e-136">Om hello problemet är löst verifiera hello inställningarna i hello brandvägg och aktivera igen.</span><span class="sxs-lookup"><span data-stu-id="8f39e-136">If hello problem is resolved, validate hello settings in hello firewall and re-enable.</span></span>

### <a name="step-4-check-whether-vm-app-or-service-is-listening-on-hello-port"></a><span data-ttu-id="8f39e-137">Steg 4: Kontrollera om VM app eller tjänst lyssnar på port hello</span><span class="sxs-lookup"><span data-stu-id="8f39e-137">Step 4: Check whether VM app or service is listening on hello port</span></span>

<span data-ttu-id="8f39e-138">Du kan använda någon av följande metoder toocheck om VM-programmet eller tjänsten lyssnar på port hello hello</span><span class="sxs-lookup"><span data-stu-id="8f39e-138">You can use one of hello following methods toocheck whether VM Application or Service is listening on hello port</span></span>

- <span data-ttu-id="8f39e-139">Kör följande kommandon toocheck om hello servern lyssnar på porten hello.</span><span class="sxs-lookup"><span data-stu-id="8f39e-139">Run hello following commands toocheck whether hello server is listening on that port.</span></span>

<span data-ttu-id="8f39e-140">**Virtuell Windows-dator**</span><span class="sxs-lookup"><span data-stu-id="8f39e-140">**Windows VM**</span></span>

    netstat –ano

<span data-ttu-id="8f39e-141">**Virtuell Linux-dator**</span><span class="sxs-lookup"><span data-stu-id="8f39e-141">**Linux VM**</span></span>

    netstat -l

- <span data-ttu-id="8f39e-142">Kör hello **Telnet** på hello VM tooitself tootest hello port.</span><span class="sxs-lookup"><span data-stu-id="8f39e-142">Run hello **Telnet** command on hello VM tooitself tootest hello port.</span></span> <span data-ttu-id="8f39e-143">Om hello testet misslyckas, är programmet eller tjänsten inte konfigurerade toolisten på hello port.</span><span class="sxs-lookup"><span data-stu-id="8f39e-143">If hello test fails, application or service is not configured toolisten on hello port.</span></span>

### <a name="step-5-check-whether-hello-problem-is-caused-by-snat"></a><span data-ttu-id="8f39e-144">Steg 5: Kontrollera om hello problemet orsakas av SNAT</span><span class="sxs-lookup"><span data-stu-id="8f39e-144">Step 5: Check whether hello problem is caused by SNAT</span></span>

<span data-ttu-id="8f39e-145">I vissa scenarier, är hello VM placerad bakom en lösning för utjämna belastningen som har ett beroende resurser utanför Azure.</span><span class="sxs-lookup"><span data-stu-id="8f39e-145">In some scenarios, hello VM is placed behind a Load balance solution that has a dependency on resources outside of Azure.</span></span> <span data-ttu-id="8f39e-146">I dessa scenarier, om det uppstår återkommande anslutningsproblem, hello problemet kan orsakas av [SNAT port resursuttömning](../load-balancer/load-balancer-outbound-connections.md).</span><span class="sxs-lookup"><span data-stu-id="8f39e-146">In these scenarios, if you experience intermittent connection problems, hello problem may be caused by [SNAT port exhaustion](../load-balancer/load-balancer-outbound-connections.md).</span></span> <span data-ttu-id="8f39e-147">tooresolve hello problemet, skapa en VIP (eller går för klassisk) för varje virtuell dator som är bakom hello belastningsutjämnare och skydda med NSG eller ACL.</span><span class="sxs-lookup"><span data-stu-id="8f39e-147">tooresolve hello issue, create a VIP (or ILPIP for classic) for each VM that is behind hello Load balancer and secure with NSG or ACL.</span></span> 

### <a name="step-6-check-whether-traffic-is-blocked-by-acls-for-hello-classic-vm"></a><span data-ttu-id="8f39e-148">Steg 6: Kontrollera om trafik blockeras av ACL: er för hello klassiska VM</span><span class="sxs-lookup"><span data-stu-id="8f39e-148">Step 6: Check whether traffic is blocked by ACLs for hello classic VM</span></span>

<span data-ttu-id="8f39e-149">En ACL ger hello möjlighet tooselectively bevilja eller neka trafik för en virtuell dator-slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="8f39e-149">An ACL provides hello ability tooselectively permit or deny traffic for a virtual machine endpoint.</span></span> <span data-ttu-id="8f39e-150">Mer information finns i [hantera hello ACL på en slutpunkt](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span><span class="sxs-lookup"><span data-stu-id="8f39e-150">For more information, see [Manage hello ACL on an endpoint](../virtual-machines/windows/classic/setup-endpoints.md#manage-the-acl-on-an-endpoint).</span></span>

### <a name="step-7-check-whether-hello-endpoint-is-created-for-hello-classic-vm"></a><span data-ttu-id="8f39e-151">Steg 7: Kontrollera om hello slutpunkten har skapats för hello klassiska VM</span><span class="sxs-lookup"><span data-stu-id="8f39e-151">Step 7: Check whether hello endpoint is created for hello classic VM</span></span>

<span data-ttu-id="8f39e-152">Alla virtuella datorer som du skapar i Azure med hjälp av hello klassiska distributionsmodellen kan automatiskt kommunicera via en kanal för privat nätverk med andra virtuella datorer i hello samma cloud service eller virtuellt nätverk.</span><span class="sxs-lookup"><span data-stu-id="8f39e-152">All VMs that you create in Azure using hello classic deployment model can automatically communicate over a private network channel with other virtual machines in hello same cloud service or virtual network.</span></span> <span data-ttu-id="8f39e-153">Datorer med andra virtuella nätverk måste dock slutpunkter toodirect hello nätverk för inkommande trafik tooa virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="8f39e-153">However, computers on other virtual networks require endpoints toodirect hello inbound network traffic tooa virtual machine.</span></span> <span data-ttu-id="8f39e-154">Mer information finns i [hur tooset av slutpunkter](../virtual-machines/windows/classic/setup-endpoints.md).</span><span class="sxs-lookup"><span data-stu-id="8f39e-154">For more information, see [How tooset up endpoints](../virtual-machines/windows/classic/setup-endpoints.md).</span></span>

### <a name="step-8-unable-tooconnect-tooa-vm-network-share"></a><span data-ttu-id="8f39e-155">Steg 8: Det gick inte tooconnect tooa VM nätverksresurs</span><span class="sxs-lookup"><span data-stu-id="8f39e-155">Step 8: Unable tooconnect tooa VM network share</span></span>

<span data-ttu-id="8f39e-156">Om du är tooconnect tooa VM nätverksresurs orsakas hello problemet av tillgänglig nätverkskort i hello VM.</span><span class="sxs-lookup"><span data-stu-id="8f39e-156">If you are unable tooconnect tooa VM network share, hello problem can be caused by unavailable NICs in hello VM.</span></span> <span data-ttu-id="8f39e-157">toodelete Hej tillgänglig nätverkskort, se [hur toodelete hello tillgänglig nätverkskort](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span><span class="sxs-lookup"><span data-stu-id="8f39e-157">toodelete hello unavailable NICs, see [How toodelete hello unavailable NICs](../virtual-machines/windows/reset-network-interface.md#delete-the-unavailable-nics)</span></span>

### <a name="step-9-inter-vnet-connectivity"></a><span data-ttu-id="8f39e-158">Steg 9: Bland Vnet-anslutningen</span><span class="sxs-lookup"><span data-stu-id="8f39e-158">Step 9: Inter-Vnet connectivity</span></span>

<span data-ttu-id="8f39e-159">Använda [nätverk Watcher IP-flöde Kontrollera](../network-watcher/network-watcher-ip-flow-verify-overview.md) och [NSG flödet loggning](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify om det är en Nätverkssäkerhetsgrupp eller User-defined väg kan störa trafikflödet.</span><span class="sxs-lookup"><span data-stu-id="8f39e-159">Utilize [Network Watcher IP Flow Verify](../network-watcher/network-watcher-ip-flow-verify-overview.md) and [NSG Flow Logging](../network-watcher/network-watcher-nsg-flow-logging-overview.md) tooidentify if there is a Network Security Group or User-Defined Route that is interfering with traffic flow.</span></span> <span data-ttu-id="8f39e-160">Du kan också verifiera konfigurationen av Inter-Vnet [här](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span><span class="sxs-lookup"><span data-stu-id="8f39e-160">You can also validate your Inter-Vnet configuration [here](https://support.microsoft.com/en-us/help/4032151/configuring-and-validating-vnet-or-vpn-connections).</span></span>

### <a name="need-help-contact-support"></a><span data-ttu-id="8f39e-161">Behöver du hjälp?</span><span class="sxs-lookup"><span data-stu-id="8f39e-161">Need help?</span></span> <span data-ttu-id="8f39e-162">Kontakta supporten.</span><span class="sxs-lookup"><span data-stu-id="8f39e-162">Contact support.</span></span>
<span data-ttu-id="8f39e-163">Om du fortfarande behöver hjälp [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget problemet lösas snabbt.</span><span class="sxs-lookup"><span data-stu-id="8f39e-163">If you still need help, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) tooget your issue resolved quickly.</span></span>
