---
title: aaaSchedule jobb med Azure IoT Hub (.NET/nod) | Microsoft Docs
description: "Hur jobbet tooschedule en Azure IoT-hubb tooinvoke direkt metod på flera enheter. Du kan använda hello Azure IoT-enhet SDK för Node.js tooimplement hello simulerade enhetsappar och hello Azure IoT-tjänsten SDK för .NET tooimplement ett app toorun hello jobb."
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
ms.openlocfilehash: f6148b67129dde4580bfe9ccceafd6400fbc5976
ms.sourcegitcommit: 523283cc1b3c37c428e77850964dc1c33742c5f0
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 10/06/2017
---
# <a name="schedule-and-broadcast-jobs-netnodejs"></a><span data-ttu-id="e04aa-104">Schemat och sändning jobb (.NET/Node.js)</span><span class="sxs-lookup"><span data-stu-id="e04aa-104">Schedule and broadcast jobs (.NET/Node.js)</span></span>

[!INCLUDE [iot-hub-selector-schedule-jobs](../../includes/iot-hub-selector-schedule-jobs.md)]

<span data-ttu-id="e04aa-105">Använd Azure IoT Hub tooschedule och spåra jobb som uppdaterar miljoner enheter.</span><span class="sxs-lookup"><span data-stu-id="e04aa-105">Use Azure IoT Hub tooschedule and track jobs that update millions of devices.</span></span> <span data-ttu-id="e04aa-106">Använd jobb till:</span><span class="sxs-lookup"><span data-stu-id="e04aa-106">Use jobs to:</span></span>

* <span data-ttu-id="e04aa-107">Uppdatera önskade egenskaper</span><span class="sxs-lookup"><span data-stu-id="e04aa-107">Update desired properties</span></span>
* <span data-ttu-id="e04aa-108">Uppdatera taggar</span><span class="sxs-lookup"><span data-stu-id="e04aa-108">Update tags</span></span>
* <span data-ttu-id="e04aa-109">Anropa direkt metoder</span><span class="sxs-lookup"><span data-stu-id="e04aa-109">Invoke direct methods</span></span>

<span data-ttu-id="e04aa-110">Ett jobb radbryts någon av följande åtgärder och spårar hello körning mot en uppsättning enheter som definieras av en enhet dubbla fråga.</span><span class="sxs-lookup"><span data-stu-id="e04aa-110">A job wraps one of these actions and tracks hello execution against a set of devices that is defined by a device twin query.</span></span> <span data-ttu-id="e04aa-111">En backend-app kan till exempel använda en jobbet tooinvoke direkt metod på 10 000 enheter som startar om hello-enheter.</span><span class="sxs-lookup"><span data-stu-id="e04aa-111">For example, a back-end app can use a job tooinvoke a direct method on 10,000 devices that reboots hello devices.</span></span> <span data-ttu-id="e04aa-112">Du anger hello uppsättning av enheter med en enhet dubbla fråga och schemalägga hello jobbet toorun vid ett senare tillfälle.</span><span class="sxs-lookup"><span data-stu-id="e04aa-112">You specify hello set of devices with a device twin query and schedule hello job toorun at a future time.</span></span> <span data-ttu-id="e04aa-113">hello spårar jobbförloppet som var och en av hello enheter tar emot och kör hello omstart direkt metod.</span><span class="sxs-lookup"><span data-stu-id="e04aa-113">hello job tracks progress as each of hello devices receive and execute hello reboot direct method.</span></span>

<span data-ttu-id="e04aa-114">toolearn mer information om var och en av dessa funktioner finns:</span><span class="sxs-lookup"><span data-stu-id="e04aa-114">toolearn more about each of these capabilities, see:</span></span>

* <span data-ttu-id="e04aa-115">Enheten dubbla och egenskaper: [Kom igång med enheten twins] [ lnk-get-started-twin] och [Självstudier: hur toouse enheten dubbla egenskaper][lnk-twin-props]</span><span class="sxs-lookup"><span data-stu-id="e04aa-115">Device twin and properties: [Get started with device twins][lnk-get-started-twin] and [Tutorial: How toouse device twin properties][lnk-twin-props]</span></span>
* <span data-ttu-id="e04aa-116">Dirigera metoder: [IoT-hubb Utvecklarhandbok - direkt metoder] [ lnk-dev-methods] och [kursen: direkta metoder][lnk-c2d-methods]</span><span class="sxs-lookup"><span data-stu-id="e04aa-116">Direct methods: [IoT Hub developer guide - direct methods][lnk-dev-methods] and [Tutorial: Use direct methods][lnk-c2d-methods]</span></span>

<span data-ttu-id="e04aa-117">I den här självstudiekursen lär du dig att:</span><span class="sxs-lookup"><span data-stu-id="e04aa-117">This tutorial shows you how to:</span></span>

* <span data-ttu-id="e04aa-118">Skapa en enhetsapp som implementerar en direkt metod som kallas **lockDoor** som kan anropas av hello backend-app.</span><span class="sxs-lookup"><span data-stu-id="e04aa-118">Create a device app that implements a direct method called **lockDoor** that can be called by hello back-end app.</span></span> <span data-ttu-id="e04aa-119">Hej enhetsapp tar också emot önskade egenskapsändringar från hello backend-app.</span><span class="sxs-lookup"><span data-stu-id="e04aa-119">hello device app also receives desired property changes from hello back-end app.</span></span>
* <span data-ttu-id="e04aa-120">Skapa en backend-app som skapar ett jobb toocall hello **lockDoor** direkt metod på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="e04aa-120">Create a back-end app that creates a job toocall hello **lockDoor** direct method on multiple devices.</span></span> <span data-ttu-id="e04aa-121">Ett annat jobb skickar önskade egenskapen uppdaterar toomultiple enheter.</span><span class="sxs-lookup"><span data-stu-id="e04aa-121">Another job sends desired property updates toomultiple devices.</span></span>

<span data-ttu-id="e04aa-122">Hello slutet av den här självstudiekursen har du en enhet för Node.js konsolapp och en serverdel för .NET (C#) konsolapp:</span><span class="sxs-lookup"><span data-stu-id="e04aa-122">At hello end of this tutorial, you have a Node.js console device app and a .NET (C#) console back-end app:</span></span>

<span data-ttu-id="e04aa-123">**simDevice.js** som ansluter tooyour IoT-hubb, implementerar hello **lockDoor** direkt metod och hanterar behövs egenskapsändringar.</span><span class="sxs-lookup"><span data-stu-id="e04aa-123">**simDevice.js** that connects tooyour IoT hub, implements hello **lockDoor** direct method, and handles desired property changes.</span></span>

<span data-ttu-id="e04aa-124">**ScheduleJob** som använder jobb toocall hello **lockDoor** direkta metoden och uppdatera hello enheten dubbla önskade egenskaper på flera enheter.</span><span class="sxs-lookup"><span data-stu-id="e04aa-124">**ScheduleJob** that uses jobs toocall hello **lockDoor** direct method and update hello device twin desired properties on multiple devices.</span></span>

<span data-ttu-id="e04aa-125">toocomplete den här kursen behöver du hello följande:</span><span class="sxs-lookup"><span data-stu-id="e04aa-125">toocomplete this tutorial, you need hello following:</span></span>

* <span data-ttu-id="e04aa-126">Visual Studio 2015 eller Visual Studio 2017.</span><span class="sxs-lookup"><span data-stu-id="e04aa-126">Visual Studio 2015 or Visual Studio 2017.</span></span>
* <span data-ttu-id="e04aa-127">Node.js version 0.12.x eller senare.</span><span class="sxs-lookup"><span data-stu-id="e04aa-127">Node.js version 0.12.x or later.</span></span> <span data-ttu-id="e04aa-128">hello artikel [förbereda din utvecklingsmiljö] [ lnk-dev-setup] beskriver hur tooinstall Node.js för den här självstudiekursen på Windows- eller Linux.</span><span class="sxs-lookup"><span data-stu-id="e04aa-128">hello article [Prepare your development environment][lnk-dev-setup] describes how tooinstall Node.js for this tutorial on either Windows or Linux.</span></span>
* <span data-ttu-id="e04aa-129">Ett aktivt Azure-konto.</span><span class="sxs-lookup"><span data-stu-id="e04aa-129">An active Azure account.</span></span> <span data-ttu-id="e04aa-130">Om du inte har något konto kan du skapa ett [kostnadsfritt konto][lnk-free-trial] på bara några minuter.</span><span class="sxs-lookup"><span data-stu-id="e04aa-130">If you don't have an account, you can create a [free account][lnk-free-trial] in just a couple of minutes.</span></span>

[!INCLUDE [iot-hub-get-started-create-hub](../../includes/iot-hub-get-started-create-hub.md)]

[!INCLUDE [iot-hub-get-started-create-device-identity](../../includes/iot-hub-get-started-create-device-identity.md)]

## <a name="schedule-jobs-for-calling-a-direct-method-and-sending-device-twin-updates"></a><span data-ttu-id="e04aa-131">Schemaläggning för direkta metoden och skicka dubbla uppdateringar</span><span class="sxs-lookup"><span data-stu-id="e04aa-131">Schedule jobs for calling a direct method and sending device twin updates</span></span>

<span data-ttu-id="e04aa-132">I det här avsnittet skapar du en .NET-konsolapp (med C#) som använder jobb toocall hello **lockDoor** direkt metod och skicka önskade egenskapen uppdaterar toomultiple enheter.</span><span class="sxs-lookup"><span data-stu-id="e04aa-132">In this section, you create a .NET console app (using C#) that uses jobs toocall hello **lockDoor** direct method and send desired property updates toomultiple devices.</span></span>

1. <span data-ttu-id="e04aa-133">I Visual Studio, lägger du till en Visual C# klassiska skrivbordet projektet toohello aktuella lösning med hjälp av hello **konsolprogram** projektmall.</span><span class="sxs-lookup"><span data-stu-id="e04aa-133">In Visual Studio, add a Visual C# Windows Classic Desktop project toohello current solution by using hello **Console Application** project template.</span></span> <span data-ttu-id="e04aa-134">Namnet hello projektet **ScheduleJob**.</span><span class="sxs-lookup"><span data-stu-id="e04aa-134">Name hello project **ScheduleJob**.</span></span>

    ![Nytt Visual C# Windows Classic Desktop-projekt][img-createapp]

1. <span data-ttu-id="e04aa-136">I Solution Explorer högerklickar du på hello **ScheduleJob** projektet och klicka sedan på **hantera NuGet-paket...** .</span><span class="sxs-lookup"><span data-stu-id="e04aa-136">In Solution Explorer, right-click hello **ScheduleJob** project, and then click **Manage NuGet Packages...**.</span></span>
1. <span data-ttu-id="e04aa-137">I hello **NuGet Package Manager** väljer **Bläddra**, söka efter **microsoft.azure.devices**väljer **installera** tooinstall Hej **Microsoft.Azure.Devices** paketet och acceptera hello villkor för användning.</span><span class="sxs-lookup"><span data-stu-id="e04aa-137">In hello **NuGet Package Manager** window, select **Browse**, search for **microsoft.azure.devices**, select **Install** tooinstall hello **Microsoft.Azure.Devices** package, and accept hello terms of use.</span></span> <span data-ttu-id="e04aa-138">Det här steget hämtar, installerar och lägger till en referens toohello [Azure IoT service SDK] [ lnk-nuget-service-sdk] NuGet-paketet och dess beroenden.</span><span class="sxs-lookup"><span data-stu-id="e04aa-138">This step downloads, installs, and adds a reference toohello [Azure IoT service SDK][lnk-nuget-service-sdk] NuGet package and its dependencies.</span></span>

    ![Fönstret för NuGet-pakethanteraren][img-servicenuget]
1. <span data-ttu-id="e04aa-140">Lägg till följande hello `using` instruktioner överst hello i hello **Program.cs** fil:</span><span class="sxs-lookup"><span data-stu-id="e04aa-140">Add hello following `using` statements at hello top of hello **Program.cs** file:</span></span>
    
    ```csharp
    using Microsoft.Azure.Devices;
    using Microsoft.Azure.Devices.Shared;
    ```

1. <span data-ttu-id="e04aa-141">Lägg till följande hello `using` instruktionen om den inte redan finns i hello standard instruktioner.</span><span class="sxs-lookup"><span data-stu-id="e04aa-141">Add hello following `using` statement if not already present in hello default statements.</span></span>

    ```csharp
    using System.Threading.Tasks;
    ```

1. <span data-ttu-id="e04aa-142">Lägg till följande fält toohello hello **programmet** klass.</span><span class="sxs-lookup"><span data-stu-id="e04aa-142">Add hello following fields toohello **Program** class.</span></span> <span data-ttu-id="e04aa-143">Ersätt platshållaren hello med hello IoT-hubb anslutningssträngen för hello-hubb som du skapade i föregående avsnitt i hello.</span><span class="sxs-lookup"><span data-stu-id="e04aa-143">Replace hello placeholder with hello IoT Hub connection string for hello hub that you created in hello previous section.</span></span>

    ```csharp
    static string connString = "{iot hub connection string}";
    static ServiceClient client;
    static JobClient jobClient;
    ```

1. <span data-ttu-id="e04aa-144">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e04aa-144">Add hello following method toohello **Program** class:</span></span>

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

1. <span data-ttu-id="e04aa-145">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e04aa-145">Add hello following method toohello **Program** class:</span></span>

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

1. <span data-ttu-id="e04aa-146">Lägg till följande metod toohello hello **programmet** klass:</span><span class="sxs-lookup"><span data-stu-id="e04aa-146">Add hello following method toohello **Program** class:</span></span>

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

1. <span data-ttu-id="e04aa-147">Slutligen lägger du till följande rader toohello hello **Main** metoden:</span><span class="sxs-lookup"><span data-stu-id="e04aa-147">Finally, add hello following lines toohello **Main** method:</span></span>

    ```csharp
    jobClient = JobClient.CreateFromConnectionString(connString);

    string methodJobId = Guid.NewGuid().ToString();

    StartMethodJob(methodJobId);
    MonitorJob(methodJobId).Wait();
    Console.WriteLine("Press ENTER toorun hello next job.");
    Console.ReadLine();

    string twinUpdateJobId = Guid.NewGuid().ToString();

    StartTwinUpdateJob(twinUpdateJobId);
    MonitorJob(twinUpdateJobId).Wait();
    Console.WriteLine("Press ENTER tooexit.");
    Console.ReadLine();
    ```

1. <span data-ttu-id="e04aa-148">Hello Solution Explorer, öppna hello **ange Startprojekt...**  och se till att hello **åtgärd** för **ScheduleJob** projektet är **starta**.</span><span class="sxs-lookup"><span data-stu-id="e04aa-148">In hello Solution Explorer, open hello **Set StartUp projects...** and make sure hello **Action** for **ScheduleJob** project is **Start**.</span></span> <span data-ttu-id="e04aa-149">Skapa hello lösning.</span><span class="sxs-lookup"><span data-stu-id="e04aa-149">Build hello solution.</span></span>

## <a name="create-a-simulated-device-app"></a><span data-ttu-id="e04aa-150">Skapa en simulerad enhetsapp</span><span class="sxs-lookup"><span data-stu-id="e04aa-150">Create a simulated device app</span></span>

<span data-ttu-id="e04aa-151">I det här avsnittet skapar du en Node.js-konsolprogram som svarar tooa direkta metoden anropas av hello moln, vilket utlöser simulerade enheten startas om och använder hello rapporterade egenskaper tooenable enheter dubbla frågor tooidentify enheter och när de senast startas om.</span><span class="sxs-lookup"><span data-stu-id="e04aa-151">In this section, you create a Node.js console app that responds tooa direct method called by hello cloud, which triggers a simulated device reboot and uses hello reported properties tooenable device twin queries tooidentify devices and when they last rebooted.</span></span>

1. <span data-ttu-id="e04aa-152">Skapa en ny tom mapp som kallas **simDevice**.</span><span class="sxs-lookup"><span data-stu-id="e04aa-152">Create a new empty folder called **simDevice**.</span></span>  <span data-ttu-id="e04aa-153">I hello **simDevice** mapp, skapa en package.json-fil med hello följande kommando vid en kommandotolk.</span><span class="sxs-lookup"><span data-stu-id="e04aa-153">In hello **simDevice** folder, create a package.json file using hello following command at your command prompt.</span></span>  <span data-ttu-id="e04aa-154">Acceptera alla standardvärden för hello:</span><span class="sxs-lookup"><span data-stu-id="e04aa-154">Accept all hello defaults:</span></span>

    ```cmd/sh
    npm init
    ```

1. <span data-ttu-id="e04aa-155">Vid en kommandotolk i hello **simDevice** mapp, kör följande kommando tooinstall hello hello **azure iot-enhet** och **azure-iot-enhet – mqtt** paket:</span><span class="sxs-lookup"><span data-stu-id="e04aa-155">At your command prompt in hello **simDevice** folder, run hello following command tooinstall hello **azure-iot-device** and **azure-iot-device-mqtt** packages:</span></span>

    ```cmd/sh
    npm install azure-iot-device azure-iot-device-mqtt --save
    ```

1. <span data-ttu-id="e04aa-156">Med hjälp av en textredigerare, skapa en ny **simDevice.js** filen i hello **simDevice** mapp.</span><span class="sxs-lookup"><span data-stu-id="e04aa-156">Using a text editor, create a new **simDevice.js** file in hello **simDevice** folder.</span></span>

1. <span data-ttu-id="e04aa-157">Lägg till följande hello 'krävs, instruktioner hello början av hello **simDevice.js** fil:</span><span class="sxs-lookup"><span data-stu-id="e04aa-157">Add hello following 'require' statements at hello start of hello **simDevice.js** file:</span></span>

    ```nodejs
    'use strict';
   
    var Client = require('azure-iot-device').Client;
    var Protocol = require('azure-iot-device-mqtt').Mqtt;
    ```

1. <span data-ttu-id="e04aa-158">Lägg till en **connectionString** variabel och använda den toocreate en **klienten** instans.</span><span class="sxs-lookup"><span data-stu-id="e04aa-158">Add a **connectionString** variable and use it toocreate a **Client** instance.</span></span> <span data-ttu-id="e04aa-159">Kontrollera att tooreplace hello platshållarna med värdena lämpliga tooyour installationen.</span><span class="sxs-lookup"><span data-stu-id="e04aa-159">Make sure tooreplace hello placeholders with values appropriate tooyour setup.</span></span>

    ```nodejs
    var connectionString = 'HostName={youriothostname};DeviceId={yourdeviceid};SharedAccessKey={yourdevicekey}';
    var client = Client.fromConnectionString(connectionString, Protocol);
    ```

1. <span data-ttu-id="e04aa-160">Lägg till följande funktion toohandle hello hello **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="e04aa-160">Add hello following function toohandle hello **lockDoor** method.</span></span>

    ```nodejs
    var onLockDoor = function(request, response) {
   
        // Respond hello cloud app for hello direct method
        response.send(200, function(err) {
            if (!err) {
                console.error('An error occured when sending a method response:\n' + err.toString());
            } else {
                console.log('Response toomethod \'' + request.methodName + '\' sent successfully.');
            }
        });
   
        console.log('Locking Door!');
    };
    ```

1. <span data-ttu-id="e04aa-161">Lägg till följande kod tooregister hello hanterare för hello hello **lockDoor** metod.</span><span class="sxs-lookup"><span data-stu-id="e04aa-161">Add hello following code tooregister hello handler for hello **lockDoor** method.</span></span>

    ```nodejs
    client.open(function(err) {
        if (err) {
            console.error('Could not connect tooIotHub client.');
        }  else {
            console.log('Client connected tooIoT Hub.  Waiting for lockDoor direct method.');
            client.onDeviceMethod('lockDoor', onLockDoor);
        }
    });
    ```

1. <span data-ttu-id="e04aa-162">Spara och Stäng hello **simDevice.js** fil.</span><span class="sxs-lookup"><span data-stu-id="e04aa-162">Save and close hello **simDevice.js** file.</span></span>

> [!NOTE]
> <span data-ttu-id="e04aa-163">enkel tookeep saker, den här självstudiekursen implementerar inte några återförsöksprincip.</span><span class="sxs-lookup"><span data-stu-id="e04aa-163">tookeep things simple, this tutorial does not implement any retry policy.</span></span> <span data-ttu-id="e04aa-164">I produktionskod, bör du implementera försök principer (till exempel en exponentiell backoff) enligt förslaget i hello MSDN-artikel [hantering av tillfälliga fel][lnk-transient-faults].</span><span class="sxs-lookup"><span data-stu-id="e04aa-164">In production code, you should implement retry policies (such as an exponential backoff), as suggested in hello MSDN article [Transient Fault Handling][lnk-transient-faults].</span></span>

## <a name="run-hello-apps"></a><span data-ttu-id="e04aa-165">Köra hello appar</span><span class="sxs-lookup"><span data-stu-id="e04aa-165">Run hello apps</span></span>

<span data-ttu-id="e04aa-166">Du är nu redo toorun hello appar.</span><span class="sxs-lookup"><span data-stu-id="e04aa-166">You are now ready toorun hello apps.</span></span>

1. <span data-ttu-id="e04aa-167">Kommandotolken hello i hello **simDevice** mapp, kör följande kommando toobegin lyssnar efter hello omstart direkta metoden hello.</span><span class="sxs-lookup"><span data-stu-id="e04aa-167">At hello command prompt in hello **simDevice** folder, run hello following command toobegin listening for hello reboot direct method.</span></span>

    ```cmd/sh
    node simDevice.js
    ```

1. <span data-ttu-id="e04aa-168">Kör hello C#-konsolapp **ScheduleJob** genom att högerklicka på hello **ScheduleJob** projekt, sedan välja **felsöka** och **Starta ny instans**.</span><span class="sxs-lookup"><span data-stu-id="e04aa-168">Run hello C# console app **ScheduleJob** by right-clicking on hello **ScheduleJob** project, then selecting **Debug** and **Start new instance**.</span></span>

1. <span data-ttu-id="e04aa-169">Du kan se hello utdata från både enheten och backend-appar.</span><span class="sxs-lookup"><span data-stu-id="e04aa-169">You see hello output from both device and back-end apps.</span></span>

    ![Köra hello appar tooschedule jobb][img-schedulejobs]

## <a name="next-steps"></a><span data-ttu-id="e04aa-171">Nästa steg</span><span class="sxs-lookup"><span data-stu-id="e04aa-171">Next steps</span></span>

<span data-ttu-id="e04aa-172">I den här kursen används ett jobb tooschedule en direkta metoden tooa enhet och hello uppdatering av hello enheten dubbla egenskaper.</span><span class="sxs-lookup"><span data-stu-id="e04aa-172">In this tutorial, you used a job tooschedule a direct method tooa device and hello update of hello device twin's properties.</span></span>

<span data-ttu-id="e04aa-173">komma igång med IoT-hubb och device management-mönster, till exempel remote över hello luften firmware-uppdatering, läsa toocontinue [Självstudier: hur toodo en inbyggd programvara uppdatera][lnk-fwupdate].</span><span class="sxs-lookup"><span data-stu-id="e04aa-173">toocontinue getting started with IoT Hub and device management patterns such as remote over hello air firmware update, read [Tutorial: How toodo a firmware update][lnk-fwupdate].</span></span>

<span data-ttu-id="e04aa-174">komma igång med IoT-hubb toocontinue finns [komma igång med IoT kant][lnk-iot-edge].</span><span class="sxs-lookup"><span data-stu-id="e04aa-174">toocontinue getting started with IoT Hub, see [Getting started with IoT Edge][lnk-iot-edge].</span></span>

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
