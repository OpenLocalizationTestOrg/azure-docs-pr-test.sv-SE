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
# <a name="get-started-with-device-twins-java"></a><span data-ttu-id="ba6de-104">Kom igång med enheten twins (Java)</span><span class="sxs-lookup"><span data-stu-id="ba6de-104">Get started with device twins (Java)</span></span>

[!INCLUDE [iot-hub-selector-twin-get-started](../../includes/iot-hub-selector-twin-get-started.md)]

<span data-ttu-id="ba6de-105">I kursen får skapa du två Java-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="ba6de-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="ba6de-106">**Lägg till-taggar-query**, en backend-Java-app som lägger till taggar och frågar enheten twins.</span><span class="sxs-lookup"><span data-stu-id="ba6de-106">**add-tags-query**, a Java back-end app that adds tags and queries device twins.</span></span>
* <span data-ttu-id="ba6de-107">**simulerade enheten**, en Java-enhetsapp att som ansluter tooyour IoT-hubb och rapporter tillståndet anslutning med hjälp av en rapporterade egenskap.</span><span class="sxs-lookup"><span data-stu-id="ba6de-107">**simulated-device**, a Java device app that that connects tooyour IoT hub and reports its connectivity condition using a reported property.</span></span>

> [!NOTE]
> <span data-ttu-id="ba6de-108">hello artikel [Azure IoT SDK](iot-hub-devguide-sdks.md) innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="ba6de-108">hello article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>

<span data-ttu-id="ba6de-109">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="ba6de-109">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="ba6de-110">Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="ba6de-110">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="ba6de-111">Maven 3</span><span class="sxs-lookup"><span data-stu-id="ba6de-111">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="ba6de-112">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="ba6de-112">An active Azure account.</span></span> <span data-ttu-id="ba6de-113">(Om du inte har ett konto kan du skapa en [kostnadsfritt konto](http://azure.microsoft.com/pricing/free-trial/) på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="ba6de-113">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="ba6de-114">Om du vill toocreate hello enhetsidentitet programmässigt kan läsa hello motsvarande avsnitt i hello [ansluta din enhet tooyour IoT-hubb som använder Java](iot-hub-java-java-getstarted.md#create-a-device-identity) artikel.</span><span class="sxs-lookup"><span data-stu-id="ba6de-114">If you prefer toocreate hello device identity programmatically, read hello corresponding section in hello [Connect your device tooyour IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="ba6de-115">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="ba6de-115">Create hello service app</span></span>

<span data-ttu-id="ba6de-116">I det här avsnittet skapar du en Java-app som platsen metadata läggs till som en tagg toohello enheten dubbla i IoT-hubb som är associerade med **myDeviceId**.</span><span class="sxs-lookup"><span data-stu-id="ba6de-116">In this section, you create a Java app that adds location metadata as a tag toohello device twin in IoT Hub associated with **myDeviceId**.</span></span> <span data-ttu-id="ba6de-117">hello app frågor IoT-hubb för enheter som finns i hello USA och sedan för enheter som rapporterar en mobilnät-anslutning.</span><span class="sxs-lookup"><span data-stu-id="ba6de-117">hello app first queries IoT hub for devices located in hello US, and then for devices that report a cellular network connection.</span></span>

1. <span data-ttu-id="ba6de-118">Skapa en tom mapp med namnet på utvecklingsdatorn, `iot-java-twin-getstarted`.</span><span class="sxs-lookup"><span data-stu-id="ba6de-118">On your development machine, create an empty folder called `iot-java-twin-getstarted`.</span></span>

1. <span data-ttu-id="ba6de-119">I hello `iot-java-twin-getstarted` mapp, skapa ett Maven-projekt som kallas **Lägg till-taggar-query** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ba6de-119">In hello `iot-java-twin-getstarted` folder, create a Maven project called **add-tags-query** using hello following command at your command prompt.</span></span> <span data-ttu-id="ba6de-120">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="ba6de-120">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=add-tags-query -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="ba6de-121">Kommandotolken, gå toohello `add-tags-query` mapp.</span><span class="sxs-lookup"><span data-stu-id="ba6de-121">At your command prompt, navigate toohello `add-tags-query` folder.</span></span>

1. <span data-ttu-id="ba6de-122">Använd en textredigerare och öppna hello `pom.xml` filen i hello `add-tags-query` mapp och Lägg till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="ba6de-122">Using a text editor, open hello `pom.xml` file in hello `add-tags-query` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="ba6de-123">Det här beroendet kan du toouse hello **iot-service-klient** paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="ba6de-123">This dependency enables you toouse hello **iot-service-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="ba6de-124">Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="ba6de-124">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="ba6de-125">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="ba6de-125">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="ba6de-126">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="ba6de-126">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="ba6de-127">Spara och Stäng hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="ba6de-127">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="ba6de-128">Använd en textredigerare och öppna hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="ba6de-128">Using a text editor, open hello `add-tags-query\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ba6de-129">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="ba6de-129">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.*;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.HashSet;
    import java.util.Set;
    ```

1. <span data-ttu-id="ba6de-130">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="ba6de-130">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="ba6de-131">Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="ba6de-131">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String region = "US";
    public static final String plant = "Redmond43";
    ```

1. <span data-ttu-id="ba6de-132">Uppdatera hello **huvudsakliga** metoden signatur tooinclude hello följande `throws` sats:</span><span class="sxs-lookup"><span data-stu-id="ba6de-132">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException
    ```

1. <span data-ttu-id="ba6de-133">Lägg till följande kod toohello hello **huvudsakliga** metoden toocreate hello **DeviceTwin** och **DeviceTwinDevice** objekt.</span><span class="sxs-lookup"><span data-stu-id="ba6de-133">Add hello following code toohello **main** method toocreate hello **DeviceTwin** and **DeviceTwinDevice** objects.</span></span> <span data-ttu-id="ba6de-134">Hej **DeviceTwin** objekt hanterar hello kommunikation med IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ba6de-134">hello **DeviceTwin** object handles hello communication with your IoT hub.</span></span> <span data-ttu-id="ba6de-135">Hej **DeviceTwinDevice** -objektet representerar hello enheten dubbla med dess egenskaper och taggar:</span><span class="sxs-lookup"><span data-stu-id="ba6de-135">hello **DeviceTwinDevice** object represents hello device twin with its properties and tags:</span></span>

    ```java
    // Get hello DeviceTwin and DeviceTwinDevice objects
    DeviceTwin twinClient = DeviceTwin.createFromConnectionString(iotHubConnectionString);
    DeviceTwinDevice device = new DeviceTwinDevice(deviceId);
    ```

1. <span data-ttu-id="ba6de-136">Lägg till följande hello `try/catch` blockera toohello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="ba6de-136">Add hello following `try/catch` block toohello **main** method:</span></span>

    ```java
    try {
      // Code goes here
    } catch (IotHubException e) {
      System.out.println(e.getMessage());
    } catch (IOException e) {
      System.out.println(e.getMessage());
    }
    ```

1. <span data-ttu-id="ba6de-137">tooupdate hello **region** och **anläggningen** enheten dubbla taggar i din enhet dubbla Lägg till följande kod i hello hello `try` block:</span><span class="sxs-lookup"><span data-stu-id="ba6de-137">tooupdate hello **region** and **plant** device twin tags in your device twin, add hello following code in hello `try` block:</span></span>

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

1. <span data-ttu-id="ba6de-138">tooquery hello enheten twins i IoT-hubb Lägg till följande kod toohello hello `try` block efter hello du lade till i hello föregående steg.</span><span class="sxs-lookup"><span data-stu-id="ba6de-138">tooquery hello device twins in IoT hub, add hello following code toohello `try` block after hello code you added in hello previous step.</span></span> <span data-ttu-id="ba6de-139">hello koden körs två frågor.</span><span class="sxs-lookup"><span data-stu-id="ba6de-139">hello code runs two queries.</span></span> <span data-ttu-id="ba6de-140">Varje fråga returnerar högst 100 enheter:</span><span class="sxs-lookup"><span data-stu-id="ba6de-140">Each query returns a maximum of 100 devices:</span></span>

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

1. <span data-ttu-id="ba6de-141">Spara och Stäng hello `add-tags-query\src\main\java\com\mycompany\app\App.java` fil</span><span class="sxs-lookup"><span data-stu-id="ba6de-141">Save and close hello `add-tags-query\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="ba6de-142">Skapa hello **Lägg till-taggar-query** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="ba6de-142">Build hello **add-tags-query** app and correct any errors.</span></span> <span data-ttu-id="ba6de-143">Kommandotolken, gå toohello `add-tags-query` mappen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ba6de-143">At your command prompt, navigate toohello `add-tags-query` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="ba6de-144">Skapa en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="ba6de-144">Create a device app</span></span>

<span data-ttu-id="ba6de-145">I det här avsnittet skapar du en Java-konsolapp som anger ett rapporterat egenskapsvärde som skickas tooIoT hubb.</span><span class="sxs-lookup"><span data-stu-id="ba6de-145">In this section, you create a Java console app that sets a reported property value that is sent tooIoT Hub.</span></span>

1. <span data-ttu-id="ba6de-146">I hello `iot-java-twin-getstarted` mapp, skapa ett Maven-projekt som kallas **simulerade enheten** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="ba6de-146">In hello `iot-java-twin-getstarted` folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="ba6de-147">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="ba6de-147">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="ba6de-148">Kommandotolken, gå toohello `simulated-device` mapp.</span><span class="sxs-lookup"><span data-stu-id="ba6de-148">At your command prompt, navigate toohello `simulated-device` folder.</span></span>

1. <span data-ttu-id="ba6de-149">Använd en textredigerare och öppna hello `pom.xml` filen i hello `simulated-device` mapp och Lägg till följande beroenden toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="ba6de-149">Using a text editor, open hello `pom.xml` file in hello `simulated-device` folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="ba6de-150">Det här beroendet kan du toouse hello **iot enhetsklienten** paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="ba6de-150">This dependency enables you toouse hello **iot-device-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="ba6de-151">Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="ba6de-151">You can check for hello latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="ba6de-152">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="ba6de-152">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="ba6de-153">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="ba6de-153">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="ba6de-154">Spara och Stäng hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="ba6de-154">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="ba6de-155">Använd en textredigerare och öppna hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="ba6de-155">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ba6de-156">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="ba6de-156">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="ba6de-157">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="ba6de-157">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="ba6de-158">Ersätta `{youriothubname}` med din IoT-hubbnamnet och `{yourdevicekey}` hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="ba6de-158">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    ```

    <span data-ttu-id="ba6de-159">Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="ba6de-159">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="ba6de-160">För närvarande toouse dubbla funktioner för enheter som du måste använda hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="ba6de-160">Currently, toouse device twin features you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="ba6de-161">Lägg till följande kod toohello hello **huvudsakliga** metod för att:</span><span class="sxs-lookup"><span data-stu-id="ba6de-161">Add hello following code toohello **main** method to:</span></span>
    * <span data-ttu-id="ba6de-162">Skapa en enhet klienten toocommunicate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="ba6de-162">Create a device client toocommunicate with IoT Hub.</span></span>
    * <span data-ttu-id="ba6de-163">Skapa en **enhet** objekt toostore hello dubbla Enhetsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="ba6de-163">Create a **Device** object toostore hello device twin properties.</span></span>

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

1. <span data-ttu-id="ba6de-164">Lägg till följande kod toohello hello **huvudsakliga** metoden toocreate en **connectivityType** rapporterade egenskapen och skicka den tooIoT hubb:</span><span class="sxs-lookup"><span data-stu-id="ba6de-164">Add hello following code toohello **main** method toocreate a **connectivityType** reported property and send it tooIoT Hub:</span></span>

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

1. <span data-ttu-id="ba6de-165">Lägg till hello efter koden toohello hello **huvudsakliga** metod.</span><span class="sxs-lookup"><span data-stu-id="ba6de-165">Add hello following code toohello end of hello **main** method.</span></span> <span data-ttu-id="ba6de-166">Väntar på hello **RETUR** nyckel kan tid för IoT-hubb tooreport hello status hello enheten dubbla åtgärder:</span><span class="sxs-lookup"><span data-stu-id="ba6de-166">Waiting for hello **Enter** key allows time for IoT Hub tooreport hello status of hello device twin operations:</span></span>

    ```java
    System.out.println("Press any key tooexit...");

    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();

    dataCollector.clean();
    client.close();
    ```

1. <span data-ttu-id="ba6de-167">Spara och Stäng hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="ba6de-167">Save and close hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="ba6de-168">Skapa hello **simulerade enheten** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="ba6de-168">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="ba6de-169">Kommandotolken, gå toohello `simulated-device` mappen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="ba6de-169">At your command prompt, navigate toohello `simulated-device` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="ba6de-170">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="ba6de-170">Run hello apps</span></span>

<span data-ttu-id="ba6de-171">Du är nu redo toorun hello konsolappar.</span><span class="sxs-lookup"><span data-stu-id="ba6de-171">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="ba6de-172">Vid en kommandotolk i hello `add-tags-query` mapp, kör följande kommando toorun hello hello **Lägg till-taggar-query** service-appen:</span><span class="sxs-lookup"><span data-stu-id="ba6de-172">At a command prompt in hello `add-tags-query` folder, run hello following command toorun hello **add-tags-query** service app:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app tooupdate tagga värden och köra frågor för enhet](media/iot-hub-java-java-twin-getstarted/service-app-1.png)

    <span data-ttu-id="ba6de-174">Du kan se hello **anläggningen** och **region** taggar läggs toohello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="ba6de-174">You can see hello **plant** and **region** tags added toohello device twin.</span></span> <span data-ttu-id="ba6de-175">hello första frågan returnerar enheten, men hello andra inte.</span><span class="sxs-lookup"><span data-stu-id="ba6de-175">hello first query returns your device, but hello second does not.</span></span>

1. <span data-ttu-id="ba6de-176">Vid en kommandotolk i hello `simulated-device` mapp, kör följande kommando tooadd hello hello **connectivityType** rapporterade egenskapen toohello enheten dubbla:</span><span class="sxs-lookup"><span data-stu-id="ba6de-176">At a command prompt in hello `simulated-device` folder, run hello following command tooadd hello **connectivityType** reported property toohello device twin:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Hej enhetsklienten lägger till hello ** connectivityType ** rapporterade egenskapen](media/iot-hub-java-java-twin-getstarted/device-app-1.png)

1. <span data-ttu-id="ba6de-178">Vid en kommandotolk i hello `add-tags-query` mapp, kör följande kommando toorun hello hello **Lägg till-taggar-query** service-appen en gång:</span><span class="sxs-lookup"><span data-stu-id="ba6de-178">At a command prompt in hello `add-tags-query` folder, run hello following command toorun hello **add-tags-query** service app a second time:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app tooupdate tagga värden och köra frågor för enhet](media/iot-hub-java-java-twin-getstarted/service-app-2.png)

    <span data-ttu-id="ba6de-180">Nu har enheten skickat hello **connectivityType** egenskapen tooIoT hubben, hello andra frågan returnerar enheten.</span><span class="sxs-lookup"><span data-stu-id="ba6de-180">Now your device has sent hello **connectivityType** property tooIoT Hub, hello second query returns your device.</span></span>

## <a name="next-steps"></a><span data-ttu-id="ba6de-181">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="ba6de-181">Next steps</span></span>

<span data-ttu-id="ba6de-182">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="ba6de-182">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="ba6de-183">Du lagt till enhetsmetadata som taggar från en backend-app och skrev en app tooreport enheten anslutning enhetsinformation i hello enheten dubbla.</span><span class="sxs-lookup"><span data-stu-id="ba6de-183">You added device metadata as tags from a back-end app, and wrote a device app tooreport device connectivity information in hello device twin.</span></span> <span data-ttu-id="ba6de-184">Du också lära dig hur tooquery hello dubbla enhetsinformation med hjälp av frågespråket för hello SQL-liknande IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="ba6de-184">You also learned how tooquery hello device twin information using hello SQL-like IoT Hub query language.</span></span>

<span data-ttu-id="ba6de-185">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="ba6de-185">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="ba6de-186">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="ba6de-186">Send telemetry from devices with hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="ba6de-187">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt](iot-hub-java-java-direct-methods.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="ba6de-187">Control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>

<!-- Images. -->
[7]: ./media/iot-hub-java-java-twin-getstarted/invoke-method.png
[8]: ./media/iot-hub-java-java-twin-getstarted/device-listen.png
[9]: ./media/iot-hub-java-java-twin-getstarted/device-respond.png

<!-- Links -->
[lnk-hub-sdks]: iot-hub-devguide-sdks.md
