---
title: "Förstå Azure IoT-SDK | Microsoft Docs"
description: "Utvecklarhandbok - information om och länkar till olika Azure IoT-enheten och tjänsten SDK: erna som du kan använda för att skapa enhetsappar och backend-appar."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: c5c9a497-bb03-4301-be2d-00edfb7d308f
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bcbf4b9633f58293edb19aeb33dec6602ac4ec8f
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a><span data-ttu-id="79237-103">Förstå och använda Azure IoT-SDK</span><span class="sxs-lookup"><span data-stu-id="79237-103">Understand and use Azure IoT SDKs</span></span>

<span data-ttu-id="79237-104">Det finns tre kategorier av SDK för att arbeta med IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="79237-104">There are three categories of SDK for working with IoT Hub:</span></span>

* <span data-ttu-id="79237-105">**Enheten SDK** gör att du kan skapa appar som körs på din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="79237-105">**Device SDKs** enable you to build apps that run on your IoT devices.</span></span> <span data-ttu-id="79237-106">De här apparna skicka telemetri för din IoT-hubb och du kan också ta emot meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="79237-106">These apps send telemetry to your IoT hub, and optionally receive messages from your IoT hub.</span></span>

* <span data-ttu-id="79237-107">**SDK-tjänsten** kan du hantera din IoT-hubb och (valfritt) skicka meddelanden till din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="79237-107">**Service SDKs** enable you to manage your IoT hub, and optionally send messages to your IoT devices.</span></span>

* <span data-ttu-id="79237-108">**Azure IoT-Edge** låter dig skapa gateways för att aktivera enheter som inte använder något av protokoll som stöds eller när du behöver bearbeta meddelanden på kanten.</span><span class="sxs-lookup"><span data-stu-id="79237-108">**Azure IoT Edge** enables you to build gateways to enable devices that don't use one of the supported protocols, or when you need to process messages on the edge.</span></span>

<span data-ttu-id="79237-109">SDK: er som stöder flera programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="79237-109">SDKs are provided to support multiple programming languages.</span></span>

## <a name="azure-iot-device-sdks"></a><span data-ttu-id="79237-110">Azure IoT-enhet SDK</span><span class="sxs-lookup"><span data-stu-id="79237-110">Azure IoT device SDKs</span></span>

<span data-ttu-id="79237-111">Microsoft Azure IoT-enhet SDK innehåller kod som underlättar skapande av enheter och program som ansluter till och hanteras av Azure IoT Hub-tjänster.</span><span class="sxs-lookup"><span data-stu-id="79237-111">The Microsoft Azure IoT device SDKs contain code that facilitates building devices and applications that connect to and are managed by Azure IoT Hub services.</span></span>

<span data-ttu-id="79237-112">Följande Azure IoT-enhet SDK: er kan hämtas från GitHub:</span><span class="sxs-lookup"><span data-stu-id="79237-112">The following Azure IoT device SDKs are available to download from GitHub:</span></span>

* <span data-ttu-id="79237-113">[Azure IoT-enhet SDK för C] [ lnk-c-device-sdk] skrivs i ANSI C (C99) för överföring och bred plattformskompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="79237-113">[Azure IoT device SDK for C][lnk-c-device-sdk] written in ANSI C (C99) for portability and broad platform compatibility.</span></span> <span data-ttu-id="79237-114">Det finns två enhet-klientbibliotek för C, på låg nivå **iothub_client** och **serialiseraren**.</span><span class="sxs-lookup"><span data-stu-id="79237-114">There are two device client libraries for C, the low-level **iothub_client** and the **serializer**.</span></span>
* <span data-ttu-id="79237-115">[Azure IoT-enhet SDK för .NET][lnk-dotnet-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-115">[Azure IoT device SDK for .NET][lnk-dotnet-device-sdk]</span></span>
* <span data-ttu-id="79237-116">[Azure IoT-enhet SDK för Java][lnk-java-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-116">[Azure IoT device SDK for Java][lnk-java-device-sdk]</span></span>
* <span data-ttu-id="79237-117">[Azure IoT-enhet SDK för Node.js][lnk-node-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-117">[Azure IoT device SDK for Node.js][lnk-node-device-sdk]</span></span>
* <span data-ttu-id="79237-118">[Azure IoT-enhet SDK för Python][lnk-python-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-118">[Azure IoT device SDK for Python][lnk-python-device-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="79237-119">Se Viktigt-filerna i GitHub-lagringsplatser för information om språk och plattformsspecifika paketet chefer för att installera binärfiler och beroenden på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="79237-119">See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.</span></span>
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a><span data-ttu-id="79237-120">OS-plattformen och maskinvarukompatibilitet</span><span class="sxs-lookup"><span data-stu-id="79237-120">OS platform and hardware compatibility</span></span>

<span data-ttu-id="79237-121">Mer information om SDK-kompatibilitet med specifika maskinvaruenheter finns i [Azure certifierad för IoT-enhet katalogen][lnk-certified].</span><span class="sxs-lookup"><span data-stu-id="79237-121">For more information about SDK compatibility with specific hardware devices, see the [Azure Certified for IoT device catalog][lnk-certified].</span></span>

## <a name="azure-iot-service-sdks"></a><span data-ttu-id="79237-122">Azure IoT service SDK</span><span class="sxs-lookup"><span data-stu-id="79237-122">Azure IoT service SDKs</span></span>

<span data-ttu-id="79237-123">Azure IoT service SDK innehåller kod för att underlätta skapa program som kommunicerar direkt med IoT-hubb att hantera enheter och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="79237-123">The Azure IoT service SDKs contain code to facilitate building applications that interact directly with IoT Hub to manage devices and security.</span></span>

<span data-ttu-id="79237-124">Följande Azure IoT-tjänst SDK: er kan hämtas från GitHub:</span><span class="sxs-lookup"><span data-stu-id="79237-124">The following Azure IoT service SDKs are available to download from GitHub:</span></span>

* <span data-ttu-id="79237-125">[Azure IoT-tjänsten SDK för .NET][lnk-dotnet-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-125">[Azure IoT service SDK for .NET][lnk-dotnet-service-sdk]</span></span>
* <span data-ttu-id="79237-126">[Azure IoT-tjänsten SDK för Node.js][lnk-node-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-126">[Azure IoT service SDK for Node.js][lnk-node-service-sdk]</span></span>
* <span data-ttu-id="79237-127">[Azure IoT-tjänsten SDK för Java][lnk-java-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-127">[Azure IoT service SDK for Java][lnk-java-service-sdk]</span></span>
* <span data-ttu-id="79237-128">[Azure IoT-tjänsten SDK för Python][lnk-python-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-128">[Azure IoT service SDK for Python][lnk-python-service-sdk]</span></span>
* <span data-ttu-id="79237-129">[Azure IoT-tjänsten SDK för C][lnk-c-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="79237-129">[Azure IoT service SDK for C][lnk-c-service-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="79237-130">Se Viktigt-filerna i GitHub-lagringsplatser för information om språk och plattformsspecifika paketet chefer för att installera binärfiler och beroenden på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="79237-130">See the readme files in the GitHub repositories for information about using language and platform-specific package managers to install binaries and dependencies on your development machine.</span></span>

## <a name="azure-iot-edge"></a><span data-ttu-id="79237-131">Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="79237-131">Azure IoT Edge</span></span>

<span data-ttu-id="79237-132">Azure IoT-Edge innehåller infrastruktur och moduler för att skapa lösningar för IoT-gateway.</span><span class="sxs-lookup"><span data-stu-id="79237-132">Azure IoT Edge contains the infrastructure and modules to create IoT gateway solutions.</span></span> <span data-ttu-id="79237-133">Du kan utöka IoT kant om du vill skapa gateways som är skräddarsydda för slutpunkt till slutpunkt scenarier.</span><span class="sxs-lookup"><span data-stu-id="79237-133">You can extend IoT Edge to create gateways tailored to any end-to-end scenario.</span></span>

<span data-ttu-id="79237-134">Du kan hämta [Azure IoT kant] [ lnk-iot-edge] från GitHub.</span><span class="sxs-lookup"><span data-stu-id="79237-134">You can download [Azure IoT Edge][lnk-iot-edge] from GitHub.</span></span>

## <a name="online-api-reference-documentation"></a><span data-ttu-id="79237-135">Onlinedokumentation för API-referens</span><span class="sxs-lookup"><span data-stu-id="79237-135">Online API reference documentation</span></span>

<span data-ttu-id="79237-136">Följande lista innehåller länkar till online API-referensdokumentationen för Azure IoT-enhet, tjänst och gateway-bibliotek:</span><span class="sxs-lookup"><span data-stu-id="79237-136">The following list contains links to online API reference documentation for Azure IoT device, service, and gateway libraries:</span></span>

* <span data-ttu-id="79237-137">[Sakernas (IoT) .NET Internet][lnk-dotnet-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-137">[Internet of Things (IoT) .NET][lnk-dotnet-ref]</span></span>
* <span data-ttu-id="79237-138">[IoT-hubb REST][lnk-rest-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-138">[IoT Hub REST][lnk-rest-ref]</span></span>
* <span data-ttu-id="79237-139">[Azure IoT-enhet SDK för C][lnk-c-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-139">[Azure IoT device SDK for C][lnk-c-ref]</span></span>
* <span data-ttu-id="79237-140">[Azure IoT-enhet SDK för Java][lnk-java-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-140">[Azure IoT device SDK for Java][lnk-java-ref]</span></span>
* <span data-ttu-id="79237-141">[Azure IoT-tjänsten SDK för Java][lnk-java-service-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-141">[Azure IoT service SDK for Java][lnk-java-service-ref]</span></span>
* <span data-ttu-id="79237-142">[Azure IoT-enhet SDK för Node.js][lnk-node-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-142">[Azure IoT device SDK for Node.js][lnk-node-ref]</span></span>
* <span data-ttu-id="79237-143">[Azure IoT-tjänsten SDK för Node.js][lnk-node-service-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-143">[Azure IoT service SDK for Node.js][lnk-node-service-ref]</span></span>
* <span data-ttu-id="79237-144">[Azure IoT kant][lnk-gateway-ref]</span><span class="sxs-lookup"><span data-stu-id="79237-144">[Azure IoT Edge][lnk-gateway-ref]</span></span>

## <a name="next-steps"></a><span data-ttu-id="79237-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="79237-145">Next steps</span></span>

<span data-ttu-id="79237-146">Andra referensavsnitten i den här IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="79237-146">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="79237-147">[IoT-hubbslutpunkter][lnk-devguide-endpoints]</span><span class="sxs-lookup"><span data-stu-id="79237-147">[IoT Hub endpoints][lnk-devguide-endpoints]</span></span>
* <span data-ttu-id="79237-148">[IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="79237-148">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="79237-149">[Kvoter och begränsning][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="79237-149">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="79237-150">[IoT-hubb MQTT stöd][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="79237-150">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

<!-- Links and images -->

[lnk-c-device-sdk]: https://github.com/Azure/azure-iot-sdk-c
[lnk-c-service-sdk]: https://github.com/Azure/azure-iot-sdk-c/tree/master/iothub_service_client
[lnk-dotnet-device-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/device
[lnk-java-device-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/device
[lnk-dotnet-service-sdk]: https://github.com/Azure/azure-iot-sdk-csharp/tree/master/service
[lnk-java-service-sdk]: https://github.com/Azure/azure-iot-sdk-java/tree/master/service
[lnk-node-device-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/device
[lnk-node-service-sdk]: https://github.com/Azure/azure-iot-sdk-node/tree/master/service
[lnk-python-device-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/device
[lnk-python-service-sdk]: https://github.com/Azure/azure-iot-sdk-python/tree/master/service
[lnk-certified]: https://catalog.azureiotsuite.com/
[lnk-iot-edge]: https://github.com/Azure/iot-edge

[lnk-dotnet-ref]: https://docs.microsoft.com/dotnet/api/microsoft.azure.devices
[lnk-c-ref]: https://azure.github.io/azure-iot-sdk-c/index.html
[lnk-java-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.device
[lnk-node-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-rest-ref]: https://docs.microsoft.com/rest/api/iothub/
[lnk-java-service-ref]: https://docs.microsoft.com/java/api/com.microsoft.azure.sdk.iot.service.auth
[lnk-node-service-ref]: https://azure.github.io/azure-iot-sdk-node/
[lnk-gateway-ref]: http://azure.github.io/iot-edge/api_reference/c/html/

[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md
[lnk-devguide-quotas]: iot-hub-devguide-quotas-throttling.md
[lnk-devguide-query]: iot-hub-devguide-query-language.md
[lnk-devguide-mqtt]: iot-hub-mqtt-support.md
