---
title: "aaaMicrosoft Azure IoT Suite-översikt | Microsoft Docs"
description: "Översikt över hur Azure IoT Suite ger internet av saker förkonfigurerade lösningar toocollect analysera, och lagra data, ange visualiseringar och integrera med andra system."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 2d38d08a-4133-4e5c-8b28-f93cadb5df05
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/24/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 385025c5ec0d37c74689a928bc09e85b33439634
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="overview-of-azure-iot-suite"></a>Översikt över Azure IoT Suite

hello Azure internet av saker (IoT) services erbjuder en mängd funktioner. Med dessa tjänster i företagsklass kan du:

* Samla in data från enheter
* Analysera dataströmmar i rörelse
* Lagra och skicka frågor mot stora datamängder
* Visualisera både realtidsdata och historiska data
* Integrera med back office-system
* Hantera dina enheter

toodeliver dessa funktioner, Azure IoT Suite paket tillsammans flera Azure-tjänster med anpassade tillägg som *förkonfigurerade lösningar*. Dessa förkonfigurerade lösningar är grundläggande implementeringar av vanliga IoT-lösningen mönster som hjälper tooreduce hello tid som går åt toodeliver IoT-lösningar. Med hjälp av hello [IoT software development Kit][lnk-sdks], du kan anpassa och utöka dessa lösningar toomeet dina egna behov. Du kan också använda dessa lösningar som exempel eller mallar när du utvecklar nya IoT-lösningar.

hello ger följande videoklipp en introduktion tooAzure IoT Suite:

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a>Azure IoT-tjänster i Azure IoT Suite
hello förkonfigurerade lösningar använder vanligtvis hello följande tjänster:

* Core tooAzure IoT Suite är hello [Azure IoT Hub] [ lnk-iot-hub] service. Den här tjänsten tillhandahåller hello enhet till moln och moln till enhet meddelandefunktioner och fungerar som gateway toohello hello molnet och hello andra viktiga IoT Suite-tjänster. hello-tjänsten kan du tooreceive meddelanden från dina enheter med skala och skicka kommandon tooyour enheter. hello-tjänsten kan du också för[hantera dina enheter][lnk-device-management]. Till exempel konfigurera, starta om eller utför en fabriksåterställning på en eller flera enheter anslutna toohello hubben.
* [Azure Stream Analytics][lnk-asa] tillhandahåller analys av data i rörelse. IoT Suite använder den här tjänsten tooprocess inkommande telemetri, utföra aggregation och identifiera händelser. hello förkonfigurerade lösningar kan också använda stream analytics tooprocess informationsmeddelanden som innehåller data, till exempel metadata eller kommandot svar från enheter. hello-lösningar använder Stream Analytics tooprocess hello meddelanden från dina enheter och leverera meddelandena tooother tjänster.
* [Azure Storage] [ lnk-azure-storage] och [Azure Cosmos DB] [ lnk-document-db] ange hello funktioner för lagring av data. hello förkonfigurerade lösningar använder blob storage toostore telemetri och toomake den tillgänglig för analys. hello-lösningar använder Cosmos DB toostore enhetens metadata och aktivera hello enhetshanteringsfunktioner hello lösningar.
* [Azure Web Apps] [ lnk-web-apps] och [Microsoft Power BI] [ lnk-power-bi] ange hello data visualiseringsfunktioner. hello flexibilitet Power BI kan du tooquickly skapa egna interaktiva instrumentpaneler som använder IoT Suite data.

En översikt över hello arkitekturen för en typisk IoT-lösningen, se [Microsoft Azure och hello Sakernas Internet (IoT)][iot-suite-what-is-azure-iot].

## <a name="preconfigured-solutions"></a>Förkonfigurerade lösningar

IoT Suite innehåller förkonfigurerade lösningar för att aktivera du tooquickly Kom igång med och tooexplore hello vanliga IoT-scenarier som:

* Fjärrövervakning
* Förebyggande underhåll
* Ansluten fabrik

Du kan distribuera dessa lösningar tooyour Azure-prenumeration och sedan köra en fullständig, slutpunkt-till-slutpunkt IoT-scenariot.

## <a name="next-steps"></a>Nästa steg

Nu när du har en översikt över vad IoT Suite kan göra och vad är dess huvudkomponenter kan du lära dig mer om hello förkonfigurerade lösningar i IoT Suite. Mer information finns i [vad hello Azure IoT förkonfigurerade lösningar?][lnk-what-are-preconfig]

[lnk-sdks]: https://azure.microsoft.com/documentation/articles/iot-hub-sdks-summary/
[lnk-iot-hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-asa]: https://azure.microsoft.com/documentation/services/stream-analytics/
[lnk-azure-storage]: https://azure.microsoft.com/documentation/services/storage/
[lnk-document-db]: https://azure.microsoft.com/documentation/services/documentdb/
[lnk-power-bi]: https://powerbi.microsoft.com/
[lnk-web-apps]: https://azure.microsoft.com/documentation/services/app-service/web/
[iot-suite-what-is-azure-iot]: iot-suite-what-is-azure-iot.md
[lnk-what-are-preconfig]: iot-suite-what-are-preconfigured-solutions.md
[lnk-device-management]: ../iot-hub/iot-hub-device-management-overview.md
