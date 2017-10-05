---
title: "Verifiera VPN-genomströmning till ett Microsoft Azure-virtuellt nätverk | Microsoft Docs"
description: "Syftet med det här dokumentet är att en användare verifiera genomströmningen i nätverket från sina lokala resurser till en virtuell Azure-dator."
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
ms.openlocfilehash: 2e0347854b5d30c955a50a01d6f7ba08e24f94b6
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="how-to-validate-vpn-throughput-to-a-virtual-network"></a><span data-ttu-id="2cfa4-103">Hur du verifierar VPN-genomströmning till ett virtuellt nätverk</span><span class="sxs-lookup"><span data-stu-id="2cfa4-103">How to validate VPN throughput to a virtual network</span></span>

<span data-ttu-id="2cfa4-104">En VPN-gateway-anslutningen kan du upprätta säkra, mellan lokala anslutningen mellan ditt virtuella nätverk i Azure och ditt lokala IT-infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-104">A VPN gateway connection enables you to establish secure, cross-premises connectivity between your Virtual Network within Azure and your on-premises IT infrastructure.</span></span>

<span data-ttu-id="2cfa4-105">Den här artikeln visar hur du verifierar dataflödet i nätverket från de lokala resurserna till en virtuell Azure-dator.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-105">This article shows how to validate network throughput from the on-premises resources to an Azure virtual machine.</span></span> <span data-ttu-id="2cfa4-106">Det ger också felsökningsinformation.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-106">It also provides troubleshooting guidance.</span></span>

>[!NOTE]
><span data-ttu-id="2cfa4-107">Den här artikeln är avsedd för att diagnostisera och lösa vanliga problem.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-107">This article is intended to help diagnose and fix common issues.</span></span> <span data-ttu-id="2cfa4-108">Om du inte kan lösa problemet med hjälp av följande information [supporten](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-108">If you're unable to solve the issue by using the following information, [contact support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade).</span></span>
>
>

## <a name="overview"></a><span data-ttu-id="2cfa4-109">Översikt</span><span class="sxs-lookup"><span data-stu-id="2cfa4-109">Overview</span></span>

<span data-ttu-id="2cfa4-110">VPN-gatewayanslutning omfattar följande komponenter:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-110">The VPN gateway connection involves the following components:</span></span>

- <span data-ttu-id="2cfa4-111">Lokala VPN-enhet (visa en lista över [verifiera VPN-enheter)](vpn-gateway-about-vpn-devices.md#devicetable).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-111">On-premises VPN device (view a list of [validated VPN devices)](vpn-gateway-about-vpn-devices.md#devicetable).</span></span>
- <span data-ttu-id="2cfa4-112">Offentliga Internet</span><span class="sxs-lookup"><span data-stu-id="2cfa4-112">Public Internet</span></span>
- <span data-ttu-id="2cfa4-113">Azure VPN-gateway</span><span class="sxs-lookup"><span data-stu-id="2cfa4-113">Azure VPN gateway</span></span>
- <span data-ttu-id="2cfa4-114">Virtuell Azure-dator</span><span class="sxs-lookup"><span data-stu-id="2cfa4-114">Azure virtual machine</span></span>

<span data-ttu-id="2cfa4-115">Följande diagram visar logiska anslutningen till ett lokalt nätverk till Azure-nätverk via VPN.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-115">The following diagram shows the logical connectivity of an on-premises network to an Azure virtual network through VPN.</span></span>

![Logiskt anslutning av kundens nätverk till MSFT nätverk via VPN](./media/vpn-gateway-validate-throughput-to-vnet/VPNPerf.png)

## <a name="calculate-the-maximum-expected-ingressegress"></a><span data-ttu-id="2cfa4-117">Beräkna det högsta förväntade ingång-/ utgång</span><span class="sxs-lookup"><span data-stu-id="2cfa4-117">Calculate the maximum expected ingress/egress</span></span>

1.  <span data-ttu-id="2cfa4-118">Fastställa kraven för ditt program baslinjen genomflöde.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-118">Determine your application's baseline throughput requirements.</span></span>
2.  <span data-ttu-id="2cfa4-119">Bestämma din Azure VPN gateway genomflöde.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-119">Determine your Azure VPN gateway throughput limits.</span></span> <span data-ttu-id="2cfa4-120">Mer hjälp finns i avsnittet ”sammanställda genomflöde av SKU- och VPN-typ” av [planering och design för VPN-Gateway](vpn-gateway-plan-design.md).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-120">For help, see the "Aggregate throughput by SKU and VPN type" section of [Planning and design for VPN Gateway](vpn-gateway-plan-design.md).</span></span>
3.  <span data-ttu-id="2cfa4-121">Ta reda på [Azure VM genomströmning vägledning](../virtual-machines/virtual-machines-windows-sizes.md) för VM-storlek.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-121">Determine the [Azure VM throughput guidance](../virtual-machines/virtual-machines-windows-sizes.md) for your VM size.</span></span>
4.  <span data-ttu-id="2cfa4-122">Kontrollera din Internet-leverantör (ISP) bandbredd.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-122">Determine your Internet Service Provider (ISP) bandwidth.</span></span>
5.  <span data-ttu-id="2cfa4-123">Beräkna din förväntade genomströmning - minsta bandbredd (VM-Gateway ISP) * 0,8.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-123">Calculate your expected throughput - Least bandwidth of (VM, Gateway, ISP) * 0.8.</span></span>

<span data-ttu-id="2cfa4-124">Om din beräknade genomströmning inte uppfyller kraven för ditt program baslinjen dataflöde, måste du öka bandbredden för den resurs som du identifierade prioriteten.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-124">If your calculated throughput does not meet your application's baseline throughput requirements, you need to increase the bandwidth of the resource that you identified as the bottleneck.</span></span> <span data-ttu-id="2cfa4-125">Om du vill ändra storlek på en Azure VPN-Gateway, se [ändra en gateway-SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-125">To resize an Azure VPN Gateway, see [Changing a gateway SKU](https://docs.microsoft.com/en-us/azure/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).</span></span> <span data-ttu-id="2cfa4-126">Om du vill ändra storlek på en virtuell dator, se [ändra storlek på en virtuell dator](../virtual-machines/virtual-machines-windows-resize-vm.md).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-126">To resize a virtual machine, see [Resize a VM](../virtual-machines/virtual-machines-windows-resize-vm.md).</span></span> <span data-ttu-id="2cfa4-127">Om du inte har den förväntade internetbandbredd, kan du också vill kontakta Leverantören.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-127">If you are not experiencing expected Internet bandwidth, you may also want to contact your ISP.</span></span>

## <a name="validate-network-throughput-by-using-performance-tools"></a><span data-ttu-id="2cfa4-128">Verifiera dataflödet i nätverket med hjälp av prestandaverktygen</span><span class="sxs-lookup"><span data-stu-id="2cfa4-128">Validate network throughput by using performance tools</span></span>

<span data-ttu-id="2cfa4-129">Den här verifieringen ska utföras vid låg belastning, som VPN-tunnel genomströmning mättnad under testningen inte ger korrekta resultat.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-129">This validation should be performed during non-peak hours, as VPN tunnel throughput saturation during testing does not give accurate results.</span></span>

<span data-ttu-id="2cfa4-130">Verktyget vi använder för det här testet är iPerf som fungerar i både Windows och Linux och har både klienten och servern lägen.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-130">The tool we use for this test is iPerf, which works on both Windows and Linux and has both client and server modes.</span></span> <span data-ttu-id="2cfa4-131">Det är begränsat till 3 Gbit/s för virtuella Windows-datorer.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-131">It is limited to 3 Gbps for Windows VMs.</span></span>

<span data-ttu-id="2cfa4-132">Det här verktyget inte att utföra några åtgärder för läsning och skrivning till disk.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-132">This tool does not perform any read/write operations to disk.</span></span> <span data-ttu-id="2cfa4-133">Den genererar endast självgenererat TCP-trafik från en slutpunkt till den andra.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-133">It solely produces self-generated TCP traffic from one end to the other.</span></span> <span data-ttu-id="2cfa4-134">Den genererade statistik baserat på experiment som mäter bandbredden som är tillgänglig mellan klienten och servern noder.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-134">It generated statistics based on experimentation that measures the bandwidth available between client and server nodes.</span></span> <span data-ttu-id="2cfa4-135">När du testar mellan två noder som en fungerar som servern och den andra som en klient.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-135">When testing between two nodes, one acts as the server and the other as a client.</span></span> <span data-ttu-id="2cfa4-136">När det här testet har slutförts, rekommenderar vi att du återställer roller för att testa både ladda upp och ladda ned dataflödet på båda noderna.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-136">Once this test is completed, we recommend that you reverse the roles to test both upload and download throughput on both nodes.</span></span>

### <a name="download-iperf"></a><span data-ttu-id="2cfa4-137">Hämta iPerf</span><span class="sxs-lookup"><span data-stu-id="2cfa4-137">Download iPerf</span></span>
<span data-ttu-id="2cfa4-138">Hämta [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-138">Download [iPerf](https://iperf.fr/download/iperf_3.1/iperf-3.1.2-win64.zip).</span></span> <span data-ttu-id="2cfa4-139">Mer information finns i [iPerf dokumentationen](https://iperf.fr/iperf-doc.php).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-139">For details, see [iPerf documentation](https://iperf.fr/iperf-doc.php).</span></span>

 >[!NOTE]
 ><span data-ttu-id="2cfa4-140">Tredjepartsprodukter som den här artikeln beskrivs tillverkas oberoende av Microsoft.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-140">The third-party products that this article discusses are manufactured by companies that are independent of Microsoft.</span></span> <span data-ttu-id="2cfa4-141">Microsoft lämnar inga garantier, uttryckliga eller på annat sätt, prestanda och pålitlighet.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-141">Microsoft makes no warranty, implied or otherwise, about the performance or reliability of these products.</span></span>
 >
 >

### <a name="run-iperf-iperf3exe"></a><span data-ttu-id="2cfa4-142">Kör iPerf (iperf3.exe)</span><span class="sxs-lookup"><span data-stu-id="2cfa4-142">Run iPerf (iperf3.exe)</span></span>
1. <span data-ttu-id="2cfa4-143">Aktivera en NSG/ACL-regel som tillåter trafik (för offentliga IP-adressen testa på Azure VM).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-143">Enable an NSG/ACL rule allowing the traffic (for public IP address testing on Azure VM).</span></span>

2. <span data-ttu-id="2cfa4-144">Aktivera ett brandväggsundantag för port 5001 på båda noderna.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-144">On both nodes, enable a firewall exception for port 5001.</span></span>

    <span data-ttu-id="2cfa4-145">**Windows:** kör följande kommando som administratör:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-145">**Windows:** Run the following command as an administrator:</span></span>

    ```CMD
    netsh advfirewall firewall add rule name="Open Port 5001" dir=in action=allow protocol=TCP localport=5001
    ```

    <span data-ttu-id="2cfa4-146">Om du vill ta bort regeln när testningen är klar, köra det här kommandot:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-146">To remove the rule when testing is complete, run this command:</span></span>

    ```CMD
    netsh advfirewall firewall delete rule name="Open Port 5001" protocol=TCP localport=5001
    ```
    </br>
    <span data-ttu-id="2cfa4-147">**Azure Linux:** Azure Linux bilder har Tillåtande brandväggar.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-147">**Azure Linux:**  Azure Linux images have permissive firewalls.</span></span> <span data-ttu-id="2cfa4-148">Om det finns ett program som lyssnar på en port, tillåts trafik via.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-148">If there is an application listening on a port, the traffic is allowed through.</span></span> <span data-ttu-id="2cfa4-149">Anpassade avbildningar som är skyddade behöva portar som öppnats explicit.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-149">Custom images that are secured may need ports opened explicitly.</span></span> <span data-ttu-id="2cfa4-150">Vanliga Linux OS-lager brandväggar inkluderar `iptables`, `ufw`, eller `firewalld`.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-150">Common Linux OS-layer firewalls include `iptables`, `ufw`, or `firewalld`.</span></span>

3. <span data-ttu-id="2cfa4-151">Ändra till katalogen där iperf3.exe extraherade på server-noden.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-151">On the server node, change to the directory where iperf3.exe is extracted.</span></span> <span data-ttu-id="2cfa4-152">Sedan kör iPerf i serverläge och ställas in att lyssna på porten 5001 som följande kommandon:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-152">Then run iPerf in server mode and set it to listen on port 5001 as the following commands:</span></span>

     ```CMD
     cd c:\iperf-3.1.2-win65

     iperf3.exe -s -p 5001
     ```

4. <span data-ttu-id="2cfa4-153">Ändra till katalogen där iperf verktyget extraheras och kör sedan följande kommando på noden klienten:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-153">On the client node, change to the directory where iperf tool is extracted and then run the following command:</span></span>

    ```CMD
    iperf3.exe -c <IP of the iperf Server> -t 30 -p 5001 -P 32
    ```

    <span data-ttu-id="2cfa4-154">Klienten att trafik på port 5001 till servern i 30 sekunder.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-154">The client is inducing traffic on port 5001 to the server for 30 seconds.</span></span> <span data-ttu-id="2cfa4-155">Flaggan '-P-värde som anger att vi använder 32 samtidiga anslutningar till noden.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-155">The flag '-P ' that indicates we are using 32 simultaneous connections to the server node.</span></span>

    <span data-ttu-id="2cfa4-156">Följande skärmbild visar utdata från det här exemplet:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-156">The following screen shows the output from this example:</span></span>

    ![Resultat](./media/vpn-gateway-validate-throughput-to-vnet/06theoutput.png)

5. <span data-ttu-id="2cfa4-158">(VALFRITT) För att bevara testresultaten, kör du kommandot:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-158">(OPTIONAL) To preserve the testing results, run this command:</span></span>

    ```CMD
    iperf3.exe -c IPofTheServerToReach -t 30 -p 5001 -P 32  >> output.txt
    ```

6. <span data-ttu-id="2cfa4-159">När du har slutfört föregående steg, kör du samma steg med de roller som återställs, så att servernoden kommer nu att klienten och vice versa.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-159">After completing the previous steps, execute the same steps with the roles reversed, so that the server node will now be the client and vice-versa.</span></span>

## <a name="address-slow-file-copy-issues"></a><span data-ttu-id="2cfa4-160">Åtgärda problem med långsam kopia</span><span class="sxs-lookup"><span data-stu-id="2cfa4-160">Address slow file copy issues</span></span>
<span data-ttu-id="2cfa4-161">Det kan uppstå långsam filen kopieringen när Utforskaren eller dra och släppa via en RDP-session.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-161">You may experience slow file coping when using Windows Explorer or dragging and dropping through an RDP session.</span></span> <span data-ttu-id="2cfa4-162">Det här problemet beror normalt på en eller båda av följande faktorer:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-162">This problem is normally due to one or both of the following factors:</span></span>

- <span data-ttu-id="2cfa4-163">Kopiera program, till exempel Utforskaren och RDP-filen kan inte använda flera trådar när du kopierar filer.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-163">File copy applications, such as Windows Explorer and RDP, do not use multiple threads when copying files.</span></span> <span data-ttu-id="2cfa4-164">För bättre prestanda, Använd ett flertrådat filen kopiera program som [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) att kopiera filer med hjälp av 16 eller 32 trådar.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-164">For better performance, use a multi-threaded file copy application such as [Richcopy](https://technet.microsoft.com/en-us/magazine/2009.04.utilityspotlight.aspx) to copy files by using 16 or 32 threads.</span></span> <span data-ttu-id="2cfa4-165">Så här ändrar du numret tråd filkopiering i Richcopy **åtgärd** > **kopiera alternativ** > **filkopieringen**.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-165">To change the thread number for file copy in Richcopy, click **Action** > **Copy options** > **File copy**.</span></span><br><br><span data-ttu-id="2cfa4-166">
![Problem med långsam filens kopia](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span><span class="sxs-lookup"><span data-stu-id="2cfa4-166">
![Slow file copy issues](./media/vpn-gateway-validate-throughput-to-vnet/Richcopy.png)</span></span><br>
- <span data-ttu-id="2cfa4-167">Otillräcklig VM disk läsning och skrivning hastighet.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-167">Insufficient VM disk read/write speed.</span></span> <span data-ttu-id="2cfa4-168">Mer information finns i [felsökning av Azure Storage](../storage/common/storage-e2e-troubleshooting.md).</span><span class="sxs-lookup"><span data-stu-id="2cfa4-168">For more information, see [Azure Storage Troubleshooting](../storage/common/storage-e2e-troubleshooting.md).</span></span>

## <a name="on-premises-device-external-facing-interface"></a><span data-ttu-id="2cfa4-169">Lokal enhet externa Internetriktade gränssnittet</span><span class="sxs-lookup"><span data-stu-id="2cfa4-169">On-premises device external facing interface</span></span>
<span data-ttu-id="2cfa4-170">Om lokala VPN-enhet mot Internet IP-adress som ingår i den [lokala nätverket](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition i Azure kan uppstå när det gäller att sätta upp kopplar från VPN, sporadiska eller prestandaproblem.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-170">If the on-premises VPN device Internet-facing IP address is included in the [local network](vpn-gateway-howto-site-to-site-resource-manager-portal.md#LocalNetworkGateway) definition in Azure, you may experience inability to bring up the VPN, sporadic disconnects, or performance issues.</span></span>

## <a name="checking-latency"></a><span data-ttu-id="2cfa4-171">Kontroll av svarstid</span><span class="sxs-lookup"><span data-stu-id="2cfa4-171">Checking latency</span></span>
<span data-ttu-id="2cfa4-172">Använd tracert att spåra till Microsoft Azure-enhet för att avgöra om det är fördröjningar mer än 100 millisekunder, mellan hopp.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-172">Use tracert to trace to Microsoft Azure Edge device to determine if there are any delays exceeding 100 ms between hops.</span></span>

<span data-ttu-id="2cfa4-173">Kör från det lokala nätverket *tracert* till VIP Azure Gateway eller VM.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-173">From the on-premises network, run *tracert* to the VIP of the Azure Gateway or VM.</span></span> <span data-ttu-id="2cfa4-174">När du ser endast * returneras genom att du vet att du har nått gränsen för Azure.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-174">Once you see only * returned, you know you have reached the Azure edge.</span></span> <span data-ttu-id="2cfa4-175">När DNS-namn som innehåller ”MSN” returneras visas vet du Microsoft-stamnät har uppnåtts.</span><span class="sxs-lookup"><span data-stu-id="2cfa4-175">When you see DNS names that include "MSN" returned, you know you have reached the Microsoft backbone.</span></span><br><br><span data-ttu-id="2cfa4-176">
![Kontroll av svarstid](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span><span class="sxs-lookup"><span data-stu-id="2cfa4-176">
![Checking Latency](./media/vpn-gateway-validate-throughput-to-vnet/08checkinglatency.png)</span></span>

## <a name="next-steps"></a><span data-ttu-id="2cfa4-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2cfa4-177">Next steps</span></span>
<span data-ttu-id="2cfa4-178">Mer information eller hjälp finns på följande länkar:</span><span class="sxs-lookup"><span data-stu-id="2cfa4-178">For more information or help, check out the following links:</span></span>

- [<span data-ttu-id="2cfa4-179">Optimera dataflödet i nätverket för virtuella Azure-datorer</span><span class="sxs-lookup"><span data-stu-id="2cfa4-179">Optimize network throughput for Azure virtual machines</span></span>](../virtual-network/virtual-network-optimize-network-bandwidth.md)
- [<span data-ttu-id="2cfa4-180">Microsoft Support</span><span class="sxs-lookup"><span data-stu-id="2cfa4-180">Microsoft Support</span></span>](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
