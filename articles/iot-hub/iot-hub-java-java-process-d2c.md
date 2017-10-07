---
title: "aaaProcess meddelanden för Azure IoT Hub-enhet till moln (Java) | Microsoft Docs"
description: "Hur meddelanden tooprocess IoT-hubb meddelanden från enhet till moln med hjälp av regler för Routning och anpassade slutpunkter toodispatch tooother backend-tjänster."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: bd9af5f9-a740-4780-a2a6-8c0e2752cf48
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.openlocfilehash: 084e84e721ca4297c4d7d6cb06a43b0bed9bce85
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="process-iot-hub-device-to-cloud-messages-java"></a>Bearbeta meddelanden från enhet till moln IoT-hubb (Java)

[!INCLUDE [iot-hub-selector-process-d2c](../../includes/iot-hub-selector-process-d2c.md)]

Azure IoT Hub är en helt hanterad tjänst som gör tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning för serverdel. Andra självstudier ([Kom igång med IoT-hubb] och [meddelanden moln till enhet med IoT-hubben][lnk-c2d]) visar hur toouse hello grundläggande enhet till moln och moln till enhet meddelandefunktioner för IoT-hubb.

Den här kursen bygger på hello koden som visas i hello [Kom igång med IoT-hubb] kursen, och visar hur toouse meddelande routning tooprocess enhet till moln på ett skalbart sätt. hello kursen visar hur tooprocess meddelanden som kräver omedelbar åtgärd från hello lösning serverdel. En enhet kan till exempel skicka larmmeddelandet som utlöser lägga till ett ärende i ett CRM-system. Däremot feed bara datapunkt meddelanden till en analytics-motor. Temperatur telemetri från en enhet som är toobe lagras för senare analys är till exempel en datapunkt meddelande.

Hello slutet av den här självstudiekursen, kan du köra tre Java konsolappar:

* **simulerade enheten**, en modifierad version av hello-app som skapats i hello [Kom igång med IoT-hubb] självstudiekursen skickar meddelanden från enhet till moln datapunkt varje sekund och interaktiva enhet till moln meddelanden var 10 sekunder. Den här appen använder hello AMQP protokollet toocommunicate med IoT-hubb.
* **Läs-d2c-meddelanden** visar hello telemetri som skickats av din enhetsapp.
* **Läs kritisk kö** tar bort hello kritiska meddelanden från hello Service Bus-kö kopplade toohello IoT-hubb.

> [!NOTE]
> IoT-hubben har SDK stöd för många vilka plattformar och språk, inklusive C, Java och JavaScript. Anvisningar för hur tooreplace hello enheten i den här självstudiekursen med en fysisk enhet och hur tooconnect enheter tooan IoT Hub, se hello [Azure IoT Developer Center].

toocomplete den här kursen behöver du hello följande:

* En fullständig fungerande version av hello [Kom igång med IoT-hubb] kursen.
* Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Ett aktivt Azure-konto. (Om du inte har ett konto kan du skapa ett [kostnadsfritt konto] [lnk-kostnadsfria-utvärderingsversion] på bara några minuter.)

Du bör ha vissa grundläggande kunskaper om [Azure Storage] och [Azure Service Bus].

## <a name="send-interactive-messages-from-a-device-app"></a>Skicka interaktiva meddelanden från en enhetsapp
I det här avsnittet kan du ändra hello enhetsapp som du skapade i hello [Kom igång med IoT-hubb] självstudiekursen toooccasionally skicka meddelanden som kräver omedelbar bearbetning.

1. Använda en redigerare tooopen hello simulated-device\src\main\java\com\mycompany\app\App.java textfil. Den här filen innehåller hello kod för hello **simulerade enheten** app som du skapade i hello [Kom igång med IoT-hubb] kursen.

2. Ersätt hello **MessageSender** klassen med följande kod hello:

    ```java
    private static class MessageSender implements Runnable {

        public void run()  {
            try {
                double minTemperature = 20;
                double minHumidity = 60;
                Random rand = new Random();

                while (true) {
                    String msgStr;
                    Message msg;
                    if (new Random().nextDouble() > 0.7) {
                        msgStr = "This is a critical message.";
                        msg = new Message(msgStr);
                        msg.setProperty("level", "critical");
                    } else {
                        double currentTemperature = minTemperature + rand.nextDouble() * 15;
                        double currentHumidity = minHumidity + rand.nextDouble() * 20; 
                        TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
                        telemetryDataPoint.deviceId = deviceId;
                        telemetryDataPoint.temperature = currentTemperature;
                        telemetryDataPoint.humidity = currentHumidity;

                        msgStr = telemetryDataPoint.serialize();
                        msg = new Message(msgStr);
                    }
                    
                    System.out.println("Sending: " + msgStr);

                    Object lockobj = new Object();
                    EventCallback callback = new EventCallback();
                    client.sendEventAsync(msg, callback, lockobj);

                    synchronized (lockobj) {
                        lockobj.wait();
                    }
                    Thread.sleep(1000);
                }
            } catch (InterruptedException e) {
                System.out.println("Finished.");
            }
        }
    }
    ```
   
    Den här metoden lägger slumpmässigt hello egenskapen `"level": "critical"` toomessages som skickas av hello enhet som simulerar ett meddelande som kräver omedelbara åtgärder med hello programmet serverdel. hello program Överför denna information i Egenskaper för hello-meddelande, i stället för i hello meddelandetexten, så att IoT-hubb kan vidarebefordra hello meddelandets toohello rätt meddelande mål.
   
   > [!NOTE]
   > Du kan använda egenskaper tooroute meddelanden för olika scenarier, inklusive kall sökväg, bearbetning, dessutom toohello varm sökväg exemplet som visas här.

2. Spara och Stäng hello simulated-device\src\main\java\com\mycompany\app\App.java fil.

    > [!NOTE]
    > Den här självstudiekursen implementerar inte några återförsöksprincip på hello ut för enkelhetens skull. I produktionskod, bör du implementera en återförsöksprincip som exponentiell backoff enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].

3. toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="add-a-queue-tooyour-iot-hub-and-route-messages-tooit"></a>Lägg till en kö tooyour IoT hub och flöde meddelanden tooit

I det här avsnittet skapar du en Service Bus-kö, Anslut den tooyour IoT-hubb och konfigurera din IoT-hubb toosend meddelanden toohello kö baserat på hello förekomsten av en egenskap på hello-meddelande. Mer information om hur tooprocess meddelanden från Service Bus-köer finns [Kom igång med köer][lnk-sb-queues-java].

1. Skapa en Service Bus-kö som beskrivs i [Kom igång med köer][lnk-sb-queues-java]. Anteckna hello namnområde och kön namn.

2. I hello Azure-portalen, öppna din IoT-hubb och klickar på **slutpunkter**.

    ![Slutpunkter i IoT-hubb][30]

3. I hello **slutpunkter** bladet, klickar du på **Lägg till** på hello översta tooadd kön tooyour IoT-hubb. Namnet hello endpoint **CriticalQueue** och använda hello-listrutor tooselect **Service Bus-kö**hello Service Bus-namnrymd som kön finns och hello namnet på din kö. När du är klar klickar du på **spara** längst ned hello.

    ![Lägga till en slutpunkt][31]

4. Klicka på **vägar** i din IoT-hubb. Klicka på **Lägg till** hello överst i hello bladet toocreate en routningsregel som skickar meddelanden toohello kö du just lagt till. Välj **DeviceTelemetry** som hello datakälla. Ange `level="critical"` som hello villkor och välj hello kö som du just har lagt till som en anpassad slutpunkt som hello routning regeln slutpunkt. När du är klar klickar du på **spara** längst ned hello.

    ![Lägga till en väg][32]

    Kontrollera hello återställningsplats vägen anges för**på**. Den här inställningen är hello standardkonfigurationen för en IoT-hubb.

    ![Fallback-väg][33]

## <a name="optional-read-from-hello-queue-endpoint"></a>(Valfritt) Läsa från hello kön slutpunkt

Du kan om du vill läsa hälsningsmeddelande från hello kön slutpunkten genom att följa instruktionerna hello i [Kom igång med köer][lnk-sb-queues-java]. Namnet hello app **Läs kritisk kö**.

## <a name="run-hello-applications"></a>Köra hello program

Nu är du redo toorun hello tre program.

1. toorun hello **lästa d2c meddelanden** program i en kommandotolk eller shell navigera toohello Läs d2c mappen och kör hello följande kommando:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```

   ![Kör Läs-d2c-meddelanden][readd2c]

2. toorun hello **Läs kritisk kö** program i en kommandotolk eller shell navigera toohello Läs kritisk kö mappen och kör hello följande kommando:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Kör Läs kritiska meddelanden][readqueue]

3. toorun hello **simulerade enheten** app i en kommandotolk eller shell navigera toohello simulerade enheten mappen och kör hello följande kommando:

   ```cmd/sh
   mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
   ```
   
   ![Kör simulerade enheten][simulateddevice]

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig hur tooreliably skicka meddelanden från enhet till moln med hjälp av hello meddelandet routningsfunktionen IoT-hubb.

Hej [hur toosend moln till enhet meddelanden med IoT-hubb] [ lnk-c2d] visar hur toosend meddelanden tooyour enheter från din lösningens serverdel.

toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite][lnk-suite].

toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].

toolearn mer om meddelanderoutning i IoT Hub, se [skicka och ta emot meddelanden med IoT-hubben][lnk-devguide-messaging].

<!-- Images. -->
<!-- TODO: UPDATE PICTURES -->
[simulateddevice]: ./media/iot-hub-java-java-process-d2c/runsimulateddevice.png
[readd2c]: ./media/iot-hub-java-java-process-d2c/runprocessinteractive.png
[readqueue]: ./media/iot-hub-java-java-process-d2c/runprocessd2c.png

[30]: ./media/iot-hub-java-java-process-d2c/click-endpoints.png
[31]: ./media/iot-hub-java-java-process-d2c/endpoint-creation.png
[32]: ./media/iot-hub-java-java-process-d2c/route-creation.png
[33]: ./media/iot-hub-java-java-process-d2c/fallback-route.png

<!-- Links -->

[lnk-sb-queues-java]: ../service-bus-messaging/service-bus-java-how-to-use-queues.md

[Azure Storage]: https://azure.microsoft.com/documentation/services/storage/
[Azure Service Bus]: https://azure.microsoft.com/documentation/services/service-bus/

[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[lnk-devguide-messaging]: iot-hub-devguide-messaging.md
[Kom igång med IoT-hubb]: iot-hub-java-java-getstarted.md
[Azure IoT Developer Center]: https://azure.microsoft.com/develop/iot
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh675232.aspx

<!-- Links -->
[Hantering av tillfälligt fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-c2d]: iot-hub-java-java-c2d.md
[lnk-suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
