---
title: "Paketinspektion med Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här artikeln beskriver hur du använder Nätverksbevakaren för att utföra djup paketinspektion samlas in från en virtuell dator"
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 7b907d00-9c35-40f5-a61e-beb7b782276f
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: 91c47bb8922a9be21dff72e7cf64b29b14a36e9e
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a><span data-ttu-id="a643a-103">Paketinspektion med Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="a643a-103">Packet inspection with Azure Network Watcher</span></span>

<span data-ttu-id="a643a-104">Funktionen paket avbildning av Nätverksbevakaren kan du starta och hantera insamlingar sessionerna på din virtuella Azure-datorer från portalen, PowerShell, CLI och programmässigt via SDK och REST-API.</span><span class="sxs-lookup"><span data-stu-id="a643a-104">Using the packet capture feature of Network Watcher, you can initiate and manage captures sessions on your Azure VMs from the portal, PowerShell, CLI, and programmatically through the SDK and REST API.</span></span> <span data-ttu-id="a643a-105">Du kan adressen scenarier som kräver nivån paketdata genom att tillhandahålla information i ett format som är användbara i paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="a643a-105">Packet capture allows you to address scenarios that require packet level data by providing the information in a readily usable format.</span></span> <span data-ttu-id="a643a-106">Utnyttja gratisverktyg för att granska data du undersöka kommunikation till och från dina virtuella datorer och få insyn i nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="a643a-106">Leveraging freely available tools to inspect the data, you can examine communications sent to and from your VMs and gain insights into your network traffic.</span></span> <span data-ttu-id="a643a-107">Vissa exempel användningsområden för avbildning paketdata är: undersöker problem med nätverket eller ett program, identifiera nätverket missbruk och intrångsidentifiering försök eller upprätthålla regelefterlevnad.</span><span class="sxs-lookup"><span data-stu-id="a643a-107">Some example uses of packet capture data include: investigating network or application issues, detecting network misuse and intrusion attempts, or maintaining regulatory compliance.</span></span> <span data-ttu-id="a643a-108">I den här artikeln visar vi hur du öppnar en fil för avbildning av paket som tillhandahålls av Nätverksbevakaren verktyget populära öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="a643a-108">In this article, we show how to open a packet capture file provided by Network Watcher using a popular open source tool.</span></span> <span data-ttu-id="a643a-109">Vi ger även exempel som visar hur du beräkna en fördröjning för anslutning, identifiera onormalt trafik och undersöka nätverk statistik.</span><span class="sxs-lookup"><span data-stu-id="a643a-109">We will also provide examples showing how to calculate a connection latency, identify abnormal traffic, and examine networking statistics.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="a643a-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="a643a-110">Before you begin</span></span>

<span data-ttu-id="a643a-111">Den här artikeln går igenom några förkonfigurerade scenarier på paketinsamling som kördes tidigare.</span><span class="sxs-lookup"><span data-stu-id="a643a-111">This article goes through some pre-configured scenarios on a packet capture that was run previously.</span></span> <span data-ttu-id="a643a-112">Dessa scenarier visar funktioner som kan nås genom att granska en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="a643a-112">These scenarios illustrate capabilities that can be accessed by reviewing a packet capture.</span></span> <span data-ttu-id="a643a-113">Det här scenariot använder [WireShark](https://www.wireshark.org/) att inspektera paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="a643a-113">This scenario uses [WireShark](https://www.wireshark.org/) to inspect the packet capture.</span></span>

<span data-ttu-id="a643a-114">Det här scenariot förutsätter att du körde en paketinsamling på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="a643a-114">This scenario assumes you already ran a packet capture on a virtual machine.</span></span> <span data-ttu-id="a643a-115">Mer information om hur du skapar ett paket avbilda besök [hantera paket som samlar in med portalen](network-watcher-packet-capture-manage-portal.md) eller med övriga genom att besöka [hantera paket som avbildar med REST API](network-watcher-packet-capture-manage-rest.md).</span><span class="sxs-lookup"><span data-stu-id="a643a-115">To learn how to create a packet capture visit [Manage packet captures with the portal](network-watcher-packet-capture-manage-portal.md) or with REST by visiting [Managing Packet Captures with REST API](network-watcher-packet-capture-manage-rest.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="a643a-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="a643a-116">Scenario</span></span>

<span data-ttu-id="a643a-117">I det här scenariot kan du:</span><span class="sxs-lookup"><span data-stu-id="a643a-117">In this scenario, you:</span></span>

* <span data-ttu-id="a643a-118">Granska en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="a643a-118">Review a packet capture</span></span>

## <a name="calculate-network-latency"></a><span data-ttu-id="a643a-119">Beräkna nätverks-svarstid</span><span class="sxs-lookup"><span data-stu-id="a643a-119">Calculate network latency</span></span>

<span data-ttu-id="a643a-120">I det här scenariot visar vi hur du visar inledande förfluten tid (RTT) i en konversation för Transmission Control Protocol (TCP) som sker mellan två slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="a643a-120">In this scenario, we show how to view the initial Round Trip Time (RTT) of a Transmission Control Protocol (TCP) conversation occurring between two endpoints.</span></span>

<span data-ttu-id="a643a-121">När en TCP-anslutning har upprättats följer de första tre paket som skickats under anslutningen ett mönster som benämns trevägs-handskakningen.</span><span class="sxs-lookup"><span data-stu-id="a643a-121">When a TCP connection is established, the first three packets sent in the connection follow a pattern commonly referred to as the three-way handshake.</span></span> <span data-ttu-id="a643a-122">Genom att undersöka först två paket som skickats under den här handskakningen första begäran från klienten och ett svar från servern, kan vi beräkna svarstiden när anslutningen upprättades.</span><span class="sxs-lookup"><span data-stu-id="a643a-122">By examining the first two packets sent in this handshake, an initial request from the client and a response from the server, we can calculate the latency when this connection was established.</span></span> <span data-ttu-id="a643a-123">Den här fördröjningen kallas som förfluten tid (RTT).</span><span class="sxs-lookup"><span data-stu-id="a643a-123">This latency is referred to as the Round Trip Time (RTT).</span></span> <span data-ttu-id="a643a-124">Mer information om TCP-protokollet och trevägs handskakningen finns i följande resurs.</span><span class="sxs-lookup"><span data-stu-id="a643a-124">For more information on the TCP protocol and the three-way handshake refer to the following resource.</span></span> <span data-ttu-id="a643a-125">https://support.microsoft.com/en-US/Help/172983/EXPLANATION-of-the-Three-Way-Handshake-via-TCP-IP</span><span class="sxs-lookup"><span data-stu-id="a643a-125">https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip</span></span>

### <a name="step-1"></a><span data-ttu-id="a643a-126">Steg 1</span><span class="sxs-lookup"><span data-stu-id="a643a-126">Step 1</span></span>

<span data-ttu-id="a643a-127">Starta WireShark</span><span class="sxs-lookup"><span data-stu-id="a643a-127">Launch WireShark</span></span>

### <a name="step-2"></a><span data-ttu-id="a643a-128">Steg 2</span><span class="sxs-lookup"><span data-stu-id="a643a-128">Step 2</span></span>

<span data-ttu-id="a643a-129">Läs in den **CAP** fil från din paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="a643a-129">Load the **.cap** file from your packet capture.</span></span> <span data-ttu-id="a643a-130">Den här filen finns i blob som den har sparats i vårt lokalt på den virtuella datorn, beroende på hur du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="a643a-130">This file can be found in the blob it was saved in our locally on the virtual machine, depending on how you configured it.</span></span>

### <a name="step-3"></a><span data-ttu-id="a643a-131">Steg 3</span><span class="sxs-lookup"><span data-stu-id="a643a-131">Step 3</span></span>

<span data-ttu-id="a643a-132">Om du vill visa inledande förfluten tid (RTT) i TCP konversationer kommer vi bara att titta på de första två paket som ingår i TCP-handskakningen.</span><span class="sxs-lookup"><span data-stu-id="a643a-132">To view the initial Round Trip Time (RTT) in TCP conversations, we will only be looking at the first two packets involved in the TCP handshake.</span></span> <span data-ttu-id="a643a-133">Vi kommer att använda två första paketen i 3-vägs-handskakning som är [SYN], [SYN, ACK] paket.</span><span class="sxs-lookup"><span data-stu-id="a643a-133">We will be using the first two packets in the three-way handshake, which are the [SYN], [SYN, ACK] packets.</span></span> <span data-ttu-id="a643a-134">De har namngetts för flaggor i TCP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="a643a-134">They are named for flags set in the TCP header.</span></span> <span data-ttu-id="a643a-135">Senaste paketet i handskakningen [ACK]-paketet används inte i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="a643a-135">The last packet in the handshake, the [ACK] packet, will not be used in this scenario.</span></span> <span data-ttu-id="a643a-136">[SYN]-paket har skickats av klienten.</span><span class="sxs-lookup"><span data-stu-id="a643a-136">The [SYN] packet is sent by the client.</span></span> <span data-ttu-id="a643a-137">När den tas emot skickar servern [ACK]-paketet som en bekräftelse för att ta emot SYN från klienten.</span><span class="sxs-lookup"><span data-stu-id="a643a-137">Once it is received the server sends the [ACK] packet as an acknowledgement of receiving the SYN from the client.</span></span> <span data-ttu-id="a643a-138">Utnyttja det faktum att serverns svar kräver mycket lite belastning vi beräkna RTT genom att subtrahera tiden [SYN, ACK] paketet togs emot av klienten när [SYN] paket skickades av klienten.</span><span class="sxs-lookup"><span data-stu-id="a643a-138">Leveraging the fact that the server’s response requires very little overhead, we calculate the RTT by subtracting the time the [SYN, ACK] packet was received by the client by the time [SYN] packet was sent by the client.</span></span>

<span data-ttu-id="a643a-139">Med WireShark beräknas detta värde för oss.</span><span class="sxs-lookup"><span data-stu-id="a643a-139">Using WireShark this value is calculated for us.</span></span>

<span data-ttu-id="a643a-140">Om du vill visa de första två paket enklare i TCP trevägs handskakningen, ska vi använda filtrering funktioner av WireShark.</span><span class="sxs-lookup"><span data-stu-id="a643a-140">To more easily view the first two packets in the TCP three-way handshake, we will utilize the filtering capability provided by WireShark.</span></span>

<span data-ttu-id="a643a-141">Om du vill använda filter i WireShark, expandera ”Transmission Control Protocol”-Segment i ett paket [SYN] i din avbildning och undersöka flaggor i TCP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="a643a-141">To apply the filter in WireShark, expand the “Transmission Control Protocol” Segment of a [SYN] packet in your capture and examine the flags set in the TCP header.</span></span>

<span data-ttu-id="a643a-142">Eftersom vi vill filtrera på alla [SYN] och [SYN ACK] paket under flaggor cofirm att Syn biten anges till 1 och sedan högerklickar du på Syn-bitars -> Använd som Filter -> valda.</span><span class="sxs-lookup"><span data-stu-id="a643a-142">Since we are looking to filter on all [SYN] and [SYN, ACK] packets, under flags cofirm that the Syn bit is set to 1, then right click on the Syn bit -> Apply as Filter -> Selected.</span></span>

![Bild 7][7]

### <a name="step-4"></a><span data-ttu-id="a643a-144">Steg 4</span><span class="sxs-lookup"><span data-stu-id="a643a-144">Step 4</span></span>

<span data-ttu-id="a643a-145">Du kan enkelt välja konversationer som du är intresserad av att visa inledande RTT nu när du har filtrerat fönstret om du vill endast visa paket med bitars [SYN].</span><span class="sxs-lookup"><span data-stu-id="a643a-145">Now that you have filtered the window to only see packets with the [SYN] bit set, you can easily select conversations you are interested in to view the initial RTT.</span></span> <span data-ttu-id="a643a-146">Ett enkelt sätt att visa RTT i WireShark klickar du på listrutan markerad ”SEQ-ACK” analys.</span><span class="sxs-lookup"><span data-stu-id="a643a-146">A simple way to view the RTT in WireShark simply click the dropdown marked “SEQ/ACK” analysis.</span></span> <span data-ttu-id="a643a-147">Sedan visas RTT visas.</span><span class="sxs-lookup"><span data-stu-id="a643a-147">You will then see the RTT displayed.</span></span> <span data-ttu-id="a643a-148">I det här fallet har RTT 0.0022114 sekunder eller 2.211 ms.</span><span class="sxs-lookup"><span data-stu-id="a643a-148">In this case, the RTT was 0.0022114 seconds, or 2.211 ms.</span></span>

![bild 8][8]

## <a name="unwanted-protocols"></a><span data-ttu-id="a643a-150">Oönskade protokoll</span><span class="sxs-lookup"><span data-stu-id="a643a-150">Unwanted protocols</span></span>

<span data-ttu-id="a643a-151">Du kan ha många program som körs på en virtuell dator-instans som du har distribuerat i Azure.</span><span class="sxs-lookup"><span data-stu-id="a643a-151">You can have many applications running on a virtual machine instance you have deployed in Azure.</span></span> <span data-ttu-id="a643a-152">Många av dessa program kommunicera över nätverket, kanske utan uttryckligt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="a643a-152">Many of these applications communicate over the network, perhaps without your explicit permission.</span></span> <span data-ttu-id="a643a-153">Använder paketinsamling för att lagra nätverkskommunikation, kan vi undersöka hur programmet pratar i nätverket och leta efter eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="a643a-153">Using packet capture to store network communication, we can investigate how application are talking on the network and look for any issues.</span></span>

<span data-ttu-id="a643a-154">I det här exemplet vi går igenom ett tidigare körde paketinsamling för oönskade protokoll som kan indikera obehörig kommunikation från ett program som körs på datorn.</span><span class="sxs-lookup"><span data-stu-id="a643a-154">In this example, we review a previous ran packet capture for unwanted protocols that may indicate unauthorized communication from an application running on your machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="a643a-155">Steg 1</span><span class="sxs-lookup"><span data-stu-id="a643a-155">Step 1</span></span>

<span data-ttu-id="a643a-156">Använda samma avbildning i scenariot ovan klickar du på **statistik** > **protokollet hierarki**</span><span class="sxs-lookup"><span data-stu-id="a643a-156">Using the same capture in the previous scenario click **Statistics** > **Protocol Hierarchy**</span></span>

![protokollet hierarki-menyn][2]

<span data-ttu-id="a643a-158">Fönstret protokollet hierarki visas.</span><span class="sxs-lookup"><span data-stu-id="a643a-158">The protocol hierarchy window appears.</span></span> <span data-ttu-id="a643a-159">Den här vyn visar en lista över protokoll som har använts under avbildningssessionen och antalet paket som skickas och tas emot med hjälp av protokollen.</span><span class="sxs-lookup"><span data-stu-id="a643a-159">This view provides a list of all the protocols that were in use during the capture session and the number of packets transmitted and received using the protocols.</span></span> <span data-ttu-id="a643a-160">Den här vyn kan vara användbart för att hitta oönskad trafik på virtuella datorer eller i nätverket.</span><span class="sxs-lookup"><span data-stu-id="a643a-160">This view can be useful for finding unwanted network traffic on your virtual machines or network.</span></span>

![protokollet hierachy öppnas][3]

<span data-ttu-id="a643a-162">Som du ser i följande skärmbild uppstod trafik via BitTorrent-protokollet som används för peer-to-peer-fildelning.</span><span class="sxs-lookup"><span data-stu-id="a643a-162">As you can see in the following screen capture, there was traffic using the BitTorrent protocol, which is used for peer to peer file sharing.</span></span> <span data-ttu-id="a643a-163">Som administratör du inte räknar med att se BitTorrent trafik på denna specifika virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="a643a-163">As an administrator you do not expect to see BitTorrent traffic on this particular virtual machines.</span></span> <span data-ttu-id="a643a-164">Nu kan du känner till den här trafiken, du kan ta bort peer-to-peer-program som installerats på den virtuella datorn eller blockera trafik med hjälp av Nätverkssäkerhetsgrupper eller en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="a643a-164">Now you aware of this traffic, you can remove the peer to peer software that installed on this virtual machine, or block the traffic using Network Security Groups or a Firewall.</span></span> <span data-ttu-id="a643a-165">Dessutom kan du välja att köra paket insamlingar enligt ett schema så att du kan granska protokollet används på de virtuella datorerna regelbundet.</span><span class="sxs-lookup"><span data-stu-id="a643a-165">Additionally, you may elect to run packet captures on a schedule, so you can review the protocol use on your virtual machines regularly.</span></span> <span data-ttu-id="a643a-166">Ett exempel på hur du automatiserar Nätverksaktiviteter i azure finns [övervaka nätverksresurser med azure automation](network-watcher-monitor-with-azure-automation.md)</span><span class="sxs-lookup"><span data-stu-id="a643a-166">For an example on how to automate network tasks in azure visit [Monitor network resources with azure automation](network-watcher-monitor-with-azure-automation.md)</span></span>

## <a name="finding-top-destinations-and-ports"></a><span data-ttu-id="a643a-167">Söka efter vanliga mål och portar</span><span class="sxs-lookup"><span data-stu-id="a643a-167">Finding top destinations and ports</span></span>

<span data-ttu-id="a643a-168">Förstå vilka typer av trafik, slutpunkterna och portar som kommunicerades via är en viktig vid övervakning eller felsöka program och resurser i nätverket.</span><span class="sxs-lookup"><span data-stu-id="a643a-168">Understanding the types of traffic, the endpoints, and the ports communicated over is an important when monitoring or troubleshooting applications and resources on your network.</span></span> <span data-ttu-id="a643a-169">Använda filen ett paket från ovan kan vi snabbt lära vanliga mål våra VM kommunicerar med och portar som används.</span><span class="sxs-lookup"><span data-stu-id="a643a-169">Utilizing a packet capture file from above, we can quickly learn the top destinations our VM is communicating with and the ports being utilized.</span></span>

### <a name="step-1"></a><span data-ttu-id="a643a-170">Steg 1</span><span class="sxs-lookup"><span data-stu-id="a643a-170">Step 1</span></span>

<span data-ttu-id="a643a-171">Använda samma avbildning i scenariot ovan klickar du på **statistik** > **IPv4-statistik** > **mål och portar**</span><span class="sxs-lookup"><span data-stu-id="a643a-171">Using the same capture in the previous scenario click **Statistics** > **IPv4 Statistics** > **Destinations and Ports**</span></span>

![paketet insamlings-fönstret][4]

### <a name="step-2"></a><span data-ttu-id="a643a-173">Steg 2</span><span class="sxs-lookup"><span data-stu-id="a643a-173">Step 2</span></span>

<span data-ttu-id="a643a-174">Som vi titta igenom de resultat som en rad framhävs fanns det flera anslutningar på port 111.</span><span class="sxs-lookup"><span data-stu-id="a643a-174">As we look through the results a line stands out, there were multiple connections on port 111.</span></span> <span data-ttu-id="a643a-175">Var används mest port 3389, som är Fjärrskrivbord och de återstående är dynamiska RPC-portar.</span><span class="sxs-lookup"><span data-stu-id="a643a-175">The most used port was 3389, which is remote desktop, and the remaining are RPC dynamic ports.</span></span>

<span data-ttu-id="a643a-176">När den här trafiken kan innebära att ingenting, är en port som användes för många anslutningar och är okänt för administratören.</span><span class="sxs-lookup"><span data-stu-id="a643a-176">While this traffic may mean nothing, it is a port that was used for many connections and is unknown to the administrator.</span></span>

![Bild 5][5]

### <a name="step-3"></a><span data-ttu-id="a643a-178">Steg 3</span><span class="sxs-lookup"><span data-stu-id="a643a-178">Step 3</span></span>

<span data-ttu-id="a643a-179">Nu när vi har fastställt att en out-of plats port filtrera vi våra avbildning baserat på porten.</span><span class="sxs-lookup"><span data-stu-id="a643a-179">Now that we have determined an out of place port we can filter our capture based on the port.</span></span>

<span data-ttu-id="a643a-180">Filter i det här scenariot är:</span><span class="sxs-lookup"><span data-stu-id="a643a-180">The filter in this scenario would be:</span></span>

```
tcp.port == 111
```

<span data-ttu-id="a643a-181">Vi ange filtertext ovan i textrutan filter och träffar.</span><span class="sxs-lookup"><span data-stu-id="a643a-181">We enter the filter text from above in the filter textbox and hit enter.</span></span>

![Bild 6][6]

<span data-ttu-id="a643a-183">Vi kan se all trafik kommer från en lokal virtuell dator i samma undernät från sökresultaten.</span><span class="sxs-lookup"><span data-stu-id="a643a-183">From the results, we can see all the traffic is coming from a local virtual machine on the same subnet.</span></span> <span data-ttu-id="a643a-184">Om vi inte fortfarande förstår varför den här trafiken uppstår, kan vi ytterligare inspektera paket för att avgöra varför den anropar dessa på port 111.</span><span class="sxs-lookup"><span data-stu-id="a643a-184">If we still don’t understand why this traffic is occurring, we can further inspect the packets to determine why it is making these calls on port 111.</span></span> <span data-ttu-id="a643a-185">Med den här informationen kan vi vidta lämplig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="a643a-185">With this information we can take the appropriate action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="a643a-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a643a-186">Next steps</span></span>

<span data-ttu-id="a643a-187">Lär dig mer om andra diagnostiska funktioner i Nätverksbevakaren genom att besöka [Azure översikt för nätverksövervakning](network-watcher-monitoring-overview.md)</span><span class="sxs-lookup"><span data-stu-id="a643a-187">Learn about the other diagnostic features of Network Watcher by visiting [Azure network monitoring overview](network-watcher-monitoring-overview.md)</span></span>

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













