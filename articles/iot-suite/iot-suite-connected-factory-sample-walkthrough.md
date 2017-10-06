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
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Genomgång av den förkonfigurerade lösningen Ansluten fabrik

Hej IoT Suite anslutna factory [förkonfigurerade lösningen] [ lnk-preconfigured-solutions] är en implementering av en slutpunkt till slutpunkt industriella lösning som:

* Ansluter tooboth simulerade industriella enheter som kör OPC UA servrar i simulerade factory produktionsrader och verkliga OPC UA server-enheter. Mer information om OPC UA finns hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).
* Visar KPI:er och OEE:er för drift för enheterna och produktionsraderna.
* Visar hur ett molnbaserade program kan använda toointeract med OPC UA serversystem.
* Aktiverar du tooconnect egna OPC UA server-enheter.
* Aktiverar toobrowse och ändra hello OPC UA serverdata.
* Integreras med hello Azure tid serien insikter (TSD) tjänsten tooprovide anpassade datavyer hello från OPC UA-servrar.

Du kan använda hello lösning som en startpunkt för din egen implementering och [anpassa] [ lnk-customize] den toomeet egna specifika krav i företaget.

Den här artikeln guidar dig igenom några av hello viktiga delar i hello anslutna factory lösning tooenable toounderstand hur det fungerar. Med den här kunskapen kan du sedan:

* Felsöka problem i hello-lösning.
* Planera hur toocustomize toohello lösning toomeet egna specifika krav.
* Utforma en egen IoT-lösning som använder Azure-tjänster.

Mer information finns i hello [anslutna factory vanliga frågor och svar](iot-suite-faq-cf.md).

## <a name="logical-architecture"></a>Logisk arkitektur

följande diagram hello beskrivs hello logiska komponenter av hello förkonfigurerade lösningen:

![Logisk arkitektur för ansluten fabrik][connected-factory-logical]

## <a name="communication-patterns"></a>Kommunikationsmönster

hello lösningen använder hello [OPC UA Pub/Sub-specifikationen](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/) toosend OPC UA telemetri data tooIoT hubb i JSON-format. hello lösningen använder hello [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher) IoT kant-modul för detta ändamål.

hello-lösningen har också en OPC UA-klient som är integrerade i ett program som kan upprätta anslutningar med lokala OPC UA servrar. hello-klienten använder en [omvänd proxy](https://wikipedia.org/wiki/Reverse_proxy) och tar emot hjälp från IoT-hubb toomake hello anslutning utan att öppna portar i brandväggen för hello lokalt. Det här kommunikationsmönstret kallas [tjänstassisterad kommunikation](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/). hello lösningen använder hello [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/) IoT kant-modul för detta ändamål.


## <a name="simulation"></a>Simulering

Hej simulerade stationer och hello simulerade Tillverkningskörning system (MES) utgör en fabrik produktion rad. hello simulerade enheterna och hello OPC Publisher modulen baseras på hello [OPC UA .NET Standard] [ lnk-OPC-UA-NET-Standard] publicerats av hello OPC Foundation.

hello OPC-Proxy och OPC utgivare implementeras som moduler baserat på [Azure IoT kant][lnk-Azure-IoT-Gateway]. Varje simulerad produktionsrad har en särskild gateway ansluten.

Alla simuleringskomponenter körs i Docker-behållare som finns i en virtuell Azure Linux-dator. hello simuleringen är konfigurerade toorun åtta simulerade produktionsrader som standard.

## <a name="simulated-production-line"></a>Simulerad produktionsrad

En produktionsrad tillverkar delar. Den består av olika stationer: en sammansättningsstation, en teststation och en förpackningsstation.

hello simuleringen körs och uppdaterar hello data som exponeras via hello OPC UA noder. Alla simulerade produktion rad stationer styrd av hello MES via OPC UA.

## <a name="simulated-manufacturing-execution-system"></a>Simulerat körningssystem för tillverkning

hello MES övervakar varje station i hello produktion linje genom OPC UA toodetect station status ändras. Den anropar OPC UA metoder toocontrol hello stationer och överför en produkt från en station toohello bredvid tills den är klar.

## <a name="gateway-opc-publisher-module"></a>Gateway OPC Publisher-modul

OPC Publisher modulen ansluter toohello station OPC UA servrar och prenumererar toohello OPC noder toobe publiceras. hello modulen konverterar hello noden data till JSON-format, krypteras det och skickar den tooIoT hubb som OPC UA Pub/Sub-meddelanden.

hello OPC Publisher modulen endast kräver en utgående https-port (443) och kan arbeta med den befintliga infrastrukturen för företaget.

## <a name="gateway-opc-proxy-module"></a>Gateway OPC Proxy-modul

hello Gateway OPC UA Proxy modulen tunnlar binära OPC UA kommando- och meddelanden och kräver bara en utgående https-port (443). Den kan fungera med befintlig företagsinfrastruktur, inklusive webbproxyservrar.

Den använder IoT Hub-enhet metoder tootransfer packetized TCP/IP-data på programnivå hello och därmed säkerställer endpoint förtroende, datakryptering och dataintegritet med hjälp av SSL/TLS.

hello OPC UA binära protokollet vidarebefordrande via hello proxy själva använder UA autentisering och kryptering.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

hello Gateway OPC Publisher modulen prenumererar tooOPC UA server noder toodetect ändring i hello datavärden. Om en dataändring av har identifierats i en av noderna hello, skickar meddelanden tooAzure IoT-hubb sedan i den här modulen.

IoT-hubb innehåller en händelse källa tooAzure TSD. TSD lagrar data i 30 dagar, utifrån tidsstämplar anslutna toohello meddelanden. Dessa data omfattar:

* OPC UA ApplicationUri
* OPC UA NodeId
* Värdet för hello nod
* Tidsstämpel för källa
* OPC UA DisplayName

TSD tillåter för närvarande inte kunder toocustomize hur länge de önskar tookeep hello data för.

TSI-frågor mot noddata med ett SearchSpan (Time.From, Time.To) och aggregeras av OPC UA ApplicationUri eller OPC UA NodeId eller OPC UA DisplayName.

tooretrieve hello data för hello OEE och KPI mätare och hello för serien tidsdiagram data har aggregerats med antal händelser, Sum, Avg, Min och Max.

hello tidsserier skapas med hjälp av en annan process. OEE och KPI: er beräknas från station grunddata och bubblas ned för hello-topologi (produktion rader, fabriker, enterprise) i hello program.

Dessutom beräknas tidsserier för OEE och KPI-topologi i hello app när timespan visas är klar. Till exempel uppdateras hello dagsvyn varje fullständig timme.

hello serien vy av noden data kommer direkt från TSD med en aggregering för timespan.

## <a name="iot-hub"></a>IoT Hub
Hej [IoT-hubb] [ lnk-IoT Hub] tar emot data som skickas från hello OPC Publisher modul i hello moln och gör den tillgänglig toohello Azure TSD tjänsten. 

Hej IoT-hubb i hello-lösning också:
- Upprätthåller en identitetsregistret som lagrar hello-ID: N för alla OPC Publisher modulen och alla OPC Proxy moduler.
- Används som Transportkanalen för dubbelriktad kommunikation av hello OPC Proxy-modulen.

## <a name="azure-storage"></a>Azure Storage
hello lösningen använder Azure blob storage som disklagring för hello VM- och toostore distribution.

## <a name="web-app"></a>Webbapp
hello-webbprogram som distribueras som en del av hello förkonfigurerade lösningen består av en integrerad OPC UA-klient, aviseringar bearbetning och telemetri visualiseringen.

## <a name="next-steps"></a>Nästa steg

Du kan fortsätta komma igång med IoT Suite genom att läsa hello följande artiklar:

* [Behörigheter för hello azureiotsuite.com plats][lnk-permissions]
* [Distribuera en gateway på Windows- eller Linux för hello anslutna factory förkonfigurerade lösningen](iot-suite-connected-factory-gateway-deployment.md)

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
