---
title: aaaUnderstand hello Azure IoT Hub inbyggd slutpunkt | Microsoft Docs
description: "Guide för utvecklare – beskriver hur toouse hello inbyggd, meddelanden från Event Hub-kompatibel endpoint toread enhet till moln."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.openlocfilehash: 15484c1b1828151ffcae5f4a1407264374223da1
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="read-device-to-cloud-messages-from-hello-built-in-endpoint"></a><span data-ttu-id="aeaf1-103">Läsa meddelanden från enhet till moln från hello inbyggd slutpunkt</span><span class="sxs-lookup"><span data-stu-id="aeaf1-103">Read device-to-cloud messages from hello built-in endpoint</span></span>

<span data-ttu-id="aeaf1-104">Som standard är meddelanden routade toohello inbyggd service-riktade slutpunkt (**meddelanden/händelser**), som är kompatibel med [Händelsehubbar][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="aeaf1-104">By default, messages are routed toohello built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="aeaf1-105">Den här slutpunkten är för närvarande endast exponeras med hjälp av hello [AMQP] [ lnk-amqp] -protokollet på port 5671.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-105">This endpoint is currently only exposed using hello [AMQP][lnk-amqp] protocol on port 5671.</span></span> <span data-ttu-id="aeaf1-106">En IoT-hubb visar hello följande egenskaper tooenable du toocontrol hello inbyggd Event Hub-kompatibel meddelanden slutpunkt **meddelanden/händelser**.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-106">An IoT hub exposes hello following properties tooenable you toocontrol hello built-in Event Hub-compatible messaging endpoint **messages/events**.</span></span>

| <span data-ttu-id="aeaf1-107">Egenskap</span><span class="sxs-lookup"><span data-stu-id="aeaf1-107">Property</span></span>            | <span data-ttu-id="aeaf1-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="aeaf1-108">Description</span></span> |
| ------------------- | ----------- |
| <span data-ttu-id="aeaf1-109">**Partitionsantal**</span><span class="sxs-lookup"><span data-stu-id="aeaf1-109">**Partition count**</span></span> | <span data-ttu-id="aeaf1-110">Ange den här egenskapen vid skapandet toodefine hello antalet [partitioner] [ lnk-event-hub-partitions] för enhet till moln händelse införandet.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-110">Set this property at creation toodefine hello number of [partitions][lnk-event-hub-partitions] for device-to-cloud event ingestion.</span></span> |
| <span data-ttu-id="aeaf1-111">**Kvarhållningstid**</span><span class="sxs-lookup"><span data-stu-id="aeaf1-111">**Retention time**</span></span>  | <span data-ttu-id="aeaf1-112">Den här egenskapen anger hur länge i dagar meddelanden finns kvar i IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-112">This property specifies how long in days messages are retained by IoT Hub.</span></span> <span data-ttu-id="aeaf1-113">hello standardvärdet är en dag, men det kan vara bättre tooseven dagar.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-113">hello default is one day, but it can be increased tooseven days.</span></span> |

<span data-ttu-id="aeaf1-114">IoT-hubb kan du också toomanage konsumentgrupper på hello enhet till moln i inbyggda får slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-114">IoT Hub also enables you toomanage consumer groups on hello built-in device-to-cloud receive endpoint.</span></span>

<span data-ttu-id="aeaf1-115">Som standard skrivs alla meddelanden som inte uttryckligen matchar en regel för vidarebefordran av meddelandet toohello inbyggd slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-115">By default, all messages that do not explicitly match a message routing rule are written toohello built-in endpoint.</span></span> <span data-ttu-id="aeaf1-116">Om du inaktiverar den här återställningsplats vägen släpps meddelanden som inte uttryckligen matchar alla regler för routning av meddelandet.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-116">If you disable this fallback route, messages that do not explicitly match any message routing rules are dropped.</span></span>

<span data-ttu-id="aeaf1-117">Du kan ändra hello kvarhållningstiden, antingen via programmering genom hello [IoT-hubb resursprovidern REST API: er][lnk-resource-provider-apis], eller genom att använda hello [Azure-portalen] [ lnk-management-portal].</span><span class="sxs-lookup"><span data-stu-id="aeaf1-117">You can modify hello retention time, either programmatically through hello [IoT Hub resource provider REST APIs][lnk-resource-provider-apis], or by using hello [Azure portal][lnk-management-portal].</span></span>

<span data-ttu-id="aeaf1-118">IoT-hubb visar hello **meddelanden/händelser** inbyggd slutpunkt för din serverdel services tooread hello enhet till moln har tagits emot av din hubb.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-118">IoT Hub exposes hello **messages/events** built-in endpoint for your back-end services tooread hello device-to-cloud messages received by your hub.</span></span> <span data-ttu-id="aeaf1-119">Den här slutpunkten är Event Hub-kompatibel, vilket gör att du toouse någon hello mekanismer hello händelsehubbtjänsten har stöd för att läsa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-119">This endpoint is Event Hub-compatible, which enables you toouse any of hello mechanisms hello Event Hubs service supports for reading messages.</span></span>

## <a name="read-from-hello-built-in-endpoint"></a><span data-ttu-id="aeaf1-120">Läsa från hello inbyggd slutpunkt</span><span class="sxs-lookup"><span data-stu-id="aeaf1-120">Read from hello built-in endpoint</span></span>

<span data-ttu-id="aeaf1-121">När du använder hello [Azure Service Bus SDK för .NET] [ lnk-servicebus-sdk] eller hello [Händelsehubbar - värd för händelsebearbetning][lnk-eventprocessorhost], du kan använda alla IoT-hubb-anslutning strängar med hello rätt behörigheter.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-121">When you use hello [Azure Service Bus SDK for .NET][lnk-servicebus-sdk] or hello [Event Hubs - Event Processor Host][lnk-eventprocessorhost], you can use any IoT Hub connection strings with hello correct permissions.</span></span> <span data-ttu-id="aeaf1-122">Använd sedan **meddelanden/händelser** som hello Event Hub-namn.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-122">Then use **messages/events** as hello Event Hub name.</span></span>

<span data-ttu-id="aeaf1-123">När du använder SDK: er (eller produkten integreringar) som inte känner till IoT-hubb, du måste hämta en Event Hub-kompatibel slutpunkt och Event Hub-kompatibelt namn från hello IoT-hubb inställningarna i hello [Azure-portalen] [ lnk-management-portal]:</span><span class="sxs-lookup"><span data-stu-id="aeaf1-123">When you use SDKs (or product integrations) that are unaware of IoT Hub, you must retrieve an Event Hub-compatible endpoint and Event Hub-compatible name from hello IoT Hub settings in hello [Azure portal][lnk-management-portal]:</span></span>

1. <span data-ttu-id="aeaf1-124">I hello IoT-hubb-bladet, klickar du på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-124">In hello IoT hub blade, click **Endpoints**.</span></span>
1. <span data-ttu-id="aeaf1-125">I hello **inbyggda slutpunkter** klickar du på **händelser**.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-125">In hello **Built-in endpoints** section, click **Events**.</span></span> <span data-ttu-id="aeaf1-126">hello bladet innehåller hello följande värden: **Event Hub-kompatibel endpoint**, **Event Hub-kompatibelt namn**, **partitioner**, **kvarhållningstiden**, och **konsumentgrupper**.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-126">hello blade contains hello following values: **Event Hub-compatible endpoint**, **Event Hub-compatible name**, **Partitions**, **Retention time**, and **Consumer groups**.</span></span>

    ![Inställningar för enhet till moln][img-eventhubcompatible]

<span data-ttu-id="aeaf1-128">Hej IoT-hubb SDK kräver hello slutpunktsnamnet för IoT-hubb som är **meddelanden/händelser** enligt hello **slutpunkter** bladet.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-128">hello IoT Hub SDK requires hello IoT Hub endpoint name, which is **messages/events** as shown in hello **Endpoints** blade.</span></span>

<span data-ttu-id="aeaf1-129">Om hello SDK som du använder kräver en **värdnamn** eller **Namespace** värde, ta bort hello schema från hello **Event Hub-kompatibel endpoint**.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-129">If hello SDK you are using requires a **Hostname** or **Namespace** value, remove hello scheme from hello **Event Hub-compatible endpoint**.</span></span> <span data-ttu-id="aeaf1-130">Om din Event Hub-kompatibel slutpunkten är till exempel **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **värdnamn** skulle vara  **iothub-ns-myiothub-1234.servicebus.windows.net**, och hello **Namespace** skulle vara **iothub-ns-myiothub-1234**.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-130">For example, if your Event Hub-compatible endpoint is **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **Hostname** would be **iothub-ns-myiothub-1234.servicebus.windows.net**, and hello **Namespace** would be **iothub-ns-myiothub-1234**.</span></span>

<span data-ttu-id="aeaf1-131">Du kan sedan använda en princip för delad åtkomst med hello **ServiceConnect** behörigheter tooconnect toohello angetts Event Hub.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-131">You can then use any shared access policy that has hello **ServiceConnect** permissions tooconnect toohello specified Event Hub.</span></span>

<span data-ttu-id="aeaf1-132">Om du behöver toobuild en Event Hub-anslutningssträngen med hjälp av hello tidigare information, Använd hello följande mönster:</span><span class="sxs-lookup"><span data-stu-id="aeaf1-132">If you need toobuild an Event Hub connection string by using hello previous information, use hello following pattern:</span></span>

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

<span data-ttu-id="aeaf1-133">hello SDK: er och integreringar som du kan använda med Event Hub-kompatibel slutpunkter som IoT-hubb exponerar innehåller hello objekt i hello följande lista:</span><span class="sxs-lookup"><span data-stu-id="aeaf1-133">hello SDKs and integrations that you can use with Event Hub-compatible endpoints that IoT Hub exposes includes hello items in hello following list:</span></span>

* <span data-ttu-id="aeaf1-134">[Java händelsehubbklient](https://github.com/hdinsight/eventhubs-client).</span><span class="sxs-lookup"><span data-stu-id="aeaf1-134">[Java Event Hubs client](https://github.com/hdinsight/eventhubs-client).</span></span>
* <span data-ttu-id="aeaf1-135">[Apache Storm-kanal](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="aeaf1-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span> <span data-ttu-id="aeaf1-136">Du kan visa hello [prata källa](https://github.com/apache/storm/tree/master/external/storm-eventhubs) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-136">You can view hello [spout source](https://github.com/apache/storm/tree/master/external/storm-eventhubs) on GitHub.</span></span>
* <span data-ttu-id="aeaf1-137">[Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span><span class="sxs-lookup"><span data-stu-id="aeaf1-137">[Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="aeaf1-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="aeaf1-138">Next steps</span></span>

<span data-ttu-id="aeaf1-139">Läs mer om IoT-hubbslutpunkter [IoT-hubbslutpunkter][lnk-endpoints].</span><span class="sxs-lookup"><span data-stu-id="aeaf1-139">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-endpoints].</span></span>

<span data-ttu-id="aeaf1-140">Hej [Kom igång] [ lnk-get-started] självstudiekurser visar hur meddelanden från enhet till moln toosend från simulerade enheter och läsa hälsningsmeddelande från hello inbyggd slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-140">hello [Get Started][lnk-get-started] tutorials show you how toosend device-to-cloud messages from simulated devices and read hello messages from hello built-in endpoint.</span></span> <span data-ttu-id="aeaf1-141">Mer information finns i hello [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="aeaf1-141">For more detail, see hello [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

<span data-ttu-id="aeaf1-142">Om du vill tooroute din enhet till moln meddelanden toocustom slutpunkter, se [använder meddelandevägar och anpassade slutpunkter för meddelanden från enhet till moln][lnk-custom].</span><span class="sxs-lookup"><span data-stu-id="aeaf1-142">If you want tooroute your device-to-cloud messages toocustom endpoints, see [Use message routes and custom endpoints for device-to-cloud messages][lnk-custom].</span></span>

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/
