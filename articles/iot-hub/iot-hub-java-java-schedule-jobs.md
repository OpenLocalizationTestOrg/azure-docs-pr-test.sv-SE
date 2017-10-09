---
title: aaaSchedule jobb med Azure IoT Hub (Java) | Microsoft Docs
description: "Hur tooschedule en Azure IoT-hubb jobb tooinvoke direkt metod och ange en önskad egenskap på flera enheter. Du kan använda hello Azure IoT-enhet SDK för Java tooimplement hello simulerade enhetsappar och hello Azure IoT-tjänsten SDK för Java tooimplement ett app toorun hello jobb."
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
ms.date: 07/10/2017
ms.author: dobett
ms.openlocfilehash: b1b05fa56c3ce96af0b33d4cca0dd54da0f4e927
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-java"></a><span data-ttu-id="f8d9d-104">Schemat och sändning jobb (Java)</span><span class="sxs-lookup"><span data-stu-id="f8d9d-104">Schedule and broadcast jobs (Java)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="f8d9d-105">Använd Azure IoT Hub tooschedule och spåra jobb som uppdaterar miljoner enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-105">Use Azure IoT Hub tooschedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="f8d9d-106">Använd jobb till:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-106">Use jobs to:</span></span>

* <span data-ttu-id="f8d9d-107">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="f8d9d-107">Update desired properties</span></span>
* <span data-ttu-id="f8d9d-108">Uppdatera taggar</span><span class="sxs-lookup"><span data-stu-id="f8d9d-108">Update tags</span></span>
* <span data-ttu-id="f8d9d-109">Anropa direkt metoder</span><span class="sxs-lookup"><span data-stu-id="f8d9d-109">Invoke direct methods</span></span>

<span data-ttu-id="f8d9d-110">Ett jobb radbryts någon av följande åtgärder och spårar hello körning mot en uppsättning enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-110">A job wraps one of these actions and tracks hello execution against a set of devices.</span></span> <span data-ttu-id="f8d9d-111">En enhet dubbla fråga definierar hello enheter hello jobbet körs mot.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-111">A device twin query defines hello set of devices hello job executes against.</span></span> <span data-ttu-id="f8d9d-112">En backend-app kan till exempel använda en jobbet tooinvoke direkt metod på 10 000 enheter som startar om hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-112">For example, a back-end app can use a job tooinvoke a direct method on 10,000 devices that reboots hello devices.</span></span> <span data-ttu-id="f8d9d-113">Du anger hello uppsättning av enheter med en enhet dubbla fråga och schemalägga hello jobbet toorun vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-113">You specify hello set of devices with a device twin query and schedule hello job toorun at a future time.</span></span> <span data-ttu-id="f8d9d-114">hello spårar jobbförloppet som var och en av hello enheter tar emot och kör hello omstart direkt metod.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-114">hello job tracks progress as each of hello devices receive and execute hello reboot direct method.</span></span>

<span data-ttu-id="f8d9d-115">toolearn mer information om var och en av dessa funktioner finns:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-115">toolearn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="f8d9d-116">Enheten dubbla och egenskaper: [Kom igång med enheten twins](iot-hub-java-java-twin-getstarted.md)</span><span class="sxs-lookup"><span data-stu-id="f8d9d-116">Device twin and properties: [Get started with device twins](iot-hub-java-java-twin-getstarted.md)</span></span>
* <span data-ttu-id="f8d9d-117">Dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder](iot-hub-devguide-direct-methods.md) och [kursen: direkta metoder](iot-hub-java-java-direct-methods.md)</span><span class="sxs-lookup"><span data-stu-id="f8d9d-117">Direct methods: [IoT Hub developer guide - direct methods](iot-hub-devguide-direct-methods.md) and [Tutorial: Use direct methods](iot-hub-java-java-direct-methods.md)</span></span>

<span data-ttu-id="f8d9d-118">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-118">This tutorial shows you how to:</span></span>

* <span data-ttu-id="f8d9d-119">Skapa en enhetsapp som implementerar en direkt metod som kallas **lockDoor**.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-119">Create a device app that implements a direct method called **lockDoor**.</span></span> <span data-ttu-id="f8d9d-120">Hej enhetsapp tar också emot önskade egenskapsändringar från hello backend-app.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-120">hello device app also receives desired property changes from hello back-end app.</span></span>
* <span data-ttu-id="f8d9d-121">Skapa en backend-app som skapar ett jobb toocall hello **lockDoor** direkt metod på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-121">Create a back-end app that creates a job toocall hello **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="f8d9d-122">Ett annat jobb skickar önskade egenskapen uppdaterar toomultiple enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-122">Another job sends desired property updates toomultiple devices.</span></span>

<span data-ttu-id="f8d9d-123">Hello slutet av den här självstudiekursen har du en enhet för java konsolapp och en java-backend-konsolapp:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-123">At hello end of this tutorial, you have a java console device app and a java console back-end app:</span></span>

<span data-ttu-id="f8d9d-124">**simulerade enheten** som ansluter tooyour IoT-hubb, implementerar hello **lockDoor** direkt metod och hanterar behövs egenskapsändringar.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-124">**simulated-device** that connects tooyour IoT hub, implements hello **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="f8d9d-125">**schema-jobb** som använder jobb toocall hello **lockDoor** direkta metoden och uppdatera hello enheten dubbla önskade egenskaper på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-125">**schedule-jobs** that use jobs toocall hello **lockDoor** direct method and update hello device twin desired properties on multiple devices.</span></span>

> [!NOTE]
> <span data-ttu-id="f8d9d-126">hello artikel [Azure IoT SDK](iot-hub-devguide-sdks.md) innehåller information om hello Azure IoT-SDK: er som du kan använda toobuild både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-126">hello article [Azure IoT SDKs](iot-hub-devguide-sdks.md) provides information about hello Azure IoT SDKs that you can use toobuild both device and back-end apps.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="f8d9d-127">Krav</span><span class="sxs-lookup"><span data-stu-id="f8d9d-127">Prerequisites</span></span>

<span data-ttu-id="f8d9d-128">toocomplete den här kursen behöver du:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-128">toocomplete this tutorial, you need:</span></span>

* <span data-ttu-id="f8d9d-129">Hej senaste [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span><span class="sxs-lookup"><span data-stu-id="f8d9d-129">hello latest [Java SE Development Kit 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html)</span></span>
* [<span data-ttu-id="f8d9d-130">Maven 3</span><span class="sxs-lookup"><span data-stu-id="f8d9d-130">Maven 3</span></span>](https://maven.apache.org/install.html)
* <span data-ttu-id="f8d9d-131">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-131">An active Azure account.</span></span> <span data-ttu-id="f8d9d-132">(Om du inte har ett konto kan du skapa en [kostnadsfritt konto](http://azure.microsoft.com/pricing/free-trial/) på bara några minuter.)</span><span class="sxs-lookup"><span data-stu-id="f8d9d-132">(If you don't have an account, you can create a [free account](http://azure.microsoft.com/pricing/free-trial/) in just a couple of minutes.)</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity-portal](../../includes/iot-hub-get-started-create-device-identity-portal.md)]

<span data-ttu-id="f8d9d-133">Om du vill toocreate hello enhetsidentitet programmässigt kan läsa hello motsvarande avsnitt i hello [ansluta din enhet tooyour IoT-hubb som använder Java](iot-hub-java-java-getstarted.md#create-a-device-identity) artikel.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-133">If you prefer toocreate hello device identity programmatically, read hello corresponding section in hello [Connect your device tooyour IoT hub using Java](iot-hub-java-java-getstarted.md#create-a-device-identity) article.</span></span> <span data-ttu-id="f8d9d-134">Du kan också använda hello [iothub explorer](https://github.com/Azure/iothub-explorer) verktyget tooadd enhet tooyour IoT-hubb.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-134">You can also use hello [iothub-explorer](https://github.com/Azure/iothub-explorer) tool tooadd a device tooyour IoT hub.</span></span>

## <a name="create-hello-service-app"></a><span data-ttu-id="f8d9d-135">Skapa hello service-appen</span><span class="sxs-lookup"><span data-stu-id="f8d9d-135">Create hello service app</span></span>

<span data-ttu-id="f8d9d-136">I det här avsnittet skapar du en Java-konsolapp som använder jobb till:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-136">In this section, you create a Java console app that uses jobs to:</span></span>

* <span data-ttu-id="f8d9d-137">Anropa hello **lockDoor** direkt metod på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-137">Call hello **lockDoor** direct method on multiple devices.</span></span>
* <span data-ttu-id="f8d9d-138">Skicka egenskaper toomultiple enheter.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-138">Send desired properties toomultiple devices.</span></span>

<span data-ttu-id="f8d9d-139">toocreate hello app:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-139">toocreate hello app:</span></span>

1. <span data-ttu-id="f8d9d-140">Skapa en tom mapp med namnet på utvecklingsdatorn, `iot-java-schedule-jobs`.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-140">On your development machine, create an empty folder called `iot-java-schedule-jobs`.</span></span>

1. <span data-ttu-id="f8d9d-141">I hello `iot-java-schedule-jobs` mapp, skapa ett Maven-projekt som kallas **schema jobb** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-141">In hello `iot-java-schedule-jobs` folder, create a Maven project called **schedule-jobs** using hello following command at your command prompt.</span></span> <span data-ttu-id="f8d9d-142">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-142">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=schedule-jobs -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="f8d9d-143">Kommandotolken, gå toohello `schedule-jobs` mapp.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-143">At your command prompt, navigate toohello `schedule-jobs` folder.</span></span>

1. <span data-ttu-id="f8d9d-144">Använd en textredigerare och öppna hello `pom.xml` filen i hello `schedule-jobs` mapp och Lägg till följande beroende toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-144">Using a text editor, open hello `pom.xml` file in hello `schedule-jobs` folder and add hello following dependency toohello **dependencies** node.</span></span> <span data-ttu-id="f8d9d-145">Det här beroendet kan du toouse hello **iot-service-klient** paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-145">This dependency enables you toouse hello **iot-service-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-service-client</artifactId>
      <version>1.7.23</version>
      <type>jar</type>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="f8d9d-146">Du kan söka efter hello senaste versionen av **iot tjänstklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="f8d9d-146">You can check for hello latest version of **iot-service-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-service-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="f8d9d-147">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-147">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="f8d9d-148">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-148">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="f8d9d-149">Spara och Stäng hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-149">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="f8d9d-150">Använd en textredigerare och öppna hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-150">Using a text editor, open hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="f8d9d-151">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-151">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.service.devicetwin.DeviceTwinDevice;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Pair;
    import com.microsoft.azure.sdk.iot.service.devicetwin.Query;
    import com.microsoft.azure.sdk.iot.service.devicetwin.SqlQuery;
    import com.microsoft.azure.sdk.iot.service.jobs.JobClient;
    import com.microsoft.azure.sdk.iot.service.jobs.JobResult;
    import com.microsoft.azure.sdk.iot.service.jobs.JobStatus;

    import java.util.Date;
    import java.time.Instant;
    import java.util.HashSet;
    import java.util.Set;
    import java.util.UUID;
    ```

1. <span data-ttu-id="f8d9d-152">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-152">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="f8d9d-153">Ersätt `{youriothubconnectionstring}` med din IoT-hubb-anslutningssträng som du antecknade i hello *skapar en IoT-hubb* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-153">Replace `{youriothubconnectionstring}` with your IoT hub connection string you noted in hello *Create an IoT Hub* section:</span></span>

    ```java
    public static final String iotHubConnectionString = "{youriothubconnectionstring}";
    public static final String deviceId = "myDeviceId";

    // How long hello job is permitted toorun without
    // completing its work on hello set of devices
    private static final long maxExecutionTimeInSeconds = 30;
    ```

1. <span data-ttu-id="f8d9d-154">Lägg till följande metod toohello hello **App** klassen tooschedule ett jobb tooupdate hello **byggnad** och **våning** önskade egenskaper i hello enheten dubbla:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-154">Add hello following method toohello **App** class tooschedule a job tooupdate hello **Building** and **Floor** desired properties in hello device twin:</span></span>

    ```java
    private static JobResult scheduleJobSetDesiredProperties(JobClient jobClient, String jobId) {
      DeviceTwinDevice twin = new DeviceTwinDevice(deviceId);
      Set<Pair> desiredProperties = new HashSet<Pair>();
      desiredProperties.add(new Pair("Building", 43));
      desiredProperties.add(new Pair("Floor", 3));
      twin.setDesiredProperties(desiredProperties);
      // Optimistic concurrency control
      twin.setETag("*");

      // Schedule hello update twin job toorun now
      // against a single device
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleUpdateTwin(jobId, 
          "deviceId='" + deviceId + "'",
          twin,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling desired properties job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    }
    ```

1. <span data-ttu-id="f8d9d-155">tooschedule ett jobb toocall hello **lockDoor** metod, Lägg till följande metod toohello hello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-155">tooschedule a job toocall hello **lockDoor** method, add hello following method toohello **App** class:</span></span>

    ```java
    private static JobResult scheduleJobCallDirectMethod(JobClient jobClient, String jobId) {
      // Schedule a job now toocall hello lockDoor direct method
      // against a single device. Response and connection
      // timeouts are set too5 seconds.
      System.out.println("Schedule job " + jobId + " for device " + deviceId);
      try {
        JobResult jobResult = jobClient.scheduleDeviceMethod(jobId,
          "deviceId='" + deviceId + "'",
          "lockDoor",
          5L, 5L, null,
          new Date(),
          maxExecutionTimeInSeconds);
        return jobResult;
      } catch (Exception e) {
        System.out.println("Exception scheduling direct method job: " + jobId);
        System.out.println(e.getMessage());
        return null;
      }
    };
    ```

1. <span data-ttu-id="f8d9d-156">toomonitor ett jobb, Lägg till följande metod toohello hello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-156">toomonitor a job, add hello following method toohello **App** class:</span></span>

    ```java
    private static void monitorJob(JobClient jobClient, String jobId) {
      try {
        JobResult jobResult = jobClient.getJob(jobId);
        if(jobResult == null)
        {
          System.out.println("No JobResult for: " + jobId);
          return;
        }
        // Check hello job result until it's completed
        while(jobResult.getJobStatus() != JobStatus.completed)
        {
          Thread.sleep(100);
          jobResult = jobClient.getJob(jobId);
          System.out.println("Status " + jobResult.getJobStatus() + " for job " + jobId);
        }
        System.out.println("Final status " + jobResult.getJobStatus() + " for job " + jobId);
      } catch (Exception e) {
        System.out.println("Exception monitoring job: " + jobId);
        System.out.println(e.getMessage());
        return;
      }
    }
    ```

1. <span data-ttu-id="f8d9d-157">tooquery hello information om hello jobb som du körde Lägg till följande metod hello:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-157">tooquery for hello details of hello jobs you ran, add hello following method:</span></span>

    ```java
    private static void queryDeviceJobs(JobClient jobClient, String start) throws Exception {
      System.out.println("\nQuery device jobs since " + start);

      // Create a jobs query using hello time hello jobs started
      Query deviceJobQuery = jobClient
          .queryDeviceJob(SqlQuery.createSqlQuery("*", SqlQuery.FromType.JOBS, "devices.jobs.startTimeUtc > '" + start + "'", null).getQuery());

      // Iterate over hello list of jobs and print hello details
      while (jobClient.hasNextJob(deviceJobQuery)) {
        System.out.println(jobClient.getNextJob(deviceJobQuery));
      }
    }
    ```

1. <span data-ttu-id="f8d9d-158">Uppdatera hello **huvudsakliga** metoden signatur tooinclude hello följande `throws` sats:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-158">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws Exception
    ```

1. <span data-ttu-id="f8d9d-159">toorun och övervaka två jobb, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-159">toorun and monitor two jobs sequentially, add hello following code toohello **main** method:</span></span>

    ```java
    // Record hello start time
    String start = Instant.now().toString();

    // Create JobClient
    JobClient jobClient = JobClient.createFromConnectionString(iotHubConnectionString);
    System.out.println("JobClient created with success");

    // Schedule twin job desired properties
    // Maximum concurrent jobs is 1 for Free and S1 tiers
    String desiredPropertiesJobId = "DPCMD" + UUID.randomUUID();
    scheduleJobSetDesiredProperties(jobClient, desiredPropertiesJobId);
    monitorJob(jobClient, desiredPropertiesJobId);

    // Schedule twin job direct method
    String directMethodJobId = "DMCMD" + UUID.randomUUID();
    scheduleJobCallDirectMethod(jobClient, directMethodJobId);
    monitorJob(jobClient, directMethodJobId);

    // Run a query tooshow hello job detail
    queryDeviceJobs(jobClient, start);

    System.out.println("Shutting down schedule-jobs app");
    ```

1. <span data-ttu-id="f8d9d-160">Spara och Stäng hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` fil</span><span class="sxs-lookup"><span data-stu-id="f8d9d-160">Save and close hello `schedule-jobs\src\main\java\com\mycompany\app\App.java` file</span></span>

1. <span data-ttu-id="f8d9d-161">Skapa hello **schema jobb** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-161">Build hello **schedule-jobs** app and correct any errors.</span></span> <span data-ttu-id="f8d9d-162">Kommandotolken, gå toohello `schedule-jobs` mappen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-162">At your command prompt, navigate toohello `schedule-jobs` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="create-a-device-app"></a><span data-ttu-id="f8d9d-163">Skapa en enhetsapp</span><span class="sxs-lookup"><span data-stu-id="f8d9d-163">Create a device app</span></span>

<span data-ttu-id="f8d9d-164">I det här avsnittet skapar du en Java-konsolapp som hanterar hello önskade egenskaper som skickats från IoT-hubb och implementerar hello direkt metodanrop.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-164">In this section, you create a Java console app that handles hello desired properties sent from IoT Hub and implements hello direct method call.</span></span>

1. <span data-ttu-id="f8d9d-165">I hello `iot-java-schedule-jobs` mapp, skapa ett Maven-projekt som kallas **simulerade enheten** med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-165">In hello `iot-java-schedule-jobs` folder, create a Maven project called **simulated-device** using hello following command at your command prompt.</span></span> <span data-ttu-id="f8d9d-166">Observera att detta är ett enda långt kommando:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-166">Note this is a single, long command:</span></span>

    `mvn archetype:generate -DgroupId=com.mycompany.app -DartifactId=simulated-device -DarchetypeArtifactId=maven-archetype-quickstart -DinteractiveMode=false`

1. <span data-ttu-id="f8d9d-167">Kommandotolken, gå toohello `simulated-device` mapp.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-167">At your command prompt, navigate toohello `simulated-device` folder.</span></span>

1. <span data-ttu-id="f8d9d-168">Använd en textredigerare och öppna hello `pom.xml` filen i hello `simulated-device` mapp och Lägg till följande beroenden toohello hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-168">Using a text editor, open hello `pom.xml` file in hello `simulated-device` folder and add hello following dependencies toohello **dependencies** node.</span></span> <span data-ttu-id="f8d9d-169">Det här beroendet kan du toouse hello **iot enhetsklienten** paketet i din app toocommunicate med IoT-hubben:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-169">This dependency enables you toouse hello **iot-device-client** package in your app toocommunicate with your IoT hub:</span></span>

    ```xml
    <dependency>
      <groupId>com.microsoft.azure.sdk.iot</groupId>
      <artifactId>iot-device-client</artifactId>
      <version>1.3.32</version>
    </dependency>
    ```

    > [!NOTE]
    > <span data-ttu-id="f8d9d-170">Du kan söka efter hello senaste versionen av **iot enhetsklienten** med [Maven Sök](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span><span class="sxs-lookup"><span data-stu-id="f8d9d-170">You can check for hello latest version of **iot-device-client** using [Maven search](http://search.maven.org/#search%7Cga%7C1%7Ca%3A%22iot-device-client%22%20g%3A%22com.microsoft.azure.sdk.iot%22).</span></span>

1. <span data-ttu-id="f8d9d-171">Lägg till följande hello **skapa** nod efter hello **beroenden** nod.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-171">Add hello following **build** node after hello **dependencies** node.</span></span> <span data-ttu-id="f8d9d-172">Den här konfigurationen instruerar Maven toouse Java 1,8 toobuild hello app:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-172">This configuration instructs Maven toouse Java 1.8 toobuild hello app:</span></span>

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

1. <span data-ttu-id="f8d9d-173">Spara och Stäng hello `pom.xml` fil.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-173">Save and close hello `pom.xml` file.</span></span>

1. <span data-ttu-id="f8d9d-174">Använd en textredigerare och öppna hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-174">Using a text editor, open hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="f8d9d-175">Lägg till följande hello **importera** instruktioner toohello fil:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-175">Add hello following **import** statements toohello file:</span></span>

    ```java
    import com.microsoft.azure.sdk.iot.device.*;
    import com.microsoft.azure.sdk.iot.device.DeviceTwin.*;

    import java.io.IOException;
    import java.net.URISyntaxException;
    import java.util.Scanner;
    ```

1. <span data-ttu-id="f8d9d-176">Lägg till följande variabler klassen på toohello hello **App** klass.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-176">Add hello following class-level variables toohello **App** class.</span></span> <span data-ttu-id="f8d9d-177">Ersätta `{youriothubname}` med din IoT-hubbnamnet och `{yourdevicekey}` hello enheten nyckelvärdet du genererade i hello *skapa en enhetsidentitet* avsnitt:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-177">Replacing `{youriothubname}` with your IoT hub name, and `{yourdevicekey}` with hello device key value you generated in hello *Create a device identity* section:</span></span>

    ```java
    private static String connString = "HostName={youriothubname}.azure-devices.net;DeviceId=myDeviceID;SharedAccessKey={yourdevicekey}";
    private static IotHubClientProtocol protocol = IotHubClientProtocol.MQTT;
    private static final int METHOD_SUCCESS = 200;
    private static final int METHOD_NOT_DEFINED = 404;
    ```

    <span data-ttu-id="f8d9d-178">Den här exempelappen använder hello **protokollet** variabeln när den instantierar en **DeviceClient** objekt.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-178">This sample app uses hello **protocol** variable when it instantiates a **DeviceClient** object.</span></span> <span data-ttu-id="f8d9d-179">För närvarande toouse dubbla funktioner för enheter som du måste använda hello MQTT-protokollet.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-179">Currently, toouse device twin features you must use hello MQTT protocol.</span></span>

1. <span data-ttu-id="f8d9d-180">tooprint enhet dubbla meddelanden toohello konsolen, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-180">tooprint device twin notifications toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Handler for device twin operation notifications from IoT Hub
    protected static class DeviceTwinStatusCallBack implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodevice twin operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="f8d9d-181">tooprint direkta metoden meddelanden toohello konsolen, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-181">tooprint direct method notifications toohello console, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Handler for direct method notifications from IoT Hub
    protected static class DirectMethodStatusCallback implements IotHubEventCallback {
      public void execute(IotHubStatusCode status, Object context) {
        System.out.println("IoT Hub responded toodirect method operation with status " + status.name());
      }
    }
    ```

1. <span data-ttu-id="f8d9d-182">toohandle direkta metodanrop från IoT-hubb, Lägg till följande hello kapslad klass toohello **App** klass:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-182">toohandle direct method calls from IoT Hub, add hello following nested class toohello **App** class:</span></span>

    ```java
    // Handler for direct method calls from IoT Hub
    protected static class DirectMethodCallback
        implements DeviceMethodCallback {
      @Override
      public DeviceMethodData call(String methodName, Object methodData, Object context) {
        DeviceMethodData deviceMethodData;
        switch (methodName) {
          case "lockDoor": {
            System.out.println("Executing direct method: " + methodName);
            deviceMethodData = new DeviceMethodData(METHOD_SUCCESS, "Executed direct method " + methodName);
            break;
          }
          default: {
            deviceMethodData = new DeviceMethodData(METHOD_NOT_DEFINED, "Not defined direct method " + methodName);
          }
        }
        // Notify IoT Hub of result
        return deviceMethodData;
      }
    }
    ```

1. <span data-ttu-id="f8d9d-183">Uppdatera hello **huvudsakliga** metoden signatur tooinclude hello följande `throws` sats:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-183">Update hello **main** method signature tooinclude hello following `throws` clause:</span></span>

    ```java
    public static void main( String[] args ) throws IOException, URISyntaxException
    ```

1. <span data-ttu-id="f8d9d-184">Lägg till följande kod toohello hello **huvudsakliga** metod för att:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-184">Add hello following code toohello **main** method to:</span></span>
    * <span data-ttu-id="f8d9d-185">Skapa en enhet klienten toocommunicate med IoT-hubben.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-185">Create a device client toocommunicate with IoT Hub.</span></span>
    * <span data-ttu-id="f8d9d-186">Skapa en **enhet** objekt toostore hello dubbla Enhetsegenskaper.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-186">Create a **Device** object toostore hello device twin properties.</span></span>

    ```java
    // Create a device client
    DeviceClient client = new DeviceClient(connString, protocol);

    // An object toomanage device twin desired and reported properties
    Device dataCollector = new Device() {
      @Override
      public void PropertyCall(String propertyKey, Object propertyValue, Object context)
      {
        System.out.println("Received desired property change: " + propertyKey + " " + propertyValue);
      }
    };
    ```

1. <span data-ttu-id="f8d9d-187">klienttjänster toostart hello enhet, Lägg till följande kod toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-187">toostart hello device client services, add hello following code toohello **main** method:</span></span>

    ```java
    try {
      // Open hello DeviceClient
      // Start hello device twin services
      // Subscribe toodirect method calls
      client.open();
      client.startDeviceTwin(new DeviceTwinStatusCallBack(), null, dataCollector, null);
      client.subscribeToDeviceMethod(new DirectMethodCallback(), null, new DirectMethodStatusCallback(), null);
    } catch (Exception e) {
      System.out.println("Exception, shutting down \n" + " Cause: " + e.getCause() + " \n" + e.getMessage());
      dataCollector.clean();
      client.closeNow();
      System.out.println("Shutting down...");
    }
    ```

1. <span data-ttu-id="f8d9d-188">toowait för hello användaren toopress hello **RETUR** nyckeln innan du stänger, lägga till hello efter koden toohello hello **huvudsakliga** metoden:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-188">toowait for hello user toopress hello **Enter** key before shutting down, add hello following code toohello end of hello **main** method:</span></span>

    ```java
    // Close hello app
    System.out.println("Press any key tooexit...");
    Scanner scanner = new Scanner(System.in);
    scanner.nextLine();
    dataCollector.clean();
    client.closeNow();
    scanner.close();
    ```

1. <span data-ttu-id="f8d9d-189">Spara och Stäng hello `simulated-device\src\main\java\com\mycompany\app\App.java` fil.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-189">Save and close hello `simulated-device\src\main\java\com\mycompany\app\App.java` file.</span></span>

1. <span data-ttu-id="f8d9d-190">Skapa hello **simulerade enheten** app och korrigera eventuella fel.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-190">Build hello **simulated-device** app and correct any errors.</span></span> <span data-ttu-id="f8d9d-191">Kommandotolken, gå toohello `simulated-device` mappen och kör hello följande kommando:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-191">At your command prompt, navigate toohello `simulated-device` folder and run hello following command:</span></span>

    `mvn clean package -DskipTests`

## <a name="run-hello-apps"></a><span data-ttu-id="f8d9d-192">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="f8d9d-192">Run hello apps</span></span>

<span data-ttu-id="f8d9d-193">Du är nu redo toorun hello konsolappar.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-193">You are now ready toorun hello console apps.</span></span>

1. <span data-ttu-id="f8d9d-194">Vid en kommandotolk i hello `simulated-device` mapp, kör följande kommando toostart hello enhetsapp lyssnar efter önskad egenskapsändringar och direkt metodanrop hello:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-194">At a command prompt in hello `simulated-device` folder, run hello following command toostart hello device app listening for desired property changes and direct method calls:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Hej enhetsklienten startar](media/iot-hub-java-java-schedule-jobs/device-app-1.png)

1. <span data-ttu-id="f8d9d-196">Vid en kommandotolk i hello `schedule-jobs` mapp, kör följande kommando toorun hello hello **schema jobb** tjänsten app toorun två jobb.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-196">At a command prompt in hello `schedule-jobs` folder, run hello following command toorun hello **schedule-jobs** service app toorun two jobs.</span></span> <span data-ttu-id="f8d9d-197">hello först anger hello önskad egenskapsvärden, hello andra anrop hello direkta metoden:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-197">hello first sets hello desired property values, hello second calls hello direct method:</span></span>

    `mvn exec:java -Dexec.mainClass="com.mycompany.app.App"`

    ![Service-appen för Java IoT-hubb skapar två jobb](media/iot-hub-java-java-schedule-jobs/service-app-1.png)

1. <span data-ttu-id="f8d9d-199">Hej enhetsapp hanterar hello önskad egenskapen ändrings- och hello direkt metodanrop:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-199">hello device app handles hello desired property change and hello direct method call:</span></span>

    ![Hej enhetsklienten svarar toohello ändringar](media/iot-hub-java-java-schedule-jobs/device-app-2.png)

## <a name="next-steps"></a><span data-ttu-id="f8d9d-201">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="f8d9d-201">Next steps</span></span>

<span data-ttu-id="f8d9d-202">I den här självstudiekursen konfigurerade en ny IoT-hubb i hello Azure-portalen och sedan skapa en enhetsidentitet i hello IoT hub identitetsregistret.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-202">In this tutorial, you configured a new IoT hub in hello Azure portal, and then created a device identity in hello IoT hub's identity registry.</span></span> <span data-ttu-id="f8d9d-203">Du har skapat en backend-app toorun två jobb.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-203">You created a back-end app toorun two jobs.</span></span> <span data-ttu-id="f8d9d-204">hello första jobb ange önskade egenskapsvärden och hello andra jobb anropas direkt metod.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-204">hello first job set desired property values, and hello second job called a direct method.</span></span>

<span data-ttu-id="f8d9d-205">Använd hello följande resurser toolearn hur till:</span><span class="sxs-lookup"><span data-stu-id="f8d9d-205">Use hello following resources toolearn how to:</span></span>

* <span data-ttu-id="f8d9d-206">Skicka telemetri från enheter med hello [Kom igång med IoT-hubb](iot-hub-java-java-getstarted.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-206">Send telemetry from devices with hello [Get started with IoT Hub](iot-hub-java-java-getstarted.md) tutorial.</span></span>
* <span data-ttu-id="f8d9d-207">Kontrollera enheter interaktivt (till exempel aktivera fläktar från en användare-kontrollerade app) med hello [metoder från direkt](iot-hub-java-java-direct-methods.md) kursen.</span><span class="sxs-lookup"><span data-stu-id="f8d9d-207">Control devices interactively (such as turning on a fan from a user-controlled app) with hello [Use direct methods](iot-hub-java-java-direct-methods.md) tutorial.</span></span>
