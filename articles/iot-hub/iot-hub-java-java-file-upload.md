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
# <a name="upload-files-from-your-device-toohello-cloud-with-iot-hub"></a><span data-ttu-id="60648-104">Ladda upp filer från din enhet toohello moln med IoT-hubb</span><span class="sxs-lookup"><span data-stu-id="60648-104">Upload files from your device toohello cloud with IoT Hub</span></span>

[!INCLUDE [iot-hub-file-upload-language-selector](../../includes/iot-hub-file-upload-language-selector.md)]

<span data-ttu-id="60648-105">Den här kursen bygger på hello koden i hello [meddelanden moln till enhet med IoT-hubben](iot-hub-java-java-c2d.md) självstudiekursen tooshow du hur toouse hello [filen överför funktionerna i IoT-hubb](iot-hub-devguide-file-upload.md) tooupload en fil för[ Azure-blobblagring](../storage/index.md).</span><span class="sxs-lookup"><span data-stu-id="60648-105">This tutorial builds on hello code in hello [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorial tooshow you how toouse hello [file upload capabilities of IoT Hub](iot-hub-devguide-file-upload.md) tooupload a file too[Azure blob storage](../storage/index.md).</span></span> <span data-ttu-id="60648-106">hello kursen visar hur till:</span><span class="sxs-lookup"><span data-stu-id="60648-106">hello tutorial shows you how to:</span></span>

- <span data-ttu-id="60648-107">Ange en enhet på ett säkert sätt med en Azure blob-URI för att överföra en fil.</span><span class="sxs-lookup"><span data-stu-id="60648-107">Securely provide a device with an Azure blob URI for uploading a file.</span></span>
- <span data-ttu-id="60648-108">Använd hello IoT-hubb överför meddelanden tootrigger filbearbetning hello-filen i din app-serverdelen.</span><span class="sxs-lookup"><span data-stu-id="60648-108">Use hello IoT Hub file upload notifications tootrigger processing hello file in your app back end.</span></span>

<span data-ttu-id="60648-109">Hej [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) och [meddelanden moln till enhet med IoT-hubben](iot-hub-java-java-c2d.md) självstudiekurser visar hello grundläggande enhet till moln och moln till enhet meddelandefunktioner för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-109">hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) and [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tutorials show hello basic device-to-cloud and cloud-to-device messaging functionality of IoT Hub.</span></span> <span data-ttu-id="60648-110">Hej [meddelanden processen enhet till moln](iot-hub-java-java-process-d2c.md) självstudiekursen beskriver ett sätt tooreliably spara enhet till moln meddelanden i Azure blob storage.</span><span class="sxs-lookup"><span data-stu-id="60648-110">hello [Process Device-to-Cloud messages](iot-hub-java-java-process-d2c.md) tutorial describes a way tooreliably store device-to-cloud messages in Azure blob storage.</span></span> <span data-ttu-id="60648-111">Men i vissa fall kan du enkelt kartdata hello enheterna skickar till relativt liten enhet till moln hälsningsmeddelande som accepterar IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-111">However, in some scenarios you cannot easily map hello data your devices send into hello relatively small device-to-cloud messages that IoT Hub accepts.</span></span> <span data-ttu-id="60648-112">Exempel:</span><span class="sxs-lookup"><span data-stu-id="60648-112">For example:</span></span>

* <span data-ttu-id="60648-113">Stora filer som innehåller bilder</span><span class="sxs-lookup"><span data-stu-id="60648-113">Large files that contain images</span></span>
* <span data-ttu-id="60648-114">Videoklipp</span><span class="sxs-lookup"><span data-stu-id="60648-114">Videos</span></span>
* <span data-ttu-id="60648-115">Vibration data samplas vid hög frekvens</span><span class="sxs-lookup"><span data-stu-id="60648-115">Vibration data sampled at high frequency</span></span>
* <span data-ttu-id="60648-116">Någon form av förbearbetade data.</span><span class="sxs-lookup"><span data-stu-id="60648-116">Some form of preprocessed data.</span></span>

<span data-ttu-id="60648-117">Dessa filer finns vanligtvis i hello molnet med hjälp av verktyg som bearbetas i batch [Azure Data Factory](../data-factory/index.md) eller hello [Hadoop](../hdinsight/index.md) stacken.</span><span class="sxs-lookup"><span data-stu-id="60648-117">These files are typically batch processed in hello cloud using tools such as [Azure Data Factory](../data-factory/index.md) or hello [Hadoop](../hdinsight/index.md) stack.</span></span> <span data-ttu-id="60648-118">När du behöver tooupland filer från en enhet, kan du fortfarande använda hello säkerheten och pålitligheten för IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-118">When you need tooupland files from a device, you can still use hello security and reliability of IoT Hub.</span></span>

<span data-ttu-id="60648-119">Hello slutet av den här kursen kan du köra två appar som Java-konsolen:</span><span class="sxs-lookup"><span data-stu-id="60648-119">At hello end of this tutorial you run two Java console apps:</span></span>

* <span data-ttu-id="60648-120">**simulerade enheten**, en modifierad version av hello app som skapats i hello [skicka moln till enhet meddelanden med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="60648-120">**simulated-device**, a modified version of hello app created in hello [Send Cloud-to-Device messages with IoT Hub] tutorial.</span></span> <span data-ttu-id="60648-121">Den här appen överför en fil toostorage med en SAS-URI som tillhandahålls av din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-121">This app uploads a file toostorage using a SAS URI provided by your IoT hub.</span></span>
* <span data-ttu-id="60648-122">**Läs filen-överför-meddelande**, som tar emot filen överföra meddelanden från din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-122">**read-file-upload-notification**, which receives file upload notifications from your IoT hub.</span></span>

> [!NOTE]
> <span data-ttu-id="60648-123">IoT-hubb stöder många vilka plattformar och språk (inklusive C, .NET och Javascript) via Azure IoT-enhet SDK: er.</span><span class="sxs-lookup"><span data-stu-id="60648-123">IoT Hub supports many device platforms and languages (including C, .NET, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="60648-124">Se toohello [Azure IoT Developer Center] stegvisa anvisningar för hur tooconnect din enhet tooAzure IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-124">Refer toohello [Azure IoT Developer Center] for step-by-step instructions on how tooconnect your device tooAzure IoT Hub.</span></span>

<span data-ttu-id="60648-125">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="60648-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="60648-126">Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="60648-126">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="60648-127">Maven 3</span><span class="sxs-lookup"><span data-stu-id="60648-127">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="60648-128">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="60648-128">An active Azure account.</span></span> <span data-ttu-id="60648-129">(Om du inte har ett konto kan du skapa en [kostnadsfritt konto](http://azure.microsoft.com/pricing/free-trial/) på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="60648-129">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-associate-storage](../../includes/iot-hub-associate-storage.md)]

## <a name="upload-a-file-from-a-device-app"></a><span data-ttu-id="60648-130">Överför en fil från en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="60648-130">Upload a file from a device app</span></span>

<span data-ttu-id="60648-131">I det här avsnittet kan du ändra hello enhetsapp som du skapade i [meddelanden moln till enhet med IoT-hubben](iot-hub-java-java-c2d.md) tooupload en fil tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-131">In this section, you modify hello device app you created in [Send Cloud-to-Device messages with IoT Hub](iot-hub-java-java-c2d.md) tooupload a file tooIoT hub.</span></span>

1. <span data-ttu-id="60648-132">Kopiera en bild filen toohello `simulated-device` mappen och Byt namn på den `myimage.png`.</span><span class="sxs-lookup"><span data-stu-id="60648-132">Copy an image file toohello `simulated-device` folder and rename it `myimage.png`.</span></span>

1. <span data-ttu-id="60648-133">Använd en textredigerare och öppna hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="60648-133">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="60648-134">Lägg till hello variabeldeklarationen toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="60648-134">Add hello variable declaration toohello **App** class:</span></span>

    ```java
    private static String fileName = "myimage.png";
    ```

1. <span data-ttu-id="60648-135">tooprocess fil överför motringning statusmeddelanden, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="60648-135">tooprocess file upload status callback messages, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Define a callback method tooprint status codes from IoT Hub.
    protected static class FileUploadStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toofile upload for " + fileName
            + " operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="60648-136">tooupload bilder tooIoT hubben, Lägg till följande metod toohello hello **App** klassen tooupload bilder tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="60648-136">tooupload images tooIoT Hub, add hello following method toohello **App** class tooupload images tooIoT Hub:</span></span>

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

1. <span data-ttu-id="60648-137">Ändra hello **huvudsakliga** metoden toocall hello **uploadFile** metod som visas i följande fragment hello:</span><span class="sxs-lookup"><span data-stu-id="60648-137">Modify hello **main** method toocall hello **uploadFile** method as shown in hello following snippet:</span></span>

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

1. <span data-ttu-id="60648-138">Använd hello följande kommando toobuild hello **simulerade enheten** appen och Sök efter fel:</span><span class="sxs-lookup"><span data-stu-id="60648-138">Use hello following command toobuild hello **simulated-device** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="receive-a-file-upload-notification"></a><span data-ttu-id="60648-139">Ta emot ett meddelande om överföringen</span><span class="sxs-lookup"><span data-stu-id="60648-139">Receive a file upload notification</span></span>

<span data-ttu-id="60648-140">I det här avsnittet skapar du en Java-konsolapp som tar emot filen överföra meddelanden från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="60648-140">In this section, you create a Java console app that receives file upload notification messages from IoT Hub.</span></span>

<span data-ttu-id="60648-141">Du behöver hello **iothubowner** anslutningssträngen för din IoT-hubb toocomplete i det här avsnittet.</span><span class="sxs-lookup"><span data-stu-id="60648-141">You need hello **iothubowner** connection string for your IoT Hub toocomplete this section.</span></span> <span data-ttu-id="60648-142">Du hittar hello anslutningssträngen i hello [Azure-portalen](https://portal.azure.com/) på hello **princip för delad åtkomst** bladet.</span><span class="sxs-lookup"><span data-stu-id="60648-142">You can find hello connection string in hello [Azure portal](https://portal.azure.com/) on hello **Shared access policy** blade.</span></span>

1. <span data-ttu-id="60648-143">Skapa ett Maven-projekt som kallas **läsa filen-överför-meddelande** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="60648-143">Create a Maven project called **read-file-upload-notification** using hello following command at your command prompt.</span></span> <span data-ttu-id="60648-144">Observera att det här kommandot är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="60648-144">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-file-upload-notification -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

1. <span data-ttu-id="60648-145">Kommandotolken, gå toohello nya `read-file-upload-notification` mapp.</span><span class="sxs-lookup"><span data-stu-id="60648-145">At your command prompt, navigate toohello new `read-file-upload-notification` folder.</span></span>

1. <span data-ttu-id="60648-146">Använd en textredigerare och öppna hello `pom.xml` filen i hello `read-file-upload-notification` mapp och Lägg till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="60648-146">Using a text editor, open hello `pom.xml` file in hello `read-file-upload-notification` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="60648-147">Lägger till hello beroende kan du toouse hello **iothub-java-service-klient** paketet i ditt program toocommunicate med din IoT-hubb-tjänst:</span><span class="sxs-lookup"><span data-stu-id="60648-147">Adding hello dependency enables you toouse hello **iothub-java-service-client** package in your application toocommunicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="60648-148">Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="60648-148">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="60648-149">Spara och Stäng hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="60648-149">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="60648-150">Använd en textredigerare och öppna hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="60648-150">Using a text editor, open hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="60648-151">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="60648-151">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.concurrent.ExecutorService;
    import java.util.concurrent.Executors;
    ```

1. <span data-ttu-id="60648-152">Lägg till följande variabler klassen på toohello hello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="60648-152">Add hello following class-level variables toohello **App** class:</span></span>

    ```java
    private static final String connectionString = "{Your IoT Hub connection string}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    private static FileUploadNotificationReceiver fileUploadNotificationReceiver = null;
    ```

1. <span data-ttu-id="60648-153">tooprint information om hello filen överför toohello-konsolen Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="60648-153">tooprint information about hello file upload toohello console, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="60648-154">toostart hello tråd som lyssnar efter filen överför meddelanden, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="60648-154">toostart hello thread that listens for file upload notifications, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="60648-155">Spara och Stäng hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="60648-155">Save and close hello `read-file-upload-notification\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="60648-156">Använd hello följande kommando toobuild hello **läsa filen-överför-meddelande** appen och Sök efter fel:</span><span class="sxs-lookup"><span data-stu-id="60648-156">Use hello following command toobuild hello **read-file-upload-notification** app and check for errors:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="60648-157">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="60648-157">Run hello applications</span></span>

<span data-ttu-id="60648-158">Nu är du redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="60648-158">Now you are ready toorun hello applications.</span></span>

<span data-ttu-id="60648-159">Vid en kommandotolk i hello `read-file-upload-notification` mapp, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="60648-159">At a command prompt in hello `read-file-upload-notification` folder, run hello following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="60648-160">Vid en kommandotolk i hello `simulated-device` mapp, kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="60648-160">At a command prompt in hello `simulated-device` folder, run hello following command:</span></span>

```cmd/sh
mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
```

<span data-ttu-id="60648-161">hello följande skärmbild visar hello utdata från hello **simulerade enheten** app:</span><span class="sxs-lookup"><span data-stu-id="60648-161">hello following screenshot shows hello output from hello **simulated-device** app:</span></span>

![Utdata från simulerade enheten app](media/iot-hub-java-java-upload/simulated-device.png)

<span data-ttu-id="60648-163">hello följande skärmbild visar hello utdata från hello **läsa filen-överför-meddelande** app:</span><span class="sxs-lookup"><span data-stu-id="60648-163">hello following screenshot shows hello output from hello **read-file-upload-notification** app:</span></span>

![Utdata från Läs-filen-överför-meddelandeprogram](media/iot-hub-java-java-upload/read-file-upload-notification.png)

<span data-ttu-id="60648-165">Du kan använda hello portal tooview hello upp filen i hello lagringsbehållare som du har konfigurerat:</span><span class="sxs-lookup"><span data-stu-id="60648-165">You can use hello portal tooview hello uploaded file in hello storage container you configured:</span></span>

![Överförda filen](media/iot-hub-java-java-upload/uploaded-file.png)

## <a name="next-steps"></a><span data-ttu-id="60648-167">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="60648-167">Next steps</span></span>

<span data-ttu-id="60648-168">I kursen får du lärt dig hur toouse hello filöverföring funktionerna i IoT-hubb toosimplify filöverföringar från enheter.</span><span class="sxs-lookup"><span data-stu-id="60648-168">In this tutorial, you learned how toouse hello file upload capabilities of IoT Hub toosimplify file uploads from devices.</span></span> <span data-ttu-id="60648-169">Du kan fortsätta tooexplore IoT-hubb funktioner och scenarier med hello följande artiklar:</span><span class="sxs-lookup"><span data-stu-id="60648-169">You can continue tooexplore IoT hub features and scenarios with hello following articles:</span></span>

* <span data-ttu-id="60648-170">[Skapa en IoT-hubb programmässigt][lnk-create-hub]</span><span class="sxs-lookup"><span data-stu-id="60648-170">[Create an IoT hub programmatically][lnk-create-hub]</span></span>
* <span data-ttu-id="60648-171">[Introduktion tooC SDK][lnk-c-sdk]</span><span class="sxs-lookup"><span data-stu-id="60648-171">[Introduction tooC SDK][lnk-c-sdk]</span></span>
* <span data-ttu-id="60648-172">[Azure IoT-SDK][lnk-sdks]</span><span class="sxs-lookup"><span data-stu-id="60648-172">[Azure IoT SDKs][lnk-sdks]</span></span>

<span data-ttu-id="60648-173">toofurther utforska hello funktionerna i IoT Hub, se:</span><span class="sxs-lookup"><span data-stu-id="60648-173">toofurther explore hello capabilities of IoT Hub, see:</span></span>

* <span data-ttu-id="60648-174">[Simulera en enhet med IoT kant][lnk-iotedge]</span><span class="sxs-lookup"><span data-stu-id="60648-174">[Simulating a device with IoT Edge][lnk-iotedge]</span></span>

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


