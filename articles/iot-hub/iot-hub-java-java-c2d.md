---
title: Moln till enhet meddelanden med Azure IoT Hub (Java) | Microsoft Docs
description: "Hur du skickar meddelanden moln till enhet till en enhet från en Azure IoT-hubb med hjälp av Azure IoT-SDK för Java. Du kan ändra en simulerad enhetsapp för att ta emot meddelanden moln till enhet och ändra en backend-app för att skicka meddelanden moln till enhet."
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
ms.openlocfilehash: f5e3ac46f4d144b12e2ab7fcfb456665ff6cc68f
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="send-cloud-to-device-messages-with-iot-hub-java"></a><span data-ttu-id="3a5f7-104">Skicka meddelanden moln till enhet med IoT-hubb (Java)</span><span class="sxs-lookup"><span data-stu-id="3a5f7-104">Send cloud-to-device messages with IoT Hub (Java)</span></span>
[!INCLUDE [iot-hub-selector-c2d](../../includes/iot-hub-selector-c2d.md)]

<span data-ttu-id="3a5f7-105">Azure IoT Hub är en helt hanterad tjänst som hjälper till att aktivera tillförlitlig och säker dubbelriktad kommunikation mellan miljoner enheter och en lösning tillbaka avslutas.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-105">Azure IoT Hub is a fully managed service that helps enable reliable and secure bi-directional communications between millions of devices and a solution back end.</span></span> <span data-ttu-id="3a5f7-106">Den [Kom igång med IoT-hubb] kursen visar hur du skapar en IoT-hubb, etablera en enhetsidentitet i den och code en simulerad enhetsapp som skickar meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-106">The [Get started with IoT Hub] tutorial shows how to create an IoT hub, provision a device identity in it, and code a simulated device app that sends device-to-cloud messages.</span></span>

<span data-ttu-id="3a5f7-107">Den här kursen bygger på [Kom igång med IoT-hubb].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-107">This tutorial builds on [Get started with IoT Hub].</span></span> <span data-ttu-id="3a5f7-108">Den visar hur till:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-108">It shows you how to:</span></span>

* <span data-ttu-id="3a5f7-109">Skicka meddelanden moln till enhet på en enhet till IoT-hubb från din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-109">From your solution back end, send cloud-to-device messages to a single device through IoT Hub.</span></span>
* <span data-ttu-id="3a5f7-110">Ta emot meddelanden moln till enhet på en enhet.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-110">Receive cloud-to-device messages on a device.</span></span>
* <span data-ttu-id="3a5f7-111">Begära leverans bekräftelse från din lösningens serverdel (*feedback*) för meddelanden som skickas till en enhet från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-111">From your solution back end, request delivery acknowledgement (*feedback*) for messages sent to a device from IoT Hub.</span></span>

<span data-ttu-id="3a5f7-112">Du hittar mer information om moln till enhet meddelanden i den [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-112">You can find more information on cloud-to-device messages in the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

<span data-ttu-id="3a5f7-113">I slutet av den här kursen kan köra du två Java-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-113">At the end of this tutorial, you run two Java console apps:</span></span>

* <span data-ttu-id="3a5f7-114">**simulerade enheten**, en modifierad version av app som skapats i [Kom igång med IoT-hubb], som ansluter till din IoT-hubb och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-114">**simulated-device**, a modified version of the app created in [Get started with IoT Hub], which connects to your IoT hub and receives cloud-to-device messages.</span></span>
* <span data-ttu-id="3a5f7-115">**Skicka c2d meddelanden**, som skickar ett moln till enhet-meddelande till appen simulerade enheten via IoT-hubb och tar emot dess leverans bekräftelse.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-115">**send-c2d-messages**, which sends a cloud-to-device message to the simulated device app through IoT Hub, and then receives its delivery acknowledgement.</span></span>

> [!NOTE]
> <span data-ttu-id="3a5f7-116">IoT-hubben har SDK stöd för många enhetsplattformar och språk (inklusive C, Java och Javascript) via SDK för Azure IoT-enhet.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-116">IoT Hub has SDK support for many device platforms and languages (including C, Java, and Javascript) through Azure IoT device SDKs.</span></span> <span data-ttu-id="3a5f7-117">Stegvisa instruktioner om hur du ansluter enheten till den här självstudiekursen kod och vanligtvis Azure IoT Hub finns i [Azure IoT Developer Center].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-117">For step-by-step instructions on how to connect your device to this tutorial's code, and generally to Azure IoT Hub, see the [Azure IoT Developer Center].</span></span>

<span data-ttu-id="3a5f7-118">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-118">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="3a5f7-119">En fullständig fungerande version av den [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) eller [processen IoT-hubb meddelanden från enhet till moln](iot-hub-java-java-process-d2c.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-119">A complete working version of the [Get started with IoT Hub](iot-hub-java-java-getstarted.md) or [Process IoT Hub device-to-cloud messages](iot-hub-java-java-process-d2c.md) tutorial.</span></span>
* <span data-ttu-id="3a5f7-120">Senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="3a5f7-120">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="3a5f7-121">Maven 3</span><span class="sxs-lookup"><span data-stu-id="3a5f7-121">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="3a5f7-122">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-122">An active Azure account.</span></span> <span data-ttu-id="3a5f7-123">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="3a5f7-123">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

## <a name="receive-messages-in-the-simulated-device-app"></a><span data-ttu-id="3a5f7-124">Ta emot meddelanden i appen simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="3a5f7-124">Receive messages in the simulated device app</span></span>

<span data-ttu-id="3a5f7-125">I det här avsnittet kan du ändra den simulerade enhetsapp som du skapade i [Kom igång med IoT-hubb] ta emot meddelanden moln till enhet från IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-125">In this section, you modify the simulated device app you created in [Get started with IoT Hub] to receive cloud-to-device messages from the IoT hub.</span></span>

1. <span data-ttu-id="3a5f7-126">Öppna filen simulated-device\src\main\java\com\mycompany\app\App.java i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-126">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

2. <span data-ttu-id="3a5f7-127">Lägg till följande **MessageCallback** klass som en kapslad klass inuti den **App** klass.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-127">Add the following **MessageCallback** class as a nested class inside the **App** class.</span></span> <span data-ttu-id="3a5f7-128">Den **köra** -metoden har anropats när enheten tar emot ett meddelande från IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-128">The **execute** method is invoked when the device receives a message from IoT Hub.</span></span> <span data-ttu-id="3a5f7-129">I det här exemplet meddelar enheten alltid IoT-hubben att det har slutförts meddelandet:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-129">In this example, the device always notifies the IoT hub that it has completed the message:</span></span>

    ```java
    private static class AppMessageCallback implements MessageCallback {
      public IotHubMessageResult execute(Message msg, Object context) {
        System.out.println("Received message from hub: "
          + new String(msg.getBytes(), Message.DEFAULT_IOTHUB_MESSAGE_CHARSET));
    
        return IotHubMessageResult.COMPLETE;
      }
    }
    ```
3. <span data-ttu-id="3a5f7-130">Ändra den **huvudsakliga** metoden för att skapa en **AppMessageCallback** -instans och anrop i **setMessageCallback** metoden innan det öppnar klienten på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-130">Modify the **main** method to create an **AppMessageCallback** instance and call the **setMessageCallback** method before it opens the client as follows:</span></span>

    ```java
    client = new DeviceClient(connString, protocol);
   
    MessageCallback callback = new AppMessageCallback();
    client.setMessageCallback(callback, null);
    client.open();
    ```

    > [!NOTE]
    > <span data-ttu-id="3a5f7-131">Om du använder HTTP i stället för MQTT eller AMQP som transport, den **DeviceClient** instans söker efter meddelanden från IoT-hubb sällan (mindre än var 25: e minut).</span><span class="sxs-lookup"><span data-stu-id="3a5f7-131">If you use HTTP instead of MQTT or AMQP as the transport, the **DeviceClient** instance checks for messages from IoT Hub infrequently (less than every 25 minutes).</span></span> <span data-ttu-id="3a5f7-132">Mer information om skillnaderna mellan MQTT, AMQP och HTTP-stöd och IoT-hubb begränsning finns på [IoT-hubb Utvecklarhandbok][IoT Hub developer guide - C2D].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-132">For more information about the differences between MQTT, AMQP and HTTP support, and IoT Hub throttling, see the [IoT Hub developer guide][IoT Hub developer guide - C2D].</span></span>

4. <span data-ttu-id="3a5f7-133">Skapa appen **simulated-device** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen simulated-device:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-133">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="send-a-cloud-to-device-message"></a><span data-ttu-id="3a5f7-134">Skicka ett meddelande moln till enhet</span><span class="sxs-lookup"><span data-stu-id="3a5f7-134">Send a cloud-to-device message</span></span>

<span data-ttu-id="3a5f7-135">I det här avsnittet skapar du en Java-konsolapp som skickar moln till enhet meddelanden i appen simulerade enheten.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-135">In this section, you create a Java console app that sends cloud-to-device messages to the simulated device app.</span></span> <span data-ttu-id="3a5f7-136">Du behöver enhets-ID på den enhet som du lade till i den [Kom igång med IoT-hubb] kursen.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-136">You need the device ID of the device you added in the [Get started with IoT Hub] tutorial.</span></span> <span data-ttu-id="3a5f7-137">Du måste också anslutningssträngen IoT-hubb för din hubb hittar du i den [Azure-portalen].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-137">You also need the IoT Hub connection string for your hub that you can find in the [Azure portal].</span></span>

1. <span data-ttu-id="3a5f7-138">Skapa ett Maven-projekt som kallas **skicka c2d meddelanden** med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-138">Create a Maven project called **send-c2d-messages** using the following command at your command prompt.</span></span> <span data-ttu-id="3a5f7-139">Observera att det här kommandot är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-139">Note this command is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=send-c2d-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="3a5f7-140">Navigera till mappen Skicka c2d meddelanden i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-140">At your command prompt, navigate to the new send-c2d-messages folder.</span></span>

3. <span data-ttu-id="3a5f7-141">Använd en textredigerare och öppna filen pom.xml i mappen Skicka c2d meddelanden och Lägg till följande beroende på den **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-141">Using a text editor, open the pom.xml file in the send-c2d-messages folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="3a5f7-142">Lägga till beroendet kan du använda den **iothub-java-service-klient** paketet i ditt program för att kommunicera med din IoT-hubb-tjänst:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-142">Adding the dependency enables you to use the **iothub-java-service-client** package in your application to communicate with your IoT hub service:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="3a5f7-143">Du kan söka efter den senaste versionen av **iot-service-client** med [Maven-sökning][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-143">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="3a5f7-144">Spara och stäng filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-144">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="3a5f7-145">Använd en textredigerare och öppna filen send-c2d-messages\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-145">Using a text editor, open the send-c2d-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="3a5f7-146">Lägg till följande **Import**-instruktioner i filen:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-146">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.*;
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="3a5f7-147">Lägg till följande klassnivå variabler till den **App** class, ersätter **{yourhubconnectionstring}** och **{yourdeviceid}** med värden din anges tidigare:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-147">Add the following class-level variables to the **App** class, replacing **{yourhubconnectionstring}** and **{yourdeviceid}** with the values your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "{yourdeviceid}";
    private static final IotHubServiceClientProtocol protocol = IotHubServiceClientProtocol.AMQPS;
    ```

8. <span data-ttu-id="3a5f7-148">Ersätt den **huvudsakliga** metoden med följande kod.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-148">Replace the **main** method with the following code.</span></span> <span data-ttu-id="3a5f7-149">Den här koden ansluter till din IoT-hubb skickar ett meddelande till din enhet och väntar sedan en bekräftelse att enheten emot och bearbetas meddelandet:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-149">This code connects to your IoT hub, sends a message to your device, and then waits for an acknowledgment that the device received and processed the message:</span></span>
   
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
   
        Message messageToSend = new Message("Cloud to device message.");
        messageToSend.setDeliveryAcknowledgement(DeliveryAcknowledgement.Full);
   
        serviceClient.send(deviceId, messageToSend);
        System.out.println("Message sent to device");
   
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
    > <span data-ttu-id="3a5f7-150">Den här självstudiekursen implementerar inte några återförsöksprincip sätt.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-150">For simplicity's sake, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="3a5f7-151">I produktionskod, bör du implementera försök principer (till exempel exponentiell backoff), enligt förslaget i MSDN-artikel [hantering av tillfälliga fel].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-151">In production code, you should implement retry policies (such as exponential backoff), as suggested in the MSDN article [Transient Fault Handling].</span></span>


9. <span data-ttu-id="3a5f7-152">Skapa appen **simulated-device** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen simulated-device:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-152">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="run-the-applications"></a><span data-ttu-id="3a5f7-153">Köra programmen</span><span class="sxs-lookup"><span data-stu-id="3a5f7-153">Run the applications</span></span>

<span data-ttu-id="3a5f7-154">Nu är det dags att köra programmen.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-154">You are now ready to run the applications.</span></span>

1. <span data-ttu-id="3a5f7-155">Kör följande kommando börjar skicka telemetri för din IoT-hubb och lyssnar efter meddelanden från din hubb moln till enhet i en kommandotolk i mappen simulerade enhet:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-155">At a command prompt in the simulated-device folder, run the following command to begin sending telemetry to your IoT hub and to listen for cloud-to-device messages sent from your hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Kör appen simulerade enheten][img-simulated-device]

2. <span data-ttu-id="3a5f7-157">Kör följande kommando för att skicka meddelandet moln till enhet och väntar på bekräftelse en feedback i en kommandotolk i mappen Skicka c2d meddelanden:</span><span class="sxs-lookup"><span data-stu-id="3a5f7-157">At a command prompt in the send-c2d-messages folder, run the following command to send a cloud-to-device message and wait for a feedback acknowledgment:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Kör kommandot för att skicka meddelandet moln till enhet][img-send-command]

## <a name="next-steps"></a><span data-ttu-id="3a5f7-159">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="3a5f7-159">Next steps</span></span>

<span data-ttu-id="3a5f7-160">I den här självstudiekursen beskrivs hur du skickar och tar emot meddelanden moln till enhet.</span><span class="sxs-lookup"><span data-stu-id="3a5f7-160">In this tutorial, you learned how to send and receive cloud-to-device messages.</span></span> 

<span data-ttu-id="3a5f7-161">Exempel på fullständiga lösningar för slutpunkt till slutpunkt med IoT-hubb finns [Azure IoT Suite].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-161">To see examples of complete end-to-end solutions that use IoT Hub, see [Azure IoT Suite].</span></span>

<span data-ttu-id="3a5f7-162">Mer information om hur du utvecklar lösningar med IoT-hubb finns i [IoT-hubb Utvecklarhandbok].</span><span class="sxs-lookup"><span data-stu-id="3a5f7-162">To learn more about developing solutions with IoT Hub, see the [IoT Hub developer guide].</span></span>

<!-- Images -->
[img-simulated-device]: media/iot-hub-java-java-c2d/receivec2d.png
[img-send-command]:  media/iot-hub-java-java-c2d/sendc2d.png
<!-- Links -->

<span data-ttu-id="3a5f7-163">[Kom igång med IoT-hubb]: iot-hub-java-java-getstarted.md</span><span class="sxs-lookup"><span data-stu-id="3a5f7-163">[Get started with IoT Hub]: iot-hub-java-java-getstarted.md</span></span>
[IoT Hub developer guide - C2D]: iot-hub-devguide-messaging.md
<span data-ttu-id="3a5f7-164">[IoT-hubb Utvecklarhandbok]: iot-hub-devguide.md</span><span class="sxs-lookup"><span data-stu-id="3a5f7-164">[IoT Hub developer guide]: iot-hub-devguide.md</span></span>
<span data-ttu-id="3a5f7-165">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span><span class="sxs-lookup"><span data-stu-id="3a5f7-165">[Azure IoT Developer Center]: http://www.azure.com/develop/iot</span></span>
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java
<span data-ttu-id="3a5f7-166">[hantering av tillfälliga fel]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span><span class="sxs-lookup"><span data-stu-id="3a5f7-166">[Transient Fault Handling]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx</span></span>
<span data-ttu-id="3a5f7-167">[Azure-portalen]: https://portal.azure.com</span><span class="sxs-lookup"><span data-stu-id="3a5f7-167">[Azure portal]: https://portal.azure.com</span></span>
<span data-ttu-id="3a5f7-168">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span><span class="sxs-lookup"><span data-stu-id="3a5f7-168">[Azure IoT Suite]: https://azure.microsoft.com/documentation/suites/iot-suite/</span></span>
[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22