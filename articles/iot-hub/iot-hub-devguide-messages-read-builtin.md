---
title: "Förstå Azure IoT Hub inbyggd slutpunkt | Microsoft Docs"
description: "Utvecklarhandbok - beskriver hur du använder inbyggda, Event Hub-kompatibel endpoint toread enhet till moln meddelanden."
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
ms.openlocfilehash: fcc3743028e369fdc42b71887d49fb41fba2c0dc
ms.sourcegitcommit: f537befafb079256fba0529ee554c034d73f36b0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 07/11/2017
---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a><span data-ttu-id="7f753-103">Läsa meddelanden från enhet till moln från inbyggd slutpunkt</span><span class="sxs-lookup"><span data-stu-id="7f753-103">Read device-to-cloud messages from the built-in endpoint</span></span>

<span data-ttu-id="7f753-104">Som standard meddelanden dirigeras till den inbyggda tjänst-riktade slutpunkten (**meddelanden/händelser**), som är kompatibel med [Händelsehubbar][lnk-event-hubs].</span><span class="sxs-lookup"><span data-stu-id="7f753-104">By default, messages are routed to the built-in service-facing endpoint (**messages/events**), that is compatible with [Event Hubs][lnk-event-hubs].</span></span> <span data-ttu-id="7f753-105">Den här slutpunkten är bara synliga använder den [AMQP] [ lnk-amqp] -protokollet på port 5671.</span><span class="sxs-lookup"><span data-stu-id="7f753-105">This endpoint is currently only exposed using the [AMQP][lnk-amqp] protocol on port 5671.</span></span> <span data-ttu-id="7f753-106">En IoT-hubb exponerar följande egenskaper så att du kan styra inbyggda Event Hub-kompatibel meddelanden slutpunkten **meddelanden/händelser**.</span><span class="sxs-lookup"><span data-stu-id="7f753-106">An IoT hub exposes the following properties to enable you to control the built-in Event Hub-compatible messaging endpoint **messages/events**.</span></span>

| <span data-ttu-id="7f753-107">Egenskap</span><span class="sxs-lookup"><span data-stu-id="7f753-107">Property</span></span>            | <span data-ttu-id="7f753-108">Beskrivning</span><span class="sxs-lookup"><span data-stu-id="7f753-108">Description</span></span> |
| ------------------- | ----------- |
| <span data-ttu-id="7f753-109">**Partitionsantal**</span><span class="sxs-lookup"><span data-stu-id="7f753-109">**Partition count**</span></span> | <span data-ttu-id="7f753-110">Ange den här egenskapen när skapas för att definiera antalet [partitioner] [ lnk-event-hub-partitions] för enhet till moln händelse införandet.</span><span class="sxs-lookup"><span data-stu-id="7f753-110">Set this property at creation to define the number of [partitions][lnk-event-hub-partitions] for device-to-cloud event ingestion.</span></span> |
| <span data-ttu-id="7f753-111">**Kvarhållningstid**</span><span class="sxs-lookup"><span data-stu-id="7f753-111">**Retention time**</span></span>  | <span data-ttu-id="7f753-112">Den här egenskapen anger hur länge i dagar meddelanden finns kvar i IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="7f753-112">This property specifies how long in days messages are retained by IoT Hub.</span></span> <span data-ttu-id="7f753-113">Standardvärdet är en dag, men det kan ökas till sju dagar.</span><span class="sxs-lookup"><span data-stu-id="7f753-113">The default is one day, but it can be increased to seven days.</span></span> |

<span data-ttu-id="7f753-114">IoT-hubb kan du hantera konsumentgrupper på det inbyggda enhet till molnet får slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="7f753-114">IoT Hub also enables you to manage consumer groups on the built-in device-to-cloud receive endpoint.</span></span>

<span data-ttu-id="7f753-115">Som standard skrivs alla meddelanden som inte uttryckligen matchar en regel för routning av meddelanden till inbyggd slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="7f753-115">By default, all messages that do not explicitly match a message routing rule are written to the built-in endpoint.</span></span> <span data-ttu-id="7f753-116">Om du inaktiverar den här återställningsplats vägen släpps meddelanden som inte uttryckligen matchar alla regler för routning av meddelandet.</span><span class="sxs-lookup"><span data-stu-id="7f753-116">If you disable this fallback route, messages that do not explicitly match any message routing rules are dropped.</span></span>

<span data-ttu-id="7f753-117">Du kan ändra tiden för datakvarhållning antingen programmässigt via den [IoT-hubb resursprovidern REST API: er][lnk-resource-provider-apis], eller genom att använda den [Azure-portalen][lnk-management-portal].</span><span class="sxs-lookup"><span data-stu-id="7f753-117">You can modify the retention time, either programmatically through the [IoT Hub resource provider REST APIs][lnk-resource-provider-apis], or by using the [Azure portal][lnk-management-portal].</span></span>

<span data-ttu-id="7f753-118">IoT-hubb visar den **meddelanden/händelser** inbyggd slutpunkt för dina backend-tjänster att läsa de enhet till moln har tagits emot av din hubb.</span><span class="sxs-lookup"><span data-stu-id="7f753-118">IoT Hub exposes the **messages/events** built-in endpoint for your back-end services to read the device-to-cloud messages received by your hub.</span></span> <span data-ttu-id="7f753-119">Den här slutpunkten är Event Hub-kompatibel, där du kan använda någon av av mekanismer händelsehubbtjänsten har stöd för att läsa meddelanden.</span><span class="sxs-lookup"><span data-stu-id="7f753-119">This endpoint is Event Hub-compatible, which enables you to use any of the mechanisms the Event Hubs service supports for reading messages.</span></span>

## <a name="read-from-the-built-in-endpoint"></a><span data-ttu-id="7f753-120">Läsa från inbyggd slutpunkt</span><span class="sxs-lookup"><span data-stu-id="7f753-120">Read from the built-in endpoint</span></span>

<span data-ttu-id="7f753-121">När du använder den [Azure Service Bus SDK för .NET] [ lnk-servicebus-sdk] eller [Händelsehubbar - värd för händelsebearbetning][lnk-eventprocessorhost], du kan använda alla IoT-hubb anslutningssträngar med rätt behörighet.</span><span class="sxs-lookup"><span data-stu-id="7f753-121">When you use the [Azure Service Bus SDK for .NET][lnk-servicebus-sdk] or the [Event Hubs - Event Processor Host][lnk-eventprocessorhost], you can use any IoT Hub connection strings with the correct permissions.</span></span> <span data-ttu-id="7f753-122">Använd sedan **meddelanden/händelser** som Event Hub-namn.</span><span class="sxs-lookup"><span data-stu-id="7f753-122">Then use **messages/events** as the Event Hub name.</span></span>

<span data-ttu-id="7f753-123">När du använder SDK: er (eller produkten integreringar) som inte känner till IoT-hubb, du måste hämta en Event Hub-kompatibel slutpunkt och Event Hub-kompatibelt namn från IoT-hubb inställningarna i den [Azure-portalen][lnk-management-portal]:</span><span class="sxs-lookup"><span data-stu-id="7f753-123">When you use SDKs (or product integrations) that are unaware of IoT Hub, you must retrieve an Event Hub-compatible endpoint and Event Hub-compatible name from the IoT Hub settings in the [Azure portal][lnk-management-portal]:</span></span>

1. <span data-ttu-id="7f753-124">I IoT hub-blad klickar du på **slutpunkter**.</span><span class="sxs-lookup"><span data-stu-id="7f753-124">In the IoT hub blade, click **Endpoints**.</span></span>
1. <span data-ttu-id="7f753-125">I den **inbyggda slutpunkter** klickar du på **händelser**.</span><span class="sxs-lookup"><span data-stu-id="7f753-125">In the **Built-in endpoints** section, click **Events**.</span></span> <span data-ttu-id="7f753-126">Bladet innehåller följande värden: **Event Hub-kompatibel endpoint**, **Event Hub-kompatibelt namn**, **partitioner**, **kvarhållningstiden**, och **konsumentgrupper**.</span><span class="sxs-lookup"><span data-stu-id="7f753-126">The blade contains the following values: **Event Hub-compatible endpoint**, **Event Hub-compatible name**, **Partitions**, **Retention time**, and **Consumer groups**.</span></span>

    ![Inställningar för enhet till moln][img-eventhubcompatible]

<span data-ttu-id="7f753-128">IoT-hubb SDK kräver slutpunktsnamnet IoT-hubb som är **meddelanden/händelser** enligt den **slutpunkter** bladet.</span><span class="sxs-lookup"><span data-stu-id="7f753-128">The IoT Hub SDK requires the IoT Hub endpoint name, which is **messages/events** as shown in the **Endpoints** blade.</span></span>

<span data-ttu-id="7f753-129">Om du använder SDK kräver en **värdnamn** eller **Namespace** värde, ta bort schemat från den **Event Hub-kompatibel endpoint**.</span><span class="sxs-lookup"><span data-stu-id="7f753-129">If the SDK you are using requires a **Hostname** or **Namespace** value, remove the scheme from the **Event Hub-compatible endpoint**.</span></span> <span data-ttu-id="7f753-130">Om din Event Hub-kompatibel slutpunkten är till exempel **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, **värdnamn** skulle vara **iothub-ns-myiothub-1234.servicebus.windows.net**, och **Namespace** skulle vara **iothub-ns-myiothub-1234**.</span><span class="sxs-lookup"><span data-stu-id="7f753-130">For example, if your Event Hub-compatible endpoint is **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, the **Hostname** would be **iothub-ns-myiothub-1234.servicebus.windows.net**, and the **Namespace** would be **iothub-ns-myiothub-1234**.</span></span>

<span data-ttu-id="7f753-131">Du kan sedan använda en princip för delad åtkomst som har den **ServiceConnect** behörighet att ansluta till den angivna Event Hub.</span><span class="sxs-lookup"><span data-stu-id="7f753-131">You can then use any shared access policy that has the **ServiceConnect** permissions to connect to the specified Event Hub.</span></span>

<span data-ttu-id="7f753-132">Om du behöver för att skapa en Event Hub-anslutningssträngen med hjälp av föregående informationen, använder du följande mönster:</span><span class="sxs-lookup"><span data-stu-id="7f753-132">If you need to build an Event Hub connection string by using the previous information, use the following pattern:</span></span>

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

<span data-ttu-id="7f753-133">SDK: er och integreringar som du kan använda med Event Hub-kompatibel slutpunkter som IoT-hubb exponerar innehåller objekt i listan nedan:</span><span class="sxs-lookup"><span data-stu-id="7f753-133">The SDKs and integrations that you can use with Event Hub-compatible endpoints that IoT Hub exposes includes the items in the following list:</span></span>

* <span data-ttu-id="7f753-134">[Java händelsehubbklient](https://github.com/hdinsight/eventhubs-client).</span><span class="sxs-lookup"><span data-stu-id="7f753-134">[Java Event Hubs client](https://github.com/hdinsight/eventhubs-client).</span></span>
* <span data-ttu-id="7f753-135">[Apache Storm-kanal](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span><span class="sxs-lookup"><span data-stu-id="7f753-135">[Apache Storm spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md).</span></span> <span data-ttu-id="7f753-136">Du kan visa den [prata källa](https://github.com/apache/storm/tree/master/external/storm-eventhubs) på GitHub.</span><span class="sxs-lookup"><span data-stu-id="7f753-136">You can view the [spout source](https://github.com/apache/storm/tree/master/external/storm-eventhubs) on GitHub.</span></span>
* <span data-ttu-id="7f753-137">[Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span><span class="sxs-lookup"><span data-stu-id="7f753-137">[Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).</span></span>

## <a name="next-steps"></a><span data-ttu-id="7f753-138">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="7f753-138">Next steps</span></span>

<span data-ttu-id="7f753-139">Läs mer om IoT-hubbslutpunkter [IoT-hubbslutpunkter][lnk-endpoints].</span><span class="sxs-lookup"><span data-stu-id="7f753-139">For more information about IoT Hub endpoints, see [IoT Hub endpoints][lnk-endpoints].</span></span>

<span data-ttu-id="7f753-140">Den [Kom igång] [ lnk-get-started] självstudiekurser visar hur du skickar meddelanden från enhet till moln från simulerade enheter och läsa meddelanden från den inbyggda slutpunkten.</span><span class="sxs-lookup"><span data-stu-id="7f753-140">The [Get Started][lnk-get-started] tutorials show you how to send device-to-cloud messages from simulated devices and read the messages from the built-in endpoint.</span></span> <span data-ttu-id="7f753-141">Mer information finns i [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="7f753-141">For more detail, see the [Process IoT Hub device-to-cloud messages using routes][lnk-d2c-tutorial] tutorial.</span></span>

<span data-ttu-id="7f753-142">Om du vill dirigera dina meddelanden från enhet till moln till anpassade slutpunkter, se [använder meddelandevägar och anpassade slutpunkter för meddelanden från enhet till moln][lnk-custom].</span><span class="sxs-lookup"><span data-stu-id="7f753-142">If you want to route your device-to-cloud messages to custom endpoints, see [Use message routes and custom endpoints for device-to-cloud messages][lnk-custom].</span></span>

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
