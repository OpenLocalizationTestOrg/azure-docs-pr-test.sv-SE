---
title: "aaaVisualize nätverk trafikmönster med öppen källkod verktyg och Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan beskrivs hur toouse Nätverksbevakaren paket avbilda med Capanalysis toovisualize trafik mönster tooand från dina virtuella datorer."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 936d881b-49f9-4798-8e45-d7185ec9fe89
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: fca9a226729162cd90d412c7b699ac54d2257a0d
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualize-network-traffic-patterns-tooand-from-your-vms-using-open-source-tools"></a><span data-ttu-id="52199-103">Visualisera nätverket trafik mönster tooand från dina virtuella datorer med hjälp av verktyg för öppen källkod</span><span class="sxs-lookup"><span data-stu-id="52199-103">Visualize network traffic patterns tooand from your VMs using open source tools</span></span>

<span data-ttu-id="52199-104">Paketet insamlingar innehålla nätverksdata som gör att du tooperform nätverk dataforensik och djup paketinspektion.</span><span class="sxs-lookup"><span data-stu-id="52199-104">Packet captures contain network data that allow you tooperform network forensics and deep packet inspection.</span></span> <span data-ttu-id="52199-105">Det finns många öppnas källa verktyg som du kan använda tooanalyze paket insamlingar toogain insikter om nätverket.</span><span class="sxs-lookup"><span data-stu-id="52199-105">There are many opens source tools you can use tooanalyze packet captures toogain insights about your network.</span></span> <span data-ttu-id="52199-106">Ett sådant verktyg är CapAnalysis, ett verktyg för öppen källkod nätverkspaket avbilda visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="52199-106">One such tool is CapAnalysis, an open source packet capture visualization tool.</span></span> <span data-ttu-id="52199-107">Visualisera avbilda paketdata är ett bra sätt tooquickly härleda insikter på trafikmönster och avvikelser i nätverket.</span><span class="sxs-lookup"><span data-stu-id="52199-107">Visualizing packet capture data is a valuable way tooquickly derive insights on patterns and anomalies within your network.</span></span> <span data-ttu-id="52199-108">Visualiseringar ger också möjlighet att dela sådana insikter på ett sätt som är lätta att använda.</span><span class="sxs-lookup"><span data-stu-id="52199-108">Visualizations also provide a means of sharing such insights in an easily consumable manner.</span></span>

<span data-ttu-id="52199-109">Azures Nätverksbevakaren ger du hello möjlighet toocapture värdefulla data genom att låta dig tooperform paket samlar in i nätverket.</span><span class="sxs-lookup"><span data-stu-id="52199-109">Azure’s Network Watcher provides you hello ability toocapture this valuable data by allowing you tooperform packet captures on your network.</span></span> <span data-ttu-id="52199-110">I den här artikeln får en genomgång av hur toovisualize och få insikter från paketet fångar med Nätverksbevakaren CapAnalysis.</span><span class="sxs-lookup"><span data-stu-id="52199-110">In this article, we provide a walkthrough of how toovisualize and gain insights from packet captures using CapAnalysis with Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="52199-111">Scenario</span><span class="sxs-lookup"><span data-stu-id="52199-111">Scenario</span></span>

<span data-ttu-id="52199-112">Du har en enkel webbapp som distribueras på en virtuell dator i Azure vill toouse öppen källkod verktyg toovisualize dess nätverk trafik tooquickly identifiera flödet mönster och alla eventuella avvikelser.</span><span class="sxs-lookup"><span data-stu-id="52199-112">You have a simple web application deployed on a VM in Azure want toouse open source tools toovisualize its network traffic tooquickly identify flow patterns and any possible anomalies.</span></span> <span data-ttu-id="52199-113">Med Nätverksbevakaren, kan du hämta en paketinsamling för din nätverksmiljö och spara dem direkt på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="52199-113">With Network Watcher, you can obtain a packet capture of your network environment and directly store it on your storage account.</span></span> <span data-ttu-id="52199-114">CapAnalysis kan mata in hello paketinsamling direkt från hello storage-blob och visualisera dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="52199-114">CapAnalysis can then ingest hello packet capture directly from hello storage blob and visualize its contents.</span></span>

![scenario][1]

## <a name="steps"></a><span data-ttu-id="52199-116">Steg</span><span class="sxs-lookup"><span data-stu-id="52199-116">Steps</span></span>

### <a name="install-capanalysis"></a><span data-ttu-id="52199-117">Installera CapAnalysis</span><span class="sxs-lookup"><span data-stu-id="52199-117">Install CapAnalysis</span></span>

<span data-ttu-id="52199-118">tooinstall CapAnalysis på en virtuell dator som du kan se toohello officiella anvisningarna här https://www.capanalysis.net/ca/how-to-install-capanalysis.</span><span class="sxs-lookup"><span data-stu-id="52199-118">tooinstall CapAnalysis on a virtual machine, you can refer toohello official instructions here https://www.capanalysis.net/ca/how-to-install-capanalysis.</span></span>
<span data-ttu-id="52199-119">I ordning åt CapAnalysis måste vi tooopen port 9877 på den virtuella datorn genom att lägga till en ny regel för inkommande säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="52199-119">In order access CapAnalysis remotely, we need tooopen port 9877 on your VM by adding a new inbound security rule.</span></span> <span data-ttu-id="52199-120">Mer information om hur du skapar regler i Nätverkssäkerhetsgrupper finns för[skapa regler i en befintlig NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span><span class="sxs-lookup"><span data-stu-id="52199-120">For more about creating rules in Network Security Groups, refer too[Create rules in an existing NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span></span> <span data-ttu-id="52199-121">När hello regeln har lagts bör du kunna tooaccess CapAnalysis från`http://<PublicIP>:9877`</span><span class="sxs-lookup"><span data-stu-id="52199-121">Once hello rule has been successfully added, you should be able tooaccess CapAnalysis from `http://<PublicIP>:9877`</span></span>

### <a name="use-azure-network-watcher-toostart-a-packet-capture-session"></a><span data-ttu-id="52199-122">Använd Azure Nätverksbevakaren toostart ett paket avbilda session</span><span class="sxs-lookup"><span data-stu-id="52199-122">Use Azure Network Watcher toostart a packet capture session</span></span>

<span data-ttu-id="52199-123">Nätverksbevakaren tillåter toocapture paket tootrack trafik till och från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="52199-123">Network Watcher allows you toocapture packets tootrack traffic in and out of a virtual machine.</span></span> <span data-ttu-id="52199-124">Du kan se toohello instruktionerna på [hantera paket som samlar in med Nätverksbevakaren](network-watcher-packet-capture-manage-portal.md) toostart en paketet avbildningssessionen.</span><span class="sxs-lookup"><span data-stu-id="52199-124">You can refer toohello instructions at [Manage packet captures with Network Watcher](network-watcher-packet-capture-manage-portal.md) toostart a packet capture session.</span></span> <span data-ttu-id="52199-125">Den här paketinsamling kan lagras i en lagring blob toobe nås av CapAnalysis.</span><span class="sxs-lookup"><span data-stu-id="52199-125">This packet capture can be stored in a storage blob toobe accessed by CapAnalysis.</span></span>

### <a name="upload-a-packet-capture-toocapanalysis"></a><span data-ttu-id="52199-126">Ladda upp en avbildning tooCapAnalysis för paketet</span><span class="sxs-lookup"><span data-stu-id="52199-126">Upload a packet capture tooCapAnalysis</span></span>
<span data-ttu-id="52199-127">Du kan ladda upp en paketinsamling vidtagit nätverksbevakaren hello ”importera från URL” fliken och tillhandahålla en länk toohello lagringsblob där hello paketinsamling lagras direkt.</span><span class="sxs-lookup"><span data-stu-id="52199-127">You can directly upload a packet capture taken by network watcher using hello “Import from URL” tab and providing a link toohello storage blob where hello packet capture is stored.</span></span>

<span data-ttu-id="52199-128">När du anger en länk tooCapAnalysis se till att tooappend en SAS-token toohello storage blob-URL.</span><span class="sxs-lookup"><span data-stu-id="52199-128">When providing a link tooCapAnalysis, make sure tooappend a SAS token toohello storage blob URL.</span></span>  <span data-ttu-id="52199-129">toodo detta, navigera tooShared åtkomstsignatur från hello lagringskonto, ange hello behörigheter och tryck på hello generera SAS knappen toocreate en token.</span><span class="sxs-lookup"><span data-stu-id="52199-129">toodo this, navigate tooShared access signature from hello storage account, designate hello allowed permissions, and press hello Generate SAS button toocreate a token.</span></span> <span data-ttu-id="52199-130">Du kan sedan lägga till den här SAS token toohello paket avbilda storage blob-URL.</span><span class="sxs-lookup"><span data-stu-id="52199-130">You can then append this SAS token toohello packet capture storage blob URL.</span></span>

<span data-ttu-id="52199-131">hello resulterande URL: en kommer att se ut ungefär så här: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span><span class="sxs-lookup"><span data-stu-id="52199-131">hello resulting URL will look something like this: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span></span>


### <a name="analyzing-packet-captures"></a><span data-ttu-id="52199-132">När paketet avbildas</span><span class="sxs-lookup"><span data-stu-id="52199-132">Analyzing packet captures</span></span>

<span data-ttu-id="52199-133">CapAnalysis erbjuder olika alternativ toovisualize din paketinsamling, varje med analys från olika perspektiv.</span><span class="sxs-lookup"><span data-stu-id="52199-133">CapAnalysis offers various options toovisualize your packet capture, each providing analysis from a different perspective.</span></span> <span data-ttu-id="52199-134">Med dessa visual sammanfattningar du förstå trenderna nätverket trafik och snabbt hitta några ovanliga aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="52199-134">With these visual summaries, you can understand your network traffic trends and quickly spot any unusual activity.</span></span> <span data-ttu-id="52199-135">Några av dessa funktioner visas i följande lista hello:</span><span class="sxs-lookup"><span data-stu-id="52199-135">A few of these features are shown in hello following list:</span></span>

1. <span data-ttu-id="52199-136">Flödet tabeller</span><span class="sxs-lookup"><span data-stu-id="52199-136">Flow Tables</span></span>

    <span data-ttu-id="52199-137">Den här tabellen kan du hello lista med flödena i hello paketdata hello tidsstämpel som är associerade med hello flöden och hello olika protokoll som är associerade med hello flödet som källa och mål-IP.</span><span class="sxs-lookup"><span data-stu-id="52199-137">This table gives you hello list of flows in hello packet data, hello time stamp associated with hello flows and hello various protocols associated with hello flow, as well as source and destination IP.</span></span>

    ![capanalysis flödar sida][5]

1. <span data-ttu-id="52199-139">Översikt över Protocol</span><span class="sxs-lookup"><span data-stu-id="52199-139">Protocol Overview</span></span>

    <span data-ttu-id="52199-140">Det här fönstret kan du tooquickly Se hello distribution av nätverkstrafik via hello olika protokoll och geografiska områden.</span><span class="sxs-lookup"><span data-stu-id="52199-140">This pane allows you tooquickly see hello distribution of network traffic over hello various protocols and geographies.</span></span>

    ![Översikt över capanalysis protocol][6]

1. <span data-ttu-id="52199-142">Statistik</span><span class="sxs-lookup"><span data-stu-id="52199-142">Statistics</span></span>

    <span data-ttu-id="52199-143">Det här fönstret kan du tooview trafik nätverksstatistik – byte skickas och tas emot från käll- och IP-adresser, flöden för varje hello käll- och IP-adresser, protokoll som används för olika flöden och hello varaktighet för flöden.</span><span class="sxs-lookup"><span data-stu-id="52199-143">This pane allows you tooview network traffic statistics – bytes sent and received from source and destination IPs, flows for each of hello source and destination IPs, protocol used for various flows, and hello duration of flows.</span></span>

    ![capanalysis statistik][7]

1. <span data-ttu-id="52199-145">geomap</span><span class="sxs-lookup"><span data-stu-id="52199-145">Geomap</span></span>

    <span data-ttu-id="52199-146">Det här fönstret ger dig en kartvyn av nätverkstrafiken, med färger skalning toohello mängden trafik från varje land.</span><span class="sxs-lookup"><span data-stu-id="52199-146">This pane provides you with a map view of your network traffic, with colors scaling toohello volume of traffic from each country.</span></span> <span data-ttu-id="52199-147">Du kan välja markerade länder tooview ytterligare flödet av statistik, t.ex hello andel av data som skickas och tas emot från IP-adresser i det landet.</span><span class="sxs-lookup"><span data-stu-id="52199-147">You can select highlighted countries tooview additional flow statistics such as hello proportion of data sent and received from IPs in that country.</span></span>

    ![geomap][8]

1. <span data-ttu-id="52199-149">Filter</span><span class="sxs-lookup"><span data-stu-id="52199-149">Filters</span></span>

    <span data-ttu-id="52199-150">CapAnalysis innehåller en uppsättning filter för snabb analys av specifika paket.</span><span class="sxs-lookup"><span data-stu-id="52199-150">CapAnalysis provides a set of filters for quick analysis of specific packets.</span></span> <span data-ttu-id="52199-151">Exempelvis kan du välja toofilter hello data av protokollet toogain specifika insikter om den del av trafiken.</span><span class="sxs-lookup"><span data-stu-id="52199-151">For example, you can choose toofilter hello data by protocol toogain specific insights on that subset of traffic.</span></span>

    ![filter][11]

    <span data-ttu-id="52199-153">Besök [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) toolearn mer om alla CapAnalysis funktioner.</span><span class="sxs-lookup"><span data-stu-id="52199-153">Visit [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) toolearn more about all CapAnalysis' capabilities.</span></span>

## <a name="conclusion"></a><span data-ttu-id="52199-154">Slutsats</span><span class="sxs-lookup"><span data-stu-id="52199-154">Conclusion</span></span>

<span data-ttu-id="52199-155">Nätverket Watcher paket capture-funktionen kan du toocapture hello data nödvändiga tooperform nätverk dataforensik och bättre förstå trafik på nätverket.</span><span class="sxs-lookup"><span data-stu-id="52199-155">Network Watcher’s packet capture feature allows you toocapture hello data necessary tooperform network forensics and better understand your network traffic.</span></span> <span data-ttu-id="52199-156">I det här scenariot visade hur paket insamlingar från Nätverksbevakaren enkelt kan integreras med öppen källkod visualiseringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="52199-156">In this scenario, we showed how packet captures from Network Watcher can easily be integrated with open source visualization tools.</span></span> <span data-ttu-id="52199-157">Med öppen källkod som samlar in CapAnalysis toovisualize paket kan du utföra djup paketinspektion och snabbt identifiera trender i nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="52199-157">By using open source tools such as CapAnalysis toovisualize packets captures, you can perform deep packet inspection and quickly identify trends within your network traffic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="52199-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="52199-158">Next steps</span></span>

<span data-ttu-id="52199-159">toolearn mer information om NSG flödet loggar finns [NSG flödet loggar](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="52199-159">toolearn more about NSG flow logs, visit [NSG Flow logs](network-watcher-nsg-flow-logging-overview.md)</span></span>

<span data-ttu-id="52199-160">Lär dig hur toovisualize NSG-flöde loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="52199-160">Learn how toovisualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>
<!--Image references-->

[1]: ./media/network-watcher-using-open-source-tools/figure1.png
[2]: ./media/network-watcher-using-open-source-tools/figure2.png
[3]: ./media/network-watcher-using-open-source-tools/figure3.png
[4]: ./media/network-watcher-using-open-source-tools/figure4.png
[5]: ./media/network-watcher-using-open-source-tools/figure5.png
[6]: ./media/network-watcher-using-open-source-tools/figure6.png
[7]: ./media/network-watcher-using-open-source-tools/figure7.png
[8]: ./media/network-watcher-using-open-source-tools/figure8.png
[9]: ./media/network-watcher-using-open-source-tools/figure9.png
[10]: ./media/network-watcher-using-open-source-tools/figure10.png
[11]: ./media/network-watcher-using-open-source-tools/figure11.png
