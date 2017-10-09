---
title: "aaaGet igång med Azure IoT Hub enhetshantering (Java) | Microsoft Docs"
description: "Hur toouse Azure IoT Hub-enhet management tooinitiate fjärranslutna enheten startas om. Du kan använda hello Azure IoT-enhet SDK för Java tooimplement en simulerad enhetsapp som innehåller en direkt metod och hello Azure IoT-tjänsten SDK för Java tooimplement service-appen som anropar hello direkta metoden."
services: iot-hub
documentationcenter: .java
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/08/2017
ms.author: dobett
ms.openlocfilehash: 7aaeda9d4ff7002e5c66adfd61e2dfd5bcea964f
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="get-started-with-device-management-java"></a><span data-ttu-id="7168e-104">Komma igång med hantering av enheter (Java)</span><span class="sxs-lookup"><span data-stu-id="7168e-104">Get started with device management (Java)</span></span>

[!INCLUDE [iot-hub-selector-dm-getstarted](../../includes/iot-hub-selector-dm-getstarted.md)]

<span data-ttu-id="7168e-105">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="7168e-105">This tutorial shows you how to:</span></span>

* <span data-ttu-id="7168e-106">Använd hello Azure portal toocreate en IoT-hubb och skapa en enhetsidentitet i din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7168e-106">Use hello Azure portal toocreate an IoT Hub and create a device identity in your IoT hub.</span></span>
* <span data-ttu-id="7168e-107">Skapa en simulerad enhetsapp som implementerar en direkta metoden tooreboot hello-enhet.</span><span class="sxs-lookup"><span data-stu-id="7168e-107">Create a simulated device app that implements a direct method tooreboot hello device.</span></span> <span data-ttu-id="7168e-108">Direkta metoder anropas från hello molnet.</span><span class="sxs-lookup"><span data-stu-id="7168e-108">Direct methods are invoked from hello cloud.</span></span>
* <span data-ttu-id="7168e-109">Skapa en app som anropar hello omstart direkt metod i hello simulerade enheten appen via din IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="7168e-109">Create an app that invokes hello reboot direct method in hello simulated device app through your IoT hub.</span></span> <span data-ttu-id="7168e-110">Den här appen och sedan Övervakare hello rapporterade egenskaper från hello enheten toosee när hello omstart åtgärden är klar.</span><span class="sxs-lookup"><span data-stu-id="7168e-110">This app then monitors hello reported properties from hello device toosee when hello reboot operation is complete.</span></span>

<span data-ttu-id="7168e-111">Hello slutet av den här självstudiekursen har du två appar som Java-konsolen:</span><span class="sxs-lookup"><span data-stu-id="7168e-111">At hello end of this tutorial, you have two Java console apps:</span></span>

<span data-ttu-id="7168e-112">**simulerade enheten**.</span><span class="sxs-lookup"><span data-stu-id="7168e-112">**simulated-device**.</span></span> <span data-ttu-id="7168e-113">Den här appen:</span><span class="sxs-lookup"><span data-stu-id="7168e-113">This app:</span></span>

* <span data-ttu-id="7168e-114">Ansluter tooyour IoT-hubb hello enhetsidentitet skapade tidigare.</span><span class="sxs-lookup"><span data-stu-id="7168e-114">Connects tooyour IoT hub with hello device identity created earlier.</span></span>
* <span data-ttu-id="7168e-115">Tar emot ett direkt metodanrop för omstart.</span><span class="sxs-lookup"><span data-stu-id="7168e-115">Receives a reboot direct method call.</span></span>
* <span data-ttu-id="7168e-116">Simulerar en fysisk omstart.</span><span class="sxs-lookup"><span data-stu-id="7168e-116">Simulates a physical reboot.</span></span>
* <span data-ttu-id="7168e-117">Rapporter hello tid hello senaste omstarten via en rapporterade egenskap.</span><span class="sxs-lookup"><span data-stu-id="7168e-117">Reports hello time of hello last reboot through a reported property.</span></span>

<span data-ttu-id="7168e-118">**utlösaren omstart**.</span><span class="sxs-lookup"><span data-stu-id="7168e-118">**trigger-reboot**.</span></span> <span data-ttu-id="7168e-119">Den här appen:</span><span class="sxs-lookup"><span data-stu-id="7168e-119">This app:</span></span>

* <span data-ttu-id="7168e-120">Anropar en direkt metod i hello simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="7168e-120">Calls a direct method in hello simulated device app.</span></span>
* <span data-ttu-id="7168e-121">Visar hello svar toohello direkt metodanrop skickas av hello simulerade enheten</span><span class="sxs-lookup"><span data-stu-id="7168e-121">Displays hello response toohello direct method call sent by hello simulated device</span></span>
* <span data-ttu-id="7168e-122">Visar hello uppdateras rapporterade egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7168e-122">Displays hello updated reported properties.</span></span>

> [!NOTE]
> <span data-ttu-id="7168e-123">Information om hello SDK: er som du kan använda toobuild program toorun på enheter och din lösningens serverdel finns [Azure IoT SDK][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="7168e-123">For information about hello SDKs that you can use toobuild applications toorun on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="7168e-124">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="7168e-124">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="7168e-125">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="7168e-125">Java SE 8.</span></span> <br/> <span data-ttu-id="7168e-126">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Java för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="7168e-126">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="7168e-127">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="7168e-127">Maven 3.</span></span>  <br/> <span data-ttu-id="7168e-128">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall [Maven] [ lnk-maven] för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="7168e-128">[Prepare your development environment][lnk-dev-setup] describes how tooinstall [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="7168e-129">[Node.js-version 0.10.0 eller senare](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="7168e-129">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="trigger-a-remote-reboot-on-hello-device-using-a-direct-method"></a><span data-ttu-id="7168e-130">Utlös en fjärransluten omstart på hello-enhet med en direkt metod</span><span class="sxs-lookup"><span data-stu-id="7168e-130">Trigger a remote reboot on hello device using a direct method</span></span>

<span data-ttu-id="7168e-131">I det här avsnittet kan skapa du en Java-konsolapp som:</span><span class="sxs-lookup"><span data-stu-id="7168e-131">In this section, you create a Java console app that:</span></span>

1. <span data-ttu-id="7168e-132">Anropar hello omstart direkt metod i hello simulerade enhetsapp.</span><span class="sxs-lookup"><span data-stu-id="7168e-132">Invokes hello reboot direct method in hello simulated device app.</span></span>
1. <span data-ttu-id="7168e-133">Visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="7168e-133">Displays hello response.</span></span>
1. <span data-ttu-id="7168e-134">Omröstningar hello rapporterade egenskaper som skickas från hello enheten toodetermine när hello omstarten är klar.</span><span class="sxs-lookup"><span data-stu-id="7168e-134">Polls hello reported properties sent from hello device toodetermine when hello reboot is complete.</span></span>

<span data-ttu-id="7168e-135">Den här appen konsolen ansluter tooyour IoT-hubb tooinvoke hello direkta metoden och Läs hello rapporteras egenskaper.</span><span class="sxs-lookup"><span data-stu-id="7168e-135">This console app connects tooyour IoT Hub tooinvoke hello direct method and read hello reported properties.</span></span>

1. <span data-ttu-id="7168e-136">Skapa en tom mapp som kallas dm-get-started.</span><span class="sxs-lookup"><span data-stu-id="7168e-136">Create an empty folder called dm-get-started.</span></span>

1. <span data-ttu-id="7168e-137">Skapa ett Maven-projekt som kallas i hello dm-get-started mappen **utlösaren omstart** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="7168e-137">In hello dm-get-started folder, create a Maven project called **trigger-reboot** using hello following command at your command prompt.</span></span> <span data-ttu-id="7168e-138">hello följande visar ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="7168e-138">hello following shows a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=trigger-reboot -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="7168e-139">Navigera toohello utlösaren omstart mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="7168e-139">At your command prompt, navigate toohello trigger-reboot folder.</span></span>

1. <span data-ttu-id="7168e-140">Med en textredigerare, öppna hello pom.xml filen hello utlösaren omstart mapp och lägga till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="7168e-140">Using a text editor, open hello pom.xml file in hello trigger-reboot folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="7168e-141">Det här beroendet kan du toouse hello iot-service-klient paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="7168e-141">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="7168e-142">Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="7168e-142">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="7168e-143">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="7168e-143">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="7168e-144">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="7168e-144">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="7168e-145">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="7168e-145">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="7168e-146">Använd en textredigerare, öppna hello trigger-reboot\src\main\java\com\mycompany\app\App.java källfilen.</span><span class="sxs-lookup"><span data-stu-id="7168e-146">Using a text editor, open hello trigger-reboot\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="7168e-147">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="7168e-147">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwin;
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    import java.util.concurrent.Executors;
    import java.util.concurrent.ExecutorService;
    ```

1. <span data-ttu-id="7168e-148">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="7168e-148">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="7168e-149">Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7168e-149">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    private static final String methodName = "reboot";
    private static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    private static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    ```

1. <span data-ttu-id="7168e-150">tooimplement en tråd som läser hello rapporterade egenskaper hello enheten dubbla var 10: e sekund, lägga till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="7168e-150">tooimplement a thread that reads hello reported properties from hello device twin every 10 seconds, add hello following nested class toohello **App** class:</span></span>

    ```java
    private static class ShowReportedProperties implements Runnable {
      public void run() {
        try {
          DeviceTwin deviceTwins = DeviceTwin.createFromConnectionString(iotHubConnectionString);
          DeviceTwinDevice twinDevice = new DeviceTwinDevice(deviceId);
          while (true) {
            System.out.println("Get reported properties from device twin");
            deviceTwins.getTwin(twinDevice);
            System.out.println(twinDevice.reportedPropertiesToString());
            Thread.sleep(10000);
          }
        } catch (Exception ex) {
          System.out.println("Exception reading reported properties: " + ex.getMessage());
        }
      }
    }
    ```

1. <span data-ttu-id="7168e-151">tooinvoke hello omstart direkt metod på hello simulerade enheten, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="7168e-151">tooinvoke hello reboot direct method on hello simulated device, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Starting sample...");
    DeviceMethod methodClient = DeviceMethod.createFromConnectionString(iotHubConnectionString);

    try
    {
      System.out.println("Invoke reboot direct method");
      MethodResult result = methodClient.invoke(deviceId, methodName, responseTimeout, connectTimeout, null);

      if(result == null)
      {
        throw new IOException("Invoke direct method reboot returns null");
      }
      System.out.println("Invoked reboot on device");
      System.out.println("Status for device:   " + result.getStatus());
      System.out.println("Message from device: " + result.getPayload());
    }
    catch (IotHubException e)
    {
        System.out.println(e.getMessage());
    }
    ```

1. <span data-ttu-id="7168e-152">toostart hello tråd toopoll Hej rapporterade egenskaper från hello simulerade enhet, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="7168e-152">toostart hello thread toopoll hello reported properties from hello simulated device, add hello following code toohello **main** method:</span></span>

    ```java
    ShowReportedProperties showReportedProperties = new ShowReportedProperties();
    ExecutorService executor = Executors.newFixedThreadPool(1);
    executor.execute(showReportedProperties);
    ```

1. <span data-ttu-id="7168e-153">tooenable du toostop hello app, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="7168e-153">tooenable you toostop hello app, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Press ENTER tooexit.");
    System.in.read();
    executor.shutdownNow();
    System.out.println("Shutting down sample...");
    ```

1. <span data-ttu-id="7168e-154">Spara och Stäng hello trigger-reboot\src\main\java\com\mycompany\app\App.java fil.</span><span class="sxs-lookup"><span data-stu-id="7168e-154">Save and close hello trigger-reboot\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="7168e-155">Skapa hello **utlösaren omstart** backend-app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="7168e-155">Build hello **trigger-reboot** back-end app and correct any errors.</span></span> <span data-ttu-id="7168e-156">Navigera toohello utlösaren omstart mappen och kör hello följande kommando vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="7168e-156">At your command prompt, navigate toohello trigger-reboot folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="7168e-157">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="7168e-157">Create a simulated device app</span></span>

<span data-ttu-id="7168e-158">I det här avsnittet skapar du en Java-konsolapp som simulerar en enhet.</span><span class="sxs-lookup"><span data-stu-id="7168e-158">In this section, you create a Java console app that simulates a device.</span></span> <span data-ttu-id="7168e-159">hello app lyssnar efter metodanrop för hello omstart direkt från din IoT-hubb och svarar omedelbart toothat anrop.</span><span class="sxs-lookup"><span data-stu-id="7168e-159">hello app listens for hello reboot direct method call from your IoT hub and immediately responds toothat call.</span></span> <span data-ttu-id="7168e-160">Hej appen och klicka sedan på ett tag i viloläge toosimulate hello omstart process innan den använder en rapporterade egenskapen toonotify hello **utlösaren omstart** backend-app som hello omstarten är klar.</span><span class="sxs-lookup"><span data-stu-id="7168e-160">hello app then sleeps for a while toosimulate hello reboot process before it uses a reported property toonotify hello **trigger-reboot** back-end app that hello reboot is complete.</span></span>

1. <span data-ttu-id="7168e-161">Skapa ett Maven-projekt som kallas i hello dm-get-started mappen **simulerade enheten** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="7168e-161">In hello dm-get-started folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="7168e-162">hello följande är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="7168e-162">hello following is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="7168e-163">Navigera toohello simulerade enheten mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="7168e-163">At your command prompt, navigate toohello simulated-device folder.</span></span>

1. <span data-ttu-id="7168e-164">Med en textredigerare, öppna hello pom.xml filen hello simulerade enheten mapp och lägga till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="7168e-164">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="7168e-165">Det här beroendet kan du toouse hello iot-service-klient paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="7168e-165">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="7168e-166">Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="7168e-166">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="7168e-167">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="7168e-167">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="7168e-168">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="7168e-168">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="7168e-169">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="7168e-169">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="7168e-170">Använd en textredigerare, öppna hello simulated-device\src\main\java\com\mycompany\app\App.java källfilen.</span><span class="sxs-lookup"><span data-stu-id="7168e-170">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java source file.</span></span>

1. <span data-ttu-id="7168e-171">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="7168e-171">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.time.LocalDateTime;
    import java.util.Scanner;
    import java.util.Set;
    import java.util.HashSet;
    ```

1. <span data-ttu-id="7168e-172">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="7168e-172">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="7168e-173">Ersätt `{yourdeviceconnectionstring}` med hello enheten anslutningssträng som du antecknade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="7168e-173">Replace `{yourdeviceconnectionstring}` with hello device connection string you noted in hello *Create a device identity* section:</span></span>

    ```java
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;

    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String connString = "{yourdeviceconnectionstring}";
    private static DeviceClient client;
    ```

1. <span data-ttu-id="7168e-174">tooimplement en motringning hanterare för direkta metoden statushändelser, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="7168e-174">tooimplement a callback handler for direct method status events, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="7168e-175">tooimplement en motringning hanterare för enheten dubbla statushändelser, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="7168e-175">tooimplement a callback handler for device twin status events, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DeviceTwinStatusCallback implements IotHubEventCallback
    {
        public void execute(IotHubStatusCode status, Object context)
        {
            System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
        }
    }
    ```

1. <span data-ttu-id="7168e-176">tooimplement en motringning hanterare för egenskapen händelser, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="7168e-176">tooimplement a callback handler for property events, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class PropertyCallback implements PropertyCallBack<String, String>
    {
      public void PropertyCall(String propertyKey, String propertyValue, Object context)
      {
        System.out.println("PropertyKey:     " + propertyKey);
        System.out.println("PropertyKvalue:  " + propertyKey);
      }
    }
    ```

1. <span data-ttu-id="7168e-177">tooimplement en tråd toosimulate hello enheten omstart, Lägg till följande hello kapslad klass toohello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="7168e-177">tooimplement a thread toosimulate hello device reboot, add hello following nested class toohello **App** class.</span></span> <span data-ttu-id="7168e-178">hello tråd i viloläge i fem sekunder och sedan anger hello **lastReboot** rapporterade egenskapen:</span><span class="sxs-lookup"><span data-stu-id="7168e-178">hello thread sleeps for five seconds and then sets hello **lastReboot** reported property:</span></span>

    ```java
    protected static class RebootDeviceThread implements Runnable {
      public void run() {
        try {
          System.out.println("Rebooting...");
          Thread.sleep(5000);
          Property property = new Property("lastReboot", LocalDateTime.now());
          Set<Property> properties = new HashSet<Property>();
          properties.add(property);
          client.sendReportedProperties(properties);
          System.out.println("Rebooted");
        }
        catch (Exception ex) {
          System.out.println("Exception in reboot thread: " + ex.getMessage());
        }
      }
    }
    ```

1. <span data-ttu-id="7168e-179">tooimplement hello direkt metod på hello enhet Lägg till följande hello kapslad klass toohello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="7168e-179">tooimplement hello direct method on hello device, add hello following nested class toohello **App** class.</span></span> <span data-ttu-id="7168e-180">När hello simulerade appen tar emot ett samtal toohello **omstart** direkta metoden returnerar en bekräftelse toohello anroparen och sedan startar en tråd tooprocess hello omstart:</span><span class="sxs-lookup"><span data-stu-id="7168e-180">When hello simulated app receives a call toohello **reboot** direct method, it returns an acknowledgement toohello caller and then starts a thread tooprocess hello reboot:</span></span>

    ```java
    protected static class DirectMethodCallback implements com.microsoft.azure.sdk.iot.device.DeviceTwin.DeviceMethodCallback
    {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context)
      {
        DeviceMethodData deviceMethodData;
        switch (methodName)
        {
          case "reboot" :
          {
            int status = METHOD_SUCCESS;
            System.out.println("Received reboot request");
            deviceMethodData = new DeviceMethodData(status, "Started reboot");
            RebootDeviceThread rebootThread = new RebootDeviceThread();
            Thread t = new Thread(rebootThread);
            t.start();
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

1. <span data-ttu-id="7168e-181">Ändra hello signaturen för hello **huvudsakliga** metoden toothrow hello följande undantag:</span><span class="sxs-lookup"><span data-stu-id="7168e-181">Modify hello signature of hello **main** method toothrow hello following exceptions:</span></span>

    ```java
    public static void main(String[] args) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="7168e-182">tooinstantiate en **DeviceClient**, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="7168e-182">tooinstantiate a **DeviceClient**, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Starting device client sample...");
    client = new DeviceClient(connString, protocol);
    ```

1. <span data-ttu-id="7168e-183">toostart lyssnar efter direkta metodanrop, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="7168e-183">toostart listening for direct method calls, add hello following code toohello **main** method:</span></span>

    ```java
    try
    {
      client.open();
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
      client.startDeviceTwin(new DeviceTwinStatusCallback(), null, new PropertyCallback(), null);
      System.out.println("Subscribed toodirect methods and polling for reported properties. Waiting...");
    }
    catch (Exception e)
    {
      System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. <span data-ttu-id="7168e-184">tooshut ned hello enheten simulatorn Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="7168e-184">tooshut down hello device simulator, add hello following code toohello **main** method:</span></span>

    ```java
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    scanner.close();
    client.close();
    System.out.println("Shutting down...");
    ```

1. <span data-ttu-id="7168e-185">Spara och Stäng hello simulated-device\src\main\java\com\mycompany\app\App.java fil.</span><span class="sxs-lookup"><span data-stu-id="7168e-185">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="7168e-186">Skapa hello **simulerade enheten** backend-app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="7168e-186">Build hello **simulated-device** back-end app and correct any errors.</span></span> <span data-ttu-id="7168e-187">Navigera toohello simulerade enheten mappen och kör hello följande kommando vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="7168e-187">At your command prompt, navigate toohello simulated-device folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="7168e-188">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="7168e-188">Run hello apps</span></span>

<span data-ttu-id="7168e-189">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="7168e-189">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="7168e-190">Kör hello efter kommandot toobegin lyssnar efter omstart metodanrop från din IoT-hubb i en kommandotolk i hello simulerade enheten mapp:</span><span class="sxs-lookup"><span data-stu-id="7168e-190">At a command prompt in hello simulated-device folder, run hello following command toobegin listening for reboot method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb simulerade enheten app toolisten för omstart direkt metodanrop][1]

1. <span data-ttu-id="7168e-192">Kör hello efter kommandot toocall hello omstart metoden på den simulerade enheten från din IoT-hubb i en kommandotolk i hello utlösaren omstart mapp:</span><span class="sxs-lookup"><span data-stu-id="7168e-192">At a command prompt in hello trigger-reboot folder, run hello following command toocall hello reboot method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app toocall hello omstart direkt metod][2]

1. <span data-ttu-id="7168e-194">hello simulerade enheten svarar toohello omstart direkt metodanrop:</span><span class="sxs-lookup"><span data-stu-id="7168e-194">hello simulated device responds toohello reboot direct method call:</span></span>

    ![Java IoT-hubb simulerade enhetsapp svarar toohello direkt metodanrop][3]

[!INCLUDE [iot-hub-dm-followup](../../includes/iot-hub-dm-followup.md)]

<!-- images and links -->
[1]: ./media/iot-hub-java-java-device-management-getstarted/launchsimulator.png
[2]: ./media/iot-hub-java-java-device-management-getstarted/triggerreboot.png
[3]: ./media/iot-hub-java-java-device-management-getstarted/respondtoreboot.png
<!-- Links -->

[lnk-maven]: https://maven.apache.org/what-is-maven.html

[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-java/blob/master/doc/java-devbox-setup.md

[lnk-hub-sdks]: iot-hub-devguide-sdks.md

[lnk-maven-service-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22
[lnk-maven-device-search]: http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22