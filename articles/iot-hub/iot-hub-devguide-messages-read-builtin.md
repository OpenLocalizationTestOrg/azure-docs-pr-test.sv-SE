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
# <a name="read-device-to-cloud-messages-from-hello-built-in-endpoint"></a>Läsa meddelanden från enhet till moln från hello inbyggd slutpunkt

Som standard är meddelanden routade toohello inbyggd service-riktade slutpunkt (**meddelanden/händelser**), som är kompatibel med [Händelsehubbar][lnk-event-hubs]. Den här slutpunkten är för närvarande endast exponeras med hjälp av hello [AMQP] [ lnk-amqp] -protokollet på port 5671. En IoT-hubb visar hello följande egenskaper tooenable du toocontrol hello inbyggd Event Hub-kompatibel meddelanden slutpunkt **meddelanden/händelser**.

| Egenskap            | Beskrivning |
| ------------------- | ----------- |
| **Partitionsantal** | Ange den här egenskapen vid skapandet toodefine hello antalet [partitioner] [ lnk-event-hub-partitions] för enhet till moln händelse införandet. |
| **Kvarhållningstid**  | Den här egenskapen anger hur länge i dagar meddelanden finns kvar i IoT Hub. hello standardvärdet är en dag, men det kan vara bättre tooseven dagar. |

IoT-hubb kan du också toomanage konsumentgrupper på hello enhet till moln i inbyggda får slutpunkt.

Som standard skrivs alla meddelanden som inte uttryckligen matchar en regel för vidarebefordran av meddelandet toohello inbyggd slutpunkt. Om du inaktiverar den här återställningsplats vägen släpps meddelanden som inte uttryckligen matchar alla regler för routning av meddelandet.

Du kan ändra hello kvarhållningstiden, antingen via programmering genom hello [IoT-hubb resursprovidern REST API: er][lnk-resource-provider-apis], eller genom att använda hello [Azure-portalen] [ lnk-management-portal].

IoT-hubb visar hello **meddelanden/händelser** inbyggd slutpunkt för din serverdel services tooread hello enhet till moln har tagits emot av din hubb. Den här slutpunkten är Event Hub-kompatibel, vilket gör att du toouse någon hello mekanismer hello händelsehubbtjänsten har stöd för att läsa meddelanden.

## <a name="read-from-hello-built-in-endpoint"></a>Läsa från hello inbyggd slutpunkt

När du använder hello [Azure Service Bus SDK för .NET] [ lnk-servicebus-sdk] eller hello [Händelsehubbar - värd för händelsebearbetning][lnk-eventprocessorhost], du kan använda alla IoT-hubb-anslutning strängar med hello rätt behörigheter. Använd sedan **meddelanden/händelser** som hello Event Hub-namn.

När du använder SDK: er (eller produkten integreringar) som inte känner till IoT-hubb, du måste hämta en Event Hub-kompatibel slutpunkt och Event Hub-kompatibelt namn från hello IoT-hubb inställningarna i hello [Azure-portalen] [ lnk-management-portal]:

1. I hello IoT-hubb-bladet, klickar du på **slutpunkter**.
1. I hello **inbyggda slutpunkter** klickar du på **händelser**. hello bladet innehåller hello följande värden: **Event Hub-kompatibel endpoint**, **Event Hub-kompatibelt namn**, **partitioner**, **kvarhållningstiden**, och **konsumentgrupper**.

    ![Inställningar för enhet till moln][img-eventhubcompatible]

Hej IoT-hubb SDK kräver hello slutpunktsnamnet för IoT-hubb som är **meddelanden/händelser** enligt hello **slutpunkter** bladet.

Om hello SDK som du använder kräver en **värdnamn** eller **Namespace** värde, ta bort hello schema från hello **Event Hub-kompatibel endpoint**. Om din Event Hub-kompatibel slutpunkten är till exempel **sb://iothub-ns-myiothub-1234.servicebus.windows.net/**, hello **värdnamn** skulle vara  **iothub-ns-myiothub-1234.servicebus.windows.net**, och hello **Namespace** skulle vara **iothub-ns-myiothub-1234**.

Du kan sedan använda en princip för delad åtkomst med hello **ServiceConnect** behörigheter tooconnect toohello angetts Event Hub.

Om du behöver toobuild en Event Hub-anslutningssträngen med hjälp av hello tidigare information, Använd hello följande mönster:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

hello SDK: er och integreringar som du kan använda med Event Hub-kompatibel slutpunkter som IoT-hubb exponerar innehåller hello objekt i hello följande lista:

* [Java händelsehubbklient](https://github.com/hdinsight/eventhubs-client).
* [Apache Storm-kanal](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Du kan visa hello [prata källa](https://github.com/apache/storm/tree/master/external/storm-eventhubs) på GitHub.
* [Apache Spark integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Nästa steg

Läs mer om IoT-hubbslutpunkter [IoT-hubbslutpunkter][lnk-endpoints].

Hej [Kom igång] [ lnk-get-started] självstudiekurser visar hur meddelanden från enhet till moln toosend från simulerade enheter och läsa hälsningsmeddelande från hello inbyggd slutpunkt. Mer information finns i hello [processen IoT Hub-enhet till moln meddelanden med hjälp av vägar] [ lnk-d2c-tutorial] kursen.

Om du vill tooroute din enhet till moln meddelanden toocustom slutpunkter, se [använder meddelandevägar och anpassade slutpunkter för meddelanden från enhet till moln][lnk-custom].

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
