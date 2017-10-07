---
title: "aaaAzure lösningar för Sakernas Internet (IoT Suite) | Microsoft Docs"
description: "Översikt över ett exempel IoT-lösningsarkitektur och hur det relaterar toodevices, hello Azure IoT Hub-tjänsten, SDK: er för Azure IoT-enhet, SDK: er för Azure IoT-tjänsten och andra Azure-tjänster."
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
ms.openlocfilehash: 2d934e3f988c530de6a242869c021712d2aa1576
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
[!INCLUDE [iot-azure-and-iot](../../includes/iot-azure-and-iot.md)]

## <a name="next-steps"></a><span data-ttu-id="c0540-103">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="c0540-103">Next steps</span></span>

<span data-ttu-id="c0540-104">Azure IoT Hub är en Azure-tjänst som möjliggör säker och tillförlitlig dubbelriktad kommunikation mellan serverdelen för din lösning och flera miljoner enheter.</span><span class="sxs-lookup"><span data-stu-id="c0540-104">Azure IoT Hub is an Azure service that enables secure and reliable bi-directional communications between your solution back end and millions of devices.</span></span> <span data-ttu-id="c0540-105">Hello lösningens serverdel så kan:</span><span class="sxs-lookup"><span data-stu-id="c0540-105">It enables hello solution back end to:</span></span>

* <span data-ttu-id="c0540-106">Ta emot telemetri i stor skala från dina enheter.</span><span class="sxs-lookup"><span data-stu-id="c0540-106">Receive telemetry at scale from your devices.</span></span>
* <span data-ttu-id="c0540-107">Vidarebefordra data från dina enheter tooa dataströmmen Händelseprocessorn.</span><span class="sxs-lookup"><span data-stu-id="c0540-107">Route data from your devices tooa stream event processor.</span></span>
* <span data-ttu-id="c0540-108">Ta emot filöverföringar från enheter.</span><span class="sxs-lookup"><span data-stu-id="c0540-108">Receive file uploads from devices.</span></span>
* <span data-ttu-id="c0540-109">Skicka meddelanden moln till enhet toospecific enheter.</span><span class="sxs-lookup"><span data-stu-id="c0540-109">Send cloud-to-device messages toospecific devices.</span></span>

<span data-ttu-id="c0540-110">Du kan använda IoT-hubb tooimplement din egen lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="c0540-110">You can use IoT Hub tooimplement your own solution back end.</span></span> <span data-ttu-id="c0540-111">IoT-hubb innehåller dessutom en identitet registret används tooprovision enheter, deras säkerhetsreferenser och deras rättigheter tooconnect toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="c0540-111">In addition, IoT Hub includes an identity registry used tooprovision devices, their security credentials, and their rights tooconnect toohello IoT hub.</span></span> <span data-ttu-id="c0540-112">toolearn mer om IoT Hub, se [vad är IoT-hubb?] [lnk-iot-hub].</span><span class="sxs-lookup"><span data-stu-id="c0540-112">toolearn more about IoT Hub, see [What is IoT Hub?][lnk-iot-hub].</span></span>

<span data-ttu-id="c0540-113">toolearn hur Azure IoT Hub aktiverar standardbaserad enhetshantering för du tooremotely hantera, konfigurera och uppdatera dina enheter finns i [översikt över hantering av enheter med IoT-hubben][lnk-device-management].</span><span class="sxs-lookup"><span data-stu-id="c0540-113">toolearn how Azure IoT Hub enables standards-based device management for you tooremotely manage, configure, and update your devices, see [Overview of device management with IoT Hub][lnk-device-management].</span></span>

<span data-ttu-id="c0540-114">Du kan använda hello Azure IoT-enhet SDK tooimplement klientprogram på flera olika plattformar för enheter maskinvara och operativsystem.</span><span class="sxs-lookup"><span data-stu-id="c0540-114">tooimplement client applications on a wide variety of device hardware platforms and operating systems, you can use hello Azure IoT device SDKs.</span></span> <span data-ttu-id="c0540-115">hello enhet SDK innehåller bibliotek som underlättar skicka telemetri tooan IoT-hubb och ta emot moln till enhet meddelanden.</span><span class="sxs-lookup"><span data-stu-id="c0540-115">hello device SDKs include libraries that facilitate sending telemetry tooan IoT hub and receiving cloud-to-device messages.</span></span> <span data-ttu-id="c0540-116">När du använder hello enheten SDK: er, du kan välja mellan flera nätverk protokoll toocommunicate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="c0540-116">When you use hello device SDKs, you can choose from several network protocols toocommunicate with IoT Hub.</span></span> <span data-ttu-id="c0540-117">toolearn finns fler hello [information om enhet SDK][lnk-device-sdks].</span><span class="sxs-lookup"><span data-stu-id="c0540-117">toolearn more, see hello [information about device SDKs][lnk-device-sdks].</span></span>

<span data-ttu-id="c0540-118">tooget startade skriva kod och köra några exempel finns hello [Kom igång med IoT-hubb] [ lnk-getstarted] kursen.</span><span class="sxs-lookup"><span data-stu-id="c0540-118">tooget started writing some code and running some samples, see hello [Get started with IoT Hub][lnk-getstarted] tutorial.</span></span>

<span data-ttu-id="c0540-119">Du kanske också är intresserad av [Azure IoT Suite][lnk-iot-suite], som är en samling förkonfigurerade lösningar.</span><span class="sxs-lookup"><span data-stu-id="c0540-119">You may also be interested in [Azure IoT Suite][lnk-iot-suite], which is a collection of preconfigured solutions.</span></span> <span data-ttu-id="c0540-120">IoT Suite kan tooget igång snabbt och skala IoT projekt tooaddress vanliga IoT scenarier – till exempel fjärråtkomst övervakning, hantering och förutsägande underhåll.</span><span class="sxs-lookup"><span data-stu-id="c0540-120">IoT Suite enables you tooget started quickly and scale IoT projects tooaddress common IoT scenarios--such as remote monitoring, asset management, and predictive maintenance.</span></span>

[lnk-getstarted]: iot-hub-csharp-csharp-getstarted.md
[lnk-device-sdks]: https://github.com/Azure/azure-iot-sdks
[lnk-iot-hub]: iot-hub-what-is-iot-hub.md
[lnk-iot-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-iotdev]: https://azure.microsoft.com/develop/iot/
[lnk-device-management]: iot-hub-device-management-overview.md
