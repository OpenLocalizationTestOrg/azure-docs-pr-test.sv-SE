---
title: "aaaConnected factory Azure IoT Suite-lösningen genomgången | Microsoft Docs"
description: "En beskrivning av hello Azure IoT förkonfigurerade lösningen ansluten fabriken och dess arkitektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.openlocfilehash: 7fd55c51351659401349cfde91a20fce1045b4f0
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a><span data-ttu-id="d2006-103">Genomgång av den förkonfigurerade lösningen Ansluten fabrik</span><span class="sxs-lookup"><span data-stu-id="d2006-103">Connected factory preconfigured solution walkthrough</span></span>

<span data-ttu-id="d2006-104">Hej IoT Suite anslutna factory [förkonfigurerade lösningen] [ lnk-preconfigured-solutions] är en implementering av en slutpunkt till slutpunkt industriella lösning som:</span><span class="sxs-lookup"><span data-stu-id="d2006-104">hello IoT Suite connected factory [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end industrial solution that:</span></span>

* <span data-ttu-id="d2006-105">Ansluter tooboth simulerade industriella enheter som kör OPC UA servrar i simulerade factory produktionsrader och verkliga OPC UA server-enheter.</span><span class="sxs-lookup"><span data-stu-id="d2006-105">Connects tooboth simulated industrial devices running OPC UA servers in simulated factory production lines, and real OPC UA server devices.</span></span> <span data-ttu-id="d2006-106">Mer information om OPC UA finns hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="d2006-106">For more information about OPC UA, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="d2006-107">Visar KPI:er och OEE:er för drift för enheterna och produktionsraderna.</span><span class="sxs-lookup"><span data-stu-id="d2006-107">Shows operational KPIs and OEE of those devices and production lines.</span></span>
* <span data-ttu-id="d2006-108">Visar hur ett molnbaserade program kan använda toointeract med OPC UA serversystem.</span><span class="sxs-lookup"><span data-stu-id="d2006-108">Demonstrates how a cloud-based application could be used toointeract with OPC UA server systems.</span></span>
* <span data-ttu-id="d2006-109">Aktiverar du tooconnect egna OPC UA server-enheter.</span><span class="sxs-lookup"><span data-stu-id="d2006-109">Enables you tooconnect your own OPC UA server devices.</span></span>
* <span data-ttu-id="d2006-110">Aktiverar toobrowse och ändra hello OPC UA serverdata.</span><span class="sxs-lookup"><span data-stu-id="d2006-110">Enables you toobrowse and modify hello OPC UA server data.</span></span>
* <span data-ttu-id="d2006-111">Integreras med hello Azure tid serien insikter (TSD) tjänsten tooprovide anpassade datavyer hello från OPC UA-servrar.</span><span class="sxs-lookup"><span data-stu-id="d2006-111">Integrates with hello Azure Time Series Insights (TSI) service tooprovide customized views of hello data from your OPC UA servers.</span></span>

<span data-ttu-id="d2006-112">Du kan använda hello lösning som en startpunkt för din egen implementering och [anpassa] [ lnk-customize] den toomeet egna specifika krav i företaget.</span><span class="sxs-lookup"><span data-stu-id="d2006-112">You can use hello solution as a starting point for your own implementation and [customize][lnk-customize] it toomeet your own specific business requirements.</span></span>

<span data-ttu-id="d2006-113">Den här artikeln guidar dig igenom några av hello viktiga delar i hello anslutna factory lösning tooenable toounderstand hur det fungerar.</span><span class="sxs-lookup"><span data-stu-id="d2006-113">This article walks you through some of hello key elements of hello connected factory solution tooenable you toounderstand how it works.</span></span> <span data-ttu-id="d2006-114">Med den här kunskapen kan du sedan:</span><span class="sxs-lookup"><span data-stu-id="d2006-114">This knowledge helps you to:</span></span>

* <span data-ttu-id="d2006-115">Felsöka problem i hello-lösning.</span><span class="sxs-lookup"><span data-stu-id="d2006-115">Troubleshoot issues in hello solution.</span></span>
* <span data-ttu-id="d2006-116">Planera hur toocustomize toohello lösning toomeet egna specifika krav.</span><span class="sxs-lookup"><span data-stu-id="d2006-116">Plan how toocustomize toohello solution toomeet your own specific requirements.</span></span>
* <span data-ttu-id="d2006-117">Utforma en egen IoT-lösning som använder Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="d2006-117">Design your own IoT solution that uses Azure services.</span></span>

<span data-ttu-id="d2006-118">Mer information finns i hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="d2006-118">For more information, see hello [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="d2006-119">Logisk arkitektur</span><span class="sxs-lookup"><span data-stu-id="d2006-119">Logical architecture</span></span>

<span data-ttu-id="d2006-120">följande diagram hello beskrivs hello logiska komponenter av hello förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="d2006-120">hello following diagram outlines hello logical components of hello preconfigured solution:</span></span>

![Logisk arkitektur för ansluten fabrik][connected-factory-logical]

## <a name="communication-patterns"></a><span data-ttu-id="d2006-122">Kommunikationsmönster</span><span class="sxs-lookup"><span data-stu-id="d2006-122">Communication patterns</span></span>

<span data-ttu-id="d2006-123">hello lösningen använder hello [OPC UA Pub/Sub-specifikationen](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC UA telemetri data tooIoT hubb i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="d2006-123">hello solution uses hello [OPC UA Pub/Sub specification](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC UA telemetry data tooIoT Hub in JSON format.</span></span> <span data-ttu-id="d2006-124">hello lösningen använder hello [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT kant-modul för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="d2006-124">hello solution uses hello [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge module for this purpose.</span></span>

<span data-ttu-id="d2006-125">hello-lösningen har också en OPC UA-klient som är integrerade i ett program som kan upprätta anslutningar med lokala OPC UA servrar.</span><span class="sxs-lookup"><span data-stu-id="d2006-125">hello solution also has an OPC UA client integrated into a web application that can establish connections with on-premises OPC UA servers.</span></span> <span data-ttu-id="d2006-126">hello-klienten använder en [omvänd proxy](https://wikipedia.org/wiki/Reverse_proxy) och tar emot hjälp från IoT-hubb toomake hello anslutning utan att öppna portar i brandväggen för hello lokalt.</span><span class="sxs-lookup"><span data-stu-id="d2006-126">hello client uses a [reverse-proxy](https://wikipedia.org/wiki/Reverse_proxy) and receives help from IoT Hub toomake hello connection without requiring open ports in hello on-premises firewall.</span></span> <span data-ttu-id="d2006-127">Det här kommunikationsmönstret kallas [tjänstassisterad kommunikation](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span><span class="sxs-lookup"><span data-stu-id="d2006-127">This communication pattern is called [service-assisted communication](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span></span> <span data-ttu-id="d2006-128">hello lösningen använder hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT kant-modul för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="d2006-128">hello solution uses hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge module for this purpose.</span></span>


## <a name="simulation"></a><span data-ttu-id="d2006-129">Simulering</span><span class="sxs-lookup"><span data-stu-id="d2006-129">Simulation</span></span>

<span data-ttu-id="d2006-130">Hej simulerade stationer och hello simulerade Tillverkningskörning system (MES) utgör en fabrik produktion rad.</span><span class="sxs-lookup"><span data-stu-id="d2006-130">hello simulated stations and hello simulated manufacturing execution systems (MES) make up a factory production line.</span></span> <span data-ttu-id="d2006-131">hello simulerade enheterna och hello OPC Publisher modulen baseras på hello [OPC UA .NET Standard] [ lnk-OPC-UA-NET-Standard] publicerats av hello OPC Foundation.</span><span class="sxs-lookup"><span data-stu-id="d2006-131">hello simulated devices and hello OPC Publisher Module are based on hello [OPC UA .NET Standard][lnk-OPC-UA-NET-Standard] published by hello OPC Foundation.</span></span>

<span data-ttu-id="d2006-132">hello OPC-Proxy och OPC utgivare implementeras som moduler baserat på [Azure IoT kant][lnk-Azure-IoT-Gateway].</span><span class="sxs-lookup"><span data-stu-id="d2006-132">hello OPC Proxy and OPC Publisher are implemented as modules based on [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span></span> <span data-ttu-id="d2006-133">Varje simulerad produktionsrad har en särskild gateway ansluten.</span><span class="sxs-lookup"><span data-stu-id="d2006-133">Each simulated production line has a designated gateway attached.</span></span>

<span data-ttu-id="d2006-134">Alla simuleringskomponenter körs i Docker-behållare som finns i en virtuell Azure Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="d2006-134">All simulation components run in Docker containers  hosted in an Azure Linux VM.</span></span> <span data-ttu-id="d2006-135">hello simuleringen är konfigurerade toorun åtta simulerade produktionsrader som standard.</span><span class="sxs-lookup"><span data-stu-id="d2006-135">hello simulation is configured toorun eight simulated production lines by default.</span></span>

## <a name="simulated-production-line"></a><span data-ttu-id="d2006-136">Simulerad produktionsrad</span><span class="sxs-lookup"><span data-stu-id="d2006-136">Simulated production line</span></span>

<span data-ttu-id="d2006-137">En produktionsrad tillverkar delar.</span><span class="sxs-lookup"><span data-stu-id="d2006-137">A production line manufactures parts.</span></span> <span data-ttu-id="d2006-138">Den består av olika stationer: en sammansättningsstation, en teststation och en förpackningsstation.</span><span class="sxs-lookup"><span data-stu-id="d2006-138">It is composed of different stations: an assembly station, a test station, and a packaging station.</span></span>

<span data-ttu-id="d2006-139">hello simuleringen körs och uppdaterar hello data som exponeras via hello OPC UA noder.</span><span class="sxs-lookup"><span data-stu-id="d2006-139">hello simulation runs and updates hello data that is exposed through hello OPC UA nodes.</span></span> <span data-ttu-id="d2006-140">Alla simulerade produktion rad stationer styrd av hello MES via OPC UA.</span><span class="sxs-lookup"><span data-stu-id="d2006-140">All simulated production line stations are orchestrated by hello MES through OPC UA.</span></span>

## <a name="simulated-manufacturing-execution-system"></a><span data-ttu-id="d2006-141">Simulerat körningssystem för tillverkning</span><span class="sxs-lookup"><span data-stu-id="d2006-141">Simulated manufacturing execution system</span></span>

<span data-ttu-id="d2006-142">hello MES övervakar varje station i hello produktion linje genom OPC UA toodetect station status ändras.</span><span class="sxs-lookup"><span data-stu-id="d2006-142">hello MES monitors each station in hello production line through OPC UA toodetect station status changes.</span></span> <span data-ttu-id="d2006-143">Den anropar OPC UA metoder toocontrol hello stationer och överför en produkt från en station toohello bredvid tills den är klar.</span><span class="sxs-lookup"><span data-stu-id="d2006-143">It calls OPC UA methods toocontrol hello stations and passes a product from one station toohello next until it is complete.</span></span>

## <a name="gateway-opc-publisher-module"></a><span data-ttu-id="d2006-144">Gateway OPC Publisher-modul</span><span class="sxs-lookup"><span data-stu-id="d2006-144">Gateway OPC publisher module</span></span>

<span data-ttu-id="d2006-145">OPC Publisher modulen ansluter toohello station OPC UA servrar och prenumererar toohello OPC noder toobe publiceras.</span><span class="sxs-lookup"><span data-stu-id="d2006-145">OPC Publisher Module connects toohello station OPC UA servers and subscribes toohello OPC nodes toobe published.</span></span> <span data-ttu-id="d2006-146">hello modulen konverterar hello noden data till JSON-format, krypteras det och skickar den tooIoT hubb som OPC UA Pub/Sub-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d2006-146">hello module converts hello node data into JSON format, encrypts it, and sends it tooIoT Hub as OPC UA Pub/Sub messages.</span></span>

<span data-ttu-id="d2006-147">hello OPC Publisher modulen endast kräver en utgående https-port (443) och kan arbeta med den befintliga infrastrukturen för företaget.</span><span class="sxs-lookup"><span data-stu-id="d2006-147">hello OPC Publisher module only requires an outbound https port (443) and can work with existing enterprise infrastructure.</span></span>

## <a name="gateway-opc-proxy-module"></a><span data-ttu-id="d2006-148">Gateway OPC Proxy-modul</span><span class="sxs-lookup"><span data-stu-id="d2006-148">Gateway OPC proxy module</span></span>

<span data-ttu-id="d2006-149">hello Gateway OPC UA Proxy modulen tunnlar binära OPC UA kommando- och meddelanden och kräver bara en utgående https-port (443).</span><span class="sxs-lookup"><span data-stu-id="d2006-149">hello Gateway OPC UA Proxy Module tunnels binary OPC UA command and control messages and only requires an outbound https port (443).</span></span> <span data-ttu-id="d2006-150">Den kan fungera med befintlig företagsinfrastruktur, inklusive webbproxyservrar.</span><span class="sxs-lookup"><span data-stu-id="d2006-150">It can work with existing enterprise infrastructure, including Web Proxies.</span></span>

<span data-ttu-id="d2006-151">Den använder IoT Hub-enhet metoder tootransfer packetized TCP/IP-data på programnivå hello och därmed säkerställer endpoint förtroende, datakryptering och dataintegritet med hjälp av SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="d2006-151">It uses IoT Hub Device methods tootransfer packetized TCP/IP data at hello application layer and thus ensures endpoint trust, data encryption, and integrity using SSL/TLS.</span></span>

<span data-ttu-id="d2006-152">hello OPC UA binära protokollet vidarebefordrande via hello proxy själva använder UA autentisering och kryptering.</span><span class="sxs-lookup"><span data-stu-id="d2006-152">hello OPC UA binary protocol relayed through hello proxy itself uses UA authentication and encryption.</span></span>

## <a name="azure-time-series-insights"></a><span data-ttu-id="d2006-153">Azure Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="d2006-153">Azure Time Series Insights</span></span>

<span data-ttu-id="d2006-154">hello Gateway OPC Publisher modulen prenumererar tooOPC UA server noder toodetect ändring i hello datavärden.</span><span class="sxs-lookup"><span data-stu-id="d2006-154">hello Gateway OPC Publisher Module subscribes tooOPC UA server nodes toodetect change in hello data values.</span></span> <span data-ttu-id="d2006-155">Om en dataändring av har identifierats i en av noderna hello, skickar meddelanden tooAzure IoT-hubb sedan i den här modulen.</span><span class="sxs-lookup"><span data-stu-id="d2006-155">If a data change is detected in one of hello nodes, this module then sends messages tooAzure IoT Hub.</span></span>

<span data-ttu-id="d2006-156">IoT-hubb innehåller en händelse källa tooAzure TSD.</span><span class="sxs-lookup"><span data-stu-id="d2006-156">IoT Hub provides an event source tooAzure TSI.</span></span> <span data-ttu-id="d2006-157">TSD lagrar data i 30 dagar, utifrån tidsstämplar anslutna toohello meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d2006-157">TSI stores data for 30 days based on timestamps attached toohello messages.</span></span> <span data-ttu-id="d2006-158">Dessa data omfattar:</span><span class="sxs-lookup"><span data-stu-id="d2006-158">This data includes:</span></span>

* <span data-ttu-id="d2006-159">OPC UA ApplicationUri</span><span class="sxs-lookup"><span data-stu-id="d2006-159">OPC UA ApplicationUri</span></span>
* <span data-ttu-id="d2006-160">OPC UA NodeId</span><span class="sxs-lookup"><span data-stu-id="d2006-160">OPC UA NodeId</span></span>
* <span data-ttu-id="d2006-161">Värdet för hello nod</span><span class="sxs-lookup"><span data-stu-id="d2006-161">Value of hello node</span></span>
* <span data-ttu-id="d2006-162">Tidsstämpel för källa</span><span class="sxs-lookup"><span data-stu-id="d2006-162">Source timestamp</span></span>
* <span data-ttu-id="d2006-163">OPC UA DisplayName</span><span class="sxs-lookup"><span data-stu-id="d2006-163">OPC UA DisplayName</span></span>

<span data-ttu-id="d2006-164">TSD tillåter för närvarande inte kunder toocustomize hur länge de önskar tookeep hello data för.</span><span class="sxs-lookup"><span data-stu-id="d2006-164">Currently, TSI does not allow customers toocustomize how long they wish tookeep hello data for.</span></span>

<span data-ttu-id="d2006-165">TSI-frågor mot noddata med ett SearchSpan (Time.From, Time.To) och aggregeras av OPC UA ApplicationUri eller OPC UA NodeId eller OPC UA DisplayName.</span><span class="sxs-lookup"><span data-stu-id="d2006-165">TSI queries against node data using a SearchSpan (Time.From, Time.To) and aggregates by OPC UA ApplicationUri or OPC UA NodeId or OPC UA DisplayName.</span></span>

<span data-ttu-id="d2006-166">tooretrieve hello data för hello OEE och KPI mätare och hello för serien tidsdiagram data har aggregerats med antal händelser, Sum, Avg, Min och Max.</span><span class="sxs-lookup"><span data-stu-id="d2006-166">tooretrieve hello data for hello OEE and KPI gauges, and hello time series charts, data is aggregated by count of events, Sum, Avg, Min, and Max.</span></span>

<span data-ttu-id="d2006-167">hello tidsserier skapas med hjälp av en annan process.</span><span class="sxs-lookup"><span data-stu-id="d2006-167">hello time series are built using a different process.</span></span> <span data-ttu-id="d2006-168">OEE och KPI: er beräknas från station grunddata och bubblas ned för hello-topologi (produktion rader, fabriker, enterprise) i hello program.</span><span class="sxs-lookup"><span data-stu-id="d2006-168">OEE and KPIs are calculated from station base data and bubbled up for hello topology (production lines, factories, enterprise) in hello application.</span></span>

<span data-ttu-id="d2006-169">Dessutom beräknas tidsserier för OEE och KPI-topologi i hello app när timespan visas är klar.</span><span class="sxs-lookup"><span data-stu-id="d2006-169">Additionally, time series for OEE and KPI topology is calculated in hello app, whenever a displayed timespan is ready.</span></span> <span data-ttu-id="d2006-170">Till exempel uppdateras hello dagsvyn varje fullständig timme.</span><span class="sxs-lookup"><span data-stu-id="d2006-170">For example, hello day view is updated every full hour.</span></span>

<span data-ttu-id="d2006-171">hello serien vy av noden data kommer direkt från TSD med en aggregering för timespan.</span><span class="sxs-lookup"><span data-stu-id="d2006-171">hello time series view of node data comes directly from TSI using an aggregation for timespan.</span></span>

## <a name="iot-hub"></a><span data-ttu-id="d2006-172">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="d2006-172">IoT Hub</span></span>
<span data-ttu-id="d2006-173">Hej [IoT-hubb] [ lnk-IoT Hub] tar emot data som skickas från hello OPC Publisher modul i hello moln och gör den tillgänglig toohello Azure TSD tjänsten.</span><span class="sxs-lookup"><span data-stu-id="d2006-173">hello [IoT hub][lnk-IoT Hub] receives data sent from hello OPC Publisher Module into hello cloud and makes it available toohello Azure TSI service.</span></span> 

<span data-ttu-id="d2006-174">Hej IoT-hubb i hello-lösning också:</span><span class="sxs-lookup"><span data-stu-id="d2006-174">hello IoT Hub in hello solution also:</span></span>
- <span data-ttu-id="d2006-175">Upprätthåller en identitetsregistret som lagrar hello-ID: N för alla OPC Publisher modulen och alla OPC Proxy moduler.</span><span class="sxs-lookup"><span data-stu-id="d2006-175">Maintains an identity registry that stores hello IDs for all OPC Publisher Module and all OPC Proxy Modules.</span></span>
- <span data-ttu-id="d2006-176">Används som Transportkanalen för dubbelriktad kommunikation av hello OPC Proxy-modulen.</span><span class="sxs-lookup"><span data-stu-id="d2006-176">Is used as transport channel for bidirectional communication of hello OPC Proxy Module.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="d2006-177">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="d2006-177">Azure Storage</span></span>
<span data-ttu-id="d2006-178">hello lösningen använder Azure blob storage som disklagring för hello VM- och toostore distribution.</span><span class="sxs-lookup"><span data-stu-id="d2006-178">hello solution uses Azure blob storage as disk storage for hello VM and toostore deployment data.</span></span>

## <a name="web-app"></a><span data-ttu-id="d2006-179">Webbapp</span><span class="sxs-lookup"><span data-stu-id="d2006-179">Web app</span></span>
<span data-ttu-id="d2006-180">hello-webbprogram som distribueras som en del av hello förkonfigurerade lösningen består av en integrerad OPC UA-klient, aviseringar bearbetning och telemetri visualiseringen.</span><span class="sxs-lookup"><span data-stu-id="d2006-180">hello web app deployed as part of hello preconfigured solution comprises of an integrated OPC UA client, alerts processing and telemetry visualization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="d2006-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d2006-181">Next steps</span></span>

<span data-ttu-id="d2006-182">Du kan fortsätta komma igång med IoT Suite genom att läsa hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="d2006-182">You can continue getting started with IoT Suite by reading hello following articles:</span></span>

* <span data-ttu-id="d2006-183">[Behörigheter för hello azureiotsuite.com plats][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="d2006-183">[Permissions on hello azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="d2006-184">Distribuera en gateway på Windows- eller Linux för hello anslutna factory förkonfigurerade lösningen</span><span class="sxs-lookup"><span data-stu-id="d2006-184">Deploy a gateway on Windows or Linux for hello connected factory preconfigured solution</span></span>](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
