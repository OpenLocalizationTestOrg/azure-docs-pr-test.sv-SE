---
title: aaaCloud till enhet meddelanden med Azure IoT Hub (Java) | Microsoft Docs
description: "Hur meddelanden toosend moln till enhet tooa enhet från en Azure IoT-hubb med hello Azure IoT SDK för Java. Du ändrar en simulerad enhet tooreceive moln till enhet meddelanden och ändra en backend-app toosend moln till enhet hälsningsmeddelande."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 7f785ea8-e7c2-40c5-87ef-96525e9b9e1e
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: 8721f18428c849ed9a04aa2e45c65605c3e38101
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a>Skicka meddelanden moln till enhet med IoT-hubb (Java)
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas. Hej [Kom igång med IoT-hubb] kursen visar hur toocreate en IoT-hubb etablera en enhetsidentitet i den och code en simulerad enhetsapp som skickar meddelanden från enhet till moln.

Den här kursen bygger på [Kom igång med IoT-hubb]. Den visar hur till:

* Skicka meddelanden moln till enhet tooa enhet via IoT-hubb från din lösningens serverdel.
* Ta emot meddelanden moln till enhet på en enhet.
* Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas tooa enheten från IoT-hubb.

Du hittar mer information om moln till enhet meddelanden i hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].

Hello slutet av den här självstudiekursen kör du två appar som Java-konsolen:

* **simulerade enheten**, en modifierad version av hello-app som skapats i [Kom igång med IoT-hubb], som ansluter tooyour IoT-hubb och tar emot meddelanden moln till enhet.
* **Skicka c2d meddelanden**, som skickar ett moln-till-enhetsmeddelande toohello simulerade enheten appen via IoT-hubb och tar emot dess leverans bekräftelse.

> [!NOTE]
> IoT-hubben har SDK stöd för många enhetsplattformar och språk (inklusive C, Java och Javascript) via SDK för Azure IoT-enhet. Stegvisa instruktioner om hur tooconnect enhet toothis kursens kod och vanligtvis tooAzure IoT Hub, se hello [Azure IoT Developer Center].

toocomplete den här kursen behöver du hello följande:

* En fullständig fungerande version av hello [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) eller [processen IoT-hubb meddelanden från enhet till moln](iot-hub-java-java-process-d2c.md) kursen.
* Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

## <a name="receive-messages-in-hello-simulated-device-app"></a>Ta emot meddelanden i hello simulerade enhetsapp

I det här avsnittet kan du ändra hello simulerade enhetsapp som du skapade i [Kom igång med IoT-hubb] tooreceive moln till enhet meddelanden från hello IoT-hubb.

1. Använd en textredigerare och öppna hello simulated-device\src\main\java\com\mycompany\app\App.java filen.

2. Lägg till följande hello **MessageCallback** klass som en kapslad klass i hello **App** klass. Hej **köra** -metoden har anropats när hello enheten tar emot ett meddelande från IoT-hubb. I det här exemplet meddelar hello enhet alltid hello IoT-hubb som det har slutförts hello-meddelande:

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. Ändra hello **huvudsakliga** metoden toocreate en **AppMessageCallback** -instans och anrop hello **setMessageCallback** metoden innan det öppnar hello-klienten på följande sätt:

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > Om du använder HTTP i stället för MQTT eller AMQP som hello transport hello **DeviceClient** instans söker efter meddelanden från IoT-hubb sällan (mindre än var 25: e minut). Mer information om hello skillnader mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].

4. toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a>Skicka ett meddelande moln till enhet

I det här avsnittet skapar du en Java-konsolapp som skickar meddelanden moln till enhet toohello simulerade enhetsapp. Du behöver hello enhets-ID för hello-enhet som du lade till i hello [Kom igång med IoT-hubb] kursen. Du måste också hello anslutningssträngen för IoT-hubb för din hubb hittar du i hello [Azure-portalen].

1. Skapa ett Maven-projekt som kallas **skicka c2d meddelanden** med hello följande kommando vid en kommandotolk. Observera att det här kommandot är ett långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigera toohello ny skicka c2d meddelanden mapp i en kommandotolk.

3. Med en textredigerare, öppna hello pom.xml filen hello skicka c2d meddelanden mapp och lägga till följande beroende toohello hello **beroenden** nod. Lägger till hello beroende kan du toouse hello **iothub-java-service-klient** paketet i ditt program toocommunicate med din IoT-hubb-tjänst:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök][lnk-maven-service-search].

4. Spara och Stäng hello pom.xml fil.

5. Använd en textredigerare och öppna hello send-c2d-messages\src\main\java\com\mycompany\app\App.java filen.

6. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Lägg till följande variabler klassen på toohello hello **App** class, ersätter **{yourhubconnectionstring}** och **{yourdeviceid}** med hello värden din anges tidigare:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. Ersätt hello **huvudsakliga** metod med hello följande kod. Den här koden ansluter tooyour IoT-hubb skickar ett meddelande tooyour enhet och väntar sedan en bekräftelse att hello enheten tagits emot och bearbetat hello-meddelande:
   
    ```java
    public static void main(String[] args) throws IOException,
        URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(
        connectionString, protocol);
   
      if (serviceClient != null) {
        serviceClient.open();
        FeedbackReceiver feedbackReceiver = serviceClient
          .getFeedbackReceiver();
        if (feedbackReceiver != null) feedbackReceiver.open();
   
        Message messageToSend = new Message("Cloud toodevice message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent toodevice");
   
        FeedbackBatch feedbackBatch = feedbackReceiver.receive(10000);
        if (feedbackBatch != null) {
          System.out.println("Message feedback received, feedback time: "
            + feedbackBatch.getEnqueuedTimeUtc().toString());
        }
   
        if (feedbackReceiver != null) feedbackReceiver.close();
        serviceClient.close();
      }
    }
    ```

    > [!NOTE]
    > Den här självstudiekursen implementerar inte några återförsöksprincip sätt. I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].


9. toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a>Köra hello program

Du är nu redo toorun hello program.

1. Kör följande kommando toobegin skicka telemetri tooyour IoT-hubb och toolisten för moln till enhet meddelanden som skickas från din hubb hello vid en kommandotolk i hello simulerade enheten mapp:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Kör hello simulerade enhetsapp][img-simulated-device]

2. Vid en kommandotolk i hello skicka c2d meddelanden mapp, kör du hello efter kommandot toosend ett moln-till-enhetsmeddelande och vänta tills en feedback-bekräftelse:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Kör kommandot toosend hello moln till enhet hälsningsmeddelande][img-send-command]

## <a name="next-steps"></a>Nästa steg

I kursen får du lära sig hur toosend och ta emot meddelanden moln till enhet. 

toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].

toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

[Kom igång med IoT-hubb]: iot-hub-java-java-getstarted.md
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md
[Azure IoT Developer Center]: http://www.azure.com/develop/iot
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure-portalen]: https://portal.azure.com
[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22