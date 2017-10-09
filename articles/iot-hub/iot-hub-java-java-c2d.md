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
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a><span data-ttu-id="ba21f-104">Skicka meddelanden moln till enhet med IoT-hubb (Java)</span><span class="sxs-lookup"><span data-stu-id="ba21f-104">Send cloud-to-device messages with IoT Hub (Java)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

<span data-ttu-id="ba21f-105">Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="ba21f-105">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="ba21f-106">Hej [Kom igång med IoT-hubb] kursen visar hur toocreate en IoT-hubb etablera en enhetsidentitet i den och code en simulerad enhetsapp som skickar meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="ba21f-106">hello [Get started with IoT Hub] tutorial shows how toocreate an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="ba21f-107">Den här kursen bygger på [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="ba21f-107">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="ba21f-108">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="ba21f-108">It shows you how to:</span></span>

* <span data-ttu-id="ba21f-109">Skicka meddelanden moln till enhet tooa enhet via IoT-hubb från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="ba21f-109">From your solution back end, send cloud-to-device messages tooa single device through IoT Hub.</span></span>
* <span data-ttu-id="ba21f-110">Ta emot meddelanden moln till enhet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="ba21f-110">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="ba21f-111">Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas tooa enheten från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ba21f-111">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent tooa device from IoT Hub.</span></span>

<span data-ttu-id="ba21f-112">Du hittar mer information om moln till enhet meddelanden i hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="ba21f-112">You can find more information on cloud-to-device messages in hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="ba21f-113">Hello slutet av den här självstudiekursen kör du två appar som Java-konsolen:</span><span class="sxs-lookup"><span data-stu-id="ba21f-113">At hello end of this tutorial, you run two Java console apps:</span></span>

* <span data-ttu-id="ba21f-114">**simulerade enheten**, en modifierad version av hello-app som skapats i [Kom igång med IoT-hubb], som ansluter tooyour IoT-hubb och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="ba21f-114">**simulated-device**, a modified version of hello app created in [Get started with IoT Hub], which connects tooyour IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="ba21f-115">**Skicka c2d meddelanden**, som skickar ett moln-till-enhetsmeddelande toohello simulerade enheten appen via IoT-hubb och tar emot dess leverans bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="ba21f-115">**send-c2d-messages**, which sends a cloud-to-device message toohello simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="ba21f-116">IoT-hubben har SDK stöd för många enhetsplattformar och språk (inklusive C, Java och Javascript) via SDK för Azure IoT-enhet.</span><span class="sxs-lookup"><span data-stu-id="ba21f-116">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="ba21f-117">Stegvisa instruktioner om hur tooconnect enhet toothis kursens kod och vanligtvis tooAzure IoT Hub, se hello [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="ba21f-117">For step-by-step instructions on how tooconnect your device toothis tutorial's code, and generally tooAzure IoT Hub, see hello [Azure IoT Developer Center].</span></span>

<span data-ttu-id="ba21f-118">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="ba21f-118">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="ba21f-119">En fullständig fungerande version av hello [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) eller [processen IoT-hubb meddelanden från enhet till moln](iot-hub-java-java-process-d2c.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="ba21f-119">A complete working version of hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) or [Process IoT Hub device-to-cloud messages](iot-hub-java-java-process-d2c.md) tutorial.</span></span>
* <span data-ttu-id="ba21f-120">Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="ba21f-120">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="ba21f-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="ba21f-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="ba21f-122">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ba21f-122">An active Azure account.</span></span> <span data-ttu-id="ba21f-123">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="ba21f-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-hello-simulated-device-app"></a><span data-ttu-id="ba21f-124">Ta emot meddelanden i hello simulerade enhetsapp</span><span class="sxs-lookup"><span data-stu-id="ba21f-124">Receive messages in hello simulated device app</span></span>

<span data-ttu-id="ba21f-125">I det här avsnittet kan du ändra hello simulerade enhetsapp som du skapade i [Kom igång med IoT-hubb] tooreceive moln till enhet meddelanden från hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ba21f-125">In this section, you modify hello simulated device app you created in [Get started with IoT Hub] tooreceive cloud-to-device messages from hello IoT hub.</span></span>

1. <span data-ttu-id="ba21f-126">Använd en textredigerare och öppna hello simulated-device\src\main\java\com\mycompany\app\App.java filen.</span><span class="sxs-lookup"><span data-stu-id="ba21f-126">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

2. <span data-ttu-id="ba21f-127">Lägg till följande hello **MessageCallback** klass som en kapslad klass i hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="ba21f-127">Add hello following **MessageCallback** class as a nested class inside hello **App** class.</span></span> <span data-ttu-id="ba21f-128">Hej **köra** -metoden har anropats när hello enheten tar emot ett meddelande från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ba21f-128">hello **execute** method is invoked when hello device receives a message from IoT Hub.</span></span> <span data-ttu-id="ba21f-129">I det här exemplet meddelar hello enhet alltid hello IoT-hubb som det har slutförts hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="ba21f-129">In this example, hello device always notifies hello IoT hub that it has completed hello message:</span></span>

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. <span data-ttu-id="ba21f-130">Ändra hello **huvudsakliga** metoden toocreate en **AppMessageCallback** -instans och anrop hello **setMessageCallback** metoden innan det öppnar hello-klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="ba21f-130">Modify hello **main** method toocreate an **AppMessageCallback** instance and call hello **setMessageCallback** method before it opens hello client as follows:</span></span>

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > <span data-ttu-id="ba21f-131">Om du använder HTTP i stället för MQTT eller AMQP som hello transport hello **DeviceClient** instans söker efter meddelanden från IoT-hubb sällan (mindre än var 25: e minut).</span><span class="sxs-lookup"><span data-stu-id="ba21f-131">If you use HTTP instead of MQTT or AMQP as hello transport, hello **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="ba21f-132">Mer information om hello skillnader mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns hello [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="ba21f-132">For more information about hello differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see hello [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

4. <span data-ttu-id="ba21f-133">toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:</span><span class="sxs-lookup"><span data-stu-id="ba21f-133">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="ba21f-134">Skicka ett meddelande moln till enhet</span><span class="sxs-lookup"><span data-stu-id="ba21f-134">Send a cloud-to-device message</span></span>

<span data-ttu-id="ba21f-135">I det här avsnittet skapar du en Java-konsolapp som skickar meddelanden moln till enhet toohello simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="ba21f-135">In this section, you create a Java console app that sends cloud-to-device messages toohello simulated device app.</span></span> <span data-ttu-id="ba21f-136">Du behöver hello enhets-ID för hello-enhet som du lade till i hello [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="ba21f-136">You need hello device ID of hello device you added in hello [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="ba21f-137">Du måste också hello anslutningssträngen för IoT-hubb för din hubb hittar du i hello [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="ba21f-137">You also need hello IoT Hub connection string for your hub that you can find in hello [Azure portal].</span></span>

1. <span data-ttu-id="ba21f-138">Skapa ett Maven-projekt som kallas **skicka c2d meddelanden** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ba21f-138">Create a Maven project called **send-c2d-messages** using hello following command at your command prompt.</span></span> <span data-ttu-id="ba21f-139">Observera att det här kommandot är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="ba21f-139">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="ba21f-140">Navigera toohello ny skicka c2d meddelanden mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ba21f-140">At your command prompt, navigate toohello new send-c2d-messages folder.</span></span>

3. <span data-ttu-id="ba21f-141">Med en textredigerare, öppna hello pom.xml filen hello skicka c2d meddelanden mapp och lägga till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="ba21f-141">Using a text editor, open hello pom.xml file in hello send-c2d-messages folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="ba21f-142">Lägger till hello beroende kan du toouse hello **iothub-java-service-klient** paketet i ditt program toocommunicate med din IoT-hubb-tjänst:</span><span class="sxs-lookup"><span data-stu-id="ba21f-142">Adding hello dependency enables you toouse hello **iothub-java-service-client** package in your application toocommunicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="ba21f-143">Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="ba21f-143">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="ba21f-144">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="ba21f-144">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="ba21f-145">Använd en textredigerare och öppna hello send-c2d-messages\src\main\java\com\mycompany\app\App.java filen.</span><span class="sxs-lookup"><span data-stu-id="ba21f-145">Using a text editor, open hello send-c2d-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="ba21f-146">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="ba21f-146">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="ba21f-147">Lägg till följande variabler klassen på toohello hello **App** class, ersätter **{yourhubconnectionstring}** och **{yourdeviceid}** med hello värden din anges tidigare:</span><span class="sxs-lookup"><span data-stu-id="ba21f-147">Add hello following class-level variables toohello **App** class, replacing **{yourhubconnectionstring}** and **{yourdeviceid}** with hello values your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. <span data-ttu-id="ba21f-148">Ersätt hello **huvudsakliga** metod med hello följande kod.</span><span class="sxs-lookup"><span data-stu-id="ba21f-148">Replace hello **main** method with hello following code.</span></span> <span data-ttu-id="ba21f-149">Den här koden ansluter tooyour IoT-hubb skickar ett meddelande tooyour enhet och väntar sedan en bekräftelse att hello enheten tagits emot och bearbetat hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="ba21f-149">This code connects tooyour IoT hub, sends a message tooyour device, and then waits for an acknowledgment that hello device received and processed hello message:</span></span>
   
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
    > <span data-ttu-id="ba21f-150">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="ba21f-150">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="ba21f-151">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="ba21f-151">In production code, you should implement retry policies (such as exponential backoff), as suggested in hello MSDN article [Transient Fault Handling].</span></span>


9. <span data-ttu-id="ba21f-152">toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:</span><span class="sxs-lookup"><span data-stu-id="ba21f-152">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-hello-applications"></a><span data-ttu-id="ba21f-153">Köra hello program</span><span class="sxs-lookup"><span data-stu-id="ba21f-153">Run hello applications</span></span>

<span data-ttu-id="ba21f-154">Du är nu redo toorun hello program.</span><span class="sxs-lookup"><span data-stu-id="ba21f-154">You are now ready toorun hello applications.</span></span>

1. <span data-ttu-id="ba21f-155">Kör följande kommando toobegin skicka telemetri tooyour IoT-hubb och toolisten för moln till enhet meddelanden som skickas från din hubb hello vid en kommandotolk i hello simulerade enheten mapp:</span><span class="sxs-lookup"><span data-stu-id="ba21f-155">At a command prompt in hello simulated-device folder, run hello following command toobegin sending telemetry tooyour IoT hub and toolisten for cloud-to-device messages sent from your hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Kör hello simulerade enhetsapp][img-simulated-device]

2. <span data-ttu-id="ba21f-157">Vid en kommandotolk i hello skicka c2d meddelanden mapp, kör du hello efter kommandot toosend ett moln-till-enhetsmeddelande och vänta tills en feedback-bekräftelse:</span><span class="sxs-lookup"><span data-stu-id="ba21f-157">At a command prompt in hello send-c2d-messages folder, run hello following command toosend a cloud-to-device message and wait for a feedback acknowledgment:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Kör kommandot toosend hello moln till enhet hälsningsmeddelande][img-send-command]

## <a name="next-steps"></a><span data-ttu-id="ba21f-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba21f-159">Next steps</span></span>

<span data-ttu-id="ba21f-160">I kursen får du lära sig hur toosend och ta emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="ba21f-160">In this tutorial, you learned how toosend and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="ba21f-161">toosee exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="ba21f-161">toosee examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="ba21f-162">toolearn mer information om hur du utvecklar lösningar med IoT-hubb finns hello [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="ba21f-162">toolearn more about developing solutions with IoT Hub, see hello [IoT Hub developer guide].</span></span>

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