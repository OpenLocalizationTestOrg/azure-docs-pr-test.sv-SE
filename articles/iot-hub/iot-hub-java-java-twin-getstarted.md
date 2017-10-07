---
title: "aaaGet igång med Azure IoT Hub-enhet twins (Java) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet twins tooadd taggar och sedan använda en IoT-hubb-fråga. Du kan använda hello Azure IoT-enhet SDK för Java tooimplement hello enhetsapp och hello Azure IoT-tjänsten SDK för Java tooimplement service-appen som lägger till hello taggar och kör hello IoT-hubb frågan."
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
ms.date: 07/04/2017
ms.author: dobett
ms.openlocfilehash: 25f6fc81471d59c62bcdc3766bb5c33f5733c930
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-twins-java"></a>Kom igång med enheten twins (Java)

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

I kursen får skapa du två Java-konsolappar:

* **Lägg till-taggar-query**, en backend-Java-app som lägger till taggar och frågar enheten twins.
* **simulerade enheten**, en Java-enhetsapp att som ansluter tooyour IoT-hubb och rapporter tillståndet anslutning med hjälp av en rapporterade egenskap.

> [!NOTE]
> hello artikel [Azure IoT SDK](iot-hub-devguide-sdks.md) innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.

toocomplete den här kursen behöver du:

* Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Ett aktivt Azure-konto. (Om du inte har ett konto kan du skapa en [kostnadsfritt konto](http://azure.microsoft.com/pricing/free-trial/) på bara några minuter.)

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

Om du vill toocreate hello enhetsidentitet programmässigt kan läsa hello motsvarande avsnitt i hello [ansluta din enhet tooyour IoT-hubb som använder Java](iot-hub-java-java-getstarted.md#create-a-device-identity) artikel.

## <a name="create-hello-service-app"></a>Skapa hello service-appen

I det här avsnittet skapar du en Java-app som platsen metadata läggs till som en tagg toohello enheten dubbla i IoT-hubb som är associerade med **myDeviceId**. hello app frågor IoT-hubb för enheter som finns i hello USA och sedan för enheter som rapporterar en mobilnät-anslutning.

1. Skapa en tom mapp med namnet på utvecklingsdatorn, `iot-java-twin-getstarted`.

1. I hello `iot-java-twin-getstarted` mapp, skapa ett Maven-projekt som kallas **Lägg till-taggar-query** med hello följande kommando vid en kommandotolk. Observera att detta är ett enda långt kommando:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Kommandotolken, gå toohello `add-tags-query` mapp.

1. Använd en textredigerare och öppna hello `pom.xml` filen i hello `add-tags-query` mapp och Lägg till följande beroende toohello hello **beroenden** nod. Det här beroendet kan du toouse hello **iot-service-klient** paketet i din app toocommunicate med IoT-hubben:

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

1. Använd en textredigerare och öppna hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fil.

1. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. Lägg till följande variabler klassen på toohello hello **App** klass. Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. Uppdatera hello **huvudsakliga** metoden signatur tooinclude hello följande `throws` sats:

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. Lägg till följande kod toohello hello **huvudsakliga** metoden toocreate hello **DeviceTwin** och **DeviceTwinDevice** objekt. Hej **DeviceTwin** objekt hanterar hello kommunikation med IoT-hubb. Hej **DeviceTwinDevice** -objektet representerar hello enheten dubbla med dess egenskaper och taggar:

    ```java
    // Get hello DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. Lägg till följande hello `try/catch` blockera toohello **huvudsakliga** metoden:

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. tooupdate hello **region** och **anläggningen** enheten dubbla taggar i din enhet dubbla Lägg till följande kod i hello hello `try` block:

    ```java
    // Get hello device twin from IoT Hub
    System.out.println("Device twin before update:");
    twinClient.getTwin(device);
    System.out.println(device);

    // Update device twin tags if they are different
    // from hello existing values
    String currentTags = device.tagsToString();
    if ((!currentTags.contains("region=" + region) && !currentTags.contains("plant=" + plant))) {
      // Create hello tags and attach them toohello DeviceTwinDevice object
      Set<Pair> tags = new HashSet<Pair>();
      tags.add(new Pair("region", region));
      tags.add(new Pair("plant", plant));
      device.setTags(tags);

      // Update hello device twin in IoT Hub
      System.out.println("Updating device twin");
      twinClient.updateTwin(device);
    }

    // Retrieve hello device twin with hello tag values from IoT Hub
    System.out.println("Device twin after update:");
    twinClient.getTwin(device);
    System.out.println(device);
    ```

1. tooquery hello enheten twins i IoT-hubb Lägg till följande kod toohello hello `try` block efter hello du lade till i hello föregående steg. hello koden körs två frågor. Varje fråga returnerar högst 100 enheter:

    ```java
    // Query hello device twins in IoT Hub
    System.out.println("Devices in Redmond:");

    // Construct hello query
    SqlQuery sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43'", null);

    // Run hello query, returning a maximum of 100 devices
    Query twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 100);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }

    System.out.println("Devices in Redmond using a cellular network:");

    // Construct hello query
    sqlQuery = SqlQuery.createSqlQuery("*", SqlQuery.FromType.DEVICES, "tags.plant='Redmond43' AND properties.reported.connectivityType = 'cellular'", null);

    // Run hello query, returning a maximum of 100 devices
    twinQuery = twinClient.queryTwin(sqlQuery.getQuery(), 3);
    while (twinClient.hasNextDeviceTwin(twinQuery)) {
      DeviceTwinDevice d = twinClient.getNextDeviceTwin(twinQuery);
      System.out.println(d.getDeviceId());
    }
    ```

1. Spara och Stäng hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fil

1. Skapa hello **Lägg till-taggar-query** app och korrigera eventuella fel. Kommandotolken, gå toohello `add-tags-query` mappen och kör hello följande kommando:

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a>Skapa en enhetsapp

I det här avsnittet skapar du en Java-konsolapp som anger ett rapporterat egenskapsvärde som skickas tooIoT hubb.

1. I hello `iot-java-twin-getstarted` mapp, skapa ett Maven-projekt som kallas **simulerade enheten** med hello följande kommando vid en kommandotolk. Observera att detta är ett enda långt kommando:

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
    private static String deviceId = "myDeviceId";
    ```

    Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt. För närvarande toouse dubbla funktioner för enheter som du måste använda hello MQTT-protokollet.

1. Lägg till följande kod toohello hello **huvudsakliga** metod för att:
    * Skapa en enhet klienten toocommunicate med IoT-hubben.
    * Skapa en **enhet** objekt toostore hello dubbla Enhetsegenskaper.

    ```java
    DeviceClient client = new DeviceClient(connString, protocol);

    // Create a Device object toostore hello device twin properties
    Device dataCollector = new Device() {
      // Print details when a property value changes
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context) {
        System.out.println(propertyKey + " changed too" + propertyValue);
      }
    };
    ```

1. Lägg till följande kod toohello hello **huvudsakliga** metoden toocreate en **connectivityType** rapporterade egenskapen och skicka den tooIoT hubb:

    ```java
    try {
      // Open hello DeviceClient and start hello device twin services.
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);

      // Create a reported property and send it tooyour IoT hub.
      dataCollector.setReportedProp(new Property("connectivityType", "cellular"));
      client.sendReportedProperties(dataCollector.getReportedProp());
    }
    catch (Exception e) {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Lägg till hello efter koden toohello hello **huvudsakliga** metod. Väntar på hello **RETUR** nyckel kan tid för IoT-hubb tooreport hello status hello enheten dubbla åtgärder:

    ```java
    System.out.println("Press any key tooexit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. Spara och Stäng hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.

1. Skapa hello **simulerade enheten** app och korrigera eventuella fel. Kommandotolken, gå toohello `simulated-device` mappen och kör hello följande kommando:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Köra hello appar

Du är nu redo toorun hello konsolappar.

1. Vid en kommandotolk i hello `add-tags-query` mapp, kör följande kommando toorun hello hello **Lägg till-taggar-query** service-appen:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app tooupdate tagga värden och köra frågor för enhet](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    Du kan se hello **anläggningen** och **region** taggar läggs toohello enheten dubbla. hello första frågan returnerar enheten, men hello andra inte.

1. Vid en kommandotolk i hello `simulated-device` mapp, kör följande kommando tooadd hello hello **connectivityType** rapporterade egenskapen toohello enheten dubbla:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Hej enhetsklienten lägger till hello ** connectivityType ** rapporterade egenskapen](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. Vid en kommandotolk i hello `add-tags-query` mapp, kör följande kommando toorun hello hello **Lägg till-taggar-query** service-appen en gång:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app tooupdate tagga värden och köra frågor för enhet](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    Nu har enheten skickat hello **connectivityType** egenskapen tooIoT hubben, hello andra frågan returnerar enheten.

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du lagt till enhetsmetadata som taggar från en backend-app och skrev en app tooreport enheten anslutning enhetsinformation i hello enheten dubbla. Du också lära dig hur tooquery hello dubbla enhetsinformation med hjälp av frågespråket för hello SQL-liknande IoT-hubb.

Använd hello följande resurser toolearn hur till:

* Skicka telemetri från enheter med hello [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) kursen.
* Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt](iot-hub-java-java-direct-methods.md) kursen.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
