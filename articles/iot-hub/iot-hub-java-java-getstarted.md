---
title: "Komma igång med IoT Hub (Java) | Microsoft Docs"
description: "Läs hur man skickar meddelanden från enheten till molnet i Azure IoT Hub med hjälp av IoT SDK:er för Java. Skapa appar för simulerade enheter och tjänster för att registrera din enhet, skicka meddelanden och läsa meddelanden från IoT Hub."
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
ms.openlocfilehash: 5f41dd62d3f074eebe0c996a9c14bc7811d5b365
ms.sourcegitcommit: e266df9f97d04acfc4a843770fadfd8edf4fa2b7
ms.translationtype: HT
ms.contentlocale: sv-SE
ms.lasthandoff: 12/11/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a>Anslut din enhet till IoT Hub med hjälp av Java
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

I slutet av den här självstudiekursen har du tre Java-konsolappar:

* **create-device-identity**, som skapar en enhetsidentitet och en associerad säkerhetsnyckel för att ansluta din enhetsapp.
* **read-d2c-messages**, som visar telemetri som skickas av din enhetsapp.
* **simulated-device**, som ansluter till din IoT Hub med enhetsidentiteten som skapades tidigare och som skickar ett telemetrimeddelande varje sekund med hjälp av MQTT-protokollet.

> [!NOTE]
> Artikeln om [Azure IoT SDK:er][lnk-hub-sdks] innehåller information om Azure IoT SDK:er som du kan använda för att skapa båda apparna så att de kan köras på enheter och på lösningens backend-servrar.

För att kunna genomföra den här kursen behöver du följande:

* Senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html) 
* [Maven 3](https://maven.apache.org/install.html) 
* Ett aktivt Azure-konto. (Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

Som ett sista steg antecknar du **primärnyckelvärdet**. Klicka sedan på **Slutpunkter** och den inbyggda slutpunkten **Händelser**. På bladet **Egenskaper** noterar du **Händelsehubb-kompatibelt namn** och adressen i **Händelsehubb-kompatibel slutpunkt**. Du behöver dessa tre värden när du skapar appen **read-d2c-messages**.

![Aviseringsblad, Azure-portal, IoT Hub][6]

Nu har du skapat din IoT Hub. Du har IoT Hub-värdnamnet, IoT Hub-anslutningssträngen, den primära IoT Hub-nyckeln, det Händelsehubb-kompatibla namnet och den Händelsehubb-kompatibla slutpunkten som du behöver för att slutföra resten av kursen.

## <a name="create-a-device-identity"></a>Skapa en enhetsidentitet
I det här avsnittet ska du skapa en Java-konsolapp som skapar en enhetsidentitet i identitetsregistret i IoT Hub. En enhet kan inte ansluta till IoT Hub om den inte har en post i identitetsregistret. Mer information finns i avsnittet om **identitetsregistret** i [utvecklarhandboken för IoT Hub][lnk-devguide-identity]. När du kör den här konsolappen genererar det ett unikt enhets-ID och en nyckel som din enhet kan använda för att identifiera sig själv när den skickar ”enhet-till-molnet”-meddelanden till IoT Hub.

1. Skapa en tom mapp med namnet iot-java-get-started. Skapa ett Maven-projekt i mappen iot-java-get-started med namnet **create-device-identity** med hjälp av följande kommando i Kommandotolken. Observera att detta är ett enda långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Gå till mappen create-device-identity i Kommandotolken.

3. Använd en textredigerare och öppna filen pom.xml i mappen create-device-identity och lägg till följande beroende till noden **dependencies**. Det här beroendet gör att du kan använda paketet iot-service-client i din app:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter den senaste versionen av **iot-service-client** med [Maven-sökning][lnk-maven-service-search].

4. Spara och stäng filen pom.xml.

5. Använd en textredigerare och öppna filen create-device-identity\src\main\java\com\mycompany\app\App.java.

6. Lägg till följande **Import**-instruktioner i filen:

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. Lägg till följande variabler på klassnivå till klassen **App** och ersätt **{yourhubconnectionstring}** med värdena som du noterade och skrev ned tidigare:

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. Ändra signaturen för **main**-metoden och ta med undantagen som visas nedan:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. Lägg till följande kod som huvudstycket i **main**-metoden. Den här koden skapar en enhet med namnet *javadevice* i ditt IoT Hub-identitetsregister om den inte redan finns. Den visar sedan enhets-ID:t och enhetsnyckeln som du behöver senare:

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If the device already exists.
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

10. Spara och stäng filen App.java.

11. Skapa appen **create-device-identity** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen create-device-identity:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. Kör appen **create-device-identity** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen create-device-identity:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. Skriv ner **Enhets-ID** och **Enhetsnyckel**. Du behöver dessa värden senare när du skapar en app som ansluter till IoT Hub som en enhet.

> [!NOTE]
> IoT Hub-identitetsregistret lagrar bara enhetsidentiteter för att skydda åtkomsten till IoT Hub. Registret lagrar enhets-ID:n och enhetsnycklar som ska användas som säkerhetsreferenser och en aktiverad/inaktiverad-flagga som du kan använda för att inaktivera åtkomst för en enskild enhet. Om din app behöver lagra andra enhetsspecifika metadata bör den använda en appspecifik butik. Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].

## <a name="receive-device-to-cloud-messages"></a>Ta emot meddelanden från enheten till molnet

I det här avsnittet ska du skapa en Java-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub. En IoT Hub exponerar en [Event Hubs][lnk-event-hubs-overview]-kompatibel slutpunkt så att du kan läsa meddelanden från enheter till molnet. För att göra det så enkelt som möjligt skapar vi en grundläggande läsare i den härs självstudiekursen som inte passar för distributioner med hög genomströmning. Självstudiekursen [Bearbeta meddelanden från enhet till moln][lnk-process-d2c-tutorial] beskriver hur du bearbetar ”enhet till molnet”-meddelanden i hög skala. Självstudiekursen [Komma igång med Event Hubs][lnk-eventhubs-tutorial] innehåller ytterligare information om hur du bearbetar meddelanden från Event Hubs och gäller för de Event Hubs-kompatibla slutpunkterna i IoT Hub.

> [!NOTE]
> Event Hub-kompatibla slutpunkter för läsning av meddelanden från enheter till molnet använder alltid AMQP-protokollet.

1. I mappen iot-java-get-started som du skapade i avsnittet *Skapa en enhetsidentitet* skapar du ett Maven-projekt med namnet **read-d2c-messages** genom att köra följande kommando i Kommandotolken. Observera att detta är ett enda långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Gå till mappen read-d2c-messages i Kommandotolken.

3. Använd en textredigerare och öppna filen pom.xml i mappen read-d2c-messages och lägg till följande beroende till noden **dependencies**. Det här beroendet gör att du kan använda eventhubs-client-paketet i din app för att läsa från den Event Hubs-kompatibla slutpunkten:

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.15.0</version> 
    </dependency>
    ```

4. Spara och stäng filen pom.xml.

5. Öppna filen read-d2c-messages\src\main\java\com\mycompany\app\App.java i en textredigerare.

6. Lägg till följande **Import**-instruktioner i filen:

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. Lägg till följande klassnivåvariabler i klassen **App**. Ersätt **{youriothubkey}**, **{youreventhubcompatibleendpoint}** och **{youreventhubcompatiblename}** med de värden som du skrev ner tidigare:

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. Lägg till följande **receiveMessages**-metod till klassen **App**. Den här metoden skapar en **EventHubClient**-instans för att ansluta till den Event Hubs-kompatibla slutpunkten och skapar sedan en **PartitionReceiver**-instans asynkront för att läsa från Event Hubs-partitionen. Den loopar kontinuerligt och skriver ut meddelandeinformationen tills appen avslutas.

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed to create client: " + e.getMessage());
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
                    System.out.println("Got some events");
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
                System.out.println("Failed to receive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed to create receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > Den här metoden använder ett filter när den skapar mottagaren så att mottagaren endast läser meddelanden som skickas till IoT Hub efter att mottagaren har börjat köra. Den här tekniken är användbar i en testmiljö så att du kan se den aktuella uppsättningen meddelanden. I en produktionsmiljö bör koden se till att alla meddelanden bearbetas. Mer information finns i självstudiekursen [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] (Bearbeta meddelanden från enheten till molnet i IoT Hub).

9. Ändra signaturen för **main**-metoden och ta med undantaget som visas nedan:

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. Lägg till följande kod i metoden **main** i klassen **App**. Den här koden skapar två **EventHubClient**- och **PartitionReceiver**-instanser och gör att du kan stänga appen när du har bearbetat meddelandena:

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER to exit.");
    System.in.read();
    try {
      client0.closeSync();
      client1.closeSync();
      System.exit(0);
    } catch (Exception e) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > Den här koden förutsätter att du skapade IoT Hub på F1-nivån (kostnadsfri). En kostnadsfri IoT Hub har två partitioner med namnen ”0” och ”1”.

11. Spara och stäng filen App.java.

12. Skapa appen **read-d2c-messages** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen read-d2c-messages:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a>Skapa en enhetsapp
I det här avsnittet ska du skapa en Java-konsolapp som simulerar en enhet som skickar ”enhet till molnet”-meddelanden till en IoT Hub.

1. I mappen iot-java-get-started som du skapade i avsnittet *Skapa en enhetsidentitet* skapar du ett Maven-projekt med namnet **simulated-device** genom att köra följande kommando i Kommandotolken. Observera att detta är ett enda långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. Gå till mappen simulated-device i Kommandotolken.

3. Använd en textredigerare och öppna filen pom.xml i mappen simulated-device och lägg till följande beroenden till noden **dependencies**. Det här beroendet gör att du kan använda iothub-java-client-paketet i din app för att kommunicera med IoT Hub och för att serialisera Java-objekt till JSON:

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
    > Du kan söka efter den senaste versionen av **iot-device-client** med [Maven-sökning][lnk-maven-device-search].

4. Spara och stäng filen pom.xml.

5. Öppna filen simulated-device\src\main\java\com\mycompany\app\App.java i en textredigerare.

6. Lägg till följande **Import**-instruktioner i filen:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. Lägg till följande variabler på klassnivå till klassen **App**. Ersätt **{youriothubname}** med namnet på din IoT Hub och **{yourdevicekey}** med enhetsnyckelvärdet som du genererade i avsnittet *Skapa en enhetsidentitet*:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    Den här exempelappen använder variabeln **protocol** när den instantierar ett **DeviceClient**-objekt. Du kan använda antingen MQTT-, AMQP- eller HTTPS-protokollet för att kommunicera med IoT Hub.

8. Lägg till följande kapslade **TelemetryDataPoint**-klass inuti klassen **App** för att ange de telemetridata som enheten skickar till IoT Hub:

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
9. Lägg till följande kapslade **EventCallback**-klass inuti klassen **App** för att visa bekräftelsestatusen som IoT Hub returnerar när den bearbetar ett meddelande från enhetsappen. Den här metoden meddelar även huvudtråden i appen när meddelandet har bearbetats:
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded to message with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. Lägg till följande kapslade **MessageSender**-klass inuti klassen **App**. Metoden **Kör** i den här klassen genererar exempeltelemetridata som skickas till IoT Hub och väntar på en bekräftelse innan nästa meddelande skickas:

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

    Den här metoden skickar ett nytt ”enhet till molnet”-meddelande en sekund efter att IoT Hub bekräftar det föregående meddelandet. Meddelandet innehåller ett JSON-serialiserat objekt med enhets-ID:t och ett slumpmässigt genererat nummer för att simulera en temperatursensor och en fuktighetssensor.

11. Ersätt metoden **main** med följande kod som skapar en tråd för att skicka ”enhet till molnet”-meddelanden till din IoT Hub:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER to exit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. Spara och stäng filen App.java.

13. Skapa appen **simulated-device** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen simulated-device:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen. I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].

## <a name="run-the-apps"></a>Kör apparna

Nu är det dags att köra apparna.

1. Kör följande kommando i Kommandotolken i mappen read-d2c för att börja övervaka den första partitionen i din IoT Hub:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub-tjänstapp för att övervaka meddelanden från enheten till molnet][7]

2. Kör följande kommando i Kommandotolken i mappen simulated-device för att börja skicka telemetridata till din IoT Hub:

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub-enhetsapp för att skicka meddelanden från enheten till molnet][8]

3. På panelen **Användning** på [Azure Portal][lnk-portal] kan du se hur många meddelanden som har skickats till IoT Hub:

    ![Azure-portal Användningspanel som visar antalet meddelanden som har skickats till IoT Hub][43]

## <a name="next-steps"></a>Nästa steg
I den här självstudiekursen konfigurerade du en ny IoT Hub på Azure Portal och skapade sedan en enhetsidentitet i IoT-hubbens identitetsregister. Du använde den här enhetsidentiteten så att enhetsappen kunde skicka ”enhet till molnet”-meddelanden till IoT Hub. Du skapade också en app som visar meddelandena som tagits emot av IoT Hub.

Mer information om hur du kan komma igång med IoT Hub och utforska andra IoT-scenarier finns här:

* [Connecting your device][lnk-connect-device] (Ansluta din enhet)
* [Connecting your device][lnk-device-management] (Komma igång med enhetshantering)
* [Komma igång med Azure IoT Edge][lnk-iot-edge]

Självstudiekursen [Bearbeta meddelanden från enhet till moln][lnk-process-d2c-tutorial] beskriver hur du utökar din IoT-lösning och bearbetar ”enhet till molnet”-meddelanden i hög skala.
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
[lnk-iot-edge]: ../iot-edge/tutorial-simulate-device-linux.md
[lnk-connect-device]: https://azure.microsoft.com/develop/iot/
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-eventhubs-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22
