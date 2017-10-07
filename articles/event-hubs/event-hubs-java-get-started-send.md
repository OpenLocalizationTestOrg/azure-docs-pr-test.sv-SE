---
title: "aaaSend händelser tooAzure Händelsehubbar använder Java | Microsoft Docs"
description: "Komma igång med att skicka tooEvent Hubs använder Java"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: core
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: ec537b8849a0cb49855e76c0c0ef4093108fe83c
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-java"></a>Skicka händelser tooAzure Händelsehubbar använder Java

## <a name="introduction"></a>Introduktion
Händelsehubbar är en mycket skalbar införandet system som kan mata in miljontals händelser per sekund som aktiverar ett program tooprocess och analysera hello stora mängder data som produceras av dina anslutna enheter och program. När uppgifterna väl samlats i en händelsehubb kan du omvandla och lagra data med hjälp av en leverantör av realtidsanalys eller lagringskluster.

Mer information finns i hello [översikt av Händelsehubbar][Event Hubs overview].

Den här kursen visar hur toosend händelser tooan händelsehubb med hjälp av ett konsolprogram i Java. tooreceive händelser med hjälp av hello Java värd för händelsebearbetning bibliotek Se [i den här artikeln](event-hubs-java-get-started-receive-eph.md), eller klicka på hello mottagande språket i hello vänstra innehållsförteckning.

I ordning toocomplete den här kursen behöver du hello följande:

* Java development environment. Den här självstudiekursen förutsätter vi att [Eclipse](https://www.eclipse.org/).
* Ett aktivt Azure-konto. <br/>Om du inte har något konto kan du skapa ett kostnadsfritt konto på bara några minuter. Mer information om den <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.

## <a name="send-messages-tooevent-hubs"></a>Skicka meddelanden tooEvent hubbar
hello Java-klientbibliotek för Händelsehubbar är tillgänglig för användning i Maven-projekt från hello [Maven centrallager](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22). Du kan referera till det här biblioteket med följande beroende deklarationen i projektfilen Maven hello:    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

För olika typer av versionsmiljöer du explicit kan hämta hello senaste publicerat JAR-filer från hello [Maven centrallager](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).  

För en enkel händelse utgivare, importera hello *com.microsoft.azure.eventhubs* paketet för hello Händelsehubbar klientklasser och hello *com.microsoft.azure.servicebus* paketet för verktyget klasserna exempel som vanliga undantag som delas med hello Azure Service Bus-klientprogramvara. 

För hello följande exempel, skapa ett nytt Maven-projekt för ett program på konsolen-gränssnittet i din favorit Java-utvecklingsmiljö. Namnge hello klassen `Send`.     

```java
import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
    public static void main(String[] args) 
            throws ServiceBusException, ExecutionException, InterruptedException, IOException
    {
```

Ersätt hello namnområde och händelsen hub-namn med hello värden som ska användas när du skapade hello händelsehubb.

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Skapa sedan en enda händelse genom att omvandla en sträng i UTF-8 byte kodningen. Sedan skapa en ny instans av Händelsehubbar klienten från hello anslutningssträngen och skicka hello-meddelande.   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Ta emot händelser med hjälp av hello EventProcessorHost](event-hubs-java-get-started-receive-eph.md)
* [Översikt av händelsehubbar][Event Hubs overview]
* [Skapa en Event Hub](event-hubs-create.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md