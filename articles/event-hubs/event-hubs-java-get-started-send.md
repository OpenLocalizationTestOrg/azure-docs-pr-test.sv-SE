---
title: "Skicka händelser till Azure Event Hubs använder Java | Microsoft Docs"
description: "Kom igång skickar till Händelsehubbar som använder Java"
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
ms.openlocfilehash: b31771001989e20b88bc8d7bca1afceb58ec197c
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="send-events-to-azure-event-hubs-using-java"></a><span data-ttu-id="2b294-103">Skicka händelser till Azure Event Hubs använder Java</span><span class="sxs-lookup"><span data-stu-id="2b294-103">Send events to Azure Event Hubs using Java</span></span>

## <a name="introduction"></a><span data-ttu-id="2b294-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="2b294-104">Introduction</span></span>
<span data-ttu-id="2b294-105">Händelsehubbar är en mycket skalbar införandet system som kan mata in miljontals händelser per sekund, aktivera ett program för att bearbeta och analysera de enorma mängder data som produceras av dina anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="2b294-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="2b294-106">När uppgifterna väl samlats i en händelsehubb kan du omvandla och lagra data med hjälp av en leverantör av realtidsanalys eller lagringskluster.</span><span class="sxs-lookup"><span data-stu-id="2b294-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="2b294-107">Mer information finns i [översikt av Händelsehubbar][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="2b294-107">For more information, see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="2b294-108">Den här kursen visar hur du skickar händelser till en händelsehubb med hjälp av ett konsolprogram i Java.</span><span class="sxs-lookup"><span data-stu-id="2b294-108">This tutorial shows how to send events to an event hub by using a console application in Java.</span></span> <span data-ttu-id="2b294-109">För att ta emot händelser med hjälp av värd för händelsebearbetning Java-bibliotek finns [i den här artikeln](event-hubs-java-get-started-receive-eph.md), eller klicka på det mottagande språket i den vänstra tabellen i innehållet.</span><span class="sxs-lookup"><span data-stu-id="2b294-109">To receive events using the Java Event Processor Host library, see [this article](event-hubs-java-get-started-receive-eph.md), or click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="2b294-110">För att kunna slutföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="2b294-110">In order to complete this tutorial, you will need the following:</span></span>

* <span data-ttu-id="2b294-111">Java development environment.</span><span class="sxs-lookup"><span data-stu-id="2b294-111">A Java development environment.</span></span> <span data-ttu-id="2b294-112">Den här självstudiekursen förutsätter vi att [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="2b294-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="2b294-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="2b294-113">An active Azure account.</span></span> <br/><span data-ttu-id="2b294-114">Om du inte har något konto kan du skapa ett kostnadsfritt konto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="2b294-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="2b294-115">Mer information om den <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="2b294-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="send-messages-to-event-hubs"></a><span data-ttu-id="2b294-116">Skicka meddelanden till Event Hubs</span><span class="sxs-lookup"><span data-stu-id="2b294-116">Send messages to Event Hubs</span></span>
<span data-ttu-id="2b294-117">Java-klientbibliotek för Händelsehubbar är tillgänglig för användning i Maven-projekt från den [Maven centrallager](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="2b294-117">The Java client library for Event Hubs is available for use in Maven projects from the [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span> <span data-ttu-id="2b294-118">Du kan referera till det här biblioteket med följande beroende deklarationen i projektfilen Maven:</span><span class="sxs-lookup"><span data-stu-id="2b294-118">You can reference this library using the following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

<span data-ttu-id="2b294-119">För olika typer av versionsmiljöer du explicit kan hämta de senaste utgivna JAR-filerna från de [Maven centrallager](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="2b294-119">For different types of build environments, you can explicitly obtain the latest released JAR files from the [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span>  

<span data-ttu-id="2b294-120">En enkel händelse utgivare och importera den *com.microsoft.azure.eventhubs* paketet för Händelsehubbar klientklasser och *com.microsoft.azure.servicebus* paketet för verktygsklasser som vanliga undantag som delas med Azure Service Bus-klientprogramvara.</span><span class="sxs-lookup"><span data-stu-id="2b294-120">For a simple event publisher, import the *com.microsoft.azure.eventhubs* package for the Event Hubs client classes and the *com.microsoft.azure.servicebus* package for utility classes such as common exceptions that are shared with the Azure Service Bus messaging client.</span></span> 

<span data-ttu-id="2b294-121">För följande exempel skapar du först ett nytt Maven-projekt för ett konsol-/gränssnittsprogram i din favorit Java Development Environment.</span><span class="sxs-lookup"><span data-stu-id="2b294-121">For the following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="2b294-122">Klassen namnet `Send`.</span><span class="sxs-lookup"><span data-stu-id="2b294-122">Name the class `Send`.</span></span>     

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

<span data-ttu-id="2b294-123">Ersätt namnen på namnområde och händelsen hubb med de värden som används när du skapade händelsehubben.</span><span class="sxs-lookup"><span data-stu-id="2b294-123">Replace the namespace and event hub names with the values used when you created the event hub.</span></span>

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

<span data-ttu-id="2b294-124">Skapa sedan en enda händelse genom att omvandla en sträng i UTF-8 byte kodningen.</span><span class="sxs-lookup"><span data-stu-id="2b294-124">Then, create a singular event by transforming a string into its UTF-8 byte encoding.</span></span> <span data-ttu-id="2b294-125">Sedan skapar en ny instans av Händelsehubbar klienten från anslutningssträngen och skicka meddelandet.</span><span class="sxs-lookup"><span data-stu-id="2b294-125">Then, create a new Event Hubs client instance from the connection string and send the message.</span></span>   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a><span data-ttu-id="2b294-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="2b294-126">Next steps</span></span>
<span data-ttu-id="2b294-127">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="2b294-127">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="2b294-128">Ta emot händelser med hjälp av EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="2b294-128">Receive events using the EventProcessorHost</span></span>](event-hubs-java-get-started-receive-eph.md)
* <span data-ttu-id="2b294-129">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="2b294-129">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="2b294-130">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="2b294-130">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="2b294-131">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="2b294-131">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md