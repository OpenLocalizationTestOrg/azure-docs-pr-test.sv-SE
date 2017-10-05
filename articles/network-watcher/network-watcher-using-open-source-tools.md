---
title: "Visualisera nätverket trafikmönster med öppen källkod verktyg och Azure Nätverksbevakaren | Microsoft Docs"
description: "Den här sidan beskrivs hur du använder Nätverksbevakaren paketinsamling med Capanalysis visualisera trafikmönster till och från dina virtuella datorer."
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
ms.openlocfilehash: e27bb694d0cbcf1ff7c9d8ca4682a79c8b5c5cb1
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="visualize-network-traffic-patterns-to-and-from-your-vms-using-open-source-tools"></a><span data-ttu-id="84582-103">Visualisera nätverket trafikmönster till och från dina virtuella datorer med hjälp av verktyg för öppen källkod</span><span class="sxs-lookup"><span data-stu-id="84582-103">Visualize network traffic patterns to and from your VMs using open source tools</span></span>

<span data-ttu-id="84582-104">Paketet insamlingar innehålla nätverksdata så att du kan utföra nätverket dataforensik och djup paketinspektion.</span><span class="sxs-lookup"><span data-stu-id="84582-104">Packet captures contain network data that allow you to perform network forensics and deep packet inspection.</span></span> <span data-ttu-id="84582-105">Det finns många öppnas källa verktyg som du kan använda för att analysera paket insamlingar för att få insikter om nätverket.</span><span class="sxs-lookup"><span data-stu-id="84582-105">There are many opens source tools you can use to analyze packet captures to gain insights about your network.</span></span> <span data-ttu-id="84582-106">Ett sådant verktyg är CapAnalysis, ett verktyg för öppen källkod nätverkspaket avbilda visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="84582-106">One such tool is CapAnalysis, an open source packet capture visualization tool.</span></span> <span data-ttu-id="84582-107">Visualisera avbilda paketdata är ett bra sätt att snabbt härleda insikter på trafikmönster och avvikelser i nätverket.</span><span class="sxs-lookup"><span data-stu-id="84582-107">Visualizing packet capture data is a valuable way to quickly derive insights on patterns and anomalies within your network.</span></span> <span data-ttu-id="84582-108">Visualiseringar ger också möjlighet att dela sådana insikter på ett sätt som är lätta att använda.</span><span class="sxs-lookup"><span data-stu-id="84582-108">Visualizations also provide a means of sharing such insights in an easily consumable manner.</span></span>

<span data-ttu-id="84582-109">Azures Nätverksbevakaren ger möjlighet att samla in värdefulla data genom att du kan utföra paket insamlingar i nätverket.</span><span class="sxs-lookup"><span data-stu-id="84582-109">Azure’s Network Watcher provides you the ability to capture this valuable data by allowing you to perform packet captures on your network.</span></span> <span data-ttu-id="84582-110">I den här artikeln ger en genomgång av hur du visualisera och få insikter från paketet fångar med Nätverksbevakaren CapAnalysis.</span><span class="sxs-lookup"><span data-stu-id="84582-110">In this article, we provide a walkthrough of how to visualize and gain insights from packet captures using CapAnalysis with Network Watcher.</span></span>

## <a name="scenario"></a><span data-ttu-id="84582-111">Scenario</span><span class="sxs-lookup"><span data-stu-id="84582-111">Scenario</span></span>

<span data-ttu-id="84582-112">Du har en enkel webbapp har distribuerats på en virtuell dator i Azure vill använda öppen källkod verktyg för att visualisera dess nätverkstrafik för att snabbt identifiera flödet mönster och alla möjliga inkonsekvenser.</span><span class="sxs-lookup"><span data-stu-id="84582-112">You have a simple web application deployed on a VM in Azure want to use open source tools to visualize its network traffic to quickly identify flow patterns and any possible anomalies.</span></span> <span data-ttu-id="84582-113">Med Nätverksbevakaren, kan du hämta en paketinsamling för din nätverksmiljö och spara dem direkt på ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="84582-113">With Network Watcher, you can obtain a packet capture of your network environment and directly store it on your storage account.</span></span> <span data-ttu-id="84582-114">CapAnalysis kan mata in paketinsamling direkt från storage-blob och visualisera dess innehåll.</span><span class="sxs-lookup"><span data-stu-id="84582-114">CapAnalysis can then ingest the packet capture directly from the storage blob and visualize its contents.</span></span>

![scenario][1]

## <a name="steps"></a><span data-ttu-id="84582-116">Steg</span><span class="sxs-lookup"><span data-stu-id="84582-116">Steps</span></span>

### <a name="install-capanalysis"></a><span data-ttu-id="84582-117">Installera CapAnalysis</span><span class="sxs-lookup"><span data-stu-id="84582-117">Install CapAnalysis</span></span>

<span data-ttu-id="84582-118">Om du vill installera CapAnalysis på en virtuell dator måste referera du till officiella anvisningarna här https://www.capanalysis.net/ca/how-to-install-capanalysis.</span><span class="sxs-lookup"><span data-stu-id="84582-118">To install CapAnalysis on a virtual machine, you can refer to the official instructions here https://www.capanalysis.net/ca/how-to-install-capanalysis.</span></span>
<span data-ttu-id="84582-119">I ordning åt CapAnalysis måste vi öppna port 9877 på den virtuella datorn genom att lägga till en ny regel för inkommande säkerhetsregler.</span><span class="sxs-lookup"><span data-stu-id="84582-119">In order access CapAnalysis remotely, we need to open port 9877 on your VM by adding a new inbound security rule.</span></span> <span data-ttu-id="84582-120">Mer information om hur du skapar regler i Nätverkssäkerhetsgrupper avser [skapa regler i en befintlig NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span><span class="sxs-lookup"><span data-stu-id="84582-120">For more about creating rules in Network Security Groups, refer to [Create rules in an existing NSG](../virtual-network/virtual-networks-create-nsg-arm-pportal.md#create-rules-in-an-existing-nsg).</span></span> <span data-ttu-id="84582-121">När regeln har lagts ska du kunna komma åt CapAnalysis från`http://<PublicIP>:9877`</span><span class="sxs-lookup"><span data-stu-id="84582-121">Once the rule has been successfully added, you should be able to access CapAnalysis from `http://<PublicIP>:9877`</span></span>

### <a name="use-azure-network-watcher-to-start-a-packet-capture-session"></a><span data-ttu-id="84582-122">Använda Azure Nätverksbevakaren för att starta en session för avbildningen av paketet</span><span class="sxs-lookup"><span data-stu-id="84582-122">Use Azure Network Watcher to start a packet capture session</span></span>

<span data-ttu-id="84582-123">Nätverksbevakaren kan du avbilda paket för att spåra trafik till och från en virtuell dator.</span><span class="sxs-lookup"><span data-stu-id="84582-123">Network Watcher allows you to capture packets to track traffic in and out of a virtual machine.</span></span> <span data-ttu-id="84582-124">Du kan se anvisningarna i [hantera paket som samlar in med Nätverksbevakaren](network-watcher-packet-capture-manage-portal.md) starta en session för avbildningen av paketet.</span><span class="sxs-lookup"><span data-stu-id="84582-124">You can refer to the instructions at [Manage packet captures with Network Watcher](network-watcher-packet-capture-manage-portal.md) to start a packet capture session.</span></span> <span data-ttu-id="84582-125">Den här paketinsamling kan lagras i ett blob storage som kan nås av CapAnalysis.</span><span class="sxs-lookup"><span data-stu-id="84582-125">This packet capture can be stored in a storage blob to be accessed by CapAnalysis.</span></span>

### <a name="upload-a-packet-capture-to-capanalysis"></a><span data-ttu-id="84582-126">Ladda upp en paketinsamling CapAnalysis</span><span class="sxs-lookup"><span data-stu-id="84582-126">Upload a packet capture to CapAnalysis</span></span>
<span data-ttu-id="84582-127">Du kan ladda upp en paketinsamling vidtagit nätverksbevakaren på fliken ”importera från URL”, och tillhandahålla en länk till blob storage där paketinsamling lagras direkt.</span><span class="sxs-lookup"><span data-stu-id="84582-127">You can directly upload a packet capture taken by network watcher using the “Import from URL” tab and providing a link to the storage blob where the packet capture is stored.</span></span>

<span data-ttu-id="84582-128">När du anger en länk till CapAnalysis, se till att lägga till en SAS-token till blob storage-URL.</span><span class="sxs-lookup"><span data-stu-id="84582-128">When providing a link to CapAnalysis, make sure to append a SAS token to the storage blob URL.</span></span>  <span data-ttu-id="84582-129">För att göra detta, navigera till signatur för delad åtkomst från lagringskontot, ange tillåtna behörigheter och tryck på knappen Generera en SAS för att skapa ett token.</span><span class="sxs-lookup"><span data-stu-id="84582-129">To do this, navigate to Shared access signature from the storage account, designate the allowed permissions, and press the Generate SAS button to create a token.</span></span> <span data-ttu-id="84582-130">Du kan sedan lägga till SAS-token till URL: en för paketet avbilda storage blob.</span><span class="sxs-lookup"><span data-stu-id="84582-130">You can then append this SAS token to the packet capture storage blob URL.</span></span>

<span data-ttu-id="84582-131">Den resulterande URL: en kommer att se ut ungefär så här: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span><span class="sxs-lookup"><span data-stu-id="84582-131">The resulting URL will look something like this: http://storageaccount.blob.core.windows.net/container/location?addSASkeyhere</span></span>


### <a name="analyzing-packet-captures"></a><span data-ttu-id="84582-132">När paketet avbildas</span><span class="sxs-lookup"><span data-stu-id="84582-132">Analyzing packet captures</span></span>

<span data-ttu-id="84582-133">CapAnalysis erbjuder olika alternativ för att visualisera dina paketinsamling, varje med analys från olika perspektiv.</span><span class="sxs-lookup"><span data-stu-id="84582-133">CapAnalysis offers various options to visualize your packet capture, each providing analysis from a different perspective.</span></span> <span data-ttu-id="84582-134">Med dessa visual sammanfattningar du förstå trenderna nätverket trafik och snabbt hitta några ovanliga aktiviteten.</span><span class="sxs-lookup"><span data-stu-id="84582-134">With these visual summaries, you can understand your network traffic trends and quickly spot any unusual activity.</span></span> <span data-ttu-id="84582-135">Några av dessa funktioner visas i listan nedan:</span><span class="sxs-lookup"><span data-stu-id="84582-135">A few of these features are shown in the following list:</span></span>

1. <span data-ttu-id="84582-136">Flödet tabeller</span><span class="sxs-lookup"><span data-stu-id="84582-136">Flow Tables</span></span>

    <span data-ttu-id="84582-137">Den här tabellen innehåller listan över flöden i datapaketet, tidsstämpeln som är associerade med flödena och olika protokoll som är kopplade till flödet, samt källa och mål-IP.</span><span class="sxs-lookup"><span data-stu-id="84582-137">This table gives you the list of flows in the packet data, the time stamp associated with the flows and the various protocols associated with the flow, as well as source and destination IP.</span></span>

    ![capanalysis flödar sida][5]

1. <span data-ttu-id="84582-139">Översikt över Protocol</span><span class="sxs-lookup"><span data-stu-id="84582-139">Protocol Overview</span></span>

    <span data-ttu-id="84582-140">Det här fönstret kan du snabbt se distributionen av nätverkstrafik via olika protokoll och geografiska områden.</span><span class="sxs-lookup"><span data-stu-id="84582-140">This pane allows you to quickly see the distribution of network traffic over the various protocols and geographies.</span></span>

    ![Översikt över capanalysis protocol][6]

1. <span data-ttu-id="84582-142">Statistik</span><span class="sxs-lookup"><span data-stu-id="84582-142">Statistics</span></span>

    <span data-ttu-id="84582-143">Det här fönstret kan du visa trafik nätverksstatistik – byte skickas och tas emot från käll- och IP-adresser, flöden för varje käll- och IP-adresser, protokoll som används för olika flödena och varaktighet för flöden.</span><span class="sxs-lookup"><span data-stu-id="84582-143">This pane allows you to view network traffic statistics – bytes sent and received from source and destination IPs, flows for each of the source and destination IPs, protocol used for various flows, and the duration of flows.</span></span>

    ![capanalysis statistik][7]

1. <span data-ttu-id="84582-145">geomap</span><span class="sxs-lookup"><span data-stu-id="84582-145">Geomap</span></span>

    <span data-ttu-id="84582-146">Det här fönstret ger dig en kartvyn av nätverkstrafiken, med färger skalning till mängden trafik från varje land.</span><span class="sxs-lookup"><span data-stu-id="84582-146">This pane provides you with a map view of your network traffic, with colors scaling to the volume of traffic from each country.</span></span> <span data-ttu-id="84582-147">Du kan välja markerade länder att visa ytterligare Flödesstatistik som del av data som skickas och tas emot från IP-adresser i det landet.</span><span class="sxs-lookup"><span data-stu-id="84582-147">You can select highlighted countries to view additional flow statistics such as the proportion of data sent and received from IPs in that country.</span></span>

    ![geomap][8]

1. <span data-ttu-id="84582-149">Filter</span><span class="sxs-lookup"><span data-stu-id="84582-149">Filters</span></span>

    <span data-ttu-id="84582-150">CapAnalysis innehåller en uppsättning filter för snabb analys av specifika paket.</span><span class="sxs-lookup"><span data-stu-id="84582-150">CapAnalysis provides a set of filters for quick analysis of specific packets.</span></span> <span data-ttu-id="84582-151">Exempelvis kan du filtrera informationen efter protokoll för att få specifik insikter som delmängd av trafik.</span><span class="sxs-lookup"><span data-stu-id="84582-151">For example, you can choose to filter the data by protocol to gain specific insights on that subset of traffic.</span></span>

    ![filter][11]

    <span data-ttu-id="84582-153">Besök [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) till mer information om alla CapAnalysis funktioner.</span><span class="sxs-lookup"><span data-stu-id="84582-153">Visit [https://www.capanalysis.net/ca/#about](https://www.capanalysis.net/ca/#about) to learn more about all CapAnalysis' capabilities.</span></span>

## <a name="conclusion"></a><span data-ttu-id="84582-154">Slutsats</span><span class="sxs-lookup"><span data-stu-id="84582-154">Conclusion</span></span>

<span data-ttu-id="84582-155">Nätverket Watcher paket avbilda funktionen kan du samla in uppgifter som behövs för att utföra nätverket dataforensik och bättre förstå trafik på nätverket.</span><span class="sxs-lookup"><span data-stu-id="84582-155">Network Watcher’s packet capture feature allows you to capture the data necessary to perform network forensics and better understand your network traffic.</span></span> <span data-ttu-id="84582-156">I det här scenariot visade hur paket insamlingar från Nätverksbevakaren enkelt kan integreras med öppen källkod visualiseringsverktyg.</span><span class="sxs-lookup"><span data-stu-id="84582-156">In this scenario, we showed how packet captures from Network Watcher can easily be integrated with open source visualization tools.</span></span> <span data-ttu-id="84582-157">Med öppen källkod, till exempel CapAnalysis visualisera insamlingar av paket kan du utföra djup paketinspektion och snabbt identifiera trender i nätverkstrafiken.</span><span class="sxs-lookup"><span data-stu-id="84582-157">By using open source tools such as CapAnalysis to visualize packets captures, you can perform deep packet inspection and quickly identify trends within your network traffic.</span></span>

## <a name="next-steps"></a><span data-ttu-id="84582-158">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="84582-158">Next steps</span></span>

<span data-ttu-id="84582-159">Mer information om flödet NSG-loggar finns [NSG flödet loggar](network-watcher-nsg-flow-logging-overview.md)</span><span class="sxs-lookup"><span data-stu-id="84582-159">To learn more about NSG flow logs, visit [NSG Flow logs](network-watcher-nsg-flow-logging-overview.md)</span></span>

<span data-ttu-id="84582-160">Lär dig hur du visualisera dina NSG flödet loggar med Power BI genom att besöka [visualisera NSG flödar loggar med Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span><span class="sxs-lookup"><span data-stu-id="84582-160">Learn how to visualize your NSG flow logs with Power BI by visiting [Visualize NSG flows logs with Power BI](network-watcher-visualize-nsg-flow-logs-power-bi.md)</span></span>
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
