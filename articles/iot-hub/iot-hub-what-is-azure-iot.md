---
title: "Azure-lösningar för Sakernas Internet (IoT Suite) | Microsoft Docs"
description: "Översikt över ett exempel på IoT-lösningsarkitektur och hur den relaterar till enheter, Azure IoT Hub-tjänsten, Azure IoT-enhetens SDK:er, Azure IoT-tjänstens SDK:er och övriga Azure-tjänster."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: a859e379-dca7-42fa-bdf6-1125c86ad140
ms.service: iot-hub
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/16/2017
ms.author: dobett
ms.openlocfilehash: 1d54f090a0e07cd5102cb48cd70a1377845d6654
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a><span data-ttu-id="ec6dc-103">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ec6dc-103">Next steps</span></span>

<span data-ttu-id="ec6dc-104">Azure IoT Hub är en Azure-tjänst som möjliggör säker och tillförlitlig dubbelriktad kommunikation mellan serverdelen för din lösning och flera miljoner enheter.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-104">Azure IoT Hub is an Azure service that enables secure and reliable bi-directional communications between your solution back end and millions of devices.</span></span> <span data-ttu-id="ec6dc-105">Det gör att lösningens serverdel kan:</span><span class="sxs-lookup"><span data-stu-id="ec6dc-105">It enables the solution back end to:</span></span>

* <span data-ttu-id="ec6dc-106">Ta emot telemetri i stor skala från dina enheter.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-106">Receive telemetry at scale from your devices.</span></span>
* <span data-ttu-id="ec6dc-107">Vidarebefordra data från dina enheter till en dataström-händelseprocessor.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-107">Route data from your devices to a stream event processor.</span></span>
* <span data-ttu-id="ec6dc-108">Ta emot filöverföringar från enheter.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-108">Receive file uploads from devices.</span></span>
* <span data-ttu-id="ec6dc-109">Skicka meddelanden från moln till enhet för specifika enheter.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-109">Send cloud-to-device messages to specific devices.</span></span>

<span data-ttu-id="ec6dc-110">Du kan använda IoT Hub för att implementera din egen backend-lösning.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-110">You can use IoT Hub to implement your own solution back end.</span></span> <span data-ttu-id="ec6dc-111">IoT Hub tillhandahåller även ett identitetsregister som används för att etablera enheter och deras säkerhetsreferenser, samt deras behörigheter för anslutning till IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-111">In addition, IoT Hub includes an identity registry used to provision devices, their security credentials, and their rights to connect to the IoT hub.</span></span> <span data-ttu-id="ec6dc-112">Läs mer om IoT Hub i [Vad är IoT Hub?][lnk-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="ec6dc-112">To learn more about IoT Hub, see [What is IoT Hub?][lnk-iot-hub].</span></span>

<span data-ttu-id="ec6dc-113">Information om hur Azure IoT Hub tillhandahåller standardbaserad enhetshantering som hjälper dig att hantera, konfigurera och uppdatera dina enheter finns i [Översikt över enhetshantering med IoT Hub][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="ec6dc-113">To learn how Azure IoT Hub enables standards-based device management for you to remotely manage, configure, and update your devices, see [Overview of device management with IoT Hub][lnk-device-management].</span></span>

<span data-ttu-id="ec6dc-114">Om du vill implementera klientprogram på många olika enhetsspecifika maskinvaruplattformar och operativsystem kan du använda enhets-SDK:er för Azure IoT.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-114">To implement client applications on a wide variety of device hardware platforms and operating systems, you can use the Azure IoT device SDKs.</span></span> <span data-ttu-id="ec6dc-115">Enhets-SDK:erna innehåller bibliotek som gör det lätt att skicka telemetri till en IoT-hubb och att ta emot meddelanden från molnet till enheten.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-115">The device SDKs include libraries that facilitate sending telemetry to an IoT hub and receiving cloud-to-device messages.</span></span> <span data-ttu-id="ec6dc-116">När du använder enhets-SDK:erna kan du välja mellan olika nätverksprotokoll för att kommunicera med IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-116">When you use the device SDKs, you can choose from several network protocols to communicate with IoT Hub.</span></span> <span data-ttu-id="ec6dc-117">Mer information finns i [informationen om enhets-SDK:er][lnk-device-sdks].</span><span class="sxs-lookup"><span data-stu-id="ec6dc-117">To learn more, see the [information about device SDKs][lnk-device-sdks].</span></span>

<span data-ttu-id="ec6dc-118">Information om hur du börjar skriva kod och hur du kan köra några exempel finns i självstudien [Komma igång med IoT Hub][lnk-getstarted].</span><span class="sxs-lookup"><span data-stu-id="ec6dc-118">To get started writing some code and running some samples, see the [Get started with IoT Hub][lnk-getstarted] tutorial.</span></span>

<span data-ttu-id="ec6dc-119">Du kanske också är intresserad av [Azure IoT Suite][lnk-iot-suite], som är en samling förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-119">You may also be interested in [Azure IoT Suite][lnk-iot-suite], which is a collection of preconfigured solutions.</span></span> <span data-ttu-id="ec6dc-120">Med IoT Suite kan du snabbt komma igång och skala IoT-projekt för att hantera vanliga IoT-scenarier – till exempel fjärrövervakning, tillgångshantering och förebyggande underhåll.</span><span class="sxs-lookup"><span data-stu-id="ec6dc-120">IoT Suite enables you to get started quickly and scale IoT projects to address common IoT scenarios--such as remote monitoring, asset management, and predictive maintenance.</span></span>

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
