---
title: "aaaUpload filer från enheter tooAzure IoT-hubb med Java | Microsoft Docs"
description: "Hur tooupload filer från en enhet toohello moln som använder Azure IoT-enhet SDK för Java. Överförda filer lagras i ett Azure storage blob-behållaren."
services: iot-hub
documentationcenter: java
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 4759d229-f856-4526-abda-414f8b00a56d
ms.service: iot-hub
ms.devlang: java
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 06/28/2017
ms.author: dobett
ms.openlocfilehash: e305fe61bf7ca0aeb2c092bc2c7efebdc78d4f68
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub"></a>Ladda upp filer från din enhet toohello moln med IoT-hubb

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

Den här kursen bygger på hello koden i hello [meddelanden moln till enhet med IoT-hubben](iot-hub-java-java-c2d.md) självstudiekursen tooshow du hur toouse hello [filen överför funktionerna i IoT-hubb](iot-hub-devguide-file-upload.md) tooupload en fil för[ Azure-blobblagring](../storage/index.md). hello kursen visar hur till:

- Ange en enhet på ett säkert sätt med en Azure blob-URI för att överföra en fil.
- Använd hello IoT-hubb överför meddelanden tootrigger filbearbetning hello-filen i din app-serverdelen.

Hej [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) och [meddelanden moln till enhet med IoT-hubben](iot-hub-java-java-c2d.md) självstudiekurser visar hello grundläggande enhet till moln och moln till enhet meddelandefunktioner för IoT-hubb. Hej [meddelanden processen enhet till moln](iot-hub-java-java-process-d2c.md) självstudiekursen beskriver ett sätt tooreliably spara enhet till moln meddelanden i Azure blob storage. Men i vissa fall kan du enkelt kartdata hello enheterna skickar till relativt liten enhet till moln hälsningsmeddelande som accepterar IoT-hubb. Exempel:

* Stora filer som innehåller bilder
* Videoklipp
* Vibration data samplas vid hög frekvens
* Någon form av förbearbetade data.

Dessa filer finns vanligtvis i hello molnet med hjälp av verktyg som bearbetas i batch [Azure Data Factory](../data-factory/index.md) eller hello [Hadoop](../hdinsight/index.md) stacken. När du behöver tooupland filer från en enhet, kan du fortfarande använda hello säkerheten och pålitligheten för IoT-hubb.

Hello slutet av den här kursen kan du köra två appar som Java-konsolen:

* **simulerade enheten**, en modifierad version av hello app som skapats i hello [skicka moln till enhet meddelanden med IoT-hubb] kursen. Den här appen överför en fil toostorage med en SAS-URI som tillhandahålls av din IoT-hubb.
* **Läs filen-överför-meddelande**, som tar emot filen överföra meddelanden från din IoT-hubb.

> [!NOTE]
> IoT-hubb stöder många vilka plattformar och språk (inklusive C, .NET och Javascript) via Azure IoT-enhet SDK: er. Se toohello [Azure IoT Developer Center] stegvisa anvisningar för hur tooconnect din enhet tooAzure IoT-hubb.

toocomplete den här kursen behöver du hello följande:

* Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)
* [Maven 3](https://maven.apache.org/install.html)
* Ett aktivt Azure-konto. (Om du inte har ett konto kan du skapa en [kostnadsfritt konto](http://azure.microsoft.com/pricing/free-trial/) på bara några minuter.)

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a>Överför en fil från en enhetsapp

I det här avsnittet kan du ändra hello enhetsapp som du skapade i [meddelanden moln till enhet med IoT-hubben](iot-hub-java-java-c2d.md) tooupload en fil tooIoT hubb.

1. Kopiera en bild filen toohello `simulated-device` mappen och Byt namn på den `myimage.png`.

1. Använd en textredigerare och öppna hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.

1. Lägg till hello variabeldeklarationen toohello **App** klass:

    ```java
    private static String fileName = "myimage.png";
    ```

1. tooprocess fil överför motringning statusmeddelanden, Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    // Define a callback method tooprint status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toofile upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. tooupload bilder tooIoT hubben, Lägg till följande metod toohello hello **App** klassen tooupload bilder tooIoT hubb:

    ```java
    // Use IoT Hub tooupload a file asynchronously tooAzure blob storage.
    private static void uploadFile(String fullFileName) throws FileNotFoundException, IOException
    {
      File file = new File(fullFileName);
      InputStream inputStream = new FileInputStream(file);
      long streamLength = file.length();

      client.uploadToBlobAsync(fileName, inputStream, streamLength, new FileUploadStatusCallBack(), null);
    }
    ```

1. Ändra hello **huvudsakliga** metoden toocall hello **uploadFile** metod som visas i följande fragment hello:

    ```java
    client.open();

    try
    {
      // Get hello filename and start hello upload.
      String fullFileName = System.getProperty("user.dir") + File.separator + fileName;
      uploadFile(fullFileName);
      System.out.println("File upload started with success");
    }
    catch (Exception e)
    {
      System.out.println("Exception uploading file: " + e.getCause() + " \nERROR: " + e.getMessage());
    }

    MessageSender sender = new MessageSender();
    ```

1. Använd hello följande kommando toobuild hello **simulerade enheten** appen och Sök efter fel:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a>Ta emot ett meddelande om överföringen

I det här avsnittet skapar du en Java-konsolapp som tar emot filen överföra meddelanden från IoT-hubb.

Du behöver hello **iothubowner** anslutningssträngen för din IoT-hubb toocomplete i det här avsnittet. Du hittar hello anslutningssträngen i hello [Azure-portalen](https://portal.azure.com/) på hello **princip för delad åtkomst** bladet.

1. Skapa ett Maven-projekt som kallas **läsa filen-överför-meddelande** med hello följande kommando vid en kommandotolk. Observera att det här kommandot är ett långt kommando:

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. Kommandotolken, gå toohello nya `read-file-upload-notification` mapp.

1. Använd en textredigerare och öppna hello `pom.xml` filen i hello `read-file-upload-notification` mapp och Lägg till följande beroende toohello hello **beroenden** nod. Lägger till hello beroende kan du toouse hello **iothub-java-service-klient** paketet i ditt program toocommunicate med din IoT-hubb-tjänst:

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).

1. Spara och Stäng hello `pom.xml` fil.

1. Använd en textredigerare och öppna hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fil.

1. Lägg till följande hello **importera** instruktioner toohello fil:

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. Lägg till följande variabler klassen på toohello hello **App** klass:

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. tooprint information om hello filen överför toohello-konsolen Lägg till följande hello kapslad klass toohello **App** klass:

    ```java
    // Create a thread tooreceive file upload notifications.
    private static class ShowFileUploadNotifications implements Runnable {
      public void run() {
        try {
          while (true) {
            System.out.println("Recieve file upload notifications...");
            FileUploadNotification fileUploadNotification = fileUploadNotificationReceiver.receive();
            if (fileUploadNotification != null) {
              System.out.println("File Upload notification received");
              System.out.println("Device Id : " + fileUploadNotification.getDeviceId());
              System.out.println("Blob Uri: " + fileUploadNotification.getBlobUri());
              System.out.println("Blob Name: " + fileUploadNotification.getBlobName());
              System.out.println("Last Updated : " + fileUploadNotification.getLastUpdatedTimeDate());
              System.out.println("Blob Size (Bytes): " + fileUploadNotification.getBlobSizeInBytes());
              System.out.println("Enqueued Time: " + fileUploadNotification.getEnqueuedTimeUtcDate());
            }
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. toostart hello tråd som lyssnar efter filen överför meddelanden, Lägg till följande kod toohello hello **huvudsakliga** metoden:

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException, Exception {
      ServiceClient serviceClient = ServiceClient.createFromConnectionString(connectionString, protocol);

      if (serviceClient != null) {
        serviceClient.open();

        // Get a file upload notification receiver from hello ServiceClient.
        fileUploadNotificationReceiver = serviceClient.getFileUploadNotificationReceiver();
        fileUploadNotificationReceiver.open();

        // Start hello thread tooreceive file upload notifications.
        ShowFileUploadNotifications showFileUploadNotifications = new ShowFileUploadNotifications();
        ExecutorService executor = Executors.newFixedThreadPool(1);
        executor.execute(showFileUploadNotifications);

        System.out.println("Press ENTER tooexit.");
        System.in.read();
        executor.shutdownNow();
        System.out.println("Shutting down sample...");
        fileUploadNotificationReceiver.close();
        serviceClient.close();
      }
    }
    ```

1. Spara och Stäng hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fil.

1. Använd hello följande kommando toobuild hello **läsa filen-överför-meddelande** appen och Sök efter fel:

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a>Köra hello program

Nu är du redo toorun hello program.

Vid en kommandotolk i hello `read-file-upload-notification` mapp, kör hello följande kommando:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

Vid en kommandotolk i hello `simulated-device` mapp, kör hello följande kommando:

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

hello följande skärmbild visar hello utdata från hello **simulerade enheten** app:

![Utdata från simulerade enheten app](media/iot-hub-java-java-upload/simulated-device.png)

hello följande skärmbild visar hello utdata från hello **läsa filen-överför-meddelande** app:

![Utdata från Läs-filen-överför-meddelandeprogram](media/iot-hub-java-java-upload/read-file-upload-notification.png)

Du kan använda hello portal tooview hello upp filen i hello lagringsbehållare som du har konfigurerat:

![Överförda filen](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a>Nästa steg

I kursen får du lärt dig hur toouse hello filöverföring funktionerna i IoT-hubb toosimplify filöverföringar från enheter. Du kan fortsätta tooexplore IoT-hubb funktioner och scenarier med hello följande artiklar:

* [Skapa en IoT-hubb programmässigt][lnk-create-hub]
* [Introduktion tooC SDK][lnk-c-sdk]
* [Azure IoT-SDK][lnk-sdks]

toofurther utforska hello funktionerna i IoT Hub, se:

* [Simulera en enhet med IoT kant][lnk-iotedge]

<!-- Images. -->

[50]: ./media/iot-hub-csharp-csharp-file-upload/run-apps1.png
[1]: ./media/iot-hub-csharp-csharp-file-upload/image-properties.png
[2]: ./media/iot-hub-csharp-csharp-file-upload/file-upload-project-csharp1.png
[3]: ./media/iot-hub-csharp-csharp-file-upload/enable-file-notifications.png

<!-- Links -->



[Azure IoT Developer Center]: http://www.azure.com/develop/iot

[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[Azure Storage]:../storage/common/storage-create-storage-account.md#create-a-storage-account
[lnk-configure-upload]: iot-hub-configure-file-upload.md
[Azure IoT service SDK NuGet package]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/

[lnk-create-hub]: iot-hub-rm-template-powershell.md
[lnk-c-sdk]: iot-hub-device-sdk-c-intro.md
[lnk-sdks]: iot-hub-devguide-sdks.md

[lnk-iotedge]: iot-hub-windows-iot-edge-simulated-device.md


