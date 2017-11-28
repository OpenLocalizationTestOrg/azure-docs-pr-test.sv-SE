---
title: aaaUnderstand hello Azure IoT SDK | Microsoft Docs
description: "Utvecklarhandbok - information om och länkar toohello olika Azure IoT-enheten och tjänsten SDK: er som du kan använda toobuild enhetsappar och backend-appar."
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
ms.openlocfilehash: e319451ca97876666e1c011ee0e1a73d0a0f1ed1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="understand-and-use-azure-iot-sdks"></a><span data-ttu-id="8d619-103">Förstå och använda Azure IoT-SDK</span><span class="sxs-lookup"><span data-stu-id="8d619-103">Understand and use Azure IoT SDKs</span></span>

<span data-ttu-id="8d619-104">Det finns tre kategorier av SDK för att arbeta med IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="8d619-104">There are three categories of SDK for working with IoT Hub:</span></span>

* <span data-ttu-id="8d619-105">**Enheten SDK** aktivera toobuild appar som körs på din IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="8d619-105">**Device SDKs** enable you toobuild apps that run on your IoT devices.</span></span> <span data-ttu-id="8d619-106">De här apparna skicka telemetri tooyour IoT-hubb och du kan också ta emot meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="8d619-106">These apps send telemetry tooyour IoT hub, and optionally receive messages from your IoT hub.</span></span>

* <span data-ttu-id="8d619-107">**SDK-tjänsten** aktiverar du toomanage din IoT-hubb och (valfritt) skicka meddelanden tooyour IoT-enheter.</span><span class="sxs-lookup"><span data-stu-id="8d619-107">**Service SDKs** enable you toomanage your IoT hub, and optionally send messages tooyour IoT devices.</span></span>

* <span data-ttu-id="8d619-108">**Azure IoT-Edge** kan du toobuild gateways tooenable enheter som inte använder ett av hello stöds protokoll eller när du behöver tooprocess meddelanden på hello kant.</span><span class="sxs-lookup"><span data-stu-id="8d619-108">**Azure IoT Edge** enables you toobuild gateways tooenable devices that don't use one of hello supported protocols, or when you need tooprocess messages on hello edge.</span></span>

<span data-ttu-id="8d619-109">SDK: er angivna toosupport flera programmeringsspråk.</span><span class="sxs-lookup"><span data-stu-id="8d619-109">SDKs are provided toosupport multiple programming languages.</span></span>

## <a name="azure-iot-device-sdks"></a><span data-ttu-id="8d619-110">Azure IoT-enhet SDK</span><span class="sxs-lookup"><span data-stu-id="8d619-110">Azure IoT device SDKs</span></span>

<span data-ttu-id="8d619-111">hello Microsoft Azure IoT-enhet SDK innehåller kod som underlättar skapande av enheter och program som ansluter tooand hanteras av Azure IoT Hub-tjänster.</span><span class="sxs-lookup"><span data-stu-id="8d619-111">hello Microsoft Azure IoT device SDKs contain code that facilitates building devices and applications that connect tooand are managed by Azure IoT Hub services.</span></span>

<span data-ttu-id="8d619-112">hello är följande SDK för Azure IoT-enhet tillgänglig toodownload från GitHub:</span><span class="sxs-lookup"><span data-stu-id="8d619-112">hello following Azure IoT device SDKs are available toodownload from GitHub:</span></span>

* <span data-ttu-id="8d619-113">[Azure IoT-enhet SDK för C] [ lnk-c-device-sdk] skrivs i ANSI C (C99) för överföring och bred plattformskompatibilitet.</span><span class="sxs-lookup"><span data-stu-id="8d619-113">[Azure IoT device SDK for C][lnk-c-device-sdk] written in ANSI C (C99) for portability and broad platform compatibility.</span></span> <span data-ttu-id="8d619-114">Det finns två enhet-klientbibliotek för C, hello låg nivå **iothub_client** och hello **serialiseraren**.</span><span class="sxs-lookup"><span data-stu-id="8d619-114">There are two device client libraries for C, hello low-level **iothub_client** and hello **serializer**.</span></span>
* <span data-ttu-id="8d619-115">[Azure IoT-enhet SDK för .NET][lnk-dotnet-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-115">[Azure IoT device SDK for .NET][lnk-dotnet-device-sdk]</span></span>
* <span data-ttu-id="8d619-116">[Azure IoT-enhet SDK för Java][lnk-java-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-116">[Azure IoT device SDK for Java][lnk-java-device-sdk]</span></span>
* <span data-ttu-id="8d619-117">[Azure IoT-enhet SDK för Node.js][lnk-node-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-117">[Azure IoT device SDK for Node.js][lnk-node-device-sdk]</span></span>
* <span data-ttu-id="8d619-118">[Azure IoT-enhet SDK för Python][lnk-python-device-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-118">[Azure IoT device SDK for Python][lnk-python-device-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="8d619-119">Avsnittet hello viktigt-filer i hello GitHub-lagringsplatser för information om hur du använder språket och plattformsspecifika paketet chefer tooinstall binärfiler och beroenden på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="8d619-119">See hello readme files in hello GitHub repositories for information about using language and platform-specific package managers tooinstall binaries and dependencies on your development machine.</span></span>
> 
> 

### <a name="os-platform-and-hardware-compatibility"></a><span data-ttu-id="8d619-120">OS-plattformen och maskinvarukompatibilitet</span><span class="sxs-lookup"><span data-stu-id="8d619-120">OS platform and hardware compatibility</span></span>

<span data-ttu-id="8d619-121">Mer information om SDK-kompatibilitet med specifika maskinvaruenheter finns hello [Azure certifierad för IoT-enhet katalogen][lnk-certified].</span><span class="sxs-lookup"><span data-stu-id="8d619-121">For more information about SDK compatibility with specific hardware devices, see hello [Azure Certified for IoT device catalog][lnk-certified].</span></span>

## <a name="azure-iot-service-sdks"></a><span data-ttu-id="8d619-122">Azure IoT service SDK</span><span class="sxs-lookup"><span data-stu-id="8d619-122">Azure IoT service SDKs</span></span>

<span data-ttu-id="8d619-123">hello Azure IoT service SDK innehåller koden toofacilitate bygga program som interagerar direkt med IoT-hubb toomanage enheter och säkerhet.</span><span class="sxs-lookup"><span data-stu-id="8d619-123">hello Azure IoT service SDKs contain code toofacilitate building applications that interact directly with IoT Hub toomanage devices and security.</span></span>

<span data-ttu-id="8d619-124">hello är följande SDK för Azure IoT-tjänsten tillgänglig toodownload från GitHub:</span><span class="sxs-lookup"><span data-stu-id="8d619-124">hello following Azure IoT service SDKs are available toodownload from GitHub:</span></span>

* <span data-ttu-id="8d619-125">[Azure IoT-tjänsten SDK för .NET][lnk-dotnet-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-125">[Azure IoT service SDK for .NET][lnk-dotnet-service-sdk]</span></span>
* <span data-ttu-id="8d619-126">[Azure IoT-tjänsten SDK för Node.js][lnk-node-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-126">[Azure IoT service SDK for Node.js][lnk-node-service-sdk]</span></span>
* <span data-ttu-id="8d619-127">[Azure IoT-tjänsten SDK för Java][lnk-java-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-127">[Azure IoT service SDK for Java][lnk-java-service-sdk]</span></span>
* <span data-ttu-id="8d619-128">[Azure IoT-tjänsten SDK för Python][lnk-python-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-128">[Azure IoT service SDK for Python][lnk-python-service-sdk]</span></span>
* <span data-ttu-id="8d619-129">[Azure IoT-tjänsten SDK för C][lnk-c-service-sdk]</span><span class="sxs-lookup"><span data-stu-id="8d619-129">[Azure IoT service SDK for C][lnk-c-service-sdk]</span></span>

> [!NOTE]
> <span data-ttu-id="8d619-130">Avsnittet hello viktigt-filer i hello GitHub-lagringsplatser för information om hur du använder språket och plattformsspecifika paketet chefer tooinstall binärfiler och beroenden på utvecklingsdatorn.</span><span class="sxs-lookup"><span data-stu-id="8d619-130">See hello readme files in hello GitHub repositories for information about using language and platform-specific package managers tooinstall binaries and dependencies on your development machine.</span></span>

## <a name="azure-iot-edge"></a><span data-ttu-id="8d619-131">Azure IoT Edge</span><span class="sxs-lookup"><span data-stu-id="8d619-131">Azure IoT Edge</span></span>

<span data-ttu-id="8d619-132">Azure IoT-Edge innehåller hello infrastruktur och moduler toocreate IoT gateway-lösningar.</span><span class="sxs-lookup"><span data-stu-id="8d619-132">Azure IoT Edge contains hello infrastructure and modules toocreate IoT gateway solutions.</span></span> <span data-ttu-id="8d619-133">Du kan utöka IoT kant toocreate gateways skräddarsydd tooany slutpunkt till slutpunkt scenario.</span><span class="sxs-lookup"><span data-stu-id="8d619-133">You can extend IoT Edge toocreate gateways tailored tooany end-to-end scenario.</span></span>

<span data-ttu-id="8d619-134">Du kan hämta [Azure IoT kant] [ lnk-iot-edge] från GitHub.</span><span class="sxs-lookup"><span data-stu-id="8d619-134">You can download [Azure IoT Edge][lnk-iot-edge] from GitHub.</span></span>

## <a name="online-api-reference-documentation"></a><span data-ttu-id="8d619-135">Onlinedokumentation för API-referens</span><span class="sxs-lookup"><span data-stu-id="8d619-135">Online API reference documentation</span></span>

<span data-ttu-id="8d619-136">hello innehåller följande lista länkar tooonline API-referensdokumentationen för Azure IoT-enhet, tjänst och gateway-bibliotek:</span><span class="sxs-lookup"><span data-stu-id="8d619-136">hello following list contains links tooonline API reference documentation for Azure IoT device, service, and gateway libraries:</span></span>

* <span data-ttu-id="8d619-137">[Sakernas (IoT) .NET Internet][lnk-dotnet-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-137">[Internet of Things (IoT) .NET][lnk-dotnet-ref]</span></span>
* <span data-ttu-id="8d619-138">[IoT-hubb REST][lnk-rest-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-138">[IoT Hub REST][lnk-rest-ref]</span></span>
* <span data-ttu-id="8d619-139">[Azure IoT-enhet SDK för C][lnk-c-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-139">[Azure IoT device SDK for C][lnk-c-ref]</span></span>
* <span data-ttu-id="8d619-140">[Azure IoT-enhet SDK för Java][lnk-java-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-140">[Azure IoT device SDK for Java][lnk-java-ref]</span></span>
* <span data-ttu-id="8d619-141">[Azure IoT-tjänsten SDK för Java][lnk-java-service-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-141">[Azure IoT service SDK for Java][lnk-java-service-ref]</span></span>
* <span data-ttu-id="8d619-142">[Azure IoT-enhet SDK för Node.js][lnk-node-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-142">[Azure IoT device SDK for Node.js][lnk-node-ref]</span></span>
* <span data-ttu-id="8d619-143">[Azure IoT-tjänsten SDK för Node.js][lnk-node-service-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-143">[Azure IoT service SDK for Node.js][lnk-node-service-ref]</span></span>
* <span data-ttu-id="8d619-144">[Azure IoT kant][lnk-gateway-ref]</span><span class="sxs-lookup"><span data-stu-id="8d619-144">[Azure IoT Edge][lnk-gateway-ref]</span></span>

## <a name="next-steps"></a><span data-ttu-id="8d619-145">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="8d619-145">Next steps</span></span>

<span data-ttu-id="8d619-146">Andra referensavsnitten i den här IoT-hubb Utvecklarhandbok inkluderar:</span><span class="sxs-lookup"><span data-stu-id="8d619-146">Other reference topics in this IoT Hub developer guide include:</span></span>

* <span data-ttu-id="8d619-147">[IoT-hubbslutpunkter][lnk-devguide-endpoints]</span><span class="sxs-lookup"><span data-stu-id="8d619-147">[IoT Hub endpoints][lnk-devguide-endpoints]</span></span>
* <span data-ttu-id="8d619-148">[IoT-hubb frågespråk för enheten twins, jobb och meddelanderoutning][lnk-devguide-query]</span><span class="sxs-lookup"><span data-stu-id="8d619-148">[IoT Hub query language for device twins, jobs, and message routing][lnk-devguide-query]</span></span>
* <span data-ttu-id="8d619-149">[Kvoter och begränsning][lnk-devguide-quotas]</span><span class="sxs-lookup"><span data-stu-id="8d619-149">[Quotas and throttling][lnk-devguide-quotas]</span></span>
* <span data-ttu-id="8d619-150">[IoT-hubb MQTT stöd][lnk-devguide-mqtt]</span><span class="sxs-lookup"><span data-stu-id="8d619-150">[IoT Hub MQTT support][lnk-devguide-mqtt]</span></span>

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
