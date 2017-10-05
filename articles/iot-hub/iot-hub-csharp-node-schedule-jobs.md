---
title: "Schemalägga med Azure IoT Hub (.NET/nod) | Microsoft Docs"
description: "Så här schemalägger du ett Azure IoT Hub-jobb att anropa en direkt metod på flera enheter. Du kan använda Azure IoT-enhet SDK för Node.js för att implementera apparna simulerade enheten och tjänsten Azure IoT SDK för .NET att implementera ett service-appen att köra jobbet."
services: iot-hub
documentationcenter: .net
author: juanjperez
manager: timlt
editor: 
ms.assetid: 2233356e-b005-4765-ae41-3a4872bda943
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/10/2017
ms.author: juanpere
ms.openlocfilehash: a8f4f34aa99c4a9966957cac213ec9170de80a46
ms.sourcegitcommit: 02e69c4a9d17645633357fe3d46677c2ff22c85a
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/03/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a><span data-ttu-id="98e05-104">Schemat och sändning jobb (.NET/Node.js)</span><span class="sxs-lookup"><span data-stu-id="98e05-104">Schedule and broadcast jobs (.NET/Node.js)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="98e05-105">Använd Azure IoT-hubb för att schemalägga och spåra jobb som uppdaterar miljoner enheter.</span><span class="sxs-lookup"><span data-stu-id="98e05-105">Use Azure IoT Hub to schedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="98e05-106">Använd jobb till:</span><span class="sxs-lookup"><span data-stu-id="98e05-106">Use jobs to:</span></span>

* <span data-ttu-id="98e05-107">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="98e05-107">Update desired properties</span></span>
* <span data-ttu-id="98e05-108">Uppdatera taggar</span><span class="sxs-lookup"><span data-stu-id="98e05-108">Update tags</span></span>
* <span data-ttu-id="98e05-109">Anropa direkt metoder</span><span class="sxs-lookup"><span data-stu-id="98e05-109">Invoke direct methods</span></span>

<span data-ttu-id="98e05-110">Ett jobb radbryts i någon av följande åtgärder och spårar körning mot en uppsättning enheter som definieras av en enhet dubbla fråga.</span><span class="sxs-lookup"><span data-stu-id="98e05-110">A job wraps one of these actions and tracks the execution against a set of devices that is defined by a device twin query.</span></span> <span data-ttu-id="98e05-111">En backend-app kan till exempel använda ett jobb för att anropa en direkt metod i 10 000 enheter som startar om enheterna.</span><span class="sxs-lookup"><span data-stu-id="98e05-111">For example, a back-end app can use a job to invoke a direct method on 10,000 devices that reboots the devices.</span></span> <span data-ttu-id="98e05-112">Anger vilken uppsättning av enheter med en enhet dubbla fråga och schemalägga jobbet ska köras vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="98e05-112">You specify the set of devices with a device twin query and schedule the job to run at a future time.</span></span> <span data-ttu-id="98e05-113">Spårar jobbförloppet som var och en av enheterna som tar emot och köra den direkta metoden omstart.</span><span class="sxs-lookup"><span data-stu-id="98e05-113">The job tracks progress as each of the devices receive and execute the reboot direct method.</span></span>

<span data-ttu-id="98e05-114">Mer information om var och en av dessa funktioner finns:</span><span class="sxs-lookup"><span data-stu-id="98e05-114">To learn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="98e05-115">Enheten dubbla och egenskaper: [Kom igång med enheten twins] [ lnk-get-started-twin] och [Självstudier: hur du använder identiska enhetsegenskaper][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="98e05-115">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How to use device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="98e05-116">Dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder] [ lnk-dev-methods] och [kursen: direkta metoder][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="98e05-116">Direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: Use direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="98e05-117">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="98e05-117">This tutorial shows you how to:</span></span>

* <span data-ttu-id="98e05-118">Skapa en enhetsapp som implementerar en direkt metod som kallas **lockDoor** som kan anropas av backend-app.</span><span class="sxs-lookup"><span data-stu-id="98e05-118">Create a device app that implements a direct method called **lockDoor** that can be called by the back-end app.</span></span> <span data-ttu-id="98e05-119">Enheten appen tar också emot önskade egenskapsändringar från backend-app.</span><span class="sxs-lookup"><span data-stu-id="98e05-119">The device app also receives desired property changes from the back-end app.</span></span>
* <span data-ttu-id="98e05-120">Skapa en backend-app som skapar ett jobb för att anropa den **lockDoor** direkt metod på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="98e05-120">Create a back-end app that creates a job to call the **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="98e05-121">Ett annat jobb skickar önskade egenskapen uppdateringar till flera enheter.</span><span class="sxs-lookup"><span data-stu-id="98e05-121">Another job sends desired property updates to multiple devices.</span></span>

<span data-ttu-id="98e05-122">I slutet av den här kursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:</span><span class="sxs-lookup"><span data-stu-id="98e05-122">At the end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="98e05-123">**simDevice.js** som ansluter till din IoT-hubb implementerar den **lockDoor** direkt metod och hanterar behövs egenskapsändringar.</span><span class="sxs-lookup"><span data-stu-id="98e05-123">**simDevice.js** that connects to your IoT hub, implements the **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="98e05-124">**ScheduleJob** som använder jobb för att anropa den **lockDoor** direkt metod och uppdatera dubbla önskad enhetsegenskaper på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="98e05-124">**ScheduleJob** that uses jobs to call the **lockDoor** direct method and update the device twin desired properties on multiple devices.</span></span>

<span data-ttu-id="98e05-125">För att kunna genomföra den här kursen behöver du följande:</span><span class="sxs-lookup"><span data-stu-id="98e05-125">To complete this tutorial, you need the following:</span></span>

* <span data-ttu-id="98e05-126">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="98e05-126">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="98e05-127">Node.js version 0.12.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="98e05-127">Node.js version 0.12.x or later.</span></span> <span data-ttu-id="98e05-128">Artikeln [förbereda din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur du installerar Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="98e05-128">The article [Prepare your development environment][lnk-dev-setup] describes how to install Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="98e05-129">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="98e05-129">An active Azure account.</span></span> <span data-ttu-id="98e05-130">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="98e05-130">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a><span data-ttu-id="98e05-131">Schemaläggning för direkta metoden och skicka dubbla uppdateringar</span><span class="sxs-lookup"><span data-stu-id="98e05-131">Schedule jobs for calling a direct method and sending device twin updates</span></span>

<span data-ttu-id="98e05-132">I det här avsnittet skapar du en .NET-konsolapp (med C#) som använder jobb för att anropa den **lockDoor** direkt metod och skicka önskade egenskapen uppdateringar till flera enheter.</span><span class="sxs-lookup"><span data-stu-id="98e05-132">In this section, you create a .NET console app (using C#) that uses jobs to call the **lockDoor** direct method and send desired property updates to multiple devices.</span></span>

1. <span data-ttu-id="98e05-133">I Visual Studio lägger du till ett Visual C# Classic Desktop-projekt i den aktuella lösningen med hjälp av projektmallen **Konsolprogram**.</span><span class="sxs-lookup"><span data-stu-id="98e05-133">In Visual Studio, add a Visual C# Windows Classic Desktop project to the current solution by using the **Console Application** project template.</span></span> <span data-ttu-id="98e05-134">Namnge projektet **ScheduleJob**.</span><span class="sxs-lookup"><span data-stu-id="98e05-134">Name the project **ScheduleJob**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

1. <span data-ttu-id="98e05-136">I Solution Explorer högerklickar du på den **ScheduleJob** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="98e05-136">In Solution Explorer, right-click the **ScheduleJob** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="98e05-137">Välj **Bläddra** i fönstret för **NuGet-pakethanteraren**, leta upp **microsoft.azure.devices**, välj **Installera** för att installera **Microsoft.Azure.Devices**-paketet och godkänn användningsvillkoren.</span><span class="sxs-lookup"><span data-stu-id="98e05-137">In the **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** to install the **Microsoft.Azure.Devices** package, and accept the terms of use.</span></span> <span data-ttu-id="98e05-138">Det här steget hämtar, installerar och lägger till en referens till den [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="98e05-138">This step downloads, installs, and adds a reference to the [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="98e05-140">Lägg till följande `using`-uttryck överst i **Program.cs**-filen:</span><span class="sxs-lookup"><span data-stu-id="98e05-140">Add the following `using` statements at the top of the **Program.cs** file:</span></span>
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. <span data-ttu-id="98e05-141">Lägg till följande `using` instruktionen om den inte redan finns i standard-instruktioner.</span><span class="sxs-lookup"><span data-stu-id="98e05-141">Add the following `using` statement if not already present in the default statements.</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="98e05-142">Lägg till följande fält i klassen **Program**.</span><span class="sxs-lookup"><span data-stu-id="98e05-142">Add the following fields to the **Program** class.</span></span> <span data-ttu-id="98e05-143">Ersätt platshållaren med IoT-hubb anslutningssträngen för hubben som du skapade i föregående avsnitt.</span><span class="sxs-lookup"><span data-stu-id="98e05-143">Replace the placeholder with the IoT Hub connection string for the hub that you created in the previous section.</span></span>

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. <span data-ttu-id="98e05-144">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="98e05-144">Add the following method to the **Program** class:</span></span>

    ```csharp
    public static async Task MonitorJob(string jobId)
    {
        JobResponse result;
        do
        {
            result = await jobClient.GetJobAsync(jobId);
            Console.WriteLine("Job Status : " + result.Status.ToString());
            Thread.Sleep(2000);
        } while ((result.Status != JobStatus.Completed) && (result.Status != JobStatus.Failed));
    }
    ```

1. <span data-ttu-id="98e05-145">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="98e05-145">Add the following method to the **Program** class:</span></span>

    ```csharp
    public static async Task StartMethodJob(string jobId)
    {
        CloudToDeviceMethod directMethod = new CloudToDeviceMethod("lockDoor", TimeSpan.FromSeconds(5), TimeSpan.FromSeconds(5));

        JobResponse result = await jobClient.ScheduleDeviceMethodAsync(jobId,
            "deviceId='myDeviceId'",
            directMethod,
            DateTime.Now,
            10);

        Console.WriteLine("Started Method Job");
    }
    ```

1. <span data-ttu-id="98e05-146">Lägg till följande metod i klassen **Program**:</span><span class="sxs-lookup"><span data-stu-id="98e05-146">Add the following method to the **Program** class:</span></span>

    ```csharp
    public static async Task StartTwinUpdateJob(string jobId)
    {
        var twin = new Twin();
        twin.Properties.Desired["Building"] = "43";
        twin.Properties.Desired["Floor"] = "3";
        twin.ETag = "*";

        JobResponse result = await jobClient.ScheduleTwinUpdateAsync(jobId,
            "deviceId='myDeviceId'",
            twin,
            DateTime.Now,
            10);

        Console.WriteLine("Started Twin Update Job");
    }
    ```

1. <span data-ttu-id="98e05-147">Slutligen lägger du till följande rader till **Main**-metoden:</span><span class="sxs-lookup"><span data-stu-id="98e05-147">Finally, add the following lines to the **Main** method:</span></span>

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER to run the next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER to exit.");
    Console.ReadLine();
    ```

1. <span data-ttu-id="98e05-148">I Solution Explorer, öppna den **ange Startprojekt...**  och kontrollera att den **åtgärd** för **ScheduleJob** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="98e05-148">In the Solution Explorer, open the **Set StartUp projects...** and make sure the **Action** for **ScheduleJob** project is **Start**.</span></span> <span data-ttu-id="98e05-149">Skapa lösningen.</span><span class="sxs-lookup"><span data-stu-id="98e05-149">Build the solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="98e05-150">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="98e05-150">Create a simulated device app</span></span>

<span data-ttu-id="98e05-151">I det här avsnittet skapar du en Node.js-konsolprogram som svarar på en direkt metod som anropas av molnet, vilket utlöser simulerade enheten startas om och använder rapporterade egenskaperna för att aktivera enheten dubbla frågor för att identifiera enheter och när de senast startas om.</span><span class="sxs-lookup"><span data-stu-id="98e05-151">In this section, you create a Node.js console app that responds to a direct method called by the cloud, which triggers a simulated device reboot and uses the reported properties to enable device twin queries to identify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="98e05-152">Skapa en ny tom mapp som kallas **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="98e05-152">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="98e05-153">I den **simDevice** mapp, skapa en package.json-fil med följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="98e05-153">In the **simDevice** folder, create a package.json file using the following command at your command prompt.</span></span>  <span data-ttu-id="98e05-154">Acceptera alla standardvärden:</span><span class="sxs-lookup"><span data-stu-id="98e05-154">Accept all the defaults:</span></span>

    ```cmd/sh
    npm init
    ```

1. <span data-ttu-id="98e05-155">Vid en kommandotolk i den **simDevice** mapp, kör följande kommando för att installera den **azure iot-enhet** och **azure-iot-enhet – mqtt** paket:</span><span class="sxs-lookup"><span data-stu-id="98e05-155">At your command prompt in the **simDevice** folder, run the following command to install the **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="98e05-156">Med hjälp av en textredigerare, skapa en ny **simDevice.js** filen i den **simDevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="98e05-156">Using a text editor, create a new **simDevice.js** file in the **simDevice** folder.</span></span>

1. <span data-ttu-id="98e05-157">Lägg till följande 'krävs, instruktioner i början av den **simDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="98e05-157">Add the following 'require' statements at the start of the **simDevice.js** file:</span></span>

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. <span data-ttu-id="98e05-158">Lägg till en **connectionString**-variabel och använd den för att skapa en **klientinstans**.</span><span class="sxs-lookup"><span data-stu-id="98e05-158">Add a **connectionString** variable and use it to create a **Client** instance.</span></span> <span data-ttu-id="98e05-159">Se till att ersätta platshållarna med värden som är lämpliga för din installation.</span><span class="sxs-lookup"><span data-stu-id="98e05-159">Make sure to replace the placeholders with values appropriate to your setup.</span></span>

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="98e05-160">Lägg till följande funktion att hantera den **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="98e05-160">Add the following function to handle the **lockDoor** method.</span></span>

    ```nodejs
    var onLockDoor = function(request, response) {
   
        // Respond the cloud app for the direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response to method \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

1. <span data-ttu-id="98e05-161">Lägg till följande kod för att registrera hanterare för den **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="98e05-161">Add the following code to register the handler for the **lockDoor** method.</span></span>

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect to IotHub client.');
        }  else {
            console.log('Client connected to IoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. <span data-ttu-id="98e05-162">Spara och Stäng den **simDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="98e05-162">Save and close the **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="98e05-163">För att göra det så enkelt som möjligt implementerar vi ingen princip för omförsök i den här självstudiekursen.</span><span class="sxs-lookup"><span data-stu-id="98e05-163">To keep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="98e05-164">I produktionskoden bör du implementera principer för omförsök (till exempel en exponentiell backoff), vilket rekommenderas i MSDN-artikeln om [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="98e05-164">In production code, you should implement retry policies (such as an exponential backoff), as suggested in the MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-the-apps"></a><span data-ttu-id="98e05-165">Kör apparna</span><span class="sxs-lookup"><span data-stu-id="98e05-165">Run the apps</span></span>

<span data-ttu-id="98e05-166">Nu är det dags att köra apparna.</span><span class="sxs-lookup"><span data-stu-id="98e05-166">You are now ready to run the apps.</span></span>

1. <span data-ttu-id="98e05-167">I Kommandotolken i den **simDevice** mapp, kör följande kommando för att börja lyssna efter omstart direkta metoden.</span><span class="sxs-lookup"><span data-stu-id="98e05-167">At the command prompt in the **simDevice** folder, run the following command to begin listening for the reboot direct method.</span></span>

    ```cmd/sh
    node simDevice.js
    ```

1. <span data-ttu-id="98e05-168">Kör C#-konsolapp **ScheduleJob** genom att högerklicka på den **ScheduleJob** projekt, sedan välja **felsöka** och **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="98e05-168">Run the C# console app **ScheduleJob** by right-clicking on the **ScheduleJob** project, then selecting **Debug** and **Start new instance**.</span></span>

1. <span data-ttu-id="98e05-169">Du kan se utdata från både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="98e05-169">You see the output from both device and back-end apps.</span></span>

    ![Köra appar för att schemalägga jobb][img-schedulejobs]

## <a name="next-steps"></a><span data-ttu-id="98e05-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="98e05-171">Next steps</span></span>

<span data-ttu-id="98e05-172">I den här kursen används ett jobb för att schemalägga en direkt metod för att en enhet och enheten-dubbla egenskaper att uppdatera.</span><span class="sxs-lookup"><span data-stu-id="98e05-172">In this tutorial, you used a job to schedule a direct method to a device and the update of the device twin's properties.</span></span>

<span data-ttu-id="98e05-173">Om du vill fortsätta komma igång med IoT-hubb och device management mönster, till exempel remote via luften firmware-uppdatering, läsa [Självstudier: hur du gör en firmware-uppdatering][lnk-fwupdate].</span><span class="sxs-lookup"><span data-stu-id="98e05-173">To continue getting started with IoT Hub and device management patterns such as remote over the air firmware update, read [Tutorial: How to do a firmware update][lnk-fwupdate].</span></span>

<span data-ttu-id="98e05-174">Om du vill fortsätta komma igång med IoT-hubb finns [komma igång med IoT kant][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="98e05-174">To continue getting started with IoT Hub, see [Getting started with IoT Edge][lnk-iot-edge].</span></span>

<!-- images -->
[img-servicenuget]: media/iot-hub-csharp-node-schedule-jobs/servicesdknuget.png
[img-createapp]: media/iot-hub-csharp-node-schedule-jobs/createnetapp.png
[img-schedulejobs]: media/iot-hub-csharp-node-schedule-jobs/schedulejobs.png

[lnk-get-started-twin]: iot-hub-node-node-twin-getstarted.md
[lnk-twin-props]: iot-hub-node-node-twin-how-to-configure.md
[lnk-c2d-methods]: iot-hub-node-node-direct-methods.md
[lnk-dev-methods]: iot-hub-devguide-direct-methods.md
[lnk-fwupdate]: iot-hub-node-node-firmware-update.md
[lnk-iot-edge]: iot-hub-linux-iot-edge-get-started.md
[lnk-dev-setup]: https://github.com/Azure/azure-iot-sdk-node/blob/master/doc/node-devbox-setup.md
[lnk-free-trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-transient-faults]: https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx
[lnk-nuget-service-sdk]: https://www.nuget.org/packages/Microsoft.Azure.Devices/
