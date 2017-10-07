---
title: aaaUse Azure IoT Hub direkt metoder (Java) | Microsoft Docs
description: "Hur toouse Azure IoT Hub direkt metoder. Du kan använda hello Azure IoT-enhet SDK för Java tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för Java tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: ab035b8e-bff8-4e12-9536-f31d6b6fe425
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: b6f2f4a64535ab649a3965cd9c5a19bebaf88eef
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="use-direct-methods-java"></a>Använda direkt metoder (Java)

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

I kursen får skapa du två Java-konsolappar:

* **anropa-direct-method**, en backend-Java-app som anropar en metod i hello simulerade enhetsapp och visar hello svar.
* **simulerade enheten**, en Java-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhets-ID som du skapar. Den här appen svarar toohello anropas direkt från hello serverdel.

> [!NOTE]
> Information om hello SDK: er som du kan använda toobuild program toorun på enheter och din lösningens serverdel finns [Azure IoT SDK][lnk-hub-sdks].

toocomplete den här kursen behöver du:

* Java SE 8. <br/> [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Java för den här självstudiekursen på Windows- eller Linux.
* Maven 3.  <br/> [Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall [Maven] [ lnk-maven] för den här självstudiekursen på Windows- eller Linux.
* [Node.js-version 0.10.0 eller senare](http://nodejs.org).

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a>Skapa en simulerad enhetsapp

I det här avsnittet skapar du en Java-konsolapp som svarar tooa metoden anropas av hello-lösningen slutet.

1. Skapa en tom mapp som kallas iot-java-direct-metoden.

1. Skapa ett Maven-projekt som kallas i hello iot-java-direct-method mappen **simulerade enheten** med hello följande kommando vid en kommandotolk. hello följande kommando är ett långt kommando:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigera toohello simulerade enheten mapp i en kommandotolk.

1. Med en textredigerare, öppna hello pom.xml filen hello simulerade enheten mapp och lägga till följande beroenden toohello hello **beroenden** nod. Det här beroendet kan du toouse hello iot enhetsklienten paketet i din app toocommunicate med IoT-hubben:

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

1. Använd en textredigerare och öppna hello simulated-device\src\main\java\com\mycompany\app\App.java filen.

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
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt. För närvarande direkt toouse metoder som du måste använda hello MQTT-protokollet.

1. tooreturn en status kod tooyour IoT-hubb, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. toohandle hello direkta metoden anrop från hello lösningens serverdel, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "writeLine" :
          {
            int status = METHOD_SUCCESS;
            System.out.println(new String((byte[])methodData));
            deviceMethodData = new DeviceMethodData(status, "Executed direct method " + methodName);
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

1. toocreate en **DeviceClient** och lyssna efter direkta metoden anrop, lägga till en **huvudsakliga** metoden toohello **App** klass:

    ```java
    public static void main(String[] args)
      throws IOException, URISyntaxException
    {
      System.out.println("Starting device sample...");

      DeviceClient client = new DeviceClient(connString, protocol);

      try
      {
        client.open();
        client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
        System.out.println("Subscribed toodirect methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key tooexit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. Spara och Stäng hello simulated-device\src\main\java\com\mycompany\app\App.java fil

1. Skapa hello **simulerade enheten** app och korrigera eventuella fel. Navigera toohello simulerade enheten mappen och kör hello följande kommando vid en kommandotolk:

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a>Anropa en metod som direkt på en enhet

I det här avsnittet skapar du en Java-konsolapp som anropar en direkt metod och sedan visar hello svar. Den här appen konsolen ansluter tooyour IoT-hubb tooinvoke hello direkta metoden.

1. Skapa ett Maven-projekt som kallas i hello iot-java-direct-method mappen **invoke-direct-metoden** med hello följande kommando vid en kommandotolk. hello följande kommando är ett långt kommando:

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. Navigera toohello invoke-direct-method mapp i en kommandotolk.

1. Med en textredigerare, öppna hello pom.xml filen hello invoke-direct-method mapp och lägga till följande beroende toohello hello **beroenden** nod. Det här beroendet kan du toouse hello iot-service-klient paketet i din app toocommunicate med IoT-hubben:

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

1. Använd en textredigerare och öppna hello invoke-direct-method\src\main\java\com\mycompany\app\App.java filen.

1. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. Lägg till följande variabler klassen på toohello hello **App** klass. Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line toobe written";
    ```

1. tooinvoke hello metod på hello simulerade enheten, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
        System.out.println("Invoke direct method");
        MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, payload);

        if(result == null)
        {
            throw new IOException("Direct method invoke returns null");
        }
        System.out.println("Status=" + result.getStatus());
        System.out.println("Payload=" + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }

    System.out.println("Shutting down sample...");
    ```

1. Spara och Stäng hello invoke-direct-method\src\main\java\com\mycompany\app\App.java fil

1. Skapa hello **invoke-direct-method** app och korrigera eventuella fel. Navigera toohello invoke-direct-method mappen och kör hello följande kommando vid en kommandotolk:

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a>Köra hello appar

Du är nu redo toorun hello konsolappar.

1. Kör hello efter kommandot toobegin lyssnar efter metodanrop från din IoT-hubb i en kommandotolk i hello simulerade enheten mapp:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb simulerade enheten app toolisten för direkta metodanrop][8]

1. Vid en kommandotolk i hello invoke-direct-method mappen kör hello efter kommandot toocall en metod på den simulerade enheten från din IoT-hubb:

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app toocall direkt metod][7]

1. hello simulerade enheten svarar toohello direkt metodanrop:

    ![Java IoT-hubb simulerade enhetsapp svarar toohello direkt metodanrop][9]

## <a name="next-steps"></a>Nästa steg

I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret. Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet. Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet.

tooexplore andra IoT-scenarier finns [schemalägga jobb på flera enheter][lnk-devguide-jobs].

toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.

<!-- Images. -->
[7]: ./media/iot-hub-java-java-direct-methods/invoke-method.png
[8]: ./media/iot-hub-java-java-direct-methods/device-listen.png
[9]: ./media/iot-hub-java-java-direct-methods/device-respond.png

<!-- Links -->
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md
[lnk-maven]: https://maven.apache.org/what-is-maven.html
[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-devguide-jobs]: iot-hub-devguide-jobs.md
[lnk-tutorial-jobs]: iot-hub-node-node-schedule-jobs.md
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
