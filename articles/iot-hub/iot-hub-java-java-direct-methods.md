---
title: "Använda Azure IoT Hub direkt metoder (Java) | Microsoft Docs"
description: "Hur du använder Azure IoT Hub direkt metoder. Azure IoT-enhet SDK för Java används för att implementera en simulerad enhetsapp som innehåller en direkt metod och Azure IoT SDK för Java att implementera ett service-appen som anropar metoden direkt."
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
ms.openlocfilehash: 6243a1a8cc971c53c797182b2beb6f594d2ac5f7
ms.sourcegitcommit: 50e23e8d3b1148ae2d36dad3167936b4e52c8a23
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/18/2017
---
# <a name="use-direct-methods-java"></a><span data-ttu-id="f5b54-104">Använda direkt metoder (Java)</span><span class="sxs-lookup"><span data-stu-id="f5b54-104">Use direct methods (Java)</span></span>

[!INCLUDE [iot-hub-selector-c2d-methods](../../includes/iot-hub-selector-c2d-methods.md)]

<span data-ttu-id="f5b54-105">I kursen får skapa du två Java-konsolappar:</span><span class="sxs-lookup"><span data-stu-id="f5b54-105">In this tutorial, you create two Java console apps:</span></span>

* <span data-ttu-id="f5b54-106">**anropa-direct-method**, en backend-Java-app som anropar en metod i appen simulerade enheten och visar svaret.</span><span class="sxs-lookup"><span data-stu-id="f5b54-106">**invoke-direct-method**, a Java back-end app that calls a method in the simulated device app and displays the response.</span></span>
* <span data-ttu-id="f5b54-107">**simulerade enheten**, en Java-app som simulerar en enhet som ansluter till din IoT-hubb med enhetens identitet som du skapar.</span><span class="sxs-lookup"><span data-stu-id="f5b54-107">**simulated-device**, a Java app that simulates a device connecting to your IoT hub with the device identity you create.</span></span> <span data-ttu-id="f5b54-108">Den här appen svarar direkt anropas från serverdelen.</span><span class="sxs-lookup"><span data-stu-id="f5b54-108">This app responds to the direct invoked from the back end.</span></span>

> [!NOTE]
> <span data-ttu-id="f5b54-109">Information om SDK: er som du kan använda för att bygga program ska köras på enheter och din lösningens serverdel finns [Azure IoT SDK][lnk-hub-sdks].</span><span class="sxs-lookup"><span data-stu-id="f5b54-109">For information about the SDKs that you can use to build applications to run on devices and your solution back end, see [Azure IoT SDKs][lnk-hub-sdks].</span></span>

<span data-ttu-id="f5b54-110">För att slutföra den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="f5b54-110">To complete this tutorial, you need:</span></span>

* <span data-ttu-id="f5b54-111">Java SE 8.</span><span class="sxs-lookup"><span data-stu-id="f5b54-111">Java SE 8.</span></span> <br/> <span data-ttu-id="f5b54-112">[Förbereda utvecklingsmiljön][lnk-dev-setup] beskriver hur du installerar Java för den här självstudiekursen i Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="f5b54-112">[Prepare your development environment][lnk-dev-setup] describes how to install Java for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="f5b54-113">Maven 3.</span><span class="sxs-lookup"><span data-stu-id="f5b54-113">Maven 3.</span></span>  <br/> <span data-ttu-id="f5b54-114">[Förbereda utvecklingsmiljön][lnk-dev-setup] beskriver hur du installerar [Maven][lnk-maven] för den här självstudiekursen i Windows eller Linux.</span><span class="sxs-lookup"><span data-stu-id="f5b54-114">[Prepare your development environment][lnk-dev-setup] describes how to install [Maven][lnk-maven] for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="f5b54-115">[Node.js-version 0.10.0 eller senare](http://nodejs.org).</span><span class="sxs-lookup"><span data-stu-id="f5b54-115">[Node.js version 0.10.0 or later](http://nodejs.org).</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="f5b54-116">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="f5b54-116">Create a simulated device app</span></span>

<span data-ttu-id="f5b54-117">I det här avsnittet skapar du en Java-konsolapp som svarar på en metod som anropas av lösningen tillbaka slutet.</span><span class="sxs-lookup"><span data-stu-id="f5b54-117">In this section, you create a Java console app that responds to a method called by the solution back end.</span></span>

1. <span data-ttu-id="f5b54-118">Skapa en tom mapp som kallas iot-java-direct-metoden.</span><span class="sxs-lookup"><span data-stu-id="f5b54-118">Create an empty folder called iot-java-direct-method.</span></span>

1. <span data-ttu-id="f5b54-119">I mappen iot-java-direct-metoden skapar du ett Maven-projekt som kallas **simulerade enheten** med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f5b54-119">In the iot-java-direct-method folder, create a Maven project called **simulated-device** using the following command at your command prompt.</span></span> <span data-ttu-id="f5b54-120">Följande kommando är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="f5b54-120">The following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="f5b54-121">Gå till mappen simulated-device i Kommandotolken.</span><span class="sxs-lookup"><span data-stu-id="f5b54-121">At your command prompt, navigate to the simulated-device folder.</span></span>

1. <span data-ttu-id="f5b54-122">Använd en textredigerare och öppna filen pom.xml i mappen simulated-device och lägg till följande beroenden till noden **dependencies**.</span><span class="sxs-lookup"><span data-stu-id="f5b54-122">Using a text editor, open the pom.xml file in the simulated-device folder and add the following dependencies to the **dependencies** node.</span></span> <span data-ttu-id="f5b54-123">Det här beroendet kan du använda iot-enhet-klientpaketet i appen kommunicerar med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="f5b54-123">This dependency enables you to use the iot-device-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="f5b54-124">Du kan söka efter den senaste versionen av **iot-device-client** med [Maven-sökning][lnk-maven-device-search].</span><span class="sxs-lookup"><span data-stu-id="f5b54-124">You can check for the latest version of **iot-device-client** using [Maven search][lnk-maven-device-search].</span></span>

1. <span data-ttu-id="f5b54-125">Lägg till följande **skapa** noden efter den **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="f5b54-125">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="f5b54-126">Den här konfigurationen instruerar Maven använda Java 1.8 för att skapa appen:</span><span class="sxs-lookup"><span data-stu-id="f5b54-126">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="f5b54-127">Spara och stäng filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="f5b54-127">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="f5b54-128">Öppna filen simulated-device\src\main\java\com\mycompany\app\App.java i en textredigerare.</span><span class="sxs-lookup"><span data-stu-id="f5b54-128">Using a text editor, open the simulated-device\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="f5b54-129">Lägg till följande **Import**-instruktioner i filen:</span><span class="sxs-lookup"><span data-stu-id="f5b54-129">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="f5b54-130">Lägg till följande variabler på klassnivå till klassen **App**.</span><span class="sxs-lookup"><span data-stu-id="f5b54-130">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="f5b54-131">Ersätta `{youriothubname}` med din IoT-hubbnamnet och `{yourdevicekey}` med nyckeln för enheten värde som du genererade i den *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="f5b54-131">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with the device key value you generated in the *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static String deviceId = "myDeviceId";
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="f5b54-132">Den här exempelappen använder variabeln **protocol** när den instantierar ett **DeviceClient**-objekt.</span><span class="sxs-lookup"><span data-stu-id="f5b54-132">This sample app uses the **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="f5b54-133">För närvarande för att använda direct-metoder måste du använda MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f5b54-133">Currently, to use direct methods you must use the MQTT protocol.</span></span>

1. <span data-ttu-id="f5b54-134">Lägg till följande kapslade klassen för att returnera en statuskod för din IoT-hubb i **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f5b54-134">To return a status code to your IoT hub, add the following nested class to the **App** class:</span></span>

    ```java
    protected static class DirectMethodStatusCallback implements IotHubEventCallback
    {
      public void execute(IotHubStatusCode status, Object context)
      {
        System.out.println("IoT Hub responded to device method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="f5b54-135">Lägg till följande kapslade klassen för att hantera direkta metoden anrop från lösningens serverdel den **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f5b54-135">To handle the direct method invocations from the solution back end, add the following nested class to the **App** class:</span></span>

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

1. <span data-ttu-id="f5b54-136">Så här skapar du en **DeviceClient** och lyssna efter direkta metoden anrop, lägga till en **huvudsakliga** metod för att den **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f5b54-136">To create a **DeviceClient** and listen for direct method invocations, add a **main** method to the **App** class:</span></span>

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
        System.out.println("Subscribed to direct methods. Waiting...");
      }
      catch (Exception e)
      {
        System.out.println("On exception, shutting down \n" + " Cause: " + e.getCause() + " \n" +  e.getMessage());
        client.close();
        System.out.println("Shutting down...");
      }

      System.out.println("Press any key to exit...");
      Scanner scanner = new Scanner(System.in);
      scanner.nextLine();
      scanner.close();
      client.close();
      System.out.println("Shutting down...");
    }
    ```

1. <span data-ttu-id="f5b54-137">Spara och stäng filen simulated-device\src\main\java\com\mycompany\app\App.java</span><span class="sxs-lookup"><span data-stu-id="f5b54-137">Save and close the simulated-device\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="f5b54-138">Skapa den **simulerade enheten** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="f5b54-138">Build the **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="f5b54-139">Navigera till mappen simulerade enheten och kör följande kommando vid en kommandotolk:</span><span class="sxs-lookup"><span data-stu-id="f5b54-139">At your command prompt, navigate to the simulated-device folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="call-a-direct-method-on-a-device"></a><span data-ttu-id="f5b54-140">Anropa en metod som direkt på en enhet</span><span class="sxs-lookup"><span data-stu-id="f5b54-140">Call a direct method on a device</span></span>

<span data-ttu-id="f5b54-141">I det här avsnittet skapar du en Java-konsolapp som anropar en direkt metod och visar sedan svaret.</span><span class="sxs-lookup"><span data-stu-id="f5b54-141">In this section, you create a Java console app that invokes a direct method and then displays the response.</span></span> <span data-ttu-id="f5b54-142">Den här konsolen appen ansluter till din IoT-hubb för att anropa metoden direkt.</span><span class="sxs-lookup"><span data-stu-id="f5b54-142">This console app connects to your IoT Hub to invoke the direct method.</span></span>

1. <span data-ttu-id="f5b54-143">I mappen iot-java-direct-metoden skapar du ett Maven-projekt som kallas **invoke-direct-metoden** med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f5b54-143">In the iot-java-direct-method folder, create a Maven project called **invoke-direct-method** using the following command at your command prompt.</span></span> <span data-ttu-id="f5b54-144">Följande kommando är ett långt kommando:</span><span class="sxs-lookup"><span data-stu-id="f5b54-144">The following command is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=invoke-direct-method -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="f5b54-145">Navigera till mappen invoke-direct-metod i en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f5b54-145">At your command prompt, navigate to the invoke-direct-method folder.</span></span>

1. <span data-ttu-id="f5b54-146">Med en textredigerare, öppna filen pom.xml i mappen invoke-direct-metoden och Lägg till följande beroende på den **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="f5b54-146">Using a text editor, open the pom.xml file in the invoke-direct-method folder and add the following dependency to the **dependencies** node.</span></span> <span data-ttu-id="f5b54-147">Detta beroende kan du använda iot-service-klient-paketet i appen kommunicerar med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="f5b54-147">This dependency enables you to use the iot-service-client package in your app to communicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="f5b54-148">Du kan söka efter den senaste versionen av **iot-service-client** med [Maven-sökning][lnk-maven-service-search].</span><span class="sxs-lookup"><span data-stu-id="f5b54-148">You can check for the latest version of **iot-service-client** using [Maven search][lnk-maven-service-search].</span></span>

1. <span data-ttu-id="f5b54-149">Lägg till följande **skapa** noden efter den **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="f5b54-149">Add the following **build** node after the **dependencies** node.</span></span> <span data-ttu-id="f5b54-150">Den här konfigurationen instruerar Maven använda Java 1.8 för att skapa appen:</span><span class="sxs-lookup"><span data-stu-id="f5b54-150">This configuration instructs Maven to use Java 1.8 to build the app:</span></span>

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

1. <span data-ttu-id="f5b54-151">Spara och stäng filen pom.xml.</span><span class="sxs-lookup"><span data-stu-id="f5b54-151">Save and close the pom.xml file.</span></span>

1. <span data-ttu-id="f5b54-152">Använd en textredigerare och öppna filen invoke-direct-method\src\main\java\com\mycompany\app\App.java.</span><span class="sxs-lookup"><span data-stu-id="f5b54-152">Using a text editor, open the invoke-direct-method\src\main\java\com\mycompany\app\App.java file.</span></span>

1. <span data-ttu-id="f5b54-153">Lägg till följande **Import**-instruktioner i filen:</span><span class="sxs-lookup"><span data-stu-id="f5b54-153">Add the following **import** statements to the file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceMethod;
    import com.microsoft.azure.sdk.iot.service.devicetwin.MethodResult;
    import com.microsoft.azure.sdk.iot.service.exceptions.IotHubException;

    import java.io.IOException;
    import java.util.concurrent.TimeUnit;
    ```

1. <span data-ttu-id="f5b54-154">Lägg till följande variabler på klassnivå till klassen **App**.</span><span class="sxs-lookup"><span data-stu-id="f5b54-154">Add the following class-level variables to the **App** class.</span></span> <span data-ttu-id="f5b54-155">Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i den *skapar en IoT-hubb* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="f5b54-155">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in the *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    public static final String methodName = "writeLine";
    public static final Long responseTimeout = TimeUnit.SECONDS.toSeconds(30);
    public static final Long connectTimeout = TimeUnit.SECONDS.toSeconds(5);
    public static final String payload = "a line to be written";
    ```

1. <span data-ttu-id="f5b54-156">Lägg till följande kod för att anropa metoden på den simulerade enheten den **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="f5b54-156">To invoke the method on the simulated device, add the following code to the **main** method:</span></span>

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

1. <span data-ttu-id="f5b54-157">Spara och stäng filen invoke-direct-method\src\main\java\com\mycompany\app\App.java</span><span class="sxs-lookup"><span data-stu-id="f5b54-157">Save and close the invoke-direct-method\src\main\java\com\mycompany\app\App.java file</span></span>

1. <span data-ttu-id="f5b54-158">Skapa den **invoke-direct-method** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="f5b54-158">Build the **invoke-direct-method** app and correct any errors.</span></span> <span data-ttu-id="f5b54-159">Navigera till mappen invoke-direct-metod i en kommandotolk och kör följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f5b54-159">At your command prompt, navigate to the invoke-direct-method folder and run the following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-the-apps"></a><span data-ttu-id="f5b54-160">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="f5b54-160">Run the apps</span></span>

<span data-ttu-id="f5b54-161">Du är nu redo att köra konsolappar.</span><span class="sxs-lookup"><span data-stu-id="f5b54-161">You are now ready to run the console apps.</span></span>

1. <span data-ttu-id="f5b54-162">Kör följande kommando för att börja lyssna efter metodanrop från din IoT-hubb i en kommandotolk i mappen simulerade enhet:</span><span class="sxs-lookup"><span data-stu-id="f5b54-162">At a command prompt in the simulated-device folder, run the following command to begin listening for method calls from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb simulerade enhetsapp att lyssna efter direkt metodanrop][8]

1. <span data-ttu-id="f5b54-164">Kör följande kommando för att anropa en metod för den simulerade enheten från din IoT-hubb i en kommandotolk i mappen invoke-direct-metoden:</span><span class="sxs-lookup"><span data-stu-id="f5b54-164">At a command prompt in the invoke-direct-method folder, run the following command to call a method on your simulated device from your IoT hub:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Java IoT-hubb service-appen för att anropa en direkt metod][7]

1. <span data-ttu-id="f5b54-166">Den simulerade enheten svarar på metodanropet direkt:</span><span class="sxs-lookup"><span data-stu-id="f5b54-166">The simulated device responds to the direct method call:</span></span>

    ![Java IoT-hubb simulerade enhetsapp besvarar metodanropet direkt][9]

## <a name="next-steps"></a><span data-ttu-id="f5b54-168">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f5b54-168">Next steps</span></span>

<span data-ttu-id="f5b54-169">I den här självstudiekursen konfigurerade du en ny IoT Hub på Azure Portal och skapade sedan en enhetsidentitet i IoT-hubbens identitetsregister.</span><span class="sxs-lookup"><span data-stu-id="f5b54-169">In this tutorial, you configured a new IoT hub in the Azure portal, and then created a device identity in the IoT hub's identity registry.</span></span> <span data-ttu-id="f5b54-170">Denna enhetsidentitet använde för att aktivera appen simulerade enheten att ta hänsyn till metoder som anropas av molnet.</span><span class="sxs-lookup"><span data-stu-id="f5b54-170">You used this device identity to enable the simulated device app to react to methods invoked by the cloud.</span></span> <span data-ttu-id="f5b54-171">Du kan också skapat en app som anropar metoder på enheten och visar svaret från enheten.</span><span class="sxs-lookup"><span data-stu-id="f5b54-171">You also created an app that invokes methods on the device and displays the response from the device.</span></span>

<span data-ttu-id="f5b54-172">Om du vill utforska andra IoT-scenarier finns [schemalägga jobb på flera enheter][lnk-devguide-jobs].</span><span class="sxs-lookup"><span data-stu-id="f5b54-172">To explore other IoT scenarios, see [Schedule jobs on multiple devices][lnk-devguide-jobs].</span></span>

<span data-ttu-id="f5b54-173">Information om hur du utökar din IoT-lösningen och schema metodanrop på flera enheter finns i [schema och broadcast jobb] [ lnk-tutorial-jobs] kursen.</span><span class="sxs-lookup"><span data-stu-id="f5b54-173">To learn how to extend your IoT solution and schedule method calls on multiple devices, see the [Schedule and broadcast jobs][lnk-tutorial-jobs] tutorial.</span></span>

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
