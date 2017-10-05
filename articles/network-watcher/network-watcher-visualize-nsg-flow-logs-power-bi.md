---
title: "Nätverkssäkerhetsgruppen för visualizing Azure flödet loggar med Power BI | Microsoft Docs"
description: "Den här sidan beskrivs visualisera NSG flödet loggar med Power BI."
services: network-watcher
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
ms.assetid: 1e4f95fa-f5f0-4e03-bc25-008fbfc4934c
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: gwallace
ms.openlocfilehash: d8c61ca2a3bd5affe02e8f9500655db6699a245f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a><span data-ttu-id="6ed04-103">Visualizing Nätverkssäkerhetsgruppen flöde loggar med Power BI</span><span class="sxs-lookup"><span data-stu-id="6ed04-103">Visualizing Network Security Group flow logs with Power BI</span></span>

<span data-ttu-id="6ed04-104">Nätverkssäkerhetsgruppen flöde loggar kan du visa information om ingående och utgående IP-trafik på Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="6ed04-104">Network Security Group flow logs allow you to view information about ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="6ed04-105">Dessa trafikflödet loggar Visa utgående och inkommande flöden på grundval av per regel, NIC flödet gäller, 5-tuppel information om flödet (källan/målet IP, källan/målet Port Protocol), och om trafiken tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="6ed04-105">These flow logs show outbound and inbound flows on a per rule basis, the NIC the flow applies to, 5-tuple information about the flow (Source/Destination IP, Source/Destination Port, Protocol), and if the traffic was allowed or denied.</span></span>

<span data-ttu-id="6ed04-106">Det kan vara svårt att få insikter om flödet loggningsdata genom att söka i loggfiler manuellt.</span><span class="sxs-lookup"><span data-stu-id="6ed04-106">It can be difficult to gain insights into flow logging data by manually searching the log files.</span></span> <span data-ttu-id="6ed04-107">I den här artikeln får en lösning för att visualisera dina senaste flödet loggar och lär dig mer om trafik i nätverket.</span><span class="sxs-lookup"><span data-stu-id="6ed04-107">In this article, we provide a solution to visualize your most recent flow logs and learn about traffic on your network.</span></span>

## <a name="scenario"></a><span data-ttu-id="6ed04-108">Scenario</span><span class="sxs-lookup"><span data-stu-id="6ed04-108">Scenario</span></span>

<span data-ttu-id="6ed04-109">I följande scenario ansluta vi Power BI desktop till lagringskontot som vi har konfigurerats som sink för våra data för NSG flöda loggning.</span><span class="sxs-lookup"><span data-stu-id="6ed04-109">In the following scenario, we connect Power BI desktop to the storage account we have configured as the sink for our NSG Flow Logging data.</span></span> <span data-ttu-id="6ed04-110">När vi har anslutit till vår lagringskontot Power BI hämtar och Parsar loggarna för att ge en bild av trafiken som loggas av nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="6ed04-110">After we connect to our storage account, Power BI downloads and parses the logs to provide a visual representation of the traffic that is logged by Network Security groups.</span></span>

<span data-ttu-id="6ed04-111">Använda den visuella informationen som anges i mallen som du kan undersöka:</span><span class="sxs-lookup"><span data-stu-id="6ed04-111">Using the visuals supplied in the template you can examine:</span></span>

* <span data-ttu-id="6ed04-112">Övre Talkers</span><span class="sxs-lookup"><span data-stu-id="6ed04-112">Top Talkers</span></span>
* <span data-ttu-id="6ed04-113">Serien flöda tidsdata genom riktning och regeln beslut</span><span class="sxs-lookup"><span data-stu-id="6ed04-113">Time Series Flow Data by direction and rule decision</span></span>
* <span data-ttu-id="6ed04-114">Flöden av Network Interface MAC-adress</span><span class="sxs-lookup"><span data-stu-id="6ed04-114">Flows by Network Interface MAC address</span></span>
* <span data-ttu-id="6ed04-115">Flöden av NSG och regel</span><span class="sxs-lookup"><span data-stu-id="6ed04-115">Flows by NSG and Rule</span></span>
* <span data-ttu-id="6ed04-116">Flöden av målport</span><span class="sxs-lookup"><span data-stu-id="6ed04-116">Flows by Destination Port</span></span>

<span data-ttu-id="6ed04-117">Den mall som kan redigeras så att du kan ändra den och lägga till nya data, visuell information, eller redigera frågor så att de passar dina behov.</span><span class="sxs-lookup"><span data-stu-id="6ed04-117">The template provided is editable so you can modify it to add new data, visuals, or edit queries to suit your needs.</span></span>

## <a name="setup"></a><span data-ttu-id="6ed04-118">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="6ed04-118">Setup</span></span>

<span data-ttu-id="6ed04-119">Du måste ha nätverket grupp flöda säkerhetsloggning aktiverad på en eller flera Nätverkssäkerhetsgrupper i ditt konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="6ed04-119">Before you begin, you must have Network Security Group Flow Logging enabled on one or many Network Security Groups in your account.</span></span> <span data-ttu-id="6ed04-120">Anvisningar om hur du aktiverar Network Security flöda loggar, finns i följande artikel: [introduktion till flödet loggning för Nätverkssäkerhetsgrupper](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="6ed04-120">For instructions on enabling Network Security flow logs, refer to the following article: [Introduction to flow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="6ed04-121">Du måste också ha Power BI Desktop klienten är installerad på din dator och tillräckligt med ledigt utrymme på din dator för att ladda ned och läsa in loggdata som finns i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6ed04-121">You must also have the Power BI Desktop client installed on your machine, and enough free space on your machine to download and load the log data that exists in your storage account.</span></span>

![Visio-diagram][1]

### <a name="steps"></a><span data-ttu-id="6ed04-123">Steg</span><span class="sxs-lookup"><span data-stu-id="6ed04-123">Steps</span></span>

1. <span data-ttu-id="6ed04-124">Ladda ned och öppna följande Power BI-mall i Power BI Desktop Application [nätverk Watcher PowerBI flöde loggar mall](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span><span class="sxs-lookup"><span data-stu-id="6ed04-124">Download and open the following Power BI template in the Power BI Desktop Application [Network Watcher PowerBI flow logs template](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span></span>
1. <span data-ttu-id="6ed04-125">Ange de obligatoriska parametrarna i frågan</span><span class="sxs-lookup"><span data-stu-id="6ed04-125">Enter the required Query parameters</span></span>
    1. <span data-ttu-id="6ed04-126">**StorageAccountName** – anger namnet på lagringskontot som innehåller flödet NSG-loggar som du vill läsa in och visualisera.</span><span class="sxs-lookup"><span data-stu-id="6ed04-126">**StorageAccountName** – Specifies to the name of the storage account containing the NSG flow logs that you would like to load and visualize.</span></span>
    1. <span data-ttu-id="6ed04-127">**NumberOfLogFiles** – anger antal loggfiler som du vill hämta och visualisera i Power BI.</span><span class="sxs-lookup"><span data-stu-id="6ed04-127">**NumberOfLogFiles** – Specifies the number of log files that you would like to download and visualize in Power BI.</span></span> <span data-ttu-id="6ed04-128">Om exempelvis 50 anges 50 senaste loggfilerna.</span><span class="sxs-lookup"><span data-stu-id="6ed04-128">For example, if 50 is specified, the 50 latest log files.</span></span> <span data-ttu-id="6ed04-129">FF har vi 2 NSG: er aktiverad och konfigurerad att skicka NSG flödet loggar till det här kontot och de senaste 25 timmarna loggar kan visas.</span><span class="sxs-lookup"><span data-stu-id="6ed04-129">Ff we have 2 NSGs enabled and configured to send NSG flow logs to this account, then the past 25 hours of logs can be viewed.</span></span>

    ![Power BI main][2]

1. <span data-ttu-id="6ed04-131">Ange åtkomstnyckeln för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="6ed04-131">Enter the Access Key for your storage account.</span></span> <span data-ttu-id="6ed04-132">Du kan hitta giltig åtkomstnycklar genom att navigera till ditt lagringskonto i Azure portal och markera **åtkomstnycklar** på menyn Inställningar.</span><span class="sxs-lookup"><span data-stu-id="6ed04-132">You can find valid access keys by navigating to your storage account in the Azure portal and selecting **Access Keys** from the Settings menu.</span></span> <span data-ttu-id="6ed04-133">Klicka på **Anslut** tillämpa ändringarna.</span><span class="sxs-lookup"><span data-stu-id="6ed04-133">Click **Connect** then apply changes.</span></span>

    ![Snabbtangenter][3]

    ![åtkomstnyckeln 2][4]

4.  <span data-ttu-id="6ed04-136">Loggarna är hämta och analysera och du kan nu använda förskapade visuell information.</span><span class="sxs-lookup"><span data-stu-id="6ed04-136">Your logs are download and parsed and you can now utilize the pre-created visuals.</span></span>

## <a name="understanding-the-visuals"></a><span data-ttu-id="6ed04-137">Förstå den visuella informationen</span><span class="sxs-lookup"><span data-stu-id="6ed04-137">Understanding the visuals</span></span>

<span data-ttu-id="6ed04-138">I mallen är en uppsättning visuell information som gör en uppfattning om loggdata NSG flöda.</span><span class="sxs-lookup"><span data-stu-id="6ed04-138">Provided in the template are a set of visuals that help make sense of the NSG Flow Log data.</span></span> <span data-ttu-id="6ed04-139">Följande bilder visar ett exempel på hur instrumentpanelen ser ut när fylls i med data.</span><span class="sxs-lookup"><span data-stu-id="6ed04-139">The following images show a sample of what the dashboard looks like when populated with data.</span></span> <span data-ttu-id="6ed04-140">Nedan igenom vi varje visual i större detalj</span><span class="sxs-lookup"><span data-stu-id="6ed04-140">Below we examine each visual in greater detail</span></span> 

![powerbi][5]
 
<span data-ttu-id="6ed04-142">Top Talkers visual visas i IP-adresser som har initierat de flesta anslutningar under perioden anges.</span><span class="sxs-lookup"><span data-stu-id="6ed04-142">The Top Talkers visual shows the IPs that have initiated the most connections over the period specified.</span></span> <span data-ttu-id="6ed04-143">Storleken på rutorna motsvarar det relativa antalet anslutningar.</span><span class="sxs-lookup"><span data-stu-id="6ed04-143">The size of the boxes corresponds to the relative number of connections.</span></span> 

![toptalkers][6]

<span data-ttu-id="6ed04-145">Följande tid serien diagram visar antal flöden under perioden.</span><span class="sxs-lookup"><span data-stu-id="6ed04-145">The following time series graphs show the number of flows over the period.</span></span> <span data-ttu-id="6ed04-146">Övre diagrammet segmenterade av flödesriktning och lägst segmenterat genom beslut (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="6ed04-146">The upper graph is segmented by the flow direction, and the lower is segmented by the decision made (allow or deny).</span></span> <span data-ttu-id="6ed04-147">Med den här visuella informationen du undersöka trenderna trafik över tid, och upptäcka eventuella onormala toppar eller neka trafik eller trafiksegmenteringen.</span><span class="sxs-lookup"><span data-stu-id="6ed04-147">With this visual, you can examine your traffic trends over time, and spot any abnormal spikes or decline in traffic or traffic segmentation.</span></span>

![flowsoverperiod][7]

<span data-ttu-id="6ed04-149">Följande diagram visar flöden per nätverksgränssnitt med övre åtskilda med flödesriktning och lägst åtskilda med beslut.</span><span class="sxs-lookup"><span data-stu-id="6ed04-149">The following graphs show the flows per Network interface, with the upper segmented by flow direction and the lower segmented by decision made.</span></span> <span data-ttu-id="6ed04-150">Med den här informationen kan du få insikter om vilka av dina virtuella datorer har kommunicerat mest i förhållande till andra och om trafik till en specifik VM som tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="6ed04-150">With this information, you can gain insights into which of your VMs communicated the most relative to others, and if traffic to a specific VM is being allowed or denied.</span></span>

![flowspernic][8]

<span data-ttu-id="6ed04-152">Följande ringen hjul diagram visar en sammanfattning av flöden av målport.</span><span class="sxs-lookup"><span data-stu-id="6ed04-152">The following donut wheel chart shows a breakdown of Flows by Destination Port.</span></span> <span data-ttu-id="6ed04-153">Med den här informationen kan visa du de vanligaste målportar som används inom den angivna tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="6ed04-153">With this information, you can view the most commonly used destination ports used within the specified period.</span></span>

![Ring][9]

<span data-ttu-id="6ed04-155">Följande stapeldiagram visar flödet av NSG och regeln.</span><span class="sxs-lookup"><span data-stu-id="6ed04-155">The following bar chart shows the Flow by NSG and Rule.</span></span> <span data-ttu-id="6ed04-156">Med denna information kan se du ansvarar för de flesta trafiken och fördelningen av trafiken NSG: er på en NSG av regeln.</span><span class="sxs-lookup"><span data-stu-id="6ed04-156">With this information, you can see the NSGs responsible for the most traffic, and the breakdown of traffic on an NSG by rule.</span></span>

![barchart][10]
 
<span data-ttu-id="6ed04-158">Följande information diagram visar information om NSG: er finns i loggarna, Antal flöden som avbildas under tid och datum loggens tidigaste avbildas.</span><span class="sxs-lookup"><span data-stu-id="6ed04-158">The following informational charts display information about the NSGs present in the logs, the number of Flows captured over the period, and the date of the earliest log captured.</span></span> <span data-ttu-id="6ed04-159">Den här informationen ger dig en uppfattning om vilken NSG: er är loggas och datumintervallet för flöden.</span><span class="sxs-lookup"><span data-stu-id="6ed04-159">This information gives you an idea of what NSGs are being logged and the date range of flows.</span></span>

![infochart1][11]

![infochart2][12]

<span data-ttu-id="6ed04-162">Den här mallen innehåller följande utsnitt så att du kan visa endast de data du är mest intresserad av.</span><span class="sxs-lookup"><span data-stu-id="6ed04-162">This template includes the following slicers to allow you to view only the data you are most interested in.</span></span> <span data-ttu-id="6ed04-163">Du kan filtrera på resursgrupper, NSG: er och regler.</span><span class="sxs-lookup"><span data-stu-id="6ed04-163">You can filter on your resource groups, NSGs, and rules.</span></span> <span data-ttu-id="6ed04-164">Du kan också filtrera på 5-tuppel information beslut och den tid som loggen har skrivits.</span><span class="sxs-lookup"><span data-stu-id="6ed04-164">You can also filter on 5-tuple information, decision, and the time the log was written.</span></span>

![utsnitt][13]

## <a name="conclusion"></a><span data-ttu-id="6ed04-166">Slutsats</span><span class="sxs-lookup"><span data-stu-id="6ed04-166">Conclusion</span></span>

<span data-ttu-id="6ed04-167">Beskrevs i det här scenariot att med hjälp av Network Security Group flöda loggarna som tillhandahålls av Nätverksbevakaren och Power BI, kommer du att visualisera och förstå trafiken.</span><span class="sxs-lookup"><span data-stu-id="6ed04-167">We showed in this scenario that by using Network Security Group Flow logs provided by Network Watcher and Power BI, we are able to visualize and understand the traffic.</span></span> <span data-ttu-id="6ed04-168">Med den angivna mallen Power BI hämtar loggarna direkt från lagring och bearbetar dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="6ed04-168">Using the provided template, Power BI downloads the logs directly from storage and processes them locally.</span></span> <span data-ttu-id="6ed04-169">Åtgången tid för att läsa in mallen varierar beroende på antalet filer som begärs och totala storleken på filerna hämtas.</span><span class="sxs-lookup"><span data-stu-id="6ed04-169">Time taken to load the template varies depending on the number of files requested and total size of files downloaded.</span></span>

<span data-ttu-id="6ed04-170">Du kan anpassa den här mallen för dina behov.</span><span class="sxs-lookup"><span data-stu-id="6ed04-170">Feel free to customize this template for your needs.</span></span> <span data-ttu-id="6ed04-171">Det finns många flera olika sätt som du kan använda Power BI med Network Security Group flöda loggar.</span><span class="sxs-lookup"><span data-stu-id="6ed04-171">There are many numerous ways that you can use Power BI with Network Security Group Flow Logs.</span></span> 

## <a name="notes"></a><span data-ttu-id="6ed04-172">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="6ed04-172">Notes</span></span>

* <span data-ttu-id="6ed04-173">Loggar som standard lagras i`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span><span class="sxs-lookup"><span data-stu-id="6ed04-173">Logs by default are stored in `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span></span>

    * <span data-ttu-id="6ed04-174">Om det finns andra data i en annan katalog måste de frågor till pull och bearbeta data ändras.</span><span class="sxs-lookup"><span data-stu-id="6ed04-174">If other data exists in another directory they the queries to pull and process the data must be modified.</span></span>

* <span data-ttu-id="6ed04-175">Den angivna mallen rekommenderas inte för användning med mer än 1 GB loggar.</span><span class="sxs-lookup"><span data-stu-id="6ed04-175">The provided template is not recommended for use with more than 1 GB of logs.</span></span>

* <span data-ttu-id="6ed04-176">Om du har en stor mängd loggar rekommenderar vi att du undersöker en lösning med hjälp av ett annat datalager som Data Lake eller SQL server.</span><span class="sxs-lookup"><span data-stu-id="6ed04-176">If you have a large amount of logs, we recommend that you investigate a solution using another data store like Data Lake or SQL server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="6ed04-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="6ed04-177">Next Steps</span></span>

<span data-ttu-id="6ed04-178">Lär dig hur du visualisera dina NSG flödet loggar med Elastick-stacken genom att besöka [visualisera nätverket trafikmönster till och från dina virtuella datorer med hjälp av verktyg för öppen källkod](network-watcher-using-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="6ed04-178">Learn how to visualize your NSG flow logs with the Elastick Stack by visiting [Visualize network traffic patterns to and from your VMs using open source tools](network-watcher-using-open-source-tools.md)</span></span>

[1]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure1.png
[2]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure2.png
[3]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure3.png
[4]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure4.png
[5]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure5.png
[6]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure6.png
[7]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure7.png
[8]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure8.png
[9]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure9.png
[10]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure10.png
[11]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure11.png
[12]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure12.png
[13]: ./media/network-watcher-visualize-nsg-flow-logs-power-bi/figure13.png
