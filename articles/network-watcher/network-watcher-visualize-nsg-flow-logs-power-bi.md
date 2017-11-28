---
title: "aaaVisualizing Azure Network Security Group flödet loggar med Power BI | Microsoft Docs"
description: "Den här sidan beskrivs hur toovisualize NSG flödet loggar med Power BI."
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
ms.openlocfilehash: d9adcf256df8fed68c39be1a026ca64cc6b5c6d5
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="visualizing-network-security-group-flow-logs-with-power-bi"></a><span data-ttu-id="b0ebd-103">Visualizing Nätverkssäkerhetsgruppen flöde loggar med Power BI</span><span class="sxs-lookup"><span data-stu-id="b0ebd-103">Visualizing Network Security Group flow logs with Power BI</span></span>

<span data-ttu-id="b0ebd-104">Nätverkssäkerhetsgruppen flöde loggar Tillåt tooview information om ingående och utgående IP-trafik på Nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-104">Network Security Group flow logs allow you tooview information about ingress and egress IP traffic on Network Security Groups.</span></span> <span data-ttu-id="b0ebd-105">Loggarna flödet visar utgående och inkommande flöden på grundval av per regel hello NIC hello flödet gäller för 5-tuppel information om hello flödet (källan/målet IP, källan/målet Port Protocol), och om hello trafik tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-105">These flow logs show outbound and inbound flows on a per rule basis, hello NIC hello flow applies to, 5-tuple information about hello flow (Source/Destination IP, Source/Destination Port, Protocol), and if hello traffic was allowed or denied.</span></span>

<span data-ttu-id="b0ebd-106">Det kan vara svårt toogain insikter om flödet loggningsdata genom att söka manuellt hello loggfiler.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-106">It can be difficult toogain insights into flow logging data by manually searching hello log files.</span></span> <span data-ttu-id="b0ebd-107">I den här artikeln får en lösning toovisualize din senaste flödet loggar och lär dig mer om trafik i nätverket.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-107">In this article, we provide a solution toovisualize your most recent flow logs and learn about traffic on your network.</span></span>

## <a name="scenario"></a><span data-ttu-id="b0ebd-108">Scenario</span><span class="sxs-lookup"><span data-stu-id="b0ebd-108">Scenario</span></span>

<span data-ttu-id="b0ebd-109">I följande scenario hello, ansluta vi Power BI desktop toohello storage-konto som vi har konfigurerats som hello sink för våra data för NSG flöda loggning.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-109">In hello following scenario, we connect Power BI desktop toohello storage account we have configured as hello sink for our NSG Flow Logging data.</span></span> <span data-ttu-id="b0ebd-110">När vi har anslutit tooour storage-konto Power BI hämtar och analyserar hello loggar tooprovide en bild av hello trafik som loggats av nätverkssäkerhetsgrupper.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-110">After we connect tooour storage account, Power BI downloads and parses hello logs tooprovide a visual representation of hello traffic that is logged by Network Security groups.</span></span>

<span data-ttu-id="b0ebd-111">Använda hello visuell information tillhandahålls i hello-mallen som du kan undersöka:</span><span class="sxs-lookup"><span data-stu-id="b0ebd-111">Using hello visuals supplied in hello template you can examine:</span></span>

* <span data-ttu-id="b0ebd-112">Övre Talkers</span><span class="sxs-lookup"><span data-stu-id="b0ebd-112">Top Talkers</span></span>
* <span data-ttu-id="b0ebd-113">Serien flöda tidsdata genom riktning och regeln beslut</span><span class="sxs-lookup"><span data-stu-id="b0ebd-113">Time Series Flow Data by direction and rule decision</span></span>
* <span data-ttu-id="b0ebd-114">Flöden av Network Interface MAC-adress</span><span class="sxs-lookup"><span data-stu-id="b0ebd-114">Flows by Network Interface MAC address</span></span>
* <span data-ttu-id="b0ebd-115">Flöden av NSG och regel</span><span class="sxs-lookup"><span data-stu-id="b0ebd-115">Flows by NSG and Rule</span></span>
* <span data-ttu-id="b0ebd-116">Flöden av målport</span><span class="sxs-lookup"><span data-stu-id="b0ebd-116">Flows by Destination Port</span></span>

<span data-ttu-id="b0ebd-117">hello-mall kan redigeras så att du kan ändra den tooadd nya data, visuell information, eller redigera frågor toosuit dina behov.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-117">hello template provided is editable so you can modify it tooadd new data, visuals, or edit queries toosuit your needs.</span></span>

## <a name="setup"></a><span data-ttu-id="b0ebd-118">Konfiguration</span><span class="sxs-lookup"><span data-stu-id="b0ebd-118">Setup</span></span>

<span data-ttu-id="b0ebd-119">Du måste ha nätverket grupp flöda säkerhetsloggning aktiverad på en eller flera Nätverkssäkerhetsgrupper i ditt konto innan du börjar.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-119">Before you begin, you must have Network Security Group Flow Logging enabled on one or many Network Security Groups in your account.</span></span> <span data-ttu-id="b0ebd-120">Anvisningar om hur du aktiverar Network Security flöda loggar finns i följande artikel toohello: [introduktion tooflow loggning för Nätverkssäkerhetsgrupper](network-watcher-nsg-flow-logging-overview.md).</span><span class="sxs-lookup"><span data-stu-id="b0ebd-120">For instructions on enabling Network Security flow logs, refer toohello following article: [Introduction tooflow logging for Network Security Groups](network-watcher-nsg-flow-logging-overview.md).</span></span>

<span data-ttu-id="b0ebd-121">Du måste också ha hello Power BI Desktop-klienten är installerad på datorn och tillräckligt med ledigt utrymme på din dator toodownload och Läs in hello loggdata som finns i ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-121">You must also have hello Power BI Desktop client installed on your machine, and enough free space on your machine toodownload and load hello log data that exists in your storage account.</span></span>

![Visio-diagram][1]

### <a name="steps"></a><span data-ttu-id="b0ebd-123">Steg</span><span class="sxs-lookup"><span data-stu-id="b0ebd-123">Steps</span></span>

1. <span data-ttu-id="b0ebd-124">Ladda ned och öppna hello följande Power BI-mallen i hello Power BI Desktop Application [nätverk Watcher PowerBI flöde loggar mall](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span><span class="sxs-lookup"><span data-stu-id="b0ebd-124">Download and open hello following Power BI template in hello Power BI Desktop Application [Network Watcher PowerBI flow logs template](https://aka.ms/networkwatcherpowerbiflowlogstemplate)</span></span>
1. <span data-ttu-id="b0ebd-125">Ange frågeparametrar Hej krävs</span><span class="sxs-lookup"><span data-stu-id="b0ebd-125">Enter hello required Query parameters</span></span>
    1. <span data-ttu-id="b0ebd-126">**StorageAccountName** – anger toohello namnet på hello storage-konto som innehåller hello NSG flödet loggar som du skulle t.ex tooload och visualisera.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-126">**StorageAccountName** – Specifies toohello name of hello storage account containing hello NSG flow logs that you would like tooload and visualize.</span></span>
    1. <span data-ttu-id="b0ebd-127">**NumberOfLogFiles** – anger hello antal loggfiler som du skulle t.ex. toodownload och visualisera i Power BI.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-127">**NumberOfLogFiles** – Specifies hello number of log files that you would like toodownload and visualize in Power BI.</span></span> <span data-ttu-id="b0ebd-128">Till exempel om 50 anges hello 50 senaste loggfiler.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-128">For example, if 50 is specified, hello 50 latest log files.</span></span> <span data-ttu-id="b0ebd-129">FF som vi har 2 NSG: er som är aktiverad och konfigurerad toosend NSG flödet loggar toothis konto och sedan hello senaste 25 timmarna loggar kan visas.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-129">Ff we have 2 NSGs enabled and configured toosend NSG flow logs toothis account, then hello past 25 hours of logs can be viewed.</span></span>

    ![Power BI main][2]

1. <span data-ttu-id="b0ebd-131">Ange hello åtkomstnyckeln för ditt lagringskonto.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-131">Enter hello Access Key for your storage account.</span></span> <span data-ttu-id="b0ebd-132">Du kan hitta giltig åtkomstnycklar genom att gå tooyour storage-konto i hello Azure portal och markera **åtkomstnycklar** hello inställningsmenyn.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-132">You can find valid access keys by navigating tooyour storage account in hello Azure portal and selecting **Access Keys** from hello Settings menu.</span></span> <span data-ttu-id="b0ebd-133">Klicka på **Anslut** tillämpa ändringarna.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-133">Click **Connect** then apply changes.</span></span>

    ![Snabbtangenter][3]

    ![åtkomstnyckeln 2][4]

4.  <span data-ttu-id="b0ebd-136">Loggarna är hämta och analysera och du kan nu använda hello förskapade visuell information.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-136">Your logs are download and parsed and you can now utilize hello pre-created visuals.</span></span>

## <a name="understanding-hello-visuals"></a><span data-ttu-id="b0ebd-137">Förstå hello visuell information</span><span class="sxs-lookup"><span data-stu-id="b0ebd-137">Understanding hello visuals</span></span>

<span data-ttu-id="b0ebd-138">Förutsatt att i hello mallen är en uppsättning visuell information som hjälper passar för hello NSG flöda loggdata.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-138">Provided in hello template are a set of visuals that help make sense of hello NSG Flow Log data.</span></span> <span data-ttu-id="b0ebd-139">hello visar följande bilder ett exempel på vilka hello instrumentpanel ser ut när fylls i med data.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-139">hello following images show a sample of what hello dashboard looks like when populated with data.</span></span> <span data-ttu-id="b0ebd-140">Nedan igenom vi varje visual i större detalj</span><span class="sxs-lookup"><span data-stu-id="b0ebd-140">Below we examine each visual in greater detail</span></span> 

![powerbi][5]
 
<span data-ttu-id="b0ebd-142">hello upp Talkers visual visar hello IP-adresser som har initierat hello de flesta anslutningar över hello tidsperioden.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-142">hello Top Talkers visual shows hello IPs that have initiated hello most connections over hello period specified.</span></span> <span data-ttu-id="b0ebd-143">hello storleken på hello rutor motsvarar toohello relativa antalet anslutningar.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-143">hello size of hello boxes corresponds toohello relative number of connections.</span></span> 

![toptalkers][6]

<span data-ttu-id="b0ebd-145">hello visar följande tid serien hello Antal flöden under hello period.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-145">hello following time series graphs show hello number of flows over hello period.</span></span> <span data-ttu-id="b0ebd-146">hello övre diagrammet åtskilda med hello flödesriktning och hello lägre åtskilda med hello beslut om (Tillåt eller neka).</span><span class="sxs-lookup"><span data-stu-id="b0ebd-146">hello upper graph is segmented by hello flow direction, and hello lower is segmented by hello decision made (allow or deny).</span></span> <span data-ttu-id="b0ebd-147">Med den här visuella informationen du undersöka trenderna trafik över tid, och upptäcka eventuella onormala toppar eller neka trafik eller trafiksegmenteringen.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-147">With this visual, you can examine your traffic trends over time, and spot any abnormal spikes or decline in traffic or traffic segmentation.</span></span>

![flowsoverperiod][7]

<span data-ttu-id="b0ebd-149">hello visar följande diagram hello flöden per nätverksgränssnitt med hello övre åtskilda med flödesriktning och hello lägre åtskilda med beslut.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-149">hello following graphs show hello flows per Network interface, with hello upper segmented by flow direction and hello lower segmented by decision made.</span></span> <span data-ttu-id="b0ebd-150">Med den här informationen kan du få insikter om vilka av dina virtuella datorer kommunicerat hello de flesta relativa tooothers och om trafik tooa specifik VM används tillåts eller nekas.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-150">With this information, you can gain insights into which of your VMs communicated hello most relative tooothers, and if traffic tooa specific VM is being allowed or denied.</span></span>

![flowspernic][8]

<span data-ttu-id="b0ebd-152">hello följande hjul ringdiagram visar en sammanfattning av flöden av målport.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-152">hello following donut wheel chart shows a breakdown of Flows by Destination Port.</span></span> <span data-ttu-id="b0ebd-153">Med den här informationen kan du visa hello vanligaste målportar används i hello angivna perioden.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-153">With this information, you can view hello most commonly used destination ports used within hello specified period.</span></span>

![Ring][9]

<span data-ttu-id="b0ebd-155">hello visar följande stapeldiagram hello flödet av NSG och regeln.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-155">hello following bar chart shows hello Flow by NSG and Rule.</span></span> <span data-ttu-id="b0ebd-156">Med den här informationen visas hello NSG: er som är ansvarig för hello de flesta trafik och hello fördelning av trafik på en NSG av regeln.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-156">With this information, you can see hello NSGs responsible for hello most traffic, and hello breakdown of traffic on an NSG by rule.</span></span>

![barchart][10]
 
<span data-ttu-id="b0ebd-158">hello följande information diagram visar information om hello NSG: er finns i hello loggar kan hello Antal flöden som avbildas under hello tid och hello datum hello tidigaste loggen avbildas.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-158">hello following informational charts display information about hello NSGs present in hello logs, hello number of Flows captured over hello period, and hello date of hello earliest log captured.</span></span> <span data-ttu-id="b0ebd-159">Den här informationen ger dig en uppfattning om vilken NSG: er loggas och hello datumintervall för flöden.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-159">This information gives you an idea of what NSGs are being logged and hello date range of flows.</span></span>

![infochart1][11]

![infochart2][12]

<span data-ttu-id="b0ebd-162">Den här mallen innehåller hello följande utsnitt tooallow du tooview endast hello data som du är mest intresserad av.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-162">This template includes hello following slicers tooallow you tooview only hello data you are most interested in.</span></span> <span data-ttu-id="b0ebd-163">Du kan filtrera på resursgrupper, NSG: er och regler.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-163">You can filter on your resource groups, NSGs, and rules.</span></span> <span data-ttu-id="b0ebd-164">Du kan också filtrera på 5-tuppel information och beslut hello tid hello loggen har skrivits.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-164">You can also filter on 5-tuple information, decision, and hello time hello log was written.</span></span>

![utsnitt][13]

## <a name="conclusion"></a><span data-ttu-id="b0ebd-166">Slutsats</span><span class="sxs-lookup"><span data-stu-id="b0ebd-166">Conclusion</span></span>

<span data-ttu-id="b0ebd-167">Beskrevs i det här scenariot att genom att använda Network Security Group flöda loggarna som tillhandahålls av Nätverksbevakaren och Power BI kan vi är kan toovisualize och förstå hello trafik.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-167">We showed in this scenario that by using Network Security Group Flow logs provided by Network Watcher and Power BI, we are able toovisualize and understand hello traffic.</span></span> <span data-ttu-id="b0ebd-168">Använd hello som mall Power BI hämtar hello loggarna direkt från lagring och bearbetar dem lokalt.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-168">Using hello provided template, Power BI downloads hello logs directly from storage and processes them locally.</span></span> <span data-ttu-id="b0ebd-169">Tidsåtgång tooload hello mallen varierar beroende på hello antalet filer som begärs och totala storleken på filerna hämtas.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-169">Time taken tooload hello template varies depending on hello number of files requested and total size of files downloaded.</span></span>

<span data-ttu-id="b0ebd-170">Känna sig fria toocustomize mallen för dina behov.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-170">Feel free toocustomize this template for your needs.</span></span> <span data-ttu-id="b0ebd-171">Det finns många flera olika sätt som du kan använda Power BI med Network Security Group flöda loggar.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-171">There are many numerous ways that you can use Power BI with Network Security Group Flow Logs.</span></span> 

## <a name="notes"></a><span data-ttu-id="b0ebd-172">Anteckningar</span><span class="sxs-lookup"><span data-stu-id="b0ebd-172">Notes</span></span>

* <span data-ttu-id="b0ebd-173">Loggar som standard lagras i`https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span><span class="sxs-lookup"><span data-stu-id="b0ebd-173">Logs by default are stored in `https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/`</span></span>

    * <span data-ttu-id="b0ebd-174">Om det finns andra data i en annan katalog de hello frågor toopull och bearbeta hello data måste ändras.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-174">If other data exists in another directory they hello queries toopull and process hello data must be modified.</span></span>

* <span data-ttu-id="b0ebd-175">hello tillhandahålls mallen rekommenderas inte för användning med mer än 1 GB loggar.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-175">hello provided template is not recommended for use with more than 1 GB of logs.</span></span>

* <span data-ttu-id="b0ebd-176">Om du har en stor mängd loggar rekommenderar vi att du undersöker en lösning med hjälp av ett annat datalager som Data Lake eller SQL server.</span><span class="sxs-lookup"><span data-stu-id="b0ebd-176">If you have a large amount of logs, we recommend that you investigate a solution using another data store like Data Lake or SQL server.</span></span>

## <a name="next-steps"></a><span data-ttu-id="b0ebd-177">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="b0ebd-177">Next Steps</span></span>

<span data-ttu-id="b0ebd-178">Lär dig hur toovisualize NSG-flöde loggar med hello Elastick-stacken genom att besöka [visualisera nätverket trafik mönster tooand från dina virtuella datorer med hjälp av verktyg för öppen källkod](network-watcher-using-open-source-tools.md)</span><span class="sxs-lookup"><span data-stu-id="b0ebd-178">Learn how toovisualize your NSG flow logs with hello Elastick Stack by visiting [Visualize network traffic patterns tooand from your VMs using open source tools](network-watcher-using-open-source-tools.md)</span></span>

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
