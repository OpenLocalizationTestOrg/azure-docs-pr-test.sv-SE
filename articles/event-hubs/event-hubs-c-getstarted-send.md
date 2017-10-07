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
# <a name="send-events-tooazure-event-hubs-using-c"></a>Skicka händelser tooAzure Händelsehubbar med hjälp av C

## <a name="introduction"></a>Introduktion
Händelsehubbar är en mycket skalbar införandet system som kan mata in miljontals händelser per sekund som aktiverar ett program tooprocess och analysera hello stora mängder data som produceras av dina anslutna enheter och program. När uppgifterna väl samlats i en händelsehubb kan du omvandla och lagra data med hjälp av en leverantör av realtidsanalys eller lagringskluster.

Mer information finns hello [översikt av Händelsehubbar] [översikt av Händelsehubbar].

I den här kursen får du lära dig hur toosend händelser tooan händelsehubb med hjälp av ett konsolprogram i C. tooreceive händelser, klicka på hello mottagande språket i hello vänstra innehållsförteckning.

toocomplete den här kursen behöver du hello följande:

* En C-utvecklingsmiljö. Den här självstudiekursen förutsätter vi att hello gcc stacken på en virtuell Azure Linux-dator med Ubuntu 14.04.
* [Microsoft Visual Studio](https://www.visualstudio.com/).
* Ett aktivt Azure-konto. Om du inte har något konto kan du skapa ett kostnadsfritt utvärderingskonto på bara några minuter. Mer information om den [kostnadsfria utvärderingsversionen av Azure](https://azure.microsoft.com/pricing/free-trial/).

## <a name="send-messages-tooevent-hubs"></a>Skicka meddelanden tooEvent hubbar
I det här avsnittet skriva vi en C app toosend händelser tooyour händelsehubb. hello koden använder hello Proton AMQP bibliotek från hello [Apache Qpid projekt](http://qpid.apache.org/). Detta är detsamma toousing Service Bus-köer och ämnen med AMQP från C enligt [här](https://code.msdn.microsoft.com/Using-Apache-Qpid-Proton-C-afd76504). Mer information finns i [Qpid Proton dokumentationen](http://qpid.apache.org/proton/index.html).

1. Från hello [Qpid AMQP Messenger sidan](https://qpid.apache.org/proton/messenger.html), följ hello instruktioner tooinstall Qpid Proton, beroende på din miljö.
2. toocompile Hej Proton bibliotek, installera hello följande paket:
   
    ```shell
    sudo apt-get install build-essential cmake uuid-dev openssl libssl-dev
    ```
3. Hämta hello [Qpid Proton biblioteket](http://qpid.apache.org/proton/index.html), och extrahera, t.ex.:
   
    ```shell
    wget http://archive.apache.org/dist/qpid/proton/0.7/qpid-proton-0.7.tar.gz
    tar xvfz qpid-proton-0.7.tar.gz
    ```
4. Skapa ett build-katalog, kompilera och installera:
   
    ```shell
    cd qpid-proton-0.7
    mkdir build
    cd build
    cmake -DCMAKE_INSTALL_PREFIX=/usr ..
    sudo make install
    ```
5. Skapa en ny fil med namnet i arbetskatalogen **sender.c** med hello följande kod. Kom ihåg toosubstitute hello värdet för händelsehubbens namn och namn för namnområdet. Du måste också ersätta en URL-kodade version av hello nyckel för hello **SendRule** skapade tidigare. Du kan URL-koda den [här](http://www.w3schools.com/tags/ref_urlencode.asp).
   
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
6. Kompilera hello-fil, förutsatt att **gcc**:
   
    ```
    gcc sender.c -o sender -lqpid-proton
    ```

    > [!NOTE]
    > I den här koden använder vi en utgående fönster för 1 tooforce hälsningsmeddelande ut så snart som möjligt. Ditt program bör generellt sett försök toobatch meddelanden tooincrease genomflöde. Se hello [Qpid AMQP Messenger sidan](https://qpid.apache.org/proton/messenger.html) information om hur toouse hello Qpid Proton biblioteket i den här och andra miljöer och från plattformar som bindningar har angetts (för närvarande Perl, PHP, Python eller Ruby).


## <a name="next-steps"></a>Nästa steg
Mer information om Händelsehubbar genom att besöka hello följande länkar:

* [Event Hubs-översikt](event-hubs-what-is-event-hubs.md
)
* [Skapa en Event Hub](event-hubs-create.md)
* [Vanliga frågor och svar om Event Hubs](event-hubs-faq.md)

<!-- Images. -->
[21]: ./media/event-hubs-c-ephcs-getstarted/run-csharp-ephcs1.png
[24]: ./media/event-hubs-c-ephcs-getstarted/receive-eph-c.png
