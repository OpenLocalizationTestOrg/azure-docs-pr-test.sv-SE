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
# <a name="use-direct-methods-java"></a><span data-ttu-id="1af3b-104">Använda direkt metoder (Java)</span><span class="sxs-lookup"><span data-stu-id="1af3b-104">Use direct methods (Java)</span></span>

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="1af3b-105">I kursen får skapa du två Java-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="1af3b-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="1af3b-106">**anropa-direct-method**, en backend-Java-app som anropar en metod i hello simulerade enhetsapp och visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="1af3b-106">**invoke-direct-method**, a Java back-end app that calls a method in hello simulated device app and displays hello response.</span></span>
* <span data-ttu-id="1af3b-107">**simulerade enheten**, en Java-app som simulerar en enhet som ansluter tooyour IoT-hubb med hello enhets-ID som du skapar.</span><span class="sxs-lookup"><span data-stu-id="1af3b-107">**simulated-device**, a Java app that simulates a device connecting tooyour IoT hub with hello device identity you create.</span></span> <span data-ttu-id="1af3b-108">Den här appen svarar toohello anropas direkt från hello serverdel.</span><span class="sxs-lookup"><span data-stu-id="1af3b-108">This app responds toohello direct invoked from hello back end.</span></span>

> [!NOTE]
> <span data-ttu-id="1af3b-109">Information om hello SDK: er som du kan använda toobuild program toorun på enheter och din lösningens serverdel finns [Azure IoT SDK][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="1af3b-109">For information about hello SDKs that you can use toobuild applications toorun on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="1af3b-110">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="1af3b-110">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="1af3b-111">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="1af3b-111">Java SE 8.</span></span> <br/> <span data-ttu-id="1af3b-112">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Java för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="1af3b-112">[Prepare your development environment][lnk-dev-setup] describes how tooinstall Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="1af3b-113">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="1af3b-113">Maven 3.</span></span>  <br/> <span data-ttu-id="1af3b-114">[Förbered din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall [Maven] [ lnk-maven] för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="1af3b-114">[Prepare your development environment][lnk-dev-setup] describes how tooinstall [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="1af3b-115">[Node.js-version 0.10.0 eller senare](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="1af3b-115">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="1af3b-116">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="1af3b-116">Create a simulated device app</span></span>

<span data-ttu-id="1af3b-117">I det här avsnittet skapar du en Java-konsolapp som svarar tooa metoden anropas av hello-lösningen slutet.</span><span class="sxs-lookup"><span data-stu-id="1af3b-117">In this section, you create a Java console app that responds tooa method called by hello solution back end.</span></span>

1. <span data-ttu-id="1af3b-118">Skapa en tom mapp som kallas iot-java-direct-metoden.</span><span class="sxs-lookup"><span data-stu-id="1af3b-118">Create an empty folder called iot-java-direct-method.</span></span>

1. <span data-ttu-id="1af3b-119">Skapa ett Maven-projekt som kallas i hello iot-java-direct-method mappen **simulerade enheten** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1af3b-119">In hello iot-java-direct-method folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="1af3b-120">hello följande kommando är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="1af3b-120">hello following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="1af3b-121">Navigera toohello simulerade enheten mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1af3b-121">At your command prompt, navigate toohello simulated-device folder.</span></span>

1. <span data-ttu-id="1af3b-122">Med en textredigerare, öppna hello pom.xml filen hello simulerade enheten mapp och lägga till följande beroenden toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="1af3b-122">Using a text editor, open hello pom.xml file in hello simulated-device folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="1af3b-123">Det här beroendet kan du toouse hello iot enhetsklienten paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="1af3b-123">This dependency enables you toouse hello iot-device-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="1af3b-124">Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="1af3b-124">You can check for hello latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="1af3b-125">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="1af3b-125">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="1af3b-126">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="1af3b-126">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="1af3b-127">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="1af3b-127">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="1af3b-128">Använd en textredigerare och öppna hello simulated-device\src\main\java\com\mycompany\app\App.java filen.</span><span class="sxs-lookup"><span data-stu-id="1af3b-128">Using a text editor, open hello simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="1af3b-129">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="1af3b-129">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="1af3b-130">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="1af3b-130">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="1af3b-131">Ersätta `{youriothubname}` med din IoT-hubbnamnet och `{yourdevicekey}` hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="1af3b-131">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="1af3b-132">Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="1af3b-132">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="1af3b-133">För närvarande direkt toouse metoder som du måste använda hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="1af3b-133">Currently, toouse direct methods you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="1af3b-134">tooreturn en status kod tooyour IoT-hubb, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="1af3b-134">tooreturn a status code tooyour IoT hub, add hello following nested class toohello **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded toodevice method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="1af3b-135">toohandle hello direkta metoden anrop från hello lösningens serverdel, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="1af3b-135">toohandle hello direct method invocations from hello solution back end, add hello following nested class toohello **App** class:</span></span>

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

1. <span data-ttu-id="1af3b-136">toocreate en **DeviceClient** och lyssna efter direkta metoden anrop, lägga till en **huvudsakliga** metoden toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="1af3b-136">toocreate a **DeviceClient** and listen for direct method invocations, add a **main** method toohello **App** class:</span></span>

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

1. <span data-ttu-id="1af3b-137">Spara och Stäng hello simulated-device\src\main\java\com\mycompany\app\App.java fil</span><span class="sxs-lookup"><span data-stu-id="1af3b-137">Save and close hello simulated-device\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="1af3b-138">Skapa hello **simulerade enheten** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="1af3b-138">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="1af3b-139">Navigera toohello simulerade enheten mappen och kör hello följande kommando vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="1af3b-139">At your command prompt, navigate toohello simulated-device folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="1af3b-140">Anropa en metod som direkt på en enhet</span><span class="sxs-lookup"><span data-stu-id="1af3b-140">Call a direct method on a device</span></span>

<span data-ttu-id="1af3b-141">I det här avsnittet skapar du en Java-konsolapp som anropar en direkt metod och sedan visar hello svar.</span><span class="sxs-lookup"><span data-stu-id="1af3b-141">In this section, you create a Java console app that invokes a direct method and then displays hello response.</span></span> <span data-ttu-id="1af3b-142">Den här appen konsolen ansluter tooyour IoT-hubb tooinvoke hello direkta metoden.</span><span class="sxs-lookup"><span data-stu-id="1af3b-142">This console app connects tooyour IoT Hub tooinvoke hello direct method.</span></span>

1. <span data-ttu-id="1af3b-143">Skapa ett Maven-projekt som kallas i hello iot-java-direct-method mappen **invoke-direct-metoden** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1af3b-143">In hello iot-java-direct-method folder, create a Maven project called **invoke-direct-method** using hello following command at your command prompt.</span></span> <span data-ttu-id="1af3b-144">hello följande kommando är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="1af3b-144">hello following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="1af3b-145">Navigera toohello invoke-direct-method mapp i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="1af3b-145">At your command prompt, navigate toohello invoke-direct-method folder.</span></span>

1. <span data-ttu-id="1af3b-146">Med en textredigerare, öppna hello pom.xml filen hello invoke-direct-method mapp och lägga till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="1af3b-146">Using a text editor, open hello pom.xml file in hello invoke-direct-method folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="1af3b-147">Det här beroendet kan du toouse hello iot-service-klient paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="1af3b-147">This dependency enables you toouse hello iot-service-client package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="1af3b-148">Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="1af3b-148">You can check for hello latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="1af3b-149">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="1af3b-149">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="1af3b-150">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="1af3b-150">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="1af3b-151">Spara och Stäng hello pom.xml fil.</span><span class="sxs-lookup"><span data-stu-id="1af3b-151">Save and close hello pom.xml file.</span></span>

1. <span data-ttu-id="1af3b-152">Använd en textredigerare och öppna hello invoke-direct-method\src\main\java\com\mycompany\app\App.java filen.</span><span class="sxs-lookup"><span data-stu-id="1af3b-152">Using a text editor, open hello invoke-direct-method\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="1af3b-153">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="1af3b-153">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. <span data-ttu-id="1af3b-154">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="1af3b-154">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="1af3b-155">Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="1af3b-155">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line toobe written";
    ```

1. <span data-ttu-id="1af3b-156">tooinvoke hello metod på hello simulerade enheten, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="1af3b-156">tooinvoke hello method on hello simulated device, add hello following code toohello **main** method:</span></span>

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

1. <span data-ttu-id="1af3b-157">Spara och Stäng hello invoke-direct-method\src\main\java\com\mycompany\app\App.java fil</span><span class="sxs-lookup"><span data-stu-id="1af3b-157">Save and close hello invoke-direct-method\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="1af3b-158">Skapa hello **invoke-direct-method** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="1af3b-158">Build hello **invoke-direct-method** app and correct any errors.</span></span> <span data-ttu-id="1af3b-159">Navigera toohello invoke-direct-method mappen och kör hello följande kommando vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="1af3b-159">At your command prompt, navigate toohello invoke-direct-method folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="1af3b-160">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="1af3b-160">Run hello apps</span></span>

<span data-ttu-id="1af3b-161">Du är nu redo toorun hello konsolappar.</span><span class="sxs-lookup"><span data-stu-id="1af3b-161">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="1af3b-162">Kör hello efter kommandot toobegin lyssnar efter metodanrop från din IoT-hubb i en kommandotolk i hello simulerade enheten mapp:</span><span class="sxs-lookup"><span data-stu-id="1af3b-162">At a command prompt in hello simulated-device folder, run hello following command toobegin listening for method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb simulerade enheten app toolisten för direkta metodanrop][8]

1. <span data-ttu-id="1af3b-164">Vid en kommandotolk i hello invoke-direct-method mappen kör hello efter kommandot toocall en metod på den simulerade enheten från din IoT-hubb:</span><span class="sxs-lookup"><span data-stu-id="1af3b-164">At a command prompt in hello invoke-direct-method folder, run hello following command toocall a method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service app toocall direkt metod][7]

1. <span data-ttu-id="1af3b-166">hello simulerade enheten svarar toohello direkt metodanrop:</span><span class="sxs-lookup"><span data-stu-id="1af3b-166">hello simulated device responds toohello direct method call:</span></span>

    ![Java IoT-hubb simulerade enhetsapp svarar toohello direkt metodanrop][9]

## <a name="next-steps"></a><span data-ttu-id="1af3b-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="1af3b-168">Next steps</span></span>

<span data-ttu-id="1af3b-169">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="1af3b-169">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="1af3b-170">Du har använt den här enheten identitet tooenable hello simulerade enheten app tooreact toomethods anropas av hello molnet.</span><span class="sxs-lookup"><span data-stu-id="1af3b-170">You used this device identity tooenable hello simulated device app tooreact toomethods invoked by hello cloud.</span></span> <span data-ttu-id="1af3b-171">Skapade du även en app som anropar metoder på hello enhet och visar hello svar från hello enhet.</span><span class="sxs-lookup"><span data-stu-id="1af3b-171">You also created an app that invokes methods on hello device and displays hello response from hello device.</span></span>

<span data-ttu-id="1af3b-172">tooexplore andra IoT-scenarier finns [schemalägga jobb på flera enheter][lnk-devguide-jobs].</span><span class="sxs-lookup"><span data-stu-id="1af3b-172">tooexplore other IoT scenarios, see [Schedule jobs on multiple devices][lnk-devguide-jobs].</span></span>

<span data-ttu-id="1af3b-173">toolearn hur tooextend din IoT-lösningen och schema metodanrop på flera enheter, finns i hello [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="1af3b-173">toolearn how tooextend your IoT solution and schedule method calls on multiple devices, see hello [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
