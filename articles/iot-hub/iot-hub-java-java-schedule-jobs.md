---
title: aaaSchedule jobb med Azure IoT Hub (Java) | Microsoft Docs
description: "Hur tooschedule en Azure IoT-hubb jobb tooinvoke direkt metod och ange en önskad egenskap på flera enheter. Du kan använda hello Azure IoT-enhet SDK för Java tooimplement hello simulerade enhetsappar och hello Azure IoT-tjänsten SDK för Java tooimplement ett app toorun hello jobb."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: b1b05fa56c3ce96af0b33d4cca0dd54da0f4e927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-java"></a>Schemat och sändning jobb (Java)

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

Använd Azure IoT Hub tooschedule och spåra jobb som uppdaterar miljoner enheter. Använd jobb till:

* Uppdatera önskade egenskaper
* Uppdatera taggar
* Anropa direkt metoder

Ett jobb radbryts någon av följande åtgärder och spårar hello körning mot en uppsättning enheter. En enhet dubbla fråga definierar hello enheter hello jobbet körs mot. En backend-app kan till exempel använda en jobbet tooinvoke direkt metod på 10 000 enheter som startar om hello-enheter. Du anger hello uppsättning av enheter med en enhet dubbla fråga och schemalägga hello jobbet toorun vid ett senare tillfälle. hello spårar jobbförloppet som var och en av hello enheter tar emot och kör hello omstart direkt metod.

toolearn mer information om var och en av dessa funktioner finns:

* Enheten dubbla och egenskaper: [Kom igång med enheten twins](iot-hub-java-java-twin-getstarted.md)
* Dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder](iot-hub-devguide-direct-methods.md) och [kursen: direkta metoder](iot-hub-java-java-direct-methods.md)

I den här självstudiekursen lär du dig att:

* Skapa en enhetsapp som implementerar en direkt metod som kallas **lockDoor**. Hej enhetsapp tar också emot önskade egenskapsändringar från hello backend-app.
* Skapa en backend-app som skapar ett jobb toocall hello **lockDoor** direkt metod på flera enheter. Ett annat jobb skickar önskade egenskapen uppdaterar toomultiple enheter.

Hello slutet av den här självstudiekursen har du en enhet för java konsolapp och en java-backend-konsolapp:

**simulerade enheten** som ansluter tooyour IoT-hubb, implementerar hello **lockDoor** direkt metod och hanterar behövs egenskapsändringar.

**schema-jobb** som använder jobb toocall hello **lockDoor** direkta metoden och uppdatera hello enheten dubbla önskade egenskaper på flera enheter.

> [!NOTE]
> hello artikel [Azure IoT SDK](iot-hub-devguide-sdks.md) innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.

## <a name="prerequisites"></a>Krav

toocomplete den här kursen behöver du:

* Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Ett aktivt Azure-konto. (Om du inte har ett konto kan du skapa en [kostnadsfritt konto](http://azure.microsoft.com/pricing/free-trial/) på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Om du vill toocreate hello enhetsidentitet programmässigt kan läsa hello motsvarande avsnitt i hello [ansluta din enhet tooyour IoT-hubb som använder Java](iot-hub-java-java-getstarted.md#create-a-device-identity) artikel. Du kan också använda hello [iothub explorer](https://github.com/Azure/iothub-explorer) verktyget tooadd enhet tooyour IoT-hubb.

## <a name="create-hello-service-app"></a>Skapa hello service-appen

I det här avsnittet skapar du en Java-konsolapp som använder jobb till:

* Anropa hello **lockDoor** direkt metod på flera enheter.
* Skicka egenskaper toomultiple enheter.

toocreate hello app:

1. Skapa en tom mapp med namnet på utvecklingsdatorn, `iot-java-schedule-jobs`.

1. I hello `iot-java-schedule-jobs` mapp, skapa ett Maven-projekt som kallas **schema jobb** med hello följande kommando vid en kommandotolk. Observera att detta är ett enda långt kommando:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Kommandotolken, gå toohello `schedule-jobs` mapp.

1. Använd en textredigerare och öppna hello `pom.xml` filen i hello `schedule-jobs` mapp och Lägg till följande beroende toohello hello **beroenden** nod. Det här beroendet kan du toouse hello **iot-service-klient** paketet i din app toocommunicate med IoT-hubben:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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

1. Spara och Stäng hello `pom.xml` fil.

1. Använd en textredigerare och öppna hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fil.

1. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

1. Lägg till följande variabler klassen på toohello hello **App** klass. Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long hello job is permitted toorun without
    // completing its work on hello set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. Lägg till följande metod toohello hello **App** klassen tooschedule ett jobb tooupdate hello **byggnad** och **våning** önskade egenskaper i hello enheten dubbla:

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule hello update twin job toorun now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

1. tooschedule ett jobb toocall hello **lockDoor** metod, Lägg till följande metod toohello hello **App** klass:

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now toocall hello lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set too5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

1. toomonitor ett jobb, Lägg till följande metod toohello hello **App** klass:

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check hello job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

1. tooquery hello information om hello jobb som du körde Lägg till följande metod hello:

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using hello time hello jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over hello list of jobs and print hello details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. Uppdatera hello **huvudsakliga** metoden signatur tooinclude hello följande `throws` sats:

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. toorun och övervaka två jobb, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    // Record hello start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query tooshow hello job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. Spara och Stäng hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fil

1. Skapa hello **schema jobb** app och korrigera eventuella fel. Kommandotolken, gå toohello `schedule-jobs` mappen och kör hello följande kommando:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Skapa en enhetsapp

I det här avsnittet skapar du en Java-konsolapp som hanterar hello önskade egenskaper som skickats från IoT-hubb och implementerar hello direkt metodanrop.

1. I hello `iot-java-schedule-jobs` mapp, skapa ett Maven-projekt som kallas **simulerade enheten** med hello följande kommando vid en kommandotolk. Observera att detta är ett enda långt kommando:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Kommandotolken, gå toohello `simulated-device` mapp.

1. Använd en textredigerare och öppna hello `pom.xml` filen i hello `simulated-device` mapp och Lägg till följande beroenden toohello hello **beroenden** nod. Det här beroendet kan du toouse hello **iot enhetsklienten** paketet i din app toocommunicate med IoT-hubben:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

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

1. Spara och Stäng hello `pom.xml` fil.

1. Använd en textredigerare och öppna hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.

1. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. Lägg till följande variabler klassen på toohello hello **App** klass. Ersätta `{youriothubname}` med din IoT-hubbnamnet och `{yourdevicekey}` hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt. För närvarande toouse dubbla funktioner för enheter som du måste använda hello MQTT-protokollet.

1. tooprint enhet dubbla meddelanden toohello konsolen, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
      }
    }
    ```

1. tooprint direkta metoden meddelanden toohello konsolen, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodirect method operation with status " + status.name());
      }
    }
    ```

1. toohandle direkta metodanrop från IoT-hubb, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

1. Uppdatera hello **huvudsakliga** metoden signatur tooinclude hello följande `throws` sats:

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. Lägg till följande kod toohello hello **huvudsakliga** metod för att:
    * Skapa en enhet klienten toocommunicate med IoT-hubben.
    * Skapa en **enhet** objekt toostore hello dubbla Enhetsegenskaper.

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object toomanage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. klienttjänster toostart hello enhet, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    try {
      // Open hello DeviceClient
      // Start hello device twin services
      // Subscribe toodirect method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

1. toowait för hello användaren toopress hello **RETUR** nyckeln innan du stänger, lägga till hello efter koden toohello hello **huvudsakliga** metoden:

    ```java
    // Close hello app
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. Spara och Stäng hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.

1. Skapa hello **simulerade enheten** app och korrigera eventuella fel. Kommandotolken, gå toohello `simulated-device` mappen och kör hello följande kommando:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Köra hello appar

Du är nu redo toorun hello konsolappar.

1. Vid en kommandotolk i hello `simulated-device` mapp, kör följande kommando toostart hello enhetsapp lyssnar efter önskad egenskapsändringar och direkt metodanrop hello:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Hej enhetsklienten startar](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. Vid en kommandotolk i hello `schedule-jobs` mapp, kör följande kommando toorun hello hello **schema jobb** tjänsten app toorun två jobb. hello först anger hello önskad egenskapsvärden, hello andra anrop hello direkta metoden:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Service-appen för Java IoT-hubb skapar två jobb](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. Hej enhetsapp hanterar hello önskad egenskapen ändrings- och hello direkt metodanrop:

    ![Hej enhetsklienten svarar toohello ändringar](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har skapat en backend-app toorun två jobb. hello första jobb ange önskade egenskapsvärden och hello andra jobb anropas direkt metod.

Använd hello följande resurser toolearn hur till:

* Skicka telemetri från enheter med hello [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) kursen.
* Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt](iot-hub-java-java-direct-methods.md) kursen.
