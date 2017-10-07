---
title: "aaaGet igång med Azure IoT Hub (Java) | Microsoft Docs"
description: "Lär dig hur toosend enhet till moln meddelanden tooAzure IoT-hubb med IoT-SDK för Java. Skapa simulerade enheten och tjänsten appar tooregister enheten, skicka meddelanden och läsa meddelanden från IoT-hubb."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 70dae4a8-0e98-4c53-b5a5-9d6963abb245
ms.service: iot-hub
ms.devlang: java
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/29/2017
ms.author: dobett
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: ac954f0522b46ed2a5b4a819bc611c13be0b9a9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-java"></a>Ansluta din enhet tooyour IoT-hubb som använder Java
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

Hello slutet av den här självstudiekursen har du tre Java konsolappar:

* **Skapa enhetsidentitet**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect din enhetsapp.
* **Läs-d2c-meddelanden**, som visar hello telemetri som skickats av din enhetsapp.
* **simulerade enheten**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar meddelandet telemetri som alla andra med hello MQTT-protokollet.

> [!NOTE]
> hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild båda toorun för appar på enheter och din lösningens serverdel.

toocomplete den här kursen behöver du hello följande:

* Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Som ett sista steg anteckna hello **primärnyckel** värde. Klicka på **slutpunkter** och hello **händelser** inbyggd slutpunkt. På hello **egenskaper** bladet antecknar hello **Event Hub-kompatibelt namn** och hello **Event Hub-kompatibel endpoint** adress. Du behöver dessa tre värden när du skapar appen **read-d2c-messages**.

![Aviseringsblad, Azure-portal, IoT Hub][6]

Nu har du skapat din IoT Hub. Du har hello IoT Hub-värdnamnet, IoT-hubb anslutningssträngen, IoT-hubb primärnyckel, Event Hub-kompatibelt namn och Event Hub-kompatibel endpoint toocomplete måste den här kursen.

## <a name="create-a-device-identity"></a>Skapa en enhetsidentitet
I det här avsnittet skapar du en Java-konsolapp som skapar en enhetsidentitet i hello identitetsregistret i din IoT-hubb. En enhet kan inte ansluta tooIoT hubb om den inte har en post i registret för hello identitet. Mer information finns i hello **Identitetsregistret** avsnitt i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity]. När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb.

1. Skapa en tom mapp med namnet iot-java-get-started. Skapa ett Maven-projekt som kallas i hello iot-java-get-started mappen **skapa enhetsidentitet** med hello följande kommando vid en kommandotolk. Observera att detta är ett enda långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigera toohello skapa enhetsidentitet mapp i en kommandotolk.

3. Med en textredigerare, öppna hello pom.xml filen hello skapa enhetsidentitet mapp och lägga till följande beroende toohello hello **beroenden** nod. Det här beroendet kan du toouse hello iot-service-klient paketet i din app:

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

5. Använd en textredigerare och öppna hello create-device-identity\src\main\java\com\mycompany\app\App.java filen.

6. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Lägg till följande variabler klassen på toohello hello **App** class, ersätter **{yourhubconnectionstring}** med hello värde din anges tidigare:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Ändra hello signaturen för hello **huvudsakliga** metoden tooinclude hello undantag på följande sätt:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Lägg till hello efter koden som hello brödtext hello **huvudsakliga** metod. Den här koden skapar en enhet med namnet *javadevice* i ditt IoT Hub-identitetsregister om den inte redan finns. Den visar hello enhets-ID och nyckel som du behöver senare:

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If hello device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If hello device already exists.
      try {
        device = registryManager.getDevice(deviceId);
      } catch (IotHubException iotf) {
        iotf.printStackTrace();
      }
    }

    // Display information about the
    // device you created.
    System.out.println("Device Id: " + device.getDeviceId());
    System.out.println("Device key: " + device.getPrimaryKey());
    ```

10. Spara och Stäng hello App.java fil.

11. toobuild hello **skapa enhetsidentitet** app med Maven, kör följande kommando i Kommandotolken hello i hello skapa enhetsidentitet mapp hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. toorun hello **skapa enhetsidentitet** app med Maven, kör följande kommando i Kommandotolken hello i hello skapa enhetsidentitet mapp hello:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Anteckna hello **enhets-ID** och **enhetsnyckel**. Du måste dessa värden senare när du skapar en app som ansluter tooIoT hubb som en enhet.

> [!NOTE]
> Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb. Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet. Om din app måste toostore andra enhetsspecifika metadata, ska det använda en app-specifik butik. Mer information finns i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Ta emot meddelanden från enheten till molnet

I det här avsnittet ska du skapa en Java-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub. En IoT-hubb Exponerar en [Händelsehubb][lnk-event-hubs-overview]-kompatibel endpoint tooenable du tooread meddelanden från enhet till moln. enkel tookeep saker, den här guiden skapar en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning. Hej [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen visar hur tooprocess enhet till moln meddelanden i större skala. Hej [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] självstudier innehåller ytterligare information om hur tooprocess meddelanden från Event Hubs och är tillämplig toohello IoT-hubb Event Hub-kompatibel slutpunkter.

> [!NOTE]
> hello använder Event Hub-kompatibel slutpunkt för att läsa meddelanden från enhet till moln alltid hello AMQP-protokollet.

1. I hello iot-java-get-started mappen du skapade i hello *skapa en enhetsidentitet* avsnittet, skapa ett Maven-projekt som kallas **lästa d2c meddelanden** med hello följande kommando vid en kommandotolk. Observera att detta är ett enda långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigera toohello lästa d2c meddelanden mapp i en kommandotolk.

3. Med en textredigerare, öppna hello pom.xml filen hello lästa d2c meddelanden mapp och lägga till följande beroende toohello hello **beroenden** nod. Det här beroendet kan du toouse eventhubs hello-klientpaketet i din app tooread från hello Event Hub-kompatibel slutpunkten:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. Spara och Stäng hello pom.xml fil.

5. Använd en textredigerare och öppna hello read-d2c-messages\src\main\java\com\mycompany\app\App.java filen.

6. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Lägg till följande klassnivå variabeln toohello hello **App** klass. Ersätt **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, och **{youreventhubcompatiblename}** med hello-värden som du antecknade tidigare:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Lägg till följande hello **receiveMessages** metoden toohello **App** klass. Den här metoden skapar en **EventHubClient** tooconnect toohello Event Hub-kompatibel endpoint-instans och asynkront skapar en **PartitionReceiver** instans tooread från en Händelsehubb partition. Den slinga kontinuerligt och skriver ut hello meddelandet visas tills hello app slutar.

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed toocreate client: " + e.getMessage());
        System.exit(1);
      }
      try {
        // Create a receiver using the
        // default Event Hubs consumer group
        // that listens for messages from now on.
        client.createReceiver(EventHubClient.DEFAULT_CONSUMER_GROUP_NAME, partitionId, Instant.now())
          .thenAccept(new Consumer<PartitionReceiver>() {
            public void accept(PartitionReceiver receiver) {
              System.out.println("** Created receiver on partition " + partitionId);
              try {
                while (true) {
                  Iterable<EventData> receivedEvents = receiver.receive(100).get();
                  int batchSize = 0;
                  if (receivedEvents != null) {
                    System.out.println("Got some evenst");
                    for (EventData receivedEvent : receivedEvents) {
                      System.out.println(String.format("Offset: %s, SeqNo: %s, EnqueueTime: %s",
                        receivedEvent.getSystemProperties().getOffset(),
                        receivedEvent.getSystemProperties().getSequenceNumber(),
                        receivedEvent.getSystemProperties().getEnqueuedTime()));
                      System.out.println(String.format("| Device ID: %s",
                        receivedEvent.getSystemProperties().get("iothub-connection-device-id")));
                      System.out.println(String.format("| Message Payload: %s",
                        new String(receivedEvent.getBytes(), Charset.defaultCharset())));
                      batchSize++;
                    }
                  }
                  System.out.println(String.format("Partition: %s, ReceivedBatch Size: %s", partitionId, batchSize));
                }
              } catch (Exception e) {
                System.out.println("Failed tooreceive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed toocreate receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > Den här metoden använder ett filter när den skapar hello mottagare så att hello mottagaren läser endast meddelanden som skickas tooIoT hubb när hello mottagaren börjar köras. Den här metoden är användbar i en testmiljö så att du kan se hello aktuella uppsättning av meddelanden. I en produktionsmiljö bör koden se till att den bearbetar alla meddelanden för hello - mer information, se hello [hur tooprocess IoT-hubb meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.

9. Ändra hello signaturen för hello **huvudsakliga** metoden tooinclude hello undantag på följande sätt:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Lägg till följande kod toohello hello **huvudsakliga** metod i hello **App** klass. Den här koden skapar hello två **EventHubClient** och **PartitionReceiver** instanser och gör att du tooclose hello app när du är klar behandlar meddelanden:

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (ServiceBusException sbe) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > Den här koden förutsätter att du skapade din IoT-hubb i (kostnadsfritt) hello F1-nivån. En kostnadsfri IoT Hub har två partitioner med namnen ”0” och ”1”.

11. Spara och Stäng hello App.java fil.

12. toobuild hello **lästa d2c meddelanden** app med Maven, kör följande kommando i Kommandotolken hello i hello lästa d2c meddelanden mappen hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Skapa en enhetsapp
I det här avsnittet skapar du en Java-konsolapp som simulerar en enhet som skickar meddelanden från enhet till moln tooan IoT-hubb.

1. Hej iot-java-get-started mappen du skapade i hello *skapa en enhetsidentitet* avsnittet, skapa ett Maven-projekt som kallas **simulerade enheten** med hello följande kommando vid en kommandotolk. Observera att detta är ett enda långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Navigera toohello simulerade enheten mapp i en kommandotolk.

3. Med en textredigerare, öppna hello pom.xml filen hello simulerade enheten mapp och lägga till följande beroenden toohello hello **beroenden** nod. Det här beroendet kan du toouse hello iothub-java-klient paketet i din app toocommunicate med IoT-hubb och tooserialize tooJSON för Java-objekt:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    <dependency>
      <groupId>com.google.code.gson</groupId>
      <artifactId>gson</artifactId>
      <version>2.3.1</version>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök][lnk-maven-device-search].

4. Spara och Stäng hello pom.xml fil.

5. Använd en textredigerare och öppna hello simulated-device\src\main\java\com\mycompany\app\App.java filen.

6. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Lägg till följande variabler klassen på toohello hello **App** klass. Ersätta **{youriothubname}** med din IoT-hubbnamnet och **{yourdevicekey}** hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt. Du kan använda toocommunicate för protokollet av MQTT, AMQP och HTTP-antingen hello med IoT-hubben.

8. Lägg till hello följande kapslade **TelemetryDataPoint** klass i hello **App** klassen toospecify hello telemetridata enheten skickar tooyour IoT-hubb:

    ```java
    private static class TelemetryDataPoint {
      public String deviceId;
      public double temperature;
      public double humidity;
   
      public String serialize() {
        Gson gson = new Gson();
        return gson.toJson(this);
      }
    }
    ```
9. Lägg till hello följande kapslade **EventCallback** klass i hello **App** klassen toodisplay hello bekräftelse status som hello IoT-hubb returnerar när den bearbetar ett meddelande från hello enhetsapp. Den här metoden meddelar även hello huvudtråden i hello app när har bearbetats hello-meddelande:
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toomessage with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Lägg till hello följande kapslade **MessageSender** klass i hello **App** klass. Hej **kör** metod i den här klassen genererar exempel telemetri data toosend tooyour IoT-hubb och väntar på en bekräftelse innan du skickar nästa hello-meddelande:

    ```java
    private static class MessageSender implements Runnable {
      public void run()  {
        try {
          double minTemperature = 20;
          double minHumidity = 60;
          Random rand = new Random();
    
          while (true) {
            double currentTemperature = minTemperature + rand.nextDouble() * 15;
            double currentHumidity = minHumidity + rand.nextDouble() * 20;
            TelemetryDataPoint telemetryDataPoint = new TelemetryDataPoint();
            telemetryDataPoint.deviceId = deviceId;
            telemetryDataPoint.temperature = currentTemperature;
            telemetryDataPoint.humidity = currentHumidity;
    
            String msgStr = telemetryDataPoint.serialize();
            Message msg = new Message(msgStr);
            msg.setProperty("temperatureAlert", (currentTemperature > 30) ? "true" : "false");
            msg.setMessageId(java.util.UUID.randomUUID().toString()); 
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

    Den här metoden skickar meddelandet ny enhet till moln en sekund efter hello IoT-hubb bekräftat tidigare hello-meddelande. hello-meddelande innehåller en JSON-serialiserade objekt med hello deviceId och slumpmässigt genererade siffror toosimulate en temperatursensor och en fuktighet sensor.

11. Ersätt hello **huvudsakliga** metod med följande kod som skapar en tråd toosend meddelanden från enhet till moln tooyour IoT-hubb hello:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER tooexit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. Spara och Stäng hello App.java fil.

13. toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip. I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].

## <a name="run-hello-apps"></a>Köra hello appar

Du är nu redo toorun hello appar.

1. Kör hello efter kommandot toobegin övervakning hello första partitionen i din IoT-hubb i en kommandotolk i hello Läs d2c mapp:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Meddelanden från Java IoT-hubb service app toomonitor enhet till moln][7]

2. Kör hello efter kommandot toobegin skicka telemetri data tooyour IoT-hubb i en kommandotolk i hello simulerade enheten mapp:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Meddelanden från Java IoT-hubb enheten app toosend enhet till moln][8]

3. Hej **användning** panelen i hello [Azure-portalen] [ lnk-portal] visar hello antalet meddelanden som skickas toohello IoT-hubb:

    ![Azure portal användning panelen visar antalet meddelanden som skickas tooIoT Hub][43]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enhetens identitet tooenable hello enheten app toosend meddelanden från enhet till moln toohello IoT-hubb. Du kan också skapat en app som visar hello som tagits emot av hello IoT-hubb.

toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:

* [Connecting your device][lnk-connect-device] (Ansluta din enhet)
* [Connecting your device][lnk-device-management] (Komma igång med enhetshantering)
* [Komma igång med Azure IoT Edge][lnk-iot-edge]

toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.
[!INCLUDE [iot-hub-get-started-next-steps](../../includes/iot-hub-get-started-next-steps.md)]

<!-- Images. -->
[6]: ./media/iot-hub-java-java-getstarted/create-iot-hub6.png
[7]: ./media/iot-hub-java-java-getstarted/runapp1.png
[8]: ./media/iot-hub-java-java-getstarted/runapp2.png
[43]: ./media/iot-hub-java-java-getstarted/usage.png

<!-- Links -->
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx

[lnk-eventhubs-tutorial]: ../event-hubs/event-hubs-csharp-ephcs-getstarted.md
[lnk-devguide-identity]: iot-hub-devguide-identity-registry.md
[lnk-event-hubs-overview]: ../event-hubs/event-hubs-overview.md
[lnk-process-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-portal]: https://portal.azure.com/

[lnk-device-management]: iot-hub-node-node-device-management-get-started.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22
