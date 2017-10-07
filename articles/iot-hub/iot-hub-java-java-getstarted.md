---
title: "aaaGet igång med Azure IoT Hub (Java) | Microsoft Docs"
description: "Lär dig hur toosend enhet till moln meddelanden tooAzure IoT-hubb med IoT-SDK för Java. Skapa simulerade enheten och tjänsten appar tooregister enheten, skicka meddelanden och läsa meddelanden från IoT-hubb."
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
ms.openlocfilehash: ac954f0522b46ed2a5b4a819bc611c13be0b9a9e
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="connect-your-device-tooyour-iot-hub-using-java"></a><span data-ttu-id="1a4f0-104">Ansluta din enhet tooyour IoT-hubb som använder Java</span><span class="sxs-lookup"><span data-stu-id="1a4f0-104">Connect your device tooyour IoT hub using Java</span></span>
[!INCLUDE [iot-hub-selector-get-started](../../includes/iot-hub-selector-get-started.md)]

<span data-ttu-id="1a4f0-105">Hello slutet av den här självstudiekursen har du tre Java konsolappar:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-105">At hello end of this tutorial, you have three Java console apps:</span></span>

* <span data-ttu-id="1a4f0-106">**Skapa enhetsidentitet**, vilket skapar en enhetsidentitet och tillhörande nyckeln tooconnect din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-106">**create-device-identity**, which creates a device identity and associated security key tooconnect your device app.</span></span>
* <span data-ttu-id="1a4f0-107">**Läs-d2c-meddelanden**, som visar hello telemetri som skickats av din enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-107">**read-d2c-messages**, which displays hello telemetry sent by your device app.</span></span>
* <span data-ttu-id="1a4f0-108">**simulerade enheten**, som ansluter tooyour IoT-hubb med hello enhetsidentitet skapade tidigare och skickar meddelandet telemetri som alla andra med hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-108">**simulated-device**, which connects tooyour IoT hub with hello device identity created earlier, and sends a telemetry message every second using hello MQTT protocol.</span></span>

> [!NOTE]
> <span data-ttu-id="1a4f0-109">hello artikel [Azure IoT SDK] [ lnk-hub-sdks] innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild båda toorun för appar på enheter och din lösningens serverdel.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-109">hello article [Azure IoT SDKs][lnk-hub-sdks] provides information about hello Azure IoT SDKs that you can use toobuild both apps toorun on devices and your solution back end.</span></span>

<span data-ttu-id="1a4f0-110">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-110">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="1a4f0-111">Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="1a4f0-111">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span> 
* [<span data-ttu-id="1a4f0-112">Maven 3</span><span class="sxs-lookup"><span data-stu-id="1a4f0-112">Maven 3</span></span>](https://maven.apache.org/install.html) 
* <span data-ttu-id="1a4f0-113">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-113">An active Azure account.</span></span> <span data-ttu-id="1a4f0-114">(Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="1a4f0-114">(If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

<span data-ttu-id="1a4f0-115">Som ett sista steg anteckna hello **primärnyckel** värde.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-115">As a final step, make a note of hello **Primary key** value.</span></span> <span data-ttu-id="1a4f0-116">Klicka på **slutpunkter** och hello **händelser** inbyggd slutpunkt.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-116">Then click **Endpoints** and hello **Events** built-in endpoint.</span></span> <span data-ttu-id="1a4f0-117">På hello **egenskaper** bladet antecknar hello **Event Hub-kompatibelt namn** och hello **Event Hub-kompatibel endpoint** adress.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-117">On hello **Properties** blade, make a note of hello **Event Hub-compatible name** and hello **Event Hub-compatible endpoint** address.</span></span> <span data-ttu-id="1a4f0-118">Du behöver dessa tre värden när du skapar appen **read-d2c-messages**.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-118">You need these three values when you create your **read-d2c-messages** app.</span></span>

![Aviseringsblad, Azure-portal, IoT Hub][6]

<span data-ttu-id="1a4f0-120">Nu har du skapat din IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-120">You have now created your IoT hub.</span></span> <span data-ttu-id="1a4f0-121">Du har hello IoT Hub-värdnamnet, IoT-hubb anslutningssträngen, IoT-hubb primärnyckel, Event Hub-kompatibelt namn och Event Hub-kompatibel endpoint toocomplete måste den här kursen.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-121">You have hello IoT Hub host name, IoT Hub connection string, IoT Hub Primary Key, Event Hub-compatible name, and Event Hub-compatible endpoint you need toocomplete this tutorial.</span></span>

## <a name="create-a-device-identity"></a><span data-ttu-id="1a4f0-122">Skapa en enhetsidentitet</span><span class="sxs-lookup"><span data-stu-id="1a4f0-122">Create a device identity</span></span>
<span data-ttu-id="1a4f0-123">I det här avsnittet skapar du en Java-konsolapp som skapar en enhetsidentitet i hello identitetsregistret i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-123">In this section, you create a Java console app that creates a device identity in hello identity registry in your IoT hub.</span></span> <span data-ttu-id="1a4f0-124">En enhet kan inte ansluta tooIoT hubb om den inte har en post i registret för hello identitet.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-124">A device cannot connect tooIoT hub unless it has an entry in hello identity registry.</span></span> <span data-ttu-id="1a4f0-125">Mer information finns i hello **Identitetsregistret** avsnitt i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="1a4f0-125">For more information, see hello **Identity Registry** section of hello [IoT Hub developer guide][lnk-devguide-identity].</span></span> <span data-ttu-id="1a4f0-126">När du kör den här konsolen appen genereras ett unikt enhets-ID och nyckel att enheten kan använda tooidentify själva när den skickar enhet till moln meddelanden tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-126">When you run this console app, it generates a unique device ID and key that your device can use tooidentify itself when it sends device-to-cloud messages tooIoT Hub.</span></span>

1. <span data-ttu-id="1a4f0-127">Skapa en tom mapp med namnet iot-java-get-started.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-127">Create an empty folder called iot-java-get-started.</span></span> <span data-ttu-id="1a4f0-128">Skapa ett Maven-projekt som kallas i hello iot-java-get-started mappen **skapa enhetsidentitet** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-128">In hello iot-java-get-started folder, create a Maven project called **create-device-identity** using hello following command at your command prompt.</span></span> <span data-ttu-id="1a4f0-129">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-129">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=create-device-identity -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="1a4f0-130">Navigera toohello skapa enhetsidentitet mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-130">At your command prompt, navigate toohello create-device-identity folder.</span></span>

3. <span data-ttu-id="1a4f0-131">Med en textredigerare, öppna hello pom.xml filen hello skapa enhetsidentitet mapp och lägga till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-131">Using a text editor, open hello pom.xml file in hello create-device-identity folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="1a4f0-132">Det här beroendet kan du toouse hello iot-service-klient paketet i din app:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-132">This dependency enables you toouse hello iot-service-client package in your app:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="1a4f0-133">Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="1a4f0-133">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

4. <span data-ttu-id="1a4f0-134">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-134">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="1a4f0-135">Använd en textredigerare och öppna hello create-device-identity\src\main\java\com\mycompany\app\App.java filen.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-135">Using a text editor, open hello create-device-identity\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="1a4f0-136">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-136">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.Device;
    import com.microsoft.azure.sdk.iot.service.RegistryManager;
   
    import java.io.IOException;
    import java.net.URISyntaxException;
    ```

7. <span data-ttu-id="1a4f0-137">Lägg till följande variabler klassen på toohello hello **App** class, ersätter **{yourhubconnectionstring}** med hello värde din anges tidigare:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-137">Add hello following class-level variables toohello **App** class, replacing **{yourhubconnectionstring}** with hello value your noted earlier:</span></span>

    ```java
    private static final String connectionString = "{yourhubconnectionstring}";
    private static final String deviceId = "myFirstJavaDevice";
    ```
[!INCLUDE [iot-hub-pii-note-naming-device](../../includes/iot-hub-pii-note-naming-device.md)]

8. <span data-ttu-id="1a4f0-138">Ändra hello signaturen för hello **huvudsakliga** metoden tooinclude hello undantag på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-138">Modify hello signature of hello **main** method tooinclude hello exceptions as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException, Exception
    ```

9. <span data-ttu-id="1a4f0-139">Lägg till hello efter koden som hello brödtext hello **huvudsakliga** metod.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-139">Add hello following code as hello body of hello **main** method.</span></span> <span data-ttu-id="1a4f0-140">Den här koden skapar en enhet med namnet *javadevice* i ditt IoT Hub-identitetsregister om den inte redan finns.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-140">This code creates a device called *javadevice* in your IoT Hub identity registry if doesn't already exist.</span></span> <span data-ttu-id="1a4f0-141">Den visar hello enhets-ID och nyckel som du behöver senare:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-141">It then displays hello device ID and key that you need later:</span></span>

    ```java
    RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);
RegistryManager registryManager = RegistryManager.createFromConnectionString(connectionString);

    // Create a device that's enabled by default, 
    // with an autogenerated key.
    Device device = Device.createFromId(deviceId, null, null);
    try {
      device = registryManager.addDevice(device);
    } catch (IotHubException iote) {
      // If hello device already exists.
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
      // If hello device already exists.
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

10. <span data-ttu-id="1a4f0-142">Spara och Stäng hello App.java fil.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-142">Save and close hello App.java file.</span></span>

11. <span data-ttu-id="1a4f0-143">toobuild hello **skapa enhetsidentitet** app med Maven, kör följande kommando i Kommandotolken hello i hello skapa enhetsidentitet mapp hello:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-143">toobuild hello **create-device-identity** app using Maven, execute hello following command at hello command prompt in hello create-device-identity folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

12. <span data-ttu-id="1a4f0-144">toorun hello **skapa enhetsidentitet** app med Maven, kör följande kommando i Kommandotolken hello i hello skapa enhetsidentitet mapp hello:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-144">toorun hello **create-device-identity** app using Maven, execute hello following command at hello command prompt in hello create-device-identity folder:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

13. <span data-ttu-id="1a4f0-145">Anteckna hello **enhets-ID** och **enhetsnyckel**.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-145">Make a note of hello **Device ID** and **Device key**.</span></span> <span data-ttu-id="1a4f0-146">Du måste dessa värden senare när du skapar en app som ansluter tooIoT hubb som en enhet.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-146">You need these values later when you create an app that connects tooIoT Hub as a device.</span></span>

> [!NOTE]
> <span data-ttu-id="1a4f0-147">Hej IoT-hubb identitetsregistret lagrar bara enheten identiteter tooenable säker åtkomst toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-147">hello IoT Hub identity registry only stores device identities tooenable secure access toohello IoT hub.</span></span> <span data-ttu-id="1a4f0-148">Enheten ID och nycklar toouse lagras som säkerhetsreferenser och en aktiverat/inaktiverat flagga som du kan använda toodisable åtkomst för en enskild enhet.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-148">It stores device IDs and keys toouse as security credentials and an enabled/disabled flag that you can use toodisable access for an individual device.</span></span> <span data-ttu-id="1a4f0-149">Om din app måste toostore andra enhetsspecifika metadata, ska det använda en app-specifik butik.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-149">If your app needs toostore other device-specific metadata, it should use an app-specific store.</span></span> <span data-ttu-id="1a4f0-150">Mer information finns i hello [IoT-hubb Utvecklarhandbok][lnk-devguide-identity].</span><span class="sxs-lookup"><span data-stu-id="1a4f0-150">For more information, see hello [IoT Hub developer guide][lnk-devguide-identity].</span></span>

## <a name="receive-device-to-cloud-messages"></a><span data-ttu-id="1a4f0-151">Ta emot meddelanden från enheten till molnet</span><span class="sxs-lookup"><span data-stu-id="1a4f0-151">Receive device-to-cloud messages</span></span>

<span data-ttu-id="1a4f0-152">I det här avsnittet ska du skapa en Java-konsolapp som läser ”enhet till molnet”-meddelanden från IoT Hub.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-152">In this section, you create a Java console app that reads device-to-cloud messages from IoT Hub.</span></span> <span data-ttu-id="1a4f0-153">En IoT-hubb Exponerar en [Händelsehubb][lnk-event-hubs-overview]-kompatibel endpoint tooenable du tooread meddelanden från enhet till moln.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-153">An IoT hub exposes an [Event Hub][lnk-event-hubs-overview]-compatible endpoint tooenable you tooread device-to-cloud messages.</span></span> <span data-ttu-id="1a4f0-154">enkel tookeep saker, den här guiden skapar en grundläggande läsare som inte lämpar sig för en distribution med hög genomströmning.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-154">tookeep things simple, this tutorial creates a basic reader that is not suitable for a high throughput deployment.</span></span> <span data-ttu-id="1a4f0-155">Hej [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen visar hur tooprocess enhet till moln meddelanden i större skala.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-155">hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial shows you how tooprocess device-to-cloud messages at scale.</span></span> <span data-ttu-id="1a4f0-156">Hej [Kom igång med Händelsehubbar] [ lnk-eventhubs-tutorial] självstudier innehåller ytterligare information om hur tooprocess meddelanden från Event Hubs och är tillämplig toohello IoT-hubb Event Hub-kompatibel slutpunkter.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-156">hello [Get Started with Event Hubs][lnk-eventhubs-tutorial] tutorial provides further information on how tooprocess messages from Event Hubs and is applicable toohello IoT Hub Event Hub-compatible endpoints.</span></span>

> [!NOTE]
> <span data-ttu-id="1a4f0-157">hello använder Event Hub-kompatibel slutpunkt för att läsa meddelanden från enhet till moln alltid hello AMQP-protokollet.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-157">hello Event Hub-compatible endpoint for reading device-to-cloud messages always uses hello AMQP protocol.</span></span>

1. <span data-ttu-id="1a4f0-158">I hello iot-java-get-started mappen du skapade i hello *skapa en enhetsidentitet* avsnittet, skapa ett Maven-projekt som kallas **lästa d2c meddelanden** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-158">In hello iot-java-get-started folder you created in hello *Create a device identity* section, create a Maven project called **read-d2c-messages** using hello following command at your command prompt.</span></span> <span data-ttu-id="1a4f0-159">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-159">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=read-d2c-messages -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="1a4f0-160">Navigera toohello lästa d2c meddelanden mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-160">At your command prompt, navigate toohello read-d2c-messages folder.</span></span>

3. <span data-ttu-id="1a4f0-161">Med en textredigerare, öppna hello pom.xml filen hello lästa d2c meddelanden mapp och lägga till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-161">Using a text editor, open hello pom.xml file in hello read-d2c-messages folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="1a4f0-162">Det här beroendet kan du toouse eventhubs hello-klientpaketet i din app tooread från hello Event Hub-kompatibel slutpunkten:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-162">This dependency enables you toouse hello eventhubs-client package in your app tooread from hello Event Hub-compatible endpoint:</span></span>

    ```xml
    <dependency> 
        <groupId>com.microsoft.azure</groupId> 
        <artifactId>azure-eventhubs</artifactId> 
        <version>0.13.0</version> 
    </dependency>
    ```

4. <span data-ttu-id="1a4f0-163">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-163">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="1a4f0-164">Använd en textredigerare och öppna hello read-d2c-messages\src\main\java\com\mycompany\app\App.java filen.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-164">Using a text editor, open hello read-d2c-messages\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="1a4f0-165">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-165">Add hello following **import** statements toohello file:</span></span>

    ```java
    import java.io.IOException;
    import com.microsoft.azure.eventhubs.*;
    import com.microsoft.azure.servicebus.*;

    import java.nio.charset.Charset;
    import java.time.*;
    import java.util.function.*;
    ```

7. <span data-ttu-id="1a4f0-166">Lägg till följande klassnivå variabeln toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-166">Add hello following class-level variable toohello **App** class.</span></span> <span data-ttu-id="1a4f0-167">Ersätt **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, och **{youreventhubcompatiblename}** med hello-värden som du antecknade tidigare:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-167">Replace **{youriothubkey}**, **{youreventhubcompatibleendpoint}**, and **{youreventhubcompatiblename}** with hello values you noted previously:</span></span>

    ```java
    private static String connStr = "Endpoint={youreventhubcompatibleendpoint};EntityPath={youreventhubcompatiblename};SharedAccessKeyName=iothubowner;SharedAccessKey={youriothubkey}";
    ```

8. <span data-ttu-id="1a4f0-168">Lägg till följande hello **receiveMessages** metoden toohello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-168">Add hello following **receiveMessages** method toohello **App** class.</span></span> <span data-ttu-id="1a4f0-169">Den här metoden skapar en **EventHubClient** tooconnect toohello Event Hub-kompatibel endpoint-instans och asynkront skapar en **PartitionReceiver** instans tooread från en Händelsehubb partition.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-169">This method creates an **EventHubClient** instance tooconnect toohello Event Hub-compatible endpoint and then asynchronously creates a **PartitionReceiver** instance tooread from an Event Hub partition.</span></span> <span data-ttu-id="1a4f0-170">Den slinga kontinuerligt och skriver ut hello meddelandet visas tills hello app slutar.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-170">It loops continuously and prints hello message details until hello app terminates.</span></span>

    ```java
    // Create a receiver on a partition.
    private static EventHubClient receiveMessages(final String partitionId) {
      EventHubClient client = null;
      try {
        client = EventHubClient.createFromConnectionStringSync(connStr);
      } catch (Exception e) {
        System.out.println("Failed toocreate client: " + e.getMessage());
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
                System.out.println("Failed tooreceive messages: " + e.getMessage());
              }
            }
          });
        } catch (Exception e) {
          System.out.println("Failed toocreate receiver: " + e.getMessage());
      }
      return client;
    }
    ```

   > [!NOTE]
   > <span data-ttu-id="1a4f0-171">Den här metoden använder ett filter när den skapar hello mottagare så att hello mottagaren läser endast meddelanden som skickas tooIoT hubb när hello mottagaren börjar köras.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-171">This method uses a filter when it creates hello receiver so that hello receiver only reads messages sent tooIoT Hub after hello receiver starts running.</span></span> <span data-ttu-id="1a4f0-172">Den här metoden är användbar i en testmiljö så att du kan se hello aktuella uppsättning av meddelanden.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-172">This technique is useful in a test environment so you can see hello current set of messages.</span></span> <span data-ttu-id="1a4f0-173">I en produktionsmiljö bör koden se till att den bearbetar alla meddelanden för hello - mer information, se hello [hur tooprocess IoT-hubb meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-173">In a production environment, your code should make sure that it processes all hello messages - for more information, see hello [How tooprocess IoT Hub device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>

9. <span data-ttu-id="1a4f0-174">Ändra hello signaturen för hello **huvudsakliga** metoden tooinclude hello undantag på följande sätt:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-174">Modify hello signature of hello **main** method tooinclude hello exception as follows:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

10. <span data-ttu-id="1a4f0-175">Lägg till följande kod toohello hello **huvudsakliga** metod i hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-175">Add hello following code toohello **main** method in hello **App** class.</span></span> <span data-ttu-id="1a4f0-176">Den här koden skapar hello två **EventHubClient** och **PartitionReceiver** instanser och gör att du tooclose hello app när du är klar behandlar meddelanden:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-176">This code creates hello two **EventHubClient** and **PartitionReceiver** instances and enables you tooclose hello app when you have finished processing messages:</span></span>

    ```java
    // Create receivers for partitions 0 and 1.
    EventHubClient client0 = receiveMessages("0");
    EventHubClient client1 = receiveMessages("1");
    System.out.println("Press ENTER tooexit.");
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
    > <span data-ttu-id="1a4f0-177">Den här koden förutsätter att du skapade din IoT-hubb i (kostnadsfritt) hello F1-nivån.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-177">This code assumes you created your IoT hub in hello F1 (free) tier.</span></span> <span data-ttu-id="1a4f0-178">En kostnadsfri IoT Hub har två partitioner med namnen ”0” och ”1”.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-178">A free IoT hub has two partitions named "0" and "1".</span></span>

11. <span data-ttu-id="1a4f0-179">Spara och Stäng hello App.java fil.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-179">Save and close hello App.java file.</span></span>

12. <span data-ttu-id="1a4f0-180">toobuild hello **lästa d2c meddelanden** app med Maven, kör följande kommando i Kommandotolken hello i hello lästa d2c meddelanden mappen hello:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-180">toobuild hello **read-d2c-messages** app using Maven, execute hello following command at hello command prompt in hello read-d2c-messages folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

## <a name="create-a-device-app"></a><span data-ttu-id="1a4f0-181">Skapa en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="1a4f0-181">Create a device app</span></span>
<span data-ttu-id="1a4f0-182">I det här avsnittet skapar du en Java-konsolapp som simulerar en enhet som skickar meddelanden från enhet till moln tooan IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-182">In this section, you create a Java console app that simulates a device that sends device-to-cloud messages tooan IoT hub.</span></span>

1. <span data-ttu-id="1a4f0-183">Hej iot-java-get-started mappen du skapade i hello *skapa en enhetsidentitet* avsnittet, skapa ett Maven-projekt som kallas **simulerade enheten** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-183">In hello iot-java-get-started folder you created in hello *Create a device identity* section, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="1a4f0-184">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-184">Note this is a single, long command:</span></span>

    ```cmd/sh
    mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false
    ```

2. <span data-ttu-id="1a4f0-185">Navigera toohello simulerade enheten mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-185">At your command prompt, navigate toohello simulated-device folder.</span></span>

3. <span data-ttu-id="1a4f0-186">Med en textredigerare, öppna hello pom.xml filen hello simulerade enheten mapp och lägga till följande beroenden toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-186">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="1a4f0-187">Det här beroendet kan du toouse hello iothub-java-klient paketet i din app toocommunicate med IoT-hubb och tooserialize tooJSON för Java-objekt:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-187">This dependency enables you toouse hello iothub-java-client package in your app toocommunicate with your IoT hub and tooserialize Java objects tooJSON:</span></span>

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
    > <span data-ttu-id="1a4f0-188">Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="1a4f0-188">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

4. <span data-ttu-id="1a4f0-189">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-189">Save and close hello pom.xml file.</span></span>

5. <span data-ttu-id="1a4f0-190">Använd en textredigerare och öppna hello simulated-device\src\main\java\com\mycompany\app\App.java filen.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-190">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

6. <span data-ttu-id="1a4f0-191">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-191">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.google.gson.Gson;

    import java.io.*;
    import java.net.URISyntaxException;
    import java.util.Random;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

7. <span data-ttu-id="1a4f0-192">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-192">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="1a4f0-193">Ersätta **{youriothubname}** med din IoT-hubbnamnet och **{yourdevicekey}** hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-193">Replacing **{youriothubname}** with your IoT hub name, and **{yourdevicekey}** with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myFirstJavaDevice;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myFirstJavaDevice";
    private static DeviceClient client;
    ```
   
    <span data-ttu-id="1a4f0-194">Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-194">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="1a4f0-195">Du kan använda toocommunicate för protokollet av MQTT, AMQP och HTTP-antingen hello med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-195">You can use either hello MQTT, AMQP, or HTTP protocol toocommunicate with IoT Hub.</span></span>

8. <span data-ttu-id="1a4f0-196">Lägg till hello följande kapslade **TelemetryDataPoint** klass i hello **App** klassen toospecify hello telemetridata enheten skickar tooyour IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-196">Add hello following nested **TelemetryDataPoint** class inside hello **App** class toospecify hello telemetry data your device sends tooyour IoT hub:</span></span>

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
9. <span data-ttu-id="1a4f0-197">Lägg till hello följande kapslade **EventCallback** klass i hello **App** klassen toodisplay hello bekräftelse status som hello IoT-hubb returnerar när den bearbetar ett meddelande från hello enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-197">Add hello following nested **EventCallback** class inside hello **App** class toodisplay hello acknowledgement status that hello IoT hub returns when it processes a message from hello device app.</span></span> <span data-ttu-id="1a4f0-198">Den här metoden meddelar även hello huvudtråden i hello app när har bearbetats hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-198">This method also notifies hello main thread in hello app when hello message has been processed:</span></span>
   
    ```java
    private static class EventCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toomessage with status: " + status.name());
   
        if (context != null) {
          synchronized (context) {
            context.notify();
          }
        }
      }
    }
    ```

10. <span data-ttu-id="1a4f0-199">Lägg till hello följande kapslade **MessageSender** klass i hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-199">Add hello following nested **MessageSender** class inside hello **App** class.</span></span> <span data-ttu-id="1a4f0-200">Hej **kör** metod i den här klassen genererar exempel telemetri data toosend tooyour IoT-hubb och väntar på en bekräftelse innan du skickar nästa hello-meddelande:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-200">hello **run** method in this class generates sample telemetry data toosend tooyour IoT hub and waits for an acknowledgement before sending hello next message:</span></span>

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

    <span data-ttu-id="1a4f0-201">Den här metoden skickar meddelandet ny enhet till moln en sekund efter hello IoT-hubb bekräftat tidigare hello-meddelande.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-201">This method sends a new device-to-cloud message one second after hello IoT hub acknowledges hello previous message.</span></span> <span data-ttu-id="1a4f0-202">hello-meddelande innehåller en JSON-serialiserade objekt med hello deviceId och slumpmässigt genererade siffror toosimulate en temperatursensor och en fuktighet sensor.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-202">hello message contains a JSON-serialized object with hello deviceId and randomly generated numbers toosimulate a temperature sensor, and a humidity sensor.</span></span>

11. <span data-ttu-id="1a4f0-203">Ersätt hello **huvudsakliga** metod med följande kod som skapar en tråd toosend meddelanden från enhet till moln tooyour IoT-hubb hello:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-203">Replace hello **main** method with hello following code that creates a thread toosend device-to-cloud messages tooyour IoT hub:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException {
      client = new DeviceClient(connString, protocol);
      client.open();
    
      MessageSender sender = new MessageSender();
    
      ExecutorService executor = Executors.newFixedThreadPool(1);
      executor.execute(sender);
    
      System.out.println("Press ENTER tooexit.");
      System.in.read();
      executor.shutdownNow();
      client.closeNow();
    }
    ```

12. <span data-ttu-id="1a4f0-204">Spara och Stäng hello App.java fil.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-204">Save and close hello App.java file.</span></span>

13. <span data-ttu-id="1a4f0-205">toobuild hello **simulerade enheten** app med Maven, kör följande kommando i Kommandotolken hello i hello simulerade enheten mappen hello:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-205">toobuild hello **simulated-device** app using Maven, execute hello following command at hello command prompt in hello simulated-device folder:</span></span>

    ```cmd/sh
    mvn clean package -DskipTests
    ```

> [!NOTE]
> <span data-ttu-id="1a4f0-206">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-206">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="1a4f0-207">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="1a4f0-207">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="1a4f0-208">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="1a4f0-208">Run hello apps</span></span>

<span data-ttu-id="1a4f0-209">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-209">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="1a4f0-210">Kör hello efter kommandot toobegin övervakning hello första partitionen i din IoT-hubb i en kommandotolk i hello Läs d2c mapp:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-210">At a command prompt in hello read-d2c folder, run hello following command toobegin monitoring hello first partition in your IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App"
    ```

    ![Meddelanden från Java IoT-hubb service app toomonitor enhet till moln][7]

2. <span data-ttu-id="1a4f0-212">Kör hello efter kommandot toobegin skicka telemetri data tooyour IoT-hubb i en kommandotolk i hello simulerade enheten mapp:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-212">At a command prompt in hello simulated-device folder, run hello following command toobegin sending telemetry data tooyour IoT hub:</span></span>

    ```cmd/sh
    mvn exec:java -Dexec.mainClass="com.mycompany.app.App" 
    ```

    ![Meddelanden från Java IoT-hubb enheten app toosend enhet till moln][8]

3. <span data-ttu-id="1a4f0-214">Hej **användning** panelen i hello [Azure-portalen] [ lnk-portal] visar hello antalet meddelanden som skickas toohello IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-214">hello **Usage** tile in hello [Azure portal][lnk-portal] shows hello number of messages sent toohello IoT hub:</span></span>

    ![Azure portal användning panelen visar antalet meddelanden som skickas tooIoT Hub][43]

## <a name="next-steps"></a><span data-ttu-id="1a4f0-216">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1a4f0-216">Next steps</span></span>
<span data-ttu-id="1a4f0-217">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-217">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="1a4f0-218">Du har använt den här enhetens identitet tooenable hello enheten app toosend meddelanden från enhet till moln toohello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-218">You used this device identity tooenable hello device app toosend device-to-cloud messages toohello IoT hub.</span></span> <span data-ttu-id="1a4f0-219">Du kan också skapat en app som visar hello som tagits emot av hello IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-219">You also created an app that displays hello messages received by hello IoT hub.</span></span>

<span data-ttu-id="1a4f0-220">toocontinue komma igång med IoT-hubb och tooexplore finns i andra IoT-scenarier:</span><span class="sxs-lookup"><span data-stu-id="1a4f0-220">toocontinue getting started with IoT Hub and tooexplore other IoT scenarios, see:</span></span>

* <span data-ttu-id="1a4f0-221">[Connecting your device][lnk-connect-device] (Ansluta din enhet)</span><span class="sxs-lookup"><span data-stu-id="1a4f0-221">[Connecting your device][lnk-connect-device]</span></span>
* <span data-ttu-id="1a4f0-222">[Connecting your device][lnk-device-management] (Komma igång med enhetshantering)</span><span class="sxs-lookup"><span data-stu-id="1a4f0-222">[Getting started with device management][lnk-device-management]</span></span>
* <span data-ttu-id="1a4f0-223">[Komma igång med Azure IoT Edge][lnk-iot-edge]</span><span class="sxs-lookup"><span data-stu-id="1a4f0-223">[Getting started with Azure IoT Edge][lnk-iot-edge]</span></span>

<span data-ttu-id="1a4f0-224">toolearn hur tooextend IoT-lösningen och processen enhet till moln meddelandena i skala, se hello [bearbeta meddelanden från enhet till moln] [ lnk-process-d2c-tutorial] kursen.</span><span class="sxs-lookup"><span data-stu-id="1a4f0-224">toolearn how tooextend your IoT solution and process device-to-cloud messages at scale, see hello [Process device-to-cloud messages][lnk-process-d2c-tutorial] tutorial.</span></span>
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
