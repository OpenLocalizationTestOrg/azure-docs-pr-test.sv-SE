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
# <a name="send-events-tooazure-event-hubs-using-java"></a><span data-ttu-id="a4c8b-103">Skicka händelser tooAzure Händelsehubbar använder Java</span><span class="sxs-lookup"><span data-stu-id="a4c8b-103">Send events tooAzure Event Hubs using Java</span></span>

## <a name="introduction"></a><span data-ttu-id="a4c8b-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="a4c8b-104">Introduction</span></span>
<span data-ttu-id="a4c8b-105">Händelsehubbar är en mycket skalbar införandet system som kan mata in miljontals händelser per sekund som aktiverar ett program tooprocess och analysera hello stora mängder data som produceras av dina anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="a4c8b-106">När uppgifterna väl samlats i en händelsehubb kan du omvandla och lagra data med hjälp av en leverantör av realtidsanalys eller lagringskluster.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="a4c8b-107">Mer information finns i hello [översikt av Händelsehubbar][Event Hubs overview].</span><span class="sxs-lookup"><span data-stu-id="a4c8b-107">For more information, see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="a4c8b-108">Den här kursen visar hur toosend händelser tooan händelsehubb med hjälp av ett konsolprogram i Java.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-108">This tutorial shows how toosend events tooan event hub by using a console application in Java.</span></span> <span data-ttu-id="a4c8b-109">tooreceive händelser med hjälp av hello Java värd för händelsebearbetning bibliotek Se [i den här artikeln](event-hubs-java-get-started-receive-eph.md), eller klicka på hello mottagande språket i hello vänstra innehållsförteckning.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-109">tooreceive events using hello Java Event Processor Host library, see [this article](event-hubs-java-get-started-receive-eph.md), or click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="a4c8b-110">I ordning toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="a4c8b-110">In order toocomplete this tutorial, you will need hello following:</span></span>

* <span data-ttu-id="a4c8b-111">Java development environment.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-111">A Java development environment.</span></span> <span data-ttu-id="a4c8b-112">Den här självstudiekursen förutsätter vi att [Eclipse](https://www.eclipse.org/).</span><span class="sxs-lookup"><span data-stu-id="a4c8b-112">For this tutorial, we assume [Eclipse](https://www.eclipse.org/).</span></span>
* <span data-ttu-id="a4c8b-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-113">An active Azure account.</span></span> <br/><span data-ttu-id="a4c8b-114">Om du inte har något konto kan du skapa ett kostnadsfritt konto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-114">If you don't have an account, you can create a free account in just a couple of minutes.</span></span> <span data-ttu-id="a4c8b-115">Mer information om den <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">kostnadsfria utvärderingsversionen av Azure</a>.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-115">For details, see <a href="http://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A0E0E5C02&amp;returnurl=http%3A%2F%2Fazure.microsoft.com%2Fdevelop%2Fmobile%2Ftutorials%2Fget-started%2F" target="_blank">Azure Free Trial</a>.</span></span>

## <a name="send-messages-tooevent-hubs"></a><span data-ttu-id="a4c8b-116">Skicka meddelanden tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="a4c8b-116">Send messages tooEvent Hubs</span></span>
<span data-ttu-id="a4c8b-117">hello Java-klientbibliotek för Händelsehubbar är tillgänglig för användning i Maven-projekt från hello [Maven centrallager](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="a4c8b-117">hello Java client library for Event Hubs is available for use in Maven projects from hello [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span> <span data-ttu-id="a4c8b-118">Du kan referera till det här biblioteket med följande beroende deklarationen i projektfilen Maven hello:</span><span class="sxs-lookup"><span data-stu-id="a4c8b-118">You can reference this library using hello following dependency declaration inside your Maven project file:</span></span>    

```xml
<dependency>
    <groupId>com.microsoft.azure</groupId>
    <artifactId>azure-eventhubs</artifactId>
    <version>{VERSION}</version>
</dependency>
```

<span data-ttu-id="a4c8b-119">För olika typer av versionsmiljöer du explicit kan hämta hello senaste publicerat JAR-filer från hello [Maven centrallager](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span><span class="sxs-lookup"><span data-stu-id="a4c8b-119">For different types of build environments, you can explicitly obtain hello latest released JAR files from hello [Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22).</span></span>  

<span data-ttu-id="a4c8b-120">För en enkel händelse utgivare, importera hello *com.microsoft.azure.eventhubs* paketet för hello Händelsehubbar klientklasser och hello *com.microsoft.azure.servicebus* paketet för verktyget klasserna exempel som vanliga undantag som delas med hello Azure Service Bus-klientprogramvara.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-120">For a simple event publisher, import hello *com.microsoft.azure.eventhubs* package for hello Event Hubs client classes and hello *com.microsoft.azure.servicebus* package for utility classes such as common exceptions that are shared with hello Azure Service Bus messaging client.</span></span> 

<span data-ttu-id="a4c8b-121">För hello följande exempel, skapa ett nytt Maven-projekt för ett program på konsolen-gränssnittet i din favorit Java-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-121">For hello following sample, first create a new Maven project for a console/shell application in your favorite Java development environment.</span></span> <span data-ttu-id="a4c8b-122">Namnge hello klassen `Send`.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-122">Name hello class `Send`.</span></span>     

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

<span data-ttu-id="a4c8b-123">Ersätt hello namnområde och händelsen hub-namn med hello värden som ska användas när du skapade hello händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-123">Replace hello namespace and event hub names with hello values used when you created hello event hub.</span></span>

```java
    final String namespaceName = "----ServiceBusNamespaceName-----";
    final String eventHubName = "----EventHubName-----";
    final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
    final String sasKey = "---SharedAccessSignatureKey----";
    ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

<span data-ttu-id="a4c8b-124">Skapa sedan en enda händelse genom att omvandla en sträng i UTF-8 byte kodningen.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-124">Then, create a singular event by transforming a string into its UTF-8 byte encoding.</span></span> <span data-ttu-id="a4c8b-125">Sedan skapa en ny instans av Händelsehubbar klienten från hello anslutningssträngen och skicka hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="a4c8b-125">Then, create a new Event Hubs client instance from hello connection string and send hello message.</span></span>   

```java 

    byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
    EventData sendEvent = new EventData(payloadBytes);

    EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
    ehClient.sendSync(sendEvent);
    }
}

``` 

## <a name="next-steps"></a><span data-ttu-id="a4c8b-126">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="a4c8b-126">Next steps</span></span>
<span data-ttu-id="a4c8b-127">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="a4c8b-127">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="a4c8b-128">Ta emot händelser med hjälp av hello EventProcessorHost</span><span class="sxs-lookup"><span data-stu-id="a4c8b-128">Receive events using hello EventProcessorHost</span></span>](event-hubs-java-get-started-receive-eph.md)
* <span data-ttu-id="a4c8b-129">[Översikt av händelsehubbar][Event Hubs overview]</span><span class="sxs-lookup"><span data-stu-id="a4c8b-129">[Event Hubs overview][Event Hubs overview]</span></span>
* [<span data-ttu-id="a4c8b-130">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="a4c8b-130">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="a4c8b-131">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="a4c8b-131">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Links -->
[Event Hubs overview]: event-hubs-overview.md