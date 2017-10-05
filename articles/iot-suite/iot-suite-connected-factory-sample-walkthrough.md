---
title: "Genomgång av ansluten fabrikslösning i Azure IoT Suite | Microsoft Docs"
description: "En beskrivning av den förkonfigurerade lösningen Ansluten fabrik i Azure IoT och dess arkitektur."
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
ms.openlocfilehash: 517e908a744734139ed0aeee314a4f3b9eda86cc
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a><span data-ttu-id="acbc5-103">Genomgång av den förkonfigurerade lösningen Ansluten fabrik</span><span class="sxs-lookup"><span data-stu-id="acbc5-103">Connected factory preconfigured solution walkthrough</span></span>

<span data-ttu-id="acbc5-104">IoT Suites [förkonfigurerade lösning][lnk-preconfigured-solutions] är en implementering av en branschlösning från slutpunkt till slutpunkt som:</span><span class="sxs-lookup"><span data-stu-id="acbc5-104">The IoT Suite connected factory [preconfigured solution][lnk-preconfigured-solutions] is an implementation of an end-to-end industrial solution that:</span></span>

* <span data-ttu-id="acbc5-105">Ansluter till både simulerade industriella enheter som kör OPC UA-servrar i simulerade fabriksproduktionsrader och äkta OPC UA-serverenheter.</span><span class="sxs-lookup"><span data-stu-id="acbc5-105">Connects to both simulated industrial devices running OPC UA servers in simulated factory production lines, and real OPC UA server devices.</span></span> <span data-ttu-id="acbc5-106">Mer information om OPC UA finns i [Vanliga frågor och svar om ansluten fabrik](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="acbc5-106">For more information about OPC UA, see the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>
* <span data-ttu-id="acbc5-107">Visar KPI:er och OEE:er för drift för enheterna och produktionsraderna.</span><span class="sxs-lookup"><span data-stu-id="acbc5-107">Shows operational KPIs and OEE of those devices and production lines.</span></span>
* <span data-ttu-id="acbc5-108">Visar hur ett molnbaserat program kan användas för att interagera med OPC UA-serversystem.</span><span class="sxs-lookup"><span data-stu-id="acbc5-108">Demonstrates how a cloud-based application could be used to interact with OPC UA server systems.</span></span>
* <span data-ttu-id="acbc5-109">Gör att du kan ansluta dina egna OPC UA-serverenheter.</span><span class="sxs-lookup"><span data-stu-id="acbc5-109">Enables you to connect your own OPC UA server devices.</span></span>
* <span data-ttu-id="acbc5-110">Gör att du kan bläddra och ändra OPC UA-serverdata.</span><span class="sxs-lookup"><span data-stu-id="acbc5-110">Enables you to browse and modify the OPC UA server data.</span></span>
* <span data-ttu-id="acbc5-111">Integreras med Azure-tjänsten Time Series Insights (TSI) för att tillhandahålla anpassade vyer av data från dina OPC UA-servrar.</span><span class="sxs-lookup"><span data-stu-id="acbc5-111">Integrates with the Azure Time Series Insights (TSI) service to provide customized views of the data from your OPC UA servers.</span></span>

<span data-ttu-id="acbc5-112">Du kan använda lösningen som startpunkt för en egen implementering och [anpassa][lnk-customize] den efter dina egna affärsbehov.</span><span class="sxs-lookup"><span data-stu-id="acbc5-112">You can use the solution as a starting point for your own implementation and [customize][lnk-customize] it to meet your own specific business requirements.</span></span>

<span data-ttu-id="acbc5-113">Den här artikeln beskriver några av de viktigaste elementen i den anslutna fabrikslösningen så att du förstår hur den fungerar.</span><span class="sxs-lookup"><span data-stu-id="acbc5-113">This article walks you through some of the key elements of the connected factory solution to enable you to understand how it works.</span></span> <span data-ttu-id="acbc5-114">Med den här kunskapen kan du sedan:</span><span class="sxs-lookup"><span data-stu-id="acbc5-114">This knowledge helps you to:</span></span>

* <span data-ttu-id="acbc5-115">Felsöka problem i lösningen.</span><span class="sxs-lookup"><span data-stu-id="acbc5-115">Troubleshoot issues in the solution.</span></span>
* <span data-ttu-id="acbc5-116">Planera hur lösningen kan anpassas för att uppfylla dina behov.</span><span class="sxs-lookup"><span data-stu-id="acbc5-116">Plan how to customize to the solution to meet your own specific requirements.</span></span>
* <span data-ttu-id="acbc5-117">Utforma en egen IoT-lösning som använder Azure-tjänster.</span><span class="sxs-lookup"><span data-stu-id="acbc5-117">Design your own IoT solution that uses Azure services.</span></span>

<span data-ttu-id="acbc5-118">Mer information finns i [Vanliga frågor och svar om ansluten fabrik](iot-suite-faq-cf.md).</span><span class="sxs-lookup"><span data-stu-id="acbc5-118">For more information, see the [Connected factory FAQ](iot-suite-faq-cf.md).</span></span>

## <a name="logical-architecture"></a><span data-ttu-id="acbc5-119">Logisk arkitektur</span><span class="sxs-lookup"><span data-stu-id="acbc5-119">Logical architecture</span></span>

<span data-ttu-id="acbc5-120">Följande diagram illustrerar de logiska komponenterna i den förkonfigurerade lösningen:</span><span class="sxs-lookup"><span data-stu-id="acbc5-120">The following diagram outlines the logical components of the preconfigured solution:</span></span>

![Logisk arkitektur för ansluten fabrik][connected-factory-logical]

## <a name="communication-patterns"></a><span data-ttu-id="acbc5-122">Kommunikationsmönster</span><span class="sxs-lookup"><span data-stu-id="acbc5-122">Communication patterns</span></span>

<span data-ttu-id="acbc5-123">Lösningen använder [OPC UA Pub/Sub-specifikationen](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) för att skicka OPC UA-telemetridata till IoT Hub i JSON-format.</span><span class="sxs-lookup"><span data-stu-id="acbc5-123">The solution uses the [OPC UA Pub/Sub specification](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) to send OPC UA telemetry data to IoT Hub in JSON format.</span></span> <span data-ttu-id="acbc5-124">Lösningen använder [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge-modulen för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="acbc5-124">The solution uses the [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT Edge module for this purpose.</span></span>

<span data-ttu-id="acbc5-125">Lösningen har också en OPC UA-klient som är integrerad i ett webbprogram som kan upprätta anslutningar med lokala OPC UA-servrar.</span><span class="sxs-lookup"><span data-stu-id="acbc5-125">The solution also has an OPC UA client integrated into a web application that can establish connections with on-premises OPC UA servers.</span></span> <span data-ttu-id="acbc5-126">Klienten använder en [omvänd proxy](https://wikipedia.org/wiki/Reverse_proxy) och får hjälp från IoT Hub med att upprätta anslutningen utan att kräva öppna portar i den lokala brandväggen.</span><span class="sxs-lookup"><span data-stu-id="acbc5-126">The client uses a [reverse-proxy](https://wikipedia.org/wiki/Reverse_proxy) and receives help from IoT Hub to make the connection without requiring open ports in the on-premises firewall.</span></span> <span data-ttu-id="acbc5-127">Det här kommunikationsmönstret kallas [tjänstassisterad kommunikation](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span><span class="sxs-lookup"><span data-stu-id="acbc5-127">This communication pattern is called [service-assisted communication](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/).</span></span> <span data-ttu-id="acbc5-128">Lösningen använder [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge-modulen för detta ändamål.</span><span class="sxs-lookup"><span data-stu-id="acbc5-128">The solution uses the [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT Edge module for this purpose.</span></span>


## <a name="simulation"></a><span data-ttu-id="acbc5-129">Simulering</span><span class="sxs-lookup"><span data-stu-id="acbc5-129">Simulation</span></span>

<span data-ttu-id="acbc5-130">De simulerade stationerna och de simulerade tillverkningsverkställningssystemen (MES) bildar en fabriksproduktionsrad.</span><span class="sxs-lookup"><span data-stu-id="acbc5-130">The simulated stations and the simulated manufacturing execution systems (MES) make up a factory production line.</span></span> <span data-ttu-id="acbc5-131">De simulerade enheterna och OPC Publisher-modulen baseras på [OPC UA .NET-standarden][lnk-OPC-UA-NET-Standard] som ges ut av OPC Foundation.</span><span class="sxs-lookup"><span data-stu-id="acbc5-131">The simulated devices and the OPC Publisher Module are based on the [OPC UA .NET Standard][lnk-OPC-UA-NET-Standard] published by the OPC Foundation.</span></span>

<span data-ttu-id="acbc5-132">OPC Proxy och OPC Publisher implementeras som moduler baserat på [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span><span class="sxs-lookup"><span data-stu-id="acbc5-132">The OPC Proxy and OPC Publisher are implemented as modules based on [Azure IoT Edge][lnk-Azure-IoT-Gateway].</span></span> <span data-ttu-id="acbc5-133">Varje simulerad produktionsrad har en särskild gateway ansluten.</span><span class="sxs-lookup"><span data-stu-id="acbc5-133">Each simulated production line has a designated gateway attached.</span></span>

<span data-ttu-id="acbc5-134">Alla simuleringskomponenter körs i Docker-behållare som finns i en virtuell Azure Linux-dator.</span><span class="sxs-lookup"><span data-stu-id="acbc5-134">All simulation components run in Docker containers  hosted in an Azure Linux VM.</span></span> <span data-ttu-id="acbc5-135">Simuleringen konfigureras för att köra åtta simulerade produktionsrader som standard.</span><span class="sxs-lookup"><span data-stu-id="acbc5-135">The simulation is configured to run eight simulated production lines by default.</span></span>

## <a name="simulated-production-line"></a><span data-ttu-id="acbc5-136">Simulerad produktionsrad</span><span class="sxs-lookup"><span data-stu-id="acbc5-136">Simulated production line</span></span>

<span data-ttu-id="acbc5-137">En produktionsrad tillverkar delar.</span><span class="sxs-lookup"><span data-stu-id="acbc5-137">A production line manufactures parts.</span></span> <span data-ttu-id="acbc5-138">Den består av olika stationer: en sammansättningsstation, en teststation och en förpackningsstation.</span><span class="sxs-lookup"><span data-stu-id="acbc5-138">It is composed of different stations: an assembly station, a test station, and a packaging station.</span></span>

<span data-ttu-id="acbc5-139">Simuleringen körs och uppdaterar data som exponeras via OPC UA-noderna.</span><span class="sxs-lookup"><span data-stu-id="acbc5-139">The simulation runs and updates the data that is exposed through the OPC UA nodes.</span></span> <span data-ttu-id="acbc5-140">Alla simulerade produktionsradstationer samordnas av MES via OPC UA.</span><span class="sxs-lookup"><span data-stu-id="acbc5-140">All simulated production line stations are orchestrated by the MES through OPC UA.</span></span>

## <a name="simulated-manufacturing-execution-system"></a><span data-ttu-id="acbc5-141">Simulerat körningssystem för tillverkning</span><span class="sxs-lookup"><span data-stu-id="acbc5-141">Simulated manufacturing execution system</span></span>

<span data-ttu-id="acbc5-142">MES övervakar varje station i produktionsraden via OPC UA för att identifiera ändringar i stationens status.</span><span class="sxs-lookup"><span data-stu-id="acbc5-142">The MES monitors each station in the production line through OPC UA to detect station status changes.</span></span> <span data-ttu-id="acbc5-143">Den anropar OPC UA-metoder för att kontrollera stationerna och överför en produkt från en station till en annan tills det är klart.</span><span class="sxs-lookup"><span data-stu-id="acbc5-143">It calls OPC UA methods to control the stations and passes a product from one station to the next until it is complete.</span></span>

## <a name="gateway-opc-publisher-module"></a><span data-ttu-id="acbc5-144">Gateway OPC Publisher-modul</span><span class="sxs-lookup"><span data-stu-id="acbc5-144">Gateway OPC publisher module</span></span>

<span data-ttu-id="acbc5-145">OPC Publisher-modulen ansluter till stationens OPC UA-servrar och prenumererar på OPC-noder som ska publiceras.</span><span class="sxs-lookup"><span data-stu-id="acbc5-145">OPC Publisher Module connects to the station OPC UA servers and subscribes to the OPC nodes to be published.</span></span> <span data-ttu-id="acbc5-146">Modulen omvandlar noddata till JSON-format, krypterar dem och skickar dem till IoT Hub som OPC UA pub-/sub-meddelanden.</span><span class="sxs-lookup"><span data-stu-id="acbc5-146">The module converts the node data into JSON format, encrypts it, and sends it to IoT Hub as OPC UA Pub/Sub messages.</span></span>

<span data-ttu-id="acbc5-147">OPC Publisher-modulen kräver endast en utgående https-port (443) och kan arbeta med företagets befintliga infrastruktur.</span><span class="sxs-lookup"><span data-stu-id="acbc5-147">The OPC Publisher module only requires an outbound https port (443) and can work with existing enterprise infrastructure.</span></span>

## <a name="gateway-opc-proxy-module"></a><span data-ttu-id="acbc5-148">Gateway OPC Proxy-modul</span><span class="sxs-lookup"><span data-stu-id="acbc5-148">Gateway OPC proxy module</span></span>

<span data-ttu-id="acbc5-149">Gateway OPC UA Proxy-modulen fungerar som en tunnel för binära OPC UA-kommandon och kontrollerar meddelanden och kräver endast en utgående https-port (443).</span><span class="sxs-lookup"><span data-stu-id="acbc5-149">The Gateway OPC UA Proxy Module tunnels binary OPC UA command and control messages and only requires an outbound https port (443).</span></span> <span data-ttu-id="acbc5-150">Den kan fungera med befintlig företagsinfrastruktur, inklusive webbproxyservrar.</span><span class="sxs-lookup"><span data-stu-id="acbc5-150">It can work with existing enterprise infrastructure, including Web Proxies.</span></span>

<span data-ttu-id="acbc5-151">Den använder IoT Hub-enhetsmetoder för att överföra paketerade TCP/IP-data på programnivån och garanterar på så sätt slutpunktsförtroende, datakryptering och integritet med SSL/TLS.</span><span class="sxs-lookup"><span data-stu-id="acbc5-151">It uses IoT Hub Device methods to transfer packetized TCP/IP data at the application layer and thus ensures endpoint trust, data encryption, and integrity using SSL/TLS.</span></span>

<span data-ttu-id="acbc5-152">Det binära OPC UA-protokollet som är vidarebefordrande via själva proxyservern använder UA-autentisering och -kryptering.</span><span class="sxs-lookup"><span data-stu-id="acbc5-152">The OPC UA binary protocol relayed through the proxy itself uses UA authentication and encryption.</span></span>

## <a name="azure-time-series-insights"></a><span data-ttu-id="acbc5-153">Azure Time Series Insights</span><span class="sxs-lookup"><span data-stu-id="acbc5-153">Azure Time Series Insights</span></span>

<span data-ttu-id="acbc5-154">Gateway OPC Publisher-modulen prenumererar på OPC UA-servernoder för att identifiera ändringar i datavärdena.</span><span class="sxs-lookup"><span data-stu-id="acbc5-154">The Gateway OPC Publisher Module subscribes to OPC UA server nodes to detect change in the data values.</span></span> <span data-ttu-id="acbc5-155">Om en dataändring identifieras i någon av noderna kan modulen skicka meddelanden till Azure IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="acbc5-155">If a data change is detected in one of the nodes, this module then sends messages to Azure IoT Hub.</span></span>

<span data-ttu-id="acbc5-156">IoT Hub tillhandahåller en händelsekälla till Azure TSI.</span><span class="sxs-lookup"><span data-stu-id="acbc5-156">IoT Hub provides an event source to Azure TSI.</span></span> <span data-ttu-id="acbc5-157">TSI lagrar data i 30 dagar baserat på tidsstämplar som är kopplade till meddelanden.</span><span class="sxs-lookup"><span data-stu-id="acbc5-157">TSI stores data for 30 days based on timestamps attached to the messages.</span></span> <span data-ttu-id="acbc5-158">Dessa data omfattar:</span><span class="sxs-lookup"><span data-stu-id="acbc5-158">This data includes:</span></span>

* <span data-ttu-id="acbc5-159">OPC UA ApplicationUri</span><span class="sxs-lookup"><span data-stu-id="acbc5-159">OPC UA ApplicationUri</span></span>
* <span data-ttu-id="acbc5-160">OPC UA NodeId</span><span class="sxs-lookup"><span data-stu-id="acbc5-160">OPC UA NodeId</span></span>
* <span data-ttu-id="acbc5-161">Nodens värde</span><span class="sxs-lookup"><span data-stu-id="acbc5-161">Value of the node</span></span>
* <span data-ttu-id="acbc5-162">Tidsstämpel för källa</span><span class="sxs-lookup"><span data-stu-id="acbc5-162">Source timestamp</span></span>
* <span data-ttu-id="acbc5-163">OPC UA DisplayName</span><span class="sxs-lookup"><span data-stu-id="acbc5-163">OPC UA DisplayName</span></span>

<span data-ttu-id="acbc5-164">För närvarande tillåter inte TSI att kunder anpassar hur länge de vill behålla data.</span><span class="sxs-lookup"><span data-stu-id="acbc5-164">Currently, TSI does not allow customers to customize how long they wish to keep the data for.</span></span>

<span data-ttu-id="acbc5-165">TSI-frågor mot noddata med ett SearchSpan (Time.From, Time.To) och aggregeras av OPC UA ApplicationUri eller OPC UA NodeId eller OPC UA DisplayName.</span><span class="sxs-lookup"><span data-stu-id="acbc5-165">TSI queries against node data using a SearchSpan (Time.From, Time.To) and aggregates by OPC UA ApplicationUri or OPC UA NodeId or OPC UA DisplayName.</span></span>

<span data-ttu-id="acbc5-166">Om du vill hämta data för OEE- och KPI-mätare och tidsseriediagram aggregeras data via antal händelser, Summa, Medel, Min och Max.</span><span class="sxs-lookup"><span data-stu-id="acbc5-166">To retrieve the data for the OEE and KPI gauges, and the time series charts, data is aggregated by count of events, Sum, Avg, Min, and Max.</span></span>

<span data-ttu-id="acbc5-167">Tidsserier skapas med en annan process.</span><span class="sxs-lookup"><span data-stu-id="acbc5-167">The time series are built using a different process.</span></span> <span data-ttu-id="acbc5-168">OEE och KPI:er beräknas från stationsgrunddata och används för topologin (produktionsrader, fabriker, företag) i programmet.</span><span class="sxs-lookup"><span data-stu-id="acbc5-168">OEE and KPIs are calculated from station base data and bubbled up for the topology (production lines, factories, enterprise) in the application.</span></span>

<span data-ttu-id="acbc5-169">Dessutom beräknas tidsserier för OEE- och KPI-topologi i appen när en visad tidsangivelse är klar.</span><span class="sxs-lookup"><span data-stu-id="acbc5-169">Additionally, time series for OEE and KPI topology is calculated in the app, whenever a displayed timespan is ready.</span></span> <span data-ttu-id="acbc5-170">Dagsvyn uppdateras exempelvis varje hel timme.</span><span class="sxs-lookup"><span data-stu-id="acbc5-170">For example, the day view is updated every full hour.</span></span>

<span data-ttu-id="acbc5-171">Tidsserievyn för noddata kommer direkt från TSI med en sammansättning för tidsangivelse.</span><span class="sxs-lookup"><span data-stu-id="acbc5-171">The time series view of node data comes directly from TSI using an aggregation for timespan.</span></span>

## <a name="iot-hub"></a><span data-ttu-id="acbc5-172">IoT Hub</span><span class="sxs-lookup"><span data-stu-id="acbc5-172">IoT Hub</span></span>
<span data-ttu-id="acbc5-173">[IoT Hub][lnk-IoT Hub] tar emot data som skickas från OPC Publisher-modulen till molnet och gör dem tillgängliga för Azure TSI-tjänsten.</span><span class="sxs-lookup"><span data-stu-id="acbc5-173">The [IoT hub][lnk-IoT Hub] receives data sent from the OPC Publisher Module into the cloud and makes it available to the Azure TSI service.</span></span> 

<span data-ttu-id="acbc5-174">IoT Hub ansvarar även för följande uppgifter i lösningen:</span><span class="sxs-lookup"><span data-stu-id="acbc5-174">The IoT Hub in the solution also:</span></span>
- <span data-ttu-id="acbc5-175">Underhåller ett identitetsregister som lagrar ID för alla OPC Publisher-moduler och OPC Proxy-moduler.</span><span class="sxs-lookup"><span data-stu-id="acbc5-175">Maintains an identity registry that stores the IDs for all OPC Publisher Module and all OPC Proxy Modules.</span></span>
- <span data-ttu-id="acbc5-176">Det används som en transportkanal för dubbelriktad kommunikation för OPC Proxy-modulen.</span><span class="sxs-lookup"><span data-stu-id="acbc5-176">Is used as transport channel for bidirectional communication of the OPC Proxy Module.</span></span>

## <a name="azure-storage"></a><span data-ttu-id="acbc5-177">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="acbc5-177">Azure Storage</span></span>
<span data-ttu-id="acbc5-178">Lösningen använder Azure Blob Storage som disklagring för den virtuella datorn och för att lagra distribueringsdata.</span><span class="sxs-lookup"><span data-stu-id="acbc5-178">The solution uses Azure blob storage as disk storage for the VM and to store deployment data.</span></span>

## <a name="web-app"></a><span data-ttu-id="acbc5-179">Webbapp</span><span class="sxs-lookup"><span data-stu-id="acbc5-179">Web app</span></span>
<span data-ttu-id="acbc5-180">Webbappen distribueras som en del av den förkonfigurerade lösningen består av en integrerad OPC UA-klient, aviseringsbehandling och telemetrivisualisering.</span><span class="sxs-lookup"><span data-stu-id="acbc5-180">The web app deployed as part of the preconfigured solution comprises of an integrated OPC UA client, alerts processing and telemetry visualization.</span></span>

## <a name="next-steps"></a><span data-ttu-id="acbc5-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="acbc5-181">Next steps</span></span>

<span data-ttu-id="acbc5-182">Läs följande artiklar om du vill fortsätta och lära dig mer om IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="acbc5-182">You can continue getting started with IoT Suite by reading the following articles:</span></span>

* <span data-ttu-id="acbc5-183">[Behörigheter på webbplatsen azureiotsuite.com][lnk-permissions]</span><span class="sxs-lookup"><span data-stu-id="acbc5-183">[Permissions on the azureiotsuite.com site][lnk-permissions]</span></span>
* [<span data-ttu-id="acbc5-184">Distribuera en gateway på Windows eller Linux för den förkonfigurerade lösningen Ansluten fabrik</span><span class="sxs-lookup"><span data-stu-id="acbc5-184">Deploy a gateway on Windows or Linux for the connected factory preconfigured solution</span></span>](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
