---
title: "Verifiera VPN-genomströmning tooa Microsoft Azure Virtual Network | Microsoft Docs"
description: "hello syftet med det här dokumentet är toohelp en användare verifiera hello dataflödet i nätverket från sina lokala resurser tooan virtuella Azure-datorn."
services: vpn-gateway
documentationcenter: na
author: chadmath
manager: jasmc
editor: 
tags: azure-resource-manager,azure-service-management
ms.assetid: 
ms.service: vpn-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 04/10/2017
ms.author: radwiv;chadmat;genli
ms.openlocfilehash: 9cf0b67145645a3c2a9556b0cc910066cc62a9ab
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="how-toovalidate-vpn-throughput-tooa-virtual-network"></a><span data-ttu-id="afd3f-103">Hur toovalidate VPN-genomströmning tooa virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="afd3f-103">How toovalidate VPN throughput tooa virtual network</span></span>

<span data-ttu-id="afd3f-104">En VPN-gateway-anslutningen kan du tooestablish säker anslutning mellan ditt virtuella nätverk i Azure och ditt lokala IT-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="afd3f-104">A VPN gateway connection enables you tooestablish secure, cross-premises connectivity between your Virtual Network within Azure and your on-premises IT infrastructure.</span></span>

<span data-ttu-id="afd3f-105">Den här artikeln visar hur toovalidate dataflödet i nätverket från hello lokala resurser tooan virtuella Azure-datorn.</span><span class="sxs-lookup"><span data-stu-id="afd3f-105">This article shows how toovalidate network throughput from hello on-premises resources tooan Azure virtual machine.</span></span> <span data-ttu-id="afd3f-106">Det ger också felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="afd3f-106">It also provides troubleshooting guidance.</span></span>

>[!NOTE]
><span data-ttu-id="afd3f-107">Den här artikeln är avsedd toohelp diagnostisera och lösa vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="afd3f-107">This article is intended toohelp diagnose and fix common issues.</span></span> <span data-ttu-id="afd3f-108">Om du toosolve hello problemet med hjälp av följande information hello [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="afd3f-108">If you're unable toosolve hello issue by using hello following information, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="afd3f-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="afd3f-109">Overview</span></span>

<span data-ttu-id="afd3f-110">hello VPN-gatewayanslutning omfattar hello följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="afd3f-110">hello VPN gateway connection involves hello following components:</span></span>

- <span data-ttu-id="afd3f-111">Lokala VPN-enhet (visa en lista över [verifiera VPN-enheter)](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="afd3f-111">On-premises VPN device (view a list of [validated VPN devices)](vpn-gateway-about-vpn-devices.md#devicetable).</span></span>
- <span data-ttu-id="afd3f-112">Offentliga Internet</span><span class="sxs-lookup"><span data-stu-id="afd3f-112">Public Internet</span></span>
- <span data-ttu-id="afd3f-113">Azure VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="afd3f-113">Azure VPN gateway</span></span>
- <span data-ttu-id="afd3f-114">Virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="afd3f-114">Azure virtual machine</span></span>

<span data-ttu-id="afd3f-115">hello följande diagram visar hello logiska anslutningen för ett lokalt nätverk tooan virtuella Azure-nätverket via VPN.</span><span class="sxs-lookup"><span data-stu-id="afd3f-115">hello following diagram shows hello logical connectivity of an on-premises network tooan Azure virtual network through VPN.</span></span>

![Logisk anslutning i kundens nätverk tooMSFT nätverket med VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-hello-maximum-expected-ingressegress"></a><span data-ttu-id="afd3f-117">Beräkna hello högsta förväntade ingång-/ utgång</span><span class="sxs-lookup"><span data-stu-id="afd3f-117">Calculate hello maximum expected ingress/egress</span></span>

1.  <span data-ttu-id="afd3f-118">Fastställa kraven för ditt program baslinjen genomflöde.</span><span class="sxs-lookup"><span data-stu-id="afd3f-118">Determine your application's baseline throughput requirements.</span></span>
2.  <span data-ttu-id="afd3f-119">Bestämma din Azure VPN gateway genomflöde.</span><span class="sxs-lookup"><span data-stu-id="afd3f-119">Determine your Azure VPN gateway throughput limits.</span></span> <span data-ttu-id="afd3f-120">Mer hjälp finns hello ”sammanställda genomflöde av SKU- och VPN-typ” i [planering och design för VPN-Gateway](vpn-gateway-plan-design.md).</span><span class="sxs-lookup"><span data-stu-id="afd3f-120">For help, see hello "Aggregate throughput by SKU and VPN type" section of [Planning and design for VPN Gateway](vpn-gateway-plan-design.md).</span></span>
3.  <span data-ttu-id="afd3f-121">Fastställa hello [Azure VM genomströmning vägledning](../virtual-machines/virtual-machines-windows-sizes.md) för VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="afd3f-121">Determine hello [Azure VM throughput guidance](../virtual-machines/virtual-machines-windows-sizes.md) for your VM size.</span></span>
4.  <span data-ttu-id="afd3f-122">Kontrollera din Internet-leverantör (ISP) bandbredd.</span><span class="sxs-lookup"><span data-stu-id="afd3f-122">Determine your Internet Service Provider (ISP) bandwidth.</span></span>
5.  <span data-ttu-id="afd3f-123">Beräkna din förväntade genomströmning - minsta bandbredd (VM-Gateway ISP) * 0,8.</span><span class="sxs-lookup"><span data-stu-id="afd3f-123">Calculate your expected throughput - Least bandwidth of (VM, Gateway, ISP) * 0.8.</span></span>

<span data-ttu-id="afd3f-124">Om din beräknade genomströmning inte uppfyller kraven för ditt program baslinjen dataflöde, måste tooincrease hello bandbredden för hello-resurs som du har identifierat som hello flaskhals.</span><span class="sxs-lookup"><span data-stu-id="afd3f-124">If your calculated throughput does not meet your application's baseline throughput requirements, you need tooincrease hello bandwidth of hello resource that you identified as hello bottleneck.</span></span> <span data-ttu-id="afd3f-125">tooresize Azure VPN-Gateway, se [ändra en gateway-SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="afd3f-125">tooresize an Azure VPN Gateway, see [Changing a gateway SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="afd3f-126">tooresize en virtuell dator finns [ändra storlek på en virtuell dator](../virtual-machines/virtual-machines-windows-resize-vm.md).</span><span class="sxs-lookup"><span data-stu-id="afd3f-126">tooresize a virtual machine, see [Resize a VM](../virtual-machines/virtual-machines-windows-resize-vm.md).</span></span> <span data-ttu-id="afd3f-127">Om du inte har den förväntade internetbandbredd, kan du också toocontact din Internetleverantör.</span><span class="sxs-lookup"><span data-stu-id="afd3f-127">If you are not experiencing expected Internet bandwidth, you may also want toocontact your ISP.</span></span>

## <a name="validate-network-throughput-by-using-performance-tools"></a><span data-ttu-id="afd3f-128">Verifiera dataflödet i nätverket med hjälp av prestandaverktygen</span><span class="sxs-lookup"><span data-stu-id="afd3f-128">Validate network throughput by using performance tools</span></span>

<span data-ttu-id="afd3f-129">Den här verifieringen ska utföras vid låg belastning, som VPN-tunnel genomströmning mättnad under testningen inte ger korrekta resultat.</span><span class="sxs-lookup"><span data-stu-id="afd3f-129">This validation should be performed during non-peak hours, as VPN tunnel throughput saturation during testing does not give accurate results.</span></span>

<span data-ttu-id="afd3f-130">hello-verktyget som vi använder för det här testet är iPerf som fungerar i både Windows och Linux och har både klienten och servern lägen.</span><span class="sxs-lookup"><span data-stu-id="afd3f-130">hello tool we use for this test is iPerf, which works on both Windows and Linux and has both client and server modes.</span></span> <span data-ttu-id="afd3f-131">Det är begränsad too3 Gbit/s för virtuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="afd3f-131">It is limited too3 Gbps for Windows VMs.</span></span>

<span data-ttu-id="afd3f-132">Det här verktyget inte att utföra några åtgärder toodisk för läsning och skrivning.</span><span class="sxs-lookup"><span data-stu-id="afd3f-132">This tool does not perform any read/write operations toodisk.</span></span> <span data-ttu-id="afd3f-133">Den genererar endast självgenererat TCP-trafik från en end toohello andra.</span><span class="sxs-lookup"><span data-stu-id="afd3f-133">It solely produces self-generated TCP traffic from one end toohello other.</span></span> <span data-ttu-id="afd3f-134">Den genererade statistik baserat på experiment som mäter hello bandbredd som är tillgänglig mellan klienten och servern noder.</span><span class="sxs-lookup"><span data-stu-id="afd3f-134">It generated statistics based on experimentation that measures hello bandwidth available between client and server nodes.</span></span> <span data-ttu-id="afd3f-135">När du testar mellan två noder som en fungerar som hello server och hello andra som en klient.</span><span class="sxs-lookup"><span data-stu-id="afd3f-135">When testing between two nodes, one acts as hello server and hello other as a client.</span></span> <span data-ttu-id="afd3f-136">När det här testet har slutförts, rekommenderar vi att du återställer hello roller tootest både ladda upp och hämta genomströmning på båda noderna.</span><span class="sxs-lookup"><span data-stu-id="afd3f-136">Once this test is completed, we recommend that you reverse hello roles tootest both upload and download throughput on both nodes.</span></span>

### <a name="download-iperf"></a><span data-ttu-id="afd3f-137">Hämta iPerf</span><span class="sxs-lookup"><span data-stu-id="afd3f-137">Download iPerf</span></span>
<span data-ttu-id="afd3f-138">Hämta [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span><span class="sxs-lookup"><span data-stu-id="afd3f-138">Download [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span></span> <span data-ttu-id="afd3f-139">Mer information finns i [iPerf dokumentationen](https://iperf.fr/iperf-doc.php).</span><span class="sxs-lookup"><span data-stu-id="afd3f-139">For details, see [iPerf documentation](https://iperf.fr/iperf-doc.php).</span></span>

 >[!NOTE]
 ><span data-ttu-id="afd3f-140">hello tredjepartsprodukter som den här artikeln beskrivs tillverkas oberoende av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="afd3f-140">hello third-party products that this article discusses are manufactured by companies that are independent of Microsoft.</span></span> <span data-ttu-id="afd3f-141">Microsoft lämnar inga garantier, underförstådda eller, om hello prestanda och pålitlighet.</span><span class="sxs-lookup"><span data-stu-id="afd3f-141">Microsoft makes no warranty, implied or otherwise, about hello performance or reliability of these products.</span></span>
 >
 >

### <a name="run-iperf-iperf3exe"></a><span data-ttu-id="afd3f-142">Kör iPerf (iperf3.exe)</span><span class="sxs-lookup"><span data-stu-id="afd3f-142">Run iPerf (iperf3.exe)</span></span>
1. <span data-ttu-id="afd3f-143">Aktivera en NSG/regeln tillåter hello trafik (för offentliga IP-adressen testa på Azure VM).</span><span class="sxs-lookup"><span data-stu-id="afd3f-143">Enable an NSG/ACL rule allowing hello traffic (for public IP address testing on Azure VM).</span></span>

2. <span data-ttu-id="afd3f-144">Aktivera ett brandväggsundantag för port 5001 på båda noderna.</span><span class="sxs-lookup"><span data-stu-id="afd3f-144">On both nodes, enable a firewall exception for port 5001.</span></span>

    <span data-ttu-id="afd3f-145">**Windows:** kör hello följande kommando som administratör:</span><span class="sxs-lookup"><span data-stu-id="afd3f-145">**Windows:** Run hello following command as an administrator:</span></span>

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    <span data-ttu-id="afd3f-146">tooremove hello regeln när testningen är klar, kör det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="afd3f-146">tooremove hello rule when testing is complete, run this command:</span></span>

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    <span data-ttu-id="afd3f-147">**Azure Linux:** Azure Linux bilder har Tillåtande brandväggar.</span><span class="sxs-lookup"><span data-stu-id="afd3f-147">**Azure Linux:**  Azure Linux images have permissive firewalls.</span></span> <span data-ttu-id="afd3f-148">Om det finns ett program som lyssnar på en port, tillåts hello trafiken.</span><span class="sxs-lookup"><span data-stu-id="afd3f-148">If there is an application listening on a port, hello traffic is allowed through.</span></span> <span data-ttu-id="afd3f-149">Anpassade avbildningar som är skyddade behöva portar som öppnats explicit.</span><span class="sxs-lookup"><span data-stu-id="afd3f-149">Custom images that are secured may need ports opened explicitly.</span></span> <span data-ttu-id="afd3f-150">Vanliga Linux OS-lager brandväggar inkluderar `iptables`, `ufw`, eller `firewalld`.</span><span class="sxs-lookup"><span data-stu-id="afd3f-150">Common Linux OS-layer firewalls include `iptables`, `ufw`, or `firewalld`.</span></span>

3. <span data-ttu-id="afd3f-151">Ändra toohello directory där iperf3.exe extraherade på hello server-noden.</span><span class="sxs-lookup"><span data-stu-id="afd3f-151">On hello server node, change toohello directory where iperf3.exe is extracted.</span></span> <span data-ttu-id="afd3f-152">Sedan kör iPerf i serverläge och ange den toolisten på port 5001 som hello följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="afd3f-152">Then run iPerf in server mode and set it toolisten on port 5001 as hello following commands:</span></span>

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. <span data-ttu-id="afd3f-153">Ändra toohello katalogen där iperf verktyget extraheras och kör sedan följande kommando hello på hello klientnod:</span><span class="sxs-lookup"><span data-stu-id="afd3f-153">On hello client node, change toohello directory where iperf tool is extracted and then run hello following command:</span></span>

    ```CMD
    iperf3.exe -c <IP of hello iperf Server> -t 30 -p 5001 -P 32
    ```

    <span data-ttu-id="afd3f-154">hello-klienten att trafik på port 5001 toohello server i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="afd3f-154">hello client is inducing traffic on port 5001 toohello server for 30 seconds.</span></span> <span data-ttu-id="afd3f-155">hello flaggan '-P-värde som anger att vi använder 32 toohello-servernoden för samtidiga anslutningar.</span><span class="sxs-lookup"><span data-stu-id="afd3f-155">hello flag '-P ' that indicates we are using 32 simultaneous connections toohello server node.</span></span>

    <span data-ttu-id="afd3f-156">hello visar följande skärmbild hello utdata från det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="afd3f-156">hello following screen shows hello output from this example:</span></span>

    ![Resultat](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. <span data-ttu-id="afd3f-158">(Valfritt) toopreserve hello testresultaten, köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="afd3f-158">(OPTIONAL) toopreserve hello testing results, run this command:</span></span>

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. <span data-ttu-id="afd3f-159">När du har slutfört föregående steg i hello köra hello samma steg med hello roller återställs, så att hello servernoden kommer nu att hello klienten och vice versa.</span><span class="sxs-lookup"><span data-stu-id="afd3f-159">After completing hello previous steps, execute hello same steps with hello roles reversed, so that hello server node will now be hello client and vice-versa.</span></span>

## <a name="address-slow-file-copy-issues"></a><span data-ttu-id="afd3f-160">Åtgärda problem med långsam kopia</span><span class="sxs-lookup"><span data-stu-id="afd3f-160">Address slow file copy issues</span></span>
<span data-ttu-id="afd3f-161">Det kan uppstå långsam filen kopieringen när Utforskaren eller dra och släppa via en RDP-session.</span><span class="sxs-lookup"><span data-stu-id="afd3f-161">You may experience slow file coping when using Windows Explorer or dragging and dropping through an RDP session.</span></span> <span data-ttu-id="afd3f-162">Det här problemet är vanligtvis på grund av tooone eller båda av hello följande faktorer:</span><span class="sxs-lookup"><span data-stu-id="afd3f-162">This problem is normally due tooone or both of hello following factors:</span></span>

- <span data-ttu-id="afd3f-163">Kopiera program, till exempel Utforskaren och RDP-filen kan inte använda flera trådar när du kopierar filer.</span><span class="sxs-lookup"><span data-stu-id="afd3f-163">File copy applications, such as Windows Explorer and RDP, do not use multiple threads when copying files.</span></span> <span data-ttu-id="afd3f-164">För bättre prestanda, Använd ett flertrådat filen kopiera program som [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy filer med hjälp av 16 eller 32 trådar.</span><span class="sxs-lookup"><span data-stu-id="afd3f-164">For better performance, use a multi-threaded file copy application such as [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) toocopy files by using 16 or 32 threads.</span></span> <span data-ttu-id="afd3f-165">toochange hello tråd nummer för filkopiering i Richcopy, klickar du på **åtgärd** > **kopiera alternativ** > **filkopieringen**.</span><span class="sxs-lookup"><span data-stu-id="afd3f-165">toochange hello thread number for file copy in Richcopy, click **Action** > **Copy options** > **File copy**.</span></span><br><br><span data-ttu-id="afd3f-166">
![Problem med långsam filens kopia](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span><span class="sxs-lookup"><span data-stu-id="afd3f-166">
![Slow file copy issues](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span></span><br>
- <span data-ttu-id="afd3f-167">Otillräcklig VM disk läsning och skrivning hastighet.</span><span class="sxs-lookup"><span data-stu-id="afd3f-167">Insufficient VM disk read/write speed.</span></span> <span data-ttu-id="afd3f-168">Mer information finns i [felsökning av Azure Storage](../storage/common/storage-e2e-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="afd3f-168">For more information, see [Azure Storage Troubleshooting](../storage/common/storage-e2e-troubleshooting.md).</span></span>

## <a name="on-premises-device-external-facing-interface"></a><span data-ttu-id="afd3f-169">Lokal enhet externa Internetriktade gränssnittet</span><span class="sxs-lookup"><span data-stu-id="afd3f-169">On-premises device external facing interface</span></span>
<span data-ttu-id="afd3f-170">Om hello lokala VPN-enhet mot Internet IP-adress som ingår i hello [lokala nätverket](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition i Azure kan uppstå när det gäller toobring hello kopplar från VPN, sporadiska eller prestandaproblem.</span><span class="sxs-lookup"><span data-stu-id="afd3f-170">If hello on-premises VPN device Internet-facing IP address is included in hello [local network](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition in Azure, you may experience inability toobring up hello VPN, sporadic disconnects, or performance issues.</span></span>

## <a name="checking-latency"></a><span data-ttu-id="afd3f-171">Kontroll av svarstid</span><span class="sxs-lookup"><span data-stu-id="afd3f-171">Checking latency</span></span>
<span data-ttu-id="afd3f-172">Använd tracert tootrace tooMicrosoft Azure Edge enheten toodetermine om fördröjningar mer än 100 millisekunder, mellan hopp.</span><span class="sxs-lookup"><span data-stu-id="afd3f-172">Use tracert tootrace tooMicrosoft Azure Edge device toodetermine if there are any delays exceeding 100 ms between hops.</span></span>

<span data-ttu-id="afd3f-173">Kör från hello lokalt nätverk *tracert* toohello VIP hello Azure Gateway eller VM.</span><span class="sxs-lookup"><span data-stu-id="afd3f-173">From hello on-premises network, run *tracert* toohello VIP of hello Azure Gateway or VM.</span></span> <span data-ttu-id="afd3f-174">När du ser endast * returneras genom att du vet hello Azure gränsen har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="afd3f-174">Once you see only * returned, you know you have reached hello Azure edge.</span></span> <span data-ttu-id="afd3f-175">När DNS-namn som innehåller ”MSN” returneras visas vet du hello Microsoft stamnät har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="afd3f-175">When you see DNS names that include "MSN" returned, you know you have reached hello Microsoft backbone.</span></span><br><br><span data-ttu-id="afd3f-176">
![Kontroll av svarstid](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span><span class="sxs-lookup"><span data-stu-id="afd3f-176">
![Checking Latency](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="afd3f-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="afd3f-177">Next steps</span></span>
<span data-ttu-id="afd3f-178">Mer information eller hjälp, ta en titt hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="afd3f-178">For more information or help, check out hello following links:</span></span>

- [<span data-ttu-id="afd3f-179">Optimera dataflödet i nätverket för virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="afd3f-179">Optimize network throughput for Azure virtual machines</span></span>](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [<span data-ttu-id="afd3f-180">Microsoft Support</span><span class="sxs-lookup"><span data-stu-id="afd3f-180">Microsoft Support</span></span>](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
