---
title: "aaaPacket kontroll och Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här artikeln beskriver hur toouse Nätverksbevakaren tooperform djup paketinspektion samlas in från en virtuell dator"
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
ms.openlocfilehash: 4aeddcd482edc4df3d63e87b5c4b0788c540850b
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="packet-inspection-with-azure-network-watcher"></a><span data-ttu-id="c2d33-103">Paketinspektion med Azure Nätverksbevakaren</span><span class="sxs-lookup"><span data-stu-id="c2d33-103">Packet inspection with Azure Network Watcher</span></span>

<span data-ttu-id="c2d33-104">Funktionen hello paket avbildning av Nätverksbevakaren kan du starta och hantera insamlingar sessionerna på din virtuella Azure-datorer från hello-portalen, PowerShell, CLI, och genom programmering via hello SDK och REST-API.</span><span class="sxs-lookup"><span data-stu-id="c2d33-104">Using hello packet capture feature of Network Watcher, you can initiate and manage captures sessions on your Azure VMs from hello portal, PowerShell, CLI, and programmatically through hello SDK and REST API.</span></span> <span data-ttu-id="c2d33-105">Paketinsamling kan tooaddress scenarier som kräver nivån paketdata genom att ange hello information i ett format som enkelt kan användas.</span><span class="sxs-lookup"><span data-stu-id="c2d33-105">Packet capture allows you tooaddress scenarios that require packet level data by providing hello information in a readily usable format.</span></span> <span data-ttu-id="c2d33-106">Utnyttja gratisverktyg tooinspect hello data du undersöka tooand för kommunikation från dina virtuella datorer och få insyn i nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="c2d33-106">Leveraging freely available tools tooinspect hello data, you can examine communications sent tooand from your VMs and gain insights into your network traffic.</span></span> <span data-ttu-id="c2d33-107">Vissa exempel användningsområden för avbildning paketdata är: undersöker problem med nätverket eller ett program, identifiera nätverket missbruk och intrångsidentifiering försök eller upprätthålla regelefterlevnad.</span><span class="sxs-lookup"><span data-stu-id="c2d33-107">Some example uses of packet capture data include: investigating network or application issues, detecting network misuse and intrusion attempts, or maintaining regulatory compliance.</span></span> <span data-ttu-id="c2d33-108">I den här artikeln visar vi hur tooopen filen ett paket som tillhandahålls av Nätverksbevakaren verktyget populära öppen källkod.</span><span class="sxs-lookup"><span data-stu-id="c2d33-108">In this article, we show how tooopen a packet capture file provided by Network Watcher using a popular open source tool.</span></span> <span data-ttu-id="c2d33-109">Vi ger även exempel som visar hur toocalculate en fördröjning för anslutning, identifiera onormalt trafik och undersöka nätverk statistik.</span><span class="sxs-lookup"><span data-stu-id="c2d33-109">We will also provide examples showing how toocalculate a connection latency, identify abnormal traffic, and examine networking statistics.</span></span>

## <a name="before-you-begin"></a><span data-ttu-id="c2d33-110">Innan du börjar</span><span class="sxs-lookup"><span data-stu-id="c2d33-110">Before you begin</span></span>

<span data-ttu-id="c2d33-111">Den här artikeln går igenom några förkonfigurerade scenarier på paketinsamling som kördes tidigare.</span><span class="sxs-lookup"><span data-stu-id="c2d33-111">This article goes through some pre-configured scenarios on a packet capture that was run previously.</span></span> <span data-ttu-id="c2d33-112">Dessa scenarier visar funktioner som kan nås genom att granska en paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="c2d33-112">These scenarios illustrate capabilities that can be accessed by reviewing a packet capture.</span></span> <span data-ttu-id="c2d33-113">Det här scenariot använder [WireShark](https://www.wireshark.org/) tooinspect hello paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="c2d33-113">This scenario uses [WireShark](https://www.wireshark.org/) tooinspect hello packet capture.</span></span>

<span data-ttu-id="c2d33-114">Det här scenariot förutsätter att du körde en paketinsamling på en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="c2d33-114">This scenario assumes you already ran a packet capture on a virtual machine.</span></span> <span data-ttu-id="c2d33-115">toolearn hur toocreate en paketinsamling finns [hantera paket som samlar in med hello portal](network-watcher-packet-capture-manage-portal.md) eller med övriga genom att besöka [hantera paket som avbildar med REST API](network-watcher-packet-capture-manage-rest.md).</span><span class="sxs-lookup"><span data-stu-id="c2d33-115">toolearn how toocreate a packet capture visit [Manage packet captures with hello portal](network-watcher-packet-capture-manage-portal.md) or with REST by visiting [Managing Packet Captures with REST API](network-watcher-packet-capture-manage-rest.md).</span></span>

## <a name="scenario"></a><span data-ttu-id="c2d33-116">Scenario</span><span class="sxs-lookup"><span data-stu-id="c2d33-116">Scenario</span></span>

<span data-ttu-id="c2d33-117">I det här scenariot kan du:</span><span class="sxs-lookup"><span data-stu-id="c2d33-117">In this scenario, you:</span></span>

* <span data-ttu-id="c2d33-118">Granska en paketinsamling</span><span class="sxs-lookup"><span data-stu-id="c2d33-118">Review a packet capture</span></span>

## <a name="calculate-network-latency"></a><span data-ttu-id="c2d33-119">Beräkna nätverks-svarstid</span><span class="sxs-lookup"><span data-stu-id="c2d33-119">Calculate network latency</span></span>

<span data-ttu-id="c2d33-120">I det här scenariot visar vi hur tooview hello inledande förfluten tid (RTT) i en konversation för Transmission Control Protocol (TCP) som sker mellan två slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="c2d33-120">In this scenario, we show how tooview hello initial Round Trip Time (RTT) of a Transmission Control Protocol (TCP) conversation occurring between two endpoints.</span></span>

<span data-ttu-id="c2d33-121">När en TCP-anslutning har upprättats Följ hello först tre paket som skickas i hello-anslutning en mönstret kallas tooas hello trevägs handskakning.</span><span class="sxs-lookup"><span data-stu-id="c2d33-121">When a TCP connection is established, hello first three packets sent in hello connection follow a pattern commonly referred tooas hello three-way handshake.</span></span> <span data-ttu-id="c2d33-122">Genom att undersöka först två hälsningspaket skickas i den här handskakningen, första begäran hello-klient och ett svar från hello server beräkna vi hello fördröjning när den här anslutningen har upprättats.</span><span class="sxs-lookup"><span data-stu-id="c2d33-122">By examining hello first two packets sent in this handshake, an initial request from hello client and a response from hello server, we can calculate hello latency when this connection was established.</span></span> <span data-ttu-id="c2d33-123">Den här fördröjningen är refererad tooas hello förfluten tid (RTT).</span><span class="sxs-lookup"><span data-stu-id="c2d33-123">This latency is referred tooas hello Round Trip Time (RTT).</span></span> <span data-ttu-id="c2d33-124">Mer information om hello TCP-protokollet och hello trevägs handskakning finns toohello efter resurs.</span><span class="sxs-lookup"><span data-stu-id="c2d33-124">For more information on hello TCP protocol and hello three-way handshake refer toohello following resource.</span></span> <span data-ttu-id="c2d33-125">https://support.microsoft.com/en-US/Help/172983/EXPLANATION-of-the-Three-Way-Handshake-via-TCP-IP</span><span class="sxs-lookup"><span data-stu-id="c2d33-125">https://support.microsoft.com/en-us/help/172983/explanation-of-the-three-way-handshake-via-tcp-ip</span></span>

### <a name="step-1"></a><span data-ttu-id="c2d33-126">Steg 1</span><span class="sxs-lookup"><span data-stu-id="c2d33-126">Step 1</span></span>

<span data-ttu-id="c2d33-127">Starta WireShark</span><span class="sxs-lookup"><span data-stu-id="c2d33-127">Launch WireShark</span></span>

### <a name="step-2"></a><span data-ttu-id="c2d33-128">Steg 2</span><span class="sxs-lookup"><span data-stu-id="c2d33-128">Step 2</span></span>

<span data-ttu-id="c2d33-129">Läs in hello **CAP** fil från din paketinsamling.</span><span class="sxs-lookup"><span data-stu-id="c2d33-129">Load hello **.cap** file from your packet capture.</span></span> <span data-ttu-id="c2d33-130">Den här filen finns i hello blob som den har sparats i vårt lokalt på hello virtuell dator, beroende på hur du har konfigurerat.</span><span class="sxs-lookup"><span data-stu-id="c2d33-130">This file can be found in hello blob it was saved in our locally on hello virtual machine, depending on how you configured it.</span></span>

### <a name="step-3"></a><span data-ttu-id="c2d33-131">Steg 3</span><span class="sxs-lookup"><span data-stu-id="c2d33-131">Step 3</span></span>

<span data-ttu-id="c2d33-132">tooview Hej inledande förfluten tid (RTT) i TCP konversationer, vi kommer bara att titta på två första hälsningspaket ingår i hello TCP-handskakningen.</span><span class="sxs-lookup"><span data-stu-id="c2d33-132">tooview hello initial Round Trip Time (RTT) in TCP conversations, we will only be looking at hello first two packets involved in hello TCP handshake.</span></span> <span data-ttu-id="c2d33-133">Vi kommer att använda två första hälsningspaket i hello trevägs handskakning som är hello [SYN], [SYN, ACK] paket.</span><span class="sxs-lookup"><span data-stu-id="c2d33-133">We will be using hello first two packets in hello three-way handshake, which are hello [SYN], [SYN, ACK] packets.</span></span> <span data-ttu-id="c2d33-134">De har namngetts för flaggor i hello TCP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="c2d33-134">They are named for flags set in hello TCP header.</span></span> <span data-ttu-id="c2d33-135">senaste hello-paket i hello-handskakningen hello [ACK] paketet används inte i det här scenariot.</span><span class="sxs-lookup"><span data-stu-id="c2d33-135">hello last packet in hello handshake, hello [ACK] packet, will not be used in this scenario.</span></span> <span data-ttu-id="c2d33-136">Hej [SYN] paket skickas av hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="c2d33-136">hello [SYN] packet is sent by hello client.</span></span> <span data-ttu-id="c2d33-137">När den tas emot skickar hello server hello [ACK] paket som en bekräftelse för att ta emot hello SYN hello-klient.</span><span class="sxs-lookup"><span data-stu-id="c2d33-137">Once it is received hello server sends hello [ACK] packet as an acknowledgement of receiving hello SYN from hello client.</span></span> <span data-ttu-id="c2d33-138">Utnyttja hello faktum att hello serversvar kräver mycket lite belastning beräkna vi hello RTT genom att subtrahera hello tid hello [SYN, ACK] paketet togs emot av klienten hello av hello tid [SYN] paket har skickats av hello-klienten.</span><span class="sxs-lookup"><span data-stu-id="c2d33-138">Leveraging hello fact that hello server’s response requires very little overhead, we calculate hello RTT by subtracting hello time hello [SYN, ACK] packet was received by hello client by hello time [SYN] packet was sent by hello client.</span></span>

<span data-ttu-id="c2d33-139">Med WireShark beräknas detta värde för oss.</span><span class="sxs-lookup"><span data-stu-id="c2d33-139">Using WireShark this value is calculated for us.</span></span>

<span data-ttu-id="c2d33-140">toomore Se enkelt två första hälsningspaket i hello TCP trevägs handskakning, vi ska använda hello filtrering kapaciteten som tillhandahålls av WireShark.</span><span class="sxs-lookup"><span data-stu-id="c2d33-140">toomore easily view hello first two packets in hello TCP three-way handshake, we will utilize hello filtering capability provided by WireShark.</span></span>

<span data-ttu-id="c2d33-141">tooapply hello filter i WireShark, expandera hello ”Transmission Control Protocol” segmentet i ett paket [SYN] i din avbildning och undersöka hello flaggor i hello TCP-huvudet.</span><span class="sxs-lookup"><span data-stu-id="c2d33-141">tooapply hello filter in WireShark, expand hello “Transmission Control Protocol” Segment of a [SYN] packet in your capture and examine hello flags set in hello TCP header.</span></span>

<span data-ttu-id="c2d33-142">Eftersom vi söker toofilter på alla [SYN] och [SYN ACK] paket under flaggor cofirm att hello Syn-biten är aktiverad too1 sedan högerklickar du på hello Syn bitar -> Använd som Filter -> valda.</span><span class="sxs-lookup"><span data-stu-id="c2d33-142">Since we are looking toofilter on all [SYN] and [SYN, ACK] packets, under flags cofirm that hello Syn bit is set too1, then right click on hello Syn bit -> Apply as Filter -> Selected.</span></span>

![Bild 7][7]

### <a name="step-4"></a><span data-ttu-id="c2d33-144">Steg 4</span><span class="sxs-lookup"><span data-stu-id="c2d33-144">Step 4</span></span>

<span data-ttu-id="c2d33-145">Nu när du har filtrerat fönstret tooonly Se hälsningspaket med hello [SYN] set-bitars kan du enkelt välja konversationer som du är intresserad av tooview hello inledande RTT.</span><span class="sxs-lookup"><span data-stu-id="c2d33-145">Now that you have filtered hello window tooonly see packets with hello [SYN] bit set, you can easily select conversations you are interested in tooview hello initial RTT.</span></span> <span data-ttu-id="c2d33-146">Ett enkelt sätt tooview hello RTT i WireShark Klicka bara på hello dropdown markerad ”SEQ-ACK” analys.</span><span class="sxs-lookup"><span data-stu-id="c2d33-146">A simple way tooview hello RTT in WireShark simply click hello dropdown marked “SEQ/ACK” analysis.</span></span> <span data-ttu-id="c2d33-147">Sedan visas hello RTT visas.</span><span class="sxs-lookup"><span data-stu-id="c2d33-147">You will then see hello RTT displayed.</span></span> <span data-ttu-id="c2d33-148">I det här fallet kunde hello RTT 0.0022114 sekunder eller 2.211 ms.</span><span class="sxs-lookup"><span data-stu-id="c2d33-148">In this case, hello RTT was 0.0022114 seconds, or 2.211 ms.</span></span>

![bild 8][8]

## <a name="unwanted-protocols"></a><span data-ttu-id="c2d33-150">Oönskade protokoll</span><span class="sxs-lookup"><span data-stu-id="c2d33-150">Unwanted protocols</span></span>

<span data-ttu-id="c2d33-151">Du kan ha många program som körs på en virtuell dator-instans som du har distribuerat i Azure.</span><span class="sxs-lookup"><span data-stu-id="c2d33-151">You can have many applications running on a virtual machine instance you have deployed in Azure.</span></span> <span data-ttu-id="c2d33-152">Många av dessa program kommunicerar via hello-nätverket kanske utan uttryckligt tillstånd.</span><span class="sxs-lookup"><span data-stu-id="c2d33-152">Many of these applications communicate over hello network, perhaps without your explicit permission.</span></span> <span data-ttu-id="c2d33-153">Undersök med hjälp av paket avbilda toostore nätverkskommunikation, vi kan hur programmet pratar hello nätverket och leta efter eventuella problem.</span><span class="sxs-lookup"><span data-stu-id="c2d33-153">Using packet capture toostore network communication, we can investigate how application are talking on hello network and look for any issues.</span></span>

<span data-ttu-id="c2d33-154">I det här exemplet vi går igenom ett tidigare körde paketinsamling för oönskade protokoll som kan indikera obehörig kommunikation från ett program som körs på datorn.</span><span class="sxs-lookup"><span data-stu-id="c2d33-154">In this example, we review a previous ran packet capture for unwanted protocols that may indicate unauthorized communication from an application running on your machine.</span></span>

### <a name="step-1"></a><span data-ttu-id="c2d33-155">Steg 1</span><span class="sxs-lookup"><span data-stu-id="c2d33-155">Step 1</span></span>

<span data-ttu-id="c2d33-156">Med hjälp av hello samma avbildning i hello föregående scenario klickar du på **statistik** > **protokollet hierarki**</span><span class="sxs-lookup"><span data-stu-id="c2d33-156">Using hello same capture in hello previous scenario click **Statistics** > **Protocol Hierarchy**</span></span>

![protokollet hierarki-menyn][2]

<span data-ttu-id="c2d33-158">hello visas protokollet hierarki.</span><span class="sxs-lookup"><span data-stu-id="c2d33-158">hello protocol hierarchy window appears.</span></span> <span data-ttu-id="c2d33-159">Den här vyn visar en lista över alla hello-protokoll som har använts under hello avbildningssessionen och hello antalet paket som skickas och tas emot med hello-protokoll.</span><span class="sxs-lookup"><span data-stu-id="c2d33-159">This view provides a list of all hello protocols that were in use during hello capture session and hello number of packets transmitted and received using hello protocols.</span></span> <span data-ttu-id="c2d33-160">Den här vyn kan vara användbart för att hitta oönskad trafik på virtuella datorer eller i nätverket.</span><span class="sxs-lookup"><span data-stu-id="c2d33-160">This view can be useful for finding unwanted network traffic on your virtual machines or network.</span></span>

![protokollet hierachy öppnas][3]

<span data-ttu-id="c2d33-162">Som du ser i följande skärmbild hello uppstod trafik med hjälp av hello BitTorrent protokoll som används för peer-toopeer fildelning.</span><span class="sxs-lookup"><span data-stu-id="c2d33-162">As you can see in hello following screen capture, there was traffic using hello BitTorrent protocol, which is used for peer toopeer file sharing.</span></span> <span data-ttu-id="c2d33-163">Som administratör räknar du inte toosee BitTorrent trafik på denna specifika virtuella datorer.</span><span class="sxs-lookup"><span data-stu-id="c2d33-163">As an administrator you do not expect toosee BitTorrent traffic on this particular virtual machines.</span></span> <span data-ttu-id="c2d33-164">Nu du känner till den här trafiken, du kan ta bort hello peer toopeer programvara som installeras på den här virtuella datorn eller blockera hello trafik med hjälp av Nätverkssäkerhetsgrupper eller en brandvägg.</span><span class="sxs-lookup"><span data-stu-id="c2d33-164">Now you aware of this traffic, you can remove hello peer toopeer software that installed on this virtual machine, or block hello traffic using Network Security Groups or a Firewall.</span></span> <span data-ttu-id="c2d33-165">Du kan dessutom välja toorun paket insamlingar enligt ett schema så att du kan granska hello-protokollet används på de virtuella datorerna regelbundet.</span><span class="sxs-lookup"><span data-stu-id="c2d33-165">Additionally, you may elect toorun packet captures on a schedule, so you can review hello protocol use on your virtual machines regularly.</span></span> <span data-ttu-id="c2d33-166">Ett exempel på hur tooautomate nätverk uppgifter i azure besök [övervaka nätverksresurser med azure automation](network-watcher-monitor-with-azure-automation.md)</span><span class="sxs-lookup"><span data-stu-id="c2d33-166">For an example on how tooautomate network tasks in azure visit [Monitor network resources with azure automation](network-watcher-monitor-with-azure-automation.md)</span></span>

## <a name="finding-top-destinations-and-ports"></a><span data-ttu-id="c2d33-167">Söka efter vanliga mål och portar</span><span class="sxs-lookup"><span data-stu-id="c2d33-167">Finding top destinations and ports</span></span>

<span data-ttu-id="c2d33-168">Förstå hello typer av trafik, hello slutpunkter och hello-portar som kommunicerades via är en viktig vid övervakning eller felsöka program och resurser i nätverket.</span><span class="sxs-lookup"><span data-stu-id="c2d33-168">Understanding hello types of traffic, hello endpoints, and hello ports communicated over is an important when monitoring or troubleshooting applications and resources on your network.</span></span> <span data-ttu-id="c2d33-169">Använda filen ett paket från ovan kan vi snabbt lära hello vanliga mål våra VM kommunicerar med och hello portar som används.</span><span class="sxs-lookup"><span data-stu-id="c2d33-169">Utilizing a packet capture file from above, we can quickly learn hello top destinations our VM is communicating with and hello ports being utilized.</span></span>

### <a name="step-1"></a><span data-ttu-id="c2d33-170">Steg 1</span><span class="sxs-lookup"><span data-stu-id="c2d33-170">Step 1</span></span>

<span data-ttu-id="c2d33-171">Med hjälp av hello samma avbildning i hello föregående scenario klickar du på **statistik** > **IPv4-statistik** > **mål och portar**</span><span class="sxs-lookup"><span data-stu-id="c2d33-171">Using hello same capture in hello previous scenario click **Statistics** > **IPv4 Statistics** > **Destinations and Ports**</span></span>

![paketet insamlings-fönstret][4]

### <a name="step-2"></a><span data-ttu-id="c2d33-173">Steg 2</span><span class="sxs-lookup"><span data-stu-id="c2d33-173">Step 2</span></span>

<span data-ttu-id="c2d33-174">Vi ser hello resultat via en rad framhävs, det fanns flera anslutningar på port 111.</span><span class="sxs-lookup"><span data-stu-id="c2d33-174">As we look through hello results a line stands out, there were multiple connections on port 111.</span></span> <span data-ttu-id="c2d33-175">var hello används oftast port 3389, vilket är Fjärrskrivbord och hello återstående är dynamiska RPC-portar.</span><span class="sxs-lookup"><span data-stu-id="c2d33-175">hello most used port was 3389, which is remote desktop, and hello remaining are RPC dynamic ports.</span></span>

<span data-ttu-id="c2d33-176">När den här trafiken kan innebära att ingenting, är en port som användes för många anslutningar och är okänd toohello administratör.</span><span class="sxs-lookup"><span data-stu-id="c2d33-176">While this traffic may mean nothing, it is a port that was used for many connections and is unknown toohello administrator.</span></span>

![Bild 5][5]

### <a name="step-3"></a><span data-ttu-id="c2d33-178">Steg 3</span><span class="sxs-lookup"><span data-stu-id="c2d33-178">Step 3</span></span>

<span data-ttu-id="c2d33-179">Nu när vi har fastställt att en out-of plats port filtrera vi våra avbildning baserat på hello port.</span><span class="sxs-lookup"><span data-stu-id="c2d33-179">Now that we have determined an out of place port we can filter our capture based on hello port.</span></span>

<span data-ttu-id="c2d33-180">hello filter i det här scenariot är:</span><span class="sxs-lookup"><span data-stu-id="c2d33-180">hello filter in this scenario would be:</span></span>

```
tcp.port == 111
```

<span data-ttu-id="c2d33-181">Vi ange hello filtertext ovan hello filter-textrutan med och träffar.</span><span class="sxs-lookup"><span data-stu-id="c2d33-181">We enter hello filter text from above in hello filter textbox and hit enter.</span></span>

![Bild 6][6]

<span data-ttu-id="c2d33-183">Hello resultaten kan vi se alla hello trafiken kommer från en lokal virtuell dator på hello samma undernät.</span><span class="sxs-lookup"><span data-stu-id="c2d33-183">From hello results, we can see all hello traffic is coming from a local virtual machine on hello same subnet.</span></span> <span data-ttu-id="c2d33-184">Om vi inte fortfarande förstår varför den här trafiken uppstår, kan vi ytterligare inspektera hello paket toodetermine varför den anropar dessa på port 111.</span><span class="sxs-lookup"><span data-stu-id="c2d33-184">If we still don’t understand why this traffic is occurring, we can further inspect hello packets toodetermine why it is making these calls on port 111.</span></span> <span data-ttu-id="c2d33-185">Med den här informationen kan vi ta hello lämplig åtgärd.</span><span class="sxs-lookup"><span data-stu-id="c2d33-185">With this information we can take hello appropriate action.</span></span>

## <a name="next-steps"></a><span data-ttu-id="c2d33-186">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c2d33-186">Next steps</span></span>

<span data-ttu-id="c2d33-187">Lär dig mer om hello andra diagnostiska funktioner i Nätverksbevakaren genom att besöka [Azure översikt för nätverksövervakning](network-watcher-monitoring-overview.md)</span><span class="sxs-lookup"><span data-stu-id="c2d33-187">Learn about hello other diagnostic features of Network Watcher by visiting [Azure network monitoring overview](network-watcher-monitoring-overview.md)</span></span>

[1]: ./media/network-watcher-deep-packet-inspection/figure1.png
[2]: ./media/network-watcher-deep-packet-inspection/figure2.png
[3]: ./media/network-watcher-deep-packet-inspection/figure3.png
[4]: ./media/network-watcher-deep-packet-inspection/figure4.png
[5]: ./media/network-watcher-deep-packet-inspection/figure5.png
[6]: ./media/network-watcher-deep-packet-inspection/figure6.png
[7]: ./media/network-watcher-deep-packet-inspection/figure7.png
[8]: ./media/network-watcher-deep-packet-inspection/figure8.png













