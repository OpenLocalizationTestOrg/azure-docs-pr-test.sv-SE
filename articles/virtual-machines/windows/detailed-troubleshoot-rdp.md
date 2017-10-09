---
title: "aaaDetailed fjärrskrivbord felsökning i Azure | Microsoft Docs"
description: "Granska detaljerad felsökning av remote desktop fel där du kan inte tooa virtuella Windows-datorer i Azure"
services: virtual-machines-windows
documentationcenter: 
author: genlin
manager: timlt
editor: 
tags: top-support-issue,azure-service-management,azure-resource-manager
keywords: "Det går inte att ansluta tooremote skrivbordet, Felsöka fjärrskrivbord fjärrskrivbord kan inte ansluta skrivbord fjärrfel, felsökning: fjärrskrivbord, remote desktop problem"
ms.assetid: 9da36f3d-30dd-44af-824b-8ce5ef07e5e0
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.devlang: na
ms.topic: support-article
ms.date: 07/25/2017
ms.author: genli
ms.openlocfilehash: fcb0d06aa66b748f3ebbbbe3431471d3cbe7c60d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="detailed-troubleshooting-steps-for-remote-desktop-connection-issues-toowindows-vms-in-azure"></a><span data-ttu-id="128dd-104">Detaljerad felsökning för anslutning till fjärrskrivbord utfärdar tooWindows virtuella datorer i Azure</span><span class="sxs-lookup"><span data-stu-id="128dd-104">Detailed troubleshooting steps for remote desktop connection issues tooWindows VMs in Azure</span></span>
<span data-ttu-id="128dd-105">Den här artikeln innehåller detaljerad felsökning toodiagnose och fixa komplexa Remote Desktop-fel för Windows-baserade virtuella Azure-datorer.</span><span class="sxs-lookup"><span data-stu-id="128dd-105">This article provides detailed troubleshooting steps toodiagnose and fix complex Remote Desktop errors for Windows-based Azure virtual machines.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="128dd-106">tooeliminate hello vanliga Remote Desktop-fel, kontrollera att tooread [hello grundläggande felsökningsartikel för fjärrskrivbord](troubleshoot-rdp-connection.md) innan du fortsätter.</span><span class="sxs-lookup"><span data-stu-id="128dd-106">tooeliminate hello more common Remote Desktop errors, make sure tooread [hello basic troubleshooting article for Remote Desktop](troubleshoot-rdp-connection.md) before proceeding.</span></span>

<span data-ttu-id="128dd-107">Du kan stöta på ett felmeddelande som inte överensstämmer med någon av hello felmeddelanden som beskrivs i Fjärrskrivbord [hello grundläggande fjärrskrivbord felsökningsguide för](troubleshoot-rdp-connection.md).</span><span class="sxs-lookup"><span data-stu-id="128dd-107">You may encounter a Remote Desktop error message that does not resemble any of hello specific error messages covered in [hello basic Remote Desktop troubleshooting guide](troubleshoot-rdp-connection.md).</span></span> <span data-ttu-id="128dd-108">Följ dessa steg toodetermine varför hello RDP (Remote Desktop) klienten är tooconnect toohello RDP-tjänsten på hello Azure VM.</span><span class="sxs-lookup"><span data-stu-id="128dd-108">Follow these steps toodetermine why hello Remote Desktop (RDP) client is unable tooconnect toohello RDP service on hello Azure VM.</span></span>

[!INCLUDE [learn-about-deployment-models](../../../includes/learn-about-deployment-models-both-include.md)]

<span data-ttu-id="128dd-109">Om du behöver mer hjälp när som helst i den här artikeln kan du kontakta hello Azure experter på [hello MSDN Azure och hello Stack Overflow-forum](https://azure.microsoft.com/support/forums/).</span><span class="sxs-lookup"><span data-stu-id="128dd-109">If you need more help at any point in this article, you can contact hello Azure experts on [hello MSDN Azure and hello Stack Overflow forums](https://azure.microsoft.com/support/forums/).</span></span> <span data-ttu-id="128dd-110">Alternativt kan du även filen en incident i Azure-supporten.</span><span class="sxs-lookup"><span data-stu-id="128dd-110">Alternatively, you can also file an Azure support incident.</span></span> <span data-ttu-id="128dd-111">Gå toohello [Azure supportwebbplats](https://azure.microsoft.com/support/options/) och på **Get Support**.</span><span class="sxs-lookup"><span data-stu-id="128dd-111">Go toohello [Azure Support site](https://azure.microsoft.com/support/options/) and click **Get Support**.</span></span> <span data-ttu-id="128dd-112">Information om hur du använder Azure-supporten finns hello [Microsoft Azure Support FAQ](https://azure.microsoft.com/support/faq/).</span><span class="sxs-lookup"><span data-stu-id="128dd-112">For information about using Azure Support, read hello [Microsoft Azure Support FAQ](https://azure.microsoft.com/support/faq/).</span></span>

## <a name="components-of-a-remote-desktop-connection"></a><span data-ttu-id="128dd-113">Komponenter i en fjärrskrivbordsanslutning</span><span class="sxs-lookup"><span data-stu-id="128dd-113">Components of a Remote Desktop connection</span></span>
<span data-ttu-id="128dd-114">hello följande komponenter ingår i en RDP-anslutning:</span><span class="sxs-lookup"><span data-stu-id="128dd-114">hello following components are involved in an RDP connection:</span></span>

![](./media/detailed-troubleshoot-rdp/tshootrdp_0.png)

<span data-ttu-id="128dd-115">Innan du fortsätter kan det hjälpa toomentally granska vad som har ändrats sedan hello senaste lyckade Fjärrskrivbord anslutning toohello VM.</span><span class="sxs-lookup"><span data-stu-id="128dd-115">Before proceeding, it might help toomentally review what has changed since hello last successful Remote Desktop connection toohello VM.</span></span> <span data-ttu-id="128dd-116">Exempel:</span><span class="sxs-lookup"><span data-stu-id="128dd-116">For example:</span></span>

* <span data-ttu-id="128dd-117">Hej hello VM eller hello tjänst i molnet som innehåller hello VM offentliga IP-adress (kallas även hello virtuella IP-adressen [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) har ändrats.</span><span class="sxs-lookup"><span data-stu-id="128dd-117">hello public IP address of hello VM or hello cloud service containing hello VM (also called hello virtual IP address [VIP](https://en.wikipedia.org/wiki/Virtual_IP_address)) has changed.</span></span> <span data-ttu-id="128dd-118">hello RDP-fel kan bero på att din DNS-klientens cacheminne har fortfarande hello *gamla IP-adressen* registrerats för hello DNS-namn.</span><span class="sxs-lookup"><span data-stu-id="128dd-118">hello RDP failure could be because your DNS client cache still has hello *old IP address* registered for hello DNS name.</span></span> <span data-ttu-id="128dd-119">Rensa din DNS-klientens cacheminne och försök ansluta igen hello VM.</span><span class="sxs-lookup"><span data-stu-id="128dd-119">Flush your DNS client cache and try connecting hello VM again.</span></span> <span data-ttu-id="128dd-120">Eller försök ansluta direkt med hello ny VIP.</span><span class="sxs-lookup"><span data-stu-id="128dd-120">Or try connecting directly with hello new VIP.</span></span>
* <span data-ttu-id="128dd-121">Du använder ett program från tredje part toomanage anslutningar till fjärrskrivbord i stället för att använda hello anslutning som genererats av hello Azure-portalen.</span><span class="sxs-lookup"><span data-stu-id="128dd-121">You are using a third-party application toomanage your Remote Desktop connections instead of using hello connection generated by hello Azure portal.</span></span> <span data-ttu-id="128dd-122">Kontrollera att programmet hello-konfigurationen innehåller hello rätt TCP-port för hello Remote Desktop-trafik.</span><span class="sxs-lookup"><span data-stu-id="128dd-122">Verify that hello application configuration includes hello correct TCP port for hello Remote Desktop traffic.</span></span> <span data-ttu-id="128dd-123">Du kan kontrollera den här porten för en klassisk virtuell dator i hello [Azure-portalen](https://portal.azure.com), genom att klicka på hello VM Inställningar > slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="128dd-123">You can check this port for a classic virtual machine in hello [Azure portal](https://portal.azure.com), by clicking hello VM's Settings > Endpoints.</span></span>

## <a name="preliminary-steps"></a><span data-ttu-id="128dd-124">Preliminära steg</span><span class="sxs-lookup"><span data-stu-id="128dd-124">Preliminary steps</span></span>
<span data-ttu-id="128dd-125">Innan du fortsätter toohello detaljerad felsökning,</span><span class="sxs-lookup"><span data-stu-id="128dd-125">Before proceeding toohello detailed troubleshooting,</span></span>

* <span data-ttu-id="128dd-126">Kontrollera hello status hello virtuell dator i hello Azure portal för eventuella uppenbara problem.</span><span class="sxs-lookup"><span data-stu-id="128dd-126">Check hello status of hello virtual machine in hello Azure portal for any obvious issues.</span></span>
* <span data-ttu-id="128dd-127">Följ hello [snabbkorrigering anvisningar för vanliga RDP-fel i grundläggande felsökning av hello guide](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).</span><span class="sxs-lookup"><span data-stu-id="128dd-127">Follow hello [quick fix steps for common RDP errors in hello basic troubleshooting guide](troubleshoot-rdp-connection.md#quick-troubleshooting-steps).</span></span>

<span data-ttu-id="128dd-128">Försök att ansluta igen toohello virtuella datorn via fjärrskrivbord efter de här stegen.</span><span class="sxs-lookup"><span data-stu-id="128dd-128">Try reconnecting toohello VM via Remote Desktop after these steps.</span></span>

## <a name="detailed-troubleshooting-steps"></a><span data-ttu-id="128dd-129">Detaljerade felsökningsanvisningar</span><span class="sxs-lookup"><span data-stu-id="128dd-129">Detailed troubleshooting steps</span></span>
<span data-ttu-id="128dd-130">hello fjärrskrivbordsklienten kanske inte kan tooreach hello Remote Desktop-tjänsten på hello Azure VM på grund av tooissues på hello följande källor:</span><span class="sxs-lookup"><span data-stu-id="128dd-130">hello Remote Desktop client may not be able tooreach hello Remote Desktop service on hello Azure VM due tooissues at hello following sources:</span></span>

* [<span data-ttu-id="128dd-131">Fjärrskrivbordsklient dator</span><span class="sxs-lookup"><span data-stu-id="128dd-131">Remote Desktop client computer</span></span>](#source-1-remote-desktop-client-computer)
* [<span data-ttu-id="128dd-132">Gränsenheten för organisationen intranät</span><span class="sxs-lookup"><span data-stu-id="128dd-132">Organization intranet edge device</span></span>](#source-2-organization-intranet-edge-device)
* [<span data-ttu-id="128dd-133">Molnet tjänstslutpunkten och åtkomstkontrollista (ACL)</span><span class="sxs-lookup"><span data-stu-id="128dd-133">Cloud service endpoint and access control list (ACL)</span></span>](#source-3-cloud-service-endpoint-and-acl)
* [<span data-ttu-id="128dd-134">Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="128dd-134">Network security groups</span></span>](#source-4-network-security-groups)
* [<span data-ttu-id="128dd-135">Windows-baserade Azure VM</span><span class="sxs-lookup"><span data-stu-id="128dd-135">Windows-based Azure VM</span></span>](#source-5-windows-based-azure-vm)

## <a name="source-1-remote-desktop-client-computer"></a><span data-ttu-id="128dd-136">: 1 fjärrskrivbordsklienten källdator</span><span class="sxs-lookup"><span data-stu-id="128dd-136">Source 1: Remote Desktop client computer</span></span>
<span data-ttu-id="128dd-137">Kontrollera att datorn kan göra Fjärrskrivbord anslutningar tooanother lokalt, Windows-baserad dator.</span><span class="sxs-lookup"><span data-stu-id="128dd-137">Verify that your computer can make Remote Desktop connections tooanother on-premises, Windows-based computer.</span></span>

![](./media/detailed-troubleshoot-rdp/tshootrdp_1.png)

<span data-ttu-id="128dd-138">Om det går inte att kontrollera hello följande inställningar på datorn:</span><span class="sxs-lookup"><span data-stu-id="128dd-138">If you cannot, check for hello following settings on your computer:</span></span>

* <span data-ttu-id="128dd-139">En inställning för lokal brandvägg som blockerar trafik för fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="128dd-139">A local firewall setting that is blocking Remote Desktop traffic.</span></span>
* <span data-ttu-id="128dd-140">Lokalt installerat klientprogramvaran för proxy som hindrar anslutningar till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="128dd-140">Locally installed client proxy software that is preventing Remote Desktop connections.</span></span>
* <span data-ttu-id="128dd-141">Lokalt installerad programvara som förhindrar anslutning till fjärrskrivbord för nätverksövervakning.</span><span class="sxs-lookup"><span data-stu-id="128dd-141">Locally installed network monitoring software that is preventing Remote Desktop connections.</span></span>
* <span data-ttu-id="128dd-142">Andra typer av säkerhetsprogram som övervaka trafik eller tillåta/neka vissa typer av trafik som förhindrar anslutning till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="128dd-142">Other types of security software that either monitor traffic or allow/disallow specific types of traffic that is preventing Remote Desktop connections.</span></span>

<span data-ttu-id="128dd-143">I dessa fall kan tillfälligt inaktivera hello programvara och försök tooconnect tooan lokala dator via fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="128dd-143">In all these cases, temporarily disable hello software and try tooconnect tooan on-premises computer via Remote Desktop.</span></span> <span data-ttu-id="128dd-144">Om det här sättet kan ta reda på hello faktiska orsak kan fungera med din administratör toocorrect hello programvara inställningar tooallow fjärrskrivbord nätverksanslutningar.</span><span class="sxs-lookup"><span data-stu-id="128dd-144">If you can find out hello actual cause this way, work with your network administrator toocorrect hello software settings tooallow Remote Desktop connections.</span></span>

## <a name="source-2-organization-intranet-edge-device"></a><span data-ttu-id="128dd-145">Källan 2: Gränsenheten organisation intranät</span><span class="sxs-lookup"><span data-stu-id="128dd-145">Source 2: Organization intranet edge device</span></span>
<span data-ttu-id="128dd-146">Kontrollera att en dator direkt ansluten toohello Internet kan göra Fjärrskrivbord anslutningar tooyour virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="128dd-146">Verify that a computer directly connected toohello Internet can make Remote Desktop connections tooyour Azure virtual machine.</span></span>

![](./media/detailed-troubleshoot-rdp/tshootrdp_2.png)

<span data-ttu-id="128dd-147">Om du inte har en dator som är direkt ansluten toohello Internet, skapa och testa med en ny Azure virtuell dator i en resurs eller molnomfång tjänst.</span><span class="sxs-lookup"><span data-stu-id="128dd-147">If you do not have a computer that is directly connected toohello Internet, create and test with a new Azure virtual machine in a resource group or cloud service.</span></span> <span data-ttu-id="128dd-148">Mer information finns i [skapa en virtuell dator som kör Windows i Azure](../virtual-machines-windows-hero-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="128dd-148">For more information, see [Create a virtual machine running Windows in Azure](../virtual-machines-windows-hero-tutorial.md).</span></span> <span data-ttu-id="128dd-149">Du kan ta bort hello virtuell dator och hello resursgrupp eller hello molntjänst när hello testa.</span><span class="sxs-lookup"><span data-stu-id="128dd-149">You can delete hello virtual machine and hello resource group or hello cloud service, after hello test.</span></span>

<span data-ttu-id="128dd-150">Om du kan skapa en fjärrskrivbordsanslutning med en dator som är direkt ansluten toohello Internet, kontrollera din organisation intranät insticksenhet för:</span><span class="sxs-lookup"><span data-stu-id="128dd-150">If you can create a Remote Desktop connection with a computer directly attached toohello Internet, check your organization intranet edge device for:</span></span>

* <span data-ttu-id="128dd-151">En intern brandvägg blockerar HTTPS-anslutningar toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="128dd-151">An internal firewall blocking HTTPS connections toohello Internet.</span></span>
* <span data-ttu-id="128dd-152">En proxyserver som förhindrar anslutning till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="128dd-152">A proxy server preventing Remote Desktop connections.</span></span>
* <span data-ttu-id="128dd-153">Intrångsidentifiering eller program som körs på enheter i nätverket kant som förhindrar anslutning till fjärrskrivbord för nätverksövervakning.</span><span class="sxs-lookup"><span data-stu-id="128dd-153">Intrusion detection or network monitoring software running on devices in your edge network that is preventing Remote Desktop connections.</span></span>

<span data-ttu-id="128dd-154">Arbeta med din administratör toocorrect hello nätverksinställningarna för din organisation intranät edge enheten tooallow HTTPS-baserade Fjärrskrivbord anslutningar toohello Internet.</span><span class="sxs-lookup"><span data-stu-id="128dd-154">Work with your network administrator toocorrect hello settings of your organization intranet edge device tooallow HTTPS-based Remote Desktop connections toohello Internet.</span></span>

## <a name="source-3-cloud-service-endpoint-and-acl"></a><span data-ttu-id="128dd-155">Källa 3: Molnet tjänstslutpunkten och ACL</span><span class="sxs-lookup"><span data-stu-id="128dd-155">Source 3: Cloud service endpoint and ACL</span></span>
<span data-ttu-id="128dd-156">För virtuella datorer som skapades med hjälp av hello klassiska distributionsmodellen, kontrollera att en annan virtuell Azure-dator som är i hello samma molntjänst eller virtuella nätverk kan du Fjärrskrivbord anslutningar tooyour Azure VM.</span><span class="sxs-lookup"><span data-stu-id="128dd-156">For VMs created using hello Classic deployment model, verify that another Azure VM that is in hello same cloud service or virtual network can make Remote Desktop connections tooyour Azure VM.</span></span>

![](./media/detailed-troubleshoot-rdp/tshootrdp_3.png)

> [!NOTE]
> <span data-ttu-id="128dd-157">För virtuella datorer som skapats i Resource Manager, hoppar du över för[källa 4: Nätverkssäkerhetsgrupper](#source-4-network-security-groups).</span><span class="sxs-lookup"><span data-stu-id="128dd-157">For virtual machines created in Resource Manager, skip too[Source 4: Network Security Groups](#source-4-network-security-groups).</span></span>

<span data-ttu-id="128dd-158">Om du inte har en annan dator i Hej samma molntjänst eller virtuella nätverk, skapar du en.</span><span class="sxs-lookup"><span data-stu-id="128dd-158">If you do not have another virtual machine in hello same cloud service or virtual network, create one.</span></span> <span data-ttu-id="128dd-159">Gör så hello i [skapa en virtuell dator som kör Windows i Azure](../virtual-machines-windows-hero-tutorial.md).</span><span class="sxs-lookup"><span data-stu-id="128dd-159">Follow hello steps in [Create a virtual machine running Windows in Azure](../virtual-machines-windows-hero-tutorial.md).</span></span> <span data-ttu-id="128dd-160">Ta bort hello testa virtuell dator när hello test har slutförts.</span><span class="sxs-lookup"><span data-stu-id="128dd-160">Delete hello test virtual machine after hello test is completed.</span></span>

<span data-ttu-id="128dd-161">Om du kan ansluta via fjärrskrivbord tooa virtuell dator i hello samma cloud service eller virtuella nätverk, söka efter dessa inställningar:</span><span class="sxs-lookup"><span data-stu-id="128dd-161">If you can connect via Remote Desktop tooa virtual machine in hello same cloud service or virtual network, check for these settings:</span></span>

* <span data-ttu-id="128dd-162">hello slutpunktskonfigurationen för Remote Desktop-trafik på hello målet VM: hello privata TCP-port för hello slutpunkten måste matcha hello TCP-port på vilken hello Virtuella datorns fjärrskrivbord tjänsten lyssnar (standard är 3389).</span><span class="sxs-lookup"><span data-stu-id="128dd-162">hello endpoint configuration for Remote Desktop traffic on hello target VM: hello private TCP port of hello endpoint must match hello TCP port on which hello VM's Remote Desktop service is listening (default is 3389).</span></span>
* <span data-ttu-id="128dd-163">hello ACL för hello fjärrskrivbord trafik slutpunkt på hello målet VM: ACL: er kan du toospecify tillåts eller nekas inkommande trafik från hello Internet baserat på dess IP-adress.</span><span class="sxs-lookup"><span data-stu-id="128dd-163">hello ACL for hello Remote Desktop traffic endpoint on hello target VM: ACLs allow you toospecify allowed or denied incoming traffic from hello Internet based on its source IP address.</span></span> <span data-ttu-id="128dd-164">Felkonfigurerad ACL: er kan förhindra att inkommande fjärrskrivbord trafik toohello slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="128dd-164">Misconfigured ACLs can prevent incoming Remote Desktop traffic toohello endpoint.</span></span> <span data-ttu-id="128dd-165">Kontrollera din ACL: er tooensure som inkommande trafik från din offentliga IP-adresser i din proxyserver eller andra gränsservern tillåts.</span><span class="sxs-lookup"><span data-stu-id="128dd-165">Check your ACLs tooensure that incoming traffic from your public IP addresses of your proxy or other edge server is allowed.</span></span> <span data-ttu-id="128dd-166">Mer information finns i [vad är ett nätverk åtkomstkontrollista (ACL)?](../../virtual-network/virtual-networks-acl.md)</span><span class="sxs-lookup"><span data-stu-id="128dd-166">For more information, see [What is a Network Access Control List (ACL)?](../../virtual-network/virtual-networks-acl.md)</span></span>

<span data-ttu-id="128dd-167">toocheck om hello-slutpunkten är hello källan hello problemet, ta bort hello aktuella slutpunkt och skapa en ny, välja en slumpmässigt vald port i intervallet för hello 49152 – 65535 för hello Externt portnummer.</span><span class="sxs-lookup"><span data-stu-id="128dd-167">toocheck if hello endpoint is hello source of hello problem, remove hello current endpoint and create a new one, choosing a random port in hello range 49152–65535 for hello external port number.</span></span> <span data-ttu-id="128dd-168">Mer information finns i [hur tooset slutpunkter tooa virtuella datorn](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span><span class="sxs-lookup"><span data-stu-id="128dd-168">For more information, see [How tooset up endpoints tooa virtual machine](classic/setup-endpoints.md?toc=%2fazure%2fvirtual-machines%2fwindows%2fclassic%2ftoc.json).</span></span>

## <a name="source-4-network-security-groups"></a><span data-ttu-id="128dd-169">Datakällan 4: Nätverkssäkerhetsgrupper</span><span class="sxs-lookup"><span data-stu-id="128dd-169">Source 4: Network Security Groups</span></span>
<span data-ttu-id="128dd-170">Nätverkssäkerhetsgrupper kan mer detaljerad kontroll över tillåtna inkommande och utgående trafik.</span><span class="sxs-lookup"><span data-stu-id="128dd-170">Network Security Groups allow more granular control of allowed inbound and outbound traffic.</span></span> <span data-ttu-id="128dd-171">Du kan skapa regler som utsträckning undernät och molntjänster i Azure-nätverk.</span><span class="sxs-lookup"><span data-stu-id="128dd-171">You can create rules spanning subnets and cloud services in an Azure virtual network.</span></span>

<span data-ttu-id="128dd-172">Använd [IP-flöde Kontrollera](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm om en regel i en Nätverkssäkerhetsgrupp blockerar trafik tooor från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="128dd-172">Use [IP flow verify](../../network-watcher/network-watcher-check-ip-flow-verify-portal.md) tooconfirm if a rule in a Network Security Group is blocking traffic tooor from a virtual machine.</span></span> <span data-ttu-id="128dd-173">Du kan också granska effektiva säkerhetsgrupp regler tooensure inkommande ”Tillåt” NSG regel finns och prioriteras för RDP-porten (standard 3389).</span><span class="sxs-lookup"><span data-stu-id="128dd-173">You can also review effective security group rules tooensure inbound "Allow" NSG rule exists and is prioritized for RDP port(default 3389).</span></span> <span data-ttu-id="128dd-174">Mer information finns i [med effektiva säkerhetsregler tootroubleshoot VM infrastrukturtrafiken rör](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).</span><span class="sxs-lookup"><span data-stu-id="128dd-174">For more information, see [Using Effective Security Rules tootroubleshoot VM traffic flow](../../virtual-network/virtual-network-nsg-troubleshoot-portal.md#using-effective-security-rules-to-troubleshoot-vm-traffic-flow).</span></span>

## <a name="source-5-windows-based-azure-vm"></a><span data-ttu-id="128dd-175">Källa 5: Windows-baserade Azure VM</span><span class="sxs-lookup"><span data-stu-id="128dd-175">Source 5: Windows-based Azure VM</span></span>
![](./media/detailed-troubleshoot-rdp/tshootrdp_5.png)

<span data-ttu-id="128dd-176">Följ anvisningarna för hello i [i den här artikeln](reset-rdp.md).</span><span class="sxs-lookup"><span data-stu-id="128dd-176">Follow hello instructions in [this article](reset-rdp.md).</span></span> <span data-ttu-id="128dd-177">Den här artikeln återställs hello Remote Desktop-tjänsten på hello virtuell dator:</span><span class="sxs-lookup"><span data-stu-id="128dd-177">This article resets hello Remote Desktop service on hello virtual machine:</span></span>

* <span data-ttu-id="128dd-178">Aktivera Standardregeln för hello ”Remote Desktop” Windows-brandväggen (TCP-port 3389).</span><span class="sxs-lookup"><span data-stu-id="128dd-178">Enable hello "Remote Desktop" Windows Firewall default rule (TCP port 3389).</span></span>
* <span data-ttu-id="128dd-179">Aktivera anslutning till fjärrskrivbord genom att ange hello HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections registret värdet too0.</span><span class="sxs-lookup"><span data-stu-id="128dd-179">Enable Remote Desktop connections by setting hello HKLM\System\CurrentControlSet\Control\Terminal Server\fDenyTSConnections registry value too0.</span></span>

<span data-ttu-id="128dd-180">Försök hello anslutningen från datorn igen.</span><span class="sxs-lookup"><span data-stu-id="128dd-180">Try hello connection from your computer again.</span></span> <span data-ttu-id="128dd-181">Om du ändå inte kan tooconnect via fjärrskrivbord kan du söka efter hello följande möjliga problem:</span><span class="sxs-lookup"><span data-stu-id="128dd-181">If you are still not able tooconnect via Remote Desktop, check for hello following possible problems:</span></span>

* <span data-ttu-id="128dd-182">hello Remote Desktop-tjänsten körs inte på hello målet VM.</span><span class="sxs-lookup"><span data-stu-id="128dd-182">hello Remote Desktop service is not running on hello target VM.</span></span>
* <span data-ttu-id="128dd-183">hello Remote Desktop-tjänsten lyssnar inte på TCP-port 3389.</span><span class="sxs-lookup"><span data-stu-id="128dd-183">hello Remote Desktop service is not listening on TCP port 3389.</span></span>
* <span data-ttu-id="128dd-184">Windows-brandväggen eller en annan lokal brandvägg har en utgående regel som hindrar Remote Desktop-trafik.</span><span class="sxs-lookup"><span data-stu-id="128dd-184">Windows Firewall or another local firewall has an outbound rule that is preventing Remote Desktop traffic.</span></span>
* <span data-ttu-id="128dd-185">Intrångsidentifiering eller program som körs på hello Azure-dator för nätverksövervakning förhindrar anslutning till fjärrskrivbord.</span><span class="sxs-lookup"><span data-stu-id="128dd-185">Intrusion detection or network monitoring software running on hello Azure virtual machine is preventing Remote Desktop connections.</span></span>

<span data-ttu-id="128dd-186">För virtuella datorer skapas med hello klassiska distributionsmodellen kan använda du en fjärransluten Azure PowerShell-session toohello virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="128dd-186">For VMs created using hello classic deployment model, you can use a remote Azure PowerShell session toohello Azure virtual machine.</span></span> <span data-ttu-id="128dd-187">Du måste först, tooinstall ett certifikat för hello virtuella datorns värd Molntjänsten.</span><span class="sxs-lookup"><span data-stu-id="128dd-187">First, you need tooinstall a certificate for hello virtual machine's hosting cloud service.</span></span> <span data-ttu-id="128dd-188">Gå för[konfigurera Secure PowerShell fjärråtkomst tooAzure virtuella datorer](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) och hämta hello **InstallWinRMCertAzureVM.ps1** skriptet filen tooyour lokal dator.</span><span class="sxs-lookup"><span data-stu-id="128dd-188">Go too[Configure Secure Remote PowerShell Access tooAzure Virtual Machines](http://gallery.technet.microsoft.com/scriptcenter/Configures-Secure-Remote-b137f2fe) and download hello **InstallWinRMCertAzureVM.ps1** script file tooyour local computer.</span></span>

<span data-ttu-id="128dd-189">Installera Azure PowerShell om du inte redan har gjort.</span><span class="sxs-lookup"><span data-stu-id="128dd-189">Next, install Azure PowerShell if you haven't already.</span></span> <span data-ttu-id="128dd-190">Se [hur tooinstall och konfigurera Azure PowerShell](/powershell/azure/overview).</span><span class="sxs-lookup"><span data-stu-id="128dd-190">See [How tooinstall and configure Azure PowerShell](/powershell/azure/overview).</span></span>

<span data-ttu-id="128dd-191">Öppna en Azure PowerShell-kommandotolk och ändra hello aktuella toohello sökvägen för hello **InstallWinRMCertAzureVM.ps1** skriptfilen.</span><span class="sxs-lookup"><span data-stu-id="128dd-191">Next, open an Azure PowerShell command prompt and change hello current folder toohello location of hello **InstallWinRMCertAzureVM.ps1** script file.</span></span> <span data-ttu-id="128dd-192">Du måste ange hello rätt körningsprincipen toorun Azure PowerShell-skript.</span><span class="sxs-lookup"><span data-stu-id="128dd-192">toorun an Azure PowerShell script, you must set hello correct execution policy.</span></span> <span data-ttu-id="128dd-193">Kör hello **Get-ExecutionPolicy** kommandot toodetermine din aktuella principnivån.</span><span class="sxs-lookup"><span data-stu-id="128dd-193">Run hello **Get-ExecutionPolicy** command toodetermine your current policy level.</span></span> <span data-ttu-id="128dd-194">Information om hur du anger rätt nivå för hello finns [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).</span><span class="sxs-lookup"><span data-stu-id="128dd-194">For information about setting hello appropriate level, see [Set-ExecutionPolicy](https://technet.microsoft.com/library/hh849812.aspx).</span></span>

<span data-ttu-id="128dd-195">Därefter Fyll i din Azure-prenumeration, hello molntjänstnamnet, och din virtuella namn (ta bort Hej < och > tecken) och kör sedan följande kommandon.</span><span class="sxs-lookup"><span data-stu-id="128dd-195">Next, fill in your Azure subscription name, hello cloud service name, and your virtual machine name (removing hello < and > characters), and then run these commands.</span></span>

```powershell
$subscr="<Name of your Azure subscription>"
$serviceName="<Name of hello cloud service that contains hello target virtual machine>"
$vmName="<Name of hello target virtual machine>"
.\InstallWinRMCertAzureVM.ps1 -SubscriptionName $subscr -ServiceName $serviceName -Name $vmName
```

<span data-ttu-id="128dd-196">Du kan hämta hello rätt prenumerationsnamn från hello *SubscriptionName* -egenskapen för hello visningen av hello **Get-AzureSubscription** kommando.</span><span class="sxs-lookup"><span data-stu-id="128dd-196">You can get hello correct subscription name from hello *SubscriptionName* property of hello display of hello **Get-AzureSubscription** command.</span></span> <span data-ttu-id="128dd-197">Du kan hämta hello molntjänstnamnet för hello virtuell dator från hello *ServiceName* kolumn i hello visningen av hello **Get-AzureVM** kommando.</span><span class="sxs-lookup"><span data-stu-id="128dd-197">You can get hello cloud service name for hello virtual machine from hello *ServiceName* column in hello display of hello **Get-AzureVM** command.</span></span>

<span data-ttu-id="128dd-198">Kontrollera att du har hello nytt certifikat.</span><span class="sxs-lookup"><span data-stu-id="128dd-198">Check if you have hello new certificate.</span></span> <span data-ttu-id="128dd-199">Öppna en snapin-modulen för certifikat för den aktuella användaren i hello och titta i hello **Trusted Root Certification Authorities\Certificates** mapp.</span><span class="sxs-lookup"><span data-stu-id="128dd-199">Open a Certificates snap-in for hello current user and look in hello **Trusted Root Certification Authorities\Certificates** folder.</span></span> <span data-ttu-id="128dd-200">Du bör se ett certifikat med hello DNS-namnet på Molntjänsten i hello utfärdat toocolumn (exempel: cloudservice4testing.cloudapp.net).</span><span class="sxs-lookup"><span data-stu-id="128dd-200">You should see a certificate with hello DNS name of your cloud service in hello Issued toocolumn (example: cloudservice4testing.cloudapp.net).</span></span>

<span data-ttu-id="128dd-201">Sedan initiera Azure PowerShell-fjärrsession med hjälp av dessa kommandon.</span><span class="sxs-lookup"><span data-stu-id="128dd-201">Next, initiate a remote Azure PowerShell session by using these commands.</span></span>

```powershell
$uri = Get-AzureWinRMUri -ServiceName $serviceName -Name $vmName
$creds = Get-Credential
Enter-PSSession -ConnectionUri $uri -Credential $creds
```

<span data-ttu-id="128dd-202">När du har angett giltiga autentiseringsuppgifter, bör du se något liknande toohello följande Azure PowerShell-prompten:</span><span class="sxs-lookup"><span data-stu-id="128dd-202">After entering valid administrator credentials, you should see something similar toohello following Azure PowerShell prompt:</span></span>

```powershell
[cloudservice4testing.cloudapp.net]: PS C:\Users\User1\Documents>
```

<span data-ttu-id="128dd-203">hello första delen av det här meddelandet är din molntjänstnamnet som innehåller hello mål VM, vilket kan skilja sig från ”cloudservice4testing.cloudapp.net”.</span><span class="sxs-lookup"><span data-stu-id="128dd-203">hello first part of this prompt is your cloud service name that contains hello target VM, which could be different from "cloudservice4testing.cloudapp.net".</span></span> <span data-ttu-id="128dd-204">Du kan nu utfärda Azure PowerShell-kommandon för det här molnet tooinvestigate hello serviceproblem nämns och korrigera hello konfiguration.</span><span class="sxs-lookup"><span data-stu-id="128dd-204">You can now issue Azure PowerShell commands for this cloud service tooinvestigate hello problems mentioned and correct hello configuration.</span></span>

### <a name="toomanually-correct-hello-remote-desktop-services-listening-tcp-port"></a><span data-ttu-id="128dd-205">toomanually rätt hello Fjärrskrivbordstjänster lyssnande TCP-port</span><span class="sxs-lookup"><span data-stu-id="128dd-205">toomanually correct hello Remote Desktop Services listening TCP port</span></span>
<span data-ttu-id="128dd-206">Kör kommandot i Kommandotolken hello remote Azure PowerShell-session.</span><span class="sxs-lookup"><span data-stu-id="128dd-206">At hello remote Azure PowerShell session prompt, run this command.</span></span>

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

<span data-ttu-id="128dd-207">hello PortNumber egenskapen visar hello aktuella portnumret.</span><span class="sxs-lookup"><span data-stu-id="128dd-207">hello PortNumber property shows hello current port number.</span></span> <span data-ttu-id="128dd-208">Om det behövs ändrar du hello fjärrskrivbord port number tillbaka tooits standardvärdet (port 3389) med hjälp av det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="128dd-208">If needed, change hello Remote Desktop port number back tooits default value (3389) by using this command.</span></span>

```powershell
Set-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber" -Value 3389
```

<span data-ttu-id="128dd-209">Kontrollera att hello-porten har ändrats too3389 med hjälp av det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="128dd-209">Verify that hello port has been changed too3389 by using this command.</span></span>

```powershell
Get-ItemProperty -Path "HKLM:\System\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" -Name "PortNumber"
```

<span data-ttu-id="128dd-210">Avsluta hello Azure PowerShell-fjärrsession med hjälp av det här kommandot.</span><span class="sxs-lookup"><span data-stu-id="128dd-210">Exit hello remote Azure PowerShell session by using this command.</span></span>

```powershell
Exit-PSSession
```

<span data-ttu-id="128dd-211">Kontrollera att hello fjärrskrivbord slutpunkt för hello Azure VM också använder TCP-port 3398 som dess Intern port.</span><span class="sxs-lookup"><span data-stu-id="128dd-211">Verify that hello Remote Desktop endpoint for hello Azure VM is also using TCP port 3398 as its internal port.</span></span> <span data-ttu-id="128dd-212">Starta om hello Azure VM och försök hello fjärrskrivbordsanslutning igen.</span><span class="sxs-lookup"><span data-stu-id="128dd-212">Restart hello Azure VM and try hello Remote Desktop connection again.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="128dd-213">Ytterligare resurser</span><span class="sxs-lookup"><span data-stu-id="128dd-213">Additional resources</span></span>
[<span data-ttu-id="128dd-214">Hur tooreset ett lösenord eller hello fjärrskrivbord service för Windows-datorer</span><span class="sxs-lookup"><span data-stu-id="128dd-214">How tooreset a password or hello Remote Desktop service for Windows virtual machines</span></span>](reset-rdp.md)

[<span data-ttu-id="128dd-215">Hur tooinstall och konfigurera Azure PowerShell</span><span class="sxs-lookup"><span data-stu-id="128dd-215">How tooinstall and configure Azure PowerShell</span></span>](/powershell/azure/overview)

[<span data-ttu-id="128dd-216">Felsökning av SSH (Secure Shell) anslutningar tooa Linux-baserade virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="128dd-216">Troubleshoot Secure Shell (SSH) connections tooa Linux-based Azure virtual machine</span></span>](../linux/troubleshoot-ssh-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

[<span data-ttu-id="128dd-217">Felsöka åtkomst tooan program som körs på en virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="128dd-217">Troubleshoot access tooan application running on an Azure virtual machine</span></span>](../linux/troubleshoot-app-connection.md?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json)

