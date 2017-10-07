---
title: "aaaSend händelser tooAzure Händelsehubbar med hjälp av C | Microsoft Docs"
description: "Skicka händelser tooAzure Händelsehubbar med hjälp av C"
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
ms.openlocfilehash: bb53300c070debb4a3658a38df9d3966f08e81ae
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-events-tooazure-event-hubs-using-c"></a><span data-ttu-id="622f8-103">Skicka händelser tooAzure Händelsehubbar med hjälp av C</span><span class="sxs-lookup"><span data-stu-id="622f8-103">Send events tooAzure Event Hubs using C</span></span>

## <a name="introduction"></a><span data-ttu-id="622f8-104">Introduktion</span><span class="sxs-lookup"><span data-stu-id="622f8-104">Introduction</span></span>
<span data-ttu-id="622f8-105">Händelsehubbar är en mycket skalbar införandet system som kan mata in miljontals händelser per sekund som aktiverar ett program tooprocess och analysera hello stora mängder data som produceras av dina anslutna enheter och program.</span><span class="sxs-lookup"><span data-stu-id="622f8-105">Event Hubs is a highly scalable ingestion system that can ingest millions of events per second, enabling an application tooprocess and analyze hello massive amounts of data produced by your connected devices and applications.</span></span> <span data-ttu-id="622f8-106">När uppgifterna väl samlats i en händelsehubb kan du omvandla och lagra data med hjälp av en leverantör av realtidsanalys eller lagringskluster.</span><span class="sxs-lookup"><span data-stu-id="622f8-106">Once collected into an event hub, you can transform and store data using any real-time analytics provider or storage cluster.</span></span>

<span data-ttu-id="622f8-107">Mer information finns hello [översikt av Händelsehubbar] [översikt av Händelsehubbar].</span><span class="sxs-lookup"><span data-stu-id="622f8-107">For more information, please see hello [Event Hubs overview][Event Hubs overview].</span></span>

<span data-ttu-id="622f8-108">I den här kursen får du lära dig hur toosend händelser tooan händelsehubb med hjälp av ett konsolprogram i C. tooreceive händelser, klicka på hello mottagande språket i hello vänstra innehållsförteckning.</span><span class="sxs-lookup"><span data-stu-id="622f8-108">In this tutorial, you will learn how toosend events tooan event hub using a console application in C. tooreceive events, click hello appropriate receiving language in hello left-hand table of contents.</span></span>

<span data-ttu-id="622f8-109">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="622f8-109">toocomplete this tutorial, you will need hello following:</span></span>

* <span data-ttu-id="622f8-110">En C-utvecklingsmiljö.</span><span class="sxs-lookup"><span data-stu-id="622f8-110">A C development environment.</span></span> <span data-ttu-id="622f8-111">Den här självstudiekursen förutsätter vi att hello gcc stacken på en virtuell Azure Linux-dator med Ubuntu 14.04.</span><span class="sxs-lookup"><span data-stu-id="622f8-111">For this tutorial, we will assume hello gcc stack on an Azure Linux VM with Ubuntu 14.04.</span></span>
* <span data-ttu-id="622f8-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span><span class="sxs-lookup"><span data-stu-id="622f8-112">[Microsoft Visual Studio](https://www.visualstudio.com/).</span></span>
* <span data-ttu-id="622f8-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="622f8-113">An active Azure account.</span></span> <span data-ttu-id="622f8-114">Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="622f8-114">If you don't have an account, you can create a free trial account in just a couple of minutes.</span></span> <span data-ttu-id="622f8-115">Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).</span><span class="sxs-lookup"><span data-stu-id="622f8-115">For details, see [Azure Free Trial](https://azure.microsoft.com/pricing/free-trial/).</span></span>

## <a name="send-messages-tooevent-hubs"></a><span data-ttu-id="622f8-116">Skicka meddelanden tooEvent hubbar</span><span class="sxs-lookup"><span data-stu-id="622f8-116">Send messages tooEvent Hubs</span></span>
<span data-ttu-id="622f8-117">I det här avsnittet skriva vi en C app toosend händelser tooyour händelsehubb.</span><span class="sxs-lookup"><span data-stu-id="622f8-117">In this section we write a C app toosend events tooyour event hub.</span></span> <span data-ttu-id="622f8-118">hello koden använder hello Proton AMQP bibliotek från hello [Apache Qpid projekt](http://qpid.apache.org/).</span><span class="sxs-lookup"><span data-stu-id="622f8-118">hello code uses hello Proton AMQP library from hello [Apache Qpid project](http://qpid.apache.org/).</span></span> <span data-ttu-id="622f8-119">Detta är detsamma toousing Service Bus-köer och ämnen med AMQP från C enligt [här](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span><span class="sxs-lookup"><span data-stu-id="622f8-119">This is analogous toousing Service Bus queues and topics with AMQP from C as shown [here](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504).</span></span> <span data-ttu-id="622f8-120">Mer information finns i [Qpid Proton dokumentationen](http://qpid.apache.org/proton/index.html).</span><span class="sxs-lookup"><span data-stu-id="622f8-120">For more information, see [Qpid Proton documentation](http://qpid.apache.org/proton/index.html).</span></span>

1. <span data-ttu-id="622f8-121">Från hello [Qpid AMQP Messenger sidan](https://qpid.apache.org/proton/messenger.html), följ hello instruktioner tooinstall Qpid Proton, beroende på din miljö.</span><span class="sxs-lookup"><span data-stu-id="622f8-121">From hello [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html), follow hello instructions tooinstall Qpid Proton, depending on your environment.</span></span>
2. <span data-ttu-id="622f8-122">toocompile Hej Proton bibliotek, installera hello följande paket:</span><span class="sxs-lookup"><span data-stu-id="622f8-122">toocompile hello Proton library, install hello following packages:</span></span>
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. <span data-ttu-id="622f8-123">Hämta hello [Qpid Proton biblioteket](http://qpid.apache.org/proton/index.html), och extrahera, t.ex.:</span><span class="sxs-lookup"><span data-stu-id="622f8-123">Download hello [Qpid Proton library](http://qpid.apache.org/proton/index.html), and extract it, e.g.:</span></span>
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. <span data-ttu-id="622f8-124">Skapa ett build-katalog, kompilera och installera:</span><span class="sxs-lookup"><span data-stu-id="622f8-124">Create a build directory, compile and install:</span></span>
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. <span data-ttu-id="622f8-125">Skapa en ny fil med namnet i arbetskatalogen **sender.c** med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="622f8-125">In your work directory, create a new file called **sender.c** with hello following code.</span></span> <span data-ttu-id="622f8-126">Kom ihåg toosubstitute hello värdet för händelsehubbens namn och namn för namnområdet.</span><span class="sxs-lookup"><span data-stu-id="622f8-126">Remember toosubstitute hello value for your event hub name and namespace name.</span></span> <span data-ttu-id="622f8-127">Du måste också ersätta en URL-kodade version av hello nyckel för hello **SendRule** skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="622f8-127">You must also substitute a URL-encoded version of hello key for hello **SendRule** created earlier.</span></span> <span data-ttu-id="622f8-128">Du kan URL-koda den [här](http://www.w3schools.com/tags/ref_urlencode.asp).</span><span class="sxs-lookup"><span data-stu-id="622f8-128">You can URL-encode it [here](http://www.w3schools.com/tags/ref_urlencode.asp).</span></span>
   
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
        printf("Press Ctrl-C toostop hello sender process\n");
   
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
6. <span data-ttu-id="622f8-129">Kompilera hello-fil, förutsatt att **gcc**:</span><span class="sxs-lookup"><span data-stu-id="622f8-129">Compile hello file, assuming **gcc**:</span></span>
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > <span data-ttu-id="622f8-130">I den här koden använder vi en utgående fönster för 1 tooforce hälsningsmeddelande ut så snart som möjligt.</span><span class="sxs-lookup"><span data-stu-id="622f8-130">In this code, we use an outgoing window of 1 tooforce hello messages out as soon as possible.</span></span> <span data-ttu-id="622f8-131">Ditt program bör generellt sett försök toobatch meddelanden tooincrease genomflöde.</span><span class="sxs-lookup"><span data-stu-id="622f8-131">In general, your application should try toobatch messages tooincrease throughput.</span></span> <span data-ttu-id="622f8-132">Se hello [Qpid AMQP Messenger sidan](https://qpid.apache.org/proton/messenger.html) information om hur toouse hello Qpid Proton biblioteket i den här och andra miljöer och från plattformar som bindningar har angetts (för närvarande Perl, PHP, Python eller Ruby).</span><span class="sxs-lookup"><span data-stu-id="622f8-132">See hello [Qpid AMQP Messenger page](https://qpid.apache.org/proton/messenger.html) for information about how toouse hello Qpid Proton library in this and other environments, and from platforms for which bindings are provided (currently Perl, PHP, Python, and Ruby).</span></span>


## <a name="next-steps"></a><span data-ttu-id="622f8-133">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="622f8-133">Next steps</span></span>
<span data-ttu-id="622f8-134">Mer information om Händelsehubbar genom att besöka hello följande länkar:</span><span class="sxs-lookup"><span data-stu-id="622f8-134">You can learn more about Event Hubs by visiting hello following links:</span></span>

* [<span data-ttu-id="622f8-135">Event Hubs-översikt</span><span class="sxs-lookup"><span data-stu-id="622f8-135">Event Hubs overview</span></span>](event-hubs-what-is-event-hubs.md
)
* [<span data-ttu-id="622f8-136">Skapa en Event Hub</span><span class="sxs-lookup"><span data-stu-id="622f8-136">Create an event hub</span></span>](event-hubs-create.md)
* [<span data-ttu-id="622f8-137">Vanliga frågor och svar om Event Hubs</span><span class="sxs-lookup"><span data-stu-id="622f8-137">Event Hubs FAQ</span></span>](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
