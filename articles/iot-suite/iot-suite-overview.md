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
# <a name="overview-of-azure-iot-suite"></a><span data-ttu-id="823b3-103">Översikt över Azure IoT Suite</span><span class="sxs-lookup"><span data-stu-id="823b3-103">Overview of Azure IoT Suite</span></span>

<span data-ttu-id="823b3-104">hello Azure internet av saker (IoT) services erbjuder en mängd funktioner.</span><span class="sxs-lookup"><span data-stu-id="823b3-104">hello Azure internet of things (IoT) services offer a broad range of capabilities.</span></span> <span data-ttu-id="823b3-105">Med dessa tjänster i företagsklass kan du:</span><span class="sxs-lookup"><span data-stu-id="823b3-105">These enterprise grade services enable you to:</span></span>

* <span data-ttu-id="823b3-106">Samla in data från enheter</span><span class="sxs-lookup"><span data-stu-id="823b3-106">Collect data from devices</span></span>
* <span data-ttu-id="823b3-107">Analysera dataströmmar i rörelse</span><span class="sxs-lookup"><span data-stu-id="823b3-107">Analyze data streams in-motion</span></span>
* <span data-ttu-id="823b3-108">Lagra och skicka frågor mot stora datamängder</span><span class="sxs-lookup"><span data-stu-id="823b3-108">Store and query large data sets</span></span>
* <span data-ttu-id="823b3-109">Visualisera både realtidsdata och historiska data</span><span class="sxs-lookup"><span data-stu-id="823b3-109">Visualize both real-time and historical data</span></span>
* <span data-ttu-id="823b3-110">Integrera med back office-system</span><span class="sxs-lookup"><span data-stu-id="823b3-110">Integrate with back-office systems</span></span>
* <span data-ttu-id="823b3-111">Hantera dina enheter</span><span class="sxs-lookup"><span data-stu-id="823b3-111">Manage your devices</span></span>

<span data-ttu-id="823b3-112">toodeliver dessa funktioner, Azure IoT Suite paket tillsammans flera Azure-tjänster med anpassade tillägg som *förkonfigurerade lösningar*.</span><span class="sxs-lookup"><span data-stu-id="823b3-112">toodeliver these capabilities, Azure IoT Suite packages together multiple Azure services with custom extensions as *preconfigured solutions*.</span></span> <span data-ttu-id="823b3-113">Dessa förkonfigurerade lösningar är grundläggande implementeringar av vanliga IoT-lösningen mönster som hjälper tooreduce hello tid som går åt toodeliver IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="823b3-113">These preconfigured solutions are base implementations of common IoT solution patterns that help tooreduce hello time you take toodeliver your IoT solutions.</span></span> <span data-ttu-id="823b3-114">Med hjälp av hello [IoT software development Kit][lnk-sdks], du kan anpassa och utöka dessa lösningar toomeet dina egna behov.</span><span class="sxs-lookup"><span data-stu-id="823b3-114">Using hello [IoT software development kits][lnk-sdks], you can customize and extend these solutions toomeet your own requirements.</span></span> <span data-ttu-id="823b3-115">Du kan också använda dessa lösningar som exempel eller mallar när du utvecklar nya IoT-lösningar.</span><span class="sxs-lookup"><span data-stu-id="823b3-115">You can also use these solutions as examples or templates when you are developing new IoT solutions.</span></span>

<span data-ttu-id="823b3-116">hello ger följande videoklipp en introduktion tooAzure IoT Suite:</span><span class="sxs-lookup"><span data-stu-id="823b3-116">hello following video provides an introduction tooAzure IoT Suite:</span></span>

> [!VIDEO https://channel9.msdn.com/Events/Microsoft-Azure/AzureCon-2015/ACON309/player]
> 
> 

## <a name="azure-iot-services-in-azure-iot-suite"></a><span data-ttu-id="823b3-117">Azure IoT-tjänster i Azure IoT Suite</span><span class="sxs-lookup"><span data-stu-id="823b3-117">Azure IoT services in Azure IoT Suite</span></span>
<span data-ttu-id="823b3-118">hello förkonfigurerade lösningar använder vanligtvis hello följande tjänster:</span><span class="sxs-lookup"><span data-stu-id="823b3-118">hello preconfigured solutions typically use hello following services:</span></span>

* <span data-ttu-id="823b3-119">Core tooAzure IoT Suite är hello [Azure IoT Hub] [ lnk-iot-hub] service.</span><span class="sxs-lookup"><span data-stu-id="823b3-119">Core tooAzure IoT Suite is hello [Azure IoT Hub][lnk-iot-hub] service.</span></span> <span data-ttu-id="823b3-120">Den här tjänsten tillhandahåller hello enhet till moln och moln till enhet meddelandefunktioner och fungerar som gateway toohello hello molnet och hello andra viktiga IoT Suite-tjänster.</span><span class="sxs-lookup"><span data-stu-id="823b3-120">This service provides hello device-to-cloud and cloud-to-device messaging capabilities and acts as hello gateway toohello cloud and hello other key IoT Suite services.</span></span> <span data-ttu-id="823b3-121">hello-tjänsten kan du tooreceive meddelanden från dina enheter med skala och skicka kommandon tooyour enheter.</span><span class="sxs-lookup"><span data-stu-id="823b3-121">hello service enables you tooreceive messages from your devices at scale, and send commands tooyour devices.</span></span> <span data-ttu-id="823b3-122">hello-tjänsten kan du också för[hantera dina enheter][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="823b3-122">hello service also enables you too[manage your devices][lnk-device-management].</span></span> <span data-ttu-id="823b3-123">Till exempel konfigurera, starta om eller utför en fabriksåterställning på en eller flera enheter anslutna toohello hubben.</span><span class="sxs-lookup"><span data-stu-id="823b3-123">For example, you can configure, reboot, or perform a factory reset on one or more devices connected toohello hub.</span></span>
* <span data-ttu-id="823b3-124">[Azure Stream Analytics][lnk-asa] tillhandahåller analys av data i rörelse.</span><span class="sxs-lookup"><span data-stu-id="823b3-124">[Azure Stream Analytics][lnk-asa] provides in-motion data analysis.</span></span> <span data-ttu-id="823b3-125">IoT Suite använder den här tjänsten tooprocess inkommande telemetri, utföra aggregation och identifiera händelser.</span><span class="sxs-lookup"><span data-stu-id="823b3-125">IoT Suite uses this service tooprocess incoming telemetry, perform aggregation, and detect events.</span></span> <span data-ttu-id="823b3-126">hello förkonfigurerade lösningar kan också använda stream analytics tooprocess informationsmeddelanden som innehåller data, till exempel metadata eller kommandot svar från enheter.</span><span class="sxs-lookup"><span data-stu-id="823b3-126">hello preconfigured solutions also use stream analytics tooprocess informational messages that contain data such as metadata or command responses from devices.</span></span> <span data-ttu-id="823b3-127">hello-lösningar använder Stream Analytics tooprocess hello meddelanden från dina enheter och leverera meddelandena tooother tjänster.</span><span class="sxs-lookup"><span data-stu-id="823b3-127">hello solutions use Stream Analytics tooprocess hello messages from your devices and deliver those messages tooother services.</span></span>
* <span data-ttu-id="823b3-128">[Azure Storage] [ lnk-azure-storage] och [Azure Cosmos DB] [ lnk-document-db] ange hello funktioner för lagring av data.</span><span class="sxs-lookup"><span data-stu-id="823b3-128">[Azure Storage][lnk-azure-storage] and [Azure Cosmos DB][lnk-document-db] provide hello data storage capabilities.</span></span> <span data-ttu-id="823b3-129">hello förkonfigurerade lösningar använder blob storage toostore telemetri och toomake den tillgänglig för analys.</span><span class="sxs-lookup"><span data-stu-id="823b3-129">hello preconfigured solutions use blob storage toostore telemetry and toomake it available for analysis.</span></span> <span data-ttu-id="823b3-130">hello-lösningar använder Cosmos DB toostore enhetens metadata och aktivera hello enhetshanteringsfunktioner hello lösningar.</span><span class="sxs-lookup"><span data-stu-id="823b3-130">hello solutions use Cosmos DB toostore device metadata and enable hello device management capabilities of hello solutions.</span></span>
* <span data-ttu-id="823b3-131">[Azure Web Apps] [ lnk-web-apps] och [Microsoft Power BI] [ lnk-power-bi] ange hello data visualiseringsfunktioner.</span><span class="sxs-lookup"><span data-stu-id="823b3-131">[Azure Web Apps][lnk-web-apps] and [Microsoft Power BI][lnk-power-bi] provide hello data visualization capabilities.</span></span> <span data-ttu-id="823b3-132">hello flexibilitet Power BI kan du tooquickly skapa egna interaktiva instrumentpaneler som använder IoT Suite data.</span><span class="sxs-lookup"><span data-stu-id="823b3-132">hello flexibility of Power BI enables you tooquickly build your own interactive dashboards that use IoT Suite data.</span></span>

<span data-ttu-id="823b3-133">En översikt över hello arkitekturen för en typisk IoT-lösningen, se [Microsoft Azure och hello Sakernas Internet (IoT)][iot-suite-what-is-azure-iot].</span><span class="sxs-lookup"><span data-stu-id="823b3-133">For an overview of hello architecture of a typical IoT solution, see [Microsoft Azure and hello Internet of Things (IoT)][iot-suite-what-is-azure-iot].</span></span>

## <a name="preconfigured-solutions"></a><span data-ttu-id="823b3-134">Förkonfigurerade lösningar</span><span class="sxs-lookup"><span data-stu-id="823b3-134">Preconfigured solutions</span></span>

<span data-ttu-id="823b3-135">IoT Suite innehåller förkonfigurerade lösningar för att aktivera du tooquickly Kom igång med och tooexplore hello vanliga IoT-scenarier som:</span><span class="sxs-lookup"><span data-stu-id="823b3-135">IoT Suite includes preconfigured solutions that enable you tooquickly get started with and tooexplore hello common IoT scenarios, such as:</span></span>

* <span data-ttu-id="823b3-136">Fjärrövervakning</span><span class="sxs-lookup"><span data-stu-id="823b3-136">Remote monitoring</span></span>
* <span data-ttu-id="823b3-137">Förebyggande underhåll</span><span class="sxs-lookup"><span data-stu-id="823b3-137">Predictive maintenance</span></span>
* <span data-ttu-id="823b3-138">Ansluten fabrik</span><span class="sxs-lookup"><span data-stu-id="823b3-138">Connected factory</span></span>

<span data-ttu-id="823b3-139">Du kan distribuera dessa lösningar tooyour Azure-prenumeration och sedan köra en fullständig, slutpunkt-till-slutpunkt IoT-scenariot.</span><span class="sxs-lookup"><span data-stu-id="823b3-139">You can deploy these solutions tooyour Azure subscription and then run a complete, end-to-end IoT scenario.</span></span>

## <a name="next-steps"></a><span data-ttu-id="823b3-140">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="823b3-140">Next steps</span></span>

<span data-ttu-id="823b3-141">Nu när du har en översikt över vad IoT Suite kan göra och vad är dess huvudkomponenter kan du lära dig mer om hello förkonfigurerade lösningar i IoT Suite.</span><span class="sxs-lookup"><span data-stu-id="823b3-141">Now that you have an overview of what IoT Suite can do and what are its main components, you can learn more about hello preconfigured solutions in IoT Suite.</span></span> <span data-ttu-id="823b3-142">Mer information finns i [vad hello Azure IoT förkonfigurerade lösningar?][lnk-what-are-preconfig]</span><span class="sxs-lookup"><span data-stu-id="823b3-142">For more information, see [What are hello Azure IoT preconfigured solutions?][lnk-what-are-preconfig]</span></span>

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
