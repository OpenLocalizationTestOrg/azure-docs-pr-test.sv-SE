---
title: "aaaGet igång med Azure IoT Hub enhetshantering (Java) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet management tooinitiate fjärranslutna enheten startas om. Du kan använda hello Azure IoT-enhet SDK för Java tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för Java tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: .java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 7aaeda9d4ff7002e5c66adfd61e2dfd5bcea964f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-java"></a>Komma igång med hantering av enheter (Java)

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

I den här självstudiekursen lär du dig att:

* Använd hello Azure portal toocreate en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.
* Skapa en simulerad enhetsapp som implementerar en direkta metoden tooreboot hello-enhet. Direkta metoder anropas från hello molnet.
* Skapa en app som anropar hello omstart direkt metod i hello simulerade enheten appen via din IoT-hubb. Den här appen och sedan Övervakare hello rapporterade egenskaper från hello enheten toosee när hello omstart åtgärden är klar.

Hello slutet av den här självstudiekursen har du två appar som Java-konsolen:

**simulerade enheten**. Den här appen:

* Ansluter tooyour IoT-hubb hello enhetsidentitet skapade tidigare.
* Tar emot ett direkt metodanrop för omstart.
* Simulerar en fysisk omstart.
* Rapporter hello tid hello senaste omstarten via en rapporterade egenskap.

**utlösaren omstart**. Den här appen:

* Anropar en direkt metod i hello simulerade enhetsapp.
* Visar hello svar toohello direkt metodanrop skickas av hello simulerade enheten
* Visar hello uppdateras rapporterade egenskaper.

> [!NOTE]
> Information om hello SDK: er som du kan använda toobuild program toorun på enheter och din lösningens serverdel finns [Azure IoT SDK][lnk-hub-sdks].

toocomplete den här kursen behöver du:

* Java SE 8. <br/> [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Java för den här självstudiekursen på Windows- eller Linux.
* Maven 3.  <br/> [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall [Maven] [ lnk-maven] för den här självstudiekursen på Windows- eller Linux.
* [Node.js-version 0.10.0 eller senare](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a>Utlös en fjärransluten omstart på hello-enhet med en direkt metod

I det här avsnittet kan skapa du en Java-konsolapp som:

1. Anropar hello omstart direkt metod i hello simulerade enhetsapp.
1. Visar hello svar.
1. Omröstningar hello rapporterade egenskaper som skickas från hello enheten toodetermine när hello omstarten är klar.

Den här appen konsolen ansluter tooyour IoT-hubb tooinvoke hello direkta metoden och Läs hello rapporteras egenskaper.

1. Skapa en tom mapp som kallas dm-get-started.

1. Skapa ett Maven-projekt som kallas i hello dm-get-started mappen **utlösaren omstart** med hello följande kommando vid en kommandotolk. hello följande visar ett långt kommando:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigera toohello utlösaren omstart mapp i en kommandotolk.

1. Med en textredigerare, öppna hello pom.xml filen hello utlösaren omstart mapp och lägga till följande beroende toohello hello **beroenden** nod. Det här beroendet kan du toouse hello iot-service-klient paketet i din app toocommunicate med IoT-hubben:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök][lnk-maven-service-search].

1. Lägg till följande hello **skapa** nod efter hello **beroenden** nod. Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Spara och Stäng hello pom.xml fil.

1. Använd en textredigerare, öppna hello trigger-reboot\src\main\java\com\mycompany\app\App.java källfilen.

1. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

1. Lägg till följande variabler klassen på toohello hello **App** klass. Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. tooimplement en tråd som läser hello rapporterade egenskaper hello enheten dubbla var 10: e sekund, lägga till följande hello kapslad klass toohello **App** klass:

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. tooinvoke hello omstart direkt metod på hello simulerade enheten, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

1. toostart hello tråd toopoll Hej rapporterade egenskaper från hello simulerade enhet, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. tooenable du toostop hello app, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. Spara och Stäng hello trigger-reboot\src\main\java\com\mycompany\app\App.java fil.

1. Skapa hello **utlösaren omstart** backend-app och korrigera eventuella fel. Navigera toohello utlösaren omstart mappen och kör hello följande kommando vid en kommandotolk:

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp

I det här avsnittet skapar du en Java-konsolapp som simulerar en enhet. hello app lyssnar efter metodanrop för hello omstart direkt från din IoT-hubb och svarar omedelbart toothat anrop. Hej appen och klicka sedan på ett tag i viloläge toosimulate hello omstart process innan den använder en rapporterade egenskapen toonotify hello **utlösaren omstart** backend-app som hello omstarten är klar.

1. Skapa ett Maven-projekt som kallas i hello dm-get-started mappen **simulerade enheten** med hello följande kommando vid en kommandotolk. hello följande är ett långt kommando:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigera toohello simulerade enheten mapp i en kommandotolk.

1. Med en textredigerare, öppna hello pom.xml filen hello simulerade enheten mapp och lägga till följande beroende toohello hello **beroenden** nod. Det här beroendet kan du toouse hello iot-service-klient paketet i din app toocommunicate med IoT-hubben:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök][lnk-maven-device-search].

1. Lägg till följande hello **skapa** nod efter hello **beroenden** nod. Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:

    ```xml
    <build>
      <plugins>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>3.3</version>
          <configuration>
            <source>1.8</source>
            <target>1.8</target>
          </configuration>
        </plugin>
      </plugins>
    </build>
    ```

1. Spara och Stäng hello pom.xml fil.

1. Använd en textredigerare, öppna hello simulated-device\src\main\java\com\mycompany\app\App.java källfilen.

1. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

1. Lägg till följande variabler klassen på toohello hello **App** klass. Ersätt `{yourdeviceconnectionstring}` med hello enheten anslutningssträng som du antecknade i hello *skapa en enhetsidentitet* avsnitt:

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. tooimplement en motringning hanterare för direkta metoden statushändelser, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. tooimplement en motringning hanterare för enheten dubbla statushändelser, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
        }
    }
    ```

1. tooimplement en motringning hanterare för egenskapen händelser, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. tooimplement en tråd toosimulate hello enheten omstart, Lägg till följande hello kapslad klass toohello **App** klass. hello tråd i viloläge i fem sekunder och sedan anger hello **lastReboot** rapporterade egenskapen:

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. tooimplement hello direkt metod på hello enhet Lägg till följande hello kapslad klass toohello **App** klass. När hello simulerade appen tar emot ett samtal toohello **omstart** direkta metoden returnerar en bekräftelse toohello anroparen och sedan startar en tråd tooprocess hello omstart:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
            break;
          }
          default:
          {
            int status = METHOD_NOT_DEFINED;
            deviceMethodData = new DeviceMethodData(status, "Not defined direct method " + methodName);
          }
        }
        return deviceMethodData;
      }
    }
    ```

1. Ändra hello signaturen för hello **huvudsakliga** metoden toothrow hello följande undantag:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. tooinstantiate en **DeviceClient**, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. toostart lyssnar efter direkta metodanrop, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed toodirect methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. tooshut ned hello enheten simulatorn Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. Spara och Stäng hello simulated-device\src\main\java\com\mycompany\app\App.java fil.

1. Skapa hello **simulerade enheten** backend-app och korrigera eventuella fel. Navigera toohello simulerade enheten mappen och kör hello följande kommando vid en kommandotolk:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Köra hello appar

Du är nu redo toorun hello appar.

1. Kör hello efter kommandot toobegin lyssnar efter omstart metodanrop från din IoT-hubb i en kommandotolk i hello simulerade enheten mapp:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb simulerade enheten app toolisten för omstart direkt metodanrop][1]

1. Kör hello efter kommandot toocall hello omstart metoden på den simulerade enheten från din IoT-hubb i en kommandotolk i hello utlösaren omstart mapp:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app toocall hello omstart direkt metod][2]

1. hello simulerade enheten svarar toohello omstart direkt metodanrop:

    ![Java IoT-hubb simulerade enhetsapp svarar toohello direkt metodanrop][3]

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[1]: ./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png
[2]: ./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png
[3]: ./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png
<!-- Links -->

[lnk-maven]: https://maven.apache.org/what-is-maven.html

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22