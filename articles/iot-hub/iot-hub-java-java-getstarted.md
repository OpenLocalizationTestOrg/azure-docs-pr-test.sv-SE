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
ms.openlocfilehash: 707356a49970bcd76a55ee1b8a6fbddf6a6ba390
ms.sourcegitcommit: 18ad9bc049589c8e44ed277f8f43dcaa483f3339
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/29/2017
---
# <a name="connect-your-device-to-your-iot-hub-using-java"></a><span data-ttu-id="d1a74-104">Anslut din enhet till IoT Hub med hjälp av Java</span><span class="sxs-lookup"><span data-stu-id="d1a74-104">Connect your device to your IoT hub using Java</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="d1a74-105">I slutet av den här självstudiekursen har du tre Java-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="d1a74-105">At the end of this tutorial, you have three Java console apps:</span></span>

* <span data-ttu-id="d1a74-106">**create-device-identity**, som skapar en enhetsidentitet och en associerad säkerhetsnyckel för att ansluta din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="d1a74-106">**create-device-identity**, which creates a device identity and associated security key to connect your device app.</span></span>
* <span data-ttu-id="d1a74-107">**read-d2c-messages**, som visar telemetri som skickas av din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="d1a74-107">**read-d2c-messages**, which displays the telemetry sent by your device app.</span></span>
* <span data-ttu-id="d1a74-108">**simulated-device**, som ansluter till din IoT Hub med enhetsidentiteten som skapades tidigare och som skickar ett telemetrimeddelande varje sekund med hjälp av MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="d1a74-108">**simulated-device**, which connects to your IoT hub with the device identity created earlier, and sends a telemetry message every second using the MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="d1a74-109">Artikeln om [Azure IoT SDK:er][lnk-hub-sdks] innehåller information om Azure IoT SDK:er som du kan använda för att skapa båda apparna så att de kan köras på enheter och på lösningens backend-servrar.</span><span class="sxs-lookup"><span data-stu-id="d1a74-109">The article [Azure IoT SDKs][lnk-hub-sdks] provides information about the Azure IoT SDKs that you can use to build both apps to run on devices and your solution back end.</span></span>

<span data-ttu-id="d1a74-110">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="d1a74-110">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="d1a74-111">Senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="d1a74-111">The latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span> 
* [<span data-ttu-id="d1a74-112">Maven 3</span><span class="sxs-lookup"><span data-stu-id="d1a74-112">Maven 3</span></span>](https://maven.apache.org/install.html) 
* <span data-ttu-id="d1a74-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="d1a74-113">An active Azure account.</span></span> <span data-ttu-id="d1a74-114">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="d1a74-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="d1a74-115">Som ett sista steg antecknar du **primärnyckelvärdet**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-115">As a final step, make a note of the **Primary key** value.</span></span> <span data-ttu-id="d1a74-116">Klicka sedan på **Slutpunkter** och den inbyggda slutpunkten **Händelser**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-116">Then click **Endpoints** and the **Events** built-in endpoint.</span></span> <span data-ttu-id="d1a74-117">På bladet **Egenskaper** noterar du **Händelsehubb-kompatibelt namn** och adressen i **Händelsehubb-kompatibel slutpunkt**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-117">On the **Properties** blade, make a note of the **Event Hub-compatible name** and the **Event Hub-compatible endpoint** address.</span></span> <span data-ttu-id="d1a74-118">Du behöver dessa tre värden när du skapar appen **read-d2c-messages**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-118">You need these three values when you create your **read-d2c-messages** app.</span></span>

![Aviseringsblad, Azure-portal, IoT Hub][6]

<span data-ttu-id="d1a74-120">Nu har du skapat din IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-120">You have now created your IoT hub.</span></span> <span data-ttu-id="d1a74-121">Du har IoT Hub-värdnamnet, IoT Hub-anslutningssträngen, den primära IoT Hub-nyckeln, det Händelsehubb-kompatibla namnet och den Händelsehubb-kompatibla slutpunkten som du behöver för att slutföra resten av kursen.</span><span class="sxs-lookup"><span data-stu-id="d1a74-121">You have the IoT Hub host name, IoT Hub connection string, IoT Hub Primary Key, Event Hub-compatible name, and Event Hub-compatible endpoint you need to complete this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="d1a74-122">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="d1a74-122">Create a device identity</span></span>
<span data-ttu-id="d1a74-123">I det här avsnittet ska du skapa en Java-konsolapp som skapar en enhetsidentitet i identitetsregistret i IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-123">In this section, you create a Java console app that creates a device identity in the identity registry in your IoT hub.</span></span> <span data-ttu-id="d1a74-124">En enhet kan inte ansluta till IoT Hub om den inte har en post i identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="d1a74-124">A device cannot connect to IoT hub unless it has an entry in the identity registry.</span></span> <span data-ttu-id="d1a74-125">Mer information finns i avsnittet om **identitetsregistret** i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="d1a74-125">For more information, see the **Identity Registry** section of the [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="d1a74-126">När du kör den här konsolappen genererar det ett unikt enhets-ID och en nyckel som din enhet kan använda för att identifiera sig själv när den skickar ”enhet-till-molnet”-meddelanden till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-126">When you run this console app, it generates a unique device ID and key that your device can use to identify itself when it sends device-to-cloud messages to IoT Hub.</span></span>

1. <span data-ttu-id="d1a74-127">Skapa en tom mapp med namnet iot-java-get-started.</span><span class="sxs-lookup"><span data-stu-id="d1a74-127">Create an empty folder called iot-java-get-started.</span></span> <span data-ttu-id="d1a74-128">Skapa ett Maven-projekt i mappen iot-java-get-started med namnet **create-device-identity** med hjälp av följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d1a74-128">In the iot-java-get-started folder, create a Maven project called **create-device-identity** using the following command at your command prompt.</span></span> <span data-ttu-id="d1a74-129">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="d1a74-129">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="d1a74-130">Gå till mappen create-device-identity i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d1a74-130">At your command prompt, navigate to the create-device-identity folder.</span></span>

3. <span data-ttu-id="d1a74-131">Använd en textredigerare och öppna filen pom.xml i mappen create-device-identity och lägg till följande beroende till noden **dependencies**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-131">Using a text editor, open the pom.xml file in the create-device-identity folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="d1a74-132">Det här beroendet gör att du kan använda paketet iot-service-client i din app:</span><span class="sxs-lookup"><span data-stu-id="d1a74-132">This dependency enables you to use the iot-service-client package in your app:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="d1a74-133">Du kan söka efter den senaste versionen av **iot-service-client** med [Maven-sökning][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="d1a74-133">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="d1a74-134">Spara och stäng filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="d1a74-134">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="d1a74-135">Använd en textredigerare och öppna filen create-device-identity\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="d1a74-135">Using a text editor, open the create-device-identity\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="d1a74-136">Lägg till följande **Import**-instruktioner i filen:</span><span class="sxs-lookup"><span data-stu-id="d1a74-136">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="d1a74-137">Lägg till följande variabler på klassnivå till klassen **App** och ersätt **{yourhubconnectionstring}** med värdena som du noterade och skrev ned tidigare:</span><span class="sxs-lookup"><span data-stu-id="d1a74-137">Add the following class-level variables to the **App** class, replacing **{yourhubconnectionstring}** with the value your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. <span data-ttu-id="d1a74-138">Ändra signaturen för **main**-metoden och ta med undantagen som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="d1a74-138">Modify the signature of the **main** method to include the exceptions as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. <span data-ttu-id="d1a74-139">Lägg till följande kod som huvudstycket i **main**-metoden.</span><span class="sxs-lookup"><span data-stu-id="d1a74-139">Add the following code as the body of the **main** method.</span></span> <span data-ttu-id="d1a74-140">Den här koden skapar en enhet med namnet *javadevice* i ditt IoT Hub-identitetsregister om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="d1a74-140">This code creates a device called *javadevice* in your IoT Hub identity registry if doesn't already exist.</span></span> <span data-ttu-id="d1a74-141">Den visar sedan enhets-ID:t och enhetsnyckeln som du behöver senare:</span><span class="sxs-lookup"><span data-stu-id="d1a74-141">It then displays the device ID and key that you need later:</span></span>

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
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

10. <span data-ttu-id="d1a74-142">Spara och stäng filen App.java.</span><span class="sxs-lookup"><span data-stu-id="d1a74-142">Save and close the App.java file.</span></span>

11. <span data-ttu-id="d1a74-143">Skapa appen **create-device-identity** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen create-device-identity:</span><span class="sxs-lookup"><span data-stu-id="d1a74-143">To build the **create-device-identity** app using Maven, execute the following command at the command prompt in the create-device-identity folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. <span data-ttu-id="d1a74-144">Kör appen **create-device-identity** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen create-device-identity:</span><span class="sxs-lookup"><span data-stu-id="d1a74-144">To run the **create-device-identity** app using Maven, execute the following command at the command prompt in the create-device-identity folder:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. <span data-ttu-id="d1a74-145">Skriv ner **Enhets-ID** och **Enhetsnyckel**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-145">Make a note of the **Device ID** and **Device key**.</span></span> <span data-ttu-id="d1a74-146">Du behöver dessa värden senare när du skapar en app som ansluter till IoT Hub som en enhet.</span><span class="sxs-lookup"><span data-stu-id="d1a74-146">You need these values later when you create an app that connects to IoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="d1a74-147">IoT Hub-identitetsregistret lagrar bara enhetsidentiteter för att skydda åtkomsten till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-147">The IoT Hub identity registry only stores device identities to enable secure access to the IoT hub.</span></span> <span data-ttu-id="d1a74-148">Registret lagrar enhets-ID:n och enhetsnycklar som ska användas som säkerhetsreferenser och en aktiverad/inaktiverad-flagga som du kan använda för att inaktivera åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="d1a74-148">It stores device IDs and keys to use as security credentials and an enabled/disabled flag that you can use to disable access for an individual device.</span></span> <span data-ttu-id="d1a74-149">Om din app behöver lagra andra enhetsspecifika metadata bör den använda en appspecifik butik.</span><span class="sxs-lookup"><span data-stu-id="d1a74-149">If your app needs to store other device-specific metadata, it should use an app-specific store.</span></span> <span data-ttu-id="d1a74-150">Mer information finns i [utvecklarhandboken för IoT Hub][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="d1a74-150">For more information, see the [IoT Hub developer guide][lnk-devguide-identity].</span></span>

## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="d1a74-151">Ta emot meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="d1a74-151">Receive device-to-cloud messages</span></span>

<span data-ttu-id="d1a74-152">I det här avsnittet ska du skapa en Java-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-152">In this section, you create a Java console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="d1a74-153">En IoT Hub exponerar en [Event Hubs][lnk-event-hubs-overview]-kompatibel slutpunkt så att du kan läsa meddelanden från enheter till molnet.</span><span class="sxs-lookup"><span data-stu-id="d1a74-153">An IoT hub exposes an [Event Hub][lnk-event-hubs-overview]-compatible endpoint to enable you to read device-to-cloud messages.</span></span> <span data-ttu-id="d1a74-154">För att göra det så enkelt som möjligt skapar vi en grundläggande läsare i den härs självstudiekursen som inte passar för distributioner med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="d1a74-154">To keep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="d1a74-155">Självstudiekursen [Bearbeta meddelanden från enhet till moln][lnk-process-d2c-tutorial] beskriver hur du bearbetar ”enhet till molnet”-meddelanden i hög skala.</span><span class="sxs-lookup"><span data-stu-id="d1a74-155">The [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how to process device-to-cloud messages at scale.</span></span> <span data-ttu-id="d1a74-156">Självstudiekursen [Komma igång med Event Hubs][lnk-eventhubs-tutorial] innehåller ytterligare information om hur du bearbetar meddelanden från Event Hubs och gäller för de Event Hubs-kompatibla slutpunkterna i IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-156">The [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how to process messages from Event Hubs and is applicable to the IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="d1a74-157">Event Hub-kompatibla slutpunkter för läsning av meddelanden från enheter till molnet använder alltid AMQP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="d1a74-157">The Event Hub-compatible endpoint for reading device-to-cloud messages always uses the AMQP protocol.</span></span>

1. <span data-ttu-id="d1a74-158">I mappen iot-java-get-started som du skapade i avsnittet *Skapa en enhetsidentitet* skapar du ett Maven-projekt med namnet **read-d2c-messages** genom att köra följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d1a74-158">In the iot-java-get-started folder you created in the *Create a device identity* section, create a Maven project called **read-d2c-messages** using the following command at your command prompt.</span></span> <span data-ttu-id="d1a74-159">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="d1a74-159">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="d1a74-160">Gå till mappen read-d2c-messages i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d1a74-160">At your command prompt, navigate to the read-d2c-messages folder.</span></span>

3. <span data-ttu-id="d1a74-161">Använd en textredigerare och öppna filen pom.xml i mappen read-d2c-messages och lägg till följande beroende till noden **dependencies**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-161">Using a text editor, open the pom.xml file in the read-d2c-messages folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="d1a74-162">Det här beroendet gör att du kan använda eventhubs-client-paketet i din app för att läsa från den Event Hubs-kompatibla slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="d1a74-162">This dependency enables you to use the eventhubs-client package in your app to read from the Event Hub-compatible endpoint:</span></span>

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. <span data-ttu-id="d1a74-163">Spara och stäng filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="d1a74-163">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="d1a74-164">Öppna filen read-d2c-messages\src\main\java\com\mycompany\app\App.java i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="d1a74-164">Using a text editor, open the read-d2c-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="d1a74-165">Lägg till följande **Import**-instruktioner i filen:</span><span class="sxs-lookup"><span data-stu-id="d1a74-165">Add the following **import** statements to the file:</span></span>

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. <span data-ttu-id="d1a74-166">Lägg till följande klassnivåvariabler i klassen **App**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-166">Add the following class-level variable to the **App** class.</span></span> <span data-ttu-id="d1a74-167">Ersätt **{youriothubkey}**, **{youreventhubcompatibleendpoint}** och **{youreventhubcompatiblename}** med de värden som du skrev ner tidigare:</span><span class="sxs-lookup"><span data-stu-id="d1a74-167">Replace **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, and **{youreventhubcompatiblename}** with the values you noted previously:</span></span>

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. <span data-ttu-id="d1a74-168">Lägg till följande **receiveMessages**-metod till klassen **App**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-168">Add the following **receiveMessages** method to the **App** class.</span></span> <span data-ttu-id="d1a74-169">Den här metoden skapar en **EventHubClient**-instans för att ansluta till den Event Hubs-kompatibla slutpunkten och skapar sedan en **PartitionReceiver**-instans asynkront för att läsa från Event Hubs-partitionen.</span><span class="sxs-lookup"><span data-stu-id="d1a74-169">This method creates an **EventHubClient** instance to connect to the Event Hub-compatible endpoint and then asynchronously creates a **PartitionReceiver** instance to read from an Event Hub partition.</span></span> <span data-ttu-id="d1a74-170">Den loopar kontinuerligt och skriver ut meddelandeinformationen tills appen avslutas.</span><span class="sxs-lookup"><span data-stu-id="d1a74-170">It loops continuously and prints the message details until the app terminates.</span></span>

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
   > <span data-ttu-id="d1a74-171">Den här metoden använder ett filter när den skapar mottagaren så att mottagaren endast läser meddelanden som skickas till IoT Hub efter att mottagaren har börjat köra.</span><span class="sxs-lookup"><span data-stu-id="d1a74-171">This method uses a filter when it creates the receiver so that the receiver only reads messages sent to IoT Hub after the receiver starts running.</span></span> <span data-ttu-id="d1a74-172">Den här tekniken är användbar i en testmiljö så att du kan se den aktuella uppsättningen meddelanden.</span><span class="sxs-lookup"><span data-stu-id="d1a74-172">This technique is useful in a test environment so you can see the current set of messages.</span></span> <span data-ttu-id="d1a74-173">I en produktionsmiljö bör koden se till att alla meddelanden bearbetas. Mer information finns i självstudiekursen [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] (Bearbeta meddelanden från enheten till molnet i IoT Hub).</span><span class="sxs-lookup"><span data-stu-id="d1a74-173">In a production environment, your code should make sure that it processes all the messages - for more information, see the [How to process IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

9. <span data-ttu-id="d1a74-174">Ändra signaturen för **main**-metoden och ta med undantaget som visas nedan:</span><span class="sxs-lookup"><span data-stu-id="d1a74-174">Modify the signature of the **main** method to include the exception as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. <span data-ttu-id="d1a74-175">Lägg till följande kod i metoden **main** i klassen **App**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-175">Add the following code to the **main** method in the **App** class.</span></span> <span data-ttu-id="d1a74-176">Den här koden skapar två **EventHubClient**- och **PartitionReceiver**-instanser och gör att du kan stänga appen när du har bearbetat meddelandena:</span><span class="sxs-lookup"><span data-stu-id="d1a74-176">This code creates the two **EventHubClient** and **PartitionReceiver** instances and enables you to close the app when you have finished processing messages:</span></span>

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
    } catch (ServiceBusException sbe) {
      System.exit(1);
    }
    ```

    > [!NOTE]
    > <span data-ttu-id="d1a74-177">Den här koden förutsätter att du skapade IoT Hub på F1-nivån (kostnadsfri).</span><span class="sxs-lookup"><span data-stu-id="d1a74-177">This code assumes you created your IoT hub in the F1 (free) tier.</span></span> <span data-ttu-id="d1a74-178">En kostnadsfri IoT Hub har två partitioner med namnen ”0” och ”1”.</span><span class="sxs-lookup"><span data-stu-id="d1a74-178">A free IoT hub has two partitions named "0" and "1".</span></span>

11. <span data-ttu-id="d1a74-179">Spara och stäng filen App.java.</span><span class="sxs-lookup"><span data-stu-id="d1a74-179">Save and close the App.java file.</span></span>

12. <span data-ttu-id="d1a74-180">Skapa appen **read-d2c-messages** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen read-d2c-messages:</span><span class="sxs-lookup"><span data-stu-id="d1a74-180">To build the **read-d2c-messages** app using Maven, execute the following command at the command prompt in the read-d2c-messages folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a><span data-ttu-id="d1a74-181">Skapa en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="d1a74-181">Create a device app</span></span>
<span data-ttu-id="d1a74-182">I det här avsnittet ska du skapa en Java-konsolapp som simulerar en enhet som skickar ”enhet till molnet”-meddelanden till en IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-182">In this section, you create a Java console app that simulates a device that sends device-to-cloud messages to an IoT hub.</span></span>

1. <span data-ttu-id="d1a74-183">I mappen iot-java-get-started som du skapade i avsnittet *Skapa en enhetsidentitet* skapar du ett Maven-projekt med namnet **simulated-device** genom att köra följande kommando i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d1a74-183">In the iot-java-get-started folder you created in the *Create a device identity* section, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="d1a74-184">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="d1a74-184">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="d1a74-185">Gå till mappen simulated-device i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="d1a74-185">At your command prompt, navigate to the simulated-device folder.</span></span>

3. <span data-ttu-id="d1a74-186">Använd en textredigerare och öppna filen pom.xml i mappen simulated-device och lägg till följande beroenden till noden **dependencies**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-186">Using a text editor, open the pom.xml file in the simulated-device folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="d1a74-187">Det här beroendet gör att du kan använda iothub-java-client-paketet i din app för att kommunicera med IoT Hub och för att serialisera Java-objekt till JSON:</span><span class="sxs-lookup"><span data-stu-id="d1a74-187">This dependency enables you to use the iothub-java-client package in your app to communicate with your IoT hub and to serialize Java objects to JSON:</span></span>

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
    > <span data-ttu-id="d1a74-188">Du kan söka efter den senaste versionen av **iot-device-client** med [Maven-sökning][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="d1a74-188">You can check for the latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

4. <span data-ttu-id="d1a74-189">Spara och stäng filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="d1a74-189">Save and close the pom.xml file.</span></span>

5. <span data-ttu-id="d1a74-190">Öppna filen simulated-device\src\main\java\com\mycompany\app\App.java i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="d1a74-190">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="d1a74-191">Lägg till följande **Import**-instruktioner i filen:</span><span class="sxs-lookup"><span data-stu-id="d1a74-191">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. <span data-ttu-id="d1a74-192">Lägg till följande variabler på klassnivå till klassen **App**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-192">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="d1a74-193">Ersätt **{youriothubname}** med namnet på din IoT Hub och **{yourdevicekey}** med enhetsnyckelvärdet som du genererade i avsnittet *Skapa en enhetsidentitet*:</span><span class="sxs-lookup"><span data-stu-id="d1a74-193">Replacing **{youriothubname}** with your IoT hub name, and **{yourdevicekey}** with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    <span data-ttu-id="d1a74-194">Den här exempelappen använder variabeln **protocol** när den instantierar ett **DeviceClient**-objekt.</span><span class="sxs-lookup"><span data-stu-id="d1a74-194">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="d1a74-195">Du kan använda antingen MQTT-, AMQP- eller HTTP-protokollet för att kommunicera med IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-195">You can use either the MQTT, AMQP, or HTTP protocol to communicate with IoT Hub.</span></span>

8. <span data-ttu-id="d1a74-196">Lägg till följande kapslade **TelemetryDataPoint**-klass inuti klassen **App** för att ange de telemetridata som enheten skickar till IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d1a74-196">Add the following nested **TelemetryDataPoint** class inside the **App** class to specify the telemetry data your device sends to your IoT hub:</span></span>

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
9. <span data-ttu-id="d1a74-197">Lägg till följande kapslade **EventCallback**-klass inuti klassen **App** för att visa bekräftelsestatusen som IoT Hub returnerar när den bearbetar ett meddelande från enhetsappen.</span><span class="sxs-lookup"><span data-stu-id="d1a74-197">Add the following nested **EventCallback** class inside the **App** class to display the acknowledgement status that the IoT hub returns when it processes a message from the device app.</span></span> <span data-ttu-id="d1a74-198">Den här metoden meddelar även huvudtråden i appen när meddelandet har bearbetats:</span><span class="sxs-lookup"><span data-stu-id="d1a74-198">This method also notifies the main thread in the app when the message has been processed:</span></span>
   
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

10. <span data-ttu-id="d1a74-199">Lägg till följande kapslade **MessageSender**-klass inuti klassen **App**.</span><span class="sxs-lookup"><span data-stu-id="d1a74-199">Add the following nested **MessageSender** class inside the **App** class.</span></span> <span data-ttu-id="d1a74-200">Metoden **Kör** i den här klassen genererar exempeltelemetridata som skickas till IoT Hub och väntar på en bekräftelse innan nästa meddelande skickas:</span><span class="sxs-lookup"><span data-stu-id="d1a74-200">The **run** method in this class generates sample telemetry data to send to your IoT hub and waits for an acknowledgement before sending the next message:</span></span>

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

    <span data-ttu-id="d1a74-201">Den här metoden skickar ett nytt ”enhet till molnet”-meddelande en sekund efter att IoT Hub bekräftar det föregående meddelandet.</span><span class="sxs-lookup"><span data-stu-id="d1a74-201">This method sends a new device-to-cloud message one second after the IoT hub acknowledges the previous message.</span></span> <span data-ttu-id="d1a74-202">Meddelandet innehåller ett JSON-serialiserat objekt med enhets-ID:t och ett slumpmässigt genererat nummer för att simulera en temperatursensor och en fuktighetssensor.</span><span class="sxs-lookup"><span data-stu-id="d1a74-202">The message contains a JSON-serialized object with the deviceId and randomly generated numbers to simulate a temperature sensor, and a humidity sensor.</span></span>

11. <span data-ttu-id="d1a74-203">Ersätt metoden **main** med följande kod som skapar en tråd för att skicka ”enhet till molnet”-meddelanden till din IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d1a74-203">Replace the **main** method with the following code that creates a thread to send device-to-cloud messages to your IoT hub:</span></span>

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

12. <span data-ttu-id="d1a74-204">Spara och stäng filen App.java.</span><span class="sxs-lookup"><span data-stu-id="d1a74-204">Save and close the App.java file.</span></span>

13. <span data-ttu-id="d1a74-205">Skapa appen **simulated-device** med hjälp av Maven genom att köra följande kommando i Kommandotolken i mappen simulated-device:</span><span class="sxs-lookup"><span data-stu-id="d1a74-205">To build the **simulated-device** app using Maven, execute the following command at the command prompt in the simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> <span data-ttu-id="d1a74-206">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="d1a74-206">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="d1a74-207">I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="d1a74-207">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="d1a74-208">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="d1a74-208">Run the apps</span></span>

<span data-ttu-id="d1a74-209">Nu är det dags att köra apparna.</span><span class="sxs-lookup"><span data-stu-id="d1a74-209">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="d1a74-210">Kör följande kommando i Kommandotolken i mappen read-d2c för att börja övervaka den första partitionen i din IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d1a74-210">At a command prompt in the read-d2c folder, run the following command to begin monitoring the first partition in your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Java IoT Hub-tjänstapp för att övervaka meddelanden från enheten till molnet][7]

2. <span data-ttu-id="d1a74-212">Kör följande kommando i Kommandotolken i mappen simulated-device för att börja skicka telemetridata till din IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d1a74-212">At a command prompt in the simulated-device folder, run the following command to begin sending telemetry data to your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Java IoT Hub-enhetsapp för att skicka meddelanden från enheten till molnet][8]

3. <span data-ttu-id="d1a74-214">På panelen **Användning** på [Azure Portal][lnk-portal] kan du se hur många meddelanden som har skickats till IoT Hub:</span><span class="sxs-lookup"><span data-stu-id="d1a74-214">The **Usage** tile in the [Azure portal][lnk-portal] shows the number of messages sent to the IoT hub:</span></span>

    ![Azure-portal Användningspanel som visar antalet meddelanden som har skickats till IoT Hub][43]

## <a name="next-steps"></a><span data-ttu-id="d1a74-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="d1a74-216">Next steps</span></span>
<span data-ttu-id="d1a74-217">I den här självstudiekursen konfigurerade du en ny IoT Hub på Azure Portal och skapade sedan en enhetsidentitet i IoT-hubbens identitetsregister.</span><span class="sxs-lookup"><span data-stu-id="d1a74-217">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="d1a74-218">Du använde den här enhetsidentiteten så att enhetsappen kunde skicka ”enhet till molnet”-meddelanden till IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-218">You used this device identity to enable the device app to send device-to-cloud messages to the IoT hub.</span></span> <span data-ttu-id="d1a74-219">Du skapade också en app som visar meddelandena som tagits emot av IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="d1a74-219">You also created an app that displays the messages received by the IoT hub.</span></span>

<span data-ttu-id="d1a74-220">Mer information om hur du kan komma igång med IoT Hub och utforska andra IoT-scenarier finns här:</span><span class="sxs-lookup"><span data-stu-id="d1a74-220">To continue getting started with IoT Hub and to explore other IoT scenarios, see:</span></span>

* <span data-ttu-id="d1a74-221">[Connecting your device][lnk-connect-device] (Ansluta din enhet)</span><span class="sxs-lookup"><span data-stu-id="d1a74-221">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="d1a74-222">[Connecting your device][lnk-device-management] (Komma igång med enhetshantering)</span><span class="sxs-lookup"><span data-stu-id="d1a74-222">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="d1a74-223">[Komma igång med Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="d1a74-223">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="d1a74-224">Självstudiekursen [Bearbeta meddelanden från enhet till moln][lnk-process-d2c-tutorial] beskriver hur du utökar din IoT-lösning och bearbetar ”enhet till molnet”-meddelanden i hög skala.</span><span class="sxs-lookup"><span data-stu-id="d1a74-224">To learn how to extend your IoT solution and process device-to-cloud messages at scale, see the [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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
