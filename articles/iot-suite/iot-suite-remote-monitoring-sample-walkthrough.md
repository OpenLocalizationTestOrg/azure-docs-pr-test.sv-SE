---
title: "Arkitektur för fjärranslutna övervakningslösning - Azure | Microsoft Docs"
description: "En genomgång av arkitekturen för fjärråtkomst övervakning förkonfigurerade lösningen."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 11/10/2017
ms.author: dobett
ms.openlocfilehash: e19ba9c88e4fbe4f065c45ce7029247436f7155c
ms.sourcegitcommit: bc8d39fa83b3c4a66457fba007d215bccd8be985
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 11/10/2017
---
# <a name="remote-monitoring-preconfigured-solution-architecture"></a>Fjärrövervaknings förkonfigurerade lösningsarkitektur

IoT Suite fjärråtkomst övervakning [förkonfigurerade lösningen](iot-suite-what-are-preconfigured-solutions.md) implementerar en övervakningslösning för slutpunkt till slutpunkt för flera datorer på fjärrplatser. I lösningen kombineras viktiga Azure-tjänster till en allmän implementering av affärsscenariot. Du kan använda lösningen som en startpunkt för din egen implementering och [anpassa](iot-suite-remote-monitoring-customize.md) att uppfylla dina egna specifika affärsbehov.

Den här artikeln beskriver några av de viktigaste elementen i fjärrövervakningslösningen så att du förstår hur den fungerar. Med den här kunskapen kan du sedan:

* Felsöka problem i lösningen.
* Planera hur lösningen kan anpassas för att uppfylla dina behov.
* Utforma en egen IoT-lösning som använder Azure-tjänster.

## <a name="logical-architecture"></a>Logisk arkitektur

Diagrammet nedan visar logiska komponenterna i den fjärranslutna förkonfigurerade övervakningslösning högst upp på den [IoT-arkitekturen](iot-suite-what-is-azure-iot.md):

![Logisk arkitektur](media/iot-suite-remote-monitoring-sample-walkthrough/remote-monitoring-architecture.png)

## <a name="why-microservices"></a>Varför mikrotjänster?

Molnarkitektur har utvecklats eftersom Microsoft har publicerat de första förkonfigurerade lösningarna. [Mikrotjänster](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) har vuxit fram som en beprövad idé att uppnå skalbarhet och flexibilitet utan att kompromissa development hastighet. Flera Microsoft-tjänster använder den här arkitekturen mönster internt med bra tillförlitlighet och skalbarhet resultat. Uppdaterade förkonfigurerade lösningar placera dessa learnings i praktiken så att du kan också dra nytta av dem.

> [!TIP]
> Läs mer om arkitekturer för mikrotjänster i [.NET Application Architecture](https://www.microsoft.com/net/learn/architecture) (.NET-programarkitektur) och [Microservices: An application revolution powered by the cloud](https://azure.microsoft.com/blog/microservices-an-application-revolution-powered-by-the-cloud/) (Mikrotjänster: En programrevolution som drivs av molnet).

## <a name="device-connectivity"></a>Enhetsanslutning

Lösningen innehåller följande komponenter i enheten anslutning en del av logisk arkitektur:

### <a name="simulated-devices"></a>Simulerade enheter

Lösningen innehåller en mikrotjänster som gör det möjligt för dig att hantera en pool med simulerade enheter för att testa flödet för slutpunkt till slutpunkt i lösningen. Simulerade enheter:

* Generera enhet till moln telemetri.
* Svara på metodanrop moln till enhet från IoT-hubb.

Mikrotjänster ger en RESTful-slutpunkt som du kan skapa, starta och stoppa simulering. Varje simuleringen består av en uppsättning virtuella enheter av olika typer som skicka telemetri och svara på metodanrop.

Du kan etablera simulerade enheter från instrumentpanelen i portalen för lösningen.

### <a name="physical-devices"></a>Fysiska enheter

Du kan ansluta fysiska enheter till lösningen. Du kan implementera beteendet för din simulerade enheter med hjälp av Azure IoT-enhet SDK: er.

Du kan etablera fysiska enheter från instrumentpanelen i portalen för lösningen.

### <a name="iot-hub-and-the-iot-manager-microservice"></a>IoT-hubb och mikrotjänster för IoT-hanteraren

Den [IoT-hubb](../iot-hub/index.md) en data som skickas från enheter i molnet och gör den tillgänglig för den `telemetry-agent` mikrotjänster.

IoT Hub ansvarar även för följande uppgifter i lösningen:

* Upprätthåller en identitetsregistret som lagrar ID och autentiseringsnycklar över alla enheter som får ansluta till portalen. Du kan aktivera och inaktivera enheter via ID-registret.
* Att anropa metoder på dina enheter för lösningsportalens räkning.
* Att underhålla enhetstvillingar för alla registrerade enheter. De egenskapsvärden som rapporteras av en enhet lagras i enhetstvillingen. De önskade egenskaper som anges i lösningsportalen och som enheten ska hämta vid nästa anslutning lagras också där.
* Att schemalägga jobb där egenskaper ska anges för flera enheter eller där metoder ska anropas på flera enheter.

Lösningen innehåller de `iot-manager` mikrotjänster att hantera interaktioner med din IoT-hubb som:

* Skapa och hantera IoT-enheter.
* Hantering av enheten twins.
* Anropar metoder på enheter.
* Hantering av IoT-autentiseringsuppgifter.

Den här tjänsten körs även IoT-hubb frågor för att hämta enheter som hör till användardefinierade grupper.

Mikrotjänster ger en RESTful-slutpunkt för att hantera enheter och enheter twins, anropa metoder och köra frågor för IoT-hubb.

## <a name="data-processing-and-analytics"></a>Databearbetning och analys

Lösningen innehåller följande komponenter i databehandling och analytics en del av logisk arkitektur:

### <a name="device-telemetry"></a>Enhetstelemetrin

Lösningen innehåller två mikrotjänster för att hantera enheter telemetri.

Den [telemetri-agent](https://github.com/Azure/telemetry-agent-dotnet) mikrotjänster:

* Lagrar telemetri i Cosmos-databasen.
* Analyserar dataströmmen telemetri från enheter.
* Genererar larm enligt definierade regler.

Larm lagras i Cosmos-databasen.

Den `telemetry-agent` mikrotjänster kan lösningen-portalen för att läsa telemetri som skickas från enheter. Lösning portalen använder också den här tjänsten till:

* Definiera regler för övervakning, till exempel tröskelvärden som utlöser larm
* Hämta listan över de senaste larm.

Använda RESTful slutpunkten som tillhandahålls av den här mikrotjänster för att hantera telemetri, regler och larm.

### <a name="storage"></a>Lagring

Den [lagringsadapter](https://github.com/Azure/pcs-storage-adapter-dotnet) mikrotjänster är ett kort framför den huvudsakliga storage-tjänst som används för förkonfigurerade lösningen. Det ger enkel insamling och nyckel / värde-lagring.

Standarddistribution av förkonfigurerade lösningen använder Cosmos DB som dess huvudsakliga storage-tjänst.

Cosmos-DB-databasen lagrar data i den förkonfigurerade lösningen. Den **lagringsadapter** mikrotjänster fungerar som ett kort för den andra mikrotjänster i lösningen till lagringstjänster för åtkomst.

## <a name="presentation"></a>Presentation

Lösningen innehåller följande komponenter i presentation-delen av logisk arkitektur:

Den [webbanvändargränssnitt är ett reagera Javascript-program](https://github.com/Azure/pcs-remote-monitoring-webui). Program:

* Använder Javascript reagera endast och körs i webbläsaren.
* Är formaterad med CSS.
* Samverkar med offentliga Internetriktade mikrotjänster via AJAX-anrop.

Användargränssnittet visar alla förkonfigurerade lösningen funktioner och samverkar med andra tjänster som:

* Den [autentisering](https://github.com/Azure/pcs-auth-dotnet) mikrotjänster att skydda användardata.
* Den [iothub manager](https://github.com/Azure/iothub-manager-dotnet) mikrotjänster att visa och hantera IoT-enheter.

Den [ui-config](https://github.com/Azure/pcs-config-dotnet) mikrotjänster aktiverar användargränssnittet för att lagra och hämta konfigurationsinställningar.

## <a name="next-steps"></a>Nästa steg

Om du vill utforska källa kod och utvecklare dokumentationen börjar du med en två huvudsakliga GitHub-databaser:

* [Förkonfigurerade lösning för fjärråtkomst övervakning med Azure IoT (.NET)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/).
* [Förkonfigurerade lösning för fjärråtkomst övervakning med Azure IoT (Java)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-java).
* [Förkonfigurerade lösning för fjärråtkomst övervakning arkitektur)](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Architecture).

Mer information om fjärråtkomst övervakning förkonfigurerade lösningen finns [anpassa förkonfigurerade lösningen](iot-suite-remote-monitoring-customize.md).
