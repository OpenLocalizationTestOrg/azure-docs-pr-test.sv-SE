---
title: "Skicka händelser till Händelsehubbar i Azure med hjälp av C | Microsoft Docs"
description: "Skicka händelser till Händelsehubbar i Azure med hjälp av C"
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: c
ms.devlang: csharp
ms.topic: article
ms.date: 08/15/2017
ms.author: sethm
ms.openlocfilehash: a615ee39b6c3731cc7df366e9fabeed5219a71b4
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="send-events-to-azure-event-hubs-using-c"></a><span data-ttu-id="fe175-103">Skicka händelser till Händelsehubbar i Azure med hjälp av C</span><span class="sxs-lookup"><span data-stu-id="fe175-103">Send events to Azure Event Hubs using C</span></span>

## <a name="introduction"></a><span data-ttu-id="fe175-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="fe175-104">Introduction</span></span>
<span data-ttu-id="fe175-105">Händelsehubbar är en mycket skalbar införandet system som kan mata in miljontals händelser per sekund, aktivera ett program för att bearbeta och analysera de enorma mängder data som produceras av dina anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="fe175-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application to process and analyze the massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="fe175-106">När uppgifterna väl samlats i en händelsehubb kan du omvandla och lagra data med hjälp av en leverantör av realtidsanalys eller lagringskluster.</span><span class="sxs-lookup"><span data-stu-id="fe175-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="fe175-107">Mer information finns i [översikt av Händelsehubbar] [översikt av Händelsehubbar].</span><span class="sxs-lookup"><span data-stu-id="fe175-107">For more information, please see the [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="fe175-108">I kursen får du lära dig hur du skickar händelser till en händelsehubb med hjälp av ett konsolprogram i C. Klicka på mottagande språket i den vänstra tabellen i innehållet för att ta emot händelser.</span><span class="sxs-lookup"><span data-stu-id="fe175-108">In this tutorial, you will learn how to send events to an event hub using a console application in C. To receive events, click the appropriate receiving language in the left-hand table of contents.</span></span>

<span data-ttu-id="fe175-109">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="fe175-109">To complete this tutorial, you will need the following:</span></span>

* <span data-ttu-id="fe175-110">En C-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="fe175-110">A C development environment.</span></span> <span data-ttu-id="fe175-111">Den här självstudiekursen förutsätter vi att gcc stacken på en virtuell Azure Linux-dator med Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="fe175-111">For this tutorial, we will assume the gcc stack on an Azure Linux VM with Ubuntu 14.04.</span></span>
* <span data-ttu-id="fe175-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="fe175-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span></span>
* <span data-ttu-id="fe175-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="fe175-113">An active Azure account.</span></span> <span data-ttu-id="fe175-114">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="fe175-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="fe175-115">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="fe175-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="send-messages-to-event-hubs"></a><span data-ttu-id="fe175-116">Skicka meddelanden till Event Hubs</span><span class="sxs-lookup"><span data-stu-id="fe175-116">Send messages to Event Hubs</span></span>
<span data-ttu-id="fe175-117">I det här avsnittet skriver vi en C-app för att skicka händelser till din event hub.</span><span class="sxs-lookup"><span data-stu-id="fe175-117">In this section we write a C app to send events to your event hub.</span></span> <span data-ttu-id="fe175-118">Koden använder Proton AMQP-bibliotek från den [Apache Qpid projekt](http://qpid.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="fe175-118">The code uses the Proton AMQP library from the [Apache Qpid project](http://qpid.apache.org/).</span></span> <span data-ttu-id="fe175-119">Detta är detsamma som använder Service Bus-köer och ämnen med AMQP från C enligt [här](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span><span class="sxs-lookup"><span data-stu-id="fe175-119">This is analogous to using Service Bus queues and topics with AMQP from C as shown [here](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span></span> <span data-ttu-id="fe175-120">Mer information finns i [Qpid Proton dokumentationen](http://qpid.apache.org/proton/index.html).</span><span class="sxs-lookup"><span data-stu-id="fe175-120">For more information, see [Qpid Proton documentation](http://qpid.apache.org/proton/index.html).</span></span>

1. <span data-ttu-id="fe175-121">Från den [Qpid AMQP Messenger sidan](https://qpid.apache.org/proton/messenger.html), följ instruktionerna för att installera Qpid Proton, beroende på din miljö.</span><span class="sxs-lookup"><span data-stu-id="fe175-121">From the [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html), follow the instructions to install Qpid Proton, depending on your environment.</span></span>
2. <span data-ttu-id="fe175-122">Kompilera Proton-biblioteket genom att installera följande paket:</span><span class="sxs-lookup"><span data-stu-id="fe175-122">To compile the Proton library, install the following packages:</span></span>
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. <span data-ttu-id="fe175-123">Hämta den [Qpid Proton biblioteket](http://qpid.apache.org/proton/index.html), och extrahera, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="fe175-123">Download the [Qpid Proton library](http://qpid.apache.org/proton/index.html), and extract it, e.g.:</span></span>
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. <span data-ttu-id="fe175-124">Skapa ett build-katalog, kompilera och installera:</span><span class="sxs-lookup"><span data-stu-id="fe175-124">Create a build directory, compile and install:</span></span>
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. <span data-ttu-id="fe175-125">Skapa en ny fil med namnet i arbetskatalogen **sender.c** med följande kod.</span><span class="sxs-lookup"><span data-stu-id="fe175-125">In your work directory, create a new file called **sender.c** with the following code.</span></span> <span data-ttu-id="fe175-126">Kom ihåg att ersätta värdet för händelsehubbens namn och namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="fe175-126">Remember to substitute the value for your event hub name and namespace name.</span></span> <span data-ttu-id="fe175-127">Du måste också ersätta en URL-kodade version av nyckel för den **SendRule** skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="fe175-127">You must also substitute a URL-encoded version of the key for the **SendRule** created earlier.</span></span> <span data-ttu-id="fe175-128">Du kan URL-koda den [här](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="fe175-128">You can URL-encode it [here](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
   
    ```c
    #include "proton/message.h"
    #include "proton/messenger.h"
   
    #include <getopt.h>
    #include <proton/util.h>
    #include <sys/time.h>
    #include <stddef.h>
    #include <stdio.h>
    #include <string.h>
    #include <unistd.h>
    #include <stdlib.h>
   
    #define check(messenger)                                                     
      {                                                                          
        if(pn_messenger_errno(messenger))                                        
        {                                                                        
          printf("check\n");                                                     
          die(__FILE__, __LINE__, pn_error_text(pn_messenger_error(messenger))); 
        }                                                                        
      }  
   
    pn_timestamp_t time_now(void)
    {
      struct timeval now;
      if (gettimeofday(&now, NULL)) pn_fatal("gettimeofday failed\n");
      return ((pn_timestamp_t)now.tv_sec) * 1000 + (now.tv_usec / 1000);
    }  
   
    void die(const char *file, int line, const char *message)
    {
      printf("Dead\n");
      fprintf(stderr, "%s:%i: %s\n", file, line, message);
      exit(1);
    }
   
    int sendMessage(pn_messenger_t * messenger) {
        char * address = (char *) "amqps://SendRule:{Send Rule key}@{namespace name}.servicebus.windows.net/{event hub name}";
        char * msgtext = (char *) "Hello from C!";
   
        pn_message_t * message;
        pn_data_t * body;
        message = pn_message();
   
        pn_message_set_address(message, address);
        pn_message_set_content_type(message, (char*) "application/octect-stream");
        pn_message_set_inferred(message, true);
   
        body = pn_message_body(message);
        pn_data_put_binary(body, pn_bytes(strlen(msgtext), msgtext));
   
        pn_messenger_put(messenger, message);
        check(messenger);
        pn_messenger_send(messenger, 1);
        check(messenger);
   
        pn_message_free(message);
    }
   
    int main(int argc, char** argv) {
        printf("Press Ctrl-C to stop the sender process\n");
   
        pn_messenger_t *messenger = pn_messenger(NULL);
        pn_messenger_set_outgoing_window(messenger, 1);
        pn_messenger_start(messenger);
   
        while(true) {
            sendMessage(messenger);
            printf("Sent message\n");
            sleep(1);
        }
   
        // release messenger resources
        pn_messenger_stop(messenger);
        pn_messenger_free(messenger);
   
        return 0;
    }
    ```
6. <span data-ttu-id="fe175-129">Kompilera filen, förutsatt att **gcc**:</span><span class="sxs-lookup"><span data-stu-id="fe175-129">Compile the file, assuming **gcc**:</span></span>
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > <span data-ttu-id="fe175-130">I den här koden använder vi ett utgående fönster 1 för att tvinga meddelanden ut så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="fe175-130">In this code, we use an outgoing window of 1 to force the messages out as soon as possible.</span></span> <span data-ttu-id="fe175-131">I allmänhet kan försöka programmet batch-meddelanden för att öka genomflödet.</span><span class="sxs-lookup"><span data-stu-id="fe175-131">In general, your application should try to batch messages to increase throughput.</span></span> <span data-ttu-id="fe175-132">Finns det [Qpid AMQP Messenger sidan](https://qpid.apache.org/proton/messenger.html) information om hur du använder Qpid Proton biblioteket i den här och andra miljöer och från plattformar som bindningar har angetts (för närvarande Perl, PHP, Python eller Ruby).</span><span class="sxs-lookup"><span data-stu-id="fe175-132">See the [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html) for information about how to use the Qpid Proton library in this and other environments, and from platforms for which bindings are provided (currently Perl, PHP, Python, and Ruby).</span></span>


## <a name="next-steps"></a><span data-ttu-id="fe175-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="fe175-133">Next steps</span></span>
<span data-ttu-id="fe175-134">Du kan lära dig mer om Event Hubs genom att gå till följande länkar:</span><span class="sxs-lookup"><span data-stu-id="fe175-134">You can learn more about Event Hubs by visiting the following links:</span></span>

* [<span data-ttu-id="fe175-135">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="fe175-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md
)
* [<span data-ttu-id="fe175-136">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="fe175-136">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="fe175-137">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="fe175-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
